---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - kopírování (přesunout) spravované toosame disky nebo jiné předplatné | Microsoft Docs"
description: "Azure ukázka skriptu rozhraní příkazového řádku - toosame disky kopírování (přesunout), spravovat nebo jiném předplatném."
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
ms.openlocfilehash: 8581169baa0fd0e0eec1c72eab77b657f48b1cfa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="copy-managed-disks-toosame-or-different-subscription-with-cli"></a>Zkopírujte toosame spravované disky nebo jiné předplatné pomocí rozhraní příkazového řádku

Tento skript zkopíruje toosame spravovaného disku nebo jiného předplatného, ale v hello stejné oblasti. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli[main](../../../cli_scripts/storage/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.sh "Copy managed disk")]


## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy toocreate nový disk spravované hello cílové předplatné pomocí hello Id zdroje hello spravované disku. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Zobrazit az disku](https://docs.microsoft.com/cli/azure/disk#show) | Získá všechny vlastnosti hello spravovaného disku pomocí hello název a prostředků vlastnosti skupiny hello spravovaného disku. Vlastnost ID je použité toocopy hello spravované disku toodifferent předplatné.  |
| [Vytvoření az disku](https://docs.microsoft.com/cli/azure/disk#create) | Kopíruje se spravovaným diskem vytvořením nového spravovaného disku v jiném předplatném. pomocí Id a název nadřazené hello spravované disku.  |

## <a name="next-steps"></a>Další kroky

[Vytvoření virtuálního počítače z spravovaného disku](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další virtuální počítač a spravované disky ukázky skriptu rozhraní příkazového řádku najdete v hello [virtuální počítač Azure s Linuxem dokumentaci](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
