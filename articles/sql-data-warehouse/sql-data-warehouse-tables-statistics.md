---
title: "Správa statistiky u tabulek v SQL Data Warehouse | Microsoft Docs"
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
ms.openlocfilehash: 1d5ded69e394643ddfc3de0c6d30dbd30c8e848f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="managing-statistics-on-tables-in-sql-data-warehouse"></a><span data-ttu-id="cb803-103">Správa statistiky u tabulek v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="cb803-103">Managing statistics on tables in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="cb803-104">[Přehled][Overview]</span><span class="sxs-lookup"><span data-stu-id="cb803-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="cb803-105">[Datové typy][Data Types]</span><span class="sxs-lookup"><span data-stu-id="cb803-105">[Data Types][Data Types]</span></span>
> * <span data-ttu-id="cb803-106">[Distribuce][Distribute]</span><span class="sxs-lookup"><span data-stu-id="cb803-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="cb803-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="cb803-107">[Index][Index]</span></span>
> * <span data-ttu-id="cb803-108">[Oddíl][Partition]</span><span class="sxs-lookup"><span data-stu-id="cb803-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="cb803-109">[Statistiky][Statistics]</span><span class="sxs-lookup"><span data-stu-id="cb803-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="cb803-110">[Dočasné][Temporary]</span><span class="sxs-lookup"><span data-stu-id="cb803-110">[Temporary][Temporary]</span></span>
> 
> 

<span data-ttu-id="cb803-111">Čím SQL Data Warehouse ví o vašich dat, tím rychleji se spouštět dotazy na data.</span><span class="sxs-lookup"><span data-stu-id="cb803-111">The more SQL Data Warehouse knows about your data, the faster it can execute queries against your data.</span></span>  <span data-ttu-id="cb803-112">Způsobem říct SQL Data Warehouse o vašich dat je shromažďování statistických údajů o vaše data.</span><span class="sxs-lookup"><span data-stu-id="cb803-112">The way that you tell SQL Data Warehouse about your data, is by collecting statistics about your data.</span></span>  <span data-ttu-id="cb803-113">S statistické údaje o vašich dat je jedním z nejdůležitějších kroků, které můžete provést za účelem optimalizace své dotazy.</span><span class="sxs-lookup"><span data-stu-id="cb803-113">Having statistics on your data is one of the most important things you can do to optimize your queries.</span></span>  <span data-ttu-id="cb803-114">Statistiky pomoci vytvořit optimální plán pro své dotazy SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="cb803-114">Statistics help SQL Data Warehouse create the most optimal plan for your queries.</span></span>  <span data-ttu-id="cb803-115">Je to proto, že je pro optimalizaci nákladů na základě Optimalizátor dotazů SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="cb803-115">This is because the SQL Data Warehouse query optimizer is a cost based optimizer.</span></span>  <span data-ttu-id="cb803-116">To znamená porovná náklady na různé plány dotazů a potom vybere plán s nejnižší náklady, které by se měly také plán, který provede nejrychlejší.</span><span class="sxs-lookup"><span data-stu-id="cb803-116">That is, it compares the cost of various query plans and then chooses the plan with the lowest cost, which should also be the plan that will execute the fastest.</span></span>

<span data-ttu-id="cb803-117">Statistiku lze vytvořit na jeden sloupec, více sloupců nebo index tabulky.</span><span class="sxs-lookup"><span data-stu-id="cb803-117">Statistics can be created on a single column, multiple columns or on an index of a table.</span></span>  <span data-ttu-id="cb803-118">Statistiky jsou uloženy v histogram, který zachycuje rozsah a selektivitu hodnot.</span><span class="sxs-lookup"><span data-stu-id="cb803-118">Statistics are stored in a histogram which captures the range and selectivity of values.</span></span>  <span data-ttu-id="cb803-119">Toto je zajímají hlavně o, když Optimalizátor potřebuje k vyhodnocení spojení, GROUP BY, HAVING a klauzule WHERE v dotazu.</span><span class="sxs-lookup"><span data-stu-id="cb803-119">This is of particular interest when the optimizer needs to evaluate JOINs, GROUP BY, HAVING and WHERE clauses in a query.</span></span>  <span data-ttu-id="cb803-120">Například pokud okně Optimalizace odhadne, že data jsou filtrování v dotazu vrátí 1 řádek, může vybrat, velmi jiné plánování než v případě ji odhadne jejich datum, na které jste vybrali bude vracet 1 milionu řádků.</span><span class="sxs-lookup"><span data-stu-id="cb803-120">For example, if the optimizer estimates that the date you are filtering in your query will return 1 row, it may choose a very different plan than if it estimates that they date you have selected will return 1 million rows.</span></span>  <span data-ttu-id="cb803-121">Při vytváření statistik je velmi důležité, je stejně důležité této statistiky *přesně* odráží aktuální stav tabulky.</span><span class="sxs-lookup"><span data-stu-id="cb803-121">While creating statistics is extremely important, it is equally important that statistics *accurately* reflect the current state of the table.</span></span>  <span data-ttu-id="cb803-122">S aktuální statistiky zajistí, že je kvalitní plán vybrány pomocí pro optimalizaci.</span><span class="sxs-lookup"><span data-stu-id="cb803-122">Having up-to-date statistics ensures that a good plan is selected by the optimizer.</span></span>  <span data-ttu-id="cb803-123">Plány vytvořené Optimalizátor jsou pouze jako vhodné jako statistiku na vaše data.</span><span class="sxs-lookup"><span data-stu-id="cb803-123">The plans created by the optimizer are only as good as the statistics on your data.</span></span>

<span data-ttu-id="cb803-124">Proces vytváření a aktualizaci statistiky je aktuálně ruční proces, ale je velmi jednoduchý udělat.</span><span class="sxs-lookup"><span data-stu-id="cb803-124">The process of creating and updating statistics is currently a manual process, but is very simple to do.</span></span>  <span data-ttu-id="cb803-125">To je rozdíl oproti systému SQL Server, který automaticky vytvoří a aktualizuje statistické údaje o jednoho sloupce a indexy.</span><span class="sxs-lookup"><span data-stu-id="cb803-125">This is unlike SQL Server which automatically creates and updates statistics on single columns and indexes.</span></span>  <span data-ttu-id="cb803-126">Pomocí následujících informací můžete výrazně automatizovat správu statistiku na vaše data.</span><span class="sxs-lookup"><span data-stu-id="cb803-126">By using the information below, you can greatly automate the management of the statistics on your data.</span></span> 

## <a name="getting-started-with-statistics"></a><span data-ttu-id="cb803-127">Začínáme s statistiky</span><span class="sxs-lookup"><span data-stu-id="cb803-127">Getting started with statistics</span></span>
 <span data-ttu-id="cb803-128">Vytvoření jen Vzorkovaná statistiku pro každý sloupec je snadný způsob, jak začít pracovat s statistiky.</span><span class="sxs-lookup"><span data-stu-id="cb803-128">Creating sampled statistics on every column is an easy way to get started with statistics.</span></span>  <span data-ttu-id="cb803-129">Vzhledem k tomu, že je stejně důležité k zachování aktualizovaného stavu statistiky, může být konzervativní přístup k aktualizaci statistice denně nebo po každé zatížení.</span><span class="sxs-lookup"><span data-stu-id="cb803-129">Since it is equally important to keep statistics up-to-date, a conservative approach may be to update your statistics daily or after each load.</span></span> <span data-ttu-id="cb803-130">Vždy existují kompromisy mezi výkonem a náklady na vytvoření a aktualizaci statistik.</span><span class="sxs-lookup"><span data-stu-id="cb803-130">There are always trade-offs between performance and the cost to create and update statistics.</span></span>  <span data-ttu-id="cb803-131">Pokud si myslíte, že údržba všech vašich statistik trvá příliš dlouho, možná byste měli pečlivěji vybírat sloupce, které mají statistiky, nebo sloupce, které vyžadují časté aktualizace.</span><span class="sxs-lookup"><span data-stu-id="cb803-131">If you find it is taking too long to maintain all of your statistics, you may want to try to be more selective about which columns have statistics or which columns need frequent updating.</span></span>  <span data-ttu-id="cb803-132">Například můžete chtít aktualizovat sloupců s kalendářními daty denně, jak lze přidat nové hodnoty spíše než po každé zatížení.</span><span class="sxs-lookup"><span data-stu-id="cb803-132">For example, you might want to update date columns daily, as new values may be added rather than after every load.</span></span> <span data-ttu-id="cb803-133">Znovu, budou co nejvíce výhod získáte tak, že statistiky na sloupce použité ve spojení, GROUP BY, HAVING a klauzule WHERE.</span><span class="sxs-lookup"><span data-stu-id="cb803-133">Again, you will gain the most benefit by having statistics on columns involved in JOINs, GROUP BY, HAVING and WHERE clauses.</span></span>  <span data-ttu-id="cb803-134">Pokud máte tabulku s velkým množstvím sloupce, které se používají jenom v klauzuli SELECT, nemusí být úspěšná statistiky pro tyto sloupce a výdaje trochu další úsilí k identifikaci pouze sloupce, které vám umožní statistiky, můžete snížit čas k udržování statistice.</span><span class="sxs-lookup"><span data-stu-id="cb803-134">If you have a table with a lot of columns which are only used in the SELECT clause, statistics on these columns may not help, and spending a little more effort to identify only the columns where statistics will help, can reduce the time to maintain your statistics.</span></span>

## <a name="multi-column-statistics"></a><span data-ttu-id="cb803-135">Statistiky více sloupce</span><span class="sxs-lookup"><span data-stu-id="cb803-135">Multi-column statistics</span></span>
<span data-ttu-id="cb803-136">Kromě vytvoření statistiky pro jednoho sloupce, můžete zjistit, vaše dotazy bude využívat více sloupci statistiky.</span><span class="sxs-lookup"><span data-stu-id="cb803-136">In addition to creating statistics on single columns, you may find that your queries will benefit from multi-column statistics.</span></span>  <span data-ttu-id="cb803-137">Statistiky více sloupce jsou statistiky vytvořena na seznam sloupců.</span><span class="sxs-lookup"><span data-stu-id="cb803-137">Multi-column statistics are statistics created on a list of columns.</span></span>  <span data-ttu-id="cb803-138">Obsahují jeden sloupec statistiky na první sloupec v seznamu a některé informace korelace mezi sloupci názvem densities –.</span><span class="sxs-lookup"><span data-stu-id="cb803-138">They include single column statistics on the first column in the list, plus some cross-column correlation information called densities.</span></span>  <span data-ttu-id="cb803-139">Například pokud máte tabulka, která se připojí k jiné na dva sloupce, můžete zjistit, že SQL Data Warehouse můžete lépe optimalizovat plánu v případě, že rozumí relaci mezi dvěma sloupci.</span><span class="sxs-lookup"><span data-stu-id="cb803-139">For example, if you have a table that joins to another on two columns, you may find that SQL Data Warehouse can better optimize the plan if it understands the relationship between two columns.</span></span>   <span data-ttu-id="cb803-140">Statistiky více sloupce můžete zlepšit výkon dotazu pro některé operace, jako je například složené spojení a seskupit podle.</span><span class="sxs-lookup"><span data-stu-id="cb803-140">Multi-column statistics can improve query performance for some operations such as composite joins and group by.</span></span>

## <a name="updating-statistics"></a><span data-ttu-id="cb803-141">Aktualizuje statistické údaje</span><span class="sxs-lookup"><span data-stu-id="cb803-141">Updating statistics</span></span>
<span data-ttu-id="cb803-142">Aktualizuje statistické údaje, je důležitou součástí rutiny správy vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="cb803-142">Updating statistics is an important part of your database management routine.</span></span>  <span data-ttu-id="cb803-143">Když se změní distribuci dat v databázi, je potřeba aktualizovat statistiku.</span><span class="sxs-lookup"><span data-stu-id="cb803-143">When the distribution of data in the database changes, statistics need to be updated.</span></span>  <span data-ttu-id="cb803-144">Zastaralé statistiky povede k dotazu optimální výkon.</span><span class="sxs-lookup"><span data-stu-id="cb803-144">Out-of-date statistics will lead to sub-optimal query performance.</span></span>

<span data-ttu-id="cb803-145">Jeden osvědčeným postupem je aktualizovat statistiku sloupců s kalendářními daty každý den při přidávání nová data.</span><span class="sxs-lookup"><span data-stu-id="cb803-145">One best practice is to update statistics on date columns each day as new dates are added.</span></span>  <span data-ttu-id="cb803-146">Každé nové řádky času jsou načtená do datového skladu, nové zatížení kalendářní data nebo data transakcí se přidají.</span><span class="sxs-lookup"><span data-stu-id="cb803-146">Each time new rows are loaded into the data warehouse, new load dates or transaction dates are added.</span></span> <span data-ttu-id="cb803-147">Tyto změnit distribuci dat a proveďte statistiku zastaralé.</span><span class="sxs-lookup"><span data-stu-id="cb803-147">These change the data distribution and make the statistics out-of-date.</span></span> <span data-ttu-id="cb803-148">Naopak statistiky země sloupec v tabulce zákazníka může být nikdy potřeba aktualizovat, jako rozdělení hodnot nemění obecně.</span><span class="sxs-lookup"><span data-stu-id="cb803-148">Conversely, statistics on a country column in a customer table might never need to be updated, as the distribution of values doesn’t generally change.</span></span> <span data-ttu-id="cb803-149">Za předpokladu, že distribuce je konstantní mezi odběrateli, přidávání nových řádků do tabulky variace není chystáte změnit rozdělení data.</span><span class="sxs-lookup"><span data-stu-id="cb803-149">Assuming the distribution is constant between customers, adding new rows to the table variation isn't going to change the data distribution.</span></span> <span data-ttu-id="cb803-150">Ale pokud váš datový sklad obsahuje pouze jeden země a připojte ve data z nové země, výsledkem data z několika zemích, které ukládají, pak výborný musíte aktualizovat statistiku na sloupci země.</span><span class="sxs-lookup"><span data-stu-id="cb803-150">However, if your data warehouse only contains one country and you bring in data from a new country, resulting in data from multiple countries being stored, then you definitely need to update statistics on the country column.</span></span>

<span data-ttu-id="cb803-151">Jedním z první otázky při řešení potíží s dotazu je "Jsou aktuální statistiky?"</span><span class="sxs-lookup"><span data-stu-id="cb803-151">One of the first questions to ask when troubleshooting a query is, "Are the statistics up-to-date?"</span></span>

<span data-ttu-id="cb803-152">Tento dotaz není ten, který může odpovídat stáří data.</span><span class="sxs-lookup"><span data-stu-id="cb803-152">This question is not one that can be answered by the age of the data.</span></span> <span data-ttu-id="cb803-153">Aktuální statistiku objektu může být velmi staré, pokud byl žádné závažné změny v základních datech.</span><span class="sxs-lookup"><span data-stu-id="cb803-153">An up to date statistics object could be very old if there's been no material change to the underlying data.</span></span> <span data-ttu-id="cb803-154">Pokud počet řádků, došlo ke změně podstatně nebo dojde ke změně podstatným v distribuci hodnot pro daný sloupec *pak* je třeba aktualizovat statistiku.</span><span class="sxs-lookup"><span data-stu-id="cb803-154">When the number of rows has changed substantially or there is a material change in the distribution of values for a given column *then* it's time to update statistics.</span></span>  

<span data-ttu-id="cb803-155">Pro referenci **systému SQL Server** (ne SQL Data Warehouse) automaticky aktualizuje statistické údaje o těchto situacích:</span><span class="sxs-lookup"><span data-stu-id="cb803-155">For reference, **SQL Server** (not SQL Data Warehouse) automatically updates statistics for these situations:</span></span>

* <span data-ttu-id="cb803-156">Pokud máte nulový počet řádků v tabulce, při přidání řádků, získáte automatickou aktualizaci statistik</span><span class="sxs-lookup"><span data-stu-id="cb803-156">If you have zero rows in the table, when you add rows, you’ll get an automatic update of statistics</span></span>
* <span data-ttu-id="cb803-157">Když přidáte více než 500 řádků do tabulky spuštění s menší než 500 řádky (například při spuštění máte 499 a pak přidejte 500 řádků do celkem 999 řádků), získáte automatických aktualizací</span><span class="sxs-lookup"><span data-stu-id="cb803-157">When you add more than 500 rows to a table starting with less than 500 rows (e.g. at start you have 499 and then add 500 rows to a total of 999 rows), you’ll get an automatic update</span></span> 
* <span data-ttu-id="cb803-158">Jakmile jste více než 500 řádků, budete muset přidat 500 dodatečné řádky + 20 % velikosti tabulky předtím, než se zobrazí na statistiky automatických aktualizací</span><span class="sxs-lookup"><span data-stu-id="cb803-158">Once you’re over 500 rows you will have to add 500 additional rows + 20% of the size of the table before you’ll see an automatic update on the stats</span></span>

<span data-ttu-id="cb803-159">Vzhledem k tomu, že neexistuje žádná DMV k určení, jestli data v tabulce se změnil od posledního statistiku časových údajů byly aktualizovány, znalost stáří statistice můžete nabízejí část obrázku.</span><span class="sxs-lookup"><span data-stu-id="cb803-159">Since there is no DMV to determine if data within the table has changed since the last time statistics were updated, knowing the age of your statistics can provide you with part of the picture.</span></span>  <span data-ttu-id="cb803-160">Následující dotaz můžete použít k určení poslední statistice tam, kde na každou tabulku aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="cb803-160">You can use the following query to determine the last time your statistics where updated on each table.</span></span>  

> [!NOTE]
> <span data-ttu-id="cb803-161">Mějte na paměti, že pokud dojde ke změně podstatným v distribuci hodnot pro daný sloupec, by měl aktualizovat statistiku bez ohledu na to, kdy se aktualizovaly naposledy.</span><span class="sxs-lookup"><span data-stu-id="cb803-161">Remember if there is a material change in the distribution of values for a given column, you should update statistics regardless of the last time they were updated.</span></span>  
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

<span data-ttu-id="cb803-162">Sloupců s kalendářními daty v datovém skladu, například třeba často aktualizace statistiky.</span><span class="sxs-lookup"><span data-stu-id="cb803-162">Date columns in a data warehouse, for example, usually need frequent statistics updates.</span></span> <span data-ttu-id="cb803-163">Každé nové řádky času jsou načtená do datového skladu, nové zatížení kalendářní data nebo data transakcí se přidají.</span><span class="sxs-lookup"><span data-stu-id="cb803-163">Each time new rows are loaded into the data warehouse, new load dates or transaction dates are added.</span></span> <span data-ttu-id="cb803-164">Tyto změnit distribuci dat a proveďte statistiku zastaralé.</span><span class="sxs-lookup"><span data-stu-id="cb803-164">These change the data distribution and make the statistics out-of-date.</span></span>  <span data-ttu-id="cb803-165">Naopak statistiky o pohlaví sloupec v tabulce zákazníka může být nikdy potřeba aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="cb803-165">Conversely, statistics on a gender column on a customer table might never need to be updated.</span></span> <span data-ttu-id="cb803-166">Za předpokladu, že distribuce je konstantní mezi odběrateli, přidávání nových řádků do tabulky variace není chystáte změnit rozdělení data.</span><span class="sxs-lookup"><span data-stu-id="cb803-166">Assuming the distribution is constant between customers, adding new rows to the table variation isn't going to change the data distribution.</span></span> <span data-ttu-id="cb803-167">Ale pokud váš datový sklad obsahuje pouze jeden pohlaví a nový požadavek výsledkem více pohlaví výborný musíte aktualizovat statistiku na sloupci pohlaví.</span><span class="sxs-lookup"><span data-stu-id="cb803-167">However, if your data warehouse only contains one gender and a new requirement results in multiple genders then you definitely need to update statistics on the gender column.</span></span>

<span data-ttu-id="cb803-168">Další informace naleznete v části [statistiky] [ Statistics] na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="cb803-168">For further explanation, see [Statistics][Statistics] on MSDN.</span></span>

## <a name="implementing-statistics-management"></a><span data-ttu-id="cb803-169">Implementace správy statistiky</span><span class="sxs-lookup"><span data-stu-id="cb803-169">Implementing statistics management</span></span>
<span data-ttu-id="cb803-170">Často je vhodné rozšířit vaše data načítání procesu zajistit, že se statistika aktualizuje na konci zatížení.</span><span class="sxs-lookup"><span data-stu-id="cb803-170">It is often a good idea to extend your data loading process to ensure that statistics are updated at the end of the load.</span></span> <span data-ttu-id="cb803-171">Načtení dat je při tabulky nejčastěji změnit jejich velikost a jejich distribuci hodnoty.</span><span class="sxs-lookup"><span data-stu-id="cb803-171">The data load is when tables most frequently change their size and/or their distribution of values.</span></span> <span data-ttu-id="cb803-172">To je proto logické místo, kde můžete implementovat některé procesy správy.</span><span class="sxs-lookup"><span data-stu-id="cb803-172">Therefore, this is a logical place to implement some management processes.</span></span>

<span data-ttu-id="cb803-173">Některé zásady jsou uvedené pro aktualizaci statistice během procesu načítání:</span><span class="sxs-lookup"><span data-stu-id="cb803-173">Some guiding principles are provided below for updating your statistics during the load process:</span></span>

* <span data-ttu-id="cb803-174">Zajistěte, aby každá tabulka načíst minimálně jeden objekt statistiky aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="cb803-174">Ensure that each loaded table has at least one statistics object updated.</span></span> <span data-ttu-id="cb803-175">Tím se aktualizuje informace o velikosti (počet řádků a počet stránek) tabulky jako součást aktualizace statistiky.</span><span class="sxs-lookup"><span data-stu-id="cb803-175">This updates the tables size (row count and page count) information as part of the stats update.</span></span>
* <span data-ttu-id="cb803-176">Zaměřit se na sloupců podílejících se na připojení, GROUP BY, ORDER BY a DISTINCT – klauzule</span><span class="sxs-lookup"><span data-stu-id="cb803-176">Focus on columns participating in JOIN, GROUP BY, ORDER BY and DISTINCT clauses</span></span>
* <span data-ttu-id="cb803-177">Zvažte aktualizaci "vzestupné klíč" sloupců, jako je například transakce častěji, jak tyto hodnoty nebudou zahrnuty do statistiky histogram kalendářní data.</span><span class="sxs-lookup"><span data-stu-id="cb803-177">Consider updating "ascending key" columns such as transaction dates more frequently as these values will not be included in the statistics histogram.</span></span>
* <span data-ttu-id="cb803-178">Zvažte aktualizaci statické distribuční sloupce méně často.</span><span class="sxs-lookup"><span data-stu-id="cb803-178">Consider updating static distribution columns less frequently.</span></span>
* <span data-ttu-id="cb803-179">Mějte na paměti, že každý objekt statistiky je aktualizovat v řadě.</span><span class="sxs-lookup"><span data-stu-id="cb803-179">Remember each statistic object is updated in series.</span></span> <span data-ttu-id="cb803-180">Implementací `UPDATE STATISTICS <TABLE_NAME>` nemusí být právě ideální - hlavně pro široké tabulky s mnoha objekty statistiky.</span><span class="sxs-lookup"><span data-stu-id="cb803-180">Simply implementing `UPDATE STATISTICS <TABLE_NAME>` may not be ideal - especially for wide tables with lots of statistics objects.</span></span>

> [!NOTE]
> <span data-ttu-id="cb803-181">Další podrobnosti na [vzestupné klíče] naleznete v SQL serveru 2014 mohutnost odhad modelu dokumentu White Paper.</span><span class="sxs-lookup"><span data-stu-id="cb803-181">For more details on [ascending key] please refer to the SQL Server 2014 cardinality estimation model whitepaper.</span></span>
> 
> 

<span data-ttu-id="cb803-182">Další informace naleznete v části [odhadu kardinality] [ Cardinality Estimation] na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="cb803-182">For further explanation, see  [Cardinality Estimation][Cardinality Estimation] on MSDN.</span></span>

## <a name="examples-create-statistics"></a><span data-ttu-id="cb803-183">Příklady: Vytvoření statistiky</span><span class="sxs-lookup"><span data-stu-id="cb803-183">Examples: Create statistics</span></span>
<span data-ttu-id="cb803-184">Tyto příklady ukazují, jak používat různé možnosti pro vytvoření statistiky.</span><span class="sxs-lookup"><span data-stu-id="cb803-184">These examples show how to use various options for creating statistics.</span></span> <span data-ttu-id="cb803-185">Možnosti, které používáte pro každý sloupec závisí na vlastnosti data a jak sloupec se použije v dotazech.</span><span class="sxs-lookup"><span data-stu-id="cb803-185">The options that you use for each column depend on the characteristics of your data and how the column will be used in queries.</span></span>

### <a name="a-create-single-column-statistics-with-default-options"></a><span data-ttu-id="cb803-186">A.</span><span class="sxs-lookup"><span data-stu-id="cb803-186">A.</span></span> <span data-ttu-id="cb803-187">Vytvoření statistiky jednoho sloupce s výchozími možnostmi</span><span class="sxs-lookup"><span data-stu-id="cb803-187">Create single-column statistics with default options</span></span>
<span data-ttu-id="cb803-188">Chcete-li vytvoření statistiky pro sloupec, stačí zadáte název pro objekt statistiky a název sloupce.</span><span class="sxs-lookup"><span data-stu-id="cb803-188">To create statistics on a column, simply provide a name for the statistics object and the name of the column.</span></span>

<span data-ttu-id="cb803-189">Tuto syntaxi používá všechny výchozí možnosti.</span><span class="sxs-lookup"><span data-stu-id="cb803-189">This syntax uses all of the default options.</span></span> <span data-ttu-id="cb803-190">Ve výchozím nastavení ukázky SQL Data Warehouse 20 procent tabulky při vytváření statistik.</span><span class="sxs-lookup"><span data-stu-id="cb803-190">By default, SQL Data Warehouse samples 20 percent of the table when it creates statistics.</span></span>

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]);
```

<span data-ttu-id="cb803-191">Například:</span><span class="sxs-lookup"><span data-stu-id="cb803-191">For example:</span></span>

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1);
```

### <a name="b-create-single-column-statistics-by-examining-every-row"></a><span data-ttu-id="cb803-192">B.</span><span class="sxs-lookup"><span data-stu-id="cb803-192">B.</span></span> <span data-ttu-id="cb803-193">Vytvořit jednosloupcovou statistiku tak, že prověří každý řádek</span><span class="sxs-lookup"><span data-stu-id="cb803-193">Create single-column statistics by examining every row</span></span>
<span data-ttu-id="cb803-194">Pro většině případů stačí výchozí vzorkovací frekvenci 20 procent.</span><span class="sxs-lookup"><span data-stu-id="cb803-194">The default sampling rate of 20 percent is sufficient for most situations.</span></span> <span data-ttu-id="cb803-195">Můžete však upravit vzorkovací frekvenci.</span><span class="sxs-lookup"><span data-stu-id="cb803-195">However, you can adjust the sampling rate.</span></span>

<span data-ttu-id="cb803-196">Chcete-li ukázkové úplné tabulky, použijte následující syntaxi:</span><span class="sxs-lookup"><span data-stu-id="cb803-196">To sample the full table, use this syntax:</span></span>

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]) WITH FULLSCAN;
```

<span data-ttu-id="cb803-197">Například:</span><span class="sxs-lookup"><span data-stu-id="cb803-197">For example:</span></span>

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH FULLSCAN;
```

### <a name="c-create-single-column-statistics-by-specifying-the-sample-size"></a><span data-ttu-id="cb803-198">C.</span><span class="sxs-lookup"><span data-stu-id="cb803-198">C.</span></span> <span data-ttu-id="cb803-199">Vytvořte jednosloupcovou statistiku zadáním velikost vzorku</span><span class="sxs-lookup"><span data-stu-id="cb803-199">Create single-column statistics by specifying the sample size</span></span>
<span data-ttu-id="cb803-200">Alternativně můžete určit velikost vzorku v procentech:</span><span class="sxs-lookup"><span data-stu-id="cb803-200">Alternatively, you can specify the sample size as a percent:</span></span>

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH SAMPLE = 50 PERCENT;
```

### <a name="d-create-single-column-statistics-on-only-some-of-the-rows"></a><span data-ttu-id="cb803-201">D.</span><span class="sxs-lookup"><span data-stu-id="cb803-201">D.</span></span> <span data-ttu-id="cb803-202">Vytvořit jednosloupcovou statistiku pro jenom některé řádky</span><span class="sxs-lookup"><span data-stu-id="cb803-202">Create single-column statistics on only some of the rows</span></span>
<span data-ttu-id="cb803-203">Další možností statistiky můžete vytvořit na část řádky v tabulce.</span><span class="sxs-lookup"><span data-stu-id="cb803-203">Another option, you can create statistics on a portion of the rows in your table.</span></span> <span data-ttu-id="cb803-204">Tomu se říká filtrované statistiky.</span><span class="sxs-lookup"><span data-stu-id="cb803-204">This is called a filtered statistic.</span></span>

<span data-ttu-id="cb803-205">Můžete například použít filtrovanou statistiku při plánování k dotazování na konkrétní oddíl velkých oddílů tabulky.</span><span class="sxs-lookup"><span data-stu-id="cb803-205">For example, you could use filtered statistics when you plan to query a specific partition of a large partitioned table.</span></span> <span data-ttu-id="cb803-206">Vytvořením statistiky na pouze hodnoty oddílu se přesnost statistik pro zlepšení a proto zlepšit výkon dotazu.</span><span class="sxs-lookup"><span data-stu-id="cb803-206">By creating statistics on only the partition values, the accuracy of the statistics will improve, and therefore improve query performance.</span></span>

<span data-ttu-id="cb803-207">Tento příklad vytvoří statistiky na rozsah hodnot.</span><span class="sxs-lookup"><span data-stu-id="cb803-207">This example creates statistics on a range of values.</span></span> <span data-ttu-id="cb803-208">Hodnoty můžete snadno nadefinovat tak, aby odpovídaly rozsahu hodnot v oddílu.</span><span class="sxs-lookup"><span data-stu-id="cb803-208">The values could easily be defined to match the range of values in a partition.</span></span>

```sql
CREATE STATISTICS stats_col1 ON table1(col1) WHERE col1 > '2000101' AND col1 < '20001231';
```

> [!NOTE]
> <span data-ttu-id="cb803-209">Pro Optimalizátor dotazů, zvažte možnost použití filtrovanou statistiku, když se vybere plánu distribuovaných dotazů dotaz musí vejít do definice objektu statistiky.</span><span class="sxs-lookup"><span data-stu-id="cb803-209">For the query optimizer to consider using filtered statistics when it chooses the distributed query plan, the query must fit inside the definition of the statistics object.</span></span> <span data-ttu-id="cb803-210">Použijeme předchozí příklad dotazu kde klauzule musí určovat Sloupec1 hodnoty mezi 2000101 a 20001231.</span><span class="sxs-lookup"><span data-stu-id="cb803-210">Using the previous example, the query's where clause needs to specify col1 values between 2000101 and 20001231.</span></span>
> 
> 

### <a name="e-create-single-column-statistics-with-all-the-options"></a><span data-ttu-id="cb803-211">E.</span><span class="sxs-lookup"><span data-stu-id="cb803-211">E.</span></span> <span data-ttu-id="cb803-212">Vytvořit jednosloupcovou statistiku se všemi možnostmi</span><span class="sxs-lookup"><span data-stu-id="cb803-212">Create single-column statistics with all the options</span></span>
<span data-ttu-id="cb803-213">Možnosti můžete kombinovat samozřejmě společně.</span><span class="sxs-lookup"><span data-stu-id="cb803-213">You can, of course, combine the options together.</span></span> <span data-ttu-id="cb803-214">Následující příklad vytvoří objekt filtrovanou statistiku s velikost vlastní vzorku:</span><span class="sxs-lookup"><span data-stu-id="cb803-214">The example below creates a filtered statistics object with a custom sample size:</span></span>

```sql
CREATE STATISTICS stats_col1 ON table1 (col1) WHERE col1 > '2000101' AND col1 < '20001231' WITH SAMPLE = 50 PERCENT;
```

<span data-ttu-id="cb803-215">Úplný přehled najdete v tématu [CREATE STATISTICS] [ CREATE STATISTICS] na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="cb803-215">For the full reference, see [CREATE STATISTICS][CREATE STATISTICS] on MSDN.</span></span>

### <a name="f-create-multi-column-statistics"></a><span data-ttu-id="cb803-216">F.</span><span class="sxs-lookup"><span data-stu-id="cb803-216">F.</span></span> <span data-ttu-id="cb803-217">Vytvoření statistiky více sloupci</span><span class="sxs-lookup"><span data-stu-id="cb803-217">Create multi-column statistics</span></span>
<span data-ttu-id="cb803-218">Vytvoření statistiky vícesloupcového, jednoduše použijte v předchozích příkladech, ale zadat více sloupců.</span><span class="sxs-lookup"><span data-stu-id="cb803-218">To create a multi-column statistics, simply use the previous examples, but specify more columns.</span></span>

> [!NOTE]
> <span data-ttu-id="cb803-219">Histogram, který slouží k zjištění přibližné hodnoty počet řádků ve výsledku dotazu, je dostupná jenom pro první sloupec uvedené v definici objektu statistiky.</span><span class="sxs-lookup"><span data-stu-id="cb803-219">The histogram, which is used to estimate number of rows in the query result, is only available for the first column listed in the statistics object definition.</span></span>
> 
> 

<span data-ttu-id="cb803-220">V tomto příkladu je histogramu na *produktu\_kategorie*.</span><span class="sxs-lookup"><span data-stu-id="cb803-220">In this example, the histogram is on *product\_category*.</span></span> <span data-ttu-id="cb803-221">Statistiky mezi sloupce jsou vypočítány na *produktu\_kategorie* a *produktu\_sub_c\ategory*:</span><span class="sxs-lookup"><span data-stu-id="cb803-221">Cross-column statistics are calculated on *product\_category* and *product\_sub_c\ategory*:</span></span>

```sql
CREATE STATISTICS stats_2cols ON table1 (product_category, product_sub_category) WHERE product_category > '2000101' AND product_category < '20001231' WITH SAMPLE = 50 PERCENT;
```

<span data-ttu-id="cb803-222">Vzhledem k tomu, že existuje korelace mezi *produktu\_kategorie* a *produktu\_sub\_kategorie*, může být užitečné, pokud tyto sloupce, ke kterým se přistupuje vícesloupcového stat ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="cb803-222">Since there is a correlation between *product\_category* and *product\_sub\_category*, a multi-column stat can be useful if these columns are accessed at the same time.</span></span>

### <a name="g-create-statistics-on-all-the-columns-in-a-table"></a><span data-ttu-id="cb803-223">G.</span><span class="sxs-lookup"><span data-stu-id="cb803-223">G.</span></span> <span data-ttu-id="cb803-224">Vytvoření statistiky pro všechny sloupce v tabulce</span><span class="sxs-lookup"><span data-stu-id="cb803-224">Create statistics on all the columns in a table</span></span>
<span data-ttu-id="cb803-225">Jeden způsob, jak vytvořit statistiku problémy příkazy CREATE STATISTICS je po vytvoření tabulky.</span><span class="sxs-lookup"><span data-stu-id="cb803-225">One way to create statistics is to issues CREATE STATISTICS commands after creating the table.</span></span>

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

### <a name="h-use-a-stored-procedure-to-create-statistics-on-all-columns-in-a-database"></a><span data-ttu-id="cb803-226">H.</span><span class="sxs-lookup"><span data-stu-id="cb803-226">H.</span></span> <span data-ttu-id="cb803-227">Vytvoření statistiky pro všechny sloupce v databázi pomocí uložené procedury</span><span class="sxs-lookup"><span data-stu-id="cb803-227">Use a stored procedure to create statistics on all columns in a database</span></span>
<span data-ttu-id="cb803-228">SQL Data Warehouse nemá ekvivalentní [] – [sp_create_stats] systémové uložené procedury v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="cb803-228">SQL Data Warehouse does not have a system stored procedure equivalent to [sp_create_stats][] in SQL Server.</span></span> <span data-ttu-id="cb803-229">Tato uložená procedura vytvoří objekt statistiky jeden sloupec pro každý sloupec databáze, který ještě nemá statistiky.</span><span class="sxs-lookup"><span data-stu-id="cb803-229">This stored procedure creates a single column statistics object on every column of the database that doesn't already have statistics.</span></span>

<span data-ttu-id="cb803-230">To vám pomůže začít pracovat s návrhu databáze.</span><span class="sxs-lookup"><span data-stu-id="cb803-230">This will help you get started with your database design.</span></span> <span data-ttu-id="cb803-231">Nebojte se, že jej přizpůsobit svým potřebám.</span><span class="sxs-lookup"><span data-stu-id="cb803-231">Feel free to adapt it to your needs.</span></span>

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

<span data-ttu-id="cb803-232">Vytvoření statistiky pro všechny sloupce v tabulce s tímto postupem, jednoduše voláním procedury.</span><span class="sxs-lookup"><span data-stu-id="cb803-232">To create statistics on all columns in the table with this procedure, simply call the procedure.</span></span>

```sql
prc_sqldw_create_stats;
```

## <a name="examples-update-statistics"></a><span data-ttu-id="cb803-233">Příklady: aktualizovat statistiky</span><span class="sxs-lookup"><span data-stu-id="cb803-233">Examples: update statistics</span></span>
<span data-ttu-id="cb803-234">Chcete-li aktualizovat statistiku, můžete:</span><span class="sxs-lookup"><span data-stu-id="cb803-234">To update statistics, you can:</span></span>

1. <span data-ttu-id="cb803-235">Aktualizujte jeden objekt statistiky.</span><span class="sxs-lookup"><span data-stu-id="cb803-235">Update one statistics object.</span></span> <span data-ttu-id="cb803-236">Zadejte název objektu statistiku, který se má aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="cb803-236">Specify the name of the statistics object you wish to update.</span></span>
2. <span data-ttu-id="cb803-237">Aktualizujte všechny statistiky objekty v tabulce.</span><span class="sxs-lookup"><span data-stu-id="cb803-237">Update all statistics objects on a table.</span></span> <span data-ttu-id="cb803-238">Zadejte název tabulky místo jeden objekt konkrétní statistiku.</span><span class="sxs-lookup"><span data-stu-id="cb803-238">Specify the name of the table instead of one specific statistics object.</span></span>

### <a name="a-update-one-specific-statistics-object"></a><span data-ttu-id="cb803-239">A.</span><span class="sxs-lookup"><span data-stu-id="cb803-239">A.</span></span> <span data-ttu-id="cb803-240">Aktualizovat jeden objekt konkrétní Statistika</span><span class="sxs-lookup"><span data-stu-id="cb803-240">Update one specific statistics object</span></span>
<span data-ttu-id="cb803-241">Aktualizovat objekt konkrétní statistiku použijte následující syntaxi:</span><span class="sxs-lookup"><span data-stu-id="cb803-241">Use the following syntax to update a specific statistics object:</span></span>

```sql
UPDATE STATISTICS [schema_name].[table_name]([stat_name]);
```

<span data-ttu-id="cb803-242">Například:</span><span class="sxs-lookup"><span data-stu-id="cb803-242">For example:</span></span>

```sql
UPDATE STATISTICS [dbo].[table1] ([stats_col1]);
```

<span data-ttu-id="cb803-243">Při aktualizaci statistiky konkrétní objekty, můžete snížit čas a prostředky, které jsou nezbytné ke správě statistiky.</span><span class="sxs-lookup"><span data-stu-id="cb803-243">By updating specific statistics objects, you can minimize the time and resources required to manage statistics.</span></span> <span data-ttu-id="cb803-244">To vyžaduje některé myšlenku, ale vybrat nejlepší statistiky objekty, které chcete aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="cb803-244">This requires some thought, though, to choose the best statistics objects to update.</span></span>

### <a name="b-update-all-statistics-on-a-table"></a><span data-ttu-id="cb803-245">B.</span><span class="sxs-lookup"><span data-stu-id="cb803-245">B.</span></span> <span data-ttu-id="cb803-246">Aktualizovat všechny statistiky v tabulce</span><span class="sxs-lookup"><span data-stu-id="cb803-246">Update all statistics on a table</span></span>
<span data-ttu-id="cb803-247">Ukazuje to jednoduše aktualizace všechny statistiky objektů v tabulce.</span><span class="sxs-lookup"><span data-stu-id="cb803-247">This shows a simple method for updating all the statistics objects on a table.</span></span>

```sql
UPDATE STATISTICS [schema_name].[table_name];
```

<span data-ttu-id="cb803-248">Například:</span><span class="sxs-lookup"><span data-stu-id="cb803-248">For example:</span></span>

```sql
UPDATE STATISTICS dbo.table1;
```

<span data-ttu-id="cb803-249">Tento příkaz je snadno použitelný.</span><span class="sxs-lookup"><span data-stu-id="cb803-249">This statement is easy to use.</span></span> <span data-ttu-id="cb803-250">Jenom nezapomeňte to aktualizuje všechny statistiky v tabulce a proto může provést další práci, než je nezbytné.</span><span class="sxs-lookup"><span data-stu-id="cb803-250">Just remember this updates all statistics on the table, and therefore might perform more work than is necessary.</span></span> <span data-ttu-id="cb803-251">Pokud výkon není problém, to je výborný nejúplnější a nejjednodušší způsob, jak zajistit, že statistiky jsou aktuální.</span><span class="sxs-lookup"><span data-stu-id="cb803-251">If the performance is not an issue, this is definitely the easiest and most complete way to guarantee statistics are up-to-date.</span></span>

> [!NOTE]
> <span data-ttu-id="cb803-252">Při aktualizaci všechny statistiky v tabulce se SQL Data Warehouse nepodporuje vyhledávání s cílem ukázková tabulka pro každou statistiku.</span><span class="sxs-lookup"><span data-stu-id="cb803-252">When updating all statistics on a table, SQL Data Warehouse does a scan to sample the table for each statistics.</span></span> <span data-ttu-id="cb803-253">Pokud je tabulka velký, má mnoho sloupců a mnoho statistiky, může to být efektivnější jednotlivých statistiku podle potřeb.</span><span class="sxs-lookup"><span data-stu-id="cb803-253">If the table is large, has many columns, and many statistics, it might be more efficient to update individual statistics based on need.</span></span>
> 
> 

<span data-ttu-id="cb803-254">Implementace `UPDATE STATISTICS` postupu najdete v tématu [dočasných tabulek] [ Temporary] článku.</span><span class="sxs-lookup"><span data-stu-id="cb803-254">For an implementation of an `UPDATE STATISTICS` procedure please see the [Temporary Tables][Temporary] article.</span></span> <span data-ttu-id="cb803-255">Implementace metody se mírně liší na `CREATE STATISTICS` výše uvedený postup, ale konečný výsledek je stejný.</span><span class="sxs-lookup"><span data-stu-id="cb803-255">The implementation method is slightly different to the `CREATE STATISTICS` procedure above but the end result is the same.</span></span>

<span data-ttu-id="cb803-256">Úplná syntaxe, najdete v části [Update Statistics] [ Update Statistics] na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="cb803-256">For the full syntax, see [Update Statistics][Update Statistics] on MSDN.</span></span>

## <a name="statistics-metadata"></a><span data-ttu-id="cb803-257">Statistiky metadat</span><span class="sxs-lookup"><span data-stu-id="cb803-257">Statistics metadata</span></span>
<span data-ttu-id="cb803-258">Existuje několik systémové zobrazení a funkce, které můžete použít k nalezení informací o statistikách.</span><span class="sxs-lookup"><span data-stu-id="cb803-258">There are several system view and functions that you can use to find information about statistics.</span></span> <span data-ttu-id="cb803-259">Například se zobrazí, pokud objekt statistiky může být zastaralé pomocí funkce statistiky date a zjistěte, kdy byly statistiky poslední vytvořil nebo aktualizoval.</span><span class="sxs-lookup"><span data-stu-id="cb803-259">For example, you can see if a statistics object might be out-of-date by using the stats-date function to see when statistics were last created or updated.</span></span>

### <a name="catalog-views-for-statistics"></a><span data-ttu-id="cb803-260">Zobrazení katalogu pro statistiky</span><span class="sxs-lookup"><span data-stu-id="cb803-260">Catalog views for statistics</span></span>
<span data-ttu-id="cb803-261">Tato systémová zobrazení obsahují informace o statistiky:</span><span class="sxs-lookup"><span data-stu-id="cb803-261">These system views provide information about statistics:</span></span>

| <span data-ttu-id="cb803-262">Zobrazení katalogu</span><span class="sxs-lookup"><span data-stu-id="cb803-262">Catalog View</span></span> | <span data-ttu-id="cb803-263">Popis</span><span class="sxs-lookup"><span data-stu-id="cb803-263">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="cb803-264">[Sys.Columns][sys.columns]</span><span class="sxs-lookup"><span data-stu-id="cb803-264">[sys.columns][sys.columns]</span></span> |<span data-ttu-id="cb803-265">Jeden řádek pro každý sloupec.</span><span class="sxs-lookup"><span data-stu-id="cb803-265">One row for each column.</span></span> |
| <span data-ttu-id="cb803-266">[Sys.Objects][sys.objects]</span><span class="sxs-lookup"><span data-stu-id="cb803-266">[sys.objects][sys.objects]</span></span> |<span data-ttu-id="cb803-267">Jeden řádek pro každý objekt v databázi.</span><span class="sxs-lookup"><span data-stu-id="cb803-267">One row for each object in the database.</span></span> |
| <span data-ttu-id="cb803-268">[Sys.Schemas][sys.schemas]</span><span class="sxs-lookup"><span data-stu-id="cb803-268">[sys.schemas][sys.schemas]</span></span> |<span data-ttu-id="cb803-269">Jeden řádek pro každý schématu v databázi.</span><span class="sxs-lookup"><span data-stu-id="cb803-269">One row for each schema in the database.</span></span> |
| <span data-ttu-id="cb803-270">[Sys.stats][sys.stats]</span><span class="sxs-lookup"><span data-stu-id="cb803-270">[sys.stats][sys.stats]</span></span> |<span data-ttu-id="cb803-271">Jeden řádek pro každý objekt statistiky.</span><span class="sxs-lookup"><span data-stu-id="cb803-271">One row for each statistics object.</span></span> |
| <span data-ttu-id="cb803-272">[Sys.stats_columns][sys.stats_columns]</span><span class="sxs-lookup"><span data-stu-id="cb803-272">[sys.stats_columns][sys.stats_columns]</span></span> |<span data-ttu-id="cb803-273">Jeden řádek pro každý sloupec v objektu statistiky.</span><span class="sxs-lookup"><span data-stu-id="cb803-273">One row for each column in the statistics object.</span></span> <span data-ttu-id="cb803-274">Odkazy Zpět na sys.columns.</span><span class="sxs-lookup"><span data-stu-id="cb803-274">Links back to sys.columns.</span></span> |
| <span data-ttu-id="cb803-275">[zobrazení Sys.Tables][sys.tables]</span><span class="sxs-lookup"><span data-stu-id="cb803-275">[sys.tables][sys.tables]</span></span> |<span data-ttu-id="cb803-276">Jeden řádek pro každou tabulku (zahrnuje externí tabulky).</span><span class="sxs-lookup"><span data-stu-id="cb803-276">One row for each table (includes external tables).</span></span> |
| <span data-ttu-id="cb803-277">[Sys.table_types][sys.table_types]</span><span class="sxs-lookup"><span data-stu-id="cb803-277">[sys.table_types][sys.table_types]</span></span> |<span data-ttu-id="cb803-278">Jeden řádek pro každý typ dat.</span><span class="sxs-lookup"><span data-stu-id="cb803-278">One row for each data type.</span></span> |

### <a name="system-functions-for-statistics"></a><span data-ttu-id="cb803-279">Funkce systému pro statistiky</span><span class="sxs-lookup"><span data-stu-id="cb803-279">System functions for statistics</span></span>
<span data-ttu-id="cb803-280">Tyto funkce systému jsou užitečné pro práci s statistiky:</span><span class="sxs-lookup"><span data-stu-id="cb803-280">These system functions are useful for working with statistics:</span></span>

| <span data-ttu-id="cb803-281">System – funkce</span><span class="sxs-lookup"><span data-stu-id="cb803-281">System Function</span></span> | <span data-ttu-id="cb803-282">Popis</span><span class="sxs-lookup"><span data-stu-id="cb803-282">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="cb803-283">[STATS_DATE][STATS_DATE]</span><span class="sxs-lookup"><span data-stu-id="cb803-283">[STATS_DATE][STATS_DATE]</span></span> |<span data-ttu-id="cb803-284">Datum poslední aktualizace objekt statistiky.</span><span class="sxs-lookup"><span data-stu-id="cb803-284">Date the statistics object was last updated.</span></span> |
| <span data-ttu-id="cb803-285">[PŘÍKAZ DBCC SHOW_STATISTICS][DBCC SHOW_STATISTICS]</span><span class="sxs-lookup"><span data-stu-id="cb803-285">[DBCC SHOW_STATISTICS][DBCC SHOW_STATISTICS]</span></span> |<span data-ttu-id="cb803-286">Poskytuje souhrnné úrovně a podrobné informace o distribuci hodnoty rozumí objekt statistiky.</span><span class="sxs-lookup"><span data-stu-id="cb803-286">Provides summary level and detailed information about the distribution of values as understood by the statistics object.</span></span> |

### <a name="combine-statistics-columns-and-functions-into-one-view"></a><span data-ttu-id="cb803-287">Zkombinovat do jednoho zobrazení statistiky sloupce a funkce</span><span class="sxs-lookup"><span data-stu-id="cb803-287">Combine statistics columns and functions into one view</span></span>
<span data-ttu-id="cb803-288">Toto zobrazení přináší sloupce, které se týkají statistiky a výsledkem [] – funkce [STATS_DATE()] společně.</span><span class="sxs-lookup"><span data-stu-id="cb803-288">This view brings columns that relate to statistics, and results from the [STATS_DATE()][]function together.</span></span>

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

## <a name="dbcc-showstatistics-examples"></a><span data-ttu-id="cb803-289">Příkaz DBCC SHOW_STATISTICS() příklady</span><span class="sxs-lookup"><span data-stu-id="cb803-289">DBCC SHOW_STATISTICS() examples</span></span>
<span data-ttu-id="cb803-290">Příkaz DBCC SHOW_STATISTICS() zobrazuje data ukládaná v rámci objektu statistiky.</span><span class="sxs-lookup"><span data-stu-id="cb803-290">DBCC SHOW_STATISTICS() shows the data held within a statistics object.</span></span> <span data-ttu-id="cb803-291">Tato data je rozdělena na tři části.</span><span class="sxs-lookup"><span data-stu-id="cb803-291">This data comes in three parts.</span></span>

1. <span data-ttu-id="cb803-292">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="cb803-292">Header</span></span>
2. <span data-ttu-id="cb803-293">Hustotu vektoru</span><span class="sxs-lookup"><span data-stu-id="cb803-293">Density Vector</span></span>
3. <span data-ttu-id="cb803-294">Histogram</span><span class="sxs-lookup"><span data-stu-id="cb803-294">Histogram</span></span>

<span data-ttu-id="cb803-295">Hlavička metadata o statistikách.</span><span class="sxs-lookup"><span data-stu-id="cb803-295">The header metadata about the statistics.</span></span> <span data-ttu-id="cb803-296">Histogramu zobrazí rozdělení hodnot ve sloupci první klíče objektu statistiky.</span><span class="sxs-lookup"><span data-stu-id="cb803-296">The histogram displays the distribution of values in the first key column of the statistics object.</span></span> <span data-ttu-id="cb803-297">Vektor hustotu měří korelace mezi sloupci.</span><span class="sxs-lookup"><span data-stu-id="cb803-297">The density vector measures cross-column correlation.</span></span> <span data-ttu-id="cb803-298">SQLDW vypočítá mohutnost odhady s žádným z dat v objektu statistiky.</span><span class="sxs-lookup"><span data-stu-id="cb803-298">SQLDW computes cardinality estimates with any of the data in the statistics object.</span></span>

### <a name="show-header-density-and-histogram"></a><span data-ttu-id="cb803-299">Zobrazit záhlaví, hustotu a histogram</span><span class="sxs-lookup"><span data-stu-id="cb803-299">Show header, density, and histogram</span></span>
<span data-ttu-id="cb803-300">Tento jednoduchý příklad ukazuje všechny tři částí objektu statistiky.</span><span class="sxs-lookup"><span data-stu-id="cb803-300">This simple example shows all three parts of a statistics object.</span></span>

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>)
```

<span data-ttu-id="cb803-301">Například:</span><span class="sxs-lookup"><span data-stu-id="cb803-301">For example:</span></span>

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1);
```

### <a name="show-one-or-more-parts-of-dbcc-showstatistics"></a><span data-ttu-id="cb803-302">Zobrazit jednu nebo více částí DBCC SHOW_STATISTICS();</span><span class="sxs-lookup"><span data-stu-id="cb803-302">Show one or more parts of DBCC SHOW_STATISTICS();</span></span>
<span data-ttu-id="cb803-303">Pokud vás zajímá pouze v zobrazení konkrétní části, použijte `WITH` klauzule a zadejte části, které chcete zobrazit:</span><span class="sxs-lookup"><span data-stu-id="cb803-303">If you are only interested in viewing specific parts, use the `WITH` clause and specify which parts you want to see:</span></span>

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>) WITH stat_header, histogram, density_vector
```

<span data-ttu-id="cb803-304">Například:</span><span class="sxs-lookup"><span data-stu-id="cb803-304">For example:</span></span>

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1) WITH histogram, density_vector
```

## <a name="dbcc-showstatistics-differences"></a><span data-ttu-id="cb803-305">Příkaz DBCC SHOW_STATISTICS() rozdíly</span><span class="sxs-lookup"><span data-stu-id="cb803-305">DBCC SHOW_STATISTICS() differences</span></span>
<span data-ttu-id="cb803-306">Příkaz DBCC SHOW_STATISTICS() je implementováno více výhradně v SQL Data Warehouse ve srovnání s systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="cb803-306">DBCC SHOW_STATISTICS() is more strictly implemented in SQL Data Warehouse compared to SQL Server.</span></span>

1. <span data-ttu-id="cb803-307">Nezdokumentovaný funkce nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="cb803-307">Undocumented features are not supported</span></span>
2. <span data-ttu-id="cb803-308">Nelze použít Stats_stream</span><span class="sxs-lookup"><span data-stu-id="cb803-308">Cannot use Stats_stream</span></span>
3. <span data-ttu-id="cb803-309">Se nemůže připojit k výsledky pro konkrétní podmnožiny dat statistiky např (STAT_HEADER spojení DENSITY_VECTOR)</span><span class="sxs-lookup"><span data-stu-id="cb803-309">Cannot join results for specific subsets of statistics data e.g. (STAT_HEADER JOIN DENSITY_VECTOR)</span></span>
4. <span data-ttu-id="cb803-310">NO_INFOMSGS nelze nastavit pro potlačení zprávy</span><span class="sxs-lookup"><span data-stu-id="cb803-310">NO_INFOMSGS cannot be set for message suppression</span></span>
5. <span data-ttu-id="cb803-311">Hranaté závorky kolem názvů statistiky nelze použít.</span><span class="sxs-lookup"><span data-stu-id="cb803-311">Square brackets around statistics names cannot be used</span></span>
6. <span data-ttu-id="cb803-312">Názvy sloupců nelze použít k identifikaci objektů statistiky</span><span class="sxs-lookup"><span data-stu-id="cb803-312">Cannot use column names to identify statistics objects</span></span>
7. <span data-ttu-id="cb803-313">Vlastní chyba 2767 není podporovaná.</span><span class="sxs-lookup"><span data-stu-id="cb803-313">Custom error 2767 is not supported</span></span>

## <a name="next-steps"></a><span data-ttu-id="cb803-314">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cb803-314">Next steps</span></span>
<span data-ttu-id="cb803-315">Další podrobnosti najdete v tématu [DBCC SHOW_STATISTICS] [ DBCC SHOW_STATISTICS] na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="cb803-315">For more details, see [DBCC SHOW_STATISTICS][DBCC SHOW_STATISTICS] on MSDN.</span></span>  <span data-ttu-id="cb803-316">Další informace najdete v článcích na [tabulky přehled][Overview], [tabulky datové typy][Data Types], [distribuci tabulku] [ Distribute], [Indexování tabulku][Index], [vytváření oddílů tabulky] [ Partition] a [Dočasných tabulek][Temporary].</span><span class="sxs-lookup"><span data-stu-id="cb803-316">To learn more, see the articles on [Table Overview][Overview], [Table Data Types][Data Types], [Distributing a Table][Distribute], [Indexing a Table][Index],  [Partitioning a Table][Partition] and [Temporary Tables][Temporary].</span></span>  <span data-ttu-id="cb803-317">Další informace o osvědčených postupech najdete v tématu [SQL Data Warehouse osvědčené postupy][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="cb803-317">For more about best practices, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>  

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
