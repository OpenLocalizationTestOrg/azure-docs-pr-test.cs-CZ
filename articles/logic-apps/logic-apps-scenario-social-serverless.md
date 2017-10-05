---
title: "Scénář – vytvoření zákazníka přehledný řídicí panel s bez serveru Azure | Microsoft Docs"
description: "Příklad, jak můžete vytvořit řídicí panel pro správu názory zákazníků, sociální data a další s funkcemi Azure a Azure Logic Apps."
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
ms.openlocfilehash: 0b6e118cb13ab8185d8eeb42bec6147155967967
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-real-time-customer-insights-dashboard-with-azure-logic-apps-and-azure-functions"></a><span data-ttu-id="55539-103">Vytvořte v reálném čase zákazníka přehledný řídicí panel s funkcemi Azure a Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="55539-103">Create a real-time customer insights dashboard with Azure Logic Apps and Azure Functions</span></span>

<span data-ttu-id="55539-104">Nástroje Azure bez serveru zadejte výkonné možnosti k rychlému vytvoření a hostování aplikací v cloudu, aniž by museli vezměte v úvahu infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="55539-104">Azure Serverless tools provide powerful capabilities to quickly build and host applications in the cloud, without having to think about infrastructure.</span></span>  <span data-ttu-id="55539-105">V tomto scénáři vytvoříme řídicí panel aktivovat názorů zákazníků, analýzu zpětnou vazbu pomocí machine learning a publikování Statistika zdroj jako Power BI nebo Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="55539-105">In this scenario, we will create a dashboard to trigger on customer feedback, analyze feedback with machine learning, and publish insights a source like Power BI or Azure Data Lake.</span></span>

## <a name="overview-of-the-scenario-and-tools-used"></a><span data-ttu-id="55539-106">Přehled scénáře a použít nástroje</span><span class="sxs-lookup"><span data-stu-id="55539-106">Overview of the scenario and tools used</span></span>

<span data-ttu-id="55539-107">Chcete-li implementaci tohoto řešení, jsme využije dvě klíčové komponenty aplikace bez serveru v Azure: [Azure Functions](https://azure.microsoft.com/services/functions/) a [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).</span><span class="sxs-lookup"><span data-stu-id="55539-107">In order to implement this solution, we will leverage the two key components of serverless apps in Azure: [Azure Functions](https://azure.microsoft.com/services/functions/) and [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).</span></span>

<span data-ttu-id="55539-108">Služba Logic Apps je modul bez serveru pracovního postupu v cloudu.</span><span class="sxs-lookup"><span data-stu-id="55539-108">Logic Apps is a serverless workflow engine in the cloud.</span></span>  <span data-ttu-id="55539-109">Poskytuje orchestration mezi komponentami bez serveru a také se připojuje k více než 100 služeb a rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="55539-109">It provides orchestration across serverless components, and also connects to over 100 services and APIs.</span></span>  <span data-ttu-id="55539-110">V tomto scénáři vytvoříme aplikace logiky k aktivaci na zpětnou vazbu od zákazníků.</span><span class="sxs-lookup"><span data-stu-id="55539-110">For this scenario, we will create a logic app to trigger on feedback from customers.</span></span>  <span data-ttu-id="55539-111">Mezi konektory, které může být užitečné při reaktivní na názory zákazníků patří Outlook.com, Office 365, opic průzkum, Twitter a požadavek HTTP [z webového formuláře](https://blogs.msdn.microsoft.com/logicapps/2017/01/30/calling-a-logic-app-from-an-html-form/).</span><span class="sxs-lookup"><span data-stu-id="55539-111">Some of the connectors that can assist in reacting to customer feedback include Outlook.com, Office 365, Survey Monkey, Twitter, and an HTTP Request [from a web form](https://blogs.msdn.microsoft.com/logicapps/2017/01/30/calling-a-logic-app-from-an-html-form/).</span></span>  <span data-ttu-id="55539-112">V pracovním postupu níže jsme monitorovat hashtag na Twitteru.</span><span class="sxs-lookup"><span data-stu-id="55539-112">For the workflow below, we will monitor a hashtag on Twitter.</span></span>

<span data-ttu-id="55539-113">Funkce zajistit bez serveru výpočetní v cloudu.</span><span class="sxs-lookup"><span data-stu-id="55539-113">Functions provide serverless compute in the cloud.</span></span>  <span data-ttu-id="55539-114">V tomto scénáři budeme používat Azure Functions na příznak tweetů od zákazníků založené na řadu předem definovaná klíčová slova.</span><span class="sxs-lookup"><span data-stu-id="55539-114">In this scenario, we will use Azure Functions to flag tweets from customers based on a series of pre-defined key words.</span></span>

<span data-ttu-id="55539-115">Může být celé řešení [sestavení v sadě Visual Studio](logic-apps-deploy-from-vs.md) a [nasazen jako součást šablony prostředků](logic-apps-create-deploy-template.md).</span><span class="sxs-lookup"><span data-stu-id="55539-115">The entire solution can be [build in Visual Studio](logic-apps-deploy-from-vs.md) and [deployed as part of a resource template](logic-apps-create-deploy-template.md).</span></span>  <span data-ttu-id="55539-116">Je také video s návodem scénáře [na webu Channel 9](http://aka.ms/logicappsdemo).</span><span class="sxs-lookup"><span data-stu-id="55539-116">There is also video walkthrough of the scenario [on Channel 9](http://aka.ms/logicappsdemo).</span></span>

## <a name="build-the-logic-app-to-trigger-on-customer-data"></a><span data-ttu-id="55539-117">Vytvoření aplikace logiky k aktivaci na data zákazníků</span><span class="sxs-lookup"><span data-stu-id="55539-117">Build the logic app to trigger on customer data</span></span>

<span data-ttu-id="55539-118">Po [vytvoření aplikace logiky](logic-apps-create-a-logic-app.md) v sadě Visual Studio nebo portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="55539-118">After [creating a logic app](logic-apps-create-a-logic-app.md) in Visual Studio or the Azure portal:</span></span>

1. <span data-ttu-id="55539-119">Přidat aktivační událost pro **na nové Tweetů** ze služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="55539-119">Add a trigger for **On New Tweets** from Twitter</span></span>
2. <span data-ttu-id="55539-120">Nakonfigurujte aktivační událost pro naslouchání na tweetů na – klíčové slovo nebo hashtag.</span><span class="sxs-lookup"><span data-stu-id="55539-120">Configure the trigger to listen to tweets on a keyword or hashtag.</span></span>

   > [!NOTE]
   > <span data-ttu-id="55539-121">Vlastnost opakování na aktivační událost určí, jak často se aplikace logiky kontrolovat přítomnost nových položek na základě cyklického dotazování aktivační události</span><span class="sxs-lookup"><span data-stu-id="55539-121">The recurrence property on the trigger will determine how frequently the logic app checks for new items on polling-based triggers</span></span>

   ![Příklad aktivační události služby Twitter.][1]

<span data-ttu-id="55539-123">Tato aplikace bude nyní platit na všechny nové tweetů.</span><span class="sxs-lookup"><span data-stu-id="55539-123">This app will now fire on all new tweets.</span></span>  <span data-ttu-id="55539-124">Pak můžeme provést tweet dat a lépe postojích vyjádřené porozumět.</span><span class="sxs-lookup"><span data-stu-id="55539-124">We can then take that tweet data and understand more of the sentiment expressed.</span></span>  <span data-ttu-id="55539-125">Pro tento používáme [kognitivní služby Azure](https://azure.microsoft.com/services/cognitive-services/) ke zjištění postojích textu.</span><span class="sxs-lookup"><span data-stu-id="55539-125">For this we use the [Azure Cognitive Service](https://azure.microsoft.com/services/cognitive-services/) to detect sentiment of text.</span></span>

1. <span data-ttu-id="55539-126">Klikněte na tlačítko **nový krok**</span><span class="sxs-lookup"><span data-stu-id="55539-126">Click **New Step**</span></span>
1. <span data-ttu-id="55539-127">Vyberte nebo vyhledejte **Analýza textu** konektoru</span><span class="sxs-lookup"><span data-stu-id="55539-127">Select or search for the **Text Analytics** connector</span></span>
1. <span data-ttu-id="55539-128">Vyberte **zjistit postojích** operace</span><span class="sxs-lookup"><span data-stu-id="55539-128">Select the **Detect Sentiment** operation</span></span>
1. <span data-ttu-id="55539-129">Pokud se zobrazí výzva, zadejte platný klíč kognitivní služby pro službu Analýza textu</span><span class="sxs-lookup"><span data-stu-id="55539-129">If prompted, provide a valid Cognitive Services key for the Text Analytics service</span></span>
1. <span data-ttu-id="55539-130">Přidat **Tweet Text** jako text k analýze.</span><span class="sxs-lookup"><span data-stu-id="55539-130">Add the **Tweet Text** as the text to analyze.</span></span>

<span data-ttu-id="55539-131">Teď, když máme tweet dat a statistika na tweet, může být několik dalších konektorů relevantní:</span><span class="sxs-lookup"><span data-stu-id="55539-131">Now that we have the tweet data, and insights on the tweet, a number of other connectors may be relevant:</span></span>
* <span data-ttu-id="55539-132">Power BI - přidávání řádků do datové sady streamování: zobrazení tweetů reálném čase na řídicí panel Power BI.</span><span class="sxs-lookup"><span data-stu-id="55539-132">Power BI - Add Rows to Streaming Dataset: View tweets real time on a Power BI dashboard.</span></span>
* <span data-ttu-id="55539-133">Azure Data Lake - připojit soubor: přidat data zákazníků k datové sadě služby Azure Data Lake pro zahrnutí do úlohy analytics.</span><span class="sxs-lookup"><span data-stu-id="55539-133">Azure Data Lake - Append file: Add customer data to an Azure Data Lake dataset to include in analytics jobs.</span></span>
* <span data-ttu-id="55539-134">SQL - přidávání řádků: ukládání dat v databázi pro pozdější načtení.</span><span class="sxs-lookup"><span data-stu-id="55539-134">SQL - Add rows: Store data in a database for later retrieval.</span></span>
* <span data-ttu-id="55539-135">Slack – poslat zprávu: výstrahy slack kanál na záporné zpětnou vazbu, která vyžaduje akce.</span><span class="sxs-lookup"><span data-stu-id="55539-135">Slack - Send message: Alert a slack channel on negative feedback that requires actions.</span></span>

<span data-ttu-id="55539-136">Funkce Azure lze také provést další vlastní výpočet nad data.</span><span class="sxs-lookup"><span data-stu-id="55539-136">An Azure Function can also be used to do more custom compute on top of the data.</span></span>

## <a name="enriching-the-data-with-an-azure-function"></a><span data-ttu-id="55539-137">Rozšíření dat pomocí funkce Azure</span><span class="sxs-lookup"><span data-stu-id="55539-137">Enriching the data with an Azure Function</span></span>

<span data-ttu-id="55539-138">Můžeme vytvořit funkci, potřebujeme mít aplikaci funkce v naše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="55539-138">Before we can create a function, we need to have a function app in our Azure subscription.</span></span>  <span data-ttu-id="55539-139">Informace o vytvoření funkce Azure na portálu můžete [se nachází tady](../azure-functions/functions-create-first-azure-function-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="55539-139">Details on creating an Azure Function in the portal can [be found here](../azure-functions/functions-create-first-azure-function-azure-portal.md)</span></span>

<span data-ttu-id="55539-140">Pro funkce k volání přímo z aplikace logiky, je nutné mít HTTP aktivovat vazby.</span><span class="sxs-lookup"><span data-stu-id="55539-140">For a function to be called directly from a logic app, it needs to have an HTTP trigger binding.</span></span>  <span data-ttu-id="55539-141">Doporučujeme používat **HttpTrigger** šablony.</span><span class="sxs-lookup"><span data-stu-id="55539-141">We recommend using the **HttpTrigger** template.</span></span>

<span data-ttu-id="55539-142">V tomto scénáři by textu žádosti funkce Azure tweet text.</span><span class="sxs-lookup"><span data-stu-id="55539-142">In this scenario, the request body of the Azure Function would be the tweet text.</span></span>  <span data-ttu-id="55539-143">V kódu funkce jednoduše definujte logiku Pokud tweet text obsahuje klíčové slovo nebo frázi.</span><span class="sxs-lookup"><span data-stu-id="55539-143">In the function code, simply define logic on if the tweet text contains a key word or phrase.</span></span>  <span data-ttu-id="55539-144">Samotná funkce může být udržovány jako jednoduché nebo komplexní, podle potřeby pro scénář.</span><span class="sxs-lookup"><span data-stu-id="55539-144">The function itself could be kept as simple or complex as needed for the scenario.</span></span>

<span data-ttu-id="55539-145">Na konci funkce jednoduše vrátí odpověď do aplikace logiky určitými daty.</span><span class="sxs-lookup"><span data-stu-id="55539-145">At the end of the function, simply return a response to the logic app with some data.</span></span>  <span data-ttu-id="55539-146">To může být jednoduchý logickou hodnotu (například `containsKeyword`), nebo komplexní objekt.</span><span class="sxs-lookup"><span data-stu-id="55539-146">This could be a simple boolean value (e.g. `containsKeyword`), or a complex object.</span></span>

![Nakonfigurované krok funkce Azure][2]

> [!TIP]
> <span data-ttu-id="55539-148">Při přístupu k komplexní odpověď z funkce v aplikaci logiky, použijte akci analyzovat JSON.</span><span class="sxs-lookup"><span data-stu-id="55539-148">When accessing a complex response from a function in a logic app, use the Parse JSON action.</span></span>

<span data-ttu-id="55539-149">Po uložení funkce mohou být přidány do aplikace logiky vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="55539-149">Once the function is saved, it can be added into the logic app created above.</span></span>  <span data-ttu-id="55539-150">V aplikaci logiky:</span><span class="sxs-lookup"><span data-stu-id="55539-150">In the logic app:</span></span>

1. <span data-ttu-id="55539-151">Klikněte na tlačítko Přidat **nový krok**</span><span class="sxs-lookup"><span data-stu-id="55539-151">Click to add a **New Step**</span></span>
1. <span data-ttu-id="55539-152">Vyberte **Azure Functions** konektoru</span><span class="sxs-lookup"><span data-stu-id="55539-152">Select the **Azure Functions** connector</span></span>
1. <span data-ttu-id="55539-153">Vybrat a vyberte existující funkce a přejděte do funkce vytvořena</span><span class="sxs-lookup"><span data-stu-id="55539-153">Select to choose an existing function, and browse to the function created</span></span>
1. <span data-ttu-id="55539-154">Poslat **Tweet Text** pro **text žádosti**</span><span class="sxs-lookup"><span data-stu-id="55539-154">Send in the **Tweet Text** for the **Request Body**</span></span>

## <a name="running-and-monitoring-the-solution"></a><span data-ttu-id="55539-155">Spuštění a sledování řešení</span><span class="sxs-lookup"><span data-stu-id="55539-155">Running and monitoring the solution</span></span>

<span data-ttu-id="55539-156">Jednou z výhod vytváření bez serveru orchestrations v Logic Apps je bohaté ladění a možnosti monitorování.</span><span class="sxs-lookup"><span data-stu-id="55539-156">One of the benefits of authoring serverless orchestrations in Logic Apps is the rich debug and monitoring capabilities.</span></span>  <span data-ttu-id="55539-157">Všechny spustit (aktuální nebo historické) lze zobrazit z v sadě Visual Studio, portálu Azure nebo prostřednictvím rozhraní API REST a sady SDK.</span><span class="sxs-lookup"><span data-stu-id="55539-157">Any run (current or historic) can be viewed from within Visual Studio, the Azure portal, or via the REST API and SDKs.</span></span>

<span data-ttu-id="55539-158">Jedním z nejjednodušších způsobů, jak aplikaci logiky si otestujte používá **spustit** tlačítko v návrháři.</span><span class="sxs-lookup"><span data-stu-id="55539-158">One of the easiest ways to test a logic app is using the **Run** button in the designer.</span></span>  <span data-ttu-id="55539-159">Kliknutím na tlačítko **spustit** bude pokračovat pro dotazování aktivační událost každých 5 sekund, dokud je zjištěna událost a pojmenujte za provozu zobrazení v průběhu běhu.</span><span class="sxs-lookup"><span data-stu-id="55539-159">Clicking **Run** will continue to poll the trigger every 5 seconds until an event is detected, and give a live view as the run progresses.</span></span>

<span data-ttu-id="55539-160">Předchozí spuštění historií lze zobrazit v okně Přehled v portálu Azure nebo pomocí Průzkumníka cloudové služby Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="55539-160">Previous run histories can be viewed on the Overview blade in the Azure portal, or using the Visual Studio Cloud Explorer.</span></span>

## <a name="creating-a-deployment-template-for-automated-deployments"></a><span data-ttu-id="55539-161">Vytvoření šablony nasazení pro automatické nasazení</span><span class="sxs-lookup"><span data-stu-id="55539-161">Creating a deployment template for automated deployments</span></span>

<span data-ttu-id="55539-162">Jakmile byla vyvinuta řešení, můžete zachytit a nasazují přes šablonu nasazení Azure všechny oblasti Azure na světě.</span><span class="sxs-lookup"><span data-stu-id="55539-162">Once a solution has been developed, it can be captured and deployed via an Azure deployment template to any Azure region in the world.</span></span>  <span data-ttu-id="55539-163">To je užitečné pro oba změny parametry pro různé verze tento pracovní postup, ale také pro integraci toto řešení v kanálu sestavení a verze.</span><span class="sxs-lookup"><span data-stu-id="55539-163">This is useful for both modifying parameters for different versions of this workflow, but also for integrating this solution in a build and release pipeline.</span></span>  <span data-ttu-id="55539-164">Informace o vytváření šablony nasazení lze nalézt [v tomto článku](logic-apps-create-deploy-template.md).</span><span class="sxs-lookup"><span data-stu-id="55539-164">Details on creating a deployment template can be found [in this article](logic-apps-create-deploy-template.md).</span></span>

<span data-ttu-id="55539-165">Azure Functions také být použity během nasazení šablony - tak celé řešení s všechny závislosti můžete spravovat jako jednu šablonu.</span><span class="sxs-lookup"><span data-stu-id="55539-165">Azure Functions can also be incorporated in the deployment template - so the entire solution with all dependencies can be managed as a single template.</span></span>  <span data-ttu-id="55539-166">Příklad funkce šablony nasazení lze nalézt v [úložiště šablony Azure rychlý Start](https://github.com/Azure/azure-quickstart-templates/tree/master/101-function-app-create-dynamic).</span><span class="sxs-lookup"><span data-stu-id="55539-166">An example of a function deployment template can be found in the [Azure quickstart template repository](https://github.com/Azure/azure-quickstart-templates/tree/master/101-function-app-create-dynamic).</span></span>

## <a name="next-steps"></a><span data-ttu-id="55539-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="55539-167">Next steps</span></span>

* [<span data-ttu-id="55539-168">Zobrazit další příklady a scénáře pro Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="55539-168">See other examples and scenarios for Azure Logic Apps</span></span>](logic-apps-examples-and-scenarios.md)
* [<span data-ttu-id="55539-169">Podívejte se na video s návodem k vytvoření tohoto řešení pro kompletní</span><span class="sxs-lookup"><span data-stu-id="55539-169">Watch a video walkthrough on creating this solution end-to-end</span></span>](http://aka.ms/logicappsdemo)
* [<span data-ttu-id="55539-170">Zjistěte, jak zpracovat a zachycení výjimek v rámci aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="55539-170">Learn how to handle and catch exceptions within a logic app</span></span>](logic-apps-exception-handling.md)

<!-- Image References -->
[1]: ./media/logic-apps-scenario-social-serverless/twitter.png
[2]: ./media/logic-apps-scenario-social-serverless/function.png