---
title: aaaScale na Azure SQL database | Microsoft Docs
description: "Jak toouse hello ShardMapManager klientské knihovny pro elastické databáze"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 0e9d647a-9ba9-4875-aa22-662d01283439
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 4cf670d42ad7ded98fb8d6f0830154587dd2c6f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scale-out-databases-with-hello-shard-map-manager"></a><span data-ttu-id="181ea-103">Horizontální navýšení kapacity databáze s hello horizontálního oddílu mapa správce</span><span class="sxs-lookup"><span data-stu-id="181ea-103">Scale out databases with hello shard map manager</span></span>
<span data-ttu-id="181ea-104">tooeasily škálování databáze na SQL Azure, použijte správce mapy horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="181ea-104">tooeasily scale out databases on SQL Azure, use a shard map manager.</span></span> <span data-ttu-id="181ea-105">Hello horizontálního oddílu mapa správce je speciální databáze, která udržuje globální mapování informace o všech horizontálních oddílů (databáze) v sadě horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="181ea-105">hello shard map manager is a special database that maintains global mapping information about all shards (databases) in a shard set.</span></span> <span data-ttu-id="181ea-106">Hello metadata umožňuje aplikace tooconnect toohello správnou databázi na základě hello hodnotu hello **horizontálního dělení klíč**.</span><span class="sxs-lookup"><span data-stu-id="181ea-106">hello metadata allows an application tooconnect toohello correct database based upon hello value of hello **sharding key**.</span></span> <span data-ttu-id="181ea-107">Kromě toho každých horizontálního oddílu v sadě hello obsahuje mapy, které sledují hello místní sdílení dat (označované jako **shardlets**).</span><span class="sxs-lookup"><span data-stu-id="181ea-107">In addition, every shard in hello set contains maps that track hello local shard data (known as **shardlets**).</span></span> 

![Správa mapování horizontálních](./media/sql-database-elastic-scale-shard-map-management/glossary.png)

<span data-ttu-id="181ea-109">Pochopení, jak se vytvářejí tyto mapy je nezbytné tooshard mapy management.</span><span class="sxs-lookup"><span data-stu-id="181ea-109">Understanding how these maps are constructed is essential tooshard map management.</span></span> <span data-ttu-id="181ea-110">To se provádí pomocí hello [ShardMapManager třída](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx), který se nachází v hello [klientské knihovny pro elastické databáze](sql-database-elastic-database-client-library.md) toomanage horizontálních mapy.</span><span class="sxs-lookup"><span data-stu-id="181ea-110">This is done using hello [ShardMapManager class](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx), found in hello [Elastic Database client library](sql-database-elastic-database-client-library.md) toomanage shard maps.</span></span>  

## <a name="shard-maps-and-shard-mappings"></a><span data-ttu-id="181ea-111">Mapování horizontálních a horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="181ea-111">Shard maps and shard mappings</span></span>
<span data-ttu-id="181ea-112">Pro každý horizontálního oddílu je třeba vybrat hello typ toocreate mapy horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="181ea-112">For each shard, you must select hello type of shard map toocreate.</span></span> <span data-ttu-id="181ea-113">Volba Hello závisí na architektuře databáze hello:</span><span class="sxs-lookup"><span data-stu-id="181ea-113">hello choice depends on hello database architecture:</span></span> 

1. <span data-ttu-id="181ea-114">Jednoho klienta na databázi</span><span class="sxs-lookup"><span data-stu-id="181ea-114">Single tenant per database</span></span>  
2. <span data-ttu-id="181ea-115">Několik klientů na databázi (dva typy):</span><span class="sxs-lookup"><span data-stu-id="181ea-115">Multiple tenants per database (two types):</span></span>
   1. <span data-ttu-id="181ea-116">Seznam mapování</span><span class="sxs-lookup"><span data-stu-id="181ea-116">List mapping</span></span>
   2. <span data-ttu-id="181ea-117">Mapování rozsahu</span><span class="sxs-lookup"><span data-stu-id="181ea-117">Range mapping</span></span>

<span data-ttu-id="181ea-118">Pro jednoho klienta modelu, vytvořit **seznamu mapování** horizontálního oddílu mapy.</span><span class="sxs-lookup"><span data-stu-id="181ea-118">For a single-tenant model, create a **list mapping** shard map.</span></span> <span data-ttu-id="181ea-119">model jednoho klienta Hello přiřadí jednu databázi na každého klienta.</span><span class="sxs-lookup"><span data-stu-id="181ea-119">hello single-tenant model assigns one database per tenant.</span></span> <span data-ttu-id="181ea-120">Jedná se efektivní model pro vývojáře SaaS, protože ho zjednodušuje správu.</span><span class="sxs-lookup"><span data-stu-id="181ea-120">This is an effective model for SaaS developers as it simplifies management.</span></span>

![Seznam mapování][1]

<span data-ttu-id="181ea-122">Hello víceklientského modelu přiřadí několik klientů tooa jedné databáze (a skupin klientů, které můžete distribuovat mezi více databází).</span><span class="sxs-lookup"><span data-stu-id="181ea-122">hello multi-tenant model assigns several tenants tooa single database (and you can distribute groups of tenants across multiple databases).</span></span> <span data-ttu-id="181ea-123">Pomocí tohoto modelu, pokud očekáváte, že je každý klient toohave malá data.</span><span class="sxs-lookup"><span data-stu-id="181ea-123">Use this model when you expect each tenant toohave small data needs.</span></span> <span data-ttu-id="181ea-124">V tomto modelu jsme přiřadit rozsah klientů tooa databázi pomocí **rozsah mapování**.</span><span class="sxs-lookup"><span data-stu-id="181ea-124">In this model, we assign a range of tenants tooa database using **range mapping**.</span></span> 

![Mapování rozsahu][2]

<span data-ttu-id="181ea-126">Nebo můžete implementovat s použitím modelu víceklientské databáze *seznamu mapování* tooassign více klientů tooa jedné databáze.</span><span class="sxs-lookup"><span data-stu-id="181ea-126">Or you can implement a multi-tenant database model using a *list mapping* tooassign multiple tenants tooa single database.</span></span> <span data-ttu-id="181ea-127">Například DB1 je použité toostore informace o id klienta 1 a 5 a DB2 ukládá data pro klienta 7 a klienta 10.</span><span class="sxs-lookup"><span data-stu-id="181ea-127">For example, DB1 is used toostore information about tenant id 1 and 5, and DB2 stores data for tenant 7 and tenant 10.</span></span> 

![Vnořením klientů na jednom DB][3] 

### <a name="supported-net-types-for-sharding-keys"></a><span data-ttu-id="181ea-129">Podporované typy .net pro klíče horizontálního dělení</span><span class="sxs-lookup"><span data-stu-id="181ea-129">Supported .Net types for sharding keys</span></span>
<span data-ttu-id="181ea-130">Elastické škálování podporu hello následující rozhraní .net Framework typy jako horizontálního dělení klíče:</span><span class="sxs-lookup"><span data-stu-id="181ea-130">Elastic Scale support hello following .Net Framework types as sharding keys:</span></span>

* <span data-ttu-id="181ea-131">celé číslo</span><span class="sxs-lookup"><span data-stu-id="181ea-131">integer</span></span>
* <span data-ttu-id="181ea-132">dlouhá</span><span class="sxs-lookup"><span data-stu-id="181ea-132">long</span></span>
* <span data-ttu-id="181ea-133">Identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="181ea-133">guid</span></span>
* <span data-ttu-id="181ea-134">Byte]</span><span class="sxs-lookup"><span data-stu-id="181ea-134">byte[]</span></span>  
* <span data-ttu-id="181ea-135">Data a času</span><span class="sxs-lookup"><span data-stu-id="181ea-135">datetime</span></span>
* <span data-ttu-id="181ea-136">Časový interval</span><span class="sxs-lookup"><span data-stu-id="181ea-136">timespan</span></span>
* <span data-ttu-id="181ea-137">Datový typ DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="181ea-137">datetimeoffset</span></span>

### <a name="list-and-range-shard-maps"></a><span data-ttu-id="181ea-138">Seznam a rozsah horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="181ea-138">List and range shard maps</span></span>
<span data-ttu-id="181ea-139">Mapování horizontálních se dá vytvořit pomocí **seznam jednotlivých horizontálního dělení hodnoty klíče**, nebo se dá vytvořit pomocí **rozsahy horizontálního dělení hodnoty klíče**.</span><span class="sxs-lookup"><span data-stu-id="181ea-139">Shard maps can be constructed using **lists of individual sharding key values**, or they can be constructed using **ranges of sharding key values**.</span></span> 

### <a name="list-shard-maps"></a><span data-ttu-id="181ea-140">Seznam horizontálních mapy</span><span class="sxs-lookup"><span data-stu-id="181ea-140">List shard maps</span></span>
<span data-ttu-id="181ea-141">**Horizontálních oddílů** obsahovat **shardlets** a hello mapování shardlets tooshards je možný díky mapu horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="181ea-141">**Shards** contain **shardlets** and hello mapping of shardlets tooshards is maintained by a shard map.</span></span> <span data-ttu-id="181ea-142">A **seznam horizontálních mapy** je přidružení mezi hello jednotlivé hodnoty klíče identifikují hello shardlets a hello databázemi, které slouží jako horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="181ea-142">A **list shard map** is an association between hello individual key values that identify hello shardlets and hello databases that serve as shards.</span></span>  <span data-ttu-id="181ea-143">**Seznam mapování** jsou explicitní a jiné hodnoty klíče může být namapované toohello stejné databáze.</span><span class="sxs-lookup"><span data-stu-id="181ea-143">**List mappings** are explicit and different key values can be mapped toohello same database.</span></span> <span data-ttu-id="181ea-144">Například klíč 1 mapuje tooDatabase A a B. databáze odkazovat na klíčové hodnoty 3 a 6</span><span class="sxs-lookup"><span data-stu-id="181ea-144">For example, key 1 maps tooDatabase A, and key values 3 and 6 both reference Database B.</span></span>

| <span data-ttu-id="181ea-145">Klíč</span><span class="sxs-lookup"><span data-stu-id="181ea-145">Key</span></span> | <span data-ttu-id="181ea-146">Umístění horizontálního oddílu</span><span class="sxs-lookup"><span data-stu-id="181ea-146">Shard Location</span></span> |
| --- | --- |
| <span data-ttu-id="181ea-147">1</span><span class="sxs-lookup"><span data-stu-id="181ea-147">1</span></span> |<span data-ttu-id="181ea-148">Database_A</span><span class="sxs-lookup"><span data-stu-id="181ea-148">Database_A</span></span> |
| <span data-ttu-id="181ea-149">3</span><span class="sxs-lookup"><span data-stu-id="181ea-149">3</span></span> |<span data-ttu-id="181ea-150">Database_B</span><span class="sxs-lookup"><span data-stu-id="181ea-150">Database_B</span></span> |
| <span data-ttu-id="181ea-151">4</span><span class="sxs-lookup"><span data-stu-id="181ea-151">4</span></span> |<span data-ttu-id="181ea-152">Database_C</span><span class="sxs-lookup"><span data-stu-id="181ea-152">Database_C</span></span> |
| <span data-ttu-id="181ea-153">6</span><span class="sxs-lookup"><span data-stu-id="181ea-153">6</span></span> |<span data-ttu-id="181ea-154">Database_B</span><span class="sxs-lookup"><span data-stu-id="181ea-154">Database_B</span></span> |
| <span data-ttu-id="181ea-155">Tlačítka ...</span><span class="sxs-lookup"><span data-stu-id="181ea-155">...</span></span> |<span data-ttu-id="181ea-156">Tlačítka ...</span><span class="sxs-lookup"><span data-stu-id="181ea-156">...</span></span> |

### <a name="range-shard-maps"></a><span data-ttu-id="181ea-157">Rozsah horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="181ea-157">Range shard maps</span></span>
<span data-ttu-id="181ea-158">V **rozsah horizontálního oddílu mapy**, klíče rozsah hello je popsán pár **[hodnota nízká, vysoká hodnota)** kde hello *nízká hodnota* je hello minimální klíč v rozsahu hello a hello *Vysoké hodnoty* je vyšší než hello rozsah hello první hodnota.</span><span class="sxs-lookup"><span data-stu-id="181ea-158">In a **range shard map**, hello key range is described by a pair **[Low Value, High Value)** where hello *Low Value* is hello minimum key in hello range, and hello *High Value* is hello first value higher than hello range.</span></span> 

<span data-ttu-id="181ea-159">Například **[0, 100)** zahrnuje všechny celá čísla větší než nebo rovna 0 a menší než 100.</span><span class="sxs-lookup"><span data-stu-id="181ea-159">For example, **[0, 100)** includes all integers greater than or equal 0 and less than 100.</span></span> <span data-ttu-id="181ea-160">Všimněte si, že se podporují více rozsahů může bod toohello stejné databáze a nesouvislé rozsahy (například [100,200) a [400,600) i C tooDatabase bodu v následujícím příkladu hello.)</span><span class="sxs-lookup"><span data-stu-id="181ea-160">Note that multiple ranges can point toohello same database, and disjoint ranges are supported (e.g., [100,200) and [400,600) both point tooDatabase C in hello example below.)</span></span>

| <span data-ttu-id="181ea-161">Klíč</span><span class="sxs-lookup"><span data-stu-id="181ea-161">Key</span></span> | <span data-ttu-id="181ea-162">Umístění horizontálního oddílu</span><span class="sxs-lookup"><span data-stu-id="181ea-162">Shard Location</span></span> |
| --- | --- |
| <span data-ttu-id="181ea-163">[1,50)</span><span class="sxs-lookup"><span data-stu-id="181ea-163">[1,50)</span></span> |<span data-ttu-id="181ea-164">Database_A</span><span class="sxs-lookup"><span data-stu-id="181ea-164">Database_A</span></span> |
| <span data-ttu-id="181ea-165">[50,100)</span><span class="sxs-lookup"><span data-stu-id="181ea-165">[50,100)</span></span> |<span data-ttu-id="181ea-166">Database_B</span><span class="sxs-lookup"><span data-stu-id="181ea-166">Database_B</span></span> |
| <span data-ttu-id="181ea-167">[100,200)</span><span class="sxs-lookup"><span data-stu-id="181ea-167">[100,200)</span></span> |<span data-ttu-id="181ea-168">Database_C</span><span class="sxs-lookup"><span data-stu-id="181ea-168">Database_C</span></span> |
| <span data-ttu-id="181ea-169">[400,600)</span><span class="sxs-lookup"><span data-stu-id="181ea-169">[400,600)</span></span> |<span data-ttu-id="181ea-170">Database_C</span><span class="sxs-lookup"><span data-stu-id="181ea-170">Database_C</span></span> |
| <span data-ttu-id="181ea-171">Tlačítka ...</span><span class="sxs-lookup"><span data-stu-id="181ea-171">...</span></span> |<span data-ttu-id="181ea-172">Tlačítka ...</span><span class="sxs-lookup"><span data-stu-id="181ea-172">...</span></span> |

<span data-ttu-id="181ea-173">Každý hello tabulky uvedené výše je koncepční příklad **ShardMap** objektu.</span><span class="sxs-lookup"><span data-stu-id="181ea-173">Each of hello tables shown above is a conceptual example of a **ShardMap** object.</span></span> <span data-ttu-id="181ea-174">Každý řádek je zjednodušená příklad jednotlivce **PointMapping** (pro hello seznam horizontálních map) nebo **RangeMapping** (pro mapu horizontálního oddílu rozsah hello) objektu.</span><span class="sxs-lookup"><span data-stu-id="181ea-174">Each row is a simplified example of an individual **PointMapping** (for hello list shard map) or **RangeMapping** (for hello range shard map) object.</span></span>

## <a name="shard-map-manager"></a><span data-ttu-id="181ea-175">Správce horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="181ea-175">Shard map manager</span></span>
<span data-ttu-id="181ea-176">V hello klientské knihovny správce mapy horizontálního oddílu hello je kolekce horizontálního oddílu mapy.</span><span class="sxs-lookup"><span data-stu-id="181ea-176">In hello client library, hello shard map  manager is a collection of shard maps.</span></span> <span data-ttu-id="181ea-177">Hello data spravovaná přes **ShardMapManager** instance je udržováno na třech místech:</span><span class="sxs-lookup"><span data-stu-id="181ea-177">hello data managed by a **ShardMapManager** instance is kept in three places:</span></span> 

1. <span data-ttu-id="181ea-178">**Globální horizontálního oddílu mapy (GSM)**: Zadejte tooserve databáze jako hello úložiště pro všechny její horizontálního oddílu mapy a mapování.</span><span class="sxs-lookup"><span data-stu-id="181ea-178">**Global Shard Map (GSM)**: You specify a database tooserve as hello repository for all of its shard maps and mappings.</span></span> <span data-ttu-id="181ea-179">Speciální tabulek a uložených procedur se automaticky vytvoří toomanage hello informace.</span><span class="sxs-lookup"><span data-stu-id="181ea-179">Special tables and stored procedures are automatically created toomanage hello information.</span></span> <span data-ttu-id="181ea-180">To je obvykle s malou databází a lehkým získat přístup, a nesmí být použita pro jiné potřebám aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="181ea-180">This is typically a small database and lightly accessed, and it should not be used for other needs of hello application.</span></span> <span data-ttu-id="181ea-181">Hello tabulky jsou v speciální schéma s názvem **__ShardManagement**.</span><span class="sxs-lookup"><span data-stu-id="181ea-181">hello tables are in a special schema named **__ShardManagement**.</span></span> 
2. <span data-ttu-id="181ea-182">**Místní ID horizontálního oddílu mapy (LSM)**: každou databázi, která zadáte toobe horizontálního oddílu je upravena toocontain několik malých tabulky a speciální uložené procedury, které obsahují a spravovat horizontálního oddílu mapy informace o konkrétní toothat horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="181ea-182">**Local Shard Map (LSM)**: Every database that you specify toobe a shard is modified toocontain several small tables and special stored procedures that contain and manage shard map information specific toothat shard.</span></span> <span data-ttu-id="181ea-183">Tyto informace jsou redundantní s informacemi o hello v hello GSM a umožňuje hello informace o mapování horizontálních aplikaci toovalidate do mezipaměti bez uvedení žádné zatížení na hello GSM; aplikace Hello používá hello LSM toodetermine, pokud v mezipaměti mapování je stále platný.</span><span class="sxs-lookup"><span data-stu-id="181ea-183">This information is redundant with hello information in hello GSM, and it allows hello application toovalidate cached shard map information without placing any load on hello GSM; hello application uses hello LSM toodetermine if a cached mapping is still valid.</span></span> <span data-ttu-id="181ea-184">Hello tabulky odpovídající toohello LSM na každý horizontálního oddílu jsou i ve schématu hello **__ShardManagement**.</span><span class="sxs-lookup"><span data-stu-id="181ea-184">hello tables corresponding toohello LSM on each shard are also in hello schema **__ShardManagement**.</span></span>
3. <span data-ttu-id="181ea-185">**Mezipaměť aplikací**: každý přístup instance aplikace **ShardMapManager** objekt udržuje místní mezipaměť v paměti její mapování.</span><span class="sxs-lookup"><span data-stu-id="181ea-185">**Application cache**: Each application instance accessing a **ShardMapManager** object maintains a local in-memory cache of its mappings.</span></span> <span data-ttu-id="181ea-186">Uloží informace o směrování, která nedávno byla načtena.</span><span class="sxs-lookup"><span data-stu-id="181ea-186">It stores routing information that has recently been retrieved.</span></span> 

## <a name="constructing-a-shardmapmanager"></a><span data-ttu-id="181ea-187">Vytváření ShardMapManager</span><span class="sxs-lookup"><span data-stu-id="181ea-187">Constructing a ShardMapManager</span></span>
<span data-ttu-id="181ea-188">A **ShardMapManager** objektu je vytvořený [factory](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx) vzor.</span><span class="sxs-lookup"><span data-stu-id="181ea-188">A **ShardMapManager** object is constructed using a [factory](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx) pattern.</span></span> <span data-ttu-id="181ea-189">Hello  **[ShardMapManagerFactory.GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**  metoda přebírá přihlašovací údaje (včetně hello název serveru a název databáze, která uchovává hello GSM) hello formu  **ConnectionString** a vrací instanci třídy **ShardMapManager**.</span><span class="sxs-lookup"><span data-stu-id="181ea-189">hello **[ShardMapManagerFactory.GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)** method takes credentials (including hello server name and database name holding hello GSM) in hello form of a **ConnectionString** and returns an instance of a **ShardMapManager**.</span></span>  

<span data-ttu-id="181ea-190">**Poznámka:** hello **ShardMapManager** by měla být vytvořena instance pouze jednou v každé doméně aplikace hello inicializace kódu pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="181ea-190">**Please Note:** hello **ShardMapManager** should be instantiated only once per app domain, within hello initialization code for an application.</span></span> <span data-ttu-id="181ea-191">Vytvoření dalších instancí ShardMapManager v hello stejné appdomain způsobí vyšší paměť a využití CPU aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="181ea-191">Creation of additional instances of ShardMapManager in hello same appdomain, will result in increased memory and CPU utilization of hello application.</span></span> <span data-ttu-id="181ea-192">A **ShardMapManager** může obsahovat libovolný počet horizontálních mapy.</span><span class="sxs-lookup"><span data-stu-id="181ea-192">A **ShardMapManager** can contain any number of shard maps.</span></span> <span data-ttu-id="181ea-193">Mapu jeden horizontálního oddílu může být dostatečné pro mnoho aplikací, jsou časy při použití různé sady databází pro jiné schéma nebo pro jedinečné účely; v těchto případech může být vhodnější než více mapami horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="181ea-193">While a single shard map may be sufficient for many applications, there are times when different sets of databases are used for different schema or for unique purposes; in those cases multiple shard maps may be preferable.</span></span> 

<span data-ttu-id="181ea-194">V tomto kódu aplikace pokusí tooopen existující **ShardMapManager** s hello [TryGetSqlShardMapManager metoda](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx).</span><span class="sxs-lookup"><span data-stu-id="181ea-194">In this code, an application tries tooopen an existing **ShardMapManager** with hello [TryGetSqlShardMapManager method](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx).</span></span>  <span data-ttu-id="181ea-195">Pokud objekty, které představují globální **ShardMapManager** (GSM) není dosud neexistuje uvnitř hello databáze, hello klientské knihovny vytvoří je existuje pomocí hello [CreateSqlShardMapManager metoda](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx).</span><span class="sxs-lookup"><span data-stu-id="181ea-195">If objects representing a Global **ShardMapManager** (GSM) do not yet exist inside hello database, hello client library creates them there using hello [CreateSqlShardMapManager method](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx).</span></span>

    // Try tooget a reference toohello Shard Map Manager 
     // via hello Shard Map Manager database.  
    // If it doesn't already exist, then create it. 
    ShardMapManager shardMapManager; 
    bool shardMapManagerExists = ShardMapManagerFactory.TryGetSqlShardMapManager(
                                        connectionString, 
                                        ShardMapManagerLoadPolicy.Lazy, 
                                        out shardMapManager); 

    if (shardMapManagerExists) 
     { 
        Console.WriteLine("Shard Map Manager already exists");
    } 
    else
    {
        // Create hello Shard Map Manager. 
        ShardMapManagerFactory.CreateSqlShardMapManager(connectionString);
        Console.WriteLine("Created SqlShardMapManager"); 

        shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager(
            connectionString, 
            ShardMapManagerLoadPolicy.Lazy);

        // hello connectionString contains server name, database name, and admin credentials 
        // for privileges on both hello GSM and hello shards themselves.
    } 

<span data-ttu-id="181ea-196">Jako alternativu můžete použít Powershell toocreate nového správce mapy horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="181ea-196">As an alternative, you can use Powershell toocreate a new Shard Map Manager.</span></span> <span data-ttu-id="181ea-197">Příkladem je k dispozici [zde](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span><span class="sxs-lookup"><span data-stu-id="181ea-197">An example is available [here](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span></span>

## <a name="get-a-rangeshardmap-or-listshardmap"></a><span data-ttu-id="181ea-198">Získání RangeShardMap nebo ListShardMap</span><span class="sxs-lookup"><span data-stu-id="181ea-198">Get a RangeShardMap or ListShardMap</span></span>
<span data-ttu-id="181ea-199">Po vytvoření horizontálního oddílu mapa správce, můžete získat hello [RangeShardMap](https://msdn.microsoft.com/library/azure/dn807318.aspx) nebo [ListShardMap](https://msdn.microsoft.com/library/azure/dn807370.aspx) pomocí hello [TryGetRangeShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), hello [ TryGetListShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx), nebo hello [GetShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) metoda.</span><span class="sxs-lookup"><span data-stu-id="181ea-199">After creating a shard map manager, you can get hello [RangeShardMap](https://msdn.microsoft.com/library/azure/dn807318.aspx) or [ListShardMap](https://msdn.microsoft.com/library/azure/dn807370.aspx) using hello [TryGetRangeShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), hello [TryGetListShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx), or hello [GetShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) method.</span></span>

    /// <summary>
    /// Creates a new Range Shard Map with hello specified name, or gets hello Range Shard Map if it already exists.
    /// </summary>
    public static RangeShardMap<T> CreateOrGetRangeShardMap<T>(ShardMapManager shardMapManager, string shardMapName)
    {
        // Try tooget a reference toohello Shard Map.
        RangeShardMap<T> shardMap;
        bool shardMapExists = shardMapManager.TryGetRangeShardMap(shardMapName, out shardMap);

        if (shardMapExists)
        {
            ConsoleUtils.WriteInfo("Shard Map {0} already exists", shardMap.Name);
        }
        else
        {
            // hello Shard Map does not exist, so create it
            shardMap = shardMapManager.CreateRangeShardMap<T>(shardMapName);
            ConsoleUtils.WriteInfo("Created Shard Map {0}", shardMap.Name);
        }

        return shardMap;
    } 

### <a name="shard-map-administration-credentials"></a><span data-ttu-id="181ea-200">Pověření správce horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="181ea-200">Shard map administration credentials</span></span>
<span data-ttu-id="181ea-201">Aplikace, které Správa a zpracování horizontálního oddílu maps se liší od těch, které používají hello horizontálního oddílu mapy tooroute připojení.</span><span class="sxs-lookup"><span data-stu-id="181ea-201">Applications that administer and manipulate shard maps are different from those that use hello shard maps tooroute connections.</span></span> 

<span data-ttu-id="181ea-202">mapuje tooadminister horizontálních (Přidat nebo změnit horizontálních oddílů, map horizontálního oddílu, mapování horizontálních, atd.) je třeba vytvořit instanci hello **ShardMapManager** pomocí **přihlašovací údaje, které mají oprávnění na obou hello GSM databáze a na každém pro čtení a zápis databáze, která slouží jako horizontálního oddílu**.</span><span class="sxs-lookup"><span data-stu-id="181ea-202">tooadminister shard maps (add or change shards, shard maps, shard mappings, etc.) you must instantiate hello **ShardMapManager** using **credentials that have read/write privileges on both hello GSM database and on each database that serves as a shard**.</span></span> <span data-ttu-id="181ea-203">přihlašovací údaje Hello musí povolovat zápisu hello tabulek v obou hello GSM a LSM informace ID horizontálního oddílu mapy zadat nebo změnit, stejně jako u vytváření tabulek LSM na nové horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="181ea-203">hello credentials must allow for writes against hello tables in both hello GSM and LSM as shard map information is entered or changed, as well as for creating LSM tables on new shards.</span></span>  

<span data-ttu-id="181ea-204">V tématu [použita pověření tooaccess hello elastické databáze klientské knihovny](sql-database-elastic-scale-manage-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="181ea-204">See [Credentials used tooaccess hello Elastic Database client library](sql-database-elastic-scale-manage-credentials.md).</span></span>

### <a name="only-metadata-affected"></a><span data-ttu-id="181ea-205">Vliv pouze metadata.</span><span class="sxs-lookup"><span data-stu-id="181ea-205">Only metadata affected</span></span>
<span data-ttu-id="181ea-206">Metody používané pro vyplnění nebo změna hello **ShardMapManager** data nijak nemění hello uživatelská data uložená v horizontálních oddílů hello sami.</span><span class="sxs-lookup"><span data-stu-id="181ea-206">Methods used for populating or changing hello **ShardMapManager** data do not alter hello user data stored in hello shards themselves.</span></span> <span data-ttu-id="181ea-207">Například metody, jako **CreateShard**, **DeleteShard**, **UpdateMapping**atd ovlivnit hello horizontálního oddílu mapy obsahují pouze metadata.</span><span class="sxs-lookup"><span data-stu-id="181ea-207">For example, methods such as **CreateShard**, **DeleteShard**, **UpdateMapping**, etc. affect hello shard map metadata only.</span></span> <span data-ttu-id="181ea-208">Není odebrat, přidat nebo změnit uživatele data obsažená v hello horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="181ea-208">They do not remove, add, or alter user data contained in hello shards.</span></span> <span data-ttu-id="181ea-209">Místo toho tyto metody jsou navrženou toobe používá ve spojení s samostatné operace můžete provádět toocreate nebo odebrání skutečná databáze, nebo že přesunutí řádky jeden horizontálních tooanother toorebalance horizontálně dělené prostředí.</span><span class="sxs-lookup"><span data-stu-id="181ea-209">Instead, these methods are designed toobe used in conjunction with separate operations you perform toocreate or remove actual databases, or that move rows from one shard tooanother toorebalance a sharded environment.</span></span>  <span data-ttu-id="181ea-210">(hello **rozdělení sloučení** nástroje dodávaná s nástroje elastické databáze využívá tato rozhraní API společně s Orchestrace vlastní přesun dat. mezi horizontálních oddílů.) V tématu [škálování, pomocí nástroje hello elastické databáze rozdělení sloučení](sql-database-elastic-scale-overview-split-and-merge.md).</span><span class="sxs-lookup"><span data-stu-id="181ea-210">(hello **split-merge** tool included with elastic database tools makes use of these APIs along with orchestrating actual data movement between shards.) See [Scaling using hello Elastic Database split-merge tool](sql-database-elastic-scale-overview-split-and-merge.md).</span></span>

## <a name="populating-a-shard-map-example"></a><span data-ttu-id="181ea-211">Sestavování příklad horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="181ea-211">Populating a shard map example</span></span>
<span data-ttu-id="181ea-212">Ukázková posloupnost operací toopopulate mapu konkrétní horizontálního oddílu jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="181ea-212">An example sequence of operations toopopulate a specific shard map is shown below.</span></span> <span data-ttu-id="181ea-213">Kód Hello provádí tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="181ea-213">hello code performs these steps:</span></span> 

1. <span data-ttu-id="181ea-214">Nové mapování horizontálních je vytvořen v rámci správce mapy horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="181ea-214">A new shard map is created within a shard map manager.</span></span> 
2. <span data-ttu-id="181ea-215">Hello metadata pro dva různé horizontálních oddílů je přidán toohello horizontálního oddílu mapy.</span><span class="sxs-lookup"><span data-stu-id="181ea-215">hello metadata for two different shards is added toohello shard map.</span></span> 
3. <span data-ttu-id="181ea-216">Budou přidány různé klíče rozsah mapování a hello celkové mapy horizontálního oddílu hello se zobrazí obsah.</span><span class="sxs-lookup"><span data-stu-id="181ea-216">A variety of key range mappings are added, and hello overall contents of hello shard map are displayed.</span></span> 

<span data-ttu-id="181ea-217">Kód Hello je napsán tak, aby hello metoda může být znovu, pokud dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="181ea-217">hello code is written so that hello method can be rerun if an error occurs.</span></span> <span data-ttu-id="181ea-218">Každý požadavek testuje, jestli horizontálního oddílu nebo mapování už existuje, před pokusem o toocreate ho.</span><span class="sxs-lookup"><span data-stu-id="181ea-218">Each request tests whether a shard or mapping already exists, before attempting toocreate it.</span></span> <span data-ttu-id="181ea-219">Hello kód předpokládá, že databáze s názvem **sample_shard_0**, **sample_shard_1** a **sample_shard_2** již byly vytvořeny v odkazuje řetězec serverhello**shardServer**.</span><span class="sxs-lookup"><span data-stu-id="181ea-219">hello code assumes that databases named **sample_shard_0**, **sample_shard_1** and **sample_shard_2** have already been created in hello server referenced by string **shardServer**.</span></span> 

    public void CreatePopulatedRangeMap(ShardMapManager smm, string mapName) 
        {            
            RangeShardMap<long> sm = null; 

            // check if shardmap exists and if not, create it 
            if (!smm.TryGetRangeShardMap(mapName, out sm)) 
            { 
                sm = smm.CreateRangeShardMap<long>(mapName); 
            } 

            Shard shard0 = null, shard1=null; 
            // Check if shard exists and if not, 
            // create it (Idempotent / tolerant of re-execute) 
            if (!sm.TryGetShard(new ShardLocation(
                                     shardServer, 
                                     "sample_shard_0"), 
                                     out shard0)) 
            { 
                Shard0 = sm.CreateShard(new ShardLocation(
                                            shardServer, 
                                            "sample_shard_0")); 
            } 

            if (!sm.TryGetShard(new ShardLocation(
                                    shardServer, 
                                    "sample_shard_1"), 
                                    out shard1)) 
            { 
                Shard1 = sm.CreateShard(new ShardLocation(
                                             shardServer, 
                                            "sample_shard_1"));  
            } 

            RangeMapping<long> rmpg=null; 

            // Check if mapping exists and if not,
            // create it (Idempotent / tolerant of re-execute) 
            if (!sm.TryGetMappingForKey(0, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                          new RangeMappingCreationInfo<long>
                          (new Range<long>(0, 50), 
                          shard0, 
                          MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(50, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long> 
                         (new Range<long>(50, 100), 
                         shard1, 
                         MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(100, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long>
                         (new Range<long>(100, 150), 
                         shard0, 
                         MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(150, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long> 
                         (new Range<long>(150, 200), 
                         shard1, 
                         MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(200, out rmpg)) 
            { 
               sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long> 
                         (new Range<long>(200, 300), 
                         shard0, 
                         MappingStatus.Online)); 
            } 

            // List hello shards and mappings 
            foreach (Shard s in sm.GetShards()
                         .OrderBy(s => s.Location.DataSource)
                         .ThenBy(s => s.Location.Database))
            { 
               Console.WriteLine("shard: "+ s.Location); 
            } 

            foreach (RangeMapping<long> rm in sm.GetMappings()) 
            { 
                Console.WriteLine("range: [" + rm.Value.Low.ToString() + ":" 
                        + rm.Value.High.ToString()+ ")  ==>" +rm.Shard.Location); 
            } 
        } 

<span data-ttu-id="181ea-220">Jako alternativu můžete použít PowerShell skriptů tooachieve hello stejný výsledek.</span><span class="sxs-lookup"><span data-stu-id="181ea-220">As an alternative you can use PowerShell scripts tooachieve hello same result.</span></span> <span data-ttu-id="181ea-221">Některé příklady prostředí PowerShell ukázkový hello jsou k dispozici [zde](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span><span class="sxs-lookup"><span data-stu-id="181ea-221">Some of hello sample PowerShell examples are available [here](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span></span>     

<span data-ttu-id="181ea-222">Jakmile jsou vyplněna horizontálního oddílu mapy, data přístup k aplikacím je možné vytvořit nebo přizpůsobit toowork s hello mapy.</span><span class="sxs-lookup"><span data-stu-id="181ea-222">Once shard maps have been populated, data access applications can be created or adapted toowork with hello maps.</span></span> <span data-ttu-id="181ea-223">Naplnění nebo manipulace s hello mapy nemusí dojít znovu, dokud **mapy rozložení** musí toochange.</span><span class="sxs-lookup"><span data-stu-id="181ea-223">Populating or manipulating hello maps need not occur again until **map layout** needs toochange.</span></span>  

## <a name="data-dependent-routing"></a><span data-ttu-id="181ea-224">Směrování závislé na datech</span><span class="sxs-lookup"><span data-stu-id="181ea-224">Data dependent routing</span></span>
<span data-ttu-id="181ea-225">správce mapy horizontálního oddílu Hello nejvíce použije v aplikacích, které vyžadují databázi připojení tooperform hello data specifická pro aplikaci operations.</span><span class="sxs-lookup"><span data-stu-id="181ea-225">hello shard map manager will be most used in applications that require database connections tooperform hello app-specific data operations.</span></span> <span data-ttu-id="181ea-226">Tato připojení musí být přidružen hello správné databázi.</span><span class="sxs-lookup"><span data-stu-id="181ea-226">Those connections must be associated with hello correct database.</span></span> <span data-ttu-id="181ea-227">To se označuje jako **závislé směrování dat**.</span><span class="sxs-lookup"><span data-stu-id="181ea-227">This is known as **Data Dependent Routing**.</span></span> <span data-ttu-id="181ea-228">Pro tyto aplikace vytvořit instanci objektu manager mapy horizontálního oddílu z objektu pro vytváření hello pomocí přihlašovacích údajů, které mají oprávnění jen pro čtení na hello GSM databáze.</span><span class="sxs-lookup"><span data-stu-id="181ea-228">For these applications, instantiate a shard map manager object from hello factory using credentials that have read-only access on hello GSM database.</span></span> <span data-ttu-id="181ea-229">Jednotlivých požadavků pro pozdější připojení zadat přihlašovací údaje potřebné pro připojení databáze toohello odpovídající horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="181ea-229">Individual requests for later connections supply credentials necessary for connecting toohello appropriate shard database.</span></span>

<span data-ttu-id="181ea-230">Všimněte si, že tyto aplikace (pomocí **ShardMapManager** otevřít s přihlašovacími údaji jen pro čtení) nelze provádět změny mapování nebo toohello mapy.</span><span class="sxs-lookup"><span data-stu-id="181ea-230">Note that these applications (using **ShardMapManager** opened with read-only credentials) cannot make changes toohello maps or mappings.</span></span> <span data-ttu-id="181ea-231">Pro tyto potřeby vytvořte správu konkrétní aplikace nebo skripty prostředí PowerShell, které zadat přihlašovací údaje pro vyšší úrovní oprávnění, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="181ea-231">For those needs, create administrative-specific applications or PowerShell scripts that supply higher-privileged credentials as discussed earlier.</span></span> <span data-ttu-id="181ea-232">V tématu [použita pověření tooaccess hello elastické databáze klientské knihovny](sql-database-elastic-scale-manage-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="181ea-232">See [Credentials used tooaccess hello Elastic Database client library](sql-database-elastic-scale-manage-credentials.md).</span></span>

<span data-ttu-id="181ea-233">Další podrobnosti najdete v tématu [závislé směrování dat](sql-database-elastic-scale-data-dependent-routing.md).</span><span class="sxs-lookup"><span data-stu-id="181ea-233">For more details, see [Data dependent routing](sql-database-elastic-scale-data-dependent-routing.md).</span></span> 

## <a name="modifying-a-shard-map"></a><span data-ttu-id="181ea-234">Úprava horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="181ea-234">Modifying a shard map</span></span>
<span data-ttu-id="181ea-235">Mapování horizontálních lze změnit různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="181ea-235">A shard map can be changed in different ways.</span></span> <span data-ttu-id="181ea-236">Všechny následující metody hello upravit hello metadat, která popisují hello horizontálních oddílů a jejich mapování, ale fyzicky nemění data v rámci hello horizontálních oddílů, ani se jejich vytvoření nebo odstranění hello skutečné databáze.</span><span class="sxs-lookup"><span data-stu-id="181ea-236">All of hello following methods modify hello metadata describing hello shards and their mappings, but they do not physically modify data within hello shards, nor do they create or delete hello actual databases.</span></span>  <span data-ttu-id="181ea-237">Některé operace hello na mapě horizontálního oddílu hello popsané dál může být nutné toobe koordinované s akce správy, který fyzicky přesunout data nebo který přidávat a odebírat databáze slouží jako horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="181ea-237">Some of hello operations on hello shard map described below may need toobe coordinated with administrative actions that physically move data or that add and remove databases serving as shards.</span></span>

<span data-ttu-id="181ea-238">Tyto metody spolupracovat jako stavební bloky hello k dispozici pro úpravy hello celkové distribuci dat ve vašem prostředí horizontálně dělené databáze.</span><span class="sxs-lookup"><span data-stu-id="181ea-238">These methods work together as hello building blocks available for modifying hello overall distribution of data in your sharded database environment.</span></span>  

* <span data-ttu-id="181ea-239">tooadd nebo odeberte horizontálních oddílů: použijte  **[CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx)**  a  **[DeleteShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.deleteshard.aspx)**  z hello [Shardmap třída](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.aspx).</span><span class="sxs-lookup"><span data-stu-id="181ea-239">tooadd or remove shards: use **[CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx)** and **[DeleteShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.deleteshard.aspx)** of hello [Shardmap class](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.aspx).</span></span> 
  
    <span data-ttu-id="181ea-240">Hello server a databáze představující horizontálního oddílu hello cíl již musí existovat pro tyto operace tooexecute.</span><span class="sxs-lookup"><span data-stu-id="181ea-240">hello server and database representing hello target shard must already exist for these operations tooexecute.</span></span> <span data-ttu-id="181ea-241">Tyto metody nemají žádný vliv na hello samotných databázích, jenom na metadata v mapě hello horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="181ea-241">These methods do not have any impact on hello databases themselves, only on metadata in hello shard map.</span></span>
* <span data-ttu-id="181ea-242">body toocreate nebo odeberte nebo rozsahy, které jsou namapované toohello horizontálních oddílů: použijte  **[CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn841993.aspx)**,  **[DeleteMapping](https://msdn.microsoft.com/library/azure/dn824200.aspx)**  z hello [RangeShardMapping třída](https://msdn.microsoft.com/library/azure/dn807318.aspx), a  **[CreatePointMapping](https://msdn.microsoft.com/library/azure/dn807218.aspx)**  z hello [ListShardMap](https://msdn.microsoft.com/library/azure/dn842123.aspx)</span><span class="sxs-lookup"><span data-stu-id="181ea-242">toocreate or remove points or ranges that are mapped toohello shards: use **[CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn841993.aspx)**, **[DeleteMapping](https://msdn.microsoft.com/library/azure/dn824200.aspx)** of hello [RangeShardMapping class](https://msdn.microsoft.com/library/azure/dn807318.aspx), and **[CreatePointMapping](https://msdn.microsoft.com/library/azure/dn807218.aspx)** of hello [ListShardMap](https://msdn.microsoft.com/library/azure/dn842123.aspx)</span></span>
  
    <span data-ttu-id="181ea-243">Mnoho různých bodů nebo rozsahy lze mapovat toohello stejné ID horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="181ea-243">Many different points or ranges can be mapped toohello same shard.</span></span> <span data-ttu-id="181ea-244">Tyto metody ovlivňují jenom metadata – neovlivňují všechna data, která může být již přítomny v horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="181ea-244">These methods only affect metadata - they do not affect any data that may already be present in shards.</span></span> <span data-ttu-id="181ea-245">Pokud data musí odebrat z databáze hello v pořadí toobe konzistentní s toobe **DeleteMapping** operace, budete potřebovat tooperform tyto operace samostatně, ale ve spojení s těmito metodami.</span><span class="sxs-lookup"><span data-stu-id="181ea-245">If data needs toobe removed from hello database in order toobe consistent with **DeleteMapping** operations, you will need tooperform those operations separately but in conjunction with using these methods.</span></span>  
* <span data-ttu-id="181ea-246">existující rozsahy toosplit do dvou nebo sloučení sousedící oblasti do jedné: použijte  **[SplitMapping](https://msdn.microsoft.com/library/azure/dn824205.aspx)**  a  **[MergeMappings](https://msdn.microsoft.com/library/azure/dn824201.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="181ea-246">toosplit existing ranges into two, or merge adjacent ranges into one: use **[SplitMapping](https://msdn.microsoft.com/library/azure/dn824205.aspx)** and **[MergeMappings](https://msdn.microsoft.com/library/azure/dn824201.aspx)**.</span></span>  
  
    <span data-ttu-id="181ea-247">Všimněte si, že rozdělení a operace sloučení **neměňte hello horizontálního oddílu toowhich klíč hodnoty, se namapují**.</span><span class="sxs-lookup"><span data-stu-id="181ea-247">Note that split and merge operations **do not change hello shard toowhich key values are mapped**.</span></span> <span data-ttu-id="181ea-248">Rozdělení stávající rozsah dělí na dvě části, ale ponechá i jako namapované toohello stejné ID horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="181ea-248">A split breaks an existing range into two parts, but leaves both as mapped toohello same shard.</span></span> <span data-ttu-id="181ea-249">Sloučení funguje na dva přiléhající rozsahů, které jsou už namapované toohello stejné ID horizontálního oddílu, je slučování do jednoho rozsahu.</span><span class="sxs-lookup"><span data-stu-id="181ea-249">A merge operates on two adjacent ranges that are already mapped toohello same shard, coalescing them into a single range.</span></span>  <span data-ttu-id="181ea-250">Hello pohybů bodů nebo rozsahy, sami mezi horizontálních oddílů musí toobe koordinované pomocí **UpdateMapping** ve spojení s vlastní přesun dat..</span><span class="sxs-lookup"><span data-stu-id="181ea-250">hello movement of points or ranges themselves between shards needs toobe coordinated by using **UpdateMapping** in conjunction with actual data movement.</span></span>  <span data-ttu-id="181ea-251">Můžete použít hello **rozdělení či sloučení** služba, která je součástí elastické databáze nástroje změny mapování horizontálních toocoordinate s přesun dat potřeby pohyb.</span><span class="sxs-lookup"><span data-stu-id="181ea-251">You can use hello **Split/Merge** service that is part of elastic database tools toocoordinate shard map changes with data movement, when movement is needed.</span></span> 
* <span data-ttu-id="181ea-252">toore mapy (nebo přesunutí) jednotlivé body nebo rozsahy toodifferent horizontálních oddílů: použijte  **[UpdateMapping](https://msdn.microsoft.com/library/azure/dn824207.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="181ea-252">toore-map (or move) individual points or ranges toodifferent shards: use **[UpdateMapping](https://msdn.microsoft.com/library/azure/dn824207.aspx)**.</span></span>  
  
    <span data-ttu-id="181ea-253">Vzhledem k tomu, že dat může být nutné přesunout z jednoho horizontálních tooanother v pořadí toobe konzistentní s toobe **UpdateMapping** operace, budete potřebovat tooperform tento přesun samostatně, ale ve spojení s těmito metodami.</span><span class="sxs-lookup"><span data-stu-id="181ea-253">Since data may need toobe moved from one shard tooanother in order toobe consistent with **UpdateMapping** operations, you will need tooperform that movement separately but in conjunction with using these methods.</span></span>
* <span data-ttu-id="181ea-254">mapování tootake online a offline: použijte  **[MarkMappingOffline](https://msdn.microsoft.com/library/azure/dn824202.aspx)**  a  **[MarkMappingOnline](https://msdn.microsoft.com/library/azure/dn807225.aspx)**  toocontrol hello online stavu mapování.</span><span class="sxs-lookup"><span data-stu-id="181ea-254">tootake mappings online and offline: use **[MarkMappingOffline](https://msdn.microsoft.com/library/azure/dn824202.aspx)** and **[MarkMappingOnline](https://msdn.microsoft.com/library/azure/dn807225.aspx)** toocontrol hello online state of a mapping.</span></span> 
  
    <span data-ttu-id="181ea-255">Mapování horizontálních určité operace jsou povoleny pouze při mapování je ve stavu "offline", včetně **UpdateMapping** a **DeleteMapping**.</span><span class="sxs-lookup"><span data-stu-id="181ea-255">Certain operations on shard mappings are only allowed when a mapping is in an “offline” state, including **UpdateMapping** and **DeleteMapping**.</span></span> <span data-ttu-id="181ea-256">Při mapování je offline, žádost dat závisí na základě klíče, součástí mapování vrátí chybu.</span><span class="sxs-lookup"><span data-stu-id="181ea-256">When a mapping is offline, a data-dependent request based on a key included in that mapping will return an error.</span></span> <span data-ttu-id="181ea-257">Kromě toho když rozsah je nejprve do režimu offline, ID horizontálního oddílu toohello vliv na všechny připojení jsou automaticky ukončená ve výsledcích nekonzistentní nebo neúplné tooprevent pořadí pro dotazy namířená proti rozsahy se změnil.</span><span class="sxs-lookup"><span data-stu-id="181ea-257">In addition, when a range is first taken offline, all connections toohello affected shard are automatically killed in order tooprevent inconsistent or incomplete results for queries directed against ranges being changed.</span></span> 

<span data-ttu-id="181ea-258">Mapování jsou neměnné objekty v rozhraní .net.</span><span class="sxs-lookup"><span data-stu-id="181ea-258">Mappings are immutable objects in .Net.</span></span>  <span data-ttu-id="181ea-259">Všechny metody hello výše, které změní mapování také zneplatnit toothem jakékoli odkazy v kódu.</span><span class="sxs-lookup"><span data-stu-id="181ea-259">All of hello methods above that change mappings also invalidate any references toothem in your code.</span></span> <span data-ttu-id="181ea-260">toomake it jednodušší tooperform pořadí operací, které změny stavu mapování, všechny hello metody, které změní mapování vrátit novou mapování odkaz, takže možné zřetězit operace.</span><span class="sxs-lookup"><span data-stu-id="181ea-260">toomake it easier tooperform sequences of operations that change a mapping’s state, all of hello methods that change a mapping return a new mapping reference, so operations can be chained.</span></span> <span data-ttu-id="181ea-261">Například toodelete existující mapování v shardmap sm, který obsahuje klíč hello 25, můžete provést hello následující:</span><span class="sxs-lookup"><span data-stu-id="181ea-261">For example, toodelete an existing mapping in shardmap sm that contains hello key 25, you can execute hello following:</span></span> 

        sm.DeleteMapping(sm.MarkMappingOffline(sm.GetMappingForKey(25)));

## <a name="adding-a-shard"></a><span data-ttu-id="181ea-262">Přidání horizontálního oddílu</span><span class="sxs-lookup"><span data-stu-id="181ea-262">Adding a shard</span></span>
<span data-ttu-id="181ea-263">Aplikace často potřebují toosimply přidat nová data toohandle horizontálních oddílů, kterou je očekáván z nových klíčů nebo rozsahy klíčů pro ID horizontálního oddílu mapu, která již existuje.</span><span class="sxs-lookup"><span data-stu-id="181ea-263">Applications often need toosimply add new shards toohandle data that is expected from new keys or key ranges, for a shard map that already exists.</span></span> <span data-ttu-id="181ea-264">Například aplikace horizontálně dělené podle ID klienta může potřebovat tooprovision nové horizontálního oddílu pro nového klienta, nebo horizontálně dělené měsíční dat může být nutné nové horizontálních zřídit před hello začátek každý nový měsíc.</span><span class="sxs-lookup"><span data-stu-id="181ea-264">For example, an application sharded by Tenant ID may need tooprovision a new shard for a new tenant, or data sharded monthly may need a new shard provisioned before hello start of each new month.</span></span> 

<span data-ttu-id="181ea-265">Pokud není hello nový rozsah hodnot klíče již součástí stávající mapování a žádné přesun dat je nezbytné, je velmi jednoduchý tooadd hello nové horizontálních a přidružit nový klíč hello nebo rozsah toothat komplikovaného skládání architektury.</span><span class="sxs-lookup"><span data-stu-id="181ea-265">If hello new range of key values is not already part of an existing mapping and no data movement is necessary, it is very simple tooadd hello new shard and associate hello new key or range toothat shard.</span></span> <span data-ttu-id="181ea-266">Podrobnosti o přidání nové horizontálních oddílů najdete v tématu [přidání nové horizontálního oddílu](sql-database-elastic-scale-add-a-shard.md).</span><span class="sxs-lookup"><span data-stu-id="181ea-266">For details on adding new shards, see [Adding a new shard](sql-database-elastic-scale-add-a-shard.md).</span></span>

<span data-ttu-id="181ea-267">Pro scénáře, které vyžadují přesun dat ale nástroj rozdělení sloučení hello je potřebné tooorchestrate hello přesun dat mezi horizontálních oddílů v kombinaci s aktualizace mapování hello nezbytné horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="181ea-267">For scenarios that require data movement, however, hello split-merge tool is needed tooorchestrate hello data movement between shards in combination with hello necessary shard map updates.</span></span> <span data-ttu-id="181ea-268">Podrobnosti o použití hello yool rozdělení sloučení, najdete v části [Přehled rozdělení sloučení](sql-database-elastic-scale-overview-split-and-merge.md)</span><span class="sxs-lookup"><span data-stu-id="181ea-268">For details on using hello split-merge yool, see [Overview of split-merge](sql-database-elastic-scale-overview-split-and-merge.md)</span></span> 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-shard-map-management/listmapping.png
[2]: ./media/sql-database-elastic-scale-shard-map-management/rangemapping.png
[3]: ./media/sql-database-elastic-scale-shard-map-management/multipleonsingledb.png
