---
title: "aaaManage historických dat v dočasných tabulek se zásady uchovávání informací | Microsoft Docs"
description: "Zjistěte, jak toouse dočasné uchování zásad tookeep historických dat pod kontrolou."
services: sql-database
documentationcenter: 
author: bonova
manager: drasumic
editor: 
ms.assetid: 76cfa06a-e758-453e-942c-9f1ed6a38c2a
ms.service: sql-database
ms.custom: develop databases
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: sql-database
ms.date: 10/12/2016
ms.author: bonova
ms.openlocfilehash: a72a6111a6cd7322d734d08bf3852e95f5ffea8b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-historical-data-in-temporal-tables-with-retention-policy"></a>Správa historických dat v dočasných tabulek se zásady uchovávání informací
Dočasné tabulky může zvýšit velikost databáze více než regulérních tabulkách, zejména v případě, že zachováte historických dat pro delší časové období. Proto zásady uchovávání historických dat je důležitým aspektem plánování a správě životního cyklu hello každých dočasné tabulky. Dočasné tabulky v databázi SQL Azure se dodávají s snadno použitelné uchování mechanismus, který umožňuje provedení této úlohy.

Uchování dočasné historie lze konfigurovat na úrovni hello jednotlivé tabulky, což umožňuje uživatelům, které toocreate flexibilní stárnutí zásady. Použití dočasné uchování je jednoduchý: vyžaduje jenom jeden parametr toobe nastavit během změny schématu nebo vytvoření tabulky.

Po definování zásad uchovávání informací, Azure SQL Database spustí pravidelně kontroluje, zda existují historických řádků, které jsou způsobilé pro automatické čištění. Identifikace odpovídající řádky a jejich odebrání z tabulky historie hello dojít transparentně, úlohy na pozadí hello naplánovat a spustit hello systémem. Stáří podmínku pro řádky tabulky historie hello je zaškrtnuta možnost podle sloupce hello představující konec období SYSTEM_TIME. Pokud doba uchování, je třeba nastavit toosix měsíců, řádky tabulky, které jsou vhodné pro čištění uspokojení hello následující podmínky:

````
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
````

V předchozím příkladu hello, budeme předpokládat **ValidTo** sloupec odpovídá toohello konec období SYSTEM_TIME.

## <a name="how-tooconfigure-retention-policy"></a>Jak zásady uchovávání informací tooconfigure?
Než začnete konfigurovat zásady uchovávání informací pro dočasnou tabulku, proveďte nejprve kontrolu zda je povoleno dočasné uchovávání historických *na úrovni databáze hello*.

````
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
````

Databáze příznak **is_temporal_history_retention_enabled** je sada tooON ve výchozím nastavení, ale uživatelé mohou změnit pomocí příkazu ALTER DATABASE. Je také automaticky sadu tooOFF po [obnovení bodu v čase](sql-database-recovery-using-backups.md) operaci. tooenable dočasné uchování vyčištění pro vaši databázi, spusťte následující příkaz hello:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

> [!IMPORTANT]
> Uchování pro dočasné tabulky, i když můžete nakonfigurovat **is_temporal_history_retention_enabled** je VYPNUTÝ, ale není v takovém případě aktivuje automatické čištění zastaralá řádků.
> 
> 

Zásady uchovávání informací je nakonfigurované během vytváření tabulky zadáním hodnoty pro parametr HISTORY_RETENTION_PERIOD hello:

````
CREATE TABLE dbo.WebsiteUserInfo
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH
 (
     SYSTEM_VERSIONING = ON
     (
        HISTORY_TABLE = dbo.WebsiteUserInfoHistory,
        HISTORY_RETENTION_PERIOD = 6 MONTHS
     )
 );
````

Databáze SQL Azure můžete dobu uchování toospecify pomocí různých časových jednotek: počet dnů, týdnů, měsíců a let. Pokud je vynechán HISTORY_RETENTION_PERIOD, se předpokládá NEKONEČNÉ uchování. Můžete taky NEKONEČNÉ – klíčové slovo explicitně.

V některých scénářích může být vhodné tooconfigure uchování po vytvoření tabulky nebo toochange dříve nakonfigurované hodnotu. V takovém případě použijte příkaz ALTER TABLE:

````
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
````

> [!IMPORTANT]
> Nastavení SYSTEM_VERSIONING tooOFF *nezachovává* hodnotu doby uchování. Nastavení SYSTEM_VERSIONING tooON bez HISTORY_RETENTION_PERIOD explicitně zadat výsledky v hello NEKONEČNOU dobu uchování.
> 
> 

tooreview aktuální stav hello zásady uchovávání informací, použijte následující hello dotaz tento příznak spojení dočasné uchování povolování na úrovni databáze hello s dob uchování u jednotlivých tabulek:

````
SELECT DB.is_temporal_history_retention_enabled,
SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema,
T1.name as TemporalTableName,  SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema,
T2.name as HistoryTableName,T1.history_retention_period,
T1.history_retention_period_unit_desc
FROM sys.tables T1  
OUTER APPLY (select is_temporal_history_retention_enabled from sys.databases
where name = DB_NAME()) AS DB
LEFT JOIN sys.tables T2   
ON T1.history_table_id = T2.object_id WHERE T1.temporal_type = 2
````


## <a name="how-sql-database-deletes-aged-rows"></a>Jak SQL Database odstraní stará řádky?
proces vyčištění Hello závisí na hello index rozložení tabulky historie hello. Je důležité toonotice, *pouze tabulky historie clusterovaný index (B-stromu nebo columnstore) může mít omezený uchování zásady nakonfigurované*. Úlohy na pozadí se vytvoří tooperform Vyčistit stará data pro všechny dočasné tabulky s dobou uchování omezené.
Vyčistit logiku pro clusterovaný index rowstore (B-stromu) hello odstraní stará řádek v menší bloky dat (až too10K) minimalizovat tlak na protokol databáze a vstupně-výstupních operací subsystému. I když čištění logiku využívá vyžaduje index B-stromu, pořadí odstranění pro starší než doba uchování dat se nedá zaručit pevně hello řádky. Proto *nepřebírají žádné závislostí v pořadí čištění hello ve svých aplikacích*.

Hello úlohu čištění pro columnstore hello clusteru odebere celý [řádek skupiny](https://msdn.microsoft.com/library/gg492088.aspx) najednou (obvykle obsahují 1 milionu řádků každý), což je velmi efektivní, zejména v případě, že historická data se generuje vysoký tempem.

![Clusterované columnstore uchování](./media/sql-database-temporal-tables-retention-policy/cciretention.png)

Komprese dat vynikající a efektivní uchování čištění díky clusterovaný columnstore index ideální volbou pro scénáře, když vaše úlohy generují rychle vysoký objem historická data. Tento vzor je typické pro náročné na prostředky [úlohy zpracování transakcí, které používají dočasné tabulky](https://msdn.microsoft.com/library/mt631669.aspx) pro sledování změn a auditování, analýza trendu nebo přijímání dat IoT.

## <a name="index-considerations"></a>Aspekty indexu
úloha vyčištění Hello u tabulek s clusterovaný index rowstore vyžaduje toostart index s hello sloupec odpovídající hello koncem období SYSTEM_TIME. Pokud takový indexu neexistuje, nelze nakonfigurovat doby uchování konečné:

*Msg 13765, Level 16, State 1 <br> </br> nastavení konečné uchovávají se na dočasné tabulce se systémovou správou verzí temporalstagetestdb.dbo.WebsiteUserInfo, protože tabulka historie hello. temporalstagetestdb.dbo.WebsiteUserInfoHistory' neobsahuje požadované clusterovaný index. Zvažte vytvoření clusteru columnstore nebo B-stromu index začínající hello sloupec, který odpovídá konec SYSTEM_TIME period v tabulce historie hello.*

Je důležité, že toonotice, který hello Tabulka historie výchozí vytvořené databáze SQL Azure již má clusterovaný index, který je kompatibilní s pro zásady uchovávání informací. Pokud se pokusíte tooremove tohoto indexu v tabulce s dobou uchování omezené, operace selže s touto hello následující chybě:

*Msg 13766, Level 16, State 1 <br> </br> hello clusterovaný index 'WebsiteUserInfoHistory.IX_WebsiteUserInfoHistory' nelze vyřadit, protože je používán pro automatické čištění zastaralá data. Pokud potřebujete toodrop tento index, zvažte nastavení HISTORY_RETENTION_PERIOD tooINFINITE na hello odpovídající systémovou správou verzí dočasnou tabulku.*

Čištění na hello clusterovaný index columnstore funguje optimálně vložit historických řádků ve vzestupném pořadí (seřazených podle hello konec sloupec období), který se vždy hello nestane, pokud je tabulka historie hello naplněn výhradně hello na text hello VERSIONIOING mechanismus. Pokud nejsou řádky v tabulce historie hello seřazené podle konec sloupec období (který může být případ hello, pokud jste migrovali existující historická data), je třeba znovu vytvořit clusterovaný index columnstore nad B-stromu vytvořit index, který je správně seřadit, tooachieve optimální výkon.

Vyhněte se znovu sestavit clusterovaný index columnstore v tabulce historie hello s dobou uchování konečné hello, protože může změnit, řazení do skupin řádků hello přirozeně vyvolané operace hello systémové správy verzí. Pokud potřebujete toorebuild clusterovaný index columnstore v tabulce historie hello, učiňte tak, že ho znovu vytvoříte nad kompatibilní index B-stromu, zachování pořadí v hello rowgroups potřebné pro čištění regulární dat. Dobrý den, které by měla být provedena stejný přístup, pokud vytvoříte dočasnou tabulku s stávající tabulka historie, který obsahuje clusterovaný index sloupce bez dat zaručenou pořadí:

````
/*Create B-tree ordered by hello end of period column*/
CREATE CLUSTERED INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory (ValidTo)
WITH (DROP_EXISTING = ON);
GO
/*Re-create clustered columnstore index*/
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON);
````

Pokud dobu uchování konečné je nakonfigurována pro tabulku historie hello s hello clusterovaný index columnstore, nelze vytvořit další neclusterovaných indexů B-stromu v této tabulce:

````
CREATE NONCLUSTERED INDEX IX_WebHistNCI ON WebsiteUserInfoHistory ([UserName])
````

Pokusu o tooexecute výše příkaz selže s touto hello následující chybě:

*Msg 13772, Level 16, State 1 <br> </br> nelze vytvořit neclusterovaný index na dočasnou tabulku historie 'WebsiteUserInfoHistory, protože má doba uchovávání omezené a clusterovaný index columnstore definované.*

## <a name="querying-tables-with-retention-policy"></a>Dotazy na tabulky s zásady uchovávání informací
Všechny dotazy na dočasnou tabulku hello automaticky vyfiltrovat historických řádky odpovídající zásady uchovávání informací omezené, tooavoid nepředvídatelným a konzistentní výsledky, protože zastaralá řádky může odstranit úlohu čištění hello, *v libovolném bodě v čase a v libovolný pořadí*.

Hello následující obrázek znázorňuje hello plán dotazu pro jednoduchý dotaz:

````
SELECT * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME ALL;
````

Hello dotazu, plán zahrnuje další filtr použít tooend sloupec období (ValidTo) v operátoru hello kontrolovat clusterovaný Index pro tabulku historie hello (zvýrazněné). Tento příklad předpokládá u tabulky WebsiteUserInfo se nastavil této doby uchování jeden měsíc.

![Filtr dotazu uchování](./media/sql-database-temporal-tables-retention-policy/queryexecplanwithretention.png)

Pokud dotaz Tabulka historie přímo, však může zobrazit řádky, které jsou starší než uchovávání období, ale bez jakékoli záruky na výsledky dotazu opakovatelných. Hello následující obrázek znázorňuje provedení plán dotazu pro dotaz hello v tabulce historie hello bez další filtry použít:

![Dotazování historie bez uchování filtru](./media/sql-database-temporal-tables-retention-policy/queryexecplanhistorytable.png)

Nespoléhejte obchodní logiky na čtení tabulky historie mimo dobu uchování, jak můžete obdržet nekonzistentní nebo neočekávané výsledky. Doporučujeme použít dočasné dotazy s klauzulí FOR SYSTEM_TIME pro analýzu dat v dočasných tabulek.

## <a name="point-in-time-restore-considerations"></a>Přejděte v aspekty čas obnovení
Když vytvoříte novou databázi pomocí [obnovení existující databáze tooa konkrétní bod v čase](sql-database-recovery-using-backups.md), má dočasné uchovávání dat na úrovni databáze hello zakázány. (**is_temporal_history_retention_enabled** nastaven příznak tooOFF). Tato funkce vám umožní tooexamine všechny řádky historických při obnovení, aniž byste museli, které stará řádky jsou odebrány, než získáte tooquery je. Můžete ji použít příliš*zkontrolovat historická data i po uplynutí doby uchování nakonfigurované*.

Řekněme, že dočasná tabulka má období uchování jeden měsíc. Pokud vaše databáze byla vytvořena v rámci úrovně služby Premium, bude možné toocreate kopie databáze se stav databáze hello až too35 dnů zpět v posledních hello. Která efektivně umožní tooanalyze historických řádků, které jsou too65 dny přímým dotazováním hello Tabulka historie.

Pokud chcete vyčistit dočasné uchování tooactivate, spusťte následující příkaz Transact-SQL v době obnovení za bod hello:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

## <a name="next-steps"></a>Další kroky
jak toouse dočasných tabulek v aplikacích, podívejte se na toolearn [Začínáme s dočasné tabulky v databázi SQL Azure](sql-database-temporal-tables.md).

Navštivte Channel 9 toohear [skutečné zákazníka dočasné implementace úspěšné scénáře](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) a sledujte [live dočasné ukázkový](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).

Podrobné informace o dočasné tabulky, zkontrolujte [dokumentace MSDN](https://msdn.microsoft.com/library/dn935015.aspx).

