---
title: "Vyvolat uloženou proceduru z kopie aktivita služby Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak vyvolat uloženou proceduru v Azure SQL Database nebo SQL Server z aktivity kopírování Azure Data Factory."
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
ms.openlocfilehash: af6e4a57e726598c266ee766656aa2cc22e374e3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="invoke-stored-procedure-from-copy-activity-in-azure-data-factory"></a><span data-ttu-id="e3fed-103">Vyvolat uloženou proceduru z aktivity kopírování v Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="e3fed-103">Invoke stored procedure from copy activity in Azure Data Factory</span></span>
<span data-ttu-id="e3fed-104">Při kopírování dat do [systému SQL Server](data-factory-sqlserver-connector.md) nebo [Azure SQL Database](data-factory-azure-sql-connector.md), můžete nakonfigurovat **SqlSink** v aktivitě kopírování vyvolat uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="e3fed-104">When copying data into [SQL Server](data-factory-sqlserver-connector.md) or [Azure SQL Database](data-factory-azure-sql-connector.md), you can configure the **SqlSink** in copy activity to invoke a stored procedure.</span></span> <span data-ttu-id="e3fed-105">Můžete chtít použít uložené procedury provést žádné další zpracování (slučování sloupců, vyhledávání hodnot, vložení do více tabulek, atd.) je vyžadována před vkládání dat v cílové tabulky.</span><span class="sxs-lookup"><span data-stu-id="e3fed-105">You may want to use the stored procedure to perform any additional processing (merging columns, looking up values, insertion into multiple tables, etc.) is required before inserting data in to the destination table.</span></span> <span data-ttu-id="e3fed-106">Tato funkce využívá [zavolat parametry](https://msdn.microsoft.com/library/bb675163.aspx).</span><span class="sxs-lookup"><span data-stu-id="e3fed-106">This feature takes advantage of [Table-Valued Parameters](https://msdn.microsoft.com/library/bb675163.aspx).</span></span> 

<span data-ttu-id="e3fed-107">Následující příklad ukazuje, jak vyvolat uloženou proceduru v databázi systému SQL Server z objektu pro vytváření dat kanál (aktivita kopírování):</span><span class="sxs-lookup"><span data-stu-id="e3fed-107">The following sample shows how to invoke a stored procedure in a SQL Server database from a Data Factory pipeline (copy activity):</span></span>  

## <a name="output-dataset-json"></a><span data-ttu-id="e3fed-108">Výstupní datovou sadu JSON</span><span class="sxs-lookup"><span data-stu-id="e3fed-108">Output dataset JSON</span></span>
<span data-ttu-id="e3fed-109">V výstupní datovou sadu JSON, nastavte **typ** k: **SqlServerTable**.</span><span class="sxs-lookup"><span data-stu-id="e3fed-109">In the output dataset JSON, set the **type** to: **SqlServerTable**.</span></span> <span data-ttu-id="e3fed-110">Nastavte ji na **AzureSqlTable** pro použití s Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="e3fed-110">Set it to **AzureSqlTable** to use with an Azure SQL database.</span></span> <span data-ttu-id="e3fed-111">Hodnota **tableName** vlastnost musí shodovat s názvem první parametr uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="e3fed-111">The value for **tableName** property must match the name of first parameter of the stored procedure.</span></span>  

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

## <a name="sqlsink-section-in-copy-activity-json"></a><span data-ttu-id="e3fed-112">Část SqlSink v aktivitě kopírování JSON</span><span class="sxs-lookup"><span data-stu-id="e3fed-112">SqlSink section in copy activity JSON</span></span>
<span data-ttu-id="e3fed-113">Definování **SqlSink** část v aktivitě kopírování JSON následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="e3fed-113">Define the **SqlSink** section in the copy activity JSON as follows.</span></span> <span data-ttu-id="e3fed-114">Vyvolat uloženou proceduru při vložení dat do podřízený nebo cílové databázi, zadejte hodnoty pro obě **SqlWriterStoredProcedureName** a **SqlWriterTableType** vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e3fed-114">To invoke a stored procedure while inserting data into the sink/destination database, specify values for both **SqlWriterStoredProcedureName** and **SqlWriterTableType** properties.</span></span> <span data-ttu-id="e3fed-115">Popis těchto vlastností najdete v tématu [SqlSink část v systému SQL Server konektoru článku](data-factory-sqlserver-connector.md#sqlsink).</span><span class="sxs-lookup"><span data-stu-id="e3fed-115">For descriptions of these properties, see [SqlSink section in the SQL Server connector article](data-factory-sqlserver-connector.md#sqlsink).</span></span>

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

## <a name="stored-procedure-definition"></a><span data-ttu-id="e3fed-116">Definice uložené procedury</span><span class="sxs-lookup"><span data-stu-id="e3fed-116">Stored procedure definition</span></span> 
<span data-ttu-id="e3fed-117">V databázi, zadejte uložená procedura se stejným názvem jako **SqlWriterStoredProcedureName**.</span><span class="sxs-lookup"><span data-stu-id="e3fed-117">In your database, define the stored procedure with the same name as **SqlWriterStoredProcedureName**.</span></span> <span data-ttu-id="e3fed-118">Uložená procedura zpracovává vstupní data ze zdrojového úložiště dat a vloží data do tabulky v cílové databázi.</span><span class="sxs-lookup"><span data-stu-id="e3fed-118">The stored procedure handles input data from the source data store, and inserts data into a table in the destination database.</span></span> <span data-ttu-id="e3fed-119">Název prvního parametru uložené procedury musí odpovídat tableName definované v sadě dat JSON (Marketing).</span><span class="sxs-lookup"><span data-stu-id="e3fed-119">The name of the first parameter of stored procedure must match the tableName defined in the dataset JSON (Marketing).</span></span>

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
AS
BEGIN
    DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
    INSERT [dbo].[Marketing](ProfileID, State)
    SELECT * FROM @Marketing
END
```

## <a name="table-type-definition"></a><span data-ttu-id="e3fed-120">Typ definice tabulky</span><span class="sxs-lookup"><span data-stu-id="e3fed-120">Table type definition</span></span>
<span data-ttu-id="e3fed-121">V databázi, zadejte typ tabulky se stejným názvem jako **SqlWriterTableType**.</span><span class="sxs-lookup"><span data-stu-id="e3fed-121">In your database, define the table type with the same name as **SqlWriterTableType**.</span></span> <span data-ttu-id="e3fed-122">Schéma typu tabulky musí odpovídat schématu vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="e3fed-122">The schema of the table type must match the schema of the input dataset.</span></span>

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL
)
```

## <a name="next-steps"></a><span data-ttu-id="e3fed-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e3fed-123">Next steps</span></span>
<span data-ttu-id="e3fed-124">Zkontrolujte konektor následující články, dokončení příklady JSON:</span><span class="sxs-lookup"><span data-stu-id="e3fed-124">Review the following connector articles that for complete JSON examples:</span></span> 

- [<span data-ttu-id="e3fed-125">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="e3fed-125">Azure SQL Database</span></span>](data-factory-azure-sql-connector.md)
- [<span data-ttu-id="e3fed-126">SQL Server</span><span class="sxs-lookup"><span data-stu-id="e3fed-126">SQL Server</span></span>](data-factory-sqlserver-connector.md)
