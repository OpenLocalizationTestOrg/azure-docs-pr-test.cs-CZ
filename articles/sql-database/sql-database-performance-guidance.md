---
title: "ladění pokyny výkonu databáze SQL aaaAzure | Microsoft Docs"
description: "Tento článek vám může pomoct určit, které toochoose vrstvy služby pro vaši aplikaci. Také doporučí tootune způsoby, jak vaše aplikace hello tooget naplno využít Azure SQL Database."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: dd8d95fa-24b2-4233-b3f1-8e8952a7a22b
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 02/09/2017
ms.author: carlrab
ms.openlocfilehash: 2699f755391e94ab488ac1e6acedd30f8aec4488
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tuning-performance-in-azure-sql-database"></a>Ladění výkonu v Azure SQL Database

Databáze SQL Azure poskytuje [doporučení](sql-database-advisor.md) , můžete použít tooimprove výkon vaší databáze, nebo můžete nechat Azure SQL Database [automaticky přizpůsobit tooyour aplikace](sql-database-automatic-tuning.md) a použití změn. který zlepší výkon vašich úloh.

Ve službě nemáte žádná doporučení použít a máte problémy s výkonem, můžete použít následující metody tooimprove přínos hello:
1. Zvýšit [úrovních služeb](sql-database-service-tiers.md) a poskytnout další prostředky tooyour databáze.
2. Ladění aplikace a použít některé osvědčené postupy, které může zlepšit výkon. 
3. Vyladění hello databáze změnou indexy a dotazy toomore efektivně pracovat s daty.

Tyto jsou ruční metody, protože toodecide co potřebujete [úrovních služeb](sql-database-service-tiers.md) by zvolte nebo by potřebujete toorewrite aplikace nebo kódu databáze a deply hello změny.

## <a name="increasing-performance-tier-of-your-database"></a>Zvýšení úrovně výkonu databáze

Azure SQL Database nabízí čtyři [úrovních služeb](sql-database-service-tiers.md) ze kterých si můžete vybrat: Basic, Standard, Premium a Premium RS (výkonu se měří v jednotky propustnosti databáze, nebo [Dtu](sql-database-what-is-a-dtu.md). Každé úrovni služeb výhradně izoluje hello prostředků, můžete použít SQL database a zaručuje, předvídatelný výkon této úrovně služeb. V tomto článku jsme nabízí pokyny, které vám pomohou vybrat hello vrstvy služeb pro vaši aplikaci. Můžeme také popisují způsoby, abyste mohli vyladit vaší aplikace hello tooget naplno využít Azure SQL Database.

> [!NOTE]
> Tento článek se zaměřuje na pokyny výkonu pro izolované databáze ve službě Azure SQL Database. Průvodce výkonem souvisejících tooelastic fondy, najdete v části [cenové a výkonové požadavky pro elastické fondy](sql-database-elastic-pool-guidance.md). Upozorňujeme ale, můžete použít řadu hello ladění doporučení v toodatabases Tento článek v elastickém fondu a získejte výhody podobné výkonu.
> 

* **Základní**: hello služby na úrovni Basic vrstvy nabízí dobrý výkon předvídatelnost pro každou databázi, hodinu přes hodinu. V databázi základní podporují dostatek prostředků dobrý výkon v malé databázi, která nemá více souběžných požadavků. Typické použití případy, kdy byste měli použít úroveň základní služby jsou:
  * **Můžete se právě Začínáme s Azure SQL Database**. Aplikace, které jsou často vývojem nepotřebují vysoce výkonné úrovně. Základní databáze jsou ideální prostředí pro vývoj databází nebo testování okamžiku nízkou cenu.
  * **Máte databázi s jedním uživatelem**. Aplikace, které uživatel s jednotným přidružit databáze obvykle nemají vysokou požadavky na souběžnost a výkon. Tyto aplikace jsou kandidáty pro vrstvu služby na úrovni Basic hello.
* **Standardní**: vrstvy služby na úrovni Standard hello nabízí lepší výkon, předvídatelnost a poskytuje dostatečný výkon pro databáze, které mají více souběžných žádostí, jako je pracovní skupiny a webové aplikace. Když vyberete databázi vrstvy služby na úrovni Standard, může Velikost databáze aplikace založené na předvídatelný výkon, minutu přes minutu.
  * **Databáze má více souběžných požadavků**. Aplikace, které služeb více uživatelů najednou obvykle nutné vyšší úrovně výkonu. Například pracovní skupině nebo webové aplikace, které mají podpora více souběžných dotazů požadavky na provoz nízkou toomedium vstupně-výstupní operace jsou vhodnými kandidáty pro vrstvu služby na úrovni Standard hello.
* **Premium**: hello úroveň služeb Premium poskytuje předvídatelný výkon, druhý přes druhý, pro každou databázi Premium. Když zvolíte hello úroveň služeb Premium, můžete měnit velikost databáze aplikace založené na zátěž ve špičce hello pro tuto databázi. plán Hello odebere případy, ve kterých může způsobit výkonu odchylku malé dotazy tootake déle, než se očekává v operacích citlivý na latenci. Tento model může výrazně zjednodušit hello vývoj a produktu cykly ověření pro aplikace, které potřebují toomake silné prohlášení o potřebných prostředků ve špičce, odchylku výkonu nebo latence dotazu. Většina případy použití vrstvy služby Premium mít jeden nebo více z těchto vlastností:
  * **Zátěž ve špičce vysokou**. Aplikaci, která vyžaduje významné CPU, paměť či vstupně výstupní (I/O) toocomplete jeho operace vyžaduje úroveň vyhrazené a vysoce výkonné. Například operace databáze známé tooconsume několik jader procesoru po delší dobu je kandidátem na úroveň služeb Premium hello.
  * **Mnoha souběžnými požadavky**. Některé databáze aplikace služby mnoho souběžných žádostí, například když obsluhuje web, který má intenzivní provoz. Úrovně Basic a Standard úrovně služeb omezit hello počet souběžných požadavků na databázi. Aplikace, které vyžadují více připojení potřebovat toochoose příslušné rezervace velikost toohandle hello maximální počet požadavků, které potřebné.
  * **Nízká latence**. Některé aplikace potřebují tooguarantee odpověď z databáze hello minimální včas. Pokud konkrétní uložená procedura je volána v rámci širší operace zákazníka, můžete mít požadavek toohave vrátit z tohoto volání v milisekundách více než 20, 99 procent času hello. Tento typ aplikace výhody z úroveň služeb Premium hello toomake se, že tento hello vyžadováno computing power je k dispozici.
* **Premium RS**: hello vrstvy Premium RS je určená pro úlohy náročné na vstupně-výstupní operace, které nevyžadují hello nejvyšší záruky dostupnosti. Mezi příklady patří testování vysoce výkonné úlohy, nebo analytické úlohy, kde hello databáze není hello systému záznamu.

Hello úroveň služby, kterou potřebujete pro vaši databázi SQL, závisí na požadavky na zatížení ve špičce hello každé dimenze prostředků. Některé aplikace využívat trivial množství jediný zdroj, ale mají významný požadavky na jiné prostředky.

### <a name="service-tier-capabilities-and-limits"></a>Možnosti vrstvy služby a omezení

Na jednotlivých úrovních služby nastavit úroveň výkonu hello, takže máte hello flexibilitu toopay pouze pro potřebnou kapacitu hello. Můžete [upravit kapacity](sql-database-service-tiers.md), nahoru nebo dolů, jako změny zatížení. Například pokud vaše databáze úlohy vysoké během hello zpět školní nákupní sezóny, můžete zvýšit úroveň výkonu hello hello databáze nastavte dobu, červenec prostřednictvím září. Ho může snížit, až skončí vaše sezóny ve špičce. Můžete minimalizovat platíte pomocí optimalizace vaše cloudové prostředí toohello sezónnosti vaší firmy. Tento model se také funguje dobře pro cykly verze softwaru produktu. Test tým může přidělit kapacity při testování spustí a poté uvolněte tuto kapacitu, při jejich dokončení testování. V žádosti o modelu kapacitu platíte kapacity ho potřebujete, a vyhnout se výdaje na vyhrazených prostředcích, které můžou používat jenom občas.

### <a name="why-service-tiers"></a>Proč úrovních služeb?
I když každé zatížení databáze se může lišit, hello účelem úrovně služeb je tooprovide výkonu předvídatelnost na různé úrovně výkonu. Zákazníci s požadavky na prostředky ve velkém měřítku databáze můžete pracovat ve více vyhrazené výpočetním prostředí.

## <a name="tune-your-application"></a>Vyladění aplikace
Tradiční na místním serveru SQL je hello proces plánování kapacity počáteční často oddělená od hello proces spuštění aplikace v produkčním prostředí. Hardware a produktu licence jsou nejprve zakoupit a optimalizace výkonu se provádí i později. Pokud používáte Azure SQL Database, je vhodné toointerweave hello proces spuštění aplikace a vyladění jeho. S hello model platícího kapacity na vyžádání můžete vyladit vaší aplikace toouse hello minimální prostředky potřebné teď místo předimenzování na hardwaru podle pokusů budoucímu růstu plány pro aplikace, které často jsou nesprávné. Někteří zákazníci mohou zvolte není tootune aplikace a místo toho toooverprovision hardwarové prostředky. Tento přístup může být vhodné, pokud nechcete, aby toochange klíče aplikace během období zaneprázdněný. Ale ladění aplikace snížit požadavky na prostředky a nižší měsíční faktur při použití úrovně služeb hello ve službě Azure SQL Database.

### <a name="application-characteristics"></a>Vlastnosti aplikace
I když Azure SQL Database úrovně služeb jsou navrženou tooimprove výkonu stability a předvídatelnost pro aplikaci, můžete pomocí některé z osvědčených postupů ladit aplikace využít výhody toobetter hello prostředků na úrovni výkonu. I když mnoho aplikací mít výrazné zvýšení výkonu jednoduše pomocí přepínání tooa vyšší úroveň výkonu nebo služba vrstvy, některé aplikace potřeba další ladění toobenefit z vyšší úrovně služby. Pro zvýšení výkonu vezměte v úvahu další aplikaci ladění pro aplikace, které mají tyto charakteristiky:

* **Aplikace, které mají pomalý výkon z důvodu "chatty" chování**. Chatty aplikace provést operace přístupu nadměrné dat, které jsou citlivé toonetwork latence. Toomodify může být nutné tyto typy aplikací tooreduce hello počet databázi SQL data access operations toohello. Například může zlepšení výkonu aplikací pomocí techniky jako dávkování dotazů ad-hoc nebo přesun Klientova hello dotazuje toostored postupy. Další informace najdete v tématu [dávky dotazy](#batch-queries).
* **Databáze s náročné úlohy, která nepodporují se celý jeden počítač**. Databáze, které překračují hello prostředky hello nejvyšší úroveň Premium výkonu mohou využít škálování hello zatížení. Další informace najdete v tématu [horizontálního dělení mezidatabázové](#cross-database-sharding) a [funkční dělení](#functional-partitioning).
* **Aplikace, které mají zhoršené dotazy**. Aplikace, zejména v hello vrstva, která mají špatně přizpůsobená dotazy nemusí využít vyšší úroveň výkonu. To zahrnuje dotazy, které chybí klauzule WHERE, chybějící indexy nebo zastaralé statistiky. Tyto aplikace využít techniky optimalizace výkonu standardní dotazu. Další informace najdete v tématu [chybějící indexy](#identifying-and-adding-missing-indexes) a [ladění a zobrazování rad dotazů](#query-tuning-and-hinting).
* **Aplikace, které mají zhoršené data přístup k návrhu**. Aplikace, které mají vyplývajících data potíže se souběžností přístup, například vzájemné zablokování, nemusí využít vyšší úroveň výkonu. Zvažte snížení odezev proti hello Azure SQL Database pomocí ukládání dat do mezipaměti na straně klienta hello s hello služby ukládání do mezipaměti Azure nebo jiné technologie ukládání do mezipaměti. V tématu [ukládání do mezipaměti aplikace vrstvy](#application-tier-caching).

## <a name="tune-your-database"></a>Vyladění databáze
V této části se podíváme na některé techniky, můžete použít tootune Azure SQL Database toogain hello nejlepšího výkonu pro vaši aplikaci a spustit na nejnižší úrovni možný výkon hello. Některé z těchto postupů odpovídat tradiční ladění osvědčené postupy systému SQL Server, ale jiné jsou konkrétní tooAzure databáze SQL. V některých případech můžete prozkoumat hello využívat prostředky pro databázi toofind oblasti toofurther ladit a rozšířit tradiční toowork techniky systému SQL Server v Azure SQL Database.

### <a name="identify-performance-issues-using-azure-portal"></a>Identifikovat problémy s výkonem pomocí portálu Azure
Hello následující nástroje v hello portálu Azure můžete analyzovat a opravit problémy s výkonem s vaší databázi SQL:

* [Query Performance Insight](sql-database-query-performance.md)
* [SQL Database Advisor](sql-database-advisor.md)

Hello portál Azure obsahuje další informace o oba tyto nástroje a jak toouse je. tooefficiently diagnostikovat a opravit problémy, doporučujeme Nejdřív zkuste hello nástrojů v hello portálu Azure. Doporučujeme použít ruční hello ladění přístupy, které dál probereme chybějící indexy a dotaz optimalizace ve zvláštních případech.

Další informace o identifikaci problémů ve službě Azure SQL Database na [monitorování výkonu](sql-database-single-database-monitor.md) článku.

### <a name="identifying-and-adding-missing-indexes"></a>Identifikace a přidání chybějící indexy
Běžné potíže s výkonem databáze OLTP má vztah toohello fyzická databáze návrhu. Databáze schémata jsou často, určené a dodaný bez testování ve velkém měřítku, (buď v zatížení nebo datový svazek). Bohužel hello výkon plán dotazu může být přijatelné v malém měřítku, ale podstatně snížit pod úroveň výroby datové svazky. Hello nejběžnější zdroj tohoto problému je chybějící hello filtry toosatisfy příslušné indexy nebo jiná omezení v dotazu. Často chybějící indexy manifesty jako tabulku kontrolovat může stačit indexu vyhledávání.

V tomto příkladu používá plán vybraný dotaz hello kontrolu při by stačit seek:

    DROP TABLE dbo.missingindex;
    CREATE TABLE dbo.missingindex (col1 INT IDENTITY PRIMARY KEY, col2 INT);
    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO dbo.missingindex(col2) VALUES (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION;
    GO
    SELECT m1.col1
    FROM dbo.missingindex m1 INNER JOIN dbo.missingindex m2 ON(m1.col1=m2.col1)
    WHERE m1.col2 = 4;

![Plán dotazu s chybějící indexy](./media/sql-database-performance-guidance/query_plan_missing_indexes.png)

Databáze SQL Azure můžete najít a opravu běžné chybí indexu podmínky. Podívejte se na kompilace dotazu, ve kterých by indexu výrazně snížit hello odhadované náklady na toorun dotazem zobrazení dynamické správy, které jsou součástí Azure SQL Database. Během provádění dotazu SQL Database sleduje, jak často se spustí každý plán dotazu a sleduje hello odhadované mezera mezi hello provádění plán dotazu a hello podobě jeden kde existovaly tento index. Můžete použít tyto odhad tooquickly zobrazení dynamické správy, které změny tooyour fyzická databáze návrhu může zvýšit náklady na celkové zatížení pro databázi a její skutečné pracovní vytížení.

Můžete použít tento dotaz tooevaluate potenciál chybějící indexy:

    SELECT CONVERT (varchar, getdate(), 126) AS runtime,
        mig.index_group_handle, mid.index_handle,
        CONVERT (decimal (28,1), migs.avg_total_user_cost * migs.avg_user_impact *
                (migs.user_seeks + migs.user_scans)) AS improvement_measure,
        'CREATE INDEX missing_index_' + CONVERT (varchar, mig.index_group_handle) + '_' +
                  CONVERT (varchar, mid.index_handle) + ' ON ' + mid.statement + '
                  (' + ISNULL (mid.equality_columns,'')
                  + CASE WHEN mid.equality_columns IS NOT NULL
                              AND mid.inequality_columns IS NOT NULL
                         THEN ',' ELSE '' END + ISNULL (mid.inequality_columns, '')
                  + ')'
                  + ISNULL (' INCLUDE (' + mid.included_columns + ')', '') AS create_index_statement,
        migs.*,
        mid.database_id,
        mid.[object_id]
    FROM sys.dm_db_missing_index_groups AS mig
    INNER JOIN sys.dm_db_missing_index_group_stats AS migs
        ON migs.group_handle = mig.index_group_handle
    INNER JOIN sys.dm_db_missing_index_details AS mid
        ON mig.index_handle = mid.index_handle
    ORDER BY migs.avg_total_user_cost * migs.avg_user_impact * (migs.user_seeks + migs.user_scans) DESC

V tomto příkladu dotaz hello výsledkem tohoto návrhu:

    CREATE INDEX missing_index_5006_5005 ON [dbo].[missingindex] ([col2])  

Po vytvoření, že stejný příkaz SELECT vybere jiný plán, který místo kontrolu používá hledání a pak efektivněji provede hello plánu:

![Plán dotazu s opravené indexy](./media/sql-database-performance-guidance/query_plan_corrected_indexes.png)

Přehled klíče Hello je tento hello vstupně-výstupní kapacita sdílenou, komoditním systému je omezenější než počítač vyhrazený server. Je na minimalizovat nepotřebné vstupně-výstupních operací tootake maximální výhody systému hello hello DTU jednotlivých úrovně výkonu hello úrovně služeb Azure SQL Database premium. Rozhodnutích při návrhu fyzického databázi může výrazně zlepšit latenci hello pro jednotlivé dotazy, zlepšit propustnost hello souběžných požadavků zpracovává na jednotku škálování a minimalizovat hello náklady na požadované toosatisfy hello dotazu. Další informace o hello chybí index zobrazení dynamické správy najdete v tématu [sys.dm_db_missing_index_details](https://msdn.microsoft.com/library/ms345434.aspx).

### <a name="query-tuning-and-hinting"></a>Ladění dotazu a zobrazování rad
Optimalizátor dotazů Hello ve službě Azure SQL Database je podobné toohello tradiční Optimalizátor dotazů systému SQL Server. Většina hello osvědčené postupy pro ladění dotazy a pochopení hello odůvodnění modelu omezení pro Optimalizátor dotazů hello také použít tooAzure databáze SQL. Pokud je optimalizace dotazů v databázi SQL Azure, může získat hello další výhody snižuje nároky na agregační prostředků. Aplikace může být schopný toorun při nižších nákladech, než naladěného ekvivalentní, proto můžete spustit na nižší úrovni výkonu.

Příklad, který je běžné v systému SQL Server a které se vztahují taky tooAzure databáze SQL je, jak hello Optimalizátor "zachytává obsah" parametrů dotazu. Během kompilace vyhodnotí Optimalizátor dotazů hello hello aktuální hodnota parametru toodetermine, zda může vygenerovat víc optimální plán dotazu. I když tato strategie často může způsobit tooa plán dotazu, který je mnohem rychlejší než plán kompilovat bez parametru známé hodnoty, aktuálně funguje imperfectly i v systému SQL Server a ve službě Azure SQL Database. Někdy není zachycení hello parametr a někdy zachycení hello parametr ale vygenerovaný plán hello je zhoršené pro hello úplnou sadu hodnot parametrů v zatížení. Microsoft zahrnuje pomocné parametry dotazu (direktivy), takže můžete zadat více úmyslně záměr a přepsat výchozí chování hello sledování toku dat parametr. Často Pokud používáte pomocné parametry, můžete je vyřešit případy, ve kterých je SQL Server nebo Azure SQL Database výchozí chování hello nedokonalé pro konkrétního zákazníka zatížení.

Hello další příklad ukazuje, jak může procesor dotazů hello Generovat plán, který je zhoršené pro výkon a požadavky na prostředky. Tento příklad také ukazuje, že pokud používáte pomocný parametr dotazu, můžete snížit požadavky na spuštění dotazu času a prostředků pro vaši databázi SQL:

    DROP TABLE psptest1;
    CREATE TABLE psptest1(col1 int primary key identity, col2 int, col3 binary(200));

    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO psptest1(col2) values (1);
        INSERT INTO psptest1(col2) values (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION
    CREATE INDEX i1 on psptest1(col2);
    GO

    CREATE PROCEDURE psp1 (@param1 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1
        WHERE col2 = @param1
        ORDER BY col2;
    END
    GO

    CREATE PROCEDURE psp2 (@param2 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1 WHERE col2 = @param2
        ORDER BY col2
        OPTION (OPTIMIZE FOR (@param2 UNKNOWN))
    END
    GO

    CREATE TABLE t1 (col1 int primary key, col2 int, col3 binary(200));
    GO

Hello instalační kód vytvoří tabulku, která má nesouměrně rozdělí distribuci dat. Hello optimální dotazu, že plán se liší podle které zablokováno. Bohužel hello plán chování ukládání do mezipaměti není vždy znovu zkompiluje dotaz hello na základě hodnoty parametru nejběžnější hello. Ano je možné zhoršené plán toobe do mezipaměti a použít pro více hodnot, i v případě, že jiný plán může být vhodnější plán v průměru. Plán dotazu hello vytvoří dvě uložené procedury, které jsou stejné, s tím rozdílem, že jeden má nápovědu speciální dotazu.

**Příklad, část 1**

    -- Prime Procedure Cache with scan plan
    EXEC psp1 @param1=1;
    TRUNCATE TABLE t1;

    -- Iterate multiple times tooshow hello performance difference
    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp1 @param1=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

**Příklad, část 2**

(Doporučujeme Počkejte minimálně 10 minut, než začnete, část 2 / například hello, tak, aby hello výsledky jsou jedinečné v hello výsledná telemetrická data.)

    EXEC psp2 @param2=1;
    TRUNCATE TABLE t1;

    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp2 @param2=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

Jednotlivých součástí v tomto příkladu se pokusí toorun příkaz parametrizované insert 1 000krát (toogenerate dostatečná toouse zatížení jako testovací sada dat). Pokud se provede uložené procedury, procesor dotazů hello prověří hodnota parametru hello, který je předán toohello postup během jeho první kompilace (parametr "sledování toku dat"). procesor Hello ukládá do mezipaměti hello výsledný plán a používá je pro pozdější volání i v případě, že hodnota parametru hello se liší. ve všech případech se možná nepoužívají Hello optimální plán. Někdy můžete potřebovat tooguide hello Optimalizátor toopick plán, který je lepší pro případ průměrná hello, nikoli konkrétní případ hello z při nejprve kompilovány hello dotazu. V tomto příkladu generuje počáteční plán hello "kontrola" plán, který čte všechny řádky toofind každou hodnotu, která odpovídá parametru hello:

![Dotaz na ladění pomocí plánu kontroly](./media/sql-database-performance-guidance/query_tuning_1.png)

Protože jsme hello postup provést pomocí hello hodnota 1, výsledný plán hello byla optimální pro hello hodnotu 1, ale byla zhoršené pro všechny ostatní hodnoty v tabulce hello. výsledek Hello pravděpodobně není co vhodnější, pokud byste byli toopick každý plánování náhodně, protože plán hello provede pomaleji a používá další prostředky.

Pokud spustíte test hello s `SET STATISTICS IO` nastavit příliš`ON`, pracovní hello logické kontroly v tomto příkladu se provádí pozadí hello. Můžete zjistit, že jsou 1,148 čtení provádí hello plán (což je neefektivní, pokud průměrná případ hello tooreturn pouze jeden řádek):

![Dotaz na ladění pomocí logické kontroly](./media/sql-database-performance-guidance/query_tuning_2.png)

Hello druhou částí hello příklad používá během kompilace hello dotazu pomocný parametr tootell hello Optimalizátor toouse konkrétní hodnotu. V takovém případě vynutí hello dotazu procesoru tooignore hello hodnotu, která se předá jako parametr hello a místo toho tooassume `UNKNOWN`. Vztahuje se tooa hodnotu, která má hello Průměrná frekvence v tabulce hello (ignorována zkosení). na základě seek plán, který je rychlejší a v průměru, než plán hello používá menší množství prostředků, část 1 v tomto příkladu je výsledný plán Hello:

![Ladění dotaz s použitím pomocného parametru dotazu](./media/sql-database-performance-guidance/query_tuning_3.png)

Uvidíte hello vliv v hello **sys.resource_stats** tabulky (dochází ke zpoždění od hello času spuštění testu hello a když hello dat naplní hello tabulky). Pro tento příklad, část 1 provést během časového okna hello 22:25:00 a část 2 provedená 22:35:00. Hello starší časové okno použít další zdroje v dané časové okno než hello později se jeden (z důvodu zlepšení efektivity plánu).

    SELECT TOP 1000 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![Ladění příklad výsledky dotazu](./media/sql-database-performance-guidance/query_tuning_4.png)

> [!NOTE]
> I když hello svazku v tomto příkladu je záměrně malé, může být hello účinku zhoršené parametry podstatné, zejména u velkých databází. Hello rozdíl ve výjimečných případech může být v rozmezí sekund pro rychlé případy a čas pro případy, pomalé.
> 
> 

Můžete zkontrolovat **sys.resource_stats** toodetermine tom, zda text hello prostředků pro test používá více nebo méně prostředků než jiného testu. Při porovnávání dat oddělte hello načasování testy tak, aby se nenacházejí ve hello stejného časového období 5 minut v hello **sys.resource_stats** zobrazení. Hello cílem hello cvičení je toominimize hello celkový objem prostředků používá a ne toominimize hello ve špičce prostředky. Obecně platí optimalizace úsek kódu pro latenci zmenšuje spotřeby prostředků. Ujistěte se, že je nutné provést změny hello provedete tooan aplikace a že hello změny neovlivňují negativně zkušeností zákazníků se hello někomu, kdo může pomocí pomocné parametry dotazu v aplikaci hello.

Pokud úloha má sadu opakujících se dotazů, často vytvoří toocapture smysl a ověřit hello optimality zvoleného plán, protože ho jednotky hello prostředků minimální velikost jednotky požadované toohost hello databáze. Po ověření, někdy prozkoumat hello plány že toohelp byste se ujistit, že nebyly degradovaný. Další informace o [dotaz pomocné parametry (Transact-SQL)](https://msdn.microsoft.com/library/ms181714.aspx).

### <a name="cross-database-sharding"></a>Mezidatabázové horizontálního dělení
Vzhledem k Azure SQL Database běží na komoditním hardwaru, hello limity kapacity pro izolované databáze jsou nižší než v případě tradičních místních instalace SQL serveru. Někteří zákazníci používají horizontálního dělení techniky toospread databázových operací přes více databází při hello operations není nevejde se do hello limity služby jedné databáze ve službě Azure SQL Database. Většina zákazníkům, kteří používají horizontálního dělení techniky ve službě Azure SQL Database rozdělit do několika databází svá data na jedinou dimenzí. Tento postup musíte toounderstand OLTP aplikace často provádění transakcí, které se vztahují tooonly jeden řádek nebo tooa malou skupinu řádků ve schématu hello.

> [!NOTE]
> Databáze SQL teď poskytuje knihovna tooassist s horizontálního dělení. Další informace najdete v tématu [přehled klientské knihovny pro elastické databáze](sql-database-elastic-database-client-library.md).
> 
> 

Například pokud databáze obsahuje jméno zákazníka, pořadí a podrobnosti pořadí (jako jsou hello databáze Northwind tradiční příklad, který se dodává se systémem SQL Server), může rozdělování tato data do více databází tím, že zákazník s hello související pořadí a podrobnosti pořadí informace. Může zaručit, že data tohoto zákazníka hello zůstává v jedné databáze. aplikace Hello by rozdělit různých zákazníků do databáze, efektivně šíření hello zatížení napříč více databází. S horizontálního dělení, zákazníci nejen se můžete vyhnout hello maximální limit velikosti databáze, ale Azure SQL Database může také zpracovat úlohy, které jsou podstatně větší než omezení hello hello úrovně různých výkonu, tak dlouho, dokud každé jednotlivé databáze zapadá do jeho DTU.

I když horizontálního dělení databáze nedojde k omezení kapacity hello agregační zdroje pro řešení, je vysoce efektivní v podpora velmi velké řešení, která jsou rozloženy více databází. Každou databázi, můžete spustit na výkonu různé úrovně toosupport velký, "efektivní" databáze s vysokými požadavky na prostředky.

### <a name="functional-partitioning"></a>Funkční oddíly
Uživatelům systému SQL Server často kombinovat mnoho funkcí v jedné databáze. Například pokud má aplikace logiky toomanage inventáře pro úložiště, že databáze může být logiku související s inventářem, sledování nákupních objednávek, uložené procedury a indexované nebo Vyhodnocená zobrazení, které Správa vytváření sestav koncový měsíc. Tímto způsobem lze snadněji tooadminister hello databáze pro operace, jako je zálohování, ale vyžaduje také můžete toosize hello hardwaru toohandle hello zátěž ve špičce mezi všechny funkce aplikace.

Pokud používáte architekturu škálování ve službě Azure SQL Database, je různé funkce vhodné toosplit aplikace do různých databází. Pomocí tohoto postupu nezávisle škáluje každou aplikaci. Když aplikaci stane Vytíženější (a hello zátěž zvyšuje databáze hello), hello správce může rozhodnout úrovně nezávislé výkonu pro každou funkci v aplikaci hello. Aplikace může být v hello limit, tato architektura větší než jeden komoditním počítač dokáže zpracovat, protože hello zatížení je rozdělena mezi více počítačů.

### <a name="batch-queries"></a>Batch dotazy
Pro aplikace, které přístup k datům s použitím vysoký počet časté, ad hoc dotazy, vyžadovat značné množství doba odezvy je stráví na síťovou komunikaci mezi hello aplikační vrstvě a vrstvy hello Azure SQL Database. I když obě hello aplikace a databáze SQL Azure jsou ve stejném datovém centru, hello latence sítě mezi hello dva může zvětšit podle velký počet operací přístupu k datům hello. Zaokrouhlí sítě hello tooreduce cest pro hello data v operace přístupu, zvažte použití hello možnost tooeither batch hello ad hoc dotazy nebo toocompile je jako uložené procedury. Pokud jste dávky hello ad hoc dotazy, můžete odeslat více dotazů jako jeden velký batch v jedné cestě tooAzure databáze SQL. Pokud zkompilujete ad hoc dotazy v uložené proceduře, můžete dosáhnout hello stejné dojít, jako kdyby je dávky je. Pomocí uložené procedury také poskytuje hello výhodou zvýšit pravděpodobnost hello ukládání do mezipaměti hello plány dotazů v databázi SQL Azure, abyste mohli používat hello uloženou proceduru znovu.

Některé aplikace jsou náročné na zápis. Někdy můžete snížit zatížení hello celkový počet vstupně-výstupních operací v databázi v úvahu, jak toobatch zapíše společně. Často je stejně jednoduché jako používání explicitních transakcí místo automatického potvrzení transakce v uložené procedury a ad hoc dávky. Vyhodnocení různých technik, které můžete použít, najdete v části [dávkování techniky pro aplikace, databáze SQL v Azure](https://msdn.microsoft.com/library/windowsazure/dn132615.aspx). Experimentujte s vlastní zatížení toofind hello model správné pro dávkové zpracování. Zda toounderstand, který může mít modelu se mírně zaručuje různých transakční konzistence. Hledání hello správné zatížení, které snižuje využití prostředků vyžaduje hledání hello správnou kombinaci konzistencí a výkonem kompromis.

### <a name="application-tier-caching"></a>Ukládání do mezipaměti aplikační vrstvy
Některé databázových aplikací mít úlohy náročné na čtení. Ukládání do mezipaměti vrstvy může snížit zatížení hello hello databáze a může potenciálně snížili úrovni požadované toosupport hello výkonu databáze pomocí Azure SQL Database. S [Azure Redis Cache](https://azure.microsoft.com/services/cache/), pokud máte úlohy náročné na čtení, můžete číst hello data jednou (nebo případně jednou za aplikační vrstvy počítače, v závislosti na tom, jak je nakonfigurovaná) a potom tato data mimo vaší databázi SQL. Toto je způsob, jak zatížení databáze tooreduce (procesoru a čtení vstupně-výstupních operací), ale protože hello data při čtení z mezipaměti hello mohou být synchronizován s hello data v databázi hello je vliv na transakční konzistence. V mnoha aplikacích určité úrovně, ke kterému je přijatelné, není, platí pro všechny úlohy. Všechny požadavky aplikace byste měli plně rozumět před implementací strategie pro ukládání do mezipaměti aplikační vrstvy.

## <a name="next-steps"></a>Další kroky
* Další informace o úrovních služeb najdete v tématu [výkon a možnosti databáze SQL](sql-database-service-tiers.md)
* Další informace o elastické fondy najdete v tématu [co je Azure elastickém fondu?](sql-database-elastic-pool.md)
* Informace o výkonu a Elastická fondy najdete v tématu [při tooconsider fondu elastické databáze](sql-database-elastic-pool-guidance.md)

