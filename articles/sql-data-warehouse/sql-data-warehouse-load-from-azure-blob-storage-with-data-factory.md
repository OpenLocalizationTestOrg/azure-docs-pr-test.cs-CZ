---
redirect_url: /azure/sql-data-warehouse/sql-data-warehouse-load-with-data-factory
title: "Načtení dat z Azure blob storage do Azure SQL Data Warehouse (Azure Data Factory) | Microsoft Docs"
description: "Naučte se načítat data pomocí Azure Data Factory."
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
ms.openlocfilehash: ca8bdfc21582253e8709a33eb624547fed4461d6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="load-data-from-azure-blob-storage-into-azure-sql-data-warehouse-azure-data-factory"></a><span data-ttu-id="5bf26-103">Načtení dat z Azure Blob Storage do Azure SQL Data Warehouse (Azure Data Factory)</span><span class="sxs-lookup"><span data-stu-id="5bf26-103">Load data from Azure blob storage into Azure SQL Data Warehouse (Azure Data Factory)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5bf26-104">Data Factory</span><span class="sxs-lookup"><span data-stu-id="5bf26-104">Data Factory</span></span>](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
> * [<span data-ttu-id="5bf26-105">PolyBase</span><span class="sxs-lookup"><span data-stu-id="5bf26-105">PolyBase</span></span>](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)
> 
> 

 <span data-ttu-id="5bf26-106">Tento návod ukazuje, jak v Azure Data Factory vytvořit kanál pro přesun dat z úložiště objektů blob Azure do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5bf26-106">This tutorial shows you how to create a pipeline in Azure Data Factory to move data from Azure Storage Blob to SQL Data Warehouse.</span></span> <span data-ttu-id="5bf26-107">V následujících krocích provedete toto:</span><span class="sxs-lookup"><span data-stu-id="5bf26-107">With the following steps you will:</span></span>

* <span data-ttu-id="5bf26-108">Nastavení ukázkových dat v objektu blob služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="5bf26-108">Set-up sample data in an Azure Storage Blob.</span></span>
* <span data-ttu-id="5bf26-109">Připojení prostředků k Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="5bf26-109">Connect resources to Azure Data Factory.</span></span>
* <span data-ttu-id="5bf26-110">Vytvoření kanálu pro přesun dat z úložiště objektů blob do SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="5bf26-110">Create a pipeline to move data from Storage Blobs to SQL Data Warehouse.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-Azure-SQL-Data-Warehouse-with-Azure-Data-Factory/player]
> 
> 

## <a name="before-you-begin"></a><span data-ttu-id="5bf26-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="5bf26-111">Before you begin</span></span>
<span data-ttu-id="5bf26-112">Seznamte se s Azure Data Factory. Potřebné informace najdete v tématu [Úvod do Azure Data Factory][Introduction to Azure Data Factory].</span><span class="sxs-lookup"><span data-stu-id="5bf26-112">To familiarize yourself with Azure Data Factory, see [Introduction to Azure Data Factory][Introduction to Azure Data Factory].</span></span>

### <a name="create-or-identify-resources"></a><span data-ttu-id="5bf26-113">Vytvoření nebo určení prostředků</span><span class="sxs-lookup"><span data-stu-id="5bf26-113">Create or identify resources</span></span>
<span data-ttu-id="5bf26-114">Před zahájením tohoto kurzu musíte mít následující prostředky.</span><span class="sxs-lookup"><span data-stu-id="5bf26-114">Before starting this tutorial, you need to have the following resources.</span></span>

* <span data-ttu-id="5bf26-115">**Objekt Blob služby Azure Storage:** V tomto kurzu se používá objekt Blob služby Azure Storage jako zdroj dat pro kanál Azure Data Factory, takže byste měli mít nějaký k dispozici pro uložení ukázkových dat.</span><span class="sxs-lookup"><span data-stu-id="5bf26-115">**Azure Storage Blob**: This tutorial uses Azure Storage Blob as the data source for the Azure Data Factory pipeline, and so you need to have one available to store the sample data.</span></span> <span data-ttu-id="5bf26-116">Pokud ho ještě nemáte, naučte se [vytvořit účet úložiště][Create a storage account].</span><span class="sxs-lookup"><span data-stu-id="5bf26-116">If you don't have one already, learn how to [Create a storage account][Create a storage account].</span></span>
* <span data-ttu-id="5bf26-117">**SQL Data Warehouse:** V tomto kurzu se přesouvají data z objektu blob úložiště Azure do SQL Data Warehouse. Je proto nutné, abyste měli online datový sklad, ve kterém jsou načtená ukázková data AdventureWorksDW.</span><span class="sxs-lookup"><span data-stu-id="5bf26-117">**SQL Data Warehouse**: This tutorial moves the data from Azure Storage Blob to  SQL Data Warehouse and so need to have a data warehouse online that is loaded with the AdventureWorksDW sample data.</span></span> <span data-ttu-id="5bf26-118">Pokud ještě datový sklad nemáte, přečtěte si, jak si ho [zřídit][Create a SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="5bf26-118">If you do not already have a data warehouse, learn how to [provision one][Create a SQL Data Warehouse].</span></span> <span data-ttu-id="5bf26-119">Pokud sice máte datový sklad, ale nemáte v něm ukázková data, můžete je do něj [načíst ručně][Load sample data into SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="5bf26-119">If you have a data warehouse but didn't provision it with the sample data, you can [load it manually][Load sample data into SQL Data Warehouse].</span></span>
* <span data-ttu-id="5bf26-120">**Azure Data Factory**: Azure Data Factory dokončí vlastní operaci načtení a proto je nutné mít ji, můžete použít k vytvoření kanálu přesun dat. Pokud nemáte již, zjistěte, jak vytvořit v kroku 1 tématu [Začínáme s Azure Data Factory (Data Factory Editor)][Get started with Azure Data Factory (Data Factory Editor)].</span><span class="sxs-lookup"><span data-stu-id="5bf26-120">**Azure Data Factory**: Azure Data Factory will complete the actual load and so you need to have one that you can use to build the data movement pipeline.If you don't have one already, learn how to create one in Step 1 of [Get started with Azure Data Factory (Data Factory Editor)][Get started with Azure Data Factory (Data Factory Editor)].</span></span>
* <span data-ttu-id="5bf26-121">**AZCopy**: AZCopy potřebujete ke zkopírování ukázkových dat z místního klienta do objektu blob služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="5bf26-121">**AZCopy**: You need AZCopy to copy the sample data from your local client to your Azure Storage Blob.</span></span> <span data-ttu-id="5bf26-122">Postup instalace najdete v [dokumentaci k AZCopy][AZCopy documentation].</span><span class="sxs-lookup"><span data-stu-id="5bf26-122">For install instructions, see the [AZCopy documentation][AZCopy documentation].</span></span>

## <a name="step-1-copy-sample-data-to-azure-storage-blob"></a><span data-ttu-id="5bf26-123">Krok 1: Kopírování ukázkových dat do objektu blob služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="5bf26-123">Step 1: Copy sample data to Azure Storage Blob</span></span>
<span data-ttu-id="5bf26-124">Jakmile budete mít vše připraveno, můžete zkopírovat ukázková data do objektu blob služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="5bf26-124">Once you have all of the pieces ready, you are ready to copy sample data to your Azure Storage Blob.</span></span>

1. <span data-ttu-id="5bf26-125">[Stáhněte si ukázková data][Download sample data].</span><span class="sxs-lookup"><span data-stu-id="5bf26-125">[Download sample data][Download sample data].</span></span> <span data-ttu-id="5bf26-126">Tato data přidají další tři roky prodejních dat do ukázkových dat AdventureWorksDW.</span><span class="sxs-lookup"><span data-stu-id="5bf26-126">This data will add another three years of sales data to your AdventureWorksDW sample data.</span></span>
2. <span data-ttu-id="5bf26-127">Tímto příkazem AZCopy zkopírujte tři roky dat do objektu blob služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="5bf26-127">Use this AZCopy command to copy the three years of data to your Azure Storage Blob.</span></span>

````
AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
````


## <a name="step-2-connect-resources-to-azure-data-factory"></a><span data-ttu-id="5bf26-128">Krok 2: Připojení prostředků k Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="5bf26-128">Step 2: Connect resources to Azure Data Factory</span></span>
<span data-ttu-id="5bf26-129">Teď, když jsou data tam, kde mají být, můžeme vytvořit kanál Azure Data Factory pro přesun dat z Azure Blob Storage do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5bf26-129">Now that the data is in place we can create the Azure Data Factory pipeline to move the data from Azure blob storage into SQL Data Warehouse.</span></span>

<span data-ttu-id="5bf26-130">Začněte tím, že otevřete [Azure Portal][Azure portal] a z nabídky na levé straně vyberete datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="5bf26-130">To get started, open the [Azure portal][Azure portal] and select your data factory from the left-hand menu.</span></span>

### <a name="step-21-create-linked-service"></a><span data-ttu-id="5bf26-131">Krok 2.1: Vytvoření propojené služby</span><span class="sxs-lookup"><span data-stu-id="5bf26-131">Step 2.1: Create Linked Service</span></span>
<span data-ttu-id="5bf26-132">Propojte si účet úložiště Azure a SQL Data Warehouse se svojí datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="5bf26-132">Link your Azure storage account and SQL Data Warehouse to your data factory.</span></span>  

1. <span data-ttu-id="5bf26-133">Začněte proces registrace kliknutím na část Propojené služby svojí datové továrny a potom klikněte na Nové datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="5bf26-133">First, begin the registration process by clicking the 'Linked Services' section of your data factory and then click 'New data store.'</span></span> <span data-ttu-id="5bf26-134">Zvolte název, pod kterým chcete úložiště Azure zaregistrovat, jako typ vyberte Úložiště Azure a pak zadejte název a klíč účtu.</span><span class="sxs-lookup"><span data-stu-id="5bf26-134">Choose a name to register your azure storage under, select Azure Storage as your type, and then enter your Account Name and Account Key.</span></span>
2. <span data-ttu-id="5bf26-135">SQL Data Warehouse zaregistrujete tak, že přejdete do části Vytvořit a nasadit, vyberete možnost Nové datové úložiště a pak vyberete Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5bf26-135">To register SQL Data Warehouse navigate to the 'Author and Deploy' section, select 'New Data Store', and then 'Azure SQL Data Warehouse'.</span></span> <span data-ttu-id="5bf26-136">Zkopírujte a vložte tuto šablonu a potom vyplňte konkrétní informace.</span><span class="sxs-lookup"><span data-stu-id="5bf26-136">Copy and paste in this template, and then fill in your specific information.</span></span>

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

### <a name="step-22-define-the-dataset"></a><span data-ttu-id="5bf26-137">Krok 2.2: Definování datové sady</span><span class="sxs-lookup"><span data-stu-id="5bf26-137">Step 2.2: Define the dataset</span></span>
<span data-ttu-id="5bf26-138">Po vytvoření propojených služeb budeme muset definovat datové sady.</span><span class="sxs-lookup"><span data-stu-id="5bf26-138">After creating the linked services, we will have to define the data sets.</span></span>  <span data-ttu-id="5bf26-139">Tady to znamená definovat strukturu dat přesouvaných z vašeho úložiště datového skladu.</span><span class="sxs-lookup"><span data-stu-id="5bf26-139">Here this means defining the structure of the data that is being moved from your storage to your data warehouse.</span></span>  <span data-ttu-id="5bf26-140">O vytváření si toho můžete přečíst víc.</span><span class="sxs-lookup"><span data-stu-id="5bf26-140">You can read more about creating</span></span>

1. <span data-ttu-id="5bf26-141">Tento proces začněte tím, že v datové továrně přejdete do části Vytvořit a nasadit.</span><span class="sxs-lookup"><span data-stu-id="5bf26-141">Start this process by navigating to the 'Author and Deploy' section of your data factory.</span></span>
2. <span data-ttu-id="5bf26-142">Klikněte na Nová datová sada a potom na Azure Blob Storage. Tím si svoje úložiště připojíte k datové továrně.</span><span class="sxs-lookup"><span data-stu-id="5bf26-142">Click 'New dataset' and then 'Azure Blob storage' to link your storage to your data factory.</span></span>  <span data-ttu-id="5bf26-143">K definování svých dat v Azure Blob Storage můžete použít níže uvedený skript:</span><span class="sxs-lookup"><span data-stu-id="5bf26-143">You can use the below script to define your data in Azure Blob storage:</span></span>

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


1. <span data-ttu-id="5bf26-144">Teď si také definujeme datovou sadu pro SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5bf26-144">Now we will also define our dataset for SQL Data Warehouse.</span></span>  <span data-ttu-id="5bf26-145">Začneme stejným způsobem – kliknutím na Nová datová sada a pak na Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5bf26-145">We start in the same way, by clicking 'New dataset' and then 'Azure SQL Data Warehouse'.</span></span>

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

## <a name="step-3-create-and-run-your-pipeline"></a><span data-ttu-id="5bf26-146">Krok 3: Vytvoření a spuštění kanálu</span><span class="sxs-lookup"><span data-stu-id="5bf26-146">Step 3: Create and run your pipeline</span></span>
<span data-ttu-id="5bf26-147">Nakonec v Azure Data Factory nastavíme a spustíme kanál.</span><span class="sxs-lookup"><span data-stu-id="5bf26-147">Finally, we will set-up and run the pipeline in Azure Data Factory.</span></span>  <span data-ttu-id="5bf26-148">Právě tato operace provede vlastní přesun dat.</span><span class="sxs-lookup"><span data-stu-id="5bf26-148">This is the operation that will complete the actual data movement.</span></span>  <span data-ttu-id="5bf26-149">Úplný přehled operací, které můžete s SQL Data Warehouse a Azure Data Factory provádět, najdete [tady][Move data to and from Azure SQL Data Warehouse using Azure Data Factory].</span><span class="sxs-lookup"><span data-stu-id="5bf26-149">You can find a full view of the operations that you can complete with SQL Data Warehouse and Azure Data Factory [here][Move data to and from Azure SQL Data Warehouse using Azure Data Factory].</span></span>

<span data-ttu-id="5bf26-150">V části Vytvořit a nasadit klikněte na Další příkazy a pak klikněte na Nový kanál.</span><span class="sxs-lookup"><span data-stu-id="5bf26-150">In the 'Author and Deploy' section now click 'More Commands' and then 'New Pipeline'.</span></span>  <span data-ttu-id="5bf26-151">Po vytvoření kanálu můžete pomocí níže uvedeného kódu přenést data do datového skladu:</span><span class="sxs-lookup"><span data-stu-id="5bf26-151">After you create the pipeline, you can use the below code to transfer the data to your data warehouse:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="5bf26-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5bf26-152">Next steps</span></span>
<span data-ttu-id="5bf26-153">Pokud se chcete dozvědět víc, začněte těmito zdroji informací:</span><span class="sxs-lookup"><span data-stu-id="5bf26-153">To learn more, start by viewing:</span></span>

* <span data-ttu-id="5bf26-154">[Postup výuky pro službu Azure Data Factory][Azure Data Factory learning path]</span><span class="sxs-lookup"><span data-stu-id="5bf26-154">[Azure Data Factory learning path][Azure Data Factory learning path].</span></span>
* <span data-ttu-id="5bf26-155">[Konektor služby Azure SQL Data Warehouse][Azure SQL Data Warehouse Connector]</span><span class="sxs-lookup"><span data-stu-id="5bf26-155">[Azure SQL Data Warehouse Connector][Azure SQL Data Warehouse Connector].</span></span> <span data-ttu-id="5bf26-156">Toto je základní referenční téma pro používání služby Azure Data Factory se službou Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5bf26-156">This is the core reference topic for using Azure Data Factory with Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="5bf26-157">Tato témata obsahují podrobné informace o službě Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="5bf26-157">These topics provide detailed information about Azure Data Factory.</span></span> <span data-ttu-id="5bf26-158">Jsou věnovaná službám Azure SQL Database nebo HDinsight, ale informace v nich platí také pro Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5bf26-158">They discuss Azure SQL Database or HDinsight, but the information also applies to Azure SQL Data Warehouse.</span></span>

* <span data-ttu-id="5bf26-159">[Kurz: Začínáme s Azure Data Factory][Tutorial: Get started with Azure Data Factory]. Jde o základní kurz pro zpracování dat pomocí služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="5bf26-159">[Tutorial: Get started with Azure Data Factory][Tutorial: Get started with Azure Data Factory] This is the core tutorial for processing data with Azure Data Factory.</span></span> <span data-ttu-id="5bf26-160">V tomto kurzu vytvoříte svůj první kanál, který využívá HDInsight k transformaci a měsíční analýze webových protokolů.</span><span class="sxs-lookup"><span data-stu-id="5bf26-160">In this tutorial you will build your first pipeline that uses HDInsight to transform and analyze web logs on a monthly basis.</span></span> <span data-ttu-id="5bf26-161">Upozorňujeme, že tento kurz neobsahuje žádné aktivity kopírování.</span><span class="sxs-lookup"><span data-stu-id="5bf26-161">Note, there is no copy activity in this tutorial.</span></span>
* <span data-ttu-id="5bf26-162">[Kurz: Kopírování dat z Azure Storage Blob do služby Azure SQL Database][Tutorial: Copy data from Azure Storage Blob to Azure SQL Database].</span><span class="sxs-lookup"><span data-stu-id="5bf26-162">[Tutorial: Copy data from Azure Storage Blob to Azure SQL Database][Tutorial: Copy data from Azure Storage Blob to Azure SQL Database].</span></span> <span data-ttu-id="5bf26-163">V tomto kurzu vytvoříte kanál v Azure Data Factory ke zkopírování dat z Azure Blob Storage do Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="5bf26-163">In this tutorial, you will create a pipeline in Azure Data Factory to copy data from Azure Storage Blob to Azure SQL Database.</span></span>

<!--Image references-->

<!--Article references-->
[AZCopy documentation]: ../storage/storage-use-azcopy.md
[Azure SQL Data Warehouse Connector]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[BCP]: sql-data-warehouse-load-with-bcp.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Create a storage account]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Data Factory]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Get started with Azure Data Factory (Data Factory Editor)]: ../data-factory/data-factory-build-your-first-pipeline-using-editor.md
[Introduction to Azure Data Factory]: ../data-factory/data-factory-introduction.md
[Load sample data into SQL Data Warehouse]: sql-data-warehouse-load-sample-databases.md
[Move data to and from Azure SQL Data Warehouse using Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md
[Tutorial: Copy data from Azure Storage Blob to Azure SQL Database]: ../data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[Tutorial: Get started with Azure Data Factory]: ../data-factory/data-factory-build-your-first-pipeline.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory learning path]: https://azure.microsoft.com/documentation/learning-paths/data-factory
[Azure portal]: https://portal.azure.com
[Download sample data]: https://migrhoststorage.blob.core.windows.net/adfsample/FactInternetSales.csv
