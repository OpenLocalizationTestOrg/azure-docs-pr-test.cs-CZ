---
title: "Přesunutí prostředku virtuálního počítače s Windows v Azure | Microsoft Docs"
description: "Přesuňte virtuální počítač s Windows do jiné předplatné nebo prostředek skupiny Azure v modelu nasazení Resource Manager."
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
ms.openlocfilehash: 1db25a5d9ff5cb6aa2787a0cafa40cfb010e3b06
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a><span data-ttu-id="e4412-103">Přesunout virtuální počítač s Windows do Azure jiné předplatné nebo prostředek skupiny</span><span class="sxs-lookup"><span data-stu-id="e4412-103">Move a Windows VM to another Azure subscription or resource group</span></span>
<span data-ttu-id="e4412-104">Tento článek vás provede jak přesunout virtuální počítač s Windows mezi skupinami prostředků nebo předplatných.</span><span class="sxs-lookup"><span data-stu-id="e4412-104">This article walks you through how to move a Windows VM between resource groups or subscriptions.</span></span> <span data-ttu-id="e4412-105">Přesouvání mezi předplatnými může být užitečné, pokud jste původně vytvořili virtuální počítač v odběru osobní a chcete ho přesunout do předplatného vaší společnosti chcete-li pokračovat v práci.</span><span class="sxs-lookup"><span data-stu-id="e4412-105">Moving between subscriptions can be handy if you originally created a VM in a personal subscription and now want to move it to your company's subscription to continue your work.</span></span>

> [!IMPORTANT]
><span data-ttu-id="e4412-106">V tuto chvíli nelze přesunout spravované disky.</span><span class="sxs-lookup"><span data-stu-id="e4412-106">You cannot move Managed Disks at this time.</span></span> 
>
><span data-ttu-id="e4412-107">Nové ID prostředků jsou vytvořené jako součást přesunutí.</span><span class="sxs-lookup"><span data-stu-id="e4412-107">New resource IDs are created as part of the move.</span></span> <span data-ttu-id="e4412-108">Po přesunutí virtuálního počítače je potřeba aktualizovat nástroje a skripty, které pomocí nového ID prostředku.</span><span class="sxs-lookup"><span data-stu-id="e4412-108">Once the VM has been moved, you need to update your tools and scripts to use the new resource IDs.</span></span> 
> 
> 

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="use-powershell-to-move-a-vm"></a><span data-ttu-id="e4412-109">Přesunout virtuální počítač pomocí prostředí Powershell</span><span class="sxs-lookup"><span data-stu-id="e4412-109">Use Powershell to move a VM</span></span>
<span data-ttu-id="e4412-110">Chcete-li přesunout virtuální počítač do jiné skupiny prostředků, ujistěte se, že také přesunout všechny závislé prostředky.</span><span class="sxs-lookup"><span data-stu-id="e4412-110">To move a virtual machine to another resource group, you need to make sure that you also move all of the dependent resources.</span></span> <span data-ttu-id="e4412-111">Chcete-li použijte rutinu Move-AzureRMResource, potřebujete název prostředku a typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="e4412-111">To use the Move-AzureRMResource cmdlet, you need the resource name and the type of resource.</span></span> <span data-ttu-id="e4412-112">Můžete získat z rutiny najít AzureRMResource i.</span><span class="sxs-lookup"><span data-stu-id="e4412-112">You can get both from the Find-AzureRMResource cmdlet.</span></span>

    Find-AzureRMResource -ResourceGroupNameContains "<sourceResourceGroupName>"


<span data-ttu-id="e4412-113">Přesunout virtuální počítač je potřeba přesunout více prostředků.</span><span class="sxs-lookup"><span data-stu-id="e4412-113">To move a VM we need to move multiple resources.</span></span> <span data-ttu-id="e4412-114">Můžeme jenom vytvářet samostatné proměnných pro každý prostředek a potom jejich seznam.</span><span class="sxs-lookup"><span data-stu-id="e4412-114">We can just create separate variables for each resource and then list them.</span></span> <span data-ttu-id="e4412-115">Tento příklad obsahuje většinu základní prostředků pro virtuální počítač, ale můžete přidat více podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="e4412-115">This example includes most of the basic resources for a VM, but you can add more as needed.</span></span>

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

<span data-ttu-id="e4412-116">Chcete-li přesunout prostředky do jiného předplatného, zahrňte **- DestinationSubscriptionId** parametr.</span><span class="sxs-lookup"><span data-stu-id="e4412-116">To move the resources to different subscription, include the **-DestinationSubscriptionId** parameter.</span></span> 

    Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId



<span data-ttu-id="e4412-117">Jste vyzváni k potvrzení, že chcete přesunout zadané prostředky.</span><span class="sxs-lookup"><span data-stu-id="e4412-117">You will be asked to confirm that you want to move the specified resources.</span></span> <span data-ttu-id="e4412-118">Typ **Y** potvrďte, že chcete přesunout prostředky.</span><span class="sxs-lookup"><span data-stu-id="e4412-118">Type **Y** to confirm that you want to move the resources.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e4412-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e4412-119">Next steps</span></span>
<span data-ttu-id="e4412-120">Mnoho různých typů prostředků můžete přesouvat mezi skupinami prostředků a předplatná.</span><span class="sxs-lookup"><span data-stu-id="e4412-120">You can move many different types of resources between resource groups and subscriptions.</span></span> <span data-ttu-id="e4412-121">Další informace najdete v tématu, které se zabývá [přesunutím prostředků do nové skupiny prostředků nebo předplatného](../../resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="e4412-121">For more information, see [Move resources to new resource group or subscription](../../resource-group-move-resources.md).</span></span>    

