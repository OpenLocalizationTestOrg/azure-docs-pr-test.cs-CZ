---
title: "aaaResolving T-SQL rozdíly. migrace Azure SQL Database | Microsoft Docs"
description: "Příkazy jazyka Transact-SQL, které služba Azure SQL Database plně nepodporuje"
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: c05abd9e-28a7-4c97-9bdf-bc60d08fc92e
ms.service: sql-database
ms.custom: load & move data
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 03/17/2017
ms.author: rickbyh
ms.openlocfilehash: 3852013338bfdc6c7da9d1d879c54781682bc635
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="resolving-transact-sql-differences-during-migration-toosql-database"></a>Řešení rozdílů Transact-SQL během migrace tooSQL databáze   
Když [migrace vaší databáze](sql-database-cloud-migrate.md) z tooAzure systému SQL Server SQL Server, může se stát, že vaše databáze vyžaduje některé znovu technici před hello systému SQL Server lze migrovat. Toto téma obsahuje, že je potřeba tooassist pokyny, které jste v provádění technici znovu i pochopení hello základní z důvodů, proč hello znovu technici. toodetect problémům s kompatibilitou, použijte hello [pomocníka pro migraci dat (DMA)](https://www.microsoft.com/download/details.aspx?id=53595).

## <a name="overview"></a>Přehled
Většinu funkcí jazyka Transact-SQL, které používají aplikace jsou plně podporovaný v systému Microsoft SQL Server a databáze SQL Azure. Například hello jádra součásti SQL, například datové typy, operátory, řetězec, aritmetické logické a kurzor funkce fungovat stejně jako v systému SQL Server a SQL Database. Existují, ale několik rozdílů T-SQL v DDL (data definition language) a elementy DML (jazyk pro manipulaci dat), které jsou výsledkem příkazů T-SQL a dotazy, které jsou podporovány pouze částečně (který probereme později v tomto tématu).

Kromě toho některé funkce a syntaxi, která není podporována na všech, protože databáze SQL Azure je určená tooisolate funkce závislosti v hlavní databázi hello a hello operačního systému. Jako takový většinu aktivit na úrovni serveru nejsou vhodná pro databáze SQL. Příkazů T-SQL a možnosti nejsou k dispozici, pokud konfigurovat možnosti úrovni serveru, komponent operačního systému, nebo zadejte konfiguraci systému souborů. Pokud například tyto funkce je potřeba, alternativou odpovídající je často k dispozici jiným způsobem z databáze SQL nebo z jiného Azure funkce nebo služba. 

Například vysoké dostupnosti je integrovaná do Azure, takže konfigurace Always On není nutné (i když může být vhodné tooconfigure aktivní geografickou replikací pro rychlejší obnovení v případě havárie hello). Ano příkazů T-SQL související tooavailability skupiny nejsou podporovány SQL Database a zobrazení dynamické správy hello na související tooAlways. také nejsou podporovány.

Seznam hello funkcí, které jsou podporované a nepodporované SQL Database, najdete v části [porovnání funkcí Azure SQL Database](sql-database-features.md). seznam Hello na této stránce doplňují tohoto tématu pokyny a funkce a se zaměřuje na příkazy jazyka Transact-SQL.

## <a name="transact-sql-syntax-statements-with-partial-differences"></a>Příkazy Syntaxe Transact-SQL s částečné rozdíly
příkazy DDL (data definition language) základní Hello jsou k dispozici, ale některé příkazy DDL mít rozšíření související toodisk umístění a nepodporované funkce. 

- Příkazy CREATE a ALTER DATABASE mít více než tucet tři možnosti. příkazy Hello zahrnují umístění souboru FILESTREAM a možnosti zprostředkovatele služby, které lze použít pouze tooSQL serveru. To není důležité, zda vytváření databází před migrací, ale pokud provádíte migraci kód T-SQL, který vytvoří databáze byste měli porovnat [CREATE DATABASE (Azure SQL Database)](https://msdn.microsoft.com/library/dn268335.aspx) syntaxí hello systému SQL Server v [vytvořit DATABÁZE (SQL Server Transact-SQL)](https://msdn.microsoft.com/library/ms176061.aspx) toomake, zda jsou podporovány všechny možnosti hello používáte. CREATE DATABASE pro databázi SQL Azure má také cíl služby a elastické škálování možnosti, které se vztahují pouze tooSQL databáze.
- příkazy CREATE a ALTER TABLE Hello mají FileTable možností, které nelze použít v databázi SQL, protože FILESTREAM není podporována.
- Příkazy CREATE a ALTER login jsou podporována, ale databáze SQL nenabízí všechny možnosti hello. toomake víc přenosného databáze SQL Database podporuje použití obsahovala uživatelů databáze místo přihlášení. kdykoli je to možné. Další informace najdete v tématu [CREATE nebo ALTER LOGIN](https://msdn.microsoft.com/library/ms189828.aspx) a [řízení a udělení přístupu k databázi](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins).

## <a name="transact-sql-syntax-not-supported-in-sql-database"></a>Syntaxe jazyka Transact-SQL nepodporovaná službou SQL Database   
Kromě toho tooTransact-SQL příkazy související toohello nepodporované funkce popsané v [porovnání funkcí Azure SQL Database](sql-database-features.md), hello následující příkazy a skupiny příkazy, nejsou podporovány. Jako takový, pokud vaše databáze toobe migrovat některé z používá hello následující funkce, znovu analyzovat vaše T-SQL tooeliminate těmito funkcemi T-SQL a příkazy.

- Kolace systémových objektů
- Související s připojením: příkazy ENDPOINT, `ORIGINAL_DB_NAME`. Databáze SQL nepodporuje ověřování systému Windows, ale podporuje hello podobné ověřování Azure Active Directory. Některé typy ověřování vyžadují hello nejnovější verzi aplikace SSMS. Další informace najdete v tématu [tooSQL připojení databáze nebo SQL Data Warehouse pomocí pomocí Azure ověřování Active Directory](sql-database-aad-authentication.md).
- Mezidatabázové dotazy, které používají tři nebo čtyři názvy částí (mezidatabázové dotazy jen pro čtení jsou podporované prostřednictvím [dotazů do Elastic Database](sql-database-elastic-query-overview.md)).
- Mezidatabázové řetězení vlastnictví, nastavení `TRUSTWORTHY`
- `DATABASEPROPERTY` Místo toho použijte `DATABASEPROPERTYEX`.
- `EXECUTE AS LOGIN` Místo toho použijte EXECUTE AS USER.
- Šifrování je podporované s výjimkou služby EKM (extensible key management).
- Zpracování událostí: Události, události oznámení, oznámení dotazů
- Umístění souboru: syntaxe související toodatabase umístění souboru, velikost a databázové soubory, které jsou automaticky spravovány Microsoft Azure.
- Vysoká dostupnost: syntaxe související toohigh dostupnosti, který je spravován pomocí účtu Microsoft Azure. Patří sem syntaxe zálohování, obnovení, Always On, zrcadlení databáze, přesouvání protokolu a režimů obnovení.
- Čtečky protokolu: syntaxi, která závisí na hello čtečky protokolu, která není k dispozici pro službu SQL Database: Push replikace, Change Data Capture. SQL Database může být odběratelem článku nabízené replikace.
- Funkce: `fn_get_sql`, `fn_virtualfilestats`, `fn_virtualservernodes`
- Globální dočasné tabulky
- Hardware: Syntaxe související nastavení související s toohardware serveru: například paměť, pracovních vláken, spřažení procesoru, označí příznakem trasování. Místo ní použijte úrovně služby.
- `HAS_DBACCESS`
- `KILL STATS JOB`
- `OPENQUERY`, `OPENROWSET`, `OPENDATASOURCE`a sestávající ze čtyř částí názvy
- Rozhraní .NET framework: Integrace modulu CLR s SQL serverem
- Sémantické vyhledávání
- Přihlašovací údaje serveru: použití [přihlašovací údaje na obor definovaný databází](https://msdn.microsoft.com/library/mt270260.aspx) místo.
- Serverové položky: serverové role, `IS_SRVROLEMEMBER`, `sys.login_token`. K dispozici nejsou příkazy `GRANT`, `REVOKE` a `DENY` serverových oprávnění, i když některé z nich jsou nahrazené oprávněními na úrovni databáze. Některá praktická serverová zobrazení dynamických zpráv (DMV) mají odpovídající databázová zobrazení dynamických zpráv.
- `SET REMOTE_PROC_TRANSACTIONS`
- `SHUTDOWN`
- `sp_addmessage`
- Možnosti `sp_configure` a `RECONFIGURE`. Některé možnosti jsou dostupné prostřednictvím příkazu [ALTER DATABASE SCOPED CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx).
- `sp_helpuser`
- `sp_migrate_user_to_contained`
- Agent systému SQL Server: Syntaxi, která závisí na databázi MSDB agenta systému SQL Server nebo hello hello: výstrahy, operátory, servery pro centrální správu. Použijte raději skriptování, například v Azure PowerShellu.
- SQL Server audit: auditování databáze SQL použijte místo toho.
- Trasování SQL Serveru
- Příznaky trasování: některé příznak trasování položky byly přesunuty toocompatibility režimy.
- Ladění Transact-SQL
- Aktivační události: serverové nebo přihlašovací aktivační události
- `USE`příkaz: toochange hello kontextu tooa jiné databáze, je nutné provést nové připojení toohello novou databázi.

## <a name="full-transact-sql-reference"></a>Kompletní reference k jazyku Transact-SQL
Další informace o syntaxi a používání jazyka Transact-SQL, včetně příkladů, najdete v tématu [Reference k jazyku Transact-SQL (databázový stroj)](https://msdn.microsoft.com/library/bb510741.aspx) v dokumentaci SQL Server Books Online. 

### <a name="about-hello-applies-to-tags"></a>O značkách "Platí pro" hello
Hello Transact-SQL odkaz obsahuje témata související tooSQL toohello 2008 verze serveru existuje. Pod nadpisem tématu hello je zobrazí ikona panel, výpis hello čtyři platformy systému SQL Server a naznačuje použitelnosti. Například skupiny dostupnosti byly zavedeny v SQL Serveru 2012. [Vytvořit SKUPINU AVAILABILTY](https://msdn.microsoft.com/library/ff878399.aspx) tématu označuje, zda text hello údajů se vztahuje na **systému SQL Server (počínaje 2012)**. příkaz Hello nevztahuje tooSQL Server 2008, SQL Server 2008 R2, databáze SQL Azure, Azure SQL Data Warehouse nebo Parallel Data Warehouse.

V některých případech hello předmět obecné téma lze použít v produktu, ale existují malé rozdíly mezi produkty. Hello rozdíly jsou uvedeny ve středních bodů v tématu hello podle potřeby. V některých případech hello předmět obecné téma lze použít v produktu, ale existují malé rozdíly mezi produkty. Hello rozdíly jsou uvedeny ve středních bodů v tématu hello podle potřeby. Například hello CREATE TRIGGER tématu je k dispozici v databázi SQL. Ale hello **všechny SERVER** možnost pro aktivační procedury úrovni serveru, znamená, že úrovni serveru aktivační události nelze použít v databázi SQL. Místo toho použijte úroveň databáze aktivační události.

## <a name="next-steps"></a>Další kroky

Seznam hello funkcí, které jsou podporované a nepodporované SQL Database, najdete v části [porovnání funkcí Azure SQL Database](sql-database-features.md). seznam Hello na této stránce doplňují tohoto tématu pokyny a funkce a se zaměřuje na příkazy jazyka Transact-SQL.

