---
title: "aaaInvoke uložené procedury z kopie aktivita služby Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak aktivita kopírování tooinvoke uloženou proceduru v Azure SQL Database nebo SQL Server z objektu pro vytváření dat Azure."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 986377118afb8c08607c2325fcc3ab00b3de9268
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="invoke-stored-procedure-from-copy-activity-in-azure-data-factory"></a>Vyvolat uloženou proceduru z aktivity kopírování v Azure Data Factory
Při kopírování dat do [systému SQL Server](data-factory-sqlserver-connector.md) nebo [Azure SQL Database](data-factory-azure-sql-connector.md), můžete nakonfigurovat hello **SqlSink** v tooinvoke kopie aktivity uložené procedury. Můžete chtít toouse hello uložené procedury tooperform žádné další zpracování (slučování sloupců, vyhledávání hodnot, vložení do více tabulek, atd.) je nutné před vložením dat v toohello cílové tabulky. Tato funkce využívá [zavolat parametry](https://msdn.microsoft.com/library/bb675163.aspx). 

Následující ukázka Hello ukazuje, jak tooinvoke uloženou proceduru v systému SQL Server databáze z objektu pro vytváření dat kanál (aktivita kopírování):  

## <a name="output-dataset-json"></a>Výstupní datovou sadu JSON
V datové sadě výstup hello JSON, nastavte hello **typ** k: **SqlServerTable**. Nastavit také**AzureSqlTable** toouse s Azure SQL database. Hello hodnotu **tableName** vlastnost musí odpovídat hello název první parametr hello uložené procedury.  

```json
{
  "name": "SqlOutput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlLinkedService",
    "typeProperties": {
      "tableName": "Marketing"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

## <a name="sqlsink-section-in-copy-activity-json"></a>Část SqlSink v aktivitě kopírování JSON
Definování hello **SqlSink** část v aktivitě kopírování hello JSON následujícím způsobem. tooinvoke uložené procedury při vložení dat do hello podřízený nebo cílové databázi, zadejte hodnoty pro obě **SqlWriterStoredProcedureName** a **SqlWriterTableType** vlastnosti. Popis těchto vlastností najdete v tématu [SqlSink části článku konektoru systému SQL Server hello](data-factory-sqlserver-connector.md#sqlsink).

```json
"sink":
{
    "type": "SqlSink",
    "SqlWriterTableType": "MarketingType",
    "SqlWriterStoredProcedureName": "spOverwriteMarketing", 
    "storedProcedureParameters":
            {
                "stringData": 
                {
                    "value": "str1"     
                }
            }
}
```

## <a name="stored-procedure-definition"></a>Definice uložené procedury 
V databázi, zadejte hello uložená procedura s hello stejný název jako **SqlWriterStoredProcedureName**. Hello uložené procedury zpracovává vstupní data z hello zdrojového úložiště dat a vloží data do tabulky v databázi cílové hello. Název Hello hello první parametr uložené procedury musí odpovídat hello tableName definované v datové sadě hello JSON (Marketing).

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
AS
BEGIN
    DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
    INSERT [dbo].[Marketing](ProfileID, State)
    SELECT * FROM @Marketing
END
```

## <a name="table-type-definition"></a>Typ definice tabulky
V databázi, definování typu tabulky hello s hello stejný název jako **SqlWriterTableType**. schéma Hello hello typu tabulky musí odpovídat hello schéma hello vstupní datové sady.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL
)
```

## <a name="next-steps"></a>Další kroky
Projděte si následující články konektoru, které pro dokončení JSON příklady hello: 

- [Azure SQL Database](data-factory-azure-sql-connector.md)
- [SQL Server](data-factory-sqlserver-connector.md)
