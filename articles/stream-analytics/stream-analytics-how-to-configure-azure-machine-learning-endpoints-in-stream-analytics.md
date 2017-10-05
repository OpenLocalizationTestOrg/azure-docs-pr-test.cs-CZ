---
title: "Pomocí Azure Machine Learning koncové body v Stream Analytics | Microsoft Docs"
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
ms.openlocfilehash: d3a46190dd802bf31ea03ef38304d58e6e63b66d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="machine-learning-integration-in-stream-analytics"></a><span data-ttu-id="feacd-103">Počítač integrace učení v Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="feacd-103">Machine Learning integration in Stream Analytics</span></span>
<span data-ttu-id="feacd-104">Stream Analytics podporuje uživatelsky definované funkce, které volají na koncové body Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="feacd-104">Stream Analytics supports user-defined functions that call out to Azure Machine Learning endpoints.</span></span> <span data-ttu-id="feacd-105">Podpora rozhraní REST API pro tuto funkci je podrobně popsaná v [Stream Analytics REST API knihovny](https://msdn.microsoft.com/library/azure/dn835031.aspx).</span><span class="sxs-lookup"><span data-stu-id="feacd-105">REST API support for this feature is detailed in the [Stream Analytics REST API library](https://msdn.microsoft.com/library/azure/dn835031.aspx).</span></span> <span data-ttu-id="feacd-106">Tento článek obsahuje doplňující informace potřebné pro úspěšné dokončení implementace tuto funkci v Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="feacd-106">This article provides supplemental information needed for successful implementation of this capability in Stream Analytics.</span></span> <span data-ttu-id="feacd-107">Kurz také byl odeslán a je k dispozici [zde](stream-analytics-machine-learning-integration-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="feacd-107">A tutorial has also been posted and is available [here](stream-analytics-machine-learning-integration-tutorial.md).</span></span>

## <a name="overview-azure-machine-learning-terminology"></a><span data-ttu-id="feacd-108">Přehled: Terminologie služby Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="feacd-108">Overview: Azure Machine Learning terminology</span></span>
<span data-ttu-id="feacd-109">Microsoft Azure Machine Learning poskytuje nástroj spolupráci, přetahování myší, které můžete použít k vytvoření, testování a nasazovat řešení prediktivní analýzy na vaše data.</span><span class="sxs-lookup"><span data-stu-id="feacd-109">Microsoft Azure Machine Learning provides a collaborative, drag-and-drop tool you can use to build, test, and deploy predictive analytics solutions on your data.</span></span> <span data-ttu-id="feacd-110">Tento nástroj je volána *Azure Machine Learning Studio*.</span><span class="sxs-lookup"><span data-stu-id="feacd-110">This tool is called the *Azure Machine Learning Studio*.</span></span> <span data-ttu-id="feacd-111">Nástroje studio umožňuje pracovat s prostředky Machine Learning a snadno vytvářet, testovat a iterovat návrhu.</span><span class="sxs-lookup"><span data-stu-id="feacd-111">The studio is used to interact with the Machine Learning resources and easily build, test, and iterate on your design.</span></span> <span data-ttu-id="feacd-112">Níže jsou tyto prostředky a jejich definice.</span><span class="sxs-lookup"><span data-stu-id="feacd-112">These resources and their definitions are below.</span></span>

* <span data-ttu-id="feacd-113">**Pracovní prostor**: *prostoru* je kontejner, který obsahuje všechny ostatní prostředky Machine Learning společně v kontejneru pro správu a řízení.</span><span class="sxs-lookup"><span data-stu-id="feacd-113">**Workspace**: The *workspace* is a container that holds all other Machine Learning resources together in a container for management and control.</span></span>
* <span data-ttu-id="feacd-114">**Experiment**: *experimenty* jsou vytvořené pomocí datových vědců využívat datové sady a cvičení modelu strojového učení.</span><span class="sxs-lookup"><span data-stu-id="feacd-114">**Experiment**: *Experiments* are created by data scientists to utilize datasets and train a machine learning model.</span></span>
* <span data-ttu-id="feacd-115">**Koncový bod**: *koncové body* jsou Azure Machine Learning objekt použitý trvat funkce jako vstup, používá model zadaný strojového učení a vrátit scored výstup.</span><span class="sxs-lookup"><span data-stu-id="feacd-115">**Endpoint**: *Endpoints* are the Azure Machine Learning object used to take features as input, apply a specified machine learning model and return scored output.</span></span>
* <span data-ttu-id="feacd-116">**Vyhodnocování Webservice**: A *vyhodnocování webservice* je kolekcí koncových bodů, jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="feacd-116">**Scoring Webservice**: A *scoring webservice* is a collection of endpoints as mentioned above.</span></span>

<span data-ttu-id="feacd-117">Každý koncový bod má rozhraní API pro spuštění dávky a synchronní zpracování.</span><span class="sxs-lookup"><span data-stu-id="feacd-117">Each endpoint has apis for batch execution and synchronous execution.</span></span> <span data-ttu-id="feacd-118">Stream Analytics používá synchronní zpracování.</span><span class="sxs-lookup"><span data-stu-id="feacd-118">Stream Analytics uses synchronous execution.</span></span> <span data-ttu-id="feacd-119">Název konkrétní službu [požadavků a odpovědí služby](../machine-learning/machine-learning-consume-web-services.md) v AzureML studio.</span><span class="sxs-lookup"><span data-stu-id="feacd-119">The specific service is named a [Request/Response Service](../machine-learning/machine-learning-consume-web-services.md) in AzureML studio.</span></span>

## <a name="machine-learning-resources-needed-for-stream-analytics-jobs"></a><span data-ttu-id="feacd-120">Strojového učení prostředky potřebné pro úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="feacd-120">Machine Learning resources needed for Stream Analytics jobs</span></span>
<span data-ttu-id="feacd-121">Pro účely Stream Analytics úlohy zpracování, koncový bod žádosti a odpovědi [apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md), a všechny potřebné pro úspěšné provedení definici swaggeru.</span><span class="sxs-lookup"><span data-stu-id="feacd-121">For the purposes of Stream Analytics job processing, a Request/Response endpoint, an [apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md), and a swagger definition are all necessary for successful execution.</span></span> <span data-ttu-id="feacd-122">Stream Analytics obsahuje další koncový bod, který vytvoří adresu url pro koncový bod swagger, vyhledá rozhraní a vrátí výchozí UDF definici pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="feacd-122">Stream Analytics has an additional endpoint that constructs the url for swagger endpoint, looks up the interface and returns a default UDF definition to the user.</span></span>

## <a name="configure-a-stream-analytics-and-machine-learning-udf-via-rest-api"></a><span data-ttu-id="feacd-123">Konfigurace služby Stream Analytics a strojového učení UDF přes REST API</span><span class="sxs-lookup"><span data-stu-id="feacd-123">Configure a Stream Analytics and Machine Learning UDF via REST API</span></span>
<span data-ttu-id="feacd-124">Pomocí rozhraní REST API můžete nakonfigurovat úlohu pro volání funkcí jazyka počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="feacd-124">By using REST APIs you may configure your job to call Azure Machine Language functions.</span></span> <span data-ttu-id="feacd-125">Kroky jsou následující:</span><span class="sxs-lookup"><span data-stu-id="feacd-125">The steps are as follows:</span></span>

1. <span data-ttu-id="feacd-126">Vytvoření úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="feacd-126">Create a Stream Analytics job</span></span>
2. <span data-ttu-id="feacd-127">Zadejte vstup</span><span class="sxs-lookup"><span data-stu-id="feacd-127">Define an input</span></span>
3. <span data-ttu-id="feacd-128">Definování výstup</span><span class="sxs-lookup"><span data-stu-id="feacd-128">Define an output</span></span>
4. <span data-ttu-id="feacd-129">Vytvořit uživatelem definované funkce (UDF)</span><span class="sxs-lookup"><span data-stu-id="feacd-129">Create a user-defined function (UDF)</span></span>
5. <span data-ttu-id="feacd-130">Zápis transformace Stream Analytics, která volá UDF</span><span class="sxs-lookup"><span data-stu-id="feacd-130">Write a Stream Analytics transformation that calls the UDF</span></span>
6. <span data-ttu-id="feacd-131">Spustit úlohu</span><span class="sxs-lookup"><span data-stu-id="feacd-131">Start the job</span></span>

## <a name="creating-a-udf-with-basic-properties"></a><span data-ttu-id="feacd-132">Vytváření uživatelem definovanou FUNKCI s základní vlastnosti</span><span class="sxs-lookup"><span data-stu-id="feacd-132">Creating a UDF with basic properties</span></span>
<span data-ttu-id="feacd-133">Jako příklad následující vzorový kód vytvoří skalární uživatelem definovanou FUNKCI s názvem *newudf* která sváže koncový bod Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="feacd-133">As an example, the following sample code creates a scalar UDF named *newudf* that binds to an Azure Machine Learning endpoint.</span></span> <span data-ttu-id="feacd-134">Všimněte si, že *koncový bod* (URI) se dá služba najít na stránce nápovědy rozhraní API pro službu zvolené a *apiKey* naleznete na hlavní stránce služeb.</span><span class="sxs-lookup"><span data-stu-id="feacd-134">Note that the *endpoint* (service URI) can be found on the API help page for the chosen service and the *apiKey* can be found on the Services main page.</span></span>

````
    PUT : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>  
````

<span data-ttu-id="feacd-135">Příklad textu žádosti:</span><span class="sxs-lookup"><span data-stu-id="feacd-135">Example request body:</span></span>  

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

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a><span data-ttu-id="feacd-136">Koncový bod RetrieveDefaultDefinition volání pro výchozí UDF</span><span class="sxs-lookup"><span data-stu-id="feacd-136">Call RetrieveDefaultDefinition endpoint for default UDF</span></span>
<span data-ttu-id="feacd-137">Po vytvoření je UDF kostru je potřeba dokončení definice UDF.</span><span class="sxs-lookup"><span data-stu-id="feacd-137">Once the skeleton UDF is created the complete definition of the UDF is needed.</span></span> <span data-ttu-id="feacd-138">Koncový bod RetreiveDefaultDefinition umožňuje získání výchozí definice pro skalární funkci, která je vázána na koncový bod Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="feacd-138">The RetreiveDefaultDefinition endpoint helps you get the default definition for a scalar function that is bound to an Azure Machine Learning endpoint.</span></span> <span data-ttu-id="feacd-139">Datové části níže, musíte získat výchozí definici UDF pro skalární funkci, která je vázána na koncový bod Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="feacd-139">The payload below requires you to get the default UDF definition for a scalar function that is bound to an Azure Machine Learning endpoint.</span></span> <span data-ttu-id="feacd-140">Skutečný koncový bod neurčuje jak již bylo zadáno během žádosti PUT.</span><span class="sxs-lookup"><span data-stu-id="feacd-140">It doesn’t specify the actual endpoint as it has already been provided during PUT request.</span></span> <span data-ttu-id="feacd-141">Stream Analytics volá zadaný v požadavku, pokud je explicitně zadaná koncový bod.</span><span class="sxs-lookup"><span data-stu-id="feacd-141">Stream Analytics calls the endpoint provided in the request if it is provided explicitly.</span></span> <span data-ttu-id="feacd-142">V opačném případě se použije, původně odkazuje.</span><span class="sxs-lookup"><span data-stu-id="feacd-142">Otherwise it uses the one originally referenced.</span></span> <span data-ttu-id="feacd-143">Zde trvá UDF jednoho řetězce parametr (věta) a vrátí výstup jednoho typu řetězec, který určuje popisek "postojích" pro tuto větu.</span><span class="sxs-lookup"><span data-stu-id="feacd-143">Here the UDF takes a single string parameter (a sentence) and returns a single output of type string which indicates the “sentiment” label for that sentence.</span></span>

````
POST : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>/RetrieveDefaultDefinition?api-version=<apiVersion>
````

<span data-ttu-id="feacd-144">Příklad textu žádosti:</span><span class="sxs-lookup"><span data-stu-id="feacd-144">Example request body:</span></span>  

````
    {
        "bindingType": "Microsoft.MachineLearning/WebService",
        "bindingRetrievalProperties": {
            "executeEndpoint": null,
            "udfType": "Scalar"
        }
    }
````

<span data-ttu-id="feacd-145">Ukázka výstupu tohoto vyhledejte něco chtěli níže.</span><span class="sxs-lookup"><span data-stu-id="feacd-145">A sample output of this would look something like below.</span></span>  

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

## <a name="patch-udf-with-the-response"></a><span data-ttu-id="feacd-146">Oprava UDF s odpovědí</span><span class="sxs-lookup"><span data-stu-id="feacd-146">Patch UDF with the response</span></span>
<span data-ttu-id="feacd-147">Teď UDF musí opravit s předchozí odpověď, a to, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="feacd-147">Now the UDF must be patched with the previous response, as shown below.</span></span>

````
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
````

<span data-ttu-id="feacd-148">Text žádosti (výstup z RetrieveDefaultDefinition):</span><span class="sxs-lookup"><span data-stu-id="feacd-148">Request Body (Output from RetrieveDefaultDefinition):</span></span>

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

## <a name="implement-stream-analytics-transformation-to-call-the-udf"></a><span data-ttu-id="feacd-149">Implementace služby Stream Analytics transformace volat UDF</span><span class="sxs-lookup"><span data-stu-id="feacd-149">Implement Stream Analytics transformation to call the UDF</span></span>
<span data-ttu-id="feacd-150">Nyní dotaz UDF (zde s názvem scoreTweet) pro každý vstupní událost a zápisu odpovědi pro tuto událost do výstupu.</span><span class="sxs-lookup"><span data-stu-id="feacd-150">Now query the UDF (here named scoreTweet) for every input event and write a response for that event to an output.</span></span>  

````
    {
        "name": "transformation",
        "properties": {
            "streamingUnits": null,
            "query": "select *,scoreTweet(Tweet) TweetSentiment into blobOutput from blobInput"
        }
    }
````


## <a name="get-help"></a><span data-ttu-id="feacd-151">Podpora</span><span class="sxs-lookup"><span data-stu-id="feacd-151">Get help</span></span>
<span data-ttu-id="feacd-152">Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="feacd-152">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="feacd-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="feacd-153">Next steps</span></span>
* [<span data-ttu-id="feacd-154">Úvod do služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="feacd-154">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="feacd-155">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="feacd-155">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="feacd-156">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="feacd-156">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="feacd-157">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="feacd-157">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="feacd-158">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="feacd-158">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
