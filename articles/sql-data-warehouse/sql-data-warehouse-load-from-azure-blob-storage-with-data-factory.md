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
# <a name="load-data-from-azure-blob-storage-into-azure-sql-data-warehouse-azure-data-factory"></a>Načtení dat z Azure Blob Storage do Azure SQL Data Warehouse (Azure Data Factory)
> [!div class="op_single_selector"]
> * [Data Factory](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
> * [PolyBase](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)
> 
> 

 Tento návod ukazuje, jak v Azure Data Factory vytvořit kanál pro přesun dat z úložiště objektů blob Azure do SQL Data Warehouse. V následujících krocích provedete toto:

* Nastavení ukázkových dat v objektu blob služby Azure Storage
* Připojení prostředků k Azure Data Factory
* Vytvoření kanálu pro přesun dat z úložiště objektů blob do SQL Data Warehouse

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-Azure-SQL-Data-Warehouse-with-Azure-Data-Factory/player]
> 
> 

## <a name="before-you-begin"></a>Než začnete
Seznamte se s Azure Data Factory. Potřebné informace najdete v tématu [Úvod do Azure Data Factory][Introduction to Azure Data Factory].

### <a name="create-or-identify-resources"></a>Vytvoření nebo určení prostředků
Před zahájením tohoto kurzu musíte mít následující prostředky.

* **Objekt Blob služby Azure Storage:** V tomto kurzu se používá objekt Blob služby Azure Storage jako zdroj dat pro kanál Azure Data Factory, takže byste měli mít nějaký k dispozici pro uložení ukázkových dat. Pokud ho ještě nemáte, naučte se [vytvořit účet úložiště][Create a storage account].
* **SQL Data Warehouse:** V tomto kurzu se přesouvají data z objektu blob úložiště Azure do SQL Data Warehouse. Je proto nutné, abyste měli online datový sklad, ve kterém jsou načtená ukázková data AdventureWorksDW. Pokud ještě datový sklad nemáte, přečtěte si, jak si ho [zřídit][Create a SQL Data Warehouse]. Pokud sice máte datový sklad, ale nemáte v něm ukázková data, můžete je do něj [načíst ručně][Load sample data into SQL Data Warehouse].
* **Azure Data Factory**: Azure Data Factory dokončí vlastní operaci načtení a proto je nutné mít ji, můžete použít k vytvoření kanálu přesun dat. Pokud nemáte již, zjistěte, jak vytvořit v kroku 1 tématu [Začínáme s Azure Data Factory (Data Factory Editor)][Get started with Azure Data Factory (Data Factory Editor)].
* **AZCopy**: AZCopy potřebujete ke zkopírování ukázkových dat z místního klienta do objektu blob služby Azure Storage. Postup instalace najdete v [dokumentaci k AZCopy][AZCopy documentation].

## <a name="step-1-copy-sample-data-to-azure-storage-blob"></a>Krok 1: Kopírování ukázkových dat do objektu blob služby Azure Storage
Jakmile budete mít vše připraveno, můžete zkopírovat ukázková data do objektu blob služby Azure Storage.

1. [Stáhněte si ukázková data][Download sample data]. Tato data přidají další tři roky prodejních dat do ukázkových dat AdventureWorksDW.
2. Tímto příkazem AZCopy zkopírujte tři roky dat do objektu blob služby Azure Storage.

````
AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
````


## <a name="step-2-connect-resources-to-azure-data-factory"></a>Krok 2: Připojení prostředků k Azure Data Factory
Teď, když jsou data tam, kde mají být, můžeme vytvořit kanál Azure Data Factory pro přesun dat z Azure Blob Storage do SQL Data Warehouse.

Začněte tím, že otevřete [Azure Portal][Azure portal] a z nabídky na levé straně vyberete datovou továrnu.

### <a name="step-21-create-linked-service"></a>Krok 2.1: Vytvoření propojené služby
Propojte si účet úložiště Azure a SQL Data Warehouse se svojí datovou továrnou.  

1. Začněte proces registrace kliknutím na část Propojené služby svojí datové továrny a potom klikněte na Nové datové úložiště. Zvolte název, pod kterým chcete úložiště Azure zaregistrovat, jako typ vyberte Úložiště Azure a pak zadejte název a klíč účtu.
2. SQL Data Warehouse zaregistrujete tak, že přejdete do části Vytvořit a nasadit, vyberete možnost Nové datové úložiště a pak vyberete Azure SQL Data Warehouse. Zkopírujte a vložte tuto šablonu a potom vyplňte konkrétní informace.

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

### <a name="step-22-define-the-dataset"></a>Krok 2.2: Definování datové sady
Po vytvoření propojených služeb budeme muset definovat datové sady.  Tady to znamená definovat strukturu dat přesouvaných z vašeho úložiště datového skladu.  O vytváření si toho můžete přečíst víc.

1. Tento proces začněte tím, že v datové továrně přejdete do části Vytvořit a nasadit.
2. Klikněte na Nová datová sada a potom na Azure Blob Storage. Tím si svoje úložiště připojíte k datové továrně.  K definování svých dat v Azure Blob Storage můžete použít níže uvedený skript:

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


1. Teď si také definujeme datovou sadu pro SQL Data Warehouse.  Začneme stejným způsobem – kliknutím na Nová datová sada a pak na Azure SQL Data Warehouse.

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

## <a name="step-3-create-and-run-your-pipeline"></a>Krok 3: Vytvoření a spuštění kanálu
Nakonec v Azure Data Factory nastavíme a spustíme kanál.  Právě tato operace provede vlastní přesun dat.  Úplný přehled operací, které můžete s SQL Data Warehouse a Azure Data Factory provádět, najdete [tady][Move data to and from Azure SQL Data Warehouse using Azure Data Factory].

V části Vytvořit a nasadit klikněte na Další příkazy a pak klikněte na Nový kanál.  Po vytvoření kanálu můžete pomocí níže uvedeného kódu přenést data do datového skladu:

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

## <a name="next-steps"></a>Další kroky
Pokud se chcete dozvědět víc, začněte těmito zdroji informací:

* [Postup výuky pro službu Azure Data Factory][Azure Data Factory learning path]
* [Konektor služby Azure SQL Data Warehouse][Azure SQL Data Warehouse Connector] Toto je základní referenční téma pro používání služby Azure Data Factory se službou Azure SQL Data Warehouse.

Tato témata obsahují podrobné informace o službě Azure Data Factory. Jsou věnovaná službám Azure SQL Database nebo HDinsight, ale informace v nich platí také pro Azure SQL Data Warehouse.

* [Kurz: Začínáme s Azure Data Factory][Tutorial: Get started with Azure Data Factory]. Jde o základní kurz pro zpracování dat pomocí služby Azure Data Factory. V tomto kurzu vytvoříte svůj první kanál, který využívá HDInsight k transformaci a měsíční analýze webových protokolů. Upozorňujeme, že tento kurz neobsahuje žádné aktivity kopírování.
* [Kurz: Kopírování dat z Azure Storage Blob do služby Azure SQL Database][Tutorial: Copy data from Azure Storage Blob to Azure SQL Database]. V tomto kurzu vytvoříte kanál v Azure Data Factory ke zkopírování dat z Azure Blob Storage do Azure SQL Database.

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
