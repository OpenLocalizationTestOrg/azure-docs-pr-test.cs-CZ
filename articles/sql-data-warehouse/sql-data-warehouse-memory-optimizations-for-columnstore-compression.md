---
title: "výkon index columnstore aaaImprove v Azure SQL | Microsoft Docs"
description: "Snižte požadavky na paměť nebo zvyšte hello dostupné paměti toomaximize hello počet řádků, které columnstore index zkomprimuje do každé skupiny řádků."
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
ms.openlocfilehash: 2c5a68435aa200236a2dc8538aa4638b52a59093
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="maximizing-rowgroup-quality-for-columnstore"></a><span data-ttu-id="c4e07-103">Tím se maximalizuje quality rowgroup pro columnstore</span><span class="sxs-lookup"><span data-stu-id="c4e07-103">Maximizing rowgroup quality for columnstore</span></span>

<span data-ttu-id="c4e07-104">Kvalita rowgroup je určen podle hello počet řádků v skupiny řádků.</span><span class="sxs-lookup"><span data-stu-id="c4e07-104">Rowgroup quality is determined by hello number of rows in a rowgroup.</span></span> <span data-ttu-id="c4e07-105">Snižte požadavky na paměť nebo zvyšte hello dostupné paměti toomaximize hello počet řádků, které columnstore index zkomprimuje do každé skupiny řádků.</span><span class="sxs-lookup"><span data-stu-id="c4e07-105">Reduce memory requirements or increase hello available memory toomaximize hello number of rows a columnstore index compresses into each rowgroup.</span></span>  <span data-ttu-id="c4e07-106">Použijte tyto metody tooimprove komprese rychlostí a výkon dotazů pro indexy columnstore.</span><span class="sxs-lookup"><span data-stu-id="c4e07-106">Use these methods tooimprove compression rates and query performance for columnstore indexes.</span></span>

## <a name="why-hello-rowgroup-size-matters"></a><span data-ttu-id="c4e07-107">Proč na záleží hello rowgroup velikost</span><span class="sxs-lookup"><span data-stu-id="c4e07-107">Why hello rowgroup size matters</span></span>
<span data-ttu-id="c4e07-108">Protože columnstore index tabulku kontroluje naskenováním segmenty sloupce s jednotlivých rowgroups, tím se maximalizuje hello počet řádků v každém rowgroup vylepšuje výkon dotazů.</span><span class="sxs-lookup"><span data-stu-id="c4e07-108">Since a columnstore index scans a table by scanning column segments of individual rowgroups, maximizing hello number of rows in each rowgroup enhances query performance.</span></span> <span data-ttu-id="c4e07-109">Pokud rowgroups velký počet řádků, zlepšuje komprese dat, což znamená, že je menší tooread dat z disku.</span><span class="sxs-lookup"><span data-stu-id="c4e07-109">When rowgroups have a high number of rows, data compression improves which means there is less data tooread from disk.</span></span>

<span data-ttu-id="c4e07-110">Další informace o rowgroups najdete v tématu [Průvodce indexy Columnstore](https://msdn.microsoft.com/library/gg492088.aspx).</span><span class="sxs-lookup"><span data-stu-id="c4e07-110">For more information about rowgroups, see [Columnstore Indexes Guide](https://msdn.microsoft.com/library/gg492088.aspx).</span></span>

## <a name="target-size-for-rowgroups"></a><span data-ttu-id="c4e07-111">Cílovou velikost rowgroups</span><span class="sxs-lookup"><span data-stu-id="c4e07-111">Target size for rowgroups</span></span>
<span data-ttu-id="c4e07-112">Pro nejlepší výkon dotazů je cílem hello toomaximize hello počet řádků na jeden rowgroup v indexu columnstore.</span><span class="sxs-lookup"><span data-stu-id="c4e07-112">For best query performance, hello goal is toomaximize hello number of rows per rowgroup in a columnstore index.</span></span> <span data-ttu-id="c4e07-113">Rowgroup může mít maximálně 1 048 576 řádků.</span><span class="sxs-lookup"><span data-stu-id="c4e07-113">A rowgroup can have a maximum of 1,048,576 rows.</span></span> <span data-ttu-id="c4e07-114">Případe toonot mít hello maximální počet řádků na jeden skupiny řádků.</span><span class="sxs-lookup"><span data-stu-id="c4e07-114">It's okay toonot have hello maximum number of rows per rowgroup.</span></span> <span data-ttu-id="c4e07-115">Indexy Columnstore dosáhli dobrého výkonu, pokud rowgroups nejméně 100 000 řádků.</span><span class="sxs-lookup"><span data-stu-id="c4e07-115">Columnstore indexes achieve good performance when rowgroups have at least 100,000 rows.</span></span>

## <a name="rowgroups-can-get-trimmed-during-compression"></a><span data-ttu-id="c4e07-116">Během komprese můžete získat oříznut Rowgroups</span><span class="sxs-lookup"><span data-stu-id="c4e07-116">Rowgroups can get trimmed during compression</span></span>

<span data-ttu-id="c4e07-117">Během hromadné načtení nebo columnstore index opětovném sestavení někdy není k dispozici dostatek paměti k dispozici toocompress všechny hello řádky určené pro každé skupiny řádků.</span><span class="sxs-lookup"><span data-stu-id="c4e07-117">During a bulk load or columnstore index rebuild, sometimes there isn't enough memory available toocompress all hello rows designated for each rowgroup.</span></span> <span data-ttu-id="c4e07-118">Při přetížení paměti indexy columnstore trim hello rowgroup velikosti, komprese do hello columnstore mohlo být úspěšné.</span><span class="sxs-lookup"><span data-stu-id="c4e07-118">When there is memory pressure, columnstore indexes trim hello rowgroup sizes so compression into hello columnstore can succeed.</span></span> 

<span data-ttu-id="c4e07-119">Pokud nastal nedostatek paměti toocompress alespoň 10 000 řádků do každé skupiny řádků, SQL Data Warehouse, vygeneruje se chyba.</span><span class="sxs-lookup"><span data-stu-id="c4e07-119">When there is insufficient memory toocompress at least 10,000 rows into each rowgroup, SQL Data Warehouse generates an error.</span></span>

<span data-ttu-id="c4e07-120">Další informace o hromadné načítání, najdete v části [hromadné načtení do clusterovaný index columnstore](https://msdn.microsoft.com/en-us/library/dn935008.aspx#Bulk load into a clustered columnstore index).</span><span class="sxs-lookup"><span data-stu-id="c4e07-120">For more information on bulk loading, see [Bulk load into a clustered columnstore index](https://msdn.microsoft.com/en-us/library/dn935008.aspx#Bulk load into a clustered columnstore index).</span></span>

## <a name="how-toomonitor-rowgroup-quality"></a><span data-ttu-id="c4e07-121">Jak toomonitor rowgroup kvality</span><span class="sxs-lookup"><span data-stu-id="c4e07-121">How toomonitor rowgroup quality</span></span>

<span data-ttu-id="c4e07-122">Není DMV (sys.dm_pdw_nodes_db_column_store_row_group_physical_stats), který zveřejňuje užitečné informace, jako je počet řádků v rowgroups a hello důvod oříznutí, pokud byl ořezávání.</span><span class="sxs-lookup"><span data-stu-id="c4e07-122">There is a DMV (sys.dm_pdw_nodes_db_column_store_row_group_physical_stats) that exposes useful information such as number of rows in rowgroups and hello reason for trimming if there was trimming.</span></span> <span data-ttu-id="c4e07-123">Tyto informace tooget DMV hello následující zobrazení jako tooquery užitečný způsob, jak můžete vytvořit na rowgroup oříznutí.</span><span class="sxs-lookup"><span data-stu-id="c4e07-123">You can create hello following view as a handy way tooquery this DMV tooget information on rowgroup trimming.</span></span>

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

<span data-ttu-id="c4e07-124">Hello trim_reason_desc oznamuje, zda byl oříznut hello rowgroup (trim_reason_desc = NO_TRIM znamená došlo bez oříznutí a skupiny řádků je optimální kvality).</span><span class="sxs-lookup"><span data-stu-id="c4e07-124">hello trim_reason_desc tells whether hello rowgroup was trimmed(trim_reason_desc = NO_TRIM implies there was no trimming and row group is of optimal quality).</span></span> <span data-ttu-id="c4e07-125">Hello uvolnění dočasné paměti z následujících důvodů znamenat předčasné ořezávání hello rowgroup:</span><span class="sxs-lookup"><span data-stu-id="c4e07-125">hello following trim reasons indicate premature trimming of hello rowgroup:</span></span>
- <span data-ttu-id="c4e07-126">BULKLOAD: Z tohoto důvodu uvolnění dočasné paměti se používá při hello příchozí dávku řádků pro zatížení hello měl menší než 1 milionu řádků.</span><span class="sxs-lookup"><span data-stu-id="c4e07-126">BULKLOAD: This trim reason is used when hello incoming batch of rows for hello load had less than 1 million rows.</span></span> <span data-ttu-id="c4e07-127">modul Hello vytvoří skupiny komprimované řádků, pokud jsou větší než 100 000 řádků vkládání (jako názvem na rozdíl od tooinserting do úložiště rozdílů hello), ale nastaví hello tooBULKLOAD trim důvod.</span><span class="sxs-lookup"><span data-stu-id="c4e07-127">hello engine will create compressed row groups if there are greater than 100,000 rows being inserted (as opposed tooinserting into hello delta store) but sets hello trim reason tooBULKLOAD.</span></span> <span data-ttu-id="c4e07-128">V tomto scénáři zvažte zvýšení vaší batch zatížení okno tooaccumulate další řádky.</span><span class="sxs-lookup"><span data-stu-id="c4e07-128">In this scenario, consider increasing your batch load window tooaccumulate more rows.</span></span> <span data-ttu-id="c4e07-129">Navíc přehodnocovat vaší rozdělení schéma tooensure není příliš podrobné jako skupiny řádků nemůžou zahrnovat hranice oddílů.</span><span class="sxs-lookup"><span data-stu-id="c4e07-129">Also, reevaluate your partitioning scheme tooensure it is not too granular as row groups cannot span partition boundaries.</span></span>
- <span data-ttu-id="c4e07-130">MEMORY_LIMITATION: toocreate skupin řádků s 1 milionu řádků, množství paměti pracovní vyžadují modul hello.</span><span class="sxs-lookup"><span data-stu-id="c4e07-130">MEMORY_LIMITATION: toocreate row groups with 1 million rows, a certain amount of working memory is required by hello engine.</span></span> <span data-ttu-id="c4e07-131">Při načítání relace hello dostupné paměti je menší než hello potřebné práce paměti, získat předčasně oříznut skupiny řádků.</span><span class="sxs-lookup"><span data-stu-id="c4e07-131">When available memory of hello loading session is less than hello required working memory, row groups get prematurely trimmed.</span></span> <span data-ttu-id="c4e07-132">Hello následující části popisují, jak tooestimate paměti vyžaduje a přidělení více paměti.</span><span class="sxs-lookup"><span data-stu-id="c4e07-132">hello following sections explain how tooestimate memory required and allocate more memory.</span></span>
- <span data-ttu-id="c4e07-133">DICTIONARY_SIZE: Z tohoto důvodu trim označuje, že rowgroup oříznutí došlo k chybě kvůli alespoň jeden sloupec řetězec s širokým nebo vysokou kardinalitou řetězce.</span><span class="sxs-lookup"><span data-stu-id="c4e07-133">DICTIONARY_SIZE: This trim reason indicates that rowgroup trimming occurred because there was at least one string column with wide and/or high cardinality strings.</span></span> <span data-ttu-id="c4e07-134">velikost slovník Hello je omezená too16, které se komprimují MB paměti a po dosažení tohoto limitu hello skupiny řádků.</span><span class="sxs-lookup"><span data-stu-id="c4e07-134">hello dictionary size is limited too16 MB in memory and once this limit is reached hello row group is compressed.</span></span> <span data-ttu-id="c4e07-135">Pokud k této situaci, zvažte možnost odizolování hello problematické sloupce do samostatné tabulky.</span><span class="sxs-lookup"><span data-stu-id="c4e07-135">If you do run into this situation, consider isolating hello problematic column into a separate table.</span></span>

## <a name="how-tooestimate-memory-requirements"></a><span data-ttu-id="c4e07-136">Jak tooestimate požadavky na paměť</span><span class="sxs-lookup"><span data-stu-id="c4e07-136">How tooestimate memory requirements</span></span>

<!--
tooview an estimate of hello memory requirements toocompress a rowgroup of maximum size into a columnstore index, download and run hello view [dbo.vCS_mon_mem_grant](). This view shows hello size of hello memory grant that a rowgroup requires for compression in toohello columnstore.
-->

<span data-ttu-id="c4e07-137">jeden rowgroup toocompress Hello maximální požadovanou paměť je přibližně</span><span class="sxs-lookup"><span data-stu-id="c4e07-137">hello maximum required memory toocompress one rowgroup is approximately</span></span>

- <span data-ttu-id="c4e07-138">72 MB +</span><span class="sxs-lookup"><span data-stu-id="c4e07-138">72 MB +</span></span>
- <span data-ttu-id="c4e07-139">\#řádky \* \#sloupce \* 8 bajtů +</span><span class="sxs-lookup"><span data-stu-id="c4e07-139">\#rows \* \#columns \* 8 bytes +</span></span>
- <span data-ttu-id="c4e07-140">\#řádky \* \#krátké. řetězcové sloupce \* 32 bajtů +</span><span class="sxs-lookup"><span data-stu-id="c4e07-140">\#rows \* \#short-string-columns \* 32 bytes +</span></span>
- <span data-ttu-id="c4e07-141">\#Long. řetězcové sloupce \* 16 MB pro slovník komprese</span><span class="sxs-lookup"><span data-stu-id="c4e07-141">\#long-string-columns \* 16 MB for compression dictionary</span></span>

<span data-ttu-id="c4e07-142">kde krátké. řetězcové sloupce použít datové typy řetězec < = 32 bajtů a datové typy řetězec použití dlouho. řetězcové sloupce > 32 bajtů.</span><span class="sxs-lookup"><span data-stu-id="c4e07-142">where short-string-columns use string data types of <= 32 bytes and long-string-columns use string data types of > 32 bytes.</span></span>

<span data-ttu-id="c4e07-143">Dlouhé řetězce jsou komprimována pomocí metody komprese určený pro kompresi text.</span><span class="sxs-lookup"><span data-stu-id="c4e07-143">Long strings are compressed with a compression method designed for compressing text.</span></span> <span data-ttu-id="c4e07-144">Tato metoda komprese používá *slovník* toostore textové vzory.</span><span class="sxs-lookup"><span data-stu-id="c4e07-144">This compression method uses a *dictionary* toostore text patterns.</span></span> <span data-ttu-id="c4e07-145">maximální velikost Hello slovník je 16 MB.</span><span class="sxs-lookup"><span data-stu-id="c4e07-145">hello maximum size of a dictionary is 16 MB.</span></span> <span data-ttu-id="c4e07-146">Není k dispozici pouze jeden slovník pro každý sloupec dlouhý řetězec v hello rowgroup.</span><span class="sxs-lookup"><span data-stu-id="c4e07-146">There is only one dictionary for each long string column in hello rowgroup.</span></span>

<span data-ttu-id="c4e07-147">Podrobné informace o požadavky na paměť columnstore, najdete v části video [škálování Azure SQL Data Warehouse: Konfigurace a pokyny](https://myignite.microsoft.com/videos/14822).</span><span class="sxs-lookup"><span data-stu-id="c4e07-147">For an in-depth discussion of columnstore memory requirements, see the video [Azure SQL Data Warehouse scaling: configuration and guidance](https://myignite.microsoft.com/videos/14822).</span></span>

## <a name="ways-tooreduce-memory-requirements"></a><span data-ttu-id="c4e07-148">Požadavky na paměť tooreduce způsoby</span><span class="sxs-lookup"><span data-stu-id="c4e07-148">Ways tooreduce memory requirements</span></span>

<span data-ttu-id="c4e07-149">Použijte následující postupy tooreduce hello požadavky na paměť pro kompresi rowgroups do indexy columnstore hello.</span><span class="sxs-lookup"><span data-stu-id="c4e07-149">Use hello following techniques tooreduce hello memory requirements for compressing rowgroups into columnstore indexes.</span></span>

### <a name="use-fewer-columns"></a><span data-ttu-id="c4e07-150">Použití méně sloupců</span><span class="sxs-lookup"><span data-stu-id="c4e07-150">Use fewer columns</span></span>
<span data-ttu-id="c4e07-151">Pokud je to možné návrh hello tabulku s méně sloupců.</span><span class="sxs-lookup"><span data-stu-id="c4e07-151">If possible, design hello table with fewer columns.</span></span> <span data-ttu-id="c4e07-152">Když do hello columnstore se komprimují rowgroup, hello columnstore index komprimaci každý segment sloupce samostatně.</span><span class="sxs-lookup"><span data-stu-id="c4e07-152">When a rowgroup is compressed into hello columnstore, hello columnstore index compresses each column segment separately.</span></span> <span data-ttu-id="c4e07-153">Proto text hello, které toocompress rowgroup zvýšit jako hello počet sloupců zvyšuje požadavky na paměť.</span><span class="sxs-lookup"><span data-stu-id="c4e07-153">Therefore hello memory requirements toocompress a rowgroup increase as hello number of columns increases.</span></span>


### <a name="use-fewer-string-columns"></a><span data-ttu-id="c4e07-154">Použití méně sloupců řetězec</span><span class="sxs-lookup"><span data-stu-id="c4e07-154">Use fewer string columns</span></span>
<span data-ttu-id="c4e07-155">Sloupce řetězce vyžadují větší spotřebu paměti než číselné a datový typ datum.</span><span class="sxs-lookup"><span data-stu-id="c4e07-155">Columns of string data types require more memory than numeric and date data types.</span></span> <span data-ttu-id="c4e07-156">požadavky na paměť tooreduce, zvažte odebrání řetězcové sloupce z tabulky faktů a jejich uvedení v menší tabulky dimenzí.</span><span class="sxs-lookup"><span data-stu-id="c4e07-156">tooreduce memory requirements, consider removing string columns from fact tables and putting them in smaller dimension tables.</span></span>

<span data-ttu-id="c4e07-157">Požadavky na další paměť pro kompresi řetězec:</span><span class="sxs-lookup"><span data-stu-id="c4e07-157">Additional memory requirements for string compression:</span></span>

- <span data-ttu-id="c4e07-158">Datové typy řetězec až too32 znaků může vyžadovat 32 další bajtů pro hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c4e07-158">String data types up too32 characters can require 32 additional bytes per value.</span></span>
- <span data-ttu-id="c4e07-159">Řetězce s více než 32 znaků jsou komprimována pomocí metody slovníku.</span><span class="sxs-lookup"><span data-stu-id="c4e07-159">String data types with more than 32 characters are compressed using dictionary methods.</span></span>  <span data-ttu-id="c4e07-160">Každý sloupec v hello rowgroup může vyžadovat až tooan další 16 MB toobuild hello slovníku.</span><span class="sxs-lookup"><span data-stu-id="c4e07-160">Each column in hello rowgroup can require up tooan additional 16 MB toobuild hello dictionary.</span></span>

### <a name="avoid-over-partitioning"></a><span data-ttu-id="c4e07-161">Vyhněte se přepsání dělení</span><span class="sxs-lookup"><span data-stu-id="c4e07-161">Avoid over-partitioning</span></span>

<span data-ttu-id="c4e07-162">Indexy Columnstore vytvořit jeden nebo více rowgroups na jeden oddíl.</span><span class="sxs-lookup"><span data-stu-id="c4e07-162">Columnstore indexes create one or more rowgroups per partition.</span></span> <span data-ttu-id="c4e07-163">V SQL Data Warehouse hello počet oddílů zvětšování rychle protože distribuci dat hello a každý distribuční je rozdělena na oddíly.</span><span class="sxs-lookup"><span data-stu-id="c4e07-163">In SQL Data Warehouse, hello number of partitions grows quickly because hello data is distributed and each distribution is partitioned.</span></span> <span data-ttu-id="c4e07-164">Pokud hello tabulka má příliš mnoho oddíly, nemusí být dost rowgroups hello toofill řádků.</span><span class="sxs-lookup"><span data-stu-id="c4e07-164">If hello table has too many partitions, there might not be enough rows toofill hello rowgroups.</span></span> <span data-ttu-id="c4e07-165">Nedostatečná Hello řádků nevytvoří přetížení paměti při kompresi, ale jeho vede toorowgroups, který nezapojujte hello nejlepší výkon dotazů columnstore.</span><span class="sxs-lookup"><span data-stu-id="c4e07-165">hello lack of rows does not create memory pressure during compression, but it leads toorowgroups that do not achieve hello best columnstore query performance.</span></span>

<span data-ttu-id="c4e07-166">Jiné důvod tooavoid přepsání rozdělení do oddílů je, že není dostatek paměti režie pro načtení řádků do indexu columnstore v dělenou tabulku.</span><span class="sxs-lookup"><span data-stu-id="c4e07-166">Another reason tooavoid over-partitioning is there is a memory overhead for loading rows into a columnstore index on a partitioned table.</span></span> <span data-ttu-id="c4e07-167">Při načítání mnoha oddílů obdržet hello příchozí řádky, které jsou uložené v paměti, dokud každý oddíl má dostatek řádky toobe komprimované.</span><span class="sxs-lookup"><span data-stu-id="c4e07-167">During a load, many partitions could receive hello incoming rows, which are held in memory until each partition has enough rows toobe compressed.</span></span> <span data-ttu-id="c4e07-168">S příliš mnoha oddílů vytvoří přetížení další paměť.</span><span class="sxs-lookup"><span data-stu-id="c4e07-168">Having too many partitions creates additional memory pressure.</span></span>

### <a name="simplify-hello-load-query"></a><span data-ttu-id="c4e07-169">Zjednodušení hello zatížení dotazu</span><span class="sxs-lookup"><span data-stu-id="c4e07-169">Simplify hello load query</span></span>

<span data-ttu-id="c4e07-170">databáze Hello sdílí hello přidělení paměti pro dotaz mezi všechny operátory hello v dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="c4e07-170">hello database shares hello memory grant for a query among all hello operators in hello query.</span></span> <span data-ttu-id="c4e07-171">Při načítání dotazu má rozšířené řazení a spojení, se snižuje hello paměti k dispozici pro kompresi.</span><span class="sxs-lookup"><span data-stu-id="c4e07-171">When a load query has complex sorts and joins, hello memory available for compression is reduced.</span></span>

<span data-ttu-id="c4e07-172">Návrh hello zatížení dotazu toofocus pouze k načítání dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="c4e07-172">Design hello load query toofocus only on loading hello query.</span></span> <span data-ttu-id="c4e07-173">Pokud potřebujete toorun transformací hello dat, spusťte je oddělené od hello zatížení dotazu.</span><span class="sxs-lookup"><span data-stu-id="c4e07-173">If you need toorun transformations on hello data, run them separate from hello load query.</span></span> <span data-ttu-id="c4e07-174">Fáze hello data v tabulce haldy, například spustit transformace hello a pak můžete načíst hello přípravná tabulka do indexu columnstore hello.</span><span class="sxs-lookup"><span data-stu-id="c4e07-174">For example, stage hello data in a heap table, run hello transformations, and then load hello staging table into hello columnstore index.</span></span> <span data-ttu-id="c4e07-175">Můžete také načíst hello data nejprve a pak použít hello MPP systému tootransform hello data.</span><span class="sxs-lookup"><span data-stu-id="c4e07-175">You can also load hello data first and then use hello MPP system tootransform hello data.</span></span>

### <a name="adjust-maxdop"></a><span data-ttu-id="c4e07-176">Upravit MAXDOP</span><span class="sxs-lookup"><span data-stu-id="c4e07-176">Adjust MAXDOP</span></span>

<span data-ttu-id="c4e07-177">Každý distribuční zkomprimuje rowgroups do columnstore hello paralelně, je-li k dispozici na distribučních víc než jednoho jádra procesoru.</span><span class="sxs-lookup"><span data-stu-id="c4e07-177">Each distribution compresses rowgroups into hello columnstore in parallel when there is more than one CPU core available per distribution.</span></span> <span data-ttu-id="c4e07-178">paralelismus Hello vyžaduje další paměťové prostředky, které může způsobit toomemory zatížení a oříznutí skupiny řádků.</span><span class="sxs-lookup"><span data-stu-id="c4e07-178">hello parallelism requires additional memory resources, which can lead toomemory pressure and rowgroup trimming.</span></span>

<span data-ttu-id="c4e07-179">tooreduce přetížení paměti, můžete použít hello MAXDOP dotazu pomocný parametr tooforce hello zatížení operace toorun v sériové režimu v rámci každé distribuce.</span><span class="sxs-lookup"><span data-stu-id="c4e07-179">tooreduce memory pressure, you can use hello MAXDOP query hint tooforce hello load operation toorun in serial mode within each distribution.</span></span>

```
CREATE TABLE MyFactSalesQuota
WITH (DISTRIBUTION = ROUND_ROBIN)
AS SELECT * FROM FactSalesQUota
OPTION (MAXDOP 1);
```

## <a name="ways-tooallocate-more-memory"></a><span data-ttu-id="c4e07-180">Způsoby tooallocate více paměti</span><span class="sxs-lookup"><span data-stu-id="c4e07-180">Ways tooallocate more memory</span></span>

<span data-ttu-id="c4e07-181">Třída prostředků uživatele DWU pro velikost a hello společně určit, kolik paměti je k dispozici pro dotaz uživatele.</span><span class="sxs-lookup"><span data-stu-id="c4e07-181">DWU size and hello user resource class together determine how much memory is available for a user query.</span></span> <span data-ttu-id="c4e07-182">paměť hello tooincrease udělit pro dotaz zatížení, můžete zvýšit hello počet Dwu nebo zvýšit Třída prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="c4e07-182">tooincrease hello memory grant for a load query, you can either increase hello number of DWUs or increase hello resource class.</span></span>

- <span data-ttu-id="c4e07-183">hello tooincrease Dwu, najdete v části [jak škálování výkonu?](sql-data-warehouse-manage-compute-overview.md#scale-compute)</span><span class="sxs-lookup"><span data-stu-id="c4e07-183">tooincrease hello DWUs, see [How do I scale performance?](sql-data-warehouse-manage-compute-overview.md#scale-compute)</span></span>
- <span data-ttu-id="c4e07-184">Třída prostředků hello toochange pro daný dotaz, najdete v části [změnit v příkladu třída prostředků uživatele](sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span><span class="sxs-lookup"><span data-stu-id="c4e07-184">toochange hello resource class for a query, see [Change a user resource class example](sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span></span>

<span data-ttu-id="c4e07-185">Například na DWU 100 může uživatel v třídě prostředků smallrc hello používat 100 MB paměti pro každý distribuci.</span><span class="sxs-lookup"><span data-stu-id="c4e07-185">For example, on DWU 100 a user in hello smallrc resource class can use 100 MB of memory for each distribution.</span></span> <span data-ttu-id="c4e07-186">Hello podrobnosti najdete v tématu [souběžnost v SQL Data Warehouse](sql-data-warehouse-develop-concurrency.md).</span><span class="sxs-lookup"><span data-stu-id="c4e07-186">For hello details, see [Concurrency in SQL Data Warehouse](sql-data-warehouse-develop-concurrency.md).</span></span>

<span data-ttu-id="c4e07-187">Předpokládejme, že zjistíte, je nutné, aby 700 MB paměti tooget vysoce kvalitní rowgroup velikostí.</span><span class="sxs-lookup"><span data-stu-id="c4e07-187">Suppose you determine that you need 700 MB of memory tooget high-quality rowgroup sizes.</span></span> <span data-ttu-id="c4e07-188">Tyto příklady ukazují, jak můžete spuštěním dotazu zatížení hello s dostatek paměti.</span><span class="sxs-lookup"><span data-stu-id="c4e07-188">These examples show how you can run hello load query with enough memory.</span></span>

- <span data-ttu-id="c4e07-189">Pomocí DWU 1000 a mediumrc, vaše přidělení paměti je 800 MB</span><span class="sxs-lookup"><span data-stu-id="c4e07-189">Using DWU 1000 and mediumrc, your memory grant is 800 MB</span></span>
- <span data-ttu-id="c4e07-190">Pomocí DWU 600 a largerc, vaše přidělení paměti je 800 MB.</span><span class="sxs-lookup"><span data-stu-id="c4e07-190">Using DWU 600 and largerc, your memory grant is 800 MB.</span></span>


## <a name="next-steps"></a><span data-ttu-id="c4e07-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c4e07-191">Next steps</span></span>

<span data-ttu-id="c4e07-192">toofind další způsoby tooimprove výkonu v SQL Data Warehouse, najdete v části hello [přehled výkonnostní](sql-data-warehouse-overview-manage-user-queries.md).</span><span class="sxs-lookup"><span data-stu-id="c4e07-192">toofind more ways tooimprove performance in SQL Data Warehouse, see hello [Performance overview](sql-data-warehouse-overview-manage-user-queries.md).</span></span>

<!--Image references-->

<!--Article references-->


<!--MSDN references-->

<!--Other Web references-->
