---
title: "Služba Azure AD v2.0 .NET webové aplikace volání rozhraní API, Začínáme | Microsoft Docs"
description: "Postup vytvoření webové aplikace .NET MVC tohoto volání webové služby, používání osobních účtů Microsoft a pracovní nebo školní účty pro přihlášení."
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
ms.openlocfilehash: dc3162ae8e6ce622139125c2e78fa45d2e90d534
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="calling-a-web-api-from-a-net-web-app"></a><span data-ttu-id="20c49-103">Volání webového rozhraní API z webové aplikace .NET</span><span class="sxs-lookup"><span data-stu-id="20c49-103">Calling a web API from a .NET web app</span></span>
<span data-ttu-id="20c49-104">S koncovým bodem v2.0 můžete rychle přidání ověřování do vaší webové aplikace a webové rozhraní API s podporou pro oba osobní účty Microsoft a pracovní nebo školní účty.</span><span class="sxs-lookup"><span data-stu-id="20c49-104">With the v2.0 endpoint, you can quickly add authentication to your web apps and web APIs with support for both personal Microsoft accounts and work or school accounts.</span></span>  <span data-ttu-id="20c49-105">Zde jsme budete vytvářet webové aplikace MVC, která podepisuje uživatele pomocí OpenID Connect, s pomoc od společnosti Microsoft OWIN middleware.</span><span class="sxs-lookup"><span data-stu-id="20c49-105">Here, we'll build an MVC web app that signs users in using OpenID Connect, with some help from Microsoft's OWIN middleware.</span></span>  <span data-ttu-id="20c49-106">Webové aplikace se získání přístupových tokenů OAuth 2.0 pro webové rozhraní api, které jsou zabezpečeny OAuth 2.0, který umožňuje vytvořit, číst a odstranit na daného uživatele "seznam úkolů".</span><span class="sxs-lookup"><span data-stu-id="20c49-106">The web app will get OAuth 2.0 access tokens for a web api secured by OAuth 2.0 that allows create, read, and delete on a given user's "To-Do List".</span></span>

<span data-ttu-id="20c49-107">V tomto kurzu se soustředí hlavně na pomocí MSAL na získání a používání ve webové aplikaci, popsané v úplné přístupové tokeny [zde](active-directory-v2-flows.md#web-apps).</span><span class="sxs-lookup"><span data-stu-id="20c49-107">This tutorial will focus primarily on using MSAL to acquire and use access tokens in a web app, described in full [here](active-directory-v2-flows.md#web-apps).</span></span>  <span data-ttu-id="20c49-108">Jako požadavky, můžete chtít nejdřív zjistěte, jak [přidat základní přihlášení do webové aplikace](active-directory-v2-devquickstarts-dotnet-web.md) nebo jak [správně zabezpečení webového rozhraní API](active-directory-v2-devquickstarts-dotnet-api.md).</span><span class="sxs-lookup"><span data-stu-id="20c49-108">As prerequisites, you may want to first learn how to [add basic sign-in to a web app](active-directory-v2-devquickstarts-dotnet-web.md) or how to [properly secure a web API](active-directory-v2-devquickstarts-dotnet-api.md).</span></span>

> [!NOTE]
> <span data-ttu-id="20c49-109">Ne všechny scénáře Azure Active Directory a funkce jsou podporovány koncového bodu v2.0.</span><span class="sxs-lookup"><span data-stu-id="20c49-109">Not all Azure Active Directory scenarios & features are supported by the v2.0 endpoint.</span></span>  <span data-ttu-id="20c49-110">Pokud chcete zjistit, pokud byste měli používat koncový bod v2.0, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="20c49-110">To determine if you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="download-sample-code"></a><span data-ttu-id="20c49-111">Stáhněte si ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="20c49-111">Download sample code</span></span>
<span data-ttu-id="20c49-112">Kód k tomuto kurzu je udržovaný [na GitHubu](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet).</span><span class="sxs-lookup"><span data-stu-id="20c49-112">The code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet).</span></span>  <span data-ttu-id="20c49-113">Chcete-li sledovat, můžete [stáhnout kostru aplikace jako ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) nebo tuto kostru klonovat:</span><span class="sxs-lookup"><span data-stu-id="20c49-113">To follow along, you can [download the app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) or clone the skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

<span data-ttu-id="20c49-114">Alternativně můžete [stáhnout dokončené aplikace jako ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) nebo klonovat dokončené aplikace:</span><span class="sxs-lookup"><span data-stu-id="20c49-114">Alternatively, you can [download the completed app as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) or clone the completed app:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

## <a name="register-an-app"></a><span data-ttu-id="20c49-115">Registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="20c49-115">Register an app</span></span>
<span data-ttu-id="20c49-116">Vytvoření nové aplikace v [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo postupujte podle těchto [podrobné kroky](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="20c49-116">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="20c49-117">Zkontrolujte, že:</span><span class="sxs-lookup"><span data-stu-id="20c49-117">Make sure to:</span></span>

* <span data-ttu-id="20c49-118">Zkopírování **Id aplikace** přiřazené vaší aplikaci, budete ho potřebovat brzy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="20c49-118">Copy down the **Application Id** assigned to your app, you'll need it soon.</span></span>
* <span data-ttu-id="20c49-119">Vytvoření **tajný klíč aplikace** z **heslo** typu a poznamenejte jeho hodnoty pro později</span><span class="sxs-lookup"><span data-stu-id="20c49-119">Create an **App Secret** of the **Password** type, and copy down its value for later</span></span>
* <span data-ttu-id="20c49-120">Přidat **webové** platformu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="20c49-120">Add the **Web** platform for your app.</span></span>
* <span data-ttu-id="20c49-121">Zadejte správný **identifikátor URI pro přesměrování**.</span><span class="sxs-lookup"><span data-stu-id="20c49-121">Enter the correct **Redirect URI**.</span></span> <span data-ttu-id="20c49-122">Určuje identifikátor uri přesměrování do služby Azure AD, kde se mají směrovat ověřování odpovědí – výchozí hodnota pro tento kurz je `https://localhost:44326/`.</span><span class="sxs-lookup"><span data-stu-id="20c49-122">The redirect uri indicates to Azure AD where authentication responses should be directed - the default for this tutorial is `https://localhost:44326/`.</span></span>

## <a name="install-owin"></a><span data-ttu-id="20c49-123">Instalace OWIN</span><span class="sxs-lookup"><span data-stu-id="20c49-123">Install OWIN</span></span>
<span data-ttu-id="20c49-124">Přidání balíčků NuGet middleware OWIN pro `TodoList-WebApp` projektu pomocí konzoly Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="20c49-124">Add the OWIN middleware NuGet packages to the `TodoList-WebApp` project using the Package Manager Console.</span></span>  <span data-ttu-id="20c49-125">Middlewaru OWIN, který se použije k vydávání požadavků na přihlášení a odhlášení, spravovat relace uživatele a získat informace o uživateli, mimo jiné.</span><span class="sxs-lookup"><span data-stu-id="20c49-125">The OWIN middleware will be used to issue sign-in and sign-out requests, manage the user's session, and get information about the user, amongst other things.</span></span>

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoList-WebApp
```

## <a name="sign-the-user-in"></a><span data-ttu-id="20c49-126">Přihlášení uživatele</span><span class="sxs-lookup"><span data-stu-id="20c49-126">Sign the user in</span></span>
<span data-ttu-id="20c49-127">Teď nakonfigurovat middlewaru OWIN, který má být použit [ověřovacího protokolu OpenID Connect](active-directory-v2-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="20c49-127">Now configure the OWIN middleware to use the [OpenID Connect authentication protocol](active-directory-v2-protocols.md).</span></span>  

* <span data-ttu-id="20c49-128">Otevřete `web.config` soubor v kořenovém `TodoList-WebApp` projektu a zadejte hodnoty konfigurace vaší aplikace v `<appSettings>` oddílu.</span><span class="sxs-lookup"><span data-stu-id="20c49-128">Open the `web.config` file in the root of the `TodoList-WebApp` project, and enter your app's configuration values in the `<appSettings>` section.</span></span>
  * <span data-ttu-id="20c49-129">`ida:ClientId` Je **Id aplikace** přiřazené vaší aplikaci v portálu pro registraci.</span><span class="sxs-lookup"><span data-stu-id="20c49-129">The `ida:ClientId` is the **Application Id** assigned to your app in the registration portal.</span></span>
  * <span data-ttu-id="20c49-130">`ida:ClientSecret` Je **tajný klíč aplikace** jste vytvořili v portálu pro registraci.</span><span class="sxs-lookup"><span data-stu-id="20c49-130">The `ida:ClientSecret` is the **App Secret** you created in the registration portal.</span></span>
  * <span data-ttu-id="20c49-131">`ida:RedirectUri` Je **identifikátor Uri pro přesměrování** jste zadali v portálu.</span><span class="sxs-lookup"><span data-stu-id="20c49-131">The `ida:RedirectUri` is the **Redirect Uri** you entered in the portal.</span></span>
* <span data-ttu-id="20c49-132">Otevřete `web.config` soubor v kořenovém `TodoList-Service` projektu a nahraďte `ida:Audience` se stejným **Id aplikace** jako výše.</span><span class="sxs-lookup"><span data-stu-id="20c49-132">Open the `web.config` file in the root of the `TodoList-Service` project, and replace the `ida:Audience` with the same **Application Id** as above.</span></span>
* <span data-ttu-id="20c49-133">Otevřete soubor `App_Start\Startup.Auth.cs` a přidejte `using` příkazy pro knihovny z výše.</span><span class="sxs-lookup"><span data-stu-id="20c49-133">Open the file `App_Start\Startup.Auth.cs` and add `using` statements for the libraries from above.</span></span>
* <span data-ttu-id="20c49-134">Ve stejném souboru implementovat `ConfigureAuth(...)` metoda.</span><span class="sxs-lookup"><span data-stu-id="20c49-134">In the same file, implement the `ConfigureAuth(...)` method.</span></span>  <span data-ttu-id="20c49-135">Parametry, zadejte v `OpenIDConnectAuthenticationOptions` bude sloužit jako souřadnice pro vaši aplikaci komunikovat s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="20c49-135">The parameters you provide in `OpenIDConnectAuthenticationOptions` will serve as coordinates for your app to communicate with Azure AD.</span></span>

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
                    Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0 "),
                    Scope = "openid email profile offline_access",
                    RedirectUri = redirectUri,
                    PostLogoutRedirectUri = redirectUri,
                    TokenValidationParameters = new TokenValidationParameters
                    {
                        ValidateIssuer = false,
                    },

                    // The `AuthorizationCodeReceived` notification is used to capture and redeem the authorization_code that the v2.0 endpoint returns to your app.

                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed,
                        AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    }

        });
}
// ...
```

## <a name="use-msal-to-get-access-tokens"></a><span data-ttu-id="20c49-136">Použití MSAL získat přístupové tokeny</span><span class="sxs-lookup"><span data-stu-id="20c49-136">Use MSAL to get access tokens</span></span>
<span data-ttu-id="20c49-137">V `AuthorizationCodeReceived` oznámení, chceme používat [OAuth 2.0 v kombinaci s OpenID Connect](active-directory-v2-protocols.md) chcete uplatnit authorization_code pro token přístupu ke službě seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="20c49-137">In the `AuthorizationCodeReceived` notification, we want to use [OAuth 2.0 in tandem with OpenID Connect](active-directory-v2-protocols.md) to redeem the authorization_code for an access token to the To-Do List Service.</span></span>  <span data-ttu-id="20c49-138">MSAL můžete tento proces snadno pro vás provedl:</span><span class="sxs-lookup"><span data-stu-id="20c49-138">MSAL can make this process easy for you:</span></span>

* <span data-ttu-id="20c49-139">Nejdřív nainstalujte verzi preview MSAL:</span><span class="sxs-lookup"><span data-stu-id="20c49-139">First, install the preview version of MSAL:</span></span>

```PM> Install-Package Microsoft.Identity.Client -ProjectName TodoList-WebApp -IncludePrerelease```

* <span data-ttu-id="20c49-140">A přidat další `using` příkaz, který má `App_Start\Startup.Auth.cs` MSAL v souboru.</span><span class="sxs-lookup"><span data-stu-id="20c49-140">And add another `using` statement to the `App_Start\Startup.Auth.cs` file for MSAL.</span></span>
* <span data-ttu-id="20c49-141">Nyní přidejte nové metody, `OnAuthorizationCodeReceived` obslužné rutiny události.</span><span class="sxs-lookup"><span data-stu-id="20c49-141">Now add a new method, the `OnAuthorizationCodeReceived` event handler.</span></span>  <span data-ttu-id="20c49-142">Tato obslužná rutina použije MSAL získat token přístupu k rozhraní API seznamu úkolů a uloží token v mezipaměti na MSAL tokenu pro pozdější:</span><span class="sxs-lookup"><span data-stu-id="20c49-142">This handler will use MSAL to acquire an access token to the To-Do List API, and will store the token in MSAL's token cache for later:</span></span>

```C#
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
        string userObjectId = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        string tenantID = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
        string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenantID, string.Empty);
        ClientCredential cred = new ClientCredential(clientId, clientSecret);

        // Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
        app = new ConfidentialClientApplication(Startup.clientId, redirectUri, cred, new NaiveSessionCache(userObjectId, notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase)) {};
        var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { clientId }, notification.Code);
}
// ...
```

* <span data-ttu-id="20c49-143">Ve službě web apps má MSAL extensible mezipamětí tokenů, který slouží k uložení tokeny.</span><span class="sxs-lookup"><span data-stu-id="20c49-143">In web apps, MSAL has an extensible token cache that can be used to store tokens.</span></span>  <span data-ttu-id="20c49-144">Tato ukázka implementuje `NaiveSessionCache` úložiště relace http pro tokeny mezipaměti, který používá.</span><span class="sxs-lookup"><span data-stu-id="20c49-144">This sample implements the `NaiveSessionCache` which uses http session storage to cache tokens.</span></span>

<!-- TODO: Token Cache article -->


## <a name="call-the-web-api"></a><span data-ttu-id="20c49-145">Volání webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="20c49-145">Call the Web API</span></span>
<span data-ttu-id="20c49-146">Nyní je čas skutečně používáte access_token, které jste získali v kroku 3.</span><span class="sxs-lookup"><span data-stu-id="20c49-146">Now it's time to actually use the access_token you acquired in step 3.</span></span>  <span data-ttu-id="20c49-147">Otevřete web app `Controllers\TodoListController.cs` souboru, který provede všechny požadavky CRUD rozhraní API seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="20c49-147">Open the web app's `Controllers\TodoListController.cs` file, which makes all the CRUD requests to the To-Do List API.</span></span>

* <span data-ttu-id="20c49-148">Můžete MSAL znovu zde načíst access_tokens z mezipaměti MSAL.</span><span class="sxs-lookup"><span data-stu-id="20c49-148">You can use MSAL again here to fetch access_tokens from the MSAL cache.</span></span>  <span data-ttu-id="20c49-149">Nejprve přidejte `using` příkaz pro MSAL do tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="20c49-149">First, add a `using` statement for MSAL to this file.</span></span>
  
    `using Microsoft.Identity.Client;`
* <span data-ttu-id="20c49-150">V `Index` akce, použijte MSAL `AcquireTokenSilentAsync` metoda získat access_token, které je možné načíst data ze služby na seznam úkolů:</span><span class="sxs-lookup"><span data-stu-id="20c49-150">In the `Index` action, use MSAL's `AcquireTokenSilentAsync` method to get an access_token that can be used to read data from the To-Do List service:</span></span>

```C#
// ...
string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
string tenantID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
string authority = String.Format(CultureInfo.InvariantCulture, Startup.aadInstance, tenantID, string.Empty);
ClientCredential credential = new ClientCredential(Startup.clientId, Startup.clientSecret);

// Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
app = new ConfidentialClientApplication(Startup.clientId, redirectUri, credential, new NaiveSessionCache(userObjectID, this.HttpContext)){};
result = await app.AcquireTokenSilentAsync(new string[] { Startup.clientId });
// ...
```

* <span data-ttu-id="20c49-151">Ukázka pak přidá výsledný token na požadavek HTTP GET `Authorization` hlavičky, která služba seznam úkolů používá k ověření žádosti.</span><span class="sxs-lookup"><span data-stu-id="20c49-151">The sample then adds the resulting token to the HTTP GET request as the `Authorization` header, which the To-Do List service uses to authenticate the request.</span></span>
* <span data-ttu-id="20c49-152">Pokud vrátí seznam úkolů služby `401 Unauthorized` odpovědi, access_tokens v MSAL mít zneplatní z nějakého důvodu.</span><span class="sxs-lookup"><span data-stu-id="20c49-152">If the To-Do List service returns a `401 Unauthorized` response, the access_tokens in MSAL have become invalid for some reason.</span></span>  <span data-ttu-id="20c49-153">V takovém případě by měl vyřaďte všechny access_tokens z mezipaměti MSAL a zobrazí uživateli zprávu, která budou muset přihlásit znovu, který bude restartován tok tokenu pořízení.</span><span class="sxs-lookup"><span data-stu-id="20c49-153">In this case, you should drop any access_tokens from the MSAL cache and show the user a message that they may need to sign in again, which will restart the token acquisition flow.</span></span>

```C#
// ...
// If the call failed with access denied, then drop the current access token from the cache,
// and show the user an error indicating they might need to sign-in again.
if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
{
        app.AppTokenCache.Clear(Startup.clientId);

        return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
}
// ...
```

* <span data-ttu-id="20c49-154">Podobně pokud MSAL nelze vrátit access_token z jakéhokoli důvodu, byste měli vyzvat uživatele se znovu přihlásit.</span><span class="sxs-lookup"><span data-stu-id="20c49-154">Similarly, if MSAL is unable to return an access_token for any reason, you should instruct the user to sign in again.</span></span>  <span data-ttu-id="20c49-155">To je jednoduché, zachytávání žádné `MSALException`:</span><span class="sxs-lookup"><span data-stu-id="20c49-155">This is as simple as catching any `MSALException`:</span></span>

```C#
// ...
catch (MsalException ee)
{
        // If MSAL could not get a token silently, show the user an error indicating they might need to sign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ee.Message + " You might need to log out and log back in.");
}
// ...
```

* <span data-ttu-id="20c49-156">Stejné přesnou `AcquireTokenSilentAsync` volání je implementd v `Create` a `Delete` akce.</span><span class="sxs-lookup"><span data-stu-id="20c49-156">The exact same `AcquireTokenSilentAsync` call is implementd in the `Create` and `Delete` actions.</span></span>  <span data-ttu-id="20c49-157">Ve službě web apps můžete použít tuto metodu MSAL získat access_tokens kdykoli budete potřebovat ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="20c49-157">In web apps, you can use this MSAL method to get access_tokens whenever you need them in your app.</span></span>  <span data-ttu-id="20c49-158">MSAL se postará o získávání, ukládání do mezipaměti a aktualizaci tokeny pro vás.</span><span class="sxs-lookup"><span data-stu-id="20c49-158">MSAL will take care of acquiring, caching, and refreshing tokens for you.</span></span>

<span data-ttu-id="20c49-159">Nakonec sestavte a spusťte aplikaci!</span><span class="sxs-lookup"><span data-stu-id="20c49-159">Finally, build and run your app!</span></span>  <span data-ttu-id="20c49-160">Přihlaste se pomocí Account Microsoft nebo účtu Azure AD a Všimněte si, jak se odrazí identitu uživatelů v horním navigačním panelu.</span><span class="sxs-lookup"><span data-stu-id="20c49-160">Sign in with either a Microsoft Account or an Azure AD Account, and notice how the user's identity is reflected in the top navigation bar.</span></span>  <span data-ttu-id="20c49-161">Přidat a odstranit některé položky ze seznamu úkolů uživatele chcete zjistit že OAuth 2.0 zabezpečená volání rozhraní API v akci.</span><span class="sxs-lookup"><span data-stu-id="20c49-161">Add and delete some items from the user's To-Do List to see the OAuth 2.0 secured API calls in action.</span></span>  <span data-ttu-id="20c49-162">Nyní máte webovou aplikaci & webového rozhraní API, jak zabezpečit pomocí standardních protokolů, které může ověřit uživatele s svoje osobní, tak i pracovní nebo školní účty.</span><span class="sxs-lookup"><span data-stu-id="20c49-162">You now have a web app & web API, both secured using industry standard protocols, that can authenticate users with both their personal and work/school accounts.</span></span>

<span data-ttu-id="20c49-163">Pro srovnání je hotová ukázka (bez vašich hodnot nastavení) [je tady uvedené](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="20c49-163">For reference, the completed sample (without your configuration values) [is provided here](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip).</span></span>  

## <a name="next-steps"></a><span data-ttu-id="20c49-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="20c49-164">Next Steps</span></span>
<span data-ttu-id="20c49-165">Další zdroje projděte si:</span><span class="sxs-lookup"><span data-stu-id="20c49-165">For additional resources, check out:</span></span>

* [<span data-ttu-id="20c49-166">Příručka vývojáře v2.0 >></span><span class="sxs-lookup"><span data-stu-id="20c49-166">The v2.0 developer guide >></span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="20c49-167">Značka StackOverflow "azure-active-directory" >></span><span class="sxs-lookup"><span data-stu-id="20c49-167">StackOverflow "azure-active-directory" tag >></span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="20c49-168">Získejte bezpečnostní aktualizace našich produktů</span><span class="sxs-lookup"><span data-stu-id="20c49-168">Get security updates for our products</span></span>
<span data-ttu-id="20c49-169">Doporučujeme vám získávat oznámení o bezpečnostních incidentech tak, že navštívíte [tuto stránku](https://technet.microsoft.com/security/dd252948) a přihlásíte se k odběru služby Security Advisory Alerts.</span><span class="sxs-lookup"><span data-stu-id="20c49-169">We encourage you to get notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing to Security Advisory Alerts.</span></span>

