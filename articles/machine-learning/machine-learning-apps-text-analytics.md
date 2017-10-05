---
title: "Rozhraní API pro Machine Learning: Analýza textu | Microsoft Docs"
description: "Společnosti Microsoft Machine Learning Text Analytics rozhraní API slouží k analýze nestrukturovaných text pro analýzu postojích, klíče frázi extrakce, zjišťování jazyka a detekce tématu."
services: machine-learning
documentationcenter: 
author: onewth
manager: jhubbard
editor: cgronlun
ms.assetid: 5b60694e-5521-4e4d-bf6a-1a92fdf94b65
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: onewth
ROBOTS: NOINDEX
redirect_url: ../cognitive-services/cognitive-services-text-analytics-quick-start
redirect_document_id: TRUE
ms.openlocfilehash: 10eae2ff5624dcb57de1cf72b326147f35bc2a0b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="machine-learning-apis-text-analytics-for-sentiment-key-phrase-extraction-language-detection-and-topic-detection"></a><span data-ttu-id="fe036-103">API pro Machine Learning: Analýza textu pro zjištění mínění, Extrakce klíčových frází, Rozpoznávání jazyka a Rozpoznávání témat</span><span class="sxs-lookup"><span data-stu-id="fe036-103">Machine Learning APIs: Text Analytics for Sentiment, Key Phrase Extraction, Language Detection and Topic Detection</span></span>
> [!NOTE]
> <span data-ttu-id="fe036-104">Tento průvodce je pro rozhraní API verze 1.</span><span class="sxs-lookup"><span data-stu-id="fe036-104">This guide is for version 1 of the API.</span></span> <span data-ttu-id="fe036-105">Verze 2 [ **odkazovat na tento dokument**](../cognitive-services/cognitive-services-text-analytics-quick-start.md).</span><span class="sxs-lookup"><span data-stu-id="fe036-105">For version 2, [**refer to this document**](../cognitive-services/cognitive-services-text-analytics-quick-start.md).</span></span> <span data-ttu-id="fe036-106">Verze 2 je nyní upřednostňované verzi toto rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fe036-106">Version 2 is now the preferred version of this API.</span></span>
> 
> 

## <a name="overview"></a><span data-ttu-id="fe036-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="fe036-107">Overview</span></span>
<span data-ttu-id="fe036-108">Rozhraní API Analytics textu je sada Analýza textu [webové služby](https://datamarket.azure.com/dataset/amla/text-analytics) vytvořené s nástroji Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="fe036-108">The Text Analytics API is a suite of text analytics [web services](https://datamarket.azure.com/dataset/amla/text-analytics) built with Azure Machine Learning.</span></span> <span data-ttu-id="fe036-109">Rozhraní API slouží k analýze nestrukturovaných text pro úlohy, jako je postojích analýzy, klíče frázi extrakce, zjišťování jazyka a detekce tématu.</span><span class="sxs-lookup"><span data-stu-id="fe036-109">The API can be used to analyze unstructured text for tasks such as sentiment analysis, key phrase extraction, language detection and topic detection.</span></span> <span data-ttu-id="fe036-110">Žádná Cvičná data je potřeba k použití toto rozhraní API: právě Oživte svoje data textu.</span><span class="sxs-lookup"><span data-stu-id="fe036-110">No training data is needed to use this API: just bring your text data.</span></span> <span data-ttu-id="fe036-111">Toto rozhraní API používá pokročilé přirozeného jazyka zpracování techniky dodávat nejlépe ve třída předpovědi.</span><span class="sxs-lookup"><span data-stu-id="fe036-111">This API uses advanced natural language processing techniques to deliver best in class predictions.</span></span>

<span data-ttu-id="fe036-112">Analýza textu v akci uvidíte na našich [ukázku lokality](https://text-analytics-demo.azurewebsites.net/), kde taky najdete [ukázky](https://text-analytics-demo.azurewebsites.net/Home/SampleCode) o tom, jak implementovat Analýza textu v C# a Python.</span><span class="sxs-lookup"><span data-stu-id="fe036-112">You can see text analytics in action on our [demo site](https://text-analytics-demo.azurewebsites.net/), where you will also find [samples](https://text-analytics-demo.azurewebsites.net/Home/SampleCode) on how to implement text analytics in C# and Python.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

- - -
## <a name="sentiment-analysis"></a><span data-ttu-id="fe036-113">Analýza subjektivního hodnocení</span><span class="sxs-lookup"><span data-stu-id="fe036-113">Sentiment analysis</span></span>
<span data-ttu-id="fe036-114">Rozhraní API vrátí číselný skóre mezi 0 a 1.</span><span class="sxs-lookup"><span data-stu-id="fe036-114">The API returns a numeric score between 0 & 1.</span></span> <span data-ttu-id="fe036-115">Skóre blíží 1 znamenat kladné postojích, zatímco skóre blízko 0 označuje záporné postojích.</span><span class="sxs-lookup"><span data-stu-id="fe036-115">Scores close to 1 indicate positive sentiment, while scores close to 0 indicate negative sentiment.</span></span> <span data-ttu-id="fe036-116">Hodnocení zabarvení se generuje pomocí klasifikačních technik.</span><span class="sxs-lookup"><span data-stu-id="fe036-116">Sentiment score is generated using classification techniques.</span></span> <span data-ttu-id="fe036-117">Vstupní funkce na třídění zahrnují n gram, funkce, které generují z části řeči značky a vkládaných aplikace word.</span><span class="sxs-lookup"><span data-stu-id="fe036-117">The input features to the classifier include n-grams, features generated from part-of-speech tags, and word embeddings.</span></span> <span data-ttu-id="fe036-118">V současné době pouze podporovaných jazykem je angličtina.</span><span class="sxs-lookup"><span data-stu-id="fe036-118">Currently, English is the only supported language.</span></span>

## <a name="key-phrase-extraction"></a><span data-ttu-id="fe036-119">Extrakce klíčových frází</span><span class="sxs-lookup"><span data-stu-id="fe036-119">Key phrase extraction</span></span>
<span data-ttu-id="fe036-120">Rozhraní API vrátí seznam řetězců označujících klíčová témata ve vstupním textu.</span><span class="sxs-lookup"><span data-stu-id="fe036-120">The API returns a list of strings denoting the key talking points in the input text.</span></span> <span data-ttu-id="fe036-121">Využíváme techniky z propracované sady nástrojů pro zpracování přirozeného jazyka v systému Microsoft Office.</span><span class="sxs-lookup"><span data-stu-id="fe036-121">We employ techniques from Microsoft Office's sophisticated Natural Language Processing toolkit.</span></span> <span data-ttu-id="fe036-122">V současné době pouze podporovaných jazykem je angličtina.</span><span class="sxs-lookup"><span data-stu-id="fe036-122">Currently, English is the only supported language.</span></span>

## <a name="language-detection"></a><span data-ttu-id="fe036-123">Detekce jazyka</span><span class="sxs-lookup"><span data-stu-id="fe036-123">Language detection</span></span>
<span data-ttu-id="fe036-124">Rozhraní API vrátí zjištěný jazyk a číselné skóre mezi 0 a 1.</span><span class="sxs-lookup"><span data-stu-id="fe036-124">The API returns the detected language and a numeric score between 0 & 1.</span></span> <span data-ttu-id="fe036-125">Hodnocení blížící se 1 značí 100% jistotu správné identifikace jazyka.</span><span class="sxs-lookup"><span data-stu-id="fe036-125">Scores close to 1 indicate 100% certainty that the identified language is true.</span></span> <span data-ttu-id="fe036-126">Celkem se podporuje 120 jazyků.</span><span class="sxs-lookup"><span data-stu-id="fe036-126">A total of 120 languages are supported.</span></span>

## <a name="topic-detection"></a><span data-ttu-id="fe036-127">Detekce témat</span><span class="sxs-lookup"><span data-stu-id="fe036-127">Topic detection</span></span>
<span data-ttu-id="fe036-128">Toto je nově vydaných rozhraní API, která vrátí horní zjistil témata seznam odeslána text záznamy.</span><span class="sxs-lookup"><span data-stu-id="fe036-128">This is a newly released API which returns the top detected topics for a list of submitted text records.</span></span> <span data-ttu-id="fe036-129">Téma se určuje podle klíčové fráze, což může být jedno slovo nebo více souvisejících slov.</span><span class="sxs-lookup"><span data-stu-id="fe036-129">A topic is identified with a key phrase, which can be one or more related words.</span></span> <span data-ttu-id="fe036-130">Toto rozhraní API vyžaduje odeslání minimálně 100 textových záznamů, ale je navržené k detekování témat napříč stovkami tisíc záznamů.</span><span class="sxs-lookup"><span data-stu-id="fe036-130">This API requires a minimum of 100 text records to be submitted, but is designed to detect topics across hundreds to thousands of records.</span></span> <span data-ttu-id="fe036-131">Pamatujte, že toto rozhraní API účtuje 1 transakci za každý odeslaný textový záznam.</span><span class="sxs-lookup"><span data-stu-id="fe036-131">Note that this API charges 1 transaction per text record submitted.</span></span> <span data-ttu-id="fe036-132">Rozhraní API slouží k fungují dobře u krátký, lidské napsané text, například recenze a zpětnou vazbu od uživatelů.</span><span class="sxs-lookup"><span data-stu-id="fe036-132">The API is designed to work well for short, human written text such as reviews and user feedback.</span></span>

- - -
## <a name="api-definition"></a><span data-ttu-id="fe036-133">Definice rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fe036-133">API Definition</span></span>
### <a name="headers"></a><span data-ttu-id="fe036-134">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="fe036-134">Headers</span></span>
<span data-ttu-id="fe036-135">Ujistěte se, jestli jste zahrnuli správné hlavičky ve vaší žádosti, které by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="fe036-135">Ensure that you include the correct headers in your request, which should be as follows:</span></span>

    Authorization: Basic <creds>
    Accept: application/json

    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

<span data-ttu-id="fe036-136">Můžete najít klíč účtu z vašeho účtu v [Azure dat trhu](https://datamarket.azure.com/account/keys).</span><span class="sxs-lookup"><span data-stu-id="fe036-136">You can find your account key from your account in the [Azure Data Market](https://datamarket.azure.com/account/keys).</span></span> <span data-ttu-id="fe036-137">Všimněte si, že je aktuálně jedinou JSON přijímán pro vstupní a výstupní formáty.</span><span class="sxs-lookup"><span data-stu-id="fe036-137">Note that currently only JSON is accepted for input and output formats.</span></span> <span data-ttu-id="fe036-138">XML není podporován.</span><span class="sxs-lookup"><span data-stu-id="fe036-138">XML is not supported.</span></span>

- - -
## <a name="single-response-apis"></a><span data-ttu-id="fe036-139">Odpověď jedné rozhraní API</span><span class="sxs-lookup"><span data-stu-id="fe036-139">Single Response APIs</span></span>
### <a name="getsentiment"></a><span data-ttu-id="fe036-140">GetSentiment</span><span class="sxs-lookup"><span data-stu-id="fe036-140">GetSentiment</span></span>
<span data-ttu-id="fe036-141">**ADRESA URL**</span><span class="sxs-lookup"><span data-stu-id="fe036-141">**URL**</span></span>    

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment

<span data-ttu-id="fe036-142">**Příklad požadavku**</span><span class="sxs-lookup"><span data-stu-id="fe036-142">**Example request**</span></span>

<span data-ttu-id="fe036-143">Ve volání níže jsme žádají o postojích analýzy pro frázi "Hello World":</span><span class="sxs-lookup"><span data-stu-id="fe036-143">In the call below, we are requesting sentiment analysis for the phrase "Hello World":</span></span>

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment?Text=hello+world

<span data-ttu-id="fe036-144">Tato možnost vrátí odpověď následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fe036-144">This will return a response as follows:</span></span>

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
        "Score":1.0
    }

- - -
### <a name="getkeyphrases"></a><span data-ttu-id="fe036-145">GetKeyPhrases</span><span class="sxs-lookup"><span data-stu-id="fe036-145">GetKeyPhrases</span></span>
<span data-ttu-id="fe036-146">**ADRESA URL**</span><span class="sxs-lookup"><span data-stu-id="fe036-146">**URL**</span></span>

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases

<span data-ttu-id="fe036-147">**Příklad požadavku**</span><span class="sxs-lookup"><span data-stu-id="fe036-147">**Example request**</span></span>

<span data-ttu-id="fe036-148">Ve volání níže jsme žádají o klíč frází, naleznete v textu "Je vynikající hotelů zůstane v, s vnitřní jedinečný a popisný pracovníci":</span><span class="sxs-lookup"><span data-stu-id="fe036-148">In the call below, we are requesting the key phrases found in the text "It was a wonderful hotel to stay at, with unique decor and friendly staff":</span></span>

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases?
    Text=It+was+a+wonderful+hotel+to+stay+at,+with+unique+decor+and+friendly+staff

<span data-ttu-id="fe036-149">Tato možnost vrátí odpověď následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fe036-149">This will return a response as follows:</span></span>

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
      "KeyPhrases":[
        "wonderful hotel",
        "unique decor",
        "friendly staff"
      ]
    }

- - -
### <a name="getlanguage"></a><span data-ttu-id="fe036-150">GetLanguage</span><span class="sxs-lookup"><span data-stu-id="fe036-150">GetLanguage</span></span>
<span data-ttu-id="fe036-151">**ADRESA URL**</span><span class="sxs-lookup"><span data-stu-id="fe036-151">**URL**</span></span>

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguage

<span data-ttu-id="fe036-152">**Příklad požadavku**</span><span class="sxs-lookup"><span data-stu-id="fe036-152">**Example request**</span></span>

<span data-ttu-id="fe036-153">V níže uvedené volání GET jsme požaduje pro postojích pro klíče frází v textu *Hello World*</span><span class="sxs-lookup"><span data-stu-id="fe036-153">In the GET call below, we are requesting for the sentiment for the key phrases in the text *Hello World*</span></span>

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguages?
    Text=Hello+World

<span data-ttu-id="fe036-154">Tato možnost vrátí odpověď následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fe036-154">This will return a response as follows:</span></span>

    {
      "UnknownLanguage": false,
      "DetectedLanguages": [{
        "Name": "English",
        "Iso6391Name": "en",
        "Score": 1.0
      }]
    }

<span data-ttu-id="fe036-155">**Volitelné parametry**</span><span class="sxs-lookup"><span data-stu-id="fe036-155">**Optional parameters**</span></span>

<span data-ttu-id="fe036-156">`NumberOfLanguagesToDetect`je volitelný parametr.</span><span class="sxs-lookup"><span data-stu-id="fe036-156">`NumberOfLanguagesToDetect` is an optional parameter.</span></span> <span data-ttu-id="fe036-157">Výchozí hodnota je 1.</span><span class="sxs-lookup"><span data-stu-id="fe036-157">The default is 1.</span></span>

- - -
## <a name="batch-apis"></a><span data-ttu-id="fe036-158">Rozhraní API služby batch</span><span class="sxs-lookup"><span data-stu-id="fe036-158">Batch APIs</span></span>
<span data-ttu-id="fe036-159">Analýza textu služba umožňuje udělat postojích a klíč frázi extrakce v dávkovém režimu.</span><span class="sxs-lookup"><span data-stu-id="fe036-159">The Text Analytics service allows you to do sentiment and key-phrase extractions in batch mode.</span></span> <span data-ttu-id="fe036-160">Všimněte si, že každý záznam skóre počty jedné transakci.</span><span class="sxs-lookup"><span data-stu-id="fe036-160">Note that each of the records scored counts as one transaction.</span></span> <span data-ttu-id="fe036-161">Jako příklad Pokud si vyžádáte postojích 1000 záznamů v jediném volání 1000 transakce bude odečtena.</span><span class="sxs-lookup"><span data-stu-id="fe036-161">As an example, if you request sentiment for 1000 records in a single call, 1000 transactions will be deducted.</span></span>

<span data-ttu-id="fe036-162">Všimněte si, že zadané ID do systému jsou ID vrácené systému.</span><span class="sxs-lookup"><span data-stu-id="fe036-162">Note that the IDs entered into the system are the IDs returned by the system.</span></span> <span data-ttu-id="fe036-163">Webovou službu nekontroluje, že jsou tyto identifikátory jedinečné.</span><span class="sxs-lookup"><span data-stu-id="fe036-163">The web service does not check that these IDs are unique.</span></span> <span data-ttu-id="fe036-164">Je zodpovědností volajícího, aby ověření jedinečnosti.</span><span class="sxs-lookup"><span data-stu-id="fe036-164">It is the responsibility of the caller to verify uniqueness.</span></span> 

### <a name="getsentimentbatch"></a><span data-ttu-id="fe036-165">GetSentimentBatch</span><span class="sxs-lookup"><span data-stu-id="fe036-165">GetSentimentBatch</span></span>
<span data-ttu-id="fe036-166">**ADRESA URL**</span><span class="sxs-lookup"><span data-stu-id="fe036-166">**URL**</span></span>    

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch

<span data-ttu-id="fe036-167">**Příklad požadavku**</span><span class="sxs-lookup"><span data-stu-id="fe036-167">**Example request**</span></span>

<span data-ttu-id="fe036-168">Ve volání POST níže jsme požaduje pro chráněny frází "Hello World", "Foo Vítáme" a "Hello Moje World" v textu žádosti:</span><span class="sxs-lookup"><span data-stu-id="fe036-168">In the POST call below, we are requesting for the sentiments of the phrases "Hello World", "Hello Foo World" and "Hello My World" in the body of the request:</span></span>

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch 

<span data-ttu-id="fe036-169">Text žádosti:</span><span class="sxs-lookup"><span data-stu-id="fe036-169">Request body:</span></span>

    {"Inputs":
    [
        {"Id":"1","Text":"hello world"},
        {"Id":"2","Text":"hello foo world"},
        {"Id":"3","Text":"hello my world"},
    ]}

<span data-ttu-id="fe036-170">V níže uvedené odpovědi získat seznam skóre přidružené text ID:</span><span class="sxs-lookup"><span data-stu-id="fe036-170">In the response below, you get the list of scores associated with your text Ids:</span></span>

    {
      "odata.metadata":"<url>", 
      "SentimentBatch":
      [
        {"Score":0.9549767,"Id":"1"},
        {"Score":0.7767222,"Id":"2"},
        {"Score":0.8988889,"Id":"3"}
      ],  
      "Errors":[]
    }


- - -
### <a name="getkeyphrasesbatch"></a><span data-ttu-id="fe036-171">GetKeyPhrasesBatch</span><span class="sxs-lookup"><span data-stu-id="fe036-171">GetKeyPhrasesBatch</span></span>
<span data-ttu-id="fe036-172">**ADRESA URL**</span><span class="sxs-lookup"><span data-stu-id="fe036-172">**URL**</span></span>

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

<span data-ttu-id="fe036-173">**Příklad požadavku**</span><span class="sxs-lookup"><span data-stu-id="fe036-173">**Example request**</span></span>

<span data-ttu-id="fe036-174">V tomto příkladu jsme žádají o seznam chráněny pro klíče frází v těchto údajů:</span><span class="sxs-lookup"><span data-stu-id="fe036-174">In this example, we are requesting for the list of sentiments for the key phrases in the following texts:</span></span> 

* <span data-ttu-id="fe036-175">"Je vynikající hotelů zůstane v, s vnitřní jedinečný a popisný pracovníci"</span><span class="sxs-lookup"><span data-stu-id="fe036-175">"It was a wonderful hotel to stay at, with unique decor and friendly staff"</span></span>
* <span data-ttu-id="fe036-176">"Je úžasné konference build, s velmi zajímavé rozhovory"</span><span class="sxs-lookup"><span data-stu-id="fe036-176">"It was an amazing build conference, with very interesting talks"</span></span>
* <span data-ttu-id="fe036-177">"Provoz byl strašlivých, I stráví tři hodiny, přejdete na letišti"</span><span class="sxs-lookup"><span data-stu-id="fe036-177">"The traffic was terrible, I spent three hours going to the airport"</span></span>

<span data-ttu-id="fe036-178">Tato žádost jako volání POST do koncového bodu:</span><span class="sxs-lookup"><span data-stu-id="fe036-178">This request is made as a POST call to the endpoint:</span></span>

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

<span data-ttu-id="fe036-179">Text žádosti:</span><span class="sxs-lookup"><span data-stu-id="fe036-179">Request body:</span></span>

    {"Inputs":
    [
        {"Id":"1","Text":"It was a wonderful hotel to stay at, with unique decor and friendly staff"},
        {"Id":"2","Text":"It was an amazing build conference, with very interesting talks"},
        {"Id":"3","Text":"The traffic was terrible, I spent three hours going to the airport"}
    ]}

<span data-ttu-id="fe036-180">V odpovědi níže zobrazí se seznam klíčů frází přidružené text ID:</span><span class="sxs-lookup"><span data-stu-id="fe036-180">In the response below, you get the list of key phrases associated with your text Ids:</span></span>

    { "odata.metadata":"<url>",
         "KeyPhrasesBatch":
        [
           {"KeyPhrases":["unique decor","friendly staff","wonderful hotel"],"Id":"1"},
           {"KeyPhrases":["amazing build conference","interesting talks"],"Id":"2"},
           {"KeyPhrases":["hours","traffic","airport"],"Id":"3" }
        ],
        "Errors":[]
    }

- - -
### <a name="getlanguagebatch"></a><span data-ttu-id="fe036-181">GetLanguageBatch</span><span class="sxs-lookup"><span data-stu-id="fe036-181">GetLanguageBatch</span></span>

<span data-ttu-id="fe036-182">Ve volání POST níže jsme žádají o zjišťování jazyka pro dva vstupy text:</span><span class="sxs-lookup"><span data-stu-id="fe036-182">In the POST call below, we are requesting language detection for two text inputs:</span></span>

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguageBatch

<span data-ttu-id="fe036-183">Text žádosti:</span><span class="sxs-lookup"><span data-stu-id="fe036-183">Request body:</span></span>

    {
      "Inputs": [
        {"Text": "hello world", "Id": "1"},
        {"Text": "c'est la vie", "Id": "2"}
      ]
    }

<span data-ttu-id="fe036-184">Vrátí následující odpověď, kde je angličtina zjištěna v první vstup a francouzštinu v druhý vstup:</span><span class="sxs-lookup"><span data-stu-id="fe036-184">This returns the following response, where English is detected in the first input and French in the second input:</span></span>

    {
       "LanguageBatch": [{
         "Id": "1",
         "DetectedLanguages": [{
            "Name": "English",
            "Iso6391Name": "en",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       },
       {
         "Id": "2",
         "DetectedLanguages": [{
            "Name": "French",
            "Iso6391Name": "fr",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       }],
       "Errors": []
    }

- - -
## <a name="topic-detection-apis"></a><span data-ttu-id="fe036-185">Rozhraní API pro rozpoznávání tématu</span><span class="sxs-lookup"><span data-stu-id="fe036-185">Topic Detection APIs</span></span>
<span data-ttu-id="fe036-186">Toto je nově vydaných rozhraní API, která vrátí horní zjistil témata seznam odeslána text záznamy.</span><span class="sxs-lookup"><span data-stu-id="fe036-186">This is a newly released API which returns the top detected topics for a list of submitted text records.</span></span> <span data-ttu-id="fe036-187">Téma se určuje podle klíčové fráze, což může být jedno slovo nebo více souvisejících slov.</span><span class="sxs-lookup"><span data-stu-id="fe036-187">A topic is identified with a key phrase, which can be one or more related words.</span></span> <span data-ttu-id="fe036-188">Pamatujte, že toto rozhraní API účtuje 1 transakci za každý odeslaný textový záznam.</span><span class="sxs-lookup"><span data-stu-id="fe036-188">Note that this API charges 1 transaction per text record submitted.</span></span>

<span data-ttu-id="fe036-189">Toto rozhraní API vyžaduje odeslání minimálně 100 textových záznamů, ale je navržené k detekování témat napříč stovkami tisíc záznamů.</span><span class="sxs-lookup"><span data-stu-id="fe036-189">This API requires a minimum of 100 text records to be submitted, but is designed to detect topics across hundreds to thousands of records.</span></span>

### <a name="topics--submit-job"></a><span data-ttu-id="fe036-190">Témata – odeslání úlohy</span><span class="sxs-lookup"><span data-stu-id="fe036-190">Topics – Submit job</span></span>
<span data-ttu-id="fe036-191">**ADRESA URL**</span><span class="sxs-lookup"><span data-stu-id="fe036-191">**URL**</span></span>

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection

<span data-ttu-id="fe036-192">**Příklad požadavku**</span><span class="sxs-lookup"><span data-stu-id="fe036-192">**Example request**</span></span>

<span data-ttu-id="fe036-193">V níže uvedené volání POST jsme žádají o tématech sadu 100 články, kde jsou uvedeny v prvním a posledním vstupní článcích a dvě StopPhrases jsou zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="fe036-193">In the POST call below, we are requesting topics for a set of 100 articles, where the first and last input articles are shown, and two StopPhrases are included.</span></span>

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection HTTP/1.1

<span data-ttu-id="fe036-194">Text žádosti:</span><span class="sxs-lookup"><span data-stu-id="fe036-194">Request body:</span></span>

    {"Inputs":[
        {"Id":"1","Text":"I loved the food at this restaurant"},
        ...,
        {"Id":"100","Text":"I hated the decor"}
    ],
    "StopPhrases":[
        "restaurant", “visitor"
    ]}

<span data-ttu-id="fe036-195">V níže uvedené odpovědi získat JobId odeslaná úlohy:</span><span class="sxs-lookup"><span data-stu-id="fe036-195">In the response below, you get the JobId for the submitted job:</span></span>

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

<span data-ttu-id="fe036-196">Seznam jednoho slova či více fráze slovo, které by neměly být vrácen jako témata.</span><span class="sxs-lookup"><span data-stu-id="fe036-196">A list of single word or multiple word phrases which should not be returned as topics.</span></span> <span data-ttu-id="fe036-197">Mohou být použity k filtrování se velmi obecná témata.</span><span class="sxs-lookup"><span data-stu-id="fe036-197">Can be used to filter out very generic topics.</span></span> <span data-ttu-id="fe036-198">Například v datové sadě o hotelů recenze "ubytovací" a "hostel" může být rozumný zastavení frází.</span><span class="sxs-lookup"><span data-stu-id="fe036-198">For example, in a dataset about hotel reviews, "hotel" and "hostel" may be sensible stop phrases.</span></span>  

### <a name="topics--poll-for-job-results"></a><span data-ttu-id="fe036-199">Témata – dotazování pro výsledky úlohy</span><span class="sxs-lookup"><span data-stu-id="fe036-199">Topics – Poll for job results</span></span>
<span data-ttu-id="fe036-200">**ADRESA URL**</span><span class="sxs-lookup"><span data-stu-id="fe036-200">**URL**</span></span>

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult

<span data-ttu-id="fe036-201">**Příklad požadavku**</span><span class="sxs-lookup"><span data-stu-id="fe036-201">**Example request**</span></span>

<span data-ttu-id="fe036-202">Předejte JobId vrácená z kroku 'Odeslání úlohy' Chcete-li načíst výsledky.</span><span class="sxs-lookup"><span data-stu-id="fe036-202">Pass the JobId returned from the ‘Submit job’ step to fetch the results.</span></span> <span data-ttu-id="fe036-203">Doporučujeme, abyste volání tento koncový bod každou minutu až stav = 'Dokončit' v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="fe036-203">We recommend that you call this endpoint every minute until Status=’Complete’ in the response.</span></span> <span data-ttu-id="fe036-204">Dokončení nebo delší dobu, úlohy se mnoho tisíc záznamů bude trvat přibližně 10 minut pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="fe036-204">It will take around 10 mins for a job to complete, or longer for jobs with many thousands of records.</span></span>

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult?JobId=<JobId>


<span data-ttu-id="fe036-205">Během zpracování, odpověď bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="fe036-205">While it is processing, the response will be as follows:</span></span>

    {
        "odata.metadata":"<url>",
        "Status":"Running",
         "TopicInfo":[],
        "TopicAssignment":[],
        "Errors":[]
    }


<span data-ttu-id="fe036-206">Rozhraní API vrátí výstup ve formátu JSON v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="fe036-206">The API returns output in JSON format in the following format:</span></span>

    {
        "odata.metadata":"<url>",
        "Status":"Finished",
        "TopicInfo":[
        {
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Score":8.0,
            "KeyPhrase":"food"
        },
        ...
        {
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Score":6.0,
            "KeyPhrase":"decor"
            }
          ],
        "TopicAssignment":[
        {
            "Id":"1",
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Distance":0.7809
        },
        ...
        {
            "Id":"100",
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Distance":0.8034
        }
        ],
        "Errors":[]


<span data-ttu-id="fe036-207">Vlastnosti pro každou část odpovědi jsou následující:</span><span class="sxs-lookup"><span data-stu-id="fe036-207">The properties for each part of the response are as follows:</span></span>

<span data-ttu-id="fe036-208">**Vlastnosti TopicInfo**</span><span class="sxs-lookup"><span data-stu-id="fe036-208">**TopicInfo properties**</span></span>

| <span data-ttu-id="fe036-209">Klíč</span><span class="sxs-lookup"><span data-stu-id="fe036-209">Key</span></span> | <span data-ttu-id="fe036-210">Popis</span><span class="sxs-lookup"><span data-stu-id="fe036-210">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="fe036-211">TopicId</span><span class="sxs-lookup"><span data-stu-id="fe036-211">TopicId</span></span> |<span data-ttu-id="fe036-212">Jedinečný identifikátor každého tématu.</span><span class="sxs-lookup"><span data-stu-id="fe036-212">A unique identifier for each topic.</span></span> |
| <span data-ttu-id="fe036-213">Hodnocení</span><span class="sxs-lookup"><span data-stu-id="fe036-213">Score</span></span> |<span data-ttu-id="fe036-214">Počet záznamů, které jsou přiřazeny k tématu.</span><span class="sxs-lookup"><span data-stu-id="fe036-214">Count of records assigned to topic.</span></span> |
| <span data-ttu-id="fe036-215">KeyPhrase</span><span class="sxs-lookup"><span data-stu-id="fe036-215">KeyPhrase</span></span> |<span data-ttu-id="fe036-216">Summarizing slovo nebo frázi pro téma.</span><span class="sxs-lookup"><span data-stu-id="fe036-216">A summarizing word or phrase for the topic.</span></span> <span data-ttu-id="fe036-217">Může být 1 nebo více slova.</span><span class="sxs-lookup"><span data-stu-id="fe036-217">Can be 1 or multiple words.</span></span> |

<span data-ttu-id="fe036-218">**Vlastnosti TopicAssignment**</span><span class="sxs-lookup"><span data-stu-id="fe036-218">**TopicAssignment properties**</span></span>

| <span data-ttu-id="fe036-219">Klíč</span><span class="sxs-lookup"><span data-stu-id="fe036-219">Key</span></span> | <span data-ttu-id="fe036-220">Popis</span><span class="sxs-lookup"><span data-stu-id="fe036-220">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="fe036-221">ID</span><span class="sxs-lookup"><span data-stu-id="fe036-221">Id</span></span> |<span data-ttu-id="fe036-222">Identifikátor záznamu.</span><span class="sxs-lookup"><span data-stu-id="fe036-222">Identifier for the record.</span></span> <span data-ttu-id="fe036-223">Rovná ID součástí vstupu.</span><span class="sxs-lookup"><span data-stu-id="fe036-223">Equates to the ID included in the input.</span></span> |
| <span data-ttu-id="fe036-224">TopicId</span><span class="sxs-lookup"><span data-stu-id="fe036-224">TopicId</span></span> |<span data-ttu-id="fe036-225">ID tématu, která byla přiřazena záznamu.</span><span class="sxs-lookup"><span data-stu-id="fe036-225">The topic ID which the record has been assigned to.</span></span> |
| <span data-ttu-id="fe036-226">Vzdálenost</span><span class="sxs-lookup"><span data-stu-id="fe036-226">Distance</span></span> |<span data-ttu-id="fe036-227">Jistotu, že záznam patří do tématu.</span><span class="sxs-lookup"><span data-stu-id="fe036-227">Confidence that the record belongs to the topic.</span></span> <span data-ttu-id="fe036-228">Vzdálenost blíž nule označuje vyšší spolehlivosti.</span><span class="sxs-lookup"><span data-stu-id="fe036-228">Distance closer to zero indicates higher confidence.</span></span> |

