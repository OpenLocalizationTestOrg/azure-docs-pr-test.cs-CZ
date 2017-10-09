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
# <a name="ring-buffer-target-code-for-extended-events-in-sql-database"></a>Prstence kódu cílové vyrovnávací paměti pro rozšířené události v databázi SQL

[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Chcete úplného příkladu kódu hello nejjednodušší rychlý způsob toocapture a sestava informace pro rozšířené události během testu. Hello nejjednodušší cíl pro data rozšířených událostí je hello [cíl cyklické vyrovnávací paměti](http://msdn.microsoft.com/library/ff878182.aspx).

Toto téma představuje ukázku kódu jazyka Transact-SQL, který:

1. Vytvoří tabulku se toodemonstrate data s.
2. Vytvoří relaci pro existující událost rozšířené, a to **sqlserver.sql_statement_starting**.
   
   * Hello událostí je omezená tooSQL příkazů, které obsahují určitý řetězec aktualizace: **příkaz jako '% aktualizace tabEmployee %'**.
   * Vybere toosend hello výstup hello cíl tooa události typu cyklické vyrovnávací paměti, a to **package0.ring_buffer**.
3. Spustí relaci události hello.
4. Vydá několik jednoduchých příkazy aktualizace SQL.
5. Problémy SQL vyberte příkaz tooretrieve událostí výstup hello cyklické vyrovnávací paměti.
   
   * **Sys.dm_xe_database_session_targets** a jsou připojené ostatní zobrazení dynamické správy (zobrazení dynamické správy).
6. Zastaví hello relace události.
7. Vyřazuje hello cíl cyklické vyrovnávací paměti, toorelease její prostředky.
8. Zahodí relace události hello a hello ukázkové tabulky.

## <a name="prerequisites"></a>Požadavky

* Účet a předplatné Azure. Můžete si zaregistrovat i [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
* Všechny databáze, které je možné vytvořit tabulku v.
  
  * Volitelně můžete [vytvořit **AdventureWorksLT** ukázkovou databázi](sql-database-get-started.md) v minutách.
* SQL Server Management Studio (ssms.exe), v ideálním případě její nejnovější měsíční aktualizace verzi. 
  Si můžete stáhnout nejnovější ssms.exe hello z:
  
  * Téma s názvem [stáhnout SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).
  * [Stahování toohello přímý odkaz.](http://go.microsoft.com/fwlink/?linkid=616025)

## <a name="code-sample"></a>Ukázka kódu

S velmi malé změny hello následující ukázka kódu cyklické vyrovnávací paměti lze spustit v Azure SQL Database nebo Microsoft SQL Server. Hello rozdíl je hello přítomnosti hello uzlu '_databáze"v názvu hello některé zobrazení dynamické správy (zobrazení dynamické správy), použít v klauzuli FROM hello v kroku 5. Například:

* Sys.dm_xe**_databáze**_session_targets
* Sys.dm_xe_session_targets

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

## <a name="ring-buffer-contents"></a>Obsah prstenec vyrovnávací paměti

Použili jsme ukázka kódu hello toorun ssms.exe.

výsledky hello tooview, jsme klikli hello buňky v záhlaví sloupce hello **target_data_XML**.

Potom v podokně výsledků hello jsme klikli hello buňky v záhlaví sloupce hello **target_data_XML**. Klepněte na tlačítko vytvořit jinou kartu Soubor v ssms.exe v které hello zobrazila obsah buňky hello výsledek ve formátu XML.

výstup Hello se zobrazí v následující bloku hello. Vypadá tak dlouho, ale je právě dva  **<event>**  elementy.

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


#### <a name="release-resources-held-by-your-ring-buffer"></a>Uvolnit prostředky držené vaší cyklické vyrovnávací paměti

Až skončíte s vaší cyklické vyrovnávací paměti, můžete ho odebrat a její prostředky vydání verze **ALTER** jako hello následující:

```sql
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO
```


definice Hello relaci události je aktualizován, ale není vyřazen. Později můžete přidat další instanci relace události tooyour hello cyklické vyrovnávací paměti:

```sql
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
```


## <a name="more-information"></a>Další informace

Hello primární téma pro rozšířené události v databázi SQL Azure je:

* [Rozšířené aspekty událost v databázi SQL](sql-database-xevent-db-diff-from-svr.md), který se liší od některých aspektů rozšířené události, které se liší od Azure SQL Database a serveru Microsoft SQL Server.

Další témata ukázkový kód pro rozšířené události jsou k dispozici hello následující odkazy. Jestli hello ukázka cílem Microsoft SQL Server a databáze SQL Azure však musíte zkontrolovat pravidelně žádné toosee ukázka. Potom můžete rozhodnout, zda jsou mírně potřebné toorun hello ukázka.

* Ukázka kódu pro Azure SQL Database: [kód cílový soubor událostí pro rozšířené události v databázi SQL](sql-database-xevent-code-event-file.md)

<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find hello Objects That Have hello Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
