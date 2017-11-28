---
title: aaaInvoke MapReduce Program z Azure Data Factory
description: "Zjistěte, jak se data tooprocess spuštěním MapReduce programy v Azure HDInsight clusteru ze služby Azure data factory."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: c34db93f-570a-44f1-a7d6-00390f4dc0fa
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: shlo
ms.openlocfilehash: 448ef93a10bd97e7ecd4be4f04f88f8a05decc1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="invoke-mapreduce-programs-from-data-factory"></a><span data-ttu-id="b11aa-103">Vyvolání MapReduce programy z objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="b11aa-103">Invoke MapReduce Programs from Data Factory</span></span>
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="b11aa-104">Aktivita Hive</span><span class="sxs-lookup"><span data-stu-id="b11aa-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="b11aa-105">Pig aktivity</span><span class="sxs-lookup"><span data-stu-id="b11aa-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="b11aa-106">Činnost MapReduce</span><span class="sxs-lookup"><span data-stu-id="b11aa-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="b11aa-107">Streamované aktivitě Hadoop</span><span class="sxs-lookup"><span data-stu-id="b11aa-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="b11aa-108">Spark aktivity</span><span class="sxs-lookup"><span data-stu-id="b11aa-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="b11aa-109">Aktivita Provedení dávky služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="b11aa-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="b11aa-110">Aktivita Aktualizace prostředků služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="b11aa-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="b11aa-111">Aktivita Uložená procedura</span><span class="sxs-lookup"><span data-stu-id="b11aa-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="b11aa-112">Aktivita U-SQL služby Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="b11aa-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="b11aa-113">Vlastní aktivity rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="b11aa-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="b11aa-114">Hello činnost HDInsight MapReduce v datové továrně [kanálu](data-factory-create-pipelines.md) provede MapReduce programy na [vlastní](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) nebo [na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) clusteru HDInsight se systémem Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="b11aa-114">hello HDInsight MapReduce activity in a Data Factory [pipeline](data-factory-create-pipelines.md) executes MapReduce programs on [your own](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or [on-demand](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-based HDInsight cluster.</span></span> <span data-ttu-id="b11aa-115">Tento článek vychází hello [aktivit transformace dat](data-factory-data-transformation-activities.md) článek, který poskytne Obecné přehled o transformaci dat a aktivit transformace hello podporována.</span><span class="sxs-lookup"><span data-stu-id="b11aa-115">This article builds on hello [data transformation activities](data-factory-data-transformation-activities.md) article, which presents a general overview of data transformation and hello supported transformation activities.</span></span>

> [!NOTE] 
> <span data-ttu-id="b11aa-116">Pokud jste tooAzure nový objekt pro vytváření dat, pročtěte [Úvod tooAzure Data Factory](data-factory-introduction.md) a hello kurz: [sestavit svůj první kanál dat](data-factory-build-your-first-pipeline.md) před přečtení tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="b11aa-116">If you are new tooAzure Data Factory, read through [Introduction tooAzure Data Factory](data-factory-introduction.md) and do hello tutorial: [Build your first data pipeline](data-factory-build-your-first-pipeline.md) before reading this article.</span></span>  

## <a name="introduction"></a><span data-ttu-id="b11aa-117">Úvod</span><span class="sxs-lookup"><span data-stu-id="b11aa-117">Introduction</span></span>
<span data-ttu-id="b11aa-118">Kanál v objektu pro vytváření dat Azure zpracovává data v služby propojené úložiště pomocí propojené výpočetní služby.</span><span class="sxs-lookup"><span data-stu-id="b11aa-118">A pipeline in an Azure data factory processes data in linked storage services by using linked compute services.</span></span> <span data-ttu-id="b11aa-119">Obsahuje posloupnost aktivit, kde každá aktivita provede konkrétní zpracování operace.</span><span class="sxs-lookup"><span data-stu-id="b11aa-119">It contains a sequence of activities where each activity performs a specific processing operation.</span></span> <span data-ttu-id="b11aa-120">Tento článek popisuje použití hello činnost MapReduce HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b11aa-120">This article describes using hello HDInsight MapReduce Activity.</span></span>

<span data-ttu-id="b11aa-121">V tématu [Pig](data-factory-pig-activity.md) a [Hive](data-factory-hive-activity.md) podrobnosti o spouštění Pig nebo Hive skriptů na HDInsight se systémem Windows nebo Linux clusteru z kanálu pomocí aktivity HDInsight Pig a Hive.</span><span class="sxs-lookup"><span data-stu-id="b11aa-121">See [Pig](data-factory-pig-activity.md) and [Hive](data-factory-hive-activity.md) for details about running Pig/Hive scripts on a Windows/Linux-based HDInsight cluster from a pipeline by using HDInsight Pig and Hive activities.</span></span> 

## <a name="json-for-hdinsight-mapreduce-activity"></a><span data-ttu-id="b11aa-122">JSON pro činnost MapReduce s HDInsight</span><span class="sxs-lookup"><span data-stu-id="b11aa-122">JSON for HDInsight MapReduce Activity</span></span>
<span data-ttu-id="b11aa-123">V definici JSON pro hello aktivita HDInsight hello:</span><span class="sxs-lookup"><span data-stu-id="b11aa-123">In hello JSON definition for hello HDInsight Activity:</span></span> 

1. <span data-ttu-id="b11aa-124">Sada hello **typ** z hello **aktivity** příliš**HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="b11aa-124">Set hello **type** of hello **activity** too**HDInsight**.</span></span>
2. <span data-ttu-id="b11aa-125">Zadejte název hello hello třídu pro **className** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="b11aa-125">Specify hello name of hello class for **className** property.</span></span>
3. <span data-ttu-id="b11aa-126">Zadejte hello cesta toohello soubor JAR včetně hello název souboru pro **jarFilePath** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="b11aa-126">Specify hello path toohello JAR file including hello file name for **jarFilePath** property.</span></span>
4. <span data-ttu-id="b11aa-127">Zadejte hello propojené služby, která odkazuje toohello Azure Blob Storage, který obsahuje soubor JAR hello pro **jarLinkedService** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="b11aa-127">Specify hello linked service that refers toohello Azure Blob Storage that contains hello JAR file for **jarLinkedService** property.</span></span>   
5. <span data-ttu-id="b11aa-128">Zadejte všechny argumenty pro hello MapReduce program v hello **argumenty** části.</span><span class="sxs-lookup"><span data-stu-id="b11aa-128">Specify any arguments for hello MapReduce program in hello **arguments** section.</span></span> <span data-ttu-id="b11aa-129">V době běhu zobrazí několik další argumenty (například: mapreduce.job.tags) z rozhraní MapReduce hello.</span><span class="sxs-lookup"><span data-stu-id="b11aa-129">At runtime, you see a few extra arguments (for example: mapreduce.job.tags) from hello MapReduce framework.</span></span> <span data-ttu-id="b11aa-130">pomocí možnosti a hodnoty jako argumenty, jak ukazuje následující příklad hello toodifferentiate vaše argumenty s argumenty hello MapReduce, zvažte (- s, – vstup, – výstupní atd., jsou možnosti bezprostředně následované jejich hodnoty).</span><span class="sxs-lookup"><span data-stu-id="b11aa-130">toodifferentiate your arguments with hello MapReduce arguments, consider using both option and value as arguments as shown in hello following example (-s, --input, --output etc., are options immediately followed by their values).</span></span>

    ```JSON   
    {
        "name": "MahoutMapReduceSamplePipeline",
        "properties": {
            "description": "Sample Pipeline tooRun a Mahout Custom Map Reduce Jar. This job calcuates an Item Similarity Matrix toodetermine hello similarity between 2 items",
            "activities": [
                {
                    "type": "HDInsightMapReduce",
                    "typeProperties": {
                        "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                        "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                        "jarLinkedService": "StorageLinkedService",
                        "arguments": [
                            "-s",
                            "SIMILARITY_LOGLIKELIHOOD",
                            "--input",
                            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input",
                            "--output",
                            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/",
                            "--maxSimilaritiesPerItem",
                            "500",
                            "--tempDir",
                            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"
                        ]
                    },
                    "inputs": [
                        {
                            "name": "MahoutInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "MahoutOutput"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "MahoutActivity",
                    "description": "Custom Map Reduce toogenerate Mahout result",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2017-01-03T00:00:00Z",
            "end": "2017-01-04T00:00:00Z"
        }
    }
    ```
<span data-ttu-id="b11aa-131">Hello činnost MapReduce HDInsight toorun můžete použít libovolný soubor jar MapReduce v clusteru služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b11aa-131">You can use hello HDInsight MapReduce Activity toorun any MapReduce jar file on an HDInsight cluster.</span></span> <span data-ttu-id="b11aa-132">Následující ukázka definici JSON kanálu, hello je aktivita HDInsight v hello nakonfigurované toorun soubor Mahout JAR.</span><span class="sxs-lookup"><span data-stu-id="b11aa-132">In hello following sample JSON definition of a pipeline, hello HDInsight Activity is configured toorun a Mahout JAR file.</span></span>

## <a name="sample-on-github"></a><span data-ttu-id="b11aa-133">Ukázce na Githubu</span><span class="sxs-lookup"><span data-stu-id="b11aa-133">Sample on GitHub</span></span>
<span data-ttu-id="b11aa-134">Můžete si stáhnout ukázku pro používání hello činnost MapReduce HDInsight z: [ukázky Data Factory na webu GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON/MapReduce_Activity_Sample).</span><span class="sxs-lookup"><span data-stu-id="b11aa-134">You can download a sample for using hello HDInsight MapReduce Activity from: [Data Factory Samples on GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON/MapReduce_Activity_Sample).</span></span>  

## <a name="running-hello-word-count-program"></a><span data-ttu-id="b11aa-135">Spuštění programu hello počet slov</span><span class="sxs-lookup"><span data-stu-id="b11aa-135">Running hello Word Count program</span></span>
<span data-ttu-id="b11aa-136">Hello kanál v tomto příkladu se spustí hello program Mapa nebo snižte počet slov v clusteru Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b11aa-136">hello pipeline in this example runs hello Word Count Map/Reduce program on your Azure HDInsight cluster.</span></span>   

### <a name="linked-services"></a><span data-ttu-id="b11aa-137">Propojené služby</span><span class="sxs-lookup"><span data-stu-id="b11aa-137">Linked Services</span></span>
<span data-ttu-id="b11aa-138">Nejprve je třeba vytvořit hello toolink propojené služby Azure Storage, který je používán hello Azure HDInsight clusteru toohello Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="b11aa-138">First, you create a linked service toolink hello Azure Storage that is used by hello Azure HDInsight cluster toohello Azure data factory.</span></span> <span data-ttu-id="b11aa-139">Pokud jste zkopírujte a vložte následující kód hello, nevynechali tooreplace **název účtu** a **klíč účtu** s hello název a klíč vašeho úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="b11aa-139">If you copy/paste hello following code, do not forget tooreplace **account name** and **account key** with hello name and key of your Azure Storage.</span></span> 

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="b11aa-140">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="b11aa-140">Azure Storage linked service</span></span>

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

#### <a name="azure-hdinsight-linked-service"></a><span data-ttu-id="b11aa-141">Azure propojené služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="b11aa-141">Azure HDInsight linked service</span></span>
<span data-ttu-id="b11aa-142">V dalším kroku vytvoření propojené služby toolink vaše Azure HDInsight clusteru toohello Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="b11aa-142">Next, you create a linked service toolink your Azure HDInsight cluster toohello Azure data factory.</span></span> <span data-ttu-id="b11aa-143">Pokud jste zkopírujte a vložte následující kód hello, nahraďte **název clusteru HDInsight** s názvem hello HDInsight cluster a změnit uživatelské jméno a heslo hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b11aa-143">If you copy/paste hello following code, replace **HDInsight cluster name** with hello name of your HDInsight cluster, and change user name and password values.</span></span>   

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

### <a name="datasets"></a><span data-ttu-id="b11aa-144">Datové sady</span><span class="sxs-lookup"><span data-stu-id="b11aa-144">Datasets</span></span>
#### <a name="output-dataset"></a><span data-ttu-id="b11aa-145">Výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="b11aa-145">Output dataset</span></span>
<span data-ttu-id="b11aa-146">Hello kanálu v tomto příkladu nevyžaduje žádné vstupy.</span><span class="sxs-lookup"><span data-stu-id="b11aa-146">hello pipeline in this example does not take any inputs.</span></span> <span data-ttu-id="b11aa-147">Zadáte výstupní datovou sadu pro činnost MapReduce HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="b11aa-147">You specify an output dataset for hello HDInsight MapReduce Activity.</span></span> <span data-ttu-id="b11aa-148">Tato datová sada je právě fiktivní datovou sadu, která je požadovaná toodrive hello kanálu plán.</span><span class="sxs-lookup"><span data-stu-id="b11aa-148">This dataset is just a dummy dataset that is required toodrive hello pipeline schedule.</span></span>  

```JSON
{
    "name": "MROutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "fileName": "WordCountOutput1.txt",
            "folderPath": "example/data/",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

### <a name="pipeline"></a><span data-ttu-id="b11aa-149">Kanál</span><span class="sxs-lookup"><span data-stu-id="b11aa-149">Pipeline</span></span>
<span data-ttu-id="b11aa-150">Hello kanálu v tomto příkladu má jenom jedna aktivita, která je typu: HDInsightMapReduce.</span><span class="sxs-lookup"><span data-stu-id="b11aa-150">hello pipeline in this example has only one activity that is of type: HDInsightMapReduce.</span></span> <span data-ttu-id="b11aa-151">Některé důležité vlastnosti hello v hello JSON jsou:</span><span class="sxs-lookup"><span data-stu-id="b11aa-151">Some of hello important properties in hello JSON are:</span></span> 

| <span data-ttu-id="b11aa-152">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="b11aa-152">Property</span></span> | <span data-ttu-id="b11aa-153">Poznámky</span><span class="sxs-lookup"><span data-stu-id="b11aa-153">Notes</span></span> |
|:--- |:--- |
| <span data-ttu-id="b11aa-154">type</span><span class="sxs-lookup"><span data-stu-id="b11aa-154">type</span></span> |<span data-ttu-id="b11aa-155">Hello typ musí být nastavený příliš**HDInsightMapReduce**.</span><span class="sxs-lookup"><span data-stu-id="b11aa-155">hello type must be set too**HDInsightMapReduce**.</span></span> |
| <span data-ttu-id="b11aa-156">Název třídy</span><span class="sxs-lookup"><span data-stu-id="b11aa-156">className</span></span> |<span data-ttu-id="b11aa-157">Název třídy hello je: **wordcount**</span><span class="sxs-lookup"><span data-stu-id="b11aa-157">Name of hello class is: **wordcount**</span></span> |
| <span data-ttu-id="b11aa-158">jarFilePath</span><span class="sxs-lookup"><span data-stu-id="b11aa-158">jarFilePath</span></span> |<span data-ttu-id="b11aa-159">Cesta toohello soubor jar obsahující hello třídy.</span><span class="sxs-lookup"><span data-stu-id="b11aa-159">Path toohello jar file containing hello class.</span></span> <span data-ttu-id="b11aa-160">Pokud jste zkopírujte a vložte následující kód hello, nezapomeňte toochange hello název clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="b11aa-160">If you copy/paste hello following code, don't forget toochange hello name of hello cluster.</span></span> |
| <span data-ttu-id="b11aa-161">jarLinkedService</span><span class="sxs-lookup"><span data-stu-id="b11aa-161">jarLinkedService</span></span> |<span data-ttu-id="b11aa-162">Propojená služba, která obsahuje soubor jar hello Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="b11aa-162">Azure Storage linked service that contains hello jar file.</span></span> <span data-ttu-id="b11aa-163">Tato propojená služba odkazuje toohello úložiště, který je přidružen hello HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="b11aa-163">This linked service refers toohello storage that is associated with hello HDInsight cluster.</span></span> |
| <span data-ttu-id="b11aa-164">Argumenty</span><span class="sxs-lookup"><span data-stu-id="b11aa-164">arguments</span></span> |<span data-ttu-id="b11aa-165">Hello wordcount program přebírá dva argumenty, vstup a výstup.</span><span class="sxs-lookup"><span data-stu-id="b11aa-165">hello wordcount program takes two arguments, an input and an output.</span></span> <span data-ttu-id="b11aa-166">vstupní soubor Hello je soubor davinci.txt hello.</span><span class="sxs-lookup"><span data-stu-id="b11aa-166">hello input file is hello davinci.txt file.</span></span> |
| <span data-ttu-id="b11aa-167">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="b11aa-167">frequency/interval</span></span> |<span data-ttu-id="b11aa-168">Hello hodnoty těchto vlastností odpovídat hello výstupní datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="b11aa-168">hello values for these properties match hello output dataset.</span></span> |
| <span data-ttu-id="b11aa-169">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="b11aa-169">linkedServiceName</span></span> |<span data-ttu-id="b11aa-170">odkazuje toohello propojené služby HDInsight, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="b11aa-170">refers toohello HDInsight linked service you had created earlier.</span></span> |

```JSON
{
    "name": "MRSamplePipeline",
    "properties": {
        "description": "Sample Pipeline tooRun hello Word Count Program",
        "activities": [
            {
                "type": "HDInsightMapReduce",
                "typeProperties": {
                    "className": "wordcount",
                    "jarFilePath": "<HDInsight cluster name>/example/jars/hadoop-examples.jar",
                    "jarLinkedService": "StorageLinkedService",
                    "arguments": [
                        "/example/data/gutenberg/davinci.txt",
                        "/example/data/WordCountOutput1"
                    ]
                },
                "outputs": [
                    {
                        "name": "MROutput"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "MRActivity",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2014-01-03T00:00:00Z",
        "end": "2014-01-04T00:00:00Z"
    }
}
```

## <a name="run-spark-programs"></a><span data-ttu-id="b11aa-171">Spouštět programy Spark</span><span class="sxs-lookup"><span data-stu-id="b11aa-171">Run Spark programs</span></span>
<span data-ttu-id="b11aa-172">MapReduce aktivity toorun Spark programy můžete použít v clusteru HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="b11aa-172">You can use MapReduce activity toorun Spark programs on your HDInsight Spark cluster.</span></span> <span data-ttu-id="b11aa-173">Podrobnosti najdete v článku [Vyvolání programů Spark ze služby Azure Data Factory](data-factory-spark.md).</span><span class="sxs-lookup"><span data-stu-id="b11aa-173">See [Invoke Spark programs from Azure Data Factory](data-factory-spark.md) for details.</span></span>  

[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[adfgetstartedmonitoring]:data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#monitor-pipelines 

[Developer Reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[Azure Portal]: http://portal.azure.com

## <a name="see-also"></a><span data-ttu-id="b11aa-174">Viz také</span><span class="sxs-lookup"><span data-stu-id="b11aa-174">See Also</span></span>
* [<span data-ttu-id="b11aa-175">Aktivita Hive</span><span class="sxs-lookup"><span data-stu-id="b11aa-175">Hive Activity</span></span>](data-factory-hive-activity.md)
* [<span data-ttu-id="b11aa-176">Pig aktivity</span><span class="sxs-lookup"><span data-stu-id="b11aa-176">Pig Activity</span></span>](data-factory-pig-activity.md)
* [<span data-ttu-id="b11aa-177">Streamované aktivitě Hadoop</span><span class="sxs-lookup"><span data-stu-id="b11aa-177">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
* [<span data-ttu-id="b11aa-178">Vyvolání programů Spark</span><span class="sxs-lookup"><span data-stu-id="b11aa-178">Invoke Spark programs</span></span>](data-factory-spark.md)
* [<span data-ttu-id="b11aa-179">Vyvolání skriptů jazyka R</span><span class="sxs-lookup"><span data-stu-id="b11aa-179">Invoke R scripts</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

