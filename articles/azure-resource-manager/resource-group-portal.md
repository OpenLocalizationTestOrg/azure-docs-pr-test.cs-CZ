---
title: "aaaUse Azure portálu toomanage prostředky Azure | Microsoft Docs"
description: "Pomocí portálu Azure a Azure Resource Manageru toomanage vašich prostředků. Ukazuje, jak toowork s prostředky toomonitor řídicí panely."
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
ms.openlocfilehash: 0c89a197a31c5b6dd03ba457cb4d1fdf9f6d00f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-resources-through-portal"></a><span data-ttu-id="fc846-104">Spravovat prostředky prostřednictvím portálu Azure</span><span class="sxs-lookup"><span data-stu-id="fc846-104">Manage Azure resources through portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fc846-105">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="fc846-105">Azure PowerShell</span></span>](powershell-azure-resource-manager.md)
> * [<span data-ttu-id="fc846-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="fc846-106">Azure CLI</span></span>](xplat-cli-azure-resource-manager.md)
> * [<span data-ttu-id="fc846-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="fc846-107">Portal</span></span>](resource-group-portal.md) 
> * [<span data-ttu-id="fc846-108">REST API</span><span class="sxs-lookup"><span data-stu-id="fc846-108">REST API</span></span>](resource-manager-rest-api.md)
> 
> 

<span data-ttu-id="fc846-109">Toto téma ukazuje, jak toouse hello [portál Azure](https://portal.azure.com) s [Azure Resource Manager](resource-group-overview.md) toomanage vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="fc846-109">This topic shows how toouse hello [Azure portal](https://portal.azure.com) with [Azure Resource Manager](resource-group-overview.md) toomanage your Azure resources.</span></span> <span data-ttu-id="fc846-110">v tématu toolearn o nasazení prostředků prostřednictvím portálu hello [nasazení prostředků pomocí šablony Resource Manageru a portálu Azure](resource-group-template-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="fc846-110">toolearn about deploying resources through hello portal, see [Deploy resources with Resource Manager templates and Azure portal](resource-group-template-deploy-portal.md).</span></span>

<span data-ttu-id="fc846-111">Ne všechny služby v současné době podporuje hello portál nebo Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="fc846-111">Currently, not every service supports hello portal or Resource Manager.</span></span> <span data-ttu-id="fc846-112">Pro tyto služby, je třeba toouse hello [portálu classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="fc846-112">For those services, you need toouse hello [classic portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="fc846-113">Hello stav každé služby, najdete v části [Azure portálu dostupnosti grafu](https://azure.microsoft.com/features/azure-portal/availability/).</span><span class="sxs-lookup"><span data-stu-id="fc846-113">For hello status of each service, see [Azure portal availability chart](https://azure.microsoft.com/features/azure-portal/availability/).</span></span>

## <a name="manage-resource-groups"></a><span data-ttu-id="fc846-114">Správa skupin prostředků</span><span class="sxs-lookup"><span data-stu-id="fc846-114">Manage resource groups</span></span>

<span data-ttu-id="fc846-115">Skupina prostředků je kontejner, který obsahuje související prostředky pro řešení s Azure.</span><span class="sxs-lookup"><span data-stu-id="fc846-115">A resource group is a container that holds related resources for an Azure solution.</span></span> <span data-ttu-id="fc846-116">Skupina prostředků Hello může zahrnovat všechny hello prostředky pro řešení hello nebo jenom prostředky, které chcete toomanage jako skupina.</span><span class="sxs-lookup"><span data-stu-id="fc846-116">hello resource group can include all hello resources for hello solution, or only those resources that you want toomanage as a group.</span></span> <span data-ttu-id="fc846-117">Můžete určit, jak mají prostředky tooallocate tooresource skupin podle díky hello nejvhodnější pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="fc846-117">You decide how you want tooallocate resources tooresource groups based on what makes hello most sense for your organization.</span></span> <span data-ttu-id="fc846-118">Obecně platí, přidejte prostředky, které sdílejí hello stejný životní cyklus toohello skupina stejné prostředků, můžete snadno nasadit, aktualizovat a odstranit jejich jako skupina.</span><span class="sxs-lookup"><span data-stu-id="fc846-118">Generally, add resources that share hello same lifecycle toohello same resource group so you can easily deploy, update, and delete them as a group.</span></span> 

<span data-ttu-id="fc846-119">Skupina prostředků Hello ukládají metadata o hello prostředky.</span><span class="sxs-lookup"><span data-stu-id="fc846-119">hello resource group stores metadata about hello resources.</span></span> <span data-ttu-id="fc846-120">Proto když zadáte umístění pro skupinu prostředků hello, určíte se uloží aby metadata.</span><span class="sxs-lookup"><span data-stu-id="fc846-120">Therefore, when you specify a location for hello resource group, you are specifying where that metadata is stored.</span></span> <span data-ttu-id="fc846-121">Pro dodržování předpisů, pravděpodobně bude třeba tooensure data uložená v určité oblasti.</span><span class="sxs-lookup"><span data-stu-id="fc846-121">For compliance reasons, you may need tooensure that your data is stored in a particular region.</span></span>

1. <span data-ttu-id="fc846-122">Vyberte všechny skupiny zdrojů hello ve vašem předplatném toosee **skupiny prostředků**.</span><span class="sxs-lookup"><span data-stu-id="fc846-122">toosee all hello resource groups in your subscription, select **Resource groups**.</span></span>
   
    ![Procházet skupiny prostředků](./media/resource-group-portal/browse-groups.png)
2. <span data-ttu-id="fc846-124">Vyberte skupinu prostředků prázdný toocreate **přidat**.</span><span class="sxs-lookup"><span data-stu-id="fc846-124">toocreate an empty resource group, select **Add**.</span></span>
   
    ![Přidat skupinu prostředků](./media/resource-group-portal/add-resource-group.png)
3. <span data-ttu-id="fc846-126">Zadejte název a umístění pro novou skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="fc846-126">Provide a name and location for hello new resource group.</span></span> <span data-ttu-id="fc846-127">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="fc846-127">Select **Create**.</span></span>
   
    ![Vytvořte skupinu prostředků](./media/resource-group-portal/create-empty-group.png)
4. <span data-ttu-id="fc846-129">Může být nutné tooselect **aktualizovat** toosee hello nedávno vytvořen prostředek skupiny.</span><span class="sxs-lookup"><span data-stu-id="fc846-129">You may need tooselect **Refresh** toosee hello recently created resource group.</span></span>
   
    ![Aktualizace skupiny prostředků](./media/resource-group-portal/refresh-resource-groups.png)
5. <span data-ttu-id="fc846-131">Vyberte toocustomize hello informace zobrazené pro vaší skupiny prostředků **sloupce**.</span><span class="sxs-lookup"><span data-stu-id="fc846-131">toocustomize hello information displayed for your resource groups, select **Columns**.</span></span>
   
    ![přizpůsobení sloupců](./media/resource-group-portal/select-columns.png)
6. <span data-ttu-id="fc846-133">Vyberte sloupce tooadd hello a pak vyberte **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="fc846-133">Select hello columns tooadd, and then select **Update**.</span></span>
   
    ![Přidání sloupců](./media/resource-group-portal/add-columns.png)
7. <span data-ttu-id="fc846-135">toolearn o nasazení prostředků tooyour novou skupinu prostředků, najdete v části [nasazení prostředků pomocí šablony Resource Manageru a portálu Azure](resource-group-template-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="fc846-135">toolearn about deploying resources tooyour new resource group, see [Deploy resources with Resource Manager templates and Azure portal](resource-group-template-deploy-portal.md).</span></span>
8. <span data-ttu-id="fc846-136">Pro skupinu prostředků tooa rychlý přístup budete moct připnout hello okno tooyour řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="fc846-136">For quick access tooa resource group, you can pin hello blade tooyour dashboard.</span></span>
   
    ![Skupina prostředků PIN kódu](./media/resource-group-portal/pin-group.png)
9. <span data-ttu-id="fc846-138">řídicí panel Hello zobrazí hello skupinu prostředků a její prostředky.</span><span class="sxs-lookup"><span data-stu-id="fc846-138">hello dashboard displays hello resource group and its resources.</span></span> <span data-ttu-id="fc846-139">Můžete vybrat skupiny prostředků hello nebo všechny jeho prostředky toonavigate toohello položky.</span><span class="sxs-lookup"><span data-stu-id="fc846-139">You can select either hello resource groups or any of its resources toonavigate toohello item.</span></span>
   
    ![Skupina prostředků PIN kódu](./media/resource-group-portal/show-resource-group-dashboard.png)

## <a name="tag-resources"></a><span data-ttu-id="fc846-141">Značka prostředky</span><span class="sxs-lookup"><span data-stu-id="fc846-141">Tag resources</span></span>
<span data-ttu-id="fc846-142">Můžete použít skupiny tooresource značky a prostředky toologically uspořádání vaše prostředky.</span><span class="sxs-lookup"><span data-stu-id="fc846-142">You can apply tags tooresource groups and resources toologically organize your assets.</span></span> <span data-ttu-id="fc846-143">Informace o práci s značky najdete v tématu [pomocí značky tooorganize vašich prostředků Azure](resource-group-using-tags.md).</span><span class="sxs-lookup"><span data-stu-id="fc846-143">For information about working with tags, see [Using tags tooorganize your Azure resources](resource-group-using-tags.md).</span></span>

[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]

## <a name="monitor-resources"></a><span data-ttu-id="fc846-144">Sledování prostředků</span><span class="sxs-lookup"><span data-stu-id="fc846-144">Monitor resources</span></span>
<span data-ttu-id="fc846-145">Když vyberete prostředek, uvede okna prostředků hello výchozí grafů a tabulek pro monitorování tohoto typu prostředku.</span><span class="sxs-lookup"><span data-stu-id="fc846-145">When you select a resource, hello resource blade presents default graphs and tables for monitoring that resource type.</span></span>

1. <span data-ttu-id="fc846-146">Vyberte prostředek a Všimněte si, hello **monitorování** části.</span><span class="sxs-lookup"><span data-stu-id="fc846-146">Select a resource and notice hello **Monitoring** section.</span></span> <span data-ttu-id="fc846-147">Obsahuje grafy, které jsou relevantní toohello typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="fc846-147">It includes graphs that are relevant toohello resource type.</span></span> <span data-ttu-id="fc846-148">Hello následující obrázek znázorňuje hello výchozí údaje pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="fc846-148">hello following image shows hello default monitoring data for a storage account.</span></span>
   
    ![zobrazení monitorování](./media/resource-group-portal/show-monitoring.png)
2. <span data-ttu-id="fc846-150">Oddíl hello okno tooyour řídicí panel můžete připnout výběrem hello tečkami (...) nad sekcí hello.</span><span class="sxs-lookup"><span data-stu-id="fc846-150">You can pin a section of hello blade tooyour dashboard by selecting hello ellipsis (...) above hello section.</span></span> <span data-ttu-id="fc846-151">Můžete také přizpůsobit hello velikost hello oddíl v okně hello nebo ji úplně odebrat.</span><span class="sxs-lookup"><span data-stu-id="fc846-151">You can also customize hello size hello section in hello blade or remove it completely.</span></span> <span data-ttu-id="fc846-152">Hello následující obrázek ukazuje, jak přizpůsobit toopin, nebo odeberte hello procesoru a paměti části.</span><span class="sxs-lookup"><span data-stu-id="fc846-152">hello following image shows how toopin, customize, or remove hello CPU and Memory section.</span></span>
   
    ![části kódu PIN](./media/resource-group-portal/pin-cpu-section.png)
3. <span data-ttu-id="fc846-154">Po Připnutí hello části toohello řídicí panel, uvidíte na řídicím panelu hello hello souhrnu.</span><span class="sxs-lookup"><span data-stu-id="fc846-154">After pinning hello section toohello dashboard, you will see hello summary on hello dashboard.</span></span> <span data-ttu-id="fc846-155">A okamžitě ji vyberete trvá toomore údaje o datech hello.</span><span class="sxs-lookup"><span data-stu-id="fc846-155">And, selecting it immediately takes you toomore details about hello data.</span></span>
   
    ![Zobrazení řídicí panel](./media/resource-group-portal/view-startboard.png)
4. <span data-ttu-id="fc846-157">toocompletely přizpůsobit hello data monitorování prostřednictvím hello portálu, přejděte tooyour výchozí řídicí panely a vyberte **novým řídicím panelem**.</span><span class="sxs-lookup"><span data-stu-id="fc846-157">toocompletely customize hello data you monitor through hello portal, navigate tooyour default dashboard, and select **New dashboard**.</span></span>
   
    ![řídicí panel](./media/resource-group-portal/dashboard.png)
5. <span data-ttu-id="fc846-159">Pojmenujte váš nový řídicí panel a přetáhněte dlaždice na řídicím panelu hello.</span><span class="sxs-lookup"><span data-stu-id="fc846-159">Give your new dashboard a name and drag tiles onto hello dashboard.</span></span> <span data-ttu-id="fc846-160">dlaždice Hello jsou filtrovány podle různé možnosti.</span><span class="sxs-lookup"><span data-stu-id="fc846-160">hello tiles are filtered by different options.</span></span>
   
    ![řídicí panel](./media/resource-group-portal/create-dashboard.png)
   
     <span data-ttu-id="fc846-162">toolearn o práci s řídicí panely, najdete v části [vytvoření a sdílení řídicích panelů v hello portál Azure](../azure-portal/azure-portal-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="fc846-162">toolearn about working with dashboards, see [Creating and sharing dashboards in hello Azure portal](../azure-portal/azure-portal-dashboards.md).</span></span>

## <a name="manage-resources"></a><span data-ttu-id="fc846-163">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="fc846-163">Manage resources</span></span>
<span data-ttu-id="fc846-164">V okně hello prostředku najdete v části hello možností pro správu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="fc846-164">In hello blade for a resource, you see hello options for managing hello resource.</span></span> <span data-ttu-id="fc846-165">portál Hello zobrazí možnosti správy pro tuto konkrétní typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="fc846-165">hello portal presents management options for that particular resource type.</span></span> <span data-ttu-id="fc846-166">Zobrazí příkazy pro správu hello napříč hello horní části okna prostředků hello a na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="fc846-166">You see hello management commands across hello top of hello resource blade and on hello left side.</span></span>

![Správa prostředků](./media/resource-group-portal/manage-resources.png)

<span data-ttu-id="fc846-168">Z těchto možností můžete provádět operace, jako je například spuštění a zastavení virtuálního počítače nebo znovu konfigurovat vlastnosti hello hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fc846-168">From these options, you can perform operations such as starting and stopping a virtual machine, or reconfiguring hello properties of hello virtual machine.</span></span>

## <a name="move-resources"></a><span data-ttu-id="fc846-169">Přesunutí prostředků</span><span class="sxs-lookup"><span data-stu-id="fc846-169">Move resources</span></span>
<span data-ttu-id="fc846-170">Pokud potřebujete skupinu prostředků tooanother toomove prostředky nebo jiné předplatné, přečtěte si [přesunout skupiny prostředků toonew prostředků nebo předplatného](resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="fc846-170">If you need toomove resources tooanother resource group or another subscription, see [Move resources toonew resource group or subscription](resource-group-move-resources.md).</span></span>

## <a name="lock-resources"></a><span data-ttu-id="fc846-171">Uzamčení prostředků</span><span class="sxs-lookup"><span data-stu-id="fc846-171">Lock resources</span></span>
<span data-ttu-id="fc846-172">Můžete uzamknout předplatné, skupinu prostředků nebo prostředek tooprevent ostatní uživatelé ve vaší organizaci neúmyslnému odstranění nebo úprava důležitých prostředků.</span><span class="sxs-lookup"><span data-stu-id="fc846-172">You can lock a subscription, resource group, or resource tooprevent other users in your organization from accidentally deleting or modifying critical resources.</span></span> <span data-ttu-id="fc846-173">Další informace najdete v tématu [Zamknutí prostředků pomocí Azure Resource Manageru](resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="fc846-173">For more information, see [Lock resources with Azure Resource Manager](resource-group-lock-resources.md).</span></span>

[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="view-your-subscription-and-costs"></a><span data-ttu-id="fc846-174">Zobrazit vaše předplatné a náklady</span><span class="sxs-lookup"><span data-stu-id="fc846-174">View your subscription and costs</span></span>
<span data-ttu-id="fc846-175">Pro všechny vaše prostředky můžete zobrazit informace o předplatném a hello zahrnuté náklady.</span><span class="sxs-lookup"><span data-stu-id="fc846-175">You can view information about your subscription and hello rolled-up costs for all your resources.</span></span> <span data-ttu-id="fc846-176">Vyberte **odběry** a chcete, aby toosee předplatné hello.</span><span class="sxs-lookup"><span data-stu-id="fc846-176">Select **Subscriptions** and hello subscription you want toosee.</span></span> <span data-ttu-id="fc846-177">Může mít pouze jeden tooselect předplatné.</span><span class="sxs-lookup"><span data-stu-id="fc846-177">You might only have one subscription tooselect.</span></span>

![předplatné](./media/resource-group-portal/select-subscription.png)

<span data-ttu-id="fc846-179">V okně hello předplatné najdete v části pracovní tempo.</span><span class="sxs-lookup"><span data-stu-id="fc846-179">Within hello subscription blade, you see a burn rate.</span></span>

![rychlost zápisu](./media/resource-group-portal/burn-rate.png)

<span data-ttu-id="fc846-181">A rozdělení nákladů podle typů prostředků.</span><span class="sxs-lookup"><span data-stu-id="fc846-181">And, a breakdown of costs by resource type.</span></span>

![náklady na prostředek](./media/resource-group-portal/cost-by-resource.png)

## <a name="export-template"></a><span data-ttu-id="fc846-183">Export šablony</span><span class="sxs-lookup"><span data-stu-id="fc846-183">Export template</span></span>
<span data-ttu-id="fc846-184">Po nastavení vaší skupiny prostředků, může být vhodné šablony Resource Manageru hello tooview pro skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="fc846-184">After setting up your resource group, you may want tooview hello Resource Manager template for hello resource group.</span></span> <span data-ttu-id="fc846-185">Export šablony hello nabízí dvě výhody:</span><span class="sxs-lookup"><span data-stu-id="fc846-185">Exporting hello template offers two benefits:</span></span>

1. <span data-ttu-id="fc846-186">Snadno můžete automatizovat budoucí nasazení řešení hello protože hello šablona obsahuje všechny hello kompletní infrastrukturu.</span><span class="sxs-lookup"><span data-stu-id="fc846-186">You can easily automate future deployments of hello solution because hello template contains all hello complete infrastructure.</span></span>
2. <span data-ttu-id="fc846-187">Můžete seznámit se syntaxí šablony prohlížením hello JSON JavaScript Object Notation () představující řešení.</span><span class="sxs-lookup"><span data-stu-id="fc846-187">You can become familiar with template syntax by looking at hello JavaScript Object Notation (JSON) that represents your solution.</span></span>

<span data-ttu-id="fc846-188">Podrobné pokyny najdete v tématu [šablony exportovat Azure Resource Manageru ze stávajících prostředků](resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="fc846-188">For step-by-step guidance, see [Export Azure Resource Manager template from existing resources](resource-manager-export-template.md).</span></span>

## <a name="delete-resource-group-or-resources"></a><span data-ttu-id="fc846-189">Odstranit skupinu prostředků nebo prostředky</span><span class="sxs-lookup"><span data-stu-id="fc846-189">Delete resource group or resources</span></span>
<span data-ttu-id="fc846-190">Odstranění skupiny prostředků se odstraní všechny hello prostředky, které jsou v něm obsažena.</span><span class="sxs-lookup"><span data-stu-id="fc846-190">Deleting a resource group deletes all hello resources contained within it.</span></span> <span data-ttu-id="fc846-191">Můžete také odstranit jednotlivé prostředky ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="fc846-191">You can also delete individual resources within a resource group.</span></span> <span data-ttu-id="fc846-192">Pokud odstraníte skupinu prostředků, protože může být prostředky další skupiny zdrojů, které jsou propojené tooit chcete tooexercise upozornění.</span><span class="sxs-lookup"><span data-stu-id="fc846-192">You want tooexercise caution when you delete a resource group because there might be resources in other resource groups that are linked tooit.</span></span> <span data-ttu-id="fc846-193">Správce prostředků nedojde k odstranění propojené prostředky, ale nemusí správně fungovat bez hello očekává prostředků.</span><span class="sxs-lookup"><span data-stu-id="fc846-193">Resource Manager does not delete linked resources, but they may not operate correctly without hello expected resources.</span></span>

![Odstranění skupiny](./media/resource-group-portal/delete-group.png)

## <a name="next-steps"></a><span data-ttu-id="fc846-195">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fc846-195">Next steps</span></span>
* <span data-ttu-id="fc846-196">Protokoly aktivity tooview, najdete v části [auditovat operace s Resource Managerem](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="fc846-196">tooview activity logs, see [Audit operations with Resource Manager](resource-group-audit.md).</span></span>
* <span data-ttu-id="fc846-197">tooview podrobnosti o nasazení, najdete v tématu [zobrazit operace nasazení](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="fc846-197">tooview details about a deployment, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
* <span data-ttu-id="fc846-198">toodeploy prostředky prostřednictvím portálu hello, najdete v části [nasazení prostředků pomocí šablony Resource Manageru a portálu Azure](resource-group-template-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="fc846-198">toodeploy resources through hello portal, see [Deploy resources with Resource Manager templates and Azure portal](resource-group-template-deploy-portal.md).</span></span>
* <span data-ttu-id="fc846-199">toomanage tooresources přístup, najdete v části [používat roli přiřazení toomanage přístup tooyour předplatného Azure prostředky](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="fc846-199">toomanage access tooresources, see [Use role assignments toomanage access tooyour Azure subscription resources](../active-directory/role-based-access-control-configure.md).</span></span>
* <span data-ttu-id="fc846-200">Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="fc846-200">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

