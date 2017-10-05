---
title: "Rozšířené události do databáze SQL | Microsoft Docs"
description: "Popisuje rozšířené události (XEvents) ve službě Azure SQL Database a jak relace události mírně lišit od relace události v systému Microsoft SQL Server."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: 
tags: 
ms.assetid: 3b28cf15-f820-4b3c-8310-908d6d5b9d0c
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: genemi
ms.openlocfilehash: 7e5da1c32484b0b94d2ad32ead6bb7c28f9744aa
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="extended-events-in-sql-database"></a><span data-ttu-id="b8080-103">Rozšířené události v databázi SQL</span><span class="sxs-lookup"><span data-stu-id="b8080-103">Extended events in SQL Database</span></span>
[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

<span data-ttu-id="b8080-104">Toto téma vysvětluje, jak se mírně liší implementace rozšířených událostí ve službě Azure SQL Database ve srovnání s rozšířené události v systému Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b8080-104">This topic explains how the implementation of extended events in Azure SQL Database is slightly different compared to extended events in Microsoft SQL Server.</span></span>

- <span data-ttu-id="b8080-105">Databáze SQL verze 12 získávají funkci rozšířených událostí za sekundu polovinu kalendáře 2015.</span><span class="sxs-lookup"><span data-stu-id="b8080-105">SQL Database V12 gained the extended events feature in the second half of calendar 2015.</span></span>
- <span data-ttu-id="b8080-106">SQL Server došlo rozšířené události od 2008.</span><span class="sxs-lookup"><span data-stu-id="b8080-106">SQL Server has had extended events since 2008.</span></span>
- <span data-ttu-id="b8080-107">Sada funkcí rozšířených událostí na SQL Database je robustní podmnožinu funkcí v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b8080-107">The feature set of extended events on SQL Database is a robust subset of the features on SQL Server.</span></span>

<span data-ttu-id="b8080-108">*Události Xevent* je neformální přezdívka, která se někdy používá pro parametr 'rozšířené události: v blogy a jiných neformální umístění.</span><span class="sxs-lookup"><span data-stu-id="b8080-108">*XEvents* is an informal nickname that is sometimes used for 'extended events' in blogs and other informal locations.</span></span>

<span data-ttu-id="b8080-109">Další informace o rozšířených událostí, Azure SQL Database a Microsoft SQL Server, je k dispozici na:</span><span class="sxs-lookup"><span data-stu-id="b8080-109">Additional information about extended events, for Azure SQL Database and Microsoft SQL Server, is available at:</span></span>

- [<span data-ttu-id="b8080-110">Rychlé zahájení: Rozšířených událostí v systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="b8080-110">Quick Start: Extended events in SQL Server</span></span>](http://msdn.microsoft.com/library/mt733217.aspx)
- [<span data-ttu-id="b8080-111">Rozšířené události</span><span class="sxs-lookup"><span data-stu-id="b8080-111">Extended Events</span></span>](http://msdn.microsoft.com/library/bb630282.aspx)

## <a name="prerequisites"></a><span data-ttu-id="b8080-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b8080-112">Prerequisites</span></span>

<span data-ttu-id="b8080-113">Toto téma předpokládá, že již máte některé znalosti:</span><span class="sxs-lookup"><span data-stu-id="b8080-113">This topic assumes you already have some knowledge of:</span></span>

- <span data-ttu-id="b8080-114">[Služba Azure SQL Database](https://azure.microsoft.com/services/sql-database/).</span><span class="sxs-lookup"><span data-stu-id="b8080-114">[Azure SQL Database service](https://azure.microsoft.com/services/sql-database/).</span></span>
- <span data-ttu-id="b8080-115">[Rozšířené události](http://msdn.microsoft.com/library/bb630282.aspx) v systému Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b8080-115">[Extended events](http://msdn.microsoft.com/library/bb630282.aspx) in Microsoft SQL Server.</span></span>

- <span data-ttu-id="b8080-116">Hromadným naší dokumentaci o rozšířených událostech, se vztahuje na systém SQL Server a databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="b8080-116">The bulk of our documentation about extended events applies to both SQL Server and SQL Database.</span></span>

<span data-ttu-id="b8080-117">Předchozí ohrožení k následujícím položkám je užitečné, pokud vyberete soubor událostí jako [cíl](#AzureXEventsTargets):</span><span class="sxs-lookup"><span data-stu-id="b8080-117">Prior exposure to the following items is helpful when choosing the Event File as the [target](#AzureXEventsTargets):</span></span>

- [<span data-ttu-id="b8080-118">Služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="b8080-118">Azure Storage service</span></span>](https://azure.microsoft.com/services/storage/)


- <span data-ttu-id="b8080-119">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b8080-119">PowerShell</span></span>
    - <span data-ttu-id="b8080-120">[Použití Azure PowerShell s Azure Storage](../storage/common/storage-powershell-guide-full.md) – poskytuje podrobné informace o prostředí PowerShell a službu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="b8080-120">[Using Azure PowerShell with Azure Storage](../storage/common/storage-powershell-guide-full.md) - Provides comprehensive information about PowerShell and the Azure Storage service.</span></span>

## <a name="code-samples"></a><span data-ttu-id="b8080-121">Ukázky kódů</span><span class="sxs-lookup"><span data-stu-id="b8080-121">Code samples</span></span>

<span data-ttu-id="b8080-122">Související témata obsahují dva ukázky kódu:</span><span class="sxs-lookup"><span data-stu-id="b8080-122">Related topics provide two code samples:</span></span>


- [<span data-ttu-id="b8080-123">Prstence kódu cílové vyrovnávací paměti pro rozšířené události v databázi SQL</span><span class="sxs-lookup"><span data-stu-id="b8080-123">Ring Buffer target code for extended events in SQL Database</span></span>](sql-database-xevent-code-ring-buffer.md)
    - <span data-ttu-id="b8080-124">Krátký jednoduchý skript Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="b8080-124">Short simple Transact-SQL script.</span></span>
    - <span data-ttu-id="b8080-125">Jsme zdůraznil v tématu ukázkový kód, že až skončíte s cílem cyklické vyrovnávací paměti, měli uvolnění jeho prostředků spuštěním alter myší `ALTER EVENT SESSION ... ON DATABASE DROP TARGET ...;` příkaz.</span><span class="sxs-lookup"><span data-stu-id="b8080-125">We emphasize in the code sample topic that, when you are done with a Ring Buffer target, you should release its resources by executing an alter-drop `ALTER EVENT SESSION ... ON DATABASE DROP TARGET ...;` statement.</span></span> <span data-ttu-id="b8080-126">Později můžete přidat další instanci cyklické vyrovnávací paměti podle `ALTER EVENT SESSION ... ON DATABASE ADD TARGET ...`.</span><span class="sxs-lookup"><span data-stu-id="b8080-126">Later you can add another instance of Ring Buffer by `ALTER EVENT SESSION ... ON DATABASE ADD TARGET ...`.</span></span>


- [<span data-ttu-id="b8080-127">Kód cílový soubor události pro rozšířené události v databázi SQL</span><span class="sxs-lookup"><span data-stu-id="b8080-127">Event File target code for extended events in SQL Database</span></span>](sql-database-xevent-code-event-file.md)
    - <span data-ttu-id="b8080-128">Fáze 1 je prostředí PowerShell vytvořit kontejner úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="b8080-128">Phase 1 is PowerShell to create an Azure Storage container.</span></span>
    - <span data-ttu-id="b8080-129">Fáze 2 je Transact-SQL, který používá kontejner úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="b8080-129">Phase 2 is Transact-SQL that uses the Azure Storage container.</span></span>

## <a name="transact-sql-differences"></a><span data-ttu-id="b8080-130">Rozdíly v Transact-SQL</span><span class="sxs-lookup"><span data-stu-id="b8080-130">Transact-SQL differences</span></span>


- <span data-ttu-id="b8080-131">Při spuštění [vytvořit událost relace](http://msdn.microsoft.com/library/bb677289.aspx) příkaz na serveru SQL Server, můžete použít **ON SERVER** klauzule.</span><span class="sxs-lookup"><span data-stu-id="b8080-131">When you execute the [CREATE EVENT SESSION](http://msdn.microsoft.com/library/bb677289.aspx) command on SQL Server, you use the **ON SERVER** clause.</span></span> <span data-ttu-id="b8080-132">Ale v databázi SQL můžete použít **ON databáze** klauzule místo.</span><span class="sxs-lookup"><span data-stu-id="b8080-132">But on SQL Database you use the **ON DATABASE** clause instead.</span></span>


- <span data-ttu-id="b8080-133">**ON databáze** klauzule platí také pro [ALTER událostí relace](http://msdn.microsoft.com/library/bb630368.aspx) a [VYŘADIT událostí relace](http://msdn.microsoft.com/library/bb630257.aspx) příkazy jazyka Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="b8080-133">The **ON DATABASE** clause also applies to the [ALTER EVENT SESSION](http://msdn.microsoft.com/library/bb630368.aspx) and [DROP EVENT SESSION](http://msdn.microsoft.com/library/bb630257.aspx) Transact-SQL commands.</span></span>


- <span data-ttu-id="b8080-134">Osvědčeným postupem je zahrnují možnost relace události **STARTUP_STATE = ON** ve vaší **vytvořit událost relace** nebo **ALTER událostí relace** příkazy.</span><span class="sxs-lookup"><span data-stu-id="b8080-134">A best practice is to include the event session option of **STARTUP_STATE = ON** in your **CREATE EVENT SESSION**  or **ALTER EVENT SESSION** statements.</span></span>
    - <span data-ttu-id="b8080-135">**= ON** hodnotu podporuje automatické restartování po Rekonfigurace logické databáze z důvodu selhání.</span><span class="sxs-lookup"><span data-stu-id="b8080-135">The **= ON** value supports an automatic restart after a reconfiguration of the logical database due to a failover.</span></span>

## <a name="new-catalog-views"></a><span data-ttu-id="b8080-136">Nová zobrazení katalogu</span><span class="sxs-lookup"><span data-stu-id="b8080-136">New catalog views</span></span>

<span data-ttu-id="b8080-137">Funkce Rozšířené události podporuje několik [katalogu zobrazení](http://msdn.microsoft.com/library/ms174365.aspx).</span><span class="sxs-lookup"><span data-stu-id="b8080-137">The extended events feature is supported by several [catalog views](http://msdn.microsoft.com/library/ms174365.aspx).</span></span> <span data-ttu-id="b8080-138">Zobrazení katalogu říct o *metadata nebo definice* relací vytvořené uživatelem událostí v aktuální databázi.</span><span class="sxs-lookup"><span data-stu-id="b8080-138">Catalog views tell you about *metadata or definitions* of user-created event sessions in the current database.</span></span> <span data-ttu-id="b8080-139">Zobrazení nevrátí informace o instancích relace aktivní události.</span><span class="sxs-lookup"><span data-stu-id="b8080-139">The views do not return information about instances of active event sessions.</span></span>

| <span data-ttu-id="b8080-140">Název</span><span class="sxs-lookup"><span data-stu-id="b8080-140">Name of</span></span><br/><span data-ttu-id="b8080-141">zobrazení katalogu</span><span class="sxs-lookup"><span data-stu-id="b8080-141">catalog view</span></span> | <span data-ttu-id="b8080-142">Popis</span><span class="sxs-lookup"><span data-stu-id="b8080-142">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="b8080-143">**Sys.database_event_session_actions**</span><span class="sxs-lookup"><span data-stu-id="b8080-143">**sys.database_event_session_actions**</span></span> |<span data-ttu-id="b8080-144">Vrátí řádek pro každou akci v každé události relace události.</span><span class="sxs-lookup"><span data-stu-id="b8080-144">Returns a row for each action on each event of an event session.</span></span> |
| <span data-ttu-id="b8080-145">**Sys.database_event_session_events**</span><span class="sxs-lookup"><span data-stu-id="b8080-145">**sys.database_event_session_events**</span></span> |<span data-ttu-id="b8080-146">Vrátí řádek pro každou jednotlivou událost v relaci události.</span><span class="sxs-lookup"><span data-stu-id="b8080-146">Returns a row for each event in an event session.</span></span> |
| <span data-ttu-id="b8080-147">**Sys.database_event_session_fields**</span><span class="sxs-lookup"><span data-stu-id="b8080-147">**sys.database_event_session_fields**</span></span> |<span data-ttu-id="b8080-148">Vrátí řádek pro každý možné přizpůsobit sloupec, který byl explicitně nastavit na události a cíle.</span><span class="sxs-lookup"><span data-stu-id="b8080-148">Returns a row for each customize-able column that was explicitly set on events and targets.</span></span> |
| <span data-ttu-id="b8080-149">**Sys.database_event_session_targets**</span><span class="sxs-lookup"><span data-stu-id="b8080-149">**sys.database_event_session_targets**</span></span> |<span data-ttu-id="b8080-150">Vrátí řádek pro každý cíl události pro relace události.</span><span class="sxs-lookup"><span data-stu-id="b8080-150">Returns a row for each event target for an event session.</span></span> |
| <span data-ttu-id="b8080-151">**Sys.database_event_sessions**</span><span class="sxs-lookup"><span data-stu-id="b8080-151">**sys.database_event_sessions**</span></span> |<span data-ttu-id="b8080-152">Vrátí řádek pro každou relaci události v databázi SQL Database.</span><span class="sxs-lookup"><span data-stu-id="b8080-152">Returns a row for each event session in the SQL Database database.</span></span> |

<span data-ttu-id="b8080-153">V systému Microsoft SQL Server, podobně jako zobrazení katalogu mít názvy, které zahrnují *server\_*  místo *.database\_*.</span><span class="sxs-lookup"><span data-stu-id="b8080-153">In Microsoft SQL Server, similar catalog views have names that include *.server\_* instead of *.database\_*.</span></span> <span data-ttu-id="b8080-154">Vzor názvů je jako **sys.server_event_%**.</span><span class="sxs-lookup"><span data-stu-id="b8080-154">The name pattern is like **sys.server_event_%**.</span></span>

## <a name="new-dynamic-management-views-dmvshttpmsdnmicrosoftcomlibraryms188754aspx"></a><span data-ttu-id="b8080-155">Nové zobrazení dynamické správy [(zobrazení dynamické správy)](http://msdn.microsoft.com/library/ms188754.aspx)</span><span class="sxs-lookup"><span data-stu-id="b8080-155">New dynamic management views [(DMVs)](http://msdn.microsoft.com/library/ms188754.aspx)</span></span>

<span data-ttu-id="b8080-156">Azure SQL Database má [zobrazení dynamické správy (zobrazení dynamické správy)](http://msdn.microsoft.com/library/bb677293.aspx) podporující rozšířené události.</span><span class="sxs-lookup"><span data-stu-id="b8080-156">Azure SQL Database has [dynamic management views (DMVs)](http://msdn.microsoft.com/library/bb677293.aspx) that support extended events.</span></span> <span data-ttu-id="b8080-157">Zobrazení dynamické správy říct o *active* relace události.</span><span class="sxs-lookup"><span data-stu-id="b8080-157">DMVs tell you about *active* event sessions.</span></span>

| <span data-ttu-id="b8080-158">Název DMV</span><span class="sxs-lookup"><span data-stu-id="b8080-158">Name of DMV</span></span> | <span data-ttu-id="b8080-159">Popis</span><span class="sxs-lookup"><span data-stu-id="b8080-159">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="b8080-160">**Sys.dm_xe_database_session_event_actions**</span><span class="sxs-lookup"><span data-stu-id="b8080-160">**sys.dm_xe_database_session_event_actions**</span></span> |<span data-ttu-id="b8080-161">Vrátí informace o akcích relace události.</span><span class="sxs-lookup"><span data-stu-id="b8080-161">Returns information about event session actions.</span></span> |
| <span data-ttu-id="b8080-162">**Sys.dm_xe_database_session_events**</span><span class="sxs-lookup"><span data-stu-id="b8080-162">**sys.dm_xe_database_session_events**</span></span> |<span data-ttu-id="b8080-163">Vrací informace o události relací.</span><span class="sxs-lookup"><span data-stu-id="b8080-163">Returns information about session events.</span></span> |
| <span data-ttu-id="b8080-164">**Sys.dm_xe_database_session_object_columns**</span><span class="sxs-lookup"><span data-stu-id="b8080-164">**sys.dm_xe_database_session_object_columns**</span></span> |<span data-ttu-id="b8080-165">Zobrazuje hodnoty konfigurace pro objekty, které jsou vázány na relaci.</span><span class="sxs-lookup"><span data-stu-id="b8080-165">Shows the configuration values for objects that are bound to a session.</span></span> |
| <span data-ttu-id="b8080-166">**Sys.dm_xe_database_session_targets**</span><span class="sxs-lookup"><span data-stu-id="b8080-166">**sys.dm_xe_database_session_targets**</span></span> |<span data-ttu-id="b8080-167">Vrací informace o relaci cíle.</span><span class="sxs-lookup"><span data-stu-id="b8080-167">Returns information about session targets.</span></span> |
| <span data-ttu-id="b8080-168">**jestli v Sys.dm_xe_database_sessions nejsou**</span><span class="sxs-lookup"><span data-stu-id="b8080-168">**sys.dm_xe_database_sessions**</span></span> |<span data-ttu-id="b8080-169">Vrátí řádek pro každou relaci události, které budou platit na aktuální databázi.</span><span class="sxs-lookup"><span data-stu-id="b8080-169">Returns a row for each event session that is scoped to the current database.</span></span> |

<span data-ttu-id="b8080-170">V systému Microsoft SQL Server, jsou podobné zobrazení katalogu pojmenované bez  *\_databáze* část názvu, jako například:</span><span class="sxs-lookup"><span data-stu-id="b8080-170">In Microsoft SQL Server, similar catalog views are named without the *\_database* portion of the name, such as:</span></span>

- <span data-ttu-id="b8080-171">**Sys.dm_xe_sessions**, místo názvu</span><span class="sxs-lookup"><span data-stu-id="b8080-171">**sys.dm_xe_sessions**, instead of name</span></span><br/><span data-ttu-id="b8080-172">**jestli v Sys.dm_xe_database_sessions nejsou**.</span><span class="sxs-lookup"><span data-stu-id="b8080-172">**sys.dm_xe_database_sessions**.</span></span>

### <a name="dmvs-common-to-both"></a><span data-ttu-id="b8080-173">Zobrazení dynamické správy společné pro objekty</span><span class="sxs-lookup"><span data-stu-id="b8080-173">DMVs common to both</span></span>
<span data-ttu-id="b8080-174">Rozšířené události existují další zobrazení dynamické správy, které jsou společné pro Azure SQL Database a serveru Microsoft SQL Server:</span><span class="sxs-lookup"><span data-stu-id="b8080-174">For extended events there are additional DMVs that are common to both Azure SQL Database and Microsoft SQL Server:</span></span>

- <span data-ttu-id="b8080-175">**Sys.dm_xe_map_values**</span><span class="sxs-lookup"><span data-stu-id="b8080-175">**sys.dm_xe_map_values**</span></span>
- <span data-ttu-id="b8080-176">**Sys.dm_xe_object_columns**</span><span class="sxs-lookup"><span data-stu-id="b8080-176">**sys.dm_xe_object_columns**</span></span>
- <span data-ttu-id="b8080-177">**Sys.dm_xe_objects**</span><span class="sxs-lookup"><span data-stu-id="b8080-177">**sys.dm_xe_objects**</span></span>
- <span data-ttu-id="b8080-178">**Sys.dm_xe_packages**</span><span class="sxs-lookup"><span data-stu-id="b8080-178">**sys.dm_xe_packages**</span></span>

 <a name="sqlfindseventsactionstargets" id="sqlfindseventsactionstargets"></a>

## <a name="find-the-available-extended-events-actions-and-targets"></a><span data-ttu-id="b8080-179">Najít dostupných rozšířených událostí, akcí a cíle</span><span class="sxs-lookup"><span data-stu-id="b8080-179">Find the available extended events, actions, and targets</span></span>

<span data-ttu-id="b8080-180">Můžete spustit jednoduché SQL **vyberte** získat seznam dostupných událostí, akcí a cíle.</span><span class="sxs-lookup"><span data-stu-id="b8080-180">You can run a simple SQL **SELECT** to obtain a list of the available events, actions, and target.</span></span>

```sql
SELECT
        o.object_type,
        p.name         AS [package_name],
        o.name         AS [db_object_name],
        o.description  AS [db_obj_description]
    FROM
                   sys.dm_xe_objects  AS o
        INNER JOIN sys.dm_xe_packages AS p  ON p.guid = o.package_guid
    WHERE
        o.object_type in
            (
            'action',  'event',  'target'
            )
    ORDER BY
        o.object_type,
        p.name,
        o.name;
```


<span data-ttu-id="b8080-181"><a name="AzureXEventsTargets" id="AzureXEventsTargets"></a> &nbsp;</span><span class="sxs-lookup"><span data-stu-id="b8080-181"><a name="AzureXEventsTargets" id="AzureXEventsTargets"></a> &nbsp;</span></span>

## <a name="targets-for-your-sql-database-event-sessions"></a><span data-ttu-id="b8080-182">Cíle pro relace události vaší databáze SQL</span><span class="sxs-lookup"><span data-stu-id="b8080-182">Targets for your SQL Database event sessions</span></span>

<span data-ttu-id="b8080-183">Zde jsou cíle, které můžete zaznamenat výsledky z vaší relace události v databázi SQL:</span><span class="sxs-lookup"><span data-stu-id="b8080-183">Here are targets that can capture results from your event sessions on SQL Database:</span></span>

- <span data-ttu-id="b8080-184">[Cílové vyrovnávací paměti prstenec](http://msdn.microsoft.com/library/ff878182.aspx) -stručně uchovává data události v paměti.</span><span class="sxs-lookup"><span data-stu-id="b8080-184">[Ring Buffer target](http://msdn.microsoft.com/library/ff878182.aspx) - Briefly holds event data in memory.</span></span>
- <span data-ttu-id="b8080-185">[Cíl události čítače](http://msdn.microsoft.com/library/ff878025.aspx) – počty všechny události, ke kterým došlo během relace rozšířených událostí.</span><span class="sxs-lookup"><span data-stu-id="b8080-185">[Event Counter target](http://msdn.microsoft.com/library/ff878025.aspx) - Counts all events that occur during an extended events session.</span></span>
- <span data-ttu-id="b8080-186">[Cíl souboru událostí](http://msdn.microsoft.com/library/ff878115.aspx) -zapíše dokončení vyrovnávací paměti s kontejnerem Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="b8080-186">[Event File target](http://msdn.microsoft.com/library/ff878115.aspx) - Writes complete buffers to an Azure Storage container.</span></span>

<span data-ttu-id="b8080-187">[Trasování událostí pro Windows (ETW)](http://msdn.microsoft.com/library/ms751538.aspx) rozhraní API není k dispozici pro rozšířené události v databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="b8080-187">The [Event Tracing for Windows (ETW)](http://msdn.microsoft.com/library/ms751538.aspx) API is not available for extended events on SQL Database.</span></span>

## <a name="restrictions"></a><span data-ttu-id="b8080-188">Omezení</span><span class="sxs-lookup"><span data-stu-id="b8080-188">Restrictions</span></span>

<span data-ttu-id="b8080-189">Existuje několik rozdílů související se zabezpečením befitting prostředí cloudu SQL Database:</span><span class="sxs-lookup"><span data-stu-id="b8080-189">There are a couple of security-related differences befitting the cloud environment of SQL Database:</span></span>

- <span data-ttu-id="b8080-190">Rozšířené události vznikla na izolaci klientů jeden model.</span><span class="sxs-lookup"><span data-stu-id="b8080-190">Extended events are founded on the single-tenant isolation model.</span></span> <span data-ttu-id="b8080-191">Relace události v jedné databáze nelze přistupovat k datům nebo události z jiné databáze.</span><span class="sxs-lookup"><span data-stu-id="b8080-191">An event session in one database cannot access data or events from another database.</span></span>
- <span data-ttu-id="b8080-192">Nemůžou vystavovat **vytvořit událost relace** příkaz v kontextu **hlavní** databáze.</span><span class="sxs-lookup"><span data-stu-id="b8080-192">You cannot issue a **CREATE EVENT SESSION** statement in the context of the **master** database.</span></span>

## <a name="permission-model"></a><span data-ttu-id="b8080-193">Oprávnění modelu</span><span class="sxs-lookup"><span data-stu-id="b8080-193">Permission model</span></span>

<span data-ttu-id="b8080-194">Musíte mít **řízení** oprávnění v databázi k vydávání **vytvořit událost relace** příkaz.</span><span class="sxs-lookup"><span data-stu-id="b8080-194">You must have **Control** permission on the database to issue a **CREATE EVENT SESSION** statement.</span></span> <span data-ttu-id="b8080-195">Vlastník databáze (dbo) má **řízení** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="b8080-195">The database owner (dbo) has **Control** permission.</span></span>

### <a name="storage-container-authorizations"></a><span data-ttu-id="b8080-196">Povolení kontejneru úložiště</span><span class="sxs-lookup"><span data-stu-id="b8080-196">Storage container authorizations</span></span>

<span data-ttu-id="b8080-197">Tokenu SAS můžete vygenerovat pro vaše kontejner úložiště Azure, musíte zadat **rwl** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="b8080-197">The SAS token you generate for your Azure Storage container must specify **rwl** for the permissions.</span></span> <span data-ttu-id="b8080-198">**Rwl** hodnota poskytuje následující oprávnění:</span><span class="sxs-lookup"><span data-stu-id="b8080-198">The **rwl** value provides the following permissions:</span></span>

- <span data-ttu-id="b8080-199">Čtení</span><span class="sxs-lookup"><span data-stu-id="b8080-199">Read</span></span>
- <span data-ttu-id="b8080-200">Zápis</span><span class="sxs-lookup"><span data-stu-id="b8080-200">Write</span></span>
- <span data-ttu-id="b8080-201">Seznam</span><span class="sxs-lookup"><span data-stu-id="b8080-201">List</span></span>

## <a name="performance-considerations"></a><span data-ttu-id="b8080-202">Otázky výkonu</span><span class="sxs-lookup"><span data-stu-id="b8080-202">Performance considerations</span></span>

<span data-ttu-id="b8080-203">Existují scénáře, kde můžete náročné pomocí rozšířených událostí hromadit aktivnější paměti, než je v pořádku pro celého systému.</span><span class="sxs-lookup"><span data-stu-id="b8080-203">There are scenarios where intensive use of extended events can accumulate more active memory than is healthy for the overall system.</span></span> <span data-ttu-id="b8080-204">Proto systém Azure SQL Database dynamicky nastaví a upraví omezení pro aktivní paměti, která může být shromážděných řešením relace události.</span><span class="sxs-lookup"><span data-stu-id="b8080-204">Therefore the Azure SQL Database system dynamically sets and adjusts limits on the amount of active memory that can be accumulated by an event session.</span></span> <span data-ttu-id="b8080-205">Mnoha faktorech, přejděte do dynamické výpočtu.</span><span class="sxs-lookup"><span data-stu-id="b8080-205">Many factors go into the dynamic calculation.</span></span>

<span data-ttu-id="b8080-206">Pokud se zobrazí chybová zpráva s upozorněním, že byla vynucená maximální paměť, jsou některé opravné akce, které můžete provést:</span><span class="sxs-lookup"><span data-stu-id="b8080-206">If you receive an error message that says a memory maximum was enforced, some corrective actions you can take are:</span></span>

- <span data-ttu-id="b8080-207">Spuštění relace méně souběžných události.</span><span class="sxs-lookup"><span data-stu-id="b8080-207">Run fewer concurrent event sessions.</span></span>
- <span data-ttu-id="b8080-208">Prostřednictvím vaší **vytvořit** a **ALTER** příkazy pro relace události, snížit množství paměti, které jste zadali na **maximální\_paměti** klauzule.</span><span class="sxs-lookup"><span data-stu-id="b8080-208">Through your **CREATE** and **ALTER** statements for event sessions, reduce the amount of memory you specify on the **MAX\_MEMORY** clause.</span></span>

### <a name="network-latency"></a><span data-ttu-id="b8080-209">Latence sítě</span><span class="sxs-lookup"><span data-stu-id="b8080-209">Network latency</span></span>

<span data-ttu-id="b8080-210">**Soubor událostí** cíl setkat latence sítě nebo selhání při zachování dat na objekty BLOB úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="b8080-210">The **Event File** target might experience network latency or failures while persisting data to Azure Storage blobs.</span></span> <span data-ttu-id="b8080-211">Při čekání komunikaci sítě s k dokončení může být zpoždění další události v databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="b8080-211">Other events in SQL Database might be delayed while they wait for the network communication to complete.</span></span> <span data-ttu-id="b8080-212">Toto zpoždění může dojít ke snížení velikosti pracovní zátěže.</span><span class="sxs-lookup"><span data-stu-id="b8080-212">This delay can slow your workload.</span></span>

- <span data-ttu-id="b8080-213">Pro zmírnění rizik tento výkon, vyhněte se nastavení **EVENT_RETENTION_MODE** možnost k **NO_EVENT_LOSS** v definic relace události.</span><span class="sxs-lookup"><span data-stu-id="b8080-213">To mitigate this performance risk, avoid setting the **EVENT_RETENTION_MODE** option to **NO_EVENT_LOSS** in your event session definitions.</span></span>

## <a name="related-links"></a><span data-ttu-id="b8080-214">Související odkazy</span><span class="sxs-lookup"><span data-stu-id="b8080-214">Related links</span></span>

- <span data-ttu-id="b8080-215">[Použití Azure PowerShell s Azure Storage](../storage/common/storage-powershell-guide-full.md).</span><span class="sxs-lookup"><span data-stu-id="b8080-215">[Using Azure PowerShell with Azure Storage](../storage/common/storage-powershell-guide-full.md).</span></span>
- [<span data-ttu-id="b8080-216">Rutiny úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="b8080-216">Azure Storage Cmdlets</span></span>](http://msdn.microsoft.com/library/dn806401.aspx)
- <span data-ttu-id="b8080-217">[Použití Azure PowerShell s Azure Storage](../storage/common/storage-powershell-guide-full.md) – poskytuje podrobné informace o prostředí PowerShell a službu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="b8080-217">[Using Azure PowerShell with Azure Storage](../storage/common/storage-powershell-guide-full.md) - Provides comprehensive information about PowerShell and the Azure Storage service.</span></span>
- [<span data-ttu-id="b8080-218">Používání úložiště Blob z rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="b8080-218">How to use Blob storage from .NET</span></span>](../storage/blobs/storage-dotnet-how-to-use-blobs.md)
- [<span data-ttu-id="b8080-219">CREATE CREDENTIAL (Transact-SQL)</span><span class="sxs-lookup"><span data-stu-id="b8080-219">CREATE CREDENTIAL (Transact-SQL)</span></span>](http://msdn.microsoft.com/library/ms189522.aspx)
- [<span data-ttu-id="b8080-220">Vytvoření relace události (Transact-SQL)</span><span class="sxs-lookup"><span data-stu-id="b8080-220">CREATE EVENT SESSION (Transact-SQL)</span></span>](http://msdn.microsoft.com/library/bb677289.aspx)
- [<span data-ttu-id="b8080-221">Jonathan Kehayias příspěvky o rozšířených událostí v systému Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="b8080-221">Jonathan Kehayias' blog posts about extended events in Microsoft SQL Server</span></span>](http://www.sqlskills.com/blogs/jonathan/category/extended-events/)


- <span data-ttu-id="b8080-222">Azure *aktualizace služby* webovou stránku, co nejlépe určen parametrem do Azure SQL Database:</span><span class="sxs-lookup"><span data-stu-id="b8080-222">The Azure *Service Updates* webpage, narrowed by parameter to Azure SQL Database:</span></span>
    - [<span data-ttu-id="b8080-223">https://Azure.microsoft.com/Updates/?Service=SQL-Database</span><span class="sxs-lookup"><span data-stu-id="b8080-223">https://azure.microsoft.com/updates/?service=sql-database</span></span>](https://azure.microsoft.com/updates/?service=sql-database)


<span data-ttu-id="b8080-224">Další témata ukázkový kód pro rozšířené události jsou dostupné prostřednictvím následujících odkazů.</span><span class="sxs-lookup"><span data-stu-id="b8080-224">Other code sample topics for extended events are available at the following links.</span></span> <span data-ttu-id="b8080-225">Ale je nutné pravidelně zkontrolovat všechny ukázkové zobrazíte zda ukázku cílem Microsoft SQL Server a databáze SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="b8080-225">However, you must routinely check any sample to see whether the sample targets Microsoft SQL Server versus Azure SQL Database.</span></span> <span data-ttu-id="b8080-226">Potom můžete rozhodnout, zda jsou mírně potřebné ke spuštění ukázky.</span><span class="sxs-lookup"><span data-stu-id="b8080-226">Then you can decide whether minor changes are needed to run the sample.</span></span>

<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
