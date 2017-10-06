---
title: "aaaHow distribuované funguje dat v Azure SQL Data Warehouse | Microsoft Docs"
description: "Zjistěte, jak distribuci dat pro masivně paralelní zpracování MPP a hello možností distribuce tabulek v Azure SQL Data Warehouse a Parallel Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: bae494a6-7ac5-4c38-8ca3-ab2696c63a9f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 07/12/2017
ms.author: jrj;barbkess
ms.openlocfilehash: 9a712d8d5251e4391ede245105918283aaa4b193
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="distributed-data-and-distributed-tables-for-massively-parallel-processing-mpp"></a>Rozložení dat a distribuované tabulky pro masivně paralelní zpracování MPP)
Zjistěte, jak distribuci dat uživatele v Azure SQL Data Warehouse a Parallel Data Warehouse, které jsou systémy masivně paralelní zpracování (MPP) společnosti Microsoft. Návrh vašeho datového skladu toouse distribuovaných datech efektivně pomáhá vám tooachieve hello zpracování dotazů výhod hello architektura MPP. Několik možností návrhu databáze může mít významný dopad na zlepšení výkonu dotazů.  

> [!NOTE]
> Azure SQL Data Warehouse a Parallel Data Warehouse pomocí hello návrh stejné Massively Parallel Processing (MPP), ale mají několik rozdílů kvůli hello základní platformy. SQL Data Warehouse je platforma jako služba (PaaS), která běží na Azure. Parallel Data Warehouse se spustí na Analytics Platform System (AP), což je místní zařízení, která běží na systému Windows Server.
> 
> 

## <a name="what-is-distributed-data"></a>Co je rozložení dat?
V SQL Data Warehouse a Parallel Data Warehouse distribuované data představují toouser data uložená na více místech napříč hello MPP systému. Každé z těchto umístění funguje jako nezávislé úložiště a výpočetní jednotky používající dotazy na jeho část dat hello. Rozložení dat je zásadní toorunning dotazy v paralelní tooachieve vysokého výkonu dotazu.

toodistribute dat datového skladu hello přiřadí každý řádek tabulky tooone distribuované umístění uživatele.  Můžete distribuovat tabulky s způsobu distribuce hash nebo metodu kruhového dotazování. Dobrý den distribution – metoda je zadaný v příkazu CREATE TABLE hello. 

## <a name="hash-distributed-tables"></a>Distribuovat algoritmu hash tabulky
Hello následující diagram znázorňuje, jak získá úplnou (tabulka distribuované) uložené jako tabulku distribuovat algoritmu hash. Deterministické funkce přiřadí distribuční tooone toobelong každý řádek. V definici tabulky hello některý ze sloupců hello je určený jako hello distribuční sloupce. Funkce hash Hello používá hodnotu hello v hello distribuční sloupce tooassign distribuční tooa každý řádek.

Existují důležité informace o výkonu pro výběr hello distribuční sloupce, například odlišnosti, zkosení dat a hello typy dotazů spustit v systému hello.

![Tabulka distribuované](media/sql-data-warehouse-distributed-data/hash-distributed-table.png "distribuované tabulky")  

* Každý řádek patří tooone distribuce.  
* Algoritmus hash deterministický přiřadí distribuční tooone každý řádek.  
* Hello počet řádků tabulky na distribuční se liší, jak je znázorněno pomocí různých velikostí hello tabulek.

## <a name="round-robin-distributed-tables"></a>Distribuované tabulky pomocí kruhového dotazování
Distribuované tabulku kruhového dotazování distribuuje hello řádky přiřazením každý řádek tooa distribuční sekvenční způsobem. Je rychlý tooload data do tabulky pomocí kruhového dotazování, ale může být pomalejší výkon dotazů.  Připojení pomocí kruhového dotazování tabulky obvykle vyžaduje promísení hello řádky tooenable hello dotazu tooproduce přesné výsledky, který přebírá čas.

## <a name="distributed-storage-locations-are-called-distributions"></a>Umístění distribuované úložiště se nazývají distribuce
Každé distribuované umístění se nazývá distribuce. Když dotazu běží paralelně, každý distribuční provede dotaz SQL na jeho část dat hello. SQL Data Warehouse používá databázi SQL toorun hello dotazu. Parallel Data Warehouse používá SQL Server toorun hello dotaz. Tento návrh architektury shared nothing je základní tooachieving škálování paralelní výpočty.

### <a name="can-i-view-hello-distributions"></a>Můžete zobrazit hello distribuce?
Každý distribuční má ID distribuce a je viditelný v zobrazeních systému, které se týkají tooSQL datového skladu a Parallel Data Warehouse. Můžete použít výkon dotazů hello distribuční ID tootroubleshoot a jiné problémy. Seznam hello systémová zobrazení, naleznete v části hello [MPP systémové zobrazení](sql-data-warehouse-reference-tsql-statements.md).

## <a name="difference-between-a-distribution-and-a-compute-node"></a>Rozdíl mezi distribučním a výpočetního uzlu
Distribuce je základní jednotkou hello pro ukládání dat v distribuované a zpracování paralelní dotazy. Distribuce se seskupují do výpočetních uzlů. Každý výpočetní uzel sleduje jeden nebo více distribuce.  

* Analytics Platform System používá jako součást centrální hello hardwaru architektura a škálování funkcí výpočetních uzlů. Vždy používá osm distribuce na výpočetním uzlu, jak je vidět v diagramu hello u tabulek se distribuovat algoritmu hash. Dobrý den počet výpočetních uzlů a proto hello počet distribuce, je dáno hello počet výpočetních uzlů, které jste si koupili pro hello zařízení. Například pokud si koupíte osm výpočetní uzly, získáte 64 distribuce (8 výpočetní uzly x 8 distribuce/uzel). 
* SQL Data Warehouse má pevný počet 60 distribuce a flexibilní počet výpočetních uzlů. Hello výpočetní uzly jsou implementovány pomocí Azure výpočetních a paměťových prostředků. Hello počet výpočetních uzlů můžete změnit podle zatížení služby back-end toohello a hello výpočetní kapacity (Dwu) zadejte pro hello datového skladu. Když se změní hello počet výpočetních uzlů, změní se také hello počet distribuce na výpočetním uzlu. 

### <a name="can-i-view-hello-compute-nodes"></a>Můžete zobrazit hello výpočetní uzly?
Každý výpočetní uzel obsahuje ID uzlu a je viditelný v zobrazeních systému, které se týkají tooSQL datového skladu a Parallel Data Warehouse.  Uvidíte hello výpočetního uzlu tak, že vyhledá hello node_id sloupec v systémových zobrazeních, jejichž názvy začínají řetězcem sys.pdw_nodes. Seznam hello systémová zobrazení, naleznete v části hello [MPP systémové zobrazení](sql-data-warehouse-reference-tsql-statements.md).

## <a name="Replicated"></a>Replikované tabulky
Obsahuje tabulku, která se replikují plně kopie hello tabulky uložené na každém výpočetním uzlu. Replikace tabulku odebere hello nutné tootransfer data mezi výpočetní uzly před spojení nebo agregace. Replikované tabulky lze provést pomocí malé tabulky pouze z důvodu hello úložiště navíc vyžaduje toostore hello úplné tabulky na každém výpočetním uzlu.  

Hello následující diagram znázorňuje replikované tabulky, který je uložený na každém výpočetním uzlu. Hello replikované tabulky pro SQL Data Warehouse je plně zkopírovaný tooa distribuční databázi na každém výpočetním uzlu. Parallel Data Warehouse replikované tabulce hello ukládají na všechny disky přiřazené toohello výpočetním uzlu.  Tato strategie disku je implementována pomocí skupin souborů systému SQL Server.  

![Replikované tabulky](media/sql-data-warehouse-distributed-data/replicated-table.png "replikované tabulky") 

## <a name="next-steps"></a>Další kroky
efektivní, najdete v části tabulky toouse distribuované [distribuci tabulek v SQL Data Warehouse](sql-data-warehouse-tables-distribute.md)  

