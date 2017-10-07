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
# <a name="monitor-your-workload-using-dmvs"></a>Monitorování vaší úlohy pomocí DMV
Tento článek popisuje, jak toouse zobrazení dynamické správy (zobrazení dynamické správy) toomonitor vaše úlohy a prozkoumejte provádění dotazů v Azure SQL Data Warehouse.

## <a name="permissions"></a>Oprávnění
tooquery hello zobrazení dynamické správy v tomto článku, potřebujete oprávnění stav zobrazení databáze nebo ovládací PRVEK. Stav databáze poskytující zobrazení obvykle je hello preferované oprávnění, protože to je mnohem víc omezující.

```sql
GRANT VIEW DATABASE STATE toomyuser;
```

## <a name="monitor-connections"></a>Monitorování připojení
Všechny přihlášení tooSQL datového skladu se protokolují příliš[sys.dm_pdw_exec_sessions][sys.dm_pdw_exec_sessions].  Tato DMV obsahuje hello posledních 10 000 přihlášení.  Hello session_id je hello primární klíč a je přiřazen postupně pro každou novou přihlášení.

```sql
-- Other Active Connections
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
```

## <a name="monitor-query-execution"></a>Při provádění dotazu monitorování
Všechny dotazy spouštěné v SQL Data Warehouse se protokolují příliš[sys.dm_pdw_exec_requests][sys.dm_pdw_exec_requests].  Tato DMV obsahuje hello posledních 10 000 dotazy spouštěné.  Hello request_id jednoznačně identifikuje každý dotaz a je hello primární klíč pro tento DMV.  Hello request_id postupně přiřazen při každém novém dotazu a je s předponou QID, který zastupuje ID dotazu.  Dotaz na tento DMV pro danou session_id uvedeny všechny dotazy pro danou přihlášení.

> [!NOTE]
> Uložené procedury použít víc ID požadavku.  ID požadavku přiřazené postupně. 
> 
> 

Tady jsou kroky toofollow tooinvestigate dotazu provádění plány a časy pro konkrétní dotaz.

### <a name="step-1-identify-hello-query-you-wish-tooinvestigate"></a>Krok 1: Identifikace hello dotaz, zda že chcete tooinvestigate
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

Z předchozích výsledky dotazu hello **Poznámka hello ID požadavku** hello dotazu, které chcete tooinvestigate.

Dotazy v hello **pozastaveno** stavu jsou zařazena do fronty z důvodu omezení tooconcurrency. Tyto dotazy se zobrazí také v hello sys.dm_pdw_waits počká dotazu s typem UserConcurrencyResourceType. V tématu [souběžnosti a úlohy správy] [ Concurrency and workload management] další podrobnosti o souběžnosti omezení. Dotazy můžete taky počkat z jiných důvodů, jako pro zámek objektu.  Pokud váš dotaz je čeká na prostředek, přečtěte si téma [příčin dotazy čekání na prostředky] [ Investigating queries waiting for resources] další dolů v tomto článku.

vyhledávání hello toosimplify dotazu v tabulce sys.dm_pdw_exec_requests hello, použijte [popisek] [ LABEL] tooassign dotaz tooyour komentář, který lze vyhledávat v zobrazení sys.dm_pdw_exec_requests hello.

```sql
-- Query with Label
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query')
;
```

### <a name="step-2-investigate-hello-query-plan"></a>Krok 2: Prozkoumat plán dotazu hello
Použít plán hello ID požadavku tooretrieve hello dotazů na distribuovaných SQL (DSQL) z [sys.dm_pdw_request_steps][sys.dm_pdw_request_steps].

```sql
-- Find hello distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

Pokud plán DSQL trvá déle, než se očekávalo, může být příčina hello komplexní plán s mnoho kroků DSQL nebo jenom jeden krok trvá příliš dlouho.  Pokud je plán hello mnoho kroků s několika operace přesunutí, zvažte optimalizaci přesun dat tooreduce k rozdělení tabulky. Hello [distribuce tabulky] [ Table distribution] článek vysvětluje, proč data musí být přesunutý toosolve dotazu a vysvětluje některé přesun dat toominimize distribuční strategie.

tooinvestigate další podrobnosti o jeden krok, hello *operation_type* sloupec hello dlouho běžící dotaz krok a Poznámka hello **krok Index**:

* Pokračujte krok 3a pro **operace SQL**: OnOperation, RemoteOperation, ReturnOperation.
* Pokračujte krok 3b pro **operace přesunu dat**: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.

### <a name="step-3a-investigate-sql-on-hello-distributed-databases"></a>KROK 3a: prozkoumat na hello distribuované databáze SQL
Použít hello ID žádosti a hello krok Index tooretrieve z podrobností o [sys.dm_pdw_sql_requests][sys.dm_pdw_sql_requests], který obsahuje informace o provádění hello dotazu kroku na všech hello distribuované databáze.

```sql
-- Find hello distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

Po spuštění dotazu krok hello [DBCC PDW_SHOWEXECUTIONPLAN] [ DBCC PDW_SHOWEXECUTIONPLAN] může být odhadované plánu systému SQL Server používané tooretrieve hello z hello mezipaměti plánu systému SQL Server pro spuštění na konkrétní krok hello distribuce.

```sql
-- Find hello SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### <a name="step-3b-investigate-data-movement-on-hello-distributed-databases"></a>Krok 3b: prozkoumat přesun dat v databázích hello distribuované
Použít hello ID žádosti a hello krok Index tooretrieve informace o krok přesun dat spuštěná v každém distribučním z [sys.dm_pdw_dms_workers][sys.dm_pdw_dms_workers].

```sql
-- Find hello information about all hello workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

* Zkontrolujte hello *total_elapsed_time* toosee sloupce, pokud konkrétní distribuční trvá výrazně delší než jiné pro přesun dat.
* Pro distribuci dlouho běžící hello, zkontrolujte hello *rows_processed* toosee sloupce, pokud hello počet řádků přesouvaných z příslušné distribuci je podstatně větší než jiné. Pokud ano, může to znamenat zkosení podkladová data.

Pokud dotaz hello běží, [DBCC PDW_SHOWEXECUTIONPLAN] [ DBCC PDW_SHOWEXECUTIONPLAN] může být odhadované plánu systému SQL Server používané tooretrieve hello z hello mezipaměti plánu systému SQL Server pro hello aktuálně spuštěna v rámci konkrétní krok SQL distribuce.

```sql
-- Find hello SQL Server estimated plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>

## <a name="monitor-waiting-queries"></a>Monitorování čekání na dotazy
Pokud zjistíte, že dotazu nepostupuje vpřed, protože se čeká na prostředek, tady je dotaz, který obsahuje všechny prostředky hello že čeká dotazu.

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

Pokud dotaz hello aktivně čeká na prostředky z jiného dotazu, pak bude mít stav hello **AcquireResources**.  Pokud dotaz hello má všechny hello požadované prostředky, pak bude mít stav hello **udělit**.

## <a name="monitor-tempdb"></a>Databáze tempdb monitorování
Databáze tempdb vysoké využití může být hello příčiny nízký výkon a mimo problémy s pamětí. Zkontrolujte nejprve Pokud máte rowgroups kvality zkosení nebo nízký dat a proveďte příslušné akce hello. Vezměte v úvahu škálování datového skladu, pokud zjistíte, tempdb dosažení jeho omezení během provádění dotazu. Hello následující text popisuje, jak tooidentify využití databáze tempdb na jeden dotaz na každém uzlu. 

Vytvořte následující zobrazení tooassociate hello příslušný uzel id pro sys.dm_pdw_sql_requests hello. To povolí tooleverage můžete další průchozí zobrazení dynamické správy a připojte tyto tabulky s sys.dm_pdw_sql_requests.

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
Spusťte následující dotaz toomonitor tempdb hello:

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
## <a name="monitor-memory"></a>Sledování paměti

Paměť může být hello příčiny nízký výkon a mimo problémy s pamětí. Zkontrolujte nejprve Pokud máte rowgroups kvality zkosení nebo nízký dat a proveďte příslušné akce hello. Zvažte, pokud zjistíte, využití paměti systému SQL Server během provádění dotazu dosažení jeho omezení škálování datového skladu.

Hello následující dotaz vrátí přetížení využití a paměti paměti systému SQL Server na každém uzlu: 
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
## <a name="monitor-transaction-log-size"></a>Velikost protokolu transakcí monitorování
Hello následující dotaz vrátí velikost protokolu transakcí hello v každém distribučním. Zkontrolujte, pokud máte rowgroups kvality zkosení nebo nízký dat a proveďte příslušné akce hello. Pokud některý ze souborů protokolu hello dosahuje 160GB, měli byste zvážit vertikálním navýšení kapacity instanci nebo omezíte velikost vašeho transakce. 
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
## <a name="monitor-transaction-log-rollback"></a>Monitorování odvolání transakce protokolu
Pokud se nedaří své dotazy nebo trvá dlouho tooproceed, můžete zkontrolovat a monitorování, pokud máte jakékoli transakce vrácení zpět.
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

## <a name="next-steps"></a>Další kroky
V tématu [zobrazení systému] [ System views] Další informace o zobrazení dynamické správy.
V tématu [osvědčené postupy pro SQL Data Warehouse] [ SQL Data Warehouse best practices] Další informace o osvědčených postupů

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
