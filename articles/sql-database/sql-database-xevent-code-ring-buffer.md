---
title: "Kód XEvent cyklické vyrovnávací paměti pro databázi SQL. | Microsoft Docs"
description: "Poskytuje ukázky kódu jazyka Transact-SQL, která přišla rychlé a snadné použití cíl cyklické vyrovnávací paměti, v databázi SQL Azure."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: 
tags: 
ms.assetid: 2510fb3f-c8f2-437a-8f49-9d5f6c96e75b
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: genemi
ms.openlocfilehash: 6fbefe151901ac3b15d93712422878fc4d6206f1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="ring-buffer-target-code-for-extended-events-in-sql-database"></a><span data-ttu-id="ccccd-103">Prstence kódu cílové vyrovnávací paměti pro rozšířené události v databázi SQL</span><span class="sxs-lookup"><span data-stu-id="ccccd-103">Ring Buffer target code for extended events in SQL Database</span></span>

[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

<span data-ttu-id="ccccd-104">Chcete úplného příkladu kódu pro nejjednodušší rychlý způsob, jak informace o zachycení a sestav pro rozšířené události během testu.</span><span class="sxs-lookup"><span data-stu-id="ccccd-104">You want a complete code sample for the easiest quick way to capture and report information for an extended event during a test.</span></span> <span data-ttu-id="ccccd-105">Je nejjednodušší cíl pro rozšířené události data [cíl cyklické vyrovnávací paměti](http://msdn.microsoft.com/library/ff878182.aspx).</span><span class="sxs-lookup"><span data-stu-id="ccccd-105">The easiest target for extended event data is the [Ring Buffer target](http://msdn.microsoft.com/library/ff878182.aspx).</span></span>

<span data-ttu-id="ccccd-106">Toto téma představuje ukázku kódu jazyka Transact-SQL, který:</span><span class="sxs-lookup"><span data-stu-id="ccccd-106">This topic presents a Transact-SQL code sample that:</span></span>

1. <span data-ttu-id="ccccd-107">Vytvoří tabulku s daty k předvedení s.</span><span class="sxs-lookup"><span data-stu-id="ccccd-107">Creates a table with data to demonstrate with.</span></span>
2. <span data-ttu-id="ccccd-108">Vytvoří relaci pro existující událost rozšířené, a to **sqlserver.sql_statement_starting**.</span><span class="sxs-lookup"><span data-stu-id="ccccd-108">Creates a session for an existing extended event, namely **sqlserver.sql_statement_starting**.</span></span>
   
   * <span data-ttu-id="ccccd-109">Událost je omezený na příkazy SQL, které obsahují určitý řetězec aktualizace: **příkaz jako '% aktualizace tabEmployee %'**.</span><span class="sxs-lookup"><span data-stu-id="ccccd-109">The event is limited to SQL statements that contain a particular Update string: **statement LIKE '%UPDATE tabEmployee%'**.</span></span>
   * <span data-ttu-id="ccccd-110">Vybere možnost odesílat výstup události k cíli typu cyklické vyrovnávací paměti, a to **package0.ring_buffer**.</span><span class="sxs-lookup"><span data-stu-id="ccccd-110">Chooses to send the output of the event to a target of type Ring Buffer, namely  **package0.ring_buffer**.</span></span>
3. <span data-ttu-id="ccccd-111">Spustí relaci události.</span><span class="sxs-lookup"><span data-stu-id="ccccd-111">Starts the event session.</span></span>
4. <span data-ttu-id="ccccd-112">Vydá několik jednoduchých příkazy aktualizace SQL.</span><span class="sxs-lookup"><span data-stu-id="ccccd-112">Issues a couple of simple SQL UPDATE statements.</span></span>
5. <span data-ttu-id="ccccd-113">Vydá příkazu SQL SELECT načíst výstup událostí z cyklické vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="ccccd-113">Issues a SQL SELECT statement to retrieve event output from the Ring Buffer.</span></span>
   
   * <span data-ttu-id="ccccd-114">**Sys.dm_xe_database_session_targets** a jsou připojené ostatní zobrazení dynamické správy (zobrazení dynamické správy).</span><span class="sxs-lookup"><span data-stu-id="ccccd-114">**sys.dm_xe_database_session_targets** and other dynamic management views (DMVs) are joined.</span></span>
6. <span data-ttu-id="ccccd-115">Zastaví relace události.</span><span class="sxs-lookup"><span data-stu-id="ccccd-115">Stops the event session.</span></span>
7. <span data-ttu-id="ccccd-116">Zahodí cíl cyklické vyrovnávací paměti, chcete-li uvolnit její prostředky.</span><span class="sxs-lookup"><span data-stu-id="ccccd-116">Drops the Ring Buffer target, to release its resources.</span></span>
8. <span data-ttu-id="ccccd-117">Zahodí ukázkové tabulky a relace události.</span><span class="sxs-lookup"><span data-stu-id="ccccd-117">Drops the event session and the demo table.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ccccd-118">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ccccd-118">Prerequisites</span></span>

* <span data-ttu-id="ccccd-119">Účet a předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="ccccd-119">An Azure account and subscription.</span></span> <span data-ttu-id="ccccd-120">Můžete si zaregistrovat i [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ccccd-120">You can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="ccccd-121">Všechny databáze, které je možné vytvořit tabulku v.</span><span class="sxs-lookup"><span data-stu-id="ccccd-121">Any database you can create a table in.</span></span>
  
  * <span data-ttu-id="ccccd-122">Volitelně můžete [vytvořit **AdventureWorksLT** ukázkovou databázi](sql-database-get-started.md) v minutách.</span><span class="sxs-lookup"><span data-stu-id="ccccd-122">Optionally you can [create an **AdventureWorksLT** demonstration database](sql-database-get-started.md) in minutes.</span></span>
* <span data-ttu-id="ccccd-123">SQL Server Management Studio (ssms.exe), v ideálním případě její nejnovější měsíční aktualizace verzi.</span><span class="sxs-lookup"><span data-stu-id="ccccd-123">SQL Server Management Studio (ssms.exe), ideally its latest monthly update version.</span></span> 
  <span data-ttu-id="ccccd-124">Si můžete stáhnout nejnovější ssms.exe z:</span><span class="sxs-lookup"><span data-stu-id="ccccd-124">You can download the latest ssms.exe from:</span></span>
  
  * <span data-ttu-id="ccccd-125">Téma s názvem [stáhnout SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="ccccd-125">Topic titled [Download SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span></span>
  * [<span data-ttu-id="ccccd-126">Přímý odkaz na stažení.</span><span class="sxs-lookup"><span data-stu-id="ccccd-126">A direct link to the download.</span></span>](http://go.microsoft.com/fwlink/?linkid=616025)

## <a name="code-sample"></a><span data-ttu-id="ccccd-127">Ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="ccccd-127">Code sample</span></span>

<span data-ttu-id="ccccd-128">S velmi malé změny můžete spustit následující ukázka kódu cyklické vyrovnávací paměti na Azure SQL Database nebo Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ccccd-128">With very minor modification, the following Ring Buffer code sample can be run on either Azure SQL Database or Microsoft SQL Server.</span></span> <span data-ttu-id="ccccd-129">Rozdíl je přítomnost uzlu '_databáze' názvu některá zobrazení dynamické správy (zobrazení dynamické správy), použít v klauzuli FROM v kroku 5.</span><span class="sxs-lookup"><span data-stu-id="ccccd-129">The difference is the presence of the node '_database' in the name of some dynamic management views (DMVs), used in the FROM clause in Step 5.</span></span> <span data-ttu-id="ccccd-130">Například:</span><span class="sxs-lookup"><span data-stu-id="ccccd-130">For example:</span></span>

* <span data-ttu-id="ccccd-131">Sys.dm_xe**_databáze**_session_targets</span><span class="sxs-lookup"><span data-stu-id="ccccd-131">sys.dm_xe**_database**_session_targets</span></span>
* <span data-ttu-id="ccccd-132">Sys.dm_xe_session_targets</span><span class="sxs-lookup"><span data-stu-id="ccccd-132">sys.dm_xe_session_targets</span></span>

&nbsp;

```sql
GO
----  Transact-SQL.
---- Step set 1.

SET NOCOUNT ON;
GO


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'tabEmployee')
BEGIN
    DROP TABLE tabEmployee;
END
GO


CREATE TABLE tabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO tabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO

---- Step set 2.


IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'eventsession_gm_azuresqldb51')
BEGIN
    DROP EVENT SESSION eventsession_gm_azuresqldb51
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE '%UPDATE tabEmployee%'
            )
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
GO

---- Step set 3.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = START;
GO

---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
GO

---- Step set 5.


SELECT
    se.name                      AS [session-name],
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source,
    st.target_data,
    CAST(st.target_data AS XML)  AS [target_data_XML]
FROM
               sys.dm_xe_database_session_event_actions  AS ac

    INNER JOIN sys.dm_xe_database_session_events         AS ev  ON ev.event_name = ac.event_name
        AND CAST(ev.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_object_columns AS oc
         ON CAST(oc.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_targets        AS st
         ON CAST(st.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_sessions               AS se
         ON CAST(ac.event_session_address AS BINARY(8)) = CAST(se.address AS BINARY(8))
WHERE
        oc.column_name = 'occurrence_number'
    AND
        se.name        = 'eventsession_gm_azuresqldb51'
    AND
        ac.action_name = 'sql_text'
ORDER BY
    se.name,
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source
;
GO

---- Step set 6.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = STOP;
GO

---- Step set 7.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO

---- Step set 8.


DROP EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE;
GO

DROP TABLE tabEmployee;
GO
```


&nbsp;

## <a name="ring-buffer-contents"></a><span data-ttu-id="ccccd-133">Obsah prstenec vyrovnávací paměti</span><span class="sxs-lookup"><span data-stu-id="ccccd-133">Ring Buffer contents</span></span>

<span data-ttu-id="ccccd-134">Použili jsme ssms.exe spustit ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="ccccd-134">We used ssms.exe to run the code sample.</span></span>

<span data-ttu-id="ccccd-135">Pokud chcete zobrazit výsledky, jsme klikli buňky v záhlaví sloupce **target_data_XML**.</span><span class="sxs-lookup"><span data-stu-id="ccccd-135">To view the results, we clicked the cell under the column header **target_data_XML**.</span></span>

<span data-ttu-id="ccccd-136">Potom v podokně výsledků jsme klikli buňky v záhlaví sloupce **target_data_XML**.</span><span class="sxs-lookup"><span data-stu-id="ccccd-136">Then in the results pane we clicked the cell under the column header **target_data_XML**.</span></span> <span data-ttu-id="ccccd-137">Klepněte na tlačítko vytvořit jinou kartu Soubor v ssms.exe ve kterém byl obsah buňky výsledek zobrazí, jako XML.</span><span class="sxs-lookup"><span data-stu-id="ccccd-137">This click created another file tab in ssms.exe in which the content of the result cell was displayed, as XML.</span></span>

<span data-ttu-id="ccccd-138">Výstup se zobrazuje v následující bloku.</span><span class="sxs-lookup"><span data-stu-id="ccccd-138">The output is shown in the following block.</span></span> <span data-ttu-id="ccccd-139">Vypadá tak dlouho, ale je právě dva  **<event>**  elementy.</span><span class="sxs-lookup"><span data-stu-id="ccccd-139">It looks long, but it is just two **<event>** elements.</span></span>

&nbsp;

```
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="1728">
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.317Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>7</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>184</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>328</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.327Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>10</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>340</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>486</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
</RingBufferTarget>
```


#### <a name="release-resources-held-by-your-ring-buffer"></a><span data-ttu-id="ccccd-140">Uvolnit prostředky držené vaší cyklické vyrovnávací paměti</span><span class="sxs-lookup"><span data-stu-id="ccccd-140">Release resources held by your Ring Buffer</span></span>

<span data-ttu-id="ccccd-141">Až skončíte s vaší cyklické vyrovnávací paměti, můžete ho odebrat a její prostředky vydání verze **ALTER** podobně jako následující:</span><span class="sxs-lookup"><span data-stu-id="ccccd-141">When you are done with your Ring Buffer, you can remove it and release its resources issuing an **ALTER** like the following:</span></span>

```sql
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO
```


<span data-ttu-id="ccccd-142">Definice relaci události je aktualizován, ale není vyřazen.</span><span class="sxs-lookup"><span data-stu-id="ccccd-142">The definition of your event session is updated, but not dropped.</span></span> <span data-ttu-id="ccccd-143">Později můžete přidat další instanci cyklické vyrovnávací paměti do relace události:</span><span class="sxs-lookup"><span data-stu-id="ccccd-143">Later you can add another instance of the Ring Buffer to your event session:</span></span>

```sql
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
```


## <a name="more-information"></a><span data-ttu-id="ccccd-144">Další informace</span><span class="sxs-lookup"><span data-stu-id="ccccd-144">More information</span></span>

<span data-ttu-id="ccccd-145">Primární téma pro rozšířené události v databázi SQL Azure je:</span><span class="sxs-lookup"><span data-stu-id="ccccd-145">The primary topic for extended events on Azure SQL Database is:</span></span>

* <span data-ttu-id="ccccd-146">[Rozšířené aspekty událost v databázi SQL](sql-database-xevent-db-diff-from-svr.md), který se liší od některých aspektů rozšířené události, které se liší od Azure SQL Database a serveru Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ccccd-146">[Extended event considerations in SQL Database](sql-database-xevent-db-diff-from-svr.md), which contrasts some aspects of extended events that differ between Azure SQL Database versus Microsoft SQL Server.</span></span>

<span data-ttu-id="ccccd-147">Další témata ukázkový kód pro rozšířené události jsou dostupné prostřednictvím následujících odkazů.</span><span class="sxs-lookup"><span data-stu-id="ccccd-147">Other code sample topics for extended events are available at the following links.</span></span> <span data-ttu-id="ccccd-148">Ale je nutné pravidelně zkontrolovat všechny ukázkové zobrazíte zda ukázku cílem Microsoft SQL Server a databáze SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="ccccd-148">However, you must routinely check any sample to see whether the sample targets Microsoft SQL Server versus Azure SQL Database.</span></span> <span data-ttu-id="ccccd-149">Potom můžete rozhodnout, zda jsou mírně potřebné ke spuštění ukázky.</span><span class="sxs-lookup"><span data-stu-id="ccccd-149">Then you can decide whether minor changes are needed to run the sample.</span></span>

* <span data-ttu-id="ccccd-150">Ukázka kódu pro Azure SQL Database: [kód cílový soubor událostí pro rozšířené události v databázi SQL](sql-database-xevent-code-event-file.md)</span><span class="sxs-lookup"><span data-stu-id="ccccd-150">Code sample for Azure SQL Database: [Event File target code for extended events in SQL Database](sql-database-xevent-code-event-file.md)</span></span>

<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
