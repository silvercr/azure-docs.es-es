---
title: Novedades de Azure Load Balancer
description: Conozca las novedades de Azure Load Balancer, como, por ejemplo, las notas de la versión más recientes, los problemas conocidos, las correcciones de errores, las funcionalidades en desuso y los próximos cambios.
services: load-balancer
author: anavinahar
ms.service: load-balancer
ms.topic: overview
ms.date: 07/07/2020
ms.author: anavin
ms.openlocfilehash: 8b44dc230dbee1b29b9889a1b81e35ebe25f6b97
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "87078695"
---
# <a name="whats-new-in-azure-load-balancer"></a>Novedades de Azure Load Balancer

Azure Load Balancer se actualiza con regularidad. Manténgase al día con los anuncios más recientes. En este artículo se proporciona información acerca de lo siguiente:

- Versiones más recientes
- Problemas conocidos
- Corrección de errores
- Funcionalidad en desuso (si procede)

También puede encontrar las últimas actualizaciones de Azure Load Balancer y suscribirse a la fuente RSS [aquí](https://azure.microsoft.com/updates/?category=networking&query=load%20balancer).

## <a name="recent-releases"></a>Versiones recientes

| Tipo |Nombre |Descripción  |Fecha de adición  |
| ------ |---------|---------|---------|
| Característica | Compatibilidad con la administración de grupos de back-end basada en IP (versión preliminar) | Azure Load Balancer permite agregar y quitar recursos de un grupo de back-end a través de direcciones IPv4 o IPv6. Esto permite la administración sencilla de contenedores, máquinas virtuales y conjuntos de escalado de máquinas virtuales asociados a Load Balancer. También permitirá reservar direcciones IP como parte de un grupo de back-end antes de que se creen los recursos asociados. Obtenga más información [aquí](backend-pool-management.md).|Julio de 2020 |
| Característica| Información sobre Azure Load Balancer con Azure Monitor | Integrados como parte de Azure Monitor para redes, ahora los clientes cuentan con mapas topológicos de todas sus configuraciones de Load Balancer y paneles de mantenimiento de sus instancias de Standard Load Balancer preconfiguradas con métricas en Azure Portal. [Introducción y más información](https://azure.microsoft.com/blog/introducing-azure-load-balancer-insights-using-azure-monitor-for-networks/) | Junio de 2020 |
| Validación | Incorporación de la validación para puertos de alta disponibilidad | Se ha agregado una validación para garantizar que las reglas de puerto de alta disponibilidad y las reglas que no son de este tipo solo se puedan configurar cuando se habilite la IP flotante. Anteriormente, esta configuración se procesaba, pero no funcionaba según lo previsto. No se realizó ningún cambio en la funcionalidad. Puede obtener más información [aquí](load-balancer-ha-ports-overview.md#limitations)| Junio de 2020 |
| Característica| Compatibilidad de IPv6 con Azure Load Balancer (disponible con carácter general) | Puede tener direcciones IPv6 como front-end para las instancias de Azure Load Balancer. Aprenda a [crear una aplicación de pila doble aquí](../virtual-network/virtual-network-ipv4-ipv6-dual-stack-standard-load-balancer-powershell.md). |Abril de 2020|
| Característica| Restablecimientos TCP en tiempo de espera de inactividad (disponible con carácter general)| Use los restablecimientos TCP para crear un comportamiento más predecible de las aplicaciones. [Más información](load-balancer-tcp-reset.md)| Febrero de 2020 |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure Load Balancer, consulte [¿Qué e Azure Load Balancer?](load-balancer-overview.md) y las [preguntas frecuentes](load-balancer-faqs.md).
