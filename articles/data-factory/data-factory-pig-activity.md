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
# <a name="transform-data-using-pig-activity-in-azure-data-factory"></a>Transformace dat pomocí Pig aktivity v Azure Data Factory
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Aktivita Hive](data-factory-hive-activity.md) 
> * [Pig aktivity](data-factory-pig-activity.md)
> * [Činnost MapReduce](data-factory-map-reduce.md)
> * [Streamované aktivitě Hadoop](data-factory-hadoop-streaming-activity.md)
> * [Spark aktivity](data-factory-spark.md)
> * [Aktivita Provedení dávky služby Machine Learning](data-factory-azure-ml-batch-execution-activity.md)
> * [Aktivita Aktualizace prostředků služby Machine Learning](data-factory-azure-ml-update-resource-activity.md)
> * [Aktivita Uložená procedura](data-factory-stored-proc-activity.md)
> * [Aktivita U-SQL služby Data Lake Analytics](data-factory-usql-activity.md)
> * [Vlastní aktivity rozhraní .NET](data-factory-use-custom-activities.md)

Hello aktivity HDInsight Pig v datové továrně [kanálu](data-factory-create-pipelines.md) provádí Pig dotazy na [vlastní](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) nebo [na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) clusteru HDInsight se systémem Windows nebo Linux. Tento článek vychází hello [aktivit transformace dat](data-factory-data-transformation-activities.md) článek, který poskytne Obecné přehled o transformaci dat a aktivit transformace hello podporována.

> [!NOTE] 
> Pokud jste tooAzure nový objekt pro vytváření dat, pročtěte [Úvod tooAzure Data Factory](data-factory-introduction.md) a hello kurz: [sestavit svůj první kanál dat](data-factory-build-your-first-pipeline.md) před přečtení tohoto článku. 

## <a name="syntax"></a>Syntaxe

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
## <a name="syntax-details"></a>Syntaxe podrobnosti
| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| jméno |Název aktivity hello |Ano |
| description |Popisuje, jaké aktivity hello se používá pro text |Ne |
| type |HDinsightPig |Ano |
| Vstupy |Jeden nebo více vstupů spotřebovávají hello Pig aktivity |Ne |
| Výstupy |Jeden nebo více výstupů vyprodukované hello Pig aktivity |Ano |
| linkedServiceName |Odkaz na clusteru HDInsight toohello registrován jako propojené služby v datové továrně |Ano |
| Skript |Zadejte vloženého skriptu Pig hello |Ne |
| cestu ke skriptu |Uložte skript Pig hello v Azure blob storage a zadejte toohello hello cestě k souboru. Pomocí vlastnosti 'skript' nebo 'scriptPath'. Obě nelze použít společně. Název souboru Hello je malá a velká písmena. |Ne |
| definuje |Zadejte parametry dvojic klíč/hodnota pro odkazování v rámci hello skript Pig |Ne |

## <a name="example"></a>Příklad
Zvažte například ve hře protokoly analýzy, kam chcete tooidentify hello času spotřebovaného přehrávače hraní her spuštění ve vaší společnosti.

Následující příklad protokolu o herní Hello je soubor oddělených čárkou (,). Obsahuje následující pole – ProfileID, SessionStart, doba trvání, SrcIPAddress a GameType hello.

```
1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
.....
```

Hello **vepřových skriptu** tooprocess tato data:

```
PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);

GroupProfile = Group PigSampleIn all;

PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);

Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');
```

tooexecute tento Pig skript v kanálu pro vytváření dat, hello následující kroky:

1. Vytvoření propojené služby tooregister [vlastní HDInsight výpočetní cluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) nebo nakonfigurovat [výpočetní cluster HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Umožňuje volání Tato propojená služba **HDInsightLinkedService**.
2. Vytvoření [propojená služba](data-factory-azure-blob-connector.md) tooconfigure hello připojení tooAzure úložiště objektů Blob hostování hello data. Umožňuje volání Tato propojená služba **StorageLinkedService**.
3. Vytvoření [datové sady](data-factory-create-datasets.md) ukazují toohello vstup a hello výstupní data. Umožňuje volání vstupní datové sady hello **PigSampleIn** a hello výstupní datovou sadu **PigSampleOut**.
4. Zkopírujte soubor hello Azure Blob Storage nakonfigurovali v kroku #2 hello Pig dotazu. Pokud hello úložiště Azure, který je hostitelem hello dat se liší od hello jeden, který je hostitelem hello soubor dotazů, vytvořte samostatné propojenou službu úložiště Azure. Naleznete toohello propojené služby v rámci konfigurace aktivit hello. Použijte ** scriptPath ** hello toospecify, cestu souboru skriptu toopig a **scriptLinkedService**. 
   
   > [!NOTE]
   > Vložený skript Pig hello v definici aktivity hello můžete zadat taky pomocí hello **skriptu** vlastnost. Však není doporučeno tento přístup jako všechny speciální znaky ve skriptu hello potřebuje toobe uvozené a může způsobit problémy ladění. Hello osvědčeným postupem je toofollow krok #4.
   > 
   > 
5. Vytvoření kanálu hello s hello HDInsightPig aktivity. Tato aktivita zpracovává hello vstupní data tak, že spustíte skript Pig na clusteru HDInsight.

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
6. Nasaďte hello kanálu. V tématu [vytváření kanálů](data-factory-create-pipelines.md) článku. 
7. Monitorování kanálu hello pomocí hello data objektu pro vytváření monitorování a zobrazení správy. V tématu [monitorování a Správa kanálů služby Data Factory](data-factory-monitor-manage-pipelines.md) článku.

## <a name="specifying-parameters-for-a-pig-script"></a>Zadání parametrů pro skript Pig
Vezměte v úvahu hello následující ukázka: herní protokoly jsou požity denně do Azure Blob Storage a uložena ve složce oddílů na základě datum a čas. Chcete skript Pig hello tooparameterize a předat hello vstupní složky dynamicky za běhu a také vytvořit několik výstup hello rozděleny do oddílů s datem a časem.

skript Pig s parametry toouse, proveďte následující hello:

* Definujte parametry hello v **definuje**.

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
* V hello skript Pig, označují toohello parametry s využitím '**$parameterName**, jak je znázorněno v hello následující ukázka:

    ```  
    PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);    
    GroupProfile = Group PigSampleIn all;        
    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);        
    Store PigSampleOut into '$Output' USING PigStorage (','); 
    ```
## <a name="see-also"></a>Viz také
* [Aktivita Hive](data-factory-hive-activity.md)
* [Činnost MapReduce](data-factory-map-reduce.md)
* [Streamované aktivitě Hadoop](data-factory-hadoop-streaming-activity.md)
* [Vyvolání programů Spark](data-factory-spark.md)
* [Vyvolání skriptů jazyka R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

