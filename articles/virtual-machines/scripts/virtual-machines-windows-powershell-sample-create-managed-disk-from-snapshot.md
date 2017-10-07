---
title: "aaaAzure ukázkový skript prostředí PowerShell - vytvoření spravovaného disku ze snímku | Microsoft Docs"
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
ms.openlocfilehash: b99413b4c3e9a662a5fef74b7e4aeb5ecbb26af4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-disk-from-a-snapshot-with-powershell"></a>Vytvoření spravovaného disku ze snímku pomocí prostředí PowerShell

Tento skript vytvoří se spravovaným diskem ze snímku. Použijte toorestore pro virtuální počítač z snímky operačního systému a dat disků. Vytvořte operačního systému a data spravovaná disky z příslušného snímky a pak vytvořte nový virtuální počítač připojením spravované disky. Můžete také obnovit datové disky stávajícího virtuálního počítače připojením datových disků, které jsou vytvořené z snímky.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-managed-disk-from-snapshot/create-managed-disk-from-snapshot.ps1 "Create managed disk from snapshot")]


## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá tyto příkazy toocreate se spravovaným diskem ze snímku. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Get-AzureRmSnapshot](/powershell/module/azurerm.compute/Get-AzureRmSnapshot) | Získá vlastnosti snímku.  |
| [Nové AzureRmDiskConfig](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | Vytvoří konfigurace disku, který slouží k vytváření disků. Obsahuje Id hello nadřazené snímku, umístění, které je stejné jako umístění hello nadřazené snímku a hello úložiště typu prostředku hello.  |
| [Nové AzureRmDisk](/powershell/module/azurerm.compute/New-AzureRmDisk) | Vytvoří disk pomocí konfigurace disku, název disku a předány jako parametry název skupiny prostředků. |


## <a name="next-steps"></a>Další kroky

[Vytvoření virtuálního počítače z spravovaného disku](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Ukázky skriptu PowerShell další virtuální počítač nachází v hello [virtuálního počítače Windows Azure dokumentaci](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
