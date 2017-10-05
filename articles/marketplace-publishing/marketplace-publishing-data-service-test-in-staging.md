---
title: "Testování dat služby nabídku pro Marketplace | Microsoft Docs"
description: "Pochopit, jak otestovat vaši nabídku Data služby pro Azure Marketplace."
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
ms.openlocfilehash: 56a8aad7484fed18b74200ffa7acf22363625a15
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="testing-your-data-service-offer-in-staging"></a><span data-ttu-id="1e74a-103">Při přípravě testování vaši nabídku Data Service</span><span class="sxs-lookup"><span data-stu-id="1e74a-103">Testing your Data Service offer in Staging</span></span>
> [!IMPORTANT]
> <span data-ttu-id="1e74a-104">**V tuto chvíli jsme již nejsou registrace všechny nové služby Data vydavatele. Nové dataservices nebude získat schválení pro výpis.**</span><span class="sxs-lookup"><span data-stu-id="1e74a-104">**At this time we are no longer onboarding any new Data Service publishers. New dataservices will not get approved for listing.**</span></span> <span data-ttu-id="1e74a-105">Pokud máte SaaS obchodní aplikace, který chcete publikovat na AppSource můžete najít další informace [zde](https://appsource.microsoft.com/partners).</span><span class="sxs-lookup"><span data-stu-id="1e74a-105">If you have a SaaS business application you would like to publish on AppSource you can find more information [here](https://appsource.microsoft.com/partners).</span></span> <span data-ttu-id="1e74a-106">Pokud máte IaaS aplikace nebo služby vývojáře, které chcete publikovat na webu Azure Marketplace můžete najít další informace [zde](https://azure.microsoft.com/marketplace/programs/certified/).</span><span class="sxs-lookup"><span data-stu-id="1e74a-106">If you have an IaaS applications or developer service you would like to publish on Azure Marketplace you can find more information [here](https://azure.microsoft.com/marketplace/programs/certified/).</span></span>
> 
> 

<span data-ttu-id="1e74a-107">Po dokončení první dva kroky [vytvoření účtu Microsoft Developer](marketplace-publishing-accounts-creation-registration.md) a [vytváří nabízejí služby Data publikování portálu](marketplace-publishing-data-service-creation.md) jste připraveni zpřístupnění vaši nabídku ve službě Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="1e74a-107">After completing the first two steps of [Creating your Microsoft Developer account](marketplace-publishing-accounts-creation-registration.md) and [Creating your Data Service Offer in Publishing Portal](marketplace-publishing-data-service-creation.md) you’re ready for making your offer available in the Azure Marketplace.</span></span> <span data-ttu-id="1e74a-108">Toto téma vás provede první, zprostředkující krok s názvem "Přípravy"</span><span class="sxs-lookup"><span data-stu-id="1e74a-108">This topic will walk you through the first, intermediate, step called “Staging”</span></span>

<span data-ttu-id="1e74a-109">Pracovní znamená nasazení vaši nabídku v soukromé "izolovaného", kde můžete otestovat a ověřit jeho funkčnost před odesláním do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="1e74a-109">Staging means deploying your offer in a private "sandbox" where you can test and verify its functionality before pushing it to production.</span></span> <span data-ttu-id="1e74a-110">Nabídka se zobrazí v pracovním stejně, jako by zákazníkovi, který je nasazena.</span><span class="sxs-lookup"><span data-stu-id="1e74a-110">The offer will appear in staging just as it would to a customer who has deployed it.</span></span>

## <a name="step-1-pushing-your-offer-to-staging"></a><span data-ttu-id="1e74a-111">Krok 1.</span><span class="sxs-lookup"><span data-stu-id="1e74a-111">Step 1.</span></span> <span data-ttu-id="1e74a-112">Když zavedete vaši nabídku do přípravy</span><span class="sxs-lookup"><span data-stu-id="1e74a-112">Pushing your offer to staging</span></span>
<span data-ttu-id="1e74a-113">Když zavedete vaši nabídku pracovní umožňuje otestovat nabídku předtím, než je k dispozici budoucí odběratelům.</span><span class="sxs-lookup"><span data-stu-id="1e74a-113">Pushing your offer to staging allows you to test the offer before it becomes available to future subscribers.</span></span>  <span data-ttu-id="1e74a-114">Uvidíte, jak vaši nabídku zobrazených a funkce pro ty odběr k datům.</span><span class="sxs-lookup"><span data-stu-id="1e74a-114">You can see how your offer will appear and function for those subscribing to your data.</span></span>  

  ![Kreslení](media/marketplace-publishing-data-service-test-in-staging/step-1.1.png)

1. <span data-ttu-id="1e74a-116">Přihlášení do [publikování portálu](https://publish.windowsazure.com)</span><span class="sxs-lookup"><span data-stu-id="1e74a-116">Login into the [Publishing Portal](https://publish.windowsazure.com)</span></span>
2. <span data-ttu-id="1e74a-117">Vyberte **datové služby** v levém navigačním okně</span><span class="sxs-lookup"><span data-stu-id="1e74a-117">Select **Data Services** in the left navigation window</span></span>
3. <span data-ttu-id="1e74a-118">Vyberte nabídku, které chcete nabízená pracovní.</span><span class="sxs-lookup"><span data-stu-id="1e74a-118">Select your offer you want to push to staging.</span></span> <span data-ttu-id="1e74a-119">Zobrazí se na obrazovce výše.</span><span class="sxs-lookup"><span data-stu-id="1e74a-119">You will see the above screen.</span></span>
4. <span data-ttu-id="1e74a-120">Klikněte na tlačítko **Push pracovní** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1e74a-120">Click **Push To Staging** button.</span></span>  
5. <span data-ttu-id="1e74a-121">Pokud jsou problémy s nabídka, která potřeba provést před vkládání do přípravy, zobrazí se seznam zobrazí.</span><span class="sxs-lookup"><span data-stu-id="1e74a-121">If there are issues with the offer that needed to be completed prior to pushing to staging, you will see a list displayed.</span></span>  <span data-ttu-id="1e74a-122">Opravte tyto položky kliknutím na každou položku v seznamu.</span><span class="sxs-lookup"><span data-stu-id="1e74a-122">Correct these items by clicking on each item in the list.</span></span> <span data-ttu-id="1e74a-123">Po kliknutí na všechny opravy provedené, **nabízená pracovní** tlačítko znovu.</span><span class="sxs-lookup"><span data-stu-id="1e74a-123">When all corrections made, click **Push to Staging** button again.</span></span>

<span data-ttu-id="1e74a-124">Pokud zde nejsou žádné problémy s vaši nabídku uvidíte automaticky otevíraném okně níže.</span><span class="sxs-lookup"><span data-stu-id="1e74a-124">If there are no issues with your offer you will see the popup window below.</span></span>  

<span data-ttu-id="1e74a-125">Pokud jste není plánování nebo nebyla schválena prezentovat vaši nabídku na portálu Azure (aktuálně má omezenou kapacitu), zavřete právě překryvné okno.</span><span class="sxs-lookup"><span data-stu-id="1e74a-125">If you’re not planning/not approved to surface your offer in Azure Portal (currently has limited capacity), then just close the pop-up window.</span></span>

<span data-ttu-id="1e74a-126">Testování dat služby v portálu Azure (kromě portálu DataMarket), budete potřebovat ID předplatného Azure k testování s.</span><span class="sxs-lookup"><span data-stu-id="1e74a-126">To test your Data Service in Azure Portal (in addition to the DataMarket portal), you will need an Azure Subscription ID to test with.</span></span>  <span data-ttu-id="1e74a-127">Toto ID předplatného bude identifikovat účet, který se bude moct otestovat vaši nabídku.</span><span class="sxs-lookup"><span data-stu-id="1e74a-127">This Subscription ID will identify the account that will be allowed to test your offer.</span></span>  

<span data-ttu-id="1e74a-128">Vyjmout a vložit svoje ID předplatného a kliknutím na značku zaškrtnutí pokračujte.</span><span class="sxs-lookup"><span data-stu-id="1e74a-128">Cut and paste your Subscription ID and click the checkmark to continue.</span></span>

  ![Kreslení](media/marketplace-publishing-data-service-test-in-staging/step-1.2.png)

> [!NOTE]
> <span data-ttu-id="1e74a-130">ID těchto předplatných Azure jsou pouze požadované pro testovacích a přípravných v [portálu pro správu Azure](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="1e74a-130">These Azure subscriptions IDs are only required for testing and staging in the [Azure Management Portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="1e74a-131">Nejsou potřeba otestovat v Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="1e74a-131">They are not required to test in Azure Marketplace.</span></span>
> 
> 

<span data-ttu-id="1e74a-132">Na další obrazovce, která se zobrazí ukazuje, že se zobrazením na ikonu "Probíhá" Probíhá publikování zvýrazněná žlutý níže.</span><span class="sxs-lookup"><span data-stu-id="1e74a-132">The next screen that appears shows that publishing is taking place by displaying the “In progress” icon highlighted yellow below.</span></span> <span data-ttu-id="1e74a-133">Když zavedete pracovní trvat 10 až 15 minut.</span><span class="sxs-lookup"><span data-stu-id="1e74a-133">Pushing to staging takes between 10 to 15 minutes.</span></span>  <span data-ttu-id="1e74a-134">Pokud trvá déle, nejprve aktualizujte webový prohlížeč (stiskněte F5 v aplikaci Internet Explorer).</span><span class="sxs-lookup"><span data-stu-id="1e74a-134">If it takes longer, first refresh your browser (press F5 in IE).</span></span>  <span data-ttu-id="1e74a-135">Ve výjimečných případech, kde je vaši nabídku stále nabízet pracovní po hodině, klikněte na tlačítko kontaktujte nás propojit dejte nám vědět, že se vyskytl problém.</span><span class="sxs-lookup"><span data-stu-id="1e74a-135">In the rare cases where your offer is still pushing to staging after an hour, click the contact us link to let us know that there is an issue.</span></span>

  ![Kreslení](media/marketplace-publishing-data-service-test-in-staging/step-1.3.png)

<span data-ttu-id="1e74a-137">Po dokončení nabízeného oznámení do přípravy na ikonu "Probíhá" bude zastavení přesouvání a stav bude aktualizovat na "Připravený".</span><span class="sxs-lookup"><span data-stu-id="1e74a-137">When the Push to Staging completes the “In progress” icon will stop moving and the status will be updated to “Staged”.</span></span>  <span data-ttu-id="1e74a-138">Nyní jste připraveni k testování vaši nabídku.</span><span class="sxs-lookup"><span data-stu-id="1e74a-138">You are now ready to test your offer.</span></span>  

## <a name="step-2-test-your-staged-offer-in-datamarket"></a><span data-ttu-id="1e74a-139">Krok 2.</span><span class="sxs-lookup"><span data-stu-id="1e74a-139">Step 2.</span></span> <span data-ttu-id="1e74a-140">Otestovat vaši nabídku dvoufázové instalace v DataMarket</span><span class="sxs-lookup"><span data-stu-id="1e74a-140">Test your staged offer in DataMarket</span></span>
<span data-ttu-id="1e74a-141">Klikněte na odkaz následující text **"v tématu služby nabízejí v..."**</span><span class="sxs-lookup"><span data-stu-id="1e74a-141">Click the link following the text **“See Your service offer at…”**</span></span> <span data-ttu-id="1e74a-142">Chcete-li zobrazit na obrazovce, že odběratele se zobrazí, když vaši nabídku přejde do produkčního prostředí a zobrazí se v DataMarket.</span><span class="sxs-lookup"><span data-stu-id="1e74a-142">to display the screen that the subscriber will see when your offer goes to production and will appear in DataMarket.</span></span>

  ![Kreslení](media/marketplace-publishing-data-service-test-in-staging/step-2.2.png)

<span data-ttu-id="1e74a-144">Testování nebo ověřte jednotlivých 12 položkách označena výše zajistit všechny loga, ceny nebo transakce, text, obrázky, dokumentace, a odkazy správně jsou správné a funkční.</span><span class="sxs-lookup"><span data-stu-id="1e74a-144">Test or verify each of the 12 items marked above to ensure all logos, prices/transactions, text, images, documentation, and links are correct and working properly.</span></span>  <span data-ttu-id="1e74a-145">Toto je vhodná doba na Ujistěte se, že všechny testovací hodnoty, které jste zadali při vytváření vaši nabídku byly nahrazeny skutečnými hodnotami.</span><span class="sxs-lookup"><span data-stu-id="1e74a-145">This is a good time to ensure any test values you entered when creating your offer have been replaced with actual values.</span></span>

1. <span data-ttu-id="1e74a-146">Logo nabídka</span><span class="sxs-lookup"><span data-stu-id="1e74a-146">Offer logo</span></span>
2. <span data-ttu-id="1e74a-147">Název nabídky</span><span class="sxs-lookup"><span data-stu-id="1e74a-147">Offer name</span></span>
3. <span data-ttu-id="1e74a-148">Název vydavatele nebo odkaz na web vaší společnosti</span><span class="sxs-lookup"><span data-stu-id="1e74a-148">Publisher name/link to your company's website</span></span>
4. <span data-ttu-id="1e74a-149">Kategorie vyhledávání pro vaši nabídku</span><span class="sxs-lookup"><span data-stu-id="1e74a-149">Search categories for your offer</span></span>
5. <span data-ttu-id="1e74a-150">Odkaz na podporu vaši nabídku pomůže odběratele</span><span class="sxs-lookup"><span data-stu-id="1e74a-150">Your offer's support link to assist subscribers</span></span>
6. <span data-ttu-id="1e74a-151">Kontextová popis pro vaši nabídku</span><span class="sxs-lookup"><span data-stu-id="1e74a-151">Contextual description for your offer</span></span>
7. <span data-ttu-id="1e74a-152">Nabídka plán znázorňující fakturačních údajů</span><span class="sxs-lookup"><span data-stu-id="1e74a-152">Offer plan depicting billing details</span></span>
8. <span data-ttu-id="1e74a-153">Odkaz na implementaci kódu</span><span class="sxs-lookup"><span data-stu-id="1e74a-153">Link to implementation code</span></span>
9. <span data-ttu-id="1e74a-154">Ukázka bitové kopie, které ilustrují použití dat nabídky</span><span class="sxs-lookup"><span data-stu-id="1e74a-154">Sample images that illustrate use of offer data</span></span>
10. <span data-ttu-id="1e74a-155">Metadata vstupu a výstupu u každé služby v rámci nabídky</span><span class="sxs-lookup"><span data-stu-id="1e74a-155">Input/Output metadata for each service within the offer</span></span>
11. <span data-ttu-id="1e74a-156">Nabídka pro podmínky použití</span><span class="sxs-lookup"><span data-stu-id="1e74a-156">Offer's Terms of Use</span></span>
12. <span data-ttu-id="1e74a-157">Náhled nabídka dat</span><span class="sxs-lookup"><span data-stu-id="1e74a-157">Preview of the offer's data</span></span>

<span data-ttu-id="1e74a-158">Nakonec zkontrolujte, že služba bude fungovat prostřednictvím Datamarket kliknutím na odkaz "Prozkoumat tento datovou sadu".</span><span class="sxs-lookup"><span data-stu-id="1e74a-158">Finally, check the service will work through the Datamarket by clicking the link “EXPLORE THIS DATASET”.</span></span>  <span data-ttu-id="1e74a-159">Otevře se nové okno v nástroji říkáme "Service Explorer", můžete zobrazit náhled výsledků dotazu na vaši službu.</span><span class="sxs-lookup"><span data-stu-id="1e74a-159">A new window will open in the tool we call “Service Explorer” so you can preview the results of a query against your service.</span></span>  <span data-ttu-id="1e74a-160">V tomto okně můžete zadat potřebné parametry a zobrazit výsledky zobrazí z dotazu na vaši službu.</span><span class="sxs-lookup"><span data-stu-id="1e74a-160">In this window, you can enter the parameters needed and see the results displayed from a query against your service.</span></span>   <span data-ttu-id="1e74a-161">Navíc zobrazí je adresa URL pro svůj dotaz.</span><span class="sxs-lookup"><span data-stu-id="1e74a-161">Also, displayed is the URL for your Query.</span></span>  

> [!NOTE]
> <span data-ttu-id="1e74a-162">Nezapomeňte si projít textový popis služby zobrazí v horní části.</span><span class="sxs-lookup"><span data-stu-id="1e74a-162">Be sure to review the textual description of the service displayed at the top.</span></span>  <span data-ttu-id="1e74a-163">A pokud vaši nabídku se skládá z více než jedna služba volání, klikněte na karty v dolní části přejděte na další služby ke kontrole a testování.</span><span class="sxs-lookup"><span data-stu-id="1e74a-163">And if your offer consists of more than one service call, click the tabs at the bottom to switch to the next service to review and test.</span></span>
> 
> 

## <a name="next-step"></a><span data-ttu-id="1e74a-164">Další krok</span><span class="sxs-lookup"><span data-stu-id="1e74a-164">Next step</span></span>
<span data-ttu-id="1e74a-165">Pokud potřebujete další pomoc a problémy s jejich řešení obraťte se prosím [Azure technickou podporu vydavatele](http://go.microsoft.com/fwlink/?LinkId=272975).</span><span class="sxs-lookup"><span data-stu-id="1e74a-165">If you are having issues and need help resolving them please contact [Azure Publisher Support](http://go.microsoft.com/fwlink/?LinkId=272975).</span></span>

<span data-ttu-id="1e74a-166">Pokud jste splněna a připravena k publikování vaši nabídku, přečtěte si prosím [schválení žádosti o změnu do Push do produkčního prostředí](marketplace-publishing-push-to-production.md) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="1e74a-166">If you are satisfied and ready to publish your offer please read the [Request Approval to Push To Production](marketplace-publishing-push-to-production.md) documentation.</span></span>

## <a name="see-also"></a><span data-ttu-id="1e74a-167">Viz také</span><span class="sxs-lookup"><span data-stu-id="1e74a-167">See Also</span></span>
* [<span data-ttu-id="1e74a-168">Začínáme: Jak publikování nabídky pro Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="1e74a-168">Getting Started: How to publish an offer to the Azure Marketplace</span></span>](marketplace-publishing-getting-started.md)

