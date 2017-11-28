---
title: "aaaTransform dat pomocí streamované aktivitě Hadoop - Azure | Microsoft Docs"
description: "Zjistěte, jak můžete pomocí hello streamované aktivitě Hadoop ve model služby Azure data factory tootransform data spuštěním streamování Hadoop programy na na vyžádání nebo vaše vlastní cluster HDInsight."
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
ms.openlocfilehash: a7ddb7268f47162709a9c8136ccd69e0b7d4ad7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="transform-data-using-hadoop-streaming-activity-in-azure-data-factory"></a><span data-ttu-id="f9fc2-103">Transformace dat pomocí streamované aktivitě Hadoop v Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="f9fc2-103">Transform data using Hadoop Streaming Activity in Azure Data Factory</span></span>
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="f9fc2-104">Aktivita Hive</span><span class="sxs-lookup"><span data-stu-id="f9fc2-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="f9fc2-105">Pig aktivity</span><span class="sxs-lookup"><span data-stu-id="f9fc2-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="f9fc2-106">Činnost MapReduce</span><span class="sxs-lookup"><span data-stu-id="f9fc2-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="f9fc2-107">Streamované aktivitě Hadoop</span><span class="sxs-lookup"><span data-stu-id="f9fc2-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="f9fc2-108">Spark aktivity</span><span class="sxs-lookup"><span data-stu-id="f9fc2-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="f9fc2-109">Aktivita Provedení dávky služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="f9fc2-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="f9fc2-110">Aktivita Aktualizace prostředků služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="f9fc2-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="f9fc2-111">Aktivita Uložená procedura</span><span class="sxs-lookup"><span data-stu-id="f9fc2-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="f9fc2-112">Aktivita U-SQL služby Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="f9fc2-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="f9fc2-113">Vlastní aktivity rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="f9fc2-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="f9fc2-114">Můžete použít hello HDInsightStreamingActivity aktivity vyvolání úlohu streamování Hadoop z kanál služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-114">You can use hello HDInsightStreamingActivity Activity invoke a Hadoop Streaming job from an Azure Data Factory pipeline.</span></span> <span data-ttu-id="f9fc2-115">Hello následující fragment kódu JSON ukazuje hello syntaxe pro používání hello HDInsightStreamingActivity v souboru JSON kanálu.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-115">hello following JSON snippet shows hello syntax for using hello HDInsightStreamingActivity in a pipeline JSON file.</span></span> 

<span data-ttu-id="f9fc2-116">Hello HDInsight streamované aktivitě v datové továrně [kanálu](data-factory-create-pipelines.md) provede streamování Hadoop programy na [vlastní](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) nebo [na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight se systémem Windows nebo Linux cluster.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-116">hello HDInsight Streaming Activity in a Data Factory [pipeline](data-factory-create-pipelines.md) executes Hadoop Streaming programs on [your own](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or [on-demand](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-based HDInsight cluster.</span></span> <span data-ttu-id="f9fc2-117">Tento článek vychází hello [aktivit transformace dat](data-factory-data-transformation-activities.md) článek, který poskytne Obecné přehled o transformaci dat a aktivit transformace hello podporována.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-117">This article builds on hello [data transformation activities](data-factory-data-transformation-activities.md) article, which presents a general overview of data transformation and hello supported transformation activities.</span></span>

> [!NOTE] 
> <span data-ttu-id="f9fc2-118">Pokud jste tooAzure nový objekt pro vytváření dat, pročtěte [Úvod tooAzure Data Factory](data-factory-introduction.md) a hello kurz: [sestavit svůj první kanál dat](data-factory-build-your-first-pipeline.md) před přečtení tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-118">If you are new tooAzure Data Factory, read through [Introduction tooAzure Data Factory](data-factory-introduction.md) and do hello tutorial: [Build your first data pipeline](data-factory-build-your-first-pipeline.md) before reading this article.</span></span> 

## <a name="json-sample"></a><span data-ttu-id="f9fc2-119">Ukázka JSON</span><span class="sxs-lookup"><span data-stu-id="f9fc2-119">JSON sample</span></span>
<span data-ttu-id="f9fc2-120">Hello clusteru HDInsight se automaticky zadá příklad programy (wc.exe a cat.exe) a data (davinci.txt).</span><span class="sxs-lookup"><span data-stu-id="f9fc2-120">hello HDInsight cluster is automatically populated with example programs (wc.exe and cat.exe) and data (davinci.txt).</span></span> <span data-ttu-id="f9fc2-121">Ve výchozím nastavení je název hello kontejneru, který je používán clusteru HDInsight hello hello název samotného clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-121">By default, name of hello container that is used by hello HDInsight cluster is hello name of hello cluster itself.</span></span> <span data-ttu-id="f9fc2-122">Například pokud je název clusteru myhdicluster, bude název kontejneru objektu blob hello přidruženého myhdicluster.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-122">For example, if your cluster name is myhdicluster, name of hello blob container associated would be myhdicluster.</span></span> 

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

<span data-ttu-id="f9fc2-123">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="f9fc2-123">Note hello following points:</span></span>

1. <span data-ttu-id="f9fc2-124">Sada hello **linkedServiceName** toohello název hello propojené služby, který odkazuje clusteru HDInsight tooyour které hello streamování mapreduce se spustí úloha.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-124">Set hello **linkedServiceName** toohello name of hello linked service that points tooyour HDInsight cluster on which hello streaming mapreduce job is run.</span></span>
2. <span data-ttu-id="f9fc2-125">Nastavte typ hello hello aktivity příliš**HDInsightStreaming**.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-125">Set hello type of hello activity too**HDInsightStreaming**.</span></span>
3. <span data-ttu-id="f9fc2-126">Pro hello **mapper** vlastnost, zadejte název spustitelného souboru mapper hello.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-126">For hello **mapper** property, specify hello name of mapper executable.</span></span> <span data-ttu-id="f9fc2-127">V příkladu hello je cat.exe hello mapper spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-127">In hello example, cat.exe is hello mapper executable.</span></span>
4. <span data-ttu-id="f9fc2-128">Pro hello **reduktorem** vlastnost, zadejte název spustitelného souboru reduktorem hello.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-128">For hello **reducer** property, specify hello name of reducer executable.</span></span> <span data-ttu-id="f9fc2-129">V příkladu hello je wc.exe hello reduktorem spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-129">In hello example, wc.exe is hello reducer executable.</span></span>
5. <span data-ttu-id="f9fc2-130">Pro hello **vstupní** zadejte vlastnost, zadejte hello vstupní soubor (včetně umístění hello) pro hello mapper.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-130">For hello **input** type property, specify hello input file (including hello location) for hello mapper.</span></span> <span data-ttu-id="f9fc2-131">V příkladu hello: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample je kontejner objektů blob hello, například/data/Gutenberg je složka hello a davinci.txt je hello blob.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-131">In hello example: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample is hello blob container, example/data/Gutenberg is hello folder, and davinci.txt is hello blob.</span></span>
6. <span data-ttu-id="f9fc2-132">Pro hello **výstup** zadejte vlastnost, zadejte pro hello reduktorem hello výstupního souboru (včetně hello umístění).</span><span class="sxs-lookup"><span data-stu-id="f9fc2-132">For hello **output** type property, specify hello output file (including hello location) for hello reducer.</span></span> <span data-ttu-id="f9fc2-133">výstup Hello úlohy streamování Hadoop hello je zapsán toohello umístění zadané pro tuto vlastnost.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-133">hello output of hello Hadoop Streaming job is written toohello location specified for this property.</span></span>
7. <span data-ttu-id="f9fc2-134">V hello **filePaths** části, zadejte cesty hello hello mapper a reduktorem spustitelných souborů.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-134">In hello **filePaths** section, specify hello paths for hello mapper and reducer executables.</span></span> <span data-ttu-id="f9fc2-135">V příkladu hello: "adfsample/example/apps/wc.exe" adfsample je kontejner objektů blob hello, příklad nebo aplikací je složka hello a wc.exe je hello spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-135">In hello example: "adfsample/example/apps/wc.exe", adfsample is hello blob container, example/apps is hello folder, and wc.exe is hello executable.</span></span>
8. <span data-ttu-id="f9fc2-136">Pro hello **fileLinkedService** vlastnost, zadejte hello Azure Storage, propojené služby, která představuje hello úložiště Azure, který obsahuje soubory hello zadané v části filePaths hello.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-136">For hello **fileLinkedService** property, specify hello Azure Storage linked service that represents hello Azure storage that contains hello files specified in hello filePaths section.</span></span>
9. <span data-ttu-id="f9fc2-137">Pro hello **argumenty** vlastnost, zadejte hello argumenty pro hello streamování úlohy.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-137">For hello **arguments** property, specify hello arguments for hello streaming job.</span></span>
10. <span data-ttu-id="f9fc2-138">Hello **getdebuginfo –** vlastnost je volitelná elementu.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-138">hello **getDebugInfo** property is an optional element.</span></span> <span data-ttu-id="f9fc2-139">Pokud je nastavené tooFailure, protokoly hello se stáhnou pouze při selhání.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-139">When it is set tooFailure, hello logs are downloaded only on failure.</span></span> <span data-ttu-id="f9fc2-140">Pokud je nastavené tooAlways, protokoly budou staženy vždy bez ohledu na stav spuštění hello.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-140">When it is set tooAlways, logs are always downloaded irrespective of hello execution status.</span></span>

> [!NOTE]
> <span data-ttu-id="f9fc2-141">Jak je znázorněno v příkladu hello, zadáte výstupní datovou sadu pro hello streamované aktivitě Hadoop pro hello **výstupy** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-141">As shown in hello example, you specify an output dataset for hello Hadoop Streaming Activity for hello **outputs** property.</span></span> <span data-ttu-id="f9fc2-142">Tato datová sada je právě fiktivní datovou sadu, která je požadovaná toodrive hello kanálu plán.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-142">This dataset is just a dummy dataset that is required toodrive hello pipeline schedule.</span></span> <span data-ttu-id="f9fc2-143">Není nutné toospecify všechny vstupní datovou sadu aktivity hello pro hello **vstupy** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-143">You do not need toospecify any input dataset for hello activity for hello **inputs** property.</span></span>  
> 
> 

## <a name="example"></a><span data-ttu-id="f9fc2-144">Příklad</span><span class="sxs-lookup"><span data-stu-id="f9fc2-144">Example</span></span>
<span data-ttu-id="f9fc2-145">Hello kanál v tomto návodu spustí hello počet slov streamování mapy nebo snižte program v clusteru Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-145">hello pipeline in this walkthrough runs hello Word Count streaming Map/Reduce program on your Azure HDInsight cluster.</span></span> 

### <a name="linked-services"></a><span data-ttu-id="f9fc2-146">Propojené služby</span><span class="sxs-lookup"><span data-stu-id="f9fc2-146">Linked services</span></span>
#### <a name="azure-storage-linked-service"></a><span data-ttu-id="f9fc2-147">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="f9fc2-147">Azure Storage linked service</span></span>
<span data-ttu-id="f9fc2-148">Nejprve je třeba vytvořit hello toolink propojené služby Azure Storage, který je používán hello Azure HDInsight clusteru toohello Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-148">First, you create a linked service toolink hello Azure Storage that is used by hello Azure HDInsight cluster toohello Azure data factory.</span></span> <span data-ttu-id="f9fc2-149">Pokud jste zkopírujte a vložte následující kód hello, nevynechali tooreplace název účtu a účet klíč s názvem hello a klíč vašeho úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-149">If you copy/paste hello following code, do not forget tooreplace account name and account key with hello name and key of your Azure Storage.</span></span> 

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

#### <a name="azure-hdinsight-linked-service"></a><span data-ttu-id="f9fc2-150">Azure propojené služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="f9fc2-150">Azure HDInsight linked service</span></span>
<span data-ttu-id="f9fc2-151">V dalším kroku vytvoření propojené služby toolink vaše Azure HDInsight clusteru toohello Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-151">Next, you create a linked service toolink your Azure HDInsight cluster toohello Azure data factory.</span></span> <span data-ttu-id="f9fc2-152">Pokud jste zkopírujte a vložte následující kód hello, nahraďte název clusteru HDInsight hello názvem clusteru HDInsight a změňte hodnoty uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-152">If you copy/paste hello following code, replace HDInsight cluster name with hello name of your HDInsight cluster, and change user name and password values.</span></span> 

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

### <a name="datasets"></a><span data-ttu-id="f9fc2-153">Datové sady</span><span class="sxs-lookup"><span data-stu-id="f9fc2-153">Datasets</span></span>
#### <a name="output-dataset"></a><span data-ttu-id="f9fc2-154">Výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="f9fc2-154">Output dataset</span></span>
<span data-ttu-id="f9fc2-155">Hello kanálu v tomto příkladu nevyžaduje žádné vstupy.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-155">hello pipeline in this example does not take any inputs.</span></span> <span data-ttu-id="f9fc2-156">Zadáte výstupní datovou sadu pro hello HDInsight streamované aktivitě.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-156">You specify an output dataset for hello HDInsight Streaming Activity.</span></span> <span data-ttu-id="f9fc2-157">Tato datová sada je právě fiktivní datovou sadu, která je požadovaná toodrive hello kanálu plán.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-157">This dataset is just a dummy dataset that is required toodrive hello pipeline schedule.</span></span> 

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

### <a name="pipeline"></a><span data-ttu-id="f9fc2-158">Kanál</span><span class="sxs-lookup"><span data-stu-id="f9fc2-158">Pipeline</span></span>
<span data-ttu-id="f9fc2-159">Hello kanálu v tomto příkladu má jenom jedna aktivita, která je typu: **HDInsightStreaming**.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-159">hello pipeline in this example has only one activity that is of type: **HDInsightStreaming**.</span></span> 

<span data-ttu-id="f9fc2-160">Hello clusteru HDInsight se automaticky zadá příklad programy (wc.exe a cat.exe) a data (davinci.txt).</span><span class="sxs-lookup"><span data-stu-id="f9fc2-160">hello HDInsight cluster is automatically populated with example programs (wc.exe and cat.exe) and data (davinci.txt).</span></span> <span data-ttu-id="f9fc2-161">Ve výchozím nastavení je název hello kontejneru, který je používán clusteru HDInsight hello hello název samotného clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-161">By default, name of hello container that is used by hello HDInsight cluster is hello name of hello cluster itself.</span></span> <span data-ttu-id="f9fc2-162">Například pokud je název clusteru myhdicluster, bude název kontejneru objektu blob hello přidruženého myhdicluster.</span><span class="sxs-lookup"><span data-stu-id="f9fc2-162">For example, if your cluster name is myhdicluster, name of hello blob container associated would be myhdicluster.</span></span>  

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
## <a name="see-also"></a><span data-ttu-id="f9fc2-163">Viz také</span><span class="sxs-lookup"><span data-stu-id="f9fc2-163">See Also</span></span>
* [<span data-ttu-id="f9fc2-164">Aktivita Hive</span><span class="sxs-lookup"><span data-stu-id="f9fc2-164">Hive Activity</span></span>](data-factory-hive-activity.md)
* [<span data-ttu-id="f9fc2-165">Pig aktivity</span><span class="sxs-lookup"><span data-stu-id="f9fc2-165">Pig Activity</span></span>](data-factory-pig-activity.md)
* [<span data-ttu-id="f9fc2-166">Činnost MapReduce</span><span class="sxs-lookup"><span data-stu-id="f9fc2-166">MapReduce Activity</span></span>](data-factory-map-reduce.md)
* [<span data-ttu-id="f9fc2-167">Vyvolání programů Spark</span><span class="sxs-lookup"><span data-stu-id="f9fc2-167">Invoke Spark programs</span></span>](data-factory-spark.md)
* [<span data-ttu-id="f9fc2-168">Vyvolání skriptů jazyka R</span><span class="sxs-lookup"><span data-stu-id="f9fc2-168">Invoke R scripts</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

