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
# <a name="aspnet-web-app-sign-in-and-sign-out-with-azure-ad"></a><span data-ttu-id="4d00e-103">Webové aplikace ASP.NET přihlášení a odhlášení s Azure AD</span><span class="sxs-lookup"><span data-stu-id="4d00e-103">ASP.NET web app sign-in and sign-out with Azure AD</span></span>
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="4d00e-104">Tím, že poskytuje jeden přihlášení a odhlášení jenom pár řádků kódu, Azure Active Directory (Azure AD) umožňuje snadno můžete toooutsource webové aplikace správy identit.</span><span class="sxs-lookup"><span data-stu-id="4d00e-104">By providing a single sign-in and sign-out with only a few lines of code, Azure Active Directory (Azure AD) makes it simple for you toooutsource web-app identity management.</span></span> <span data-ttu-id="4d00e-105">Uživatelé a deaktivovat webové aplikace ASP.NET můžete přihlásit pomocí implementace Microsoft hello Open Web Interface pro .NET (OWIN) middleware.</span><span class="sxs-lookup"><span data-stu-id="4d00e-105">You can sign users in and out of ASP.NET web apps by using hello Microsoft implementation of Open Web Interface for .NET (OWIN) middleware.</span></span> <span data-ttu-id="4d00e-106">Rozhraní .NET Framework 4.5 je součástí komunitou vytvářený middleware OWIN.</span><span class="sxs-lookup"><span data-stu-id="4d00e-106">Community-driven OWIN middleware is included in .NET Framework 4.5.</span></span> <span data-ttu-id="4d00e-107">Tento článek ukazuje, jak toouse OWIN na:</span><span class="sxs-lookup"><span data-stu-id="4d00e-107">This article shows how toouse OWIN to:</span></span>

* <span data-ttu-id="4d00e-108">Přihlášení uživatelů tooweb aplikace pomocí Azure AD jako zprostředkovatele identity hello.</span><span class="sxs-lookup"><span data-stu-id="4d00e-108">Sign users in tooweb apps by using Azure AD as hello identity provider.</span></span>
* <span data-ttu-id="4d00e-109">Zobrazí některé informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="4d00e-109">Display some user information.</span></span>
* <span data-ttu-id="4d00e-110">Přihlášení uživatelů ze hello aplikací.</span><span class="sxs-lookup"><span data-stu-id="4d00e-110">Sign users out of hello apps.</span></span>

## <a name="before-you-get-started"></a><span data-ttu-id="4d00e-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="4d00e-111">Before you get started</span></span>
* <span data-ttu-id="4d00e-112">Stáhnout hello [kostru aplikace](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) nebo stáhnout hello [hotová ukázka](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="4d00e-112">Download hello [app skeleton](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) or download hello [completed sample](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).</span></span>
* <span data-ttu-id="4d00e-113">Musíte taky klient služby Azure AD, ve které tooregister hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="4d00e-113">You also need an Azure AD tenant in which tooregister hello app.</span></span> <span data-ttu-id="4d00e-114">Pokud ještě nemáte klient služby Azure AD [zjistěte, jak tooget jeden](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="4d00e-114">If you don't already have an Azure AD tenant, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

<span data-ttu-id="4d00e-115">Až budete připravení, postupujte podle pokynů hello v hello další čtyři části.</span><span class="sxs-lookup"><span data-stu-id="4d00e-115">When you are ready, follow hello procedures in hello next four sections.</span></span>

## <a name="step-1-register-hello-new-app-with-azure-ad"></a><span data-ttu-id="4d00e-116">Krok 1: Zaregistrujte hello novou aplikaci s Azure AD</span><span class="sxs-lookup"><span data-stu-id="4d00e-116">Step 1: Register hello new app with Azure AD</span></span>
<span data-ttu-id="4d00e-117">tooset si uživatelé tooauthenticate aplikace hello, nejprve zaregistrovat ve vašem klientovi pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="4d00e-117">tooset up hello app tooauthenticate users, first register it in your tenant by doing hello following:</span></span>

1. <span data-ttu-id="4d00e-118">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4d00e-118">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="4d00e-119">Na horním panelu hello klikněte na název účtu.</span><span class="sxs-lookup"><span data-stu-id="4d00e-119">On hello top bar, click your account name.</span></span> <span data-ttu-id="4d00e-120">V části hello **Directory** seznamu, vyberte hello klienta služby Active Directory místo tooregister hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="4d00e-120">Under hello **Directory** list, select hello Active Directory tenant where you want tooregister hello app.</span></span>
3. <span data-ttu-id="4d00e-121">Klikněte na tlačítko **více služeb** v levém podokně text hello a potom vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="4d00e-121">Click **More Services** in hello left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="4d00e-122">Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="4d00e-122">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="4d00e-123">Postupujte podle hello vyzve toocreate nový **webové aplikace nebo WebAPI**.</span><span class="sxs-lookup"><span data-stu-id="4d00e-123">Follow hello prompts toocreate a new **Web Application and/or WebAPI**.</span></span>
  * <span data-ttu-id="4d00e-124">**Název** popisuje toousers aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="4d00e-124">**Name** describes hello app toousers.</span></span>
  * <span data-ttu-id="4d00e-125">**Adresa URL přihlašování** je hello základní adresu URL aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="4d00e-125">**Sign-On URL** is hello base URL of hello app.</span></span> <span data-ttu-id="4d00e-126">kostru Hello výchozí adresa URL je https://localhost:44320 /.</span><span class="sxs-lookup"><span data-stu-id="4d00e-126">hello skeleton's default URL is https://localhost:44320/.</span></span>
6. <span data-ttu-id="4d00e-127">Po dokončení registrace hello, Azure AD přiřadí aplikace hello ID jedinečný aplikace.</span><span class="sxs-lookup"><span data-stu-id="4d00e-127">After you've completed hello registration, Azure AD assigns hello app a unique application ID.</span></span> <span data-ttu-id="4d00e-128">Zkopírujte hodnotu hello z toouse stránku hello aplikace v dalších částech hello.</span><span class="sxs-lookup"><span data-stu-id="4d00e-128">Copy hello value from hello app page toouse in hello next sections.</span></span>
7. <span data-ttu-id="4d00e-129">Z hello **nastavení** -> **vlastnosti** stránky pro aplikace, aktualizujte hello identifikátor ID URI aplikace.</span><span class="sxs-lookup"><span data-stu-id="4d00e-129">From hello **Settings** -> **Properties** page for your application, update hello App ID URI.</span></span> <span data-ttu-id="4d00e-130">Hello **identifikátor ID URI aplikace** je jedinečný identifikátor pro aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="4d00e-130">hello **App ID URI** is a unique identifier for hello app.</span></span> <span data-ttu-id="4d00e-131">Hello nazevspolecnosti.technologickaoblast.nazevproduktu.funkcnioblast.nazev `https://<tenant-domain>/<app-name>` (například `https://contoso.onmicrosoft.com/my-first-aad-app`).</span><span class="sxs-lookup"><span data-stu-id="4d00e-131">hello naming convention is `https://<tenant-domain>/<app-name>` (for example, `https://contoso.onmicrosoft.com/my-first-aad-app`).</span></span>

## <a name="step-2-set-up-hello-app-toouse-hello-owin-authentication-pipeline"></a><span data-ttu-id="4d00e-132">Krok 2: Nastavení hello aplikace toouse hello OWIN ověřovacího kanálu</span><span class="sxs-lookup"><span data-stu-id="4d00e-132">Step 2: Set up hello app toouse hello OWIN authentication pipeline</span></span>
<span data-ttu-id="4d00e-133">V tomto kroku nakonfigurujete hello OWIN middleware toouse hello ověřovacího protokolu OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="4d00e-133">In this step, you configure hello OWIN middleware toouse hello OpenID Connect authentication protocol.</span></span> <span data-ttu-id="4d00e-134">Použití požadavků na přihlášení a odhlášení tooissue OWIN, správě uživatelských relací, získání informací o uživateli a tak dále.</span><span class="sxs-lookup"><span data-stu-id="4d00e-134">You use OWIN tooissue sign-in and sign-out requests, manage user sessions, get user information, and so forth.</span></span>

1. <span data-ttu-id="4d00e-135">toobegin, přidejte hello OWIN middleware NuGet balíčky toohello projekt pomocí hello Konzola správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="4d00e-135">toobegin, add hello OWIN middleware NuGet packages toohello project by using hello Package Manager Console.</span></span>

     ```
     PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
     PM> Install-Package Microsoft.Owin.Security.Cookies
     PM> Install-Package Microsoft.Owin.Host.SystemWeb
     ```

2. <span data-ttu-id="4d00e-136">tooadd projekt toohello – třída aplikace OWIN při spuštění nazývá `Startup.cs`, klikněte pravým tlačítkem na projekt hello, vyberte **přidat**, vyberte **nová položka**a poté vyhledejte **OWIN**.</span><span class="sxs-lookup"><span data-stu-id="4d00e-136">tooadd an OWIN Startup class toohello project called `Startup.cs`, right-click hello project, select **Add**, select **New Item**, and then search for **OWIN**.</span></span> <span data-ttu-id="4d00e-137">Hello OWIN middleware vyvolá hello **Configuration(...)**  metoda při spuštění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="4d00e-137">hello OWIN middleware invokes hello **Configuration(...)** method when hello app starts.</span></span>
3. <span data-ttu-id="4d00e-138">Změňte deklaraci třídy hello příliš`public partial class Startup`.</span><span class="sxs-lookup"><span data-stu-id="4d00e-138">Change hello class declaration too`public partial class Startup`.</span></span> <span data-ttu-id="4d00e-139">Již implementovali jsme součástí této třídy pro vás v jiném souboru.</span><span class="sxs-lookup"><span data-stu-id="4d00e-139">We've already implemented part of this class for you in another file.</span></span> <span data-ttu-id="4d00e-140">V hello **Configuration(...)**  metody se volat příliš**ConfgureAuth(...)**  tooset až ověřování pro aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="4d00e-140">In hello **Configuration(...)** method, make a call too**ConfgureAuth(...)** tooset up authentication for hello app.</span></span>  

     ```C#
     public partial class Startup
     {
         public void Configuration(IAppBuilder app)
         {
             ConfigureAuth(app);
         }
     }
     ```

4. <span data-ttu-id="4d00e-141">Otevřete soubor App_Start\Startup.Auth.cs hello a potom implementovat hello **ConfigureAuth(...)**  metoda.</span><span class="sxs-lookup"><span data-stu-id="4d00e-141">Open hello App_Start\Startup.Auth.cs file, and then implement hello **ConfigureAuth(...)** method.</span></span> <span data-ttu-id="4d00e-142">Hello parametry, které zadáte v *OpenIDConnectAuthenticationOptions* sloužit jako souřadnice toocommunicate aplikace hello s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4d00e-142">hello parameters you provide in *OpenIDConnectAuthenticationOptions* serve as coordinates for hello app toocommunicate with Azure AD.</span></span> <span data-ttu-id="4d00e-143">Musíte taky tooset až ověřování souborů cookie, protože middleware OpenID Connect hello používá soubory cookie v pozadí hello.</span><span class="sxs-lookup"><span data-stu-id="4d00e-143">You also need tooset up cookie authentication, because hello OpenID Connect middleware uses cookies in hello background.</span></span>

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

5. <span data-ttu-id="4d00e-144">Otevřete soubor web.config hello v kořenovém hello hello projektu a potom zadejte hodnoty konfigurace hello v hello `<appSettings>` části.</span><span class="sxs-lookup"><span data-stu-id="4d00e-144">Open hello web.config file in hello root of hello project, and then enter hello configuration values in hello `<appSettings>` section.</span></span>
  * <span data-ttu-id="4d00e-145">`ida:ClientId`: hello GUID, které jste zkopírovali ze hello portál Azure "krok 1: registrace hello novou aplikaci s Azure AD."</span><span class="sxs-lookup"><span data-stu-id="4d00e-145">`ida:ClientId`: hello GUID you copied from hello Azure portal in "Step 1: Register hello new app with Azure AD."</span></span>
  * <span data-ttu-id="4d00e-146">`ida:Tenant`: hello název klienta služby Azure AD (například contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="4d00e-146">`ida:Tenant`: hello name of your Azure AD tenant (for example, contoso.onmicrosoft.com).</span></span>
  * <span data-ttu-id="4d00e-147">`ida:PostLogoutRedirectUri`: hello indikátoru, která říká službě Azure AD, kde by měl být uživatel přesměrován po úspěšném dokončení žádost o odhlášení.</span><span class="sxs-lookup"><span data-stu-id="4d00e-147">`ida:PostLogoutRedirectUri`: hello indicator that tells Azure AD where a user should be redirected after a sign-out request is successfully completed.</span></span>

## <a name="step-3-use-owin-tooissue-sign-in-and-sign-out-requests-tooazure-ad"></a><span data-ttu-id="4d00e-148">Krok 3: Použití tooAzure AD požadavky OWIN tooissue přihlášení a odhlášení</span><span class="sxs-lookup"><span data-stu-id="4d00e-148">Step 3: Use OWIN tooissue sign-in and sign-out requests tooAzure AD</span></span>
<span data-ttu-id="4d00e-149">Hello aplikace je nyní správně nakonfigurované toocommunicate s Azure AD pomocí ověřovacího protokolu OpenID Connect hello.</span><span class="sxs-lookup"><span data-stu-id="4d00e-149">hello app is now properly configured toocommunicate with Azure AD by using hello OpenID Connect authentication protocol.</span></span> <span data-ttu-id="4d00e-150">OWIN má zpracovává všechny podrobnosti hello věnujte zpráv ověřování, ověřování tokenů z Azure AD a údržbě uživatelských relací.</span><span class="sxs-lookup"><span data-stu-id="4d00e-150">OWIN has handled all of hello details of crafting authentication messages, validating tokens from Azure AD, and maintaining user sessions.</span></span> <span data-ttu-id="4d00e-151">Všechno, co zůstane je toogive vaši uživatelé odhlaste se a způsob toosign v.</span><span class="sxs-lookup"><span data-stu-id="4d00e-151">All that remains is toogive your users a way toosign in and sign out.</span></span>

1. <span data-ttu-id="4d00e-152">Můžete použít autorizovat značky v vaše řadiče toorequire uživatelé toosign v před přístupem k určité stránky.</span><span class="sxs-lookup"><span data-stu-id="4d00e-152">You can use authorize tags in your controllers toorequire users toosign in before they access certain pages.</span></span> <span data-ttu-id="4d00e-153">toodo Ano, otevřete Controllers\HomeController.cs a pak přidejte hello `[Authorize]` značky toohello o řadiče.</span><span class="sxs-lookup"><span data-stu-id="4d00e-153">toodo so, open Controllers\HomeController.cs, and then add hello `[Authorize]` tag toohello About controller.</span></span>

     ```C#
     [Authorize]
     public ActionResult About()
     {
       ...
     ```

2. <span data-ttu-id="4d00e-154">Můžete taky OWIN toodirectly problém žádosti o ověření z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="4d00e-154">You can also use OWIN toodirectly issue authentication requests from within your code.</span></span> <span data-ttu-id="4d00e-155">toodo tak, že otevřete Controllers\AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="4d00e-155">toodo so, open Controllers\AccountController.cs.</span></span> <span data-ttu-id="4d00e-156">Potom v hello SignIn() a SignOut() akce vydávat OpenID Connect výzvy a odhlášení požadavky.</span><span class="sxs-lookup"><span data-stu-id="4d00e-156">Then, in hello SignIn() and SignOut() actions, issue OpenID Connect challenge and sign-out requests.</span></span>

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

3. <span data-ttu-id="4d00e-157">Otevřete Views\Shared\_LoginPartial.cshtml tooshow hello uživatele hello aplikace přihlášení a odhlášení odkazy a tooprint out hello uživatelské jméno v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="4d00e-157">Open Views\Shared\_LoginPartial.cshtml tooshow hello user hello app sign-in and sign-out links, and tooprint out hello user's name in a view.</span></span>

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

## <a name="step-4-display-user-information"></a><span data-ttu-id="4d00e-158">Krok 4: Zobrazit informace o uživateli</span><span class="sxs-lookup"><span data-stu-id="4d00e-158">Step 4: Display user information</span></span>
<span data-ttu-id="4d00e-159">Během ověřování uživatelů s OpenID Connect, Azure AD vrátí požadavku id_token toohello aplikaci, která obsahuje "deklarace identity" nebo tvrzení o hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="4d00e-159">When it authenticates users with OpenID Connect, Azure AD returns an id_token toohello app that contains "claims," or assertions about hello user.</span></span> <span data-ttu-id="4d00e-160">Můžete tyto deklarace identity toopersonalize hello aplikaci pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="4d00e-160">You can use these claims toopersonalize hello app by doing hello following:</span></span>

1. <span data-ttu-id="4d00e-161">Otevřete soubor Controllers\HomeController.cs hello.</span><span class="sxs-lookup"><span data-stu-id="4d00e-161">Open hello Controllers\HomeController.cs file.</span></span> <span data-ttu-id="4d00e-162">Deklarace identity uživatelů hello dostanete v řadičích prostřednictvím hello `ClaimsPrincipal.Current` zaregistrovaný objekt zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="4d00e-162">You can access hello user's claims in your controllers via hello `ClaimsPrincipal.Current` security principal object.</span></span>

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

2. <span data-ttu-id="4d00e-163">Sestavení a spuštění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="4d00e-163">Build and run hello app.</span></span> <span data-ttu-id="4d00e-164">Pokud již jste dosud nevytvořili nového uživatele ve vašem klientovi s doméně onmicrosoft.com, teď proto je toodo čas hello.</span><span class="sxs-lookup"><span data-stu-id="4d00e-164">If you haven't already created a new user in your tenant with an onmicrosoft.com domain, now is hello time toodo so.</span></span> <span data-ttu-id="4d00e-165">Zde je uveden postup:</span><span class="sxs-lookup"><span data-stu-id="4d00e-165">Here's how:</span></span>

  <span data-ttu-id="4d00e-166">a.</span><span class="sxs-lookup"><span data-stu-id="4d00e-166">a.</span></span> <span data-ttu-id="4d00e-167">Přihlaste se pomocí tohoto uživatele a Všimněte si, jak se identita uživatele hello projeví na horním panelu hello.</span><span class="sxs-lookup"><span data-stu-id="4d00e-167">Sign in with that user, and note how hello user's identity is reflected on hello top bar.</span></span>

  <span data-ttu-id="4d00e-168">b.</span><span class="sxs-lookup"><span data-stu-id="4d00e-168">b.</span></span> <span data-ttu-id="4d00e-169">Odhlásit se a přihlaste se zpět jako jiný uživatel ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="4d00e-169">Sign out, and then sign back in as another user in your tenant.</span></span>

  <span data-ttu-id="4d00e-170">c.</span><span class="sxs-lookup"><span data-stu-id="4d00e-170">c.</span></span> <span data-ttu-id="4d00e-171">Pokud se cítíte zvlášť ambiciózní, zaregistrujte a spustit jiná instance této aplikace (pomocí vlastní clientId) a podívejte se na jednom přihlášení v akci.</span><span class="sxs-lookup"><span data-stu-id="4d00e-171">If you're feeling particularly ambitious, register and run another instance of this app (with its own clientId), and watch single sign-in in action.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4d00e-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4d00e-172">Next steps</span></span>
<span data-ttu-id="4d00e-173">Odkaz, najdete v části [hello dokončit ukázka](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip) (bez vašich hodnot nastavení).</span><span class="sxs-lookup"><span data-stu-id="4d00e-173">For reference, see [hello completed sample](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip) (without your configuration values).</span></span>

<span data-ttu-id="4d00e-174">Nyní se můžete přesunout na toomore advanced témata.</span><span class="sxs-lookup"><span data-stu-id="4d00e-174">You can now move on toomore advanced topics.</span></span> <span data-ttu-id="4d00e-175">Zkuste například [zabezpečení webového rozhraní API s Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="4d00e-175">For example, try [Secure a Web API with Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
