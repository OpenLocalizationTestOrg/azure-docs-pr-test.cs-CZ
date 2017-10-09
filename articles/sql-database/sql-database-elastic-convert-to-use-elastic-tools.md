---
title: "aaaMigrate existující databáze na více systémů tooscale | Microsoft Docs"
description: "Převést nástroje elastické databáze toouse horizontálně dělené databáze tak, že vytvoříte manažera horizontálního oddílu mapy"
services: sql-database
documentationcenter: 
author: ddove
manager: jhubbard
editor: 
ms.assetid: 8c851d8e-8fd5-4327-89c1-9178b20ddd69
ms.service: sql-database
ms.custom: scale out apps
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: fa2c9e3699f30667cf547d1faadf4504609199be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-existing-databases-tooscale-out"></a><span data-ttu-id="2125f-103">Migrovat existující databáze tooscale na více systémů</span><span class="sxs-lookup"><span data-stu-id="2125f-103">Migrate existing databases tooscale-out</span></span>
<span data-ttu-id="2125f-104">Snadno spravovat existující upraveným horizontálně dělené databáze pomocí nástrojů databáze Azure SQL Database (například hello [klientské knihovny pro elastické databáze](sql-database-elastic-database-client-library.md)).</span><span class="sxs-lookup"><span data-stu-id="2125f-104">Easily manage your existing scaled-out sharded databases using Azure SQL Database database tools (such as hello [Elastic Database client library](sql-database-elastic-database-client-library.md)).</span></span> <span data-ttu-id="2125f-105">Je nutné nejprve převést existující sady databází toouse hello [správce mapy horizontálního oddílu](sql-database-elastic-scale-shard-map-management.md).</span><span class="sxs-lookup"><span data-stu-id="2125f-105">You must first convert an existing set of databases toouse hello [shard map manager](sql-database-elastic-scale-shard-map-management.md).</span></span> 

## <a name="overview"></a><span data-ttu-id="2125f-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="2125f-106">Overview</span></span>
<span data-ttu-id="2125f-107">toomigrate existující horizontálně dělené databázi:</span><span class="sxs-lookup"><span data-stu-id="2125f-107">toomigrate an existing sharded database:</span></span> 

1. <span data-ttu-id="2125f-108">Příprava hello [horizontálního oddílu mapa správce databáze](sql-database-elastic-scale-shard-map-management.md).</span><span class="sxs-lookup"><span data-stu-id="2125f-108">Prepare hello [shard map manager database](sql-database-elastic-scale-shard-map-management.md).</span></span>
2. <span data-ttu-id="2125f-109">Vytvoření mapy hello horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="2125f-109">Create hello shard map.</span></span>
3. <span data-ttu-id="2125f-110">Připravte hello jednotlivých horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="2125f-110">Prepare hello individual shards.</span></span>  
4. <span data-ttu-id="2125f-111">Přidáte mapu horizontálního oddílu toohello mapování.</span><span class="sxs-lookup"><span data-stu-id="2125f-111">Add mappings toohello shard map.</span></span>

<span data-ttu-id="2125f-112">Tyto postupy můžou být implementovaná pomocí buď hello [Klientská knihovna pro rozhraní .NET Framework](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/), nebo skripty prostředí PowerShell hello nalezený na [Azure SQL DB - skripty nástroje elastické databáze](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span><span class="sxs-lookup"><span data-stu-id="2125f-112">These techniques can be implemented using either hello [.NET Framework client library](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/), or hello PowerShell scripts found at [Azure SQL DB - Elastic Database tools scripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span></span> <span data-ttu-id="2125f-113">Zde Hello příklady použití skriptů prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="2125f-113">hello examples here use hello PowerShell scripts.</span></span>

<span data-ttu-id="2125f-114">Další informace o hello ShardMapManager najdete v tématu [horizontálního oddílu mapy správu](sql-database-elastic-scale-shard-map-management.md).</span><span class="sxs-lookup"><span data-stu-id="2125f-114">For more information about hello ShardMapManager, see [Shard map management](sql-database-elastic-scale-shard-map-management.md).</span></span> <span data-ttu-id="2125f-115">Přehled nástroje elastické databáze hello najdete v tématu [přehled funkcí elastické databáze](sql-database-elastic-scale-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2125f-115">For an overview of hello elastic database tools, see [Elastic Database features overview](sql-database-elastic-scale-introduction.md).</span></span>

## <a name="prepare-hello-shard-map-manager-database"></a><span data-ttu-id="2125f-116">Příprava hello horizontálního oddílu mapa správce databáze</span><span class="sxs-lookup"><span data-stu-id="2125f-116">Prepare hello shard map manager database</span></span>
<span data-ttu-id="2125f-117">Hello horizontálního oddílu mapa správce je speciální databázi, která obsahuje hello data toomanage upraveným databáze.</span><span class="sxs-lookup"><span data-stu-id="2125f-117">hello shard map manager is a special database that contains hello data toomanage scaled-out databases.</span></span> <span data-ttu-id="2125f-118">Můžete použít existující databázi nebo vytvořte novou databázi.</span><span class="sxs-lookup"><span data-stu-id="2125f-118">You can use an existing database, or create a new database.</span></span> <span data-ttu-id="2125f-119">Všimněte si, že databáze, který funguje jako správce mapy horizontálního oddílu by neměl být hello stejné databázi jako horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="2125f-119">Note that a database acting as shard map manager should not be hello same database as a shard.</span></span> <span data-ttu-id="2125f-120">Všimněte si také, že skript prostředí PowerShell hello nevytvoří hello databáze za vás.</span><span class="sxs-lookup"><span data-stu-id="2125f-120">Also note that hello PowerShell script does not create hello database for you.</span></span> 

## <a name="step-1-create-a-shard-map-manager"></a><span data-ttu-id="2125f-121">Krok 1: vytvoření horizontálního oddílu správce mapy</span><span class="sxs-lookup"><span data-stu-id="2125f-121">Step 1: create a shard map manager</span></span>
    # Create a shard map manager. 
    New-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 
    #<server_name> and <smm_db_name> are hello server name and database name 
    # for hello new or existing database that should be used for storing 
    # tenant-database mapping information.

### <a name="tooretrieve-hello-shard-map-manager"></a><span data-ttu-id="2125f-122">Správce tooretrieve hello horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="2125f-122">tooretrieve hello shard map manager</span></span>
<span data-ttu-id="2125f-123">Po vytvoření můžete načíst hello horizontálního oddílu mapy manager pomocí této rutiny.</span><span class="sxs-lookup"><span data-stu-id="2125f-123">After creation, you can retrieve hello shard map manager with this cmdlet.</span></span> <span data-ttu-id="2125f-124">Tento krok je nutný pokaždé, když potřebujete toouse hello ShardMapManager objektu.</span><span class="sxs-lookup"><span data-stu-id="2125f-124">This step is needed every time you need toouse hello ShardMapManager object.</span></span>

    # Try tooget a reference toohello Shard Map Manager  
    $ShardMapManager = Get-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 


## <a name="step-2-create-hello-shard-map"></a><span data-ttu-id="2125f-125">Krok 2: vytvoření mapy horizontálního oddílu hello</span><span class="sxs-lookup"><span data-stu-id="2125f-125">Step 2: create hello shard map</span></span>
<span data-ttu-id="2125f-126">Je nutné vybrat typ hello toocreate mapy horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="2125f-126">You must select hello type of shard map toocreate.</span></span> <span data-ttu-id="2125f-127">Volba Hello závisí na architektuře databáze hello:</span><span class="sxs-lookup"><span data-stu-id="2125f-127">hello choice depends on hello database architecture:</span></span> 

1. <span data-ttu-id="2125f-128">Jednoho klienta na databázi (podmínky, najdete v části hello [Glosář](sql-database-elastic-scale-glossary.md).)</span><span class="sxs-lookup"><span data-stu-id="2125f-128">Single tenant per database (For terms, see hello [glossary](sql-database-elastic-scale-glossary.md).)</span></span> 
2. <span data-ttu-id="2125f-129">Několik klientů na databázi (dva typy):</span><span class="sxs-lookup"><span data-stu-id="2125f-129">Multiple tenants per database (two types):</span></span>
   1. <span data-ttu-id="2125f-130">Seznam mapování</span><span class="sxs-lookup"><span data-stu-id="2125f-130">List mapping</span></span>
   2. <span data-ttu-id="2125f-131">Mapování rozsahu</span><span class="sxs-lookup"><span data-stu-id="2125f-131">Range mapping</span></span>

<span data-ttu-id="2125f-132">Pro jednoho klienta modelu, vytvořit **seznamu mapování** horizontálního oddílu mapy.</span><span class="sxs-lookup"><span data-stu-id="2125f-132">For a single-tenant model, create a **list mapping** shard map.</span></span> <span data-ttu-id="2125f-133">model jednoho klienta Hello přiřadí jednu databázi na každého klienta.</span><span class="sxs-lookup"><span data-stu-id="2125f-133">hello single-tenant model assigns one database per tenant.</span></span> <span data-ttu-id="2125f-134">Jedná se efektivní model pro vývojáře SaaS, protože ho zjednodušuje správu.</span><span class="sxs-lookup"><span data-stu-id="2125f-134">This is an effective model for SaaS developers as it simplifies management.</span></span>

![Seznam mapování][1]

<span data-ttu-id="2125f-136">Hello víceklientského modelu přiřadí několik klientů tooa jedné databáze (a skupin klientů, které můžete distribuovat mezi více databází).</span><span class="sxs-lookup"><span data-stu-id="2125f-136">hello multi-tenant model assigns several tenants tooa single database (and you can distribute groups of tenants across multiple databases).</span></span> <span data-ttu-id="2125f-137">Pomocí tohoto modelu, pokud očekáváte, že je každý klient toohave malá data.</span><span class="sxs-lookup"><span data-stu-id="2125f-137">Use this model when you expect each tenant toohave small data needs.</span></span> <span data-ttu-id="2125f-138">V tomto modelu jsme přiřadit rozsah klientů tooa databázi pomocí **rozsah mapování**.</span><span class="sxs-lookup"><span data-stu-id="2125f-138">In this model, we assign a range of tenants tooa database using **range mapping**.</span></span> 

![Mapování rozsahu][2]

<span data-ttu-id="2125f-140">Nebo můžete implementovat s použitím modelu víceklientské databáze *seznamu mapování* tooassign více klientů tooa jedné databáze.</span><span class="sxs-lookup"><span data-stu-id="2125f-140">Or you can implement a multi-tenant database model using a *list mapping* tooassign multiple tenants tooa single database.</span></span> <span data-ttu-id="2125f-141">Například DB1 je použité toostore informace o id klienta 1 a 5 a DB2 ukládá data pro klienta 7 a klienta 10.</span><span class="sxs-lookup"><span data-stu-id="2125f-141">For example, DB1 is used toostore information about tenant id 1 and 5, and DB2 stores data for tenant 7 and tenant 10.</span></span> 

![Vnořením klientů na jednom DB][3] 

<span data-ttu-id="2125f-143">**Podle své volby, vyberte jednu z těchto možností:**</span><span class="sxs-lookup"><span data-stu-id="2125f-143">**Based on your choice, choose one of these options:**</span></span>

### <a name="option-1-create-a-shard-map-for-a-list-mapping"></a><span data-ttu-id="2125f-144">Možnost 1: vytvoření mapy horizontálního oddílu pro seznam mapování</span><span class="sxs-lookup"><span data-stu-id="2125f-144">Option 1: create a shard map for a list mapping</span></span>
<span data-ttu-id="2125f-145">Vytvoření mapy horizontálního oddílu pomocí objektu ShardMapManager hello.</span><span class="sxs-lookup"><span data-stu-id="2125f-145">Create a shard map using hello ShardMapManager object.</span></span> 

    # $ShardMapManager is hello shard map manager object. 
    $ShardMap = New-ListShardMap -KeyType $([int]) 
    -ListShardMapName 'ListShardMap' 
    -ShardMapManager $ShardMapManager 


### <a name="option-2-create-a-shard-map-for-a-range-mapping"></a><span data-ttu-id="2125f-146">Možnost 2: vytvoření mapy horizontálního oddílu pro mapování rozsahu</span><span class="sxs-lookup"><span data-stu-id="2125f-146">Option 2: create a shard map for a range mapping</span></span>
<span data-ttu-id="2125f-147">Všimněte si, že tooutilize tento vzor mapování, hodnoty id klienta musí toobe průběžné rozsahy a je přijatelné toohave mezera v oblastech hello jednoduše přeskočením hello rozsahu při vytváření databází hello.</span><span class="sxs-lookup"><span data-stu-id="2125f-147">Note that tooutilize this mapping pattern, tenant id values needs toobe continuous ranges, and it is acceptable toohave gap in hello ranges by simply skipping hello range when creating hello databases.</span></span>

    # $ShardMapManager is hello shard map manager object 
    # 'RangeShardMap' is hello unique identifier for hello range shard map.  
    $ShardMap = New-RangeShardMap 
    -KeyType $([int]) 
    -RangeShardMapName 'RangeShardMap' 
    -ShardMapManager $ShardMapManager 

### <a name="option-3-list-mappings-on-a-single-database"></a><span data-ttu-id="2125f-148">Možnost 3: Seznam mapování na jedné databáze</span><span class="sxs-lookup"><span data-stu-id="2125f-148">Option 3: List mappings on a single database</span></span>
<span data-ttu-id="2125f-149">Nastavení tohoto vzoru také vyžaduje vytvoření seznamu mapování, jak je znázorněno v kroku 2, možnost 1.</span><span class="sxs-lookup"><span data-stu-id="2125f-149">Setting up this pattern also requires creation of a list map as shown in step 2, option 1.</span></span>

## <a name="step-3-prepare-individual-shards"></a><span data-ttu-id="2125f-150">Krok 3: Příprava jednotlivých horizontálních oddílů</span><span class="sxs-lookup"><span data-stu-id="2125f-150">Step 3: Prepare individual shards</span></span>
<span data-ttu-id="2125f-151">Přidejte každou horizontálního oddílu (databáze) toohello horizontálního oddílu mapy správci.</span><span class="sxs-lookup"><span data-stu-id="2125f-151">Add each shard (database) toohello shard map manager.</span></span> <span data-ttu-id="2125f-152">To připraví hello jednotlivé databáze pro ukládání informací o mapování.</span><span class="sxs-lookup"><span data-stu-id="2125f-152">This prepares hello individual databases for storing mapping information.</span></span> <span data-ttu-id="2125f-153">Tato metoda spusťte na každém horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="2125f-153">Execute this method on each shard.</span></span>

    Add-Shard 
    -ShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>'
    # hello $ShardMap is hello shard map created in step 2.


## <a name="step-4-add-mappings"></a><span data-ttu-id="2125f-154">Krok 4: Přidejte mapování</span><span class="sxs-lookup"><span data-stu-id="2125f-154">Step 4: Add mappings</span></span>
<span data-ttu-id="2125f-155">Přidání Hello mapování závisí na druhu hello mapy horizontálního oddílu, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="2125f-155">hello addition of mappings depends on hello kind of shard map you created.</span></span> <span data-ttu-id="2125f-156">Pokud jste vytvořili seznam mapy, můžete přidat mapování seznamu.</span><span class="sxs-lookup"><span data-stu-id="2125f-156">If you created a list map, you add list mappings.</span></span> <span data-ttu-id="2125f-157">Pokud jste vytvořili mapu rozsahu, můžete přidat mapování rozsahu.</span><span class="sxs-lookup"><span data-stu-id="2125f-157">If you created a range map, you add range mappings.</span></span>

### <a name="option-1-map-hello-data-for-a-list-mapping"></a><span data-ttu-id="2125f-158">Možnost 1: mapování hello dat pro mapování seznamu</span><span class="sxs-lookup"><span data-stu-id="2125f-158">Option 1: map hello data for a list mapping</span></span>
<span data-ttu-id="2125f-159">Mapování dat hello přidáním seznamu mapování pro každého klienta.</span><span class="sxs-lookup"><span data-stu-id="2125f-159">Map hello data by adding a list mapping for each tenant.</span></span>  

    # Create hello mappings and associate it with hello new shards 
    Add-ListMapping 
    -KeyType $([int]) 
    -ListPoint '<tenant_id>' 
    -ListShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 

### <a name="option-2-map-hello-data-for-a-range-mapping"></a><span data-ttu-id="2125f-160">Možnost 2: mapování hello dat pro mapování rozsahu</span><span class="sxs-lookup"><span data-stu-id="2125f-160">Option 2: map hello data for a range mapping</span></span>
<span data-ttu-id="2125f-161">Přidejte hello rozsah mapování pro všechny hello klienta rozsah id - přidružení databáze:</span><span class="sxs-lookup"><span data-stu-id="2125f-161">Add hello range mappings for all hello tenant id range - database associations:</span></span>

    # Create hello mappings and associate it with hello new shards 
    Add-RangeMapping 
    -KeyType $([int]) 
    -RangeHigh '5' 
    -RangeLow '1' 
    -RangeShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 


### <a name="step-4-option-3-map-hello-data-for-multiple-tenants-on-a-single-database"></a><span data-ttu-id="2125f-162">Krok 4 možnost 3: mapování hello dat pro více klientů v jedné databáze</span><span class="sxs-lookup"><span data-stu-id="2125f-162">Step 4 option 3: map hello data for multiple tenants on a single database</span></span>
<span data-ttu-id="2125f-163">Pro každého klienta, spusťte hello přidat ListMapping (možnost 1, výše).</span><span class="sxs-lookup"><span data-stu-id="2125f-163">For each tenant, run hello Add-ListMapping (option 1, above).</span></span> 

## <a name="checking-hello-mappings"></a><span data-ttu-id="2125f-164">Kontrola, zda text hello mapování</span><span class="sxs-lookup"><span data-stu-id="2125f-164">Checking hello mappings</span></span>
<span data-ttu-id="2125f-165">Informace o hello existující horizontálních oddílů a mapování hello s nimi spojených lze dotazovat pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="2125f-165">Information about hello existing shards and hello mappings associated with them can be queried using following commands:</span></span>  

    # List hello shards and mappings 
    Get-Shards -ShardMap $ShardMap 
    Get-Mappings -ShardMap $ShardMap 

## <a name="summary"></a><span data-ttu-id="2125f-166">Souhrn</span><span class="sxs-lookup"><span data-stu-id="2125f-166">Summary</span></span>
<span data-ttu-id="2125f-167">Po dokončení instalace hello, můžete začít toouse hello elastické databáze klientské knihovny.</span><span class="sxs-lookup"><span data-stu-id="2125f-167">Once you have completed hello setup, you can begin toouse hello Elastic Database client library.</span></span> <span data-ttu-id="2125f-168">Můžete také použít [závislé směrování dat](sql-database-elastic-scale-data-dependent-routing.md) a [dotazu víc horizontálních](sql-database-elastic-scale-multishard-querying.md).</span><span class="sxs-lookup"><span data-stu-id="2125f-168">You can also use [data dependent routing](sql-database-elastic-scale-data-dependent-routing.md) and [multi-shard query](sql-database-elastic-scale-multishard-querying.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2125f-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2125f-169">Next steps</span></span>
<span data-ttu-id="2125f-170">Získat skriptů prostředí PowerShell hello z [Azure SQL DB Elastická databáze nástroje sripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span><span class="sxs-lookup"><span data-stu-id="2125f-170">Get hello PowerShell scripts from [Azure SQL DB-Elastic Database tools sripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span></span>

<span data-ttu-id="2125f-171">Hello nástroje jsou také na Githubu: [/Elastická db nástroje Azure](https://github.com/Azure/elastic-db-tools).</span><span class="sxs-lookup"><span data-stu-id="2125f-171">hello tools are also on GitHub: [Azure/elastic-db-tools](https://github.com/Azure/elastic-db-tools).</span></span>

<span data-ttu-id="2125f-172">Použijte hello nástroji pro sloučení rozdělení toomove data tooor z modelu víceklientského modelu tooa jednoho klienta.</span><span class="sxs-lookup"><span data-stu-id="2125f-172">Use hello split-merge tool toomove data tooor from a multi-tenant model tooa single tenant model.</span></span> <span data-ttu-id="2125f-173">V tématu [nástroji pro sloučení rozdělení](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="2125f-173">See [Split merge tool](sql-database-elastic-scale-get-started.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2125f-174">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="2125f-174">Additional resources</span></span>
<span data-ttu-id="2125f-175">Informace o běžných vzorech architektury dat databázových aplikací softwaru s více tenanty jako služby (SaaS) naleznete v části [Vzory návrhu pro aplikace SaaS s více tenanty s databází Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).</span><span class="sxs-lookup"><span data-stu-id="2125f-175">For information on common data architecture patterns of multi-tenant software-as-a-service (SaaS) database applications, see [Design Patterns for Multi-tenant SaaS Applications with Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).</span></span>

## <a name="questions-and-feature-requests"></a><span data-ttu-id="2125f-176">Otázky a žádosti o funkce</span><span class="sxs-lookup"><span data-stu-id="2125f-176">Questions and Feature Requests</span></span>
<span data-ttu-id="2125f-177">Máte dotazy, kontaktujte toous na hello [SQL Database fórum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) a pro žádosti o funkce, přidejte je toohello [fóru pro zpětnou vazbu SQL Database](https://feedback.azure.com/forums/217321-sql-database/).</span><span class="sxs-lookup"><span data-stu-id="2125f-177">For questions, please reach out toous on hello [SQL Database forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) and for feature requests, please add them toohello [SQL Database feedback forum](https://feedback.azure.com/forums/217321-sql-database/).</span></span>

<!--Image references-->
[1]: ./media/sql-database-elastic-convert-to-use-elastic-tools/listmapping.png
[2]: ./media/sql-database-elastic-convert-to-use-elastic-tools/rangemapping.png
[3]: ./media/sql-database-elastic-convert-to-use-elastic-tools/multipleonsingledb.png

