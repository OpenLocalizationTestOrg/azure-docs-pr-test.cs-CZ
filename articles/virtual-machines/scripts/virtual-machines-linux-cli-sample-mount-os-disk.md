---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - disk operačního systému připojit | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - připojení disku operačního systému"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 5c614d09a64780575b70424d29052f1a6affec59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-vms-operating-system-disk"></a>Řešení potíží s diskem operačního systému virtuálních počítačů

Tento skript připojí hello disk operačního systému virtuálního počítače se nezdařilo nebo problematické jako datový disk tooa druhý virtuální počítač. To může být užitečné při řešení potíží s disku problémy nebo obnovení dat. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/mount-os-disk/mount-os-disk.sh "Quick Create VM")]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy toocreate skupinu prostředků, virtuální počítač, a všechny související prostředky. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Zobrazit az virtuálních počítačů](https://docs.microsoft.com/cli/azure/vm#show) | Vrátí seznam virtuálních počítačů. V takovém případě je možnost dotazu hello disku operačního systému virtuálního počítače používané tooreturn hello. Tato hodnota se pak přidá tooa název proměnné 'uri'. |
| [Odstranění virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm#delete) | Odstraní virtuální počítač. |
| [Vytvoření virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm#create) | Vytvoří virtuální počítač.  |
| [připojit disk az virtuálních počítačů](https://docs.microsoft.com/cli/azure/vm/disk#attach) | Připojí disku tooa virtuálního počítače. |
| [virtuální počítač az seznam ip adres](https://docs.microsoft.com/cli/azure/vm#list-ip-addresses) | Vrátí hello IP adresy virtuálního počítače. |

## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
