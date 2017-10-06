---
title: "aaaTesting služby dat nabízejí pro hello Marketplace | Microsoft Docs"
description: "Pochopte, jak tootest služby dat nabízejí pro hello Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: e861bd11-f74d-4d77-b4b5-23fb463644ad
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 1b9c7027d8e0818b9bdee5cfca971bab25dd1959
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="testing-your-data-service-offer-in-staging"></a><span data-ttu-id="46e7e-103">Při přípravě testování vaši nabídku Data Service</span><span class="sxs-lookup"><span data-stu-id="46e7e-103">Testing your Data Service offer in Staging</span></span>
> [!IMPORTANT]
> <span data-ttu-id="46e7e-104">**V tuto chvíli jsme již nejsou registrace všechny nové služby Data vydavatele. Nové dataservices nebude získat schválení pro výpis.**</span><span class="sxs-lookup"><span data-stu-id="46e7e-104">**At this time we are no longer onboarding any new Data Service publishers. New dataservices will not get approved for listing.**</span></span> <span data-ttu-id="46e7e-105">Pokud máte obchodní aplikace SaaS chcete toopublish na AppSource najdete další informace [zde](https://appsource.microsoft.com/partners).</span><span class="sxs-lookup"><span data-stu-id="46e7e-105">If you have a SaaS business application you would like toopublish on AppSource you can find more information [here](https://appsource.microsoft.com/partners).</span></span> <span data-ttu-id="46e7e-106">Pokud máte IaaS aplikace nebo služba vývojáře by jako toopublish na webu Azure Marketplace můžete najít další informace [zde](https://azure.microsoft.com/marketplace/programs/certified/).</span><span class="sxs-lookup"><span data-stu-id="46e7e-106">If you have an IaaS applications or developer service you would like toopublish on Azure Marketplace you can find more information [here](https://azure.microsoft.com/marketplace/programs/certified/).</span></span>
> 
> 

<span data-ttu-id="46e7e-107">Po dokončení hello první dva kroky [vytvoření účtu Microsoft Developer](marketplace-publishing-accounts-creation-registration.md) a [vytváří nabízejí služby Data publikování portálu](marketplace-publishing-data-service-creation.md) jste připraveni zpřístupňuje vaši nabídku v hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="46e7e-107">After completing hello first two steps of [Creating your Microsoft Developer account](marketplace-publishing-accounts-creation-registration.md) and [Creating your Data Service Offer in Publishing Portal](marketplace-publishing-data-service-creation.md) you’re ready for making your offer available in hello Azure Marketplace.</span></span> <span data-ttu-id="46e7e-108">Toto téma se nejprve vás provede procesem hello, mezilehlé, krok názvem "přípravy"</span><span class="sxs-lookup"><span data-stu-id="46e7e-108">This topic will walk you through hello first, intermediate, step called “Staging”</span></span>

<span data-ttu-id="46e7e-109">Pracovní znamená nasazení vaši nabídku v soukromé "izolovaného", kde můžete otestovat a ověřit jeho funkčnost před odesláním ji tooproduction.</span><span class="sxs-lookup"><span data-stu-id="46e7e-109">Staging means deploying your offer in a private "sandbox" where you can test and verify its functionality before pushing it tooproduction.</span></span> <span data-ttu-id="46e7e-110">Nabídka Hello se zobrazí v pracovním stejně, jako by tooa zákazníkovi, který je nasazena.</span><span class="sxs-lookup"><span data-stu-id="46e7e-110">hello offer will appear in staging just as it would tooa customer who has deployed it.</span></span>

## <a name="step-1-pushing-your-offer-toostaging"></a><span data-ttu-id="46e7e-111">Krok 1.</span><span class="sxs-lookup"><span data-stu-id="46e7e-111">Step 1.</span></span> <span data-ttu-id="46e7e-112">Když zavedete vaší toostaging nabídka</span><span class="sxs-lookup"><span data-stu-id="46e7e-112">Pushing your offer toostaging</span></span>
<span data-ttu-id="46e7e-113">Vkládání vaší toostaging nabídka umožňuje tootest hello nabídka předtím, než bude k dispozici toofuture odběratele.</span><span class="sxs-lookup"><span data-stu-id="46e7e-113">Pushing your offer toostaging allows you tootest hello offer before it becomes available toofuture subscribers.</span></span>  <span data-ttu-id="46e7e-114">Uvidíte, jak vaši nabídku zobrazených a funkce pro ty přihlášení k odběru tooyour data.</span><span class="sxs-lookup"><span data-stu-id="46e7e-114">You can see how your offer will appear and function for those subscribing tooyour data.</span></span>  

  ![Kreslení](media/marketplace-publishing-data-service-test-in-staging/step-1.1.png)

1. <span data-ttu-id="46e7e-116">Přihlášení do hello [publikování portálu](https://publish.windowsazure.com)</span><span class="sxs-lookup"><span data-stu-id="46e7e-116">Login into hello [Publishing Portal](https://publish.windowsazure.com)</span></span>
2. <span data-ttu-id="46e7e-117">Vyberte **datové služby** v levém navigačním okně hello</span><span class="sxs-lookup"><span data-stu-id="46e7e-117">Select **Data Services** in hello left navigation window</span></span>
3. <span data-ttu-id="46e7e-118">Vyberte nabídku chcete toopush toostaging.</span><span class="sxs-lookup"><span data-stu-id="46e7e-118">Select your offer you want toopush toostaging.</span></span> <span data-ttu-id="46e7e-119">Zobrazí se hello výše obrazovky.</span><span class="sxs-lookup"><span data-stu-id="46e7e-119">You will see hello above screen.</span></span>
4. <span data-ttu-id="46e7e-120">Klikněte na tlačítko **Push tooStaging** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="46e7e-120">Click **Push tooStaging** button.</span></span>  
5. <span data-ttu-id="46e7e-121">Pokud dochází k potížím s hello nabídka, která potřeby toobe dokončit předchozí toopushing toostaging, zobrazí se seznam zobrazí.</span><span class="sxs-lookup"><span data-stu-id="46e7e-121">If there are issues with hello offer that needed toobe completed prior toopushing toostaging, you will see a list displayed.</span></span>  <span data-ttu-id="46e7e-122">Kliknutím na každou položku v seznamu hello opravte tyto položky.</span><span class="sxs-lookup"><span data-stu-id="46e7e-122">Correct these items by clicking on each item in hello list.</span></span> <span data-ttu-id="46e7e-123">Po kliknutí na všechny opravy provedené, **Push tooStaging** tlačítko znovu.</span><span class="sxs-lookup"><span data-stu-id="46e7e-123">When all corrections made, click **Push tooStaging** button again.</span></span>

<span data-ttu-id="46e7e-124">Pokud zde nejsou žádné problémy s vaši nabídku uvidíte hello automaticky otevíraném okně níže.</span><span class="sxs-lookup"><span data-stu-id="46e7e-124">If there are no issues with your offer you will see hello popup window below.</span></span>  

<span data-ttu-id="46e7e-125">Pokud si nejste plánování nebo nebyla schválena toosurface vaši nabídku na portálu Azure (aktuálně má omezenou kapacitu), pak právě zavřít hello automaticky otevírané okno.</span><span class="sxs-lookup"><span data-stu-id="46e7e-125">If you’re not planning/not approved toosurface your offer in Azure Portal (currently has limited capacity), then just close hello pop-up window.</span></span>

<span data-ttu-id="46e7e-126">tootest Data služby v portálu Azure (v přidání toohello DataMarket portál), budete potřebovat tootest ID předplatného Azure s.</span><span class="sxs-lookup"><span data-stu-id="46e7e-126">tootest your Data Service in Azure Portal (in addition toohello DataMarket portal), you will need an Azure Subscription ID tootest with.</span></span>  <span data-ttu-id="46e7e-127">Toto ID předplatného určují hello účet, který bude povoleno tootest vaši nabídku.</span><span class="sxs-lookup"><span data-stu-id="46e7e-127">This Subscription ID will identify hello account that will be allowed tootest your offer.</span></span>  

<span data-ttu-id="46e7e-128">Vyjmout a vložit svoje ID předplatného a klikněte na značku zaškrtnutí toocontinue hello.</span><span class="sxs-lookup"><span data-stu-id="46e7e-128">Cut and paste your Subscription ID and click hello checkmark toocontinue.</span></span>

  ![Kreslení](media/marketplace-publishing-data-service-test-in-staging/step-1.2.png)

> [!NOTE]
> <span data-ttu-id="46e7e-130">ID těchto předplatných Azure jsou pouze požadované pro testovacích a přípravných v hello [portálu pro správu Azure](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="46e7e-130">These Azure subscriptions IDs are only required for testing and staging in hello [Azure Management Portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="46e7e-131">Nejsou požadované tootest v Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="46e7e-131">They are not required tootest in Azure Marketplace.</span></span>
> 
> 

<span data-ttu-id="46e7e-132">Hello na další obrazovce zobrazí ukazuje, že se zobrazením hello "Probíhá" Probíhá publikování ikonu zvýrazněná žlutý níže.</span><span class="sxs-lookup"><span data-stu-id="46e7e-132">hello next screen that appears shows that publishing is taking place by displaying hello “In progress” icon highlighted yellow below.</span></span> <span data-ttu-id="46e7e-133">Když zavedete toostaging trvat 10 minut too15.</span><span class="sxs-lookup"><span data-stu-id="46e7e-133">Pushing toostaging takes between 10 too15 minutes.</span></span>  <span data-ttu-id="46e7e-134">Pokud trvá déle, nejprve aktualizujte webový prohlížeč (stiskněte F5 v aplikaci Internet Explorer).</span><span class="sxs-lookup"><span data-stu-id="46e7e-134">If it takes longer, first refresh your browser (press F5 in IE).</span></span>  <span data-ttu-id="46e7e-135">Ve výjimečných případech hello kde vaši nabídku stále předává toostaging po hodině klikněte na tlačítko hello kontaktujte nás na odkaz toolet nám vědět, že se vyskytl problém.</span><span class="sxs-lookup"><span data-stu-id="46e7e-135">In hello rare cases where your offer is still pushing toostaging after an hour, click hello contact us link toolet us know that there is an issue.</span></span>

  ![Kreslení](media/marketplace-publishing-data-service-test-in-staging/step-1.3.png)

<span data-ttu-id="46e7e-137">Po dokončení hello nabízené tooStaging hello "Probíhá" zastaví ikona přesunutí a hello stav bude aktualizován příliš "připravený".</span><span class="sxs-lookup"><span data-stu-id="46e7e-137">When hello Push tooStaging completes hello “In progress” icon will stop moving and hello status will be updated too“Staged”.</span></span>  <span data-ttu-id="46e7e-138">Můžete je nyní připraven tootest vaši nabídku.</span><span class="sxs-lookup"><span data-stu-id="46e7e-138">You are now ready tootest your offer.</span></span>  

## <a name="step-2-test-your-staged-offer-in-datamarket"></a><span data-ttu-id="46e7e-139">Krok 2.</span><span class="sxs-lookup"><span data-stu-id="46e7e-139">Step 2.</span></span> <span data-ttu-id="46e7e-140">Otestovat vaši nabídku dvoufázové instalace v DataMarket</span><span class="sxs-lookup"><span data-stu-id="46e7e-140">Test your staged offer in DataMarket</span></span>
<span data-ttu-id="46e7e-141">Kliknutím na odkaz hello následující hello text **"v tématu služby nabízejí v..."**</span><span class="sxs-lookup"><span data-stu-id="46e7e-141">Click hello link following hello text **“See Your service offer at…”**</span></span> <span data-ttu-id="46e7e-142">toodisplay úvodní obrazovka, která hello odběratele se zobrazí, když vaši nabídku přejde tooproduction a zobrazí se v DataMarket.</span><span class="sxs-lookup"><span data-stu-id="46e7e-142">toodisplay hello screen that hello subscriber will see when your offer goes tooproduction and will appear in DataMarket.</span></span>

  ![Kreslení](media/marketplace-publishing-data-service-test-in-staging/step-2.2.png)

<span data-ttu-id="46e7e-144">Testování nebo ověřte všechny položky 12 hello označen výše tooensure všechny loga, ceny nebo transakce, text, obrázky, dokumentace a odkazy jsou správné a funkční správně.</span><span class="sxs-lookup"><span data-stu-id="46e7e-144">Test or verify each of hello 12 items marked above tooensure all logos, prices/transactions, text, images, documentation, and links are correct and working properly.</span></span>  <span data-ttu-id="46e7e-145">Toto je vhodná doba tooensure, které všechny testovací hodnoty, které jste zadali při vytváření vaši nabídku byly nahrazeny skutečnými hodnotami.</span><span class="sxs-lookup"><span data-stu-id="46e7e-145">This is a good time tooensure any test values you entered when creating your offer have been replaced with actual values.</span></span>

1. <span data-ttu-id="46e7e-146">Logo nabídka</span><span class="sxs-lookup"><span data-stu-id="46e7e-146">Offer logo</span></span>
2. <span data-ttu-id="46e7e-147">Název nabídky</span><span class="sxs-lookup"><span data-stu-id="46e7e-147">Offer name</span></span>
3. <span data-ttu-id="46e7e-148">Vydavatel název/propojení tooyour web společnosti</span><span class="sxs-lookup"><span data-stu-id="46e7e-148">Publisher name/link tooyour company's website</span></span>
4. <span data-ttu-id="46e7e-149">Kategorie vyhledávání pro vaši nabídku</span><span class="sxs-lookup"><span data-stu-id="46e7e-149">Search categories for your offer</span></span>
5. <span data-ttu-id="46e7e-150">Vaši nabídku podporu odkaz tooassist odběratele</span><span class="sxs-lookup"><span data-stu-id="46e7e-150">Your offer's support link tooassist subscribers</span></span>
6. <span data-ttu-id="46e7e-151">Kontextová popis pro vaši nabídku</span><span class="sxs-lookup"><span data-stu-id="46e7e-151">Contextual description for your offer</span></span>
7. <span data-ttu-id="46e7e-152">Nabídka plán znázorňující fakturačních údajů</span><span class="sxs-lookup"><span data-stu-id="46e7e-152">Offer plan depicting billing details</span></span>
8. <span data-ttu-id="46e7e-153">Kód tooimplementation vazby</span><span class="sxs-lookup"><span data-stu-id="46e7e-153">Link tooimplementation code</span></span>
9. <span data-ttu-id="46e7e-154">Ukázka bitové kopie, které ilustrují použití dat nabídky</span><span class="sxs-lookup"><span data-stu-id="46e7e-154">Sample images that illustrate use of offer data</span></span>
10. <span data-ttu-id="46e7e-155">Metadata vstupu a výstupu u každé služby v rámci nabídky hello</span><span class="sxs-lookup"><span data-stu-id="46e7e-155">Input/Output metadata for each service within hello offer</span></span>
11. <span data-ttu-id="46e7e-156">Nabídka pro podmínky použití</span><span class="sxs-lookup"><span data-stu-id="46e7e-156">Offer's Terms of Use</span></span>
12. <span data-ttu-id="46e7e-157">Náhled hello nabídka dat</span><span class="sxs-lookup"><span data-stu-id="46e7e-157">Preview of hello offer's data</span></span>

<span data-ttu-id="46e7e-158">Nakonec zkontrolujte, že služba hello bude fungovat prostřednictvím hello Datamarket kliknutím na odkaz hello "Prozkoumat tento datovou sadu".</span><span class="sxs-lookup"><span data-stu-id="46e7e-158">Finally, check hello service will work through hello Datamarket by clicking hello link “EXPLORE THIS DATASET”.</span></span>  <span data-ttu-id="46e7e-159">Otevře se nové okno v nástroji hello říkáme "Service Explorer", můžete zobrazit náhled hello výsledků dotazu na vaši službu.</span><span class="sxs-lookup"><span data-stu-id="46e7e-159">A new window will open in hello tool we call “Service Explorer” so you can preview hello results of a query against your service.</span></span>  <span data-ttu-id="46e7e-160">V tomto okně můžete zadat hello parametry nejsou potřebné a zobrazte výsledky hello zobrazí z dotazu na vaši službu.</span><span class="sxs-lookup"><span data-stu-id="46e7e-160">In this window, you can enter hello parameters needed and see hello results displayed from a query against your service.</span></span>   <span data-ttu-id="46e7e-161">Navíc zobrazí je hello URL pro svůj dotaz.</span><span class="sxs-lookup"><span data-stu-id="46e7e-161">Also, displayed is hello URL for your Query.</span></span>  

> [!NOTE]
> <span data-ttu-id="46e7e-162">Být jisti tooreview hello textový popis hello služby zobrazí v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="46e7e-162">Be sure tooreview hello textual description of hello service displayed at hello top.</span></span>  <span data-ttu-id="46e7e-163">A pokud vaši nabídku se skládá z více než jedna služba volání, klikněte na karty hello v hello dolní tooswitch toohello další služby tooreview a testování.</span><span class="sxs-lookup"><span data-stu-id="46e7e-163">And if your offer consists of more than one service call, click hello tabs at hello bottom tooswitch toohello next service tooreview and test.</span></span>
> 
> 

## <a name="next-step"></a><span data-ttu-id="46e7e-164">Další krok</span><span class="sxs-lookup"><span data-stu-id="46e7e-164">Next step</span></span>
<span data-ttu-id="46e7e-165">Pokud potřebujete další pomoc a problémy s jejich řešení obraťte se prosím [Azure technickou podporu vydavatele](http://go.microsoft.com/fwlink/?LinkId=272975).</span><span class="sxs-lookup"><span data-stu-id="46e7e-165">If you are having issues and need help resolving them please contact [Azure Publisher Support](http://go.microsoft.com/fwlink/?LinkId=272975).</span></span>

<span data-ttu-id="46e7e-166">Pokud budete spokojeni a připravena toopublish vaši nabídku přečtěte hello [schválení žádosti o změnu tooPush tooProduction](marketplace-publishing-push-to-production.md) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="46e7e-166">If you are satisfied and ready toopublish your offer please read hello [Request Approval tooPush tooProduction](marketplace-publishing-push-to-production.md) documentation.</span></span>

## <a name="see-also"></a><span data-ttu-id="46e7e-167">Viz také</span><span class="sxs-lookup"><span data-stu-id="46e7e-167">See Also</span></span>
* [<span data-ttu-id="46e7e-168">Začínáme: Jak toopublish toohello nabídka Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="46e7e-168">Getting Started: How toopublish an offer toohello Azure Marketplace</span></span>](marketplace-publishing-getting-started.md)

