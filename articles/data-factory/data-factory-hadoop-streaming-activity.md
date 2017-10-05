---
title: "Transformace dat pomocí streamované aktivitě Hadoop - Azure | Microsoft Docs"
description: "Zjistěte, jak můžete pomocí streamované aktivitě Hadoop v objektu pro vytváření dat Azure pro transformaci dat spuštěním streamování Hadoop programy na na vyžádání nebo vaše vlastní cluster HDInsight."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 4c3ff8f2-2c00-434e-a416-06dfca2c41ec
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: shlo
ms.openlocfilehash: bfe62aa60f5a0ff339e1d495d22a5fdfac10d5dc
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="transform-data-using-hadoop-streaming-activity-in-azure-data-factory"></a><span data-ttu-id="ed5f0-103">Transformace dat pomocí streamované aktivitě Hadoop v Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="ed5f0-103">Transform data using Hadoop Streaming Activity in Azure Data Factory</span></span>
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="ed5f0-104">Aktivita Hive</span><span class="sxs-lookup"><span data-stu-id="ed5f0-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="ed5f0-105">Pig aktivity</span><span class="sxs-lookup"><span data-stu-id="ed5f0-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="ed5f0-106">Činnost MapReduce</span><span class="sxs-lookup"><span data-stu-id="ed5f0-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="ed5f0-107">Streamované aktivitě Hadoop</span><span class="sxs-lookup"><span data-stu-id="ed5f0-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="ed5f0-108">Spark aktivity</span><span class="sxs-lookup"><span data-stu-id="ed5f0-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="ed5f0-109">Aktivita Provedení dávky služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="ed5f0-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="ed5f0-110">Aktivita Aktualizace prostředků služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="ed5f0-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="ed5f0-111">Aktivita Uložená procedura</span><span class="sxs-lookup"><span data-stu-id="ed5f0-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="ed5f0-112">Aktivita U-SQL služby Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="ed5f0-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="ed5f0-113">Vlastní aktivity rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="ed5f0-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="ed5f0-114">Můžete použít HDInsightStreamingActivity aktivity vyvolání úlohu streamování Hadoop z kanál služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-114">You can use the HDInsightStreamingActivity Activity invoke a Hadoop Streaming job from an Azure Data Factory pipeline.</span></span> <span data-ttu-id="ed5f0-115">Následující fragment kódu JSON ukazuje syntaxi pro použití HDInsightStreamingActivity v souboru JSON kanálu.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-115">The following JSON snippet shows the syntax for using the HDInsightStreamingActivity in a pipeline JSON file.</span></span> 

<span data-ttu-id="ed5f0-116">HDInsight streamované aktivitě v datové továrně [kanálu](data-factory-create-pipelines.md) provede streamování Hadoop programy na [vlastní](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) nebo [na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) clusteru HDInsight se systémem Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-116">The HDInsight Streaming Activity in a Data Factory [pipeline](data-factory-create-pipelines.md) executes Hadoop Streaming programs on [your own](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or [on-demand](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-based HDInsight cluster.</span></span> <span data-ttu-id="ed5f0-117">Tento článek vychází [aktivit transformace dat](data-factory-data-transformation-activities.md) článek, který poskytne Obecné přehled o transformaci dat a aktivity podporované transformace.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-117">This article builds on the [data transformation activities](data-factory-data-transformation-activities.md) article, which presents a general overview of data transformation and the supported transformation activities.</span></span>

> [!NOTE] 
> <span data-ttu-id="ed5f0-118">Pokud jste do Azure Data Factory nové, přečtěte si [Úvod do Azure Data Factory](data-factory-introduction.md) a proveďte kurz: [sestavit svůj první kanál dat](data-factory-build-your-first-pipeline.md) před přečtení tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-118">If you are new to Azure Data Factory, read through [Introduction to Azure Data Factory](data-factory-introduction.md) and do the tutorial: [Build your first data pipeline](data-factory-build-your-first-pipeline.md) before reading this article.</span></span> 

## <a name="json-sample"></a><span data-ttu-id="ed5f0-119">Ukázka JSON</span><span class="sxs-lookup"><span data-stu-id="ed5f0-119">JSON sample</span></span>
<span data-ttu-id="ed5f0-120">HDInsight cluster se automaticky zadá příklad programy (wc.exe a cat.exe) a data (davinci.txt).</span><span class="sxs-lookup"><span data-stu-id="ed5f0-120">The HDInsight cluster is automatically populated with example programs (wc.exe and cat.exe) and data (davinci.txt).</span></span> <span data-ttu-id="ed5f0-121">Ve výchozím nastavení je název kontejneru, který je používán clusteru HDInsight název samotného clusteru.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-121">By default, name of the container that is used by the HDInsight cluster is the name of the cluster itself.</span></span> <span data-ttu-id="ed5f0-122">Například pokud je název clusteru myhdicluster, bude název kontejneru objektu blob přidruženého myhdicluster.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-122">For example, if your cluster name is myhdicluster, name of the blob container associated would be myhdicluster.</span></span> 

```JSON
{
    "name": "HadoopStreamingPipeline",
    "properties": {
        "description": "Hadoop Streaming Demo",
        "activities": [
            {
                "type": "HDInsightStreaming",
                "typeProperties": {
                    "mapper": "cat.exe",
                    "reducer": "wc.exe",
                    "input": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                    "output": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                    "filePaths": [
                        "<nameofthecluster>/example/apps/wc.exe",
                        "<nameofthecluster>/example/apps/cat.exe"
                    ],
                    "fileLinkedService": "AzureStorageLinkedService",
                    "getDebugInfo": "Failure"
                },
                "outputs": [
                    {
                        "name": "StreamingOutputDataset"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "RunHadoopStreamingJob",
                "description": "Run a Hadoop streaming job",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2014-01-04T00:00:00Z",
        "end": "2014-01-05T00:00:00Z"
    }
}
```

<span data-ttu-id="ed5f0-123">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="ed5f0-123">Note the following points:</span></span>

1. <span data-ttu-id="ed5f0-124">Nastavte **linkedServiceName** název propojené služby, která odkazuje na vaše HDInsight clusteru na úlohu streamování mapreduce běží.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-124">Set the **linkedServiceName** to the name of the linked service that points to your HDInsight cluster on which the streaming mapreduce job is run.</span></span>
2. <span data-ttu-id="ed5f0-125">Nastavte typ aktivity na **HDInsightStreaming**.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-125">Set the type of the activity to **HDInsightStreaming**.</span></span>
3. <span data-ttu-id="ed5f0-126">Pro **mapper** vlastnost, zadejte název spustitelného souboru mapper.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-126">For the **mapper** property, specify the name of mapper executable.</span></span> <span data-ttu-id="ed5f0-127">V příkladu je cat.exe mapper spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-127">In the example, cat.exe is the mapper executable.</span></span>
4. <span data-ttu-id="ed5f0-128">Pro **reduktorem** vlastnost, zadejte název spustitelného souboru reduktorem.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-128">For the **reducer** property, specify the name of reducer executable.</span></span> <span data-ttu-id="ed5f0-129">V příkladu je wc.exe reduktorem spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-129">In the example, wc.exe is the reducer executable.</span></span>
5. <span data-ttu-id="ed5f0-130">Pro **vstupní** zadejte vlastnost, zadejte vstupní soubor (včetně umístění) pro mapper.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-130">For the **input** type property, specify the input file (including the location) for the mapper.</span></span> <span data-ttu-id="ed5f0-131">Příklad: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample je kontejner objektů blob, například/data/Gutenberg je složka, a davinci.txt je objekt blob.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-131">In the example: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample is the blob container, example/data/Gutenberg is the folder, and davinci.txt is the blob.</span></span>
6. <span data-ttu-id="ed5f0-132">Pro **výstup** zadejte vlastnost, pro reduktorem zadejte výstupní soubor (včetně umístění).</span><span class="sxs-lookup"><span data-stu-id="ed5f0-132">For the **output** type property, specify the output file (including the location) for the reducer.</span></span> <span data-ttu-id="ed5f0-133">Výstup úlohy streamování Hadoop je zapsán do umístění zadané pro tuto vlastnost.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-133">The output of the Hadoop Streaming job is written to the location specified for this property.</span></span>
7. <span data-ttu-id="ed5f0-134">V **filePaths** části, zadejte cesty pro spustitelné soubory mapper a reduktorem.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-134">In the **filePaths** section, specify the paths for the mapper and reducer executables.</span></span> <span data-ttu-id="ed5f0-135">Příklad: "adfsample/example/apps/wc.exe" adfsample je kontejner objektů blob, příklad nebo aplikací je složka a wc.exe je spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-135">In the example: "adfsample/example/apps/wc.exe", adfsample is the blob container, example/apps is the folder, and wc.exe is the executable.</span></span>
8. <span data-ttu-id="ed5f0-136">Pro **fileLinkedService** vlastnost, zadejte služby Azure Storage, propojené, který představuje službu Azure storage, který obsahuje soubory zadané v části filePaths.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-136">For the **fileLinkedService** property, specify the Azure Storage linked service that represents the Azure storage that contains the files specified in the filePaths section.</span></span>
9. <span data-ttu-id="ed5f0-137">Pro **argumenty** vlastnost, zadejte argumenty pro úlohu streamování.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-137">For the **arguments** property, specify the arguments for the streaming job.</span></span>
10. <span data-ttu-id="ed5f0-138">**Getdebuginfo –** vlastnost je volitelná elementu.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-138">The **getDebugInfo** property is an optional element.</span></span> <span data-ttu-id="ed5f0-139">Pokud je nastavena k chybě, protokoly se stáhnou pouze při selhání.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-139">When it is set to Failure, the logs are downloaded only on failure.</span></span> <span data-ttu-id="ed5f0-140">Pokud je nastavený na vždy, protokoly budou staženy vždy bez ohledu na stav spuštění.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-140">When it is set to Always, logs are always downloaded irrespective of the execution status.</span></span>

> [!NOTE]
> <span data-ttu-id="ed5f0-141">Jak je znázorněno v příkladu, zadáte pro streamované aktivitě Hadoop pro datovou sadu výstupů **výstupy** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-141">As shown in the example, you specify an output dataset for the Hadoop Streaming Activity for the **outputs** property.</span></span> <span data-ttu-id="ed5f0-142">Tato datová sada je právě fiktivní datovou sadu, která je potřeba jednotka plán kanálu.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-142">This dataset is just a dummy dataset that is required to drive the pipeline schedule.</span></span> <span data-ttu-id="ed5f0-143">Není potřeba zadat aktivity pro všechny vstupní datové sady **vstupy** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-143">You do not need to specify any input dataset for the activity for the **inputs** property.</span></span>  
> 
> 

## <a name="example"></a><span data-ttu-id="ed5f0-144">Příklad</span><span class="sxs-lookup"><span data-stu-id="ed5f0-144">Example</span></span>
<span data-ttu-id="ed5f0-145">Kanál v tomto návodu se spustí program streamování mapy nebo snižte počet slov v clusteru Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-145">The pipeline in this walkthrough runs the Word Count streaming Map/Reduce program on your Azure HDInsight cluster.</span></span> 

### <a name="linked-services"></a><span data-ttu-id="ed5f0-146">Propojené služby</span><span class="sxs-lookup"><span data-stu-id="ed5f0-146">Linked services</span></span>
#### <a name="azure-storage-linked-service"></a><span data-ttu-id="ed5f0-147">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="ed5f0-147">Azure Storage linked service</span></span>
<span data-ttu-id="ed5f0-148">Nejdřív vytvoříte propojené služby Azure Storage, který se používá cluster Azure HDInsight do Azure data factory propojení.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-148">First, you create a linked service to link the Azure Storage that is used by the Azure HDInsight cluster to the Azure data factory.</span></span> <span data-ttu-id="ed5f0-149">Pokud jste zkopírujte a vložte následující kód, nezapomeňte nahradit název účtu a klíč účtu název a klíč vašeho úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-149">If you copy/paste the following code, do not forget to replace account name and account key with the name and key of your Azure Storage.</span></span> 

```JSON
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
        }
    }
}
```

#### <a name="azure-hdinsight-linked-service"></a><span data-ttu-id="ed5f0-150">Azure propojené služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="ed5f0-150">Azure HDInsight linked service</span></span>
<span data-ttu-id="ed5f0-151">Dále vytvoříte propojené služby propojení clusteru Azure HDInsight do Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-151">Next, you create a linked service to link your Azure HDInsight cluster to the Azure data factory.</span></span> <span data-ttu-id="ed5f0-152">Pokud jste zkopírujte a vložte následující kód, nahraďte název clusteru HDInsight s názvem clusteru HDInsight a změňte hodnoty uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-152">If you copy/paste the following code, replace HDInsight cluster name with the name of your HDInsight cluster, and change user name and password values.</span></span> 

```JSON
{
    "name": "HDInsightLinkedService",
    "properties": {
        "type": "HDInsight",
        "typeProperties": {
            "clusterUri": "https://<HDInsight cluster name>.azurehdinsight.net",
            "userName": "admin",
            "password": "**********",
            "linkedServiceName": "StorageLinkedService"
        }
    }
}
```

### <a name="datasets"></a><span data-ttu-id="ed5f0-153">Datové sady</span><span class="sxs-lookup"><span data-stu-id="ed5f0-153">Datasets</span></span>
#### <a name="output-dataset"></a><span data-ttu-id="ed5f0-154">Výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="ed5f0-154">Output dataset</span></span>
<span data-ttu-id="ed5f0-155">Kanál v tomto příkladu nevyžaduje žádné vstupy.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-155">The pipeline in this example does not take any inputs.</span></span> <span data-ttu-id="ed5f0-156">Zadáte datovou sadu výstupů aktivity HDInsight streamování.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-156">You specify an output dataset for the HDInsight Streaming Activity.</span></span> <span data-ttu-id="ed5f0-157">Tato datová sada je právě fiktivní datovou sadu, která je potřeba jednotka plán kanálu.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-157">This dataset is just a dummy dataset that is required to drive the pipeline schedule.</span></span> 

```JSON
{
    "name": "StreamingOutputDataset",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "adftutorial/streamingdata/",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            },
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

### <a name="pipeline"></a><span data-ttu-id="ed5f0-158">Kanál</span><span class="sxs-lookup"><span data-stu-id="ed5f0-158">Pipeline</span></span>
<span data-ttu-id="ed5f0-159">Kanál v tomto příkladu má jenom jedna aktivita, která je typu: **HDInsightStreaming**.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-159">The pipeline in this example has only one activity that is of type: **HDInsightStreaming**.</span></span> 

<span data-ttu-id="ed5f0-160">HDInsight cluster se automaticky zadá příklad programy (wc.exe a cat.exe) a data (davinci.txt).</span><span class="sxs-lookup"><span data-stu-id="ed5f0-160">The HDInsight cluster is automatically populated with example programs (wc.exe and cat.exe) and data (davinci.txt).</span></span> <span data-ttu-id="ed5f0-161">Ve výchozím nastavení je název kontejneru, který je používán clusteru HDInsight název samotného clusteru.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-161">By default, name of the container that is used by the HDInsight cluster is the name of the cluster itself.</span></span> <span data-ttu-id="ed5f0-162">Například pokud je název clusteru myhdicluster, bude název kontejneru objektu blob přidruženého myhdicluster.</span><span class="sxs-lookup"><span data-stu-id="ed5f0-162">For example, if your cluster name is myhdicluster, name of the blob container associated would be myhdicluster.</span></span>  

```JSON
{
    "name": "HadoopStreamingPipeline",
    "properties": {
        "description": "Hadoop Streaming Demo",
        "activities": [
            {
                "type": "HDInsightStreaming",
                "typeProperties": {
                    "mapper": "cat.exe",
                    "reducer": "wc.exe",
                    "input": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                    "output": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                    "filePaths": [
                        "<blobcontainer>/example/apps/wc.exe",
                        "<blobcontainer>/example/apps/cat.exe"
                    ],
                    "fileLinkedService": "StorageLinkedService"
                },
                "outputs": [
                    {
                        "name": "StreamingOutputDataset"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "RunHadoopStreamingJob",
                "description": "Run a Hadoop streaming job",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-01-03T00:00:00Z",
        "end": "2017-01-04T00:00:00Z"
    }
}
```
## <a name="see-also"></a><span data-ttu-id="ed5f0-163">Viz také</span><span class="sxs-lookup"><span data-stu-id="ed5f0-163">See Also</span></span>
* [<span data-ttu-id="ed5f0-164">Aktivita Hive</span><span class="sxs-lookup"><span data-stu-id="ed5f0-164">Hive Activity</span></span>](data-factory-hive-activity.md)
* [<span data-ttu-id="ed5f0-165">Pig aktivity</span><span class="sxs-lookup"><span data-stu-id="ed5f0-165">Pig Activity</span></span>](data-factory-pig-activity.md)
* [<span data-ttu-id="ed5f0-166">Činnost MapReduce</span><span class="sxs-lookup"><span data-stu-id="ed5f0-166">MapReduce Activity</span></span>](data-factory-map-reduce.md)
* [<span data-ttu-id="ed5f0-167">Vyvolání programů Spark</span><span class="sxs-lookup"><span data-stu-id="ed5f0-167">Invoke Spark programs</span></span>](data-factory-spark.md)
* [<span data-ttu-id="ed5f0-168">Vyvolání skriptů jazyka R</span><span class="sxs-lookup"><span data-stu-id="ed5f0-168">Invoke R scripts</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

