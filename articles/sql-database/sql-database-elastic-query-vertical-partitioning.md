---
title: "aaaQuery v cloudové databáze s jinou schématu | Microsoft Docs"
description: "jak tooset až mezidatabázové dotazy na svislé oddíly"
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
ms.assetid: 84c261f2-9edc-42f4-988c-cf2f251f5eff
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: torsteng
ms.openlocfilehash: 1134e2e608128b7a9cac47ff35a22a11e6e5bc14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="query-across-cloud-databases-with-different-schemas-preview"></a>Dotazování mezi databází cloudu s různými schématy (preview)
![Dotazování mezi tabulkami v různých databází][1]

Svisle oddíly databáze používají různé sady tabulek na různých databází. To znamená, že schéma této hello se liší v různých databází. Například všechny tabulky pro inventář je na jednu databázi, zatímco všechny tabulky související s monitorování účtů na druhý databáze. 

## <a name="prerequisites"></a>Požadavky
* Hello uživatel musí mít oprávnění ALTER ANY EXTERNAL DATA SOURCE. Toto oprávnění je součástí hello oprávnění ALTER DATABASE.
* Oprávnění ALTER ANY externí zdroj dat se zdroji dat toohello potřebné toorefer.

## <a name="overview"></a>Přehled

> [!NOTE]
> Na rozdíl od s vodorovné rozdělení do oddílů, tyto příkazy DDL nezávisí na definování datové vrstvy s mapou horizontálního oddílu pomocí klientské knihovny pro elastické databáze hello.
>

1. [VYTVOŘENÍ HLAVNÍHO KLÍČE](https://msdn.microsoft.com/library/ms174382.aspx)
2. [VYTVOŘENÍ DATABÁZE OBOR PŘIHLAŠOVACÍCH ÚDAJŮ](https://msdn.microsoft.com/library/mt270260.aspx)
3. [VYTVOŘENÍ EXTERNÍHO ZDROJE DAT](https://msdn.microsoft.com/library/dn935022.aspx)
4. [CREATE EXTERNAL TABLE](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="create-database-scoped-master-key-and-credentials"></a>Vytvořit hlavní klíč databáze obor a přihlašovací údaje
Hello pověření je používáno hello elastické dotazu tooconnect tooyour vzdálené databáze.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]

> [!NOTE]
> Ujistěte se, že hello `<username>` neobsahuje žádné **"@servername"** příponu. 
>

## <a name="create-external-data-sources"></a>Vytvoření externích zdrojů dat.
Syntaxe:

    <External_Data_Source> ::=
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH 
               (TYPE = RDBMS,
                LOCATION = ’<fully_qualified_server_name>’,
                DATABASE_NAME = ‘<remote_database_name>’,  
                CREDENTIAL = <credential_name> 
                ) [;] 

> [!IMPORTANT]
> parametr typu Hello musí být nastaven příliš**RDBMS**. 
>

### <a name="example"></a>Příklad
Hello následující příklad ukazuje použití hello hello vytvořit příkaz pro externí zdroje dat. 

    CREATE EXTERNAL DATA SOURCE RemoteReferenceData 
    WITH 
    ( 
        TYPE=RDBMS, 
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ReferenceData', 
        CREDENTIAL= SqlUser 
    ); 

tooretrieve hello seznam aktuálních zdrojů dat externí: 

    select * from sys.external_data_sources; 

### <a name="external-tables"></a>Externí tabulky
Syntaxe:

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name . ] table_name  
    ( { <column_definition> } [ ,...n ])     
    { WITH ( <rdbms_external_table_options> ) } 
    )[;] 

    <rdbms_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>, 
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 

### <a name="example"></a>Příklad
    CREATE EXTERNAL TABLE [dbo].[customer]( 
        [c_id] int NOT NULL, 
        [c_firstname] nvarchar(256) NULL, 
        [c_lastname] nvarchar(256) NOT NULL, 
        [street] nvarchar(256) NOT NULL, 
        [city] nvarchar(256) NOT NULL, 
        [state] nvarchar(20) NULL, 
        [country] nvarchar(50) NOT NULL, 
    ) 
    WITH 
    ( 
           DATA_SOURCE = RemoteReferenceData 
    ); 

Hello následující příklad ukazuje, jak tooretrieve hello seznam externí tabulky z aktuální databáze hello: 

    select * from sys.external_tables; 

### <a name="remarks"></a>Poznámky
Elastické dotazu rozšiřuje hello existující externí tabulky syntax toodefine externí tabulky používající externí zdroje dat typu relační. Definici externí tabulky pro vertikální dělení obsahuje hello následující aspekty: 

* **Schéma**: externí příkaz DDL tabulky hello definuje schéma, které můžete své dotazy. musí se schéma Hello součástí vaší definici externí tabulky toomatch hello schématu tabulek hello v hello vzdálenou databázi se uloží hello skutečná data. 
* **Odkaz na vzdálené databáze**: tooan externí zdroj dat odkazuje hello DDL pro externí tabulky. Hello externí zdroj dat určuje hello název logického serveru a název databáze hello vzdálenou databázi se uloží data hello skutečné tabulky. 

Pomocí externího zdroje dat, jak je uvedeno v předchozí části hello, externí tabulky toocreate hello syntaxe vypadá takto: 

klauzule DATA_SOURCE Hello definuje zdroj externích dat hello (tj. hello vzdálené databáze v případě vertikální dělení), který se používá pro externí tabulky hello.  

klauzule SCHEMA_NAME a OBJECT_NAME Hello zadejte hello možnost toomap hello externí tooa tabulky v jiném schématu na hello vzdálenou databázi nebo tabulku tooa s jiným názvem, v uvedeném pořadí. To je užitečné, pokud chcete toodefine zobrazení katalogu tooa externí tabulky nebo DMV na vzdálené databáze - nebo jakékoliv jiné situaci, kde název vzdálené tabulky hello je už zabrané místně.  

Hello následující příkaz DDL zahodí existující definici externí tabulky od hello místního katalogu. Nemělo vliv hello vzdálené databáze. 

    DROP EXTERNAL TABLE [ [ schema_name ] . | schema_name. ] table_name[;]  

**Oprávnění pro příkaz CREATE/DROP externí tabulky**: pro externí tabulky DDL, který je taky potřeba toorefer toohello podkladové zdroje dat jsou potřeba oprávnění ALTER ANY EXTERNAL DATA SOURCE.  

## <a name="security-considerations"></a>Aspekty zabezpečení
Uživatelé s externí tabulky toohello přístupu automaticky získají přístup toohello základní vzdálených tabulek v rámci hello přihlašovací údaje zadané v definici zdrojové externích dat hello. Pečlivě by měla spravovat přístup toohello externí tabulky v pořadí tooavoid nežádoucí zvýšení oprávnění prostřednictvím hello externí zdroj dat hello přihlašovací údaje. Regulární oprávnění SQL lze použít tooGRANT nebo ODVOLAT přístup tooan externí tabulky stejně, jako by šlo o běžnou tabulku.  

## <a name="example-querying-vertically-partitioned-databases"></a>Příklad: dotazování svisle na oddíly databáze
Hello následující dotaz spojí třícestný hello dvě místní tabulky pro objednávky a řádky určené pořadí a hello vzdálenou tabulku pro zákazníky. Jedná se o příklad hello referenční data případu použití pro elastické dotaz: 

    SELECT      
     c_id as customer,
     c_lastname as customer_name,
     count(*) as cnt_orderline, 
     max(ol_quantity) as max_quantity,
     avg(ol_amount) as avg_amount,
     min(ol_delivery_d) as min_deliv_date
    FROM customer 
    JOIN orders 
    ON c_id = o_c_id
    JOIN  order_line 
    ON o_id = ol_o_id and o_c_id = ol_c_id
    WHERE c_id = 100


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
Vaše data a BI toodatabases nástroje integrace regulární tooconnect řetězce připojení SQL Server můžete použít na hello databázi SQL serveru, který má povolené elastické dotazu a externí tabulky definované. Ujistěte se, že systém SQL Server je podporovaný jako zdroj dat pro vaše nástroje. Potom se podívejte toohello elastické dotaz do databáze a její externí tabulky stejně jako ostatní databáze systému SQL Server, že byste připojili toowith vaše nástroje. 

## <a name="best-practices"></a>Osvědčené postupy
* Zkontrolujte, zda že tento koncový bod elastické dotaz hello do databáze byla vydána vzdálené databáze access toohello povolením přístupu pro služby Azure v konfiguraci brány firewall SQL DB. Ujistěte se také, přihlašovacích údajů tohoto hello zadaný v externích dat hello definice zdroje může úspěšně přihlásit k hello vzdálené databáze a má hello oprávnění tooaccess hello vzdálenou tabulku.  
* Elastické dotazu je nejvhodnější pro dotazy kde lze provést většinu výpočtu hello na hello vzdálené databáze. Obvykle získáte nejlepší výkon dotazů hello s predikáty selektivní filtru, které lze vyhodnotit na vzdálené databáze hello nebo spojení, které lze provést zcela na hello vzdálené databáze. Ostatní typy dotazů může být nutné tooload velkých objemů dat z hello vzdálené databáze a může být špatná. 

## <a name="next-steps"></a>Další kroky

* Přehled elastické dotazů najdete v tématu [elastické dotazu přehled](sql-database-elastic-query-overview.md).
* Vertikální dělení kurzu, najdete v části [Začínáme s mezidatabázové dotazu (vertikální dělení)](sql-database-elastic-query-getting-started-vertical.md).
* Podívejte se kurz vodorovné rozdělení do oddílů (horizontálního dělení) [Začínáme s elastické dotazu pro vodorovné rozdělení do oddílů (horizontálního dělení)](sql-database-elastic-query-getting-started.md).
* Syntaxe a ukázkové dotazy pro horizontálně dělenou data, najdete v části [dotazování vodorovně rozdělena na oddíly dat)](sql-database-elastic-query-horizontal-partitioning.md)
* V tématu [sp\_provést \_vzdáleného](https://msdn.microsoft.com/library/mt703714) pro uloženou proceduru, který spouští příkaz jazyka Transact-SQL na jedné vzdálené databáze SQL Azure nebo sadu databází, které slouží jako horizontálních oddílů v vodorovné schéma rozdělení oddílů.


<!--Image references-->
[1]: ./media/sql-database-elastic-query-vertical-partitioning/verticalpartitioning.png


<!--anchors-->
