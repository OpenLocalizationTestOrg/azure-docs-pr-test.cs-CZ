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
# <a name="query-across-cloud-databases-with-different-schemas-preview"></a><span data-ttu-id="b2990-103">Dotazování mezi databází cloudu s různými schématy (preview)</span><span class="sxs-lookup"><span data-stu-id="b2990-103">Query across cloud databases with different schemas (preview)</span></span>
![Dotazování mezi tabulkami v různých databází][1]

<span data-ttu-id="b2990-105">Svisle oddíly databáze používají různé sady tabulek na různých databází.</span><span class="sxs-lookup"><span data-stu-id="b2990-105">Vertically-partitioned databases use different sets of tables on different databases.</span></span> <span data-ttu-id="b2990-106">To znamená, že schéma této hello se liší v různých databází.</span><span class="sxs-lookup"><span data-stu-id="b2990-106">That means that hello schema is different on different databases.</span></span> <span data-ttu-id="b2990-107">Například všechny tabulky pro inventář je na jednu databázi, zatímco všechny tabulky související s monitorování účtů na druhý databáze.</span><span class="sxs-lookup"><span data-stu-id="b2990-107">For instance, all tables for inventory are on one database while all accounting-related tables are on a second database.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="b2990-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b2990-108">Prerequisites</span></span>
* <span data-ttu-id="b2990-109">Hello uživatel musí mít oprávnění ALTER ANY EXTERNAL DATA SOURCE.</span><span class="sxs-lookup"><span data-stu-id="b2990-109">hello user must possess ALTER ANY EXTERNAL DATA SOURCE permission.</span></span> <span data-ttu-id="b2990-110">Toto oprávnění je součástí hello oprávnění ALTER DATABASE.</span><span class="sxs-lookup"><span data-stu-id="b2990-110">This permission is included with hello ALTER DATABASE permission.</span></span>
* <span data-ttu-id="b2990-111">Oprávnění ALTER ANY externí zdroj dat se zdroji dat toohello potřebné toorefer.</span><span class="sxs-lookup"><span data-stu-id="b2990-111">ALTER ANY EXTERNAL DATA SOURCE permissions are needed toorefer toohello underlying data source.</span></span>

## <a name="overview"></a><span data-ttu-id="b2990-112">Přehled</span><span class="sxs-lookup"><span data-stu-id="b2990-112">Overview</span></span>

> [!NOTE]
> <span data-ttu-id="b2990-113">Na rozdíl od s vodorovné rozdělení do oddílů, tyto příkazy DDL nezávisí na definování datové vrstvy s mapou horizontálního oddílu pomocí klientské knihovny pro elastické databáze hello.</span><span class="sxs-lookup"><span data-stu-id="b2990-113">Unlike with horizontal partitioning, these DDL statements do not depend on defining a data tier with a shard map through hello elastic database client library.</span></span>
>

1. [<span data-ttu-id="b2990-114">VYTVOŘENÍ HLAVNÍHO KLÍČE</span><span class="sxs-lookup"><span data-stu-id="b2990-114">CREATE MASTER KEY</span></span>](https://msdn.microsoft.com/library/ms174382.aspx)
2. [<span data-ttu-id="b2990-115">VYTVOŘENÍ DATABÁZE OBOR PŘIHLAŠOVACÍCH ÚDAJŮ</span><span class="sxs-lookup"><span data-stu-id="b2990-115">CREATE DATABASE SCOPED CREDENTIAL</span></span>](https://msdn.microsoft.com/library/mt270260.aspx)
3. [<span data-ttu-id="b2990-116">VYTVOŘENÍ EXTERNÍHO ZDROJE DAT</span><span class="sxs-lookup"><span data-stu-id="b2990-116">CREATE EXTERNAL DATA SOURCE</span></span>](https://msdn.microsoft.com/library/dn935022.aspx)
4. [<span data-ttu-id="b2990-117">CREATE EXTERNAL TABLE</span><span class="sxs-lookup"><span data-stu-id="b2990-117">CREATE EXTERNAL TABLE</span></span>](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="create-database-scoped-master-key-and-credentials"></a><span data-ttu-id="b2990-118">Vytvořit hlavní klíč databáze obor a přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="b2990-118">Create database scoped master key and credentials</span></span>
<span data-ttu-id="b2990-119">Hello pověření je používáno hello elastické dotazu tooconnect tooyour vzdálené databáze.</span><span class="sxs-lookup"><span data-stu-id="b2990-119">hello credential is used by hello elastic query tooconnect tooyour remote databases.</span></span>  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]

> [!NOTE]
> <span data-ttu-id="b2990-120">Ujistěte se, že hello `<username>` neobsahuje žádné **"@servername"** příponu.</span><span class="sxs-lookup"><span data-stu-id="b2990-120">Ensure that hello `<username>` does not include any **"@servername"** suffix.</span></span> 
>

## <a name="create-external-data-sources"></a><span data-ttu-id="b2990-121">Vytvoření externích zdrojů dat.</span><span class="sxs-lookup"><span data-stu-id="b2990-121">Create external data sources</span></span>
<span data-ttu-id="b2990-122">Syntaxe:</span><span class="sxs-lookup"><span data-stu-id="b2990-122">Syntax:</span></span>

    <External_Data_Source> ::=
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH 
               (TYPE = RDBMS,
                LOCATION = ’<fully_qualified_server_name>’,
                DATABASE_NAME = ‘<remote_database_name>’,  
                CREDENTIAL = <credential_name> 
                ) [;] 

> [!IMPORTANT]
> <span data-ttu-id="b2990-123">parametr typu Hello musí být nastaven příliš**RDBMS**.</span><span class="sxs-lookup"><span data-stu-id="b2990-123">hello TYPE parameter must be set too**RDBMS**.</span></span> 
>

### <a name="example"></a><span data-ttu-id="b2990-124">Příklad</span><span class="sxs-lookup"><span data-stu-id="b2990-124">Example</span></span>
<span data-ttu-id="b2990-125">Hello následující příklad ukazuje použití hello hello vytvořit příkaz pro externí zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="b2990-125">hello following example illustrates hello use of hello CREATE statement for external data sources.</span></span> 

    CREATE EXTERNAL DATA SOURCE RemoteReferenceData 
    WITH 
    ( 
        TYPE=RDBMS, 
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ReferenceData', 
        CREDENTIAL= SqlUser 
    ); 

<span data-ttu-id="b2990-126">tooretrieve hello seznam aktuálních zdrojů dat externí:</span><span class="sxs-lookup"><span data-stu-id="b2990-126">tooretrieve hello list of current external data sources:</span></span> 

    select * from sys.external_data_sources; 

### <a name="external-tables"></a><span data-ttu-id="b2990-127">Externí tabulky</span><span class="sxs-lookup"><span data-stu-id="b2990-127">External Tables</span></span>
<span data-ttu-id="b2990-128">Syntaxe:</span><span class="sxs-lookup"><span data-stu-id="b2990-128">Syntax:</span></span>

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name . ] table_name  
    ( { <column_definition> } [ ,...n ])     
    { WITH ( <rdbms_external_table_options> ) } 
    )[;] 

    <rdbms_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>, 
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 

### <a name="example"></a><span data-ttu-id="b2990-129">Příklad</span><span class="sxs-lookup"><span data-stu-id="b2990-129">Example</span></span>
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

<span data-ttu-id="b2990-130">Hello následující příklad ukazuje, jak tooretrieve hello seznam externí tabulky z aktuální databáze hello:</span><span class="sxs-lookup"><span data-stu-id="b2990-130">hello following example shows how tooretrieve hello list of external tables from hello current database:</span></span> 

    select * from sys.external_tables; 

### <a name="remarks"></a><span data-ttu-id="b2990-131">Poznámky</span><span class="sxs-lookup"><span data-stu-id="b2990-131">Remarks</span></span>
<span data-ttu-id="b2990-132">Elastické dotazu rozšiřuje hello existující externí tabulky syntax toodefine externí tabulky používající externí zdroje dat typu relační.</span><span class="sxs-lookup"><span data-stu-id="b2990-132">Elastic query extends hello existing external table syntax toodefine external tables that use external data sources of type RDBMS.</span></span> <span data-ttu-id="b2990-133">Definici externí tabulky pro vertikální dělení obsahuje hello následující aspekty:</span><span class="sxs-lookup"><span data-stu-id="b2990-133">An external table definition for vertical partitioning covers hello following aspects:</span></span> 

* <span data-ttu-id="b2990-134">**Schéma**: externí příkaz DDL tabulky hello definuje schéma, které můžete své dotazy.</span><span class="sxs-lookup"><span data-stu-id="b2990-134">**Schema**: hello external table DDL defines a schema that your queries can use.</span></span> <span data-ttu-id="b2990-135">musí se schéma Hello součástí vaší definici externí tabulky toomatch hello schématu tabulek hello v hello vzdálenou databázi se uloží hello skutečná data.</span><span class="sxs-lookup"><span data-stu-id="b2990-135">hello schema provided in your external table definition needs toomatch hello schema of hello tables in hello remote database where hello actual data is stored.</span></span> 
* <span data-ttu-id="b2990-136">**Odkaz na vzdálené databáze**: tooan externí zdroj dat odkazuje hello DDL pro externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="b2990-136">**Remote database reference**: hello external table DDL refers tooan external data source.</span></span> <span data-ttu-id="b2990-137">Hello externí zdroj dat určuje hello název logického serveru a název databáze hello vzdálenou databázi se uloží data hello skutečné tabulky.</span><span class="sxs-lookup"><span data-stu-id="b2990-137">hello external data source specifies hello logical server name and database name of hello remote database where hello actual table data is stored.</span></span> 

<span data-ttu-id="b2990-138">Pomocí externího zdroje dat, jak je uvedeno v předchozí části hello, externí tabulky toocreate hello syntaxe vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="b2990-138">Using an external data source as outlined in hello previous section, hello syntax toocreate external tables is as follows:</span></span> 

<span data-ttu-id="b2990-139">klauzule DATA_SOURCE Hello definuje zdroj externích dat hello (tj. hello vzdálené databáze v případě vertikální dělení), který se používá pro externí tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="b2990-139">hello DATA_SOURCE clause defines hello external data source (i.e. hello remote database in case of vertical partitioning) that is used for hello external table.</span></span>  

<span data-ttu-id="b2990-140">klauzule SCHEMA_NAME a OBJECT_NAME Hello zadejte hello možnost toomap hello externí tooa tabulky v jiném schématu na hello vzdálenou databázi nebo tabulku tooa s jiným názvem, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="b2990-140">hello SCHEMA_NAME and OBJECT_NAME clauses provide hello ability toomap hello external table definition tooa table in a different schema on hello remote database, or tooa table with a different name, respectively.</span></span> <span data-ttu-id="b2990-141">To je užitečné, pokud chcete toodefine zobrazení katalogu tooa externí tabulky nebo DMV na vzdálené databáze - nebo jakékoliv jiné situaci, kde název vzdálené tabulky hello je už zabrané místně.</span><span class="sxs-lookup"><span data-stu-id="b2990-141">This is useful if you want toodefine an external table tooa catalog view or DMV on your remote database - or any other situation where hello remote table name is already taken locally.</span></span>  

<span data-ttu-id="b2990-142">Hello následující příkaz DDL zahodí existující definici externí tabulky od hello místního katalogu.</span><span class="sxs-lookup"><span data-stu-id="b2990-142">hello following DDL statement drops an existing external table definition from hello local catalog.</span></span> <span data-ttu-id="b2990-143">Nemělo vliv hello vzdálené databáze.</span><span class="sxs-lookup"><span data-stu-id="b2990-143">It does not impact hello remote database.</span></span> 

    DROP EXTERNAL TABLE [ [ schema_name ] . | schema_name. ] table_name[;]  

<span data-ttu-id="b2990-144">**Oprávnění pro příkaz CREATE/DROP externí tabulky**: pro externí tabulky DDL, který je taky potřeba toorefer toohello podkladové zdroje dat jsou potřeba oprávnění ALTER ANY EXTERNAL DATA SOURCE.</span><span class="sxs-lookup"><span data-stu-id="b2990-144">**Permissions for CREATE/DROP EXTERNAL TABLE**: ALTER ANY EXTERNAL DATA SOURCE permissions are needed for external table DDL which is also needed toorefer toohello underlying data source.</span></span>  

## <a name="security-considerations"></a><span data-ttu-id="b2990-145">Aspekty zabezpečení</span><span class="sxs-lookup"><span data-stu-id="b2990-145">Security considerations</span></span>
<span data-ttu-id="b2990-146">Uživatelé s externí tabulky toohello přístupu automaticky získají přístup toohello základní vzdálených tabulek v rámci hello přihlašovací údaje zadané v definici zdrojové externích dat hello.</span><span class="sxs-lookup"><span data-stu-id="b2990-146">Users with access toohello external table automatically gain access toohello underlying remote tables under hello credential given in hello external data source definition.</span></span> <span data-ttu-id="b2990-147">Pečlivě by měla spravovat přístup toohello externí tabulky v pořadí tooavoid nežádoucí zvýšení oprávnění prostřednictvím hello externí zdroj dat hello přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="b2990-147">You should carefully manage access toohello external table in order tooavoid undesired elevation of privileges through hello credential of hello external data source.</span></span> <span data-ttu-id="b2990-148">Regulární oprávnění SQL lze použít tooGRANT nebo ODVOLAT přístup tooan externí tabulky stejně, jako by šlo o běžnou tabulku.</span><span class="sxs-lookup"><span data-stu-id="b2990-148">Regular SQL permissions can be used tooGRANT or REVOKE access tooan external table just as though it were a regular table.</span></span>  

## <a name="example-querying-vertically-partitioned-databases"></a><span data-ttu-id="b2990-149">Příklad: dotazování svisle na oddíly databáze</span><span class="sxs-lookup"><span data-stu-id="b2990-149">Example: querying vertically partitioned databases</span></span>
<span data-ttu-id="b2990-150">Hello následující dotaz spojí třícestný hello dvě místní tabulky pro objednávky a řádky určené pořadí a hello vzdálenou tabulku pro zákazníky.</span><span class="sxs-lookup"><span data-stu-id="b2990-150">hello following query performs a three-way join between hello two local tables for orders and order lines and hello remote table for customers.</span></span> <span data-ttu-id="b2990-151">Jedná se o příklad hello referenční data případu použití pro elastické dotaz:</span><span class="sxs-lookup"><span data-stu-id="b2990-151">This is an example of hello reference data use case for elastic query:</span></span> 

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


## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a><span data-ttu-id="b2990-152">Uložené procedury pro vzdálené spuštění T-SQL: sp\_execute_remote</span><span class="sxs-lookup"><span data-stu-id="b2990-152">Stored procedure for remote T-SQL execution: sp\_execute_remote</span></span>
<span data-ttu-id="b2990-153">Elastické dotazu také zavádí uložené procedury, která poskytuje přímý přístup toohello horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="b2990-153">Elastic query also introduces a stored procedure that provides direct access toohello shards.</span></span> <span data-ttu-id="b2990-154">Hello uložená procedura je volána [sp\_provést \_vzdáleného](https://msdn.microsoft.com/library/mt703714) a může být vzdálené uložené procedury používané tooexecute nebo kód T-SQL na hello vzdálené databáze.</span><span class="sxs-lookup"><span data-stu-id="b2990-154">hello stored procedure is called [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) and can be used tooexecute remote stored procedures or T-SQL code on hello remote databases.</span></span> <span data-ttu-id="b2990-155">Jak dlouho trvá hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="b2990-155">It takes hello following parameters:</span></span> 

* <span data-ttu-id="b2990-156">Název zdroje dat (nvarchar): název hello hello externí zdroj dat typu relační.</span><span class="sxs-lookup"><span data-stu-id="b2990-156">Data source name (nvarchar): hello name of hello external data source of type RDBMS.</span></span> 
* <span data-ttu-id="b2990-157">Dotaz (nvarchar): toobe hello T-SQL dotaz spustit na každém horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="b2990-157">Query (nvarchar): hello T-SQL query toobe executed on each shard.</span></span> 
* <span data-ttu-id="b2990-158">Deklarace parametru (nvarchar) - volitelné: řetězec s definice typu dat pro hello parametry použité v parametru dotazu hello (např. sp_executesql).</span><span class="sxs-lookup"><span data-stu-id="b2990-158">Parameter declaration (nvarchar) - optional: String with data type definitions for hello parameters used in hello Query parameter (like sp_executesql).</span></span> 
* <span data-ttu-id="b2990-159">Seznam hodnot parametru - volitelné: čárkami oddělený seznam hodnot parametrů (např. sp_executesql).</span><span class="sxs-lookup"><span data-stu-id="b2990-159">Parameter value list - optional: Comma-separated list of parameter values (like sp_executesql).</span></span>

<span data-ttu-id="b2990-160">Hello sp\_provést\_vzdálené používá hello externí zdroj dat součástí hello volání parametry tooexecute hello zadaný příkaz jazyka T-SQL na hello vzdálené databáze.</span><span class="sxs-lookup"><span data-stu-id="b2990-160">hello sp\_execute\_remote uses hello external data source provided in hello invocation parameters tooexecute hello given T-SQL statement on hello remote databases.</span></span> <span data-ttu-id="b2990-161">Používá hello přihlašovací údaje správce databáze shardmap tooconnect toohello hello externí datového zdroje a hello vzdálené databáze.</span><span class="sxs-lookup"><span data-stu-id="b2990-161">It uses hello credential of hello external data source tooconnect toohello shardmap manager database and hello remote databases.</span></span>  

<span data-ttu-id="b2990-162">Příklad:</span><span class="sxs-lookup"><span data-stu-id="b2990-162">Example:</span></span> 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 



## <a name="connectivity-for-tools"></a><span data-ttu-id="b2990-163">Připojení nástroje</span><span class="sxs-lookup"><span data-stu-id="b2990-163">Connectivity for tools</span></span>
<span data-ttu-id="b2990-164">Vaše data a BI toodatabases nástroje integrace regulární tooconnect řetězce připojení SQL Server můžete použít na hello databázi SQL serveru, který má povolené elastické dotazu a externí tabulky definované.</span><span class="sxs-lookup"><span data-stu-id="b2990-164">You can use regular SQL Server connection strings tooconnect your BI and data integration tools toodatabases on hello SQL DB server that has elastic query enabled and external tables defined.</span></span> <span data-ttu-id="b2990-165">Ujistěte se, že systém SQL Server je podporovaný jako zdroj dat pro vaše nástroje.</span><span class="sxs-lookup"><span data-stu-id="b2990-165">Make sure that SQL Server is supported as a data source for your tool.</span></span> <span data-ttu-id="b2990-166">Potom se podívejte toohello elastické dotaz do databáze a její externí tabulky stejně jako ostatní databáze systému SQL Server, že byste připojili toowith vaše nástroje.</span><span class="sxs-lookup"><span data-stu-id="b2990-166">Then refer toohello elastic query database and its external tables just like any other SQL Server database that you would connect toowith your tool.</span></span> 

## <a name="best-practices"></a><span data-ttu-id="b2990-167">Osvědčené postupy</span><span class="sxs-lookup"><span data-stu-id="b2990-167">Best practices</span></span>
* <span data-ttu-id="b2990-168">Zkontrolujte, zda že tento koncový bod elastické dotaz hello do databáze byla vydána vzdálené databáze access toohello povolením přístupu pro služby Azure v konfiguraci brány firewall SQL DB.</span><span class="sxs-lookup"><span data-stu-id="b2990-168">Ensure that hello elastic query endpoint database has been given access toohello remote database by enabling access for Azure Services in its SQL DB firewall configuration.</span></span> <span data-ttu-id="b2990-169">Ujistěte se také, přihlašovacích údajů tohoto hello zadaný v externích dat hello definice zdroje může úspěšně přihlásit k hello vzdálené databáze a má hello oprávnění tooaccess hello vzdálenou tabulku.</span><span class="sxs-lookup"><span data-stu-id="b2990-169">Also ensure that hello credential provided in hello external data source definition can successfully log into hello remote database and has hello permissions tooaccess hello remote table.</span></span>  
* <span data-ttu-id="b2990-170">Elastické dotazu je nejvhodnější pro dotazy kde lze provést většinu výpočtu hello na hello vzdálené databáze.</span><span class="sxs-lookup"><span data-stu-id="b2990-170">Elastic query works best for queries where most of hello computation can be done on hello remote databases.</span></span> <span data-ttu-id="b2990-171">Obvykle získáte nejlepší výkon dotazů hello s predikáty selektivní filtru, které lze vyhodnotit na vzdálené databáze hello nebo spojení, které lze provést zcela na hello vzdálené databáze.</span><span class="sxs-lookup"><span data-stu-id="b2990-171">You typically get hello best query performance with selective filter predicates that can be evaluated on hello remote databases or joins that can be performed completely on hello remote database.</span></span> <span data-ttu-id="b2990-172">Ostatní typy dotazů může být nutné tooload velkých objemů dat z hello vzdálené databáze a může být špatná.</span><span class="sxs-lookup"><span data-stu-id="b2990-172">Other query patterns may need tooload large amounts of data from hello remote database and may perform poorly.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="b2990-173">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b2990-173">Next steps</span></span>

* <span data-ttu-id="b2990-174">Přehled elastické dotazů najdete v tématu [elastické dotazu přehled](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b2990-174">For an overview of elastic query, see [Elastic query overview](sql-database-elastic-query-overview.md).</span></span>
* <span data-ttu-id="b2990-175">Vertikální dělení kurzu, najdete v části [Začínáme s mezidatabázové dotazu (vertikální dělení)](sql-database-elastic-query-getting-started-vertical.md).</span><span class="sxs-lookup"><span data-stu-id="b2990-175">For a vertical partitioning tutorial, see [Getting started with cross-database query (vertical partitioning)](sql-database-elastic-query-getting-started-vertical.md).</span></span>
* <span data-ttu-id="b2990-176">Podívejte se kurz vodorovné rozdělení do oddílů (horizontálního dělení) [Začínáme s elastické dotazu pro vodorovné rozdělení do oddílů (horizontálního dělení)](sql-database-elastic-query-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="b2990-176">For a horizontal partitioning (sharding) tutorial, see [Getting started with elastic query for horizontal partitioning (sharding)](sql-database-elastic-query-getting-started.md).</span></span>
* <span data-ttu-id="b2990-177">Syntaxe a ukázkové dotazy pro horizontálně dělenou data, najdete v části [dotazování vodorovně rozdělena na oddíly dat)](sql-database-elastic-query-horizontal-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="b2990-177">For syntax and sample queries for horizontally partitioned data, see [Querying horizontally partitioned data)](sql-database-elastic-query-horizontal-partitioning.md)</span></span>
* <span data-ttu-id="b2990-178">V tématu [sp\_provést \_vzdáleného](https://msdn.microsoft.com/library/mt703714) pro uloženou proceduru, který spouští příkaz jazyka Transact-SQL na jedné vzdálené databáze SQL Azure nebo sadu databází, které slouží jako horizontálních oddílů v vodorovné schéma rozdělení oddílů.</span><span class="sxs-lookup"><span data-stu-id="b2990-178">See [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) for a stored procedure that executes a Transact-SQL statement on a single remote Azure SQL Database or set of databases serving as shards in a horizontal partitioning scheme.</span></span>


<!--Image references-->
[1]: ./media/sql-database-elastic-query-vertical-partitioning/verticalpartitioning.png


<!--anchors-->
