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
# <a name="transform-data-using-hive-activity-in-azure-data-factory"></a>Transformace dat pomocí Hive aktivity v Azure Data Factory 
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

Hello aktivitu HDInsight Hive v datové továrně [kanálu](data-factory-create-pipelines.md) provádí dotazy Hive na [vlastní](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) nebo [na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) clusteru HDInsight se systémem Windows nebo Linux. Tento článek vychází hello [aktivit transformace dat](data-factory-data-transformation-activities.md) článek, který poskytne Obecné přehled o transformaci dat a aktivit transformace hello podporována.

> [!NOTE] 
> Pokud jste tooAzure nový objekt pro vytváření dat, pročtěte [Úvod tooAzure Data Factory](data-factory-introduction.md) a hello kurz: [sestavit svůj první kanál dat](data-factory-build-your-first-pipeline.md) před přečtení tohoto článku. 

## <a name="syntax"></a>Syntaxe

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
## <a name="syntax-details"></a>Syntaxe podrobnosti
| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| jméno |Název aktivity hello |Ano |
| description |Popisuje, jaké aktivity hello se používá pro text |Ne |
| type |HDinsightHive |Ano |
| Vstupy |Vstupy spotřebované hello Hive aktivity |Ne |
| Výstupy |Výstupy vyprodukované hello Hive aktivity |Ano |
| linkedServiceName |Odkaz na clusteru HDInsight toohello registrován jako propojené služby v datové továrně |Ano |
| Skript |Zadejte vloženého skriptu Hive hello |Ne |
| cestu ke skriptu |Úložiště hello Hive skript v Azure blob storage a zadejte toohello hello cestě k souboru. Pomocí vlastnosti 'skript' nebo 'scriptPath'. Obě nelze použít společně. Název souboru Hello je malá a velká písmena. |Ne |
| definuje |Zadejte parametry dvojic klíč/hodnota pro odkazování v rámci skriptu Hive hello pomocí 'hiveconf. |Ne |

## <a name="example"></a>Příklad
Zvažte například ve hře protokoly analýzy, kam chcete tooidentify hello čas strávený uživatelé hraní her spuštění ve vaší společnosti. 

Hello následující protokol je ukázkový herní protokol, který je čárkou (`,`) oddělený a obsahuje následující pole – ProfileID, SessionStart, doba trvání, SrcIPAddress a GameType hello.

```
1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
.....
```

Hello **skript podregistru** tooprocess tato data:

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

tooexecute tomuto podregistru skript v kanálu pro vytváření dat, budete potřebovat následující toodo hello

1. Vytvoření propojené služby tooregister [vlastní HDInsight výpočetní cluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) nebo nakonfigurovat [výpočetní cluster HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Umožňuje volat tuto propojenou službu "HDInsightLinkedService".
2. Vytvoření [propojená služba](data-factory-azure-blob-connector.md) tooconfigure hello připojení tooAzure úložiště objektů Blob hostování hello data. Umožňuje volání této propojené služby "StorageLinkedService"
3. Vytvoření [datové sady](data-factory-create-datasets.md) ukazují toohello vstup a hello výstupní data. Umožňuje volání hello vstupní datovou sadu "HiveSampleIn" a hello výstupní datovou sadu "HiveSampleOut"
4. Kopírovat dotaz Hive hello jako soubor tooAzure nakonfigurovali v kroku #2 úložiště objektů Blob. Pokud hello úložiště pro hostování hello dat se liší od hello jeden hostování tento soubor dotazu, vytvořit samostatné propojenou službu úložiště Azure a tooit při hello aktivity. Použijte ** scriptPath ** hello toospecify cestě k souboru dotazu toohive a **scriptLinkedService** toospecify hello úložiště Azure, která obsahuje soubor skriptu hello. 
   
   > [!NOTE]
   > Vložený skript Hive hello v definici aktivity hello můžete zadat taky pomocí hello **skriptu** vlastnost. Nedoporučujeme tento přístup všechny speciální znaky v hello skriptu v dokumentu JSON hello musí toobe uvozené a může příčina ladění problémů. Hello osvědčeným postupem je toofollow krok #4.
   > 
   > 
5. Vytvoření kanálu s hello aktivita HDInsightHive. Aktivita Hello procesy nebo transformace dat hello.

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
6. Nasaďte hello kanálu. V tématu [vytváření kanálů](data-factory-create-pipelines.md) článku. 
7. Monitorování kanálu hello pomocí hello data objektu pro vytváření monitorování a zobrazení správy. V tématu [monitorování a Správa kanálů služby Data Factory](data-factory-monitor-manage-pipelines.md) článku. 

## <a name="specifying-parameters-for-a-hive-script"></a>Zadání parametrů pro skript Hive
V tomto příkladu herní protokoly jsou denně požity do Azure Blob Storage a jsou uložené ve složce rozdělený do oddílů pomocí data a času. Má skript Hive hello tooparameterize a předat hello vstupní složky dynamicky za běhu a také vytvořit několik výstup hello rozdělený do oddílů pomocí data a času.

toouse parametry skriptu Hive, proveďte následující hello

* Definujte parametry hello v **definuje**.

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
* V hello skriptu Hive, označují pomocí parametru toohello **${hiveconf:parameterName}**. 
  
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
## <a name="see-also"></a>Viz také
* [Pig aktivity](data-factory-pig-activity.md)
* [Činnost MapReduce](data-factory-map-reduce.md)
* [Streamované aktivitě Hadoop](data-factory-hadoop-streaming-activity.md)
* [Vyvolání programů Spark](data-factory-spark.md)
* [Vyvolání skriptů jazyka R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

