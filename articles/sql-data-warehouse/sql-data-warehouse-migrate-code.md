---
title: "Migrace kódu SQL do SQL Data Warehouse | Microsoft Docs"
description: "Tipy k migraci kódu SQL Azure SQL Data Warehouse na vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 19c252a3-0e41-4eec-9d3e-09a68c7e7add
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 06/23/2017
ms.author: joeyong;barbkess
ms.openlocfilehash: c6e6b890f5e2d0e31b10bbb6803adad02bf60248
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="migrate-your-sql-code-to-sql-data-warehouse"></a><span data-ttu-id="aeb24-103">Migrace do SQL Data Warehouse kódu SQL</span><span class="sxs-lookup"><span data-stu-id="aeb24-103">Migrate your SQL code to SQL Data Warehouse</span></span>
<span data-ttu-id="aeb24-104">Tento článek vysvětluje změny kódu, které bude pravděpodobně třeba, aby při migraci kódu z jiné databáze SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="aeb24-104">This article explains code changes you will probably need to make when migrating your code from another database to SQL Data Warehouse.</span></span> <span data-ttu-id="aeb24-105">Některé funkce SQL Data Warehouse může výrazně zlepšit výkon, jako jsou navrženy pro práci v distribuované způsobem.</span><span class="sxs-lookup"><span data-stu-id="aeb24-105">Some SQL Data Warehouse features can significantly improve performance as they are designed to work in a distributed fashion.</span></span> <span data-ttu-id="aeb24-106">Ale pokud chcete zachovat, výkonu a možností škálování, některé funkce nejsou také k dispozici.</span><span class="sxs-lookup"><span data-stu-id="aeb24-106">However, to maintain performance and scale, some features are also not available.</span></span>

## <a name="common-t-sql-limitations"></a><span data-ttu-id="aeb24-107">Běžné omezení T-SQL</span><span class="sxs-lookup"><span data-stu-id="aeb24-107">Common T-SQL Limitations</span></span>
<span data-ttu-id="aeb24-108">Následující seznam shrnuje nejběžnější funkcí, které SQL Data Warehouse nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="aeb24-108">The following list summarizes the most common features that SQL Data Warehouse does not support.</span></span> <span data-ttu-id="aeb24-109">Uvedené odkazy vedou k řešení nepodporované funkce:</span><span class="sxs-lookup"><span data-stu-id="aeb24-109">The links take you to workarounds for the unsupported features:</span></span>

* <span data-ttu-id="aeb24-110">[ANSI spojení na aktualizace][ANSI joins on updates]</span><span class="sxs-lookup"><span data-stu-id="aeb24-110">[ANSI joins on updates][ANSI joins on updates]</span></span>
* <span data-ttu-id="aeb24-111">[ANSI spojení na odstranění][ANSI joins on deletes]</span><span class="sxs-lookup"><span data-stu-id="aeb24-111">[ANSI joins on deletes][ANSI joins on deletes]</span></span>
* <span data-ttu-id="aeb24-112">[příkaz Merge][merge statement]</span><span class="sxs-lookup"><span data-stu-id="aeb24-112">[merge statement][merge statement]</span></span>
* <span data-ttu-id="aeb24-113">spojení mezi databáze</span><span class="sxs-lookup"><span data-stu-id="aeb24-113">cross-database joins</span></span>
* <span data-ttu-id="aeb24-114">[kurzory][cursors]</span><span class="sxs-lookup"><span data-stu-id="aeb24-114">[cursors][cursors]</span></span>
* <span data-ttu-id="aeb24-115">[PŘÍKAZ INSERT... EXEC –][INSERT..EXEC]</span><span class="sxs-lookup"><span data-stu-id="aeb24-115">[INSERT..EXEC][INSERT..EXEC]</span></span>
* <span data-ttu-id="aeb24-116">klauzuli Output</span><span class="sxs-lookup"><span data-stu-id="aeb24-116">output clause</span></span>
* <span data-ttu-id="aeb24-117">vložené uživatelem definované funkce</span><span class="sxs-lookup"><span data-stu-id="aeb24-117">inline user-defined functions</span></span>
* <span data-ttu-id="aeb24-118">vícepříkazové funkce</span><span class="sxs-lookup"><span data-stu-id="aeb24-118">multi-statement functions</span></span>
* [<span data-ttu-id="aeb24-119">běžných výrazech tabulky</span><span class="sxs-lookup"><span data-stu-id="aeb24-119">common table expressions</span></span>](#Common-table-expressions)
* <span data-ttu-id="aeb24-120">[rekurzivní běžných výrazech tabulky (CTE)] (#Recursive-common-table-expressions-(CTE)</span><span class="sxs-lookup"><span data-stu-id="aeb24-120">[recursive common table expressions (CTE)](#Recursive-common-table-expressions-(CTE)</span></span>
* <span data-ttu-id="aeb24-121">Funkce CLR a procedury</span><span class="sxs-lookup"><span data-stu-id="aeb24-121">CLR functions and procedures</span></span>
* <span data-ttu-id="aeb24-122">$partition – funkce</span><span class="sxs-lookup"><span data-stu-id="aeb24-122">$partition function</span></span>
* <span data-ttu-id="aeb24-123">proměnné tabulky</span><span class="sxs-lookup"><span data-stu-id="aeb24-123">table variables</span></span>
* <span data-ttu-id="aeb24-124">parametry s hodnotou tabulky</span><span class="sxs-lookup"><span data-stu-id="aeb24-124">table value parameters</span></span>
* <span data-ttu-id="aeb24-125">distribuované transakce</span><span class="sxs-lookup"><span data-stu-id="aeb24-125">distributed transactions</span></span>
* <span data-ttu-id="aeb24-126">potvrzení / vrácení zpět práce</span><span class="sxs-lookup"><span data-stu-id="aeb24-126">commit / rollback work</span></span>
* <span data-ttu-id="aeb24-127">Uložit transakce</span><span class="sxs-lookup"><span data-stu-id="aeb24-127">save transaction</span></span>
* <span data-ttu-id="aeb24-128">kontexty provádění (EXECUTE AS)</span><span class="sxs-lookup"><span data-stu-id="aeb24-128">execution contexts (EXECUTE AS)</span></span>
* <span data-ttu-id="aeb24-129">[s kumulativní klauzule Group by / datové krychle / nastaví možnosti seskupení][group by clause with rollup / cube / grouping sets options]</span><span class="sxs-lookup"><span data-stu-id="aeb24-129">[group by clause with rollup / cube / grouping sets options][group by clause with rollup / cube / grouping sets options]</span></span>
* <span data-ttu-id="aeb24-130">[vnořených úrovní nad rámec 8][nesting levels beyond 8]</span><span class="sxs-lookup"><span data-stu-id="aeb24-130">[nesting levels beyond 8][nesting levels beyond 8]</span></span>
* <span data-ttu-id="aeb24-131">[aktualizace prostřednictvím zobrazení][updating through views]</span><span class="sxs-lookup"><span data-stu-id="aeb24-131">[updating through views][updating through views]</span></span>
* <span data-ttu-id="aeb24-132">[použití vyberte pro přiřazení proměnné][use of select for variable assignment]</span><span class="sxs-lookup"><span data-stu-id="aeb24-132">[use of select for variable assignment][use of select for variable assignment]</span></span>
* <span data-ttu-id="aeb24-133">[žádné maximální datový typ pro dynamické řetězce SQL][no MAX data type for dynamic SQL strings]</span><span class="sxs-lookup"><span data-stu-id="aeb24-133">[no MAX data type for dynamic SQL strings][no MAX data type for dynamic SQL strings]</span></span>

<span data-ttu-id="aeb24-134">Naštěstí můžete být kolem fungovala většinu těchto omezení.</span><span class="sxs-lookup"><span data-stu-id="aeb24-134">Fortunately most of these limitations can be worked around.</span></span> <span data-ttu-id="aeb24-135">Vysvětlení najdete v článcích relevantní vývoj výše uvedené.</span><span class="sxs-lookup"><span data-stu-id="aeb24-135">Explanations are provided in the relevant development articles referenced above.</span></span>

## <a name="supported-cte-features"></a><span data-ttu-id="aeb24-136">Podporované funkce CTE</span><span class="sxs-lookup"><span data-stu-id="aeb24-136">Supported CTE features</span></span>
<span data-ttu-id="aeb24-137">Běžných výrazech tabulky (odkazu Cte) jsou podporovány jen částečně v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="aeb24-137">Common table expressions (CTEs) are partially supported in SQL Data Warehouse.</span></span>  <span data-ttu-id="aeb24-138">Aktuálně jsou podporovány následující funkce CTE:</span><span class="sxs-lookup"><span data-stu-id="aeb24-138">The following CTE features are currently supported:</span></span>

* <span data-ttu-id="aeb24-139">CTE lze zadat v příkazu SELECT.</span><span class="sxs-lookup"><span data-stu-id="aeb24-139">A CTE can be specified in a SELECT statement.</span></span>
* <span data-ttu-id="aeb24-140">CTE lze zadat v příkazu CREATE VIEW.</span><span class="sxs-lookup"><span data-stu-id="aeb24-140">A CTE can be specified in a CREATE VIEW statement.</span></span>
* <span data-ttu-id="aeb24-141">CTE lze zadat v příkazu vytvořte tabulku AS vyberte funkce CTAS ().</span><span class="sxs-lookup"><span data-stu-id="aeb24-141">A CTE can be specified in a CREATE TABLE AS SELECT (CTAS) statement.</span></span>
* <span data-ttu-id="aeb24-142">CTE lze zadat v příkazu vytvořit vzdálené tabulky AS vyberte (CRTAS).</span><span class="sxs-lookup"><span data-stu-id="aeb24-142">A CTE can be specified in a CREATE REMOTE TABLE AS SELECT (CRTAS) statement.</span></span>
* <span data-ttu-id="aeb24-143">CTE lze zadat v příkazu vytvořit externí tabulky AS vyberte (CETAS).</span><span class="sxs-lookup"><span data-stu-id="aeb24-143">A CTE can be specified in a CREATE EXTERNAL TABLE AS SELECT (CETAS) statement.</span></span>
* <span data-ttu-id="aeb24-144">Vzdálenou tabulku se může odkazovat z CTE.</span><span class="sxs-lookup"><span data-stu-id="aeb24-144">A remote table can be referenced from a CTE.</span></span>
* <span data-ttu-id="aeb24-145">Externí tabulku se může odkazovat z CTE.</span><span class="sxs-lookup"><span data-stu-id="aeb24-145">An external table can be referenced from a CTE.</span></span>
* <span data-ttu-id="aeb24-146">Několik definic CTE dotazu může být definován v CTE.</span><span class="sxs-lookup"><span data-stu-id="aeb24-146">Multiple CTE query definitions can be defined in a CTE.</span></span>

## <a name="cte-limitations"></a><span data-ttu-id="aeb24-147">Omezení CTE</span><span class="sxs-lookup"><span data-stu-id="aeb24-147">CTE Limitations</span></span>
<span data-ttu-id="aeb24-148">Běžných výrazech tabulky mají určitá omezení v SQL Data Warehouse, včetně:</span><span class="sxs-lookup"><span data-stu-id="aeb24-148">Common table expressions have some limitations in SQL Data Warehouse including:</span></span>

* <span data-ttu-id="aeb24-149">CTE musí následovat jediný příkaz SELECT.</span><span class="sxs-lookup"><span data-stu-id="aeb24-149">A CTE must be followed by a single SELECT statement.</span></span> <span data-ttu-id="aeb24-150">Příkaz INSERT, UPDATE, DELETE a MERGE příkazy nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="aeb24-150">INSERT, UPDATE, DELETE, and MERGE statements are not supported.</span></span>
* <span data-ttu-id="aeb24-151">Výraz běžné tabulky, který obsahuje odkazy na sebe sama (rekurzivní výraz běžné tabulky) nepodporuje (viz níže části).</span><span class="sxs-lookup"><span data-stu-id="aeb24-151">A common table expression that includes references to itself (a recursive common table expression) is not supported (see below section).</span></span>
* <span data-ttu-id="aeb24-152">Určení více než jeden s klauzulí v CTE není povoleno.</span><span class="sxs-lookup"><span data-stu-id="aeb24-152">Specifying more than one WITH clause in a CTE is not allowed.</span></span> <span data-ttu-id="aeb24-153">Například pokud CTE_query_definition obsahuje poddotaz, že poddotazu nesmí obsahovat vnořený s klauzulí, která definuje jiné CTE.</span><span class="sxs-lookup"><span data-stu-id="aeb24-153">For example, if a CTE_query_definition contains a subquery, that subquery cannot contain a nested WITH clause that defines another CTE.</span></span>
* <span data-ttu-id="aeb24-154">Klauzuli ORDER BY nelze použít v CTE_query_definition, s výjimkou, když je zadané klauzuli TOP.</span><span class="sxs-lookup"><span data-stu-id="aeb24-154">An ORDER BY clause cannot be used in the CTE_query_definition, except when a TOP clause is specified.</span></span>
* <span data-ttu-id="aeb24-155">Pokud CTE se používá v příkazu, který je součástí dávky, příkaz dříve, než se musí následovat středníkem.</span><span class="sxs-lookup"><span data-stu-id="aeb24-155">When a CTE is used in a statement that is part of a batch, the statement before it must be followed by a semicolon.</span></span>
* <span data-ttu-id="aeb24-156">Pokud se použije v příkazech připravené sp_prepare, bude odkazu Cte chovají stejným způsobem jako ostatní příkazů SELECT v PDW.</span><span class="sxs-lookup"><span data-stu-id="aeb24-156">When used in statements prepared by sp_prepare, CTEs will behave the same way as other SELECT statements in PDW.</span></span> <span data-ttu-id="aeb24-157">Ale pokud odkazu Cte se používají jako součást CETAS připravené sp_prepare, chování můžete odložit ze systému SQL Server a další příkazy PDW kvůli způsob, jakým vazba je implementována pro sp_prepare.</span><span class="sxs-lookup"><span data-stu-id="aeb24-157">However, if CTEs are used as part of CETAS prepared by sp_prepare, the behavior can defer from SQL Server and other PDW statements because of the way binding is implemented for sp_prepare.</span></span> <span data-ttu-id="aeb24-158">Pokud vyberte odkazů CTE je pomocí nesprávného sloupce, který neexistuje v CTE, sp_prepare předá bez zjišťování chyba, že chyba bude vyvolána při proceduře sp_execute místo.</span><span class="sxs-lookup"><span data-stu-id="aeb24-158">If SELECT that references CTE is using a wrong column that does not exist in CTE, the sp_prepare will pass without detecting the error, but the error will be thrown during sp_execute instead.</span></span>

## <a name="recursive-ctes"></a><span data-ttu-id="aeb24-159">Rekurzivního odkazu Cte</span><span class="sxs-lookup"><span data-stu-id="aeb24-159">Recursive CTEs</span></span>
<span data-ttu-id="aeb24-160">Rekurzivního odkazu Cte nejsou podporovány v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="aeb24-160">Recursive CTEs are not supported in SQL Data Warehouse.</span></span>  <span data-ttu-id="aeb24-161">Migrace rekurzivní CTE může být poněkud složité a proces nejlepší je rozdělit na několik kroků.</span><span class="sxs-lookup"><span data-stu-id="aeb24-161">The migration of recursive CTE can be somewhat complex and the best process is to break it into multiple steps.</span></span> <span data-ttu-id="aeb24-162">Obvykle můžete použít smyčku a podle iterace v rekurzivní dotazy dočasné naplňte dočasné tabulky.</span><span class="sxs-lookup"><span data-stu-id="aeb24-162">You can typically use a loop and populate a temporary table as you iterate over the recursive interim queries.</span></span> <span data-ttu-id="aeb24-163">Jakmile se naplní dočasné tabulky můžete vrátí data jako jednu výslednou sadu.</span><span class="sxs-lookup"><span data-stu-id="aeb24-163">Once the temporary table is populated you can then return the data as a single result set.</span></span> <span data-ttu-id="aeb24-164">Podobný postup se používá ke vyřešit `GROUP BY WITH CUBE` v [s kumulativní klauzule group by / datové krychle / nastaví možnosti seskupení] [ group by clause with rollup / cube / grouping sets options] článku.</span><span class="sxs-lookup"><span data-stu-id="aeb24-164">A similar approach has been used to solve `GROUP BY WITH CUBE` in the [group by clause with rollup / cube / grouping sets options][group by clause with rollup / cube / grouping sets options] article.</span></span>

## <a name="unsupported-system-functions"></a><span data-ttu-id="aeb24-165">Funkce nepodporované systému</span><span class="sxs-lookup"><span data-stu-id="aeb24-165">Unsupported system functions</span></span>
<span data-ttu-id="aeb24-166">Existují také některé funkce systému, které nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="aeb24-166">There are also some system functions that are not supported.</span></span> <span data-ttu-id="aeb24-167">Mezi hlavní ty, které obvykle je možné použít v datových skladů, patří:</span><span class="sxs-lookup"><span data-stu-id="aeb24-167">Some of the main ones you might typically find used in data warehousing are:</span></span>

* <span data-ttu-id="aeb24-168">NEWSEQUENTIALID()</span><span class="sxs-lookup"><span data-stu-id="aeb24-168">NEWSEQUENTIALID()</span></span>
* <span data-ttu-id="aeb24-169">@@NESTLEVEL()</span><span class="sxs-lookup"><span data-stu-id="aeb24-169">@@NESTLEVEL()</span></span>
* <span data-ttu-id="aeb24-170">@@IDENTITY()</span><span class="sxs-lookup"><span data-stu-id="aeb24-170">@@IDENTITY()</span></span>
* <span data-ttu-id="aeb24-171">@@ROWCOUNT()</span><span class="sxs-lookup"><span data-stu-id="aeb24-171">@@ROWCOUNT()</span></span>
* <span data-ttu-id="aeb24-172">ROWCOUNT_BIG</span><span class="sxs-lookup"><span data-stu-id="aeb24-172">ROWCOUNT_BIG</span></span>
* <span data-ttu-id="aeb24-173">ERROR_LINE()</span><span class="sxs-lookup"><span data-stu-id="aeb24-173">ERROR_LINE()</span></span>

<span data-ttu-id="aeb24-174">Některé z těchto problémů můžete fungovala kolem.</span><span class="sxs-lookup"><span data-stu-id="aeb24-174">Some of these issues can be worked around.</span></span>

## <a name="rowcount-workaround"></a><span data-ttu-id="aeb24-175">@@ROWCOUNT alternativní řešení</span><span class="sxs-lookup"><span data-stu-id="aeb24-175">@@ROWCOUNT workaround</span></span>
<span data-ttu-id="aeb24-176">Chcete-li vyřešit nedostatečná podpora pro @@ROWCOUNT, vytvoření uložené procedury, která bude načítat poslední počet řádků z sys.dm_pdw_request_steps a potom spusťte `EXEC LastRowCount` po příkaz DML.</span><span class="sxs-lookup"><span data-stu-id="aeb24-176">To work around lack of support for @@ROWCOUNT, create a stored procedure that will retrieve the last row count from sys.dm_pdw_request_steps and then execute `EXEC LastRowCount` after a DML statement.</span></span>

```sql
CREATE PROCEDURE LastRowCount AS
WITH LastRequest as 
(   SELECT TOP 1    request_id
    FROM            sys.dm_pdw_exec_requests
    WHERE           session_id = SESSION_ID()
    AND             resource_class IS NOT NULL
    ORDER BY end_time DESC
),
LastRequestRowCounts as
(
    SELECT  step_index, row_count
    FROM    sys.dm_pdw_request_steps
    WHERE   row_count >= 0
    AND     request_id IN (SELECT request_id from LastRequest)
)
SELECT TOP 1 row_count FROM LastRequestRowCounts ORDER BY step_index DESC
;
```

## <a name="next-steps"></a><span data-ttu-id="aeb24-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="aeb24-177">Next steps</span></span>
<span data-ttu-id="aeb24-178">Úplný seznam všech podporovaných příkazů T-SQL najdete v tématu [Transact-SQL témata][Transact-SQL topics].</span><span class="sxs-lookup"><span data-stu-id="aeb24-178">For a complete list of all supported T-SQL statements, see [Transact-SQL topics][Transact-SQL topics].</span></span>

<!--Image references-->

<!--Article references-->
[ANSI joins on updates]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[ANSI joins on deletes]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[merge statement]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[INSERT..EXEC]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[Transact-SQL topics]: ./sql-data-warehouse-reference-tsql-statements.md

[cursors]: ./sql-data-warehouse-develop-loops.md
[group by clause with rollup / cube / grouping sets options]: ./sql-data-warehouse-develop-group-by-options.md
[nesting levels beyond 8]: ./sql-data-warehouse-develop-transactions.md
[updating through views]: ./sql-data-warehouse-develop-views.md
[use of select for variable assignment]: ./sql-data-warehouse-develop-variable-assignment.md
[no MAX data type for dynamic SQL strings]: ./sql-data-warehouse-develop-dynamic-sql.md

<!--MSDN references-->

<!--Other Web references-->
