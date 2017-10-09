---
title: "aaaAzure AD v2 ASP.NET Web Server Začínáme - použití | Microsoft Docs"
description: "Implementace přihlašování společnosti Microsoft na řešení technologie ASP.NET s tradiční webovou aplikací využívajících prohlížeč pomocí OpenID Connect standard"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 03afce6fa6598215e8c4af841c00762c143a0cd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="add-a-controller-toohandle-sign-in-and-sign-out-requests"></a>Přidejte žádosti řadič toohandle přihlášení a odhlášení

Tento krok ukazuje, jak toocreate nový řadič tooexpose přihlášení a odhlášení metody.

1.  Klikněte pravým tlačítkem na hello `Controllers` složky a vyberte`Add` > `Controller`
2.  Vyberte `MVC (.NET version) Controller – Empty`.
3.  Klikněte na tlačítko *přidat*
4.  Pojmenujte ji `HomeController` a klikněte na tlačítko *přidat*
5.  Přidat *OWIN* odkazuje na třídu toohello:

```csharp
using Microsoft.Owin.Security;
using Microsoft.Owin.Security.Cookies;
using Microsoft.Owin.Security.OpenIdConnect;
```
<!-- Workaround for Docs conversion bug -->
<ol start="6">
<li>
Přidáte dvě metody hello níže toohandle přihlášení a odhlášení tooyour řadiče pomocí inicializace výzvu ověřování prostřednictvím kódu:
</li>
</ol>

```csharp
/// <summary>
/// Send an OpenID Connect sign-in request.
/// Alternatively, you can just decorate hello SignIn method with hello [Authorize] attribute
/// </summary>
public void SignIn()
{
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties{ RedirectUri = "/" },
            OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}

/// <summary>
/// Send an OpenID Connect sign-out request.
/// </summary>
public void SignOut()
{
    HttpContext.GetOwinContext().Authentication.SignOut(
            OpenIdConnectAuthenticationDefaults.AuthenticationType,
            CookieAuthenticationDefaults.AuthenticationType);
}
```

## <a name="create-hello-apps-home-page-toosign-in-users-via-a-sign-in-button"></a>Vytvoření aplikace hello domovskou stránku toosign v uživatele prostřednictvím přihlášení tlačítko

V sadě Visual Studio vytvořte nové zobrazení tooadd hello přihlašovací tlačítko a zobrazit informace o uživateli po ověření:

1.  Klikněte pravým tlačítkem na hello `Views\Home` složky a vyberte`Add View`
2.  Pojmenujte ji `Index`.
3.  Přidejte následující HTML, který obsahuje hello přihlašovací tlačítko toohello souboru hello:

```html
<html>
<head>
    <meta name="viewport" content="width=device-width" />
    <title>Sign-In with Microsoft Guide</title>
</head>
<body>
@if (!Request.IsAuthenticated)
{
    <!-- If hello user is not authenticated, display hello sign-in button -->
    <a href="@Url.Action("SignIn", "Home")" style="text-decoration: none;">
        <svg xmlns="http://www.w3.org/2000/svg" xml:space="preserve" width="300px" height="50px" viewBox="0 0 3278 522" class="SignInButton">
        <style type="text/css">.fil0:hover {fill: #4B4B4B;} .fnt0 {font-size: 260px;font-family: 'Segoe UI Semibold', 'Segoe UI'; text-decoration: none;}</style>
        <rect class="fil0" x="2" y="2" width="3174" height="517" fill="black" />
        <rect x="150" y="129" width="122" height="122" fill="#F35325" />
        <rect x="284" y="129" width="122" height="122" fill="#81BC06" />
        <rect x="150" y="263" width="122" height="122" fill="#05A6F0" />
        <rect x="284" y="263" width="122" height="122" fill="#FFBA08" />
        <text x="470" y="357" fill="white" class="fnt0">Sign in with Microsoft</text>
        </svg>
    </a>
}
else
{
    <span><br/>Hello @System.Security.Claims.ClaimsPrincipal.Current.FindFirst("name").Value;</span>
    <br /><br />
    @Html.ActionLink("See Your Claims", "Index", "Claims")
    <br /><br />
    @Html.ActionLink("Sign out", "SignOut", "Home")
}
@if (!string.IsNullOrWhiteSpace(Request.QueryString["errormessage"]))
{
    <div style="background-color:red;color:white;font-weight: bold;">Error: @Request.QueryString["errormessage"]</div>
}
</body>
</html>
```
<!--start-collapse-->
### <a name="more-information"></a>Další informace
> Tato stránka přidá tlačítko přihlášení ve formátu SVG s černým pozadí:<br/>![Přihlášení se společností Microsoft](media/active-directory-serversidewebapp-aspnetwebappowin-use/aspnetsigninbuttonsample.png)<br/> Pro tlačítka Další přihlášení, přejděte prosím toohello [tuto stránku](https://docs.microsoft.com/azure/active-directory/develop/active-directory-branding-guidelines "Branding pokyny").
<!--end-collapse-->

## <a name="add-a-controller-toodisplay-users-claims"></a>Přidání deklarace identity uživatele toodisplay řadiče
Tento řadič ukazuje hello používání hello `[Authorize]` atribut tooprotect kontroleru. Tento atribut omezuje přístup toohello řadiče tím, že se pouze ověřené uživatele. Hello kódu níže využívá hello atribut toodisplay uživatele deklarace, které byly získány v rámci hello přihlásit.

1.  Klikněte pravým tlačítkem na hello `Controllers` složky:`Add` > `Controller`
2.  Vyberte `MVC {version} Controller – Empty`.
3.  Klikněte na tlačítko *přidat*
4.  Název`ClaimsController`
5.  Nahraďte kód hello třídy controller kódem hello níže – tento postup přidá hello `[Authorize]` toohello třídy atributů:

```csharp
[Authorize]
public class ClaimsController : Controller
{
    /// <summary>
    /// Add user's claims tooviewbag
    /// </summary>
    /// <returns></returns>
    public ActionResult Index()
    {
        var claimsPrincipalCurrent = System.Security.Claims.ClaimsPrincipal.Current;
        //You get hello user’s first and last name below:
        ViewBag.Name = claimsPrincipalCurrent.FindFirst("name").Value;

        // hello 'preferred_username' claim can be used for showing hello username
        ViewBag.Username = claimsPrincipalCurrent.FindFirst("preferred_username").Value;

        // hello subject claim can be used toouniquely identify hello user across hello web
        ViewBag.Subject = claimsPrincipalCurrent.FindFirst(System.Security.Claims.ClaimTypes.NameIdentifier).Value;

        // TenantId is hello unique Tenant Id - which represents an organization in Azure AD
        ViewBag.TenantId = claimsPrincipalCurrent.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;

        return View();
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a>Další informace
> Kvůli použití hello hello `[Authorize]` atribut, všechny metody tohoto řadiče lze spustit pouze pokud hello uživatel ověřen. Pokud hello uživatel není ověřen a pokusí tooaccess hello řadiče, bude OWIN zahájit výzvu ověřování a vynutit tooauthenticate hello uživatele. Kód Hello výše vypadá na hello deklarací kolekce hello `ClaimsPrincipal.Current` instance pro konkrétního uživatele atributy součástí token uživatele hello. Tyto atributy zahrnují hello uživatele úplný název a uživatelské jméno, jakož i hello globální uživatelský identifikátor subjektu. Obsahuje taky hello *ID klienta*, která reprezentuje ID hello hello uživatele organizaci. 
<!--end-collapse-->

## <a name="create-a-view-toodisplay-hello-users-claims"></a>Vytvoření zobrazení deklarace identity uživatelů toodisplay hello

V sadě Visual Studio vytvořte nové zobrazení deklarací identity toodisplay hello uživatele na webové stránce:

1.  Klikněte pravým tlačítkem na hello `Views\Claims` složky a:`Add View`
2.  Pojmenujte ji `Index`.
3.  Přidejte následující soubor HTML toohello hello:

```html
<html>
<head>
    <meta name="viewport" content="width=device-width" />
    <title>Sign-In with Microsoft Sample</title>
    <link href="@Url.Content("~/Content/bootstrap.min.css")" rel="stylesheet" type="text/css" />
</head>
<body style="padding:50px">
    <h3>Main Claims:</h3>
    <table class="table table-striped table-bordered table-hover">
        <tr><td>Name</td><td>@ViewBag.Name</td></tr>
        <tr><td>Username</td><td>@ViewBag.Username</td></tr>
        <tr><td>Subject</td><td>@ViewBag.Subject</td></tr>
        <tr><td>TenantId</td><td>@ViewBag.TenantId</td></tr>
    </table>
    <br />
    <h3>All Claims:</h3>
    <table class="table table-striped table-bordered table-hover table-condensed">
    @foreach (var claim in System.Security.Claims.ClaimsPrincipal.Current.Claims)
    {
        <tr><td>@claim.Type</td><td>@claim.Value</td></tr>
    }
</table>
    <br />
    <br />
    @Html.ActionLink("Sign out", "SignOut", "Home", null, new { @class = "btn btn-primary" })
</body>
</html>
```
