---
title: "aaaUse Azure portálu toodeploy prostředky Azure | Microsoft Docs"
description: "Pomocí portálu Azure a Azure Resource Manageru toodeploy vašich prostředků."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 2c98a4aa-8d9f-4a0a-b764-214dbe8ed009
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: tomfitz
ms.openlocfilehash: 5a5217f94c8dfc0c1ebd613903ea3dcbe1197bfc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a><span data-ttu-id="dce35-103">Nasazení prostředků pomocí šablon Resource Manageru a portálu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="dce35-103">Deploy resources with Resource Manager templates and Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="dce35-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="dce35-104">PowerShell</span></span>](resource-group-template-deploy.md)
> * [<span data-ttu-id="dce35-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="dce35-105">Azure CLI</span></span>](resource-group-template-deploy-cli.md)
> * [<span data-ttu-id="dce35-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="dce35-106">Portal</span></span>](resource-group-template-deploy-portal.md)
> * [<span data-ttu-id="dce35-107">REST API</span><span class="sxs-lookup"><span data-stu-id="dce35-107">REST API</span></span>](resource-group-template-deploy-rest.md)
> 
> 

<span data-ttu-id="dce35-108">Toto téma ukazuje, jak toouse hello [portál Azure](https://portal.azure.com) s [Azure Resource Manager](resource-group-overview.md) toodeploy vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="dce35-108">This topic shows how toouse hello [Azure portal](https://portal.azure.com) with [Azure Resource Manager](resource-group-overview.md) toodeploy your Azure resources.</span></span> <span data-ttu-id="dce35-109">toolearn o správu prostředků, najdete v části [Azure spravovat prostředky prostřednictvím portálu](resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="dce35-109">toolearn about managing your resources, see [Manage Azure resources through portal](resource-group-portal.md).</span></span>

<span data-ttu-id="dce35-110">Ne všechny služby v současné době podporuje hello portál nebo Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="dce35-110">Currently, not every service supports hello portal or Resource Manager.</span></span> <span data-ttu-id="dce35-111">Pro tyto služby, je třeba toouse hello [portálu classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="dce35-111">For those services, you need toouse hello [classic portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="dce35-112">Hello stav každé služby, najdete v části [Azure portálu dostupnosti grafu](https://azure.microsoft.com/features/azure-portal/availability/).</span><span class="sxs-lookup"><span data-stu-id="dce35-112">For hello status of each service, see [Azure portal availability chart](https://azure.microsoft.com/features/azure-portal/availability/).</span></span>

## <a name="create-resource-group"></a><span data-ttu-id="dce35-113">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="dce35-113">Create resource group</span></span>
1. <span data-ttu-id="dce35-114">toocreate prázdné skupině prostředků, vyberte **nový** > **správy** > **skupiny prostředků**.</span><span class="sxs-lookup"><span data-stu-id="dce35-114">toocreate an empty resource group, select **New** > **Management** > **Resource Group**.</span></span>
   
    ![Vytvoření prázdné skupině prostředků](./media/resource-group-template-deploy-portal/create-empty-group.png)
2. <span data-ttu-id="dce35-116">Poskytněte název a umístění a v případě potřeby vyberte předplatné.</span><span class="sxs-lookup"><span data-stu-id="dce35-116">Give it a name and location, and, if necessary, select a subscription.</span></span> <span data-ttu-id="dce35-117">Tooprovide umístění pro skupinu prostředků hello vyžadují, protože skupina prostředků hello ukládají metadata o hello prostředky.</span><span class="sxs-lookup"><span data-stu-id="dce35-117">You need tooprovide a location for hello resource group because hello resource group stores metadata about hello resources.</span></span> <span data-ttu-id="dce35-118">Kvůli dodržování předpisů můžete toospecify se uloží aby metadata.</span><span class="sxs-lookup"><span data-stu-id="dce35-118">For compliance reasons, you may want toospecify where that metadata is stored.</span></span> <span data-ttu-id="dce35-119">Doporučujeme obecně platí, že zadáte umístění, kde se bude nacházet většina vašich prostředků.</span><span class="sxs-lookup"><span data-stu-id="dce35-119">In general, we recommend that you specify a location where most of your resources will reside.</span></span> <span data-ttu-id="dce35-120">Pomocí hello stejné umístění můžete zjednodušit vaše šablony.</span><span class="sxs-lookup"><span data-stu-id="dce35-120">Using hello same location can simplify your template.</span></span>
   
    ![nastavené hodnoty skupiny](./media/resource-group-template-deploy-portal/set-group-properties.png)

## <a name="deploy-resources-from-marketplace"></a><span data-ttu-id="dce35-122">Nasadit prostředky z webu Marketplace</span><span class="sxs-lookup"><span data-stu-id="dce35-122">Deploy resources from Marketplace</span></span>
<span data-ttu-id="dce35-123">Po vytvoření skupiny prostředků, můžete nasadit tooit prostředky z hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="dce35-123">After you create a resource group, you can deploy resources tooit from hello Marketplace.</span></span> <span data-ttu-id="dce35-124">Hello Marketplace obsahuje předem definovaná řešení pro běžné scénáře.</span><span class="sxs-lookup"><span data-stu-id="dce35-124">hello Marketplace provides pre-defined solutions for common scenarios.</span></span>

1. <span data-ttu-id="dce35-125">toostart nasazení, vyberte **nový** a hello typu prostředku byste chtěli toodeploy.</span><span class="sxs-lookup"><span data-stu-id="dce35-125">toostart a deployment, select **New** and hello type of resource you would like toodeploy.</span></span> <span data-ttu-id="dce35-126">Pak, hledejte pro konkrétní verzi hello prostředků hello chcete toodeploy.</span><span class="sxs-lookup"><span data-stu-id="dce35-126">Then, look for hello particular version of hello resource you would like toodeploy.</span></span>
   
    ![nasazení prostředků](./media/resource-group-template-deploy-portal/deploy-resource.png)
2. <span data-ttu-id="dce35-128">Pokud nevidíte konkrétní řešení hello chcete toodeploy, můžete vyhledat hello Marketplace ho.</span><span class="sxs-lookup"><span data-stu-id="dce35-128">If you do not see hello particular solution you would like toodeploy, you can search hello Marketplace for it.</span></span>
   
    ![hledání marketplace](./media/resource-group-template-deploy-portal/search-resource.png)
3. <span data-ttu-id="dce35-130">V závislosti na typu hello vybraného prostředku máte kolekci relevantní vlastnosti tooset před nasazením.</span><span class="sxs-lookup"><span data-stu-id="dce35-130">Depending on hello type of selected resource, you have a collection of relevant properties tooset before deployment.</span></span> <span data-ttu-id="dce35-131">Tyto možnosti nejsou zobrazeny zde, jak se budou lišit v závislosti na typu prostředku.</span><span class="sxs-lookup"><span data-stu-id="dce35-131">Those options are not shown here, as they vary based on resource type.</span></span> <span data-ttu-id="dce35-132">Pro všechny typy musíte vybrat cílové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="dce35-132">For all types, you must select a destination resource group.</span></span> <span data-ttu-id="dce35-133">Hello následující obrázek znázorňuje, jak toocreate webové aplikace a nasadit ji toohello prostředků skupinu, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="dce35-133">hello following image shows how toocreate a web app and deploy it toohello resource group you created.</span></span>
   
    ![Vytvořte skupinu prostředků](./media/resource-group-template-deploy-portal/select-existing-group.png)
   
    <span data-ttu-id="dce35-135">Alternativně můžete rozhodnout toocreate skupinu prostředků, při nasazení vašich prostředků.</span><span class="sxs-lookup"><span data-stu-id="dce35-135">Alternatively, you can decide toocreate a resource group when deploying your resources.</span></span> <span data-ttu-id="dce35-136">Vyberte **vytvořit nový** a zadejte název skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="dce35-136">Select **Create new** and give hello resource group a name.</span></span>
   
    ![Vytvořit novou skupinu prostředků](./media/resource-group-template-deploy-portal/select-new-group.png)
4. <span data-ttu-id="dce35-138">Spustí se vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="dce35-138">Your deployment begins.</span></span> <span data-ttu-id="dce35-139">Hello nasazení může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="dce35-139">hello deployment could take a few minutes.</span></span> <span data-ttu-id="dce35-140">Po dokončení nasazení hello zobrazí oznámení.</span><span class="sxs-lookup"><span data-stu-id="dce35-140">When hello deployment has finished, you see a notification.</span></span>
   
    ![zobrazení oznámení](./media/resource-group-template-deploy-portal/view-notification.png)
5. <span data-ttu-id="dce35-142">Po nasazení vašich prostředků, můžete přidat další skupiny zdrojů toohello prostředky pomocí hello **přidat** příkazu v okně skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="dce35-142">After deploying your resources, you can add more resources toohello resource group by using hello **Add** command on hello resource group blade.</span></span>
   
    ![přidání prostředku](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a><span data-ttu-id="dce35-144">Nasadit prostředky z vlastní šablony</span><span class="sxs-lookup"><span data-stu-id="dce35-144">Deploy resources from custom template</span></span>
<span data-ttu-id="dce35-145">Pokud chcete tooexecute nasazení, ale není použít některou z šablon hello v hello Marketplace, můžete vytvořit vlastní šablonu, která definuje hello infrastrukturu pro vaše řešení.</span><span class="sxs-lookup"><span data-stu-id="dce35-145">If you want tooexecute a deployment but not use any of hello templates in hello Marketplace, you can create a customized template that defines hello infrastructure for your solution.</span></span> <span data-ttu-id="dce35-146">toolearn o vytváření šablon, najdete v části [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="dce35-146">toolearn about creating templates, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>

1. <span data-ttu-id="dce35-147">Vyberte toodeploy vlastní šablonu prostřednictvím portálu hello **nový**a spusťte hledání **nasazení šablony** dokud ho můžete vybrat z možností hello.</span><span class="sxs-lookup"><span data-stu-id="dce35-147">toodeploy a customized template through hello portal, select **New**, and start searching for **Template Deployment** until you can select it from hello options.</span></span>
   
    ![nasazení šablony vyhledávání](./media/resource-group-template-deploy-portal/search-template.png)
2. <span data-ttu-id="dce35-149">Vyberte **nasazení šablony** z hello dostupné prostředky.</span><span class="sxs-lookup"><span data-stu-id="dce35-149">Select **Template Deployment** from hello available resources.</span></span>
   
    ![Vyberte šablonu nasazení](./media/resource-group-template-deploy-portal/select-template.png)
3. <span data-ttu-id="dce35-151">Po spuštění nasazení šablony hello, otevřete hello prázdné šablonu, která je k dispozici pro přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="dce35-151">After launching hello template deployment, open hello blank template that is available for customizing.</span></span>
   
    ![Vytvoření šablony](./media/resource-group-template-deploy-portal/show-custom-template.png)
   
    <span data-ttu-id="dce35-153">V editoru hello přidáte hello syntaxe JSON, který definuje prostředky hello chcete toodeploy.</span><span class="sxs-lookup"><span data-stu-id="dce35-153">In hello editor, add hello JSON syntax that defines hello resources you want toodeploy.</span></span> <span data-ttu-id="dce35-154">Vyberte **Uložit** po dokončení.</span><span class="sxs-lookup"><span data-stu-id="dce35-154">Select **Save** when done.</span></span> <span data-ttu-id="dce35-155">Pokyny v zápisu JSON syntaxe hello najdete v tématu [názorný Průvodce šablonou Resource Manageru](resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="dce35-155">For guidance on writing hello JSON syntax, see [Resource Manager template walkthrough](resource-manager-template-walkthrough.md).</span></span>
   
    ![Úprava šablony](./media/resource-group-template-deploy-portal/edit-template.png)
4. <span data-ttu-id="dce35-157">Nebo můžete vybrat existující šablonu z hello [šablony Azure rychlý Start](https://azure.microsoft.com/documentation/templates/).</span><span class="sxs-lookup"><span data-stu-id="dce35-157">Or, you can select a pre-existing template from hello [Azure quickstart templates](https://azure.microsoft.com/documentation/templates/).</span></span> <span data-ttu-id="dce35-158">Tyto šablony jsou přispěli hello komunity.</span><span class="sxs-lookup"><span data-stu-id="dce35-158">These templates are contributed by hello community.</span></span> <span data-ttu-id="dce35-159">Pokrývaly mnoha běžným scénářům a někdo přidat šablonu, která je podobné toowhat se pokoušíte toodeploy.</span><span class="sxs-lookup"><span data-stu-id="dce35-159">They cover many common scenarios, and someone may have added a template that is similar toowhat you are trying toodeploy.</span></span> <span data-ttu-id="dce35-160">Můžete hledat hello šablony toofind něco, která odpovídá vašemu scénáři.</span><span class="sxs-lookup"><span data-stu-id="dce35-160">You can search hello templates toofind something that matches your scenario.</span></span>
   
    ![Vyberte šablonu rychlý start](./media/resource-group-template-deploy-portal/select-quickstart-template.png)
   
    <span data-ttu-id="dce35-162">Vybranou šablonu hello můžete zobrazit v editoru hello.</span><span class="sxs-lookup"><span data-stu-id="dce35-162">You can view hello selected template in hello editor.</span></span>
5. <span data-ttu-id="dce35-163">Po zadání všech hello jiné hodnoty, vyberte **vytvořit** toodeploy hello šablony.</span><span class="sxs-lookup"><span data-stu-id="dce35-163">After providing all hello other values, select **Create** toodeploy hello template.</span></span> 
   
    ![nasazení šablony](./media/resource-group-template-deploy-portal/create-custom-deploy.png)

## <a name="deploy-resources-from-a-template-saved-tooyour-account"></a><span data-ttu-id="dce35-165">Nasadit prostředky ze šablony uložit tooyour účtu</span><span class="sxs-lookup"><span data-stu-id="dce35-165">Deploy resources from a template saved tooyour account</span></span>
<span data-ttu-id="dce35-166">portál Hello umožňuje toosave šablony tooyour účet Azure a znovu nasaďte později.</span><span class="sxs-lookup"><span data-stu-id="dce35-166">hello portal enables you toosave a template tooyour Azure account, and redeploy it later.</span></span> <span data-ttu-id="dce35-167">Další informace o práci s těmito uložit šablony [Začínáme se soukromými šablonami na portálu Azure hello](../marketplace-consumer/mytemplates-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="dce35-167">For more information about working with these saved templates, [Get started with private templates on hello Azure portal](../marketplace-consumer/mytemplates-getstarted.md).</span></span>

1. <span data-ttu-id="dce35-168">Vyberte uložené šablony toofind **Procházet** > **šablony**.</span><span class="sxs-lookup"><span data-stu-id="dce35-168">toofind your saved templates, select **Browse** > **Templates**.</span></span>
   
    ![Procházet šablony](./media/resource-group-template-deploy-portal/browse-templates.png)
2. <span data-ttu-id="dce35-170">Hello seznam šablon uložit tooyour účtu vyberte, kterou chcete toowork na hello.</span><span class="sxs-lookup"><span data-stu-id="dce35-170">From hello list of templates saved tooyour account, select hello one you wish toowork on.</span></span>
   
    ![uložené šablony](./media/resource-group-template-deploy-portal/saved-templates.png)
3. <span data-ttu-id="dce35-172">Vyberte **nasadit** tooredeploy to uložit šablonu.</span><span class="sxs-lookup"><span data-stu-id="dce35-172">Select **Deploy** tooredeploy this saved template.</span></span>
   
    ![nasazení uloženého šablony](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

## <a name="next-steps"></a><span data-ttu-id="dce35-174">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dce35-174">Next steps</span></span>
* <span data-ttu-id="dce35-175">protokoly auditu tooview, najdete v části [auditovat operace s Resource Managerem](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="dce35-175">tooview audit logs, see [Audit operations with Resource Manager](resource-group-audit.md).</span></span>
* <span data-ttu-id="dce35-176">chyby nasazení tootroubleshoot, najdete v části [zobrazit operace nasazení](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="dce35-176">tootroubleshoot deployment errors, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
* <span data-ttu-id="dce35-177">tooretrieve šablonu z nasazení nebo skupinu prostředků, najdete v části [šablony exportovat Azure Resource Manageru ze stávajících prostředků](resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="dce35-177">tooretrieve a template from a deployment or resource group, see [Export Azure Resource Manager template from existing resources](resource-manager-export-template.md).</span></span>
* <span data-ttu-id="dce35-178">Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="dce35-178">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

