---
title: "Azure skript prostředí PowerShell ukázkový – vytvoření snímku z disku VHD vytvořit více stejné spravované disky malé množství času | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – vytvoření snímku z disku VHD vytvořit více stejné spravované disky malé množství času"
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
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: 02a69abd6c17ce765996379309e22afad82c4e10
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-snapshot-from-a-vhd-to-create-multiple-identical-managed-disks-in-small-amount-of-time-with-powershell"></a>Vytvoření snímku z disku VHD vytvořit více stejné spravované disky malé množství času v prostředí PowerShell

Tento skript vytvoří snímek ze souboru virtuálního pevného disku v účtu úložiště ve stejné nebo jiné předplatné. Pomocí tohoto skriptu k importu specializované (není zobecněný/Sysprep) virtuálního pevného disku na snímku a pak použít snímku vytvořit více identické discích spravovaných malé množství času. Navíc můžete importovat data virtuálního pevného disku na snímek a potom pomocí snímku můžete vytvořit více spravovaných disků malé množství času. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[hlavní](../../../powershell_scripts/virtual-machine/create-snapshots-from-vhd-in-different-subscription/create-snapshots-from-vhd-in-different-subscription.ps1 "vytvořit snímek z virtuálního pevného disku")]


## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření spravovaného disku z disku VHD v jiném předplatném. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Nové AzureRmDiskConfig](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | Vytvoří konfigurace disku, který slouží k vytváření disků. Obsahuje typ úložiště, umístění, prostředek Id účtu úložiště, kde je uložený nadřazený virtuální pevný disk a URI virtuálního pevného disku nadřazeného virtuálního pevného disku. |
| [Nové AzureRmDisk](/powershell/module/azurerm.compute/New-AzureRmDisk) | Vytvoří disk pomocí konfigurace disku, název disku a předány jako parametry název skupiny prostředků. |

## <a name="next-steps"></a>Další kroky

[Vytvoření spravovaného disku ze snímku](virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)


[Vytvoření virtuálního počítače připojením se spravovaným diskem jako disk operačního systému](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Ukázky skriptu PowerShell další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).