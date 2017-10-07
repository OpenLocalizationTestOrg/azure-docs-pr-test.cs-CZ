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
# <a name="multi-tenant-applications-with-elastic-database-tools-and-row-level-security"></a>Víceklientské aplikací pomocí nástroje elastické databáze a zabezpečení na úrovni řádků
[Nástroje elastické databáze](sql-database-elastic-scale-get-started.md) a [nízkoúrovňového zabezpečení (RLS)](https://msdn.microsoft.com/library/dn765131) nabízejí výkonnou sadu funkcí pro flexibilní a efektivní škálování hello datové vrstvy víceklientské aplikace s Azure SQL Database. V tématu [návrhová schémata pro víceklientské aplikace SaaS ve službě Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) Další informace. 

Tento článek ukazuje, jak toouse společně toobuild aplikace s vysoce škálovatelné datové vrstvy, která podporuje více klientů horizontálních oddílů, pomocí těchto technologií **ADO.NET SqlClient** nebo **Entity Framework**.  

* **Nástroje elastické databáze** umožňuje vývojářům tooscale na datové vrstvě hello aplikace prostřednictvím horizontálního dělení standardní postupy, pomocí sady knihovny .NET a šablony služeb Azure. Správa horizontálních oddílů s použitím hello klientské knihovny pro elastické databáze pomáhá zautomatizovat a zjednodušit řadu hello infrastruktury úloh obvykle přidružených horizontálního dělení. 
* **Zabezpečení na úrovni řádků** umožňuje vývojářům toostore data pro více klienty ve stejné databázi pomocí toofilter zásad zabezpečení na řádky, které nejsou součástí klienta toohello provádění dotazu hello. Centralizuje logiku přístup s RLS uvnitř databáze hello spíše než v aplikaci hello zjednodušuje údržbu a snižuje riziko hello chyby jako základu kódu aplikace zvětšování. 

Použití těchto funkcí společně, aplikace, můžete využít zvýšení úspory a efektivitu nákladů uložením dat pro více klienty ve hello stejné ID horizontálního oddílu databáze. Na hello má stejnou dobu, stále aplikace hello flexibilitu toooffer izolované, jednoho klienta horizontálních oddílů pro "premium" klienty, kteří vyžadují přísnější záruky výkonu vzhledem k tomu, že více klientů horizontálních oddílů není zaručeno distribuce rovna prostředků mezi klienty.  

Stručně řečeno, hello elastické databáze klientské knihovny [data závislé směrování](sql-database-elastic-scale-data-dependent-routing.md) rozhraní API automaticky připojit klienty toohello správné horizontálních databáze obsahující jejich horizontálního dělení klíč (obecně "TenantId"). Po připojení, zásady zabezpečení RLS v rámci databáze hello zajišťuje, aby klienti můžete pouze přístup k řádky, které obsahují jejich TenantId. Předpokládá se, že všechny tabulky obsahují tooindicate sloupce TenantId řádky patří tooeach klienta. 

![Architektura aplikace blogu][1]

## <a name="download-hello-sample-project"></a>Stáhněte si ukázkový projekt hello
### <a name="prerequisites"></a>Požadavky
* Použijte sadu Visual Studio (2012 nebo vyšší) 
* Vytvořte tři databáze SQL Azure 
* Stáhněte si ukázkový projekt: [elastické databáze nástroje pro Azure SQL - víceklientské horizontálních oddílů](http://go.microsoft.com/?linkid=9888163)
  * Vyplnit hello informace pro vaše databáze na začátku hello **Program.cs** 

Rozšiřuje hello jeden popsané v tomto projektu [elastické databáze nástroje pro Azure SQL - Entity Framework integrace](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) přidáním podpory pro víc klientů horizontálních databáze. Sestavuje pro vytváření blogy a příspěvky, s čtyři klientů a dvě databáze víceklientské horizontálních, jak je ukázáno v hello výše diagram jednoduché konzolové aplikace. 

Sestavení a spuštění aplikace hello. To bude bootstrap nástroje elastické databáze hello horizontálního oddílu mapa správce a spusťte hello následující testy: 

1. Pomocí rozhraní Entity Framework a LINQ, vytvořte nový blog a následně se zobrazí všechny blogy pro každého klienta
2. Pomocí ADO.NET SqlClient, zobrazte všechny blogy pro klienta
3. Zkuste tooinsert blog pro tooverify hello nesprávný klienta, která je vyvolána chyba  

Všimněte si, že protože RLS dosud nebyla spuštěna v databázích hello horizontálního oddílu, každý z těchto testů zjistí problém: klientů jsou možné toosee blogy, který nepatří toothem a aplikace hello není mohou vkládání blogu pro hello nesprávný klienta. Hello zbývající část tohoto článku popisuje, jak tooresolve tyto problémy vynucením klienta izolace s RLS. Existují dva kroky: 

1. **Aplikační vrstvě**: Upravit kód aplikace hello tooalways sadu hello aktuální TenantId v hello SESSION_CONTEXT po otevření připojení. Ukázkový projekt Hello již bylo dokončeno to. 
2. **Datová vrstva**: vytvoření zásady zabezpečení RLS v každé toofilter databáze horizontálního oddílu řádků v závislosti na hello TenantId uložené v SESSION_CONTEXT. Budete potřebovat toodo pro každou databází horizontálního oddílu, jinak se nebudou filtrovat řádky v víceklientské horizontálních oddílů. 

## <a name="step-1-application-tier-set-tenantid-in-hello-sessioncontext"></a>Krok 1) aplikační vrstvě: nastavte TenantId v hello SESSION_CONTEXT
Po připojování tooa horizontálního oddílu databázi pomocí hello elastické databáze klientské knihovny pro data, která je stále rozhraní API pro směrování závislé, aplikace hello musí databáze hello tootell které TenantId používá toto připojení tak, aby zásady zabezpečení RLS můžete filtrovat řádky patřící tooother klienty. Hello doporučeným způsobem toopass tyto informace jsou toostore hello aktuální TenantId pro toto připojení v hello [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806.aspx). (Poznámka: můžete alternativně použít [CONTEXT_INFO](https://msdn.microsoft.com/library/ms180125.aspx), ale SESSION_CONTEXT je lepší volbou, protože je jednodušší toouse, vrátí hodnotu NULL ve výchozím nastavení a podporuje páry klíč hodnota.)

### <a name="entity-framework"></a>Entity Framework
Pro aplikace používající rozhraní Entity Framework hello nejjednodušší způsob je tooset hello SESSION_CONTEXT v rámci hello ElasticScaleContext přepsat popsané v [Data závislé směrování pomocí EF DbContext](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md#data-dependent-routing-using-ef-dbcontext). Před vrácením hello připojení zprostředkované přes data závislé směrování, jednoduše vytvoření a provedení SqlCommand, který nastaví 'TenantId' v hello SESSION_CONTEXT toohello shardingKey zadaný pro toto připojení. Tímto způsobem, stačí toowrite kódu po tooset hello SESSION_CONTEXT. 

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

Teď hello SESSION_CONTEXT bude automaticky nastavena pomocí zadané hello TenantId vždy, když je volána ElasticScaleContext: 

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

### <a name="adonet-sqlclient"></a>ADO.NET SqlClient
Pro aplikace pomocí ADO.NET SqlClient hello doporučený postup je toocreate funkce obálku kolem ShardMap.OpenConnectionForKey(), který automaticky nastaví 'TenantId' v hello SESSION_CONTEXT toohello opravte TenantId před vrácením připojení. tooensure, který je vždycky nastavený SESSION_CONTEXT, by měla pouze otevřít připojení pomocí této funkce obálku.

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

## <a name="step-2-data-tier-create-row-level-security-policy"></a>Krok 2) Data vrstvy: vytvoření zásad zabezpečení na úrovni řádků
### <a name="create-a-security-policy-toofilter-hello-rows-each-tenant-can-access"></a>Vytvoření hello toofilter zásady zabezpečení řádky, které můžete přístup každý klient
Teď, když je aplikace hello nastavení SESSION_CONTEXT s hello aktuální TenantId před odesláním dotazu, můžete zásadu zabezpečení RLS filtrovat dotazy a vyloučit řádky, které mají různé TenantId.  

RLS je implementovaná v T-SQL: uživatelem definované funkce definuje logiku hello přístupu a zásady zabezpečení sváže této funkce tooany počet tabulek. Pro tento projekt bude funkce hello jednoduše ověřte, zda hello aplikaci (namísto jiný uživatel SQL) je připojený toohello databáze a že hello 'TenantId' uložené v hello SESSION_CONTEXT odpovídá hello TenantId daného řádku. Predikát filtru vám umožní řádky, které splňují tyto podmínky toopass prostřednictvím hello filtru pro dotazy SELECT, UPDATE a DELETE; a predikát block zabrání řádků, které porušují tyto podmínky z vložené nebo aktualizované. Pokud SESSION_CONTEXT nebyla nastavena, vrátí hodnotu NULL a žádné řádky bude toobe viditelný nebo možnost vložit. 

tooenable RLS, proveďte následující T-SQL na všechny horizontálních oddílů pomocí buď Visual Studio (SSDT), aplikace SSMS, hello nebo hello skript prostředí PowerShell, které jsou součástí projektu hello (nebo pokud používáte [elastické databáze úlohy](sql-database-elastic-jobs-overview.md), můžete ho tooautomate provádění Tato T-SQL na všechny horizontálních oddílů): 

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
> Pro složitější projekty, které je třeba tooadd hello predikát stovky tabulky můžete pomocná uložené procedury, která automaticky generuje zásadu zabezpečení přidání predikát ve všech tabulkách ve schématu. V tématu [použití zabezpečení na úrovni řádků tabulky tooall – Pomocník skriptu (blog)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/03/31/apply-row-level-security-to-all-tables-helper-script).  
> 
> 

Teď Pokud spustíte znovu hello ukázkovou aplikaci, klienti budou moct toosee pouze řádky, které patří toothem. Kromě toho aplikace hello nejdou vložit řádky, které patří tootenants než hello jeden aktuálně připojené toohello horizontálního oddílu databáze a nemůže se aktualizovat viditelné řádky toohave různých TenantId. Pokud aplikace hello pokusí toodo buď, DbUpdateException, bude vyvolána.

Pokud později na přidáte nové tabulky, jednoduše ALTER hello zásady zabezpečení a přidejte filtr a zároveň se zablokují predikáty v nové tabulce hello: 

```
ALTER SECURITY POLICY rls.tenantAccessPolicy     
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable
GO 
```

### <a name="add-default-constraints-tooautomatically-populate-tenantid-for-inserts"></a>Přidat výchozí omezení tooautomatically naplnit TenantId pro vložení
Můžete vložit výchozí omezení pro každou tabulku tooautomatically naplnit hello TenantId s hello hodnoty, které jsou aktuálně uloženy ve SESSION_CONTEXT při vložení řádků. Například: 

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

Nyní aplikace hello nepotřebuje toospecify TenantId při vložení řádků: 

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
> Pokud používáte výchozí omezení pro projekt Entity Framework, se doporučuje nezahrnujte do datového modelu EF hello TenantId sloupce. Je to proto, že automaticky zadat výchozí hodnoty, které přepíší hello výchozí omezení vytvořené v T-SQL používající SESSION_CONTEXT dotazy na Entity Framework. výchozí omezení toouse v hello ukázkový projekt, například TenantId byste měli odebrat z DataClasses.cs (a spuštění Add-Migration v hello Konzola správce balíčků) a tooensure použití T-SQL, který hello pole existuje pouze v tabulkách databáze hello. Tímto způsobem EF nebude zadat automaticky nesprávný výchozí hodnoty při vkládání dat. 
> 
> 

### <a name="optional-enable-a-superuser-tooaccess-all-rows"></a>(Volitelné) Povolit "superuživatel" tooaccess všechny řádky
Některé aplikace může být vhodné toocreate "superuživatele" kdo má přístup všechny řádky, například v tooenable pořadí vytváření sestav v rámci všech klientů na všech horizontálních oddílů nebo tooperform rozdělení či sloučení operací na horizontálních oddílů, které zahrnují přesunutí řádků klienta mezi databázemi. tooenable, měli byste vytvořit nového uživatele SQL ("superuživatel" v tomto příkladu) v každé databázi horizontálního oddílu. Potom změňte zásady zabezpečení hello s novou predikátem funkci, která umožňuje tento uživatel tooaccess všechny řádky:

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


### <a name="maintenance"></a>Údržby
* **Přidání nové horizontálních oddílů**: hello T-SQL skriptu tooenable RLS je třeba spustit na jakékoli nové horizontálních oddílů, v opačném případě se nebudou filtrovat dotazy na tyto horizontálních oddílů.
* **Přidání nové tabulky**: je nutné přidat filtr a blokovat predikátem toohello zásad zabezpečení na všechny horizontálních oddílů vždy, když se vytvoří nové tabulky, v opačném případě se nebudou filtrovat dotazy na novou tabulku hello. To je možné automatizovat pomocí aktivační událost jazyka DDL, jak je popsáno v [použití zabezpečení na úrovni řádků automaticky toonewly vytvořené tabulky (blog)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/22/apply-row-level-security-automatically-to-newly-created-tables.aspx).

## <a name="summary"></a>Souhrn
Nástroje elastické databáze a zabezpečení na úrovni řádků může být použité tooscale společně se aplikace datové vrstvy s podporou pro více klientů a jednoho klienta horizontálních oddílů. Víceklientské horizontálních oddílů lze použít toostore data efektivněji (obzvláště v případech, kdy velký počet klientů, které mají jenom pár řádků dat), při jednoho klienta horizontálních oddílů lze použít toosupport premium klientů s přísnější výkon a izolace požadavky.  Další informace najdete v tématu [zabezpečení na úrovni řádků odkaz](https://msdn.microsoft.com/library/dn765131). 

## <a name="additional-resources"></a>Další zdroje
* [Co je Azure elastickém fondu?](sql-database-elastic-pool.md)
* [Horizontální navýšení kapacity s Azure SQL Database](sql-database-elastic-scale-introduction.md)
* [Vzory návrhu pro aplikace SaaS s více tenanty využívající Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Ověřování ve víceklientských aplikacích s využitím Azure AD a OpenID Connect](../guidance/guidance-multitenant-identity-authenticate.md)
* [Aplikace Tailspin Surveys](../guidance/guidance-multitenant-identity-tailspin.md)

## <a name="questions-and-feature-requests"></a>Otázky a žádosti o funkce
Máte dotazy, kontaktujte toous na hello [SQL Database fórum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) a pro žádosti o funkce, přidejte je toohello [fóru pro zpětnou vazbu SQL Database](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-tools-multi-tenant-row-level-security/blogging-app.png
<!--anchors-->


