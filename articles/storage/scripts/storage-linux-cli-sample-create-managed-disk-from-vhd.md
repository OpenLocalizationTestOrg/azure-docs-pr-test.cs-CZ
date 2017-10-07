---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvoření spravovaného disku ze souboru VHD v účtu úložiště v hello stejného předplatného. | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření spravovaného disku ze souboru VHD v účtu úložiště v hello stejného předplatného."
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.openlocfilehash: 1e792fdbb7daea92bf6a6589a5d8aab5b9b5a670
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-hello-same-subscription-with-cli"></a>Vytvoření spravovaného disku ze souboru VHD v účtu úložiště v hello stejného předplatného pomocí rozhraní příkazového řádku

Tento skript vytvoří se spravovaným diskem z soubor virtuálního pevného disku v účtu úložiště v hello stejného předplatného. Použijte tento skript tooimport specializované (není zobecněný/Sysprep) virtuálního pevného disku toomanaged operačního systému disku toocreate virtuálního počítače. Nebo ho používat tooimport data virtuálního pevného disku toomanaged datový disk. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli[main](../../../cli_scripts/storage/create-managed-data-disks-from-vhd/create-managed-data-disks-from-vhd.sh "Create managed disk from VHD")]


## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá tyto příkazy toocreate spravované disk z virtuálního pevného disku. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření az disku](https://docs.microsoft.com/cli/azure/disk#create) | Vytvoří se spravovaným diskem pomocí identifikátoru URI virtuálního pevného disku v účtu úložiště v hello stejného předplatného. |

## <a name="next-steps"></a>Další kroky

[Vytvoření virtuálního počítače připojením se spravovaným diskem jako disk operačního systému](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další virtuální počítač a spravované disky ukázky skriptu rozhraní příkazového řádku najdete v hello [virtuální počítač Azure s Linuxem dokumentaci](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
