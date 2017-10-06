---
title: statistiky aaaManaging u tabulek v SQL Data Warehouse | Microsoft Docs
description: "Začínáme s statistiky pro tabulky v Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: faa1034d-314c-4f9d-af81-f5a9aedf33e4
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 10/31/2016
ms.author: shigu;barbkess
ms.openlocfilehash: c9521dc47891f68d124e77a53e2e15d03275caaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-statistics-on-tables-in-sql-data-warehouse"></a><span data-ttu-id="b9165-103">Správa statistiky u tabulek v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="b9165-103">Managing statistics on tables in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="b9165-104">[Přehled][Overview]</span><span class="sxs-lookup"><span data-stu-id="b9165-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="b9165-105">[Datové typy][Data Types]</span><span class="sxs-lookup"><span data-stu-id="b9165-105">[Data Types][Data Types]</span></span>
> * <span data-ttu-id="b9165-106">[Distribuce][Distribute]</span><span class="sxs-lookup"><span data-stu-id="b9165-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="b9165-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="b9165-107">[Index][Index]</span></span>
> * <span data-ttu-id="b9165-108">[Oddíl][Partition]</span><span class="sxs-lookup"><span data-stu-id="b9165-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="b9165-109">[Statistiky][Statistics]</span><span class="sxs-lookup"><span data-stu-id="b9165-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="b9165-110">[Dočasné][Temporary]</span><span class="sxs-lookup"><span data-stu-id="b9165-110">[Temporary][Temporary]</span></span>
> 
> 

<span data-ttu-id="b9165-111">Hello další SQL Data Warehouse ví o vašich dat hello rychlejší ho můžete spouštět dotazy na data.</span><span class="sxs-lookup"><span data-stu-id="b9165-111">hello more SQL Data Warehouse knows about your data, hello faster it can execute queries against your data.</span></span>  <span data-ttu-id="b9165-112">Hello způsobem, který říct SQL Data Warehouse o vašich dat je shromažďování statistických údajů o vaše data.</span><span class="sxs-lookup"><span data-stu-id="b9165-112">hello way that you tell SQL Data Warehouse about your data, is by collecting statistics about your data.</span></span>  <span data-ttu-id="b9165-113">S statistické údaje o vašich dat je jedním z nejdůležitějších věcí hello můžete provést toooptimize své dotazy.</span><span class="sxs-lookup"><span data-stu-id="b9165-113">Having statistics on your data is one of hello most important things you can do toooptimize your queries.</span></span>  <span data-ttu-id="b9165-114">Statistiky pomoci vytvořit hello optimální plán pro své dotazy SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="b9165-114">Statistics help SQL Data Warehouse create hello most optimal plan for your queries.</span></span>  <span data-ttu-id="b9165-115">To je proto hello SQL Data Warehouse dotazu pro optimalizaci je s náklady na základě pro optimalizaci.</span><span class="sxs-lookup"><span data-stu-id="b9165-115">This is because hello SQL Data Warehouse query optimizer is a cost based optimizer.</span></span>  <span data-ttu-id="b9165-116">To znamená porovná hello náklady na různé plány dotazů a potom vybere hello plán s nejnižší náklady hello, které by se měly také hello plán, který provede nejrychlejší hello.</span><span class="sxs-lookup"><span data-stu-id="b9165-116">That is, it compares hello cost of various query plans and then chooses hello plan with hello lowest cost, which should also be hello plan that will execute hello fastest.</span></span>

<span data-ttu-id="b9165-117">Statistiku lze vytvořit na jeden sloupec, více sloupců nebo index tabulky.</span><span class="sxs-lookup"><span data-stu-id="b9165-117">Statistics can be created on a single column, multiple columns or on an index of a table.</span></span>  <span data-ttu-id="b9165-118">Statistiky jsou uloženy v histogram, který zachycuje hello rozsah a selektivitu hodnot.</span><span class="sxs-lookup"><span data-stu-id="b9165-118">Statistics are stored in a histogram which captures hello range and selectivity of values.</span></span>  <span data-ttu-id="b9165-119">Toto je zajímají hlavně o při hello Optimalizátor musí tooevaluate spojení, GROUP BY, HAVING a klauzule WHERE v dotazu.</span><span class="sxs-lookup"><span data-stu-id="b9165-119">This is of particular interest when hello optimizer needs tooevaluate JOINs, GROUP BY, HAVING and WHERE clauses in a query.</span></span>  <span data-ttu-id="b9165-120">Například pokud hello Optimalizátor odhadne, datum hello filtrujete v dotazu vrátí 1 řádek, může vybrat, velmi jiné plánování než pokud ho odhady jejich datum, na které jste vybrali bude vracet 1 milionu řádků.</span><span class="sxs-lookup"><span data-stu-id="b9165-120">For example, if hello optimizer estimates that hello date you are filtering in your query will return 1 row, it may choose a very different plan than if it estimates that they date you have selected will return 1 million rows.</span></span>  <span data-ttu-id="b9165-121">Při vytváření statistik je velmi důležité, je stejně důležité této statistiky *přesně* odráží aktuální stav hello hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="b9165-121">While creating statistics is extremely important, it is equally important that statistics *accurately* reflect hello current state of hello table.</span></span>  <span data-ttu-id="b9165-122">S aktuální statistiky zajistí, že je kvalitní plán vybrány ve Optimalizátor hello.</span><span class="sxs-lookup"><span data-stu-id="b9165-122">Having up-to-date statistics ensures that a good plan is selected by hello optimizer.</span></span>  <span data-ttu-id="b9165-123">Hello plány vytvořené hello Optimalizátor jsou pouze jako vhodné jako hello statistiky na vaše data.</span><span class="sxs-lookup"><span data-stu-id="b9165-123">hello plans created by hello optimizer are only as good as hello statistics on your data.</span></span>

<span data-ttu-id="b9165-124">Hello proces vytváření a aktualizaci statistiky je aktuálně ruční proces, ale je velmi jednoduchý toodo.</span><span class="sxs-lookup"><span data-stu-id="b9165-124">hello process of creating and updating statistics is currently a manual process, but is very simple toodo.</span></span>  <span data-ttu-id="b9165-125">To je rozdíl oproti systému SQL Server, který automaticky vytvoří a aktualizuje statistické údaje o jednoho sloupce a indexy.</span><span class="sxs-lookup"><span data-stu-id="b9165-125">This is unlike SQL Server which automatically creates and updates statistics on single columns and indexes.</span></span>  <span data-ttu-id="b9165-126">Pomocí níže uvedené informace hello můžete výrazně automatizovat správu hello hello statistik na vaše data.</span><span class="sxs-lookup"><span data-stu-id="b9165-126">By using hello information below, you can greatly automate hello management of hello statistics on your data.</span></span> 

## <a name="getting-started-with-statistics"></a><span data-ttu-id="b9165-127">Začínáme s statistiky</span><span class="sxs-lookup"><span data-stu-id="b9165-127">Getting started with statistics</span></span>
 <span data-ttu-id="b9165-128">Vytváření jen Vzorkovaná statistiku pro každý sloupec je tooget snadno pracovat s statistiky.</span><span class="sxs-lookup"><span data-stu-id="b9165-128">Creating sampled statistics on every column is an easy way tooget started with statistics.</span></span>  <span data-ttu-id="b9165-129">Vzhledem k tomu, že je stejně důležité tookeep statistiky aktuální, konzervativní přístup může být tooupdate statistice denně nebo po každé zatížení.</span><span class="sxs-lookup"><span data-stu-id="b9165-129">Since it is equally important tookeep statistics up-to-date, a conservative approach may be tooupdate your statistics daily or after each load.</span></span> <span data-ttu-id="b9165-130">Jsou vždy kompromis mezi výkonem a hello náklady toocreate a aktualizaci statistik.</span><span class="sxs-lookup"><span data-stu-id="b9165-130">There are always trade-offs between performance and hello cost toocreate and update statistics.</span></span>  <span data-ttu-id="b9165-131">Pokud zjistíte, že trvá příliš dlouho toomaintain všechny statistické údaje, můžete, třeba tootry toobe užší sloupce, které mají statistiky nebo sloupce, které často aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="b9165-131">If you find it is taking too long toomaintain all of your statistics, you may want tootry toobe more selective about which columns have statistics or which columns need frequent updating.</span></span>  <span data-ttu-id="b9165-132">Můžete například sloupců s kalendářními daty tooupdate denně, jak lze přidat nové hodnoty spíše než po každé zatížení.</span><span class="sxs-lookup"><span data-stu-id="b9165-132">For example, you might want tooupdate date columns daily, as new values may be added rather than after every load.</span></span> <span data-ttu-id="b9165-133">Znovu, bude získáte hello využívat výhod tak, že statistiky na sloupce použité ve spojení, GROUP BY, HAVING a klauzule WHERE.</span><span class="sxs-lookup"><span data-stu-id="b9165-133">Again, you will gain hello most benefit by having statistics on columns involved in JOINs, GROUP BY, HAVING and WHERE clauses.</span></span>  <span data-ttu-id="b9165-134">Pokud máte tabulku s velkým množstvím sloupce, které se používají jenom v hello SELECT – klauzule, nemusí být úspěšná statistiky pro tyto sloupce a výdaje trochu další úsilí tooidentify jenom hello sloupce, které vám umožní statistiky, můžete snížit čas toomaintain hello statistice .</span><span class="sxs-lookup"><span data-stu-id="b9165-134">If you have a table with a lot of columns which are only used in hello SELECT clause, statistics on these columns may not help, and spending a little more effort tooidentify only hello columns where statistics will help, can reduce hello time toomaintain your statistics.</span></span>

## <a name="multi-column-statistics"></a><span data-ttu-id="b9165-135">Statistiky více sloupce</span><span class="sxs-lookup"><span data-stu-id="b9165-135">Multi-column statistics</span></span>
<span data-ttu-id="b9165-136">Kromě toho toocreating statistik na jednoho sloupce, můžete zjistit, vaše dotazy bude využívat více sloupci statistiky.</span><span class="sxs-lookup"><span data-stu-id="b9165-136">In addition toocreating statistics on single columns, you may find that your queries will benefit from multi-column statistics.</span></span>  <span data-ttu-id="b9165-137">Statistiky více sloupce jsou statistiky vytvořena na seznam sloupců.</span><span class="sxs-lookup"><span data-stu-id="b9165-137">Multi-column statistics are statistics created on a list of columns.</span></span>  <span data-ttu-id="b9165-138">Obsahují jeden sloupec statistiku hello první sloupec v seznamu hello plus densities – názvem některé informace korelace mezi sloupci.</span><span class="sxs-lookup"><span data-stu-id="b9165-138">They include single column statistics on hello first column in hello list, plus some cross-column correlation information called densities.</span></span>  <span data-ttu-id="b9165-139">Například pokud máte tabulku, která připojí tooanother na dva sloupce, můžete zjistit, že SQL Data Warehouse můžete lépe optimalizovat hello plán v případě, že rozumí hello relaci mezi dvěma sloupci.</span><span class="sxs-lookup"><span data-stu-id="b9165-139">For example, if you have a table that joins tooanother on two columns, you may find that SQL Data Warehouse can better optimize hello plan if it understands hello relationship between two columns.</span></span>   <span data-ttu-id="b9165-140">Statistiky více sloupce můžete zlepšit výkon dotazu pro některé operace, jako je například složené spojení a seskupit podle.</span><span class="sxs-lookup"><span data-stu-id="b9165-140">Multi-column statistics can improve query performance for some operations such as composite joins and group by.</span></span>

## <a name="updating-statistics"></a><span data-ttu-id="b9165-141">Aktualizuje statistické údaje</span><span class="sxs-lookup"><span data-stu-id="b9165-141">Updating statistics</span></span>
<span data-ttu-id="b9165-142">Aktualizuje statistické údaje, je důležitou součástí rutiny správy vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="b9165-142">Updating statistics is an important part of your database management routine.</span></span>  <span data-ttu-id="b9165-143">Když se změní hello distribuci dat v databázi hello, třeba statistiky toobe aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="b9165-143">When hello distribution of data in hello database changes, statistics need toobe updated.</span></span>  <span data-ttu-id="b9165-144">Zastaralé statistiky povede dotazu toosub optimální výkon.</span><span class="sxs-lookup"><span data-stu-id="b9165-144">Out-of-date statistics will lead toosub-optimal query performance.</span></span>

<span data-ttu-id="b9165-145">Jeden osvědčeným postupem je tooupdate statistiku sloupců s kalendářními daty jako nová data jsou přidány každý den.</span><span class="sxs-lookup"><span data-stu-id="b9165-145">One best practice is tooupdate statistics on date columns each day as new dates are added.</span></span>  <span data-ttu-id="b9165-146">Každé nové řádky času jsou načtená do datového skladu hello, nové zatížení kalendářní data nebo data transakcí se přidají.</span><span class="sxs-lookup"><span data-stu-id="b9165-146">Each time new rows are loaded into hello data warehouse, new load dates or transaction dates are added.</span></span> <span data-ttu-id="b9165-147">Tyto změnit hello distribuci dat a proveďte hello statistiky zastaralé.</span><span class="sxs-lookup"><span data-stu-id="b9165-147">These change hello data distribution and make hello statistics out-of-date.</span></span> <span data-ttu-id="b9165-148">Naopak statistiky země sloupec v tabulce zákazníka může být nikdy nepotřebují toobe aktualizován, jako distribuční hello hodnot nemění obecně.</span><span class="sxs-lookup"><span data-stu-id="b9165-148">Conversely, statistics on a country column in a customer table might never need toobe updated, as hello distribution of values doesn’t generally change.</span></span> <span data-ttu-id="b9165-149">Za předpokladu, že distribuce hello je konstantní mezi odběrateli, přidání nové řádky toohello tabulky variace není přejdete distribuci dat toochange hello.</span><span class="sxs-lookup"><span data-stu-id="b9165-149">Assuming hello distribution is constant between customers, adding new rows toohello table variation isn't going toochange hello data distribution.</span></span> <span data-ttu-id="b9165-150">Ale pokud váš datový sklad obsahuje pouze jeden země a připojte ve data z nové země, výsledkem data z několika zemích, které ukládají, výborný musíte tooupdate statistiky ve sloupci země hello.</span><span class="sxs-lookup"><span data-stu-id="b9165-150">However, if your data warehouse only contains one country and you bring in data from a new country, resulting in data from multiple countries being stored, then you definitely need tooupdate statistics on hello country column.</span></span>

<span data-ttu-id="b9165-151">Jeden z hello první otázky tooask při řešení potíží s dotazem, "jsou hello statistiky aktuální?"</span><span class="sxs-lookup"><span data-stu-id="b9165-151">One of hello first questions tooask when troubleshooting a query is, "Are hello statistics up-to-date?"</span></span>

<span data-ttu-id="b9165-152">Tento dotaz není ten, který může odpovídat hello stáří dat hello.</span><span class="sxs-lookup"><span data-stu-id="b9165-152">This question is not one that can be answered by hello age of hello data.</span></span> <span data-ttu-id="b9165-153">Objekt si statistiky toodate může být velmi staré, pokud byl žádné závažné změny toohello základní data.</span><span class="sxs-lookup"><span data-stu-id="b9165-153">An up toodate statistics object could be very old if there's been no material change toohello underlying data.</span></span> <span data-ttu-id="b9165-154">Když hello počet řádků, došlo ke změně podstatně nebo dojde ke změně podstatným hello rozdělení hodnot pro daný sloupec *pak* je čas tooupdate statistiky.</span><span class="sxs-lookup"><span data-stu-id="b9165-154">When hello number of rows has changed substantially or there is a material change in hello distribution of values for a given column *then* it's time tooupdate statistics.</span></span>  

<span data-ttu-id="b9165-155">Pro referenci **systému SQL Server** (ne SQL Data Warehouse) automaticky aktualizuje statistické údaje o těchto situacích:</span><span class="sxs-lookup"><span data-stu-id="b9165-155">For reference, **SQL Server** (not SQL Data Warehouse) automatically updates statistics for these situations:</span></span>

* <span data-ttu-id="b9165-156">Pokud máte nulový počet řádků v tabulce hello při přidání řádků, získáte automatickou aktualizaci statistik</span><span class="sxs-lookup"><span data-stu-id="b9165-156">If you have zero rows in hello table, when you add rows, you’ll get an automatic update of statistics</span></span>
* <span data-ttu-id="b9165-157">Když přidáte více než 500 tabulky tooa řádky začínající menší než 500 řádků (například při spuštění 499 a pak přidejte 500 celkový počet řádků tooa 999 řádků), získáte automatických aktualizací</span><span class="sxs-lookup"><span data-stu-id="b9165-157">When you add more than 500 rows tooa table starting with less than 500 rows (e.g. at start you have 499 and then add 500 rows tooa total of 999 rows), you’ll get an automatic update</span></span> 
* <span data-ttu-id="b9165-158">Jakmile jste více než 500 řádků budete mít tooadd 500 dodatečné řádky + 20 % velikosti hello hello tabulky předtím, než se zobrazí automatických aktualizací na hello statistiky</span><span class="sxs-lookup"><span data-stu-id="b9165-158">Once you’re over 500 rows you will have tooadd 500 additional rows + 20% of hello size of hello table before you’ll see an automatic update on hello stats</span></span>

<span data-ttu-id="b9165-159">Vzhledem k tomu, že neexistuje žádná DMV toodetermine Pokud došlo ke změně dat v rámci hello tabulky byly aktualizace hello poslední čas statistiky, znalost hello stáří statistice můžete nabízejí části hello obrázku.</span><span class="sxs-lookup"><span data-stu-id="b9165-159">Since there is no DMV toodetermine if data within hello table has changed since hello last time statistics were updated, knowing hello age of your statistics can provide you with part of hello picture.</span></span>  <span data-ttu-id="b9165-160">Můžete použít následující dotaz toodetermine hello čas poslední statistice hello tam, kde na každou tabulku aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="b9165-160">You can use hello following query toodetermine hello last time your statistics where updated on each table.</span></span>  

> [!NOTE]
> <span data-ttu-id="b9165-161">Mějte na paměti, že pokud dojde ke změně podstatným hello rozdělení hodnot pro daný sloupec, je třeba aktualizovat statistiku bez ohledu na to hello čas posledního jejich aktualizací.</span><span class="sxs-lookup"><span data-stu-id="b9165-161">Remember if there is a material change in hello distribution of values for a given column, you should update statistics regardless of hello last time they were updated.</span></span>  
> 
> 

```sql
SELECT
    sm.[name] AS [schema_name],
    tb.[name] AS [table_name],
    co.[name] AS [stats_column_name],
    st.[name] AS [stats_name],
    STATS_DATE(st.[object_id],st.[stats_id]) AS [stats_last_updated_date]
FROM
    sys.objects ob
    JOIN sys.stats st
        ON  ob.[object_id] = st.[object_id]
    JOIN sys.stats_columns sc    
        ON  st.[stats_id] = sc.[stats_id]
        AND st.[object_id] = sc.[object_id]
    JOIN sys.columns co    
        ON  sc.[column_id] = co.[column_id]
        AND sc.[object_id] = co.[object_id]
    JOIN sys.types  ty    
        ON  co.[user_type_id] = ty.[user_type_id]
    JOIN sys.tables tb    
        ON  co.[object_id] = tb.[object_id]
    JOIN sys.schemas sm    
        ON  tb.[schema_id] = sm.[schema_id]
WHERE
    st.[user_created] = 1;
```

<span data-ttu-id="b9165-162">Sloupců s kalendářními daty v datovém skladu, například třeba často aktualizace statistiky.</span><span class="sxs-lookup"><span data-stu-id="b9165-162">Date columns in a data warehouse, for example, usually need frequent statistics updates.</span></span> <span data-ttu-id="b9165-163">Každé nové řádky času jsou načtená do datového skladu hello, nové zatížení kalendářní data nebo data transakcí se přidají.</span><span class="sxs-lookup"><span data-stu-id="b9165-163">Each time new rows are loaded into hello data warehouse, new load dates or transaction dates are added.</span></span> <span data-ttu-id="b9165-164">Tyto změnit hello distribuci dat a proveďte hello statistiky zastaralé.</span><span class="sxs-lookup"><span data-stu-id="b9165-164">These change hello data distribution and make hello statistics out-of-date.</span></span>  <span data-ttu-id="b9165-165">Statistiky o pohlaví sloupce pro tabulku zákazníků a naopak, může být nutné nikdy toobe aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="b9165-165">Conversely, statistics on a gender column on a customer table might never need toobe updated.</span></span> <span data-ttu-id="b9165-166">Za předpokladu, že distribuce hello je konstantní mezi odběrateli, přidání nové řádky toohello tabulky variace není přejdete distribuci dat toochange hello.</span><span class="sxs-lookup"><span data-stu-id="b9165-166">Assuming hello distribution is constant between customers, adding new rows toohello table variation isn't going toochange hello data distribution.</span></span> <span data-ttu-id="b9165-167">Ale pokud váš datový sklad obsahuje pouze jeden pohlaví a nový požadavek výsledkem více pohlaví výborný musíte tooupdate statistiku hello pohlaví sloupce.</span><span class="sxs-lookup"><span data-stu-id="b9165-167">However, if your data warehouse only contains one gender and a new requirement results in multiple genders then you definitely need tooupdate statistics on hello gender column.</span></span>

<span data-ttu-id="b9165-168">Další informace naleznete v části [statistiky] [ Statistics] na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="b9165-168">For further explanation, see [Statistics][Statistics] on MSDN.</span></span>

## <a name="implementing-statistics-management"></a><span data-ttu-id="b9165-169">Implementace správy statistiky</span><span class="sxs-lookup"><span data-stu-id="b9165-169">Implementing statistics management</span></span>
<span data-ttu-id="b9165-170">Často je vhodné tooextend hello data načítání tooensure proces, který se statistika aktualizuje na konci hello zatížení.</span><span class="sxs-lookup"><span data-stu-id="b9165-170">It is often a good idea tooextend your data loading process tooensure that statistics are updated at hello end of hello load.</span></span> <span data-ttu-id="b9165-171">načtení dat Hello je při tabulky nejčastěji změnit jejich velikost a jejich distribuci hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b9165-171">hello data load is when tables most frequently change their size and/or their distribution of values.</span></span> <span data-ttu-id="b9165-172">To je proto logické místní tooimplement některé procesy správy.</span><span class="sxs-lookup"><span data-stu-id="b9165-172">Therefore, this is a logical place tooimplement some management processes.</span></span>

<span data-ttu-id="b9165-173">Některé zásady jsou uvedené pro aktualizaci statistice během procesu načítání hello:</span><span class="sxs-lookup"><span data-stu-id="b9165-173">Some guiding principles are provided below for updating your statistics during hello load process:</span></span>

* <span data-ttu-id="b9165-174">Zajistěte, aby každá tabulka načíst minimálně jeden objekt statistiky aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="b9165-174">Ensure that each loaded table has at least one statistics object updated.</span></span> <span data-ttu-id="b9165-175">Tato aktualizace hello tabulky informace o velikosti (počet řádků a počet stránek) jako součást aktualizace statistiky hello.</span><span class="sxs-lookup"><span data-stu-id="b9165-175">This updates hello tables size (row count and page count) information as part of hello stats update.</span></span>
* <span data-ttu-id="b9165-176">Zaměřit se na sloupců podílejících se na připojení, GROUP BY, ORDER BY a DISTINCT – klauzule</span><span class="sxs-lookup"><span data-stu-id="b9165-176">Focus on columns participating in JOIN, GROUP BY, ORDER BY and DISTINCT clauses</span></span>
* <span data-ttu-id="b9165-177">Zvažte aktualizaci data častěji, jak tyto hodnoty nebudou zahrnuty do histogram statistiky hello "vzestupné klíč" sloupců, jako je například transakce.</span><span class="sxs-lookup"><span data-stu-id="b9165-177">Consider updating "ascending key" columns such as transaction dates more frequently as these values will not be included in hello statistics histogram.</span></span>
* <span data-ttu-id="b9165-178">Zvažte aktualizaci statické distribuční sloupce méně často.</span><span class="sxs-lookup"><span data-stu-id="b9165-178">Consider updating static distribution columns less frequently.</span></span>
* <span data-ttu-id="b9165-179">Mějte na paměti, že každý objekt statistiky je aktualizovat v řadě.</span><span class="sxs-lookup"><span data-stu-id="b9165-179">Remember each statistic object is updated in series.</span></span> <span data-ttu-id="b9165-180">Implementací `UPDATE STATISTICS <TABLE_NAME>` nemusí být právě ideální - hlavně pro široké tabulky s mnoha objekty statistiky.</span><span class="sxs-lookup"><span data-stu-id="b9165-180">Simply implementing `UPDATE STATISTICS <TABLE_NAME>` may not be ideal - especially for wide tables with lots of statistics objects.</span></span>

> [!NOTE]
> <span data-ttu-id="b9165-181">Další podrobnosti na [vzestupné klíč] naleznete v dokumentu White Paper toohello SQL Server 2014 mohutnost odhad modelu.</span><span class="sxs-lookup"><span data-stu-id="b9165-181">For more details on [ascending key] please refer toohello SQL Server 2014 cardinality estimation model whitepaper.</span></span>
> 
> 

<span data-ttu-id="b9165-182">Další informace naleznete v části [odhadu kardinality] [ Cardinality Estimation] na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="b9165-182">For further explanation, see  [Cardinality Estimation][Cardinality Estimation] on MSDN.</span></span>

## <a name="examples-create-statistics"></a><span data-ttu-id="b9165-183">Příklady: Vytvoření statistiky</span><span class="sxs-lookup"><span data-stu-id="b9165-183">Examples: Create statistics</span></span>
<span data-ttu-id="b9165-184">Tyto příklady ukazují, jak toouse různé možnosti pro vytvoření statistiky.</span><span class="sxs-lookup"><span data-stu-id="b9165-184">These examples show how toouse various options for creating statistics.</span></span> <span data-ttu-id="b9165-185">Hello možnosti, které používáte pro každý sloupec závisí na vlastnosti hello vašich dat a použití hello sloupec v dotazech.</span><span class="sxs-lookup"><span data-stu-id="b9165-185">hello options that you use for each column depend on hello characteristics of your data and how hello column will be used in queries.</span></span>

### <a name="a-create-single-column-statistics-with-default-options"></a><span data-ttu-id="b9165-186">A.</span><span class="sxs-lookup"><span data-stu-id="b9165-186">A.</span></span> <span data-ttu-id="b9165-187">Vytvoření statistiky jednoho sloupce s výchozími možnostmi</span><span class="sxs-lookup"><span data-stu-id="b9165-187">Create single-column statistics with default options</span></span>
<span data-ttu-id="b9165-188">statistiky toocreate na sloupci, jednoduše zadejte název pro objekt hello statistiky a hello název sloupce hello.</span><span class="sxs-lookup"><span data-stu-id="b9165-188">toocreate statistics on a column, simply provide a name for hello statistics object and hello name of hello column.</span></span>

<span data-ttu-id="b9165-189">Všechny výchozí možnosti hello využívá tuto syntaxi.</span><span class="sxs-lookup"><span data-stu-id="b9165-189">This syntax uses all of hello default options.</span></span> <span data-ttu-id="b9165-190">Ve výchozím nastavení ukázky SQL Data Warehouse 20 procent hello tabulky při vytváření statistik.</span><span class="sxs-lookup"><span data-stu-id="b9165-190">By default, SQL Data Warehouse samples 20 percent of hello table when it creates statistics.</span></span>

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]);
```

<span data-ttu-id="b9165-191">Například:</span><span class="sxs-lookup"><span data-stu-id="b9165-191">For example:</span></span>

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1);
```

### <a name="b-create-single-column-statistics-by-examining-every-row"></a><span data-ttu-id="b9165-192">B.</span><span class="sxs-lookup"><span data-stu-id="b9165-192">B.</span></span> <span data-ttu-id="b9165-193">Vytvořit jednosloupcovou statistiku tak, že prověří každý řádek</span><span class="sxs-lookup"><span data-stu-id="b9165-193">Create single-column statistics by examining every row</span></span>
<span data-ttu-id="b9165-194">pro většinu situacích stačí Hello výchozí vzorkovací frekvenci 20 procent.</span><span class="sxs-lookup"><span data-stu-id="b9165-194">hello default sampling rate of 20 percent is sufficient for most situations.</span></span> <span data-ttu-id="b9165-195">Můžete však upravit vzorkovací frekvenci hello.</span><span class="sxs-lookup"><span data-stu-id="b9165-195">However, you can adjust hello sampling rate.</span></span>

<span data-ttu-id="b9165-196">toosample hello úplné tabulky, použijte tuto syntaxi:</span><span class="sxs-lookup"><span data-stu-id="b9165-196">toosample hello full table, use this syntax:</span></span>

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]) WITH FULLSCAN;
```

<span data-ttu-id="b9165-197">Například:</span><span class="sxs-lookup"><span data-stu-id="b9165-197">For example:</span></span>

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH FULLSCAN;
```

### <a name="c-create-single-column-statistics-by-specifying-hello-sample-size"></a><span data-ttu-id="b9165-198">C.</span><span class="sxs-lookup"><span data-stu-id="b9165-198">C.</span></span> <span data-ttu-id="b9165-199">Vytvořte jednosloupcovou statistiku zadáním velikost vzorku hello</span><span class="sxs-lookup"><span data-stu-id="b9165-199">Create single-column statistics by specifying hello sample size</span></span>
<span data-ttu-id="b9165-200">Alternativně můžete určit velikost vzorku hello v procentech:</span><span class="sxs-lookup"><span data-stu-id="b9165-200">Alternatively, you can specify hello sample size as a percent:</span></span>

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH SAMPLE = 50 PERCENT;
```

### <a name="d-create-single-column-statistics-on-only-some-of-hello-rows"></a><span data-ttu-id="b9165-201">D.</span><span class="sxs-lookup"><span data-stu-id="b9165-201">D.</span></span> <span data-ttu-id="b9165-202">Vytvořit jednosloupcovou statistiku pro jenom některé řádky hello</span><span class="sxs-lookup"><span data-stu-id="b9165-202">Create single-column statistics on only some of hello rows</span></span>
<span data-ttu-id="b9165-203">Další možností statistiky můžete vytvořit na část hello řádky v tabulce.</span><span class="sxs-lookup"><span data-stu-id="b9165-203">Another option, you can create statistics on a portion of hello rows in your table.</span></span> <span data-ttu-id="b9165-204">Tomu se říká filtrované statistiky.</span><span class="sxs-lookup"><span data-stu-id="b9165-204">This is called a filtered statistic.</span></span>

<span data-ttu-id="b9165-205">Můžete například použít filtrovanou statistiku při plánování tooquery na konkrétní oddíl velkých oddílů tabulky.</span><span class="sxs-lookup"><span data-stu-id="b9165-205">For example, you could use filtered statistics when you plan tooquery a specific partition of a large partitioned table.</span></span> <span data-ttu-id="b9165-206">Vytvořením statistiky na pouze hello oddílu hodnoty, bude hello přesnost hello statistiky zlepšit a proto zlepšit výkon dotazu.</span><span class="sxs-lookup"><span data-stu-id="b9165-206">By creating statistics on only hello partition values, hello accuracy of hello statistics will improve, and therefore improve query performance.</span></span>

<span data-ttu-id="b9165-207">Tento příklad vytvoří statistiky na rozsah hodnot.</span><span class="sxs-lookup"><span data-stu-id="b9165-207">This example creates statistics on a range of values.</span></span> <span data-ttu-id="b9165-208">hodnoty Hello by snadno mohlo být definovaný v oddílu toomatch hello rozsah hodnot.</span><span class="sxs-lookup"><span data-stu-id="b9165-208">hello values could easily be defined toomatch hello range of values in a partition.</span></span>

```sql
CREATE STATISTICS stats_col1 ON table1(col1) WHERE col1 > '2000101' AND col1 < '20001231';
```

> [!NOTE]
> <span data-ttu-id="b9165-209">Pro hello dotazu pro optimalizaci tooconsider pomocí filtrovanou statistiku, když se vybere hello distribuovaného dotazu plán musí dotaz hello vejít do hello definici objektu statistiky hello.</span><span class="sxs-lookup"><span data-stu-id="b9165-209">For hello query optimizer tooconsider using filtered statistics when it chooses hello distributed query plan, hello query must fit inside hello definition of hello statistics object.</span></span> <span data-ttu-id="b9165-210">Použijeme předchozí příklad hello hello dotazu kde klauzule musí toospecify Sloupec1 hodnoty mezi 2000101 a 20001231.</span><span class="sxs-lookup"><span data-stu-id="b9165-210">Using hello previous example, hello query's where clause needs toospecify col1 values between 2000101 and 20001231.</span></span>
> 
> 

### <a name="e-create-single-column-statistics-with-all-hello-options"></a><span data-ttu-id="b9165-211">E.</span><span class="sxs-lookup"><span data-stu-id="b9165-211">E.</span></span> <span data-ttu-id="b9165-212">Vytvořit jednosloupcovou statistiku se všemi možnostmi hello</span><span class="sxs-lookup"><span data-stu-id="b9165-212">Create single-column statistics with all hello options</span></span>
<span data-ttu-id="b9165-213">Samozřejmě můžete, možnosti hello kombinovat společně.</span><span class="sxs-lookup"><span data-stu-id="b9165-213">You can, of course, combine hello options together.</span></span> <span data-ttu-id="b9165-214">Následující příklad Hello vytvoří objekt filtrovanou statistiku s velikost vlastní vzorku:</span><span class="sxs-lookup"><span data-stu-id="b9165-214">hello example below creates a filtered statistics object with a custom sample size:</span></span>

```sql
CREATE STATISTICS stats_col1 ON table1 (col1) WHERE col1 > '2000101' AND col1 < '20001231' WITH SAMPLE = 50 PERCENT;
```

<span data-ttu-id="b9165-215">Hello úplný přehled najdete v tématu [CREATE STATISTICS] [ CREATE STATISTICS] na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="b9165-215">For hello full reference, see [CREATE STATISTICS][CREATE STATISTICS] on MSDN.</span></span>

### <a name="f-create-multi-column-statistics"></a><span data-ttu-id="b9165-216">F.</span><span class="sxs-lookup"><span data-stu-id="b9165-216">F.</span></span> <span data-ttu-id="b9165-217">Vytvoření statistiky více sloupci</span><span class="sxs-lookup"><span data-stu-id="b9165-217">Create multi-column statistics</span></span>
<span data-ttu-id="b9165-218">toocreate vícesloupcového statistik, jednoduše použijte hello předchozích příkladech, ale zadat více sloupců.</span><span class="sxs-lookup"><span data-stu-id="b9165-218">toocreate a multi-column statistics, simply use hello previous examples, but specify more columns.</span></span>

> [!NOTE]
> <span data-ttu-id="b9165-219">Hello histogram, který se používá tooestimate počet řádků ve výsledku dotazu hello, je dostupná jenom pro první sloupec hello uvedené v definici objektu statistiky hello.</span><span class="sxs-lookup"><span data-stu-id="b9165-219">hello histogram, which is used tooestimate number of rows in hello query result, is only available for hello first column listed in hello statistics object definition.</span></span>
> 
> 

<span data-ttu-id="b9165-220">V tomto příkladu je hello histogram na *produktu\_kategorie*.</span><span class="sxs-lookup"><span data-stu-id="b9165-220">In this example, hello histogram is on *product\_category*.</span></span> <span data-ttu-id="b9165-221">Statistiky mezi sloupce jsou vypočítány na *produktu\_kategorie* a *produktu\_sub_c\ategory*:</span><span class="sxs-lookup"><span data-stu-id="b9165-221">Cross-column statistics are calculated on *product\_category* and *product\_sub_c\ategory*:</span></span>

```sql
CREATE STATISTICS stats_2cols ON table1 (product_category, product_sub_category) WHERE product_category > '2000101' AND product_category < '20001231' WITH SAMPLE = 50 PERCENT;
```

<span data-ttu-id="b9165-222">Vzhledem k tomu, že existuje korelace mezi *produktu\_kategorie* a *produktu\_sub\_kategorie*, může být užitečné, pokud tyto sloupce, ke kterým se přistupuje vícesloupcového stat v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="b9165-222">Since there is a correlation between *product\_category* and *product\_sub\_category*, a multi-column stat can be useful if these columns are accessed at hello same time.</span></span>

### <a name="g-create-statistics-on-all-hello-columns-in-a-table"></a><span data-ttu-id="b9165-223">G.</span><span class="sxs-lookup"><span data-stu-id="b9165-223">G.</span></span> <span data-ttu-id="b9165-224">Vytvoření statistiky pro všechny hello sloupců v tabulce.</span><span class="sxs-lookup"><span data-stu-id="b9165-224">Create statistics on all hello columns in a table</span></span>
<span data-ttu-id="b9165-225">Jedním ze způsobů toocreate statistiky je po vytvoření tabulky hello tooissues příkazy CREATE STATISTICS.</span><span class="sxs-lookup"><span data-stu-id="b9165-225">One way toocreate statistics is tooissues CREATE STATISTICS commands after creating hello table.</span></span>

```sql
CREATE TABLE dbo.table1
(
   col1 int
,  col2 int
,  col3 int
)
WITH
  (
    CLUSTERED COLUMNSTORE INDEX
  )
;

CREATE STATISTICS stats_col1 on dbo.table1 (col1);
CREATE STATISTICS stats_col2 on dbo.table2 (col2);
CREATE STATISTICS stats_col3 on dbo.table3 (col3);
```

### <a name="h-use-a-stored-procedure-toocreate-statistics-on-all-columns-in-a-database"></a><span data-ttu-id="b9165-226">H.</span><span class="sxs-lookup"><span data-stu-id="b9165-226">H.</span></span> <span data-ttu-id="b9165-227">Uložené procedury toocreate statistik použití na všechny sloupce v databázi</span><span class="sxs-lookup"><span data-stu-id="b9165-227">Use a stored procedure toocreate statistics on all columns in a database</span></span>
<span data-ttu-id="b9165-228">SQL Data Warehouse nemá ekvivalentu uložené procedury systém příliš [] [sp_create_stats] v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b9165-228">SQL Data Warehouse does not have a system stored procedure equivalent too[sp_create_stats][] in SQL Server.</span></span> <span data-ttu-id="b9165-229">Tato uložená procedura vytvoří objekt statistiky jeden sloupec pro každý sloupec hello databáze, který ještě nemá statistiky.</span><span class="sxs-lookup"><span data-stu-id="b9165-229">This stored procedure creates a single column statistics object on every column of hello database that doesn't already have statistics.</span></span>

<span data-ttu-id="b9165-230">To vám pomůže začít pracovat s návrhu databáze.</span><span class="sxs-lookup"><span data-stu-id="b9165-230">This will help you get started with your database design.</span></span> <span data-ttu-id="b9165-231">Myslíte, že volné tooadapt ho tooyour potřebuje.</span><span class="sxs-lookup"><span data-stu-id="b9165-231">Feel free tooadapt it tooyour needs.</span></span>

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_create_stats]
(   @create_type    tinyint -- 1 default 2 Fullscan 3 Sample
,   @sample_pct     tinyint
)
AS

IF @create_type NOT IN (1,2,3)
BEGIN
    THROW 151000,'Invalid value for @stats_type parameter. Valid range 1 (default), 2 (fullscan) or 3 (sample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN;
    DROP TABLE #stats_ddl;
END;

CREATE TABLE #stats_ddl
WITH    (   DISTRIBUTION    = HASH([seq_nmbr])
        ,   LOCATION        = USER_DB
        )
AS
WITH T
AS
(
SELECT      t.[name]                        AS [table_name]
,           s.[name]                        AS [table_schema_name]
,           c.[name]                        AS [column_name]
,           c.[column_id]                   AS [column_id]
,           t.[object_id]                   AS [object_id]
,           ROW_NUMBER()
            OVER(ORDER BY (SELECT NULL))    AS [seq_nmbr]
FROM        sys.[tables] t
JOIN        sys.[schemas] s         ON  t.[schema_id]       = s.[schema_id]
JOIN        sys.[columns] c         ON  t.[object_id]       = c.[object_id]
LEFT JOIN   sys.[stats_columns] l   ON  l.[object_id]       = c.[object_id]
                                    AND l.[column_id]       = c.[column_id]
                                    AND l.[stats_column_id] = 1
LEFT JOIN    sys.[external_tables] e    ON    e.[object_id]        = t.[object_id]
WHERE       l.[object_id] IS NULL
AND            e.[object_id] IS NULL -- not an external table
)
SELECT  [table_schema_name]
,       [table_name]
,       [column_name]
,       [column_id]
,       [object_id]
,       [seq_nmbr]
,       CASE @create_type
        WHEN 1
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+')' AS VARCHAR(8000))
        WHEN 2
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH FULLSCAN' AS VARCHAR(8000))
        WHEN 3
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH SAMPLE '+@sample_pct+'PERCENT' AS VARCHAR(8000))
        END AS create_stat_ddl
FROM T
;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''
;

WHILE @i <= @t
BEGIN
    SET @s=(SELECT create_stat_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

<span data-ttu-id="b9165-232">toocreate statistiky pro všechny sloupce v tabulce hello s Tento postup jednoduše volání procedury hello.</span><span class="sxs-lookup"><span data-stu-id="b9165-232">toocreate statistics on all columns in hello table with this procedure, simply call hello procedure.</span></span>

```sql
prc_sqldw_create_stats;
```

## <a name="examples-update-statistics"></a><span data-ttu-id="b9165-233">Příklady: aktualizovat statistiky</span><span class="sxs-lookup"><span data-stu-id="b9165-233">Examples: update statistics</span></span>
<span data-ttu-id="b9165-234">tooupdate statistiky, můžete postupovat následovně:</span><span class="sxs-lookup"><span data-stu-id="b9165-234">tooupdate statistics, you can:</span></span>

1. <span data-ttu-id="b9165-235">Aktualizujte jeden objekt statistiky.</span><span class="sxs-lookup"><span data-stu-id="b9165-235">Update one statistics object.</span></span> <span data-ttu-id="b9165-236">Zadejte název hello hello statistiku objektu, že chcete tooupdate.</span><span class="sxs-lookup"><span data-stu-id="b9165-236">Specify hello name of hello statistics object you wish tooupdate.</span></span>
2. <span data-ttu-id="b9165-237">Aktualizujte všechny statistiky objekty v tabulce.</span><span class="sxs-lookup"><span data-stu-id="b9165-237">Update all statistics objects on a table.</span></span> <span data-ttu-id="b9165-238">Zadejte název hello hello tabulky místo jeden objekt konkrétní statistiku.</span><span class="sxs-lookup"><span data-stu-id="b9165-238">Specify hello name of hello table instead of one specific statistics object.</span></span>

### <a name="a-update-one-specific-statistics-object"></a><span data-ttu-id="b9165-239">A.</span><span class="sxs-lookup"><span data-stu-id="b9165-239">A.</span></span> <span data-ttu-id="b9165-240">Aktualizovat jeden objekt konkrétní Statistika</span><span class="sxs-lookup"><span data-stu-id="b9165-240">Update one specific statistics object</span></span>
<span data-ttu-id="b9165-241">Použijte následující syntaxi tooupdate objekt konkrétní statistiku hello:</span><span class="sxs-lookup"><span data-stu-id="b9165-241">Use hello following syntax tooupdate a specific statistics object:</span></span>

```sql
UPDATE STATISTICS [schema_name].[table_name]([stat_name]);
```

<span data-ttu-id="b9165-242">Například:</span><span class="sxs-lookup"><span data-stu-id="b9165-242">For example:</span></span>

```sql
UPDATE STATISTICS [dbo].[table1] ([stats_col1]);
```

<span data-ttu-id="b9165-243">Při aktualizaci statistiky konkrétní objekty, můžete minimalizovat hello čas a prostředky požadované toomanage statistiky.</span><span class="sxs-lookup"><span data-stu-id="b9165-243">By updating specific statistics objects, you can minimize hello time and resources required toomanage statistics.</span></span> <span data-ttu-id="b9165-244">To vyžaduje, že některé chápat, ale toochoose hello nejlepší statistiky objekty tooupdate.</span><span class="sxs-lookup"><span data-stu-id="b9165-244">This requires some thought, though, toochoose hello best statistics objects tooupdate.</span></span>

### <a name="b-update-all-statistics-on-a-table"></a><span data-ttu-id="b9165-245">B.</span><span class="sxs-lookup"><span data-stu-id="b9165-245">B.</span></span> <span data-ttu-id="b9165-246">Aktualizovat všechny statistiky v tabulce</span><span class="sxs-lookup"><span data-stu-id="b9165-246">Update all statistics on a table</span></span>
<span data-ttu-id="b9165-247">Ukazuje to jednoduše aktualizuje všechny objekty statistiky hello v tabulce.</span><span class="sxs-lookup"><span data-stu-id="b9165-247">This shows a simple method for updating all hello statistics objects on a table.</span></span>

```sql
UPDATE STATISTICS [schema_name].[table_name];
```

<span data-ttu-id="b9165-248">Například:</span><span class="sxs-lookup"><span data-stu-id="b9165-248">For example:</span></span>

```sql
UPDATE STATISTICS dbo.table1;
```

<span data-ttu-id="b9165-249">Tento příkaz je snadno toouse.</span><span class="sxs-lookup"><span data-stu-id="b9165-249">This statement is easy toouse.</span></span> <span data-ttu-id="b9165-250">Jenom nezapomeňte to aktualizuje všechny statistické údaje o hello tabulky a proto může provést další práci, než je nezbytné.</span><span class="sxs-lookup"><span data-stu-id="b9165-250">Just remember this updates all statistics on hello table, and therefore might perform more work than is necessary.</span></span> <span data-ttu-id="b9165-251">Pokud výkon hello není problém, je to výborný hello nejúplnější a nejjednodušší způsob, jakým tooguarantee statistiky jsou aktuální.</span><span class="sxs-lookup"><span data-stu-id="b9165-251">If hello performance is not an issue, this is definitely hello easiest and most complete way tooguarantee statistics are up-to-date.</span></span>

> [!NOTE]
> <span data-ttu-id="b9165-252">Při aktualizaci všechny statistiky v tabulce se SQL Data Warehouse nepodporuje kontrolu toosample hello tabulku pro každou statistiku.</span><span class="sxs-lookup"><span data-stu-id="b9165-252">When updating all statistics on a table, SQL Data Warehouse does a scan toosample hello table for each statistics.</span></span> <span data-ttu-id="b9165-253">Pokud je tabulka hello velký, má mnoho sloupců a mnoho statistiky, může to být efektivnější jednotlivých statistiky tooupdate podle potřeb.</span><span class="sxs-lookup"><span data-stu-id="b9165-253">If hello table is large, has many columns, and many statistics, it might be more efficient tooupdate individual statistics based on need.</span></span>
> 
> 

<span data-ttu-id="b9165-254">Implementace `UPDATE STATISTICS` postupu najdete v tématu hello [dočasných tabulek] [ Temporary] článku.</span><span class="sxs-lookup"><span data-stu-id="b9165-254">For an implementation of an `UPDATE STATISTICS` procedure please see hello [Temporary Tables][Temporary] article.</span></span> <span data-ttu-id="b9165-255">Metoda implementace Hello je mírně odlišný toohello `CREATE STATISTICS` výše uvedeného postupu ale hello konečný výsledek je hello stejné.</span><span class="sxs-lookup"><span data-stu-id="b9165-255">hello implementation method is slightly different toohello `CREATE STATISTICS` procedure above but hello end result is hello same.</span></span>

<span data-ttu-id="b9165-256">Úplná syntaxe hello, najdete v části [Update Statistics] [ Update Statistics] na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="b9165-256">For hello full syntax, see [Update Statistics][Update Statistics] on MSDN.</span></span>

## <a name="statistics-metadata"></a><span data-ttu-id="b9165-257">Statistiky metadat</span><span class="sxs-lookup"><span data-stu-id="b9165-257">Statistics metadata</span></span>
<span data-ttu-id="b9165-258">Existuje několik systémové zobrazení a funkce, které můžete použít toofind informace o statistiky.</span><span class="sxs-lookup"><span data-stu-id="b9165-258">There are several system view and functions that you can use toofind information about statistics.</span></span> <span data-ttu-id="b9165-259">Například se zobrazí, pokud objekt statistiky může být zastaralé pomocí hello statistiky datum funkce toosee při statistiky byly naposledy vytvoření nebo aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="b9165-259">For example, you can see if a statistics object might be out-of-date by using hello stats-date function toosee when statistics were last created or updated.</span></span>

### <a name="catalog-views-for-statistics"></a><span data-ttu-id="b9165-260">Zobrazení katalogu pro statistiky</span><span class="sxs-lookup"><span data-stu-id="b9165-260">Catalog views for statistics</span></span>
<span data-ttu-id="b9165-261">Tato systémová zobrazení obsahují informace o statistiky:</span><span class="sxs-lookup"><span data-stu-id="b9165-261">These system views provide information about statistics:</span></span>

| <span data-ttu-id="b9165-262">Zobrazení katalogu</span><span class="sxs-lookup"><span data-stu-id="b9165-262">Catalog View</span></span> | <span data-ttu-id="b9165-263">Popis</span><span class="sxs-lookup"><span data-stu-id="b9165-263">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="b9165-264">[Sys.Columns][sys.columns]</span><span class="sxs-lookup"><span data-stu-id="b9165-264">[sys.columns][sys.columns]</span></span> |<span data-ttu-id="b9165-265">Jeden řádek pro každý sloupec.</span><span class="sxs-lookup"><span data-stu-id="b9165-265">One row for each column.</span></span> |
| <span data-ttu-id="b9165-266">[Sys.Objects][sys.objects]</span><span class="sxs-lookup"><span data-stu-id="b9165-266">[sys.objects][sys.objects]</span></span> |<span data-ttu-id="b9165-267">Jeden řádek pro každý objekt v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="b9165-267">One row for each object in hello database.</span></span> |
| <span data-ttu-id="b9165-268">[Sys.Schemas][sys.schemas]</span><span class="sxs-lookup"><span data-stu-id="b9165-268">[sys.schemas][sys.schemas]</span></span> |<span data-ttu-id="b9165-269">Jeden řádek pro každý schématu v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="b9165-269">One row for each schema in hello database.</span></span> |
| <span data-ttu-id="b9165-270">[Sys.stats][sys.stats]</span><span class="sxs-lookup"><span data-stu-id="b9165-270">[sys.stats][sys.stats]</span></span> |<span data-ttu-id="b9165-271">Jeden řádek pro každý objekt statistiky.</span><span class="sxs-lookup"><span data-stu-id="b9165-271">One row for each statistics object.</span></span> |
| <span data-ttu-id="b9165-272">[Sys.stats_columns][sys.stats_columns]</span><span class="sxs-lookup"><span data-stu-id="b9165-272">[sys.stats_columns][sys.stats_columns]</span></span> |<span data-ttu-id="b9165-273">Jeden řádek pro každý sloupec v objektu statistiky hello.</span><span class="sxs-lookup"><span data-stu-id="b9165-273">One row for each column in hello statistics object.</span></span> <span data-ttu-id="b9165-274">Odkazy Zpět toosys.columns.</span><span class="sxs-lookup"><span data-stu-id="b9165-274">Links back toosys.columns.</span></span> |
| <span data-ttu-id="b9165-275">[zobrazení Sys.Tables][sys.tables]</span><span class="sxs-lookup"><span data-stu-id="b9165-275">[sys.tables][sys.tables]</span></span> |<span data-ttu-id="b9165-276">Jeden řádek pro každou tabulku (zahrnuje externí tabulky).</span><span class="sxs-lookup"><span data-stu-id="b9165-276">One row for each table (includes external tables).</span></span> |
| <span data-ttu-id="b9165-277">[Sys.table_types][sys.table_types]</span><span class="sxs-lookup"><span data-stu-id="b9165-277">[sys.table_types][sys.table_types]</span></span> |<span data-ttu-id="b9165-278">Jeden řádek pro každý typ dat.</span><span class="sxs-lookup"><span data-stu-id="b9165-278">One row for each data type.</span></span> |

### <a name="system-functions-for-statistics"></a><span data-ttu-id="b9165-279">Funkce systému pro statistiky</span><span class="sxs-lookup"><span data-stu-id="b9165-279">System functions for statistics</span></span>
<span data-ttu-id="b9165-280">Tyto funkce systému jsou užitečné pro práci s statistiky:</span><span class="sxs-lookup"><span data-stu-id="b9165-280">These system functions are useful for working with statistics:</span></span>

| <span data-ttu-id="b9165-281">System – funkce</span><span class="sxs-lookup"><span data-stu-id="b9165-281">System Function</span></span> | <span data-ttu-id="b9165-282">Popis</span><span class="sxs-lookup"><span data-stu-id="b9165-282">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="b9165-283">[STATS_DATE][STATS_DATE]</span><span class="sxs-lookup"><span data-stu-id="b9165-283">[STATS_DATE][STATS_DATE]</span></span> |<span data-ttu-id="b9165-284">Objekt statistiky hello datum poslední aktualizace.</span><span class="sxs-lookup"><span data-stu-id="b9165-284">Date hello statistics object was last updated.</span></span> |
| <span data-ttu-id="b9165-285">[PŘÍKAZ DBCC SHOW_STATISTICS][DBCC SHOW_STATISTICS]</span><span class="sxs-lookup"><span data-stu-id="b9165-285">[DBCC SHOW_STATISTICS][DBCC SHOW_STATISTICS]</span></span> |<span data-ttu-id="b9165-286">Poskytuje souhrn úrovně a podrobné informace o distribuci hello hodnot rozumí hello statistiku objektu.</span><span class="sxs-lookup"><span data-stu-id="b9165-286">Provides summary level and detailed information about hello distribution of values as understood by hello statistics object.</span></span> |

### <a name="combine-statistics-columns-and-functions-into-one-view"></a><span data-ttu-id="b9165-287">Zkombinovat do jednoho zobrazení statistiky sloupce a funkce</span><span class="sxs-lookup"><span data-stu-id="b9165-287">Combine statistics columns and functions into one view</span></span>
<span data-ttu-id="b9165-288">Toto zobrazení zobrazí sloupce, které se týkají toostatistics a výsledky z hello [STATS_DATE()] [] funkce společně.</span><span class="sxs-lookup"><span data-stu-id="b9165-288">This view brings columns that relate toostatistics, and results from hello [STATS_DATE()][]function together.</span></span>

```sql
CREATE VIEW dbo.vstats_columns
AS
SELECT
        sm.[name]                           AS [schema_name]
,       tb.[name]                           AS [table_name]
,       st.[name]                           AS [stats_name]
,       st.[filter_definition]              AS [stats_filter_defiinition]
,       st.[has_filter]                     AS [stats_is_filtered]
,       STATS_DATE(st.[object_id],st.[stats_id])
                                            AS [stats_last_updated_date]
,       co.[name]                           AS [stats_column_name]
,       ty.[name]                           AS [column_type]
,       co.[max_length]                     AS [column_max_length]
,       co.[precision]                      AS [column_precision]
,       co.[scale]                          AS [column_scale]
,       co.[is_nullable]                    AS [column_is_nullable]
,       co.[collation_name]                 AS [column_collation_name]
,       QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS two_part_name
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS three_part_name
FROM    sys.objects                         AS ob
JOIN    sys.stats           AS st ON    ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc ON    st.[stats_id]       = sc.[stats_id]
                            AND         st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co ON    sc.[column_id]      = co.[column_id]
                            AND         sc.[object_id]      = co.[object_id]
JOIN    sys.types           AS ty ON    co.[user_type_id]   = ty.[user_type_id]
JOIN    sys.tables          AS tb ON  co.[object_id]        = tb.[object_id]
JOIN    sys.schemas         AS sm ON  tb.[schema_id]        = sm.[schema_id]
WHERE   1=1
AND     st.[user_created] = 1
;
```

## <a name="dbcc-showstatistics-examples"></a><span data-ttu-id="b9165-289">Příkaz DBCC SHOW_STATISTICS() příklady</span><span class="sxs-lookup"><span data-stu-id="b9165-289">DBCC SHOW_STATISTICS() examples</span></span>
<span data-ttu-id="b9165-290">Příkaz DBCC SHOW_STATISTICS() ukazuje hello data ukládaná v rámci objektu statistiky.</span><span class="sxs-lookup"><span data-stu-id="b9165-290">DBCC SHOW_STATISTICS() shows hello data held within a statistics object.</span></span> <span data-ttu-id="b9165-291">Tato data je rozdělena na tři části.</span><span class="sxs-lookup"><span data-stu-id="b9165-291">This data comes in three parts.</span></span>

1. <span data-ttu-id="b9165-292">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="b9165-292">Header</span></span>
2. <span data-ttu-id="b9165-293">Hustotu vektoru</span><span class="sxs-lookup"><span data-stu-id="b9165-293">Density Vector</span></span>
3. <span data-ttu-id="b9165-294">Histogram</span><span class="sxs-lookup"><span data-stu-id="b9165-294">Histogram</span></span>

<span data-ttu-id="b9165-295">Hello záhlaví metadata o statistikách hello.</span><span class="sxs-lookup"><span data-stu-id="b9165-295">hello header metadata about hello statistics.</span></span> <span data-ttu-id="b9165-296">Hello histogram zobrazí hello distribuci hodnot v první klíčový sloupec hello hello statistiku objektu.</span><span class="sxs-lookup"><span data-stu-id="b9165-296">hello histogram displays hello distribution of values in hello first key column of hello statistics object.</span></span> <span data-ttu-id="b9165-297">vektor hustotu Hello měří korelace mezi sloupci.</span><span class="sxs-lookup"><span data-stu-id="b9165-297">hello density vector measures cross-column correlation.</span></span> <span data-ttu-id="b9165-298">SQLDW vypočítá mohutnost odhady s žádným z hello data hello statistiku objektu.</span><span class="sxs-lookup"><span data-stu-id="b9165-298">SQLDW computes cardinality estimates with any of hello data in hello statistics object.</span></span>

### <a name="show-header-density-and-histogram"></a><span data-ttu-id="b9165-299">Zobrazit záhlaví, hustotu a histogram</span><span class="sxs-lookup"><span data-stu-id="b9165-299">Show header, density, and histogram</span></span>
<span data-ttu-id="b9165-300">Tento jednoduchý příklad ukazuje všechny tři částí objektu statistiky.</span><span class="sxs-lookup"><span data-stu-id="b9165-300">This simple example shows all three parts of a statistics object.</span></span>

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>)
```

<span data-ttu-id="b9165-301">Například:</span><span class="sxs-lookup"><span data-stu-id="b9165-301">For example:</span></span>

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1);
```

### <a name="show-one-or-more-parts-of-dbcc-showstatistics"></a><span data-ttu-id="b9165-302">Zobrazit jednu nebo více částí DBCC SHOW_STATISTICS();</span><span class="sxs-lookup"><span data-stu-id="b9165-302">Show one or more parts of DBCC SHOW_STATISTICS();</span></span>
<span data-ttu-id="b9165-303">Pokud vás zajímá pouze v zobrazení konkrétní části, použijte hello `WITH` klauzule a určete, které jste části má toosee:</span><span class="sxs-lookup"><span data-stu-id="b9165-303">If you are only interested in viewing specific parts, use hello `WITH` clause and specify which parts you want toosee:</span></span>

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>) WITH stat_header, histogram, density_vector
```

<span data-ttu-id="b9165-304">Například:</span><span class="sxs-lookup"><span data-stu-id="b9165-304">For example:</span></span>

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1) WITH histogram, density_vector
```

## <a name="dbcc-showstatistics-differences"></a><span data-ttu-id="b9165-305">Příkaz DBCC SHOW_STATISTICS() rozdíly</span><span class="sxs-lookup"><span data-stu-id="b9165-305">DBCC SHOW_STATISTICS() differences</span></span>
<span data-ttu-id="b9165-306">Příkaz DBCC SHOW_STATISTICS() je implementováno více výhradně v SQL Data Warehouse porovnání tooSQL serveru.</span><span class="sxs-lookup"><span data-stu-id="b9165-306">DBCC SHOW_STATISTICS() is more strictly implemented in SQL Data Warehouse compared tooSQL Server.</span></span>

1. <span data-ttu-id="b9165-307">Nezdokumentovaný funkce nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="b9165-307">Undocumented features are not supported</span></span>
2. <span data-ttu-id="b9165-308">Nelze použít Stats_stream</span><span class="sxs-lookup"><span data-stu-id="b9165-308">Cannot use Stats_stream</span></span>
3. <span data-ttu-id="b9165-309">Se nemůže připojit k výsledky pro konkrétní podmnožiny dat statistiky např (STAT_HEADER spojení DENSITY_VECTOR)</span><span class="sxs-lookup"><span data-stu-id="b9165-309">Cannot join results for specific subsets of statistics data e.g. (STAT_HEADER JOIN DENSITY_VECTOR)</span></span>
4. <span data-ttu-id="b9165-310">NO_INFOMSGS nelze nastavit pro potlačení zprávy</span><span class="sxs-lookup"><span data-stu-id="b9165-310">NO_INFOMSGS cannot be set for message suppression</span></span>
5. <span data-ttu-id="b9165-311">Hranaté závorky kolem názvů statistiky nelze použít.</span><span class="sxs-lookup"><span data-stu-id="b9165-311">Square brackets around statistics names cannot be used</span></span>
6. <span data-ttu-id="b9165-312">Nelze použít sloupec názvy tooidentify statistiky objekty</span><span class="sxs-lookup"><span data-stu-id="b9165-312">Cannot use column names tooidentify statistics objects</span></span>
7. <span data-ttu-id="b9165-313">Vlastní chyba 2767 není podporovaná.</span><span class="sxs-lookup"><span data-stu-id="b9165-313">Custom error 2767 is not supported</span></span>

## <a name="next-steps"></a><span data-ttu-id="b9165-314">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b9165-314">Next steps</span></span>
<span data-ttu-id="b9165-315">Další podrobnosti najdete v tématu [DBCC SHOW_STATISTICS] [ DBCC SHOW_STATISTICS] na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="b9165-315">For more details, see [DBCC SHOW_STATISTICS][DBCC SHOW_STATISTICS] on MSDN.</span></span>  <span data-ttu-id="b9165-316">články hello toolearn více, najdete na [tabulky přehled][Overview], [tabulky datové typy][Data Types], [distribuci tabulku] [ Distribute], [Indexování tabulku][Index], [vytváření oddílů tabulky] [ Partition] a [ Dočasné tabulky][Temporary].</span><span class="sxs-lookup"><span data-stu-id="b9165-316">toolearn more, see hello articles on [Table Overview][Overview], [Table Data Types][Data Types], [Distributing a Table][Distribute], [Indexing a Table][Index],  [Partitioning a Table][Partition] and [Temporary Tables][Temporary].</span></span>  <span data-ttu-id="b9165-317">Další informace o osvědčených postupech najdete v tématu [SQL Data Warehouse osvědčené postupy][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="b9165-317">For more about best practices, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>  

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->  
[Cardinality Estimation]: https://msdn.microsoft.com/library/dn600374.aspx
[CREATE STATISTICS]: https://msdn.microsoft.com/library/ms188038.aspx
[DBCC SHOW_STATISTICS]:https://msdn.microsoft.com/library/ms174384.aspx
[Statistics]: https://msdn.microsoft.com/library/ms190397.aspx
[STATS_DATE]: https://msdn.microsoft.com/library/ms190330.aspx
[sys.columns]: https://msdn.microsoft.com/library/ms176106.aspx
[sys.objects]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.schemas]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.stats]: https://msdn.microsoft.com/library/ms177623.aspx
[sys.stats_columns]: https://msdn.microsoft.com/library/ms187340.aspx
[sys.tables]: https://msdn.microsoft.com/library/ms187406.aspx
[sys.table_types]: https://msdn.microsoft.com/library/bb510623.aspx
[UPDATE STATISTICS]: https://msdn.microsoft.com/library/ms187348.aspx

<!--Other Web references-->  
