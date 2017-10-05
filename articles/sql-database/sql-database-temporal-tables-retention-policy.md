---
title: "Správa historických dat v dočasných tabulek se zásady uchovávání informací | Microsoft Docs"
description: "Další informace o použití zásad dočasné uchovávání informací pro historických dat pod kontrolou."
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
ms.openlocfilehash: 8975d7a7d39114b2758d64a4df9f992cba6bf561
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-historical-data-in-temporal-tables-with-retention-policy"></a><span data-ttu-id="b94ae-103">Správa historických dat v dočasných tabulek se zásady uchovávání informací</span><span class="sxs-lookup"><span data-stu-id="b94ae-103">Manage historical data in Temporal Tables with retention policy</span></span>
<span data-ttu-id="b94ae-104">Dočasné tabulky může zvýšit velikost databáze více než regulérních tabulkách, zejména v případě, že zachováte historických dat pro delší časové období.</span><span class="sxs-lookup"><span data-stu-id="b94ae-104">Temporal Tables may increase database size more than regular tables, especially if you retain historical data for a longer period of time.</span></span> <span data-ttu-id="b94ae-105">Proto zásady uchovávání historických dat je důležitým aspektem plánování a správu životního cyklu každých dočasnou tabulku.</span><span class="sxs-lookup"><span data-stu-id="b94ae-105">Hence, retention policy for historical data is an important aspect of planning and managing the lifecycle of every temporal table.</span></span> <span data-ttu-id="b94ae-106">Dočasné tabulky v databázi SQL Azure se dodávají s snadno použitelné uchování mechanismus, který umožňuje provedení této úlohy.</span><span class="sxs-lookup"><span data-stu-id="b94ae-106">Temporal Tables in Azure SQL Database come with easy-to-use retention mechanism that helps you accomplish this task.</span></span>

<span data-ttu-id="b94ae-107">Uchování dočasné historie může být nakonfigurována na úrovni jednotlivých tabulce, což umožňuje uživatelům vytvářet flexibilní stárnutí zásady.</span><span class="sxs-lookup"><span data-stu-id="b94ae-107">Temporal history retention can be configured at the individual table level, which allows users to create flexible aging polices.</span></span> <span data-ttu-id="b94ae-108">Použití dočasné uchování je jednoduchý: vyžaduje jenom jeden parametr nastavit během změny schématu nebo vytvoření tabulky.</span><span class="sxs-lookup"><span data-stu-id="b94ae-108">Applying temporal retention is simple: it requires only one parameter to be set during table creation or schema change.</span></span>

<span data-ttu-id="b94ae-109">Po definování zásad uchovávání informací, Azure SQL Database spustí pravidelně kontroluje, zda existují historických řádků, které jsou způsobilé pro automatické čištění.</span><span class="sxs-lookup"><span data-stu-id="b94ae-109">After you define retention policy, Azure SQL Database starts checking regularly if there are historical rows that are eligible for automatic data cleanup.</span></span> <span data-ttu-id="b94ae-110">Identifikace odpovídající řádky a jejich odebrání z tabulky historie dojít transparentně, v pozadí úloha, která je naplánována a spustit v systému.</span><span class="sxs-lookup"><span data-stu-id="b94ae-110">Identification of matching rows and their removal from the history table occur transparently, in the background task that is scheduled and run by the system.</span></span> <span data-ttu-id="b94ae-111">Stáří podmínku pro řádky tabulky historie je zaškrtnuta možnost podle sloupce představující konec období SYSTEM_TIME.</span><span class="sxs-lookup"><span data-stu-id="b94ae-111">Age condition for the history table rows is checked based on the column representing end of SYSTEM_TIME period.</span></span> <span data-ttu-id="b94ae-112">Pokud doba uchování, například je nastavené na šest měsíců, řádky tabulky, které jsou vhodné pro čištění splňovat následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="b94ae-112">If retention period, for example, is set to six months, table rows eligible for cleanup satisfy the following condition:</span></span>

````
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
````

<span data-ttu-id="b94ae-113">V předchozím příkladu jsme předpokládá **ValidTo** sloupec odpovídá na konci období SYSTEM_TIME.</span><span class="sxs-lookup"><span data-stu-id="b94ae-113">In the preceding example, we assumed that **ValidTo** column corresponds to the end of SYSTEM_TIME period.</span></span>

## <a name="how-to-configure-retention-policy"></a><span data-ttu-id="b94ae-114">Postup konfigurace zásad uchovávání informací?</span><span class="sxs-lookup"><span data-stu-id="b94ae-114">How to configure retention policy?</span></span>
<span data-ttu-id="b94ae-115">Než začnete konfigurovat zásady uchovávání informací pro dočasnou tabulku, proveďte nejprve kontrolu zda je povoleno dočasné uchovávání historických *na úrovni databáze*.</span><span class="sxs-lookup"><span data-stu-id="b94ae-115">Before you configure retention policy for a temporal table, check first whether temporal historical retention is enabled *at the database level*.</span></span>

````
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
````

<span data-ttu-id="b94ae-116">Databáze příznak **is_temporal_history_retention_enabled** je nastaven na hodnotu ON ve výchozím nastavení, ale uživatelé mohou změnit pomocí příkazu ALTER DATABASE.</span><span class="sxs-lookup"><span data-stu-id="b94ae-116">Database flag **is_temporal_history_retention_enabled** is set to ON by default, but users can change it with ALTER DATABASE statement.</span></span> <span data-ttu-id="b94ae-117">Je také automaticky nastaví na hodnotu OFF po [obnovení bodu v čase](sql-database-recovery-using-backups.md) operaci.</span><span class="sxs-lookup"><span data-stu-id="b94ae-117">It is also automatically set to OFF after [point in time restore](sql-database-recovery-using-backups.md) operation.</span></span> <span data-ttu-id="b94ae-118">Pokud chcete povolit dočasné uchování vyčištění pro vaši databázi, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b94ae-118">To enable temporal history retention cleanup for your database, execute the following statement:</span></span>

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

> [!IMPORTANT]
> <span data-ttu-id="b94ae-119">Uchování pro dočasné tabulky, i když můžete nakonfigurovat **is_temporal_history_retention_enabled** je VYPNUTÝ, ale není v takovém případě aktivuje automatické čištění zastaralá řádků.</span><span class="sxs-lookup"><span data-stu-id="b94ae-119">You can configure retention for temporal tables even if **is_temporal_history_retention_enabled** is OFF, but automatic cleanup for aged rows is not triggered in that case.</span></span>
> 
> 

<span data-ttu-id="b94ae-120">Zásady uchovávání informací je nakonfigurované během vytváření tabulky zadáním hodnoty pro parametr HISTORY_RETENTION_PERIOD:</span><span class="sxs-lookup"><span data-stu-id="b94ae-120">Retention policy is configured during table creation by specifying value for the HISTORY_RETENTION_PERIOD parameter:</span></span>

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

<span data-ttu-id="b94ae-121">Databáze SQL Azure umožňuje určit dobu uchování pomocí různých časových jednotek: počet dnů, týdnů, měsíců a let.</span><span class="sxs-lookup"><span data-stu-id="b94ae-121">Azure SQL Database allows you to specify retention period by using different time units: DAYS, WEEKS, MONTHS, and YEARS.</span></span> <span data-ttu-id="b94ae-122">Pokud je vynechán HISTORY_RETENTION_PERIOD, se předpokládá NEKONEČNÉ uchování.</span><span class="sxs-lookup"><span data-stu-id="b94ae-122">If HISTORY_RETENTION_PERIOD is omitted, INFINITE retention is assumed.</span></span> <span data-ttu-id="b94ae-123">Můžete taky NEKONEČNÉ – klíčové slovo explicitně.</span><span class="sxs-lookup"><span data-stu-id="b94ae-123">You can also use INFINITE keyword explicitly.</span></span>

<span data-ttu-id="b94ae-124">V některých případech můžete chtít nakonfigurovat uchování po vytvoření tabulky nebo ke změně nakonfigurovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="b94ae-124">In some scenarios, you may want to configure retention after table creation, or to change previously configured value.</span></span> <span data-ttu-id="b94ae-125">V takovém případě použijte příkaz ALTER TABLE:</span><span class="sxs-lookup"><span data-stu-id="b94ae-125">In that case use ALTER TABLE statement:</span></span>

````
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
````

> [!IMPORTANT]
> <span data-ttu-id="b94ae-126">Nastavení možnosti SYSTEM_VERSIONING na hodnotu OFF *nezachovává* hodnotu doby uchování.</span><span class="sxs-lookup"><span data-stu-id="b94ae-126">Setting SYSTEM_VERSIONING to OFF *does not preserve* retention period value.</span></span> <span data-ttu-id="b94ae-127">Nastavení možnosti SYSTEM_VERSIONING na ON bez HISTORY_RETENTION_PERIOD zadaného explicitně výsledky v NEKONEČNOU dobu uchování.</span><span class="sxs-lookup"><span data-stu-id="b94ae-127">Setting SYSTEM_VERSIONING to ON without HISTORY_RETENTION_PERIOD specified explicitly results in the INFINITE retention period.</span></span>
> 
> 

<span data-ttu-id="b94ae-128">Pokud chcete zkontrolovat aktuální stav zásady uchovávání informací, použijte následující dotaz, který spojuje příznak povolení dočasné uchovávání dat na úrovni databáze s dob uchování u jednotlivých tabulek:</span><span class="sxs-lookup"><span data-stu-id="b94ae-128">To review current state of the retention policy, use the following query that joins temporal retention enablement flag at the database level with retention periods for individual tables:</span></span>

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


## <a name="how-sql-database-deletes-aged-rows"></a><span data-ttu-id="b94ae-129">Jak SQL Database odstraní stará řádky?</span><span class="sxs-lookup"><span data-stu-id="b94ae-129">How SQL Database deletes aged rows?</span></span>
<span data-ttu-id="b94ae-130">Proces vyčištění závisí na rozložení indexu tabulky historie.</span><span class="sxs-lookup"><span data-stu-id="b94ae-130">The cleanup process depends on the index layout of the history table.</span></span> <span data-ttu-id="b94ae-131">Je důležité si všimněte si, že *pouze tabulky historie clusterovaný index (B-stromu nebo columnstore) může mít omezený uchování zásady nakonfigurované*.</span><span class="sxs-lookup"><span data-stu-id="b94ae-131">It is important to notice that *only history tables with a clustered index (B-tree or columnstore) can have finite retention policy configured*.</span></span> <span data-ttu-id="b94ae-132">Úlohy na pozadí se vytvoří pro Vyčistit stará data pro všechny dočasné tabulky s dobou uchování omezené.</span><span class="sxs-lookup"><span data-stu-id="b94ae-132">A background task is created to perform aged data cleanup for all temporal tables with finite retention period.</span></span>
<span data-ttu-id="b94ae-133">Vyčištění logiku pro clusterovaný index rowstore (B-stromu) odstraňuje zastaralá řádek v menší bloky dat (až 10 kB) minimalizovat tlak na protokol databáze a vstupně-výstupních operací subsystému.</span><span class="sxs-lookup"><span data-stu-id="b94ae-133">Cleanup logic for the rowstore (B-tree) clustered index deletes aged row in smaller chunks (up to 10K) minimizing pressure on database log and I/O subsystem.</span></span> <span data-ttu-id="b94ae-134">I když čištění logiku využívá požadovaným indexem B-stromu, pořadí odstranění starší než doba uchování dat se nedá zaručit pevně řádků.</span><span class="sxs-lookup"><span data-stu-id="b94ae-134">Although cleanup logic utilizes required B-tree index, order of deletions for the rows older than retention period cannot be firmly guaranteed.</span></span> <span data-ttu-id="b94ae-135">Proto *nepřebírají žádné závislostí v pořadí čištění ve svých aplikacích*.</span><span class="sxs-lookup"><span data-stu-id="b94ae-135">Hence, *do not take any dependency on the cleanup order in your applications*.</span></span>

<span data-ttu-id="b94ae-136">Úloha vyčištění pro Clusterované columnstore odebere celý [řádek skupiny](https://msdn.microsoft.com/library/gg492088.aspx) najednou (obvykle obsahují 1 milionu řádků každý), což je velmi efektivní, zejména v případě, že historická data se generuje vysoký tempem.</span><span class="sxs-lookup"><span data-stu-id="b94ae-136">The cleanup task for the clustered columnstore removes entire [row groups](https://msdn.microsoft.com/library/gg492088.aspx) at once (typically contain 1 million of rows each), which is very efficient, especially when historical data is generated at a high pace.</span></span>

![Clusterované columnstore uchování](./media/sql-database-temporal-tables-retention-policy/cciretention.png)

<span data-ttu-id="b94ae-138">Komprese dat vynikající a efektivní uchování čištění díky clusterovaný columnstore index ideální volbou pro scénáře, když vaše úlohy generují rychle vysoký objem historická data.</span><span class="sxs-lookup"><span data-stu-id="b94ae-138">Excellent data compression and efficient retention cleanup makes clustered columnstore index a perfect choice for scenarios when your workload rapidly generates high amount of historical data.</span></span> <span data-ttu-id="b94ae-139">Tento vzor je typické pro náročné na prostředky [úlohy zpracování transakcí, které používají dočasné tabulky](https://msdn.microsoft.com/library/mt631669.aspx) pro sledování změn a auditování, analýza trendu nebo přijímání dat IoT.</span><span class="sxs-lookup"><span data-stu-id="b94ae-139">That pattern is typical for intensive [transactional processing workloads that use temporal tables](https://msdn.microsoft.com/library/mt631669.aspx) for change tracking and auditing, trend analysis, or IoT data ingestion.</span></span>

## <a name="index-considerations"></a><span data-ttu-id="b94ae-140">Aspekty indexu</span><span class="sxs-lookup"><span data-stu-id="b94ae-140">Index considerations</span></span>
<span data-ttu-id="b94ae-141">Úloha vyčištění tabulky s clusterovaný index rowstore vyžaduje index začínat sloupec odpovídající konci období SYSTEM_TIME.</span><span class="sxs-lookup"><span data-stu-id="b94ae-141">The cleanup task for tables with rowstore clustered index requires index to start with the column corresponding the end of SYSTEM_TIME period.</span></span> <span data-ttu-id="b94ae-142">Pokud takový indexu neexistuje, nelze nakonfigurovat doby uchování konečné:</span><span class="sxs-lookup"><span data-stu-id="b94ae-142">If such index doesn't exist, you cannot configure a finite retention period:</span></span>

<span data-ttu-id="b94ae-143">*Msg 13765, Level 16, State 1 <br> </br> nastavení konečné uchovávají se na dočasné tabulce se systémovou správou verzí temporalstagetestdb.dbo.WebsiteUserInfo, protože tabulka historie. temporalstagetestdb.dbo.WebsiteUserInfoHistory' neobsahuje požadované clusterovaný index. Zvažte vytvoření clusteru columnstore nebo B-stromu index začínající sloupec, který odpovídá konec SYSTEM_TIME period v tabulce historie.*</span><span class="sxs-lookup"><span data-stu-id="b94ae-143">*Msg 13765, Level 16, State 1 <br></br> Setting finite retention period failed on system-versioned temporal table 'temporalstagetestdb.dbo.WebsiteUserInfo' because the history table 'temporalstagetestdb.dbo.WebsiteUserInfoHistory' does not contain required clustered index. Consider creating a clustered columnstore or B-tree index starting with the column that matches end of SYSTEM_TIME period, on the history table.*</span></span>

<span data-ttu-id="b94ae-144">Je důležité si všimněte si, že v tabulce historie výchozí vytvořené databáze SQL Azure již má clusterovaný index, který je kompatibilní s pro zásady uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="b94ae-144">It is important to notice that the default history table created by Azure SQL Database already has clustered index, which is compliant for retention policy.</span></span> <span data-ttu-id="b94ae-145">Pokud se pokusíte odebrat tohoto indexu v tabulce s dobou uchování omezené, operace selže s následující chybou:</span><span class="sxs-lookup"><span data-stu-id="b94ae-145">If you try to remove that index on a table with finite retention period, operation fails with the following error:</span></span>

<span data-ttu-id="b94ae-146">*Msg 13766, Level 16, State 1 <br> </br> nelze vyřadit clusterovaný index 'WebsiteUserInfoHistory.IX_WebsiteUserInfoHistory', protože je používán pro automatické čištění zastaralá data. Zvažte nastavení HISTORY_RETENTION_PERIOD k NEKONEČNÉ na odpovídající systémovou správou verzí dočasnou tabulku, pokud je třeba vyřadit tohoto indexu.*</span><span class="sxs-lookup"><span data-stu-id="b94ae-146">*Msg 13766, Level 16, State 1 <br></br> Cannot drop the clustered index 'WebsiteUserInfoHistory.IX_WebsiteUserInfoHistory' because it is being used for automatic cleanup of aged data. Consider setting HISTORY_RETENTION_PERIOD to INFINITE on the corresponding system-versioned temporal table if you need to drop this index.*</span></span>

<span data-ttu-id="b94ae-147">Čištění na clusterovaný index columnstore funguje optimálně vložit historických řádků ve vzestupném pořadí (na konci období sloupec), tomu je vždy, když v tabulce historie je naplněn výhradně SYSTEM_VERSIONIOING mechanismus.</span><span class="sxs-lookup"><span data-stu-id="b94ae-147">Cleanup on the clustered columnstore index works optimally if historical rows are inserted in the ascending order (ordered by the end of period column), which is always the case when the history table is populated exclusively by the SYSTEM_VERSIONIOING mechanism.</span></span> <span data-ttu-id="b94ae-148">Pokud nejsou řádky v tabulce historie seřazené podle konec sloupec období (který může být případ, pokud jste migrovali existující historická data), je třeba znovu vytvořit clusterovaný index columnstore nad B-stromu vytvořit index, který je správně seřadit, abyste dosáhli optimálního výkon.</span><span class="sxs-lookup"><span data-stu-id="b94ae-148">If rows in the history table are not ordered by end of period column (which may be the case if you migrated existing historical data), you should re-create clustered columnstore index on top of B-tree rowstore index that is properly ordered, to achieve optimal performance.</span></span>

<span data-ttu-id="b94ae-149">Vyhněte se znovu sestavit clusterovaný index columnstore v tabulce historie s dobou uchování omezené, protože může změnit, řazení do skupin řádků přirozeně vyvolané operace systémové správy verzí.</span><span class="sxs-lookup"><span data-stu-id="b94ae-149">Avoid rebuilding clustered columnstore index on the history table with the finite retention period, because it may change ordering in the row groups naturally imposed by the system-versioning operation.</span></span> <span data-ttu-id="b94ae-150">Pokud potřebujete znovu sestavit clusterovaný index columnstore v tabulce historie, učiňte tak, že ho znovu vytvoříte nad kompatibilní index B-stromu, zachování pořadí v rowgroups potřebné pro čištění regulární dat.</span><span class="sxs-lookup"><span data-stu-id="b94ae-150">If you need to rebuild clustered columnstore index on the history table, do that by re-creating it on top of compliant B-tree index, preserving ordering in the rowgroups necessary for regular data cleanup.</span></span> <span data-ttu-id="b94ae-151">Pokud vytvoříte dočasnou tabulku s stávající tabulka historie, který obsahuje clusterovaný index sloupce bez pořadí zaručenou dat by měla být provedena ve stejný přístup:</span><span class="sxs-lookup"><span data-stu-id="b94ae-151">The same approach should be taken if you create temporal table with existing history table that has clustered column index without guaranteed data order:</span></span>

````
/*Create B-tree ordered by the end of period column*/
CREATE CLUSTERED INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory (ValidTo)
WITH (DROP_EXISTING = ON);
GO
/*Re-create clustered columnstore index*/
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON);
````

<span data-ttu-id="b94ae-152">Pokud dobu uchování konečné je nakonfigurována pro tabulku historie s clusterovaný index columnstore, nelze vytvořit další neclusterovaných indexů B-stromu v této tabulce:</span><span class="sxs-lookup"><span data-stu-id="b94ae-152">When finite retention period is configured for the history table with the clustered columnstore index, you cannot create additional non-clustered B-tree indexes on that table:</span></span>

````
CREATE NONCLUSTERED INDEX IX_WebHistNCI ON WebsiteUserInfoHistory ([UserName])
````

<span data-ttu-id="b94ae-153">Pokus o provedení výše příkaz selže s následující chybou:</span><span class="sxs-lookup"><span data-stu-id="b94ae-153">An attempt to execute above statement fails with the following error:</span></span>

<span data-ttu-id="b94ae-154">*Msg 13772, Level 16, State 1 <br> </br> nelze vytvořit neclusterovaný index na dočasnou tabulku historie 'WebsiteUserInfoHistory, protože má doba uchovávání omezené a clusterovaný index columnstore definované.*</span><span class="sxs-lookup"><span data-stu-id="b94ae-154">*Msg 13772, Level 16, State 1 <br></br> Cannot create non-clustered index on a temporal history table 'WebsiteUserInfoHistory' since it has finite retention period and clustered columnstore index defined.*</span></span>

## <a name="querying-tables-with-retention-policy"></a><span data-ttu-id="b94ae-155">Dotazy na tabulky s zásady uchovávání informací</span><span class="sxs-lookup"><span data-stu-id="b94ae-155">Querying tables with retention policy</span></span>
<span data-ttu-id="b94ae-156">Všechny dotazy na dočasnou tabulku automaticky vyfiltrovat historických řádky odpovídající zásady uchovávání informací omezené, aby se zabránilo nepředvídatelným a konzistentní výsledky, protože zastaralá řádky může odstranit úlohu čištění *v libovolném bodě v čase a v libovolné pořadí*.</span><span class="sxs-lookup"><span data-stu-id="b94ae-156">All queries on the temporal table automatically filter out historical rows matching finite retention policy, to avoid unpredictable and inconsistent results, since aged rows can be deleted by the cleanup task, *at any point in time and in arbitrary order*.</span></span>

<span data-ttu-id="b94ae-157">Následující obrázek znázorňuje plán dotazu pro jednoduchý dotaz:</span><span class="sxs-lookup"><span data-stu-id="b94ae-157">The following picture shows the query plan for a simple query:</span></span>

````
SELECT * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME ALL;
````

<span data-ttu-id="b94ae-158">Plán dotazu obsahuje další filtr použít na konci období sloupce (ValidTo) v operátoru kontrolovat clusterovaný Index pro tabulku historie (zvýrazněné).</span><span class="sxs-lookup"><span data-stu-id="b94ae-158">The query plan includes additional filter applied to end of period column (ValidTo) in the Clustered Index Scan operator on the history table (highlighted).</span></span> <span data-ttu-id="b94ae-159">Tento příklad předpokládá u tabulky WebsiteUserInfo se nastavil této doby uchování jeden měsíc.</span><span class="sxs-lookup"><span data-stu-id="b94ae-159">This example assumes that one MONTH retention period was set on WebsiteUserInfo table.</span></span>

![Filtr dotazu uchování](./media/sql-database-temporal-tables-retention-policy/queryexecplanwithretention.png)

<span data-ttu-id="b94ae-161">Pokud dotaz Tabulka historie přímo, však může zobrazit řádky, které jsou starší než uchovávání období, ale bez jakékoli záruky na výsledky dotazu opakovatelných.</span><span class="sxs-lookup"><span data-stu-id="b94ae-161">However, if you query history table directly, you may see rows that are older than specified retention period, but without any guarantee for repeatable query results.</span></span> <span data-ttu-id="b94ae-162">Následující obrázek znázorňuje provedení plán dotazu pro dotaz v tabulce historie bez další filtry použít:</span><span class="sxs-lookup"><span data-stu-id="b94ae-162">The following picture shows query execution plan for the query on the history table without additional filters applied:</span></span>

![Dotazování historie bez uchování filtru](./media/sql-database-temporal-tables-retention-policy/queryexecplanhistorytable.png)

<span data-ttu-id="b94ae-164">Nespoléhejte obchodní logiky na čtení tabulky historie mimo dobu uchování, jak můžete obdržet nekonzistentní nebo neočekávané výsledky.</span><span class="sxs-lookup"><span data-stu-id="b94ae-164">Do not rely your business logic on reading history table beyond retention period as you may get inconsistent or unexpected results.</span></span> <span data-ttu-id="b94ae-165">Doporučujeme použít dočasné dotazy s klauzulí FOR SYSTEM_TIME pro analýzu dat v dočasných tabulek.</span><span class="sxs-lookup"><span data-stu-id="b94ae-165">We recommend that you use temporal queries with FOR SYSTEM_TIME clause for analyzing data in temporal tables.</span></span>

## <a name="point-in-time-restore-considerations"></a><span data-ttu-id="b94ae-166">Přejděte v aspekty čas obnovení</span><span class="sxs-lookup"><span data-stu-id="b94ae-166">Point in time restore considerations</span></span>
<span data-ttu-id="b94ae-167">Když vytvoříte novou databázi pomocí [obnovení existující databázi k určitému bodu v čase](sql-database-recovery-using-backups.md), má dočasné uchování zakázáno na úrovni databáze.</span><span class="sxs-lookup"><span data-stu-id="b94ae-167">When you create new database by [restoring existing database to a specific point in time](sql-database-recovery-using-backups.md), it has temporal retention disabled at the database level.</span></span> <span data-ttu-id="b94ae-168">(**is_temporal_history_retention_enabled** příznak nastaven na hodnotu OFF).</span><span class="sxs-lookup"><span data-stu-id="b94ae-168">(**is_temporal_history_retention_enabled** flag set to OFF).</span></span> <span data-ttu-id="b94ae-169">Tato funkce umožňuje zkontrolovat všechny řádky historických při obnovení, aniž byste museli, že jsou zastaralá řádky odebrat předtím, než dojde k dotazování je.</span><span class="sxs-lookup"><span data-stu-id="b94ae-169">This functionality allows you to examine all historical rows upon restore, without worrying that aged rows are removed before you get to query them.</span></span> <span data-ttu-id="b94ae-170">Můžete jej do *zkontrolovat historická data i po uplynutí doby uchování nakonfigurované*.</span><span class="sxs-lookup"><span data-stu-id="b94ae-170">You can use it to *inspect historical data beyond configured retention period*.</span></span>

<span data-ttu-id="b94ae-171">Řekněme, že dočasná tabulka má období uchování jeden měsíc.</span><span class="sxs-lookup"><span data-stu-id="b94ae-171">Say that a temporal table has one MONTH retention period specified.</span></span> <span data-ttu-id="b94ae-172">Pokud vaše databáze byla vytvořena v rámci úrovně služby Premium, bude moci vytvořit kopii databáze se stav databáze až 35 dnů zpět v minulosti.</span><span class="sxs-lookup"><span data-stu-id="b94ae-172">If your database was created in Premium Service tier, you would be able to create database copy with the database state up to 35 days back in the past.</span></span> <span data-ttu-id="b94ae-173">Které efektivně by umožňují analýza historických řádků, které jsou až 65 dny v tabulce historie přímým dotazováním.</span><span class="sxs-lookup"><span data-stu-id="b94ae-173">That effectively would allow you to analyze historical rows that are up to 65 days old by querying the history table directly.</span></span>

<span data-ttu-id="b94ae-174">Pokud chcete aktivovat čištění dočasné uchování, spusťte následující příkaz Transact-SQL za bod v době obnovení:</span><span class="sxs-lookup"><span data-stu-id="b94ae-174">If you want to activate temporal retention cleanup, run the following Transact-SQL statement after point in time restore:</span></span>

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

## <a name="next-steps"></a><span data-ttu-id="b94ae-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b94ae-175">Next steps</span></span>
<span data-ttu-id="b94ae-176">Další informace o použití dočasných tabulek v aplikacích, projděte si [Začínáme s dočasné tabulky v databázi SQL Azure](sql-database-temporal-tables.md).</span><span class="sxs-lookup"><span data-stu-id="b94ae-176">To learn how to use Temporal Tables in your applications, check out [Getting Started with Temporal Tables in Azure SQL Database](sql-database-temporal-tables.md).</span></span>

<span data-ttu-id="b94ae-177">Navštivte Channel 9 a poslechnout [scénáře úspěchu dočasné implementace skutečné zákazníka](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) a sledujte [live dočasné ukázkový](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).</span><span class="sxs-lookup"><span data-stu-id="b94ae-177">Visit Channel 9 to hear a [real customer temporal implementation success story](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) and watch a [live temporal demonstration](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).</span></span>

<span data-ttu-id="b94ae-178">Podrobné informace o dočasné tabulky, zkontrolujte [dokumentace MSDN](https://msdn.microsoft.com/library/dn935015.aspx).</span><span class="sxs-lookup"><span data-stu-id="b94ae-178">For detailed information about Temporal Tables, review [MSDN documentation](https://msdn.microsoft.com/library/dn935015.aspx).</span></span>

