---
title: "Migraci účet Automation a prostředků | Microsoft Docs"
description: "Tento článek popisuje, jak přesunout účet Automation v Azure Automation a přidružených prostředků z jednoho předplatného."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
ms.assetid: 9c2db4a2-f324-48dc-8ce7-3343bf7230d5
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/21/2016
ms.author: magoedte
ms.openlocfilehash: 687da15bdaf854254321b59350f47549781676f5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="migrate-automation-account-and-resources"></a><span data-ttu-id="2c79f-103">Migraci účet Automation a prostředků</span><span class="sxs-lookup"><span data-stu-id="2c79f-103">Migrate Automation Account and resources</span></span>
<span data-ttu-id="2c79f-104">Pro účty Automation a jeho přidružené prostředky (tj. prostředky, sady runbook, moduly atd.), které jste vytvořili na portálu Azure a chcete provést migraci z jedné skupiny prostředků do jiné nebo z jedno předplatné na jiný, můžete to provést snadno pomocí [přesunout prostředky](../azure-resource-manager/resource-group-move-resources.md) funkce dostupné na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2c79f-104">For Automation accounts and its associated resources (i.e. assets, runbooks, modules, etc.) that you have created in the Azure portal and want to migrate from one resource group to another or from one subscription to another, you can accomplish this easily with the [move resources](../azure-resource-manager/resource-group-move-resources.md) feature available in the Azure portal.</span></span> <span data-ttu-id="2c79f-105">Před pokračováním této akce je však měli nejdřív si následující [kontrolní seznam před přesunutím prostředků](../azure-resource-manager/resource-group-move-resources.md#checklist-before-moving-resources) a navíc v seznamu níže jsou specifické pro automatizaci.</span><span class="sxs-lookup"><span data-stu-id="2c79f-105">However, before proceeding with this action, you should first review the following [checklist before moving resources](../azure-resource-manager/resource-group-move-resources.md#checklist-before-moving-resources) and additionally, the list below specific to Automation.</span></span>   

1. <span data-ttu-id="2c79f-106">Cílového předplatného/skupiny prostředků musí být ve stejné oblasti jako zdroj.</span><span class="sxs-lookup"><span data-stu-id="2c79f-106">The destination subscription/resource group must be in same region as the source.</span></span>  <span data-ttu-id="2c79f-107">Znamená, nelze jej přesunout účty Automation v oblastech.</span><span class="sxs-lookup"><span data-stu-id="2c79f-107">Meaning, Automation accounts cannot be moved across regions.</span></span>
2. <span data-ttu-id="2c79f-108">Při přesouvání prostředků (např. sady runbook, úlohy atd.), skupině zdrojové a cílové skupiny jsou zamčené pro dobu trvání operace.</span><span class="sxs-lookup"><span data-stu-id="2c79f-108">When moving resources (e.g. runbooks, jobs, etc.), both the source group and the target group are locked for the duration of the operation.</span></span> <span data-ttu-id="2c79f-109">Zápis a operace odstranění se v skupiny blokuje až po dokončení přesunu.</span><span class="sxs-lookup"><span data-stu-id="2c79f-109">Write and delete operations are blocked on the groups until the move completes.</span></span>  
3. <span data-ttu-id="2c79f-110">Všechny runbooky a proměnné, které odkazují na ID prostředků nebo předplatného z existujícího předplatného bude potřeba aktualizovat po dokončení migrace.</span><span class="sxs-lookup"><span data-stu-id="2c79f-110">Any runbooks or variables which reference a resource or subscription ID from the existing subscription will need to be updated after migration is completed.</span></span>   

> [!NOTE]
> <span data-ttu-id="2c79f-111">Tato funkce nepodporuje přesunutí klasické prostředky služby automation.</span><span class="sxs-lookup"><span data-stu-id="2c79f-111">This feature does not support moving Classic automation resources.</span></span>
>
>

## <a name="to-move-the-automation-account-using-the-portal"></a><span data-ttu-id="2c79f-112">Přesunutí účtu Automation pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="2c79f-112">To move the Automation Account using the portal</span></span>
1. <span data-ttu-id="2c79f-113">Z vašeho účtu Automation, klikněte na tlačítko **přesunout** v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="2c79f-113">From your Automation account, click **Move** at the top of the blade.</span></span><br> <span data-ttu-id="2c79f-114">![Přesuňte možnost](media/automation-migrate-account-subscription/automation-menu-move.png)</span><span class="sxs-lookup"><span data-stu-id="2c79f-114">![Move option](media/automation-migrate-account-subscription/automation-menu-move.png)</span></span><br>
2. <span data-ttu-id="2c79f-115">Na **přesunout prostředky** okno, Všimněte si, že představuje zdroje související s účtu Automation a vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="2c79f-115">On the **Move resources** blade, note that it presents resources related to both your Automation account and your resource group(s).</span></span>  <span data-ttu-id="2c79f-116">Vyberte **předplatné** a **skupiny prostředků** z rozevíracího seznamu, nebo vyberte možnost **vytvořit novou skupinu prostředků** a do pole zadejte nový název skupiny prostředků k dispozici.</span><span class="sxs-lookup"><span data-stu-id="2c79f-116">Select the **subscription** and **resource group** from the drop-down lists, or select the option **create a new resource group** and enter a new resource group name in the field provided.</span></span>  
3. <span data-ttu-id="2c79f-117">Zkontrolujte a zaškrtněte políčko můžete potvrdit *pochopit nástroje a skripty bude nutné aktualizovat, a použít nové ID prostředku po přesunutí prostředků* a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="2c79f-117">Review and select the checkbox to acknowledge you *understand tools and scripts will need to be updated to use new resource IDs after resources have been moved* and then click **OK**.</span></span><br> <span data-ttu-id="2c79f-118">![Přesun prostředků okna](media/automation-migrate-account-subscription/automation-move-resources-blade.png)</span><span class="sxs-lookup"><span data-stu-id="2c79f-118">![Move Resources Blade](media/automation-migrate-account-subscription/automation-move-resources-blade.png)</span></span><br>   

<span data-ttu-id="2c79f-119">Tato akce bude trvat několik minut na dokončení.</span><span class="sxs-lookup"><span data-stu-id="2c79f-119">This action will take several minutes to complete.</span></span>  <span data-ttu-id="2c79f-120">V **oznámení**, zobrazí se stav každé akce, který probíhá - ověřování, migrace a potom nakonec po dokončení.</span><span class="sxs-lookup"><span data-stu-id="2c79f-120">In **Notifications**, you will be presented with a status of each action that takes place - validation, migration, and then finally when it is completed.</span></span>     

## <a name="to-move-the-automation-account-using-powershell"></a><span data-ttu-id="2c79f-121">Přesunutí účtu Automation pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2c79f-121">To move the Automation Account using PowerShell</span></span>
<span data-ttu-id="2c79f-122">Chcete-li existující prostředky Automation přesunout do jiné skupiny prostředků nebo předplatného, použijte **Get-AzureRmResource** rutiny konkrétní účet Automation a potom **přesunutí AzureRmResource** rutiny provedení přesunu.</span><span class="sxs-lookup"><span data-stu-id="2c79f-122">To move existing Automation resources to another resource group or subscription, use the  **Get-AzureRmResource** cmdlet to get the specific Automation account and then **Move-AzureRmResource** cmdlet to perform the move.</span></span>

<span data-ttu-id="2c79f-123">V prvním příkladu ukazuje, jak přesunout účet služby Automation do nové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="2c79f-123">The first example shows how to move an Automation account to a new resource group.</span></span>

   ```
    $resource = Get-AzureRmResource -ResourceName "TestAutomationAccount" -ResourceGroupName "ResourceGroup01"
    Move-AzureRmResource -ResourceId $resource.ResourceId -DestinationResourceGroupName "NewResourceGroup"
   ```

<span data-ttu-id="2c79f-124">Po provedení výše uvedený příklad kódu se výzva k ověření, že chcete provést tuto akci.</span><span class="sxs-lookup"><span data-stu-id="2c79f-124">After you execute the above code example, you will be prompted to verify you want to perform this action.</span></span>  <span data-ttu-id="2c79f-125">Po kliknutí na tlačítko **Ano** a povolit skript, který chcete-li pokračovat, neobdržíte žádné oznámení, když provádí migraci.</span><span class="sxs-lookup"><span data-stu-id="2c79f-125">Once you click **Yes** and allow the script to proceed, you will not receive any notifications while it's performing the migration.</span></span>  

<span data-ttu-id="2c79f-126">Pokud chcete přesunout do nového předplatného, obsahovat hodnotu pro *DestinationSubscriptionId* parametr.</span><span class="sxs-lookup"><span data-stu-id="2c79f-126">To move to a new subscription, include a value for the *DestinationSubscriptionId* parameter.</span></span>

   ```
    $resource = Get-AzureRmResource -ResourceName "TestAutomationAccount" -ResourceGroupName "ResourceGroup01"
    Move-AzureRmResource -ResourceId $resource.ResourceId -DestinationResourceGroupName "NewResourceGroup" -DestinationSubscriptionId "SubscriptionId"
   ```

<span data-ttu-id="2c79f-127">Stejně jako u předchozí příklad, zobrazí se výzva k potvrzení přesunutí.</span><span class="sxs-lookup"><span data-stu-id="2c79f-127">As with the previous example, you will be prompted to confirm the move.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="2c79f-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2c79f-128">Next steps</span></span>
* <span data-ttu-id="2c79f-129">Další informace o přesunutí prostředků do nové skupiny prostředků nebo předplatného najdete v tématu [přesunutím prostředků do nové skupiny prostředků nebo předplatného](../azure-resource-manager/resource-group-move-resources.md)</span><span class="sxs-lookup"><span data-stu-id="2c79f-129">For more information about moving resources to new resource group or subscription, see [Move  resources to new resource group or subscription](../azure-resource-manager/resource-group-move-resources.md)</span></span>
* <span data-ttu-id="2c79f-130">Další informace o řízení přístupu na základě rolí ve službě Azure Automation najdete v článku [Řízení přístupu na základě rolí ve službě Azure Automation](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="2c79f-130">For more information about Role-based Access Control in Azure Automation, refer to [Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="2c79f-131">Další informace o rutinách prostředí PowerShell pro správu předplatného, najdete v části [pomocí prostředí Azure PowerShell s Resource Managerem](../azure-resource-manager/powershell-azure-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="2c79f-131">To learn about PowerShell cmdlets for managing your subscription, see [Using Azure PowerShell with Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md)</span></span>
* <span data-ttu-id="2c79f-132">Další informace o portálu funkce pro správu předplatného najdete v tématu [pomocí portálu Azure ke správě prostředků](../azure-resource-manager/resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="2c79f-132">To learn about portal features for managing your subscription, see [Using the Azure Portal to manage resources](../azure-resource-manager/resource-group-portal.md).</span></span>
