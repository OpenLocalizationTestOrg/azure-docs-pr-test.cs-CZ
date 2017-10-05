---
title: "Využít smyčky T-SQL v Azure SQL Data Warehouse | Microsoft Docs"
description: "Tipy pro smyčky Transact-SQL a nahraďte kurzory v Azure SQL Data Warehouse na vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: f3384b81-b943-431b-bc73-90e47e4c195f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 40a872ff310f48bfd543ac184fe7301b85b50258
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="loops-in-sql-data-warehouse"></a><span data-ttu-id="2cb54-103">Smyčky v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="2cb54-103">Loops in SQL Data Warehouse</span></span>
<span data-ttu-id="2cb54-104">Podporuje SQL Data Warehouse [při][při] smyčky pro opakovaně provádění blok příkazu.</span><span class="sxs-lookup"><span data-stu-id="2cb54-104">SQL Data Warehouse supports the [WHILE][WHILE] loop for repeatedly executing statement blocks.</span></span> <span data-ttu-id="2cb54-105">To bude pokračovat pro tak dlouho, dokud zadané podmínky splněny, nebo dokud se kód konkrétně ukončí, pomocí smyčky `BREAK` – klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="2cb54-105">This will continue for as long as the specified conditions are true or until the code specifically terminates the loop using the `BREAK` keyword.</span></span> <span data-ttu-id="2cb54-106">Smyčky jsou obzvláště užitečná pro nahrazení kurzory definované v kódu SQL.</span><span class="sxs-lookup"><span data-stu-id="2cb54-106">Loops are particularly useful for replacing cursors defined in SQL code.</span></span> <span data-ttu-id="2cb54-107">Naštěstí téměř všechny kurzory, které jsou zapsány v kódu SQL jsou rychloposuv vpřed číst pouze řady.</span><span class="sxs-lookup"><span data-stu-id="2cb54-107">Fortunately, almost all cursors that are written in SQL code are of the fast forward, read only variety.</span></span> <span data-ttu-id="2cb54-108">Proto [při] smyčky jsou skvělý alternativní, pokud se přistihnete museli nahradit jednu.</span><span class="sxs-lookup"><span data-stu-id="2cb54-108">Therefore [WHILE] loops are a great alternative if you find yourself having to replace one.</span></span>

## <a name="leveraging-loops-and-replacing-cursors-in-sql-data-warehouse"></a><span data-ttu-id="2cb54-109">Využití smyčky a nahrazení kurzory v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="2cb54-109">Leveraging loops and replacing cursors in SQL Data Warehouse</span></span>
<span data-ttu-id="2cb54-110">Ale, než začnete v head nejdřív měli byste požádat sami následující otázku: "Tento kurzor lze znovu zapsat používat sady na základě operace?".</span><span class="sxs-lookup"><span data-stu-id="2cb54-110">However, before diving in head first you should ask yourself the following question: "Could this cursor be re-written to use set based operations?".</span></span> <span data-ttu-id="2cb54-111">V mnoha případech odpověď bude Ano a je často nejlepším přístupem.</span><span class="sxs-lookup"><span data-stu-id="2cb54-111">In many cases the answer will be yes and is often the best approach.</span></span> <span data-ttu-id="2cb54-112">Operace set na základě často provádí výrazně rychlejší než přístup iterativní, řádek po řádku.</span><span class="sxs-lookup"><span data-stu-id="2cb54-112">A set based operation often performs significantly faster than an iterative, row by row approach.</span></span>

<span data-ttu-id="2cb54-113">Rychloposuv vpřed kurzory jen pro čtení můžete snadno nahradit opakování konstrukce.</span><span class="sxs-lookup"><span data-stu-id="2cb54-113">Fast forward read-only cursors can be easily replaced with a looping construct.</span></span> <span data-ttu-id="2cb54-114">Níže je jednoduchý příklad.</span><span class="sxs-lookup"><span data-stu-id="2cb54-114">Below is a simple example.</span></span> <span data-ttu-id="2cb54-115">Tento příklad kódu aktualizuje statistiku pro každou tabulku v databázi.</span><span class="sxs-lookup"><span data-stu-id="2cb54-115">This code example updates the statistics for every table in the database.</span></span> <span data-ttu-id="2cb54-116">Iterování přes tabulky v smyčky jsme jsou mohou ke spuštění každého příkazu v pořadí.</span><span class="sxs-lookup"><span data-stu-id="2cb54-116">By iterating over the tables in the loop we are able to execute each command in sequence.</span></span>

<span data-ttu-id="2cb54-117">Nejprve vytvořte dočasnou tabulku obsahující řádek jedinečné číslo, které používají k identifikaci jednotlivých příkazy:</span><span class="sxs-lookup"><span data-stu-id="2cb54-117">First, create a temporary table containing a unique row number used to identify the individual statements:</span></span>

```
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

<span data-ttu-id="2cb54-118">Druhý inicializace proměnné potřebná k provedení smyčka:</span><span class="sxs-lookup"><span data-stu-id="2cb54-118">Second, initialize the variables required to perform the loop:</span></span>

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

<span data-ttu-id="2cb54-119">Nyní smyčku příkazy provádění jeden současně:</span><span class="sxs-lookup"><span data-stu-id="2cb54-119">Now loop over statements executing them one at a time:</span></span>

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

<span data-ttu-id="2cb54-120">Nakonec vyřaďte dočasné tabulky vytvořené v prvním kroku</span><span class="sxs-lookup"><span data-stu-id="2cb54-120">Finally drop the temporary table created in the first step</span></span>

```
DROP TABLE #tbl;
```


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a><span data-ttu-id="2cb54-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2cb54-121">Next steps</span></span>
<span data-ttu-id="2cb54-122">Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].</span><span class="sxs-lookup"><span data-stu-id="2cb54-122">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
<span data-ttu-id="2cb54-123">[při]: https://msdn.microsoft.com/library/ms178642.aspx</span><span class="sxs-lookup"><span data-stu-id="2cb54-123">[WHILE]: https://msdn.microsoft.com/library/ms178642.aspx</span></span>


<!--Other Web references-->
