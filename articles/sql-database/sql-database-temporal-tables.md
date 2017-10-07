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
# <a name="getting-started-with-temporal-tables-in-azure-sql-database"></a><span data-ttu-id="a6268-103">Začínáme s dočasné tabulky v databázi Azure SQL</span><span class="sxs-lookup"><span data-stu-id="a6268-103">Getting Started with Temporal Tables in Azure SQL Database</span></span>
<span data-ttu-id="a6268-104">Dočasné tabulky jsou novou funkcí programovatelnosti databáze SQL Azure, který vám umožní tootrack a analyzujte hello úplnou historii změn ve vašich datech bez nutnosti hello vlastní kódování.</span><span class="sxs-lookup"><span data-stu-id="a6268-104">Temporal Tables are a new programmability feature of Azure SQL Database that allows you tootrack and analyze hello full history of changes in your data, without hello need for custom coding.</span></span> <span data-ttu-id="a6268-105">Dočasné tabulky zachovat kontextu úzce související tootime dat tak, aby uložené fakty lze interpretovat jako platný pouze v rámci hello určitou dobu.</span><span class="sxs-lookup"><span data-stu-id="a6268-105">Temporal Tables keep data closely related tootime context so that stored facts can be interpreted as valid only within hello specific period.</span></span> <span data-ttu-id="a6268-106">Tato vlastnost dočasné tabulky umožňuje efektivní analýzu založené na čase a získávání přehledy z dat vývoj.</span><span class="sxs-lookup"><span data-stu-id="a6268-106">This property of Temporal Tables allows for efficient time-based analysis and getting insights from data evolution.</span></span>

## <a name="temporal-scenario"></a><span data-ttu-id="a6268-107">Dočasné scénář</span><span class="sxs-lookup"><span data-stu-id="a6268-107">Temporal Scenario</span></span>
<span data-ttu-id="a6268-108">Tento článek ukazuje hello kroky tooutilize dočasných tabulek se v případě pomocí aplikace.</span><span class="sxs-lookup"><span data-stu-id="a6268-108">This article illustrates hello steps tooutilize Temporal Tables in an application scenario.</span></span> <span data-ttu-id="a6268-109">Předpokládejme, že chcete tootrack aktivity uživatelů na nový web, který je vyvíjen od začátku nebo na existující web, které chcete tooextend s analytics aktivity uživatele.</span><span class="sxs-lookup"><span data-stu-id="a6268-109">Suppose that you want tootrack user activity on a new website that is being developed from scratch or on an existing website that you want tooextend with user activity analytics.</span></span> <span data-ttu-id="a6268-110">V tomto příkladu zjednodušené budeme předpokládat, že je hello počet navštívené webové stránky během v časovém intervalu indikátor, který potřebuje toobe zachytit a monitorovat v databázi webu hello, který je hostován na Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="a6268-110">In this simplified example, we assume that hello number of visited web pages during a period of time is an indicator that needs toobe captured and monitored in hello website database that is hosted on Azure SQL Database.</span></span> <span data-ttu-id="a6268-111">cílem Hello hello historické analýzy aktivity uživatelů, je tooget vstupy tooredesign web a poskytuje lepší prostředí pro návštěvníky hello.</span><span class="sxs-lookup"><span data-stu-id="a6268-111">hello goal of hello historical analysis of user activity is tooget inputs tooredesign website and provide better experience for hello visitors.</span></span>

<span data-ttu-id="a6268-112">model Hello databáze pro tento scénář je velmi jednoduchý – metrika aktivity uživatele je reprezentována s polem jeden celé číslo, **PageVisited**a zachytí společně s základní informace o profilu uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="a6268-112">hello database model for this scenario is very simple - user activity metric is represented with a single integer field, **PageVisited**, and is captured along with basic information on hello user profile.</span></span> <span data-ttu-id="a6268-113">Kromě toho pro čas na základě analýzy, byste udržovali řadu řádků pro každého uživatele, přičemž každý řádek představuje hello počet stránek konkrétní uživatel navštívil v konkrétním časovém období.</span><span class="sxs-lookup"><span data-stu-id="a6268-113">Additionally, for time based analysis, you would keep a series of rows for each user, where every row represents hello number of pages a particular user visited within a specific period of time.</span></span>

![Schéma](./media/sql-database-temporal-tables/AzureTemporal1.png)

<span data-ttu-id="a6268-115">Naštěstí, není nutné tooput všechny úsilí ve vaší aplikaci toomaintain informace této aktivity.</span><span class="sxs-lookup"><span data-stu-id="a6268-115">Fortunately, you do not need tooput any effort in your app toomaintain this activity information.</span></span> <span data-ttu-id="a6268-116">U dočasných tabulek je tento proces automatizované - budete úplnou flexibilitu při návrhu webu a další čas toofocus na hello analýzy dat, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="a6268-116">With Temporal Tables, this process is automated - giving you full flexibility during website design and more time toofocus on hello data analysis itself.</span></span> <span data-ttu-id="a6268-117">Hello jen, co máte toodo je tooensure, **WebSiteInfo** tabulka je nakonfigurovaný jako [dočasné systémovou správou verzí](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_0).</span><span class="sxs-lookup"><span data-stu-id="a6268-117">hello only thing you have toodo is tooensure that **WebSiteInfo** table is configured as [temporal system-versioned](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_0).</span></span> <span data-ttu-id="a6268-118">přesný postup tooutilize Hello dočasných tabulek se v tomto scénáři jsou popsané níže.</span><span class="sxs-lookup"><span data-stu-id="a6268-118">hello exact steps tooutilize Temporal Tables in this scenario are described below.</span></span>

## <a name="step-1-configure-tables-as-temporal"></a><span data-ttu-id="a6268-119">Krok 1: Konfigurace jako dočasné tabulky</span><span class="sxs-lookup"><span data-stu-id="a6268-119">Step 1: Configure tables as temporal</span></span>
<span data-ttu-id="a6268-120">V závislosti na tom, jestli jsou spuštění vývoj nových nebo upgrade stávající aplikaci bude dočasné tabulky umožňuje vytvořit nebo upravit existující přidáním dočasné atributy.</span><span class="sxs-lookup"><span data-stu-id="a6268-120">Depending on whether you are starting new development or upgrading existing application, you will either create temporal tables or modify existing ones by adding temporal attributes.</span></span> <span data-ttu-id="a6268-121">Obecně případu, váš scénář může jednat o kombinaci těchto dvou možností.</span><span class="sxs-lookup"><span data-stu-id="a6268-121">In general case, your scenario can be a mix of these two options.</span></span> <span data-ttu-id="a6268-122">Proveďte tyto akce pomocí [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SSMS), [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) (SSDT) nebo jakýkoli jiný nástroj pro vývoj Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="a6268-122">Perform these action using [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SSMS), [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) (SSDT) or any other Transact-SQL development tool.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a6268-123">Se doporučuje hello vždy použít nejnovější verzi tooremain Management Studio synchronizovat se službou aktualizace tooMicrosoft Azure a SQL Database.</span><span class="sxs-lookup"><span data-stu-id="a6268-123">It is recommended that you always use hello latest version of Management Studio tooremain synchronized with updates tooMicrosoft Azure and SQL Database.</span></span> <span data-ttu-id="a6268-124">[Aktualizovat aplikaci SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="a6268-124">[Update SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).</span></span>
> 
> 

### <a name="create-new-table"></a><span data-ttu-id="a6268-125">Vytvořit novou tabulku</span><span class="sxs-lookup"><span data-stu-id="a6268-125">Create new table</span></span>
<span data-ttu-id="a6268-126">Pomocí položky kontextové nabídky "Novou systémovou správou verzí tabulku" v editoru dotazů hello tooopen Průzkumník objektů aplikace SSMS skript dočasná tabulka šablony a poté použít šablonu hello toopopulate "Zadejte hodnoty pro parametry šablony" (Ctrl + Shift + M):</span><span class="sxs-lookup"><span data-stu-id="a6268-126">Use context menu item “New System-Versioned Table” in SSMS Object Explorer tooopen hello query editor with a temporal table template script and then use “Specify Values for Template Parameters” (Ctrl+Shift+M) toopopulate hello template:</span></span>

![SSMSNewTable](./media/sql-database-temporal-tables/AzureTemporal2.png)

<span data-ttu-id="a6268-128">V sadě SSDT zvolili při přidávání nové položky toohello databázového projektu šablony "Dočasnou tabulku (systémovou správou verzí)".</span><span class="sxs-lookup"><span data-stu-id="a6268-128">In SSDT, chose “Temporal Table (System-Versioned)” template when adding new items toohello database project.</span></span> <span data-ttu-id="a6268-129">Aby se otevřít tabulku designer a povolit tooeasily můžete zadat hello rozložení tabulky:</span><span class="sxs-lookup"><span data-stu-id="a6268-129">That will open table designer and enable you tooeasily specify hello table layout:</span></span>

![SSDTNewTable](./media/sql-database-temporal-tables/AzureTemporal3.png)

<span data-ttu-id="a6268-131">Můžete také vytvořit dočasnou tabulku zadáním hello Transact-SQL příkazy přímo, jak je znázorněno v následujícím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="a6268-131">You can also a create temporal table by specifying hello Transact-SQL statements directly, as shown in hello example below.</span></span> <span data-ttu-id="a6268-132">Upozorňujeme, že jsou povinné elementy hello každých dočasné tabulky definice období hello a klauzuli SYSTEM_VERSIONING hello s tabulkou uživatele tooanother odkaz, který bude ukládat historické řádek verze:</span><span class="sxs-lookup"><span data-stu-id="a6268-132">Note that hello mandatory elements of every temporal table are hello PERIOD definition and hello SYSTEM_VERSIONING clause with a reference tooanother user table that will store historical row versions:</span></span>

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

<span data-ttu-id="a6268-133">Při vytváření dočasné tabulce se systémovou správou verzí se automaticky vytvoří hello doplňujícími tabulku historie s hello výchozí konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a6268-133">When you create system-versioned temporal table, hello accompanying history table with hello default configuration is automatically created.</span></span> <span data-ttu-id="a6268-134">Tabulka historie výchozí Hello obsahuje clusterovaný index B-stromu na sloupce období hello (end, start) s stránky komprese zapnuta.</span><span class="sxs-lookup"><span data-stu-id="a6268-134">hello default history table contains a clustered B-tree index on hello period columns (end, start) with page compression enabled.</span></span> <span data-ttu-id="a6268-135">Tato konfigurace je optimální pro hello většinu scénářů, ve kterých se používají dočasné tabulky, hlavně pro [auditování dat](https://msdn.microsoft.com/library/mt631669.aspx#Anchor_0).</span><span class="sxs-lookup"><span data-stu-id="a6268-135">This configuration is optimal for hello majority of scenarios in which temporal tables are used, especially for [data auditing](https://msdn.microsoft.com/library/mt631669.aspx#Anchor_0).</span></span> 

<span data-ttu-id="a6268-136">V tomto případě usilujeme o analýzy trendů založené na čase tooperform přes delší historie dat a s větší sady dat, takže volba hello úložiště pro tabulku historie hello je clusterovaný index columnstore.</span><span class="sxs-lookup"><span data-stu-id="a6268-136">In this particular case, we aim tooperform time-based trend analysis over a longer data history and with bigger data sets, so hello storage choice for hello history table is a clustered columnstore index.</span></span> <span data-ttu-id="a6268-137">Clusterované columnstore poskytuje velmi dobré komprese a výkon pro analytické dotazy.</span><span class="sxs-lookup"><span data-stu-id="a6268-137">A clustered columnstore provides very good compression and performance for analytical queries.</span></span> <span data-ttu-id="a6268-138">Dočasné tabulky udělení hello flexibilitu tooconfigure indexy na aktuální a dočasné tabulky hello zcela nezávisle.</span><span class="sxs-lookup"><span data-stu-id="a6268-138">Temporal Tables give you hello flexibility tooconfigure indexes on hello current and temporal tables completely independently.</span></span> 

> [!NOTE]
> <span data-ttu-id="a6268-139">Indexy Columnstore jsou dostupné jen ve vrstvě služeb premium hello.</span><span class="sxs-lookup"><span data-stu-id="a6268-139">Columnstore indexes are only available in hello premium service tier.</span></span>
>

<span data-ttu-id="a6268-140">Následující skript Hello ukazuje, jak výchozí index pro tabulku historie mohou být změněné toohello clusterovaný columnstore:</span><span class="sxs-lookup"><span data-stu-id="a6268-140">hello following script shows how default index on history table can be changed toohello clustered columnstore:</span></span>

````
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

<span data-ttu-id="a6268-141">Dočasné tabulky jsou reprezentované v hello Průzkumník objektů konkrétní ikonou hello pro snazší identifikaci při jeho tabulka historie se zobrazí jako podřízený uzel.</span><span class="sxs-lookup"><span data-stu-id="a6268-141">Temporal Tables are represented in hello Object Explorer with hello specific icon for easier identification, while its history table is displayed as a child node.</span></span>

![AlterTable](./media/sql-database-temporal-tables/AzureTemporal4.png)

### <a name="alter-existing-table-tootemporal"></a><span data-ttu-id="a6268-143">Příkaz ALTER tootemporal existující tabulky</span><span class="sxs-lookup"><span data-stu-id="a6268-143">Alter existing table tootemporal</span></span>
<span data-ttu-id="a6268-144">Pojďme se věnují hello alternativní scénář, ve které hello WebsiteUserInfo tabulka již existuje, ale nebyla navrženou tookeep historii změn.</span><span class="sxs-lookup"><span data-stu-id="a6268-144">Let’s cover hello alternative scenario in which hello WebsiteUserInfo table already exists, but was not designed tookeep a history of changes.</span></span> <span data-ttu-id="a6268-145">V takovém případě můžete jednoduše rozšířit hello existující tabulky toobecome dočasné, jak je uvedené v následující ukázka hello:</span><span class="sxs-lookup"><span data-stu-id="a6268-145">In this case, you can simply extend hello existing table toobecome temporal, as shown in hello following example:</span></span>

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

## <a name="step-2-run-your-workload-regularly"></a><span data-ttu-id="a6268-146">Krok 2: Spuštění úlohy pravidelně</span><span class="sxs-lookup"><span data-stu-id="a6268-146">Step 2: Run your workload regularly</span></span>
<span data-ttu-id="a6268-147">Hello hlavní výhodou dočasné tabulky je nutné toochange nebo upravit svůj web ke změně tooperform způsob sledování.</span><span class="sxs-lookup"><span data-stu-id="a6268-147">hello main advantage of Temporal Tables is that you do not need toochange or adjust your website in any way tooperform change tracking.</span></span> <span data-ttu-id="a6268-148">Po vytvoření dočasných tabulek se pokaždé, když provádíte změny na vaše data transparentně zachovat předchozí verze řádku.</span><span class="sxs-lookup"><span data-stu-id="a6268-148">Once created, Temporal Tables transparently persist previous row versions every time you perform modifications on your data.</span></span> 

<span data-ttu-id="a6268-149">V pořadí tooleverage sledování automatických změn pro tento konkrétní scénář, umožňuje pouze aktualizovat sloupec **PagesVisited** pokaždé, když se při ukončení uživatele za své relace na webu hello:</span><span class="sxs-lookup"><span data-stu-id="a6268-149">In order tooleverage automatic change tracking for this particular scenario, let’s just update column **PagesVisited** every time when user ends her/his session on hello website:</span></span>

````
UPDATE WebsiteUserInfo  SET [PagesVisited] = 5 
WHERE [UserID] = 1;
````

<span data-ttu-id="a6268-150">Je důležité, že toonotice, který hello aktualizace dotazu nepotřebuje tooknow hello přesný čas, když došlo k chybě aktuální operace hello ani jak historická data se zachovají pro budoucí analýzu.</span><span class="sxs-lookup"><span data-stu-id="a6268-150">It is important toonotice that hello update query doesn’t need tooknow hello exact time when hello actual operation occurred nor how historical data will be preserved for future analysis.</span></span> <span data-ttu-id="a6268-151">Oba tyto aspekty jsou automaticky zpracovávány hello Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="a6268-151">Both aspects are automatically handled by hello Azure SQL Database.</span></span> <span data-ttu-id="a6268-152">Hello následující diagram znázorňuje způsob, jakým se data o historii vygeneruje při každé aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="a6268-152">hello following diagram illustrates how history data is being generated on every update.</span></span>

![TemporalArchitecture](./media/sql-database-temporal-tables/AzureTemporal5.png)

## <a name="step-3-perform-historical-data-analysis"></a><span data-ttu-id="a6268-154">Krok 3: Provedení analýza historických dat</span><span class="sxs-lookup"><span data-stu-id="a6268-154">Step 3: Perform historical data analysis</span></span>
<span data-ttu-id="a6268-155">Teď když je povolené dočasné systému – Správa verzí, analýza historických dat je pouze jeden dotazu od vás.</span><span class="sxs-lookup"><span data-stu-id="a6268-155">Now when temporal system-versioning is enabled, historical data analysis is just one query away from you.</span></span> <span data-ttu-id="a6268-156">V tomto článku jsme poskytují několik příkladů, které řeší běžné scénáře analýzy - toolearn všechny podrobnosti, prozkoumejte různé možnosti zavedené hello [FOR SYSTEM_TIME](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_3) klauzule.</span><span class="sxs-lookup"><span data-stu-id="a6268-156">In this article, we will provide a few examples that address common analysis scenarios - toolearn all details, explore various options introduced with hello [FOR SYSTEM_TIME](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_3) clause.</span></span>

<span data-ttu-id="a6268-157">toosee hello top 10 uživatelů seřazené podle hello počet navštívené webové stránky od hodinou, spusťte tento dotaz:</span><span class="sxs-lookup"><span data-stu-id="a6268-157">toosee hello top 10 users ordered by hello number of visited web pages as of an hour ago, run this query:</span></span>

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
SELECT TOP 10 * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME AS OF @hourAgo
ORDER BY PagesVisited DESC
````

<span data-ttu-id="a6268-158">Můžete snadno upravit tento dotaz tooanalyze hello lokality navštíví od před jedním dnem, měsícem nebo kdykoli během posledních hello nechcete.</span><span class="sxs-lookup"><span data-stu-id="a6268-158">You can easily modify this query tooanalyze hello site visits as of a day ago, a month ago or at any point in hello past you wish.</span></span>

<span data-ttu-id="a6268-159">základní statistické analýzy tooperform pro hello předchozí den, použijte následující ukázka hello:</span><span class="sxs-lookup"><span data-stu-id="a6268-159">tooperform basic statistical analysis for hello previous day, use hello following example:</span></span>

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

<span data-ttu-id="a6268-160">toosearch aktivity konkrétního uživatele, v rámci určitou dobu, použijte hello omezením v klauzuli:</span><span class="sxs-lookup"><span data-stu-id="a6268-160">toosearch for activities of a specific user, within a period of time, use hello CONTAINED IN clause:</span></span>

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
DECLARE @twoHoursAgo datetime2 = DATEADD(HOUR, -2, SYSUTCDATETIME());
SELECT * FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME CONTAINED IN (@twoHoursAgo, @hourAgo)
WHERE [UserID] = 1;
````

<span data-ttu-id="a6268-161">Grafické vizualizace je zvlášť vhodné pro dočasné dotazy, jak můžete zobrazit trendy a vzorce používání v intuitivní způsob velmi snadno:</span><span class="sxs-lookup"><span data-stu-id="a6268-161">Graphic visualization is especially convenient for temporal queries as you can show trends and usage patterns in an intuitive way very easily:</span></span>

![TemporalGraph](./media/sql-database-temporal-tables/AzureTemporal6.png)

## <a name="evolving-table-schema"></a><span data-ttu-id="a6268-163">Vyvíjející se schéma tabulky</span><span class="sxs-lookup"><span data-stu-id="a6268-163">Evolving table schema</span></span>
<span data-ttu-id="a6268-164">Obvykle je nutné toochange hello dočasná tabulka schématu při vedete vývoj aplikací.</span><span class="sxs-lookup"><span data-stu-id="a6268-164">Typically, you will need toochange hello temporal table schema while you are doing app development.</span></span> <span data-ttu-id="a6268-165">Pro tento jednoduše spusťte regulární příkazů ALTER TABLE a Azure SQL Database správně rozšíří tabulku historie toohello změny.</span><span class="sxs-lookup"><span data-stu-id="a6268-165">For that, simply run regular ALTER TABLE statements and Azure SQL Database will appropriately propagate changes toohello history table.</span></span> <span data-ttu-id="a6268-166">Hello skript je ukázkou, jak můžete přidat další atribut pro sledování:</span><span class="sxs-lookup"><span data-stu-id="a6268-166">hello following script shows how you can add additional attribute for tracking:</span></span>

````
/*Add new column for tracking source IP address*/
ALTER TABLE dbo.WebsiteUserInfo 
ADD  [IPAddress] varchar(128) NOT NULL CONSTRAINT DF_Address DEFAULT 'N/A';
````

<span data-ttu-id="a6268-167">Podobně můžete změnit definici sloupce při aktivní úlohy:</span><span class="sxs-lookup"><span data-stu-id="a6268-167">Similarly, you can change column definition while your workload is active:</span></span>

````
/*Increase hello length of name column*/
ALTER TABLE dbo.WebsiteUserInfo 
    ALTER COLUMN  UserName nvarchar(256) NOT NULL;
````

<span data-ttu-id="a6268-168">Nakonec můžete odebrat sloupec, který už nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="a6268-168">Finally, you can remove a column that you do not need anymore.</span></span>

````
/*Drop unnecessary column */
ALTER TABLE dbo.WebsiteUserInfo 
    DROP COLUMN TemporaryColumn; 
````

<span data-ttu-id="a6268-169">Můžete taky použít nejnovější [SSDT](https://msdn.microsoft.com/library/mt204009.aspx) toochange dočasné tabulky schématu jsou připojené toohello databáze (online režim) nebo jako součást hello databázový projekt (offline režim).</span><span class="sxs-lookup"><span data-stu-id="a6268-169">Alternatively, use latest [SSDT](https://msdn.microsoft.com/library/mt204009.aspx) toochange temporal table schema while you are connected toohello database (online mode) or as part of hello database project (offline mode).</span></span>

## <a name="controlling-retention-of-historical-data"></a><span data-ttu-id="a6268-170">Řízení uchovávání historických dat</span><span class="sxs-lookup"><span data-stu-id="a6268-170">Controlling retention of historical data</span></span>
<span data-ttu-id="a6268-171">Dočasných tabulek se systémovou správou verzí může zvýšit Tabulka historie hello hello velikost databáze více než regulérních tabulkách.</span><span class="sxs-lookup"><span data-stu-id="a6268-171">With system-versioned temporal tables, hello history table may increase hello database size more than regular tables.</span></span> <span data-ttu-id="a6268-172">Tabulky historie velké a stále se rozšiřujícího se může stát problém splatnosti toopure náklady na úložiště, jakož i nastavení výkonu i daňové na dočasné dotazování.</span><span class="sxs-lookup"><span data-stu-id="a6268-172">A large and ever-growing history table can become an issue both due toopure storage costs as well as imposing a performance tax on temporal querying.</span></span> <span data-ttu-id="a6268-173">Proto vývoj data zásady uchovávání informací pro správu dat v tabulce historie hello je důležitým aspektem plánování a správě životního cyklu hello každých dočasné tabulky.</span><span class="sxs-lookup"><span data-stu-id="a6268-173">Hence, developing a data retention policy for managing data in hello history table is an important aspect of planning and managing hello lifecycle of every temporal table.</span></span> <span data-ttu-id="a6268-174">S Azure SQL Database máte hello následujících postupů pro správu historická data v dočasné tabulce hello:</span><span class="sxs-lookup"><span data-stu-id="a6268-174">With Azure SQL Database, you have hello following approaches for managing historical data in hello temporal table:</span></span>

* [<span data-ttu-id="a6268-175">Vytváření oddílů tabulky</span><span class="sxs-lookup"><span data-stu-id="a6268-175">Table Partitioning</span></span>](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_2)
* [<span data-ttu-id="a6268-176">Skript pro vyčištění vlastní</span><span class="sxs-lookup"><span data-stu-id="a6268-176">Custom Cleanup Script</span></span>](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_3)

## <a name="next-steps"></a><span data-ttu-id="a6268-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a6268-177">Next steps</span></span>
<span data-ttu-id="a6268-178">Podrobné informace o dočasné tabulky, podívejte se na [dokumentace MSDN](https://msdn.microsoft.com/library/dn935015.aspx).</span><span class="sxs-lookup"><span data-stu-id="a6268-178">For detailed information on Temporal Tables, check out [MSDN documentation](https://msdn.microsoft.com/library/dn935015.aspx).</span></span>
<span data-ttu-id="a6268-179">Navštivte Channel 9 toohear [skutečné zákazníka dočasné implementace úspěšné scénáře](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) a sledujte [live dočasné ukázkový](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).</span><span class="sxs-lookup"><span data-stu-id="a6268-179">Visit Channel 9 toohear a [real customer temporal implemenation success story](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) and watch a [live temporal demonstration](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).</span></span>

