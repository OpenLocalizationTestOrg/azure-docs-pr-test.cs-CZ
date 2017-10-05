---
title: "Kurz: Webové aplikace s více klienty databázi pomocí Entity Framework a zabezpečení na úrovni řádků"
description: "Další informace jak vyvíjet webové aplikace ASP.NET MVC 5 s více klienty backent, SQL Database pomocí rozhraní Entity Framework a zabezpečení na úrovni řádků."
metakeywords: azure asp.net mvc entity framework multi tenant row level security rls sql database
services: app-service\web
documentationcenter: .net
manager: jeffreyg
author: tmullaney
ms.assetid: 8fdc47a5-6fc3-4d29-ab6a-33e79f50699f
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/25/2016
ms.author: thmullan
ms.openlocfilehash: ba1bb3d84b462dfebbb2564569517d7336bf54fd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-web-app-with-a-multi-tenant-database-using-entity-framework-and-row-level-security"></a><span data-ttu-id="c00ff-103">Kurz: Webové aplikace s více klienty databázi pomocí Entity Framework a zabezpečení na úrovni řádků</span><span class="sxs-lookup"><span data-stu-id="c00ff-103">Tutorial: Web app with a multi-tenant database using Entity Framework and Row-Level Security</span></span>
<span data-ttu-id="c00ff-104">Tento kurz ukazuje, jak sestavit víceklientské webové aplikace s "[sdílenou databázi, sdílené schématu](https://msdn.microsoft.com/library/aa479086.aspx)" klientů modelu s použitím rozhraní Entity Framework a [zabezpečení na úrovni řádků](https://msdn.microsoft.com/library/dn765131.aspx).</span><span class="sxs-lookup"><span data-stu-id="c00ff-104">This tutorial shows how to build a multi-tenant web app with a "[shared database, shared schema](https://msdn.microsoft.com/library/aa479086.aspx)" tenancy model, using Entity Framework and [Row-Level Security](https://msdn.microsoft.com/library/dn765131.aspx).</span></span> <span data-ttu-id="c00ff-105">V tomto modelu jedné databáze obsahuje data pro velký počet klientů každý řádek v každé tabulce souvisí s "Klienta ID"</span><span class="sxs-lookup"><span data-stu-id="c00ff-105">In this model, a single database contains data for many tenants, and each row in each table is associated with a "Tenant ID."</span></span> <span data-ttu-id="c00ff-106">Zabránit klientům v přístupu k výměně dat se používá nízkoúrovňového zabezpečení (RLS), novou funkci pro databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="c00ff-106">Row-Level Security (RLS), a new feature for Azure SQL Database, is used to prevent tenants from accessing each other's data.</span></span> <span data-ttu-id="c00ff-107">To vyžaduje pouze jediné, malé změny do aplikace.</span><span class="sxs-lookup"><span data-stu-id="c00ff-107">This requires just a single, small change to the application.</span></span> <span data-ttu-id="c00ff-108">Centralizací logiky přístupu klienta v rámci samotná databáze RLS zjednodušuje kódu aplikace a snižuje riziko úniku dat náhodných mezi klienty.</span><span class="sxs-lookup"><span data-stu-id="c00ff-108">By centralizing the tenant access logic within the database itself, RLS simplifies the application code and reduces the risk of accidental data leakage between tenants.</span></span>

<span data-ttu-id="c00ff-109">Začněme s jednoduchou aplikaci obraťte se na správce z [vytvoření aplikace ASP.NET MVP pomocí ověřování a databázi SQL a nasazení do Azure App Service](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="c00ff-109">Let's start with the simple Contact Manager application from [Create an ASP.NET MVP app with auth and SQL DB and deploy to Azure App Service](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).</span></span> <span data-ttu-id="c00ff-110">Právo nyní aplikace umožňuje všem uživatelům (klienty) Chcete-li zobrazit všechny kontakty:</span><span class="sxs-lookup"><span data-stu-id="c00ff-110">Right now, the application allows all users (tenants) to see all contacts:</span></span>

![Obraťte se na správce aplikace před povolením RLS](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-Before.png)

<span data-ttu-id="c00ff-112">S několika menší změny přidáme podporu pro víceklientskou architekturu, tak, aby se uživatelé moct prohlížet pouze kontakty, které patří k nim.</span><span class="sxs-lookup"><span data-stu-id="c00ff-112">With just a few small changes, we will add support for multi-tenancy, so that users are able to see only the contacts that belong to them.</span></span>

## <a name="step-1-add-an-interceptor-class-in-the-application-to-set-the-sessioncontext"></a><span data-ttu-id="c00ff-113">Krok 1: Přidání třídu interceptoru v aplikaci nastavení SESSION_CONTEXT</span><span class="sxs-lookup"><span data-stu-id="c00ff-113">Step 1: Add an Interceptor class in the application to set the SESSION_CONTEXT</span></span>
<span data-ttu-id="c00ff-114">Neexistuje jeden změny aplikace, které je třeba mít.</span><span class="sxs-lookup"><span data-stu-id="c00ff-114">There is one application change we need to make.</span></span> <span data-ttu-id="c00ff-115">Vzhledem k tomu, že všichni uživatelé aplikací připojit k databázi pomocí stejný připojovací řetězec (tj. stejné přihlašovací údaje SQL), se aktuálně nijak pro zásadu RLS vědět, kteří uživatelé měli filtrovat.</span><span class="sxs-lookup"><span data-stu-id="c00ff-115">Because all application users connect to the database using the same connection string (i.e. same SQL login), there is currently no way for an RLS policy to know which user it should filter for.</span></span> <span data-ttu-id="c00ff-116">Tento přístup je velmi běžné u webových aplikací, protože umožňuje efektivní sdružování připojení, ale znamená, že potřebujeme jiný způsob, jak identifikovat aktuální uživatel aplikace v rámci databáze.</span><span class="sxs-lookup"><span data-stu-id="c00ff-116">This approach is very common in web applications because it enables efficient connection pooling, but it means we need another way to identify the current application user within the database.</span></span> <span data-ttu-id="c00ff-117">Řešení je nastavit pár klíč hodnota pro aktuální ID uživatele v aplikaci [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) okamžitě po otevření připojení, než se provede žádné dotazy.</span><span class="sxs-lookup"><span data-stu-id="c00ff-117">The solution is to have the application set a key-value pair for the current UserId in the [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) immediately after opening a connection, before it executes any queries.</span></span> <span data-ttu-id="c00ff-118">Úložiště dvojic klíč hodnota s rozsahem relace je SESSION_CONTEXT a naše zásady RLS bude používat ID uživatele v ní uloženy k identifikaci aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="c00ff-118">SESSION_CONTEXT is a session-scoped key-value store, and our RLS policy will use the UserId stored in it to identify the current user.</span></span>

<span data-ttu-id="c00ff-119">Přidáme [interceptoru](https://msdn.microsoft.com/data/dn469464.aspx) (konkrétně [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), nová funkce v Framework Entity (EF) 6, umožňuje automaticky nastavit aktuální ID uživatele v SESSION_CONTEXT spuštěním příkazu T-SQL vždy, když EF otevře připojení.</span><span class="sxs-lookup"><span data-stu-id="c00ff-119">We will add an [interceptor](https://msdn.microsoft.com/data/dn469464.aspx) (in particular, a [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), a new feature in Entity Framework (EF) 6, to automatically set the current UserId in the SESSION_CONTEXT by executing a T-SQL statement whenever EF opens a connection.</span></span>

1. <span data-ttu-id="c00ff-120">Otevřete projekt ContactManager v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c00ff-120">Open the ContactManager project in Visual Studio.</span></span>
2. <span data-ttu-id="c00ff-121">Klikněte pravým tlačítkem na složku modely v Průzkumníku řešení a zvolte možnost Přidat > třída.</span><span class="sxs-lookup"><span data-stu-id="c00ff-121">Right-click on the Models folder in the Solution Explorer, and choose Add > Class.</span></span>
3. <span data-ttu-id="c00ff-122">Pojmenujte novou třídu "SessionContextInterceptor.cs" a klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="c00ff-122">Name the new class "SessionContextInterceptor.cs" and click Add.</span></span>
4. <span data-ttu-id="c00ff-123">Obsah SessionContextInterceptor.cs nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="c00ff-123">Replace the contents of SessionContextInterceptor.cs with the following code.</span></span>

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Data.Common;
using System.Data.Entity;
using System.Data.Entity.Infrastructure.Interception;
using Microsoft.AspNet.Identity;

namespace ContactManager.Models
{
    public class SessionContextInterceptor : IDbConnectionInterceptor
    {
        public void Opened(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
            // Set SESSION_CONTEXT to current UserId whenever EF opens a connection
            try
            {
                var userId = System.Web.HttpContext.Current.User.Identity.GetUserId();
                if (userId != null)
                {
                    DbCommand cmd = connection.CreateCommand();
                    cmd.CommandText = "EXEC sp_set_session_context @key=N'UserId', @value=@UserId";
                    DbParameter param = cmd.CreateParameter();
                    param.ParameterName = "@UserId";
                    param.Value = userId;
                    cmd.Parameters.Add(param);
                    cmd.ExecuteNonQuery();
                }
            }
            catch (System.NullReferenceException)
            {
                // If no user is logged in, leave SESSION_CONTEXT null (all rows will be filtered)
            }
        }

        public void Opening(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void BeganTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void BeginningTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void Closed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Closing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void ConnectionStringGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSet(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSetting(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionTimeoutGetting(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void ConnectionTimeoutGot(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void DataSourceGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DataSourceGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void Disposed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Disposing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void EnlistedTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void EnlistingTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void ServerVersionGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ServerVersionGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void StateGetting(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }

        public void StateGot(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }
    }

    public class SessionContextConfiguration : DbConfiguration
    {
        public SessionContextConfiguration()
        {
            AddInterceptor(new SessionContextInterceptor());
        }
    }
}
```

<span data-ttu-id="c00ff-124">To je vyžadována změna jenom aplikace.</span><span class="sxs-lookup"><span data-stu-id="c00ff-124">That's the only application change required.</span></span> <span data-ttu-id="c00ff-125">Pokračujte a sestavení a publikování aplikace.</span><span class="sxs-lookup"><span data-stu-id="c00ff-125">Go ahead and build and publish the application.</span></span>

## <a name="step-2-add-a-userid-column-to-the-database-schema"></a><span data-ttu-id="c00ff-126">Krok 2: Přidáte sloupec UserId schématu databáze</span><span class="sxs-lookup"><span data-stu-id="c00ff-126">Step 2: Add a UserId column to the database schema</span></span>
<span data-ttu-id="c00ff-127">Dále je potřeba přidat sloupec ID uživatele do tabulky kontaktů pro každý řádek přidružení uživatele (klientů).</span><span class="sxs-lookup"><span data-stu-id="c00ff-127">Next, we need to add a UserId column to the Contacts table to associate each row with a user (tenant).</span></span> <span data-ttu-id="c00ff-128">Jsme změní schéma přímo v databázi, aby nemáme mají být zahrnuty v tomto poli našeho EF datového modelu.</span><span class="sxs-lookup"><span data-stu-id="c00ff-128">We will alter the schema directly in the database, so that we don't have to include this field in our EF data model.</span></span>

<span data-ttu-id="c00ff-129">Připojení k databázi přímo, pomocí SQL Server Management Studio nebo Visual Studio a potom spusťte následující T-SQL:</span><span class="sxs-lookup"><span data-stu-id="c00ff-129">Connect to the database directly, using either SQL Server Management Studio or Visual Studio, and then execute the following T-SQL:</span></span>

```
ALTER TABLE Contacts ADD UserId nvarchar(128)
    DEFAULT CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
```

<span data-ttu-id="c00ff-130">Tento postup přidá sloupec ID uživatele do tabulky kontaktů.</span><span class="sxs-lookup"><span data-stu-id="c00ff-130">This adds a UserId column to the Contacts table.</span></span> <span data-ttu-id="c00ff-131">Můžeme použít datový typ nvarchar(128) tak, aby odpovídaly ID uživatelů uložené v tabulce AspNetUsers a vytvoříme výchozí omezení, která bude automaticky nastavena jako ID uživatele, který je uložen v SESSION_CONTEXT ID uživatele pro nově vložené řádky.</span><span class="sxs-lookup"><span data-stu-id="c00ff-131">We use the nvarchar(128) data type to match the UserIds stored in the AspNetUsers table, and we create a DEFAULT constraint that will automatically set the UserId for newly inserted rows to be the UserId currently stored in SESSION_CONTEXT.</span></span>

<span data-ttu-id="c00ff-132">V tabulce teď vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="c00ff-132">Now the table looks like this:</span></span>

![Tabulky kontaktů aplikace SSMS](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-Contacts.png)

<span data-ttu-id="c00ff-134">Při vytvoření nových kontaktů, že budete automaticky přiřazen správné ID uživatele.</span><span class="sxs-lookup"><span data-stu-id="c00ff-134">When new contacts are created, they'll automatically be assigned the correct UserId.</span></span> <span data-ttu-id="c00ff-135">Pro účely ukázky však umožňuje přiřadit několik těchto existující kontakty stávajícího uživatele.</span><span class="sxs-lookup"><span data-stu-id="c00ff-135">For demo purposes, however, let's assign a few of these existing contacts to an existing user.</span></span>

<span data-ttu-id="c00ff-136">Pokud jste vytvořili na několik uživatelů v aplikaci už (například pomocí místní, Google nebo Facebooku účty), zobrazí se v tabulce AspNetUsers.</span><span class="sxs-lookup"><span data-stu-id="c00ff-136">If you've created a few users in the application already (e.g., using local, Google, or Facebook accounts), you'll see them in the AspNetUsers table.</span></span> <span data-ttu-id="c00ff-137">Na tomto snímku obrazovky je pouze jeden uživatel dosavadní práce.</span><span class="sxs-lookup"><span data-stu-id="c00ff-137">In the screenshot below, there is only one user so far.</span></span>

![Tabulka SSMS AspNetUsers](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-AspNetUsers.png)

<span data-ttu-id="c00ff-139">Zkopírujte Id pro user1@contoso.coma vložte jej do příkaz T-SQL níže.</span><span class="sxs-lookup"><span data-stu-id="c00ff-139">Copy the Id for user1@contoso.com, and paste it into the T-SQL statement below.</span></span> <span data-ttu-id="c00ff-140">Spusťte tento příkaz tři kontakty přidružit ID uživatele.</span><span class="sxs-lookup"><span data-stu-id="c00ff-140">Execute this statement to associate three of the Contacts with this UserId.</span></span>

```
UPDATE Contacts SET UserId = '19bc9b0d-28dd-4510-bd5e-d6b6d445f511'
WHERE ContactId IN (1, 2, 5)
```

## <a name="step-3-create-a-row-level-security-policy-in-the-database"></a><span data-ttu-id="c00ff-141">Krok 3: Vytvoření zásad zabezpečení na úrovni řádků v databázi</span><span class="sxs-lookup"><span data-stu-id="c00ff-141">Step 3: Create a Row-Level Security policy in the database</span></span>
<span data-ttu-id="c00ff-142">Posledním krokem je vytvoření zásady zabezpečení, která používá ID uživatele v SESSION_CONTEXT automaticky filtrovat výsledky vrácené dotazy.</span><span class="sxs-lookup"><span data-stu-id="c00ff-142">The final step is to create a security policy that uses the UserId in SESSION_CONTEXT to automatically filter the results returned by queries.</span></span>

<span data-ttu-id="c00ff-143">Pokud stále připojené k databázi, spusťte následující T-SQL:</span><span class="sxs-lookup"><span data-stu-id="c00ff-143">While still connected to the database, execute the following T-SQL:</span></span>

```
CREATE SCHEMA Security
go

CREATE FUNCTION Security.userAccessPredicate(@UserId nvarchar(128))
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS accessResult
    WHERE @UserId = CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
go

CREATE SECURITY POLICY Security.userSecurityPolicy
    ADD FILTER PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts,
    ADD BLOCK PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts
go

```

<span data-ttu-id="c00ff-144">Tento kód provede tři věci.</span><span class="sxs-lookup"><span data-stu-id="c00ff-144">This code does three things.</span></span> <span data-ttu-id="c00ff-145">Nejprve vytvoří nové schéma jako osvědčený postup pro centralizuje a omezení přístupu k objektům RLS.</span><span class="sxs-lookup"><span data-stu-id="c00ff-145">First, it creates a new schema as a best practice for centralizing and limiting access to the RLS objects.</span></span> <span data-ttu-id="c00ff-146">V dalším kroku vytvoří predikátem funkci, která bude vracet '1' UserId řádku odpovídá ID uživatele v SESSION_CONTEXT.</span><span class="sxs-lookup"><span data-stu-id="c00ff-146">Next, it creates a predicate function that will return '1' when the UserId of a row matches the UserId in SESSION_CONTEXT.</span></span> <span data-ttu-id="c00ff-147">Nakonec se vytvoří zásadu zabezpečení, který přidává funkce jako filtr i bloku predikát pro tabulku kontaktů.</span><span class="sxs-lookup"><span data-stu-id="c00ff-147">Finally, it creates a security policy that adds this function as both a filter and block predicate on the Contacts table.</span></span> <span data-ttu-id="c00ff-148">Predikát filtru způsobí, že dotazy vrátit pouze sloupce, které patří do aktuálního uživatele a predikát block funguje jako pojistku zabránit aplikaci z někdy omylem vložíte řádek pro chybné uživatelské.</span><span class="sxs-lookup"><span data-stu-id="c00ff-148">The filter predicate causes queries to return only rows that belong to the current user, and the block predicate acts as a safeguard to prevent the application from ever accidentally inserting a row for the wrong user.</span></span>

<span data-ttu-id="c00ff-149">Nyní spusťte aplikaci a přihlaste se jako user1@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="c00ff-149">Now run the application, and sign in as user1@contoso.com.</span></span> <span data-ttu-id="c00ff-150">Tento uživatel nyní uvidí pouze kontakty jsme přiřazené k ID uživatele dříve:</span><span class="sxs-lookup"><span data-stu-id="c00ff-150">This user now sees only the Contacts we assigned to this UserId earlier:</span></span>

![Obraťte se na správce aplikace před povolením RLS](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-After.png)

<span data-ttu-id="c00ff-152">Chcete-li to další ověřit, zkuste registraci nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="c00ff-152">To validate this further, try registering a new user.</span></span> <span data-ttu-id="c00ff-153">Uvidí žádné kontakty, protože žádný byla přiřazena k nim.</span><span class="sxs-lookup"><span data-stu-id="c00ff-153">They will see no contacts, because none have been assigned to them.</span></span> <span data-ttu-id="c00ff-154">Pokud uživatel vytvořit nový kontakt, bude jí přiřazeno k nim a pouze bude moct zobrazovat.</span><span class="sxs-lookup"><span data-stu-id="c00ff-154">If they create a new contact, it will be assigned to them, and only they will be able to see it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c00ff-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c00ff-155">Next steps</span></span>
<span data-ttu-id="c00ff-156">A to je vše!</span><span class="sxs-lookup"><span data-stu-id="c00ff-156">That's it!</span></span> <span data-ttu-id="c00ff-157">Jednoduché webové aplikace obraťte se na správce byl převeden do více klientů, jeden kde každý uživatel má svou vlastní seznamu kontaktů.</span><span class="sxs-lookup"><span data-stu-id="c00ff-157">The simple Contact Manager web app has been converted into a multi-tenant one where each user has its own contact list.</span></span> <span data-ttu-id="c00ff-158">Pomocí zabezpečení na úrovni řádků jsme jste předejde složitosti vynucování logiku přístupu klienta v našem kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="c00ff-158">By using Row-Level Security, we've avoided the complexity of enforcing tenant access logic in our application code.</span></span> <span data-ttu-id="c00ff-159">Tato průhlednost umožňuje zaměřit se na aktuální skutečné obchodní problém aplikaci, a také snižuje riziko omylem úniky dat jako základu kódu aplikace zvětšování.</span><span class="sxs-lookup"><span data-stu-id="c00ff-159">This transparency allows the application to focus on the real business problem at hand, and it also reduces the risk of accidentally leaking data as the application's codebase grows.</span></span>

<span data-ttu-id="c00ff-160">V tomto kurzu má pouze poškrábání prostor co je možné s RLS.</span><span class="sxs-lookup"><span data-stu-id="c00ff-160">This tutorial has only scratched the surface of what's possible with RLS.</span></span> <span data-ttu-id="c00ff-161">Například je možné mít víc pokročilé nebo logiku granulární přístup ale je možné uložit více než jen aktuální ID uživatele SESSION_CONTEXT.</span><span class="sxs-lookup"><span data-stu-id="c00ff-161">For instance, it's possible to have more sophisticated or granular access logic, and it's possible to store more than just the current UserId in the SESSION_CONTEXT.</span></span> <span data-ttu-id="c00ff-162">Je také možné [RLS integrovat knihovny klienta nástroje elastické databáze](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) pro podporu víceklientské horizontálních oddílů v Škálováním na více systémů datové vrstvy.</span><span class="sxs-lookup"><span data-stu-id="c00ff-162">It's also possible to [integrate RLS with the elastic database tools client libraries](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) to support multi-tenant shards in a scale-out data tier.</span></span>

<span data-ttu-id="c00ff-163">Kromě těchto možnosti také pracujeme na RLS i vylepšit.</span><span class="sxs-lookup"><span data-stu-id="c00ff-163">Beyond these possibilities, we're also working to make RLS even better.</span></span> <span data-ttu-id="c00ff-164">Pokud máte nějaké otázky, nápady nebo věcí, které chcete zobrazit, dejte nám vědět, v komentářích.</span><span class="sxs-lookup"><span data-stu-id="c00ff-164">If you have any questions, ideas, or things you'd like to see, please let us know in the comments.</span></span> <span data-ttu-id="c00ff-165">Děkujeme za váš názor!</span><span class="sxs-lookup"><span data-stu-id="c00ff-165">We appreciate your feedback!</span></span>

