---
title: "Kurz: Webové aplikace s více klienty databázi pomocí Entity Framework a zabezpečení na úrovni řádků"
description: "Zjistěte, jak toodevelop ASP.NET MVC 5 webová aplikace s více klienty backent, SQL Database pomocí rozhraní Entity Framework a zabezpečení na úrovni řádků."
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
ms.openlocfilehash: 1b715e01807032c3f6497c374ce427dd762af141
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-web-app-with-a-multi-tenant-database-using-entity-framework-and-row-level-security"></a><span data-ttu-id="2805d-103">Kurz: Webové aplikace s více klienty databázi pomocí Entity Framework a zabezpečení na úrovni řádků</span><span class="sxs-lookup"><span data-stu-id="2805d-103">Tutorial: Web app with a multi-tenant database using Entity Framework and Row-Level Security</span></span>
<span data-ttu-id="2805d-104">Tento kurz ukazuje, jak toobuild více klientů webové aplikace pomocí "[sdílenou databázi, sdílené schématu](https://msdn.microsoft.com/library/aa479086.aspx)" klientů modelu s použitím rozhraní Entity Framework a [zabezpečení na úrovni řádků](https://msdn.microsoft.com/library/dn765131.aspx).</span><span class="sxs-lookup"><span data-stu-id="2805d-104">This tutorial shows how toobuild a multi-tenant web app with a "[shared database, shared schema](https://msdn.microsoft.com/library/aa479086.aspx)" tenancy model, using Entity Framework and [Row-Level Security](https://msdn.microsoft.com/library/dn765131.aspx).</span></span> <span data-ttu-id="2805d-105">V tomto modelu jedné databáze obsahuje data pro velký počet klientů každý řádek v každé tabulce souvisí s "Klienta ID"</span><span class="sxs-lookup"><span data-stu-id="2805d-105">In this model, a single database contains data for many tenants, and each row in each table is associated with a "Tenant ID."</span></span> <span data-ttu-id="2805d-106">Nízkoúrovňového zabezpečení (RLS), novou funkci pro databázi SQL Azure, je použít tooprevent klientům v přístupu k výměně dat.</span><span class="sxs-lookup"><span data-stu-id="2805d-106">Row-Level Security (RLS), a new feature for Azure SQL Database, is used tooprevent tenants from accessing each other's data.</span></span> <span data-ttu-id="2805d-107">To vyžaduje pouze jedinou, aplikaci toohello malých změn.</span><span class="sxs-lookup"><span data-stu-id="2805d-107">This requires just a single, small change toohello application.</span></span> <span data-ttu-id="2805d-108">Logikou centralizace hello klienta přístup v rámci samotná databáze hello RLS zjednodušuje hello kódu aplikace a snižuje riziko úniku dat náhodných mezi klienty hello.</span><span class="sxs-lookup"><span data-stu-id="2805d-108">By centralizing hello tenant access logic within hello database itself, RLS simplifies hello application code and reduces hello risk of accidental data leakage between tenants.</span></span>

<span data-ttu-id="2805d-109">Začněme s jednoduchou aplikaci obraťte se na správce hello z [vytvoření aplikace ASP.NET MVP pomocí ověřování a databázi SQL a nasazení tooAzure služby App Service](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="2805d-109">Let's start with hello simple Contact Manager application from [Create an ASP.NET MVP app with auth and SQL DB and deploy tooAzure App Service](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).</span></span> <span data-ttu-id="2805d-110">Právo teď hello aplikace umožňuje všichni uživatelé (klienty) toosee všechny kontakty:</span><span class="sxs-lookup"><span data-stu-id="2805d-110">Right now, hello application allows all users (tenants) toosee all contacts:</span></span>

![Obraťte se na správce aplikace před povolením RLS](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-Before.png)

<span data-ttu-id="2805d-112">S několika menší změny přidáme podporu pro víceklientskou architekturu, tak, aby se uživatelé moct toosee pouze hello kontakty, které patří toothem.</span><span class="sxs-lookup"><span data-stu-id="2805d-112">With just a few small changes, we will add support for multi-tenancy, so that users are able toosee only hello contacts that belong toothem.</span></span>

## <a name="step-1-add-an-interceptor-class-in-hello-application-tooset-hello-sessioncontext"></a><span data-ttu-id="2805d-113">Krok 1: Přidání třídu interceptoru v hello aplikace tooset hello SESSION_CONTEXT</span><span class="sxs-lookup"><span data-stu-id="2805d-113">Step 1: Add an Interceptor class in hello application tooset hello SESSION_CONTEXT</span></span>
<span data-ttu-id="2805d-114">Neexistuje jeden změna aplikace potřebujeme toomake.</span><span class="sxs-lookup"><span data-stu-id="2805d-114">There is one application change we need toomake.</span></span> <span data-ttu-id="2805d-115">Vzhledem k tomu, že všichni uživatelé aplikací připojit databázi pomocí toohello hello stejný připojovací řetězec (tj. stejné přihlašovací údaje SQL), aktuálně neexistuje žádný způsob pro RLS tooknow zásady uživatele, který by měl filtru pro.</span><span class="sxs-lookup"><span data-stu-id="2805d-115">Because all application users connect toohello database using hello same connection string (i.e. same SQL login), there is currently no way for an RLS policy tooknow which user it should filter for.</span></span> <span data-ttu-id="2805d-116">Tento přístup je velmi běžné u webových aplikací, protože umožňuje efektivní sdružování připojení, ale znamená, že potřebujeme jiný způsob tooidentify hello aktuální uživatel aplikace v rámci hello databáze.</span><span class="sxs-lookup"><span data-stu-id="2805d-116">This approach is very common in web applications because it enables efficient connection pooling, but it means we need another way tooidentify hello current application user within hello database.</span></span> <span data-ttu-id="2805d-117">Hello řešení je sada aplikace hello toohave pár klíč hodnota pro hello aktuální ID uživatele v hello [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) okamžitě po otevření připojení, než se provede žádné dotazy.</span><span class="sxs-lookup"><span data-stu-id="2805d-117">hello solution is toohave hello application set a key-value pair for hello current UserId in hello [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) immediately after opening a connection, before it executes any queries.</span></span> <span data-ttu-id="2805d-118">Úložiště dvojic klíč hodnota s rozsahem relace je SESSION_CONTEXT a naše zásady RLS bude používat hello ID uživatele v ní uloženy tooidentify hello aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="2805d-118">SESSION_CONTEXT is a session-scoped key-value store, and our RLS policy will use hello UserId stored in it tooidentify hello current user.</span></span>

<span data-ttu-id="2805d-119">Přidáme [interceptoru](https://msdn.microsoft.com/data/dn469464.aspx) (konkrétně [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), nová funkce v Entity Framework (EF) 6, tooautomatically sadu hello aktuální ID uživatele v hello SESSION_CONTEXT spuštěním Příkaz T-SQL vždy, když EF otevře připojení.</span><span class="sxs-lookup"><span data-stu-id="2805d-119">We will add an [interceptor](https://msdn.microsoft.com/data/dn469464.aspx) (in particular, a [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), a new feature in Entity Framework (EF) 6, tooautomatically set hello current UserId in hello SESSION_CONTEXT by executing a T-SQL statement whenever EF opens a connection.</span></span>

1. <span data-ttu-id="2805d-120">Otevřete hello ContactManager projektu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2805d-120">Open hello ContactManager project in Visual Studio.</span></span>
2. <span data-ttu-id="2805d-121">Klikněte pravým tlačítkem na složku modely hello v hello Průzkumníku řešení a zvolte možnost Přidat > třída.</span><span class="sxs-lookup"><span data-stu-id="2805d-121">Right-click on hello Models folder in hello Solution Explorer, and choose Add > Class.</span></span>
3. <span data-ttu-id="2805d-122">Pojmenujte novou třídu hello "SessionContextInterceptor.cs" a klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="2805d-122">Name hello new class "SessionContextInterceptor.cs" and click Add.</span></span>
4. <span data-ttu-id="2805d-123">Nahraďte obsah hello SessionContextInterceptor.cs hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="2805d-123">Replace hello contents of SessionContextInterceptor.cs with hello following code.</span></span>

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
            // Set SESSION_CONTEXT toocurrent UserId whenever EF opens a connection
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

<span data-ttu-id="2805d-124">Je vyžadována změna jenom aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="2805d-124">That's hello only application change required.</span></span> <span data-ttu-id="2805d-125">Pokračujte a sestavení a publikování aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="2805d-125">Go ahead and build and publish hello application.</span></span>

## <a name="step-2-add-a-userid-column-toohello-database-schema"></a><span data-ttu-id="2805d-126">Krok 2: Přidání schéma databáze toohello sloupec ID uživatele</span><span class="sxs-lookup"><span data-stu-id="2805d-126">Step 2: Add a UserId column toohello database schema</span></span>
<span data-ttu-id="2805d-127">V dalším kroku potřebujeme tooadd tooassociate tabulky kontaktů UserId sloupec toohello každý řádek s uživatelem (klientů).</span><span class="sxs-lookup"><span data-stu-id="2805d-127">Next, we need tooadd a UserId column toohello Contacts table tooassociate each row with a user (tenant).</span></span> <span data-ttu-id="2805d-128">Jsme změní schéma hello přímo v hello databáze, tak, aby nemáme tooinclude toto pole v našem EF datového modelu.</span><span class="sxs-lookup"><span data-stu-id="2805d-128">We will alter hello schema directly in hello database, so that we don't have tooinclude this field in our EF data model.</span></span>

<span data-ttu-id="2805d-129">Připojit databáze toohello přímo, pomocí SQL Server Management Studio nebo Visual Studio a potom spusťte hello následující T-SQL:</span><span class="sxs-lookup"><span data-stu-id="2805d-129">Connect toohello database directly, using either SQL Server Management Studio or Visual Studio, and then execute hello following T-SQL:</span></span>

```
ALTER TABLE Contacts ADD UserId nvarchar(128)
    DEFAULT CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
```

<span data-ttu-id="2805d-130">Tento postup přidá tabulku kontaktů toohello sloupec ID uživatele.</span><span class="sxs-lookup"><span data-stu-id="2805d-130">This adds a UserId column toohello Contacts table.</span></span> <span data-ttu-id="2805d-131">Používáme hello nvarchar(128) datový typ toomatch hello ID uživatelů uložené v tabulce AspNetUsers hello a vytvoříme výchozí omezení, která bude automaticky nastavena hello ID uživatele pro nově vložené řádky toobe hello ID uživatele, které jsou aktuálně uloženy ve SESSION_CONTEXT.</span><span class="sxs-lookup"><span data-stu-id="2805d-131">We use hello nvarchar(128) data type toomatch hello UserIds stored in hello AspNetUsers table, and we create a DEFAULT constraint that will automatically set hello UserId for newly inserted rows toobe hello UserId currently stored in SESSION_CONTEXT.</span></span>

<span data-ttu-id="2805d-132">Tabulka hello teď vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="2805d-132">Now hello table looks like this:</span></span>

![Tabulky kontaktů aplikace SSMS](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-Contacts.png)

<span data-ttu-id="2805d-134">Při vytvoření nových kontaktů, že budete automaticky přiřazen hello Opravte ID uživatele.</span><span class="sxs-lookup"><span data-stu-id="2805d-134">When new contacts are created, they'll automatically be assigned hello correct UserId.</span></span> <span data-ttu-id="2805d-135">Pro účely ukázky ale umožňuje přiřadit několik těchto existující kontakty tooan stávající uživatele.</span><span class="sxs-lookup"><span data-stu-id="2805d-135">For demo purposes, however, let's assign a few of these existing contacts tooan existing user.</span></span>

<span data-ttu-id="2805d-136">Pokud jste vytvořili na několik uživatelů v aplikaci hello již (například pomocí místní, Google nebo Facebooku účty), zobrazí se v tabulce AspNetUsers hello.</span><span class="sxs-lookup"><span data-stu-id="2805d-136">If you've created a few users in hello application already (e.g., using local, Google, or Facebook accounts), you'll see them in hello AspNetUsers table.</span></span> <span data-ttu-id="2805d-137">Na snímku obrazovky hello níže není dosavadní jenom s jedním uživatelem.</span><span class="sxs-lookup"><span data-stu-id="2805d-137">In hello screenshot below, there is only one user so far.</span></span>

![Tabulka SSMS AspNetUsers](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-AspNetUsers.png)

<span data-ttu-id="2805d-139">Kopírování hello Id pro user1@contoso.coma vložte jej do hello T-SQL příkaz níže.</span><span class="sxs-lookup"><span data-stu-id="2805d-139">Copy hello Id for user1@contoso.com, and paste it into hello T-SQL statement below.</span></span> <span data-ttu-id="2805d-140">Spusťte tento příkaz tooassociate tři hello kontaktů s ID uživatele.</span><span class="sxs-lookup"><span data-stu-id="2805d-140">Execute this statement tooassociate three of hello Contacts with this UserId.</span></span>

```
UPDATE Contacts SET UserId = '19bc9b0d-28dd-4510-bd5e-d6b6d445f511'
WHERE ContactId IN (1, 2, 5)
```

## <a name="step-3-create-a-row-level-security-policy-in-hello-database"></a><span data-ttu-id="2805d-141">Krok 3: Vytvoření zásad zabezpečení na úrovni řádků v databázi hello</span><span class="sxs-lookup"><span data-stu-id="2805d-141">Step 3: Create a Row-Level Security policy in hello database</span></span>
<span data-ttu-id="2805d-142">posledním krokem Hello je toocreate zásadu zabezpečení, která používá hello ID uživatele v SESSION_CONTEXT tooautomatically filtru hello výsledků vrácených dotazy.</span><span class="sxs-lookup"><span data-stu-id="2805d-142">hello final step is toocreate a security policy that uses hello UserId in SESSION_CONTEXT tooautomatically filter hello results returned by queries.</span></span>

<span data-ttu-id="2805d-143">Při toohello stále připojená databáze spusťte hello následující T-SQL:</span><span class="sxs-lookup"><span data-stu-id="2805d-143">While still connected toohello database, execute hello following T-SQL:</span></span>

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

<span data-ttu-id="2805d-144">Tento kód provede tři věci.</span><span class="sxs-lookup"><span data-stu-id="2805d-144">This code does three things.</span></span> <span data-ttu-id="2805d-145">Nejprve vytvoří nové schéma jako osvědčený postup pro centralizuje a omezení přístupu toohello RLS objekty.</span><span class="sxs-lookup"><span data-stu-id="2805d-145">First, it creates a new schema as a best practice for centralizing and limiting access toohello RLS objects.</span></span> <span data-ttu-id="2805d-146">V dalším kroku vytvoří predikátem funkci, která bude vracet '1' hello UserId řádek odpovídá hello ID uživatele v SESSION_CONTEXT.</span><span class="sxs-lookup"><span data-stu-id="2805d-146">Next, it creates a predicate function that will return '1' when hello UserId of a row matches hello UserId in SESSION_CONTEXT.</span></span> <span data-ttu-id="2805d-147">Nakonec se vytvoří zásadu zabezpečení, který přidává funkce jako filtr i bloku predikát pro tabulku kontaktů hello.</span><span class="sxs-lookup"><span data-stu-id="2805d-147">Finally, it creates a security policy that adds this function as both a filter and block predicate on hello Contacts table.</span></span> <span data-ttu-id="2805d-148">Predikát filtru Hello způsobí, že dotazy tooreturn pouze řádky, které patří toohello aktuální uživatel a predikát block hello funguje jako aplikace hello tooprevent chránit z někdy omylem vložíte řádek pro chybné uživatelské hello.</span><span class="sxs-lookup"><span data-stu-id="2805d-148">hello filter predicate causes queries tooreturn only rows that belong toohello current user, and hello block predicate acts as a safeguard tooprevent hello application from ever accidentally inserting a row for hello wrong user.</span></span>

<span data-ttu-id="2805d-149">Spuštění aplikace hello a přihlaste se jako teď user1@contoso.com. Tento uživatel nyní uvidí pouze hello kontakty jsme přiřazené toothis UserId dříve:</span><span class="sxs-lookup"><span data-stu-id="2805d-149">Now run hello application, and sign in as user1@contoso.com. This user now sees only hello Contacts we assigned toothis UserId earlier:</span></span>

![Obraťte se na správce aplikace před povolením RLS](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-After.png)

<span data-ttu-id="2805d-151">toovalidate to další, pokuste se registraci nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="2805d-151">toovalidate this further, try registering a new user.</span></span> <span data-ttu-id="2805d-152">Uvidí žádné kontakty, protože žádný byla přiřazena toothem.</span><span class="sxs-lookup"><span data-stu-id="2805d-152">They will see no contacts, because none have been assigned toothem.</span></span> <span data-ttu-id="2805d-153">Pokud uživatel vytvořit nový kontakt, bude jí přiřazeno toothem a pouze budou mít toosee ho.</span><span class="sxs-lookup"><span data-stu-id="2805d-153">If they create a new contact, it will be assigned toothem, and only they will be able toosee it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2805d-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2805d-154">Next steps</span></span>
<span data-ttu-id="2805d-155">A to je vše!</span><span class="sxs-lookup"><span data-stu-id="2805d-155">That's it!</span></span> <span data-ttu-id="2805d-156">Hello jednoduché obraťte se na správce webové aplikace byla převedena do více klientů, jeden kde každý uživatel má svou vlastní seznamu kontaktů.</span><span class="sxs-lookup"><span data-stu-id="2805d-156">hello simple Contact Manager web app has been converted into a multi-tenant one where each user has its own contact list.</span></span> <span data-ttu-id="2805d-157">Pomocí zabezpečení na úrovni řádků jsme jste předejde hello složitosti vynucování logiku přístupu klienta v našem kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="2805d-157">By using Row-Level Security, we've avoided hello complexity of enforcing tenant access logic in our application code.</span></span> <span data-ttu-id="2805d-158">Tato průhlednost umožňuje toofocus aplikace hello na hello skutečné obchodní problém, a také snižuje riziko hello omylem úniky dat jako základu kódu aplikace hello zvětšování.</span><span class="sxs-lookup"><span data-stu-id="2805d-158">This transparency allows hello application toofocus on hello real business problem at hand, and it also reduces hello risk of accidentally leaking data as hello application's codebase grows.</span></span>

<span data-ttu-id="2805d-159">V tomto kurzu má pouze poškrábaný hello prostor co je možné s RLS.</span><span class="sxs-lookup"><span data-stu-id="2805d-159">This tutorial has only scratched hello surface of what's possible with RLS.</span></span> <span data-ttu-id="2805d-160">Například je možné toohave sofistikovanější nebo logiku granulární přístup a jeho možných toostore více než jen hello aktuální ID uživatele v hello SESSION_CONTEXT.</span><span class="sxs-lookup"><span data-stu-id="2805d-160">For instance, it's possible toohave more sophisticated or granular access logic, and it's possible toostore more than just hello current UserId in hello SESSION_CONTEXT.</span></span> <span data-ttu-id="2805d-161">Je také možné příliš[RLS integrovat knihovny klienta nástroje elastické databáze hello](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) toosupport víceklientské horizontálních oddílů v Škálováním na více systémů datové vrstvy.</span><span class="sxs-lookup"><span data-stu-id="2805d-161">It's also possible too[integrate RLS with hello elastic database tools client libraries](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) toosupport multi-tenant shards in a scale-out data tier.</span></span>

<span data-ttu-id="2805d-162">Kromě těchto možnosti také pracujeme toomake RLS ještě lepší.</span><span class="sxs-lookup"><span data-stu-id="2805d-162">Beyond these possibilities, we're also working toomake RLS even better.</span></span> <span data-ttu-id="2805d-163">Pokud máte nějaké otázky, nápady nebo co byste chtěli toosee, dejte nám vědět, v komentářích hello.</span><span class="sxs-lookup"><span data-stu-id="2805d-163">If you have any questions, ideas, or things you'd like toosee, please let us know in hello comments.</span></span> <span data-ttu-id="2805d-164">Děkujeme za váš názor!</span><span class="sxs-lookup"><span data-stu-id="2805d-164">We appreciate your feedback!</span></span>

