---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvoření spravovaného disku ze snímku | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření spravovaného disku ze snímku"
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.openlocfilehash: f92059353bfb739cff05213a9d206bfde2829a98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-disk-from-a-snapshot-with-cli"></a>Vytvoření spravovaného disku ze snímku s rozhraní příkazového řádku

Tento skript vytvoří se spravovaným diskem ze snímku. Použijte toorestore pro virtuální počítač z snímky operačního systému a dat disků. Vytvořte operačního systému a data spravovaná disky z příslušného snímky a pak vytvořte nový virtuální počítač připojením spravované disky. Můžete také obnovit datové disky stávajícího virtuálního počítače připojením datových disků, které jsou vytvořené z snímky.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli[main](../../../cli_scripts/storage/create-managed-disks-from-snapshot/create-managed-disks-from-snapshot.sh "Create managed disk from snapshot")]


## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá tyto příkazy toocreate se spravovaným diskem ze snímku. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Zobrazit az snímku](https://docs.microsoft.com/cli/azure/snapshot#show) | Získá všechny vlastnosti hello snímku pomocí názvu hello a vlastnosti skupiny prostředků hello snímku. Vlastnost ID je použité toocreate spravovaného disku.  |
| [Vytvoření az disku](https://docs.microsoft.com/cli/azure/disk#create) | Vytvoří spravované pomocí disku snímku Id spravované snímku |

## <a name="next-steps"></a>Další kroky

[Vytvoření virtuálního počítače připojením se spravovaným diskem jako disk operačního systému](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další virtuální počítač a spravované disky ukázky skriptu rozhraní příkazového řádku najdete v hello [virtuální počítač Azure s Linuxem dokumentaci](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
