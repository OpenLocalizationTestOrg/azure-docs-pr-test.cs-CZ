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
# <a name="machine-learning-integration-in-stream-analytics"></a><span data-ttu-id="bd149-103">Počítač integrace učení v Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="bd149-103">Machine Learning integration in Stream Analytics</span></span>
<span data-ttu-id="bd149-104">Stream Analytics podporuje uživatelsky definované funkce, které zdůrazňují tooAzure Machine Learning koncové body.</span><span class="sxs-lookup"><span data-stu-id="bd149-104">Stream Analytics supports user-defined functions that call out tooAzure Machine Learning endpoints.</span></span> <span data-ttu-id="bd149-105">Podpora rozhraní REST API pro tuto funkci je podrobně popsaná v hello [Stream Analytics REST API knihovny](https://msdn.microsoft.com/library/azure/dn835031.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd149-105">REST API support for this feature is detailed in hello [Stream Analytics REST API library](https://msdn.microsoft.com/library/azure/dn835031.aspx).</span></span> <span data-ttu-id="bd149-106">Tento článek obsahuje doplňující informace potřebné pro úspěšné dokončení implementace tuto funkci v Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="bd149-106">This article provides supplemental information needed for successful implementation of this capability in Stream Analytics.</span></span> <span data-ttu-id="bd149-107">Kurz také byl odeslán a je k dispozici [zde](stream-analytics-machine-learning-integration-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="bd149-107">A tutorial has also been posted and is available [here](stream-analytics-machine-learning-integration-tutorial.md).</span></span>

## <a name="overview-azure-machine-learning-terminology"></a><span data-ttu-id="bd149-108">Přehled: Terminologie služby Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="bd149-108">Overview: Azure Machine Learning terminology</span></span>
<span data-ttu-id="bd149-109">Microsoft Azure Machine Learning poskytuje můžete použít nástroj pro spolupráci, přetažení myší toobuild, testovat a nasazovat řešení prediktivní analýzy na vaše data.</span><span class="sxs-lookup"><span data-stu-id="bd149-109">Microsoft Azure Machine Learning provides a collaborative, drag-and-drop tool you can use toobuild, test, and deploy predictive analytics solutions on your data.</span></span> <span data-ttu-id="bd149-110">Tento nástroj se nazývá hello *Azure Machine Learning Studio*.</span><span class="sxs-lookup"><span data-stu-id="bd149-110">This tool is called hello *Azure Machine Learning Studio*.</span></span> <span data-ttu-id="bd149-111">Hello studio je použité toointeract s hello prostředky Machine Learning a snadno vytvářet, testování a iterovat návrhu.</span><span class="sxs-lookup"><span data-stu-id="bd149-111">hello studio is used toointeract with hello Machine Learning resources and easily build, test, and iterate on your design.</span></span> <span data-ttu-id="bd149-112">Níže jsou tyto prostředky a jejich definice.</span><span class="sxs-lookup"><span data-stu-id="bd149-112">These resources and their definitions are below.</span></span>

* <span data-ttu-id="bd149-113">**Pracovní prostor**: hello *prostoru* je kontejner, který obsahuje všechny ostatní prostředky Machine Learning společně v kontejneru pro správu a řízení.</span><span class="sxs-lookup"><span data-stu-id="bd149-113">**Workspace**: hello *workspace* is a container that holds all other Machine Learning resources together in a container for management and control.</span></span>
* <span data-ttu-id="bd149-114">**Experiment**: *experimenty* jsou vytvořené pomocí dat vědců tooutilize datové sady a cvičení model machine learning.</span><span class="sxs-lookup"><span data-stu-id="bd149-114">**Experiment**: *Experiments* are created by data scientists tooutilize datasets and train a machine learning model.</span></span>
* <span data-ttu-id="bd149-115">**Koncový bod**: *koncové body* jsou hello Azure Machine Learning funkce tootake objektu se používá jako vstup, používá model zadaný machine learning a vrátit scored výstup.</span><span class="sxs-lookup"><span data-stu-id="bd149-115">**Endpoint**: *Endpoints* are hello Azure Machine Learning object used tootake features as input, apply a specified machine learning model and return scored output.</span></span>
* <span data-ttu-id="bd149-116">**Vyhodnocování Webservice**: A *vyhodnocování webservice* je kolekcí koncových bodů, jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="bd149-116">**Scoring Webservice**: A *scoring webservice* is a collection of endpoints as mentioned above.</span></span>

<span data-ttu-id="bd149-117">Každý koncový bod má rozhraní API pro spuštění dávky a synchronní zpracování.</span><span class="sxs-lookup"><span data-stu-id="bd149-117">Each endpoint has apis for batch execution and synchronous execution.</span></span> <span data-ttu-id="bd149-118">Stream Analytics používá synchronní zpracování.</span><span class="sxs-lookup"><span data-stu-id="bd149-118">Stream Analytics uses synchronous execution.</span></span> <span data-ttu-id="bd149-119">názvem Hello konkrétní službu [požadavků a odpovědí služby](../machine-learning/machine-learning-consume-web-services.md) v AzureML studio.</span><span class="sxs-lookup"><span data-stu-id="bd149-119">hello specific service is named a [Request/Response Service](../machine-learning/machine-learning-consume-web-services.md) in AzureML studio.</span></span>

## <a name="machine-learning-resources-needed-for-stream-analytics-jobs"></a><span data-ttu-id="bd149-120">Strojového učení prostředky potřebné pro úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="bd149-120">Machine Learning resources needed for Stream Analytics jobs</span></span>
<span data-ttu-id="bd149-121">Pro účely hello Stream Analytics úlohy zpracování, koncový bod žádosti a odpovědi [apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md), a všechny potřebné pro úspěšné provedení definici swaggeru.</span><span class="sxs-lookup"><span data-stu-id="bd149-121">For hello purposes of Stream Analytics job processing, a Request/Response endpoint, an [apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md), and a swagger definition are all necessary for successful execution.</span></span> <span data-ttu-id="bd149-122">Stream Analytics obsahuje další koncový bod, který vytvoří hello url pro koncový bod swagger, vyhledá hello rozhraní a vrátí výchozí UDF definice toohello uživatele.</span><span class="sxs-lookup"><span data-stu-id="bd149-122">Stream Analytics has an additional endpoint that constructs hello url for swagger endpoint, looks up hello interface and returns a default UDF definition toohello user.</span></span>

## <a name="configure-a-stream-analytics-and-machine-learning-udf-via-rest-api"></a><span data-ttu-id="bd149-123">Konfigurace služby Stream Analytics a strojového učení UDF přes REST API</span><span class="sxs-lookup"><span data-stu-id="bd149-123">Configure a Stream Analytics and Machine Learning UDF via REST API</span></span>
<span data-ttu-id="bd149-124">Pomocí rozhraní REST API můžete nakonfigurovat funkce Azure Machine jazyk toocall úlohy.</span><span class="sxs-lookup"><span data-stu-id="bd149-124">By using REST APIs you may configure your job toocall Azure Machine Language functions.</span></span> <span data-ttu-id="bd149-125">Hello kroky jsou následující:</span><span class="sxs-lookup"><span data-stu-id="bd149-125">hello steps are as follows:</span></span>

1. <span data-ttu-id="bd149-126">Vytvoření úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="bd149-126">Create a Stream Analytics job</span></span>
2. <span data-ttu-id="bd149-127">Zadejte vstup</span><span class="sxs-lookup"><span data-stu-id="bd149-127">Define an input</span></span>
3. <span data-ttu-id="bd149-128">Definování výstup</span><span class="sxs-lookup"><span data-stu-id="bd149-128">Define an output</span></span>
4. <span data-ttu-id="bd149-129">Vytvořit uživatelem definované funkce (UDF)</span><span class="sxs-lookup"><span data-stu-id="bd149-129">Create a user-defined function (UDF)</span></span>
5. <span data-ttu-id="bd149-130">Zápis transformace Stream Analytics, že volání hello UDF</span><span class="sxs-lookup"><span data-stu-id="bd149-130">Write a Stream Analytics transformation that calls hello UDF</span></span>
6. <span data-ttu-id="bd149-131">Spuštění úlohy hello</span><span class="sxs-lookup"><span data-stu-id="bd149-131">Start hello job</span></span>

## <a name="creating-a-udf-with-basic-properties"></a><span data-ttu-id="bd149-132">Vytváření uživatelem definovanou FUNKCI s základní vlastnosti</span><span class="sxs-lookup"><span data-stu-id="bd149-132">Creating a UDF with basic properties</span></span>
<span data-ttu-id="bd149-133">Jako příklad hello následující ukázkový kód vytvoří skalární uživatelem definovanou FUNKCI s názvem *newudf* který váže koncový bod Azure Machine Learning tooan.</span><span class="sxs-lookup"><span data-stu-id="bd149-133">As an example, hello following sample code creates a scalar UDF named *newudf* that binds tooan Azure Machine Learning endpoint.</span></span> <span data-ttu-id="bd149-134">Všimněte si, že hello *koncový bod* (URI) se dá služba najít na stránce nápovědy hello rozhraní API pro hello vybrali služby a hello *apiKey* naleznete na hlavní stránce služeb hello.</span><span class="sxs-lookup"><span data-stu-id="bd149-134">Note that hello *endpoint* (service URI) can be found on hello API help page for hello chosen service and hello *apiKey* can be found on hello Services main page.</span></span>

````
    PUT : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>  
````

<span data-ttu-id="bd149-135">Příklad textu žádosti:</span><span class="sxs-lookup"><span data-stu-id="bd149-135">Example request body:</span></span>  

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

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a><span data-ttu-id="bd149-136">Koncový bod RetrieveDefaultDefinition volání pro výchozí UDF</span><span class="sxs-lookup"><span data-stu-id="bd149-136">Call RetrieveDefaultDefinition endpoint for default UDF</span></span>
<span data-ttu-id="bd149-137">Jednou hello kostru UDF se vytvoří kompletní definice hello hello je nutná UDF.</span><span class="sxs-lookup"><span data-stu-id="bd149-137">Once hello skeleton UDF is created hello complete definition of hello UDF is needed.</span></span> <span data-ttu-id="bd149-138">koncový bod RetreiveDefaultDefinition Hello pomáhá vám hello výchozí definici pro skalární funkce, která je koncový bod Azure Machine Learning vázané tooan.</span><span class="sxs-lookup"><span data-stu-id="bd149-138">hello RetreiveDefaultDefinition endpoint helps you get hello default definition for a scalar function that is bound tooan Azure Machine Learning endpoint.</span></span> <span data-ttu-id="bd149-139">datová část Hello níže vyžaduje tooget hello výchozí UDF definici pro skalární funkce, která je koncový bod Azure Machine Learning vázané tooan.</span><span class="sxs-lookup"><span data-stu-id="bd149-139">hello payload below requires you tooget hello default UDF definition for a scalar function that is bound tooan Azure Machine Learning endpoint.</span></span> <span data-ttu-id="bd149-140">Skutečný koncový bod hello neurčuje jak již bylo zadáno během žádosti PUT.</span><span class="sxs-lookup"><span data-stu-id="bd149-140">It doesn’t specify hello actual endpoint as it has already been provided during PUT request.</span></span> <span data-ttu-id="bd149-141">Stream Analytics volá zadaný v požadavku hello, pokud je výslovně zadaný koncový bod hello.</span><span class="sxs-lookup"><span data-stu-id="bd149-141">Stream Analytics calls hello endpoint provided in hello request if it is provided explicitly.</span></span> <span data-ttu-id="bd149-142">Jinak používá hello jeden původně odkazuje.</span><span class="sxs-lookup"><span data-stu-id="bd149-142">Otherwise it uses hello one originally referenced.</span></span> <span data-ttu-id="bd149-143">Zde hello UDF trvá jednoho řetězce parametr (věta) a vrátí výstup jednoho typu řetězec, který označuje hello "postojích" popisku pro tuto větu.</span><span class="sxs-lookup"><span data-stu-id="bd149-143">Here hello UDF takes a single string parameter (a sentence) and returns a single output of type string which indicates hello “sentiment” label for that sentence.</span></span>

````
POST : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>/RetrieveDefaultDefinition?api-version=<apiVersion>
````

<span data-ttu-id="bd149-144">Příklad textu žádosti:</span><span class="sxs-lookup"><span data-stu-id="bd149-144">Example request body:</span></span>  

````
    {
        "bindingType": "Microsoft.MachineLearning/WebService",
        "bindingRetrievalProperties": {
            "executeEndpoint": null,
            "udfType": "Scalar"
        }
    }
````

<span data-ttu-id="bd149-145">Ukázka výstupu tohoto vyhledejte něco chtěli níže.</span><span class="sxs-lookup"><span data-stu-id="bd149-145">A sample output of this would look something like below.</span></span>  

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

## <a name="patch-udf-with-hello-response"></a><span data-ttu-id="bd149-146">Oprava UDF hello odpovědi</span><span class="sxs-lookup"><span data-stu-id="bd149-146">Patch UDF with hello response</span></span>
<span data-ttu-id="bd149-147">Nyní hello UDF musí být opravit hello předchozí odpovědi, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="bd149-147">Now hello UDF must be patched with hello previous response, as shown below.</span></span>

````
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
````

<span data-ttu-id="bd149-148">Text žádosti (výstup z RetrieveDefaultDefinition):</span><span class="sxs-lookup"><span data-stu-id="bd149-148">Request Body (Output from RetrieveDefaultDefinition):</span></span>

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

## <a name="implement-stream-analytics-transformation-toocall-hello-udf"></a><span data-ttu-id="bd149-149">Implementace služby Stream Analytics transformace toocall hello UDF</span><span class="sxs-lookup"><span data-stu-id="bd149-149">Implement Stream Analytics transformation toocall hello UDF</span></span>
<span data-ttu-id="bd149-150">Nyní dotaz hello UDF (zde s názvem scoreTweet) pro každý vstupní událost a zápisu odpovědi pro tento výstup tooan událostí.</span><span class="sxs-lookup"><span data-stu-id="bd149-150">Now query hello UDF (here named scoreTweet) for every input event and write a response for that event tooan output.</span></span>  

````
    {
        "name": "transformation",
        "properties": {
            "streamingUnits": null,
            "query": "select *,scoreTweet(Tweet) TweetSentiment into blobOutput from blobInput"
        }
    }
````


## <a name="get-help"></a><span data-ttu-id="bd149-151">Podpora</span><span class="sxs-lookup"><span data-stu-id="bd149-151">Get help</span></span>
<span data-ttu-id="bd149-152">Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="bd149-152">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="bd149-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bd149-153">Next steps</span></span>
* [<span data-ttu-id="bd149-154">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="bd149-154">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="bd149-155">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="bd149-155">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="bd149-156">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="bd149-156">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="bd149-157">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="bd149-157">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="bd149-158">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="bd149-158">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
