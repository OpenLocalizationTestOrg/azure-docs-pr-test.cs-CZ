---
title: "Rozhraní API pro Machine Learning: Analýza textu | Microsoft Docs"
description: "Rozhraní API společnosti Microsoft Machine Learning Text Analytics může být použité tooanalyze nestrukturovaných text pro analýzu postojích, klíče frázi extrakce, zjišťování jazyka a detekce tématu."
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
redirect_document_id: True
ms.openlocfilehash: 49380c83849c5d5fdd8dce4f3899ebcb3d6870f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="machine-learning-apis-text-analytics-for-sentiment-key-phrase-extraction-language-detection-and-topic-detection"></a><span data-ttu-id="14946-103">API pro Machine Learning: Analýza textu pro zjištění mínění, Extrakce klíčových frází, Rozpoznávání jazyka a Rozpoznávání témat</span><span class="sxs-lookup"><span data-stu-id="14946-103">Machine Learning APIs: Text Analytics for Sentiment, Key Phrase Extraction, Language Detection and Topic Detection</span></span>
> [!NOTE]
> <span data-ttu-id="14946-104">Tento průvodce je verze 1 hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="14946-104">This guide is for version 1 of hello API.</span></span> <span data-ttu-id="14946-105">Verze 2 [ **naleznete v dokumentu toothis**](../cognitive-services/cognitive-services-text-analytics-quick-start.md).</span><span class="sxs-lookup"><span data-stu-id="14946-105">For version 2, [**refer toothis document**](../cognitive-services/cognitive-services-text-analytics-quick-start.md).</span></span> <span data-ttu-id="14946-106">Verze 2 je nyní hello upřednostňované verzi toto rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="14946-106">Version 2 is now hello preferred version of this API.</span></span>
> 
> 

## <a name="overview"></a><span data-ttu-id="14946-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="14946-107">Overview</span></span>
<span data-ttu-id="14946-108">Hello Text Analytics API je sada Analýza textu [webové služby](https://datamarket.azure.com/dataset/amla/text-analytics) vytvořené s nástroji Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="14946-108">hello Text Analytics API is a suite of text analytics [web services](https://datamarket.azure.com/dataset/amla/text-analytics) built with Azure Machine Learning.</span></span> <span data-ttu-id="14946-109">Hello rozhraní API lze použít tooanalyze nestrukturovaných text pro úlohy, jako je postojích analýzy, klíče frázi extrakce, zjišťování jazyka a detekce tématu.</span><span class="sxs-lookup"><span data-stu-id="14946-109">hello API can be used tooanalyze unstructured text for tasks such as sentiment analysis, key phrase extraction, language detection and topic detection.</span></span> <span data-ttu-id="14946-110">Žádné školení dat je potřeba toouse toto rozhraní API: právě Oživte svoje data textu.</span><span class="sxs-lookup"><span data-stu-id="14946-110">No training data is needed toouse this API: just bring your text data.</span></span> <span data-ttu-id="14946-111">Toto rozhraní API používá pokročilé přirozeného jazyka zpracování techniky toodeliver nejlépe v třída předpovědi.</span><span class="sxs-lookup"><span data-stu-id="14946-111">This API uses advanced natural language processing techniques toodeliver best in class predictions.</span></span>

<span data-ttu-id="14946-112">Analýza textu v akci uvidíte na naše [ukázku lokality](https://text-analytics-demo.azurewebsites.net/), kde taky najdete [ukázky](https://text-analytics-demo.azurewebsites.net/Home/SampleCode) o tom, analýza textu tooimplement v C# a Python.</span><span class="sxs-lookup"><span data-stu-id="14946-112">You can see text analytics in action on our [demo site](https://text-analytics-demo.azurewebsites.net/), where you will also find [samples](https://text-analytics-demo.azurewebsites.net/Home/SampleCode) on how tooimplement text analytics in C# and Python.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

- - -
## <a name="sentiment-analysis"></a><span data-ttu-id="14946-113">Analýza subjektivního hodnocení</span><span class="sxs-lookup"><span data-stu-id="14946-113">Sentiment analysis</span></span>
<span data-ttu-id="14946-114">Hello rozhraní API vrátí číselný skóre mezi 0 a 1.</span><span class="sxs-lookup"><span data-stu-id="14946-114">hello API returns a numeric score between 0 & 1.</span></span> <span data-ttu-id="14946-115">Zavřít too1 skóre znamenat kladné postojích, zatímco zavřít too0 skóre znamenat záporné postojích.</span><span class="sxs-lookup"><span data-stu-id="14946-115">Scores close too1 indicate positive sentiment, while scores close too0 indicate negative sentiment.</span></span> <span data-ttu-id="14946-116">Hodnocení zabarvení se generuje pomocí klasifikačních technik.</span><span class="sxs-lookup"><span data-stu-id="14946-116">Sentiment score is generated using classification techniques.</span></span> <span data-ttu-id="14946-117">Klasifikátor toohello Hello vstupní funkce zahrnují n gram, funkce, které generují z části řeči značky a vkládaných aplikace word.</span><span class="sxs-lookup"><span data-stu-id="14946-117">hello input features toohello classifier include n-grams, features generated from part-of-speech tags, and word embeddings.</span></span> <span data-ttu-id="14946-118">V současné době je angličtina, že hello pouze podporovaný jazyk.</span><span class="sxs-lookup"><span data-stu-id="14946-118">Currently, English is hello only supported language.</span></span>

## <a name="key-phrase-extraction"></a><span data-ttu-id="14946-119">Extrakce klíčových frází</span><span class="sxs-lookup"><span data-stu-id="14946-119">Key phrase extraction</span></span>
<span data-ttu-id="14946-120">Hello rozhraní API vrátí seznam řetězců, které označuje hello klíčové čtených body vstupního textu hello.</span><span class="sxs-lookup"><span data-stu-id="14946-120">hello API returns a list of strings denoting hello key talking points in hello input text.</span></span> <span data-ttu-id="14946-121">Využíváme techniky z propracované sady nástrojů pro zpracování přirozeného jazyka v systému Microsoft Office.</span><span class="sxs-lookup"><span data-stu-id="14946-121">We employ techniques from Microsoft Office's sophisticated Natural Language Processing toolkit.</span></span> <span data-ttu-id="14946-122">V současné době je angličtina, že hello pouze podporovaný jazyk.</span><span class="sxs-lookup"><span data-stu-id="14946-122">Currently, English is hello only supported language.</span></span>

## <a name="language-detection"></a><span data-ttu-id="14946-123">Detekce jazyka</span><span class="sxs-lookup"><span data-stu-id="14946-123">Language detection</span></span>
<span data-ttu-id="14946-124">Hello rozhraní API vrátí hello zjistil jazyk a číselné skóre mezi 0 a 1.</span><span class="sxs-lookup"><span data-stu-id="14946-124">hello API returns hello detected language and a numeric score between 0 & 1.</span></span> <span data-ttu-id="14946-125">Zavřít too1 skóre znamenat 100 % jistoty platí hello identifikovat jazyk.</span><span class="sxs-lookup"><span data-stu-id="14946-125">Scores close too1 indicate 100% certainty that hello identified language is true.</span></span> <span data-ttu-id="14946-126">Celkem se podporuje 120 jazyků.</span><span class="sxs-lookup"><span data-stu-id="14946-126">A total of 120 languages are supported.</span></span>

## <a name="topic-detection"></a><span data-ttu-id="14946-127">Detekce témat</span><span class="sxs-lookup"><span data-stu-id="14946-127">Topic detection</span></span>
<span data-ttu-id="14946-128">Toto je nově vydaných rozhraní API, která vrátí hodnotu hello nejvyšší zjištěné tématech seznam záznamů odeslaná text.</span><span class="sxs-lookup"><span data-stu-id="14946-128">This is a newly released API which returns hello top detected topics for a list of submitted text records.</span></span> <span data-ttu-id="14946-129">Téma se určuje podle klíčové fráze, což může být jedno slovo nebo více souvisejících slov.</span><span class="sxs-lookup"><span data-stu-id="14946-129">A topic is identified with a key phrase, which can be one or more related words.</span></span> <span data-ttu-id="14946-130">Toto rozhraní API vyžaduje nejméně 100 text zaznamenává toobe odeslání, ale je navrženou toodetect témata na mnoha toothousands záznamů.</span><span class="sxs-lookup"><span data-stu-id="14946-130">This API requires a minimum of 100 text records toobe submitted, but is designed toodetect topics across hundreds toothousands of records.</span></span> <span data-ttu-id="14946-131">Pamatujte, že toto rozhraní API účtuje 1 transakci za každý odeslaný textový záznam.</span><span class="sxs-lookup"><span data-stu-id="14946-131">Note that this API charges 1 transaction per text record submitted.</span></span> <span data-ttu-id="14946-132">Hello rozhraní API je navrženou toowork dobře u short, lidské zapisovat text například recenze a zpětnou vazbu od uživatelů.</span><span class="sxs-lookup"><span data-stu-id="14946-132">hello API is designed toowork well for short, human written text such as reviews and user feedback.</span></span>

- - -
## <a name="api-definition"></a><span data-ttu-id="14946-133">Definice rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="14946-133">API Definition</span></span>
### <a name="headers"></a><span data-ttu-id="14946-134">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="14946-134">Headers</span></span>
<span data-ttu-id="14946-135">Ujistěte se, jestli jste zahrnuli správné hlavičky hello ve vaší žádosti, které by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="14946-135">Ensure that you include hello correct headers in your request, which should be as follows:</span></span>

    Authorization: Basic <creds>
    Accept: application/json

    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

<span data-ttu-id="14946-136">Klíče účtu z vašeho účtu můžete najít v hello [Azure dat trhu](https://datamarket.azure.com/account/keys).</span><span class="sxs-lookup"><span data-stu-id="14946-136">You can find your account key from your account in hello [Azure Data Market](https://datamarket.azure.com/account/keys).</span></span> <span data-ttu-id="14946-137">Všimněte si, že je aktuálně jedinou JSON přijímán pro vstupní a výstupní formáty.</span><span class="sxs-lookup"><span data-stu-id="14946-137">Note that currently only JSON is accepted for input and output formats.</span></span> <span data-ttu-id="14946-138">XML není podporován.</span><span class="sxs-lookup"><span data-stu-id="14946-138">XML is not supported.</span></span>

- - -
## <a name="single-response-apis"></a><span data-ttu-id="14946-139">Odpověď jedné rozhraní API</span><span class="sxs-lookup"><span data-stu-id="14946-139">Single Response APIs</span></span>
### <a name="getsentiment"></a><span data-ttu-id="14946-140">GetSentiment</span><span class="sxs-lookup"><span data-stu-id="14946-140">GetSentiment</span></span>
<span data-ttu-id="14946-141">**ADRESA URL**</span><span class="sxs-lookup"><span data-stu-id="14946-141">**URL**</span></span>    

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment

<span data-ttu-id="14946-142">**Příklad požadavku**</span><span class="sxs-lookup"><span data-stu-id="14946-142">**Example request**</span></span>

<span data-ttu-id="14946-143">Ve volání hello níže jsme žádají o postojích analýza hello frázi "Hello, World":</span><span class="sxs-lookup"><span data-stu-id="14946-143">In hello call below, we are requesting sentiment analysis for hello phrase "Hello World":</span></span>

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment?Text=hello+world

<span data-ttu-id="14946-144">Tato možnost vrátí odpověď následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="14946-144">This will return a response as follows:</span></span>

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
        "Score":1.0
    }

- - -
### <a name="getkeyphrases"></a><span data-ttu-id="14946-145">GetKeyPhrases</span><span class="sxs-lookup"><span data-stu-id="14946-145">GetKeyPhrases</span></span>
<span data-ttu-id="14946-146">**ADRESA URL**</span><span class="sxs-lookup"><span data-stu-id="14946-146">**URL**</span></span>

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases

<span data-ttu-id="14946-147">**Příklad požadavku**</span><span class="sxs-lookup"><span data-stu-id="14946-147">**Example request**</span></span>

<span data-ttu-id="14946-148">Ve volání hello níže jsme žádají o hello klíče frází, naleznete v textu hello "Je vynikající toostay hotelů v, s vnitřní jedinečný a popisný pracovníci":</span><span class="sxs-lookup"><span data-stu-id="14946-148">In hello call below, we are requesting hello key phrases found in hello text "It was a wonderful hotel toostay at, with unique decor and friendly staff":</span></span>

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases?
    Text=It+was+a+wonderful+hotel+to+stay+at,+with+unique+decor+and+friendly+staff

<span data-ttu-id="14946-149">Tato možnost vrátí odpověď následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="14946-149">This will return a response as follows:</span></span>

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
      "KeyPhrases":[
        "wonderful hotel",
        "unique decor",
        "friendly staff"
      ]
    }

- - -
### <a name="getlanguage"></a><span data-ttu-id="14946-150">GetLanguage</span><span class="sxs-lookup"><span data-stu-id="14946-150">GetLanguage</span></span>
<span data-ttu-id="14946-151">**ADRESA URL**</span><span class="sxs-lookup"><span data-stu-id="14946-151">**URL**</span></span>

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguage

<span data-ttu-id="14946-152">**Příklad požadavku**</span><span class="sxs-lookup"><span data-stu-id="14946-152">**Example request**</span></span>

<span data-ttu-id="14946-153">Ve volání GET hello níže, jsme požaduje pro hello postojích hello klíče vět v textu hello *Hello, World*</span><span class="sxs-lookup"><span data-stu-id="14946-153">In hello GET call below, we are requesting for hello sentiment for hello key phrases in hello text *Hello World*</span></span>

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguages?
    Text=Hello+World

<span data-ttu-id="14946-154">Tato možnost vrátí odpověď následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="14946-154">This will return a response as follows:</span></span>

    {
      "UnknownLanguage": false,
      "DetectedLanguages": [{
        "Name": "English",
        "Iso6391Name": "en",
        "Score": 1.0
      }]
    }

<span data-ttu-id="14946-155">**Volitelné parametry**</span><span class="sxs-lookup"><span data-stu-id="14946-155">**Optional parameters**</span></span>

<span data-ttu-id="14946-156">`NumberOfLanguagesToDetect`je volitelný parametr.</span><span class="sxs-lookup"><span data-stu-id="14946-156">`NumberOfLanguagesToDetect` is an optional parameter.</span></span> <span data-ttu-id="14946-157">Hello výchozí hodnota je 1.</span><span class="sxs-lookup"><span data-stu-id="14946-157">hello default is 1.</span></span>

- - -
## <a name="batch-apis"></a><span data-ttu-id="14946-158">Rozhraní API služby batch</span><span class="sxs-lookup"><span data-stu-id="14946-158">Batch APIs</span></span>
<span data-ttu-id="14946-159">Hello Analýza textu služby umožňuje toodo postojích a klíč frázi extrakce v dávkovém režimu.</span><span class="sxs-lookup"><span data-stu-id="14946-159">hello Text Analytics service allows you toodo sentiment and key-phrase extractions in batch mode.</span></span> <span data-ttu-id="14946-160">Všimněte si, že každý záznam hello skóre počty jedné transakci.</span><span class="sxs-lookup"><span data-stu-id="14946-160">Note that each of hello records scored counts as one transaction.</span></span> <span data-ttu-id="14946-161">Jako příklad Pokud si vyžádáte postojích 1000 záznamů v jediném volání 1000 transakce bude odečtena.</span><span class="sxs-lookup"><span data-stu-id="14946-161">As an example, if you request sentiment for 1000 records in a single call, 1000 transactions will be deducted.</span></span>

<span data-ttu-id="14946-162">Všimněte si, že zadané ID hello do systému hello jsou hello ID vrácené hello systému.</span><span class="sxs-lookup"><span data-stu-id="14946-162">Note that hello IDs entered into hello system are hello IDs returned by hello system.</span></span> <span data-ttu-id="14946-163">Hello webová služba nekontroluje, že jsou tyto identifikátory jedinečné.</span><span class="sxs-lookup"><span data-stu-id="14946-163">hello web service does not check that these IDs are unique.</span></span> <span data-ttu-id="14946-164">Je zodpovědností hello hello volající tooverify jedinečnost.</span><span class="sxs-lookup"><span data-stu-id="14946-164">It is hello responsibility of hello caller tooverify uniqueness.</span></span> 

### <a name="getsentimentbatch"></a><span data-ttu-id="14946-165">GetSentimentBatch</span><span class="sxs-lookup"><span data-stu-id="14946-165">GetSentimentBatch</span></span>
<span data-ttu-id="14946-166">**ADRESA URL**</span><span class="sxs-lookup"><span data-stu-id="14946-166">**URL**</span></span>    

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch

<span data-ttu-id="14946-167">**Příklad požadavku**</span><span class="sxs-lookup"><span data-stu-id="14946-167">**Example request**</span></span>

<span data-ttu-id="14946-168">V hello POST volejte níže, jsme požaduje pro hello chráněny frází hello "Hello, World", "Foo Hello, World" a "Hello Moje World" v textu hello hello žádosti:</span><span class="sxs-lookup"><span data-stu-id="14946-168">In hello POST call below, we are requesting for hello sentiments of hello phrases "Hello World", "Hello Foo World" and "Hello My World" in hello body of hello request:</span></span>

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch 

<span data-ttu-id="14946-169">Text žádosti:</span><span class="sxs-lookup"><span data-stu-id="14946-169">Request body:</span></span>

    {"Inputs":
    [
        {"Id":"1","Text":"hello world"},
        {"Id":"2","Text":"hello foo world"},
        {"Id":"3","Text":"hello my world"},
    ]}

<span data-ttu-id="14946-170">V odpovědi hello níže získat hello seznam skóre přidružené text ID:</span><span class="sxs-lookup"><span data-stu-id="14946-170">In hello response below, you get hello list of scores associated with your text Ids:</span></span>

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
### <a name="getkeyphrasesbatch"></a><span data-ttu-id="14946-171">GetKeyPhrasesBatch</span><span class="sxs-lookup"><span data-stu-id="14946-171">GetKeyPhrasesBatch</span></span>
<span data-ttu-id="14946-172">**ADRESA URL**</span><span class="sxs-lookup"><span data-stu-id="14946-172">**URL**</span></span>

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

<span data-ttu-id="14946-173">**Příklad požadavku**</span><span class="sxs-lookup"><span data-stu-id="14946-173">**Example request**</span></span>

<span data-ttu-id="14946-174">V tomto příkladu jsme žádají o hello seznam chráněny hello klíče vět v hello následující texty:</span><span class="sxs-lookup"><span data-stu-id="14946-174">In this example, we are requesting for hello list of sentiments for hello key phrases in hello following texts:</span></span> 

* <span data-ttu-id="14946-175">"Je vynikající toostay hotelů v, s vnitřní jedinečný a popisný pracovníci"</span><span class="sxs-lookup"><span data-stu-id="14946-175">"It was a wonderful hotel toostay at, with unique decor and friendly staff"</span></span>
* <span data-ttu-id="14946-176">"Je úžasné konference build, s velmi zajímavé rozhovory"</span><span class="sxs-lookup"><span data-stu-id="14946-176">"It was an amazing build conference, with very interesting talks"</span></span>
* <span data-ttu-id="14946-177">"hello provoz byl strašlivých, I stráví tři hodiny, budete toohello letiště"</span><span class="sxs-lookup"><span data-stu-id="14946-177">"hello traffic was terrible, I spent three hours going toohello airport"</span></span>

<span data-ttu-id="14946-178">Tento požadavek se provádí jako koncový bod toohello volání POST:</span><span class="sxs-lookup"><span data-stu-id="14946-178">This request is made as a POST call toohello endpoint:</span></span>

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

<span data-ttu-id="14946-179">Text žádosti:</span><span class="sxs-lookup"><span data-stu-id="14946-179">Request body:</span></span>

    {"Inputs":
    [
        {"Id":"1","Text":"It was a wonderful hotel toostay at, with unique decor and friendly staff"},
        {"Id":"2","Text":"It was an amazing build conference, with very interesting talks"},
        {"Id":"3","Text":"hello traffic was terrible, I spent three hours going toohello airport"}
    ]}

<span data-ttu-id="14946-180">V odpovědi hello níže zobrazí se seznam hello klíče frází přidružené text ID:</span><span class="sxs-lookup"><span data-stu-id="14946-180">In hello response below, you get hello list of key phrases associated with your text Ids:</span></span>

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
### <a name="getlanguagebatch"></a><span data-ttu-id="14946-181">GetLanguageBatch</span><span class="sxs-lookup"><span data-stu-id="14946-181">GetLanguageBatch</span></span>

<span data-ttu-id="14946-182">Ve volání POST hello níže jsme žádají o zjišťování jazyka pro dva vstupy text:</span><span class="sxs-lookup"><span data-stu-id="14946-182">In hello POST call below, we are requesting language detection for two text inputs:</span></span>

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguageBatch

<span data-ttu-id="14946-183">Text žádosti:</span><span class="sxs-lookup"><span data-stu-id="14946-183">Request body:</span></span>

    {
      "Inputs": [
        {"Text": "hello world", "Id": "1"},
        {"Text": "c'est la vie", "Id": "2"}
      ]
    }

<span data-ttu-id="14946-184">Tento příkaz vrátí hello následující odpověď, kde je angličtina zjištěna v hello první vstup a francouzštinu v hello druhý vstup:</span><span class="sxs-lookup"><span data-stu-id="14946-184">This returns hello following response, where English is detected in hello first input and French in hello second input:</span></span>

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
## <a name="topic-detection-apis"></a><span data-ttu-id="14946-185">Rozhraní API pro rozpoznávání tématu</span><span class="sxs-lookup"><span data-stu-id="14946-185">Topic Detection APIs</span></span>
<span data-ttu-id="14946-186">Toto je nově vydaných rozhraní API, která vrátí hodnotu hello nejvyšší zjištěné tématech seznam záznamů odeslaná text.</span><span class="sxs-lookup"><span data-stu-id="14946-186">This is a newly released API which returns hello top detected topics for a list of submitted text records.</span></span> <span data-ttu-id="14946-187">Téma se určuje podle klíčové fráze, což může být jedno slovo nebo více souvisejících slov.</span><span class="sxs-lookup"><span data-stu-id="14946-187">A topic is identified with a key phrase, which can be one or more related words.</span></span> <span data-ttu-id="14946-188">Pamatujte, že toto rozhraní API účtuje 1 transakci za každý odeslaný textový záznam.</span><span class="sxs-lookup"><span data-stu-id="14946-188">Note that this API charges 1 transaction per text record submitted.</span></span>

<span data-ttu-id="14946-189">Toto rozhraní API vyžaduje nejméně 100 text zaznamenává toobe odeslání, ale je navrženou toodetect témata na mnoha toothousands záznamů.</span><span class="sxs-lookup"><span data-stu-id="14946-189">This API requires a minimum of 100 text records toobe submitted, but is designed toodetect topics across hundreds toothousands of records.</span></span>

### <a name="topics--submit-job"></a><span data-ttu-id="14946-190">Témata – odeslání úlohy</span><span class="sxs-lookup"><span data-stu-id="14946-190">Topics – Submit job</span></span>
<span data-ttu-id="14946-191">**ADRESA URL**</span><span class="sxs-lookup"><span data-stu-id="14946-191">**URL**</span></span>

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection

<span data-ttu-id="14946-192">**Příklad požadavku**</span><span class="sxs-lookup"><span data-stu-id="14946-192">**Example request**</span></span>

<span data-ttu-id="14946-193">Ve volání POST hello níže jsme žádají o tématech sadu 100 články, kde hello první a poslední vstup se zobrazí články a dvě StopPhrases jsou zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="14946-193">In hello POST call below, we are requesting topics for a set of 100 articles, where hello first and last input articles are shown, and two StopPhrases are included.</span></span>

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection HTTP/1.1

<span data-ttu-id="14946-194">Text žádosti:</span><span class="sxs-lookup"><span data-stu-id="14946-194">Request body:</span></span>

    {"Inputs":[
        {"Id":"1","Text":"I loved hello food at this restaurant"},
        ...,
        {"Id":"100","Text":"I hated hello decor"}
    ],
    "StopPhrases":[
        "restaurant", “visitor"
    ]}

<span data-ttu-id="14946-195">V odpovědi hello níže získat hello JobId hello odeslaná úlohy:</span><span class="sxs-lookup"><span data-stu-id="14946-195">In hello response below, you get hello JobId for hello submitted job:</span></span>

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

<span data-ttu-id="14946-196">Seznam jednoho slova či více fráze slovo, které by neměly být vrácen jako témata.</span><span class="sxs-lookup"><span data-stu-id="14946-196">A list of single word or multiple word phrases which should not be returned as topics.</span></span> <span data-ttu-id="14946-197">Lze použít toofilter se velmi obecná témata.</span><span class="sxs-lookup"><span data-stu-id="14946-197">Can be used toofilter out very generic topics.</span></span> <span data-ttu-id="14946-198">Například v datové sadě o hotelů recenze "ubytovací" a "hostel" může být rozumný zastavení frází.</span><span class="sxs-lookup"><span data-stu-id="14946-198">For example, in a dataset about hotel reviews, "hotel" and "hostel" may be sensible stop phrases.</span></span>  

### <a name="topics--poll-for-job-results"></a><span data-ttu-id="14946-199">Témata – dotazování pro výsledky úlohy</span><span class="sxs-lookup"><span data-stu-id="14946-199">Topics – Poll for job results</span></span>
<span data-ttu-id="14946-200">**ADRESA URL**</span><span class="sxs-lookup"><span data-stu-id="14946-200">**URL**</span></span>

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult

<span data-ttu-id="14946-201">**Příklad požadavku**</span><span class="sxs-lookup"><span data-stu-id="14946-201">**Example request**</span></span>

<span data-ttu-id="14946-202">Předejte hello JobId vrácena z hello "Odeslání úlohy" krok toofetch hello výsledků.</span><span class="sxs-lookup"><span data-stu-id="14946-202">Pass hello JobId returned from hello ‘Submit job’ step toofetch hello results.</span></span> <span data-ttu-id="14946-203">Doporučujeme, abyste volání tento koncový bod každou minutu až do stavu v odpovědi hello = 'Dokončit'.</span><span class="sxs-lookup"><span data-stu-id="14946-203">We recommend that you call this endpoint every minute until Status=’Complete’ in hello response.</span></span> <span data-ttu-id="14946-204">To bude trvat přibližně 10 minut toocomplete úlohy nebo delší dobu, úlohy s mnoha tisíce záznamů.</span><span class="sxs-lookup"><span data-stu-id="14946-204">It will take around 10 mins for a job toocomplete, or longer for jobs with many thousands of records.</span></span>

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult?JobId=<JobId>


<span data-ttu-id="14946-205">Během zpracování, hello odpověď bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="14946-205">While it is processing, hello response will be as follows:</span></span>

    {
        "odata.metadata":"<url>",
        "Status":"Running",
         "TopicInfo":[],
        "TopicAssignment":[],
        "Errors":[]
    }


<span data-ttu-id="14946-206">Hello rozhraní API vrátí výstup ve formátu JSON v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="14946-206">hello API returns output in JSON format in hello following format:</span></span>

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


<span data-ttu-id="14946-207">Hello vlastnosti pro každou část hello odpovědi jsou následující:</span><span class="sxs-lookup"><span data-stu-id="14946-207">hello properties for each part of hello response are as follows:</span></span>

<span data-ttu-id="14946-208">**Vlastnosti TopicInfo**</span><span class="sxs-lookup"><span data-stu-id="14946-208">**TopicInfo properties**</span></span>

| <span data-ttu-id="14946-209">Klíč</span><span class="sxs-lookup"><span data-stu-id="14946-209">Key</span></span> | <span data-ttu-id="14946-210">Popis</span><span class="sxs-lookup"><span data-stu-id="14946-210">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="14946-211">TopicId</span><span class="sxs-lookup"><span data-stu-id="14946-211">TopicId</span></span> |<span data-ttu-id="14946-212">Jedinečný identifikátor každého tématu.</span><span class="sxs-lookup"><span data-stu-id="14946-212">A unique identifier for each topic.</span></span> |
| <span data-ttu-id="14946-213">Hodnocení</span><span class="sxs-lookup"><span data-stu-id="14946-213">Score</span></span> |<span data-ttu-id="14946-214">Počet záznamů přiřadit tootopic.</span><span class="sxs-lookup"><span data-stu-id="14946-214">Count of records assigned tootopic.</span></span> |
| <span data-ttu-id="14946-215">KeyPhrase</span><span class="sxs-lookup"><span data-stu-id="14946-215">KeyPhrase</span></span> |<span data-ttu-id="14946-216">Summarizing slovo nebo frázi pro téma hello.</span><span class="sxs-lookup"><span data-stu-id="14946-216">A summarizing word or phrase for hello topic.</span></span> <span data-ttu-id="14946-217">Může být 1 nebo více slova.</span><span class="sxs-lookup"><span data-stu-id="14946-217">Can be 1 or multiple words.</span></span> |

<span data-ttu-id="14946-218">**Vlastnosti TopicAssignment**</span><span class="sxs-lookup"><span data-stu-id="14946-218">**TopicAssignment properties**</span></span>

| <span data-ttu-id="14946-219">Klíč</span><span class="sxs-lookup"><span data-stu-id="14946-219">Key</span></span> | <span data-ttu-id="14946-220">Popis</span><span class="sxs-lookup"><span data-stu-id="14946-220">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="14946-221">ID</span><span class="sxs-lookup"><span data-stu-id="14946-221">Id</span></span> |<span data-ttu-id="14946-222">Identifikátor záznamu hello.</span><span class="sxs-lookup"><span data-stu-id="14946-222">Identifier for hello record.</span></span> <span data-ttu-id="14946-223">Znamená zároveň toohello ID součástí hello vstup.</span><span class="sxs-lookup"><span data-stu-id="14946-223">Equates toohello ID included in hello input.</span></span> |
| <span data-ttu-id="14946-224">TopicId</span><span class="sxs-lookup"><span data-stu-id="14946-224">TopicId</span></span> |<span data-ttu-id="14946-225">byl přiřazen ID Hello téma, které hello záznamu.</span><span class="sxs-lookup"><span data-stu-id="14946-225">hello topic ID which hello record has been assigned to.</span></span> |
| <span data-ttu-id="14946-226">Vzdálenost</span><span class="sxs-lookup"><span data-stu-id="14946-226">Distance</span></span> |<span data-ttu-id="14946-227">Jistotu, že záznam hello patří toohello tématu.</span><span class="sxs-lookup"><span data-stu-id="14946-227">Confidence that hello record belongs toohello topic.</span></span> <span data-ttu-id="14946-228">Vzdálenost blíže toozero označuje vyšší spolehlivosti.</span><span class="sxs-lookup"><span data-stu-id="14946-228">Distance closer toozero indicates higher confidence.</span></span> |

