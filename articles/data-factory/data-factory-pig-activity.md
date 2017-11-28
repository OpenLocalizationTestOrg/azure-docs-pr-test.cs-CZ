---
title: "aaaTransform dat pomocí Pig aktivity v Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak můžete pomocí hello Pig aktivity v skriptů Pig toorun Azure data factory na na vyžádání nebo vaše vlastní cluster HDInsight."
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
ms.openlocfilehash: 3ad096c4a9e8603b09f574f6d129b4339a75d381
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="transform-data-using-pig-activity-in-azure-data-factory"></a><span data-ttu-id="7eb41-103">Transformace dat pomocí Pig aktivity v Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="7eb41-103">Transform data using Pig Activity in Azure Data Factory</span></span>
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="7eb41-104">Aktivita Hive</span><span class="sxs-lookup"><span data-stu-id="7eb41-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="7eb41-105">Pig aktivity</span><span class="sxs-lookup"><span data-stu-id="7eb41-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="7eb41-106">Činnost MapReduce</span><span class="sxs-lookup"><span data-stu-id="7eb41-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="7eb41-107">Streamované aktivitě Hadoop</span><span class="sxs-lookup"><span data-stu-id="7eb41-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="7eb41-108">Spark aktivity</span><span class="sxs-lookup"><span data-stu-id="7eb41-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="7eb41-109">Aktivita Provedení dávky služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="7eb41-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="7eb41-110">Aktivita Aktualizace prostředků služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="7eb41-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="7eb41-111">Aktivita Uložená procedura</span><span class="sxs-lookup"><span data-stu-id="7eb41-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="7eb41-112">Aktivita U-SQL služby Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="7eb41-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="7eb41-113">Vlastní aktivity rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="7eb41-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="7eb41-114">Hello aktivity HDInsight Pig v datové továrně [kanálu](data-factory-create-pipelines.md) provádí Pig dotazy na [vlastní](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) nebo [na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) clusteru HDInsight se systémem Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="7eb41-114">hello HDInsight Pig activity in a Data Factory [pipeline](data-factory-create-pipelines.md) executes Pig queries on [your own](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or [on-demand](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-based HDInsight cluster.</span></span> <span data-ttu-id="7eb41-115">Tento článek vychází hello [aktivit transformace dat](data-factory-data-transformation-activities.md) článek, který poskytne Obecné přehled o transformaci dat a aktivit transformace hello podporována.</span><span class="sxs-lookup"><span data-stu-id="7eb41-115">This article builds on hello [data transformation activities](data-factory-data-transformation-activities.md) article, which presents a general overview of data transformation and hello supported transformation activities.</span></span>

> [!NOTE] 
> <span data-ttu-id="7eb41-116">Pokud jste tooAzure nový objekt pro vytváření dat, pročtěte [Úvod tooAzure Data Factory](data-factory-introduction.md) a hello kurz: [sestavit svůj první kanál dat](data-factory-build-your-first-pipeline.md) před přečtení tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="7eb41-116">If you are new tooAzure Data Factory, read through [Introduction tooAzure Data Factory](data-factory-introduction.md) and do hello tutorial: [Build your first data pipeline](data-factory-build-your-first-pipeline.md) before reading this article.</span></span> 

## <a name="syntax"></a><span data-ttu-id="7eb41-117">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="7eb41-117">Syntax</span></span>

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
## <a name="syntax-details"></a><span data-ttu-id="7eb41-118">Syntaxe podrobnosti</span><span class="sxs-lookup"><span data-stu-id="7eb41-118">Syntax details</span></span>
| <span data-ttu-id="7eb41-119">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="7eb41-119">Property</span></span> | <span data-ttu-id="7eb41-120">Popis</span><span class="sxs-lookup"><span data-stu-id="7eb41-120">Description</span></span> | <span data-ttu-id="7eb41-121">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="7eb41-121">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7eb41-122">jméno</span><span class="sxs-lookup"><span data-stu-id="7eb41-122">name</span></span> |<span data-ttu-id="7eb41-123">Název aktivity hello</span><span class="sxs-lookup"><span data-stu-id="7eb41-123">Name of hello activity</span></span> |<span data-ttu-id="7eb41-124">Ano</span><span class="sxs-lookup"><span data-stu-id="7eb41-124">Yes</span></span> |
| <span data-ttu-id="7eb41-125">description</span><span class="sxs-lookup"><span data-stu-id="7eb41-125">description</span></span> |<span data-ttu-id="7eb41-126">Popisuje, jaké aktivity hello se používá pro text</span><span class="sxs-lookup"><span data-stu-id="7eb41-126">Text describing what hello activity is used for</span></span> |<span data-ttu-id="7eb41-127">Ne</span><span class="sxs-lookup"><span data-stu-id="7eb41-127">No</span></span> |
| <span data-ttu-id="7eb41-128">type</span><span class="sxs-lookup"><span data-stu-id="7eb41-128">type</span></span> |<span data-ttu-id="7eb41-129">HDinsightPig</span><span class="sxs-lookup"><span data-stu-id="7eb41-129">HDinsightPig</span></span> |<span data-ttu-id="7eb41-130">Ano</span><span class="sxs-lookup"><span data-stu-id="7eb41-130">Yes</span></span> |
| <span data-ttu-id="7eb41-131">Vstupy</span><span class="sxs-lookup"><span data-stu-id="7eb41-131">inputs</span></span> |<span data-ttu-id="7eb41-132">Jeden nebo více vstupů spotřebovávají hello Pig aktivity</span><span class="sxs-lookup"><span data-stu-id="7eb41-132">One or more inputs consumed by hello Pig activity</span></span> |<span data-ttu-id="7eb41-133">Ne</span><span class="sxs-lookup"><span data-stu-id="7eb41-133">No</span></span> |
| <span data-ttu-id="7eb41-134">Výstupy</span><span class="sxs-lookup"><span data-stu-id="7eb41-134">outputs</span></span> |<span data-ttu-id="7eb41-135">Jeden nebo více výstupů vyprodukované hello Pig aktivity</span><span class="sxs-lookup"><span data-stu-id="7eb41-135">One or more outputs produced by hello Pig activity</span></span> |<span data-ttu-id="7eb41-136">Ano</span><span class="sxs-lookup"><span data-stu-id="7eb41-136">Yes</span></span> |
| <span data-ttu-id="7eb41-137">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="7eb41-137">linkedServiceName</span></span> |<span data-ttu-id="7eb41-138">Odkaz na clusteru HDInsight toohello registrován jako propojené služby v datové továrně</span><span class="sxs-lookup"><span data-stu-id="7eb41-138">Reference toohello HDInsight cluster registered as a linked service in Data Factory</span></span> |<span data-ttu-id="7eb41-139">Ano</span><span class="sxs-lookup"><span data-stu-id="7eb41-139">Yes</span></span> |
| <span data-ttu-id="7eb41-140">Skript</span><span class="sxs-lookup"><span data-stu-id="7eb41-140">script</span></span> |<span data-ttu-id="7eb41-141">Zadejte vloženého skriptu Pig hello</span><span class="sxs-lookup"><span data-stu-id="7eb41-141">Specify hello Pig script inline</span></span> |<span data-ttu-id="7eb41-142">Ne</span><span class="sxs-lookup"><span data-stu-id="7eb41-142">No</span></span> |
| <span data-ttu-id="7eb41-143">cestu ke skriptu</span><span class="sxs-lookup"><span data-stu-id="7eb41-143">script path</span></span> |<span data-ttu-id="7eb41-144">Uložte skript Pig hello v Azure blob storage a zadejte toohello hello cestě k souboru.</span><span class="sxs-lookup"><span data-stu-id="7eb41-144">Store hello Pig script in an Azure blob storage and provide hello path toohello file.</span></span> <span data-ttu-id="7eb41-145">Pomocí vlastnosti 'skript' nebo 'scriptPath'.</span><span class="sxs-lookup"><span data-stu-id="7eb41-145">Use 'script' or 'scriptPath' property.</span></span> <span data-ttu-id="7eb41-146">Obě nelze použít společně.</span><span class="sxs-lookup"><span data-stu-id="7eb41-146">Both cannot be used together.</span></span> <span data-ttu-id="7eb41-147">Název souboru Hello je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="7eb41-147">hello file name is case-sensitive.</span></span> |<span data-ttu-id="7eb41-148">Ne</span><span class="sxs-lookup"><span data-stu-id="7eb41-148">No</span></span> |
| <span data-ttu-id="7eb41-149">definuje</span><span class="sxs-lookup"><span data-stu-id="7eb41-149">defines</span></span> |<span data-ttu-id="7eb41-150">Zadejte parametry dvojic klíč/hodnota pro odkazování v rámci hello skript Pig</span><span class="sxs-lookup"><span data-stu-id="7eb41-150">Specify parameters as key/value pairs for referencing within hello Pig script</span></span> |<span data-ttu-id="7eb41-151">Ne</span><span class="sxs-lookup"><span data-stu-id="7eb41-151">No</span></span> |

## <a name="example"></a><span data-ttu-id="7eb41-152">Příklad</span><span class="sxs-lookup"><span data-stu-id="7eb41-152">Example</span></span>
<span data-ttu-id="7eb41-153">Zvažte například ve hře protokoly analýzy, kam chcete tooidentify hello času spotřebovaného přehrávače hraní her spuštění ve vaší společnosti.</span><span class="sxs-lookup"><span data-stu-id="7eb41-153">Let’s consider an example of game logs analytics where you want tooidentify hello time spent by players playing games launched by your company.</span></span>

<span data-ttu-id="7eb41-154">Následující příklad protokolu o herní Hello je soubor oddělených čárkou (,).</span><span class="sxs-lookup"><span data-stu-id="7eb41-154">hello following sample game log is a comma (,) separated file.</span></span> <span data-ttu-id="7eb41-155">Obsahuje následující pole – ProfileID, SessionStart, doba trvání, SrcIPAddress a GameType hello.</span><span class="sxs-lookup"><span data-stu-id="7eb41-155">It contains hello following fields – ProfileID, SessionStart, Duration, SrcIPAddress, and GameType.</span></span>

```
1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
.....
```

<span data-ttu-id="7eb41-156">Hello **vepřových skriptu** tooprocess tato data:</span><span class="sxs-lookup"><span data-stu-id="7eb41-156">hello **Pig script** tooprocess this data:</span></span>

```
PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);

GroupProfile = Group PigSampleIn all;

PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);

Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');
```

<span data-ttu-id="7eb41-157">tooexecute tento Pig skript v kanálu pro vytváření dat, hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7eb41-157">tooexecute this Pig script in a Data Factory pipeline, do hello following steps:</span></span>

1. <span data-ttu-id="7eb41-158">Vytvoření propojené služby tooregister [vlastní HDInsight výpočetní cluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) nebo nakonfigurovat [výpočetní cluster HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="7eb41-158">Create a linked service tooregister [your own HDInsight compute cluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or configure [on-demand HDInsight compute cluster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span></span> <span data-ttu-id="7eb41-159">Umožňuje volání Tato propojená služba **HDInsightLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="7eb41-159">Let’s call this linked service **HDInsightLinkedService**.</span></span>
2. <span data-ttu-id="7eb41-160">Vytvoření [propojená služba](data-factory-azure-blob-connector.md) tooconfigure hello připojení tooAzure úložiště objektů Blob hostování hello data.</span><span class="sxs-lookup"><span data-stu-id="7eb41-160">Create a [linked service](data-factory-azure-blob-connector.md) tooconfigure hello connection tooAzure Blob storage hosting hello data.</span></span> <span data-ttu-id="7eb41-161">Umožňuje volání Tato propojená služba **StorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="7eb41-161">Let’s call this linked service **StorageLinkedService**.</span></span>
3. <span data-ttu-id="7eb41-162">Vytvoření [datové sady](data-factory-create-datasets.md) ukazují toohello vstup a hello výstupní data.</span><span class="sxs-lookup"><span data-stu-id="7eb41-162">Create [datasets](data-factory-create-datasets.md) pointing toohello input and hello output data.</span></span> <span data-ttu-id="7eb41-163">Umožňuje volání vstupní datové sady hello **PigSampleIn** a hello výstupní datovou sadu **PigSampleOut**.</span><span class="sxs-lookup"><span data-stu-id="7eb41-163">Let’s call hello input dataset **PigSampleIn** and hello output dataset **PigSampleOut**.</span></span>
4. <span data-ttu-id="7eb41-164">Zkopírujte soubor hello Azure Blob Storage nakonfigurovali v kroku #2 hello Pig dotazu.</span><span class="sxs-lookup"><span data-stu-id="7eb41-164">Copy hello Pig query in a file hello Azure Blob Storage configured in step #2.</span></span> <span data-ttu-id="7eb41-165">Pokud hello úložiště Azure, který je hostitelem hello dat se liší od hello jeden, který je hostitelem hello soubor dotazů, vytvořte samostatné propojenou službu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="7eb41-165">If hello Azure storage that hosts hello data is different from hello one that hosts hello query file, create a separate Azure Storage linked service.</span></span> <span data-ttu-id="7eb41-166">Naleznete toohello propojené služby v rámci konfigurace aktivit hello.</span><span class="sxs-lookup"><span data-stu-id="7eb41-166">Refer toohello linked service in hello activity configuration.</span></span> <span data-ttu-id="7eb41-167">Použijte ** scriptPath ** hello toospecify, cestu souboru skriptu toopig a **scriptLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="7eb41-167">Use **scriptPath **toospecify hello path toopig script file and **scriptLinkedService**.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="7eb41-168">Vložený skript Pig hello v definici aktivity hello můžete zadat taky pomocí hello **skriptu** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="7eb41-168">You can also provide hello Pig script inline in hello activity definition by using hello **script** property.</span></span> <span data-ttu-id="7eb41-169">Však není doporučeno tento přístup jako všechny speciální znaky ve skriptu hello potřebuje toobe uvozené a může způsobit problémy ladění.</span><span class="sxs-lookup"><span data-stu-id="7eb41-169">However, we do not recommend this approach as all special characters in hello script needs toobe escaped and may cause debugging issues.</span></span> <span data-ttu-id="7eb41-170">Hello osvědčeným postupem je toofollow krok #4.</span><span class="sxs-lookup"><span data-stu-id="7eb41-170">hello best practice is toofollow step #4.</span></span>
   > 
   > 
5. <span data-ttu-id="7eb41-171">Vytvoření kanálu hello s hello HDInsightPig aktivity.</span><span class="sxs-lookup"><span data-stu-id="7eb41-171">Create hello pipeline with hello HDInsightPig activity.</span></span> <span data-ttu-id="7eb41-172">Tato aktivita zpracovává hello vstupní data tak, že spustíte skript Pig na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="7eb41-172">This activity processes hello input data by running Pig script on HDInsight cluster.</span></span>

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
6. <span data-ttu-id="7eb41-173">Nasaďte hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="7eb41-173">Deploy hello pipeline.</span></span> <span data-ttu-id="7eb41-174">V tématu [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="7eb41-174">See [Creating pipelines](data-factory-create-pipelines.md) article for details.</span></span> 
7. <span data-ttu-id="7eb41-175">Monitorování kanálu hello pomocí hello data objektu pro vytváření monitorování a zobrazení správy.</span><span class="sxs-lookup"><span data-stu-id="7eb41-175">Monitor hello pipeline using hello data factory monitoring and management views.</span></span> <span data-ttu-id="7eb41-176">V tématu [monitorování a Správa kanálů služby Data Factory](data-factory-monitor-manage-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="7eb41-176">See [Monitoring and manage Data Factory pipelines](data-factory-monitor-manage-pipelines.md) article for details.</span></span>

## <a name="specifying-parameters-for-a-pig-script"></a><span data-ttu-id="7eb41-177">Zadání parametrů pro skript Pig</span><span class="sxs-lookup"><span data-stu-id="7eb41-177">Specifying parameters for a Pig script</span></span>
<span data-ttu-id="7eb41-178">Vezměte v úvahu hello následující ukázka: herní protokoly jsou požity denně do Azure Blob Storage a uložena ve složce oddílů na základě datum a čas.</span><span class="sxs-lookup"><span data-stu-id="7eb41-178">Consider hello following example: game logs are ingested daily into Azure Blob Storage and stored in a folder partitioned based on date and time.</span></span> <span data-ttu-id="7eb41-179">Chcete skript Pig hello tooparameterize a předat hello vstupní složky dynamicky za běhu a také vytvořit několik výstup hello rozděleny do oddílů s datem a časem.</span><span class="sxs-lookup"><span data-stu-id="7eb41-179">You want tooparameterize hello Pig script and pass hello input folder location dynamically during runtime and also produce hello output partitioned with date and time.</span></span>

<span data-ttu-id="7eb41-180">skript Pig s parametry toouse, proveďte následující hello:</span><span class="sxs-lookup"><span data-stu-id="7eb41-180">toouse parameterized Pig script, do hello following:</span></span>

* <span data-ttu-id="7eb41-181">Definujte parametry hello v **definuje**.</span><span class="sxs-lookup"><span data-stu-id="7eb41-181">Define hello parameters in **defines**.</span></span>

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
* <span data-ttu-id="7eb41-182">V hello skript Pig, označují toohello parametry s využitím '**$parameterName**, jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="7eb41-182">In hello Pig Script, refer toohello parameters using '**$parameterName**' as shown in hello following example:</span></span>

    ```  
    PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);    
    GroupProfile = Group PigSampleIn all;        
    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);        
    Store PigSampleOut into '$Output' USING PigStorage (','); 
    ```
## <a name="see-also"></a><span data-ttu-id="7eb41-183">Viz také</span><span class="sxs-lookup"><span data-stu-id="7eb41-183">See Also</span></span>
* [<span data-ttu-id="7eb41-184">Aktivita Hive</span><span class="sxs-lookup"><span data-stu-id="7eb41-184">Hive Activity</span></span>](data-factory-hive-activity.md)
* [<span data-ttu-id="7eb41-185">Činnost MapReduce</span><span class="sxs-lookup"><span data-stu-id="7eb41-185">MapReduce Activity</span></span>](data-factory-map-reduce.md)
* [<span data-ttu-id="7eb41-186">Streamované aktivitě Hadoop</span><span class="sxs-lookup"><span data-stu-id="7eb41-186">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
* [<span data-ttu-id="7eb41-187">Vyvolání programů Spark</span><span class="sxs-lookup"><span data-stu-id="7eb41-187">Invoke Spark programs</span></span>](data-factory-spark.md)
* [<span data-ttu-id="7eb41-188">Vyvolání skriptů jazyka R</span><span class="sxs-lookup"><span data-stu-id="7eb41-188">Invoke R scripts</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

