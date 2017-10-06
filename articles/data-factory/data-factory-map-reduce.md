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
# <a name="invoke-mapreduce-programs-from-data-factory"></a>Vyvolání MapReduce programy z objektu pro vytváření dat
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

Hello činnost HDInsight MapReduce v datové továrně [kanálu](data-factory-create-pipelines.md) provede MapReduce programy na [vlastní](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) nebo [na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) clusteru HDInsight se systémem Windows nebo Linux. Tento článek vychází hello [aktivit transformace dat](data-factory-data-transformation-activities.md) článek, který poskytne Obecné přehled o transformaci dat a aktivit transformace hello podporována.

> [!NOTE] 
> Pokud jste tooAzure nový objekt pro vytváření dat, pročtěte [Úvod tooAzure Data Factory](data-factory-introduction.md) a hello kurz: [sestavit svůj první kanál dat](data-factory-build-your-first-pipeline.md) před přečtení tohoto článku.  

## <a name="introduction"></a>Úvod
Kanál v objektu pro vytváření dat Azure zpracovává data v služby propojené úložiště pomocí propojené výpočetní služby. Obsahuje posloupnost aktivit, kde každá aktivita provede konkrétní zpracování operace. Tento článek popisuje použití hello činnost MapReduce HDInsight.

V tématu [Pig](data-factory-pig-activity.md) a [Hive](data-factory-hive-activity.md) podrobnosti o spouštění Pig nebo Hive skriptů na HDInsight se systémem Windows nebo Linux clusteru z kanálu pomocí aktivity HDInsight Pig a Hive. 

## <a name="json-for-hdinsight-mapreduce-activity"></a>JSON pro činnost MapReduce s HDInsight
V definici JSON pro hello aktivita HDInsight hello: 

1. Sada hello **typ** z hello **aktivity** příliš**HDInsight**.
2. Zadejte název hello hello třídu pro **className** vlastnost.
3. Zadejte hello cesta toohello soubor JAR včetně hello název souboru pro **jarFilePath** vlastnost.
4. Zadejte hello propojené služby, která odkazuje toohello Azure Blob Storage, který obsahuje soubor JAR hello pro **jarLinkedService** vlastnost.   
5. Zadejte všechny argumenty pro hello MapReduce program v hello **argumenty** části. V době běhu zobrazí několik další argumenty (například: mapreduce.job.tags) z rozhraní MapReduce hello. pomocí možnosti a hodnoty jako argumenty, jak ukazuje následující příklad hello toodifferentiate vaše argumenty s argumenty hello MapReduce, zvažte (- s, – vstup, – výstupní atd., jsou možnosti bezprostředně následované jejich hodnoty).

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
Hello činnost MapReduce HDInsight toorun můžete použít libovolný soubor jar MapReduce v clusteru služby HDInsight. Následující ukázka definici JSON kanálu, hello je aktivita HDInsight v hello nakonfigurované toorun soubor Mahout JAR.

## <a name="sample-on-github"></a>Ukázce na Githubu
Můžete si stáhnout ukázku pro používání hello činnost MapReduce HDInsight z: [ukázky Data Factory na webu GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON/MapReduce_Activity_Sample).  

## <a name="running-hello-word-count-program"></a>Spuštění programu hello počet slov
Hello kanál v tomto příkladu se spustí hello program Mapa nebo snižte počet slov v clusteru Azure HDInsight.   

### <a name="linked-services"></a>Propojené služby
Nejprve je třeba vytvořit hello toolink propojené služby Azure Storage, který je používán hello Azure HDInsight clusteru toohello Azure data factory. Pokud jste zkopírujte a vložte následující kód hello, nevynechali tooreplace **název účtu** a **klíč účtu** s hello název a klíč vašeho úložiště Azure. 

#### <a name="azure-storage-linked-service"></a>Propojená služba Azure Storage

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

#### <a name="azure-hdinsight-linked-service"></a>Azure propojené služby HDInsight
V dalším kroku vytvoření propojené služby toolink vaše Azure HDInsight clusteru toohello Azure data factory. Pokud jste zkopírujte a vložte následující kód hello, nahraďte **název clusteru HDInsight** s názvem hello HDInsight cluster a změnit uživatelské jméno a heslo hodnoty.   

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

### <a name="datasets"></a>Datové sady
#### <a name="output-dataset"></a>Výstupní datové sady
Hello kanálu v tomto příkladu nevyžaduje žádné vstupy. Zadáte výstupní datovou sadu pro činnost MapReduce HDInsight hello. Tato datová sada je právě fiktivní datovou sadu, která je požadovaná toodrive hello kanálu plán.  

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

### <a name="pipeline"></a>Kanál
Hello kanálu v tomto příkladu má jenom jedna aktivita, která je typu: HDInsightMapReduce. Některé důležité vlastnosti hello v hello JSON jsou: 

| Vlastnost | Poznámky |
|:--- |:--- |
| type |Hello typ musí být nastavený příliš**HDInsightMapReduce**. |
| Název třídy |Název třídy hello je: **wordcount** |
| jarFilePath |Cesta toohello soubor jar obsahující hello třídy. Pokud jste zkopírujte a vložte následující kód hello, nezapomeňte toochange hello název clusteru hello. |
| jarLinkedService |Propojená služba, která obsahuje soubor jar hello Azure Storage. Tato propojená služba odkazuje toohello úložiště, který je přidružen hello HDInsight cluster. |
| Argumenty |Hello wordcount program přebírá dva argumenty, vstup a výstup. vstupní soubor Hello je soubor davinci.txt hello. |
| frequency/interval |Hello hodnoty těchto vlastností odpovídat hello výstupní datovou sadu. |
| linkedServiceName |odkazuje toohello propojené služby HDInsight, které jste vytvořili dříve. |

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

## <a name="run-spark-programs"></a>Spouštět programy Spark
MapReduce aktivity toorun Spark programy můžete použít v clusteru HDInsight Spark. Podrobnosti najdete v článku [Vyvolání programů Spark ze služby Azure Data Factory](data-factory-spark.md).  

[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[adfgetstartedmonitoring]:data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#monitor-pipelines 

[Developer Reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[Azure Portal]: http://portal.azure.com

## <a name="see-also"></a>Viz také
* [Aktivita Hive](data-factory-hive-activity.md)
* [Pig aktivity](data-factory-pig-activity.md)
* [Streamované aktivitě Hadoop](data-factory-hadoop-streaming-activity.md)
* [Vyvolání programů Spark](data-factory-spark.md)
* [Vyvolání skriptů jazyka R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

