---
title: "Databáze úroveň kompatibility 130 – Azure SQL Database | Microsoft Docs"
description: "V tomto článku jsme prozkoumejte výhody spuštění vaší databázi SQL Azure s úrovní kompatibility 130 a využívat výhod nový Optimalizátor dotazů a funkce procesoru dotazů. Také jsme adres nežádoucí vliv na výkon dotazů pro existující aplikace SQL."
services: sql-database
documentationcenter: 
author: alainlissoir
manager: jhubbard
editor: 
ms.assetid: 8619f90b-7516-46dc-9885-98429add0053
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.topic: article
ms.date: 08/08/2016
ms.author: alainl
ms.openlocfilehash: c08c0690df4f389416e4ed2e2df2dbb72d6fd567
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="improved-query-performance-with-compatibility-level-130-in-azure-sql-database"></a><span data-ttu-id="12624-104">Zvýšení výkonu dotazů s kompatibilitou úrovně 130 ve službě Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="12624-104">Improved query performance with compatibility Level 130 in Azure SQL Database</span></span>
<span data-ttu-id="12624-105">Databáze SQL Azure běží transparentně stovky tisíc databází na mnoha různých kompatibility úrovních, zachování a zajištění zpětné kompatibility na odpovídající verzi systému Microsoft SQL Server pro všechny své zákazníky!</span><span class="sxs-lookup"><span data-stu-id="12624-105">Azure SQL Database is running transparently hundreds of thousands of databases at many different compatibility levels, preserving and guaranteeing the backward compatibility to the corresponding version of Microsoft SQL Server for all its customers!</span></span>

<span data-ttu-id="12624-106">V tomto článku jsme prozkoumejte výhody spuštění vaše připojení databáze SQL Azure s úrovní kompatibility 130 a využívat výhod nový Optimalizátor dotazů a funkce procesoru dotazů.</span><span class="sxs-lookup"><span data-stu-id="12624-106">In this article, we explore the benefits of running your Azure SQL Databse at compatibility level 130, and leveraging the benefits of the new query optimizer and query processor features.</span></span> <span data-ttu-id="12624-107">Také jsme adres nežádoucí vliv na výkon dotazů pro existující aplikace SQL.</span><span class="sxs-lookup"><span data-stu-id="12624-107">We also address the possible side-effects on the query performance for the existing SQL applications.</span></span>

<span data-ttu-id="12624-108">Připomínáme historie zarovnání verze SQL výchozí úrovně kompatibility jsou následující:</span><span class="sxs-lookup"><span data-stu-id="12624-108">As a reminder of history, the alignment of SQL versions to default compatibility levels are as follows:</span></span>

* <span data-ttu-id="12624-109">100: v systému SQL Server 2008 a Azure SQL Database verze 11.</span><span class="sxs-lookup"><span data-stu-id="12624-109">100: in SQL Server 2008 and Azure SQL Database V11.</span></span>
* <span data-ttu-id="12624-110">110: v systému SQL Server 2012 a Azure SQL Database verze 11.</span><span class="sxs-lookup"><span data-stu-id="12624-110">110: in SQL Server 2012 and Azure SQL Database V11.</span></span>
* <span data-ttu-id="12624-111">120: v systému SQL Server 2014 a Azure SQL Database verze 12.</span><span class="sxs-lookup"><span data-stu-id="12624-111">120: in SQL Server 2014 and Azure SQL Database V12.</span></span>
* <span data-ttu-id="12624-112">130: v systému SQL Server 2016 a Azure SQL databáze verze 12.</span><span class="sxs-lookup"><span data-stu-id="12624-112">130: in SQL Server 2016 and Azure SQL Database V12.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="12624-113">Počínaje **mid června 2016**, v databázi SQL Azure, bude výchozí úroveň kompatibility 130 místo 120 pro **nově vytvořený** databáze.</span><span class="sxs-lookup"><span data-stu-id="12624-113">Starting in **mid-June 2016**, in Azure SQL Database, the default compatibility level will be 130 instead of 120 for **newly created** databases.</span></span>
> 
> <span data-ttu-id="12624-114">Databáze vytvořené před mid června 2016 se *není* mít vliv a bude udržovat své aktuální úroveň kompatibility (100, 110 nebo 120).</span><span class="sxs-lookup"><span data-stu-id="12624-114">Databases created before mid-June 2016 will *not* be affected, and will maintain their current compatibility level (100, 110, or 120).</span></span> <span data-ttu-id="12624-115">Databáze, které se migrovat z verzí Azure SQL Database verze 11 na verzi 12, bude mít úrovní kompatibility 110 nebo 100.</span><span class="sxs-lookup"><span data-stu-id="12624-115">Databases that migrated from Azure SQL Database version V11 to V12 will have a compatibility level of either 100 or 110.</span></span> 
> 

## <a name="about-compatibility-level-130"></a><span data-ttu-id="12624-116">O úroveň kompatibility 130</span><span class="sxs-lookup"><span data-stu-id="12624-116">About compatibility level 130</span></span>
<span data-ttu-id="12624-117">První Pokud chcete vědět, aktuální úroveň kompatibility databáze, spusťte následující příkaz Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="12624-117">First, if you want to know the current compatibility level of your database, execute the following Transact-SQL statement.</span></span>

```
SELECT compatibility_level
    FROM sys.databases
    WHERE name = '<YOUR DATABASE_NAME>’;
```


<span data-ttu-id="12624-118">Před provedením této změny na úroveň 130 pro **nově** vytvořené databáze, můžeme zkontrolujte co tato změna se točí kolem prostřednictvím příklady velmi základního dotazu a v tématu Jak každý, kdo může těžit z něj.</span><span class="sxs-lookup"><span data-stu-id="12624-118">Before this change to level 130 happens for **newly** created databases, let’s review what this change is all about through some very basic query examples, and see how anyone can benefit from it.</span></span>

<span data-ttu-id="12624-119">Zpracování dotazu v relační databáze může být velmi složité a může vést k mnoha vědy a matematika pochopit rozhodnutích při návrhu vyplývajících a chování.</span><span class="sxs-lookup"><span data-stu-id="12624-119">Query processing in relational databases can be very complex and can lead to lots of computer science and mathematics to understand the inherent design choices and behaviors.</span></span> <span data-ttu-id="12624-120">V tomto dokumentu obsah záměrně zjednodušenou zajistěte, aby každý, kdo má minimální technické základní můžete porozumět dopadu Změna úrovně kompatibility a určit, jak se může hodit aplikace.</span><span class="sxs-lookup"><span data-stu-id="12624-120">In this document, the content has been intentionally simplified to ensure that anyone with some minimum technical background can understand the impact of the compatibility level change and determine how it can benefit applications.</span></span>

<span data-ttu-id="12624-121">Umožňuje rychlý podívejte se na úroveň kompatibility 130 přináší v tabulce.</span><span class="sxs-lookup"><span data-stu-id="12624-121">Let’s have a quick look at what the compatibility level 130 brings at the table.</span></span>  <span data-ttu-id="12624-122">Můžete najít další podrobnosti o [změnit úroveň kompatibility databáze (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx), ale zde je uveden krátký:</span><span class="sxs-lookup"><span data-stu-id="12624-122">You can find more details at [ALTER DATABASE Compatibility Level (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx), but here is a short summary:</span></span>

* <span data-ttu-id="12624-123">Operace Insert příkazu Insert vyberte může být Vícevláknová nebo může mít paralelní plán při před jednovláknové tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="12624-123">The Insert operation of an Insert-select statement can be multi-threaded or can have a parallel plan, while before this operation was single-threaded.</span></span>
* <span data-ttu-id="12624-124">Paměť optimalizovaný tabulka a tabulka proměnné dotazy mohou mít nyní paralelní plány při předtím, než tato operace se také jednovláknové.</span><span class="sxs-lookup"><span data-stu-id="12624-124">Memory Optimized table and table variables queries can now have parallel plans, while before this operation was also single-threaded .</span></span>
* <span data-ttu-id="12624-125">Statistiky pro paměťově optimalizované tabulky se dá teď vzorkovat a jsou automaticky aktualizovány.</span><span class="sxs-lookup"><span data-stu-id="12624-125">Statistics for Memory Optimized table can now be sampled and are auto-updated.</span></span> <span data-ttu-id="12624-126">V tématu [co je nového ve službě databázového stroje: OLTP v paměti](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="12624-126">See [What's New in Database Engine: In-Memory OLTP](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) for more details.</span></span>
* <span data-ttu-id="12624-127">V režimu dávky nebo s režimu Row změny s indexy úložiště sloupců</span><span class="sxs-lookup"><span data-stu-id="12624-127">Batch mode v/s Row Mode changes with Column Store indexes</span></span>
  * <span data-ttu-id="12624-128">Řazení v tabulce s indexem úložiště sloupce jsou nyní v dávkovém režimu.</span><span class="sxs-lookup"><span data-stu-id="12624-128">Sorts on a table with a Column Store index are now in batch mode.</span></span>
  * <span data-ttu-id="12624-129">Agregace oddílová nyní pracovat v dávkovém režimu například TSQL funkce LAG nebo realizace příkazy.</span><span class="sxs-lookup"><span data-stu-id="12624-129">Windowing aggregates now operate in batch mode such as TSQL LAG/LEAD statements.</span></span>
  * <span data-ttu-id="12624-130">Dotazy na úložiště sloupce tabulky s více distinct klauzulí pracovat v dávkovém režimu.</span><span class="sxs-lookup"><span data-stu-id="12624-130">Queries on Column Store tables with Multiple distinct clauses operate in Batch mode.</span></span>
  * <span data-ttu-id="12624-131">Dotazy spuštěnými na úrovni DOP = 1 nebo sériového plán také spustit v dávkovém režimu.</span><span class="sxs-lookup"><span data-stu-id="12624-131">Queries running under DOP=1 or with a serial plan also execute in Batch Mode.</span></span>
* <span data-ttu-id="12624-132">Poslední vylepšení odhadu kardinality ve skutečnosti přicházejí s úrovní kompatibility 120, ale těch, které je spuštěna na nižší úrovni kompatibility (tj. 100 nebo 110), přesunutí kompatibility úroveň 130 se otevře také tato vylepšení a tyto může taky využívat dotazu výkon aplikací.</span><span class="sxs-lookup"><span data-stu-id="12624-132">Last, Cardinality Estimation improvements are actually coming with compatibility level 120, but for those of you running at a lower Compatibility level (i.e. 100, or 110), the move to compatibility level 130 will also bring these improvements, and these can also benefit the query performance of your applications.</span></span>

## <a name="practicing-compatibility-level-130"></a><span data-ttu-id="12624-133">Cvičení úroveň kompatibility 130</span><span class="sxs-lookup"><span data-stu-id="12624-133">Practicing compatibility level 130</span></span>
<span data-ttu-id="12624-134">První Pojďme některé tabulky, indexy a náhodná data vytvořená Pokud chcete provádět některé tyto nové funkce.</span><span class="sxs-lookup"><span data-stu-id="12624-134">First let’s get some tables, indexes and random data created to practice some of these new capabilities.</span></span> <span data-ttu-id="12624-135">Příklady skriptů TSQL mohou být provedeny v části SQL Server 2016 nebo v databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="12624-135">The TSQL script examples can be executed under SQL Server 2016, or under Azure SQL Database.</span></span> <span data-ttu-id="12624-136">Ale při vytváření databáze Azure SQL, ujistěte se, že zvolíte na minimální databázi P2 vzhledem k tomu, že budete potřebovat alespoň několik jader, který má povolení více vláken a proto těžit z těchto funkcí.</span><span class="sxs-lookup"><span data-stu-id="12624-136">However, when creating an Azure SQL database, make sure you choose at the minimum a P2 database because you need at least a couple of cores to allow multi-threading and therefore benefit from these features.</span></span>

```
-- Create a Premium P2 Database in Azure SQL Database

CREATE DATABASE MyTestDB
    (EDITION=’Premium’, SERVICE_OBJECTIVE=’P2′);
GO

-- Create 2 tables with a column store index on
-- the second one (only available on Premium databases)

CREATE TABLE T_source
    (Color varchar(10), c1 bigint, c2 bigint);

CREATE TABLE T_target
    (c1 bigint, c2 bigint);

CREATE CLUSTERED COLUMNSTORE INDEX CCI ON T_target;
GO

-- Insert few rows.

INSERT T_source VALUES
    (‘Blue’, RAND() * 100000, RAND() * 100000),
    (‘Yellow’, RAND() * 100000, RAND() * 100000),
    (‘Red’, RAND() * 100000, RAND() * 100000),
    (‘Green’, RAND() * 100000, RAND() * 100000),
    (‘Black’, RAND() * 100000, RAND() * 100000);

GO 200

INSERT T_source SELECT * FROM T_source;

GO 10
```


<span data-ttu-id="12624-137">Nyní Pojďme Podíváme se na některé z funkcí zpracování dotazu bude s úrovní kompatibility 130.</span><span class="sxs-lookup"><span data-stu-id="12624-137">Now, let’s have a look to some of the Query Processing features coming with compatibility level 130.</span></span>

## <a name="parallel-insert"></a><span data-ttu-id="12624-138">Paralelní vložení</span><span class="sxs-lookup"><span data-stu-id="12624-138">Parallel INSERT</span></span>
<span data-ttu-id="12624-139">Provádění níže TSQL příkazy provede operaci vložení v rámci úroveň kompatibility 120 a 130, který v uvedeném pořadí provede operaci vložení v jednom zařazování modelu (120) a ve model Vícevláknová (130).</span><span class="sxs-lookup"><span data-stu-id="12624-139">Executing the TSQL statements below executes the INSERT operation under compatibility level 120 and 130, which respectively executes the INSERT operation in a single threaded model (120), and in a multi-threaded model (130).</span></span>

```
-- Parallel INSERT … SELECT … in heap or CCI
-- is available under 130 only

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO 

-- The INSERT part is in serial

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130
GO

-- The INSERT part is in parallel

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

SET STATISTICS XML OFF;
```


<span data-ttu-id="12624-140">Tím, že požádá skutečnou plán dotazu, prohlížení jeho grafické vyjádření, její obsah XML, můžete určit, které mohutnost odhad funkce je v play.</span><span class="sxs-lookup"><span data-stu-id="12624-140">By requesting the actual the query plan, looking at its graphical representation or its XML content, you can determine which Cardinality Estimation function is at play.</span></span> <span data-ttu-id="12624-141">Prohlížení plánech-souběžného na obrázku 1, jsme jasně uvidí, že provádění vložit sloupec úložiště přejde z sériového portu v 120 na paralelní ve 130.</span><span class="sxs-lookup"><span data-stu-id="12624-141">Looking at the plans side-by-side on figure 1, we can clearly see that the Column Store INSERT execution goes from serial in 120 to parallel in 130.</span></span> <span data-ttu-id="12624-142">Všimněte si také, že změna ikony iterátor ve 130 plán zobrazující dva paralelní šipky skutečnost ilustrující, která teď provádění iterator je skutečně paralelní.</span><span class="sxs-lookup"><span data-stu-id="12624-142">Also, note that the change of the iterator icon in the 130 plan showing two parallel arrows, illustrating the fact that now the iterator execution is indeed parallel.</span></span> <span data-ttu-id="12624-143">Pokud máte velké operace INSERT na dokončení, paralelní provádění, spojen s číslem jádra, které máte k dispozici pro databázi, provede lépe; až 100krát rychlejší podle vaší konkrétní situace!</span><span class="sxs-lookup"><span data-stu-id="12624-143">If you have large INSERT operations to complete, the parallel execution, linked to the number of core you have at your disposal for the database, will perform better; up to a 100 times faster depending your situation!</span></span>

<span data-ttu-id="12624-144">*Obrázek 1: Operace INSERT se mění z sériové paralelní s kompatibilitou úrovně 130.*</span><span class="sxs-lookup"><span data-stu-id="12624-144">*Figure 1: INSERT operation changes from serial to parallel with compatibility level 130.*</span></span>

![Obrázek 1](./media/sql-database-compatibility-level-query-performance-130/figure-1.jpg)

## <a name="serial-batch-mode"></a><span data-ttu-id="12624-146">SÉRIOVÉ dávkovém režimu</span><span class="sxs-lookup"><span data-stu-id="12624-146">SERIAL Batch Mode</span></span>
<span data-ttu-id="12624-147">Podobně přesunutí úroveň kompatibility 130 při zpracování řádky dat umožňuje režimu zpracování dávky.</span><span class="sxs-lookup"><span data-stu-id="12624-147">Similarly, moving to compatibility level 130 when processing rows of data enables batch mode processing.</span></span> <span data-ttu-id="12624-148">Nejprve dávkových operací režimu jsou dostupná pouze při mají index úložiště sloupců na místě.</span><span class="sxs-lookup"><span data-stu-id="12624-148">First, batch mode operations  are only available when you have a column store index in place.</span></span> <span data-ttu-id="12624-149">Druhý dávky obvykle představuje ~ 900 řádky a používá kódu logiku, optimalizované pro vícejádrovými procesoru, vyšší propustnost paměti a přímo využívá komprimovaná data sloupce úložiště, pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="12624-149">Second, a batch typically represents ~900 rows, and uses a code logic optimized for multicore CPU, higher memory throughput and directly leverages the compressed data of the Column Store whenever possible.</span></span> <span data-ttu-id="12624-150">Za těchto podmínek SQL serveru 2016 zpracovávat ~ 900 řádků najednou, namísto 1 řádek v době, a v důsledku toho celkové režijní náklady operace nyní sdíleny celý batch, snižuje celkové náklady řádek.</span><span class="sxs-lookup"><span data-stu-id="12624-150">Under these conditions, SQL Server 2016 can process ~900 rows at once, instead of 1 row at the time, and as a consequence, the overall overhead cost of the operation is now shared by the entire batch, reducing the overall cost by row.</span></span> <span data-ttu-id="12624-151">Toto množství sdílených operací v kombinaci s kompresí úložiště sloupec v podstatě snižuje latenci zahrnutých v režimu vyberte dávkovou operaci.</span><span class="sxs-lookup"><span data-stu-id="12624-151">This shared amount of operations combined with the column store compression basically reduces the latency involved in a SELECT batch mode operation.</span></span> <span data-ttu-id="12624-152">Můžete najít další podrobnosti o úložišti sloupce a dávky režimu v [Průvodce indexy Columnstore](https://msdn.microsoft.com/library/gg492088.aspx).</span><span class="sxs-lookup"><span data-stu-id="12624-152">You can find more details about the column store and batch mode at [Columnstore Indexes Guide](https://msdn.microsoft.com/library/gg492088.aspx).</span></span>

```
-- Serial batch mode execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT (C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO 

-- The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="12624-153">Jako viditelná níže, pomocí sledování dotaz plány vedle sebe na obrázku 2, vidíme, že došlo ke změně režimu zpracování s úrovní kompatibility, a v důsledku toho při provádění dotazů v obou úroveň kompatibility zcela, vidíme, že většina Doba zpracování je stráví v režimu row (86 %) ve srovnání s dávkovém režimu (14 %), které byly zpracovány 2 dávky.</span><span class="sxs-lookup"><span data-stu-id="12624-153">As visible below, by observing the query plans side-by-side on figure 2, we can see that the processing mode has changed with the compatibility level, and as a consequence, when executing the queries in both compatibility level altogether, we can see that most of the processing time is spent in row mode (86%) compared to the batch mode (14%), where 2 batches have been processed.</span></span> <span data-ttu-id="12624-154">Zvýšit datovou sadu, výhodou zvýší.</span><span class="sxs-lookup"><span data-stu-id="12624-154">Increase the dataset, the benefit will increase.</span></span>

<span data-ttu-id="12624-155">*Obrázek 2: Vybrat ze sériové operace změny dávkovém režimu s úrovní kompatibility 130.*</span><span class="sxs-lookup"><span data-stu-id="12624-155">*Figure 2: SELECT operation changes from serial to batch mode with compatibility level 130.*</span></span>

![Obrázek 2](./media/sql-database-compatibility-level-query-performance-130/figure-2.jpg)

## <a name="batch-mode-on-sort-execution"></a><span data-ttu-id="12624-157">Dávkovém režimu na provedení řazení</span><span class="sxs-lookup"><span data-stu-id="12624-157">Batch mode on Sort Execution</span></span>
<span data-ttu-id="12624-158">Podobně jako výše, ale použité na operace řazení, přechod z režimu row (úroveň kompatibility 120) dávkovém režimu (úroveň kompatibility 130) zvyšuje výkon operace řazení ze stejných důvodů.</span><span class="sxs-lookup"><span data-stu-id="12624-158">Similar to the above, but applied to a sort operation, the transition from row mode (compatibility level 120) to batch mode (compatibility level 130) improves the performance of the SORT operation for the same reasons.</span></span>

```
-- Batch mode on sort execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

-- The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="12624-159">Viditelné-souběžného na obrázku 3, vidíme, že operace řazení v režimu row představuje 81 % nákladů, zatímco dávkovém režimu pouze představuje % 19 náklady (v uvedeném pořadí 81 % a % 56 na řazení samotné).</span><span class="sxs-lookup"><span data-stu-id="12624-159">Visible side-by-side on figure 3, we can see that the sort operation in row mode represents 81% of the cost, while the batch mode only represents 19% of the cost (respectively 81% and 56% on the sort itself).</span></span>

<span data-ttu-id="12624-160">*Obrázek 3: Operace řazení změny z řádku dávkovém režimu s úrovní kompatibility 130.*</span><span class="sxs-lookup"><span data-stu-id="12624-160">*Figure 3: SORT operation changes from row to batch mode with compatibility level 130.*</span></span>

![Obrázek 3](./media/sql-database-compatibility-level-query-performance-130/figure-3.png)

<span data-ttu-id="12624-162">Samozřejmě tyto ukázky obsahovat pouze desetitisíce řádků, který není nic při vyhledávání v datech dostupných v většina serverů SQL dní.</span><span class="sxs-lookup"><span data-stu-id="12624-162">Obviously, these samples only contain tens of thousands of rows, which is nothing when looking at the data available in most SQL Servers these days.</span></span> <span data-ttu-id="12624-163">Právě tyto proti miliony řádků místo projektu a tento fakt může projevit v několika minutách provádění ušetřena každý den čekající na povaze vašich úloh.</span><span class="sxs-lookup"><span data-stu-id="12624-163">Just project these against millions of rows instead, and this can translate in several minutes of execution spared every day pending the nature of your workload.</span></span>

## <a name="cardinality-estimation-ce-improvements"></a><span data-ttu-id="12624-164">Vylepšení mohutnost odhad (CE)</span><span class="sxs-lookup"><span data-stu-id="12624-164">Cardinality Estimation (CE) improvements</span></span>
<span data-ttu-id="12624-165">Zavedena v systému SQL Server 2014, všechny databáze s úrovní kompatibility 120 nebo vyšší bude využívat nové odhadu kardinality funkce.</span><span class="sxs-lookup"><span data-stu-id="12624-165">Introduced with SQL Server 2014, any database running at a compatibility level 120 or above will make use of the new Cardinality Estimation functionality.</span></span> <span data-ttu-id="12624-166">Mohutnost odhad je v podstatě logikou používanou k určení, jak bude systému SQL server spustit dotaz založený na jeho odhadované náklady.</span><span class="sxs-lookup"><span data-stu-id="12624-166">Essentially, cardinality estimation is the logic used to determine how SQL server will execute a query based on its estimated cost.</span></span> <span data-ttu-id="12624-167">Odhad se počítá pomocí vstup z statistiky přidružené objekty zahrnutých v tomto dotazu.</span><span class="sxs-lookup"><span data-stu-id="12624-167">The estimation is calculated using input from statistics associated with objects involved in that query.</span></span> <span data-ttu-id="12624-168">Prakticky, v hlavní funkce odhadu kardinality jsou odhady počet řádek společně s informacemi o distribuci hodnot počty odlišné hodnoty, a duplicitní počty obsažené v tabulky a objekty odkazovaná v dotazu.</span><span class="sxs-lookup"><span data-stu-id="12624-168">Practically, at a high-level, Cardinality Estimation functions are row count estimates along with information about the distribution of the values, distinct value counts, and duplicate counts contained in the tables and objects referenced in the query.</span></span> <span data-ttu-id="12624-169">Získání tyto odhady nesprávný, může způsobit nepotřebné vstupu/výstupu disku z důvodu nedostatku paměti uděluje (tj. v databázi TempDB úniky) nebo výběr sériové plán spuštění přes spouštění paralelní plán, a další.</span><span class="sxs-lookup"><span data-stu-id="12624-169">Getting these estimates wrong, can lead to unnecessary disk I/O due to insufficient memory grants (i.e. TempDB spills), or to a selection of a serial plan execution over a parallel plan execution, to name a few.</span></span> <span data-ttu-id="12624-170">Uzavření, nesprávný odhady může vést k snížení celkového výkonu spuštění dotazu.</span><span class="sxs-lookup"><span data-stu-id="12624-170">Conclusion, incorrect estimates can lead to an overall performance degradation of the query execution.</span></span> <span data-ttu-id="12624-171">Na druhé straně lepší odhady přesnější odhady vede k lepší spuštěních dotazu!</span><span class="sxs-lookup"><span data-stu-id="12624-171">On the other side, better estimates, more accurate estimates, leads to better query executions!</span></span>

<span data-ttu-id="12624-172">Jak je uvedeno nahoře, optimalizace dotazu a odhady jsou komplexní záležitosti, ale pokud chcete další informace o plány dotazů a architektury odhadu kardinality, najdete v dokumentu v [optimalizace vaše plány dotazů mohutností SQL serveru 2014 Odhadu](https://msdn.microsoft.com/library/dn673537.aspx) pro podrobnější prohlídku.</span><span class="sxs-lookup"><span data-stu-id="12624-172">As mentioned before, query optimizations and estimates are a complex matter, but if you want to learn more about query plans and cardinality estimator, you can refer to the document at [Optimizing Your Query Plans with the SQL Server 2014 Cardinality Estimator](https://msdn.microsoft.com/library/dn673537.aspx) for a deeper dive.</span></span>

## <a name="which-cardinality-estimation-do-you-currently-use"></a><span data-ttu-id="12624-173">Které odhadu kardinality právě používáte?</span><span class="sxs-lookup"><span data-stu-id="12624-173">Which Cardinality Estimation do you currently use?</span></span>
<span data-ttu-id="12624-174">Chcete-li určit, pod které mohutnost odhad své dotazy běží, právě použijeme ukázky dotazů, níže.</span><span class="sxs-lookup"><span data-stu-id="12624-174">To determine under which Cardinality Estimation your queries are running, let’s just use the query samples below.</span></span> <span data-ttu-id="12624-175">Všimněte si, že tento první příklad budou spouštěny pod úrovní kompatibility 110, za použití staré mohutnost odhad funkce.</span><span class="sxs-lookup"><span data-stu-id="12624-175">Note that this first example will run under compatibility level 110, implying the use of the old Cardinality Estimation functions.</span></span>

```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 110;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="12624-176">Po dokončení provádění klikněte na odkaz XML a podívejte se na vlastnosti první iterator, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="12624-176">Once execution is complete, click on the XML link, and look at the properties of the first iterator as shown below.</span></span> <span data-ttu-id="12624-177">Poznámka: název vlastnosti, která je volána CardinalityEstimationModelVersion nastaveno na 70.</span><span class="sxs-lookup"><span data-stu-id="12624-177">Note the property name called CardinalityEstimationModelVersion currently set on 70.</span></span> <span data-ttu-id="12624-178">Neznamená to, že úroveň kompatibility databáze je nastavena na verzi SQL Server 7.0 (je nastavený na 110 jako viditelné ve výše uvedené příkazy TSQL), ale hodnota 70 jednoduše představuje starší verze funkce mohutnost odhad, která je k dispozici od verze SQL Server 7.0 které měl žádné hlavní revize až SQL Server 2014 (který se dodává s úrovní kompatibility 120).</span><span class="sxs-lookup"><span data-stu-id="12624-178">It does not mean that the database compatibility level is set to the SQL Server 7.0 version (it is set on 110 as visible in the TSQL statements above), but the value 70 simply represents the legacy Cardinality Estimation functionality available since SQL Server 7.0, which had no major revisions until SQL Server 2014 (which comes with a compatibility level of 120).</span></span>

<span data-ttu-id="12624-179">*Obrázek 4: CardinalityEstimationModelVersion nastavena na hodnotu 70 při použití úrovní kompatibility 110 nebo níže.*</span><span class="sxs-lookup"><span data-stu-id="12624-179">*Figure 4: The CardinalityEstimationModelVersion is set to 70 when using a compatibility level of 110 or below.*</span></span>

![Obrázek 4](./media/sql-database-compatibility-level-query-performance-130/figure-4.png)

<span data-ttu-id="12624-181">Alternativně můžete změnit úroveň kompatibility na 130 a zakázat používání nové funkce odhadu kardinality pomocí LEGACY_CARDINALITY_ESTIMATION nastavenou na ON se [ALTER DATABASE OBOR CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx).</span><span class="sxs-lookup"><span data-stu-id="12624-181">Alternatively, you can change the compatibility level to 130, and disable the use of the new Cardinality Estimation function by using the LEGACY_CARDINALITY_ESTIMATION set to ON with [ALTER DATABASE SCOPED CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx).</span></span> <span data-ttu-id="12624-182">Bude jím přesně stejný jako pomocí 110 z odhadu kardinality funkce hlediska, při použití nejnovější dotaz zpracování úroveň kompatibility.</span><span class="sxs-lookup"><span data-stu-id="12624-182">This will be exactly the same as using 110 from a Cardinality Estimation function point of view, while using the latest query processing compatibility level.</span></span> <span data-ttu-id="12624-183">To uděláte, můžete těžit z nový dotaz zpracování funkce přicházející s nejnovější úroveň kompatibility (tj. v dávkovém režimu), ale stále spoléhají na staré mohutnost odhad funkce v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="12624-183">Doing so, you can benefit from the new query processing features coming with the latest compatibility level (i.e. batch mode), but still rely on the old Cardinality Estimation functionality if necessary.</span></span>

```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="12624-184">Přesunutí jednoduše úroveň kompatibility 120 nebo 130 umožňuje nové funkce odhadu kardinality.</span><span class="sxs-lookup"><span data-stu-id="12624-184">Simply moving to the compatibility level 120 or 130 enables the new Cardinality Estimation functionality.</span></span> <span data-ttu-id="12624-185">V takovém případě výchozí CardinalityEstimationModelVersion bude nastavena odpovídajícím způsobem 120 nebo 130 jako zobrazené níže.</span><span class="sxs-lookup"><span data-stu-id="12624-185">In such a case, the default CardinalityEstimationModelVersion will be set accordingly to 120 or 130 as visible below.</span></span>

```
-- New CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="12624-186">*Obrázek 5: CardinalityEstimationModelVersion nastavena na 130 při použití 130 úroveň kompatibility.*</span><span class="sxs-lookup"><span data-stu-id="12624-186">*Figure 5: The CardinalityEstimationModelVersion is set to 130 when using a compatibility level of 130.*</span></span>

![Obrázek 5](./media/sql-database-compatibility-level-query-performance-130/figure-5.jpg)

## <a name="witnessing-the-cardinality-estimation-differences"></a><span data-ttu-id="12624-188">Mohutnost odhad rozdíly při jeho práci</span><span class="sxs-lookup"><span data-stu-id="12624-188">Witnessing the Cardinality Estimation differences</span></span>
<span data-ttu-id="12624-189">Teď umožňuje spustit trochu složitější dotaz zahrnující vnitřního spojení s klauzulí WHERE se některé predikáty a podíváme se na odhad počtu řádků z původního funkce odhadu kardinality nejdřív.</span><span class="sxs-lookup"><span data-stu-id="12624-189">Now, let’s run a slightly more complex query involving an INNER JOIN with a WHERE clause with some predicates, and let’s look at the row count estimate from the old Cardinality Estimation function first.</span></span>

```
-- Old CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="12624-190">Efektivní provádění tohoto dotazu vrátí 200,704 řádky, zatímco odhad řádek s původním mohutnost odhad funkce deklarací 194,284 řádků.</span><span class="sxs-lookup"><span data-stu-id="12624-190">Executing this query effectively returns 200,704 rows, while the row estimate with the old Cardinality Estimation functionality claims 194,284 rows.</span></span> <span data-ttu-id="12624-191">Samozřejmě jak je uvedená před, tyto řádků počet výsledků bude také záviset jak často jste spustili předchozí ukázky, která naplní ukázkové tabulky opakovaně v každé spuštění.</span><span class="sxs-lookup"><span data-stu-id="12624-191">Obviously, as said before, these row count results will also depend how often you ran the previous samples, which populates the sample tables over and over again at each run.</span></span> <span data-ttu-id="12624-192">Samozřejmě predikáty v dotazu bude také mít vliv na skutečný odhad kromě zajištění dostatečného obrazec tabulky, obsah, dat a jak tato data ve skutečnosti korelaci mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="12624-192">Obviously, the predicates in your query will also have an influence on the actual estimation aside from the table shape, data content, and how this data actually correlate with each other.</span></span>

<span data-ttu-id="12624-193">*Obrázek 6: Odhad počtu řádků vypnuté 194,284 nebo 6 000 řádků z 200,704 řádky očekává.*</span><span class="sxs-lookup"><span data-stu-id="12624-193">*Figure 6: The row count estimate is 194,284 or 6,000 rows off from the 200,704 rows expected.*</span></span>

![Obrázek 6](./media/sql-database-compatibility-level-query-performance-130/figure-6.jpg)

<span data-ttu-id="12624-195">Stejným způsobem můžeme teď spustit stejný dotaz s novými funkcemi odhadu kardinality.</span><span class="sxs-lookup"><span data-stu-id="12624-195">In the same way, let’s now execute the same query with the new Cardinality Estimation functionality.</span></span>

```
-- New CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="12624-196">Prohlížení níže, jsme teď zjistíte, že odhad řádek 202,877, nebo mnohem blíže a vyšší než původní odhadu kardinality.</span><span class="sxs-lookup"><span data-stu-id="12624-196">Looking at the below, we now see that the row estimate is 202,877, or much closer and higher than the old Cardinality Estimation.</span></span>

<span data-ttu-id="12624-197">*Obrázek 7: Odhad počtu řádků je nyní 202,877 místo 194,284.*</span><span class="sxs-lookup"><span data-stu-id="12624-197">*Figure 7: The row count estimate is now 202,877, instead of 194,284.*</span></span>

![Obrázek 7](./media/sql-database-compatibility-level-query-performance-130/figure-7.jpg)

<span data-ttu-id="12624-199">Ve skutečnosti sadu výsledků dotazu se 200,704 řádků (ale všechny závisí, jak často spouštět dotazy předchozí ukázky, ale je důležité, protože TSQL používá příkaz RAND(), skutečné hodnoty, vrátí se můžou lišit od prvního spuštění na další).</span><span class="sxs-lookup"><span data-stu-id="12624-199">In reality, the result set is 200,704 rows (but all of it depends how often you did run the queries of the previous samples, but more importantly, because the TSQL uses the RAND() statement, the actual values returned can vary from one run to the next).</span></span> <span data-ttu-id="12624-200">Proto v tomto konkrétním příkladu nové odhadu kardinality nepodporuje lépe daří v odhadnout počet řádků, protože je mnohem blíže 200,704, než 194,284 202,877!</span><span class="sxs-lookup"><span data-stu-id="12624-200">Therefore, in this particular example, the new Cardinality Estimation does a better job at estimating the number of rows because 202,877 is much closer to 200,704, than 194,284!</span></span> <span data-ttu-id="12624-201">Poslední, pokud změníte klauzule WHERE predikáty k rovnosti (místo ">" pro instanci), může to zkontrolujte odhad mezi starý a můžete získat novou funkci mohutnost i více lišit podle toho, kolik odpovídá.</span><span class="sxs-lookup"><span data-stu-id="12624-201">Last, if you change the WHERE clause predicates to equality (rather than “>” for instance), this could make the estimates between the old and new Cardinality function even more different, depending on how many matches you can get.</span></span>

<span data-ttu-id="12624-202">Samozřejmě v takovém případě se ~ 6000 řádků vypnout z skutečný počet nepředstavuje velké množství dat v některých situacích.</span><span class="sxs-lookup"><span data-stu-id="12624-202">Obviously, in this case, being ~6000 rows off from actual count does not represent a lot of data in some situations.</span></span> <span data-ttu-id="12624-203">Nyní, transpozice na miliony řádků napříč několika tabulek a složitější dotazy a v některých případech odhad může být vypnuto miliony řádky, a proto riziko výdej up plán spuštění nesprávný, nebo pro požadování vedoucí k databázi TempDB uděluje nedostatek paměti úniky a proto více vstupně-výstupních operací, jsou mnohem vyšší.</span><span class="sxs-lookup"><span data-stu-id="12624-203">Now, transpose this to millions of rows across several tables and more complex queries, and at times the estimate can be off by millions of rows , and therefore, the risk of picking-up the wrong execution plan, or requesting insufficient memory grants leading to TempDB spills, and so more I/O, are much higher.</span></span>

<span data-ttu-id="12624-204">Pokud máte možnost, praxi toto porovnání s nejčastější dotazy a datové sady a uvidíte sami jak moc některé odhady starý a nový jsou ovlivněni, zatímco některé právě může stát další vypnout z když ve skutečnosti nebo některé jiné stačí jednoduše blíže k počty skutečné řádků ve skutečnosti vrátila v sadách výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="12624-204">If you have the opportunity, practice this comparison with your most typical queries and datasets, and see for yourself by how much some of the old and new estimates are affected, while some could just become more off from the reality, or some others just simply closer to the actual row counts actually returned in the result sets.</span></span> <span data-ttu-id="12624-205">Všechny záviset tvaru své dotazy, vlastnosti databáze Azure SQL, povaze a velikosti vaše datové sady a statistiky o je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="12624-205">All of it will depend of the shape of your queries, the Azure SQL database characteristics, the nature and the size of your datasets, and the statistics available about them.</span></span> <span data-ttu-id="12624-206">Pokud jste právě vytvořili instanci databáze SQL Azure, bude mít Optimalizátor dotazů k sestavení své znalosti od začátku namísto opakovaného použití statistiky, které se skládají z předchozích spuštění dotazu.</span><span class="sxs-lookup"><span data-stu-id="12624-206">If you just created your Azure SQL Database instance, the query optimizer will have to build its knowledge from scratch instead of reusing statistics made of the previous query runs.</span></span> <span data-ttu-id="12624-207">Ano odhad jsou velmi kontextové a téměř specifické pro každé situaci serveru a aplikace.</span><span class="sxs-lookup"><span data-stu-id="12624-207">So, the estimates are very contextual and almost specific to every server and application situation.</span></span> <span data-ttu-id="12624-208">Je důležitým aspektem pamatovat!</span><span class="sxs-lookup"><span data-stu-id="12624-208">It is an important aspect to keep in mind!</span></span>

## <a name="some-considerations-to-take-into-account"></a><span data-ttu-id="12624-209">Některé aspekty vzít v úvahu</span><span class="sxs-lookup"><span data-stu-id="12624-209">Some considerations to take into account</span></span>
<span data-ttu-id="12624-210">I když většina úloh by těžit z úrovně kompatibility 130, před přijetím úroveň kompatibility pro produkční prostředí, máte v podstatě 3 možnosti:</span><span class="sxs-lookup"><span data-stu-id="12624-210">Although most workloads would benefit from the compatibility level 130, before you adopting the compatibility level for your production environment, you basically have 3 options:</span></span>

1. <span data-ttu-id="12624-211">Přesunout na úroveň kompatibility 130 a v tématu Jak provádět věcí.</span><span class="sxs-lookup"><span data-stu-id="12624-211">You move to compatibility level 130, and see how things perform.</span></span> <span data-ttu-id="12624-212">V případě, že jste si všimli některé regresí, stačí jednoduše nastavit úroveň kompatibility zpět na jeho původní úroveň, nebo se zachovat 130 a pouze zpětné odhadu kardinality zpět do režimu starší verze (jak je popsáno výše, to samostatně může vyřešit problém).</span><span class="sxs-lookup"><span data-stu-id="12624-212">In case you notice some regressions, you just simply set the compatibility level back to its original level, or keep 130, and only reverse the Cardinality Estimation back to the legacy mode (As explained above, this alone could address the issue).</span></span>
2. <span data-ttu-id="12624-213">Důkladně otestovat existující aplikace podobné produkční zatížení, vyladit a ověření výkonu před přechodem do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="12624-213">You thoroughly test your existing applications under similar production load, fine tune, and validate the performance before going to production.</span></span> <span data-ttu-id="12624-214">V případě problémů stejné jako výše, můžete vždy přejděte zpět na původní úroveň kompatibility, nebo jednoduše odhadu kardinality vrátit zpět do režimu starší verze.</span><span class="sxs-lookup"><span data-stu-id="12624-214">In case of issues, same as above, you can always go back to the original compatibility level, or simply reverse the Cardinality Estimation back to the legacy mode.</span></span>
3. <span data-ttu-id="12624-215">Jako poslední možnost a nejnovější způsobu, jak vyřešit tyto otázky je využít úložiště dotazů.</span><span class="sxs-lookup"><span data-stu-id="12624-215">As a final option, and the most recent way to address these questions, is to leverage the Query Store.</span></span> <span data-ttu-id="12624-216">To je doporučená možnost dnešní!</span><span class="sxs-lookup"><span data-stu-id="12624-216">That’s today’s recommended option!</span></span> <span data-ttu-id="12624-217">Pomůže analýzy vašich dotazů v rámci kompatibility úrovně 120 nebo pod versus 130, nelze doporučujeme dostatečně používat úložiště dotazů.</span><span class="sxs-lookup"><span data-stu-id="12624-217">To assist the analysis of your queries under compatibility level 120 or below versus 130, we cannot encourage you enough to use Query Store.</span></span> <span data-ttu-id="12624-218">Úložiště dotazů je k dispozici v nejnovější verzi Azure SQL Database V12 a je určený vám pomůže při řešení potíží s výkonem dotazu.</span><span class="sxs-lookup"><span data-stu-id="12624-218">Query Store is available with the latest version of Azure SQL Database V12, and it’s designed to help you with query performance troubleshooting.</span></span> <span data-ttu-id="12624-219">Úložiště dotazů si můžete představit jako záznam dat pro vaši databázi shromažďování a nabízí podrobné historické informace o všech dotazů.</span><span class="sxs-lookup"><span data-stu-id="12624-219">Think of the Query Store as a flight data recorder for your database collecting and presenting detailed historic information about all queries.</span></span> <span data-ttu-id="12624-220">To výrazně zjednodušuje forenzních výkon snížením doba diagnostikovat a vyřešit problémy.</span><span class="sxs-lookup"><span data-stu-id="12624-220">This greatly simplifies performance forensics by reducing the time to diagnose and resolve issues.</span></span> <span data-ttu-id="12624-221">Další informace najdete [úložiště dotazů: záznam dat pro vaši databázi](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).</span><span class="sxs-lookup"><span data-stu-id="12624-221">You can find more information at [Query Store: A flight data recorder for your database](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).</span></span>

<span data-ttu-id="12624-222">Na vysoké úrovně, pokud již máte sadu databází s úrovní kompatibility 120 nebo níže a chcete přesunout některé z nich na 130, nebo protože vaše úlohy automaticky zřizovat nové databáze, které budou brzy se ve výchozím nastavení k 130, zvažte prosím Tady:</span><span class="sxs-lookup"><span data-stu-id="12624-222">At the high-level, if you already have a set of databases running at compatibility level 120 or below, and plan to move some of them to 130, or because your workload automatically provision new databases that will be soon be set by default to 130, please consider the followings:</span></span>

* <span data-ttu-id="12624-223">Před změnou na novou úroveň kompatibility v produkčním prostředí, povolte úložiště dotazů.</span><span class="sxs-lookup"><span data-stu-id="12624-223">Before changing to the new compatibility level in production, enable Query Store.</span></span> <span data-ttu-id="12624-224">Můžete se podívat do [změnit režim kompatibility databáze a používat úložiště dotazů](https://msdn.microsoft.com/library/bb895281.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="12624-224">You can refer to [Change the Database Compatibility Mode and Use the Query Store](https://msdn.microsoft.com/library/bb895281.aspx) for more information.</span></span>
* <span data-ttu-id="12624-225">V dalším kroku otestujte všechny kritické úlohy pomocí reprezentativní dat a dotazy z prostředí produkčního prostředí a porovnání výkonu zaznamenal a jako hlášené pomocí úložiště dotazů.</span><span class="sxs-lookup"><span data-stu-id="12624-225">Next, test all critical workloads using representative data and queries of a production-like environment, and compare the performance experienced and as reported by Query Store.</span></span> <span data-ttu-id="12624-226">Pokud dojde k některé regresí, můžete identifikovat regressed dotazy s úložiště dotazů a použít plán vynucení možnost úložiště dotazů (neboli plán Připnutí).</span><span class="sxs-lookup"><span data-stu-id="12624-226">If you experience some regressions, you can identify the regressed queries with the Query Store and use the plan forcing option from Query Store (aka plan pinning).</span></span> <span data-ttu-id="12624-227">V takovém případě můžete platností zůstat s úrovní kompatibility 130 a používat plán bývalé dotazu na návrh úložiště dotazů.</span><span class="sxs-lookup"><span data-stu-id="12624-227">In such a case, you definitively stay with the compatibility level 130, and use the former query plan as suggested by the Query Store.</span></span>
* <span data-ttu-id="12624-228">Pokud chcete využívat nové funkce a možnosti Azure SQL Database (který je spuštěn SQL Server 2016), ale jsou citlivé na změny uvést do režimu podle úrovně kompatibility 130, jako poslední možnost, zvažte vynucení úroveň kompatibility zpět na úroveň který vyhovuje vaše úlohy pomocí příkazu ALTER DATABASE.</span><span class="sxs-lookup"><span data-stu-id="12624-228">If you want to leverage new features and capabilities of Azure SQL Database (which is running SQL Server 2016), but are sensitive to changes brought by the compatibility level 130, as a last resort, you could consider forcing the compatibility level back to the level that suits your workload by using an ALTER DATABASE statement.</span></span> <span data-ttu-id="12624-229">Ale nejprve, mějte na paměti, úložiště dotazů plánu Připnutí možnost je nejlepší možnost, protože není pomocí 130 je v podstatě zachování na úrovni funkcí starší verze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="12624-229">But first, be aware that the Query Store plan pinning option is your best option because not using 130 is basically staying at the functionality level of an older SQL Server version.</span></span>
* <span data-ttu-id="12624-230">Pokud máte víceklientské aplikace pokrývání uzlů více databází, bude pravděpodobně nutné aktualizovat zřizování logiku databází zajistit úroveň kompatibility konzistentní napříč všechny databáze; starý a nově zřízeného ty.</span><span class="sxs-lookup"><span data-stu-id="12624-230">If you have multitenant applications spanning multiple databases, it may be necessary to update the provisioning logic of your databases to ensure a consistent compatibility level across all databases; old and newly provisioned ones.</span></span> <span data-ttu-id="12624-231">Výkonu úloh aplikace může být citlivá s ohledem na skutečnost, že některé databáze, které jsou spuštěné v různých kompatibility úrovně a proto může vyžadovat kompatibility úrovně konzistence napříč všechny databáze chcete-li poskytovat stejné prostředí k vašim zákazníkům všechny napříč panelu.</span><span class="sxs-lookup"><span data-stu-id="12624-231">Your application workload performance could be sensitive to the fact that some databases are running at different compatibility levels, and therefore, compatibility level consistency across any database could be required in order to provide the same experience to your customers all across the board.</span></span> <span data-ttu-id="12624-232">Všimněte si, že se nejedná o pověření, ve skutečnosti závisí na tom, jak vaše aplikace je ovlivňován úroveň kompatibility.</span><span class="sxs-lookup"><span data-stu-id="12624-232">Note that it is not a mandate, it really depends on how your application is affected by the compatibility level.</span></span>
* <span data-ttu-id="12624-233">Poslední, pokud jde o odhad mohutnost a stejně jako změna úrovně kompatibility, než budete pokračovat v produkčním prostředí, se doporučuje k testování vaše produkční úlohy v rámci nové podmínky k určení, pokud vaše aplikace výhody z Vylepšení odhadu kardinality.</span><span class="sxs-lookup"><span data-stu-id="12624-233">Last, regarding the Cardinality Estimation, and just like changing the compatibility level, before proceeding in production, it is recommended to test your production workload under the new conditions to determine if your application benefits from the Cardinality Estimation improvements.</span></span>

## <a name="conclusion"></a><span data-ttu-id="12624-234">Závěr</span><span class="sxs-lookup"><span data-stu-id="12624-234">Conclusion</span></span>
<span data-ttu-id="12624-235">Používání Azure SQL Database, abyste mohli využívat výhod všechny vylepšení SQL Server 2016 jasně zlepšit spuštěních vašeho dotazu.</span><span class="sxs-lookup"><span data-stu-id="12624-235">Using Azure SQL Database to benefit from all SQL Server 2016 enhancements can clearly improve your query executions.</span></span> <span data-ttu-id="12624-236">Stejně jako-je!</span><span class="sxs-lookup"><span data-stu-id="12624-236">Just as-is!</span></span> <span data-ttu-id="12624-237">Samozřejmě podobně jako u jakékoli nové funkce, je třeba provést řádné vyhodnocení určit přesnou podmínky, za kterých vaše databáze zatížení funguje nejvhodnější.</span><span class="sxs-lookup"><span data-stu-id="12624-237">Of course, like any new feature, a proper evaluation must be done to determine the exact conditions under which your database workload operates the best.</span></span> <span data-ttu-id="12624-238">Prostředí ukazuje, že většina úloh se očekává alespoň běh transparentně úroveň kompatibility 130, při využití nový dotaz zpracování funkce a novou odhadu kardinality.</span><span class="sxs-lookup"><span data-stu-id="12624-238">Experience shows that most workload are expected to at least run transparently under compatibility level 130, while leveraging new query processing functions, and new Cardinality Estimation.</span></span> <span data-ttu-id="12624-239">Který uvedená reálně jsou vždy některé výjimky a provádění správné splatnosti opatrností je důležité určit, kolik můžete využívat výhod těchto vylepšení posouzení.</span><span class="sxs-lookup"><span data-stu-id="12624-239">That said, realistically, there are always some exceptions and doing proper due diligence is an important assessment to determine how much you can benefit from these enhancements.</span></span> <span data-ttu-id="12624-240">A znovu, můžou být úložiště dotazů skvělé pomáhá při provádění této práce!</span><span class="sxs-lookup"><span data-stu-id="12624-240">And again, the Query Store can be of a great help in doing this work!</span></span>

<span data-ttu-id="12624-241">S rozvojem zpracovaní SQL Azure, můžete očekávat, že úroveň kompatibility 140 v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="12624-241">As SQL Azure evolves, you can expect a compatibility level 140 in the future.</span></span> <span data-ttu-id="12624-242">Kdy je vhodné čas, spustíme posuzování co se otevře tato úroveň kompatibilitou v budoucnu 140, stejně jako jsme stručně tady popisovaných jakou úroveň kompatibility 130 je přináší dnes.</span><span class="sxs-lookup"><span data-stu-id="12624-242">When time is appropriate, we will start talking about what this future compatibility level 140 will bring, just as we briefly discussed here what compatibility level 130 is bringing today.</span></span>

<span data-ttu-id="12624-243">Teď, můžeme není zapomněli, od června 2016, Azure SQL Database se změní výchozí úroveň kompatibility z 120 na 130 pro nově vytvořené databáze.</span><span class="sxs-lookup"><span data-stu-id="12624-243">For now, let’s not forget, starting June 2016, Azure SQL Database will change the default compatibility level from 120 to 130 for newly created databases.</span></span> <span data-ttu-id="12624-244">Mějte na paměti!</span><span class="sxs-lookup"><span data-stu-id="12624-244">Be aware!</span></span>

## <a name="references"></a><span data-ttu-id="12624-245">Odkazy</span><span class="sxs-lookup"><span data-stu-id="12624-245">References</span></span>
* [<span data-ttu-id="12624-246">Co je nového v databázový stroj</span><span class="sxs-lookup"><span data-stu-id="12624-246">What’s New in Database Engine</span></span>](https://msdn.microsoft.com/library/bb510411.aspx#InMemory)
* [<span data-ttu-id="12624-247">Blog: Úložiště dotazů: záznam dat pro vaši databázi pomocí Borko Novakovic, 8. června 2016</span><span class="sxs-lookup"><span data-stu-id="12624-247">Blog: Query Store: A flight data recorder for your database, by Borko Novakovic, June 8 2016</span></span>](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/)
* [<span data-ttu-id="12624-248">Úroveň kompatibility databáze ALTER (Transact-SQL)</span><span class="sxs-lookup"><span data-stu-id="12624-248">ALTER DATABASE Compatibility Level (Transact-SQL)</span></span>](https://msdn.microsoft.com/library/bb510680.aspx)
* [<span data-ttu-id="12624-249">PŘÍKAZ ALTER VYMEZENÁ KONFIGURACE DATABÁZE</span><span class="sxs-lookup"><span data-stu-id="12624-249">ALTER DATABASE SCOPED CONFIGURATION</span></span>](https://msdn.microsoft.com/library/mt629158.aspx)
* [<span data-ttu-id="12624-250">Úroveň kompatibility 130 pro Azure SQL Database verze 12</span><span class="sxs-lookup"><span data-stu-id="12624-250">Compatibility Level 130 for Azure SQL Database V12</span></span>](https://azure.microsoft.com/updates/compatibility-level-130-for-azure-sql-database-v12/)
* [<span data-ttu-id="12624-251">Optimalizace dotazu plány s SQL serverem 2014 architektury odhadu kardinality</span><span class="sxs-lookup"><span data-stu-id="12624-251">Optimizing Your Query Plans with the SQL Server 2014 Cardinality Estimator</span></span>](https://msdn.microsoft.com/library/dn673537.aspx)
* [<span data-ttu-id="12624-252">Průvodce indexy Columnstore</span><span class="sxs-lookup"><span data-stu-id="12624-252">Columnstore Indexes Guide</span></span>](https://msdn.microsoft.com/library/gg492088.aspx)
* [<span data-ttu-id="12624-253">Blog: Vylepšené dotazu výkonu s úrovní kompatibility 130 služby Azure SQL Database pomocí Alain Lissoir 6 může 2016</span><span class="sxs-lookup"><span data-stu-id="12624-253">Blog: Improved Query Performance with Compatibility Level 130 in Azure SQL Database, by Alain Lissoir, May 6 2016</span></span>](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/)

<!--
Improved Query Performance with Compatibility Level 130 in Azure SQL Database

May 6, 2016 by Alain Lissoir (AlainL), on GitHub 'alainlissoir'.

https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/

..... Now, above.
....................
..... Soon, below?

CAPS / MSDN ideally, but instead on ACom:
.. # Assess effects of latest compatibility level on query performance, how to

sql-database-compatibility-level-query-performance-130.md

genemi = MightyPen , 2016-05-20  Friday  17:00pm
-->
