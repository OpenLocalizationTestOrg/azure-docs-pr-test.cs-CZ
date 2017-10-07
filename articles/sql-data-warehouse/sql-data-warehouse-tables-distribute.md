---
title: aaaDistributing tabulek v SQL Data Warehouse | Microsoft Docs
description: "Začínáme s distribuci tabulek v Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: barbkess
editor: 
ms.assetid: 5ed4337f-7262-4ef6-8fd6-1809ce9634fc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 10/31/2016
ms.author: shigu;barbkess
ms.openlocfilehash: 65093eeaeb00fef85aaa6070da2c976fed3f4bbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="distributing-tables-in-sql-data-warehouse"></a>Distribuce tabulek v SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Přehled][Overview]
> * [Datové typy][Data Types]
> * [Distribuce][Distribute]
> * [Index][Index]
> * [Oddíl][Partition]
> * [Statistiky][Statistics]
> * [Dočasné][Temporary]
>
>

Řešení SQL Data Warehouse je distribuovaný databázový systém, postavený na architektuře MPP (Massively Parallel Processing).  SQL Data Warehouse nabízí jedinečnou škálovatelnost, daleko za možnostmi jediného systému, a to díky dělení dat a schopnosti zpracování napříč více uzly.  Při rozhodování o tom, jak toodistribute svá data do SQL Data Warehouse je jedním z nejdůležitějších hello faktory tooachieving optimální výkon.   Hello klíče toooptimal výkonu je minimalizaci přesun dat a naopak je přesun dat klíče toominimizing hello Výběr strategie správné distribuční hello.

## <a name="understanding-data-movement"></a>Princip přesunu dat
V systému MPP hello data z tabulek rozděluje do několika základní databáze.  Hello nejvíce optimalizované dotazy v systému MPP lze předat jednoduše prostřednictvím tooexecute na hello jednotlivé distribuované databáze bez jakéhokoli zásahu mezi hello jiné databáze.  Řekněme například, že máte databázi s prodejní data, která obsahuje dvě tabulky, prodej a zákazníků.  Pokud máte dotaz, který potřebuje toojoin vašeho zákazníka tabulku tooyour prodeje a budete provádět dělení prodeje a tabulky zákazníka až po zákaznické číslo uvedení každého zákazníka a to v samostatné databáze, bude možné v každém vyřešit všechny dotazy, které připojení prodeje a zákazníka databáze s žádná znalost hello jiné databáze.  Naproti tomu Pokud vaše data prodeje dělený pořadové číslo a zákaznických údajů číslem zákazníka, pak všechny danou databázi nebude mít hello odpovídajících dat pro každého zákazníka a proto pokud jste chtěli toojoin zákaznických údajů tooyour data prodeje, potřebovali byste tooget hello dat pro každého zákazníka a to z hello jiné databáze.  V tomto příkladu druhý přesun dat potřebovat toooccur toomove hello data toohello prodejní data zákazníků, takže hello dvou tabulek je možné připojit.  

Přesun dat není vždy chybný věcí, někdy je nezbytné toosolve dotazu.  Ale když tento další krok se vyhnout, přirozeně dotazu bude pracovat rychleji.  Přesun dat nejčastěji mohou nastat, pokud jsou připojené k tabulky nebo agregace se provádí.  Často musíte toodo obě, takže když bude pravděpodobně možné toooptimize pro jeden scénář, jako je připojení, je stále nutné toohelp přesun dat, které řešit pro hello další scénáře, jako je agregace.  Hello efektu je přijít na to, což je méně práce.  Ve většině případů distribuci tabulky faktů velký na běžně připojené k sloupci je hello co nejúčinnější metodu pro snížení hello většina přesun dat.  Distribuci dat na sloupce spojení je přesun dat tooreduce metoda mnohem častější než distribuci dat na sloupce, které se účastní agregace.

## <a name="select-distribution-method"></a>Vyberte možnost distribution – metoda
SQL Data Warehouse pozadí hello, rozděluje data do 60 databáze.  Každé jednotlivé databáze je odkazované tooas **distribuční**.  Při načítání dat do tabulek SQL Data Warehouse má tooknow jak toodivide vaše data v těchto 60 distribuce.  

Dobrý den distribution – metoda definovanou na úrovni tabulky hello a aktuálně existují dvě možnosti:

1. **Kruhové dotazování** který distribuci dat rovnoměrně ale náhodně.
2. **Hodnoty hash distribuované** která distribuuje data podle algoritmu hash hodnoty z jednoho sloupce

Ve výchozím nastavení, když nejsou definovány způsobu distribuce dat, tabulka budou distribuována pomocí hello **kruhové dotazování** na metodu.  Ale, až se sofistikovanější v implementaci, můžete pomocí tooconsider **hash distribuované** tabulky toominimize přesun dat, která bude zase optimalizovat výkon dotazů.

### <a name="round-robin-tables"></a>Kruhové dotazování tabulky
Použití hello kruhové dotazování metoda distribuci dat je velmi dobře v tom, jak ho vyznívá.  Při načtení dat je každý řádek jednoduše odeslána toohello další distribuční.  Tato metoda distribuci dat hello bude vždy náhodně distribuci dat hello velmi rovnoměrně napříč všemi hello distribuce.  To znamená, že neexistuje žádné řazení done během hello round robin procesu, který uloží data.  Kruhové dotazování distribuční se někdy nazývá náhodných hash z tohoto důvodu.  S tabulkou distribuované kruhového dotazování nejsou žádná data hello toounderstand potřeba.  Z tohoto důvodu kruhového dotazování tabulky často je dobré načítání cíle.

Ve výchozím nastavení pokud je vybrána žádná metoda distribuce, hello kruhové dotazování distribuční metoda se použije.  Kruhové dotazování tabulky jsou snadno toouse, protože data se náhodně distribuuje do systému hello znamená to, že hello systém nemůže zaručit, které distribuční každý řádek je však na.  Výsledek, někdy systému hello stačit tooinvoke toobetter operace přesunu dat organizování vašich dat před překlad dotazu.  Tento krok navíc může zpomalit své dotazy.

Zvažte použití kruhové dotazování distribuce pro tabulku v hello následující scénáře:

* Pro začátek jednoduché počáteční bod
* Pokud neexistuje zřejmé spojující klíč
* Pokud není k dispozici sloupec vhodným kandidátem pro distribuci hello tabulku hash
* Pokud hello tabulky není sdílet společný klíč spojení s jinými tabulkami
* Pokud je méně důležité než jiné spojení v dotazu hello hello spojení
* Pokud je tabulka hello dočasné pracovní tabulky

Obě tyto příklady vytvoří tabulku, kruhové dotazování:

```SQL
-- Round Robin created by default
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
;

-- Explicitly Created Round Robin Table
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = ROUND_ROBIN
)
;
```

> [!NOTE]
> Pokud kruhové dotazování je, že typ tabulky výchozí hello probíhá explicitní v DDL je považovat za osvědčený postup, aby byly zrušte tooothers záměry hello rozložení tabulky.
>
>

### <a name="hash-distributed-tables"></a>Hodnoty hash distribuované tabulky
Použití **Hash distribuované** algoritmus toodistribute vaše tabulky můžete zlepšit výkon pro mnoho scénářů snížením přesun dat v době dotazu.  Hodnota hash, který distribuované tabulky jsou tabulky, které jsou rozděleny mezi hello distribuované databáze pomocí algoritmu hash na jeden sloupec, který jste vybrali.  Hello distribuční sloupce je určen jak rozděluje hello data do distribuované databáze.  Funkce hash Hello používá hello distribuční sloupce tooassign řádky toodistributions.  Hello algoritmu hash a výsledný distribuce je deterministický.  To znamená hello toohello má stejnou hodnotu s hello stejný datový typ. bude vždy stejné distribuce.    

Tento příklad vytvoří tabulku rozdělit na id:

```SQL
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,  DISTRIBUTION = HASH([ProductKey])
)
;
```

## <a name="select-distribution-column"></a>Vybrat distribuční sloupce
Pokud vyberete příliš**distribuovat algoritmu hash** tabulku, budete potřebovat tooselect jednoho distribučního sloupec.  Když vyberete distribuční sloupce, existují tři hlavní faktory tooconsider.  

Vyberte jeden sloupec, který bude:

1. Není možné aktualizovat
2. Distribuci dat rovnoměrně, zabraňující zkosení dat
3. Minimalizovat přesun dat

### <a name="select-distribution-column-which-will-not-be-updated"></a>Vybrat distribuční sloupec, který nebude aktualizován
Distribuční sloupce nejsou aktualizovat, proto, vyberte sloupec s statické hodnoty.  Pokud sloupec bude nutné aktualizovat toobe, není obecně správné distribuční candidate.  Pokud je případ, kde je nutné aktualizovat distribuční sloupce, můžete to provést nejdřív odstranit řádek hello a následného vložení nového řádku.

### <a name="select-distribution-column-which-will-distribute-data-evenly"></a>Vybrat distribuční sloupec, který bude rovnoměrně distribuovat data
Vzhledem k tomu, že distribuovaného systému provádí pouze rychlé jako jeho nejpomalejší distribuci, je důležité toodivide hello pracovní rovnoměrně mezi hello distribuce v pořadí spouštění tooachieve vyrovnáváním napříč hello systému.  způsob Hello hello práce je rozdělena na distribuovaného systému je založena na tam, kde hello dat pro každý distribuci platný.  Díky tomu je velmi důležité tooselect hello správné distribuční sloupce pro distribuci dat hello tak, aby každý distribuční má stejnou práci a bude trvat hello stejný čas toocomplete jeho část práce hello.  Při práci a rozděluje do hello systému, dat hello vyvážená hello distribuce.  Když data není vyrovnáváním rovnoměrně, říkáme to **data zkosení**.  

toodivide data rovnoměrně a vyhnout se data zkosení, zvažte následující hello při výběru vaší distribuční sloupce:

1. Vyberte sloupce, který obsahuje velký počet jedinečných hodnot.
2. Vyhněte se distribuci dat na sloupce s několika jedinečných hodnot.
3. Vyhněte se distribuci dat na sloupce s vysoká frekvence hodnot Null.
4. Vyhněte se distribuci dat na sloupců s kalendářními daty.

Protože každá hodnota hash too1 60 distribuce, tooachieve rovnoměrné rozdělení můžete tooselect sloupec, který je vysoce jedinečný a obsahuje více než 60 jedinečné hodnoty.  tooillustrate, představte si případ, kdy sloupec má jenom 40 jedinečné hodnoty.  Pokud v tomto sloupci jste vybrali jako hello distribučního klíče, hello data pro tuto tabulku by nebude zobrazovat na 40 distribuce maximálně ponechat 20 distribuce se žádná data a žádné toodo zpracování.  Naopak hello dalších 40 distribuce by měla mít více pracovní toodo, že pokud hello byla data rovnoměrně šířit přes 60 distribuce.  Tento scénář je příkladem zkosení data.

V systému MPP každého kroku dotazu čeká na všechny toocomplete distribuce jejich sdílení práce hello.  Pokud do jednoho distribučního je to více práce, než ostatní hello, hello prostředků hello jiných distribuce jsou v podstatě ke znehodnocení části právě čekání na zaneprázdněn distribuční hello.  Při práci není rovnoměrně rozloženy všechny distribuce, říkáme to **zpracování zkosení**.  Zpracování zkosení způsobí, že dotazy toorun nižší než pokud hello zatížení může být rovnoměrně rozloženy hello distribuce.  Data zkosení povede tooprocessing zkosení.

Nedávejte na vysoce sloupec jako hello hodnoty null budou všechny zobrazovat hello stejné distribuce. Distribuce ve sloupci Datum může také způsobit zpracování zkosení vzhledem k tomu, že všechna data pro konkrétního data budou zobrazovat hello stejné distribuce. Pokud několik uživatelů provádí dotazy, které jsou všechny filtrování na hello stejné datum, pak pouze 1 hello 60 distribuce bude by všechny hello práce od konkrétního data budou pouze na jednom distribučním. V tomto scénáři pravděpodobně hello dotazy se spustí 60 x pomalejší než pokud hello dat byly rovnoměrně rozloženy všechny hello distribuce.

Pokud neexistují žádné sloupce vhodným kandidátem, můžete použít kruhové dotazování jako metodu distribuce hello.

### <a name="select-distribution-column-which-will-minimize-data-movement"></a>Vybrat distribuční sloupec, který bude minimalizovat přesun dat
Minimalizace přesun dat zvolením hello správné distribuční sloupce je jedním z hello nejdůležitější strategie pro optimalizaci výkonu SQL Data Warehouse.  Přesun dat nejčastěji mohou nastat, pokud jsou připojené k tabulky nebo agregace se provádí.  Sloupce použité v `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` a `HAVING` klauzule pro všechny provedl **dobrý** hash kandidáty distribuce.

Na druhé straně sloupců v hello hello `WHERE` klauzule proveďte **není** provést pro sloupec kandidátů na dobrý hash vzhledem k tomu, že omezit, které distribuce účastnit hello dotazu, která způsobila zpracování zkosení.  Dobrým příkladem sloupce, který může být tempting toodistribute na, ale často může způsobit zpracování zkosení je sloupec datum.

Obecně platí Pokud máte dvě tabulky faktů velké často zahrnutých ve spojení, bude získáte hello většina výkonu distribucí obě tabulky v jednom ze sloupce spojení hello.  Pokud máte tabulku, která je nikdy tabulky faktů velké připojené k tooanother, pak hledejte toocolumns, které jsou často v hello `GROUP BY` klauzule.

Existuje několik klíčů kritérií, které musí být nesplnění tooavoid přesun dat během spojení:

1. Hello tabulky zahrnutých v hello připojení musí být hodnota hash rozdělit na **jeden** hello sloupců podílejících se na hello spojení.
2. Hello datové typy sloupce spojení hello musí odpovídat mezi obě tabulky.
3. Hello sloupce musí být připojené pomocí operátoru rovná se.
4. Hello typ spojení nesmí být `CROSS JOIN`.

## <a name="troubleshooting-data-skew"></a>Řešení potíží s zkosení dat
Při distribuci dat tabulky pomocí metody distribuce hash hello existuje šance, že bude některých distribucích nesouměrně rozdělí toohave nepřiměřeně více dat než jiné. Nadměrné data zkosení může ovlivnit výkon dotazů, protože hello konečný výsledek distribuovaného dotazu musí čekat na toofinish distribuční nejdelší spuštěné hello. V závislosti na hello stupeň hello data zkosení, že může být nutné tooaddress ho.

### <a name="identifying-skew"></a>Identifikace zkosení
Jednoduchý způsob tooidentify tabulku jako nesouměrně rozdělí je toouse `DBCC PDW_SHOWSPACEUSED`.  To je velmi rychlý a jednoduchý způsob toosee hello počet řádků tabulky, které jsou uložené v každé z distribuce hello 60 vaší databáze.  Mějte na paměti, že hello nejvíce vyrovnáváním výkonu hello řádky v tabulce distribuované by měl být rovnoměrně rozloženy všechny hello distribuce.

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Pokud dotaz zobrazení dynamické správy Azure SQL Data Warehouse hello (DMV) však můžete provádět více podrobné analýzy.  toostart, vytvořit zobrazení hello [dbo.vTableSizes] [ dbo.vTableSizes] zobrazit pomocí hello SQL z [tabulky přehled] [ Overview] článku.  Po vytvoření hello zobrazení, spusťte tento dotaz tooidentify, které tabulky mít víc než 10 % data zkosení.

```sql
select *
from dbo.vTableSizes
where two_part_name in
    (
    select two_part_name
    from dbo.vTableSizes
    where row_count > 0
    group by two_part_name
    having min(row_count * 1.000)/max(row_count * 1.000) > .10
    )
order by two_part_name, row_count
;
```

### <a name="resolving-data-skew"></a>Řešení zkosení dat
Všechny zkosení je dostatek toowarrant oprava.  V některých případech může výkon hello tabulky v některé dotazy převáží nad hello škodu dat zkosení.  Pokud byste měli vyřešit dat toodecide zkreslit v tabulce, byste měli porozumět v vaše úlohy co nejvíce o hello datové svazky a dotazy.   Jedním ze způsobů toolook v hello dopad zkosení je toouse hello kroky v hello [dotazu monitorování] [ Query Monitoring] článek toomonitor hello dopad na výkon dotazů zkosení a konkrétně hello dopad toohow dlouhé dotazy toocomplete převezmou jednotlivé distribuce hello.

Distribuci dat je řádu hello rovnováhu mezi minimalizaci zkosení dat a současně minimalizujete její přesun dat hledání. To může být proti cíle a někdy budete chtít tookeep data zkosení v přesunu dat tooreduce pořadí. Například když hello distribuční sloupec je často hello sdílené v spojování a agregaci, můžete se se současně minimalizujete její přesun dat. Hello Výhodou vytvoření přesun dat minimální hello vyváží hello dopad toho, že data zkreslit.

obvyklým způsobem Hello tooresolve data zkosení je toore-vytvořit hello tabulku se sloupcem jiný distribuční. Vzhledem k tomu, že neexistuje žádný způsob toochange hello distribuční sloupce na existující tabulky, hello způsob toochange hello distribuční tabulky ho toorecreate její [] [funkce CTAS].  Zde jsou dva příklady, jak vyřešit data zkosení:

### <a name="example-1-re-create-hello-table-with-a-new-distribution-column"></a>Příklad 1: Vytvořte znovu hello tabulky se sloupcem nový distribuční
Tento příklad používá toore [] – [funkce CTAS]-vytvoří tabulku se sloupcem distribuční různé hodnoty hash.

```sql
CREATE TABLE [dbo].[FactInternetSales_CustomerKey]
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  HASH([CustomerKey])
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_CustomerKey')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_CustomerKey] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_CustomerKey] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_CustomerKey] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_CustomerKey] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_CustomerKey] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_CustomerKey] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_CustomerKey] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_CustomerKey] ([SalesAmount]);

--Rename hello tables
RENAME OBJECT [dbo].[FactInternetSales] too[FactInternetSales_ProductKey];
RENAME OBJECT [dbo].[FactInternetSales_CustomerKey] too[FactInternetSales];
```

### <a name="example-2-re-create-hello-table-using-round-robin-distribution"></a>Příklad 2: Znovu vytvořit tabulku hello použití distribučních bodů kruhové dotazování
Tento příklad používá toore [] – [funkce CTAS]-vytvořte si tabulku s každým místo hash distribuce. Tato změna způsobí distribuce i data hello náklady na přesun dat vyšší.

```sql
CREATE TABLE [dbo].[FactInternetSales_ROUND_ROBIN]
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  ROUND_ROBIN
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_ROUND_ROBIN')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_ROUND_ROBIN] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_ROUND_ROBIN] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_ROUND_ROBIN] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_ROUND_ROBIN] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_ROUND_ROBIN] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_ROUND_ROBIN] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_ROUND_ROBIN] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_ROUND_ROBIN] ([SalesAmount]);

--Rename hello tables
RENAME OBJECT [dbo].[FactInternetSales] too[FactInternetSales_HASH];
RENAME OBJECT [dbo].[FactInternetSales_ROUND_ROBIN] too[FactInternetSales];
```

## <a name="next-steps"></a>Další kroky
toolearn Další informace o návrh tabulky, najdete v části hello [distribuovat][Distribute], [Index][Index], [oddílu] [ Partition], [Datové typy][Data Types], [statistiky] [ Statistics] a [dočasných tabulek] [ Temporary] články.

Přehled osvědčených postupů najdete v tématu [SQL Data Warehouse osvědčené postupy][SQL Data Warehouse Best Practices].

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
[Query Monitoring]: ./sql-data-warehouse-manage-monitor.md
[dbo.vTableSizes]: ./sql-data-warehouse-tables-overview.md#table-size-queries

<!--MSDN references-->
[DBCC PDW_SHOWSPACEUSED()]: https://msdn.microsoft.com/library/mt204028.aspx

<!--Other Web references-->
