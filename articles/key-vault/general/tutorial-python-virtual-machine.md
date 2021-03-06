---
title: 'Tutorial: Uso de Azure Key Vault con una máquina virtual en Python | Microsoft Docs'
description: En este tutorial, va a configurar una aplicación ASP.NET Core para que lea un secreto de su almacén de claves.
services: key-vault
author: msmbaldwin
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 07/20/2020
ms.author: mbaldwin
ms.custom: mvc, tracking-python
ms.openlocfilehash: 453307b304c4cb1899b1de31117c944ac66fcddb
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "87093934"
---
# <a name="tutorial-use-azure-key-vault-with-a-virtual-machine-in-python"></a>Tutorial: Uso de Azure Key Vault con una máquina virtual en Python

Azure Key Vault ayuda a proteger secretos como las claves de API, las cadenas de conexión de base de datos necesarias para acceder a las aplicaciones, los servicios y los recursos de TI.

En este tutorial, aprenderá cómo obtener una aplicación de consola para leer información de Azure Key Vault. Para ello, use identidades administradas para recursos de Azure. 

En este tutorial se muestra cómo realizar las siguientes acciones:

> [!div class="checklist"]
> * Cree un almacén de claves.
> * Agregue un secreto al almacén de claves.
> * Recuperar un secreto del almacén de claves.
> * Cree una máquina virtual de Azure.
> * Habilite una entidad administrada.
> * Asigne permisos a la identidad de máquina virtual.

Antes de empezar, lea los [conceptos básicos de Key Vault](basic-concepts.md). 

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Requisitos previos

Para Windows, Mac y Linux:
  * [Git](https://git-scm.com/downloads)
  * Este tutorial requiere que se ejecute localmente la CLI de Azure. Debe tener instalada la versión 2.0.4 de la CLI de Azure o una versión posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure 2.0](/cli/azure/install-azure-cli).

## <a name="log-in-to-azure"></a>Inicio de sesión en Azure

Para iniciar sesión en Azure mediante la CLI de Azure, escriba:

```azurecli
az login
```

### <a name="create-a-resource-group-and-key-vault"></a>Creación de un grupo de recursos y de un almacén de claves

[!INCLUDE [Create a resource group and key vault](../../../includes/key-vault-rg-kv-creation.md)]

## <a name="add-a-secret-to-the-key-vault"></a>Incorporación de un secreto al almacén de claves

Estamos agregando un secreto para ayudar a ilustrar cómo funciona. El secreto podría ser una cadena de conexión SQL o cualquier otra información que necesite mantener segura y disponible para la aplicación.

Para crear un secreto en el almacén de claves denominado **AppSecret**, escriba el siguiente comando:

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Este secreto almacena el valor **MySecret**.

## <a name="create-a-virtual-machine"></a>Creación de una máquina virtual
Puede crear una máquina virtual mediante uno de los métodos siguientes:

* [La CLI de Azure](../../virtual-machines/windows/quick-create-cli.md)
* [PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
* [Portal de Azure](../../virtual-machines/windows/quick-create-portal.md)

## <a name="assign-an-identity-to-the-vm"></a>Asignación de una identidad a la máquina virtual
En este paso, va a crear una identidad asignada por el sistema para la máquina virtual mediante la ejecución del siguiente comando en la CLI de Azure:

```azurecli
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

Tenga en cuenta la identidad asignada por el sistema que se muestra en el código siguiente. La salida del comando anterior sería: 

```output
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

## <a name="assign-permissions-to-the-vm-identity"></a>Asignación de permisos a la identidad de máquina virtual
Ahora puede asignar los permisos de la identidad creada anteriormente al almacén de claves mediante la ejecución del comando siguiente:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="log-on-to-the-virtual-machine"></a>Iniciar sesión en la nueva máquina virtual

Para iniciar sesión en la máquina virtual, siga las instrucciones de [Conexión a una máquina virtual de Azure donde se ejecuta Windows e inicio de sesión en ella](../../virtual-machines/windows/connect-logon.md).

## <a name="create-and-run-a-sample-python-app"></a>Creación y ejecución de una aplicación de Python de ejemplo

En la siguiente sección, se muestra un archivo de ejemplo denominado *Sample.py*. Usa una biblioteca [requests](https://2.python-requests.org/en/master/) para realizar llamadas HTTP GET.

## <a name="edit-samplepy"></a>Edición de Sample.py

Después de crear *Sample.py*, abra el archivo y, después, copie el código en esta sección. 

El código presenta un proceso de dos pasos:
1. Capturar un token del punto de conexión MSI local en la máquina virtual.  
  Si lo hace, también captura un token de Azure AD.
1. Pase el token al almacén de claves y, después, capture el secreto. 

```python
    # importing the requests library 
    import requests 

    # Step 1: Fetch an access token from a Managed Identity enabled azure resource.
    # Resources with an MSI configured recieve an AAD access token by using the Azure Instance Metadata Service (IMDS)
    # IMDS provides an endpoint accessible to all IaaS VMs using a non-routable well-known IP Address
    # To learn more about IMDS and MSI Authentication see the following link: https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service
    # Note that the resource here is https://vault.azure.net for public cloud and api-version is 2018-02-01
    MSI_ENDPOINT = "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net"
    r = requests.get(MSI_ENDPOINT, headers = {"Metadata" : "true"}) 
      
    # extracting data in json format 
    # This request gets an access_token from Azure AD by using the local MSI endpoint.
    data = r.json() 
    
    # Step 2: Pass the access_token received from previous HTTP GET call to your key vault.
    KeyVaultURL = "https://{YOUR KEY VAULT NAME}.vault.azure.net/secrets/{YOUR SECRET NAME}?api-version=2016-10-01"
    kvSecret = requests.get(url = KeyVaultURL, headers = {"Authorization": "Bearer " + data["access_token"]})
    
    print(kvSecret.json()["value"])
```

Puede mostrar el valor del secreto al ejecutar el código siguiente: 

```console
python Sample.py
```

El código anterior muestra cómo realizar operaciones con Azure Key Vault en una máquina virtual Windows. 

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no son necesarios, elimine la máquina virtual y el almacén de claves.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [API REST de Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/)
