---
title: "aaaCreate funkci, která se integruje se službou Azure Logic Apps | Microsoft Docs"
description: "Vytvoří funkci, která se integruje se službou Azure Logic Apps a kognitivní služby Azure postojích toocategorize tweet a odesílání oznámení, když postojích je nízký."
services: functions, logic-apps, cognitive-services
keywords: "pracovní postup, cloudové aplikace, cloudové služby, firemní procesy, systémová integrace, integrace podnikových aplikací, enterprise application integration, EAI"
documentationcenter: 
author: ggailey777
manager: erikre
editor: 
ms.assetid: 60495cc5-1638-4bf0-8174-52786d227734
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: e176bd946af9a3684b3ad0e4b1bed1c3ee344019
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-that-integrates-with-azure-logic-apps"></a><span data-ttu-id="bf004-104">Vytvoří funkci, která se integruje se službou Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="bf004-104">Create a function that integrates with Azure Logic Apps</span></span>

<span data-ttu-id="bf004-105">Azure Functions se integruje s Azure Logic Apps v hello návrhář aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="bf004-105">Azure Functions integrates with Azure Logic Apps in hello Logic Apps Designer.</span></span> <span data-ttu-id="bf004-106">Tato integrace umožňuje použít hello výpočetní výkon funkcí v orchestrations s jinými Azure a služby třetích stran.</span><span class="sxs-lookup"><span data-stu-id="bf004-106">This integration lets you use hello computing power of Functions in orchestrations with other Azure and third-party services.</span></span> 

<span data-ttu-id="bf004-107">Tento kurz ukazuje, jak funguje toouse s Logic Apps a kognitivní služby Azure postojích tooanalyze z příspěvky služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="bf004-107">This tutorial shows you how toouse Functions with Logic Apps and Azure Cognitive Services tooanalyze sentiment from Twitter posts.</span></span> <span data-ttu-id="bf004-108">Funkce protokolu HTTP aktivované rozděluje tweetů jako zelené, žluté nebo červené podle skóre postojích hello.</span><span class="sxs-lookup"><span data-stu-id="bf004-108">An HTTP triggered function categorizes tweets as green, yellow, or red based on hello sentiment score.</span></span> <span data-ttu-id="bf004-109">E-mail je odeslán, když je zjištěna nízký postojích.</span><span class="sxs-lookup"><span data-stu-id="bf004-109">An email is sent when poor sentiment is detected.</span></span> 

![první dva kroky Image aplikace v návrháři aplikace logiky](media/functions-twitter-email/designer1.png)

<span data-ttu-id="bf004-111">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="bf004-111">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bf004-112">Vytvoření účtu kognitivní Services.</span><span class="sxs-lookup"><span data-stu-id="bf004-112">Create a Cognitive Services account.</span></span>
> * <span data-ttu-id="bf004-113">Vytvoří funkci, která rozděluje tweet postojích.</span><span class="sxs-lookup"><span data-stu-id="bf004-113">Create a function that categorizes tweet sentiment.</span></span>
> * <span data-ttu-id="bf004-114">Vytvoření aplikace logiky, která se připojuje tooTwitter.</span><span class="sxs-lookup"><span data-stu-id="bf004-114">Create a logic app that connects tooTwitter.</span></span>
> * <span data-ttu-id="bf004-115">Přidáte aplikaci logiky toohello postojích detekce.</span><span class="sxs-lookup"><span data-stu-id="bf004-115">Add sentiment detection toohello logic app.</span></span> 
> * <span data-ttu-id="bf004-116">Připojte hello logiku aplikace toohello funkce.</span><span class="sxs-lookup"><span data-stu-id="bf004-116">Connect hello logic app toohello function.</span></span>
> * <span data-ttu-id="bf004-117">Odešlete e-mail podle hello odpověď z funkce hello.</span><span class="sxs-lookup"><span data-stu-id="bf004-117">Send an email based on hello response from hello function.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bf004-118">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bf004-118">Prerequisites</span></span>

+ <span data-ttu-id="bf004-119">Aktivní [Twitter](https://twitter.com/) účtu.</span><span class="sxs-lookup"><span data-stu-id="bf004-119">An active [Twitter](https://twitter.com/) account.</span></span> 
+ <span data-ttu-id="bf004-120">[Outlook.com](https://outlook.com/) účet (pro posílání oznámení).</span><span class="sxs-lookup"><span data-stu-id="bf004-120">An [Outlook.com](https://outlook.com/) account (for sending notifications).</span></span>
+ <span data-ttu-id="bf004-121">Toto téma se používá jako jeho výchozí prostředky hello bodu vytvořené v [vytvořit svoji první funkci z portálu Azure hello](functions-create-first-azure-function.md).</span><span class="sxs-lookup"><span data-stu-id="bf004-121">This topic uses as its starting point hello resources created in [Create your first function from hello Azure portal](functions-create-first-azure-function.md).</span></span>  
<span data-ttu-id="bf004-122">Pokud jste tak již neučinili, dokončete tyto kroky nyní toocreate funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="bf004-122">If you haven't already done so, complete these steps now toocreate your function app.</span></span>

## <a name="create-a-cognitive-services-account"></a><span data-ttu-id="bf004-123">Vytvoření účtu kognitivní Services</span><span class="sxs-lookup"><span data-stu-id="bf004-123">Create a Cognitive Services account</span></span>

<span data-ttu-id="bf004-124">Účet kognitivní Services je postojích hello požadované toodetect z tweetů monitorovány.</span><span class="sxs-lookup"><span data-stu-id="bf004-124">A Cognitive Services account is required toodetect hello sentiment of tweets being monitored.</span></span>

1. <span data-ttu-id="bf004-125">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="bf004-125">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>

2. <span data-ttu-id="bf004-126">Klikněte na tlačítko hello **nový** nalezeno tlačítko na hello levém horním rohu hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bf004-126">Click hello **New** button found on hello upper left-hand corner of hello Azure portal.</span></span>

3. <span data-ttu-id="bf004-127">Klikněte na tlačítko **Data + analýzy** > **kognitivní služby**.</span><span class="sxs-lookup"><span data-stu-id="bf004-127">Click **Data + Analytics** > **Cognitive  Services**.</span></span> <span data-ttu-id="bf004-128">Pak použít nastavení hello jako zadaný v tabulce hello, přijměte podmínky hello a zkontrolujte **Pin toodashboard**.</span><span class="sxs-lookup"><span data-stu-id="bf004-128">Then, use hello settings as specified in hello table, accept hello terms, and check **Pin toodashboard**.</span></span>

    ![Vytvoření okna kognitivní účtu](media/functions-twitter-email/cog_svcs_account.png)

    | <span data-ttu-id="bf004-130">Nastavení</span><span class="sxs-lookup"><span data-stu-id="bf004-130">Setting</span></span>      |  <span data-ttu-id="bf004-131">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="bf004-131">Suggested value</span></span>   | <span data-ttu-id="bf004-132">Popis</span><span class="sxs-lookup"><span data-stu-id="bf004-132">Description</span></span>                                        |
    | --- | --- | --- |
    | <span data-ttu-id="bf004-133">**Název**</span><span class="sxs-lookup"><span data-stu-id="bf004-133">**Name**</span></span> | <span data-ttu-id="bf004-134">MyCognitiveServicesAccnt</span><span class="sxs-lookup"><span data-stu-id="bf004-134">MyCognitiveServicesAccnt</span></span> | <span data-ttu-id="bf004-135">Zvolte název jedinečný účet.</span><span class="sxs-lookup"><span data-stu-id="bf004-135">Choose a unique account name.</span></span> |
    | <span data-ttu-id="bf004-136">**Typ rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="bf004-136">**API type**</span></span> | <span data-ttu-id="bf004-137">Rozhraní Text Analytics API</span><span class="sxs-lookup"><span data-stu-id="bf004-137">Text Analytics API</span></span> | <span data-ttu-id="bf004-138">Rozhraní API používá tooanalyze text.</span><span class="sxs-lookup"><span data-stu-id="bf004-138">API used tooanalyze text.</span></span>  |
    | <span data-ttu-id="bf004-139">**Umístění**</span><span class="sxs-lookup"><span data-stu-id="bf004-139">**Location**</span></span> | <span data-ttu-id="bf004-140">Západní USA</span><span class="sxs-lookup"><span data-stu-id="bf004-140">West US</span></span> | <span data-ttu-id="bf004-141">V současné době pouze **západní USA** je k dispozici pro Analýza textu.</span><span class="sxs-lookup"><span data-stu-id="bf004-141">Currently, only **West US** is available for text analytics.</span></span> |
    | <span data-ttu-id="bf004-142">**Cenová úroveň**</span><span class="sxs-lookup"><span data-stu-id="bf004-142">**Pricing tier**</span></span> | <span data-ttu-id="bf004-143">F0</span><span class="sxs-lookup"><span data-stu-id="bf004-143">F0</span></span> | <span data-ttu-id="bf004-144">Začněte s nejnižší úroveň hello.</span><span class="sxs-lookup"><span data-stu-id="bf004-144">Start with hello lowest tier.</span></span> <span data-ttu-id="bf004-145">Pokud spustíte z volání, škálovat tooa vyšší úroveň.</span><span class="sxs-lookup"><span data-stu-id="bf004-145">If you run out of calls, scale tooa higher tier.</span></span>|
    | <span data-ttu-id="bf004-146">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="bf004-146">**Resource group**</span></span> | <span data-ttu-id="bf004-147">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="bf004-147">myResourceGroup</span></span> | <span data-ttu-id="bf004-148">Použití hello stejnou skupinu prostředků pro všechny služby v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="bf004-148">Use hello same resource group for all services in this tutorial.</span></span>|

4. <span data-ttu-id="bf004-149">Klikněte na tlačítko **vytvořit** toocreate váš účet.</span><span class="sxs-lookup"><span data-stu-id="bf004-149">Click **Create** toocreate your account.</span></span> <span data-ttu-id="bf004-150">Po vytvoření účtu hello, klikněte na tlačítko nový řídicí panel kognitivní služby definovaného toohello účtu.</span><span class="sxs-lookup"><span data-stu-id="bf004-150">After hello account is created, click your new Cognitive Services account pinned toohello dashboard.</span></span> 

5. <span data-ttu-id="bf004-151">V hello účet, klikněte na **klíče**a poté zkopírujte hodnotu hello **klíč 1** a uložte ho.</span><span class="sxs-lookup"><span data-stu-id="bf004-151">In hello account, click **Keys**, and then copy hello value of **Key 1** and save it.</span></span> <span data-ttu-id="bf004-152">Můžete použít tento klíč tooconnect hello logiku aplikace tooyour účtu kognitivní Services.</span><span class="sxs-lookup"><span data-stu-id="bf004-152">You use this key tooconnect hello logic app tooyour Cognitive Services account.</span></span> 
 
    ![Klíče](media/functions-twitter-email/keys.png)

## <a name="create-hello-function"></a><span data-ttu-id="bf004-154">Vytvoření funkce hello</span><span class="sxs-lookup"><span data-stu-id="bf004-154">Create hello function</span></span>

<span data-ttu-id="bf004-155">Funkce nabízí skvělý způsob toooffload úloh zpracování v pracovním postupu logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="bf004-155">Functions provides a great way toooffload processing tasks in a logic apps workflow.</span></span> <span data-ttu-id="bf004-156">Tento kurz používá protokolu HTTP aktivované funkce tooprocess tweet postojích skóre z kognitivní služeb a vrátí hodnotu kategorie.</span><span class="sxs-lookup"><span data-stu-id="bf004-156">This tutorial uses an HTTP triggered function tooprocess tweet sentiment scores from Cognitive Services and return a category value.</span></span>  

1. <span data-ttu-id="bf004-157">Rozbalte funkce aplikace, klikněte na tlačítko hello  **+**  tlačítko vedle příliš**funkce**, klikněte na tlačítko hello **HTTPTrigger** šablony.</span><span class="sxs-lookup"><span data-stu-id="bf004-157">Expand your function app, click hello **+** button next too**Functions**, click hello **HTTPTrigger** template.</span></span> <span data-ttu-id="bf004-158">Typ `CategorizeSentiment` pro funkci hello **název** a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="bf004-158">Type `CategorizeSentiment` for hello function **Name** and click **Create**.</span></span>

    ![Okno aplikace funkce, funkce +](media/functions-twitter-email/add_fun.png)

2. <span data-ttu-id="bf004-160">Nahraďte hello obsah souboru run.csx hello hello následující kód a potom klikněte na **Uložit**:</span><span class="sxs-lookup"><span data-stu-id="bf004-160">Replace hello contents of hello run.csx file with hello following code, then click **Save**:</span></span>

    ```c#
    using System.Net;
    
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        // hello sentiment category defaults too'GREEN'. 
        string category = "GREEN";
    
        // Get hello sentiment score from hello request body.
        double score = await req.Content.ReadAsAsync<double>();
        log.Info(string.Format("hello sentiment score received is '{0}'.",
                    score.ToString()));
    
        // Set hello category based on hello sentiment score.
        if (score < .3)
        {
            category = "RED";
        }
        else if (score < .6)
        {
            category = "YELLOW";
        }
        return req.CreateResponse(HttpStatusCode.OK, category);
    }
    ```
    <span data-ttu-id="bf004-161">Tento kód funkce vrátí barevné kategorie podle skóre postojích hello dostali hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="bf004-161">This function code returns a color category based on hello sentiment score received in hello request.</span></span> 

3. <span data-ttu-id="bf004-162">Funkce hello tootest, klikněte na tlačítko **Test** v karta hello nejvíce vpravo tooexpand hello testu. Zadejte hodnotu `0.2` pro hello **text žádosti**a potom klikněte na **spustit**.</span><span class="sxs-lookup"><span data-stu-id="bf004-162">tootest hello function, click **Test** at hello far right tooexpand hello Test tab. Type a value of `0.2` for hello **Request body**, and then click **Run**.</span></span> <span data-ttu-id="bf004-163">Hodnota **RED** je vrácený v textu hello hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="bf004-163">A value of **RED** is returned in hello body of hello response.</span></span> 

    ![Testování funkce hello v hello portálu Azure](./media/functions-twitter-email/test.png)

<span data-ttu-id="bf004-165">Nyní máte funkci, která rozděluje postojích skóre.</span><span class="sxs-lookup"><span data-stu-id="bf004-165">Now you have a function that categorizes sentiment scores.</span></span> <span data-ttu-id="bf004-166">Dále vytvoříte aplikaci logiky, která integruje funkce s vaše účty služby Twitter a kognitivní služby.</span><span class="sxs-lookup"><span data-stu-id="bf004-166">Next, you create a logic app that integrates your function with your Twitter and Cognitive Services accounts.</span></span> 

## <a name="create-a-logic-app"></a><span data-ttu-id="bf004-167">Vytvoření aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="bf004-167">Create a logic app</span></span>   

1. <span data-ttu-id="bf004-168">V hello portálu Azure, klikněte na tlačítko hello **nový** nalezeno tlačítko na hello levém horním rohu hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bf004-168">In hello Azure portal, click hello **New** button found on hello upper left-hand corner of hello Azure portal.</span></span>

2. <span data-ttu-id="bf004-169">Klikněte na tlačítko **Enterprise integrace** > **aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="bf004-169">Click **Enterprise Integration** > **Logic App**.</span></span> <span data-ttu-id="bf004-170">Poté použití hello nastavení zadané v tabulce hello, zkontrolujte **Pin toodashboard**a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="bf004-170">Then, use hello settings as specified in hello table, check **Pin toodashboard**, and click **Create**.</span></span>
 
4. <span data-ttu-id="bf004-171">Poté zadejte **název** jako `TweetSentiment`, použijte nastavení hello uvedeného v tabulce hello, přijměte podmínky hello a zkontrolujte **Pin toodashboard**.</span><span class="sxs-lookup"><span data-stu-id="bf004-171">Then, type a **Name** like `TweetSentiment`,  use hello settings as specified in hello table, accept hello terms, and check **Pin toodashboard**.</span></span>

    ![Vytvoření aplikace logiky v hello portálu Azure](./media/functions-twitter-email/new_logicApp.png)

    | <span data-ttu-id="bf004-173">Nastavení</span><span class="sxs-lookup"><span data-stu-id="bf004-173">Setting</span></span>      |  <span data-ttu-id="bf004-174">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="bf004-174">Suggested value</span></span>   | <span data-ttu-id="bf004-175">Popis</span><span class="sxs-lookup"><span data-stu-id="bf004-175">Description</span></span>                                        |
    | ----------------- | ------------ | ------------- |
    | <span data-ttu-id="bf004-176">**Název**</span><span class="sxs-lookup"><span data-stu-id="bf004-176">**Name**</span></span> | <span data-ttu-id="bf004-177">TweetSentiment</span><span class="sxs-lookup"><span data-stu-id="bf004-177">TweetSentiment</span></span> | <span data-ttu-id="bf004-178">Vyberte vhodný název pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bf004-178">Choose an appropriate name for your app.</span></span> |
    | <span data-ttu-id="bf004-179">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="bf004-179">**Resource group**</span></span> | <span data-ttu-id="bf004-180">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="bf004-180">myResourceGroup</span></span> | <span data-ttu-id="bf004-181">Rozhraní API používá tooanalyze text.</span><span class="sxs-lookup"><span data-stu-id="bf004-181">API used tooanalyze text.</span></span>  |
    | <span data-ttu-id="bf004-182">**Umístění**</span><span class="sxs-lookup"><span data-stu-id="bf004-182">**Location**</span></span> | <span data-ttu-id="bf004-183">Východ USA</span><span class="sxs-lookup"><span data-stu-id="bf004-183">East US</span></span> | <span data-ttu-id="bf004-184">Zvolte tooyou zavřít umístění.</span><span class="sxs-lookup"><span data-stu-id="bf004-184">Choose a location close tooyou.</span></span> |
    | <span data-ttu-id="bf004-185">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="bf004-185">**Resource group**</span></span> | <span data-ttu-id="bf004-186">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="bf004-186">myResourceGroup</span></span> | <span data-ttu-id="bf004-187">Zvolte hello stejné existující skupině prostředků jako předtím.</span><span class="sxs-lookup"><span data-stu-id="bf004-187">Choose hello same existing resource group as before.</span></span>|

4. <span data-ttu-id="bf004-188">Klikněte na tlačítko **vytvořit** toocreate svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="bf004-188">Click **Create** toocreate your logic app.</span></span> <span data-ttu-id="bf004-189">Po vytvoření aplikace hello klikněte na tlačítko nový řídicí panel definovaného toohello aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="bf004-189">After hello app is created, click your new logic app pinned toohello dashboard.</span></span> <span data-ttu-id="bf004-190">Pak v hello návrhář aplikace logiky, posuňte se dolů a klikněte na hello **prázdné aplikace logiky** šablony.</span><span class="sxs-lookup"><span data-stu-id="bf004-190">Then in hello Logic Apps Designer, scroll down and click hello **Blank Logic App** template.</span></span> 

    ![Prázdná šablona aplikace logiky](media/functions-twitter-email/blank.png)

<span data-ttu-id="bf004-192">Teď můžete použít hello logiku aplikace Návrhář tooadd služby a aktivuje tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="bf004-192">You can now use hello Logic Apps Designer tooadd services and triggers tooyour app.</span></span>

## <a name="connect-tootwitter"></a><span data-ttu-id="bf004-193">Připojit tooTwitter</span><span class="sxs-lookup"><span data-stu-id="bf004-193">Connect tooTwitter</span></span>

<span data-ttu-id="bf004-194">Nejprve vytvořte připojení tooyour účtu sítě Twitter.</span><span class="sxs-lookup"><span data-stu-id="bf004-194">First, create a connection tooyour Twitter account.</span></span> <span data-ttu-id="bf004-195">aplikace logiky Hello se dotazuje na tweetů, které aktivují toorun aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="bf004-195">hello logic app polls for tweets, which trigger hello app toorun.</span></span>

1. <span data-ttu-id="bf004-196">V Návrháři hello, klikněte na tlačítko hello **Twitter** služby a klikněte na tlačítko hello **při odeslání nové tweet** aktivační události.</span><span class="sxs-lookup"><span data-stu-id="bf004-196">In hello designer, click hello **Twitter** service, and click hello **When a new tweet is posted** trigger.</span></span> <span data-ttu-id="bf004-197">Přihlaste se tooyour účtu sítě Twitter a autorizovat Logic Apps toouse váš účet.</span><span class="sxs-lookup"><span data-stu-id="bf004-197">Sign in tooyour Twitter account and authorize Logic Apps toouse your account.</span></span>

2. <span data-ttu-id="bf004-198">Použijte nastavení aktivační události služby Twitter hello jako tabulka hello.</span><span class="sxs-lookup"><span data-stu-id="bf004-198">Use hello Twitter trigger settings as specified in hello table.</span></span> 

    ![Nastavení konektoru služby Twitter.](media/functions-twitter-email/azure_tweet.png)

    | <span data-ttu-id="bf004-200">Nastavení</span><span class="sxs-lookup"><span data-stu-id="bf004-200">Setting</span></span>      |  <span data-ttu-id="bf004-201">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="bf004-201">Suggested value</span></span>   | <span data-ttu-id="bf004-202">Popis</span><span class="sxs-lookup"><span data-stu-id="bf004-202">Description</span></span>                                        |
    | ----------------- | ------------ | ------------- |
    | <span data-ttu-id="bf004-203">**Hledaný text**</span><span class="sxs-lookup"><span data-stu-id="bf004-203">**Search text**</span></span> | <span data-ttu-id="bf004-204">#Azure</span><span class="sxs-lookup"><span data-stu-id="bf004-204">#Azure</span></span> | <span data-ttu-id="bf004-205">Použijte hashtagu, který je dostatečně oblíbených pro nové tweetů toogenerate hello vybrali intervalu.</span><span class="sxs-lookup"><span data-stu-id="bf004-205">Use a hashtag that is popular enough for toogenerate new tweets in hello chosen interval.</span></span> <span data-ttu-id="bf004-206">Při použití úroveň Free hello a vaše hashtag je příliš oblíbených, můžete rychle použít hello transakce ve vašem účtu kognitivní služby.</span><span class="sxs-lookup"><span data-stu-id="bf004-206">When using hello Free tier and your hashtag is too popular, you can quickly use up hello transactions in your Cognitive Services account.</span></span> |
    | <span data-ttu-id="bf004-207">**Frekvence**</span><span class="sxs-lookup"><span data-stu-id="bf004-207">**Frequency**</span></span> | <span data-ttu-id="bf004-208">Minuta</span><span class="sxs-lookup"><span data-stu-id="bf004-208">Minute</span></span> | <span data-ttu-id="bf004-209">jednotka frekvence Hello sloužící pro cyklické dotazování služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="bf004-209">hello frequency unit used for polling Twitter.</span></span>  |
    | <span data-ttu-id="bf004-210">**Interval**</span><span class="sxs-lookup"><span data-stu-id="bf004-210">**Interval**</span></span> | <span data-ttu-id="bf004-211">15</span><span class="sxs-lookup"><span data-stu-id="bf004-211">15</span></span> | <span data-ttu-id="bf004-212">Uplynulý čas Hello mezi požadavků služby Twitter, v jednotkách frekvence.</span><span class="sxs-lookup"><span data-stu-id="bf004-212">hello time elapsed between Twitter requests, in frequency units.</span></span> |

3.  <span data-ttu-id="bf004-213">Klikněte na tlačítko **Uložit** tooconnect tooyour účtu sítě Twitter.</span><span class="sxs-lookup"><span data-stu-id="bf004-213">Click **Save** tooconnect tooyour Twitter account.</span></span> 

<span data-ttu-id="bf004-214">Nyní je vaše aplikace připojená tooTwitter.</span><span class="sxs-lookup"><span data-stu-id="bf004-214">Now your app is connected tooTwitter.</span></span> <span data-ttu-id="bf004-215">V dalším kroku připojíte tootext analytics toodetect hello představu o shromážděných tweetů.</span><span class="sxs-lookup"><span data-stu-id="bf004-215">Next, you connect tootext analytics toodetect hello sentiment of collected tweets.</span></span>

## <a name="add-sentiment-detection"></a><span data-ttu-id="bf004-216">Přidat postojích detekce</span><span class="sxs-lookup"><span data-stu-id="bf004-216">Add sentiment detection</span></span>

1. <span data-ttu-id="bf004-217">Klikněte na tlačítko **nový krok**a potom **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="bf004-217">Click **New Step**, and then **Add an action**.</span></span>

    ![Nový krok a potom přidat akci.](media/functions-twitter-email/new_step.png)

2. <span data-ttu-id="bf004-219">V **vybrat akci**, klikněte na tlačítko **Analýza textu**a potom klikněte na hello **zjistit postojích** akce.</span><span class="sxs-lookup"><span data-stu-id="bf004-219">In **Choose an action**, click **Text Analytics**, and then click hello **Detect sentiment** action.</span></span>

    ![Zjištění postojích](media/functions-twitter-email/detect_sent.png)

3. <span data-ttu-id="bf004-221">Zadejte například název připojení `MyCognitiveServicesConnection`, vložte hello klíč pro vaše kognitivní služby účtu, který jste uložili a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="bf004-221">Type a connection name such as `MyCognitiveServicesConnection`, paste hello key for your Cognitive Services account that you saved, and click **Create**.</span></span>  

4. <span data-ttu-id="bf004-222">Klikněte na tlačítko **Text tooanalyze** > **Tweet text**a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="bf004-222">Click **Text tooanalyze** > **Tweet text**, and then click **Save**.</span></span>  

    ![Zjištění postojích](media/functions-twitter-email/ds_tta.png)

<span data-ttu-id="bf004-224">Teď, když je nakonfigurovaná postojích detekce, můžete přidat funkci tooyour připojení, která využívá hello postojích skóre výstup.</span><span class="sxs-lookup"><span data-stu-id="bf004-224">Now that sentiment detection is configured, you can add a connection tooyour function that consumes hello sentiment score output.</span></span>

## <a name="connect-sentiment-output-tooyour-function"></a><span data-ttu-id="bf004-225">Připojit postojích výstup tooyour funkce</span><span class="sxs-lookup"><span data-stu-id="bf004-225">Connect sentiment output tooyour function</span></span>

1. <span data-ttu-id="bf004-226">V hello návrhář aplikace logiky, klikněte na **nový krok** > **přidat akci**a potom klikněte na **Azure Functions**.</span><span class="sxs-lookup"><span data-stu-id="bf004-226">In hello Logic Apps Designer, click **New step** > **Add an action**, and then click **Azure Functions**.</span></span> 

2. <span data-ttu-id="bf004-227">Klikněte na tlačítko **zvolte Azure funkce**, vyberte hello **CategorizeSentiment** funkce, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="bf004-227">Click **Choose an Azure function**, select hello **CategorizeSentiment** function you created earlier.</span></span>  

    ![Azure funkce pole zobrazující zvolte Azure funkce](media/functions-twitter-email/choose_fun.png)

3. <span data-ttu-id="bf004-229">V **text žádosti**, klikněte na tlačítko **skóre** a potom **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="bf004-229">In **Request Body**, click **Score** and then **Save**.</span></span>

    ![Hodnocení](media/functions-twitter-email/trigger_score.png)

<span data-ttu-id="bf004-231">Nyní funkce se aktivuje, když postojích skóre se odesílá z aplikace logiky hello.</span><span class="sxs-lookup"><span data-stu-id="bf004-231">Now, your function is triggered when a sentiment score is sent from hello logic app.</span></span> <span data-ttu-id="bf004-232">Barevně kategorie je aplikace logiky toohello vrácené funkcí hello.</span><span class="sxs-lookup"><span data-stu-id="bf004-232">A color-coded category is returned toohello logic app by hello function.</span></span> <span data-ttu-id="bf004-233">Dál přidejte e-mailové oznámení, odeslaný při hodnotu postojích **RED** je vrácen z funkce hello.</span><span class="sxs-lookup"><span data-stu-id="bf004-233">Next, you add an email notification that is sent when a sentiment value of **RED** is returned from hello function.</span></span> 

## <a name="add-email-notifications"></a><span data-ttu-id="bf004-234">Přidání e-mailových oznámení</span><span class="sxs-lookup"><span data-stu-id="bf004-234">Add email notifications</span></span>

<span data-ttu-id="bf004-235">poslední část Hello hello pracovního postupu je tootrigger e-mailu, když hello postojích skóre pro magnitudu jako _RED_.</span><span class="sxs-lookup"><span data-stu-id="bf004-235">hello last part of hello workflow is tootrigger an email when hello sentiment is scored as _RED_.</span></span> <span data-ttu-id="bf004-236">Toto téma používá konektor Outlook.com.</span><span class="sxs-lookup"><span data-stu-id="bf004-236">This topic uses an Outlook.com connector.</span></span> <span data-ttu-id="bf004-237">Můžete provést podobné kroky toouse konektor z Gmailu nebo Office 365 Outlook.</span><span class="sxs-lookup"><span data-stu-id="bf004-237">You can perform similar steps toouse a Gmail or Office 365 Outlook connector.</span></span>   

1. <span data-ttu-id="bf004-238">V hello návrhář aplikace logiky, klikněte na **nový krok** > **přidat podmínku**.</span><span class="sxs-lookup"><span data-stu-id="bf004-238">In hello Logic Apps Designer, click **New step** > **Add a condition**.</span></span> 

2. <span data-ttu-id="bf004-239">Klikněte na tlačítko **zvolte hodnotu**, pak klikněte na tlačítko **textu**.</span><span class="sxs-lookup"><span data-stu-id="bf004-239">Click **Choose a value**, then click **Body**.</span></span> <span data-ttu-id="bf004-240">Vyberte **rovná**, klikněte na tlačítko **zvolte hodnotu** a typ `RED`a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="bf004-240">Select **is equal to**, click **Choose a value** and type `RED`, and click **Save**.</span></span> 

    ![Přidáte aplikaci logiky toohello podmínku.](media/functions-twitter-email/condition.png)

3. <span data-ttu-id="bf004-242">V **Pokud ano, NEDĚLAT nic**, klikněte na tlačítko **přidat akci**, vyhledejte `outlook.com`, klikněte na tlačítko **e-mailovou zprávu**a přihlaste se tooyour účet Outlook.com.</span><span class="sxs-lookup"><span data-stu-id="bf004-242">In **IF YES, DO NOTHING**, click **Add an action**, search for `outlook.com`, click **Send an email**, and sign in tooyour Outlook.com account.</span></span>
    
    ![Vyberte akci pro podmínku hello.](media/functions-twitter-email/outlook.png)

    > [!NOTE]
    > <span data-ttu-id="bf004-244">Pokud nemáte účet Outlook.com, můžete vybrat jiný konektor, třeba z Gmailu nebo Office 365 Outlook</span><span class="sxs-lookup"><span data-stu-id="bf004-244">If you don't have an Outlook.com account, you can choose another connector, such as Gmail or Office 365 Outlook</span></span>

4. <span data-ttu-id="bf004-245">V hello **e-mailovou zprávu** akci, zadat nastavení e-mailu hello použijte jako v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="bf004-245">In hello **Send an email** action, use hello email settings as specified in hello table.</span></span> 

    ![Nakonfigurujte hello e-mailu pro odeslání hello akce e-mailu.](media/functions-twitter-email/sendemail.png)

    | <span data-ttu-id="bf004-247">Nastavení</span><span class="sxs-lookup"><span data-stu-id="bf004-247">Setting</span></span>      |  <span data-ttu-id="bf004-248">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="bf004-248">Suggested value</span></span>   | <span data-ttu-id="bf004-249">Popis</span><span class="sxs-lookup"><span data-stu-id="bf004-249">Description</span></span>  |
    | ----------------- | ------------ | ------------- |
    | <span data-ttu-id="bf004-250">**K**</span><span class="sxs-lookup"><span data-stu-id="bf004-250">**To**</span></span> | <span data-ttu-id="bf004-251">Zadejte e-mailovou adresu</span><span class="sxs-lookup"><span data-stu-id="bf004-251">Type your email address</span></span> | <span data-ttu-id="bf004-252">Hello e-mailovou adresu, která obdrží oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="bf004-252">hello email address that receives hello notification.</span></span> |
    | <span data-ttu-id="bf004-253">**Předmět**</span><span class="sxs-lookup"><span data-stu-id="bf004-253">**Subject**</span></span> | <span data-ttu-id="bf004-254">Záporné tweet postojích zjistil</span><span class="sxs-lookup"><span data-stu-id="bf004-254">Negative tweet sentiment detected</span></span>  | <span data-ttu-id="bf004-255">Hello předmět hello e-mailové oznámení.</span><span class="sxs-lookup"><span data-stu-id="bf004-255">hello subject line of hello email notification.</span></span>  |
    | <span data-ttu-id="bf004-256">**Text**</span><span class="sxs-lookup"><span data-stu-id="bf004-256">**Body**</span></span> | <span data-ttu-id="bf004-257">Text tweet, umístění</span><span class="sxs-lookup"><span data-stu-id="bf004-257">Tweet text, Location</span></span> | <span data-ttu-id="bf004-258">Klikněte na tlačítko hello **Tweet text** a **umístění** parametry.</span><span class="sxs-lookup"><span data-stu-id="bf004-258">Click hello **Tweet text** and **Location** parameters.</span></span> |

5.  <span data-ttu-id="bf004-259">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="bf004-259">Click **Save**.</span></span>

<span data-ttu-id="bf004-260">Teď, když pracovní postup hello je dokončen, můžete povolit aplikaci logiky hello a najdete v části funkce hello v práci.</span><span class="sxs-lookup"><span data-stu-id="bf004-260">Now that hello workflow is complete, you can enable hello logic app and see hello function at work.</span></span>

## <a name="test-hello-workflow"></a><span data-ttu-id="bf004-261">Pracovní postup testovacího hello</span><span class="sxs-lookup"><span data-stu-id="bf004-261">Test hello workflow</span></span>

1. <span data-ttu-id="bf004-262">V hello návrhář aplikace logiky, klikněte na **spustit** toostart hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="bf004-262">In hello Logic App Designer, click **Run** toostart hello app.</span></span>

2. <span data-ttu-id="bf004-263">V levém sloupci hello, klikněte na **přehled** toosee hello stav hello logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="bf004-263">In hello left column, click **Overview** toosee hello status of hello logic app.</span></span> 
 
    ![Stav spuštění aplikace logiky](media/functions-twitter-email/over1.png)

3. <span data-ttu-id="bf004-265">(Volitelné) Klikněte na jednu z hello spustí toosee podrobnosti o provádění hello.</span><span class="sxs-lookup"><span data-stu-id="bf004-265">(Optional) Click one of hello runs toosee details of hello execution.</span></span>

4. <span data-ttu-id="bf004-266">Přejděte tooyour funkce, hello protokoly a ověřte, že postojích hodnoty byly přijme a zpracuje.</span><span class="sxs-lookup"><span data-stu-id="bf004-266">Go tooyour function, view hello logs, and verify that sentiment values were received and processed.</span></span>
 
    ![Zobrazit protokoly – funkce](media/functions-twitter-email/sent.png)

5. <span data-ttu-id="bf004-268">Když se zjistí potenciálně záporné postojích, obdržíte e-mail.</span><span class="sxs-lookup"><span data-stu-id="bf004-268">When a potentially negative sentiment is detected, you receive an email.</span></span> <span data-ttu-id="bf004-269">Pokud jste neobdrželi e-mailu, můžete změnit hello funkce kód tooreturn RED pokaždé, když:</span><span class="sxs-lookup"><span data-stu-id="bf004-269">If you haven't received an email, you can change hello function code tooreturn RED every time:</span></span>

        return req.CreateResponse(HttpStatusCode.OK, "RED");

    <span data-ttu-id="bf004-270">Po ověření e-mailová oznámení, změňte back toohello původní kód:</span><span class="sxs-lookup"><span data-stu-id="bf004-270">After you have verified email notifications, change back toohello original code:</span></span>

        return req.CreateResponse(HttpStatusCode.OK, category);

    > [!IMPORTANT]
    > <span data-ttu-id="bf004-271">Po dokončení tohoto kurzu, měli byste zakázat aplikaci logiky hello.</span><span class="sxs-lookup"><span data-stu-id="bf004-271">After you have completed this tutorial, you should disable hello logic app.</span></span> <span data-ttu-id="bf004-272">Zakázáním aplikace hello se vyhnout se účtovat pro spuštění a pomocí hello transakce ve vašem účtu kognitivní služby.</span><span class="sxs-lookup"><span data-stu-id="bf004-272">By disabling hello app, you avoid being charged for executions and using up hello transactions in your Cognitive Services account.</span></span>

<span data-ttu-id="bf004-273">Nyní jste viděli, jak je snadné toointegrate funkce do pracovního postupu Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="bf004-273">Now you have seen how easy it is toointegrate Functions into a Logic Apps workflow.</span></span>

## <a name="disable-hello-logic-app"></a><span data-ttu-id="bf004-274">Vypnout aplikaci logiky hello</span><span class="sxs-lookup"><span data-stu-id="bf004-274">Disable hello logic app</span></span>

<span data-ttu-id="bf004-275">aplikace logiky hello toodisable, klikněte na tlačítko **přehled** a pak klikněte na **zakázat** hello horní části úvodní obrazovka.</span><span class="sxs-lookup"><span data-stu-id="bf004-275">toodisable hello logic app, click **Overview** and then click **Disable** at hello top of hello screen.</span></span> <span data-ttu-id="bf004-276">To zastaví hello aplikace logiky spuštěná a nabíhání poplatků za bez odstranění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="bf004-276">This stops hello logic app from running and incurring charges without deleting hello app.</span></span> 

![Protokoly – funkce](media/functions-twitter-email/disable-logic-app.png)

## <a name="next-steps"></a><span data-ttu-id="bf004-278">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bf004-278">Next steps</span></span>

<span data-ttu-id="bf004-279">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="bf004-279">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bf004-280">Vytvoření účtu kognitivní Services.</span><span class="sxs-lookup"><span data-stu-id="bf004-280">Create a Cognitive Services account.</span></span>
> * <span data-ttu-id="bf004-281">Vytvoří funkci, která rozděluje tweet postojích.</span><span class="sxs-lookup"><span data-stu-id="bf004-281">Create a function that categorizes tweet sentiment.</span></span>
> * <span data-ttu-id="bf004-282">Vytvoření aplikace logiky, která se připojuje tooTwitter.</span><span class="sxs-lookup"><span data-stu-id="bf004-282">Create a logic app that connects tooTwitter.</span></span>
> * <span data-ttu-id="bf004-283">Přidáte aplikaci logiky toohello postojích detekce.</span><span class="sxs-lookup"><span data-stu-id="bf004-283">Add sentiment detection toohello logic app.</span></span> 
> * <span data-ttu-id="bf004-284">Připojte hello logiku aplikace toohello funkce.</span><span class="sxs-lookup"><span data-stu-id="bf004-284">Connect hello logic app toohello function.</span></span>
> * <span data-ttu-id="bf004-285">Odešlete e-mail podle hello odpověď z funkce hello.</span><span class="sxs-lookup"><span data-stu-id="bf004-285">Send an email based on hello response from hello function.</span></span>

<span data-ttu-id="bf004-286">Jak zálohy další kurz toolearn toohello toocreate bez serveru rozhraní API pro funkce.</span><span class="sxs-lookup"><span data-stu-id="bf004-286">Advance toohello next tutorial toolearn how toocreate a serverless API for your function.</span></span>

> [!div class="nextstepaction"] 
> [<span data-ttu-id="bf004-287">Vytvoření rozhraní API bez serveru pomocí služby Azure Functions</span><span class="sxs-lookup"><span data-stu-id="bf004-287">Create a serverless API using Azure Functions</span></span>](functions-create-serverless-api.md)

<span data-ttu-id="bf004-288">toolearn Další informace o Logic Apps, najdete v části [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md).</span><span class="sxs-lookup"><span data-stu-id="bf004-288">toolearn more about Logic Apps, see [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md).</span></span>

