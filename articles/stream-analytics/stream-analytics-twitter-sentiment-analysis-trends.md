---
title: "Analýza postojích Twitter aaaReal běhu pomocí služby Azure Stream Analytics | Microsoft Docs"
description: "Zjistěte, jak toouse Stream Analytics k analýze postojích v reálném čase služby Twitter. Podrobné pokyny z toodata generování událostí na řídicím panelu za provozu."
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
ms.openlocfilehash: 157790caa7ea6f5570dd9c9d3bd9694d437eb4c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="real-time-twitter-sentiment-analysis-in-azure-stream-analytics"></a><span data-ttu-id="cbac5-105">Analýzy v reálném čase postojích Twitter v Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="cbac5-105">Real-time Twitter sentiment analysis in Azure Stream Analytics</span></span>

<span data-ttu-id="cbac5-106">Zjistěte, jak toobuild postojích analýzu řešení pro analýzy sociálních médií tak, že převedou v reálném čase Twitter události do centra událostí Azure.</span><span class="sxs-lookup"><span data-stu-id="cbac5-106">Learn how toobuild a sentiment analysis solution for social media analytics by bringing real-time Twitter events into Azure Event Hubs.</span></span> <span data-ttu-id="cbac5-107">Poté můžete napsat data hello tooanalyze dotaz Azure Stream Analytics a buď úložiště hello výsledky pro pozdější použití nebo použít řídicí panel a [Power BI](https://powerbi.com/) tooprovide statistiky v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="cbac5-107">You can then write an Azure Stream Analytics query tooanalyze hello data and either store hello results for later use or use a dashboard and [Power BI](https://powerbi.com/) tooprovide insights in real time.</span></span>

<span data-ttu-id="cbac5-108">Analýza nástrojů sociálních médiích pomůžou organizacím pochopit trendů témata.</span><span class="sxs-lookup"><span data-stu-id="cbac5-108">Social media analytics tools help organizations understand trending topics.</span></span> <span data-ttu-id="cbac5-109">Trendů témata jsou témata a postojích, které mají k velkému počtu příspěvcích v sociálních sítích.</span><span class="sxs-lookup"><span data-stu-id="cbac5-109">Trending topics are subjects and attitudes that have a high volume of posts in social media.</span></span> <span data-ttu-id="cbac5-110">Postojích analýzy, která se označuje taky jako *stanovisko dolování*, používá sociálních médií analytics nástroje toodetermine postoje směrem k produktu, je vhodné a tak dále.</span><span class="sxs-lookup"><span data-stu-id="cbac5-110">Sentiment analysis, which is also called *opinion mining*, uses social media analytics tools toodetermine attitudes toward a product, idea, and so on.</span></span> 

<span data-ttu-id="cbac5-111">Analýza trendu v reálném čase Twitter je skvělé příklad nástroj pro analýzu, protože hello hashtag předplatné modelu vám umožní toospecific toolisten (hashtags) – klíčová slova a vyvíjet postojích analýzu hello informačního kanálu.</span><span class="sxs-lookup"><span data-stu-id="cbac5-111">Real-time Twitter trend analysis is a great example of an analytics tool, because hello hashtag subscription model enables you toolisten toospecific keywords (hashtags) and develop sentiment analysis of hello feed.</span></span>

## <a name="scenario-social-media-sentiment-analysis-in-real-time"></a><span data-ttu-id="cbac5-112">Scénář: Sociálních médií postojích analýzy v reálném čase</span><span class="sxs-lookup"><span data-stu-id="cbac5-112">Scenario: Social media sentiment analysis in real time</span></span>

<span data-ttu-id="cbac5-113">Společnost, která má web zprávy média má zájem o získání několik výhod před konkurencí podle s funkcí lokality obsah, který je okamžitě relevantní tooits čtečky.</span><span class="sxs-lookup"><span data-stu-id="cbac5-113">A company that has a news media website is interested in gaining an advantage over its competitors by featuring site content that is immediately relevant tooits readers.</span></span> <span data-ttu-id="cbac5-114">Společnost Hello používá analýzy sociálních médiích na témata, která jsou relevantní tooreaders díky v reálném čase postojích analýza dat Twitteru.</span><span class="sxs-lookup"><span data-stu-id="cbac5-114">hello company uses social media analysis on topics that are relevant tooreaders by doing real-time sentiment analysis of Twitter data.</span></span>

<span data-ttu-id="cbac5-115">témata trendů tooidentify v reálném čase v síti Twitter, hello společnosti potřeby analýzy v reálném čase o hello tweet svazek a postojích klíče témata.</span><span class="sxs-lookup"><span data-stu-id="cbac5-115">tooidentify trending topics in real time on Twitter, hello company needs real-time analytics about hello tweet volume and sentiment for key topics.</span></span> <span data-ttu-id="cbac5-116">Jinými slovy potřeba hello je modul postojích analysis analytics je založena na této sociálních médií informačního kanálu.</span><span class="sxs-lookup"><span data-stu-id="cbac5-116">In other words, hello need is a sentiment analysis analytics engine that's based on this social media feed.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cbac5-117">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cbac5-117">Prerequisites</span></span>
<span data-ttu-id="cbac5-118">V tomto kurzu použijete klientská aplikace, která se připojuje tooTwitter a hledá tweetů, které mají určité hashtags (kterou můžete nastavit).</span><span class="sxs-lookup"><span data-stu-id="cbac5-118">In this tutorial, you use a client application that connects tooTwitter and looks for tweets that have certain hashtags (which you can set).</span></span> <span data-ttu-id="cbac5-119">V pořadí toorun hello aplikace a analyzovat hello tweetů pomocí Azure streamování Analytics, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="cbac5-119">In order toorun hello application and analyze hello tweets using Azure Streaming Analytics, you must have hello following:</span></span>

* <span data-ttu-id="cbac5-120">Předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="cbac5-120">An Azure subscription</span></span>
* <span data-ttu-id="cbac5-121">Účet na Twitteru</span><span class="sxs-lookup"><span data-stu-id="cbac5-121">A Twitter account</span></span> 
* <span data-ttu-id="cbac5-122">Aplikace služby Twitter a hello [přístupového tokenu OAuth](https://dev.twitter.com/oauth/overview/application-owner-access-tokens) pro příslušnou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cbac5-122">A Twitter application, and hello [OAuth access token](https://dev.twitter.com/oauth/overview/application-owner-access-tokens) for that application.</span></span> <span data-ttu-id="cbac5-123">Poskytujeme vysoké úrovně pokyny, jak toocreate aplikace Twitter později.</span><span class="sxs-lookup"><span data-stu-id="cbac5-123">We provide high-level instructions for how toocreate a Twitter application later.</span></span>
* <span data-ttu-id="cbac5-124">Hello TwitterWPFClient aplikace, který čte hello Twitter informačního kanálu.</span><span class="sxs-lookup"><span data-stu-id="cbac5-124">hello TwitterWPFClient application, which reads hello Twitter feed.</span></span> <span data-ttu-id="cbac5-125">tooget pro tuto aplikaci, stahování hello [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) souborů z webu GitHub a potom rozbalte balíček hello do složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="cbac5-125">tooget this application, download hello [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) file from GitHub and then unzip hello package into a folder on your computer.</span></span> <span data-ttu-id="cbac5-126">Pokud chcete, aby toosee hello zdrojového kódu a spustit aplikaci hello v ladicí program, můžete získat hello zdrojového kódu z [Githubu](https://aka.ms/azure-stream-analytics-telcogenerator).</span><span class="sxs-lookup"><span data-stu-id="cbac5-126">If you want toosee hello source code and run hello application in a debugger, you can get hello source code from [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator).</span></span> 

## <a name="create-an-event-hub-for-streaming-analytics-input"></a><span data-ttu-id="cbac5-127">Vytvoření centra událostí pro vstup analýzy datových proudů</span><span class="sxs-lookup"><span data-stu-id="cbac5-127">Create an event hub for Streaming Analytics input</span></span>

<span data-ttu-id="cbac5-128">Ukázková aplikace Hello vygeneruje události a nabízených oznámení, je centra událostí Azure tooan.</span><span class="sxs-lookup"><span data-stu-id="cbac5-128">hello sample application generates events and pushes them tooan Azure event hub.</span></span> <span data-ttu-id="cbac5-129">Azure event hubs je hello upřednostňovaný způsob přijímání událostí pro Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="cbac5-129">Azure event hubs are hello preferred method of event ingestion for Stream Analytics.</span></span> <span data-ttu-id="cbac5-130">Další informace najdete v tématu hello [dokumentace Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="cbac5-130">For more information, see hello [Azure Event Hubs documentation](../event-hubs/event-hubs-what-is-event-hubs.md).</span></span>


### <a name="create-an-event-hub-namespace-and-event-hub"></a><span data-ttu-id="cbac5-131">Vytvoření centra událostí obor názvů a centra událostí</span><span class="sxs-lookup"><span data-stu-id="cbac5-131">Create an event hub namespace and event hub</span></span>
<span data-ttu-id="cbac5-132">V tomto postupu vytvoříte na obor názvů centra událostí a poté přidejte na obor názvů toothat centra událostí.</span><span class="sxs-lookup"><span data-stu-id="cbac5-132">In this procedure, you first create an event hub namespace, and then you add an event hub toothat namespace.</span></span> <span data-ttu-id="cbac5-133">Obory názvů centra událostí jsou používány toologically seskupení souvisejících událostí sběrnice instancí.</span><span class="sxs-lookup"><span data-stu-id="cbac5-133">Event hub namespaces are used toologically group related event bus instances.</span></span> 

1. <span data-ttu-id="cbac5-134">Přihlaste se toohello portál Azure a klikněte na tlačítko **nový** > **Internet věcí** > **centra událostí**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-134">Log  in toohello Azure portal and click **New** > **Internet of Things** > **Event Hub**.</span></span> 

2. <span data-ttu-id="cbac5-135">V hello **vytvoření oboru názvů** okno, zadejte název oboru názvů, jako `<yourname>-socialtwitter-eh-ns`.</span><span class="sxs-lookup"><span data-stu-id="cbac5-135">In hello **Create namespace** blade, enter a namespace name such as `<yourname>-socialtwitter-eh-ns`.</span></span> <span data-ttu-id="cbac5-136">Můžete použít libovolný název pro obor názvů hello, ale název hello musí být platné adresy URL a musí být jedinečný v Azure.</span><span class="sxs-lookup"><span data-stu-id="cbac5-136">You can use any name for hello namespace, but hello name must be valid for a URL and it must be unique across Azure.</span></span> 
    
3. <span data-ttu-id="cbac5-137">Vyberte předplatné a vytvořit nebo vybrat skupinu prostředků a potom klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-137">Select a subscription and create or choose a resource group, then click **Create**.</span></span> 

    ![Vytvoření oboru názvů události rozbočovače](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub-namespace.png)
 
4. <span data-ttu-id="cbac5-139">Po dokončení nasazení obor názvů hello najdete hello události rozbočovače oboru názvů v seznamu prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="cbac5-139">When hello namespace has finished deploying, find hello event hub namespace in your list of Azure resources.</span></span> 

5. <span data-ttu-id="cbac5-140">Klikněte na tlačítko Nový obor názvů hello a v okně hello obor názvů, klikněte na tlačítko  **+ &nbsp;centra událostí**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-140">Click hello new namespace, and in hello namespace blade, click **+&nbsp;Event Hub**.</span></span> 

    ![<span data-ttu-id="cbac5-141">tlačítko Přidat centra událostí Hello pro vytvoření nového centra událostí</span><span class="sxs-lookup"><span data-stu-id="cbac5-141">hello Add Event Hub button for creating a new event hub</span></span> ](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub-button.png)    
 
6. <span data-ttu-id="cbac5-142">Název hello nového centra událostí `socialtwitter-eh`.</span><span class="sxs-lookup"><span data-stu-id="cbac5-142">Name hello new event hub `socialtwitter-eh`.</span></span> <span data-ttu-id="cbac5-143">Můžete použít jiný název.</span><span class="sxs-lookup"><span data-stu-id="cbac5-143">You can use a different name.</span></span> <span data-ttu-id="cbac5-144">Pokud tak učiníte, poznamenejte si ho, protože potřebujete název hello později.</span><span class="sxs-lookup"><span data-stu-id="cbac5-144">If you do, make a note of it, because you need hello name later.</span></span> <span data-ttu-id="cbac5-145">Nepotřebujete tooset další možnosti pro centra událostí hello.</span><span class="sxs-lookup"><span data-stu-id="cbac5-145">You don't need tooset any other options for hello event hub.</span></span>

    ![Okno pro vytvoření nového centra událostí](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub.png)
 
7. <span data-ttu-id="cbac5-147">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-147">Click **Create**.</span></span>


### <a name="grant-access-toohello-event-hub"></a><span data-ttu-id="cbac5-148">Centra událostí toohello udělení přístupu</span><span class="sxs-lookup"><span data-stu-id="cbac5-148">Grant access toohello event hub</span></span>

<span data-ttu-id="cbac5-149">Předtím, než se proces může odesílat data centra událostí tooan, musí mít centra událostí hello zásadu, která umožňuje odpovídající přístup.</span><span class="sxs-lookup"><span data-stu-id="cbac5-149">Before a process can send data tooan event hub, hello event hub must have a policy that allows appropriate access.</span></span> <span data-ttu-id="cbac5-150">zásady přístupu Hello vytvoří připojovací řetězec, který obsahuje informace o ověření.</span><span class="sxs-lookup"><span data-stu-id="cbac5-150">hello access policy produces a connection string that includes authorization information.</span></span>

1.  <span data-ttu-id="cbac5-151">V okně oboru názvů událostí hello, klikněte na tlačítko **Event Hubs** a pak klikněte na název hello nového centra událostí.</span><span class="sxs-lookup"><span data-stu-id="cbac5-151">In hello event namespace blade, click **Event Hubs** and then click hello name of your new event hub.</span></span>

2.  <span data-ttu-id="cbac5-152">V okně Centrum událostí hello, klikněte na tlačítko **zásady sdíleného přístupu** a pak klikněte na  **+ &nbsp;přidat**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-152">In hello event hub blade, click **Shared access policies** and then click **+&nbsp;Add**.</span></span>

    >[!NOTE]
    ><span data-ttu-id="cbac5-153">Ujistěte se, že pracujete s hello centra událostí, není hello názvů centra událostí.</span><span class="sxs-lookup"><span data-stu-id="cbac5-153">Make sure you're working with hello event hub, not hello event hub namespace.</span></span>

3.  <span data-ttu-id="cbac5-154">Přidat zásadu s názvem `socialtwitter-access` a **deklarace identity**, vyberte **spravovat**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-154">Add a policy named `socialtwitter-access` and for **Claim**, select **Manage**.</span></span>

    ![Okno pro vytvoření nové zásady přístupu centra událostí](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-shared-access-policy-manage.png)
 
4.  <span data-ttu-id="cbac5-156">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-156">Click **Create**.</span></span>

5.  <span data-ttu-id="cbac5-157">Po hello byla zásada nasazená, klikněte na něj v seznamu hello zásady sdíleného přístupu.</span><span class="sxs-lookup"><span data-stu-id="cbac5-157">After hello policy has been deployed, click it in hello list of shared access policies.</span></span>

6.  <span data-ttu-id="cbac5-158">Najít hello políčko s názvem **PŘIPOJOVACÍ řetězec primární klíč** a klikněte na tlačítko hello kopie tlačítko Další toohello připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="cbac5-158">Find hello box labeled **CONNECTION STRING-PRIMARY KEY** and click hello copy button next toohello connection string.</span></span> 
    
    ![Kopírování hello primární připojovací řetězec klíč ze zásad přístupu hello](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-shared-access-policy-copy-connection-string.png)
 
7.  <span data-ttu-id="cbac5-160">Vložte připojovací řetězec hello do textového editoru.</span><span class="sxs-lookup"><span data-stu-id="cbac5-160">Paste hello connection string into a text editor.</span></span> <span data-ttu-id="cbac5-161">Tento připojovací řetězec pro další části hello, musíte po provedení některé tooit malé úpravy.</span><span class="sxs-lookup"><span data-stu-id="cbac5-161">You need this connection string for hello next section, after you make some small edits tooit.</span></span>

    <span data-ttu-id="cbac5-162">Hello připojovací řetězec vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="cbac5-162">hello connection string looks like this:</span></span>

        Endpoint=sb://YOURNAME-socialtwitter-eh-ns.servicebus.windows.net/;SharedAccessKeyName=socialtwitter-access;SharedAccessKey=Gw2NFZw6r...FxKbXaC2op6a0ZsPkI=;EntityPath=socialtwitter-eh

    <span data-ttu-id="cbac5-163">Všimněte si, že hello připojovací řetězec obsahuje více párů klíč hodnota oddělené středníky: `Endpoint`, `SharedAccessKeyName`, `SharedAccessKey`, a `EntityPath`.</span><span class="sxs-lookup"><span data-stu-id="cbac5-163">Notice that hello connection string contains multiple key-value pairs, separated with semicolons: `Endpoint`, `SharedAccessKeyName`, `SharedAccessKey`, and `EntityPath`.</span></span>  

    > [!NOTE]
    > <span data-ttu-id="cbac5-164">Pro zabezpečení se odebraly částí hello připojovací řetězec v příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="cbac5-164">For security, parts of hello connection string in hello example have been removed.</span></span>

8.  <span data-ttu-id="cbac5-165">V textovém editoru hello odebrat hello `EntityPath` pár z hello připojovací řetězec (Nezapomeňte tooremove hello středník, předcházejícího).</span><span class="sxs-lookup"><span data-stu-id="cbac5-165">In hello text editor, remove hello `EntityPath` pair from hello connection string (don't forget tooremove hello semicolon that precedes it).</span></span> <span data-ttu-id="cbac5-166">Když jste hotovi, hello připojovací řetězec vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="cbac5-166">When you're done, hello connection string looks like this:</span></span>

        Endpoint=sb://YOURNAME-socialtwitter-eh-ns.servicebus.windows.net/;SharedAccessKeyName=socialtwitter-access;SharedAccessKey=Gw2NFZw6r...FxKbXaC2op6a0ZsPkI=


## <a name="configure-and-start-hello-twitter-client-application"></a><span data-ttu-id="cbac5-167">Nakonfigurujte a spusťte hello Twitter klientské aplikace</span><span class="sxs-lookup"><span data-stu-id="cbac5-167">Configure and start hello Twitter client application</span></span>
<span data-ttu-id="cbac5-168">klientská aplikace Hello získá tweet události přímo ze služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="cbac5-168">hello client application gets tweet events directly from Twitter.</span></span> <span data-ttu-id="cbac5-169">V pořadí toodo, tak, potřebuje oprávnění toocall hello Twitter streamování rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="cbac5-169">In order toodo so, it needs permission toocall hello Twitter Streaming APIs.</span></span> <span data-ttu-id="cbac5-170">tooconfigure, oprávnění, vytvoříte aplikaci v Twitter, který generuje jedinečné přihlašovací údaje (například tokenu OAuth).</span><span class="sxs-lookup"><span data-stu-id="cbac5-170">tooconfigure that permission, you create an application in Twitter, which generates unique credentials (such as an OAuth token).</span></span> <span data-ttu-id="cbac5-171">Potom můžete nakonfigurovat hello klienta aplikace toouse tyto přihlašovací údaje při volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="cbac5-171">You can then configure hello client application toouse these credentials when it makes API calls.</span></span> 

### <a name="create-a-twitter-application"></a><span data-ttu-id="cbac5-172">Vytvořit aplikaci služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="cbac5-172">Create a Twitter application</span></span>
<span data-ttu-id="cbac5-173">Pokud již nemáte Twitter aplikace, která můžete použít pro tento kurz, můžete vytvořit jeden.</span><span class="sxs-lookup"><span data-stu-id="cbac5-173">If you do not already have a Twitter application that you can use for this tutorial, you can create one.</span></span> <span data-ttu-id="cbac5-174">Již musí mít účet služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="cbac5-174">You must already have a Twitter account.</span></span>

> [!NOTE]
> <span data-ttu-id="cbac5-175">přesný postup Hello v Twitter pro vytváření aplikací a získávání klíčů hello, tajných klíčů a token může změnit.</span><span class="sxs-lookup"><span data-stu-id="cbac5-175">hello exact process in Twitter for creating an application and getting hello keys, secrets, and token might change.</span></span> <span data-ttu-id="cbac5-176">Pokud tyto pokyny neodpovídají, co vidíte v lokalitě hello Twitter, naleznete v dokumentaci pro vývojáře toohello Twitter.</span><span class="sxs-lookup"><span data-stu-id="cbac5-176">If these instructions don't match what you see on hello Twitter site, refer toohello Twitter developer documentation.</span></span>

1. <span data-ttu-id="cbac5-177">Přejděte toohello [stránku Správa aplikací Twitter](https://apps.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="cbac5-177">Go toohello [Twitter application management page](https://apps.twitter.com/).</span></span> 

2. <span data-ttu-id="cbac5-178">Vytvoření nové aplikace.</span><span class="sxs-lookup"><span data-stu-id="cbac5-178">Create a new application.</span></span> 

    * <span data-ttu-id="cbac5-179">Adresa URL webu hello zadejte platnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="cbac5-179">For hello website URL, specify a valid URL.</span></span> <span data-ttu-id="cbac5-180">Nemá toobe živý web.</span><span class="sxs-lookup"><span data-stu-id="cbac5-180">It does not have toobe a live site.</span></span> <span data-ttu-id="cbac5-181">(Není možné určit pouze `localhost`.)</span><span class="sxs-lookup"><span data-stu-id="cbac5-181">(You can't specify just `localhost`.)</span></span>
    * <span data-ttu-id="cbac5-182">Pole zpětného volání hello zůstat prázdné.</span><span class="sxs-lookup"><span data-stu-id="cbac5-182">Leave hello callback field blank.</span></span> <span data-ttu-id="cbac5-183">Hello klientská aplikace, kterou použijete pro tento kurz nevyžaduje zpětných volání.</span><span class="sxs-lookup"><span data-stu-id="cbac5-183">hello client application you use for this tutorial doesn't require callbacks.</span></span>

    ![Vytváření aplikací v Twitter](./media/stream-analytics-twitter-sentiment-analysis-trends/create-twitter-application.png)

3. <span data-ttu-id="cbac5-185">Volitelně můžete změňte oprávnění aplikace hello jen tooread.</span><span class="sxs-lookup"><span data-stu-id="cbac5-185">Optionally, change hello application's permissions tooread-only.</span></span>

4. <span data-ttu-id="cbac5-186">Při vytváření aplikace hello přejděte toohello **klíče a přístupové tokeny** stránky.</span><span class="sxs-lookup"><span data-stu-id="cbac5-186">When hello application is created, go toohello **Keys and Access Tokens** page.</span></span>

5. <span data-ttu-id="cbac5-187">Klikněte na tlačítko toogenerate hello přístupový token a tajný klíč přístupového tokenu.</span><span class="sxs-lookup"><span data-stu-id="cbac5-187">Click hello button toogenerate an access token and access token secret.</span></span>

<span data-ttu-id="cbac5-188">Ponechat tyto informace užitečné, protože je budete potřebovat v dalším postupu hello.</span><span class="sxs-lookup"><span data-stu-id="cbac5-188">Keep this information handy, because you will need it in hello next procedure.</span></span>

>[!NOTE]
><span data-ttu-id="cbac5-189">Hello klíče a tajné klíče pro aplikaci služby Twitter hello zadejte účet pro přístup k tooyour Twitter.</span><span class="sxs-lookup"><span data-stu-id="cbac5-189">hello keys and secrets for hello Twitter application provide access tooyour Twitter account.</span></span> <span data-ttu-id="cbac5-190">S takovými informacemi nakládat jako velká a malá písmena, hello stejné jako heslo služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="cbac5-190">Treat this information as sensitive, hello same as you do your Twitter password.</span></span> <span data-ttu-id="cbac5-191">Nevnořujte například v aplikaci tyto informace, že poskytnete tooothers.</span><span class="sxs-lookup"><span data-stu-id="cbac5-191">For example, don't embed this information in an application that you give tooothers.</span></span> 


### <a name="configure-hello-client-application"></a><span data-ttu-id="cbac5-192">Konfigurace hello klientské aplikace</span><span class="sxs-lookup"><span data-stu-id="cbac5-192">Configure hello client application</span></span>
<span data-ttu-id="cbac5-193">Vytvořili jsme klientskou aplikaci, která se připojuje tooTwitter dat pomocí [rozhraní API pro Streaming na Twitteru](https://dev.twitter.com/streaming/overview) toocollect tweet události týkající se konkrétní sadu témat.</span><span class="sxs-lookup"><span data-stu-id="cbac5-193">We've created a client application that connects tooTwitter data using [Twitter's Streaming APIs](https://dev.twitter.com/streaming/overview) toocollect tweet events about a specific set of topics.</span></span> <span data-ttu-id="cbac5-194">aplikace Hello používá hello [Sentiment140](http://help.sentiment140.com/) nástroj s otevřeným zdrojem, který přiřazuje hello následující postojích hodnotu tooeach tweet:</span><span class="sxs-lookup"><span data-stu-id="cbac5-194">hello application uses hello [Sentiment140](http://help.sentiment140.com/) open source tool, which assigns hello following sentiment value tooeach tweet:</span></span>

* <span data-ttu-id="cbac5-195">0 = záporná</span><span class="sxs-lookup"><span data-stu-id="cbac5-195">0 = negative</span></span>
* <span data-ttu-id="cbac5-196">2 = neutral</span><span class="sxs-lookup"><span data-stu-id="cbac5-196">2 = neutral</span></span>
* <span data-ttu-id="cbac5-197">4 = kladné</span><span class="sxs-lookup"><span data-stu-id="cbac5-197">4 = positive</span></span>

<span data-ttu-id="cbac5-198">Po události tweet hello byly přiřazeny hodnotu postojích, odesílají se toohello centra událostí, který jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="cbac5-198">After hello tweet events have been assigned a sentiment value, they are pushed toohello event hub that you created earlier.</span></span>

<span data-ttu-id="cbac5-199">Před spuštěním aplikace hello, vyžaduje určité informace z vašeho počítače, jako je hello Twitter klíče a hello event hub připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="cbac5-199">Before hello application runs, it requires certain information from you, like hello Twitter keys and hello event hub connection string.</span></span> <span data-ttu-id="cbac5-200">Můžete zadat informace o konfiguraci hello těmito způsoby:</span><span class="sxs-lookup"><span data-stu-id="cbac5-200">You can provide hello configuration information in these ways:</span></span>

* <span data-ttu-id="cbac5-201">Spuštění aplikace hello a potom pomocí aplikace hello uživatelského rozhraní tooenter hello klíčů, tajné klíče a připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="cbac5-201">Run hello application, and then use hello application's UI tooenter hello keys, secrets, and connection string.</span></span> <span data-ttu-id="cbac5-202">Pokud to uděláte, informace o konfiguraci hello se používá pro aktuální relaci, ale nebyla uložena.</span><span class="sxs-lookup"><span data-stu-id="cbac5-202">If you do this, hello configuration information is used for your current session, but it isn't saved.</span></span>
* <span data-ttu-id="cbac5-203">Upravte soubor .config hello aplikace a hodnoty sady hello.</span><span class="sxs-lookup"><span data-stu-id="cbac5-203">Edit hello application's .config file and set hello values there.</span></span> <span data-ttu-id="cbac5-204">Tento přístup udržuje informace o konfiguraci hello, ale je také znamená, že tento potenciálně citlivé informace je uložena ve formátu prostého textu ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="cbac5-204">This approach persists hello configuration information, but it also means that this potentially sensitive information is stored in plain text on your computer.</span></span>

<span data-ttu-id="cbac5-205">Hello následující postup dokumenty obou přístupů.</span><span class="sxs-lookup"><span data-stu-id="cbac5-205">hello following procedure documents both approaches.</span></span> 

1. <span data-ttu-id="cbac5-206">Zkontrolujte, že jste stažené a rozbalené hello [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) aplikaci, jak je uvedeno v hello požadavky.</span><span class="sxs-lookup"><span data-stu-id="cbac5-206">Make sure you've downloaded and unzipped hello [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) application, as listed in hello prerequisites.</span></span>

2. <span data-ttu-id="cbac5-207">tooset hello hodnoty v době běhu (a pouze pro aktuální relaci hello), spusťte hello `TwitterWPFClient.exe` aplikace.</span><span class="sxs-lookup"><span data-stu-id="cbac5-207">tooset hello values at run time (and only for hello current session), run hello `TwitterWPFClient.exe` application.</span></span> <span data-ttu-id="cbac5-208">Pokud aplikace hello vás vyzve, zadejte hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="cbac5-208">When hello application prompts you, enter hello following values:</span></span>

    * <span data-ttu-id="cbac5-209">Hello Twitter uživatelský klíč (klíč rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="cbac5-209">hello Twitter Consumer Key (API Key).</span></span>
    * <span data-ttu-id="cbac5-210">Hello Twitter uživatelský tajný klíč (tajný klíč rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="cbac5-210">hello Twitter Consumer Secret (API Secret).</span></span>
    * <span data-ttu-id="cbac5-211">Hello Twitter tokenu přístupu.</span><span class="sxs-lookup"><span data-stu-id="cbac5-211">hello Twitter Access Token.</span></span>
    * <span data-ttu-id="cbac5-212">Hello Twitter tajný klíč přístupového tokenu.</span><span class="sxs-lookup"><span data-stu-id="cbac5-212">hello Twitter Access Token Secret.</span></span>
    * <span data-ttu-id="cbac5-213">Hello připojovací řetězec informace, které jste předtím uložili.</span><span class="sxs-lookup"><span data-stu-id="cbac5-213">hello connection string information that you saved earlier.</span></span> <span data-ttu-id="cbac5-214">Ujistěte se, že používáte hello připojovací řetězec, že jste odebrali hello `EntityPath` dvojice klíč hodnota z.</span><span class="sxs-lookup"><span data-stu-id="cbac5-214">Make sure that you use hello connection string that you removed hello `EntityPath` key-value pair from.</span></span>
    * <span data-ttu-id="cbac5-215">Hello Twitter klíčová slova, která chcete toodetermine postojích pro.</span><span class="sxs-lookup"><span data-stu-id="cbac5-215">hello Twitter keywords that you want toodetermine sentiment for.</span></span>

   ![TwitterWpfClient aplikace spuštěna, zobrazující zakryt nastavení](./media/stream-analytics-twitter-sentiment-analysis-trends/wpfclientlines.png)

3. <span data-ttu-id="cbac5-217">hodnoty hello tooset trvalé, pomocí textového editoru tooopen hello TwitterWpfClient.exe.config souboru.</span><span class="sxs-lookup"><span data-stu-id="cbac5-217">tooset hello values persistently, use a text editor tooopen hello TwitterWpfClient.exe.config file.</span></span> <span data-ttu-id="cbac5-218">Potom v hello `<appSettings>` elementu, to udělat:</span><span class="sxs-lookup"><span data-stu-id="cbac5-218">Then in hello `<appSettings>` element, do this:</span></span>

    * <span data-ttu-id="cbac5-219">Nastavit `oauth_consumer_key` toohello Twitter uživatelský klíč (klíč rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="cbac5-219">Set `oauth_consumer_key` toohello Twitter Consumer Key (API Key).</span></span> 
    * <span data-ttu-id="cbac5-220">Nastavit `oauth_consumer_secret` toohello Twitter uživatelský tajný klíč (tajný klíč rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="cbac5-220">Set `oauth_consumer_secret` toohello Twitter Consumer Secret (API Secret).</span></span>
    * <span data-ttu-id="cbac5-221">Nastavit `oauth_token` toohello Twitter tokenu přístupu.</span><span class="sxs-lookup"><span data-stu-id="cbac5-221">Set `oauth_token` toohello Twitter Access Token.</span></span>
    * <span data-ttu-id="cbac5-222">Nastavit `oauth_token_secret` toohello Twitter tajný klíč přístupového tokenu.</span><span class="sxs-lookup"><span data-stu-id="cbac5-222">Set `oauth_token_secret` toohello Twitter Access Token Secret.</span></span>

    <span data-ttu-id="cbac5-223">Dále v hello `<appSettings>` elementu, tyto změny:</span><span class="sxs-lookup"><span data-stu-id="cbac5-223">Later in hello `<appSettings>` element, make these changes:</span></span>

    * <span data-ttu-id="cbac5-224">Nastavit `EventHubName` toohello název centra událostí (tedy toohello hodnota hello entity cesty).</span><span class="sxs-lookup"><span data-stu-id="cbac5-224">Set `EventHubName` toohello event hub name (that is, toohello value of hello entity path).</span></span>
    * <span data-ttu-id="cbac5-225">Nastavit `EventHubNameConnectionString` toohello připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="cbac5-225">Set `EventHubNameConnectionString` toohello connection string.</span></span> <span data-ttu-id="cbac5-226">Ujistěte se, že používáte hello připojovací řetězec, že jste odebrali hello `EntityPath` dvojice klíč hodnota z.</span><span class="sxs-lookup"><span data-stu-id="cbac5-226">Make sure that you use hello connection string that you removed hello `EntityPath` key-value pair from.</span></span>

    <span data-ttu-id="cbac5-227">Hello `<appSettings>` části vypadá hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="cbac5-227">hello `<appSettings>` section looks like hello following example.</span></span> <span data-ttu-id="cbac5-228">(Pro přehlednost a zabezpečení, jsme zabalená některé řádky a odebrat některé znaky.)</span><span class="sxs-lookup"><span data-stu-id="cbac5-228">(For clarity and security, we wrapped some lines and removed some characters.)</span></span>

    ![Konfigurační soubor aplikace TwitterWpfClient v textovém editoru, zobrazující hello Twitter klíče a tajné klíče a informace o připojovacím řetězci hello události rozbočovače](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-tiwtter-app-config.png)
 
4. <span data-ttu-id="cbac5-230">Pokud již jste nezahájili hello aplikace, spusťte nyní TwitterWpfClient.exe.</span><span class="sxs-lookup"><span data-stu-id="cbac5-230">If you didn't already start hello application, run TwitterWpfClient.exe now.</span></span> 

5. <span data-ttu-id="cbac5-231">Klikněte na tlačítko hello zelená počáteční tlačítko toocollect sociálních postojích.</span><span class="sxs-lookup"><span data-stu-id="cbac5-231">Click hello green start button toocollect social sentiment.</span></span> <span data-ttu-id="cbac5-232">Zobrazí události Tweet s hello **CreatedAt**, **tématu**, a **SentimentScore** hodnoty odesílány tooyour centra událostí.</span><span class="sxs-lookup"><span data-stu-id="cbac5-232">You see Tweet events with hello **CreatedAt**, **Topic**, and **SentimentScore** values being sent tooyour event hub.</span></span>

    ![Spuštění aplikace TwitterWpfClient zobrazující seznam tweetů](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-app-listing.png)

    >[!NOTE]
    ><span data-ttu-id="cbac5-234">Pokud se zobrazí chyby a proud tweetů zobrazí v dolní části okna hello hello nevidíte, zkontrolujte hello klíče a tajné klíče.</span><span class="sxs-lookup"><span data-stu-id="cbac5-234">If you see errors, and you don't see a stream of tweets displayed in hello lower part of hello window, double-check hello keys and secrets.</span></span> <span data-ttu-id="cbac5-235">Také zkontrolujte připojovací řetězec hello (ujistěte se, že neobsahuje hello `EntityPath` klíče a hodnoty.)</span><span class="sxs-lookup"><span data-stu-id="cbac5-235">Also check hello connection string (make sure that it does not include hello `EntityPath` key and value.)</span></span>


## <a name="create-a-stream-analytics-job"></a><span data-ttu-id="cbac5-236">Vytvoření úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="cbac5-236">Create a Stream Analytics job</span></span>

<span data-ttu-id="cbac5-237">Teď, když jsou události tweet streamování v reálném čase z Twitteru, můžete nastavit až tooanalyze úlohy Stream Analytics tyto události v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="cbac5-237">Now that tweet events are streaming in real time from Twitter, you can set up a Stream Analytics job tooanalyze these events in real time.</span></span>

1. <span data-ttu-id="cbac5-238">V hello portálu Azure, klikněte na **nový** > **Internet věcí** > **úlohy služby Stream Analytics**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-238">In hello Azure portal, click **New** > **Internet of Things** > **Stream Analytics job**.</span></span>

2. <span data-ttu-id="cbac5-239">Název úlohy hello `socialtwitter-sa-job` a určete předplatné, skupinu prostředků a umístění.</span><span class="sxs-lookup"><span data-stu-id="cbac5-239">Name hello job `socialtwitter-sa-job` and specify a subscription, resource group, and location.</span></span>

    <span data-ttu-id="cbac5-240">Je vhodné tooplace hello úlohy a hello událostí rozbočovač v hello stejné oblasti pro nejlepší výkon a tak, že nemáte platíte tootransfer dat mezi oblastmi.</span><span class="sxs-lookup"><span data-stu-id="cbac5-240">It's a good idea tooplace hello job and hello event hub in hello same region for best performance and so that you don't pay tootransfer data between regions.</span></span>

    ![Vytvoření nové úlohy Stream Analytics](./media/stream-analytics-twitter-sentiment-analysis-trends/newjob.png)

3. <span data-ttu-id="cbac5-242">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-242">Click **Create**.</span></span>

    <span data-ttu-id="cbac5-243">bude vytvořena úloha Hello a hello portálu se zobrazí podrobnosti úlohy.</span><span class="sxs-lookup"><span data-stu-id="cbac5-243">hello job is created and hello portal displays job details.</span></span>


## <a name="specify-hello-job-input"></a><span data-ttu-id="cbac5-244">Zadejte vstup úlohy hello</span><span class="sxs-lookup"><span data-stu-id="cbac5-244">Specify hello job input</span></span>

1. <span data-ttu-id="cbac5-245">V úloze Stream Analytics v části **úlohy topologie** uprostřed hello hello okno úlohy, klikněte na tlačítko **vstupy**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-245">In your Stream Analytics job, under **Job Topology** in hello middle of hello job blade, click **Inputs**.</span></span> 

2. <span data-ttu-id="cbac5-246">V hello **vstupy** okně klikněte na tlačítko  **+ &nbsp;přidat** a potom vyplňte hello okno s těmito hodnotami:</span><span class="sxs-lookup"><span data-stu-id="cbac5-246">In hello **Inputs** blade, click **+&nbsp;Add** and then fill out hello blade with these values:</span></span>

    * <span data-ttu-id="cbac5-247">**Vstupní alias**: použití hello název `TwitterStream`.</span><span class="sxs-lookup"><span data-stu-id="cbac5-247">**Input alias**: Use hello name `TwitterStream`.</span></span> <span data-ttu-id="cbac5-248">Pokud použijete jiný název, poznamenejte si jeho protože ji budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="cbac5-248">If you use a different name, make a note of it because you need it later.</span></span>
    * <span data-ttu-id="cbac5-249">**Typ zdroje**: vyberte **datový proud**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-249">**Source type**: Select **Data stream**.</span></span>
    * <span data-ttu-id="cbac5-250">**Zdroj**: vyberte **centra událostí**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-250">**Source**: Select **Event hub**.</span></span>
    * <span data-ttu-id="cbac5-251">**Import možnost**: vyberte **použijte Centrum událostí z aktuálního předplatného**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-251">**Import option**: Select **Use event hub from current subscription**.</span></span> 
    * <span data-ttu-id="cbac5-252">**Obor názvů sběrnice**: Vyberte hello události rozbočovače názvů, který jste vytvořili dříve (`<yourname>-socialtwitter-eh-ns`).</span><span class="sxs-lookup"><span data-stu-id="cbac5-252">**Service bus namespace**: Select hello event hub namespace that you created earlier (`<yourname>-socialtwitter-eh-ns`).</span></span>
    * <span data-ttu-id="cbac5-253">**Centra událostí**: Vyberte hello centra událostí, který jste vytvořili dříve (`socialtwitter-eh`).</span><span class="sxs-lookup"><span data-stu-id="cbac5-253">**Event hub**: Select hello event hub that you created earlier (`socialtwitter-eh`).</span></span>
    * <span data-ttu-id="cbac5-254">**Název zásady centra událostí**: Vyberte hello zásady přístupu, kterou jste vytvořili dříve (`socialtwitter-access`).</span><span class="sxs-lookup"><span data-stu-id="cbac5-254">**Event hub policy name**: Select hello access policy that you created earlier (`socialtwitter-access`).</span></span>

    ![Vytvořit nový vstupní úlohy streamování Analytics](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-new-input.png)

3. <span data-ttu-id="cbac5-256">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-256">Click **Create**.</span></span>


## <a name="specify-hello-job-query"></a><span data-ttu-id="cbac5-257">Zadejte dotaz úlohy hello</span><span class="sxs-lookup"><span data-stu-id="cbac5-257">Specify hello job query</span></span>

<span data-ttu-id="cbac5-258">Stream Analytics podporuje jednoduchý a deklarativní dotazu model, který popisuje transformace.</span><span class="sxs-lookup"><span data-stu-id="cbac5-258">Stream Analytics supports a simple, declarative query model that describes transformations.</span></span> <span data-ttu-id="cbac5-259">toolearn Další informace o jazyce hello, najdete v části hello [referenční příručka Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx).</span><span class="sxs-lookup"><span data-stu-id="cbac5-259">toolearn more about hello language, see hello [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx).</span></span>  <span data-ttu-id="cbac5-260">Tento kurz vám pomůže vytvořit a vyzkoušet několik dotazů přes Twitter data.</span><span class="sxs-lookup"><span data-stu-id="cbac5-260">This tutorial helps you author and test several queries over Twitter data.</span></span>

<span data-ttu-id="cbac5-261">toocompare hello počet zmínkami mezi témata, můžete použít [Přeskakující okno](https://msdn.microsoft.com/library/azure/dn835055.aspx) tooget hello počet zmínkami podle tématu každých pět sekund.</span><span class="sxs-lookup"><span data-stu-id="cbac5-261">toocompare hello number of mentions among topics, you can use a [Tumbling window](https://msdn.microsoft.com/library/azure/dn835055.aspx) tooget hello count of mentions by topic every five seconds.</span></span>

1. <span data-ttu-id="cbac5-262">Zavřít hello **vstupy** okno, pokud jste tak ještě neučinili.</span><span class="sxs-lookup"><span data-stu-id="cbac5-262">Close hello **Inputs** blade if you haven't already.</span></span>

2. <span data-ttu-id="cbac5-263">V okně úlohy hello, klikněte na tlačítko hello **dotazu** pole.</span><span class="sxs-lookup"><span data-stu-id="cbac5-263">In hello job blade, click hello **Query** box.</span></span> <span data-ttu-id="cbac5-264">Azure obsahuje hello vstupy a výstupy, které jsou nakonfigurované pro hello úlohy a umožňuje vám vytvořit dotaz, který umožňuje transformovat vstupního datového proudu hello při přenosu toohello výstup.</span><span class="sxs-lookup"><span data-stu-id="cbac5-264">Azure lists hello inputs and outputs that are configured for hello job, and lets you create a query that lets you transform hello input stream as it is sent toohello output.</span></span>

3. <span data-ttu-id="cbac5-265">Ujistěte se, že tento hello TwitterWpfClient aplikace běží.</span><span class="sxs-lookup"><span data-stu-id="cbac5-265">Make sure that hello TwitterWpfClient application is running.</span></span> 

3. <span data-ttu-id="cbac5-266">V hello **dotazu** okně klikněte na tlačítko Další toohello hello tečky `TwitterStream` vstup a pak vyberte **vzorová data ze vstupu**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-266">In hello **Query** blade, click hello dots next toohello `TwitterStream` input and then select **Sample data from input**.</span></span>

    ![Nabídka Možnosti toouse ukázková data pro hello položka se "Ukázkových dat ze vstupu" vybrané úlohy streamování Analytics](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-sample-data-from-input.png)

    <span data-ttu-id="cbac5-268">Otevře se okno, které umožňuje určit, kolik tooget ukázková data, definována v tom, jak dlouho tooread hello vstupní datový proud.</span><span class="sxs-lookup"><span data-stu-id="cbac5-268">This opens a blade that lets you specify how much sample data tooget, defined in terms of how long tooread hello input stream.</span></span>

4. <span data-ttu-id="cbac5-269">Nastavit **minut** too3 a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-269">Set **Minutes** too3 and then click **OK**.</span></span> 
    
    ![Možnosti vzorkování hello vstupního datového proudu, s vybrané "3 minut".](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-input-create-sample-data.png)

    <span data-ttu-id="cbac5-271">Azure ukázky data z vstupního datového proudu hello za 3 minuty a vás upozorní, jakmile je připraven hello ukázková data.</span><span class="sxs-lookup"><span data-stu-id="cbac5-271">Azure samples 3 minutes' worth of data from hello input stream and notifies you when hello sample data is ready.</span></span> <span data-ttu-id="cbac5-272">(To trvá nějakou dobu.)</span><span class="sxs-lookup"><span data-stu-id="cbac5-272">(This takes a short while.)</span></span> 

    <span data-ttu-id="cbac5-273">Hello ukázková data jsou ukládána dočasně a je k dispozici, když máte otevřete okno dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="cbac5-273">hello sample data is stored temporarily and is available while you have hello query window open.</span></span> <span data-ttu-id="cbac5-274">Pokud zavřete okno dotazu hello hello ukázková data zahozena a máte toocreate novou sadu ukázková data.</span><span class="sxs-lookup"><span data-stu-id="cbac5-274">If you close hello query window, hello sample data is discarded, and you have toocreate a new set of sample data.</span></span> 

5. <span data-ttu-id="cbac5-275">Změna dotazu hello v hello kód editor toohello následující:</span><span class="sxs-lookup"><span data-stu-id="cbac5-275">Change hello query in hello code editor toohello following:</span></span>

    ```
    SELECT System.Timestamp as Time, Topic, COUNT(*)
    FROM TwitterStream TIMESTAMP BY CreatedAt
    GROUP BY TUMBLINGWINDOW(s, 5), Topic
    ```

    <span data-ttu-id="cbac5-276">Pokud nepoužili `TwitterStream` jako hello alias pro vstup hello, nahraďte váš alias pro `TwitterStream` v dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="cbac5-276">If didn't use `TwitterStream` as hello alias for hello input, substitute your alias for `TwitterStream` in hello query.</span></span>  

    <span data-ttu-id="cbac5-277">Tento dotaz používá hello **TIMESTAMP BY** – klíčové slovo toospecify pole časového razítka v toobe datové části hello používá v dočasné výpočty hello.</span><span class="sxs-lookup"><span data-stu-id="cbac5-277">This query uses hello **TIMESTAMP BY** keyword toospecify a timestamp field in hello payload toobe used in hello temporal computation.</span></span> <span data-ttu-id="cbac5-278">Pokud toto pole nezadáte, hello oddílová operace pomocí hello dobu, po kterou všechny události byly přijaty hello centra událostí.</span><span class="sxs-lookup"><span data-stu-id="cbac5-278">If this field isn't specified, hello windowing operation is performed by using hello time that each event arrived at hello event hub.</span></span> <span data-ttu-id="cbac5-279">Další informace najdete v části hello "čas doručení vs doba aplikace" [referenční příručka Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx).</span><span class="sxs-lookup"><span data-stu-id="cbac5-279">Learn more in hello "Arrival Time vs Application Time" section of [Stream Analytics Query Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx).</span></span>

    <span data-ttu-id="cbac5-280">Tento dotaz také přistoupí časové razítko pro hello konec každé okno pomocí hello **System.Timestamp** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="cbac5-280">This query also accesses a timestamp for hello end of each window by using hello **System.Timestamp** property.</span></span>

5. <span data-ttu-id="cbac5-281">Klikněte na tlačítko **Test**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-281">Click **Test**.</span></span> <span data-ttu-id="cbac5-282">Spustí se dotaz Hello hello data, která jste vzorků.</span><span class="sxs-lookup"><span data-stu-id="cbac5-282">hello query runs against hello data that you sampled.</span></span>
    
6. <span data-ttu-id="cbac5-283">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-283">Click **Save**.</span></span> <span data-ttu-id="cbac5-284">To umožňuje ušetřit hello dotaz jako součást úlohy streamování Analytics hello.</span><span class="sxs-lookup"><span data-stu-id="cbac5-284">This saves hello query as part of hello Streaming Analytics job.</span></span> <span data-ttu-id="cbac5-285">(Ho nebude uložen hello ukázková data.)</span><span class="sxs-lookup"><span data-stu-id="cbac5-285">(It doesn't save hello sample data.)</span></span>


## <a name="experiment-using-different-fields-from-hello-stream"></a><span data-ttu-id="cbac5-286">Experiment pomocí různých polí z datového proudu hello</span><span class="sxs-lookup"><span data-stu-id="cbac5-286">Experiment using different fields from hello stream</span></span> 

<span data-ttu-id="cbac5-287">Hello následující tabulka uvádí hello pole, které jsou součástí hello streamování data JSON.</span><span class="sxs-lookup"><span data-stu-id="cbac5-287">hello following table lists hello fields that are part of hello JSON streaming data.</span></span> <span data-ttu-id="cbac5-288">Zaregistrované volné tooexperiment v editoru dotazů hello.</span><span class="sxs-lookup"><span data-stu-id="cbac5-288">Feel free tooexperiment in hello query editor.</span></span>

|<span data-ttu-id="cbac5-289">Vlastnost JSON</span><span class="sxs-lookup"><span data-stu-id="cbac5-289">JSON property</span></span> | <span data-ttu-id="cbac5-290">Definice</span><span class="sxs-lookup"><span data-stu-id="cbac5-290">Definition</span></span>|
|--- | ---|
|<span data-ttu-id="cbac5-291">CreatedAt</span><span class="sxs-lookup"><span data-stu-id="cbac5-291">CreatedAt</span></span> | <span data-ttu-id="cbac5-292">čas Hello byl vytvořen tento tweet hello</span><span class="sxs-lookup"><span data-stu-id="cbac5-292">hello time that hello tweet was created</span></span>|
|<span data-ttu-id="cbac5-293">Téma</span><span class="sxs-lookup"><span data-stu-id="cbac5-293">Topic</span></span> | <span data-ttu-id="cbac5-294">Hello téma, které odpovídá hello zadané – klíčové slovo</span><span class="sxs-lookup"><span data-stu-id="cbac5-294">hello topic that matches hello specified keyword</span></span>|
|<span data-ttu-id="cbac5-295">SentimentScore</span><span class="sxs-lookup"><span data-stu-id="cbac5-295">SentimentScore</span></span> | <span data-ttu-id="cbac5-296">Hello postojích skóre z Sentiment140</span><span class="sxs-lookup"><span data-stu-id="cbac5-296">hello sentiment score from Sentiment140</span></span>|
|<span data-ttu-id="cbac5-297">Autor</span><span class="sxs-lookup"><span data-stu-id="cbac5-297">Author</span></span> | <span data-ttu-id="cbac5-298">Popisovač Hello Twitter, odeslaný hello tweet</span><span class="sxs-lookup"><span data-stu-id="cbac5-298">hello Twitter handle that sent hello tweet</span></span>|
|<span data-ttu-id="cbac5-299">Text</span><span class="sxs-lookup"><span data-stu-id="cbac5-299">Text</span></span> | <span data-ttu-id="cbac5-300">úplné tělo Hello hello tweet</span><span class="sxs-lookup"><span data-stu-id="cbac5-300">hello full body of hello tweet</span></span>|


## <a name="create-an-output-sink"></a><span data-ttu-id="cbac5-301">Vytvoření výstupní jímku</span><span class="sxs-lookup"><span data-stu-id="cbac5-301">Create an output sink</span></span>

<span data-ttu-id="cbac5-302">Nyní jste definovali datového proudu událostí, události vstupní tooingest centra událostí a dotaz tooperform transformace přes hello datového proudu.</span><span class="sxs-lookup"><span data-stu-id="cbac5-302">You have now defined an event stream, an event hub input tooingest events, and a query tooperform a transformation over hello stream.</span></span> <span data-ttu-id="cbac5-303">posledním krokem Hello je toodefine výstupní jímku pro úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="cbac5-303">hello last step is toodefine an output sink for hello job.</span></span>  

<span data-ttu-id="cbac5-304">V tomto kurzu zapsat hello agregovat tweet události z hello úlohy dotazu tooAzure úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="cbac5-304">In this tutorial, you write hello aggregated tweet events from hello job query tooAzure Blob storage.</span></span>  <span data-ttu-id="cbac5-305">Můžete také nabízené vaše výsledky tooAzure databáze SQL Azure Table storage, Event Hubs, nebo Power BI, v závislosti na vaší aplikace potřebuje.</span><span class="sxs-lookup"><span data-stu-id="cbac5-305">You can also push your results tooAzure SQL Database, Azure Table storage, Event Hubs, or Power BI, depending on your application needs.</span></span>

## <a name="specify-hello-job-output"></a><span data-ttu-id="cbac5-306">Zadejte hello výstup úlohy</span><span class="sxs-lookup"><span data-stu-id="cbac5-306">Specify hello job output</span></span>

1. <span data-ttu-id="cbac5-307">V hello **úlohy topologie** klikněte na tlačítko hello **výstup** pole.</span><span class="sxs-lookup"><span data-stu-id="cbac5-307">In hello **Job Topology** section, click hello **Output** box.</span></span> 

2. <span data-ttu-id="cbac5-308">V hello **výstupy** okně klikněte na tlačítko  **+ &nbsp;přidat** a potom vyplňte hello okno s těmito hodnotami:</span><span class="sxs-lookup"><span data-stu-id="cbac5-308">In hello **Outputs** blade, click **+&nbsp;Add** and then fill out hello blade with these values:</span></span>

    * <span data-ttu-id="cbac5-309">**Alias pro výstup**: použití hello název `TwitterStream-Output`.</span><span class="sxs-lookup"><span data-stu-id="cbac5-309">**Output alias**: Use hello name `TwitterStream-Output`.</span></span> 
    * <span data-ttu-id="cbac5-310">**Jímky**: vyberte **úložiště objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-310">**Sink**: Select **Blob storage**.</span></span>
    * <span data-ttu-id="cbac5-311">**Možnosti importu**: vyberte **pomocí úložiště objektů blob z aktuálního předplatného**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-311">**Import options**: Select **Use blob storage from current subscription**.</span></span>
    * <span data-ttu-id="cbac5-312">**Účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-312">**Storage account**.</span></span> <span data-ttu-id="cbac5-313">Vyberte **vytvořit nový účet úložiště.**</span><span class="sxs-lookup"><span data-stu-id="cbac5-313">Select **Create a new storage account.**</span></span>
    * <span data-ttu-id="cbac5-314">**Účet úložiště** (druhé pole).</span><span class="sxs-lookup"><span data-stu-id="cbac5-314">**Storage account** (second box).</span></span> <span data-ttu-id="cbac5-315">Zadejte `YOURNAMEsa`, kde `YOURNAME` je název vaší nebo jiné jedinečné řetězce.</span><span class="sxs-lookup"><span data-stu-id="cbac5-315">Enter `YOURNAMEsa`, where `YOURNAME` is your name or another unique string.</span></span> <span data-ttu-id="cbac5-316">v názvu Hello lze použít pouze malá písmena a čísla a musí být jedinečný v Azure.</span><span class="sxs-lookup"><span data-stu-id="cbac5-316">hello name can use only lowercase letters and numbers, and it must be unique across Azure.</span></span> 
    * <span data-ttu-id="cbac5-317">**Kontejner**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-317">**Container**.</span></span> <span data-ttu-id="cbac5-318">Zadejte `socialtwitter`.</span><span class="sxs-lookup"><span data-stu-id="cbac5-318">Enter `socialtwitter`.</span></span>
    <span data-ttu-id="cbac5-319">název účtu úložiště Hello a název kontejneru jsou používané společně tooprovide identifikátor URI pro úložiště objektů blob hello, například takto:</span><span class="sxs-lookup"><span data-stu-id="cbac5-319">hello storage account name and container name are used together tooprovide a URI for hello blob storage, like this:</span></span> 

    `http://YOURNAMEsa.blob.core.windows.net/socialtwitter/...`
    
    !["Nové výstup" okno úlohy Stream Analytics](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-output-blob-storage.png)
    
4. <span data-ttu-id="cbac5-321">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-321">Click **Create**.</span></span> 

    <span data-ttu-id="cbac5-322">Azure vytvoří účet úložiště hello a automaticky vygeneruje klíč.</span><span class="sxs-lookup"><span data-stu-id="cbac5-322">Azure creates hello storage account and generates a key automatically.</span></span> 

5. <span data-ttu-id="cbac5-323">Zavřít hello **výstupy** okno.</span><span class="sxs-lookup"><span data-stu-id="cbac5-323">Close hello **Outputs** blade.</span></span> 


## <a name="start-hello-job"></a><span data-ttu-id="cbac5-324">Spuštění úlohy hello</span><span class="sxs-lookup"><span data-stu-id="cbac5-324">Start hello job</span></span>

<span data-ttu-id="cbac5-325">Úloha vstup, dotaz a výstup nejsou zadány.</span><span class="sxs-lookup"><span data-stu-id="cbac5-325">A job input, query, and output are specified.</span></span> <span data-ttu-id="cbac5-326">Jste úlohy služby Stream Analytics připravené toostart hello.</span><span class="sxs-lookup"><span data-stu-id="cbac5-326">You are ready toostart hello Stream Analytics job.</span></span>

1. <span data-ttu-id="cbac5-327">Ujistěte se, že tento hello TwitterWpfClient aplikace běží.</span><span class="sxs-lookup"><span data-stu-id="cbac5-327">Make sure that hello TwitterWpfClient application is running.</span></span> 

2. <span data-ttu-id="cbac5-328">V okně úlohy hello, klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-328">In hello job blade, click **Start**.</span></span>

    ![Spuštění úlohy Stream Analytics hello](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sa-job-start-output.png)

3. <span data-ttu-id="cbac5-330">V hello **spuštění úlohy** okně pro **výstup úlohy počáteční čas**, vyberte **nyní** a pak klikněte na **spustit**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-330">In hello **Start job** blade, for **Job output start time**, select **Now** and then click **Start**.</span></span> 

    !["Spuštění úlohy" okno úlohy Stream Analytics hello](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sa-job-start-job-blade.png)

    <span data-ttu-id="cbac5-332">Azure vás upozorní, jakmile byla spuštěna úloha hello a v okně úlohy hello hello stav se zobrazí jako **systémem**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-332">Azure notifies you when hello job has started, and in hello job blade, hello status is displayed as **Running**.</span></span>

    ![Spuštěná úloha](./media/stream-analytics-twitter-sentiment-analysis-trends/jobrunning.png)

## <a name="view-output-for-sentiment-analysis"></a><span data-ttu-id="cbac5-334">Zobrazit výstup pro analýzu postojích</span><span class="sxs-lookup"><span data-stu-id="cbac5-334">View output for sentiment analysis</span></span>

<span data-ttu-id="cbac5-335">Po zahájení spuštěná vaše úloha zpracovává datový proud v reálném čase pro Twitter hello, můžete zobrazit výstup hello postojích analýzy.</span><span class="sxs-lookup"><span data-stu-id="cbac5-335">After your job has started running and is processing hello real-time Twitter stream, you can view hello output for sentiment analysis.</span></span>

<span data-ttu-id="cbac5-336">Můžete použít nástroje, jako je [Azure Storage Explorer](https://http://storageexplorer.com/) nebo [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) tooview úlohu výstup v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="cbac5-336">You can use a tool like [Azure Storage Explorer](https://http://storageexplorer.com/) or [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) tooview your job output in real time.</span></span> <span data-ttu-id="cbac5-337">Tady můžete použít [Power BI](https://powerbi.com/) tooextend vaší aplikace tooinclude přizpůsobený řídicí panel, jako ukazuje následující snímek obrazovky hello hello:</span><span class="sxs-lookup"><span data-stu-id="cbac5-337">From here, you can use [Power BI](https://powerbi.com/) tooextend your application tooinclude a customized dashboard like hello one shown in hello following screenshot:</span></span>

![Power BI](./media/stream-analytics-twitter-sentiment-analysis-trends/power-bi.png)


## <a name="create-another-query-tooidentify-trending-topics"></a><span data-ttu-id="cbac5-339">Vytvořte další dotaz tooidentify trendů témata</span><span class="sxs-lookup"><span data-stu-id="cbac5-339">Create another query tooidentify trending topics</span></span>

<span data-ttu-id="cbac5-340">Další dotaz můžete použít sentimentu Twitter toounderstand vychází [posuvné okno](https://msdn.microsoft.com/library/azure/dn835051.aspx).</span><span class="sxs-lookup"><span data-stu-id="cbac5-340">Another query you can use toounderstand Twitter sentiment is based on a [Sliding Window](https://msdn.microsoft.com/library/azure/dn835051.aspx).</span></span> <span data-ttu-id="cbac5-341">témata tooidentify trendů, můžete hledat témata, které zasahují prahová hodnota pro zmínkami ve stanoveném čase.</span><span class="sxs-lookup"><span data-stu-id="cbac5-341">tooidentify trending topics, you look for topics that cross a threshold value for mentions in a specified amount of time.</span></span>

<span data-ttu-id="cbac5-342">Pro účely tohoto kurzu hello vyhledejte témata, která jsou uveden více než 20 výskyty v hello posledních 5 sekund.</span><span class="sxs-lookup"><span data-stu-id="cbac5-342">For hello purposes of this tutorial, you check for topics that are mentioned more than 20 times in hello last 5 seconds.</span></span>

1. <span data-ttu-id="cbac5-343">V okně úlohy hello, klikněte na tlačítko **Zastavit** toostop hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="cbac5-343">In hello job blade, click **Stop** toostop hello job.</span></span> 

2. <span data-ttu-id="cbac5-344">V hello **úlohy topologie** klikněte na tlačítko hello **dotazu** pole.</span><span class="sxs-lookup"><span data-stu-id="cbac5-344">In hello **Job Topology** section, click hello **Query** box.</span></span> 

3. <span data-ttu-id="cbac5-345">Změna hello dotazu toohello následující:</span><span class="sxs-lookup"><span data-stu-id="cbac5-345">Change hello query toohello following:</span></span>

    ```    
    SELECT System.Timestamp as Time, Topic, COUNT(*) as Mentions
    FROM TwitterStream TIMESTAMP BY CreatedAt
    GROUP BY SLIDINGWINDOW(s, 5), topic
    HAVING COUNT(*) > 20
    ```

4. <span data-ttu-id="cbac5-346">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="cbac5-346">Click **Save**.</span></span>

5. <span data-ttu-id="cbac5-347">Ujistěte se, že tento hello TwitterWpfClient aplikace běží.</span><span class="sxs-lookup"><span data-stu-id="cbac5-347">Make sure that hello TwitterWpfClient application is running.</span></span> 

6. <span data-ttu-id="cbac5-348">Klikněte na tlačítko **spustit** toorestart hello úlohy pomocí hello nový dotaz.</span><span class="sxs-lookup"><span data-stu-id="cbac5-348">Click **Start** toorestart hello job using hello new query.</span></span>


## <a name="get-support"></a><span data-ttu-id="cbac5-349">Získat podporu</span><span class="sxs-lookup"><span data-stu-id="cbac5-349">Get support</span></span>
<span data-ttu-id="cbac5-350">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="cbac5-350">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="cbac5-351">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cbac5-351">Next steps</span></span>
* [<span data-ttu-id="cbac5-352">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="cbac5-352">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="cbac5-353">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="cbac5-353">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="cbac5-354">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="cbac5-354">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="cbac5-355">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="cbac5-355">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="cbac5-356">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="cbac5-356">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
