---
title: "technologie SQL databázi v paměti aaaAzure | Microsoft Docs"
description: "Azure SQL Database v paměti technologie výrazně zlepšit hello výkon transakcí a analýzy zatížení. Zjistěte, jak tootake výhod těchto technologií."
services: sql-database
documentationCenter: 
author: jodebrui
manager: jhubbard
editor: 
ms.assetid: 250ef341-90e5-492f-b075-b4750d237c05
ms.service: sql-database
ms.custom: develop databases
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: jodebrui
ms.openlocfilehash: 1bacd7297b2f9b018853088eabf2a2ee66a9cb43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-performance-by-using-in-memory-technologies-in-sql-database"></a>Optimalizace výkonu pomocí technologie v paměti v databázi SQL

Použití technologií v paměti ve službě Azure SQL Database, můžete dosáhnout zlepšení výkonu pomocí různých úloh: transakcí (online zpracování transakcí (OLTP)), analytics (online analytického zpracování (OLAP)) a ve smíšeném (hybridní transakce/analytického zpracování (HTAP)). Z důvodu hello efektivnější dotazů a zpracování transakcí, v paměti technologie vám také pomoci tooreduce náklady. Obvykle není nutné tooupgrade hello cenová úroveň zvýšení výkonu tooachieve hello databáze. V některých případech i je možné snížit cenovou úroveň, při vylepšení výkonu s technologiemi v paměti se přesto zobrazuje hello.

Zde jsou dva příklady, jak OLTP v paměti pomohl toosignificantly zlepšení výkonu:

- Pomocí OLTP v paměti, [kvora obchodními řešeními bylo možné toodouble jejich zatížení při současném zvyšování Dtu 70 %](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database).
    - DTU znamená *jednotek propustnosti databáze*, a obsahuje měření spotřeby prostředků.
- Hello toto video ukazuje výrazné zlepšení spotřeby prostředků s ukázky pracovního vytížení: [OLTP v paměti v Azure SQL Database Video](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB).
    - Další podrobnosti najdete v tématu hello blogu: [OLTP v paměti v příspěvku blogu databáze SQL Azure](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

Technologie v paměti jsou k dispozici v všechny databáze v úrovni Premium hello, včetně databází v elastické fondy Premium.

Hello následující video vysvětluje potenciální zvýšení výkonu s technologiemi v paměti ve službě Azure SQL Database. Pamatujte, že hello výkonnější, který se zobrazí vždy závisí na mnoha faktorech, včetně hello povaha hello zatížení a data, vzor přístupu hello databáze a tak dále.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-In-Memory-Technologies/player]
>
>

Azure SQL Database má hello následující technologie v paměti:

- *OLTP v paměti* zvyšuje propustnost a snižuje latence pro zpracování transakcí. Scénáře využívající OLTP v paměti jsou: transakce vysokou propustností zpracování například obchodní a hraní, přijímání dat ze zařízení IoT, ukládání do mezipaměti, načtení dat a dočasné tabulky a scénáře proměnné tabulky nebo události.
- *Clusterované indexy columnstore* redukuje vašeho úložiště (až too10 časy) a zlepšení výkonu pro dotazy analýz a generování sestav. Můžete ho použít s tabulky faktů vaše datové Tržiště toofit další data v databázi a zlepšit výkon. Můžete také pomocí historických dat ve vaší tooarchive provozní databáze a být schopný tooquery too10 časů další data.
- *Neclusterovaných indexů columnstore* nápovědu HTAP můžete toogain v reálném čase přehled o svém podnikání prostřednictvím dotazování hello provozní databáze přímo, bez nutnosti toorun hello nákladné extrakce, transformace a načítání (ETL) proces a čekání pro hello datového skladu toobe naplněno. Neclusterovaných indexů columnstore povolí velmi rychlé spuštění analytické dotazy na databáze OLTP hello, a současně hello dopad na hello provozní úlohy.
- Můžete taky nechat hello kombinace paměťově optimalizované tabulky s indexem columnstore. Tato kombinace umožňuje velmi rychlé transakce tooperform zpracování a příliš*souběžně* spuštění analytics dotazuje velmi rychle na hello stejná data.

Indexy columnstore a OLTP v paměti se součástí produktu SQL Server hello od 2012 a 2014, v uvedeném pořadí. Azure SQL Database a SQL Server hello sdílet stejné implementaci technologií v paměti. Do budoucna, nové funkce pro tyto technologie jsou vydávány v Azure SQL Database nejprve před jejich vydání v systému SQL Server.

Toto téma popisuje aspekty OLTP v paměti a columnstore indexy, které jsou specifické tooAzure databáze SQL a také zahrnuje ukázky:
- Uvidíte hello dopad těchto technologií na omezení velikosti úložiště a data.
- Uvidíte, jak toomanage hello pohybů databází, které používají tyto technologie mezi hello jiných cenových úrovní.
- Zobrazí se dvou vzorcích, které ilustrují použití hello OLTP v paměti, jakož i indexy columnstore ve službě Azure SQL Database.

V tématu hello následující prostředky pro další informace.

Podrobné informace o technologiích hello:

- [Přehled OLTP v paměti a scénáře použití](https://msdn.microsoft.com/library/mt774593.aspx) (zahrnuje odkazy toocustomer případové studie a informace o tooget spuštění)
- [Dokumentace pro OLTP v paměti](http://msdn.microsoft.com/library/dn133186.aspx)
- [Průvodce indexy Columnstore](https://msdn.microsoft.com/library/gg492088.aspx)
- Hybridní transakcí nebo analytického zpracování (HTAP), také známé jako [provozní analýzu v reálném čase](https://msdn.microsoft.com/library/dn817827.aspx)

Rychlý úvod do na OLTP v paměti: [rychlý Start 1: technologie OLTP v paměti pro rychlejší T-SQL výkonu](http://msdn.microsoft.com/library/mt694156.aspx) (jiný článek toohelp začnete)

Podrobný videa o technologiích hello:

- [OLTP v paměti ve službě Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB) (která obsahuje ukázku výkonu výhody a kroky tooreproduce těchto výsledků sami)
- [Videa OLTP v paměti: Co je a když/jak toouse ho](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/03/in-memory-oltp-video-what-it-is-and-whenhow-to-use-it/)
- [Columnstore Index: Analýzy v paměti videa z webu Ignite 2016](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/04/columnstore-index-in-memory-analytics-i-e-columnstore-index-videos-from-ignite-2016/)

## <a name="storage-and-data-size"></a>Velikost úložiště a dat.

### <a name="data-size-and-storage-cap-for-in-memory-oltp"></a>Limitu velikosti a úložiště dat pro OLTP v paměti

OLTP v paměti obsahuje paměťově optimalizované tabulky, které se používají k ukládání dat uživatele. Tyhle tabulky jsou požadované toofit v paměti. Vzhledem k tomu, že budete spravovat přímo paměti hello služba SQL Database, máme hello konceptu kvótu pro data uživatelů. Tento nápad je odkazované tooas *OLTP v paměti úložiště*.

Každý podporovaný samostatná databáze cenová úroveň a každý elastického fondu cenová úroveň zahrnuje množství OLTP v paměti úložiště. V době psaní textu hello zobrazí gigabajt úložiště pro každý 125 jednotky transakcí databáze (Dtu) nebo elastické databáze jednotky transakcí (Edtu).

Hello [úrovně služeb SQL Database](sql-database-service-tiers.md) článek obsahuje seznam oficiální hello úložiště hello OLTP v paměti, která je k dispozici pro všechny podporované samostatná databáze a elastického fondu cenová úroveň.

Hello následující počet položek směrem k vaší úložiště cap OLTP v paměti:

- Řádky dat aktivního uživatele v paměťově optimalizované tabulky a proměnné tabulek. Všimněte si, že nemáte staré verze řádku započítávat hello zakončení.
- Indexy v paměťově optimalizovaných tabulkách.
- Provozní režie operací ALTER TABLE.

Při dosažení limitu hello, obdržíte chybu na více systémů kvóty a již není možné data tooinsert nebo aktualizace. toomitigate odstranit data této chybě, nebo zvyšte hello cenová úroveň databáze hello nebo fond.

Podrobnosti o monitorování využití úložiště OLTP v paměti a konfiguraci výstrahy, když téměř dosáhl limitu hello najdete v tématu [monitorování v paměti úložiště](sql-database-in-memory-oltp-monitoring.md).

#### <a name="about-elastic-pools"></a>O elastické fondy

S elastické fondy hello OLTP v paměti úložiště sdílet všechny databáze ve fondu hello. Proto hello využití v jedné databáze může potenciálně ovlivnit jiné databáze. Jsou dvě jejich zmírnění pro toto:

- Nakonfigurujte maximální-počet jednotek eDTU pro databáze, která je nižší než počet hello eDTU pro fond hello jako celek. Tento maximální caps hello využití úložiště OLTP v paměti, všechny databáze ve fondu hello, toohello velikost, která odpovídá počet eDTU toohello.
- Nakonfigurujte Min eDTU, která je větší než 0. Tato minimální záruky, zda má každý databáze ve fondu hello hello množství dostupného úložiště OLTP v paměti, která odpovídá toohello nakonfigurovat Min eDTU.

### <a name="data-size-and-storage-for-columnstore-indexes"></a>Velikost dat a úložiště pro indexy columnstore

Indexy Columnstore nejsou požadované toofit v paměti. Proto hello pouze zakončení na velikosti hello hello indexů je hello maximální celkovou velikost databáze, která je popsána v hello [úrovně služeb SQL Database](sql-database-service-tiers.md) článku.

Při použití Clusterované indexy columnstore sloupcovém komprese se používá pro hello základní tabulka úložiště. Tato komprese může výrazně redukuje hello úložiště uživatelských dat, což znamená, že můžete začlenit další data v databázi hello. A komprese hello může být zvýšena další s [sloupcovém archivace komprese](https://msdn.microsoft.com/library/cc280449.aspx#Using Columnstore and Columnstore Archive Compression). Hello množství komprese, můžete dosáhnout závisí na povaze hello hello dat, ale 10krát hello komprese není.

Například pokud máte databázi s maximální velikostí 1 terabajt (TB) a dosáhnout 10krát hello komprese pomocí indexy columnstore, můžete začlenit celkem 10 TB dat uživatele v databázi hello.

Při použití neclusterovaných indexů columnstore hello základní tabulka je pořád uložená ve formátu tradiční rowstore hello. Proto nejsou úspory úložiště hello stejnou velikost jako s Clusterované indexy columnstore. Pokud chcete nahradit počet tradiční neclusterované indexy s indexem columnstore jeden, uvidíte stále celkové úspory v hello nároků úložiště pro tabulku hello.

## <a name="moving-databases-that-use-in-memory-technologies-between-pricing-tiers"></a>Přesunutí databází, které používají technologie v paměti mezi cenové úrovně

Nejsou nikdy žádné nekompatibility nebo jiné problémy při upgradu tooa vyšší cenové úrovně, jako třeba ze standardní tooPremium. k dispozici funkce Hello a prostředky pouze zvýšit.

Ale přechod na starší verzi hello cenová úroveň může mít negativní vliv na vaši databázi. dopad Hello je obzvláště zřejmá při downgradovat z Premium tooStandard nebo Basic, když databáze obsahuje objekty OLTP v paměti. Paměťově optimalizované tabulky a indexy columnstore jsou k dispozici po hello přechod na starší verzi (i v případě, že zůstanou viditelné). Dobrý den, platí stejné aspekty při jste snížení hello cenová úroveň fondu elastické databáze nebo přesunutí databáze s technologiemi v paměti do Standard a Basic elastického fondu.

### <a name="in-memory-oltp"></a>OLTP v paměti

*Přechod na starší verzi tooBasic a standardní*: OLTP v paměti není podporována v databázích v hello úroveň Standard nebo Basic. Kromě toho není možné toomove databáze, která obsahuje všechny OLTP v paměti objekty toohello úroveň Standard nebo Basic.

Předtím, než jste starší verzi databáze hello tooStandard a základní, odeberte všechny paměťově optimalizované tabulky a typů tabulek, jakož i všechny nativně Kompilované moduly T-SQL.

Není toounderstand programový způsob, jestli na danou databázi podporuje OLTP v paměti. Můžete provést hello následující jazyka Transact-SQL:

```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```

Pokud hello dotaz vrátí **1**, OLTP v paměti je podporováno v této databázi.


*Přechod na starší verzi nižší úroveň Premium tooa*: Data v paměťově optimalizovaných tabulkách musí být přizpůsobena úložiště hello OLTP v paměti, které souvisí s hello cenová úroveň databáze hello nebo není k dispozici hello elastického fondu. Pokud zkuste toolower hello cenová úroveň nebo přesunutí hello databáze ve fondu, který nemá dostatek dostupného úložiště OLTP v paměti, hello operace selže.

### <a name="columnstore-indexes"></a>Indexy Columnstore

*Přechod na starší verzi Standard nebo tooBasic*: Columnstore indexy jsou podporovány pouze na cenová úroveň Premium hello a ne v hello Standard nebo Basic vrstev. Když jste starší verzi databáze tooStandard nebo Basic, stane se indexu columnstore není k dispozici. Hello systém uchovává indexu columnstore, ale nikdy využívá hello index. Pokud později upgradujete tooPremium zpět, je indexu columnstore okamžitě připraven toobe využít znovu.

Pokud máte **clusterové** columnstore index, celé tabulky hello nedostupný po vrstvy přechod na starší verzi. Proto doporučujeme vyřaďte všechny *clusterové* indexy columnstore před downgradovat databáze nižší než úroveň Premium hello.

*Přechod na starší verzi nižší úroveň Premium tooa*: Tento přechod na starší verzi úspěšná, pokud odpovídá hello celé databáze v rámci hello maximální velikost databáze pro cíl hello cenovou úroveň, nebo dostupné úložiště hello v hello elastického fondu. Neexistuje žádná konkrétní vlivu indexy columnstore hello.


<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="1-install-hello-in-memory-oltp-sample"></a>1. Instalace ukázkové hello OLTP v paměti

Ukázkové databáze AdventureWorksLT hello můžete vytvořit pomocí několika kliknutí v hello [portál Azure](https://portal.azure.com/). Potom hello kroky v této části popisují, jak můžete rozšířit vaše databáze AdventureWorksLT s objekty OLTP v paměti a předvedení výkonnostních výhod.

Více zneužívající vlastností prohlížeče, ale vizuálně výkonu ukázku pro OLTP v paměti najdete v části:

- Verze: [v – paměť oltp-demo-verze 1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)
- Zdrojový kód: [in-memory-oltp-demo-source-code](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/in-memory/ticket-reservations)

#### <a name="installation-steps"></a>Postup instalace

1. V hello [portál Azure](https://portal.azure.com/), vytvořte na serveru databáze Premium. Sada hello **zdroj** ukázkové databáze AdventureWorksLT toohello. Podrobné pokyny najdete v tématu [vytvořit svoji první databázi Azure SQL](sql-database-get-started-portal.md).

2. Připojit databáze toohello s SQL Server Management Studio [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx).

3. Kopírování hello [OLTP v paměti jazyka Transact-SQL skriptu](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) tooyour schránky. Hello skriptu T-SQL vytvoří hello objekty nezbytné v paměti v ukázkové databáze AdventureWorksLT hello, kterou jste vytvořili v kroku 1.

4. Vložit do aplikace SSMS hello skriptu T-SQL a pak spusťte skript hello. Hello `MEMORY_OPTIMIZED = ON` příkazy CREATE TABLE klauzule jsou zásadní. Například:


```
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a>Chyba 40536


Pokud se zobrazí chyba 40536 při spuštění skriptu hello T-SQL, spusťte hello následující tooverify skriptu T-SQL, zda text hello databáze podporuje v paměti:


```
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


Důsledkem **0** znamená, že v paměti není podporován, a **1** znamená, že je podporovaná. toodiagnose hello problém, ujistěte se, že hello je databáze na úroveň služeb Premium hello.


#### <a name="about-hello-created-memory-optimized-items"></a>O hello vytvořili paměťově optimalizované položky

**Tabulky**: Ukázka hello obsahuje hello následující paměťově optimalizované tabulky:

- SalesLT.Product_inmem
- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem
- Demo.DemoSalesOrderHeaderSeed
- Demo.DemoSalesOrderDetailSeed


Si můžete prohlédnout paměťově optimalizované tabulky prostřednictvím hello **Průzkumník objektů** v aplikaci SSMS. Klikněte pravým tlačítkem na **tabulky** > **filtru** > **nastavení filtru** > **je paměťově optimalizovaná**. Hello hodnota se rovná 1.


Nebo můžete dát dotaz na zobrazení katalogu hello, jako například:


```
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


**Nativně kompilované uložené procedury**: SalesLT.usp_InsertSalesOrder_inmem si můžete prohlédnout pomocí zobrazení katalogu dotazu:


```
SELECT uses_native_compilation, OBJECT_NAME(object_id), definition
    FROM sys.sql_modules
    WHERE uses_native_compilation = 1;
```


&nbsp;

### <a name="run-hello-sample-oltp-workload"></a>Spustit hello ukázky OLTP pracovního vytížení

Hello jenom rozdíl mezi hello následující dva *uložené procedury* je první postup hello používá verzích paměťově optimalizované tabulky hello při hello druhý postup používá hello regulární tabulky na disku:

- SalesLT**.** usp_InsertSalesOrder**_inmem**
- SalesLT**.** usp_InsertSalesOrder**_ondisk**


V této části, uvidíte, jak hello užitečný toouse **ostress.exe** tooexecute nástroj hello dvě uložené procedury na stressful úrovních. Jak dlouho trvá pro hello dva přízvuk spustí toofinish, můžete porovnat.


Když spustíte ostress.exe, doporučujeme předat hodnoty parametrů, které jsou určené pro obě hello následující:

- Spustit velký počet souběžných připojení pomocí - n100.
- Pomocí mít každý připojení smyčku stovky dobu, pomocí - r500.


Můžete ale chtít toostart s mnohem menšími hodnoty jako - n10 a - r 50 tooensure, který vše funguje.


### <a name="script-for-ostressexe"></a>Skript pro ostress.exe


V této části zobrazí skript hello T-SQL, který je vložen v našem ostress.exe příkazového řádku. skript Hello používá položky, které byly vytvořeny hello skriptu T-SQL, který jste dříve nainstalovali.


Hello následující skript vloží ukázka prodejní objednávky s pěti položek řádku do následující hello paměťově optimalizované *tabulky*:

- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem


```
DECLARE
    @i int = 0,
    @od SalesLT.SalesOrderDetailType_inmem,
    @SalesOrderID int,
    @DueDate datetime2 = sysdatetime(),
    @CustomerID int = rand() * 8000,
    @BillToAddressID int = rand() * 10000,
    @ShipToAddressID int = rand() * 10000;

INSERT INTO @od
    SELECT OrderQty, ProductID
    FROM Demo.DemoSalesOrderDetailSeed
    WHERE OrderID= cast((rand()*60) as int);

WHILE (@i < 20)
begin;
    EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT,
        @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od;
    SET @i = @i + 1;
end
```


toomake hello *_ondisk* verzi hello předchozí skriptu T-SQL pro ostress.exe, měli byste nahradit oba výskyty hello *_inmem* substring s *_ondisk*. Tyto náhrady ovlivnit hello názvy tabulek a uložených procedur.


### <a name="install-rml-utilities-and-ostress"></a>Instalace nástrojů RML a ostress


V ideálním případě by plánování toorun ostress.exe na virtuální počítač Azure (VM). Měli byste vytvořit [virtuálního počítače Azure](https://azure.microsoft.com/documentation/services/virtual-machines/) v hello stejné Azure geografické oblasti, kde se nachází databáze AdventureWorksLT. Ale ostress.exe na svém přenosném počítači může spouštět místo.


Na hello virtuálního počítače nebo na ať hostitele, můžete zvolit, nainstalujte nástroje hello opětovného přehrání Markup Language (RML). Hello nástroje zahrnují ostress.exe.

Další informace naleznete v tématu:
- Hello ostress.exe diskuse ve [ukázkové databáze pro OLTP v paměti](http://msdn.microsoft.com/library/mt465764.aspx).
- [Ukázkové databáze pro OLTP v paměti](http://msdn.microsoft.com/library/mt465764.aspx).
- Hello [blogu pro instalaci ostress.exe](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx).



<!--
dn511655.aspx is for SQL 2014,
[Extensions tooAdventureWorks tooDemonstrate In-Memory OLTP]
(http://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(http://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-hello-inmem-stress-workload-first"></a>Spustit hello *_inmem* nejprve vystavila zátěži pracovního vytížení


Můžete použít *RML Cmd výzva* okno toorun naše ostress.exe příkazového řádku. Parametry příkazového řádku Hello přímé ostress na:

- Připojení 100 běželo (-n100).
- Každé připojení 50 času spuštění skriptu T-SQL hello (-r 50).


```
ostress.exe -n100 -r50 -S<servername>.database.windows.net -U<login> -P<password> -d<database> -q -Q"DECLARE @i int = 0, @od SalesLT.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand()* 10000; INSERT INTO @od SELECT OrderQty, ProductID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*60) as int); WHILE (@i < 20) begin; EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od; set @i += 1; end"
```


hello toorun předcházející ostress.exe příkazového řádku:


1. Resetování hello databáze data obsah tak, že spustíte následující příkaz v aplikaci SSMS, toodelete hello všechna hello data, která byla vložená všech předchozích spuštění:

    ``` tsql
    EXECUTE Demo.usp_DemoReset;
    ```

2. Zkopírujte text hello hello předcházející schránky tooyour ostress.exe příkazového řádku.

3. Nahraďte hello `<placeholders>` pro hello parametry -S - U -P -d s hello opravte skutečné hodnoty.

4. V okně RML Cmd spusťte do upravená příkazového řádku.


#### <a name="result-is-a-duration"></a>Výsledkem je, doba trvání


Po dokončení ostress.exe zapíše hello spustit trvání jako jeho poslední řádek výstupu v okně RML Cmd hello. Například kratší testovací běh už bylo asi 1,5 minuty:

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-ondisk-then-rerun"></a>Resetovat, upravit pro *_ondisk*, pak znovu spusťte


Až budete mít hello výsledek z hello *_inmem* spustit, proveďte následující kroky pro hello hello *_ondisk* spustit:


1. Obnovení databáze hello tak, že spustíte následující příkaz v aplikaci SSMS toodelete hello všechna hello data, která byla vložená hello předchozí spustit:
```
EXECUTE Demo.usp_DemoReset;
```

2. Upravit hello ostress.exe příkazového řádku tooreplace všechny *_inmem* s *_ondisk*.

3. Znovu spustit ostress.exe pro hello ještě jednou a zachycení hello trvání výsledek.

4. Znovu obnovte databáze hello (pro odstranění jeho zodpovědné, může být velký objem dat test).


#### <a name="expected-comparison-results"></a>Očekávaný porovnání výsledků

Naše testy v paměti ukázaly, že výkonu vylepšené podle **devětkrát** pro tuto úlohu zneužívající vlastností prohlížeče s ostress systémem virtuálního počítače Azure v hello stejné oblasti Azure jako hello databáze.

<a id="install_analytics_manuallink" name="install_analytics_manuallink"></a>

&nbsp;

## <a name="2-install-hello-in-memory-analytics-sample"></a>2. Instalace ukázkové hello analýzy v paměti


V této části můžete porovnat hello vstupně-výstupní operace a statistiky výsledky při použití index columnstore a index tradiční b stromu.


Pro analýzu v reálném čase na úloh s online zpracováním je často nejlepším toouse neclusterovaný index columnstore. Podrobnosti najdete v tématu [popsané indexy Columnstore](http://msdn.microsoft.com/library/gg492088.aspx).



### <a name="prepare-hello-columnstore-analytics-test"></a>Příprava hello columnstore analytics testu


1. Pomocí portálu Azure toocreate čerstvé databáze AdventureWorksLT z ukázkové hello hello.
 - Použijte tento přesný název.
 - Vyberte všechny úroveň služeb Premium.

2. Kopírování hello [sql_in memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) tooyour schránky.
 - Hello skriptu T-SQL vytvoří hello objekty nezbytné v paměti v ukázkové databáze AdventureWorksLT hello, kterou jste vytvořili v kroku 1.
 - skript Hello vytvoří hello tabulce dimenzí a dvě tabulky faktů. tabulky faktů Hello se naplní 3.5 milionu řádků.
 - skript Hello může trvat 15 minut toocomplete.

3. Vložit do aplikace SSMS hello skriptu T-SQL a pak spusťte skript hello. Hello **COLUMNSTORE** – klíčové slovo v hello **CREATE INDEX** údajů je velmi důležitý, stejně jako na:<br/>`CREATE NONCLUSTERED COLUMNSTORE INDEX ...;`

4. Nastavení úrovně toocompatibility AdventureWorksLT 130:<br/>`ALTER DATABASE AdventureworksLT SET compatibility_level = 130;`

    Úroveň 130 není funkce přímo související tooIn paměti. Ale úroveň 130 obecně poskytuje vyšší výkon dotazu než 120.


#### <a name="key-tables-and-columnstore-indexes"></a>Klíče tabulky a indexy columnstore


- dbo. Tabulka, která má clusterovaný index columnstore, která obsahuje rozšířené komprese na hello je FactResellerSalesXL_CCI *data* úroveň.

- dbo. Tabulka, která má ekvivalentní regulární clusterovaný index, který se komprimují jenom na hello je FactResellerSalesXL_PageCompressed *stránky* úroveň.


#### <a name="key-queries-toocompare-hello-columnstore-index"></a>Index columnstore hello toocompare klíče dotazů


Existují [několik typů dotazu T-SQL, které můžete spustit](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) toosee vylepšení výkonu. V kroku 2 hello skriptu T-SQL věnujte pozornost toothis pár dotazů. Liší se pouze na jednom řádku:


- `FROM FactResellerSalesXL_PageCompressed a`
- `FROM FactResellerSalesXL_CCI a`


Clusterovaný index columnstore je v hello FactResellerSalesXL\_KÚS tabulky.

Hello následující výňatek ze skriptu T-SQL vytiskne statistiky pro vstupně-výstupní operace a čas pro dotaz hello každé tabulky.


```
/*********************************************************************
Step 2 -- Overview
-- Page Compressed BTree table v/s Columnstore table performance differences
-- Enable actual Query Plan in order toosee Plan differences when Executing
*/
-- Ensure Database is in 130 compatibility mode
ALTER DATABASE AdventureworksLT SET compatibility_level = 130
GO

-- Execute a typical query that joins hello Fact Table with dimension tables
-- Note this query will run on hello Page Compressed table, Note down hello time
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO

SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_PageCompressed a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO
SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO


-- This is hello same Prior query on a table with a clustered columnstore index CCI
-- hello comparison numbers are even more dramatic hello larger hello table is (this is an 11 million row table only)
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO
SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_CCI a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO

SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO
```

V databázi s hello P2 cenovou úroveň bude pravděpodobně přibližně devětkrát hello výkonnější pro tento dotaz pomocí hello clusterovaný index columnstore v porovnání s tradiční index hello. S P15 můžete očekávat o 57 časy hello výkonnější pomocí hello columnstore index.



## <a name="next-steps"></a>Další kroky

- [Rychlý Start 1: Technologie OLTP v paměti pro dosažení vyššího výkonu T-SQL](http://msdn.microsoft.com/library/mt694156.aspx)

- [Použití OLTP v paměti v aplikaci existující Azure SQL](sql-database-in-memory-oltp-migration.md)

- [Monitorování OLTP v paměti úložiště](sql-database-in-memory-oltp-monitoring.md) pro OLTP v paměti


## <a name="additional-resources"></a>Další zdroje

#### <a name="deeper-information"></a>Podrobnější informace.

- [Zjistěte, jak kvora zdvojnásobí klíče databáze zatížení při snížení DTU 70 % s OLTP v paměti v databázi SQL](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)

- [OLTP v paměti v příspěvku na blogu databáze Azure SQL](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

- [Další informace o OLTP v paměti](http://msdn.microsoft.com/library/dn133186.aspx)

- [Další informace o indexy columnstore](https://msdn.microsoft.com/library/gg492088.aspx)

- [Další informace o provozní analýzu v reálném čase](http://msdn.microsoft.com/library/dn817827.aspx)

- V tématu [vzory běžné úlohy a posouzení migrace](http://msdn.microsoft.com/library/dn673538.aspx) (které popisuje vzory úlohy, kde OLTP v paměti běžně nabízí výrazné zvýšení výkonu)

#### <a name="application-design"></a>Návrh aplikace

- [Paměť OLTP (v paměti optimalizace)](http://msdn.microsoft.com/library/dn133186.aspx)

- [Použití OLTP v paměti v aplikaci existující Azure SQL](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a>Nástroje

- [Azure Portal](https://portal.azure.com/)

- [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)

- [SQL Server Data Tools (SSDT)](http://msdn.microsoft.com/library/mt204009.aspx)
