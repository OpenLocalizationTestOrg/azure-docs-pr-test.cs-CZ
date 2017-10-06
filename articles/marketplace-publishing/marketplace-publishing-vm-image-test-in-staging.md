---
title: "Nabídka služeb virtuálního počítače pro hello Marketplace aaaTest | Microsoft Docs"
description: "Pochopte, jak tootest virtuálního počítače obrázků pro hello Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 7a41c3c6-625c-4478-b804-e124dee89040
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2016
ms.author: hascipio
ms.openlocfilehash: ab166d2c3c536810a3a8f48330f0482b9b4e58d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="test-your-vm-offer-for-hello-azure-marketplace-in-staging"></a><span data-ttu-id="92439-103">Při přípravě otestovat vaši nabídku virtuálních počítačů pro hello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="92439-103">Test your VM offer for hello Azure Marketplace in staging</span></span>
<span data-ttu-id="92439-104">Pracovní znamená nasazení vaší SKU privátního "izolovaného", kde můžete testovat a ověřit jeho funkce ještě před nasazením toohello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="92439-104">Staging means deploying your SKU in a private “sandbox” where you can test and validate its functionality before deploying it toohello Marketplace.</span></span> <span data-ttu-id="92439-105">Hello SKU se zobrazí v pracovním stejně, jako by tooa zákazníkovi, který je nasazena.</span><span class="sxs-lookup"><span data-stu-id="92439-105">hello SKU appears in staging just as it would tooa customer who has deployed it.</span></span> <span data-ttu-id="92439-106">Bitové kopie virtuálního počítače musí být toostaging certifikovaných toobe poslat.</span><span class="sxs-lookup"><span data-stu-id="92439-106">Your VM image must be certified toobe pushed toostaging.</span></span>

## <a name="step-1-push-your-offer-toostaging"></a><span data-ttu-id="92439-107">Krok 1: Push vaší toostaging nabídka</span><span class="sxs-lookup"><span data-stu-id="92439-107">Step 1: Push your offer toostaging</span></span>
1. <span data-ttu-id="92439-108">Na hello **publikovat** , klikněte na **Push tooStaging**.</span><span class="sxs-lookup"><span data-stu-id="92439-108">On hello **Publish** tab, click **Push tooStaging**.</span></span>
   
    ![Kreslení](media/marketplace-publishing-vm-image-test-in-staging/vm-image-push-to-staging.png)
2. <span data-ttu-id="92439-110">Pokud hello publikování portál upozorňuje na všechny chyby, opravte je.</span><span class="sxs-lookup"><span data-stu-id="92439-110">If hello Publishing Portal notifies you of any errors, correct them.</span></span>
3. <span data-ttu-id="92439-111">V hello **přístup dvoufázové instalace nabídku?** dialogovém okně zadejte hello seznam předplatných Azure, že použijete toopreview vaši nabídku v hello [portál Azure preview](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="92439-111">In hello **Who can access your staged offer?** dialog box, enter hello list of Azure subscriptions that you will use toopreview your offer in hello [Azure preview portal](https://portal.azure.com).</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="92439-112">V případě virtuálních počítačů a šablony řešení prosím **nepodporují** povolených odběrů typu CSP, DreamSpark nebo Azure v otevřené.</span><span class="sxs-lookup"><span data-stu-id="92439-112">In case of Virtual Machines and Solution templates, please **do not** whitelist subscriptions of type CSP, DreamSpark or Azure in Open.</span></span>
   > 
   > 

    > <span data-ttu-id="92439-113">V případě virtuálních počítačů, když kliknete na tlačítko hello **nabízené tooSTAGING**, hello následující kroky jsou prováděny za scény hello.</span><span class="sxs-lookup"><span data-stu-id="92439-113">In case of Virtual Machines, when you click on hello button **PUSH tooSTAGING**, hello following steps are performed behind hello scene.</span></span> <span data-ttu-id="92439-114">Bude moct tooview hello průběh jednotlivých kroků v části kartu hello publikovat v hello publikování portálu.</span><span class="sxs-lookup"><span data-stu-id="92439-114">You will be able tooview hello progress of each step under hello PUBLISH tab in hello Publishing portal.</span></span> <span data-ttu-id="92439-115">Je nutné zkontrolovat tuto stránku v pravidelných intervalech (dokud hello stavu zobrazuje PŘIPRAVENÝ) pro všechny informace o selhání, které musí oprava z vaší elementu end.</span><span class="sxs-lookup"><span data-stu-id="92439-115">You must check this page at regular interval (until hello status shows STAGED) for any failure information which need correction from your end.</span></span>

    > - <span data-ttu-id="92439-116">Na první pracovní žádost přejde toohello certifikační týmu, který ověření hello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="92439-116">At first your staging request goes toohello certification team who validate hello vhd.</span></span> <span data-ttu-id="92439-117">Ale pokud vaše žádost má potom pouze marketingové změnit, pak hello certifikační krok se přeskočí.</span><span class="sxs-lookup"><span data-stu-id="92439-117">However, if your request has got only marketing change, then hello certification step is skipped.</span></span>
    > - <span data-ttu-id="92439-118">Po dokončení hello certifikační replikace hello nabídka start napříč všemi hello datových centrech Azure.</span><span class="sxs-lookup"><span data-stu-id="92439-118">Once hello certification is complete, replication of hello offer start across all hello Azure datacenters.</span></span> <span data-ttu-id="92439-119">Obvykle trvá 24 48hours pro toocomplete hello replikace, ale může trvat až tooa týden v závislosti na velikosti hello hello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="92439-119">It generally takes 24-48hours for hello replication toocomplete but may take up tooa week depending on hello size of hello vhd.</span></span> <span data-ttu-id="92439-120">Pokud vaše žádost má potom pouze marketingové změnit, pak hello replikace je však rychlejší.</span><span class="sxs-lookup"><span data-stu-id="92439-120">However, if your request has got only marketing change, then hello replication is faster.</span></span>
    > - <span data-ttu-id="92439-121">Po dokončení replikace hello pak hello nabídka bude k dispozici v hello [portál Azure](http:/portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="92439-121">When hello replication is complete, then hello offer will be available in hello [Azure portal](http:/portal.azure.com).</span></span> <span data-ttu-id="92439-122">V tento čas hello stav stát dvoufázové instalace v hello publikování portálu.</span><span class="sxs-lookup"><span data-stu-id="92439-122">At that time hello status become STAGED in hello Publishing portal.</span></span> <span data-ttu-id="92439-123">Dvoufázové instalace nabídka je viditelný v hello [portál Azure](http:/portal.azure.com) jenom pomocí ID e-mailu hello přidružené k předplatnému hello s které hello nabídka připravený.</span><span class="sxs-lookup"><span data-stu-id="92439-123">A staged offer is visible in hello [Azure portal](http:/portal.azure.com) only using hello email id(s) associated with hello subscription with which hello offer is staged.</span></span>

1. <span data-ttu-id="92439-124">Přihlaste se toohello [portál Azure preview](https://portal.azure.com) pomocí jedné z hello předplatná Azure uvedené v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="92439-124">Sign in toohello [Azure preview portal](https://portal.azure.com) by using one of hello Azure subscriptions listed in hello previous step.</span></span>
2. <span data-ttu-id="92439-125">Najít vaši nabídku a ověřit bodům bitové kopie virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="92439-125">Find your offer and validate your VM image points:</span></span>
   
   * <span data-ttu-id="92439-126">Ujistěte se, že marketingové obsah se zobrazí správně v hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="92439-126">Make sure that marketing content shows up correctly in hello Marketplace.</span></span>
   * <span data-ttu-id="92439-127">Nasazení začátku do konce hello image virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="92439-127">End-to-end deployment of hello VM image.</span></span>
     
      ![IMG. Mapa portálu](media/marketplace-publishing-push-to-staging/pubportal-mapping-azure-portal.jpg)

> [!IMPORTANT]
> <span data-ttu-id="92439-129">Vaši nabídku zůstane v pracovním dokud oznámit Microsoft prostřednictvím hello publikování portálu [**publikovat** kartě > klikněte na tlačítko hello **"Požádat o schválení tooPush tooProduction"**] se připravené toopush tooproduction.</span><span class="sxs-lookup"><span data-stu-id="92439-129">Your offer will remain in staging until you notify Microsoft via hello Publishing Portal [**Publish** tab > click on hello button **"Request Approval tooPush tooProduction"**] that you are ready toopush tooproduction.</span></span> <span data-ttu-id="92439-130">Jde ideální čas toohave, všichni členové týmu kontrolu nad vše v rámci přípravy vaši nabídku budete uvedené.</span><span class="sxs-lookup"><span data-stu-id="92439-130">This is an ideal time toohave all members of your team check over everything in preparation for your offer going listed.</span></span>
> 
> <span data-ttu-id="92439-131">Hello pracovní platformy je určená pro testování hello nabídka v režimu náhledu hello vydavatelem.</span><span class="sxs-lookup"><span data-stu-id="92439-131">hello staging platform is designed for testing hello offer in a preview mode by hello publisher.</span></span> <span data-ttu-id="92439-132">Důrazně bránit pomocí této platofrm pro obchodní účely.</span><span class="sxs-lookup"><span data-stu-id="92439-132">We strongly discourage using this platofrm for commerical purposes.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="92439-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="92439-133">Next steps</span></span>
<span data-ttu-id="92439-134">Teď, když vaši nabídku je "připravený" a testování její funkce a marketingu obsah, abyste mohli pokračovat toohello konečné publikování fázi **krok 4**: [nasazení vaší toohello nabídku Marketplace](marketplace-publishing-push-to-production.md).</span><span class="sxs-lookup"><span data-stu-id="92439-134">Now that your offer is "staged" and you have tested its functionality and marketing content, you can proceed toohello final publishing phase, **Step 4**: [Deploying your offer toohello Marketplace](marketplace-publishing-push-to-production.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="92439-135">Viz také</span><span class="sxs-lookup"><span data-stu-id="92439-135">See also</span></span>
* [<span data-ttu-id="92439-136">Začínáme: jak toopublish toohello nabídka Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="92439-136">Getting started: How toopublish an offer toohello Azure Marketplace</span></span>](marketplace-publishing-getting-started.md)

