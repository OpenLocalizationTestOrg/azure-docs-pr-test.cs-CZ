---
title: "aaaMigrate vaše tooSQL kódu SQL Data Warehouse | Microsoft Docs"
description: "Tipy pro migraci vaší SQL kódu tooAzure SQL Data Warehouse na vývoj řešení."
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
ms.openlocfilehash: 7a16d579d068e9df9aba3dc61e4a09bcaa551588
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-sql-code-toosql-data-warehouse"></a><span data-ttu-id="d56d5-103">Migrace vaší tooSQL kódu SQL datového skladu</span><span class="sxs-lookup"><span data-stu-id="d56d5-103">Migrate your SQL code tooSQL Data Warehouse</span></span>
<span data-ttu-id="d56d5-104">Tento článek vysvětluje změny kódu, pravděpodobně bude třeba toomake při migraci kódu z jiné databáze tooSQL datového skladu.</span><span class="sxs-lookup"><span data-stu-id="d56d5-104">This article explains code changes you will probably need toomake when migrating your code from another database tooSQL Data Warehouse.</span></span> <span data-ttu-id="d56d5-105">Některé funkce SQL Data Warehouse může výrazně zlepšit výkon, protože se jedná o navrženou toowork distribuované způsobem.</span><span class="sxs-lookup"><span data-stu-id="d56d5-105">Some SQL Data Warehouse features can significantly improve performance as they are designed toowork in a distributed fashion.</span></span> <span data-ttu-id="d56d5-106">Ale toomaintain výkonu a možností škálování, některé funkce nejsou také k dispozici.</span><span class="sxs-lookup"><span data-stu-id="d56d5-106">However, toomaintain performance and scale, some features are also not available.</span></span>

## <a name="common-t-sql-limitations"></a><span data-ttu-id="d56d5-107">Běžné omezení T-SQL</span><span class="sxs-lookup"><span data-stu-id="d56d5-107">Common T-SQL Limitations</span></span>
<span data-ttu-id="d56d5-108">Hello následující seznam shrnuje hello nejběžnější funkce, které SQL Data Warehouse nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="d56d5-108">hello following list summarizes hello most common features that SQL Data Warehouse does not support.</span></span> <span data-ttu-id="d56d5-109">Hello odkazy obsahují tooworkarounds pro hello nepodporované funkce:</span><span class="sxs-lookup"><span data-stu-id="d56d5-109">hello links take you tooworkarounds for hello unsupported features:</span></span>

* <span data-ttu-id="d56d5-110">[ANSI spojení na aktualizace][ANSI joins on updates]</span><span class="sxs-lookup"><span data-stu-id="d56d5-110">[ANSI joins on updates][ANSI joins on updates]</span></span>
* <span data-ttu-id="d56d5-111">[ANSI spojení na odstranění][ANSI joins on deletes]</span><span class="sxs-lookup"><span data-stu-id="d56d5-111">[ANSI joins on deletes][ANSI joins on deletes]</span></span>
* <span data-ttu-id="d56d5-112">[příkaz Merge][merge statement]</span><span class="sxs-lookup"><span data-stu-id="d56d5-112">[merge statement][merge statement]</span></span>
* <span data-ttu-id="d56d5-113">spojení mezi databáze</span><span class="sxs-lookup"><span data-stu-id="d56d5-113">cross-database joins</span></span>
* <span data-ttu-id="d56d5-114">[kurzory][cursors]</span><span class="sxs-lookup"><span data-stu-id="d56d5-114">[cursors][cursors]</span></span>
* <span data-ttu-id="d56d5-115">[PŘÍKAZ INSERT... EXEC –][INSERT..EXEC]</span><span class="sxs-lookup"><span data-stu-id="d56d5-115">[INSERT..EXEC][INSERT..EXEC]</span></span>
* <span data-ttu-id="d56d5-116">klauzuli Output</span><span class="sxs-lookup"><span data-stu-id="d56d5-116">output clause</span></span>
* <span data-ttu-id="d56d5-117">vložené uživatelem definované funkce</span><span class="sxs-lookup"><span data-stu-id="d56d5-117">inline user-defined functions</span></span>
* <span data-ttu-id="d56d5-118">vícepříkazové funkce</span><span class="sxs-lookup"><span data-stu-id="d56d5-118">multi-statement functions</span></span>
* [<span data-ttu-id="d56d5-119">běžných výrazech tabulky</span><span class="sxs-lookup"><span data-stu-id="d56d5-119">common table expressions</span></span>](#Common-table-expressions)
* <span data-ttu-id="d56d5-120">[rekurzivní běžných výrazech tabulky (CTE)] (#Recursive-common-table-expressions-(CTE)</span><span class="sxs-lookup"><span data-stu-id="d56d5-120">[recursive common table expressions (CTE)](#Recursive-common-table-expressions-(CTE)</span></span>
* <span data-ttu-id="d56d5-121">Funkce CLR a procedury</span><span class="sxs-lookup"><span data-stu-id="d56d5-121">CLR functions and procedures</span></span>
* <span data-ttu-id="d56d5-122">$partition – funkce</span><span class="sxs-lookup"><span data-stu-id="d56d5-122">$partition function</span></span>
* <span data-ttu-id="d56d5-123">proměnné tabulky</span><span class="sxs-lookup"><span data-stu-id="d56d5-123">table variables</span></span>
* <span data-ttu-id="d56d5-124">parametry s hodnotou tabulky</span><span class="sxs-lookup"><span data-stu-id="d56d5-124">table value parameters</span></span>
* <span data-ttu-id="d56d5-125">distribuované transakce</span><span class="sxs-lookup"><span data-stu-id="d56d5-125">distributed transactions</span></span>
* <span data-ttu-id="d56d5-126">potvrzení / vrácení zpět práce</span><span class="sxs-lookup"><span data-stu-id="d56d5-126">commit / rollback work</span></span>
* <span data-ttu-id="d56d5-127">Uložit transakce</span><span class="sxs-lookup"><span data-stu-id="d56d5-127">save transaction</span></span>
* <span data-ttu-id="d56d5-128">kontexty provádění (EXECUTE AS)</span><span class="sxs-lookup"><span data-stu-id="d56d5-128">execution contexts (EXECUTE AS)</span></span>
* <span data-ttu-id="d56d5-129">[s kumulativní klauzule Group by / datové krychle / nastaví možnosti seskupení][group by clause with rollup / cube / grouping sets options]</span><span class="sxs-lookup"><span data-stu-id="d56d5-129">[group by clause with rollup / cube / grouping sets options][group by clause with rollup / cube / grouping sets options]</span></span>
* <span data-ttu-id="d56d5-130">[vnořených úrovní nad rámec 8][nesting levels beyond 8]</span><span class="sxs-lookup"><span data-stu-id="d56d5-130">[nesting levels beyond 8][nesting levels beyond 8]</span></span>
* <span data-ttu-id="d56d5-131">[aktualizace prostřednictvím zobrazení][updating through views]</span><span class="sxs-lookup"><span data-stu-id="d56d5-131">[updating through views][updating through views]</span></span>
* <span data-ttu-id="d56d5-132">[použití vyberte pro přiřazení proměnné][use of select for variable assignment]</span><span class="sxs-lookup"><span data-stu-id="d56d5-132">[use of select for variable assignment][use of select for variable assignment]</span></span>
* <span data-ttu-id="d56d5-133">[žádné maximální datový typ pro dynamické řetězce SQL][no MAX data type for dynamic SQL strings]</span><span class="sxs-lookup"><span data-stu-id="d56d5-133">[no MAX data type for dynamic SQL strings][no MAX data type for dynamic SQL strings]</span></span>

<span data-ttu-id="d56d5-134">Naštěstí můžete být kolem fungovala většinu těchto omezení.</span><span class="sxs-lookup"><span data-stu-id="d56d5-134">Fortunately most of these limitations can be worked around.</span></span> <span data-ttu-id="d56d5-135">Vysvětlení najdete v článcích relevantní vývoj hello výše uvedené.</span><span class="sxs-lookup"><span data-stu-id="d56d5-135">Explanations are provided in hello relevant development articles referenced above.</span></span>

## <a name="supported-cte-features"></a><span data-ttu-id="d56d5-136">Podporované funkce CTE</span><span class="sxs-lookup"><span data-stu-id="d56d5-136">Supported CTE features</span></span>
<span data-ttu-id="d56d5-137">Běžných výrazech tabulky (odkazu Cte) jsou podporovány jen částečně v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="d56d5-137">Common table expressions (CTEs) are partially supported in SQL Data Warehouse.</span></span>  <span data-ttu-id="d56d5-138">aktuálně jsou podporovány následující funkce CTE Hello:</span><span class="sxs-lookup"><span data-stu-id="d56d5-138">hello following CTE features are currently supported:</span></span>

* <span data-ttu-id="d56d5-139">CTE lze zadat v příkazu SELECT.</span><span class="sxs-lookup"><span data-stu-id="d56d5-139">A CTE can be specified in a SELECT statement.</span></span>
* <span data-ttu-id="d56d5-140">CTE lze zadat v příkazu CREATE VIEW.</span><span class="sxs-lookup"><span data-stu-id="d56d5-140">A CTE can be specified in a CREATE VIEW statement.</span></span>
* <span data-ttu-id="d56d5-141">CTE lze zadat v příkazu vytvořte tabulku AS vyberte funkce CTAS ().</span><span class="sxs-lookup"><span data-stu-id="d56d5-141">A CTE can be specified in a CREATE TABLE AS SELECT (CTAS) statement.</span></span>
* <span data-ttu-id="d56d5-142">CTE lze zadat v příkazu vytvořit vzdálené tabulky AS vyberte (CRTAS).</span><span class="sxs-lookup"><span data-stu-id="d56d5-142">A CTE can be specified in a CREATE REMOTE TABLE AS SELECT (CRTAS) statement.</span></span>
* <span data-ttu-id="d56d5-143">CTE lze zadat v příkazu vytvořit externí tabulky AS vyberte (CETAS).</span><span class="sxs-lookup"><span data-stu-id="d56d5-143">A CTE can be specified in a CREATE EXTERNAL TABLE AS SELECT (CETAS) statement.</span></span>
* <span data-ttu-id="d56d5-144">Vzdálenou tabulku se může odkazovat z CTE.</span><span class="sxs-lookup"><span data-stu-id="d56d5-144">A remote table can be referenced from a CTE.</span></span>
* <span data-ttu-id="d56d5-145">Externí tabulku se může odkazovat z CTE.</span><span class="sxs-lookup"><span data-stu-id="d56d5-145">An external table can be referenced from a CTE.</span></span>
* <span data-ttu-id="d56d5-146">Několik definic CTE dotazu může být definován v CTE.</span><span class="sxs-lookup"><span data-stu-id="d56d5-146">Multiple CTE query definitions can be defined in a CTE.</span></span>

## <a name="cte-limitations"></a><span data-ttu-id="d56d5-147">Omezení CTE</span><span class="sxs-lookup"><span data-stu-id="d56d5-147">CTE Limitations</span></span>
<span data-ttu-id="d56d5-148">Běžných výrazech tabulky mají určitá omezení v SQL Data Warehouse, včetně:</span><span class="sxs-lookup"><span data-stu-id="d56d5-148">Common table expressions have some limitations in SQL Data Warehouse including:</span></span>

* <span data-ttu-id="d56d5-149">CTE musí následovat jediný příkaz SELECT.</span><span class="sxs-lookup"><span data-stu-id="d56d5-149">A CTE must be followed by a single SELECT statement.</span></span> <span data-ttu-id="d56d5-150">Příkaz INSERT, UPDATE, DELETE a MERGE příkazy nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="d56d5-150">INSERT, UPDATE, DELETE, and MERGE statements are not supported.</span></span>
* <span data-ttu-id="d56d5-151">Výraz běžné tabulky, který obsahuje odkazy na tooitself (rekurzivní výraz běžné tabulky) nepodporuje (viz níže části).</span><span class="sxs-lookup"><span data-stu-id="d56d5-151">A common table expression that includes references tooitself (a recursive common table expression) is not supported (see below section).</span></span>
* <span data-ttu-id="d56d5-152">Určení více než jeden s klauzulí v CTE není povoleno.</span><span class="sxs-lookup"><span data-stu-id="d56d5-152">Specifying more than one WITH clause in a CTE is not allowed.</span></span> <span data-ttu-id="d56d5-153">Například pokud CTE_query_definition obsahuje poddotaz, že poddotazu nesmí obsahovat vnořený s klauzulí, která definuje jiné CTE.</span><span class="sxs-lookup"><span data-stu-id="d56d5-153">For example, if a CTE_query_definition contains a subquery, that subquery cannot contain a nested WITH clause that defines another CTE.</span></span>
* <span data-ttu-id="d56d5-154">Klauzuli ORDER BY nelze použít v hello CTE_query_definition, s výjimkou, když je zadané klauzuli TOP.</span><span class="sxs-lookup"><span data-stu-id="d56d5-154">An ORDER BY clause cannot be used in hello CTE_query_definition, except when a TOP clause is specified.</span></span>
* <span data-ttu-id="d56d5-155">Pokud CTE se používá v příkazu, který je součástí dávky, příkaz hello dříve, než se musí následovat středníkem.</span><span class="sxs-lookup"><span data-stu-id="d56d5-155">When a CTE is used in a statement that is part of a batch, hello statement before it must be followed by a semicolon.</span></span>
* <span data-ttu-id="d56d5-156">Pokud se použije v příkazech připravené sp_prepare, odkazu Cte budou chovat hello stejným způsobem jako ostatní příkazů SELECT v PDW.</span><span class="sxs-lookup"><span data-stu-id="d56d5-156">When used in statements prepared by sp_prepare, CTEs will behave hello same way as other SELECT statements in PDW.</span></span> <span data-ttu-id="d56d5-157">Ale pokud odkazu Cte se používají jako součást CETAS připravené sp_prepare, hello chování můžete odložit ze systému SQL Server a další příkazy PDW kvůli hello způsob, jakým vazba je implementována pro sp_prepare.</span><span class="sxs-lookup"><span data-stu-id="d56d5-157">However, if CTEs are used as part of CETAS prepared by sp_prepare, hello behavior can defer from SQL Server and other PDW statements because of hello way binding is implemented for sp_prepare.</span></span> <span data-ttu-id="d56d5-158">Pokud vyberte odkazů CTE je pomocí nesprávného sloupce, který neexistuje v CTE, hello sp_prepare předá bez zjišťování hello chyba, že hello bude vyvolána chyba při proceduře sp_execute místo.</span><span class="sxs-lookup"><span data-stu-id="d56d5-158">If SELECT that references CTE is using a wrong column that does not exist in CTE, hello sp_prepare will pass without detecting hello error, but hello error will be thrown during sp_execute instead.</span></span>

## <a name="recursive-ctes"></a><span data-ttu-id="d56d5-159">Rekurzivního odkazu Cte</span><span class="sxs-lookup"><span data-stu-id="d56d5-159">Recursive CTEs</span></span>
<span data-ttu-id="d56d5-160">Rekurzivního odkazu Cte nejsou podporovány v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="d56d5-160">Recursive CTEs are not supported in SQL Data Warehouse.</span></span>  <span data-ttu-id="d56d5-161">migrace Hello rekurzivní CTE může být poněkud složité a nejlepší proces hello je toobreak do více kroků.</span><span class="sxs-lookup"><span data-stu-id="d56d5-161">hello migration of recursive CTE can be somewhat complex and hello best process is toobreak it into multiple steps.</span></span> <span data-ttu-id="d56d5-162">Obvykle můžete použít smyčku a podle iterace v mezičase dotazy hello rekurzivní naplňte dočasné tabulky.</span><span class="sxs-lookup"><span data-stu-id="d56d5-162">You can typically use a loop and populate a temporary table as you iterate over hello recursive interim queries.</span></span> <span data-ttu-id="d56d5-163">Jakmile se naplní hello dočasné tabulky můžete pak vrátit hello data jako jednu výslednou sadu.</span><span class="sxs-lookup"><span data-stu-id="d56d5-163">Once hello temporary table is populated you can then return hello data as a single result set.</span></span> <span data-ttu-id="d56d5-164">Podobný postup bylo použité toosolve `GROUP BY WITH CUBE` v hello [s kumulativní klauzule group by / datové krychle / nastaví možnosti seskupení] [ group by clause with rollup / cube / grouping sets options] článku.</span><span class="sxs-lookup"><span data-stu-id="d56d5-164">A similar approach has been used toosolve `GROUP BY WITH CUBE` in hello [group by clause with rollup / cube / grouping sets options][group by clause with rollup / cube / grouping sets options] article.</span></span>

## <a name="unsupported-system-functions"></a><span data-ttu-id="d56d5-165">Funkce nepodporované systému</span><span class="sxs-lookup"><span data-stu-id="d56d5-165">Unsupported system functions</span></span>
<span data-ttu-id="d56d5-166">Existují také některé funkce systému, které nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="d56d5-166">There are also some system functions that are not supported.</span></span> <span data-ttu-id="d56d5-167">Některé z hello hlavní ty, které jsou obvykle je možné použít v datových skladů jsou:</span><span class="sxs-lookup"><span data-stu-id="d56d5-167">Some of hello main ones you might typically find used in data warehousing are:</span></span>

* <span data-ttu-id="d56d5-168">NEWSEQUENTIALID()</span><span class="sxs-lookup"><span data-stu-id="d56d5-168">NEWSEQUENTIALID()</span></span>
* <span data-ttu-id="d56d5-169">@@NESTLEVEL()</span><span class="sxs-lookup"><span data-stu-id="d56d5-169">@@NESTLEVEL()</span></span>
* <span data-ttu-id="d56d5-170">@@IDENTITY()</span><span class="sxs-lookup"><span data-stu-id="d56d5-170">@@IDENTITY()</span></span>
* <span data-ttu-id="d56d5-171">@@ROWCOUNT()</span><span class="sxs-lookup"><span data-stu-id="d56d5-171">@@ROWCOUNT()</span></span>
* <span data-ttu-id="d56d5-172">ROWCOUNT_BIG</span><span class="sxs-lookup"><span data-stu-id="d56d5-172">ROWCOUNT_BIG</span></span>
* <span data-ttu-id="d56d5-173">ERROR_LINE()</span><span class="sxs-lookup"><span data-stu-id="d56d5-173">ERROR_LINE()</span></span>

<span data-ttu-id="d56d5-174">Některé z těchto problémů můžete fungovala kolem.</span><span class="sxs-lookup"><span data-stu-id="d56d5-174">Some of these issues can be worked around.</span></span>

## <a name="rowcount-workaround"></a><span data-ttu-id="d56d5-175">@@ROWCOUNT alternativní řešení</span><span class="sxs-lookup"><span data-stu-id="d56d5-175">@@ROWCOUNT workaround</span></span>
<span data-ttu-id="d56d5-176">toowork kolem nedostatečná podpora pro @@ROWCOUNT, vytvoření uložené procedury, která bude načítat hello poslední počet řádků z sys.dm_pdw_request_steps a potom spusťte `EXEC LastRowCount` po příkaz DML.</span><span class="sxs-lookup"><span data-stu-id="d56d5-176">toowork around lack of support for @@ROWCOUNT, create a stored procedure that will retrieve hello last row count from sys.dm_pdw_request_steps and then execute `EXEC LastRowCount` after a DML statement.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="d56d5-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d56d5-177">Next steps</span></span>
<span data-ttu-id="d56d5-178">Úplný seznam všech podporovaných příkazů T-SQL najdete v tématu [Transact-SQL témata][Transact-SQL topics].</span><span class="sxs-lookup"><span data-stu-id="d56d5-178">For a complete list of all supported T-SQL statements, see [Transact-SQL topics][Transact-SQL topics].</span></span>

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
