---
title: "aaaMulti klienta aplikací pomocí nástroje elastické databáze a zabezpečení na úrovni řádků"
description: "Zjistěte, jak nástroje elastické databáze toouse společně s nízkoúrovňové zabezpečení toobuild aplikace s vysoce škálovatelné datové vrstvě v databázi SQL Azure, který podporuje víceklientské horizontálních oddílů."
metakeywords: azure sql database elastic tools multi tenant row level security rls
services: sql-database
documentationcenter: 
manager: jhubbard
author: tmullaney
ms.assetid: e72d3cfe-e9be-4326-b776-9c6d96c0a18e
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: thmullan;torsteng
ms.openlocfilehash: e00076a8db4a295374993aedd49f2318bd4d701d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="multi-tenant-applications-with-elastic-database-tools-and-row-level-security"></a><span data-ttu-id="965e8-103">Víceklientské aplikací pomocí nástroje elastické databáze a zabezpečení na úrovni řádků</span><span class="sxs-lookup"><span data-stu-id="965e8-103">Multi-tenant applications with elastic database tools and row-level security</span></span>
<span data-ttu-id="965e8-104">[Nástroje elastické databáze](sql-database-elastic-scale-get-started.md) a [nízkoúrovňového zabezpečení (RLS)](https://msdn.microsoft.com/library/dn765131) nabízejí výkonnou sadu funkcí pro flexibilní a efektivní škálování hello datové vrstvy víceklientské aplikace s Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="965e8-104">[Elastic database tools](sql-database-elastic-scale-get-started.md) and [row-level security (RLS)](https://msdn.microsoft.com/library/dn765131) offer a powerful set of capabilities for flexibly and efficiently scaling hello data tier of a multi-tenant application with Azure SQL Database.</span></span> <span data-ttu-id="965e8-105">V tématu [návrhová schémata pro víceklientské aplikace SaaS ve službě Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="965e8-105">See [Design Patterns for Multi-tenant SaaS Applications with Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) for more information.</span></span> 

<span data-ttu-id="965e8-106">Tento článek ukazuje, jak toouse společně toobuild aplikace s vysoce škálovatelné datové vrstvy, která podporuje více klientů horizontálních oddílů, pomocí těchto technologií **ADO.NET SqlClient** nebo **Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="965e8-106">This article illustrates how toouse these technologies together toobuild an application with a highly scalable data tier that supports multi-tenant shards, using **ADO.NET SqlClient** and/or **Entity Framework**.</span></span>  

* <span data-ttu-id="965e8-107">**Nástroje elastické databáze** umožňuje vývojářům tooscale na datové vrstvě hello aplikace prostřednictvím horizontálního dělení standardní postupy, pomocí sady knihovny .NET a šablony služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="965e8-107">**Elastic database tools** enables developers tooscale out hello data tier of an application via industry-standard sharding practices using a set of .NET libraries and Azure service templates.</span></span> <span data-ttu-id="965e8-108">Správa horizontálních oddílů s použitím hello klientské knihovny pro elastické databáze pomáhá zautomatizovat a zjednodušit řadu hello infrastruktury úloh obvykle přidružených horizontálního dělení.</span><span class="sxs-lookup"><span data-stu-id="965e8-108">Managing shards with using hello Elastic Database Client Library helps automate and streamline many of hello infrastructural tasks typically associated with sharding.</span></span> 
* <span data-ttu-id="965e8-109">**Zabezpečení na úrovni řádků** umožňuje vývojářům toostore data pro více klienty ve stejné databázi pomocí toofilter zásad zabezpečení na řádky, které nejsou součástí klienta toohello provádění dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="965e8-109">**Row-level security** enables developers toostore data for multiple tenants in hello same database using security policies toofilter out rows that do not belong toohello tenant executing a query.</span></span> <span data-ttu-id="965e8-110">Centralizuje logiku přístup s RLS uvnitř databáze hello spíše než v aplikaci hello zjednodušuje údržbu a snižuje riziko hello chyby jako základu kódu aplikace zvětšování.</span><span class="sxs-lookup"><span data-stu-id="965e8-110">Centralizing access logic with RLS inside hello database, rather than in hello application, simplifies maintenance and reduces hello risk of error as an application’s codebase grows.</span></span> 

<span data-ttu-id="965e8-111">Použití těchto funkcí společně, aplikace, můžete využít zvýšení úspory a efektivitu nákladů uložením dat pro více klienty ve hello stejné ID horizontálního oddílu databáze.</span><span class="sxs-lookup"><span data-stu-id="965e8-111">Using these features together, an application can benefit from cost savings and efficiency gains by storing data for multiple tenants in hello same shard database.</span></span> <span data-ttu-id="965e8-112">Na hello má stejnou dobu, stále aplikace hello flexibilitu toooffer izolované, jednoho klienta horizontálních oddílů pro "premium" klienty, kteří vyžadují přísnější záruky výkonu vzhledem k tomu, že více klientů horizontálních oddílů není zaručeno distribuce rovna prostředků mezi klienty.</span><span class="sxs-lookup"><span data-stu-id="965e8-112">At hello same time, an application still has hello flexibility toooffer isolated, single-tenant shards for “premium” tenants who require stricter performance guarantees since multi-tenant shards do not guarantee equal resource distribution among tenants.</span></span>  

<span data-ttu-id="965e8-113">Stručně řečeno, hello elastické databáze klientské knihovny [data závislé směrování](sql-database-elastic-scale-data-dependent-routing.md) rozhraní API automaticky připojit klienty toohello správné horizontálních databáze obsahující jejich horizontálního dělení klíč (obecně "TenantId").</span><span class="sxs-lookup"><span data-stu-id="965e8-113">In short, hello elastic database client library’s [data dependent routing](sql-database-elastic-scale-data-dependent-routing.md) APIs automatically connect tenants toohello correct shard database containing their sharding key (generally a “TenantId”).</span></span> <span data-ttu-id="965e8-114">Po připojení, zásady zabezpečení RLS v rámci databáze hello zajišťuje, aby klienti můžete pouze přístup k řádky, které obsahují jejich TenantId.</span><span class="sxs-lookup"><span data-stu-id="965e8-114">Once connected, an RLS security policy within hello database ensures that tenants can only access rows that contain their TenantId.</span></span> <span data-ttu-id="965e8-115">Předpokládá se, že všechny tabulky obsahují tooindicate sloupce TenantId řádky patří tooeach klienta.</span><span class="sxs-lookup"><span data-stu-id="965e8-115">It is assumed that all tables contain a TenantId column tooindicate which rows belong tooeach tenant.</span></span> 

![Architektura aplikace blogu][1]

## <a name="download-hello-sample-project"></a><span data-ttu-id="965e8-117">Stáhněte si ukázkový projekt hello</span><span class="sxs-lookup"><span data-stu-id="965e8-117">Download hello sample project</span></span>
### <a name="prerequisites"></a><span data-ttu-id="965e8-118">Požadavky</span><span class="sxs-lookup"><span data-stu-id="965e8-118">Prerequisites</span></span>
* <span data-ttu-id="965e8-119">Použijte sadu Visual Studio (2012 nebo vyšší)</span><span class="sxs-lookup"><span data-stu-id="965e8-119">Use Visual Studio (2012 or higher)</span></span> 
* <span data-ttu-id="965e8-120">Vytvořte tři databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="965e8-120">Create three Azure SQL Databases</span></span> 
* <span data-ttu-id="965e8-121">Stáhněte si ukázkový projekt: [elastické databáze nástroje pro Azure SQL - víceklientské horizontálních oddílů](http://go.microsoft.com/?linkid=9888163)</span><span class="sxs-lookup"><span data-stu-id="965e8-121">Download sample project: [Elastic DB Tools for Azure SQL - Multi-Tenant Shards](http://go.microsoft.com/?linkid=9888163)</span></span>
  * <span data-ttu-id="965e8-122">Vyplnit hello informace pro vaše databáze na začátku hello **Program.cs**</span><span class="sxs-lookup"><span data-stu-id="965e8-122">Fill in hello information for your databases at hello beginning of **Program.cs**</span></span> 

<span data-ttu-id="965e8-123">Rozšiřuje hello jeden popsané v tomto projektu [elastické databáze nástroje pro Azure SQL - Entity Framework integrace](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) přidáním podpory pro víc klientů horizontálních databáze.</span><span class="sxs-lookup"><span data-stu-id="965e8-123">This project extends hello one described in [Elastic DB Tools for Azure SQL - Entity Framework Integration](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) by adding support for multi-tenant shard databases.</span></span> <span data-ttu-id="965e8-124">Sestavuje pro vytváření blogy a příspěvky, s čtyři klientů a dvě databáze víceklientské horizontálních, jak je ukázáno v hello výše diagram jednoduché konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="965e8-124">It builds a simple console application for creating blogs and posts, with four tenants and two multi-tenant shard databases as illustrated in hello above diagram.</span></span> 

<span data-ttu-id="965e8-125">Sestavení a spuštění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="965e8-125">Build and run hello application.</span></span> <span data-ttu-id="965e8-126">To bude bootstrap nástroje elastické databáze hello horizontálního oddílu mapa správce a spusťte hello následující testy:</span><span class="sxs-lookup"><span data-stu-id="965e8-126">This will bootstrap hello elastic database tools’ shard map manager and run hello following tests:</span></span> 

1. <span data-ttu-id="965e8-127">Pomocí rozhraní Entity Framework a LINQ, vytvořte nový blog a následně se zobrazí všechny blogy pro každého klienta</span><span class="sxs-lookup"><span data-stu-id="965e8-127">Using Entity Framework and LINQ, create a new blog and then display all blogs for each tenant</span></span>
2. <span data-ttu-id="965e8-128">Pomocí ADO.NET SqlClient, zobrazte všechny blogy pro klienta</span><span class="sxs-lookup"><span data-stu-id="965e8-128">Using ADO.NET SqlClient, display all blogs for a tenant</span></span>
3. <span data-ttu-id="965e8-129">Zkuste tooinsert blog pro tooverify hello nesprávný klienta, která je vyvolána chyba</span><span class="sxs-lookup"><span data-stu-id="965e8-129">Try tooinsert a blog for hello wrong tenant tooverify that an error is thrown</span></span>  

<span data-ttu-id="965e8-130">Všimněte si, že protože RLS dosud nebyla spuštěna v databázích hello horizontálního oddílu, každý z těchto testů zjistí problém: klientů jsou možné toosee blogy, který nepatří toothem a aplikace hello není mohou vkládání blogu pro hello nesprávný klienta.</span><span class="sxs-lookup"><span data-stu-id="965e8-130">Notice that because RLS has not yet been enabled in hello shard databases, each of these tests reveals a problem: tenants are able toosee blogs that do not belong toothem, and hello application is not prevented from inserting a blog for hello wrong tenant.</span></span> <span data-ttu-id="965e8-131">Hello zbývající část tohoto článku popisuje, jak tooresolve tyto problémy vynucením klienta izolace s RLS.</span><span class="sxs-lookup"><span data-stu-id="965e8-131">hello remainder of this article describes how tooresolve these problems by enforcing tenant isolation with RLS.</span></span> <span data-ttu-id="965e8-132">Existují dva kroky:</span><span class="sxs-lookup"><span data-stu-id="965e8-132">There are two steps:</span></span> 

1. <span data-ttu-id="965e8-133">**Aplikační vrstvě**: Upravit kód aplikace hello tooalways sadu hello aktuální TenantId v hello SESSION_CONTEXT po otevření připojení.</span><span class="sxs-lookup"><span data-stu-id="965e8-133">**Application tier**: Modify hello application code tooalways set hello current TenantId in hello SESSION_CONTEXT after opening a connection.</span></span> <span data-ttu-id="965e8-134">Ukázkový projekt Hello již bylo dokončeno to.</span><span class="sxs-lookup"><span data-stu-id="965e8-134">hello sample project has already done this.</span></span> 
2. <span data-ttu-id="965e8-135">**Datová vrstva**: vytvoření zásady zabezpečení RLS v každé toofilter databáze horizontálního oddílu řádků v závislosti na hello TenantId uložené v SESSION_CONTEXT.</span><span class="sxs-lookup"><span data-stu-id="965e8-135">**Data tier**: Create an RLS security policy in each shard database toofilter rows based on hello TenantId stored in SESSION_CONTEXT.</span></span> <span data-ttu-id="965e8-136">Budete potřebovat toodo pro každou databází horizontálního oddílu, jinak se nebudou filtrovat řádky v víceklientské horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="965e8-136">You will need toodo this for each of your shard databases, otherwise rows in multi-tenant shards will not be filtered.</span></span> 

## <a name="step-1-application-tier-set-tenantid-in-hello-sessioncontext"></a><span data-ttu-id="965e8-137">Krok 1) aplikační vrstvě: nastavte TenantId v hello SESSION_CONTEXT</span><span class="sxs-lookup"><span data-stu-id="965e8-137">Step 1) Application tier: Set TenantId in hello SESSION_CONTEXT</span></span>
<span data-ttu-id="965e8-138">Po připojování tooa horizontálního oddílu databázi pomocí hello elastické databáze klientské knihovny pro data, která je stále rozhraní API pro směrování závislé, aplikace hello musí databáze hello tootell které TenantId používá toto připojení tak, aby zásady zabezpečení RLS můžete filtrovat řádky patřící tooother klienty.</span><span class="sxs-lookup"><span data-stu-id="965e8-138">After connecting tooa shard database using hello elastic database client library’s data dependent routing APIs, hello application still needs tootell hello database which TenantId is using that connection so that an RLS security policy can filter out rows belonging tooother tenants.</span></span> <span data-ttu-id="965e8-139">Hello doporučeným způsobem toopass tyto informace jsou toostore hello aktuální TenantId pro toto připojení v hello [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806.aspx).</span><span class="sxs-lookup"><span data-stu-id="965e8-139">hello recommended way toopass this information is toostore hello current TenantId for that connection in hello [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806.aspx).</span></span> <span data-ttu-id="965e8-140">(Poznámka: můžete alternativně použít [CONTEXT_INFO](https://msdn.microsoft.com/library/ms180125.aspx), ale SESSION_CONTEXT je lepší volbou, protože je jednodušší toouse, vrátí hodnotu NULL ve výchozím nastavení a podporuje páry klíč hodnota.)</span><span class="sxs-lookup"><span data-stu-id="965e8-140">(Note: You could alternatively use [CONTEXT_INFO](https://msdn.microsoft.com/library/ms180125.aspx), but SESSION_CONTEXT is a better option because it is easier toouse, returns NULL by default, and supports key-value pairs.)</span></span>

### <a name="entity-framework"></a><span data-ttu-id="965e8-141">Entity Framework</span><span class="sxs-lookup"><span data-stu-id="965e8-141">Entity Framework</span></span>
<span data-ttu-id="965e8-142">Pro aplikace používající rozhraní Entity Framework hello nejjednodušší způsob je tooset hello SESSION_CONTEXT v rámci hello ElasticScaleContext přepsat popsané v [Data závislé směrování pomocí EF DbContext](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md#data-dependent-routing-using-ef-dbcontext).</span><span class="sxs-lookup"><span data-stu-id="965e8-142">For applications using Entity Framework, hello easiest approach is tooset hello SESSION_CONTEXT within hello ElasticScaleContext override described in [Data Dependent Routing using EF DbContext](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md#data-dependent-routing-using-ef-dbcontext).</span></span> <span data-ttu-id="965e8-143">Před vrácením hello připojení zprostředkované přes data závislé směrování, jednoduše vytvoření a provedení SqlCommand, který nastaví 'TenantId' v hello SESSION_CONTEXT toohello shardingKey zadaný pro toto připojení.</span><span class="sxs-lookup"><span data-stu-id="965e8-143">Before returning hello connection brokered through data dependent routing, simply create and execute a SqlCommand that sets 'TenantId' in hello SESSION_CONTEXT toohello shardingKey specified for that connection.</span></span> <span data-ttu-id="965e8-144">Tímto způsobem, stačí toowrite kódu po tooset hello SESSION_CONTEXT.</span><span class="sxs-lookup"><span data-stu-id="965e8-144">This way, you only need toowrite code once tooset hello SESSION_CONTEXT.</span></span> 

```
// ElasticScaleContext.cs 
// ... 
// C'tor for data dependent routing. This call will open a validated connection routed toohello proper 
// shard by hello shard map manager. Note that hello base class c'tor call will fail for an open connection 
// if migrations need toobe done and SQL credentials are used. This is hello reason for hello  
// separation of c'tors into hello DDR case (this c'tor) and hello internal c'tor for new shards. 
public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
    : base(OpenDDRConnection(shardMap, shardingKey, connectionStr), true /* contextOwnsConnection */)
{
}

public static SqlConnection OpenDDRConnection(ShardMap shardMap, T shardingKey, string connectionStr)
{
    // No initialization
    Database.SetInitializer<ElasticScaleContext<T>>(null);

    // Ask shard map toobroker a validated connection for hello given key
    SqlConnection conn = null;
    try
    {
        conn = shardMap.OpenConnectionForKey(shardingKey, connectionStr, ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT tooshardingKey tooenable Row-Level Security filtering
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"exec sp_set_session_context @key=N'TenantId', @value=@shardingKey";
        cmd.Parameters.AddWithValue("@shardingKey", shardingKey);
        cmd.ExecuteNonQuery();

        return conn;
    }
    catch (Exception)
    {
        if (conn != null)
        {
            conn.Dispose();
        }

        throw;
    }
} 
// ... 
```

<span data-ttu-id="965e8-145">Teď hello SESSION_CONTEXT bude automaticky nastavena pomocí zadané hello TenantId vždy, když je volána ElasticScaleContext:</span><span class="sxs-lookup"><span data-stu-id="965e8-145">Now hello SESSION_CONTEXT is automatically set with hello specified TenantId whenever ElasticScaleContext is invoked:</span></span> 

```
// Program.cs 
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
{   
    using (var db = new ElasticScaleContext<int>(sharding.ShardMap, tenantId, connStrBldr.ConnectionString))   
    {     
        var query = from b in db.Blogs
                    orderby b.Name
                    select b;

        Console.WriteLine("All blogs for TenantId {0}:", tenantId);     
        foreach (var item in query)     
        {       
            Console.WriteLine(item.Name);     
        }   
    } 
}); 
```

### <a name="adonet-sqlclient"></a><span data-ttu-id="965e8-146">ADO.NET SqlClient</span><span class="sxs-lookup"><span data-stu-id="965e8-146">ADO.NET SqlClient</span></span>
<span data-ttu-id="965e8-147">Pro aplikace pomocí ADO.NET SqlClient hello doporučený postup je toocreate funkce obálku kolem ShardMap.OpenConnectionForKey(), který automaticky nastaví 'TenantId' v hello SESSION_CONTEXT toohello opravte TenantId před vrácením připojení.</span><span class="sxs-lookup"><span data-stu-id="965e8-147">For applications using ADO.NET SqlClient, hello recommended approach is toocreate a wrapper function around ShardMap.OpenConnectionForKey() that automatically sets 'TenantId' in hello SESSION_CONTEXT toohello correct TenantId before returning a connection.</span></span> <span data-ttu-id="965e8-148">tooensure, který je vždycky nastavený SESSION_CONTEXT, by měla pouze otevřít připojení pomocí této funkce obálku.</span><span class="sxs-lookup"><span data-stu-id="965e8-148">tooensure that SESSION_CONTEXT is always set, you should only open connections using this wrapper function.</span></span>

```
// Program.cs
// ...

// Wrapper function for ShardMap.OpenConnectionForKey() that automatically sets SESSION_CONTEXT with hello correct
// tenantId before returning a connection. As a best practice, you should only open connections using this 
// method tooensure that SESSION_CONTEXT is always set before executing a query.
public static SqlConnection OpenConnectionForTenant(ShardMap shardMap, int tenantId, string connectionStr)
{
    SqlConnection conn = null;
    try
    {
        // Ask shard map toobroker a validated connection for hello given key
        conn = shardMap.OpenConnectionForKey(tenantId, connectionStr, ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT tooshardingKey tooenable Row-Level Security filtering
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"exec sp_set_session_context @key=N'TenantId', @value=@shardingKey";
        cmd.Parameters.AddWithValue("@shardingKey", tenantId);
        cmd.ExecuteNonQuery();

        return conn;
    }
    catch (Exception)
    {
        if (conn != null)
        {
            conn.Dispose();
        }

        throw;
    }
}

// ...

// Example query via ADO.NET SqlClient
// If row-level security is enabled, only Tenant 4's blogs will be listed
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
{
    using (SqlConnection conn = OpenConnectionForTenant(sharding.ShardMap, tenantId4, connStrBldr.ConnectionString))
    {
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"SELECT * FROM Blogs";

        Console.WriteLine("--\nAll blogs for TenantId {0} (using ADO.NET SqlClient):", tenantId4);
        SqlDataReader reader = cmd.ExecuteReader();
        while (reader.Read())
        {
            Console.WriteLine("{0}", reader["Name"]);
        }
    }
});

```

## <a name="step-2-data-tier-create-row-level-security-policy"></a><span data-ttu-id="965e8-149">Krok 2) Data vrstvy: vytvoření zásad zabezpečení na úrovni řádků</span><span class="sxs-lookup"><span data-stu-id="965e8-149">Step 2) Data tier: Create row-level security policy</span></span>
### <a name="create-a-security-policy-toofilter-hello-rows-each-tenant-can-access"></a><span data-ttu-id="965e8-150">Vytvoření hello toofilter zásady zabezpečení řádky, které můžete přístup každý klient</span><span class="sxs-lookup"><span data-stu-id="965e8-150">Create a security policy toofilter hello rows each tenant can access</span></span>
<span data-ttu-id="965e8-151">Teď, když je aplikace hello nastavení SESSION_CONTEXT s hello aktuální TenantId před odesláním dotazu, můžete zásadu zabezpečení RLS filtrovat dotazy a vyloučit řádky, které mají různé TenantId.</span><span class="sxs-lookup"><span data-stu-id="965e8-151">Now that hello application is setting SESSION_CONTEXT with hello current TenantId before querying, an RLS security policy can filter queries and exclude rows that have a different TenantId.</span></span>  

<span data-ttu-id="965e8-152">RLS je implementovaná v T-SQL: uživatelem definované funkce definuje logiku hello přístupu a zásady zabezpečení sváže této funkce tooany počet tabulek.</span><span class="sxs-lookup"><span data-stu-id="965e8-152">RLS is implemented in T-SQL: a user-defined function defines hello access logic, and a security policy binds this function tooany number of tables.</span></span> <span data-ttu-id="965e8-153">Pro tento projekt bude funkce hello jednoduše ověřte, zda hello aplikaci (namísto jiný uživatel SQL) je připojený toohello databáze a že hello 'TenantId' uložené v hello SESSION_CONTEXT odpovídá hello TenantId daného řádku.</span><span class="sxs-lookup"><span data-stu-id="965e8-153">For this project, hello function will simply verify that hello application (rather than some other SQL user) is connected toohello database, and that hello 'TenantId' stored in hello SESSION_CONTEXT matches hello TenantId of a given row.</span></span> <span data-ttu-id="965e8-154">Predikát filtru vám umožní řádky, které splňují tyto podmínky toopass prostřednictvím hello filtru pro dotazy SELECT, UPDATE a DELETE; a predikát block zabrání řádků, které porušují tyto podmínky z vložené nebo aktualizované.</span><span class="sxs-lookup"><span data-stu-id="965e8-154">A filter predicate will allow rows that meet these conditions toopass through hello filter for SELECT, UPDATE, and DELETE queries; and a block predicate will prevent rows that violate these conditions from being INSERTed or UPDATEd.</span></span> <span data-ttu-id="965e8-155">Pokud SESSION_CONTEXT nebyla nastavena, vrátí hodnotu NULL a žádné řádky bude toobe viditelný nebo možnost vložit.</span><span class="sxs-lookup"><span data-stu-id="965e8-155">If SESSION_CONTEXT has not been set, it will return NULL and no rows will be visible or able toobe inserted.</span></span> 

<span data-ttu-id="965e8-156">tooenable RLS, proveďte následující T-SQL na všechny horizontálních oddílů pomocí buď Visual Studio (SSDT), aplikace SSMS, hello nebo hello skript prostředí PowerShell, které jsou součástí projektu hello (nebo pokud používáte [elastické databáze úlohy](sql-database-elastic-jobs-overview.md), můžete ho tooautomate provádění Tato T-SQL na všechny horizontálních oddílů):</span><span class="sxs-lookup"><span data-stu-id="965e8-156">tooenable RLS, execute hello following T-SQL on all shards using either Visual Studio (SSDT), SSMS, or hello PowerShell script included in hello project (or if you are using [Elastic Database Jobs](sql-database-elastic-jobs-overview.md), you can use it tooautomate execution of this T-SQL on all shards):</span></span> 

```
CREATE SCHEMA rls -- separate schema tooorganize RLS objects 
GO

CREATE FUNCTION rls.fn_tenantAccessPredicate(@TenantId int)     
    RETURNS TABLE     
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult          
        WHERE DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo') -- hello user in your application’s connection string (dbo is only for demo purposes!)         
        AND CAST(SESSION_CONTEXT(N'TenantId') AS int) = @TenantId
GO

CREATE SECURITY POLICY rls.tenantAccessPolicy
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts
GO 
```

> [!TIP]
> <span data-ttu-id="965e8-157">Pro složitější projekty, které je třeba tooadd hello predikát stovky tabulky můžete pomocná uložené procedury, která automaticky generuje zásadu zabezpečení přidání predikát ve všech tabulkách ve schématu.</span><span class="sxs-lookup"><span data-stu-id="965e8-157">For more complex projects that need tooadd hello predicate on hundreds of tables, you can use a helper stored procedure that automatically generates a security policy adding a predicate on all tables in a schema.</span></span> <span data-ttu-id="965e8-158">V tématu [použití zabezpečení na úrovni řádků tabulky tooall – Pomocník skriptu (blog)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/03/31/apply-row-level-security-to-all-tables-helper-script).</span><span class="sxs-lookup"><span data-stu-id="965e8-158">See [Apply Row-Level Security tooall tables - helper script (blog)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/03/31/apply-row-level-security-to-all-tables-helper-script).</span></span>  
> 
> 

<span data-ttu-id="965e8-159">Teď Pokud spustíte znovu hello ukázkovou aplikaci, klienti budou moct toosee pouze řádky, které patří toothem.</span><span class="sxs-lookup"><span data-stu-id="965e8-159">Now if you run hello sample application again, tenants will able toosee only rows that belong toothem.</span></span> <span data-ttu-id="965e8-160">Kromě toho aplikace hello nejdou vložit řádky, které patří tootenants než hello jeden aktuálně připojené toohello horizontálního oddílu databáze a nemůže se aktualizovat viditelné řádky toohave různých TenantId.</span><span class="sxs-lookup"><span data-stu-id="965e8-160">In addition, hello application cannot insert rows that belong tootenants other than hello one currently connected toohello shard database, and it cannot update visible rows toohave a different TenantId.</span></span> <span data-ttu-id="965e8-161">Pokud aplikace hello pokusí toodo buď, DbUpdateException, bude vyvolána.</span><span class="sxs-lookup"><span data-stu-id="965e8-161">If hello application attempts toodo either, a DbUpdateException will be raised.</span></span>

<span data-ttu-id="965e8-162">Pokud později na přidáte nové tabulky, jednoduše ALTER hello zásady zabezpečení a přidejte filtr a zároveň se zablokují predikáty v nové tabulce hello:</span><span class="sxs-lookup"><span data-stu-id="965e8-162">If you add a new table later on, simply ALTER hello security policy and add filter and block predicates on hello new table:</span></span> 

```
ALTER SECURITY POLICY rls.tenantAccessPolicy     
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable
GO 
```

### <a name="add-default-constraints-tooautomatically-populate-tenantid-for-inserts"></a><span data-ttu-id="965e8-163">Přidat výchozí omezení tooautomatically naplnit TenantId pro vložení</span><span class="sxs-lookup"><span data-stu-id="965e8-163">Add default constraints tooautomatically populate TenantId for INSERTs</span></span>
<span data-ttu-id="965e8-164">Můžete vložit výchozí omezení pro každou tabulku tooautomatically naplnit hello TenantId s hello hodnoty, které jsou aktuálně uloženy ve SESSION_CONTEXT při vložení řádků.</span><span class="sxs-lookup"><span data-stu-id="965e8-164">You can put a default constraint on each table tooautomatically populate hello TenantId with hello value currently stored in SESSION_CONTEXT when inserting rows.</span></span> <span data-ttu-id="965e8-165">Například:</span><span class="sxs-lookup"><span data-stu-id="965e8-165">For example:</span></span> 

```
-- Create default constraints tooauto-populate TenantId with hello value of SESSION_CONTEXT for inserts 
ALTER TABLE Blogs     
    ADD CONSTRAINT df_TenantId_Blogs      
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId 
GO

ALTER TABLE Posts     
    ADD CONSTRAINT df_TenantId_Posts      
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId 
GO 
```

<span data-ttu-id="965e8-166">Nyní aplikace hello nepotřebuje toospecify TenantId při vložení řádků:</span><span class="sxs-lookup"><span data-stu-id="965e8-166">Now hello application does not need toospecify a TenantId when inserting rows:</span></span> 

```
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
{   
    using (var db = new ElasticScaleContext<int>(sharding.ShardMap, tenantId, connStrBldr.ConnectionString))
    {
        var blog = new Blog { Name = name }; // default constraint sets TenantId automatically     
        db.Blogs.Add(blog);     
        db.SaveChanges();   
    } 
}); 
```

> [!NOTE]
> <span data-ttu-id="965e8-167">Pokud používáte výchozí omezení pro projekt Entity Framework, se doporučuje nezahrnujte do datového modelu EF hello TenantId sloupce.</span><span class="sxs-lookup"><span data-stu-id="965e8-167">If you use default constraints for an Entity Framework project, it is recommended that you do NOT include hello TenantId column in your EF data model.</span></span> <span data-ttu-id="965e8-168">Je to proto, že automaticky zadat výchozí hodnoty, které přepíší hello výchozí omezení vytvořené v T-SQL používající SESSION_CONTEXT dotazy na Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="965e8-168">This is because Entity Framework queries automatically supply default values which will override hello default constraints created in T-SQL that use SESSION_CONTEXT.</span></span> <span data-ttu-id="965e8-169">výchozí omezení toouse v hello ukázkový projekt, například TenantId byste měli odebrat z DataClasses.cs (a spuštění Add-Migration v hello Konzola správce balíčků) a tooensure použití T-SQL, který hello pole existuje pouze v tabulkách databáze hello.</span><span class="sxs-lookup"><span data-stu-id="965e8-169">toouse default constraints in hello sample project, for instance, you should remove TenantId from DataClasses.cs (and run Add-Migration in hello Package Manager Console) and use T-SQL tooensure that hello field only exists in hello database tables.</span></span> <span data-ttu-id="965e8-170">Tímto způsobem EF nebude zadat automaticky nesprávný výchozí hodnoty při vkládání dat.</span><span class="sxs-lookup"><span data-stu-id="965e8-170">This way, EF will not automatically supply incorrect default values when inserting data.</span></span> 
> 
> 

### <a name="optional-enable-a-superuser-tooaccess-all-rows"></a><span data-ttu-id="965e8-171">(Volitelné) Povolit "superuživatel" tooaccess všechny řádky</span><span class="sxs-lookup"><span data-stu-id="965e8-171">(Optional) Enable a "superuser" tooaccess all rows</span></span>
<span data-ttu-id="965e8-172">Některé aplikace může být vhodné toocreate "superuživatele" kdo má přístup všechny řádky, například v tooenable pořadí vytváření sestav v rámci všech klientů na všech horizontálních oddílů nebo tooperform rozdělení či sloučení operací na horizontálních oddílů, které zahrnují přesunutí řádků klienta mezi databázemi.</span><span class="sxs-lookup"><span data-stu-id="965e8-172">Some applications may want toocreate a "superuser" who can access all rows, for instance, in order tooenable reporting across all tenants on all shards, or tooperform Split/Merge operations on shards that involve moving tenant rows between databases.</span></span> <span data-ttu-id="965e8-173">tooenable, měli byste vytvořit nového uživatele SQL ("superuživatel" v tomto příkladu) v každé databázi horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="965e8-173">tooenable this, you should create a new SQL user ("superuser" in this example) in each shard database.</span></span> <span data-ttu-id="965e8-174">Potom změňte zásady zabezpečení hello s novou predikátem funkci, která umožňuje tento uživatel tooaccess všechny řádky:</span><span class="sxs-lookup"><span data-stu-id="965e8-174">Then alter hello security policy with a new predicate function that allows this user tooaccess all rows:</span></span>

```
-- New predicate function that adds superuser logic
CREATE FUNCTION rls.fn_tenantAccessPredicateWithSuperUser(@TenantId int)
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult 
        WHERE 
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo') -- note, should not be dbo!
            AND CAST(SESSION_CONTEXT(N'TenantId') AS int) = @TenantId
        ) 
        OR
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('superuser')
        )
GO

-- Atomically swap in hello new predicate function on each table
ALTER SECURITY POLICY rls.tenantAccessPolicy
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER BLOCK PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts,
    ALTER BLOCK PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts
GO
```


### <a name="maintenance"></a><span data-ttu-id="965e8-175">Údržby</span><span class="sxs-lookup"><span data-stu-id="965e8-175">Maintenance</span></span>
* <span data-ttu-id="965e8-176">**Přidání nové horizontálních oddílů**: hello T-SQL skriptu tooenable RLS je třeba spustit na jakékoli nové horizontálních oddílů, v opačném případě se nebudou filtrovat dotazy na tyto horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="965e8-176">**Adding new shards**: You must execute hello T-SQL script tooenable RLS on any new shards, otherwise queries on these shards will not be filtered.</span></span>
* <span data-ttu-id="965e8-177">**Přidání nové tabulky**: je nutné přidat filtr a blokovat predikátem toohello zásad zabezpečení na všechny horizontálních oddílů vždy, když se vytvoří nové tabulky, v opačném případě se nebudou filtrovat dotazy na novou tabulku hello.</span><span class="sxs-lookup"><span data-stu-id="965e8-177">**Adding new tables**: You must add a filter and block predicate toohello security policy on all shards whenever a new table is created, otherwise queries on hello new table will not be filtered.</span></span> <span data-ttu-id="965e8-178">To je možné automatizovat pomocí aktivační událost jazyka DDL, jak je popsáno v [použití zabezpečení na úrovni řádků automaticky toonewly vytvořené tabulky (blog)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/22/apply-row-level-security-automatically-to-newly-created-tables.aspx).</span><span class="sxs-lookup"><span data-stu-id="965e8-178">This can be automated using a DDL trigger, as described in [Apply Row-Level Security automatically toonewly created tables (blog)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/22/apply-row-level-security-automatically-to-newly-created-tables.aspx).</span></span>

## <a name="summary"></a><span data-ttu-id="965e8-179">Souhrn</span><span class="sxs-lookup"><span data-stu-id="965e8-179">Summary</span></span>
<span data-ttu-id="965e8-180">Nástroje elastické databáze a zabezpečení na úrovni řádků může být použité tooscale společně se aplikace datové vrstvy s podporou pro více klientů a jednoho klienta horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="965e8-180">Elastic database tools and row-level security can be used together tooscale out an application’s data tier with support for both multi-tenant and single-tenant shards.</span></span> <span data-ttu-id="965e8-181">Víceklientské horizontálních oddílů lze použít toostore data efektivněji (obzvláště v případech, kdy velký počet klientů, které mají jenom pár řádků dat), při jednoho klienta horizontálních oddílů lze použít toosupport premium klientů s přísnější výkon a izolace požadavky.</span><span class="sxs-lookup"><span data-stu-id="965e8-181">Multi-tenant shards can be used toostore data more efficiently (particularly in cases where a large number of tenants have only a few rows of data), while single-tenant shards can be used toosupport premium tenants with stricter performance and isolation requirements.</span></span>  <span data-ttu-id="965e8-182">Další informace najdete v tématu [zabezpečení na úrovni řádků odkaz](https://msdn.microsoft.com/library/dn765131).</span><span class="sxs-lookup"><span data-stu-id="965e8-182">For more information, see [Row-Level Security reference](https://msdn.microsoft.com/library/dn765131).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="965e8-183">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="965e8-183">Additional resources</span></span>
* [<span data-ttu-id="965e8-184">Co je Azure elastickém fondu?</span><span class="sxs-lookup"><span data-stu-id="965e8-184">What is an Azure elastic pool?</span></span>](sql-database-elastic-pool.md)
* [<span data-ttu-id="965e8-185">Horizontální navýšení kapacity s Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="965e8-185">Scaling out with Azure SQL Database</span></span>](sql-database-elastic-scale-introduction.md)
* [<span data-ttu-id="965e8-186">Vzory návrhu pro aplikace SaaS s více tenanty využívající Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="965e8-186">Design Patterns for Multi-tenant SaaS Applications with Azure SQL Database</span></span>](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [<span data-ttu-id="965e8-187">Ověřování ve víceklientských aplikacích s využitím Azure AD a OpenID Connect</span><span class="sxs-lookup"><span data-stu-id="965e8-187">Authentication in multitenant apps, using Azure AD and OpenID Connect</span></span>](../guidance/guidance-multitenant-identity-authenticate.md)
* [<span data-ttu-id="965e8-188">Aplikace Tailspin Surveys</span><span class="sxs-lookup"><span data-stu-id="965e8-188">Tailspin Surveys application</span></span>](../guidance/guidance-multitenant-identity-tailspin.md)

## <a name="questions-and-feature-requests"></a><span data-ttu-id="965e8-189">Otázky a žádosti o funkce</span><span class="sxs-lookup"><span data-stu-id="965e8-189">Questions and Feature Requests</span></span>
<span data-ttu-id="965e8-190">Máte dotazy, kontaktujte toous na hello [SQL Database fórum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) a pro žádosti o funkce, přidejte je toohello [fóru pro zpětnou vazbu SQL Database](https://feedback.azure.com/forums/217321-sql-database/).</span><span class="sxs-lookup"><span data-stu-id="965e8-190">For questions, please reach out toous on hello [SQL Database forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) and for feature requests, please add them toohello [SQL Database feedback forum](https://feedback.azure.com/forums/217321-sql-database/).</span></span>

<!--Image references-->
[1]: ./media/sql-database-elastic-tools-multi-tenant-row-level-security/blogging-app.png
<!--anchors-->


