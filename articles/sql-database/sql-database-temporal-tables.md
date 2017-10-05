---
title: "Začínáme s dočasné tabulky v databázi Azure SQL Database | Microsoft Docs"
description: "Zjistěte, jak začít s použitím dočasné tabulky v databázi SQL Azure."
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
ms.openlocfilehash: d84db682089c65c2716d2d9bd92f7bc0ac47af27
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-temporal-tables-in-azure-sql-database"></a><span data-ttu-id="83993-103">Začínáme s dočasné tabulky v databázi Azure SQL</span><span class="sxs-lookup"><span data-stu-id="83993-103">Getting Started with Temporal Tables in Azure SQL Database</span></span>
<span data-ttu-id="83993-104">Dočasné tabulky jsou novou funkcí programovatelnosti databáze SQL Azure, která umožňuje sledovat a analyzovat úplnou historii změn ve vašich datech bez nutnosti vlastní kódování.</span><span class="sxs-lookup"><span data-stu-id="83993-104">Temporal Tables are a new programmability feature of Azure SQL Database that allows you to track and analyze the full history of changes in your data, without the need for custom coding.</span></span> <span data-ttu-id="83993-105">Dočasné tabulky zachovat data úzce související čas kontextu tak, aby uložené fakty lze interpretovat jako platný jen v určitou dobu.</span><span class="sxs-lookup"><span data-stu-id="83993-105">Temporal Tables keep data closely related to time context so that stored facts can be interpreted as valid only within the specific period.</span></span> <span data-ttu-id="83993-106">Tato vlastnost dočasné tabulky umožňuje efektivní analýzu založené na čase a získávání přehledy z dat vývoj.</span><span class="sxs-lookup"><span data-stu-id="83993-106">This property of Temporal Tables allows for efficient time-based analysis and getting insights from data evolution.</span></span>

## <a name="temporal-scenario"></a><span data-ttu-id="83993-107">Dočasné scénář</span><span class="sxs-lookup"><span data-stu-id="83993-107">Temporal Scenario</span></span>
<span data-ttu-id="83993-108">Tento článek ukazuje postup využívat dočasných tabulek se v případě pomocí aplikace.</span><span class="sxs-lookup"><span data-stu-id="83993-108">This article illustrates the steps to utilize Temporal Tables in an application scenario.</span></span> <span data-ttu-id="83993-109">Předpokládejme, že chcete sledovat aktivity uživatelů na nový web, který je vyvíjen od začátku nebo na existující web, který chcete rozšířit pomocí analýzy chování aktivity uživatelů.</span><span class="sxs-lookup"><span data-stu-id="83993-109">Suppose that you want to track user activity on a new website that is being developed from scratch or on an existing website that you want to extend with user activity analytics.</span></span> <span data-ttu-id="83993-110">V tomto příkladu zjednodušené budeme předpokládat, že je počet navštívené webové stránky během v časovém intervalu indikátor, který je třeba zaznamenat a sledovat v databázi webu, který je hostován na Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="83993-110">In this simplified example, we assume that the number of visited web pages during a period of time is an indicator that needs to be captured and monitored in the website database that is hosted on Azure SQL Database.</span></span> <span data-ttu-id="83993-111">Cílem historické analýzy aktivity uživatelů, je získat vstupy přepracovat webu a poskytuje lepší prostředí pro návštěvníkům.</span><span class="sxs-lookup"><span data-stu-id="83993-111">The goal of the historical analysis of user activity is to get inputs to redesign website and provide better experience for the visitors.</span></span>

<span data-ttu-id="83993-112">Model databáze pro tento scénář je velmi jednoduchý – metrika aktivity uživatele je reprezentována s polem jeden celé číslo, **PageVisited**a zachytí společně s základní informace o profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="83993-112">The database model for this scenario is very simple - user activity metric is represented with a single integer field, **PageVisited**, and is captured along with basic information on the user profile.</span></span> <span data-ttu-id="83993-113">Kromě toho pro čas na základě analýzy, byste udržovali řadu řádků pro každého uživatele, přičemž každý řádek představuje počet stránek konkrétní uživatel navštívil v konkrétním časovém období.</span><span class="sxs-lookup"><span data-stu-id="83993-113">Additionally, for time based analysis, you would keep a series of rows for each user, where every row represents the number of pages a particular user visited within a specific period of time.</span></span>

![Schéma](./media/sql-database-temporal-tables/AzureTemporal1.png)

<span data-ttu-id="83993-115">Naštěstí není nutné uvést všechny úsilí v aplikaci, kterou chcete spravovat informace o této aktivity.</span><span class="sxs-lookup"><span data-stu-id="83993-115">Fortunately, you do not need to put any effort in your app to maintain this activity information.</span></span> <span data-ttu-id="83993-116">U dočasných tabulek je tento proces automatizované - budete úplnou flexibilitu při návrhu webu a delší dobu a zaměřit se na samotný analýzy dat.</span><span class="sxs-lookup"><span data-stu-id="83993-116">With Temporal Tables, this process is automated - giving you full flexibility during website design and more time to focus on the data analysis itself.</span></span> <span data-ttu-id="83993-117">Jediné, co musíte udělat, je zajistit, aby **WebSiteInfo** tabulka je nakonfigurovaný jako [dočasné systémovou správou verzí](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_0).</span><span class="sxs-lookup"><span data-stu-id="83993-117">The only thing you have to do is to ensure that **WebSiteInfo** table is configured as [temporal system-versioned](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_0).</span></span> <span data-ttu-id="83993-118">Přesný postup využívat dočasných tabulek se v tomto scénáři jsou popsané níže.</span><span class="sxs-lookup"><span data-stu-id="83993-118">The exact steps to utilize Temporal Tables in this scenario are described below.</span></span>

## <a name="step-1-configure-tables-as-temporal"></a><span data-ttu-id="83993-119">Krok 1: Konfigurace jako dočasné tabulky</span><span class="sxs-lookup"><span data-stu-id="83993-119">Step 1: Configure tables as temporal</span></span>
<span data-ttu-id="83993-120">V závislosti na tom, jestli jsou spuštění vývoj nových nebo upgrade stávající aplikaci bude dočasné tabulky umožňuje vytvořit nebo upravit existující přidáním dočasné atributy.</span><span class="sxs-lookup"><span data-stu-id="83993-120">Depending on whether you are starting new development or upgrading existing application, you will either create temporal tables or modify existing ones by adding temporal attributes.</span></span> <span data-ttu-id="83993-121">Obecně případu, váš scénář může jednat o kombinaci těchto dvou možností.</span><span class="sxs-lookup"><span data-stu-id="83993-121">In general case, your scenario can be a mix of these two options.</span></span> <span data-ttu-id="83993-122">Proveďte tyto akce pomocí [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SSMS), [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) (SSDT) nebo jakýkoli jiný nástroj pro vývoj Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="83993-122">Perform these action using [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SSMS), [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) (SSDT) or any other Transact-SQL development tool.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="83993-123">Doporučujeme vám vždy používat nejnovější verzi aplikace Management Studio, aby se zajistila synchronizovanost s aktualizacemi Microsoft Azure a SQL Database.</span><span class="sxs-lookup"><span data-stu-id="83993-123">It is recommended that you always use the latest version of Management Studio to remain synchronized with updates to Microsoft Azure and SQL Database.</span></span> <span data-ttu-id="83993-124">[Aktualizovat aplikaci SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="83993-124">[Update SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).</span></span>
> 
> 

### <a name="create-new-table"></a><span data-ttu-id="83993-125">Vytvořit novou tabulku</span><span class="sxs-lookup"><span data-stu-id="83993-125">Create new table</span></span>
<span data-ttu-id="83993-126">Pomocí položky kontextové nabídky "Novou systémovou správou verzí tabulku" v Průzkumníku objektů aplikace SSMS otevřete editor dotazů pomocí skriptu dočasná tabulka šablony a potom pomocí "Zadejte hodnoty pro parametry šablony" (Ctrl + Shift + M) k naplnění šablony:</span><span class="sxs-lookup"><span data-stu-id="83993-126">Use context menu item “New System-Versioned Table” in SSMS Object Explorer to open the query editor with a temporal table template script and then use “Specify Values for Template Parameters” (Ctrl+Shift+M) to populate the template:</span></span>

![SSMSNewTable](./media/sql-database-temporal-tables/AzureTemporal2.png)

<span data-ttu-id="83993-128">V sadě SSDT zvolili při přidávání nových položek do k databázovému projektu šablony "Dočasnou tabulku (systémovou správou verzí)".</span><span class="sxs-lookup"><span data-stu-id="83993-128">In SSDT, chose “Temporal Table (System-Versioned)” template when adding new items to the database project.</span></span> <span data-ttu-id="83993-129">Které otevřete návrháře tabulky a umožňují snadno určit rozložení tabulky:</span><span class="sxs-lookup"><span data-stu-id="83993-129">That will open table designer and enable you to easily specify the table layout:</span></span>

![SSDTNewTable](./media/sql-database-temporal-tables/AzureTemporal3.png)

<span data-ttu-id="83993-131">Můžete také vytvořit dočasnou tabulku zadáním příkazy jazyka Transact-SQL přímo, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="83993-131">You can also a create temporal table by specifying the Transact-SQL statements directly, as shown in the example below.</span></span> <span data-ttu-id="83993-132">Upozorňujeme, že jsou povinné elementy každých dočasná tabulka definice období a klauzuli SYSTEM_VERSIONING s odkazem na jinou tabulku uživatele, které se uloží verze historických řádek:</span><span class="sxs-lookup"><span data-stu-id="83993-132">Note that the mandatory elements of every temporal table are the PERIOD definition and the SYSTEM_VERSIONING clause with a reference to another user table that will store historical row versions:</span></span>

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

<span data-ttu-id="83993-133">Při vytváření dočasné tabulce se systémovou správou verzí se automaticky vytvoří doprovodné tabulce historie s výchozí konfigurací.</span><span class="sxs-lookup"><span data-stu-id="83993-133">When you create system-versioned temporal table, the accompanying history table with the default configuration is automatically created.</span></span> <span data-ttu-id="83993-134">Tabulka historie výchozí obsahuje clusterovaný index B-stromu na sloupce období (end, start) s stránky komprese zapnuta.</span><span class="sxs-lookup"><span data-stu-id="83993-134">The default history table contains a clustered B-tree index on the period columns (end, start) with page compression enabled.</span></span> <span data-ttu-id="83993-135">Tato konfigurace je optimální pro většinu scénářů, ve kterých se používají dočasné tabulky, hlavně pro [auditování dat](https://msdn.microsoft.com/library/mt631669.aspx#Anchor_0).</span><span class="sxs-lookup"><span data-stu-id="83993-135">This configuration is optimal for the majority of scenarios in which temporal tables are used, especially for [data auditing](https://msdn.microsoft.com/library/mt631669.aspx#Anchor_0).</span></span> 

<span data-ttu-id="83993-136">V tomto konkrétním případě usilujeme provádět analýzy trendů založené na čase přes delší data historie a s větší sady dat, takže volba úložiště pro tabulku historie je clusterovaný index columnstore.</span><span class="sxs-lookup"><span data-stu-id="83993-136">In this particular case, we aim to perform time-based trend analysis over a longer data history and with bigger data sets, so the storage choice for the history table is a clustered columnstore index.</span></span> <span data-ttu-id="83993-137">Clusterované columnstore poskytuje velmi dobré komprese a výkon pro analytické dotazy.</span><span class="sxs-lookup"><span data-stu-id="83993-137">A clustered columnstore provides very good compression and performance for analytical queries.</span></span> <span data-ttu-id="83993-138">Dočasné tabulky poskytují flexibilitu: Konfigurace indexy zcela nezávisle na aktuální a dočasné tabulky.</span><span class="sxs-lookup"><span data-stu-id="83993-138">Temporal Tables give you the flexibility to configure indexes on the current and temporal tables completely independently.</span></span> 

> [!NOTE]
> <span data-ttu-id="83993-139">Indexy Columnstore jsou dostupné jen ve vrstvě služeb premium.</span><span class="sxs-lookup"><span data-stu-id="83993-139">Columnstore indexes are only available in the premium service tier.</span></span>
>

<span data-ttu-id="83993-140">Tento skript je ukázkou, jak lze změnit výchozí index pro tabulku historie na clusterových columnstore:</span><span class="sxs-lookup"><span data-stu-id="83993-140">The following script shows how default index on history table can be changed to the clustered columnstore:</span></span>

````
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

<span data-ttu-id="83993-141">Dočasné tabulky jsou reprezentované v Průzkumníku objektů s konkrétní ikonou pro snazší identifikaci při jeho tabulka historie se zobrazí jako podřízený uzel.</span><span class="sxs-lookup"><span data-stu-id="83993-141">Temporal Tables are represented in the Object Explorer with the specific icon for easier identification, while its history table is displayed as a child node.</span></span>

![AlterTable](./media/sql-database-temporal-tables/AzureTemporal4.png)

### <a name="alter-existing-table-to-temporal"></a><span data-ttu-id="83993-143">Změna existující tabulky k dočasné</span><span class="sxs-lookup"><span data-stu-id="83993-143">Alter existing table to temporal</span></span>
<span data-ttu-id="83993-144">Pojďme se věnují alternativní scénář, ve kterém WebsiteUserInfo tabulce již existuje, ale nebyl navržen, aby historii změn.</span><span class="sxs-lookup"><span data-stu-id="83993-144">Let’s cover the alternative scenario in which the WebsiteUserInfo table already exists, but was not designed to keep a history of changes.</span></span> <span data-ttu-id="83993-145">V takovém případě můžete jednoduše rozšířit existující tabulky k dočasné, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="83993-145">In this case, you can simply extend the existing table to become temporal, as shown in the following example:</span></span>

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

## <a name="step-2-run-your-workload-regularly"></a><span data-ttu-id="83993-146">Krok 2: Spuštění úlohy pravidelně</span><span class="sxs-lookup"><span data-stu-id="83993-146">Step 2: Run your workload regularly</span></span>
<span data-ttu-id="83993-147">Hlavní výhodou dočasné tabulky je, že není nutné změnit nebo upravit svůj web žádným způsobem provést sledování změn.</span><span class="sxs-lookup"><span data-stu-id="83993-147">The main advantage of Temporal Tables is that you do not need to change or adjust your website in any way to perform change tracking.</span></span> <span data-ttu-id="83993-148">Po vytvoření dočasných tabulek se pokaždé, když provádíte změny na vaše data transparentně zachovat předchozí verze řádku.</span><span class="sxs-lookup"><span data-stu-id="83993-148">Once created, Temporal Tables transparently persist previous row versions every time you perform modifications on your data.</span></span> 

<span data-ttu-id="83993-149">Aby mohli využít automatické sledování změn pro tento konkrétní scénář, umožňuje pouze aktualizovat sloupec **PagesVisited** pokaždé, když se při ukončení uživatele za své relace na webu:</span><span class="sxs-lookup"><span data-stu-id="83993-149">In order to leverage automatic change tracking for this particular scenario, let’s just update column **PagesVisited** every time when user ends her/his session on the website:</span></span>

````
UPDATE WebsiteUserInfo  SET [PagesVisited] = 5 
WHERE [UserID] = 1;
````

<span data-ttu-id="83993-150">Je důležité si všimněte si, že aktualizace dotaz nemusí vědět přesný čas při aktuální operace došlo k chybě, ani jak historická data se zachovají pro budoucí analýzu.</span><span class="sxs-lookup"><span data-stu-id="83993-150">It is important to notice that the update query doesn’t need to know the exact time when the actual operation occurred nor how historical data will be preserved for future analysis.</span></span> <span data-ttu-id="83993-151">Oba tyto aspekty jsou automaticky zpracovávány databáze SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="83993-151">Both aspects are automatically handled by the Azure SQL Database.</span></span> <span data-ttu-id="83993-152">Následující diagram znázorňuje, jak má být vygenerován data o historii při každé aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="83993-152">The following diagram illustrates how history data is being generated on every update.</span></span>

![TemporalArchitecture](./media/sql-database-temporal-tables/AzureTemporal5.png)

## <a name="step-3-perform-historical-data-analysis"></a><span data-ttu-id="83993-154">Krok 3: Provedení analýza historických dat</span><span class="sxs-lookup"><span data-stu-id="83993-154">Step 3: Perform historical data analysis</span></span>
<span data-ttu-id="83993-155">Teď když je povolené dočasné systému – Správa verzí, analýza historických dat je pouze jeden dotazu od vás.</span><span class="sxs-lookup"><span data-stu-id="83993-155">Now when temporal system-versioning is enabled, historical data analysis is just one query away from you.</span></span> <span data-ttu-id="83993-156">V tomto článku, poskytujeme několik příkladů, které řeší běžné scénáře analýzy - další všechny podrobnosti, prozkoumejte různé možnosti zavedené [FOR SYSTEM_TIME](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_3) klauzule.</span><span class="sxs-lookup"><span data-stu-id="83993-156">In this article, we will provide a few examples that address common analysis scenarios - to learn all details, explore various options introduced with the [FOR SYSTEM_TIME](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_3) clause.</span></span>

<span data-ttu-id="83993-157">Chcete-li zobrazit top 10 uživatele seřazených podle čísla navštívené webové stránky od hodinou, spusťte tento dotaz:</span><span class="sxs-lookup"><span data-stu-id="83993-157">To see the top 10 users ordered by the number of visited web pages as of an hour ago, run this query:</span></span>

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
SELECT TOP 10 * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME AS OF @hourAgo
ORDER BY PagesVisited DESC
````

<span data-ttu-id="83993-158">Můžete snadno upravit tento dotaz k analýze navštívených stránek od před jedním dnem, měsícem nebo libovolném okamžiku v minulosti chcete.</span><span class="sxs-lookup"><span data-stu-id="83993-158">You can easily modify this query to analyze the site visits as of a day ago, a month ago or at any point in the past you wish.</span></span>

<span data-ttu-id="83993-159">Pokud chcete provést základní statistické analýzy za předchozí den, použijte následující příklad:</span><span class="sxs-lookup"><span data-stu-id="83993-159">To perform basic statistical analysis for the previous day, use the following example:</span></span>

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

<span data-ttu-id="83993-160">K vyhledání aktivit konkrétního uživatele, v určitou dobu, použijte klauzuli obsažené:</span><span class="sxs-lookup"><span data-stu-id="83993-160">To search for activities of a specific user, within a period of time, use the CONTAINED IN clause:</span></span>

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
DECLARE @twoHoursAgo datetime2 = DATEADD(HOUR, -2, SYSUTCDATETIME());
SELECT * FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME CONTAINED IN (@twoHoursAgo, @hourAgo)
WHERE [UserID] = 1;
````

<span data-ttu-id="83993-161">Grafické vizualizace je zvlášť vhodné pro dočasné dotazy, jak můžete zobrazit trendy a vzorce používání v intuitivní způsob velmi snadno:</span><span class="sxs-lookup"><span data-stu-id="83993-161">Graphic visualization is especially convenient for temporal queries as you can show trends and usage patterns in an intuitive way very easily:</span></span>

![TemporalGraph](./media/sql-database-temporal-tables/AzureTemporal6.png)

## <a name="evolving-table-schema"></a><span data-ttu-id="83993-163">Vyvíjející se schéma tabulky</span><span class="sxs-lookup"><span data-stu-id="83993-163">Evolving table schema</span></span>
<span data-ttu-id="83993-164">Obvykle bude muset změnit schéma dočasnou tabulku, při práci vývoj aplikací.</span><span class="sxs-lookup"><span data-stu-id="83993-164">Typically, you will need to change the temporal table schema while you are doing app development.</span></span> <span data-ttu-id="83993-165">Pro tento jednoduše spusťte regulární příkaz ALTER TABLE příkazů a Azure SQL Database správně rozšíří změny do tabulky historie.</span><span class="sxs-lookup"><span data-stu-id="83993-165">For that, simply run regular ALTER TABLE statements and Azure SQL Database will appropriately propagate changes to the history table.</span></span> <span data-ttu-id="83993-166">Tento skript je ukázkou, jak můžete přidat další atribut pro sledování:</span><span class="sxs-lookup"><span data-stu-id="83993-166">The following script shows how you can add additional attribute for tracking:</span></span>

````
/*Add new column for tracking source IP address*/
ALTER TABLE dbo.WebsiteUserInfo 
ADD  [IPAddress] varchar(128) NOT NULL CONSTRAINT DF_Address DEFAULT 'N/A';
````

<span data-ttu-id="83993-167">Podobně můžete změnit definici sloupce při aktivní úlohy:</span><span class="sxs-lookup"><span data-stu-id="83993-167">Similarly, you can change column definition while your workload is active:</span></span>

````
/*Increase the length of name column*/
ALTER TABLE dbo.WebsiteUserInfo 
    ALTER COLUMN  UserName nvarchar(256) NOT NULL;
````

<span data-ttu-id="83993-168">Nakonec můžete odebrat sloupec, který už nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="83993-168">Finally, you can remove a column that you do not need anymore.</span></span>

````
/*Drop unnecessary column */
ALTER TABLE dbo.WebsiteUserInfo 
    DROP COLUMN TemporaryColumn; 
````

<span data-ttu-id="83993-169">Můžete taky použít nejnovější [SSDT](https://msdn.microsoft.com/library/mt204009.aspx) změnit dočasná tabulka schématu během připojení k databázi (online režim) nebo jako součást k databázovému projektu (offline režim).</span><span class="sxs-lookup"><span data-stu-id="83993-169">Alternatively, use latest [SSDT](https://msdn.microsoft.com/library/mt204009.aspx) to change temporal table schema while you are connected to the database (online mode) or as part of the database project (offline mode).</span></span>

## <a name="controlling-retention-of-historical-data"></a><span data-ttu-id="83993-170">Řízení uchovávání historických dat</span><span class="sxs-lookup"><span data-stu-id="83993-170">Controlling retention of historical data</span></span>
<span data-ttu-id="83993-171">Dočasných tabulek se systémovou správou verzí může zvýšit v tabulce historie regulérních tabulkách velikost databáze více než.</span><span class="sxs-lookup"><span data-stu-id="83993-171">With system-versioned temporal tables, the history table may increase the database size more than regular tables.</span></span> <span data-ttu-id="83993-172">Tabulky historie velké a stále se rozšiřujícího se může stát problém i z důvodu náklady na úložiště čistá, jakož i nastavení výkonu daňové na dočasné dotazování.</span><span class="sxs-lookup"><span data-stu-id="83993-172">A large and ever-growing history table can become an issue both due to pure storage costs as well as imposing a performance tax on temporal querying.</span></span> <span data-ttu-id="83993-173">Proto vývoj data zásady uchovávání informací pro správu dat v tabulce historie je důležitým aspektem plánování a správu životního cyklu každých dočasnou tabulku.</span><span class="sxs-lookup"><span data-stu-id="83993-173">Hence, developing a data retention policy for managing data in the history table is an important aspect of planning and managing the lifecycle of every temporal table.</span></span> <span data-ttu-id="83993-174">S Azure SQL Database máte následující přístupy pro správu historická data v dočasné tabulce:</span><span class="sxs-lookup"><span data-stu-id="83993-174">With Azure SQL Database, you have the following approaches for managing historical data in the temporal table:</span></span>

* [<span data-ttu-id="83993-175">Vytváření oddílů tabulky</span><span class="sxs-lookup"><span data-stu-id="83993-175">Table Partitioning</span></span>](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_2)
* [<span data-ttu-id="83993-176">Skript pro vyčištění vlastní</span><span class="sxs-lookup"><span data-stu-id="83993-176">Custom Cleanup Script</span></span>](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_3)

## <a name="next-steps"></a><span data-ttu-id="83993-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="83993-177">Next steps</span></span>
<span data-ttu-id="83993-178">Podrobné informace o dočasné tabulky, podívejte se na [dokumentace MSDN](https://msdn.microsoft.com/library/dn935015.aspx).</span><span class="sxs-lookup"><span data-stu-id="83993-178">For detailed information on Temporal Tables, check out [MSDN documentation](https://msdn.microsoft.com/library/dn935015.aspx).</span></span>
<span data-ttu-id="83993-179">Navštivte Channel 9 a poslechnout [scénáře úspěchu dočasné implementace skutečné zákazníka](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) a sledujte [live dočasné ukázkový](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).</span><span class="sxs-lookup"><span data-stu-id="83993-179">Visit Channel 9 to hear a [real customer temporal implemenation success story](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) and watch a [live temporal demonstration](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).</span></span>

