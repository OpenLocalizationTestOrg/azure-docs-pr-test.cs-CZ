---
title: "Transformace dat pomocí Hive aktivita – Azure | Microsoft Docs"
description: "Zjistěte, jak pomocí aktivity Hive v objektu pro vytváření dat Azure ke spouštění dotazů Hive v clusteru HDInsight na vyžádání nebo vaše vlastní."
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
ms.openlocfilehash: a3e9b2d0a8c851939acd228d8086ddfc9f38a4c1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="transform-data-using-hive-activity-in-azure-data-factory"></a><span data-ttu-id="b324e-103">Transformace dat pomocí Hive aktivity v Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="b324e-103">Transform data using Hive Activity in Azure Data Factory</span></span> 
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="b324e-104">Aktivita Hive</span><span class="sxs-lookup"><span data-stu-id="b324e-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="b324e-105">Pig aktivity</span><span class="sxs-lookup"><span data-stu-id="b324e-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="b324e-106">Činnost MapReduce</span><span class="sxs-lookup"><span data-stu-id="b324e-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="b324e-107">Streamované aktivitě Hadoop</span><span class="sxs-lookup"><span data-stu-id="b324e-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="b324e-108">Spark aktivity</span><span class="sxs-lookup"><span data-stu-id="b324e-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="b324e-109">Aktivita Provedení dávky služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="b324e-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="b324e-110">Aktivita Aktualizace prostředků služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="b324e-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="b324e-111">Aktivita Uložená procedura</span><span class="sxs-lookup"><span data-stu-id="b324e-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="b324e-112">Aktivita U-SQL služby Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="b324e-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="b324e-113">Vlastní aktivity rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="b324e-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="b324e-114">Aktivity HDInsight Hive v datové továrně [kanálu](data-factory-create-pipelines.md) provádí dotazy Hive na [vlastní](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) nebo [na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) clusteru HDInsight se systémem Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="b324e-114">The HDInsight Hive activity in a Data Factory [pipeline](data-factory-create-pipelines.md) executes Hive queries on [your own](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or [on-demand](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-based HDInsight cluster.</span></span> <span data-ttu-id="b324e-115">Tento článek vychází [aktivit transformace dat](data-factory-data-transformation-activities.md) článek, který poskytne Obecné přehled o transformaci dat a aktivity podporované transformace.</span><span class="sxs-lookup"><span data-stu-id="b324e-115">This article builds on the [data transformation activities](data-factory-data-transformation-activities.md) article, which presents a general overview of data transformation and the supported transformation activities.</span></span>

> [!NOTE] 
> <span data-ttu-id="b324e-116">Pokud jste do Azure Data Factory nové, přečtěte si [Úvod do Azure Data Factory](data-factory-introduction.md) a proveďte kurz: [sestavit svůj první kanál dat](data-factory-build-your-first-pipeline.md) před přečtení tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="b324e-116">If you are new to Azure Data Factory, read through [Introduction to Azure Data Factory](data-factory-introduction.md) and do the tutorial: [Build your first data pipeline](data-factory-build-your-first-pipeline.md) before reading this article.</span></span> 

## <a name="syntax"></a><span data-ttu-id="b324e-117">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="b324e-117">Syntax</span></span>

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
## <a name="syntax-details"></a><span data-ttu-id="b324e-118">Syntaxe podrobnosti</span><span class="sxs-lookup"><span data-stu-id="b324e-118">Syntax details</span></span>
| <span data-ttu-id="b324e-119">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="b324e-119">Property</span></span> | <span data-ttu-id="b324e-120">Popis</span><span class="sxs-lookup"><span data-stu-id="b324e-120">Description</span></span> | <span data-ttu-id="b324e-121">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b324e-121">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b324e-122">jméno</span><span class="sxs-lookup"><span data-stu-id="b324e-122">name</span></span> |<span data-ttu-id="b324e-123">Název aktivity</span><span class="sxs-lookup"><span data-stu-id="b324e-123">Name of the activity</span></span> |<span data-ttu-id="b324e-124">Ano</span><span class="sxs-lookup"><span data-stu-id="b324e-124">Yes</span></span> |
| <span data-ttu-id="b324e-125">Popis</span><span class="sxs-lookup"><span data-stu-id="b324e-125">description</span></span> |<span data-ttu-id="b324e-126">Text popisující, co se používá aktivitu pro</span><span class="sxs-lookup"><span data-stu-id="b324e-126">Text describing what the activity is used for</span></span> |<span data-ttu-id="b324e-127">Ne</span><span class="sxs-lookup"><span data-stu-id="b324e-127">No</span></span> |
| <span data-ttu-id="b324e-128">type</span><span class="sxs-lookup"><span data-stu-id="b324e-128">type</span></span> |<span data-ttu-id="b324e-129">HDinsightHive</span><span class="sxs-lookup"><span data-stu-id="b324e-129">HDinsightHive</span></span> |<span data-ttu-id="b324e-130">Ano</span><span class="sxs-lookup"><span data-stu-id="b324e-130">Yes</span></span> |
| <span data-ttu-id="b324e-131">Vstupy</span><span class="sxs-lookup"><span data-stu-id="b324e-131">inputs</span></span> |<span data-ttu-id="b324e-132">Vstupy spotřebovávané aktivitou Hive</span><span class="sxs-lookup"><span data-stu-id="b324e-132">Inputs consumed by the Hive activity</span></span> |<span data-ttu-id="b324e-133">Ne</span><span class="sxs-lookup"><span data-stu-id="b324e-133">No</span></span> |
| <span data-ttu-id="b324e-134">Výstupy</span><span class="sxs-lookup"><span data-stu-id="b324e-134">outputs</span></span> |<span data-ttu-id="b324e-135">Výstupy produkované aktivitou Hive</span><span class="sxs-lookup"><span data-stu-id="b324e-135">Outputs produced by the Hive activity</span></span> |<span data-ttu-id="b324e-136">Ano</span><span class="sxs-lookup"><span data-stu-id="b324e-136">Yes</span></span> |
| <span data-ttu-id="b324e-137">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="b324e-137">linkedServiceName</span></span> |<span data-ttu-id="b324e-138">Referenční dokumentace ke clusteru HDInsight registrován jako propojené služby v datové továrně</span><span class="sxs-lookup"><span data-stu-id="b324e-138">Reference to the HDInsight cluster registered as a linked service in Data Factory</span></span> |<span data-ttu-id="b324e-139">Ano</span><span class="sxs-lookup"><span data-stu-id="b324e-139">Yes</span></span> |
| <span data-ttu-id="b324e-140">Skript</span><span class="sxs-lookup"><span data-stu-id="b324e-140">script</span></span> |<span data-ttu-id="b324e-141">Zadejte vloženého skriptu Hive</span><span class="sxs-lookup"><span data-stu-id="b324e-141">Specify the Hive script inline</span></span> |<span data-ttu-id="b324e-142">Ne</span><span class="sxs-lookup"><span data-stu-id="b324e-142">No</span></span> |
| <span data-ttu-id="b324e-143">cestu ke skriptu</span><span class="sxs-lookup"><span data-stu-id="b324e-143">script path</span></span> |<span data-ttu-id="b324e-144">Uložení skriptu Hive v Azure blob storage a zadejte cestu k souboru.</span><span class="sxs-lookup"><span data-stu-id="b324e-144">Store the Hive script in an Azure blob storage and provide the path to the file.</span></span> <span data-ttu-id="b324e-145">Pomocí vlastnosti 'skript' nebo 'scriptPath'.</span><span class="sxs-lookup"><span data-stu-id="b324e-145">Use 'script' or 'scriptPath' property.</span></span> <span data-ttu-id="b324e-146">Obě nelze použít společně.</span><span class="sxs-lookup"><span data-stu-id="b324e-146">Both cannot be used together.</span></span> <span data-ttu-id="b324e-147">Název souboru je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="b324e-147">The file name is case-sensitive.</span></span> |<span data-ttu-id="b324e-148">Ne</span><span class="sxs-lookup"><span data-stu-id="b324e-148">No</span></span> |
| <span data-ttu-id="b324e-149">definuje</span><span class="sxs-lookup"><span data-stu-id="b324e-149">defines</span></span> |<span data-ttu-id="b324e-150">Zadejte parametry dvojic klíč/hodnota pro odkazování v rámci skriptu Hive pomocí 'hiveconf.</span><span class="sxs-lookup"><span data-stu-id="b324e-150">Specify parameters as key/value pairs for referencing within the Hive script using 'hiveconf'</span></span> |<span data-ttu-id="b324e-151">Ne</span><span class="sxs-lookup"><span data-stu-id="b324e-151">No</span></span> |

## <a name="example"></a><span data-ttu-id="b324e-152">Příklad</span><span class="sxs-lookup"><span data-stu-id="b324e-152">Example</span></span>
<span data-ttu-id="b324e-153">Zvažte příklad herní protokolů analýzy, kam chcete identifikovat čas strávený uživatelé hraní her spuštění ve vaší společnosti.</span><span class="sxs-lookup"><span data-stu-id="b324e-153">Let’s consider an example of game logs analytics where you want to identify the time spent by users playing games launched by your company.</span></span> 

<span data-ttu-id="b324e-154">Následující protokol je ukázkový herní protokol, který je čárkou (`,`) oddělený a obsahuje následující pole – ProfileID, SessionStart, doba trvání, SrcIPAddress a GameType.</span><span class="sxs-lookup"><span data-stu-id="b324e-154">The following log is a sample game log, which is comma (`,`) separated and contains the following fields – ProfileID, SessionStart, Duration, SrcIPAddress, and GameType.</span></span>

```
1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
.....
```

<span data-ttu-id="b324e-155">**Skript podregistru** zpracovat tato data:</span><span class="sxs-lookup"><span data-stu-id="b324e-155">The **Hive script** to process this data:</span></span>

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

<span data-ttu-id="b324e-156">Spuštění tohoto skriptu Hive v kanálu pro vytváření dat, musíte udělat následující</span><span class="sxs-lookup"><span data-stu-id="b324e-156">To execute this Hive script in a Data Factory pipeline, you need to do the following</span></span>

1. <span data-ttu-id="b324e-157">Vytvoření propojené služby k registraci [vlastní HDInsight výpočetní cluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) nebo nakonfigurovat [výpočetní cluster HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="b324e-157">Create a linked service to register [your own HDInsight compute cluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or configure [on-demand HDInsight compute cluster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span></span> <span data-ttu-id="b324e-158">Umožňuje volat tuto propojenou službu "HDInsightLinkedService".</span><span class="sxs-lookup"><span data-stu-id="b324e-158">Let’s call this linked service “HDInsightLinkedService”.</span></span>
2. <span data-ttu-id="b324e-159">Vytvoření [propojená služba](data-factory-azure-blob-connector.md) ke konfiguraci připojení k hostování dat úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="b324e-159">Create a [linked service](data-factory-azure-blob-connector.md) to configure the connection to Azure Blob storage hosting the data.</span></span> <span data-ttu-id="b324e-160">Umožňuje volání této propojené služby "StorageLinkedService"</span><span class="sxs-lookup"><span data-stu-id="b324e-160">Let’s call this linked service “StorageLinkedService”</span></span>
3. <span data-ttu-id="b324e-161">Vytvoření [datové sady](data-factory-create-datasets.md) odkazující na vstupní a výstupní data.</span><span class="sxs-lookup"><span data-stu-id="b324e-161">Create [datasets](data-factory-create-datasets.md) pointing to the input and the output data.</span></span> <span data-ttu-id="b324e-162">Umožňuje volání vstupní datovou sadu "HiveSampleIn" a výstupní datovou sadu "HiveSampleOut"</span><span class="sxs-lookup"><span data-stu-id="b324e-162">Let’s call the input dataset “HiveSampleIn” and the output dataset “HiveSampleOut”</span></span>
4. <span data-ttu-id="b324e-163">Kopírovat dotaz Hive jako soubor do úložiště objektů Blob Azure nakonfigurovali v kroku #2.</span><span class="sxs-lookup"><span data-stu-id="b324e-163">Copy the Hive query as a file to Azure Blob Storage configured in step #2.</span></span> <span data-ttu-id="b324e-164">Pokud je úložiště pro hostování dat jiný než hostování tento soubor dotazu, vytvořte samostatné propojenou službu úložiště Azure a na ni odkazuje v rámci aktivity.</span><span class="sxs-lookup"><span data-stu-id="b324e-164">if the storage for hosting the data is different from the one hosting this query file, create a separate Azure Storage linked service and refer to it in the activity.</span></span> <span data-ttu-id="b324e-165">Použijte ** scriptPath ** zadat cestu k souboru dotazu hive a **scriptLinkedService** k určení úložiště Azure, která obsahuje soubor skriptu.</span><span class="sxs-lookup"><span data-stu-id="b324e-165">Use **scriptPath **to specify the path to hive query file and **scriptLinkedService** to specify the Azure storage that contains the script file.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="b324e-166">Můžete zadat také vloženého skriptu Hive v definici aktivity pomocí **skriptu** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="b324e-166">You can also provide the Hive script inline in the activity definition by using the **script** property.</span></span> <span data-ttu-id="b324e-167">Tento přístup není doporučujeme jako všechny speciální znaky ve skriptu v rámci je nutné dokumentu JSON předcházet a může způsobit problémy ladění.</span><span class="sxs-lookup"><span data-stu-id="b324e-167">We do not recommend this approach as all special characters in the script within the JSON document needs to be escaped and may cause debugging issues.</span></span> <span data-ttu-id="b324e-168">Osvědčeným postupem je postupujte podle kroku #4.</span><span class="sxs-lookup"><span data-stu-id="b324e-168">The best practice is to follow step #4.</span></span>
   > 
   > 
5. <span data-ttu-id="b324e-169">Vytvoření kanálu s aktivita HDInsightHive.</span><span class="sxs-lookup"><span data-stu-id="b324e-169">Create a pipeline with the HDInsightHive activity.</span></span> <span data-ttu-id="b324e-170">Aktivity procesy nebo transformací data.</span><span class="sxs-lookup"><span data-stu-id="b324e-170">The activity processes/transforms the data.</span></span>

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
6. <span data-ttu-id="b324e-171">Nasaďte kanál.</span><span class="sxs-lookup"><span data-stu-id="b324e-171">Deploy the pipeline.</span></span> <span data-ttu-id="b324e-172">V tématu [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="b324e-172">See [Creating pipelines](data-factory-create-pipelines.md) article for details.</span></span> 
7. <span data-ttu-id="b324e-173">Monitorování pomocí zobrazení monitorování a správu objekt pro vytváření dat kanál.</span><span class="sxs-lookup"><span data-stu-id="b324e-173">Monitor the pipeline using the data factory monitoring and management views.</span></span> <span data-ttu-id="b324e-174">V tématu [monitorování a Správa kanálů služby Data Factory](data-factory-monitor-manage-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="b324e-174">See [Monitoring and manage Data Factory pipelines](data-factory-monitor-manage-pipelines.md) article for details.</span></span> 

## <a name="specifying-parameters-for-a-hive-script"></a><span data-ttu-id="b324e-175">Zadání parametrů pro skript Hive</span><span class="sxs-lookup"><span data-stu-id="b324e-175">Specifying parameters for a Hive script</span></span>
<span data-ttu-id="b324e-176">V tomto příkladu herní protokoly jsou denně požity do Azure Blob Storage a jsou uložené ve složce rozdělený do oddílů pomocí data a času.</span><span class="sxs-lookup"><span data-stu-id="b324e-176">In this example, game logs are ingested daily into Azure Blob Storage and are stored in a folder partitioned with date and time.</span></span> <span data-ttu-id="b324e-177">Chcete Parametrizace skriptu Hive a předat vstupní složky dynamicky za běhu a také vytvořit několik výstup rozdělený do oddílů pomocí data a času.</span><span class="sxs-lookup"><span data-stu-id="b324e-177">You want to parameterize the Hive script and pass the input folder location dynamically during runtime and also produce the output partitioned with date and time.</span></span>

<span data-ttu-id="b324e-178">Chcete-li použít parametrizované skriptu Hive, postupujte takto</span><span class="sxs-lookup"><span data-stu-id="b324e-178">To use parameterized Hive script, do the following</span></span>

* <span data-ttu-id="b324e-179">Zadejte parametry v **definuje**.</span><span class="sxs-lookup"><span data-stu-id="b324e-179">Define the parameters in **defines**.</span></span>

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
* <span data-ttu-id="b324e-180">Ve skriptu Hive, najdete pomocí parametru **${hiveconf:parameterName}**.</span><span class="sxs-lookup"><span data-stu-id="b324e-180">In the Hive Script, refer to the parameter using **${hiveconf:parameterName}**.</span></span> 
  
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
## <a name="see-also"></a><span data-ttu-id="b324e-181">Viz také</span><span class="sxs-lookup"><span data-stu-id="b324e-181">See Also</span></span>
* [<span data-ttu-id="b324e-182">Pig aktivity</span><span class="sxs-lookup"><span data-stu-id="b324e-182">Pig Activity</span></span>](data-factory-pig-activity.md)
* [<span data-ttu-id="b324e-183">Činnost MapReduce</span><span class="sxs-lookup"><span data-stu-id="b324e-183">MapReduce Activity</span></span>](data-factory-map-reduce.md)
* [<span data-ttu-id="b324e-184">Streamované aktivitě Hadoop</span><span class="sxs-lookup"><span data-stu-id="b324e-184">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
* [<span data-ttu-id="b324e-185">Vyvolání programů Spark</span><span class="sxs-lookup"><span data-stu-id="b324e-185">Invoke Spark programs</span></span>](data-factory-spark.md)
* [<span data-ttu-id="b324e-186">Vyvolání skriptů jazyka R</span><span class="sxs-lookup"><span data-stu-id="b324e-186">Invoke R scripts</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

