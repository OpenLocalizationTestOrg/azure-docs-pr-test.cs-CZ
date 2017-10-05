---
title: "Nahrát velké objemy dat do Data Lake Store pomocí offline metod | Microsoft Docs"
description: "Použijte nástroj AdlCopy ke zkopírování dat z Azure úložiště objektů BLOB do Data Lake Store"
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
ms.openlocfilehash: b469c0ebe9838a1ea986cff3043e3008941e9aa9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-azure-importexport-service-for-offline-copy-of-data-to-data-lake-store"></a><span data-ttu-id="fd6e6-103">Používat službu Azure Import/Export pro offline kopii dat do Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="fd6e6-103">Use the Azure Import/Export service for offline copy of data to Data Lake Store</span></span>
<span data-ttu-id="fd6e6-104">V tomto článku se dozvíte kopírování obrovských sad dat (> 200 GB) do Azure Data Lake Store pomocí metod kopii offline, jako je třeba [služba Azure Import/Export](../storage/common/storage-import-export-service.md).</span><span class="sxs-lookup"><span data-stu-id="fd6e6-104">In this article, you'll learn how to copy huge data sets (>200 GB) into an Azure Data Lake Store by using offline copy methods, like the [Azure Import/Export service](../storage/common/storage-import-export-service.md).</span></span> <span data-ttu-id="fd6e6-105">Soubor používá jako příklad v tomto článku je konkrétně 339,420,860,416 bajtů nebo přibližně 319 GB na disku.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-105">Specifically, the file used as an example in this article is 339,420,860,416 bytes, or about 319 GB on disk.</span></span> <span data-ttu-id="fd6e6-106">Umožňuje volání 319GB.tsv tento soubor.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-106">Let's call this file 319GB.tsv.</span></span>

<span data-ttu-id="fd6e6-107">Služba Azure Import/Export vám umožní bezpečněji přenos velkých objemů dat do úložiště objektů Blob v Azure pomocí přesouvání pevných disků o datovém centru Azure.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-107">The Azure Import/Export service helps you to transfer large amounts of data more securely to Azure Blob storage by shipping hard disk drives to an Azure datacenter.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fd6e6-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fd6e6-108">Prerequisites</span></span>
<span data-ttu-id="fd6e6-109">Než začnete, musíte mít následující:</span><span class="sxs-lookup"><span data-stu-id="fd6e6-109">Before you begin, you must have the following:</span></span>

* <span data-ttu-id="fd6e6-110">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-110">**An Azure subscription**.</span></span> <span data-ttu-id="fd6e6-111">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fd6e6-111">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="fd6e6-112">**Účet úložiště Azure**.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-112">**An Azure storage account**.</span></span>
* <span data-ttu-id="fd6e6-113">**Účet Azure Data Lake Store**.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-113">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="fd6e6-114">Pokyny o tom, jak vytvořit najdete v tématu [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="fd6e6-114">For instructions on how to create one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>

## <a name="preparing-the-data"></a><span data-ttu-id="fd6e6-115">Příprava dat</span><span class="sxs-lookup"><span data-stu-id="fd6e6-115">Preparing the data</span></span>
<span data-ttu-id="fd6e6-116">Než začnete používat službu Import/Export, rozdělit datového souboru k přesunu **do kopie, které jsou menší než 200 GB** velikost.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-116">Before using the Import/Export service, break the data file to be transferred **into copies that are less than 200 GB** in size.</span></span> <span data-ttu-id="fd6e6-117">Nástroj pro import nefunguje s soubory větší než 200 GB.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-117">The import tool does not work with files greater than 200 GB.</span></span> <span data-ttu-id="fd6e6-118">V tomto kurzu jsme rozdělit do bloků, 100 GB.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-118">In this tutorial, we split the file into chunks of 100 GB each.</span></span> <span data-ttu-id="fd6e6-119">Můžete to provést pomocí [emulaci](https://cygwin.com/install.html).</span><span class="sxs-lookup"><span data-stu-id="fd6e6-119">You can do this by using [Cygwin](https://cygwin.com/install.html).</span></span> <span data-ttu-id="fd6e6-120">Emulaci podporuje příkazy Linux.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-120">Cygwin supports Linux commands.</span></span> <span data-ttu-id="fd6e6-121">V takovém případě použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fd6e6-121">In this case, use the following command:</span></span>

    split -b 100m 319GB.tsv

<span data-ttu-id="fd6e6-122">Operace rozdělení vytvoří soubory s následujícími názvy.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-122">The split operation creates files with the following names.</span></span>

    319GB.tsv-part-aa

    319GB.tsv-part-ab

    319GB.tsv-part-ac

    319GB.tsv-part-ad

## <a name="get-disks-ready-with-data"></a><span data-ttu-id="fd6e6-123">Příprava disky s daty</span><span class="sxs-lookup"><span data-stu-id="fd6e6-123">Get disks ready with data</span></span>
<span data-ttu-id="fd6e6-124">Postupujte podle pokynů v [pomocí služby Azure Import/Export](../storage/common/storage-import-export-service.md) (v části **Příprava jednotky** část) k přípravě pevné disky.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-124">Follow the instructions in [Using the Azure Import/Export service](../storage/common/storage-import-export-service.md) (under the **Prepare your drives** section) to prepare your hard drives.</span></span> <span data-ttu-id="fd6e6-125">Tady je celkový pořadí:</span><span class="sxs-lookup"><span data-stu-id="fd6e6-125">Here's the overall sequence:</span></span>

1. <span data-ttu-id="fd6e6-126">Pořídit pevný disk, který splňuje požadavek na který se má použít pro službu Azure Import/Export.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-126">Procure a hard disk that meets the requirement to be used for the Azure Import/Export service.</span></span>
2. <span data-ttu-id="fd6e6-127">Určete účet úložiště Azure, kde data se zkopírují po je dodáván na datovém centru Azure.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-127">Identify an Azure storage account where the data will be copied after it is shipped to the Azure datacenter.</span></span>
3. <span data-ttu-id="fd6e6-128">Použití [nástroj Azure Import/Export](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), nástroj příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-128">Use the [Azure Import/Export Tool](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), a command-line utility.</span></span> <span data-ttu-id="fd6e6-129">Zde je ukázka fragment kódu, který ukazuje, jak používat nástroj.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-129">Here's a sample snippet that shows how to use the tool.</span></span>

    ````
    WAImportExport PrepImport /sk:<StorageAccountKey> /t: <TargetDriveLetter> /format /encrypt /logdir:e:\myexportimportjob\logdir /j:e:\myexportimportjob\journal1.jrn /id:myexportimportjob /srcdir:F:\demo\ExImContainer /dstdir:importcontainer/vf1/
    ````
    <span data-ttu-id="fd6e6-130">V tématu [pomocí služby Azure Import/Export](../storage/common/storage-import-export-service.md) pro další fragmenty ukázka.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-130">See [Using the Azure Import/Export service](../storage/common/storage-import-export-service.md) for more sample snippets.</span></span>
4. <span data-ttu-id="fd6e6-131">Předchozí příkaz vytvoří soubor deníku do zadaného umístění.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-131">The preceding command creates a journal file at the specified location.</span></span> <span data-ttu-id="fd6e6-132">Tento soubor deníku použít k vytvoření úlohy importu z [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="fd6e6-132">Use this journal file to create an import job from the [Azure classic portal](https://manage.windowsazure.com).</span></span>

## <a name="create-an-import-job"></a><span data-ttu-id="fd6e6-133">Vytvoření úlohy importu</span><span class="sxs-lookup"><span data-stu-id="fd6e6-133">Create an import job</span></span>
<span data-ttu-id="fd6e6-134">Nyní můžete vytvořit úlohy importu pomocí pokynů uvedených v [pomocí služby Azure Import/Export](../storage/common/storage-import-export-service.md) (v části **vytvořit úlohy importu** části).</span><span class="sxs-lookup"><span data-stu-id="fd6e6-134">You can now create an import job by using the instructions in [Using the Azure Import/Export service](../storage/common/storage-import-export-service.md) (under the **Create the Import job** section).</span></span> <span data-ttu-id="fd6e6-135">Pro tuto úlohu importu s další podrobnosti, také poskytněte soubor deníku vytvořili při přípravě na pevných discích.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-135">For this import job, with other details, also provide the journal file created while preparing the disk drives.</span></span>

## <a name="physically-ship-the-disks"></a><span data-ttu-id="fd6e6-136">Fyzicky dodávat disky</span><span class="sxs-lookup"><span data-stu-id="fd6e6-136">Physically ship the disks</span></span>
<span data-ttu-id="fd6e6-137">Nyní můžete fyzicky dodávat disky datového centra Azure.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-137">You can now physically ship the disks to an Azure datacenter.</span></span> <span data-ttu-id="fd6e6-138">Existuje ale data se zkopírují přes Azure úložiště objektů BLOB, které jste zadali při vytváření úlohy importu.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-138">There, the data is copied over to the Azure Storage blobs you provided while creating the import job.</span></span> <span data-ttu-id="fd6e6-139">Navíc při vytváření úlohy, když jste se rozhodli poskytnout informace o sledování později, můžete nyní přejděte zpět na vaše úlohy importu a aktualizovat číslo sledování.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-139">Also, while creating the job, if you opted to provide the tracking information later, you can now go back to your import job and update the tracking number.</span></span>

## <a name="copy-data-from-azure-storage-blobs-to-azure-data-lake-store"></a><span data-ttu-id="fd6e6-140">Kopírování dat z objektů BLOB služby Azure Storage do Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="fd6e6-140">Copy data from Azure Storage blobs to Azure Data Lake Store</span></span>
<span data-ttu-id="fd6e6-141">Po ve stavu úlohy importu zobrazuje, zda je dokončena, můžete ověřit, zda je k dispozici do objektů BLOB Azure Storage, měl zadaná data.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-141">After the status of the import job shows that it's completed, you can verify whether the data is available in the Azure Storage blobs you had specified.</span></span> <span data-ttu-id="fd6e6-142">Potom můžete různé metody pro přesun dat z objektů BLOB do Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-142">You can then use a variety of methods to move that data from the blobs to Azure Data Lake Store.</span></span> <span data-ttu-id="fd6e6-143">Všechny dostupné možnosti, pro nahrávání dat najdete v tématu [příjem dat do Data Lake Store](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store).</span><span class="sxs-lookup"><span data-stu-id="fd6e6-143">For all the available options for uploading data, see [Ingesting data into Data Lake Store](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store).</span></span>

<span data-ttu-id="fd6e6-144">V této části nám umožňují JSON definice, které můžete použít k vytvoření kanál služby Azure Data Factory pro kopírování dat.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-144">In this section, we provide you with the JSON definitions that you can use to create an Azure Data Factory pipeline for copying data.</span></span> <span data-ttu-id="fd6e6-145">Můžete použít tyto definice JSON z [portál Azure](../data-factory/data-factory-copy-activity-tutorial-using-azure-portal.md), nebo [Visual Studio](../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md), nebo [prostředí Azure PowerShell](../data-factory/data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="fd6e6-145">You can use these JSON definitions from the [Azure portal](../data-factory/data-factory-copy-activity-tutorial-using-azure-portal.md), or [Visual Studio](../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md), or [Azure PowerShell](../data-factory/data-factory-copy-activity-tutorial-using-powershell.md).</span></span>

### <a name="source-linked-service-azure-storage-blob"></a><span data-ttu-id="fd6e6-146">Zdroj propojené služby (objekt blob úložiště Azure)</span><span class="sxs-lookup"><span data-stu-id="fd6e6-146">Source linked service (Azure Storage blob)</span></span>
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

### <a name="target-linked-service-azure-data-lake-store"></a><span data-ttu-id="fd6e6-147">Cíl propojené služby (Azure Data Lake Store)</span><span class="sxs-lookup"><span data-stu-id="fd6e6-147">Target linked service (Azure Data Lake Store)</span></span>
````
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "description": "",
        "typeProperties": {
            "authorization": "<Click 'Authorize' to allow this data factory and the activities it runs to access this Data Lake Store with your access rights>",
            "dataLakeStoreUri": "https://<adls_account_name>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<OAuth session id from the OAuth authorization session. Each session id is unique and may only be used once>"
        }
    }
}
````
### <a name="input-data-set"></a><span data-ttu-id="fd6e6-148">Vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="fd6e6-148">Input data set</span></span>
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
### <a name="output-data-set"></a><span data-ttu-id="fd6e6-149">Výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="fd6e6-149">Output data set</span></span>
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
### <a name="pipeline-copy-activity"></a><span data-ttu-id="fd6e6-150">Kanál (aktivita kopírování)</span><span class="sxs-lookup"><span data-stu-id="fd6e6-150">Pipeline (copy activity)</span></span>
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
<span data-ttu-id="fd6e6-151">Další informace najdete v tématu [přesun dat z objektu blob úložiště Azure do Azure Data Lake Store pomocí Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md).</span><span class="sxs-lookup"><span data-stu-id="fd6e6-151">For more information, see [Move data from Azure Storage blob to Azure Data Lake Store using Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md).</span></span>

## <a name="reconstruct-the-data-files-in-azure-data-lake-store"></a><span data-ttu-id="fd6e6-152">Rekonstrukci datových souborů v Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="fd6e6-152">Reconstruct the data files in Azure Data Lake Store</span></span>
<span data-ttu-id="fd6e6-153">Spuštění se souborem, který byl 319 GB a překročila ho dolů do souborů menší velikost tak, že by mohla být přenesena pomocí služby Azure Import/Export.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-153">We started with a file that was 319 GB, and broke it down into files of smaller size so that it could be transferred by using the Azure Import/Export service.</span></span> <span data-ttu-id="fd6e6-154">Teď, když se data v Azure Data Lake Store, jsme rekonstrukci soubor původní velikost.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-154">Now that the data is in Azure Data Lake Store, we can reconstruct the file to its original size.</span></span> <span data-ttu-id="fd6e6-155">K tomu můžete použít následující cmldts prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fd6e6-155">You can use the following Azure PowerShell cmldts to do so.</span></span>

````
# Login to our account
Login-AzureRmAccount

# List your subscriptions
Get-AzureRmSubscription

# Switch to the subscription you want to work with
Set-AzureRmContext –SubscriptionId
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

# Join  the files
Join-AzureRmDataLakeStoreItem -AccountName "<adls_account_name" -Paths "/importeddatafeb8job/319GB.tsv-part-aa","/importeddatafeb8job/319GB.tsv-part-ab", "/importeddatafeb8job/319GB.tsv-part-ac", "/importeddatafeb8job/319GB.tsv-part-ad" -Destination "/importeddatafeb8job/MergedFile.csv”
````

## <a name="next-steps"></a><span data-ttu-id="fd6e6-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fd6e6-156">Next steps</span></span>
* [<span data-ttu-id="fd6e6-157">Zabezpečení dat ve službě Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="fd6e6-157">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="fd6e6-158">Použití Azure Data Lake Analytics se službou Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="fd6e6-158">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="fd6e6-159">Použití Azure HDInsight se službou Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="fd6e6-159">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
