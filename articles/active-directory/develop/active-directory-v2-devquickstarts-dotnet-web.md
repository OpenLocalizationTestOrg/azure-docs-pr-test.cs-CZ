---
title: "Služba Azure AD v2.0 .NET webové přihlášení aplikace Začínáme | Microsoft Docs"
description: "Postup vytvoření webové aplikace MVC .NET, která podepisuje uživatele přihlašují pomocí Account Microsoft i osobní a pracovní nebo školní účty."
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
ms.openlocfilehash: ba5bdf7daba6086b70aec54ebe25d4445fa708c3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-sign-in-to-an-net-mvc-web-app"></a><span data-ttu-id="48ac8-103">Přidání přihlašování do webové aplikace .NET MVC</span><span class="sxs-lookup"><span data-stu-id="48ac8-103">Add sign-in to an .NET MVC web app</span></span>
<span data-ttu-id="48ac8-104">S koncovým bodem v2.0 můžete rychle přidat ověřování do vaší webové aplikace s podporou pro oba osobní účty Microsoft a pracovní nebo školní účty.</span><span class="sxs-lookup"><span data-stu-id="48ac8-104">With the v2.0 endpoint, you can quickly add authentication to your web apps with support for both personal Microsoft accounts and work or school accounts.</span></span>  <span data-ttu-id="48ac8-105">V aplikacích ASP.NET web můžete to provést pomocí middlewaru OWIN společnosti Microsoft, zahrnutá v rozhraní .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="48ac8-105">In ASP.NET web apps, you can accomplish this using Microsoft's OWIN middleware included in .NET Framework 4.5.</span></span>

> [!NOTE]
> <span data-ttu-id="48ac8-106">Ne všechny scénáře Azure Active Directory a funkce jsou podporovány koncového bodu v2.0.</span><span class="sxs-lookup"><span data-stu-id="48ac8-106">Not all Azure Active Directory scenarios & features are supported by the v2.0 endpoint.</span></span>  <span data-ttu-id="48ac8-107">Pokud chcete zjistit, pokud byste měli používat koncový bod v2.0, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="48ac8-107">To determine if you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
>
>

 <span data-ttu-id="48ac8-108">Zde jsme budete vytvářet webové aplikace, která používá OWIN uživatele přihlásit, zobrazí některé informace o uživateli a přihlášení uživatele mimo aplikaci.</span><span class="sxs-lookup"><span data-stu-id="48ac8-108">Here we'll build an web app that uses OWIN to sign the user in, display some information about the user, and sign the user out of the app.</span></span>

## <a name="download"></a><span data-ttu-id="48ac8-109">Ke stažení</span><span class="sxs-lookup"><span data-stu-id="48ac8-109">Download</span></span>
<span data-ttu-id="48ac8-110">Kód k tomuto kurzu je udržovaný [na GitHubu](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet).</span><span class="sxs-lookup"><span data-stu-id="48ac8-110">The code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet).</span></span>  <span data-ttu-id="48ac8-111">Chcete-li sledovat, můžete [stáhnout kostru aplikace jako ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) nebo tuto kostru klonovat:</span><span class="sxs-lookup"><span data-stu-id="48ac8-111">To follow along, you can [download the app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) or clone the skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

<span data-ttu-id="48ac8-112">Dokončená aplikace je k dispozici na konci tohoto kurzu také.</span><span class="sxs-lookup"><span data-stu-id="48ac8-112">The completed app is provided at the end of this tutorial as well.</span></span>

## <a name="register-an-app"></a><span data-ttu-id="48ac8-113">Registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="48ac8-113">Register an app</span></span>
<span data-ttu-id="48ac8-114">Vytvoření nové aplikace v [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo postupujte podle těchto [podrobné kroky](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="48ac8-114">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="48ac8-115">Zkontrolujte, že:</span><span class="sxs-lookup"><span data-stu-id="48ac8-115">Make sure to:</span></span>

* <span data-ttu-id="48ac8-116">Zkopírování **Id aplikace** přiřazené vaší aplikaci, budete ho potřebovat brzy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="48ac8-116">Copy down the **Application Id** assigned to your app, you'll need it soon.</span></span>
* <span data-ttu-id="48ac8-117">Přidat **webové** platformu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="48ac8-117">Add the **Web** platform for your app.</span></span>
* <span data-ttu-id="48ac8-118">Zadejte správný **identifikátor URI pro přesměrování**.</span><span class="sxs-lookup"><span data-stu-id="48ac8-118">Enter the correct **Redirect URI**.</span></span> <span data-ttu-id="48ac8-119">Určuje identifikátor uri přesměrování do služby Azure AD, kde se mají směrovat ověřování odpovědí – výchozí hodnota pro tento kurz je `https://localhost:44326/`.</span><span class="sxs-lookup"><span data-stu-id="48ac8-119">The redirect uri indicates to Azure AD where authentication responses should be directed - the default for this tutorial is `https://localhost:44326/`.</span></span>

## <a name="install--configure-owin-authentication"></a><span data-ttu-id="48ac8-120">Instalace a konfigurace ověřování OWIN.</span><span class="sxs-lookup"><span data-stu-id="48ac8-120">Install & configure OWIN authentication</span></span>
<span data-ttu-id="48ac8-121">Zde nakonfigurujeme middleware OWIN pro použití ověřovacího protokolu OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="48ac8-121">Here, we'll configure the OWIN middleware to use the OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="48ac8-122">OWIN se použije k vydávání požadavků na přihlášení a odhlášení, spravovat relace uživatele a získat informace o uživateli, mimo jiné.</span><span class="sxs-lookup"><span data-stu-id="48ac8-122">OWIN will be used to issue sign-in and sign-out requests, manage the user's session, and get information about the user, amongst other things.</span></span>

1. <span data-ttu-id="48ac8-123">Chcete-li začít, otevřete `web.config` v kořenovém adresáři projektu soubor a zadejte hodnoty konfigurace vaší aplikace v `<appSettings>` oddílu.</span><span class="sxs-lookup"><span data-stu-id="48ac8-123">To begin, open the `web.config` file in the root of the project, and enter your app's configuration values in the `<appSettings>` section.</span></span>

  * <span data-ttu-id="48ac8-124">`ida:ClientId` Je **Id aplikace** přiřazené vaší aplikaci v portálu pro registraci.</span><span class="sxs-lookup"><span data-stu-id="48ac8-124">The `ida:ClientId` is the **Application Id** assigned to your app in the registration portal.</span></span>
  * <span data-ttu-id="48ac8-125">`ida:RedirectUri` Je **identifikátor Uri pro přesměrování** jste zadali v portálu.</span><span class="sxs-lookup"><span data-stu-id="48ac8-125">The `ida:RedirectUri` is the **Redirect Uri** you entered in the portal.</span></span>

2. <span data-ttu-id="48ac8-126">Balíčky NuGet middleware OWIN v dalším kroku přidejte do projektu pomocí konzoly Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="48ac8-126">Next, add the OWIN middleware NuGet packages to the project using the Package Manager Console.</span></span>

        ```
        PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
        PM> Install-Package Microsoft.Owin.Security.Cookies
        PM> Install-Package Microsoft.Owin.Host.SystemWeb
        ```  

3. <span data-ttu-id="48ac8-127">Přidání "Třídy pro spuštění OWIN" do projektu názvem `Startup.cs` správné, klikněte na projekt--> **přidat** --> **nová položka** --> vyhledejte "OWIN".</span><span class="sxs-lookup"><span data-stu-id="48ac8-127">Add an "OWIN Startup Class" to the project called `Startup.cs`  Right click on the project --> **Add** --> **New Item** --> Search for "OWIN".</span></span>  <span data-ttu-id="48ac8-128">Middleware OWIN při spuštění vaší aplikace vyvolá metodu `Configuration(...)`.</span><span class="sxs-lookup"><span data-stu-id="48ac8-128">The OWIN middleware will invoke the `Configuration(...)` method when your app starts.</span></span>
4. <span data-ttu-id="48ac8-129">Změňte deklaraci třídy k `public partial class Startup` -již implementovali jsme součástí této třídy pro vás v jiném souboru.</span><span class="sxs-lookup"><span data-stu-id="48ac8-129">Change the class declaration to `public partial class Startup` - we've already implemented part of this class for you in another file.</span></span>  <span data-ttu-id="48ac8-130">V `Configuration(...)` metoda, zkontrolujte zavolá ConfigureAuth(...) nastavení ověřování pro webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="48ac8-130">In the `Configuration(...)` method, make a call to ConfigureAuth(...) to set up authentication for your web app</span></span>  

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

5. <span data-ttu-id="48ac8-131">Otevřete soubor `App_Start\Startup.Auth.cs` a implementovat `ConfigureAuth(...)` metoda.</span><span class="sxs-lookup"><span data-stu-id="48ac8-131">Open the file `App_Start\Startup.Auth.cs` and implement the `ConfigureAuth(...)` method.</span></span>  <span data-ttu-id="48ac8-132">Parametry, zadejte v `OpenIdConnectAuthenticationOptions` bude sloužit jako souřadnice pro vaši aplikaci komunikovat s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="48ac8-132">The parameters you provide in `OpenIdConnectAuthenticationOptions` will serve as coordinates for your app to communicate with Azure AD.</span></span>  <span data-ttu-id="48ac8-133">Také budete muset nastavit ověřování souborů Cookie – používá soubory cookie pod pozadí, middleware OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="48ac8-133">You'll also need to set up Cookie Authentication - the OpenID Connect middleware uses cookies underneath the covers.</span></span>

        ```C#
        public void ConfigureAuth(IAppBuilder app)
                     {
                             app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);
        
                             app.UseCookieAuthentication(new CookieAuthenticationOptions());
        
                             app.UseOpenIdConnectAuthentication(
                                     new OpenIdConnectAuthenticationOptions
                                     {
                                             // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0
                                             // The `Scope` describes the permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                                             // In a real application you could use issuer validation for additional checks, like making sure the user's organization has signed up for your app, for instance.
        
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

## <a name="send-authentication-requests"></a><span data-ttu-id="48ac8-134">Odeslání žádosti o ověření</span><span class="sxs-lookup"><span data-stu-id="48ac8-134">Send authentication requests</span></span>
<span data-ttu-id="48ac8-135">Aplikace je nyní správně nakonfigurováno pro komunikaci s koncovým bodem v2.0 pomocí ověřovacího protokolu OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="48ac8-135">Your app is now properly configured to communicate with the v2.0 endpoint using the OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="48ac8-136">OWIN má postaráno všechny ugly podrobnosti věnujte zpráv ověřování, ověřování tokenů z Azure AD a udržování uživatelské relace.</span><span class="sxs-lookup"><span data-stu-id="48ac8-136">OWIN has taken care of all of the ugly details of crafting authentication messages, validating tokens from Azure AD, and maintaining user session.</span></span>  <span data-ttu-id="48ac8-137">Už jen zbývá umožnit uživatelům způsob, jak přihlášení a odhlášení.</span><span class="sxs-lookup"><span data-stu-id="48ac8-137">All that remains is to give your users a way to sign in and sign out.</span></span>

- <span data-ttu-id="48ac8-138">Můžete použít autorizovat značky v řadičích tak, aby vyžadovala přihlášení tohoto uživatele před přístupem k určité stránky.</span><span class="sxs-lookup"><span data-stu-id="48ac8-138">You can use authorize tags in your controllers to require that user signs in before accessing a certain page.</span></span>  <span data-ttu-id="48ac8-139">Otevřete `Controllers\HomeController.cs`a přidejte `[Authorize]` značka, které je o kontroleru.</span><span class="sxs-lookup"><span data-stu-id="48ac8-139">Open `Controllers\HomeController.cs`, and add the `[Authorize]` tag to the About controller.</span></span>
        
        ```C#
        [Authorize]
        public ActionResult About()
        {
          ...
        ```

- <span data-ttu-id="48ac8-140">Můžete taky OWIN přímo vydání žádosti o ověření z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="48ac8-140">You can also use OWIN to directly issue authentication requests from within your code.</span></span>  <span data-ttu-id="48ac8-141">Otevřete `Controllers\AccountController.cs`.</span><span class="sxs-lookup"><span data-stu-id="48ac8-141">Open `Controllers\AccountController.cs`.</span></span>  <span data-ttu-id="48ac8-142">V SignIn() a SignOut() akce vydejte OpenID Connect výzvy a odhlášení požadavků, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="48ac8-142">In the SignIn() and SignOut() actions, issue OpenID Connect challenge and sign-out requests, respectively.</span></span>

        ```C#
        public void SignIn()
        {
            // Send an OpenID Connect sign-in request.
            if (!Request.IsAuthenticated)
            {
                HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
            }
        }
        
        // BUGBUG: Ending a session with the v2.0 endpoint is not yet supported.  Here, we just end the session with the web app.  
        public void SignOut()
        {
            // Send an OpenID Connect sign-out request.
            HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
            Response.Redirect("/");
        }
        ```

- <span data-ttu-id="48ac8-143">Nyní otevřete `Views\Shared\_LoginPartial.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="48ac8-143">Now, open `Views\Shared\_LoginPartial.cshtml`.</span></span>  <span data-ttu-id="48ac8-144">Toto je, kde můžete zobrazit uživatele vaší aplikace přihlášení a odhlášení odkazy a vytiskněte uživatelské jméno v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="48ac8-144">This is where you'll show the user your app's sign-in and sign-out links, and print out the user's name in a view.</span></span>

        ```HTML
        @if (Request.IsAuthenticated)
        {
            <text>
                <ul class="nav navbar-nav navbar-right">
                    <li class="navbar-text">
        
                        @*The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves.*@
        
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

## <a name="display-user-information"></a><span data-ttu-id="48ac8-145">Zobrazit informace o uživateli</span><span class="sxs-lookup"><span data-stu-id="48ac8-145">Display user information</span></span>
<span data-ttu-id="48ac8-146">Při ověřování uživatelů s OpenID Connect, vrátí koncový bod v2.0 požadavku id_token na aplikaci, která obsahuje deklarace identity nebo tvrzení o uživateli.</span><span class="sxs-lookup"><span data-stu-id="48ac8-146">When authenticating users with OpenID Connect, the v2.0 endpoint returns an id_token to the app that contains claims, or assertions about the user.</span></span>  <span data-ttu-id="48ac8-147">Tyto deklarace identity můžete použít k přizpůsobení funkcí aplikace:</span><span class="sxs-lookup"><span data-stu-id="48ac8-147">You can use these claims to personalize your app:</span></span>

- <span data-ttu-id="48ac8-148">Otevřete soubor `Controllers\HomeController.cs`.</span><span class="sxs-lookup"><span data-stu-id="48ac8-148">Open the `Controllers\HomeController.cs` file.</span></span>  <span data-ttu-id="48ac8-149">Dostanete deklaracích identity uživatele v řadičích prostřednictvím `ClaimsPrincipal.Current` zaregistrovaný objekt zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="48ac8-149">You can access the user's claims in your controllers via the `ClaimsPrincipal.Current` security principal object.</span></span>

        ```C#
        [Authorize]
        public ActionResult About()
        {
            ViewBag.Name = ClaimsPrincipal.Current.FindFirst("name").Value;
        
            // The object ID claim will only be emitted for work or school accounts at this time.
            Claim oid = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier");
            ViewBag.ObjectId = oid == null ? string.Empty : oid.Value;
        
            // The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves
            ViewBag.Username = ClaimsPrincipal.Current.FindFirst("preferred_username").Value;
        
            // The subject or nameidentifier claim can be used to uniquely identify the user
            ViewBag.Subject = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        
            return View();
        }
        ```

## <a name="run"></a><span data-ttu-id="48ac8-150">Spuštění</span><span class="sxs-lookup"><span data-stu-id="48ac8-150">Run</span></span>
<span data-ttu-id="48ac8-151">Nakonec sestavte a spusťte aplikaci!</span><span class="sxs-lookup"><span data-stu-id="48ac8-151">Finally, build and run your app!</span></span>   <span data-ttu-id="48ac8-152">Přihlaste se pomocí osobního Account Microsoft nebo pracovní nebo školní účet a Všimněte si, jak se odrazí identitu uživatelů v horním navigačním panelu.</span><span class="sxs-lookup"><span data-stu-id="48ac8-152">Sign in with either a personal Microsoft Account or a work or school account, and notice how the user's identity is reflected in the top navigation bar.</span></span>  <span data-ttu-id="48ac8-153">Nyní máte webovou aplikaci zabezpečené pomocí standardních oborových protokolech, které může ověřit uživatele s svoje osobní, tak i pracovní nebo školní účty.</span><span class="sxs-lookup"><span data-stu-id="48ac8-153">You now have a web app secured using industry standard protocols that can authenticate users with both their personal and work/school accounts.</span></span>

<span data-ttu-id="48ac8-154">Pro srovnání je hotová ukázka (bez vašich hodnot nastavení) [je k dispozici jako ZIP zde](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip), nebo ji můžete klonovat z Githubu:</span><span class="sxs-lookup"><span data-stu-id="48ac8-154">For reference, the completed sample (without your configuration values) [is provided as a .zip here](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip), or you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

## <a name="next-steps"></a><span data-ttu-id="48ac8-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="48ac8-155">Next steps</span></span>
<span data-ttu-id="48ac8-156">Nyní se můžete přesunout na pokročilejší témata.</span><span class="sxs-lookup"><span data-stu-id="48ac8-156">You can now move onto more advanced topics.</span></span>  <span data-ttu-id="48ac8-157">Můžete se pokusit:</span><span class="sxs-lookup"><span data-stu-id="48ac8-157">You may want to try:</span></span>

[<span data-ttu-id="48ac8-158">Zabezpečení webového rozhraní API se koncový bod v2.0 >></span><span class="sxs-lookup"><span data-stu-id="48ac8-158">Secure a Web API with the the v2.0 endpoint >></span></span>](active-directory-devquickstarts-webapi-dotnet.md)

<span data-ttu-id="48ac8-159">Další zdroje projděte si:</span><span class="sxs-lookup"><span data-stu-id="48ac8-159">For additional resources, check out:</span></span>

* [<span data-ttu-id="48ac8-160">Příručka vývojáře v2.0 >></span><span class="sxs-lookup"><span data-stu-id="48ac8-160">The v2.0 developer guide >></span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="48ac8-161">Značka StackOverflow "azure-active-directory" >></span><span class="sxs-lookup"><span data-stu-id="48ac8-161">StackOverflow "azure-active-directory" tag >></span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="48ac8-162">Získejte bezpečnostní aktualizace našich produktů</span><span class="sxs-lookup"><span data-stu-id="48ac8-162">Get security updates for our products</span></span>
<span data-ttu-id="48ac8-163">Doporučujeme vám získávat oznámení o bezpečnostních incidentech tak, že navštívíte [tuto stránku](https://technet.microsoft.com/security/dd252948) a přihlásíte se k odběru služby Security Advisory Alerts.</span><span class="sxs-lookup"><span data-stu-id="48ac8-163">We encourage you to get notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing to Security Advisory Alerts.</span></span>
