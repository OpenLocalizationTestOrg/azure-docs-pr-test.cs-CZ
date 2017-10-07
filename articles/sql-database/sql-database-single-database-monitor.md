---
title: "výkon databáze aaaMonitoring ve službě Azure SQL Database | Microsoft Docs"
description: "Informace o možnostech hello monitorování vaší databáze pomocí nástrojů Azure a zobrazení dynamické správy."
keywords: "monitorování databáze, výkon cloudové databáze"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: a2e47475-c955-4a8d-a65c-cbef9a6d9b9f
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: b13771183d4ccf37f58e2fc518b9b14de38212dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-database-performance-in-azure-sql-database"></a>Monitorování výkonu databáze ve službě Azure SQL Database
Sledování výkonu hello databáze SQL v Azure začíná sledováním využití hello prostředků relativní toohello úroveň výkonu databáze, které zvolíte. Monitorování vám pomůže určit, zda vaše databáze má přebytečnou kapacitou, nebo má potíže s, protože prostředky jsou podle toho, se a pak rozhodnout, zda je úroveň výkonu hello tooadjust čas a [vrstvy služby](sql-database-service-tiers.md) vaší databáze. Můžete monitorovat pomocí grafických nástrojů v hello databáze [portál Azure](https://portal.azure.com) nebo pomocí nástroje SQL [zobrazení dynamické správy](https://msdn.microsoft.com/library/ms188754.aspx).

## <a name="monitor-databases-using-hello-azure-portal"></a>Monitorování databází pomocí hello portálu Azure
V hello [portál Azure](https://portal.azure.com/), můžete monitorovat využití izolované databáze vyberete databázi a kliknutím na hello **monitorování** grafu. Po výběru této možnosti **metrika** okno, které můžete změnit kliknutím hello **upravit graf** tlačítko. Přidejte následující metriky hello:

* Procento CPU
* Procento DTU
* Procento datových V/V
* Procento velikosti databáze

Jakmile přidáte tyto metriky, můžete pokračovat v tooview je v hello **monitorování** graf s další podrobnosti o hello **metrika** okno. Všechny čtyři metriky uvádějí hello průměrné využití procento relativní toohello **DTU** vaší databáze. V tématu hello [úrovních služeb](sql-database-service-tiers.md) podrobnosti o jednotkách Dtu najdete v článku.

![Monitorování výkonu databáze v rámci úrovně služeb](./media/sql-database-service-tiers/sqldb_service_tier_monitoring.png)

Výstrahy můžete nakonfigurovat také na metriky výkonu hello. Klikněte na tlačítko hello **přidat upozornění** tlačítka na hello **metrika** okno. Postupujte podle průvodce tooconfigure hello upozornění. Máte možnost tooalert hello, pokud hello překročení zadané prahové hodnoty nebo pokud hello metrika poklesne pod určitou mez.

Například pokud očekáváte hello zatížení na toogrow vaší databáze, můžete tooconfigure e-mailové upozornění pokaždé, když vaše databáze překročí 80 % kterékoli hello metrik výkonu. Můžete použít jako včasné upozornění toofigure se při může být tooswitch toohello vyšší úroveň výkonu.

metriky výkonu Hello také můžete zjistit, zda je možné toodowngrade tooa nižší úroveň výkonu. Předpokládají se používá databáze Standard S2 a zobrazit všechny metriky výkonu, který hello databáze v průměru nevyužívá více než 10 % v daném okamžiku. Je pravděpodobné, že hello databáze bude fungovat i v úrovni Standard S1. Však být vědomi úlohy, které špiček nebo náhlého před provedením hello rozhodnutí toomove tooa nižší úroveň výkonu.

## <a name="monitor-databases-using-dmvs"></a>Monitorování databází pomocí zobrazení dynamické správy
Hello stejné metriky, které jsou zveřejněné hello portálu jsou taky dostupné prostřednictvím systémových zobrazení: [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) v hello logické **hlavní** databázi vašeho serveru a [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) v hello uživatelské databáze. Použití **sys.resource_stats** Pokud potřebujete toomonitor méně granulární dat napříč delší časové období. Použití **sys.dm_db_resource_stats** Pokud potřebujete toomonitor podrobnější data v kratších úsecích. Další informace najdete v tématu [Azure SQL Database – průvodce výkonem](sql-database-single-database-monitor.md#monitor-resource-use).

> [!NOTE]
> **Sys.dm_db_resource_stats** vrací prázdný výsledek při použití pro databáze s úrovněmi Web a Business, které jsou již ukončené.
>
>

### <a name="monitor-resource-use"></a>Sledování využití prostředků

Můžete monitorovat využití prostředků pomocí [SQL databáze Query Performance Insight](sql-database-query-performance.md) a [úložiště dotazů](https://msdn.microsoft.com/library/dn817826.aspx).

Můžete také sledovat využití pomocí těchto dvou zobrazení:

* [Sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)
* [Sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx)

#### <a name="sysdmdbresourcestats"></a>Sys.dm_db_resource_stats
Můžete použít hello [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) zobrazení v každé databázi SQL. Hello **sys.dm_db_resource_stats** zobrazení ukazuje poslední prostředků pomocí datové relativní toohello služby vrstvě. Průměrnou procentuální hodnotu pro procesor, vstupů/výstupů dat, protokolu zápisy a paměti se zaznamenávají každých 15 sekund a jsou uchovávány 1 hodina.

Protože toto zobrazení nabízí podrobnější pohled na využití prostředků, použijte **sys.dm_db_resource_stats** první pro nějakou analýzu aktuální stav nebo řešení potíží. Tento dotaz zobrazí například, že hello průměrný a maximální prostředků používat pro aktuální databázi hello prostřednictvím hello poslední hodiny:

    SELECT  
        AVG(avg_cpu_percent) AS 'Average CPU use in percent',
        MAX(avg_cpu_percent) AS 'Maximum CPU use in percent',
        AVG(avg_data_io_percent) AS 'Average data I/O in percent',
        MAX(avg_data_io_percent) AS 'Maximum data I/O in percent',
        AVG(avg_log_write_percent) AS 'Average log write use in percent',
        MAX(avg_log_write_percent) AS 'Maximum log write use in percent',
        AVG(avg_memory_usage_percent) AS 'Average memory use in percent',
        MAX(avg_memory_usage_percent) AS 'Maximum memory use in percent'
    FROM sys.dm_db_resource_stats;  

Pro jiné dotazy hello příklady naleznete v [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx).

#### <a name="sysresourcestats"></a>Sys.resource_stats
Hello [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) zobrazení v hello **hlavní** databáze obsahuje další informace, které můžete sledovat výkon hello vaší databáze SQL na úrovni konkrétní službu a výkonu . Hello data se shromažďují pro každých 5 minut a bude zachována pro účely přibližně 35 dnů. Toto zobrazení je užitečné pro dlouhodobější analýzu historie používání prostředků vaší databázi SQL.

Hello následující graf ukazuje hello využití procesoru prostředků u databáze Premium P2 úroveň výkonu hello pro každou hodinu v týdnu. Tento graf začíná v pondělí, zobrazuje 5 pracovních dní a poté zobrazí víkendu, když se u aplikace hello stane mnohem méně.

![Využití prostředků databáze SQL](./media/sql-database-performance-guidance/sql_db_resource_utilization.png)

Z dat hello tuto databázi aktuálně má zatížení procesoru ve špičce právě více než 50 procent procesoru použít relativní toohello P2 úroveň výkonu (poledne úterý). Pokud procesor hello dominantní faktor v profilu aplikace hello prostředků, může rozhodnout, že P2 je, že vyhovuje tooguarantee úrovni výkonu, který hello zatížení vždy hello. Pokud očekáváte toogrow aplikaci v čase, je vhodné toohave vyrovnávací paměť navíc prostředků tak, aby aplikace hello není někdy dosáhla limitu úroveň výkonu hello. Pokud zvýšíte úroveň výkonu hello, můžete vyhnout zákazníka viditelné chybách, ke kterým může dojít, když databáze nemá dostatek požadavky tooprocess power efektivně, zejména v prostředích citlivý na latenci. Příkladem je databáze, která podporuje aplikace, která vybarví webové stránky, na základě výsledků hello volání databáze.

Jinými typy aplikací může vyhodnotit hello stejné graf jinak. Například pokud aplikace pokusí data mzdy tooprocess každý den a má hello stejném grafu, tento druh modelu "dávkovou úlohu" může udělat bez problémů na úroveň výkonu P1. Hello úroveň výkonu P1 má 100 Dtu too200 Dtu porovnání v hello P2 úroveň výkonu. Hello úroveň výkonu P1 poskytuje poloviční hello výkon hello P2 úroveň výkonu. Ano 50 procent hodnoty využití procesoru v P2 rovná 100 procent využití procesoru v P1. Pokud aplikace hello nemá vypršení časových limitů, je nemusí důležité, zda úloha trvá 2 hodiny nebo toofinish 2,5 hodiny, pokud získá dnes provést. Aplikace v této kategorii pravděpodobně můžete použít úroveň výkonu P1. Můžete využít výhod hello fakt, že jsou dobu během dne hello, pokud je využití prostředků nižší, tak, aby všechny "velký ve špičce" může distribuována do jedné žlaby hello později v den hello. tak dlouho, dokud hello úloh můžete dokončit na čas každý den, může být vhodné pro tento typ aplikace (a uložte peníze) Hello úroveň výkonu P1.

Azure SQL Database zpřístupňuje využívat informace o prostředcích pro každou aktivní databáze v hello **sys.resource_stats** zobrazení hello **hlavní** databází v každém serveru. Hello data v tabulce hello se shromažďují pro 5 minutách. S hello Basic, Standard a Premium úrovně služeb hello dat může trvat víc než 5 minut tooappear v tabulce hello tak, aby tato data užitečnější pro historické analýzy, nikoli analysis téměř v reálném čase. Dotaz hello **sys.resource_stats** zobrazení toosee hello nedávné historii databáze a toovalidate, zda text hello rezervace zvolíte doručit hello výkonu, které chcete v případě potřeby.

> [!NOTE]
> Musí být připojené toohello **hlavní** databáze vaše logické tooquery databáze serveru SQL **sys.resource_stats** v hello následující příklady.
> 
> 

Tento příklad ukazuje, jak je vystaven hello data v tomto zobrazení:

    SELECT TOP 10 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![zobrazení katalogu sys.resource_stats Hello](./media/sql-database-performance-guidance/sys_resource_stats.png)

Hello další příklad ukazuje, různé způsoby, které můžete použít hello **sys.resource_stats** katalogu zobrazit tooget informace o používání prostředků vaší databázi SQL:

1. toolook v hello za týden prostředků použít pro userdb1 hello databáze, můžete spustit tento dotaz:
   
        SELECT *
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND
              start_time > DATEADD(day, -7, GETDATE())
        ORDER BY start_time DESC;
2. tooevaluate jak dobře vaše úlohy odpovídá hello úroveň výkonu, je nutné toodrill dolů do každý aspekt metrika prostředků hello: procesoru, čtení, zápisu, počet pracovních procesů a počet relací. Tady je revidované dotazování pomocí **sys.resource_stats** tooreport hello průměrný a maximální hodnoty metrik těchto prostředků:
   
        SELECT
            avg(avg_cpu_percent) AS 'Average CPU use in percent',
            max(avg_cpu_percent) AS 'Maximum CPU use in percent',
            avg(avg_data_io_percent) AS 'Average physical data I/O use in percent',
            max(avg_data_io_percent) AS 'Maximum physical data I/O use in percent',
            avg(avg_log_write_percent) AS 'Average log write use in percent',
            max(avg_log_write_percent) AS 'Maximum log write use in percent',
            avg(max_session_percent) AS 'Average % of sessions',
            max(max_session_percent) AS 'Maximum % of sessions',
            avg(max_worker_percent) AS 'Average % of workers',
            max(max_worker_percent) AS 'Maximum % of workers'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
3. Tyto informace o hello průměrný a maximální hodnoty každého prostředku metriky a můžete vyhodnotit, jak dobře vaše úlohy zapadá do hello úroveň výkonu, které vyberete. Obvykle, průměrná hodnoty z **sys.resource_stats** poskytují základní funkční toouse proti hello cílovou velikost. Mělo by být váš primární měření Flash disk. Příklad by mohla využívat hello vrstvy služby na úrovni Standard S2 úroveň výkonu. průměr Hello pomocí procenta pro procesor a vstupně-výstupní operace čtení a zápisů jsou pod 40 procent, hello průměrný počet pracovních procesů je menší než 50 a hello průměrný počet relací, které je nižší než 200. Vaše zatížení může začlenit do hello úrovní výkonu S1. Je snadno toosee zda databáze se vejde hello worker a omezení relací. toosee, zda databáze zapadá do nižší úroveň výkonu s jde o tooCPU, čtení a zápisu, vydělte hello DTU počet nižší úroveň výkonu hello hello DTU počet vaše aktuální úroveň výkonu a výsledek hello vynásobit 100:
   
    **S1 DTU / S2 DTU * 100 = 20 NEBO 50 * 100 = 40**
   
    Výsledkem Hello je hello relativní výkon rozdíl mezi hello dvě úrovně výkonu v procentech. Pokud vaše využití prostředků nepřekročí toto množství, může vaše úlohy začlenit do hello nižší úroveň výkonu. Ale potřebovat toolook na všechny rozsahy hodnot použití prostředků a zjistit, v procentech, jak často by vaše databáze úlohy se vešla do hello nižší úroveň výkonu. Hello následující dotaz vypíše, že hello přizpůsobit procento na dimenzi prostředků, podle hello prahovou hodnotu 40 procent vypočtené v tomto příkladu:
   
        SELECT
            (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log Write Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical Data IO Fit Percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
   
    Podle vaší databáze cíle na úrovni služby (SLO), můžete rozhodnout, jestli vaše úlohy zapadá do hello nižší úroveň výkonu. Pokud vaše databáze úlohy SLO je 99,9 % a hello předchozí dotaz vrátí hodnoty vyšší než 99,9 % pro všechny tři prostředků dimenze, vaše úlohy pravděpodobně zapadá do hello nižší úroveň výkonu.
   
    Prohlížení hello přizpůsobit procento také získáte přehled o tom, jestli je možné přesunout toohello další vyšší výkon úrovně toomeet vaše SLO. Například userdb1 zobrazuje hello následující využití procesoru pro hello uplynulý týden:
   
   | Průměrné využití procesoru v procentech | Maximální procento využití procesoru |
   | --- | --- |
   | 24.5 |100.00 |
   
    průměrné využití procesoru Hello je o čtvrtletí hello omezení hello úroveň výkonu, které by se vešla do úroveň výkonu hello hello databáze. Ale maximální hodnota hello ukazuje, že hello databáze dosáhne hello limit úroveň výkonu hello. Potřebujete toomove toohello vyšší úroveň výkonu? Podívejte se na to, jak tolikrát, kolikrát vaše úlohy dosáhnou 100 procent a porovnejte je zatížení databáze tooyour SLO.
   
        SELECT
        (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log write fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical data I/O fit percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
   
    Pokud tento dotaz vrací hodnotu menší než 99,9 % pro některá z dimenzí hello tři prostředků, zvažte buď přesunutí toohello vyšší úroveň výkonu nebo použijte optimalizace pro aplikace techniky tooreduce hello zatížení v databázi SQL hello.
4. Tento postup také zvažuje vaší předpokládané úlohy zvýšení hello budoucí.

Pro elastické fondy můžete monitorovat jednotlivé databáze ve fondu hello s hello technik popsaných v této části. Ale můžete také sledovat hello fond jako celek. Další informace najdete v tématu [Monitorování a správa elastického fondu](sql-database-elastic-pool-manage-portal.md).


### <a name="maximum-concurrent-requests"></a>Maximální souběžných požadavků
toosee hello počet souběžných požadavků, spusťte tento dotaz jazyka Transact-SQL na databázi SQL:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R

Tento dotaz toofilter upravit zatížení hello tooanalyze místní databáze systému SQL Server, na konkrétní databázi hello chcete tooanalyze. Pokud máte místní databázi s názvem databáze, tento dotaz jazyka Transact-SQL vrátí hello počet souběžných požadavků v databázi:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R
    INNER JOIN sys.databases D ON D.database_id = R.database_id
    AND D.name = 'MyDatabase'

Toto je právě snímku na jednom místě v čase. tooget lépe porozumět zatížení a požadavky na počtu souběžných požadavků, budete potřebovat toocollect mnoha ukázek v čase.

### <a name="maximum-concurrent-logins"></a>Maximální souběžných přihlášení
Můžete analyzovat vaše uživatele a aplikace vzory tooget představu o hello frekvenci přihlášení. Skutečné zatížení taky můžete spustit v toomake testovací prostředí, zda nejsou stiskne to nebo další omezení, které v tomto článku probereme. Není k dispozici jeden dotaz nebo zobrazení dynamické správy (DMV), která umožňuje zobrazit souběžných, že počty přihlášení nebo historie.

Pokud používat více klientů hello stejný připojovací řetězec, hello služby ověřuje každé přihlášení. Pokud 10 uživatelů najednou připojit tooa databáze pomocí hello stejné uživatelské jméno a heslo, bude 10 souběžných přihlášení. Toto omezení se vztahuje pouze toohello trvání hello přihlášení a ověřování. Pokud uživatelé hello stejné 10 připojit databázi toohello postupně, by nikdy hello počet souběžných přihlášení být větší než 1.

> [!NOTE]
> V současné době toto omezení neplatí toodatabases v elastické fondy.
> 
> 

### <a name="maximum-sessions"></a>Maximální počet relací
toosee hello počet aktuální aktivních relací, spusťte tento dotaz jazyka Transact-SQL ve vaší databázi SQL:

    SELECT COUNT(*) AS [Sessions]
    FROM sys.dm_exec_connections

Pokud při analýze pracovního vytížení místní systém SQL Server, upravte dotaz toofocus hello určitou databázi. Tento dotaz vám pomůže určit možné relace potřeby hello databáze Pokud uvažujete o přesunutím tooAzure databáze SQL.

    SELECT COUNT(*)  AS [Sessions]
    FROM sys.dm_exec_connections C
    INNER JOIN sys.dm_exec_sessions S ON (S.session_id = C.session_id)
    INNER JOIN sys.databases D ON (D.database_id = S.database_id)
    WHERE D.name = 'MyDatabase'

Tyto dotazy znovu, vrátí počet bodu v čase. Pokud shromažďujete více ukázky v čase, budete mít hello je nejlepší Principy relace použít.

Pro analýzu SQL Database, můžete získat historická statistiky u relací dotazováním hello [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) zobrazení a kontrola hello **active_session_count** sloupce. 
