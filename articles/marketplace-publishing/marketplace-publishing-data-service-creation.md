---
title: "Příručka o vytváření datové služby pro Marketplace | Microsoft Docs"
description: "Podrobné pokyny o tom, jak vytvořit, certifikovat a datové služby pro nasazení zakoupit na webu Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 96194198-6991-43b4-918e-ee337e250339
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: c0c9362f1c2e15c947aaaf7187f3383ad243140f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="data-service-publishing-guide-for-the-azure-marketplace"></a><span data-ttu-id="4fc5e-103">Služba data publikování průvodce pro Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="4fc5e-103">Data Service Publishing Guide for the Azure Marketplace</span></span>
> [!IMPORTANT]
> <span data-ttu-id="4fc5e-104">**V tuto chvíli jsme již nejsou registrace všechny nové služby Data vydavatele. Nové dataservices nebude získat schválení pro výpis.**</span><span class="sxs-lookup"><span data-stu-id="4fc5e-104">**At this time we are no longer onboarding any new Data Service publishers. New dataservices will not get approved for listing.**</span></span> <span data-ttu-id="4fc5e-105">Pokud máte SaaS obchodní aplikace, který chcete publikovat na AppSource můžete najít další informace [zde](https://appsource.microsoft.com/partners).</span><span class="sxs-lookup"><span data-stu-id="4fc5e-105">If you have a SaaS business application you would like to publish on AppSource you can find more information [here](https://appsource.microsoft.com/partners).</span></span> <span data-ttu-id="4fc5e-106">Pokud máte IaaS aplikace nebo služby vývojáře, které chcete publikovat na webu Azure Marketplace můžete najít další informace [zde](https://azure.microsoft.com/marketplace/programs/certified/).</span><span class="sxs-lookup"><span data-stu-id="4fc5e-106">If you have an IaaS applications or developer service you would like to publish on Azure Marketplace you can find more information [here](https://azure.microsoft.com/marketplace/programs/certified/).</span></span>
> 
> 

<span data-ttu-id="4fc5e-107">Po dokončení kroku 1, [vytváření účtů a jejich registrace](marketplace-publishing-accounts-creation-registration.md), jsme na základě vás [obecné netechnické](marketplace-publishing-pre-requisites.md) a [technické požadavky](marketplace-publishing-data-service-creation-prerequisites.md) z datové služby nabídku Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-107">After completing the step 1, [Account Creation and Registration](marketplace-publishing-accounts-creation-registration.md), we guided you through the [General Non-Technical](marketplace-publishing-pre-requisites.md) and [Technical Requirements](marketplace-publishing-data-service-creation-prerequisites.md) of a Data Service offer on Azure Marketplace.</span></span> <span data-ttu-id="4fc5e-108">Nyní jsme vás provede kroky pro vytvoření nabídka služby Data na [publikování portál] [ link-pubportal] pro Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-108">Now we will walk you through the steps for creating a Data Service offer on the [Publishing Portal][link-pubportal] for the Azure Marketplace.</span></span>

## <a name="1----login-to-the-publishing-portal"></a><span data-ttu-id="4fc5e-109">1.    Přihlášení k publikování portálu.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-109">1.    Login to the Publishing Portal.</span></span>
<span data-ttu-id="4fc5e-110">Přejděte na [https://publish.windowsazure.com](https://publish.windowsazure.com.)</span><span class="sxs-lookup"><span data-stu-id="4fc5e-110">Go to [https://publish.windowsazure.com](https://publish.windowsazure.com.)</span></span>

<span data-ttu-id="4fc5e-111">**Pro první čas přihlášení na portál publikování použijte stejný účet, pomocí kterého byl zaregistrován vaší společnosti prodejce profil ve středisku pro vývojáře.**</span><span class="sxs-lookup"><span data-stu-id="4fc5e-111">**For first time login to Publishing Portal, use the same account with which your company’s Seller Profile was registered in Developer Center.**</span></span>  <span data-ttu-id="4fc5e-112">(Později můžete přidat všechny zaměstnance ve vaší společnosti jako spolusprávce publikování portálu).</span><span class="sxs-lookup"><span data-stu-id="4fc5e-112">(Later you can add any employee of your company as a co-admin in the Publishing Portal).</span></span>

<span data-ttu-id="4fc5e-113">Klikněte na **publikovat Data Services** dlaždici, pokud je to první přihlášení na portál pro publikování.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-113">Click on the **Publish a Data Services** tile if this is the first login into the publishing portal.</span></span>

## <a name="2----choose-data-services-in-the-navigation-menu-on-the-left-side"></a><span data-ttu-id="4fc5e-114">2.    Zvolte **datové služby** v navigační nabídce na levé straně.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-114">2.    Choose **Data Services** in the navigation menu on the left side.</span></span>
  ![Kreslení](media/marketplace-publishing-data-service-creation/pubportal-main-nav.png)

## <a name="3----create-a-new-data-service"></a><span data-ttu-id="4fc5e-116">3.    Vytvoření nové služby dat</span><span class="sxs-lookup"><span data-stu-id="4fc5e-116">3.    Create a New Data Service</span></span>
<span data-ttu-id="4fc5e-117">Zadejte název pro vaše nové dat služby nabízejí a klikněte na "+" na pravé straně.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-117">Fill in the title for your new Data Service Offer and click on “+” on the right.</span></span>

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-3.png)

## <a name="4----review-the-sub-menu-under-the-newly-created-data-service-in-the-navigation-menu"></a><span data-ttu-id="4fc5e-119">4.    Zkontrolujte dílčí nabídky pod nově vytvořený službou Data v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-119">4.    Review the sub-menu under the newly created Data Service in the navigation menu.</span></span>
<span data-ttu-id="4fc5e-120">Klikněte na **návod** a zkontrolujte všechny potřebné kroky potřebné k publikování správně službu dat v Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-120">Click on the **Walkthrough** tab and review all necessary steps needed to publish properly the Data Service on the Azure Marketplace.</span></span>

> [!TIP]
> <span data-ttu-id="4fc5e-121">Vždy můžete kliknutím na odkazy na stránce "Postup" nebo použití karet na nabídku služby Data dílčí nabídky na levé straně.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-121">You always can click on the links in the “Walkthrough” page or use tabs on the Data Service offer’s sub-menu on the left side.</span></span>
> 
> 

## <a name="5----create-a-new-plan"></a><span data-ttu-id="4fc5e-122">5.    Vytvořte nový plán.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-122">5.    Create a new Plan.</span></span>
### <a name="offers-plans-transactions"></a><span data-ttu-id="4fc5e-123">Nabízí, plánů, transakce.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-123">Offers, Plans, transactions.</span></span>
<span data-ttu-id="4fc5e-124">Každý nabídka může mít více plánů, ale musí mít alespoň jedna (1) plánu.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-124">Each Offer can have multiple Plans, but must have at least one (1) Plan.</span></span> <span data-ttu-id="4fc5e-125">Když koncoví uživatelé přihlášení k odběru na vaši nabídku přihlášení odběru pro jednu nabídku na plánu.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-125">When end-users subscribe to your offer they subscribe for one of the offer’s Plan.</span></span> <span data-ttu-id="4fc5e-126">Každý plán definuje, jak koncoví uživatelé budou moci používat vaši službu.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-126">Each plan defines how end-users will be able to use your service.</span></span>

<span data-ttu-id="4fc5e-127">Aktuálně Azure Marketplace podporovat pouze měsíční předplatné transakce založené model pro datové služby, tj. koncoví uživatelé budou platit měsíční poplatek podle cenu konkrétní plán, který se přihlásit k odběru a budou moci využívat každý měsíc počet transakcí definované v plánu.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-127">Currently Azure Marketplace support only Monthly Subscription Transaction Based model for Data Services, i.e. end-users will pay monthly fee according to the price of the specific plan they subscribed to and will be able to consume each month number of transaction defined by the plan.</span></span>

<span data-ttu-id="4fc5e-128">Každou transakci, obvykle definovány jako počet záznamů, které vrátí Data služby založené na dotazu, odešle do služby.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-128">Each Transaction usually defined as number of records your Data Service will return based on the query sent to the Service.</span></span> <span data-ttu-id="4fc5e-129">Výchozí hodnota je 100.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-129">The default is 100.</span></span> <span data-ttu-id="4fc5e-130">Počet transakcí vrátil pro každý dotaz musí být počet záznamů dělený 100 a zaokrouhlené nahoru na nejbližší celé číslo.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-130">Number of transactions returned to each query will be number of records divided by 100 and rounded up to the closest integer.</span></span>

<span data-ttu-id="4fc5e-131">Je odpovědností vrstvy služby Azure Marketplace monitorování (monitorování) počet transakcí spotřebovávají každý dotaz.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-131">It’s Azure Marketplace Service layer responsibility to monitor (meter) number of transactions consumed by each query.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4fc5e-132">Koncoví uživatelé, které bylo dosaženo limitu transakce v měsíci se bude blokovat pokračovat v používání služby až konce jejich měsíčním cyklu předplatného.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-132">End-Users which reached the transaction limit during the month will be blocked from continuing to use the service until end of their monthly subscription cycle.</span></span>
> 
> <span data-ttu-id="4fc5e-133">Plán nebo jeden z plánů může (ale není nutné) obsahovat neomezený počet transakcí.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-133">The plan or one of the plans can (but not must) include unlimited number of transactions.</span></span>
> 
> 

### <a name="create-a-plan"></a><span data-ttu-id="4fc5e-134">Vytvoření plánu.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-134">Create a plan.</span></span>
1. <span data-ttu-id="4fc5e-135">Klikněte na **"+"** vedle "Přidat nový plán".</span><span class="sxs-lookup"><span data-stu-id="4fc5e-135">Click on **“+”** next to the “Add a new plan”.</span></span>
2. <span data-ttu-id="4fc5e-136">Vyberte jednu z možností: **neomezený** nebo **omezené** využití pro tento plán.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-136">Choose one of the options: **Unlimited** or **Limited** usage for this plan.</span></span>  <span data-ttu-id="4fc5e-137">Pokud omezené potom zadejte počet transakcí s plánem vám umožní využívat za měsíc.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-137">If Limited then provide the number of transaction the plan will allow to consume in a month.</span></span>
   
    ![Kreslení](media/marketplace-publishing-data-service-creation/step-5.1.png)  
   
    <span data-ttu-id="4fc5e-139">Publikování portál také navrhnout "Plán identifikátor", který bude používaný pro komunikaci se koncoví uživatelé název plánu v uživatelském rozhraní a také používá služba Marketplace k identifikaci plánu.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-139">Publishing Portal will also suggest “Plan Identifier”, which will be used to communicate to the end-users the name of the plan in the UI and also used by the Market Place Service to identify the Plan.</span></span> <span data-ttu-id="4fc5e-140">Pokud chcete, můžete změnit "Plánování identifikátor".</span><span class="sxs-lookup"><span data-stu-id="4fc5e-140">You can change the “Plan Identifier” if you want.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="4fc5e-141">"Plánování identifikátor" musí být jedinečný v rámci oboru každou nabídku.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-141">The “Plan Identifier” must be unique within the scope of each offer.</span></span> <span data-ttu-id="4fc5e-142">Po první publikování do výroby a nebude možné změnit tento identifikátor se uzamkne jako mnoho dalších identifikátorů použít v identifikátoru plánování publikování portálu.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-142">As many other Identifiers used in the Publishing Portal Plan identifier will be locked after the first publishing to production and you will not be able to change this identifier.</span></span>
   > 
   > 
3. <span data-ttu-id="4fc5e-143">Kliknutím na tlačítko Přijmout svého výběru.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-143">Click to accept your choice.</span></span>
4. <span data-ttu-id="4fc5e-144">Potom budete požádáni několik další otázky týkající se nově vytvořená plánu.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-144">Then you will be asked few additional questions regarding your newly created Plan.</span></span>
   
    ![Kreslení](media/marketplace-publishing-data-service-creation/step-5.2.png)

| <span data-ttu-id="4fc5e-146">Otázky</span><span class="sxs-lookup"><span data-stu-id="4fc5e-146">Question</span></span> | <span data-ttu-id="4fc5e-147">Násobek.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-147">Significance</span></span> |
| --- | --- |
| <span data-ttu-id="4fc5e-148">**Tento plán je volné a dostupné celosvětové?**</span><span class="sxs-lookup"><span data-stu-id="4fc5e-148">**This Plan is free and available world-wide?**</span></span> |<span data-ttu-id="4fc5e-149">Můžete vytvořit plán úplně bez z – zdarma.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-149">You can create a completely free-of-charge plan.</span></span> <span data-ttu-id="4fc5e-150">Pokud se jedná pouze plán k této nabídce –, znamená to, že publikujete "Volné nabízejí" na webu Marketplace.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-150">If it’s the only plan for this offer – it means that you are publishing “Free Offer” in the Marketplace.</span></span> <span data-ttu-id="4fc5e-151">Pokud se jedná pouze pro jeden (z několika) plán, it vám dává možnost nabízí koncovým uživatelům, aby další informace o služby s relativně malý počet transakcí za měsíc.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-151">If it’s only for one (of few) Plan, the it gives you an option to offer end-users to learn more about your service with a relatively small number of transactions per month.</span></span>  <span data-ttu-id="4fc5e-152">Pokud je odpověď "Ano", bude se dotaz žádné další otázky.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-152">If the answer is "Yes," then no further questions will be asked.</span></span> |

> [!NOTE]
> <span data-ttu-id="4fc5e-153">Koncoví uživatelé můžete vždy upgradovat na placené plány.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-153">End users can always upgrade to the paid plans.</span></span>
> 
> 

| <span data-ttu-id="4fc5e-154">Otázky</span><span class="sxs-lookup"><span data-stu-id="4fc5e-154">Question</span></span> | <span data-ttu-id="4fc5e-155">Násobek.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-155">Significance</span></span> |
| --- | --- |
| <span data-ttu-id="4fc5e-156">**Je k dispozici bezplatná zkušební verze?**</span><span class="sxs-lookup"><span data-stu-id="4fc5e-156">**Is free trial available?**</span></span> |<span data-ttu-id="4fc5e-157">Můžete vybrat mezi "Ne zkušební verzi" vůbec nebo poskytnout možnost k použití vašeho plánu pro "Jeden měsíc".</span><span class="sxs-lookup"><span data-stu-id="4fc5e-157">You can choose between “No Trial” at all or give an option to use your Plan for “One Month”.</span></span> <span data-ttu-id="4fc5e-158">Vydavatelé se chtěli použít tuto možnost dát koncovým uživatelům možnost pochopit výhody sady nabídku zdarma pro jeden měsíc.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-158">Publishers like to use this option to provide end-users the possibility to understand the benefits of the offer for free for one month.</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="4fc5e-159">Koncoví uživatelé budou pouze moct zakoupit bezplatnou zkušební verzi, pokud budou mít vytvořené platebním nástrojem například platební karty, smlouvy enterprise.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-159">End-users will only be able to purchase a free trial if they have established payment instrument e.g. credit card, enterprise agreement.</span></span>
> 
> <span data-ttu-id="4fc5e-160">Po jednom měsíci z bezplatné zkušební verze Azure Marketplace se spustí zákazníkům poplatků za cenu k datu odběru, pokud zákazník iniciované zrušení předplatného.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-160">After one month of the free trial, Azure Marketplace will start charging customers the price as of the date of the subscription, unless the customer initiated the subscription cancellation.</span></span> <span data-ttu-id="4fc5e-161">Žádná speciální oznámení bude potřeba poskytnout koncovým uživatelům.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-161">No special notification will be provided to the end-users.</span></span>
> 
> 

| <span data-ttu-id="4fc5e-162">Otázky</span><span class="sxs-lookup"><span data-stu-id="4fc5e-162">Question</span></span> | <span data-ttu-id="4fc5e-163">Násobek.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-163">Significance</span></span> |
| --- | --- |
| <span data-ttu-id="4fc5e-164">**Tento plán vyžaduje propagační kód, chcete-li zakoupit?**</span><span class="sxs-lookup"><span data-stu-id="4fc5e-164">**This plan requires a promotion code to purchase?**</span></span> |<span data-ttu-id="4fc5e-165">Vydavatelé mít možnost omezit přístup k jejich plány služby tím, že poskytuje speciální kódu, nazývají "A Promocode" pro konkrétní zákazníky.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-165">Publishers have an option to limit access to their Service Plans by providing a special code, called “A Promocode” to specific customers.</span></span> <span data-ttu-id="4fc5e-166">Pouze koncoví uživatelé, kteří budou mít tento Promocode budou moci přihlásit do plánu.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-166">Only end-users which will have this Promocode will be able to subscribe to the Plan.</span></span> <span data-ttu-id="4fc5e-167">Pokud zvolíte "Ne", pak souhlasíte s tím, každý z oblasti, kde je k dispozici nabídku (najdete v části [Marketplace Marketing obsahu Průvodce](marketplace-publishing-push-to-staging.md) podrobnosti) budou moci přihlásit na tento plán.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-167">If you choose “No”, then you agree that everyone from the region where the offer is available (See [Marketplace Marketing Content Guide](marketplace-publishing-push-to-staging.md) for more details) will be able to subscribe to this plan.</span></span> <span data-ttu-id="4fc5e-168">Žádné další otázky se dotaz.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-168">No further questions will be asked.</span></span> |
| <span data-ttu-id="4fc5e-169">**Také skrýt tento plán z každý, kdo nemá platný propagační kód?**</span><span class="sxs-lookup"><span data-stu-id="4fc5e-169">**Also hide this plan from anyone who doesn’t have a valid promotion code?**</span></span> |<span data-ttu-id="4fc5e-170">Pokud je odpověď na otázku předchozí "Ano" vydavatele má možnost úplně odebrat tento plán zobrazování v uživatelském rozhraní Marketplace.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-170">If the answer to the previous question is “Yes” the Publisher has an option to completely remove this plan from appearing in the UI of the Marketplace.</span></span> <span data-ttu-id="4fc5e-171">Znamená to, zákazníci neuvidí tento plán nabídku na stránce Podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-171">It means, customers will not see this plan in the Offer’s details page.</span></span> <span data-ttu-id="4fc5e-172">Koncoví uživatelé, kteří obdrží promocode o zakoupení, budou moci přihlásit se pomocí této promocode.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-172">End-users which will receive a promocode to purchase it, will be able to subscribe to it using this promocode.</span></span> |

## <a name="6----create-your-marketplace-marketing-content"></a><span data-ttu-id="4fc5e-173">6.    Vytvoření vašeho webu Marketplace marketingové obsahu</span><span class="sxs-lookup"><span data-stu-id="4fc5e-173">6.    Create your Marketplace marketing content</span></span>
<span data-ttu-id="4fc5e-174">Postup zadejte informace v **Marketing, Cenová, podpory a kategorie** karty navštivte [Marketplace Marketing obsahu Průvodce](marketplace-publishing-push-to-staging.md) tedy společné pro všechny artefakty publikována ve službě Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-174">For How to provide information required in **Marketing, Pricing, Support and Categories** tabs please visit [Marketplace Marketing Content Guide](marketplace-publishing-push-to-staging.md) which is common to all artifacts published in the Azure Marketplace.</span></span>  

## <a name="7----connect-your-offer-to-your-service-sql-azure-based-or-web-service-based"></a><span data-ttu-id="4fc5e-175">7.    Vaši nabídku připojení k službě (na základě SQL Azure nebo na základě webové služby).</span><span class="sxs-lookup"><span data-stu-id="4fc5e-175">7.    Connect your offer to your Service (SQL Azure based or Web Service based).</span></span>
<span data-ttu-id="4fc5e-176">Klikněte na **datové služby** dílčí nabídky.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-176">Click on the **Data Services** sub-menu.</span></span>

<span data-ttu-id="4fc5e-177">V horní polovině stránky budete požádáni o zadání nabídka **Namespace**.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-177">On the upper half of the page you’ll be asked to provide the offer’s **Namespace**.</span></span>  

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.png)

<span data-ttu-id="4fc5e-179">Následující otázky bude definovat jak vydavatele bude nově vytvořený zveřejněte nabídku na webu Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-179">The below question will define how the Publisher is going to expose newly created offer to Azure Marketplace.</span></span> <span data-ttu-id="4fc5e-180">(Podrobnosti najdete v části [Data služby technické požadovaných průvodce](marketplace-publishing-data-service-creation-prerequisites.md)).</span><span class="sxs-lookup"><span data-stu-id="4fc5e-180">(For more details see the [Data Services Technical Prerequisite Guide](marketplace-publishing-data-service-creation-prerequisites.md)).</span></span>

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.2.png)

<span data-ttu-id="4fc5e-182">**Publikování databáze na základě služby**</span><span class="sxs-lookup"><span data-stu-id="4fc5e-182">**Publishing the Database based service**</span></span>

<span data-ttu-id="4fc5e-183">Klikněte na **databáze**.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-183">Click on **Database**.</span></span> <span data-ttu-id="4fc5e-184">Zobrazí se následující stránka:</span><span class="sxs-lookup"><span data-stu-id="4fc5e-184">The following page will appear:</span></span>

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.3.png)

<span data-ttu-id="4fc5e-186">Vytvořit mapování CSDL datové sady založené na databázi SQL Azure:</span><span class="sxs-lookup"><span data-stu-id="4fc5e-186">To create a CSDL mapping for the Dataset based on the SQL Azure DB:</span></span>

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.4.png)

<span data-ttu-id="4fc5e-188">A potom pro každou tabulku</span><span class="sxs-lookup"><span data-stu-id="4fc5e-188">And then for each table</span></span>

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.5.png)

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.6.png)

<span data-ttu-id="4fc5e-191">Pokud webová služba</span><span class="sxs-lookup"><span data-stu-id="4fc5e-191">If Web Service</span></span>

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.7.png)

> [!IMPORTANT]
> <span data-ttu-id="4fc5e-193">Čtení [mapování existující webové služby OData prostřednictvím CSDL](marketplace-publishing-data-service-creation-odata-mapping.md) podrobné pokyny a příklady pro vytváření CSDL webové služby.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-193">Read [Mapping an existing web service to OData through CSDL](marketplace-publishing-data-service-creation-odata-mapping.md) for detailed instructions and examples for creating a CSDL Web Service.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="4fc5e-194">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4fc5e-194">Next Steps</span></span>
<span data-ttu-id="4fc5e-195">Teď, když jste vytvořili vaši nabídku Data Service, zkontrolujte, že provedete podle pokynů v [Marketplace Marketing obsahu Průvodce](marketplace-publishing-push-to-staging.md) předtím, než můžete přejít ke [testování služby dat v pracovním](marketplace-publishing-data-service-test-in-staging.md).</span><span class="sxs-lookup"><span data-stu-id="4fc5e-195">Now that you've created your Data Service offer, please ensure that you complete the instructions in the [Marketplace Marketing Content Guide](marketplace-publishing-push-to-staging.md) before you move forward to [Testing your Data Service in Staging](marketplace-publishing-data-service-test-in-staging.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="4fc5e-196">Viz také</span><span class="sxs-lookup"><span data-stu-id="4fc5e-196">See Also</span></span>
* [<span data-ttu-id="4fc5e-197">Začínáme: Jak publikování nabídky pro Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="4fc5e-197">Getting Started: How to publish an offer to the Azure Marketplace</span></span>](marketplace-publishing-getting-started.md)
* <span data-ttu-id="4fc5e-198">Pokud vás zajímá porozumět celkový proces mapování OData a účel, přečtěte si tento článek [mapování dat služby OData](marketplace-publishing-data-service-creation-odata-mapping.md) ke kontrole definice struktury a pokynů.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-198">If you are interested in understanding the overall OData mapping process and purpose, read this article [Data Service OData Mapping](marketplace-publishing-data-service-creation-odata-mapping.md) to review definitions, structures, and instructions.</span></span>
* <span data-ttu-id="4fc5e-199">Pokud vás zajímá učení a seznámit se s konkrétním uzlům a jejich parametrů, přečtěte si tento článek [datové služby OData mapování uzly](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) pro definice a vysvětlení, příklady a kontext případů použití.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-199">If you are interested in learning and understanding the specific nodes and their parameters, read this article [Data Service OData Mapping Nodes](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) for definitions and explanations, examples, and use case context.</span></span>
* <span data-ttu-id="4fc5e-200">Pokud vás zajímá kontrola příklady, přečtěte si tento článek [Data služby OData mapování příklady](marketplace-publishing-data-service-creation-odata-mapping-examples.md) najdete ukázkový kód a pochopit syntaxe kódu a kontext.</span><span class="sxs-lookup"><span data-stu-id="4fc5e-200">If you are interested in reviewing examples, read this article [Data Service OData Mapping Examples](marketplace-publishing-data-service-creation-odata-mapping-examples.md) to see sample code and understand code syntax and context.</span></span>

[link-pubportal]:https://publish.windowsazure.com
