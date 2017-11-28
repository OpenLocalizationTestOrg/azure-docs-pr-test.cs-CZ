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
# <a name="use-hello-azure-importexport-service-for-offline-copy-of-data-toodata-lake-store"></a><span data-ttu-id="f29b3-103">Pomocí služby Azure Import/Export hello offline kopie dat tooData Lake Store</span><span class="sxs-lookup"><span data-stu-id="f29b3-103">Use hello Azure Import/Export service for offline copy of data tooData Lake Store</span></span>
<span data-ttu-id="f29b3-104">V tomto článku se dozvíte, jak velký data toocopy nastaví (> 200 GB) do Azure Data Lake Store pomocí metod kopii offline, jako je hello [služba Azure Import/Export](../storage/common/storage-import-export-service.md).</span><span class="sxs-lookup"><span data-stu-id="f29b3-104">In this article, you'll learn how toocopy huge data sets (>200 GB) into an Azure Data Lake Store by using offline copy methods, like hello [Azure Import/Export service](../storage/common/storage-import-export-service.md).</span></span> <span data-ttu-id="f29b3-105">Konkrétně hello souboru, který slouží jako příklad v tomto článku je 339,420,860,416 bajtů, nebo o 319 GB na disku.</span><span class="sxs-lookup"><span data-stu-id="f29b3-105">Specifically, hello file used as an example in this article is 339,420,860,416 bytes, or about 319 GB on disk.</span></span> <span data-ttu-id="f29b3-106">Umožňuje volání 319GB.tsv tento soubor.</span><span class="sxs-lookup"><span data-stu-id="f29b3-106">Let's call this file 319GB.tsv.</span></span>

<span data-ttu-id="f29b3-107">Služba Azure Import/Export Hello vám pomůže tootransfer velkých objemů dat další bezpečně tooAzure úložiště objektů Blob přesouvání pevným diskem disky tooan datového centra Azure.</span><span class="sxs-lookup"><span data-stu-id="f29b3-107">hello Azure Import/Export service helps you tootransfer large amounts of data more securely tooAzure Blob storage by shipping hard disk drives tooan Azure datacenter.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f29b3-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f29b3-108">Prerequisites</span></span>
<span data-ttu-id="f29b3-109">Než začnete, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="f29b3-109">Before you begin, you must have hello following:</span></span>

* <span data-ttu-id="f29b3-110">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="f29b3-110">**An Azure subscription**.</span></span> <span data-ttu-id="f29b3-111">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f29b3-111">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="f29b3-112">**Účet úložiště Azure**.</span><span class="sxs-lookup"><span data-stu-id="f29b3-112">**An Azure storage account**.</span></span>
* <span data-ttu-id="f29b3-113">**Účet Azure Data Lake Store**.</span><span class="sxs-lookup"><span data-stu-id="f29b3-113">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="f29b3-114">Návod, jak jeden, viz toocreate [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="f29b3-114">For instructions on how toocreate one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>

## <a name="preparing-hello-data"></a><span data-ttu-id="f29b3-115">Příprava dat hello</span><span class="sxs-lookup"><span data-stu-id="f29b3-115">Preparing hello data</span></span>
<span data-ttu-id="f29b3-116">Než začnete používat službu Import/Export hello, zalomení hello dat souboru toobe přenést **do kopie, které jsou menší než 200 GB** velikost.</span><span class="sxs-lookup"><span data-stu-id="f29b3-116">Before using hello Import/Export service, break hello data file toobe transferred **into copies that are less than 200 GB** in size.</span></span> <span data-ttu-id="f29b3-117">Nástroj pro import Hello nefunguje s soubory větší než 200 GB.</span><span class="sxs-lookup"><span data-stu-id="f29b3-117">hello import tool does not work with files greater than 200 GB.</span></span> <span data-ttu-id="f29b3-118">V tomto kurzu jsme rozdělit hello soubor do bloků, 100 GB.</span><span class="sxs-lookup"><span data-stu-id="f29b3-118">In this tutorial, we split hello file into chunks of 100 GB each.</span></span> <span data-ttu-id="f29b3-119">Můžete to provést pomocí [emulaci](https://cygwin.com/install.html).</span><span class="sxs-lookup"><span data-stu-id="f29b3-119">You can do this by using [Cygwin](https://cygwin.com/install.html).</span></span> <span data-ttu-id="f29b3-120">Emulaci podporuje příkazy Linux.</span><span class="sxs-lookup"><span data-stu-id="f29b3-120">Cygwin supports Linux commands.</span></span> <span data-ttu-id="f29b3-121">V takovém případě použijte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f29b3-121">In this case, use hello following command:</span></span>

    split -b 100m 319GB.tsv

<span data-ttu-id="f29b3-122">operace rozdělení Hello vytvoří soubory s hello následující názvy.</span><span class="sxs-lookup"><span data-stu-id="f29b3-122">hello split operation creates files with hello following names.</span></span>

    319GB.tsv-part-aa

    319GB.tsv-part-ab

    319GB.tsv-part-ac

    319GB.tsv-part-ad

## <a name="get-disks-ready-with-data"></a><span data-ttu-id="f29b3-123">Příprava disky s daty</span><span class="sxs-lookup"><span data-stu-id="f29b3-123">Get disks ready with data</span></span>
<span data-ttu-id="f29b3-124">Postupujte podle pokynů hello v [pomocí služby Azure Import/Export hello](../storage/common/storage-import-export-service.md) (v části hello **Příprava jednotky** část) tooprepare pevné disky.</span><span class="sxs-lookup"><span data-stu-id="f29b3-124">Follow hello instructions in [Using hello Azure Import/Export service](../storage/common/storage-import-export-service.md) (under hello **Prepare your drives** section) tooprepare your hard drives.</span></span> <span data-ttu-id="f29b3-125">Tady je hello celkové pořadí:</span><span class="sxs-lookup"><span data-stu-id="f29b3-125">Here's hello overall sequence:</span></span>

1. <span data-ttu-id="f29b3-126">Pořídit pevný disk, který splňuje toobe požadavek hello používá pro hello služba Azure Import/Export.</span><span class="sxs-lookup"><span data-stu-id="f29b3-126">Procure a hard disk that meets hello requirement toobe used for hello Azure Import/Export service.</span></span>
2. <span data-ttu-id="f29b3-127">Určete účet úložiště Azure, kde hello data se zkopírují po uvidíte toohello datového centra Azure.</span><span class="sxs-lookup"><span data-stu-id="f29b3-127">Identify an Azure storage account where hello data will be copied after it is shipped toohello Azure datacenter.</span></span>
3. <span data-ttu-id="f29b3-128">Použití hello [nástroj Azure Import/Export](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), nástroj příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="f29b3-128">Use hello [Azure Import/Export Tool](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), a command-line utility.</span></span> <span data-ttu-id="f29b3-129">Zde je ukázka fragment kódu, který ukazuje, jak toouse hello nástroj.</span><span class="sxs-lookup"><span data-stu-id="f29b3-129">Here's a sample snippet that shows how toouse hello tool.</span></span>

    ````
    WAImportExport PrepImport /sk:<StorageAccountKey> /t: <TargetDriveLetter> /format /encrypt /logdir:e:\myexportimportjob\logdir /j:e:\myexportimportjob\journal1.jrn /id:myexportimportjob /srcdir:F:\demo\ExImContainer /dstdir:importcontainer/vf1/
    ````
    <span data-ttu-id="f29b3-130">V tématu [pomocí služby Azure Import/Export hello](../storage/common/storage-import-export-service.md) pro další fragmenty ukázka.</span><span class="sxs-lookup"><span data-stu-id="f29b3-130">See [Using hello Azure Import/Export service](../storage/common/storage-import-export-service.md) for more sample snippets.</span></span>
4. <span data-ttu-id="f29b3-131">Hello předchozí příkaz vytvoří deník souboru v hello zadané umístění.</span><span class="sxs-lookup"><span data-stu-id="f29b3-131">hello preceding command creates a journal file at hello specified location.</span></span> <span data-ttu-id="f29b3-132">Použít tento deník souboru toocreate úlohy importu z hello [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="f29b3-132">Use this journal file toocreate an import job from hello [Azure classic portal](https://manage.windowsazure.com).</span></span>

## <a name="create-an-import-job"></a><span data-ttu-id="f29b3-133">Vytvoření úlohy importu</span><span class="sxs-lookup"><span data-stu-id="f29b3-133">Create an import job</span></span>
<span data-ttu-id="f29b3-134">Nyní můžete vytvořit úlohy importu podle pokynů hello v [pomocí služby Azure Import/Export hello](../storage/common/storage-import-export-service.md) (v části hello **úlohy importu hello vytvořit** části).</span><span class="sxs-lookup"><span data-stu-id="f29b3-134">You can now create an import job by using hello instructions in [Using hello Azure Import/Export service](../storage/common/storage-import-export-service.md) (under hello **Create hello Import job** section).</span></span> <span data-ttu-id="f29b3-135">Pro tuto úlohu importu s další podrobnosti, také poskytněte soubor deníku hello vytvořili při přípravě hello diskových jednotek.</span><span class="sxs-lookup"><span data-stu-id="f29b3-135">For this import job, with other details, also provide hello journal file created while preparing hello disk drives.</span></span>

## <a name="physically-ship-hello-disks"></a><span data-ttu-id="f29b3-136">Fyzicky dodávat hello disky</span><span class="sxs-lookup"><span data-stu-id="f29b3-136">Physically ship hello disks</span></span>
<span data-ttu-id="f29b3-137">Nyní můžete fyzicky dodávat hello disky tooan datového centra Azure.</span><span class="sxs-lookup"><span data-stu-id="f29b3-137">You can now physically ship hello disks tooan Azure datacenter.</span></span> <span data-ttu-id="f29b3-138">Zde hello data se zkopírují nad objekty toohello úložiště Azure BLOB, které jste zadali při vytváření úlohy importu hello.</span><span class="sxs-lookup"><span data-stu-id="f29b3-138">There, hello data is copied over toohello Azure Storage blobs you provided while creating hello import job.</span></span> <span data-ttu-id="f29b3-139">Navíc při vytváření úlohy hello, pokud jste se rozhodli tooprovide hello později, informace o sledování můžete nyní se vrátit úlohy a aktualizace hello serveru tooyour import sledování Číslo.</span><span class="sxs-lookup"><span data-stu-id="f29b3-139">Also, while creating hello job, if you opted tooprovide hello tracking information later, you can now go back tooyour import job and update hello tracking number.</span></span>

## <a name="copy-data-from-azure-storage-blobs-tooazure-data-lake-store"></a><span data-ttu-id="f29b3-140">Kopírování dat z Azure Storage blobs tooAzure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="f29b3-140">Copy data from Azure Storage blobs tooAzure Data Lake Store</span></span>
<span data-ttu-id="f29b3-141">Po hello stav hello úlohy importu ukazuje, že nebyla dokončena, můžete ověřit, zda jsou k dispozici do objektů BLOB Azure Storage hello, měl zadaná hello data.</span><span class="sxs-lookup"><span data-stu-id="f29b3-141">After hello status of hello import job shows that it's completed, you can verify whether hello data is available in hello Azure Storage blobs you had specified.</span></span> <span data-ttu-id="f29b3-142">Pak můžete použít různé metody toomove, že data z hello objekty BLOB tooAzure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="f29b3-142">You can then use a variety of methods toomove that data from hello blobs tooAzure Data Lake Store.</span></span> <span data-ttu-id="f29b3-143">Všechny hello dostupné možnosti pro nahrávání dat, najdete v části [příjem dat do Data Lake Store](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store).</span><span class="sxs-lookup"><span data-stu-id="f29b3-143">For all hello available options for uploading data, see [Ingesting data into Data Lake Store](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store).</span></span>

<span data-ttu-id="f29b3-144">V této části nám umožňují hello JSON definice používané toocreate kanál služby Azure Data Factory pro kopírování dat.</span><span class="sxs-lookup"><span data-stu-id="f29b3-144">In this section, we provide you with hello JSON definitions that you can use toocreate an Azure Data Factory pipeline for copying data.</span></span> <span data-ttu-id="f29b3-145">Můžete použít tyto definice JSON z hello [portál Azure](../data-factory/data-factory-copy-activity-tutorial-using-azure-portal.md), nebo [Visual Studio](../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md), nebo [prostředí Azure PowerShell](../data-factory/data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="f29b3-145">You can use these JSON definitions from hello [Azure portal](../data-factory/data-factory-copy-activity-tutorial-using-azure-portal.md), or [Visual Studio](../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md), or [Azure PowerShell](../data-factory/data-factory-copy-activity-tutorial-using-powershell.md).</span></span>

### <a name="source-linked-service-azure-storage-blob"></a><span data-ttu-id="f29b3-146">Zdroj propojené služby (objekt blob úložiště Azure)</span><span class="sxs-lookup"><span data-stu-id="f29b3-146">Source linked service (Azure Storage blob)</span></span>
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

### <a name="target-linked-service-azure-data-lake-store"></a><span data-ttu-id="f29b3-147">Cíl propojené služby (Azure Data Lake Store)</span><span class="sxs-lookup"><span data-stu-id="f29b3-147">Target linked service (Azure Data Lake Store)</span></span>
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
### <a name="input-data-set"></a><span data-ttu-id="f29b3-148">Vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="f29b3-148">Input data set</span></span>
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
### <a name="output-data-set"></a><span data-ttu-id="f29b3-149">Výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="f29b3-149">Output data set</span></span>
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
### <a name="pipeline-copy-activity"></a><span data-ttu-id="f29b3-150">Kanál (aktivita kopírování)</span><span class="sxs-lookup"><span data-stu-id="f29b3-150">Pipeline (copy activity)</span></span>
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
<span data-ttu-id="f29b3-151">Další informace najdete v tématu [přesun dat z úložiště Azure blob tooAzure Data Lake Store pomocí Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md).</span><span class="sxs-lookup"><span data-stu-id="f29b3-151">For more information, see [Move data from Azure Storage blob tooAzure Data Lake Store using Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md).</span></span>

## <a name="reconstruct-hello-data-files-in-azure-data-lake-store"></a><span data-ttu-id="f29b3-152">Rekonstrukci hello datových souborů v Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="f29b3-152">Reconstruct hello data files in Azure Data Lake Store</span></span>
<span data-ttu-id="f29b3-153">Spuštění se souborem, který byl 319 GB a něco stalo ho do souborů menší velikost tak, že by mohla být přenesena pomocí služby Azure Import/Export hello.</span><span class="sxs-lookup"><span data-stu-id="f29b3-153">We started with a file that was 319 GB, and broke it down into files of smaller size so that it could be transferred by using hello Azure Import/Export service.</span></span> <span data-ttu-id="f29b3-154">Nyní když hello data v Azure Data Lake Store, jsme rekonstrukci hello souboru tooits původní velikost.</span><span class="sxs-lookup"><span data-stu-id="f29b3-154">Now that hello data is in Azure Data Lake Store, we can reconstruct hello file tooits original size.</span></span> <span data-ttu-id="f29b3-155">Můžete použít následující toodo cmldts prostředí Azure PowerShell tak hello.</span><span class="sxs-lookup"><span data-stu-id="f29b3-155">You can use hello following Azure PowerShell cmldts toodo so.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="f29b3-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f29b3-156">Next steps</span></span>
* [<span data-ttu-id="f29b3-157">Zabezpečení dat ve službě Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="f29b3-157">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="f29b3-158">Použití Azure Data Lake Analytics se službou Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="f29b3-158">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="f29b3-159">Použití Azure HDInsight se službou Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="f29b3-159">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
