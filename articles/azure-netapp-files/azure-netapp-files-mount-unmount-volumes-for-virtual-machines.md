---
title: Montaje de volúmenes de Azure NetApp Files para máquinas virtuales
description: Descubre cómo montar o desmontar un volumen para máquinas virtuales Windows o máquinas virtuales Linux en Azure.
author: b-juche
ms.author: b-juche
ms.service: azure-netapp-files
ms.workload: storage
ms.topic: how-to
ms.date: 07/06/2020
ms.openlocfilehash: 4bfd90be2a469c5ab94172769729095069f53cd7
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2020
ms.locfileid: "86045661"
---
# <a name="mount-or-unmount-a-volume-for-windows-or-linux-virtual-machines"></a>Montaje o desmontaje de un volumen para máquinas virtuales Linux 

Puede montar o desmontar un volumen para máquinas virtuales Windows o Linux según sea necesario.  Las instrucciones de montaje para las máquinas virtuales Linux están disponibles en Azure NetApp Files.  

> [!IMPORTANT] 
> Debe tener al menos una directiva de exportación para poder tener acceso a un volumen de NFS.

1. Haga clic en la hoja **Volúmenes** y seleccione el volumen para el que desea realizar el montaje. 
2. Haga clic en **Instrucciones de montaje** en el volumen seleccionado y siga las instrucciones para montar el volumen. 

    ![Instrucciones de montaje NFS](../media/azure-netapp-files/azure-netapp-files-mount-instructions-nfs.png)

    ![Instrucciones de montaje SMB](../media/azure-netapp-files/azure-netapp-files-mount-instructions-smb.png)  
    * Si monta un volumen NFS, asegúrese de usar la opción `vers` en el comando `mount` para especificar la versión de protocolo NFS correspondiente al volumen que desea montar. 
    * Si usa NFSv4.1, utilice el siguiente comando para montar el sistema de archivos: `sudo mount -t nfs -o rw,hard,rsize=65536,wsize=65536,vers=4.1,tcp,sec=sys $MOUNTTARGETIPADDRESS:/$VOLUMENAME $MOUNTPOINT`  

3. Si desea tener un volumen NFS montado automáticamente cuando se inicia o se reinicia una máquina virtual de Azure, agregue una entrada al archivo `/etc/fstab` del host. 

    Por ejemplo: `$ANFIP:/$FILEPATH        /$MOUNTPOINT    nfs bg,rw,hard,noatime,nolock,rsize=65536,wsize=65536,vers=3,tcp,_netdev 0 0`

    * `$ANFIP` es la dirección IP del volumen de Azure NetApp Files que se encuentra en la hoja de propiedades de volumen.
    * `$FILEPATH` es la ruta de acceso de exportación del volumen de Azure NetApp Files.
    * `$MOUNTPOINT` es el directorio creado en el host de Linux que se utiliza para montar la exportación de NFS.

4. Si desea montar el volumen en Windows con NFS:

    a. Monte primero el volumen en una máquina virtual Unix o Linux.  
    b. Ejecute el comando `chmod 777` o `chmod 775` en el volumen.  
    c. Monte el volumen con el cliente NFS en Windows.

## <a name="next-steps"></a>Pasos siguientes

* [Configuración del dominio predeterminado de NFS, versión 4.1, para Azure NetApp Files](azure-netapp-files-configure-nfsv41-domain.md)
* [Preguntas más frecuentes sobre NFS](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-faqs#nfs-faqs)
* [Introducción a Network File System](https://docs.microsoft.com/windows-server/storage/nfs/nfs-overview)
