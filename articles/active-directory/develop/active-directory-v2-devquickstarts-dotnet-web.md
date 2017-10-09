---
title: "aaaAzure AD v2.0 .NET webové přihlášení aplikace Začínáme | Microsoft Docs"
description: "Jak toobuild webové aplikace .NET MVC, přihlášení uživatelů s Account Microsoft i osobní a pracovní nebo školní účty."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: c8b97ac6-0a06-4367-81b6-7d1d98152b14
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 241e9c90bd752fbecc3696ce4f1bed3f9772189d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooan-net-mvc-web-app"></a>Přidání přihlášení tooan .NET MVC webové aplikace
S koncovým bodem v2.0 hello můžete rychle přidat ověřování tooyour webové aplikace s podporou pro oba osobní účty Microsoft a pracovní nebo školní účty.  V aplikacích ASP.NET web můžete to provést pomocí middlewaru OWIN společnosti Microsoft, zahrnutá v rozhraní .NET Framework 4.5.

> [!NOTE]
> Ne všechny scénáře Azure Active Directory a funkce jsou podporovány koncového bodu v2.0 hello.  toodetermine Pokud byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).
>
>

 Zde jsme budete vytvářet webové aplikace, která používá OWIN toosign hello uživatele v zobrazení některé informace o uživateli hello a přihlašovací hello uživatele mimo aplikaci hello.

## <a name="download"></a>Ke stažení
Hello kód v tomto kurzu se udržuje [na Githubu](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet).  toofollow společně, můžete [stáhnout kostru aplikace hello jako ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) nebo hello kostru klonovat:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

aplikace Hello dokončit, je k dispozici na konci hello také tohoto kurzu.

## <a name="register-an-app"></a>Registrace aplikace
Vytvoření nové aplikace v [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo postupujte podle těchto [podrobné kroky](active-directory-v2-app-registration.md).  Zkontrolujte, že:

* Poznamenejte hello **Id aplikace** přiřazen tooyour aplikace, budete ho potřebovat brzy k dispozici.
* Přidat hello **webové** platformu pro vaši aplikaci.
* Zadejte správný hello **identifikátor URI pro přesměrování**. Hello identifikátor uri přesměrování označuje tooAzure AD, kde se mají směrovat ověřování odpovědí – hello výchozí hodnota pro tento kurz je `https://localhost:44326/`.

## <a name="install--configure-owin-authentication"></a>Instalace a konfigurace ověřování OWIN.
Zde nakonfigurujeme hello OWIN middleware toouse hello ověřovacího protokolu OpenID Connect.  OWIN bude použité tooissue požadavků na přihlášení a odhlášení, spravovat hello uživatelské relace a získat informace o uživateli hello, mimo jiné.

1. toobegin, otevřete hello `web.config` v kořenovém hello hello projektu soubor a zadejte hodnoty konfigurace vaší aplikace v hello `<appSettings>` části.

  * Hello `ida:ClientId` je hello **Id aplikace** přiřazené tooyour aplikace v portálu pro registraci hello.
  * Hello `ida:RedirectUri` je hello **identifikátor Uri pro přesměrování** jste zadali v portálu hello.

2. Dál přidejte hello OWIN middleware NuGet balíčky toohello projekt pomocí hello Konzola správce balíčků.

        ```
        PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
        PM> Install-Package Microsoft.Owin.Security.Cookies
        PM> Install-Package Microsoft.Owin.Host.SystemWeb
        ```  

3. Přidat projekt aplikace "Třídy pro spuštění OWIN" toohello nazývá `Startup.cs` správné, klikněte na projekt hello--> **přidat** --> **nová položka** --> vyhledejte "OWIN".  Hello OWIN middleware vyvolá hello `Configuration(...)` metoda při spuštění aplikace.
4. Změňte deklaraci třídy hello příliš`public partial class Startup` -již implementovali jsme součástí této třídy pro vás v jiném souboru.  V hello `Configuration(...)` metoda, ujistěte se, volání tooConfigureAuth(...) tooset až ověřování pro webovou aplikaci  

        ```C#
        [assembly: OwinStartup(typeof(Startup))]
        
        namespace TodoList_WebApp
        {
            public partial class Startup
            {
                public void Configuration(IAppBuilder app)
                {
                    ConfigureAuth(app);
                }
            }
        }
        ```

5. Soubor otevřete hello `App_Start\Startup.Auth.cs` a implementovat hello `ConfigureAuth(...)` metoda.  Hello parametry, které zadáte v `OpenIdConnectAuthenticationOptions` bude sloužit jako souřadnice toocommunicate vaší aplikace s Azure AD.  Budete také potřebovat tooset až ověřování souborů Cookie – používá soubory cookie pod hello zahrnuje, OpenID Connect middleware hello.

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
                                             Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0"),
                                             RedirectUri = redirectUri,
                                             Scope = "openid email profile",
                                             ResponseType = "id_token",
                                             PostLogoutRedirectUri = redirectUri,
                                             TokenValidationParameters = new TokenValidationParameters
                                             {
                                                     ValidateIssuer = false,
                                             },
                                             Notifications = new OpenIdConnectAuthenticationNotifications
                                             {
                                                     AuthenticationFailed = OnAuthenticationFailed,
                                             }
                                     });
                     }
        ```

## <a name="send-authentication-requests"></a>Odeslání žádosti o ověření
Aplikace je nyní správně nakonfigurované toocommunicate s koncovým bodem v2.0 hello pomocí ověřovacího protokolu OpenID Connect hello.  OWIN má postaráno všechny hello ugly podrobnosti o věnujte zpráv ověřování, ověřování tokenů z Azure AD a údržbě uživatelské relace.  Všechno, co zůstane je toogive vaši uživatelé odhlaste se a způsob toosign v.

- Můžete použít autorizovat značky v vaše řadiče toorequire tento uživatel přihlásí před přístupem k určité stránky.  Otevřete `Controllers\HomeController.cs`a přidejte hello `[Authorize]` značky toohello o řadiče.
        
        ```C#
        [Authorize]
        public ActionResult About()
        {
          ...
        ```

- Můžete taky OWIN toodirectly problém žádosti o ověření z vašeho kódu.  Otevřete `Controllers\AccountController.cs`.  V hello SignIn() a SignOut() akce vydejte OpenID Connect výzvy a odhlášení požadavků, v uvedeném pořadí.

        ```C#
        public void SignIn()
        {
            // Send an OpenID Connect sign-in request.
            if (!Request.IsAuthenticated)
            {
                HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
            }
        }
        
        // BUGBUG: Ending a session with hello v2.0 endpoint is not yet supported.  Here, we just end hello session with hello web app.  
        public void SignOut()
        {
            // Send an OpenID Connect sign-out request.
            HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
            Response.Redirect("/");
        }
        ```

- Nyní otevřete `Views\Shared\_LoginPartial.cshtml`.  Toto je, kde můžete zobrazit uživatele hello odkazy přihlášení a odhlášení vaší aplikace a vytiskněte hello uživatelské jméno v zobrazení.

        ```HTML
        @if (Request.IsAuthenticated)
        {
            <text>
                <ul class="nav navbar-nav navbar-right">
                    <li class="navbar-text">
        
                        @*hello 'preferred_username' claim can be used for showing hello user's primary way of identifying themselves.*@
        
                        Hello, @(System.Security.Claims.ClaimsPrincipal.Current.FindFirst("preferred_username").Value)!
                    </li>
                    <li>
                        @Html.ActionLink("Sign out", "SignOut", "Account")
                    </li>
                </ul>
            </text>
        }
        else
        {
            <ul class="nav navbar-nav navbar-right">
                <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
            </ul>
        }
        ```

## <a name="display-user-information"></a>Zobrazit informace o uživateli
Při ověřování uživatelů s OpenID Connect, vrátí koncového bodu v2.0 hello požadavku id_token toohello aplikaci, která obsahuje deklarace identity nebo tvrzení o hello uživatele.  Můžete použít tyto deklarace identity toopersonalize aplikace:

- Otevřete hello `Controllers\HomeController.cs` souboru.  Deklarace identity uživatelů hello dostanete v řadičích prostřednictvím hello `ClaimsPrincipal.Current` zaregistrovaný objekt zabezpečení.

        ```C#
        [Authorize]
        public ActionResult About()
        {
            ViewBag.Name = ClaimsPrincipal.Current.FindFirst("name").Value;
        
            // hello object ID claim will only be emitted for work or school accounts at this time.
            Claim oid = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier");
            ViewBag.ObjectId = oid == null ? string.Empty : oid.Value;
        
            // hello 'preferred_username' claim can be used for showing hello user's primary way of identifying themselves
            ViewBag.Username = ClaimsPrincipal.Current.FindFirst("preferred_username").Value;
        
            // hello subject or nameidentifier claim can be used toouniquely identify hello user
            ViewBag.Subject = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        
            return View();
        }
        ```

## <a name="run"></a>Spusťte
Nakonec sestavte a spusťte aplikaci!   Přihlaste se pomocí osobního Account Microsoft nebo pracovní nebo školní účet a Všimněte si, jak identitu uživatele hello se odrazí v horním navigačním panelu hello.  Nyní máte webovou aplikaci zabezpečené pomocí standardních oborových protokolech, které může ověřit uživatele s svoje osobní, tak i pracovní nebo školní účty.

Pro referenci hello dokončit ukázka (bez vašich hodnot nastavení) [je k dispozici jako ZIP zde](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip), nebo ji můžete klonovat z Githubu:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

## <a name="next-steps"></a>Další kroky
Nyní se můžete přesunout na pokročilejší témata.  Může být vhodné tootry:

[Zabezpečení webového rozhraní API s koncovým bodem v2.0 hello hello >>](active-directory-devquickstarts-webapi-dotnet.md)

Další zdroje projděte si:

* [Příručka vývojáře v2.0 Hello >>](active-directory-appmodel-v2-overview.md)
* [Značka StackOverflow "azure-active-directory" >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Získejte bezpečnostní aktualizace našich produktů
Doporučujeme vám tooget oznámení o bezpečnostních incidentech navštivte stránky [tuto stránku](https://technet.microsoft.com/security/dd252948) a přihlášení k odběru tooSecurity Advisory Alerts.
