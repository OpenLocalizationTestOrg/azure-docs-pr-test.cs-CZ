---
title: aaaBest postupy pro Azure SQL Data Warehouse | Microsoft Docs
description: "Doporučení a osvědčené postupy, které byste měli znát, když budete vyvíjet řešení pro službu Azure SQL Data Warehouse. Pomohou vám stát se úspěšnými."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: 7b698cad-b152-4d33-97f5-5155dfa60f79
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 10/31/2016
ms.author: shigu;barbkess
ms.openlocfilehash: 1f42908fc03e1ca52278a6853d1afe9543d648b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-azure-sql-data-warehouse"></a>Osvědčené postupy pro službu Azure SQL Data Warehouse
Tento článek je kolekce mnoho osvědčených postupů, které vám pomůžou tooachieve optimální výkon z Azure SQL Data Warehouse.  Některé koncepty hello v tomto článku jsou základní a snadno tooexplain, jsou pokročilejší dalších konceptech a jsme právě pomocné hello prostor v tomto článku.  účelem Hello tohoto článku je toogive některé základní pokyny a tooraise povědomí o toofocus důležité oblasti, jako je vytváření datového skladu.  Každá část zavádí koncept tooa a pak bodu toomore podrobné články, které titulní hello koncept do větší hloubky.

Pokud se službou Azure SQL Data Warehouse teprve začínáte, nenechte se tímto článkem zahltit.  Hello pořadí témat hello je většinou v pořadí hello význam.  Pokud začnete se zaměříte na hello nejprve několika koncepty, budete v pořádku.  Až budete o službě SQL Data Warehouse vědět víc a budete si jistější, vraťte se a prozkoumejte pár dalších konceptů.  Nebude potřebovat dlouhé všem toomake smysl.

## <a name="reduce-cost-with-pause-and-scale"></a>Snižte náklady pomocí pozastavení a škálování
Klíčovou funkcí SQL Data Warehouse je hello možnost toopause když nepoužíváte, která zastaví hello fakturace výpočetních prostředků.  Další klíčových funkcí je hello možnost tooscale prostředky.  Pozastavení a škálování lze provést prostřednictvím hello portál Azure nebo prostřednictvím příkazy prostředí PowerShell.  Jak se může výrazně snížit náklady hello datového skladu, když není používán se seznámit s tyto funkce.  Pokud chcete, aby váš datový sklad dostupný, můžete tooconsider škálování dolů toohello nejmenší velikost, od DW100 než pozastavení.

Viz také [Pozastavení výpočetních prostředků][Pause compute resources], [Obnovení výpočetních prostředků][Resume compute resources], [Škálování výpočetních prostředků].

## <a name="drain-transactions-before-pausing-or-scaling"></a>Vypusťte transakce před pozastavením nebo škálováním
Při pozastavení nebo škálování SQL Data Warehouse, pozadí hello, které vaše dotazy došlo ke zrušení při zahájení hello pozastavení nebo žádost o škálování.  Zrušení jednoduchý dotaz SELECT je rychlý operace a obsahuje téměř žádné dopad toohello doba trvání toopause nebo škálovat vaše instance.  Transakční dotazy, které upravit strukturu dat nebo hello hello dat, ale nemusí být možné toostop rychle.  **Transakční dotazy se podle definice musí dokončit v celém rozsahu, nebo musí vrátit zpět provedené změny.**  Vrácení zpět dokončené transakční dotazu hello práci může trvat, protože dlouho nebo i déle, než původní změn hello hello dotazu byl použití.  Například pokud zrušíte dotazu, který byl odstranění řádků a již byla spuštěna pro jednu hodinu, může to trvat hello systému hodinu tooinsert back hello řádky, které byly odstraněny.  Pokud spustíte pozastavení nebo škálování při transakce jsou v cestě, vaše pozastavení nebo škálování se může zdát tootake dlouhou dobu, protože pozastavení a škálování má toowait pro vrácení zpět toocomplete hello mohli pokračovat.

Viz také [Vysvětlení transakcí][Understanding transactions], [Optimalizace transakcí][Optimizing transactions].

## <a name="maintain-statistics"></a>Udržujte statistiky
Na rozdíl od systému SQL Server, který automaticky detekuje a vytváří nebo aktualizuje statistiky nad sloupci, služba SQL Data Warehouse vyžaduje ruční údržbu statistik.  Při plánujeme toochange jsou optimalizované v hello budoucí, pro teď můžete toomaintain vaše tooensure statistiky, které hello plány SQL Data Warehouse.  Hello plány vytvořené hello Optimalizátor jsou pouze jako vhodné jako hello k dispozici statistiky.  **Vytváření jen Vzorkovaná statistiku pro každý sloupec je tooget snadno pracovat s statistiky.**  Je stejně důležité tooupdate statistiky jako významné změny dojít tooyour data.  Konzervativní přístup může být tooupdate statistice denně nebo po každé zatížení.  Jsou vždy kompromis mezi výkonem a hello náklady toocreate a aktualizaci statistik. Pokud zjistíte, že trvá příliš dlouho toomaintain všechny statistické údaje, můžete, třeba tootry toobe užší sloupce, které mají statistiky nebo sloupce, které často aktualizovat.  Například můžete tooupdate sloupců s kalendářními daty, kde mohou být nové hodnoty přidané, denní. **Využívat výhod hello bude získáte tak, že statistiky na sloupce použité ve spojení, sloupce použité v hello, kde najít klauzule a sloupce v GROUP BY.**

Viz také [Správa statistik tabulek][Manage table statistics], [CREATE STATISTICS][CREATE STATISTICS], [UPDATE STATISTICS][UPDATE STATISTICS].

## <a name="group-insert-statements-into-batches"></a>Seskupujte příkazy INSERT do dávek
Jednorázové zatížení tooa malé tabulku s příkazu INSERT nebo i pravidelné načtěte vyhledávání může provádět správně pro vaše potřeby příkazem `INSERT INTO MyLookup VALUES (1, 'Type 1')`.  Ale pokud budete potřebovat tooload tisíce nebo miliony řádků v rámci hello den, je možné, že singleton VLOŽÍ právě nemůže nadále.  Místo toho vytvořte procesů vašich tak, aby se soubor tooa zapsat a jiný proces pravidelně se dodává a načte tento soubor.

Viz také [INSERT][INSERT].

## <a name="use-polybase-tooload-and-export-data-quickly"></a>Rychle pomocí PolyBase tooload a export dat
SQL Data Warehouse podporuje načítání a export dat prostřednictvím různých nástrojů, včetně Azure Data Factory, PolyBase a BCP.  Pro malá množství dat, kde není výkon tak důležitý, by vám měl stačit libovolný nástroj.  Když jsou načítání nebo export velkých objemů dat, nebo je potřeba vysoký výkon, je PolyBase hello nejlepší volbou.  PolyBase je architektura MPP (Massively Parallel zpracování) hello navrženou tooleverage služby SQL Data Warehouse a budou proto načíst a export dat veličin rychleji než pomocí jakéhokoli jiného nástroje.  Úlohy funkce PolyBase můžete spustit pomocí příkazů CTAS nebo INSERT INTO.  **Pomocí funkce CTAS bude minimalizovat protokolování transakcí a hello nejrychlejší způsob, jak tooload vaše data.**  Azure Data Factory také podporuje úlohy funkce PolyBase.  PolyBase podporuje řadu formátů souborů, včetně souborů GZip.  **Při použití gzip textové soubory, soubory zalomení do 60 nebo více souborů toomaximize paralelismu zatížení toomaximize propustnost.**  Pro rychlejší celkovou propustnost zvažte souběžné načítání dat.

Viz také [Načtení dat][Load data], [Průvodce používáním funkce PolyBase][Guide for using PolyBase], [Vzory a strategie načítání služby Azure SQL Data Warehouse][Azure SQL Data Warehouse loading patterns and strategies], [Načtení dat pomocí služby Azure Data Factory][Load Data with Azure Data Factory], [Přesun dat pomocí služby Azure Data Factory][Move data with Azure Data Factory], [CREATE EXTERNAL FILE FORMAT][CREATE EXTERNAL FILE FORMAT], [CREATE TABLE AS SELECT (CTAS)][Create table as select (CTAS)].

## <a name="load-then-query-external-tables"></a>Načtěte a následně dotazujte externí tabulky
Polybase, také známé jako externí tabulky, mohou být hello nejrychlejší způsob, jak tooload data, není pro dotazy optimální. Tabulky PolyBase služby SQL Data Warehouse aktuálně podporují pouze soubory Azure blob. Tyto soubory nemají podporu v žádných výpočetních prostředcích.  SQL Data Warehouse v důsledku toho nelze přesměrování zpracování této pracovní a proto musí načíst celý soubor hello načtením tootempdb v pořadí tooread hello data.  Pokud máte několik dotazů, které bude dotazování na data, proto je lepší tooload tato data jednou a máte dotazy, použijte místní tabulky hello.

Viz také [Průvodce používáním funkce PolyBase][Guide for using PolyBase].

## <a name="hash-distribute-large-tables"></a>Distribuujte velké tabulky pomocí hodnot hash
Ve výchozím nastavení jsou tabulky distribuované metodou kruhového dotazování.  To usnadňuje uživatelům tooget zahájeno vytváření tabulek bez nutnosti toodecide jak by měly být distribuovány jejich tabulky.  Výkon tabulek kruhového dotazování může být pro některé úlohy dostatečný, ale ve většině případů bude lépe fungovat výběr distribučního sloupce.  je při tabulku distribuován podle sloupce daleko překonat kruhové dotazování tabulky Hello nejběžnější například v případě jsou připojené dva tabulky faktů velké.  Například pokud máte tabulku objednávky, který je distribuován podle order_id, a transakce tabulku, která je také distribuovat order_id, když se připojíte tabulky objednávek tabulky tooyour transakce na order_id, tento dotaz bude průchozí dotaz, což znamená jsme eliminovat operace přesunu dat.  Méně kroků znamená rychlejší dotaz.  Méně přesunů dat také přispívá ke zrychlení dotazů.  Tento prostor hello jenom škrábance vysvětlení. Při načítání distribuované tabulky, se ujistěte, že příchozích dat není seřazen na hello distribučního klíče, jak to bude zpomalit vaše zatížení.  Najdete v části níže hello odkazů pro většinu další podrobnosti o tom, jak výběrem distribuční sloupce může zlepšit výkon a toodefine distribuované tabulku v klauzuli WITH hello údajů vytvoření tabulky.

Viz také [Přehled tabulek][Table overview], [Distribuce tabulky][Table distribution], [Výběr distribuce tabulky][Selecting table distribution], [CREATE TABLE][CREATE TABLE], [CREATE TABLE AS SELECT][CREATE TABLE AS SELECT].

## <a name="do-not-over-partition"></a>Nevytvářejte zbytečně moc oddílů
Přestože dělení dat může být velice efektivní pro udržování dat prostřednictvím přepínání oddílů nebo optimalizace prohledávání pomocí eliminace oddílů, příliš mnoho oddílů může zpomalit vaše dotazy.  Strategie vysoce členitého dělení, která může dobře fungovat v systému SQL Server, často nemusí fungovat dobře ve službě SQL Data Warehouse.  S příliš mnoha oddílů může také snížit hello účinnosti Clusterované indexy columnstore, pokud každý oddíl má méně než 1 milionu řádků.  Mějte na paměti, že pozadí hello, SQL Data Warehouse oddíly vaše data můžete do 60 databáze, takže pokud vytvoříte tabulku s 100 oddíly, ve skutečnosti výsledkem 6000 oddíly.  Každé zatížení je jiný hello nejlepší Rady, jak je tooexperiment s dělení toosee poznatků o velikosti pracovní zátěže.  Zvažte použití nižší členitosti, než jaká by pro vás byla vhodná v systému SQL Server.  Například místo denního dělení zvažte použití týdenního nebo měsíčního dělení.

Viz také [Dělení tabulky][Table partitioning].

## <a name="minimize-transaction-sizes"></a>Minimalizujte velikosti transakcí
Příkazy INSERT, UPDATE a DELETE se spouštějí v rámci transakce, a když selžou, musí se transakce odvolat.  toominimize hello potenciální dlouho vrácení, minimalizujte velikost transakce, kdykoli je to možné.  Můžete to provést rozdělením příkazů INSERT, UPDATE a DELETE na části.  Například pokud máte typu vložení, které očekáváte, že tootake 1 hod., pokud je to možné, rozdělte hello vložení do 4 částí, které bude každá běží za 15 minut.  Využít zvláštních případech minimální protokolování, jako jsou funkce CTAS, TRUNCATE, VYŘAĎTE tabulku nebo tabulky tooempty vložení, riziko tooreduce vrácení zpět.  Odvolání tooeliminate jiný způsob, jak je toouse Metadata pouze operace, jako je pro správu dat na přepnutí oddílu.  Například spíš než spustit příkaz toodelete odstranit všechny řádky v tabulce, kde byla hello order_date v říjnu 2001, můžete každý měsíc oddílu vaše data a pak přejděte na oddíl hello s daty pro prázdný oddíl z jiné tabulky (viz ALTER Příklady tabulky).  Pomocí funkce CTAS toowrite hello dat. Chcete tookeep v tabulce, místo používání příkazu DELETE použít pro bez oddílů tabulky.  Pokud funkce CTAS trvá hello stejné množství času, protože je mnohem bezpečnější toorun operace, jako má velmi minimální transakce protokolování a zrušit lze rychle v případě potřeby.

Viz také [Vysvětlení transakcí][Understanding transactions], [Optimalizace transakcí][Optimizing transactions], [Dělení tabulky][Table partitioning], [TRUNCATE TABLE][TRUNCATE TABLE], [ALTER TABLE][ALTER TABLE], [CREATE TABLE AS SELECT (CTAS)][Create table as select (CTAS)].

## <a name="use-hello-smallest-possible-column-size"></a>Použít nejmenší velikost možné sloupce hello
Při definování DDL, bude pomocí hello nejmenší datový typ, který bude podporovat vaše data zlepšit výkon dotazu.  To je obzvlášť důležité pro sloupce typu CHAR a VARCHAR.  Pokud hello nejdelší hodnotu ve sloupci 25 znaků, definujte vaše sloupec jako VARCHAR(25).  Nedefinujte všechny sloupce tooa velké výchozí znaků.  Kromě toho sloupce definujte jako VARCHAR, pokud tento typ splňuje všechny požadavky, místo používání NVARCHAR.

Viz také [Přehled tabulek][Table overview], [Typy tabulkových dat][Table data types], [CREATE TABLE][CREATE TABLE].

## <a name="use-temporary-heap-tables-for-transient-data"></a>Použijte dočasné tabulky hald pro přechodná data
Pokud data jsou dočasně cílová v SQL Data Warehouse, můžete zjistit, které pomocí tabulky haldy provede hello celý proces rychlejší.  Při zavádění toostage pouze data před spuštěním další transformace, načítání tooheap tabulku hello je mnohem rychlejší než načítání dat tooa hello Clusterované tabulky columnstore.  Kromě toho načítání dat tooa dočasnou tabulku také načte mnohem rychleji než načítání úložiště toopermanent tabulky.  Dočasné tabulky začínat "#" a jsou přístupné jenom podle hello relace, který vytvoří, tak může fungovat pouze v omezené scénáře.   Halda tabulky jsou definovány v klauzuli WITH hello CREATE TABLE.  Pokud používáte dočasné tabulky, nezapomeňte příliš toocreate statistiky na dočasné tabulky.

Viz také [Dočasné tabulky][Temporary tables], [CREATE TABLE][CREATE TABLE], [CREATE TABLE AS SELECT][CREATE TABLE AS SELECT].

## <a name="optimize-clustered-columnstore-tables"></a>Optimalizujte clusterované tabulky columnstore
Clusterované indexy columnstore jsou jedním ze způsobů nejúčinnější hello můžete ukládat data v Azure SQL Data Warehouse.  Vy výchozím nastavení se tabulky ve službě SQL Data Warehouse vytváří jako clusterované columnstore.  je důležité tooget hello nejlepší výkon pro dotazy na tabulky columnstore, s funkčním segment kvality.  Když řádky jsou zapsány toocolumnstore tabulky přetížena paměť, může dojít ke snížení kvality columnstore segmentu.  Kvalitu segmentů lze změřit podle počtu řádků v komprimované skupině řádků.  Najdete v části hello [příčiny nízký columnstore index kvality] [ Causes of poor columnstore index quality] v hello [tabulky indexy] [ Table indexes] článek na pokyny krok za krokem zjišťování a zlepšování kvality segmentu pro Clusterované tabulky columnstore.  Vysoké kvality columnstore segmenty je důležité, a proto je vhodné uživatelé ID toouse, které jsou ve třídě hello střední a velké prostředků pro načítání dat.  Hello méně Dwu, které používáte, hello větší hello Třída prostředků můžete tooassign tooyour načítání uživatele.

Vzhledem k tomu, že tabulky columnstore obecně nebude nabízet data do segment komprimované columnstore dokud existuje více než 1 milionu řádků každou tabulku a každý SQL Data Warehouse tabulka je rozdělena na oddíly do 60 tabulek jako existuje pravidlo, nebude mít užitek tabulky columnstore dotazu Pokud hello tabulka obsahuje více než 60 milionu řádků.  Pro tabulku s méně než 60 milionu řádků nemusí mít žádné smysl toohave columnstore index.  Ale také to nemusí vadit.  Kromě toho pokud jste oddílu dat, pak budete chtít tooconsider, že každý oddíl bude potřebovat toobenefit toohave 1 milionu řádků z clusterovaný index columnstore.  Pokud tabulka má oddíly 100, pak bude je nutné toobenefit toohave alespoň 6 miliardy řádků z úložiště clusteru sloupců (60 distribuce * 100 oddíly * 1 milionu řádků).  Pokud vaše tabulka nemá 6 miliardy řádků v tomto příkladu, snižte počet hello oddílů nebo zvažte použití haldy tabulku místo.  Také může být vhodné experimentování toosee Pokud lepšího výkonu můžete získávají haldy tabulku s sekundární indexy než tabulky columnstore.  Tabulky columnstore zatím ještě nepodporují sekundární indexy.

Při dotazování tabulky columnstore, dotazy se spustí rychleji, pokud vyberete jenom hello sloupce, které potřebujete.  

Viz také [Indexy tabulky][Table indexes], [Průvodce indexy columnstore][Columnstore indexes guide], [Obnovení indexů columnstore][Rebuilding columnstore indexes].

## <a name="use-larger-resource-class-tooimprove-query-performance"></a>Použít větší výkon dotazů tooimprove prostředků – třída
SQL Data Warehouse používá jako tooqueries paměti způsob tooallocate skupiny prostředků.  Předinstalované hello všichni uživatelé jsou přiřazeni třída toohello malé prostředků, která uděluje 100 MB paměti na jeden distribuční.  Vzhledem k tomu, že jsou vždy 60 distribuce a každý distribuční je zadána minimálně 100 MB, celková paměť pro systém širokou hello přidělení je 6 000 MB, nebo jenom pod 6 GB.  Některé dotazy, například velké spojení nebo tabulky columnstore tooclustered zatížení, budou využívat větší přidělení paměti.  U některých dotazů, jako jsou třeba pouhá prohledávání, se výhody neprojeví.  Na straně překlopit hello využitím větší třídy prostředků má dopad na souběžnosti, proto je vhodné tootake to v úvahu před přesunutím všechny třídě uživatelů tooa velké prostředků.

Viz také [Souběžnost a správa úloh][Concurrency and workload management].

## <a name="use-smaller-resource-class-tooincrease-concurrency"></a>Použít menší Třída prostředků tooIncrease souběžnosti
Pokud jsou vašeho povšimnutí, uživatelských dotazů pravděpodobně toohave dlouhém zpoždění, které může být vaši uživatelé jsou spuštěny v větší třídy prostředků a spotřebovávají velké množství souběžnosti sloty způsobuje další dotazy tooqueue nahoru.  toosee, pokud uživatelé dotazy jsou zařazeny do fronty, spusťte `SELECT * FROM sys.dm_pdw_waits` toosee, pokud jsou vráceny všechny řádky.

Viz také [Souběžnost a správa úloh][Concurrency and workload management], [sys.dm_pdw_waits][sys.dm_pdw_waits].

## <a name="use-dmvs-toomonitor-and-optimize-your-queries"></a>Pomocí zobrazení dynamické správy toomonitor a optimalizovat dotazy
SQL Data Warehouse má několik zobrazení dynamické správy, které můžou být použité toomonitor při provádění dotazu.  Hello monitorování následujícího článku provede podrobné pokyny o tom, jak toolook na podrobnosti hello provedení dotazu.  dotazy tooquickly najít v těchto zobrazení dynamické správy, pomocí možnosti popisek hello se vaše dotazy může pomoct.

Viz také [Monitorování úloh pomocí zobrazení dynamických zpráv][Monitor your workload using DMVs], [LABEL][LABEL], [OPTION][OPTION], [sys.dm_exec_sessions][sys.dm_exec_sessions], [sys.dm_pdw_exec_requests][sys.dm_pdw_exec_requests], [sys.dm_pdw_request_steps][sys.dm_pdw_request_steps], [sys.dm_pdw_sql_requests][sys.dm_pdw_sql_requests], [sys.dm_pdw_dms_workers], [DBCC PDW_SHOWEXECUTIONPLAN][DBCC PDW_SHOWEXECUTIONPLAN], [sys.dm_pdw_waits][sys.dm_pdw_waits]

## <a name="other-resources"></a>Další prostředky
Přečtěte si také článek [Řešení potíží][Troubleshooting], ve kterém najdete běžné problémy a jejich řešení.

Pokud nebyl nalezen, co jste hledali v tomto článku, zkuste použít hello "Hledání pro dokumenty" na levé straně hello tuto stránku toosearch všech dokumentů Azure SQL Data Warehouse hello.  Hello [fórum MSDN Azure SQL Data Warehouse] [ Azure SQL Data Warehouse MSDN Forum] byla vytvoření jako místo pro otázky, které jste tooask tooother uživatelů a toohello skupinu produktu pro SQL datového skladu.  Toto fórum tooensure, že vaše otázky jsou zodpovězeny jiným uživatelem nebo jeden z nám aktivně sledujeme.  Pokud dáváte přednost tooask vaše otázky na Stack Overflow, máme také [SQL Data Warehouse Stack Overflow fóru Azure][Azure SQL Data Warehouse Stack Overflow Forum].

Nakonec použijte hello [Azure SQL Data Warehouse zpětné vazby] [ Azure SQL Data Warehouse Feedback] stránka toomake žádosti o funkce.  Přidáním vlastních žádostí nebo hlasováním pro ostatní žádosti nám pomůžete určit prioritu funkcí.

<!--Image references-->

<!--Article references-->
[Create a support ticket]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Concurrency and workload management]: ./sql-data-warehouse-develop-concurrency.md
[Create table as select (CTAS)]: ./sql-data-warehouse-develop-ctas.md
[Table overview]: ./sql-data-warehouse-tables-overview.md
[Table data types]: ./sql-data-warehouse-tables-data-types.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Table indexes]: ./sql-data-warehouse-tables-index.md
[Causes of poor columnstore index quality]: ./sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Rebuilding columnstore indexes]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Table partitioning]: ./sql-data-warehouse-tables-partition.md
[Manage table statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary tables]: ./sql-data-warehouse-tables-temporary.md
[Guide for using PolyBase]: ./sql-data-warehouse-load-polybase-guide.md
[Load data]: ./sql-data-warehouse-overview-load.md
[Move data with Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[Load data with Azure Data Factory]: ./sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Load data with PolyBase]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Monitor your workload using DMVs]: ./sql-data-warehouse-manage-monitor.md
[Pause compute resources]: ./sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Resume compute resources]: ./sql-data-warehouse-manage-compute-overview.md#resume-compute-bk
[Škálování výpočetních prostředků]: ./sql-data-warehouse-manage-compute-overview.md#scale-compute
[Understanding transactions]: ./sql-data-warehouse-develop-transactions.md
[Optimizing transactions]: ./sql-data-warehouse-develop-best-practices-transactions.md
[Troubleshooting]: ./sql-data-warehouse-troubleshoot.md
[LABEL]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->
[ALTER TABLE]: https://msdn.microsoft.com/library/ms190273.aspx
[CREATE EXTERNAL FILE FORMAT]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE STATISTICS]: https://msdn.microsoft.com/library/ms188038.aspx
[CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx
[CREATE TABLE AS SELECT]: https://msdn.microsoft.com/library/mt204041.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: https://msdn.microsoft.com/library/mt204017.aspx
[INSERT]: https://msdn.microsoft.com/library/ms174335.aspx
[OPTION]: https://msdn.microsoft.com/library/ms190322.aspx
[TRUNCATE TABLE]: https://msdn.microsoft.com/library/ms177570.aspx
[UPDATE STATISTICS]: https://msdn.microsoft.com/library/ms187348.aspx
[sys.dm_exec_sessions]: https://msdn.microsoft.com/library/ms176013.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_request_steps]: https://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: https://msdn.microsoft.com/library/mt203889.aspx
[sys.dm_pdw_dms_workers]: https://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_waits]: https://msdn.microsoft.com/library/mt203893.aspx
[Columnstore indexes guide]: https://msdn.microsoft.com/library/gg492088.aspx

<!--Other Web references-->
[Selecting table distribution]: https://blogs.msdn.microsoft.com/sqlcat/2015/08/11/choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/
[Azure SQL Data Warehouse Feedback]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Azure SQL Data Warehouse MSDN Forum]: https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=AzureSQLDataWarehouse
[Azure SQL Data Warehouse Stack Overflow Forum]:  http://stackoverflow.com/questions/tagged/azure-sqldw
[Azure SQL Data Warehouse loading patterns and strategies]: https://blogs.msdn.microsoft.com/sqlcat/2016/02/06/azure-sql-data-warehouse-loading-patterns-and-strategies
