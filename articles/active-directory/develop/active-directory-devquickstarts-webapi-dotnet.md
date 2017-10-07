---
title: "aaaAzure AD .NET webové rozhraní API Začínáme | Microsoft Docs"
description: "Jak: toobuild webového rozhraní API .NET MVC, který umožňuje integraci s Azure AD pro ověřování a autorizaci."
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
ms.openlocfilehash: 91c93e1fe18855f5648076e59e2ccf081eec34bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="help-protect-a-web-api-by-using-bearer-tokens-from-azure-ad"></a>Pomoc při ochraně webového rozhraní API pomocí nosných tokenů z Azure AD
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Pokud vytváříte aplikace, která poskytuje přístup tooprotected prostředky, je třeba tooknow jak tooprevent bude vyplacena neoprávněně přístup k prostředkům toothose.
Azure Active Directory (Azure AD) umožňuje jednoduché a přehledné toohelp chránit webového rozhraní API pomocí přístupových tokenů nosiče OAuth 2.0 jenom pár řádků kódu.

V aplikacích ASP.NET web můžete provést tuto ochranu pomocí implementace Microsoft hello hello komunitou vytvářený middleware OWIN zahrnuté v rozhraní .NET Framework 4.5. Zde použijeme toobuild OWIN web API "tooDo seznamu" který:

* Označí, které rozhraní API jsou chráněny.
* Ověří, že volání webového rozhraní API hello obsahovat platné přístupový token.

toobuild hello tooDo rozhraní API seznamu, je nejprve třeba:

1. Zaregistrovat aplikaci s Azure AD.
2. Nastavte hello aplikace toouse hello OWIN ověřovacího kanálu.
3. Konfigurace klienta aplikace toocall hello webového rozhraní API.

spuštění, tooget [stáhnout kostru aplikace hello](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) nebo [stažení ukázky hello Dokončit](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip). Každá je řešením sady Visual Studio 2013. Musíte taky klient služby Azure AD, ve které tooregister vaší aplikace. Pokud již, nemáte [zjistěte, jak tooget jeden](active-directory-howto-tenant.md).

## <a name="step-1-register-an-application-with-azure-ad"></a>Krok 1: Zaregistrujte aplikaci s Azure AD
toohelp zabezpečení vaší aplikace, je nejprve nutné toocreate aplikace ve vašem klientovi a Azure AD poskytují několik důležité informace.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).

2. Na horním panelu hello klikněte na váš účet. V hello **Directory** vyberte místo, kam chcete tooregister klienta hello Azure AD vaší aplikace.

3. Klikněte na tlačítko **více služeb** v levém podokně text hello a potom vyberte **Azure Active Directory**.

4. Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.

5. Postupujte podle pokynů hello a vytvořte novou **webové aplikace nebo webové rozhraní API**.
  * **Název** popisuje toousers vaší aplikace. Zadejte **tooDo seznamu služby**.
  * **Identifikátor Uri pro přesměrování** je kombinací schématu a řetězec používající tooreturn všechny tokeny, že vaše aplikace vyžaduje Azure AD. Zadejte `https://localhost:44321/` pro tuto hodnotu.

6. Z hello **nastavení** -> **vlastnosti** stránky pro aplikace, aktualizujte hello identifikátor ID URI aplikace. Zadejte identifikátor konkrétního klienta. Zadejte například `https://contoso.onmicrosoft.com/TodoListService`.

7. Uložte konfiguraci hello. Ponechte hello portál otevřít, protože budete také potřebovat tooregister klientské aplikace za chvíli.

## <a name="step-2-set-up-hello-app-toouse-hello-owin-authentication-pipeline"></a>Krok 2: Nastavení hello aplikace toouse hello OWIN ověřovacího kanálu
toovalidate příchozí požadavky a tokeny, musíte tooset až toocommunicate vaší aplikace s Azure AD.

1. toobegin, otevřete řešení hello a přidejte hello OWIN middleware NuGet balíčky toohello TodoListService projektu pomocí hello Konzola správce balíčků.

    ```
    PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
    PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
    ```

2. Přidat OWIN při spuštění třída toohello TodoListService projekt s názvem `Startup.cs`.  Klikněte pravým tlačítkem na projekt hello, vyberte **přidat** > **nová položka**a poté vyhledejte **OWIN**. Hello OWIN middleware vyvolá hello `Configuration(…)` metoda při spuštění aplikace.

3. Změňte deklaraci třídy hello příliš`public partial class Startup`. Již implementovali jsme součástí této třídy pro vás v jiném souboru. V hello `Configuration(…)` metody se volat příliš`ConfgureAuth(…)` tooset až ověřování pro webovou aplikaci.

    ```C#
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
    ```

4. Soubor otevřete hello `App_Start\Startup.Auth.cs` a implementovat hello `ConfigureAuth(…)` metoda. Hello parametry, které zadáte v `WindowsAzureActiveDirectoryBearerAuthenticationOptions` bude sloužit jako souřadnice toocommunicate vaší aplikace s Azure AD.

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

5. Nyní můžete pomocí `[Authorize]` atributy toohelp chránit řadiče a akce s ověřování nosiče tokenu pro webové JSON (JWT). Uspořádání hello `Controllers\TodoListController.cs` se značky autorizovat. Tato akce vynutí hello toosign uživatele v před přístupem k této stránce.

    ```C#
    [Authorize]
    public class TodoListController : ApiController
    {
    ```

    Když oprávnění volající úspěšně vyvolá jeden hello `TodoListController` rozhraní API, hello akce může potřebovat přístup k tooinformation o hello volajícího. OWIN poskytuje přístup toohello deklarace identity uvnitř tokenu nosiče hello prostřednictvím hello `ClaimsPrincpal` objektu.  

6. Běžné požadavky pro webové rozhraní API je toovalidate hello "obory" v hello token. Tím se zajistí, že tento hello uživatel souhlasí toohello oprávnění požadované tooaccess hello tooDo seznamu služby.

    ```C#
    public IEnumerable<TodoItem> Get()
    {
        // user_impersonation is hello default permission exposed by applications in Azure AD
        if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
        {
            throw new HttpResponseException(new HttpResponseMessage {
              StatusCode = HttpStatusCode.Unauthorized,
              ReasonPhrase = "hello Scope claim does not contain 'user_impersonation' or scope claim not found"
            });
        }
        ...
    }
    ```

7. Otevřete hello `web.config` souboru v kořenovém hello hello TodoListService projektu a zadejte hodnoty konfigurace v hello `<appSettings>` části.
  * `ida:Tenant`je název hello klienta služby Azure AD – například contoso.onmicrosoft.com.
  * `ida:Audience`identifikátor ID URI aplikace hello aplikace, kterou jste zadali v hello portál Azure je hello.

## <a name="step-3-configure-a-client-application-and-run-hello-service"></a>Krok 3: Konfigurace klientské aplikace a spustit službu hello
Než budete moct vidět hello tooDo seznamu služby v akci, musíte tooconfigure hello tooDo seznamu klienta, můžete získat tokeny z Azure AD a ujistěte se, volání toohello služby.

1. Přejděte zpět toohello [portál Azure](https://portal.azure.com).

2. Vytvoření nové aplikace v klientovi služby Azure AD a vyberte **nativní klientská aplikace** hello výsledné řádku.
  * **Název** popisuje toousers vaší aplikace.
  * Zadejte `http://TodoListClient/` pro hello **identifikátor Uri pro přesměrování** hodnotu.

3. Po dokončení registrace přiřadí Azure AD aplikace jedinečné ID tooyour aplikace. Tuto hodnotu budete potřebovat v dalších krocích hello, takže zkopírujte jej ze stránky aplikace hello.

4. Z hello **nastavení** vyberte **požadovaných oprávnění**a potom vyberte **přidat**. Vyhledejte a vyberte hello tooDo seznamu služby, přidejte hello **přístup TodoListService** oprávnění v rámci **delegovaná oprávnění**a potom klikněte na **provádí**.

5. V sadě Visual Studio otevřete `App.config` v hello TodoListClient projektu a potom zadejte hodnoty konfigurace v hello `<appSettings>` části.

  * `ida:Tenant`je název hello klienta služby Azure AD – například contoso.onmicrosoft.com.
  * `ida:ClientId`je hello ID aplikace, který jste zkopírovali ze hello portálu Azure.
  * `todo:TodoListResourceId`je hello identifikátor ID URI aplikace z hello tooDo aplikace seznamu služby, kterou jste zadali v hello portálu Azure.

## <a name="next-steps"></a>Další kroky
Nakonec vyčistit, sestavte a spusťte každý projekt. Pokud jste to ještě neudělali, nyní je čas toocreate hello nového uživatele ve vašem klientovi s *. onmicrosoft.com domény. Přihlášení toohello tooDo seznamu klienta s tímto uživatelem a přidejte některé úlohy toohello seznam úkolů uživatele.

Pro srovnání je k dispozici v hello dokončit ukázka (bez vašich hodnot nastavení) [Githubu](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip). Nyní se můžete přesunout na toomore identity scénáře.

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
