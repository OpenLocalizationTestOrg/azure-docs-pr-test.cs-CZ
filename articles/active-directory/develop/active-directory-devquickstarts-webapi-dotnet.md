---
title: "Azure AD .NET webové rozhraní API Začínáme | Microsoft Docs"
description: "Postup sestavení .NET MVC webového rozhraní API, který se integruje s Azure AD pro ověřování a autorizaci."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 67e74774-1748-43ea-8130-55275a18320f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: f44d75f45073a5d9aa9b1863ed227aba4efcf785
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="help-protect-a-web-api-by-using-bearer-tokens-from-azure-ad"></a>Pomoc při ochraně webového rozhraní API pomocí nosných tokenů z Azure AD
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Pokud vytváříte aplikace, která poskytuje přístup k chráněným prostředkům, musíte vědět, jak zabránit bude vyplacena neoprávněně přístupu na tyto prostředky.
Díky Azure Active Directory (Azure AD) je jednoduchá a přímočará k ochraně webového rozhraní API pomocí přístupu k nosiče OAuth 2.0 tokeny jenom pár řádků kódu.

V aplikacích ASP.NET web můžete provést tuto ochranu pomocí implementace Microsoft OWIN middleware komunitou vytvářený zahrnuté v rozhraní .NET Framework 4.5. Zde použijeme OWIN pro sestavení webového rozhraní API "Seznam úkolů", který:

* Označí, které rozhraní API jsou chráněny.
* Ověří, že volání webového rozhraní API obsahovat platné přístupový token.

Pokud chcete vytvořit pro provést seznamu rozhraní API, musíte nejprve:

1. Zaregistrovat aplikaci s Azure AD.
2. Nastavení aplikace k použití ověřovacího kanálu OWIN.
3. Konfigurovat klientskou aplikaci pro volání webového rozhraní API.

Abyste mohli začít, [stáhnout kostru aplikace](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) nebo [stažení je hotová ukázka](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip). Každá je řešením sady Visual Studio 2013. Musíte taky klient služby Azure AD, do kterého chcete registrace vaší aplikace. Pokud již, nemáte [zjistěte, jak získat](active-directory-howto-tenant.md).

## <a name="step-1-register-an-application-with-azure-ad"></a>Krok 1: Zaregistrujte aplikaci s Azure AD
K zabezpečení vaší aplikace, musíte nejprve vytvořit aplikaci v klientovi a Azure AD poskytují několik důležité informace.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).

2. Na horním panelu klikněte na váš účet. V **Directory** vyberte klienta Azure AD, kam chcete registrace vaší aplikace.

3. Klikněte na tlačítko **více služeb** v levém podokně a potom vyberte **Azure Active Directory**.

4. Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.

5. Postupujte podle výzev a vytvořte novou **webové aplikace nebo webové rozhraní API**.
  * **Název** popisuje vaší aplikace pro uživatele. Zadejte **seznam úkolů služby**.
  * **Identifikátor Uri pro přesměrování** je kombinace schéma a řetězec, Azure AD pomocí, která vrátí všechny tokeny, které vaše aplikace vyžaduje. Zadejte `https://localhost:44321/` pro tuto hodnotu.

6. Z **nastavení** -> **vlastnosti** stránky pro aplikace, aktualizujte identifikátor ID URI aplikace. Zadejte identifikátor konkrétního klienta. Zadejte například `https://contoso.onmicrosoft.com/TodoListService`.

7. Uložte konfiguraci. Ponechte portálu otevřené, protože musíte také zaregistrovat klientské aplikace za chvíli.

## <a name="step-2-set-up-the-app-to-use-the-owin-authentication-pipeline"></a>Krok 2: Nastavení aplikace pro použití ověřovacího kanálu OWIN
Chcete-li ověřit příchozí požadavky a tokeny, nastavte aplikaci komunikovat s Azure AD.

1. Pokud chcete začít, otevřete řešení a přidání balíčků NuGet middleware OWIN do projektu TodoListService pomocí konzoly Správce balíčků.

    ```
    PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
    PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
    ```

2. Přidejte třídu OWIN při spuštění do projektu TodoListService názvem `Startup.cs`.  Klikněte pravým tlačítkem na projekt, vyberte **přidat** > **nová položka**a poté vyhledejte **OWIN**. Middleware OWIN při spuštění vaší aplikace vyvolá metodu `Configuration(…)`.

3. Změňte deklaraci třídy k `public partial class Startup`. Již implementovali jsme součástí této třídy pro vás v jiném souboru. V `Configuration(…)` metoda, zkontrolujte zavolá `ConfgureAuth(…)` nastavení ověřování pro webovou aplikaci.

    ```C#
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
    ```

4. Otevřete soubor `App_Start\Startup.Auth.cs` a implementovat `ConfigureAuth(…)` metoda. Parametry, které zadáte v `WindowsAzureActiveDirectoryBearerAuthenticationOptions` bude sloužit jako souřadnice pro vaši aplikaci komunikovat s Azure AD.

    ```C#
    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseWindowsAzureActiveDirectoryBearerAuthentication(
            new WindowsAzureActiveDirectoryBearerAuthenticationOptions
            {
                Audience = ConfigurationManager.AppSettings["ida:Audience"],
                Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
            });
    }
    ```

5. Nyní můžete pomocí `[Authorize]` atributů k ochraně vašich řadičů a akce s ověřování nosiče tokenu pro webové JSON (JWT). Uspořádání `Controllers\TodoListController.cs` se značky autorizovat. Tato akce vynutí uživatele k přihlášení před přístupem k této stránce.

    ```C#
    [Authorize]
    public class TodoListController : ApiController
    {
    ```

    Když oprávnění volající úspěšně vyvolá jeden z `TodoListController` rozhraní API, akce může potřebovat přístup k informacím o volajícím. OWIN poskytuje přístup k deklarace identity uvnitř tokenu nosiče prostřednictvím `ClaimsPrincpal` objektu.  

6. Běžné požadavky pro webová rozhraní API jsou ověření „oborů“ v tokenu. Tím se zajistí, že uživatel souhlasí s tím oprávnění požadovaná pro přístup k provést seznamu službu.

    ```C#
    public IEnumerable<TodoItem> Get()
    {
        // user_impersonation is the default permission exposed by applications in Azure AD
        if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
        {
            throw new HttpResponseException(new HttpResponseMessage {
              StatusCode = HttpStatusCode.Unauthorized,
              ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found"
            });
        }
        ...
    }
    ```

7. Otevřete `web.config` souboru v kořenovém adresáři projektu TodoListService a zadejte svoje hodnoty konfigurace v `<appSettings>` oddílu.
  * `ida:Tenant`je název vašeho klienta Azure AD – například contoso.onmicrosoft.com.
  * `ida:Audience`je identifikátor ID URI aplikace aplikace, která jste zadali v portálu Azure.

## <a name="step-3-configure-a-client-application-and-run-the-service"></a>Krok 3: Konfigurace klientské aplikace a spustit službu
Než budete moct vidět na udělat seznamu službu v akci, budete muset nakonfigurovat klienta seznam úkolů, aby mohl získat tokeny z Azure AD a provádět volání do služby.

1. Přejděte zpět [portál Azure](https://portal.azure.com).

2. Vytvoření nové aplikace v klientovi služby Azure AD a vyberte **nativní klientská aplikace** výsledné řádku.
  * **Název** popisuje vaší aplikace pro uživatele.
  * Zadejte `http://TodoListClient/` pro **identifikátor Uri pro přesměrování** hodnotu.

3. Po dokončení registrace Azure AD jedinečný Identifikátor aplikace přiřadí vaší aplikace. Tuto hodnotu budete potřebovat v dalších krocích, zkopírujte jej ze stránky aplikace.

4. Z **nastavení** vyberte **požadovaných oprávnění**a potom vyberte **přidat**. Vyhledejte a vyberte k provést seznamu služby, přidejte **přístup TodoListService** oprávnění v rámci **delegovaná oprávnění**a potom klikněte na **provádí**.

5. V sadě Visual Studio otevřete `App.config` v TodoListClient projektu a pak zadejte svoje hodnoty konfigurace v `<appSettings>` oddílu.

  * `ida:Tenant`je název vašeho klienta Azure AD – například contoso.onmicrosoft.com.
  * `ida:ClientId`je ID aplikace, kterou jste zkopírovali z portálu Azure.
  * `todo:TodoListResourceId`je identifikátor ID URI aplikace do služby seznamu se aplikace, která jste zadali v portálu Azure.

## <a name="next-steps"></a>Další kroky
Nakonec vyčistit, sestavte a spusťte každý projekt. Pokud jste to ještě neudělali, nyní je čas vytvořit nového uživatele ve vašem klientovi s *. onmicrosoft.com domény. Přihlaste se na seznam úkolů klienta se tento uživatel a některé úlohy přidat do seznamu úkolů uživatele.

Pro srovnání je hotová ukázka (bez vašich hodnot nastavení) k dispozici v [Githubu](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip). Nyní se můžete přesunout další identity scénářů.

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
