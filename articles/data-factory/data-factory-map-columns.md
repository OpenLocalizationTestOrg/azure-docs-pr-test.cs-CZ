---
title: "aaaMapping datovou sadu sloupců v Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak toomap zdrojové sloupce toodestination sloupce."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 8f78d4af675bec0a70e5f6e83ec1ffb511408b5a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="map-source-dataset-columns-toodestination-dataset-columns"></a><span data-ttu-id="6bc2c-103">Mapování zdrojových datovou sadu sloupců toodestination datovou sadu sloupců</span><span class="sxs-lookup"><span data-stu-id="6bc2c-103">Map source dataset columns toodestination dataset columns</span></span>
<span data-ttu-id="6bc2c-104">Mapování sloupce lze použít toospecify jak sloupců zadaných ve zdrojové tabulce mapy toocolumns "struktura" hello zadaný v hello "struktura" tabulky jímky.</span><span class="sxs-lookup"><span data-stu-id="6bc2c-104">Column mapping can be used toospecify how columns specified in hello “structure” of source table map toocolumns specified in hello “structure” of sink table.</span></span> <span data-ttu-id="6bc2c-105">Hello **columnMapping** vlastnost je k dispozici v hello **rámci typeProperties** části hello aktivity kopírování.</span><span class="sxs-lookup"><span data-stu-id="6bc2c-105">hello **columnMapping** property is available in hello **typeProperties** section of hello Copy activity.</span></span>

<span data-ttu-id="6bc2c-106">Mapování sloupců podporuje hello následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="6bc2c-106">Column mapping supports hello following scenarios:</span></span>

* <span data-ttu-id="6bc2c-107">Všechny sloupce ve struktuře datové sady zdroje hello jsou namapované tooall sloupce ve struktuře datové sady podřízený hello.</span><span class="sxs-lookup"><span data-stu-id="6bc2c-107">All columns in hello source dataset structure are mapped tooall columns in hello sink dataset structure.</span></span>
* <span data-ttu-id="6bc2c-108">Podmnožinu sloupců hello ve struktuře datové sady zdroje hello je namapované tooall sloupce ve struktuře datové sady podřízený hello.</span><span class="sxs-lookup"><span data-stu-id="6bc2c-108">A subset of hello columns in hello source dataset structure is mapped tooall columns in hello sink dataset structure.</span></span>

<span data-ttu-id="6bc2c-109">Hello následují chybové stavy, které mít za následek výjimku:</span><span class="sxs-lookup"><span data-stu-id="6bc2c-109">hello following are error conditions that result in an exception:</span></span>

* <span data-ttu-id="6bc2c-110">Buď méně sloupců, nebo v hello "struktura" podřízený tabulky než zadaná v mapování hello je více sloupců.</span><span class="sxs-lookup"><span data-stu-id="6bc2c-110">Either fewer columns or more columns in hello “structure” of sink table than specified in hello mapping.</span></span>
* <span data-ttu-id="6bc2c-111">Duplicitní mapování.</span><span class="sxs-lookup"><span data-stu-id="6bc2c-111">Duplicate mapping.</span></span>
* <span data-ttu-id="6bc2c-112">Výsledek dotazu SQL neobsahuje název sloupce, který je uveden v mapování hello.</span><span class="sxs-lookup"><span data-stu-id="6bc2c-112">SQL query result does not have a column name that is specified in hello mapping.</span></span>

> [!NOTE]
> <span data-ttu-id="6bc2c-113">Hello následující ukázky jsou pro Azure SQL a objektů Blob v Azure, ale jsou příslušné tooany datové úložiště, které podporuje obdélníková datové sady.</span><span class="sxs-lookup"><span data-stu-id="6bc2c-113">hello following samples are for Azure SQL and Azure Blob but are applicable tooany data store that supports rectangular datasets.</span></span> <span data-ttu-id="6bc2c-114">Upravte datovou sadu a propojené služby definice v toodata toopoint příklady ve zdroji dat relevantní hello.</span><span class="sxs-lookup"><span data-stu-id="6bc2c-114">Adjust dataset and linked service definitions in examples toopoint toodata in hello relevant data source.</span></span>

## <a name="sample-1--column-mapping-from-azure-sql-tooazure-blob"></a><span data-ttu-id="6bc2c-115">Ukázka 1 – mapování z objektu blob Azure SQL tooAzure sloupců</span><span class="sxs-lookup"><span data-stu-id="6bc2c-115">Sample 1 – column mapping from Azure SQL tooAzure blob</span></span>
<span data-ttu-id="6bc2c-116">V této ukázce hello vstupní tabulka obsahuje strukturu a odkazuje tooa tabulky SQL v databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="6bc2c-116">In this sample, hello input table has a structure and it points tooa SQL table in an Azure SQL database.</span></span>

```json
{
    "name": "AzureSQLInput",
    "properties": {
        "structure": 
         [
           { "name": "userid"},
           { "name": "name"},
           { "name": "group"}
         ],
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
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

<span data-ttu-id="6bc2c-117">V této ukázce hello výstupní tabulka obsahuje strukturu a odkazuje tooa objektů blob v Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="6bc2c-117">In this sample, hello output table has a structure and it points tooa blob in an Azure blob storage.</span></span>

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
         "structure": 
          [
                { "name": "myuserid"},
                { "name": "myname" },
                { "name": "mygroup"}
          ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder",
            "fileName":"myfile.csv",
            "format":
            {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="6bc2c-118">Hello následující JSON definuje aktivitu kopírování v kanálu.</span><span class="sxs-lookup"><span data-stu-id="6bc2c-118">hello following JSON defines a copy activity in a pipeline.</span></span> <span data-ttu-id="6bc2c-119">Hello sloupců ze zdroje namapované toocolumns sink (**columnMappings**) pomocí hello **překladač** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="6bc2c-119">hello columns from source mapped toocolumns in sink (**columnMappings**) by using hello **Translator** property.</span></span>

```json
{
    "name": "CopyActivity",
    "description": "description", 
    "type": "Copy",
    "inputs":  [ { "name": "AzureSQLInput"  } ],
    "outputs":  [ { "name": "AzureBlobOutput" } ],
    "typeProperties":    {
        "source":
        {
            "type": "SqlSource"
        },
        "sink":
        {
            "type": "BlobSink"
        },
        "translator": 
        {
            "type": "TabularTranslator",
            "ColumnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"
        }
    },
   "scheduler": {
          "frequency": "Hour",
          "interval": 1
        }
}
```
<span data-ttu-id="6bc2c-120">**Tok mapování sloupců:**</span><span class="sxs-lookup"><span data-stu-id="6bc2c-120">**Column mapping flow:**</span></span>

![Sloupec mapování toku](./media/data-factory-map-columns/column-mapping-flow.png)

## <a name="sample-2--column-mapping-with-sql-query-from-azure-sql-tooazure-blob"></a><span data-ttu-id="6bc2c-122">Ukázka 2 – mapování pomocí dotazu SQL z objektu blob Azure SQL tooAzure sloupců</span><span class="sxs-lookup"><span data-stu-id="6bc2c-122">Sample 2 – column mapping with SQL query from Azure SQL tooAzure blob</span></span>
<span data-ttu-id="6bc2c-123">V této ukázce je dotaz SQL použít tooextract dat z Azure SQL místo jednoduše zadání názvu tabulky hello a hello názvy sloupců v části "struktura".</span><span class="sxs-lookup"><span data-stu-id="6bc2c-123">In this sample, a SQL query is used tooextract data from Azure SQL instead of simply specifying hello table name and hello column names in “structure” section.</span></span> 

```json
{
    "name": "CopyActivity",
    "description": "description", 
    "type": "CopyActivity",
    "inputs":  [ { "name": " AzureSQLInput"  } ],
    "outputs":  [ { "name": " AzureBlobOutput" } ],
    "typeProperties":
    {
        "source":
        {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartDateTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
        },
        "sink":
        {
            "type": "BlobSink"
        },
        "Translator": 
        {
            "type": "TabularTranslator",
            "ColumnMappings": "UserId: MyUserId, Group: MyGroup,Name: MyName"
        }
    },
    "scheduler": {
          "frequency": "Hour",
          "interval": 1
        }
}
```
<span data-ttu-id="6bc2c-124">V tomto případě hello výsledky dotazu jsou první namapované toocolumns zadaný v "struktura" zdroje.</span><span class="sxs-lookup"><span data-stu-id="6bc2c-124">In this case, hello query results are first mapped toocolumns specified in “structure” of source.</span></span> <span data-ttu-id="6bc2c-125">V dalším kroku hello sloupce ze zdroje "struktura", jsou namapované toocolumns Sink "struktura" pomocí pravidel zadaná ve vlastnosti columnMappings.</span><span class="sxs-lookup"><span data-stu-id="6bc2c-125">Next, hello columns from source “structure” are mapped toocolumns in sink “structure” with rules specified in columnMappings.</span></span>  <span data-ttu-id="6bc2c-126">Předpokládejme, že hello dotaz vrátí 5 sloupců, než procesory zadané v hello "struktura" zdroje další dva sloupce.</span><span class="sxs-lookup"><span data-stu-id="6bc2c-126">Suppose hello query returns 5 columns, two more columns than those specified in hello “structure” of source.</span></span>

<span data-ttu-id="6bc2c-127">**Sloupec mapování toku**</span><span class="sxs-lookup"><span data-stu-id="6bc2c-127">**Column mapping flow**</span></span>

![Mapování sloupce toku-2](./media/data-factory-map-columns/column-mapping-flow-2.png)

## <a name="next-steps"></a><span data-ttu-id="6bc2c-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6bc2c-129">Next steps</span></span>
<span data-ttu-id="6bc2c-130">Kurz týkající se použití aktivitě kopírování najdete v článku hello:</span><span class="sxs-lookup"><span data-stu-id="6bc2c-130">See hello article for a tutorial on using Copy Activity:</span></span> 

- [<span data-ttu-id="6bc2c-131">Kopírování dat z úložiště objektů Blob tooSQL databáze</span><span class="sxs-lookup"><span data-stu-id="6bc2c-131">Copy data from Blob Storage tooSQL Database</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
