---
title: "Azure skript prostředí PowerShell ukázkový – vytvoření spravovaného disku ze snímku | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – vytvoření spravovaného disku ze snímku"
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
ms.openlocfilehash: 9105d9dc06eea33b3a4e1eeea7fd793919166c9b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-managed-disk-from-a-snapshot-with-powershell"></a>Vytvoření spravovaného disku ze snímku pomocí prostředí PowerShell

Tento skript vytvoří se spravovaným diskem ze snímku. Ji použijte k obnovení virtuálního počítače z snímky operačního systému a dat disků. Vytvořte operačního systému a data spravovaná disky z příslušného snímky a pak vytvořte nový virtuální počítač připojením spravované disky. Můžete také obnovit datové disky stávajícího virtuálního počítače připojením datových disků, které jsou vytvořené z snímky.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[hlavní](../../../powershell_scripts/storage/create-managed-disk-from-snapshot/create-managed-disk-from-snapshot.ps1 "vytvořit spravovaných disků ze snímku")]


## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření spravovaného disku ze snímku. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Get-AzureRmSnapshot](/powershell/module/azurerm.compute/Get-AzureRmSnapshot) | Získá vlastnosti snímku.  |
| [Nové AzureRmDiskConfig](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | Vytvoří konfigurace disku, který slouží k vytváření disků. Obsahuje Id nadřazené snímku, umístění, které je stejné jako umístění nadřazeného snímku a typ úložiště prostředku.  |
| [Nové AzureRmDisk](/powershell/module/azurerm.compute/New-AzureRmDisk) | Vytvoří disk pomocí konfigurace disku, název disku a předány jako parametry název skupiny prostředků. |


## <a name="next-steps"></a>Další kroky

[Vytvoření virtuálního počítače z spravovaného disku](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Ukázky skriptu PowerShell další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).