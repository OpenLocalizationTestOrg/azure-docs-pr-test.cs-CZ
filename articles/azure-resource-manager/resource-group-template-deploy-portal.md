---
title: "Nasazení prostředků Azure pomocí portálu Azure | Microsoft Docs"
description: "Portál Azure a Azure Resource Manageru použijte k nasazení vašich prostředků."
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
ms.openlocfilehash: a4cac5834c667976b0a2d1f2748e4309a166d16a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a><span data-ttu-id="0ffd7-103">Nasazení prostředků pomocí šablon Resource Manageru a portálu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0ffd7-103">Deploy resources with Resource Manager templates and Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0ffd7-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0ffd7-104">PowerShell</span></span>](resource-group-template-deploy.md)
> * [<span data-ttu-id="0ffd7-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="0ffd7-105">Azure CLI</span></span>](resource-group-template-deploy-cli.md)
> * [<span data-ttu-id="0ffd7-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0ffd7-106">Portal</span></span>](resource-group-template-deploy-portal.md)
> * [<span data-ttu-id="0ffd7-107">REST API</span><span class="sxs-lookup"><span data-stu-id="0ffd7-107">REST API</span></span>](resource-group-template-deploy-rest.md)
> 
> 

<span data-ttu-id="0ffd7-108">Toto téma ukazuje, jak používat [portál Azure](https://portal.azure.com) s [Azure Resource Manager](resource-group-overview.md) nasazení vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-108">This topic shows how to use the [Azure portal](https://portal.azure.com) with [Azure Resource Manager](resource-group-overview.md) to deploy your Azure resources.</span></span> <span data-ttu-id="0ffd7-109">Další informace o správě prostředků najdete v tématu [Azure spravovat prostředky prostřednictvím portálu](resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="0ffd7-109">To learn about managing your resources, see [Manage Azure resources through portal](resource-group-portal.md).</span></span>

<span data-ttu-id="0ffd7-110">Ne všechny služby v současné době podporuje portál nebo Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-110">Currently, not every service supports the portal or Resource Manager.</span></span> <span data-ttu-id="0ffd7-111">Pro tyto služby, budete muset použít [portálu classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="0ffd7-111">For those services, you need to use the [classic portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="0ffd7-112">Stav jednotlivých služeb, naleznete v části [Azure portálu dostupnosti grafu](https://azure.microsoft.com/features/azure-portal/availability/).</span><span class="sxs-lookup"><span data-stu-id="0ffd7-112">For the status of each service, see [Azure portal availability chart](https://azure.microsoft.com/features/azure-portal/availability/).</span></span>

## <a name="create-resource-group"></a><span data-ttu-id="0ffd7-113">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="0ffd7-113">Create resource group</span></span>
1. <span data-ttu-id="0ffd7-114">Vytvořit skupinu prostředků prázdný, vyberte **nový** > **správy** > **skupiny prostředků**.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-114">To create an empty resource group, select **New** > **Management** > **Resource Group**.</span></span>
   
    ![Vytvoření prázdné skupině prostředků](./media/resource-group-template-deploy-portal/create-empty-group.png)
2. <span data-ttu-id="0ffd7-116">Poskytněte název a umístění a v případě potřeby vyberte předplatné.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-116">Give it a name and location, and, if necessary, select a subscription.</span></span> <span data-ttu-id="0ffd7-117">Je třeba zadat umístění pro skupinu prostředků, protože skupina prostředků ukládá metadata o prostředcích.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-117">You need to provide a location for the resource group because the resource group stores metadata about the resources.</span></span> <span data-ttu-id="0ffd7-118">Kvůli dodržování předpisů můžete určit, kam je uložen aby metadata.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-118">For compliance reasons, you may want to specify where that metadata is stored.</span></span> <span data-ttu-id="0ffd7-119">Doporučujeme obecně platí, že zadáte umístění, kde se bude nacházet většina vašich prostředků.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-119">In general, we recommend that you specify a location where most of your resources will reside.</span></span> <span data-ttu-id="0ffd7-120">Pomocí stejného umístění, můžete zjednodušit vaše šablony.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-120">Using the same location can simplify your template.</span></span>
   
    ![nastavené hodnoty skupiny](./media/resource-group-template-deploy-portal/set-group-properties.png)

## <a name="deploy-resources-from-marketplace"></a><span data-ttu-id="0ffd7-122">Nasadit prostředky z webu Marketplace</span><span class="sxs-lookup"><span data-stu-id="0ffd7-122">Deploy resources from Marketplace</span></span>
<span data-ttu-id="0ffd7-123">Po vytvoření skupiny prostředků, můžete nasadit prostředky k němu z Marketplace.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-123">After you create a resource group, you can deploy resources to it from the Marketplace.</span></span> <span data-ttu-id="0ffd7-124">Marketplace obsahuje předem definovaná řešení pro běžné scénáře.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-124">The Marketplace provides pre-defined solutions for common scenarios.</span></span>

1. <span data-ttu-id="0ffd7-125">Chcete-li spustit nasazení, vyberte **nový** a typ prostředku, kterou chcete nasadit.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-125">To start a deployment, select **New** and the type of resource you would like to deploy.</span></span> <span data-ttu-id="0ffd7-126">Poté vyhledejte konkrétní verzi prostředek, který chcete nasadit.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-126">Then, look for the particular version of the resource you would like to deploy.</span></span>
   
    ![nasazení prostředků](./media/resource-group-template-deploy-portal/deploy-resource.png)
2. <span data-ttu-id="0ffd7-128">Pokud nevidíte konkrétní řešení, které chcete nasadit, můžete vyhledat na webu Marketplace ho.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-128">If you do not see the particular solution you would like to deploy, you can search the Marketplace for it.</span></span>
   
    ![hledání marketplace](./media/resource-group-template-deploy-portal/search-resource.png)
3. <span data-ttu-id="0ffd7-130">V závislosti na typu vybraného prostředku máte kolekci relevantní vlastnosti nastavit před nasazením.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-130">Depending on the type of selected resource, you have a collection of relevant properties to set before deployment.</span></span> <span data-ttu-id="0ffd7-131">Tyto možnosti nejsou zobrazeny zde, jak se budou lišit v závislosti na typu prostředku.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-131">Those options are not shown here, as they vary based on resource type.</span></span> <span data-ttu-id="0ffd7-132">Pro všechny typy musíte vybrat cílové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-132">For all types, you must select a destination resource group.</span></span> <span data-ttu-id="0ffd7-133">Následující obrázek ukazuje, jak vytvořit webovou aplikaci a nasadit do skupiny prostředků, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-133">The following image shows how to create a web app and deploy it to the resource group you created.</span></span>
   
    ![Vytvořte skupinu prostředků](./media/resource-group-template-deploy-portal/select-existing-group.png)
   
    <span data-ttu-id="0ffd7-135">Alternativně můžete vytvořit skupinu prostředků, při nasazení vašich prostředků.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-135">Alternatively, you can decide to create a resource group when deploying your resources.</span></span> <span data-ttu-id="0ffd7-136">Vyberte **vytvořit nový** a zadejte název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-136">Select **Create new** and give the resource group a name.</span></span>
   
    ![Vytvořit novou skupinu prostředků](./media/resource-group-template-deploy-portal/select-new-group.png)
4. <span data-ttu-id="0ffd7-138">Spustí se vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-138">Your deployment begins.</span></span> <span data-ttu-id="0ffd7-139">Nasazení může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-139">The deployment could take a few minutes.</span></span> <span data-ttu-id="0ffd7-140">Pokud nasazení úspěšně proběhlo, zobrazí se upozornění.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-140">When the deployment has finished, you see a notification.</span></span>
   
    ![zobrazení oznámení](./media/resource-group-template-deploy-portal/view-notification.png)
5. <span data-ttu-id="0ffd7-142">Po nasazení vašich prostředků, můžete přidat více prostředků do skupiny prostředků pomocí **přidat** příkazu v okně skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-142">After deploying your resources, you can add more resources to the resource group by using the **Add** command on the resource group blade.</span></span>
   
    ![přidání prostředku](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a><span data-ttu-id="0ffd7-144">Nasadit prostředky z vlastní šablony</span><span class="sxs-lookup"><span data-stu-id="0ffd7-144">Deploy resources from custom template</span></span>
<span data-ttu-id="0ffd7-145">Pokud chcete provést nasazení, ale nechcete použít některou z šablon na webu Marketplace, můžete vytvořit vlastní šablonu, která definuje infrastrukturu pro vaše řešení.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-145">If you want to execute a deployment but not use any of the templates in the Marketplace, you can create a customized template that defines the infrastructure for your solution.</span></span> <span data-ttu-id="0ffd7-146">Další informace o vytváření šablon najdete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="0ffd7-146">To learn about creating templates, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>

1. <span data-ttu-id="0ffd7-147">Nasadit vlastní šablonu prostřednictvím portálu, vyberte **nový**a spusťte hledání **nasazení šablony** dokud ho můžete vybrat z možností.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-147">To deploy a customized template through the portal, select **New**, and start searching for **Template Deployment** until you can select it from the options.</span></span>
   
    ![nasazení šablony vyhledávání](./media/resource-group-template-deploy-portal/search-template.png)
2. <span data-ttu-id="0ffd7-149">Vyberte **nasazení šablony** z dostupných prostředků.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-149">Select **Template Deployment** from the available resources.</span></span>
   
    ![Vyberte šablonu nasazení](./media/resource-group-template-deploy-portal/select-template.png)
3. <span data-ttu-id="0ffd7-151">Po spuštění nasazení šablony, otevřete prázdné šablonu, kterou je k dispozici pro přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-151">After launching the template deployment, open the blank template that is available for customizing.</span></span>
   
    ![Vytvoření šablony](./media/resource-group-template-deploy-portal/show-custom-template.png)
   
    <span data-ttu-id="0ffd7-153">V editoru přidejte syntaxe JSON, který definuje prostředky, které chcete nasadit.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-153">In the editor, add the JSON syntax that defines the resources you want to deploy.</span></span> <span data-ttu-id="0ffd7-154">Vyberte **Uložit** po dokončení.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-154">Select **Save** when done.</span></span> <span data-ttu-id="0ffd7-155">Pokyny v zápisu JSON syntaxi najdete v tématu [názorný Průvodce šablonou Resource Manageru](resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="0ffd7-155">For guidance on writing the JSON syntax, see [Resource Manager template walkthrough](resource-manager-template-walkthrough.md).</span></span>
   
    ![Úprava šablony](./media/resource-group-template-deploy-portal/edit-template.png)
4. <span data-ttu-id="0ffd7-157">Nebo můžete vybrat existující šablony z [šablony Azure rychlý Start](https://azure.microsoft.com/documentation/templates/).</span><span class="sxs-lookup"><span data-stu-id="0ffd7-157">Or, you can select a pre-existing template from the [Azure quickstart templates](https://azure.microsoft.com/documentation/templates/).</span></span> <span data-ttu-id="0ffd7-158">Tyto šablony jsou přispěli komunitou.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-158">These templates are contributed by the community.</span></span> <span data-ttu-id="0ffd7-159">Pokrývaly mnoha běžným scénářům a někdo přidat šablonu, která je podobná co se pokoušíte nasadit.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-159">They cover many common scenarios, and someone may have added a template that is similar to what you are trying to deploy.</span></span> <span data-ttu-id="0ffd7-160">Můžete hledat šablony najít něco, která odpovídá vašemu scénáři.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-160">You can search the templates to find something that matches your scenario.</span></span>
   
    ![Vyberte šablonu rychlý start](./media/resource-group-template-deploy-portal/select-quickstart-template.png)
   
    <span data-ttu-id="0ffd7-162">Vybranou šablonu můžete zobrazit v editoru.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-162">You can view the selected template in the editor.</span></span>
5. <span data-ttu-id="0ffd7-163">Po zadání všech hodnot, vyberte **vytvořit** k nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-163">After providing all the other values, select **Create** to deploy the template.</span></span> 
   
    ![nasazení šablony](./media/resource-group-template-deploy-portal/create-custom-deploy.png)

## <a name="deploy-resources-from-a-template-saved-to-your-account"></a><span data-ttu-id="0ffd7-165">Nasadit prostředky ze šablony do účtu</span><span class="sxs-lookup"><span data-stu-id="0ffd7-165">Deploy resources from a template saved to your account</span></span>
<span data-ttu-id="0ffd7-166">Na portálu můžete uložit šablonu k účtu Azure a znovu ji nasaďte později.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-166">The portal enables you to save a template to your Azure account, and redeploy it later.</span></span> <span data-ttu-id="0ffd7-167">Další informace o práci s těmito uložit šablony [Začínáme se soukromými šablonami na portálu Azure](../marketplace-consumer/mytemplates-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="0ffd7-167">For more information about working with these saved templates, [Get started with private templates on the Azure portal](../marketplace-consumer/mytemplates-getstarted.md).</span></span>

1. <span data-ttu-id="0ffd7-168">Chcete-li najít uložené šablony, vyberte **Procházet** > **šablony**.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-168">To find your saved templates, select **Browse** > **Templates**.</span></span>
   
    ![Procházet šablony](./media/resource-group-template-deploy-portal/browse-templates.png)
2. <span data-ttu-id="0ffd7-170">Seznam šablon do účtu vyberte ten, který si přejete pracovat na.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-170">From the list of templates saved to your account, select the one you wish to work on.</span></span>
   
    ![uložené šablony](./media/resource-group-template-deploy-portal/saved-templates.png)
3. <span data-ttu-id="0ffd7-172">Vyberte **nasadit** k opětovnému nasazení této šablony uložené.</span><span class="sxs-lookup"><span data-stu-id="0ffd7-172">Select **Deploy** to redeploy this saved template.</span></span>
   
    ![nasazení uloženého šablony](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

## <a name="next-steps"></a><span data-ttu-id="0ffd7-174">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0ffd7-174">Next steps</span></span>
* <span data-ttu-id="0ffd7-175">Chcete-li zobrazit protokoly auditu, najdete v části [auditovat operace s Resource Managerem](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="0ffd7-175">To view audit logs, see [Audit operations with Resource Manager](resource-group-audit.md).</span></span>
* <span data-ttu-id="0ffd7-176">Chcete-li vyřešit chyby nasazení, přečtěte si téma [zobrazit operace nasazení](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="0ffd7-176">To troubleshoot deployment errors, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
* <span data-ttu-id="0ffd7-177">Pokud chcete načíst šablonu z nasazení nebo skupinu prostředků, najdete v části [šablony exportovat Azure Resource Manageru ze stávajících prostředků](resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="0ffd7-177">To retrieve a template from a deployment or resource group, see [Export Azure Resource Manager template from existing resources](resource-manager-export-template.md).</span></span>
* <span data-ttu-id="0ffd7-178">Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="0ffd7-178">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

