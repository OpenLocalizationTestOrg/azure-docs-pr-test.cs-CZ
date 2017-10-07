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
# <a name="map-source-dataset-columns-toodestination-dataset-columns"></a>Mapování zdrojových datovou sadu sloupců toodestination datovou sadu sloupců
Mapování sloupce lze použít toospecify jak sloupců zadaných ve zdrojové tabulce mapy toocolumns "struktura" hello zadaný v hello "struktura" tabulky jímky. Hello **columnMapping** vlastnost je k dispozici v hello **rámci typeProperties** části hello aktivity kopírování.

Mapování sloupců podporuje hello následující scénáře:

* Všechny sloupce ve struktuře datové sady zdroje hello jsou namapované tooall sloupce ve struktuře datové sady podřízený hello.
* Podmnožinu sloupců hello ve struktuře datové sady zdroje hello je namapované tooall sloupce ve struktuře datové sady podřízený hello.

Hello následují chybové stavy, které mít za následek výjimku:

* Buď méně sloupců, nebo v hello "struktura" podřízený tabulky než zadaná v mapování hello je více sloupců.
* Duplicitní mapování.
* Výsledek dotazu SQL neobsahuje název sloupce, který je uveden v mapování hello.

> [!NOTE]
> Hello následující ukázky jsou pro Azure SQL a objektů Blob v Azure, ale jsou příslušné tooany datové úložiště, které podporuje obdélníková datové sady. Upravte datovou sadu a propojené služby definice v toodata toopoint příklady ve zdroji dat relevantní hello.

## <a name="sample-1--column-mapping-from-azure-sql-tooazure-blob"></a>Ukázka 1 – mapování z objektu blob Azure SQL tooAzure sloupců
V této ukázce hello vstupní tabulka obsahuje strukturu a odkazuje tooa tabulky SQL v databázi Azure SQL.

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

V této ukázce hello výstupní tabulka obsahuje strukturu a odkazuje tooa objektů blob v Azure blob storage.

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

Hello následující JSON definuje aktivitu kopírování v kanálu. Hello sloupců ze zdroje namapované toocolumns sink (**columnMappings**) pomocí hello **překladač** vlastnost.

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
**Tok mapování sloupců:**

![Sloupec mapování toku](./media/data-factory-map-columns/column-mapping-flow.png)

## <a name="sample-2--column-mapping-with-sql-query-from-azure-sql-tooazure-blob"></a>Ukázka 2 – mapování pomocí dotazu SQL z objektu blob Azure SQL tooAzure sloupců
V této ukázce je dotaz SQL použít tooextract dat z Azure SQL místo jednoduše zadání názvu tabulky hello a hello názvy sloupců v části "struktura". 

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
V tomto případě hello výsledky dotazu jsou první namapované toocolumns zadaný v "struktura" zdroje. V dalším kroku hello sloupce ze zdroje "struktura", jsou namapované toocolumns Sink "struktura" pomocí pravidel zadaná ve vlastnosti columnMappings.  Předpokládejme, že hello dotaz vrátí 5 sloupců, než procesory zadané v hello "struktura" zdroje další dva sloupce.

**Sloupec mapování toku**

![Mapování sloupce toku-2](./media/data-factory-map-columns/column-mapping-flow-2.png)

## <a name="next-steps"></a>Další kroky
Kurz týkající se použití aktivitě kopírování najdete v článku hello: 

- [Kopírování dat z úložiště objektů Blob tooSQL databáze](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
