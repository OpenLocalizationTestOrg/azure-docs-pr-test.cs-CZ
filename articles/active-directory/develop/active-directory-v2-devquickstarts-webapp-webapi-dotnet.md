---
title: "aaaAzure AD v2.0 .NET webové aplikace volání rozhraní API, Začínáme | Microsoft Docs"
description: "Jak toobuild webové aplikace MVC .NET, která volá webové služby pomocí Microsoft osobní účty a pracovní nebo školní účty pro přihlášení."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 56be906e-71de-469d-9a5c-9fc08aae4223
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 1a70791418bc2a7d1fdfbafb9b5126a033a32292
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="calling-a-web-api-from-a-net-web-app"></a>Volání webového rozhraní API z webové aplikace .NET
S koncovým bodem v2.0 hello můžete rychle přidat ověřování tooyour webové aplikace a webové rozhraní API s podporou pro oba osobní účty Microsoft a pracovní nebo školní účty.  Zde jsme budete vytvářet webové aplikace MVC, která podepisuje uživatele pomocí OpenID Connect, s pomoc od společnosti Microsoft OWIN middleware.  Hello webové aplikace se získání přístupových tokenů OAuth 2.0 pro webové rozhraní api, které jsou zabezpečeny OAuth 2.0, který umožňuje vytvořit, číst a odstranit na daného uživatele "seznam úkolů".

V tomto kurzu se soustředí hlavně na pomocí MSAL tooacquire a pomocí přístupových tokenů ve webové aplikaci, popsané v úplné [zde](active-directory-v2-flows.md#web-apps).  Jako požadavky, můžete toofirst zjistěte, jak příliš[přidat základní tooa přihlašovací webovou aplikaci](active-directory-v2-devquickstarts-dotnet-web.md) nebo jak příliš[správně zabezpečení webového rozhraní API](active-directory-v2-devquickstarts-dotnet-api.md).

> [!NOTE]
> Ne všechny scénáře Azure Active Directory a funkce jsou podporovány koncového bodu v2.0 hello.  toodetermine Pokud byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).
> 
> 

## <a name="download-sample-code"></a>Stáhněte si ukázkový kód
Hello kód v tomto kurzu se udržuje [na Githubu](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet).  toofollow společně, můžete [stáhnout kostru aplikace hello jako ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) nebo hello kostru klonovat:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

Alternativně můžete [stáhnout aplikaci hello dokončit jako ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) nebo klonování hello dokončit aplikace:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

## <a name="register-an-app"></a>Registrace aplikace
Vytvoření nové aplikace v [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo postupujte podle těchto [podrobné kroky](active-directory-v2-app-registration.md).  Zkontrolujte, že:

* Poznamenejte hello **Id aplikace** přiřazen tooyour aplikace, budete ho potřebovat brzy k dispozici.
* Vytvoření **tajný klíč aplikace** Dobrý den **heslo** typu a poznamenejte jeho hodnoty pro později
* Přidat hello **webové** platformu pro vaši aplikaci.
* Zadejte správný hello **identifikátor URI pro přesměrování**. Hello identifikátor uri přesměrování označuje tooAzure AD, kde se mají směrovat ověřování odpovědí – hello výchozí hodnota pro tento kurz je `https://localhost:44326/`.

## <a name="install-owin"></a>Instalace OWIN
Přidat hello OWIN middleware NuGet balíčky toohello `TodoList-WebApp` projekt pomocí hello Konzola správce balíčků.  Hello OWIN middleware bude použité tooissue požadavků na přihlášení a odhlášení, spravovat hello uživatelské relace a získat informace o uživateli hello, mimo jiné.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoList-WebApp
```

## <a name="sign-hello-user-in"></a>Přihlášení uživatele hello v
Teď nakonfigurovat hello OWIN middleware toouse hello [ověřovacího protokolu OpenID Connect](active-directory-v2-protocols.md).  

* Otevřete hello `web.config` soubor v kořenovém adresáři hello hello `TodoList-WebApp` projektu a zadejte hodnoty konfigurace vaší aplikace v hello `<appSettings>` části.
  * Hello `ida:ClientId` je hello **Id aplikace** přiřazené tooyour aplikace v portálu pro registraci hello.
  * Hello `ida:ClientSecret` je hello **tajný klíč aplikace** jste vytvořili v portálu pro registraci hello.
  * Hello `ida:RedirectUri` je hello **identifikátor Uri pro přesměrování** jste zadali v portálu hello.
* Otevřete hello `web.config` soubor v kořenovém adresáři hello hello `TodoList-Service` projektu a nahraďte hello `ida:Audience` s hello stejné **Id aplikace** jako výše.
* Soubor otevřete hello `App_Start\Startup.Auth.cs` a přidejte `using` příkazy pro knihovny hello z výše.
* V hello stejného souboru, implementovat hello `ConfigureAuth(...)` metoda.  Hello parametry, které zadáte v `OpenIDConnectAuthenticationOptions` bude sloužit jako souřadnice toocommunicate vaší aplikace s Azure AD.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {

                    // hello `Authority` represents hello v2.0 endpoint - https://login.microsoftonline.com/common/v2.0
                    // hello `Scope` describes hello permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                    // In a real application you could use issuer validation for additional checks, like making sure hello user's organization has signed up for your app, for instance.

                    ClientId = clientId,
                    Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0 "),
                    Scope = "openid email profile offline_access",
                    RedirectUri = redirectUri,
                    PostLogoutRedirectUri = redirectUri,
                    TokenValidationParameters = new TokenValidationParameters
                    {
                        ValidateIssuer = false,
                    },

                    // hello `AuthorizationCodeReceived` notification is used toocapture and redeem hello authorization_code that hello v2.0 endpoint returns tooyour app.

                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed,
                        AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    }

        });
}
// ...
```

## <a name="use-msal-tooget-access-tokens"></a>Použít MSAL tooget přístupové tokeny
V hello `AuthorizationCodeReceived` oznámení, chceme toouse [OAuth 2.0 v kombinaci s OpenID Connect](active-directory-v2-protocols.md) tooredeem hello authorization_code pro token toohello k přístupu služby seznamu úkolů.  MSAL můžete tento proces snadno pro vás provedl:

* Nejdřív nainstalujte verzi preview hello MSAL:

```PM> Install-Package Microsoft.Identity.Client -ProjectName TodoList-WebApp -IncludePrerelease```

* A přidat další `using` příkaz toohello `App_Start\Startup.Auth.cs` MSAL v souboru.
* Nyní přidejte nová metoda hello `OnAuthorizationCodeReceived` obslužné rutiny události.  Tato obslužná rutina použije MSAL tooacquire tokenu toohello přístupu k rozhraní API seznamu úkolů a uloží hello token v mezipaměti na MSAL tokenu pro pozdější:

```C#
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
        string userObjectId = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        string tenantID = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
        string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenantID, string.Empty);
        ClientCredential cred = new ClientCredential(clientId, clientSecret);

        // Here you ask for a token using hello web app's clientId as hello scope, since hello web app and service share hello same clientId.
        app = new ConfidentialClientApplication(Startup.clientId, redirectUri, cred, new NaiveSessionCache(userObjectId, notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase)) {};
        var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { clientId }, notification.Code);
}
// ...
```

* Ve službě web apps má MSAL extensible mezipamětí tokenů, které můžou být použité toostore tokeny.  Tato ukázka implementuje hello `NaiveSessionCache` tokeny toocache úložiště relace http, který používá.

<!-- TODO: Token Cache article -->


## <a name="call-hello-web-api"></a>Hello volání webového rozhraní API
Nyní je čas tooactually použít hello access_token, které jste získali v kroku 3.  Otevřete hello webové aplikace `Controllers\TodoListController.cs` souboru, takže všechny hello CRUD požadavky toohello rozhraní API seznamu úkolů.

* Můžete použít MSAL znovu zde toofetch access_tokens z hello MSAL mezipaměti.  Nejprve přidejte `using` příkaz MSAL toothis souboru.
  
    `using Microsoft.Identity.Client;`
* V hello `Index` akce, použijte MSAL `AcquireTokenSilentAsync` metoda tooget access_token, kterou lze použít tooread data z hello seznam úkolů služby:

```C#
// ...
string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
string tenantID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
string authority = String.Format(CultureInfo.InvariantCulture, Startup.aadInstance, tenantID, string.Empty);
ClientCredential credential = new ClientCredential(Startup.clientId, Startup.clientSecret);

// Here you ask for a token using hello web app's clientId as hello scope, since hello web app and service share hello same clientId.
app = new ConfidentialClientApplication(Startup.clientId, redirectUri, credential, new NaiveSessionCache(userObjectID, this.HttpContext)){};
result = await app.AcquireTokenSilentAsync(new string[] { Startup.clientId });
// ...
```

* Hello ukázka potom přidá hello výsledný token toohello požadavek HTTP GET jako hello `Authorization` záhlaví, seznam úkolů služby, která hello používá tooauthenticate hello požadavku.
* Pokud vrátí hello seznam úkolů služby `401 Unauthorized` odpovědi, hello access_tokens v MSAL mít zneplatní z nějakého důvodu.  V takovém případě by měl vyřaďte všechny access_tokens z hello MSAL mezipaměti a hello uživatelům zobrazí zpráva, že potřebují toosign v akci, která restartuje tok tokenu pořízení hello.

```C#
// ...
// If hello call failed with access denied, then drop hello current access token from hello cache,
// and show hello user an error indicating they might need toosign-in again.
if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
{
        app.AppTokenCache.Clear(Startup.clientId);

        return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need toosign in again.");
}
// ...
```

* Podobně pokud MSAL nelze tooreturn access_token z jakéhokoli důvodu, byste měli vyzvat, hello toosign uživatele v akci.  To je jednoduché, zachytávání žádné `MSALException`:

```C#
// ...
catch (MsalException ee)
{
        // If MSAL could not get a token silently, show hello user an error indicating they might need toosign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading tooDo List: " + ee.Message + " You might need toolog out and log back in.");
}
// ...
```

* Hello přesnou stejné `AcquireTokenSilentAsync` volání je implementd v hello `Create` a `Delete` akce.  Ve službě web apps můžete použít tento MSAL metoda tooget access_tokens kdykoli budete potřebovat ve vaší aplikaci.  MSAL se postará o získávání, ukládání do mezipaměti a aktualizaci tokeny pro vás.

Nakonec sestavte a spusťte aplikaci!  Přihlaste se pomocí Account Microsoft nebo účtu Azure AD a Všimněte si, jak identitu uživatele hello se odrazí v horním navigačním panelu hello.  Přidat a odstranit některé položky ze seznamu úkolů uživatele hello hello toosee OAuth 2.0 zabezpečené rozhraní API se volá v akci.  Nyní máte webovou aplikaci & webového rozhraní API, jak zabezpečit pomocí standardních protokolů, které může ověřit uživatele s svoje osobní, tak i pracovní nebo školní účty.

Pro referenci hello dokončit ukázka (bez vašich hodnot nastavení) [je tady uvedené](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip).  

## <a name="next-steps"></a>Další kroky
Další zdroje projděte si:

* [Příručka vývojáře v2.0 Hello >>](active-directory-appmodel-v2-overview.md)
* [Značka StackOverflow "azure-active-directory" >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Získejte bezpečnostní aktualizace našich produktů
Doporučujeme vám tooget oznámení o bezpečnostních incidentech navštivte stránky [tuto stránku](https://technet.microsoft.com/security/dd252948) a přihlášení k odběru tooSecurity Advisory Alerts.

