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
# <a name="manage-historical-data-in-temporal-tables-with-retention-policy"></a><span data-ttu-id="b69db-103">Správa historických dat v dočasných tabulek se zásady uchovávání informací</span><span class="sxs-lookup"><span data-stu-id="b69db-103">Manage historical data in Temporal Tables with retention policy</span></span>
<span data-ttu-id="b69db-104">Dočasné tabulky může zvýšit velikost databáze více než regulérních tabulkách, zejména v případě, že zachováte historických dat pro delší časové období.</span><span class="sxs-lookup"><span data-stu-id="b69db-104">Temporal Tables may increase database size more than regular tables, especially if you retain historical data for a longer period of time.</span></span> <span data-ttu-id="b69db-105">Proto zásady uchovávání historických dat je důležitým aspektem plánování a správě životního cyklu hello každých dočasné tabulky.</span><span class="sxs-lookup"><span data-stu-id="b69db-105">Hence, retention policy for historical data is an important aspect of planning and managing hello lifecycle of every temporal table.</span></span> <span data-ttu-id="b69db-106">Dočasné tabulky v databázi SQL Azure se dodávají s snadno použitelné uchování mechanismus, který umožňuje provedení této úlohy.</span><span class="sxs-lookup"><span data-stu-id="b69db-106">Temporal Tables in Azure SQL Database come with easy-to-use retention mechanism that helps you accomplish this task.</span></span>

<span data-ttu-id="b69db-107">Uchování dočasné historie lze konfigurovat na úrovni hello jednotlivé tabulky, což umožňuje uživatelům, které toocreate flexibilní stárnutí zásady.</span><span class="sxs-lookup"><span data-stu-id="b69db-107">Temporal history retention can be configured at hello individual table level, which allows users toocreate flexible aging polices.</span></span> <span data-ttu-id="b69db-108">Použití dočasné uchování je jednoduchý: vyžaduje jenom jeden parametr toobe nastavit během změny schématu nebo vytvoření tabulky.</span><span class="sxs-lookup"><span data-stu-id="b69db-108">Applying temporal retention is simple: it requires only one parameter toobe set during table creation or schema change.</span></span>

<span data-ttu-id="b69db-109">Po definování zásad uchovávání informací, Azure SQL Database spustí pravidelně kontroluje, zda existují historických řádků, které jsou způsobilé pro automatické čištění.</span><span class="sxs-lookup"><span data-stu-id="b69db-109">After you define retention policy, Azure SQL Database starts checking regularly if there are historical rows that are eligible for automatic data cleanup.</span></span> <span data-ttu-id="b69db-110">Identifikace odpovídající řádky a jejich odebrání z tabulky historie hello dojít transparentně, úlohy na pozadí hello naplánovat a spustit hello systémem.</span><span class="sxs-lookup"><span data-stu-id="b69db-110">Identification of matching rows and their removal from hello history table occur transparently, in hello background task that is scheduled and run by hello system.</span></span> <span data-ttu-id="b69db-111">Stáří podmínku pro řádky tabulky historie hello je zaškrtnuta možnost podle sloupce hello představující konec období SYSTEM_TIME.</span><span class="sxs-lookup"><span data-stu-id="b69db-111">Age condition for hello history table rows is checked based on hello column representing end of SYSTEM_TIME period.</span></span> <span data-ttu-id="b69db-112">Pokud doba uchování, je třeba nastavit toosix měsíců, řádky tabulky, které jsou vhodné pro čištění uspokojení hello následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="b69db-112">If retention period, for example, is set toosix months, table rows eligible for cleanup satisfy hello following condition:</span></span>

````
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
````

<span data-ttu-id="b69db-113">V předchozím příkladu hello, budeme předpokládat **ValidTo** sloupec odpovídá toohello konec období SYSTEM_TIME.</span><span class="sxs-lookup"><span data-stu-id="b69db-113">In hello preceding example, we assumed that **ValidTo** column corresponds toohello end of SYSTEM_TIME period.</span></span>

## <a name="how-tooconfigure-retention-policy"></a><span data-ttu-id="b69db-114">Jak zásady uchovávání informací tooconfigure?</span><span class="sxs-lookup"><span data-stu-id="b69db-114">How tooconfigure retention policy?</span></span>
<span data-ttu-id="b69db-115">Než začnete konfigurovat zásady uchovávání informací pro dočasnou tabulku, proveďte nejprve kontrolu zda je povoleno dočasné uchovávání historických *na úrovni databáze hello*.</span><span class="sxs-lookup"><span data-stu-id="b69db-115">Before you configure retention policy for a temporal table, check first whether temporal historical retention is enabled *at hello database level*.</span></span>

````
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
````

<span data-ttu-id="b69db-116">Databáze příznak **is_temporal_history_retention_enabled** je sada tooON ve výchozím nastavení, ale uživatelé mohou změnit pomocí příkazu ALTER DATABASE.</span><span class="sxs-lookup"><span data-stu-id="b69db-116">Database flag **is_temporal_history_retention_enabled** is set tooON by default, but users can change it with ALTER DATABASE statement.</span></span> <span data-ttu-id="b69db-117">Je také automaticky sadu tooOFF po [obnovení bodu v čase](sql-database-recovery-using-backups.md) operaci.</span><span class="sxs-lookup"><span data-stu-id="b69db-117">It is also automatically set tooOFF after [point in time restore](sql-database-recovery-using-backups.md) operation.</span></span> <span data-ttu-id="b69db-118">tooenable dočasné uchování vyčištění pro vaši databázi, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="b69db-118">tooenable temporal history retention cleanup for your database, execute hello following statement:</span></span>

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

> [!IMPORTANT]
> <span data-ttu-id="b69db-119">Uchování pro dočasné tabulky, i když můžete nakonfigurovat **is_temporal_history_retention_enabled** je VYPNUTÝ, ale není v takovém případě aktivuje automatické čištění zastaralá řádků.</span><span class="sxs-lookup"><span data-stu-id="b69db-119">You can configure retention for temporal tables even if **is_temporal_history_retention_enabled** is OFF, but automatic cleanup for aged rows is not triggered in that case.</span></span>
> 
> 

<span data-ttu-id="b69db-120">Zásady uchovávání informací je nakonfigurované během vytváření tabulky zadáním hodnoty pro parametr HISTORY_RETENTION_PERIOD hello:</span><span class="sxs-lookup"><span data-stu-id="b69db-120">Retention policy is configured during table creation by specifying value for hello HISTORY_RETENTION_PERIOD parameter:</span></span>

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

<span data-ttu-id="b69db-121">Databáze SQL Azure můžete dobu uchování toospecify pomocí různých časových jednotek: počet dnů, týdnů, měsíců a let.</span><span class="sxs-lookup"><span data-stu-id="b69db-121">Azure SQL Database allows you toospecify retention period by using different time units: DAYS, WEEKS, MONTHS, and YEARS.</span></span> <span data-ttu-id="b69db-122">Pokud je vynechán HISTORY_RETENTION_PERIOD, se předpokládá NEKONEČNÉ uchování.</span><span class="sxs-lookup"><span data-stu-id="b69db-122">If HISTORY_RETENTION_PERIOD is omitted, INFINITE retention is assumed.</span></span> <span data-ttu-id="b69db-123">Můžete taky NEKONEČNÉ – klíčové slovo explicitně.</span><span class="sxs-lookup"><span data-stu-id="b69db-123">You can also use INFINITE keyword explicitly.</span></span>

<span data-ttu-id="b69db-124">V některých scénářích může být vhodné tooconfigure uchování po vytvoření tabulky nebo toochange dříve nakonfigurované hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b69db-124">In some scenarios, you may want tooconfigure retention after table creation, or toochange previously configured value.</span></span> <span data-ttu-id="b69db-125">V takovém případě použijte příkaz ALTER TABLE:</span><span class="sxs-lookup"><span data-stu-id="b69db-125">In that case use ALTER TABLE statement:</span></span>

````
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
````

> [!IMPORTANT]
> <span data-ttu-id="b69db-126">Nastavení SYSTEM_VERSIONING tooOFF *nezachovává* hodnotu doby uchování.</span><span class="sxs-lookup"><span data-stu-id="b69db-126">Setting SYSTEM_VERSIONING tooOFF *does not preserve* retention period value.</span></span> <span data-ttu-id="b69db-127">Nastavení SYSTEM_VERSIONING tooON bez HISTORY_RETENTION_PERIOD explicitně zadat výsledky v hello NEKONEČNOU dobu uchování.</span><span class="sxs-lookup"><span data-stu-id="b69db-127">Setting SYSTEM_VERSIONING tooON without HISTORY_RETENTION_PERIOD specified explicitly results in hello INFINITE retention period.</span></span>
> 
> 

<span data-ttu-id="b69db-128">tooreview aktuální stav hello zásady uchovávání informací, použijte následující hello dotaz tento příznak spojení dočasné uchování povolování na úrovni databáze hello s dob uchování u jednotlivých tabulek:</span><span class="sxs-lookup"><span data-stu-id="b69db-128">tooreview current state of hello retention policy, use hello following query that joins temporal retention enablement flag at hello database level with retention periods for individual tables:</span></span>

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


## <a name="how-sql-database-deletes-aged-rows"></a><span data-ttu-id="b69db-129">Jak SQL Database odstraní stará řádky?</span><span class="sxs-lookup"><span data-stu-id="b69db-129">How SQL Database deletes aged rows?</span></span>
<span data-ttu-id="b69db-130">proces vyčištění Hello závisí na hello index rozložení tabulky historie hello.</span><span class="sxs-lookup"><span data-stu-id="b69db-130">hello cleanup process depends on hello index layout of hello history table.</span></span> <span data-ttu-id="b69db-131">Je důležité toonotice, *pouze tabulky historie clusterovaný index (B-stromu nebo columnstore) může mít omezený uchování zásady nakonfigurované*.</span><span class="sxs-lookup"><span data-stu-id="b69db-131">It is important toonotice that *only history tables with a clustered index (B-tree or columnstore) can have finite retention policy configured*.</span></span> <span data-ttu-id="b69db-132">Úlohy na pozadí se vytvoří tooperform Vyčistit stará data pro všechny dočasné tabulky s dobou uchování omezené.</span><span class="sxs-lookup"><span data-stu-id="b69db-132">A background task is created tooperform aged data cleanup for all temporal tables with finite retention period.</span></span>
<span data-ttu-id="b69db-133">Vyčistit logiku pro clusterovaný index rowstore (B-stromu) hello odstraní stará řádek v menší bloky dat (až too10K) minimalizovat tlak na protokol databáze a vstupně-výstupních operací subsystému.</span><span class="sxs-lookup"><span data-stu-id="b69db-133">Cleanup logic for hello rowstore (B-tree) clustered index deletes aged row in smaller chunks (up too10K) minimizing pressure on database log and I/O subsystem.</span></span> <span data-ttu-id="b69db-134">I když čištění logiku využívá vyžaduje index B-stromu, pořadí odstranění pro starší než doba uchování dat se nedá zaručit pevně hello řádky.</span><span class="sxs-lookup"><span data-stu-id="b69db-134">Although cleanup logic utilizes required B-tree index, order of deletions for hello rows older than retention period cannot be firmly guaranteed.</span></span> <span data-ttu-id="b69db-135">Proto *nepřebírají žádné závislostí v pořadí čištění hello ve svých aplikacích*.</span><span class="sxs-lookup"><span data-stu-id="b69db-135">Hence, *do not take any dependency on hello cleanup order in your applications*.</span></span>

<span data-ttu-id="b69db-136">Hello úlohu čištění pro columnstore hello clusteru odebere celý [řádek skupiny](https://msdn.microsoft.com/library/gg492088.aspx) najednou (obvykle obsahují 1 milionu řádků každý), což je velmi efektivní, zejména v případě, že historická data se generuje vysoký tempem.</span><span class="sxs-lookup"><span data-stu-id="b69db-136">hello cleanup task for hello clustered columnstore removes entire [row groups](https://msdn.microsoft.com/library/gg492088.aspx) at once (typically contain 1 million of rows each), which is very efficient, especially when historical data is generated at a high pace.</span></span>

![Clusterované columnstore uchování](./media/sql-database-temporal-tables-retention-policy/cciretention.png)

<span data-ttu-id="b69db-138">Komprese dat vynikající a efektivní uchování čištění díky clusterovaný columnstore index ideální volbou pro scénáře, když vaše úlohy generují rychle vysoký objem historická data.</span><span class="sxs-lookup"><span data-stu-id="b69db-138">Excellent data compression and efficient retention cleanup makes clustered columnstore index a perfect choice for scenarios when your workload rapidly generates high amount of historical data.</span></span> <span data-ttu-id="b69db-139">Tento vzor je typické pro náročné na prostředky [úlohy zpracování transakcí, které používají dočasné tabulky](https://msdn.microsoft.com/library/mt631669.aspx) pro sledování změn a auditování, analýza trendu nebo přijímání dat IoT.</span><span class="sxs-lookup"><span data-stu-id="b69db-139">That pattern is typical for intensive [transactional processing workloads that use temporal tables](https://msdn.microsoft.com/library/mt631669.aspx) for change tracking and auditing, trend analysis, or IoT data ingestion.</span></span>

## <a name="index-considerations"></a><span data-ttu-id="b69db-140">Aspekty indexu</span><span class="sxs-lookup"><span data-stu-id="b69db-140">Index considerations</span></span>
<span data-ttu-id="b69db-141">úloha vyčištění Hello u tabulek s clusterovaný index rowstore vyžaduje toostart index s hello sloupec odpovídající hello koncem období SYSTEM_TIME.</span><span class="sxs-lookup"><span data-stu-id="b69db-141">hello cleanup task for tables with rowstore clustered index requires index toostart with hello column corresponding hello end of SYSTEM_TIME period.</span></span> <span data-ttu-id="b69db-142">Pokud takový indexu neexistuje, nelze nakonfigurovat doby uchování konečné:</span><span class="sxs-lookup"><span data-stu-id="b69db-142">If such index doesn't exist, you cannot configure a finite retention period:</span></span>

<span data-ttu-id="b69db-143">*Msg 13765, Level 16, State 1 <br> </br> nastavení konečné uchovávají se na dočasné tabulce se systémovou správou verzí temporalstagetestdb.dbo.WebsiteUserInfo, protože tabulka historie hello. temporalstagetestdb.dbo.WebsiteUserInfoHistory' neobsahuje požadované clusterovaný index. Zvažte vytvoření clusteru columnstore nebo B-stromu index začínající hello sloupec, který odpovídá konec SYSTEM_TIME period v tabulce historie hello.*</span><span class="sxs-lookup"><span data-stu-id="b69db-143">*Msg 13765, Level 16, State 1 <br></br> Setting finite retention period failed on system-versioned temporal table 'temporalstagetestdb.dbo.WebsiteUserInfo' because hello history table 'temporalstagetestdb.dbo.WebsiteUserInfoHistory' does not contain required clustered index. Consider creating a clustered columnstore or B-tree index starting with hello column that matches end of SYSTEM_TIME period, on hello history table.*</span></span>

<span data-ttu-id="b69db-144">Je důležité, že toonotice, který hello Tabulka historie výchozí vytvořené databáze SQL Azure již má clusterovaný index, který je kompatibilní s pro zásady uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="b69db-144">It is important toonotice that hello default history table created by Azure SQL Database already has clustered index, which is compliant for retention policy.</span></span> <span data-ttu-id="b69db-145">Pokud se pokusíte tooremove tohoto indexu v tabulce s dobou uchování omezené, operace selže s touto hello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="b69db-145">If you try tooremove that index on a table with finite retention period, operation fails with hello following error:</span></span>

<span data-ttu-id="b69db-146">*Msg 13766, Level 16, State 1 <br> </br> hello clusterovaný index 'WebsiteUserInfoHistory.IX_WebsiteUserInfoHistory' nelze vyřadit, protože je používán pro automatické čištění zastaralá data. Pokud potřebujete toodrop tento index, zvažte nastavení HISTORY_RETENTION_PERIOD tooINFINITE na hello odpovídající systémovou správou verzí dočasnou tabulku.*</span><span class="sxs-lookup"><span data-stu-id="b69db-146">*Msg 13766, Level 16, State 1 <br></br> Cannot drop hello clustered index 'WebsiteUserInfoHistory.IX_WebsiteUserInfoHistory' because it is being used for automatic cleanup of aged data. Consider setting HISTORY_RETENTION_PERIOD tooINFINITE on hello corresponding system-versioned temporal table if you need toodrop this index.*</span></span>

<span data-ttu-id="b69db-147">Čištění na hello clusterovaný index columnstore funguje optimálně vložit historických řádků ve vzestupném pořadí (seřazených podle hello konec sloupec období), který se vždy hello nestane, pokud je tabulka historie hello naplněn výhradně hello na text hello VERSIONIOING mechanismus.</span><span class="sxs-lookup"><span data-stu-id="b69db-147">Cleanup on hello clustered columnstore index works optimally if historical rows are inserted in hello ascending order (ordered by hello end of period column), which is always hello case when hello history table is populated exclusively by hello SYSTEM_VERSIONIOING mechanism.</span></span> <span data-ttu-id="b69db-148">Pokud nejsou řádky v tabulce historie hello seřazené podle konec sloupec období (který může být případ hello, pokud jste migrovali existující historická data), je třeba znovu vytvořit clusterovaný index columnstore nad B-stromu vytvořit index, který je správně seřadit, tooachieve optimální výkon.</span><span class="sxs-lookup"><span data-stu-id="b69db-148">If rows in hello history table are not ordered by end of period column (which may be hello case if you migrated existing historical data), you should re-create clustered columnstore index on top of B-tree rowstore index that is properly ordered, tooachieve optimal performance.</span></span>

<span data-ttu-id="b69db-149">Vyhněte se znovu sestavit clusterovaný index columnstore v tabulce historie hello s dobou uchování konečné hello, protože může změnit, řazení do skupin řádků hello přirozeně vyvolané operace hello systémové správy verzí.</span><span class="sxs-lookup"><span data-stu-id="b69db-149">Avoid rebuilding clustered columnstore index on hello history table with hello finite retention period, because it may change ordering in hello row groups naturally imposed by hello system-versioning operation.</span></span> <span data-ttu-id="b69db-150">Pokud potřebujete toorebuild clusterovaný index columnstore v tabulce historie hello, učiňte tak, že ho znovu vytvoříte nad kompatibilní index B-stromu, zachování pořadí v hello rowgroups potřebné pro čištění regulární dat.</span><span class="sxs-lookup"><span data-stu-id="b69db-150">If you need toorebuild clustered columnstore index on hello history table, do that by re-creating it on top of compliant B-tree index, preserving ordering in hello rowgroups necessary for regular data cleanup.</span></span> <span data-ttu-id="b69db-151">Dobrý den, které by měla být provedena stejný přístup, pokud vytvoříte dočasnou tabulku s stávající tabulka historie, který obsahuje clusterovaný index sloupce bez dat zaručenou pořadí:</span><span class="sxs-lookup"><span data-stu-id="b69db-151">hello same approach should be taken if you create temporal table with existing history table that has clustered column index without guaranteed data order:</span></span>

````
/*Create B-tree ordered by hello end of period column*/
CREATE CLUSTERED INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory (ValidTo)
WITH (DROP_EXISTING = ON);
GO
/*Re-create clustered columnstore index*/
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON);
````

<span data-ttu-id="b69db-152">Pokud dobu uchování konečné je nakonfigurována pro tabulku historie hello s hello clusterovaný index columnstore, nelze vytvořit další neclusterovaných indexů B-stromu v této tabulce:</span><span class="sxs-lookup"><span data-stu-id="b69db-152">When finite retention period is configured for hello history table with hello clustered columnstore index, you cannot create additional non-clustered B-tree indexes on that table:</span></span>

````
CREATE NONCLUSTERED INDEX IX_WebHistNCI ON WebsiteUserInfoHistory ([UserName])
````

<span data-ttu-id="b69db-153">Pokusu o tooexecute výše příkaz selže s touto hello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="b69db-153">An attempt tooexecute above statement fails with hello following error:</span></span>

<span data-ttu-id="b69db-154">*Msg 13772, Level 16, State 1 <br> </br> nelze vytvořit neclusterovaný index na dočasnou tabulku historie 'WebsiteUserInfoHistory, protože má doba uchovávání omezené a clusterovaný index columnstore definované.*</span><span class="sxs-lookup"><span data-stu-id="b69db-154">*Msg 13772, Level 16, State 1 <br></br> Cannot create non-clustered index on a temporal history table 'WebsiteUserInfoHistory' since it has finite retention period and clustered columnstore index defined.*</span></span>

## <a name="querying-tables-with-retention-policy"></a><span data-ttu-id="b69db-155">Dotazy na tabulky s zásady uchovávání informací</span><span class="sxs-lookup"><span data-stu-id="b69db-155">Querying tables with retention policy</span></span>
<span data-ttu-id="b69db-156">Všechny dotazy na dočasnou tabulku hello automaticky vyfiltrovat historických řádky odpovídající zásady uchovávání informací omezené, tooavoid nepředvídatelným a konzistentní výsledky, protože zastaralá řádky může odstranit úlohu čištění hello, *v libovolném bodě v čase a v libovolný pořadí*.</span><span class="sxs-lookup"><span data-stu-id="b69db-156">All queries on hello temporal table automatically filter out historical rows matching finite retention policy, tooavoid unpredictable and inconsistent results, since aged rows can be deleted by hello cleanup task, *at any point in time and in arbitrary order*.</span></span>

<span data-ttu-id="b69db-157">Hello následující obrázek znázorňuje hello plán dotazu pro jednoduchý dotaz:</span><span class="sxs-lookup"><span data-stu-id="b69db-157">hello following picture shows hello query plan for a simple query:</span></span>

````
SELECT * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME ALL;
````

<span data-ttu-id="b69db-158">Hello dotazu, plán zahrnuje další filtr použít tooend sloupec období (ValidTo) v operátoru hello kontrolovat clusterovaný Index pro tabulku historie hello (zvýrazněné).</span><span class="sxs-lookup"><span data-stu-id="b69db-158">hello query plan includes additional filter applied tooend of period column (ValidTo) in hello Clustered Index Scan operator on hello history table (highlighted).</span></span> <span data-ttu-id="b69db-159">Tento příklad předpokládá u tabulky WebsiteUserInfo se nastavil této doby uchování jeden měsíc.</span><span class="sxs-lookup"><span data-stu-id="b69db-159">This example assumes that one MONTH retention period was set on WebsiteUserInfo table.</span></span>

![Filtr dotazu uchování](./media/sql-database-temporal-tables-retention-policy/queryexecplanwithretention.png)

<span data-ttu-id="b69db-161">Pokud dotaz Tabulka historie přímo, však může zobrazit řádky, které jsou starší než uchovávání období, ale bez jakékoli záruky na výsledky dotazu opakovatelných.</span><span class="sxs-lookup"><span data-stu-id="b69db-161">However, if you query history table directly, you may see rows that are older than specified retention period, but without any guarantee for repeatable query results.</span></span> <span data-ttu-id="b69db-162">Hello následující obrázek znázorňuje provedení plán dotazu pro dotaz hello v tabulce historie hello bez další filtry použít:</span><span class="sxs-lookup"><span data-stu-id="b69db-162">hello following picture shows query execution plan for hello query on hello history table without additional filters applied:</span></span>

![Dotazování historie bez uchování filtru](./media/sql-database-temporal-tables-retention-policy/queryexecplanhistorytable.png)

<span data-ttu-id="b69db-164">Nespoléhejte obchodní logiky na čtení tabulky historie mimo dobu uchování, jak můžete obdržet nekonzistentní nebo neočekávané výsledky.</span><span class="sxs-lookup"><span data-stu-id="b69db-164">Do not rely your business logic on reading history table beyond retention period as you may get inconsistent or unexpected results.</span></span> <span data-ttu-id="b69db-165">Doporučujeme použít dočasné dotazy s klauzulí FOR SYSTEM_TIME pro analýzu dat v dočasných tabulek.</span><span class="sxs-lookup"><span data-stu-id="b69db-165">We recommend that you use temporal queries with FOR SYSTEM_TIME clause for analyzing data in temporal tables.</span></span>

## <a name="point-in-time-restore-considerations"></a><span data-ttu-id="b69db-166">Přejděte v aspekty čas obnovení</span><span class="sxs-lookup"><span data-stu-id="b69db-166">Point in time restore considerations</span></span>
<span data-ttu-id="b69db-167">Když vytvoříte novou databázi pomocí [obnovení existující databáze tooa konkrétní bod v čase](sql-database-recovery-using-backups.md), má dočasné uchovávání dat na úrovni databáze hello zakázány.</span><span class="sxs-lookup"><span data-stu-id="b69db-167">When you create new database by [restoring existing database tooa specific point in time](sql-database-recovery-using-backups.md), it has temporal retention disabled at hello database level.</span></span> <span data-ttu-id="b69db-168">(**is_temporal_history_retention_enabled** nastaven příznak tooOFF).</span><span class="sxs-lookup"><span data-stu-id="b69db-168">(**is_temporal_history_retention_enabled** flag set tooOFF).</span></span> <span data-ttu-id="b69db-169">Tato funkce vám umožní tooexamine všechny řádky historických při obnovení, aniž byste museli, které stará řádky jsou odebrány, než získáte tooquery je.</span><span class="sxs-lookup"><span data-stu-id="b69db-169">This functionality allows you tooexamine all historical rows upon restore, without worrying that aged rows are removed before you get tooquery them.</span></span> <span data-ttu-id="b69db-170">Můžete ji použít příliš*zkontrolovat historická data i po uplynutí doby uchování nakonfigurované*.</span><span class="sxs-lookup"><span data-stu-id="b69db-170">You can use it too*inspect historical data beyond configured retention period*.</span></span>

<span data-ttu-id="b69db-171">Řekněme, že dočasná tabulka má období uchování jeden měsíc.</span><span class="sxs-lookup"><span data-stu-id="b69db-171">Say that a temporal table has one MONTH retention period specified.</span></span> <span data-ttu-id="b69db-172">Pokud vaše databáze byla vytvořena v rámci úrovně služby Premium, bude možné toocreate kopie databáze se stav databáze hello až too35 dnů zpět v posledních hello.</span><span class="sxs-lookup"><span data-stu-id="b69db-172">If your database was created in Premium Service tier, you would be able toocreate database copy with hello database state up too35 days back in hello past.</span></span> <span data-ttu-id="b69db-173">Která efektivně umožní tooanalyze historických řádků, které jsou too65 dny přímým dotazováním hello Tabulka historie.</span><span class="sxs-lookup"><span data-stu-id="b69db-173">That effectively would allow you tooanalyze historical rows that are up too65 days old by querying hello history table directly.</span></span>

<span data-ttu-id="b69db-174">Pokud chcete vyčistit dočasné uchování tooactivate, spusťte následující příkaz Transact-SQL v době obnovení za bod hello:</span><span class="sxs-lookup"><span data-stu-id="b69db-174">If you want tooactivate temporal retention cleanup, run hello following Transact-SQL statement after point in time restore:</span></span>

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

## <a name="next-steps"></a><span data-ttu-id="b69db-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b69db-175">Next steps</span></span>
<span data-ttu-id="b69db-176">jak toouse dočasných tabulek v aplikacích, podívejte se na toolearn [Začínáme s dočasné tabulky v databázi SQL Azure](sql-database-temporal-tables.md).</span><span class="sxs-lookup"><span data-stu-id="b69db-176">toolearn how toouse Temporal Tables in your applications, check out [Getting Started with Temporal Tables in Azure SQL Database](sql-database-temporal-tables.md).</span></span>

<span data-ttu-id="b69db-177">Navštivte Channel 9 toohear [skutečné zákazníka dočasné implementace úspěšné scénáře](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) a sledujte [live dočasné ukázkový](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).</span><span class="sxs-lookup"><span data-stu-id="b69db-177">Visit Channel 9 toohear a [real customer temporal implementation success story](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) and watch a [live temporal demonstration](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).</span></span>

<span data-ttu-id="b69db-178">Podrobné informace o dočasné tabulky, zkontrolujte [dokumentace MSDN](https://msdn.microsoft.com/library/dn935015.aspx).</span><span class="sxs-lookup"><span data-stu-id="b69db-178">For detailed information about Temporal Tables, review [MSDN documentation](https://msdn.microsoft.com/library/dn935015.aspx).</span></span>

