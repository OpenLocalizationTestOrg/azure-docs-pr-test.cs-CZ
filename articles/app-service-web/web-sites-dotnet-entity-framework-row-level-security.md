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
# <a name="tutorial-web-app-with-a-multi-tenant-database-using-entity-framework-and-row-level-security"></a>Kurz: Webové aplikace s více klienty databázi pomocí Entity Framework a zabezpečení na úrovni řádků
Tento kurz ukazuje, jak sestavit víceklientské webové aplikace s "[sdílenou databázi, sdílené schématu](https://msdn.microsoft.com/library/aa479086.aspx)" klientů modelu s použitím rozhraní Entity Framework a [zabezpečení na úrovni řádků](https://msdn.microsoft.com/library/dn765131.aspx). V tomto modelu jedné databáze obsahuje data pro velký počet klientů každý řádek v každé tabulce souvisí s "Klienta ID" Zabránit klientům v přístupu k výměně dat se používá nízkoúrovňového zabezpečení (RLS), novou funkci pro databázi SQL Azure. To vyžaduje pouze jediné, malé změny do aplikace. Centralizací logiky přístupu klienta v rámci samotná databáze RLS zjednodušuje kódu aplikace a snižuje riziko úniku dat náhodných mezi klienty.

Začněme s jednoduchou aplikaci obraťte se na správce z [vytvoření aplikace ASP.NET MVP pomocí ověřování a databázi SQL a nasazení do Azure App Service](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). Právo nyní aplikace umožňuje všem uživatelům (klienty) Chcete-li zobrazit všechny kontakty:

![Obraťte se na správce aplikace před povolením RLS](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-Before.png)

S několika menší změny přidáme podporu pro víceklientskou architekturu, tak, aby se uživatelé moct prohlížet pouze kontakty, které patří k nim.

## <a name="step-1-add-an-interceptor-class-in-the-application-to-set-the-sessioncontext"></a>Krok 1: Přidání třídu interceptoru v aplikaci nastavení SESSION_CONTEXT
Neexistuje jeden změny aplikace, které je třeba mít. Vzhledem k tomu, že všichni uživatelé aplikací připojit k databázi pomocí stejný připojovací řetězec (tj. stejné přihlašovací údaje SQL), se aktuálně nijak pro zásadu RLS vědět, kteří uživatelé měli filtrovat. Tento přístup je velmi běžné u webových aplikací, protože umožňuje efektivní sdružování připojení, ale znamená, že potřebujeme jiný způsob, jak identifikovat aktuální uživatel aplikace v rámci databáze. Řešení je nastavit pár klíč hodnota pro aktuální ID uživatele v aplikaci [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) okamžitě po otevření připojení, než se provede žádné dotazy. Úložiště dvojic klíč hodnota s rozsahem relace je SESSION_CONTEXT a naše zásady RLS bude používat ID uživatele v ní uloženy k identifikaci aktuálního uživatele.

Přidáme [interceptoru](https://msdn.microsoft.com/data/dn469464.aspx) (konkrétně [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), nová funkce v Framework Entity (EF) 6, umožňuje automaticky nastavit aktuální ID uživatele v SESSION_CONTEXT spuštěním příkazu T-SQL vždy, když EF otevře připojení.

1. Otevřete projekt ContactManager v sadě Visual Studio.
2. Klikněte pravým tlačítkem na složku modely v Průzkumníku řešení a zvolte možnost Přidat > třída.
3. Pojmenujte novou třídu "SessionContextInterceptor.cs" a klikněte na tlačítko Přidat.
4. Obsah SessionContextInterceptor.cs nahraďte následujícím kódem.

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

To je vyžadována změna jenom aplikace. Pokračujte a sestavení a publikování aplikace.

## <a name="step-2-add-a-userid-column-to-the-database-schema"></a>Krok 2: Přidáte sloupec UserId schématu databáze
Dále je potřeba přidat sloupec ID uživatele do tabulky kontaktů pro každý řádek přidružení uživatele (klientů). Jsme změní schéma přímo v databázi, aby nemáme mají být zahrnuty v tomto poli našeho EF datového modelu.

Připojení k databázi přímo, pomocí SQL Server Management Studio nebo Visual Studio a potom spusťte následující T-SQL:

```
ALTER TABLE Contacts ADD UserId nvarchar(128)
    DEFAULT CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
```

Tento postup přidá sloupec ID uživatele do tabulky kontaktů. Můžeme použít datový typ nvarchar(128) tak, aby odpovídaly ID uživatelů uložené v tabulce AspNetUsers a vytvoříme výchozí omezení, která bude automaticky nastavena jako ID uživatele, který je uložen v SESSION_CONTEXT ID uživatele pro nově vložené řádky.

V tabulce teď vypadá takto:

![Tabulky kontaktů aplikace SSMS](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-Contacts.png)

Při vytvoření nových kontaktů, že budete automaticky přiřazen správné ID uživatele. Pro účely ukázky však umožňuje přiřadit několik těchto existující kontakty stávajícího uživatele.

Pokud jste vytvořili na několik uživatelů v aplikaci už (například pomocí místní, Google nebo Facebooku účty), zobrazí se v tabulce AspNetUsers. Na tomto snímku obrazovky je pouze jeden uživatel dosavadní práce.

![Tabulka SSMS AspNetUsers](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-AspNetUsers.png)

Zkopírujte Id pro user1@contoso.coma vložte jej do příkaz T-SQL níže. Spusťte tento příkaz tři kontakty přidružit ID uživatele.

```
UPDATE Contacts SET UserId = '19bc9b0d-28dd-4510-bd5e-d6b6d445f511'
WHERE ContactId IN (1, 2, 5)
```

## <a name="step-3-create-a-row-level-security-policy-in-the-database"></a>Krok 3: Vytvoření zásad zabezpečení na úrovni řádků v databázi
Posledním krokem je vytvoření zásady zabezpečení, která používá ID uživatele v SESSION_CONTEXT automaticky filtrovat výsledky vrácené dotazy.

Pokud stále připojené k databázi, spusťte následující T-SQL:

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

Tento kód provede tři věci. Nejprve vytvoří nové schéma jako osvědčený postup pro centralizuje a omezení přístupu k objektům RLS. V dalším kroku vytvoří predikátem funkci, která bude vracet '1' UserId řádku odpovídá ID uživatele v SESSION_CONTEXT. Nakonec se vytvoří zásadu zabezpečení, který přidává funkce jako filtr i bloku predikát pro tabulku kontaktů. Predikát filtru způsobí, že dotazy vrátit pouze sloupce, které patří do aktuálního uživatele a predikát block funguje jako pojistku zabránit aplikaci z někdy omylem vložíte řádek pro chybné uživatelské.

Nyní spusťte aplikaci a přihlaste se jako user1@contoso.com. Tento uživatel nyní uvidí pouze kontakty jsme přiřazené k ID uživatele dříve:

![Obraťte se na správce aplikace před povolením RLS](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-After.png)

Chcete-li to další ověřit, zkuste registraci nového uživatele. Uvidí žádné kontakty, protože žádný byla přiřazena k nim. Pokud uživatel vytvořit nový kontakt, bude jí přiřazeno k nim a pouze bude moct zobrazovat.

## <a name="next-steps"></a>Další kroky
A to je vše! Jednoduché webové aplikace obraťte se na správce byl převeden do více klientů, jeden kde každý uživatel má svou vlastní seznamu kontaktů. Pomocí zabezpečení na úrovni řádků jsme jste předejde složitosti vynucování logiku přístupu klienta v našem kódu aplikace. Tato průhlednost umožňuje zaměřit se na aktuální skutečné obchodní problém aplikaci, a také snižuje riziko omylem úniky dat jako základu kódu aplikace zvětšování.

V tomto kurzu má pouze poškrábání prostor co je možné s RLS. Například je možné mít víc pokročilé nebo logiku granulární přístup ale je možné uložit více než jen aktuální ID uživatele SESSION_CONTEXT. Je také možné [RLS integrovat knihovny klienta nástroje elastické databáze](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) pro podporu víceklientské horizontálních oddílů v Škálováním na více systémů datové vrstvy.

Kromě těchto možnosti také pracujeme na RLS i vylepšit. Pokud máte nějaké otázky, nápady nebo věcí, které chcete zobrazit, dejte nám vědět, v komentářích. Děkujeme za váš názor!

