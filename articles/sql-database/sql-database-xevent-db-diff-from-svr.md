---
title: "aaaExtended události do databáze SQL | Microsoft Docs"
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
ms.openlocfilehash: 8c966a84412aa561c92b16e5c6902102483eb1bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="extended-events-in-sql-database"></a><span data-ttu-id="b3408-103">Rozšířené události v databázi SQL</span><span class="sxs-lookup"><span data-stu-id="b3408-103">Extended events in SQL Database</span></span>
[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

<span data-ttu-id="b3408-104">Toto téma vysvětluje, jak hello implementace rozšířených událostí ve službě Azure SQL Database je mírně odlišný porovnání tooextended události v systému Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b3408-104">This topic explains how hello implementation of extended events in Azure SQL Database is slightly different compared tooextended events in Microsoft SQL Server.</span></span>

- <span data-ttu-id="b3408-105">Databáze SQL verze 12 získávají hello funkce Rozšířené události v hello druhou polovinu kalendáře 2015.</span><span class="sxs-lookup"><span data-stu-id="b3408-105">SQL Database V12 gained hello extended events feature in hello second half of calendar 2015.</span></span>
- <span data-ttu-id="b3408-106">SQL Server došlo rozšířené události od 2008.</span><span class="sxs-lookup"><span data-stu-id="b3408-106">SQL Server has had extended events since 2008.</span></span>
- <span data-ttu-id="b3408-107">Sada funkcí Hello rozšířených událostí na SQL Database je robustní podmnožinu funkcí hello na serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b3408-107">hello feature set of extended events on SQL Database is a robust subset of hello features on SQL Server.</span></span>

<span data-ttu-id="b3408-108">*Události Xevent* je neformální přezdívka, která se někdy používá pro parametr 'rozšířené události: v blogy a jiných neformální umístění.</span><span class="sxs-lookup"><span data-stu-id="b3408-108">*XEvents* is an informal nickname that is sometimes used for 'extended events' in blogs and other informal locations.</span></span>

<span data-ttu-id="b3408-109">Další informace o rozšířených událostí, Azure SQL Database a Microsoft SQL Server, je k dispozici na:</span><span class="sxs-lookup"><span data-stu-id="b3408-109">Additional information about extended events, for Azure SQL Database and Microsoft SQL Server, is available at:</span></span>

- [<span data-ttu-id="b3408-110">Rychlé zahájení: Rozšířených událostí v systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="b3408-110">Quick Start: Extended events in SQL Server</span></span>](http://msdn.microsoft.com/library/mt733217.aspx)
- [<span data-ttu-id="b3408-111">Rozšířené události</span><span class="sxs-lookup"><span data-stu-id="b3408-111">Extended Events</span></span>](http://msdn.microsoft.com/library/bb630282.aspx)

## <a name="prerequisites"></a><span data-ttu-id="b3408-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b3408-112">Prerequisites</span></span>

<span data-ttu-id="b3408-113">Toto téma předpokládá, že již máte některé znalosti:</span><span class="sxs-lookup"><span data-stu-id="b3408-113">This topic assumes you already have some knowledge of:</span></span>

- <span data-ttu-id="b3408-114">[Služba Azure SQL Database](https://azure.microsoft.com/services/sql-database/).</span><span class="sxs-lookup"><span data-stu-id="b3408-114">[Azure SQL Database service](https://azure.microsoft.com/services/sql-database/).</span></span>
- <span data-ttu-id="b3408-115">[Rozšířené události](http://msdn.microsoft.com/library/bb630282.aspx) v systému Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b3408-115">[Extended events](http://msdn.microsoft.com/library/bb630282.aspx) in Microsoft SQL Server.</span></span>

- <span data-ttu-id="b3408-116">hromadné Hello část naší dokumentace o rozšířených událostí platí tooboth systému SQL Server a SQL Database.</span><span class="sxs-lookup"><span data-stu-id="b3408-116">hello bulk of our documentation about extended events applies tooboth SQL Server and SQL Database.</span></span>

<span data-ttu-id="b3408-117">Předchozí ohrožení toohello následující položky je užitečné, pokud vyberete hello soubor událostí jako hello [cíl](#AzureXEventsTargets):</span><span class="sxs-lookup"><span data-stu-id="b3408-117">Prior exposure toohello following items is helpful when choosing hello Event File as hello [target](#AzureXEventsTargets):</span></span>

- [<span data-ttu-id="b3408-118">Služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="b3408-118">Azure Storage service</span></span>](https://azure.microsoft.com/services/storage/)


- <span data-ttu-id="b3408-119">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b3408-119">PowerShell</span></span>
    - <span data-ttu-id="b3408-120">[Použití Azure PowerShell s Azure Storage](../storage/common/storage-powershell-guide-full.md) – poskytuje podrobné informace o prostředí PowerShell a hello služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="b3408-120">[Using Azure PowerShell with Azure Storage](../storage/common/storage-powershell-guide-full.md) - Provides comprehensive information about PowerShell and hello Azure Storage service.</span></span>

## <a name="code-samples"></a><span data-ttu-id="b3408-121">Ukázky kódů</span><span class="sxs-lookup"><span data-stu-id="b3408-121">Code samples</span></span>

<span data-ttu-id="b3408-122">Související témata obsahují dva ukázky kódu:</span><span class="sxs-lookup"><span data-stu-id="b3408-122">Related topics provide two code samples:</span></span>


- [<span data-ttu-id="b3408-123">Prstence kódu cílové vyrovnávací paměti pro rozšířené události v databázi SQL</span><span class="sxs-lookup"><span data-stu-id="b3408-123">Ring Buffer target code for extended events in SQL Database</span></span>](sql-database-xevent-code-ring-buffer.md)
    - <span data-ttu-id="b3408-124">Krátký jednoduchý skript Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="b3408-124">Short simple Transact-SQL script.</span></span>
    - <span data-ttu-id="b3408-125">Jsme zdůraznil v tématu ukázkový kód hello je, že až skončíte s cílem cyklické vyrovnávací paměti, měli uvolnění jeho prostředků spuštěním alter myší `ALTER EVENT SESSION ... ON DATABASE DROP TARGET ...;` příkaz.</span><span class="sxs-lookup"><span data-stu-id="b3408-125">We emphasize in hello code sample topic that, when you are done with a Ring Buffer target, you should release its resources by executing an alter-drop `ALTER EVENT SESSION ... ON DATABASE DROP TARGET ...;` statement.</span></span> <span data-ttu-id="b3408-126">Později můžete přidat další instanci cyklické vyrovnávací paměti podle `ALTER EVENT SESSION ... ON DATABASE ADD TARGET ...`.</span><span class="sxs-lookup"><span data-stu-id="b3408-126">Later you can add another instance of Ring Buffer by `ALTER EVENT SESSION ... ON DATABASE ADD TARGET ...`.</span></span>


- [<span data-ttu-id="b3408-127">Kód cílový soubor události pro rozšířené události v databázi SQL</span><span class="sxs-lookup"><span data-stu-id="b3408-127">Event File target code for extended events in SQL Database</span></span>](sql-database-xevent-code-event-file.md)
    - <span data-ttu-id="b3408-128">Fáze 1 je prostředí PowerShell toocreate kontejner Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="b3408-128">Phase 1 is PowerShell toocreate an Azure Storage container.</span></span>
    - <span data-ttu-id="b3408-129">Fáze 2 je Transact-SQL, používající hello kontejner úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="b3408-129">Phase 2 is Transact-SQL that uses hello Azure Storage container.</span></span>

## <a name="transact-sql-differences"></a><span data-ttu-id="b3408-130">Rozdíly v Transact-SQL</span><span class="sxs-lookup"><span data-stu-id="b3408-130">Transact-SQL differences</span></span>


- <span data-ttu-id="b3408-131">Při spuštění hello [vytvořit událost relace](http://msdn.microsoft.com/library/bb677289.aspx) příkaz na serveru SQL Server, použijte hello **ON SERVER** klauzule.</span><span class="sxs-lookup"><span data-stu-id="b3408-131">When you execute hello [CREATE EVENT SESSION](http://msdn.microsoft.com/library/bb677289.aspx) command on SQL Server, you use hello **ON SERVER** clause.</span></span> <span data-ttu-id="b3408-132">Ale v databázi SQL používat hello **ON databáze** klauzule místo.</span><span class="sxs-lookup"><span data-stu-id="b3408-132">But on SQL Database you use hello **ON DATABASE** clause instead.</span></span>


- <span data-ttu-id="b3408-133">Hello **ON databáze** klauzule platí také toohello [ALTER událostí relace](http://msdn.microsoft.com/library/bb630368.aspx) a [VYŘADIT událostí relace](http://msdn.microsoft.com/library/bb630257.aspx) příkazy jazyka Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="b3408-133">hello **ON DATABASE** clause also applies toohello [ALTER EVENT SESSION](http://msdn.microsoft.com/library/bb630368.aspx) and [DROP EVENT SESSION](http://msdn.microsoft.com/library/bb630257.aspx) Transact-SQL commands.</span></span>


- <span data-ttu-id="b3408-134">Osvědčeným postupem je možnost relace události hello tooinclude z **STARTUP_STATE = ON** ve vaší **vytvořit událost relace** nebo **ALTER událostí relace** příkazy.</span><span class="sxs-lookup"><span data-stu-id="b3408-134">A best practice is tooinclude hello event session option of **STARTUP_STATE = ON** in your **CREATE EVENT SESSION**  or **ALTER EVENT SESSION** statements.</span></span>
    - <span data-ttu-id="b3408-135">Hello **= ON** hodnotu podporuje automatické restartování po Rekonfigurace hello logické databáze z důvodu tooa převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="b3408-135">hello **= ON** value supports an automatic restart after a reconfiguration of hello logical database due tooa failover.</span></span>

## <a name="new-catalog-views"></a><span data-ttu-id="b3408-136">Nová zobrazení katalogu</span><span class="sxs-lookup"><span data-stu-id="b3408-136">New catalog views</span></span>

<span data-ttu-id="b3408-137">Hello rozšířených událostí je funkce podporována několik [katalogu zobrazení](http://msdn.microsoft.com/library/ms174365.aspx).</span><span class="sxs-lookup"><span data-stu-id="b3408-137">hello extended events feature is supported by several [catalog views](http://msdn.microsoft.com/library/ms174365.aspx).</span></span> <span data-ttu-id="b3408-138">Zobrazení katalogu říct o *metadata nebo definice* relací vytvořené uživatelem událostí v aktuální databázi hello.</span><span class="sxs-lookup"><span data-stu-id="b3408-138">Catalog views tell you about *metadata or definitions* of user-created event sessions in hello current database.</span></span> <span data-ttu-id="b3408-139">zobrazení Hello nevrátí informace o instancích relace aktivní události.</span><span class="sxs-lookup"><span data-stu-id="b3408-139">hello views do not return information about instances of active event sessions.</span></span>

| <span data-ttu-id="b3408-140">Název</span><span class="sxs-lookup"><span data-stu-id="b3408-140">Name of</span></span><br/><span data-ttu-id="b3408-141">zobrazení katalogu</span><span class="sxs-lookup"><span data-stu-id="b3408-141">catalog view</span></span> | <span data-ttu-id="b3408-142">Popis</span><span class="sxs-lookup"><span data-stu-id="b3408-142">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="b3408-143">**Sys.database_event_session_actions**</span><span class="sxs-lookup"><span data-stu-id="b3408-143">**sys.database_event_session_actions**</span></span> |<span data-ttu-id="b3408-144">Vrátí řádek pro každou akci v každé události relace události.</span><span class="sxs-lookup"><span data-stu-id="b3408-144">Returns a row for each action on each event of an event session.</span></span> |
| <span data-ttu-id="b3408-145">**Sys.database_event_session_events**</span><span class="sxs-lookup"><span data-stu-id="b3408-145">**sys.database_event_session_events**</span></span> |<span data-ttu-id="b3408-146">Vrátí řádek pro každou jednotlivou událost v relaci události.</span><span class="sxs-lookup"><span data-stu-id="b3408-146">Returns a row for each event in an event session.</span></span> |
| <span data-ttu-id="b3408-147">**Sys.database_event_session_fields**</span><span class="sxs-lookup"><span data-stu-id="b3408-147">**sys.database_event_session_fields**</span></span> |<span data-ttu-id="b3408-148">Vrátí řádek pro každý možné přizpůsobit sloupec, který byl explicitně nastavit na události a cíle.</span><span class="sxs-lookup"><span data-stu-id="b3408-148">Returns a row for each customize-able column that was explicitly set on events and targets.</span></span> |
| <span data-ttu-id="b3408-149">**Sys.database_event_session_targets**</span><span class="sxs-lookup"><span data-stu-id="b3408-149">**sys.database_event_session_targets**</span></span> |<span data-ttu-id="b3408-150">Vrátí řádek pro každý cíl události pro relace události.</span><span class="sxs-lookup"><span data-stu-id="b3408-150">Returns a row for each event target for an event session.</span></span> |
| <span data-ttu-id="b3408-151">**Sys.database_event_sessions**</span><span class="sxs-lookup"><span data-stu-id="b3408-151">**sys.database_event_sessions**</span></span> |<span data-ttu-id="b3408-152">Vrátí řádek pro každou relaci události v databázi SQL Database hello.</span><span class="sxs-lookup"><span data-stu-id="b3408-152">Returns a row for each event session in hello SQL Database database.</span></span> |

<span data-ttu-id="b3408-153">V systému Microsoft SQL Server, podobně jako zobrazení katalogu mít názvy, které zahrnují *server\_*  místo *.database\_*.</span><span class="sxs-lookup"><span data-stu-id="b3408-153">In Microsoft SQL Server, similar catalog views have names that include *.server\_* instead of *.database\_*.</span></span> <span data-ttu-id="b3408-154">vzor názvů Hello je jako **sys.server_event_%**.</span><span class="sxs-lookup"><span data-stu-id="b3408-154">hello name pattern is like **sys.server_event_%**.</span></span>

## <a name="new-dynamic-management-views-dmvshttpmsdnmicrosoftcomlibraryms188754aspx"></a><span data-ttu-id="b3408-155">Nové zobrazení dynamické správy [(zobrazení dynamické správy)](http://msdn.microsoft.com/library/ms188754.aspx)</span><span class="sxs-lookup"><span data-stu-id="b3408-155">New dynamic management views [(DMVs)](http://msdn.microsoft.com/library/ms188754.aspx)</span></span>

<span data-ttu-id="b3408-156">Azure SQL Database má [zobrazení dynamické správy (zobrazení dynamické správy)](http://msdn.microsoft.com/library/bb677293.aspx) podporující rozšířené události.</span><span class="sxs-lookup"><span data-stu-id="b3408-156">Azure SQL Database has [dynamic management views (DMVs)](http://msdn.microsoft.com/library/bb677293.aspx) that support extended events.</span></span> <span data-ttu-id="b3408-157">Zobrazení dynamické správy říct o *active* relace události.</span><span class="sxs-lookup"><span data-stu-id="b3408-157">DMVs tell you about *active* event sessions.</span></span>

| <span data-ttu-id="b3408-158">Název DMV</span><span class="sxs-lookup"><span data-stu-id="b3408-158">Name of DMV</span></span> | <span data-ttu-id="b3408-159">Popis</span><span class="sxs-lookup"><span data-stu-id="b3408-159">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="b3408-160">**Sys.dm_xe_database_session_event_actions**</span><span class="sxs-lookup"><span data-stu-id="b3408-160">**sys.dm_xe_database_session_event_actions**</span></span> |<span data-ttu-id="b3408-161">Vrátí informace o akcích relace události.</span><span class="sxs-lookup"><span data-stu-id="b3408-161">Returns information about event session actions.</span></span> |
| <span data-ttu-id="b3408-162">**Sys.dm_xe_database_session_events**</span><span class="sxs-lookup"><span data-stu-id="b3408-162">**sys.dm_xe_database_session_events**</span></span> |<span data-ttu-id="b3408-163">Vrací informace o události relací.</span><span class="sxs-lookup"><span data-stu-id="b3408-163">Returns information about session events.</span></span> |
| <span data-ttu-id="b3408-164">**Sys.dm_xe_database_session_object_columns**</span><span class="sxs-lookup"><span data-stu-id="b3408-164">**sys.dm_xe_database_session_object_columns**</span></span> |<span data-ttu-id="b3408-165">Zobrazuje hello hodnoty konfigurace pro objekty, které jsou vázané tooa relace.</span><span class="sxs-lookup"><span data-stu-id="b3408-165">Shows hello configuration values for objects that are bound tooa session.</span></span> |
| <span data-ttu-id="b3408-166">**Sys.dm_xe_database_session_targets**</span><span class="sxs-lookup"><span data-stu-id="b3408-166">**sys.dm_xe_database_session_targets**</span></span> |<span data-ttu-id="b3408-167">Vrací informace o relaci cíle.</span><span class="sxs-lookup"><span data-stu-id="b3408-167">Returns information about session targets.</span></span> |
| <span data-ttu-id="b3408-168">**jestli v Sys.dm_xe_database_sessions nejsou**</span><span class="sxs-lookup"><span data-stu-id="b3408-168">**sys.dm_xe_database_sessions**</span></span> |<span data-ttu-id="b3408-169">Vrátí řádek pro každou relaci události, který je vymezená toohello aktuální databázi.</span><span class="sxs-lookup"><span data-stu-id="b3408-169">Returns a row for each event session that is scoped toohello current database.</span></span> |

<span data-ttu-id="b3408-170">V systému Microsoft SQL Server, jsou podobné zobrazení katalogu pojmenované bez hello  *\_databáze* část hello název, jako například:</span><span class="sxs-lookup"><span data-stu-id="b3408-170">In Microsoft SQL Server, similar catalog views are named without hello *\_database* portion of hello name, such as:</span></span>

- <span data-ttu-id="b3408-171">**Sys.dm_xe_sessions**, místo názvu</span><span class="sxs-lookup"><span data-stu-id="b3408-171">**sys.dm_xe_sessions**, instead of name</span></span><br/><span data-ttu-id="b3408-172">**jestli v Sys.dm_xe_database_sessions nejsou**.</span><span class="sxs-lookup"><span data-stu-id="b3408-172">**sys.dm_xe_database_sessions**.</span></span>

### <a name="dmvs-common-tooboth"></a><span data-ttu-id="b3408-173">Běžné tooboth zobrazení dynamické správy</span><span class="sxs-lookup"><span data-stu-id="b3408-173">DMVs common tooboth</span></span>
<span data-ttu-id="b3408-174">Pro rozšířené události existují další zobrazení dynamické správy, které jsou společné tooboth Azure SQL Database a serveru Microsoft SQL Server:</span><span class="sxs-lookup"><span data-stu-id="b3408-174">For extended events there are additional DMVs that are common tooboth Azure SQL Database and Microsoft SQL Server:</span></span>

- <span data-ttu-id="b3408-175">**Sys.dm_xe_map_values**</span><span class="sxs-lookup"><span data-stu-id="b3408-175">**sys.dm_xe_map_values**</span></span>
- <span data-ttu-id="b3408-176">**Sys.dm_xe_object_columns**</span><span class="sxs-lookup"><span data-stu-id="b3408-176">**sys.dm_xe_object_columns**</span></span>
- <span data-ttu-id="b3408-177">**Sys.dm_xe_objects**</span><span class="sxs-lookup"><span data-stu-id="b3408-177">**sys.dm_xe_objects**</span></span>
- <span data-ttu-id="b3408-178">**Sys.dm_xe_packages**</span><span class="sxs-lookup"><span data-stu-id="b3408-178">**sys.dm_xe_packages**</span></span>

 <a name="sqlfindseventsactionstargets" id="sqlfindseventsactionstargets"></a>

## <a name="find-hello-available-extended-events-actions-and-targets"></a><span data-ttu-id="b3408-179">Najít hello dostupných rozšířených událostí, akcí a cíle</span><span class="sxs-lookup"><span data-stu-id="b3408-179">Find hello available extended events, actions, and targets</span></span>

<span data-ttu-id="b3408-180">Můžete spustit jednoduché SQL **vyberte** tooobtain seznam dostupných událostí hello, akce a cíle.</span><span class="sxs-lookup"><span data-stu-id="b3408-180">You can run a simple SQL **SELECT** tooobtain a list of hello available events, actions, and target.</span></span>

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


<span data-ttu-id="b3408-181"><a name="AzureXEventsTargets" id="AzureXEventsTargets"></a> &nbsp;</span><span class="sxs-lookup"><span data-stu-id="b3408-181"><a name="AzureXEventsTargets" id="AzureXEventsTargets"></a> &nbsp;</span></span>

## <a name="targets-for-your-sql-database-event-sessions"></a><span data-ttu-id="b3408-182">Cíle pro relace události vaší databáze SQL</span><span class="sxs-lookup"><span data-stu-id="b3408-182">Targets for your SQL Database event sessions</span></span>

<span data-ttu-id="b3408-183">Zde jsou cíle, které můžete zaznamenat výsledky z vaší relace události v databázi SQL:</span><span class="sxs-lookup"><span data-stu-id="b3408-183">Here are targets that can capture results from your event sessions on SQL Database:</span></span>

- <span data-ttu-id="b3408-184">[Cílové vyrovnávací paměti prstenec](http://msdn.microsoft.com/library/ff878182.aspx) -stručně uchovává data události v paměti.</span><span class="sxs-lookup"><span data-stu-id="b3408-184">[Ring Buffer target](http://msdn.microsoft.com/library/ff878182.aspx) - Briefly holds event data in memory.</span></span>
- <span data-ttu-id="b3408-185">[Cíl události čítače](http://msdn.microsoft.com/library/ff878025.aspx) – počty všechny události, ke kterým došlo během relace rozšířených událostí.</span><span class="sxs-lookup"><span data-stu-id="b3408-185">[Event Counter target](http://msdn.microsoft.com/library/ff878025.aspx) - Counts all events that occur during an extended events session.</span></span>
- <span data-ttu-id="b3408-186">[Cíl souboru událostí](http://msdn.microsoft.com/library/ff878115.aspx) -zápisy dokončení vyrovnávací paměti tooan Azure Storage kontejneru.</span><span class="sxs-lookup"><span data-stu-id="b3408-186">[Event File target](http://msdn.microsoft.com/library/ff878115.aspx) - Writes complete buffers tooan Azure Storage container.</span></span>

<span data-ttu-id="b3408-187">Hello [trasování událostí pro Windows (ETW)](http://msdn.microsoft.com/library/ms751538.aspx) rozhraní API není k dispozici pro rozšířené události v databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="b3408-187">hello [Event Tracing for Windows (ETW)](http://msdn.microsoft.com/library/ms751538.aspx) API is not available for extended events on SQL Database.</span></span>

## <a name="restrictions"></a><span data-ttu-id="b3408-188">Omezení</span><span class="sxs-lookup"><span data-stu-id="b3408-188">Restrictions</span></span>

<span data-ttu-id="b3408-189">Existuje několik rozdílů související se zabezpečením befitting hello cloudového prostředí databáze SQL:</span><span class="sxs-lookup"><span data-stu-id="b3408-189">There are a couple of security-related differences befitting hello cloud environment of SQL Database:</span></span>

- <span data-ttu-id="b3408-190">Rozšířené události vznikla na hello izolaci klientů jeden model.</span><span class="sxs-lookup"><span data-stu-id="b3408-190">Extended events are founded on hello single-tenant isolation model.</span></span> <span data-ttu-id="b3408-191">Relace události v jedné databáze nelze přistupovat k datům nebo události z jiné databáze.</span><span class="sxs-lookup"><span data-stu-id="b3408-191">An event session in one database cannot access data or events from another database.</span></span>
- <span data-ttu-id="b3408-192">Nemůžou vystavovat **vytvořit událost relace** příkaz v kontextu hello hello **hlavní** databáze.</span><span class="sxs-lookup"><span data-stu-id="b3408-192">You cannot issue a **CREATE EVENT SESSION** statement in hello context of hello **master** database.</span></span>

## <a name="permission-model"></a><span data-ttu-id="b3408-193">Oprávnění modelu</span><span class="sxs-lookup"><span data-stu-id="b3408-193">Permission model</span></span>

<span data-ttu-id="b3408-194">Musíte mít **řízení** oprávnění na hello databáze tooissue **vytvořit událost relace** příkaz.</span><span class="sxs-lookup"><span data-stu-id="b3408-194">You must have **Control** permission on hello database tooissue a **CREATE EVENT SESSION** statement.</span></span> <span data-ttu-id="b3408-195">Hello vlastníka databáze (dbo) má **řízení** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="b3408-195">hello database owner (dbo) has **Control** permission.</span></span>

### <a name="storage-container-authorizations"></a><span data-ttu-id="b3408-196">Povolení kontejneru úložiště</span><span class="sxs-lookup"><span data-stu-id="b3408-196">Storage container authorizations</span></span>

<span data-ttu-id="b3408-197">Hello tokenu SAS můžete vygenerovat pro vaše kontejner úložiště Azure, musíte zadat **rwl** hello oprávnění.</span><span class="sxs-lookup"><span data-stu-id="b3408-197">hello SAS token you generate for your Azure Storage container must specify **rwl** for hello permissions.</span></span> <span data-ttu-id="b3408-198">Hello **rwl** hodnota poskytuje hello následující oprávnění:</span><span class="sxs-lookup"><span data-stu-id="b3408-198">hello **rwl** value provides hello following permissions:</span></span>

- <span data-ttu-id="b3408-199">Čtení</span><span class="sxs-lookup"><span data-stu-id="b3408-199">Read</span></span>
- <span data-ttu-id="b3408-200">Zápis</span><span class="sxs-lookup"><span data-stu-id="b3408-200">Write</span></span>
- <span data-ttu-id="b3408-201">Seznam</span><span class="sxs-lookup"><span data-stu-id="b3408-201">List</span></span>

## <a name="performance-considerations"></a><span data-ttu-id="b3408-202">Otázky výkonu</span><span class="sxs-lookup"><span data-stu-id="b3408-202">Performance considerations</span></span>

<span data-ttu-id="b3408-203">Existují scénáře, kde můžete náročné pomocí rozšířených událostí hromadit aktivnější paměti, než je v pořádku pro hello celého systému.</span><span class="sxs-lookup"><span data-stu-id="b3408-203">There are scenarios where intensive use of extended events can accumulate more active memory than is healthy for hello overall system.</span></span> <span data-ttu-id="b3408-204">Proto hello systému Azure SQL Database dynamicky nastaví a upraví omezení hello množství aktivní paměti, který může být shromážděných řešením relace události.</span><span class="sxs-lookup"><span data-stu-id="b3408-204">Therefore hello Azure SQL Database system dynamically sets and adjusts limits on hello amount of active memory that can be accumulated by an event session.</span></span> <span data-ttu-id="b3408-205">Mnoha faktorech, přejděte do výpočtu dynamické hello.</span><span class="sxs-lookup"><span data-stu-id="b3408-205">Many factors go into hello dynamic calculation.</span></span>

<span data-ttu-id="b3408-206">Pokud se zobrazí chybová zpráva s upozorněním, že byla vynucená maximální paměť, jsou některé opravné akce, které můžete provést:</span><span class="sxs-lookup"><span data-stu-id="b3408-206">If you receive an error message that says a memory maximum was enforced, some corrective actions you can take are:</span></span>

- <span data-ttu-id="b3408-207">Spuštění relace méně souběžných události.</span><span class="sxs-lookup"><span data-stu-id="b3408-207">Run fewer concurrent event sessions.</span></span>
- <span data-ttu-id="b3408-208">Prostřednictvím vaší **vytvořit** a **ALTER** příkazy pro relace události, snížit hello množství paměti, které jste zadali na hello **maximální\_paměti** klauzule.</span><span class="sxs-lookup"><span data-stu-id="b3408-208">Through your **CREATE** and **ALTER** statements for event sessions, reduce hello amount of memory you specify on hello **MAX\_MEMORY** clause.</span></span>

### <a name="network-latency"></a><span data-ttu-id="b3408-209">Latence sítě</span><span class="sxs-lookup"><span data-stu-id="b3408-209">Network latency</span></span>

<span data-ttu-id="b3408-210">Hello **soubor událostí** cíl může zaznamenat latenci sítě nebo selhání při zachování dat tooAzure úložiště objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="b3408-210">hello **Event File** target might experience network latency or failures while persisting data tooAzure Storage blobs.</span></span> <span data-ttu-id="b3408-211">Další události v databázi SQL může zpoždění, které čekají na toocomplete komunikace sítě hello.</span><span class="sxs-lookup"><span data-stu-id="b3408-211">Other events in SQL Database might be delayed while they wait for hello network communication toocomplete.</span></span> <span data-ttu-id="b3408-212">Toto zpoždění může dojít ke snížení velikosti pracovní zátěže.</span><span class="sxs-lookup"><span data-stu-id="b3408-212">This delay can slow your workload.</span></span>

- <span data-ttu-id="b3408-213">toomitigate tento výkon rizik, nenastavujte hello **EVENT_RETENTION_MODE** možnost příliš**NO_EVENT_LOSS** v definic relace události.</span><span class="sxs-lookup"><span data-stu-id="b3408-213">toomitigate this performance risk, avoid setting hello **EVENT_RETENTION_MODE** option too**NO_EVENT_LOSS** in your event session definitions.</span></span>

## <a name="related-links"></a><span data-ttu-id="b3408-214">Související odkazy</span><span class="sxs-lookup"><span data-stu-id="b3408-214">Related links</span></span>

- <span data-ttu-id="b3408-215">[Použití Azure PowerShell s Azure Storage](../storage/common/storage-powershell-guide-full.md).</span><span class="sxs-lookup"><span data-stu-id="b3408-215">[Using Azure PowerShell with Azure Storage](../storage/common/storage-powershell-guide-full.md).</span></span>
- [<span data-ttu-id="b3408-216">Rutiny úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="b3408-216">Azure Storage Cmdlets</span></span>](http://msdn.microsoft.com/library/dn806401.aspx)
- <span data-ttu-id="b3408-217">[Použití Azure PowerShell s Azure Storage](../storage/common/storage-powershell-guide-full.md) – poskytuje podrobné informace o prostředí PowerShell a hello služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="b3408-217">[Using Azure PowerShell with Azure Storage](../storage/common/storage-powershell-guide-full.md) - Provides comprehensive information about PowerShell and hello Azure Storage service.</span></span>
- [<span data-ttu-id="b3408-218">Jak toouse úložiště Blob z rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="b3408-218">How toouse Blob storage from .NET</span></span>](../storage/blobs/storage-dotnet-how-to-use-blobs.md)
- [<span data-ttu-id="b3408-219">CREATE CREDENTIAL (Transact-SQL)</span><span class="sxs-lookup"><span data-stu-id="b3408-219">CREATE CREDENTIAL (Transact-SQL)</span></span>](http://msdn.microsoft.com/library/ms189522.aspx)
- [<span data-ttu-id="b3408-220">Vytvoření relace události (Transact-SQL)</span><span class="sxs-lookup"><span data-stu-id="b3408-220">CREATE EVENT SESSION (Transact-SQL)</span></span>](http://msdn.microsoft.com/library/bb677289.aspx)
- [<span data-ttu-id="b3408-221">Jonathan Kehayias příspěvky o rozšířených událostí v systému Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="b3408-221">Jonathan Kehayias' blog posts about extended events in Microsoft SQL Server</span></span>](http://www.sqlskills.com/blogs/jonathan/category/extended-events/)


- <span data-ttu-id="b3408-222">Hello Azure *aktualizace služby* webovou stránku, zúžit pomocí parametru tooAzure SQL Database:</span><span class="sxs-lookup"><span data-stu-id="b3408-222">hello Azure *Service Updates* webpage, narrowed by parameter tooAzure SQL Database:</span></span>
    - [<span data-ttu-id="b3408-223">https://Azure.microsoft.com/Updates/?Service=SQL-Database</span><span class="sxs-lookup"><span data-stu-id="b3408-223">https://azure.microsoft.com/updates/?service=sql-database</span></span>](https://azure.microsoft.com/updates/?service=sql-database)


<span data-ttu-id="b3408-224">Další témata ukázkový kód pro rozšířené události jsou k dispozici hello následující odkazy.</span><span class="sxs-lookup"><span data-stu-id="b3408-224">Other code sample topics for extended events are available at hello following links.</span></span> <span data-ttu-id="b3408-225">Jestli hello ukázka cílem Microsoft SQL Server a databáze SQL Azure však musíte zkontrolovat pravidelně žádné toosee ukázka.</span><span class="sxs-lookup"><span data-stu-id="b3408-225">However, you must routinely check any sample toosee whether hello sample targets Microsoft SQL Server versus Azure SQL Database.</span></span> <span data-ttu-id="b3408-226">Potom můžete rozhodnout, zda jsou mírně potřebné toorun hello ukázka.</span><span class="sxs-lookup"><span data-stu-id="b3408-226">Then you can decide whether minor changes are needed toorun hello sample.</span></span>

<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find hello Objects That Have hello Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
