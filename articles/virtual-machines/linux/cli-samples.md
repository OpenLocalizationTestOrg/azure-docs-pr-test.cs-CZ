---
title: "aaaAzure ukázky rozhraní příkazového řádku | Microsoft Docs"
description: "Ukázky rozhraní příkazového řádku Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/08/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 776c947e6daca564242fc77b0527dcb124fa057d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cli-samples-for-linux-virtual-machines"></a>Ukázek Azure CLI pro virtuální počítače s Linuxem

Hello následující tabulka obsahuje odkazy toobash skripty vyvíjené hello rozhraní příkazového řádku Azure.

| | |
|---|---|
|**Vytváření virtuálních počítačů**||
| [Vytvoření virtuálního počítače](./../scripts/virtual-machines-linux-cli-sample-create-vm-quick-create.md?toc=%2fcli%2fazure%2ftoc.json) | Vytvoří virtuální počítač s Linuxem s minimální konfigurací. |
| [Vytvoření kompletně nakonfigurovaný virtuálního počítače](./../scripts/virtual-machines-linux-cli-sample-create-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Vytvoří skupinu prostředků, virtuální počítač a všechny související prostředky.|
| [Vytvoření vysoce dostupné virtuální počítače](./../scripts/virtual-machines-linux-cli-sample-nlb.md?toc=%2fcli%2fazure%2ftoc.json) | Vytvoří několik virtuálních počítačů v s vysokou dostupností a konfigurace skupinu s vyrovnáváním zatížení. |
| [Vytvoření virtuálního počítače s Docker povoleno](./../scripts/virtual-machines-linux-cli-sample-create-docker-host.md?toc=%2fcli%2fazure%2ftoc.json) | Vytvoří virtuální počítač, nakonfiguruje tento virtuální počítač jako hostitele Docker a spouští kontejner NGINX. |
| [Vytvořte virtuální počítač a spusťte skript konfigurace](./../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md?toc=%2fcli%2fazure%2ftoc.json) | Vytvoří virtuální počítač a používá hello Azure vlastní skript rozšíření tooinstall NGINX. |
| [Vytvoření virtuálního počítače s WordPress nainstalován](./../scripts/virtual-machines-linux-cli-sample-create-vm-wordpress.md?toc=%2fcli%2fazure%2ftoc.json) | Vytvoří virtuální počítač a používá hello Azure vlastní skript rozšíření tooinstall WordPress. |
| [Vytvoření virtuálního počítače ze spravovaných disk operačního systému](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json) | Vytvoří virtuální počítač pomocí stávající spravované disk jako disk s operačním systémem. |
| [Vytvoření virtuálního počítače ze snímku](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) | Vytvoří virtuální počítač ze snímku nejprve vytvořením se spravovaným diskem ze snímku a potom připojení hello nový spravované disk jako disk s operačním systémem. |
|**Správa úložiště**||
| [Vytvoření spravovaného disku z virtuálního pevného disku](../scripts/virtual-machines-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json) | Vytvoří se spravovaným diskem z specializované virtuální pevný disk jako disk operačního systému nebo data virtuálního pevného disku jako datový disk.  |
| [Vytvoření spravovaného disku ze snímku](../scripts/virtual-machines-linux-cli-sample-create-managed-disk-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) | Vytvoří se spravovaným diskem ze snímku. |
| [Zkopírujte spravovaných disků na toosame nebo jiném předplatném.](../scripts/virtual-machines-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Kopie spravované disku toosame nebo jiného předplatného, ale v hello stejné oblasti jako nadřazené hello spravované disku. 
| [Export snímku jako účet úložiště tooa virtuálního pevného disku](../scripts/virtual-machines-linux-cli-sample-copy-snapshot-to-storage-account.md?toc=%2fcli%2fmodule%2ftoc.json) | Exporty spravované snímek jako účet úložiště tooa virtuálního pevného disku v jiné oblasti. |
| [Kopie snímků toosame nebo jiném předplatném.](../scripts/virtual-machines-linux-cli-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Kopie snímků toosame nebo jiné předplatné ale v hello stejné oblasti jako hello nadřazené snímku. |
|**Síť virtuálních počítačů**||
| [Zabezpečení síťového provozu mezi virtuálními počítači](./../scripts/virtual-machines-linux-cli-sample-create-vm-nsg.md?toc=%2fcli%2fazure%2ftoc.json) | Vytvoří dva virtuální počítače, všechny související prostředky a skupiny zabezpečení k interní a externí sítě (NSG). |
|**Zabezpečený virtuální počítače**||
| [Šifrování virtuálních počítačů a datovými disky](./../scripts/virtual-machines-linux-cli-sample-encrypt-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Vytvoří Azure Key Vault, šifrovací klíč a instanční objekt a potom šifruje virtuálního počítače. |
|**Monitorování virtuálních počítačů**||
| [Monitorování virtuálních počítačů pomocí služby Operations Management Suite](./../scripts/virtual-machines-linux-cli-sample-create-vm-oms.md?toc=%2fcli%2fazure%2ftoc.json) | Vytvoří virtuální počítač, nainstaluje agenta Operations Management Suite hello a zaregistruje hello virtuálních počítačů v pracovním prostoru OMS.  |
|**Řešení potíží s virtuální počítače**||
| [Řešení potíží s diskem operačního systému virtuálních počítačů](./../scripts/virtual-machines-linux-cli-sample-mount-os-disk.md?toc=%2fcli%2fazure%2ftoc.json) | Jako datový disk pro druhý virtuální počítač připojí hello disk operačního systému z jednoho virtuálního počítače. |
| | |
