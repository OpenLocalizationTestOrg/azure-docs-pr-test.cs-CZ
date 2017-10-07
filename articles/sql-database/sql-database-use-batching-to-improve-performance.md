---
title: "aaaHow toouse dávkování tooimprove výkonu aplikací Azure SQL Database"
description: "Hello tématu poskytuje důkaz, že dávkování databáze operations výrazně imroves hello rychlost a škálovatelnost aplikací Azure SQL Database. I když tyto dávkování techniky fungovat pro libovolnou databázi systému SQL Server, hello cílem tohoto článku hello je v Azure."
services: sql-database
documentationcenter: na
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 563862ca-c65a-46f6-975d-10df7ff6aa9c
ms.service: sql-database
ms.custom: develop apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/12/2016
ms.author: sstein
ms.openlocfilehash: 124b203ee69c595f0813852ff09ef9ec6841233a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-batching-tooimprove-sql-database-application-performance"></a>Jak toouse dávkování výkon aplikace tooimprove databáze SQL
Dávkování operací tooAzure SQL Database výrazně zlepšuje hello výkon a škálovatelnost aplikací. V pořadí toounderstand hello výhody hello první část tohoto článku popisuje některé vzorové výsledky testů, které porovnávají požadavky sekvenční a dávkové tooa databáze SQL. Hello zbývající části článku hello se zobrazuje hello techniky, scénáře a aspekty toohelp toouse dávkování úspěšně v aplikacích Azure.

## <a name="why-is-batching-important-for-sql-database"></a>Proč je dávkování důležité pro databázi SQL?
Dávkování volání tooa vzdálené služby je dobře známé strategie pro zvýšení výkonu a škálovatelnosti. Existuje se vyřešilo zpracování náklady tooany interakce s vzdálené služby, jako je například serializace, přenos v síti a deserializace. Balení mnoho samostatné transakcí do jedné dávkové minimalizuje tyto náklady.

V tomto dokumentu chceme tooexamine různé dávkování strategie SQL Database a scénáře. I když tyto strategie jsou také důležité pro místní aplikace, které používají systém SQL Server, tady je několik důvodů pro zvýraznění hello použití dávkování pro databázi SQL:

* Je potenciálně vyšší latence sítě přístup k databázi SQL, zejména pokud v přístupu k databázi SQL z mimo hello stejného datového centra Microsoft Azure.
* Hello víceklientské vlastnosti SQL Database znamená, že hello efektivitu hello dat přístup k vrstvě korelaci toohello celkovou škálovatelnost hello databáze. Databáze SQL zabránit přivlastňuje databáze prostředků toohello úkor ostatních klientů žádné jednoho klienta nebo uživatele. Databáze SQL v odpovědi toousage překračující předdefinované kvóty, můžete snížit propustnost nebo odpovědět s omezení výjimky. Efektivitu své činnosti, jako je například dávkování, povolte je toodo další práci v databázi SQL dříve, než dorazila těchto mezních hodnot. 
* Dávkování je také efektivní pro architektury, které používají více databází (horizontálního dělení). Hello efektivitu interakce se jednotlivých jednotek databáze je stále klíčovým faktorem vaší celkovou škálovatelnost. 

Jednou z výhod hello používání databáze SQL je, že nemáte servery hello toomanage databázi hello hostitele. Však této spravované infrastruktury také znamená, že můžete mít toothink jinak o optimalizace databáze. Už můžete zobrazit tooimprove hello databáze hardware nebo síť infrastruktury. Microsoft Azure řídí těchto prostředích. Hello hlavní oblasti, která můžete řídit je, jak vaše aplikace komunikuje s databází SQL. Dávkování je jedním z těchto optimalizace. 

první část Hello hello dokumentu prověří různé dávkování techniky pro aplikace .NET, které používají SQL Database. Hello poslední dvě části se věnují dávkování pokyny a scénáře.

## <a name="batching-strategies"></a>Dávkování strategie
### <a name="note-about-timing-results-in-this-topic"></a>Všimněte si o výsledcích časování v tomto tématu
> [!NOTE]
> Výsledky nejsou srovnávacích testů, ale jsou určené tooshow **relativní výkon**. Časování jsou založené na v průměru minimálně 10 test spustí. Operace se vloží do prázdná tabulka. Tyto testy byly měřená pre-V12 a neodpovídají nutně toothroughput, ke kterému může dojít v databázi V12 pomocí hello nové [úrovních služeb](sql-database-service-tiers.md). Hello relativní výhodou hello dávkování technika by mělo být podobné.
> 
> 

### <a name="transactions"></a>Transakce
Podle všeho neobvyklé toobegin o dávkování podle hovoříte o transakce. Ale hello použití transakce na straně klienta má vliv dávkování jemně straně serveru, který zlepšuje výkon. A transakcí lze přidat pouze zadání několika řádků kódu, takže poskytují rychlý způsob tooimprove výkonu sekvenčních operací.

Zvažte hello následující kód C#, který obsahuje posloupnost vložení a aktualizace operací na jednoduché tabulky.

    List<string> dbOperations = new List<string>();
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 1");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 2");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 3");
    dbOperations.Add("insert MyTable values ('new value',1)");
    dbOperations.Add("insert MyTable values ('new value',2)");
    dbOperations.Add("insert MyTable values ('new value',3)");

Hello následující kód ADO.NET postupně provádí tyto operace.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();

        foreach(string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn);
            cmd.ExecuteNonQuery();                   
        }
    }

Hello toooptimize nejlepší způsob, jak tento kód je tooimplement určitou formu klienta dávkování těchto volání. Ale výkonu hello tooincrease jednoduchý způsob tohoto kódu při jednoduše zabalení hello pořadí volání v transakci. Tady je hello stejný kód, který používá transakce.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
        SqlTransaction transaction = conn.BeginTransaction();

        foreach (string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn, transaction);
            cmd.ExecuteNonQuery();
        }

        transaction.Commit();
    }

Transakce se používá ve skutečnosti v obou těchto příkladech. V prvním příkladu Dobrý den každý jednotlivých volání je implicitní transakci. V druhém příkladu hello explicitní transakce zabalí všechny hello volání. Za dokumentaci hello hello [předběžné transakčního protokolu](https://msdn.microsoft.com/library/ms186259.aspx), jsou záznamy protokolu vyprázdněn toohello disku při hello transakce potvrzena. Proto zahrnutím další volání v transakci hello zápisu toohello transakčního protokolu můžete počkat, až je hello transakce potvrzena. V důsledku toho jsou povolení dávkování pro hello zápisy toohello serveru transakčního protokolu.

Hello následující tabulka uvádí některé výsledky testování ad hoc. Hello testy prováděné hello stejné sekvenční vloží s i bez transakce. Pro další perspektivy spustili hello první sada testů vzdáleně z databáze toohello přenosných počítačů v Microsoft Azure. Hello druhá sada testů spustili z cloudové služby a databáze, že oba nacházejí v rámci hello stejné datacenter Microsoft Azure (západní USA). Hello následující tabulka uvádí hello doba v milisekundách sekvenční operace INSERT a bez transakce.

**Místní tooAzure**:

| Operace | Žádná transakce (ms) | Transakce (ms) |
| --- | --- | --- |
| 1 |130 |402 |
| 10 |1208 |1226 |
| 100 |12662 |10395 |
| 1000 |128852 |102917 |

**Azure tooAzure (stejného datového centra)**:

| Operace | Žádná transakce (ms) | Transakce (ms) |
| --- | --- | --- |
| 1 |21 |26 |
| 10 |220 |56 |
| 100 |2145 |341 |
| 1000 |21479 |2756 |

> [!NOTE]
> Výsledky nejsou srovnávacích testů. V tématu hello [Poznámka o výsledcích časování v tomto tématu](#note-about-timing-results-in-this-topic).
> 
> 

Na základě výsledků testu předchozí hello zabalení jedné operace v transakci ve skutečnosti sníží výkon. Ale můžete zvýšit hello počet operací v rámci jedné transakce, zlepšování výkonu hello stane více označen. rozdíly ve výkonnosti Hello je také významnější, když dojde k všechny operace v rámci datového centra hello Microsoft Azure. Hello zvýší latence používání databáze SQL z datového centra Microsoft Azure mimo hello ruší hello výkonnější použití transakcí.

I když hello použití transakcí může zvýšit výkon, pokračovat příliš[sledovat osvědčené postupy pro připojení a transakce](https://msdn.microsoft.com/library/ms187484.aspx). Zachovat hello transakce co nejkratší připojení k databázi možná a zavřít hello po dokončení práce hello. pomocí příkazu v předchozím příkladu hello Hello zaručuje, že hello připojení je ukončeno po dokončení hello následné kód bloku.

Hello předchozí příklad ukazuje, že přidáte kód ADO.NET tooany místní transakce se dvěma řádky. Transakce nabízejí rychlý způsob tooimprove hello výkon kód, který umožňuje sekvenční vložit, aktualizovat a odstranit operace. Ale hello nejrychlejší výkonu, zvažte změnu hello kód další tootake výhod dávkování na straně klienta, jako jsou parametry s hodnotou tabulky.

Další informace o transakcích v ADO.NET naleznete v tématu [místní transakce v ADO.NET](https://docs.microsoft.com/dotnet/framework/data/adonet/local-transactions).

### <a name="table-valued-parameters"></a>Parametry s hodnotou tabulky
Parametry s hodnotou tabulky podporují uživatelem definovaná tabulka typy jako parametry příkazy jazyka Transact-SQL, uložené procedury a funkce. Tato technika dávkování na straně klienta vám umožní toosend více řádků dat v rámci parametr s hodnotou tabulky hello. parametry s hodnotou tabulky toouse nejdříve definovat typ tabulky. Následující příkaz jazyka Transact-SQL Hello vytvoří typ tabulky s názvem **MyTableType**.

    CREATE TYPE MyTableType AS TABLE 
    ( mytext TEXT,
      num INT );


V kódu, můžete vytvořit **DataTable** s hello přesnou stejné názvy a typy hello typ tabulky. To předat **DataTable** parametr v textu dotazu nebo uložené proceduře volání. Hello následující příklad ukazuje, tento postup:

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();

        DataTable table = new DataTable();
        // Add columns and rows. hello following is a simple example.
        table.Columns.Add("mytext", typeof(string));
        table.Columns.Add("num", typeof(int));    
        for (var i = 0; i < 10; i++)
        {
            table.Rows.Add(DateTime.Now.ToString(), DateTime.Now.Millisecond);
        }

        SqlCommand cmd = new SqlCommand(
            "INSERT INTO MyTable(mytext, num) SELECT mytext, num FROM @TestTvp",
            connection);

        cmd.Parameters.Add(
            new SqlParameter()
            {
                ParameterName = "@TestTvp",
                SqlDbType = SqlDbType.Structured,
                TypeName = "MyTableType",
                Value = table,
            });

        cmd.ExecuteNonQuery();
    }

V předchozím příkladu hello hello **SqlCommand** objekt vloží řádky z parametr s hodnotou tabulky  **@TestTvp** . Hello vytvořili **DataTable** objektu je přiřazen toothis parametr s hello **SqlCommand.Parameters.Add** metoda. Dávkování hello vložení v jednom volání výrazně zvyšuje výkon hello přes sekvenční vložení.

tooimprove hello předchozí příklad další, použijte uloženou proceduru místo příkaz založený na textu. Následující příkaz Transact-SQL Hello vytvoří uložené procedury, která přebírá hello **SimpleTestTableType** parametr s hodnotou tabulky.

    CREATE PROCEDURE [dbo].[sp_InsertRows] 
    @TestTvp as MyTableType READONLY
    AS
    BEGIN
    INSERT INTO MyTable(mytext, num) 
    SELECT mytext, num FROM @TestTvp
    END
    GO

Změňte hello **SqlCommand** objektu deklarace v hello předchozí kód příklad toohello následující.

    SqlCommand cmd = new SqlCommand("sp_InsertRows", connection);
    cmd.CommandType = CommandType.StoredProcedure;

Ve většině případů parametry s hodnotou tabulky mít ekvivalentní, nebo lepší výkon než jinými technikami, dávkování. Parametry s hodnotou tabulky jsou často vhodnější, protože jsou flexibilnější než jiné možnosti. Jiné postupy, jako je například hromadného kopírování SQL, například povolit pouze hello vkládání nových řádků. Ale s parametry s hodnotou tabulky, můžete použít logiku v hello uložené procedury toodetermine řádky jsou aktualizace a který se vloží. typ tabulky Hello může být také upravené toocontain sloupec "Operace", která udává, zda text hello zadat řádek by měla být vložit, aktualizovat nebo odstranit.

Hello následující tabulka ukazuje výsledky testů ad-hoc pro použití hello parametry s hodnotou tabulky v milisekundách.

| Operace | Místní tooAzure (ms) | Azure stejného datového centra (ms) |
| --- | --- | --- |
| 1 |124 |32 |
| 10 |131 |25 |
| 100 |338 |51 |
| 1000 |2615 |382 |
| 10000 |23830 |3586 |

> [!NOTE]
> Výsledky nejsou srovnávacích testů. V tématu hello [Poznámka o výsledcích časování v tomto tématu](#note-about-timing-results-in-this-topic).
> 
> 

Hello výkonnější z dávkování je okamžitě zřejmá. V předchozím testu sekvenčních hello 1000 operace trvalo 129 sekund mimo hello datacenter a 21 sekund z v rámci datového centra hello. Ale s parametry s hodnotou tabulky 1000 operace trvat jenom 2.6 sekund mimo datové centrum hello a 0,4 sekundy v rámci datového centra hello.

Další informace o parametry s hodnotou tabulky najdete v tématu [zavolat parametry](https://msdn.microsoft.com/library/bb510489.aspx).

### <a name="sql-bulk-copy"></a>Hromadné kopírování SQL
Hromadné kopírování SQL je jiný způsob tooinsert velké objemy dat do cílové databáze. Aplikace .NET mohou použít hello **SqlBulkCopy** třída tooperform hromadné operace vložení. **SqlBulkCopy** je podobný v funkce toohello nástroj příkazového řádku, **Bcp.exe**, nebo hello příkazu Transact-SQL, **BULK INSERT**. Hello následující příklad kódu ukazuje, jak toobulk kopie hello řádků ve zdroji hello **DataTable**, tabulce, toohello cílové tabulky v systému SQL Server, MyTable.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();

        using (SqlBulkCopy bulkCopy = new SqlBulkCopy(connection))
        {
            bulkCopy.DestinationTableName = "MyTable";
            bulkCopy.ColumnMappings.Add("mytext", "mytext");
            bulkCopy.ColumnMappings.Add("num", "num");
            bulkCopy.WriteToServer(table);
        }
    }

Existují případy, kdy je hromadné kopírování upřednostňované nad parametry s hodnotou tabulky. V tématu hello srovnávací tabulka s hodnotou tabulky parametrů versus BULK INSERT operace v tématu hello [zavolat parametry](https://msdn.microsoft.com/library/bb510489.aspx).

Hello následující výsledky testů ad-hoc zobrazit výkon hello dávkování s **SqlBulkCopy** v milisekundách.

| Operace | Místní tooAzure (ms) | Azure stejného datového centra (ms) |
| --- | --- | --- |
| 1 |433 |57 |
| 10 |441 |32 |
| 100 |636 |53 |
| 1000 |2535 |341 |
| 10000 |21605 |2737 |

> [!NOTE]
> Výsledky nejsou srovnávacích testů. V tématu hello [Poznámka o výsledcích časování v tomto tématu](#note-about-timing-results-in-this-topic).
> 
> 

V menší velikosti dávky parametry s hodnotou tabulky hello použití outperformed hello **SqlBulkCopy** třídy. Ale **SqlBulkCopy** provést 12-31 % rychlejší než parametry s hodnotou tabulky pro testy hello 1 000 a 10 000 řádků. Jako parametry s hodnotou tabulky **SqlBulkCopy** je vhodný pro dávkové vložení, zejména v případě, že porovnání výkonu toohello operací jiný-zpracovat v dávce.

Další informace o hromadné kopírování v ADO.NET naleznete v tématu [operace hromadného kopírování v systému SQL Server](https://msdn.microsoft.com/library/7ek5da1a.aspx).

### <a name="multiple-row-parameterized-insert-statements"></a>Příkazy s parametry vložit více řádků
Jeden alternativní pro malé dávky je tooconstruct velké parametry příkazu INSERT, která vloží více řádků. Hello následující příklad kódu ukazuje tento postup.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();

        string insertCommand = "INSERT INTO [MyTable] ( mytext, num ) " +
            "VALUES (@p1, @p2), (@p3, @p4), (@p5, @p6), (@p7, @p8), (@p9, @p10)";

        SqlCommand cmd = new SqlCommand(insertCommand, connection);

        for (int i = 1; i <= 10; i += 2)
        {
            cmd.Parameters.Add(new SqlParameter("@p" + i.ToString(), "test"));
            cmd.Parameters.Add(new SqlParameter("@p" + (i+1).ToString(), i));
        }

        cmd.ExecuteNonQuery();
    }


V tomto příkladu je určená tooshow hello základní koncept. Realističtější scénář by projít řetězec dotazu hello požadované entity tooconstruct hello a parametry příkazu hello současně. Jste omezeni tooa celkem parametry dotazu 2100, toto nastavení omezuje hello celkový počet řádků, které lze zpracovat tímto způsobem.

Hello následující výsledky testování ad-hoc zobrazit hello výkonu tento typ příkazu insert v milisekundách.

| Operace | Parametry s hodnotou tabulky (ms) | Vložení jedním příkazem (ms) |
| --- | --- | --- |
| 1 |32 |20 |
| 10 |30 |25 |
| 100 |33 |51 |

> [!NOTE]
> Výsledky nejsou srovnávacích testů. V tématu hello [Poznámka o výsledcích časování v tomto tématu](#note-about-timing-results-in-this-topic).
> 
> 

Tento přístup může být mírně rychlejší pro balíků, které jsou menší než 100 řádků. I když zlepšování hello je malá, tento postup je jinou možnost, která může fungovat i ve vašem scénáři konkrétní aplikaci.

### <a name="dataadapter"></a>DataAdapter
Hello **DataAdapter** třída vám umožní toomodify **datovou sadu** objekt a potom odeslat změny hello jako operace INSERT, UPDATE a DELETE. Pokud používáte hello **DataAdapter** tímto způsobem je důležité toonote, který samostatné volání jsou vytvářeny pro každou operaci distinct. tooimprove výkonu, použijte hello **UpdateBatchSize** vlastnost toohello počet operací, které by měl zpracovat v dávce najednou. Další informace najdete v tématu [provádění dávkové operace pomocí DataAdapters](https://msdn.microsoft.com/library/aadf8fk2.aspx).

### <a name="entity-framework"></a>Rozhraní Entity framework
Rozhraní Entity Framework aktuálně nepodporuje dávkování. Různé vývojáři v komunitě hello pokusili toodemonstrate řešení, například přepsání hello **SaveChanges** metoda. Ale hello řešení jsou obvykle toohello komplexní a vlastní aplikace a datového modelu. Hello Entity Framework webu codeplex projekt má aktuálně stránky diskuzi na žádost o této funkce. tooview toto pojednání, najdete v části [poznámky ze schůzky návrhu - 2 srpen 2012](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).

### <a name="xml"></a>XML
Pro úplnost se domníváme, že je důležité tootalk o XML jako strategie dávkování. Použití hello XML má však žádné výhody přes jiné metody a několik nevýhody. Hello přístup je podobné tootable s hodnotou parametry, ale souboru XML nebo řetězec, je předaná tooa uložené procedury místo uživatelem definovaná tabulka. Hello uložené procedury analyzuje hello příkazy v hello uložené procedury.

Existuje několik nevýhody toothis přístup:

* Práce s XML může být náročná a chyba náchylné k chybám.
* Analýza hello XML na hello databáze může být náročná na prostředky procesoru.
* Ve většině případů je tato metoda pomalejší než parametry s hodnotou tabulky.

Z těchto důvodů se nedoporučuje hello použití XML pro dotazy batch.

## <a name="batching-considerations"></a>Dávkování aspekty
Hello následující části obsahují další pokyny pro použití hello dávkování v aplikacích databáze SQL.

### <a name="tradeoffs"></a>Kompromisy
V závislosti na vaší architektury dávkování může zahrnovat kompromis mezi výkon a odolnost. Zvažte například scénář hello, kde vaše role neočekávaně ocitne mimo provoz. Pokud ztratíte jeden řádek dat, dopad hello je menší než hello dopad ztráty velké dávku neodeslané řádků. Existuje větší riziko, pokud vyrovnávací paměť řádky před jejich odesláním toohello databáze v zadaném časovém období.

Z důvodu této kompromis vyhodnoťte hello typ operací, že batch můžete. Batch důkladnějšímu (větší dávky a delší dobu windows) s daty, která je méně kritický.

### <a name="batch-size"></a>Velikost dávky
V našich testech se obvykle žádné výhody toobreaking velké dávky na menší bloky. Ve skutečnosti tento pododdíl často za následek nižší výkon než jeden velký dávky. Zvažte například scénář, kde chcete tooinsert 1 000 řádků. Hello následující tabulka ukazuje, jak dlouho trvalo parametry s hodnotou tabulky toouse, tooinsert 1 000 řádků při rozdělit do menších dávek.

| Velikost dávky | Iterace | Parametry s hodnotou tabulky (ms) |
| --- | --- | --- |
| 1000 |1 |347 |
| 500 |2 |355 |
| 100 |10 |465 |
| 50 |20 |630 |

> [!NOTE]
> Výsledky nejsou srovnávacích testů. V tématu hello [Poznámka o výsledcích časování v tomto tématu](#note-about-timing-results-in-this-topic).
> 
> 

Uvidíte, že nejlepší výkon hello 1 000 řádků je toosubmit všechny najednou. V jiné testy (není tady zobrazené) došlo malé výkonu nárůst toobreak dávce 10000 řádek do dvou dávek 5000. Ale hello schématu tabulky pro tyto testy se poměrně snadno, měli byste provést testy na konkrétních dat a tooverify velikosti dávky tato zjištění.

Jiné tooconsider faktor je, že pokud celkový počet batch hello příliš velká, SQL Database může omezení a odmítnout toocommit hello batch. Nejlepších výsledků dosáhnete hello testovací toodetermine vaše konkrétní scénář, pokud dojde velikost dávky ideální. Zkontrolujte velikost dávky hello konfigurovat v modulu runtime tooenable rychlé úpravy na základě výkonu nebo chyby.

Nakonec vyrovnávat hello velikost dávky hello s hello rizika spojená s dávkování. Pokud nejsou přechodné chyby nebo hello role selže, vezměte v úvahu důsledky hello opakování operace hello nebo ztráty dat hello v dávce hello.

### <a name="parallel-processing"></a>Paralelní zpracování
Co když trvalo hello přístup snižuje velikost dávky hello ale používá více vláken tooexecute hello pracovní? Naše testy znovu, ukázalo, že několik menších vícevláknové dávek zpravidla dělá horší, než jeden větší batch. Hello následující testovací pokusí tooinsert 1 000 řádků v jedné nebo více paralelních dávek. Tento test ukazuje, jak více souběžných dávky ve skutečnosti snížení výkonu.

| Velikost dávky [iterací] | Dva vláken (ms) | Čtyři vláken (ms) | Šest vláken (ms) |
| --- | --- | --- | --- |
| 1000 [1] |277 |315 |266 |
| 500 [2] |548 |278 |256 |
| 250 [4] |405 |329 |265 |
| 100 [10] |488 |439 |391 |

> [!NOTE]
> Výsledky nejsou srovnávacích testů. V tématu hello [Poznámka o výsledcích časování v tomto tématu](#note-about-timing-results-in-this-topic).
> 
> 

Existuje několik možných důvodů pro hello snížení výkonu kvůli tooparallelism:

* Existuje více souběžných sítě volání místo jeden.
* Více operací pro jedinou tabulku může způsobit konflikty a blokování.
* Existují režijní náklady spojené s více vláken.
* Hello výdajů otevření více připojení převáží hello výhodou paralelní zpracování.

Pokud cílíte různých tabulek nebo databází, je možné toosee poklesu výkonu získáte pomocí této strategie. Scénář pro tento postup by horizontálního dělení databáze nebo federace. Horizontálního dělení používá více databází a databázi tooeach různých datových trasy. Pokud každé malé dávky tooa jiné databázi, může být pak paralelní provádění operací hello efektivnější. Ale hello výkonnější není dostatečně významné toouse jako hello základ pro horizontálního dělení rozhodnutí toouse databáze ve vašem řešení.

V některé návrhy můžete paralelní provádění menší dávek způsobit lepší propustnost požadavky v rámci systému zatížení. V takovém případě i když je rychlejší tooprocess jeden větší batch, více listů paralelní zpracování může být efektivnější.

Pokud používáte paralelní provádění, vezměte v úvahu řízení hello maximální počet pracovních vláken. Zmenšete počet může být výsledkem nižší výskyt kolizí a rychlejší dobu provádění. Zvažte také hello další zátěže, které to umístí na hello cílová databáze v připojení a transakce.

### <a name="related-performance-factors"></a>Faktory související výkonu
Dávkování ovlivní také typické pokyny na výkon databáze. Můžete například vložit pro tabulky, které mají velký primární klíč, nebo mnoho neclusterované indexy je snížit výkon.

Pokud parametry s hodnotou tabulky pomocí uložené procedury, můžete použít příkaz hello **SET NOCOUNT ON** od začátku hello hello procedury. Tento příkaz potlačí hello návrat hello počet hello ovlivněných řádků v postupu hello. Ale v testech hello použití **SET NOCOUNT ON** nemělo žádný vliv nebo snížení výkonu. Hello testovací uložené procedury bylo jednoduché s jedním **vložit** příkazu z parametru s hodnotou tabulky hello. Je možné, že by složitější uložené procedury těžit z tohoto prohlášení. Ale Nepředpokládejte, že přidání **SET NOCOUNT ON** tooyour uložené procedury automaticky zvyšuje výkon. toounderstand hello vliv, testovací vaše uložené procedury s i bez hello **SET NOCOUNT ON** příkaz.

## <a name="batching-scenarios"></a>Dávkování scénáře
Hello následující části popisují, jak toouse parametry s hodnotou tabulky třemi způsoby aplikace. Hello první scénář popisuje, jak ukládání do vyrovnávací paměti a dávkování vzájemně spolupracují. Druhý scénář Hello zlepšuje výkon provádění operací s podrobnostmi v jedné uložené procedury volání. Hello konečné scénář ukazuje jak toouse parametry s hodnotou tabulky v operaci "UPSERT".

### <a name="buffering"></a>Ukládání do vyrovnávací paměti
I když je několik scénářů, které jsou zřejmé kandidáta pro dávkování, existuje mnoho scénářů, které může využívat výhod dávkování zpožděné zpracování. Zpožděné zpracování také však představuje větší riziko, že je v případě hello neočekávané selhání ztrátám dat hello. Je důležité toounderstand toto riziko a zvážit důsledky hello.

Představte si třeba webové aplikace, která sleduje hello navigační historii jednotlivých uživatelů. S každým požadavkem stránky může aplikace hello nastavit uživatele databáze volání toorecord hello stránky zobrazení. Ale vyšší výkon a škálovatelnost lze dosáhnout ukládání do vyrovnávací paměti hello uživatelé navigační aktivity a pak odešle tato data toohello databáze v dávkách. Můžete aktivovat aktualizaci databáze hello uplynulý čas nebo velikost vyrovnávací paměti. Pravidlo může například určit, že tento hello batch, měla by být zpracována po 20 sekund nebo když vyrovnávací paměti hello dosáhne 1000 položek.

Hello následující příklad kódu používá [reaktivní rozšíření - Rx](https://msdn.microsoft.com/data/gg577609) tooprocess do vyrovnávací paměti události vyvolané službou třída monitorování. Když hello výplněmi vyrovnávací paměti nebo je dosaženo časového limitu, hello dávku uživatelská data se odesílají toohello databáze s parametr s hodnotou tabulky.

Hello následující NavHistoryData třída modely hello navigační podrobné informace o uživateli. Obsahuje základní informace, jako je například hello uživatelský identifikátor, adresa URL hello přístup a hello doba přístupu k.

    public class NavHistoryData
    {
        public NavHistoryData(int userId, string url, DateTime accessTime)
        { UserId = userId; URL = url; AccessTime = accessTime; }
        public int UserId { get; set; }
        public string URL { get; set; }
        public DateTime AccessTime { get; set; }
    }

Hello NavHistoryDataMonitor třída je zodpovědná za ukládání do vyrovnávací paměti hello uživatele navigační data toohello databáze. Obsahuje metody, RecordUserNavigationEntry, který odpovídá zobrazením **OnAdded** událostí. Hello následující kód ukazuje hello logiku konstruktoru, který používá Rx toocreate kolekci pozorovatelné založené na události hello. Pak přihlásí toothis pozorovatelné kolekce s metodou hello vyrovnávací paměti. přetížení Hello Určuje, že vyrovnávací paměť hello by měly být odeslány každých 20 sekund nebo 1 000 položek.

    public NavHistoryDataMonitor()
    {
        var observableData =
            Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");

        observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
    }

Obslužná rutina Hello převede všechny položky hello uložená do vyrovnávací paměti typu s hodnotou tabulky a pak předá tento typ tooa uložený postup této batch hello procesy. Hello následující kód ukazuje dokončení definice hello hello NavHistoryDataEventArgs i hello NavHistoryDataMonitor třídy.

    public class NavHistoryDataEventArgs : System.EventArgs
    {
        public NavHistoryDataEventArgs(NavHistoryData data) { Data = data; }
        public NavHistoryData Data { get; set; }
    }

    public class NavHistoryDataMonitor
    {
        public event EventHandler<NavHistoryDataEventArgs> OnAdded;

        public NavHistoryDataMonitor()
        {
            var observableData =
                Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");

            observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
        }

        public void RecordUserNavigationEntry(NavHistoryData data)
        {    
            if (OnAdded != null)
                OnAdded(this, new NavHistoryDataEventArgs(data));
        }

        protected void Handler(IList<EventPattern<NavHistoryDataEventArgs>> items)
        {
            DataTable navHistoryBatch = new DataTable("NavigationHistoryBatch");
            navHistoryBatch.Columns.Add("UserId", typeof(int));
            navHistoryBatch.Columns.Add("URL", typeof(string));
            navHistoryBatch.Columns.Add("AccessTime", typeof(DateTime));
            foreach (EventPattern<NavHistoryDataEventArgs> item in items)
            {
                NavHistoryData data = item.EventArgs.Data;
                navHistoryBatch.Rows.Add(data.UserId, data.URL, data.AccessTime);
            }

            using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
            {
                connection.Open();

                SqlCommand cmd = new SqlCommand("sp_RecordUserNavigation", connection);
                cmd.CommandType = CommandType.StoredProcedure;

                cmd.Parameters.Add(
                    new SqlParameter()
                    {
                        ParameterName = "@NavHistoryBatch",
                        SqlDbType = SqlDbType.Structured,
                        TypeName = "NavigationHistoryTableType",
                        Value = navHistoryBatch,
                    });

                cmd.ExecuteNonQuery();
            }
        }
    }

toouse této vyrovnávací paměti třídy hello aplikace vytvoří objekt NavHistoryDataMonitor statické. Pokaždé, když uživatel přistupuje k na stránce aplikace hello volá metodu NavHistoryDataMonitor.RecordUserNavigationEntry hello. ukládání do vyrovnávací paměti logiku Hello pokračuje tootake péče o odesílání tyto položky toohello databáze v dávkách.

### <a name="master-detail"></a>Hlavní podrobností
Parametry s hodnotou tabulky jsou užitečné pro jednoduché scénáře INSERT. Však může být náročnější toobatch vložení, které zahrnují více než jedna tabulka. scénář "hlavního a podrobného" Hello je dobrým příkladem. hlavní tabulka Hello identifikuje hello primární entity. Minimálně jedna tabulka podrobností uložit víc dat o hello entity. V tomto scénáři vynutit relace cizích klíčů relace hello podrobnosti tooa jedinečný hlavní entity. Vezměte v úvahu zjednodušenou verzi PurchaseOrder tabulka a její přidružené OrderDetail tabulkou. Hello následující Transact-SQL vytvoří tabulku PurchaseOrder hello s čtyři sloupce: OrderID, OrderDate, CustomerID a stav.

    CREATE TABLE [dbo].[PurchaseOrder](
    [OrderID] [int] IDENTITY(1,1) NOT NULL,
    [OrderDate] [datetime] NOT NULL,
    [CustomerID] [int] NOT NULL,
    [Status] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrder] 
    PRIMARY KEY CLUSTERED ( [OrderID] ASC ))

Každý pořadí obsahuje jeden nebo více nákupy produktu. Tyto informace se zaznamená v tabulce PurchaseOrderDetail hello. Hello následující Transact-SQL vytvoří hello PurchaseOrderDetail tabulku se sloupci pět: OrderID, OrderDetailID, ProductID, UnitPrice a OrderQty.

    CREATE TABLE [dbo].[PurchaseOrderDetail](
    [OrderID] [int] NOT NULL,
    [OrderDetailID] [int] IDENTITY(1,1) NOT NULL,
    [ProductID] [int] NOT NULL,
    [UnitPrice] [money] NULL,
    [OrderQty] [smallint] NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrderDetail] PRIMARY KEY CLUSTERED 
    ( [OrderID] ASC, [OrderDetailID] ASC ))

Hello OrderID sloupec v tabulce PurchaseOrderDetail hello musíte odkázat pořadí z tabulky PurchaseOrder hello. Následující definice cizího klíče Hello vynucuje toto omezení.

    ALTER TABLE [dbo].[PurchaseOrderDetail]  WITH CHECK ADD 
    CONSTRAINT [FK_OrderID_PurchaseOrder] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[PurchaseOrder] ([OrderID])

V pořadí toouse vracející tabulku parametrů musí mít jeden typ uživatelem definovaná tabulka pro každou cílovou tabulku.

    CREATE TYPE PurchaseOrderTableType AS TABLE 
    ( OrderID INT,
      OrderDate DATETIME,
      CustomerID INT,
      Status NVARCHAR(50) );
    GO

    CREATE TYPE PurchaseOrderDetailTableType AS TABLE 
    ( OrderID INT,
      ProductID INT,
      UnitPrice MONEY,
      OrderQty SMALLINT );
    GO

Poté definujte uložené procedury, která přijímá tabulky z těchto typů. Tento postup umožňuje batch toolocally aplikace sadu objednávek a podrobnosti o pořadí v jediném volání. Hello následující Transact-SQL poskytuje hello deklarace dokončení uložené procedury v tomto příkladu pořadí nákupu.

    CREATE PROCEDURE sp_InsertOrdersBatch (
    @orders as PurchaseOrderTableType READONLY,
    @details as PurchaseOrderDetailTableType READONLY )
    AS
    SET NOCOUNT ON;

    -- Table that connects hello order identifiers in hello @orders
    -- table with hello actual order identifiers in hello PurchaseOrder table
    DECLARE @IdentityLink AS TABLE ( 
    SubmittedKey int, 
    ActualKey int, 
    RowNumber int identity(1,1)
    );

          -- Add new orders toohello PurchaseOrder table, storing hello actual
    -- order identifiers in hello @IdentityLink table   
    INSERT INTO PurchaseOrder ([OrderDate], [CustomerID], [Status])
    OUTPUT inserted.OrderID INTO @IdentityLink (ActualKey)
    SELECT [OrderDate], [CustomerID], [Status] FROM @orders ORDER BY OrderID;

    -- Match hello passed-in order identifiers with hello actual identifiers
    -- and complete hello @IdentityLink table for use with inserting hello details
    WITH OrderedRows As (
    SELECT OrderID, ROW_NUMBER () OVER (ORDER BY OrderID) As RowNumber 
    FROM @orders
    )
    UPDATE @IdentityLink SET SubmittedKey = M.OrderID
    FROM @IdentityLink L JOIN OrderedRows M ON L.RowNumber = M.RowNumber;

    -- Insert hello order details into hello PurchaseOrderDetail table, 
          -- using hello actual order identifiers of hello master table, PurchaseOrder
    INSERT INTO PurchaseOrderDetail (
    [OrderID],
    [ProductID],
    [UnitPrice],
    [OrderQty] )
    SELECT L.ActualKey, D.ProductID, D.UnitPrice, D.OrderQty
    FROM @details D
    JOIN @IdentityLink L ON L.SubmittedKey = D.OrderID;
    GO

V tomto příkladu hello místně definované @IdentityLink ukládá hello skutečnými hodnotami OrderID z hello nově vložené řádky tabulky. Tyto identifikátory pořadí se liší od hello dočasné OrderID hodnoty v hello @orders a @details parametry s hodnotou tabulky. Z tohoto důvodu hello @IdentityLink tabulky pak připojí hello OrderID hodnoty z hello @orders toohello skutečné OrderID hodnoty parametrů pro hello nové řádky v tabulce PurchaseOrder hello. Po provedení tohoto kroku hello @IdentityLink tabulky můžete usnadnit vkládání podrobnosti pořadí hello s hello skutečné OrderID, který splňuje hello omezení cizího klíče.

Tuto uloženou proceduru lze z kódu nebo jiná volání jazyka Transact-SQL. Najdete v části parametry s hodnotou tabulky hello tento dokument příklad kódu. Hello následující Transact-SQL ukazuje, jak toocall hello sp_InsertOrdersBatch.

    declare @orders as PurchaseOrderTableType
    declare @details as PurchaseOrderDetailTableType

    INSERT @orders 
    ([OrderID], [OrderDate], [CustomerID], [Status])
    VALUES(1, '1/1/2013', 1125, 'Complete'),
    (2, '1/13/2013', 348, 'Processing'),
    (3, '1/12/2013', 2504, 'Shipped')

    INSERT @details
    ([OrderID], [ProductID], [UnitPrice], [OrderQty])
    VALUES(1, 10, $11.50, 1),
    (1, 12, $1.58, 1),
    (2, 23, $2.57, 2),
    (3, 4, $10.00, 1)

    exec sp_InsertOrdersBatch @orders, @details

Toto řešení umožňuje každé dávky toouse sadu OrderID hodnot, které začínají znakem 1. Tyto hodnoty dočasné OrderID popisují hello vztahy v dávce hello, ale skutečné hodnoty OrderID hello jsou určeny v době hello operace insert hello. Můžete spustit hello stejné příkazy v předchozím příkladu hello opakovaně a generovat jedinečný objednávky v databázi hello. Z tohoto důvodu je vhodné přidat další kód nebo databáze logiku, která brání duplicitní objednávky při použití tohoto dávkování techniku.

Tento příklad ukazuje, že i složitější databázových operací, jako je například seznam podrobnosti operace, může zpracovat v dávce pomocí parametry s hodnotou tabulky.

### <a name="upsert"></a>UPSERT
Jiné dávkování scénář zahrnuje současně aktualizaci existujících řádků a vkládání nových řádků. Tato operace je někdy označují tooas operace "UPSERT" (aktualizace + insert). Místo provedení samostatné volání tooINSERT a aktualizace, je příkazu MERGE hello nejlépe hodí toothis úloha. Hello příkazu MERGE můžete provést i insert a operace v jednom volání aktualizace.

Parametry s hodnotou tabulky můžete použít s hello SLOUČENÍ příkaz tooperform aktualizace a vkládání. Představte si třeba zjednodušené zaměstnanec tabulku, která obsahuje následující sloupce hello: EmployeeID, FirstName, LastName, SocialSecurityNumber:

    CREATE TABLE [dbo].[Employee](
    [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [SocialSecurityNumber] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_Employee] PRIMARY KEY CLUSTERED 
    ([EmployeeID] ASC ))

V tomto příkladu můžete hello fakt, že tento hello SocialSecurityNumber je jedinečný tooperform SLOUČENÍM několika zaměstnanci. Nejprve vytvořte hello uživatele definovaný typ tabulky:

    CREATE TYPE EmployeeTableType AS TABLE 
    ( Employee_ID INT,
      FirstName NVARCHAR(50),
      LastName NVARCHAR(50),
      SocialSecurityNumber NVARCHAR(50) );
    GO

Dále vytvořte uložené procedury nebo napsat kód, že používá hello SLOUČENÍ příkaz tooperform hello aktualizace a vkládání. Hello následující příklad používá příkazu MERGE hello na parametr s hodnotou tabulky @employees, typu EmployeeTableType. Hello obsah hello @employees tabulky nejsou zobrazeny zde.

    MERGE Employee AS target
    USING (SELECT [FirstName], [LastName], [SocialSecurityNumber] FROM @employees) 
    AS source ([FirstName], [LastName], [SocialSecurityNumber])
    ON (target.[SocialSecurityNumber] = source.[SocialSecurityNumber])
    WHEN MATCHED THEN 
    UPDATE SET
    target.FirstName = source.FirstName, 
    target.LastName = source.LastName
    WHEN NOT MATCHED THEN
       INSERT ([FirstName], [LastName], [SocialSecurityNumber])
       VALUES (source.[FirstName], source.[LastName], source.[SocialSecurityNumber]);

Další informace najdete v dokumentaci hello a příklady příkazu MERGE hello. I když hello pracovní je možné provádět v několika krocích volání uložené procedury s samostatné operace INSERT a UPDATE, je efektivnější hello příkazu MERGE. Kód databáze můžete také vytvořit volání jazyka Transact-SQL, která pomocí příkazu MERGE hello přímo bez nutnosti dvě volání databáze pro příkaz INSERT a UPDATE.

## <a name="recommendation-summary"></a>Souhrnná doporučení
Hello následující seznam obsahuje souhrn hello dávkování doporučení, které jsou popsané v tomto tématu:

* Pomocí ukládání do vyrovnávací paměti a dávkování tooincrease hello výkon a škálovatelnost aplikace SQL Database.
* Pochopení hello kompromisy mezi dávkování nebo ukládání do vyrovnávací paměti a odolnost. Při selhání role vyváží hello riziko ztráty nezpracované dávku důležitých podnikových dat výhodou výkonu hello dávkování.
* Byl proveden pokus tookeep všechny databáze toohello volání v rámci jednoho datového centra tooreduce latence.
* Pokud si zvolíte jednu dávkování techniku, nabízejí parametry s hodnotou tabulky hello optimálního výkonu a flexibility.
* Pro hello nejrychlejší vložit výkonu, postupujte podle následujících obecných pokynů ale otestovat váš scénář:
  * < 100 řádků pomocí jedné parametrizovaného příkaz INSERT.
  * < 1 000 řádků použijte parametry s hodnotou tabulky.
  * Pro > = 1 000 řádků, použijte SqlBulkCopy.
* Pro aktualizaci a operace odstranění, použijte parametry s hodnotou tabulky s logiky uložené procedury, která určuje hello správné operace na každý řádek v tabulce parametru hello.
* Pokyny pro velikost dávky:
  * Použití hello největší batch velikosti, které dávají smysl pro vaše aplikace a podnikových požadavků.
  * Vyrovnávat hello výkonu získáte velké dávek s hello rizika dočasné nebo závažné selhání. Co je důsledkem hello opakování nebo ke ztrátě dat. hello v dávce hello? 
  * Otestujte hello největší batch velikost tooverify, databáze SQL není odmítnout ho.
  * Vytvořte nastavení konfigurace tohoto ovládacího prvku dávkování, jako je například velikost dávky hello nebo hello vyrovnávací paměti časový interval. Tato nastavení poskytují flexibilitu. Hello dávkování chování v produkčním prostředí bez opětovného nasazení hello cloudovou službu, můžete změnit.
* Vyhněte se paralelní zpracování dávek, které působí na jednotlivé tabulky v jedné databáze. Pokud si zvolíte toodivide jedné dávkové napříč několika pracovních vláken, spusťte testy toodetermine hello ideální počet vláken. Po neurčené prahová hodnota další podprocesy bude snížit výkon, a nikoli zvýšit ji.
* Vezměte v úvahu ukládání do vyrovnávací paměti na velikost a čas jako způsob implementace dávkování pro více scénářů.

## <a name="next-steps"></a>Další kroky
Tento článek zaměřuje na tom, jak návrhu databáze a kódování techniky související toobatching může zlepšit výkon aplikace a škálovatelnost. Ale toto je pouze jediný faktor v vaše celková strategie. Další způsoby tooimprove výkon a škálovatelnost, najdete v části [Azure SQL Database – Průvodce výkonem pro izolované databáze](sql-database-performance-guidance.md) a [cenové a výkonové požadavky fondu elastické databáze](sql-database-elastic-pool-guidance.md).

