---
title: Solución de problemas de evaluación y visualización de dependencias en Azure Migrate
description: Obtenga ayuda con la solución de problemas de evaluación y visualización de dependencias en Azure Migrate.
ms.service: azure-migrate
ms.topic: troubleshooting
author: musa-57
ms.manager: abhemraj
ms.author: hamusa
ms.date: 01/02/2020
ms.openlocfilehash: 61afc3ec0f37f5d8b1030818d21b7daabb7fce40
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86121680"
---
# <a name="troubleshoot-assessmentdependency-visualization"></a>Solución de problemas de evaluación o visualización de dependencias

Este artículo incluye ayuda para la solución de problemas de evaluación y visualización de dependencias con [Azure Migrate: Server Assessment](migrate-services-overview.md#azure-migrate-server-assessment-tool).


## <a name="assessment-readiness-issues"></a>Problemas de preparación de la evaluación

Revise los problemas de preparación de la evaluación como se indica a continuación:

**Problema** | **Revisión**
--- | ---
Tipo de arranque no compatible | Azure no admite máquinas virtuales con el tipo de arranque EFI. Se recomienda convertir el tipo de arranque a BIOS antes de ejecutar una migración. <br/><br/>Puede usar Azure Migrate Server Migration para controlar la migración de estas máquinas virtuales. Durante la migración, el tipo de arranque de la máquina virtual se convertirá a BIOS.
Sistemas operativos Windows condicionalmente compatibles | El sistema operativo alcanzó la fecha de finalización del soporte técnico y necesita un contrato de soporte técnico personalizado (CSA) para [recibir soporte técnico en Azure](https://aka.ms/WSosstatement). Considere la posibilidad de actualizar antes de migrar a Azure.
Sistemas operativos Windows no compatibles | Azure solo admite [versiones del sistema operativo Windows seleccionadas](https://aka.ms/WSosstatement). Considere la posibilidad de actualizar la máquina antes de migrar a Azure.
Sistema operativo Linux aprobado con condiciones | Azure aprueba solo [versiones del sistema operativo Linux seleccionadas](../virtual-machines/linux/endorsed-distros.md). Considere la posibilidad de actualizar la máquina antes de migrar a Azure.
Sistema operativo Linux no aprobado | La máquina puede iniciarse en Azure, pero Azure no proporciona compatibilidad con el sistema operativo. Considere la actualización a una [versión de Linux aprobada](../virtual-machines/linux/endorsed-distros.md) antes de migrar a Azure.
Sistema operativo desconocido | El sistema operativo de la máquina virtual se especificó como "Otros" en vCenter Server. Este comportamiento impide que Azure Migrate compruebe la preparación para Azure de la máquina virtual. Asegúrese de que Azure [admite](https://aka.ms/azureoslist) el sistema operativo antes de migrarla.
Versión de bits no admitida | Las máquinas virtuales con un sistema operativo de 32 bits pueden arrancar en Azure, pero se recomienda que actualice a 64 bits antes de migrar a Azure.
Requiere una suscripción de Microsoft Visual Studio | En la máquina se ejecuta un sistema operativo cliente Windows, que solo se admite mediante una suscripción de Visual Studio.
No se encontró ninguna máquina virtual para el rendimiento de almacenamiento requerido | El rendimiento del almacenamiento (operaciones de entrada/salida por segundo [IOPS] y rendimiento) necesario para la máquina supera la compatibilidad con máquinas virtuales de Azure. Reduzca los requisitos de almacenamiento de la máquina antes de realizar la migración.
No se encontró ninguna máquina virtual para el rendimiento de red requerido | El rendimiento de red (entrada/salida) requerido para la máquina excede el soporte técnico de máquina virtual de Azure. Reduzca los requisitos de red de la máquina.
No se encontró ninguna máquina virtual en la ubicación especificada | Utilice una ubicación de destino diferente antes de la migración.
Uno o varios discos no son adecuados | Uno o varios discos conectados a la máquina virtual no cumplen los requisitos de Azure.<br/><br/> Azure Migrate: Server Assessment no admite actualmente discos Ultra SSD y evalúa los discos en función de los límites de disco de los discos administrados Premium (32 TB).<br/><br/> Para cada disco conectado a la máquina virtual, asegúrese de que el tamaño del disco es menor que 64 TB (compatible con discos SSD Ultra).<br/><br/> Si no lo es, reduzca el tamaño de los discos antes de la migración o bien use varios discos en Azure y [colóquelos juntos](../virtual-machines/windows/premium-storage-performance.md#disk-striping) para obtener mayores límites de almacenamiento. Asegúrese de que el rendimiento (IOPS y rendimiento) necesario para cada disco sea compatible con los [discos de máquinas virtuales administradas](../azure-resource-manager/management/azure-subscription-service-limits.md#storage-limits) de Azure.
Uno o varios adaptadores de red no son adecuados | Quite los adaptadores de red no utilizados de la máquina antes de realizar la migración.
El recuento del disco supera el límite | Quite los discos no utilizados del equipo antes de la migración.
El tamaño del disco supera el límite | Azure Migrate: Server Assessment no admite actualmente discos Ultra SSD y evalúa los discos en función de los límites de los discos Premium (32 TB).<br/><br/> Sin embargo, Azure admite discos con un tamaño de hasta 64 TB (compatible con discos SSD Ultra). Reduzca los discos a menos de 64 TB antes de la migración o use varios discos en Azure y [colóquelos juntos](../virtual-machines/windows/premium-storage-performance.md#disk-striping) para obtener mayores límites de almacenamiento.
El disco no está disponible en la ubicación especificada | Asegúrese de que el disco está en la ubicación de destino antes de realizar la migración.
El disco no está disponible para la redundancia especificada | El disco debe usar el tipo de almacenamiento de redundancia definido en la configuración de evaluación (LRS de forma predeterminada).
No se pudo determinar la idoneidad del disco debido a un error interno | Intente crear una evaluación para el grupo.
No se encontró ninguna máquina virtual con los núcleos y memoria requeridos | Azure no pudo encontrar un tipo de máquina virtual adecuado. Reduzca la memoria y el número de núcleos de la máquina local antes de realizar la migración.
No se pudo determinar la idoneidad de la máquina virtual debido a un error interno | Intente crear una evaluación para el grupo.
No se pudo determinar la idoneidad de uno o varios discos debido a un error interno | Intente crear una evaluación para el grupo.
No se pudo determinar la idoneidad de uno o varios adaptadores de red debido a un error interno | Intente crear una evaluación para el grupo.
No se encontró ningún tamaño de máquina virtual para la instancia reservada de moneda de oferta. | Máquina marcada como no válida porque no se encontró el tamaño de la máquina virtual para la combinación de RI, oferta y moneda seleccionada. Edite las propiedades de valoración para elegir las combinaciones válidas y recalcular la valoración. 
Protocolo de Internet preparado condicionalmente | Solo se aplica a las valoraciones de Azure VMware Solution (AVS). AVS no es compatible con el factor de direcciones IPv6 de Internet. Póngase en contacto con el equipo de AVS para obtener instrucciones de corrección si la máquina se detecta con IPv6.

## <a name="suggested-migration-tool-in-import-based-avs-assessment-marked-as-unknown"></a>La herramienta de migración sugerida en la evaluación de AVS basada en la importación está marcada como desconocida.

En el caso de las máquinas importadas mediante un archivo CSV, se desconoce la herramienta de migración predeterminada en una valoración de AVS. Sin embargo, para las máquinas de VMware, se recomienda usar la solución Hybrid Cloud Extension (HCX) de VMWare. [Más información](../azure-vmware/hybrid-cloud-extension-installation.md).

## <a name="linux-vms-are-conditionally-ready-in-an-azure-vm-assessment"></a>Las máquinas virtuales Linux están "condicionalmente preparadas" en una valoración de máquinas virtuales de Azure.

En el caso de las máquinas virtuales de VMware e Hyper-V, Server Assessment marca las máquinas virtuales Linux como "condicionalmente preparadas" debido a una brecha conocida en Server Assessment. 

- La brecha evita que detecte la versión secundaria del sistema operativo Linux instalada en las máquinas virtuales locales.
- Por ejemplo, para RHEL 6.10, Server Assessment actualmente solo detecta RHEL 6 como la versión del sistema operativo. Esto se debe a que el vCenter Server y el host de Hyper-V no proporcionan la versión de kernel para los sistemas operativos de máquinas virtuales Linux.
-  Puesto que Azure solo aprueba versiones específicas de Linux, las máquinas virtuales Linux actualmente se marcan como listas con condiciones en Server Assessment.
- Puede determinar si el sistema operativo Linux en ejecución en la máquina virtual local está aprobado en Azure; para ello, consulte [Soporte técnico de Linux en Azure](https://aka.ms/migrate/selfhost/azureendorseddistros).
-  Una vez haya comprobado que la distribución está aprobada, puede omitir esta advertencia.

Este intervalo se puede solucionar habilitando [la detección de aplicaciones](./how-to-discover-applications.md) en las máquinas virtuales de VMware. Server Assessment usa el sistema operativo detectado de la máquina virtual con las credenciales de invitado proporcionadas. Estos datos del sistema operativo identifican la información del sistema operativo adecuada en el caso de las máquinas virtuales Windows y Linux.


## <a name="azure-skus-bigger-than-on-premises-in-an-azure-vm-assessment"></a>Las SKU de Azure son más grandes que las locales en una valoración de máquinas virtuales de Azure.

Azure Migrate Server Assessment podría recomendar las SKU de máquinas virtuales de Azure con más núcleos y memoria que la asignación local actual según el tipo de evaluación:


- La recomendación de las SKU de máquinas virtuales depende de las propiedades de la evaluación.
- Esto se ve afectado por el tipo de evaluación que realiza en Server Assessment: *Basado en el rendimiento* y *Como local*.
- Para obtener evaluaciones en función del rendimiento, Server Assessment tiene en cuenta los datos de uso de las máquinas virtuales en el entorno local (uso de CPU, memoria, disco y red) para determinar la SKU de máquina virtual de destino correcta para las máquinas virtuales en el entorno local. También agrega un factor de confort al determinar el uso efectivo.
- Para el ajuste de tamaño en la opción como en el entorno local, no se tienen en cuenta los datos de rendimiento y se recomienda una SKU de destino en función de la asignación local.

Para mostrar cómo esto puede afectar a las recomendaciones, veamos un ejemplo:

Tenemos una máquina virtual local con cuatro núcleos y 8 GB de memoria, con un uso de CPU del 50 % y un uso de memoria del 50 % y un factor de confort específico de 1,3.

-  Si la evaluación es **Como local**, se recomienda una SKU de VM de Azure con 4 núcleos y 8 GB de memoria.
- Si la evaluación está basada en el rendimiento, en función de la utilización de memoria y CPU efectiva (el 50 % de 4 núcleos * 1,3 = 2,6 núcleos y el 50 % de 8 GB de memoria * 1,3 = 5,3 GB de memoria), se recomienda la SKU de máquina virtual más barata de cuatro núcleos (número de núcleos admitidos más cercano) y 8 GB de memoria (tamaño de memoria admitido más cercano).
- [Más información](concepts-assessment-calculation.md#types-of-assessments) sobre el ajuste de tamaño de evaluación.

## <a name="azure-disk-skus-bigger-than-on-premises-in-an-azure-vm-assessment"></a>Las SKU de discos de Azure son más grandes que las locales en una valoración de máquinas virtuales de Azure.

Azure Migrate Server Assessment podría recomendar un disco más grande según el tipo de evaluación.
- El ajuste de tamaño del disco en Server Assessment depende de dos propiedades de la evaluación: el criterio de ajuste de tamaño y el tipo de almacenamiento.
- Si el criterio de ajuste de tamaño es **Basado en el rendimiento** y el tipo de almacenamiento es **Automático**, se tendrán en cuenta los valores de rendimiento y de IOPS del disco para identificar el tipo de disco de destino (discos HDD estándar, SSD estándar o Premium). Se recomienda una SKU de disco del tipo de disco y esta recomendación tiene en cuenta los requisitos de tamaño del disco en el entorno local.
- Si el criterio de ajuste de tamaño es **En función del rendimiento** y el tipo de almacenamiento es **Premium**, se recomienda una SKU de disco Premium de Azure en función de los requisitos de IOPS, rendimiento y tamaño del disco en el entorno local. Se usa la misma lógica para ajustar el tamaño del disco cuando el criterio de ajuste de tamaño es **Como local** y el tipo de almacenamiento es **HDD estándar**, **SSD estándar** o **Premium**.

Por ejemplo, si tiene un disco en el entorno local con 32 GB de memoria, pero las IOPS de lectura y escritura agregadas son 800 IOPS, Server Assessment recomienda un tipo de disco Premium (debido a los mayores requisitos de IOPS) y, a continuación, una SKU de disco que pueda admitir el tamaño y las IOPS necesarias. La coincidencia más cercana en este ejemplo sería P15 (256 GB, 1100 IOPS). Aunque el tamaño requerido por el disco en el entorno local fue de 32 GB, Server Assessment recomienda un disco con un tamaño mayor debido a los altos requisitos de IOPS del disco en el entorno local.

## <a name="utilized-corememory-percentage-missing"></a>Falta el porcentaje de núcleos/memoria usado

Los informes de Server Assessment "PercentageOfCoresUtilizedMissing" o "PercentageOfMemoryUtilizedMissing" cuando el dispositivo de Azure Migrate no puede recopilar datos de rendimiento de las máquinas virtuales locales pertinentes.

- Esto puede ocurrir si las máquinas virtuales están desactivadas durante la duración de la evaluación. El dispositivo no puede recopilar datos de rendimiento de una máquina virtual cuando está desactivada.
- Si solo faltan los contadores de memoria e intenta evaluar máquinas virtuales de Hyper-V, compruebe si tiene habilitada la memoria dinámica en estas máquinas virtuales. Existe un problema conocido solo para las máquinas virtuales de Hyper-V, en el que un dispositivo de Azure Migrate no puede recopilar los datos de uso de memoria de las máquinas virtuales que no tienen habilitada la memoria dinámica.
- Si falta alguno de los contadores de rendimiento, Azure Migrate Server Assessment recurre a la memoria y los núcleos asignados y recomienda un tamaño de máquina virtual correspondiente.
- Si faltan todos los contadores de rendimiento, asegúrese de que se cumplen los requisitos de acceso a puertos para la evaluación. Obtenga más información sobre los requisitos de acceso a puertos para la valoración de [VMware](./migrate-support-matrix-vmware.md#port-access-requirements), [Hyper-V](./migrate-support-matrix-hyper-v.md#port-access) y los servidores [físicos](./migrate-support-matrix-physical.md#port-access).

## <a name="is-the-operating-system-license-included-in-an-azure-vm-assessment"></a>¿Se incluye la licencia del sistema operativo en una valoración de máquinas virtuales de Azure?

Azure Migrate Server Assessment solo tiene en cuenta el costo de la licencia del sistema operativo para las máquinas Windows. Los costos de las licencias para las máquinas Linux no se tienen en cuenta actualmente.

## <a name="how-does-performance-based-sizing-work-in-an-azure-vm-assessment"></a>¿Cómo funciona el dimensionamiento basado en el rendimiento en una valoración de máquinas virtuales de Azure?

Server Assessment recopila continuamente datos de rendimiento de las máquinas en el entorno local y los usa para recomendar la SKU de máquina virtual y la SKU de disco de Azure. [Obtenga información](concepts-assessment-calculation.md#calculate-sizing-performance-based) sobre cómo se recopilan datos basados en el rendimiento.

## <a name="why-is-my-assessment-showing-a-warning-that-it-was-created-with-an-invalid-combination-of-reserved-instances-vm-uptime-and-discount-"></a>¿Por qué mi evaluación muestra una advertencia de que se creó con una combinación no válida de instancias reservadas, tiempo de actividad de la máquina virtual y descuento (%)?
Al seleccionar ' instancias reservadas ', el ' descuento (%) ' y las propiedades de "tiempo de actividad de la máquina virtual" no son aplicables. A medida que la evaluación se creó con una combinación no válida de estas propiedades, se deshabilitan los botones editar y volver a calcular. Cree una nueva valoración. [Más información](https://go.microsoft.com/fwlink/?linkid=2131554).

## <a name="i-do-not-see-performance-data-for-some-network-adapters-on-my-physical-servers"></a>No veo los datos de rendimiento de algunos adaptadores de red en los servidores físicos.

Esto puede ocurrir si el servidor físico tiene habilitada la virtualización de Hyper-V. En estos servidores, debido a una brecha en el producto, Azure Migrate detecta actualmente los adaptadores de red físicos y virtuales. El rendimiento de la red solo se captura en los adaptadores de red virtuales descubiertos.

## <a name="recommended-azure-vm-sku-for-my-physical-server-is-oversized"></a>La SKU de máquina virtual de Azure recomendada para el servidor físico tiene un gran tamaño.

Esto puede ocurrir si el servidor físico tiene habilitada la virtualización de Hyper-V. En estos servidores, Azure Migrate detecta actualmente los adaptadores de red físicos y virtuales. Por lo tanto, el número de adaptadores de red detectados es mayor que el valor real. Como la evaluación de servidores elige una máquina virtual de Azure que puede admitir el número requerido de adaptadores de red, esto puede dar lugar a una máquina virtual de gran tamaño. [Obtenga más información](./concepts-assessment-calculation.md#calculating-sizing) sobre el impacto del número de adaptadores de red para dimensionamiento. Se trata de una brecha del producto que se solucionará más adelante.

## <a name="readiness-category-not-ready-for-my-physical-server"></a>Categoría de preparación "No preparado" para mi servidor físico

La categoría de preparación puede estar marcada incorrectamente como "No preparado" en el caso de un servidor físico que tenga habilitada la virtualización de Hyper-V. En estos servidores, debido a una brecha en el producto, Azure Migrate detecta actualmente los adaptadores físicos y virtuales. Por lo tanto, el número de adaptadores de red detectados es mayor que el valor real. En las valoraciones tanto locales como basadas en el rendimiento, Server Assessment elige una máquina virtual de Azure que puede admitir el número requerido de adaptadores de red. Si se detecta que el número de adaptadores de red es mayor que 32, el número máximo de NIC admitidas en las máquinas virtuales de Azure, la máquina se marcará como "No preparada".  [Obtenga más información](./concepts-assessment-calculation.md#calculating-sizing) sobre el impacto del número de NIC en el dimensionamiento.


## <a name="number-of-discovered-nics-higher-than-actual-for-physical-servers"></a>El número de NIC detectado es mayor que el valor real para servidores físicos.

Esto puede ocurrir si el servidor físico tiene habilitada la virtualización de Hyper-V. Actualmente, Azure Migrate detecta los adaptadores físicos y virtuales en estos servidores. Por lo tanto, el número de NIC detectadas es mayor que el valor real.


## <a name="low-confidence-rating-on-physical-server-assessments"></a>Clasificación de confianza baja en las valoraciones del servidor físico.
Esta clasificación se asigna en función de la disponibilidad de los puntos de datos necesarios para calcular tal valoración. En el caso de los servidores físicos que tienen habilitada la virtualización de Hyper-V, existe una brecha en el producto debido a que la clasificación de confianza baja puede estar asignada incorrectamente a las valoraciones del servidor físico. Actualmente, Azure Migrate detecta los adaptadores físicos y virtuales en estos servidores. El rendimiento de la red se captura en los adaptadores de red virtuales descubiertos, pero no en los adaptadores de red físicos. Debido a la ausencia de puntos de datos en los adaptadores de red físicos, la clasificación de confianza puede verse afectada, lo que da lugar a una clasificación baja. Se trata de una brecha del producto que se solucionará más adelante.

## <a name="dependency-visualization-in-azure-government"></a>Visualización de dependencias en Azure Government

Azure Migrate depende de Service Map para la funcionalidad de visualización de dependencias. Como Service Map actualmente no está disponible en Azure Government, esta funcionalidad no está disponible en Azure Government.

## <a name="dependencies-dont-show-after-agent-install"></a>Las dependencias no se muestran después de la instalación del agente

Después de instalar los agentes de visualización de dependencias en las máquinas virtuales locales, Azure Migrate suele tardar entre 15 y 30 minutos en mostrar las dependencias en el portal. Si ha esperado durante más de 30 minutos, asegúrese de que Microsoft Monitoring Agent (MMA) puede conectarse al área de trabajo de Log Analytics.

Para las máquinas virtuales de Windows:
1. En el Panel de control, inicie MMA.
2. En **Propiedades de Microsoft Monitoring Agent** > **Azure Log Analytics (OMS)** , asegúrese de que el **estado** del área de trabajo esté en verde.
3. Si el estado no está en verde, intente quitar el área de trabajo y vuelva a agregarla a MMA.

    ![Estado de MMA](./media/troubleshoot-assessment/mma-properties.png)

Para las máquinas virtuales Linux, asegúrese de que se hayan completado correctamente los comandos de instalación de MMA y el agente de dependencias.

## <a name="supported-operating-systems"></a>Sistemas operativos admitidos

- **Agente de MMS**: Revise los sistemas operativos [Windows](../azure-monitor/platform/log-analytics-agent.md#supported-windows-operating-systems) y [Linux](../azure-monitor/platform/log-analytics-agent.md#supported-linux-operating-systems) admitidos.
- **Agente de dependencias**: sistemas operativos [Windows y Linux](../azure-monitor/insights/vminsights-enable-overview.md#supported-operating-systems) admitidos.

## <a name="visualize-dependencies-for--hour"></a>Visualización de dependencias de > hora

Con el análisis de dependencia sin agente, puede visualizar dependencias o exportarlas en un mapa para una duración de hasta 30 días.

Con el análisis de dependencias basado en agentes, aunque Azure Migrate le permita volver atrás a una fecha determinada en el último mes, el tiempo máximo durante el que se pueden visualizar las dependencias es de una hora. Por ejemplo, puede usar la característica de duración de tiempo en el mapa de dependencias para ver las dependencias de ayer, pero solo puede verlas durante un período de una hora. Sin embargo, puede usar los registros de Azure Monitor para [consultar los datos de dependencia](./how-to-create-group-machine-dependencies.md) durante un período más largo.

## <a name="visualized-dependencies-for--10-machines"></a>Visualización de dependencias de > 10 máquinas

En Azure Migrate Server Assessment, puede [visualizar las dependencias para grupos](./how-to-create-a-group.md#refine-a-group-with-dependency-mapping) que tengan un máximo de 10 máquinas virtuales con un análisis de dependencia basado en agentes. En el caso de los grupos más grandes, se recomienda dividir las máquinas virtuales en grupos más pequeños para visualizar las dependencias.


## <a name="machines-show-install-agent"></a>Las máquinas muestran "Instalar agente"

Después de migrar las máquinas con la visualización de dependencias habilitada en Azure, es posible que las máquinas muestren la acción "Instalar agente" en lugar de "Ver dependencias" debido al comportamiento siguiente:


- Después de la migración a Azure, las máquinas locales están desconectadas y las máquinas virtuales equivalentes trabajan en Azure. Estos equipos adquieren una dirección MAC diferente.
- Las máquinas también pueden adquirir una dirección IP distinta en función de si el se elige conservar o no la dirección IP local.
- Si las direcciones IP y MAC difieren de las locales, Azure Migrate no asocia las máquinas locales con ningún dato de dependencias de Service Map. En este caso, se mostrará la opción para instalar el agente en lugar de ver las dependencias.
- Después de una migración de prueba a Azure, las máquinas locales permanecen encendidas según lo previsto. Las máquinas equivalentes que se activan en Azure adquieren una dirección MAC distinta y pueden adquirir una dirección IP diferente. A menos que bloquee el tráfico saliente del registro de Azure Monitor procedente de estas máquinas, Azure Migrate no asociará las máquinas locales con ningún dato de dependencias de Service Map y, por tanto, mostrará la opción de instalar los agentes en lugar de visualizar las dependencias.

## <a name="dependencies-export-csv-shows-unknown-process"></a>Dependencias de exportación de CSV muestra "Proceso desconocido".
En el análisis de dependencias sin agente, los nombres de los procesos se capturan de la mejor manera posible. En ciertos escenarios, aunque los nombres de los servidores de origen y destino y el puerto de destino se capturen, no es factible determinar los nombres de los procesos en ambos extremos de la dependencia. En tales casos, el proceso se marca como "Proceso desconocido".


## <a name="capture-network-traffic"></a>Captura del tráfico de red

Recopile registros de tráfico de red como se indica a continuación:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Presione F12 para iniciar las Herramientas de desarrollo. Si es necesario, desactive la opción **Borrar entradas de navegación**.
3. Seleccione la pestaña **Red** para empezar a capturar el tráfico de red:
   - En Chrome, seleccione **Preserve log** (Conservar registro). La grabación debe iniciarse automáticamente. Un círculo rojo indica que la captura del tráfico está en curso. Si no aparece el círculo rojo, seleccione el círculo negro para comenzar.
   - En Microsoft Edge e Internet Explorer, la grabación debe iniciarse automáticamente. Si no es así, seleccione el botón de reproducción de color verde.
4. Pruebe a reproducir el error.
5. Una vez haya encontrado el error durante la grabación, detenga la grabación y guarde una copia de la actividad grabada:
   - En Chrome, haga clic con el botón derecho y seleccione **Guardar como HAR con contenido**. Esta acción comprime y exporta los registros como un archivo .har.
   - En Microsoft Edge o Internet Explorer, seleccione la opción **Exportar tráfico capturado**. Esta acción comprime y exporta el registro.
6. Seleccione la pestaña **Consola** para ver si hay alguna advertencia o error. Para guardar el registro de la consola:
   - En Chrome, haga clic con el botón derecho en cualquier lugar en el registro de la consola. Seleccione **Save as** (Guardar como) para exportar y comprimir el registro.
   - En Microsoft Edge o Internet Explorer, haga clic con el botón derecho sobre los errores y seleccione **Copiar todo**.
7. Cierre las Herramientas de desarrollo.


## <a name="where-is-the-operating-system-data-in-my-assessment-discovered-from"></a>¿Dónde se detectan los datos del sistema operativo en mi valoración?

- En el caso de las máquinas virtuales de VMware, de forma predeterminada, son los datos del sistema operativo que proporciona vCenter. 
   - En el caso de las máquinas virtuales Linux de VMware, si está habilitada la detección de aplicaciones, los detalles del sistema operativo se capturan desde la máquina virtual invitada. Para comprobar los detalles del sistema operativo de la valoración, vaya a la vista de Servidores detectados y ponga el mouse sobre el valor de la columna "Sistema operativo". En el texto que aparece, podrá ver si los datos del sistema operativo que ve se recopilan de vCenter Server o de la máquina virtual invitada con las credenciales de la máquina virtual. 
   - En el caso de las máquinas virtuales Windows, los detalles del sistema operativo siempre se capturan desde el vCenter Server.
- En el caso de las máquinas virtuales de Hyper-V, los datos del sistema operativo se recopilan desde el host de Hyper-V.
- En el caso de los servidores físicos, se capturan desde el servidor.

## <a name="next-steps"></a>Pasos siguientes

[Crear](how-to-create-assessment.md) o [personalizar](how-to-modify-assessment.md) una evaluación.
