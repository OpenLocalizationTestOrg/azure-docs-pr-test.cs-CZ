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
# <a name="tutorial-web-app-with-a-multi-tenant-database-using-entity-framework-and-row-level-security"></a>Kurz: Webové aplikace s více klienty databázi pomocí Entity Framework a zabezpečení na úrovni řádků
Tento kurz ukazuje, jak toobuild více klientů webové aplikace pomocí "[sdílenou databázi, sdílené schématu](https://msdn.microsoft.com/library/aa479086.aspx)" klientů modelu s použitím rozhraní Entity Framework a [zabezpečení na úrovni řádků](https://msdn.microsoft.com/library/dn765131.aspx). V tomto modelu jedné databáze obsahuje data pro velký počet klientů každý řádek v každé tabulce souvisí s "Klienta ID" Nízkoúrovňového zabezpečení (RLS), novou funkci pro databázi SQL Azure, je použít tooprevent klientům v přístupu k výměně dat. To vyžaduje pouze jedinou, aplikaci toohello malých změn. Logikou centralizace hello klienta přístup v rámci samotná databáze hello RLS zjednodušuje hello kódu aplikace a snižuje riziko úniku dat náhodných mezi klienty hello.

Začněme s jednoduchou aplikaci obraťte se na správce hello z [vytvoření aplikace ASP.NET MVP pomocí ověřování a databázi SQL a nasazení tooAzure služby App Service](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). Právo teď hello aplikace umožňuje všichni uživatelé (klienty) toosee všechny kontakty:

![Obraťte se na správce aplikace před povolením RLS](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-Before.png)

S několika menší změny přidáme podporu pro víceklientskou architekturu, tak, aby se uživatelé moct toosee pouze hello kontakty, které patří toothem.

## <a name="step-1-add-an-interceptor-class-in-hello-application-tooset-hello-sessioncontext"></a>Krok 1: Přidání třídu interceptoru v hello aplikace tooset hello SESSION_CONTEXT
Neexistuje jeden změna aplikace potřebujeme toomake. Vzhledem k tomu, že všichni uživatelé aplikací připojit databázi pomocí toohello hello stejný připojovací řetězec (tj. stejné přihlašovací údaje SQL), aktuálně neexistuje žádný způsob pro RLS tooknow zásady uživatele, který by měl filtru pro. Tento přístup je velmi běžné u webových aplikací, protože umožňuje efektivní sdružování připojení, ale znamená, že potřebujeme jiný způsob tooidentify hello aktuální uživatel aplikace v rámci hello databáze. Hello řešení je sada aplikace hello toohave pár klíč hodnota pro hello aktuální ID uživatele v hello [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) okamžitě po otevření připojení, než se provede žádné dotazy. Úložiště dvojic klíč hodnota s rozsahem relace je SESSION_CONTEXT a naše zásady RLS bude používat hello ID uživatele v ní uloženy tooidentify hello aktuálního uživatele.

Přidáme [interceptoru](https://msdn.microsoft.com/data/dn469464.aspx) (konkrétně [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), nová funkce v Entity Framework (EF) 6, tooautomatically sadu hello aktuální ID uživatele v hello SESSION_CONTEXT spuštěním Příkaz T-SQL vždy, když EF otevře připojení.

1. Otevřete hello ContactManager projektu v sadě Visual Studio.
2. Klikněte pravým tlačítkem na složku modely hello v hello Průzkumníku řešení a zvolte možnost Přidat > třída.
3. Pojmenujte novou třídu hello "SessionContextInterceptor.cs" a klikněte na tlačítko Přidat.
4. Nahraďte obsah hello SessionContextInterceptor.cs hello následující kód.

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

Je vyžadována změna jenom aplikace hello. Pokračujte a sestavení a publikování aplikace hello.

## <a name="step-2-add-a-userid-column-toohello-database-schema"></a>Krok 2: Přidání schéma databáze toohello sloupec ID uživatele
V dalším kroku potřebujeme tooadd tooassociate tabulky kontaktů UserId sloupec toohello každý řádek s uživatelem (klientů). Jsme změní schéma hello přímo v hello databáze, tak, aby nemáme tooinclude toto pole v našem EF datového modelu.

Připojit databáze toohello přímo, pomocí SQL Server Management Studio nebo Visual Studio a potom spusťte hello následující T-SQL:

```
ALTER TABLE Contacts ADD UserId nvarchar(128)
    DEFAULT CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
```

Tento postup přidá tabulku kontaktů toohello sloupec ID uživatele. Používáme hello nvarchar(128) datový typ toomatch hello ID uživatelů uložené v tabulce AspNetUsers hello a vytvoříme výchozí omezení, která bude automaticky nastavena hello ID uživatele pro nově vložené řádky toobe hello ID uživatele, které jsou aktuálně uloženy ve SESSION_CONTEXT.

Tabulka hello teď vypadá takto:

![Tabulky kontaktů aplikace SSMS](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-Contacts.png)

Při vytvoření nových kontaktů, že budete automaticky přiřazen hello Opravte ID uživatele. Pro účely ukázky ale umožňuje přiřadit několik těchto existující kontakty tooan stávající uživatele.

Pokud jste vytvořili na několik uživatelů v aplikaci hello již (například pomocí místní, Google nebo Facebooku účty), zobrazí se v tabulce AspNetUsers hello. Na snímku obrazovky hello níže není dosavadní jenom s jedním uživatelem.

![Tabulka SSMS AspNetUsers](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-AspNetUsers.png)

Kopírování hello Id pro user1@contoso.coma vložte jej do hello T-SQL příkaz níže. Spusťte tento příkaz tooassociate tři hello kontaktů s ID uživatele.

```
UPDATE Contacts SET UserId = '19bc9b0d-28dd-4510-bd5e-d6b6d445f511'
WHERE ContactId IN (1, 2, 5)
```

## <a name="step-3-create-a-row-level-security-policy-in-hello-database"></a>Krok 3: Vytvoření zásad zabezpečení na úrovni řádků v databázi hello
posledním krokem Hello je toocreate zásadu zabezpečení, která používá hello ID uživatele v SESSION_CONTEXT tooautomatically filtru hello výsledků vrácených dotazy.

Při toohello stále připojená databáze spusťte hello následující T-SQL:

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

Tento kód provede tři věci. Nejprve vytvoří nové schéma jako osvědčený postup pro centralizuje a omezení přístupu toohello RLS objekty. V dalším kroku vytvoří predikátem funkci, která bude vracet '1' hello UserId řádek odpovídá hello ID uživatele v SESSION_CONTEXT. Nakonec se vytvoří zásadu zabezpečení, který přidává funkce jako filtr i bloku predikát pro tabulku kontaktů hello. Predikát filtru Hello způsobí, že dotazy tooreturn pouze řádky, které patří toohello aktuální uživatel a predikát block hello funguje jako aplikace hello tooprevent chránit z někdy omylem vložíte řádek pro chybné uživatelské hello.

Spuštění aplikace hello a přihlaste se jako teď user1@contoso.com. Tento uživatel nyní uvidí pouze hello kontakty jsme přiřazené toothis UserId dříve:

![Obraťte se na správce aplikace před povolením RLS](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-After.png)

toovalidate to další, pokuste se registraci nového uživatele. Uvidí žádné kontakty, protože žádný byla přiřazena toothem. Pokud uživatel vytvořit nový kontakt, bude jí přiřazeno toothem a pouze budou mít toosee ho.

## <a name="next-steps"></a>Další kroky
A to je vše! Hello jednoduché obraťte se na správce webové aplikace byla převedena do více klientů, jeden kde každý uživatel má svou vlastní seznamu kontaktů. Pomocí zabezpečení na úrovni řádků jsme jste předejde hello složitosti vynucování logiku přístupu klienta v našem kódu aplikace. Tato průhlednost umožňuje toofocus aplikace hello na hello skutečné obchodní problém, a také snižuje riziko hello omylem úniky dat jako základu kódu aplikace hello zvětšování.

V tomto kurzu má pouze poškrábaný hello prostor co je možné s RLS. Například je možné toohave sofistikovanější nebo logiku granulární přístup a jeho možných toostore více než jen hello aktuální ID uživatele v hello SESSION_CONTEXT. Je také možné příliš[RLS integrovat knihovny klienta nástroje elastické databáze hello](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) toosupport víceklientské horizontálních oddílů v Škálováním na více systémů datové vrstvy.

Kromě těchto možnosti také pracujeme toomake RLS ještě lepší. Pokud máte nějaké otázky, nápady nebo co byste chtěli toosee, dejte nám vědět, v komentářích hello. Děkujeme za váš názor!

