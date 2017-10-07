---
title: "aaaMigrate účet Automation a prostředky | Microsoft Docs"
description: "Tento článek popisuje, jak toomove Automation účtu v Azure Automation a přidružených prostředků z tooanother jedno předplatné."
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
ms.openlocfilehash: 201c9091cd2d78d7ea407c1e5fb27f366bb4fa8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-automation-account-and-resources"></a><span data-ttu-id="2253c-103">Migraci účet Automation a prostředků</span><span class="sxs-lookup"><span data-stu-id="2253c-103">Migrate Automation Account and resources</span></span>
<span data-ttu-id="2253c-104">Pro účty Automation a jeho přidružené prostředky (tj. prostředky, sady runbook, moduly atd.), které jste vytvořili v hello portál Azure a chcete toomigrate z jednoho prostředku skupiny tooanother nebo z tooanother jedno předplatné, můžete to provést snadno s Hello [přesunout prostředky](../azure-resource-manager/resource-group-move-resources.md) funkce, které jsou k dispozici v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2253c-104">For Automation accounts and its associated resources (i.e. assets, runbooks, modules, etc.) that you have created in hello Azure portal and want toomigrate from one resource group tooanother or from one subscription tooanother, you can accomplish this easily with hello [move resources](../azure-resource-manager/resource-group-move-resources.md) feature available in hello Azure portal.</span></span> <span data-ttu-id="2253c-105">Před pokračováním této akce, je však měli nejdřív si následující hello [kontrolní seznam před přesunutím prostředků](../azure-resource-manager/resource-group-move-resources.md#checklist-before-moving-resources) a kromě toho hello seznamu konkrétní tooAutomation.</span><span class="sxs-lookup"><span data-stu-id="2253c-105">However, before proceeding with this action, you should first review hello following [checklist before moving resources](../azure-resource-manager/resource-group-move-resources.md#checklist-before-moving-resources) and additionally, hello list below specific tooAutomation.</span></span>   

1. <span data-ttu-id="2253c-106">Hello cílového předplatného/skupiny prostředků musí být ve stejné oblasti jako zdroj hello.</span><span class="sxs-lookup"><span data-stu-id="2253c-106">hello destination subscription/resource group must be in same region as hello source.</span></span>  <span data-ttu-id="2253c-107">Znamená, nelze jej přesunout účty Automation v oblastech.</span><span class="sxs-lookup"><span data-stu-id="2253c-107">Meaning, Automation accounts cannot be moved across regions.</span></span>
2. <span data-ttu-id="2253c-108">Při přesouvání prostředků (např. sady runbook, úlohy atd.), hello zdrojová skupina a hello cílové skupiny jsou zamčené pro dobu trvání hello hello operace.</span><span class="sxs-lookup"><span data-stu-id="2253c-108">When moving resources (e.g. runbooks, jobs, etc.), both hello source group and hello target group are locked for hello duration of hello operation.</span></span> <span data-ttu-id="2253c-109">Zápis a operace odstranění se v hello skupiny blokuje až po dokončení přesunu hello.</span><span class="sxs-lookup"><span data-stu-id="2253c-109">Write and delete operations are blocked on hello groups until hello move completes.</span></span>  
3. <span data-ttu-id="2253c-110">Všechny runbooky a proměnné, které odkazují na ID prostředků nebo předplatného z existujícího předplatného hello potřebovat toobe aktualizován po dokončení migrace.</span><span class="sxs-lookup"><span data-stu-id="2253c-110">Any runbooks or variables which reference a resource or subscription ID from hello existing subscription will need toobe updated after migration is completed.</span></span>   

> [!NOTE]
> <span data-ttu-id="2253c-111">Tato funkce nepodporuje přesunutí klasické prostředky služby automation.</span><span class="sxs-lookup"><span data-stu-id="2253c-111">This feature does not support moving Classic automation resources.</span></span>
>
>

## <a name="toomove-hello-automation-account-using-hello-portal"></a><span data-ttu-id="2253c-112">toomove hello účtu Automation pomocí portálu hello</span><span class="sxs-lookup"><span data-stu-id="2253c-112">toomove hello Automation Account using hello portal</span></span>
1. <span data-ttu-id="2253c-113">Z vašeho účtu Automation, klikněte na tlačítko **přesunout** hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="2253c-113">From your Automation account, click **Move** at hello top of hello blade.</span></span><br> <span data-ttu-id="2253c-114">![Přesuňte možnost](media/automation-migrate-account-subscription/automation-menu-move.png)</span><span class="sxs-lookup"><span data-stu-id="2253c-114">![Move option](media/automation-migrate-account-subscription/automation-menu-move.png)</span></span><br>
2. <span data-ttu-id="2253c-115">Na hello **přesunout prostředky** okno, Všimněte si, že uvede tooboth související prostředky účtu Automation a vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="2253c-115">On hello **Move resources** blade, note that it presents resources related tooboth your Automation account and your resource group(s).</span></span>  <span data-ttu-id="2253c-116">Vyberte hello **předplatné** a **skupiny prostředků** z rozevíracího seznamu hello nebo vyberte hello možnost **vytvořit novou skupinu prostředků** a zadejte nový název skupiny prostředků v Zadané pole Hello.</span><span class="sxs-lookup"><span data-stu-id="2253c-116">Select hello **subscription** and **resource group** from hello drop-down lists, or select hello option **create a new resource group** and enter a new resource group name in hello field provided.</span></span>  
3. <span data-ttu-id="2253c-117">Přečtěte si a vyberte hello políčko tooacknowledge jste *pochopit nástroje a skripty, bude nutné aktualizovat toobe toouse nový prostředek ID po přesunutí prostředků* a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="2253c-117">Review and select hello checkbox tooacknowledge you *understand tools and scripts will need toobe updated toouse new resource IDs after resources have been moved* and then click **OK**.</span></span><br> <span data-ttu-id="2253c-118">![Přesun prostředků okna](media/automation-migrate-account-subscription/automation-move-resources-blade.png)</span><span class="sxs-lookup"><span data-stu-id="2253c-118">![Move Resources Blade](media/automation-migrate-account-subscription/automation-move-resources-blade.png)</span></span><br>   

<span data-ttu-id="2253c-119">Tato akce bude trvat několik minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="2253c-119">This action will take several minutes toocomplete.</span></span>  <span data-ttu-id="2253c-120">V **oznámení**, zobrazí se stav každé akce, který probíhá - ověřování, migrace a potom nakonec po dokončení.</span><span class="sxs-lookup"><span data-stu-id="2253c-120">In **Notifications**, you will be presented with a status of each action that takes place - validation, migration, and then finally when it is completed.</span></span>     

## <a name="toomove-hello-automation-account-using-powershell"></a><span data-ttu-id="2253c-121">toomove hello účtu Automation pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2253c-121">toomove hello Automation Account using PowerShell</span></span>
<span data-ttu-id="2253c-122">toomove existující skupiny prostředků tooanother prostředky Automation nebo předplatného, použijte hello **Get-AzureRmResource** rutiny tooget hello konkrétní účet Automation a potom **přesunutí AzureRmResource** rutiny tooperform hello přesunout.</span><span class="sxs-lookup"><span data-stu-id="2253c-122">toomove existing Automation resources tooanother resource group or subscription, use hello  **Get-AzureRmResource** cmdlet tooget hello specific Automation account and then **Move-AzureRmResource** cmdlet tooperform hello move.</span></span>

<span data-ttu-id="2253c-123">Hello první příklad ukazuje, jak toomove Automation účet tooa novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="2253c-123">hello first example shows how toomove an Automation account tooa new resource group.</span></span>

   ```
    $resource = Get-AzureRmResource -ResourceName "TestAutomationAccount" -ResourceGroupName "ResourceGroup01"
    Move-AzureRmResource -ResourceId $resource.ResourceId -DestinationResourceGroupName "NewResourceGroup"
   ```

<span data-ttu-id="2253c-124">Po provedení hello výše ukázkový kód bude výzvami tooverify tooperform chcete tuto akci.</span><span class="sxs-lookup"><span data-stu-id="2253c-124">After you execute hello above code example, you will be prompted tooverify you want tooperform this action.</span></span>  <span data-ttu-id="2253c-125">Po kliknutí na tlačítko **Ano** a povolit hello tooproceed skriptu, neobdržíte žádné oznámení, když provádí migraci hello.</span><span class="sxs-lookup"><span data-stu-id="2253c-125">Once you click **Yes** and allow hello script tooproceed, you will not receive any notifications while it's performing hello migration.</span></span>  

<span data-ttu-id="2253c-126">toomove tooa nové předplatné, vložte hodnotu pro hello *DestinationSubscriptionId* parametr.</span><span class="sxs-lookup"><span data-stu-id="2253c-126">toomove tooa new subscription, include a value for hello *DestinationSubscriptionId* parameter.</span></span>

   ```
    $resource = Get-AzureRmResource -ResourceName "TestAutomationAccount" -ResourceGroupName "ResourceGroup01"
    Move-AzureRmResource -ResourceId $resource.ResourceId -DestinationResourceGroupName "NewResourceGroup" -DestinationSubscriptionId "SubscriptionId"
   ```

<span data-ttu-id="2253c-127">Stejně jako v předchozím příkladu hello bude výzvami tooconfirm hello move.</span><span class="sxs-lookup"><span data-stu-id="2253c-127">As with hello previous example, you will be prompted tooconfirm hello move.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="2253c-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2253c-128">Next steps</span></span>
* <span data-ttu-id="2253c-129">Další informace o přesunutí skupiny prostředků toonew prostředků nebo předplatného najdete v tématu [přesunout skupiny prostředků toonew prostředků nebo předplatného](../azure-resource-manager/resource-group-move-resources.md)</span><span class="sxs-lookup"><span data-stu-id="2253c-129">For more information about moving resources toonew resource group or subscription, see [Move  resources toonew resource group or subscription](../azure-resource-manager/resource-group-move-resources.md)</span></span>
* <span data-ttu-id="2253c-130">Další informace o řízení přístupu na základě rolí ve službě Azure Automation najdete v části příliš[řízení přístupu na základě Role ve službě Azure Automation](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="2253c-130">For more information about Role-based Access Control in Azure Automation, refer too[Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="2253c-131">toolearn o rutinách prostředí PowerShell pro správu předplatného, najdete v části [pomocí prostředí Azure PowerShell s Resource Managerem](../azure-resource-manager/powershell-azure-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="2253c-131">toolearn about PowerShell cmdlets for managing your subscription, see [Using Azure PowerShell with Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md)</span></span>
* <span data-ttu-id="2253c-132">toolearn o funkcích portálu pro správu předplatného, najdete v části [pomocí hello portálu Azure toomanage prostředky](../azure-resource-manager/resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="2253c-132">toolearn about portal features for managing your subscription, see [Using hello Azure Portal toomanage resources](../azure-resource-manager/resource-group-portal.md).</span></span>
