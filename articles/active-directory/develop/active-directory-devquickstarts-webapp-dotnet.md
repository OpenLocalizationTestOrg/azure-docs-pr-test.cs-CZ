---
title: "webové aplikace .NET aaaAzure AD Začínáme | Microsoft Docs"
description: "Vytvoření webové aplikace .NET MVC, která se integruje se službou Azure AD pro přihlášení."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: e15a41a4-dc5d-4c90-b3fe-5dc33b9a1e96
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 6d3098c9e3d7e1916ccb110c703f501ae52e788f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="aspnet-web-app-sign-in-and-sign-out-with-azure-ad"></a>Webové aplikace ASP.NET přihlášení a odhlášení s Azure AD
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Tím, že poskytuje jeden přihlášení a odhlášení jenom pár řádků kódu, Azure Active Directory (Azure AD) umožňuje snadno můžete toooutsource webové aplikace správy identit. Uživatelé a deaktivovat webové aplikace ASP.NET můžete přihlásit pomocí implementace Microsoft hello Open Web Interface pro .NET (OWIN) middleware. Rozhraní .NET Framework 4.5 je součástí komunitou vytvářený middleware OWIN. Tento článek ukazuje, jak toouse OWIN na:

* Přihlášení uživatelů tooweb aplikace pomocí Azure AD jako zprostředkovatele identity hello.
* Zobrazí některé informace o uživateli.
* Přihlášení uživatelů ze hello aplikací.

## <a name="before-you-get-started"></a>Než začnete
* Stáhnout hello [kostru aplikace](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) nebo stáhnout hello [hotová ukázka](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).
* Musíte taky klient služby Azure AD, ve které tooregister hello aplikace. Pokud ještě nemáte klient služby Azure AD [zjistěte, jak tooget jeden](active-directory-howto-tenant.md).

Až budete připravení, postupujte podle pokynů hello v hello další čtyři části.

## <a name="step-1-register-hello-new-app-with-azure-ad"></a>Krok 1: Zaregistrujte hello novou aplikaci s Azure AD
tooset si uživatelé tooauthenticate aplikace hello, nejprve zaregistrovat ve vašem klientovi pomocí tohoto postupu hello následující:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Na horním panelu hello klikněte na název účtu. V části hello **Directory** seznamu, vyberte hello klienta služby Active Directory místo tooregister hello aplikace.
3. Klikněte na tlačítko **více služeb** v levém podokně text hello a potom vyberte **Azure Active Directory**.
4. Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.
5. Postupujte podle hello vyzve toocreate nový **webové aplikace nebo WebAPI**.
  * **Název** popisuje toousers aplikace hello.
  * **Adresa URL přihlašování** je hello základní adresu URL aplikace hello. kostru Hello výchozí adresa URL je https://localhost:44320 /.
6. Po dokončení registrace hello, Azure AD přiřadí aplikace hello ID jedinečný aplikace. Zkopírujte hodnotu hello z toouse stránku hello aplikace v dalších částech hello.
7. Z hello **nastavení** -> **vlastnosti** stránky pro aplikace, aktualizujte hello identifikátor ID URI aplikace. Hello **identifikátor ID URI aplikace** je jedinečný identifikátor pro aplikaci hello. Hello nazevspolecnosti.technologickaoblast.nazevproduktu.funkcnioblast.nazev `https://<tenant-domain>/<app-name>` (například `https://contoso.onmicrosoft.com/my-first-aad-app`).

## <a name="step-2-set-up-hello-app-toouse-hello-owin-authentication-pipeline"></a>Krok 2: Nastavení hello aplikace toouse hello OWIN ověřovacího kanálu
V tomto kroku nakonfigurujete hello OWIN middleware toouse hello ověřovacího protokolu OpenID Connect. Použití požadavků na přihlášení a odhlášení tooissue OWIN, správě uživatelských relací, získání informací o uživateli a tak dále.

1. toobegin, přidejte hello OWIN middleware NuGet balíčky toohello projekt pomocí hello Konzola správce balíčků.

     ```
     PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
     PM> Install-Package Microsoft.Owin.Security.Cookies
     PM> Install-Package Microsoft.Owin.Host.SystemWeb
     ```

2. tooadd projekt toohello – třída aplikace OWIN při spuštění nazývá `Startup.cs`, klikněte pravým tlačítkem na projekt hello, vyberte **přidat**, vyberte **nová položka**a poté vyhledejte **OWIN**. Hello OWIN middleware vyvolá hello **Configuration(...)**  metoda při spuštění aplikace hello.
3. Změňte deklaraci třídy hello příliš`public partial class Startup`. Již implementovali jsme součástí této třídy pro vás v jiném souboru. V hello **Configuration(...)**  metody se volat příliš**ConfgureAuth(...)**  tooset až ověřování pro aplikaci hello.  

     ```C#
     public partial class Startup
     {
         public void Configuration(IAppBuilder app)
         {
             ConfigureAuth(app);
         }
     }
     ```

4. Otevřete soubor App_Start\Startup.Auth.cs hello a potom implementovat hello **ConfigureAuth(...)**  metoda. Hello parametry, které zadáte v *OpenIDConnectAuthenticationOptions* sloužit jako souřadnice toocommunicate aplikace hello s Azure AD. Musíte taky tooset až ověřování souborů cookie, protože middleware OpenID Connect hello používá soubory cookie v pozadí hello.

     ```C#
     public void ConfigureAuth(IAppBuilder app)
     {
         app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

         app.UseCookieAuthentication(new CookieAuthenticationOptions());

         app.UseOpenIdConnectAuthentication(
             new OpenIdConnectAuthenticationOptions
             {
                 ClientId = clientId,
                 Authority = authority,
                 PostLogoutRedirectUri = postLogoutRedirectUri,
                 Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = context =>
                        {
                            context.HandleResponse();
                            context.Response.Redirect("/Error?message=" + context.Exception.Message);
                            return Task.FromResult(0);
                        }
                    }
             });
     }
     ```

5. Otevřete soubor web.config hello v kořenovém hello hello projektu a potom zadejte hodnoty konfigurace hello v hello `<appSettings>` části.
  * `ida:ClientId`: hello GUID, které jste zkopírovali ze hello portál Azure "krok 1: registrace hello novou aplikaci s Azure AD."
  * `ida:Tenant`: hello název klienta služby Azure AD (například contoso.onmicrosoft.com).
  * `ida:PostLogoutRedirectUri`: hello indikátoru, která říká službě Azure AD, kde by měl být uživatel přesměrován po úspěšném dokončení žádost o odhlášení.

## <a name="step-3-use-owin-tooissue-sign-in-and-sign-out-requests-tooazure-ad"></a>Krok 3: Použití tooAzure AD požadavky OWIN tooissue přihlášení a odhlášení
Hello aplikace je nyní správně nakonfigurované toocommunicate s Azure AD pomocí ověřovacího protokolu OpenID Connect hello. OWIN má zpracovává všechny podrobnosti hello věnujte zpráv ověřování, ověřování tokenů z Azure AD a údržbě uživatelských relací. Všechno, co zůstane je toogive vaši uživatelé odhlaste se a způsob toosign v.

1. Můžete použít autorizovat značky v vaše řadiče toorequire uživatelé toosign v před přístupem k určité stránky. toodo Ano, otevřete Controllers\HomeController.cs a pak přidejte hello `[Authorize]` značky toohello o řadiče.

     ```C#
     [Authorize]
     public ActionResult About()
     {
       ...
     ```

2. Můžete taky OWIN toodirectly problém žádosti o ověření z vašeho kódu. toodo tak, že otevřete Controllers\AccountController.cs. Potom v hello SignIn() a SignOut() akce vydávat OpenID Connect výzvy a odhlášení požadavky.

     ```C#
     public void SignIn()
     {
         // Send an OpenID Connect sign-in request.
         if (!Request.IsAuthenticated)
         {
             HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
         }
     }
     public void SignOut()
     {
         // Send an OpenID Connect sign-out request.
         HttpContext.GetOwinContext().Authentication.SignOut(
              OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
     }
     ```

3. Otevřete Views\Shared\_LoginPartial.cshtml tooshow hello uživatele hello aplikace přihlášení a odhlášení odkazy a tooprint out hello uživatelské jméno v zobrazení.

    ```HTML
    @if (Request.IsAuthenticated)
    {
     <text>
         <ul class="nav navbar-nav navbar-right">
             <li class="navbar-text">
                 Hello, @User.Identity.Name!
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

## <a name="step-4-display-user-information"></a>Krok 4: Zobrazit informace o uživateli
Během ověřování uživatelů s OpenID Connect, Azure AD vrátí požadavku id_token toohello aplikaci, která obsahuje "deklarace identity" nebo tvrzení o hello uživatele. Můžete tyto deklarace identity toopersonalize hello aplikaci pomocí tohoto postupu hello následující:

1. Otevřete soubor Controllers\HomeController.cs hello. Deklarace identity uživatelů hello dostanete v řadičích prostřednictvím hello `ClaimsPrincipal.Current` zaregistrovaný objekt zabezpečení.

 ```C#
 public ActionResult About()
 {
     ViewBag.Name = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value;
     ViewBag.ObjectId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
     ViewBag.GivenName = ClaimsPrincipal.Current.FindFirst(ClaimTypes.GivenName).Value;
     ViewBag.Surname = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Surname).Value;
     ViewBag.UPN = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn).Value;

     return View();
 }
 ```

2. Sestavení a spuštění aplikace hello. Pokud již jste dosud nevytvořili nového uživatele ve vašem klientovi s doméně onmicrosoft.com, teď proto je toodo čas hello. Zde je uveden postup:

  a. Přihlaste se pomocí tohoto uživatele a Všimněte si, jak se identita uživatele hello projeví na horním panelu hello.

  b. Odhlásit se a přihlaste se zpět jako jiný uživatel ve vašem klientovi.

  c. Pokud se cítíte zvlášť ambiciózní, zaregistrujte a spustit jiná instance této aplikace (pomocí vlastní clientId) a podívejte se na jednom přihlášení v akci.

## <a name="next-steps"></a>Další kroky
Odkaz, najdete v části [hello dokončit ukázka](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip) (bez vašich hodnot nastavení).

Nyní se můžete přesunout na toomore advanced témata. Zkuste například [zabezpečení webového rozhraní API s Azure AD](active-directory-devquickstarts-webapi-dotnet.md).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
