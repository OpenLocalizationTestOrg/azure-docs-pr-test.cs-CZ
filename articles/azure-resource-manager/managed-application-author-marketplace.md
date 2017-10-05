---
title: "Azure spravované aplikace na webu Marketplace | Microsoft Docs"
description: "Popisuje Azure spravované aplikace, které jsou dostupné přes Marketplace."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/09/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: 58ac7665abf7e75a43bb0b92bdf6f41005c3efe8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-managed-applications-in-the-marketplace"></a><span data-ttu-id="a2183-103">Azure spravovaných aplikací na webu Marketplace</span><span class="sxs-lookup"><span data-stu-id="a2183-103">Azure managed applications in the Marketplace</span></span>

 <span data-ttu-id="a2183-104">MSPs, nezávislí dodavatelé softwaru a systémových integrátorech (si) můžete použít Azure spravované aplikace na nabídku svá řešení pro všechny zákazníky využívající Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="a2183-104">MSPs, ISVs, and system integrators (SIs) can use Azure managed applications to offer their solutions to all Azure Marketplace customers.</span></span> <span data-ttu-id="a2183-105">Taková řešení snížit údržby a režijní náklady na údržbu zákazníků.</span><span class="sxs-lookup"><span data-stu-id="a2183-105">Such solutions reduce the maintenance and servicing overhead for customers.</span></span> <span data-ttu-id="a2183-106">Vydavatelé můžete prodeje, infrastruktury a software přes Marketplace.</span><span class="sxs-lookup"><span data-stu-id="a2183-106">Publishers can sell infrastructure and software through the Marketplace.</span></span> <span data-ttu-id="a2183-107">Službám a podpoře provozní se můžete připojit k spravovaných aplikací.</span><span class="sxs-lookup"><span data-stu-id="a2183-107">They can attach services and operational support to managed applications.</span></span> <span data-ttu-id="a2183-108">Další informace najdete v tématu [spravované aplikace přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a2183-108">For more information, see [Managed application overview](managed-application-overview.md).</span></span>

<span data-ttu-id="a2183-109">Tento článek vysvětluje, jak MSP, ISV nebo SI můžete publikovat aplikaci na Marketplace s cílem a co nejvíce zpřístupnili zákazníkům.</span><span class="sxs-lookup"><span data-stu-id="a2183-109">This article explains how an MSP, ISV, or SI can publish an application to the Marketplace and make it broadly available to customers.</span></span>

## <a name="prerequisites-for-publishing-a-managed-application"></a><span data-ttu-id="a2183-110">Předpoklady pro publikování spravované aplikace</span><span class="sxs-lookup"><span data-stu-id="a2183-110">Prerequisites for publishing a managed application</span></span>

<span data-ttu-id="a2183-111">Předpoklady pro výpis v Marketplace:</span><span class="sxs-lookup"><span data-stu-id="a2183-111">Prerequisites to listing in the Marketplace:</span></span>

* <span data-ttu-id="a2183-112">Technické</span><span class="sxs-lookup"><span data-stu-id="a2183-112">Technical</span></span>

    *  <span data-ttu-id="a2183-113">Informace o základní strukturu a syntaxe šablon Azure Resource Manageru najdete v tématu [šablon Azure Resource Manageru](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="a2183-113">For information about the basic structure and syntax of Azure Resource Manager templates, see [Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
    *  <span data-ttu-id="a2183-114">Chcete-li zobrazit úplnou šablonu řešení, najdete v části [šablony Azure Quickstart](https://azure.microsoft.com/en-us/documentation/templates/) nebo [úložiště šablony rychlý Start](https://github.com/azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="a2183-114">To view complete template solutions, see [Azure Quickstart templates](https://azure.microsoft.com/en-us/documentation/templates/) or the [Quickstart template repository](https://github.com/azure/azure-quickstart-templates).</span></span>
    *  <span data-ttu-id="a2183-115">Informace o tom, jak vytvořit rozhraní pro zákazníky, kteří nasazují aplikace prostřednictvím Marketplace najdete v tématu [vytvoření uživatelského rozhraní definice souboru](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a2183-115">For information about how to create the interface for customers who deploy your application through the Marketplace, see [Create a user interface definition file](managed-application-createuidefinition-overview.md).</span></span>

* <span data-ttu-id="a2183-116">Běžné uživatele (obchodní požadavky)</span><span class="sxs-lookup"><span data-stu-id="a2183-116">Nontechnical (business requirements)</span></span>

    *   <span data-ttu-id="a2183-117">Vaše společnost nebo její dceřiné společnosti, musí být umístěny v určité zemi, kde je podporováno prodeje podle Marketplace.</span><span class="sxs-lookup"><span data-stu-id="a2183-117">Your company or its subsidiary must be located in a country where sales are supported by the Marketplace.</span></span>
    *   <span data-ttu-id="a2183-118">Tento produkt musí mít licenci způsobem, který je kompatibilní s modely fakturace nepodporuje Marketplace.</span><span class="sxs-lookup"><span data-stu-id="a2183-118">Your product must be licensed in a way that is compatible with billing models supported by the Marketplace.</span></span>
    *   <span data-ttu-id="a2183-119">Jste zodpovědná za zpřístupnění technické podpory zákazníků vyvineme způsobem.</span><span class="sxs-lookup"><span data-stu-id="a2183-119">You're responsible for making technical support available to customers in a commercially reasonable manner.</span></span> <span data-ttu-id="a2183-120">Podporu může být volná, placené, nebo prostřednictvím komunity podporovat.</span><span class="sxs-lookup"><span data-stu-id="a2183-120">The support can be free, paid, or through community support.</span></span>
    *   <span data-ttu-id="a2183-121">Jste zodpovědní za licencování softwaru a všechny závislosti software třetích stran.</span><span class="sxs-lookup"><span data-stu-id="a2183-121">You're responsible for licensing your software and any third-party software dependencies.</span></span>
    *   <span data-ttu-id="a2183-122">Je nutné zadat obsah, který splňuje kritéria pro vaši nabídku uveden v Marketplace. a na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a2183-122">You must provide content that meets criteria for your offering to be listed in the Marketplace and in the Azure portal.</span></span>
    *   <span data-ttu-id="a2183-123">Budete muset souhlasit s podmínkami Azure Marketplace zapojení zásady a vydavatele smlouvy.</span><span class="sxs-lookup"><span data-stu-id="a2183-123">You must agree to the terms of the Azure Marketplace Participation Policies and Publisher Agreement.</span></span>
    *   <span data-ttu-id="a2183-124">Budete muset souhlasit s splňovat podmínky použití, prohlášení o ochraně osobních údajů Microsoft a certifikované smlouvy programu služby Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="a2183-124">You must agree to comply with the Terms of Use, Microsoft Privacy Statement, and Microsoft Azure Certified Program Agreement.</span></span>

## <a name="create-a-new-azure-application-offer"></a><span data-ttu-id="a2183-125">Vytvořit novou aplikaci Azure nabídka</span><span class="sxs-lookup"><span data-stu-id="a2183-125">Create a new Azure application offer</span></span>

<span data-ttu-id="a2183-126">Po splňujete požadavky, jste připraveni vytvořit nabídku spravované aplikace.</span><span class="sxs-lookup"><span data-stu-id="a2183-126">After you meet the prerequisites, you're ready to create your managed application offer.</span></span> <span data-ttu-id="a2183-127">Podívejme rychlý přehled o nabídku a SKU.</span><span class="sxs-lookup"><span data-stu-id="a2183-127">Let's take a quick overview of an offer and a SKU.</span></span>

### <a name="offer"></a><span data-ttu-id="a2183-128">Nabídka</span><span class="sxs-lookup"><span data-stu-id="a2183-128">Offer</span></span>

<span data-ttu-id="a2183-129">Nabídka pro spravované aplikace odpovídá na třídu produktu nabídky od vydavatele.</span><span class="sxs-lookup"><span data-stu-id="a2183-129">The offer for a managed application corresponds to a class of product offering from a publisher.</span></span> <span data-ttu-id="a2183-130">Pokud máte nový typ řešení nebo aplikace, kterou chcete zpřístupnit v Marketplace, můžete ho nastavit jako novou nabídku.</span><span class="sxs-lookup"><span data-stu-id="a2183-130">If you have a new type of solution/application that you want to make available in the Marketplace, you can set it up as a new offer.</span></span> <span data-ttu-id="a2183-131">Nabídka je kolekce SKU.</span><span class="sxs-lookup"><span data-stu-id="a2183-131">An offer is a collection of SKUs.</span></span> <span data-ttu-id="a2183-132">Všechny nabídky se zobrazí jako vlastní entity v Marketplace.</span><span class="sxs-lookup"><span data-stu-id="a2183-132">Every offer appears as its own entity in the Marketplace.</span></span>

### <a name="sku"></a><span data-ttu-id="a2183-133">Skladová jednotka (SKU)</span><span class="sxs-lookup"><span data-stu-id="a2183-133">SKU</span></span>

<span data-ttu-id="a2183-134">SKU je nejmenší nabízené ke koupi jednotkou nabídku.</span><span class="sxs-lookup"><span data-stu-id="a2183-134">A SKU is the smallest purchasable unit of an offer.</span></span> <span data-ttu-id="a2183-135">Skladová položka v rámci stejné třídy produktu (nabídka) můžete použít k rozlišení mezi:</span><span class="sxs-lookup"><span data-stu-id="a2183-135">You can use a SKU within the same product class (offer) to differentiate between:</span></span>

* <span data-ttu-id="a2183-136">Různé funkce, které jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="a2183-136">Different features that are supported.</span></span>
* <span data-ttu-id="a2183-137">Jestli je nabídka spravované nebo nespravované.</span><span class="sxs-lookup"><span data-stu-id="a2183-137">Whether the offer is managed or unmanaged.</span></span>
* <span data-ttu-id="a2183-138">Modely fakturace, které jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="a2183-138">Billing models that are supported.</span></span>

<span data-ttu-id="a2183-139">V případě nadřazené nabídky na Marketplace se zobrazí SKU.</span><span class="sxs-lookup"><span data-stu-id="a2183-139">A SKU appears under the parent offer in the Marketplace.</span></span> <span data-ttu-id="a2183-140">Zobrazí se jako vlastní nabízené ke koupi entity na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a2183-140">It appears as its own purchasable entity in the Azure portal.</span></span>

### <a name="set-up-an-offer"></a><span data-ttu-id="a2183-141">Nastavit nabídky</span><span class="sxs-lookup"><span data-stu-id="a2183-141">Set up an offer</span></span>

1. <span data-ttu-id="a2183-142">Přihlaste se k [portál Cloud Partner](https://cloudpartner.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="a2183-142">Sign in to the [Cloud Partner portal](https://cloudpartner.azure.com/).</span></span>

2. <span data-ttu-id="a2183-143">V navigačním podokně na levé straně vyberte **+ nové nabídky** > **aplikací Azure**.</span><span class="sxs-lookup"><span data-stu-id="a2183-143">In the navigation pane on the left, select **+ New offer** > **Azure Applications**.</span></span>

    ![Nová nabídka](./media/managed-application-author-marketplace/newOffer.png)

3. <span data-ttu-id="a2183-145">Vyplnění formuláře, které se zobrazují na levé straně v **Editor** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a2183-145">Fill out the forms that appear on the left in the **Editor** view.</span></span> <span data-ttu-id="a2183-146">Povinná pole jsou označené červenou hvězdičkou (*).</span><span class="sxs-lookup"><span data-stu-id="a2183-146">Required fields are marked with a red asterisk (*).</span></span>

    ![Nabídka nastavení](./media/managed-application-author-marketplace/newOffer_OfferSettings.png)

    <span data-ttu-id="a2183-148">Čtyři hlavní forms slouží k vytváření spravované aplikaci:</span><span class="sxs-lookup"><span data-stu-id="a2183-148">Four main forms are used to create a managed application:</span></span>

    <span data-ttu-id="a2183-149">a.</span><span class="sxs-lookup"><span data-stu-id="a2183-149">a.</span></span> <span data-ttu-id="a2183-150">Nabídka nastavení</span><span class="sxs-lookup"><span data-stu-id="a2183-150">Offer Settings</span></span>

    <span data-ttu-id="a2183-151">b.</span><span class="sxs-lookup"><span data-stu-id="a2183-151">b.</span></span> <span data-ttu-id="a2183-152">SKU</span><span class="sxs-lookup"><span data-stu-id="a2183-152">SKUs</span></span>

    <span data-ttu-id="a2183-153">c.</span><span class="sxs-lookup"><span data-stu-id="a2183-153">c.</span></span> <span data-ttu-id="a2183-154">Marketplace</span><span class="sxs-lookup"><span data-stu-id="a2183-154">Marketplace</span></span>

    <span data-ttu-id="a2183-155">d.</span><span class="sxs-lookup"><span data-stu-id="a2183-155">d.</span></span> <span data-ttu-id="a2183-156">Podpora</span><span class="sxs-lookup"><span data-stu-id="a2183-156">Support</span></span>

<span data-ttu-id="a2183-157">Tyto formuláře jsou popsány podrobněji v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="a2183-157">These forms are described in greater detail in the following sections.</span></span>

## <a name="offer-settings-form"></a><span data-ttu-id="a2183-158">Nabídka nastavení formuláře</span><span class="sxs-lookup"><span data-stu-id="a2183-158">Offer Settings form</span></span>
<span data-ttu-id="a2183-159">Tento základní formulář použijte pro zadání nastavení nabídky.</span><span class="sxs-lookup"><span data-stu-id="a2183-159">Use this basic form to specify the offer settings.</span></span>

1. <span data-ttu-id="a2183-160">Vyplňte **nabízejí nastavení** formuláře.</span><span class="sxs-lookup"><span data-stu-id="a2183-160">Fill in the **Offer Settings** form.</span></span> <span data-ttu-id="a2183-161">Různých polí jsou:</span><span class="sxs-lookup"><span data-stu-id="a2183-161">The different fields are:</span></span>

    <span data-ttu-id="a2183-162">a.</span><span class="sxs-lookup"><span data-stu-id="a2183-162">a.</span></span> <span data-ttu-id="a2183-163">**ID nabídky**: Tento jedinečný identifikátor identifikuje nabídku v rámci profilu vydavatele.</span><span class="sxs-lookup"><span data-stu-id="a2183-163">**Offer ID**: This unique identifier identifies the offer within a publisher profile.</span></span> <span data-ttu-id="a2183-164">Toto ID je viditelná v adresách URL produktu, šablony Resource Manageru a fakturace sestavy.</span><span class="sxs-lookup"><span data-stu-id="a2183-164">This ID is visible in product URLs, Resource Manager templates, and billing reports.</span></span> <span data-ttu-id="a2183-165">Může být složené jenom malé alfanumerické znaky nebo pomlčky (-).</span><span class="sxs-lookup"><span data-stu-id="a2183-165">It can only be composed of lowercase alphanumeric characters or dashes (-).</span></span> <span data-ttu-id="a2183-166">ID nemůže končit pomlčkou.</span><span class="sxs-lookup"><span data-stu-id="a2183-166">The ID can't end in a dash.</span></span> <span data-ttu-id="a2183-167">Jeho omezeno na maximálně 50 znaků.</span><span class="sxs-lookup"><span data-stu-id="a2183-167">It's limited to a maximum of 50 characters.</span></span> <span data-ttu-id="a2183-168">Po nabídku přejde za provozu, toto pole je uzamčené.</span><span class="sxs-lookup"><span data-stu-id="a2183-168">After an offer goes live, this field is locked.</span></span>

    <span data-ttu-id="a2183-169">b.</span><span class="sxs-lookup"><span data-stu-id="a2183-169">b.</span></span> <span data-ttu-id="a2183-170">**ID vydavatele**: pomocí tohoto rozevíracího seznamu vyberte vydavatele profil, který chcete publikovat v rámci této nabídky.</span><span class="sxs-lookup"><span data-stu-id="a2183-170">**Publisher ID**: Use this drop-down list to choose the publisher profile you want to publish this offer under.</span></span> <span data-ttu-id="a2183-171">Po nabídku přejde za provozu, toto pole je uzamčené.</span><span class="sxs-lookup"><span data-stu-id="a2183-171">After an offer goes live, this field is locked.</span></span>

    <span data-ttu-id="a2183-172">c.</span><span class="sxs-lookup"><span data-stu-id="a2183-172">c.</span></span> <span data-ttu-id="a2183-173">**Název**: Tento název zobrazení pro danou nabídku se zobrazí v Marketplace. a na portálu.</span><span class="sxs-lookup"><span data-stu-id="a2183-173">**Name**: This display name for your offer appears in the Marketplace and in the portal.</span></span> <span data-ttu-id="a2183-174">Může mít maximálně 50 znaků.</span><span class="sxs-lookup"><span data-stu-id="a2183-174">It can have a maximum of 50 characters.</span></span> <span data-ttu-id="a2183-175">Zahrnují srozumitelný název značky pro svůj produkt.</span><span class="sxs-lookup"><span data-stu-id="a2183-175">Include a recognizable brand name for your product.</span></span> <span data-ttu-id="a2183-176">Nezadávejte název společnosti, pokud to není, jak je na trh.</span><span class="sxs-lookup"><span data-stu-id="a2183-176">Don't include your company name here unless that's how it's marketed.</span></span> <span data-ttu-id="a2183-177">Pokud jste marketingové v rámci této nabídky na vlastní web, ujistěte se, zda je název přesně jak se zobrazuje na vašem webu.</span><span class="sxs-lookup"><span data-stu-id="a2183-177">If you're marketing this offer on your own website, ensure that the name is exactly how it appears on your website.</span></span>

2. <span data-ttu-id="a2183-178">Vyberte **Uložit** k rozdělanou práci uložit.</span><span class="sxs-lookup"><span data-stu-id="a2183-178">Select **Save** to save your progress.</span></span> 

## <a name="skus-form"></a><span data-ttu-id="a2183-179">SKU formuláře</span><span class="sxs-lookup"><span data-stu-id="a2183-179">SKUs form</span></span>
<span data-ttu-id="a2183-180">Dalším krokem je přidání SKU pro vaši nabídku.</span><span class="sxs-lookup"><span data-stu-id="a2183-180">The next step is to add SKUs for your offer.</span></span>

1. <span data-ttu-id="a2183-181">Vyberte **SKU** > **nové SKU**.</span><span class="sxs-lookup"><span data-stu-id="a2183-181">Select **SKUs** > **New SKU**.</span></span> 

    ![Vyberte nové SKU](./media/managed-application-author-marketplace/newOffer_skus.png)

2. <span data-ttu-id="a2183-183">Zadejte **SKU ID**.</span><span class="sxs-lookup"><span data-stu-id="a2183-183">Enter a **SKU ID**.</span></span> <span data-ttu-id="a2183-184">SKU ID je jedinečný identifikátor pro SKU v rámci nabídku.</span><span class="sxs-lookup"><span data-stu-id="a2183-184">A SKU ID is a unique identifier for the SKU within an offer.</span></span> <span data-ttu-id="a2183-185">Toto ID je viditelná v adresách URL produktu, šablony Resource Manageru a fakturace sestavy.</span><span class="sxs-lookup"><span data-stu-id="a2183-185">This ID is visible in product URLs, Resource Manager templates, and billing reports.</span></span> <span data-ttu-id="a2183-186">Může být složené jenom malé alfanumerické znaky nebo pomlčky (-).</span><span class="sxs-lookup"><span data-stu-id="a2183-186">It can only be composed of lowercase alphanumeric characters or dashes (-).</span></span> <span data-ttu-id="a2183-187">ID nemůže končit pomlčkou a jeho omezeno na maximálně 50 znaků.</span><span class="sxs-lookup"><span data-stu-id="a2183-187">The ID can't end in a dash, and it's limited to a maximum of 50 characters.</span></span> <span data-ttu-id="a2183-188">Po nabídku přejde za provozu, toto pole je uzamčené.</span><span class="sxs-lookup"><span data-stu-id="a2183-188">After an offer goes live, this field is locked.</span></span> <span data-ttu-id="a2183-189">Můžete mít více SKU v rámci nabídku.</span><span class="sxs-lookup"><span data-stu-id="a2183-189">You can have multiple SKUs within an offer.</span></span> <span data-ttu-id="a2183-190">Je nutné SKU pro každé bitové kopie, které chcete publikovat.</span><span class="sxs-lookup"><span data-stu-id="a2183-190">You need a SKU for each image you plan to publish.</span></span>

3. <span data-ttu-id="a2183-191">Vyplňte **SKU podrobnosti** část v následující podobě:</span><span class="sxs-lookup"><span data-stu-id="a2183-191">Fill out the **SKU Details** section on the following form:</span></span>

    ![Zadejte nové SKU](./media/managed-application-author-marketplace/newOffer_newsku.png)

    <span data-ttu-id="a2183-193">Vyplňte následující pole:</span><span class="sxs-lookup"><span data-stu-id="a2183-193">Fill out the following fields:</span></span>
    
    <span data-ttu-id="a2183-194">a.</span><span class="sxs-lookup"><span data-stu-id="a2183-194">a.</span></span> <span data-ttu-id="a2183-195">**Název**: Zadejte název pro tento SKU.</span><span class="sxs-lookup"><span data-stu-id="a2183-195">**Title**: Enter a title for this SKU.</span></span> <span data-ttu-id="a2183-196">Tento název se zobrazí v galerii pro tuto položku.</span><span class="sxs-lookup"><span data-stu-id="a2183-196">This title appears in the gallery for this item.</span></span>

    <span data-ttu-id="a2183-197">b.</span><span class="sxs-lookup"><span data-stu-id="a2183-197">b.</span></span> <span data-ttu-id="a2183-198">**Souhrn**: Zadejte krátký souhrn pro tento SKU.</span><span class="sxs-lookup"><span data-stu-id="a2183-198">**Summary**: Enter a short summary for this SKU.</span></span> <span data-ttu-id="a2183-199">Tento text se zobrazí pod názvem.</span><span class="sxs-lookup"><span data-stu-id="a2183-199">This text appears underneath the title.</span></span>

    <span data-ttu-id="a2183-200">c.</span><span class="sxs-lookup"><span data-stu-id="a2183-200">c.</span></span> <span data-ttu-id="a2183-201">**Popis**: Zadejte podrobný popis o verze SKU.</span><span class="sxs-lookup"><span data-stu-id="a2183-201">**Description**: Enter a detailed description about the SKU.</span></span>

    <span data-ttu-id="a2183-202">d.</span><span class="sxs-lookup"><span data-stu-id="a2183-202">d.</span></span> <span data-ttu-id="a2183-203">**Typ SKU**: povolené hodnoty jsou **spravované aplikace** a **šablony řešení**.</span><span class="sxs-lookup"><span data-stu-id="a2183-203">**SKU Type**: The allowed values are **Managed Application** and **Solution Templates**.</span></span> <span data-ttu-id="a2183-204">Pro tento případ, vyberte **spravované aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a2183-204">For this case, select **Managed Application**.</span></span>

4. <span data-ttu-id="a2183-205">Vyplňte **podrobnosti balíčku** část v následující podobě:</span><span class="sxs-lookup"><span data-stu-id="a2183-205">Fill out the **Package Details** section on the following form:</span></span>

    ![Balíček](./media/managed-application-author-marketplace/newOffer_newsku_package.png)

    <span data-ttu-id="a2183-207">Vyplňte následující pole:</span><span class="sxs-lookup"><span data-stu-id="a2183-207">Fill out the following fields:</span></span>

    <span data-ttu-id="a2183-208">a.</span><span class="sxs-lookup"><span data-stu-id="a2183-208">a.</span></span> <span data-ttu-id="a2183-209">**Aktuální verze**: Zadejte verzi balíčku můžete nahrávat na server.</span><span class="sxs-lookup"><span data-stu-id="a2183-209">**Current Version**: Enter a version for the package you upload.</span></span> <span data-ttu-id="a2183-210">Musí být ve formátu `{number}.{number}.{number}{number}`.</span><span class="sxs-lookup"><span data-stu-id="a2183-210">It should be in the format `{number}.{number}.{number}{number}`.</span></span>

    <span data-ttu-id="a2183-211">b.</span><span class="sxs-lookup"><span data-stu-id="a2183-211">b.</span></span> <span data-ttu-id="a2183-212">**Vyberte soubor balíčku**: Tento balíček obsahuje následující soubory, které jsou komprimované a do souboru .zip:</span><span class="sxs-lookup"><span data-stu-id="a2183-212">**Select a package file**: This package contains the following files that are compressed into a .zip file:</span></span>
    * <span data-ttu-id="a2183-213">**applianceMainTemplate.json**: soubor šablony nasazení, který se používá k nasazení řešení nebo aplikace.</span><span class="sxs-lookup"><span data-stu-id="a2183-213">**applianceMainTemplate.json**: The deployment template file that's used to deploy the solution/application.</span></span> <span data-ttu-id="a2183-214">Informace o tom, jak vytvořit nasazení soubory šablony najdete v tématu [vytvoření vaší první šablony Azure Resource Manager](resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="a2183-214">For information about how to create deployment template files, see [Create your first Azure Resource Manager template](resource-manager-create-first-template.md).</span></span>
    * <span data-ttu-id="a2183-215">**appliancecreateUIDefinition.json**: Tento soubor se používá na portálu Azure ke generování uživatelského rozhraní, které slouží ke zřízení tohoto řešení aplikace.</span><span class="sxs-lookup"><span data-stu-id="a2183-215">**appliancecreateUIDefinition.json**: This file is used by the Azure portal to generate the user interface that's used to provision this solution/application.</span></span> <span data-ttu-id="a2183-216">Další informace najdete v tématu [začít pracovat s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a2183-216">For more information, see [Get started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
    * <span data-ttu-id="a2183-217">**mainTemplate.json**: Tento soubor šablony obsahuje pouze Microsoft.Solution/appliances prostředků.</span><span class="sxs-lookup"><span data-stu-id="a2183-217">**mainTemplate.json**: This template file contains only the Microsoft.Solution/appliances resource.</span></span> <span data-ttu-id="a2183-218">Soubor mainTemplate zahrnuje následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="a2183-218">The mainTemplate file includes the following properties:</span></span>

        *  <span data-ttu-id="a2183-219">**druh**: použití **Marketplace** spravovaných aplikací na webu Marketplace.</span><span class="sxs-lookup"><span data-stu-id="a2183-219">**kind**: Use **Marketplace** for managed applications in the Marketplace.</span></span>
        *  <span data-ttu-id="a2183-220">**ManagedResourceGroupId**: Tato skupina prostředků v předplatném zákazníka je, kde jsou všechny prostředky, které jsou definované v applianceMainTemplate.json nasazen.</span><span class="sxs-lookup"><span data-stu-id="a2183-220">**ManagedResourceGroupId**: This resource group in the customer's subscription is where all the resources defined in applianceMainTemplate.json are deployed.</span></span>
        *  <span data-ttu-id="a2183-221">**PublisherPackageId**: Tento řetězec jednoznačně identifikuje balíčku.</span><span class="sxs-lookup"><span data-stu-id="a2183-221">**PublisherPackageId**: This string uniquely identifies the package.</span></span> <span data-ttu-id="a2183-222">Zadejte hodnotu ve formátu `{publisherId}.{OfferId}.{SKUID}.{PackageVersion}`.</span><span class="sxs-lookup"><span data-stu-id="a2183-222">Provide the value in the format of `{publisherId}.{OfferId}.{SKUID}.{PackageVersion}`.</span></span>

<span data-ttu-id="a2183-223">Získat **nabízejí ID** a **ID vydavatele** z portálu pro publikování, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="a2183-223">Obtain the **Offer ID** and **Publisher ID** from the publishing portal, as shown in the following image:</span></span>

![ID nabídky](./media/managed-application-author-marketplace/UniqueString_pubid_offerid.png)
        
<span data-ttu-id="a2183-225">Získat **SKU ID**, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="a2183-225">Obtain the **SKU ID**, as shown in the following image:</span></span>

![SKU ID](./media/managed-application-author-marketplace/UniqueString_skuid.png)
        
<span data-ttu-id="a2183-227">Získání balíčku **verze**, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="a2183-227">Obtain the package **Version**, as shown in the following image:</span></span>

![Verze balíčku](./media/managed-application-author-marketplace/UniqueString_packageversion.png)
    
  <span data-ttu-id="a2183-229">Podle předchozích ukázkách, hodnota **PublisherPackageId** je `azureappliance-test.ravmanagedapptest.ravpreviewmanagedsku.1.0.0`.</span><span class="sxs-lookup"><span data-stu-id="a2183-229">Based on the preceding examples, the value of **PublisherPackageId** is `azureappliance-test.ravmanagedapptest.ravpreviewmanagedsku.1.0.0`.</span></span>

  <span data-ttu-id="a2183-230">Ukázka mainTemplate.json:</span><span class="sxs-lookup"><span data-stu-id="a2183-230">Sample mainTemplate.json:</span></span>

  ```json
  {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "storageAccountNamePrefix": {
        "type": "string",
        "metadata": {
          "description": "Specify the name of the storage account"
        }
      },
      "storageAccountType": {
        "type": "string"
      }
    },
    "variables": {
      "managedResourceGroup": "[concat(resourceGroup().id,uniquestring(resourceGroup().id))]"
    },
    "resources": [{
      "type": "Microsoft.Solutions/appliances",
      "apiVersion": "2016-09-01-preview",
      "name": "[concat(parameters('storageAccountNamePrefix'), '-', 'managed')]",
      "location": "[resourceGroup().location]",
      "kind": "marketplace",
      "properties": {
        "managedResourceGroupId": "[variables('managedResourceGroup')]",
        "PublisherPackageId":"azureappliancetest.ravmanagedapptest.ravpreviewmanagedsku.1.0.0",
        "parameters": {
          "storageAccountName": {
            "value": "[parameters('storageAccountNamePrefix')]"
          },
          "storageAccountType": {
            "value": "[parameters('storageAccountType')]"
          }
        }
      }
    }],
    "outputs": {

    }
  }
  ```

<span data-ttu-id="a2183-231">Tento balíček měl obsahovat všechny vnořené šablony nebo skripty, které jsou požadovány k přidělení úspěšně této aplikace.</span><span class="sxs-lookup"><span data-stu-id="a2183-231">This package should contain any other nested templates or scripts that are required to successfully provision this application.</span></span> <span data-ttu-id="a2183-232">MainTemplate.json, applianceMainTemplate.json a applianceCreateUIDefinition.json soubory musí být přítomen v kořenové složce.</span><span class="sxs-lookup"><span data-stu-id="a2183-232">The mainTemplate.json, applianceMainTemplate.json, and applianceCreateUIDefinition.json files must be present at the root folder.</span></span>

* <span data-ttu-id="a2183-233">**Povolení**: definuje tato vlastnost, který získá přístup a úroveň přístupu k prostředkům v rámci předplatných zákazníků.</span><span class="sxs-lookup"><span data-stu-id="a2183-233">**Authorizations**: This property defines who gets access and the level of access to the resources in customers' subscriptions.</span></span> <span data-ttu-id="a2183-234">Vydavatel slouží ke správě aplikace jménem zákazníka.</span><span class="sxs-lookup"><span data-stu-id="a2183-234">The publisher can use it to manage the application on behalf of the customer.</span></span>
* <span data-ttu-id="a2183-235">**PrincipalId**: Tato vlastnost je Azure Active Directory (Azure AD) identifikátor uživatele, skupiny uživatelů nebo aplikací, který má oprávnění určité s prostředky v rámci předplatného zákazníka.</span><span class="sxs-lookup"><span data-stu-id="a2183-235">**PrincipalId**: This property is the Azure Active Directory (Azure AD) identifier of a user, user group, or application that's granted certain permissions on the resources in the customer's subscription.</span></span> <span data-ttu-id="a2183-236">Definice Role jsou popsána oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a2183-236">The Role Definition describes the permissions.</span></span> 
* <span data-ttu-id="a2183-237">**Definice role**: Tato vlastnost je seznam všech uvedené vestavěné role řízení přístupu na základě Role (RBAC) poskytované Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a2183-237">**Role Definition**: This property is a list of all the built-in Role-Based Access Control (RBAC) roles provided by Azure AD.</span></span> <span data-ttu-id="a2183-238">Můžete vybrat roli, která je nejvhodnější používat ke správě prostředků jménem zákazníka.</span><span class="sxs-lookup"><span data-stu-id="a2183-238">You can select the role that's most appropriate to use to manage the resources on behalf of the customer.</span></span>

<span data-ttu-id="a2183-239">Můžete přidat více oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a2183-239">You can add multiple authorizations.</span></span> <span data-ttu-id="a2183-240">Doporučujeme vytvořit skupinu uživatele AD a zadejte své ID v **PrincipalId**.</span><span class="sxs-lookup"><span data-stu-id="a2183-240">We recommend that you create an AD user group and specify its ID in **PrincipalId**.</span></span> <span data-ttu-id="a2183-241">Tímto způsobem můžete přidat další uživatele do skupiny uživatelů, aniž by bylo nutné aktualizovat verze SKU.</span><span class="sxs-lookup"><span data-stu-id="a2183-241">This way, you can add more users to the user group without the need to update the SKU.</span></span>

<span data-ttu-id="a2183-242">Další informace o RBAC najdete v tématu [začít pracovat s RBAC na portálu Azure](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="a2183-242">For more information about RBAC, see [Get started with RBAC in the Azure portal](../active-directory/role-based-access-control-what-is.md).</span></span>

## <a name="marketplace-form"></a><span data-ttu-id="a2183-243">Formulář Marketplace.</span><span class="sxs-lookup"><span data-stu-id="a2183-243">Marketplace form</span></span>

<span data-ttu-id="a2183-244">Ve formuláři Marketplace je dotaz pro pole, které se zobrazí na [Azure Marketplace](https://azuremarketplace.microsoft.com) a na [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="a2183-244">The Marketplace form asks for fields that show up on the [Azure Marketplace](https://azuremarketplace.microsoft.com) and on the [Azure portal](https://portal.azure.com/).</span></span>

### <a name="preview-subscription-ids"></a><span data-ttu-id="a2183-245">ID odběru Preview</span><span class="sxs-lookup"><span data-stu-id="a2183-245">Preview subscription IDs</span></span>

<span data-ttu-id="a2183-246">Zadejte seznam předplatného Azure ID, kteří mohou přistupovat k nabídku po publikování.</span><span class="sxs-lookup"><span data-stu-id="a2183-246">Enter a list of Azure subscription IDs that can access the offer after it's published.</span></span> <span data-ttu-id="a2183-247">Tyto odběry uvedené prázdné můžete použít k testování nabídku náhled před jeho provedením za provozu.</span><span class="sxs-lookup"><span data-stu-id="a2183-247">You can use these white-listed subscriptions to test the previewed offer before you make it live.</span></span> <span data-ttu-id="a2183-248">Seznam povolených až 100 předplatná v portálu pro partnery můžete zkompilovat.</span><span class="sxs-lookup"><span data-stu-id="a2183-248">You can compile a white list of up to 100 subscriptions in the partner portal.</span></span>

### <a name="suggested-categories"></a><span data-ttu-id="a2183-249">Navrhované kategorií</span><span class="sxs-lookup"><span data-stu-id="a2183-249">Suggested categories</span></span>

<span data-ttu-id="a2183-250">Vyberte až pěti kategorií ze seznamu, který může být nejlépe přidružený vaši nabídku.</span><span class="sxs-lookup"><span data-stu-id="a2183-250">Select up to five categories from the list that your offer can be best associated with.</span></span> <span data-ttu-id="a2183-251">Tyto kategorie se používají pro mapování vaši nabídku do kategorie produktů, které jsou k dispozici v [Azure Marketplace](https://azuremarketplace.microsoft.com) a [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="a2183-251">These categories are used to map your offer to the product categories that are available in the [Azure Marketplace](https://azuremarketplace.microsoft.com) and the [Azure portal](https://portal.azure.com/).</span></span>

#### <a name="azure-marketplace"></a><span data-ttu-id="a2183-252">Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="a2183-252">Azure Marketplace</span></span>

<span data-ttu-id="a2183-253">Souhrn spravované aplikace zobrazí následující pole:</span><span class="sxs-lookup"><span data-stu-id="a2183-253">The summary of your managed application displays the following fields:</span></span>

![Souhrn Marketplace](./media/managed-application-author-marketplace/publishvm10.png)

<span data-ttu-id="a2183-255">**Přehled** kartě pro spravované aplikace zobrazí následující pole:</span><span class="sxs-lookup"><span data-stu-id="a2183-255">The **Overview** tab for your managed application displays the following fields:</span></span>

![Přehled webu Marketplace](./media/managed-application-author-marketplace/publishvm11.png)

<span data-ttu-id="a2183-257">**Plány + cenová** kartě pro spravované aplikace zobrazí následující pole:</span><span class="sxs-lookup"><span data-stu-id="a2183-257">The **Plans + Pricing** tab for your managed application displays the following fields:</span></span>

![Plány Marketplace.](./media/managed-application-author-marketplace/publishvm15.png)

#### <a name="azure-portal"></a><span data-ttu-id="a2183-259">portál Azure</span><span class="sxs-lookup"><span data-stu-id="a2183-259">Azure portal</span></span>

<span data-ttu-id="a2183-260">Souhrn spravované aplikace zobrazí následující pole:</span><span class="sxs-lookup"><span data-stu-id="a2183-260">The summary of your managed application displays the following fields:</span></span>

![Portál souhrn](./media/managed-application-author-marketplace/publishvm12.png)

<span data-ttu-id="a2183-262">Přehled pro spravované aplikace zobrazí následující pole:</span><span class="sxs-lookup"><span data-stu-id="a2183-262">The overview for your managed application displays the following fields:</span></span>

![Přehled portálu](./media/managed-application-author-marketplace/publishvm13.png)

#### <a name="logo-guidelines"></a><span data-ttu-id="a2183-264">Logu</span><span class="sxs-lookup"><span data-stu-id="a2183-264">Logo guidelines</span></span>

<span data-ttu-id="a2183-265">Postupujte podle těchto pokynů pro všechny logo, který nahrajete na portálu Cloud Partner:</span><span class="sxs-lookup"><span data-stu-id="a2183-265">Follow these guidelines for any logo that you upload in the Cloud Partner portal:</span></span>

*   <span data-ttu-id="a2183-266">Azure návrh obsahuje paletu barev jednoduché.</span><span class="sxs-lookup"><span data-stu-id="a2183-266">The Azure design has a simple color palette.</span></span> <span data-ttu-id="a2183-267">Omezte počet primární a sekundární barev v loga.</span><span class="sxs-lookup"><span data-stu-id="a2183-267">Limit the number of primary and secondary colors on your logo.</span></span>
*   <span data-ttu-id="a2183-268">Barev motivů portálu jsou bílé a černé.</span><span class="sxs-lookup"><span data-stu-id="a2183-268">The theme colors of the portal are white and black.</span></span> <span data-ttu-id="a2183-269">Nepoužívejte tyto barvy jako barvu pozadí pro loga.</span><span class="sxs-lookup"><span data-stu-id="a2183-269">Don't use these colors as the background color for your logo.</span></span> <span data-ttu-id="a2183-270">Použijte barvu, která umožňuje loga viditelného na portálu.</span><span class="sxs-lookup"><span data-stu-id="a2183-270">Use a color that makes your logo prominent in the portal.</span></span> <span data-ttu-id="a2183-271">Doporučujeme, abyste jednoduché primární barvy.</span><span class="sxs-lookup"><span data-stu-id="a2183-271">We recommend simple primary colors.</span></span> <span data-ttu-id="a2183-272">*Pokud používáte průhledné pozadí, ujistěte se, že nejsou bílé, logem a text černé nebo blue.*</span><span class="sxs-lookup"><span data-stu-id="a2183-272">*If you use a transparent background, make sure that the logo and text aren't white, black, or blue.*</span></span>
*   <span data-ttu-id="a2183-273">Nepoužívejte barevného přechodu pozadí na logo.</span><span class="sxs-lookup"><span data-stu-id="a2183-273">Don't use a gradient background on the logo.</span></span>
*   <span data-ttu-id="a2183-274">Neukládejte text do logo, ani vaše společnost nebo název značky.</span><span class="sxs-lookup"><span data-stu-id="a2183-274">Don't place text on the logo, not even your company or brand name.</span></span> <span data-ttu-id="a2183-275">Vzhled a chování vaše logo musí být plochý a vyhnout se přechody.</span><span class="sxs-lookup"><span data-stu-id="a2183-275">The look and feel of your logo should be flat and avoid gradients.</span></span>
*   <span data-ttu-id="a2183-276">Zajistěte, aby že logo není roztažená.</span><span class="sxs-lookup"><span data-stu-id="a2183-276">Make sure the logo isn't stretched.</span></span>

#### <a name="hero-logo"></a><span data-ttu-id="a2183-277">Logo nejdůležitější</span><span class="sxs-lookup"><span data-stu-id="a2183-277">Hero logo</span></span>

<span data-ttu-id="a2183-278">Logo nejdůležitější je volitelný.</span><span class="sxs-lookup"><span data-stu-id="a2183-278">The hero logo is optional.</span></span> <span data-ttu-id="a2183-279">Vydavatele můžete vypnout nahrávat logo nejdůležitější.</span><span class="sxs-lookup"><span data-stu-id="a2183-279">The publisher can choose not to upload a hero logo.</span></span> <span data-ttu-id="a2183-280">Po odeslání ikonu hrdina nelze odstranit.</span><span class="sxs-lookup"><span data-stu-id="a2183-280">After the hero icon is uploaded, it can't be deleted.</span></span> <span data-ttu-id="a2183-281">V ten moment se musí partnerovi postupujte podle pokynů Marketplace pro nejdůležitější ikony.</span><span class="sxs-lookup"><span data-stu-id="a2183-281">At that time, the partner must follow the Marketplace guidelines for hero icons.</span></span>

<span data-ttu-id="a2183-282">Postupujte podle těchto pokynů pro ikonu logo nejdůležitější:</span><span class="sxs-lookup"><span data-stu-id="a2183-282">Follow these guidelines for the hero logo icon:</span></span>

*   <span data-ttu-id="a2183-283">Zobrazovaný název vydavatele, název plánu a nabídky dlouhé shrnutí jsou zobrazeny v prázdné.</span><span class="sxs-lookup"><span data-stu-id="a2183-283">The publisher display name, the plan title, and the offer long summary are displayed in white.</span></span> <span data-ttu-id="a2183-284">Proto nepoužívejte světla barvu pozadí ikonu nejdůležitější.</span><span class="sxs-lookup"><span data-stu-id="a2183-284">Therefore, don't use a light color for the background of the hero icon.</span></span> <span data-ttu-id="a2183-285">Černé, bílé nebo průhledné pozadí není povolena pro nejdůležitější ikony.</span><span class="sxs-lookup"><span data-stu-id="a2183-285">A black, white, or transparent background isn't allowed for hero icons.</span></span>
*   <span data-ttu-id="a2183-286">Po nabídka je uvedena, vydavatele zobrazí název, plán title, nabídku dlouhé shrnutí a **vytvořit** tlačítko jsou uvnitř logo hrdina vložených prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="a2183-286">After the offer is listed, the publisher display name, the plan title, the offer long summary, and the **Create** button are embedded programmatically inside the hero logo.</span></span> <span data-ttu-id="a2183-287">V důsledku toho není zadat libovolný text při návrhu logo nejdůležitější.</span><span class="sxs-lookup"><span data-stu-id="a2183-287">Consequently, don't enter any text while you design the hero logo.</span></span> <span data-ttu-id="a2183-288">Vzhledem k tomu, že je text do tohoto místa zahrnuto prostřednictvím kódu programu, ponechte prázdné místo na pravé straně.</span><span class="sxs-lookup"><span data-stu-id="a2183-288">Leave empty space on the right because the text is included programmatically in that space.</span></span> <span data-ttu-id="a2183-289">Prázdné místo pro text by mělo být 415 × 100 pixelů na pravé straně.</span><span class="sxs-lookup"><span data-stu-id="a2183-289">The empty space for the text should be 415 x 100 pixels on the right.</span></span> <span data-ttu-id="a2183-290">Posunut 370 pixelů od levého okraje.</span><span class="sxs-lookup"><span data-stu-id="a2183-290">It's offset by 370 pixels from the left.</span></span>

    ![Příklad logo nejdůležitější](./media/managed-application-author-marketplace/publishvm14.png)

## <a name="support-form"></a><span data-ttu-id="a2183-292">Podpora formuláře</span><span class="sxs-lookup"><span data-stu-id="a2183-292">Support form</span></span>

<span data-ttu-id="a2183-293">Vyplňte **podporu** formuláře s podporou kontaktuje z vaší společnosti.</span><span class="sxs-lookup"><span data-stu-id="a2183-293">Fill out the **Support** form with support contacts from your company.</span></span> <span data-ttu-id="a2183-294">Tyto informace může technici, kontakty a kontakty podpory zákazníků.</span><span class="sxs-lookup"><span data-stu-id="a2183-294">This information might be engineering contacts and customer support contacts.</span></span>

## <a name="publish-an-offer"></a><span data-ttu-id="a2183-295">Publikování nabídky</span><span class="sxs-lookup"><span data-stu-id="a2183-295">Publish an offer</span></span>

<span data-ttu-id="a2183-296">Po vyplnění všech oddílů, vyberte **publikovat** ke spuštění procesu, který zpřístupní vaši nabídku pro zákazníky.</span><span class="sxs-lookup"><span data-stu-id="a2183-296">After you fill out all the sections, select **Publish** to start the process that makes your offer available to customers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a2183-297">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a2183-297">Next steps</span></span>

* <span data-ttu-id="a2183-298">Úvod do spravovaných aplikací, najdete v části [spravované aplikace přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a2183-298">For an introduction to managed applications, see [Managed application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="a2183-299">Informace o využívání spravované aplikace z webu Marketplace najdete v tématu [využívat Azure spravované aplikace na webu Marketplace](managed-application-consume-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="a2183-299">For information about consuming a managed application from the Marketplace, see [Consume Azure managed applications in the Marketplace](managed-application-consume-marketplace.md).</span></span>
* <span data-ttu-id="a2183-300">Informace o publikování aplikace spravované katalogu služeb najdete v tématu [vytvoření a publikování aplikace spravované katalogu služeb](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="a2183-300">For information about publishing a Service Catalog managed application, see [Create and publish a Service Catalog managed application](managed-application-publishing.md).</span></span>
* <span data-ttu-id="a2183-301">Informace o použití katalogu služeb spravovaných aplikací najdete v tématu [využívat katalogu služeb, které jsou spravované aplikace](managed-application-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="a2183-301">For information about consuming a Service Catalog managed application, see [Consume a Service Catalog managed application](managed-application-consumption.md).</span></span>
