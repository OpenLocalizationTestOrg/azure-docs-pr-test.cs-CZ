---
title: aaaLeverage cyklu T-SQL v Azure SQL Data Warehouse | Microsoft Docs
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
ms.openlocfilehash: c7e8f71b910d00d0dfc30f6e5eba190fd05014b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="loops-in-sql-data-warehouse"></a><span data-ttu-id="59623-103">Smyčky v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="59623-103">Loops in SQL Data Warehouse</span></span>
<span data-ttu-id="59623-104">SQL Data Warehouse podporuje hello [při][při] smyčky pro opakovaně provádění blok příkazu.</span><span class="sxs-lookup"><span data-stu-id="59623-104">SQL Data Warehouse supports hello [WHILE][WHILE] loop for repeatedly executing statement blocks.</span></span> <span data-ttu-id="59623-105">To bude pokračovat pro tak dlouho, dokud hello zadán, jsou splněny podmínky, nebo dokud se kód hello konkrétně ukončí hello smyčky pomocí hello `BREAK` – klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="59623-105">This will continue for as long as hello specified conditions are true or until hello code specifically terminates hello loop using hello `BREAK` keyword.</span></span> <span data-ttu-id="59623-106">Smyčky jsou obzvláště užitečná pro nahrazení kurzory definované v kódu SQL.</span><span class="sxs-lookup"><span data-stu-id="59623-106">Loops are particularly useful for replacing cursors defined in SQL code.</span></span> <span data-ttu-id="59623-107">Naštěstí téměř všechny kurzory, které jsou zapsány v kódu SQL jsou z hello rychlé předat dál, přečtěte si pouze různých.</span><span class="sxs-lookup"><span data-stu-id="59623-107">Fortunately, almost all cursors that are written in SQL code are of hello fast forward, read only variety.</span></span> <span data-ttu-id="59623-108">Proto [při] smyčky jsou skvělý alternativní, pokud se přistihnete s tooreplace jeden.</span><span class="sxs-lookup"><span data-stu-id="59623-108">Therefore [WHILE] loops are a great alternative if you find yourself having tooreplace one.</span></span>

## <a name="leveraging-loops-and-replacing-cursors-in-sql-data-warehouse"></a><span data-ttu-id="59623-109">Využití smyčky a nahrazení kurzory v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="59623-109">Leveraging loops and replacing cursors in SQL Data Warehouse</span></span>
<span data-ttu-id="59623-110">Nicméně, než začnete v head nejdřív měli byste požádat sami hello následující otázku: "může být tento kurzor znovu napsané toouse sady na základě operations?".</span><span class="sxs-lookup"><span data-stu-id="59623-110">However, before diving in head first you should ask yourself hello following question: "Could this cursor be re-written toouse set based operations?".</span></span> <span data-ttu-id="59623-111">V mnoha případech hello odpovědí bude Ano a je často nejlepším postupem hello.</span><span class="sxs-lookup"><span data-stu-id="59623-111">In many cases hello answer will be yes and is often hello best approach.</span></span> <span data-ttu-id="59623-112">Operace set na základě často provádí výrazně rychlejší než přístup iterativní, řádek po řádku.</span><span class="sxs-lookup"><span data-stu-id="59623-112">A set based operation often performs significantly faster than an iterative, row by row approach.</span></span>

<span data-ttu-id="59623-113">Rychloposuv vpřed kurzory jen pro čtení můžete snadno nahradit opakování konstrukce.</span><span class="sxs-lookup"><span data-stu-id="59623-113">Fast forward read-only cursors can be easily replaced with a looping construct.</span></span> <span data-ttu-id="59623-114">Níže je jednoduchý příklad.</span><span class="sxs-lookup"><span data-stu-id="59623-114">Below is a simple example.</span></span> <span data-ttu-id="59623-115">Tento příklad kódu aktualizuje hello statistiky pro každou tabulku v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="59623-115">This code example updates hello statistics for every table in hello database.</span></span> <span data-ttu-id="59623-116">Podle iterování přes hello tabulky ve smyčce hello jsme jsou možné tooexecute každý příkaz v pořadí.</span><span class="sxs-lookup"><span data-stu-id="59623-116">By iterating over hello tables in hello loop we are able tooexecute each command in sequence.</span></span>

<span data-ttu-id="59623-117">Nejprve vytvořte dočasnou tabulku obsahující jedinečný řádek číslo používané tooidentify hello jednotlivé příkazy:</span><span class="sxs-lookup"><span data-stu-id="59623-117">First, create a temporary table containing a unique row number used tooidentify hello individual statements:</span></span>

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

<span data-ttu-id="59623-118">Druhý inicializace hello proměnné požadované tooperform hello smyčka:</span><span class="sxs-lookup"><span data-stu-id="59623-118">Second, initialize hello variables required tooperform hello loop:</span></span>

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

<span data-ttu-id="59623-119">Nyní smyčku příkazy provádění jeden současně:</span><span class="sxs-lookup"><span data-stu-id="59623-119">Now loop over statements executing them one at a time:</span></span>

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

<span data-ttu-id="59623-120">Nakonec vyřaďte hello dočasné tabulky vytvořili v prvním kroku hello</span><span class="sxs-lookup"><span data-stu-id="59623-120">Finally drop hello temporary table created in hello first step</span></span>

```
DROP TABLE #tbl;
```


<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->

## <a name="next-steps"></a><span data-ttu-id="59623-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="59623-121">Next steps</span></span>
<span data-ttu-id="59623-122">Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].</span><span class="sxs-lookup"><span data-stu-id="59623-122">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[při]: https://msdn.microsoft.com/library/ms178642.aspx


<!--Other Web references-->
