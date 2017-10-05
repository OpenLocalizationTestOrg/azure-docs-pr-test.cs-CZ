---
title: "Ukázka skriptu Azure CLI - kopírování (přesunout) spravovaných disků na stejný nebo jiný předplatné | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - kopírování (přesunout) spravovaných disků na stejný nebo jiný odběr"
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
ms.openlocfilehash: 784ad81db2c83da14665fa926425928f12a15c27
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="copy-managed-disks-to-same-or-different-subscription-with-cli"></a>Zkopírujte spravovaných disků na stejný nebo jiný odběr pomocí rozhraní příkazového řádku

Tento skript zkopíruje spravovaných disků na stejný nebo jiný odběr, ale ve stejné oblasti. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli[hlavní](../../../cli_scripts/storage/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.sh "kopie spravované disku")]


## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření nového spravovaného disku v cílové předplatné pomocí Id zdrojového spravovaného disku. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Zobrazit az disku](https://docs.microsoft.com/cli/azure/disk#show) | Získá všechny vlastnosti spravovaného disku pomocí názvu a vlastnosti skupiny prostředků spravovaného disku. Vlastnost ID se používá ke zkopírování spravovaných disků do jiného předplatného.  |
| [Vytvoření az disku](https://docs.microsoft.com/cli/azure/disk#create) | Kopie spravovaného disku tak, že vytvoříte novou spravované disk v jiném předplatném. pomocí Id a název spravovaného nadřazený disk.  |

## <a name="next-steps"></a>Další kroky

[Vytvoření virtuálního počítače z spravovaného disku](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další virtuální počítač a spravované disky ukázky skriptu rozhraní příkazového řádku najdete v [virtuální počítač Azure s Linuxem dokumentaci](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
