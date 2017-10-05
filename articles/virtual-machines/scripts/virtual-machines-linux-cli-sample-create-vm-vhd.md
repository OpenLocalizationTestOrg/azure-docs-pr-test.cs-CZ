---
title: "Rozhraní příkazového řádku Azure Script ukázka – vytvoření virtuálního počítače s virtuální pevný disk | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření virtuálního počítače pomocí virtuálního pevného disku."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/09/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: fab65296a552c1839522c5254a868a3dc96227f7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-a-virtual-hard-disk"></a>Vytvoření virtuálního počítače s virtuálním pevným diskem

Tento příklad vytvoří virtuální počítač pomocí virtuálního pevného disku.
Vytvoří skupinu prostředků, účet úložiště a kontejner a potom vytvoří virtuální počítač odesláním disku VHD do kontejneru.
Nahradí ssh veřejný klíč pomocí veřejného klíče, máte přístup k virtuálnímu počítači.

Budete potřebovat spouštěcí virtuální pevný disk.
Můžete stáhnout z https://azclisamples.blob.core.windows.net/vhds/sample.vhd virtuální pevný disk, který jsme použili nebo použít vlastní virtuální pevný disk. Skript hledá `~/sample.vhd`.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-machine/create-vm-vhd/create-vm-vhd.sh "vytvoření virtuálního počítače pomocí virtuálního pevného disku")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.

```azurecli-interactive 
az group delete -n az-cli-vhd
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální počítač, skupina dostupnosti, nástroj pro vyrovnávání zatížení a všechny související prostředky. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [seznam účtů úložiště az](https://docs.microsoft.com/cli/azure/storage/account#list) | Účty úložiště seznamy |
| [AZ název účtu úložiště kontrola-](https://docs.microsoft.com/cli/azure/storage/account#check-name) | Ověří, zda je platný název účtu úložiště a že ještě neexistuje |
| [seznam klíčů účtu úložiště az](https://docs.microsoft.com/cli/azure/storage/account/keys#list) | Obsahuje seznam klíčů pro účty úložiště |
| [Objekt blob úložiště az existuje](https://docs.microsoft.com/cli/azure/storage/blob#exists) | Ověří, zda existuje objektu blob |
| [Vytvoření kontejneru úložiště az](https://docs.microsoft.com/cli/azure/storage/container#create) | Vytvoří kontejner v účtu úložiště. |
| [Nahrání objektu blob úložiště az](https://docs.microsoft.com/cli/azure/storage/blob#upload) | Vytvoří objekt blob v kontejneru, tím, že nahrajete virtuální pevný disk. |
| [Seznam virtuálních počítačů az](https://docs.microsoft.com/cli/azure/vm#list) | Použít s `--query` zkontrolujte, zda název virtuálního počítače je používán. | 
| [Vytvoření virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | Vytvoří virtuální počítače. |
| [AZ virtuálních počítačů přístupu sada linux uživatele](https://docs.microsoft.com/cli/azure/vm/access#set-linux-user) | Obnoví klíč SSH poskytnout aktuální uživatel přístup k virtuálnímu počítači. |
| [virtuální počítač az seznam ip adres](https://docs.microsoft.com/cli/azure/vm#list-ip-addresses) | Získá IP adresu virtuálního počítače, který byl vytvořen. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
