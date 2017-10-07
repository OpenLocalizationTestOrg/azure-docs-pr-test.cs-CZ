---
title: "modely Machine Learning aaaUpdate pomocí Azure Data Factory | Microsoft Docs"
description: "Popisuje, jak vytvořit toocreate prediktivní kanály pomocí Azure Data Factory a Azure Machine Learning"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: 6e5e4d2cfd245c7a9ed3bb9cdacca1f7f82b9620
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="updating-azure-machine-learning-models-using-update-resource-activity"></a><span data-ttu-id="57cc0-103">Aktualizace pomocí aktivita prostředku aktualizace modely Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="57cc0-103">Updating Azure Machine Learning models using Update Resource Activity</span></span>

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="57cc0-104">Aktivita Hive</span><span class="sxs-lookup"><span data-stu-id="57cc0-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="57cc0-105">Pig aktivity</span><span class="sxs-lookup"><span data-stu-id="57cc0-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="57cc0-106">Činnost MapReduce</span><span class="sxs-lookup"><span data-stu-id="57cc0-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="57cc0-107">Streamované aktivitě Hadoop</span><span class="sxs-lookup"><span data-stu-id="57cc0-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="57cc0-108">Spark aktivity</span><span class="sxs-lookup"><span data-stu-id="57cc0-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="57cc0-109">Aktivita Provedení dávky služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="57cc0-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="57cc0-110">Aktivita Aktualizace prostředků služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="57cc0-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="57cc0-111">Aktivita Uložená procedura</span><span class="sxs-lookup"><span data-stu-id="57cc0-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="57cc0-112">Aktivita U-SQL služby Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="57cc0-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="57cc0-113">Vlastní aktivity rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="57cc0-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="57cc0-114">Tento článek doplňuje hello hlavní Azure Data Factory – článek integrace Azure Machine Learning: [vytvořit prediktivní kanály pomocí Azure Machine Learning a Azure Data Factory](data-factory-azure-ml-batch-execution-activity.md).</span><span class="sxs-lookup"><span data-stu-id="57cc0-114">This article complements hello main Azure Data Factory - Azure Machine Learning integration article: [Create predictive pipelines using Azure Machine Learning and Azure Data Factory](data-factory-azure-ml-batch-execution-activity.md).</span></span> <span data-ttu-id="57cc0-115">Pokud jste tak již neučinili, přečtěte si hello hlavní článek než si přečtete prostřednictvím tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="57cc0-115">If you haven't already done so, review hello main article before reading through this article.</span></span> 

## <a name="overview"></a><span data-ttu-id="57cc0-116">Přehled</span><span class="sxs-lookup"><span data-stu-id="57cc0-116">Overview</span></span>
<span data-ttu-id="57cc0-117">V čase třeba hello prediktivní modely v experimentech vyhodnocování Azure ML hello toobe retrained pomocí nové vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="57cc0-117">Over time, hello predictive models in hello Azure ML scoring experiments need toobe retrained using new input datasets.</span></span> <span data-ttu-id="57cc0-118">Po dokončení práce s retraining, budete chtít tooupdate hello vyhodnocování webové služby s hello retrained modelu ML.</span><span class="sxs-lookup"><span data-stu-id="57cc0-118">After you are done with retraining, you want tooupdate hello scoring web service with hello retrained ML model.</span></span> <span data-ttu-id="57cc0-119">jsou Hello obvyklé kroky tooenable retraining a aktualizace modelů Azure ML prostřednictvím webové služby:</span><span class="sxs-lookup"><span data-stu-id="57cc0-119">hello typical steps tooenable retraining and updating Azure ML models via web services are:</span></span>

1. <span data-ttu-id="57cc0-120">Vytvoření experimentu v [Azure ML Studio](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="57cc0-120">Create an experiment in [Azure ML Studio](https://studio.azureml.net).</span></span>
2. <span data-ttu-id="57cc0-121">Jakmile budete spokojeni s modelem hello, použít Azure ML Studio toopublish webové služby pro obě hello **výukový experiment** a vyhodnocování /**prediktivní experiment**.</span><span class="sxs-lookup"><span data-stu-id="57cc0-121">When you are satisfied with hello model, use Azure ML Studio toopublish web services for both hello **training experiment** and scoring/**predictive experiment**.</span></span>

<span data-ttu-id="57cc0-122">Hello následující tabulka popisuje hello webové služby použité v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="57cc0-122">hello following table describes hello web services used in this example.</span></span>  <span data-ttu-id="57cc0-123">V tématu [Machine Learning Přeučování modelů prostřednictvím kódu programu](../machine-learning/machine-learning-retrain-models-programmatically.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="57cc0-123">See [Retrain Machine Learning models programmatically](../machine-learning/machine-learning-retrain-models-programmatically.md) for details.</span></span>

- <span data-ttu-id="57cc0-124">**Cvičení webové služby** – přijímá Cvičná data a vytváří trénované modely.</span><span class="sxs-lookup"><span data-stu-id="57cc0-124">**Training web service** - Receives training data and produces trained models.</span></span> <span data-ttu-id="57cc0-125">výstup Hello hello retraining je soubor .ilearner v Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="57cc0-125">hello output of hello retraining is an .ilearner file in an Azure Blob storage.</span></span> <span data-ttu-id="57cc0-126">Hello **výchozí koncový bod** se automaticky vytvoří pro při publikování hello školení experimentování jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="57cc0-126">hello **default endpoint** is automatically created for you when you publish hello training experiment as a web service.</span></span> <span data-ttu-id="57cc0-127">Můžete vytvořit další koncové body, ale hello příklad používá jenom hello výchozí koncový bod.</span><span class="sxs-lookup"><span data-stu-id="57cc0-127">You can create more endpoints but hello example uses only hello default endpoint.</span></span>
- <span data-ttu-id="57cc0-128">**Vyhodnocování webové služby** – bez popisku dat příklady obdrží a umožňuje předpovědi.</span><span class="sxs-lookup"><span data-stu-id="57cc0-128">**Scoring web service** - Receives unlabeled data examples and makes predictions.</span></span> <span data-ttu-id="57cc0-129">výstup Hello předpovědi může mít různé formy, například soubor .csv nebo řádků v Azure SQL database, v závislosti na konfiguraci hello hello experimentu.</span><span class="sxs-lookup"><span data-stu-id="57cc0-129">hello output of prediction could have various forms, such as a .csv file or rows in an Azure SQL database, depending on hello configuration of hello experiment.</span></span> <span data-ttu-id="57cc0-130">Hello výchozí koncový bod se automaticky vytvoří za vás, když publikujete hello prediktivní experiment jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="57cc0-130">hello default endpoint is automatically created for you when you publish hello predictive experiment as a web service.</span></span> 

<span data-ttu-id="57cc0-131">Hello následující obrázek znázorňuje hello vztah mezi školení a vyhodnocování koncových bodů v Azure ML.</span><span class="sxs-lookup"><span data-stu-id="57cc0-131">hello following picture depicts hello relationship between training and scoring endpoints in Azure ML.</span></span>

![Webové služby](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)

<span data-ttu-id="57cc0-133">Můžete vyvolat hello **cvičení webové služby** pomocí hello **aktivita provedení dávky Azure ML**.</span><span class="sxs-lookup"><span data-stu-id="57cc0-133">You can invoke hello **training web service** by using hello **Azure ML Batch Execution Activity**.</span></span> <span data-ttu-id="57cc0-134">Vyvolání webové služby školení je stejný jako vyvolání webové služby Azure ML (vyhodnocování webová služba) pro vyhodnocování data.</span><span class="sxs-lookup"><span data-stu-id="57cc0-134">Invoking a training web service is same as invoking an Azure ML web service (scoring web service) for scoring data.</span></span> <span data-ttu-id="57cc0-135">Hello předchozí části titulní jak tooinvoke webové služby Azure ML ze služby Azure Data Factory kanálu podrobně.</span><span class="sxs-lookup"><span data-stu-id="57cc0-135">hello preceding sections cover how tooinvoke an Azure ML web service from an Azure Data Factory pipeline in detail.</span></span> 

<span data-ttu-id="57cc0-136">Můžete vyvolat hello **vyhodnocování webové služby** pomocí hello **aktivita prostředku aktualizace Azure ML** tooupdate hello webové služby s nově trained model hello.</span><span class="sxs-lookup"><span data-stu-id="57cc0-136">You can invoke hello **scoring web service** by using hello **Azure ML Update Resource Activity** tooupdate hello web service with hello newly trained model.</span></span> <span data-ttu-id="57cc0-137">Následující příklady Hello poskytují definice propojené služby:</span><span class="sxs-lookup"><span data-stu-id="57cc0-137">hello following examples provide linked service definitions:</span></span> 

## <a name="scoring-web-service-is-a-classic-web-service"></a><span data-ttu-id="57cc0-138">Vyhodnocování webové služby je classic webová služba</span><span class="sxs-lookup"><span data-stu-id="57cc0-138">Scoring web service is a classic web service</span></span>
<span data-ttu-id="57cc0-139">Pokud je hello vyhodnocování webové služby **classic webové služby**, vytvořit hello druhý **jiné než výchozí a aktualizovat koncový bod** pomocí hello [portál Azure](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="57cc0-139">If hello scoring web service is a **classic web service**, create hello second **non-default and updatable endpoint** by using hello [Azure portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="57cc0-140">V tématu [vytvořit koncové body](../machine-learning/machine-learning-create-endpoint.md) najdete v článku kroky.</span><span class="sxs-lookup"><span data-stu-id="57cc0-140">See [Create Endpoints](../machine-learning/machine-learning-create-endpoint.md) article for steps.</span></span> <span data-ttu-id="57cc0-141">Po vytvoření aktualizovat koncový bod hello jiné než výchozí, hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="57cc0-141">After you create hello non-default updatable endpoint, do hello following steps:</span></span>

* <span data-ttu-id="57cc0-142">Klikněte na tlačítko **BATCH EXECUTION** tooget hello URI hodnota hello **mlEndpoint** vlastnost JSON.</span><span class="sxs-lookup"><span data-stu-id="57cc0-142">Click **BATCH EXECUTION** tooget hello URI value for hello **mlEndpoint** JSON property.</span></span>
* <span data-ttu-id="57cc0-143">Klikněte na tlačítko **aktualizace prostředků** odkaz tooget hello URI hodnotu hello **updateResourceEndpoint** vlastnost JSON.</span><span class="sxs-lookup"><span data-stu-id="57cc0-143">Click **UPDATE RESOURCE** link tooget hello URI value for hello **updateResourceEndpoint** JSON property.</span></span> <span data-ttu-id="57cc0-144">klíč rozhraní API Hello je hello koncový bod stránky na (v pravém dolním rohu hello).</span><span class="sxs-lookup"><span data-stu-id="57cc0-144">hello API key is on hello endpoint page itself (in hello bottom-right corner).</span></span>

![aktualizovat koncový bod](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

<span data-ttu-id="57cc0-146">Následující ukázka Hello poskytuje definici JSON ukázka hello AzureML propojené služby.</span><span class="sxs-lookup"><span data-stu-id="57cc0-146">hello following example provides a sample JSON definition for hello AzureML linked service.</span></span> <span data-ttu-id="57cc0-147">Hello apiKey hello propojená služba používá pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="57cc0-147">hello linked service uses hello apiKey for authentication.</span></span>  

```json
{
    "name": "updatableScoringEndpoint2",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--scoring experiment--/jobs",
            "apiKey": "endpoint2Key",
            "updateResourceEndpoint": "https://management.azureml.net/workspaces/xxx/webservices/--scoring experiment--/endpoints/endpoint2"
        }
    }
}
```

## <a name="scoring-web-service-is-azure-resource-manager-web-service"></a><span data-ttu-id="57cc0-148">Vyhodnocování webové služby je webová služba Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="57cc0-148">Scoring web service is Azure Resource Manager web service</span></span> 
<span data-ttu-id="57cc0-149">Pokud hello webové služby je nový typ hello webové služby, který zveřejňuje koncový bod Azure Resource Manager, není nutné tooadd hello druhý **jiné než výchozí** koncový bod.</span><span class="sxs-lookup"><span data-stu-id="57cc0-149">If hello web service is hello new type of web service that exposes an Azure Resource Manager endpoint, you do not need tooadd hello second **non-default** endpoint.</span></span> <span data-ttu-id="57cc0-150">Hello **updateResourceEndpoint** v hello propojené služby je hello formátu:</span><span class="sxs-lookup"><span data-stu-id="57cc0-150">hello **updateResourceEndpoint** in hello linked service is of hello format:</span></span> 

```
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearning/webServices/{web-service-name}?api-version=2016-05-01-preview. 
```

<span data-ttu-id="57cc0-151">Můžete získat hodnoty pro zástupného v adrese URL hello při dotazování hello webové služby na hello [portálu služby Azure Machine Learning webové](https://services.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="57cc0-151">You can get values for place holders in hello URL when querying hello web service on hello [Azure Machine Learning Web Services Portal](https://services.azureml.net/).</span></span> <span data-ttu-id="57cc0-152">Hello nový typ prostředku aktualizace koncového bodu vyžaduje token AAD (Azure Active Directory).</span><span class="sxs-lookup"><span data-stu-id="57cc0-152">hello new type of update resource endpoint requires an AAD (Azure Active Directory) token.</span></span> <span data-ttu-id="57cc0-153">Zadejte **servicePrincipalId** a **servicePrincipalKey**v AzureML propojené služby.</span><span class="sxs-lookup"><span data-stu-id="57cc0-153">Specify **servicePrincipalId** and **servicePrincipalKey**in AzureML linked service.</span></span> <span data-ttu-id="57cc0-154">V tématu [jak toocreate službu objektu zabezpečení a přiřaďte oprávnění toomanage prostředků Azure](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="57cc0-154">See [how toocreate service principal and assign permissions toomanage Azure resource](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="57cc0-155">Zde je ukázka definice AzureML propojené služby:</span><span class="sxs-lookup"><span data-stu-id="57cc0-155">Here is a sample AzureML linked service definition:</span></span> 

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "description": "hello linked service for AML web service.",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/0000000000000000000000000000000000000/services/0000000000000000000000000000000000000/jobs?api-version=2.0",
            "apiKey": "xxxxxxxxxxxx",
            "updateResourceEndpoint": "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myRG/providers/Microsoft.MachineLearning/webServices/myWebService?api-version=2016-05-01-preview",
            "servicePrincipalId": "000000000-0000-0000-0000-0000000000000",
            "servicePrincipalKey": "xxxxx",
            "tenant": "mycompany.com"
        }
    }
}
```

<span data-ttu-id="57cc0-156">Hello následujícím scénáři najdete další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="57cc0-156">hello following scenario provides more details.</span></span> <span data-ttu-id="57cc0-157">Obsahuje příklad retraining a aktualizace Azure ML modely z kanál služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="57cc0-157">It has an example for retraining and updating Azure ML models from an Azure Data Factory pipeline.</span></span>

## <a name="scenario-retraining-and-updating-an-azure-ml-model"></a><span data-ttu-id="57cc0-158">Scénář: retraining a aktualizace model Azure ML</span><span class="sxs-lookup"><span data-stu-id="57cc0-158">Scenario: retraining and updating an Azure ML model</span></span>
<span data-ttu-id="57cc0-159">Tato část obsahuje ukázkový kanál, který používá hello **aktivita provedení dávky Azure ML** tooretrain modelu.</span><span class="sxs-lookup"><span data-stu-id="57cc0-159">This section provides a sample pipeline that uses hello **Azure ML Batch Execution activity** tooretrain a model.</span></span> <span data-ttu-id="57cc0-160">kanál Hello používá také hello **aktivita prostředku aktualizace Azure ML** tooupdate hello model v hello vyhodnocování webové služby.</span><span class="sxs-lookup"><span data-stu-id="57cc0-160">hello pipeline also uses hello **Azure ML Update Resource activity** tooupdate hello model in hello scoring web service.</span></span> <span data-ttu-id="57cc0-161">Hello oddíl obsahuje také fragmenty kódu JSON pro všechny hello propojených služeb, datových sad a kanálu v příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="57cc0-161">hello section also provides JSON snippets for all hello linked services, datasets, and pipeline in hello example.</span></span>

<span data-ttu-id="57cc0-162">Zde je zobrazení diagramu hello hello ukázkový kanál.</span><span class="sxs-lookup"><span data-stu-id="57cc0-162">Here is hello diagram view of hello sample pipeline.</span></span> <span data-ttu-id="57cc0-163">Jak můžete vidět, hello aktivita provedení dávky Azure ML přijímá hello školení vstup a výstup školení (soubor iLearner).</span><span class="sxs-lookup"><span data-stu-id="57cc0-163">As you can see, hello Azure ML Batch Execution Activity takes hello training input and produces a training output (iLearner file).</span></span> <span data-ttu-id="57cc0-164">Hello aktivita prostředku aktualizace Azure ML trvá tento výstup školení a aktualizace hello model v hello vyhodnocování koncový bod webové služby.</span><span class="sxs-lookup"><span data-stu-id="57cc0-164">hello Azure ML Update Resource Activity takes this training output and updates hello model in hello scoring web service endpoint.</span></span> <span data-ttu-id="57cc0-165">Hello aktivita prostředku aktualizace nevytváří žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="57cc0-165">hello Update Resource Activity does not produce any output.</span></span> <span data-ttu-id="57cc0-166">Hello placeholderBlob je právě fiktivní výstupní datovou sadu, která vyžadují hello Azure Data Factory služby toorun hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="57cc0-166">hello placeholderBlob is just a dummy output dataset that is required by hello Azure Data Factory service toorun hello pipeline.</span></span>

![diagram kanálu](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

### <a name="azure-blob-storage-linked-service"></a><span data-ttu-id="57cc0-168">Propojená služba Azure Blob storage:</span><span class="sxs-lookup"><span data-stu-id="57cc0-168">Azure Blob storage linked service:</span></span>
<span data-ttu-id="57cc0-169">Hello Azure Storage obsahuje hello následující data:</span><span class="sxs-lookup"><span data-stu-id="57cc0-169">hello Azure Storage holds hello following data:</span></span>

* <span data-ttu-id="57cc0-170">Cvičná data.</span><span class="sxs-lookup"><span data-stu-id="57cc0-170">training data.</span></span> <span data-ttu-id="57cc0-171">Hello vstupní data pro hello Azure ML školení webové služby.</span><span class="sxs-lookup"><span data-stu-id="57cc0-171">hello input data for hello Azure ML training web service.</span></span>  
* <span data-ttu-id="57cc0-172">reprezentuje soubor iLearner.</span><span class="sxs-lookup"><span data-stu-id="57cc0-172">iLearner file.</span></span> <span data-ttu-id="57cc0-173">Hello výstup z hello Azure ML školení webové služby.</span><span class="sxs-lookup"><span data-stu-id="57cc0-173">hello output from hello Azure ML training web service.</span></span> <span data-ttu-id="57cc0-174">Tento soubor je také hello vstupní toohello aktivita prostředku aktualizace.</span><span class="sxs-lookup"><span data-stu-id="57cc0-174">This file is also hello input toohello Update Resource activity.</span></span>  

<span data-ttu-id="57cc0-175">Tady je definici JSON ukázka hello hello propojené služby:</span><span class="sxs-lookup"><span data-stu-id="57cc0-175">Here is hello sample JSON definition of hello linked service:</span></span>

```JSON
{
    "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=name;AccountKey=key"
        }
    }
}
```

### <a name="training-input-dataset"></a><span data-ttu-id="57cc0-176">Školení vstupní datové sady:</span><span class="sxs-lookup"><span data-stu-id="57cc0-176">Training input dataset:</span></span>
<span data-ttu-id="57cc0-177">Hello následující datovou sadu představuje hello školení vstupní data pro hello Azure ML školení webovou službu.</span><span class="sxs-lookup"><span data-stu-id="57cc0-177">hello following dataset represents hello input training data for hello Azure ML training web service.</span></span> <span data-ttu-id="57cc0-178">Hello aktivita provedení dávky Azure ML trvá tuto datovou sadu jako vstup.</span><span class="sxs-lookup"><span data-stu-id="57cc0-178">hello Azure ML Batch Execution activity takes this dataset as an input.</span></span>

```JSON
{
    "name": "trainingData",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "labeledexamples",
            "fileName": "labeledexamples.arff",
            "format": {
                "type": "TextFormat"
            }
        },
        "availability": {
            "frequency": "Week",
            "interval": 1
        },
        "policy": {          
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

### <a name="training-output-dataset"></a><span data-ttu-id="57cc0-179">Školení výstupní datovou sadu:</span><span class="sxs-lookup"><span data-stu-id="57cc0-179">Training output dataset:</span></span>
<span data-ttu-id="57cc0-180">Hello následující datovou sadu reprezentuje soubor iLearner výstup hello z hello Azure ML školení webové služby.</span><span class="sxs-lookup"><span data-stu-id="57cc0-180">hello following dataset represents hello output iLearner file from hello Azure ML training web service.</span></span> <span data-ttu-id="57cc0-181">Hello aktivita provedení dávky Azure ML vyprodukuje tuto datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="57cc0-181">hello Azure ML Batch Execution Activity produces this dataset.</span></span> <span data-ttu-id="57cc0-182">Tato datová sada je také hello vstupní toohello aktivita prostředku aktualizace Azure ML.</span><span class="sxs-lookup"><span data-stu-id="57cc0-182">This dataset is also hello input toohello Azure ML Update Resource activity.</span></span>

```JSON
{
    "name": "trainedModelBlob",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "trainingoutput",
            "fileName": "model.ilearner",
            "format": {
                "type": "TextFormat"
            }
        },
        "availability": {
            "frequency": "Week",
            "interval": 1
        }
    }
}
```

### <a name="linked-service-for-azure-ml-training-endpoint"></a><span data-ttu-id="57cc0-183">Propojené služby pro koncový bod Azure ML školení</span><span class="sxs-lookup"><span data-stu-id="57cc0-183">Linked service for Azure ML training endpoint</span></span>
<span data-ttu-id="57cc0-184">Následující fragment kódu JSON Hello definuje službou Azure Machine Learning, která je propojená, která ukazuje toohello výchozí koncový bod hello školení webové služby.</span><span class="sxs-lookup"><span data-stu-id="57cc0-184">hello following JSON snippet defines an Azure Machine Learning linked service that points toohello default endpoint of hello training web service.</span></span>

```JSON
{    
    "name": "trainingEndpoint",
      "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--training experiment--/jobs",
              "apiKey": "myKey"
        }
      }
}
```

<span data-ttu-id="57cc0-185">V **Azure ML Studio**, hello tyto hodnoty tooget pro **mlEndpoint** a **apiKey**:</span><span class="sxs-lookup"><span data-stu-id="57cc0-185">In **Azure ML Studio**, do hello following tooget values for **mlEndpoint** and **apiKey**:</span></span>

1. <span data-ttu-id="57cc0-186">Klikněte na tlačítko **webové služby** v levé nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="57cc0-186">Click **WEB SERVICES** on hello left menu.</span></span>
2. <span data-ttu-id="57cc0-187">Klikněte na tlačítko hello **cvičení webové služby** v seznamu hello webových služeb.</span><span class="sxs-lookup"><span data-stu-id="57cc0-187">Click hello **training web service** in hello list of web services.</span></span>
3. <span data-ttu-id="57cc0-188">Klikněte na tlačítko Kopírovat další příliš**klíč rozhraní API** textové pole.</span><span class="sxs-lookup"><span data-stu-id="57cc0-188">Click copy next too**API key** text box.</span></span> <span data-ttu-id="57cc0-189">Vložte klíč hello hello schránky do editoru JSON objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="57cc0-189">Paste hello key in hello clipboard into hello Data Factory JSON editor.</span></span>
4. <span data-ttu-id="57cc0-190">V hello **Azure ML studio**, klikněte na tlačítko **BATCH EXECUTION** odkaz.</span><span class="sxs-lookup"><span data-stu-id="57cc0-190">In hello **Azure ML studio**, click **BATCH EXECUTION** link.</span></span>
5. <span data-ttu-id="57cc0-191">Kopírování hello **URI požadavku** z hello **požadavku** části a vložte ho do editoru JSON objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="57cc0-191">Copy hello **Request URI** from hello **Request** section and paste it into hello Data Factory JSON editor.</span></span>   

### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a><span data-ttu-id="57cc0-192">Propojená služba Azure ML aktualizovat vyhodnocování koncového bodu:</span><span class="sxs-lookup"><span data-stu-id="57cc0-192">Linked Service for Azure ML updatable scoring endpoint:</span></span>
<span data-ttu-id="57cc0-193">Následující fragment kódu JSON Hello definuje služby Azure Machine Learning propojené, který odkazuje toohello jiné než výchozí aktualizovat koncový bod hello vyhodnocování webové služby.</span><span class="sxs-lookup"><span data-stu-id="57cc0-193">hello following JSON snippet defines an Azure Machine Learning linked service that points toohello non-default updatable endpoint of hello scoring web service.</span></span>  

```JSON
{
    "name": "updatableScoringEndpoint2",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/00000000eb0abe4d6bbb1d7886062747d7/services/00000000026734a5889e02fbb1f65cefd/jobs?api-version=2.0",
            "apiKey": "sooooooooooh3WvG1hBfKS2BNNcfwSO7hhY6dY98noLfOdqQydYDIXyf2KoIaN3JpALu/AKtflHWMOCuicm/Q==",
            "updateResourceEndpoint": "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/myWebService?api-version=2016-05-01-preview",
            "servicePrincipalId": "fe200044-c008-4008-a005-94000000731",
            "servicePrincipalKey": "zWa0000000000Tp6FjtZOspK/WMA2tQ08c8U+gZRBlw=",
            "tenant": "mycompany.com"
        }
    }
}
```

### <a name="placeholder-output-dataset"></a><span data-ttu-id="57cc0-194">Zástupný symbol výstupní datovou sadu:</span><span class="sxs-lookup"><span data-stu-id="57cc0-194">Placeholder output dataset:</span></span>
<span data-ttu-id="57cc0-195">Hello aktivita prostředku aktualizace Azure ML negeneruje žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="57cc0-195">hello Azure ML Update Resource activity does not generate any output.</span></span> <span data-ttu-id="57cc0-196">Azure Data Factory však vyžaduje plán výstupní datová sada toodrive hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="57cc0-196">However, Azure Data Factory requires an output dataset toodrive hello schedule of a pipeline.</span></span> <span data-ttu-id="57cc0-197">Proto jsme použít datovou sadu fiktivní/zástupný symbol v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="57cc0-197">Therefore, we use a dummy/placeholder dataset in this example.</span></span>  

```JSON
{
    "name": "placeholderBlob",
    "properties": {
        "availability": {
            "frequency": "Week",
            "interval": 1
        },
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "any",
            "format": {
                "type": "TextFormat"
            }
        }
    }
}
```

### <a name="pipeline"></a><span data-ttu-id="57cc0-198">Kanál</span><span class="sxs-lookup"><span data-stu-id="57cc0-198">Pipeline</span></span>
<span data-ttu-id="57cc0-199">Hello kanálu má dvě aktivity: **AzureMLBatchExecution** a **AzureMLUpdateResource**.</span><span class="sxs-lookup"><span data-stu-id="57cc0-199">hello pipeline has two activities: **AzureMLBatchExecution** and **AzureMLUpdateResource**.</span></span> <span data-ttu-id="57cc0-200">Hello aktivita provedení dávky Azure ML trvá hello Cvičná data jako vstup a vytvoří soubor iLearner jako výstup.</span><span class="sxs-lookup"><span data-stu-id="57cc0-200">hello Azure ML Batch Execution activity takes hello training data as input and produces an iLearner file as an output.</span></span> <span data-ttu-id="57cc0-201">Aktivita Hello vyvolá hello školení webové služby (výukový experiment zveřejněné jako webovou službu) se vstupem hello trénovací data a obdrží od hello webservice soubor ilearner hello.</span><span class="sxs-lookup"><span data-stu-id="57cc0-201">hello activity invokes hello training web service (training experiment exposed as a web service) with hello input training data and receives hello ilearner file from hello webservice.</span></span> <span data-ttu-id="57cc0-202">Hello placeholderBlob je právě fiktivní výstupní datovou sadu, která vyžadují hello Azure Data Factory služby toorun hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="57cc0-202">hello placeholderBlob is just a dummy output dataset that is required by hello Azure Data Factory service toorun hello pipeline.</span></span>

![diagram kanálu](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [
            {
                "name": "retraining",
                "type": "AzureMLBatchExecution",
                "inputs": [
                    {
                        "name": "trainingData"
                    }
                ],
                "outputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "typeProperties": {
                    "webServiceInput": "trainingData",
                    "webServiceOutputs": {
                        "output1": "trainedModelBlob"
                    }              
                 },
                "linkedServiceName": "trainingEndpoint",
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            },
            {
                "type": "AzureMLUpdateResource",
                "typeProperties": {
                    "trainedModelName": "Training Exp for ADF ML [trained model]",
                    "trainedModelDatasetName" :  "trainedModelBlob"
                },
                "inputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "outputs": [
                    {
                        "name": "placeholderBlob"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "AzureML Update Resource",
                "linkedServiceName": "updatableScoringEndpoint2"
            }
        ],
        "start": "2016-02-13T00:00:00Z",
           "end": "2016-02-14T00:00:00Z"
    }
}
```
