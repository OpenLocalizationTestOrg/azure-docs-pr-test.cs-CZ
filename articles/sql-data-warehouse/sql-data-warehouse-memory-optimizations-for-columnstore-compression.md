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
# <a name="maximizing-rowgroup-quality-for-columnstore"></a>Tím se maximalizuje quality rowgroup pro columnstore

Kvalita rowgroup je určen podle hello počet řádků v skupiny řádků. Snižte požadavky na paměť nebo zvyšte hello dostupné paměti toomaximize hello počet řádků, které columnstore index zkomprimuje do každé skupiny řádků.  Použijte tyto metody tooimprove komprese rychlostí a výkon dotazů pro indexy columnstore.

## <a name="why-hello-rowgroup-size-matters"></a>Proč na záleží hello rowgroup velikost
Protože columnstore index tabulku kontroluje naskenováním segmenty sloupce s jednotlivých rowgroups, tím se maximalizuje hello počet řádků v každém rowgroup vylepšuje výkon dotazů. Pokud rowgroups velký počet řádků, zlepšuje komprese dat, což znamená, že je menší tooread dat z disku.

Další informace o rowgroups najdete v tématu [Průvodce indexy Columnstore](https://msdn.microsoft.com/library/gg492088.aspx).

## <a name="target-size-for-rowgroups"></a>Cílovou velikost rowgroups
Pro nejlepší výkon dotazů je cílem hello toomaximize hello počet řádků na jeden rowgroup v indexu columnstore. Rowgroup může mít maximálně 1 048 576 řádků. Případe toonot mít hello maximální počet řádků na jeden skupiny řádků. Indexy Columnstore dosáhli dobrého výkonu, pokud rowgroups nejméně 100 000 řádků.

## <a name="rowgroups-can-get-trimmed-during-compression"></a>Během komprese můžete získat oříznut Rowgroups

Během hromadné načtení nebo columnstore index opětovném sestavení někdy není k dispozici dostatek paměti k dispozici toocompress všechny hello řádky určené pro každé skupiny řádků. Při přetížení paměti indexy columnstore trim hello rowgroup velikosti, komprese do hello columnstore mohlo být úspěšné. 

Pokud nastal nedostatek paměti toocompress alespoň 10 000 řádků do každé skupiny řádků, SQL Data Warehouse, vygeneruje se chyba.

Další informace o hromadné načítání, najdete v části [hromadné načtení do clusterovaný index columnstore](https://msdn.microsoft.com/en-us/library/dn935008.aspx#Bulk load into a clustered columnstore index).

## <a name="how-toomonitor-rowgroup-quality"></a>Jak toomonitor rowgroup kvality

Není DMV (sys.dm_pdw_nodes_db_column_store_row_group_physical_stats), který zveřejňuje užitečné informace, jako je počet řádků v rowgroups a hello důvod oříznutí, pokud byl ořezávání. Tyto informace tooget DMV hello následující zobrazení jako tooquery užitečný způsob, jak můžete vytvořit na rowgroup oříznutí.

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

Hello trim_reason_desc oznamuje, zda byl oříznut hello rowgroup (trim_reason_desc = NO_TRIM znamená došlo bez oříznutí a skupiny řádků je optimální kvality). Hello uvolnění dočasné paměti z následujících důvodů znamenat předčasné ořezávání hello rowgroup:
- BULKLOAD: Z tohoto důvodu uvolnění dočasné paměti se používá při hello příchozí dávku řádků pro zatížení hello měl menší než 1 milionu řádků. modul Hello vytvoří skupiny komprimované řádků, pokud jsou větší než 100 000 řádků vkládání (jako názvem na rozdíl od tooinserting do úložiště rozdílů hello), ale nastaví hello tooBULKLOAD trim důvod. V tomto scénáři zvažte zvýšení vaší batch zatížení okno tooaccumulate další řádky. Navíc přehodnocovat vaší rozdělení schéma tooensure není příliš podrobné jako skupiny řádků nemůžou zahrnovat hranice oddílů.
- MEMORY_LIMITATION: toocreate skupin řádků s 1 milionu řádků, množství paměti pracovní vyžadují modul hello. Při načítání relace hello dostupné paměti je menší než hello potřebné práce paměti, získat předčasně oříznut skupiny řádků. Hello následující části popisují, jak tooestimate paměti vyžaduje a přidělení více paměti.
- DICTIONARY_SIZE: Z tohoto důvodu trim označuje, že rowgroup oříznutí došlo k chybě kvůli alespoň jeden sloupec řetězec s širokým nebo vysokou kardinalitou řetězce. velikost slovník Hello je omezená too16, které se komprimují MB paměti a po dosažení tohoto limitu hello skupiny řádků. Pokud k této situaci, zvažte možnost odizolování hello problematické sloupce do samostatné tabulky.

## <a name="how-tooestimate-memory-requirements"></a>Jak tooestimate požadavky na paměť

<!--
tooview an estimate of hello memory requirements toocompress a rowgroup of maximum size into a columnstore index, download and run hello view [dbo.vCS_mon_mem_grant](). This view shows hello size of hello memory grant that a rowgroup requires for compression in toohello columnstore.
-->

jeden rowgroup toocompress Hello maximální požadovanou paměť je přibližně

- 72 MB +
- \#řádky \* \#sloupce \* 8 bajtů +
- \#řádky \* \#krátké. řetězcové sloupce \* 32 bajtů +
- \#Long. řetězcové sloupce \* 16 MB pro slovník komprese

kde krátké. řetězcové sloupce použít datové typy řetězec < = 32 bajtů a datové typy řetězec použití dlouho. řetězcové sloupce > 32 bajtů.

Dlouhé řetězce jsou komprimována pomocí metody komprese určený pro kompresi text. Tato metoda komprese používá *slovník* toostore textové vzory. maximální velikost Hello slovník je 16 MB. Není k dispozici pouze jeden slovník pro každý sloupec dlouhý řetězec v hello rowgroup.

Podrobné informace o požadavky na paměť columnstore, najdete v části video [škálování Azure SQL Data Warehouse: Konfigurace a pokyny](https://myignite.microsoft.com/videos/14822).

## <a name="ways-tooreduce-memory-requirements"></a>Požadavky na paměť tooreduce způsoby

Použijte následující postupy tooreduce hello požadavky na paměť pro kompresi rowgroups do indexy columnstore hello.

### <a name="use-fewer-columns"></a>Použití méně sloupců
Pokud je to možné návrh hello tabulku s méně sloupců. Když do hello columnstore se komprimují rowgroup, hello columnstore index komprimaci každý segment sloupce samostatně. Proto text hello, které toocompress rowgroup zvýšit jako hello počet sloupců zvyšuje požadavky na paměť.


### <a name="use-fewer-string-columns"></a>Použití méně sloupců řetězec
Sloupce řetězce vyžadují větší spotřebu paměti než číselné a datový typ datum. požadavky na paměť tooreduce, zvažte odebrání řetězcové sloupce z tabulky faktů a jejich uvedení v menší tabulky dimenzí.

Požadavky na další paměť pro kompresi řetězec:

- Datové typy řetězec až too32 znaků může vyžadovat 32 další bajtů pro hodnotu.
- Řetězce s více než 32 znaků jsou komprimována pomocí metody slovníku.  Každý sloupec v hello rowgroup může vyžadovat až tooan další 16 MB toobuild hello slovníku.

### <a name="avoid-over-partitioning"></a>Vyhněte se přepsání dělení

Indexy Columnstore vytvořit jeden nebo více rowgroups na jeden oddíl. V SQL Data Warehouse hello počet oddílů zvětšování rychle protože distribuci dat hello a každý distribuční je rozdělena na oddíly. Pokud hello tabulka má příliš mnoho oddíly, nemusí být dost rowgroups hello toofill řádků. Nedostatečná Hello řádků nevytvoří přetížení paměti při kompresi, ale jeho vede toorowgroups, který nezapojujte hello nejlepší výkon dotazů columnstore.

Jiné důvod tooavoid přepsání rozdělení do oddílů je, že není dostatek paměti režie pro načtení řádků do indexu columnstore v dělenou tabulku. Při načítání mnoha oddílů obdržet hello příchozí řádky, které jsou uložené v paměti, dokud každý oddíl má dostatek řádky toobe komprimované. S příliš mnoha oddílů vytvoří přetížení další paměť.

### <a name="simplify-hello-load-query"></a>Zjednodušení hello zatížení dotazu

databáze Hello sdílí hello přidělení paměti pro dotaz mezi všechny operátory hello v dotazu hello. Při načítání dotazu má rozšířené řazení a spojení, se snižuje hello paměti k dispozici pro kompresi.

Návrh hello zatížení dotazu toofocus pouze k načítání dotazu hello. Pokud potřebujete toorun transformací hello dat, spusťte je oddělené od hello zatížení dotazu. Fáze hello data v tabulce haldy, například spustit transformace hello a pak můžete načíst hello přípravná tabulka do indexu columnstore hello. Můžete také načíst hello data nejprve a pak použít hello MPP systému tootransform hello data.

### <a name="adjust-maxdop"></a>Upravit MAXDOP

Každý distribuční zkomprimuje rowgroups do columnstore hello paralelně, je-li k dispozici na distribučních víc než jednoho jádra procesoru. paralelismus Hello vyžaduje další paměťové prostředky, které může způsobit toomemory zatížení a oříznutí skupiny řádků.

tooreduce přetížení paměti, můžete použít hello MAXDOP dotazu pomocný parametr tooforce hello zatížení operace toorun v sériové režimu v rámci každé distribuce.

```
CREATE TABLE MyFactSalesQuota
WITH (DISTRIBUTION = ROUND_ROBIN)
AS SELECT * FROM FactSalesQUota
OPTION (MAXDOP 1);
```

## <a name="ways-tooallocate-more-memory"></a>Způsoby tooallocate více paměti

Třída prostředků uživatele DWU pro velikost a hello společně určit, kolik paměti je k dispozici pro dotaz uživatele. paměť hello tooincrease udělit pro dotaz zatížení, můžete zvýšit hello počet Dwu nebo zvýšit Třída prostředků hello.

- hello tooincrease Dwu, najdete v části [jak škálování výkonu?](sql-data-warehouse-manage-compute-overview.md#scale-compute)
- Třída prostředků hello toochange pro daný dotaz, najdete v části [změnit v příkladu třída prostředků uživatele](sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).

Například na DWU 100 může uživatel v třídě prostředků smallrc hello používat 100 MB paměti pro každý distribuci. Hello podrobnosti najdete v tématu [souběžnost v SQL Data Warehouse](sql-data-warehouse-develop-concurrency.md).

Předpokládejme, že zjistíte, je nutné, aby 700 MB paměti tooget vysoce kvalitní rowgroup velikostí. Tyto příklady ukazují, jak můžete spuštěním dotazu zatížení hello s dostatek paměti.

- Pomocí DWU 1000 a mediumrc, vaše přidělení paměti je 800 MB
- Pomocí DWU 600 a largerc, vaše přidělení paměti je 800 MB.


## <a name="next-steps"></a>Další kroky

toofind další způsoby tooimprove výkonu v SQL Data Warehouse, najdete v části hello [přehled výkonnostní](sql-data-warehouse-overview-manage-user-queries.md).

<!--Image references-->

<!--Article references-->


<!--MSDN references-->

<!--Other Web references-->
