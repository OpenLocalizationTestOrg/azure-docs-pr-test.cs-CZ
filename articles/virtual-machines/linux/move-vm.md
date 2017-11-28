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
# <a name="move-a-linux-vm-tooanother-subscription-or-resource-group"></a><span data-ttu-id="08de9-103">Přesunutí virtuálního počítače s Linuxem tooanother předplatné nebo prostředek skupiny</span><span class="sxs-lookup"><span data-stu-id="08de9-103">Move a Linux VM tooanother subscription or resource group</span></span>
<span data-ttu-id="08de9-104">Tento článek vás provede toomove virtuálního počítače s Linuxem mezi skupinami prostředků nebo předplatných.</span><span class="sxs-lookup"><span data-stu-id="08de9-104">This article walks you through how toomove a Linux VM between resource groups or subscriptions.</span></span> <span data-ttu-id="08de9-105">Přesunutí virtuálního počítače mezi předplatnými může být užitečné, když vytvoříte virtuální počítač v odběru osobní a teď chcete toomove ho předplatného tooyour společnosti.</span><span class="sxs-lookup"><span data-stu-id="08de9-105">Moving a VM between subscriptions can be handy if you created a VM in a personal subscription and now want toomove it tooyour company's subscription.</span></span>

> [!IMPORTANT]
><span data-ttu-id="08de9-106">V tuto chvíli nelze přesunout spravované disky.</span><span class="sxs-lookup"><span data-stu-id="08de9-106">You cannot move Managed Disks at this time.</span></span> 
>
><span data-ttu-id="08de9-107">Nové ID prostředků jsou vytvořené jako součást přesunutí hello.</span><span class="sxs-lookup"><span data-stu-id="08de9-107">New resource IDs are created as part of hello move.</span></span> <span data-ttu-id="08de9-108">Jakmile hello virtuálního počítače byl přesunut, musíte tooupdate vaše nástroje a skripty toouse hello nové ID prostředku.</span><span class="sxs-lookup"><span data-stu-id="08de9-108">Once hello VM has been moved, you need tooupdate your tools and scripts toouse hello new resource IDs.</span></span> 
> 
> 

## <a name="use-hello-azure-cli-toomove-a-vm"></a><span data-ttu-id="08de9-109">Použití Azure CLI toomove hello virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="08de9-109">Use hello Azure CLI toomove a VM</span></span>
<span data-ttu-id="08de9-110">toosuccessfully přesunout virtuální počítač, budete potřebovat toomove hello virtuálních počítačů a všechny její Podpůrné prostředky.</span><span class="sxs-lookup"><span data-stu-id="08de9-110">toosuccessfully move a VM, you need toomove hello VM and all its supporting resources.</span></span> <span data-ttu-id="08de9-111">Použití hello **zobrazit skupiny azure** příkaz toolist všechny prostředky hello v skupinu prostředků a jejich ID.</span><span class="sxs-lookup"><span data-stu-id="08de9-111">Use hello **azure group show** command toolist all hello resources in a resource group and their IDs.</span></span> <span data-ttu-id="08de9-112">Pomáhá toopipe hello výstup tohoto příkazu tooa souboru, můžete zkopírovat a vložit hello ID do novější příkazy.</span><span class="sxs-lookup"><span data-stu-id="08de9-112">It helps toopipe hello output of this command tooa file so you can copy and paste hello IDs into later commands.</span></span>

    azure group show <resourceGroupName>

<span data-ttu-id="08de9-113">toomove virtuálního počítače a jeho skupin prostředků tooanother prostředky používají hello **přesunutí prostředku azure** rozhraní příkazového řádku příkaz.</span><span class="sxs-lookup"><span data-stu-id="08de9-113">toomove a VM and its resources tooanother resource group, use hello **azure resource move** CLI command.</span></span> <span data-ttu-id="08de9-114">Hello následující příklad ukazuje, jak toomove virtuální počítač a prostředky nejběžnější hello vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="08de9-114">hello following example shows how toomove a VM and hello most common resources it requires.</span></span> <span data-ttu-id="08de9-115">Používáme hello **-i** parametr a předejte jí seznam oddělený čárkami (bez mezer) ID pro toomove prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="08de9-115">We use hello **-i** parameter and pass in a comma-separated list (without spaces) of IDs for hello resources toomove.</span></span>

    vm=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Compute/virtualMachines/<vmName>
    nic=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkInterfaces/<nicName>
    nsg=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkSecurityGroups/<nsgName>
    pip=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/publicIPAddresses/<publicIPName>
    vnet=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/virtualNetworks/<vnetName>
    diag=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<diagnosticStorageAccountName>
    storage=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<storageAcountName>      

    azure resource move --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag -d "<destinationResourceGroup>"

<span data-ttu-id="08de9-116">Pokud chcete, aby toomove hello virtuální počítač a jeho prostředky tooa jiného předplatného, přidejte hello **– ID cílového předplatného & č. 60; destinationSubscriptionID & č. 62;** parametr toospecify hello cílového předplatného.</span><span class="sxs-lookup"><span data-stu-id="08de9-116">If you want toomove hello VM and its resources tooa different subscription, add hello **--destination-subscriptionId &#60;destinationSubscriptionID&#62;** parameter toospecify hello destination subscription.</span></span>

<span data-ttu-id="08de9-117">Pokud pracujete se z hello příkazového řádku na počítači se systémem Windows, musíte tooadd  **$**  před názvy proměnných hello při je deklarovat.</span><span class="sxs-lookup"><span data-stu-id="08de9-117">If you are working from hello Command Prompt on a Windows computer, you need tooadd a **$** in front of hello variable names when you declare them.</span></span> <span data-ttu-id="08de9-118">Toto není nutné v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="08de9-118">This isn't needed in Linux.</span></span>

<span data-ttu-id="08de9-119">Jste vyzváni, které chcete toomove hello tooconfirm zadaný prostředek.</span><span class="sxs-lookup"><span data-stu-id="08de9-119">You are asked tooconfirm that you want toomove hello specified resource.</span></span> <span data-ttu-id="08de9-120">Typ **Y** tooconfirm, že chcete toomove hello prostředky.</span><span class="sxs-lookup"><span data-stu-id="08de9-120">Type **Y** tooconfirm that you want toomove hello resources.</span></span>

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a><span data-ttu-id="08de9-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="08de9-121">Next steps</span></span>
<span data-ttu-id="08de9-122">Mnoho různých typů prostředků můžete přesouvat mezi skupinami prostředků a předplatná.</span><span class="sxs-lookup"><span data-stu-id="08de9-122">You can move many different types of resources between resource groups and subscriptions.</span></span> <span data-ttu-id="08de9-123">Další informace najdete v tématu [přesunout skupiny prostředků toonew prostředků nebo předplatného](../../resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="08de9-123">For more information, see [Move resources toonew resource group or subscription](../../resource-group-move-resources.md).</span></span>    

