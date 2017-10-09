---
title: "aaaData závislé směrování s Azure SQL Database | Microsoft Docs"
description: "Jak toouse hello ShardMapManager třídy v aplikacích .NET pro data závislé na směrování, funkce horizontálně dělené databází ve službě Azure SQL Database"
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
ms.openlocfilehash: 34014508ae01905686791fe096bb275cb84f53b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="data-dependent-routing"></a><span data-ttu-id="47a81-103">Směrování závislé na datech</span><span class="sxs-lookup"><span data-stu-id="47a81-103">Data dependent routing</span></span>
<span data-ttu-id="47a81-104">**Data závislé směrování** je hello možnost toouse hello data v dotazu tooroute hello požadavek tooan příslušné databázi.</span><span class="sxs-lookup"><span data-stu-id="47a81-104">**Data dependent routing** is hello ability toouse hello data in a query tooroute hello request tooan appropriate database.</span></span> <span data-ttu-id="47a81-105">Toto je základní vzor při práci s horizontálně dělené databáze.</span><span class="sxs-lookup"><span data-stu-id="47a81-105">This is a fundamental pattern when working with sharded databases.</span></span> <span data-ttu-id="47a81-106">kontext požadavku Hello může být také použít tooroute hello požadavku, zvlášť pokud hello horizontálního dělení klíč není součástí hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="47a81-106">hello request context may also be used tooroute hello request, especially if hello sharding key is not part of hello query.</span></span> <span data-ttu-id="47a81-107">Každé specifického dotazu nebo transakce v aplikaci pomocí závislé směrování dat je omezená tooaccessing jednu databázi na základě požadavku.</span><span class="sxs-lookup"><span data-stu-id="47a81-107">Each specific query or transaction in an application using data dependent routing is restricted tooaccessing a single database per request.</span></span> <span data-ttu-id="47a81-108">Azure SQL Database elastické nástroje hello, tento směrování se provádí s hello  **[ShardMapManager třída](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)**  v aplikacích ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="47a81-108">For hello Azure SQL Database Elastic tools, this routing is accomplished with hello **[ShardMapManager class](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)** in ADO.NET applications.</span></span>

<span data-ttu-id="47a81-109">aplikace Hello nemusí tootrack různé připojovací řetězce nebo DB umístění přidružené k jiné řezy dat v horizontálně dělené prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="47a81-109">hello application does not need tootrack various connection strings or DB locations associated with different slices of data in hello sharded environment.</span></span> <span data-ttu-id="47a81-110">Místo toho hello [správce mapy horizontálního oddílu](sql-database-elastic-scale-shard-map-management.md) otevře připojení toohello správné databází v případě potřeby na základě hello dat hello horizontálního oddílu mapy a hello hodnota hello horizontálního dělení klíče, který je cílem hello žádostí na aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="47a81-110">Instead, hello [Shard Map Manager](sql-database-elastic-scale-shard-map-management.md) opens connections toohello correct databases when needed, based on hello data in hello shard map and hello value of hello sharding key that is hello target of hello application’s request.</span></span> <span data-ttu-id="47a81-111">klíč Hello je obvykle hello *customer_id*, *tenant_id*, *date_key*, nebo některé konkrétní identifikátor, který je základní parametr požadavek databáze hello).</span><span class="sxs-lookup"><span data-stu-id="47a81-111">hello key is typically hello *customer_id*, *tenant_id*, *date_key*, or some other specific identifier that is a fundamental parameter of hello database request).</span></span> 

<span data-ttu-id="47a81-112">Další informace najdete v tématu [škálování SQL serveru se Škálováním s závislé směrování dat](https://technet.microsoft.com/library/cc966448.aspx).</span><span class="sxs-lookup"><span data-stu-id="47a81-112">For more information, see [Scaling Out SQL Server with Data Dependent Routing](https://technet.microsoft.com/library/cc966448.aspx).</span></span>

## <a name="download-hello-client-library"></a><span data-ttu-id="47a81-113">Stáhnout klientskou knihovnu hello</span><span class="sxs-lookup"><span data-stu-id="47a81-113">Download hello client library</span></span>
<span data-ttu-id="47a81-114">Třída hello tooget, instalace hello [klientské knihovny pro elastické databáze](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).</span><span class="sxs-lookup"><span data-stu-id="47a81-114">tooget hello class, install hello [Elastic Database Client Library](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).</span></span> 

## <a name="using-a-shardmapmanager-in-a-data-dependent-routing-application"></a><span data-ttu-id="47a81-115">Použití ShardMapManager v závislou aplikaci dat směrování</span><span class="sxs-lookup"><span data-stu-id="47a81-115">Using a ShardMapManager in a data dependent routing application</span></span>
<span data-ttu-id="47a81-116">Aplikace by měl vytvořit instanci hello **ShardMapManager** během inicializace pomocí volání objektu pro vytváření hello  **[GetSQLShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="47a81-116">Applications should instantiate hello **ShardMapManager** during initialization, using hello factory call **[GetSQLShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**.</span></span> <span data-ttu-id="47a81-117">V tomto příkladu jak **ShardMapManager** a konkrétní **ShardMap** ji obsahující jsou inicializovány.</span><span class="sxs-lookup"><span data-stu-id="47a81-117">In this example, both a **ShardMapManager** and a specific **ShardMap** that it contains are initialized.</span></span> <span data-ttu-id="47a81-118">Tento příklad ukazuje hello GetSqlShardMapManager a [GetRangeShardMap](https://msdn.microsoft.com/library/azure/dn824173.aspx) metody.</span><span class="sxs-lookup"><span data-stu-id="47a81-118">This example shows hello GetSqlShardMapManager and [GetRangeShardMap](https://msdn.microsoft.com/library/azure/dn824173.aspx) methods.</span></span>

    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString, 
                      ShardMapManagerLoadPolicy.Lazy);
    RangeShardMap<int> customerShardMap = smm.GetRangeShardMap<int>("customerMap"); 

### <a name="use-lowest-privilege-credentials-possible-for-getting-hello-shard-map"></a><span data-ttu-id="47a81-119">Použít pověření nejnižší oprávnění možné pro získání hello horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="47a81-119">Use lowest privilege credentials possible for getting hello shard map</span></span>
<span data-ttu-id="47a81-120">Pokud aplikace není manipulace s samotná mapa hello horizontálního oddílu, hello přihlašovací údaje použité metoda factory hello musí mít oprávnění stačí jen pro čtení na hello **globální horizontálního oddílu mapy** databáze.</span><span class="sxs-lookup"><span data-stu-id="47a81-120">If an application is not manipulating hello shard map itself, hello credentials used in hello factory method should have just read-only permissions on hello **Global Shard Map** database.</span></span> <span data-ttu-id="47a81-121">Tyto přihlašovací údaje se obvykle liší od přihlašovací údaje používané tooopen připojení toohello horizontálního oddílu mapa správce.</span><span class="sxs-lookup"><span data-stu-id="47a81-121">These credentials are typically different from credentials used tooopen connections toohello shard map manager.</span></span> <span data-ttu-id="47a81-122">Viz také [použita pověření tooaccess hello elastické databáze klientské knihovny](sql-database-elastic-scale-manage-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="47a81-122">See also [Credentials used tooaccess hello Elastic Database client library](sql-database-elastic-scale-manage-credentials.md).</span></span> 

## <a name="call-hello-openconnectionforkey-method"></a><span data-ttu-id="47a81-123">Volání metody OpenConnectionForKey hello</span><span class="sxs-lookup"><span data-stu-id="47a81-123">Call hello OpenConnectionForKey method</span></span>
<span data-ttu-id="47a81-124">Hello  **[ShardMap.OpenConnectionForKey metoda](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx)**  vrátí připojení k ADO.Net připravené k vydávání příkazů toohello příslušné databáze na základě hodnoty hello hello **klíč**parametr.</span><span class="sxs-lookup"><span data-stu-id="47a81-124">hello **[ShardMap.OpenConnectionForKey method](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx)** returns an ADO.Net connection ready for issuing commands toohello appropriate database based on hello value of hello **key** parameter.</span></span> <span data-ttu-id="47a81-125">Informace ID horizontálního oddílu jsou uloženy v mezipaměti v aplikaci hello hello **ShardMapManager**, takže tyto požadavky nejsou obvykle zahrnují vyhledávání v databázi proti hello **globální horizontálního oddílu mapy** databáze.</span><span class="sxs-lookup"><span data-stu-id="47a81-125">Shard information is cached in hello application by hello **ShardMapManager**, so these requests do not typically involve a database lookup against hello **Global Shard Map** database.</span></span> 

    // Syntax: 
    public SqlConnection OpenConnectionForKey<TKey>(
        TKey key,
        string connectionString,
        ConnectionOptions options
    )


* <span data-ttu-id="47a81-126">Hello **klíč** parametr se používá jako klíč vyhledávání do hello horizontálního oddílu mapy toodetermine hello příslušné databáze pro požadavek hello.</span><span class="sxs-lookup"><span data-stu-id="47a81-126">hello **key** parameter is used as a lookup key into hello shard map toodetermine hello appropriate database for hello request.</span></span> 
* <span data-ttu-id="47a81-127">Hello **connectionString** je použité toopass pouze hello uživatelská pověření pro připojení hello potřeby.</span><span class="sxs-lookup"><span data-stu-id="47a81-127">hello **connectionString** is used toopass only hello user credentials for hello desired connection.</span></span> <span data-ttu-id="47a81-128">Žádný název databáze nebo název serveru jsou zahrnuté v tomto *connectionString* vzhledem k tomu, že metoda hello určí hello databáze a serveru pomocí hello **ShardMap**.</span><span class="sxs-lookup"><span data-stu-id="47a81-128">No database name or server name are included in this *connectionString* since hello method will determine hello database and server using hello **ShardMap**.</span></span> 
* <span data-ttu-id="47a81-129">Hello  **[connectionOptions](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.connectionoptions.aspx)**  by mělo být nastavené příliš**ConnectionOptions.Validate** Pokud prostředí, kde horizontálního oddílu mapuje může změnit a řádky může přesunutí databází tooother jako výsledek operace rozdělení nebo sloučení.</span><span class="sxs-lookup"><span data-stu-id="47a81-129">hello **[connectionOptions](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.connectionoptions.aspx)** should be set too**ConnectionOptions.Validate** if an environment where shard maps may change and rows may move tooother databases as a result of split or merge operations.</span></span> <span data-ttu-id="47a81-130">To zahrnuje mapu místní horizontálních toohello stručný dotaz na hello cílová databáze (ne toohello globální horizontálních map) před hello připojení doručuje toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="47a81-130">This involves a brief query toohello local shard map on hello target database (not toohello global shard map) before hello connection is delivered toohello application.</span></span> 

<span data-ttu-id="47a81-131">Pokud hello ověření vůči hello místní horizontálních mapování nezdaří, (což znamená, že mezipaměť hello je nesprávná), bude hello správce mapy horizontálního oddílu dotaz hello globální horizontálních mapy tooobtain hello nové správnou hodnotu pro vyhledávání hello aktualizace mezipaměti hello a získat a vrátí hello připojení k příslušné databázi.</span><span class="sxs-lookup"><span data-stu-id="47a81-131">If hello validation against hello local shard map fails (indicating that hello cache is incorrect), hello Shard Map Manager will query hello global shard map tooobtain hello new correct value for hello lookup, update hello cache, and obtain and return hello appropriate database connection.</span></span> 

<span data-ttu-id="47a81-132">Použití **ConnectionOptions.None** pouze když změny mapování horizontálních nejsou očekáván aplikace je online.</span><span class="sxs-lookup"><span data-stu-id="47a81-132">Use **ConnectionOptions.None** only when shard mapping changes are not expected while an application is online.</span></span> <span data-ttu-id="47a81-133">V takovém případě hello mezipaměti hodnoty dá se předpokládat tooalways být správná, a hello velmi odezvy ověření volání toohello cílová databáze mohou být bezpečně přeskočeny.</span><span class="sxs-lookup"><span data-stu-id="47a81-133">In that case, hello cached values can be assumed tooalways be correct, and hello extra round-trip validation call toohello target database can be safely skipped.</span></span> <span data-ttu-id="47a81-134">Který snižuje zatížení databáze.</span><span class="sxs-lookup"><span data-stu-id="47a81-134">That reduces database traffic.</span></span> <span data-ttu-id="47a81-135">Hello **connectionOptions** také lze nastavit prostřednictvím hodnotou v souboru tooindicate konfigurace horizontálního dělení změny se očekává, nebo není během v časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="47a81-135">hello **connectionOptions** may also be set via a value in a configuration file tooindicate whether sharding changes are expected or not during a period of time.</span></span>  

<span data-ttu-id="47a81-136">Tento příklad používá hello hodnotu celé číslo klíče **CustomerID**pomocí **ShardMap** objekt s názvem **customerShardMap**.</span><span class="sxs-lookup"><span data-stu-id="47a81-136">This example uses hello value of an integer key **CustomerID**, using a **ShardMap** object named **customerShardMap**.</span></span>  

    int customerId = 12345; 
    int newPersonId = 4321; 

    // Connect toohello shard for that customer ID. No need toocall a SqlConnection 
    // constructor followed by hello Open method.
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

<span data-ttu-id="47a81-137">Hello **OpenConnectionForKey** metoda vrátí novou databázi správné toohello již otevřené připojení.</span><span class="sxs-lookup"><span data-stu-id="47a81-137">hello **OpenConnectionForKey** method returns a new already-open connection toohello correct database.</span></span> <span data-ttu-id="47a81-138">Připojení v tímto způsobem nadále využívat všech výhod ADO.Net sdružování připojení.</span><span class="sxs-lookup"><span data-stu-id="47a81-138">Connections utilized in this way still take full advantage of ADO.Net connection pooling.</span></span> <span data-ttu-id="47a81-139">Tak dlouho, dokud transakce a požadavky lze splnit jednu horizontálního oddílu současně, to by měl být hello pouze změny potřebné v aplikaci už používá ADO.Net.</span><span class="sxs-lookup"><span data-stu-id="47a81-139">As long as transactions and requests can be satisfied by one shard at a time, this should be hello only modification necessary in an application already using ADO.Net.</span></span> 

<span data-ttu-id="47a81-140">Hello  **[OpenConnectionForKeyAsync metoda](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkeyasync.aspx)**  je také dostupná v případě, že vaše aplikace umožňuje použití asynchronní programování s ADO.Net.</span><span class="sxs-lookup"><span data-stu-id="47a81-140">hello **[OpenConnectionForKeyAsync method](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkeyasync.aspx)** is also available if your application makes use asynchronous programming with ADO.Net.</span></span> <span data-ttu-id="47a81-141">Její chování je závislé směrování ekvivalentní ADO hello data. NET na  **[Connection.OpenAsync](https://msdn.microsoft.com/library/hh223688\(v=vs.110\).aspx)**  metoda.</span><span class="sxs-lookup"><span data-stu-id="47a81-141">Its behavior is hello data dependent routing equivalent of ADO.Net's **[Connection.OpenAsync](https://msdn.microsoft.com/library/hh223688\(v=vs.110\).aspx)** method.</span></span>

## <a name="integrating-with-transient-fault-handling"></a><span data-ttu-id="47a81-142">Integrace s přechodná chyba zpracování</span><span class="sxs-lookup"><span data-stu-id="47a81-142">Integrating with transient fault handling</span></span>
<span data-ttu-id="47a81-143">Osvědčeným postupem při vývoji aplikací přístup dat v cloudu hello je tooensure že přechodných zachytila hello aplikaci a že hello operace jsou několikrát opakována před vyvolání k chybě.</span><span class="sxs-lookup"><span data-stu-id="47a81-143">A best practice in developing data access applications in hello cloud is tooensure that transient faults are caught by hello app, and that hello operations are retried several times before throwing an error.</span></span> <span data-ttu-id="47a81-144">Přechodná chyba zpracování pro cloudové aplikace je popsána v [přechodných chyb](https://msdn.microsoft.com/library/dn440719\(v=pandp.60\).aspx).</span><span class="sxs-lookup"><span data-stu-id="47a81-144">Transient fault handling for cloud applications is discussed at [Transient Fault Handling](https://msdn.microsoft.com/library/dn440719\(v=pandp.60\).aspx).</span></span> 

<span data-ttu-id="47a81-145">Přechodná chyba zpracování mohou existovat vedle sebe přirozeně pomocí vzoru Data závislé směrování hello.</span><span class="sxs-lookup"><span data-stu-id="47a81-145">Transient fault handling can coexist naturally with hello Data Dependent Routing pattern.</span></span> <span data-ttu-id="47a81-146">Hello klíčovým požadavkem je tooretry hello celého datového žádost o přístup včetně hello **pomocí** blok, který získat hello závislé na data směrování připojení.</span><span class="sxs-lookup"><span data-stu-id="47a81-146">hello key requirement is tooretry hello entire data access request including hello **using** block that obtained hello data-dependent routing connection.</span></span> <span data-ttu-id="47a81-147">výše uvedený příklad Hello by mohla být přepsána následujícím způsobem (Všimněte si zvýrazněné změny).</span><span class="sxs-lookup"><span data-stu-id="47a81-147">hello example above could be rewritten as follows (note highlighted change).</span></span> 

### <a name="example---data-dependent-routing-with-transient-fault-handling"></a><span data-ttu-id="47a81-148">Příklad: závislé směrování s přechodná chyba zpracování dat</span><span class="sxs-lookup"><span data-stu-id="47a81-148">Example - data dependent routing with transient fault handling</span></span>
<pre><code>int customerId = 12345; 
int newPersonId = 4321; 

<span style="background-color:  #FFFF00">Configuration.SqlRetryPolicy.ExecuteAction(() =&gt; </span> 
<span style="background-color:  #FFFF00">    { </span>
        // Connect toohello shard for a customer ID. 
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


<span data-ttu-id="47a81-149">Balíčky, které jsou nezbytné tooimplement přechodná chyba zpracování nestahuje automaticky při vytváření hello elastické databáze ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="47a81-149">Packages necessary tooimplement transient fault handling are downloaded automatically when you build hello elastic database sample application.</span></span> <span data-ttu-id="47a81-150">Balíčky jsou k dispozici samostatně na [Enterprise Library - přechodné chyby zpracování bloku aplikace](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/).</span><span class="sxs-lookup"><span data-stu-id="47a81-150">Packages are also available separately at [Enterprise Library - Transient Fault Handling Application Block](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/).</span></span> <span data-ttu-id="47a81-151">Použijte verzi 6.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="47a81-151">Use version 6.0 or later.</span></span> 

## <a name="transactional-consistency"></a><span data-ttu-id="47a81-152">Transakční konzistence</span><span class="sxs-lookup"><span data-stu-id="47a81-152">Transactional consistency</span></span>
<span data-ttu-id="47a81-153">Transakční vlastnosti jsou pro všechny operace místní tooa horizontálních zaručená.</span><span class="sxs-lookup"><span data-stu-id="47a81-153">Transactional properties are guaranteed for all operations local tooa shard.</span></span> <span data-ttu-id="47a81-154">Například transakce odeslat prostřednictvím závislé na data směrování spustit hello rozsahu horizontálního oddílu hello cíl pro hello připojení.</span><span class="sxs-lookup"><span data-stu-id="47a81-154">For example, transactions submitted through data-dependent routing execute within hello scope of hello target shard for hello connection.</span></span> <span data-ttu-id="47a81-155">V současné době neexistují žádné možnosti zadaná pro zapsání několik připojení do transakce a proto nejsou žádné záruky transakcí pro operace prováděné napříč horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="47a81-155">At this time, there are no capabilities provided for enlisting multiple connections into a transaction, and therefore there are no transactional guarantees for operations performed across shards.</span></span>

## <a name="next-steps"></a><span data-ttu-id="47a81-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="47a81-156">Next steps</span></span>
<span data-ttu-id="47a81-157">toodetach horizontálního oddílu nebo tooreattach horizontálního oddílu, najdete v tématu [pomocí hello RecoveryManager třída toofix horizontálního oddílu mapy problémy](sql-database-elastic-database-recovery-manager.md)</span><span class="sxs-lookup"><span data-stu-id="47a81-157">toodetach a shard, or tooreattach a shard, see [Using hello RecoveryManager class toofix shard map problems](sql-database-elastic-database-recovery-manager.md)</span></span>

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

