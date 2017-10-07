---
title: "aaaMove prostředek virtuálního počítače s Windows v Azure | Microsoft Docs"
description: "Přesunete virtuální počítač s Windows tooanother předplatné nebo skupinu prostředků v modelu nasazení Resource Manager hello."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 859e78dce9acf1168780d4ee8e9f6dac0e3c11cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-a-windows-vm-tooanother-azure-subscription-or-resource-group"></a>Přesunout virtuální počítač s Windows tooanother předplatné nebo skupinu prostředků
Tento článek vás provede toomove virtuální počítač s Windows mezi skupinami prostředků nebo předplatných. Přesouvání mezi předplatnými může být užitečné, pokud jste původně vytvořili virtuální počítač v odběru osobní a teď chcete toomove ho tooyour společnosti předplatné toocontinue práci.

> [!IMPORTANT]
>V tuto chvíli nelze přesunout spravované disky. 
>
>Nové ID prostředků jsou vytvořené jako součást přesunutí hello. Jakmile hello virtuálního počítače byl přesunut, musíte tooupdate vaše nástroje a skripty toouse hello nové ID prostředku. 
> 
> 

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="use-powershell-toomove-a-vm"></a>Pomocí prostředí Powershell toomove virtuálního počítače
toomove skupiny prostředků tooanother virtuálního počítače, musíte toomake jistotu, že také přesunout všechny závislé prostředky hello. rutinu Move-AzureRMResource hello toouse, potřebujete název prostředku hello a hello typ prostředku. Můžete získat z rutiny hello najít AzureRMResource i.

    Find-AzureRMResource -ResourceGroupNameContains "<sourceResourceGroupName>"


toomove potřebujeme toomove několik prostředků virtuálního počítače. Můžeme jenom vytvářet samostatné proměnných pro každý prostředek a potom jejich seznam. Tento příklad obsahuje většinu hello základní prostředků pro virtuální počítač, ale můžete přidat více podle potřeby.

    $sourceRG = "<sourceResourceGroupName>"
    $destinationRG = "<destinationResourceGroupName>"

    $vm = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Compute/virtualMachines" -ResourceName "<vmName>"
    $storageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<storageAccountName>"
    $diagStorageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<diagnosticStorageAccountName>"
    $vNet = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/virtualNetworks" -ResourceName "<vNetName>"
    $nic = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkInterfaces" -ResourceName "<nicName>"
    $ip = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/publicIPAddresses" -ResourceName "<ipName>"
    $nsg = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkSecurityGroups" -ResourceName "<nsgName>"

    Move-AzureRmResource -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId

toomove hello prostředky předplatného toodifferent, zahrnout hello **- DestinationSubscriptionId** parametr. 

    Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId



Zobrazí se výzva, které chcete toomove hello tooconfirm zadané prostředky. Typ **Y** tooconfirm, že chcete toomove hello prostředky.

## <a name="next-steps"></a>Další kroky
Mnoho různých typů prostředků můžete přesouvat mezi skupinami prostředků a předplatná. Další informace najdete v tématu [přesunout skupiny prostředků toonew prostředků nebo předplatného](../../resource-group-move-resources.md).    

