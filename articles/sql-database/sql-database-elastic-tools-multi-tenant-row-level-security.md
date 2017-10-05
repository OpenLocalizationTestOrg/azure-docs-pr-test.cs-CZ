---
title: "Víceklientské aplikací pomocí nástroje elastické databáze a zabezpečení na úrovni řádků"
description: "Další informace o použití nástroje elastické databáze spolu s zabezpečení na úrovni řádků pro vytvoření aplikace s vysoce škálovatelné datové vrstvě v databázi SQL Azure, který podporuje víceklientské horizontálních oddílů."
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
ms.openlocfilehash: 73f1210b8d1f5ceca8fac9534d498bdc23d96d48
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="multi-tenant-applications-with-elastic-database-tools-and-row-level-security"></a><span data-ttu-id="84857-103">Víceklientské aplikací pomocí nástroje elastické databáze a zabezpečení na úrovni řádků</span><span class="sxs-lookup"><span data-stu-id="84857-103">Multi-tenant applications with elastic database tools and row-level security</span></span>
<span data-ttu-id="84857-104">[Nástroje elastické databáze](sql-database-elastic-scale-get-started.md) a [nízkoúrovňového zabezpečení (RLS)](https://msdn.microsoft.com/library/dn765131) nabízejí výkonnou sadu funkcí pro flexibilní a efektivní škálování datovou vrstvu víceklientské aplikace s Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="84857-104">[Elastic database tools](sql-database-elastic-scale-get-started.md) and [row-level security (RLS)](https://msdn.microsoft.com/library/dn765131) offer a powerful set of capabilities for flexibly and efficiently scaling the data tier of a multi-tenant application with Azure SQL Database.</span></span> <span data-ttu-id="84857-105">V tématu [návrhová schémata pro víceklientské aplikace SaaS ve službě Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="84857-105">See [Design Patterns for Multi-tenant SaaS Applications with Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) for more information.</span></span> 

<span data-ttu-id="84857-106">Tento článek ukazuje, jak použít tyto technologie společně k vytvoření aplikace s vysoce škálovatelné datové vrstvy, která podporuje více klientů horizontálních oddílů, pomocí **ADO.NET SqlClient** nebo **Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="84857-106">This article illustrates how to use these technologies together to build an application with a highly scalable data tier that supports multi-tenant shards, using **ADO.NET SqlClient** and/or **Entity Framework**.</span></span>  

* <span data-ttu-id="84857-107">**Nástroje elastické databáze** vývojářům umožňuje škálovat vrstvu data aplikace prostřednictvím horizontálního dělení standardní postupy, pomocí sady knihovny .NET a šablony služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="84857-107">**Elastic database tools** enables developers to scale out the data tier of an application via industry-standard sharding practices using a set of .NET libraries and Azure service templates.</span></span> <span data-ttu-id="84857-108">Správa horizontálních oddílů s použitím elastické databáze klientské knihovny pomáhá zautomatizovat a zjednodušit řadu infrastruktury úlohy, které jsou obvykle spojené s horizontálního dělení.</span><span class="sxs-lookup"><span data-stu-id="84857-108">Managing shards with using the Elastic Database Client Library helps automate and streamline many of the infrastructural tasks typically associated with sharding.</span></span> 
* <span data-ttu-id="84857-109">**Zabezpečení na úrovni řádků** vývojářům umožňuje ukládání dat pro více klientů ve stejné databázi pomocí zásad zabezpečení pomocí filtrů řádky, které nepatří do klienta provádění dotazu.</span><span class="sxs-lookup"><span data-stu-id="84857-109">**Row-level security** enables developers to store data for multiple tenants in the same database using security policies to filter out rows that do not belong to the tenant executing a query.</span></span> <span data-ttu-id="84857-110">Centralizuje logiku přístup s RLS v databázi, nikoli v aplikaci, zjednodušuje údržbu a snižuje riziko chyby, jako základu kódu aplikace zvětšování.</span><span class="sxs-lookup"><span data-stu-id="84857-110">Centralizing access logic with RLS inside the database, rather than in the application, simplifies maintenance and reduces the risk of error as an application’s codebase grows.</span></span> 

<span data-ttu-id="84857-111">Používání těchto funkcí společně, aplikace mohou těžit z výhod zvýšení úspory a efektivitu nákladů uložením dat pro více klientů ve stejné ID horizontálního oddílu databázi.</span><span class="sxs-lookup"><span data-stu-id="84857-111">Using these features together, an application can benefit from cost savings and efficiency gains by storing data for multiple tenants in the same shard database.</span></span> <span data-ttu-id="84857-112">Ve stejnou dobu má aplikace stále flexibilitu a nabídnout izolovaný, jednoho klienta horizontálních oddílů pro "premium" klienty, kteří vyžadují přísnější záruky výkonu vzhledem k tomu, že více klientů horizontálních oddílů není zaručeno distribuce rovna prostředků mezi klienty.</span><span class="sxs-lookup"><span data-stu-id="84857-112">At the same time, an application still has the flexibility to offer isolated, single-tenant shards for “premium” tenants who require stricter performance guarantees since multi-tenant shards do not guarantee equal resource distribution among tenants.</span></span>  

<span data-ttu-id="84857-113">Stručně řečeno, elastického databáze klientské knihovny [data závislé směrování](sql-database-elastic-scale-data-dependent-routing.md) rozhraní API pro klienty automaticky připojit k databázi správný horizontálních obsahující jejich horizontálního dělení klíč (obecně "TenantId").</span><span class="sxs-lookup"><span data-stu-id="84857-113">In short, the elastic database client library’s [data dependent routing](sql-database-elastic-scale-data-dependent-routing.md) APIs automatically connect tenants to the correct shard database containing their sharding key (generally a “TenantId”).</span></span> <span data-ttu-id="84857-114">Po připojení, zásady zabezpečení RLS v rámci databáze zajišťuje, aby klienti můžete pouze přístup k řádky, které obsahují jejich TenantId.</span><span class="sxs-lookup"><span data-stu-id="84857-114">Once connected, an RLS security policy within the database ensures that tenants can only access rows that contain their TenantId.</span></span> <span data-ttu-id="84857-115">Předpokládá se, že všechny tabulky obsahovat sloupec TenantId k označení, které řádky patří do každého klienta.</span><span class="sxs-lookup"><span data-stu-id="84857-115">It is assumed that all tables contain a TenantId column to indicate which rows belong to each tenant.</span></span> 

![Architektura aplikace blogu][1]

## <a name="download-the-sample-project"></a><span data-ttu-id="84857-117">Stáhněte si ukázkový projekt</span><span class="sxs-lookup"><span data-stu-id="84857-117">Download the sample project</span></span>
### <a name="prerequisites"></a><span data-ttu-id="84857-118">Požadavky</span><span class="sxs-lookup"><span data-stu-id="84857-118">Prerequisites</span></span>
* <span data-ttu-id="84857-119">Použijte sadu Visual Studio (2012 nebo vyšší)</span><span class="sxs-lookup"><span data-stu-id="84857-119">Use Visual Studio (2012 or higher)</span></span> 
* <span data-ttu-id="84857-120">Vytvořte tři databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="84857-120">Create three Azure SQL Databases</span></span> 
* <span data-ttu-id="84857-121">Stáhněte si ukázkový projekt: [elastické databáze nástroje pro Azure SQL - víceklientské horizontálních oddílů](http://go.microsoft.com/?linkid=9888163)</span><span class="sxs-lookup"><span data-stu-id="84857-121">Download sample project: [Elastic DB Tools for Azure SQL - Multi-Tenant Shards](http://go.microsoft.com/?linkid=9888163)</span></span>
  * <span data-ttu-id="84857-122">Zadejte informace pro vaše databáze na začátku **Program.cs**</span><span class="sxs-lookup"><span data-stu-id="84857-122">Fill in the information for your databases at the beginning of **Program.cs**</span></span> 

<span data-ttu-id="84857-123">Rozšiřuje popsané v tomto projektu [elastické databáze nástroje pro Azure SQL - Entity Framework integrace](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) přidáním podpory pro víc klientů horizontálních databáze.</span><span class="sxs-lookup"><span data-stu-id="84857-123">This project extends the one described in [Elastic DB Tools for Azure SQL - Entity Framework Integration](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) by adding support for multi-tenant shard databases.</span></span> <span data-ttu-id="84857-124">Sestavuje jednoduché konzolové aplikace pro vytvoření blogy a příspěvky, s čtyři klientů a dvě databáze víceklientské horizontálního oddílu, jak je ukázáno v diagramu.</span><span class="sxs-lookup"><span data-stu-id="84857-124">It builds a simple console application for creating blogs and posts, with four tenants and two multi-tenant shard databases as illustrated in the above diagram.</span></span> 

<span data-ttu-id="84857-125">Sestavte a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="84857-125">Build and run the application.</span></span> <span data-ttu-id="84857-126">Tato akce bootstrap správce mapy horizontálního oddílu nástroje elastické databáze a spustí následující testy:</span><span class="sxs-lookup"><span data-stu-id="84857-126">This will bootstrap the elastic database tools’ shard map manager and run the following tests:</span></span> 

1. <span data-ttu-id="84857-127">Pomocí rozhraní Entity Framework a LINQ, vytvořte nový blog a následně se zobrazí všechny blogy pro každého klienta</span><span class="sxs-lookup"><span data-stu-id="84857-127">Using Entity Framework and LINQ, create a new blog and then display all blogs for each tenant</span></span>
2. <span data-ttu-id="84857-128">Pomocí ADO.NET SqlClient, zobrazte všechny blogy pro klienta</span><span class="sxs-lookup"><span data-stu-id="84857-128">Using ADO.NET SqlClient, display all blogs for a tenant</span></span>
3. <span data-ttu-id="84857-129">Pokuste se vložit blogu pro chybný klienta k ověření, že je vyvolána chyba</span><span class="sxs-lookup"><span data-stu-id="84857-129">Try to insert a blog for the wrong tenant to verify that an error is thrown</span></span>  

<span data-ttu-id="84857-130">Všimněte si, že vzhledem k tomu, že RLS dosud nebyla spuštěna v databázích horizontálního oddílu, každý z těchto testů zjistí problém: Klienti mohou vidět blogy, který nepatří do nich a aplikace není zabránila vkládání blogu pro chybný klienta.</span><span class="sxs-lookup"><span data-stu-id="84857-130">Notice that because RLS has not yet been enabled in the shard databases, each of these tests reveals a problem: tenants are able to see blogs that do not belong to them, and the application is not prevented from inserting a blog for the wrong tenant.</span></span> <span data-ttu-id="84857-131">Zbývající část tohoto článku popisuje, jak vyřešit tyto problémy vynucování izolace klienta s RLS.</span><span class="sxs-lookup"><span data-stu-id="84857-131">The remainder of this article describes how to resolve these problems by enforcing tenant isolation with RLS.</span></span> <span data-ttu-id="84857-132">Existují dva kroky:</span><span class="sxs-lookup"><span data-stu-id="84857-132">There are two steps:</span></span> 

1. <span data-ttu-id="84857-133">**Aplikační vrstvě**: Upravit kód aplikace do vždy aktuální TenantId SESSION_CONTEXT po otevření připojení.</span><span class="sxs-lookup"><span data-stu-id="84857-133">**Application tier**: Modify the application code to always set the current TenantId in the SESSION_CONTEXT after opening a connection.</span></span> <span data-ttu-id="84857-134">Ukázkový projekt má neudělali.</span><span class="sxs-lookup"><span data-stu-id="84857-134">The sample project has already done this.</span></span> 
2. <span data-ttu-id="84857-135">**Datová vrstva**: vytvoření zásady zabezpečení RLS v každé databázi horizontálního oddílu filtrování řádků podle TenantId uložené v SESSION_CONTEXT.</span><span class="sxs-lookup"><span data-stu-id="84857-135">**Data tier**: Create an RLS security policy in each shard database to filter rows based on the TenantId stored in SESSION_CONTEXT.</span></span> <span data-ttu-id="84857-136">Budete muset provést pro každý z vašich databází horizontálního oddílu, jinak se nebudou filtrovat řádky v víceklientské horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="84857-136">You will need to do this for each of your shard databases, otherwise rows in multi-tenant shards will not be filtered.</span></span> 

## <a name="step-1-application-tier-set-tenantid-in-the-sessioncontext"></a><span data-ttu-id="84857-137">Krok 1) aplikační vrstvě: nastavte TenantId v SESSION_CONTEXT</span><span class="sxs-lookup"><span data-stu-id="84857-137">Step 1) Application tier: Set TenantId in the SESSION_CONTEXT</span></span>
<span data-ttu-id="84857-138">Po připojení k databázi horizontálního oddílu pomocí klientské knihovny elastické databáze dat rozhraní API pro směrování závislé, aplikace se musí zjistit databázi které TenantId používá toto připojení tak, aby zásady zabezpečení RLS můžete filtrovat řádky, které patří do ostatních klientů.</span><span class="sxs-lookup"><span data-stu-id="84857-138">After connecting to a shard database using the elastic database client library’s data dependent routing APIs, the application still needs to tell the database which TenantId is using that connection so that an RLS security policy can filter out rows belonging to other tenants.</span></span> <span data-ttu-id="84857-139">Je doporučeným způsobem předejte tyto informace uložit aktuální TenantId pro toto připojení v [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806.aspx).</span><span class="sxs-lookup"><span data-stu-id="84857-139">The recommended way to pass this information is to store the current TenantId for that connection in the [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806.aspx).</span></span> <span data-ttu-id="84857-140">(Poznámka: můžete alternativně použít [CONTEXT_INFO](https://msdn.microsoft.com/library/ms180125.aspx), ale SESSION_CONTEXT je lepší volbou protože je jednodušší použít, vrátí hodnotu NULL ve výchozím nastavení a podporuje páry klíč hodnota.)</span><span class="sxs-lookup"><span data-stu-id="84857-140">(Note: You could alternatively use [CONTEXT_INFO](https://msdn.microsoft.com/library/ms180125.aspx), but SESSION_CONTEXT is a better option because it is easier to use, returns NULL by default, and supports key-value pairs.)</span></span>

### <a name="entity-framework"></a><span data-ttu-id="84857-141">Entity Framework</span><span class="sxs-lookup"><span data-stu-id="84857-141">Entity Framework</span></span>
<span data-ttu-id="84857-142">Pro aplikace používající rozhraní Entity Framework, je nejjednodušší způsob nastavit SESSION_CONTEXT v rámci ElasticScaleContext přepsání popsané v [Data závislé směrování pomocí EF DbContext](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md#data-dependent-routing-using-ef-dbcontext).</span><span class="sxs-lookup"><span data-stu-id="84857-142">For applications using Entity Framework, the easiest approach is to set the SESSION_CONTEXT within the ElasticScaleContext override described in [Data Dependent Routing using EF DbContext](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md#data-dependent-routing-using-ef-dbcontext).</span></span> <span data-ttu-id="84857-143">Před vrácením připojení zprostředkované přes data závislé směrování, jednoduše vytvoření a provedení SqlCommand, který nastaví 'TenantId' v SESSION_CONTEXT k shardingKey zadaný pro toto připojení.</span><span class="sxs-lookup"><span data-stu-id="84857-143">Before returning the connection brokered through data dependent routing, simply create and execute a SqlCommand that sets 'TenantId' in the SESSION_CONTEXT to the shardingKey specified for that connection.</span></span> <span data-ttu-id="84857-144">Tímto způsobem, stačí jednou napsat kód, chcete-li nastavit SESSION_CONTEXT.</span><span class="sxs-lookup"><span data-stu-id="84857-144">This way, you only need to write code once to set the SESSION_CONTEXT.</span></span> 

```
// ElasticScaleContext.cs 
// ... 
// C'tor for data dependent routing. This call will open a validated connection routed to the proper 
// shard by the shard map manager. Note that the base class c'tor call will fail for an open connection 
// if migrations need to be done and SQL credentials are used. This is the reason for the  
// separation of c'tors into the DDR case (this c'tor) and the internal c'tor for new shards. 
public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
    : base(OpenDDRConnection(shardMap, shardingKey, connectionStr), true /* contextOwnsConnection */)
{
}

public static SqlConnection OpenDDRConnection(ShardMap shardMap, T shardingKey, string connectionStr)
{
    // No initialization
    Database.SetInitializer<ElasticScaleContext<T>>(null);

    // Ask shard map to broker a validated connection for the given key
    SqlConnection conn = null;
    try
    {
        conn = shardMap.OpenConnectionForKey(shardingKey, connectionStr, ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT to shardingKey to enable Row-Level Security filtering
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

<span data-ttu-id="84857-145">Nyní SESSION_CONTEXT automaticky nastavena zadaný TenantId vždy, když je volána ElasticScaleContext:</span><span class="sxs-lookup"><span data-stu-id="84857-145">Now the SESSION_CONTEXT is automatically set with the specified TenantId whenever ElasticScaleContext is invoked:</span></span> 

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

### <a name="adonet-sqlclient"></a><span data-ttu-id="84857-146">ADO.NET SqlClient</span><span class="sxs-lookup"><span data-stu-id="84857-146">ADO.NET SqlClient</span></span>
<span data-ttu-id="84857-147">Doporučený postup pro aplikace pomocí ADO.NET SqlClient, je vytvoření funkce obálku kolem ShardMap.OpenConnectionForKey(), který automaticky nastaví 'TenantId' v SESSION_CONTEXT na správné TenantId před vrácením připojení.</span><span class="sxs-lookup"><span data-stu-id="84857-147">For applications using ADO.NET SqlClient, the recommended approach is to create a wrapper function around ShardMap.OpenConnectionForKey() that automatically sets 'TenantId' in the SESSION_CONTEXT to the correct TenantId before returning a connection.</span></span> <span data-ttu-id="84857-148">Aby se zajistilo, že SESSION_CONTEXT je vždycky nastavený, spustíte pouze připojení pomocí této funkce obálku.</span><span class="sxs-lookup"><span data-stu-id="84857-148">To ensure that SESSION_CONTEXT is always set, you should only open connections using this wrapper function.</span></span>

```
// Program.cs
// ...

// Wrapper function for ShardMap.OpenConnectionForKey() that automatically sets SESSION_CONTEXT with the correct
// tenantId before returning a connection. As a best practice, you should only open connections using this 
// method to ensure that SESSION_CONTEXT is always set before executing a query.
public static SqlConnection OpenConnectionForTenant(ShardMap shardMap, int tenantId, string connectionStr)
{
    SqlConnection conn = null;
    try
    {
        // Ask shard map to broker a validated connection for the given key
        conn = shardMap.OpenConnectionForKey(tenantId, connectionStr, ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT to shardingKey to enable Row-Level Security filtering
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

## <a name="step-2-data-tier-create-row-level-security-policy"></a><span data-ttu-id="84857-149">Krok 2) Data vrstvy: vytvoření zásad zabezpečení na úrovni řádků</span><span class="sxs-lookup"><span data-stu-id="84857-149">Step 2) Data tier: Create row-level security policy</span></span>
### <a name="create-a-security-policy-to-filter-the-rows-each-tenant-can-access"></a><span data-ttu-id="84857-150">Vytvoření zásady zabezpečení pro filtrování řádky, které můžete přístup každý klient</span><span class="sxs-lookup"><span data-stu-id="84857-150">Create a security policy to filter the rows each tenant can access</span></span>
<span data-ttu-id="84857-151">Teď, když aplikace je před odesláním dotazu nastavení SESSION_CONTEXT s aktuální TenantId, zásady zabezpečení RLS můžete filtrovat dotazy a vyloučit řádky, které mají různé TenantId.</span><span class="sxs-lookup"><span data-stu-id="84857-151">Now that the application is setting SESSION_CONTEXT with the current TenantId before querying, an RLS security policy can filter queries and exclude rows that have a different TenantId.</span></span>  

<span data-ttu-id="84857-152">RLS je implementovaná v T-SQL: uživatelem definované funkce definuje logiku přístupu a zásady zabezpečení sváže tuto funkci Libovolný počet tabulek.</span><span class="sxs-lookup"><span data-stu-id="84857-152">RLS is implemented in T-SQL: a user-defined function defines the access logic, and a security policy binds this function to any number of tables.</span></span> <span data-ttu-id="84857-153">Pro tento projekt bude funkce jednoduše ověřte, zda aplikace (spíše než ostatní uživatele SQL) je připojen k databázi a že, TenantId, jsou uložena v SESSION_CONTEXT odpovídá TenantId daného řádku.</span><span class="sxs-lookup"><span data-stu-id="84857-153">For this project, the function will simply verify that the application (rather than some other SQL user) is connected to the database, and that the 'TenantId' stored in the SESSION_CONTEXT matches the TenantId of a given row.</span></span> <span data-ttu-id="84857-154">Predikát filtru vám umožní řádky, které splňují tyto podmínky pro ve filtru pro dotazy SELECT, UPDATE a DELETE; a predikát block zabrání řádků, které porušují tyto podmínky z vložené nebo aktualizované.</span><span class="sxs-lookup"><span data-stu-id="84857-154">A filter predicate will allow rows that meet these conditions to pass through the filter for SELECT, UPDATE, and DELETE queries; and a block predicate will prevent rows that violate these conditions from being INSERTed or UPDATEd.</span></span> <span data-ttu-id="84857-155">Pokud SESSION_CONTEXT nebyla nastavena, vrátí hodnotu NULL a žádné řádky budou viditelné či možné vložit.</span><span class="sxs-lookup"><span data-stu-id="84857-155">If SESSION_CONTEXT has not been set, it will return NULL and no rows will be visible or able to be inserted.</span></span> 

<span data-ttu-id="84857-156">Pokud chcete povolit RLS, spustit následující T-SQL na všechny horizontálních oddílů pomocí Visual Studio (SSDT), aplikace SSMS nebo ve skriptu PowerShell, který je zahrnutý v projektu (nebo pokud používáte [elastické databáze úlohy](sql-database-elastic-jobs-overview.md), které můžete použít k automatizaci provádění tohoto T-SQL na všechny horizontálních oddílů):</span><span class="sxs-lookup"><span data-stu-id="84857-156">To enable RLS, execute the following T-SQL on all shards using either Visual Studio (SSDT), SSMS, or the PowerShell script included in the project (or if you are using [Elastic Database Jobs](sql-database-elastic-jobs-overview.md), you can use it to automate execution of this T-SQL on all shards):</span></span> 

```
CREATE SCHEMA rls -- separate schema to organize RLS objects 
GO

CREATE FUNCTION rls.fn_tenantAccessPredicate(@TenantId int)     
    RETURNS TABLE     
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult          
        WHERE DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo') -- the user in your application’s connection string (dbo is only for demo purposes!)         
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
> <span data-ttu-id="84857-157">Pro složitější projekty, které je třeba přidat predikát na stovky tabulky můžete pomocná uložené procedury, která automaticky generuje zásadu zabezpečení přidání predikát ve všech tabulkách ve schématu.</span><span class="sxs-lookup"><span data-stu-id="84857-157">For more complex projects that need to add the predicate on hundreds of tables, you can use a helper stored procedure that automatically generates a security policy adding a predicate on all tables in a schema.</span></span> <span data-ttu-id="84857-158">V tématu [použití zabezpečení na úrovni řádků pro všechny tabulky – Pomocník skriptu (blog)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/03/31/apply-row-level-security-to-all-tables-helper-script).</span><span class="sxs-lookup"><span data-stu-id="84857-158">See [Apply Row-Level Security to all tables - helper script (blog)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/03/31/apply-row-level-security-to-all-tables-helper-script).</span></span>  
> 
> 

<span data-ttu-id="84857-159">Teď Pokud spustíte ukázkovou aplikaci znovu, klienti budou moci zobrazit pouze řádky, které patří k nim.</span><span class="sxs-lookup"><span data-stu-id="84857-159">Now if you run the sample application again, tenants will able to see only rows that belong to them.</span></span> <span data-ttu-id="84857-160">Kromě toho aplikace nelze vložit řádky, které patří do klientů jiného, než jaké aktuálně připojené k databázi horizontálního oddílu a nemůže se aktualizovat viditelné řádky, které mají různé TenantId.</span><span class="sxs-lookup"><span data-stu-id="84857-160">In addition, the application cannot insert rows that belong to tenants other than the one currently connected to the shard database, and it cannot update visible rows to have a different TenantId.</span></span> <span data-ttu-id="84857-161">Pokud se aplikace pokusí udělat, DbUpdateException, bude vyvolána.</span><span class="sxs-lookup"><span data-stu-id="84857-161">If the application attempts to do either, a DbUpdateException will be raised.</span></span>

<span data-ttu-id="84857-162">Pokud později na přidáte nové tabulky, jednoduše změnit zásady zabezpečení a přidejte filtr a blokovat predikáty v nové tabulce:</span><span class="sxs-lookup"><span data-stu-id="84857-162">If you add a new table later on, simply ALTER the security policy and add filter and block predicates on the new table:</span></span> 

```
ALTER SECURITY POLICY rls.tenantAccessPolicy     
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable
GO 
```

### <a name="add-default-constraints-to-automatically-populate-tenantid-for-inserts"></a><span data-ttu-id="84857-163">Přidat výchozí omezení automatické vyplnění TenantId pro vložení</span><span class="sxs-lookup"><span data-stu-id="84857-163">Add default constraints to automatically populate TenantId for INSERTs</span></span>
<span data-ttu-id="84857-164">Můžete vložit výchozího omezení na každou tabulku automaticky naplnit hodnoty TenantId s hodnotou, které jsou aktuálně uloženy ve SESSION_CONTEXT při vložení řádků.</span><span class="sxs-lookup"><span data-stu-id="84857-164">You can put a default constraint on each table to automatically populate the TenantId with the value currently stored in SESSION_CONTEXT when inserting rows.</span></span> <span data-ttu-id="84857-165">Například:</span><span class="sxs-lookup"><span data-stu-id="84857-165">For example:</span></span> 

```
-- Create default constraints to auto-populate TenantId with the value of SESSION_CONTEXT for inserts 
ALTER TABLE Blogs     
    ADD CONSTRAINT df_TenantId_Blogs      
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId 
GO

ALTER TABLE Posts     
    ADD CONSTRAINT df_TenantId_Posts      
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId 
GO 
```

<span data-ttu-id="84857-166">Aplikaci teď není nutné zadávat TenantId při vložení řádků:</span><span class="sxs-lookup"><span data-stu-id="84857-166">Now the application does not need to specify a TenantId when inserting rows:</span></span> 

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
> <span data-ttu-id="84857-167">Pokud používáte výchozí omezení pro projekt Entity Framework, se doporučuje nezahrnujte do datového modelu EF sloupci TenantId.</span><span class="sxs-lookup"><span data-stu-id="84857-167">If you use default constraints for an Entity Framework project, it is recommended that you do NOT include the TenantId column in your EF data model.</span></span> <span data-ttu-id="84857-168">Je to proto, že automaticky zadat výchozí hodnoty, které přepíší výchozí omezení vytvořené v T-SQL, které používají SESSION_CONTEXT dotazy na Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="84857-168">This is because Entity Framework queries automatically supply default values which will override the default constraints created in T-SQL that use SESSION_CONTEXT.</span></span> <span data-ttu-id="84857-169">V projektu vzorku použít výchozí omezení, například doporučujeme odebrat TenantId z DataClasses.cs (a spuštění Add-Migration v konzole Správce balíčků) a zkontrolujte, zda pole existuje pouze v tabulkách databáze pomocí T-SQL.</span><span class="sxs-lookup"><span data-stu-id="84857-169">To use default constraints in the sample project, for instance, you should remove TenantId from DataClasses.cs (and run Add-Migration in the Package Manager Console) and use T-SQL to ensure that the field only exists in the database tables.</span></span> <span data-ttu-id="84857-170">Tímto způsobem EF nebude zadat automaticky nesprávný výchozí hodnoty při vkládání dat.</span><span class="sxs-lookup"><span data-stu-id="84857-170">This way, EF will not automatically supply incorrect default values when inserting data.</span></span> 
> 
> 

### <a name="optional-enable-a-superuser-to-access-all-rows"></a><span data-ttu-id="84857-171">(Volitelné) Povolit "superuživatele" pro přístup k všechny řádky</span><span class="sxs-lookup"><span data-stu-id="84857-171">(Optional) Enable a "superuser" to access all rows</span></span>
<span data-ttu-id="84857-172">Některé aplikace může chtít vytvořit "superuživatele" kdo má přístup všechny řádky, například za účelem povolení generování sestav mezi všechny klienty na všechny horizontálních oddílů, nebo k provádění operací rozdělení či sloučení na horizontálních oddílů, které vyžadují přesunutí řádků klienta mezi databázemi.</span><span class="sxs-lookup"><span data-stu-id="84857-172">Some applications may want to create a "superuser" who can access all rows, for instance, in order to enable reporting across all tenants on all shards, or to perform Split/Merge operations on shards that involve moving tenant rows between databases.</span></span> <span data-ttu-id="84857-173">Chcete-li povolit, měli byste vytvořit nového uživatele SQL ("superuživatel" v tomto příkladu) v každé databázi horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="84857-173">To enable this, you should create a new SQL user ("superuser" in this example) in each shard database.</span></span> <span data-ttu-id="84857-174">Potom změňte zásady zabezpečení s novou predikátem funkci, která umožňuje tento uživatel pro přístup k všechny řádky:</span><span class="sxs-lookup"><span data-stu-id="84857-174">Then alter the security policy with a new predicate function that allows this user to access all rows:</span></span>

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

-- Atomically swap in the new predicate function on each table
ALTER SECURITY POLICY rls.tenantAccessPolicy
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER BLOCK PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts,
    ALTER BLOCK PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts
GO
```


### <a name="maintenance"></a><span data-ttu-id="84857-175">Údržby</span><span class="sxs-lookup"><span data-stu-id="84857-175">Maintenance</span></span>
* <span data-ttu-id="84857-176">**Přidání nové horizontálních oddílů**: je třeba spustit příkaz skriptu T-SQL povolit RLS na všechny nové horizontálních oddílů, v opačném případě se nebudou filtrovat dotazy na tyto horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="84857-176">**Adding new shards**: You must execute the T-SQL script to enable RLS on any new shards, otherwise queries on these shards will not be filtered.</span></span>
* <span data-ttu-id="84857-177">**Přidání nové tabulky**: Predikát filtru a zároveň se zablokují musíte přidat do nastavení zásad zabezpečení na všechny horizontálních oddílů vždy, když se vytvoří nové tabulky, v opačném případě se nebudou filtrovat dotazy na novou tabulku.</span><span class="sxs-lookup"><span data-stu-id="84857-177">**Adding new tables**: You must add a filter and block predicate to the security policy on all shards whenever a new table is created, otherwise queries on the new table will not be filtered.</span></span> <span data-ttu-id="84857-178">To je možné automatizovat pomocí aktivační událost jazyka DDL, jak je popsáno v [použití zabezpečení na úrovni řádků automaticky do nově vytvořené tabulky (blog)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/22/apply-row-level-security-automatically-to-newly-created-tables.aspx).</span><span class="sxs-lookup"><span data-stu-id="84857-178">This can be automated using a DDL trigger, as described in [Apply Row-Level Security automatically to newly created tables (blog)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/22/apply-row-level-security-automatically-to-newly-created-tables.aspx).</span></span>

## <a name="summary"></a><span data-ttu-id="84857-179">Souhrn</span><span class="sxs-lookup"><span data-stu-id="84857-179">Summary</span></span>
<span data-ttu-id="84857-180">Nástroje elastické databáze a zabezpečení na úrovni řádků může být společně slouží jako škálování aplikace datové vrstvy s podporou pro oba více klientů a jednoho klienta horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="84857-180">Elastic database tools and row-level security can be used together to scale out an application’s data tier with support for both multi-tenant and single-tenant shards.</span></span> <span data-ttu-id="84857-181">Víceklientské horizontálních oddílů lze použít k ukládání dat efektivněji (obzvláště v případech, kdy velký počet klientů, které mají jenom pár řádků dat), při jednoho klienta horizontálních oddílů lze použít pro podporu klientů premium s přísnější výkon a izolace požadavky.</span><span class="sxs-lookup"><span data-stu-id="84857-181">Multi-tenant shards can be used to store data more efficiently (particularly in cases where a large number of tenants have only a few rows of data), while single-tenant shards can be used to support premium tenants with stricter performance and isolation requirements.</span></span>  <span data-ttu-id="84857-182">Další informace najdete v tématu [zabezpečení na úrovni řádků odkaz](https://msdn.microsoft.com/library/dn765131).</span><span class="sxs-lookup"><span data-stu-id="84857-182">For more information, see [Row-Level Security reference](https://msdn.microsoft.com/library/dn765131).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="84857-183">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="84857-183">Additional resources</span></span>
* [<span data-ttu-id="84857-184">Co je Azure elastickém fondu?</span><span class="sxs-lookup"><span data-stu-id="84857-184">What is an Azure elastic pool?</span></span>](sql-database-elastic-pool.md)
* [<span data-ttu-id="84857-185">Horizontální navýšení kapacity s Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="84857-185">Scaling out with Azure SQL Database</span></span>](sql-database-elastic-scale-introduction.md)
* [<span data-ttu-id="84857-186">Vzory návrhu pro aplikace SaaS s více tenanty využívající Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="84857-186">Design Patterns for Multi-tenant SaaS Applications with Azure SQL Database</span></span>](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [<span data-ttu-id="84857-187">Ověřování ve víceklientských aplikacích s využitím Azure AD a OpenID Connect</span><span class="sxs-lookup"><span data-stu-id="84857-187">Authentication in multitenant apps, using Azure AD and OpenID Connect</span></span>](../guidance/guidance-multitenant-identity-authenticate.md)
* [<span data-ttu-id="84857-188">Aplikace Tailspin Surveys</span><span class="sxs-lookup"><span data-stu-id="84857-188">Tailspin Surveys application</span></span>](../guidance/guidance-multitenant-identity-tailspin.md)

## <a name="questions-and-feature-requests"></a><span data-ttu-id="84857-189">Otázky a žádosti o funkce</span><span class="sxs-lookup"><span data-stu-id="84857-189">Questions and Feature Requests</span></span>
<span data-ttu-id="84857-190">Máte dotazy, kontaktujte nás na [fórum SQL Database](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) a pro žádosti o funkce, přidejte je do [fóru pro zpětnou vazbu SQL Database](https://feedback.azure.com/forums/217321-sql-database/).</span><span class="sxs-lookup"><span data-stu-id="84857-190">For questions, please reach out to us on the [SQL Database forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) and for feature requests, please add them to the [SQL Database feedback forum](https://feedback.azure.com/forums/217321-sql-database/).</span></span>

<!--Image references-->
[1]: ./media/sql-database-elastic-tools-multi-tenant-row-level-security/blogging-app.png
<!--anchors-->


