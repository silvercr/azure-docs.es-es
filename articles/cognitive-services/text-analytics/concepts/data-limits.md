---
title: Límites de datos de la API Text Analytics
titleSuffix: Azure Cognitive Services
description: Limitaciones de datos de la API Text Analytics en Azure Cognitive Services.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: overview
ms.date: 04/27/2020
ms.author: aahi
ms.reviewer: chtufts
ms.openlocfilehash: 79a12505ccc7cea709a88818513ba95710989954
ms.sourcegitcommit: f0b206a6c6d51af096a4dc6887553d3de908abf3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2020
ms.locfileid: "84142374"
---
# <a name="data-and-rate-limits-for-the-text-analytics-api"></a>Límites de datos y velocidad de la API Text Analytics
<a name="data-limits"></a>

Use este artículo para encontrar los límites de tamaño y las velocidades a las que se pueden enviar datos a la API Text Analytics. 

## <a name="data-limits"></a>Límites de datos

> [!NOTE]
> * Si necesita analizar documentos con un tamaño superior al límite, puede dividir el texto en fragmentos más pequeños antes de enviarlos a la API. 
> * Un documento es una sola cadena de caracteres de texto.  

| Límite | Value |
|------------------------|---------------|
| Tamaño máximo de un documento individual | 5120 caracteres medidos por [StringInfo.LengthInTextElements](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo.lengthintextelements). |
| Tamaño máximo de la solicitud completa | 1 MB |

El número máximo de documentos que puede enviar en una única solicitud dependerá de la versión de la API y de la característica que use.

#### <a name="version-3"></a>[Versión 3](#tab/version-3)

> [!NOTE]
> Si la solicitud de API de v3 supera estos límites, pero está dentro de los límites de v2, se devolverá una advertencia en la respuesta de la API. A partir del 15/07/2020, se devolverá un código de error 400. 

Los límites siguientes han cambiado en la versión 3 de la API. Si se superan los límites siguientes, se generará una advertencia en la respuesta de la API.


| Característica | Número máximo de documentos por solicitud | 
|----------|-----------|
| Detección de idiomas | 1000 |
| Análisis de sentimiento | 10 |
| Extracción de frases clave | 10 |
| Reconocimiento de entidades con nombre | 5 |
| Entity Linking | 5 |

#### <a name="version-2"></a>[Versión 2](#tab/version-2)

| Característica | Número máximo de documentos por solicitud | 
|----------|-----------|
| Detección de idiomas | 1000 |
| Análisis de sentimiento | 1000 |
| Extracción de frases clave | 1000 |
| Reconocimiento de entidades con nombre | 1000 |
| Entity Linking | 1000 |

---

## <a name="rate-limits"></a>Límites de frecuencia

El límite de velocidad variará en función del [plan de tarifa](https://azure.microsoft.com/pricing/details/cognitive-services/text-analytics/). Estos límites son los mismos en ambas versiones de la API.

| Nivel          | Solicitudes por segundo | Solicitudes por minuto |
|---------------|---------------------|---------------------|
| S / Varios servicios | 1000                | 1000                |
| S0 / F0         | 100                 | 300                 |
| S1            | 200                 | 300                 |
| S2            | 300                 | 300                 |
| S3            | 500                 | 500                 |
| S4            | 1000                | 1000                |

Las solicitudes se miden por separado para cada característica de Text Analytics. Por ejemplo, puede enviar el número máximo de solicitudes correspondiente al plan de tarifa a cada característica, al mismo tiempo.  


## <a name="see-also"></a>Consulte también

* [¿Qué es la API Text Analytics?](../overview.md)
* [Detalles de precios](https://azure.microsoft.com/pricing/details/cognitive-services/text-analytics/)
