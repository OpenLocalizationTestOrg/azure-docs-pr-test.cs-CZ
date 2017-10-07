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
# <a name="reporting-across-scaled-out-cloud-databases-preview"></a><span data-ttu-id="809d1-103">Vytváření sestav napříč instancemi cloudu databází (preview)</span><span class="sxs-lookup"><span data-stu-id="809d1-103">Reporting across scaled-out cloud databases (preview)</span></span>
![Dotazování mezi horizontálních oddílů][1]

<span data-ttu-id="809d1-105">Horizontálně dělené databáze distribuovat řádky napříč škálovaný dat vrstvy.</span><span class="sxs-lookup"><span data-stu-id="809d1-105">Sharded databases distribute rows across a scaled out data tier.</span></span> <span data-ttu-id="809d1-106">schéma Hello je stejná na všechny zúčastněné databáze, také známé jako vodorovné rozdělení do oddílů.</span><span class="sxs-lookup"><span data-stu-id="809d1-106">hello schema is identical on all participating databases, also known as horizontal partitioning.</span></span> <span data-ttu-id="809d1-107">Pomocí elastické dotazu, můžete vytvořit sestavy, které jsou rozmístěny všechny databáze v horizontálně dělené databázi.</span><span class="sxs-lookup"><span data-stu-id="809d1-107">Using an elastic query, you can create reports that span all databases in a sharded database.</span></span>

<span data-ttu-id="809d1-108">Rychle začít, najdete v části [vytváření sestav napříč instancemi cloudu databáze](sql-database-elastic-query-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="809d1-108">For a quick start, see [Reporting across scaled-out cloud databases](sql-database-elastic-query-getting-started.md).</span></span>

<span data-ttu-id="809d1-109">Horizontálně dělené databází, najdete v části [dotazu mezi databázemi cloudu s různými schématy](sql-database-elastic-query-vertical-partitioning.md).</span><span class="sxs-lookup"><span data-stu-id="809d1-109">For non-sharded databases, see [Query across cloud databases with different schemas](sql-database-elastic-query-vertical-partitioning.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="809d1-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="809d1-110">Prerequisites</span></span>
* <span data-ttu-id="809d1-111">Vytvoření mapy horizontálního oddílu pomocí klientské knihovny pro elastické databáze hello.</span><span class="sxs-lookup"><span data-stu-id="809d1-111">Create a shard map using hello elastic database client library.</span></span> <span data-ttu-id="809d1-112">v tématu [horizontálního oddílu mapy správu](sql-database-elastic-scale-shard-map-management.md).</span><span class="sxs-lookup"><span data-stu-id="809d1-112">see [Shard map management](sql-database-elastic-scale-shard-map-management.md).</span></span> <span data-ttu-id="809d1-113">Nebo použijte hello ukázkovou aplikaci v [začít pracovat s nástroji elastické databáze](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="809d1-113">Or use hello sample app in [Get started with elastic database tools](sql-database-elastic-scale-get-started.md).</span></span>
* <span data-ttu-id="809d1-114">Alternativně si zobrazte [migrací stávající databáze, databáze na více systémů tooscaled](sql-database-elastic-convert-to-use-elastic-tools.md).</span><span class="sxs-lookup"><span data-stu-id="809d1-114">Alternatively, see [Migrate existing databases tooscaled-out databases](sql-database-elastic-convert-to-use-elastic-tools.md).</span></span>
* <span data-ttu-id="809d1-115">Hello uživatel musí mít oprávnění ALTER ANY EXTERNAL DATA SOURCE.</span><span class="sxs-lookup"><span data-stu-id="809d1-115">hello user must possess ALTER ANY EXTERNAL DATA SOURCE permission.</span></span> <span data-ttu-id="809d1-116">Toto oprávnění je součástí hello oprávnění ALTER DATABASE.</span><span class="sxs-lookup"><span data-stu-id="809d1-116">This permission is included with hello ALTER DATABASE permission.</span></span>
* <span data-ttu-id="809d1-117">Oprávnění ALTER ANY externí zdroj dat se zdroji dat toohello potřebné toorefer.</span><span class="sxs-lookup"><span data-stu-id="809d1-117">ALTER ANY EXTERNAL DATA SOURCE permissions are needed toorefer toohello underlying data source.</span></span>

## <a name="overview"></a><span data-ttu-id="809d1-118">Přehled</span><span class="sxs-lookup"><span data-stu-id="809d1-118">Overview</span></span>
<span data-ttu-id="809d1-119">Tyto příkazy vytvořit reprezentaci metadata hello horizontálně dělené datové vrstvě v hello elastické dotaz do databáze.</span><span class="sxs-lookup"><span data-stu-id="809d1-119">These statements create hello metadata representation of your sharded data tier in hello elastic query database.</span></span> 

1. [<span data-ttu-id="809d1-120">VYTVOŘENÍ HLAVNÍHO KLÍČE</span><span class="sxs-lookup"><span data-stu-id="809d1-120">CREATE MASTER KEY</span></span>](https://msdn.microsoft.com/library/ms174382.aspx)
2. [<span data-ttu-id="809d1-121">VYTVOŘENÍ DATABÁZE OBOR PŘIHLAŠOVACÍCH ÚDAJŮ</span><span class="sxs-lookup"><span data-stu-id="809d1-121">CREATE DATABASE SCOPED CREDENTIAL</span></span>](https://msdn.microsoft.com/library/mt270260.aspx)
3. [<span data-ttu-id="809d1-122">VYTVOŘENÍ EXTERNÍHO ZDROJE DAT</span><span class="sxs-lookup"><span data-stu-id="809d1-122">CREATE EXTERNAL DATA SOURCE</span></span>](https://msdn.microsoft.com/library/dn935022.aspx)
4. [<span data-ttu-id="809d1-123">CREATE EXTERNAL TABLE</span><span class="sxs-lookup"><span data-stu-id="809d1-123">CREATE EXTERNAL TABLE</span></span>](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="11-create-database-scoped-master-key-and-credentials"></a><span data-ttu-id="809d1-124">1.1 vytvořit hlavní klíč databáze obor a přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="809d1-124">1.1 Create database scoped master key and credentials</span></span>
<span data-ttu-id="809d1-125">Hello pověření je používáno hello elastické dotazu tooconnect tooyour vzdálené databáze.</span><span class="sxs-lookup"><span data-stu-id="809d1-125">hello credential is used by hello elastic query tooconnect tooyour remote databases.</span></span>  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]

> [!NOTE]
> <span data-ttu-id="809d1-126">Ujistěte se, že hello *"\<uživatelské jméno\>"* neobsahuje žádné *"@servername"* příponu.</span><span class="sxs-lookup"><span data-stu-id="809d1-126">Make sure that hello *"\<username\>"* does not include any *"@servername"* suffix.</span></span> 
> 
> 

## <a name="12-create-external-data-sources"></a><span data-ttu-id="809d1-127">1.2 Vytvoření externích zdrojů dat.</span><span class="sxs-lookup"><span data-stu-id="809d1-127">1.2 Create external data sources</span></span>
<span data-ttu-id="809d1-128">Syntaxe:</span><span class="sxs-lookup"><span data-stu-id="809d1-128">Syntax:</span></span>

    <External_Data_Source> ::=    
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH                                              
            (TYPE = SHARD_MAP_MANAGER,
                       LOCATION = '<fully_qualified_server_name>',
            DATABASE_NAME = ‘<shardmap_database_name>',
            CREDENTIAL = <credential_name>, 
            SHARD_MAP_NAME = ‘<shardmapname>’ 
                   ) [;] 

### <a name="example"></a><span data-ttu-id="809d1-129">Příklad</span><span class="sxs-lookup"><span data-stu-id="809d1-129">Example</span></span>
    CREATE EXTERNAL DATA SOURCE MyExtSrc 
    WITH 
    ( 
        TYPE=SHARD_MAP_MANAGER,
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ShardMapDatabase', 
        CREDENTIAL= SMMUser, 
        SHARD_MAP_NAME='ShardMap' 
    );

<span data-ttu-id="809d1-130">Načtení hello seznam aktuálních zdrojů externích dat:</span><span class="sxs-lookup"><span data-stu-id="809d1-130">Retrieve hello list of current external data sources:</span></span> 

    select * from sys.external_data_sources; 

<span data-ttu-id="809d1-131">Hello externí zdroj dat odkazuje na mapě horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="809d1-131">hello external data source references your shard map.</span></span> <span data-ttu-id="809d1-132">Dotaz elastické pak použije hello externí zdroj dat a hello základní horizontálního oddílu mapy tooenumerate hello databáze, které účastnit hello datové vrstvy.</span><span class="sxs-lookup"><span data-stu-id="809d1-132">An elastic query then uses hello external data source and hello underlying shard map tooenumerate hello databases that participate in hello data tier.</span></span> <span data-ttu-id="809d1-133">Hello stejné přihlašovací údaje jsou použité tooread hello horizontálního oddílu mapy a tooaccess hello data na horizontálních oddílů hello během zpracování hello elastické dotazu.</span><span class="sxs-lookup"><span data-stu-id="809d1-133">hello same credentials are used tooread hello shard map and tooaccess hello data on hello shards during hello processing of an elastic query.</span></span> 

## <a name="13-create-external-tables"></a><span data-ttu-id="809d1-134">1.3 Vytvoření externí tabulky</span><span class="sxs-lookup"><span data-stu-id="809d1-134">1.3 Create external tables</span></span>
<span data-ttu-id="809d1-135">Syntaxe:</span><span class="sxs-lookup"><span data-stu-id="809d1-135">Syntax:</span></span>  

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name  
        ( { <column_definition> } [ ,...n ])     
        { WITH ( <sharded_external_table_options> ) }
    ) [;]  

    <sharded_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>,       
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 
      DISTRIBUTION = SHARDED(<sharding_column_name>) | REPLICATED |ROUND_ROBIN

<span data-ttu-id="809d1-136">**Příklad**</span><span class="sxs-lookup"><span data-stu-id="809d1-136">**Example**</span></span>

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

<span data-ttu-id="809d1-137">Načtení seznamu hello externí tabulky z aktuální databáze hello:</span><span class="sxs-lookup"><span data-stu-id="809d1-137">Retrieve hello list of external tables from hello current database:</span></span> 

    SELECT * from sys.external_tables; 

<span data-ttu-id="809d1-138">externí tabulky toodrop:</span><span class="sxs-lookup"><span data-stu-id="809d1-138">toodrop external tables:</span></span>

    DROP EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name[;]

### <a name="remarks"></a><span data-ttu-id="809d1-139">Poznámky</span><span class="sxs-lookup"><span data-stu-id="809d1-139">Remarks</span></span>
<span data-ttu-id="809d1-140">Hello DATA\_zdroj klauzule definuje hello externí zdroj dat (horizontálních map) používaný pro externí tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="809d1-140">hello DATA\_SOURCE clause defines hello external data source (a shard map) that is used for hello external table.</span></span>  

<span data-ttu-id="809d1-141">Hello schématu\_název a OBJEKT\_název klauzule mapování hello externí tooa tabulky v jiném schématu.</span><span class="sxs-lookup"><span data-stu-id="809d1-141">hello SCHEMA\_NAME and OBJECT\_NAME clauses map hello external table definition tooa table in a different schema.</span></span> <span data-ttu-id="809d1-142">Pokud tento parametr vynechán, hello schématu vzdálené objektu hello se předpokládá, že toobe "dbo" a jeho název je předpokládá, že název externí tabulky identické toohello toobe definovaný.</span><span class="sxs-lookup"><span data-stu-id="809d1-142">If omitted, hello schema of hello remote object is assumed toobe “dbo” and its name is assumed toobe identical toohello external table name being defined.</span></span> <span data-ttu-id="809d1-143">To je užitečné, když se název hello Vzdálená tabulka už používá v databázi hello místo toocreate hello externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="809d1-143">This is useful if hello name of your remote table is already taken in hello database where you want toocreate hello external table.</span></span> <span data-ttu-id="809d1-144">Například chcete toodefine externí tabulky tooget agregované zobrazení katalogu zobrazení nebo zobrazení dynamické správy na škálovaný dat vrstvy.</span><span class="sxs-lookup"><span data-stu-id="809d1-144">For  example, you want toodefine an external table tooget an aggregate view of catalog views or DMVs on your scaled out data tier.</span></span> <span data-ttu-id="809d1-145">Vzhledem k tomu, že zobrazení katalogu a zobrazení dynamické správy již existují místně, nemůžete použít jejich názvy pro definici externí tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="809d1-145">Since catalog views and DMVs already exist locally, you cannot use their names for hello external table definition.</span></span> <span data-ttu-id="809d1-146">Místo toho použijte jiný název a použijte zobrazení katalogu hello nebo hello DMV na název v hello schématu\_název nebo OBJEKT\_klauzule název.</span><span class="sxs-lookup"><span data-stu-id="809d1-146">Instead, use a different name and use hello catalog view’s or hello DMV’s name in hello SCHEMA\_NAME and/or OBJECT\_NAME clauses.</span></span> <span data-ttu-id="809d1-147">(Viz následující příklad hello.)</span><span class="sxs-lookup"><span data-stu-id="809d1-147">(See hello example below.)</span></span> 

<span data-ttu-id="809d1-148">klauzule distribuční Hello Určuje distribuci dat hello používá pro tuto tabulku.</span><span class="sxs-lookup"><span data-stu-id="809d1-148">hello DISTRIBUTION clause specifies hello data distribution used for this table.</span></span> <span data-ttu-id="809d1-149">Procesor dotazů Hello využívá hello informací uvedených v hello distribuční klauzule toobuild hello nejúčinnější plány dotazů.</span><span class="sxs-lookup"><span data-stu-id="809d1-149">hello query processor utilizes hello information provided in hello DISTRIBUTION clause toobuild hello most efficient query plans.</span></span>  

1. <span data-ttu-id="809d1-150">**Horizontálně DĚLENÉ** znamená data vodorovně rozdělena mezi databázemi hello.</span><span class="sxs-lookup"><span data-stu-id="809d1-150">**SHARDED** means data is horizontally partitioned across hello databases.</span></span> <span data-ttu-id="809d1-151">Hello klíč rozdělení do oddílů pro distribuci dat hello je hello **< sharding_column_name >** parametr.</span><span class="sxs-lookup"><span data-stu-id="809d1-151">hello partitioning key for hello data distribution is hello **<sharding_column_name>** parameter.</span></span>
2. <span data-ttu-id="809d1-152">**REPLIKOVAT** znamená, že jsou identické kopií tabulky hello na každou databázi.</span><span class="sxs-lookup"><span data-stu-id="809d1-152">**REPLICATED** means that identical copies of hello table are present on each database.</span></span> <span data-ttu-id="809d1-153">Je vaše odpovědnosti tooensure, hello repliky se shodují mezi databázemi hello.</span><span class="sxs-lookup"><span data-stu-id="809d1-153">It is your responsibility tooensure that hello replicas are identical across hello databases.</span></span>
3. <span data-ttu-id="809d1-154">**ZAOKROUHLÍ\_každý s každým** znamená, že hello tabulka vodorovně rozdělena na oddíly pomocí metody distribuce závislé aplikace.</span><span class="sxs-lookup"><span data-stu-id="809d1-154">**ROUND\_ROBIN** means that hello table is horizontally partitioned using an application-dependent distribution method.</span></span> 

<span data-ttu-id="809d1-155">**Datové vrstvy odkaz**: tooan externí zdroj dat odkazuje hello DDL pro externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="809d1-155">**Data tier reference**: hello external table DDL refers tooan external data source.</span></span> <span data-ttu-id="809d1-156">Hello externí zdroj dat určuje horizontálního oddílu mapu, která poskytuje hello externí tabulky s hello informace nezbytné toolocate všechny hello databáze v datové vrstvě.</span><span class="sxs-lookup"><span data-stu-id="809d1-156">hello external data source specifies a shard map which provides hello external table with hello information necessary toolocate all hello databases in your data tier.</span></span> 

### <a name="security-considerations"></a><span data-ttu-id="809d1-157">Aspekty zabezpečení</span><span class="sxs-lookup"><span data-stu-id="809d1-157">Security considerations</span></span>
<span data-ttu-id="809d1-158">Uživatelé s externí tabulky toohello přístupu automaticky získají přístup toohello základní vzdálených tabulek v rámci hello přihlašovací údaje zadané v definici zdrojové externích dat hello.</span><span class="sxs-lookup"><span data-stu-id="809d1-158">Users with access toohello external table automatically gain access toohello underlying remote tables under hello credential given in hello external data source definition.</span></span> <span data-ttu-id="809d1-159">Vyhněte se nežádoucí zvýšení oprávnění prostřednictvím hello externí zdroj dat hello přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="809d1-159">Avoid undesired elevation of privileges through hello credential of hello external data source.</span></span> <span data-ttu-id="809d1-160">Pomocí GRANT nebo REVOKE pro externí tabulky stejně, jako by šlo o běžnou tabulku.</span><span class="sxs-lookup"><span data-stu-id="809d1-160">Use GRANT or REVOKE for an external table just as though it were a regular table.</span></span>  

<span data-ttu-id="809d1-161">Jakmile definujete zdroj externích dat a externí tabulky, teď můžete použít úplnou T-SQL na externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="809d1-161">Once you have defined your external data source and your external tables, you can now use full T-SQL over your external tables.</span></span>

## <a name="example-querying-horizontal-partitioned-databases"></a><span data-ttu-id="809d1-162">Příklad: dotazy na vodorovné oddílů databáze</span><span class="sxs-lookup"><span data-stu-id="809d1-162">Example: querying horizontal partitioned databases</span></span>
<span data-ttu-id="809d1-163">Hello následující dotaz spojí třícestný sklady a pořadí řádky objednávek a používá několik agregace a selektivní filtru.</span><span class="sxs-lookup"><span data-stu-id="809d1-163">hello following query performs a three-way join between warehouses, orders and order lines and uses several aggregates and a selective filter.</span></span> <span data-ttu-id="809d1-164">Předpokládá (1) vodorovné rozdělení (horizontálního dělení) a (2) zda sklady, objednávek a pořadí řádky jsou horizontálně dělené podle hello skladu id sloupce, a můžete takový dotaz elastické hello společně umísťovat hello spojení na hello horizontálních oddílů a zpracovat hello nákladné součást hello dotazu na hello horizontálních oddílů paralelně.</span><span class="sxs-lookup"><span data-stu-id="809d1-164">It assumes (1) horizontal partitioning (sharding) and (2) that warehouses, orders and order lines are sharded by hello warehouse id column, and that hello elastic query can co-locate hello joins on hello shards and process hello expensive part of hello query on hello shards in parallel.</span></span> 

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

## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a><span data-ttu-id="809d1-165">Uložené procedury pro vzdálené spuštění T-SQL: sp\_execute_remote</span><span class="sxs-lookup"><span data-stu-id="809d1-165">Stored procedure for remote T-SQL execution: sp\_execute_remote</span></span>
<span data-ttu-id="809d1-166">Elastické dotazu také zavádí uložené procedury, která poskytuje přímý přístup toohello horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="809d1-166">Elastic query also introduces a stored procedure that provides direct access toohello shards.</span></span> <span data-ttu-id="809d1-167">Hello uložená procedura je volána [sp\_provést \_vzdáleného](https://msdn.microsoft.com/library/mt703714) a může být vzdálené uložené procedury používané tooexecute nebo kód T-SQL na hello vzdálené databáze.</span><span class="sxs-lookup"><span data-stu-id="809d1-167">hello stored procedure is called [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) and can be used tooexecute remote stored procedures or T-SQL code on hello remote databases.</span></span> <span data-ttu-id="809d1-168">Jak dlouho trvá hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="809d1-168">It takes hello following parameters:</span></span> 

* <span data-ttu-id="809d1-169">Název zdroje dat (nvarchar): název hello hello externí zdroj dat typu relační.</span><span class="sxs-lookup"><span data-stu-id="809d1-169">Data source name (nvarchar): hello name of hello external data source of type RDBMS.</span></span> 
* <span data-ttu-id="809d1-170">Dotaz (nvarchar): toobe hello T-SQL dotaz spustit na každém horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="809d1-170">Query (nvarchar): hello T-SQL query toobe executed on each shard.</span></span> 
* <span data-ttu-id="809d1-171">Deklarace parametru (nvarchar) - volitelné: řetězec s definice typu dat pro hello parametry použité v parametru dotazu hello (např. sp_executesql).</span><span class="sxs-lookup"><span data-stu-id="809d1-171">Parameter declaration (nvarchar) - optional: String with data type definitions for hello parameters used in hello Query parameter (like sp_executesql).</span></span> 
* <span data-ttu-id="809d1-172">Seznam hodnot parametru - volitelné: čárkami oddělený seznam hodnot parametrů (např. sp_executesql).</span><span class="sxs-lookup"><span data-stu-id="809d1-172">Parameter value list - optional: Comma-separated list of parameter values (like sp_executesql).</span></span>

<span data-ttu-id="809d1-173">Hello sp\_provést\_vzdálené používá hello externí zdroj dat součástí hello volání parametry tooexecute hello zadaný příkaz jazyka T-SQL na hello vzdálené databáze.</span><span class="sxs-lookup"><span data-stu-id="809d1-173">hello sp\_execute\_remote uses hello external data source provided in hello invocation parameters tooexecute hello given T-SQL statement on hello remote databases.</span></span> <span data-ttu-id="809d1-174">Používá hello přihlašovací údaje správce databáze shardmap tooconnect toohello hello externí datového zdroje a hello vzdálené databáze.</span><span class="sxs-lookup"><span data-stu-id="809d1-174">It uses hello credential of hello external data source tooconnect toohello shardmap manager database and hello remote databases.</span></span>  

<span data-ttu-id="809d1-175">Příklad:</span><span class="sxs-lookup"><span data-stu-id="809d1-175">Example:</span></span> 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 

## <a name="connectivity-for-tools"></a><span data-ttu-id="809d1-176">Připojení nástroje</span><span class="sxs-lookup"><span data-stu-id="809d1-176">Connectivity for tools</span></span>
<span data-ttu-id="809d1-177">Používat regulární tooconnect řetězce připojení SQL Server aplikace, vaše data a BI integrace nástroje toohello databáze s definic externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="809d1-177">Use regular SQL Server connection strings tooconnect your application, your BI and data integration tools toohello database with your external table definitions.</span></span> <span data-ttu-id="809d1-178">Ujistěte se, že systém SQL Server je podporovaný jako zdroj dat pro vaše nástroje.</span><span class="sxs-lookup"><span data-stu-id="809d1-178">Make sure that SQL Server is supported as a data source for your tool.</span></span> <span data-ttu-id="809d1-179">Pak odkazovat hello elastické dotaz do databáze jako jakýkoli jiný nástroj toohello systému SQL Server databáze připojené a použít externí tabulky z nástroje nebo aplikace, jako kdyby byly místní tabulky.</span><span class="sxs-lookup"><span data-stu-id="809d1-179">Then reference hello elastic query database like any other SQL Server database connected toohello tool, and use external tables from your tool or application as if they were local tables.</span></span> 

## <a name="best-practices"></a><span data-ttu-id="809d1-180">Osvědčené postupy</span><span class="sxs-lookup"><span data-stu-id="809d1-180">Best practices</span></span>
* <span data-ttu-id="809d1-181">Zajistěte, že hello elastické dotazu koncový bod databáze nebyla zadána databáze shardmap toohello přístupu a všechny horizontálních oddílů prostřednictvím brány firewall SQL DB hello.</span><span class="sxs-lookup"><span data-stu-id="809d1-181">Ensure that hello elastic query endpoint database has been given access toohello shardmap database and all shards through hello SQL DB firewalls.</span></span>  
* <span data-ttu-id="809d1-182">Ověřit nebo vynutit hello distribuci dat definované hello externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="809d1-182">Validate or enforce hello data distribution defined by hello external table.</span></span> <span data-ttu-id="809d1-183">Pokud distribuční skutečná data se liší od hello distribuční zadaný v definici vaší tabulky, vaše dotazy mohou vést k neočekávaným výsledkům.</span><span class="sxs-lookup"><span data-stu-id="809d1-183">If your actual data distribution is different from hello distribution specified in your table definition, your queries may yield unexpected results.</span></span> 
* <span data-ttu-id="809d1-184">Elastické dotazu aktuálně neprovádí odstranění horizontálních při predikáty nad klíčem horizontálního dělení hello by umožnilo toosafely vyloučení určitých horizontálních oddílů z zpracování.</span><span class="sxs-lookup"><span data-stu-id="809d1-184">Elastic query currently does not perform shard elimination when predicates over hello sharding key would allow it toosafely exclude certain shards from processing.</span></span>
* <span data-ttu-id="809d1-185">Elastické dotazu je nejvhodnější pro dotazy kde lze provést většinu výpočtu hello na hello horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="809d1-185">Elastic query works best for queries where most of hello computation can be done on hello shards.</span></span> <span data-ttu-id="809d1-186">Obvykle získat hello nejlepší výkon dotazů s predikáty selektivní filtru, které lze vyhodnotit na hello horizontálních oddílů nebo spojení přes hello dělení klíče, které lze provést tak oddílu zarovnaný na všechny horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="809d1-186">You typically get hello best query performance with selective filter predicates that can be evaluated on hello shards or joins over hello partitioning keys that can be performed in a partition-aligned way on all shards.</span></span> <span data-ttu-id="809d1-187">Další vzory dotazu může být nutné tooload velkých objemů dat z hlavního uzlu toohello hello horizontálních oddílů a může být špatná</span><span class="sxs-lookup"><span data-stu-id="809d1-187">Other query patterns may need tooload large amounts of data from hello shards toohello head node and may perform poorly</span></span>

## <a name="next-steps"></a><span data-ttu-id="809d1-188">Další kroky</span><span class="sxs-lookup"><span data-stu-id="809d1-188">Next steps</span></span>

* <span data-ttu-id="809d1-189">Přehled elastické dotazů najdete v tématu [elastické dotazu přehled](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="809d1-189">For an overview of elastic query, see [Elastic query overview](sql-database-elastic-query-overview.md).</span></span>
* <span data-ttu-id="809d1-190">Vertikální dělení kurzu, najdete v části [Začínáme s mezidatabázové dotazu (vertikální dělení)](sql-database-elastic-query-getting-started-vertical.md).</span><span class="sxs-lookup"><span data-stu-id="809d1-190">For a vertical partitioning tutorial, see [Getting started with cross-database query (vertical partitioning)](sql-database-elastic-query-getting-started-vertical.md).</span></span>
* <span data-ttu-id="809d1-191">Syntaxe a ukázkové dotazy pro svisle oddílů data, najdete v části [dotazování svisle na oddíly dat)](sql-database-elastic-query-vertical-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="809d1-191">For syntax and sample queries for vertically partitioned data, see [Querying vertically partitioned data)](sql-database-elastic-query-vertical-partitioning.md)</span></span>
* <span data-ttu-id="809d1-192">Podívejte se kurz vodorovné rozdělení do oddílů (horizontálního dělení) [Začínáme s elastické dotazu pro vodorovné rozdělení do oddílů (horizontálního dělení)](sql-database-elastic-query-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="809d1-192">For a horizontal partitioning (sharding) tutorial, see [Getting started with elastic query for horizontal partitioning (sharding)](sql-database-elastic-query-getting-started.md).</span></span>
* <span data-ttu-id="809d1-193">V tématu [sp\_provést \_vzdáleného](https://msdn.microsoft.com/library/mt703714) pro uloženou proceduru, který spouští příkaz jazyka Transact-SQL na jedné vzdálené databáze SQL Azure nebo sadu databází, které slouží jako horizontálních oddílů v vodorovné schéma rozdělení oddílů.</span><span class="sxs-lookup"><span data-stu-id="809d1-193">See [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) for a stored procedure that executes a Transact-SQL statement on a single remote Azure SQL Database or set of databases serving as shards in a horizontal partitioning scheme.</span></span>


<!--Image references-->
[1]: ./media/sql-database-elastic-query-horizontal-partitioning/horizontalpartitioning.png
<!--anchors-->
