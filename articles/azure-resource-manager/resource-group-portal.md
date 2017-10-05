---
title: "Použití portálu Azure ke správě prostředků Azure | Microsoft Docs"
description: "Ke správě prostředků pomocí portálu Azure a správě prostředků Azure. Ukazuje, jak pracovat s řídicí panely a sledujte prostředky."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 0725bbf2-5913-4c07-af6e-24e11d957fbc
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: tomfitz
ms.openlocfilehash: 7a94fd5065de93384460e851627a9813d439956b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="manage-azure-resources-through-portal"></a><span data-ttu-id="fe780-104">Spravovat prostředky prostřednictvím portálu Azure</span><span class="sxs-lookup"><span data-stu-id="fe780-104">Manage Azure resources through portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fe780-105">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="fe780-105">Azure PowerShell</span></span>](powershell-azure-resource-manager.md)
> * [<span data-ttu-id="fe780-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="fe780-106">Azure CLI</span></span>](xplat-cli-azure-resource-manager.md)
> * [<span data-ttu-id="fe780-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="fe780-107">Portal</span></span>](resource-group-portal.md) 
> * [<span data-ttu-id="fe780-108">REST API</span><span class="sxs-lookup"><span data-stu-id="fe780-108">REST API</span></span>](resource-manager-rest-api.md)
> 
> 

<span data-ttu-id="fe780-109">Toto téma ukazuje, jak používat [portál Azure](https://portal.azure.com) s [Azure Resource Manager](resource-group-overview.md) ke správě prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="fe780-109">This topic shows how to use the [Azure portal](https://portal.azure.com) with [Azure Resource Manager](resource-group-overview.md) to manage your Azure resources.</span></span> <span data-ttu-id="fe780-110">Další informace o nasazení prostředků prostřednictvím portálu najdete v tématu [nasazení prostředků pomocí šablony Resource Manageru a portálu Azure](resource-group-template-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="fe780-110">To learn about deploying resources through the portal, see [Deploy resources with Resource Manager templates and Azure portal](resource-group-template-deploy-portal.md).</span></span>

<span data-ttu-id="fe780-111">Ne všechny služby v současné době podporuje portál nebo Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="fe780-111">Currently, not every service supports the portal or Resource Manager.</span></span> <span data-ttu-id="fe780-112">Pro tyto služby, budete muset použít [portálu classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="fe780-112">For those services, you need to use the [classic portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="fe780-113">Stav jednotlivých služeb, naleznete v části [Azure portálu dostupnosti grafu](https://azure.microsoft.com/features/azure-portal/availability/).</span><span class="sxs-lookup"><span data-stu-id="fe780-113">For the status of each service, see [Azure portal availability chart](https://azure.microsoft.com/features/azure-portal/availability/).</span></span>

## <a name="manage-resource-groups"></a><span data-ttu-id="fe780-114">Správa skupin prostředků</span><span class="sxs-lookup"><span data-stu-id="fe780-114">Manage resource groups</span></span>

<span data-ttu-id="fe780-115">Skupina prostředků je kontejner, který obsahuje související prostředky pro řešení s Azure.</span><span class="sxs-lookup"><span data-stu-id="fe780-115">A resource group is a container that holds related resources for an Azure solution.</span></span> <span data-ttu-id="fe780-116">Skupina prostředků může zahrnovat všechny prostředky pro řešení nebo pouze ty prostředky, které chcete spravovat jako skupinu.</span><span class="sxs-lookup"><span data-stu-id="fe780-116">The resource group can include all the resources for the solution, or only those resources that you want to manage as a group.</span></span> <span data-ttu-id="fe780-117">Na základě toho, co je pro vaši organizaci nejvhodnější, rozhodnete, jakým způsobem se mají prostředky přidělovat do skupin prostředků.</span><span class="sxs-lookup"><span data-stu-id="fe780-117">You decide how you want to allocate resources to resource groups based on what makes the most sense for your organization.</span></span> <span data-ttu-id="fe780-118">Obecně platí přidejte prostředky, které sdílejí stejný životní cyklus do stejné skupiny prostředků, můžete snadno nasadit, aktualizovat a odstranit jejich jako skupina.</span><span class="sxs-lookup"><span data-stu-id="fe780-118">Generally, add resources that share the same lifecycle to the same resource group so you can easily deploy, update, and delete them as a group.</span></span> 

<span data-ttu-id="fe780-119">Skupina prostředků ukládá metadata o prostředcích.</span><span class="sxs-lookup"><span data-stu-id="fe780-119">The resource group stores metadata about the resources.</span></span> <span data-ttu-id="fe780-120">Při zadávání umístění skupiny prostředků tedy určujete, kde se tato metadata ukládají.</span><span class="sxs-lookup"><span data-stu-id="fe780-120">Therefore, when you specify a location for the resource group, you are specifying where that metadata is stored.</span></span> <span data-ttu-id="fe780-121">Z důvodu dodržování předpisů může být nutné zajistit, aby se data ukládala v určité oblasti.</span><span class="sxs-lookup"><span data-stu-id="fe780-121">For compliance reasons, you may need to ensure that your data is stored in a particular region.</span></span>

1. <span data-ttu-id="fe780-122">Pokud chcete zobrazit všechny skupiny prostředků v rámci vašeho předplatného, vyberte **skupiny prostředků**.</span><span class="sxs-lookup"><span data-stu-id="fe780-122">To see all the resource groups in your subscription, select **Resource groups**.</span></span>
   
    ![Procházet skupiny prostředků](./media/resource-group-portal/browse-groups.png)
2. <span data-ttu-id="fe780-124">Vytvořit skupinu prostředků prázdný, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="fe780-124">To create an empty resource group, select **Add**.</span></span>
   
    ![Přidat skupinu prostředků](./media/resource-group-portal/add-resource-group.png)
3. <span data-ttu-id="fe780-126">Zadejte název a umístění pro novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="fe780-126">Provide a name and location for the new resource group.</span></span> <span data-ttu-id="fe780-127">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="fe780-127">Select **Create**.</span></span>
   
    ![Vytvořte skupinu prostředků](./media/resource-group-portal/create-empty-group.png)
4. <span data-ttu-id="fe780-129">Je nutné vybrat **aktualizovat** zobrazíte skupině nedávno vytvořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="fe780-129">You may need to select **Refresh** to see the recently created resource group.</span></span>
   
    ![Aktualizace skupiny prostředků](./media/resource-group-portal/refresh-resource-groups.png)
5. <span data-ttu-id="fe780-131">Chcete-li přizpůsobit informace zobrazené pro vaší skupiny prostředků, vyberte **sloupce**.</span><span class="sxs-lookup"><span data-stu-id="fe780-131">To customize the information displayed for your resource groups, select **Columns**.</span></span>
   
    ![přizpůsobení sloupců](./media/resource-group-portal/select-columns.png)
6. <span data-ttu-id="fe780-133">Vyberte sloupce, které chcete přidat a potom vyberte **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="fe780-133">Select the columns to add, and then select **Update**.</span></span>
   
    ![Přidání sloupců](./media/resource-group-portal/add-columns.png)
7. <span data-ttu-id="fe780-135">Další informace o nasazování prostředků do nové skupiny prostředků najdete v tématu [nasazení prostředků pomocí šablony Resource Manageru a portálu Azure](resource-group-template-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="fe780-135">To learn about deploying resources to your new resource group, see [Deploy resources with Resource Manager templates and Azure portal](resource-group-template-deploy-portal.md).</span></span>
8. <span data-ttu-id="fe780-136">Pro rychlý přístup do skupiny prostředků můžete Připnout okno na řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="fe780-136">For quick access to a resource group, you can pin the blade to your dashboard.</span></span>
   
    ![Skupina prostředků PIN kódu](./media/resource-group-portal/pin-group.png)
9. <span data-ttu-id="fe780-138">Řídicí panel zobrazuje skupina prostředků a její prostředky.</span><span class="sxs-lookup"><span data-stu-id="fe780-138">The dashboard displays the resource group and its resources.</span></span> <span data-ttu-id="fe780-139">Můžete vybrat buď skupiny prostředků, nebo kterýkoli z jeho prostředků a přejděte k položce.</span><span class="sxs-lookup"><span data-stu-id="fe780-139">You can select either the resource groups or any of its resources to navigate to the item.</span></span>
   
    ![Skupina prostředků PIN kódu](./media/resource-group-portal/show-resource-group-dashboard.png)

## <a name="tag-resources"></a><span data-ttu-id="fe780-141">Značka prostředky</span><span class="sxs-lookup"><span data-stu-id="fe780-141">Tag resources</span></span>
<span data-ttu-id="fe780-142">Značky můžete použít pro skupiny prostředků a prostředky logicky uspořádat vaše prostředky.</span><span class="sxs-lookup"><span data-stu-id="fe780-142">You can apply tags to resource groups and resources to logically organize your assets.</span></span> <span data-ttu-id="fe780-143">Informace o práci s značky najdete v tématu [použití značek k uspořádání prostředků Azure](resource-group-using-tags.md).</span><span class="sxs-lookup"><span data-stu-id="fe780-143">For information about working with tags, see [Using tags to organize your Azure resources](resource-group-using-tags.md).</span></span>

[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]

## <a name="monitor-resources"></a><span data-ttu-id="fe780-144">Sledování prostředků</span><span class="sxs-lookup"><span data-stu-id="fe780-144">Monitor resources</span></span>
<span data-ttu-id="fe780-145">Když vyberete prostředku, v okně prostředků uvede výchozí grafů a tabulek pro monitorování tohoto typu prostředku.</span><span class="sxs-lookup"><span data-stu-id="fe780-145">When you select a resource, the resource blade presents default graphs and tables for monitoring that resource type.</span></span>

1. <span data-ttu-id="fe780-146">Vyberte prostředek a upozornění **monitorování** části.</span><span class="sxs-lookup"><span data-stu-id="fe780-146">Select a resource and notice the **Monitoring** section.</span></span> <span data-ttu-id="fe780-147">Obsahuje grafy, které jsou relevantní pro typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="fe780-147">It includes graphs that are relevant to the resource type.</span></span> <span data-ttu-id="fe780-148">Následující obrázek znázorňuje výchozí sledování dat pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="fe780-148">The following image shows the default monitoring data for a storage account.</span></span>
   
    ![zobrazení monitorování](./media/resource-group-portal/show-monitoring.png)
2. <span data-ttu-id="fe780-150">Oddíl v okně můžete připnout na řídicí panel tak, že vyberete se třemi tečkami (...) výše v části.</span><span class="sxs-lookup"><span data-stu-id="fe780-150">You can pin a section of the blade to your dashboard by selecting the ellipsis (...) above the section.</span></span> <span data-ttu-id="fe780-151">Můžete také přizpůsobit velikost oddílu v okně nebo ji úplně odebrat.</span><span class="sxs-lookup"><span data-stu-id="fe780-151">You can also customize the size the section in the blade or remove it completely.</span></span> <span data-ttu-id="fe780-152">Následující obrázek ukazuje, jak připnout, upravit nebo odebrat oddíl procesoru a paměti.</span><span class="sxs-lookup"><span data-stu-id="fe780-152">The following image shows how to pin, customize, or remove the CPU and Memory section.</span></span>
   
    ![části kódu PIN](./media/resource-group-portal/pin-cpu-section.png)
3. <span data-ttu-id="fe780-154">Po Připnutí části řídicího panelu, zobrazí se souhrn na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="fe780-154">After pinning the section to the dashboard, you will see the summary on the dashboard.</span></span> <span data-ttu-id="fe780-155">A okamžitě ji vyberete přejdete na další informace o datech.</span><span class="sxs-lookup"><span data-stu-id="fe780-155">And, selecting it immediately takes you to more details about the data.</span></span>
   
    ![Zobrazení řídicí panel](./media/resource-group-portal/view-startboard.png)
4. <span data-ttu-id="fe780-157">Chcete-li zcela přizpůsobit data monitorování prostřednictvím portálu, přejděte na řídicí panel Výchozí a vyberte **novým řídicím panelem**.</span><span class="sxs-lookup"><span data-stu-id="fe780-157">To completely customize the data you monitor through the portal, navigate to your default dashboard, and select **New dashboard**.</span></span>
   
    ![řídicí panel](./media/resource-group-portal/dashboard.png)
5. <span data-ttu-id="fe780-159">Pojmenujte váš nový řídicí panel a přetáhněte dlaždice na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="fe780-159">Give your new dashboard a name and drag tiles onto the dashboard.</span></span> <span data-ttu-id="fe780-160">Dlaždice jsou filtrovány podle různé možnosti.</span><span class="sxs-lookup"><span data-stu-id="fe780-160">The tiles are filtered by different options.</span></span>
   
    ![řídicí panel](./media/resource-group-portal/create-dashboard.png)
   
     <span data-ttu-id="fe780-162">Další informace o práci s řídicích panelů najdete v tématu [sdílení řídicích panelů na portálu Azure a vytvoření](../azure-portal/azure-portal-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="fe780-162">To learn about working with dashboards, see [Creating and sharing dashboards in the Azure portal](../azure-portal/azure-portal-dashboards.md).</span></span>

## <a name="manage-resources"></a><span data-ttu-id="fe780-163">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="fe780-163">Manage resources</span></span>
<span data-ttu-id="fe780-164">V okně prostředku najdete v části Možnosti pro správu prostředku.</span><span class="sxs-lookup"><span data-stu-id="fe780-164">In the blade for a resource, you see the options for managing the resource.</span></span> <span data-ttu-id="fe780-165">Na portálu uvede možnosti správy pro tuto konkrétní typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="fe780-165">The portal presents management options for that particular resource type.</span></span> <span data-ttu-id="fe780-166">Příkazy pro správu zobrazí v horní části okna prostředků a na levé straně.</span><span class="sxs-lookup"><span data-stu-id="fe780-166">You see the management commands across the top of the resource blade and on the left side.</span></span>

![Správa prostředků](./media/resource-group-portal/manage-resources.png)

<span data-ttu-id="fe780-168">Z těchto možností můžete provádět operace, jako je například spuštění a zastavení virtuálního počítače nebo znovu konfigurovat vlastnosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fe780-168">From these options, you can perform operations such as starting and stopping a virtual machine, or reconfiguring the properties of the virtual machine.</span></span>

## <a name="move-resources"></a><span data-ttu-id="fe780-169">Přesunutí prostředků</span><span class="sxs-lookup"><span data-stu-id="fe780-169">Move resources</span></span>
<span data-ttu-id="fe780-170">Pokud potřebujete k přesunu prostředků do jiné skupině prostředků nebo jiné předplatné, přečtěte si [přesunutím prostředků do nové skupiny prostředků nebo předplatného](resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="fe780-170">If you need to move resources to another resource group or another subscription, see [Move resources to new resource group or subscription](resource-group-move-resources.md).</span></span>

## <a name="lock-resources"></a><span data-ttu-id="fe780-171">Uzamčení prostředků</span><span class="sxs-lookup"><span data-stu-id="fe780-171">Lock resources</span></span>
<span data-ttu-id="fe780-172">Je možné zamknout předplatné, skupinu prostředků nebo prostředek zabránit ostatním uživatelům ve vaší organizaci neúmyslnému odstranění nebo úprava důležitých prostředků.</span><span class="sxs-lookup"><span data-stu-id="fe780-172">You can lock a subscription, resource group, or resource to prevent other users in your organization from accidentally deleting or modifying critical resources.</span></span> <span data-ttu-id="fe780-173">Další informace najdete v tématu [Zamknutí prostředků pomocí Azure Resource Manageru](resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="fe780-173">For more information, see [Lock resources with Azure Resource Manager](resource-group-lock-resources.md).</span></span>

[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="view-your-subscription-and-costs"></a><span data-ttu-id="fe780-174">Zobrazit vaše předplatné a náklady</span><span class="sxs-lookup"><span data-stu-id="fe780-174">View your subscription and costs</span></span>
<span data-ttu-id="fe780-175">Můžete zobrazit informace o předplatného a náklady na zahrnuté pro všechny vaše prostředky.</span><span class="sxs-lookup"><span data-stu-id="fe780-175">You can view information about your subscription and the rolled-up costs for all your resources.</span></span> <span data-ttu-id="fe780-176">Vyberte **odběry** a předplatné, které chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="fe780-176">Select **Subscriptions** and the subscription you want to see.</span></span> <span data-ttu-id="fe780-177">Může mít pouze jeden odběr k výběru.</span><span class="sxs-lookup"><span data-stu-id="fe780-177">You might only have one subscription to select.</span></span>

![předplatné](./media/resource-group-portal/select-subscription.png)

<span data-ttu-id="fe780-179">V okně předplatného najdete v části pracovní tempo.</span><span class="sxs-lookup"><span data-stu-id="fe780-179">Within the subscription blade, you see a burn rate.</span></span>

![rychlost zápisu](./media/resource-group-portal/burn-rate.png)

<span data-ttu-id="fe780-181">A rozdělení nákladů podle typů prostředků.</span><span class="sxs-lookup"><span data-stu-id="fe780-181">And, a breakdown of costs by resource type.</span></span>

![náklady na prostředek](./media/resource-group-portal/cost-by-resource.png)

## <a name="export-template"></a><span data-ttu-id="fe780-183">Export šablony</span><span class="sxs-lookup"><span data-stu-id="fe780-183">Export template</span></span>
<span data-ttu-id="fe780-184">Po nastavení vaší skupiny prostředků, můžete zobrazit šablony Resource Manageru pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="fe780-184">After setting up your resource group, you may want to view the Resource Manager template for the resource group.</span></span> <span data-ttu-id="fe780-185">Export šablony nabízí dvě výhody:</span><span class="sxs-lookup"><span data-stu-id="fe780-185">Exporting the template offers two benefits:</span></span>

1. <span data-ttu-id="fe780-186">Budoucí nasazení řešení můžete snadno automatizovat, protože šablona obsahuje kompletní infrastrukturu.</span><span class="sxs-lookup"><span data-stu-id="fe780-186">You can easily automate future deployments of the solution because the template contains all the complete infrastructure.</span></span>
2. <span data-ttu-id="fe780-187">Můžete seznámit se syntaxí šablony tak, že vyhledá na JSON JavaScript Object Notation () představující řešení.</span><span class="sxs-lookup"><span data-stu-id="fe780-187">You can become familiar with template syntax by looking at the JavaScript Object Notation (JSON) that represents your solution.</span></span>

<span data-ttu-id="fe780-188">Podrobné pokyny najdete v tématu [šablony exportovat Azure Resource Manageru ze stávajících prostředků](resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="fe780-188">For step-by-step guidance, see [Export Azure Resource Manager template from existing resources](resource-manager-export-template.md).</span></span>

## <a name="delete-resource-group-or-resources"></a><span data-ttu-id="fe780-189">Odstranit skupinu prostředků nebo prostředky</span><span class="sxs-lookup"><span data-stu-id="fe780-189">Delete resource group or resources</span></span>
<span data-ttu-id="fe780-190">Odstranění skupiny prostředků se odstraní všechny prostředky, které jsou v něm obsažena.</span><span class="sxs-lookup"><span data-stu-id="fe780-190">Deleting a resource group deletes all the resources contained within it.</span></span> <span data-ttu-id="fe780-191">Můžete také odstranit jednotlivé prostředky ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="fe780-191">You can also delete individual resources within a resource group.</span></span> <span data-ttu-id="fe780-192">Chcete postupujte opatrně při odstranění skupiny prostředků vzhledem k tomu může být prostředky další skupiny zdrojů, které jsou propojeny s ho.</span><span class="sxs-lookup"><span data-stu-id="fe780-192">You want to exercise caution when you delete a resource group because there might be resources in other resource groups that are linked to it.</span></span> <span data-ttu-id="fe780-193">Správce prostředků nedojde k odstranění propojené prostředky, ale nemusí správně fungovat bez očekávané prostředků.</span><span class="sxs-lookup"><span data-stu-id="fe780-193">Resource Manager does not delete linked resources, but they may not operate correctly without the expected resources.</span></span>

![Odstranění skupiny](./media/resource-group-portal/delete-group.png)

## <a name="next-steps"></a><span data-ttu-id="fe780-195">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fe780-195">Next steps</span></span>
* <span data-ttu-id="fe780-196">Chcete-li zobrazit protokoly aktivity, najdete v části [auditovat operace s Resource Managerem](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="fe780-196">To view activity logs, see [Audit operations with Resource Manager](resource-group-audit.md).</span></span>
* <span data-ttu-id="fe780-197">Chcete-li zobrazit podrobnosti o nasazení, přečtěte si téma [zobrazit operace nasazení](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="fe780-197">To view details about a deployment, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
* <span data-ttu-id="fe780-198">Nasazení prostředků prostřednictvím portálu najdete v tématu [nasazení prostředků pomocí šablony Resource Manageru a portálu Azure](resource-group-template-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="fe780-198">To deploy resources through the portal, see [Deploy resources with Resource Manager templates and Azure portal](resource-group-template-deploy-portal.md).</span></span>
* <span data-ttu-id="fe780-199">Pokud chcete spravovat přístup k prostředkům, najdete v části [použití přiřazení rolí ke správě přístupu k prostředkům předplatného Azure](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="fe780-199">To manage access to resources, see [Use role assignments to manage access to your Azure subscription resources](../active-directory/role-based-access-control-configure.md).</span></span>
* <span data-ttu-id="fe780-200">Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="fe780-200">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

