---
title: "úroveň kompatibility aaaDatabase 130 – Azure SQL Database | Microsoft Docs"
description: "V tomto článku jsme prozkoumat hello výhody spuštění vaší databázi SQL Azure s úrovní kompatibility 130 a využívat výhody hello hello nový Optimalizátor dotazů a dotaz funkce procesoru. Také jsme adres hello nežádoucí vliv na výkon dotazů hello pro existující aplikace SQL hello."
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
ms.openlocfilehash: 25693c5f7b01405b7073fa7d4cc2833fbe125e2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="improved-query-performance-with-compatibility-level-130-in-azure-sql-database"></a><span data-ttu-id="f4d4f-104">Zvýšení výkonu dotazů s kompatibilitou úrovně 130 ve službě Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="f4d4f-104">Improved query performance with compatibility Level 130 in Azure SQL Database</span></span>
<span data-ttu-id="f4d4f-105">Databáze SQL Azure běží transparentně stovky tisíc databází na mnoha různých kompatibility úrovních, zachování a zaručující hello zpětné kompatibility toohello odpovídající verzi systému Microsoft SQL Server pro všechny své zákazníky!</span><span class="sxs-lookup"><span data-stu-id="f4d4f-105">Azure SQL Database is running transparently hundreds of thousands of databases at many different compatibility levels, preserving and guaranteeing hello backward compatibility toohello corresponding version of Microsoft SQL Server for all its customers!</span></span>

<span data-ttu-id="f4d4f-106">V tomto článku jsme prozkoumat hello výhody spuštění vaše připojení databáze SQL Azure s úrovní kompatibility 130 a využívat výhody hello hello nový Optimalizátor dotazů a dotaz funkce procesoru.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-106">In this article, we explore hello benefits of running your Azure SQL Databse at compatibility level 130, and leveraging hello benefits of hello new query optimizer and query processor features.</span></span> <span data-ttu-id="f4d4f-107">Také jsme adres hello nežádoucí vliv na výkon dotazů hello pro existující aplikace SQL hello.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-107">We also address hello possible side-effects on hello query performance for hello existing SQL applications.</span></span>

<span data-ttu-id="f4d4f-108">Připomínáme historie zarovnání hello úrovně kompatibility toodefault verze SQL jsou následující:</span><span class="sxs-lookup"><span data-stu-id="f4d4f-108">As a reminder of history, hello alignment of SQL versions toodefault compatibility levels are as follows:</span></span>

* <span data-ttu-id="f4d4f-109">100: v systému SQL Server 2008 a Azure SQL Database verze 11.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-109">100: in SQL Server 2008 and Azure SQL Database V11.</span></span>
* <span data-ttu-id="f4d4f-110">110: v systému SQL Server 2012 a Azure SQL Database verze 11.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-110">110: in SQL Server 2012 and Azure SQL Database V11.</span></span>
* <span data-ttu-id="f4d4f-111">120: v systému SQL Server 2014 a Azure SQL Database verze 12.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-111">120: in SQL Server 2014 and Azure SQL Database V12.</span></span>
* <span data-ttu-id="f4d4f-112">130: v systému SQL Server 2016 a Azure SQL databáze verze 12.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-112">130: in SQL Server 2016 and Azure SQL Database V12.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f4d4f-113">Počínaje **mid června 2016**, v databázi SQL Azure, bude úroveň kompatibility výchozí hello 130 místo 120 pro **nově vytvořený** databáze.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-113">Starting in **mid-June 2016**, in Azure SQL Database, hello default compatibility level will be 130 instead of 120 for **newly created** databases.</span></span>
> 
> <span data-ttu-id="f4d4f-114">Databáze vytvořené před mid června 2016 se *není* mít vliv a bude udržovat své aktuální úroveň kompatibility (100, 110 nebo 120).</span><span class="sxs-lookup"><span data-stu-id="f4d4f-114">Databases created before mid-June 2016 will *not* be affected, and will maintain their current compatibility level (100, 110, or 120).</span></span> <span data-ttu-id="f4d4f-115">Databáze, které se migrovat z verzí Azure SQL Database verze 11 tooV12 bude mít úrovní kompatibility 110 nebo 100.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-115">Databases that migrated from Azure SQL Database version V11 tooV12 will have a compatibility level of either 100 or 110.</span></span> 
> 

## <a name="about-compatibility-level-130"></a><span data-ttu-id="f4d4f-116">O úroveň kompatibility 130</span><span class="sxs-lookup"><span data-stu-id="f4d4f-116">About compatibility level 130</span></span>
<span data-ttu-id="f4d4f-117">První Pokud chcete, aby tooknow hello aktuální úroveň kompatibility databáze, spouštění hello následující příkaz jazyka Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-117">First, if you want tooknow hello current compatibility level of your database, execute hello following Transact-SQL statement.</span></span>

```
SELECT compatibility_level
    FROM sys.databases
    WHERE name = '<YOUR DATABASE_NAME>’;
```


<span data-ttu-id="f4d4f-118">Před provedením této změny se stane toolevel 130 pro **nově** vytvořené databáze, můžeme zkontrolujte co tato změna se točí kolem prostřednictvím příklady velmi základního dotazu a v tématu Jak každý, kdo může těžit z něj.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-118">Before this change toolevel 130 happens for **newly** created databases, let’s review what this change is all about through some very basic query examples, and see how anyone can benefit from it.</span></span>

<span data-ttu-id="f4d4f-119">Zpracování dotazu v relační databáze může být velmi složité a může vést toolots počítače vědy a matematika toounderstand hello vyplývajících volbách návrhu a chování.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-119">Query processing in relational databases can be very complex and can lead toolots of computer science and mathematics toounderstand hello inherent design choices and behaviors.</span></span> <span data-ttu-id="f4d4f-120">V tomto dokumentu hello obsah byl záměrně zjednodušené tooensure každý, kdo má minimální technické základní můžete porozumět dopadu hello Změna úrovně kompatibility hello a určit, jak se může hodit aplikace.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-120">In this document, hello content has been intentionally simplified tooensure that anyone with some minimum technical background can understand hello impact of hello compatibility level change and determine how it can benefit applications.</span></span>

<span data-ttu-id="f4d4f-121">Umožňuje rychlý podívejte se na úroveň kompatibility hello 130 přináší v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-121">Let’s have a quick look at what hello compatibility level 130 brings at hello table.</span></span>  <span data-ttu-id="f4d4f-122">Můžete najít další podrobnosti o [změnit úroveň kompatibility databáze (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx), ale zde je uveden krátký:</span><span class="sxs-lookup"><span data-stu-id="f4d4f-122">You can find more details at [ALTER DATABASE Compatibility Level (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx), but here is a short summary:</span></span>

* <span data-ttu-id="f4d4f-123">Hello operace Insert příkazu Insert vyberte může být Vícevláknová nebo může mít paralelní plán při před jednovláknové tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-123">hello Insert operation of an Insert-select statement can be multi-threaded or can have a parallel plan, while before this operation was single-threaded.</span></span>
* <span data-ttu-id="f4d4f-124">Paměť optimalizovaný tabulka a tabulka proměnné dotazy mohou mít nyní paralelní plány při předtím, než tato operace se také jednovláknové.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-124">Memory Optimized table and table variables queries can now have parallel plans, while before this operation was also single-threaded .</span></span>
* <span data-ttu-id="f4d4f-125">Statistiky pro paměťově optimalizované tabulky se dá teď vzorkovat a jsou automaticky aktualizovány.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-125">Statistics for Memory Optimized table can now be sampled and are auto-updated.</span></span> <span data-ttu-id="f4d4f-126">V tématu [co je nového ve službě databázového stroje: OLTP v paměti](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-126">See [What's New in Database Engine: In-Memory OLTP](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) for more details.</span></span>
* <span data-ttu-id="f4d4f-127">V režimu dávky nebo s režimu Row změny s indexy úložiště sloupců</span><span class="sxs-lookup"><span data-stu-id="f4d4f-127">Batch mode v/s Row Mode changes with Column Store indexes</span></span>
  * <span data-ttu-id="f4d4f-128">Řazení v tabulce s indexem úložiště sloupce jsou nyní v dávkovém režimu.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-128">Sorts on a table with a Column Store index are now in batch mode.</span></span>
  * <span data-ttu-id="f4d4f-129">Agregace oddílová nyní pracovat v dávkovém režimu například TSQL funkce LAG nebo realizace příkazy.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-129">Windowing aggregates now operate in batch mode such as TSQL LAG/LEAD statements.</span></span>
  * <span data-ttu-id="f4d4f-130">Dotazy na úložiště sloupce tabulky s více distinct klauzulí pracovat v dávkovém režimu.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-130">Queries on Column Store tables with Multiple distinct clauses operate in Batch mode.</span></span>
  * <span data-ttu-id="f4d4f-131">Dotazy spuštěnými na úrovni DOP = 1 nebo sériového plán také spustit v dávkovém režimu.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-131">Queries running under DOP=1 or with a serial plan also execute in Batch Mode.</span></span>
* <span data-ttu-id="f4d4f-132">Poslední vylepšení odhadu kardinality ve skutečnosti přicházejí s úrovní kompatibility 120, ale pro těch, které je spuštěna v kompatibility nižší úrovně (tj. 100 nebo 110), hello přesunutí toocompatibility úroveň 130 se rovněž otevře tato vylepšení a tyto může taky využívat hello dotazu výkon aplikací.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-132">Last, Cardinality Estimation improvements are actually coming with compatibility level 120, but for those of you running at a lower Compatibility level (i.e. 100, or 110), hello move toocompatibility level 130 will also bring these improvements, and these can also benefit hello query performance of your applications.</span></span>

## <a name="practicing-compatibility-level-130"></a><span data-ttu-id="f4d4f-133">Cvičení úroveň kompatibility 130</span><span class="sxs-lookup"><span data-stu-id="f4d4f-133">Practicing compatibility level 130</span></span>
<span data-ttu-id="f4d4f-134">První Pojďme některé tabulky, indexy a náhodná data vytvořená toopractice některé z těchto nových funkcí.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-134">First let’s get some tables, indexes and random data created toopractice some of these new capabilities.</span></span> <span data-ttu-id="f4d4f-135">Příklady skriptů TSQL Hello mohou být provedeny v části SQL Server 2016 nebo v databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-135">hello TSQL script examples can be executed under SQL Server 2016, or under Azure SQL Database.</span></span> <span data-ttu-id="f4d4f-136">Při vytváření databáze Azure SQL, ujistěte se, ale v hello minimální P2 databázi, protože je nutné vybrat alespoň několik jader tooallow více vláken a proto těžit z těchto funkcí.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-136">However, when creating an Azure SQL database, make sure you choose at hello minimum a P2 database because you need at least a couple of cores tooallow multi-threading and therefore benefit from these features.</span></span>

```
-- Create a Premium P2 Database in Azure SQL Database

CREATE DATABASE MyTestDB
    (EDITION=’Premium’, SERVICE_OBJECTIVE=’P2′);
GO

-- Create 2 tables with a column store index on
-- hello second one (only available on Premium databases)

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


<span data-ttu-id="f4d4f-137">Nyní budeme mít vzhled toosome funkcí hello zpracování dotazu bude s úrovní kompatibility 130.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-137">Now, let’s have a look toosome of hello Query Processing features coming with compatibility level 130.</span></span>

## <a name="parallel-insert"></a><span data-ttu-id="f4d4f-138">Paralelní vložení</span><span class="sxs-lookup"><span data-stu-id="f4d4f-138">Parallel INSERT</span></span>
<span data-ttu-id="f4d4f-139">Provádění příkazů TSQL hello níže provede hello operace INSERT pod úroveň kompatibility 120 a 130, který v uvedeném pořadí provede hello operace INSERT v jednom zařazování modelu (120) a ve model Vícevláknová (130).</span><span class="sxs-lookup"><span data-stu-id="f4d4f-139">Executing hello TSQL statements below executes hello INSERT operation under compatibility level 120 and 130, which respectively executes hello INSERT operation in a single threaded model (120), and in a multi-threaded model (130).</span></span>

```
-- Parallel INSERT … SELECT … in heap or CCI
-- is available under 130 only

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO 

-- hello INSERT part is in serial

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130
GO

-- hello INSERT part is in parallel

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

SET STATISTICS XML OFF;
```


<span data-ttu-id="f4d4f-140">Tím, že požádá plán dotazu hello skutečné hello prohlížení jeho grafické vyjádření, její obsah XML, můžete určit, které mohutnost odhad funkce je v play.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-140">By requesting hello actual hello query plan, looking at its graphical representation or its XML content, you can determine which Cardinality Estimation function is at play.</span></span> <span data-ttu-id="f4d4f-141">Prohlížení hello plány-souběžného na obrázku 1, jsme jasně uvidí, že tento hello vložit sloupec úložiště provádění přejde z sériového portu v 120 tooparallel ve 130.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-141">Looking at hello plans side-by-side on figure 1, we can clearly see that hello Column Store INSERT execution goes from serial in 120 tooparallel in 130.</span></span> <span data-ttu-id="f4d4f-142">Mějte také na paměti tato změna hello hello iterator ikony v hello zobrazující dva paralelní šipky, ilustrující hello skutečnost, že nyní hello iterator spuštění je skutečně paralelní 130 plánu.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-142">Also, note that hello change of hello iterator icon in hello 130 plan showing two parallel arrows, illustrating hello fact that now hello iterator execution is indeed parallel.</span></span> <span data-ttu-id="f4d4f-143">Pokud máte velké toocomplete operace INSERT, hello paralelní provádění, propojené toohello počet jader, které máte k dispozici pro databázi hello provede lépe; až tooa 100krát rychlejší podle vaší konkrétní situace!</span><span class="sxs-lookup"><span data-stu-id="f4d4f-143">If you have large INSERT operations toocomplete, hello parallel execution, linked toohello number of core you have at your disposal for hello database, will perform better; up tooa 100 times faster depending your situation!</span></span>

<span data-ttu-id="f4d4f-144">*Obrázek 1: VLOŽTE operace změny z sériové tooparallel s úrovní kompatibility 130.*</span><span class="sxs-lookup"><span data-stu-id="f4d4f-144">*Figure 1: INSERT operation changes from serial tooparallel with compatibility level 130.*</span></span>

![Obrázek 1](./media/sql-database-compatibility-level-query-performance-130/figure-1.jpg)

## <a name="serial-batch-mode"></a><span data-ttu-id="f4d4f-146">SÉRIOVÉ dávkovém režimu</span><span class="sxs-lookup"><span data-stu-id="f4d4f-146">SERIAL Batch Mode</span></span>
<span data-ttu-id="f4d4f-147">Podobně přesunutí toocompatibility úroveň 130 při zpracování řádky dat umožňuje režimu zpracování dávky.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-147">Similarly, moving toocompatibility level 130 when processing rows of data enables batch mode processing.</span></span> <span data-ttu-id="f4d4f-148">Nejprve dávkových operací režimu jsou dostupná pouze při mají index úložiště sloupců na místě.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-148">First, batch mode operations  are only available when you have a column store index in place.</span></span> <span data-ttu-id="f4d4f-149">Druhý dávky obvykle představuje ~ 900 řádky a používá kódu logiku, optimalizované pro vícejádrovými procesoru, vyšší propustnost paměti a přímo využívá hello komprimovaná data hello sloupec úložiště, pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-149">Second, a batch typically represents ~900 rows, and uses a code logic optimized for multicore CPU, higher memory throughput and directly leverages hello compressed data of hello Column Store whenever possible.</span></span> <span data-ttu-id="f4d4f-150">Za těchto podmínek SQL serveru 2016 zpracovávat ~ 900 řádků najednou, namísto 1 řádek během hello, a v důsledku toho hello celkové režijní náklady operace hello je nyní sdílet hello celý batch, snižuje celkové náklady řádek hello.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-150">Under these conditions, SQL Server 2016 can process ~900 rows at once, instead of 1 row at hello time, and as a consequence, hello overall overhead cost of hello operation is now shared by hello entire batch, reducing hello overall cost by row.</span></span> <span data-ttu-id="f4d4f-151">Toto množství sdílených operací v podstatě v kombinaci s hello sloupec úložiště komprese snižuje latence hello zahrnutých v režimu vyberte dávkovou operaci.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-151">This shared amount of operations combined with hello column store compression basically reduces hello latency involved in a SELECT batch mode operation.</span></span> <span data-ttu-id="f4d4f-152">Můžete najít další podrobnosti o hello sloupec úložiště a dávky režimu v [Průvodce indexy Columnstore](https://msdn.microsoft.com/library/gg492088.aspx).</span><span class="sxs-lookup"><span data-stu-id="f4d4f-152">You can find more details about hello column store and batch mode at [Columnstore Indexes Guide](https://msdn.microsoft.com/library/gg492088.aspx).</span></span>

```
-- Serial batch mode execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- hello scan and aggregate are in row mode

SELECT C1, COUNT (C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO 

-- hello scan and aggregate are in batch mode,
-- and force MAXDOP too1 tooshow that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="f4d4f-153">Jako viditelná níže sledování hello dotazu plány-souběžného na obrázku 2, jsme lze zjistit, že došlo ke změně režimu zpracování hello s úrovní kompatibility hello a v důsledku toho při provádění dotazů hello v obou úroveň kompatibility zcela, jsme uvidíte, že většinu doby zpracování hello je využita pro práci v režimu (86 %) porovnání toohello batch režimu row (14 %), kde byly zpracovány 2 dávky.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-153">As visible below, by observing hello query plans side-by-side on figure 2, we can see that hello processing mode has changed with hello compatibility level, and as a consequence, when executing hello queries in both compatibility level altogether, we can see that most of hello processing time is spent in row mode (86%) compared toohello batch mode (14%), where 2 batches have been processed.</span></span> <span data-ttu-id="f4d4f-154">Zvýšit hello datovou sadu, hello benefit zvýší.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-154">Increase hello dataset, hello benefit will increase.</span></span>

<span data-ttu-id="f4d4f-155">*Obrázek 2: Vyberte operace změny z sériové toobatch režimu s úrovní kompatibility 130.*</span><span class="sxs-lookup"><span data-stu-id="f4d4f-155">*Figure 2: SELECT operation changes from serial toobatch mode with compatibility level 130.*</span></span>

![Obrázek 2](./media/sql-database-compatibility-level-query-performance-130/figure-2.jpg)

## <a name="batch-mode-on-sort-execution"></a><span data-ttu-id="f4d4f-157">Dávkovém režimu na provedení řazení</span><span class="sxs-lookup"><span data-stu-id="f4d4f-157">Batch mode on Sort Execution</span></span>
<span data-ttu-id="f4d4f-158">Podobně jako toohello výše, ale operace řazení použité tooa, hello přechod z režimu toobatch režimu (úroveň kompatibility 120) řádku (úroveň kompatibility 130) zvyšuje výkon hello hello operace řazení pro hello ze stejných důvodů.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-158">Similar toohello above, but applied tooa sort operation, hello transition from row mode (compatibility level 120) toobatch mode (compatibility level 130) improves hello performance of hello SORT operation for hello same reasons.</span></span>

```
-- Batch mode on sort execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- hello scan and aggregate are in row mode

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

-- hello scan and aggregate are in batch mode,
-- and force MAXDOP too1 tooshow that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="f4d4f-159">Viditelné-souběžného na obrázku 3, vidíme, že operace řazení hello v režimu row představuje 81 % hello náklady, zatímco hello dávkovém režimu pouze představuje % 19 hello náklady (v uvedeném pořadí 81 % a % 56 na hello řazení samotné).</span><span class="sxs-lookup"><span data-stu-id="f4d4f-159">Visible side-by-side on figure 3, we can see that hello sort operation in row mode represents 81% of hello cost, while hello batch mode only represents 19% of hello cost (respectively 81% and 56% on hello sort itself).</span></span>

<span data-ttu-id="f4d4f-160">*Obrázek 3: Operace řazení se změní z řádku toobatch režimu s úrovní kompatibility 130.*</span><span class="sxs-lookup"><span data-stu-id="f4d4f-160">*Figure 3: SORT operation changes from row toobatch mode with compatibility level 130.*</span></span>

![Obrázek 3](./media/sql-database-compatibility-level-query-performance-130/figure-3.png)

<span data-ttu-id="f4d4f-162">Samozřejmě tyto ukázky obsahovat pouze desetitisíce řádků, který není nic při prohlížení hello datech dostupných v většina serverů SQL dní.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-162">Obviously, these samples only contain tens of thousands of rows, which is nothing when looking at hello data available in most SQL Servers these days.</span></span> <span data-ttu-id="f4d4f-163">Právě tyto proti miliony řádků místo projektu a tento fakt může projevit v několika minutách provádění ušetřena každý den čekající na vyřízení hello povaze vašich úloh.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-163">Just project these against millions of rows instead, and this can translate in several minutes of execution spared every day pending hello nature of your workload.</span></span>

## <a name="cardinality-estimation-ce-improvements"></a><span data-ttu-id="f4d4f-164">Vylepšení mohutnost odhad (CE)</span><span class="sxs-lookup"><span data-stu-id="f4d4f-164">Cardinality Estimation (CE) improvements</span></span>
<span data-ttu-id="f4d4f-165">Zavedena v systému SQL Server 2014, všechny databáze s úrovní kompatibility 120 nebo vyšší budou používat nové funkce odhadu kardinality hello.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-165">Introduced with SQL Server 2014, any database running at a compatibility level 120 or above will make use of hello new Cardinality Estimation functionality.</span></span> <span data-ttu-id="f4d4f-166">V podstatě mohutnost odhad je logiku hello používá toodetermine jak systému SQL server provede dotaz založený na jeho odhadované náklady.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-166">Essentially, cardinality estimation is hello logic used toodetermine how SQL server will execute a query based on its estimated cost.</span></span> <span data-ttu-id="f4d4f-167">odhad Hello se počítá pomocí vstup z statistiky přidružené objekty zahrnutých v tomto dotazu.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-167">hello estimation is calculated using input from statistics associated with objects involved in that query.</span></span> <span data-ttu-id="f4d4f-168">Prakticky, v hlavní funkce odhadu kardinality jsou odhady počet řádek společně s informacemi o hello distribuci hodnot hello počty odlišné hodnoty, a duplicitní počty obsažené v hello tabulky a objekty odkazovaná v dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-168">Practically, at a high-level, Cardinality Estimation functions are row count estimates along with information about hello distribution of hello values, distinct value counts, and duplicate counts contained in hello tables and objects referenced in hello query.</span></span> <span data-ttu-id="f4d4f-169">Získávání tyto odhady chyba, může způsobit vstupy/výstupy disků toounnecessary kvůli tooinsufficient paměti uděluje (tj. v databázi TempDB úniky) nebo výběr tooa sériové plán spuštění přes paralelní plán spuštění, tooname pár.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-169">Getting these estimates wrong, can lead toounnecessary disk I/O due tooinsufficient memory grants (i.e. TempDB spills), or tooa selection of a serial plan execution over a parallel plan execution, tooname a few.</span></span> <span data-ttu-id="f4d4f-170">Uzavření, může způsobit nesprávné odhady tooan celkové snížení výkonu hello provedení dotazu.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-170">Conclusion, incorrect estimates can lead tooan overall performance degradation of hello query execution.</span></span> <span data-ttu-id="f4d4f-171">Na hello druhé straně lépe odhadne, přesnější odhady, zájemců toobetter dotazu spuštěních!</span><span class="sxs-lookup"><span data-stu-id="f4d4f-171">On hello other side, better estimates, more accurate estimates, leads toobetter query executions!</span></span>

<span data-ttu-id="f4d4f-172">Jak je uvedeno nahoře, optimalizace dotazu a odhady jsou komplexní záležitosti, ale pokud chcete více informací o plány dotazů a architektury odhadu kardinality toolearn, můžete se podívat toohello dokumentu v [optimalizace vaše plány dotazů s hello SQL Server 2014 Odhadu kardinality](https://msdn.microsoft.com/library/dn673537.aspx) pro podrobnější prohlídku.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-172">As mentioned before, query optimizations and estimates are a complex matter, but if you want toolearn more about query plans and cardinality estimator, you can refer toohello document at [Optimizing Your Query Plans with hello SQL Server 2014 Cardinality Estimator](https://msdn.microsoft.com/library/dn673537.aspx) for a deeper dive.</span></span>

## <a name="which-cardinality-estimation-do-you-currently-use"></a><span data-ttu-id="f4d4f-173">Které odhadu kardinality právě používáte?</span><span class="sxs-lookup"><span data-stu-id="f4d4f-173">Which Cardinality Estimation do you currently use?</span></span>
<span data-ttu-id="f4d4f-174">toodetermine v rámci které mohutnost odhad běží vaše dotazy, budeme právě hello dotazu použijte ukázky níže.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-174">toodetermine under which Cardinality Estimation your queries are running, let’s just use hello query samples below.</span></span> <span data-ttu-id="f4d4f-175">Všimněte si, že tento první příklad budou spouštěny pod úrovní kompatibility 110, zdání hello použití hello staré mohutnost odhad funkce.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-175">Note that this first example will run under compatibility level 110, implying hello use of hello old Cardinality Estimation functions.</span></span>

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


<span data-ttu-id="f4d4f-176">Po dokončení provádění klikněte na odkaz hello XML a podívejte se na vlastnosti hello hello první iterator, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-176">Once execution is complete, click on hello XML link, and look at hello properties of hello first iterator as shown below.</span></span> <span data-ttu-id="f4d4f-177">Poznámka: název vlastnosti hello názvem CardinalityEstimationModelVersion nastaveno na 70.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-177">Note hello property name called CardinalityEstimationModelVersion currently set on 70.</span></span> <span data-ttu-id="f4d4f-178">Neznamená to, že hello úroveň kompatibility databáze je nastavena toohello verze SQL Server 7.0 (je nastavený na 110 jako viditelné v příkazech TSQL hello výše), ale hodnota hello 70 jednoduše představuje hello starší verze mohutnost odhad funkce je k dispozici od SQL Server 7.0, která měla žádné hlavní revize až SQL Server 2014 (který se dodává s úrovní kompatibility 120).</span><span class="sxs-lookup"><span data-stu-id="f4d4f-178">It does not mean that hello database compatibility level is set toohello SQL Server 7.0 version (it is set on 110 as visible in hello TSQL statements above), but hello value 70 simply represents hello legacy Cardinality Estimation functionality available since SQL Server 7.0, which had no major revisions until SQL Server 2014 (which comes with a compatibility level of 120).</span></span>

<span data-ttu-id="f4d4f-179">*Obrázek 4: hello CardinalityEstimationModelVersion nastavena too70, při použití úrovní kompatibility 110 nebo níže.*</span><span class="sxs-lookup"><span data-stu-id="f4d4f-179">*Figure 4: hello CardinalityEstimationModelVersion is set too70 when using a compatibility level of 110 or below.*</span></span>

![Obrázek 4](./media/sql-database-compatibility-level-query-performance-130/figure-4.png)

<span data-ttu-id="f4d4f-181">Alternativně můžete změnit too130 úrovně kompatibility hello a zakázání hello použití nové funkce odhadu kardinality hello pomocí hello LEGACY_CARDINALITY_ESTIMATION nastavit tooON s [změnit OBOR konfigurace databáze](https://msdn.microsoft.com/library/mt629158.aspx).</span><span class="sxs-lookup"><span data-stu-id="f4d4f-181">Alternatively, you can change hello compatibility level too130, and disable hello use of hello new Cardinality Estimation function by using hello LEGACY_CARDINALITY_ESTIMATION set tooON with [ALTER DATABASE SCOPED CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx).</span></span> <span data-ttu-id="f4d4f-182">To bude mít přesně hello stejné jako pomocí 110 z odhadu kardinality funkce hlediska, při použití hello nejnovější dotaz zpracování úroveň kompatibility.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-182">This will be exactly hello same as using 110 from a Cardinality Estimation function point of view, while using hello latest query processing compatibility level.</span></span> <span data-ttu-id="f4d4f-183">To uděláte, můžete těžit z hello nový dotaz zpracování funkce přicházející s úrovní kompatibility nejnovější hello (tj. v dávkovém režimu), ale stále závisí na hello staré odhadu kardinality funkcích v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-183">Doing so, you can benefit from hello new query processing features coming with hello latest compatibility level (i.e. batch mode), but still rely on hello old Cardinality Estimation functionality if necessary.</span></span>

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


<span data-ttu-id="f4d4f-184">Přesunutí jednoduše úroveň kompatibility toohello 120 nebo 130 umožňuje nové funkce odhadu kardinality hello.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-184">Simply moving toohello compatibility level 120 or 130 enables hello new Cardinality Estimation functionality.</span></span> <span data-ttu-id="f4d4f-185">V takovém případě výchozí hello CardinalityEstimationModelVersion se nastaví odpovídajícím způsobem too120 nebo 130 jako zobrazené níže.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-185">In such a case, hello default CardinalityEstimationModelVersion will be set accordingly too120 or 130 as visible below.</span></span>

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


<span data-ttu-id="f4d4f-186">*Obrázek 5: hello CardinalityEstimationModelVersion nastavena too130, pokud používáte úroveň kompatibility 130.*</span><span class="sxs-lookup"><span data-stu-id="f4d4f-186">*Figure 5: hello CardinalityEstimationModelVersion is set too130 when using a compatibility level of 130.*</span></span>

![Obrázek 5](./media/sql-database-compatibility-level-query-performance-130/figure-5.jpg)

## <a name="witnessing-hello-cardinality-estimation-differences"></a><span data-ttu-id="f4d4f-188">Jeho práci rozdíly odhadu kardinality hello</span><span class="sxs-lookup"><span data-stu-id="f4d4f-188">Witnessing hello Cardinality Estimation differences</span></span>
<span data-ttu-id="f4d4f-189">Teď umožňuje spustit trochu složitější dotaz zahrnující vnitřního spojení s klauzulí WHERE se některé predikáty a podíváme se na odhad počtu řádků hello z původního mohutnost odhad funkce hello nejdřív.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-189">Now, let’s run a slightly more complex query involving an INNER JOIN with a WHERE clause with some predicates, and let’s look at hello row count estimate from hello old Cardinality Estimation function first.</span></span>

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


<span data-ttu-id="f4d4f-190">Efektivní provádění tohoto dotazu vrátí 200,704 řádky, zatímco hello odhad řádek s funkcemi odhadu kardinality staré hello deklarací 194,284 řádků.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-190">Executing this query effectively returns 200,704 rows, while hello row estimate with hello old Cardinality Estimation functionality claims 194,284 rows.</span></span> <span data-ttu-id="f4d4f-191">Samozřejmě jak je uvedená před, tyto řádků počet výsledků bude také záviset jak často jste spustili hello předchozí ukázky, která naplní hello ukázkové tabulky opakovaně v každé spuštění.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-191">Obviously, as said before, these row count results will also depend how often you ran hello previous samples, which populates hello sample tables over and over again at each run.</span></span> <span data-ttu-id="f4d4f-192">Predikáty hello v dotazu samozřejmě, bude mít vliv na skutečný odhad hello kromě zajištění dostatečného hello tabulky tvaru, data obsahu a jak tato data ve skutečnosti korelaci mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-192">Obviously, hello predicates in your query will also have an influence on hello actual estimation aside from hello table shape, data content, and how this data actually correlate with each other.</span></span>

<span data-ttu-id="f4d4f-193">*Obrázek 6: odhad počtu řádků hello vypnuté 194,284 nebo 6 000 řádků z hello 200,704 řádky očekává.*</span><span class="sxs-lookup"><span data-stu-id="f4d4f-193">*Figure 6: hello row count estimate is 194,284 or 6,000 rows off from hello 200,704 rows expected.*</span></span>

![Obrázek 6](./media/sql-database-compatibility-level-query-performance-130/figure-6.jpg)

<span data-ttu-id="f4d4f-195">V hello stejným způsobem, můžeme provést nyní hello stejný dotaz s novými funkcemi odhadu kardinality hello.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-195">In hello same way, let’s now execute hello same query with hello new Cardinality Estimation functionality.</span></span>

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


<span data-ttu-id="f4d4f-196">Prohlížení hello níže, teď vidíme, že tento řádek odhad hello je 202,877, nebo mnohem blíže a vyšší, než hello staré odhadu kardinality.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-196">Looking at hello below, we now see that hello row estimate is 202,877, or much closer and higher than hello old Cardinality Estimation.</span></span>

<span data-ttu-id="f4d4f-197">*Obrázek 7: odhad počtu řádků hello je nyní 202,877 místo 194,284.*</span><span class="sxs-lookup"><span data-stu-id="f4d4f-197">*Figure 7: hello row count estimate is now 202,877, instead of 194,284.*</span></span>

![Obrázek 7](./media/sql-database-compatibility-level-query-performance-130/figure-7.jpg)

<span data-ttu-id="f4d4f-199">Ve skutečnosti je sada výsledků dotazu hello 200,704 řádků (ale všechny závisí, jak často se spustit hello dotazy pro hello v předchozí ukázky, ale je důležité, protože hello TSQL používá příkaz RAND() hello, hello skutečných hodnot, vrátí se můžou lišit od jednoho spuštění toohello Další).</span><span class="sxs-lookup"><span data-stu-id="f4d4f-199">In reality, hello result set is 200,704 rows (but all of it depends how often you did run hello queries of hello previous samples, but more importantly, because hello TSQL uses hello RAND() statement, hello actual values returned can vary from one run toohello next).</span></span> <span data-ttu-id="f4d4f-200">Proto v tomto konkrétním příkladu hello nové odhadu kardinality nepodporuje lépe daří při odhadování hello počet řádků, protože 202,877 je mnohem blíže too200, 704, než 194,284!</span><span class="sxs-lookup"><span data-stu-id="f4d4f-200">Therefore, in this particular example, hello new Cardinality Estimation does a better job at estimating hello number of rows because 202,877 is much closer too200,704, than 194,284!</span></span> <span data-ttu-id="f4d4f-201">Poslední, pokud změníte hello klauzule WHERE predikáty tooequality (místo ">" pro instanci), to se ověřte hello odhady mezi hello staré a nové funkce mohutnost i více různých, v závislosti na tom, kolik odpovídá můžete získat.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-201">Last, if you change hello WHERE clause predicates tooequality (rather than “>” for instance), this could make hello estimates between hello old and new Cardinality function even more different, depending on how many matches you can get.</span></span>

<span data-ttu-id="f4d4f-202">Samozřejmě v takovém případě se ~ 6000 řádků vypnout z skutečný počet nepředstavuje velké množství dat v některých situacích.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-202">Obviously, in this case, being ~6000 rows off from actual count does not represent a lot of data in some situations.</span></span> <span data-ttu-id="f4d4f-203">Nyní, transponuje tuto toomillions řádků napříč několika tabulek a složitější dotazy a v některých případech hello odhad lze vypnout podle miliony řádky a proto hello riziko nesprávné, že plán spuštění, nebo pro požadování nedostatek paměti uděluje úvodní výdej up hello tooTempDB úniky a proto více vstupně-výstupních operací, jsou mnohem vyšší.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-203">Now, transpose this toomillions of rows across several tables and more complex queries, and at times hello estimate can be off by millions of rows , and therefore, hello risk of picking-up hello wrong execution plan, or requesting insufficient memory grants leading tooTempDB spills, and so more I/O, are much higher.</span></span>

<span data-ttu-id="f4d4f-204">Pokud máte možnost hello, praxi toto porovnání s nejčastější dotazy a datové sady a zobrazit si sami, jak moc některé hello starý a nový odhady jsou ovlivněni, zatímco některé by se mohly právě stát další vypnout ze skutečnosti hello nebo některé, ostatní stačí jednoduše co nejblíže toohello skutečné řádek počty ve skutečnosti, vrátí se v sadách výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-204">If you have hello opportunity, practice this comparison with your most typical queries and datasets, and see for yourself by how much some of hello old and new estimates are affected, while some could just become more off from hello reality, or some others just simply closer toohello actual row counts actually returned in hello result sets.</span></span> <span data-ttu-id="f4d4f-205">Všechny záviset hello tvaru dotazy, hello Azure SQL database charakteristiky, hello povaze a hello velikost datových sad a hello statistiky o je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-205">All of it will depend of hello shape of your queries, hello Azure SQL database characteristics, hello nature and hello size of your datasets, and hello statistics available about them.</span></span> <span data-ttu-id="f4d4f-206">Pokud jste právě vytvořili instanci služby Azure SQL Database, hello dotazu pro optimalizaci bude mít toobuild spustí jeho znalostní báze od začátku místo opětovné použití statistiky, které se skládají z předchozího dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-206">If you just created your Azure SQL Database instance, hello query optimizer will have toobuild its knowledge from scratch instead of reusing statistics made of hello previous query runs.</span></span> <span data-ttu-id="f4d4f-207">Ano jsou odhady hello tooevery velmi kontextové a téměř konkrétní situaci serveru a aplikace.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-207">So, hello estimates are very contextual and almost specific tooevery server and application situation.</span></span> <span data-ttu-id="f4d4f-208">Je důležitým aspektem tookeep pamatovat!</span><span class="sxs-lookup"><span data-stu-id="f4d4f-208">It is an important aspect tookeep in mind!</span></span>

## <a name="some-considerations-tootake-into-account"></a><span data-ttu-id="f4d4f-209">Některé aspekty tootake do účtu</span><span class="sxs-lookup"><span data-stu-id="f4d4f-209">Some considerations tootake into account</span></span>
<span data-ttu-id="f4d4f-210">I když většina úloh by využívat úroveň kompatibility hello 130, před přijetím hello úroveň kompatibility pro produkční prostředí, máte v podstatě 3 možnosti:</span><span class="sxs-lookup"><span data-stu-id="f4d4f-210">Although most workloads would benefit from hello compatibility level 130, before you adopting hello compatibility level for your production environment, you basically have 3 options:</span></span>

1. <span data-ttu-id="f4d4f-211">Přesunout toocompatibility úroveň 130 a v tématu Jak provádět věcí.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-211">You move toocompatibility level 130, and see how things perform.</span></span> <span data-ttu-id="f4d4f-212">V případě, že jste si všimli některé regresí, stačí jednoduše nastavit hello kompatibility úrovně back tooits původní úroveň, nebo zachovat 130 a pouze reverse hello odhadu kardinality back toohello režim starší verze (jak je popsáno výše, to samostatně může hello problém vyřešit).</span><span class="sxs-lookup"><span data-stu-id="f4d4f-212">In case you notice some regressions, you just simply set hello compatibility level back tooits original level, or keep 130, and only reverse hello Cardinality Estimation back toohello legacy mode (As explained above, this alone could address hello issue).</span></span>
2. <span data-ttu-id="f4d4f-213">Důkladně otestovat existující aplikace podobné produkční zatížení, vyladit a ověření výkonu hello před tooproduction probíhající.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-213">You thoroughly test your existing applications under similar production load, fine tune, and validate hello performance before going tooproduction.</span></span> <span data-ttu-id="f4d4f-214">V případě problémů stejné jako výše, můžete vždy vrátit původní úroveň kompatibility toohello nebo jednoduše reverse hello odhadu kardinality back toohello režim starší verze.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-214">In case of issues, same as above, you can always go back toohello original compatibility level, or simply reverse hello Cardinality Estimation back toohello legacy mode.</span></span>
3. <span data-ttu-id="f4d4f-215">Jako poslední možnost a hello nejnovější tooaddress způsob, jak tyto otázky, je úložiště dotazů tooleverage hello.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-215">As a final option, and hello most recent way tooaddress these questions, is tooleverage hello Query Store.</span></span> <span data-ttu-id="f4d4f-216">To je doporučená možnost dnešní!</span><span class="sxs-lookup"><span data-stu-id="f4d4f-216">That’s today’s recommended option!</span></span> <span data-ttu-id="f4d4f-217">Analýza hello tooassist vašich dotazů v rámci kompatibility na úrovni 120 nebo pod versus 130, nelze doporučujeme dostatek toouse úložiště dotazů.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-217">tooassist hello analysis of your queries under compatibility level 120 or below versus 130, we cannot encourage you enough toouse Query Store.</span></span> <span data-ttu-id="f4d4f-218">Úložiště dotazů je k dispozici v nejnovější verzi Azure SQL Database V12 hello a je navržena toohelp k řešení potíží s výkonem dotazu.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-218">Query Store is available with hello latest version of Azure SQL Database V12, and it’s designed toohelp you with query performance troubleshooting.</span></span> <span data-ttu-id="f4d4f-219">Hello úložiště dotazů si můžete představit jako záznam dat pro vaši databázi shromažďování a nabízí podrobné historické informace o všech dotazů.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-219">Think of hello Query Store as a flight data recorder for your database collecting and presenting detailed historic information about all queries.</span></span> <span data-ttu-id="f4d4f-220">To výrazně zjednodušuje forenzních výkon snížením hello čas toodiagnose a vyřešte problémy.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-220">This greatly simplifies performance forensics by reducing hello time toodiagnose and resolve issues.</span></span> <span data-ttu-id="f4d4f-221">Další informace najdete [úložiště dotazů: záznam dat pro vaši databázi](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).</span><span class="sxs-lookup"><span data-stu-id="f4d4f-221">You can find more information at [Query Store: A flight data recorder for your database](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).</span></span>

<span data-ttu-id="f4d4f-222">Na vysoké úrovni, pokud již máte sadu databází s úrovní kompatibility 120 nebo níže a plánování toomove hello některá z nich too130, nebo protože vaše úlohy automaticky zřizovat nové databáze, které budou brzy se ve výchozím nastavení too130 nastavení, zvažte prosím Hello těchto hodnot:</span><span class="sxs-lookup"><span data-stu-id="f4d4f-222">At hello high-level, if you already have a set of databases running at compatibility level 120 or below, and plan toomove some of them too130, or because your workload automatically provision new databases that will be soon be set by default too130, please consider hello followings:</span></span>

* <span data-ttu-id="f4d4f-223">Před změnou toohello novou úroveň kompatibility v produkčním prostředí, povolte úložiště dotazů.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-223">Before changing toohello new compatibility level in production, enable Query Store.</span></span> <span data-ttu-id="f4d4f-224">Můžete se podívat příliš[změna hello režim kompatibility databáze a používání hello dotazu úložiště](https://msdn.microsoft.com/library/bb895281.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-224">You can refer too[Change hello Database Compatibility Mode and Use hello Query Store](https://msdn.microsoft.com/library/bb895281.aspx) for more information.</span></span>
* <span data-ttu-id="f4d4f-225">V dalším kroku otestujte všechny kritické úlohy pomocí reprezentativní dat a dotazy z prostředí produkčního prostředí a porovnání výkonu hello došlo a jako hlášené pomocí úložiště dotazů.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-225">Next, test all critical workloads using representative data and queries of a production-like environment, and compare hello performance experienced and as reported by Query Store.</span></span> <span data-ttu-id="f4d4f-226">Pokud dojde k některé regresí, můžete identifikovat hello který poklesl dotazy s hello úložiště dotazů a použít plán hello vynucení možnost úložiště dotazů (neboli plán Připnutí).</span><span class="sxs-lookup"><span data-stu-id="f4d4f-226">If you experience some regressions, you can identify hello regressed queries with hello Query Store and use hello plan forcing option from Query Store (aka plan pinning).</span></span> <span data-ttu-id="f4d4f-227">V takovém případě můžete platností zůstat s úrovní kompatibility hello 130 a používat plán hello bývalé dotazu na návrh hello úložiště dotazů.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-227">In such a case, you definitively stay with hello compatibility level 130, and use hello former query plan as suggested by hello Query Store.</span></span>
* <span data-ttu-id="f4d4f-228">Pokud chcete tooleverage nové funkce a možnosti služby Azure SQL Database (který je spuštěn SQL Server 2016), ale jsou citlivé toochanges uvést do režimu podle úrovně kompatibility hello 130, jako poslední možnost, zvažte vynucení hello úroveň kompatibility zpět úroveň toohello, který vyhovuje vaše úlohy pomocí příkazu ALTER DATABASE.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-228">If you want tooleverage new features and capabilities of Azure SQL Database (which is running SQL Server 2016), but are sensitive toochanges brought by hello compatibility level 130, as a last resort, you could consider forcing hello compatibility level back toohello level that suits your workload by using an ALTER DATABASE statement.</span></span> <span data-ttu-id="f4d4f-229">Ale nejprve, mějte na paměti, že plán úložiště dotazů hello Připnutí možnost je nejlepší možnost, protože není pomocí 130 je v podstatě zachování na úrovni funkcí hello starší verze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-229">But first, be aware that hello Query Store plan pinning option is your best option because not using 130 is basically staying at hello functionality level of an older SQL Server version.</span></span>
* <span data-ttu-id="f4d4f-230">Pokud máte víceklientské aplikace pokrývání uzlů více databází, může být nutné tooupdate hello zřizovat logiku vaší databáze tooensure úroveň kompatibility konzistentní v rámci všech databází; starý a nově zřízeného ty.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-230">If you have multitenant applications spanning multiple databases, it may be necessary tooupdate hello provisioning logic of your databases tooensure a consistent compatibility level across all databases; old and newly provisioned ones.</span></span> <span data-ttu-id="f4d4f-231">Výkonu úloh aplikace může být citlivé toohello skutečnost, že některé databáze, které jsou spuštěné v různých kompatibility úrovně, a proto může být požadované kompatibility úrovně konzistence napříč všechny databáze v pořadí tooprovide hello stejné prostředí zákazníků tooyour všechny napříč hello panelu.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-231">Your application workload performance could be sensitive toohello fact that some databases are running at different compatibility levels, and therefore, compatibility level consistency across any database could be required in order tooprovide hello same experience tooyour customers all across hello board.</span></span> <span data-ttu-id="f4d4f-232">Všimněte si, že se nejedná o pověření, ve skutečnosti závisí na tom, jak vaše aplikace je ovlivňován hello úroveň kompatibility.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-232">Note that it is not a mandate, it really depends on how your application is affected by hello compatibility level.</span></span>
* <span data-ttu-id="f4d4f-233">Poslední, týkající se hello mohutnost odhad a stejně jako změna úrovně kompatibility hello, než budete pokračovat v produkčním prostředí, je doporučené tootest vaše produkční úlohy v rámci nové podmínky toodetermine hello, pokud vaše aplikace výhody z vylepšení odhadu kardinality Hello.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-233">Last, regarding hello Cardinality Estimation, and just like changing hello compatibility level, before proceeding in production, it is recommended tootest your production workload under hello new conditions toodetermine if your application benefits from hello Cardinality Estimation improvements.</span></span>

## <a name="conclusion"></a><span data-ttu-id="f4d4f-234">Závěr</span><span class="sxs-lookup"><span data-stu-id="f4d4f-234">Conclusion</span></span>
<span data-ttu-id="f4d4f-235">Používání Azure SQL Database může toobenefit ze všech vylepšení SQL Server 2016 jasně zvýšit spuštěních vašeho dotazu.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-235">Using Azure SQL Database toobenefit from all SQL Server 2016 enhancements can clearly improve your query executions.</span></span> <span data-ttu-id="f4d4f-236">Stejně jako-je!</span><span class="sxs-lookup"><span data-stu-id="f4d4f-236">Just as-is!</span></span> <span data-ttu-id="f4d4f-237">Samozřejmě podobně jako u jakékoli nové funkce, řádné hodnocení je třeba provést toodetermine hello přesné podmínky, za kterých vaše databáze zatížení funguje hello nejlépe.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-237">Of course, like any new feature, a proper evaluation must be done toodetermine hello exact conditions under which your database workload operates hello best.</span></span> <span data-ttu-id="f4d4f-238">Prostředí uvádí, že většina úloh jsou očekávané tooat alespoň běh transparentně úroveň kompatibility 130, při využití nový dotaz zpracování funkce a novou odhadu kardinality.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-238">Experience shows that most workload are expected tooat least run transparently under compatibility level 130, while leveraging new query processing functions, and new Cardinality Estimation.</span></span> <span data-ttu-id="f4d4f-239">Který uvedená reálně jsou vždy některé výjimky a provádění správné splatnosti opatrností je toodetermine důležité vyhodnocení, kolik můžete využívat výhod těchto vylepšení.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-239">That said, realistically, there are always some exceptions and doing proper due diligence is an important assessment toodetermine how much you can benefit from these enhancements.</span></span> <span data-ttu-id="f4d4f-240">A znovu, můžou být úložiště dotazů hello skvělé pomáhá při provádění této práce!</span><span class="sxs-lookup"><span data-stu-id="f4d4f-240">And again, hello Query Store can be of a great help in doing this work!</span></span>

<span data-ttu-id="f4d4f-241">S rozvojem zpracovaní SQL Azure, můžete očekávat úroveň kompatibility 140 v hello budoucí.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-241">As SQL Azure evolves, you can expect a compatibility level 140 in hello future.</span></span> <span data-ttu-id="f4d4f-242">Kdy je vhodné čas, spustíme posuzování co se otevře tato úroveň kompatibilitou v budoucnu 140, stejně jako jsme stručně tady popisovaných jakou úroveň kompatibility 130 je přináší dnes.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-242">When time is appropriate, we will start talking about what this future compatibility level 140 will bring, just as we briefly discussed here what compatibility level 130 is bringing today.</span></span>

<span data-ttu-id="f4d4f-243">Teď, můžeme není zapomněli, od června 2016, Azure SQL Database změní úroveň kompatibility výchozí hello ze 120 too130 pro nově vytvořené databáze.</span><span class="sxs-lookup"><span data-stu-id="f4d4f-243">For now, let’s not forget, starting June 2016, Azure SQL Database will change hello default compatibility level from 120 too130 for newly created databases.</span></span> <span data-ttu-id="f4d4f-244">Mějte na paměti!</span><span class="sxs-lookup"><span data-stu-id="f4d4f-244">Be aware!</span></span>

## <a name="references"></a><span data-ttu-id="f4d4f-245">Odkazy</span><span class="sxs-lookup"><span data-stu-id="f4d4f-245">References</span></span>
* [<span data-ttu-id="f4d4f-246">Co je nového v databázový stroj</span><span class="sxs-lookup"><span data-stu-id="f4d4f-246">What’s New in Database Engine</span></span>](https://msdn.microsoft.com/library/bb510411.aspx#InMemory)
* [<span data-ttu-id="f4d4f-247">Blog: Úložiště dotazů: záznam dat pro vaši databázi pomocí Borko Novakovic, 8. června 2016</span><span class="sxs-lookup"><span data-stu-id="f4d4f-247">Blog: Query Store: A flight data recorder for your database, by Borko Novakovic, June 8 2016</span></span>](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/)
* [<span data-ttu-id="f4d4f-248">Úroveň kompatibility databáze ALTER (Transact-SQL)</span><span class="sxs-lookup"><span data-stu-id="f4d4f-248">ALTER DATABASE Compatibility Level (Transact-SQL)</span></span>](https://msdn.microsoft.com/library/bb510680.aspx)
* [<span data-ttu-id="f4d4f-249">PŘÍKAZ ALTER VYMEZENÁ KONFIGURACE DATABÁZE</span><span class="sxs-lookup"><span data-stu-id="f4d4f-249">ALTER DATABASE SCOPED CONFIGURATION</span></span>](https://msdn.microsoft.com/library/mt629158.aspx)
* [<span data-ttu-id="f4d4f-250">Úroveň kompatibility 130 pro Azure SQL Database verze 12</span><span class="sxs-lookup"><span data-stu-id="f4d4f-250">Compatibility Level 130 for Azure SQL Database V12</span></span>](https://azure.microsoft.com/updates/compatibility-level-130-for-azure-sql-database-v12/)
* [<span data-ttu-id="f4d4f-251">Optimalizace vaše plány dotazů s hello odhadu kardinality aplikace SQL Server 2014</span><span class="sxs-lookup"><span data-stu-id="f4d4f-251">Optimizing Your Query Plans with hello SQL Server 2014 Cardinality Estimator</span></span>](https://msdn.microsoft.com/library/dn673537.aspx)
* [<span data-ttu-id="f4d4f-252">Průvodce indexy Columnstore</span><span class="sxs-lookup"><span data-stu-id="f4d4f-252">Columnstore Indexes Guide</span></span>](https://msdn.microsoft.com/library/gg492088.aspx)
* [<span data-ttu-id="f4d4f-253">Blog: Vylepšené dotazu výkonu s úrovní kompatibility 130 služby Azure SQL Database pomocí Alain Lissoir 6 může 2016</span><span class="sxs-lookup"><span data-stu-id="f4d4f-253">Blog: Improved Query Performance with Compatibility Level 130 in Azure SQL Database, by Alain Lissoir, May 6 2016</span></span>](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/)

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
