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
# <a name="transform-data-using-hadoop-streaming-activity-in-azure-data-factory"></a>Transformace dat pomocí streamované aktivitě Hadoop v Azure Data Factory
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

Můžete použít hello HDInsightStreamingActivity aktivity vyvolání úlohu streamování Hadoop z kanál služby Azure Data Factory. Hello následující fragment kódu JSON ukazuje hello syntaxe pro používání hello HDInsightStreamingActivity v souboru JSON kanálu. 

Hello HDInsight streamované aktivitě v datové továrně [kanálu](data-factory-create-pipelines.md) provede streamování Hadoop programy na [vlastní](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) nebo [na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight se systémem Windows nebo Linux cluster. Tento článek vychází hello [aktivit transformace dat](data-factory-data-transformation-activities.md) článek, který poskytne Obecné přehled o transformaci dat a aktivit transformace hello podporována.

> [!NOTE] 
> Pokud jste tooAzure nový objekt pro vytváření dat, pročtěte [Úvod tooAzure Data Factory](data-factory-introduction.md) a hello kurz: [sestavit svůj první kanál dat](data-factory-build-your-first-pipeline.md) před přečtení tohoto článku. 

## <a name="json-sample"></a>Ukázka JSON
Hello clusteru HDInsight se automaticky zadá příklad programy (wc.exe a cat.exe) a data (davinci.txt). Ve výchozím nastavení je název hello kontejneru, který je používán clusteru HDInsight hello hello název samotného clusteru hello. Například pokud je název clusteru myhdicluster, bude název kontejneru objektu blob hello přidruženého myhdicluster. 

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

Všimněte si hello následující body:

1. Sada hello **linkedServiceName** toohello název hello propojené služby, který odkazuje clusteru HDInsight tooyour které hello streamování mapreduce se spustí úloha.
2. Nastavte typ hello hello aktivity příliš**HDInsightStreaming**.
3. Pro hello **mapper** vlastnost, zadejte název spustitelného souboru mapper hello. V příkladu hello je cat.exe hello mapper spustitelný soubor.
4. Pro hello **reduktorem** vlastnost, zadejte název spustitelného souboru reduktorem hello. V příkladu hello je wc.exe hello reduktorem spustitelný soubor.
5. Pro hello **vstupní** zadejte vlastnost, zadejte hello vstupní soubor (včetně umístění hello) pro hello mapper. V příkladu hello: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample je kontejner objektů blob hello, například/data/Gutenberg je složka hello a davinci.txt je hello blob.
6. Pro hello **výstup** zadejte vlastnost, zadejte pro hello reduktorem hello výstupního souboru (včetně hello umístění). výstup Hello úlohy streamování Hadoop hello je zapsán toohello umístění zadané pro tuto vlastnost.
7. V hello **filePaths** části, zadejte cesty hello hello mapper a reduktorem spustitelných souborů. V příkladu hello: "adfsample/example/apps/wc.exe" adfsample je kontejner objektů blob hello, příklad nebo aplikací je složka hello a wc.exe je hello spustitelný soubor.
8. Pro hello **fileLinkedService** vlastnost, zadejte hello Azure Storage, propojené služby, která představuje hello úložiště Azure, který obsahuje soubory hello zadané v části filePaths hello.
9. Pro hello **argumenty** vlastnost, zadejte hello argumenty pro hello streamování úlohy.
10. Hello **getdebuginfo –** vlastnost je volitelná elementu. Pokud je nastavené tooFailure, protokoly hello se stáhnou pouze při selhání. Pokud je nastavené tooAlways, protokoly budou staženy vždy bez ohledu na stav spuštění hello.

> [!NOTE]
> Jak je znázorněno v příkladu hello, zadáte výstupní datovou sadu pro hello streamované aktivitě Hadoop pro hello **výstupy** vlastnost. Tato datová sada je právě fiktivní datovou sadu, která je požadovaná toodrive hello kanálu plán. Není nutné toospecify všechny vstupní datovou sadu aktivity hello pro hello **vstupy** vlastnost.  
> 
> 

## <a name="example"></a>Příklad
Hello kanál v tomto návodu spustí hello počet slov streamování mapy nebo snižte program v clusteru Azure HDInsight. 

### <a name="linked-services"></a>Propojené služby
#### <a name="azure-storage-linked-service"></a>Propojená služba Azure Storage
Nejprve je třeba vytvořit hello toolink propojené služby Azure Storage, který je používán hello Azure HDInsight clusteru toohello Azure data factory. Pokud jste zkopírujte a vložte následující kód hello, nevynechali tooreplace název účtu a účet klíč s názvem hello a klíč vašeho úložiště Azure. 

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
V dalším kroku vytvoření propojené služby toolink vaše Azure HDInsight clusteru toohello Azure data factory. Pokud jste zkopírujte a vložte následující kód hello, nahraďte název clusteru HDInsight hello názvem clusteru HDInsight a změňte hodnoty uživatelské jméno a heslo. 

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
Hello kanálu v tomto příkladu nevyžaduje žádné vstupy. Zadáte výstupní datovou sadu pro hello HDInsight streamované aktivitě. Tato datová sada je právě fiktivní datovou sadu, která je požadovaná toodrive hello kanálu plán. 

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

### <a name="pipeline"></a>Kanál
Hello kanálu v tomto příkladu má jenom jedna aktivita, která je typu: **HDInsightStreaming**. 

Hello clusteru HDInsight se automaticky zadá příklad programy (wc.exe a cat.exe) a data (davinci.txt). Ve výchozím nastavení je název hello kontejneru, který je používán clusteru HDInsight hello hello název samotného clusteru hello. Například pokud je název clusteru myhdicluster, bude název kontejneru objektu blob hello přidruženého myhdicluster.  

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
## <a name="see-also"></a>Viz také
* [Aktivita Hive](data-factory-hive-activity.md)
* [Pig aktivity](data-factory-pig-activity.md)
* [Činnost MapReduce](data-factory-map-reduce.md)
* [Vyvolání programů Spark](data-factory-spark.md)
* [Vyvolání skriptů jazyka R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

