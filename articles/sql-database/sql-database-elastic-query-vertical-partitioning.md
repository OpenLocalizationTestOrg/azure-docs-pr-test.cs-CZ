---
title: "Dotaz mezi databázemi cloudu s jinou schématu | Microsoft Docs"
description: "jak nastavit mezidatabázové dotazy přes svislé oddíly"
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
ms.openlocfilehash: e9036f92f6c76e8c4738ee981efa8a7b9791dcc7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="query-across-cloud-databases-with-different-schemas-preview"></a><span data-ttu-id="92641-103">Dotazování mezi databází cloudu s různými schématy (preview)</span><span class="sxs-lookup"><span data-stu-id="92641-103">Query across cloud databases with different schemas (preview)</span></span>
![Dotazování mezi tabulkami v různých databází][1]

<span data-ttu-id="92641-105">Svisle oddíly databáze používají různé sady tabulek na různých databází.</span><span class="sxs-lookup"><span data-stu-id="92641-105">Vertically-partitioned databases use different sets of tables on different databases.</span></span> <span data-ttu-id="92641-106">To znamená, že schéma se liší v různých databází.</span><span class="sxs-lookup"><span data-stu-id="92641-106">That means that the schema is different on different databases.</span></span> <span data-ttu-id="92641-107">Například všechny tabulky pro inventář je na jednu databázi, zatímco všechny tabulky související s monitorování účtů na druhý databáze.</span><span class="sxs-lookup"><span data-stu-id="92641-107">For instance, all tables for inventory are on one database while all accounting-related tables are on a second database.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="92641-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="92641-108">Prerequisites</span></span>
* <span data-ttu-id="92641-109">Uživatel musí mít oprávnění ALTER ANY EXTERNAL DATA SOURCE.</span><span class="sxs-lookup"><span data-stu-id="92641-109">The user must possess ALTER ANY EXTERNAL DATA SOURCE permission.</span></span> <span data-ttu-id="92641-110">Toto oprávnění je součástí oprávnění ALTER DATABASE.</span><span class="sxs-lookup"><span data-stu-id="92641-110">This permission is included with the ALTER DATABASE permission.</span></span>
* <span data-ttu-id="92641-111">K odkazování na podkladový zdroj dat jsou potřeba oprávnění ALTER ANY EXTERNAL DATA SOURCE.</span><span class="sxs-lookup"><span data-stu-id="92641-111">ALTER ANY EXTERNAL DATA SOURCE permissions are needed to refer to the underlying data source.</span></span>

## <a name="overview"></a><span data-ttu-id="92641-112">Přehled</span><span class="sxs-lookup"><span data-stu-id="92641-112">Overview</span></span>

> [!NOTE]
> <span data-ttu-id="92641-113">Na rozdíl od s vodorovné rozdělení do oddílů, tyto příkazy DDL nezávisí na definování datové vrstvy s mapou horizontálního oddílu pomocí klientské knihovny pro elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="92641-113">Unlike with horizontal partitioning, these DDL statements do not depend on defining a data tier with a shard map through the elastic database client library.</span></span>
>

1. [<span data-ttu-id="92641-114">VYTVOŘENÍ HLAVNÍHO KLÍČE</span><span class="sxs-lookup"><span data-stu-id="92641-114">CREATE MASTER KEY</span></span>](https://msdn.microsoft.com/library/ms174382.aspx)
2. [<span data-ttu-id="92641-115">VYTVOŘENÍ DATABÁZE OBOR PŘIHLAŠOVACÍCH ÚDAJŮ</span><span class="sxs-lookup"><span data-stu-id="92641-115">CREATE DATABASE SCOPED CREDENTIAL</span></span>](https://msdn.microsoft.com/library/mt270260.aspx)
3. [<span data-ttu-id="92641-116">VYTVOŘENÍ EXTERNÍHO ZDROJE DAT</span><span class="sxs-lookup"><span data-stu-id="92641-116">CREATE EXTERNAL DATA SOURCE</span></span>](https://msdn.microsoft.com/library/dn935022.aspx)
4. [<span data-ttu-id="92641-117">CREATE EXTERNAL TABLE</span><span class="sxs-lookup"><span data-stu-id="92641-117">CREATE EXTERNAL TABLE</span></span>](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="create-database-scoped-master-key-and-credentials"></a><span data-ttu-id="92641-118">Vytvořit hlavní klíč databáze obor a přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="92641-118">Create database scoped master key and credentials</span></span>
<span data-ttu-id="92641-119">Přihlašovací údaje se používá v elastické dotazu pro připojení k vzdálené databáze.</span><span class="sxs-lookup"><span data-stu-id="92641-119">The credential is used by the elastic query to connect to your remote databases.</span></span>  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]

> [!NOTE]
> <span data-ttu-id="92641-120">Ujistěte se, že `<username>` neobsahuje žádné **"@servername"** příponu.</span><span class="sxs-lookup"><span data-stu-id="92641-120">Ensure that the `<username>` does not include any **"@servername"** suffix.</span></span> 
>

## <a name="create-external-data-sources"></a><span data-ttu-id="92641-121">Vytvoření externích zdrojů dat.</span><span class="sxs-lookup"><span data-stu-id="92641-121">Create external data sources</span></span>
<span data-ttu-id="92641-122">Syntaxe:</span><span class="sxs-lookup"><span data-stu-id="92641-122">Syntax:</span></span>

    <External_Data_Source> ::=
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH 
               (TYPE = RDBMS,
                LOCATION = ’<fully_qualified_server_name>’,
                DATABASE_NAME = ‘<remote_database_name>’,  
                CREDENTIAL = <credential_name> 
                ) [;] 

> [!IMPORTANT]
> <span data-ttu-id="92641-123">Parametr typu musí být nastaven na **RDBMS**.</span><span class="sxs-lookup"><span data-stu-id="92641-123">The TYPE parameter must be set to **RDBMS**.</span></span> 
>

### <a name="example"></a><span data-ttu-id="92641-124">Příklad</span><span class="sxs-lookup"><span data-stu-id="92641-124">Example</span></span>
<span data-ttu-id="92641-125">Následující příklad ukazuje použití příkazu vytvořit pro externí zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="92641-125">The following example illustrates the use of the CREATE statement for external data sources.</span></span> 

    CREATE EXTERNAL DATA SOURCE RemoteReferenceData 
    WITH 
    ( 
        TYPE=RDBMS, 
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ReferenceData', 
        CREDENTIAL= SqlUser 
    ); 

<span data-ttu-id="92641-126">Načíst seznam aktuálních zdrojů dat externí:</span><span class="sxs-lookup"><span data-stu-id="92641-126">To retrieve the list of current external data sources:</span></span> 

    select * from sys.external_data_sources; 

### <a name="external-tables"></a><span data-ttu-id="92641-127">Externí tabulky</span><span class="sxs-lookup"><span data-stu-id="92641-127">External Tables</span></span>
<span data-ttu-id="92641-128">Syntaxe:</span><span class="sxs-lookup"><span data-stu-id="92641-128">Syntax:</span></span>

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name . ] table_name  
    ( { <column_definition> } [ ,...n ])     
    { WITH ( <rdbms_external_table_options> ) } 
    )[;] 

    <rdbms_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>, 
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 

### <a name="example"></a><span data-ttu-id="92641-129">Příklad</span><span class="sxs-lookup"><span data-stu-id="92641-129">Example</span></span>
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

<span data-ttu-id="92641-130">Následující příklad ukazuje, jak načíst seznam externí tabulky z aktuální databáze:</span><span class="sxs-lookup"><span data-stu-id="92641-130">The following example shows how to retrieve the list of external tables from the current database:</span></span> 

    select * from sys.external_tables; 

### <a name="remarks"></a><span data-ttu-id="92641-131">Poznámky</span><span class="sxs-lookup"><span data-stu-id="92641-131">Remarks</span></span>
<span data-ttu-id="92641-132">Elastické dotazu rozšiřuje stávající syntaxe externí tabulky můžete definovat externí tabulky, které používají externí zdroje dat typu relační.</span><span class="sxs-lookup"><span data-stu-id="92641-132">Elastic query extends the existing external table syntax to define external tables that use external data sources of type RDBMS.</span></span> <span data-ttu-id="92641-133">Definici externí tabulky pro vertikální dělení zahrnuje následující aspekty:</span><span class="sxs-lookup"><span data-stu-id="92641-133">An external table definition for vertical partitioning covers the following aspects:</span></span> 

* <span data-ttu-id="92641-134">**Schéma**: externí tabulky DDL definuje schéma, které můžete své dotazy.</span><span class="sxs-lookup"><span data-stu-id="92641-134">**Schema**: The external table DDL defines a schema that your queries can use.</span></span> <span data-ttu-id="92641-135">Schéma, které jsou součástí vaší definici externí tabulky musí odpovídat schématu tabulek v databázi vzdáleného se uloží skutečná data.</span><span class="sxs-lookup"><span data-stu-id="92641-135">The schema provided in your external table definition needs to match the schema of the tables in the remote database where the actual data is stored.</span></span> 
* <span data-ttu-id="92641-136">**Odkaz na vzdálené databáze**: DDL odkazuje na externí zdroj dat externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="92641-136">**Remote database reference**: The external table DDL refers to an external data source.</span></span> <span data-ttu-id="92641-137">Externí zdroj dat Určuje název logického serveru a název databáze vzdálenou databázi se uloží na skutečná data tabulky.</span><span class="sxs-lookup"><span data-stu-id="92641-137">The external data source specifies the logical server name and database name of the remote database where the actual table data is stored.</span></span> 

<span data-ttu-id="92641-138">Pomocí externího zdroje dat, jak je uvedeno v předchozí části, syntaxe k vytvoření externí tabulky je následující:</span><span class="sxs-lookup"><span data-stu-id="92641-138">Using an external data source as outlined in the previous section, the syntax to create external tables is as follows:</span></span> 

<span data-ttu-id="92641-139">V klauzuli DATA_SOURCE definuje zdroj externích dat (tj. vzdálené databáze v případě vertikální dělení), který se používá pro externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="92641-139">The DATA_SOURCE clause defines the external data source (i.e. the remote database in case of vertical partitioning) that is used for the external table.</span></span>  

<span data-ttu-id="92641-140">Klauzule SCHEMA_NAME a OBJECT_NAME poskytují možnost mapovat definici externí tabulky do tabulky v jiné schéma na vzdálenou databázi nebo tabulku s jiným názvem, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="92641-140">The SCHEMA_NAME and OBJECT_NAME clauses provide the ability to map the external table definition to a table in a different schema on the remote database, or to a table with a different name, respectively.</span></span> <span data-ttu-id="92641-141">To je užitečné, pokud chcete definovat externí tabulky k zobrazení katalogu nebo DMV na vzdálené databáze - nebo jakékoliv jiné situaci, kde název vzdálené tabulky je už zabraný místně.</span><span class="sxs-lookup"><span data-stu-id="92641-141">This is useful if you want to define an external table to a catalog view or DMV on your remote database - or any other situation where the remote table name is already taken locally.</span></span>  

<span data-ttu-id="92641-142">Následující příkaz DDL zahodí existující definici externí tabulky z místního katalogu.</span><span class="sxs-lookup"><span data-stu-id="92641-142">The following DDL statement drops an existing external table definition from the local catalog.</span></span> <span data-ttu-id="92641-143">Nemělo vliv vzdálené databáze.</span><span class="sxs-lookup"><span data-stu-id="92641-143">It does not impact the remote database.</span></span> 

    DROP EXTERNAL TABLE [ [ schema_name ] . | schema_name. ] table_name[;]  

<span data-ttu-id="92641-144">**Oprávnění pro příkaz CREATE/DROP externí tabulky**: pro externí tabulky DDL, který je také nutný k odkazování na podkladový zdroj dat jsou potřeba oprávnění ALTER ANY EXTERNAL DATA SOURCE.</span><span class="sxs-lookup"><span data-stu-id="92641-144">**Permissions for CREATE/DROP EXTERNAL TABLE**: ALTER ANY EXTERNAL DATA SOURCE permissions are needed for external table DDL which is also needed to refer to the underlying data source.</span></span>  

## <a name="security-considerations"></a><span data-ttu-id="92641-145">Aspekty zabezpečení</span><span class="sxs-lookup"><span data-stu-id="92641-145">Security considerations</span></span>
<span data-ttu-id="92641-146">Uživatelé s přístupem k externí tabulky automaticky získají přístup k základní vzdálených tabulek v části přihlašovací údaje zadané v definici zdrojové externí data.</span><span class="sxs-lookup"><span data-stu-id="92641-146">Users with access to the external table automatically gain access to the underlying remote tables under the credential given in the external data source definition.</span></span> <span data-ttu-id="92641-147">Aby se zabránilo nežádoucí zvýšení oprávnění pomocí přihlašovacích údajů z externího zdroje dat. měli pečlivě spravovat přístup k externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="92641-147">You should carefully manage access to the external table in order to avoid undesired elevation of privileges through the credential of the external data source.</span></span> <span data-ttu-id="92641-148">Regulární SQL oprávnění lze udělit nebo ODVOLAT přístup k externí tabulku stejně, jako by šlo o běžnou tabulku.</span><span class="sxs-lookup"><span data-stu-id="92641-148">Regular SQL permissions can be used to GRANT or REVOKE access to an external table just as though it were a regular table.</span></span>  

## <a name="example-querying-vertically-partitioned-databases"></a><span data-ttu-id="92641-149">Příklad: dotazování svisle na oddíly databáze</span><span class="sxs-lookup"><span data-stu-id="92641-149">Example: querying vertically partitioned databases</span></span>
<span data-ttu-id="92641-150">Následující dotaz provede třícestný spojení mezi dvěma místní tabulky pro objednávky a řádky určené pořadí a Vzdálená tabulka pro zákazníky.</span><span class="sxs-lookup"><span data-stu-id="92641-150">The following query performs a three-way join between the two local tables for orders and order lines and the remote table for customers.</span></span> <span data-ttu-id="92641-151">Jedná se o příklad případu použití dat odkaz pro elastické dotaz:</span><span class="sxs-lookup"><span data-stu-id="92641-151">This is an example of the reference data use case for elastic query:</span></span> 

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


## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a><span data-ttu-id="92641-152">Uložené procedury pro vzdálené spuštění T-SQL: sp\_execute_remote</span><span class="sxs-lookup"><span data-stu-id="92641-152">Stored procedure for remote T-SQL execution: sp\_execute_remote</span></span>
<span data-ttu-id="92641-153">Elastické dotazu také zavádí uložené procedury, která poskytuje přímý přístup k horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="92641-153">Elastic query also introduces a stored procedure that provides direct access to the shards.</span></span> <span data-ttu-id="92641-154">Uložená procedura je volána [sp\_provést \_vzdáleného](https://msdn.microsoft.com/library/mt703714) a můžete použít ke spuštění vzdálené uložené procedury nebo kód T-SQL na vzdálené databáze.</span><span class="sxs-lookup"><span data-stu-id="92641-154">The stored procedure is called [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) and can be used to execute remote stored procedures or T-SQL code on the remote databases.</span></span> <span data-ttu-id="92641-155">Ji používá následující parametry:</span><span class="sxs-lookup"><span data-stu-id="92641-155">It takes the following parameters:</span></span> 

* <span data-ttu-id="92641-156">Název zdroje dat (nvarchar): název zdroje externích dat typu relační.</span><span class="sxs-lookup"><span data-stu-id="92641-156">Data source name (nvarchar): The name of the external data source of type RDBMS.</span></span> 
* <span data-ttu-id="92641-157">Dotaz (nvarchar): na každý horizontálního oddílu provedení dotazu T-SQL.</span><span class="sxs-lookup"><span data-stu-id="92641-157">Query (nvarchar): The T-SQL query to be executed on each shard.</span></span> 
* <span data-ttu-id="92641-158">Deklarace parametru (nvarchar) - volitelné: řetězec s definice typu dat pro parametry použité v parametru dotazu (např. sp_executesql).</span><span class="sxs-lookup"><span data-stu-id="92641-158">Parameter declaration (nvarchar) - optional: String with data type definitions for the parameters used in the Query parameter (like sp_executesql).</span></span> 
* <span data-ttu-id="92641-159">Seznam hodnot parametru - volitelné: čárkami oddělený seznam hodnot parametrů (např. sp_executesql).</span><span class="sxs-lookup"><span data-stu-id="92641-159">Parameter value list - optional: Comma-separated list of parameter values (like sp_executesql).</span></span>

<span data-ttu-id="92641-160">Sp\_provést\_vzdálené používá externí zdroj dat součástí Parametry vyvolání daný příkaz T-SQL nelze provést na vzdálené databáze.</span><span class="sxs-lookup"><span data-stu-id="92641-160">The sp\_execute\_remote uses the external data source provided in the invocation parameters to execute the given T-SQL statement on the remote databases.</span></span> <span data-ttu-id="92641-161">Přihlašovací údaje z externí zdroj dat používá pro připojení k databázi správce shardmap a vzdálené databáze.</span><span class="sxs-lookup"><span data-stu-id="92641-161">It uses the credential of the external data source to connect to the shardmap manager database and the remote databases.</span></span>  

<span data-ttu-id="92641-162">Příklad:</span><span class="sxs-lookup"><span data-stu-id="92641-162">Example:</span></span> 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 



## <a name="connectivity-for-tools"></a><span data-ttu-id="92641-163">Připojení nástroje</span><span class="sxs-lookup"><span data-stu-id="92641-163">Connectivity for tools</span></span>
<span data-ttu-id="92641-164">Regulární připojovací řetězce SQL Server můžete použít pro připojení k databázím na databázi SQL serveru, který má povolené elastické dotazu a externí tabulky definované vaše integrace nástrojů BI a data.</span><span class="sxs-lookup"><span data-stu-id="92641-164">You can use regular SQL Server connection strings to connect your BI and data integration tools to databases on the SQL DB server that has elastic query enabled and external tables defined.</span></span> <span data-ttu-id="92641-165">Ujistěte se, že systém SQL Server je podporovaný jako zdroj dat pro vaše nástroje.</span><span class="sxs-lookup"><span data-stu-id="92641-165">Make sure that SQL Server is supported as a data source for your tool.</span></span> <span data-ttu-id="92641-166">Potom se podívejte elastické dotaz do databáze a její externí tabulky stejně jako jiné databázi systému SQL Server, který by se připojit k vaší nástrojem.</span><span class="sxs-lookup"><span data-stu-id="92641-166">Then refer to the elastic query database and its external tables just like any other SQL Server database that you would connect to with your tool.</span></span> 

## <a name="best-practices"></a><span data-ttu-id="92641-167">Osvědčené postupy</span><span class="sxs-lookup"><span data-stu-id="92641-167">Best practices</span></span>
* <span data-ttu-id="92641-168">Ujistěte se, že koncový bod databáze elastické dotazu udělen přístup ke vzdálené databázi povolením přístupu pro služby Azure v konfiguraci brány firewall SQL DB.</span><span class="sxs-lookup"><span data-stu-id="92641-168">Ensure that the elastic query endpoint database has been given access to the remote database by enabling access for Azure Services in its SQL DB firewall configuration.</span></span> <span data-ttu-id="92641-169">Zkontrolujte také, že zadané v definici zdrojové externích dat přihlašovací údaje může úspěšně přihlásit do vzdálené databáze a má oprávnění pro přístup k vzdálené tabulce.</span><span class="sxs-lookup"><span data-stu-id="92641-169">Also ensure that the credential provided in the external data source definition can successfully log into the remote database and has the permissions to access the remote table.</span></span>  
* <span data-ttu-id="92641-170">Elastické dotazu je nejvhodnější pro dotazy kde lze provést většinu výpočet na vzdálené databáze.</span><span class="sxs-lookup"><span data-stu-id="92641-170">Elastic query works best for queries where most of the computation can be done on the remote databases.</span></span> <span data-ttu-id="92641-171">Obvykle získáte nejlepší výkon dotazů s predikáty selektivní filtru, které lze vyhodnotit na vzdálené databáze nebo spojení, které lze provést zcela na vzdálené databáze.</span><span class="sxs-lookup"><span data-stu-id="92641-171">You typically get the best query performance with selective filter predicates that can be evaluated on the remote databases or joins that can be performed completely on the remote database.</span></span> <span data-ttu-id="92641-172">Ostatní typy dotazů možná muset načíst velké objemy dat od vzdálené databáze a může být špatná.</span><span class="sxs-lookup"><span data-stu-id="92641-172">Other query patterns may need to load large amounts of data from the remote database and may perform poorly.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="92641-173">Další kroky</span><span class="sxs-lookup"><span data-stu-id="92641-173">Next steps</span></span>

* <span data-ttu-id="92641-174">Přehled elastické dotazů najdete v tématu [elastické dotazu přehled](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="92641-174">For an overview of elastic query, see [Elastic query overview](sql-database-elastic-query-overview.md).</span></span>
* <span data-ttu-id="92641-175">Vertikální dělení kurzu, najdete v části [Začínáme s mezidatabázové dotazu (vertikální dělení)](sql-database-elastic-query-getting-started-vertical.md).</span><span class="sxs-lookup"><span data-stu-id="92641-175">For a vertical partitioning tutorial, see [Getting started with cross-database query (vertical partitioning)](sql-database-elastic-query-getting-started-vertical.md).</span></span>
* <span data-ttu-id="92641-176">Podívejte se kurz vodorovné rozdělení do oddílů (horizontálního dělení) [Začínáme s elastické dotazu pro vodorovné rozdělení do oddílů (horizontálního dělení)](sql-database-elastic-query-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="92641-176">For a horizontal partitioning (sharding) tutorial, see [Getting started with elastic query for horizontal partitioning (sharding)](sql-database-elastic-query-getting-started.md).</span></span>
* <span data-ttu-id="92641-177">Syntaxe a ukázkové dotazy pro horizontálně dělenou data, najdete v části [dotazování vodorovně rozdělena na oddíly dat)](sql-database-elastic-query-horizontal-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="92641-177">For syntax and sample queries for horizontally partitioned data, see [Querying horizontally partitioned data)](sql-database-elastic-query-horizontal-partitioning.md)</span></span>
* <span data-ttu-id="92641-178">V tématu [sp\_provést \_vzdáleného](https://msdn.microsoft.com/library/mt703714) pro uloženou proceduru, který spouští příkaz jazyka Transact-SQL na jedné vzdálené databáze SQL Azure nebo sadu databází, které slouží jako horizontálních oddílů v vodorovné schéma rozdělení oddílů.</span><span class="sxs-lookup"><span data-stu-id="92641-178">See [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) for a stored procedure that executes a Transact-SQL statement on a single remote Azure SQL Database or set of databases serving as shards in a horizontal partitioning scheme.</span></span>


<!--Image references-->
[1]: ./media/sql-database-elastic-query-vertical-partitioning/verticalpartitioning.png


<!--anchors-->
