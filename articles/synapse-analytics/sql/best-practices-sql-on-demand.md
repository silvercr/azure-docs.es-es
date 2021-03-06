---
title: Procedimientos recomendados para SQL a petición (versión preliminar)
description: Recomendaciones y procedimientos recomendados que debe conocer cuando trabaja con SQL a petición (versión preliminar).
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 05/01/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 7bebfeba6da1493557d51777ba8438747e160750
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85476281"
---
# <a name="best-practices-for-sql-on-demand-preview-in-azure-synapse-analytics"></a>Procedimientos recomendados de SQL a petición (versión preliminar) en Azure Synapse Analytics

En este artículo, encontrará una colección de procedimientos recomendados para usar SQL a petición (versión preliminar). SQL a petición es un recurso de Azure Synapse Analytics.

## <a name="general-considerations"></a>Consideraciones generales

SQL a petición permite consultar archivos de las cuentas de almacenamiento de Azure. No tiene funcionalidades de ingesta o almacenamiento local. Por ello, todos los archivos de destino de la consulta son externos a SQL a petición. Todo lo relacionado con la lectura de archivos desde el almacenamiento puede afectar el rendimiento de las consultas.

## <a name="colocate-your-azure-storage-account-and-sql-on-demand"></a>Colocación de una cuenta de Azure Storage y SQL a petición

Para minimizar la latencia, coloque su cuenta de Azure Storage y el punto de conexión de SQL a petición. Las cuentas de almacenamiento y los puntos de conexión aprovisionados durante la creación del área de trabajo se encuentran en la misma región.

Para obtener un rendimiento óptimo, si accede a otras cuentas de almacenamiento con SQL a petición, asegúrese de que se encuentren en la misma región. Si no están en la misma región, aumentará la latencia de la transferencia de red de los datos entre la región remota y la del punto de conexión.

## <a name="azure-storage-throttling"></a>Limitaciones de Azure Storage

Varias aplicaciones y servicios pueden acceder a la cuenta de almacenamiento. Las limitaciones de Storage se producen cuando las IOPS o el rendimiento combinados que generan las aplicaciones, los servicios y la carga de trabajo a petición de SQL superan los límites de la cuenta de almacenamiento. Como resultado, se producirá un efecto negativo significativo en el rendimiento de las consultas.

Cuando se detecta la limitación, SQL a petición tiene controles integrados para resolverla. SQL a petición realizará solicitudes al almacenamiento a un ritmo más lento hasta que la limitación se resuelva.

> [!TIP]
> Para que la ejecución de las consultas sea óptima, no fuerce la cuenta de almacenamiento con otras cargas de trabajo durante la ejecución de consultas.

## <a name="prepare-files-for-querying"></a>Preparación de los archivos para la consulta

Si es posible, puede preparar los archivos para mejorar el rendimiento:

- Convierta CSV y JSON a Parquet. Parquet es un formato de columnas. Dado que está comprimido, el tamaño de sus archivos es menor que el de los archivos CSV y JSON que contienen los mismos datos. SQL a petición necesitará menos tiempo y solicitudes de almacenamiento para leerlos.
- Si una consulta tiene como destino un solo archivo de gran tamaño, se beneficiará de dividirlo en varios archivos más pequeños.
- Intente mantener el tamaño del archivo CSV por debajo de los 10 GB.
- Es mejor tener archivos de igual tamaño para una sola ruta de acceso OPENROWSET o una ubicación de tabla externa.
- Para dividir los datos, almacene las particiones en diferentes carpetas o nombres de archivo. Consulte [Uso de las funciones filename y filepath para seleccionar particiones de destino específicas](#use-filename-and-filepath-functions-to-target-specific-partitions).

## <a name="push-wildcards-to-lower-levels-in-the-path"></a>Inserción de caracteres comodín en los niveles inferiores de la ruta de acceso

Puede usar caracteres comodín en la ruta de acceso para [consultar varios archivos y carpetas](query-data-storage.md#query-multiple-files-or-folders). SQL a petición muestra los archivos de la cuenta de almacenamiento, empezando por el primer carácter comodín (*) con la API de almacenamiento. Elimina los archivos que no coinciden con la ruta de acceso especificada. Al reducir la lista inicial de archivos, puede mejorar el rendimiento si hay muchos que coincidan con la ruta de acceso especificada hasta el primer carácter comodín.

## <a name="use-appropriate-data-types"></a>Uso del tipo de datos adecuado

Los tipos de datos que se usan en la consulta afectan al rendimiento. Puede obtener un mejor rendimiento si sigue estas instrucciones: 

- Usa el tamaño de datos más pequeño que se adapte al mayor valor posible.
  - Si la longitud máxima de caracteres es de 30, utilice un tipo de datos de caracteres de esa longitud.
  - Si todos los valores de columnas de caracteres tienen un tamaño fijo, use **char** o **nchar**. De lo contrario, use **varchar** o **nvarchar**.
  - Si el valor máximo de la columna de enteros es 500, use **smallint**, ya que es el menor tipo de datos que puede contener este valor. Puede encontrar intervalos de tipos de datos enteros en [este artículo](https://docs.microsoft.com/sql/t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql?view=sql-server-ver15).
- Si es posible, use **varchar** y **char** en lugar de **nvarchar** y **nchar**.
- Utilice tipos de datos basados en enteros si es posible. Las operaciones SORT, JOIN y GROUP BY se realizan más rápidamente en números enteros que en datos de caracteres.
- Si usa la inferencia de esquemas, [compruebe los tipos de datos inferidos](#check-inferred-data-types).

## <a name="check-inferred-data-types"></a>Comprobación de los tipos de datos inferidos

La [inferencia de esquemas](query-parquet-files.md#automatic-schema-inference) ayuda a escribir consultas rápidamente y a explorar los datos sin conocer los esquemas de archivo. La contrapartida de esta comodidad es que los tipos de datos inferidos son mayores que los tipos de datos reales. Esto sucede cuando no hay suficiente información en los archivos de código fuente para asegurarse de que se utiliza el tipo de datos adecuado. Por ejemplo, los archivos Parquet no contienen metadatos sobre la longitud máxima de la columna de caracteres. Por lo tanto, SQL a petición la deduce como varchar(8000).

Puede usar [sp_describe_first_results_set](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql?view=sql-server-ver15) para comprobar los tipos de datos resultantes de la consulta.

En el ejemplo siguiente se muestra cómo se pueden optimizar los tipos de datos inferidos. Este procedimiento se usa para mostrar tipos de datos inferidos: 
```sql  
EXEC sp_describe_first_result_set N'
    SELECT
        vendor_id, pickup_datetime, passenger_count
    FROM 
        OPENROWSET(
            BULK ''https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/*/*/*'',
            FORMAT=''PARQUET''
        ) AS nyc';
```

Este es el conjunto de resultados:

|is_hidden|column_ordinal|name|system_type_name|max_length|
|----------------|---------------------|----------|--------------------|-------------------||
|0|1|vendor_id|varchar(8000)|8000|
|0|2|pickup_datetime|datetime2(7)|8|
|0|3|passenger_count|int|4|

Una vez que conozca los tipos de datos inferidos para la consulta, puede especificar los tipos de datos adecuados:

```sql  
SELECT
    vendor_id, pickup_datetime, passenger_count
FROM 
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/*/*/*',
        FORMAT='PARQUET'
    ) 
    WITH (
        vendor_id varchar(4), -- we used length of 4 instead of the inferred 8000
        pickup_datetime datetime2,
        passenger_count int
    ) AS nyc;
```

## <a name="use-filename-and-filepath-functions-to-target-specific-partitions"></a>Uso de las funciones fileinfo y filepath para seleccionar particiones de destino específicas

A menudo, los datos se organizan en particiones. Puede indicar a SQL a petición que consulte archivos y carpetas concretos. De este modo se reducirá el número de archivos y la cantidad de datos que la consulta tiene que leer y procesar. Como ventaja adicional, logrará un mejor rendimiento.

Para más información, lea acerca de las funciones [filename](query-data-storage.md#filename-function) y [filepath](query-data-storage.md#filepath-function), y consulte los ejemplos para [consultar archivos específicos](query-specific-files.md).

> [!TIP]
> Convierta siempre los resultados de las funciones filepath y filename a los tipos de datos adecuados. Si usa tipos de datos de caracteres, asegúrese de que se usa la longitud apropiada.

> [!NOTE]
> Las funciones empleadas para la eliminación de particiones, filepath y filename, no se admiten actualmente para tablas externas que no sean las creadas automáticamente para cada tabla creada en Apache Spark para Azure Synapse Analytics.

Si los datos almacenados no tienen particiones, considere la posibilidad de crearlas. De este modo, puede usar estas funciones para optimizar las consultas destinadas a esos archivos. Cuando [consulte tablas con particiones de Apache Spark para Azure Synapse](develop-storage-files-spark-tables.md) desde SQL a petición, la consulta se destinará automáticamente solo a los archivos necesarios.

## <a name="use-parser_version-20-to-query-csv-files"></a>Uso de PARSER_VERSION 2.0 para consultar archivos CSV

Puede usar el analizador optimizado para rendimiento al consultar archivos CSV. Para más información, consulte [PARSER_VERSION](develop-openrowset.md).

## <a name="use-cetas-to-enhance-query-performance-and-joins"></a>Uso de CETAS para mejorar el rendimiento de las consultas y las combinaciones

[CETAS](develop-tables-cetas.md) es una de las características más importantes que están disponibles en SQL a petición. CETAS es una operación en paralelo que crea metadatos de tabla externa y exporta los resultados de la consulta SELECT a un conjunto de archivos de la cuenta de almacenamiento.

Puede usar CETAS para almacenar en un nuevo conjunto de archivos las partes más frecuentemente usadas de las consultas, como las tablas de referencia combinadas. A continuación, puede realizar combinaciones con esta tabla externa única, en lugar de repetir combinaciones comunes en varias consultas.

Cuando CETAS genera archivos Parquet, las estadísticas se crean automáticamente cuando la primera consulta selecciona como destino a esta tabla externa, lo que mejora el rendimiento.

## <a name="azure-ad-pass-through-performance"></a>Rendimiento de paso a través de Azure AD

SQL a petición permite acceder a archivos en el almacenamiento mediante el uso de las credenciales de paso a través de Azure Active Directory (Azure AD) o SAS. Con el paso a través de Azure AD, podría experimentar un rendimiento más lento que con SAS.

Si necesita un mejor rendimiento, pruebe a usar las credenciales de SAS para acceder al almacenamiento hasta que mejore el rendimiento de paso a través de Azure AD.

## <a name="next-steps"></a>Pasos siguientes

Consulte en el artículo [Solución de problemas](../sql-data-warehouse/sql-data-warehouse-troubleshoot.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) soluciones a problemas comunes. Si va a trabajar con grupos de SQL en lugar de con SQL a petición, consulte el artículo [Procedimientos recomendados para grupos de SQL](best-practices-sql-pool.md) para obtener instrucciones específicas.
