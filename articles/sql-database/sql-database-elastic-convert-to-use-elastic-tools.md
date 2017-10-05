---
title: "Migrovat existující databáze pro škálované | Microsoft Docs"
description: "Převést horizontálně dělené databáze používejte nástroje elastické databáze tak, že vytvoříte horizontálního oddílu správce mapy"
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
ms.openlocfilehash: 099f40d00753b7c86ba726a818f17d440a125221
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="migrate-existing-databases-to-scale-out"></a><span data-ttu-id="82cc7-103">Migrace existujících databází pro horizontální navýšení kapacity</span><span class="sxs-lookup"><span data-stu-id="82cc7-103">Migrate existing databases to scale-out</span></span>
<span data-ttu-id="82cc7-104">Snadno spravovat existující upraveným horizontálně dělené databáze pomocí nástrojů databáze Azure SQL Database (například [klientské knihovny pro elastické databáze](sql-database-elastic-database-client-library.md)).</span><span class="sxs-lookup"><span data-stu-id="82cc7-104">Easily manage your existing scaled-out sharded databases using Azure SQL Database database tools (such as the [Elastic Database client library](sql-database-elastic-database-client-library.md)).</span></span> <span data-ttu-id="82cc7-105">Je nutné nejprve převést stávající sadu databází, které chcete použít [správce mapy horizontálního oddílu](sql-database-elastic-scale-shard-map-management.md).</span><span class="sxs-lookup"><span data-stu-id="82cc7-105">You must first convert an existing set of databases to use the [shard map manager](sql-database-elastic-scale-shard-map-management.md).</span></span> 

## <a name="overview"></a><span data-ttu-id="82cc7-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="82cc7-106">Overview</span></span>
<span data-ttu-id="82cc7-107">Pokud chcete migrovat existující databázi horizontálně dělené:</span><span class="sxs-lookup"><span data-stu-id="82cc7-107">To migrate an existing sharded database:</span></span> 

1. <span data-ttu-id="82cc7-108">Příprava [horizontálního oddílu mapa správce databáze](sql-database-elastic-scale-shard-map-management.md).</span><span class="sxs-lookup"><span data-stu-id="82cc7-108">Prepare the [shard map manager database](sql-database-elastic-scale-shard-map-management.md).</span></span>
2. <span data-ttu-id="82cc7-109">Vytvoření mapy horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="82cc7-109">Create the shard map.</span></span>
3. <span data-ttu-id="82cc7-110">Příprava jednotlivých horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="82cc7-110">Prepare the individual shards.</span></span>  
4. <span data-ttu-id="82cc7-111">Přidání mapování horizontálních mapy.</span><span class="sxs-lookup"><span data-stu-id="82cc7-111">Add mappings to the shard map.</span></span>

<span data-ttu-id="82cc7-112">Tyto postupy můžou být implementovaná pomocí buď [Klientská knihovna pro rozhraní .NET Framework](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/), nebo skripty prostředí PowerShell nalezený na [Azure SQL DB - skripty nástroje elastické databáze](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span><span class="sxs-lookup"><span data-stu-id="82cc7-112">These techniques can be implemented using either the [.NET Framework client library](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/), or the PowerShell scripts found at [Azure SQL DB - Elastic Database tools scripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span></span> <span data-ttu-id="82cc7-113">Zde uvedené příklady použití skriptů prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="82cc7-113">The examples here use the PowerShell scripts.</span></span>

<span data-ttu-id="82cc7-114">Další informace o ShardMapManager najdete v tématu [horizontálního oddílu mapy správu](sql-database-elastic-scale-shard-map-management.md).</span><span class="sxs-lookup"><span data-stu-id="82cc7-114">For more information about the ShardMapManager, see [Shard map management](sql-database-elastic-scale-shard-map-management.md).</span></span> <span data-ttu-id="82cc7-115">Přehled nástroje elastické databáze najdete v tématu [přehled funkcí elastické databáze](sql-database-elastic-scale-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="82cc7-115">For an overview of the elastic database tools, see [Elastic Database features overview](sql-database-elastic-scale-introduction.md).</span></span>

## <a name="prepare-the-shard-map-manager-database"></a><span data-ttu-id="82cc7-116">Příprava databáze manager horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="82cc7-116">Prepare the shard map manager database</span></span>
<span data-ttu-id="82cc7-117">Správce mapy horizontálního oddílu je speciální databázi, která obsahuje data, která mají spravovat upraveným databáze.</span><span class="sxs-lookup"><span data-stu-id="82cc7-117">The shard map manager is a special database that contains the data to manage scaled-out databases.</span></span> <span data-ttu-id="82cc7-118">Můžete použít existující databázi nebo vytvořte novou databázi.</span><span class="sxs-lookup"><span data-stu-id="82cc7-118">You can use an existing database, or create a new database.</span></span> <span data-ttu-id="82cc7-119">Všimněte si, že databáze, který funguje jako správce mapy horizontálního oddílu by neměl být stejné databázi jako horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="82cc7-119">Note that a database acting as shard map manager should not be the same database as a shard.</span></span> <span data-ttu-id="82cc7-120">Všimněte si také, že skript prostředí PowerShell není vytvořit databázi za vás.</span><span class="sxs-lookup"><span data-stu-id="82cc7-120">Also note that the PowerShell script does not create the database for you.</span></span> 

## <a name="step-1-create-a-shard-map-manager"></a><span data-ttu-id="82cc7-121">Krok 1: vytvoření horizontálního oddílu správce mapy</span><span class="sxs-lookup"><span data-stu-id="82cc7-121">Step 1: create a shard map manager</span></span>
    # Create a shard map manager. 
    New-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 
    #<server_name> and <smm_db_name> are the server name and database name 
    # for the new or existing database that should be used for storing 
    # tenant-database mapping information.

### <a name="to-retrieve-the-shard-map-manager"></a><span data-ttu-id="82cc7-122">Načtení správce horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="82cc7-122">To retrieve the shard map manager</span></span>
<span data-ttu-id="82cc7-123">Po vytvoření můžete načíst správce mapy horizontálního oddílu pomocí této rutiny.</span><span class="sxs-lookup"><span data-stu-id="82cc7-123">After creation, you can retrieve the shard map manager with this cmdlet.</span></span> <span data-ttu-id="82cc7-124">Tento krok je nutný pokaždé, když potřebujete použít objekt ShardMapManager.</span><span class="sxs-lookup"><span data-stu-id="82cc7-124">This step is needed every time you need to use the ShardMapManager object.</span></span>

    # Try to get a reference to the Shard Map Manager  
    $ShardMapManager = Get-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 


## <a name="step-2-create-the-shard-map"></a><span data-ttu-id="82cc7-125">Krok 2: vytvoření mapy horizontálního oddílu</span><span class="sxs-lookup"><span data-stu-id="82cc7-125">Step 2: create the shard map</span></span>
<span data-ttu-id="82cc7-126">Je nutné vybrat typ mapy horizontálního oddílu k vytvoření.</span><span class="sxs-lookup"><span data-stu-id="82cc7-126">You must select the type of shard map to create.</span></span> <span data-ttu-id="82cc7-127">Výběr závisí na architektuře databáze:</span><span class="sxs-lookup"><span data-stu-id="82cc7-127">The choice depends on the database architecture:</span></span> 

1. <span data-ttu-id="82cc7-128">Jednoho klienta na databázi (podmínky, najdete v článku [Glosář](sql-database-elastic-scale-glossary.md).)</span><span class="sxs-lookup"><span data-stu-id="82cc7-128">Single tenant per database (For terms, see the [glossary](sql-database-elastic-scale-glossary.md).)</span></span> 
2. <span data-ttu-id="82cc7-129">Několik klientů na databázi (dva typy):</span><span class="sxs-lookup"><span data-stu-id="82cc7-129">Multiple tenants per database (two types):</span></span>
   1. <span data-ttu-id="82cc7-130">Seznam mapování</span><span class="sxs-lookup"><span data-stu-id="82cc7-130">List mapping</span></span>
   2. <span data-ttu-id="82cc7-131">Mapování rozsahu</span><span class="sxs-lookup"><span data-stu-id="82cc7-131">Range mapping</span></span>

<span data-ttu-id="82cc7-132">Pro jednoho klienta modelu, vytvořit **seznamu mapování** horizontálního oddílu mapy.</span><span class="sxs-lookup"><span data-stu-id="82cc7-132">For a single-tenant model, create a **list mapping** shard map.</span></span> <span data-ttu-id="82cc7-133">Model jednoho klienta přiřadí jednu databázi na každého klienta.</span><span class="sxs-lookup"><span data-stu-id="82cc7-133">The single-tenant model assigns one database per tenant.</span></span> <span data-ttu-id="82cc7-134">Jedná se efektivní model pro vývojáře SaaS, protože ho zjednodušuje správu.</span><span class="sxs-lookup"><span data-stu-id="82cc7-134">This is an effective model for SaaS developers as it simplifies management.</span></span>

![Seznam mapování][1]

<span data-ttu-id="82cc7-136">Víceklientského modelu přiřadí několik klientů jedné databáze (a skupin klientů, které můžete distribuovat mezi více databází).</span><span class="sxs-lookup"><span data-stu-id="82cc7-136">The multi-tenant model assigns several tenants to a single database (and you can distribute groups of tenants across multiple databases).</span></span> <span data-ttu-id="82cc7-137">Pomocí tohoto modelu, pokud očekáváte, že každý klient k potřebami malá data.</span><span class="sxs-lookup"><span data-stu-id="82cc7-137">Use this model when you expect each tenant to have small data needs.</span></span> <span data-ttu-id="82cc7-138">V tomto modelu jsme řadu klienty přiřadit k databázi pomocí **rozsah mapování**.</span><span class="sxs-lookup"><span data-stu-id="82cc7-138">In this model, we assign a range of tenants to a database using **range mapping**.</span></span> 

![Mapování rozsahu][2]

<span data-ttu-id="82cc7-140">Nebo můžete implementovat s použitím modelu víceklientské databáze *seznamu mapování* přiřadit více klientů pro jednu databázi.</span><span class="sxs-lookup"><span data-stu-id="82cc7-140">Or you can implement a multi-tenant database model using a *list mapping* to assign multiple tenants to a single database.</span></span> <span data-ttu-id="82cc7-141">Například DB1 se používá k ukládání informací o id klienta 1 a 5 a DB2 ukládá data pro klienta 7 a klienta 10.</span><span class="sxs-lookup"><span data-stu-id="82cc7-141">For example, DB1 is used to store information about tenant id 1 and 5, and DB2 stores data for tenant 7 and tenant 10.</span></span> 

![Vnořením klientů na jednom DB][3] 

<span data-ttu-id="82cc7-143">**Podle své volby, vyberte jednu z těchto možností:**</span><span class="sxs-lookup"><span data-stu-id="82cc7-143">**Based on your choice, choose one of these options:**</span></span>

### <a name="option-1-create-a-shard-map-for-a-list-mapping"></a><span data-ttu-id="82cc7-144">Možnost 1: vytvoření mapy horizontálního oddílu pro seznam mapování</span><span class="sxs-lookup"><span data-stu-id="82cc7-144">Option 1: create a shard map for a list mapping</span></span>
<span data-ttu-id="82cc7-145">Vytvoření mapy horizontálního oddílu pomocí ShardMapManager objektu.</span><span class="sxs-lookup"><span data-stu-id="82cc7-145">Create a shard map using the ShardMapManager object.</span></span> 

    # $ShardMapManager is the shard map manager object. 
    $ShardMap = New-ListShardMap -KeyType $([int]) 
    -ListShardMapName 'ListShardMap' 
    -ShardMapManager $ShardMapManager 


### <a name="option-2-create-a-shard-map-for-a-range-mapping"></a><span data-ttu-id="82cc7-146">Možnost 2: vytvoření mapy horizontálního oddílu pro mapování rozsahu</span><span class="sxs-lookup"><span data-stu-id="82cc7-146">Option 2: create a shard map for a range mapping</span></span>
<span data-ttu-id="82cc7-147">Všimněte si, že pokud chcete využít tento vzor mapování, id klienta hodnoty musí být průběžné rozsahy a je přijatelná mezera v rozsazích jednoduše přeskočením rozsahu při vytváření databáze.</span><span class="sxs-lookup"><span data-stu-id="82cc7-147">Note that to utilize this mapping pattern, tenant id values needs to be continuous ranges, and it is acceptable to have gap in the ranges by simply skipping the range when creating the databases.</span></span>

    # $ShardMapManager is the shard map manager object 
    # 'RangeShardMap' is the unique identifier for the range shard map.  
    $ShardMap = New-RangeShardMap 
    -KeyType $([int]) 
    -RangeShardMapName 'RangeShardMap' 
    -ShardMapManager $ShardMapManager 

### <a name="option-3-list-mappings-on-a-single-database"></a><span data-ttu-id="82cc7-148">Možnost 3: Seznam mapování na jedné databáze</span><span class="sxs-lookup"><span data-stu-id="82cc7-148">Option 3: List mappings on a single database</span></span>
<span data-ttu-id="82cc7-149">Nastavení tohoto vzoru také vyžaduje vytvoření seznamu mapování, jak je znázorněno v kroku 2, možnost 1.</span><span class="sxs-lookup"><span data-stu-id="82cc7-149">Setting up this pattern also requires creation of a list map as shown in step 2, option 1.</span></span>

## <a name="step-3-prepare-individual-shards"></a><span data-ttu-id="82cc7-150">Krok 3: Příprava jednotlivých horizontálních oddílů</span><span class="sxs-lookup"><span data-stu-id="82cc7-150">Step 3: Prepare individual shards</span></span>
<span data-ttu-id="82cc7-151">Přidejte každou horizontálního oddílu (databáze) pro správce mapy horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="82cc7-151">Add each shard (database) to the shard map manager.</span></span> <span data-ttu-id="82cc7-152">To připraví jednotlivé databáze pro ukládání informací o mapování.</span><span class="sxs-lookup"><span data-stu-id="82cc7-152">This prepares the individual databases for storing mapping information.</span></span> <span data-ttu-id="82cc7-153">Tato metoda spusťte na každém horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="82cc7-153">Execute this method on each shard.</span></span>

    Add-Shard 
    -ShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>'
    # The $ShardMap is the shard map created in step 2.


## <a name="step-4-add-mappings"></a><span data-ttu-id="82cc7-154">Krok 4: Přidejte mapování</span><span class="sxs-lookup"><span data-stu-id="82cc7-154">Step 4: Add mappings</span></span>
<span data-ttu-id="82cc7-155">Přidání mapování závisí na druhu mapy horizontálního oddílu, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="82cc7-155">The addition of mappings depends on the kind of shard map you created.</span></span> <span data-ttu-id="82cc7-156">Pokud jste vytvořili seznam mapy, můžete přidat mapování seznamu.</span><span class="sxs-lookup"><span data-stu-id="82cc7-156">If you created a list map, you add list mappings.</span></span> <span data-ttu-id="82cc7-157">Pokud jste vytvořili mapu rozsahu, můžete přidat mapování rozsahu.</span><span class="sxs-lookup"><span data-stu-id="82cc7-157">If you created a range map, you add range mappings.</span></span>

### <a name="option-1-map-the-data-for-a-list-mapping"></a><span data-ttu-id="82cc7-158">Možnost 1: mapování dat pro mapování seznamu</span><span class="sxs-lookup"><span data-stu-id="82cc7-158">Option 1: map the data for a list mapping</span></span>
<span data-ttu-id="82cc7-159">Mapování dat přidáním seznamu mapování pro každého klienta.</span><span class="sxs-lookup"><span data-stu-id="82cc7-159">Map the data by adding a list mapping for each tenant.</span></span>  

    # Create the mappings and associate it with the new shards 
    Add-ListMapping 
    -KeyType $([int]) 
    -ListPoint '<tenant_id>' 
    -ListShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 

### <a name="option-2-map-the-data-for-a-range-mapping"></a><span data-ttu-id="82cc7-160">Možnost 2: mapování dat pro mapování rozsahu</span><span class="sxs-lookup"><span data-stu-id="82cc7-160">Option 2: map the data for a range mapping</span></span>
<span data-ttu-id="82cc7-161">Přidáte rozsah mapování pro všechny klientské id rozsah - přidružení databáze:</span><span class="sxs-lookup"><span data-stu-id="82cc7-161">Add the range mappings for all the tenant id range - database associations:</span></span>

    # Create the mappings and associate it with the new shards 
    Add-RangeMapping 
    -KeyType $([int]) 
    -RangeHigh '5' 
    -RangeLow '1' 
    -RangeShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 


### <a name="step-4-option-3-map-the-data-for-multiple-tenants-on-a-single-database"></a><span data-ttu-id="82cc7-162">Krok 4 možnost 3: mapování dat pro více klientů v jedné databáze</span><span class="sxs-lookup"><span data-stu-id="82cc7-162">Step 4 option 3: map the data for multiple tenants on a single database</span></span>
<span data-ttu-id="82cc7-163">Pro každého klienta, spusťte příkaz Add-ListMapping (možnost 1, výše).</span><span class="sxs-lookup"><span data-stu-id="82cc7-163">For each tenant, run the Add-ListMapping (option 1, above).</span></span> 

## <a name="checking-the-mappings"></a><span data-ttu-id="82cc7-164">Kontrola mapování</span><span class="sxs-lookup"><span data-stu-id="82cc7-164">Checking the mappings</span></span>
<span data-ttu-id="82cc7-165">Informace o existující horizontálních oddílů a s nimi spojených mapování lze dotazovat pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="82cc7-165">Information about the existing shards and the mappings associated with them can be queried using following commands:</span></span>  

    # List the shards and mappings 
    Get-Shards -ShardMap $ShardMap 
    Get-Mappings -ShardMap $ShardMap 

## <a name="summary"></a><span data-ttu-id="82cc7-166">Souhrn</span><span class="sxs-lookup"><span data-stu-id="82cc7-166">Summary</span></span>
<span data-ttu-id="82cc7-167">Po dokončení instalace můžete začít používat klientské knihovny pro elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="82cc7-167">Once you have completed the setup, you can begin to use the Elastic Database client library.</span></span> <span data-ttu-id="82cc7-168">Můžete také použít [závislé směrování dat](sql-database-elastic-scale-data-dependent-routing.md) a [dotazu víc horizontálních](sql-database-elastic-scale-multishard-querying.md).</span><span class="sxs-lookup"><span data-stu-id="82cc7-168">You can also use [data dependent routing](sql-database-elastic-scale-data-dependent-routing.md) and [multi-shard query](sql-database-elastic-scale-multishard-querying.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="82cc7-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="82cc7-169">Next steps</span></span>
<span data-ttu-id="82cc7-170">Získat skriptů prostředí PowerShell z [Azure SQL DB Elastická databáze nástroje sripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span><span class="sxs-lookup"><span data-stu-id="82cc7-170">Get the PowerShell scripts from [Azure SQL DB-Elastic Database tools sripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span></span>

<span data-ttu-id="82cc7-171">Nástroje jsou také na Githubu: [/Elastická db nástroje Azure](https://github.com/Azure/elastic-db-tools).</span><span class="sxs-lookup"><span data-stu-id="82cc7-171">The tools are also on GitHub: [Azure/elastic-db-tools](https://github.com/Azure/elastic-db-tools).</span></span>

<span data-ttu-id="82cc7-172">Použijte nástroj pro rozdělení sloučení pro přesun dat do nebo z víceklientského modelu do jednoho klienta modelu.</span><span class="sxs-lookup"><span data-stu-id="82cc7-172">Use the split-merge tool to move data to or from a multi-tenant model to a single tenant model.</span></span> <span data-ttu-id="82cc7-173">V tématu [nástroji pro sloučení rozdělení](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="82cc7-173">See [Split merge tool](sql-database-elastic-scale-get-started.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="82cc7-174">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="82cc7-174">Additional resources</span></span>
<span data-ttu-id="82cc7-175">Informace o běžných vzorech architektury dat databázových aplikací softwaru s více tenanty jako služby (SaaS) naleznete v části [Vzory návrhu pro aplikace SaaS s více tenanty s databází Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).</span><span class="sxs-lookup"><span data-stu-id="82cc7-175">For information on common data architecture patterns of multi-tenant software-as-a-service (SaaS) database applications, see [Design Patterns for Multi-tenant SaaS Applications with Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).</span></span>

## <a name="questions-and-feature-requests"></a><span data-ttu-id="82cc7-176">Otázky a žádosti o funkce</span><span class="sxs-lookup"><span data-stu-id="82cc7-176">Questions and Feature Requests</span></span>
<span data-ttu-id="82cc7-177">Máte dotazy, kontaktujte nás na [fórum SQL Database](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) a pro žádosti o funkce, přidejte je do [fóru pro zpětnou vazbu SQL Database](https://feedback.azure.com/forums/217321-sql-database/).</span><span class="sxs-lookup"><span data-stu-id="82cc7-177">For questions, please reach out to us on the [SQL Database forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) and for feature requests, please add them to the [SQL Database feedback forum](https://feedback.azure.com/forums/217321-sql-database/).</span></span>

<!--Image references-->
[1]: ./media/sql-database-elastic-convert-to-use-elastic-tools/listmapping.png
[2]: ./media/sql-database-elastic-convert-to-use-elastic-tools/rangemapping.png
[3]: ./media/sql-database-elastic-convert-to-use-elastic-tools/multipleonsingledb.png

