---
title: "aaaXEvent kód cyklické vyrovnávací paměti pro databázi SQL. | Microsoft Docs"
description: "Poskytuje ukázky kódu jazyka Transact-SQL, která přišla rychlé a snadné použití cíl hello cyklické vyrovnávací paměti, v databázi SQL Azure."
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
ms.openlocfilehash: 21df748d9999d6837d2b5bbe4a3f47fb351b4633
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="ring-buffer-target-code-for-extended-events-in-sql-database"></a><span data-ttu-id="95157-103">Prstence kódu cílové vyrovnávací paměti pro rozšířené události v databázi SQL</span><span class="sxs-lookup"><span data-stu-id="95157-103">Ring Buffer target code for extended events in SQL Database</span></span>

[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

<span data-ttu-id="95157-104">Chcete úplného příkladu kódu hello nejjednodušší rychlý způsob toocapture a sestava informace pro rozšířené události během testu.</span><span class="sxs-lookup"><span data-stu-id="95157-104">You want a complete code sample for hello easiest quick way toocapture and report information for an extended event during a test.</span></span> <span data-ttu-id="95157-105">Hello nejjednodušší cíl pro data rozšířených událostí je hello [cíl cyklické vyrovnávací paměti](http://msdn.microsoft.com/library/ff878182.aspx).</span><span class="sxs-lookup"><span data-stu-id="95157-105">hello easiest target for extended event data is hello [Ring Buffer target](http://msdn.microsoft.com/library/ff878182.aspx).</span></span>

<span data-ttu-id="95157-106">Toto téma představuje ukázku kódu jazyka Transact-SQL, který:</span><span class="sxs-lookup"><span data-stu-id="95157-106">This topic presents a Transact-SQL code sample that:</span></span>

1. <span data-ttu-id="95157-107">Vytvoří tabulku se toodemonstrate data s.</span><span class="sxs-lookup"><span data-stu-id="95157-107">Creates a table with data toodemonstrate with.</span></span>
2. <span data-ttu-id="95157-108">Vytvoří relaci pro existující událost rozšířené, a to **sqlserver.sql_statement_starting**.</span><span class="sxs-lookup"><span data-stu-id="95157-108">Creates a session for an existing extended event, namely **sqlserver.sql_statement_starting**.</span></span>
   
   * <span data-ttu-id="95157-109">Hello událostí je omezená tooSQL příkazů, které obsahují určitý řetězec aktualizace: **příkaz jako '% aktualizace tabEmployee %'**.</span><span class="sxs-lookup"><span data-stu-id="95157-109">hello event is limited tooSQL statements that contain a particular Update string: **statement LIKE '%UPDATE tabEmployee%'**.</span></span>
   * <span data-ttu-id="95157-110">Vybere toosend hello výstup hello cíl tooa události typu cyklické vyrovnávací paměti, a to **package0.ring_buffer**.</span><span class="sxs-lookup"><span data-stu-id="95157-110">Chooses toosend hello output of hello event tooa target of type Ring Buffer, namely  **package0.ring_buffer**.</span></span>
3. <span data-ttu-id="95157-111">Spustí relaci události hello.</span><span class="sxs-lookup"><span data-stu-id="95157-111">Starts hello event session.</span></span>
4. <span data-ttu-id="95157-112">Vydá několik jednoduchých příkazy aktualizace SQL.</span><span class="sxs-lookup"><span data-stu-id="95157-112">Issues a couple of simple SQL UPDATE statements.</span></span>
5. <span data-ttu-id="95157-113">Problémy SQL vyberte příkaz tooretrieve událostí výstup hello cyklické vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="95157-113">Issues a SQL SELECT statement tooretrieve event output from hello Ring Buffer.</span></span>
   
   * <span data-ttu-id="95157-114">**Sys.dm_xe_database_session_targets** a jsou připojené ostatní zobrazení dynamické správy (zobrazení dynamické správy).</span><span class="sxs-lookup"><span data-stu-id="95157-114">**sys.dm_xe_database_session_targets** and other dynamic management views (DMVs) are joined.</span></span>
6. <span data-ttu-id="95157-115">Zastaví hello relace události.</span><span class="sxs-lookup"><span data-stu-id="95157-115">Stops hello event session.</span></span>
7. <span data-ttu-id="95157-116">Vyřazuje hello cíl cyklické vyrovnávací paměti, toorelease její prostředky.</span><span class="sxs-lookup"><span data-stu-id="95157-116">Drops hello Ring Buffer target, toorelease its resources.</span></span>
8. <span data-ttu-id="95157-117">Zahodí relace události hello a hello ukázkové tabulky.</span><span class="sxs-lookup"><span data-stu-id="95157-117">Drops hello event session and hello demo table.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="95157-118">Požadavky</span><span class="sxs-lookup"><span data-stu-id="95157-118">Prerequisites</span></span>

* <span data-ttu-id="95157-119">Účet a předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="95157-119">An Azure account and subscription.</span></span> <span data-ttu-id="95157-120">Můžete si zaregistrovat i [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="95157-120">You can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="95157-121">Všechny databáze, které je možné vytvořit tabulku v.</span><span class="sxs-lookup"><span data-stu-id="95157-121">Any database you can create a table in.</span></span>
  
  * <span data-ttu-id="95157-122">Volitelně můžete [vytvořit **AdventureWorksLT** ukázkovou databázi](sql-database-get-started.md) v minutách.</span><span class="sxs-lookup"><span data-stu-id="95157-122">Optionally you can [create an **AdventureWorksLT** demonstration database](sql-database-get-started.md) in minutes.</span></span>
* <span data-ttu-id="95157-123">SQL Server Management Studio (ssms.exe), v ideálním případě její nejnovější měsíční aktualizace verzi.</span><span class="sxs-lookup"><span data-stu-id="95157-123">SQL Server Management Studio (ssms.exe), ideally its latest monthly update version.</span></span> 
  <span data-ttu-id="95157-124">Si můžete stáhnout nejnovější ssms.exe hello z:</span><span class="sxs-lookup"><span data-stu-id="95157-124">You can download hello latest ssms.exe from:</span></span>
  
  * <span data-ttu-id="95157-125">Téma s názvem [stáhnout SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="95157-125">Topic titled [Download SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span></span>
  * [<span data-ttu-id="95157-126">Stahování toohello přímý odkaz.</span><span class="sxs-lookup"><span data-stu-id="95157-126">A direct link toohello download.</span></span>](http://go.microsoft.com/fwlink/?linkid=616025)

## <a name="code-sample"></a><span data-ttu-id="95157-127">Ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="95157-127">Code sample</span></span>

<span data-ttu-id="95157-128">S velmi malé změny hello následující ukázka kódu cyklické vyrovnávací paměti lze spustit v Azure SQL Database nebo Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="95157-128">With very minor modification, hello following Ring Buffer code sample can be run on either Azure SQL Database or Microsoft SQL Server.</span></span> <span data-ttu-id="95157-129">Hello rozdíl je hello přítomnosti hello uzlu '_databáze"v názvu hello některé zobrazení dynamické správy (zobrazení dynamické správy), použít v klauzuli FROM hello v kroku 5.</span><span class="sxs-lookup"><span data-stu-id="95157-129">hello difference is hello presence of hello node '_database' in hello name of some dynamic management views (DMVs), used in hello FROM clause in Step 5.</span></span> <span data-ttu-id="95157-130">Například:</span><span class="sxs-lookup"><span data-stu-id="95157-130">For example:</span></span>

* <span data-ttu-id="95157-131">Sys.dm_xe**_databáze**_session_targets</span><span class="sxs-lookup"><span data-stu-id="95157-131">sys.dm_xe**_database**_session_targets</span></span>
* <span data-ttu-id="95157-132">Sys.dm_xe_session_targets</span><span class="sxs-lookup"><span data-stu-id="95157-132">sys.dm_xe_session_targets</span></span>

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

## <a name="ring-buffer-contents"></a><span data-ttu-id="95157-133">Obsah prstenec vyrovnávací paměti</span><span class="sxs-lookup"><span data-stu-id="95157-133">Ring Buffer contents</span></span>

<span data-ttu-id="95157-134">Použili jsme ukázka kódu hello toorun ssms.exe.</span><span class="sxs-lookup"><span data-stu-id="95157-134">We used ssms.exe toorun hello code sample.</span></span>

<span data-ttu-id="95157-135">výsledky hello tooview, jsme klikli hello buňky v záhlaví sloupce hello **target_data_XML**.</span><span class="sxs-lookup"><span data-stu-id="95157-135">tooview hello results, we clicked hello cell under hello column header **target_data_XML**.</span></span>

<span data-ttu-id="95157-136">Potom v podokně výsledků hello jsme klikli hello buňky v záhlaví sloupce hello **target_data_XML**.</span><span class="sxs-lookup"><span data-stu-id="95157-136">Then in hello results pane we clicked hello cell under hello column header **target_data_XML**.</span></span> <span data-ttu-id="95157-137">Klepněte na tlačítko vytvořit jinou kartu Soubor v ssms.exe v které hello zobrazila obsah buňky hello výsledek ve formátu XML.</span><span class="sxs-lookup"><span data-stu-id="95157-137">This click created another file tab in ssms.exe in which hello content of hello result cell was displayed, as XML.</span></span>

<span data-ttu-id="95157-138">výstup Hello se zobrazí v následující bloku hello.</span><span class="sxs-lookup"><span data-stu-id="95157-138">hello output is shown in hello following block.</span></span> <span data-ttu-id="95157-139">Vypadá tak dlouho, ale je právě dva  **<event>**  elementy.</span><span class="sxs-lookup"><span data-stu-id="95157-139">It looks long, but it is just two **<event>** elements.</span></span>

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


#### <a name="release-resources-held-by-your-ring-buffer"></a><span data-ttu-id="95157-140">Uvolnit prostředky držené vaší cyklické vyrovnávací paměti</span><span class="sxs-lookup"><span data-stu-id="95157-140">Release resources held by your Ring Buffer</span></span>

<span data-ttu-id="95157-141">Až skončíte s vaší cyklické vyrovnávací paměti, můžete ho odebrat a její prostředky vydání verze **ALTER** jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="95157-141">When you are done with your Ring Buffer, you can remove it and release its resources issuing an **ALTER** like hello following:</span></span>

```sql
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO
```


<span data-ttu-id="95157-142">definice Hello relaci události je aktualizován, ale není vyřazen.</span><span class="sxs-lookup"><span data-stu-id="95157-142">hello definition of your event session is updated, but not dropped.</span></span> <span data-ttu-id="95157-143">Později můžete přidat další instanci relace události tooyour hello cyklické vyrovnávací paměti:</span><span class="sxs-lookup"><span data-stu-id="95157-143">Later you can add another instance of hello Ring Buffer tooyour event session:</span></span>

```sql
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
```


## <a name="more-information"></a><span data-ttu-id="95157-144">Další informace</span><span class="sxs-lookup"><span data-stu-id="95157-144">More information</span></span>

<span data-ttu-id="95157-145">Hello primární téma pro rozšířené události v databázi SQL Azure je:</span><span class="sxs-lookup"><span data-stu-id="95157-145">hello primary topic for extended events on Azure SQL Database is:</span></span>

* <span data-ttu-id="95157-146">[Rozšířené aspekty událost v databázi SQL](sql-database-xevent-db-diff-from-svr.md), který se liší od některých aspektů rozšířené události, které se liší od Azure SQL Database a serveru Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="95157-146">[Extended event considerations in SQL Database](sql-database-xevent-db-diff-from-svr.md), which contrasts some aspects of extended events that differ between Azure SQL Database versus Microsoft SQL Server.</span></span>

<span data-ttu-id="95157-147">Další témata ukázkový kód pro rozšířené události jsou k dispozici hello následující odkazy.</span><span class="sxs-lookup"><span data-stu-id="95157-147">Other code sample topics for extended events are available at hello following links.</span></span> <span data-ttu-id="95157-148">Jestli hello ukázka cílem Microsoft SQL Server a databáze SQL Azure však musíte zkontrolovat pravidelně žádné toosee ukázka.</span><span class="sxs-lookup"><span data-stu-id="95157-148">However, you must routinely check any sample toosee whether hello sample targets Microsoft SQL Server versus Azure SQL Database.</span></span> <span data-ttu-id="95157-149">Potom můžete rozhodnout, zda jsou mírně potřebné toorun hello ukázka.</span><span class="sxs-lookup"><span data-stu-id="95157-149">Then you can decide whether minor changes are needed toorun hello sample.</span></span>

* <span data-ttu-id="95157-150">Ukázka kódu pro Azure SQL Database: [kód cílový soubor událostí pro rozšířené události v databázi SQL](sql-database-xevent-code-event-file.md)</span><span class="sxs-lookup"><span data-stu-id="95157-150">Code sample for Azure SQL Database: [Event File target code for extended events in SQL Database](sql-database-xevent-code-event-file.md)</span></span>

<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find hello Objects That Have hello Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
