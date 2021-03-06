---
title: 'Ejemplo de script de la CLI de Azure: Inicio de una máquina virtual en un laboratorio | Microsoft Docs'
description: Este script de la CLI de Azure inicia una máquina virtual en un laboratorio de Azure DevTest Labs.
services: lab-services
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/12/2018
ms.author: spelluru
ms.custom: mvc
ms.openlocfilehash: a38e0eacdba2c37a18c41f307e4a8120e0e6285d
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/28/2020
ms.locfileid: "87290343"
---
# <a name="use-azure-cli-to-start-a-virtual-machine-in-a-lab-in-azure-devtest-labs"></a>Use la CLI de Azure para iniciar una máquina virtual en un laboratorio de Azure DevTest Labs.

Este script de la CLI de Azure inicia una máquina virtual en un laboratorio. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../cli_scripts/devtest-lab/start-connect-virtual-machine-in-lab/start-connect-virtual-machine-in-lab.sh "Start a VM")]


## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos:

| Get-Help | Notas |
|---|---|
| [az lab vm start](/cli/azure/lab/vm?view=azure-cli-latest#az-lab-vm-start) | Inicia una máquina virtual en un laboratorio. Esta operación puede tardar varios minutos en completarse. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](/cli/azure).

Encontrará más ejemplos de scripts de la CLI de Azure Lab Services en los [ejemplos de la CLI de Azure Lab Services](../samples-cli.md).
