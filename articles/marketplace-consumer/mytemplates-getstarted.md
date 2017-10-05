---
title: "Začínáme se soukromými šablonami | Dokumentace Microsoftu"
description: "Přidávejte, spravujte a sdílejte svoje soukromé šablony pomocí portálu Azure, rozhraní příkazového řádku Azure nebo PowerShellu."
services: marketplace-customer
documentationcenter: 
author: VybavaRamadoss
manager: asimm
editor: 
tags: marketplace, azure-resource-manager
keywords: 
ms.assetid: 6ec20778-b578-4885-acb5-104b0e51ea1a
ms.service: marketplace
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: vybavar
ms.openlocfilehash: 01657619cbe579c6818a790cc3ab95a33936a565
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-private-templates-on-the-azure-portal"></a><span data-ttu-id="28f43-103">Začínáme se soukromými šablonami na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="28f43-103">Get started with private Templates on the Azure Portal</span></span>
<span data-ttu-id="28f43-104">Šablona [Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md) je deklarativní šablona, která slouží k definování nasazení.</span><span class="sxs-lookup"><span data-stu-id="28f43-104">An [Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) template is a declarative template used to define your deployment.</span></span> <span data-ttu-id="28f43-105">Můžete definovat, které prostředky se mají pro řešení nasadit, a určit parametry a proměnné, které vám umožní zadat hodnoty pro různá prostředí.</span><span class="sxs-lookup"><span data-stu-id="28f43-105">You can define the resources to deploy for a solution, and specify parameters and variables that enable you to input values for different environments.</span></span> <span data-ttu-id="28f43-106">Šablona se skládá z JSON a z výrazů, které můžete použít k vytvoření hodnot pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="28f43-106">The template consists of JSON and expressions which you can use to construct values for your deployment.</span></span>

<span data-ttu-id="28f43-107">Můžete použít novou schopnost **Šablony** v [webu Azure Portal](https://portal.azure.com) spolu s poskytovatelem prostředků **Microsoft.Gallery** jako rozšíření [Azure Marketplace](https://azure.microsoft.com/marketplace/) a umožnit tak uživatelům vytváření, správu a nasazování soukromých šablon z osobní knihovny.</span><span class="sxs-lookup"><span data-stu-id="28f43-107">You can use the new **Templates** capability in the [Azure Portal](https://portal.azure.com) along with the **Microsoft.Gallery** resource provider as an extension of the [Azure Marketplace](https://azure.microsoft.com/marketplace/) to enable users to create, manage and deploy private templates from a personal library.</span></span>

<span data-ttu-id="28f43-108">Tento dokument vás provede přidáním, správou a sdílením soukromé **šablony** pomocí webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="28f43-108">This document walks you through adding, managing and sharing a private **Template** using the Azure Portal.</span></span>

## <a name="guidance"></a><span data-ttu-id="28f43-109">Doprovodné materiály</span><span class="sxs-lookup"><span data-stu-id="28f43-109">Guidance</span></span>
<span data-ttu-id="28f43-110">Následující návrhy vám pomohou naplno využít výhody **šablon** při práci se svými řešeními:</span><span class="sxs-lookup"><span data-stu-id="28f43-110">The following suggestions will help you take full advantage of **Templates** when working with your solutions:</span></span>

* <span data-ttu-id="28f43-111">**Šablona** je zapouzdřující prostředek, který obsahuje šablonu Resource Manageru a další metadata.</span><span class="sxs-lookup"><span data-stu-id="28f43-111">A **Template** is an encapsulating resource that contains an Resource Manager template and additional metadata.</span></span> <span data-ttu-id="28f43-112">Chová se velmi podobně jako položka v Marketplace.</span><span class="sxs-lookup"><span data-stu-id="28f43-112">It behaves very similarly to an item in the Marketplace.</span></span> <span data-ttu-id="28f43-113">Klíčovým rozdílem je, že se jedná o soukromou položku, na rozdíl od veřejných položek Marketplace.</span><span class="sxs-lookup"><span data-stu-id="28f43-113">The key difference is that it is a private item as opposed to the public Marketplace items.</span></span>
* <span data-ttu-id="28f43-114">Knihovna **Šablony** dobře funguje pro uživatele, kteří potřebují přizpůsobit svá nasazení.</span><span class="sxs-lookup"><span data-stu-id="28f43-114">The **Templates** library works well for users who need to customize their deployments.</span></span>
* <span data-ttu-id="28f43-115">**Šablony** dobře fungují pro uživatele, kteří potřebují jednoduché úložiště v rámci Azure.</span><span class="sxs-lookup"><span data-stu-id="28f43-115">**Templates** work well for users who need a simple repository within Azure.</span></span>
* <span data-ttu-id="28f43-116">Začněte s existující šablonou Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="28f43-116">Start with an existing Resource Manager template.</span></span> <span data-ttu-id="28f43-117">Najděte šablony na [GitHubu](https://github.com/Azure/azure-quickstart-templates) nebo [exportujte šablonu](../azure-resource-manager/resource-manager-export-template.md) z existující skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="28f43-117">Find templates in [github](https://github.com/Azure/azure-quickstart-templates) or [Export template](../azure-resource-manager/resource-manager-export-template.md) from an existing resource group.</span></span>
* <span data-ttu-id="28f43-118">**Šablony** jsou vázané na uživatele, který je publikuje.</span><span class="sxs-lookup"><span data-stu-id="28f43-118">**Templates** are tied to the user who publishes them.</span></span> <span data-ttu-id="28f43-119">Název vydavatele je viditelný každému, kdo k němu má přístup pro čtení.</span><span class="sxs-lookup"><span data-stu-id="28f43-119">The publisher name is visible to everyone who has read access to it.</span></span>
* <span data-ttu-id="28f43-120">**Šablony** jsou prostředky Resource Manageru a po publikování je nelze přejmenovat.</span><span class="sxs-lookup"><span data-stu-id="28f43-120">**Templates** are Resource Manager resources and cannot be renamed once published.</span></span>

## <a name="add-a-template-resource"></a><span data-ttu-id="28f43-121">Přidání prostředku šablony</span><span class="sxs-lookup"><span data-stu-id="28f43-121">Add a Template resource</span></span>
<span data-ttu-id="28f43-122">Existují dva způsoby, jak lze na portálu Azure vytvořit prostředek **šablony**.</span><span class="sxs-lookup"><span data-stu-id="28f43-122">There are two ways to create a **Template** resource in the Azure portal.</span></span>

### <a name="method-1--create-a-new-template-resource-from-a-running-resource-group"></a><span data-ttu-id="28f43-123">Způsob 1: Vytvoření nového prostředku šablony ze spuštěné skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="28f43-123">Method 1 : Create a new Template resource from a running resource group</span></span>
1. <span data-ttu-id="28f43-124">Na portálu Azure přejděte do existující skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="28f43-124">Navigate to an existing resource group on the Azure Portal.</span></span> <span data-ttu-id="28f43-125">V **Nastavení** vyberte **Exportovat šablonu**.</span><span class="sxs-lookup"><span data-stu-id="28f43-125">Select **Export template** in **Settings**.</span></span>
2. <span data-ttu-id="28f43-126">Když je šablona Resource Manageru exportovaná, uložte ji pomocí tlačítka **Uložit šablonu** do úložiště **Šablony**.</span><span class="sxs-lookup"><span data-stu-id="28f43-126">Once the Resource Manager template is exported, use the **Save Template** button to save it to the **Templates** repository.</span></span> <span data-ttu-id="28f43-127">Podrobné informace o exportování šablony najdete [zde](../azure-resource-manager/resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="28f43-127">Find complete details for Export template [here](../azure-resource-manager/resource-manager-export-template.md).</span></span>
   <br /><br /><span data-ttu-id="28f43-128">
   ![Export skupiny prostředků](media/rg-export-portal1.PNG)</span><span class="sxs-lookup"><span data-stu-id="28f43-128">
![Resource group export](media/rg-export-portal1.PNG)</span></span>  <br />
3. <span data-ttu-id="28f43-129">Vyberte příkazové tlačítko **Uložit do šablony**.</span><span class="sxs-lookup"><span data-stu-id="28f43-129">Select the **Save to Template** command button.</span></span>
   <br /><br />
4. <span data-ttu-id="28f43-130">Zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="28f43-130">Enter the following information:</span></span>
   
   * <span data-ttu-id="28f43-131">Název – Název objektu šablony (Poznámka: Jedná se o název založený na Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="28f43-131">Name – Name of the template object (NOTE: This is an Azure Resource Manager based name.</span></span> <span data-ttu-id="28f43-132">Platí všechna omezení pro pojmenovávání a název nelze po vytvoření změnit).</span><span class="sxs-lookup"><span data-stu-id="28f43-132">All naming restrictions apply and it cannot be changed once created).</span></span>
   * <span data-ttu-id="28f43-133">Popis – Stručný popis šablony.</span><span class="sxs-lookup"><span data-stu-id="28f43-133">Description – Quick summary about the template.</span></span>
     
     ![Uložení šablony](media/save-template-portal1.PNG)  <br />
5. <span data-ttu-id="28f43-135">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="28f43-135">Click **Save**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="28f43-136">Obsahuje-li exportovaná šablona Resource Manageru chyby, v okně Exportovat šablonu se zobrazí oznámení, ale stále budete moci tuto šablonu Resource Manageru uložit do Šablon.</span><span class="sxs-lookup"><span data-stu-id="28f43-136">The Export template blade shows notifications when the exported Resource Manager template has errors, but you will still be able to save this Resource Manager template to the Templates.</span></span> <span data-ttu-id="28f43-137">Před tím, než znovu nasadíte exportovanou šablonu Resource Manageru, se ujistěte, že jste zkontrolovali a opravili všechny problémy šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="28f43-137">Ensure that you check and fix any Resource Manager template issues before redeploying the exported Resource Manager template.</span></span>
   > 
   > 

### <a name="method-2--add-a-new-template-resource-from-browse"></a><span data-ttu-id="28f43-138">Způsob 2: Přidání nového prostředku šablony z procházení</span><span class="sxs-lookup"><span data-stu-id="28f43-138">Method 2 : Add a new Template resource from browse</span></span>
<span data-ttu-id="28f43-139">Můžete také přidat novou **šablonu** od začátku pomocí příkazového tlačítka +Přidat v **Procházet > Šablony**.</span><span class="sxs-lookup"><span data-stu-id="28f43-139">You can also add a new **Template** from scratch using the +Add command button in **Browse > Templates**.</span></span> <span data-ttu-id="28f43-140">Budete muset zadat Název, Popis a JSON šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="28f43-140">You will need to provide a Name, Description and the Resource Manager template JSON.</span></span>

![Přidání šablony](media/add-template-portal1.PNG)  <br />

> [!NOTE]
> <span data-ttu-id="28f43-142">Microsoft.Gallery je poskytovatel prostředků Azure umístěný na Tenantu.</span><span class="sxs-lookup"><span data-stu-id="28f43-142">Microsoft.Gallery is a Tenant based Azure resource provider.</span></span> <span data-ttu-id="28f43-143">Prostředek šablony je vázán na uživatele, který ho vytvořil.</span><span class="sxs-lookup"><span data-stu-id="28f43-143">The Template resource is tied to the user who created it.</span></span> <span data-ttu-id="28f43-144">Neváže se na konkrétní předplatné.</span><span class="sxs-lookup"><span data-stu-id="28f43-144">It is not tied to any specific subscription.</span></span> <span data-ttu-id="28f43-145">Předplatné je potřeba zvolit pouze při nasazování šablony.</span><span class="sxs-lookup"><span data-stu-id="28f43-145">A subscription needs to be chosen only when deploying a Template.</span></span>
> 
> 

## <a name="view-template-resources"></a><span data-ttu-id="28f43-146">Zobrazení prostředků šablony</span><span class="sxs-lookup"><span data-stu-id="28f43-146">View Template resources</span></span>
<span data-ttu-id="28f43-147">Všechny **šablony**, které máte k dispozici, můžete zobrazit v **Procházet > Šablony**.</span><span class="sxs-lookup"><span data-stu-id="28f43-147">All **Templates** available to you can be seen at **Browse > Templates**.</span></span> <span data-ttu-id="28f43-148">To zahrnuje jak **šablony**, které jste vytvořili, tak i šablony, které jsou s vámi sdílené s různými úrovněmi oprávnění.</span><span class="sxs-lookup"><span data-stu-id="28f43-148">This includes **Templates** you have created as well as ones that have been shared with you with varying levels of permissions.</span></span> <span data-ttu-id="28f43-149">Další podrobnosti naleznete níže v oddílu [Řízení přístupu](#access-control-for-a-tenant-resource-provider).</span><span class="sxs-lookup"><span data-stu-id="28f43-149">More details in the [access control](#access-control-for-a-tenant-resource-provider) section below.</span></span>

![Zobrazit šablonu](media/view-template-portal1.PNG)  <br />

<span data-ttu-id="28f43-151">Podrobnosti o **šabloně** zobrazíte kliknutím na položku v seznamu.</span><span class="sxs-lookup"><span data-stu-id="28f43-151">You can view the details of a **Template** by clicking into an item in the list.</span></span>

![Zobrazit šablonu](media/view-template-portal2c.png)  <br />

## <a name="edit-a-template-resource"></a><span data-ttu-id="28f43-153">Úprava prostředku šablony</span><span class="sxs-lookup"><span data-stu-id="28f43-153">Edit a Template resource</span></span>
<span data-ttu-id="28f43-154">Tok upravování **šablony** můžete zahájit kliknutím pravým tlačítkem na položku v seznamu Procházet nebo vybráním příkazového tlačítka Upravit.</span><span class="sxs-lookup"><span data-stu-id="28f43-154">You can initiate the edit flow for a **Template** by right clicking the item on the Browse list or by choosing the Edit command button.</span></span>

![Úprava šablony](media/edit-template-portal1a.PNG)  <br />

<span data-ttu-id="28f43-156">Můžete upravit popis nebo text šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="28f43-156">You can edit the description or Resource Manager template text.</span></span> <span data-ttu-id="28f43-157">Nemůžete upravit název, protože se jedná o název prostředku Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="28f43-157">You cannot edit the name since it is an Resource Manager resource name.</span></span> <span data-ttu-id="28f43-158">Pokud upravíte kód JSON šablony Resource Manageru, ověříme ho, abychom zajistili, že jde o platný kód JSON.</span><span class="sxs-lookup"><span data-stu-id="28f43-158">When you edit the Resource Manager template JSON we will validate to ensure that it is valid JSON.</span></span> <span data-ttu-id="28f43-159">Vyberte **OK** a poté aktualizovanou šablonu uložte kliknutím na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="28f43-159">Choose **OK** and then **Save** to save your updated template.</span></span>

![Úprava šablony](media/edit-template-portal2a.PNG)  <br />

<span data-ttu-id="28f43-161">Po uložení **šablony** se zobrazí oznámení o potvrzení.</span><span class="sxs-lookup"><span data-stu-id="28f43-161">Once the **Template** is saved you will see a confirmation notification.</span></span>

![Úprava šablony](media/edit-template-portal3b.png)  <br />

## <a name="deploy-a-template-resource"></a><span data-ttu-id="28f43-163">Nasazení prostředku šablony</span><span class="sxs-lookup"><span data-stu-id="28f43-163">Deploy a Template resource</span></span>
<span data-ttu-id="28f43-164">Můžete nasadit jakoukoli **šablonu**, ke které máte oprávnění ke **čtení**.</span><span class="sxs-lookup"><span data-stu-id="28f43-164">You can deploy any **Template** that you have **Read** permissions on.</span></span> <span data-ttu-id="28f43-165">Tok nasazení spustí standardní okno Nasazení šablony Azure.</span><span class="sxs-lookup"><span data-stu-id="28f43-165">The deployment flow launches the standard Azure Template deployment blade.</span></span> <span data-ttu-id="28f43-166">Vyplňte hodnoty parametrů šablony Resource Manageru a pokračujte s nasazováním.</span><span class="sxs-lookup"><span data-stu-id="28f43-166">Fill out the values for the Resource Manager template parameters to proceed with the deployment.</span></span>

![Nasazení šablony](media/deploy-template-portal1b.png)  <br />

## <a name="share-a-template-resource"></a><span data-ttu-id="28f43-168">Úprava prostředku šablony</span><span class="sxs-lookup"><span data-stu-id="28f43-168">Share a Template resource</span></span>
<span data-ttu-id="28f43-169">Prostředek **Šablony** můžete sdílet s ostatními uživateli.</span><span class="sxs-lookup"><span data-stu-id="28f43-169">A **Template** resource can be shared with your peers.</span></span> <span data-ttu-id="28f43-170">Sdílení se chová podobně jako [přiřazování rolí pro jakýkoli prostředek na Azure](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="28f43-170">Sharing behaves similarly to [role assignment for any resource on Azure](../active-directory/role-based-access-control-configure.md).</span></span> <span data-ttu-id="28f43-171">Vlastník **šablony** poskytuje oprávnění ostatním uživatelům, kteří mohou interagovat s prostředkem šablony.</span><span class="sxs-lookup"><span data-stu-id="28f43-171">The **Template** owner provides permissions to other users who can interact with a Template resource.</span></span> <span data-ttu-id="28f43-172">Osoba nebo skupina osob, se kterou **šablonu** sdílíte, uvidí šablonu Resource Manageru a její vlastnosti galerie.</span><span class="sxs-lookup"><span data-stu-id="28f43-172">The person or group of people you share the **Template** with will be able to see the Resource Manager template and its gallery properties.</span></span>

### <a name="access-control-for-the-microsoftgallery-resources"></a><span data-ttu-id="28f43-173">Řízení přístupu pro prostředky Microsoft.Gallery</span><span class="sxs-lookup"><span data-stu-id="28f43-173">Access control for the Microsoft.Gallery resources</span></span>
| <span data-ttu-id="28f43-174">Role</span><span class="sxs-lookup"><span data-stu-id="28f43-174">Role</span></span> | <span data-ttu-id="28f43-175">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="28f43-175">Permissions</span></span> |
| --- | --- |
| <span data-ttu-id="28f43-176">Vlastník</span><span class="sxs-lookup"><span data-stu-id="28f43-176">Owner</span></span> |<span data-ttu-id="28f43-177">Povoluje úplné řízení prostředku šablony včetně sdílení.</span><span class="sxs-lookup"><span data-stu-id="28f43-177">Allows full control on the Template resource including Share</span></span> |
| <span data-ttu-id="28f43-178">Čtenář</span><span class="sxs-lookup"><span data-stu-id="28f43-178">Reader</span></span> |<span data-ttu-id="28f43-179">Povoluje čtení a spouštění (nasazování) prostředku šablony.</span><span class="sxs-lookup"><span data-stu-id="28f43-179">Allows Read and Execute(Deploy) on the Template resource</span></span> |
| <span data-ttu-id="28f43-180">Přispěvatel</span><span class="sxs-lookup"><span data-stu-id="28f43-180">Contributor</span></span> |<span data-ttu-id="28f43-181">Povoluje upravování a právo odstranit prostředek šablony.</span><span class="sxs-lookup"><span data-stu-id="28f43-181">Allows Edit and Delete permission on the Template resource.</span></span> <span data-ttu-id="28f43-182">Uživatel nemůže šablonu sdílet s ostatními.</span><span class="sxs-lookup"><span data-stu-id="28f43-182">User cannot Share the Template with others</span></span> |

<span data-ttu-id="28f43-183">Kliknutím pravým tlačítkem nebo v okně zobrazení konkrétní položky vyberte na procházené položce **Sdílet**</span><span class="sxs-lookup"><span data-stu-id="28f43-183">Select **Share** on the browse item by right clicking or on the view blade of a specific item.</span></span> <span data-ttu-id="28f43-184">Spustí se Možnosti sdílení.</span><span class="sxs-lookup"><span data-stu-id="28f43-184">This launches a Share experience.</span></span>

![Sdílení šablony](media/share-template-portal1a.png)  <br />

 <span data-ttu-id="28f43-186">Nyní můžete zvolit roli a uživatele nebo skupinu, kterým chcete poskytnout přístup ke konkrétní **šabloně**.</span><span class="sxs-lookup"><span data-stu-id="28f43-186">You can now choose a role and a user or group to provide access to a particular **Template**.</span></span> <span data-ttu-id="28f43-187">Dostupné role jsou Vlastník, Čtenář a Přispěvatel.</span><span class="sxs-lookup"><span data-stu-id="28f43-187">The available roles are Owner, Reader and Contributor.</span></span> <span data-ttu-id="28f43-188">Další podrobnosti najdete výše v oddílu [Řízení přístupu](#access-control-for-a-tenant-resource-provider).</span><span class="sxs-lookup"><span data-stu-id="28f43-188">More details in the [access control](#access-control-for-a-tenant-resource-provider) section above.</span></span>

![Sdílení šablony](media/share-template-portal2b.png)  <br />

![Sdílení šablony](media/share-template-portal3b.png)  <br />

<span data-ttu-id="28f43-191">Klikněte na **Vybrat** a **Ok**.</span><span class="sxs-lookup"><span data-stu-id="28f43-191">Click **Select** and **Ok**.</span></span> <span data-ttu-id="28f43-192">Nyní vidíte uživatele a skupiny, které jste přidali k prostředku.</span><span class="sxs-lookup"><span data-stu-id="28f43-192">You can now see the users or groups you added to the resource.</span></span>

![Sdílení šablony](media/share-template-portal4b.png)  <br />

> [!NOTE]
> <span data-ttu-id="28f43-194">Šablonu je možné sdílet pouze s uživateli a skupinami ve stejném tenantu služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="28f43-194">A Template can only be shared with users and groups in the same Azure Active Directory tenant.</span></span> <span data-ttu-id="28f43-195">Pokud šablonu sdílíte s e-mailovou adresou, která není ve vašem tenantu, jejímu uživateli se odešle pozvánka s žádostí o připojení k tenantu v roli hosta.</span><span class="sxs-lookup"><span data-stu-id="28f43-195">If you share a Template with an email address that is not in your tenant, an invitation will be sent asking the user to join the tenant as a guest.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="28f43-196">Další kroky</span><span class="sxs-lookup"><span data-stu-id="28f43-196">Next steps</span></span>
* <span data-ttu-id="28f43-197">Další informace o vytváření šablon Resource Manageru naleznete v tématu [Vytváření šablon](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="28f43-197">To learn about creating Resource Manager templates, see [Authoring templates](../azure-resource-manager/resource-group-authoring-templates.md)</span></span>
* <span data-ttu-id="28f43-198">Funkce, které můžete použít v šabloně Resource Manageru, jsou popsané v tématu [Funkce šablony](../azure-resource-manager/resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="28f43-198">To understand the functions you can use in an Resource Manager template, see [Template functions](../azure-resource-manager/resource-group-template-functions.md)</span></span>
* <span data-ttu-id="28f43-199">Doprovodné materiály o navrhování šablon naleznete v tématu [Osvědčené postupy pro navrhování šablon Azure Resource Manageru](../azure-resource-manager/best-practices-resource-manager-design-templates.md).</span><span class="sxs-lookup"><span data-stu-id="28f43-199">For guidance on designing your templates, see [Best practices for designing Azure Resource Manager templates](../azure-resource-manager/best-practices-resource-manager-design-templates.md)</span></span>

