---
title: "Aktualizace modelů Machine Learning pomocí Azure Data Factory | Microsoft Docs"
description: "Popisuje postup vytvoření vytvořit prediktivní kanály pomocí Azure Data Factory a Azure Machine Learning"
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
ms.openlocfilehash: e31a7a59d14de4382190b39bd70f3ddf6cf673ea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="updating-azure-machine-learning-models-using-update-resource-activity"></a><span data-ttu-id="a2ff5-103">Aktualizace pomocí aktivita prostředku aktualizace modely Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="a2ff5-103">Updating Azure Machine Learning models using Update Resource Activity</span></span>

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="a2ff5-104">Aktivita Hive</span><span class="sxs-lookup"><span data-stu-id="a2ff5-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="a2ff5-105">Pig aktivity</span><span class="sxs-lookup"><span data-stu-id="a2ff5-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="a2ff5-106">Činnost MapReduce</span><span class="sxs-lookup"><span data-stu-id="a2ff5-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="a2ff5-107">Streamované aktivitě Hadoop</span><span class="sxs-lookup"><span data-stu-id="a2ff5-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="a2ff5-108">Spark aktivity</span><span class="sxs-lookup"><span data-stu-id="a2ff5-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="a2ff5-109">Aktivita Provedení dávky služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="a2ff5-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="a2ff5-110">Aktivita Aktualizace prostředků služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="a2ff5-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="a2ff5-111">Aktivita Uložená procedura</span><span class="sxs-lookup"><span data-stu-id="a2ff5-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="a2ff5-112">Aktivita U-SQL služby Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="a2ff5-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="a2ff5-113">Vlastní aktivity rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="a2ff5-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="a2ff5-114">Tento článek doplňuje hlavní Azure Data Factory – článek integrace Azure Machine Learning: [vytvořit prediktivní kanály pomocí Azure Machine Learning a Azure Data Factory](data-factory-azure-ml-batch-execution-activity.md).</span><span class="sxs-lookup"><span data-stu-id="a2ff5-114">This article complements the main Azure Data Factory - Azure Machine Learning integration article: [Create predictive pipelines using Azure Machine Learning and Azure Data Factory](data-factory-azure-ml-batch-execution-activity.md).</span></span> <span data-ttu-id="a2ff5-115">Pokud jste tak již neučinili, přečtěte si hlavní článek než si přečtete prostřednictvím tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-115">If you haven't already done so, review the main article before reading through this article.</span></span> 

## <a name="overview"></a><span data-ttu-id="a2ff5-116">Přehled</span><span class="sxs-lookup"><span data-stu-id="a2ff5-116">Overview</span></span>
<span data-ttu-id="a2ff5-117">V průběhu času prediktivní modely v Azure ML vyhodnocování experimentů muset být retrained pomocí nové vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-117">Over time, the predictive models in the Azure ML scoring experiments need to be retrained using new input datasets.</span></span> <span data-ttu-id="a2ff5-118">Po dokončení práce s retraining, budete chtít aktualizovat webovou službu vyhodnocování retrained modelu ML.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-118">After you are done with retraining, you want to update the scoring web service with the retrained ML model.</span></span> <span data-ttu-id="a2ff5-119">Typické postup povolení retraining a aktualizace Azure ML modely prostřednictvím webových služeb jsou:</span><span class="sxs-lookup"><span data-stu-id="a2ff5-119">The typical steps to enable retraining and updating Azure ML models via web services are:</span></span>

1. <span data-ttu-id="a2ff5-120">Vytvoření experimentu v [Azure ML Studio](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="a2ff5-120">Create an experiment in [Azure ML Studio](https://studio.azureml.net).</span></span>
2. <span data-ttu-id="a2ff5-121">Jakmile budete spokojeni s modelem, používají k publikování webových služeb pro obě Azure ML Studio **výukový experiment** a vyhodnocování /**prediktivní experiment**.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-121">When you are satisfied with the model, use Azure ML Studio to publish web services for both the **training experiment** and scoring/**predictive experiment**.</span></span>

<span data-ttu-id="a2ff5-122">Následující tabulka popisuje webové služby použité v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-122">The following table describes the web services used in this example.</span></span>  <span data-ttu-id="a2ff5-123">V tématu [Machine Learning Přeučování modelů prostřednictvím kódu programu](../machine-learning/machine-learning-retrain-models-programmatically.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-123">See [Retrain Machine Learning models programmatically](../machine-learning/machine-learning-retrain-models-programmatically.md) for details.</span></span>

- <span data-ttu-id="a2ff5-124">**Cvičení webové služby** – přijímá Cvičná data a vytváří trénované modely.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-124">**Training web service** - Receives training data and produces trained models.</span></span> <span data-ttu-id="a2ff5-125">Výstup retraining je soubor .ilearner v Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-125">The output of the retraining is an .ilearner file in an Azure Blob storage.</span></span> <span data-ttu-id="a2ff5-126">**Výchozí koncový bod** se automaticky vytvoří pro při publikování školení experimentování jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-126">The **default endpoint** is automatically created for you when you publish the training experiment as a web service.</span></span> <span data-ttu-id="a2ff5-127">Můžete vytvořit další koncové body, ale v příkladu se používá pouze výchozí koncový bod.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-127">You can create more endpoints but the example uses only the default endpoint.</span></span>
- <span data-ttu-id="a2ff5-128">**Vyhodnocování webové služby** – bez popisku dat příklady obdrží a umožňuje předpovědi.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-128">**Scoring web service** - Receives unlabeled data examples and makes predictions.</span></span> <span data-ttu-id="a2ff5-129">Výstup předpovědi může mít různé formy, například soubor .csv nebo řádků v Azure SQL database, v závislosti na konfiguraci experimentu.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-129">The output of prediction could have various forms, such as a .csv file or rows in an Azure SQL database, depending on the configuration of the experiment.</span></span> <span data-ttu-id="a2ff5-130">Výchozí koncový bod se automaticky vytvoří za vás, když publikujete prediktivní experiment jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-130">The default endpoint is automatically created for you when you publish the predictive experiment as a web service.</span></span> 

<span data-ttu-id="a2ff5-131">Následující obrázek znázorňuje vztahy mezi školení a vyhodnocování koncových bodů v Azure ML.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-131">The following picture depicts the relationship between training and scoring endpoints in Azure ML.</span></span>

![Webové služby](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)

<span data-ttu-id="a2ff5-133">Můžete vyvolat **cvičení webové služby** pomocí **aktivita provedení dávky Azure ML**.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-133">You can invoke the **training web service** by using the **Azure ML Batch Execution Activity**.</span></span> <span data-ttu-id="a2ff5-134">Vyvolání webové služby školení je stejný jako vyvolání webové služby Azure ML (vyhodnocování webová služba) pro vyhodnocování data.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-134">Invoking a training web service is same as invoking an Azure ML web service (scoring web service) for scoring data.</span></span> <span data-ttu-id="a2ff5-135">V předchozích částech zabývá vyvolání webové služby Azure ML z podrobně kanál služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-135">The preceding sections cover how to invoke an Azure ML web service from an Azure Data Factory pipeline in detail.</span></span> 

<span data-ttu-id="a2ff5-136">Můžete vyvolat **vyhodnocování webové služby** pomocí **aktivita prostředku aktualizace Azure ML** aktualizace webové služby s nově naučeného modelu.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-136">You can invoke the **scoring web service** by using the **Azure ML Update Resource Activity** to update the web service with the newly trained model.</span></span> <span data-ttu-id="a2ff5-137">Následující příklady obsahují definice propojené služby:</span><span class="sxs-lookup"><span data-stu-id="a2ff5-137">The following examples provide linked service definitions:</span></span> 

## <a name="scoring-web-service-is-a-classic-web-service"></a><span data-ttu-id="a2ff5-138">Vyhodnocování webové služby je classic webová služba</span><span class="sxs-lookup"><span data-stu-id="a2ff5-138">Scoring web service is a classic web service</span></span>
<span data-ttu-id="a2ff5-139">Pokud je webová služba vyhodnocování **classic webové služby**, vytvořte druhý **jiné než výchozí a aktualizovat koncový bod** pomocí [portál Azure](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="a2ff5-139">If the scoring web service is a **classic web service**, create the second **non-default and updatable endpoint** by using the [Azure portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="a2ff5-140">V tématu [vytvořit koncové body](../machine-learning/machine-learning-create-endpoint.md) najdete v článku kroky.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-140">See [Create Endpoints](../machine-learning/machine-learning-create-endpoint.md) article for steps.</span></span> <span data-ttu-id="a2ff5-141">Po vytvoření koncového bodu aktualizovat jiné než výchozí, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a2ff5-141">After you create the non-default updatable endpoint, do the following steps:</span></span>

* <span data-ttu-id="a2ff5-142">Klikněte na tlačítko **BATCH EXECUTION** získat hodnota identifikátoru URI pro **mlEndpoint** vlastnost JSON.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-142">Click **BATCH EXECUTION** to get the URI value for the **mlEndpoint** JSON property.</span></span>
* <span data-ttu-id="a2ff5-143">Klikněte na tlačítko **aktualizace prostředků** odkaz k získání hodnoty identifikátoru URI pro **updateResourceEndpoint** vlastnost JSON.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-143">Click **UPDATE RESOURCE** link to get the URI value for the **updateResourceEndpoint** JSON property.</span></span> <span data-ttu-id="a2ff5-144">Klíč rozhraní API je na stránce koncového bodu (v pravém dolním rohu).</span><span class="sxs-lookup"><span data-stu-id="a2ff5-144">The API key is on the endpoint page itself (in the bottom-right corner).</span></span>

![aktualizovat koncový bod](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

<span data-ttu-id="a2ff5-146">Následující příklad uvádí definici JSON ukázka AzureML propojené služby.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-146">The following example provides a sample JSON definition for the AzureML linked service.</span></span> <span data-ttu-id="a2ff5-147">Propojená služba používá apiKey pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-147">The linked service uses the apiKey for authentication.</span></span>  

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

## <a name="scoring-web-service-is-azure-resource-manager-web-service"></a><span data-ttu-id="a2ff5-148">Vyhodnocování webové služby je webová služba Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="a2ff5-148">Scoring web service is Azure Resource Manager web service</span></span> 
<span data-ttu-id="a2ff5-149">Pokud webová služba je nový typ webové služby, který zveřejňuje koncový bod Azure Resource Manager, není potřeba přidat druhý **jiné než výchozí** koncový bod.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-149">If the web service is the new type of web service that exposes an Azure Resource Manager endpoint, you do not need to add the second **non-default** endpoint.</span></span> <span data-ttu-id="a2ff5-150">**UpdateResourceEndpoint** v propojené službě je ve formátu:</span><span class="sxs-lookup"><span data-stu-id="a2ff5-150">The **updateResourceEndpoint** in the linked service is of the format:</span></span> 

```
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearning/webServices/{web-service-name}?api-version=2016-05-01-preview. 
```

<span data-ttu-id="a2ff5-151">Můžete získat hodnoty pro zástupného v adrese URL při dotazování na webovou službu [portálu služby Azure Machine Learning webové](https://services.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="a2ff5-151">You can get values for place holders in the URL when querying the web service on the [Azure Machine Learning Web Services Portal](https://services.azureml.net/).</span></span> <span data-ttu-id="a2ff5-152">Nový typ prostředku aktualizace koncového bodu vyžaduje token AAD (Azure Active Directory).</span><span class="sxs-lookup"><span data-stu-id="a2ff5-152">The new type of update resource endpoint requires an AAD (Azure Active Directory) token.</span></span> <span data-ttu-id="a2ff5-153">Zadejte **servicePrincipalId** a **servicePrincipalKey**v AzureML propojené služby.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-153">Specify **servicePrincipalId** and **servicePrincipalKey**in AzureML linked service.</span></span> <span data-ttu-id="a2ff5-154">V tématu [postup vytvoření instančního objektu a přiřaďte oprávnění ke správě prostředků Azure](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a2ff5-154">See [how to create service principal and assign permissions to manage Azure resource](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="a2ff5-155">Zde je ukázka definice AzureML propojené služby:</span><span class="sxs-lookup"><span data-stu-id="a2ff5-155">Here is a sample AzureML linked service definition:</span></span> 

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "description": "The linked service for AML web service.",
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

<span data-ttu-id="a2ff5-156">V následujícím scénáři najdete další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-156">The following scenario provides more details.</span></span> <span data-ttu-id="a2ff5-157">Obsahuje příklad retraining a aktualizace Azure ML modely z kanál služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-157">It has an example for retraining and updating Azure ML models from an Azure Data Factory pipeline.</span></span>

## <a name="scenario-retraining-and-updating-an-azure-ml-model"></a><span data-ttu-id="a2ff5-158">Scénář: retraining a aktualizace model Azure ML</span><span class="sxs-lookup"><span data-stu-id="a2ff5-158">Scenario: retraining and updating an Azure ML model</span></span>
<span data-ttu-id="a2ff5-159">Tato část obsahuje ukázkový kanál, který používá **aktivita provedení dávky Azure ML** k přeučování modelu.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-159">This section provides a sample pipeline that uses the **Azure ML Batch Execution activity** to retrain a model.</span></span> <span data-ttu-id="a2ff5-160">Kanál také používá **aktivita prostředku aktualizace Azure ML** k aktualizaci modelu v rámci vyhodnocování webové služby.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-160">The pipeline also uses the **Azure ML Update Resource activity** to update the model in the scoring web service.</span></span> <span data-ttu-id="a2ff5-161">Tato část taky poskytuje fragmenty kódu JSON pro všechny propojené služby, datové sady a kanál v příkladu.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-161">The section also provides JSON snippets for all the linked services, datasets, and pipeline in the example.</span></span>

<span data-ttu-id="a2ff5-162">Zde je zobrazení diagramu ukázkový kanál.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-162">Here is the diagram view of the sample pipeline.</span></span> <span data-ttu-id="a2ff5-163">Jak můžete vidět, aktivita provedení dávky Azure ML přijímá vstup školení a vytvoří výstupní školení (soubor iLearner).</span><span class="sxs-lookup"><span data-stu-id="a2ff5-163">As you can see, the Azure ML Batch Execution Activity takes the training input and produces a training output (iLearner file).</span></span> <span data-ttu-id="a2ff5-164">Aktivita prostředku aktualizace Azure ML přebírá tento výstup školení a aktualizuje model v vyhodnocování koncový bod webové služby.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-164">The Azure ML Update Resource Activity takes this training output and updates the model in the scoring web service endpoint.</span></span> <span data-ttu-id="a2ff5-165">Aktivita prostředku aktualizace nevytváří žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-165">The Update Resource Activity does not produce any output.</span></span> <span data-ttu-id="a2ff5-166">PlaceholderBlob je právě fiktivní výstupní datovou sadu, která je vyžadovaná službou Azure Data Factory ke spuštění kanálu.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-166">The placeholderBlob is just a dummy output dataset that is required by the Azure Data Factory service to run the pipeline.</span></span>

![diagram kanálu](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

### <a name="azure-blob-storage-linked-service"></a><span data-ttu-id="a2ff5-168">Propojená služba Azure Blob storage:</span><span class="sxs-lookup"><span data-stu-id="a2ff5-168">Azure Blob storage linked service:</span></span>
<span data-ttu-id="a2ff5-169">Azure Storage obsahuje následující data:</span><span class="sxs-lookup"><span data-stu-id="a2ff5-169">The Azure Storage holds the following data:</span></span>

* <span data-ttu-id="a2ff5-170">Cvičná data.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-170">training data.</span></span> <span data-ttu-id="a2ff5-171">Vstupní data pro webovou službu Azure ML školení.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-171">The input data for the Azure ML training web service.</span></span>  
* <span data-ttu-id="a2ff5-172">reprezentuje soubor iLearner.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-172">iLearner file.</span></span> <span data-ttu-id="a2ff5-173">Výstup z webové služby Azure ML školení.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-173">The output from the Azure ML training web service.</span></span> <span data-ttu-id="a2ff5-174">Tento soubor je také vstup aktivita prostředku aktualizace.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-174">This file is also the input to the Update Resource activity.</span></span>  

<span data-ttu-id="a2ff5-175">Zde je ukázka definici JSON propojené služby:</span><span class="sxs-lookup"><span data-stu-id="a2ff5-175">Here is the sample JSON definition of the linked service:</span></span>

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

### <a name="training-input-dataset"></a><span data-ttu-id="a2ff5-176">Školení vstupní datové sady:</span><span class="sxs-lookup"><span data-stu-id="a2ff5-176">Training input dataset:</span></span>
<span data-ttu-id="a2ff5-177">Tyto datové sady představuje vstupní Cvičná data pro webovou službu Azure ML školení.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-177">The following dataset represents the input training data for the Azure ML training web service.</span></span> <span data-ttu-id="a2ff5-178">Aktivita provedení dávky Azure ML přijímá tuto datovou sadu jako vstup.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-178">The Azure ML Batch Execution activity takes this dataset as an input.</span></span>

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

### <a name="training-output-dataset"></a><span data-ttu-id="a2ff5-179">Školení výstupní datovou sadu:</span><span class="sxs-lookup"><span data-stu-id="a2ff5-179">Training output dataset:</span></span>
<span data-ttu-id="a2ff5-180">Tyto datové sady představuje výstupní soubor iLearner z webové služby Azure ML školení.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-180">The following dataset represents the output iLearner file from the Azure ML training web service.</span></span> <span data-ttu-id="a2ff5-181">Aktivita provedení dávky Azure ML vyprodukuje tuto datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-181">The Azure ML Batch Execution Activity produces this dataset.</span></span> <span data-ttu-id="a2ff5-182">Tato datová sada je také vstup aktivita prostředku aktualizace Azure ML.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-182">This dataset is also the input to the Azure ML Update Resource activity.</span></span>

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

### <a name="linked-service-for-azure-ml-training-endpoint"></a><span data-ttu-id="a2ff5-183">Propojené služby pro koncový bod Azure ML školení</span><span class="sxs-lookup"><span data-stu-id="a2ff5-183">Linked service for Azure ML training endpoint</span></span>
<span data-ttu-id="a2ff5-184">Následující fragment kódu JSON definuje služby Azure Machine Learning propojené, který ukazuje na výchozí koncový bod webové služby školení.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-184">The following JSON snippet defines an Azure Machine Learning linked service that points to the default endpoint of the training web service.</span></span>

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

<span data-ttu-id="a2ff5-185">V **Azure ML Studio**, proveďte následující kroky k získání hodnot pro **mlEndpoint** a **apiKey**:</span><span class="sxs-lookup"><span data-stu-id="a2ff5-185">In **Azure ML Studio**, do the following to get values for **mlEndpoint** and **apiKey**:</span></span>

1. <span data-ttu-id="a2ff5-186">Klikněte na tlačítko **webové služby** v levé nabídce.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-186">Click **WEB SERVICES** on the left menu.</span></span>
2. <span data-ttu-id="a2ff5-187">Klikněte **cvičení webové služby** v seznamu webové služby.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-187">Click the **training web service** in the list of web services.</span></span>
3. <span data-ttu-id="a2ff5-188">Klikněte na možnost Kopírovat do **klíč rozhraní API** textové pole.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-188">Click copy next to **API key** text box.</span></span> <span data-ttu-id="a2ff5-189">Vložte klíč do schránky do editoru Data Factory JSON.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-189">Paste the key in the clipboard into the Data Factory JSON editor.</span></span>
4. <span data-ttu-id="a2ff5-190">V **Azure ML studio**, klikněte na tlačítko **BATCH EXECUTION** odkaz.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-190">In the **Azure ML studio**, click **BATCH EXECUTION** link.</span></span>
5. <span data-ttu-id="a2ff5-191">Kopírování **URI požadavku** z **požadavku** části a vložte ho do editoru Data Factory JSON.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-191">Copy the **Request URI** from the **Request** section and paste it into the Data Factory JSON editor.</span></span>   

### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a><span data-ttu-id="a2ff5-192">Propojená služba Azure ML aktualizovat vyhodnocování koncového bodu:</span><span class="sxs-lookup"><span data-stu-id="a2ff5-192">Linked Service for Azure ML updatable scoring endpoint:</span></span>
<span data-ttu-id="a2ff5-193">Následující fragment kódu JSON definuje služby Azure Machine Learning propojené, která odkazuje na jiné než výchozí koncový bod aktualizovat vyhodnocování webové služby.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-193">The following JSON snippet defines an Azure Machine Learning linked service that points to the non-default updatable endpoint of the scoring web service.</span></span>  

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

### <a name="placeholder-output-dataset"></a><span data-ttu-id="a2ff5-194">Zástupný symbol výstupní datovou sadu:</span><span class="sxs-lookup"><span data-stu-id="a2ff5-194">Placeholder output dataset:</span></span>
<span data-ttu-id="a2ff5-195">Aktivita prostředku aktualizace Azure ML negeneruje žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-195">The Azure ML Update Resource activity does not generate any output.</span></span> <span data-ttu-id="a2ff5-196">Azure Data Factory však vyžaduje výstupní datové sady do jednotky plán kanálu.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-196">However, Azure Data Factory requires an output dataset to drive the schedule of a pipeline.</span></span> <span data-ttu-id="a2ff5-197">Proto jsme použít datovou sadu fiktivní/zástupný symbol v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-197">Therefore, we use a dummy/placeholder dataset in this example.</span></span>  

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

### <a name="pipeline"></a><span data-ttu-id="a2ff5-198">Kanál</span><span class="sxs-lookup"><span data-stu-id="a2ff5-198">Pipeline</span></span>
<span data-ttu-id="a2ff5-199">Kanál má dvě aktivity: **AzureMLBatchExecution** a **AzureMLUpdateResource**.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-199">The pipeline has two activities: **AzureMLBatchExecution** and **AzureMLUpdateResource**.</span></span> <span data-ttu-id="a2ff5-200">Aktivita provedení dávky Azure ML trvá Cvičná data jako vstup a vytvoří soubor iLearner jako výstup.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-200">The Azure ML Batch Execution activity takes the training data as input and produces an iLearner file as an output.</span></span> <span data-ttu-id="a2ff5-201">Aktivity vyvolá webovou službu školení (výukový experiment zveřejněné jako webovou službu) se vstupní Cvičná data a přijímá reprezentuje soubor ilearner z webovou službu.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-201">The activity invokes the training web service (training experiment exposed as a web service) with the input training data and receives the ilearner file from the webservice.</span></span> <span data-ttu-id="a2ff5-202">PlaceholderBlob je právě fiktivní výstupní datovou sadu, která je vyžadovaná službou Azure Data Factory ke spuštění kanálu.</span><span class="sxs-lookup"><span data-stu-id="a2ff5-202">The placeholderBlob is just a dummy output dataset that is required by the Azure Data Factory service to run the pipeline.</span></span>

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
