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
# <a name="calling-a-web-api-from-a-net-web-app"></a><span data-ttu-id="4b109-103">Volání webového rozhraní API z webové aplikace .NET</span><span class="sxs-lookup"><span data-stu-id="4b109-103">Calling a web API from a .NET web app</span></span>
<span data-ttu-id="4b109-104">S koncovým bodem v2.0 hello můžete rychle přidat ověřování tooyour webové aplikace a webové rozhraní API s podporou pro oba osobní účty Microsoft a pracovní nebo školní účty.</span><span class="sxs-lookup"><span data-stu-id="4b109-104">With hello v2.0 endpoint, you can quickly add authentication tooyour web apps and web APIs with support for both personal Microsoft accounts and work or school accounts.</span></span>  <span data-ttu-id="4b109-105">Zde jsme budete vytvářet webové aplikace MVC, která podepisuje uživatele pomocí OpenID Connect, s pomoc od společnosti Microsoft OWIN middleware.</span><span class="sxs-lookup"><span data-stu-id="4b109-105">Here, we'll build an MVC web app that signs users in using OpenID Connect, with some help from Microsoft's OWIN middleware.</span></span>  <span data-ttu-id="4b109-106">Hello webové aplikace se získání přístupových tokenů OAuth 2.0 pro webové rozhraní api, které jsou zabezpečeny OAuth 2.0, který umožňuje vytvořit, číst a odstranit na daného uživatele "seznam úkolů".</span><span class="sxs-lookup"><span data-stu-id="4b109-106">hello web app will get OAuth 2.0 access tokens for a web api secured by OAuth 2.0 that allows create, read, and delete on a given user's "To-Do List".</span></span>

<span data-ttu-id="4b109-107">V tomto kurzu se soustředí hlavně na pomocí MSAL tooacquire a pomocí přístupových tokenů ve webové aplikaci, popsané v úplné [zde](active-directory-v2-flows.md#web-apps).</span><span class="sxs-lookup"><span data-stu-id="4b109-107">This tutorial will focus primarily on using MSAL tooacquire and use access tokens in a web app, described in full [here](active-directory-v2-flows.md#web-apps).</span></span>  <span data-ttu-id="4b109-108">Jako požadavky, můžete toofirst zjistěte, jak příliš[přidat základní tooa přihlašovací webovou aplikaci](active-directory-v2-devquickstarts-dotnet-web.md) nebo jak příliš[správně zabezpečení webového rozhraní API](active-directory-v2-devquickstarts-dotnet-api.md).</span><span class="sxs-lookup"><span data-stu-id="4b109-108">As prerequisites, you may want toofirst learn how too[add basic sign-in tooa web app](active-directory-v2-devquickstarts-dotnet-web.md) or how too[properly secure a web API](active-directory-v2-devquickstarts-dotnet-api.md).</span></span>

> [!NOTE]
> <span data-ttu-id="4b109-109">Ne všechny scénáře Azure Active Directory a funkce jsou podporovány koncového bodu v2.0 hello.</span><span class="sxs-lookup"><span data-stu-id="4b109-109">Not all Azure Active Directory scenarios & features are supported by hello v2.0 endpoint.</span></span>  <span data-ttu-id="4b109-110">toodetermine Pokud byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="4b109-110">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="download-sample-code"></a><span data-ttu-id="4b109-111">Stáhněte si ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="4b109-111">Download sample code</span></span>
<span data-ttu-id="4b109-112">Hello kód v tomto kurzu se udržuje [na Githubu](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet).</span><span class="sxs-lookup"><span data-stu-id="4b109-112">hello code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet).</span></span>  <span data-ttu-id="4b109-113">toofollow společně, můžete [stáhnout kostru aplikace hello jako ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) nebo hello kostru klonovat:</span><span class="sxs-lookup"><span data-stu-id="4b109-113">toofollow along, you can [download hello app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) or clone hello skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

<span data-ttu-id="4b109-114">Alternativně můžete [stáhnout aplikaci hello dokončit jako ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) nebo klonování hello dokončit aplikace:</span><span class="sxs-lookup"><span data-stu-id="4b109-114">Alternatively, you can [download hello completed app as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) or clone hello completed app:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

## <a name="register-an-app"></a><span data-ttu-id="4b109-115">Registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="4b109-115">Register an app</span></span>
<span data-ttu-id="4b109-116">Vytvoření nové aplikace v [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo postupujte podle těchto [podrobné kroky](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="4b109-116">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="4b109-117">Zkontrolujte, že:</span><span class="sxs-lookup"><span data-stu-id="4b109-117">Make sure to:</span></span>

* <span data-ttu-id="4b109-118">Poznamenejte hello **Id aplikace** přiřazen tooyour aplikace, budete ho potřebovat brzy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="4b109-118">Copy down hello **Application Id** assigned tooyour app, you'll need it soon.</span></span>
* <span data-ttu-id="4b109-119">Vytvoření **tajný klíč aplikace** Dobrý den **heslo** typu a poznamenejte jeho hodnoty pro později</span><span class="sxs-lookup"><span data-stu-id="4b109-119">Create an **App Secret** of hello **Password** type, and copy down its value for later</span></span>
* <span data-ttu-id="4b109-120">Přidat hello **webové** platformu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4b109-120">Add hello **Web** platform for your app.</span></span>
* <span data-ttu-id="4b109-121">Zadejte správný hello **identifikátor URI pro přesměrování**.</span><span class="sxs-lookup"><span data-stu-id="4b109-121">Enter hello correct **Redirect URI**.</span></span> <span data-ttu-id="4b109-122">Hello identifikátor uri přesměrování označuje tooAzure AD, kde se mají směrovat ověřování odpovědí – hello výchozí hodnota pro tento kurz je `https://localhost:44326/`.</span><span class="sxs-lookup"><span data-stu-id="4b109-122">hello redirect uri indicates tooAzure AD where authentication responses should be directed - hello default for this tutorial is `https://localhost:44326/`.</span></span>

## <a name="install-owin"></a><span data-ttu-id="4b109-123">Instalace OWIN</span><span class="sxs-lookup"><span data-stu-id="4b109-123">Install OWIN</span></span>
<span data-ttu-id="4b109-124">Přidat hello OWIN middleware NuGet balíčky toohello `TodoList-WebApp` projekt pomocí hello Konzola správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="4b109-124">Add hello OWIN middleware NuGet packages toohello `TodoList-WebApp` project using hello Package Manager Console.</span></span>  <span data-ttu-id="4b109-125">Hello OWIN middleware bude použité tooissue požadavků na přihlášení a odhlášení, spravovat hello uživatelské relace a získat informace o uživateli hello, mimo jiné.</span><span class="sxs-lookup"><span data-stu-id="4b109-125">hello OWIN middleware will be used tooissue sign-in and sign-out requests, manage hello user's session, and get information about hello user, amongst other things.</span></span>

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoList-WebApp
```

## <a name="sign-hello-user-in"></a><span data-ttu-id="4b109-126">Přihlášení uživatele hello v</span><span class="sxs-lookup"><span data-stu-id="4b109-126">Sign hello user in</span></span>
<span data-ttu-id="4b109-127">Teď nakonfigurovat hello OWIN middleware toouse hello [ověřovacího protokolu OpenID Connect](active-directory-v2-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="4b109-127">Now configure hello OWIN middleware toouse hello [OpenID Connect authentication protocol](active-directory-v2-protocols.md).</span></span>  

* <span data-ttu-id="4b109-128">Otevřete hello `web.config` soubor v kořenovém adresáři hello hello `TodoList-WebApp` projektu a zadejte hodnoty konfigurace vaší aplikace v hello `<appSettings>` části.</span><span class="sxs-lookup"><span data-stu-id="4b109-128">Open hello `web.config` file in hello root of hello `TodoList-WebApp` project, and enter your app's configuration values in hello `<appSettings>` section.</span></span>
  * <span data-ttu-id="4b109-129">Hello `ida:ClientId` je hello **Id aplikace** přiřazené tooyour aplikace v portálu pro registraci hello.</span><span class="sxs-lookup"><span data-stu-id="4b109-129">hello `ida:ClientId` is hello **Application Id** assigned tooyour app in hello registration portal.</span></span>
  * <span data-ttu-id="4b109-130">Hello `ida:ClientSecret` je hello **tajný klíč aplikace** jste vytvořili v portálu pro registraci hello.</span><span class="sxs-lookup"><span data-stu-id="4b109-130">hello `ida:ClientSecret` is hello **App Secret** you created in hello registration portal.</span></span>
  * <span data-ttu-id="4b109-131">Hello `ida:RedirectUri` je hello **identifikátor Uri pro přesměrování** jste zadali v portálu hello.</span><span class="sxs-lookup"><span data-stu-id="4b109-131">hello `ida:RedirectUri` is hello **Redirect Uri** you entered in hello portal.</span></span>
* <span data-ttu-id="4b109-132">Otevřete hello `web.config` soubor v kořenovém adresáři hello hello `TodoList-Service` projektu a nahraďte hello `ida:Audience` s hello stejné **Id aplikace** jako výše.</span><span class="sxs-lookup"><span data-stu-id="4b109-132">Open hello `web.config` file in hello root of hello `TodoList-Service` project, and replace hello `ida:Audience` with hello same **Application Id** as above.</span></span>
* <span data-ttu-id="4b109-133">Soubor otevřete hello `App_Start\Startup.Auth.cs` a přidejte `using` příkazy pro knihovny hello z výše.</span><span class="sxs-lookup"><span data-stu-id="4b109-133">Open hello file `App_Start\Startup.Auth.cs` and add `using` statements for hello libraries from above.</span></span>
* <span data-ttu-id="4b109-134">V hello stejného souboru, implementovat hello `ConfigureAuth(...)` metoda.</span><span class="sxs-lookup"><span data-stu-id="4b109-134">In hello same file, implement hello `ConfigureAuth(...)` method.</span></span>  <span data-ttu-id="4b109-135">Hello parametry, které zadáte v `OpenIDConnectAuthenticationOptions` bude sloužit jako souřadnice toocommunicate vaší aplikace s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4b109-135">hello parameters you provide in `OpenIDConnectAuthenticationOptions` will serve as coordinates for your app toocommunicate with Azure AD.</span></span>

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

## <a name="use-msal-tooget-access-tokens"></a><span data-ttu-id="4b109-136">Použít MSAL tooget přístupové tokeny</span><span class="sxs-lookup"><span data-stu-id="4b109-136">Use MSAL tooget access tokens</span></span>
<span data-ttu-id="4b109-137">V hello `AuthorizationCodeReceived` oznámení, chceme toouse [OAuth 2.0 v kombinaci s OpenID Connect](active-directory-v2-protocols.md) tooredeem hello authorization_code pro token toohello k přístupu služby seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="4b109-137">In hello `AuthorizationCodeReceived` notification, we want toouse [OAuth 2.0 in tandem with OpenID Connect](active-directory-v2-protocols.md) tooredeem hello authorization_code for an access token toohello To-Do List Service.</span></span>  <span data-ttu-id="4b109-138">MSAL můžete tento proces snadno pro vás provedl:</span><span class="sxs-lookup"><span data-stu-id="4b109-138">MSAL can make this process easy for you:</span></span>

* <span data-ttu-id="4b109-139">Nejdřív nainstalujte verzi preview hello MSAL:</span><span class="sxs-lookup"><span data-stu-id="4b109-139">First, install hello preview version of MSAL:</span></span>

```PM> Install-Package Microsoft.Identity.Client -ProjectName TodoList-WebApp -IncludePrerelease```

* <span data-ttu-id="4b109-140">A přidat další `using` příkaz toohello `App_Start\Startup.Auth.cs` MSAL v souboru.</span><span class="sxs-lookup"><span data-stu-id="4b109-140">And add another `using` statement toohello `App_Start\Startup.Auth.cs` file for MSAL.</span></span>
* <span data-ttu-id="4b109-141">Nyní přidejte nová metoda hello `OnAuthorizationCodeReceived` obslužné rutiny události.</span><span class="sxs-lookup"><span data-stu-id="4b109-141">Now add a new method, hello `OnAuthorizationCodeReceived` event handler.</span></span>  <span data-ttu-id="4b109-142">Tato obslužná rutina použije MSAL tooacquire tokenu toohello přístupu k rozhraní API seznamu úkolů a uloží hello token v mezipaměti na MSAL tokenu pro pozdější:</span><span class="sxs-lookup"><span data-stu-id="4b109-142">This handler will use MSAL tooacquire an access token toohello To-Do List API, and will store hello token in MSAL's token cache for later:</span></span>

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

* <span data-ttu-id="4b109-143">Ve službě web apps má MSAL extensible mezipamětí tokenů, které můžou být použité toostore tokeny.</span><span class="sxs-lookup"><span data-stu-id="4b109-143">In web apps, MSAL has an extensible token cache that can be used toostore tokens.</span></span>  <span data-ttu-id="4b109-144">Tato ukázka implementuje hello `NaiveSessionCache` tokeny toocache úložiště relace http, který používá.</span><span class="sxs-lookup"><span data-stu-id="4b109-144">This sample implements hello `NaiveSessionCache` which uses http session storage toocache tokens.</span></span>

<!-- TODO: Token Cache article -->


## <a name="call-hello-web-api"></a><span data-ttu-id="4b109-145">Hello volání webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="4b109-145">Call hello Web API</span></span>
<span data-ttu-id="4b109-146">Nyní je čas tooactually použít hello access_token, které jste získali v kroku 3.</span><span class="sxs-lookup"><span data-stu-id="4b109-146">Now it's time tooactually use hello access_token you acquired in step 3.</span></span>  <span data-ttu-id="4b109-147">Otevřete hello webové aplikace `Controllers\TodoListController.cs` souboru, takže všechny hello CRUD požadavky toohello rozhraní API seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="4b109-147">Open hello web app's `Controllers\TodoListController.cs` file, which makes all hello CRUD requests toohello To-Do List API.</span></span>

* <span data-ttu-id="4b109-148">Můžete použít MSAL znovu zde toofetch access_tokens z hello MSAL mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="4b109-148">You can use MSAL again here toofetch access_tokens from hello MSAL cache.</span></span>  <span data-ttu-id="4b109-149">Nejprve přidejte `using` příkaz MSAL toothis souboru.</span><span class="sxs-lookup"><span data-stu-id="4b109-149">First, add a `using` statement for MSAL toothis file.</span></span>
  
    `using Microsoft.Identity.Client;`
* <span data-ttu-id="4b109-150">V hello `Index` akce, použijte MSAL `AcquireTokenSilentAsync` metoda tooget access_token, kterou lze použít tooread data z hello seznam úkolů služby:</span><span class="sxs-lookup"><span data-stu-id="4b109-150">In hello `Index` action, use MSAL's `AcquireTokenSilentAsync` method tooget an access_token that can be used tooread data from hello To-Do List service:</span></span>

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

* <span data-ttu-id="4b109-151">Hello ukázka potom přidá hello výsledný token toohello požadavek HTTP GET jako hello `Authorization` záhlaví, seznam úkolů služby, která hello používá tooauthenticate hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="4b109-151">hello sample then adds hello resulting token toohello HTTP GET request as hello `Authorization` header, which hello To-Do List service uses tooauthenticate hello request.</span></span>
* <span data-ttu-id="4b109-152">Pokud vrátí hello seznam úkolů služby `401 Unauthorized` odpovědi, hello access_tokens v MSAL mít zneplatní z nějakého důvodu.</span><span class="sxs-lookup"><span data-stu-id="4b109-152">If hello To-Do List service returns a `401 Unauthorized` response, hello access_tokens in MSAL have become invalid for some reason.</span></span>  <span data-ttu-id="4b109-153">V takovém případě by měl vyřaďte všechny access_tokens z hello MSAL mezipaměti a hello uživatelům zobrazí zpráva, že potřebují toosign v akci, která restartuje tok tokenu pořízení hello.</span><span class="sxs-lookup"><span data-stu-id="4b109-153">In this case, you should drop any access_tokens from hello MSAL cache and show hello user a message that they may need toosign in again, which will restart hello token acquisition flow.</span></span>

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

* <span data-ttu-id="4b109-154">Podobně pokud MSAL nelze tooreturn access_token z jakéhokoli důvodu, byste měli vyzvat, hello toosign uživatele v akci.</span><span class="sxs-lookup"><span data-stu-id="4b109-154">Similarly, if MSAL is unable tooreturn an access_token for any reason, you should instruct hello user toosign in again.</span></span>  <span data-ttu-id="4b109-155">To je jednoduché, zachytávání žádné `MSALException`:</span><span class="sxs-lookup"><span data-stu-id="4b109-155">This is as simple as catching any `MSALException`:</span></span>

```C#
// ...
catch (MsalException ee)
{
        // If MSAL could not get a token silently, show hello user an error indicating they might need toosign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading tooDo List: " + ee.Message + " You might need toolog out and log back in.");
}
// ...
```

* <span data-ttu-id="4b109-156">Hello přesnou stejné `AcquireTokenSilentAsync` volání je implementd v hello `Create` a `Delete` akce.</span><span class="sxs-lookup"><span data-stu-id="4b109-156">hello exact same `AcquireTokenSilentAsync` call is implementd in hello `Create` and `Delete` actions.</span></span>  <span data-ttu-id="4b109-157">Ve službě web apps můžete použít tento MSAL metoda tooget access_tokens kdykoli budete potřebovat ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4b109-157">In web apps, you can use this MSAL method tooget access_tokens whenever you need them in your app.</span></span>  <span data-ttu-id="4b109-158">MSAL se postará o získávání, ukládání do mezipaměti a aktualizaci tokeny pro vás.</span><span class="sxs-lookup"><span data-stu-id="4b109-158">MSAL will take care of acquiring, caching, and refreshing tokens for you.</span></span>

<span data-ttu-id="4b109-159">Nakonec sestavte a spusťte aplikaci!</span><span class="sxs-lookup"><span data-stu-id="4b109-159">Finally, build and run your app!</span></span>  <span data-ttu-id="4b109-160">Přihlaste se pomocí Account Microsoft nebo účtu Azure AD a Všimněte si, jak identitu uživatele hello se odrazí v horním navigačním panelu hello.</span><span class="sxs-lookup"><span data-stu-id="4b109-160">Sign in with either a Microsoft Account or an Azure AD Account, and notice how hello user's identity is reflected in hello top navigation bar.</span></span>  <span data-ttu-id="4b109-161">Přidat a odstranit některé položky ze seznamu úkolů uživatele hello hello toosee OAuth 2.0 zabezpečené rozhraní API se volá v akci.</span><span class="sxs-lookup"><span data-stu-id="4b109-161">Add and delete some items from hello user's To-Do List toosee hello OAuth 2.0 secured API calls in action.</span></span>  <span data-ttu-id="4b109-162">Nyní máte webovou aplikaci & webového rozhraní API, jak zabezpečit pomocí standardních protokolů, které může ověřit uživatele s svoje osobní, tak i pracovní nebo školní účty.</span><span class="sxs-lookup"><span data-stu-id="4b109-162">You now have a web app & web API, both secured using industry standard protocols, that can authenticate users with both their personal and work/school accounts.</span></span>

<span data-ttu-id="4b109-163">Pro referenci hello dokončit ukázka (bez vašich hodnot nastavení) [je tady uvedené](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="4b109-163">For reference, hello completed sample (without your configuration values) [is provided here](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip).</span></span>  

## <a name="next-steps"></a><span data-ttu-id="4b109-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4b109-164">Next Steps</span></span>
<span data-ttu-id="4b109-165">Další zdroje projděte si:</span><span class="sxs-lookup"><span data-stu-id="4b109-165">For additional resources, check out:</span></span>

* [<span data-ttu-id="4b109-166">Příručka vývojáře v2.0 Hello >></span><span class="sxs-lookup"><span data-stu-id="4b109-166">hello v2.0 developer guide >></span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="4b109-167">Značka StackOverflow "azure-active-directory" >></span><span class="sxs-lookup"><span data-stu-id="4b109-167">StackOverflow "azure-active-directory" tag >></span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="4b109-168">Získejte bezpečnostní aktualizace našich produktů</span><span class="sxs-lookup"><span data-stu-id="4b109-168">Get security updates for our products</span></span>
<span data-ttu-id="4b109-169">Doporučujeme vám tooget oznámení o bezpečnostních incidentech navštivte stránky [tuto stránku](https://technet.microsoft.com/security/dd252948) a přihlášení k odběru tooSecurity Advisory Alerts.</span><span class="sxs-lookup"><span data-stu-id="4b109-169">We encourage you tooget notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing tooSecurity Advisory Alerts.</span></span>

