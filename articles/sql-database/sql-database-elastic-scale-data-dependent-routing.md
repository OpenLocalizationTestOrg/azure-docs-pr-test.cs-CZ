---
title: "Data závislé směrování s Azure SQL Database | Microsoft Docs"
description: "Jak používat třídu ShardMapManager v aplikacích .NET pro data závislé na směrování, funkce horizontálně dělené databází ve službě Azure SQL Database"
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
editor: 
ms.assetid: cad09e15-5561-4448-aa18-b38f54cda004
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2017
ms.author: ddove
ms.openlocfilehash: 6b68bbb0133afd1493acdb58f79f3eeaf6a8d7cd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="data-dependent-routing"></a><span data-ttu-id="26b73-103">Směrování závislé na datech</span><span class="sxs-lookup"><span data-stu-id="26b73-103">Data dependent routing</span></span>
<span data-ttu-id="26b73-104">**Data závislé směrování** je možnost používat data v dotazu pro směrování požadavku k příslušné databázi.</span><span class="sxs-lookup"><span data-stu-id="26b73-104">**Data dependent routing** is the ability to use the data in a query to route the request to an appropriate database.</span></span> <span data-ttu-id="26b73-105">Toto je základní vzor při práci s horizontálně dělené databáze.</span><span class="sxs-lookup"><span data-stu-id="26b73-105">This is a fundamental pattern when working with sharded databases.</span></span> <span data-ttu-id="26b73-106">Kontext požadavku může také použít pro směrování požadavku, zvlášť pokud klíč horizontálního dělení není část dotazu.</span><span class="sxs-lookup"><span data-stu-id="26b73-106">The request context may also be used to route the request, especially if the sharding key is not part of the query.</span></span> <span data-ttu-id="26b73-107">Každé specifického dotazu nebo transakce v aplikaci pomocí závislé směrování dat je omezen na přístup k jedné databáze na základě požadavku.</span><span class="sxs-lookup"><span data-stu-id="26b73-107">Each specific query or transaction in an application using data dependent routing is restricted to accessing a single database per request.</span></span> <span data-ttu-id="26b73-108">Pro nástroje Azure SQL Database elastické tento směrování se provádí pomocí  **[ShardMapManager třída](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)**  v aplikacích ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="26b73-108">For the Azure SQL Database Elastic tools, this routing is accomplished with the **[ShardMapManager class](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)** in ADO.NET applications.</span></span>

<span data-ttu-id="26b73-109">Aplikace není nutné sledovat různé připojovací řetězce nebo DB umístění přidružené k jiné řezy dat v horizontálně dělené prostředí.</span><span class="sxs-lookup"><span data-stu-id="26b73-109">The application does not need to track various connection strings or DB locations associated with different slices of data in the sharded environment.</span></span> <span data-ttu-id="26b73-110">Místo toho [správce mapy horizontálního oddílu](sql-database-elastic-scale-shard-map-management.md) otevře připojení k databázím správné v případě potřeby na základě dat v mapě horizontálního oddílu a hodnota klíče horizontálního dělení, který je cílem dané aplikace požadavek.</span><span class="sxs-lookup"><span data-stu-id="26b73-110">Instead, the [Shard Map Manager](sql-database-elastic-scale-shard-map-management.md) opens connections to the correct databases when needed, based on the data in the shard map and the value of the sharding key that is the target of the application’s request.</span></span> <span data-ttu-id="26b73-111">Klíč je obvykle *customer_id*, *tenant_id*, *date_key*, nebo některé konkrétní identifikátor, který je základní parametr žádosti databáze).</span><span class="sxs-lookup"><span data-stu-id="26b73-111">The key is typically the *customer_id*, *tenant_id*, *date_key*, or some other specific identifier that is a fundamental parameter of the database request).</span></span> 

<span data-ttu-id="26b73-112">Další informace najdete v tématu [škálování SQL serveru se Škálováním s závislé směrování dat](https://technet.microsoft.com/library/cc966448.aspx).</span><span class="sxs-lookup"><span data-stu-id="26b73-112">For more information, see [Scaling Out SQL Server with Data Dependent Routing](https://technet.microsoft.com/library/cc966448.aspx).</span></span>

## <a name="download-the-client-library"></a><span data-ttu-id="26b73-113">Stáhnout klientské knihovny</span><span class="sxs-lookup"><span data-stu-id="26b73-113">Download the client library</span></span>
<span data-ttu-id="26b73-114">Třída získáte nainstalovat [klientské knihovny pro elastické databáze](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).</span><span class="sxs-lookup"><span data-stu-id="26b73-114">To get the class, install the [Elastic Database Client Library](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).</span></span> 

## <a name="using-a-shardmapmanager-in-a-data-dependent-routing-application"></a><span data-ttu-id="26b73-115">Použití ShardMapManager v závislou aplikaci dat směrování</span><span class="sxs-lookup"><span data-stu-id="26b73-115">Using a ShardMapManager in a data dependent routing application</span></span>
<span data-ttu-id="26b73-116">Aplikace by měl vytvořit instanci **ShardMapManager** při inicializaci pomocí volání objektu pro vytváření  **[GetSQLShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="26b73-116">Applications should instantiate the **ShardMapManager** during initialization, using the factory call **[GetSQLShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**.</span></span> <span data-ttu-id="26b73-117">V tomto příkladu jak **ShardMapManager** a konkrétní **ShardMap** ji obsahující jsou inicializovány.</span><span class="sxs-lookup"><span data-stu-id="26b73-117">In this example, both a **ShardMapManager** and a specific **ShardMap** that it contains are initialized.</span></span> <span data-ttu-id="26b73-118">Tento příklad ukazuje GetSqlShardMapManager a [GetRangeShardMap](https://msdn.microsoft.com/library/azure/dn824173.aspx) metody.</span><span class="sxs-lookup"><span data-stu-id="26b73-118">This example shows the GetSqlShardMapManager and [GetRangeShardMap](https://msdn.microsoft.com/library/azure/dn824173.aspx) methods.</span></span>

    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString, 
                      ShardMapManagerLoadPolicy.Lazy);
    RangeShardMap<int> customerShardMap = smm.GetRangeShardMap<int>("customerMap"); 

### <a name="use-lowest-privilege-credentials-possible-for-getting-the-shard-map"></a><span data-ttu-id="26b73-119">Použít pověření nejnižší oprávnění možné pro získání ID horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="26b73-119">Use lowest privilege credentials possible for getting the shard map</span></span>
<span data-ttu-id="26b73-120">Pokud aplikace není manipulace s samotná mapa horizontálního oddílu, přihlašovací údaje použité metoda objektu factory musí mít oprávnění stačí jen pro čtení **globální horizontálního oddílu mapy** databáze.</span><span class="sxs-lookup"><span data-stu-id="26b73-120">If an application is not manipulating the shard map itself, the credentials used in the factory method should have just read-only permissions on the **Global Shard Map** database.</span></span> <span data-ttu-id="26b73-121">Tyto přihlašovací údaje se obvykle liší od přihlašovací údaje používané k otevření připojení do správce mapy horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="26b73-121">These credentials are typically different from credentials used to open connections to the shard map manager.</span></span> <span data-ttu-id="26b73-122">Viz také [používá přihlašovací údaje pro přístup k klientské knihovny pro elastické databáze](sql-database-elastic-scale-manage-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="26b73-122">See also [Credentials used to access the Elastic Database client library](sql-database-elastic-scale-manage-credentials.md).</span></span> 

## <a name="call-the-openconnectionforkey-method"></a><span data-ttu-id="26b73-123">Volání metody OpenConnectionForKey</span><span class="sxs-lookup"><span data-stu-id="26b73-123">Call the OpenConnectionForKey method</span></span>
<span data-ttu-id="26b73-124"> **[ShardMap.OpenConnectionForKey metoda](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx)**  vrátí připravená pro vydání příkazy k příslušné databázi na základě hodnoty z připojení k ADO.Net **klíč** parametr.</span><span class="sxs-lookup"><span data-stu-id="26b73-124">The **[ShardMap.OpenConnectionForKey method](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx)** returns an ADO.Net connection ready for issuing commands to the appropriate database based on the value of the **key** parameter.</span></span> <span data-ttu-id="26b73-125">Informace ID horizontálního oddílu je uložené v mezipaměti v aplikace **ShardMapManager**, takže tyto požadavky nejsou obvykle zahrnují vyhledávání v databázi proti **globální mapy horizontálního oddílu** databáze.</span><span class="sxs-lookup"><span data-stu-id="26b73-125">Shard information is cached in the application by the **ShardMapManager**, so these requests do not typically involve a database lookup against the **Global Shard Map** database.</span></span> 

    // Syntax: 
    public SqlConnection OpenConnectionForKey<TKey>(
        TKey key,
        string connectionString,
        ConnectionOptions options
    )


* <span data-ttu-id="26b73-126">**Klíč** parametr se používá jako klíč vyhledávání do mapy horizontálního oddílu určit příslušné databáze pro požadavek.</span><span class="sxs-lookup"><span data-stu-id="26b73-126">The **key** parameter is used as a lookup key into the shard map to determine the appropriate database for the request.</span></span> 
* <span data-ttu-id="26b73-127">**ConnectionString** slouží k předávání pouze uživatelská pověření pro požadované připojení.</span><span class="sxs-lookup"><span data-stu-id="26b73-127">The **connectionString** is used to pass only the user credentials for the desired connection.</span></span> <span data-ttu-id="26b73-128">Žádný název databáze nebo název serveru jsou zahrnuté v tomto *connectionString* vzhledem k tomu, že metoda určí databáze a serveru pomocí **ShardMap**.</span><span class="sxs-lookup"><span data-stu-id="26b73-128">No database name or server name are included in this *connectionString* since the method will determine the database and server using the **ShardMap**.</span></span> 
* <span data-ttu-id="26b73-129"> **[ConnectionOptions](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.connectionoptions.aspx)**  musí být nastavena na **ConnectionOptions.Validate** Pokud prostředí, kde horizontálního oddílu mapuje může změnit a řádky může přesunout do jiné databáze na základě těchto rozdělení nebo sloučení operací.</span><span class="sxs-lookup"><span data-stu-id="26b73-129">The **[connectionOptions](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.connectionoptions.aspx)** should be set to **ConnectionOptions.Validate** if an environment where shard maps may change and rows may move to other databases as a result of split or merge operations.</span></span> <span data-ttu-id="26b73-130">To zahrnuje stručný dotazu mapu místní horizontálního oddílu na cílové databáze (ne do globální horizontálních map) před doručením připojení k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="26b73-130">This involves a brief query to the local shard map on the target database (not to the global shard map) before the connection is delivered to the application.</span></span> 

<span data-ttu-id="26b73-131">Selže-li ověření vůči mapování horizontálních místní (což znamená, že mezipaměť je nesprávný), Manager mapy horizontálního oddílu se bude dotazovat mapy globální horizontálních získat nové správnou hodnotu pro vyhledávání, aktualizace mezipaměti a získat a vrátit na příslušnou databázi připojení.</span><span class="sxs-lookup"><span data-stu-id="26b73-131">If the validation against the local shard map fails (indicating that the cache is incorrect), the Shard Map Manager will query the global shard map to obtain the new correct value for the lookup, update the cache, and obtain and return the appropriate database connection.</span></span> 

<span data-ttu-id="26b73-132">Použití **ConnectionOptions.None** pouze když změny mapování horizontálních nejsou očekáván aplikace je online.</span><span class="sxs-lookup"><span data-stu-id="26b73-132">Use **ConnectionOptions.None** only when shard mapping changes are not expected while an application is online.</span></span> <span data-ttu-id="26b73-133">V takovém případě uložené v mezipaměti hodnoty lze předpokládat, že vždycky být správná a mohou být velmi odezvy ověření volání na cílovou databázi bezpečně přeskočeny.</span><span class="sxs-lookup"><span data-stu-id="26b73-133">In that case, the cached values can be assumed to always be correct, and the extra round-trip validation call to the target database can be safely skipped.</span></span> <span data-ttu-id="26b73-134">Který snižuje zatížení databáze.</span><span class="sxs-lookup"><span data-stu-id="26b73-134">That reduces database traffic.</span></span> <span data-ttu-id="26b73-135">**ConnectionOptions** také lze nastavit prostřednictvím hodnotu v konfiguračním souboru k označení, zda se očekává horizontálního dělení změny nebo není během v časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="26b73-135">The **connectionOptions** may also be set via a value in a configuration file to indicate whether sharding changes are expected or not during a period of time.</span></span>  

<span data-ttu-id="26b73-136">Tento příklad používá hodnotu celé číslo klíče **CustomerID**pomocí **ShardMap** objekt s názvem **customerShardMap**.</span><span class="sxs-lookup"><span data-stu-id="26b73-136">This example uses the value of an integer key **CustomerID**, using a **ShardMap** object named **customerShardMap**.</span></span>  

    int customerId = 12345; 
    int newPersonId = 4321; 

    // Connect to the shard for that customer ID. No need to call a SqlConnection 
    // constructor followed by the Open method.
    using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId, 
        Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate)) 
    { 
        // Execute a simple command. 
        SqlCommand cmd = conn.CreateCommand(); 
        cmd.CommandText = @"UPDATE Sales.Customer 
                            SET PersonID = @newPersonID 
                            WHERE CustomerID = @customerID"; 

        cmd.Parameters.AddWithValue("@customerID", customerId); 
        cmd.Parameters.AddWithValue("@newPersonID", newPersonId); 
        cmd.ExecuteNonQuery(); 
    }  

<span data-ttu-id="26b73-137">**OpenConnectionForKey** metoda vrátí nové již otevřete připojení ke správné databázi.</span><span class="sxs-lookup"><span data-stu-id="26b73-137">The **OpenConnectionForKey** method returns a new already-open connection to the correct database.</span></span> <span data-ttu-id="26b73-138">Připojení v tímto způsobem nadále využívat všech výhod ADO.Net sdružování připojení.</span><span class="sxs-lookup"><span data-stu-id="26b73-138">Connections utilized in this way still take full advantage of ADO.Net connection pooling.</span></span> <span data-ttu-id="26b73-139">Tak dlouho, dokud transakce a požadavky lze splnit jednu horizontálního oddílu současně, to by měl být pouze změny potřebné v aplikaci už používá ADO.Net.</span><span class="sxs-lookup"><span data-stu-id="26b73-139">As long as transactions and requests can be satisfied by one shard at a time, this should be the only modification necessary in an application already using ADO.Net.</span></span> 

<span data-ttu-id="26b73-140"> **[OpenConnectionForKeyAsync metoda](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkeyasync.aspx)**  je také dostupná v případě, že vaše aplikace umožňuje použití asynchronní programování s ADO.Net.</span><span class="sxs-lookup"><span data-stu-id="26b73-140">The **[OpenConnectionForKeyAsync method](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkeyasync.aspx)** is also available if your application makes use asynchronous programming with ADO.Net.</span></span> <span data-ttu-id="26b73-141">Její chování je závislé směrování ekvivalentní ADO data. NET na  **[Connection.OpenAsync](https://msdn.microsoft.com/library/hh223688\(v=vs.110\).aspx)**  metoda.</span><span class="sxs-lookup"><span data-stu-id="26b73-141">Its behavior is the data dependent routing equivalent of ADO.Net's **[Connection.OpenAsync](https://msdn.microsoft.com/library/hh223688\(v=vs.110\).aspx)** method.</span></span>

## <a name="integrating-with-transient-fault-handling"></a><span data-ttu-id="26b73-142">Integrace s přechodná chyba zpracování</span><span class="sxs-lookup"><span data-stu-id="26b73-142">Integrating with transient fault handling</span></span>
<span data-ttu-id="26b73-143">Osvědčeným postupem při vývoji aplikací přístup dat v cloudu, je zajistit, že přechodných zachytila aplikaci a že operace, které jsou opakovat několikrát před vyvolání k chybě.</span><span class="sxs-lookup"><span data-stu-id="26b73-143">A best practice in developing data access applications in the cloud is to ensure that transient faults are caught by the app, and that the operations are retried several times before throwing an error.</span></span> <span data-ttu-id="26b73-144">Přechodná chyba zpracování pro cloudové aplikace je popsána v [přechodných chyb](https://msdn.microsoft.com/library/dn440719\(v=pandp.60\).aspx).</span><span class="sxs-lookup"><span data-stu-id="26b73-144">Transient fault handling for cloud applications is discussed at [Transient Fault Handling](https://msdn.microsoft.com/library/dn440719\(v=pandp.60\).aspx).</span></span> 

<span data-ttu-id="26b73-145">Přechodná chyba zpracování mohou existovat vedle sebe přirozeně pomocí vzoru Data závislé směrování.</span><span class="sxs-lookup"><span data-stu-id="26b73-145">Transient fault handling can coexist naturally with the Data Dependent Routing pattern.</span></span> <span data-ttu-id="26b73-146">Klíčovým požadavkem je opakovat, včetně celého datového přístupu požadavek **pomocí** blok, který získat připojení směrování závislé na data.</span><span class="sxs-lookup"><span data-stu-id="26b73-146">The key requirement is to retry the entire data access request including the **using** block that obtained the data-dependent routing connection.</span></span> <span data-ttu-id="26b73-147">V předchozím příkladu by mohla být přepsána takto (Poznámka zvýrazněná změnit).</span><span class="sxs-lookup"><span data-stu-id="26b73-147">The example above could be rewritten as follows (note highlighted change).</span></span> 

### <a name="example---data-dependent-routing-with-transient-fault-handling"></a><span data-ttu-id="26b73-148">Příklad: závislé směrování s přechodná chyba zpracování dat</span><span class="sxs-lookup"><span data-stu-id="26b73-148">Example - data dependent routing with transient fault handling</span></span>
<pre><code>int customerId = 12345; 
int newPersonId = 4321; 

<span style="background-color:  #FFFF00">Configuration.SqlRetryPolicy.ExecuteAction(() =&gt; </span> 
<span style="background-color:  #FFFF00">    { </span>
        // Connect to the shard for a customer ID. 
        using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId,  
        Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate)) 
        { 
            // Execute a simple command 
            SqlCommand cmd = conn.CreateCommand(); 

            cmd.CommandText = @&quot;UPDATE Sales.Customer 
                            SET PersonID = @newPersonID 
                            WHERE CustomerID = @customerID&quot;; 

            cmd.Parameters.AddWithValue(&quot;@customerID&quot;, customerId); 
            cmd.Parameters.AddWithValue(&quot;@newPersonID&quot;, newPersonId); 
            cmd.ExecuteNonQuery(); 

            Console.WriteLine(&quot;Update completed&quot;); 
        } 
<span style="background-color:  #FFFF00">    }); </span> 
</code></pre>


<span data-ttu-id="26b73-149">Balíčky, které jsou nezbytné k implementaci přechodná chyba zpracování se stáhnou automaticky při sestavování ukázkovou aplikaci elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="26b73-149">Packages necessary to implement transient fault handling are downloaded automatically when you build the elastic database sample application.</span></span> <span data-ttu-id="26b73-150">Balíčky jsou k dispozici samostatně na [Enterprise Library - přechodné chyby zpracování bloku aplikace](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/).</span><span class="sxs-lookup"><span data-stu-id="26b73-150">Packages are also available separately at [Enterprise Library - Transient Fault Handling Application Block](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/).</span></span> <span data-ttu-id="26b73-151">Použijte verzi 6.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="26b73-151">Use version 6.0 or later.</span></span> 

## <a name="transactional-consistency"></a><span data-ttu-id="26b73-152">Transakční konzistence</span><span class="sxs-lookup"><span data-stu-id="26b73-152">Transactional consistency</span></span>
<span data-ttu-id="26b73-153">Transakční vlastnosti jsou zaručena bezpečnost pro všechny operace místní horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="26b73-153">Transactional properties are guaranteed for all operations local to a shard.</span></span> <span data-ttu-id="26b73-154">Například spuštění transakce odeslat prostřednictvím závislé na data směrování v rámci oboru horizontálního oddílu cíl pro připojení.</span><span class="sxs-lookup"><span data-stu-id="26b73-154">For example, transactions submitted through data-dependent routing execute within the scope of the target shard for the connection.</span></span> <span data-ttu-id="26b73-155">V současné době neexistují žádné možnosti zadaná pro zapsání několik připojení do transakce a proto nejsou žádné záruky transakcí pro operace prováděné napříč horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="26b73-155">At this time, there are no capabilities provided for enlisting multiple connections into a transaction, and therefore there are no transactional guarantees for operations performed across shards.</span></span>

## <a name="next-steps"></a><span data-ttu-id="26b73-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="26b73-156">Next steps</span></span>
<span data-ttu-id="26b73-157">Odpojit horizontálního oddílu, nebo znovu připojit horizontálního oddílu, najdete v části [pomocí třídy RecoveryManager opravit problémy horizontálního oddílu mapy](sql-database-elastic-database-recovery-manager.md)</span><span class="sxs-lookup"><span data-stu-id="26b73-157">To detach a shard, or to reattach a shard, see [Using the RecoveryManager class to fix shard map problems](sql-database-elastic-database-recovery-manager.md)</span></span>

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

