---
title: "aaaGet spuštění se soukromými šablonami | Microsoft Docs"
description: "Přidat, spravovat a sdílejte svoje soukromé šablony pomocí hello portálu Azure, hello příkazového řádku Azure CLI nebo Powershellu."
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
ms.openlocfilehash: 1fe2c6422f62a98f7ae9ba5c61b9639d993f0bca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-private-templates-on-hello-azure-portal"></a><span data-ttu-id="37074-103">Začínáme se soukromými šablonami na portálu Azure hello</span><span class="sxs-lookup"><span data-stu-id="37074-103">Get started with private Templates on hello Azure Portal</span></span>
<span data-ttu-id="37074-104">[Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) šablona je deklarativní šablona, která používá toodefine vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="37074-104">An [Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) template is a declarative template used toodefine your deployment.</span></span> <span data-ttu-id="37074-105">Můžete definovat hello toodeploy prostředky pro řešení a určit parametry a proměnné, které umožňují tooinput hodnoty pro různá prostředí.</span><span class="sxs-lookup"><span data-stu-id="37074-105">You can define hello resources toodeploy for a solution, and specify parameters and variables that enable you tooinput values for different environments.</span></span> <span data-ttu-id="37074-106">Hello šablona se skládá z JSON a výrazy, které můžete použít hodnoty tooconstruct pro vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="37074-106">hello template consists of JSON and expressions which you can use tooconstruct values for your deployment.</span></span>

<span data-ttu-id="37074-107">Hello můžete použít nové **šablony** funkci hello [portálu Azure](https://portal.azure.com) společně s hello **Microsoft.Gallery** poskytovatele prostředků jako rozšíření nástroje hello [ Azure Marketplace](https://azure.microsoft.com/marketplace/) tooenable uživatelé toocreate, správě a nasazování soukromých šablon z osobní knihovny.</span><span class="sxs-lookup"><span data-stu-id="37074-107">You can use hello new **Templates** capability in hello [Azure Portal](https://portal.azure.com) along with hello **Microsoft.Gallery** resource provider as an extension of hello [Azure Marketplace](https://azure.microsoft.com/marketplace/) tooenable users toocreate, manage and deploy private templates from a personal library.</span></span>

<span data-ttu-id="37074-108">Tento dokument vás provede přidáním, správou a sdílením soukromé provede **šablony** pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="37074-108">This document walks you through adding, managing and sharing a private **Template** using hello Azure Portal.</span></span>

## <a name="guidance"></a><span data-ttu-id="37074-109">Doprovodné materiály</span><span class="sxs-lookup"><span data-stu-id="37074-109">Guidance</span></span>
<span data-ttu-id="37074-110">Hello následující návrhy vám pomohou naplno využít **šablony** při práci se svými řešeními:</span><span class="sxs-lookup"><span data-stu-id="37074-110">hello following suggestions will help you take full advantage of **Templates** when working with your solutions:</span></span>

* <span data-ttu-id="37074-111">**Šablona** je zapouzdřující prostředek, který obsahuje šablonu Resource Manageru a další metadata.</span><span class="sxs-lookup"><span data-stu-id="37074-111">A **Template** is an encapsulating resource that contains an Resource Manager template and additional metadata.</span></span> <span data-ttu-id="37074-112">Chová se velmi podobně tooan položky v hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="37074-112">It behaves very similarly tooan item in hello Marketplace.</span></span> <span data-ttu-id="37074-113">Hello klíčovým rozdílem je, že se jedná o soukromou položku jako toohello názvem na rozdíl od veřejných položek Marketplace.</span><span class="sxs-lookup"><span data-stu-id="37074-113">hello key difference is that it is a private item as opposed toohello public Marketplace items.</span></span>
* <span data-ttu-id="37074-114">Hello **šablony** knihovny funguje dobře pro uživatele, kteří potřebují toocustomize jeho nasazení.</span><span class="sxs-lookup"><span data-stu-id="37074-114">hello **Templates** library works well for users who need toocustomize their deployments.</span></span>
* <span data-ttu-id="37074-115">**Šablony** dobře fungují pro uživatele, kteří potřebují jednoduché úložiště v rámci Azure.</span><span class="sxs-lookup"><span data-stu-id="37074-115">**Templates** work well for users who need a simple repository within Azure.</span></span>
* <span data-ttu-id="37074-116">Začněte s existující šablonou Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="37074-116">Start with an existing Resource Manager template.</span></span> <span data-ttu-id="37074-117">Najděte šablony na [GitHubu](https://github.com/Azure/azure-quickstart-templates) nebo [exportujte šablonu](../azure-resource-manager/resource-manager-export-template.md) z existující skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="37074-117">Find templates in [github](https://github.com/Azure/azure-quickstart-templates) or [Export template](../azure-resource-manager/resource-manager-export-template.md) from an existing resource group.</span></span>
* <span data-ttu-id="37074-118">**Šablony** vázanou toohello uživatelem, který je publikuje.</span><span class="sxs-lookup"><span data-stu-id="37074-118">**Templates** are tied toohello user who publishes them.</span></span> <span data-ttu-id="37074-119">Název vydavatele Hello je viditelná tooeveryone, který má tooit přístup pro čtení.</span><span class="sxs-lookup"><span data-stu-id="37074-119">hello publisher name is visible tooeveryone who has read access tooit.</span></span>
* <span data-ttu-id="37074-120">**Šablony** jsou prostředky Resource Manageru a po publikování je nelze přejmenovat.</span><span class="sxs-lookup"><span data-stu-id="37074-120">**Templates** are Resource Manager resources and cannot be renamed once published.</span></span>

## <a name="add-a-template-resource"></a><span data-ttu-id="37074-121">Přidání prostředku šablony</span><span class="sxs-lookup"><span data-stu-id="37074-121">Add a Template resource</span></span>
<span data-ttu-id="37074-122">Existují dva způsoby toocreate **šablony** prostředku v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="37074-122">There are two ways toocreate a **Template** resource in hello Azure portal.</span></span>

### <a name="method-1--create-a-new-template-resource-from-a-running-resource-group"></a><span data-ttu-id="37074-123">Způsob 1: Vytvoření nového prostředku šablony ze spuštěné skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="37074-123">Method 1 : Create a new Template resource from a running resource group</span></span>
1. <span data-ttu-id="37074-124">Přejděte tooan existující skupinu prostředků na hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="37074-124">Navigate tooan existing resource group on hello Azure Portal.</span></span> <span data-ttu-id="37074-125">V **Nastavení** vyberte **Exportovat šablonu**.</span><span class="sxs-lookup"><span data-stu-id="37074-125">Select **Export template** in **Settings**.</span></span>
2. <span data-ttu-id="37074-126">Po exportu šablony Resource Manageru hello použít hello **uložit šablonu** toosave tlačítko ji toohello **šablony** úložiště.</span><span class="sxs-lookup"><span data-stu-id="37074-126">Once hello Resource Manager template is exported, use hello **Save Template** button toosave it toohello **Templates** repository.</span></span> <span data-ttu-id="37074-127">Podrobné informace o exportování šablony najdete [zde](../azure-resource-manager/resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="37074-127">Find complete details for Export template [here](../azure-resource-manager/resource-manager-export-template.md).</span></span>
   <br /><br /><span data-ttu-id="37074-128">
   ![Export skupiny prostředků](media/rg-export-portal1.PNG)</span><span class="sxs-lookup"><span data-stu-id="37074-128">
![Resource group export](media/rg-export-portal1.PNG)</span></span>  <br />
3. <span data-ttu-id="37074-129">Vyberte hello **uložit tooTemplate** příkazového tlačítka.</span><span class="sxs-lookup"><span data-stu-id="37074-129">Select hello **Save tooTemplate** command button.</span></span>
   <br /><br />
4. <span data-ttu-id="37074-130">Zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="37074-130">Enter hello following information:</span></span>
   
   * <span data-ttu-id="37074-131">Název – název objektu hello šablony (Poznámka: Toto je název založená na správci prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="37074-131">Name – Name of hello template object (NOTE: This is an Azure Resource Manager based name.</span></span> <span data-ttu-id="37074-132">Platí všechna omezení pro pojmenovávání a název nelze po vytvoření změnit).</span><span class="sxs-lookup"><span data-stu-id="37074-132">All naming restrictions apply and it cannot be changed once created).</span></span>
   * <span data-ttu-id="37074-133">Popis – stručný popis šablony hello.</span><span class="sxs-lookup"><span data-stu-id="37074-133">Description – Quick summary about hello template.</span></span>
     
     ![Uložení šablony](media/save-template-portal1.PNG)  <br />
5. <span data-ttu-id="37074-135">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="37074-135">Click **Save**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="37074-136">Hello okně Exportovat šablonu zobrazuje oznámení když hello vyexportované šablony Resource Manageru obsahuje chyby, ale stále budete moct toosave tento toohello šablony správce prostředků šablony.</span><span class="sxs-lookup"><span data-stu-id="37074-136">hello Export template blade shows notifications when hello exported Resource Manager template has errors, but you will still be able toosave this Resource Manager template toohello Templates.</span></span> <span data-ttu-id="37074-137">Ujistěte se, že jste zkontrolovali a opravili všechny Resource Manager problémy se šablonou před hello opětovné nasazení vyexportované šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="37074-137">Ensure that you check and fix any Resource Manager template issues before redeploying hello exported Resource Manager template.</span></span>
   > 
   > 

### <a name="method-2--add-a-new-template-resource-from-browse"></a><span data-ttu-id="37074-138">Způsob 2: Přidání nového prostředku šablony z procházení</span><span class="sxs-lookup"><span data-stu-id="37074-138">Method 2 : Add a new Template resource from browse</span></span>
<span data-ttu-id="37074-139">Můžete také přidat nový **šablony** od začátku pomocí hello příkazového tlačítka + přidat v **procházet > šablony**.</span><span class="sxs-lookup"><span data-stu-id="37074-139">You can also add a new **Template** from scratch using hello +Add command button in **Browse > Templates**.</span></span> <span data-ttu-id="37074-140">Je nutné tooprovide název, popis a hello JSON šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="37074-140">You will need tooprovide a Name, Description and hello Resource Manager template JSON.</span></span>

![Přidání šablony](media/add-template-portal1.PNG)  <br />

> [!NOTE]
> <span data-ttu-id="37074-142">Microsoft.Gallery je poskytovatel prostředků Azure umístěný na Tenantu.</span><span class="sxs-lookup"><span data-stu-id="37074-142">Microsoft.Gallery is a Tenant based Azure resource provider.</span></span> <span data-ttu-id="37074-143">Hello prostředek šablony je vázané toohello uživatele, který ho vytvořil.</span><span class="sxs-lookup"><span data-stu-id="37074-143">hello Template resource is tied toohello user who created it.</span></span> <span data-ttu-id="37074-144">Není vázaná tooany konkrétní předplatné.</span><span class="sxs-lookup"><span data-stu-id="37074-144">It is not tied tooany specific subscription.</span></span> <span data-ttu-id="37074-145">Předplatné je toobe zvolit pouze při nasazování šablony.</span><span class="sxs-lookup"><span data-stu-id="37074-145">A subscription needs toobe chosen only when deploying a Template.</span></span>
> 
> 

## <a name="view-template-resources"></a><span data-ttu-id="37074-146">Zobrazení prostředků šablony</span><span class="sxs-lookup"><span data-stu-id="37074-146">View Template resources</span></span>
<span data-ttu-id="37074-147">Všechny **šablony** k dispozici tooyou si můžete prohlédnout v **procházet > šablony**.</span><span class="sxs-lookup"><span data-stu-id="37074-147">All **Templates** available tooyou can be seen at **Browse > Templates**.</span></span> <span data-ttu-id="37074-148">To zahrnuje jak **šablony**, které jste vytvořili, tak i šablony, které jsou s vámi sdílené s různými úrovněmi oprávnění.</span><span class="sxs-lookup"><span data-stu-id="37074-148">This includes **Templates** you have created as well as ones that have been shared with you with varying levels of permissions.</span></span> <span data-ttu-id="37074-149">Další podrobnosti v hello [řízení přístupu](#access-control-for-a-tenant-resource-provider) části níže.</span><span class="sxs-lookup"><span data-stu-id="37074-149">More details in hello [access control](#access-control-for-a-tenant-resource-provider) section below.</span></span>

![Zobrazit šablonu](media/view-template-portal1.PNG)  <br />

<span data-ttu-id="37074-151">Můžete zobrazit podrobnosti o hello **šablony** kliknutím na položku v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="37074-151">You can view hello details of a **Template** by clicking into an item in hello list.</span></span>

![Zobrazit šablonu](media/view-template-portal2c.png)  <br />

## <a name="edit-a-template-resource"></a><span data-ttu-id="37074-153">Úprava prostředku šablony</span><span class="sxs-lookup"><span data-stu-id="37074-153">Edit a Template resource</span></span>
<span data-ttu-id="37074-154">Můžete zahájit tok upravování hello **šablony** kliknutím pravým tlačítkem na hello položka v seznamu hello procházet nebo vybráním příkazového tlačítka Upravit hello.</span><span class="sxs-lookup"><span data-stu-id="37074-154">You can initiate hello edit flow for a **Template** by right clicking hello item on hello Browse list or by choosing hello Edit command button.</span></span>

![Úprava šablony](media/edit-template-portal1a.PNG)  <br />

<span data-ttu-id="37074-156">Můžete upravit hello popis nebo text šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="37074-156">You can edit hello description or Resource Manager template text.</span></span> <span data-ttu-id="37074-157">Název hello nelze upravit, protože se jedná o název prostředku Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="37074-157">You cannot edit hello name since it is an Resource Manager resource name.</span></span> <span data-ttu-id="37074-158">Při úpravách JSON šablony Resource Manageru hello ověříme tooensure, že se jedná o platný kód JSON.</span><span class="sxs-lookup"><span data-stu-id="37074-158">When you edit hello Resource Manager template JSON we will validate tooensure that it is valid JSON.</span></span> <span data-ttu-id="37074-159">Zvolte **OK** a potom **Uložit** toosave aktualizovanou šablonu.</span><span class="sxs-lookup"><span data-stu-id="37074-159">Choose **OK** and then **Save** toosave your updated template.</span></span>

![Úprava šablony](media/edit-template-portal2a.PNG)  <br />

<span data-ttu-id="37074-161">Jednou hello **šablony** je uložen se zobrazí oznámení o potvrzení.</span><span class="sxs-lookup"><span data-stu-id="37074-161">Once hello **Template** is saved you will see a confirmation notification.</span></span>

![Úprava šablony](media/edit-template-portal3b.png)  <br />

## <a name="deploy-a-template-resource"></a><span data-ttu-id="37074-163">Nasazení prostředku šablony</span><span class="sxs-lookup"><span data-stu-id="37074-163">Deploy a Template resource</span></span>
<span data-ttu-id="37074-164">Můžete nasadit jakoukoli **šablonu**, ke které máte oprávnění ke **čtení**.</span><span class="sxs-lookup"><span data-stu-id="37074-164">You can deploy any **Template** that you have **Read** permissions on.</span></span> <span data-ttu-id="37074-165">Hello tok nasazení spustí standardní okno nasazení šablony Azure hello.</span><span class="sxs-lookup"><span data-stu-id="37074-165">hello deployment flow launches hello standard Azure Template deployment blade.</span></span> <span data-ttu-id="37074-166">Vyplňte hodnoty hello hello Resource Manager šablony parametry tooproceed hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="37074-166">Fill out hello values for hello Resource Manager template parameters tooproceed with hello deployment.</span></span>

![Nasazení šablony](media/deploy-template-portal1b.png)  <br />

## <a name="share-a-template-resource"></a><span data-ttu-id="37074-168">Úprava prostředku šablony</span><span class="sxs-lookup"><span data-stu-id="37074-168">Share a Template resource</span></span>
<span data-ttu-id="37074-169">Prostředek **Šablony** můžete sdílet s ostatními uživateli.</span><span class="sxs-lookup"><span data-stu-id="37074-169">A **Template** resource can be shared with your peers.</span></span> <span data-ttu-id="37074-170">Sdílení se chová podobně jako příliš[přiřazování rolí pro jakýkoli prostředek na Azure](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="37074-170">Sharing behaves similarly too[role assignment for any resource on Azure](../active-directory/role-based-access-control-configure.md).</span></span> <span data-ttu-id="37074-171">Hello **šablony** vlastníka poskytuje oprávnění tooother uživatelé, kteří mohou interagovat s prostředkem šablony.</span><span class="sxs-lookup"><span data-stu-id="37074-171">hello **Template** owner provides permissions tooother users who can interact with a Template resource.</span></span> <span data-ttu-id="37074-172">Hello osoba nebo skupina osob sdílet hello **šablony** se bude moct toosee šablony Resource Manageru hello a její vlastnosti galerie.</span><span class="sxs-lookup"><span data-stu-id="37074-172">hello person or group of people you share hello **Template** with will be able toosee hello Resource Manager template and its gallery properties.</span></span>

### <a name="access-control-for-hello-microsoftgallery-resources"></a><span data-ttu-id="37074-173">Řízení přístupu pro prostředky Microsoft.Gallery hello</span><span class="sxs-lookup"><span data-stu-id="37074-173">Access control for hello Microsoft.Gallery resources</span></span>
| <span data-ttu-id="37074-174">Role</span><span class="sxs-lookup"><span data-stu-id="37074-174">Role</span></span> | <span data-ttu-id="37074-175">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="37074-175">Permissions</span></span> |
| --- | --- |
| <span data-ttu-id="37074-176">Vlastník</span><span class="sxs-lookup"><span data-stu-id="37074-176">Owner</span></span> |<span data-ttu-id="37074-177">Povoluje úplné řízení hello prostředku šablony včetně sdílení.</span><span class="sxs-lookup"><span data-stu-id="37074-177">Allows full control on hello Template resource including Share</span></span> |
| <span data-ttu-id="37074-178">Čtenář</span><span class="sxs-lookup"><span data-stu-id="37074-178">Reader</span></span> |<span data-ttu-id="37074-179">Povoluje čtení a spouštění (nasazování) na hello prostředku šablony</span><span class="sxs-lookup"><span data-stu-id="37074-179">Allows Read and Execute(Deploy) on hello Template resource</span></span> |
| <span data-ttu-id="37074-180">Přispěvatel</span><span class="sxs-lookup"><span data-stu-id="37074-180">Contributor</span></span> |<span data-ttu-id="37074-181">Povoluje upravování a právo odstranit v hello prostředku šablony.</span><span class="sxs-lookup"><span data-stu-id="37074-181">Allows Edit and Delete permission on hello Template resource.</span></span> <span data-ttu-id="37074-182">Uživatele nelze sdílet s ostatními hello šablony</span><span class="sxs-lookup"><span data-stu-id="37074-182">User cannot Share hello Template with others</span></span> |

<span data-ttu-id="37074-183">Vyberte **sdílené složky** na procházené položce hello kliknutím pravým tlačítkem nebo v okně hello zobrazení konkrétní položky.</span><span class="sxs-lookup"><span data-stu-id="37074-183">Select **Share** on hello browse item by right clicking or on hello view blade of a specific item.</span></span> <span data-ttu-id="37074-184">Spustí se Možnosti sdílení.</span><span class="sxs-lookup"><span data-stu-id="37074-184">This launches a Share experience.</span></span>

![Sdílení šablony](media/share-template-portal1a.png)  <br />

 <span data-ttu-id="37074-186">Teď můžete zvolit roli a uživatele nebo skupinu tooprovide přístup tooa konkrétní **šablony**.</span><span class="sxs-lookup"><span data-stu-id="37074-186">You can now choose a role and a user or group tooprovide access tooa particular **Template**.</span></span> <span data-ttu-id="37074-187">Hello dostupné role jsou vlastník, Čtenář a Přispěvatel.</span><span class="sxs-lookup"><span data-stu-id="37074-187">hello available roles are Owner, Reader and Contributor.</span></span> <span data-ttu-id="37074-188">Další podrobnosti v hello [řízení přístupu](#access-control-for-a-tenant-resource-provider) část výše.</span><span class="sxs-lookup"><span data-stu-id="37074-188">More details in hello [access control](#access-control-for-a-tenant-resource-provider) section above.</span></span>

![Sdílení šablony](media/share-template-portal2b.png)  <br />

![Sdílení šablony](media/share-template-portal3b.png)  <br />

<span data-ttu-id="37074-191">Klikněte na **Vybrat** a **Ok**.</span><span class="sxs-lookup"><span data-stu-id="37074-191">Click **Select** and **Ok**.</span></span> <span data-ttu-id="37074-192">Nyní můžete vidět hello uživatele nebo skupiny přidat toohello prostředků.</span><span class="sxs-lookup"><span data-stu-id="37074-192">You can now see hello users or groups you added toohello resource.</span></span>

![Sdílení šablony](media/share-template-portal4b.png)  <br />

> [!NOTE]
> <span data-ttu-id="37074-194">Šablony lze sdílet pouze s uživateli a skupinami v hello stejné klienta Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="37074-194">A Template can only be shared with users and groups in hello same Azure Active Directory tenant.</span></span> <span data-ttu-id="37074-195">Pokud šablonu sdílíte s e-mailovou adresu, která není ve vašem klientovi, pozvánka odešle žádostí hello uživatele toojoin hello klienta jako Host.</span><span class="sxs-lookup"><span data-stu-id="37074-195">If you share a Template with an email address that is not in your tenant, an invitation will be sent asking hello user toojoin hello tenant as a guest.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="37074-196">Další kroky</span><span class="sxs-lookup"><span data-stu-id="37074-196">Next steps</span></span>
* <span data-ttu-id="37074-197">toolearn o vytváření šablon Resource Manageru, najdete v části [vytváření šablon](../azure-resource-manager/resource-group-authoring-templates.md)</span><span class="sxs-lookup"><span data-stu-id="37074-197">toolearn about creating Resource Manager templates, see [Authoring templates](../azure-resource-manager/resource-group-authoring-templates.md)</span></span>
* <span data-ttu-id="37074-198">Funkce hello toounderstand můžete použít v šabloně Resource Manageru, najdete v části [šablony funkcí](../azure-resource-manager/resource-group-template-functions.md)</span><span class="sxs-lookup"><span data-stu-id="37074-198">toounderstand hello functions you can use in an Resource Manager template, see [Template functions](../azure-resource-manager/resource-group-template-functions.md)</span></span>
* <span data-ttu-id="37074-199">Doprovodné materiály o navrhování šablon naleznete v tématu [Osvědčené postupy pro navrhování šablon Azure Resource Manageru](../azure-resource-manager/best-practices-resource-manager-design-templates.md).</span><span class="sxs-lookup"><span data-stu-id="37074-199">For guidance on designing your templates, see [Best practices for designing Azure Resource Manager templates](../azure-resource-manager/best-practices-resource-manager-design-templates.md)</span></span>

