---
title: 'Tipo de asignación de escritorio personal de Windows Virtual Desktop: Azure'
description: Procedimiento para configurar el tipo de asignación para un grupo de host de escritorio personal de Windows Virtual Desktop.
services: virtual-desktop
author: HeidiLohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 05/22/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 6f4ee0a6ba028a6359a83f5130aca7c6f05ba08b
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86259793"
---
# <a name="configure-the-personal-desktop-host-pool-assignment-type"></a>Configuración del tipo de asignación de grupo de host de escritorio personal

>[!IMPORTANT]
>Este contenido se aplica a la versión Fall 2019 que no admite objetos de Windows Virtual Desktop para Azure Resource Manager. Si está tratando de administrar objetos de Windows Virtual Desktop para Azure Resource Manager introducidos en la actualización de Spring 2020, consulte [este artículo](../configure-host-pool-personal-desktop-assignment-type.md).

Puede configurar el tipo de asignación del grupo de host de escritorio personal para ajustar el entorno de Windows Virtual Desktop para que se adapte mejor a sus necesidades. En este tema, se mostrará cómo configurar la asignación automática o directa para los usuarios.

>[!NOTE]
> Las instrucciones de este artículo solo se aplican a los grupos de host de escritorio personal, no a los grupos de host agrupados, ya que los usuarios de grupos de host agrupados no se asignan a hosts de sesión específicos.

## <a name="configure-automatic-assignment"></a>Configuración de la asignación automática

La asignación automática es el tipo de asignación predeterminado para los nuevos grupos de host de escritorio personal creados en el entorno de Windows Virtual Desktop. La asignación automática de usuarios no requiere un host de sesión específico.

Para asignar usuarios de forma automática, asígnelos primero al grupo de host de escritorio personal para que puedan ver el escritorio en su fuente. Cuando un usuario asignado inicia el escritorio en su fuente, notificará un host de sesión disponible si todavía no se ha conectado al grupo de host, que completa el proceso de asignación.

Antes de empezar, [descargue e importe el módulo de PowerShell para Windows Virtual Desktop](/powershell/windows-virtual-desktop/overview/), si todavía no lo ha hecho.

> [!NOTE]
> Asegúrese de haber instalado el módulo de PowerShell para Windows Virtual Desktop versión 1.0.1534.2001 o posterior antes de seguir estas instrucciones.

Después, ejecute el siguiente cmdlet para iniciar sesión en su cuenta:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Para configurar un grupo de host a fin de asignar automáticamente usuarios a las máquinas virtuales, ejecute el siguiente cmdlet de PowerShell:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -AssignmentType Automatic
```

Para asignar un usuario al grupo de host de escritorio personal, ejecute el siguiente cmdlet de PowerShell:

```powershell
Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
```

## <a name="configure-direct-assignment"></a>Configuración de la asignación directa

A diferencia de la asignación automática, cuando se usa la asignación directa, debe asignar el usuario al grupo de host de escritorio personal y a un host de sesión específico antes de que se pueda conectar a su escritorio personal. Si el usuario solo se asigna a un grupo de host sin una asignación de host de sesión, no podrá acceder a los recursos.

Para configurar un grupo de host para que requiera la asignación directa de usuarios a los hosts de sesión, ejecute el siguiente cmdlet de PowerShell:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -AssignmentType Direct
```

Para asignar un usuario al grupo de host de escritorio personal, ejecute el siguiente cmdlet de PowerShell:

```powershell
Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
```

Para asignar un usuario a un host de sesión específico, ejecute el siguiente cmdlet de PowerShell:

```powershell
Set-RdsSessionHost <tenantname> <hostpoolname> -Name <sessionhostname> -AssignedUser <userupn>
```

## <a name="remove-a-user-assignment"></a>Eliminación de una asignación de usuario

Es posible que quiera quitar una asignación de usuario porque el usuario ya no necesite el escritorio personal, haya dejado la empresa, o bien porque quiera volver a usar el escritorio para otro usuario.

En la actualidad, la única manera de quitar la asignación de usuario para un escritorio personal consiste en eliminar completamente el host de sesión. Para quitar el host de sesión, ejecute este cmdlet:

```powershell
Remove-RdsSessionHost
```

Si necesita volver a agregar el host de sesión al grupo host de escritorio personal, desinstale Windows Virtual Desktop en ese equipo y, después, siga los pasos descritos en [Creación de un grupo de hosts con PowerShell](create-host-pools-powershell-2019.md) para volver a registrar el host de sesión.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha configurado el tipo de asignación de escritorio personal, puede iniciar sesión en un cliente de Windows Virtual Desktop para probarlo como parte de una sesión de usuario. En las dos guías paso a paso siguientes se indica cómo conectarse a una sesión mediante el cliente que elija:

- [Conexión con el cliente de Escritorio de Windows](connect-windows-7-10-2019.md)
- [Conexión con el cliente web](connect-web-2019.md)
