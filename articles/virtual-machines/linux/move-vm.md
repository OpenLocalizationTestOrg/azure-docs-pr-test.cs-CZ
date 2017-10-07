---
title: "aaaMove virtuálního počítače s Linuxem v Azure | Microsoft Docs"
description: "Přesunete virtuální počítač s Linuxem tooanother předplatné nebo skupinu prostředků v modelu nasazení Resource Manager hello."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d635f0a5-4458-4b95-a5f8-eed4f41eb4d4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 938d04234059111912f03e72d14dabd338bc0678
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-a-linux-vm-tooanother-subscription-or-resource-group"></a>Přesunutí virtuálního počítače s Linuxem tooanother předplatné nebo prostředek skupiny
Tento článek vás provede toomove virtuálního počítače s Linuxem mezi skupinami prostředků nebo předplatných. Přesunutí virtuálního počítače mezi předplatnými může být užitečné, když vytvoříte virtuální počítač v odběru osobní a teď chcete toomove ho předplatného tooyour společnosti.

> [!IMPORTANT]
>V tuto chvíli nelze přesunout spravované disky. 
>
>Nové ID prostředků jsou vytvořené jako součást přesunutí hello. Jakmile hello virtuálního počítače byl přesunut, musíte tooupdate vaše nástroje a skripty toouse hello nové ID prostředku. 
> 
> 

## <a name="use-hello-azure-cli-toomove-a-vm"></a>Použití Azure CLI toomove hello virtuálního počítače
toosuccessfully přesunout virtuální počítač, budete potřebovat toomove hello virtuálních počítačů a všechny její Podpůrné prostředky. Použití hello **zobrazit skupiny azure** příkaz toolist všechny prostředky hello v skupinu prostředků a jejich ID. Pomáhá toopipe hello výstup tohoto příkazu tooa souboru, můžete zkopírovat a vložit hello ID do novější příkazy.

    azure group show <resourceGroupName>

toomove virtuálního počítače a jeho skupin prostředků tooanother prostředky používají hello **přesunutí prostředku azure** rozhraní příkazového řádku příkaz. Hello následující příklad ukazuje, jak toomove virtuální počítač a prostředky nejběžnější hello vyžaduje. Používáme hello **-i** parametr a předejte jí seznam oddělený čárkami (bez mezer) ID pro toomove prostředky hello.

    vm=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Compute/virtualMachines/<vmName>
    nic=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkInterfaces/<nicName>
    nsg=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkSecurityGroups/<nsgName>
    pip=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/publicIPAddresses/<publicIPName>
    vnet=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/virtualNetworks/<vnetName>
    diag=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<diagnosticStorageAccountName>
    storage=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<storageAcountName>      

    azure resource move --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag -d "<destinationResourceGroup>"

Pokud chcete, aby toomove hello virtuální počítač a jeho prostředky tooa jiného předplatného, přidejte hello **– ID cílového předplatného & č. 60; destinationSubscriptionID & č. 62;** parametr toospecify hello cílového předplatného.

Pokud pracujete se z hello příkazového řádku na počítači se systémem Windows, musíte tooadd  **$**  před názvy proměnných hello při je deklarovat. Toto není nutné v systému Linux.

Jste vyzváni, které chcete toomove hello tooconfirm zadaný prostředek. Typ **Y** tooconfirm, že chcete toomove hello prostředky.

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Další kroky
Mnoho různých typů prostředků můžete přesouvat mezi skupinami prostředků a předplatná. Další informace najdete v tématu [přesunout skupiny prostředků toonew prostředků nebo předplatného](../../resource-group-move-resources.md).    

