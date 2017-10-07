---
title: "aaaAzure spravované aplikace v hello Marketplace | Microsoft Docs"
description: "Popisuje Azure spravované aplikace, které jsou k dispozici prostřednictvím hello Marketplace."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/09/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: b3cdf3f1fccdd47db699e4892ae8bce35118bfd8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-managed-applications-in-hello-marketplace"></a><span data-ttu-id="b6289-103">Azure spravované aplikace v hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="b6289-103">Azure managed applications in hello Marketplace</span></span>

 <span data-ttu-id="b6289-104">MSPs, nezávislí dodavatelé softwaru a systémových integrátorech (si) můžete použít Azure spravované aplikace toooffer zákazníků řešení tooall Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="b6289-104">MSPs, ISVs, and system integrators (SIs) can use Azure managed applications toooffer their solutions tooall Azure Marketplace customers.</span></span> <span data-ttu-id="b6289-105">Taková řešení snížit hello údržby a režijní náklady na údržbu zákazníků.</span><span class="sxs-lookup"><span data-stu-id="b6289-105">Such solutions reduce hello maintenance and servicing overhead for customers.</span></span> <span data-ttu-id="b6289-106">Vydavatelé můžete prodeje, infrastruktury a software prostřednictvím hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="b6289-106">Publishers can sell infrastructure and software through hello Marketplace.</span></span> <span data-ttu-id="b6289-107">Můžete se připojit služeb a aplikací toomanaged provozní podpory.</span><span class="sxs-lookup"><span data-stu-id="b6289-107">They can attach services and operational support toomanaged applications.</span></span> <span data-ttu-id="b6289-108">Další informace najdete v tématu [spravované aplikace přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b6289-108">For more information, see [Managed application overview](managed-application-overview.md).</span></span>

<span data-ttu-id="b6289-109">Tento článek vysvětluje, jak můžete publikovat aplikaci toohello Marketplace a nastavit jej jako široce dostupné toocustomers MSP, ISV nebo serveru.</span><span class="sxs-lookup"><span data-stu-id="b6289-109">This article explains how an MSP, ISV, or SI can publish an application toohello Marketplace and make it broadly available toocustomers.</span></span>

## <a name="prerequisites-for-publishing-a-managed-application"></a><span data-ttu-id="b6289-110">Předpoklady pro publikování spravované aplikace</span><span class="sxs-lookup"><span data-stu-id="b6289-110">Prerequisites for publishing a managed application</span></span>

<span data-ttu-id="b6289-111">Požadavky toolisting v hello Marketplace:</span><span class="sxs-lookup"><span data-stu-id="b6289-111">Prerequisites toolisting in hello Marketplace:</span></span>

* <span data-ttu-id="b6289-112">Technické</span><span class="sxs-lookup"><span data-stu-id="b6289-112">Technical</span></span>

    *  <span data-ttu-id="b6289-113">Informace o základní strukturu hello a syntaxe šablon Azure Resource Manageru najdete v tématu [šablon Azure Resource Manageru](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="b6289-113">For information about hello basic structure and syntax of Azure Resource Manager templates, see [Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
    *  <span data-ttu-id="b6289-114">tooview úplnou šablonu řešení, najdete v části [šablony Azure Quickstart](https://azure.microsoft.com/en-us/documentation/templates/) nebo hello [úložiště šablony rychlý Start](https://github.com/azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="b6289-114">tooview complete template solutions, see [Azure Quickstart templates](https://azure.microsoft.com/en-us/documentation/templates/) or hello [Quickstart template repository](https://github.com/azure/azure-quickstart-templates).</span></span>
    *  <span data-ttu-id="b6289-115">Informace o tom, jak toocreate hello rozhraní pro zákazníky, kteří nasazují aplikace prostřednictvím hello Marketplace najdete v tématu [vytvoření uživatelského rozhraní definice souboru](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b6289-115">For information about how toocreate hello interface for customers who deploy your application through hello Marketplace, see [Create a user interface definition file](managed-application-createuidefinition-overview.md).</span></span>

* <span data-ttu-id="b6289-116">Běžné uživatele (obchodní požadavky)</span><span class="sxs-lookup"><span data-stu-id="b6289-116">Nontechnical (business requirements)</span></span>

    *   <span data-ttu-id="b6289-117">Vaše společnost nebo její dceřiné společnosti, musí být umístěny v určité zemi, kde je podporováno prodeje podle hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="b6289-117">Your company or its subsidiary must be located in a country where sales are supported by hello Marketplace.</span></span>
    *   <span data-ttu-id="b6289-118">Tento produkt musí mít licenci způsobem, který je kompatibilní s modely fakturace nepodporuje hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="b6289-118">Your product must be licensed in a way that is compatible with billing models supported by hello Marketplace.</span></span>
    *   <span data-ttu-id="b6289-119">Jste zajišťuje, aby se na technickou podporu k dispozici toocustomers vyvineme způsobem.</span><span class="sxs-lookup"><span data-stu-id="b6289-119">You're responsible for making technical support available toocustomers in a commercially reasonable manner.</span></span> <span data-ttu-id="b6289-120">Podpora Hello můžete být volná, placené, nebo prostřednictvím komunity podporovat.</span><span class="sxs-lookup"><span data-stu-id="b6289-120">hello support can be free, paid, or through community support.</span></span>
    *   <span data-ttu-id="b6289-121">Jste zodpovědní za licencování softwaru a všechny závislosti software třetích stran.</span><span class="sxs-lookup"><span data-stu-id="b6289-121">You're responsible for licensing your software and any third-party software dependencies.</span></span>
    *   <span data-ttu-id="b6289-122">Je nutné zadat, že obsah, který splňuje kritéria pro vaši nabídku toobe uvedené v hello Marketplace a hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b6289-122">You must provide content that meets criteria for your offering toobe listed in hello Marketplace and in hello Azure portal.</span></span>
    *   <span data-ttu-id="b6289-123">Podmínky toohello hello Azure Marketplace zapojení zásad a vydavatele smlouvu, musíte souhlasit.</span><span class="sxs-lookup"><span data-stu-id="b6289-123">You must agree toohello terms of hello Azure Marketplace Participation Policies and Publisher Agreement.</span></span>
    *   <span data-ttu-id="b6289-124">Toocomply s hello podmínky použití, prohlášení o ochraně osobních údajů Microsoft a certifikované smlouvy programu služby Microsoft Azure, musíte souhlasit.</span><span class="sxs-lookup"><span data-stu-id="b6289-124">You must agree toocomply with hello Terms of Use, Microsoft Privacy Statement, and Microsoft Azure Certified Program Agreement.</span></span>

## <a name="create-a-new-azure-application-offer"></a><span data-ttu-id="b6289-125">Vytvořit novou aplikaci Azure nabídka</span><span class="sxs-lookup"><span data-stu-id="b6289-125">Create a new Azure application offer</span></span>

<span data-ttu-id="b6289-126">Pokud jsou splněny požadavky hello jste připravené toocreate vaši nabídku spravované aplikace.</span><span class="sxs-lookup"><span data-stu-id="b6289-126">After you meet hello prerequisites, you're ready toocreate your managed application offer.</span></span> <span data-ttu-id="b6289-127">Podívejme rychlý přehled o nabídku a SKU.</span><span class="sxs-lookup"><span data-stu-id="b6289-127">Let's take a quick overview of an offer and a SKU.</span></span>

### <a name="offer"></a><span data-ttu-id="b6289-128">Nabídka</span><span class="sxs-lookup"><span data-stu-id="b6289-128">Offer</span></span>

<span data-ttu-id="b6289-129">Hello nabídka pro spravované aplikace odpovídá tooa třída produktu nabídky od vydavatele.</span><span class="sxs-lookup"><span data-stu-id="b6289-129">hello offer for a managed application corresponds tooa class of product offering from a publisher.</span></span> <span data-ttu-id="b6289-130">Pokud máte nový typ řešení nebo aplikace, které chcete toomake k dispozici v hello Marketplace, můžete ho nastavit jako novou nabídku.</span><span class="sxs-lookup"><span data-stu-id="b6289-130">If you have a new type of solution/application that you want toomake available in hello Marketplace, you can set it up as a new offer.</span></span> <span data-ttu-id="b6289-131">Nabídka je kolekce SKU.</span><span class="sxs-lookup"><span data-stu-id="b6289-131">An offer is a collection of SKUs.</span></span> <span data-ttu-id="b6289-132">Všechny nabídky se zobrazí jako vlastní entity v hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="b6289-132">Every offer appears as its own entity in hello Marketplace.</span></span>

### <a name="sku"></a><span data-ttu-id="b6289-133">Skladová jednotka (SKU)</span><span class="sxs-lookup"><span data-stu-id="b6289-133">SKU</span></span>

<span data-ttu-id="b6289-134">SKU je hello nejmenší nabízené ke koupi jednotka nabídku.</span><span class="sxs-lookup"><span data-stu-id="b6289-134">A SKU is hello smallest purchasable unit of an offer.</span></span> <span data-ttu-id="b6289-135">Můžete použít SKU v rámci hello stejné toodifferentiate – třída (nabídka) produktu mezi:</span><span class="sxs-lookup"><span data-stu-id="b6289-135">You can use a SKU within hello same product class (offer) toodifferentiate between:</span></span>

* <span data-ttu-id="b6289-136">Různé funkce, které jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="b6289-136">Different features that are supported.</span></span>
* <span data-ttu-id="b6289-137">Nabídka hello toho, jestli spravované nebo nespravované.</span><span class="sxs-lookup"><span data-stu-id="b6289-137">Whether hello offer is managed or unmanaged.</span></span>
* <span data-ttu-id="b6289-138">Modely fakturace, které jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="b6289-138">Billing models that are supported.</span></span>

<span data-ttu-id="b6289-139">SKU se zobrazí pod hello nadřazené nabídka v hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="b6289-139">A SKU appears under hello parent offer in hello Marketplace.</span></span> <span data-ttu-id="b6289-140">Zobrazí se jako vlastní nabízené ke koupi entity v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b6289-140">It appears as its own purchasable entity in hello Azure portal.</span></span>

### <a name="set-up-an-offer"></a><span data-ttu-id="b6289-141">Nastavit nabídky</span><span class="sxs-lookup"><span data-stu-id="b6289-141">Set up an offer</span></span>

1. <span data-ttu-id="b6289-142">Přihlaste se toohello [portál Cloud Partner](https://cloudpartner.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b6289-142">Sign in toohello [Cloud Partner portal](https://cloudpartner.azure.com/).</span></span>

2. <span data-ttu-id="b6289-143">V navigačním podokně hello na levé straně hello vyberte **+ nové nabídky** > **aplikací Azure**.</span><span class="sxs-lookup"><span data-stu-id="b6289-143">In hello navigation pane on hello left, select **+ New offer** > **Azure Applications**.</span></span>

    ![Nová nabídka](./media/managed-application-author-marketplace/newOffer.png)

3. <span data-ttu-id="b6289-145">Vyplnění hello formulářů, které se zobrazují na hello left v hello **Editor** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="b6289-145">Fill out hello forms that appear on hello left in hello **Editor** view.</span></span> <span data-ttu-id="b6289-146">Povinná pole jsou označené červenou hvězdičkou (*).</span><span class="sxs-lookup"><span data-stu-id="b6289-146">Required fields are marked with a red asterisk (*).</span></span>

    ![Nabídka nastavení](./media/managed-application-author-marketplace/newOffer_OfferSettings.png)

    <span data-ttu-id="b6289-148">Použít toocreate spravované aplikace jsou čtyři hlavní formuláře:</span><span class="sxs-lookup"><span data-stu-id="b6289-148">Four main forms are used toocreate a managed application:</span></span>

    <span data-ttu-id="b6289-149">a.</span><span class="sxs-lookup"><span data-stu-id="b6289-149">a.</span></span> <span data-ttu-id="b6289-150">Nabídka nastavení</span><span class="sxs-lookup"><span data-stu-id="b6289-150">Offer Settings</span></span>

    <span data-ttu-id="b6289-151">b.</span><span class="sxs-lookup"><span data-stu-id="b6289-151">b.</span></span> <span data-ttu-id="b6289-152">SKU</span><span class="sxs-lookup"><span data-stu-id="b6289-152">SKUs</span></span>

    <span data-ttu-id="b6289-153">c.</span><span class="sxs-lookup"><span data-stu-id="b6289-153">c.</span></span> <span data-ttu-id="b6289-154">Marketplace</span><span class="sxs-lookup"><span data-stu-id="b6289-154">Marketplace</span></span>

    <span data-ttu-id="b6289-155">d.</span><span class="sxs-lookup"><span data-stu-id="b6289-155">d.</span></span> <span data-ttu-id="b6289-156">Podpora</span><span class="sxs-lookup"><span data-stu-id="b6289-156">Support</span></span>

<span data-ttu-id="b6289-157">Tyto formuláře jsou popsané v následující části hello podrobněji.</span><span class="sxs-lookup"><span data-stu-id="b6289-157">These forms are described in greater detail in hello following sections.</span></span>

## <a name="offer-settings-form"></a><span data-ttu-id="b6289-158">Nabídka nastavení formuláře</span><span class="sxs-lookup"><span data-stu-id="b6289-158">Offer Settings form</span></span>
<span data-ttu-id="b6289-159">Použijte tento základní formulář toospecify hello nabídky nastavení.</span><span class="sxs-lookup"><span data-stu-id="b6289-159">Use this basic form toospecify hello offer settings.</span></span>

1. <span data-ttu-id="b6289-160">Vyplňte hello **nabízejí nastavení** formuláře.</span><span class="sxs-lookup"><span data-stu-id="b6289-160">Fill in hello **Offer Settings** form.</span></span> <span data-ttu-id="b6289-161">Hello různých polí jsou:</span><span class="sxs-lookup"><span data-stu-id="b6289-161">hello different fields are:</span></span>

    <span data-ttu-id="b6289-162">a.</span><span class="sxs-lookup"><span data-stu-id="b6289-162">a.</span></span> <span data-ttu-id="b6289-163">**ID nabídky**: Tento jedinečný identifikátor identifikuje hello nabídka v rámci profilu vydavatele.</span><span class="sxs-lookup"><span data-stu-id="b6289-163">**Offer ID**: This unique identifier identifies hello offer within a publisher profile.</span></span> <span data-ttu-id="b6289-164">Toto ID je viditelná v adresách URL produktu, šablony Resource Manageru a fakturace sestavy.</span><span class="sxs-lookup"><span data-stu-id="b6289-164">This ID is visible in product URLs, Resource Manager templates, and billing reports.</span></span> <span data-ttu-id="b6289-165">Může být složené jenom malé alfanumerické znaky nebo pomlčky (-).</span><span class="sxs-lookup"><span data-stu-id="b6289-165">It can only be composed of lowercase alphanumeric characters or dashes (-).</span></span> <span data-ttu-id="b6289-166">Hello ID nemůže končit pomlčkou.</span><span class="sxs-lookup"><span data-stu-id="b6289-166">hello ID can't end in a dash.</span></span> <span data-ttu-id="b6289-167">Je omezená tooa maximálně 50 znaků.</span><span class="sxs-lookup"><span data-stu-id="b6289-167">It's limited tooa maximum of 50 characters.</span></span> <span data-ttu-id="b6289-168">Po nabídku přejde za provozu, toto pole je uzamčené.</span><span class="sxs-lookup"><span data-stu-id="b6289-168">After an offer goes live, this field is locked.</span></span>

    <span data-ttu-id="b6289-169">b.</span><span class="sxs-lookup"><span data-stu-id="b6289-169">b.</span></span> <span data-ttu-id="b6289-170">**ID vydavatele**: pomocí tohoto rozevíracího seznamu toochoose hello vydavatele profilu chcete toopublish v rámci této nabídky.</span><span class="sxs-lookup"><span data-stu-id="b6289-170">**Publisher ID**: Use this drop-down list toochoose hello publisher profile you want toopublish this offer under.</span></span> <span data-ttu-id="b6289-171">Po nabídku přejde za provozu, toto pole je uzamčené.</span><span class="sxs-lookup"><span data-stu-id="b6289-171">After an offer goes live, this field is locked.</span></span>

    <span data-ttu-id="b6289-172">c.</span><span class="sxs-lookup"><span data-stu-id="b6289-172">c.</span></span> <span data-ttu-id="b6289-173">**Název**: Tento název zobrazení pro danou nabídku se zobrazí v hello Marketplace a hello portálu.</span><span class="sxs-lookup"><span data-stu-id="b6289-173">**Name**: This display name for your offer appears in hello Marketplace and in hello portal.</span></span> <span data-ttu-id="b6289-174">Může mít maximálně 50 znaků.</span><span class="sxs-lookup"><span data-stu-id="b6289-174">It can have a maximum of 50 characters.</span></span> <span data-ttu-id="b6289-175">Zahrnují srozumitelný název značky pro svůj produkt.</span><span class="sxs-lookup"><span data-stu-id="b6289-175">Include a recognizable brand name for your product.</span></span> <span data-ttu-id="b6289-176">Nezadávejte název společnosti, pokud to není, jak je na trh.</span><span class="sxs-lookup"><span data-stu-id="b6289-176">Don't include your company name here unless that's how it's marketed.</span></span> <span data-ttu-id="b6289-177">Pokud jste marketingové v rámci této nabídky na vlastní web, ujistěte se, zda text hello jméno přesně jak se zobrazuje na vašem webu.</span><span class="sxs-lookup"><span data-stu-id="b6289-177">If you're marketing this offer on your own website, ensure that hello name is exactly how it appears on your website.</span></span>

2. <span data-ttu-id="b6289-178">Vyberte **Uložit** toosave průběh.</span><span class="sxs-lookup"><span data-stu-id="b6289-178">Select **Save** toosave your progress.</span></span> 

## <a name="skus-form"></a><span data-ttu-id="b6289-179">SKU formuláře</span><span class="sxs-lookup"><span data-stu-id="b6289-179">SKUs form</span></span>
<span data-ttu-id="b6289-180">dalším krokem Hello je tooadd SKU pro vaši nabídku.</span><span class="sxs-lookup"><span data-stu-id="b6289-180">hello next step is tooadd SKUs for your offer.</span></span>

1. <span data-ttu-id="b6289-181">Vyberte **SKU** > **nové SKU**.</span><span class="sxs-lookup"><span data-stu-id="b6289-181">Select **SKUs** > **New SKU**.</span></span> 

    ![Vyberte nové SKU](./media/managed-application-author-marketplace/newOffer_skus.png)

2. <span data-ttu-id="b6289-183">Zadejte **SKU ID**.</span><span class="sxs-lookup"><span data-stu-id="b6289-183">Enter a **SKU ID**.</span></span> <span data-ttu-id="b6289-184">SKU ID je jedinečný identifikátor SKU hello v rámci nabídku.</span><span class="sxs-lookup"><span data-stu-id="b6289-184">A SKU ID is a unique identifier for hello SKU within an offer.</span></span> <span data-ttu-id="b6289-185">Toto ID je viditelná v adresách URL produktu, šablony Resource Manageru a fakturace sestavy.</span><span class="sxs-lookup"><span data-stu-id="b6289-185">This ID is visible in product URLs, Resource Manager templates, and billing reports.</span></span> <span data-ttu-id="b6289-186">Může být složené jenom malé alfanumerické znaky nebo pomlčky (-).</span><span class="sxs-lookup"><span data-stu-id="b6289-186">It can only be composed of lowercase alphanumeric characters or dashes (-).</span></span> <span data-ttu-id="b6289-187">Hello ID nemůže končit pomlčkou a je omezený tooa maximálně 50 znaků.</span><span class="sxs-lookup"><span data-stu-id="b6289-187">hello ID can't end in a dash, and it's limited tooa maximum of 50 characters.</span></span> <span data-ttu-id="b6289-188">Po nabídku přejde za provozu, toto pole je uzamčené.</span><span class="sxs-lookup"><span data-stu-id="b6289-188">After an offer goes live, this field is locked.</span></span> <span data-ttu-id="b6289-189">Můžete mít více SKU v rámci nabídku.</span><span class="sxs-lookup"><span data-stu-id="b6289-189">You can have multiple SKUs within an offer.</span></span> <span data-ttu-id="b6289-190">Je nutné SKU pro každou bitovou kopii naplánovat toopublish.</span><span class="sxs-lookup"><span data-stu-id="b6289-190">You need a SKU for each image you plan toopublish.</span></span>

3. <span data-ttu-id="b6289-191">Vyplňte hello **SKU podrobnosti** část hello následující formulář:</span><span class="sxs-lookup"><span data-stu-id="b6289-191">Fill out hello **SKU Details** section on hello following form:</span></span>

    ![Zadejte nové SKU](./media/managed-application-author-marketplace/newOffer_newsku.png)

    <span data-ttu-id="b6289-193">Vyplňte následující pole hello:</span><span class="sxs-lookup"><span data-stu-id="b6289-193">Fill out hello following fields:</span></span>
    
    <span data-ttu-id="b6289-194">a.</span><span class="sxs-lookup"><span data-stu-id="b6289-194">a.</span></span> <span data-ttu-id="b6289-195">**Název**: Zadejte název pro tento SKU.</span><span class="sxs-lookup"><span data-stu-id="b6289-195">**Title**: Enter a title for this SKU.</span></span> <span data-ttu-id="b6289-196">Tento název se zobrazí v galerii hello pro tuto položku.</span><span class="sxs-lookup"><span data-stu-id="b6289-196">This title appears in hello gallery for this item.</span></span>

    <span data-ttu-id="b6289-197">b.</span><span class="sxs-lookup"><span data-stu-id="b6289-197">b.</span></span> <span data-ttu-id="b6289-198">**Souhrn**: Zadejte krátký souhrn pro tento SKU.</span><span class="sxs-lookup"><span data-stu-id="b6289-198">**Summary**: Enter a short summary for this SKU.</span></span> <span data-ttu-id="b6289-199">Tento text se zobrazí pod položkou Název hello.</span><span class="sxs-lookup"><span data-stu-id="b6289-199">This text appears underneath hello title.</span></span>

    <span data-ttu-id="b6289-200">c.</span><span class="sxs-lookup"><span data-stu-id="b6289-200">c.</span></span> <span data-ttu-id="b6289-201">**Popis**: Zadejte podrobný popis o hello SKU.</span><span class="sxs-lookup"><span data-stu-id="b6289-201">**Description**: Enter a detailed description about hello SKU.</span></span>

    <span data-ttu-id="b6289-202">d.</span><span class="sxs-lookup"><span data-stu-id="b6289-202">d.</span></span> <span data-ttu-id="b6289-203">**Typ SKU**: hello povolené hodnoty jsou **spravované aplikace** a **šablony řešení**.</span><span class="sxs-lookup"><span data-stu-id="b6289-203">**SKU Type**: hello allowed values are **Managed Application** and **Solution Templates**.</span></span> <span data-ttu-id="b6289-204">Pro tento případ, vyberte **spravované aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b6289-204">For this case, select **Managed Application**.</span></span>

4. <span data-ttu-id="b6289-205">Vyplňte hello **podrobnosti balíčku** část hello následující formulář:</span><span class="sxs-lookup"><span data-stu-id="b6289-205">Fill out hello **Package Details** section on hello following form:</span></span>

    ![Balíček](./media/managed-application-author-marketplace/newOffer_newsku_package.png)

    <span data-ttu-id="b6289-207">Vyplňte následující pole hello:</span><span class="sxs-lookup"><span data-stu-id="b6289-207">Fill out hello following fields:</span></span>

    <span data-ttu-id="b6289-208">a.</span><span class="sxs-lookup"><span data-stu-id="b6289-208">a.</span></span> <span data-ttu-id="b6289-209">**Aktuální verze**: Zadejte verzi balíčku hello můžete nahrávat na server.</span><span class="sxs-lookup"><span data-stu-id="b6289-209">**Current Version**: Enter a version for hello package you upload.</span></span> <span data-ttu-id="b6289-210">Musí být ve formátu hello `{number}.{number}.{number}{number}`.</span><span class="sxs-lookup"><span data-stu-id="b6289-210">It should be in hello format `{number}.{number}.{number}{number}`.</span></span>

    <span data-ttu-id="b6289-211">b.</span><span class="sxs-lookup"><span data-stu-id="b6289-211">b.</span></span> <span data-ttu-id="b6289-212">**Vyberte soubor balíčku**: Tento balíček obsahuje následující soubory, které jsou komprimované a do souboru .zip hello:</span><span class="sxs-lookup"><span data-stu-id="b6289-212">**Select a package file**: This package contains hello following files that are compressed into a .zip file:</span></span>
    * <span data-ttu-id="b6289-213">**applianceMainTemplate.json**: hello soubor šablony nasazení, který byl použit toodeploy hello řešení nebo aplikace.</span><span class="sxs-lookup"><span data-stu-id="b6289-213">**applianceMainTemplate.json**: hello deployment template file that's used toodeploy hello solution/application.</span></span> <span data-ttu-id="b6289-214">Informace o tom najdete v části soubory šablony nasazení toocreate, [vytvoření vaší první šablony Azure Resource Manager](resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="b6289-214">For information about how toocreate deployment template files, see [Create your first Azure Resource Manager template](resource-manager-create-first-template.md).</span></span>
    * <span data-ttu-id="b6289-215">**appliancecreateUIDefinition.json**: Tento soubor je používán hello Azure portálu toogenerate hello uživatelské rozhraní, které se používá tooprovision toto řešení aplikace.</span><span class="sxs-lookup"><span data-stu-id="b6289-215">**appliancecreateUIDefinition.json**: This file is used by hello Azure portal toogenerate hello user interface that's used tooprovision this solution/application.</span></span> <span data-ttu-id="b6289-216">Další informace najdete v tématu [začít pracovat s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b6289-216">For more information, see [Get started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
    * <span data-ttu-id="b6289-217">**mainTemplate.json**: Tento soubor šablony obsahuje pouze hello Microsoft.Solution/appliances prostředků.</span><span class="sxs-lookup"><span data-stu-id="b6289-217">**mainTemplate.json**: This template file contains only hello Microsoft.Solution/appliances resource.</span></span> <span data-ttu-id="b6289-218">Hello mainTemplate soubor obsahuje hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="b6289-218">hello mainTemplate file includes hello following properties:</span></span>

        *  <span data-ttu-id="b6289-219">**druh**: použití **Marketplace** pro spravované aplikace v hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="b6289-219">**kind**: Use **Marketplace** for managed applications in hello Marketplace.</span></span>
        *  <span data-ttu-id="b6289-220">**ManagedResourceGroupId**: Tato skupina prostředků v předplatném hello zákazníka je, kde jsou všechny prostředky hello definované v applianceMainTemplate.json nasazen.</span><span class="sxs-lookup"><span data-stu-id="b6289-220">**ManagedResourceGroupId**: This resource group in hello customer's subscription is where all hello resources defined in applianceMainTemplate.json are deployed.</span></span>
        *  <span data-ttu-id="b6289-221">**PublisherPackageId**: Tento řetězec jednoznačně identifikuje hello balíčku.</span><span class="sxs-lookup"><span data-stu-id="b6289-221">**PublisherPackageId**: This string uniquely identifies hello package.</span></span> <span data-ttu-id="b6289-222">Zadejte hodnotu hello ve formátu hello `{publisherId}.{OfferId}.{SKUID}.{PackageVersion}`.</span><span class="sxs-lookup"><span data-stu-id="b6289-222">Provide hello value in hello format of `{publisherId}.{OfferId}.{SKUID}.{PackageVersion}`.</span></span>

<span data-ttu-id="b6289-223">Získat hello **nabízejí ID** a **ID vydavatele** z hello publikování portálu, jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="b6289-223">Obtain hello **Offer ID** and **Publisher ID** from hello publishing portal, as shown in hello following image:</span></span>

![ID nabídky](./media/managed-application-author-marketplace/UniqueString_pubid_offerid.png)
        
<span data-ttu-id="b6289-225">Získat hello **SKU ID**, jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="b6289-225">Obtain hello **SKU ID**, as shown in hello following image:</span></span>

![SKU ID](./media/managed-application-author-marketplace/UniqueString_skuid.png)
        
<span data-ttu-id="b6289-227">Získat balíček hello **verze**, jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="b6289-227">Obtain hello package **Version**, as shown in hello following image:</span></span>

![Verze balíčku](./media/managed-application-author-marketplace/UniqueString_packageversion.png)
    
  <span data-ttu-id="b6289-229">Podle předchozích příklady hello, hello hodnotu **PublisherPackageId** je `azureappliance-test.ravmanagedapptest.ravpreviewmanagedsku.1.0.0`.</span><span class="sxs-lookup"><span data-stu-id="b6289-229">Based on hello preceding examples, hello value of **PublisherPackageId** is `azureappliance-test.ravmanagedapptest.ravpreviewmanagedsku.1.0.0`.</span></span>

  <span data-ttu-id="b6289-230">Ukázka mainTemplate.json:</span><span class="sxs-lookup"><span data-stu-id="b6289-230">Sample mainTemplate.json:</span></span>

  ```json
  {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "storageAccountNamePrefix": {
        "type": "string",
        "metadata": {
          "description": "Specify hello name of hello storage account"
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

<span data-ttu-id="b6289-231">Tento balíček měl obsahovat všechny vnořené šablony nebo skripty, které jsou požadované toosuccessfully zřídit této aplikace.</span><span class="sxs-lookup"><span data-stu-id="b6289-231">This package should contain any other nested templates or scripts that are required toosuccessfully provision this application.</span></span> <span data-ttu-id="b6289-232">Hello mainTemplate.json, applianceMainTemplate.json a applianceCreateUIDefinition.json soubory musí být umístěny v kořenové složce hello.</span><span class="sxs-lookup"><span data-stu-id="b6289-232">hello mainTemplate.json, applianceMainTemplate.json, and applianceCreateUIDefinition.json files must be present at hello root folder.</span></span>

* <span data-ttu-id="b6289-233">**Povolení**: definuje tato vlastnost, který získá přístup a hello úroveň přístupu toohello prostředky v rámci předplatných zákazníků.</span><span class="sxs-lookup"><span data-stu-id="b6289-233">**Authorizations**: This property defines who gets access and hello level of access toohello resources in customers' subscriptions.</span></span> <span data-ttu-id="b6289-234">Vydavatel Hello ho můžete použít aplikace hello toomanage jménem zákazníka hello.</span><span class="sxs-lookup"><span data-stu-id="b6289-234">hello publisher can use it toomanage hello application on behalf of hello customer.</span></span>
* <span data-ttu-id="b6289-235">**PrincipalId**: Tato vlastnost je identifikátor hello Azure Active Directory (Azure AD) uživatele, skupiny uživatelů nebo aplikací, který má oprávnění určité hello prostředky v předplatné hello zákazníka.</span><span class="sxs-lookup"><span data-stu-id="b6289-235">**PrincipalId**: This property is hello Azure Active Directory (Azure AD) identifier of a user, user group, or application that's granted certain permissions on hello resources in hello customer's subscription.</span></span> <span data-ttu-id="b6289-236">Hello definice Role popisuje oprávnění, hello.</span><span class="sxs-lookup"><span data-stu-id="b6289-236">hello Role Definition describes hello permissions.</span></span> 
* <span data-ttu-id="b6289-237">**Definice role**: Tato vlastnost je seznam všech hello předdefinované řízení přístupu na základě Role (RBAC) rolí poskytuje Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b6289-237">**Role Definition**: This property is a list of all hello built-in Role-Based Access Control (RBAC) roles provided by Azure AD.</span></span> <span data-ttu-id="b6289-238">Můžete vybrat hello role, která je nejvhodnější toouse toomanage hello prostředky jménem zákazníka hello.</span><span class="sxs-lookup"><span data-stu-id="b6289-238">You can select hello role that's most appropriate toouse toomanage hello resources on behalf of hello customer.</span></span>

<span data-ttu-id="b6289-239">Můžete přidat více oprávnění.</span><span class="sxs-lookup"><span data-stu-id="b6289-239">You can add multiple authorizations.</span></span> <span data-ttu-id="b6289-240">Doporučujeme vytvořit skupinu uživatele AD a zadejte své ID v **PrincipalId**.</span><span class="sxs-lookup"><span data-stu-id="b6289-240">We recommend that you create an AD user group and specify its ID in **PrincipalId**.</span></span> <span data-ttu-id="b6289-241">Tímto způsobem můžete přidat další skupiny uživatelů toohello uživatelé bez hello nutné tooupdate hello SKU.</span><span class="sxs-lookup"><span data-stu-id="b6289-241">This way, you can add more users toohello user group without hello need tooupdate hello SKU.</span></span>

<span data-ttu-id="b6289-242">Další informace o RBAC najdete v tématu [začít pracovat s RBAC v hello portál Azure](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="b6289-242">For more information about RBAC, see [Get started with RBAC in hello Azure portal](../active-directory/role-based-access-control-what-is.md).</span></span>

## <a name="marketplace-form"></a><span data-ttu-id="b6289-243">Formulář Marketplace.</span><span class="sxs-lookup"><span data-stu-id="b6289-243">Marketplace form</span></span>

<span data-ttu-id="b6289-244">Hello Marketplace formuláře požádá o pole, které se zobrazí na hello [Azure Marketplace](https://azuremarketplace.microsoft.com) a na hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b6289-244">hello Marketplace form asks for fields that show up on hello [Azure Marketplace](https://azuremarketplace.microsoft.com) and on hello [Azure portal](https://portal.azure.com/).</span></span>

### <a name="preview-subscription-ids"></a><span data-ttu-id="b6289-245">ID odběru Preview</span><span class="sxs-lookup"><span data-stu-id="b6289-245">Preview subscription IDs</span></span>

<span data-ttu-id="b6289-246">Zadejte seznam předplatného Azure ID, kteří mohou přistupovat k hello nabídka po publikování.</span><span class="sxs-lookup"><span data-stu-id="b6289-246">Enter a list of Azure subscription IDs that can access hello offer after it's published.</span></span> <span data-ttu-id="b6289-247">Můžete použít tyto odběry uvedené prázdné tootest hello zobrazení náhledu nabídka před jeho provedením za provozu.</span><span class="sxs-lookup"><span data-stu-id="b6289-247">You can use these white-listed subscriptions tootest hello previewed offer before you make it live.</span></span> <span data-ttu-id="b6289-248">Prázdný seznam až too100 předplatná v portálu pro partnery hello můžete zkompilovat.</span><span class="sxs-lookup"><span data-stu-id="b6289-248">You can compile a white list of up too100 subscriptions in hello partner portal.</span></span>

### <a name="suggested-categories"></a><span data-ttu-id="b6289-249">Navrhované kategorií</span><span class="sxs-lookup"><span data-stu-id="b6289-249">Suggested categories</span></span>

<span data-ttu-id="b6289-250">Vyberte kategorie toofive hello seznamu, které vaši nabídku může být nejlépe přidružené.</span><span class="sxs-lookup"><span data-stu-id="b6289-250">Select up toofive categories from hello list that your offer can be best associated with.</span></span> <span data-ttu-id="b6289-251">Tyto kategorie jsou použité toomap kategorie produktů toohello vaší nabídky, které jsou k dispozici v hello [Azure Marketplace](https://azuremarketplace.microsoft.com) a hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b6289-251">These categories are used toomap your offer toohello product categories that are available in hello [Azure Marketplace](https://azuremarketplace.microsoft.com) and hello [Azure portal](https://portal.azure.com/).</span></span>

#### <a name="azure-marketplace"></a><span data-ttu-id="b6289-252">Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="b6289-252">Azure Marketplace</span></span>

<span data-ttu-id="b6289-253">Zobrazí souhrn Hello spravované aplikace hello následující pole:</span><span class="sxs-lookup"><span data-stu-id="b6289-253">hello summary of your managed application displays hello following fields:</span></span>

![Souhrn Marketplace](./media/managed-application-author-marketplace/publishvm10.png)

<span data-ttu-id="b6289-255">Hello **přehled** kartě pro zobrazení spravované aplikace hello následující pole:</span><span class="sxs-lookup"><span data-stu-id="b6289-255">hello **Overview** tab for your managed application displays hello following fields:</span></span>

![Přehled webu Marketplace](./media/managed-application-author-marketplace/publishvm11.png)

<span data-ttu-id="b6289-257">Hello **plány + cenová** kartě pro zobrazení spravované aplikace hello následující pole:</span><span class="sxs-lookup"><span data-stu-id="b6289-257">hello **Plans + Pricing** tab for your managed application displays hello following fields:</span></span>

![Plány Marketplace.](./media/managed-application-author-marketplace/publishvm15.png)

#### <a name="azure-portal"></a><span data-ttu-id="b6289-259">portál Azure</span><span class="sxs-lookup"><span data-stu-id="b6289-259">Azure portal</span></span>

<span data-ttu-id="b6289-260">Zobrazí souhrn Hello spravované aplikace hello následující pole:</span><span class="sxs-lookup"><span data-stu-id="b6289-260">hello summary of your managed application displays hello following fields:</span></span>

![Portál souhrn](./media/managed-application-author-marketplace/publishvm12.png)

<span data-ttu-id="b6289-262">Přehled Hello pro spravované aplikace zobrazí hello následující pole:</span><span class="sxs-lookup"><span data-stu-id="b6289-262">hello overview for your managed application displays hello following fields:</span></span>

![Přehled portálu](./media/managed-application-author-marketplace/publishvm13.png)

#### <a name="logo-guidelines"></a><span data-ttu-id="b6289-264">Logu</span><span class="sxs-lookup"><span data-stu-id="b6289-264">Logo guidelines</span></span>

<span data-ttu-id="b6289-265">Postupujte podle těchto pokynů pro všechny logo, který nahrajete na portálu Cloud Partner hello:</span><span class="sxs-lookup"><span data-stu-id="b6289-265">Follow these guidelines for any logo that you upload in hello Cloud Partner portal:</span></span>

*   <span data-ttu-id="b6289-266">Hello Azure návrhu má palety barev jednoduché.</span><span class="sxs-lookup"><span data-stu-id="b6289-266">hello Azure design has a simple color palette.</span></span> <span data-ttu-id="b6289-267">Omezit počet hello primární a sekundární barev v loga.</span><span class="sxs-lookup"><span data-stu-id="b6289-267">Limit hello number of primary and secondary colors on your logo.</span></span>
*   <span data-ttu-id="b6289-268">barev motivů Hello hello portálu jsou bílé a černé.</span><span class="sxs-lookup"><span data-stu-id="b6289-268">hello theme colors of hello portal are white and black.</span></span> <span data-ttu-id="b6289-269">Nepoužívejte tyto barvy jako barvu pozadí hello loga.</span><span class="sxs-lookup"><span data-stu-id="b6289-269">Don't use these colors as hello background color for your logo.</span></span> <span data-ttu-id="b6289-270">Použijte barvu, která umožňuje loga viditelného hello portálu.</span><span class="sxs-lookup"><span data-stu-id="b6289-270">Use a color that makes your logo prominent in hello portal.</span></span> <span data-ttu-id="b6289-271">Doporučujeme, abyste jednoduché primární barvy.</span><span class="sxs-lookup"><span data-stu-id="b6289-271">We recommend simple primary colors.</span></span> <span data-ttu-id="b6289-272">*Pokud používáte průhledné pozadí, ujistěte se, že hello logem a text nejsou bílé, černé nebo blue.*</span><span class="sxs-lookup"><span data-stu-id="b6289-272">*If you use a transparent background, make sure that hello logo and text aren't white, black, or blue.*</span></span>
*   <span data-ttu-id="b6289-273">Nepoužívejte barevného přechodu pozadí na hello logo.</span><span class="sxs-lookup"><span data-stu-id="b6289-273">Don't use a gradient background on hello logo.</span></span>
*   <span data-ttu-id="b6289-274">Neukládejte text do hello logo, ani vaše společnost nebo název značky.</span><span class="sxs-lookup"><span data-stu-id="b6289-274">Don't place text on hello logo, not even your company or brand name.</span></span> <span data-ttu-id="b6289-275">Hello vzhled a chování vaše logo musí být plochý a vyhnout se přechody.</span><span class="sxs-lookup"><span data-stu-id="b6289-275">hello look and feel of your logo should be flat and avoid gradients.</span></span>
*   <span data-ttu-id="b6289-276">Zajistěte, aby hello logo není roztažená.</span><span class="sxs-lookup"><span data-stu-id="b6289-276">Make sure hello logo isn't stretched.</span></span>

#### <a name="hero-logo"></a><span data-ttu-id="b6289-277">Logo nejdůležitější</span><span class="sxs-lookup"><span data-stu-id="b6289-277">Hero logo</span></span>

<span data-ttu-id="b6289-278">logo hrdina Hello je volitelné.</span><span class="sxs-lookup"><span data-stu-id="b6289-278">hello hero logo is optional.</span></span> <span data-ttu-id="b6289-279">Vydavatel Hello můžete zvolit není tooupload logo nejdůležitější.</span><span class="sxs-lookup"><span data-stu-id="b6289-279">hello publisher can choose not tooupload a hero logo.</span></span> <span data-ttu-id="b6289-280">Po odeslání hello hrdina ikonu nelze odstranit.</span><span class="sxs-lookup"><span data-stu-id="b6289-280">After hello hero icon is uploaded, it can't be deleted.</span></span> <span data-ttu-id="b6289-281">V ten moment se musí splňovat hello partnera hello Marketplace pravidla pro nejdůležitější ikony.</span><span class="sxs-lookup"><span data-stu-id="b6289-281">At that time, hello partner must follow hello Marketplace guidelines for hero icons.</span></span>

<span data-ttu-id="b6289-282">Postupujte podle těchto pokynů pro ikonu logo hrdina hello:</span><span class="sxs-lookup"><span data-stu-id="b6289-282">Follow these guidelines for hello hero logo icon:</span></span>

*   <span data-ttu-id="b6289-283">Název zobrazení Hello vydavatele, název plánu hello a hello nabídka dlouhé shrnutí jsou zobrazeny v prázdné.</span><span class="sxs-lookup"><span data-stu-id="b6289-283">hello publisher display name, hello plan title, and hello offer long summary are displayed in white.</span></span> <span data-ttu-id="b6289-284">Proto nepoužívejte světla barvu pozadí hello ikony hrdina hello.</span><span class="sxs-lookup"><span data-stu-id="b6289-284">Therefore, don't use a light color for hello background of hello hero icon.</span></span> <span data-ttu-id="b6289-285">Černé, bílé nebo průhledné pozadí není povolena pro nejdůležitější ikony.</span><span class="sxs-lookup"><span data-stu-id="b6289-285">A black, white, or transparent background isn't allowed for hero icons.</span></span>
*   <span data-ttu-id="b6289-286">Po hello nabídka je uvedena, hello vydavatele zobrazované jméno, název plánu hello, hello nabídka dlouhé shrnutí a hello **vytvořit** tlačítko jsou uvnitř hello hrdina logo vložených prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="b6289-286">After hello offer is listed, hello publisher display name, hello plan title, hello offer long summary, and hello **Create** button are embedded programmatically inside hello hero logo.</span></span> <span data-ttu-id="b6289-287">V důsledku toho není zadat libovolný text při návrhu logo hrdina hello.</span><span class="sxs-lookup"><span data-stu-id="b6289-287">Consequently, don't enter any text while you design hello hero logo.</span></span> <span data-ttu-id="b6289-288">Ponechte prázdné místo hello správné protože textu hello je do tohoto místa zahrnuto prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="b6289-288">Leave empty space on hello right because hello text is included programmatically in that space.</span></span> <span data-ttu-id="b6289-289">Hello prázdné místo pro hello text by mělo být 415 × 100 pixelů na hello správné.</span><span class="sxs-lookup"><span data-stu-id="b6289-289">hello empty space for hello text should be 415 x 100 pixels on hello right.</span></span> <span data-ttu-id="b6289-290">Posunut 370 pixelů zleva hello.</span><span class="sxs-lookup"><span data-stu-id="b6289-290">It's offset by 370 pixels from hello left.</span></span>

    ![Příklad logo nejdůležitější](./media/managed-application-author-marketplace/publishvm14.png)

## <a name="support-form"></a><span data-ttu-id="b6289-292">Podpora formuláře</span><span class="sxs-lookup"><span data-stu-id="b6289-292">Support form</span></span>

<span data-ttu-id="b6289-293">Vyplňte hello **podporu** formuláře s podporou kontaktuje z vaší společnosti.</span><span class="sxs-lookup"><span data-stu-id="b6289-293">Fill out hello **Support** form with support contacts from your company.</span></span> <span data-ttu-id="b6289-294">Tyto informace může technici, kontakty a kontakty podpory zákazníků.</span><span class="sxs-lookup"><span data-stu-id="b6289-294">This information might be engineering contacts and customer support contacts.</span></span>

## <a name="publish-an-offer"></a><span data-ttu-id="b6289-295">Publikování nabídky</span><span class="sxs-lookup"><span data-stu-id="b6289-295">Publish an offer</span></span>

<span data-ttu-id="b6289-296">Po vyplnění všech oddílů hello, vyberte **publikovat** toostart hello proces, který je k dispozici toocustomers vaší nabídky.</span><span class="sxs-lookup"><span data-stu-id="b6289-296">After you fill out all hello sections, select **Publish** toostart hello process that makes your offer available toocustomers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b6289-297">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b6289-297">Next steps</span></span>

* <span data-ttu-id="b6289-298">Úvod toomanaged aplikace naleznete v [spravované aplikace přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b6289-298">For an introduction toomanaged applications, see [Managed application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="b6289-299">Informace o použití spravovaných aplikací z hello Marketplace najdete v tématu [využívat Azure spravované aplikace v hello Marketplace](managed-application-consume-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="b6289-299">For information about consuming a managed application from hello Marketplace, see [Consume Azure managed applications in hello Marketplace](managed-application-consume-marketplace.md).</span></span>
* <span data-ttu-id="b6289-300">Informace o publikování aplikace spravované katalogu služeb najdete v tématu [vytvoření a publikování aplikace spravované katalogu služeb](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="b6289-300">For information about publishing a Service Catalog managed application, see [Create and publish a Service Catalog managed application](managed-application-publishing.md).</span></span>
* <span data-ttu-id="b6289-301">Informace o použití katalogu služeb spravovaných aplikací najdete v tématu [využívat katalogu služeb, které jsou spravované aplikace](managed-application-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="b6289-301">For information about consuming a Service Catalog managed application, see [Consume a Service Catalog managed application](managed-application-consumption.md).</span></span>
