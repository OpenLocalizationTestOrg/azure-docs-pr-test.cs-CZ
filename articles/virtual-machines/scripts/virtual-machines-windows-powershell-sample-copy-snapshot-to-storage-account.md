---
title: "aaaAzure ukázka skriptu prostředí PowerShell - Export nebo zkopírování snímku jako účet úložiště tooa virtuálního pevného disku v jiné oblasti. | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - Export nebo zkopírování snímku jako účet úložiště tooa virtuální pevný disk ve stejné oblasti jiné"
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
ms.openlocfilehash: c18ad4fa0bf12033fafe941a807e7b4c8d233a30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="exportcopy-managed-snapshots-as-vhd-tooa-storage-account-in-different-region-with-powershell"></a>Export nebo zkopírování spravované snímků jako účet úložiště tooa virtuálního pevného disku v jiné oblasti pomocí prostředí PowerShell

Tento skript exportuje účtu úložiště spravovaného snímku tooa v jiné oblasti. Nejprve generuje hello identifikátor URI pro SAS hello snímku a používá je toocopy ho tooa účet úložiště v jiné oblasti. Použijte tuto zálohu toomaintain skriptu spravované disky v jiné oblasti pro obnovení po havárii.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-snapshot-to-storage-account/copy-snapshot-to-storage-account.ps1 "Copy snapshot")]


## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy toogenerate tooa účet úložiště pomocí SAS URI pořízení snímku identifikátor URI pro SAS pro spravované hello snímku a kopií. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Udělení AzureRmSnapshotAccess](/powershell/module/azurerm.compute/New-AzureRmDisk) | Vygeneruje SAS URI pro snímek, který je použité toocopy ho tooa účet úložiště. |
| [Nové AzureStorageContext](/powershell/module/azure.storage/New-AzureStorageContext) | Vytvoří kontext účtu úložiště pomocí hello název účtu a klíč. Tento kontext může být použité tooperform operace čtení a zápisu v účtu úložiště hello. |
| [Počáteční AzureStorageBlobCopy](/powershell/module/azure.storage/Start-AzureStorageBlobCopy) | Kopie hello základní virtuální pevný disk účtu úložiště tooa snímku |

## <a name="next-steps"></a>Další kroky

[Vytvoření spravovaného disku z virtuálního pevného disku](virtual-machines-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json)

[Vytvoření virtuálního počítače z spravovaného disku](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Ukázky skriptu PowerShell další virtuální počítač nachází v hello [virtuálního počítače Windows Azure dokumentaci](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
