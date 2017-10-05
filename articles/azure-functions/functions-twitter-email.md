---
title: "Vytvoří funkci, která se integruje se službou Azure Logic Apps | Microsoft Docs"
description: "Vytvoří funkci, která se integruje se službou Azure Logic Apps a kognitivní služby Azure ke kategorizaci postojích tweet a odesílání oznámení, když postojích je nízký."
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
ms.openlocfilehash: 4a5dc668e21c5328b308c8f5852aaa922232374d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-function-that-integrates-with-azure-logic-apps"></a><span data-ttu-id="617d4-104">Vytvoří funkci, která se integruje se službou Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="617d4-104">Create a function that integrates with Azure Logic Apps</span></span>

<span data-ttu-id="617d4-105">Azure Functions se integruje se službou Azure Logic Apps v Návrháři logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="617d4-105">Azure Functions integrates with Azure Logic Apps in the Logic Apps Designer.</span></span> <span data-ttu-id="617d4-106">Tato integrace umožňuje využívat výpočetní výkon funkcí v orchestrations s jinými Azure a služby třetích stran.</span><span class="sxs-lookup"><span data-stu-id="617d4-106">This integration lets you use the computing power of Functions in orchestrations with other Azure and third-party services.</span></span> 

<span data-ttu-id="617d4-107">V tomto kurzu se dozvíte, jak použít funkce s Logic Apps a kognitivní služby Azure k analýze postojích z příspěvky služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="617d4-107">This tutorial shows you how to use Functions with Logic Apps and Azure Cognitive Services to analyze sentiment from Twitter posts.</span></span> <span data-ttu-id="617d4-108">Funkce protokolu HTTP aktivované rozděluje tweetů jako zelené, žluté nebo červené podle skóre postojích.</span><span class="sxs-lookup"><span data-stu-id="617d4-108">An HTTP triggered function categorizes tweets as green, yellow, or red based on the sentiment score.</span></span> <span data-ttu-id="617d4-109">E-mail je odeslán, když je zjištěna nízký postojích.</span><span class="sxs-lookup"><span data-stu-id="617d4-109">An email is sent when poor sentiment is detected.</span></span> 

![první dva kroky Image aplikace v návrháři aplikace logiky](media/functions-twitter-email/designer1.png)

<span data-ttu-id="617d4-111">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="617d4-111">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="617d4-112">Vytvoření účtu kognitivní Services.</span><span class="sxs-lookup"><span data-stu-id="617d4-112">Create a Cognitive Services account.</span></span>
> * <span data-ttu-id="617d4-113">Vytvoří funkci, která rozděluje tweet postojích.</span><span class="sxs-lookup"><span data-stu-id="617d4-113">Create a function that categorizes tweet sentiment.</span></span>
> * <span data-ttu-id="617d4-114">Vytvoření aplikace logiky, která se připojuje ke službě Twitter.</span><span class="sxs-lookup"><span data-stu-id="617d4-114">Create a logic app that connects to Twitter.</span></span>
> * <span data-ttu-id="617d4-115">Detekce postojích přidáte do aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="617d4-115">Add sentiment detection to the logic app.</span></span> 
> * <span data-ttu-id="617d4-116">Připojení aplikace logiky funkce.</span><span class="sxs-lookup"><span data-stu-id="617d4-116">Connect the logic app to the function.</span></span>
> * <span data-ttu-id="617d4-117">Odešlete e-mail podle odpověď z funkce.</span><span class="sxs-lookup"><span data-stu-id="617d4-117">Send an email based on the response from the function.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="617d4-118">Požadavky</span><span class="sxs-lookup"><span data-stu-id="617d4-118">Prerequisites</span></span>

+ <span data-ttu-id="617d4-119">Aktivní [Twitter](https://twitter.com/) účtu.</span><span class="sxs-lookup"><span data-stu-id="617d4-119">An active [Twitter](https://twitter.com/) account.</span></span> 
+ <span data-ttu-id="617d4-120">[Outlook.com](https://outlook.com/) účet (pro posílání oznámení).</span><span class="sxs-lookup"><span data-stu-id="617d4-120">An [Outlook.com](https://outlook.com/) account (for sending notifications).</span></span>
+ <span data-ttu-id="617d4-121">Toto téma používá jako výchozí bod prostředky, které jste vytvořili v kroku [Vytvoření první funkce na portálu Azure Portal](functions-create-first-azure-function.md).</span><span class="sxs-lookup"><span data-stu-id="617d4-121">This topic uses as its starting point the resources created in [Create your first function from the Azure portal](functions-create-first-azure-function.md).</span></span>  
<span data-ttu-id="617d4-122">Pokud jste tak již neučinili, dokončete tyto kroky teď k vytvoření aplikace funkce.</span><span class="sxs-lookup"><span data-stu-id="617d4-122">If you haven't already done so, complete these steps now to create your function app.</span></span>

## <a name="create-a-cognitive-services-account"></a><span data-ttu-id="617d4-123">Vytvoření účtu kognitivní Services</span><span class="sxs-lookup"><span data-stu-id="617d4-123">Create a Cognitive Services account</span></span>

<span data-ttu-id="617d4-124">Účet kognitivní Services je požadovaná pro zjištění myšlenkou tweetů monitorovány.</span><span class="sxs-lookup"><span data-stu-id="617d4-124">A Cognitive Services account is required to detect the sentiment of tweets being monitored.</span></span>

1. <span data-ttu-id="617d4-125">Přihlaste se k webu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="617d4-125">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>

2. <span data-ttu-id="617d4-126">Klikněte na tlačítko **Nový** v levém horním rohu webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="617d4-126">Click the **New** button found on the upper left-hand corner of the Azure portal.</span></span>

3. <span data-ttu-id="617d4-127">Klikněte na tlačítko **Data + analýzy** > **kognitivní služby**.</span><span class="sxs-lookup"><span data-stu-id="617d4-127">Click **Data + Analytics** > **Cognitive  Services**.</span></span> <span data-ttu-id="617d4-128">Pak používat nastavení zadané v tabulce, přijměte podmínky a zkontrolujte **připnout na řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="617d4-128">Then, use the settings as specified in the table, accept the terms, and check **Pin to dashboard**.</span></span>

    ![Vytvoření okna kognitivní účtu](media/functions-twitter-email/cog_svcs_account.png)

    | <span data-ttu-id="617d4-130">Nastavení</span><span class="sxs-lookup"><span data-stu-id="617d4-130">Setting</span></span>      |  <span data-ttu-id="617d4-131">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="617d4-131">Suggested value</span></span>   | <span data-ttu-id="617d4-132">Popis</span><span class="sxs-lookup"><span data-stu-id="617d4-132">Description</span></span>                                        |
    | --- | --- | --- |
    | <span data-ttu-id="617d4-133">**Název**</span><span class="sxs-lookup"><span data-stu-id="617d4-133">**Name**</span></span> | <span data-ttu-id="617d4-134">MyCognitiveServicesAccnt</span><span class="sxs-lookup"><span data-stu-id="617d4-134">MyCognitiveServicesAccnt</span></span> | <span data-ttu-id="617d4-135">Zvolte název jedinečný účet.</span><span class="sxs-lookup"><span data-stu-id="617d4-135">Choose a unique account name.</span></span> |
    | <span data-ttu-id="617d4-136">**Typ rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="617d4-136">**API type**</span></span> | <span data-ttu-id="617d4-137">Rozhraní Text Analytics API</span><span class="sxs-lookup"><span data-stu-id="617d4-137">Text Analytics API</span></span> | <span data-ttu-id="617d4-138">Rozhraní API použít k analýze textu.</span><span class="sxs-lookup"><span data-stu-id="617d4-138">API used to analyze text.</span></span>  |
    | <span data-ttu-id="617d4-139">**Umístění**</span><span class="sxs-lookup"><span data-stu-id="617d4-139">**Location**</span></span> | <span data-ttu-id="617d4-140">Západní USA</span><span class="sxs-lookup"><span data-stu-id="617d4-140">West US</span></span> | <span data-ttu-id="617d4-141">V současné době pouze **západní USA** je k dispozici pro Analýza textu.</span><span class="sxs-lookup"><span data-stu-id="617d4-141">Currently, only **West US** is available for text analytics.</span></span> |
    | <span data-ttu-id="617d4-142">**Cenová úroveň**</span><span class="sxs-lookup"><span data-stu-id="617d4-142">**Pricing tier**</span></span> | <span data-ttu-id="617d4-143">F0</span><span class="sxs-lookup"><span data-stu-id="617d4-143">F0</span></span> | <span data-ttu-id="617d4-144">Začněte s nejnižší úroveň.</span><span class="sxs-lookup"><span data-stu-id="617d4-144">Start with the lowest tier.</span></span> <span data-ttu-id="617d4-145">Pokud spustíte z volání, škálovat na vyšší úroveň.</span><span class="sxs-lookup"><span data-stu-id="617d4-145">If you run out of calls, scale to a higher tier.</span></span>|
    | <span data-ttu-id="617d4-146">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="617d4-146">**Resource group**</span></span> | <span data-ttu-id="617d4-147">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="617d4-147">myResourceGroup</span></span> | <span data-ttu-id="617d4-148">Používejte stejnou skupinu prostředků pro všechny služby v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="617d4-148">Use the same resource group for all services in this tutorial.</span></span>|

4. <span data-ttu-id="617d4-149">Klikněte na tlačítko **vytvořit** k vytvoření účtu.</span><span class="sxs-lookup"><span data-stu-id="617d4-149">Click **Create** to create your account.</span></span> <span data-ttu-id="617d4-150">Po vytvoření účtu klikněte na tlačítko vaší nové kognitivní služby účet připnuli k řídicímu panelu.</span><span class="sxs-lookup"><span data-stu-id="617d4-150">After the account is created, click your new Cognitive Services account pinned to the dashboard.</span></span> 

5. <span data-ttu-id="617d4-151">V rámci účtu, klikněte na **klíče**a poté zkopírujte hodnotu **klíč 1** a uložte ho.</span><span class="sxs-lookup"><span data-stu-id="617d4-151">In the account, click **Keys**, and then copy the value of **Key 1** and save it.</span></span> <span data-ttu-id="617d4-152">Tento klíč použijete pro připojení k účtu služby kognitivní aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="617d4-152">You use this key to connect the logic app to your Cognitive Services account.</span></span> 
 
    ![Klíče](media/functions-twitter-email/keys.png)

## <a name="create-the-function"></a><span data-ttu-id="617d4-154">Vytvoření funkce</span><span class="sxs-lookup"><span data-stu-id="617d4-154">Create the function</span></span>

<span data-ttu-id="617d4-155">Funkce nabízí skvělý způsob, jak přesměrování zpracování úloh zpracování v pracovním postupu logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="617d4-155">Functions provides a great way to offload processing tasks in a logic apps workflow.</span></span> <span data-ttu-id="617d4-156">Tento kurz používá funkce protokolu HTTP aktivované ke zpracování tweet postojích skóre z kognitivní služeb a vrátit hodnotu kategorie.</span><span class="sxs-lookup"><span data-stu-id="617d4-156">This tutorial uses an HTTP triggered function to process tweet sentiment scores from Cognitive Services and return a category value.</span></span>  

1. <span data-ttu-id="617d4-157">Rozbalte funkce aplikace, klikněte na tlačítko  **+**  vedle položky **funkce**, klikněte na tlačítko **HTTPTrigger** šablony.</span><span class="sxs-lookup"><span data-stu-id="617d4-157">Expand your function app, click the **+** button next to **Functions**, click the **HTTPTrigger** template.</span></span> <span data-ttu-id="617d4-158">Typ `CategorizeSentiment` pro funkci **název** a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="617d4-158">Type `CategorizeSentiment` for the function **Name** and click **Create**.</span></span>

    ![Okno aplikace funkce, funkce +](media/functions-twitter-email/add_fun.png)

2. <span data-ttu-id="617d4-160">Nahraďte obsah souboru run.csx následující kód a potom klikněte na **Uložit**:</span><span class="sxs-lookup"><span data-stu-id="617d4-160">Replace the contents of the run.csx file with the following code, then click **Save**:</span></span>

    ```c#
    using System.Net;
    
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        // The sentiment category defaults to 'GREEN'. 
        string category = "GREEN";
    
        // Get the sentiment score from the request body.
        double score = await req.Content.ReadAsAsync<double>();
        log.Info(string.Format("The sentiment score received is '{0}'.",
                    score.ToString()));
    
        // Set the category based on the sentiment score.
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
    <span data-ttu-id="617d4-161">Tento kód funkce vrátí barevné kategorie podle skóre postojích přijaté v požadavku.</span><span class="sxs-lookup"><span data-stu-id="617d4-161">This function code returns a color category based on the sentiment score received in the request.</span></span> 

3. <span data-ttu-id="617d4-162">Chcete-li otestovat funkci, klikněte na tlačítko **testování** na pravém rozbalte karta testu. Zadejte hodnotu `0.2` pro **text žádosti**a potom klikněte na **spustit**.</span><span class="sxs-lookup"><span data-stu-id="617d4-162">To test the function, click **Test** at the far right to expand the Test tab. Type a value of `0.2` for the **Request body**, and then click **Run**.</span></span> <span data-ttu-id="617d4-163">Hodnota **RED** je vrácený v textu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="617d4-163">A value of **RED** is returned in the body of the response.</span></span> 

    ![Testování funkce na portálu Azure](./media/functions-twitter-email/test.png)

<span data-ttu-id="617d4-165">Nyní máte funkci, která rozděluje postojích skóre.</span><span class="sxs-lookup"><span data-stu-id="617d4-165">Now you have a function that categorizes sentiment scores.</span></span> <span data-ttu-id="617d4-166">Dále vytvoříte aplikaci logiky, která integruje funkce s vaše účty služby Twitter a kognitivní služby.</span><span class="sxs-lookup"><span data-stu-id="617d4-166">Next, you create a logic app that integrates your function with your Twitter and Cognitive Services accounts.</span></span> 

## <a name="create-a-logic-app"></a><span data-ttu-id="617d4-167">Vytvoření aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="617d4-167">Create a logic app</span></span>   

1. <span data-ttu-id="617d4-168">Na portálu Azure klikněte **nový** nalezeno tlačítko v levém horním rohu portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="617d4-168">In the Azure portal, click the **New** button found on the upper left-hand corner of the Azure portal.</span></span>

2. <span data-ttu-id="617d4-169">Klikněte na tlačítko **Enterprise integrace** > **aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="617d4-169">Click **Enterprise Integration** > **Logic App**.</span></span> <span data-ttu-id="617d4-170">Pak používat nastavení zadané v tabulce, zkontrolujte **připnout na řídicí panel**a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="617d4-170">Then, use the settings as specified in the table, check **Pin to dashboard**, and click **Create**.</span></span>
 
4. <span data-ttu-id="617d4-171">Poté zadejte **název** jako `TweetSentiment`používat nastavení zadané v tabulce, přijměte podmínky a zkontrolujte **připnout na řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="617d4-171">Then, type a **Name** like `TweetSentiment`,  use the settings as specified in the table, accept the terms, and check **Pin to dashboard**.</span></span>

    ![Vytvoření aplikace logiky na portálu Azure](./media/functions-twitter-email/new_logicApp.png)

    | <span data-ttu-id="617d4-173">Nastavení</span><span class="sxs-lookup"><span data-stu-id="617d4-173">Setting</span></span>      |  <span data-ttu-id="617d4-174">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="617d4-174">Suggested value</span></span>   | <span data-ttu-id="617d4-175">Popis</span><span class="sxs-lookup"><span data-stu-id="617d4-175">Description</span></span>                                        |
    | ----------------- | ------------ | ------------- |
    | <span data-ttu-id="617d4-176">**Název**</span><span class="sxs-lookup"><span data-stu-id="617d4-176">**Name**</span></span> | <span data-ttu-id="617d4-177">TweetSentiment</span><span class="sxs-lookup"><span data-stu-id="617d4-177">TweetSentiment</span></span> | <span data-ttu-id="617d4-178">Vyberte vhodný název pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="617d4-178">Choose an appropriate name for your app.</span></span> |
    | <span data-ttu-id="617d4-179">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="617d4-179">**Resource group**</span></span> | <span data-ttu-id="617d4-180">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="617d4-180">myResourceGroup</span></span> | <span data-ttu-id="617d4-181">Rozhraní API použít k analýze textu.</span><span class="sxs-lookup"><span data-stu-id="617d4-181">API used to analyze text.</span></span>  |
    | <span data-ttu-id="617d4-182">**Umístění**</span><span class="sxs-lookup"><span data-stu-id="617d4-182">**Location**</span></span> | <span data-ttu-id="617d4-183">Východ USA</span><span class="sxs-lookup"><span data-stu-id="617d4-183">East US</span></span> | <span data-ttu-id="617d4-184">Vyberte umístění blízko vás.</span><span class="sxs-lookup"><span data-stu-id="617d4-184">Choose a location close to you.</span></span> |
    | <span data-ttu-id="617d4-185">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="617d4-185">**Resource group**</span></span> | <span data-ttu-id="617d4-186">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="617d4-186">myResourceGroup</span></span> | <span data-ttu-id="617d4-187">Zvolte stejné existující skupinu prostředků, jako před.</span><span class="sxs-lookup"><span data-stu-id="617d4-187">Choose the same existing resource group as before.</span></span>|

4. <span data-ttu-id="617d4-188">Klikněte na tlačítko **vytvořit** k vytvoření aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="617d4-188">Click **Create** to create your logic app.</span></span> <span data-ttu-id="617d4-189">Po vytvoření aplikace, klikněte na nové aplikace logiky připnuli k řídicímu panelu.</span><span class="sxs-lookup"><span data-stu-id="617d4-189">After the app is created, click your new logic app pinned to the dashboard.</span></span> <span data-ttu-id="617d4-190">Potom v návrháři aplikace logiky, přejděte dolů a kliknutím **prázdné aplikace logiky** šablony.</span><span class="sxs-lookup"><span data-stu-id="617d4-190">Then in the Logic Apps Designer, scroll down and click the **Blank Logic App** template.</span></span> 

    ![Prázdná šablona aplikace logiky](media/functions-twitter-email/blank.png)

<span data-ttu-id="617d4-192">Návrhář aplikace logiky teď můžete přidat do aplikace služeb a aktivační události.</span><span class="sxs-lookup"><span data-stu-id="617d4-192">You can now use the Logic Apps Designer to add services and triggers to your app.</span></span>

## <a name="connect-to-twitter"></a><span data-ttu-id="617d4-193">Připojení k Twitteru</span><span class="sxs-lookup"><span data-stu-id="617d4-193">Connect to Twitter</span></span>

<span data-ttu-id="617d4-194">Nejprve vytvořte připojení ke svému účtu služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="617d4-194">First, create a connection to your Twitter account.</span></span> <span data-ttu-id="617d4-195">Aplikace logiky se dotazuje na tweetů, které aktivují aplikaci spustit.</span><span class="sxs-lookup"><span data-stu-id="617d4-195">The logic app polls for tweets, which trigger the app to run.</span></span>

1. <span data-ttu-id="617d4-196">V návrháři, klikněte na tlačítko **Twitter** služby a klikněte na tlačítko **při odeslání nové tweet** aktivační události.</span><span class="sxs-lookup"><span data-stu-id="617d4-196">In the designer, click the **Twitter** service, and click the **When a new tweet is posted** trigger.</span></span> <span data-ttu-id="617d4-197">Přihlaste se k účtu služby Twitter a autorizaci Logic Apps, abyste používat svůj účet.</span><span class="sxs-lookup"><span data-stu-id="617d4-197">Sign in to your Twitter account and authorize Logic Apps to use your account.</span></span>

2. <span data-ttu-id="617d4-198">Použijte nastavení aktivační události služby Twitter jako zadaný v tabulce.</span><span class="sxs-lookup"><span data-stu-id="617d4-198">Use the Twitter trigger settings as specified in the table.</span></span> 

    ![Nastavení konektoru služby Twitter.](media/functions-twitter-email/azure_tweet.png)

    | <span data-ttu-id="617d4-200">Nastavení</span><span class="sxs-lookup"><span data-stu-id="617d4-200">Setting</span></span>      |  <span data-ttu-id="617d4-201">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="617d4-201">Suggested value</span></span>   | <span data-ttu-id="617d4-202">Popis</span><span class="sxs-lookup"><span data-stu-id="617d4-202">Description</span></span>                                        |
    | ----------------- | ------------ | ------------- |
    | <span data-ttu-id="617d4-203">**Hledaný text**</span><span class="sxs-lookup"><span data-stu-id="617d4-203">**Search text**</span></span> | <span data-ttu-id="617d4-204">#Azure</span><span class="sxs-lookup"><span data-stu-id="617d4-204">#Azure</span></span> | <span data-ttu-id="617d4-205">Použijte hashtagu, který je dostatečně oblíbených pro generovat nové tweetů zvolené intervalu.</span><span class="sxs-lookup"><span data-stu-id="617d4-205">Use a hashtag that is popular enough for to generate new tweets in the chosen interval.</span></span> <span data-ttu-id="617d4-206">Při použití úroveň Free a vaše hashtag je příliš oblíbených, můžete rychle použít až transakce ve vašem účtu kognitivní služby.</span><span class="sxs-lookup"><span data-stu-id="617d4-206">When using the Free tier and your hashtag is too popular, you can quickly use up the transactions in your Cognitive Services account.</span></span> |
    | <span data-ttu-id="617d4-207">**Frekvence**</span><span class="sxs-lookup"><span data-stu-id="617d4-207">**Frequency**</span></span> | <span data-ttu-id="617d4-208">Minuta</span><span class="sxs-lookup"><span data-stu-id="617d4-208">Minute</span></span> | <span data-ttu-id="617d4-209">Frekvence jednotka použít pro cyklické dotazování služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="617d4-209">The frequency unit used for polling Twitter.</span></span>  |
    | <span data-ttu-id="617d4-210">**Interval**</span><span class="sxs-lookup"><span data-stu-id="617d4-210">**Interval**</span></span> | <span data-ttu-id="617d4-211">15</span><span class="sxs-lookup"><span data-stu-id="617d4-211">15</span></span> | <span data-ttu-id="617d4-212">Uplynulý čas mezi požadavků služby Twitter, v jednotkách frekvence.</span><span class="sxs-lookup"><span data-stu-id="617d4-212">The time elapsed between Twitter requests, in frequency units.</span></span> |

3.  <span data-ttu-id="617d4-213">Klikněte na tlačítko **Uložit** pro připojení k účtu služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="617d4-213">Click **Save** to connect to your Twitter account.</span></span> 

<span data-ttu-id="617d4-214">Aplikace je teď připojený k Twitter.</span><span class="sxs-lookup"><span data-stu-id="617d4-214">Now your app is connected to Twitter.</span></span> <span data-ttu-id="617d4-215">V dalším kroku připojíte k Analýza textu k detekci myšlenkou shromážděných tweetů.</span><span class="sxs-lookup"><span data-stu-id="617d4-215">Next, you connect to text analytics to detect the sentiment of collected tweets.</span></span>

## <a name="add-sentiment-detection"></a><span data-ttu-id="617d4-216">Přidat postojích detekce</span><span class="sxs-lookup"><span data-stu-id="617d4-216">Add sentiment detection</span></span>

1. <span data-ttu-id="617d4-217">Klikněte na tlačítko **nový krok**a potom **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="617d4-217">Click **New Step**, and then **Add an action**.</span></span>

    ![Nový krok a potom přidat akci.](media/functions-twitter-email/new_step.png)

2. <span data-ttu-id="617d4-219">V **vybrat akci**, klikněte na tlačítko **Analýza textu**a pak klikněte na tlačítko **zjistit postojích** akce.</span><span class="sxs-lookup"><span data-stu-id="617d4-219">In **Choose an action**, click **Text Analytics**, and then click the **Detect sentiment** action.</span></span>

    ![Zjištění postojích](media/functions-twitter-email/detect_sent.png)

3. <span data-ttu-id="617d4-221">Zadejte například název připojení `MyCognitiveServicesConnection`, vložte klíč pro vaše kognitivní služby účtu, který jste uložili a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="617d4-221">Type a connection name such as `MyCognitiveServicesConnection`, paste the key for your Cognitive Services account that you saved, and click **Create**.</span></span>  

4. <span data-ttu-id="617d4-222">Klikněte na tlačítko **Text k analýze** > **Tweet text**a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="617d4-222">Click **Text to analyze** > **Tweet text**, and then click **Save**.</span></span>  

    ![Zjištění postojích](media/functions-twitter-email/ds_tta.png)

<span data-ttu-id="617d4-224">Teď, když je nakonfigurovaná postojích detekce, můžete přidat připojení k vaší funkci, která spotřebovává výstup postojích skóre.</span><span class="sxs-lookup"><span data-stu-id="617d4-224">Now that sentiment detection is configured, you can add a connection to your function that consumes the sentiment score output.</span></span>

## <a name="connect-sentiment-output-to-your-function"></a><span data-ttu-id="617d4-225">Připojení k vaší funkci postojích výstup</span><span class="sxs-lookup"><span data-stu-id="617d4-225">Connect sentiment output to your function</span></span>

1. <span data-ttu-id="617d4-226">V návrháři aplikace logiky, klikněte na tlačítko **nový krok** > **přidat akci**a potom klikněte na **Azure Functions**.</span><span class="sxs-lookup"><span data-stu-id="617d4-226">In the Logic Apps Designer, click **New step** > **Add an action**, and then click **Azure Functions**.</span></span> 

2. <span data-ttu-id="617d4-227">Klikněte na tlačítko **zvolte Azure funkce**, vyberte **CategorizeSentiment** funkce, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="617d4-227">Click **Choose an Azure function**, select the **CategorizeSentiment** function you created earlier.</span></span>  

    ![Azure funkce pole zobrazující zvolte Azure funkce](media/functions-twitter-email/choose_fun.png)

3. <span data-ttu-id="617d4-229">V **text žádosti**, klikněte na tlačítko **skóre** a potom **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="617d4-229">In **Request Body**, click **Score** and then **Save**.</span></span>

    ![Hodnocení](media/functions-twitter-email/trigger_score.png)

<span data-ttu-id="617d4-231">Nyní funkce se aktivuje, když postojích skóre se odesílá z aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="617d4-231">Now, your function is triggered when a sentiment score is sent from the logic app.</span></span> <span data-ttu-id="617d4-232">Barevně kategorie je do aplikace logiky vráceným funkcí.</span><span class="sxs-lookup"><span data-stu-id="617d4-232">A color-coded category is returned to the logic app by the function.</span></span> <span data-ttu-id="617d4-233">Dál přidejte e-mailové oznámení, odeslaný při hodnotu postojích **RED** se vrátí z funkce.</span><span class="sxs-lookup"><span data-stu-id="617d4-233">Next, you add an email notification that is sent when a sentiment value of **RED** is returned from the function.</span></span> 

## <a name="add-email-notifications"></a><span data-ttu-id="617d4-234">Přidání e-mailových oznámení</span><span class="sxs-lookup"><span data-stu-id="617d4-234">Add email notifications</span></span>

<span data-ttu-id="617d4-235">Poslední část pracovního postupu je pro aktivaci e-mailu, když myšlenkou skóre pro magnitudu jako _RED_.</span><span class="sxs-lookup"><span data-stu-id="617d4-235">The last part of the workflow is to trigger an email when the sentiment is scored as _RED_.</span></span> <span data-ttu-id="617d4-236">Toto téma používá konektor Outlook.com.</span><span class="sxs-lookup"><span data-stu-id="617d4-236">This topic uses an Outlook.com connector.</span></span> <span data-ttu-id="617d4-237">Abyste mohli provést podobné kroky k používání konektoru z Gmailu nebo Office 365 Outlook.</span><span class="sxs-lookup"><span data-stu-id="617d4-237">You can perform similar steps to use a Gmail or Office 365 Outlook connector.</span></span>   

1. <span data-ttu-id="617d4-238">V návrháři aplikace logiky, klikněte na tlačítko **nový krok** > **přidat podmínku**.</span><span class="sxs-lookup"><span data-stu-id="617d4-238">In the Logic Apps Designer, click **New step** > **Add a condition**.</span></span> 

2. <span data-ttu-id="617d4-239">Klikněte na tlačítko **zvolte hodnotu**, pak klikněte na tlačítko **textu**.</span><span class="sxs-lookup"><span data-stu-id="617d4-239">Click **Choose a value**, then click **Body**.</span></span> <span data-ttu-id="617d4-240">Vyberte **rovná**, klikněte na tlačítko **zvolte hodnotu** a typ `RED`a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="617d4-240">Select **is equal to**, click **Choose a value** and type `RED`, and click **Save**.</span></span> 

    ![Přidáte podmínku do aplikace logiky.](media/functions-twitter-email/condition.png)

3. <span data-ttu-id="617d4-242">V **Pokud ano, NEDĚLAT nic**, klikněte na tlačítko **přidat akci**, vyhledejte `outlook.com`, klikněte na tlačítko **e-mailovou zprávu**a přihlaste se ke svému účtu Outlook.com.</span><span class="sxs-lookup"><span data-stu-id="617d4-242">In **IF YES, DO NOTHING**, click **Add an action**, search for `outlook.com`, click **Send an email**, and sign in to your Outlook.com account.</span></span>
    
    ![Vyberte akci pro podmínku.](media/functions-twitter-email/outlook.png)

    > [!NOTE]
    > <span data-ttu-id="617d4-244">Pokud nemáte účet Outlook.com, můžete vybrat jiný konektor, třeba z Gmailu nebo Office 365 Outlook</span><span class="sxs-lookup"><span data-stu-id="617d4-244">If you don't have an Outlook.com account, you can choose another connector, such as Gmail or Office 365 Outlook</span></span>

4. <span data-ttu-id="617d4-245">V **e-mailovou zprávu** akce, použijte nastavení e-mailu jako zadané v tabulce.</span><span class="sxs-lookup"><span data-stu-id="617d4-245">In the **Send an email** action, use the email settings as specified in the table.</span></span> 

    ![Konfigurace e-mailu pro odeslání e-mailu akce.](media/functions-twitter-email/sendemail.png)

    | <span data-ttu-id="617d4-247">Nastavení</span><span class="sxs-lookup"><span data-stu-id="617d4-247">Setting</span></span>      |  <span data-ttu-id="617d4-248">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="617d4-248">Suggested value</span></span>   | <span data-ttu-id="617d4-249">Popis</span><span class="sxs-lookup"><span data-stu-id="617d4-249">Description</span></span>  |
    | ----------------- | ------------ | ------------- |
    | <span data-ttu-id="617d4-250">**K**</span><span class="sxs-lookup"><span data-stu-id="617d4-250">**To**</span></span> | <span data-ttu-id="617d4-251">Zadejte e-mailovou adresu</span><span class="sxs-lookup"><span data-stu-id="617d4-251">Type your email address</span></span> | <span data-ttu-id="617d4-252">E-mailovou adresu, která bude přijímat oznámení.</span><span class="sxs-lookup"><span data-stu-id="617d4-252">The email address that receives the notification.</span></span> |
    | <span data-ttu-id="617d4-253">**Předmět**</span><span class="sxs-lookup"><span data-stu-id="617d4-253">**Subject**</span></span> | <span data-ttu-id="617d4-254">Záporné tweet postojích zjistil</span><span class="sxs-lookup"><span data-stu-id="617d4-254">Negative tweet sentiment detected</span></span>  | <span data-ttu-id="617d4-255">Předmět e-mailové oznámení.</span><span class="sxs-lookup"><span data-stu-id="617d4-255">The subject line of the email notification.</span></span>  |
    | <span data-ttu-id="617d4-256">**Text**</span><span class="sxs-lookup"><span data-stu-id="617d4-256">**Body**</span></span> | <span data-ttu-id="617d4-257">Text tweet, umístění</span><span class="sxs-lookup"><span data-stu-id="617d4-257">Tweet text, Location</span></span> | <span data-ttu-id="617d4-258">Klikněte **Tweet text** a **umístění** parametry.</span><span class="sxs-lookup"><span data-stu-id="617d4-258">Click the **Tweet text** and **Location** parameters.</span></span> |

5.  <span data-ttu-id="617d4-259">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="617d4-259">Click **Save**.</span></span>

<span data-ttu-id="617d4-260">Teď, když je dokončení pracovního postupu, můžete povolit aplikaci logiky a najdete v části funkce v práci.</span><span class="sxs-lookup"><span data-stu-id="617d4-260">Now that the workflow is complete, you can enable the logic app and see the function at work.</span></span>

## <a name="test-the-workflow"></a><span data-ttu-id="617d4-261">Testování pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="617d4-261">Test the workflow</span></span>

1. <span data-ttu-id="617d4-262">V návrháři aplikace logiky, klikněte na tlačítko **spustit** a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="617d4-262">In the Logic App Designer, click **Run** to start the app.</span></span>

2. <span data-ttu-id="617d4-263">V levém sloupci klikněte na tlačítko **přehled** zobrazíte stav aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="617d4-263">In the left column, click **Overview** to see the status of the logic app.</span></span> 
 
    ![Stav spuštění aplikace logiky](media/functions-twitter-email/over1.png)

3. <span data-ttu-id="617d4-265">(Volitelné) Klikněte na jednu z spuštění pro podrobné informace o provádění.</span><span class="sxs-lookup"><span data-stu-id="617d4-265">(Optional) Click one of the runs to see details of the execution.</span></span>

4. <span data-ttu-id="617d4-266">Přejděte na funkce, protokoly a ověřte, že postojích hodnoty byly přijme a zpracuje.</span><span class="sxs-lookup"><span data-stu-id="617d4-266">Go to your function, view the logs, and verify that sentiment values were received and processed.</span></span>
 
    ![Zobrazit protokoly – funkce](media/functions-twitter-email/sent.png)

5. <span data-ttu-id="617d4-268">Když se zjistí potenciálně záporné postojích, obdržíte e-mail.</span><span class="sxs-lookup"><span data-stu-id="617d4-268">When a potentially negative sentiment is detected, you receive an email.</span></span> <span data-ttu-id="617d4-269">Pokud jste neobdrželi e-mailu, můžete změnit kód funkce vrátit RED pokaždé, když:</span><span class="sxs-lookup"><span data-stu-id="617d4-269">If you haven't received an email, you can change the function code to return RED every time:</span></span>

        return req.CreateResponse(HttpStatusCode.OK, "RED");

    <span data-ttu-id="617d4-270">Po ověření e-mailová oznámení, změňte zpátky na původní kód:</span><span class="sxs-lookup"><span data-stu-id="617d4-270">After you have verified email notifications, change back to the original code:</span></span>

        return req.CreateResponse(HttpStatusCode.OK, category);

    > [!IMPORTANT]
    > <span data-ttu-id="617d4-271">Po dokončení tohoto kurzu, měli byste zakázat aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="617d4-271">After you have completed this tutorial, you should disable the logic app.</span></span> <span data-ttu-id="617d4-272">Zakázáním aplikace se vyhnout se účtovat pro spuštění a pomocí transakce ve vašem účtu kognitivní služby.</span><span class="sxs-lookup"><span data-stu-id="617d4-272">By disabling the app, you avoid being charged for executions and using up the transactions in your Cognitive Services account.</span></span>

<span data-ttu-id="617d4-273">Nyní jste viděli, jak je snadné pro integraci funkcí do pracovního postupu Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="617d4-273">Now you have seen how easy it is to integrate Functions into a Logic Apps workflow.</span></span>

## <a name="disable-the-logic-app"></a><span data-ttu-id="617d4-274">Vypnout aplikaci logiky</span><span class="sxs-lookup"><span data-stu-id="617d4-274">Disable the logic app</span></span>

<span data-ttu-id="617d4-275">Chcete-li zakázat aplikaci logiky, klikněte na tlačítko **přehled** a pak klikněte na **zakázat** v horní části obrazovky.</span><span class="sxs-lookup"><span data-stu-id="617d4-275">To disable the logic app, click **Overview** and then click **Disable** at the top of the screen.</span></span> <span data-ttu-id="617d4-276">To zastaví aplikace logiky spuštěná a nabíhání poplatků za bez odstranění aplikace.</span><span class="sxs-lookup"><span data-stu-id="617d4-276">This stops the logic app from running and incurring charges without deleting the app.</span></span> 

![Protokoly – funkce](media/functions-twitter-email/disable-logic-app.png)

## <a name="next-steps"></a><span data-ttu-id="617d4-278">Další kroky</span><span class="sxs-lookup"><span data-stu-id="617d4-278">Next steps</span></span>

<span data-ttu-id="617d4-279">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="617d4-279">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="617d4-280">Vytvoření účtu kognitivní Services.</span><span class="sxs-lookup"><span data-stu-id="617d4-280">Create a Cognitive Services account.</span></span>
> * <span data-ttu-id="617d4-281">Vytvoří funkci, která rozděluje tweet postojích.</span><span class="sxs-lookup"><span data-stu-id="617d4-281">Create a function that categorizes tweet sentiment.</span></span>
> * <span data-ttu-id="617d4-282">Vytvoření aplikace logiky, která se připojuje ke službě Twitter.</span><span class="sxs-lookup"><span data-stu-id="617d4-282">Create a logic app that connects to Twitter.</span></span>
> * <span data-ttu-id="617d4-283">Detekce postojích přidáte do aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="617d4-283">Add sentiment detection to the logic app.</span></span> 
> * <span data-ttu-id="617d4-284">Připojení aplikace logiky funkce.</span><span class="sxs-lookup"><span data-stu-id="617d4-284">Connect the logic app to the function.</span></span>
> * <span data-ttu-id="617d4-285">Odešlete e-mail podle odpověď z funkce.</span><span class="sxs-lookup"><span data-stu-id="617d4-285">Send an email based on the response from the function.</span></span>

<span data-ttu-id="617d4-286">Přechodu na v dalším kurzu se dozvíte, jak vytvořit bez serveru rozhraní API pro funkce.</span><span class="sxs-lookup"><span data-stu-id="617d4-286">Advance to the next tutorial to learn how to create a serverless API for your function.</span></span>

> [!div class="nextstepaction"] 
> [<span data-ttu-id="617d4-287">Vytvoření rozhraní API bez serveru pomocí služby Azure Functions</span><span class="sxs-lookup"><span data-stu-id="617d4-287">Create a serverless API using Azure Functions</span></span>](functions-create-serverless-api.md)

<span data-ttu-id="617d4-288">Další informace o Logic Apps najdete v tématu [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md).</span><span class="sxs-lookup"><span data-stu-id="617d4-288">To learn more about Logic Apps, see [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md).</span></span>

