---
title: "aaaAzure ukázky virtuálního počítače PowerShell | Microsoft Docs"
description: "Virtuální počítač Azure PowerShell ukázky"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/05/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 89fd26aa979687cdcdf9ae4e60be7d0d2eaeae91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machine-powershell-samples"></a>Ukázek Azure PowerShell virtuálního počítače

Hello následující tabulka obsahuje odkazy tooPowerShell skripty ukázky s vytvářet a spravovat virtuální počítače s Windows.

| | |
|---|---|
|**Vytváření virtuálních počítačů**||
| [Vytvoření kompletně nakonfigurovaný virtuálního počítače](./../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Vytvoří skupinu prostředků, virtuální počítač a všechny související prostředky.|
| [Vytvoření vysoce dostupné virtuální počítače](./../scripts/virtual-machines-windows-powershell-sample-create-nlb-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Vytvoří několik virtuálních počítačů v s vysokou dostupností a konfigurace skupinu s vyrovnáváním zatížení.|
| [Vytvořte virtuální počítač a spusťte skript konfigurace](./../scripts/virtual-machines-windows-powershell-sample-create-vm-iis.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Vytvoří virtuální počítač a používá hello Azure vlastní skript rozšíření tooinstall služby IIS. |
| [Vytvořte virtuální počítač a spusťte konfigurace DSC](./../scripts/virtual-machines-windows-powershell-sample-create-iis-using-dsc.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Vytvoří virtuální počítač a používá tooinstall rozšíření Azure požadovaného stavu konfigurace (DSC) hello služby IIS. |
| [Nahrání virtuálního pevného disku a vytvořit virtuální počítače](./../scripts/virtual-machines-windows-powershell-upload-generalized-script.md) | Uplaods místního virtuálního pevného disku soubor tooAzure, vytvoří a obrázek z hello virtuální pevný disk a potom vytvoří virtuální počítač z této bitové kopie. |
| [Vytvoření virtuálního počítače ze spravovaných disk operačního systému](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Vytvoří virtuální počítač pomocí stávající spravované disk jako disk s operačním systémem. |
| [Vytvoření virtuálního počítače ze snímku](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Vytvoří virtuální počítač ze snímku nejprve vytvořením se spravovaným diskem ze snímku a potom připojení hello nový spravované disk jako disk s operačním systémem. |
|**Správa úložiště**||
| [Vytvoření spravovaného disku z virtuálního pevného disku v rámci stejného nebo jiného předplatného](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Vytvoří se spravovaným diskem z specializované virtuální pevný disk jako disk operačního systému nebo data virtuálního pevného disku jako datový disk ve stejné nebo jiné předplatné.  |
| [Vytvoření spravovaného disku ze snímku](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Vytvoří se spravovaným diskem ze snímku. |
| [Zkopírujte spravovaných disků na toosame nebo jiném předplatném.](../scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Kopie spravované disku toosame nebo jiného předplatného, ale v hello stejné oblasti jako nadřazené hello spravované disku. 
| [Export snímku jako účet úložiště tooa virtuálního pevného disku](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-storage-account.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Exporty spravované snímek jako účet úložiště tooa virtuálního pevného disku v jiné oblasti. |
| [Vytvoření snímku z disku VHD](../scripts/virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Vytvoří snímek z virtuálního pevného disku toocreate více identické spravovaných disků ze snímku v krátkém čase.  |
| [Kopie snímků toosame nebo jiném předplatném.](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Kopie snímků toosame nebo jiné předplatné ale v hello stejné oblasti jako hello nadřazené snímku. |
|**Zabezpečený virtuální počítače**||
| [Šifrování virtuálních počítačů a datovými disky](./../scripts/virtual-machines-windows-powershell-sample-encrypt-vm.md?toc=%2fpowershell%2fazure%2ftoc.json) | Vytvoří Azure Key Vault, šifrovací klíč a instanční objekt a potom šifruje virtuálního počítače. |
|**Monitorování virtuálních počítačů**||
| [Monitorování virtuálních počítačů pomocí služby Operations Management Suite](./../scripts/virtual-machines-windows-powershell-sample-create-vm-oms.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Vytvoří virtuální počítač, nainstaluje agenta Operations Management Suite hello a zaregistruje hello virtuálních počítačů v pracovním prostoru OMS.  |
| | |

