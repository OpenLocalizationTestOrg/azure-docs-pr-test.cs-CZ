---
title: "aaaReporting napříč instancemi cloudu databáze | Microsoft Docs"
description: "jak tooset až elastické dotazy na vodorovné oddíly"
services: sql-database
documentationcenter: 
manager: jhubbard
author: MladjoA
ms.assetid: f86eccb8-6323-4ba7-8559-8a7c039049f3
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: mlandzic
ms.openlocfilehash: 78986c2040bf308195bf7c77e64d4f37273fcf36
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-across-scaled-out-cloud-databases-preview"></a>Vytváření sestav napříč instancemi cloudu databází (preview)
![Dotazování mezi horizontálních oddílů][1]

Horizontálně dělené databáze distribuovat řádky napříč škálovaný dat vrstvy. schéma Hello je stejná na všechny zúčastněné databáze, také známé jako vodorovné rozdělení do oddílů. Pomocí elastické dotazu, můžete vytvořit sestavy, které jsou rozmístěny všechny databáze v horizontálně dělené databázi.

Rychle začít, najdete v části [vytváření sestav napříč instancemi cloudu databáze](sql-database-elastic-query-getting-started.md).

Horizontálně dělené databází, najdete v části [dotazu mezi databázemi cloudu s různými schématy](sql-database-elastic-query-vertical-partitioning.md). 

## <a name="prerequisites"></a>Požadavky
* Vytvoření mapy horizontálního oddílu pomocí klientské knihovny pro elastické databáze hello. v tématu [horizontálního oddílu mapy správu](sql-database-elastic-scale-shard-map-management.md). Nebo použijte hello ukázkovou aplikaci v [začít pracovat s nástroji elastické databáze](sql-database-elastic-scale-get-started.md).
* Alternativně si zobrazte [migrací stávající databáze, databáze na více systémů tooscaled](sql-database-elastic-convert-to-use-elastic-tools.md).
* Hello uživatel musí mít oprávnění ALTER ANY EXTERNAL DATA SOURCE. Toto oprávnění je součástí hello oprávnění ALTER DATABASE.
* Oprávnění ALTER ANY externí zdroj dat se zdroji dat toohello potřebné toorefer.

## <a name="overview"></a>Přehled
Tyto příkazy vytvořit reprezentaci metadata hello horizontálně dělené datové vrstvě v hello elastické dotaz do databáze. 

1. [VYTVOŘENÍ HLAVNÍHO KLÍČE](https://msdn.microsoft.com/library/ms174382.aspx)
2. [VYTVOŘENÍ DATABÁZE OBOR PŘIHLAŠOVACÍCH ÚDAJŮ](https://msdn.microsoft.com/library/mt270260.aspx)
3. [VYTVOŘENÍ EXTERNÍHO ZDROJE DAT](https://msdn.microsoft.com/library/dn935022.aspx)
4. [CREATE EXTERNAL TABLE](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="11-create-database-scoped-master-key-and-credentials"></a>1.1 vytvořit hlavní klíč databáze obor a přihlašovací údaje
Hello pověření je používáno hello elastické dotazu tooconnect tooyour vzdálené databáze.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]

> [!NOTE]
> Ujistěte se, že hello *"\<uživatelské jméno\>"* neobsahuje žádné *"@servername"* příponu. 
> 
> 

## <a name="12-create-external-data-sources"></a>1.2 Vytvoření externích zdrojů dat.
Syntaxe:

    <External_Data_Source> ::=    
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH                                              
            (TYPE = SHARD_MAP_MANAGER,
                       LOCATION = '<fully_qualified_server_name>',
            DATABASE_NAME = ‘<shardmap_database_name>',
            CREDENTIAL = <credential_name>, 
            SHARD_MAP_NAME = ‘<shardmapname>’ 
                   ) [;] 

### <a name="example"></a>Příklad
    CREATE EXTERNAL DATA SOURCE MyExtSrc 
    WITH 
    ( 
        TYPE=SHARD_MAP_MANAGER,
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ShardMapDatabase', 
        CREDENTIAL= SMMUser, 
        SHARD_MAP_NAME='ShardMap' 
    );

Načtení hello seznam aktuálních zdrojů externích dat: 

    select * from sys.external_data_sources; 

Hello externí zdroj dat odkazuje na mapě horizontálního oddílu. Dotaz elastické pak použije hello externí zdroj dat a hello základní horizontálního oddílu mapy tooenumerate hello databáze, které účastnit hello datové vrstvy. Hello stejné přihlašovací údaje jsou použité tooread hello horizontálního oddílu mapy a tooaccess hello data na horizontálních oddílů hello během zpracování hello elastické dotazu. 

## <a name="13-create-external-tables"></a>1.3 Vytvoření externí tabulky
Syntaxe:  

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name  
        ( { <column_definition> } [ ,...n ])     
        { WITH ( <sharded_external_table_options> ) }
    ) [;]  

    <sharded_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>,       
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 
      DISTRIBUTION = SHARDED(<sharding_column_name>) | REPLICATED |ROUND_ROBIN

**Příklad**

    CREATE EXTERNAL TABLE [dbo].[order_line]( 
         [ol_o_id] int NOT NULL, 
         [ol_d_id] tinyint NOT NULL,
         [ol_w_id] int NOT NULL, 
         [ol_number] tinyint NOT NULL, 
         [ol_i_id] int NOT NULL, 
         [ol_delivery_d] datetime NOT NULL, 
         [ol_amount] smallmoney NOT NULL, 
         [ol_supply_w_id] int NOT NULL, 
         [ol_quantity] smallint NOT NULL, 
         [ol_dist_info] char(24) NOT NULL 
    ) 

    WITH 
    ( 
        DATA_SOURCE = MyExtSrc, 
         SCHEMA_NAME = 'orders', 
         OBJECT_NAME = 'order_details', 
        DISTRIBUTION=SHARDED(ol_w_id)
    ); 

Načtení seznamu hello externí tabulky z aktuální databáze hello: 

    SELECT * from sys.external_tables; 

externí tabulky toodrop:

    DROP EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name[;]

### <a name="remarks"></a>Poznámky
Hello DATA\_zdroj klauzule definuje hello externí zdroj dat (horizontálních map) používaný pro externí tabulky hello.  

Hello schématu\_název a OBJEKT\_název klauzule mapování hello externí tooa tabulky v jiném schématu. Pokud tento parametr vynechán, hello schématu vzdálené objektu hello se předpokládá, že toobe "dbo" a jeho název je předpokládá, že název externí tabulky identické toohello toobe definovaný. To je užitečné, když se název hello Vzdálená tabulka už používá v databázi hello místo toocreate hello externí tabulky. Například chcete toodefine externí tabulky tooget agregované zobrazení katalogu zobrazení nebo zobrazení dynamické správy na škálovaný dat vrstvy. Vzhledem k tomu, že zobrazení katalogu a zobrazení dynamické správy již existují místně, nemůžete použít jejich názvy pro definici externí tabulky hello. Místo toho použijte jiný název a použijte zobrazení katalogu hello nebo hello DMV na název v hello schématu\_název nebo OBJEKT\_klauzule název. (Viz následující příklad hello.) 

klauzule distribuční Hello Určuje distribuci dat hello používá pro tuto tabulku. Procesor dotazů Hello využívá hello informací uvedených v hello distribuční klauzule toobuild hello nejúčinnější plány dotazů.  

1. **Horizontálně DĚLENÉ** znamená data vodorovně rozdělena mezi databázemi hello. Hello klíč rozdělení do oddílů pro distribuci dat hello je hello **< sharding_column_name >** parametr.
2. **REPLIKOVAT** znamená, že jsou identické kopií tabulky hello na každou databázi. Je vaše odpovědnosti tooensure, hello repliky se shodují mezi databázemi hello.
3. **ZAOKROUHLÍ\_každý s každým** znamená, že hello tabulka vodorovně rozdělena na oddíly pomocí metody distribuce závislé aplikace. 

**Datové vrstvy odkaz**: tooan externí zdroj dat odkazuje hello DDL pro externí tabulky. Hello externí zdroj dat určuje horizontálního oddílu mapu, která poskytuje hello externí tabulky s hello informace nezbytné toolocate všechny hello databáze v datové vrstvě. 

### <a name="security-considerations"></a>Aspekty zabezpečení
Uživatelé s externí tabulky toohello přístupu automaticky získají přístup toohello základní vzdálených tabulek v rámci hello přihlašovací údaje zadané v definici zdrojové externích dat hello. Vyhněte se nežádoucí zvýšení oprávnění prostřednictvím hello externí zdroj dat hello přihlašovací údaje. Pomocí GRANT nebo REVOKE pro externí tabulky stejně, jako by šlo o běžnou tabulku.  

Jakmile definujete zdroj externích dat a externí tabulky, teď můžete použít úplnou T-SQL na externí tabulky.

## <a name="example-querying-horizontal-partitioned-databases"></a>Příklad: dotazy na vodorovné oddílů databáze
Hello následující dotaz spojí třícestný sklady a pořadí řádky objednávek a používá několik agregace a selektivní filtru. Předpokládá (1) vodorovné rozdělení (horizontálního dělení) a (2) zda sklady, objednávek a pořadí řádky jsou horizontálně dělené podle hello skladu id sloupce, a můžete takový dotaz elastické hello společně umísťovat hello spojení na hello horizontálních oddílů a zpracovat hello nákladné součást hello dotazu na hello horizontálních oddílů paralelně. 

    select  
         w_id as warehouse,
         o_c_id as customer,
         count(*) as cnt_orderline,
         max(ol_quantity) as max_quantity,
         avg(ol_amount) as avg_amount, 
         min(ol_delivery_d) as min_deliv_date
    from warehouse 
    join orders 
    on w_id = o_w_id
    join order_line 
    on o_id = ol_o_id and o_w_id = ol_w_id 
    where w_id > 100 and w_id < 200 
    group by w_id, o_c_id 

## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a>Uložené procedury pro vzdálené spuštění T-SQL: sp\_execute_remote
Elastické dotazu také zavádí uložené procedury, která poskytuje přímý přístup toohello horizontálních oddílů. Hello uložená procedura je volána [sp\_provést \_vzdáleného](https://msdn.microsoft.com/library/mt703714) a může být vzdálené uložené procedury používané tooexecute nebo kód T-SQL na hello vzdálené databáze. Jak dlouho trvá hello následující parametry: 

* Název zdroje dat (nvarchar): název hello hello externí zdroj dat typu relační. 
* Dotaz (nvarchar): toobe hello T-SQL dotaz spustit na každém horizontálního oddílu. 
* Deklarace parametru (nvarchar) - volitelné: řetězec s definice typu dat pro hello parametry použité v parametru dotazu hello (např. sp_executesql). 
* Seznam hodnot parametru - volitelné: čárkami oddělený seznam hodnot parametrů (např. sp_executesql).

Hello sp\_provést\_vzdálené používá hello externí zdroj dat součástí hello volání parametry tooexecute hello zadaný příkaz jazyka T-SQL na hello vzdálené databáze. Používá hello přihlašovací údaje správce databáze shardmap tooconnect toohello hello externí datového zdroje a hello vzdálené databáze.  

Příklad: 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 

## <a name="connectivity-for-tools"></a>Připojení nástroje
Používat regulární tooconnect řetězce připojení SQL Server aplikace, vaše data a BI integrace nástroje toohello databáze s definic externí tabulky. Ujistěte se, že systém SQL Server je podporovaný jako zdroj dat pro vaše nástroje. Pak odkazovat hello elastické dotaz do databáze jako jakýkoli jiný nástroj toohello systému SQL Server databáze připojené a použít externí tabulky z nástroje nebo aplikace, jako kdyby byly místní tabulky. 

## <a name="best-practices"></a>Osvědčené postupy
* Zajistěte, že hello elastické dotazu koncový bod databáze nebyla zadána databáze shardmap toohello přístupu a všechny horizontálních oddílů prostřednictvím brány firewall SQL DB hello.  
* Ověřit nebo vynutit hello distribuci dat definované hello externí tabulky. Pokud distribuční skutečná data se liší od hello distribuční zadaný v definici vaší tabulky, vaše dotazy mohou vést k neočekávaným výsledkům. 
* Elastické dotazu aktuálně neprovádí odstranění horizontálních při predikáty nad klíčem horizontálního dělení hello by umožnilo toosafely vyloučení určitých horizontálních oddílů z zpracování.
* Elastické dotazu je nejvhodnější pro dotazy kde lze provést většinu výpočtu hello na hello horizontálních oddílů. Obvykle získat hello nejlepší výkon dotazů s predikáty selektivní filtru, které lze vyhodnotit na hello horizontálních oddílů nebo spojení přes hello dělení klíče, které lze provést tak oddílu zarovnaný na všechny horizontálních oddílů. Další vzory dotazu může být nutné tooload velkých objemů dat z hlavního uzlu toohello hello horizontálních oddílů a může být špatná

## <a name="next-steps"></a>Další kroky

* Přehled elastické dotazů najdete v tématu [elastické dotazu přehled](sql-database-elastic-query-overview.md).
* Vertikální dělení kurzu, najdete v části [Začínáme s mezidatabázové dotazu (vertikální dělení)](sql-database-elastic-query-getting-started-vertical.md).
* Syntaxe a ukázkové dotazy pro svisle oddílů data, najdete v části [dotazování svisle na oddíly dat)](sql-database-elastic-query-vertical-partitioning.md)
* Podívejte se kurz vodorovné rozdělení do oddílů (horizontálního dělení) [Začínáme s elastické dotazu pro vodorovné rozdělení do oddílů (horizontálního dělení)](sql-database-elastic-query-getting-started.md).
* V tématu [sp\_provést \_vzdáleného](https://msdn.microsoft.com/library/mt703714) pro uloženou proceduru, který spouští příkaz jazyka Transact-SQL na jedné vzdálené databáze SQL Azure nebo sadu databází, které slouží jako horizontálních oddílů v vodorovné schéma rozdělení oddílů.


<!--Image references-->
[1]: ./media/sql-database-elastic-query-horizontal-partitioning/horizontalpartitioning.png
<!--anchors-->
