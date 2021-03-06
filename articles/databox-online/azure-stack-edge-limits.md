---
title: Límites de Azure Stack Edge | Microsoft Docs
description: Describe los límites del sistema y los tamaños recomendados de Azure Stack Edge.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/22/2019
ms.author: alkohli
ms.openlocfilehash: 4f7800efb5d4382e8d73c819d950fdfafd10f296
ms.sourcegitcommit: 856db17a4209927812bcbf30a66b14ee7c1ac777
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82568769"
---
# <a name="azure-stack-edge-limits"></a>Límites de Azure Stack Edge

Tenga en cuenta estos límites cuando implemente y use su solución de Microsoft Azure Stack Edge. 

## <a name="azure-stack-edge-service-limits"></a>Límites del servicio de Azure Stack Edge

[!INCLUDE [data-box-edge-gateway-service-limits](../../includes/data-box-edge-gateway-service-limits.md)]

## <a name="azure-stack-device-limits"></a>Límites de los dispositivos de Azure Stack

En la tabla siguiente, se describen los límites de los dispositivos de Azure Stack Edge. 

| Descripción | Value |
|---|---|
|No. de archivos por dispositivo |100 millones |
|No. de recursos compartidos por dispositivo |24 |
|No. de recursos compartidos por contenedor |1 |
|Tamaño máximo de archivo escrito en un recurso compartido| 5 TB |

## <a name="azure-storage-limits"></a>Límites de almacenamiento de Azure

[!INCLUDE [data-box-edge-gateway-storage-limits](../../includes/data-box-edge-gateway-storage-limits.md)]

## <a name="data-upload-caveats"></a>Advertencias al cargar los datos

[!INCLUDE [data-box-edge-gateway-storage-data-upload-caveats](../../includes/data-box-edge-gateway-storage-data-upload-caveats.md)]

## <a name="azure-storage-account-size-and-object-size-limits"></a>Límites de tamaño objeto y de tamaño de cuenta de almacenamiento en Azure

[!INCLUDE [data-box-edge-gateway-storage-acct-limits](../../includes/data-box-edge-gateway-storage-acct-limits.md)]


## <a name="azure-object-size-limits"></a>Límites de tamaño de objeto de Azure

[!INCLUDE [data-box-edge-gateway-storage-object-limits](../../includes/data-box-edge-gateway-storage-object-limits.md)]

## <a name="next-steps"></a>Pasos siguientes

- [Preparación de la implementación de Azure Stack Edge](azure-stack-edge-deploy-prep.md)
