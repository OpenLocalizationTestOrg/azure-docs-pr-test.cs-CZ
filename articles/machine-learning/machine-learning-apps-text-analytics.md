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
# <a name="machine-learning-apis-text-analytics-for-sentiment-key-phrase-extraction-language-detection-and-topic-detection"></a>API pro Machine Learning: Analýza textu pro zjištění mínění, Extrakce klíčových frází, Rozpoznávání jazyka a Rozpoznávání témat
> [!NOTE]
> Tento průvodce je pro rozhraní API verze 1. Verze 2 [ **odkazovat na tento dokument**](../cognitive-services/cognitive-services-text-analytics-quick-start.md). Verze 2 je nyní upřednostňované verzi toto rozhraní API.
> 
> 

## <a name="overview"></a>Přehled
Rozhraní API Analytics textu je sada Analýza textu [webové služby](https://datamarket.azure.com/dataset/amla/text-analytics) vytvořené s nástroji Azure Machine Learning. Rozhraní API slouží k analýze nestrukturovaných text pro úlohy, jako je postojích analýzy, klíče frázi extrakce, zjišťování jazyka a detekce tématu. Žádná Cvičná data je potřeba k použití toto rozhraní API: právě Oživte svoje data textu. Toto rozhraní API používá pokročilé přirozeného jazyka zpracování techniky dodávat nejlépe ve třída předpovědi.

Analýza textu v akci uvidíte na našich [ukázku lokality](https://text-analytics-demo.azurewebsites.net/), kde taky najdete [ukázky](https://text-analytics-demo.azurewebsites.net/Home/SampleCode) o tom, jak implementovat Analýza textu v C# a Python.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

- - -
## <a name="sentiment-analysis"></a>Analýza subjektivního hodnocení
Rozhraní API vrátí číselný skóre mezi 0 a 1. Skóre blíží 1 znamenat kladné postojích, zatímco skóre blízko 0 označuje záporné postojích. Hodnocení zabarvení se generuje pomocí klasifikačních technik. Vstupní funkce na třídění zahrnují n gram, funkce, které generují z části řeči značky a vkládaných aplikace word. V současné době pouze podporovaných jazykem je angličtina.

## <a name="key-phrase-extraction"></a>Extrakce klíčových frází
Rozhraní API vrátí seznam řetězců označujících klíčová témata ve vstupním textu. Využíváme techniky z propracované sady nástrojů pro zpracování přirozeného jazyka v systému Microsoft Office. V současné době pouze podporovaných jazykem je angličtina.

## <a name="language-detection"></a>Detekce jazyka
Rozhraní API vrátí zjištěný jazyk a číselné skóre mezi 0 a 1. Hodnocení blížící se 1 značí 100% jistotu správné identifikace jazyka. Celkem se podporuje 120 jazyků.

## <a name="topic-detection"></a>Detekce témat
Toto je nově vydaných rozhraní API, která vrátí horní zjistil témata seznam odeslána text záznamy. Téma se určuje podle klíčové fráze, což může být jedno slovo nebo více souvisejících slov. Toto rozhraní API vyžaduje odeslání minimálně 100 textových záznamů, ale je navržené k detekování témat napříč stovkami tisíc záznamů. Pamatujte, že toto rozhraní API účtuje 1 transakci za každý odeslaný textový záznam. Rozhraní API slouží k fungují dobře u krátký, lidské napsané text, například recenze a zpětnou vazbu od uživatelů.

- - -
## <a name="api-definition"></a>Definice rozhraní API.
### <a name="headers"></a>Záhlaví
Ujistěte se, jestli jste zahrnuli správné hlavičky ve vaší žádosti, které by měl vypadat takto:

    Authorization: Basic <creds>
    Accept: application/json

    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

Můžete najít klíč účtu z vašeho účtu v [Azure dat trhu](https://datamarket.azure.com/account/keys). Všimněte si, že je aktuálně jedinou JSON přijímán pro vstupní a výstupní formáty. XML není podporován.

- - -
## <a name="single-response-apis"></a>Odpověď jedné rozhraní API
### <a name="getsentiment"></a>GetSentiment
**ADRESA URL**    

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment

**Příklad požadavku**

Ve volání níže jsme žádají o postojích analýzy pro frázi "Hello World":

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment?Text=hello+world

Tato možnost vrátí odpověď následujícím způsobem:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
        "Score":1.0
    }

- - -
### <a name="getkeyphrases"></a>GetKeyPhrases
**ADRESA URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases

**Příklad požadavku**

Ve volání níže jsme žádají o klíč frází, naleznete v textu "Je vynikající hotelů zůstane v, s vnitřní jedinečný a popisný pracovníci":

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases?
    Text=It+was+a+wonderful+hotel+to+stay+at,+with+unique+decor+and+friendly+staff

Tato možnost vrátí odpověď následujícím způsobem:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
      "KeyPhrases":[
        "wonderful hotel",
        "unique decor",
        "friendly staff"
      ]
    }

- - -
### <a name="getlanguage"></a>GetLanguage
**ADRESA URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguage

**Příklad požadavku**

V níže uvedené volání GET jsme požaduje pro postojích pro klíče frází v textu *Hello World*

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguages?
    Text=Hello+World

Tato možnost vrátí odpověď následujícím způsobem:

    {
      "UnknownLanguage": false,
      "DetectedLanguages": [{
        "Name": "English",
        "Iso6391Name": "en",
        "Score": 1.0
      }]
    }

**Volitelné parametry**

`NumberOfLanguagesToDetect`je volitelný parametr. Výchozí hodnota je 1.

- - -
## <a name="batch-apis"></a>Rozhraní API služby batch
Analýza textu služba umožňuje udělat postojích a klíč frázi extrakce v dávkovém režimu. Všimněte si, že každý záznam skóre počty jedné transakci. Jako příklad Pokud si vyžádáte postojích 1000 záznamů v jediném volání 1000 transakce bude odečtena.

Všimněte si, že zadané ID do systému jsou ID vrácené systému. Webovou službu nekontroluje, že jsou tyto identifikátory jedinečné. Je zodpovědností volajícího, aby ověření jedinečnosti. 

### <a name="getsentimentbatch"></a>GetSentimentBatch
**ADRESA URL**    

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch

**Příklad požadavku**

Ve volání POST níže jsme požaduje pro chráněny frází "Hello World", "Foo Vítáme" a "Hello Moje World" v textu žádosti:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch 

Text žádosti:

    {"Inputs":
    [
        {"Id":"1","Text":"hello world"},
        {"Id":"2","Text":"hello foo world"},
        {"Id":"3","Text":"hello my world"},
    ]}

V níže uvedené odpovědi získat seznam skóre přidružené text ID:

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
### <a name="getkeyphrasesbatch"></a>GetKeyPhrasesBatch
**ADRESA URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

**Příklad požadavku**

V tomto příkladu jsme žádají o seznam chráněny pro klíče frází v těchto údajů: 

* "Je vynikající hotelů zůstane v, s vnitřní jedinečný a popisný pracovníci"
* "Je úžasné konference build, s velmi zajímavé rozhovory"
* "Provoz byl strašlivých, I stráví tři hodiny, přejdete na letišti"

Tato žádost jako volání POST do koncového bodu:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

Text žádosti:

    {"Inputs":
    [
        {"Id":"1","Text":"It was a wonderful hotel to stay at, with unique decor and friendly staff"},
        {"Id":"2","Text":"It was an amazing build conference, with very interesting talks"},
        {"Id":"3","Text":"The traffic was terrible, I spent three hours going to the airport"}
    ]}

V odpovědi níže zobrazí se seznam klíčů frází přidružené text ID:

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
### <a name="getlanguagebatch"></a>GetLanguageBatch

Ve volání POST níže jsme žádají o zjišťování jazyka pro dva vstupy text:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguageBatch

Text žádosti:

    {
      "Inputs": [
        {"Text": "hello world", "Id": "1"},
        {"Text": "c'est la vie", "Id": "2"}
      ]
    }

Vrátí následující odpověď, kde je angličtina zjištěna v první vstup a francouzštinu v druhý vstup:

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
## <a name="topic-detection-apis"></a>Rozhraní API pro rozpoznávání tématu
Toto je nově vydaných rozhraní API, která vrátí horní zjistil témata seznam odeslána text záznamy. Téma se určuje podle klíčové fráze, což může být jedno slovo nebo více souvisejících slov. Pamatujte, že toto rozhraní API účtuje 1 transakci za každý odeslaný textový záznam.

Toto rozhraní API vyžaduje odeslání minimálně 100 textových záznamů, ale je navržené k detekování témat napříč stovkami tisíc záznamů.

### <a name="topics--submit-job"></a>Témata – odeslání úlohy
**ADRESA URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection

**Příklad požadavku**

V níže uvedené volání POST jsme žádají o tématech sadu 100 články, kde jsou uvedeny v prvním a posledním vstupní článcích a dvě StopPhrases jsou zahrnuty.

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection HTTP/1.1

Text žádosti:

    {"Inputs":[
        {"Id":"1","Text":"I loved the food at this restaurant"},
        ...,
        {"Id":"100","Text":"I hated the decor"}
    ],
    "StopPhrases":[
        "restaurant", “visitor"
    ]}

V níže uvedené odpovědi získat JobId odeslaná úlohy:

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

Seznam jednoho slova či více fráze slovo, které by neměly být vrácen jako témata. Mohou být použity k filtrování se velmi obecná témata. Například v datové sadě o hotelů recenze "ubytovací" a "hostel" může být rozumný zastavení frází.  

### <a name="topics--poll-for-job-results"></a>Témata – dotazování pro výsledky úlohy
**ADRESA URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult

**Příklad požadavku**

Předejte JobId vrácená z kroku 'Odeslání úlohy' Chcete-li načíst výsledky. Doporučujeme, abyste volání tento koncový bod každou minutu až stav = 'Dokončit' v odpovědi. Dokončení nebo delší dobu, úlohy se mnoho tisíc záznamů bude trvat přibližně 10 minut pro úlohu.

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult?JobId=<JobId>


Během zpracování, odpověď bude vypadat takto:

    {
        "odata.metadata":"<url>",
        "Status":"Running",
         "TopicInfo":[],
        "TopicAssignment":[],
        "Errors":[]
    }


Rozhraní API vrátí výstup ve formátu JSON v následujícím formátu:

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


Vlastnosti pro každou část odpovědi jsou následující:

**Vlastnosti TopicInfo**

| Klíč | Popis |
|:--- |:--- |
| TopicId |Jedinečný identifikátor každého tématu. |
| Hodnocení |Počet záznamů, které jsou přiřazeny k tématu. |
| KeyPhrase |Summarizing slovo nebo frázi pro téma. Může být 1 nebo více slova. |

**Vlastnosti TopicAssignment**

| Klíč | Popis |
|:--- |:--- |
| ID |Identifikátor záznamu. Rovná ID součástí vstupu. |
| TopicId |ID tématu, která byla přiřazena záznamu. |
| Vzdálenost |Jistotu, že záznam patří do tématu. Vzdálenost blíž nule označuje vyšší spolehlivosti. |

