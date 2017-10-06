---
title: "aaaAzure ukázkový skript prostředí PowerShell - vytvoření spravovaného disku ze souboru VHD v účtu úložiště v předplatném stejný nebo jiný | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – vytvoření spravovaného disku ze souboru VHD v účtu úložiště ve stejné nebo jiné předplatné"
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
ms.openlocfilehash: 93157823eb3b8cddba5e0af455d16bff1d42ce00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-same-or-different-subscription-with-powershell"></a>Vytvoření spravovaného disku ze souboru VHD v účtu úložiště ve stejné nebo jiné předplatné pomocí prostředí PowerShell

Tento skript vytvoří se spravovaným diskem z soubor virtuálního pevného disku v účtu úložiště ve stejné nebo jiné předplatné. Použijte tento skript tooimport specializované (není zobecněný/Sysprep) virtuálního pevného disku toomanaged operačního systému disku toocreate virtuálního počítače. Ji použijte i tooimport data virtuálního pevného disku toomanaged datový disk. 

Nevytvářejte více identické spravovaných disků ze souboru virtuálního pevného disku v malé množství času. toocreate spravovat disky ze souboru virtuálního pevného disku, vytvoření snímku objektu blob souboru vhd hello a pak je použité toocreate spravované disky. Můžete vytvořit pouze jeden objekt blob snímek za minutu, která způsobuje chyby při vytváření disku kvůli toothrottling. tooavoid toto omezení, vytvořit [spravované snímku ze souboru virtuálního pevného disku hello](./../scripts/storage-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) a potom použijte hello spravovat snímku toocreate více spravovaných disků v krátkém čase. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/storage/create-managed-disks-from-vhd-in-different-subscription/create-managed-disks-from-vhd-in-different-subscription.ps1 "Create managed disk from VHD")]


## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá tyto příkazy toocreate spravovaného disku z disku VHD v jiném předplatném. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Nové AzureRmDiskConfig](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | Vytvoří konfigurace disku, který slouží k vytváření disků. Obsahuje typ úložiště, umístění, prostředků Id hello účtu úložiště uloží hello nadřazený virtuální pevný disk, identifikátor URI virtuálního pevného disku hello nadřazený virtuální pevný disk. |
| [Nové AzureRmDisk](/powershell/module/azurerm.compute/New-AzureRmDisk) | Vytvoří disk pomocí konfigurace disku, název disku a předány jako parametry název skupiny prostředků. |

## <a name="next-steps"></a>Další kroky

[Vytvoření virtuálního počítače připojením se spravovaným diskem jako disk operačního systému](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Ukázky skriptu PowerShell další virtuální počítač nachází v hello [virtuálního počítače Windows Azure dokumentaci](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
