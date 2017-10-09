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
# <a name="add-sign-in-tooan-net-mvc-web-app"></a><span data-ttu-id="daa2e-103">Přidání přihlášení tooan .NET MVC webové aplikace</span><span class="sxs-lookup"><span data-stu-id="daa2e-103">Add sign-in tooan .NET MVC web app</span></span>
<span data-ttu-id="daa2e-104">S koncovým bodem v2.0 hello můžete rychle přidat ověřování tooyour webové aplikace s podporou pro oba osobní účty Microsoft a pracovní nebo školní účty.</span><span class="sxs-lookup"><span data-stu-id="daa2e-104">With hello v2.0 endpoint, you can quickly add authentication tooyour web apps with support for both personal Microsoft accounts and work or school accounts.</span></span>  <span data-ttu-id="daa2e-105">V aplikacích ASP.NET web můžete to provést pomocí middlewaru OWIN společnosti Microsoft, zahrnutá v rozhraní .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="daa2e-105">In ASP.NET web apps, you can accomplish this using Microsoft's OWIN middleware included in .NET Framework 4.5.</span></span>

> [!NOTE]
> <span data-ttu-id="daa2e-106">Ne všechny scénáře Azure Active Directory a funkce jsou podporovány koncového bodu v2.0 hello.</span><span class="sxs-lookup"><span data-stu-id="daa2e-106">Not all Azure Active Directory scenarios & features are supported by hello v2.0 endpoint.</span></span>  <span data-ttu-id="daa2e-107">toodetermine Pokud byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="daa2e-107">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
>
>

 <span data-ttu-id="daa2e-108">Zde jsme budete vytvářet webové aplikace, která používá OWIN toosign hello uživatele v zobrazení některé informace o uživateli hello a přihlašovací hello uživatele mimo aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="daa2e-108">Here we'll build an web app that uses OWIN toosign hello user in, display some information about hello user, and sign hello user out of hello app.</span></span>

## <a name="download"></a><span data-ttu-id="daa2e-109">Ke stažení</span><span class="sxs-lookup"><span data-stu-id="daa2e-109">Download</span></span>
<span data-ttu-id="daa2e-110">Hello kód v tomto kurzu se udržuje [na Githubu](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet).</span><span class="sxs-lookup"><span data-stu-id="daa2e-110">hello code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet).</span></span>  <span data-ttu-id="daa2e-111">toofollow společně, můžete [stáhnout kostru aplikace hello jako ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) nebo hello kostru klonovat:</span><span class="sxs-lookup"><span data-stu-id="daa2e-111">toofollow along, you can [download hello app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) or clone hello skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

<span data-ttu-id="daa2e-112">aplikace Hello dokončit, je k dispozici na konci hello také tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="daa2e-112">hello completed app is provided at hello end of this tutorial as well.</span></span>

## <a name="register-an-app"></a><span data-ttu-id="daa2e-113">Registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="daa2e-113">Register an app</span></span>
<span data-ttu-id="daa2e-114">Vytvoření nové aplikace v [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo postupujte podle těchto [podrobné kroky](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="daa2e-114">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="daa2e-115">Zkontrolujte, že:</span><span class="sxs-lookup"><span data-stu-id="daa2e-115">Make sure to:</span></span>

* <span data-ttu-id="daa2e-116">Poznamenejte hello **Id aplikace** přiřazen tooyour aplikace, budete ho potřebovat brzy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="daa2e-116">Copy down hello **Application Id** assigned tooyour app, you'll need it soon.</span></span>
* <span data-ttu-id="daa2e-117">Přidat hello **webové** platformu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="daa2e-117">Add hello **Web** platform for your app.</span></span>
* <span data-ttu-id="daa2e-118">Zadejte správný hello **identifikátor URI pro přesměrování**.</span><span class="sxs-lookup"><span data-stu-id="daa2e-118">Enter hello correct **Redirect URI**.</span></span> <span data-ttu-id="daa2e-119">Hello identifikátor uri přesměrování označuje tooAzure AD, kde se mají směrovat ověřování odpovědí – hello výchozí hodnota pro tento kurz je `https://localhost:44326/`.</span><span class="sxs-lookup"><span data-stu-id="daa2e-119">hello redirect uri indicates tooAzure AD where authentication responses should be directed - hello default for this tutorial is `https://localhost:44326/`.</span></span>

## <a name="install--configure-owin-authentication"></a><span data-ttu-id="daa2e-120">Instalace a konfigurace ověřování OWIN.</span><span class="sxs-lookup"><span data-stu-id="daa2e-120">Install & configure OWIN authentication</span></span>
<span data-ttu-id="daa2e-121">Zde nakonfigurujeme hello OWIN middleware toouse hello ověřovacího protokolu OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="daa2e-121">Here, we'll configure hello OWIN middleware toouse hello OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="daa2e-122">OWIN bude použité tooissue požadavků na přihlášení a odhlášení, spravovat hello uživatelské relace a získat informace o uživateli hello, mimo jiné.</span><span class="sxs-lookup"><span data-stu-id="daa2e-122">OWIN will be used tooissue sign-in and sign-out requests, manage hello user's session, and get information about hello user, amongst other things.</span></span>

1. <span data-ttu-id="daa2e-123">toobegin, otevřete hello `web.config` v kořenovém hello hello projektu soubor a zadejte hodnoty konfigurace vaší aplikace v hello `<appSettings>` části.</span><span class="sxs-lookup"><span data-stu-id="daa2e-123">toobegin, open hello `web.config` file in hello root of hello project, and enter your app's configuration values in hello `<appSettings>` section.</span></span>

  * <span data-ttu-id="daa2e-124">Hello `ida:ClientId` je hello **Id aplikace** přiřazené tooyour aplikace v portálu pro registraci hello.</span><span class="sxs-lookup"><span data-stu-id="daa2e-124">hello `ida:ClientId` is hello **Application Id** assigned tooyour app in hello registration portal.</span></span>
  * <span data-ttu-id="daa2e-125">Hello `ida:RedirectUri` je hello **identifikátor Uri pro přesměrování** jste zadali v portálu hello.</span><span class="sxs-lookup"><span data-stu-id="daa2e-125">hello `ida:RedirectUri` is hello **Redirect Uri** you entered in hello portal.</span></span>

2. <span data-ttu-id="daa2e-126">Dál přidejte hello OWIN middleware NuGet balíčky toohello projekt pomocí hello Konzola správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="daa2e-126">Next, add hello OWIN middleware NuGet packages toohello project using hello Package Manager Console.</span></span>

        ```
        PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
        PM> Install-Package Microsoft.Owin.Security.Cookies
        PM> Install-Package Microsoft.Owin.Host.SystemWeb
        ```  

3. <span data-ttu-id="daa2e-127">Přidat projekt aplikace "Třídy pro spuštění OWIN" toohello nazývá `Startup.cs` správné, klikněte na projekt hello--> **přidat** --> **nová položka** --> vyhledejte "OWIN".</span><span class="sxs-lookup"><span data-stu-id="daa2e-127">Add an "OWIN Startup Class" toohello project called `Startup.cs`  Right click on hello project --> **Add** --> **New Item** --> Search for "OWIN".</span></span>  <span data-ttu-id="daa2e-128">Hello OWIN middleware vyvolá hello `Configuration(...)` metoda při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="daa2e-128">hello OWIN middleware will invoke hello `Configuration(...)` method when your app starts.</span></span>
4. <span data-ttu-id="daa2e-129">Změňte deklaraci třídy hello příliš`public partial class Startup` -již implementovali jsme součástí této třídy pro vás v jiném souboru.</span><span class="sxs-lookup"><span data-stu-id="daa2e-129">Change hello class declaration too`public partial class Startup` - we've already implemented part of this class for you in another file.</span></span>  <span data-ttu-id="daa2e-130">V hello `Configuration(...)` metoda, ujistěte se, volání tooConfigureAuth(...) tooset až ověřování pro webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="daa2e-130">In hello `Configuration(...)` method, make a call tooConfigureAuth(...) tooset up authentication for your web app</span></span>  

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

5. <span data-ttu-id="daa2e-131">Soubor otevřete hello `App_Start\Startup.Auth.cs` a implementovat hello `ConfigureAuth(...)` metoda.</span><span class="sxs-lookup"><span data-stu-id="daa2e-131">Open hello file `App_Start\Startup.Auth.cs` and implement hello `ConfigureAuth(...)` method.</span></span>  <span data-ttu-id="daa2e-132">Hello parametry, které zadáte v `OpenIdConnectAuthenticationOptions` bude sloužit jako souřadnice toocommunicate vaší aplikace s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="daa2e-132">hello parameters you provide in `OpenIdConnectAuthenticationOptions` will serve as coordinates for your app toocommunicate with Azure AD.</span></span>  <span data-ttu-id="daa2e-133">Budete také potřebovat tooset až ověřování souborů Cookie – používá soubory cookie pod hello zahrnuje, OpenID Connect middleware hello.</span><span class="sxs-lookup"><span data-stu-id="daa2e-133">You'll also need tooset up Cookie Authentication - hello OpenID Connect middleware uses cookies underneath hello covers.</span></span>

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

## <a name="send-authentication-requests"></a><span data-ttu-id="daa2e-134">Odeslání žádosti o ověření</span><span class="sxs-lookup"><span data-stu-id="daa2e-134">Send authentication requests</span></span>
<span data-ttu-id="daa2e-135">Aplikace je nyní správně nakonfigurované toocommunicate s koncovým bodem v2.0 hello pomocí ověřovacího protokolu OpenID Connect hello.</span><span class="sxs-lookup"><span data-stu-id="daa2e-135">Your app is now properly configured toocommunicate with hello v2.0 endpoint using hello OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="daa2e-136">OWIN má postaráno všechny hello ugly podrobnosti o věnujte zpráv ověřování, ověřování tokenů z Azure AD a údržbě uživatelské relace.</span><span class="sxs-lookup"><span data-stu-id="daa2e-136">OWIN has taken care of all of hello ugly details of crafting authentication messages, validating tokens from Azure AD, and maintaining user session.</span></span>  <span data-ttu-id="daa2e-137">Všechno, co zůstane je toogive vaši uživatelé odhlaste se a způsob toosign v.</span><span class="sxs-lookup"><span data-stu-id="daa2e-137">All that remains is toogive your users a way toosign in and sign out.</span></span>

- <span data-ttu-id="daa2e-138">Můžete použít autorizovat značky v vaše řadiče toorequire tento uživatel přihlásí před přístupem k určité stránky.</span><span class="sxs-lookup"><span data-stu-id="daa2e-138">You can use authorize tags in your controllers toorequire that user signs in before accessing a certain page.</span></span>  <span data-ttu-id="daa2e-139">Otevřete `Controllers\HomeController.cs`a přidejte hello `[Authorize]` značky toohello o řadiče.</span><span class="sxs-lookup"><span data-stu-id="daa2e-139">Open `Controllers\HomeController.cs`, and add hello `[Authorize]` tag toohello About controller.</span></span>
        
        ```C#
        [Authorize]
        public ActionResult About()
        {
          ...
        ```

- <span data-ttu-id="daa2e-140">Můžete taky OWIN toodirectly problém žádosti o ověření z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="daa2e-140">You can also use OWIN toodirectly issue authentication requests from within your code.</span></span>  <span data-ttu-id="daa2e-141">Otevřete `Controllers\AccountController.cs`.</span><span class="sxs-lookup"><span data-stu-id="daa2e-141">Open `Controllers\AccountController.cs`.</span></span>  <span data-ttu-id="daa2e-142">V hello SignIn() a SignOut() akce vydejte OpenID Connect výzvy a odhlášení požadavků, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="daa2e-142">In hello SignIn() and SignOut() actions, issue OpenID Connect challenge and sign-out requests, respectively.</span></span>

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

- <span data-ttu-id="daa2e-143">Nyní otevřete `Views\Shared\_LoginPartial.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="daa2e-143">Now, open `Views\Shared\_LoginPartial.cshtml`.</span></span>  <span data-ttu-id="daa2e-144">Toto je, kde můžete zobrazit uživatele hello odkazy přihlášení a odhlášení vaší aplikace a vytiskněte hello uživatelské jméno v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="daa2e-144">This is where you'll show hello user your app's sign-in and sign-out links, and print out hello user's name in a view.</span></span>

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

## <a name="display-user-information"></a><span data-ttu-id="daa2e-145">Zobrazit informace o uživateli</span><span class="sxs-lookup"><span data-stu-id="daa2e-145">Display user information</span></span>
<span data-ttu-id="daa2e-146">Při ověřování uživatelů s OpenID Connect, vrátí koncového bodu v2.0 hello požadavku id_token toohello aplikaci, která obsahuje deklarace identity nebo tvrzení o hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="daa2e-146">When authenticating users with OpenID Connect, hello v2.0 endpoint returns an id_token toohello app that contains claims, or assertions about hello user.</span></span>  <span data-ttu-id="daa2e-147">Můžete použít tyto deklarace identity toopersonalize aplikace:</span><span class="sxs-lookup"><span data-stu-id="daa2e-147">You can use these claims toopersonalize your app:</span></span>

- <span data-ttu-id="daa2e-148">Otevřete hello `Controllers\HomeController.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="daa2e-148">Open hello `Controllers\HomeController.cs` file.</span></span>  <span data-ttu-id="daa2e-149">Deklarace identity uživatelů hello dostanete v řadičích prostřednictvím hello `ClaimsPrincipal.Current` zaregistrovaný objekt zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="daa2e-149">You can access hello user's claims in your controllers via hello `ClaimsPrincipal.Current` security principal object.</span></span>

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

## <a name="run"></a><span data-ttu-id="daa2e-150">Spusťte</span><span class="sxs-lookup"><span data-stu-id="daa2e-150">Run</span></span>
<span data-ttu-id="daa2e-151">Nakonec sestavte a spusťte aplikaci!</span><span class="sxs-lookup"><span data-stu-id="daa2e-151">Finally, build and run your app!</span></span>   <span data-ttu-id="daa2e-152">Přihlaste se pomocí osobního Account Microsoft nebo pracovní nebo školní účet a Všimněte si, jak identitu uživatele hello se odrazí v horním navigačním panelu hello.</span><span class="sxs-lookup"><span data-stu-id="daa2e-152">Sign in with either a personal Microsoft Account or a work or school account, and notice how hello user's identity is reflected in hello top navigation bar.</span></span>  <span data-ttu-id="daa2e-153">Nyní máte webovou aplikaci zabezpečené pomocí standardních oborových protokolech, které může ověřit uživatele s svoje osobní, tak i pracovní nebo školní účty.</span><span class="sxs-lookup"><span data-stu-id="daa2e-153">You now have a web app secured using industry standard protocols that can authenticate users with both their personal and work/school accounts.</span></span>

<span data-ttu-id="daa2e-154">Pro referenci hello dokončit ukázka (bez vašich hodnot nastavení) [je k dispozici jako ZIP zde](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip), nebo ji můžete klonovat z Githubu:</span><span class="sxs-lookup"><span data-stu-id="daa2e-154">For reference, hello completed sample (without your configuration values) [is provided as a .zip here](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip), or you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

## <a name="next-steps"></a><span data-ttu-id="daa2e-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="daa2e-155">Next steps</span></span>
<span data-ttu-id="daa2e-156">Nyní se můžete přesunout na pokročilejší témata.</span><span class="sxs-lookup"><span data-stu-id="daa2e-156">You can now move onto more advanced topics.</span></span>  <span data-ttu-id="daa2e-157">Může být vhodné tootry:</span><span class="sxs-lookup"><span data-stu-id="daa2e-157">You may want tootry:</span></span>

[<span data-ttu-id="daa2e-158">Zabezpečení webového rozhraní API s koncovým bodem v2.0 hello hello >></span><span class="sxs-lookup"><span data-stu-id="daa2e-158">Secure a Web API with hello hello v2.0 endpoint >></span></span>](active-directory-devquickstarts-webapi-dotnet.md)

<span data-ttu-id="daa2e-159">Další zdroje projděte si:</span><span class="sxs-lookup"><span data-stu-id="daa2e-159">For additional resources, check out:</span></span>

* [<span data-ttu-id="daa2e-160">Příručka vývojáře v2.0 Hello >></span><span class="sxs-lookup"><span data-stu-id="daa2e-160">hello v2.0 developer guide >></span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="daa2e-161">Značka StackOverflow "azure-active-directory" >></span><span class="sxs-lookup"><span data-stu-id="daa2e-161">StackOverflow "azure-active-directory" tag >></span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="daa2e-162">Získejte bezpečnostní aktualizace našich produktů</span><span class="sxs-lookup"><span data-stu-id="daa2e-162">Get security updates for our products</span></span>
<span data-ttu-id="daa2e-163">Doporučujeme vám tooget oznámení o bezpečnostních incidentech navštivte stránky [tuto stránku](https://technet.microsoft.com/security/dd252948) a přihlášení k odběru tooSecurity Advisory Alerts.</span><span class="sxs-lookup"><span data-stu-id="daa2e-163">We encourage you tooget notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing tooSecurity Advisory Alerts.</span></span>
