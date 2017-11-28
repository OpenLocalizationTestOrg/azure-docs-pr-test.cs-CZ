---
redirect_url: /azure/sql-data-warehouse/sql-data-warehouse-load-with-data-factory
title: aaaLoad dat z Azure blob storage do Azure SQL Data Warehouse (Azure Data Factory) | Microsoft Docs
description: "Další informace tooload dat pomocí Azure Data Factory"
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse
ms.assetid: 689fb02e-eb98-4f7c-81e6-6c1d22d53901
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 11/22/2016
ms.author: barbkess
ms.custom: loading
ms.openlocfilehash: 29a220679a11cedefb0dfd06c0a6838f81a90447
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-azure-blob-storage-into-azure-sql-data-warehouse-azure-data-factory"></a><span data-ttu-id="05f32-103">Načtení dat z Azure Blob Storage do Azure SQL Data Warehouse (Azure Data Factory)</span><span class="sxs-lookup"><span data-stu-id="05f32-103">Load data from Azure blob storage into Azure SQL Data Warehouse (Azure Data Factory)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="05f32-104">Data Factory</span><span class="sxs-lookup"><span data-stu-id="05f32-104">Data Factory</span></span>](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
> * [<span data-ttu-id="05f32-105">PolyBase</span><span class="sxs-lookup"><span data-stu-id="05f32-105">PolyBase</span></span>](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)
> 
> 

 <span data-ttu-id="05f32-106">Tento kurz ukazuje, jak toocreate kanál v Azure Data Factory toomove dat z Azure Storage Blob tooSQL datového skladu.</span><span class="sxs-lookup"><span data-stu-id="05f32-106">This tutorial shows you how toocreate a pipeline in Azure Data Factory toomove data from Azure Storage Blob tooSQL Data Warehouse.</span></span> <span data-ttu-id="05f32-107">Hello následujících kroků provedete následující:</span><span class="sxs-lookup"><span data-stu-id="05f32-107">With hello following steps you will:</span></span>

* <span data-ttu-id="05f32-108">Nastavení ukázkových dat v objektu blob služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="05f32-108">Set-up sample data in an Azure Storage Blob.</span></span>
* <span data-ttu-id="05f32-109">Připojte prostředky tooAzure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="05f32-109">Connect resources tooAzure Data Factory.</span></span>
* <span data-ttu-id="05f32-110">Vytvoření kanálu toomove dat z úložiště objektů BLOB tooSQL datového skladu.</span><span class="sxs-lookup"><span data-stu-id="05f32-110">Create a pipeline toomove data from Storage Blobs tooSQL Data Warehouse.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-Azure-SQL-Data-Warehouse-with-Azure-Data-Factory/player]
> 
> 

## <a name="before-you-begin"></a><span data-ttu-id="05f32-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="05f32-111">Before you begin</span></span>
<span data-ttu-id="05f32-112">toofamiliarize sami s Azure Data Factory najdete v části [Úvod tooAzure Data Factory][Introduction tooAzure Data Factory].</span><span class="sxs-lookup"><span data-stu-id="05f32-112">toofamiliarize yourself with Azure Data Factory, see [Introduction tooAzure Data Factory][Introduction tooAzure Data Factory].</span></span>

### <a name="create-or-identify-resources"></a><span data-ttu-id="05f32-113">Vytvoření nebo určení prostředků</span><span class="sxs-lookup"><span data-stu-id="05f32-113">Create or identify resources</span></span>
<span data-ttu-id="05f32-114">Před zahájením tohoto kurzu, je nutné toohave hello následující prostředky.</span><span class="sxs-lookup"><span data-stu-id="05f32-114">Before starting this tutorial, you need toohave hello following resources.</span></span>

* <span data-ttu-id="05f32-115">**Azure Storage Blob**: v tomto kurzu se používá Azure Storage Blob jako zdroj dat hello pro kanál Azure Data Factory hello, a proto je třeba toohave jeden k dispozici toostore hello ukázková data.</span><span class="sxs-lookup"><span data-stu-id="05f32-115">**Azure Storage Blob**: This tutorial uses Azure Storage Blob as hello data source for hello Azure Data Factory pipeline, and so you need toohave one available toostore hello sample data.</span></span> <span data-ttu-id="05f32-116">Pokud nemáte již, zjistěte, jak příliš[vytvořit účet úložiště][Create a storage account].</span><span class="sxs-lookup"><span data-stu-id="05f32-116">If you don't have one already, learn how too[Create a storage account][Create a storage account].</span></span>
* <span data-ttu-id="05f32-117">**SQL Data Warehouse**: Tento kurz přesune hello data z Azure Storage Blob příliš SQL Data Warehouse a tak musí toohave online datový sklad, který je načtena s hello ukázková data AdventureWorksDW.</span><span class="sxs-lookup"><span data-stu-id="05f32-117">**SQL Data Warehouse**: This tutorial moves hello data from Azure Storage Blob too SQL Data Warehouse and so need toohave a data warehouse online that is loaded with hello AdventureWorksDW sample data.</span></span> <span data-ttu-id="05f32-118">Pokud již datový sklad nemáte, zjistěte, jak příliš[zřídit][Create a SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="05f32-118">If you do not already have a data warehouse, learn how too[provision one][Create a SQL Data Warehouse].</span></span> <span data-ttu-id="05f32-119">Pokud máte datový sklad, ale nemáte v něm hello ukázková data, můžete [ručně načíst][Load sample data into SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="05f32-119">If you have a data warehouse but didn't provision it with hello sample data, you can [load it manually][Load sample data into SQL Data Warehouse].</span></span>
* <span data-ttu-id="05f32-120">**Azure Data Factory**: Azure Data Factory dokončí vlastní operaci načtení hello a proto je třeba toohave jeden, které můžete použít toobuild hello data přesun kanálu. Pokud nemáte již, zjistěte, jak toocreate jednu v kroku 1 z [Začínáme s Azure Data Factory (Data Factory Editor)][Get started with Azure Data Factory (Data Factory Editor)].</span><span class="sxs-lookup"><span data-stu-id="05f32-120">**Azure Data Factory**: Azure Data Factory will complete hello actual load and so you need toohave one that you can use toobuild hello data movement pipeline.If you don't have one already, learn how toocreate one in Step 1 of [Get started with Azure Data Factory (Data Factory Editor)][Get started with Azure Data Factory (Data Factory Editor)].</span></span>
* <span data-ttu-id="05f32-121">**AZCopy**: je třeba AZCopy toocopy hello ukázková data z vašeho místního klienta tooyour Azure Storage Blob.</span><span class="sxs-lookup"><span data-stu-id="05f32-121">**AZCopy**: You need AZCopy toocopy hello sample data from your local client tooyour Azure Storage Blob.</span></span> <span data-ttu-id="05f32-122">Postup instalace najdete v části hello [dokumentaci k AZCopy][AZCopy documentation].</span><span class="sxs-lookup"><span data-stu-id="05f32-122">For install instructions, see hello [AZCopy documentation][AZCopy documentation].</span></span>

## <a name="step-1-copy-sample-data-tooazure-storage-blob"></a><span data-ttu-id="05f32-123">Krok 1: Kopírování ukázkových dat tooAzure objektu Blob Storage</span><span class="sxs-lookup"><span data-stu-id="05f32-123">Step 1: Copy sample data tooAzure Storage Blob</span></span>
<span data-ttu-id="05f32-124">Jakmile budete mít vše připraveno hello, jste připravené toocopy ukázková data tooyour Azure Storage Blob.</span><span class="sxs-lookup"><span data-stu-id="05f32-124">Once you have all of hello pieces ready, you are ready toocopy sample data tooyour Azure Storage Blob.</span></span>

1. <span data-ttu-id="05f32-125">[Stáhněte si ukázková data][Download sample data].</span><span class="sxs-lookup"><span data-stu-id="05f32-125">[Download sample data][Download sample data].</span></span> <span data-ttu-id="05f32-126">Tato data přidají další tři roky prodejních dat tooyour ukázková data AdventureWorksDW.</span><span class="sxs-lookup"><span data-stu-id="05f32-126">This data will add another three years of sales data tooyour AdventureWorksDW sample data.</span></span>
2. <span data-ttu-id="05f32-127">Pomocí této AZCopy příkaz toocopy hello tři roky dat tooyour Azure Storage Blob.</span><span class="sxs-lookup"><span data-stu-id="05f32-127">Use this AZCopy command toocopy hello three years of data tooyour Azure Storage Blob.</span></span>

````
AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
````


## <a name="step-2-connect-resources-tooazure-data-factory"></a><span data-ttu-id="05f32-128">Krok 2: Připojení prostředků tooAzure objekt pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="05f32-128">Step 2: Connect resources tooAzure Data Factory</span></span>
<span data-ttu-id="05f32-129">Teď, když hello data je na místě můžeme vytvořit hello Azure Data Factory kanálu toomove hello dat z Azure blob storage do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="05f32-129">Now that hello data is in place we can create hello Azure Data Factory pipeline toomove hello data from Azure blob storage into SQL Data Warehouse.</span></span>

<span data-ttu-id="05f32-130">zahájení tooget, otevřete hello [portál Azure] [ Azure portal] a z nabídky hello levé straně vyberete datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="05f32-130">tooget started, open hello [Azure portal][Azure portal] and select your data factory from hello left-hand menu.</span></span>

### <a name="step-21-create-linked-service"></a><span data-ttu-id="05f32-131">Krok 2.1: Vytvoření propojené služby</span><span class="sxs-lookup"><span data-stu-id="05f32-131">Step 2.1: Create Linked Service</span></span>
<span data-ttu-id="05f32-132">Propojte svůj účet úložiště Azure a SQL Data Warehouse tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="05f32-132">Link your Azure storage account and SQL Data Warehouse tooyour data factory.</span></span>  

1. <span data-ttu-id="05f32-133">Nejprve nejprve hello registraci klepnutím na oddíl "Propojené služby" hello svojí datové továrny a potom klikněte na "Nové datové úložiště."</span><span class="sxs-lookup"><span data-stu-id="05f32-133">First, begin hello registration process by clicking hello 'Linked Services' section of your data factory and then click 'New data store.'</span></span> <span data-ttu-id="05f32-134">Zvolte název tooregister úložiště azure v části jako typ vyberte úložiště Azure a potom zadejte název účtu a klíč účtu.</span><span class="sxs-lookup"><span data-stu-id="05f32-134">Choose a name tooregister your azure storage under, select Azure Storage as your type, and then enter your Account Name and Account Key.</span></span>
2. <span data-ttu-id="05f32-135">tooregister SQL Data Warehouse přejděte toohello, vytvořit a nasadit, část, vyberte úložišti nový a pak, Azure SQL Data Warehouse'.</span><span class="sxs-lookup"><span data-stu-id="05f32-135">tooregister SQL Data Warehouse navigate toohello 'Author and Deploy' section, select 'New Data Store', and then 'Azure SQL Data Warehouse'.</span></span> <span data-ttu-id="05f32-136">Zkopírujte a vložte tuto šablonu a potom vyplňte konkrétní informace.</span><span class="sxs-lookup"><span data-stu-id="05f32-136">Copy and paste in this template, and then fill in your specific information.</span></span>

```JSON
{
    "name": "<Linked Service Name>",
    "properties": {
        "description": "",
        "type": "AzureSqlDW",
        "typeProperties": {
             "connectionString": "Data Source=tcp:<server name>.database.windows.net,1433;Initial Catalog=<server name>;Integrated Security=False;User ID=<user>@<servername>;Password=<password>;Connect Timeout=30;Encrypt=True"
         }
    }
}
```

### <a name="step-22-define-hello-dataset"></a><span data-ttu-id="05f32-137">Krok 2.2: Definování datové sady hello</span><span class="sxs-lookup"><span data-stu-id="05f32-137">Step 2.2: Define hello dataset</span></span>
<span data-ttu-id="05f32-138">Po vytvoření hello propojené služby, máme toodefine hello datových sad.</span><span class="sxs-lookup"><span data-stu-id="05f32-138">After creating hello linked services, we will have toodefine hello data sets.</span></span>  <span data-ttu-id="05f32-139">Tady to znamená definovat strukturu hello hello dat přesouvaných z vašeho úložiště tooyour datového skladu.</span><span class="sxs-lookup"><span data-stu-id="05f32-139">Here this means defining hello structure of hello data that is being moved from your storage tooyour data warehouse.</span></span>  <span data-ttu-id="05f32-140">O vytváření si toho můžete přečíst víc.</span><span class="sxs-lookup"><span data-stu-id="05f32-140">You can read more about creating</span></span>

1. <span data-ttu-id="05f32-141">Tento proces začněte tak, že přejdete toohello, vytvořit a nasadit' část vaší služby data factory.</span><span class="sxs-lookup"><span data-stu-id="05f32-141">Start this process by navigating toohello 'Author and Deploy' section of your data factory.</span></span>
2. <span data-ttu-id="05f32-142">Klikněte na nová datová sada a pak toolink 'Azure Blob storage, úložiště tooyour datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="05f32-142">Click 'New dataset' and then 'Azure Blob storage' toolink your storage tooyour data factory.</span></span>  <span data-ttu-id="05f32-143">Vaše data hello níže toodefine skriptu můžete použít v Azure Blob storage:</span><span class="sxs-lookup"><span data-stu-id="05f32-143">You can use hello below script toodefine your data in Azure Blob storage:</span></span>

```JSON
{
    "name": "<Dataset Name>",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "<linked storage name>",
        "typeProperties": {
            "folderPath": "<containter name>",
            "fileName": "FactInternetSales.csv",
            "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": "\n"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```


1. <span data-ttu-id="05f32-144">Teď si také definujeme datovou sadu pro SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="05f32-144">Now we will also define our dataset for SQL Data Warehouse.</span></span>  <span data-ttu-id="05f32-145">Začneme v hello stejným způsobem, kliknutím na nová datová sada a, Azure SQL Data Warehouse'.</span><span class="sxs-lookup"><span data-stu-id="05f32-145">We start in hello same way, by clicking 'New dataset' and then 'Azure SQL Data Warehouse'.</span></span>

```JSON
{
    "name": "DWDataset",
    "properties": {
        "type": "AzureSqlDWTable",
        "linkedServiceName": "AzureSqlDWLinkedService",
        "typeProperties": {
            "tableName": "FactInternetSales"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

## <a name="step-3-create-and-run-your-pipeline"></a><span data-ttu-id="05f32-146">Krok 3: Vytvoření a spuštění kanálu</span><span class="sxs-lookup"><span data-stu-id="05f32-146">Step 3: Create and run your pipeline</span></span>
<span data-ttu-id="05f32-147">Nakonec provedeme nastavení a spuštění kanálu hello v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="05f32-147">Finally, we will set-up and run hello pipeline in Azure Data Factory.</span></span>  <span data-ttu-id="05f32-148">Toto je hello operace provede hello pohybů skutečná data.</span><span class="sxs-lookup"><span data-stu-id="05f32-148">This is hello operation that will complete hello actual data movement.</span></span>  <span data-ttu-id="05f32-149">Úplný přehled operací hello, které můžete s SQL Data Warehouse a Azure Data Factory můžete najít [sem][Move data tooand from Azure SQL Data Warehouse using Azure Data Factory].</span><span class="sxs-lookup"><span data-stu-id="05f32-149">You can find a full view of hello operations that you can complete with SQL Data Warehouse and Azure Data Factory [here][Move data tooand from Azure SQL Data Warehouse using Azure Data Factory].</span></span>

<span data-ttu-id="05f32-150">V části "Vytvořit a nasadit" hello nyní klikněte na další příkazy a pak "Nový kanál".</span><span class="sxs-lookup"><span data-stu-id="05f32-150">In hello 'Author and Deploy' section now click 'More Commands' and then 'New Pipeline'.</span></span>  <span data-ttu-id="05f32-151">Po vytvoření kanálu hello, můžete použít hello níže kód tootransfer hello data tooyour datového skladu:</span><span class="sxs-lookup"><span data-stu-id="05f32-151">After you create hello pipeline, you can use hello below code tootransfer hello data tooyour data warehouse:</span></span>

```JSON
{
    "name": "<Pipeline Name>",
    "properties": {
        "description": "<Description>",
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "skipHeaderLineCount": 1
                },
                "sink": {
                    "type": "SqlDWSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:10"
                }
            },
            "inputs": [
              {
                "name": "<Storage Dataset>"
              }
            ],
            "outputs": [
              {
                "name": "<Data Warehouse Dataset>"
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
            "name": "Sample Copy",
            "description": "Copy Activity"
          }
        ],
        "start": "<Date YYYY-MM-DD>",
        "end": "<Date YYYY-MM-DD>",
        "isPaused": false
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="05f32-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="05f32-152">Next steps</span></span>
<span data-ttu-id="05f32-153">toolearn více, spusťte zobrazení:</span><span class="sxs-lookup"><span data-stu-id="05f32-153">toolearn more, start by viewing:</span></span>

* <span data-ttu-id="05f32-154">[Postup výuky pro službu Azure Data Factory][Azure Data Factory learning path]</span><span class="sxs-lookup"><span data-stu-id="05f32-154">[Azure Data Factory learning path][Azure Data Factory learning path].</span></span>
* <span data-ttu-id="05f32-155">[Konektor služby Azure SQL Data Warehouse][Azure SQL Data Warehouse Connector]</span><span class="sxs-lookup"><span data-stu-id="05f32-155">[Azure SQL Data Warehouse Connector][Azure SQL Data Warehouse Connector].</span></span> <span data-ttu-id="05f32-156">Toto je hello základní referenční téma pro Azure SQL Data Warehouse pomocí Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="05f32-156">This is hello core reference topic for using Azure Data Factory with Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="05f32-157">Tato témata obsahují podrobné informace o službě Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="05f32-157">These topics provide detailed information about Azure Data Factory.</span></span> <span data-ttu-id="05f32-158">Jsou věnovaná službám Azure SQL Database nebo HDinsight, ale hello informace platí také tooAzure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="05f32-158">They discuss Azure SQL Database or HDinsight, but hello information also applies tooAzure SQL Data Warehouse.</span></span>

* <span data-ttu-id="05f32-159">[Kurz: Začínáme s Azure Data Factory] [ Tutorial: Get started with Azure Data Factory] jde hello základní kurz pro zpracování dat pomocí Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="05f32-159">[Tutorial: Get started with Azure Data Factory][Tutorial: Get started with Azure Data Factory] This is hello core tutorial for processing data with Azure Data Factory.</span></span> <span data-ttu-id="05f32-160">V tomto kurzu se sestavit svůj první kanál, který používá HDInsight tootransform a analýze webových protokolů měsíčně.</span><span class="sxs-lookup"><span data-stu-id="05f32-160">In this tutorial you will build your first pipeline that uses HDInsight tootransform and analyze web logs on a monthly basis.</span></span> <span data-ttu-id="05f32-161">Upozorňujeme, že tento kurz neobsahuje žádné aktivity kopírování.</span><span class="sxs-lookup"><span data-stu-id="05f32-161">Note, there is no copy activity in this tutorial.</span></span>
* <span data-ttu-id="05f32-162">[Kurz: Kopírování dat z Azure Storage Blob tooAzure SQL Database][Tutorial: Copy data from Azure Storage Blob tooAzure SQL Database].</span><span class="sxs-lookup"><span data-stu-id="05f32-162">[Tutorial: Copy data from Azure Storage Blob tooAzure SQL Database][Tutorial: Copy data from Azure Storage Blob tooAzure SQL Database].</span></span> <span data-ttu-id="05f32-163">V tomto kurzu vytvoříte kanál v Azure Data Factory toocopy dat z Azure Storage Blob tooAzure databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="05f32-163">In this tutorial, you will create a pipeline in Azure Data Factory toocopy data from Azure Storage Blob tooAzure SQL Database.</span></span>

<!--Image references-->

<!--Article references-->
[AZCopy documentation]: ../storage/storage-use-azcopy.md
[Azure SQL Data Warehouse Connector]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[BCP]: sql-data-warehouse-load-with-bcp.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Create a storage account]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Data Factory]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Get started with Azure Data Factory (Data Factory Editor)]: ../data-factory/data-factory-build-your-first-pipeline-using-editor.md
[Introduction tooAzure Data Factory]: ../data-factory/data-factory-introduction.md
[Load sample data into SQL Data Warehouse]: sql-data-warehouse-load-sample-databases.md
[Move data tooand from Azure SQL Data Warehouse using Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md
[Tutorial: Copy data from Azure Storage Blob tooAzure SQL Database]: ../data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[Tutorial: Get started with Azure Data Factory]: ../data-factory/data-factory-build-your-first-pipeline.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory learning path]: https://azure.microsoft.com/documentation/learning-paths/data-factory
[Azure portal]: https://portal.azure.com
[Download sample data]: https://migrhoststorage.blob.core.windows.net/adfsample/FactInternetSales.csv
