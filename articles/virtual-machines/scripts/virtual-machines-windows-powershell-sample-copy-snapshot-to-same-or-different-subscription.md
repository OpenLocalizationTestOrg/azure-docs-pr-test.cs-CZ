---
title: "aaaAzure ukázka skriptu prostředí PowerShell - kopírování (přesunout) snímek toosame spravovaného disku nebo jiného předplatného | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - kopírování (přesunout) snímek toosame spravovaného disku nebo jiného předplatného"
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
ms.openlocfilehash: d7b8a71cc09d1950271f472e89b95bb551323be5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="copy-snapshot-of-a-managed-disk-in-same-subscription-or-different-subscription-with-powershell"></a>Kopírování snímku spravovaného disku v rámci stejného předplatného nebo jiného předplatného pomocí prostředí PowerShell

Tento skript vytvoří kopii snímku v hello stejné předplatné stejné nebo jiné předplatné. Pro uchovávání dat použijte tento skript toomove předplatné toodifferent snímku. Ukládání snímků v jiném předplatném. můžete chránit před nechtěným odstraněním snímků ve vašem předplatném hlavní. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.ps1 "Copy snapshot")]


## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy toocreate snímku hello cílové předplatné pomocí hello Id hello zdroj snímku. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Nové AzureRmSnapshotConfig](/powershell/module/azurerm.compute/New-AzureRmSnapshotConfig) | Vytvoří snímek konfigurace, který se používá pro vytvoření snímku. Obsahuje Id hello nadřazené snímku a umístění, které je stejné jako snímek nadřazené hello hello prostředku.  |
| [Nové AzureRmSnapshot](/powershell/module/azurerm.compute/New-AzureRmDisk) | Vytvoří snímek pomocí snímek konfigurace, název snímku a předány jako parametry název skupiny prostředků. |


## <a name="next-steps"></a>Další kroky

[Vytvoření virtuálního počítače ze snímku](./virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Ukázky skriptu PowerShell další virtuální počítač nachází v hello [virtuálního počítače Windows Azure dokumentaci](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
