---
title: "aaaScenario - vytvořit zákazníka přehledný řídicí panel s bez serveru Azure | Microsoft Docs"
description: "Příklad zákazník toomanage řídicího panelu můžete vytvořit, zpětnou vazbu, sociální data a další s funkcemi Azure a Azure Logic Apps."
keywords: 
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/29/2017
ms.author: jehollan
ms.openlocfilehash: db175e895e37aa795a9c34bf4d65566bf68f8c37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-real-time-customer-insights-dashboard-with-azure-logic-apps-and-azure-functions"></a><span data-ttu-id="e56e8-103">Vytvořte v reálném čase zákazníka přehledný řídicí panel s funkcemi Azure a Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="e56e8-103">Create a real-time customer insights dashboard with Azure Logic Apps and Azure Functions</span></span>

<span data-ttu-id="e56e8-104">Nástroje Azure bez serveru zadejte výkonné možnosti tooquickly sestavení a aplikacím v cloudu hello bez nutnosti toothink infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="e56e8-104">Azure Serverless tools provide powerful capabilities tooquickly build and host applications in hello cloud, without having toothink about infrastructure.</span></span>  <span data-ttu-id="e56e8-105">V tomto scénáři jsme bude vytvoření tootrigger řídicí panel názorů zákazníků, analýzu zpětnou vazbu pomocí machine learning a publikování Statistika zdroj jako Power BI nebo Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="e56e8-105">In this scenario, we will create a dashboard tootrigger on customer feedback, analyze feedback with machine learning, and publish insights a source like Power BI or Azure Data Lake.</span></span>

## <a name="overview-of-hello-scenario-and-tools-used"></a><span data-ttu-id="e56e8-106">Přehled hello scénář a použít nástroje</span><span class="sxs-lookup"><span data-stu-id="e56e8-106">Overview of hello scenario and tools used</span></span>

<span data-ttu-id="e56e8-107">V pořadí tooimplement toto řešení, jsme využije hello dvě klíčové komponenty aplikace bez serveru v Azure: [Azure Functions](https://azure.microsoft.com/services/functions/) a [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).</span><span class="sxs-lookup"><span data-stu-id="e56e8-107">In order tooimplement this solution, we will leverage hello two key components of serverless apps in Azure: [Azure Functions](https://azure.microsoft.com/services/functions/) and [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).</span></span>

<span data-ttu-id="e56e8-108">Služba Logic Apps je modul bez serveru pracovního postupu v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="e56e8-108">Logic Apps is a serverless workflow engine in hello cloud.</span></span>  <span data-ttu-id="e56e8-109">Poskytuje orchestration mezi komponentami bez serveru a také připojí tooover 100 služeb a rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e56e8-109">It provides orchestration across serverless components, and also connects tooover 100 services and APIs.</span></span>  <span data-ttu-id="e56e8-110">V tomto scénáři vytvoříme tootrigger aplikace logiky na zpětnou vazbu od zákazníků.</span><span class="sxs-lookup"><span data-stu-id="e56e8-110">For this scenario, we will create a logic app tootrigger on feedback from customers.</span></span>  <span data-ttu-id="e56e8-111">Mezi hello konektorů, které se podílejí na zpětnou vazbu reagující toocustomer patří Outlook.com, Office 365, opic průzkum, Twitter a požadavek HTTP [z webového formuláře](https://blogs.msdn.microsoft.com/logicapps/2017/01/30/calling-a-logic-app-from-an-html-form/).</span><span class="sxs-lookup"><span data-stu-id="e56e8-111">Some of hello connectors that can assist in reacting toocustomer feedback include Outlook.com, Office 365, Survey Monkey, Twitter, and an HTTP Request [from a web form](https://blogs.msdn.microsoft.com/logicapps/2017/01/30/calling-a-logic-app-from-an-html-form/).</span></span>  <span data-ttu-id="e56e8-112">Pro pracovní postup hello níže jsme monitorovat hashtag na Twitteru.</span><span class="sxs-lookup"><span data-stu-id="e56e8-112">For hello workflow below, we will monitor a hashtag on Twitter.</span></span>

<span data-ttu-id="e56e8-113">Funkce zajistit bez serveru výpočetní v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="e56e8-113">Functions provide serverless compute in hello cloud.</span></span>  <span data-ttu-id="e56e8-114">V tomto scénáři budeme používat Azure Functions tooflag tweetů od zákazníků založené na řadu předem definovaná klíčová slova.</span><span class="sxs-lookup"><span data-stu-id="e56e8-114">In this scenario, we will use Azure Functions tooflag tweets from customers based on a series of pre-defined key words.</span></span>

<span data-ttu-id="e56e8-115">může být celé řešení Hello [sestavení v sadě Visual Studio](logic-apps-deploy-from-vs.md) a [nasazen jako součást šablony prostředků](logic-apps-create-deploy-template.md).</span><span class="sxs-lookup"><span data-stu-id="e56e8-115">hello entire solution can be [build in Visual Studio](logic-apps-deploy-from-vs.md) and [deployed as part of a resource template](logic-apps-create-deploy-template.md).</span></span>  <span data-ttu-id="e56e8-116">Je také video s návodem hello scénář [na webu Channel 9](http://aka.ms/logicappsdemo).</span><span class="sxs-lookup"><span data-stu-id="e56e8-116">There is also video walkthrough of hello scenario [on Channel 9](http://aka.ms/logicappsdemo).</span></span>

## <a name="build-hello-logic-app-tootrigger-on-customer-data"></a><span data-ttu-id="e56e8-117">Sestavení hello logiku aplikace tootrigger na data zákazníků</span><span class="sxs-lookup"><span data-stu-id="e56e8-117">Build hello logic app tootrigger on customer data</span></span>

<span data-ttu-id="e56e8-118">Po [vytvoření aplikace logiky](logic-apps-create-a-logic-app.md) v sadě Visual Studio nebo hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="e56e8-118">After [creating a logic app](logic-apps-create-a-logic-app.md) in Visual Studio or hello Azure portal:</span></span>

1. <span data-ttu-id="e56e8-119">Přidat aktivační událost pro **na nové Tweetů** ze služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="e56e8-119">Add a trigger for **On New Tweets** from Twitter</span></span>
2. <span data-ttu-id="e56e8-120">Nakonfigurujte hello aktivační událost toolisten tootweets na – klíčové slovo nebo hashtag.</span><span class="sxs-lookup"><span data-stu-id="e56e8-120">Configure hello trigger toolisten tootweets on a keyword or hashtag.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e56e8-121">Vlastnost opakování Hello na aktivační událost hello určí, jak často se aplikace logiky hello kontrolovat přítomnost nových položek na základě cyklického dotazování aktivační události</span><span class="sxs-lookup"><span data-stu-id="e56e8-121">hello recurrence property on hello trigger will determine how frequently hello logic app checks for new items on polling-based triggers</span></span>

   ![Příklad aktivační události služby Twitter.][1]

<span data-ttu-id="e56e8-123">Tato aplikace bude nyní platit na všechny nové tweetů.</span><span class="sxs-lookup"><span data-stu-id="e56e8-123">This app will now fire on all new tweets.</span></span>  <span data-ttu-id="e56e8-124">Pak můžeme trvat dat tweet a lépe hello postojích vyjádřené porozumět.</span><span class="sxs-lookup"><span data-stu-id="e56e8-124">We can then take that tweet data and understand more of hello sentiment expressed.</span></span>  <span data-ttu-id="e56e8-125">Pro tento používáme hello [kognitivní služby Azure](https://azure.microsoft.com/services/cognitive-services/) toodetect postojích textu.</span><span class="sxs-lookup"><span data-stu-id="e56e8-125">For this we use hello [Azure Cognitive Service](https://azure.microsoft.com/services/cognitive-services/) toodetect sentiment of text.</span></span>

1. <span data-ttu-id="e56e8-126">Klikněte na tlačítko **nový krok**</span><span class="sxs-lookup"><span data-stu-id="e56e8-126">Click **New Step**</span></span>
1. <span data-ttu-id="e56e8-127">Vyberte nebo vyhledejte hello **Analýza textu** konektoru</span><span class="sxs-lookup"><span data-stu-id="e56e8-127">Select or search for hello **Text Analytics** connector</span></span>
1. <span data-ttu-id="e56e8-128">Vyberte hello **zjistit postojích** operace</span><span class="sxs-lookup"><span data-stu-id="e56e8-128">Select hello **Detect Sentiment** operation</span></span>
1. <span data-ttu-id="e56e8-129">Pokud se zobrazí výzva, zadejte platný klíč kognitivní služby pro hello služba Analýza textu</span><span class="sxs-lookup"><span data-stu-id="e56e8-129">If prompted, provide a valid Cognitive Services key for hello Text Analytics service</span></span>
1. <span data-ttu-id="e56e8-130">Přidat hello **Tweet Text** jako hello text tooanalyze.</span><span class="sxs-lookup"><span data-stu-id="e56e8-130">Add hello **Tweet Text** as hello text tooanalyze.</span></span>

<span data-ttu-id="e56e8-131">Teď, když máme hello tweet dat a statistika na hello tweet, může být několik dalších konektorů relevantní:</span><span class="sxs-lookup"><span data-stu-id="e56e8-131">Now that we have hello tweet data, and insights on hello tweet, a number of other connectors may be relevant:</span></span>
* <span data-ttu-id="e56e8-132">Power BI - přidat řádky tooStreaming datovou sadu: zobrazení tweetů reálném čase na řídicí panel Power BI.</span><span class="sxs-lookup"><span data-stu-id="e56e8-132">Power BI - Add Rows tooStreaming Dataset: View tweets real time on a Power BI dashboard.</span></span>
* <span data-ttu-id="e56e8-133">Azure Data Lake - připojit soubor: Přidání zákazníka data tooan Azure Data Lake datovou sadu tooinclude v úloh analytics.</span><span class="sxs-lookup"><span data-stu-id="e56e8-133">Azure Data Lake - Append file: Add customer data tooan Azure Data Lake dataset tooinclude in analytics jobs.</span></span>
* <span data-ttu-id="e56e8-134">SQL - přidávání řádků: ukládání dat v databázi pro pozdější načtení.</span><span class="sxs-lookup"><span data-stu-id="e56e8-134">SQL - Add rows: Store data in a database for later retrieval.</span></span>
* <span data-ttu-id="e56e8-135">Slack – poslat zprávu: výstrahy slack kanál na záporné zpětnou vazbu, která vyžaduje akce.</span><span class="sxs-lookup"><span data-stu-id="e56e8-135">Slack - Send message: Alert a slack channel on negative feedback that requires actions.</span></span>

<span data-ttu-id="e56e8-136">Funkce Azure může být také použít toodo další vlastní výpočetní nad hello data.</span><span class="sxs-lookup"><span data-stu-id="e56e8-136">An Azure Function can also be used toodo more custom compute on top of hello data.</span></span>

## <a name="enriching-hello-data-with-an-azure-function"></a><span data-ttu-id="e56e8-137">Rozšíření dat hello s funkcí Azure</span><span class="sxs-lookup"><span data-stu-id="e56e8-137">Enriching hello data with an Azure Function</span></span>

<span data-ttu-id="e56e8-138">Můžeme vytvořit funkci, potřebujeme toohave aplikaci funkce v naše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="e56e8-138">Before we can create a function, we need toohave a function app in our Azure subscription.</span></span>  <span data-ttu-id="e56e8-139">Informace o vytvoření funkce Azure hello portálu můžete [se nachází tady](../azure-functions/functions-create-first-azure-function-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="e56e8-139">Details on creating an Azure Function in hello portal can [be found here](../azure-functions/functions-create-first-azure-function-azure-portal.md)</span></span>

<span data-ttu-id="e56e8-140">Pro volání přímo z aplikace logiky toobe funkce se vyžaduje toohave HTTP aktivovat vazby.</span><span class="sxs-lookup"><span data-stu-id="e56e8-140">For a function toobe called directly from a logic app, it needs toohave an HTTP trigger binding.</span></span>  <span data-ttu-id="e56e8-141">Doporučujeme používat hello **HttpTrigger** šablony.</span><span class="sxs-lookup"><span data-stu-id="e56e8-141">We recommend using hello **HttpTrigger** template.</span></span>

<span data-ttu-id="e56e8-142">V tomto scénáři by tělo žádosti hello hello funkce Azure hello tweet text.</span><span class="sxs-lookup"><span data-stu-id="e56e8-142">In this scenario, hello request body of hello Azure Function would be hello tweet text.</span></span>  <span data-ttu-id="e56e8-143">V kódu funkce hello jednoduše definujte logiku Pokud hello tweet text obsahuje klíčové slovo nebo frázi.</span><span class="sxs-lookup"><span data-stu-id="e56e8-143">In hello function code, simply define logic on if hello tweet text contains a key word or phrase.</span></span>  <span data-ttu-id="e56e8-144">samotná Funkce Hello může uchovávat jako jednoduché nebo komplexní, podle potřeby pro scénář hello.</span><span class="sxs-lookup"><span data-stu-id="e56e8-144">hello function itself could be kept as simple or complex as needed for hello scenario.</span></span>

<span data-ttu-id="e56e8-145">Na konci hello hello funkce jednoduše vrátí odpověď aplikace logiky toohello určitými daty.</span><span class="sxs-lookup"><span data-stu-id="e56e8-145">At hello end of hello function, simply return a response toohello logic app with some data.</span></span>  <span data-ttu-id="e56e8-146">To může být jednoduchý logickou hodnotu (například `containsKeyword`), nebo komplexní objekt.</span><span class="sxs-lookup"><span data-stu-id="e56e8-146">This could be a simple boolean value (e.g. `containsKeyword`), or a complex object.</span></span>

![Nakonfigurované krok funkce Azure][2]

> [!TIP]
> <span data-ttu-id="e56e8-148">Při přístupu k komplexní odpověď z funkce v aplikaci logiky, pomocí akce hello analyzovat JSON.</span><span class="sxs-lookup"><span data-stu-id="e56e8-148">When accessing a complex response from a function in a logic app, use hello Parse JSON action.</span></span>

<span data-ttu-id="e56e8-149">Po uložení hello funkce mohou být přidány do aplikace logiky hello vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="e56e8-149">Once hello function is saved, it can be added into hello logic app created above.</span></span>  <span data-ttu-id="e56e8-150">V aplikaci logiky hello:</span><span class="sxs-lookup"><span data-stu-id="e56e8-150">In hello logic app:</span></span>

1. <span data-ttu-id="e56e8-151">Klikněte na tlačítko tooadd **nový krok**</span><span class="sxs-lookup"><span data-stu-id="e56e8-151">Click tooadd a **New Step**</span></span>
1. <span data-ttu-id="e56e8-152">Vyberte hello **Azure Functions** konektoru</span><span class="sxs-lookup"><span data-stu-id="e56e8-152">Select hello **Azure Functions** connector</span></span>
1. <span data-ttu-id="e56e8-153">Vyberte toochoose existující funkce a procházet toohello funkce vytvořit</span><span class="sxs-lookup"><span data-stu-id="e56e8-153">Select toochoose an existing function, and browse toohello function created</span></span>
1. <span data-ttu-id="e56e8-154">Odeslat v hello **Tweet Text** pro hello **text žádosti**</span><span class="sxs-lookup"><span data-stu-id="e56e8-154">Send in hello **Tweet Text** for hello **Request Body**</span></span>

## <a name="running-and-monitoring-hello-solution"></a><span data-ttu-id="e56e8-155">Spuštění a sledování řešení hello</span><span class="sxs-lookup"><span data-stu-id="e56e8-155">Running and monitoring hello solution</span></span>

<span data-ttu-id="e56e8-156">Jednou z výhod hello vytváření bez serveru orchestrations v Logic Apps je hello bohaté ladění a možnosti monitorování.</span><span class="sxs-lookup"><span data-stu-id="e56e8-156">One of hello benefits of authoring serverless orchestrations in Logic Apps is hello rich debug and monitoring capabilities.</span></span>  <span data-ttu-id="e56e8-157">Všechny spustit (aktuální nebo historické) lze zobrazit z v sadě Visual Studio, hello portál Azure, nebo prostřednictvím hello REST API a sady SDK.</span><span class="sxs-lookup"><span data-stu-id="e56e8-157">Any run (current or historic) can be viewed from within Visual Studio, hello Azure portal, or via hello REST API and SDKs.</span></span>

<span data-ttu-id="e56e8-158">Jeden z hello nejjednodušší způsoby tootest aplikace logiky používá hello **spustit** tlačítko v Návrháři hello.</span><span class="sxs-lookup"><span data-stu-id="e56e8-158">One of hello easiest ways tootest a logic app is using hello **Run** button in hello designer.</span></span>  <span data-ttu-id="e56e8-159">Kliknutím na tlačítko **spustit** bude pokračovat toopoll hello aktivace každých 5 sekund, dokud je zjištěna událost a poskytnout za provozu zobrazení průběhu hello spustit.</span><span class="sxs-lookup"><span data-stu-id="e56e8-159">Clicking **Run** will continue toopoll hello trigger every 5 seconds until an event is detected, and give a live view as hello run progresses.</span></span>

<span data-ttu-id="e56e8-160">Předchozí spuštění historií lze zobrazit v okně Přehled hello v hello portál Azure nebo pomocí hello cloudu Průzkumníka Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e56e8-160">Previous run histories can be viewed on hello Overview blade in hello Azure portal, or using hello Visual Studio Cloud Explorer.</span></span>

## <a name="creating-a-deployment-template-for-automated-deployments"></a><span data-ttu-id="e56e8-161">Vytvoření šablony nasazení pro automatické nasazení</span><span class="sxs-lookup"><span data-stu-id="e56e8-161">Creating a deployment template for automated deployments</span></span>

<span data-ttu-id="e56e8-162">Jakmile byla vyvinuta řešení, můžete zachytit a nasazují přes tooany šablony nasazení Azure oblast Azure z hello, world.</span><span class="sxs-lookup"><span data-stu-id="e56e8-162">Once a solution has been developed, it can be captured and deployed via an Azure deployment template tooany Azure region in hello world.</span></span>  <span data-ttu-id="e56e8-163">To je užitečné pro oba změny parametry pro různé verze tento pracovní postup, ale také pro integraci toto řešení v kanálu sestavení a verze.</span><span class="sxs-lookup"><span data-stu-id="e56e8-163">This is useful for both modifying parameters for different versions of this workflow, but also for integrating this solution in a build and release pipeline.</span></span>  <span data-ttu-id="e56e8-164">Informace o vytváření šablony nasazení lze nalézt [v tomto článku](logic-apps-create-deploy-template.md).</span><span class="sxs-lookup"><span data-stu-id="e56e8-164">Details on creating a deployment template can be found [in this article](logic-apps-create-deploy-template.md).</span></span>

<span data-ttu-id="e56e8-165">Azure Functions můžete také začlení do šablony nasazení hello - tak hello celé řešení s všechny závislosti můžete spravovat jako jednu šablonu.</span><span class="sxs-lookup"><span data-stu-id="e56e8-165">Azure Functions can also be incorporated in hello deployment template - so hello entire solution with all dependencies can be managed as a single template.</span></span>  <span data-ttu-id="e56e8-166">Příklad funkce šablony nasazení lze nalézt v hello [úložiště šablony Azure rychlý Start](https://github.com/Azure/azure-quickstart-templates/tree/master/101-function-app-create-dynamic).</span><span class="sxs-lookup"><span data-stu-id="e56e8-166">An example of a function deployment template can be found in hello [Azure quickstart template repository](https://github.com/Azure/azure-quickstart-templates/tree/master/101-function-app-create-dynamic).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e56e8-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e56e8-167">Next steps</span></span>

* [<span data-ttu-id="e56e8-168">Zobrazit další příklady a scénáře pro Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="e56e8-168">See other examples and scenarios for Azure Logic Apps</span></span>](logic-apps-examples-and-scenarios.md)
* [<span data-ttu-id="e56e8-169">Podívejte se na video s návodem k vytvoření tohoto řešení pro kompletní</span><span class="sxs-lookup"><span data-stu-id="e56e8-169">Watch a video walkthrough on creating this solution end-to-end</span></span>](http://aka.ms/logicappsdemo)
* [<span data-ttu-id="e56e8-170">Zjistěte, jak toohandle a catch výjimky v rámci aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="e56e8-170">Learn how toohandle and catch exceptions within a logic app</span></span>](logic-apps-exception-handling.md)

<!-- Image References -->
[1]: ./media/logic-apps-scenario-social-serverless/twitter.png
[2]: ./media/logic-apps-scenario-social-serverless/function.png