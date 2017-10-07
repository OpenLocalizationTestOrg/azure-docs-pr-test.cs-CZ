---
title: "aaaGetting Začínáme s dočasné tabulky v databázi Azure SQL Database | Microsoft Docs"
description: "Zjistěte, jak tooget pracovat s použitím dočasné tabulky v databázi SQL Azure."
services: sql-database
documentationcenter: 
author: bonova
manager: jhubbard
editor: 
ms.assetid: c8c0f232-0751-4a7f-a36e-67a0b29fa1b8
ms.service: sql-database
ms.custom: develop databases
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: sql-database
ms.date: 01/10/2017
ms.author: bonova
ms.openlocfilehash: 54f394b51df07aa2f9bb299f207e692171d23479
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-temporal-tables-in-azure-sql-database"></a>Začínáme s dočasné tabulky v databázi Azure SQL
Dočasné tabulky jsou novou funkcí programovatelnosti databáze SQL Azure, který vám umožní tootrack a analyzujte hello úplnou historii změn ve vašich datech bez nutnosti hello vlastní kódování. Dočasné tabulky zachovat kontextu úzce související tootime dat tak, aby uložené fakty lze interpretovat jako platný pouze v rámci hello určitou dobu. Tato vlastnost dočasné tabulky umožňuje efektivní analýzu založené na čase a získávání přehledy z dat vývoj.

## <a name="temporal-scenario"></a>Dočasné scénář
Tento článek ukazuje hello kroky tooutilize dočasných tabulek se v případě pomocí aplikace. Předpokládejme, že chcete tootrack aktivity uživatelů na nový web, který je vyvíjen od začátku nebo na existující web, které chcete tooextend s analytics aktivity uživatele. V tomto příkladu zjednodušené budeme předpokládat, že je hello počet navštívené webové stránky během v časovém intervalu indikátor, který potřebuje toobe zachytit a monitorovat v databázi webu hello, který je hostován na Azure SQL Database. cílem Hello hello historické analýzy aktivity uživatelů, je tooget vstupy tooredesign web a poskytuje lepší prostředí pro návštěvníky hello.

model Hello databáze pro tento scénář je velmi jednoduchý – metrika aktivity uživatele je reprezentována s polem jeden celé číslo, **PageVisited**a zachytí společně s základní informace o profilu uživatele hello. Kromě toho pro čas na základě analýzy, byste udržovali řadu řádků pro každého uživatele, přičemž každý řádek představuje hello počet stránek konkrétní uživatel navštívil v konkrétním časovém období.

![Schéma](./media/sql-database-temporal-tables/AzureTemporal1.png)

Naštěstí, není nutné tooput všechny úsilí ve vaší aplikaci toomaintain informace této aktivity. U dočasných tabulek je tento proces automatizované - budete úplnou flexibilitu při návrhu webu a další čas toofocus na hello analýzy dat, sám sebe. Hello jen, co máte toodo je tooensure, **WebSiteInfo** tabulka je nakonfigurovaný jako [dočasné systémovou správou verzí](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_0). přesný postup tooutilize Hello dočasných tabulek se v tomto scénáři jsou popsané níže.

## <a name="step-1-configure-tables-as-temporal"></a>Krok 1: Konfigurace jako dočasné tabulky
V závislosti na tom, jestli jsou spuštění vývoj nových nebo upgrade stávající aplikaci bude dočasné tabulky umožňuje vytvořit nebo upravit existující přidáním dočasné atributy. Obecně případu, váš scénář může jednat o kombinaci těchto dvou možností. Proveďte tyto akce pomocí [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SSMS), [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) (SSDT) nebo jakýkoli jiný nástroj pro vývoj Transact-SQL.

> [!IMPORTANT]
> Se doporučuje hello vždy použít nejnovější verzi tooremain Management Studio synchronizovat se službou aktualizace tooMicrosoft Azure a SQL Database. [Aktualizovat aplikaci SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

### <a name="create-new-table"></a>Vytvořit novou tabulku
Pomocí položky kontextové nabídky "Novou systémovou správou verzí tabulku" v editoru dotazů hello tooopen Průzkumník objektů aplikace SSMS skript dočasná tabulka šablony a poté použít šablonu hello toopopulate "Zadejte hodnoty pro parametry šablony" (Ctrl + Shift + M):

![SSMSNewTable](./media/sql-database-temporal-tables/AzureTemporal2.png)

V sadě SSDT zvolili při přidávání nové položky toohello databázového projektu šablony "Dočasnou tabulku (systémovou správou verzí)". Aby se otevřít tabulku designer a povolit tooeasily můžete zadat hello rozložení tabulky:

![SSDTNewTable](./media/sql-database-temporal-tables/AzureTemporal3.png)

Můžete také vytvořit dočasnou tabulku zadáním hello Transact-SQL příkazy přímo, jak je znázorněno v následujícím příkladu hello. Upozorňujeme, že jsou povinné elementy hello každých dočasné tabulky definice období hello a klauzuli SYSTEM_VERSIONING hello s tabulkou uživatele tooanother odkaz, který bude ukládat historické řádek verze:

````
CREATE TABLE WebsiteUserInfo 
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED 
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL 
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
````

Při vytváření dočasné tabulce se systémovou správou verzí se automaticky vytvoří hello doplňujícími tabulku historie s hello výchozí konfigurace. Tabulka historie výchozí Hello obsahuje clusterovaný index B-stromu na sloupce období hello (end, start) s stránky komprese zapnuta. Tato konfigurace je optimální pro hello většinu scénářů, ve kterých se používají dočasné tabulky, hlavně pro [auditování dat](https://msdn.microsoft.com/library/mt631669.aspx#Anchor_0). 

V tomto případě usilujeme o analýzy trendů založené na čase tooperform přes delší historie dat a s větší sady dat, takže volba hello úložiště pro tabulku historie hello je clusterovaný index columnstore. Clusterované columnstore poskytuje velmi dobré komprese a výkon pro analytické dotazy. Dočasné tabulky udělení hello flexibilitu tooconfigure indexy na aktuální a dočasné tabulky hello zcela nezávisle. 

> [!NOTE]
> Indexy Columnstore jsou dostupné jen ve vrstvě služeb premium hello.
>

Následující skript Hello ukazuje, jak výchozí index pro tabulku historie mohou být změněné toohello clusterovaný columnstore:

````
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

Dočasné tabulky jsou reprezentované v hello Průzkumník objektů konkrétní ikonou hello pro snazší identifikaci při jeho tabulka historie se zobrazí jako podřízený uzel.

![AlterTable](./media/sql-database-temporal-tables/AzureTemporal4.png)

### <a name="alter-existing-table-tootemporal"></a>Příkaz ALTER tootemporal existující tabulky
Pojďme se věnují hello alternativní scénář, ve které hello WebsiteUserInfo tabulka již existuje, ale nebyla navrženou tookeep historii změn. V takovém případě můžete jednoduše rozšířit hello existující tabulky toobecome dočasné, jak je uvedené v následující ukázka hello:

````
ALTER TABLE WebsiteUserInfo 
ADD 
    ValidFrom datetime2 (0) GENERATED ALWAYS AS ROW START HIDDEN  
        constraint DF_ValidFrom DEFAULT DATEADD(SECOND, -1, SYSUTCDATETIME())
    , ValidTo datetime2 (0)  GENERATED ALWAYS AS ROW END HIDDEN   
        constraint DF_ValidTo DEFAULT '9999.12.31 23:59:59.99'
    , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo); 

ALTER TABLE WebsiteUserInfo  
SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
GO

CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

## <a name="step-2-run-your-workload-regularly"></a>Krok 2: Spuštění úlohy pravidelně
Hello hlavní výhodou dočasné tabulky je nutné toochange nebo upravit svůj web ke změně tooperform způsob sledování. Po vytvoření dočasných tabulek se pokaždé, když provádíte změny na vaše data transparentně zachovat předchozí verze řádku. 

V pořadí tooleverage sledování automatických změn pro tento konkrétní scénář, umožňuje pouze aktualizovat sloupec **PagesVisited** pokaždé, když se při ukončení uživatele za své relace na webu hello:

````
UPDATE WebsiteUserInfo  SET [PagesVisited] = 5 
WHERE [UserID] = 1;
````

Je důležité, že toonotice, který hello aktualizace dotazu nepotřebuje tooknow hello přesný čas, když došlo k chybě aktuální operace hello ani jak historická data se zachovají pro budoucí analýzu. Oba tyto aspekty jsou automaticky zpracovávány hello Azure SQL Database. Hello následující diagram znázorňuje způsob, jakým se data o historii vygeneruje při každé aktualizaci.

![TemporalArchitecture](./media/sql-database-temporal-tables/AzureTemporal5.png)

## <a name="step-3-perform-historical-data-analysis"></a>Krok 3: Provedení analýza historických dat
Teď když je povolené dočasné systému – Správa verzí, analýza historických dat je pouze jeden dotazu od vás. V tomto článku jsme poskytují několik příkladů, které řeší běžné scénáře analýzy - toolearn všechny podrobnosti, prozkoumejte různé možnosti zavedené hello [FOR SYSTEM_TIME](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_3) klauzule.

toosee hello top 10 uživatelů seřazené podle hello počet navštívené webové stránky od hodinou, spusťte tento dotaz:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
SELECT TOP 10 * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME AS OF @hourAgo
ORDER BY PagesVisited DESC
````

Můžete snadno upravit tento dotaz tooanalyze hello lokality navštíví od před jedním dnem, měsícem nebo kdykoli během posledních hello nechcete.

základní statistické analýzy tooperform pro hello předchozí den, použijte následující ukázka hello:

````
DECLARE @twoDaysAgo datetime2 = DATEADD(DAY, -2, SYSUTCDATETIME());
DECLARE @aDayAgo datetime2 = DATEADD(DAY, -1, SYSUTCDATETIME());

SELECT UserID, SUM (PagesVisited) as TotalVisitedPages, AVG (PagesVisited) as AverageVisitedPages,
MAX (PagesVisited) AS MaxVisitedPages, MIN (PagesVisited) AS MinVisitedPages,
STDEV (PagesVisited) as StDevViistedPages
FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME BETWEEN @twoDaysAgo AND @aDayAgo
GROUP BY UserId
````

toosearch aktivity konkrétního uživatele, v rámci určitou dobu, použijte hello omezením v klauzuli:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
DECLARE @twoHoursAgo datetime2 = DATEADD(HOUR, -2, SYSUTCDATETIME());
SELECT * FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME CONTAINED IN (@twoHoursAgo, @hourAgo)
WHERE [UserID] = 1;
````

Grafické vizualizace je zvlášť vhodné pro dočasné dotazy, jak můžete zobrazit trendy a vzorce používání v intuitivní způsob velmi snadno:

![TemporalGraph](./media/sql-database-temporal-tables/AzureTemporal6.png)

## <a name="evolving-table-schema"></a>Vyvíjející se schéma tabulky
Obvykle je nutné toochange hello dočasná tabulka schématu při vedete vývoj aplikací. Pro tento jednoduše spusťte regulární příkazů ALTER TABLE a Azure SQL Database správně rozšíří tabulku historie toohello změny. Hello skript je ukázkou, jak můžete přidat další atribut pro sledování:

````
/*Add new column for tracking source IP address*/
ALTER TABLE dbo.WebsiteUserInfo 
ADD  [IPAddress] varchar(128) NOT NULL CONSTRAINT DF_Address DEFAULT 'N/A';
````

Podobně můžete změnit definici sloupce při aktivní úlohy:

````
/*Increase hello length of name column*/
ALTER TABLE dbo.WebsiteUserInfo 
    ALTER COLUMN  UserName nvarchar(256) NOT NULL;
````

Nakonec můžete odebrat sloupec, který už nepotřebujete.

````
/*Drop unnecessary column */
ALTER TABLE dbo.WebsiteUserInfo 
    DROP COLUMN TemporaryColumn; 
````

Můžete taky použít nejnovější [SSDT](https://msdn.microsoft.com/library/mt204009.aspx) toochange dočasné tabulky schématu jsou připojené toohello databáze (online režim) nebo jako součást hello databázový projekt (offline režim).

## <a name="controlling-retention-of-historical-data"></a>Řízení uchovávání historických dat
Dočasných tabulek se systémovou správou verzí může zvýšit Tabulka historie hello hello velikost databáze více než regulérních tabulkách. Tabulky historie velké a stále se rozšiřujícího se může stát problém splatnosti toopure náklady na úložiště, jakož i nastavení výkonu i daňové na dočasné dotazování. Proto vývoj data zásady uchovávání informací pro správu dat v tabulce historie hello je důležitým aspektem plánování a správě životního cyklu hello každých dočasné tabulky. S Azure SQL Database máte hello následujících postupů pro správu historická data v dočasné tabulce hello:

* [Vytváření oddílů tabulky](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_2)
* [Skript pro vyčištění vlastní](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_3)

## <a name="next-steps"></a>Další kroky
Podrobné informace o dočasné tabulky, podívejte se na [dokumentace MSDN](https://msdn.microsoft.com/library/dn935015.aspx).
Navštivte Channel 9 toohear [skutečné zákazníka dočasné implementace úspěšné scénáře](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) a sledujte [live dočasné ukázkový](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).

