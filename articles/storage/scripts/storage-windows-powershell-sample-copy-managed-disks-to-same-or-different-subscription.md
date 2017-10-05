---
title: "Ukázkový skript prostředí PowerShell Azure - kopírování (přesunout) spravovaných disků na stejný nebo jiný předplatné | Microsoft Docs"
description: "Ukázkový skript prostředí PowerShell Azure - kopírování (přesunout) spravovaných disků na stejný nebo jiný odběr"
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/06/2017
ms.author: ramankum
ms.openlocfilehash: 6fa94de0461cc538a60d57ca3518141afd9d0469
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="copy-managed-disks-in-the-same-subscription-or-different-subscription-with-powershell"></a>Zkopírujte spravované disky ve stejném předplatném nebo jiného předplatného pomocí prostředí PowerShell

Tento skript vytvoří kopii existující spravovaných disků ve stejném předplatném nebo jiné předplatné. Nový disk se vytvoří ve stejné oblasti jako nadřazený disk spravované.   

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[hlavní](../../../powershell_scripts/storage/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.ps1 "kopie spravované disku")]


## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření nového spravovaného disku v cílové předplatné pomocí Id zdrojového spravovaného disku. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Nové AzureRmDiskConfig](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | Vytvoří konfigurace disku, který slouží k vytváření disků. Obsahuje Id nadřazený disk a umístění, které je stejné jako umístění nadřazený disk prostředku.  |
| [Nové AzureRmDisk](/powershell/module/azurerm.compute/New-AzureRmDisk) | Vytvoří disk pomocí konfigurace disku, název disku a předány jako parametry název skupiny prostředků. |


## <a name="next-steps"></a>Další kroky

[Vytvoření virtuálního počítače z spravovaného disku](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Ukázky skriptu PowerShell další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).