---
title: "Monitorování databáze Azure SQL pomocí zobrazení dynamické správy | Microsoft Docs"
description: "Zjistěte, jak najít a diagnostikovat běžné problémy s výkonem pomocí zobrazení dynamické správy ke sledování Microsoft Azure SQL Database."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: d08f505f-3c62-47d4-bab7-35c9a834b79b
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: d9b007d29e06e672db71b4a8415673f258c3fd89
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="monitoring-azure-sql-database-using-dynamic-management-views"></a><span data-ttu-id="df47e-103">Monitorování databáze Azure SQL Database pomocí zobrazení dynamické správy</span><span class="sxs-lookup"><span data-stu-id="df47e-103">Monitoring Azure SQL Database using dynamic management views</span></span>
<span data-ttu-id="df47e-104">Microsoft Azure SQL Database umožňuje podmnožinu zobrazení dynamické správy diagnostikovat problémy s výkonem, které mohou být způsobeny blokované nebo dlouhodobé dotazy, kritických bodů prostředků, dotaz nízký plány a tak dále.</span><span class="sxs-lookup"><span data-stu-id="df47e-104">Microsoft Azure SQL Database enables a subset of dynamic management views to diagnose performance problems, which might be caused by blocked or long-running queries, resource bottlenecks, poor query plans, and so on.</span></span> <span data-ttu-id="df47e-105">Toto téma obsahuje informace o tom, jak zjišťují běžné problémy s výkonem pomocí zobrazení dynamické správy.</span><span class="sxs-lookup"><span data-stu-id="df47e-105">This topic provides information on how to detect common performance problems by using dynamic management views.</span></span>

<span data-ttu-id="df47e-106">Databáze SQL podporuje částečně tří kategorií zobrazení dynamické správy:</span><span class="sxs-lookup"><span data-stu-id="df47e-106">SQL Database partially supports three categories of dynamic management views:</span></span>

* <span data-ttu-id="df47e-107">Zobrazení dynamické správy vztahující se k databázi.</span><span class="sxs-lookup"><span data-stu-id="df47e-107">Database-related dynamic management views.</span></span>
* <span data-ttu-id="df47e-108">Zobrazení dynamické správy souvisejících s provádění.</span><span class="sxs-lookup"><span data-stu-id="df47e-108">Execution-related dynamic management views.</span></span>
* <span data-ttu-id="df47e-109">Zobrazení dynamické správy související transakci.</span><span class="sxs-lookup"><span data-stu-id="df47e-109">Transaction-related dynamic management views.</span></span>

<span data-ttu-id="df47e-110">Podrobné informace o zobrazení dynamické správy najdete v tématu [funkce (Transact-SQL) a zobrazení dynamické správy](https://msdn.microsoft.com/library/ms188754.aspx) v SQL Server Books Online.</span><span class="sxs-lookup"><span data-stu-id="df47e-110">For detailed information on dynamic management views, see [Dynamic Management Views and Functions (Transact-SQL)](https://msdn.microsoft.com/library/ms188754.aspx) in SQL Server Books Online.</span></span>

## <a name="permissions"></a><span data-ttu-id="df47e-111">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="df47e-111">Permissions</span></span>
<span data-ttu-id="df47e-112">V databázi SQL, dotaz na zobrazení dynamické správy vyžaduje **stav databáze zobrazení** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="df47e-112">In SQL Database, querying a dynamic management view requires **VIEW DATABASE STATE** permissions.</span></span> <span data-ttu-id="df47e-113">**Stav databáze zobrazení** oprávnění vrátí informace o všech objektech v aktuální databázi.</span><span class="sxs-lookup"><span data-stu-id="df47e-113">The **VIEW DATABASE STATE** permission returns information about all objects within the current database.</span></span>
<span data-ttu-id="df47e-114">Udělit **stav databáze zobrazení** oprávnění pro uživatele konkrétní databáze, spusťte následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="df47e-114">To grant the **VIEW DATABASE STATE** permission to a specific database user, run the following query:</span></span>

```GRANT VIEW DATABASE STATE TO database_user; ```

<span data-ttu-id="df47e-115">V instanci systému SQL Server, místní zobrazení dynamické správy vrátí informace o stavu serveru.</span><span class="sxs-lookup"><span data-stu-id="df47e-115">In an instance of on-premises SQL Server, dynamic management views return server state information.</span></span> <span data-ttu-id="df47e-116">V databázi SQL že budou vracet informace týkající se pouze v aktuální databázi logické.</span><span class="sxs-lookup"><span data-stu-id="df47e-116">In SQL Database, they return information regarding your current logical database only.</span></span>

## <a name="calculating-database-size"></a><span data-ttu-id="df47e-117">Výpočet velikosti databáze</span><span class="sxs-lookup"><span data-stu-id="df47e-117">Calculating database size</span></span>
<span data-ttu-id="df47e-118">Následující dotaz vrátí velikost databáze (v megabajtech):</span><span class="sxs-lookup"><span data-stu-id="df47e-118">The following query returns the size of your database (in megabytes):</span></span>

```
-- Calculates the size of the database.
SELECT SUM(reserved_page_count)*8.0/1024
FROM sys.dm_db_partition_stats;
GO
```

<span data-ttu-id="df47e-119">Následující dotaz vrátí velikost jednotlivých objektů (v megabajtech) v databázi:</span><span class="sxs-lookup"><span data-stu-id="df47e-119">The following query returns the size of individual objects (in megabytes) in your database:</span></span>

```
-- Calculates the size of individual database objects.
SELECT sys.objects.name, SUM(reserved_page_count) * 8.0 / 1024
FROM sys.dm_db_partition_stats, sys.objects
WHERE sys.dm_db_partition_stats.object_id = sys.objects.object_id
GROUP BY sys.objects.name;
GO
```

## <a name="monitoring-connections"></a><span data-ttu-id="df47e-120">Sledování připojení</span><span class="sxs-lookup"><span data-stu-id="df47e-120">Monitoring connections</span></span>
<span data-ttu-id="df47e-121">Můžete použít [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) zobrazení k načtení informací o připojení ke konkrétní server Azure SQL Database a podrobnosti o každé připojení.</span><span class="sxs-lookup"><span data-stu-id="df47e-121">You can use the [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) view to retrieve information about the connections established to a specific Azure SQL Database server and the details of each connection.</span></span> <span data-ttu-id="df47e-122">Kromě toho [zobrazení sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) zobrazení je vhodné při načítání informací o všechna připojení aktivního uživatele a interních úlohách.</span><span class="sxs-lookup"><span data-stu-id="df47e-122">In addition, the [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) view is helpful when retrieving information about all active user connections and internal tasks.</span></span>
<span data-ttu-id="df47e-123">Následující dotaz načte informace o aktuální připojení:</span><span class="sxs-lookup"><span data-stu-id="df47e-123">The following query retrieves information on the current connection:</span></span>

```
SELECT
    c.session_id, c.net_transport, c.encrypt_option,
    c.auth_scheme, s.host_name, s.program_name,
    s.client_interface_name, s.login_name, s.nt_domain,
    s.nt_user_name, s.original_login_name, c.connect_time,
    s.login_time
FROM sys.dm_exec_connections AS c
JOIN sys.dm_exec_sessions AS s
    ON c.session_id = s.session_id
WHERE c.session_id = @@SPID;
```

> [!NOTE]
> <span data-ttu-id="df47e-124">Při provádění **sys.dm_exec_requests** a **zobrazení zobrazení sys.dm_exec_sessions**, pokud máte **stav databáze zobrazení** oprávnění v databázi, uvidíte všechny provádění relace v databázi; jinak uvidíte pouze pro aktuální relaci.</span><span class="sxs-lookup"><span data-stu-id="df47e-124">When executing the **sys.dm_exec_requests** and **sys.dm_exec_sessions views**, if you have **VIEW DATABASE STATE** permission on the database, you see all executing sessions on the database; otherwise, you see only the current session.</span></span>
> 
> 

## <a name="monitoring-query-performance"></a><span data-ttu-id="df47e-125">Sledování výkonu dotazů</span><span class="sxs-lookup"><span data-stu-id="df47e-125">Monitoring query performance</span></span>
<span data-ttu-id="df47e-126">Pomalu nebo dlouho spuštění dotazů můžete využívat významné systémové prostředky.</span><span class="sxs-lookup"><span data-stu-id="df47e-126">Slow or long running queries can consume significant system resources.</span></span> <span data-ttu-id="df47e-127">V této části ukazuje, jak pomocí zobrazení dynamické správy lze zjistit několik běžných potíží s výkonem dotazu.</span><span class="sxs-lookup"><span data-stu-id="df47e-127">This section demonstrates how to use dynamic management views to detect a few common query performance problems.</span></span> <span data-ttu-id="df47e-128">Odkaz na starší, ale stále užitečné při řešení potíží se [řešení potíží s problémy s výkonem v systému SQL Server 2008](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) článek na Microsoft TechNetu.</span><span class="sxs-lookup"><span data-stu-id="df47e-128">An older but still helpful reference for troubleshooting, is the [Troubleshooting Performance Problems in SQL Server 2008](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) article on Microsoft TechNet.</span></span>

### <a name="finding-top-n-queries"></a><span data-ttu-id="df47e-129">Hledání hlavních dotazy</span><span class="sxs-lookup"><span data-stu-id="df47e-129">Finding top N queries</span></span>
<span data-ttu-id="df47e-130">Následující příklad vrací informace o nejvyšší pět dotazy podle Průměrná doba využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="df47e-130">The following example returns information about the top five queries ranked by average CPU time.</span></span> <span data-ttu-id="df47e-131">Tento příklad agreguje dotazy podle jejich hash dotaz tak, aby logicky ekvivalentní dotazy jsou seskupené podle jejich spotřeba kumulativní prostředků.</span><span class="sxs-lookup"><span data-stu-id="df47e-131">This example aggregates the queries according to their query hash, so that logically equivalent queries are grouped by their cumulative resource consumption.</span></span>

```
SELECT TOP 5 query_stats.query_hash AS "Query Hash",
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",
    MIN(query_stats.statement_text) AS "Statement Text"
FROM
    (SELECT QS.*,
    SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
    ((CASE statement_end_offset
        WHEN -1 THEN DATALENGTH(ST.text)
        ELSE QS.statement_end_offset END
            - QS.statement_start_offset)/2) + 1) AS statement_text
     FROM sys.dm_exec_query_stats AS QS
     CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
GROUP BY query_stats.query_hash
ORDER BY 2 DESC;
```

### <a name="monitoring-blocked-queries"></a><span data-ttu-id="df47e-132">Monitorování blokované dotazy</span><span class="sxs-lookup"><span data-stu-id="df47e-132">Monitoring blocked queries</span></span>
<span data-ttu-id="df47e-133">Pomalá nebo dlouhodobé dotazy můžete přispívat k všimnete nadměrné spotřeby prostředků a být důsledek blokované dotazy.</span><span class="sxs-lookup"><span data-stu-id="df47e-133">Slow or long-running queries can contribute to excessive resource consumption and be the consequence of blocked queries.</span></span> <span data-ttu-id="df47e-134">Důvod blokování může být návrh nízký aplikace, plány chybných dotazů, nedostatečná užitečné indexy a tak dále.</span><span class="sxs-lookup"><span data-stu-id="df47e-134">The cause of the blocking can be poor application design, bad query plans, the lack of useful indexes, and so on.</span></span> <span data-ttu-id="df47e-135">Zobrazení sys.dm_tran_locks můžete získat informace o aktuální uzamčení aktivita ve vaší databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="df47e-135">You can use the sys.dm_tran_locks view to get information about the current locking activity in your Azure SQL Database.</span></span> <span data-ttu-id="df47e-136">Příklad kódu, najdete v části [sys.dm_tran_locks (Transact-SQL)](https://msdn.microsoft.com/library/ms190345.aspx) v SQL Server Books Online.</span><span class="sxs-lookup"><span data-stu-id="df47e-136">For example code, see [sys.dm_tran_locks (Transact-SQL)](https://msdn.microsoft.com/library/ms190345.aspx) in SQL Server Books Online.</span></span>

### <a name="monitoring-query-plans"></a><span data-ttu-id="df47e-137">Monitorování plány dotazů</span><span class="sxs-lookup"><span data-stu-id="df47e-137">Monitoring query plans</span></span>
<span data-ttu-id="df47e-138">Plán dotazu neefektivní může také zvýšit spotřeby procesoru.</span><span class="sxs-lookup"><span data-stu-id="df47e-138">An inefficient query plan also may increase CPU consumption.</span></span> <span data-ttu-id="df47e-139">Následující příklad používá [sys.dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) zobrazení dotazu, který používá většina kumulativní procesoru.</span><span class="sxs-lookup"><span data-stu-id="df47e-139">The following example uses the [sys.dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) view to determine which query uses the most cumulative CPU.</span></span>

```
SELECT
    highest_cpu_queries.plan_handle,
    highest_cpu_queries.total_worker_time,
    q.dbid,
    q.objectid,
    q.number,
    q.encrypted,
    q.[text]
FROM
    (SELECT TOP 50
        qs.plan_handle,
        qs.total_worker_time
    FROM
        sys.dm_exec_query_stats qs
    ORDER BY qs.total_worker_time desc) AS highest_cpu_queries
    CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS q
ORDER BY highest_cpu_queries.total_worker_time DESC;
```

## <a name="see-also"></a><span data-ttu-id="df47e-140">Viz také</span><span class="sxs-lookup"><span data-stu-id="df47e-140">See also</span></span>
[<span data-ttu-id="df47e-141">Úvod do databáze SQL</span><span class="sxs-lookup"><span data-stu-id="df47e-141">Introduction to SQL Database</span></span>](sql-database-technical-overview.md)

