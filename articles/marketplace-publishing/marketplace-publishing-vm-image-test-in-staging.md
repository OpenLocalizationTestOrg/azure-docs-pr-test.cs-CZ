---
title: "Testovací virtuální počítač nabídku pro Marketplace | Microsoft Docs"
description: "Pochopit, jak chcete otestovat bitové kopie virtuálních počítačů v Azure Marketplace."
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
ms.openlocfilehash: 26f856059b381be91b9cdd1f98a11dc90813c0c5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="test-your-vm-offer-for-the-azure-marketplace-in-staging"></a><span data-ttu-id="0766c-103">Při přípravě otestovat vaši nabídku virtuálních počítačů pro Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="0766c-103">Test your VM offer for the Azure Marketplace in staging</span></span>
<span data-ttu-id="0766c-104">Pracovní znamená nasazení vaší SKU privátního "izolovaného", kde můžete testovat a před nasazením na Marketplace s cílem ověřit její funkce.</span><span class="sxs-lookup"><span data-stu-id="0766c-104">Staging means deploying your SKU in a private “sandbox” where you can test and validate its functionality before deploying it to the Marketplace.</span></span> <span data-ttu-id="0766c-105">Verze SKU se zobrazí v pracovním stejně, jako by zákazníkovi, který je nasazena.</span><span class="sxs-lookup"><span data-stu-id="0766c-105">The SKU appears in staging just as it would to a customer who has deployed it.</span></span> <span data-ttu-id="0766c-106">Chcete-li být vložena do přípravy musí mít certifikovanou bitové kopie virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0766c-106">Your VM image must be certified to be pushed to staging.</span></span>

## <a name="step-1-push-your-offer-to-staging"></a><span data-ttu-id="0766c-107">Krok 1: Push vaši nabídku do přípravy</span><span class="sxs-lookup"><span data-stu-id="0766c-107">Step 1: Push your offer to staging</span></span>
1. <span data-ttu-id="0766c-108">Na **publikovat** , klikněte na **nabízená pracovní**.</span><span class="sxs-lookup"><span data-stu-id="0766c-108">On the **Publish** tab, click **Push to Staging**.</span></span>
   
    ![Kreslení](media/marketplace-publishing-vm-image-test-in-staging/vm-image-push-to-staging.png)
2. <span data-ttu-id="0766c-110">Pokud portálu publikování upozorňuje na všechny chyby, opravte je.</span><span class="sxs-lookup"><span data-stu-id="0766c-110">If the Publishing Portal notifies you of any errors, correct them.</span></span>
3. <span data-ttu-id="0766c-111">V **přístup dvoufázové instalace nabídku?** dialogovém okně zadejte seznam předplatných Azure, které budete používat k zobrazení náhledu vaši nabídku v [portál Azure preview](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0766c-111">In the **Who can access your staged offer?** dialog box, enter the list of Azure subscriptions that you will use to preview your offer in the [Azure preview portal](https://portal.azure.com).</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="0766c-112">V případě virtuálních počítačů a šablony řešení prosím **nepodporují** povolených odběrů typu CSP, DreamSpark nebo Azure v otevřené.</span><span class="sxs-lookup"><span data-stu-id="0766c-112">In case of Virtual Machines and Solution templates, please **do not** whitelist subscriptions of type CSP, DreamSpark or Azure in Open.</span></span>
   > 
   > 

    > <span data-ttu-id="0766c-113">V případě virtuálních počítačů, když kliknete na tlačítko **PUSH pracovní**, následující kroky se provádějí za scény.</span><span class="sxs-lookup"><span data-stu-id="0766c-113">In case of Virtual Machines, when you click on the button **PUSH TO STAGING**, the following steps are performed behind the scene.</span></span> <span data-ttu-id="0766c-114">Bude moct zobrazovat průběh každého kroku na kartě Publikovat v publikační portálu.</span><span class="sxs-lookup"><span data-stu-id="0766c-114">You will be able to view the progress of each step under the PUBLISH tab in the Publishing portal.</span></span> <span data-ttu-id="0766c-115">Je nutné zkontrolovat tuto stránku v pravidelných intervalech (než je ve stavu PŘIPRAVENÝ) pro všechny informace o selhání, které musí oprava z vaší elementu end.</span><span class="sxs-lookup"><span data-stu-id="0766c-115">You must check this page at regular interval (until the status shows STAGED) for any failure information which need correction from your end.</span></span>

    > - <span data-ttu-id="0766c-116">Na první pracovní žádost přejde na certifikační týmu, který ověření virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="0766c-116">At first your staging request goes to the certification team who validate the vhd.</span></span> <span data-ttu-id="0766c-117">Ale pokud vaši žádost má potom pouze marketingové změnit, pak certifikační krok se přeskočí.</span><span class="sxs-lookup"><span data-stu-id="0766c-117">However, if your request has got only marketing change, then the certification step is skipped.</span></span>
    > - <span data-ttu-id="0766c-118">Po dokončení certifikační replikace nabídku start přes Azure datových center.</span><span class="sxs-lookup"><span data-stu-id="0766c-118">Once the certification is complete, replication of the offer start across all the Azure datacenters.</span></span> <span data-ttu-id="0766c-119">Obvykle trvá 24 48hours na dokončení replikace, ale může trvat až do týdne v závislosti na velikosti virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="0766c-119">It generally takes 24-48hours for the replication to complete but may take up to a week depending on the size of the vhd.</span></span> <span data-ttu-id="0766c-120">Pokud vaše žádost má potom pouze marketingové změnit, pak replikace je však rychlejší.</span><span class="sxs-lookup"><span data-stu-id="0766c-120">However, if your request has got only marketing change, then the replication is faster.</span></span>
    > - <span data-ttu-id="0766c-121">Po dokončení replikace poté nabídku bude k dispozici v [portál Azure](http:/portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0766c-121">When the replication is complete, then the offer will be available in the [Azure portal](http:/portal.azure.com).</span></span> <span data-ttu-id="0766c-122">V této době stav stát dvoufázové instalace v publikační portálu.</span><span class="sxs-lookup"><span data-stu-id="0766c-122">At that time the status become STAGED in the Publishing portal.</span></span> <span data-ttu-id="0766c-123">Dvoufázové instalace nabídky se zobrazí na [portál Azure](http:/portal.azure.com) jenom pomocí ID e-mailu přidruženou k odběru, ke které je vynášené nabídku.</span><span class="sxs-lookup"><span data-stu-id="0766c-123">A staged offer is visible in the [Azure portal](http:/portal.azure.com) only using the email id(s) associated with the subscription with which the offer is staged.</span></span>

1. <span data-ttu-id="0766c-124">Přihlaste se k [portál Azure preview](https://portal.azure.com) pomocí jednoho z předplatných Azure uvedené v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="0766c-124">Sign in to the [Azure preview portal](https://portal.azure.com) by using one of the Azure subscriptions listed in the previous step.</span></span>
2. <span data-ttu-id="0766c-125">Najít vaši nabídku a ověřit bodům bitové kopie virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="0766c-125">Find your offer and validate your VM image points:</span></span>
   
   * <span data-ttu-id="0766c-126">Ujistěte se, že marketingové obsah se zobrazí správně v Marketplace.</span><span class="sxs-lookup"><span data-stu-id="0766c-126">Make sure that marketing content shows up correctly in the Marketplace.</span></span>
   * <span data-ttu-id="0766c-127">Koncové nasazení bitové kopie virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0766c-127">End-to-end deployment of the VM image.</span></span>
     
      ![IMG. Mapa portálu](media/marketplace-publishing-push-to-staging/pubportal-mapping-azure-portal.jpg)

> [!IMPORTANT]
> <span data-ttu-id="0766c-129">Vaši nabídku zůstane v pracovním dokud oznámit Microsoftu prostřednictvím portálu publikování [**publikovat** kartě > klikněte na tlačítko **"Žádosti o schválení pro nabízené do výroby"**] je vše připraveno k produkční.</span><span class="sxs-lookup"><span data-stu-id="0766c-129">Your offer will remain in staging until you notify Microsoft via the Publishing Portal [**Publish** tab > click on the button **"Request Approval to Push to Production"**] that you are ready to push to production.</span></span> <span data-ttu-id="0766c-130">To je ideální třeba mít všechny členy vašeho týmu kontroly nad vše v rámci přípravy vaši nabídku budete uvedené.</span><span class="sxs-lookup"><span data-stu-id="0766c-130">This is an ideal time to have all members of your team check over everything in preparation for your offer going listed.</span></span>
> 
> <span data-ttu-id="0766c-131">Pracovní platformy je určená pro testování nabídku v režimu náhledu vydavatelem.</span><span class="sxs-lookup"><span data-stu-id="0766c-131">The staging platform is designed for testing the offer in a preview mode by the publisher.</span></span> <span data-ttu-id="0766c-132">Důrazně bránit pomocí této platofrm pro obchodní účely.</span><span class="sxs-lookup"><span data-stu-id="0766c-132">We strongly discourage using this platofrm for commerical purposes.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="0766c-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0766c-133">Next steps</span></span>
<span data-ttu-id="0766c-134">Teď, když vaši nabídku je "připravený" a testování její funkce a uvádění na trh obsah, můžete přejít k publikování závěrečné **krok 4**: [nasazení vaši nabídku Marketplace](marketplace-publishing-push-to-production.md).</span><span class="sxs-lookup"><span data-stu-id="0766c-134">Now that your offer is "staged" and you have tested its functionality and marketing content, you can proceed to the final publishing phase, **Step 4**: [Deploying your offer to the Marketplace](marketplace-publishing-push-to-production.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="0766c-135">Viz také</span><span class="sxs-lookup"><span data-stu-id="0766c-135">See also</span></span>
* [<span data-ttu-id="0766c-136">Začínáme: postup publikování nabídky pro Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="0766c-136">Getting started: How to publish an offer to the Azure Marketplace</span></span>](marketplace-publishing-getting-started.md)

