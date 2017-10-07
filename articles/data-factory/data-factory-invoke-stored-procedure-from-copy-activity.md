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
# <a name="invoke-stored-procedure-from-copy-activity-in-azure-data-factory"></a><span data-ttu-id="807c7-103">Vyvolat uloženou proceduru z aktivity kopírování v Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="807c7-103">Invoke stored procedure from copy activity in Azure Data Factory</span></span>
<span data-ttu-id="807c7-104">Při kopírování dat do [systému SQL Server](data-factory-sqlserver-connector.md) nebo [Azure SQL Database](data-factory-azure-sql-connector.md), můžete nakonfigurovat hello **SqlSink** v tooinvoke kopie aktivity uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="807c7-104">When copying data into [SQL Server](data-factory-sqlserver-connector.md) or [Azure SQL Database](data-factory-azure-sql-connector.md), you can configure hello **SqlSink** in copy activity tooinvoke a stored procedure.</span></span> <span data-ttu-id="807c7-105">Můžete chtít toouse hello uložené procedury tooperform žádné další zpracování (slučování sloupců, vyhledávání hodnot, vložení do více tabulek, atd.) je nutné před vložením dat v toohello cílové tabulky.</span><span class="sxs-lookup"><span data-stu-id="807c7-105">You may want toouse hello stored procedure tooperform any additional processing (merging columns, looking up values, insertion into multiple tables, etc.) is required before inserting data in toohello destination table.</span></span> <span data-ttu-id="807c7-106">Tato funkce využívá [zavolat parametry](https://msdn.microsoft.com/library/bb675163.aspx).</span><span class="sxs-lookup"><span data-stu-id="807c7-106">This feature takes advantage of [Table-Valued Parameters](https://msdn.microsoft.com/library/bb675163.aspx).</span></span> 

<span data-ttu-id="807c7-107">Následující ukázka Hello ukazuje, jak tooinvoke uloženou proceduru v systému SQL Server databáze z objektu pro vytváření dat kanál (aktivita kopírování):</span><span class="sxs-lookup"><span data-stu-id="807c7-107">hello following sample shows how tooinvoke a stored procedure in a SQL Server database from a Data Factory pipeline (copy activity):</span></span>  

## <a name="output-dataset-json"></a><span data-ttu-id="807c7-108">Výstupní datovou sadu JSON</span><span class="sxs-lookup"><span data-stu-id="807c7-108">Output dataset JSON</span></span>
<span data-ttu-id="807c7-109">V datové sadě výstup hello JSON, nastavte hello **typ** k: **SqlServerTable**.</span><span class="sxs-lookup"><span data-stu-id="807c7-109">In hello output dataset JSON, set hello **type** to: **SqlServerTable**.</span></span> <span data-ttu-id="807c7-110">Nastavit také**AzureSqlTable** toouse s Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="807c7-110">Set it too**AzureSqlTable** toouse with an Azure SQL database.</span></span> <span data-ttu-id="807c7-111">Hello hodnotu **tableName** vlastnost musí odpovídat hello název první parametr hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="807c7-111">hello value for **tableName** property must match hello name of first parameter of hello stored procedure.</span></span>  

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

## <a name="sqlsink-section-in-copy-activity-json"></a><span data-ttu-id="807c7-112">Část SqlSink v aktivitě kopírování JSON</span><span class="sxs-lookup"><span data-stu-id="807c7-112">SqlSink section in copy activity JSON</span></span>
<span data-ttu-id="807c7-113">Definování hello **SqlSink** část v aktivitě kopírování hello JSON následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="807c7-113">Define hello **SqlSink** section in hello copy activity JSON as follows.</span></span> <span data-ttu-id="807c7-114">tooinvoke uložené procedury při vložení dat do hello podřízený nebo cílové databázi, zadejte hodnoty pro obě **SqlWriterStoredProcedureName** a **SqlWriterTableType** vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="807c7-114">tooinvoke a stored procedure while inserting data into hello sink/destination database, specify values for both **SqlWriterStoredProcedureName** and **SqlWriterTableType** properties.</span></span> <span data-ttu-id="807c7-115">Popis těchto vlastností najdete v tématu [SqlSink části článku konektoru systému SQL Server hello](data-factory-sqlserver-connector.md#sqlsink).</span><span class="sxs-lookup"><span data-stu-id="807c7-115">For descriptions of these properties, see [SqlSink section in hello SQL Server connector article](data-factory-sqlserver-connector.md#sqlsink).</span></span>

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

## <a name="stored-procedure-definition"></a><span data-ttu-id="807c7-116">Definice uložené procedury</span><span class="sxs-lookup"><span data-stu-id="807c7-116">Stored procedure definition</span></span> 
<span data-ttu-id="807c7-117">V databázi, zadejte hello uložená procedura s hello stejný název jako **SqlWriterStoredProcedureName**.</span><span class="sxs-lookup"><span data-stu-id="807c7-117">In your database, define hello stored procedure with hello same name as **SqlWriterStoredProcedureName**.</span></span> <span data-ttu-id="807c7-118">Hello uložené procedury zpracovává vstupní data z hello zdrojového úložiště dat a vloží data do tabulky v databázi cílové hello.</span><span class="sxs-lookup"><span data-stu-id="807c7-118">hello stored procedure handles input data from hello source data store, and inserts data into a table in hello destination database.</span></span> <span data-ttu-id="807c7-119">Název Hello hello první parametr uložené procedury musí odpovídat hello tableName definované v datové sadě hello JSON (Marketing).</span><span class="sxs-lookup"><span data-stu-id="807c7-119">hello name of hello first parameter of stored procedure must match hello tableName defined in hello dataset JSON (Marketing).</span></span>

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
AS
BEGIN
    DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
    INSERT [dbo].[Marketing](ProfileID, State)
    SELECT * FROM @Marketing
END
```

## <a name="table-type-definition"></a><span data-ttu-id="807c7-120">Typ definice tabulky</span><span class="sxs-lookup"><span data-stu-id="807c7-120">Table type definition</span></span>
<span data-ttu-id="807c7-121">V databázi, definování typu tabulky hello s hello stejný název jako **SqlWriterTableType**.</span><span class="sxs-lookup"><span data-stu-id="807c7-121">In your database, define hello table type with hello same name as **SqlWriterTableType**.</span></span> <span data-ttu-id="807c7-122">schéma Hello hello typu tabulky musí odpovídat hello schéma hello vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="807c7-122">hello schema of hello table type must match hello schema of hello input dataset.</span></span>

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL
)
```

## <a name="next-steps"></a><span data-ttu-id="807c7-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="807c7-123">Next steps</span></span>
<span data-ttu-id="807c7-124">Projděte si následující články konektoru, které pro dokončení JSON příklady hello:</span><span class="sxs-lookup"><span data-stu-id="807c7-124">Review hello following connector articles that for complete JSON examples:</span></span> 

- [<span data-ttu-id="807c7-125">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="807c7-125">Azure SQL Database</span></span>](data-factory-azure-sql-connector.md)
- [<span data-ttu-id="807c7-126">SQL Server</span><span class="sxs-lookup"><span data-stu-id="807c7-126">SQL Server</span></span>](data-factory-sqlserver-connector.md)
