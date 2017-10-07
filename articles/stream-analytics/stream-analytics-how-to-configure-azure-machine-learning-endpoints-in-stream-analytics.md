---
title: "Koncové body Azure Machine Learning aaaUse v Stream Analytics | Microsoft Docs"
description: "Funkce definované uživatelem počítače jazyk v Stream Analytics"
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 406b258f-b8c2-4e55-953c-b7f84e8e5354
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 013b841ee85b1e0b6d8139a9ba0dde88fc3f8ad0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="machine-learning-integration-in-stream-analytics"></a>Počítač integrace učení v Stream Analytics
Stream Analytics podporuje uživatelsky definované funkce, které zdůrazňují tooAzure Machine Learning koncové body. Podpora rozhraní REST API pro tuto funkci je podrobně popsaná v hello [Stream Analytics REST API knihovny](https://msdn.microsoft.com/library/azure/dn835031.aspx). Tento článek obsahuje doplňující informace potřebné pro úspěšné dokončení implementace tuto funkci v Stream Analytics. Kurz také byl odeslán a je k dispozici [zde](stream-analytics-machine-learning-integration-tutorial.md).

## <a name="overview-azure-machine-learning-terminology"></a>Přehled: Terminologie služby Azure Machine Learning
Microsoft Azure Machine Learning poskytuje můžete použít nástroj pro spolupráci, přetažení myší toobuild, testovat a nasazovat řešení prediktivní analýzy na vaše data. Tento nástroj se nazývá hello *Azure Machine Learning Studio*. Hello studio je použité toointeract s hello prostředky Machine Learning a snadno vytvářet, testování a iterovat návrhu. Níže jsou tyto prostředky a jejich definice.

* **Pracovní prostor**: hello *prostoru* je kontejner, který obsahuje všechny ostatní prostředky Machine Learning společně v kontejneru pro správu a řízení.
* **Experiment**: *experimenty* jsou vytvořené pomocí dat vědců tooutilize datové sady a cvičení model machine learning.
* **Koncový bod**: *koncové body* jsou hello Azure Machine Learning funkce tootake objektu se používá jako vstup, používá model zadaný machine learning a vrátit scored výstup.
* **Vyhodnocování Webservice**: A *vyhodnocování webservice* je kolekcí koncových bodů, jak je uvedeno výše.

Každý koncový bod má rozhraní API pro spuštění dávky a synchronní zpracování. Stream Analytics používá synchronní zpracování. názvem Hello konkrétní službu [požadavků a odpovědí služby](../machine-learning/machine-learning-consume-web-services.md) v AzureML studio.

## <a name="machine-learning-resources-needed-for-stream-analytics-jobs"></a>Strojového učení prostředky potřebné pro úlohy Stream Analytics
Pro účely hello Stream Analytics úlohy zpracování, koncový bod žádosti a odpovědi [apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md), a všechny potřebné pro úspěšné provedení definici swaggeru. Stream Analytics obsahuje další koncový bod, který vytvoří hello url pro koncový bod swagger, vyhledá hello rozhraní a vrátí výchozí UDF definice toohello uživatele.

## <a name="configure-a-stream-analytics-and-machine-learning-udf-via-rest-api"></a>Konfigurace služby Stream Analytics a strojového učení UDF přes REST API
Pomocí rozhraní REST API můžete nakonfigurovat funkce Azure Machine jazyk toocall úlohy. Hello kroky jsou následující:

1. Vytvoření úlohy Stream Analytics
2. Zadejte vstup
3. Definování výstup
4. Vytvořit uživatelem definované funkce (UDF)
5. Zápis transformace Stream Analytics, že volání hello UDF
6. Spuštění úlohy hello

## <a name="creating-a-udf-with-basic-properties"></a>Vytváření uživatelem definovanou FUNKCI s základní vlastnosti
Jako příklad hello následující ukázkový kód vytvoří skalární uživatelem definovanou FUNKCI s názvem *newudf* který váže koncový bod Azure Machine Learning tooan. Všimněte si, že hello *koncový bod* (URI) se dá služba najít na stránce nápovědy hello rozhraní API pro hello vybrali služby a hello *apiKey* naleznete na hlavní stránce služeb hello.

````
    PUT : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>  
````

Příklad textu žádosti:  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77fb4b46bf2a30c63c078dca/services/b7be5e40fd194258796fb402c1958eaf/execute ",
                        "apiKey": "replacekeyhere"
                    }
                }
            }
        }
    }
````

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a>Koncový bod RetrieveDefaultDefinition volání pro výchozí UDF
Jednou hello kostru UDF se vytvoří kompletní definice hello hello je nutná UDF. koncový bod RetreiveDefaultDefinition Hello pomáhá vám hello výchozí definici pro skalární funkce, která je koncový bod Azure Machine Learning vázané tooan. datová část Hello níže vyžaduje tooget hello výchozí UDF definici pro skalární funkce, která je koncový bod Azure Machine Learning vázané tooan. Skutečný koncový bod hello neurčuje jak již bylo zadáno během žádosti PUT. Stream Analytics volá zadaný v požadavku hello, pokud je výslovně zadaný koncový bod hello. Jinak používá hello jeden původně odkazuje. Zde hello UDF trvá jednoho řetězce parametr (věta) a vrátí výstup jednoho typu řetězec, který označuje hello "postojích" popisku pro tuto větu.

````
POST : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>/RetrieveDefaultDefinition?api-version=<apiVersion>
````

Příklad textu žádosti:  

````
    {
        "bindingType": "Microsoft.MachineLearning/WebService",
        "bindingRetrievalProperties": {
            "executeEndpoint": null,
            "udfType": "Scalar"
        }
    }
````

Ukázka výstupu tohoto vyhledejte něco chtěli níže.  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="patch-udf-with-hello-response"></a>Oprava UDF hello odpovědi
Nyní hello UDF musí být opravit hello předchozí odpovědi, jak je uvedeno níže.

````
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
````

Text žádosti (výstup z RetrieveDefaultDefinition):

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="implement-stream-analytics-transformation-toocall-hello-udf"></a>Implementace služby Stream Analytics transformace toocall hello UDF
Nyní dotaz hello UDF (zde s názvem scoreTweet) pro každý vstupní událost a zápisu odpovědi pro tento výstup tooan událostí.  

````
    {
        "name": "transformation",
        "properties": {
            "streamingUnits": null,
            "query": "select *,scoreTweet(Tweet) TweetSentiment into blobOutput from blobInput"
        }
    }
````


## <a name="get-help"></a>Podpora
Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky
* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
