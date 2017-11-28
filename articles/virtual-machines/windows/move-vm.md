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
# <a name="move-a-windows-vm-tooanother-azure-subscription-or-resource-group"></a><span data-ttu-id="8f561-103">Přesunout virtuální počítač s Windows tooanother předplatné nebo skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="8f561-103">Move a Windows VM tooanother Azure subscription or resource group</span></span>
<span data-ttu-id="8f561-104">Tento článek vás provede toomove virtuální počítač s Windows mezi skupinami prostředků nebo předplatných.</span><span class="sxs-lookup"><span data-stu-id="8f561-104">This article walks you through how toomove a Windows VM between resource groups or subscriptions.</span></span> <span data-ttu-id="8f561-105">Přesouvání mezi předplatnými může být užitečné, pokud jste původně vytvořili virtuální počítač v odběru osobní a teď chcete toomove ho tooyour společnosti předplatné toocontinue práci.</span><span class="sxs-lookup"><span data-stu-id="8f561-105">Moving between subscriptions can be handy if you originally created a VM in a personal subscription and now want toomove it tooyour company's subscription toocontinue your work.</span></span>

> [!IMPORTANT]
><span data-ttu-id="8f561-106">V tuto chvíli nelze přesunout spravované disky.</span><span class="sxs-lookup"><span data-stu-id="8f561-106">You cannot move Managed Disks at this time.</span></span> 
>
><span data-ttu-id="8f561-107">Nové ID prostředků jsou vytvořené jako součást přesunutí hello.</span><span class="sxs-lookup"><span data-stu-id="8f561-107">New resource IDs are created as part of hello move.</span></span> <span data-ttu-id="8f561-108">Jakmile hello virtuálního počítače byl přesunut, musíte tooupdate vaše nástroje a skripty toouse hello nové ID prostředku.</span><span class="sxs-lookup"><span data-stu-id="8f561-108">Once hello VM has been moved, you need tooupdate your tools and scripts toouse hello new resource IDs.</span></span> 
> 
> 

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="use-powershell-toomove-a-vm"></a><span data-ttu-id="8f561-109">Pomocí prostředí Powershell toomove virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="8f561-109">Use Powershell toomove a VM</span></span>
<span data-ttu-id="8f561-110">toomove skupiny prostředků tooanother virtuálního počítače, musíte toomake jistotu, že také přesunout všechny závislé prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="8f561-110">toomove a virtual machine tooanother resource group, you need toomake sure that you also move all of hello dependent resources.</span></span> <span data-ttu-id="8f561-111">rutinu Move-AzureRMResource hello toouse, potřebujete název prostředku hello a hello typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="8f561-111">toouse hello Move-AzureRMResource cmdlet, you need hello resource name and hello type of resource.</span></span> <span data-ttu-id="8f561-112">Můžete získat z rutiny hello najít AzureRMResource i.</span><span class="sxs-lookup"><span data-stu-id="8f561-112">You can get both from hello Find-AzureRMResource cmdlet.</span></span>

    Find-AzureRMResource -ResourceGroupNameContains "<sourceResourceGroupName>"


<span data-ttu-id="8f561-113">toomove potřebujeme toomove několik prostředků virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8f561-113">toomove a VM we need toomove multiple resources.</span></span> <span data-ttu-id="8f561-114">Můžeme jenom vytvářet samostatné proměnných pro každý prostředek a potom jejich seznam.</span><span class="sxs-lookup"><span data-stu-id="8f561-114">We can just create separate variables for each resource and then list them.</span></span> <span data-ttu-id="8f561-115">Tento příklad obsahuje většinu hello základní prostředků pro virtuální počítač, ale můžete přidat více podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="8f561-115">This example includes most of hello basic resources for a VM, but you can add more as needed.</span></span>

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

<span data-ttu-id="8f561-116">toomove hello prostředky předplatného toodifferent, zahrnout hello **- DestinationSubscriptionId** parametr.</span><span class="sxs-lookup"><span data-stu-id="8f561-116">toomove hello resources toodifferent subscription, include hello **-DestinationSubscriptionId** parameter.</span></span> 

    Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId



<span data-ttu-id="8f561-117">Zobrazí se výzva, které chcete toomove hello tooconfirm zadané prostředky.</span><span class="sxs-lookup"><span data-stu-id="8f561-117">You will be asked tooconfirm that you want toomove hello specified resources.</span></span> <span data-ttu-id="8f561-118">Typ **Y** tooconfirm, že chcete toomove hello prostředky.</span><span class="sxs-lookup"><span data-stu-id="8f561-118">Type **Y** tooconfirm that you want toomove hello resources.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8f561-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8f561-119">Next steps</span></span>
<span data-ttu-id="8f561-120">Mnoho různých typů prostředků můžete přesouvat mezi skupinami prostředků a předplatná.</span><span class="sxs-lookup"><span data-stu-id="8f561-120">You can move many different types of resources between resource groups and subscriptions.</span></span> <span data-ttu-id="8f561-121">Další informace najdete v tématu [přesunout skupiny prostředků toonew prostředků nebo předplatného](../../resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="8f561-121">For more information, see [Move resources toonew resource group or subscription](../../resource-group-move-resources.md).</span></span>    

