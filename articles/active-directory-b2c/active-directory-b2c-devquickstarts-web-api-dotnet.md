---
title: aaaAzure Active Directory B2C | Microsoft Docs
description: "Jak toobuild .NET webové aplikace a volání webového rozhraní api pomocí přístupových tokenů Azure Active Directory B2C a OAuth 2.0."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: krassk
editor: 
ms.assetid: d3888556-2647-4a42-b068-027f9374aa61
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/17/2017
ms.author: parakhj
ms.openlocfilehash: 9b248e3bf18968e12aae73c07083fa8278befb3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-call-a-net-web-api-from-a-net-web-app"></a><span data-ttu-id="fda48-103">Azure AD B2C: Volání webového rozhraní API .NET z webové aplikace .NET</span><span class="sxs-lookup"><span data-stu-id="fda48-103">Azure AD B2C: Call a .NET web API from a .NET web app</span></span>

<span data-ttu-id="fda48-104">Pomocí Azure AD B2C můžete přidat výkonné identity management funkce tooyour webových aplikací a webových rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fda48-104">By using Azure AD B2C, you can add powerful identity management features tooyour web apps and web APIs.</span></span> <span data-ttu-id="fda48-105">Tento článek popisuje, jak toorequest přístup tokeny a volání z .NET "seznam úkolů" tooa webové aplikace .NET webové rozhraní api.</span><span class="sxs-lookup"><span data-stu-id="fda48-105">This article discusses how toorequest access tokens and make calls from a .NET "to-do list" web app tooa .NET web api.</span></span>

<span data-ttu-id="fda48-106">Tento článek nepopisuje jak tooimplement přihlášení, registraci a profil správy s Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="fda48-106">This article does not cover how tooimplement sign-in, sign-up and profile management with Azure AD B2C.</span></span> <span data-ttu-id="fda48-107">Zaměřuje se na volání webových rozhraní API po hello uživatel je již ověřen.</span><span class="sxs-lookup"><span data-stu-id="fda48-107">It focuses on calling web APIs after hello user is already authenticated.</span></span> <span data-ttu-id="fda48-108">Pokud jste to ještě neudělali, měli byste:</span><span class="sxs-lookup"><span data-stu-id="fda48-108">If you haven't already, you should:</span></span>

* <span data-ttu-id="fda48-109">Začínáme s [webové aplikace .NET](active-directory-b2c-devquickstarts-web-dotnet-susi.md)</span><span class="sxs-lookup"><span data-stu-id="fda48-109">Get started with a [.NET web app](active-directory-b2c-devquickstarts-web-dotnet-susi.md)</span></span>
* <span data-ttu-id="fda48-110">Začínáme s [webové rozhraní api .NET](active-directory-b2c-devquickstarts-api-dotnet.md)</span><span class="sxs-lookup"><span data-stu-id="fda48-110">Get started with a [.NET web api](active-directory-b2c-devquickstarts-api-dotnet.md)</span></span>

## <a name="prerequisite"></a><span data-ttu-id="fda48-111">Požadavek</span><span class="sxs-lookup"><span data-stu-id="fda48-111">Prerequisite</span></span>

<span data-ttu-id="fda48-112">toobuild webové aplikace, která volá webové rozhraní api, potřebujete:</span><span class="sxs-lookup"><span data-stu-id="fda48-112">toobuild a web application that calls a web api, you need to:</span></span>

1. <span data-ttu-id="fda48-113">[Vytvoření klienta Azure AD B2C](active-directory-b2c-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="fda48-113">[Create an Azure AD B2C tenant](active-directory-b2c-get-started.md).</span></span>
2. <span data-ttu-id="fda48-114">[Zaregistrovat webového rozhraní api](active-directory-b2c-app-registration.md#register-a-web-api).</span><span class="sxs-lookup"><span data-stu-id="fda48-114">[Register a web api](active-directory-b2c-app-registration.md#register-a-web-api).</span></span>
3. <span data-ttu-id="fda48-115">[Webovou aplikaci zaregistrovat](active-directory-b2c-app-registration.md#register-a-web-app).</span><span class="sxs-lookup"><span data-stu-id="fda48-115">[Register a web app](active-directory-b2c-app-registration.md#register-a-web-app).</span></span>
4. <span data-ttu-id="fda48-116">[Nastavit zásady](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="fda48-116">[Set up policies](active-directory-b2c-reference-policies.md).</span></span>
5. <span data-ttu-id="fda48-117">[Udělení hello webové aplikace oprávnění toouse hello webové rozhraní api](active-directory-b2c-access-tokens.md#publishing-permissions).</span><span class="sxs-lookup"><span data-stu-id="fda48-117">[Grant hello web app permissions toouse hello web api](active-directory-b2c-access-tokens.md#publishing-permissions).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fda48-118">Hello klientská aplikace a webové rozhraní API musí používat adresář hello stejnou službou Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="fda48-118">hello client application and web API must use hello same Azure AD B2C directory.</span></span>
>

## <a name="download-hello-code"></a><span data-ttu-id="fda48-119">Stáhněte si kód hello</span><span class="sxs-lookup"><span data-stu-id="fda48-119">Download hello code</span></span>

<span data-ttu-id="fda48-120">Hello kódu pro účely tohoto kurzu je udržovaný na [Githubu](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span><span class="sxs-lookup"><span data-stu-id="fda48-120">hello code for this tutorial is maintained on [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span></span> <span data-ttu-id="fda48-121">Ukázka hello může klonovat spuštěním:</span><span class="sxs-lookup"><span data-stu-id="fda48-121">You can clone hello sample by running:</span></span>

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

<span data-ttu-id="fda48-122">Po stažení ukázkového kódu hello hello otevřete Visual Studio .sln souboru tooget spustit.</span><span class="sxs-lookup"><span data-stu-id="fda48-122">After you download hello sample code, open hello Visual Studio .sln file tooget started.</span></span> <span data-ttu-id="fda48-123">Hello soubor řešení obsahuje dva projekty: `TaskWebApp` a `TaskService`.</span><span class="sxs-lookup"><span data-stu-id="fda48-123">hello solution file contains two projects: `TaskWebApp` and `TaskService`.</span></span> <span data-ttu-id="fda48-124">`TaskWebApp`je webová aplikace MVC, která hello uživatel komunikuje se službou.</span><span class="sxs-lookup"><span data-stu-id="fda48-124">`TaskWebApp` is a MVC web application that hello user interacts with.</span></span> <span data-ttu-id="fda48-125">`TaskService`je back endové webové rozhraní API hello aplikace, které ukládá seznam úkolů každého uživatele.</span><span class="sxs-lookup"><span data-stu-id="fda48-125">`TaskService` is hello app's back-end web API that stores each user's to-do list.</span></span> <span data-ttu-id="fda48-126">Tento článek nepopisuje vytváření hello `TaskWebApp` webovou aplikaci nebo hello `TaskService` webové rozhraní api.</span><span class="sxs-lookup"><span data-stu-id="fda48-126">This article does not cover building hello `TaskWebApp` web app or hello `TaskService` web api.</span></span> <span data-ttu-id="fda48-127">toolearn způsobu toobuild hello .NET webové aplikace pomocí Azure AD B2C, najdete v našich [kurz .NET pro webové aplikace](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span><span class="sxs-lookup"><span data-stu-id="fda48-127">toolearn how toobuild hello .NET web app using Azure AD B2C, see our [.NET web app tutorial](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span></span> <span data-ttu-id="fda48-128">toolearn jak toobuild hello .NET webové rozhraní API zabezpečené pomocí Azure AD B2C, najdete v našich [kurz webové rozhraní API .NET](active-directory-b2c-devquickstarts-api-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="fda48-128">toolearn how toobuild hello .NET web API secured using Azure AD B2C, see our [.NET web API tutorial](active-directory-b2c-devquickstarts-api-dotnet.md).</span></span>

### <a name="update-hello-azure-ad-b2c-configuration"></a><span data-ttu-id="fda48-129">Aktualizace konfigurace hello Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="fda48-129">Update hello Azure AD B2C configuration</span></span>

<span data-ttu-id="fda48-130">Naše ukázka je ID zásad a klienta hello nakonfigurované toouse naše ukázka klienta.</span><span class="sxs-lookup"><span data-stu-id="fda48-130">Our sample is configured toouse hello policies and client ID of our demo tenant.</span></span> <span data-ttu-id="fda48-131">Pokud chcete toouse vlastního klienta:</span><span class="sxs-lookup"><span data-stu-id="fda48-131">If you would like toouse your own tenant:</span></span>

1. <span data-ttu-id="fda48-132">Otevřete `web.config` v hello `TaskService` projektu a nahraďte hodnoty hello</span><span class="sxs-lookup"><span data-stu-id="fda48-132">Open `web.config` in hello `TaskService` project and replace hello values for</span></span>

    * <span data-ttu-id="fda48-133">`ida:Tenant` názvem vašeho tenanta</span><span class="sxs-lookup"><span data-stu-id="fda48-133">`ida:Tenant` with your tenant name</span></span>
    * <span data-ttu-id="fda48-134">`ida:ClientId`pomocí svého ID aplikace webového rozhraní api</span><span class="sxs-lookup"><span data-stu-id="fda48-134">`ida:ClientId` with your web api application ID</span></span>
    * <span data-ttu-id="fda48-135">`ida:SignUpSignInPolicyId` názvem zásady registrace/přihlášení</span><span class="sxs-lookup"><span data-stu-id="fda48-135">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>

2. <span data-ttu-id="fda48-136">Otevřete `web.config` v hello `TaskWebApp` projektu a nahraďte hodnoty hello</span><span class="sxs-lookup"><span data-stu-id="fda48-136">Open `web.config` in hello `TaskWebApp` project and replace hello values for</span></span>

    * <span data-ttu-id="fda48-137">`ida:Tenant` názvem vašeho tenanta</span><span class="sxs-lookup"><span data-stu-id="fda48-137">`ida:Tenant` with your tenant name</span></span>
    * <span data-ttu-id="fda48-138">`ida:ClientId` identifikátorem webového aplikace</span><span class="sxs-lookup"><span data-stu-id="fda48-138">`ida:ClientId` with your web app application ID</span></span>
    * <span data-ttu-id="fda48-139">`ida:ClientSecret` tajným klíčem webové aplikace</span><span class="sxs-lookup"><span data-stu-id="fda48-139">`ida:ClientSecret` with your web app secret key</span></span>
    * <span data-ttu-id="fda48-140">`ida:SignUpSignInPolicyId` názvem zásady registrace/přihlášení</span><span class="sxs-lookup"><span data-stu-id="fda48-140">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>
    * <span data-ttu-id="fda48-141">`ida:EditProfilePolicyId` názvem zásady pro úpravu profilu</span><span class="sxs-lookup"><span data-stu-id="fda48-141">`ida:EditProfilePolicyId` with your "Edit Profile" policy name</span></span>
    * <span data-ttu-id="fda48-142">`ida:ResetPasswordPolicyId` názvem zásady pro resetování hesla</span><span class="sxs-lookup"><span data-stu-id="fda48-142">`ida:ResetPasswordPolicyId` with your "Reset Password" policy name</span></span>



## <a name="requesting-and-saving-an-access-token"></a><span data-ttu-id="fda48-143">Požaduje a ukládání token přístupu</span><span class="sxs-lookup"><span data-stu-id="fda48-143">Requesting and saving an access token</span></span>

### <a name="specify-hello-permissions"></a><span data-ttu-id="fda48-144">Zadejte oprávnění hello</span><span class="sxs-lookup"><span data-stu-id="fda48-144">Specify hello permissions</span></span>

<span data-ttu-id="fda48-145">V pořadí toomake hello volání toohello webové rozhraní API, musíte uživatele hello tooauthenticate (pomocí zásad služby registrace-množství nebo přihlášení) a [přijímat přístupový token](active-directory-b2c-access-tokens.md) z Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="fda48-145">In order toomake hello call toohello web API, you need tooauthenticate hello user (using your sign-up/sign-in policy) and [receive an access token](active-directory-b2c-access-tokens.md) from Azure AD B2C.</span></span> <span data-ttu-id="fda48-146">V pořadí tooreceive přístupový token je nejprve nutné zadat hello oprávnění, které byste chtěli toogrant tokenu přístupu hello.</span><span class="sxs-lookup"><span data-stu-id="fda48-146">In order tooreceive an access token, you first must specify hello permissions you would like hello access token toogrant.</span></span> <span data-ttu-id="fda48-147">Hello oprávnění jsou určené v hello `scope` parametr při provádění požadavku toohello hello `/authorize` koncový bod.</span><span class="sxs-lookup"><span data-stu-id="fda48-147">hello permissions are specified in hello `scope` parameter when you make hello request toohello `/authorize` endpoint.</span></span> <span data-ttu-id="fda48-148">Například tooacquire přístupový token s hello "číst" oprávnění toohello prostředků aplikace, která má hello identifikátor ID URI aplikace z `https://contoso.onmicrosoft.com/tasks`, bude hello oboru `https://contoso.onmicrosoft.com/tasks/read`.</span><span class="sxs-lookup"><span data-stu-id="fda48-148">For example, tooacquire an access token with hello “read” permission toohello resource application that has hello App ID URI of `https://contoso.onmicrosoft.com/tasks`, hello scope would be `https://contoso.onmicrosoft.com/tasks/read`.</span></span>

<span data-ttu-id="fda48-149">obor hello toospecify v našem hello ukázku, otevřete soubor `App_Start\Startup.Auth.cs` a definovat hello `Scope` proměnné v OpenIdConnectAuthenticationOptions.</span><span class="sxs-lookup"><span data-stu-id="fda48-149">toospecify hello scope in our sample, open hello file `App_Start\Startup.Auth.cs` and define hello `Scope` variable in OpenIdConnectAuthenticationOptions.</span></span>

```CSharp
// App_Start\Startup.Auth.cs

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {
            ...

            // Specify hello scope by appending all of hello scopes requested into one string (seperated by a blank space)
            Scope = $"{OpenIdConnectScopes.OpenId} {ReadTasksScope} {WriteTasksScope}"
        }
    );
}
```

### <a name="exchange-hello-authorization-code-for-an-access-token"></a><span data-ttu-id="fda48-150">Exchange hello autorizační kód pro token přístupu</span><span class="sxs-lookup"><span data-stu-id="fda48-150">Exchange hello authorization code for an access token</span></span>

<span data-ttu-id="fda48-151">Po dokončení registrace nebo přihlášení prostředí hello uživatelé obdrží aplikace autorizační kód z Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="fda48-151">After an user completes hello sign-up or sign-in experience, your app will receive an authorization code from Azure AD B2C.</span></span> <span data-ttu-id="fda48-152">middleware OWIN OpenID Connect Hello uloží hello kód, ale nebude exchange pro přístupový token.</span><span class="sxs-lookup"><span data-stu-id="fda48-152">hello OWIN OpenID Connect middleware will store hello code, but will not exchange it for an access token.</span></span> <span data-ttu-id="fda48-153">Můžete použít hello [MSAL knihovny](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) toomake hello exchange.</span><span class="sxs-lookup"><span data-stu-id="fda48-153">You can use hello [MSAL library](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) toomake hello exchange.</span></span> <span data-ttu-id="fda48-154">V našem ukázce jsme nakonfigurovali zpětné volání oznámení do hello OpenID Connect middleware vždy, když je obdržena autorizační kód.</span><span class="sxs-lookup"><span data-stu-id="fda48-154">In our sample, we configured a notification callback into hello OpenID Connect middleware whenever an authorization code is received.</span></span> <span data-ttu-id="fda48-155">V hello zpětné volání můžeme použít MSAL tooexchange hello kód pro token a uložte hello token do mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="fda48-155">In hello callback, we use MSAL tooexchange hello code for a token and save hello token into hello cache.</span></span>

```CSharp
/*
* Callback function when an authorization code is received
*/
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
    // Extract hello code from hello response notification
    var code = notification.Code;

    var userObjectId = notification.AuthenticationTicket.Identity.FindFirst(ObjectIdElement).Value;
    var authority = String.Format(AadInstance, Tenant, DefaultPolicy);
    var httpContext = notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase;

    // Exchange hello code for a token. Make sure toospecify hello necessary scopes
    ClientCredential cred = new ClientCredential(ClientSecret);
    ConfidentialClientApplication app = new ConfidentialClientApplication(authority, Startup.ClientId,
                                            RedirectUri, cred, new NaiveSessionCache(userObjectId, httpContext));
    var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { ReadTasksScope, WriteTasksScope }, code, DefaultPolicy);
}
```

## <a name="calling-hello-web-api"></a><span data-ttu-id="fda48-156">Volání webového rozhraní API hello</span><span class="sxs-lookup"><span data-stu-id="fda48-156">Calling hello web API</span></span>

<span data-ttu-id="fda48-157">Tato část popisuje, jak toouse hello token obdržel během registrace-množství nebo přihlášení s Azure AD B2C v pořadí tooaccess hello webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fda48-157">This section discusses how toouse hello token received during sign-up/sign-in with Azure AD B2C in order tooaccess hello web API.</span></span>

### <a name="retrieve-hello-saved-token-in-hello-controllers"></a><span data-ttu-id="fda48-158">Získat token hello uložit v hello řadiče</span><span class="sxs-lookup"><span data-stu-id="fda48-158">Retrieve hello saved token in hello controllers</span></span>

<span data-ttu-id="fda48-159">Hello `TasksController` zodpovídá za komunikaci s hello webového rozhraní API a pro odesílání tooread toohello rozhraní API žádosti HTTP, vytvářet a odstraňovat úlohy.</span><span class="sxs-lookup"><span data-stu-id="fda48-159">hello `TasksController` is responsible for communicating with hello web API and for sending HTTP requests toohello API tooread, create, and delete tasks.</span></span> <span data-ttu-id="fda48-160">Protože hello rozhraní API je zabezpečená službou Azure AD B2C, je třeba toofirst načtení hello tokenu, který jste uložili v hello výše krok.</span><span class="sxs-lookup"><span data-stu-id="fda48-160">Because hello API is secured by Azure AD B2C, you need toofirst retrieve hello token you saved in hello above step.</span></span>

```CSharp
// Controllers\TasksController.cs

/*
* Uses MSAL tooretrieve hello token from hello cache
*/
private async void acquireToken(String[] scope)
{
    string userObjectID = ClaimsPrincipal.Current.FindFirst(Startup.ObjectIdElement).Value;
    string authority = String.Format(Startup.AadInstance, Startup.Tenant, Startup.DefaultPolicy);

    ClientCredential credential = new ClientCredential(Startup.ClientSecret);

    // Retrieve hello token using hello provided scopes
    ConfidentialClientApplication app = new ConfidentialClientApplication(authority, Startup.ClientId,
                                        Startup.RedirectUri, credential,
                                        new NaiveSessionCache(userObjectID, this.HttpContext));
    AuthenticationResult result = await app.AcquireTokenSilentAsync(scope);

    accessToken = result.Token;
}
```

### <a name="read-tasks-from-hello-web-api"></a><span data-ttu-id="fda48-161">Čtení úlohy z hello webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="fda48-161">Read tasks from hello web API</span></span>

<span data-ttu-id="fda48-162">Když máte token, můžete ji připojit toohello HTTP `GET` požadavku v hello `Authorization` záhlaví toosecurely volání `TaskService`:</span><span class="sxs-lookup"><span data-stu-id="fda48-162">When you have a token, you can attach it toohello HTTP `GET` request in hello `Authorization` header toosecurely call `TaskService`:</span></span>

```CSharp
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try {

        // Retrieve hello token with hello specified scopes
        acquireToken(new string[] { Startup.ReadTasksScope });

        HttpClient client = new HttpClient();
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, apiEndpoint);

        // Add token toohello Authorization header and make hello request
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
        HttpResponseMessage response = await client.SendAsync(request);

        // Handle response ...
}

```

### <a name="create-and-delete-tasks-on-hello-web-api"></a><span data-ttu-id="fda48-163">Vytvářet a odstraňovat úlohy na hello webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="fda48-163">Create and delete tasks on hello web API</span></span>

<span data-ttu-id="fda48-164">Postupujte podle hello stejný vzor při odesílání `POST` a `DELETE` požadavky toohello webového rozhraní API, pomocí MSAL tooretrieve hello přístupového tokenu z mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="fda48-164">Follow hello same pattern when you send `POST` and `DELETE` requests toohello web API, using MSAL tooretrieve hello access token from hello cache.</span></span>

## <a name="run-hello-sample-app"></a><span data-ttu-id="fda48-165">Spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="fda48-165">Run hello sample app</span></span>

<span data-ttu-id="fda48-166">Nakonec sestavte a spusťte obě aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="fda48-166">Finally, build and run both hello apps.</span></span> <span data-ttu-id="fda48-167">Registrace a přihlášení a vytvořte úkoly pro hello přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="fda48-167">Sign up and sign in, and create tasks for hello signed-in user.</span></span> <span data-ttu-id="fda48-168">Odhlásit se a přihlaste se jako jiný uživatel.</span><span class="sxs-lookup"><span data-stu-id="fda48-168">Sign out and sign in as a different user.</span></span> <span data-ttu-id="fda48-169">Vytvořte úkoly pro tohoto uživatele.</span><span class="sxs-lookup"><span data-stu-id="fda48-169">Create tasks for that user.</span></span> <span data-ttu-id="fda48-170">Všimněte si, jak hello úloh se ukládají pro každého uživatele na hello rozhraní API, protože hello rozhraní API extrahuje identitu uživatele hello z tokenu hello, které obdrží.</span><span class="sxs-lookup"><span data-stu-id="fda48-170">Notice how hello tasks are stored per-user on hello API, because hello API extracts hello user's identity from hello token it receives.</span></span> <span data-ttu-id="fda48-171">Zkuste taky přehrávání s obory hello.</span><span class="sxs-lookup"><span data-stu-id="fda48-171">Also try playing with hello scopes.</span></span> <span data-ttu-id="fda48-172">Odeberte oprávnění hello příliš "zápisu" a potom zkuste přidat úloha.</span><span class="sxs-lookup"><span data-stu-id="fda48-172">Remove hello permission too"write" and then try adding a task.</span></span> <span data-ttu-id="fda48-173">Ještě se ujistěte, že toosign se při každé změně hello oboru.</span><span class="sxs-lookup"><span data-stu-id="fda48-173">Just make sure toosign out each time you change hello scope.</span></span>

