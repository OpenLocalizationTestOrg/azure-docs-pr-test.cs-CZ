---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvoření virtuálního počítače s virtuální pevný disk | Microsoft Docs"
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
ms.openlocfilehash: ce39092697a51e4e8a8e59ba8eb919955f616458
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-a-virtual-hard-disk"></a>Vytvoření virtuálního počítače s virtuálním pevným diskem

Tento příklad vytvoří virtuální počítač pomocí virtuálního pevného disku.
Vytvoří skupinu prostředků, účet úložiště a kontejner a potom vytvoří virtuálního počítače tím, že nahrajete hello virtuálního pevného disku toohello kontejneru.
Nahradí hello ssh veřejného klíče pomocí veřejného klíče, aby měli přístup toohello virtuálních počítačů.

Budete potřebovat spouštěcí virtuální pevný disk.
Můžete stáhnout z https://azclisamples.blob.core.windows.net/vhds/sample.vhd hello virtuálního pevného disku, který jsme použili nebo použít vlastní virtuální pevný disk. skript Hello hledá `~/sample.vhd`.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-vhd/create-vm-vhd.sh "Create VM using a VHD")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.

```azurecli-interactive 
az group delete -n az-cli-vhd
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy toocreate skupinu prostředků, virtuální počítač, skupinu dostupnosti, nástroj pro vyrovnávání zatížení a všechny související prostředky. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [seznam účtů úložiště az](https://docs.microsoft.com/cli/azure/storage/account#list) | Účty úložiště seznamy |
| [AZ název účtu úložiště kontrola-](https://docs.microsoft.com/cli/azure/storage/account#check-name) | Ověří, zda je platný název účtu úložiště a že ještě neexistuje |
| [seznam klíčů účtu úložiště az](https://docs.microsoft.com/cli/azure/storage/account/keys#list) | Obsahuje seznam klíčů pro účty úložiště hello |
| [Objekt blob úložiště az existuje](https://docs.microsoft.com/cli/azure/storage/blob#exists) | Ověří, zda existuje hello objektů blob |
| [Vytvoření kontejneru úložiště az](https://docs.microsoft.com/cli/azure/storage/container#create) | Vytvoří kontejner v účtu úložiště. |
| [Nahrání objektu blob úložiště az](https://docs.microsoft.com/cli/azure/storage/blob#upload) | Vytvoří objekt blob v kontejneru hello odesílání hello virtuálního pevného disku. |
| [Seznam virtuálních počítačů az](https://docs.microsoft.com/cli/azure/vm#list) | Použít s `--query` zkontrolujte, zda text hello název virtuálního počítače se používá. | 
| [Vytvoření virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | Vytvoří hello virtuální počítače. |
| [AZ virtuálních počítačů přístupu sada linux uživatele](https://docs.microsoft.com/cli/azure/vm/access#set-linux-user) | Obnoví hello SSH klíče toogive hello aktuální uživatel přístup toohello virtuálních počítačů. |
| [virtuální počítač az seznam ip adres](https://docs.microsoft.com/cli/azure/vm#list-ip-addresses) | Získá IP adresu hello hello virtuální počítač, který byl vytvořen. |

## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
