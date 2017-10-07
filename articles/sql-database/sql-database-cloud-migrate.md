---
title: "aaaSQL Server databáze migrace tooAzure databáze SQL | Microsoft Docs"
description: "Další informace o tom, jak systému SQL Server databáze migrace tooAzure databáze SQL v cloudu hello. Pomocí databáze migrace nástroje tootest kompatibility předchozí toodatabase migrace."
keywords: "migrace databáze, migrace databáze systému sql server, nástroje pro migraci databáze, migrace databáze, migrace sql database"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 9cf09000-87fc-4589-8543-a89175151bc2
ms.service: sql-database
ms.custom: load & move data
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: sqldb-migrate
ms.date: 02/08/2017
ms.author: carlrab
ms.openlocfilehash: 3a5e879404dd2da1dd5254a6134e3ee1517648db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sql-server-database-migration-toosql-database-in-hello-cloud"></a>TooSQL migrace databáze systému SQL Server databáze v cloudu hello
V tomto článku můžete další informace o hello dvě základní metody pro migraci systému SQL Server 2005 nebo novější tooAzure databáze SQL Database. Hello první způsob je jednodušší, ale vyžaduje některé, které by mohly mít významné výpadky během migrace hello. druhý metoda Hello je složitější, ale podstatně eliminuje výpadek během migrace hello.

V obou případech je nutné tooensure této hello zdrojové databáze je kompatibilní s Azure SQL Database pomocí hello [pomocníka pro migraci dat (DMA)](https://www.microsoft.com/download/details.aspx?id=53595). Databáze SQL verze 12 dosahuje [parita funkce](sql-database-features.md) se systémem SQL Server, než operace související tooserver úrovni a mezidatabázové problémy. Databáze a aplikace, které jsou závislé na [částečně podporované nebo nepodporované funkce](sql-database-transact-sql-information.md) potřebovat některé [znovu technici toofix tyto nekompatibility](sql-database-cloud-migrate.md#resolving-database-migration-compatibility-issues) před hello systému SQL Server mohou být migrovány databáze.

> [!NOTE]
> najdete v databázi SQL serveru, včetně Microsoft Access, Sybase, MySQL Oracle a DB2 tooAzure SQL Database, toomigrate [SQL Server migrace Assistant](https://blogs.msdn.microsoft.com/datamigration/2016/12/22/released-sql-server-migration-assistant-ssma-v7-2/).
> 

## <a name="method-1-migration-with-downtime-during-hello-migration"></a>Metoda 1: Migrace s výpadky při migraci hello

 Tento způsob použijte, pokud si můžete nějaké prostoje dovolit nebo testujete migraci produkční databáze pro pozdější migraci. Podívejte se kurz [migrovat databázi systému SQL Server](sql-database-migrate-your-sql-server-database.md).

Hello následující seznam obsahuje hello obecný postup pro migraci databáze systému SQL Server pomocí této metody.

  ![Diagram migrace VSSSDT](./media/sql-database-cloud-migrate/azure-sql-migration-sql-db.png)

1. Vyhodnocení hello databáze pro kompatibilitu pomocí nejnovější verze hello [pomocníka pro migraci dat (DMA)](https://www.microsoft.com/download/details.aspx?id=53595).
2. Příprava všech nezbytných oprav ve formě skriptů Transact-SQL.
3. Vytvořit kopii hello zdrojové databáze stavu transakční konzistence migrovaného - a ujistěte se žádné další změny jsou určené toohello zdrojové databáze (nebo můžete ručně provést tyto změny po dokončení migrace hello). Existuje mnoho metody tooquiesce databáze, z zakázání toocreating připojení klienta [snímek databáze](https://msdn.microsoft.com/library/ms175876.aspx).
4. Nasaďte hello Transact-SQL skriptů tooapply hello opravy toohello kopie databáze.
5. [Export](sql-database-export.md) hello tooa kopie databáze. Soubor souboru BACPAC na místním disku.
6. [Import](sql-database-import.md) hello. Soubor souboru BACPAC jako novou databázi Azure SQL pomocí kterékoli z několika souboru BACPAC importovat nástrojů s SQLPackage.exe se hello doporučuje nástroj pro nejlepší výkon.

### <a name="optimizing-data-transfer-performance-during-migration"></a>Optimalizace výkonu přenosu dat během migrace 

Hello následující seznam obsahuje doporučení pro nejlepší výkon během procesu importu hello.

* Zvolte hello nejvyšší úrovně služby a výkonu vrstvy, rozpočet umožňuje toomaximize hello přenos výkonu. Po dokončení migrace hello toosave money, můžete škálovat. 
* Minimalizovat hello vzdálenost mezi vaší. Souboru BACPAC souborové služby a hello cílového datového centra.
* Zakažte během migrace automatické statistiky.
* Rozdělte tabulky a indexy na oddíly.
* Zrušte indexovaná zobrazení a po dokončení je znovu vytvořte.
* Odebrání databáze tooanother zřídka dotazované historických dat a migrovat tato historická data tooa samostatné databáze Azure SQL. Potom můžete historická data dotazovat pomocí [elastických dotazů](sql-database-elastic-query-overview.md).

### <a name="optimize-performance-after-hello-migration-completes"></a>Optimalizace výkonu po dokončení migrace hello

[Aktualizovat statistiku](https://msdn.microsoft.com/library/ms187348.aspx) s úplné prohledávání po dokončení migrace hello.

## <a name="method-2-use-transactional-replication"></a>Způsob 2: Použití transakční replikace

Pokud nechcete tooremove databázi SQL serveru z výroby během migrace hello je, můžete použít transakční replikace systému SQL Server jako řešení pro migraci. toouse musí tuto metodu, hello zdrojové databáze plnění hello [požadavky pro transakční replikace](https://msdn.microsoft.com/library/mt589530.aspx) a mít kompatibilní pro databázi SQL Azure. Informace o replikaci SQL s funkcí AlwaysOn najdete v tématu [konfiguraci replikace pro skupin dostupnosti Always On (SQL Server)](/sql/database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server).

toouse toto řešení nakonfigurovat vaší databázi SQL Azure jako instance systému SQL Server toohello odběratele chcete toomigrate. Hello transakční replikace distributora synchronizuje data z hello databáze toobe synchronizovány (hello vydavatel) při nové transakce pokračovat, dojde k. 

S transakční replikace všechny změny tooyour data nebo schéma objeví ve vaší databázi SQL Azure. Po dokončení synchronizace hello a jsou připravené toomigrate, změnit připojovací řetězec hello toopoint vaší aplikace je tooyour Azure SQL Database. Jakmile transakční replikace se vyprázdní změny ponecháno na zdrojové databáze a všechny aplikace bod tooAzure DB, můžete odinstalovat, transakční replikace. Vaším produkčním systémem je nyní služba Azure SQL Database.

 ![Diagram přidání počátečních hodnot do cloudu pomocí transakční replikace](./media/sql-database-cloud-migrate/SeedCloudTR.png)

> [!TIP]
> Můžete také použít transakční replikace toomigrate podmnožinu zdrojové databáze. publikace Hello replikaci tooAzure SQL Database může být omezené tooa podmnožinu hello tabulky v databázi hello replikuje. Pro každou tabulku právě replikován můžete omezit hello data tooa podmnožinu řádků hello nebo podmnožinu sloupců hello.
>

### <a name="migration-toosql-database-using-transaction-replication-workflow"></a>Migrace tooSQL databáze pomocí pracovního postupu transakcí replikace

> [!IMPORTANT]
> Použití hello nejnovější verzi SQL Server Management Studio tooremain synchronizován s tooMicrosoft aktualizace Azure a SQL Database. Starší verze aplikace SQL Server Management Studio neumožňují nastavení služby SQL Database jako odběratele. [Aktualizovat aplikaci SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
> 

1. Nastavení distribuce
   -  [Pomocí aplikace SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/ms151192.aspx#Anchor_1)
   -  [Pomocí jazyka Transact-SQL](https://msdn.microsoft.com/library/ms151192.aspx#Anchor_2)
2. Vytvoření publikace
   -  [Pomocí aplikace SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/ms151160.aspx#Anchor_1)
   -  [Pomocí jazyka Transact-SQL](https://msdn.microsoft.com/library/ms151160.aspx#Anchor_2)
3. Vytvoření předplatného
   -  [Pomocí aplikace SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/ms152566.aspx#Anchor_0)
   -  [Pomocí jazyka Transact-SQL](https://msdn.microsoft.com/library/ms152566.aspx#Anchor_1)

### <a name="some-tips-and-differences-for-migrating-toosql-database"></a>Některé tipy a rozdíly pro migraci tooSQL databáze

1. Použití místního distributora 
   - To způsobí, že dopad na výkon na serveru hello. 
   - Pokud vlivu na výkon hello nepřijatelný, můžete použít jiný server, ale přidá složitost správy a správy.
2. Když vyberete složku snímku, ujistěte se, hello složku, kterou vyberete dostatečně velké na to toohold BCP každé tabulky chcete tooreplicate. 
3. Zámky vytvoření snímku hello přidružené tabulky, dokud se nedokončí, takže správně naplánovat snímku. 
4. Služba Azure SQL Database podporuje jenom nabízené odběry. Odběratelé můžete přidat jenom ze zdrojové databáze hello.

## <a name="resolving-database-migration-compatibility-issues"></a>Řešení problémů s kompatibilitou při migrování databáze
Existují širokou škálu problémy s kompatibilitou, kterými se můžete setkat, v závislosti na hello verzi systému SQL Server v složitost databáze a hello zdroj hello hello databáze, kterou provádíte migraci. Starší verze systému SQL Server mají více problémů s kompatibilitou. Použít následující prostředky hello, kromě tooa cílené pomocí vyhledávací modul možností hledání v Internetu:

* [Funkce databáze systému SQL Server nepodporované ve službě Azure SQL Database](sql-database-transact-sql-information.md)
* [Ukončená funkce databázového stroje v systému SQL Server 2016](https://msdn.microsoft.com/library/ms144262%28v=sql.130%29)
* [Ukončená funkce databázového stroje v systému SQL Server 2014](https://msdn.microsoft.com/library/ms144262%28v=sql.120%29)
* [Ukončená funkce databázového stroje v systému SQL Server 2012](https://msdn.microsoft.com/library/ms144262%28v=sql.110%29)
* [Ukončená funkce databázového stroje v systému SQL Server 2008 R2](https://msdn.microsoft.com/library/ms144262%28v=sql.105%29)
* [Ukončená funkce databázového stroje v systému SQL Server 2005](https://msdn.microsoft.com/library/ms144262%28v=sql.90%29)

Kromě toho toosearching hello Internet a pomocí těchto prostředků, použijte hello [MSDN SQL Server komunitní fóra](https://social.msdn.microsoft.com/Forums/sqlserver/home?category=sqlserver) nebo [StackOverflow](http://stackoverflow.com/).

## <a name="next-steps"></a>Další kroky
* Použít skript hello na blogu Azure SQL EMEA technici hello příliš[sledování využití databáze tempdb během migrace](https://blogs.msdn.microsoft.com/azuresqlemea/2016/12/28/lesson-learned-10-monitoring-tempdb-usage/).
* Použít skript hello na blogu Azure SQL EMEA technici hello příliš[monitorování hello místa protokolu transakcí databáze během migrace je](https://blogs.msdn.microsoft.com/azuresqlemea/2016/10/31/lesson-learned-7-monitoring-the-transaction-log-space-of-my-database/0).
* SQL serveru poradní tým blog o migraci pomocí souboru BACPAC soubory, najdete v tématu [migrace ze systému SQL Server tooAzure databáze SQL pomocí souboru BACPAC souborů](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).
* Informace o práci s časem UTC po migraci najdete v tématu [Modifying hello výchozí časové pásmo pro místním časovém pásmu](https://blogs.msdn.microsoft.com/azuresqlemea/2016/07/27/lesson-learned-4-modifying-the-default-time-zone-for-your-local-time-zone/).
* Informace o změně hello výchozí jazyk databáze po migraci najdete v tématu [jak toochange hello výchozí jazyk databáze SQL Azure](https://blogs.msdn.microsoft.com/azuresqlemea/2017/01/13/lesson-learned-16-how-to-change-the-default-language-of-azure-sql-database/).


