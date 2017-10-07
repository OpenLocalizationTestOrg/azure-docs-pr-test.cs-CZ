---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - kopírování (přesunout) snímek toosame spravovaného disku nebo jiného předplatného pomocí rozhraní příkazového řádku | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - kopírování (přesunout) snímek toosame spravovaného disku nebo jiného předplatného pomocí rozhraní příkazového řádku"
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
ms.openlocfilehash: 4a21fd2435181a033b563100888aba0c5834496d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="copy-snapshot-of-a-managed-disk-toosame-or-different-subscription-with-cli"></a>Kopírování snímku toosame spravovaného disku nebo jiného předplatného pomocí rozhraní příkazového řádku

Tento skript zkopíruje snímek toosame spravovaného disku nebo jiného předplatného. Použijte tento skript toomove předplatné toodifferent snímku v hello stejné oblasti jako hello nadřazené snímku.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli[main](../../../cli_scripts/storage/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.sh "Copy snapshot")]


## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy toocreate snímku hello cílové předplatné pomocí hello Id hello zdroj snímku. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Zobrazit az snímku](https://docs.microsoft.com/cli/azure/snapshot#show) | Získá všechny vlastnosti hello snímku pomocí názvu hello a vlastnosti skupiny prostředků hello snímku. Vlastnost ID je použité toocopy hello snímku toodifferent předplatné.  |
| [Vytvoření snímku az](https://docs.microsoft.com/cli/azure/snapshot#create) | Kopie snímků vytvořením snímek pomocí jiného předplatného hello Id a název hello nadřazené snímku.  |

## <a name="next-steps"></a>Další kroky

[Vytvoření virtuálního počítače ze snímku](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další virtuální počítač a spravované disky ukázky skriptu rozhraní příkazového řádku najdete v hello [virtuální počítač Azure s Linuxem dokumentaci](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
