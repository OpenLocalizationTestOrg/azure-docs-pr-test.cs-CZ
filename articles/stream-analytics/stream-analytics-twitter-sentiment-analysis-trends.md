---
title: "Analýzy v reálném čase postojích Twitter pomocí služby Azure Stream Analytics | Microsoft Docs"
description: "Další informace o použití Stream Analytics k analýze postojích v reálném čase služby Twitter. Podrobné pokyny z generování událostí k datům na řídicím panelu za provozu."
keywords: "Analýza trendu v reálném čase twitter, postojích analýzy, analýzy sociálních médií, příklad analýza trendu"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 42068691-074b-4c3b-a527-acafa484fda2
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: jeffstok
ms.openlocfilehash: 8de6850964700f5b3f71d144b40af927f2e52d7e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="real-time-twitter-sentiment-analysis-in-azure-stream-analytics"></a><span data-ttu-id="04a2c-105">Analýzy v reálném čase postojích Twitter v Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="04a2c-105">Real-time Twitter sentiment analysis in Azure Stream Analytics</span></span>

<span data-ttu-id="04a2c-106">Naučte se vytvářet řešení analysis postojích pro analýzy sociálních médií tak, že převedou do centra událostí Azure v reálném čase události služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="04a2c-106">Learn how to build a sentiment analysis solution for social media analytics by bringing real-time Twitter events into Azure Event Hubs.</span></span> <span data-ttu-id="04a2c-107">Můžete pak použijte zápisu dotaz Azure Stream Analytics k analýze dat a buď ukládat výsledky pro později, nebo použijte řídicí panel a [Power BI](https://powerbi.com/) poskytnout statistiky v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="04a2c-107">You can then write an Azure Stream Analytics query to analyze the data and either store the results for later use or use a dashboard and [Power BI](https://powerbi.com/) to provide insights in real time.</span></span>

<span data-ttu-id="04a2c-108">Analýza nástrojů sociálních médiích pomůžou organizacím pochopit trendů témata.</span><span class="sxs-lookup"><span data-stu-id="04a2c-108">Social media analytics tools help organizations understand trending topics.</span></span> <span data-ttu-id="04a2c-109">Trendů témata jsou témata a postojích, které mají k velkému počtu příspěvcích v sociálních sítích.</span><span class="sxs-lookup"><span data-stu-id="04a2c-109">Trending topics are subjects and attitudes that have a high volume of posts in social media.</span></span> <span data-ttu-id="04a2c-110">Postojích analýzy, která se označuje taky jako *stanovisko dolování*, nástrojů sociálních médiích analytics používá k určení postoje směrem k produktu, je vhodné a tak dále.</span><span class="sxs-lookup"><span data-stu-id="04a2c-110">Sentiment analysis, which is also called *opinion mining*, uses social media analytics tools to determine attitudes toward a product, idea, and so on.</span></span> 

<span data-ttu-id="04a2c-111">Analýza trendu v reálném čase Twitter je skvělé příklad nástroj pro analýzu, protože modelu předplatného hashtag umožňuje naslouchání na konkrétní klíčová slova (hashtags) a vývoj postojích analysis informačního kanálu.</span><span class="sxs-lookup"><span data-stu-id="04a2c-111">Real-time Twitter trend analysis is a great example of an analytics tool, because the hashtag subscription model enables you to listen to specific keywords (hashtags) and develop sentiment analysis of the feed.</span></span>

## <a name="scenario-social-media-sentiment-analysis-in-real-time"></a><span data-ttu-id="04a2c-112">Scénář: Sociálních médií postojích analýzy v reálném čase</span><span class="sxs-lookup"><span data-stu-id="04a2c-112">Scenario: Social media sentiment analysis in real time</span></span>

<span data-ttu-id="04a2c-113">Společnost, která má web zprávy média má zájem o získání několik výhod před konkurencí podle s funkcí obsah webu, okamžitě vztahující se k jeho čtečky.</span><span class="sxs-lookup"><span data-stu-id="04a2c-113">A company that has a news media website is interested in gaining an advantage over its competitors by featuring site content that is immediately relevant to its readers.</span></span> <span data-ttu-id="04a2c-114">Společnost používá analýzy sociálních médiích na témata, která jsou relevantní pro čtečky díky v reálném čase postojích analýza dat Twitteru.</span><span class="sxs-lookup"><span data-stu-id="04a2c-114">The company uses social media analysis on topics that are relevant to readers by doing real-time sentiment analysis of Twitter data.</span></span>

<span data-ttu-id="04a2c-115">K identifikaci trendů témata v reálném čase v síti Twitter, musí společnost analýzu v reálném čase o tweet svazku a postojích klíče témata.</span><span class="sxs-lookup"><span data-stu-id="04a2c-115">To identify trending topics in real time on Twitter, the company needs real-time analytics about the tweet volume and sentiment for key topics.</span></span> <span data-ttu-id="04a2c-116">Jinými slovy potřeba je modul postojích analysis analytics je založena na této sociálních médií informačního kanálu.</span><span class="sxs-lookup"><span data-stu-id="04a2c-116">In other words, the need is a sentiment analysis analytics engine that's based on this social media feed.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="04a2c-117">Požadavky</span><span class="sxs-lookup"><span data-stu-id="04a2c-117">Prerequisites</span></span>
<span data-ttu-id="04a2c-118">V tomto kurzu použijete klientská aplikace, která se připojuje k Twitteru a hledá tweetů, které mají určité hashtags (kterou můžete nastavit).</span><span class="sxs-lookup"><span data-stu-id="04a2c-118">In this tutorial, you use a client application that connects to Twitter and looks for tweets that have certain hashtags (which you can set).</span></span> <span data-ttu-id="04a2c-119">Aby bylo možné spouštět aplikace a analyzovat tweetů pomocí Azure streamování Analytics, musíte mít následující:</span><span class="sxs-lookup"><span data-stu-id="04a2c-119">In order to run the application and analyze the tweets using Azure Streaming Analytics, you must have the following:</span></span>

* <span data-ttu-id="04a2c-120">Předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="04a2c-120">An Azure subscription</span></span>
* <span data-ttu-id="04a2c-121">Účet na Twitteru</span><span class="sxs-lookup"><span data-stu-id="04a2c-121">A Twitter account</span></span> 
* <span data-ttu-id="04a2c-122">Aplikace pomocí služby Twitter a [přístupového tokenu OAuth](https://dev.twitter.com/oauth/overview/application-owner-access-tokens) pro příslušnou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="04a2c-122">A Twitter application, and the [OAuth access token](https://dev.twitter.com/oauth/overview/application-owner-access-tokens) for that application.</span></span> <span data-ttu-id="04a2c-123">Poskytujeme vysoké úrovně pokyny, jak vytvořit aplikaci služby Twitter později.</span><span class="sxs-lookup"><span data-stu-id="04a2c-123">We provide high-level instructions for how to create a Twitter application later.</span></span>
* <span data-ttu-id="04a2c-124">Aplikace TwitterWPFClient, který čte informační kanál sítě Twitter.</span><span class="sxs-lookup"><span data-stu-id="04a2c-124">The TwitterWPFClient application, which reads the Twitter feed.</span></span> <span data-ttu-id="04a2c-125">Chcete-li získat tuto aplikaci, stáhněte [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) z Githubu a potom rozbalte balíček do složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="04a2c-125">To get this application, download the [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) file from GitHub and then unzip the package into a folder on your computer.</span></span> <span data-ttu-id="04a2c-126">Pokud chcete zobrazit zdrojový kód a spusťte aplikaci v ladicí program, můžete získat zdrojového kódu z [Githubu](https://aka.ms/azure-stream-analytics-telcogenerator).</span><span class="sxs-lookup"><span data-stu-id="04a2c-126">If you want to see the source code and run the application in a debugger, you can get the source code from [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator).</span></span> 

## <a name="create-an-event-hub-for-streaming-analytics-input"></a><span data-ttu-id="04a2c-127">Vytvoření centra událostí pro vstup analýzy datových proudů</span><span class="sxs-lookup"><span data-stu-id="04a2c-127">Create an event hub for Streaming Analytics input</span></span>

<span data-ttu-id="04a2c-128">Ukázkové aplikace vygeneruje události a jejich nabízených oznámení do centra událostí Azure.</span><span class="sxs-lookup"><span data-stu-id="04a2c-128">The sample application generates events and pushes them to an Azure event hub.</span></span> <span data-ttu-id="04a2c-129">Azure event hubs je upřednostňovaný způsob přijímání událostí pro Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="04a2c-129">Azure event hubs are the preferred method of event ingestion for Stream Analytics.</span></span> <span data-ttu-id="04a2c-130">Další informace najdete v tématu [dokumentace Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="04a2c-130">For more information, see the [Azure Event Hubs documentation](../event-hubs/event-hubs-what-is-event-hubs.md).</span></span>


### <a name="create-an-event-hub-namespace-and-event-hub"></a><span data-ttu-id="04a2c-131">Vytvoření centra událostí obor názvů a centra událostí</span><span class="sxs-lookup"><span data-stu-id="04a2c-131">Create an event hub namespace and event hub</span></span>
<span data-ttu-id="04a2c-132">V tomto postupu vytvoříte na obor názvů centra událostí a poté přidejte centra událostí do daného oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="04a2c-132">In this procedure, you first create an event hub namespace, and then you add an event hub to that namespace.</span></span> <span data-ttu-id="04a2c-133">Obory názvů centra událostí se používají k logickému seskupení souvisejících událostí sběrnice instancí.</span><span class="sxs-lookup"><span data-stu-id="04a2c-133">Event hub namespaces are used to logically group related event bus instances.</span></span> 

1. <span data-ttu-id="04a2c-134">Přihlaste se k portálu Azure a klikněte na tlačítko **nový** > **Internet věcí** > **centra událostí**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-134">Log  in to the Azure portal and click **New** > **Internet of Things** > **Event Hub**.</span></span> 

2. <span data-ttu-id="04a2c-135">V **vytvoření oboru názvů** okno, zadejte název oboru názvů, jako `<yourname>-socialtwitter-eh-ns`.</span><span class="sxs-lookup"><span data-stu-id="04a2c-135">In the **Create namespace** blade, enter a namespace name such as `<yourname>-socialtwitter-eh-ns`.</span></span> <span data-ttu-id="04a2c-136">Můžete použít libovolný název pro obor názvů, ale název musí být platné adresy URL a musí být jedinečný v Azure.</span><span class="sxs-lookup"><span data-stu-id="04a2c-136">You can use any name for the namespace, but the name must be valid for a URL and it must be unique across Azure.</span></span> 
    
3. <span data-ttu-id="04a2c-137">Vyberte předplatné a vytvořit nebo vybrat skupinu prostředků a potom klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-137">Select a subscription and create or choose a resource group, then click **Create**.</span></span> 

    ![Vytvoření oboru názvů události rozbočovače](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub-namespace.png)
 
4. <span data-ttu-id="04a2c-139">Po dokončení nasazení obor názvů v seznamu prostředků Azure najdete názvový prostor události rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="04a2c-139">When the namespace has finished deploying, find the event hub namespace in your list of Azure resources.</span></span> 

5. <span data-ttu-id="04a2c-140">Klikněte na tlačítko Nový obor názvů a v okně obor názvů, klikněte na tlačítko  **+ &nbsp;centra událostí**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-140">Click the new namespace, and in the namespace blade, click **+&nbsp;Event Hub**.</span></span> 

    ![<span data-ttu-id="04a2c-141">Tlačítko Přidat centra událostí pro vytvoření nového centra událostí</span><span class="sxs-lookup"><span data-stu-id="04a2c-141">The Add Event Hub button for creating a new event hub</span></span> ](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub-button.png)    
 
6. <span data-ttu-id="04a2c-142">Název nového centra událostí `socialtwitter-eh`.</span><span class="sxs-lookup"><span data-stu-id="04a2c-142">Name the new event hub `socialtwitter-eh`.</span></span> <span data-ttu-id="04a2c-143">Můžete použít jiný název.</span><span class="sxs-lookup"><span data-stu-id="04a2c-143">You can use a different name.</span></span> <span data-ttu-id="04a2c-144">Pokud tak učiníte, poznamenejte si, protože potřebujete název později.</span><span class="sxs-lookup"><span data-stu-id="04a2c-144">If you do, make a note of it, because you need the name later.</span></span> <span data-ttu-id="04a2c-145">Nemusíte nastavte další možnosti pro centra událostí.</span><span class="sxs-lookup"><span data-stu-id="04a2c-145">You don't need to set any other options for the event hub.</span></span>

    ![Okno pro vytvoření nového centra událostí](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub.png)
 
7. <span data-ttu-id="04a2c-147">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-147">Click **Create**.</span></span>


### <a name="grant-access-to-the-event-hub"></a><span data-ttu-id="04a2c-148">Udělení přístupu do centra událostí</span><span class="sxs-lookup"><span data-stu-id="04a2c-148">Grant access to the event hub</span></span>

<span data-ttu-id="04a2c-149">Předtím, než se proces může odesílat data do centra událostí, musí mít centra událostí zásadu, která umožňuje odpovídající přístup.</span><span class="sxs-lookup"><span data-stu-id="04a2c-149">Before a process can send data to an event hub, the event hub must have a policy that allows appropriate access.</span></span> <span data-ttu-id="04a2c-150">Zásady přístupu vytvoří připojovací řetězec, který obsahuje informace o ověření.</span><span class="sxs-lookup"><span data-stu-id="04a2c-150">The access policy produces a connection string that includes authorization information.</span></span>

1.  <span data-ttu-id="04a2c-151">V okně oboru názvů událostí, klikněte na tlačítko **Event Hubs** a pak klikněte na název vašeho nového centra událostí.</span><span class="sxs-lookup"><span data-stu-id="04a2c-151">In the event namespace blade, click **Event Hubs** and then click the name of your new event hub.</span></span>

2.  <span data-ttu-id="04a2c-152">V okně centra událostí, klikněte na **zásady sdíleného přístupu** a pak klikněte na  **+ &nbsp;přidat**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-152">In the event hub blade, click **Shared access policies** and then click **+&nbsp;Add**.</span></span>

    >[!NOTE]
    ><span data-ttu-id="04a2c-153">Ujistěte se, že pracujete s centrem událostí, není oboru názvů centra událostí.</span><span class="sxs-lookup"><span data-stu-id="04a2c-153">Make sure you're working with the event hub, not the event hub namespace.</span></span>

3.  <span data-ttu-id="04a2c-154">Přidat zásadu s názvem `socialtwitter-access` a **deklarace identity**, vyberte **spravovat**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-154">Add a policy named `socialtwitter-access` and for **Claim**, select **Manage**.</span></span>

    ![Okno pro vytvoření nové zásady přístupu centra událostí](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-shared-access-policy-manage.png)
 
4.  <span data-ttu-id="04a2c-156">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-156">Click **Create**.</span></span>

5.  <span data-ttu-id="04a2c-157">Po nasazení zásady, klikněte na něj v seznamu zásady sdíleného přístupu.</span><span class="sxs-lookup"><span data-stu-id="04a2c-157">After the policy has been deployed, click it in the list of shared access policies.</span></span>

6.  <span data-ttu-id="04a2c-158">Vyhledejte seznam **PŘIPOJOVACÍ řetězec primární klíč** a klikněte na tlačítko Kopírovat vedle připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="04a2c-158">Find the box labeled **CONNECTION STRING-PRIMARY KEY** and click the copy button next to the connection string.</span></span> 
    
    ![Kopírování klíče primární připojovací řetězec ze zásad přístupu](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-shared-access-policy-copy-connection-string.png)
 
7.  <span data-ttu-id="04a2c-160">Vložte připojovací řetězec do textového editoru.</span><span class="sxs-lookup"><span data-stu-id="04a2c-160">Paste the connection string into a text editor.</span></span> <span data-ttu-id="04a2c-161">Tento připojovací řetězec pro další části, musíte po provedení některé malé úpravy.</span><span class="sxs-lookup"><span data-stu-id="04a2c-161">You need this connection string for the next section, after you make some small edits to it.</span></span>

    <span data-ttu-id="04a2c-162">Připojovací řetězec vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="04a2c-162">The connection string looks like this:</span></span>

        Endpoint=sb://YOURNAME-socialtwitter-eh-ns.servicebus.windows.net/;SharedAccessKeyName=socialtwitter-access;SharedAccessKey=Gw2NFZw6r...FxKbXaC2op6a0ZsPkI=;EntityPath=socialtwitter-eh

    <span data-ttu-id="04a2c-163">Všimněte si, že připojovací řetězec obsahuje více párů klíč hodnota oddělené středníky: `Endpoint`, `SharedAccessKeyName`, `SharedAccessKey`, a `EntityPath`.</span><span class="sxs-lookup"><span data-stu-id="04a2c-163">Notice that the connection string contains multiple key-value pairs, separated with semicolons: `Endpoint`, `SharedAccessKeyName`, `SharedAccessKey`, and `EntityPath`.</span></span>  

    > [!NOTE]
    > <span data-ttu-id="04a2c-164">Pro zabezpečení se odebraly částí připojovacího řetězce v příkladu.</span><span class="sxs-lookup"><span data-stu-id="04a2c-164">For security, parts of the connection string in the example have been removed.</span></span>

8.  <span data-ttu-id="04a2c-165">V textovém editoru, odeberte `EntityPath` pár z připojovacího řetězce (Nezapomeňte odebrat středník předcházejícího).</span><span class="sxs-lookup"><span data-stu-id="04a2c-165">In the text editor, remove the `EntityPath` pair from the connection string (don't forget to remove the semicolon that precedes it).</span></span> <span data-ttu-id="04a2c-166">Když jste hotovi, připojovací řetězec vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="04a2c-166">When you're done, the connection string looks like this:</span></span>

        Endpoint=sb://YOURNAME-socialtwitter-eh-ns.servicebus.windows.net/;SharedAccessKeyName=socialtwitter-access;SharedAccessKey=Gw2NFZw6r...FxKbXaC2op6a0ZsPkI=


## <a name="configure-and-start-the-twitter-client-application"></a><span data-ttu-id="04a2c-167">Nakonfigurovat a spustit klientskou aplikaci služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="04a2c-167">Configure and start the Twitter client application</span></span>
<span data-ttu-id="04a2c-168">Klientská aplikace získá tweet události přímo ze služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="04a2c-168">The client application gets tweet events directly from Twitter.</span></span> <span data-ttu-id="04a2c-169">Chcete-li tak učinit, potřebuje oprávnění k volání API služby Twitter streamování.</span><span class="sxs-lookup"><span data-stu-id="04a2c-169">In order to do so, it needs permission to call the Twitter Streaming APIs.</span></span> <span data-ttu-id="04a2c-170">Pokud chcete nakonfigurovat tato oprávnění, vytvoříte aplikaci v Twitter, který generuje jedinečné přihlašovací údaje (například tokenu OAuth).</span><span class="sxs-lookup"><span data-stu-id="04a2c-170">To configure that permission, you create an application in Twitter, which generates unique credentials (such as an OAuth token).</span></span> <span data-ttu-id="04a2c-171">Potom můžete nakonfigurovat klientskou aplikaci použít tyto přihlašovací údaje při volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="04a2c-171">You can then configure the client application to use these credentials when it makes API calls.</span></span> 

### <a name="create-a-twitter-application"></a><span data-ttu-id="04a2c-172">Vytvořit aplikaci služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="04a2c-172">Create a Twitter application</span></span>
<span data-ttu-id="04a2c-173">Pokud již nemáte Twitter aplikace, která můžete použít pro tento kurz, můžete vytvořit jeden.</span><span class="sxs-lookup"><span data-stu-id="04a2c-173">If you do not already have a Twitter application that you can use for this tutorial, you can create one.</span></span> <span data-ttu-id="04a2c-174">Již musí mít účet služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="04a2c-174">You must already have a Twitter account.</span></span>

> [!NOTE]
> <span data-ttu-id="04a2c-175">Přesný postup v Twitter pro vytváření aplikací a získávání klíčů, tajných klíčů a token může změnit.</span><span class="sxs-lookup"><span data-stu-id="04a2c-175">The exact process in Twitter for creating an application and getting the keys, secrets, and token might change.</span></span> <span data-ttu-id="04a2c-176">Pokud tyto pokyny neodpovídají, najdete na webu služby Twitter, naleznete v dokumentaci pro vývojáře služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="04a2c-176">If these instructions don't match what you see on the Twitter site, refer to the Twitter developer documentation.</span></span>

1. <span data-ttu-id="04a2c-177">Přejděte na [stránku Správa aplikací Twitter](https://apps.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="04a2c-177">Go to the [Twitter application management page](https://apps.twitter.com/).</span></span> 

2. <span data-ttu-id="04a2c-178">Vytvoření nové aplikace.</span><span class="sxs-lookup"><span data-stu-id="04a2c-178">Create a new application.</span></span> 

    * <span data-ttu-id="04a2c-179">Pro adresu URL webu zadejte platnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="04a2c-179">For the website URL, specify a valid URL.</span></span> <span data-ttu-id="04a2c-180">Nemá být živý web.</span><span class="sxs-lookup"><span data-stu-id="04a2c-180">It does not have to be a live site.</span></span> <span data-ttu-id="04a2c-181">(Není možné určit pouze `localhost`.)</span><span class="sxs-lookup"><span data-stu-id="04a2c-181">(You can't specify just `localhost`.)</span></span>
    * <span data-ttu-id="04a2c-182">Pole zpětného volání zůstat prázdné.</span><span class="sxs-lookup"><span data-stu-id="04a2c-182">Leave the callback field blank.</span></span> <span data-ttu-id="04a2c-183">Klientská aplikace, kterou použijete pro tento kurz nevyžaduje zpětných volání.</span><span class="sxs-lookup"><span data-stu-id="04a2c-183">The client application you use for this tutorial doesn't require callbacks.</span></span>

    ![Vytváření aplikací v Twitter](./media/stream-analytics-twitter-sentiment-analysis-trends/create-twitter-application.png)

3. <span data-ttu-id="04a2c-185">Volitelně můžete změňte aplikaci oprávnění jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="04a2c-185">Optionally, change the application's permissions to read-only.</span></span>

4. <span data-ttu-id="04a2c-186">Při vytvoření aplikace, přejděte na **klíče a přístupové tokeny** stránky.</span><span class="sxs-lookup"><span data-stu-id="04a2c-186">When the application is created, go to the **Keys and Access Tokens** page.</span></span>

5. <span data-ttu-id="04a2c-187">Klikněte na tlačítko Generovat token a přístup tajný klíč přístupového tokenu.</span><span class="sxs-lookup"><span data-stu-id="04a2c-187">Click the button to generate an access token and access token secret.</span></span>

<span data-ttu-id="04a2c-188">Ponechat tyto informace užitečné, protože je budete potřebovat v dalším postupu.</span><span class="sxs-lookup"><span data-stu-id="04a2c-188">Keep this information handy, because you will need it in the next procedure.</span></span>

>[!NOTE]
><span data-ttu-id="04a2c-189">Klíče a tajné klíče pro aplikaci služby Twitter poskytují přístup k vašemu účtu služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="04a2c-189">The keys and secrets for the Twitter application provide access to your Twitter account.</span></span> <span data-ttu-id="04a2c-190">S takovými informacemi nakládat jako velká a malá písmena, stejné jako heslo služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="04a2c-190">Treat this information as sensitive, the same as you do your Twitter password.</span></span> <span data-ttu-id="04a2c-191">Například Nevnořujte tyto informace v aplikaci, která poskytnout ostatním uživatelům.</span><span class="sxs-lookup"><span data-stu-id="04a2c-191">For example, don't embed this information in an application that you give to others.</span></span> 


### <a name="configure-the-client-application"></a><span data-ttu-id="04a2c-192">Nakonfigurovat klientskou aplikaci</span><span class="sxs-lookup"><span data-stu-id="04a2c-192">Configure the client application</span></span>
<span data-ttu-id="04a2c-193">Vytvořili jsme klientskou aplikaci, která se připojuje k dat pomocí služby Twitter [rozhraní API pro Streaming na Twitteru](https://dev.twitter.com/streaming/overview) shromažďování událostí tweet o konkrétní sadu témat.</span><span class="sxs-lookup"><span data-stu-id="04a2c-193">We've created a client application that connects to Twitter data using [Twitter's Streaming APIs](https://dev.twitter.com/streaming/overview) to collect tweet events about a specific set of topics.</span></span> <span data-ttu-id="04a2c-194">Aplikace používá [Sentiment140](http://help.sentiment140.com/) nástroj s otevřeným zdrojem, který se přiřadí každé tweet následující hodnotu postojích:</span><span class="sxs-lookup"><span data-stu-id="04a2c-194">The application uses the [Sentiment140](http://help.sentiment140.com/) open source tool, which assigns the following sentiment value to each tweet:</span></span>

* <span data-ttu-id="04a2c-195">0 = záporná</span><span class="sxs-lookup"><span data-stu-id="04a2c-195">0 = negative</span></span>
* <span data-ttu-id="04a2c-196">2 = neutral</span><span class="sxs-lookup"><span data-stu-id="04a2c-196">2 = neutral</span></span>
* <span data-ttu-id="04a2c-197">4 = kladné</span><span class="sxs-lookup"><span data-stu-id="04a2c-197">4 = positive</span></span>

<span data-ttu-id="04a2c-198">Po události tweet byly přiřazeny postojích hodnotu, se instaluje do centra událostí, který jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="04a2c-198">After the tweet events have been assigned a sentiment value, they are pushed to the event hub that you created earlier.</span></span>

<span data-ttu-id="04a2c-199">Předtím, než je aplikace spuštěná, vyžaduje určité informace z vašeho počítače, jako jsou služby Twitter klíče a připojovací řetězec centra událostí.</span><span class="sxs-lookup"><span data-stu-id="04a2c-199">Before the application runs, it requires certain information from you, like the Twitter keys and the event hub connection string.</span></span> <span data-ttu-id="04a2c-200">Můžete zadat informace o konfiguraci těmito způsoby:</span><span class="sxs-lookup"><span data-stu-id="04a2c-200">You can provide the configuration information in these ways:</span></span>

* <span data-ttu-id="04a2c-201">Spusťte aplikaci a pomocí uživatelského rozhraní aplikace zadejte klíčů, tajné klíče a připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="04a2c-201">Run the application, and then use the application's UI to enter the keys, secrets, and connection string.</span></span> <span data-ttu-id="04a2c-202">Pokud to uděláte, informace o konfiguraci se používá pro aktuální relaci, ale nebyla uložena.</span><span class="sxs-lookup"><span data-stu-id="04a2c-202">If you do this, the configuration information is used for your current session, but it isn't saved.</span></span>
* <span data-ttu-id="04a2c-203">Upravte soubor .config aplikace a nastavte hodnoty existuje.</span><span class="sxs-lookup"><span data-stu-id="04a2c-203">Edit the application's .config file and set the values there.</span></span> <span data-ttu-id="04a2c-204">Tento přístup udržuje informace o konfiguraci, ale je také znamená, že tento potenciálně citlivé informace je uložena ve formátu prostého textu ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="04a2c-204">This approach persists the configuration information, but it also means that this potentially sensitive information is stored in plain text on your computer.</span></span>

<span data-ttu-id="04a2c-205">Následující postup dokumenty obou přístupů.</span><span class="sxs-lookup"><span data-stu-id="04a2c-205">The following procedure documents both approaches.</span></span> 

1. <span data-ttu-id="04a2c-206">Zkontrolujte, že jste stažené a rozbalené [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) aplikaci, jak je uvedené v předpokladech.</span><span class="sxs-lookup"><span data-stu-id="04a2c-206">Make sure you've downloaded and unzipped the [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) application, as listed in the prerequisites.</span></span>

2. <span data-ttu-id="04a2c-207">Chcete-li nastavit hodnoty v době běhu (a pouze pro aktuální relaci) spustit `TwitterWPFClient.exe` aplikace.</span><span class="sxs-lookup"><span data-stu-id="04a2c-207">To set the values at run time (and only for the current session), run the `TwitterWPFClient.exe` application.</span></span> <span data-ttu-id="04a2c-208">Pokud vás aplikace vyzve, zadejte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="04a2c-208">When the application prompts you, enter the following values:</span></span>

    * <span data-ttu-id="04a2c-209">Twitter uživatelský klíč (klíč rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="04a2c-209">The Twitter Consumer Key (API Key).</span></span>
    * <span data-ttu-id="04a2c-210">Twitter uživatelský tajný klíč (tajný klíč rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="04a2c-210">The Twitter Consumer Secret (API Secret).</span></span>
    * <span data-ttu-id="04a2c-211">Přístupový Token služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="04a2c-211">The Twitter Access Token.</span></span>
    * <span data-ttu-id="04a2c-212">Na Twitteru tajný klíč přístupového tokenu.</span><span class="sxs-lookup"><span data-stu-id="04a2c-212">The Twitter Access Token Secret.</span></span>
    * <span data-ttu-id="04a2c-213">Připojovací řetězec informace, které jste předtím uložili.</span><span class="sxs-lookup"><span data-stu-id="04a2c-213">The connection string information that you saved earlier.</span></span> <span data-ttu-id="04a2c-214">Ujistěte se, že používáte připojovací řetězec, který jste odebrali `EntityPath` dvojice klíč hodnota z.</span><span class="sxs-lookup"><span data-stu-id="04a2c-214">Make sure that you use the connection string that you removed the `EntityPath` key-value pair from.</span></span>
    * <span data-ttu-id="04a2c-215">Twitter klíčová slova, která chcete určit postojích pro.</span><span class="sxs-lookup"><span data-stu-id="04a2c-215">The Twitter keywords that you want to determine sentiment for.</span></span>

   ![TwitterWpfClient aplikace spuštěna, zobrazující zakryt nastavení](./media/stream-analytics-twitter-sentiment-analysis-trends/wpfclientlines.png)

3. <span data-ttu-id="04a2c-217">Pokud chcete trvale nastavit hodnoty, pomocí textového editoru otevřete soubor TwitterWpfClient.exe.config.</span><span class="sxs-lookup"><span data-stu-id="04a2c-217">To set the values persistently, use a text editor to open the TwitterWpfClient.exe.config file.</span></span> <span data-ttu-id="04a2c-218">Potom v `<appSettings>` elementu, to udělat:</span><span class="sxs-lookup"><span data-stu-id="04a2c-218">Then in the `<appSettings>` element, do this:</span></span>

    * <span data-ttu-id="04a2c-219">Nastavit `oauth_consumer_key` na uživatelský klíč pro Twitter (klíč rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="04a2c-219">Set `oauth_consumer_key` to the Twitter Consumer Key (API Key).</span></span> 
    * <span data-ttu-id="04a2c-220">Nastavit `oauth_consumer_secret` na Twitteru uživatelský tajný klíč (tajný klíč rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="04a2c-220">Set `oauth_consumer_secret` to the Twitter Consumer Secret (API Secret).</span></span>
    * <span data-ttu-id="04a2c-221">Nastavit `oauth_token` na přístupový Token služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="04a2c-221">Set `oauth_token` to the Twitter Access Token.</span></span>
    * <span data-ttu-id="04a2c-222">Nastavit `oauth_token_secret` na Twitteru tajný klíč tokenu přístupu.</span><span class="sxs-lookup"><span data-stu-id="04a2c-222">Set `oauth_token_secret` to the Twitter Access Token Secret.</span></span>

    <span data-ttu-id="04a2c-223">Dále v `<appSettings>` elementu, tyto změny:</span><span class="sxs-lookup"><span data-stu-id="04a2c-223">Later in the `<appSettings>` element, make these changes:</span></span>

    * <span data-ttu-id="04a2c-224">Nastavit `EventHubName` na název centra událostí (to znamená na hodnotu cesty entity).</span><span class="sxs-lookup"><span data-stu-id="04a2c-224">Set `EventHubName` to the event hub name (that is, to the value of the entity path).</span></span>
    * <span data-ttu-id="04a2c-225">Nastavit `EventHubNameConnectionString` připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="04a2c-225">Set `EventHubNameConnectionString` to the connection string.</span></span> <span data-ttu-id="04a2c-226">Ujistěte se, že používáte připojovací řetězec, který jste odebrali `EntityPath` dvojice klíč hodnota z.</span><span class="sxs-lookup"><span data-stu-id="04a2c-226">Make sure that you use the connection string that you removed the `EntityPath` key-value pair from.</span></span>

    <span data-ttu-id="04a2c-227">`<appSettings>` Části vypadá jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="04a2c-227">The `<appSettings>` section looks like the following example.</span></span> <span data-ttu-id="04a2c-228">(Pro přehlednost a zabezpečení, jsme zabalená některé řádky a odebrat některé znaky.)</span><span class="sxs-lookup"><span data-stu-id="04a2c-228">(For clarity and security, we wrapped some lines and removed some characters.)</span></span>

    ![Konfigurační soubor aplikace TwitterWpfClient v textovém editoru, zobrazující Twitter klíče a tajné klíče a informace o události rozbočovače připojovacím řetězci](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-tiwtter-app-config.png)
 
4. <span data-ttu-id="04a2c-230">Pokud již jste nezahájili aplikace, spusťte nyní TwitterWpfClient.exe.</span><span class="sxs-lookup"><span data-stu-id="04a2c-230">If you didn't already start the application, run TwitterWpfClient.exe now.</span></span> 

5. <span data-ttu-id="04a2c-231">Klepněte na tlačítko start zelená ke shromažďování sociálních postojích.</span><span class="sxs-lookup"><span data-stu-id="04a2c-231">Click the green start button to collect social sentiment.</span></span> <span data-ttu-id="04a2c-232">Tweet události se zobrazí **CreatedAt**, **tématu**, a **SentimentScore** hodnoty, které jsou odesílány do vašeho centra událostí.</span><span class="sxs-lookup"><span data-stu-id="04a2c-232">You see Tweet events with the **CreatedAt**, **Topic**, and **SentimentScore** values being sent to your event hub.</span></span>

    ![Spuštění aplikace TwitterWpfClient zobrazující seznam tweetů](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-app-listing.png)

    >[!NOTE]
    ><span data-ttu-id="04a2c-234">Pokud se zobrazí chyby a nevidíte proud tweetů zobrazí v dolní části okna, zkontrolujte klíče a tajné klíče.</span><span class="sxs-lookup"><span data-stu-id="04a2c-234">If you see errors, and you don't see a stream of tweets displayed in the lower part of the window, double-check the keys and secrets.</span></span> <span data-ttu-id="04a2c-235">Také zkontrolujte připojovací řetězec (ujistěte se, že nebude obsahovat `EntityPath` klíče a hodnoty.)</span><span class="sxs-lookup"><span data-stu-id="04a2c-235">Also check the connection string (make sure that it does not include the `EntityPath` key and value.)</span></span>


## <a name="create-a-stream-analytics-job"></a><span data-ttu-id="04a2c-236">Vytvoření úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="04a2c-236">Create a Stream Analytics job</span></span>

<span data-ttu-id="04a2c-237">Teď, když jsou události tweet streamování v reálném čase z Twitteru, můžete nastavit úlohu služby Stream Analytics k analýze těchto událostí v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="04a2c-237">Now that tweet events are streaming in real time from Twitter, you can set up a Stream Analytics job to analyze these events in real time.</span></span>

1. <span data-ttu-id="04a2c-238">Na portálu Azure klikněte na tlačítko **nový** > **Internet věcí** > **úlohy služby Stream Analytics**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-238">In the Azure portal, click **New** > **Internet of Things** > **Stream Analytics job**.</span></span>

2. <span data-ttu-id="04a2c-239">Název úlohy `socialtwitter-sa-job` a určete předplatné, skupinu prostředků a umístění.</span><span class="sxs-lookup"><span data-stu-id="04a2c-239">Name the job `socialtwitter-sa-job` and specify a subscription, resource group, and location.</span></span>

    <span data-ttu-id="04a2c-240">Je vhodné umístit úlohy a centra událostí ve stejné oblasti pro nejlepší výkon, a tak, že nemáte platíte k přenosu dat mezi oblastmi.</span><span class="sxs-lookup"><span data-stu-id="04a2c-240">It's a good idea to place the job and the event hub in the same region for best performance and so that you don't pay to transfer data between regions.</span></span>

    ![Vytvoření nové úlohy Stream Analytics](./media/stream-analytics-twitter-sentiment-analysis-trends/newjob.png)

3. <span data-ttu-id="04a2c-242">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-242">Click **Create**.</span></span>

    <span data-ttu-id="04a2c-243">Vytvoření úlohy a na portálu se zobrazí podrobnosti úlohy.</span><span class="sxs-lookup"><span data-stu-id="04a2c-243">The job is created and the portal displays job details.</span></span>


## <a name="specify-the-job-input"></a><span data-ttu-id="04a2c-244">Zadejte vstup úlohy</span><span class="sxs-lookup"><span data-stu-id="04a2c-244">Specify the job input</span></span>

1. <span data-ttu-id="04a2c-245">V úloze Stream Analytics v části **úlohy topologie** uprostřed okno úlohy, klikněte na tlačítko **vstupy**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-245">In your Stream Analytics job, under **Job Topology** in the middle of the job blade, click **Inputs**.</span></span> 

2. <span data-ttu-id="04a2c-246">V **vstupy** okně klikněte na tlačítko  **+ &nbsp;přidat** a potom vyplňte v okně s těmito hodnotami:</span><span class="sxs-lookup"><span data-stu-id="04a2c-246">In the **Inputs** blade, click **+&nbsp;Add** and then fill out the blade with these values:</span></span>

    * <span data-ttu-id="04a2c-247">**Vstupní alias**: použijte název `TwitterStream`.</span><span class="sxs-lookup"><span data-stu-id="04a2c-247">**Input alias**: Use the name `TwitterStream`.</span></span> <span data-ttu-id="04a2c-248">Pokud použijete jiný název, poznamenejte si jeho protože ji budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="04a2c-248">If you use a different name, make a note of it because you need it later.</span></span>
    * <span data-ttu-id="04a2c-249">**Typ zdroje**: vyberte **datový proud**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-249">**Source type**: Select **Data stream**.</span></span>
    * <span data-ttu-id="04a2c-250">**Zdroj**: vyberte **centra událostí**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-250">**Source**: Select **Event hub**.</span></span>
    * <span data-ttu-id="04a2c-251">**Import možnost**: vyberte **použijte Centrum událostí z aktuálního předplatného**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-251">**Import option**: Select **Use event hub from current subscription**.</span></span> 
    * <span data-ttu-id="04a2c-252">**Obor názvů sběrnice**: Vyberte obor názvů centra událostí, který jste vytvořili dříve (`<yourname>-socialtwitter-eh-ns`).</span><span class="sxs-lookup"><span data-stu-id="04a2c-252">**Service bus namespace**: Select the event hub namespace that you created earlier (`<yourname>-socialtwitter-eh-ns`).</span></span>
    * <span data-ttu-id="04a2c-253">**Centra událostí**: Vyberte centra událostí, které jste vytvořili dříve (`socialtwitter-eh`).</span><span class="sxs-lookup"><span data-stu-id="04a2c-253">**Event hub**: Select the event hub that you created earlier (`socialtwitter-eh`).</span></span>
    * <span data-ttu-id="04a2c-254">**Název zásady centra událostí**: vyberte zásady přístupu, které jste vytvořili dříve (`socialtwitter-access`).</span><span class="sxs-lookup"><span data-stu-id="04a2c-254">**Event hub policy name**: Select the access policy that you created earlier (`socialtwitter-access`).</span></span>

    ![Vytvořit nový vstupní úlohy streamování Analytics](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-new-input.png)

3. <span data-ttu-id="04a2c-256">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-256">Click **Create**.</span></span>


## <a name="specify-the-job-query"></a><span data-ttu-id="04a2c-257">Zadejte dotaz úlohy</span><span class="sxs-lookup"><span data-stu-id="04a2c-257">Specify the job query</span></span>

<span data-ttu-id="04a2c-258">Stream Analytics podporuje jednoduchý a deklarativní dotazu model, který popisuje transformace.</span><span class="sxs-lookup"><span data-stu-id="04a2c-258">Stream Analytics supports a simple, declarative query model that describes transformations.</span></span> <span data-ttu-id="04a2c-259">Další informace o jazyk, najdete v článku [referenční příručka Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx).</span><span class="sxs-lookup"><span data-stu-id="04a2c-259">To learn more about the language, see the [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx).</span></span>  <span data-ttu-id="04a2c-260">Tento kurz vám pomůže vytvořit a vyzkoušet několik dotazů přes Twitter data.</span><span class="sxs-lookup"><span data-stu-id="04a2c-260">This tutorial helps you author and test several queries over Twitter data.</span></span>

<span data-ttu-id="04a2c-261">Chcete-li porovnejte počet zmínkami mezi témata, můžete použít [Přeskakující okno](https://msdn.microsoft.com/library/azure/dn835055.aspx) získat počet zmínkami podle tématu každých pět sekund.</span><span class="sxs-lookup"><span data-stu-id="04a2c-261">To compare the number of mentions among topics, you can use a [Tumbling window](https://msdn.microsoft.com/library/azure/dn835055.aspx) to get the count of mentions by topic every five seconds.</span></span>

1. <span data-ttu-id="04a2c-262">Zavřít **vstupy** okno, pokud jste tak ještě neučinili.</span><span class="sxs-lookup"><span data-stu-id="04a2c-262">Close the **Inputs** blade if you haven't already.</span></span>

2. <span data-ttu-id="04a2c-263">V okně úlohy klikněte **dotazu** pole.</span><span class="sxs-lookup"><span data-stu-id="04a2c-263">In the job blade, click the **Query** box.</span></span> <span data-ttu-id="04a2c-264">Azure uvádí vstupy a výstupy, které jsou nakonfigurovány pro úlohy a umožňuje vám vytvořit dotaz, který vám umožňuje transformovat vstupní datový proud, jako jsou odeslána do výstupu.</span><span class="sxs-lookup"><span data-stu-id="04a2c-264">Azure lists the inputs and outputs that are configured for the job, and lets you create a query that lets you transform the input stream as it is sent to the output.</span></span>

3. <span data-ttu-id="04a2c-265">Ujistěte se, že je spuštěna aplikace TwitterWpfClient.</span><span class="sxs-lookup"><span data-stu-id="04a2c-265">Make sure that the TwitterWpfClient application is running.</span></span> 

3. <span data-ttu-id="04a2c-266">V **dotazu** okně klikněte na tlačítko se tečkami vedle `TwitterStream` vstup a pak vyberte **vzorová data ze vstupu**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-266">In the **Query** blade, click the dots next to the `TwitterStream` input and then select **Sample data from input**.</span></span>

    ![Položka, s "Ukázkových dat ze vstupu" vybrané úlohy možnosti nabídky ukázková data pro analýzy datových proudů](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-sample-data-from-input.png)

    <span data-ttu-id="04a2c-268">Otevře se okno, které umožňuje určit, kolik ukázková data získat, definována v tom, jak dlouho ke čtení vstupního datového proudu.</span><span class="sxs-lookup"><span data-stu-id="04a2c-268">This opens a blade that lets you specify how much sample data to get, defined in terms of how long to read the input stream.</span></span>

4. <span data-ttu-id="04a2c-269">Nastavit **minut** na 3 a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-269">Set **Minutes** to 3 and then click **OK**.</span></span> 
    
    ![Možnosti vzorkování vstupního datového proudu s vybrané "3 minut".](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-input-create-sample-data.png)

    <span data-ttu-id="04a2c-271">Azure ukázky data z vstupního datového proudu za 3 minuty a vás upozorní, jakmile je připraven ukázková data.</span><span class="sxs-lookup"><span data-stu-id="04a2c-271">Azure samples 3 minutes' worth of data from the input stream and notifies you when the sample data is ready.</span></span> <span data-ttu-id="04a2c-272">(To trvá nějakou dobu.)</span><span class="sxs-lookup"><span data-stu-id="04a2c-272">(This takes a short while.)</span></span> 

    <span data-ttu-id="04a2c-273">Ukázková data jsou ukládána dočasně a je k dispozici při otevření okna dotazu.</span><span class="sxs-lookup"><span data-stu-id="04a2c-273">The sample data is stored temporarily and is available while you have the query window open.</span></span> <span data-ttu-id="04a2c-274">Pokud zavřete okno dotazu se zahodí ukázková data a je nutné vytvořit novou sadu ukázková data.</span><span class="sxs-lookup"><span data-stu-id="04a2c-274">If you close the query window, the sample data is discarded, and you have to create a new set of sample data.</span></span> 

5. <span data-ttu-id="04a2c-275">Změna dotazu v editoru kódu takto:</span><span class="sxs-lookup"><span data-stu-id="04a2c-275">Change the query in the code editor to the following:</span></span>

    ```
    SELECT System.Timestamp as Time, Topic, COUNT(*)
    FROM TwitterStream TIMESTAMP BY CreatedAt
    GROUP BY TUMBLINGWINDOW(s, 5), Topic
    ```

    <span data-ttu-id="04a2c-276">Pokud nepoužili `TwitterStream` jako alias pro vstup, nahraďte váš alias pro `TwitterStream` v dotazu.</span><span class="sxs-lookup"><span data-stu-id="04a2c-276">If didn't use `TwitterStream` as the alias for the input, substitute your alias for `TwitterStream` in the query.</span></span>  

    <span data-ttu-id="04a2c-277">Tento dotaz používá **TIMESTAMP BY** – klíčové slovo a určete pole časového razítka v datové části pro použití v dočasné výpočty.</span><span class="sxs-lookup"><span data-stu-id="04a2c-277">This query uses the **TIMESTAMP BY** keyword to specify a timestamp field in the payload to be used in the temporal computation.</span></span> <span data-ttu-id="04a2c-278">Pokud toto pole nezadáte, operace oddílová se provádí na základě doby, které byly přijaty každé události centra událostí.</span><span class="sxs-lookup"><span data-stu-id="04a2c-278">If this field isn't specified, the windowing operation is performed by using the time that each event arrived at the event hub.</span></span> <span data-ttu-id="04a2c-279">Další informace najdete v části "Čas doručení vs doba aplikace" [referenční příručka Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx).</span><span class="sxs-lookup"><span data-stu-id="04a2c-279">Learn more in the "Arrival Time vs Application Time" section of [Stream Analytics Query Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx).</span></span>

    <span data-ttu-id="04a2c-280">Tento dotaz také přistoupí časového razítka na konci každé okno pomocí **System.Timestamp** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="04a2c-280">This query also accesses a timestamp for the end of each window by using the **System.Timestamp** property.</span></span>

5. <span data-ttu-id="04a2c-281">Klikněte na tlačítko **Test**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-281">Click **Test**.</span></span> <span data-ttu-id="04a2c-282">Spuštění dotazu na data, která jste vzorků.</span><span class="sxs-lookup"><span data-stu-id="04a2c-282">The query runs against the data that you sampled.</span></span>
    
6. <span data-ttu-id="04a2c-283">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-283">Click **Save**.</span></span> <span data-ttu-id="04a2c-284">To umožňuje ušetřit dotaz jako součást úlohy streamování Analytics.</span><span class="sxs-lookup"><span data-stu-id="04a2c-284">This saves the query as part of the Streaming Analytics job.</span></span> <span data-ttu-id="04a2c-285">(Ho nebude uložen ukázková data.)</span><span class="sxs-lookup"><span data-stu-id="04a2c-285">(It doesn't save the sample data.)</span></span>


## <a name="experiment-using-different-fields-from-the-stream"></a><span data-ttu-id="04a2c-286">Experiment pomocí různých polí z datového proudu</span><span class="sxs-lookup"><span data-stu-id="04a2c-286">Experiment using different fields from the stream</span></span> 

<span data-ttu-id="04a2c-287">Následující tabulka uvádí pole, které jsou součástí JSON streamovaných dat užitečné.</span><span class="sxs-lookup"><span data-stu-id="04a2c-287">The following table lists the fields that are part of the JSON streaming data.</span></span> <span data-ttu-id="04a2c-288">Nebojte se, že experiment v editoru dotazů.</span><span class="sxs-lookup"><span data-stu-id="04a2c-288">Feel free to experiment in the query editor.</span></span>

|<span data-ttu-id="04a2c-289">Vlastnost JSON</span><span class="sxs-lookup"><span data-stu-id="04a2c-289">JSON property</span></span> | <span data-ttu-id="04a2c-290">Definice</span><span class="sxs-lookup"><span data-stu-id="04a2c-290">Definition</span></span>|
|--- | ---|
|<span data-ttu-id="04a2c-291">CreatedAt</span><span class="sxs-lookup"><span data-stu-id="04a2c-291">CreatedAt</span></span> | <span data-ttu-id="04a2c-292">Čas, který byl vytvořen tweet</span><span class="sxs-lookup"><span data-stu-id="04a2c-292">The time that the tweet was created</span></span>|
|<span data-ttu-id="04a2c-293">Téma</span><span class="sxs-lookup"><span data-stu-id="04a2c-293">Topic</span></span> | <span data-ttu-id="04a2c-294">Téma, které odpovídá zadané klíčové slovo</span><span class="sxs-lookup"><span data-stu-id="04a2c-294">The topic that matches the specified keyword</span></span>|
|<span data-ttu-id="04a2c-295">SentimentScore</span><span class="sxs-lookup"><span data-stu-id="04a2c-295">SentimentScore</span></span> | <span data-ttu-id="04a2c-296">Skóre postojích z Sentiment140</span><span class="sxs-lookup"><span data-stu-id="04a2c-296">The sentiment score from Sentiment140</span></span>|
|<span data-ttu-id="04a2c-297">Autor</span><span class="sxs-lookup"><span data-stu-id="04a2c-297">Author</span></span> | <span data-ttu-id="04a2c-298">Odeslaný tweet popisovač služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="04a2c-298">The Twitter handle that sent the tweet</span></span>|
|<span data-ttu-id="04a2c-299">Text</span><span class="sxs-lookup"><span data-stu-id="04a2c-299">Text</span></span> | <span data-ttu-id="04a2c-300">Úplné tělo tweet</span><span class="sxs-lookup"><span data-stu-id="04a2c-300">The full body of the tweet</span></span>|


## <a name="create-an-output-sink"></a><span data-ttu-id="04a2c-301">Vytvoření výstupní jímku</span><span class="sxs-lookup"><span data-stu-id="04a2c-301">Create an output sink</span></span>

<span data-ttu-id="04a2c-302">Nyní jste definovali datového proudu událostí, centra událostí vstup pro načítání událostí a dotaz, který provádět prostřednictvím datového proudu transformace.</span><span class="sxs-lookup"><span data-stu-id="04a2c-302">You have now defined an event stream, an event hub input to ingest events, and a query to perform a transformation over the stream.</span></span> <span data-ttu-id="04a2c-303">Posledním krokem je definování výstupní jímku pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="04a2c-303">The last step is to define an output sink for the job.</span></span>  

<span data-ttu-id="04a2c-304">V tomto kurzu můžete zapsat události agregované tweet z dotazu úlohy do úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="04a2c-304">In this tutorial, you write the aggregated tweet events from the job query to Azure Blob storage.</span></span>  <span data-ttu-id="04a2c-305">Výsledky můžete také nabízené k databázi SQL Azure, Azure Table storage, Event Hubs, nebo Power BI, v závislosti na vaší aplikace potřebuje.</span><span class="sxs-lookup"><span data-stu-id="04a2c-305">You can also push your results to Azure SQL Database, Azure Table storage, Event Hubs, or Power BI, depending on your application needs.</span></span>

## <a name="specify-the-job-output"></a><span data-ttu-id="04a2c-306">Zadejte výstup úlohy</span><span class="sxs-lookup"><span data-stu-id="04a2c-306">Specify the job output</span></span>

1. <span data-ttu-id="04a2c-307">V **úlohy topologie** klikněte na položku **výstup** pole.</span><span class="sxs-lookup"><span data-stu-id="04a2c-307">In the **Job Topology** section, click the **Output** box.</span></span> 

2. <span data-ttu-id="04a2c-308">V **výstupy** okně klikněte na tlačítko  **+ &nbsp;přidat** a potom vyplňte v okně s těmito hodnotami:</span><span class="sxs-lookup"><span data-stu-id="04a2c-308">In the **Outputs** blade, click **+&nbsp;Add** and then fill out the blade with these values:</span></span>

    * <span data-ttu-id="04a2c-309">**Alias pro výstup**: použijte název `TwitterStream-Output`.</span><span class="sxs-lookup"><span data-stu-id="04a2c-309">**Output alias**: Use the name `TwitterStream-Output`.</span></span> 
    * <span data-ttu-id="04a2c-310">**Jímky**: vyberte **úložiště objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-310">**Sink**: Select **Blob storage**.</span></span>
    * <span data-ttu-id="04a2c-311">**Možnosti importu**: vyberte **pomocí úložiště objektů blob z aktuálního předplatného**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-311">**Import options**: Select **Use blob storage from current subscription**.</span></span>
    * <span data-ttu-id="04a2c-312">**Účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-312">**Storage account**.</span></span> <span data-ttu-id="04a2c-313">Vyberte **vytvořit nový účet úložiště.**</span><span class="sxs-lookup"><span data-stu-id="04a2c-313">Select **Create a new storage account.**</span></span>
    * <span data-ttu-id="04a2c-314">**Účet úložiště** (druhé pole).</span><span class="sxs-lookup"><span data-stu-id="04a2c-314">**Storage account** (second box).</span></span> <span data-ttu-id="04a2c-315">Zadejte `YOURNAMEsa`, kde `YOURNAME` je název vaší nebo jiné jedinečné řetězce.</span><span class="sxs-lookup"><span data-stu-id="04a2c-315">Enter `YOURNAMEsa`, where `YOURNAME` is your name or another unique string.</span></span> <span data-ttu-id="04a2c-316">Název můžete použít jenom malá písmena a čísla a musí být jedinečný v Azure.</span><span class="sxs-lookup"><span data-stu-id="04a2c-316">The name can use only lowercase letters and numbers, and it must be unique across Azure.</span></span> 
    * <span data-ttu-id="04a2c-317">**Kontejner**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-317">**Container**.</span></span> <span data-ttu-id="04a2c-318">Zadejte `socialtwitter`.</span><span class="sxs-lookup"><span data-stu-id="04a2c-318">Enter `socialtwitter`.</span></span>
    <span data-ttu-id="04a2c-319">Název účtu úložiště a název kontejneru se používají společně zajistit identifikátor URI pro úložiště objektů blob, například takto:</span><span class="sxs-lookup"><span data-stu-id="04a2c-319">The storage account name and container name are used together to provide a URI for the blob storage, like this:</span></span> 

    `http://YOURNAMEsa.blob.core.windows.net/socialtwitter/...`
    
    !["Nové výstup" okno úlohy Stream Analytics](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-output-blob-storage.png)
    
4. <span data-ttu-id="04a2c-321">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-321">Click **Create**.</span></span> 

    <span data-ttu-id="04a2c-322">Azure vytvoří účet úložiště a automaticky vygeneruje klíč.</span><span class="sxs-lookup"><span data-stu-id="04a2c-322">Azure creates the storage account and generates a key automatically.</span></span> 

5. <span data-ttu-id="04a2c-323">Zavřít **výstupy** okno.</span><span class="sxs-lookup"><span data-stu-id="04a2c-323">Close the **Outputs** blade.</span></span> 


## <a name="start-the-job"></a><span data-ttu-id="04a2c-324">Spustit úlohu</span><span class="sxs-lookup"><span data-stu-id="04a2c-324">Start the job</span></span>

<span data-ttu-id="04a2c-325">Úloha vstup, dotaz a výstup nejsou zadány.</span><span class="sxs-lookup"><span data-stu-id="04a2c-325">A job input, query, and output are specified.</span></span> <span data-ttu-id="04a2c-326">Jste připraveni začít úlohu služby Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="04a2c-326">You are ready to start the Stream Analytics job.</span></span>

1. <span data-ttu-id="04a2c-327">Ujistěte se, že je spuštěna aplikace TwitterWpfClient.</span><span class="sxs-lookup"><span data-stu-id="04a2c-327">Make sure that the TwitterWpfClient application is running.</span></span> 

2. <span data-ttu-id="04a2c-328">V okně úlohy klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-328">In the job blade, click **Start**.</span></span>

    ![Spustit úlohu služby Stream Analytics](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sa-job-start-output.png)

3. <span data-ttu-id="04a2c-330">V **spuštění úlohy** okně pro **výstup úlohy počáteční čas**, vyberte **nyní** a pak klikněte na **spustit**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-330">In the **Start job** blade, for **Job output start time**, select **Now** and then click **Start**.</span></span> 

    !["Spuštění úlohy" okno pro úlohu služby Stream Analytics](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sa-job-start-job-blade.png)

    <span data-ttu-id="04a2c-332">Azure vás upozorní, jakmile byla úloha spuštěna, a v okně úlohy, stav se zobrazí jako **systémem**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-332">Azure notifies you when the job has started, and in the job blade, the status is displayed as **Running**.</span></span>

    ![Spuštěná úloha](./media/stream-analytics-twitter-sentiment-analysis-trends/jobrunning.png)

## <a name="view-output-for-sentiment-analysis"></a><span data-ttu-id="04a2c-334">Zobrazit výstup pro analýzu postojích</span><span class="sxs-lookup"><span data-stu-id="04a2c-334">View output for sentiment analysis</span></span>

<span data-ttu-id="04a2c-335">Po zahájení systémem úlohu zpracovává datový proud v reálném čase Twitter, můžete zobrazit výstup pro analýzu postojích.</span><span class="sxs-lookup"><span data-stu-id="04a2c-335">After your job has started running and is processing the real-time Twitter stream, you can view the output for sentiment analysis.</span></span>

<span data-ttu-id="04a2c-336">Můžete použít nástroje, jako je [Azure Storage Explorer](https://http://storageexplorer.com/) nebo [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) k zobrazení výstupu úlohy v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="04a2c-336">You can use a tool like [Azure Storage Explorer](https://http://storageexplorer.com/) or [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) to view your job output in real time.</span></span> <span data-ttu-id="04a2c-337">Tady můžete použít [Power BI](https://powerbi.com/) rozšířit vaše aplikace a zahrnují přizpůsobený řídicí panel stejný, jako je znázorněno na následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="04a2c-337">From here, you can use [Power BI](https://powerbi.com/) to extend your application to include a customized dashboard like the one shown in the following screenshot:</span></span>

![Power BI](./media/stream-analytics-twitter-sentiment-analysis-trends/power-bi.png)


## <a name="create-another-query-to-identify-trending-topics"></a><span data-ttu-id="04a2c-339">Vytvořte další dotaz k identifikaci trendů témata</span><span class="sxs-lookup"><span data-stu-id="04a2c-339">Create another query to identify trending topics</span></span>

<span data-ttu-id="04a2c-340">Podle další dotaz můžete použít k pochopení sentimentu Twitter [posuvné okno](https://msdn.microsoft.com/library/azure/dn835051.aspx).</span><span class="sxs-lookup"><span data-stu-id="04a2c-340">Another query you can use to understand Twitter sentiment is based on a [Sliding Window](https://msdn.microsoft.com/library/azure/dn835051.aspx).</span></span> <span data-ttu-id="04a2c-341">K identifikaci trendů témata, vyhledejte témata, které zasahují prahová hodnota pro zmínkami ve stanoveném čase.</span><span class="sxs-lookup"><span data-stu-id="04a2c-341">To identify trending topics, you look for topics that cross a threshold value for mentions in a specified amount of time.</span></span>

<span data-ttu-id="04a2c-342">Pro účely tohoto kurzu vyhledejte témata, která jsou uveden více než 20 výskyty v posledních 5 sekund.</span><span class="sxs-lookup"><span data-stu-id="04a2c-342">For the purposes of this tutorial, you check for topics that are mentioned more than 20 times in the last 5 seconds.</span></span>

1. <span data-ttu-id="04a2c-343">V okně úlohy klikněte na tlačítko **Zastavit** k zastavení úlohy.</span><span class="sxs-lookup"><span data-stu-id="04a2c-343">In the job blade, click **Stop** to stop the job.</span></span> 

2. <span data-ttu-id="04a2c-344">V **úlohy topologie** klikněte na položku **dotazu** pole.</span><span class="sxs-lookup"><span data-stu-id="04a2c-344">In the **Job Topology** section, click the **Query** box.</span></span> 

3. <span data-ttu-id="04a2c-345">Změňte dotaz na následující:</span><span class="sxs-lookup"><span data-stu-id="04a2c-345">Change the query to the following:</span></span>

    ```    
    SELECT System.Timestamp as Time, Topic, COUNT(*) as Mentions
    FROM TwitterStream TIMESTAMP BY CreatedAt
    GROUP BY SLIDINGWINDOW(s, 5), topic
    HAVING COUNT(*) > 20
    ```

4. <span data-ttu-id="04a2c-346">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="04a2c-346">Click **Save**.</span></span>

5. <span data-ttu-id="04a2c-347">Ujistěte se, že je spuštěna aplikace TwitterWpfClient.</span><span class="sxs-lookup"><span data-stu-id="04a2c-347">Make sure that the TwitterWpfClient application is running.</span></span> 

6. <span data-ttu-id="04a2c-348">Klikněte na tlačítko **spustit** k restartování úlohy pomocí nový dotaz.</span><span class="sxs-lookup"><span data-stu-id="04a2c-348">Click **Start** to restart the job using the new query.</span></span>


## <a name="get-support"></a><span data-ttu-id="04a2c-349">Získat podporu</span><span class="sxs-lookup"><span data-stu-id="04a2c-349">Get support</span></span>
<span data-ttu-id="04a2c-350">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="04a2c-350">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="04a2c-351">Další kroky</span><span class="sxs-lookup"><span data-stu-id="04a2c-351">Next steps</span></span>
* [<span data-ttu-id="04a2c-352">Úvod do služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="04a2c-352">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="04a2c-353">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="04a2c-353">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="04a2c-354">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="04a2c-354">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="04a2c-355">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="04a2c-355">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="04a2c-356">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="04a2c-356">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
