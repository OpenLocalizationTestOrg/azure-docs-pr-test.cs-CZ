---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - Export nebo zkopírování snímku jako účet úložiště tooa virtuálního pevného disku v jiné oblasti. | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - Export nebo zkopírování snímku jako účet úložiště tooa virtuálního pevného disku v rámci stejného nebo jiného předplatného"
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
ms.openlocfilehash: 027c5e588c4f10d64d125c17f4c78a7d8e1ef060
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="exportcopy-managed-snapshots-as-vhd-tooa-storage-account-in-different-region-with-cli"></a>Export nebo zkopírování spravované snímků jako účet úložiště tooa virtuálního pevného disku v jiné oblasti pomocí rozhraní příkazového řádku

Tento skript exportuje účtu úložiště spravovaného snímku tooa v jiné oblasti. Nejprve generuje hello identifikátor URI pro SAS hello snímku a používá je toocopy ho tooa účet úložiště v jiné oblasti. Použijte tuto zálohu toomaintain skriptu spravované disky v jiné oblasti pro obnovení po havárii. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli[main](../../../cli_scripts/storage/copy-snapshots-to-storage-account/copy-snapshots-to-storage-account.sh "Copy snapshot")]


## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy toogenerate tooa účet úložiště pomocí SAS URI pořízení snímku identifikátor URI pro SAS pro spravované hello snímku a kopií. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [AZ snímku udělení přístupu](https://docs.microsoft.com/cli/azure/snapshot#grant-access) | Vygeneruje SAS jen pro čtení, použité toocopy základní účet úložiště tooa souboru virtuálního pevného disku nebo ho stáhnout tooon místní  |
| [kopírování objektu blob úložiště az start](https://docs.microsoft.com/en-us/cli/azure/storage/blob/copy#start) | Asynchronně zkopíruje objekt blob z jednoho tooanother účtu úložiště |

## <a name="next-steps"></a>Další kroky

[Vytvoření spravovaného disku z virtuálního pevného disku](./../scripts/storage-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json)

[Vytvoření virtuálního počítače z spravovaného disku](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další virtuální počítač a spravované disky ukázky skriptu rozhraní příkazového řádku najdete v hello [virtuální počítač Azure s Linuxem dokumentaci](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
