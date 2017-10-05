---
title: "Transformace dat pomocí Pig aktivity v Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak můžete pomocí aktivity Pig v objektu pro vytváření dat Azure ke spouštění skriptů Pig na na vyžádání nebo vaše vlastní cluster HDInsight."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 5af07a1a-2087-455e-a67b-a79841b4ada5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: 182a637ab98955129d269e2afc3ba581aa1a7c03
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="transform-data-using-pig-activity-in-azure-data-factory"></a><span data-ttu-id="4f7b6-103">Transformace dat pomocí Pig aktivity v Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="4f7b6-103">Transform data using Pig Activity in Azure Data Factory</span></span>
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="4f7b6-104">Aktivita Hive</span><span class="sxs-lookup"><span data-stu-id="4f7b6-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="4f7b6-105">Pig aktivity</span><span class="sxs-lookup"><span data-stu-id="4f7b6-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="4f7b6-106">Činnost MapReduce</span><span class="sxs-lookup"><span data-stu-id="4f7b6-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="4f7b6-107">Streamované aktivitě Hadoop</span><span class="sxs-lookup"><span data-stu-id="4f7b6-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="4f7b6-108">Spark aktivity</span><span class="sxs-lookup"><span data-stu-id="4f7b6-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="4f7b6-109">Aktivita Provedení dávky služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="4f7b6-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="4f7b6-110">Aktivita Aktualizace prostředků služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="4f7b6-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="4f7b6-111">Aktivita Uložená procedura</span><span class="sxs-lookup"><span data-stu-id="4f7b6-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="4f7b6-112">Aktivita U-SQL služby Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="4f7b6-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="4f7b6-113">Vlastní aktivity rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="4f7b6-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="4f7b6-114">Aktivita HDInsight Pig v datové továrně [kanálu](data-factory-create-pipelines.md) provádí Pig dotazy na [vlastní](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) nebo [na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) clusteru HDInsight se systémem Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-114">The HDInsight Pig activity in a Data Factory [pipeline](data-factory-create-pipelines.md) executes Pig queries on [your own](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or [on-demand](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-based HDInsight cluster.</span></span> <span data-ttu-id="4f7b6-115">Tento článek vychází [aktivit transformace dat](data-factory-data-transformation-activities.md) článek, který poskytne Obecné přehled o transformaci dat a aktivity podporované transformace.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-115">This article builds on the [data transformation activities](data-factory-data-transformation-activities.md) article, which presents a general overview of data transformation and the supported transformation activities.</span></span>

> [!NOTE] 
> <span data-ttu-id="4f7b6-116">Pokud jste do Azure Data Factory nové, přečtěte si [Úvod do Azure Data Factory](data-factory-introduction.md) a proveďte kurz: [sestavit svůj první kanál dat](data-factory-build-your-first-pipeline.md) před přečtení tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-116">If you are new to Azure Data Factory, read through [Introduction to Azure Data Factory](data-factory-introduction.md) and do the tutorial: [Build your first data pipeline](data-factory-build-your-first-pipeline.md) before reading this article.</span></span> 

## <a name="syntax"></a><span data-ttu-id="4f7b6-117">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="4f7b6-117">Syntax</span></span>

```JSON
{
    "name": "HiveActivitySamplePipeline",
      "properties": {
    "activities": [
        {
            "name": "Pig Activity",
            "description": "description",
            "type": "HDInsightPig",
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
                  "script": "Pig script",
                  "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
                  "defines": {
                    "param1": "param1Value"
                  }
            },
               "scheduler": {
                  "frequency": "Day",
                  "interval": 1
            }
          }
    ]
  }
}
```
## <a name="syntax-details"></a><span data-ttu-id="4f7b6-118">Syntaxe podrobnosti</span><span class="sxs-lookup"><span data-stu-id="4f7b6-118">Syntax details</span></span>
| <span data-ttu-id="4f7b6-119">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="4f7b6-119">Property</span></span> | <span data-ttu-id="4f7b6-120">Popis</span><span class="sxs-lookup"><span data-stu-id="4f7b6-120">Description</span></span> | <span data-ttu-id="4f7b6-121">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="4f7b6-121">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4f7b6-122">jméno</span><span class="sxs-lookup"><span data-stu-id="4f7b6-122">name</span></span> |<span data-ttu-id="4f7b6-123">Název aktivity</span><span class="sxs-lookup"><span data-stu-id="4f7b6-123">Name of the activity</span></span> |<span data-ttu-id="4f7b6-124">Ano</span><span class="sxs-lookup"><span data-stu-id="4f7b6-124">Yes</span></span> |
| <span data-ttu-id="4f7b6-125">Popis</span><span class="sxs-lookup"><span data-stu-id="4f7b6-125">description</span></span> |<span data-ttu-id="4f7b6-126">Text popisující, co se používá aktivitu pro</span><span class="sxs-lookup"><span data-stu-id="4f7b6-126">Text describing what the activity is used for</span></span> |<span data-ttu-id="4f7b6-127">Ne</span><span class="sxs-lookup"><span data-stu-id="4f7b6-127">No</span></span> |
| <span data-ttu-id="4f7b6-128">type</span><span class="sxs-lookup"><span data-stu-id="4f7b6-128">type</span></span> |<span data-ttu-id="4f7b6-129">HDinsightPig</span><span class="sxs-lookup"><span data-stu-id="4f7b6-129">HDinsightPig</span></span> |<span data-ttu-id="4f7b6-130">Ano</span><span class="sxs-lookup"><span data-stu-id="4f7b6-130">Yes</span></span> |
| <span data-ttu-id="4f7b6-131">Vstupy</span><span class="sxs-lookup"><span data-stu-id="4f7b6-131">inputs</span></span> |<span data-ttu-id="4f7b6-132">Jeden nebo více vstupů spotřebovávané aktivitou Pig</span><span class="sxs-lookup"><span data-stu-id="4f7b6-132">One or more inputs consumed by the Pig activity</span></span> |<span data-ttu-id="4f7b6-133">Ne</span><span class="sxs-lookup"><span data-stu-id="4f7b6-133">No</span></span> |
| <span data-ttu-id="4f7b6-134">Výstupy</span><span class="sxs-lookup"><span data-stu-id="4f7b6-134">outputs</span></span> |<span data-ttu-id="4f7b6-135">Jeden nebo více výstupů produkované aktivitou Pig</span><span class="sxs-lookup"><span data-stu-id="4f7b6-135">One or more outputs produced by the Pig activity</span></span> |<span data-ttu-id="4f7b6-136">Ano</span><span class="sxs-lookup"><span data-stu-id="4f7b6-136">Yes</span></span> |
| <span data-ttu-id="4f7b6-137">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="4f7b6-137">linkedServiceName</span></span> |<span data-ttu-id="4f7b6-138">Referenční dokumentace ke clusteru HDInsight registrován jako propojené služby v datové továrně</span><span class="sxs-lookup"><span data-stu-id="4f7b6-138">Reference to the HDInsight cluster registered as a linked service in Data Factory</span></span> |<span data-ttu-id="4f7b6-139">Ano</span><span class="sxs-lookup"><span data-stu-id="4f7b6-139">Yes</span></span> |
| <span data-ttu-id="4f7b6-140">Skript</span><span class="sxs-lookup"><span data-stu-id="4f7b6-140">script</span></span> |<span data-ttu-id="4f7b6-141">Zadejte vložený skript Pig</span><span class="sxs-lookup"><span data-stu-id="4f7b6-141">Specify the Pig script inline</span></span> |<span data-ttu-id="4f7b6-142">Ne</span><span class="sxs-lookup"><span data-stu-id="4f7b6-142">No</span></span> |
| <span data-ttu-id="4f7b6-143">cestu ke skriptu</span><span class="sxs-lookup"><span data-stu-id="4f7b6-143">script path</span></span> |<span data-ttu-id="4f7b6-144">Uložte skript Pig v Azure blob storage a zadejte cestu k souboru.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-144">Store the Pig script in an Azure blob storage and provide the path to the file.</span></span> <span data-ttu-id="4f7b6-145">Pomocí vlastnosti 'skript' nebo 'scriptPath'.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-145">Use 'script' or 'scriptPath' property.</span></span> <span data-ttu-id="4f7b6-146">Obě nelze použít společně.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-146">Both cannot be used together.</span></span> <span data-ttu-id="4f7b6-147">Název souboru je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-147">The file name is case-sensitive.</span></span> |<span data-ttu-id="4f7b6-148">Ne</span><span class="sxs-lookup"><span data-stu-id="4f7b6-148">No</span></span> |
| <span data-ttu-id="4f7b6-149">definuje</span><span class="sxs-lookup"><span data-stu-id="4f7b6-149">defines</span></span> |<span data-ttu-id="4f7b6-150">Zadejte parametry dvojic klíč/hodnota pro odkazování v rámci skript Pig</span><span class="sxs-lookup"><span data-stu-id="4f7b6-150">Specify parameters as key/value pairs for referencing within the Pig script</span></span> |<span data-ttu-id="4f7b6-151">Ne</span><span class="sxs-lookup"><span data-stu-id="4f7b6-151">No</span></span> |

## <a name="example"></a><span data-ttu-id="4f7b6-152">Příklad</span><span class="sxs-lookup"><span data-stu-id="4f7b6-152">Example</span></span>
<span data-ttu-id="4f7b6-153">Zvažte příklad herní protokolů analýzy, kam chcete identifikovat čas strávený hráči, hraní her spuštění ve vaší společnosti.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-153">Let’s consider an example of game logs analytics where you want to identify the time spent by players playing games launched by your company.</span></span>

<span data-ttu-id="4f7b6-154">Následující příklad herní protokolu je soubor oddělených čárkou (,).</span><span class="sxs-lookup"><span data-stu-id="4f7b6-154">The following sample game log is a comma (,) separated file.</span></span> <span data-ttu-id="4f7b6-155">Obsahuje následující pole – ProfileID, SessionStart, doba trvání, SrcIPAddress a GameType.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-155">It contains the following fields – ProfileID, SessionStart, Duration, SrcIPAddress, and GameType.</span></span>

```
1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
.....
```

<span data-ttu-id="4f7b6-156">**Vepřových skriptu** zpracovat tato data:</span><span class="sxs-lookup"><span data-stu-id="4f7b6-156">The **Pig script** to process this data:</span></span>

```
PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);

GroupProfile = Group PigSampleIn all;

PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);

Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');
```

<span data-ttu-id="4f7b6-157">Spustit tento skript Pig v objektu pro vytváření dat kanál, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4f7b6-157">To execute this Pig script in a Data Factory pipeline, do the following steps:</span></span>

1. <span data-ttu-id="4f7b6-158">Vytvoření propojené služby k registraci [vlastní HDInsight výpočetní cluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) nebo nakonfigurovat [výpočetní cluster HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="4f7b6-158">Create a linked service to register [your own HDInsight compute cluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or configure [on-demand HDInsight compute cluster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span></span> <span data-ttu-id="4f7b6-159">Umožňuje volání Tato propojená služba **HDInsightLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-159">Let’s call this linked service **HDInsightLinkedService**.</span></span>
2. <span data-ttu-id="4f7b6-160">Vytvoření [propojená služba](data-factory-azure-blob-connector.md) ke konfiguraci připojení k hostování dat úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-160">Create a [linked service](data-factory-azure-blob-connector.md) to configure the connection to Azure Blob storage hosting the data.</span></span> <span data-ttu-id="4f7b6-161">Umožňuje volání Tato propojená služba **StorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-161">Let’s call this linked service **StorageLinkedService**.</span></span>
3. <span data-ttu-id="4f7b6-162">Vytvoření [datové sady](data-factory-create-datasets.md) odkazující na vstupní a výstupní data.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-162">Create [datasets](data-factory-create-datasets.md) pointing to the input and the output data.</span></span> <span data-ttu-id="4f7b6-163">Umožňuje volání vstupní datové sady **PigSampleIn** a výstupní datovou sadu **PigSampleOut**.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-163">Let’s call the input dataset **PigSampleIn** and the output dataset **PigSampleOut**.</span></span>
4. <span data-ttu-id="4f7b6-164">Zkopírujte tento dotaz Pig v souboru Azure Blob Storage, nakonfigurovali v kroku #2.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-164">Copy the Pig query in a file the Azure Blob Storage configured in step #2.</span></span> <span data-ttu-id="4f7b6-165">Pokud službu Azure storage, který je hostitelem dat se liší od ten, který je hostitelem soubor dotazů, vytvořte samostatné propojenou službu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-165">If the Azure storage that hosts the data is different from the one that hosts the query file, create a separate Azure Storage linked service.</span></span> <span data-ttu-id="4f7b6-166">Naleznete propojené služby v konfiguraci aktivity.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-166">Refer to the linked service in the activity configuration.</span></span> <span data-ttu-id="4f7b6-167">Použijte ** scriptPath ** zadat cestu k souboru skriptu pig a **scriptLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-167">Use **scriptPath **to specify the path to pig script file and **scriptLinkedService**.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="4f7b6-168">Vložený skript Pig v definici aktivity můžete zadat taky pomocí **skriptu** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-168">You can also provide the Pig script inline in the activity definition by using the **script** property.</span></span> <span data-ttu-id="4f7b6-169">Ale nedoporučujeme jako všechny speciální znaky v je nutné skript nutné uvést tento přístup a může způsobit problémy ladění.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-169">However, we do not recommend this approach as all special characters in the script needs to be escaped and may cause debugging issues.</span></span> <span data-ttu-id="4f7b6-170">Osvědčeným postupem je postupujte podle kroku #4.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-170">The best practice is to follow step #4.</span></span>
   > 
   > 
5. <span data-ttu-id="4f7b6-171">Vytvoření kanálu s aktivitou HDInsightPig.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-171">Create the pipeline with the HDInsightPig activity.</span></span> <span data-ttu-id="4f7b6-172">Tato aktivita zpracovává vstupní data tak, že spustíte skript Pig na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-172">This activity processes the input data by running Pig script on HDInsight cluster.</span></span>

    ```JSON   
    {
      "name": "PigActivitySamplePipeline",
      "properties": {
        "activities": [
          {
            "name": "PigActivitySample",
            "type": "HDInsightPig",
            "inputs": [
              {
                "name": "PigSampleIn"
              }
            ],
            "outputs": [
              {
                "name": "PigSampleOut"
              }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "typeproperties": {
              "scriptPath": "adfwalkthrough\\scripts\\enrichlogs.pig",
              "scriptLinkedService": "StorageLinkedService"
            },
               "scheduler": {
                  "frequency": "Day",
                  "interval": 1
            }
          }
        ]
      }
    } 
    ```
6. <span data-ttu-id="4f7b6-173">Nasaďte kanál.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-173">Deploy the pipeline.</span></span> <span data-ttu-id="4f7b6-174">V tématu [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-174">See [Creating pipelines](data-factory-create-pipelines.md) article for details.</span></span> 
7. <span data-ttu-id="4f7b6-175">Monitorování pomocí zobrazení monitorování a správu objekt pro vytváření dat kanál.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-175">Monitor the pipeline using the data factory monitoring and management views.</span></span> <span data-ttu-id="4f7b6-176">V tématu [monitorování a Správa kanálů služby Data Factory](data-factory-monitor-manage-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-176">See [Monitoring and manage Data Factory pipelines](data-factory-monitor-manage-pipelines.md) article for details.</span></span>

## <a name="specifying-parameters-for-a-pig-script"></a><span data-ttu-id="4f7b6-177">Zadání parametrů pro skript Pig</span><span class="sxs-lookup"><span data-stu-id="4f7b6-177">Specifying parameters for a Pig script</span></span>
<span data-ttu-id="4f7b6-178">Podívejte se na následující příklad: herní protokoly jsou požity denně do Azure Blob Storage a uložena ve složce oddílů na základě datum a čas.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-178">Consider the following example: game logs are ingested daily into Azure Blob Storage and stored in a folder partitioned based on date and time.</span></span> <span data-ttu-id="4f7b6-179">Chcete Parametrizace skript Pig a předat vstupní složky dynamicky za běhu a také vytvořit několik výstup rozdělený do oddílů pomocí data a času.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-179">You want to parameterize the Pig script and pass the input folder location dynamically during runtime and also produce the output partitioned with date and time.</span></span>

<span data-ttu-id="4f7b6-180">Pokud chcete používat parametrizované skript Pig, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="4f7b6-180">To use parameterized Pig script, do the following:</span></span>

* <span data-ttu-id="4f7b6-181">Zadejte parametry v **definuje**.</span><span class="sxs-lookup"><span data-stu-id="4f7b6-181">Define the parameters in **defines**.</span></span>

    ```JSON  
    {
        "name": "PigActivitySamplePipeline",
          "properties": {
        "activities": [
            {
                "name": "PigActivitySample",
                "type": "HDInsightPig",
                "inputs": [
                      {
                        "name": "PigSampleIn"
                      }
                ],
                "outputs": [
                      {
                        "name": "PigSampleOut"
                      }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                      "scriptPath": "adfwalkthrough\\scripts\\samplepig.hql",
                      "scriptLinkedService": "StorageLinkedService",
                      "defines": {
                        "Input": "$$Text.Format('wasb: //adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0: yyyy}/monthno={0:MM}/dayno={0: dd}/',SliceStart)",
                        "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)"
                      }
                },
                   "scheduler": {
                      "frequency": "Day",
                      "interval": 1
                }
              }
        ]
      }
    }
    ```  
* <span data-ttu-id="4f7b6-182">Ve skriptu Pig odkazování na parametry pomocí '**$parameterName**, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="4f7b6-182">In the Pig Script, refer to the parameters using '**$parameterName**' as shown in the following example:</span></span>

    ```  
    PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);    
    GroupProfile = Group PigSampleIn all;        
    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);        
    Store PigSampleOut into '$Output' USING PigStorage (','); 
    ```
## <a name="see-also"></a><span data-ttu-id="4f7b6-183">Viz také</span><span class="sxs-lookup"><span data-stu-id="4f7b6-183">See Also</span></span>
* [<span data-ttu-id="4f7b6-184">Aktivita Hive</span><span class="sxs-lookup"><span data-stu-id="4f7b6-184">Hive Activity</span></span>](data-factory-hive-activity.md)
* [<span data-ttu-id="4f7b6-185">Činnost MapReduce</span><span class="sxs-lookup"><span data-stu-id="4f7b6-185">MapReduce Activity</span></span>](data-factory-map-reduce.md)
* [<span data-ttu-id="4f7b6-186">Streamované aktivitě Hadoop</span><span class="sxs-lookup"><span data-stu-id="4f7b6-186">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
* [<span data-ttu-id="4f7b6-187">Vyvolání programů Spark</span><span class="sxs-lookup"><span data-stu-id="4f7b6-187">Invoke Spark programs</span></span>](data-factory-spark.md)
* [<span data-ttu-id="4f7b6-188">Vyvolání skriptů jazyka R</span><span class="sxs-lookup"><span data-stu-id="4f7b6-188">Invoke R scripts</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

