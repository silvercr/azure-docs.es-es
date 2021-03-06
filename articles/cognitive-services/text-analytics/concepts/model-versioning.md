---
title: Especificación de la versión de modelo en Text Analytics v3
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo especificar el modelo de Text Analytics API que se usa en los datos.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 04/21/2020
ms.author: aahi
ms.openlocfilehash: c9c598d385768e604d9b42e3ea055629e6ae2866
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86108556"
---
# <a name="model-versioning-in-the-text-analytics-api"></a>Control de versiones de modelos en Text Analytics API

La versión 3 de Text Analytics API le permite elegir la versión del modelo que se usará en los datos. Use el parámetro opcional `model-version` para seleccionar la versión del modelo en sus solicitudes de API. Por ejemplo: `<resource-url>/text/analytics/v3.0/sentiment?model-version=2020-04-01`. Si no se especifica este parámetro, la API usará de forma predeterminada la versión estable más reciente. 

## <a name="available-versions"></a>Versiones disponibles

Use la tabla siguiente para averiguar qué versiones del modelo son compatibles con cada punto de conexión.


| Punto de conexión                        | Versiones admitidas                       | Versión más reciente |
|---------------------------------|------------------------------------------|----------------|
| `/sentiment`                    | `2019-10-01`, `2020-04-01`               | `2020-04-01`   |
| `/languages`                    | `2019-10-01`                             | `2019-10-01`   |
| `/entities/linking`             | `2019-10-01`, `2020-02-01`               | `2020-02-01`   |
| `/entities/recognition/general` | `2019-10-01`, `2020-02-01`, `2020-04-01` | `2020-04-01`   |
| `/entities/recognition/pii`     | `2019-10-01`, `2020-02-01`, `2020-04-01` | `2020-04-01`   |
| `/keyphrases`                   | `2019-10-01`                             | `2019-10-01`   |


Encontrará más detalles sobre las actualizaciones de estos modelos en [Novedades](../whats-new.md).

## <a name="text-analytics-for-health"></a>Text Analytics for Health

El contenedor de [Text Analytics for Health](../how-tos/text-analytics-for-health.md) usa versiones de modelo independientes de los puntos de conexión de API anteriores.  Tenga en cuenta que solo hay una versión de modelo disponible por cada imagen de contenedor.

| Punto de conexión                        | Etiqueta de la imagen de contenedor                     | Versión del modelo |
|---------------------------------|-----------------------------------------|---------------|
| `/domains/health`               | `1.1.012640001-amd64-preview` o más reciente | `2020-05-08`  |
| `/domains/health`               | `1.1.012420001-amd64-preview`           | `2020-05-08`  |
| `/domains/health`               | `1.1.012070001-amd64-preview`           | `2020-04-16`  |




## <a name="next-steps"></a>Pasos siguientes

* [Información general de Text Analytics](../overview.md)
* [Análisis de opiniones](../how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Reconocimiento de entidades](../how-tos/text-analytics-how-to-entity-linking.md)
