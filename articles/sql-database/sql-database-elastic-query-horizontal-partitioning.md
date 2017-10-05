---
title: "Vytváření sestav napříč instancemi cloudu databáze | Microsoft Docs"
description: "jak nastavit elastické dotazy přes vodorovné oddíly"
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
ms.openlocfilehash: 62b5bcd26aa1ed219fb38970916e0e8847ceb577
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="reporting-across-scaled-out-cloud-databases-preview"></a><span data-ttu-id="7a479-103">Vytváření sestav napříč instancemi cloudu databází (preview)</span><span class="sxs-lookup"><span data-stu-id="7a479-103">Reporting across scaled-out cloud databases (preview)</span></span>
![Dotazování mezi horizontálních oddílů][1]

<span data-ttu-id="7a479-105">Horizontálně dělené databáze distribuovat řádky napříč škálovaný dat vrstvy.</span><span class="sxs-lookup"><span data-stu-id="7a479-105">Sharded databases distribute rows across a scaled out data tier.</span></span> <span data-ttu-id="7a479-106">Schéma je stejná na všechny zúčastněné databáze, také známé jako vodorovné rozdělení do oddílů.</span><span class="sxs-lookup"><span data-stu-id="7a479-106">The schema is identical on all participating databases, also known as horizontal partitioning.</span></span> <span data-ttu-id="7a479-107">Pomocí elastické dotazu, můžete vytvořit sestavy, které jsou rozmístěny všechny databáze v horizontálně dělené databázi.</span><span class="sxs-lookup"><span data-stu-id="7a479-107">Using an elastic query, you can create reports that span all databases in a sharded database.</span></span>

<span data-ttu-id="7a479-108">Rychle začít, najdete v části [vytváření sestav napříč instancemi cloudu databáze](sql-database-elastic-query-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="7a479-108">For a quick start, see [Reporting across scaled-out cloud databases](sql-database-elastic-query-getting-started.md).</span></span>

<span data-ttu-id="7a479-109">Horizontálně dělené databází, najdete v části [dotazu mezi databázemi cloudu s různými schématy](sql-database-elastic-query-vertical-partitioning.md).</span><span class="sxs-lookup"><span data-stu-id="7a479-109">For non-sharded databases, see [Query across cloud databases with different schemas](sql-database-elastic-query-vertical-partitioning.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="7a479-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7a479-110">Prerequisites</span></span>
* <span data-ttu-id="7a479-111">Vytvoření mapy horizontálního oddílu pomocí klientské knihovny elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="7a479-111">Create a shard map using the elastic database client library.</span></span> <span data-ttu-id="7a479-112">v tématu [horizontálního oddílu mapy správu](sql-database-elastic-scale-shard-map-management.md).</span><span class="sxs-lookup"><span data-stu-id="7a479-112">see [Shard map management](sql-database-elastic-scale-shard-map-management.md).</span></span> <span data-ttu-id="7a479-113">Nebo použijte ukázkovou aplikaci v [začít pracovat s nástroji elastické databáze](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="7a479-113">Or use the sample app in [Get started with elastic database tools](sql-database-elastic-scale-get-started.md).</span></span>
* <span data-ttu-id="7a479-114">Alternativně si zobrazte [migrovat existující databáze do databází upraveným](sql-database-elastic-convert-to-use-elastic-tools.md).</span><span class="sxs-lookup"><span data-stu-id="7a479-114">Alternatively, see [Migrate existing databases to scaled-out databases](sql-database-elastic-convert-to-use-elastic-tools.md).</span></span>
* <span data-ttu-id="7a479-115">Uživatel musí mít oprávnění ALTER ANY EXTERNAL DATA SOURCE.</span><span class="sxs-lookup"><span data-stu-id="7a479-115">The user must possess ALTER ANY EXTERNAL DATA SOURCE permission.</span></span> <span data-ttu-id="7a479-116">Toto oprávnění je součástí oprávnění ALTER DATABASE.</span><span class="sxs-lookup"><span data-stu-id="7a479-116">This permission is included with the ALTER DATABASE permission.</span></span>
* <span data-ttu-id="7a479-117">K odkazování na podkladový zdroj dat jsou potřeba oprávnění ALTER ANY EXTERNAL DATA SOURCE.</span><span class="sxs-lookup"><span data-stu-id="7a479-117">ALTER ANY EXTERNAL DATA SOURCE permissions are needed to refer to the underlying data source.</span></span>

## <a name="overview"></a><span data-ttu-id="7a479-118">Přehled</span><span class="sxs-lookup"><span data-stu-id="7a479-118">Overview</span></span>
<span data-ttu-id="7a479-119">Tyto příkazy vytvoření metadat reprezentace horizontálně dělené datové vrstvě v databázi elastické dotazu.</span><span class="sxs-lookup"><span data-stu-id="7a479-119">These statements create the metadata representation of your sharded data tier in the elastic query database.</span></span> 

1. [<span data-ttu-id="7a479-120">VYTVOŘENÍ HLAVNÍHO KLÍČE</span><span class="sxs-lookup"><span data-stu-id="7a479-120">CREATE MASTER KEY</span></span>](https://msdn.microsoft.com/library/ms174382.aspx)
2. [<span data-ttu-id="7a479-121">VYTVOŘENÍ DATABÁZE OBOR PŘIHLAŠOVACÍCH ÚDAJŮ</span><span class="sxs-lookup"><span data-stu-id="7a479-121">CREATE DATABASE SCOPED CREDENTIAL</span></span>](https://msdn.microsoft.com/library/mt270260.aspx)
3. [<span data-ttu-id="7a479-122">VYTVOŘENÍ EXTERNÍHO ZDROJE DAT</span><span class="sxs-lookup"><span data-stu-id="7a479-122">CREATE EXTERNAL DATA SOURCE</span></span>](https://msdn.microsoft.com/library/dn935022.aspx)
4. [<span data-ttu-id="7a479-123">CREATE EXTERNAL TABLE</span><span class="sxs-lookup"><span data-stu-id="7a479-123">CREATE EXTERNAL TABLE</span></span>](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="11-create-database-scoped-master-key-and-credentials"></a><span data-ttu-id="7a479-124">1.1 vytvořit hlavní klíč databáze obor a přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="7a479-124">1.1 Create database scoped master key and credentials</span></span>
<span data-ttu-id="7a479-125">Přihlašovací údaje se používá v elastické dotazu pro připojení k vzdálené databáze.</span><span class="sxs-lookup"><span data-stu-id="7a479-125">The credential is used by the elastic query to connect to your remote databases.</span></span>  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]

> [!NOTE]
> <span data-ttu-id="7a479-126">Ujistěte se, že *"\<uživatelské jméno\>"* neobsahuje žádné *"@servername"* příponu.</span><span class="sxs-lookup"><span data-stu-id="7a479-126">Make sure that the *"\<username\>"* does not include any *"@servername"* suffix.</span></span> 
> 
> 

## <a name="12-create-external-data-sources"></a><span data-ttu-id="7a479-127">1.2 Vytvoření externích zdrojů dat.</span><span class="sxs-lookup"><span data-stu-id="7a479-127">1.2 Create external data sources</span></span>
<span data-ttu-id="7a479-128">Syntaxe:</span><span class="sxs-lookup"><span data-stu-id="7a479-128">Syntax:</span></span>

    <External_Data_Source> ::=    
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH                                              
            (TYPE = SHARD_MAP_MANAGER,
                       LOCATION = '<fully_qualified_server_name>',
            DATABASE_NAME = ‘<shardmap_database_name>',
            CREDENTIAL = <credential_name>, 
            SHARD_MAP_NAME = ‘<shardmapname>’ 
                   ) [;] 

### <a name="example"></a><span data-ttu-id="7a479-129">Příklad</span><span class="sxs-lookup"><span data-stu-id="7a479-129">Example</span></span>
    CREATE EXTERNAL DATA SOURCE MyExtSrc 
    WITH 
    ( 
        TYPE=SHARD_MAP_MANAGER,
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ShardMapDatabase', 
        CREDENTIAL= SMMUser, 
        SHARD_MAP_NAME='ShardMap' 
    );

<span data-ttu-id="7a479-130">Načtěte seznam aktuálních zdrojů dat externí:</span><span class="sxs-lookup"><span data-stu-id="7a479-130">Retrieve the list of current external data sources:</span></span> 

    select * from sys.external_data_sources; 

<span data-ttu-id="7a479-131">Externí zdroj dat odkazuje na mapě horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="7a479-131">The external data source references your shard map.</span></span> <span data-ttu-id="7a479-132">Dotaz elastické pak používá externí zdroj dat a základní mapy horizontálního oddílu výčet databází, které jsou součástí datové vrstvy.</span><span class="sxs-lookup"><span data-stu-id="7a479-132">An elastic query then uses the external data source and the underlying shard map to enumerate the databases that participate in the data tier.</span></span> <span data-ttu-id="7a479-133">Stejné přihlašovací údaje se používají ke čtení mapy horizontálního oddílu a k přístupu k datům na horizontálních oddílů během zpracování elastické dotazu.</span><span class="sxs-lookup"><span data-stu-id="7a479-133">The same credentials are used to read the shard map and to access the data on the shards during the processing of an elastic query.</span></span> 

## <a name="13-create-external-tables"></a><span data-ttu-id="7a479-134">1.3 Vytvoření externí tabulky</span><span class="sxs-lookup"><span data-stu-id="7a479-134">1.3 Create external tables</span></span>
<span data-ttu-id="7a479-135">Syntaxe:</span><span class="sxs-lookup"><span data-stu-id="7a479-135">Syntax:</span></span>  

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name  
        ( { <column_definition> } [ ,...n ])     
        { WITH ( <sharded_external_table_options> ) }
    ) [;]  

    <sharded_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>,       
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 
      DISTRIBUTION = SHARDED(<sharding_column_name>) | REPLICATED |ROUND_ROBIN

<span data-ttu-id="7a479-136">**Příklad**</span><span class="sxs-lookup"><span data-stu-id="7a479-136">**Example**</span></span>

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

<span data-ttu-id="7a479-137">Načtení seznamu externí tabulky z aktuální databáze:</span><span class="sxs-lookup"><span data-stu-id="7a479-137">Retrieve the list of external tables from the current database:</span></span> 

    SELECT * from sys.external_tables; 

<span data-ttu-id="7a479-138">Chcete-li vyřadit externí tabulky:</span><span class="sxs-lookup"><span data-stu-id="7a479-138">To drop external tables:</span></span>

    DROP EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name[;]

### <a name="remarks"></a><span data-ttu-id="7a479-139">Poznámky</span><span class="sxs-lookup"><span data-stu-id="7a479-139">Remarks</span></span>
<span data-ttu-id="7a479-140">DATA\_zdroj klauzule definuje zdroj externích dat (horizontálních map), který se používá pro externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="7a479-140">The DATA\_SOURCE clause defines the external data source (a shard map) that is used for the external table.</span></span>  

<span data-ttu-id="7a479-141">SCHÉMA\_název a OBJEKT\_název klauzule mapování definici externí tabulky do tabulky v jiném schématu.</span><span class="sxs-lookup"><span data-stu-id="7a479-141">The SCHEMA\_NAME and OBJECT\_NAME clauses map the external table definition to a table in a different schema.</span></span> <span data-ttu-id="7a479-142">Pokud tento parametr vynechán, je schéma ke vzdálenému objektu předpokládá se "dbo" a její název se považuje za stejný jako název externí tabulky definovaný.</span><span class="sxs-lookup"><span data-stu-id="7a479-142">If omitted, the schema of the remote object is assumed to be “dbo” and its name is assumed to be identical to the external table name being defined.</span></span> <span data-ttu-id="7a479-143">To je užitečné, když se název vzdálené tabulky se už používá v databázi kde chcete vytvořit externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="7a479-143">This is useful if the name of your remote table is already taken in the database where you want to create the external table.</span></span> <span data-ttu-id="7a479-144">Například můžete definovat externí tabulku získat agregované zobrazení katalogu zobrazení nebo zobrazení dynamické správy na škálovaný dat vrstvy.</span><span class="sxs-lookup"><span data-stu-id="7a479-144">For  example, you want to define an external table to get an aggregate view of catalog views or DMVs on your scaled out data tier.</span></span> <span data-ttu-id="7a479-145">Vzhledem k tomu, že zobrazení katalogu a zobrazení dynamické správy již existují místně, nemůžete použít jejich názvy pro definici externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="7a479-145">Since catalog views and DMVs already exist locally, you cannot use their names for the external table definition.</span></span> <span data-ttu-id="7a479-146">Místo toho použijte jiný název a použijte zobrazení katalogu nebo DMV název ve schématu\_název nebo OBJEKT\_klauzule název.</span><span class="sxs-lookup"><span data-stu-id="7a479-146">Instead, use a different name and use the catalog view’s or the DMV’s name in the SCHEMA\_NAME and/or OBJECT\_NAME clauses.</span></span> <span data-ttu-id="7a479-147">(Viz následující příklad.)</span><span class="sxs-lookup"><span data-stu-id="7a479-147">(See the example below.)</span></span> 

<span data-ttu-id="7a479-148">V klauzuli distribuční Určuje distribuci dat použít pro tuto tabulku.</span><span class="sxs-lookup"><span data-stu-id="7a479-148">The DISTRIBUTION clause specifies the data distribution used for this table.</span></span> <span data-ttu-id="7a479-149">Procesor dotazů využívá informací uvedených v klauzuli distribuční k vytvoření nejúčinnější plány dotazů.</span><span class="sxs-lookup"><span data-stu-id="7a479-149">The query processor utilizes the information provided in the DISTRIBUTION clause to build the most efficient query plans.</span></span>  

1. <span data-ttu-id="7a479-150">**Horizontálně DĚLENÉ** znamená data vodorovně rozdělena mezi databází.</span><span class="sxs-lookup"><span data-stu-id="7a479-150">**SHARDED** means data is horizontally partitioned across the databases.</span></span> <span data-ttu-id="7a479-151">Klíč rozdělení pro distribuci dat je **< sharding_column_name >** parametr.</span><span class="sxs-lookup"><span data-stu-id="7a479-151">The partitioning key for the data distribution is the **<sharding_column_name>** parameter.</span></span>
2. <span data-ttu-id="7a479-152">**REPLIKOVAT** znamená, že jsou identické kopií tabulky na každou databázi.</span><span class="sxs-lookup"><span data-stu-id="7a479-152">**REPLICATED** means that identical copies of the table are present on each database.</span></span> <span data-ttu-id="7a479-153">Je vaší povinností ujistit, že repliky jsou identické mezi databází.</span><span class="sxs-lookup"><span data-stu-id="7a479-153">It is your responsibility to ensure that the replicas are identical across the databases.</span></span>
3. <span data-ttu-id="7a479-154">**ZAOKROUHLÍ\_každý s každým** znamená, že v tabulce je vodorovně rozdělena na oddíly pomocí metody distribuce závislé aplikace.</span><span class="sxs-lookup"><span data-stu-id="7a479-154">**ROUND\_ROBIN** means that the table is horizontally partitioned using an application-dependent distribution method.</span></span> 

<span data-ttu-id="7a479-155">**Datové vrstvy odkaz**: DDL odkazuje na externí zdroj dat externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="7a479-155">**Data tier reference**: The external table DDL refers to an external data source.</span></span> <span data-ttu-id="7a479-156">Externí zdroj dat určuje horizontálního oddílu mapy, které poskytuje informace potřebné k vyhledání všechny databáze v datové vrstvě externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="7a479-156">The external data source specifies a shard map which provides the external table with the information necessary to locate all the databases in your data tier.</span></span> 

### <a name="security-considerations"></a><span data-ttu-id="7a479-157">Aspekty zabezpečení</span><span class="sxs-lookup"><span data-stu-id="7a479-157">Security considerations</span></span>
<span data-ttu-id="7a479-158">Uživatelé s přístupem k externí tabulky automaticky získají přístup k základní vzdálených tabulek v části přihlašovací údaje zadané v definici zdrojové externí data.</span><span class="sxs-lookup"><span data-stu-id="7a479-158">Users with access to the external table automatically gain access to the underlying remote tables under the credential given in the external data source definition.</span></span> <span data-ttu-id="7a479-159">Vyhněte se nežádoucí zvýšení oprávnění pomocí přihlašovacích údajů zdroje dat externí.</span><span class="sxs-lookup"><span data-stu-id="7a479-159">Avoid undesired elevation of privileges through the credential of the external data source.</span></span> <span data-ttu-id="7a479-160">Pomocí GRANT nebo REVOKE pro externí tabulky stejně, jako by šlo o běžnou tabulku.</span><span class="sxs-lookup"><span data-stu-id="7a479-160">Use GRANT or REVOKE for an external table just as though it were a regular table.</span></span>  

<span data-ttu-id="7a479-161">Jakmile definujete zdroj externích dat a externí tabulky, teď můžete použít úplnou T-SQL na externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="7a479-161">Once you have defined your external data source and your external tables, you can now use full T-SQL over your external tables.</span></span>

## <a name="example-querying-horizontal-partitioned-databases"></a><span data-ttu-id="7a479-162">Příklad: dotazy na vodorovné oddílů databáze</span><span class="sxs-lookup"><span data-stu-id="7a479-162">Example: querying horizontal partitioned databases</span></span>
<span data-ttu-id="7a479-163">Následující dotaz spojí třícestný sklady a pořadí řádky objednávek a používá několik agregace a selektivní filtru.</span><span class="sxs-lookup"><span data-stu-id="7a479-163">The following query performs a three-way join between warehouses, orders and order lines and uses several aggregates and a selective filter.</span></span> <span data-ttu-id="7a479-164">Předpokládá (1) vodorovné rozdělení (horizontálního dělení) a (2), sklady, objednávek a pořadí řádky jsou horizontálně dělené podle id sloupce skladu, a zda elastické dotaz můžete společně umísťovat spojení na horizontálních oddílů zpracovat nákladné část dotazu na horizontálních oddílů v paralelní.</span><span class="sxs-lookup"><span data-stu-id="7a479-164">It assumes (1) horizontal partitioning (sharding) and (2) that warehouses, orders and order lines are sharded by the warehouse id column, and that the elastic query can co-locate the joins on the shards and process the expensive part of the query on the shards in parallel.</span></span> 

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

## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a><span data-ttu-id="7a479-165">Uložené procedury pro vzdálené spuštění T-SQL: sp\_execute_remote</span><span class="sxs-lookup"><span data-stu-id="7a479-165">Stored procedure for remote T-SQL execution: sp\_execute_remote</span></span>
<span data-ttu-id="7a479-166">Elastické dotazu také zavádí uložené procedury, která poskytuje přímý přístup k horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="7a479-166">Elastic query also introduces a stored procedure that provides direct access to the shards.</span></span> <span data-ttu-id="7a479-167">Uložená procedura je volána [sp\_provést \_vzdáleného](https://msdn.microsoft.com/library/mt703714) a můžete použít ke spuštění vzdálené uložené procedury nebo kód T-SQL na vzdálené databáze.</span><span class="sxs-lookup"><span data-stu-id="7a479-167">The stored procedure is called [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) and can be used to execute remote stored procedures or T-SQL code on the remote databases.</span></span> <span data-ttu-id="7a479-168">Ji používá následující parametry:</span><span class="sxs-lookup"><span data-stu-id="7a479-168">It takes the following parameters:</span></span> 

* <span data-ttu-id="7a479-169">Název zdroje dat (nvarchar): název zdroje externích dat typu relační.</span><span class="sxs-lookup"><span data-stu-id="7a479-169">Data source name (nvarchar): The name of the external data source of type RDBMS.</span></span> 
* <span data-ttu-id="7a479-170">Dotaz (nvarchar): na každý horizontálního oddílu provedení dotazu T-SQL.</span><span class="sxs-lookup"><span data-stu-id="7a479-170">Query (nvarchar): The T-SQL query to be executed on each shard.</span></span> 
* <span data-ttu-id="7a479-171">Deklarace parametru (nvarchar) - volitelné: řetězec s definice typu dat pro parametry použité v parametru dotazu (např. sp_executesql).</span><span class="sxs-lookup"><span data-stu-id="7a479-171">Parameter declaration (nvarchar) - optional: String with data type definitions for the parameters used in the Query parameter (like sp_executesql).</span></span> 
* <span data-ttu-id="7a479-172">Seznam hodnot parametru - volitelné: čárkami oddělený seznam hodnot parametrů (např. sp_executesql).</span><span class="sxs-lookup"><span data-stu-id="7a479-172">Parameter value list - optional: Comma-separated list of parameter values (like sp_executesql).</span></span>

<span data-ttu-id="7a479-173">Sp\_provést\_vzdálené používá externí zdroj dat součástí Parametry vyvolání daný příkaz T-SQL nelze provést na vzdálené databáze.</span><span class="sxs-lookup"><span data-stu-id="7a479-173">The sp\_execute\_remote uses the external data source provided in the invocation parameters to execute the given T-SQL statement on the remote databases.</span></span> <span data-ttu-id="7a479-174">Přihlašovací údaje z externí zdroj dat používá pro připojení k databázi správce shardmap a vzdálené databáze.</span><span class="sxs-lookup"><span data-stu-id="7a479-174">It uses the credential of the external data source to connect to the shardmap manager database and the remote databases.</span></span>  

<span data-ttu-id="7a479-175">Příklad:</span><span class="sxs-lookup"><span data-stu-id="7a479-175">Example:</span></span> 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 

## <a name="connectivity-for-tools"></a><span data-ttu-id="7a479-176">Připojení nástroje</span><span class="sxs-lookup"><span data-stu-id="7a479-176">Connectivity for tools</span></span>
<span data-ttu-id="7a479-177">Používat regulární připojovací řetězce SQL serveru pro připojení vaší aplikace, vaše data a BI nástrojů pro integraci do databáze s definic externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="7a479-177">Use regular SQL Server connection strings to connect your application, your BI and data integration tools to the database with your external table definitions.</span></span> <span data-ttu-id="7a479-178">Ujistěte se, že systém SQL Server je podporovaný jako zdroj dat pro vaše nástroje.</span><span class="sxs-lookup"><span data-stu-id="7a479-178">Make sure that SQL Server is supported as a data source for your tool.</span></span> <span data-ttu-id="7a479-179">Pak odkaz databáze elastické dotazu jako jakékoli jiné databáze systému SQL Server připojen k nástroj a použijte externí tabulky z nástroje nebo aplikace jako kdyby byly místní tabulky.</span><span class="sxs-lookup"><span data-stu-id="7a479-179">Then reference the elastic query database like any other SQL Server database connected to the tool, and use external tables from your tool or application as if they were local tables.</span></span> 

## <a name="best-practices"></a><span data-ttu-id="7a479-180">Osvědčené postupy</span><span class="sxs-lookup"><span data-stu-id="7a479-180">Best practices</span></span>
* <span data-ttu-id="7a479-181">Ujistěte se, že koncový bod databáze elastické dotazu byl udělen přístup k databázi shardmap a všechny horizontálních oddílů přes brány firewall SQL DB.</span><span class="sxs-lookup"><span data-stu-id="7a479-181">Ensure that the elastic query endpoint database has been given access to the shardmap database and all shards through the SQL DB firewalls.</span></span>  
* <span data-ttu-id="7a479-182">Ověřit nebo vynutit distribuci dat definované externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="7a479-182">Validate or enforce the data distribution defined by the external table.</span></span> <span data-ttu-id="7a479-183">Pokud distribuční skutečná data se liší od rozdělení zadaný v definici vaší tabulky, vaše dotazy mohou vést k neočekávaným výsledkům.</span><span class="sxs-lookup"><span data-stu-id="7a479-183">If your actual data distribution is different from the distribution specified in your table definition, your queries may yield unexpected results.</span></span> 
* <span data-ttu-id="7a479-184">Elastické dotazu momentálně neprovádí odstranění horizontálních při predikáty nad klíčem horizontálního dělení by mohla bezpečně vyloučit některé horizontálních oddílů z zpracování.</span><span class="sxs-lookup"><span data-stu-id="7a479-184">Elastic query currently does not perform shard elimination when predicates over the sharding key would allow it to safely exclude certain shards from processing.</span></span>
* <span data-ttu-id="7a479-185">Elastické dotazu je nejvhodnější pro dotazy kde většinu výpočet lze provést na horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="7a479-185">Elastic query works best for queries where most of the computation can be done on the shards.</span></span> <span data-ttu-id="7a479-186">Obvykle získáte nejlepší výkon dotazů s predikáty selektivní filtru, které lze vyhodnotit na horizontálních oddílů nebo spojení přes rozdělení klíčů, které lze provést tak oddílu zarovnaný na všechny horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="7a479-186">You typically get the best query performance with selective filter predicates that can be evaluated on the shards or joins over the partitioning keys that can be performed in a partition-aligned way on all shards.</span></span> <span data-ttu-id="7a479-187">Ostatní typy dotazů možná muset načíst velkých objemů dat z horizontálních oddílů k hlavnímu uzlu a může být špatná</span><span class="sxs-lookup"><span data-stu-id="7a479-187">Other query patterns may need to load large amounts of data from the shards to the head node and may perform poorly</span></span>

## <a name="next-steps"></a><span data-ttu-id="7a479-188">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7a479-188">Next steps</span></span>

* <span data-ttu-id="7a479-189">Přehled elastické dotazů najdete v tématu [elastické dotazu přehled](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7a479-189">For an overview of elastic query, see [Elastic query overview](sql-database-elastic-query-overview.md).</span></span>
* <span data-ttu-id="7a479-190">Vertikální dělení kurzu, najdete v části [Začínáme s mezidatabázové dotazu (vertikální dělení)](sql-database-elastic-query-getting-started-vertical.md).</span><span class="sxs-lookup"><span data-stu-id="7a479-190">For a vertical partitioning tutorial, see [Getting started with cross-database query (vertical partitioning)](sql-database-elastic-query-getting-started-vertical.md).</span></span>
* <span data-ttu-id="7a479-191">Syntaxe a ukázkové dotazy pro svisle oddílů data, najdete v části [dotazování svisle na oddíly dat)](sql-database-elastic-query-vertical-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="7a479-191">For syntax and sample queries for vertically partitioned data, see [Querying vertically partitioned data)](sql-database-elastic-query-vertical-partitioning.md)</span></span>
* <span data-ttu-id="7a479-192">Podívejte se kurz vodorovné rozdělení do oddílů (horizontálního dělení) [Začínáme s elastické dotazu pro vodorovné rozdělení do oddílů (horizontálního dělení)](sql-database-elastic-query-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="7a479-192">For a horizontal partitioning (sharding) tutorial, see [Getting started with elastic query for horizontal partitioning (sharding)](sql-database-elastic-query-getting-started.md).</span></span>
* <span data-ttu-id="7a479-193">V tématu [sp\_provést \_vzdáleného](https://msdn.microsoft.com/library/mt703714) pro uloženou proceduru, který spouští příkaz jazyka Transact-SQL na jedné vzdálené databáze SQL Azure nebo sadu databází, které slouží jako horizontálních oddílů v vodorovné schéma rozdělení oddílů.</span><span class="sxs-lookup"><span data-stu-id="7a479-193">See [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) for a stored procedure that executes a Transact-SQL statement on a single remote Azure SQL Database or set of databases serving as shards in a horizontal partitioning scheme.</span></span>


<!--Image references-->
[1]: ./media/sql-database-elastic-query-horizontal-partitioning/horizontalpartitioning.png
<!--anchors-->
