---
title: "Škálování Azure SQL database | Microsoft Docs"
description: "Jak používat ShardMapManager, klientské knihovny pro elastické databáze"
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
ms.openlocfilehash: f626cf417d8b3f1761f3c900d49039b3ff83b093
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="scale-out-databases-with-the-shard-map-manager"></a><span data-ttu-id="447ad-103">Horizontální navýšení kapacity databáze pomocí Správce horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="447ad-103">Scale out databases with the shard map manager</span></span>
<span data-ttu-id="447ad-104">Chcete-li snadno škálovat databáze na SQL Azure, použijte správce mapy horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="447ad-104">To easily scale out databases on SQL Azure, use a shard map manager.</span></span> <span data-ttu-id="447ad-105">Mapa správce horizontálního oddílu je speciální databáze, která uchovává globální mapování informace o všech horizontálních oddílů (databáze) v sadě horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="447ad-105">The shard map manager is a special database that maintains global mapping information about all shards (databases) in a shard set.</span></span> <span data-ttu-id="447ad-106">Metadata umožňuje aplikaci připojit ke správné databázi na základě hodnotu **horizontálního dělení klíč**.</span><span class="sxs-lookup"><span data-stu-id="447ad-106">The metadata allows an application to connect to the correct database based upon the value of the **sharding key**.</span></span> <span data-ttu-id="447ad-107">Kromě toho každých horizontálního oddílu v sadě obsahuje mapy, které sledují místní sdílení dat (označované jako **shardlets**).</span><span class="sxs-lookup"><span data-stu-id="447ad-107">In addition, every shard in the set contains maps that track the local shard data (known as **shardlets**).</span></span> 

![Správa mapování horizontálních](./media/sql-database-elastic-scale-shard-map-management/glossary.png)

<span data-ttu-id="447ad-109">Pochopení, jak se vytvářejí tyto mapy je nezbytné pro správu mapy horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="447ad-109">Understanding how these maps are constructed is essential to shard map management.</span></span> <span data-ttu-id="447ad-110">To se provádí pomocí [ShardMapManager třída](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx), který se nachází v [klientské knihovny pro elastické databáze](sql-database-elastic-database-client-library.md) ke správě horizontálního oddílu map.</span><span class="sxs-lookup"><span data-stu-id="447ad-110">This is done using the [ShardMapManager class](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx), found in the [Elastic Database client library](sql-database-elastic-database-client-library.md) to manage shard maps.</span></span>  

## <a name="shard-maps-and-shard-mappings"></a><span data-ttu-id="447ad-111">Mapování horizontálních a horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="447ad-111">Shard maps and shard mappings</span></span>
<span data-ttu-id="447ad-112">Pro každý horizontálního oddílu je nutné vybrat typ mapy horizontálního oddílu k vytvoření.</span><span class="sxs-lookup"><span data-stu-id="447ad-112">For each shard, you must select the type of shard map to create.</span></span> <span data-ttu-id="447ad-113">Výběr závisí na architektuře databáze:</span><span class="sxs-lookup"><span data-stu-id="447ad-113">The choice depends on the database architecture:</span></span> 

1. <span data-ttu-id="447ad-114">Jednoho klienta na databázi</span><span class="sxs-lookup"><span data-stu-id="447ad-114">Single tenant per database</span></span>  
2. <span data-ttu-id="447ad-115">Několik klientů na databázi (dva typy):</span><span class="sxs-lookup"><span data-stu-id="447ad-115">Multiple tenants per database (two types):</span></span>
   1. <span data-ttu-id="447ad-116">Seznam mapování</span><span class="sxs-lookup"><span data-stu-id="447ad-116">List mapping</span></span>
   2. <span data-ttu-id="447ad-117">Mapování rozsahu</span><span class="sxs-lookup"><span data-stu-id="447ad-117">Range mapping</span></span>

<span data-ttu-id="447ad-118">Pro jednoho klienta modelu, vytvořit **seznamu mapování** horizontálního oddílu mapy.</span><span class="sxs-lookup"><span data-stu-id="447ad-118">For a single-tenant model, create a **list mapping** shard map.</span></span> <span data-ttu-id="447ad-119">Model jednoho klienta přiřadí jednu databázi na každého klienta.</span><span class="sxs-lookup"><span data-stu-id="447ad-119">The single-tenant model assigns one database per tenant.</span></span> <span data-ttu-id="447ad-120">Jedná se efektivní model pro vývojáře SaaS, protože ho zjednodušuje správu.</span><span class="sxs-lookup"><span data-stu-id="447ad-120">This is an effective model for SaaS developers as it simplifies management.</span></span>

![Seznam mapování][1]

<span data-ttu-id="447ad-122">Víceklientského modelu přiřadí několik klientů jedné databáze (a skupin klientů, které můžete distribuovat mezi více databází).</span><span class="sxs-lookup"><span data-stu-id="447ad-122">The multi-tenant model assigns several tenants to a single database (and you can distribute groups of tenants across multiple databases).</span></span> <span data-ttu-id="447ad-123">Pomocí tohoto modelu, pokud očekáváte, že každý klient k potřebami malá data.</span><span class="sxs-lookup"><span data-stu-id="447ad-123">Use this model when you expect each tenant to have small data needs.</span></span> <span data-ttu-id="447ad-124">V tomto modelu jsme řadu klienty přiřadit k databázi pomocí **rozsah mapování**.</span><span class="sxs-lookup"><span data-stu-id="447ad-124">In this model, we assign a range of tenants to a database using **range mapping**.</span></span> 

![Mapování rozsahu][2]

<span data-ttu-id="447ad-126">Nebo můžete implementovat s použitím modelu víceklientské databáze *seznamu mapování* přiřadit více klientů pro jednu databázi.</span><span class="sxs-lookup"><span data-stu-id="447ad-126">Or you can implement a multi-tenant database model using a *list mapping* to assign multiple tenants to a single database.</span></span> <span data-ttu-id="447ad-127">Například DB1 se používá k ukládání informací o id klienta 1 a 5 a DB2 ukládá data pro klienta 7 a klienta 10.</span><span class="sxs-lookup"><span data-stu-id="447ad-127">For example, DB1 is used to store information about tenant id 1 and 5, and DB2 stores data for tenant 7 and tenant 10.</span></span> 

![Vnořením klientů na jednom DB][3] 

### <a name="supported-net-types-for-sharding-keys"></a><span data-ttu-id="447ad-129">Podporované typy .net pro klíče horizontálního dělení</span><span class="sxs-lookup"><span data-stu-id="447ad-129">Supported .Net types for sharding keys</span></span>
<span data-ttu-id="447ad-130">Elastické škálování podpora následující rozhraní .net Framework typy jako horizontálního dělení klíče:</span><span class="sxs-lookup"><span data-stu-id="447ad-130">Elastic Scale support the following .Net Framework types as sharding keys:</span></span>

* <span data-ttu-id="447ad-131">celé číslo</span><span class="sxs-lookup"><span data-stu-id="447ad-131">integer</span></span>
* <span data-ttu-id="447ad-132">dlouhá</span><span class="sxs-lookup"><span data-stu-id="447ad-132">long</span></span>
* <span data-ttu-id="447ad-133">Identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="447ad-133">guid</span></span>
* <span data-ttu-id="447ad-134">Byte]</span><span class="sxs-lookup"><span data-stu-id="447ad-134">byte[]</span></span>  
* <span data-ttu-id="447ad-135">Data a času</span><span class="sxs-lookup"><span data-stu-id="447ad-135">datetime</span></span>
* <span data-ttu-id="447ad-136">Časový interval</span><span class="sxs-lookup"><span data-stu-id="447ad-136">timespan</span></span>
* <span data-ttu-id="447ad-137">Datový typ DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="447ad-137">datetimeoffset</span></span>

### <a name="list-and-range-shard-maps"></a><span data-ttu-id="447ad-138">Seznam a rozsah horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="447ad-138">List and range shard maps</span></span>
<span data-ttu-id="447ad-139">Mapování horizontálních se dá vytvořit pomocí **seznam jednotlivých horizontálního dělení hodnoty klíče**, nebo se dá vytvořit pomocí **rozsahy horizontálního dělení hodnoty klíče**.</span><span class="sxs-lookup"><span data-stu-id="447ad-139">Shard maps can be constructed using **lists of individual sharding key values**, or they can be constructed using **ranges of sharding key values**.</span></span> 

### <a name="list-shard-maps"></a><span data-ttu-id="447ad-140">Seznam horizontálních mapy</span><span class="sxs-lookup"><span data-stu-id="447ad-140">List shard maps</span></span>
<span data-ttu-id="447ad-141">**Horizontálních oddílů** obsahovat **shardlets** a mapování shardlets k horizontálních oddílů je možný díky mapu horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="447ad-141">**Shards** contain **shardlets** and the mapping of shardlets to shards is maintained by a shard map.</span></span> <span data-ttu-id="447ad-142">A **seznam horizontálních mapy** je přidružení mezi jednotlivé hodnoty klíče, které identifikují shardlets a databázemi, které slouží jako horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="447ad-142">A **list shard map** is an association between the individual key values that identify the shardlets and the databases that serve as shards.</span></span>  <span data-ttu-id="447ad-143">**Seznam mapování** jsou klíčem explicitní a jiné hodnoty lze mapovat do stejné databáze.</span><span class="sxs-lookup"><span data-stu-id="447ad-143">**List mappings** are explicit and different key values can be mapped to the same database.</span></span> <span data-ttu-id="447ad-144">Například klíč 1 se mapuje na databázi A a B. databáze odkazovat na klíčové hodnoty 3 a 6</span><span class="sxs-lookup"><span data-stu-id="447ad-144">For example, key 1 maps to Database A, and key values 3 and 6 both reference Database B.</span></span>

| <span data-ttu-id="447ad-145">Klíč</span><span class="sxs-lookup"><span data-stu-id="447ad-145">Key</span></span> | <span data-ttu-id="447ad-146">Umístění horizontálního oddílu</span><span class="sxs-lookup"><span data-stu-id="447ad-146">Shard Location</span></span> |
| --- | --- |
| <span data-ttu-id="447ad-147">1</span><span class="sxs-lookup"><span data-stu-id="447ad-147">1</span></span> |<span data-ttu-id="447ad-148">Database_A</span><span class="sxs-lookup"><span data-stu-id="447ad-148">Database_A</span></span> |
| <span data-ttu-id="447ad-149">3</span><span class="sxs-lookup"><span data-stu-id="447ad-149">3</span></span> |<span data-ttu-id="447ad-150">Database_B</span><span class="sxs-lookup"><span data-stu-id="447ad-150">Database_B</span></span> |
| <span data-ttu-id="447ad-151">4</span><span class="sxs-lookup"><span data-stu-id="447ad-151">4</span></span> |<span data-ttu-id="447ad-152">Database_C</span><span class="sxs-lookup"><span data-stu-id="447ad-152">Database_C</span></span> |
| <span data-ttu-id="447ad-153">6</span><span class="sxs-lookup"><span data-stu-id="447ad-153">6</span></span> |<span data-ttu-id="447ad-154">Database_B</span><span class="sxs-lookup"><span data-stu-id="447ad-154">Database_B</span></span> |
| <span data-ttu-id="447ad-155">Tlačítka ...</span><span class="sxs-lookup"><span data-stu-id="447ad-155">...</span></span> |<span data-ttu-id="447ad-156">Tlačítka ...</span><span class="sxs-lookup"><span data-stu-id="447ad-156">...</span></span> |

### <a name="range-shard-maps"></a><span data-ttu-id="447ad-157">Rozsah horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="447ad-157">Range shard maps</span></span>
<span data-ttu-id="447ad-158">V **rozsah horizontálního oddílu mapy**, klíče rozsah je popsán pár **[hodnota nízká, vysoká hodnota)** kde *nízká hodnota* je minimální klíč v rozsahu a *vysoké hodnoty* první hodnota vyšší než je rozsah.</span><span class="sxs-lookup"><span data-stu-id="447ad-158">In a **range shard map**, the key range is described by a pair **[Low Value, High Value)** where the *Low Value* is the minimum key in the range, and the *High Value* is the first value higher than the range.</span></span> 

<span data-ttu-id="447ad-159">Například **[0, 100)** zahrnuje všechny celá čísla větší než nebo rovna 0 a menší než 100.</span><span class="sxs-lookup"><span data-stu-id="447ad-159">For example, **[0, 100)** includes all integers greater than or equal 0 and less than 100.</span></span> <span data-ttu-id="447ad-160">Všimněte si, že více rozsahům může ukazovat na stejnou databázi a nesouvislý rozsahy jsou podporovány (například [100,200) a [400,600) i bodu c databáze v následujícím příkladu.)</span><span class="sxs-lookup"><span data-stu-id="447ad-160">Note that multiple ranges can point to the same database, and disjoint ranges are supported (e.g., [100,200) and [400,600) both point to Database C in the example below.)</span></span>

| <span data-ttu-id="447ad-161">Klíč</span><span class="sxs-lookup"><span data-stu-id="447ad-161">Key</span></span> | <span data-ttu-id="447ad-162">Umístění horizontálního oddílu</span><span class="sxs-lookup"><span data-stu-id="447ad-162">Shard Location</span></span> |
| --- | --- |
| <span data-ttu-id="447ad-163">[1,50)</span><span class="sxs-lookup"><span data-stu-id="447ad-163">[1,50)</span></span> |<span data-ttu-id="447ad-164">Database_A</span><span class="sxs-lookup"><span data-stu-id="447ad-164">Database_A</span></span> |
| <span data-ttu-id="447ad-165">[50,100)</span><span class="sxs-lookup"><span data-stu-id="447ad-165">[50,100)</span></span> |<span data-ttu-id="447ad-166">Database_B</span><span class="sxs-lookup"><span data-stu-id="447ad-166">Database_B</span></span> |
| <span data-ttu-id="447ad-167">[100,200)</span><span class="sxs-lookup"><span data-stu-id="447ad-167">[100,200)</span></span> |<span data-ttu-id="447ad-168">Database_C</span><span class="sxs-lookup"><span data-stu-id="447ad-168">Database_C</span></span> |
| <span data-ttu-id="447ad-169">[400,600)</span><span class="sxs-lookup"><span data-stu-id="447ad-169">[400,600)</span></span> |<span data-ttu-id="447ad-170">Database_C</span><span class="sxs-lookup"><span data-stu-id="447ad-170">Database_C</span></span> |
| <span data-ttu-id="447ad-171">Tlačítka ...</span><span class="sxs-lookup"><span data-stu-id="447ad-171">...</span></span> |<span data-ttu-id="447ad-172">Tlačítka ...</span><span class="sxs-lookup"><span data-stu-id="447ad-172">...</span></span> |

<span data-ttu-id="447ad-173">Každá z tabulky uvedené výše je koncepční příklad **ShardMap** objektu.</span><span class="sxs-lookup"><span data-stu-id="447ad-173">Each of the tables shown above is a conceptual example of a **ShardMap** object.</span></span> <span data-ttu-id="447ad-174">Každý řádek je zjednodušená příklad jednotlivce **PointMapping** (pro mapu horizontálního oddílu seznamu) nebo **RangeMapping** (pro mapu horizontálního oddílu rozsah) objektu.</span><span class="sxs-lookup"><span data-stu-id="447ad-174">Each row is a simplified example of an individual **PointMapping** (for the list shard map) or **RangeMapping** (for the range shard map) object.</span></span>

## <a name="shard-map-manager"></a><span data-ttu-id="447ad-175">Správce horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="447ad-175">Shard map manager</span></span>
<span data-ttu-id="447ad-176">V knihovně klienta správce mapy horizontálního oddílu je kolekce horizontálního oddílu map.</span><span class="sxs-lookup"><span data-stu-id="447ad-176">In the client library, the shard map  manager is a collection of shard maps.</span></span> <span data-ttu-id="447ad-177">Data spravovaná přes **ShardMapManager** instance je udržováno na třech místech:</span><span class="sxs-lookup"><span data-stu-id="447ad-177">The data managed by a **ShardMapManager** instance is kept in three places:</span></span> 

1. <span data-ttu-id="447ad-178">**Globální horizontálního oddílu mapy (GSM)**: Zadejte databázi, která bude sloužit jako úložiště pro všechna její mapování horizontálních a mapování.</span><span class="sxs-lookup"><span data-stu-id="447ad-178">**Global Shard Map (GSM)**: You specify a database to serve as the repository for all of its shard maps and mappings.</span></span> <span data-ttu-id="447ad-179">Speciální tabulek a uložených procedur se automaticky vytvoří ke správě informací.</span><span class="sxs-lookup"><span data-stu-id="447ad-179">Special tables and stored procedures are automatically created to manage the information.</span></span> <span data-ttu-id="447ad-180">To je obvykle s malou databází a lehkým získat přístup, a nesmí být použita pro ostatní požadavky aplikace.</span><span class="sxs-lookup"><span data-stu-id="447ad-180">This is typically a small database and lightly accessed, and it should not be used for other needs of the application.</span></span> <span data-ttu-id="447ad-181">V tabulkách jsou ve speciální schématu s názvem **__ShardManagement**.</span><span class="sxs-lookup"><span data-stu-id="447ad-181">The tables are in a special schema named **__ShardManagement**.</span></span> 
2. <span data-ttu-id="447ad-182">**Místní ID horizontálního oddílu mapy (LSM)**: každou databázi, která zadáte jako horizontálního oddílu se mění tak, aby obsahovala několik malých tabulky a speciální uložené procedury, které obsahují a spravovat informace o mapování horizontálních specifické pro tento horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="447ad-182">**Local Shard Map (LSM)**: Every database that you specify to be a shard is modified to contain several small tables and special stored procedures that contain and manage shard map information specific to that shard.</span></span> <span data-ttu-id="447ad-183">Tyto informace jsou redundantní s informacemi v GSM a umožní aplikaci ověřit informace uložené v mezipaměti horizontálních mapy bez uvedení žádné zatížení na GSM; aplikace používá LSM k určení, pokud je v mezipaměti mapování stále platný.</span><span class="sxs-lookup"><span data-stu-id="447ad-183">This information is redundant with the information in the GSM, and it allows the application to validate cached shard map information without placing any load on the GSM; the application uses the LSM to determine if a cached mapping is still valid.</span></span> <span data-ttu-id="447ad-184">V tabulkách odpovídající LSM na každý horizontálního oddílu jsou i ve schématu **__ShardManagement**.</span><span class="sxs-lookup"><span data-stu-id="447ad-184">The tables corresponding to the LSM on each shard are also in the schema **__ShardManagement**.</span></span>
3. <span data-ttu-id="447ad-185">**Mezipaměť aplikací**: každý přístup instance aplikace **ShardMapManager** objekt udržuje místní mezipaměť v paměti její mapování.</span><span class="sxs-lookup"><span data-stu-id="447ad-185">**Application cache**: Each application instance accessing a **ShardMapManager** object maintains a local in-memory cache of its mappings.</span></span> <span data-ttu-id="447ad-186">Uloží informace o směrování, která nedávno byla načtena.</span><span class="sxs-lookup"><span data-stu-id="447ad-186">It stores routing information that has recently been retrieved.</span></span> 

## <a name="constructing-a-shardmapmanager"></a><span data-ttu-id="447ad-187">Vytváření ShardMapManager</span><span class="sxs-lookup"><span data-stu-id="447ad-187">Constructing a ShardMapManager</span></span>
<span data-ttu-id="447ad-188">A **ShardMapManager** objektu je vytvořený [factory](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx) vzor.</span><span class="sxs-lookup"><span data-stu-id="447ad-188">A **ShardMapManager** object is constructed using a [factory](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx) pattern.</span></span> <span data-ttu-id="447ad-189"> **[ShardMapManagerFactory.GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**  metoda přijímá pověření (včetně názvu serveru a název databáze, která uchovává GSM) ve formě **ConnectionString** a vrací instanci třídy **ShardMapManager**.</span><span class="sxs-lookup"><span data-stu-id="447ad-189">The **[ShardMapManagerFactory.GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)** method takes credentials (including the server name and database name holding the GSM) in the form of a **ConnectionString** and returns an instance of a **ShardMapManager**.</span></span>  

<span data-ttu-id="447ad-190">**Poznámka:** **ShardMapManager** by měla být vytvořena instance pouze jednou v každé doméně aplikace v rámci kód inicializace pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="447ad-190">**Please Note:** The **ShardMapManager** should be instantiated only once per app domain, within the initialization code for an application.</span></span> <span data-ttu-id="447ad-191">Vytvoření další instance ShardMapManager do stejné domény aplikace, bude výsledkem vyšší paměť a využití CPU aplikace.</span><span class="sxs-lookup"><span data-stu-id="447ad-191">Creation of additional instances of ShardMapManager in the same appdomain, will result in increased memory and CPU utilization of the application.</span></span> <span data-ttu-id="447ad-192">A **ShardMapManager** může obsahovat libovolný počet horizontálních mapy.</span><span class="sxs-lookup"><span data-stu-id="447ad-192">A **ShardMapManager** can contain any number of shard maps.</span></span> <span data-ttu-id="447ad-193">Mapu jeden horizontálního oddílu může být dostatečné pro mnoho aplikací, jsou časy při použití různé sady databází pro jiné schéma nebo pro jedinečné účely; v těchto případech může být vhodnější než více mapami horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="447ad-193">While a single shard map may be sufficient for many applications, there are times when different sets of databases are used for different schema or for unique purposes; in those cases multiple shard maps may be preferable.</span></span> 

<span data-ttu-id="447ad-194">V tomto kódu aplikace pokusí otevřít existující **ShardMapManager** s [TryGetSqlShardMapManager metoda](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx).</span><span class="sxs-lookup"><span data-stu-id="447ad-194">In this code, an application tries to open an existing **ShardMapManager** with the [TryGetSqlShardMapManager method](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx).</span></span>  <span data-ttu-id="447ad-195">Pokud objekty, které představují globální **ShardMapManager** (GSM) není dosud neexistuje v databázi, klientské knihovny vytvoří je existuje pomocí [CreateSqlShardMapManager metoda](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx).</span><span class="sxs-lookup"><span data-stu-id="447ad-195">If objects representing a Global **ShardMapManager** (GSM) do not yet exist inside the database, the client library creates them there using the [CreateSqlShardMapManager method](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx).</span></span>

    // Try to get a reference to the Shard Map Manager 
     // via the Shard Map Manager database.  
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
        // Create the Shard Map Manager. 
        ShardMapManagerFactory.CreateSqlShardMapManager(connectionString);
        Console.WriteLine("Created SqlShardMapManager"); 

        shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager(
            connectionString, 
            ShardMapManagerLoadPolicy.Lazy);

        // The connectionString contains server name, database name, and admin credentials 
        // for privileges on both the GSM and the shards themselves.
    } 

<span data-ttu-id="447ad-196">Jako alternativu můžete použít Powershell k vytvoření nového správce mapy horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="447ad-196">As an alternative, you can use Powershell to create a new Shard Map Manager.</span></span> <span data-ttu-id="447ad-197">Příkladem je k dispozici [zde](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span><span class="sxs-lookup"><span data-stu-id="447ad-197">An example is available [here](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span></span>

## <a name="get-a-rangeshardmap-or-listshardmap"></a><span data-ttu-id="447ad-198">Získání RangeShardMap nebo ListShardMap</span><span class="sxs-lookup"><span data-stu-id="447ad-198">Get a RangeShardMap or ListShardMap</span></span>
<span data-ttu-id="447ad-199">Po vytvoření horizontálního oddílu mapa správce, můžete získat [RangeShardMap](https://msdn.microsoft.com/library/azure/dn807318.aspx) nebo [ListShardMap](https://msdn.microsoft.com/library/azure/dn807370.aspx) pomocí [TryGetRangeShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), [TryGetListShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx), nebo [GetShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) metoda.</span><span class="sxs-lookup"><span data-stu-id="447ad-199">After creating a shard map manager, you can get the [RangeShardMap](https://msdn.microsoft.com/library/azure/dn807318.aspx) or [ListShardMap](https://msdn.microsoft.com/library/azure/dn807370.aspx) using the [TryGetRangeShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), the [TryGetListShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx), or the [GetShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) method.</span></span>

    /// <summary>
    /// Creates a new Range Shard Map with the specified name, or gets the Range Shard Map if it already exists.
    /// </summary>
    public static RangeShardMap<T> CreateOrGetRangeShardMap<T>(ShardMapManager shardMapManager, string shardMapName)
    {
        // Try to get a reference to the Shard Map.
        RangeShardMap<T> shardMap;
        bool shardMapExists = shardMapManager.TryGetRangeShardMap(shardMapName, out shardMap);

        if (shardMapExists)
        {
            ConsoleUtils.WriteInfo("Shard Map {0} already exists", shardMap.Name);
        }
        else
        {
            // The Shard Map does not exist, so create it
            shardMap = shardMapManager.CreateRangeShardMap<T>(shardMapName);
            ConsoleUtils.WriteInfo("Created Shard Map {0}", shardMap.Name);
        }

        return shardMap;
    } 

### <a name="shard-map-administration-credentials"></a><span data-ttu-id="447ad-200">Pověření správce horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="447ad-200">Shard map administration credentials</span></span>
<span data-ttu-id="447ad-201">Aplikace, které Správa a zpracování mapy horizontálního oddílu se liší od těch, které pomocí map horizontálního oddílu k připojení trasy.</span><span class="sxs-lookup"><span data-stu-id="447ad-201">Applications that administer and manipulate shard maps are different from those that use the shard maps to route connections.</span></span> 

<span data-ttu-id="447ad-202">Ke správě horizontálního oddílu mapy (Přidat nebo změnit horizontálních oddílů, map horizontálního oddílu, mapování horizontálních, atd.) je třeba vytvořit instanci **ShardMapManager** pomocí **přihlašovací údaje, které mají pro čtení a zápis oprávnění v databázi GSM a na každou databázi, která slouží jako horizontálního oddílu**.</span><span class="sxs-lookup"><span data-stu-id="447ad-202">To administer shard maps (add or change shards, shard maps, shard mappings, etc.) you must instantiate the **ShardMapManager** using **credentials that have read/write privileges on both the GSM database and on each database that serves as a shard**.</span></span> <span data-ttu-id="447ad-203">Přihlašovací údaje musí povolovat zápisu tabulky v GSM i LSM informace ID horizontálního oddílu mapy zadat nebo změnit, stejně jako u vytváření tabulek LSM na nové horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="447ad-203">The credentials must allow for writes against the tables in both the GSM and LSM as shard map information is entered or changed, as well as for creating LSM tables on new shards.</span></span>  

<span data-ttu-id="447ad-204">V tématu [používá přihlašovací údaje pro přístup k klientské knihovny pro elastické databáze](sql-database-elastic-scale-manage-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="447ad-204">See [Credentials used to access the Elastic Database client library](sql-database-elastic-scale-manage-credentials.md).</span></span>

### <a name="only-metadata-affected"></a><span data-ttu-id="447ad-205">Vliv pouze metadata.</span><span class="sxs-lookup"><span data-stu-id="447ad-205">Only metadata affected</span></span>
<span data-ttu-id="447ad-206">Metody používané pro vyplnění nebo změna **ShardMapManager** data nijak nemění data uživatele uložená v horizontálních oddílů sami.</span><span class="sxs-lookup"><span data-stu-id="447ad-206">Methods used for populating or changing the **ShardMapManager** data do not alter the user data stored in the shards themselves.</span></span> <span data-ttu-id="447ad-207">Například metody, jako **CreateShard**, **DeleteShard**, **UpdateMapping**atd vliv pouze metadata mapy horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="447ad-207">For example, methods such as **CreateShard**, **DeleteShard**, **UpdateMapping**, etc. affect the shard map metadata only.</span></span> <span data-ttu-id="447ad-208">Není odebrat, přidat nebo změnit uživatele data obsažená v horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="447ad-208">They do not remove, add, or alter user data contained in the shards.</span></span> <span data-ttu-id="447ad-209">Místo toho tyto metody jsou určeny k použití ve spojení s samostatné operace, které provedete na vytvoření nebo odebrat skutečná databáze, nebo že přesunout řádky z jednoho horizontálního oddílu do jiného znovu vyvážit horizontálně dělené prostředí.</span><span class="sxs-lookup"><span data-stu-id="447ad-209">Instead, these methods are designed to be used in conjunction with separate operations you perform to create or remove actual databases, or that move rows from one shard to another to rebalance a sharded environment.</span></span>  <span data-ttu-id="447ad-210">( **Rozdělení sloučení** nástroje dodávaná s nástroje elastické databáze využívá tato rozhraní API společně s Orchestrace vlastní přesun dat. mezi horizontálních oddílů.) V tématu [škálování pomocí nástroje elastické databáze rozdělení sloučení](sql-database-elastic-scale-overview-split-and-merge.md).</span><span class="sxs-lookup"><span data-stu-id="447ad-210">(The **split-merge** tool included with elastic database tools makes use of these APIs along with orchestrating actual data movement between shards.) See [Scaling using the Elastic Database split-merge tool](sql-database-elastic-scale-overview-split-and-merge.md).</span></span>

## <a name="populating-a-shard-map-example"></a><span data-ttu-id="447ad-211">Sestavování příklad horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="447ad-211">Populating a shard map example</span></span>
<span data-ttu-id="447ad-212">V příkladu sekvenci operací k naplnění mapu konkrétní horizontálního oddílu jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="447ad-212">An example sequence of operations to populate a specific shard map is shown below.</span></span> <span data-ttu-id="447ad-213">Kód provede tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="447ad-213">The code performs these steps:</span></span> 

1. <span data-ttu-id="447ad-214">Nové mapování horizontálních je vytvořen v rámci správce mapy horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="447ad-214">A new shard map is created within a shard map manager.</span></span> 
2. <span data-ttu-id="447ad-215">Metadata pro dva různé horizontálních oddílů je přidán do mapy horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="447ad-215">The metadata for two different shards is added to the shard map.</span></span> 
3. <span data-ttu-id="447ad-216">Budou přidány různé klíče rozsah mapování a se zobrazí celkové obsah mapy horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="447ad-216">A variety of key range mappings are added, and the overall contents of the shard map are displayed.</span></span> 

<span data-ttu-id="447ad-217">Kód je napsán tak, aby metoda může být znovu, pokud dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="447ad-217">The code is written so that the method can be rerun if an error occurs.</span></span> <span data-ttu-id="447ad-218">Každý požadavek testuje, jestli horizontálního oddílu nebo mapování už existuje, před pokusem o jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="447ad-218">Each request tests whether a shard or mapping already exists, before attempting to create it.</span></span> <span data-ttu-id="447ad-219">Kód předpokládá, že databáze s názvem **sample_shard_0**, **sample_shard_1** a **sample_shard_2** již byly vytvořeny na serveru odkazuje řetězec **shardServer**.</span><span class="sxs-lookup"><span data-stu-id="447ad-219">The code assumes that databases named **sample_shard_0**, **sample_shard_1** and **sample_shard_2** have already been created in the server referenced by string **shardServer**.</span></span> 

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

            // List the shards and mappings 
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

<span data-ttu-id="447ad-220">Jako alternativu můžete použít skripty prostředí PowerShell k dosažení stejného výsledku.</span><span class="sxs-lookup"><span data-stu-id="447ad-220">As an alternative you can use PowerShell scripts to achieve the same result.</span></span> <span data-ttu-id="447ad-221">Některé příklady prostředí PowerShell ukázkový jsou k dispozici [zde](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span><span class="sxs-lookup"><span data-stu-id="447ad-221">Some of the sample PowerShell examples are available [here](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span></span>     

<span data-ttu-id="447ad-222">Jakmile jsou vyplněna horizontálního oddílu mapy, data přístup k aplikacím můžou vytvořit nebo přizpůsobit pro práci s map.</span><span class="sxs-lookup"><span data-stu-id="447ad-222">Once shard maps have been populated, data access applications can be created or adapted to work with the maps.</span></span> <span data-ttu-id="447ad-223">Naplnění nebo manipulace s map nemusí dojít znovu, dokud **mapy rozložení** musí změnit.</span><span class="sxs-lookup"><span data-stu-id="447ad-223">Populating or manipulating the maps need not occur again until **map layout** needs to change.</span></span>  

## <a name="data-dependent-routing"></a><span data-ttu-id="447ad-224">Směrování závislé na datech</span><span class="sxs-lookup"><span data-stu-id="447ad-224">Data dependent routing</span></span>
<span data-ttu-id="447ad-225">Mapa správce horizontálního oddílu nejvíce použije v aplikacích, které vyžadují připojení databáze k provádění operací data specifická pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="447ad-225">The shard map manager will be most used in applications that require database connections to perform the app-specific data operations.</span></span> <span data-ttu-id="447ad-226">Tato připojení musí být přidruženy k databázi správné.</span><span class="sxs-lookup"><span data-stu-id="447ad-226">Those connections must be associated with the correct database.</span></span> <span data-ttu-id="447ad-227">To se označuje jako **závislé směrování dat**.</span><span class="sxs-lookup"><span data-stu-id="447ad-227">This is known as **Data Dependent Routing**.</span></span> <span data-ttu-id="447ad-228">Pro tyto aplikace vytvořte instanci objektu manager mapy horizontálního oddílu z objektu pro vytváření pomocí přihlašovacích údajů, které mají oprávnění jen pro čtení na databázi GSM.</span><span class="sxs-lookup"><span data-stu-id="447ad-228">For these applications, instantiate a shard map manager object from the factory using credentials that have read-only access on the GSM database.</span></span> <span data-ttu-id="447ad-229">Jednotlivých požadavků pro pozdější připojení zadat přihlašovací údaje potřebné pro připojení k databázi odpovídající horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="447ad-229">Individual requests for later connections supply credentials necessary for connecting to the appropriate shard database.</span></span>

<span data-ttu-id="447ad-230">Všimněte si, že tyto aplikace (pomocí **ShardMapManager** otevřít s přihlašovacími údaji jen pro čtení) nelze provádět změny pro mapování nebo mapy.</span><span class="sxs-lookup"><span data-stu-id="447ad-230">Note that these applications (using **ShardMapManager** opened with read-only credentials) cannot make changes to the maps or mappings.</span></span> <span data-ttu-id="447ad-231">Pro tyto potřeby vytvořte správu konkrétní aplikace nebo skripty prostředí PowerShell, které zadat přihlašovací údaje pro vyšší úrovní oprávnění, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="447ad-231">For those needs, create administrative-specific applications or PowerShell scripts that supply higher-privileged credentials as discussed earlier.</span></span> <span data-ttu-id="447ad-232">V tématu [používá přihlašovací údaje pro přístup k klientské knihovny pro elastické databáze](sql-database-elastic-scale-manage-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="447ad-232">See [Credentials used to access the Elastic Database client library](sql-database-elastic-scale-manage-credentials.md).</span></span>

<span data-ttu-id="447ad-233">Další podrobnosti najdete v tématu [závislé směrování dat](sql-database-elastic-scale-data-dependent-routing.md).</span><span class="sxs-lookup"><span data-stu-id="447ad-233">For more details, see [Data dependent routing](sql-database-elastic-scale-data-dependent-routing.md).</span></span> 

## <a name="modifying-a-shard-map"></a><span data-ttu-id="447ad-234">Úprava horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="447ad-234">Modifying a shard map</span></span>
<span data-ttu-id="447ad-235">Mapování horizontálních lze změnit různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="447ad-235">A shard map can be changed in different ways.</span></span> <span data-ttu-id="447ad-236">Všechny z následujících metod upravit metadat, která popisují horizontálních oddílů a jejich mapování, ale nemění fyzicky dat v rámci horizontálních oddílů, ani se jejich vytvoření nebo odstranění skutečné databáze.</span><span class="sxs-lookup"><span data-stu-id="447ad-236">All of the following methods modify the metadata describing the shards and their mappings, but they do not physically modify data within the shards, nor do they create or delete the actual databases.</span></span>  <span data-ttu-id="447ad-237">Některé operace, na mapě horizontálního oddílu popsané dál může musí být zajistí koordinaci se akce správy, který fyzicky přesunout data nebo který přidávat a odebírat databáze slouží jako horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="447ad-237">Some of the operations on the shard map described below may need to be coordinated with administrative actions that physically move data or that add and remove databases serving as shards.</span></span>

<span data-ttu-id="447ad-238">Tyto metody spolupracovat jako stavební bloky, které jsou k dispozici pro úpravy celkové distribuci dat ve vašem prostředí horizontálně dělené databáze.</span><span class="sxs-lookup"><span data-stu-id="447ad-238">These methods work together as the building blocks available for modifying the overall distribution of data in your sharded database environment.</span></span>  

* <span data-ttu-id="447ad-239">Přidat nebo odebrat horizontálních oddílů: použijte  **[CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx)**  a  **[DeleteShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.deleteshard.aspx)**  z [Shardmap třída](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.aspx).</span><span class="sxs-lookup"><span data-stu-id="447ad-239">To add or remove shards: use **[CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx)** and **[DeleteShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.deleteshard.aspx)** of the [Shardmap class](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.aspx).</span></span> 
  
    <span data-ttu-id="447ad-240">Server a databázi představující horizontálního oddílu cíl již musí existovat pro tyto operace provést.</span><span class="sxs-lookup"><span data-stu-id="447ad-240">The server and database representing the target shard must already exist for these operations to execute.</span></span> <span data-ttu-id="447ad-241">Tyto metody nemají žádný vliv na databáze, sami, jenom na metadata v mapě horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="447ad-241">These methods do not have any impact on the databases themselves, only on metadata in the shard map.</span></span>
* <span data-ttu-id="447ad-242">Vytvořit nebo odstranit body nebo rozsahy, které jsou namapované horizontálních oddílů: použijte  **[CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn841993.aspx)**,  **[DeleteMapping](https://msdn.microsoft.com/library/azure/dn824200.aspx)**  z [RangeShardMapping třída](https://msdn.microsoft.com/library/azure/dn807318.aspx), a  **[CreatePointMapping](https://msdn.microsoft.com/library/azure/dn807218.aspx)**  z [ListShardMap](https://msdn.microsoft.com/library/azure/dn842123.aspx)</span><span class="sxs-lookup"><span data-stu-id="447ad-242">To create or remove points or ranges that are mapped to the shards: use **[CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn841993.aspx)**, **[DeleteMapping](https://msdn.microsoft.com/library/azure/dn824200.aspx)** of the [RangeShardMapping class](https://msdn.microsoft.com/library/azure/dn807318.aspx), and **[CreatePointMapping](https://msdn.microsoft.com/library/azure/dn807218.aspx)** of the [ListShardMap](https://msdn.microsoft.com/library/azure/dn842123.aspx)</span></span>
  
    <span data-ttu-id="447ad-243">Mnoho různých bodů nebo rozsahy lze mapovat na stejné ID horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="447ad-243">Many different points or ranges can be mapped to the same shard.</span></span> <span data-ttu-id="447ad-244">Tyto metody ovlivňují jenom metadata – neovlivňují všechna data, která může být již přítomny v horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="447ad-244">These methods only affect metadata - they do not affect any data that may already be present in shards.</span></span> <span data-ttu-id="447ad-245">Pokud data musí být odebrána z databáze, aby byl konzistentní s **DeleteMapping** operace, budete muset provést tyto operace samostatně, ale ve spojení s těmito metodami.</span><span class="sxs-lookup"><span data-stu-id="447ad-245">If data needs to be removed from the database in order to be consistent with **DeleteMapping** operations, you will need to perform those operations separately but in conjunction with using these methods.</span></span>  
* <span data-ttu-id="447ad-246">Pro existující rozsahy rozdělit na dvě nebo sloučení sousedící oblasti do jedné: použijte  **[SplitMapping](https://msdn.microsoft.com/library/azure/dn824205.aspx)**  a  **[MergeMappings](https://msdn.microsoft.com/library/azure/dn824201.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="447ad-246">To split existing ranges into two, or merge adjacent ranges into one: use **[SplitMapping](https://msdn.microsoft.com/library/azure/dn824205.aspx)** and **[MergeMappings](https://msdn.microsoft.com/library/azure/dn824201.aspx)**.</span></span>  
  
    <span data-ttu-id="447ad-247">Všimněte si, že rozdělení a operace sloučení **neměňte horizontálního oddílu, na kterou jsou hodnoty klíče namapovaná**.</span><span class="sxs-lookup"><span data-stu-id="447ad-247">Note that split and merge operations **do not change the shard to which key values are mapped**.</span></span> <span data-ttu-id="447ad-248">Rozdělení stávající rozsah dělí na dvě části, ale ponechá i jako namapované na stejné ID horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="447ad-248">A split breaks an existing range into two parts, but leaves both as mapped to the same shard.</span></span> <span data-ttu-id="447ad-249">Sloučení funguje na dva přiléhající rozsahy, které jsou už namapované stejné ID horizontálního oddílu, je slučování do jednoho rozsahu.</span><span class="sxs-lookup"><span data-stu-id="447ad-249">A merge operates on two adjacent ranges that are already mapped to the same shard, coalescing them into a single range.</span></span>  <span data-ttu-id="447ad-250">Přesun body nebo rozsahy, sami mezi horizontálních oddílů musí být koordinované pomocí **UpdateMapping** ve spojení s vlastní přesun dat..</span><span class="sxs-lookup"><span data-stu-id="447ad-250">The movement of points or ranges themselves between shards needs to be coordinated by using **UpdateMapping** in conjunction with actual data movement.</span></span>  <span data-ttu-id="447ad-251">Můžete použít **rozdělení či sloučení** službu, která je součástí nástrojů pro elastické databáze pro koordinaci změny mapování horizontálních s přesun dat potřeby pohyb.</span><span class="sxs-lookup"><span data-stu-id="447ad-251">You can use the **Split/Merge** service that is part of elastic database tools to coordinate shard map changes with data movement, when movement is needed.</span></span> 
* <span data-ttu-id="447ad-252">Znovu mapy (nebo přesunout) jednotlivé body nebo rozsahy pro různé horizontálních oddílů: použijte  **[UpdateMapping](https://msdn.microsoft.com/library/azure/dn824207.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="447ad-252">To re-map (or move) individual points or ranges to different shards: use **[UpdateMapping](https://msdn.microsoft.com/library/azure/dn824207.aspx)**.</span></span>  
  
    <span data-ttu-id="447ad-253">Vzhledem k tomu potřeba může data přesouvat z jednoho horizontálního oddílu na jiný s **UpdateMapping** operace, budete muset provést tento přesun samostatně, ale ve spojení s těmito metodami.</span><span class="sxs-lookup"><span data-stu-id="447ad-253">Since data may need to be moved from one shard to another in order to be consistent with **UpdateMapping** operations, you will need to perform that movement separately but in conjunction with using these methods.</span></span>
* <span data-ttu-id="447ad-254">Provést mapování online a offline: použijte  **[MarkMappingOffline](https://msdn.microsoft.com/library/azure/dn824202.aspx)**  a  **[MarkMappingOnline](https://msdn.microsoft.com/library/azure/dn807225.aspx)**  k řízení stavu online mapování.</span><span class="sxs-lookup"><span data-stu-id="447ad-254">To take mappings online and offline: use **[MarkMappingOffline](https://msdn.microsoft.com/library/azure/dn824202.aspx)** and **[MarkMappingOnline](https://msdn.microsoft.com/library/azure/dn807225.aspx)** to control the online state of a mapping.</span></span> 
  
    <span data-ttu-id="447ad-255">Mapování horizontálních určité operace jsou povoleny pouze při mapování je ve stavu "offline", včetně **UpdateMapping** a **DeleteMapping**.</span><span class="sxs-lookup"><span data-stu-id="447ad-255">Certain operations on shard mappings are only allowed when a mapping is in an “offline” state, including **UpdateMapping** and **DeleteMapping**.</span></span> <span data-ttu-id="447ad-256">Při mapování je offline, žádost dat závisí na základě klíče, součástí mapování vrátí chybu.</span><span class="sxs-lookup"><span data-stu-id="447ad-256">When a mapping is offline, a data-dependent request based on a key included in that mapping will return an error.</span></span> <span data-ttu-id="447ad-257">Kromě toho když rozsah je nejprve do režimu offline, všechna připojení k ovlivněných horizontálního oddílu jsou automaticky ukončená prevence nekonzistentní nebo neúplné výsledky dotazů namířená proti rozsahy se změnil.</span><span class="sxs-lookup"><span data-stu-id="447ad-257">In addition, when a range is first taken offline, all connections to the affected shard are automatically killed in order to prevent inconsistent or incomplete results for queries directed against ranges being changed.</span></span> 

<span data-ttu-id="447ad-258">Mapování jsou neměnné objekty v rozhraní .net.</span><span class="sxs-lookup"><span data-stu-id="447ad-258">Mappings are immutable objects in .Net.</span></span>  <span data-ttu-id="447ad-259">Všechny výše uvedené metody, které změní mapování zneplatnit také všechny odkazy na ně ve vašem kódu.</span><span class="sxs-lookup"><span data-stu-id="447ad-259">All of the methods above that change mappings also invalidate any references to them in your code.</span></span> <span data-ttu-id="447ad-260">Usnadnění provádění pořadí operace, které změní stav mapování všechny metody, které změní mapování vrátí nový odkaz mapování, tak možné zřetězit operace.</span><span class="sxs-lookup"><span data-stu-id="447ad-260">To make it easier to perform sequences of operations that change a mapping’s state, all of the methods that change a mapping return a new mapping reference, so operations can be chained.</span></span> <span data-ttu-id="447ad-261">Například pokud chcete odstranit existující mapování v shardmap sm, který obsahuje klíč 25, můžete spustit následující:</span><span class="sxs-lookup"><span data-stu-id="447ad-261">For example, to delete an existing mapping in shardmap sm that contains the key 25, you can execute the following:</span></span> 

        sm.DeleteMapping(sm.MarkMappingOffline(sm.GetMappingForKey(25)));

## <a name="adding-a-shard"></a><span data-ttu-id="447ad-262">Přidání horizontálního oddílu</span><span class="sxs-lookup"><span data-stu-id="447ad-262">Adding a shard</span></span>
<span data-ttu-id="447ad-263">Aplikace často potřebují jednoduše přidat nové horizontálních oddílů pro zpracování dat, která je očekávána z nových klíčů nebo rozsahy klíčů pro mapu horizontálního oddílu, který již existuje.</span><span class="sxs-lookup"><span data-stu-id="447ad-263">Applications often need to simply add new shards to handle data that is expected from new keys or key ranges, for a shard map that already exists.</span></span> <span data-ttu-id="447ad-264">Například aplikace horizontálně dělené podle ID klienta muset zřídit nové horizontálního oddílu pro nového klienta, nebo horizontálně dělené měsíční dat může být nutné nové horizontálních zřídit před zahájením každý nový měsíc.</span><span class="sxs-lookup"><span data-stu-id="447ad-264">For example, an application sharded by Tenant ID may need to provision a new shard for a new tenant, or data sharded monthly may need a new shard provisioned before the start of each new month.</span></span> 

<span data-ttu-id="447ad-265">Pokud nový rozsah hodnot klíče již není součástí stávající mapování a žádné přesun dat je nezbytné, je velmi jednoduchý a přidat nové horizontálního oddílu přidružit nový klíč nebo rozsahu do této horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="447ad-265">If the new range of key values is not already part of an existing mapping and no data movement is necessary, it is very simple to add the new shard and associate the new key or range to that shard.</span></span> <span data-ttu-id="447ad-266">Podrobnosti o přidání nové horizontálních oddílů najdete v tématu [přidání nové horizontálního oddílu](sql-database-elastic-scale-add-a-shard.md).</span><span class="sxs-lookup"><span data-stu-id="447ad-266">For details on adding new shards, see [Adding a new shard](sql-database-elastic-scale-add-a-shard.md).</span></span>

<span data-ttu-id="447ad-267">Pro scénáře, které vyžadují přesun dat ale nástroj rozdělení sloučení je nezbytná k orchestraci přesun dat mezi horizontálních oddílů v kombinaci s aktualizací map nezbytné horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="447ad-267">For scenarios that require data movement, however, the split-merge tool is needed to orchestrate the data movement between shards in combination with the necessary shard map updates.</span></span> <span data-ttu-id="447ad-268">Podrobnosti o použití rozdělení sloučení yool najdete v tématu [Přehled rozdělení sloučení](sql-database-elastic-scale-overview-split-and-merge.md)</span><span class="sxs-lookup"><span data-stu-id="447ad-268">For details on using the split-merge yool, see [Overview of split-merge](sql-database-elastic-scale-overview-split-and-merge.md)</span></span> 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-shard-map-management/listmapping.png
[2]: ./media/sql-database-elastic-scale-shard-map-management/rangemapping.png
[3]: ./media/sql-database-elastic-scale-shard-map-management/multipleonsingledb.png
