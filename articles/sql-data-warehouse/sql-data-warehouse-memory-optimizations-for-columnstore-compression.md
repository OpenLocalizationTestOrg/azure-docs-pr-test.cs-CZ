---
title: "Zlepšit výkon index columnstore v Azure SQL | Microsoft Docs"
description: "Snižte požadavky na paměť nebo zvýšení dostupné paměti maximalizovat počet řádků, které columnstore index zkomprimuje do každé skupiny řádků."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: ef170f39-ae24-4b04-af76-53bb4c4d16d3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 6/2/2017
ms.author: shigu;barbkess
ms.openlocfilehash: f0e0b839b4a0c216eee2eb5134d43b91d8f83289
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="maximizing-rowgroup-quality-for-columnstore"></a><span data-ttu-id="8ee16-103">Tím se maximalizuje quality rowgroup pro columnstore</span><span class="sxs-lookup"><span data-stu-id="8ee16-103">Maximizing rowgroup quality for columnstore</span></span>

<span data-ttu-id="8ee16-104">Kvalita rowgroup je určen podle počtu řádků v skupiny řádků.</span><span class="sxs-lookup"><span data-stu-id="8ee16-104">Rowgroup quality is determined by the number of rows in a rowgroup.</span></span> <span data-ttu-id="8ee16-105">Snižte požadavky na paměť nebo zvýšení dostupné paměti maximalizovat počet řádků, které columnstore index zkomprimuje do každé skupiny řádků.</span><span class="sxs-lookup"><span data-stu-id="8ee16-105">Reduce memory requirements or increase the available memory to maximize the number of rows a columnstore index compresses into each rowgroup.</span></span>  <span data-ttu-id="8ee16-106">Tyto metody používejte ke zlepšení kompresi a dotazování výkonu pro indexy columnstore.</span><span class="sxs-lookup"><span data-stu-id="8ee16-106">Use these methods to improve compression rates and query performance for columnstore indexes.</span></span>

## <a name="why-the-rowgroup-size-matters"></a><span data-ttu-id="8ee16-107">Proč na záleží velikost skupiny řádků</span><span class="sxs-lookup"><span data-stu-id="8ee16-107">Why the rowgroup size matters</span></span>
<span data-ttu-id="8ee16-108">Protože columnstore index tabulku kontroluje naskenováním segmenty sloupce s jednotlivých rowgroups, tím se maximalizuje počet řádků v každém rowgroup vylepšuje výkon dotazů.</span><span class="sxs-lookup"><span data-stu-id="8ee16-108">Since a columnstore index scans a table by scanning column segments of individual rowgroups, maximizing the number of rows in each rowgroup enhances query performance.</span></span> <span data-ttu-id="8ee16-109">Pokud rowgroups velký počet řádků, zlepšuje komprese dat, což znamená, že je méně dat ke čtení z disku.</span><span class="sxs-lookup"><span data-stu-id="8ee16-109">When rowgroups have a high number of rows, data compression improves which means there is less data to read from disk.</span></span>

<span data-ttu-id="8ee16-110">Další informace o rowgroups najdete v tématu [Průvodce indexy Columnstore](https://msdn.microsoft.com/library/gg492088.aspx).</span><span class="sxs-lookup"><span data-stu-id="8ee16-110">For more information about rowgroups, see [Columnstore Indexes Guide](https://msdn.microsoft.com/library/gg492088.aspx).</span></span>

## <a name="target-size-for-rowgroups"></a><span data-ttu-id="8ee16-111">Cílovou velikost rowgroups</span><span class="sxs-lookup"><span data-stu-id="8ee16-111">Target size for rowgroups</span></span>
<span data-ttu-id="8ee16-112">Pro nejlepší výkon dotazů cílem je maximalizovat počet řádků na jeden rowgroup v indexu columnstore.</span><span class="sxs-lookup"><span data-stu-id="8ee16-112">For best query performance, the goal is to maximize the number of rows per rowgroup in a columnstore index.</span></span> <span data-ttu-id="8ee16-113">Rowgroup může mít maximálně 1 048 576 řádků.</span><span class="sxs-lookup"><span data-stu-id="8ee16-113">A rowgroup can have a maximum of 1,048,576 rows.</span></span> <span data-ttu-id="8ee16-114">Je to v pořádku nemá maximální počet řádků na jeden skupiny řádků.</span><span class="sxs-lookup"><span data-stu-id="8ee16-114">It's okay to not have the maximum number of rows per rowgroup.</span></span> <span data-ttu-id="8ee16-115">Indexy Columnstore dosáhli dobrého výkonu, pokud rowgroups nejméně 100 000 řádků.</span><span class="sxs-lookup"><span data-stu-id="8ee16-115">Columnstore indexes achieve good performance when rowgroups have at least 100,000 rows.</span></span>

## <a name="rowgroups-can-get-trimmed-during-compression"></a><span data-ttu-id="8ee16-116">Během komprese můžete získat oříznut Rowgroups</span><span class="sxs-lookup"><span data-stu-id="8ee16-116">Rowgroups can get trimmed during compression</span></span>

<span data-ttu-id="8ee16-117">Během hromadné načtení nebo columnstore index opětovném sestavení někdy není k dispozici dostatek paměti pro kompresi všechny řádky, které jsou určené pro každé skupiny řádků.</span><span class="sxs-lookup"><span data-stu-id="8ee16-117">During a bulk load or columnstore index rebuild, sometimes there isn't enough memory available to compress all the rows designated for each rowgroup.</span></span> <span data-ttu-id="8ee16-118">Při přetížení paměti indexy columnstore trim rowgroup velikosti, komprese do columnstore mohlo být úspěšné.</span><span class="sxs-lookup"><span data-stu-id="8ee16-118">When there is memory pressure, columnstore indexes trim the rowgroup sizes so compression into the columnstore can succeed.</span></span> 

<span data-ttu-id="8ee16-119">Pokud nastal nedostatek paměti pro komprimovat alespoň 10 000 řádků do každé skupiny řádků, SQL Data Warehouse, vygeneruje se chyba.</span><span class="sxs-lookup"><span data-stu-id="8ee16-119">When there is insufficient memory to compress at least 10,000 rows into each rowgroup, SQL Data Warehouse generates an error.</span></span>

<span data-ttu-id="8ee16-120">Další informace o hromadné načítání, najdete v části [hromadné načtení do clusterovaný index columnstore](https://msdn.microsoft.com/en-us/library/dn935008.aspx#Bulk load into a clustered columnstore index).</span><span class="sxs-lookup"><span data-stu-id="8ee16-120">For more information on bulk loading, see [Bulk load into a clustered columnstore index](https://msdn.microsoft.com/en-us/library/dn935008.aspx#Bulk load into a clustered columnstore index).</span></span>

## <a name="how-to-monitor-rowgroup-quality"></a><span data-ttu-id="8ee16-121">Postup sledování kvality skupiny řádků</span><span class="sxs-lookup"><span data-stu-id="8ee16-121">How to monitor rowgroup quality</span></span>

<span data-ttu-id="8ee16-122">Není DMV (sys.dm_pdw_nodes_db_column_store_row_group_physical_stats), který zveřejňuje užitečné informace, jako je počet řádků v rowgroups a důvody, proč oříznutí, pokud byl ořezávání.</span><span class="sxs-lookup"><span data-stu-id="8ee16-122">There is a DMV (sys.dm_pdw_nodes_db_column_store_row_group_physical_stats) that exposes useful information such as number of rows in rowgroups and the reason for trimming if there was trimming.</span></span> <span data-ttu-id="8ee16-123">Následující zobrazení můžete vytvořit jako užitečný způsob, jak dotaz na tento DMV informace o oříznutí skupiny řádků.</span><span class="sxs-lookup"><span data-stu-id="8ee16-123">You can create the following view as a handy way to query this DMV to get information on rowgroup trimming.</span></span>

```sql
create view dbo.vCS_rg_physical_stats
as 
with cte
as
(
select   tb.[name]                    AS [logical_table_name]
,        rg.[row_group_id]            AS [row_group_id]
,        rg.[state]                   AS [state]
,        rg.[state_desc]              AS [state_desc]
,        rg.[total_rows]              AS [total_rows]
,        rg.[trim_reason_desc]        AS trim_reason_desc
,        mp.[physical_name]           AS physical_name
FROM    sys.[schemas] sm
JOIN    sys.[tables] tb               ON  sm.[schema_id]          = tb.[schema_id]                             
JOIN    sys.[pdw_table_mappings] mp   ON  tb.[object_id]          = mp.[object_id]
JOIN    sys.[pdw_nodes_tables] nt     ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[dm_pdw_nodes_db_column_store_row_group_physical_stats] rg      ON  rg.[object_id]     = nt.[object_id]
                                                                            AND rg.[pdw_node_id]   = nt.[pdw_node_id]
                                        AND rg.[distribution_id]    = nt.[distribution_id]                                          
)
select *
from cte;
```

<span data-ttu-id="8ee16-124">Trim_reason_desc oznamuje, zda byl oříznut rowgroup (trim_reason_desc = NO_TRIM znamená došlo bez oříznutí a skupiny řádků je optimální kvality).</span><span class="sxs-lookup"><span data-stu-id="8ee16-124">The trim_reason_desc tells whether the rowgroup was trimmed(trim_reason_desc = NO_TRIM implies there was no trimming and row group is of optimal quality).</span></span> <span data-ttu-id="8ee16-125">Oříznutí důvody znamenat předčasné oříznutí rowgroup:</span><span class="sxs-lookup"><span data-stu-id="8ee16-125">The following trim reasons indicate premature trimming of the rowgroup:</span></span>
- <span data-ttu-id="8ee16-126">BULKLOAD: Z tohoto důvodu uvolnění dočasné paměti se používá při příchozí dávku řádků pro zatížení měl menší než 1 milionu řádků.</span><span class="sxs-lookup"><span data-stu-id="8ee16-126">BULKLOAD: This trim reason is used when the incoming batch of rows for the load had less than 1 million rows.</span></span> <span data-ttu-id="8ee16-127">Modul vytvoří skupiny komprimované řádků, pokud jsou větší než 100 000 řádků vkládání (na rozdíl od vkládání do úložiště rozdílová), ale nastaví trim důvod BULKLOAD.</span><span class="sxs-lookup"><span data-stu-id="8ee16-127">The engine will create compressed row groups if there are greater than 100,000 rows being inserted (as opposed to inserting into the delta store) but sets the trim reason to BULKLOAD.</span></span> <span data-ttu-id="8ee16-128">V tomto scénáři zvažte zvýšení zatížení okně aplikace batch hromadit další řádky.</span><span class="sxs-lookup"><span data-stu-id="8ee16-128">In this scenario, consider increasing your batch load window to accumulate more rows.</span></span> <span data-ttu-id="8ee16-129">Navíc přehodnocovat vaše schéma rozdělení oddílů zajistit, že není příliš podrobné jako skupiny řádků nemůžou zahrnovat hranice oddílů.</span><span class="sxs-lookup"><span data-stu-id="8ee16-129">Also, reevaluate your partitioning scheme to ensure it is not too granular as row groups cannot span partition boundaries.</span></span>
- <span data-ttu-id="8ee16-130">MEMORY_LIMITATION: Pokud chcete vytvořit skupiny řádků s 1 milionu řádků, množství paměti pracovní vyžaduje modul.</span><span class="sxs-lookup"><span data-stu-id="8ee16-130">MEMORY_LIMITATION: To create row groups with 1 million rows, a certain amount of working memory is required by the engine.</span></span> <span data-ttu-id="8ee16-131">Při načítání relace dostupné paměti je menší než požadovanou paměť pracovní, získat předčasně oříznut skupiny řádků.</span><span class="sxs-lookup"><span data-stu-id="8ee16-131">When available memory of the loading session is less than the required working memory, row groups get prematurely trimmed.</span></span> <span data-ttu-id="8ee16-132">Následující části popisují, jak k zjištění přibližné hodnoty požadované paměti a přidělení více paměti.</span><span class="sxs-lookup"><span data-stu-id="8ee16-132">The following sections explain how to estimate memory required and allocate more memory.</span></span>
- <span data-ttu-id="8ee16-133">DICTIONARY_SIZE: Z tohoto důvodu trim označuje, že rowgroup oříznutí došlo k chybě kvůli alespoň jeden sloupec řetězec s širokým nebo vysokou kardinalitou řetězce.</span><span class="sxs-lookup"><span data-stu-id="8ee16-133">DICTIONARY_SIZE: This trim reason indicates that rowgroup trimming occurred because there was at least one string column with wide and/or high cardinality strings.</span></span> <span data-ttu-id="8ee16-134">Slovník velikost je omezena na 16 MB paměti a je komprimované po dosažení tohoto limitu skupiny řádků.</span><span class="sxs-lookup"><span data-stu-id="8ee16-134">The dictionary size is limited to 16 MB in memory and once this limit is reached the row group is compressed.</span></span> <span data-ttu-id="8ee16-135">Pokud k této situaci, zvažte možnost odizolování problematické sloupce do samostatné tabulky.</span><span class="sxs-lookup"><span data-stu-id="8ee16-135">If you do run into this situation, consider isolating the problematic column into a separate table.</span></span>

## <a name="how-to-estimate-memory-requirements"></a><span data-ttu-id="8ee16-136">Postup odhadnout požadavky na paměť</span><span class="sxs-lookup"><span data-stu-id="8ee16-136">How to estimate memory requirements</span></span>

<!--
To view an estimate of the memory requirements to compress a rowgroup of maximum size into a columnstore index, download and run the view [dbo.vCS_mon_mem_grant](). This view shows the size of the memory grant that a rowgroup requires for compression in to the columnstore.
-->

<span data-ttu-id="8ee16-137">Maximální velikost paměti požadované ke zkomprimování jeden rowgroup je přibližně</span><span class="sxs-lookup"><span data-stu-id="8ee16-137">The maximum required memory to compress one rowgroup is approximately</span></span>

- <span data-ttu-id="8ee16-138">72 MB +</span><span class="sxs-lookup"><span data-stu-id="8ee16-138">72 MB +</span></span>
- <span data-ttu-id="8ee16-139">\#řádky \* \#sloupce \* 8 bajtů +</span><span class="sxs-lookup"><span data-stu-id="8ee16-139">\#rows \* \#columns \* 8 bytes +</span></span>
- <span data-ttu-id="8ee16-140">\#řádky \* \#krátké. řetězcové sloupce \* 32 bajtů +</span><span class="sxs-lookup"><span data-stu-id="8ee16-140">\#rows \* \#short-string-columns \* 32 bytes +</span></span>
- <span data-ttu-id="8ee16-141">\#Long. řetězcové sloupce \* 16 MB pro slovník komprese</span><span class="sxs-lookup"><span data-stu-id="8ee16-141">\#long-string-columns \* 16 MB for compression dictionary</span></span>

<span data-ttu-id="8ee16-142">kde krátké. řetězcové sloupce použít datové typy řetězec < = 32 bajtů a datové typy řetězec použití dlouho. řetězcové sloupce > 32 bajtů.</span><span class="sxs-lookup"><span data-stu-id="8ee16-142">where short-string-columns use string data types of <= 32 bytes and long-string-columns use string data types of > 32 bytes.</span></span>

<span data-ttu-id="8ee16-143">Dlouhé řetězce jsou komprimována pomocí metody komprese určený pro kompresi text.</span><span class="sxs-lookup"><span data-stu-id="8ee16-143">Long strings are compressed with a compression method designed for compressing text.</span></span> <span data-ttu-id="8ee16-144">Tato metoda komprese používá *slovník* k uložení textové vzory.</span><span class="sxs-lookup"><span data-stu-id="8ee16-144">This compression method uses a *dictionary* to store text patterns.</span></span> <span data-ttu-id="8ee16-145">Maximální velikost slovník je 16 MB.</span><span class="sxs-lookup"><span data-stu-id="8ee16-145">The maximum size of a dictionary is 16 MB.</span></span> <span data-ttu-id="8ee16-146">Není k dispozici pouze jeden slovník pro každý sloupec dlouhý řetězec v skupiny řádků.</span><span class="sxs-lookup"><span data-stu-id="8ee16-146">There is only one dictionary for each long string column in the rowgroup.</span></span>

<span data-ttu-id="8ee16-147">Podrobné informace o požadavky na paměť columnstore, najdete v části video [škálování Azure SQL Data Warehouse: Konfigurace a pokyny](https://myignite.microsoft.com/videos/14822).</span><span class="sxs-lookup"><span data-stu-id="8ee16-147">For an in-depth discussion of columnstore memory requirements, see the video [Azure SQL Data Warehouse scaling: configuration and guidance](https://myignite.microsoft.com/videos/14822).</span></span>

## <a name="ways-to-reduce-memory-requirements"></a><span data-ttu-id="8ee16-148">Způsoby, jak snížit požadavky na paměť</span><span class="sxs-lookup"><span data-stu-id="8ee16-148">Ways to reduce memory requirements</span></span>

<span data-ttu-id="8ee16-149">Pomocí následujících postupů snížit požadavky na paměť pro kompresi rowgroups do indexy columnstore.</span><span class="sxs-lookup"><span data-stu-id="8ee16-149">Use the following techniques to reduce the memory requirements for compressing rowgroups into columnstore indexes.</span></span>

### <a name="use-fewer-columns"></a><span data-ttu-id="8ee16-150">Použití méně sloupců</span><span class="sxs-lookup"><span data-stu-id="8ee16-150">Use fewer columns</span></span>
<span data-ttu-id="8ee16-151">Pokud je to možné návrh tabulky s menším počtem sloupců.</span><span class="sxs-lookup"><span data-stu-id="8ee16-151">If possible, design the table with fewer columns.</span></span> <span data-ttu-id="8ee16-152">Když do columnstore se komprimují rowgroup, columnstore index komprimaci každý segment sloupce samostatně.</span><span class="sxs-lookup"><span data-stu-id="8ee16-152">When a rowgroup is compressed into the columnstore, the columnstore index compresses each column segment separately.</span></span> <span data-ttu-id="8ee16-153">Požadavky na paměť zkomprimovat rowgroup proto zvýšení počtu sloupců zvyšuje.</span><span class="sxs-lookup"><span data-stu-id="8ee16-153">Therefore the memory requirements to compress a rowgroup increase as the number of columns increases.</span></span>


### <a name="use-fewer-string-columns"></a><span data-ttu-id="8ee16-154">Použití méně sloupců řetězec</span><span class="sxs-lookup"><span data-stu-id="8ee16-154">Use fewer string columns</span></span>
<span data-ttu-id="8ee16-155">Sloupce řetězce vyžadují větší spotřebu paměti než číselné a datový typ datum.</span><span class="sxs-lookup"><span data-stu-id="8ee16-155">Columns of string data types require more memory than numeric and date data types.</span></span> <span data-ttu-id="8ee16-156">Chcete-li snížit požadavky na paměť, zvažte odebrání řetězcové sloupce z tabulky faktů a jejich uvedení v menší tabulky dimenzí.</span><span class="sxs-lookup"><span data-stu-id="8ee16-156">To reduce memory requirements, consider removing string columns from fact tables and putting them in smaller dimension tables.</span></span>

<span data-ttu-id="8ee16-157">Požadavky na další paměť pro kompresi řetězec:</span><span class="sxs-lookup"><span data-stu-id="8ee16-157">Additional memory requirements for string compression:</span></span>

- <span data-ttu-id="8ee16-158">Datové typy řetězec maximálně 32 znaků může vyžadovat další bajtů 32 za hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8ee16-158">String data types up to 32 characters can require 32 additional bytes per value.</span></span>
- <span data-ttu-id="8ee16-159">Řetězce s více než 32 znaků jsou komprimována pomocí metody slovníku.</span><span class="sxs-lookup"><span data-stu-id="8ee16-159">String data types with more than 32 characters are compressed using dictionary methods.</span></span>  <span data-ttu-id="8ee16-160">Každý sloupec v rowgroup může vyžadovat až dalších 16 MB k vytvoření slovníku.</span><span class="sxs-lookup"><span data-stu-id="8ee16-160">Each column in the rowgroup can require up to an additional 16 MB to build the dictionary.</span></span>

### <a name="avoid-over-partitioning"></a><span data-ttu-id="8ee16-161">Vyhněte se přepsání dělení</span><span class="sxs-lookup"><span data-stu-id="8ee16-161">Avoid over-partitioning</span></span>

<span data-ttu-id="8ee16-162">Indexy Columnstore vytvořit jeden nebo více rowgroups na jeden oddíl.</span><span class="sxs-lookup"><span data-stu-id="8ee16-162">Columnstore indexes create one or more rowgroups per partition.</span></span> <span data-ttu-id="8ee16-163">V SQL Data Warehouse počet oddílů zvětšování rychle vzhledem k tomu, že je distribuován data a každý distribuční je rozdělena na oddíly.</span><span class="sxs-lookup"><span data-stu-id="8ee16-163">In SQL Data Warehouse, the number of partitions grows quickly because the data is distributed and each distribution is partitioned.</span></span> <span data-ttu-id="8ee16-164">Pokud tabulka obsahuje příliš mnoho oddíly, nemusí být k vyplnění rowgroups dostatečný počet řádků.</span><span class="sxs-lookup"><span data-stu-id="8ee16-164">If the table has too many partitions, there might not be enough rows to fill the rowgroups.</span></span> <span data-ttu-id="8ee16-165">Nedostatečná řádků nevytvoří přetížení paměti při kompresi, ale jeho vede k rowgroups, který nezapojujte nejlepší výkon dotazů columnstore.</span><span class="sxs-lookup"><span data-stu-id="8ee16-165">The lack of rows does not create memory pressure during compression, but it leads to rowgroups that do not achieve the best columnstore query performance.</span></span>

<span data-ttu-id="8ee16-166">Dalším důvodem pro vyhnout přepsání rozdělení oddílů je, že není dostatek paměti režie pro načtení řádků do indexu columnstore v dělenou tabulku.</span><span class="sxs-lookup"><span data-stu-id="8ee16-166">Another reason to avoid over-partitioning is there is a memory overhead for loading rows into a columnstore index on a partitioned table.</span></span> <span data-ttu-id="8ee16-167">Během zatížení mnoha oddílů může přijímat příchozí řádky, které jsou uložené v paměti, dokud každý oddíl má dostatečný počet řádků, aby se komprimoval.</span><span class="sxs-lookup"><span data-stu-id="8ee16-167">During a load, many partitions could receive the incoming rows, which are held in memory until each partition has enough rows to be compressed.</span></span> <span data-ttu-id="8ee16-168">S příliš mnoha oddílů vytvoří přetížení další paměť.</span><span class="sxs-lookup"><span data-stu-id="8ee16-168">Having too many partitions creates additional memory pressure.</span></span>

### <a name="simplify-the-load-query"></a><span data-ttu-id="8ee16-169">Zjednodušte dotaz zatížení</span><span class="sxs-lookup"><span data-stu-id="8ee16-169">Simplify the load query</span></span>

<span data-ttu-id="8ee16-170">Databáze sdílí přidělení paměti pro dotaz mezi všechny operátory v dotazu.</span><span class="sxs-lookup"><span data-stu-id="8ee16-170">The database shares the memory grant for a query among all the operators in the query.</span></span> <span data-ttu-id="8ee16-171">Při načítání dotazu má rozšířené řazení a spojení, se snižuje paměti k dispozici pro kompresi.</span><span class="sxs-lookup"><span data-stu-id="8ee16-171">When a load query has complex sorts and joins, the memory available for compression is reduced.</span></span>

<span data-ttu-id="8ee16-172">Návrh zatížení dotaz tak, aby se zaměříte jenom na načítání dotazu.</span><span class="sxs-lookup"><span data-stu-id="8ee16-172">Design the load query to focus only on loading the query.</span></span> <span data-ttu-id="8ee16-173">Pokud potřebujete spustit transformace dat, spusťte je oddělené od zatížení dotazu.</span><span class="sxs-lookup"><span data-stu-id="8ee16-173">If you need to run transformations on the data, run them separate from the load query.</span></span> <span data-ttu-id="8ee16-174">Například příprava data v tabulce haldy, spusťte transformace a pak můžete načíst pracovní tabulky do indexu columnstore.</span><span class="sxs-lookup"><span data-stu-id="8ee16-174">For example, stage the data in a heap table, run the transformations, and then load the staging table into the columnstore index.</span></span> <span data-ttu-id="8ee16-175">Můžete také nejdřív načíst data a pak pomocí systému MPP transformovat data.</span><span class="sxs-lookup"><span data-stu-id="8ee16-175">You can also load the data first and then use the MPP system to transform the data.</span></span>

### <a name="adjust-maxdop"></a><span data-ttu-id="8ee16-176">Upravit MAXDOP</span><span class="sxs-lookup"><span data-stu-id="8ee16-176">Adjust MAXDOP</span></span>

<span data-ttu-id="8ee16-177">Každý distribuční zkomprimuje rowgroups do columnstore paralelně, je-li k dispozici na distribučních víc než jednoho jádra procesoru.</span><span class="sxs-lookup"><span data-stu-id="8ee16-177">Each distribution compresses rowgroups into the columnstore in parallel when there is more than one CPU core available per distribution.</span></span> <span data-ttu-id="8ee16-178">Paralelismus vyžaduje další paměťové prostředky, které může vést k přetížení paměti a oříznutí skupiny řádků.</span><span class="sxs-lookup"><span data-stu-id="8ee16-178">The parallelism requires additional memory resources, which can lead to memory pressure and rowgroup trimming.</span></span>

<span data-ttu-id="8ee16-179">Ke snížení přetížení paměti, můžete použít v pomocném parametru dotazu MAXDOP vynutit operace načtení spouštění v režimu sériového portu v rámci každé distribuční.</span><span class="sxs-lookup"><span data-stu-id="8ee16-179">To reduce memory pressure, you can use the MAXDOP query hint to force the load operation to run in serial mode within each distribution.</span></span>

```
CREATE TABLE MyFactSalesQuota
WITH (DISTRIBUTION = ROUND_ROBIN)
AS SELECT * FROM FactSalesQUota
OPTION (MAXDOP 1);
```

## <a name="ways-to-allocate-more-memory"></a><span data-ttu-id="8ee16-180">Způsoby přidělení více paměti</span><span class="sxs-lookup"><span data-stu-id="8ee16-180">Ways to allocate more memory</span></span>

<span data-ttu-id="8ee16-181">Velikost DWU a třída prostředků uživatele, společně určují, kolik paměti je k dispozici pro dotaz uživatele.</span><span class="sxs-lookup"><span data-stu-id="8ee16-181">DWU size and the user resource class together determine how much memory is available for a user query.</span></span> <span data-ttu-id="8ee16-182">Ke zvýšení přidělení paměti pro dotaz zatížení, můžete zvýšit počet Dwu nebo zvýšit Třída prostředků.</span><span class="sxs-lookup"><span data-stu-id="8ee16-182">To increase the memory grant for a load query, you can either increase the number of DWUs or increase the resource class.</span></span>

- <span data-ttu-id="8ee16-183">Pokud chcete zvýšit jednotkami Dwu, najdete v části [jak škálování výkonu?](sql-data-warehouse-manage-compute-overview.md#scale-compute)</span><span class="sxs-lookup"><span data-stu-id="8ee16-183">To increase the DWUs, see [How do I scale performance?](sql-data-warehouse-manage-compute-overview.md#scale-compute)</span></span>
- <span data-ttu-id="8ee16-184">Chcete-li změnit třídy prostředků pro dotaz, [změnit v příkladu třída prostředků uživatele](sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span><span class="sxs-lookup"><span data-stu-id="8ee16-184">To change the resource class for a query, see [Change a user resource class example](sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span></span>

<span data-ttu-id="8ee16-185">Například na DWU 100 může uživatel ve třídě prostředků smallrc používat 100 MB paměti pro každý distribuci.</span><span class="sxs-lookup"><span data-stu-id="8ee16-185">For example, on DWU 100 a user in the smallrc resource class can use 100 MB of memory for each distribution.</span></span> <span data-ttu-id="8ee16-186">Podrobnosti najdete v tématu [souběžnost v SQL Data Warehouse](sql-data-warehouse-develop-concurrency.md).</span><span class="sxs-lookup"><span data-stu-id="8ee16-186">For the details, see [Concurrency in SQL Data Warehouse](sql-data-warehouse-develop-concurrency.md).</span></span>

<span data-ttu-id="8ee16-187">Předpokládejme, že zjistíte, je nutné, aby 700 MB paměti pro získání vysoce kvalitní rowgroup velikosti.</span><span class="sxs-lookup"><span data-stu-id="8ee16-187">Suppose you determine that you need 700 MB of memory to get high-quality rowgroup sizes.</span></span> <span data-ttu-id="8ee16-188">Tyto příklady ukazují, jak můžete spuštěním dotazu zatížení s dostatek paměti.</span><span class="sxs-lookup"><span data-stu-id="8ee16-188">These examples show how you can run the load query with enough memory.</span></span>

- <span data-ttu-id="8ee16-189">Pomocí DWU 1000 a mediumrc, vaše přidělení paměti je 800 MB</span><span class="sxs-lookup"><span data-stu-id="8ee16-189">Using DWU 1000 and mediumrc, your memory grant is 800 MB</span></span>
- <span data-ttu-id="8ee16-190">Pomocí DWU 600 a largerc, vaše přidělení paměti je 800 MB.</span><span class="sxs-lookup"><span data-stu-id="8ee16-190">Using DWU 600 and largerc, your memory grant is 800 MB.</span></span>


## <a name="next-steps"></a><span data-ttu-id="8ee16-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8ee16-191">Next steps</span></span>

<span data-ttu-id="8ee16-192">Další způsoby, jak zlepšit výkon v SQL Data Warehouse, najdete v tématu [přehled výkonnostní](sql-data-warehouse-overview-manage-user-queries.md).</span><span class="sxs-lookup"><span data-stu-id="8ee16-192">To find more ways to improve performance in SQL Data Warehouse, see the [Performance overview](sql-data-warehouse-overview-manage-user-queries.md).</span></span>

<!--Image references-->

<!--Article references-->


<!--MSDN references-->

<!--Other Web references-->
