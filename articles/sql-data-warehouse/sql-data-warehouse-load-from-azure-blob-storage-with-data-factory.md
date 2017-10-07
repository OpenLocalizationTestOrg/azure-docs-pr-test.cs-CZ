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
# <a name="load-data-from-azure-blob-storage-into-azure-sql-data-warehouse-azure-data-factory"></a>Načtení dat z Azure Blob Storage do Azure SQL Data Warehouse (Azure Data Factory)
> [!div class="op_single_selector"]
> * [Data Factory](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
> * [PolyBase](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)
> 
> 

 Tento kurz ukazuje, jak toocreate kanál v Azure Data Factory toomove dat z Azure Storage Blob tooSQL datového skladu. Hello následujících kroků provedete následující:

* Nastavení ukázkových dat v objektu blob služby Azure Storage
* Připojte prostředky tooAzure Data Factory.
* Vytvoření kanálu toomove dat z úložiště objektů BLOB tooSQL datového skladu.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-Azure-SQL-Data-Warehouse-with-Azure-Data-Factory/player]
> 
> 

## <a name="before-you-begin"></a>Než začnete
toofamiliarize sami s Azure Data Factory najdete v části [Úvod tooAzure Data Factory][Introduction tooAzure Data Factory].

### <a name="create-or-identify-resources"></a>Vytvoření nebo určení prostředků
Před zahájením tohoto kurzu, je nutné toohave hello následující prostředky.

* **Azure Storage Blob**: v tomto kurzu se používá Azure Storage Blob jako zdroj dat hello pro kanál Azure Data Factory hello, a proto je třeba toohave jeden k dispozici toostore hello ukázková data. Pokud nemáte již, zjistěte, jak příliš[vytvořit účet úložiště][Create a storage account].
* **SQL Data Warehouse**: Tento kurz přesune hello data z Azure Storage Blob příliš SQL Data Warehouse a tak musí toohave online datový sklad, který je načtena s hello ukázková data AdventureWorksDW. Pokud již datový sklad nemáte, zjistěte, jak příliš[zřídit][Create a SQL Data Warehouse]. Pokud máte datový sklad, ale nemáte v něm hello ukázková data, můžete [ručně načíst][Load sample data into SQL Data Warehouse].
* **Azure Data Factory**: Azure Data Factory dokončí vlastní operaci načtení hello a proto je třeba toohave jeden, které můžete použít toobuild hello data přesun kanálu. Pokud nemáte již, zjistěte, jak toocreate jednu v kroku 1 z [Začínáme s Azure Data Factory (Data Factory Editor)][Get started with Azure Data Factory (Data Factory Editor)].
* **AZCopy**: je třeba AZCopy toocopy hello ukázková data z vašeho místního klienta tooyour Azure Storage Blob. Postup instalace najdete v části hello [dokumentaci k AZCopy][AZCopy documentation].

## <a name="step-1-copy-sample-data-tooazure-storage-blob"></a>Krok 1: Kopírování ukázkových dat tooAzure objektu Blob Storage
Jakmile budete mít vše připraveno hello, jste připravené toocopy ukázková data tooyour Azure Storage Blob.

1. [Stáhněte si ukázková data][Download sample data]. Tato data přidají další tři roky prodejních dat tooyour ukázková data AdventureWorksDW.
2. Pomocí této AZCopy příkaz toocopy hello tři roky dat tooyour Azure Storage Blob.

````
AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
````


## <a name="step-2-connect-resources-tooazure-data-factory"></a>Krok 2: Připojení prostředků tooAzure objekt pro vytváření dat
Teď, když hello data je na místě můžeme vytvořit hello Azure Data Factory kanálu toomove hello dat z Azure blob storage do SQL Data Warehouse.

zahájení tooget, otevřete hello [portál Azure] [ Azure portal] a z nabídky hello levé straně vyberete datovou továrnu.

### <a name="step-21-create-linked-service"></a>Krok 2.1: Vytvoření propojené služby
Propojte svůj účet úložiště Azure a SQL Data Warehouse tooyour data factory.  

1. Nejprve nejprve hello registraci klepnutím na oddíl "Propojené služby" hello svojí datové továrny a potom klikněte na "Nové datové úložiště." Zvolte název tooregister úložiště azure v části jako typ vyberte úložiště Azure a potom zadejte název účtu a klíč účtu.
2. tooregister SQL Data Warehouse přejděte toohello, vytvořit a nasadit, část, vyberte úložišti nový a pak, Azure SQL Data Warehouse'. Zkopírujte a vložte tuto šablonu a potom vyplňte konkrétní informace.

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

### <a name="step-22-define-hello-dataset"></a>Krok 2.2: Definování datové sady hello
Po vytvoření hello propojené služby, máme toodefine hello datových sad.  Tady to znamená definovat strukturu hello hello dat přesouvaných z vašeho úložiště tooyour datového skladu.  O vytváření si toho můžete přečíst víc.

1. Tento proces začněte tak, že přejdete toohello, vytvořit a nasadit' část vaší služby data factory.
2. Klikněte na nová datová sada a pak toolink 'Azure Blob storage, úložiště tooyour datovou továrnu.  Vaše data hello níže toodefine skriptu můžete použít v Azure Blob storage:

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


1. Teď si také definujeme datovou sadu pro SQL Data Warehouse.  Začneme v hello stejným způsobem, kliknutím na nová datová sada a, Azure SQL Data Warehouse'.

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
Nakonec provedeme nastavení a spuštění kanálu hello v Azure Data Factory.  Toto je hello operace provede hello pohybů skutečná data.  Úplný přehled operací hello, které můžete s SQL Data Warehouse a Azure Data Factory můžete najít [sem][Move data tooand from Azure SQL Data Warehouse using Azure Data Factory].

V části "Vytvořit a nasadit" hello nyní klikněte na další příkazy a pak "Nový kanál".  Po vytvoření kanálu hello, můžete použít hello níže kód tootransfer hello data tooyour datového skladu:

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
toolearn více, spusťte zobrazení:

* [Postup výuky pro službu Azure Data Factory][Azure Data Factory learning path]
* [Konektor služby Azure SQL Data Warehouse][Azure SQL Data Warehouse Connector] Toto je hello základní referenční téma pro Azure SQL Data Warehouse pomocí Azure Data Factory.

Tato témata obsahují podrobné informace o službě Azure Data Factory. Jsou věnovaná službám Azure SQL Database nebo HDinsight, ale hello informace platí také tooAzure SQL Data Warehouse.

* [Kurz: Začínáme s Azure Data Factory] [ Tutorial: Get started with Azure Data Factory] jde hello základní kurz pro zpracování dat pomocí Azure Data Factory. V tomto kurzu se sestavit svůj první kanál, který používá HDInsight tootransform a analýze webových protokolů měsíčně. Upozorňujeme, že tento kurz neobsahuje žádné aktivity kopírování.
* [Kurz: Kopírování dat z Azure Storage Blob tooAzure SQL Database][Tutorial: Copy data from Azure Storage Blob tooAzure SQL Database]. V tomto kurzu vytvoříte kanál v Azure Data Factory toocopy dat z Azure Storage Blob tooAzure databáze SQL.

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
