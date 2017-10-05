---
title: "Přesunout virtuální počítač s Linuxem v Azure | Microsoft Docs"
description: "Přesuňte virtuální počítač s Linuxem do jiné předplatné nebo prostředek skupiny Azure v modelu nasazení Resource Manager."
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
ms.openlocfilehash: 4695a9c934f97f2b2d448c4990e7ad5533e38e9f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="move-a-linux-vm-to-another-subscription-or-resource-group"></a><span data-ttu-id="7ff17-103">Přesunout virtuální počítač s Linuxem do jiné skupiny pro předplatné nebo prostředek</span><span class="sxs-lookup"><span data-stu-id="7ff17-103">Move a Linux VM to another subscription or resource group</span></span>
<span data-ttu-id="7ff17-104">Tento článek vás provede postup přesunutí virtuálního počítače s Linuxem mezi skupinami prostředků nebo předplatných.</span><span class="sxs-lookup"><span data-stu-id="7ff17-104">This article walks you through how to move a Linux VM between resource groups or subscriptions.</span></span> <span data-ttu-id="7ff17-105">Přesunutí virtuálního počítače mezi předplatnými může být užitečné, když vytvoříte virtuální počítač v odběru osobní a chcete ho přesunout do předplatného ve vaší společnosti.</span><span class="sxs-lookup"><span data-stu-id="7ff17-105">Moving a VM between subscriptions can be handy if you created a VM in a personal subscription and now want to move it to your company's subscription.</span></span>

> [!IMPORTANT]
><span data-ttu-id="7ff17-106">V tuto chvíli nelze přesunout spravované disky.</span><span class="sxs-lookup"><span data-stu-id="7ff17-106">You cannot move Managed Disks at this time.</span></span> 
>
><span data-ttu-id="7ff17-107">Nové ID prostředků jsou vytvořené jako součást přesunutí.</span><span class="sxs-lookup"><span data-stu-id="7ff17-107">New resource IDs are created as part of the move.</span></span> <span data-ttu-id="7ff17-108">Po přesunutí virtuálního počítače je potřeba aktualizovat nástroje a skripty, které pomocí nového ID prostředku.</span><span class="sxs-lookup"><span data-stu-id="7ff17-108">Once the VM has been moved, you need to update your tools and scripts to use the new resource IDs.</span></span> 
> 
> 

## <a name="use-the-azure-cli-to-move-a-vm"></a><span data-ttu-id="7ff17-109">Použijte rozhraní příkazového řádku Azure k přesunutí virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="7ff17-109">Use the Azure CLI to move a VM</span></span>
<span data-ttu-id="7ff17-110">Chcete-li úspěšně přesunout virtuální počítač, musíte přesunout virtuální počítač a všechny její Podpůrné prostředky.</span><span class="sxs-lookup"><span data-stu-id="7ff17-110">To successfully move a VM, you need to move the VM and all its supporting resources.</span></span> <span data-ttu-id="7ff17-111">Použití **zobrazit skupiny azure** seznam všechny prostředky v skupinu prostředků a jejich ID příkazu.</span><span class="sxs-lookup"><span data-stu-id="7ff17-111">Use the **azure group show** command to list all the resources in a resource group and their IDs.</span></span> <span data-ttu-id="7ff17-112">Pomáhá výstup tohoto příkazu do souboru, můžete zkopírovat a vložit ID do novější příkazy.</span><span class="sxs-lookup"><span data-stu-id="7ff17-112">It helps to pipe the output of this command to a file so you can copy and paste the IDs into later commands.</span></span>

    azure group show <resourceGroupName>

<span data-ttu-id="7ff17-113">Chcete-li přesunout virtuální počítač a jeho prostředků do jiné skupině prostředků, použijte **přesunutí prostředku azure** rozhraní příkazového řádku příkaz.</span><span class="sxs-lookup"><span data-stu-id="7ff17-113">To move a VM and its resources to another resource group, use the **azure resource move** CLI command.</span></span> <span data-ttu-id="7ff17-114">Následující příklad ukazuje, jak přesunout virtuální počítač a nejběžnější prostředky, které vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="7ff17-114">The following example shows how to move a VM and the most common resources it requires.</span></span> <span data-ttu-id="7ff17-115">Používáme **-i** parametr a předejte jí seznam oddělený čárkami (bez mezer) ID pro prostředky. Chcete-li přesunout.</span><span class="sxs-lookup"><span data-stu-id="7ff17-115">We use the **-i** parameter and pass in a comma-separated list (without spaces) of IDs for the resources to move.</span></span>

    vm=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Compute/virtualMachines/<vmName>
    nic=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkInterfaces/<nicName>
    nsg=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkSecurityGroups/<nsgName>
    pip=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/publicIPAddresses/<publicIPName>
    vnet=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/virtualNetworks/<vnetName>
    diag=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<diagnosticStorageAccountName>
    storage=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<storageAcountName>      

    azure resource move --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag -d "<destinationResourceGroup>"

<span data-ttu-id="7ff17-116">Pokud chcete přesunout virtuální počítač a jeho prostředků do jiného předplatného, přidejte **– ID cílového předplatného & č. 60; destinationSubscriptionID & č. 62;** parametr k určení cílového odběru.</span><span class="sxs-lookup"><span data-stu-id="7ff17-116">If you want to move the VM and its resources to a different subscription, add the **--destination-subscriptionId &#60;destinationSubscriptionID&#62;** parameter to specify the destination subscription.</span></span>

<span data-ttu-id="7ff17-117">Pokud pracujete z příkazového řádku na počítači se systémem Windows, je nutné přidat  **$**  před názvy proměnných, když je deklarovat.</span><span class="sxs-lookup"><span data-stu-id="7ff17-117">If you are working from the Command Prompt on a Windows computer, you need to add a **$** in front of the variable names when you declare them.</span></span> <span data-ttu-id="7ff17-118">Toto není nutné v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="7ff17-118">This isn't needed in Linux.</span></span>

<span data-ttu-id="7ff17-119">Zobrazí se výzva k potvrzení, že chcete přesunout zadaný prostředek.</span><span class="sxs-lookup"><span data-stu-id="7ff17-119">You are asked to confirm that you want to move the specified resource.</span></span> <span data-ttu-id="7ff17-120">Typ **Y** potvrďte, že chcete přesunout prostředky.</span><span class="sxs-lookup"><span data-stu-id="7ff17-120">Type **Y** to confirm that you want to move the resources.</span></span>

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a><span data-ttu-id="7ff17-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7ff17-121">Next steps</span></span>
<span data-ttu-id="7ff17-122">Mnoho různých typů prostředků můžete přesouvat mezi skupinami prostředků a předplatná.</span><span class="sxs-lookup"><span data-stu-id="7ff17-122">You can move many different types of resources between resource groups and subscriptions.</span></span> <span data-ttu-id="7ff17-123">Další informace najdete v tématu, které se zabývá [přesunutím prostředků do nové skupiny prostředků nebo předplatného](../../resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="7ff17-123">For more information, see [Move resources to new resource group or subscription](../../resource-group-move-resources.md).</span></span>    

