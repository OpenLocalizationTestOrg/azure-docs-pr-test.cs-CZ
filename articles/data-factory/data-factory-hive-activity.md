---
title: "aaaTransform dat pomocí Hive aktivita – Azure | Microsoft Docs"
description: "Zjistěte, jak můžete pomocí hello Hive aktivity v dotazů Hive toorun Azure data factory na na vyžádání nebo vaše vlastní cluster HDInsight."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 80083218-743e-4da8-bdd2-60d1c77b1227
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: 032400cdb8e8f9873f85b811b4ad7380f4410edf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="transform-data-using-hive-activity-in-azure-data-factory"></a><span data-ttu-id="6b65c-103">Transformace dat pomocí Hive aktivity v Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="6b65c-103">Transform data using Hive Activity in Azure Data Factory</span></span> 
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="6b65c-104">Aktivita Hive</span><span class="sxs-lookup"><span data-stu-id="6b65c-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="6b65c-105">Pig aktivity</span><span class="sxs-lookup"><span data-stu-id="6b65c-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="6b65c-106">Činnost MapReduce</span><span class="sxs-lookup"><span data-stu-id="6b65c-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="6b65c-107">Streamované aktivitě Hadoop</span><span class="sxs-lookup"><span data-stu-id="6b65c-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="6b65c-108">Spark aktivity</span><span class="sxs-lookup"><span data-stu-id="6b65c-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="6b65c-109">Aktivita Provedení dávky služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="6b65c-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="6b65c-110">Aktivita Aktualizace prostředků služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="6b65c-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="6b65c-111">Aktivita Uložená procedura</span><span class="sxs-lookup"><span data-stu-id="6b65c-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="6b65c-112">Aktivita U-SQL služby Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="6b65c-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="6b65c-113">Vlastní aktivity rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="6b65c-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="6b65c-114">Hello aktivitu HDInsight Hive v datové továrně [kanálu](data-factory-create-pipelines.md) provádí dotazy Hive na [vlastní](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) nebo [na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) clusteru HDInsight se systémem Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="6b65c-114">hello HDInsight Hive activity in a Data Factory [pipeline](data-factory-create-pipelines.md) executes Hive queries on [your own](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or [on-demand](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-based HDInsight cluster.</span></span> <span data-ttu-id="6b65c-115">Tento článek vychází hello [aktivit transformace dat](data-factory-data-transformation-activities.md) článek, který poskytne Obecné přehled o transformaci dat a aktivit transformace hello podporována.</span><span class="sxs-lookup"><span data-stu-id="6b65c-115">This article builds on hello [data transformation activities](data-factory-data-transformation-activities.md) article, which presents a general overview of data transformation and hello supported transformation activities.</span></span>

> [!NOTE] 
> <span data-ttu-id="6b65c-116">Pokud jste tooAzure nový objekt pro vytváření dat, pročtěte [Úvod tooAzure Data Factory](data-factory-introduction.md) a hello kurz: [sestavit svůj první kanál dat](data-factory-build-your-first-pipeline.md) před přečtení tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="6b65c-116">If you are new tooAzure Data Factory, read through [Introduction tooAzure Data Factory](data-factory-introduction.md) and do hello tutorial: [Build your first data pipeline](data-factory-build-your-first-pipeline.md) before reading this article.</span></span> 

## <a name="syntax"></a><span data-ttu-id="6b65c-117">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="6b65c-117">Syntax</span></span>

```JSON
{
    "name": "Hive Activity",
    "description": "description",
    "type": "HDInsightHive",
    "inputs": [
      {
        "name": "input tables"
      }
    ],
    "outputs": [
      {
        "name": "output tables"
      }
    ],
    "linkedServiceName": "MyHDInsightLinkedService",
    "typeProperties": {
      "script": "Hive script",
      "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
      "defines": {
        "param1": "param1Value"
      }
    },
   "scheduler": {
      "frequency": "Day",
      "interval": 1
    }
}
```
## <a name="syntax-details"></a><span data-ttu-id="6b65c-118">Syntaxe podrobnosti</span><span class="sxs-lookup"><span data-stu-id="6b65c-118">Syntax details</span></span>
| <span data-ttu-id="6b65c-119">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="6b65c-119">Property</span></span> | <span data-ttu-id="6b65c-120">Popis</span><span class="sxs-lookup"><span data-stu-id="6b65c-120">Description</span></span> | <span data-ttu-id="6b65c-121">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="6b65c-121">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6b65c-122">jméno</span><span class="sxs-lookup"><span data-stu-id="6b65c-122">name</span></span> |<span data-ttu-id="6b65c-123">Název aktivity hello</span><span class="sxs-lookup"><span data-stu-id="6b65c-123">Name of hello activity</span></span> |<span data-ttu-id="6b65c-124">Ano</span><span class="sxs-lookup"><span data-stu-id="6b65c-124">Yes</span></span> |
| <span data-ttu-id="6b65c-125">description</span><span class="sxs-lookup"><span data-stu-id="6b65c-125">description</span></span> |<span data-ttu-id="6b65c-126">Popisuje, jaké aktivity hello se používá pro text</span><span class="sxs-lookup"><span data-stu-id="6b65c-126">Text describing what hello activity is used for</span></span> |<span data-ttu-id="6b65c-127">Ne</span><span class="sxs-lookup"><span data-stu-id="6b65c-127">No</span></span> |
| <span data-ttu-id="6b65c-128">type</span><span class="sxs-lookup"><span data-stu-id="6b65c-128">type</span></span> |<span data-ttu-id="6b65c-129">HDinsightHive</span><span class="sxs-lookup"><span data-stu-id="6b65c-129">HDinsightHive</span></span> |<span data-ttu-id="6b65c-130">Ano</span><span class="sxs-lookup"><span data-stu-id="6b65c-130">Yes</span></span> |
| <span data-ttu-id="6b65c-131">Vstupy</span><span class="sxs-lookup"><span data-stu-id="6b65c-131">inputs</span></span> |<span data-ttu-id="6b65c-132">Vstupy spotřebované hello Hive aktivity</span><span class="sxs-lookup"><span data-stu-id="6b65c-132">Inputs consumed by hello Hive activity</span></span> |<span data-ttu-id="6b65c-133">Ne</span><span class="sxs-lookup"><span data-stu-id="6b65c-133">No</span></span> |
| <span data-ttu-id="6b65c-134">Výstupy</span><span class="sxs-lookup"><span data-stu-id="6b65c-134">outputs</span></span> |<span data-ttu-id="6b65c-135">Výstupy vyprodukované hello Hive aktivity</span><span class="sxs-lookup"><span data-stu-id="6b65c-135">Outputs produced by hello Hive activity</span></span> |<span data-ttu-id="6b65c-136">Ano</span><span class="sxs-lookup"><span data-stu-id="6b65c-136">Yes</span></span> |
| <span data-ttu-id="6b65c-137">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="6b65c-137">linkedServiceName</span></span> |<span data-ttu-id="6b65c-138">Odkaz na clusteru HDInsight toohello registrován jako propojené služby v datové továrně</span><span class="sxs-lookup"><span data-stu-id="6b65c-138">Reference toohello HDInsight cluster registered as a linked service in Data Factory</span></span> |<span data-ttu-id="6b65c-139">Ano</span><span class="sxs-lookup"><span data-stu-id="6b65c-139">Yes</span></span> |
| <span data-ttu-id="6b65c-140">Skript</span><span class="sxs-lookup"><span data-stu-id="6b65c-140">script</span></span> |<span data-ttu-id="6b65c-141">Zadejte vloženého skriptu Hive hello</span><span class="sxs-lookup"><span data-stu-id="6b65c-141">Specify hello Hive script inline</span></span> |<span data-ttu-id="6b65c-142">Ne</span><span class="sxs-lookup"><span data-stu-id="6b65c-142">No</span></span> |
| <span data-ttu-id="6b65c-143">cestu ke skriptu</span><span class="sxs-lookup"><span data-stu-id="6b65c-143">script path</span></span> |<span data-ttu-id="6b65c-144">Úložiště hello Hive skript v Azure blob storage a zadejte toohello hello cestě k souboru.</span><span class="sxs-lookup"><span data-stu-id="6b65c-144">Store hello Hive script in an Azure blob storage and provide hello path toohello file.</span></span> <span data-ttu-id="6b65c-145">Pomocí vlastnosti 'skript' nebo 'scriptPath'.</span><span class="sxs-lookup"><span data-stu-id="6b65c-145">Use 'script' or 'scriptPath' property.</span></span> <span data-ttu-id="6b65c-146">Obě nelze použít společně.</span><span class="sxs-lookup"><span data-stu-id="6b65c-146">Both cannot be used together.</span></span> <span data-ttu-id="6b65c-147">Název souboru Hello je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="6b65c-147">hello file name is case-sensitive.</span></span> |<span data-ttu-id="6b65c-148">Ne</span><span class="sxs-lookup"><span data-stu-id="6b65c-148">No</span></span> |
| <span data-ttu-id="6b65c-149">definuje</span><span class="sxs-lookup"><span data-stu-id="6b65c-149">defines</span></span> |<span data-ttu-id="6b65c-150">Zadejte parametry dvojic klíč/hodnota pro odkazování v rámci skriptu Hive hello pomocí 'hiveconf.</span><span class="sxs-lookup"><span data-stu-id="6b65c-150">Specify parameters as key/value pairs for referencing within hello Hive script using 'hiveconf'</span></span> |<span data-ttu-id="6b65c-151">Ne</span><span class="sxs-lookup"><span data-stu-id="6b65c-151">No</span></span> |

## <a name="example"></a><span data-ttu-id="6b65c-152">Příklad</span><span class="sxs-lookup"><span data-stu-id="6b65c-152">Example</span></span>
<span data-ttu-id="6b65c-153">Zvažte například ve hře protokoly analýzy, kam chcete tooidentify hello čas strávený uživatelé hraní her spuštění ve vaší společnosti.</span><span class="sxs-lookup"><span data-stu-id="6b65c-153">Let’s consider an example of game logs analytics where you want tooidentify hello time spent by users playing games launched by your company.</span></span> 

<span data-ttu-id="6b65c-154">Hello následující protokol je ukázkový herní protokol, který je čárkou (`,`) oddělený a obsahuje následující pole – ProfileID, SessionStart, doba trvání, SrcIPAddress a GameType hello.</span><span class="sxs-lookup"><span data-stu-id="6b65c-154">hello following log is a sample game log, which is comma (`,`) separated and contains hello following fields – ProfileID, SessionStart, Duration, SrcIPAddress, and GameType.</span></span>

```
1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
.....
```

<span data-ttu-id="6b65c-155">Hello **skript podregistru** tooprocess tato data:</span><span class="sxs-lookup"><span data-stu-id="6b65c-155">hello **Hive script** tooprocess this data:</span></span>

```
DROP TABLE IF EXISTS HiveSampleIn; 
CREATE EXTERNAL TABLE HiveSampleIn 
(
    ProfileID        string, 
    SessionStart     string, 
    Duration         int, 
    SrcIPAddress     string, 
    GameType         string
) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/samplein/'; 

DROP TABLE IF EXISTS HiveSampleOut; 
CREATE EXTERNAL TABLE HiveSampleOut 
(    
    ProfileID     string, 
    Duration     int
) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/sampleout/';

INSERT OVERWRITE TABLE HiveSampleOut
Select 
    ProfileID,
    SUM(Duration)
FROM HiveSampleIn Group by ProfileID
```

<span data-ttu-id="6b65c-156">tooexecute tomuto podregistru skript v kanálu pro vytváření dat, budete potřebovat následující toodo hello</span><span class="sxs-lookup"><span data-stu-id="6b65c-156">tooexecute this Hive script in a Data Factory pipeline, you need toodo hello following</span></span>

1. <span data-ttu-id="6b65c-157">Vytvoření propojené služby tooregister [vlastní HDInsight výpočetní cluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) nebo nakonfigurovat [výpočetní cluster HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="6b65c-157">Create a linked service tooregister [your own HDInsight compute cluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or configure [on-demand HDInsight compute cluster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span></span> <span data-ttu-id="6b65c-158">Umožňuje volat tuto propojenou službu "HDInsightLinkedService".</span><span class="sxs-lookup"><span data-stu-id="6b65c-158">Let’s call this linked service “HDInsightLinkedService”.</span></span>
2. <span data-ttu-id="6b65c-159">Vytvoření [propojená služba](data-factory-azure-blob-connector.md) tooconfigure hello připojení tooAzure úložiště objektů Blob hostování hello data.</span><span class="sxs-lookup"><span data-stu-id="6b65c-159">Create a [linked service](data-factory-azure-blob-connector.md) tooconfigure hello connection tooAzure Blob storage hosting hello data.</span></span> <span data-ttu-id="6b65c-160">Umožňuje volání této propojené služby "StorageLinkedService"</span><span class="sxs-lookup"><span data-stu-id="6b65c-160">Let’s call this linked service “StorageLinkedService”</span></span>
3. <span data-ttu-id="6b65c-161">Vytvoření [datové sady](data-factory-create-datasets.md) ukazují toohello vstup a hello výstupní data.</span><span class="sxs-lookup"><span data-stu-id="6b65c-161">Create [datasets](data-factory-create-datasets.md) pointing toohello input and hello output data.</span></span> <span data-ttu-id="6b65c-162">Umožňuje volání hello vstupní datovou sadu "HiveSampleIn" a hello výstupní datovou sadu "HiveSampleOut"</span><span class="sxs-lookup"><span data-stu-id="6b65c-162">Let’s call hello input dataset “HiveSampleIn” and hello output dataset “HiveSampleOut”</span></span>
4. <span data-ttu-id="6b65c-163">Kopírovat dotaz Hive hello jako soubor tooAzure nakonfigurovali v kroku #2 úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="6b65c-163">Copy hello Hive query as a file tooAzure Blob Storage configured in step #2.</span></span> <span data-ttu-id="6b65c-164">Pokud hello úložiště pro hostování hello dat se liší od hello jeden hostování tento soubor dotazu, vytvořit samostatné propojenou službu úložiště Azure a tooit při hello aktivity.</span><span class="sxs-lookup"><span data-stu-id="6b65c-164">if hello storage for hosting hello data is different from hello one hosting this query file, create a separate Azure Storage linked service and refer tooit in hello activity.</span></span> <span data-ttu-id="6b65c-165">Použijte ** scriptPath ** hello toospecify cestě k souboru dotazu toohive a **scriptLinkedService** toospecify hello úložiště Azure, která obsahuje soubor skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="6b65c-165">Use **scriptPath **toospecify hello path toohive query file and **scriptLinkedService** toospecify hello Azure storage that contains hello script file.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="6b65c-166">Vložený skript Hive hello v definici aktivity hello můžete zadat taky pomocí hello **skriptu** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="6b65c-166">You can also provide hello Hive script inline in hello activity definition by using hello **script** property.</span></span> <span data-ttu-id="6b65c-167">Nedoporučujeme tento přístup všechny speciální znaky v hello skriptu v dokumentu JSON hello musí toobe uvozené a může příčina ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="6b65c-167">We do not recommend this approach as all special characters in hello script within hello JSON document needs toobe escaped and may cause debugging issues.</span></span> <span data-ttu-id="6b65c-168">Hello osvědčeným postupem je toofollow krok #4.</span><span class="sxs-lookup"><span data-stu-id="6b65c-168">hello best practice is toofollow step #4.</span></span>
   > 
   > 
5. <span data-ttu-id="6b65c-169">Vytvoření kanálu s hello aktivita HDInsightHive.</span><span class="sxs-lookup"><span data-stu-id="6b65c-169">Create a pipeline with hello HDInsightHive activity.</span></span> <span data-ttu-id="6b65c-170">Aktivita Hello procesy nebo transformace dat hello.</span><span class="sxs-lookup"><span data-stu-id="6b65c-170">hello activity processes/transforms hello data.</span></span>

    ```JSON   
    {   
        "name": "HiveActivitySamplePipeline",
        "properties": {
        "activities": [
            {
                "name": "HiveActivitySample",
                "type": "HDInsightHive",
                "inputs": [
                {
                    "name": "HiveSampleIn"
                }
                ],
                "outputs": [
                {
                    "name": "HiveSampleOut"
                }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                    "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                    "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
            ]
        }
    }
    ```
6. <span data-ttu-id="6b65c-171">Nasaďte hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="6b65c-171">Deploy hello pipeline.</span></span> <span data-ttu-id="6b65c-172">V tématu [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="6b65c-172">See [Creating pipelines](data-factory-create-pipelines.md) article for details.</span></span> 
7. <span data-ttu-id="6b65c-173">Monitorování kanálu hello pomocí hello data objektu pro vytváření monitorování a zobrazení správy.</span><span class="sxs-lookup"><span data-stu-id="6b65c-173">Monitor hello pipeline using hello data factory monitoring and management views.</span></span> <span data-ttu-id="6b65c-174">V tématu [monitorování a Správa kanálů služby Data Factory](data-factory-monitor-manage-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="6b65c-174">See [Monitoring and manage Data Factory pipelines](data-factory-monitor-manage-pipelines.md) article for details.</span></span> 

## <a name="specifying-parameters-for-a-hive-script"></a><span data-ttu-id="6b65c-175">Zadání parametrů pro skript Hive</span><span class="sxs-lookup"><span data-stu-id="6b65c-175">Specifying parameters for a Hive script</span></span>
<span data-ttu-id="6b65c-176">V tomto příkladu herní protokoly jsou denně požity do Azure Blob Storage a jsou uložené ve složce rozdělený do oddílů pomocí data a času.</span><span class="sxs-lookup"><span data-stu-id="6b65c-176">In this example, game logs are ingested daily into Azure Blob Storage and are stored in a folder partitioned with date and time.</span></span> <span data-ttu-id="6b65c-177">Má skript Hive hello tooparameterize a předat hello vstupní složky dynamicky za běhu a také vytvořit několik výstup hello rozdělený do oddílů pomocí data a času.</span><span class="sxs-lookup"><span data-stu-id="6b65c-177">You want tooparameterize hello Hive script and pass hello input folder location dynamically during runtime and also produce hello output partitioned with date and time.</span></span>

<span data-ttu-id="6b65c-178">toouse parametry skriptu Hive, proveďte následující hello</span><span class="sxs-lookup"><span data-stu-id="6b65c-178">toouse parameterized Hive script, do hello following</span></span>

* <span data-ttu-id="6b65c-179">Definujte parametry hello v **definuje**.</span><span class="sxs-lookup"><span data-stu-id="6b65c-179">Define hello parameters in **defines**.</span></span>

    ```JSON  
    {
        "name": "HiveActivitySamplePipeline",
          "properties": {
        "activities": [
             {
                "name": "HiveActivitySample",
                "type": "HDInsightHive",
                "inputs": [
                      {
                        "name": "HiveSampleIn"
                      }
                ],
                "outputs": [
                      {
                        "name": "HiveSampleOut"
                    }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                      "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                      "scriptLinkedService": "StorageLinkedService",
                      "defines": {
                        "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)",
                        "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)"
                      },
                       "scheduler": {
                          "frequency": "Hour",
                          "interval": 1
                    }
                }
              }
        ]
      }
    }
    ```
* <span data-ttu-id="6b65c-180">V hello skriptu Hive, označují pomocí parametru toohello **${hiveconf:parameterName}**.</span><span class="sxs-lookup"><span data-stu-id="6b65c-180">In hello Hive Script, refer toohello parameter using **${hiveconf:parameterName}**.</span></span> 
  
    ```
    DROP TABLE IF EXISTS HiveSampleIn; 
    CREATE EXTERNAL TABLE HiveSampleIn 
    (
        ProfileID     string, 
        SessionStart     string, 
        Duration     int, 
        SrcIPAddress     string, 
        GameType     string
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Input}'; 

    DROP TABLE IF EXISTS HiveSampleOut; 
    CREATE EXTERNAL TABLE HiveSampleOut 
    (
        ProfileID     string, 
        Duration     int
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Output}';

    INSERT OVERWRITE TABLE HiveSampleOut
    Select 
        ProfileID,
        SUM(Duration)
    FROM HiveSampleIn Group by ProfileID
    ```
## <a name="see-also"></a><span data-ttu-id="6b65c-181">Viz také</span><span class="sxs-lookup"><span data-stu-id="6b65c-181">See Also</span></span>
* [<span data-ttu-id="6b65c-182">Pig aktivity</span><span class="sxs-lookup"><span data-stu-id="6b65c-182">Pig Activity</span></span>](data-factory-pig-activity.md)
* [<span data-ttu-id="6b65c-183">Činnost MapReduce</span><span class="sxs-lookup"><span data-stu-id="6b65c-183">MapReduce Activity</span></span>](data-factory-map-reduce.md)
* [<span data-ttu-id="6b65c-184">Streamované aktivitě Hadoop</span><span class="sxs-lookup"><span data-stu-id="6b65c-184">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
* [<span data-ttu-id="6b65c-185">Vyvolání programů Spark</span><span class="sxs-lookup"><span data-stu-id="6b65c-185">Invoke Spark programs</span></span>](data-factory-spark.md)
* [<span data-ttu-id="6b65c-186">Vyvolání skriptů jazyka R</span><span class="sxs-lookup"><span data-stu-id="6b65c-186">Invoke R scripts</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

