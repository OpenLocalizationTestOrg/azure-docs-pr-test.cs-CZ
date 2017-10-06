---
title: "aaaUpload velkých objemů dat do Data Lake Store pomocí offline metod | Microsoft Docs"
description: "Použití hello AdlCopy nástroj toocopy dat z Azure úložiště objektů BLOB tooData Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 45321f6a-179f-4ee4-b8aa-efa7745b8eb6
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 42ef75142a26ebfab05d89614782a54c244c4bcb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-importexport-service-for-offline-copy-of-data-toodata-lake-store"></a>Pomocí služby Azure Import/Export hello offline kopie dat tooData Lake Store
V tomto článku se dozvíte, jak velký data toocopy nastaví (> 200 GB) do Azure Data Lake Store pomocí metod kopii offline, jako je hello [služba Azure Import/Export](../storage/common/storage-import-export-service.md). Konkrétně hello souboru, který slouží jako příklad v tomto článku je 339,420,860,416 bajtů, nebo o 319 GB na disku. Umožňuje volání 319GB.tsv tento soubor.

Služba Azure Import/Export Hello vám pomůže tootransfer velkých objemů dat další bezpečně tooAzure úložiště objektů Blob přesouvání pevným diskem disky tooan datového centra Azure.

## <a name="prerequisites"></a>Požadavky
Než začnete, musíte mít následující hello:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Účet úložiště Azure**.
* **Účet Azure Data Lake Store**. Návod, jak jeden, viz toocreate [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md)

## <a name="preparing-hello-data"></a>Příprava dat hello
Než začnete používat službu Import/Export hello, zalomení hello dat souboru toobe přenést **do kopie, které jsou menší než 200 GB** velikost. Nástroj pro import Hello nefunguje s soubory větší než 200 GB. V tomto kurzu jsme rozdělit hello soubor do bloků, 100 GB. Můžete to provést pomocí [emulaci](https://cygwin.com/install.html). Emulaci podporuje příkazy Linux. V takovém případě použijte hello následující příkaz:

    split -b 100m 319GB.tsv

operace rozdělení Hello vytvoří soubory s hello následující názvy.

    319GB.tsv-part-aa

    319GB.tsv-part-ab

    319GB.tsv-part-ac

    319GB.tsv-part-ad

## <a name="get-disks-ready-with-data"></a>Příprava disky s daty
Postupujte podle pokynů hello v [pomocí služby Azure Import/Export hello](../storage/common/storage-import-export-service.md) (v části hello **Příprava jednotky** část) tooprepare pevné disky. Tady je hello celkové pořadí:

1. Pořídit pevný disk, který splňuje toobe požadavek hello používá pro hello služba Azure Import/Export.
2. Určete účet úložiště Azure, kde hello data se zkopírují po uvidíte toohello datového centra Azure.
3. Použití hello [nástroj Azure Import/Export](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), nástroj příkazového řádku. Zde je ukázka fragment kódu, který ukazuje, jak toouse hello nástroj.

    ````
    WAImportExport PrepImport /sk:<StorageAccountKey> /t: <TargetDriveLetter> /format /encrypt /logdir:e:\myexportimportjob\logdir /j:e:\myexportimportjob\journal1.jrn /id:myexportimportjob /srcdir:F:\demo\ExImContainer /dstdir:importcontainer/vf1/
    ````
    V tématu [pomocí služby Azure Import/Export hello](../storage/common/storage-import-export-service.md) pro další fragmenty ukázka.
4. Hello předchozí příkaz vytvoří deník souboru v hello zadané umístění. Použít tento deník souboru toocreate úlohy importu z hello [portál Azure classic](https://manage.windowsazure.com).

## <a name="create-an-import-job"></a>Vytvoření úlohy importu
Nyní můžete vytvořit úlohy importu podle pokynů hello v [pomocí služby Azure Import/Export hello](../storage/common/storage-import-export-service.md) (v části hello **úlohy importu hello vytvořit** části). Pro tuto úlohu importu s další podrobnosti, také poskytněte soubor deníku hello vytvořili při přípravě hello diskových jednotek.

## <a name="physically-ship-hello-disks"></a>Fyzicky dodávat hello disky
Nyní můžete fyzicky dodávat hello disky tooan datového centra Azure. Zde hello data se zkopírují nad objekty toohello úložiště Azure BLOB, které jste zadali při vytváření úlohy importu hello. Navíc při vytváření úlohy hello, pokud jste se rozhodli tooprovide hello později, informace o sledování můžete nyní se vrátit úlohy a aktualizace hello serveru tooyour import sledování Číslo.

## <a name="copy-data-from-azure-storage-blobs-tooazure-data-lake-store"></a>Kopírování dat z Azure Storage blobs tooAzure Data Lake Store
Po hello stav hello úlohy importu ukazuje, že nebyla dokončena, můžete ověřit, zda jsou k dispozici do objektů BLOB Azure Storage hello, měl zadaná hello data. Pak můžete použít různé metody toomove, že data z hello objekty BLOB tooAzure Data Lake Store. Všechny hello dostupné možnosti pro nahrávání dat, najdete v části [příjem dat do Data Lake Store](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store).

V této části nám umožňují hello JSON definice používané toocreate kanál služby Azure Data Factory pro kopírování dat. Můžete použít tyto definice JSON z hello [portál Azure](../data-factory/data-factory-copy-activity-tutorial-using-azure-portal.md), nebo [Visual Studio](../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md), nebo [prostředí Azure PowerShell](../data-factory/data-factory-copy-activity-tutorial-using-powershell.md).

### <a name="source-linked-service-azure-storage-blob"></a>Zdroj propojené služby (objekt blob úložiště Azure)
````
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "description": "",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
````

### <a name="target-linked-service-azure-data-lake-store"></a>Cíl propojené služby (Azure Data Lake Store)
````
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "description": "",
        "typeProperties": {
            "authorization": "<Click 'Authorize' tooallow this data factory and hello activities it runs tooaccess this Data Lake Store with your access rights>",
            "dataLakeStoreUri": "https://<adls_account_name>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<OAuth session id from hello OAuth authorization session. Each session id is unique and may only be used once>"
        }
    }
}
````
### <a name="input-data-set"></a>Vstupní datové sady
````
{
    "name": "InputDataSet",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "importcontainer/vf1/"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
````
### <a name="output-data-set"></a>Výstupní datové sady
````
{
"name": "OutputDataSet",
"properties": {
  "published": false,
  "type": "AzureDataLakeStore",
  "linkedServiceName": "AzureDataLakeStoreLinkedService",
  "typeProperties": {
    "folderPath": "/importeddatafeb8job/"
    },
  "availability": {
    "frequency": "Hour",
    "interval": 1
    }
  }
}
````
### <a name="pipeline-copy-activity"></a>Kanál (aktivita kopírování)
````
{
    "name": "CopyImportedData",
    "properties": {
        "description": "Pipeline with copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "AzureDataLakeStoreSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity"
            }
        ],
        "start": "2016-02-08T22:00:00Z",
        "end": "2016-02-08T23:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
````
Další informace najdete v tématu [přesun dat z úložiště Azure blob tooAzure Data Lake Store pomocí Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md).

## <a name="reconstruct-hello-data-files-in-azure-data-lake-store"></a>Rekonstrukci hello datových souborů v Azure Data Lake Store
Spuštění se souborem, který byl 319 GB a něco stalo ho do souborů menší velikost tak, že by mohla být přenesena pomocí služby Azure Import/Export hello. Nyní když hello data v Azure Data Lake Store, jsme rekonstrukci hello souboru tooits původní velikost. Můžete použít následující toodo cmldts prostředí Azure PowerShell tak hello.

````
# Login tooour account
Login-AzureRmAccount

# List your subscriptions
Get-AzureRmSubscription

# Switch toohello subscription you want toowork with
Set-AzureRmContext –SubscriptionId
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

# Join  hello files
Join-AzureRmDataLakeStoreItem -AccountName "<adls_account_name" -Paths "/importeddatafeb8job/319GB.tsv-part-aa","/importeddatafeb8job/319GB.tsv-part-ab", "/importeddatafeb8job/319GB.tsv-part-ac", "/importeddatafeb8job/319GB.tsv-part-ad" -Destination "/importeddatafeb8job/MergedFile.csv”
````

## <a name="next-steps"></a>Další kroky
* [Zabezpečení dat ve službě Data Lake Store](data-lake-store-secure-data.md)
* [Použití Azure Data Lake Analytics se službou Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Použití Azure HDInsight se službou Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
