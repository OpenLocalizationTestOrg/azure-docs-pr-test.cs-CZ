---
title: "Webové aplikace Azure AD .NET Začínáme | Microsoft Docs"
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
ms.openlocfilehash: 7ac5d3e5cc28ead993e159d003244e6451acb0cc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="aspnet-web-app-sign-in-and-sign-out-with-azure-ad"></a><span data-ttu-id="0a79f-103">Webové aplikace ASP.NET přihlášení a odhlášení s Azure AD</span><span class="sxs-lookup"><span data-stu-id="0a79f-103">ASP.NET web app sign-in and sign-out with Azure AD</span></span>
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="0a79f-104">Tím, že poskytuje jeden přihlášení a odhlášení jenom pár řádků kódu, Azure Active Directory (Azure AD) umožňuje snadno můžete využít Správa identit pro webové aplikace k.</span><span class="sxs-lookup"><span data-stu-id="0a79f-104">By providing a single sign-in and sign-out with only a few lines of code, Azure Active Directory (Azure AD) makes it simple for you to outsource web-app identity management.</span></span> <span data-ttu-id="0a79f-105">Uživatelé a deaktivovat webové aplikace ASP.NET můžete přihlásit pomocí implementace Microsoft Open Web Interface pro .NET (OWIN) middleware.</span><span class="sxs-lookup"><span data-stu-id="0a79f-105">You can sign users in and out of ASP.NET web apps by using the Microsoft implementation of Open Web Interface for .NET (OWIN) middleware.</span></span> <span data-ttu-id="0a79f-106">Rozhraní .NET Framework 4.5 je součástí komunitou vytvářený middleware OWIN.</span><span class="sxs-lookup"><span data-stu-id="0a79f-106">Community-driven OWIN middleware is included in .NET Framework 4.5.</span></span> <span data-ttu-id="0a79f-107">Tento článek ukazuje, jak používat OWIN na:</span><span class="sxs-lookup"><span data-stu-id="0a79f-107">This article shows how to use OWIN to:</span></span>

* <span data-ttu-id="0a79f-108">Přihlášení uživatelů k webové aplikace pomocí Azure AD jako zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="0a79f-108">Sign users in to web apps by using Azure AD as the identity provider.</span></span>
* <span data-ttu-id="0a79f-109">Zobrazí některé informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="0a79f-109">Display some user information.</span></span>
* <span data-ttu-id="0a79f-110">Přihlášení uživatelů z aplikace.</span><span class="sxs-lookup"><span data-stu-id="0a79f-110">Sign users out of the apps.</span></span>

## <a name="before-you-get-started"></a><span data-ttu-id="0a79f-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="0a79f-111">Before you get started</span></span>
* <span data-ttu-id="0a79f-112">Stažení [kostru aplikace](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) nebo stáhnout [hotová ukázka](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="0a79f-112">Download the [app skeleton](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) or download the [completed sample](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).</span></span>
* <span data-ttu-id="0a79f-113">Musíte taky klient služby Azure AD, ve kterém pro registraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="0a79f-113">You also need an Azure AD tenant in which to register the app.</span></span> <span data-ttu-id="0a79f-114">Pokud ještě nemáte klient služby Azure AD [zjistěte, jak získat](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="0a79f-114">If you don't already have an Azure AD tenant, [learn how to get one](active-directory-howto-tenant.md).</span></span>

<span data-ttu-id="0a79f-115">Až budete připravení, postupujte podle pokynů v další čtyři části.</span><span class="sxs-lookup"><span data-stu-id="0a79f-115">When you are ready, follow the procedures in the next four sections.</span></span>

## <a name="step-1-register-the-new-app-with-azure-ad"></a><span data-ttu-id="0a79f-116">Krok 1: Zaregistrujte novou aplikaci s Azure AD</span><span class="sxs-lookup"><span data-stu-id="0a79f-116">Step 1: Register the new app with Azure AD</span></span>
<span data-ttu-id="0a79f-117">Chcete-li nastavit aplikaci pro ověřování uživatelů, nejprve zaregistrovat ve vašem klientovi provedením následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="0a79f-117">To set up the app to authenticate users, first register it in your tenant by doing the following:</span></span>

1. <span data-ttu-id="0a79f-118">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0a79f-118">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="0a79f-119">Na horním panelu klikněte na název účtu.</span><span class="sxs-lookup"><span data-stu-id="0a79f-119">On the top bar, click your account name.</span></span> <span data-ttu-id="0a79f-120">V části **Directory** vyberte klienta služby Active Directory, ve které chcete zaregistrovat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0a79f-120">Under the **Directory** list, select the Active Directory tenant where you want to register the app.</span></span>
3. <span data-ttu-id="0a79f-121">Klikněte na tlačítko **více služeb** v levém podokně a potom vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="0a79f-121">Click **More Services** in the left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="0a79f-122">Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="0a79f-122">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="0a79f-123">Postupujte podle výzev a vytvořte novou **webové aplikace nebo WebAPI**.</span><span class="sxs-lookup"><span data-stu-id="0a79f-123">Follow the prompts to create a new **Web Application and/or WebAPI**.</span></span>
  * <span data-ttu-id="0a79f-124">**Název** popis aplikace pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="0a79f-124">**Name** describes the app to users.</span></span>
  * <span data-ttu-id="0a79f-125">**Adresa URL přihlašování** je základní adresu URL aplikace.</span><span class="sxs-lookup"><span data-stu-id="0a79f-125">**Sign-On URL** is the base URL of the app.</span></span> <span data-ttu-id="0a79f-126">Výchozí adresa URL kostru je https://localhost:44320 /.</span><span class="sxs-lookup"><span data-stu-id="0a79f-126">The skeleton's default URL is https://localhost:44320/.</span></span>
6. <span data-ttu-id="0a79f-127">Po dokončení registrace Azure AD přiřadí aplikace ID jedinečný aplikace.</span><span class="sxs-lookup"><span data-stu-id="0a79f-127">After you've completed the registration, Azure AD assigns the app a unique application ID.</span></span> <span data-ttu-id="0a79f-128">Zkopírujte hodnotu na stránce aplikace používat v dalších částech.</span><span class="sxs-lookup"><span data-stu-id="0a79f-128">Copy the value from the app page to use in the next sections.</span></span>
7. <span data-ttu-id="0a79f-129">Z **nastavení** -> **vlastnosti** stránky pro aplikace, aktualizujte identifikátor ID URI aplikace.</span><span class="sxs-lookup"><span data-stu-id="0a79f-129">From the **Settings** -> **Properties** page for your application, update the App ID URI.</span></span> <span data-ttu-id="0a79f-130">**Identifikátor ID URI aplikace** je jedinečný identifikátor pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0a79f-130">The **App ID URI** is a unique identifier for the app.</span></span> <span data-ttu-id="0a79f-131">Zásady vytváření názvů je `https://<tenant-domain>/<app-name>` (například `https://contoso.onmicrosoft.com/my-first-aad-app`).</span><span class="sxs-lookup"><span data-stu-id="0a79f-131">The naming convention is `https://<tenant-domain>/<app-name>` (for example, `https://contoso.onmicrosoft.com/my-first-aad-app`).</span></span>

## <a name="step-2-set-up-the-app-to-use-the-owin-authentication-pipeline"></a><span data-ttu-id="0a79f-132">Krok 2: Nastavení aplikace pro použití ověřovacího kanálu OWIN</span><span class="sxs-lookup"><span data-stu-id="0a79f-132">Step 2: Set up the app to use the OWIN authentication pipeline</span></span>
<span data-ttu-id="0a79f-133">V tomto kroku nakonfigurujete middleware OWIN pro použití ověřovacího protokolu OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="0a79f-133">In this step, you configure the OWIN middleware to use the OpenID Connect authentication protocol.</span></span> <span data-ttu-id="0a79f-134">OWIN slouží k vydávání požadavků na přihlášení a odhlášení, správě uživatelských relací, získání informací o uživateli a tak dále.</span><span class="sxs-lookup"><span data-stu-id="0a79f-134">You use OWIN to issue sign-in and sign-out requests, manage user sessions, get user information, and so forth.</span></span>

1. <span data-ttu-id="0a79f-135">Pokud chcete začít, přidejte do projektu pomocí konzoly Správce balíčků balíčky NuGet middlewaru OWIN.</span><span class="sxs-lookup"><span data-stu-id="0a79f-135">To begin, add the OWIN middleware NuGet packages to the project by using the Package Manager Console.</span></span>

     ```
     PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
     PM> Install-Package Microsoft.Owin.Security.Cookies
     PM> Install-Package Microsoft.Owin.Host.SystemWeb
     ```

2. <span data-ttu-id="0a79f-136">Přidat třídu OWIN při spuštění projektu názvem `Startup.cs`, klikněte pravým tlačítkem na projekt, vyberte **přidat**, vyberte **nová položka**a poté vyhledejte **OWIN**.</span><span class="sxs-lookup"><span data-stu-id="0a79f-136">To add an OWIN Startup class to the project called `Startup.cs`, right-click the project, select **Add**, select **New Item**, and then search for **OWIN**.</span></span> <span data-ttu-id="0a79f-137">Vyvolá middlewaru OWIN, který **Configuration(...)**  metoda při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="0a79f-137">The OWIN middleware invokes the **Configuration(...)** method when the app starts.</span></span>
3. <span data-ttu-id="0a79f-138">Změňte deklaraci třídy k `public partial class Startup`.</span><span class="sxs-lookup"><span data-stu-id="0a79f-138">Change the class declaration to `public partial class Startup`.</span></span> <span data-ttu-id="0a79f-139">Již implementovali jsme součástí této třídy pro vás v jiném souboru.</span><span class="sxs-lookup"><span data-stu-id="0a79f-139">We've already implemented part of this class for you in another file.</span></span> <span data-ttu-id="0a79f-140">V **Configuration(...)**  metoda, zkontrolujte zavolá **ConfgureAuth(...)**  nastavení ověřování pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0a79f-140">In the **Configuration(...)** method, make a call to **ConfgureAuth(...)** to set up authentication for the app.</span></span>  

     ```C#
     public partial class Startup
     {
         public void Configuration(IAppBuilder app)
         {
             ConfigureAuth(app);
         }
     }
     ```

4. <span data-ttu-id="0a79f-141">Otevřete soubor App_Start\Startup.Auth.cs a potom implementovat **ConfigureAuth(...)**  metoda.</span><span class="sxs-lookup"><span data-stu-id="0a79f-141">Open the App_Start\Startup.Auth.cs file, and then implement the **ConfigureAuth(...)** method.</span></span> <span data-ttu-id="0a79f-142">Parametry, zadejte v *OpenIDConnectAuthenticationOptions* sloužit jako souřadnice aplikace komunikovat s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0a79f-142">The parameters you provide in *OpenIDConnectAuthenticationOptions* serve as coordinates for the app to communicate with Azure AD.</span></span> <span data-ttu-id="0a79f-143">Musíte taky nastavit ověřování souborů cookie, protože middleware OpenID Connect používá soubory cookie na pozadí.</span><span class="sxs-lookup"><span data-stu-id="0a79f-143">You also need to set up cookie authentication, because the OpenID Connect middleware uses cookies in the background.</span></span>

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

5. <span data-ttu-id="0a79f-144">Otevřete soubor web.config v kořenovém adresáři projektu a potom zadejte hodnoty konfigurace v `<appSettings>` oddílu.</span><span class="sxs-lookup"><span data-stu-id="0a79f-144">Open the web.config file in the root of the project, and then enter the configuration values in the `<appSettings>` section.</span></span>
  * <span data-ttu-id="0a79f-145">`ida:ClientId`: Identifikátor GUID, které jste zkopírovali z portálu Azure v "krok 1: Zaregistrujte novou aplikaci s Azure AD."</span><span class="sxs-lookup"><span data-stu-id="0a79f-145">`ida:ClientId`: The GUID you copied from the Azure portal in "Step 1: Register the new app with Azure AD."</span></span>
  * <span data-ttu-id="0a79f-146">`ida:Tenant`: Název klienta služby Azure AD (například contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="0a79f-146">`ida:Tenant`: The name of your Azure AD tenant (for example, contoso.onmicrosoft.com).</span></span>
  * <span data-ttu-id="0a79f-147">`ida:PostLogoutRedirectUri`: Indikátoru, která říká službě Azure AD, kde by měl být uživatel přesměrován po úspěšném dokončení žádost o odhlášení.</span><span class="sxs-lookup"><span data-stu-id="0a79f-147">`ida:PostLogoutRedirectUri`: The indicator that tells Azure AD where a user should be redirected after a sign-out request is successfully completed.</span></span>

## <a name="step-3-use-owin-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a><span data-ttu-id="0a79f-148">Krok 3: Použití OWIN pro zasílání požadavků na přihlášení a odhlášení do Azure AD</span><span class="sxs-lookup"><span data-stu-id="0a79f-148">Step 3: Use OWIN to issue sign-in and sign-out requests to Azure AD</span></span>
<span data-ttu-id="0a79f-149">Aplikace je nyní správně nakonfigurován pro komunikaci se službou Azure AD pomocí ověřovacího protokolu OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="0a79f-149">The app is now properly configured to communicate with Azure AD by using the OpenID Connect authentication protocol.</span></span> <span data-ttu-id="0a79f-150">OWIN má zpracovává všechny podrobnosti o věnujte zpráv ověřování, ověřování tokenů z Azure AD a údržbě uživatelských relací.</span><span class="sxs-lookup"><span data-stu-id="0a79f-150">OWIN has handled all of the details of crafting authentication messages, validating tokens from Azure AD, and maintaining user sessions.</span></span> <span data-ttu-id="0a79f-151">Už jen zbývá umožnit uživatelům způsob, jak přihlášení a odhlášení.</span><span class="sxs-lookup"><span data-stu-id="0a79f-151">All that remains is to give your users a way to sign in and sign out.</span></span>

1. <span data-ttu-id="0a79f-152">Můžete použít autorizovat značky v řadičích budou muset uživatelé přihlásit před přístupem k určité stránky.</span><span class="sxs-lookup"><span data-stu-id="0a79f-152">You can use authorize tags in your controllers to require users to sign in before they access certain pages.</span></span> <span data-ttu-id="0a79f-153">Uděláte to tak, otevřete Controllers\HomeController.cs a pak přidejte `[Authorize]` značky o řadiče.</span><span class="sxs-lookup"><span data-stu-id="0a79f-153">To do so, open Controllers\HomeController.cs, and then add the `[Authorize]` tag to the About controller.</span></span>

     ```C#
     [Authorize]
     public ActionResult About()
     {
       ...
     ```

2. <span data-ttu-id="0a79f-154">Můžete taky OWIN přímo vydání žádosti o ověření z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="0a79f-154">You can also use OWIN to directly issue authentication requests from within your code.</span></span> <span data-ttu-id="0a79f-155">Chcete-li to provést, otevřete Controllers\AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="0a79f-155">To do so, open Controllers\AccountController.cs.</span></span> <span data-ttu-id="0a79f-156">Potom v SignIn() a SignOut() akce, vydávat OpenID Connect výzvy a odhlášení požadavky.</span><span class="sxs-lookup"><span data-stu-id="0a79f-156">Then, in the SignIn() and SignOut() actions, issue OpenID Connect challenge and sign-out requests.</span></span>

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

3. <span data-ttu-id="0a79f-157">Otevřete Views\Shared\_LoginPartial.cshtml se uživateli zobrazí aplikace přihlášení a odhlášení odkazy a vytiskněte uživatelské jméno v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="0a79f-157">Open Views\Shared\_LoginPartial.cshtml to show the user the app sign-in and sign-out links, and to print out the user's name in a view.</span></span>

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

## <a name="step-4-display-user-information"></a><span data-ttu-id="0a79f-158">Krok 4: Zobrazit informace o uživateli</span><span class="sxs-lookup"><span data-stu-id="0a79f-158">Step 4: Display user information</span></span>
<span data-ttu-id="0a79f-159">Během ověřování uživatelů s OpenID Connect, Azure AD vrátí požadavku id_token na aplikaci, která obsahuje "deklarace identity" a tvrzením o uživateli.</span><span class="sxs-lookup"><span data-stu-id="0a79f-159">When it authenticates users with OpenID Connect, Azure AD returns an id_token to the app that contains "claims," or assertions about the user.</span></span> <span data-ttu-id="0a79f-160">Tyto deklarace můžete použít k přizpůsobení funkcí aplikace následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="0a79f-160">You can use these claims to personalize the app by doing the following:</span></span>

1. <span data-ttu-id="0a79f-161">Otevřete soubor Controllers\HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="0a79f-161">Open the Controllers\HomeController.cs file.</span></span> <span data-ttu-id="0a79f-162">Dostanete deklaracích identity uživatele v řadičích prostřednictvím `ClaimsPrincipal.Current` zaregistrovaný objekt zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="0a79f-162">You can access the user's claims in your controllers via the `ClaimsPrincipal.Current` security principal object.</span></span>

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

2. <span data-ttu-id="0a79f-163">Sestavte a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0a79f-163">Build and run the app.</span></span> <span data-ttu-id="0a79f-164">Pokud již jste dosud nevytvořili nového uživatele ve vašem klientovi s doméně onmicrosoft.com, nyní je čas Uděláte to tak.</span><span class="sxs-lookup"><span data-stu-id="0a79f-164">If you haven't already created a new user in your tenant with an onmicrosoft.com domain, now is the time to do so.</span></span> <span data-ttu-id="0a79f-165">Zde je uveden postup:</span><span class="sxs-lookup"><span data-stu-id="0a79f-165">Here's how:</span></span>

  <span data-ttu-id="0a79f-166">a.</span><span class="sxs-lookup"><span data-stu-id="0a79f-166">a.</span></span> <span data-ttu-id="0a79f-167">Přihlaste se pomocí tohoto uživatele a Všimněte si, jak identitu uživatele se projeví na horním panelu.</span><span class="sxs-lookup"><span data-stu-id="0a79f-167">Sign in with that user, and note how the user's identity is reflected on the top bar.</span></span>

  <span data-ttu-id="0a79f-168">b.</span><span class="sxs-lookup"><span data-stu-id="0a79f-168">b.</span></span> <span data-ttu-id="0a79f-169">Odhlásit se a přihlaste se zpět jako jiný uživatel ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="0a79f-169">Sign out, and then sign back in as another user in your tenant.</span></span>

  <span data-ttu-id="0a79f-170">c.</span><span class="sxs-lookup"><span data-stu-id="0a79f-170">c.</span></span> <span data-ttu-id="0a79f-171">Pokud se cítíte zvlášť ambiciózní, zaregistrujte a spustit jiná instance této aplikace (pomocí vlastní clientId) a podívejte se na jednom přihlášení v akci.</span><span class="sxs-lookup"><span data-stu-id="0a79f-171">If you're feeling particularly ambitious, register and run another instance of this app (with its own clientId), and watch single sign-in in action.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0a79f-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0a79f-172">Next steps</span></span>
<span data-ttu-id="0a79f-173">Odkaz, najdete v části [je hotová ukázka](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip) (bez vašich hodnot nastavení).</span><span class="sxs-lookup"><span data-stu-id="0a79f-173">For reference, see [the completed sample](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip) (without your configuration values).</span></span>

<span data-ttu-id="0a79f-174">Nyní se můžete přesunout k pokročilejším tématům.</span><span class="sxs-lookup"><span data-stu-id="0a79f-174">You can now move on to more advanced topics.</span></span> <span data-ttu-id="0a79f-175">Zkuste například [zabezpečení webového rozhraní API s Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="0a79f-175">For example, try [Secure a Web API with Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
