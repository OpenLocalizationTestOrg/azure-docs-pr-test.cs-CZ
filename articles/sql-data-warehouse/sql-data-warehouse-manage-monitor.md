---
title: "aaaMonitor úlohy pomocí zobrazení dynamické správy | Microsoft Docs"
description: "Zjistěte, jak toomonitor úlohy pomocí zobrazení dynamické správy."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 69ecd479-0941-48df-b3d0-cf54c79e6549
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 10/31/2016
ms.author: joeyong;barbkess
ms.openlocfilehash: acccf952d165ccec3de3b4b1c633b18bbbf78077
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-your-workload-using-dmvs"></a><span data-ttu-id="5d803-103">Monitorování vaší úlohy pomocí DMV</span><span class="sxs-lookup"><span data-stu-id="5d803-103">Monitor your workload using DMVs</span></span>
<span data-ttu-id="5d803-104">Tento článek popisuje, jak toouse zobrazení dynamické správy (zobrazení dynamické správy) toomonitor vaše úlohy a prozkoumejte provádění dotazů v Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5d803-104">This article describes how toouse Dynamic Management Views (DMVs) toomonitor your workload and investigate query execution in Azure SQL Data Warehouse.</span></span>

## <a name="permissions"></a><span data-ttu-id="5d803-105">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="5d803-105">Permissions</span></span>
<span data-ttu-id="5d803-106">tooquery hello zobrazení dynamické správy v tomto článku, potřebujete oprávnění stav zobrazení databáze nebo ovládací PRVEK.</span><span class="sxs-lookup"><span data-stu-id="5d803-106">tooquery hello DMVs in this article, you need either VIEW DATABASE STATE or CONTROL permission.</span></span> <span data-ttu-id="5d803-107">Stav databáze poskytující zobrazení obvykle je hello preferované oprávnění, protože to je mnohem víc omezující.</span><span class="sxs-lookup"><span data-stu-id="5d803-107">Usually granting VIEW DATABASE STATE is hello preferred permission as it is much more restrictive.</span></span>

```sql
GRANT VIEW DATABASE STATE toomyuser;
```

## <a name="monitor-connections"></a><span data-ttu-id="5d803-108">Monitorování připojení</span><span class="sxs-lookup"><span data-stu-id="5d803-108">Monitor connections</span></span>
<span data-ttu-id="5d803-109">Všechny přihlášení tooSQL datového skladu se protokolují příliš[sys.dm_pdw_exec_sessions][sys.dm_pdw_exec_sessions].</span><span class="sxs-lookup"><span data-stu-id="5d803-109">All logins tooSQL Data Warehouse are logged too[sys.dm_pdw_exec_sessions][sys.dm_pdw_exec_sessions].</span></span>  <span data-ttu-id="5d803-110">Tato DMV obsahuje hello posledních 10 000 přihlášení.</span><span class="sxs-lookup"><span data-stu-id="5d803-110">This DMV contains hello last 10,000 logins.</span></span>  <span data-ttu-id="5d803-111">Hello session_id je hello primární klíč a je přiřazen postupně pro každou novou přihlášení.</span><span class="sxs-lookup"><span data-stu-id="5d803-111">hello session_id is hello primary key and is assigned sequentially for each new logon.</span></span>

```sql
-- Other Active Connections
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
```

## <a name="monitor-query-execution"></a><span data-ttu-id="5d803-112">Při provádění dotazu monitorování</span><span class="sxs-lookup"><span data-stu-id="5d803-112">Monitor query execution</span></span>
<span data-ttu-id="5d803-113">Všechny dotazy spouštěné v SQL Data Warehouse se protokolují příliš[sys.dm_pdw_exec_requests][sys.dm_pdw_exec_requests].</span><span class="sxs-lookup"><span data-stu-id="5d803-113">All queries executed on SQL Data Warehouse are logged too[sys.dm_pdw_exec_requests][sys.dm_pdw_exec_requests].</span></span>  <span data-ttu-id="5d803-114">Tato DMV obsahuje hello posledních 10 000 dotazy spouštěné.</span><span class="sxs-lookup"><span data-stu-id="5d803-114">This DMV contains hello last 10,000 queries executed.</span></span>  <span data-ttu-id="5d803-115">Hello request_id jednoznačně identifikuje každý dotaz a je hello primární klíč pro tento DMV.</span><span class="sxs-lookup"><span data-stu-id="5d803-115">hello request_id uniquely identifies each query and is hello primary key for this DMV.</span></span>  <span data-ttu-id="5d803-116">Hello request_id postupně přiřazen při každém novém dotazu a je s předponou QID, který zastupuje ID dotazu.</span><span class="sxs-lookup"><span data-stu-id="5d803-116">hello request_id is assigned sequentially for each new query and is prefixed with QID, which stands for query ID.</span></span>  <span data-ttu-id="5d803-117">Dotaz na tento DMV pro danou session_id uvedeny všechny dotazy pro danou přihlášení.</span><span class="sxs-lookup"><span data-stu-id="5d803-117">Querying this DMV for a given session_id shows all queries for a given logon.</span></span>

> [!NOTE]
> <span data-ttu-id="5d803-118">Uložené procedury použít víc ID požadavku.</span><span class="sxs-lookup"><span data-stu-id="5d803-118">Stored procedures use multiple Request IDs.</span></span>  <span data-ttu-id="5d803-119">ID požadavku přiřazené postupně.</span><span class="sxs-lookup"><span data-stu-id="5d803-119">Request IDs are assigned in sequential order.</span></span> 
> 
> 

<span data-ttu-id="5d803-120">Tady jsou kroky toofollow tooinvestigate dotazu provádění plány a časy pro konkrétní dotaz.</span><span class="sxs-lookup"><span data-stu-id="5d803-120">Here are steps toofollow tooinvestigate query execution plans and times for a particular query.</span></span>

### <a name="step-1-identify-hello-query-you-wish-tooinvestigate"></a><span data-ttu-id="5d803-121">Krok 1: Identifikace hello dotaz, zda že chcete tooinvestigate</span><span class="sxs-lookup"><span data-stu-id="5d803-121">STEP 1: Identify hello query you wish tooinvestigate</span></span>
```sql
-- Monitor active queries
SELECT * 
FROM sys.dm_pdw_exec_requests 
WHERE status not in ('Completed','Failed','Cancelled')
  AND session_id <> session_id()
ORDER BY submit_time DESC;

-- Find top 10 queries longest running queries
SELECT TOP 10 * 
FROM sys.dm_pdw_exec_requests 
ORDER BY total_elapsed_time DESC;

-- Find a query with hello Label 'My Query'
-- Use brackets when querying hello label column, as it it a key word
SELECT  *
FROM    sys.dm_pdw_exec_requests
WHERE   [label] = 'My Query';
```

<span data-ttu-id="5d803-122">Z předchozích výsledky dotazu hello **Poznámka hello ID požadavku** hello dotazu, které chcete tooinvestigate.</span><span class="sxs-lookup"><span data-stu-id="5d803-122">From hello preceding query results, **note hello Request ID** of hello query that you would like tooinvestigate.</span></span>

<span data-ttu-id="5d803-123">Dotazy v hello **pozastaveno** stavu jsou zařazena do fronty z důvodu omezení tooconcurrency.</span><span class="sxs-lookup"><span data-stu-id="5d803-123">Queries in hello **Suspended** state are being queued due tooconcurrency limits.</span></span> <span data-ttu-id="5d803-124">Tyto dotazy se zobrazí také v hello sys.dm_pdw_waits počká dotazu s typem UserConcurrencyResourceType.</span><span class="sxs-lookup"><span data-stu-id="5d803-124">These queries also appear in hello sys.dm_pdw_waits waits query with a type of UserConcurrencyResourceType.</span></span> <span data-ttu-id="5d803-125">V tématu [souběžnosti a úlohy správy] [ Concurrency and workload management] další podrobnosti o souběžnosti omezení.</span><span class="sxs-lookup"><span data-stu-id="5d803-125">See [Concurrency and workload management][Concurrency and workload management] for more details on concurrency limits.</span></span> <span data-ttu-id="5d803-126">Dotazy můžete taky počkat z jiných důvodů, jako pro zámek objektu.</span><span class="sxs-lookup"><span data-stu-id="5d803-126">Queries can also wait for other reasons such as for object locks.</span></span>  <span data-ttu-id="5d803-127">Pokud váš dotaz je čeká na prostředek, přečtěte si téma [příčin dotazy čekání na prostředky] [ Investigating queries waiting for resources] další dolů v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="5d803-127">If your query is waiting for a resource, see [Investigating queries waiting for resources][Investigating queries waiting for resources] further down in this article.</span></span>

<span data-ttu-id="5d803-128">vyhledávání hello toosimplify dotazu v tabulce sys.dm_pdw_exec_requests hello, použijte [popisek] [ LABEL] tooassign dotaz tooyour komentář, který lze vyhledávat v zobrazení sys.dm_pdw_exec_requests hello.</span><span class="sxs-lookup"><span data-stu-id="5d803-128">toosimplify hello lookup of a query in hello sys.dm_pdw_exec_requests table, use [LABEL][LABEL] tooassign a comment tooyour query that can be looked up in hello sys.dm_pdw_exec_requests view.</span></span>

```sql
-- Query with Label
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query')
;
```

### <a name="step-2-investigate-hello-query-plan"></a><span data-ttu-id="5d803-129">Krok 2: Prozkoumat plán dotazu hello</span><span class="sxs-lookup"><span data-stu-id="5d803-129">STEP 2: Investigate hello query plan</span></span>
<span data-ttu-id="5d803-130">Použít plán hello ID požadavku tooretrieve hello dotazů na distribuovaných SQL (DSQL) z [sys.dm_pdw_request_steps][sys.dm_pdw_request_steps].</span><span class="sxs-lookup"><span data-stu-id="5d803-130">Use hello Request ID tooretrieve hello query's distributed SQL (DSQL) plan from [sys.dm_pdw_request_steps][sys.dm_pdw_request_steps].</span></span>

```sql
-- Find hello distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

<span data-ttu-id="5d803-131">Pokud plán DSQL trvá déle, než se očekávalo, může být příčina hello komplexní plán s mnoho kroků DSQL nebo jenom jeden krok trvá příliš dlouho.</span><span class="sxs-lookup"><span data-stu-id="5d803-131">When a DSQL plan is taking longer than expected, hello cause can be a complex plan with many DSQL steps or just one step taking a long time.</span></span>  <span data-ttu-id="5d803-132">Pokud je plán hello mnoho kroků s několika operace přesunutí, zvažte optimalizaci přesun dat tooreduce k rozdělení tabulky.</span><span class="sxs-lookup"><span data-stu-id="5d803-132">If hello plan is many steps with several move operations, consider optimizing your table distributions tooreduce data movement.</span></span> <span data-ttu-id="5d803-133">Hello [distribuce tabulky] [ Table distribution] článek vysvětluje, proč data musí být přesunutý toosolve dotazu a vysvětluje některé přesun dat toominimize distribuční strategie.</span><span class="sxs-lookup"><span data-stu-id="5d803-133">hello [Table distribution][Table distribution] article explains why data must be moved toosolve a query and explains some distribution strategies toominimize data movement.</span></span>

<span data-ttu-id="5d803-134">tooinvestigate další podrobnosti o jeden krok, hello *operation_type* sloupec hello dlouho běžící dotaz krok a Poznámka hello **krok Index**:</span><span class="sxs-lookup"><span data-stu-id="5d803-134">tooinvestigate further details about a single step, hello *operation_type* column of hello long-running query step and note hello **Step Index**:</span></span>

* <span data-ttu-id="5d803-135">Pokračujte krok 3a pro **operace SQL**: OnOperation, RemoteOperation, ReturnOperation.</span><span class="sxs-lookup"><span data-stu-id="5d803-135">Proceed with Step 3a for **SQL operations**: OnOperation, RemoteOperation, ReturnOperation.</span></span>
* <span data-ttu-id="5d803-136">Pokračujte krok 3b pro **operace přesunu dat**: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.</span><span class="sxs-lookup"><span data-stu-id="5d803-136">Proceed with Step 3b for **Data Movement operations**: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.</span></span>

### <a name="step-3a-investigate-sql-on-hello-distributed-databases"></a><span data-ttu-id="5d803-137">KROK 3a: prozkoumat na hello distribuované databáze SQL</span><span class="sxs-lookup"><span data-stu-id="5d803-137">STEP 3a: Investigate SQL on hello distributed databases</span></span>
<span data-ttu-id="5d803-138">Použít hello ID žádosti a hello krok Index tooretrieve z podrobností o [sys.dm_pdw_sql_requests][sys.dm_pdw_sql_requests], který obsahuje informace o provádění hello dotazu kroku na všech hello distribuované databáze.</span><span class="sxs-lookup"><span data-stu-id="5d803-138">Use hello Request ID and hello Step Index tooretrieve details from [sys.dm_pdw_sql_requests][sys.dm_pdw_sql_requests], which contains execution information of hello query step on all of hello distributed databases.</span></span>

```sql
-- Find hello distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

<span data-ttu-id="5d803-139">Po spuštění dotazu krok hello [DBCC PDW_SHOWEXECUTIONPLAN] [ DBCC PDW_SHOWEXECUTIONPLAN] může být odhadované plánu systému SQL Server používané tooretrieve hello z hello mezipaměti plánu systému SQL Server pro spuštění na konkrétní krok hello distribuce.</span><span class="sxs-lookup"><span data-stu-id="5d803-139">When hello query step is running, [DBCC PDW_SHOWEXECUTIONPLAN][DBCC PDW_SHOWEXECUTIONPLAN] can be used tooretrieve hello SQL Server estimated plan from hello SQL Server plan cache for hello step running on a particular distribution.</span></span>

```sql
-- Find hello SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### <a name="step-3b-investigate-data-movement-on-hello-distributed-databases"></a><span data-ttu-id="5d803-140">Krok 3b: prozkoumat přesun dat v databázích hello distribuované</span><span class="sxs-lookup"><span data-stu-id="5d803-140">STEP 3b: Investigate data movement on hello distributed databases</span></span>
<span data-ttu-id="5d803-141">Použít hello ID žádosti a hello krok Index tooretrieve informace o krok přesun dat spuštěná v každém distribučním z [sys.dm_pdw_dms_workers][sys.dm_pdw_dms_workers].</span><span class="sxs-lookup"><span data-stu-id="5d803-141">Use hello Request ID and hello Step Index tooretrieve information about a data movement step running on each distribution from [sys.dm_pdw_dms_workers][sys.dm_pdw_dms_workers].</span></span>

```sql
-- Find hello information about all hello workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

* <span data-ttu-id="5d803-142">Zkontrolujte hello *total_elapsed_time* toosee sloupce, pokud konkrétní distribuční trvá výrazně delší než jiné pro přesun dat.</span><span class="sxs-lookup"><span data-stu-id="5d803-142">Check hello *total_elapsed_time* column toosee if a particular distribution is taking significantly longer than others for data movement.</span></span>
* <span data-ttu-id="5d803-143">Pro distribuci dlouho běžící hello, zkontrolujte hello *rows_processed* toosee sloupce, pokud hello počet řádků přesouvaných z příslušné distribuci je podstatně větší než jiné.</span><span class="sxs-lookup"><span data-stu-id="5d803-143">For hello long-running distribution, check hello *rows_processed* column toosee if hello number of rows being moved from that distribution is significantly larger than others.</span></span> <span data-ttu-id="5d803-144">Pokud ano, může to znamenat zkosení podkladová data.</span><span class="sxs-lookup"><span data-stu-id="5d803-144">If so, this may indicate skew of your underlying data.</span></span>

<span data-ttu-id="5d803-145">Pokud dotaz hello běží, [DBCC PDW_SHOWEXECUTIONPLAN] [ DBCC PDW_SHOWEXECUTIONPLAN] může být odhadované plánu systému SQL Server používané tooretrieve hello z hello mezipaměti plánu systému SQL Server pro hello aktuálně spuštěna v rámci konkrétní krok SQL distribuce.</span><span class="sxs-lookup"><span data-stu-id="5d803-145">If hello query is running, [DBCC PDW_SHOWEXECUTIONPLAN][DBCC PDW_SHOWEXECUTIONPLAN] can be used tooretrieve hello SQL Server estimated plan from hello SQL Server plan cache for hello currently running SQL Step within a particular distribution.</span></span>

```sql
-- Find hello SQL Server estimated plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>

## <a name="monitor-waiting-queries"></a><span data-ttu-id="5d803-146">Monitorování čekání na dotazy</span><span class="sxs-lookup"><span data-stu-id="5d803-146">Monitor waiting queries</span></span>
<span data-ttu-id="5d803-147">Pokud zjistíte, že dotazu nepostupuje vpřed, protože se čeká na prostředek, tady je dotaz, který obsahuje všechny prostředky hello že čeká dotazu.</span><span class="sxs-lookup"><span data-stu-id="5d803-147">If you discover that your query is not making progress because it is waiting for a resource, here is a query that shows all hello resources a query is waiting for.</span></span>

```sql
-- Find queries 
-- Replace request_id with value from Step 1.

SELECT waits.session_id,
      waits.request_id,  
      requests.command,
      requests.status,
      requests.start_time,  
      waits.type,
      waits.state,
      waits.object_type,
      waits.object_name
FROM   sys.dm_pdw_waits waits
   JOIN  sys.dm_pdw_exec_requests requests
   ON waits.request_id=requests.request_id
WHERE waits.request_id = 'QID####'
ORDER BY waits.object_name, waits.object_type, waits.state;
```

<span data-ttu-id="5d803-148">Pokud dotaz hello aktivně čeká na prostředky z jiného dotazu, pak bude mít stav hello **AcquireResources**.</span><span class="sxs-lookup"><span data-stu-id="5d803-148">If hello query is actively waiting on resources from another query, then hello state will be **AcquireResources**.</span></span>  <span data-ttu-id="5d803-149">Pokud dotaz hello má všechny hello požadované prostředky, pak bude mít stav hello **udělit**.</span><span class="sxs-lookup"><span data-stu-id="5d803-149">If hello query has all hello required resources, then hello state will be **Granted**.</span></span>

## <a name="monitor-tempdb"></a><span data-ttu-id="5d803-150">Databáze tempdb monitorování</span><span class="sxs-lookup"><span data-stu-id="5d803-150">Monitor tempdb</span></span>
<span data-ttu-id="5d803-151">Databáze tempdb vysoké využití může být hello příčiny nízký výkon a mimo problémy s pamětí.</span><span class="sxs-lookup"><span data-stu-id="5d803-151">High tempdb utilization can be hello root cause for slow performance and out of memory issues.</span></span> <span data-ttu-id="5d803-152">Zkontrolujte nejprve Pokud máte rowgroups kvality zkosení nebo nízký dat a proveďte příslušné akce hello.</span><span class="sxs-lookup"><span data-stu-id="5d803-152">Please first check if you have data skew or poor quality rowgroups and take hello appropriate actions.</span></span> <span data-ttu-id="5d803-153">Vezměte v úvahu škálování datového skladu, pokud zjistíte, tempdb dosažení jeho omezení během provádění dotazu.</span><span class="sxs-lookup"><span data-stu-id="5d803-153">Consider scaling your data warehouse if you find tempdb reaching its limits during query execution.</span></span> <span data-ttu-id="5d803-154">Hello následující text popisuje, jak tooidentify využití databáze tempdb na jeden dotaz na každém uzlu.</span><span class="sxs-lookup"><span data-stu-id="5d803-154">hello following describes how tooidentify tempdb usage per query on each node.</span></span> 

<span data-ttu-id="5d803-155">Vytvořte následující zobrazení tooassociate hello příslušný uzel id pro sys.dm_pdw_sql_requests hello.</span><span class="sxs-lookup"><span data-stu-id="5d803-155">Create hello following view tooassociate hello appropriate node id for sys.dm_pdw_sql_requests.</span></span> <span data-ttu-id="5d803-156">To povolí tooleverage můžete další průchozí zobrazení dynamické správy a připojte tyto tabulky s sys.dm_pdw_sql_requests.</span><span class="sxs-lookup"><span data-stu-id="5d803-156">This will enable you tooleverage other pass-through DMVs and join those tables with sys.dm_pdw_sql_requests.</span></span>

```sql
-- sys.dm_pdw_sql_requests with hello correct node id
CREATE VIEW sql_requests AS
(SELECT
       sr.request_id,
       sr.step_index,
       (CASE 
              WHEN (sr.distribution_id = -1 ) THEN 
              (SELECT pdw_node_id FROM sys.dm_pdw_nodes WHERE type = 'CONTROL') 
              ELSE d.pdw_node_id END) AS pdw_node_id,
       sr.distribution_id,
       sr.status,
       sr.error_id,
       sr.start_time,
       sr.end_time,
       sr.total_elapsed_time,
       sr.row_count,
       sr.spid,
       sr.command
FROM sys.pdw_distributions AS d
RIGHT JOIN sys.dm_pdw_sql_requests AS sr ON d.distribution_id = sr.distribution_id)
```
<span data-ttu-id="5d803-157">Spusťte následující dotaz toomonitor tempdb hello:</span><span class="sxs-lookup"><span data-stu-id="5d803-157">Run hello following query toomonitor tempdb:</span></span>

```sql
-- Monitor tempdb
SELECT
    sr.request_id,
    ssu.session_id,
    ssu.pdw_node_id,
    sr.command,
    sr.total_elapsed_time,
    es.login_name AS 'LoginName',
    DB_NAME(ssu.database_id) AS 'DatabaseName',
    (es.memory_usage * 8) AS 'MemoryUsage (in KB)',
    (ssu.user_objects_alloc_page_count * 8) AS 'Space Allocated For User Objects (in KB)',
    (ssu.user_objects_dealloc_page_count * 8) AS 'Space Deallocated For User Objects (in KB)',
    (ssu.internal_objects_alloc_page_count * 8) AS 'Space Allocated For Internal Objects (in KB)',
    (ssu.internal_objects_dealloc_page_count * 8) AS 'Space Deallocated For Internal Objects (in KB)',
    CASE es.is_user_process
    WHEN 1 THEN 'User Session'
    WHEN 0 THEN 'System Session'
    END AS 'SessionType',
    es.row_count AS 'RowCount'
FROM sys.dm_pdw_nodes_db_session_space_usage AS ssu
    INNER JOIN sys.dm_pdw_nodes_exec_sessions AS es ON ssu.session_id = es.session_id AND ssu.pdw_node_id = es.pdw_node_id
    INNER JOIN sys.dm_pdw_nodes_exec_connections AS er ON ssu.session_id = er.session_id AND ssu.pdw_node_id = er.pdw_node_id
    INNER JOIN sql_requests AS sr ON ssu.session_id = sr.spid AND ssu.pdw_node_id = sr.pdw_node_id
WHERE DB_NAME(ssu.database_id) = 'tempdb'
    AND es.session_id <> @@SPID
    AND es.login_name <> 'sa' 
ORDER BY sr.request_id;
```
## <a name="monitor-memory"></a><span data-ttu-id="5d803-158">Sledování paměti</span><span class="sxs-lookup"><span data-stu-id="5d803-158">Monitor memory</span></span>

<span data-ttu-id="5d803-159">Paměť může být hello příčiny nízký výkon a mimo problémy s pamětí.</span><span class="sxs-lookup"><span data-stu-id="5d803-159">Memory can be hello root cause for slow performance and out of memory issues.</span></span> <span data-ttu-id="5d803-160">Zkontrolujte nejprve Pokud máte rowgroups kvality zkosení nebo nízký dat a proveďte příslušné akce hello.</span><span class="sxs-lookup"><span data-stu-id="5d803-160">Please first check if you have data skew or poor quality rowgroups and take hello appropriate actions.</span></span> <span data-ttu-id="5d803-161">Zvažte, pokud zjistíte, využití paměti systému SQL Server během provádění dotazu dosažení jeho omezení škálování datového skladu.</span><span class="sxs-lookup"><span data-stu-id="5d803-161">Consider scaling your data warehouse if you find SQL Server memory usage reaching its limits during query execution.</span></span>

<span data-ttu-id="5d803-162">Hello následující dotaz vrátí přetížení využití a paměti paměti systému SQL Server na každém uzlu:</span><span class="sxs-lookup"><span data-stu-id="5d803-162">hello following query returns SQL Server memory usage and memory pressure per node:</span></span> 
```sql
-- Memory consumption
SELECT
  pc1.cntr_value as Curr_Mem_KB, 
  pc1.cntr_value/1024.0 as Curr_Mem_MB,
  (pc1.cntr_value/1048576.0) as Curr_Mem_GB,
  pc2.cntr_value as Max_Mem_KB,
  pc2.cntr_value/1024.0 as Max_Mem_MB,
  (pc2.cntr_value/1048576.0) as Max_Mem_GB,
  pc1.cntr_value * 100.0/pc2.cntr_value AS Memory_Utilization_Percentage,
  pc1.pdw_node_id
FROM
-- pc1: current memory
sys.dm_pdw_nodes_os_performance_counters AS pc1
-- pc2: total memory allowed for this SQL instance
JOIN sys.dm_pdw_nodes_os_performance_counters AS pc2 
ON pc1.object_name = pc2.object_name AND pc1.pdw_node_id = pc2.pdw_node_id
WHERE
pc1.counter_name = 'Total Server Memory (KB)'
AND pc2.counter_name = 'Target Server Memory (KB)'
```
## <a name="monitor-transaction-log-size"></a><span data-ttu-id="5d803-163">Velikost protokolu transakcí monitorování</span><span class="sxs-lookup"><span data-stu-id="5d803-163">Monitor transaction log size</span></span>
<span data-ttu-id="5d803-164">Hello následující dotaz vrátí velikost protokolu transakcí hello v každém distribučním.</span><span class="sxs-lookup"><span data-stu-id="5d803-164">hello following query returns hello transaction log size on each distribution.</span></span> <span data-ttu-id="5d803-165">Zkontrolujte, pokud máte rowgroups kvality zkosení nebo nízký dat a proveďte příslušné akce hello.</span><span class="sxs-lookup"><span data-stu-id="5d803-165">Please check if you have data skew or poor quality rowgroups and take hello appropriate actions.</span></span> <span data-ttu-id="5d803-166">Pokud některý ze souborů protokolu hello dosahuje 160GB, měli byste zvážit vertikálním navýšení kapacity instanci nebo omezíte velikost vašeho transakce.</span><span class="sxs-lookup"><span data-stu-id="5d803-166">If one of hello log files is reaching 160GB, you should consider scaling up your instance or limiting your transaction size.</span></span> 
```sql
-- Transaction log size
SELECT
  instance_name as distribution_db,
  cntr_value*1.0/1048576 as log_file_size_used_GB,
  pdw_node_id 
FROM sys.dm_pdw_nodes_os_performance_counters 
WHERE 
instance_name like 'Distribution_%' 
AND counter_name = 'Log File(s) Used Size (KB)'
AND counter_name = 'Target Server Memory (KB)'
```
## <a name="monitor-transaction-log-rollback"></a><span data-ttu-id="5d803-167">Monitorování odvolání transakce protokolu</span><span class="sxs-lookup"><span data-stu-id="5d803-167">Monitor transaction log rollback</span></span>
<span data-ttu-id="5d803-168">Pokud se nedaří své dotazy nebo trvá dlouho tooproceed, můžete zkontrolovat a monitorování, pokud máte jakékoli transakce vrácení zpět.</span><span class="sxs-lookup"><span data-stu-id="5d803-168">If your queries are failing or taking a long time tooproceed, you can check and monitor if you have any transactions rolling back.</span></span>
```sql
-- Monitor rollback
SELECT 
    SUM(CASE WHEN t.database_transaction_next_undo_lsn IS NOT NULL THEN 1 ELSE 0 END),
    t.pdw_node_id,
    nod.[type]
FROM sys.dm_pdw_nodes_tran_database_transactions t
JOIN sys.dm_pdw_nodes nod ON t.pdw_node_id = nod.pdw_node_id
GROUP BY t.pdw_node_id, nod.[type]
```

## <a name="next-steps"></a><span data-ttu-id="5d803-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5d803-169">Next steps</span></span>
<span data-ttu-id="5d803-170">V tématu [zobrazení systému] [ System views] Další informace o zobrazení dynamické správy.</span><span class="sxs-lookup"><span data-stu-id="5d803-170">See [System views][System views] for more information on DMVs.</span></span>
<span data-ttu-id="5d803-171">V tématu [osvědčené postupy pro SQL Data Warehouse] [ SQL Data Warehouse best practices] Další informace o osvědčených postupů</span><span class="sxs-lookup"><span data-stu-id="5d803-171">See [SQL Data Warehouse best practices][SQL Data Warehouse best practices] for more information about best practices</span></span>

<!--Image references-->

<!--Article references-->
[Manage overview]: ./sql-data-warehouse-overview-manage.md
[SQL Data Warehouse best practices]: ./sql-data-warehouse-best-practices.md
[System views]: ./sql-data-warehouse-reference-tsql-system-views.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Concurrency and workload management]: ./sql-data-warehouse-develop-concurrency.md
[Investigating queries waiting for resources]: ./sql-data-warehouse-manage-monitor.md#waiting

<!--MSDN references-->
[sys.dm_pdw_dms_workers]: http://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_exec_requests]: http://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_exec_sessions]: http://msdn.microsoft.com/library/mt203883.aspx
[sys.dm_pdw_request_steps]: http://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: http://msdn.microsoft.com/library/mt203889.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: http://msdn.microsoft.com/library/mt204017.aspx
[DBCC PDW_SHOWSPACEUSED]: http://msdn.microsoft.com/library/mt204028.aspx
[LABEL]: https://msdn.microsoft.com/library/ms190322.aspx
