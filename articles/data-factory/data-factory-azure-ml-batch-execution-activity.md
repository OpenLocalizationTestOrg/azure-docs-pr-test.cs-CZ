---
title: "Prediktivní datových kanálů aaaCreate pomocí Azure Data Factory | Microsoft Docs"
description: "Popisuje, jak vytvořit toocreate prediktivní kanály pomocí Azure Data Factory a Azure Machine Learning"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 4fad8445-4e96-4ce0-aa23-9b88e5ec1965
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: 943210c28b1696e299ff9b7cc96369b95f182354
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-predictive-pipelines-using-azure-machine-learning-and-azure-data-factory"></a><span data-ttu-id="781bb-103">Vytvořit prediktivní kanály pomocí Azure Machine Learning a Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="781bb-103">Create predictive pipelines using Azure Machine Learning and Azure Data Factory</span></span>

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="781bb-104">Aktivita Hive</span><span class="sxs-lookup"><span data-stu-id="781bb-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="781bb-105">Pig aktivity</span><span class="sxs-lookup"><span data-stu-id="781bb-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="781bb-106">Činnost MapReduce</span><span class="sxs-lookup"><span data-stu-id="781bb-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="781bb-107">Streamované aktivitě Hadoop</span><span class="sxs-lookup"><span data-stu-id="781bb-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="781bb-108">Spark aktivity</span><span class="sxs-lookup"><span data-stu-id="781bb-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="781bb-109">Aktivita Provedení dávky služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="781bb-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="781bb-110">Aktivita Aktualizace prostředků služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="781bb-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="781bb-111">Aktivita Uložená procedura</span><span class="sxs-lookup"><span data-stu-id="781bb-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="781bb-112">Aktivita U-SQL služby Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="781bb-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="781bb-113">Vlastní aktivity rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="781bb-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

## <a name="introduction"></a><span data-ttu-id="781bb-114">Úvod</span><span class="sxs-lookup"><span data-stu-id="781bb-114">Introduction</span></span>

### <a name="azure-machine-learning"></a><span data-ttu-id="781bb-115">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="781bb-115">Azure Machine Learning</span></span>
<span data-ttu-id="781bb-116">[Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) umožňuje toobuild můžete otestovat a nasadit řešení prediktivní analýzy.</span><span class="sxs-lookup"><span data-stu-id="781bb-116">[Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) enables you toobuild, test, and deploy predictive analytics solutions.</span></span> <span data-ttu-id="781bb-117">Z vysoké úrovně hlediska se provádí v tři kroky:</span><span class="sxs-lookup"><span data-stu-id="781bb-117">From a high-level point of view, it is done in three steps:</span></span>

1. <span data-ttu-id="781bb-118">**Vytvoření experimentu školení**.</span><span class="sxs-lookup"><span data-stu-id="781bb-118">**Create a training experiment**.</span></span> <span data-ttu-id="781bb-119">Tento krok se provádí pomocí hello Azure ML Studio.</span><span class="sxs-lookup"><span data-stu-id="781bb-119">You do this step by using hello Azure ML Studio.</span></span> <span data-ttu-id="781bb-120">Hello ML studio je spolupráce vizuální vývojové prostředí použít tootrain a testování pomocí Cvičná data modelu prediktivní analýzy.</span><span class="sxs-lookup"><span data-stu-id="781bb-120">hello ML studio is a collaborative visual development environment that you use tootrain and test a predictive analytics model using training data.</span></span>
2. <span data-ttu-id="781bb-121">**Převést prediktivní experiment tooa**.</span><span class="sxs-lookup"><span data-stu-id="781bb-121">**Convert it tooa predictive experiment**.</span></span> <span data-ttu-id="781bb-122">Jakmile se v modelu má cvičena s existujícími daty a jsou připravené toouse ho tooscore nová data, připravte a zjednodušit experimentu pro vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="781bb-122">Once your model has been trained with existing data and you are ready toouse it tooscore new data, you prepare and streamline your experiment for scoring.</span></span>
3. <span data-ttu-id="781bb-123">**Nasadit jako webovou službu**.</span><span class="sxs-lookup"><span data-stu-id="781bb-123">**Deploy it as a web service**.</span></span> <span data-ttu-id="781bb-124">Vyhodnocování experimentu můžete publikovat jako Azure webové služby.</span><span class="sxs-lookup"><span data-stu-id="781bb-124">You can publish your scoring experiment as an Azure web service.</span></span> <span data-ttu-id="781bb-125">Můžete odesílat datový model tooyour přes tento koncový bod webové služby a přijímat výsledek předpovědi u tabulátorů hello modelu.</span><span class="sxs-lookup"><span data-stu-id="781bb-125">You can send data tooyour model via this web service end point and receive result predictions fro hello model.</span></span>  

### <a name="azure-data-factory"></a><span data-ttu-id="781bb-126">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="781bb-126">Azure Data Factory</span></span>
<span data-ttu-id="781bb-127">Data Factory je Cloudová datová integrační služba, která orchestruje a automatizuje hello **přesun** a **transformace** dat.</span><span class="sxs-lookup"><span data-stu-id="781bb-127">Data Factory is a cloud-based data integration service that orchestrates and automates hello **movement** and **transformation** of data.</span></span> <span data-ttu-id="781bb-128">Můžete vytvořit integrace řešení pro data pomocí Azure Data Factory můžete načítání dat z různých úložišť dat, které proces nebo transformace dat hello nebo publikování hello výsledek data toohello datová úložiště.</span><span class="sxs-lookup"><span data-stu-id="781bb-128">You can create data integration solutions using Azure Data Factory that can ingest data from various data stores, transform/process hello data, and publish hello result data toohello data stores.</span></span>

<span data-ttu-id="781bb-129">Služba data Factory můžete toocreate datových kanálů, které přesunout a transformovat data a spusťte hello kanálů podle zadaného plánu (HODINOVĚ, denně, týdně, atd.).</span><span class="sxs-lookup"><span data-stu-id="781bb-129">Data Factory service allows you toocreate data pipelines that move and transform data, and then run hello pipelines on a specified schedule (hourly, daily, weekly, etc.).</span></span> <span data-ttu-id="781bb-130">Také poskytuje bohatých vizualizací toodisplay hello rodokmenu a závislostí mezi vlastními kanály a monitorování všech datových kanálů z přesně stanovit problémů jednoho jednotného zobrazení tooeasily a nastavit upozornění monitorování.</span><span class="sxs-lookup"><span data-stu-id="781bb-130">It also provides rich visualizations toodisplay hello lineage and dependencies between your data pipelines, and monitor all your data pipelines from a single unified view tooeasily pinpoint issues and setup monitoring alerts.</span></span>

<span data-ttu-id="781bb-131">V tématu [Úvod tooAzure Data Factory](data-factory-introduction.md) a [sestavit svůj první kanál](data-factory-build-your-first-pipeline.md) články tooquickly začít pracovat s hello služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="781bb-131">See [Introduction tooAzure Data Factory](data-factory-introduction.md) and [Build your first pipeline](data-factory-build-your-first-pipeline.md) articles tooquickly get started with hello Azure Data Factory service.</span></span>

### <a name="data-factory-and-machine-learning-together"></a><span data-ttu-id="781bb-132">Objekt pro vytváření dat a strojové učení společně</span><span class="sxs-lookup"><span data-stu-id="781bb-132">Data Factory and Machine Learning together</span></span>
<span data-ttu-id="781bb-133">Azure Data Factory umožňuje tooeasily můžete vytvořit kanály, které používají publikovaný [Azure Machine Learning] [ azure-machine-learning] webová služba pro prediktivní analýzy.</span><span class="sxs-lookup"><span data-stu-id="781bb-133">Azure Data Factory enables you tooeasily create pipelines that use a published [Azure Machine Learning][azure-machine-learning] web service for predictive analytics.</span></span> <span data-ttu-id="781bb-134">Pomocí hello **aktivita provedení dávky** v kanál služby Azure Data Factory můžete vyvolat Azure ML web service toomake předpovědi hello data v dávce.</span><span class="sxs-lookup"><span data-stu-id="781bb-134">Using hello **Batch Execution Activity** in an Azure Data Factory pipeline, you can invoke an Azure ML web service toomake predictions on hello data in batch.</span></span> <span data-ttu-id="781bb-135">V tématu [Azure ML vyvolání webové služby pomocí hello aktivita provedení dávky](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="781bb-135">See [Invoking an Azure ML web service using hello Batch Execution Activity](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity) section for details.</span></span>

<span data-ttu-id="781bb-136">V čase třeba hello prediktivní modely v experimentech vyhodnocování Azure ML hello toobe retrained pomocí nové vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="781bb-136">Over time, hello predictive models in hello Azure ML scoring experiments need toobe retrained using new input datasets.</span></span> <span data-ttu-id="781bb-137">Model Azure ML z objektu pro vytváření dat kanál můžete přeučování díky hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="781bb-137">You can retrain an Azure ML model from a Data Factory pipeline by doing hello following steps:</span></span>

1. <span data-ttu-id="781bb-138">Publikujte hello výukový experiment (ne prediktivní experiment) jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="781bb-138">Publish hello training experiment (not predictive experiment) as a web service.</span></span> <span data-ttu-id="781bb-139">Tento krok v hello Azure ML Studio můžete udělat, jako jste to udělali prediktivní experiment tooexpose jako webovou službu v předchozím scénáři hello.</span><span class="sxs-lookup"><span data-stu-id="781bb-139">You do this step in hello Azure ML Studio as you did tooexpose predictive experiment as a web service in hello previous scenario.</span></span>
2. <span data-ttu-id="781bb-140">Použijte hello aktivita provedení dávky Azure ML tooinvoke hello webovou službu pro hello výukový experiment.</span><span class="sxs-lookup"><span data-stu-id="781bb-140">Use hello Azure ML Batch Execution Activity tooinvoke hello web service for hello training experiment.</span></span> <span data-ttu-id="781bb-141">V podstatě můžete použít tooinvoke aktivita provedení dávky Azure ML hello cvičení webová služba i vyhodnocování webové služby.</span><span class="sxs-lookup"><span data-stu-id="781bb-141">Basically, you can use hello Azure ML Batch Execution activity tooinvoke both training web service and scoring web service.</span></span>

<span data-ttu-id="781bb-142">Po dokončení práce s retraining, aktualizujte hello vyhodnocování webové služby (zveřejněné jako webové služby prediktivní experiment) s nově trained model hello pomocí hello **aktivita prostředku aktualizace Azure ML**.</span><span class="sxs-lookup"><span data-stu-id="781bb-142">After you are done with retraining, update hello scoring web service (predictive experiment exposed as a web service) with hello newly trained model by using hello **Azure ML Update Resource Activity**.</span></span> <span data-ttu-id="781bb-143">V tématu [aktualizace modelů pomocí aktivita prostředku aktualizace](data-factory-azure-ml-update-resource-activity.md) článku.</span><span class="sxs-lookup"><span data-stu-id="781bb-143">See [Updating models using Update Resource Activity](data-factory-azure-ml-update-resource-activity.md) article for details.</span></span>

## <a name="invoking-a-web-service-using-batch-execution-activity"></a><span data-ttu-id="781bb-144">Vyvolání webové služby pomocí aktivita provedení dávky</span><span class="sxs-lookup"><span data-stu-id="781bb-144">Invoking a web service using Batch Execution Activity</span></span>
<span data-ttu-id="781bb-145">Použijte přesun dat tooorchestrate Azure Data Factory a zpracování a poté proveďte dávkového spuštění pomocí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="781bb-145">You use Azure Data Factory tooorchestrate data movement and processing, and then perform batch execution using Azure Machine Learning.</span></span> <span data-ttu-id="781bb-146">Zde jsou kroky nejvyšší úrovně hello:</span><span class="sxs-lookup"><span data-stu-id="781bb-146">Here are hello top-level steps:</span></span>

1. <span data-ttu-id="781bb-147">Vytvoření služby Azure Machine Learning propojený.</span><span class="sxs-lookup"><span data-stu-id="781bb-147">Create an Azure Machine Learning linked service.</span></span> <span data-ttu-id="781bb-148">Je třeba hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="781bb-148">You need hello following values:</span></span>

   1. <span data-ttu-id="781bb-149">**Identifikátor URI požadavku je** pro hello Batch provádění API.</span><span class="sxs-lookup"><span data-stu-id="781bb-149">**Request URI** for hello Batch Execution API.</span></span> <span data-ttu-id="781bb-150">Můžete najít hello URI požadavku kliknutím hello **BATCH EXECUTION** odkaz hello webové služby stránce.</span><span class="sxs-lookup"><span data-stu-id="781bb-150">You can find hello Request URI by clicking hello **BATCH EXECUTION** link in hello web services page.</span></span>
   2. <span data-ttu-id="781bb-151">**Klíč rozhraní API** hello publikovaného webovou službu Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="781bb-151">**API key** for hello published Azure Machine Learning web service.</span></span> <span data-ttu-id="781bb-152">Klíč rozhraní API hello můžete získat kliknutím hello webové službě, kterou jste publikovali.</span><span class="sxs-lookup"><span data-stu-id="781bb-152">You can find hello API key by clicking hello web service that you have published.</span></span>
   3. <span data-ttu-id="781bb-153">Použití hello **AzureMLBatchExecution** aktivity.</span><span class="sxs-lookup"><span data-stu-id="781bb-153">Use hello **AzureMLBatchExecution** activity.</span></span>

      ![Strojového učení řídicí panel](./media/data-factory-azure-ml-batch-execution-activity/AzureMLDashboard.png)

      ![Batch identifikátor URI](./media/data-factory-azure-ml-batch-execution-activity/batch-uri.png)

### <a name="scenario-experiments-using-web-service-inputsoutputs-that-refer-toodata-in-azure-blob-storage"></a><span data-ttu-id="781bb-156">Scénář: Experimenty pomocí webové služby vstupy/výstupy odkazovala toodata ve službě Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="781bb-156">Scenario: Experiments using Web service inputs/outputs that refer toodata in Azure Blob Storage</span></span>
<span data-ttu-id="781bb-157">V tomto scénáři hello Azure Machine Learning webové služby umožňuje předpovědi pomocí dat ze souboru v Azure blob storage a ukládá výsledky předpovědi hello v úložišti objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="781bb-157">In this scenario, hello Azure Machine Learning Web service makes predictions using data from a file in an Azure blob storage and stores hello prediction results in hello blob storage.</span></span> <span data-ttu-id="781bb-158">Hello následující kód JSON určuje objekt pro vytváření dat kanál s AzureMLBatchExecution aktivitu.</span><span class="sxs-lookup"><span data-stu-id="781bb-158">hello following JSON defines a Data Factory pipeline with an AzureMLBatchExecution activity.</span></span> <span data-ttu-id="781bb-159">Aktivita Hello má datovou sadu hello **DecisionTreeInputBlob** jako vstup a **DecisionTreeResultBlob** jako výstup hello.</span><span class="sxs-lookup"><span data-stu-id="781bb-159">hello activity has hello dataset **DecisionTreeInputBlob** as input and **DecisionTreeResultBlob** as hello output.</span></span> <span data-ttu-id="781bb-160">Hello **DecisionTreeInputBlob** se předá jako webové služby vstupní toohello podle pomocí hello **webServiceInput** vlastnost JSON.</span><span class="sxs-lookup"><span data-stu-id="781bb-160">hello **DecisionTreeInputBlob** is passed as an input toohello web service by using hello **webServiceInput** JSON property.</span></span> <span data-ttu-id="781bb-161">Hello **DecisionTreeResultBlob** se předá jako výstup toohello webové služby s použitím hello **webServiceOutputs** vlastnost JSON.</span><span class="sxs-lookup"><span data-stu-id="781bb-161">hello **DecisionTreeResultBlob** is passed as an output toohello Web service by using hello **webServiceOutputs** JSON property.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="781bb-162">Pokud hello webová služba přijímá více vstupů, použijte hello **webServiceInputs** vlastnost místo použití **webServiceInput**.</span><span class="sxs-lookup"><span data-stu-id="781bb-162">If hello web service takes multiple inputs, use hello **webServiceInputs** property instead of using **webServiceInput**.</span></span> <span data-ttu-id="781bb-163">V tématu hello [webová služba vyžaduje více vstupů](#web-service-requires-multiple-inputs) části příklad pomocí vlastnosti webServiceInputs hello.</span><span class="sxs-lookup"><span data-stu-id="781bb-163">See hello [Web service requires multiple inputs](#web-service-requires-multiple-inputs) section for an example of using hello webServiceInputs property.</span></span>
>
> <span data-ttu-id="781bb-164">Datové sady, které odkazují hello **webServiceInput**/**webServiceInputs** a **webServiceOutputs** vlastnosti (v  **rámci typeProperties**) musí být i součástí hello aktivity **vstupy** a **výstupy**.</span><span class="sxs-lookup"><span data-stu-id="781bb-164">Datasets that are referenced by hello **webServiceInput**/**webServiceInputs** and **webServiceOutputs** properties (in **typeProperties**) must also be included in hello Activity **inputs** and **outputs**.</span></span>
>
> <span data-ttu-id="781bb-165">V experimentu Azure ML vstup webové služby a porty výstup a globální parametry mají výchozí názvy ("input1", "input2"), které můžete přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="781bb-165">In your Azure ML experiment, web service input and output ports and global parameters have default names ("input1", "input2") that you can customize.</span></span> <span data-ttu-id="781bb-166">názvy Hello, který použijete pro webServiceInputs, webServiceOutputs a globalParameters nastavení musí přesně shodovat hello názvy v experimentech hello.</span><span class="sxs-lookup"><span data-stu-id="781bb-166">hello names you use for webServiceInputs, webServiceOutputs, and globalParameters settings must exactly match hello names in hello experiments.</span></span> <span data-ttu-id="781bb-167">Datová část požadavku ukázka hello můžete zobrazit na stránce nápovědy spuštění dávky hello pro vaši Azure ML koncový bod tooverify hello očekává mapování.</span><span class="sxs-lookup"><span data-stu-id="781bb-167">You can view hello sample request payload on hello Batch Execution Help page for your Azure ML endpoint tooverify hello expected mapping.</span></span>
>
>

```JSON
{
  "name": "PredictivePipeline",
  "properties": {
    "description": "use AzureML model",
    "activities": [
      {
        "name": "MLActivity",
        "type": "AzureMLBatchExecution",
        "description": "prediction analysis on batch input",
        "inputs": [
          {
            "name": "DecisionTreeInputBlob"
          }
        ],
        "outputs": [
          {
            "name": "DecisionTreeResultBlob"
          }
        ],
        "linkedServiceName": "MyAzureMLLinkedService",
        "typeProperties":
        {
            "webServiceInput": "DecisionTreeInputBlob",
            "webServiceOutputs": {
                "output1": "DecisionTreeResultBlob"
            }                
        },
        "policy": {
          "concurrency": 3,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        }
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```
> [!NOTE]
> <span data-ttu-id="781bb-168">Pouze vstupy a výstupy hello AzureMLBatchExecution aktivity mohou být předána jako toohello parametry webové služby.</span><span class="sxs-lookup"><span data-stu-id="781bb-168">Only inputs and outputs of hello AzureMLBatchExecution activity can be passed as parameters toohello Web service.</span></span> <span data-ttu-id="781bb-169">Například v hello výše fragmentu kódu JSON, je DecisionTreeInputBlob vstupní toohello AzureMLBatchExecution aktivity, které se předá jako vstupní toohello webové služby pomocí parametru webServiceInput.</span><span class="sxs-lookup"><span data-stu-id="781bb-169">For example, in hello above JSON snippet, DecisionTreeInputBlob is an input toohello AzureMLBatchExecution activity, which is passed as an input toohello Web service via webServiceInput parameter.</span></span>   
>
>

### <a name="example"></a><span data-ttu-id="781bb-170">Příklad</span><span class="sxs-lookup"><span data-stu-id="781bb-170">Example</span></span>
<span data-ttu-id="781bb-171">Tento příklad používá úložiště Azure toohold obě hello vstupní a výstupní data.</span><span class="sxs-lookup"><span data-stu-id="781bb-171">This example uses Azure Storage toohold both hello input and output data.</span></span>

<span data-ttu-id="781bb-172">Doporučujeme projít hello [sestavit svůj první kanál pomocí služby Data Factory] [ adf-build-1st-pipeline] kurz před projít tento příklad.</span><span class="sxs-lookup"><span data-stu-id="781bb-172">We recommend that you go through hello [Build your first pipeline with Data Factory][adf-build-1st-pipeline] tutorial before going through this example.</span></span> <span data-ttu-id="781bb-173">V tomto příkladu použijte hello editoru služby Data Factory toocreate artefaktů služby Data Factory (propojené služby, datové sady, kanál).</span><span class="sxs-lookup"><span data-stu-id="781bb-173">Use hello Data Factory Editor toocreate Data Factory artifacts (linked services, datasets, pipeline) in this example.</span></span>   

1. <span data-ttu-id="781bb-174">Vytvoření **propojená služba** pro vaše **Azure Storage**.</span><span class="sxs-lookup"><span data-stu-id="781bb-174">Create a **linked service** for your **Azure Storage**.</span></span> <span data-ttu-id="781bb-175">Pokud hello vstupní a výstupní soubory jsou v jiným účtům úložiště, je třeba dvě propojené služby.</span><span class="sxs-lookup"><span data-stu-id="781bb-175">If hello input and output files are in different storage accounts, you need two linked services.</span></span> <span data-ttu-id="781bb-176">Tady je příklad JSON:</span><span class="sxs-lookup"><span data-stu-id="781bb-176">Here is a JSON example:</span></span>

    ```JSON
    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=[acctName];AccountKey=[acctKey]"
        }
      }
    }
    ```
2. <span data-ttu-id="781bb-177">Vytvoření hello **vstupní** Azure Data Factory **datovou sadu**.</span><span class="sxs-lookup"><span data-stu-id="781bb-177">Create hello **input** Azure Data Factory **dataset**.</span></span> <span data-ttu-id="781bb-178">Na rozdíl od některých jiných objekt pro vytváření dat datové sady, musí obsahovat tyto datové sady obě **folderPath** a **fileName** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="781bb-178">Unlike some other Data Factory datasets, these datasets must contain both **folderPath** and **fileName** values.</span></span> <span data-ttu-id="781bb-179">Můžete použít rozdělení toocause tooprocess každé dávky spuštění (každý datový řez) nebo vytvořit jedinečný vstupní a výstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="781bb-179">You can use partitioning toocause each batch execution (each data slice) tooprocess or produce unique input and output files.</span></span> <span data-ttu-id="781bb-180">Může být nutné tooinclude některé hello tootransform nadřízené činnosti vstup na formát souboru CSV hello a umístěte jej v hello účet úložiště pro každý řez.</span><span class="sxs-lookup"><span data-stu-id="781bb-180">You may need tooinclude some upstream activity tootransform hello input into hello CSV file format and place it in hello storage account for each slice.</span></span> <span data-ttu-id="781bb-181">V takovém případě by zahrnovat hello **externí** a **externalData** nastavení zobrazené v hello následující příklad a vaše DecisionTreeInputBlob by hello výstupní datovou sadu jiné aktivitě.</span><span class="sxs-lookup"><span data-stu-id="781bb-181">In that case, you would not include hello **external** and **externalData** settings shown in hello following example, and your DecisionTreeInputBlob would be hello output dataset of a different Activity.</span></span>

    ```JSON
    {
      "name": "DecisionTreeInputBlob",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "azuremltesting/input",
          "fileName": "in.csv",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
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

    <span data-ttu-id="781bb-182">Soubor vstupní csv musí obsahovat hello řádek záhlaví sloupců.</span><span class="sxs-lookup"><span data-stu-id="781bb-182">Your input csv file must have hello column header row.</span></span> <span data-ttu-id="781bb-183">Pokud používáte hello **aktivity kopírování** csv hello toocreate nebo přesunout do úložiště objektů blob hello, měli byste nastavit vlastnost jímky hello **blobWriterAddHeader** příliš**true**.</span><span class="sxs-lookup"><span data-stu-id="781bb-183">If you are using hello **Copy Activity** toocreate/move hello csv into hello blob storage, you should set hello sink property **blobWriterAddHeader** too**true**.</span></span> <span data-ttu-id="781bb-184">Například:</span><span class="sxs-lookup"><span data-stu-id="781bb-184">For example:</span></span>

    ```JSON
    sink:
    {
        "type": "BlobSink",     
        "blobWriterAddHeader": true
    }
    ```

    <span data-ttu-id="781bb-185">Pokud soubor csv hello nemá hello řádek záhlaví, mohou se zobrazit hello následující chyba: **Chyba v aktivitě: Chyba při čtení řetězec. Neočekávaný token: StartObject. Cesta %, řádek 1, 1 umístit**.</span><span class="sxs-lookup"><span data-stu-id="781bb-185">If hello csv file does not have hello header row, you may see hello following error: **Error in Activity: Error reading string. Unexpected token: StartObject. Path '', line 1, position 1**.</span></span>
3. <span data-ttu-id="781bb-186">Vytvoření hello **výstup** Azure Data Factory **datovou sadu**.</span><span class="sxs-lookup"><span data-stu-id="781bb-186">Create hello **output** Azure Data Factory **dataset**.</span></span> <span data-ttu-id="781bb-187">Tento příklad používá rozdělení toocreate jedinečný výstupní cesta pro každé spuštění řezu.</span><span class="sxs-lookup"><span data-stu-id="781bb-187">This example uses partitioning toocreate a unique output path for each slice execution.</span></span> <span data-ttu-id="781bb-188">Bez hello oddílů, by hello aktivity přepsat soubor hello.</span><span class="sxs-lookup"><span data-stu-id="781bb-188">Without hello partitioning, hello activity would overwrite hello file.</span></span>

    ```JSON
    {
      "name": "DecisionTreeResultBlob",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "azuremltesting/scored/{folderpart}/",
          "fileName": "{filepart}result.csv",
          "partitionedBy": [
            {
              "name": "folderpart",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyyMMdd"
              }
            },
            {
              "name": "filepart",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HHmmss"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 15
        }
      }
    }
    ```
4. <span data-ttu-id="781bb-189">Vytvoření **propojená služba** typu: **AzureMLLinkedService**, poskytuje klíč hello rozhraní API a modelu adresa URL pro spuštění dávky.</span><span class="sxs-lookup"><span data-stu-id="781bb-189">Create a **linked service** of type: **AzureMLLinkedService**, providing hello API key and model batch execution URL.</span></span>

    ```JSON
    {
      "name": "MyAzureMLLinkedService",
      "properties": {
        "type": "AzureML",
        "typeProperties": {
          "mlEndpoint": "https://[batch execution endpoint]/jobs",
          "apiKey": "[apikey]"
        }
      }
    }
    ```
5. <span data-ttu-id="781bb-190">Nakonec vytvořte kanálu obsahující **AzureMLBatchExecution** aktivity.</span><span class="sxs-lookup"><span data-stu-id="781bb-190">Finally, author a pipeline containing an **AzureMLBatchExecution** Activity.</span></span> <span data-ttu-id="781bb-191">V době běhu provádí kanálu hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="781bb-191">At runtime, pipeline performs hello following steps:</span></span>

   1. <span data-ttu-id="781bb-192">Získá umístění hello hello vstupní soubor z vaší vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="781bb-192">Gets hello location of hello input file from your input datasets.</span></span>
   2. <span data-ttu-id="781bb-193">Vyvolá hello Azure Machine Learning dávkového spuštění rozhraní API</span><span class="sxs-lookup"><span data-stu-id="781bb-193">Invokes hello Azure Machine Learning batch execution API</span></span>
   3. <span data-ttu-id="781bb-194">Kopie hello batch provádění výstup toohello objektů blob v výstupní datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="781bb-194">Copies hello batch execution output toohello blob given in your output dataset.</span></span>

      > [!NOTE]
      > <span data-ttu-id="781bb-195">Aktivita AzureMLBatchExecution může mít v počtu nula či více vstupů a výstupů jeden nebo více.</span><span class="sxs-lookup"><span data-stu-id="781bb-195">AzureMLBatchExecution activity can have zero or more inputs and one or more outputs.</span></span>
      >
      >

    ```JSON
    {
        "name": "PredictivePipeline",
        "properties": {
            "description": "use AzureML model",
            "activities": [
            {
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [
                {
                    "name": "DecisionTreeInputBlob"
                }
                ],
                "outputs": [
                {
                    "name": "DecisionTreeResultBlob"
                }
                ],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties":
                {
                    "webServiceInput": "DecisionTreeInputBlob",
                    "webServiceOutputs": {
                        "output1": "DecisionTreeResultBlob"
                    }                
                },
                "policy": {
                    "concurrency": 3,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }
    ```

      <span data-ttu-id="781bb-196">Obě **spustit** a **end** času musí být v [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="781bb-196">Both **start** and **end** datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="781bb-197">Například: 2014-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="781bb-197">For example: 2014-10-14T16:32:41Z.</span></span> <span data-ttu-id="781bb-198">Hello **end** čas je volitelné.</span><span class="sxs-lookup"><span data-stu-id="781bb-198">hello **end** time is optional.</span></span> <span data-ttu-id="781bb-199">Pokud nezadáte hodnotu hello **end** vlastnost, vypočítá se jako "**start + 48 hodin.**"</span><span class="sxs-lookup"><span data-stu-id="781bb-199">If you do not specify value for hello **end** property, it is calculated as "**start + 48 hours.**"</span></span> <span data-ttu-id="781bb-200">bez omezení, zadejte toorun hello kanálu **9999-09-09** hello hodnotu hello **end** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="781bb-200">toorun hello pipeline indefinitely, specify **9999-09-09** as hello value for hello **end** property.</span></span> <span data-ttu-id="781bb-201">Podrobné informace o vlastnostech JSON najdete v tématu [JSON Scripting Reference](https://msdn.microsoft.com/library/dn835050.aspx) (Referenční příručka skriptování JSON).</span><span class="sxs-lookup"><span data-stu-id="781bb-201">See [JSON Scripting Reference](https://msdn.microsoft.com/library/dn835050.aspx) for details about JSON properties.</span></span>

      > [!NOTE]
      > <span data-ttu-id="781bb-202">Určení vstup aktivity AzureMLBatchExecution hello je volitelné.</span><span class="sxs-lookup"><span data-stu-id="781bb-202">Specifying input for hello AzureMLBatchExecution activity is optional.</span></span>
      >
      >

### <a name="scenario-experiments-using-readerwriter-modules-toorefer-toodata-in-various-storages"></a><span data-ttu-id="781bb-203">Scénář: Experimenty v různých úložných toodata toorefer moduly Reader/Writer</span><span class="sxs-lookup"><span data-stu-id="781bb-203">Scenario: Experiments using Reader/Writer Modules toorefer toodata in various storages</span></span>
<span data-ttu-id="781bb-204">Další z typických možností při vytváření experimenty Azure ML je moduly toouse čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="781bb-204">Another common scenario when creating Azure ML experiments is toouse Reader and Writer modules.</span></span> <span data-ttu-id="781bb-205">modul čtečky Hello je použité tooload data do experimentu a modul zapisovače hello je toosave data z experimentů.</span><span class="sxs-lookup"><span data-stu-id="781bb-205">hello reader module is used tooload data into an experiment and hello writer module is toosave data from your experiments.</span></span> <span data-ttu-id="781bb-206">Podrobnosti o čtení a zápis modulech najdete v tématu [čtečky](https://msdn.microsoft.com/library/azure/dn905997.aspx) a [zapisovače](https://msdn.microsoft.com/library/azure/dn905984.aspx) témata v knihovně MSDN.</span><span class="sxs-lookup"><span data-stu-id="781bb-206">For details about reader and writer modules, see [Reader](https://msdn.microsoft.com/library/azure/dn905997.aspx) and [Writer](https://msdn.microsoft.com/library/azure/dn905984.aspx) topics on MSDN Library.</span></span>     

<span data-ttu-id="781bb-207">Pokud používáte moduly hello čtení a zápis, je vhodné toouse parametr webové služby pro každou vlastnost tyto moduly čtečky nebo zapisovač.</span><span class="sxs-lookup"><span data-stu-id="781bb-207">When using hello reader and writer modules, it is good practice toouse a Web service parameter for each property of these reader/writer modules.</span></span> <span data-ttu-id="781bb-208">Tyto parametry webové povolit hodnoty hello tooconfigure za běhu.</span><span class="sxs-lookup"><span data-stu-id="781bb-208">These web parameters enable you tooconfigure hello values during runtime.</span></span> <span data-ttu-id="781bb-209">Můžete například vytvořit experimentu s čtečky modul, který používá Azure SQL Database: XXX.database.windows.net.</span><span class="sxs-lookup"><span data-stu-id="781bb-209">For example, you could create an experiment with a reader module that uses an Azure SQL Database: XXX.database.windows.net.</span></span> <span data-ttu-id="781bb-210">Po nasazení webové služby hello chcete tooenable hello spotřebiteli hello webové služby toospecify názvem YYY.database.windows.net jiný Server SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="781bb-210">After hello web service has been deployed, you want tooenable hello consumers of hello web service toospecify another Azure SQL Server called YYY.database.windows.net.</span></span> <span data-ttu-id="781bb-211">Můžete vytvořit webové služby parametr tooallow tuto hodnotu toobe nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="781bb-211">You can use a Web service parameter tooallow this value toobe configured.</span></span>

> [!NOTE]
> <span data-ttu-id="781bb-212">Webové služby vstup a výstup se liší od parametry webové služby.</span><span class="sxs-lookup"><span data-stu-id="781bb-212">Web service input and output are different from Web service parameters.</span></span> <span data-ttu-id="781bb-213">V hello prvního scénáře jste viděli, jak vstup a výstup lze zadat pro Azure ML webové služby.</span><span class="sxs-lookup"><span data-stu-id="781bb-213">In hello first scenario, you have seen how an input and output can be specified for an Azure ML Web service.</span></span> <span data-ttu-id="781bb-214">V tomto scénáři můžete předat parametry webové služby, které odpovídají tooproperties modulů čtečky nebo zapisovač.</span><span class="sxs-lookup"><span data-stu-id="781bb-214">In this scenario, you pass parameters for a Web service that correspond tooproperties of reader/writer modules.</span></span>
>
>

<span data-ttu-id="781bb-215">Podívejme se na scénář použití parametry webové služby.</span><span class="sxs-lookup"><span data-stu-id="781bb-215">Let's look at a scenario for using Web service parameters.</span></span> <span data-ttu-id="781bb-216">Máte nasazené webové službě Azure Machine Learning, která používá čtečky modulu tooread data z jednoho z hello zdroje dat podporované rozhraním Azure Machine Learning (například: Azure SQL Database).</span><span class="sxs-lookup"><span data-stu-id="781bb-216">You have a deployed Azure Machine Learning web service that uses a reader module tooread data from one of hello data sources supported by Azure Machine Learning (for example: Azure SQL Database).</span></span> <span data-ttu-id="781bb-217">Po provedení dávkového spuštění hello hello výsledky jsou zapsány pomocí zapisovače modulu (Azure SQL Database).</span><span class="sxs-lookup"><span data-stu-id="781bb-217">After hello batch execution is performed, hello results are written using a Writer module (Azure SQL Database).</span></span>  <span data-ttu-id="781bb-218">Žádné webové služby vstupy a výstupy jsou definovány v hello experimenty.</span><span class="sxs-lookup"><span data-stu-id="781bb-218">No web service inputs and outputs are defined in hello experiments.</span></span> <span data-ttu-id="781bb-219">V takovém případě doporučujeme nakonfigurovat parametry relevantní webové služby pro čtení a zápis moduly hello.</span><span class="sxs-lookup"><span data-stu-id="781bb-219">In this case, we recommend that you configure relevant web service parameters for hello reader and writer modules.</span></span> <span data-ttu-id="781bb-220">Tato konfigurace umožňuje hello čtečky nebo zapisovač toobe moduly, které jsou nakonfigurované při použití hello AzureMLBatchExecution aktivity.</span><span class="sxs-lookup"><span data-stu-id="781bb-220">This configuration allows hello reader/writer modules toobe configured when using hello AzureMLBatchExecution activity.</span></span> <span data-ttu-id="781bb-221">Zadejte parametry webové služby v hello **globalParameters** část v kódu JSON aktivity hello následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="781bb-221">You specify Web service parameters in hello **globalParameters** section in hello activity JSON as follows.</span></span>

```JSON
"typeProperties": {
    "globalParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```

<span data-ttu-id="781bb-222">Můžete také použít [funkce objektu pro vytváření dat](data-factory-functions-variables.md) v předávání hodnot pro hello parametry webové služby, jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="781bb-222">You can also use [Data Factory Functions](data-factory-functions-variables.md) in passing values for hello Web service parameters as shown in hello following example:</span></span>

```JSON
"typeProperties": {
    "globalParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> <span data-ttu-id="781bb-223">parametry webové služby Hello malá a velká písmena, zajistěte proto, že hello názvy, které zadáte v aktivitě hello JSON odpovídají hello ty, které jsou vystavené hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="781bb-223">hello Web service parameters are case-sensitive, so ensure that hello names you specify in hello activity JSON match hello ones exposed by hello Web service.</span></span>
>
>

### <a name="using-a-reader-module-tooread-data-from-multiple-files-in-azure-blob"></a><span data-ttu-id="781bb-224">Pomocí čtečky modulu tooread dat z více souborů v Azure Blob</span><span class="sxs-lookup"><span data-stu-id="781bb-224">Using a Reader module tooread data from multiple files in Azure Blob</span></span>
<span data-ttu-id="781bb-225">Velké objemy dat kanálů s aktivity, například Pig a Hive můžete vytvořit jeden nebo více výstupních souborů bez přípony.</span><span class="sxs-lookup"><span data-stu-id="781bb-225">Big data pipelines with activities such as Pig and Hive can produce one or more output files with no extensions.</span></span> <span data-ttu-id="781bb-226">Například když zadáte externí tabulku Hive, hello data pro externí tabulku Hive hello mohou být uložena v úložišti objektů Azure blob s následující název 000000_0 hello.</span><span class="sxs-lookup"><span data-stu-id="781bb-226">For example, when you specify an external Hive table, hello data for hello external Hive table can be stored in Azure blob storage with hello following name 000000_0.</span></span> <span data-ttu-id="781bb-227">Můžete použít modul čtečky hello v tooread experimentu několik souborů a použít je k předpovědi.</span><span class="sxs-lookup"><span data-stu-id="781bb-227">You can use hello reader module in an experiment tooread multiple files, and use them for predictions.</span></span>

<span data-ttu-id="781bb-228">Pokud používáte modul čtečky hello v experimentu Azure Machine Learning, můžete zadat objektů Blob v Azure jako vstup.</span><span class="sxs-lookup"><span data-stu-id="781bb-228">When using hello reader module in an Azure Machine Learning experiment, you can specify Azure Blob as an input.</span></span> <span data-ttu-id="781bb-229">soubory Hello v hello úložiště objektů blob Azure může být hello výstupní soubory (Příklad: 000000_0), vznikají pomocí funkcí Pig a Hive skript spuštěný v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="781bb-229">hello files in hello Azure blob storage can be hello output files (Example: 000000_0) that are produced by a Pig and Hive script running on HDInsight.</span></span> <span data-ttu-id="781bb-230">Hello modul čtečky vám umožní tooread souborů (žádné) tak, že nakonfigurujete hello **toocontainer cestu, adresáře nebo objekt blob**.</span><span class="sxs-lookup"><span data-stu-id="781bb-230">hello reader module allows you tooread files (with no extensions) by configuring hello **Path toocontainer, directory/blob**.</span></span> <span data-ttu-id="781bb-231">Hello **cesta toocontainer** body toohello kontejneru a **adresáře nebo objekt blob** body toofolder, který obsahuje soubory hello, jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="781bb-231">hello **Path toocontainer** points toohello container and **directory/blob** points toofolder that contains hello files as shown in hello following image.</span></span> <span data-ttu-id="781bb-232">Hello se tedy hvězdička \*) **Určuje, že všechny soubory v kontejneru nebo složku, hello hello (to znamená, data, aggregateddata a rok 2014/měsíc-6 = /\*)** se čtou v rámci experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="781bb-232">hello asterisk that is, \*) **specifies that all hello files in hello container/folder (that is, data/aggregateddata/year=2014/month-6/\*)** are read as part of hello experiment.</span></span>

![Azure Blob vlastnosti](./media/data-factory-create-predictive-pipelines/azure-blob-properties.png)

### <a name="example"></a><span data-ttu-id="781bb-234">Příklad</span><span class="sxs-lookup"><span data-stu-id="781bb-234">Example</span></span>
#### <a name="pipeline-with-azuremlbatchexecution-activity-with-web-service-parameters"></a><span data-ttu-id="781bb-235">Kanál s aktivitou AzureMLBatchExecution s parametry webové služby</span><span class="sxs-lookup"><span data-stu-id="781bb-235">Pipeline with AzureMLBatchExecution activity with Web Service Parameters</span></span>

```JSON
{
  "name": "MLWithSqlReaderSqlWriter",
  "properties": {
    "description": "Azure ML model with sql azure reader/writer",
    "activities": [
      {
        "name": "MLSqlReaderSqlWriterActivity",
        "type": "AzureMLBatchExecution",
        "description": "test",
        "inputs": [
          {
            "name": "MLSqlInput"
          }
        ],
        "outputs": [
          {
            "name": "MLSqlOutput"
          }
        ],
        "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
        "typeProperties":
        {
            "webServiceInput": "MLSqlInput",
            "webServiceOutputs": {
                "output1": "MLSqlOutput"
            }
              "globalParameters": {
                "Database server name": "<myserver>.database.windows.net",
                "Database name": "<database>",
                "Server user account name": "<user name>",
                "Server user account password": "<password>"
              }              
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        },
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```

<span data-ttu-id="781bb-236">V hello výše příklad JSON:</span><span class="sxs-lookup"><span data-stu-id="781bb-236">In hello above JSON example:</span></span>

* <span data-ttu-id="781bb-237">Hello nasadit Azure Machine Learning webové služby používá čtečka čipových karet a zapisovače modulu tooread a zápis dat z / tooan Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="781bb-237">hello deployed Azure Machine Learning Web service uses a reader and a writer module tooread/write data from/tooan Azure SQL Database.</span></span> <span data-ttu-id="781bb-238">Tato webová služba zpřístupní hello následující čtyři parametry: název serveru, název databáze, název serveru uživatelského účtu a heslo uživatelského účtu serveru databáze.</span><span class="sxs-lookup"><span data-stu-id="781bb-238">This Web service exposes hello following four parameters:  Database server name, Database name, Server user account name, and Server user account password.</span></span>  
* <span data-ttu-id="781bb-239">Obě **spustit** a **end** času musí být v [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="781bb-239">Both **start** and **end** datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="781bb-240">Například: 2014-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="781bb-240">For example: 2014-10-14T16:32:41Z.</span></span> <span data-ttu-id="781bb-241">Hello **end** čas je volitelné.</span><span class="sxs-lookup"><span data-stu-id="781bb-241">hello **end** time is optional.</span></span> <span data-ttu-id="781bb-242">Pokud nezadáte hodnotu hello **end** vlastnost, vypočítá se jako "**start + 48 hodin.**"</span><span class="sxs-lookup"><span data-stu-id="781bb-242">If you do not specify value for hello **end** property, it is calculated as "**start + 48 hours.**"</span></span> <span data-ttu-id="781bb-243">bez omezení, zadejte toorun hello kanálu **9999-09-09** hello hodnotu hello **end** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="781bb-243">toorun hello pipeline indefinitely, specify **9999-09-09** as hello value for hello **end** property.</span></span> <span data-ttu-id="781bb-244">Podrobné informace o vlastnostech JSON najdete v tématu [JSON Scripting Reference](https://msdn.microsoft.com/library/dn835050.aspx) (Referenční příručka skriptování JSON).</span><span class="sxs-lookup"><span data-stu-id="781bb-244">See [JSON Scripting Reference](https://msdn.microsoft.com/library/dn835050.aspx) for details about JSON properties.</span></span>

### <a name="other-scenarios"></a><span data-ttu-id="781bb-245">Další scénáře</span><span class="sxs-lookup"><span data-stu-id="781bb-245">Other scenarios</span></span>
#### <a name="web-service-requires-multiple-inputs"></a><span data-ttu-id="781bb-246">Webová služba vyžaduje více vstupů</span><span class="sxs-lookup"><span data-stu-id="781bb-246">Web service requires multiple inputs</span></span>
<span data-ttu-id="781bb-247">Pokud hello webová služba přijímá více vstupů, použijte hello **webServiceInputs** vlastnost místo použití **webServiceInput**.</span><span class="sxs-lookup"><span data-stu-id="781bb-247">If hello web service takes multiple inputs, use hello **webServiceInputs** property instead of using **webServiceInput**.</span></span> <span data-ttu-id="781bb-248">Datové sady, které odkazují hello **webServiceInputs** musí také obsahovat hello aktivity **vstupy**.</span><span class="sxs-lookup"><span data-stu-id="781bb-248">Datasets that are referenced by hello **webServiceInputs** must also be included in hello Activity **inputs**.</span></span>

<span data-ttu-id="781bb-249">V experimentu Azure ML vstup webové služby a porty výstup a globální parametry mají výchozí názvy ("input1", "input2"), které můžete přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="781bb-249">In your Azure ML experiment, web service input and output ports and global parameters have default names ("input1", "input2") that you can customize.</span></span> <span data-ttu-id="781bb-250">názvy Hello, který použijete pro webServiceInputs, webServiceOutputs a globalParameters nastavení musí přesně shodovat hello názvy v experimentech hello.</span><span class="sxs-lookup"><span data-stu-id="781bb-250">hello names you use for webServiceInputs, webServiceOutputs, and globalParameters settings must exactly match hello names in hello experiments.</span></span> <span data-ttu-id="781bb-251">Datová část požadavku ukázka hello můžete zobrazit na stránce nápovědy spuštění dávky hello pro vaši Azure ML koncový bod tooverify hello očekává mapování.</span><span class="sxs-lookup"><span data-stu-id="781bb-251">You can view hello sample request payload on hello Batch Execution Help page for your Azure ML endpoint tooverify hello expected mapping.</span></span>

```JSON
{
    "name": "PredictivePipeline",
    "properties": {
        "description": "use AzureML model",
        "activities": [{
            "name": "MLActivity",
            "type": "AzureMLBatchExecution",
            "description": "prediction analysis on batch input",
            "inputs": [{
                "name": "inputDataset1"
            }, {
                "name": "inputDataset2"
            }],
            "outputs": [{
                "name": "outputDataset"
            }],
            "linkedServiceName": "MyAzureMLLinkedService",
            "typeProperties": {
                "webServiceInputs": {
                    "input1": "inputDataset1",
                    "input2": "inputDataset2"
                },
                "webServiceOutputs": {
                    "output1": "outputDataset"
                }
            },
            "policy": {
                "concurrency": 3,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "02:00:00"
            }
        }],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
    }
}
```

#### <a name="web-service-does-not-require-an-input"></a><span data-ttu-id="781bb-252">Webová služba nevyžaduje žádný vstup</span><span class="sxs-lookup"><span data-stu-id="781bb-252">Web Service does not require an input</span></span>
<span data-ttu-id="781bb-253">Azure ML dávky spuštění webové služby může být použité toorun žádný pracovní postup, například R nebo Python skripty, které ale nemusí vyžadovat všechny vstupy.</span><span class="sxs-lookup"><span data-stu-id="781bb-253">Azure ML batch execution web services can be used toorun any workflows, for example R or Python scripts, that may not require any inputs.</span></span> <span data-ttu-id="781bb-254">Nebo pomocí čtečky modulu, který nevystavuje žádné GlobalParameters mohou být nakonfigurované hello experimentu.</span><span class="sxs-lookup"><span data-stu-id="781bb-254">Or, hello experiment might be configured with a Reader module that does not expose any GlobalParameters.</span></span> <span data-ttu-id="781bb-255">V takovém případě hello AzureMLBatchExecution aktivity by být nakonfigurovány takto:</span><span class="sxs-lookup"><span data-stu-id="781bb-255">In that case, hello AzureMLBatchExecution Activity would be configured as follows:</span></span>

```JSON
{
    "name": "scoring service",
    "type": "AzureMLBatchExecution",
    "outputs": [
        {
            "name": "myBlob"
        }
    ],
    "typeProperties": {
        "webServiceOutputs": {
            "output1": "myBlob"
        }              
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

#### <a name="web-service-does-not-require-an-inputoutput"></a><span data-ttu-id="781bb-256">Webová služba nevyžaduje vstupu a výstupu</span><span class="sxs-lookup"><span data-stu-id="781bb-256">Web Service does not require an input/output</span></span>
<span data-ttu-id="781bb-257">Hello Azure ML dávky spuštění webové služby, nemusí mít žádný výstup webové služby nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="781bb-257">hello Azure ML batch execution web service might not have any Web Service output configured.</span></span> <span data-ttu-id="781bb-258">V tomto příkladu není žádná webová služba vstup nebo výstup, ani nejsou nakonfigurované žádné GlobalParameters.</span><span class="sxs-lookup"><span data-stu-id="781bb-258">In this example, there is no Web Service input or output, nor are any GlobalParameters configured.</span></span> <span data-ttu-id="781bb-259">Stále je nakonfigurované na hello aktivity samotný výstup, ale není zadané jako webServiceOutput.</span><span class="sxs-lookup"><span data-stu-id="781bb-259">There is still an output configured on hello activity itself, but it is not given as a webServiceOutput.</span></span>

```JSON
{
    "name": "retraining",
    "type": "AzureMLBatchExecution",
    "outputs": [
        {
            "name": "placeholderOutputDataset"
        }
    ],
    "typeProperties": {
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

#### <a name="web-service-uses-readers-and-writers-and-hello-activity-runs-only-when-other-activities-have-succeeded"></a><span data-ttu-id="781bb-260">Webové služby používá čtení a zápis a hello aktivity spustí jenom v případě, že ostatní aktivity proběhlo úspěšně.</span><span class="sxs-lookup"><span data-stu-id="781bb-260">Web Service uses readers and writers, and hello activity runs only when other activities have succeeded</span></span>
<span data-ttu-id="781bb-261">Hello Azure ML web čtení a zápis modulů služby může být nakonfigurované toorun s nebo bez jakékoli GlobalParameters.</span><span class="sxs-lookup"><span data-stu-id="781bb-261">hello Azure ML web service reader and writer modules might be configured toorun with or without any GlobalParameters.</span></span> <span data-ttu-id="781bb-262">Můžete však tooembed služby volá v kanál, který používá datovou sadu závislosti tooinvoke hello službu jenom v případě, že některé nadřazeného zpracování byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="781bb-262">However, you may want tooembed service calls in a pipeline that uses dataset dependencies tooinvoke hello service only when some upstream processing has completed.</span></span> <span data-ttu-id="781bb-263">Můžete ji spustit taky některých jiných akcí po dokončení spuštění dávek hello použití tohoto přístupu.</span><span class="sxs-lookup"><span data-stu-id="781bb-263">You can also trigger some other action after hello batch execution has completed using this approach.</span></span> <span data-ttu-id="781bb-264">V takovém případě lze vyjádřit pomocí aktivity vstupy a výstupy, bez pojmenování některý z nich jako webová služba vstupů nebo výstupů závislosti hello.</span><span class="sxs-lookup"><span data-stu-id="781bb-264">In that case, you can express hello dependencies using activity inputs and outputs, without naming any of them as Web Service inputs or outputs.</span></span>

```JSON
{
    "name": "retraining",
    "type": "AzureMLBatchExecution",
    "inputs": [
        {
            "name": "upstreamData1"
        },
        {
            "name": "upstreamData2"
        }
    ],
    "outputs": [
        {
            "name": "downstreamData"
        }
    ],
    "typeProperties": {
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

<span data-ttu-id="781bb-265">Hello **takeaways** jsou:</span><span class="sxs-lookup"><span data-stu-id="781bb-265">hello **takeaways** are:</span></span>

* <span data-ttu-id="781bb-266">Pokud váš koncový bod experiment používá webServiceInput: to je reprezentována datovou sadu objektu blob a je součástí vstupech do aktivity hello a vlastnost webServiceInput hello.</span><span class="sxs-lookup"><span data-stu-id="781bb-266">If your experiment endpoint uses a webServiceInput: it is represented by a blob dataset and is included in hello activity inputs and hello webServiceInput property.</span></span> <span data-ttu-id="781bb-267">Vlastnost webServiceInput hello, jinak je vynechán.</span><span class="sxs-lookup"><span data-stu-id="781bb-267">Otherwise, hello webServiceInput property is omitted.</span></span>
* <span data-ttu-id="781bb-268">Pokud váš koncový bod experiment používá webServiceOutput(s): jejich jsou reprezentované pomocí datové sady objektu blob a jsou zahrnuty v hello výstupů aktivity a ve vlastnosti webServiceOutputs hello.</span><span class="sxs-lookup"><span data-stu-id="781bb-268">If your experiment endpoint uses webServiceOutput(s): they are represented by blob datasets and are included in hello activity outputs and in hello webServiceOutputs property.</span></span> <span data-ttu-id="781bb-269">výstupem aktivity Hello a webServiceOutputs jsou mapovány podle názvu hello každý výstupu v experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="781bb-269">hello activity outputs and webServiceOutputs are mapped by hello name of each output in hello experiment.</span></span> <span data-ttu-id="781bb-270">Vlastnosti webServiceOutputs hello, jinak je vynechán.</span><span class="sxs-lookup"><span data-stu-id="781bb-270">Otherwise, hello webServiceOutputs property is omitted.</span></span>
* <span data-ttu-id="781bb-271">Pokud váš koncový bod experimentu zpřístupní globalParameter(s), jsou uvedené ve vlastnosti globalParameters aktivity hello jako páry klíčů, hodnota.</span><span class="sxs-lookup"><span data-stu-id="781bb-271">If your experiment endpoint exposes globalParameter(s), they are given in hello activity globalParameters property as key, value pairs.</span></span> <span data-ttu-id="781bb-272">Vlastnosti globalParameters hello, jinak je vynechán.</span><span class="sxs-lookup"><span data-stu-id="781bb-272">Otherwise, hello globalParameters property is omitted.</span></span> <span data-ttu-id="781bb-273">Hello klíče jsou malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="781bb-273">hello keys are case-sensitive.</span></span> <span data-ttu-id="781bb-274">[Azure Data Factory funkce](data-factory-functions-variables.md) je možné použít hodnoty hello.</span><span class="sxs-lookup"><span data-stu-id="781bb-274">[Azure Data Factory functions](data-factory-functions-variables.md) may be used in hello values.</span></span>
* <span data-ttu-id="781bb-275">Další datové sady může být součástí vstupy a výstupy vlastnosti aktivity hello bez se na ně odkazovat v rámci typeProperties hello aktivity.</span><span class="sxs-lookup"><span data-stu-id="781bb-275">Additional datasets may be included in hello Activity inputs and outputs properties, without being referenced in hello Activity typeProperties.</span></span> <span data-ttu-id="781bb-276">Tyto datové sady řídí provádění pomocí závislosti řezu ale ignorují jinak hello AzureMLBatchExecution aktivity.</span><span class="sxs-lookup"><span data-stu-id="781bb-276">These datasets govern execution using slice dependencies but are otherwise ignored by hello AzureMLBatchExecution Activity.</span></span>


## <a name="updating-models-using-update-resource-activity"></a><span data-ttu-id="781bb-277">Aktualizace modelů pomocí aktivita prostředku aktualizace</span><span class="sxs-lookup"><span data-stu-id="781bb-277">Updating models using Update Resource Activity</span></span>
<span data-ttu-id="781bb-278">Po dokončení práce s retraining, aktualizujte hello vyhodnocování webové služby (zveřejněné jako webové služby prediktivní experiment) s nově trained model hello pomocí hello **aktivita prostředku aktualizace Azure ML**.</span><span class="sxs-lookup"><span data-stu-id="781bb-278">After you are done with retraining, update hello scoring web service (predictive experiment exposed as a web service) with hello newly trained model by using hello **Azure ML Update Resource Activity**.</span></span> <span data-ttu-id="781bb-279">V tématu [aktualizace modelů pomocí aktivita prostředku aktualizace](data-factory-azure-ml-update-resource-activity.md) článku.</span><span class="sxs-lookup"><span data-stu-id="781bb-279">See [Updating models using Update Resource Activity](data-factory-azure-ml-update-resource-activity.md) article for details.</span></span>

### <a name="reader-and-writer-modules"></a><span data-ttu-id="781bb-280">Čtečka a zapisovače moduly</span><span class="sxs-lookup"><span data-stu-id="781bb-280">Reader and Writer Modules</span></span>
<span data-ttu-id="781bb-281">Obvyklým scénářem pro používání parametry webové služby je hello používání Azure SQL čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="781bb-281">A common scenario for using Web service parameters is hello use of Azure SQL Readers and Writers.</span></span> <span data-ttu-id="781bb-282">modul čtečky Hello je použité tooload data do experimentu z datových služeb správy mimo Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="781bb-282">hello reader module is used tooload data into an experiment from data management services outside Azure Machine Learning Studio.</span></span> <span data-ttu-id="781bb-283">modul zapisovače Hello je toosave data z experimentů do služby pro data mimo Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="781bb-283">hello writer module is toosave data from your experiments into data management services outside Azure Machine Learning Studio.</span></span>  

<span data-ttu-id="781bb-284">Podrobnosti o čtečky nebo zápis Azure Blob nebo Azure SQL najdete v tématu [čtečky](https://msdn.microsoft.com/library/azure/dn905997.aspx) a [zapisovače](https://msdn.microsoft.com/library/azure/dn905984.aspx) témata v knihovně MSDN.</span><span class="sxs-lookup"><span data-stu-id="781bb-284">For details about Azure Blob/Azure SQL reader/writer, see [Reader](https://msdn.microsoft.com/library/azure/dn905997.aspx) and [Writer](https://msdn.microsoft.com/library/azure/dn905984.aspx) topics on MSDN Library.</span></span> <span data-ttu-id="781bb-285">Příklad Hello v předchozí části hello používá hello objektů Blob v Azure čtení a zápis objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="781bb-285">hello example in hello previous section used hello Azure Blob reader and Azure Blob writer.</span></span> <span data-ttu-id="781bb-286">Tato část popisuje používání Azure SQL čtení a zápis Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="781bb-286">This section discusses using Azure SQL reader and Azure SQL writer.</span></span>

## <a name="frequently-asked-questions"></a><span data-ttu-id="781bb-287">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="781bb-287">Frequently asked questions</span></span>
<span data-ttu-id="781bb-288">**Otázka:** mám více souborů, které jsou generovány nástrojem Moje velkých datových kanálů.</span><span class="sxs-lookup"><span data-stu-id="781bb-288">**Q:** I have multiple files that are generated by my big data pipelines.</span></span> <span data-ttu-id="781bb-289">Můžete použít hello AzureMLBatchExecution aktivity toowork u všech souborů hello?</span><span class="sxs-lookup"><span data-stu-id="781bb-289">Can I use hello AzureMLBatchExecution Activity toowork on all hello files?</span></span>

<span data-ttu-id="781bb-290">**Odpověď:** Ano.</span><span class="sxs-lookup"><span data-stu-id="781bb-290">**A:** Yes.</span></span> <span data-ttu-id="781bb-291">V tématu hello **pomocí čtečky modulu tooread dat z více souborů v Azure Blob** podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="781bb-291">See hello **Using a Reader module tooread data from multiple files in Azure Blob** section for details.</span></span>

## <a name="azure-ml-batch-scoring-activity"></a><span data-ttu-id="781bb-292">Azure ML dávkové vyhodnocování aktivity</span><span class="sxs-lookup"><span data-stu-id="781bb-292">Azure ML Batch Scoring Activity</span></span>
<span data-ttu-id="781bb-293">Pokud používáte hello **AzureMLBatchScoring** aktivity toointegrate pomocí Azure Machine Learning, doporučujeme používat nejnovější hello **AzureMLBatchExecution** aktivity.</span><span class="sxs-lookup"><span data-stu-id="781bb-293">If you are using hello **AzureMLBatchScoring** activity toointegrate with Azure Machine Learning, we recommend that you use hello latest **AzureMLBatchExecution** activity.</span></span>

<span data-ttu-id="781bb-294">Hello AzureMLBatchExecution aktivita byla zavedená v hello srpen 2015 verzi sady Azure SDK a prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="781bb-294">hello AzureMLBatchExecution activity is introduced in hello August 2015 release of Azure SDK and Azure PowerShell.</span></span>

<span data-ttu-id="781bb-295">Pokud chcete toocontinue pomocí hello AzureMLBatchScoring aktivity, pokračujte v načítání prostřednictvím této části.</span><span class="sxs-lookup"><span data-stu-id="781bb-295">If you want toocontinue using hello AzureMLBatchScoring activity, continue reading through this section.</span></span>  

### <a name="azure-ml-batch-scoring-activity-using-azure-storage-for-inputoutput"></a><span data-ttu-id="781bb-296">Použití služby Azure Storage pro vstupní a výstupní Azure aktivity ML dávkového vyhodnocování</span><span class="sxs-lookup"><span data-stu-id="781bb-296">Azure ML Batch Scoring activity using Azure Storage for input/output</span></span>

```JSON
{
  "name": "PredictivePipeline",
  "properties": {
    "description": "use AzureML model",
    "activities": [
      {
        "name": "MLActivity",
        "type": "AzureMLBatchScoring",
        "description": "prediction analysis on batch input",
        "inputs": [
          {
            "name": "ScoringInputBlob"
          }
        ],
        "outputs": [
          {
            "name": "ScoringResultBlob"
          }
        ],
        "linkedServiceName": "MyAzureMLLinkedService",
        "policy": {
          "concurrency": 3,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        }
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```

### <a name="web-service-parameters"></a><span data-ttu-id="781bb-297">Parametry webové služby</span><span class="sxs-lookup"><span data-stu-id="781bb-297">Web Service Parameters</span></span>
<span data-ttu-id="781bb-298">Přidat toospecify hodnoty pro parametry webové služby, **rámci typeProperties** části toohello **AzureMLBatchScoringActivty** část v kanálu hello JSON, jak ukazuje následující příklad hello:</span><span class="sxs-lookup"><span data-stu-id="781bb-298">toospecify values for Web service parameters, add a **typeProperties** section toohello **AzureMLBatchScoringActivty** section in hello pipeline JSON as shown in hello following example:</span></span>

```JSON
"typeProperties": {
    "webServiceParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```
<span data-ttu-id="781bb-299">Můžete také použít [funkce objektu pro vytváření dat](data-factory-functions-variables.md) v předávání hodnot pro hello parametry webové služby, jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="781bb-299">You can also use [Data Factory Functions](data-factory-functions-variables.md) in passing values for hello Web service parameters as shown in hello following example:</span></span>

```JSON
"typeProperties": {
    "webServiceParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> <span data-ttu-id="781bb-300">parametry webové služby Hello malá a velká písmena, zajistěte proto, že hello názvy, které zadáte v aktivitě hello JSON odpovídají hello ty, které jsou vystavené hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="781bb-300">hello Web service parameters are case-sensitive, so ensure that hello names you specify in hello activity JSON match hello ones exposed by hello Web service.</span></span>
>
>

## <a name="see-also"></a><span data-ttu-id="781bb-301">Viz také</span><span class="sxs-lookup"><span data-stu-id="781bb-301">See Also</span></span>
* [<span data-ttu-id="781bb-302">Azure příspěvku na blogu: Začínáme s Azure Data Factory a Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="781bb-302">Azure blog post: Getting started with Azure Data Factory and Azure Machine Learning</span></span>](https://azure.microsoft.com/blog/getting-started-with-azure-data-factory-and-azure-machine-learning-4/)

[adf-build-1st-pipeline]: data-factory-build-your-first-pipeline.md

[azure-machine-learning]: http://azure.microsoft.com/services/machine-learning/
