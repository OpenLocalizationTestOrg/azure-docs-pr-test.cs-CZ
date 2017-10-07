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
# <a name="machine-learning-apis-text-analytics-for-sentiment-key-phrase-extraction-language-detection-and-topic-detection"></a>API pro Machine Learning: Analýza textu pro zjištění mínění, Extrakce klíčových frází, Rozpoznávání jazyka a Rozpoznávání témat
> [!NOTE]
> Tento průvodce je verze 1 hello rozhraní API. Verze 2 [ **naleznete v dokumentu toothis**](../cognitive-services/cognitive-services-text-analytics-quick-start.md). Verze 2 je nyní hello upřednostňované verzi toto rozhraní API.
> 
> 

## <a name="overview"></a>Přehled
Hello Text Analytics API je sada Analýza textu [webové služby](https://datamarket.azure.com/dataset/amla/text-analytics) vytvořené s nástroji Azure Machine Learning. Hello rozhraní API lze použít tooanalyze nestrukturovaných text pro úlohy, jako je postojích analýzy, klíče frázi extrakce, zjišťování jazyka a detekce tématu. Žádné školení dat je potřeba toouse toto rozhraní API: právě Oživte svoje data textu. Toto rozhraní API používá pokročilé přirozeného jazyka zpracování techniky toodeliver nejlépe v třída předpovědi.

Analýza textu v akci uvidíte na naše [ukázku lokality](https://text-analytics-demo.azurewebsites.net/), kde taky najdete [ukázky](https://text-analytics-demo.azurewebsites.net/Home/SampleCode) o tom, analýza textu tooimplement v C# a Python.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

- - -
## <a name="sentiment-analysis"></a>Analýza subjektivního hodnocení
Hello rozhraní API vrátí číselný skóre mezi 0 a 1. Zavřít too1 skóre znamenat kladné postojích, zatímco zavřít too0 skóre znamenat záporné postojích. Hodnocení zabarvení se generuje pomocí klasifikačních technik. Klasifikátor toohello Hello vstupní funkce zahrnují n gram, funkce, které generují z části řeči značky a vkládaných aplikace word. V současné době je angličtina, že hello pouze podporovaný jazyk.

## <a name="key-phrase-extraction"></a>Extrakce klíčových frází
Hello rozhraní API vrátí seznam řetězců, které označuje hello klíčové čtených body vstupního textu hello. Využíváme techniky z propracované sady nástrojů pro zpracování přirozeného jazyka v systému Microsoft Office. V současné době je angličtina, že hello pouze podporovaný jazyk.

## <a name="language-detection"></a>Detekce jazyka
Hello rozhraní API vrátí hello zjistil jazyk a číselné skóre mezi 0 a 1. Zavřít too1 skóre znamenat 100 % jistoty platí hello identifikovat jazyk. Celkem se podporuje 120 jazyků.

## <a name="topic-detection"></a>Detekce témat
Toto je nově vydaných rozhraní API, která vrátí hodnotu hello nejvyšší zjištěné tématech seznam záznamů odeslaná text. Téma se určuje podle klíčové fráze, což může být jedno slovo nebo více souvisejících slov. Toto rozhraní API vyžaduje nejméně 100 text zaznamenává toobe odeslání, ale je navrženou toodetect témata na mnoha toothousands záznamů. Pamatujte, že toto rozhraní API účtuje 1 transakci za každý odeslaný textový záznam. Hello rozhraní API je navrženou toowork dobře u short, lidské zapisovat text například recenze a zpětnou vazbu od uživatelů.

- - -
## <a name="api-definition"></a>Definice rozhraní API.
### <a name="headers"></a>Záhlaví
Ujistěte se, jestli jste zahrnuli správné hlavičky hello ve vaší žádosti, které by měl vypadat takto:

    Authorization: Basic <creds>
    Accept: application/json

    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

Klíče účtu z vašeho účtu můžete najít v hello [Azure dat trhu](https://datamarket.azure.com/account/keys). Všimněte si, že je aktuálně jedinou JSON přijímán pro vstupní a výstupní formáty. XML není podporován.

- - -
## <a name="single-response-apis"></a>Odpověď jedné rozhraní API
### <a name="getsentiment"></a>GetSentiment
**ADRESA URL**    

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment

**Příklad požadavku**

Ve volání hello níže jsme žádají o postojích analýza hello frázi "Hello, World":

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

Ve volání hello níže jsme žádají o hello klíče frází, naleznete v textu hello "Je vynikající toostay hotelů v, s vnitřní jedinečný a popisný pracovníci":

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

Ve volání GET hello níže, jsme požaduje pro hello postojích hello klíče vět v textu hello *Hello, World*

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

`NumberOfLanguagesToDetect`je volitelný parametr. Hello výchozí hodnota je 1.

- - -
## <a name="batch-apis"></a>Rozhraní API služby batch
Hello Analýza textu služby umožňuje toodo postojích a klíč frázi extrakce v dávkovém režimu. Všimněte si, že každý záznam hello skóre počty jedné transakci. Jako příklad Pokud si vyžádáte postojích 1000 záznamů v jediném volání 1000 transakce bude odečtena.

Všimněte si, že zadané ID hello do systému hello jsou hello ID vrácené hello systému. Hello webová služba nekontroluje, že jsou tyto identifikátory jedinečné. Je zodpovědností hello hello volající tooverify jedinečnost. 

### <a name="getsentimentbatch"></a>GetSentimentBatch
**ADRESA URL**    

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch

**Příklad požadavku**

V hello POST volejte níže, jsme požaduje pro hello chráněny frází hello "Hello, World", "Foo Hello, World" a "Hello Moje World" v textu hello hello žádosti:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch 

Text žádosti:

    {"Inputs":
    [
        {"Id":"1","Text":"hello world"},
        {"Id":"2","Text":"hello foo world"},
        {"Id":"3","Text":"hello my world"},
    ]}

V odpovědi hello níže získat hello seznam skóre přidružené text ID:

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

V tomto příkladu jsme žádají o hello seznam chráněny hello klíče vět v hello následující texty: 

* "Je vynikající toostay hotelů v, s vnitřní jedinečný a popisný pracovníci"
* "Je úžasné konference build, s velmi zajímavé rozhovory"
* "hello provoz byl strašlivých, I stráví tři hodiny, budete toohello letiště"

Tento požadavek se provádí jako koncový bod toohello volání POST:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

Text žádosti:

    {"Inputs":
    [
        {"Id":"1","Text":"It was a wonderful hotel toostay at, with unique decor and friendly staff"},
        {"Id":"2","Text":"It was an amazing build conference, with very interesting talks"},
        {"Id":"3","Text":"hello traffic was terrible, I spent three hours going toohello airport"}
    ]}

V odpovědi hello níže zobrazí se seznam hello klíče frází přidružené text ID:

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

Ve volání POST hello níže jsme žádají o zjišťování jazyka pro dva vstupy text:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguageBatch

Text žádosti:

    {
      "Inputs": [
        {"Text": "hello world", "Id": "1"},
        {"Text": "c'est la vie", "Id": "2"}
      ]
    }

Tento příkaz vrátí hello následující odpověď, kde je angličtina zjištěna v hello první vstup a francouzštinu v hello druhý vstup:

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
Toto je nově vydaných rozhraní API, která vrátí hodnotu hello nejvyšší zjištěné tématech seznam záznamů odeslaná text. Téma se určuje podle klíčové fráze, což může být jedno slovo nebo více souvisejících slov. Pamatujte, že toto rozhraní API účtuje 1 transakci za každý odeslaný textový záznam.

Toto rozhraní API vyžaduje nejméně 100 text zaznamenává toobe odeslání, ale je navrženou toodetect témata na mnoha toothousands záznamů.

### <a name="topics--submit-job"></a>Témata – odeslání úlohy
**ADRESA URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection

**Příklad požadavku**

Ve volání POST hello níže jsme žádají o tématech sadu 100 články, kde hello první a poslední vstup se zobrazí články a dvě StopPhrases jsou zahrnuty.

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection HTTP/1.1

Text žádosti:

    {"Inputs":[
        {"Id":"1","Text":"I loved hello food at this restaurant"},
        ...,
        {"Id":"100","Text":"I hated hello decor"}
    ],
    "StopPhrases":[
        "restaurant", “visitor"
    ]}

V odpovědi hello níže získat hello JobId hello odeslaná úlohy:

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

Seznam jednoho slova či více fráze slovo, které by neměly být vrácen jako témata. Lze použít toofilter se velmi obecná témata. Například v datové sadě o hotelů recenze "ubytovací" a "hostel" může být rozumný zastavení frází.  

### <a name="topics--poll-for-job-results"></a>Témata – dotazování pro výsledky úlohy
**ADRESA URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult

**Příklad požadavku**

Předejte hello JobId vrácena z hello "Odeslání úlohy" krok toofetch hello výsledků. Doporučujeme, abyste volání tento koncový bod každou minutu až do stavu v odpovědi hello = 'Dokončit'. To bude trvat přibližně 10 minut toocomplete úlohy nebo delší dobu, úlohy s mnoha tisíce záznamů.

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult?JobId=<JobId>


Během zpracování, hello odpověď bude vypadat takto:

    {
        "odata.metadata":"<url>",
        "Status":"Running",
         "TopicInfo":[],
        "TopicAssignment":[],
        "Errors":[]
    }


Hello rozhraní API vrátí výstup ve formátu JSON v hello následující formát:

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


Hello vlastnosti pro každou část hello odpovědi jsou následující:

**Vlastnosti TopicInfo**

| Klíč | Popis |
|:--- |:--- |
| TopicId |Jedinečný identifikátor každého tématu. |
| Hodnocení |Počet záznamů přiřadit tootopic. |
| KeyPhrase |Summarizing slovo nebo frázi pro téma hello. Může být 1 nebo více slova. |

**Vlastnosti TopicAssignment**

| Klíč | Popis |
|:--- |:--- |
| ID |Identifikátor záznamu hello. Znamená zároveň toohello ID součástí hello vstup. |
| TopicId |byl přiřazen ID Hello téma, které hello záznamu. |
| Vzdálenost |Jistotu, že záznam hello patří toohello tématu. Vzdálenost blíže toozero označuje vyšší spolehlivosti. |

