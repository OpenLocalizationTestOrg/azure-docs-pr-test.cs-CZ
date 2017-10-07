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
# <a name="extended-events-in-sql-database"></a>Rozšířené události v databázi SQL
[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Toto téma vysvětluje, jak hello implementace rozšířených událostí ve službě Azure SQL Database je mírně odlišný porovnání tooextended události v systému Microsoft SQL Server.

- Databáze SQL verze 12 získávají hello funkce Rozšířené události v hello druhou polovinu kalendáře 2015.
- SQL Server došlo rozšířené události od 2008.
- Sada funkcí Hello rozšířených událostí na SQL Database je robustní podmnožinu funkcí hello na serveru SQL Server.

*Události Xevent* je neformální přezdívka, která se někdy používá pro parametr 'rozšířené události: v blogy a jiných neformální umístění.

Další informace o rozšířených událostí, Azure SQL Database a Microsoft SQL Server, je k dispozici na:

- [Rychlé zahájení: Rozšířených událostí v systému SQL Server](http://msdn.microsoft.com/library/mt733217.aspx)
- [Rozšířené události](http://msdn.microsoft.com/library/bb630282.aspx)

## <a name="prerequisites"></a>Požadavky

Toto téma předpokládá, že již máte některé znalosti:

- [Služba Azure SQL Database](https://azure.microsoft.com/services/sql-database/).
- [Rozšířené události](http://msdn.microsoft.com/library/bb630282.aspx) v systému Microsoft SQL Server.

- hromadné Hello část naší dokumentace o rozšířených událostí platí tooboth systému SQL Server a SQL Database.

Předchozí ohrožení toohello následující položky je užitečné, pokud vyberete hello soubor událostí jako hello [cíl](#AzureXEventsTargets):

- [Služba Azure Storage](https://azure.microsoft.com/services/storage/)


- PowerShell
    - [Použití Azure PowerShell s Azure Storage](../storage/common/storage-powershell-guide-full.md) – poskytuje podrobné informace o prostředí PowerShell a hello služby Azure Storage.

## <a name="code-samples"></a>Ukázky kódů

Související témata obsahují dva ukázky kódu:


- [Prstence kódu cílové vyrovnávací paměti pro rozšířené události v databázi SQL](sql-database-xevent-code-ring-buffer.md)
    - Krátký jednoduchý skript Transact-SQL.
    - Jsme zdůraznil v tématu ukázkový kód hello je, že až skončíte s cílem cyklické vyrovnávací paměti, měli uvolnění jeho prostředků spuštěním alter myší `ALTER EVENT SESSION ... ON DATABASE DROP TARGET ...;` příkaz. Později můžete přidat další instanci cyklické vyrovnávací paměti podle `ALTER EVENT SESSION ... ON DATABASE ADD TARGET ...`.


- [Kód cílový soubor události pro rozšířené události v databázi SQL](sql-database-xevent-code-event-file.md)
    - Fáze 1 je prostředí PowerShell toocreate kontejner Azure Storage.
    - Fáze 2 je Transact-SQL, používající hello kontejner úložiště Azure.

## <a name="transact-sql-differences"></a>Rozdíly v Transact-SQL


- Při spuštění hello [vytvořit událost relace](http://msdn.microsoft.com/library/bb677289.aspx) příkaz na serveru SQL Server, použijte hello **ON SERVER** klauzule. Ale v databázi SQL používat hello **ON databáze** klauzule místo.


- Hello **ON databáze** klauzule platí také toohello [ALTER událostí relace](http://msdn.microsoft.com/library/bb630368.aspx) a [VYŘADIT událostí relace](http://msdn.microsoft.com/library/bb630257.aspx) příkazy jazyka Transact-SQL.


- Osvědčeným postupem je možnost relace události hello tooinclude z **STARTUP_STATE = ON** ve vaší **vytvořit událost relace** nebo **ALTER událostí relace** příkazy.
    - Hello **= ON** hodnotu podporuje automatické restartování po Rekonfigurace hello logické databáze z důvodu tooa převzetí služeb při selhání.

## <a name="new-catalog-views"></a>Nová zobrazení katalogu

Hello rozšířených událostí je funkce podporována několik [katalogu zobrazení](http://msdn.microsoft.com/library/ms174365.aspx). Zobrazení katalogu říct o *metadata nebo definice* relací vytvořené uživatelem událostí v aktuální databázi hello. zobrazení Hello nevrátí informace o instancích relace aktivní události.

| Název<br/>zobrazení katalogu | Popis |
|:--- |:--- |
| **Sys.database_event_session_actions** |Vrátí řádek pro každou akci v každé události relace události. |
| **Sys.database_event_session_events** |Vrátí řádek pro každou jednotlivou událost v relaci události. |
| **Sys.database_event_session_fields** |Vrátí řádek pro každý možné přizpůsobit sloupec, který byl explicitně nastavit na události a cíle. |
| **Sys.database_event_session_targets** |Vrátí řádek pro každý cíl události pro relace události. |
| **Sys.database_event_sessions** |Vrátí řádek pro každou relaci události v databázi SQL Database hello. |

V systému Microsoft SQL Server, podobně jako zobrazení katalogu mít názvy, které zahrnují *server\_*  místo *.database\_*. vzor názvů Hello je jako **sys.server_event_%**.

## <a name="new-dynamic-management-views-dmvshttpmsdnmicrosoftcomlibraryms188754aspx"></a>Nové zobrazení dynamické správy [(zobrazení dynamické správy)](http://msdn.microsoft.com/library/ms188754.aspx)

Azure SQL Database má [zobrazení dynamické správy (zobrazení dynamické správy)](http://msdn.microsoft.com/library/bb677293.aspx) podporující rozšířené události. Zobrazení dynamické správy říct o *active* relace události.

| Název DMV | Popis |
|:--- |:--- |
| **Sys.dm_xe_database_session_event_actions** |Vrátí informace o akcích relace události. |
| **Sys.dm_xe_database_session_events** |Vrací informace o události relací. |
| **Sys.dm_xe_database_session_object_columns** |Zobrazuje hello hodnoty konfigurace pro objekty, které jsou vázané tooa relace. |
| **Sys.dm_xe_database_session_targets** |Vrací informace o relaci cíle. |
| **jestli v Sys.dm_xe_database_sessions nejsou** |Vrátí řádek pro každou relaci události, který je vymezená toohello aktuální databázi. |

V systému Microsoft SQL Server, jsou podobné zobrazení katalogu pojmenované bez hello  *\_databáze* část hello název, jako například:

- **Sys.dm_xe_sessions**, místo názvu<br/>**jestli v Sys.dm_xe_database_sessions nejsou**.

### <a name="dmvs-common-tooboth"></a>Běžné tooboth zobrazení dynamické správy
Pro rozšířené události existují další zobrazení dynamické správy, které jsou společné tooboth Azure SQL Database a serveru Microsoft SQL Server:

- **Sys.dm_xe_map_values**
- **Sys.dm_xe_object_columns**
- **Sys.dm_xe_objects**
- **Sys.dm_xe_packages**

 <a name="sqlfindseventsactionstargets" id="sqlfindseventsactionstargets"></a>

## <a name="find-hello-available-extended-events-actions-and-targets"></a>Najít hello dostupných rozšířených událostí, akcí a cíle

Můžete spustit jednoduché SQL **vyberte** tooobtain seznam dostupných událostí hello, akce a cíle.

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


<a name="AzureXEventsTargets" id="AzureXEventsTargets"></a> &nbsp;

## <a name="targets-for-your-sql-database-event-sessions"></a>Cíle pro relace události vaší databáze SQL

Zde jsou cíle, které můžete zaznamenat výsledky z vaší relace události v databázi SQL:

- [Cílové vyrovnávací paměti prstenec](http://msdn.microsoft.com/library/ff878182.aspx) -stručně uchovává data události v paměti.
- [Cíl události čítače](http://msdn.microsoft.com/library/ff878025.aspx) – počty všechny události, ke kterým došlo během relace rozšířených událostí.
- [Cíl souboru událostí](http://msdn.microsoft.com/library/ff878115.aspx) -zápisy dokončení vyrovnávací paměti tooan Azure Storage kontejneru.

Hello [trasování událostí pro Windows (ETW)](http://msdn.microsoft.com/library/ms751538.aspx) rozhraní API není k dispozici pro rozšířené události v databázi SQL.

## <a name="restrictions"></a>Omezení

Existuje několik rozdílů související se zabezpečením befitting hello cloudového prostředí databáze SQL:

- Rozšířené události vznikla na hello izolaci klientů jeden model. Relace události v jedné databáze nelze přistupovat k datům nebo události z jiné databáze.
- Nemůžou vystavovat **vytvořit událost relace** příkaz v kontextu hello hello **hlavní** databáze.

## <a name="permission-model"></a>Oprávnění modelu

Musíte mít **řízení** oprávnění na hello databáze tooissue **vytvořit událost relace** příkaz. Hello vlastníka databáze (dbo) má **řízení** oprávnění.

### <a name="storage-container-authorizations"></a>Povolení kontejneru úložiště

Hello tokenu SAS můžete vygenerovat pro vaše kontejner úložiště Azure, musíte zadat **rwl** hello oprávnění. Hello **rwl** hodnota poskytuje hello následující oprávnění:

- Čtení
- Zápis
- Seznam

## <a name="performance-considerations"></a>Otázky výkonu

Existují scénáře, kde můžete náročné pomocí rozšířených událostí hromadit aktivnější paměti, než je v pořádku pro hello celého systému. Proto hello systému Azure SQL Database dynamicky nastaví a upraví omezení hello množství aktivní paměti, který může být shromážděných řešením relace události. Mnoha faktorech, přejděte do výpočtu dynamické hello.

Pokud se zobrazí chybová zpráva s upozorněním, že byla vynucená maximální paměť, jsou některé opravné akce, které můžete provést:

- Spuštění relace méně souběžných události.
- Prostřednictvím vaší **vytvořit** a **ALTER** příkazy pro relace události, snížit hello množství paměti, které jste zadali na hello **maximální\_paměti** klauzule.

### <a name="network-latency"></a>Latence sítě

Hello **soubor událostí** cíl může zaznamenat latenci sítě nebo selhání při zachování dat tooAzure úložiště objektů BLOB. Další události v databázi SQL může zpoždění, které čekají na toocomplete komunikace sítě hello. Toto zpoždění může dojít ke snížení velikosti pracovní zátěže.

- toomitigate tento výkon rizik, nenastavujte hello **EVENT_RETENTION_MODE** možnost příliš**NO_EVENT_LOSS** v definic relace události.

## <a name="related-links"></a>Související odkazy

- [Použití Azure PowerShell s Azure Storage](../storage/common/storage-powershell-guide-full.md).
- [Rutiny úložiště Azure](http://msdn.microsoft.com/library/dn806401.aspx)
- [Použití Azure PowerShell s Azure Storage](../storage/common/storage-powershell-guide-full.md) – poskytuje podrobné informace o prostředí PowerShell a hello služby Azure Storage.
- [Jak toouse úložiště Blob z rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md)
- [CREATE CREDENTIAL (Transact-SQL)](http://msdn.microsoft.com/library/ms189522.aspx)
- [Vytvoření relace události (Transact-SQL)](http://msdn.microsoft.com/library/bb677289.aspx)
- [Jonathan Kehayias příspěvky o rozšířených událostí v systému Microsoft SQL Server](http://www.sqlskills.com/blogs/jonathan/category/extended-events/)


- Hello Azure *aktualizace služby* webovou stránku, zúžit pomocí parametru tooAzure SQL Database:
    - [https://Azure.microsoft.com/Updates/?Service=SQL-Database](https://azure.microsoft.com/updates/?service=sql-database)


Další témata ukázkový kód pro rozšířené události jsou k dispozici hello následující odkazy. Jestli hello ukázka cílem Microsoft SQL Server a databáze SQL Azure však musíte zkontrolovat pravidelně žádné toosee ukázka. Potom můžete rozhodnout, zda jsou mírně potřebné toorun hello ukázka.

<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find hello Objects That Have hello Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
