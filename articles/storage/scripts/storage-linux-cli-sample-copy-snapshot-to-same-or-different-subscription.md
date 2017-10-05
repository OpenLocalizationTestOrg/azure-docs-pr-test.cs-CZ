---
title: "Ukázka skriptu Azure CLI - kopírování (přesunout) snímek spravovaných disků na stejný nebo jiný odběr pomocí rozhraní příkazového řádku | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - kopírování (přesunout) snímek spravovaných disků na stejný nebo jiný odběr pomocí rozhraní příkazového řádku"
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
ms.openlocfilehash: 0127e342bd0c3afbe9de775399f5510814bff499
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="copy-snapshot-of-a-managed-disk-to-same-or-different-subscription-with-cli"></a>Kopírování snímku spravovaných disků na stejný nebo jiný odběr pomocí rozhraní příkazového řádku

Tento skript zkopíruje snímek spravovaných disků na stejný nebo jiný odběr. Pomocí tohoto skriptu snímek přesunout do jiného předplatného ve stejné oblasti jako nadřazené snímku.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli[hlavní](../../../cli_scripts/storage/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.sh "kopírování snímku")]


## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy pro vytvoření snímku v cílové předplatné pomocí Id zdrojové snímku. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Zobrazit az snímku](https://docs.microsoft.com/cli/azure/snapshot#show) | Získá všechny vlastnosti snímku pomocí názvu a vlastnosti skupiny prostředků snímku. Vlastnost ID se používá ke kopírování snímku do jiného předplatného.  |
| [Vytvoření snímku az](https://docs.microsoft.com/cli/azure/snapshot#create) | Zkopíruje snímek vytvořením snímku v jiné předplatné pomocí Id a název nadřazené snímku.  |

## <a name="next-steps"></a>Další kroky

[Vytvoření virtuálního počítače ze snímku](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další virtuální počítač a spravované disky ukázky skriptu rozhraní příkazového řádku najdete v [virtuální počítač Azure s Linuxem dokumentaci](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
