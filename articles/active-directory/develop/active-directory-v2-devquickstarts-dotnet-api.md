---
title: "pomocí přihlášení tooa webového rozhraní API .NET MVC aaaAdd hello koncového bodu v2.0 Azure AD | Microsoft Docs"
description: "Jak toobuild .NET MVC webové rozhraní Api, které přijímá tokeny z Account Microsoft i osobní a pracovní nebo školní účty."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: e77bc4e0-d0c9-4075-a3f6-769e2c810206
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 4e517145422bb6e9368e82a7eef4a5c57cce530a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="secure-an-mvc-web-api"></a>Zabezpečení webového rozhraní API MVC
S koncovým bodem v2.0 hello Azure Active Directory, budete moci chránit webového rozhraní API pomocí [OAuth 2.0](active-directory-v2-protocols.md) přístupových tokenů, povolení uživatelé s i osobní účet Microsoft a pracovní nebo školní účty toosecurely přístup vašeho webového rozhraní API.

> [!NOTE]
> Ne všechny scénáře Azure Active Directory a funkce jsou podporovány koncového bodu v2.0 hello.  toodetermine Pokud byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).
>
>

V rozhraní ASP.NET web API můžete to provést pomocí middlewaru OWIN společnosti Microsoft, zahrnutá v rozhraní .NET Framework 4.5.  Zde použijeme toobuild OWIN Web API MVC "tooDo seznamu", která umožňuje klientům úlohy toocreate a čtení ze seznamu úkolů uživatele.  Hello webového rozhraní API ověří, že příchozí požadavky obsahovat platné přístupový token a odmítnout všechny požadavky, které nepředávejte ověření na chráněných trase.  Tato ukázka byla vytvořená s využitím sady Visual Studio 2015.

## <a name="download"></a>Ke stažení
Hello kód v tomto kurzu se udržuje [na Githubu](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet).  toofollow společně, můžete [stáhnout kostru aplikace hello jako ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) nebo hello kostru klonovat:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

kostru aplikace Hello zahrnuje všechny hello často používaný kód pro jednoduché rozhraní API, ale chybí všechny součásti související s identity hello. Pokud nechcete, aby toofollow společně, můžete místo toho klonovat nebo [stažení ukázky hello Dokončit](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip).

```
git clone https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

## <a name="register-an-app"></a>Registrace aplikace
Vytvoření nové aplikace v [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo postupujte podle těchto [podrobné kroky](active-directory-v2-app-registration.md).  Zkontrolujte, že:

* Poznamenejte hello **Id aplikace** přiřazen tooyour aplikace, budete ho potřebovat brzy k dispozici.

Toto řešení sady visual studio také obsahuje "TodoListClient", což je jednoduchou aplikaci WPF.  Hello TodoListClient je použité toodemonstrate jak uživatel přihlásí, a jak můžete vydat klient požaduje tooyour webového rozhraní API.  V takovém případě hello TodoListClient i hello TodoListService jsou reprezentované pomocí hello stejná aplikace.  tooconfigure hello TodoListClient, měli byste také:

* Přidat hello **Mobile** platformu pro vaši aplikaci.

## <a name="install-owin"></a>Instalace OWIN
Teď, když jste registrováni aplikace, musíte tooset až toocommunicate vaší aplikace s koncovým bodem v2.0 hello v pořadí toovalidate příchozí žádosti a tokeny.

* toobegin, otevřete řešení hello a přidejte hello OWIN middleware NuGet balíčky toohello TodoListService projekt pomocí hello Konzola správce balíčků.

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
PM> Install-Package Microsoft.IdentityModel.Protocol.Extensions -ProjectName TodoListService
```

## <a name="configure-oauth-authentication"></a>Konfigurace ověřování OAuth
* Přidat OWIN při spuštění třída toohello TodoListService projekt s názvem `Startup.cs`.  Klikněte pravým tlačítkem na projekt hello--> **přidat** --> **nová položka** --> vyhledejte "OWIN".  Hello OWIN middleware vyvolá hello `Configuration(…)` metoda při spuštění aplikace.
* Změňte deklaraci třídy hello příliš`public partial class Startup` -již implementovali jsme součástí této třídy pro vás v jiném souboru.  V hello `Configuration(…)` metoda, ujistěte se, volání tooConfgureAuth(...) tooset až ověřování pro webovou aplikaci.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

* Soubor otevřete hello `App_Start\Startup.Auth.cs` a implementovat hello `ConfigureAuth(…)` metodu, která bude nastavit hello webového rozhraní API tooaccept tokeny z koncového bodu v2.0 hello.

```C#
public void ConfigureAuth(IAppBuilder app)
{
        var tvps = new TokenValidationParameters
        {
                // In this app, hello TodoListClient and TodoListService
                // are represented using hello same Application Id - we use
                // hello Application Id toorepresent hello audience, or the
                // intended recipient of tokens.

                ValidAudience = clientId,

                // In a real applicaiton, you might use issuer validation to
                // verify that hello user's organization (if applicable) has
                // signed up for hello app.  Here, we'll just turn it off.

                ValidateIssuer = false,
        };

        // Set up hello OWIN pipeline toouse OAuth 2.0 Bearer authentication.
        // hello options provided here tell hello middleware about hello type of tokens
        // that will be recieved, which are JWTs for hello v2.0 endpoint.

        // NOTE: hello usual WindowsAzureActiveDirectoryBearerAuthenticaitonMiddleware uses a
        // metadata endpoint which is not supported by hello v2.0 endpoint.  Instead, this
        // OpenIdConenctCachingSecurityTokenProvider can be used toofetch & use hello OpenIdConnect
        // metadata document.

        app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
        {
                AccessTokenFormat = new Microsoft.Owin.Security.Jwt.JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider("https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration")),
        });
}
```

* Nyní můžete pomocí `[Authorize]` atributy tooprotect řadiče a akce s ověřování nosiče OAuth 2.0.  Uspořádání hello `Controllers\TodoListController.cs` se značky autorizovat.  Tato akce vynutí hello toosign uživatele v před přístupem k této stránce.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

* Když oprávnění volající úspěšně vyvolá jeden hello `TodoListController` rozhraní API, hello akce může potřebovat přístup k tooinformation o hello volajícího.  OWIN poskytuje přístup toohello deklarace identity uvnitř tokenu nosiče hello prostřednictvím hello `ClaimsPrincpal` objektu.  

```C#
public IEnumerable<TodoItem> Get()
{
    // You can use hello ClaimsPrincipal tooaccess information about the
    // user making hello call.  In this case, we use hello 'sub' or
    // NameIdentifier claim tooserve as a key for hello tasks in hello data store.

    Claim subject = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier);

    return from todo in todoBag
           where todo.Owner == subject.Value
           select todo;
}
```

* Nakonec otevřete hello `web.config` souboru v kořenovém hello hello TodoListService projektu a zadejte hodnoty konfigurace v hello `<appSettings>` části.
  * Vaše `ida:Audience` je hello **Id aplikace** hello aplikace, kterou jste zadali v portálu hello.

## <a name="configure-hello-client-app"></a>Konfigurace klienta aplikace hello
Než budete moct vidět hello služby seznamu úkolů v akci, je nutné tooconfigure hello klienta seznamu úkolů, můžete získat tokeny z koncového bodu v2.0 hello a ujistěte se, volání toohello služby.

* V projektu TodoListClient hello otevřete `App.config` a zadejte hodnoty konfigurace v hello `<appSettings>` části.
  * Vaše `ida:ClientId` Id aplikace, které jste zkopírovali z portálu hello.

Nakonec vyčistit, sestavte a spusťte každý projekt!  Nyní máte rozhraní .NET MVC webové rozhraní API, které přijímá tokeny z obou osobní účty Microsoft a pracovní nebo školní účty.  Přihlaste se k hello TodoListClient a volání webového rozhraní api tooadd úlohy toohello uživatele na seznam úkolů.

Pro referenci hello dokončit ukázka (bez vašich hodnot nastavení) [je k dispozici jako ZIP zde](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip), nebo ji můžete klonovat z Githubu:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git```

## <a name="next-steps"></a>Další kroky
Nyní se můžete přesunout na další témata.  Může být vhodné tootry:

[Volání webového rozhraní API z webové aplikace >>](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)

Další zdroje projděte si:

* [Příručka vývojáře v2.0 Hello >>](active-directory-appmodel-v2-overview.md)
* [Značka StackOverflow "azure-active-directory" >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Získejte bezpečnostní aktualizace našich produktů
Doporučujeme vám tooget oznámení o bezpečnostních incidentech navštivte stránky [tuto stránku](https://technet.microsoft.com/security/dd252948) a přihlášení k odběru tooSecurity Advisory Alerts.
