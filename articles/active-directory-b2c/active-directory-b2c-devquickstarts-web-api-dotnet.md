---
title: "Volání zabezpečeném webové rozhraní api ASP.NET Azure Active Directory B2C | Microsoft Docs"
description: "Sestavení .NET webové aplikace a volání webového rozhraní api pomocí přístupových tokenů Azure Active Directory B2C a OAuth 2.0."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: mtillman
editor: 
ms.assetid: d3888556-2647-4a42-b068-027f9374aa61
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/17/2017
ms.author: parakhj
ms.custom: seohack1
ms.openlocfilehash: d81976988a26ce264dd7b9ed24f43aed21d4ee99
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="azure-ad-b2c-call-a-net-web-api-from-a-net-web-app"></a><span data-ttu-id="d6969-103">Azure AD B2C: Volání webového rozhraní API .NET z webové aplikace .NET</span><span class="sxs-lookup"><span data-stu-id="d6969-103">Azure AD B2C: Call a .NET web API from a .NET web app</span></span>

<span data-ttu-id="d6969-104">Pomocí Azure AD B2C můžete přidat funkce správy identit výkonné webové aplikace a webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d6969-104">By using Azure AD B2C, you can add powerful identity management features to your web apps and web APIs.</span></span> <span data-ttu-id="d6969-105">Tento článek popisuje, jak požádat o přístupové tokeny a zkontrolujte volání z webové aplikace .NET "seznam úkolů".NET webové rozhraní api.</span><span class="sxs-lookup"><span data-stu-id="d6969-105">This article discusses how to request access tokens and make calls from a .NET "to-do list" web app to a .NET web api.</span></span>

<span data-ttu-id="d6969-106">Tento článek nepopisuje, jak implementovat přihlášení, registrace a správy profilů pomocí Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="d6969-106">This article does not cover how to implement sign-in, sign-up and profile management with Azure AD B2C.</span></span> <span data-ttu-id="d6969-107">Zaměřuje se na volání webových rozhraní API po již byl uživatel ověřen.</span><span class="sxs-lookup"><span data-stu-id="d6969-107">It focuses on calling web APIs after the user is already authenticated.</span></span> <span data-ttu-id="d6969-108">Pokud jste to ještě neudělali, měli byste:</span><span class="sxs-lookup"><span data-stu-id="d6969-108">If you haven't already, you should:</span></span>

* <span data-ttu-id="d6969-109">Začínáme s [webové aplikace .NET](active-directory-b2c-devquickstarts-web-dotnet-susi.md)</span><span class="sxs-lookup"><span data-stu-id="d6969-109">Get started with a [.NET web app](active-directory-b2c-devquickstarts-web-dotnet-susi.md)</span></span>
* <span data-ttu-id="d6969-110">Začínáme s [webové rozhraní api .NET](active-directory-b2c-devquickstarts-api-dotnet.md)</span><span class="sxs-lookup"><span data-stu-id="d6969-110">Get started with a [.NET web api](active-directory-b2c-devquickstarts-api-dotnet.md)</span></span>

## <a name="prerequisite"></a><span data-ttu-id="d6969-111">Požadavek</span><span class="sxs-lookup"><span data-stu-id="d6969-111">Prerequisite</span></span>

<span data-ttu-id="d6969-112">K vytvoření webové aplikace, která volá webové rozhraní api, potřebujete:</span><span class="sxs-lookup"><span data-stu-id="d6969-112">To build a web application that calls a web api, you need to:</span></span>

1. <span data-ttu-id="d6969-113">[Vytvoření klienta Azure AD B2C](active-directory-b2c-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="d6969-113">[Create an Azure AD B2C tenant](active-directory-b2c-get-started.md).</span></span>
2. <span data-ttu-id="d6969-114">[Zaregistrovat webového rozhraní api](active-directory-b2c-app-registration.md#register-a-web-api).</span><span class="sxs-lookup"><span data-stu-id="d6969-114">[Register a web api](active-directory-b2c-app-registration.md#register-a-web-api).</span></span>
3. <span data-ttu-id="d6969-115">[Webovou aplikaci zaregistrovat](active-directory-b2c-app-registration.md#register-a-web-app).</span><span class="sxs-lookup"><span data-stu-id="d6969-115">[Register a web app](active-directory-b2c-app-registration.md#register-a-web-app).</span></span>
4. <span data-ttu-id="d6969-116">[Nastavit zásady](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="d6969-116">[Set up policies](active-directory-b2c-reference-policies.md).</span></span>
5. <span data-ttu-id="d6969-117">[Udělení oprávnění aplikace prostřednictvím webu webového rozhraní api](active-directory-b2c-access-tokens.md#publishing-permissions).</span><span class="sxs-lookup"><span data-stu-id="d6969-117">[Grant the web app permissions to use the web api](active-directory-b2c-access-tokens.md#publishing-permissions).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d6969-118">Klientská aplikace a webové rozhraní API musí používat stejný adresář Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="d6969-118">The client application and web API must use the same Azure AD B2C directory.</span></span>
>

## <a name="download-the-code"></a><span data-ttu-id="d6969-119">Stáhněte si kód</span><span class="sxs-lookup"><span data-stu-id="d6969-119">Download the code</span></span>

<span data-ttu-id="d6969-120">Kód k tomuto kurzu se udržuje na [GitHubu](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span><span class="sxs-lookup"><span data-stu-id="d6969-120">The code for this tutorial is maintained on [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span></span> <span data-ttu-id="d6969-121">Ukázku můžete klonovat spuštěním:</span><span class="sxs-lookup"><span data-stu-id="d6969-121">You can clone the sample by running:</span></span>

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

<span data-ttu-id="d6969-122">Po stažení ukázkového kódu otevřete soubor Visual Studio .sln, abyste mohli začít.</span><span class="sxs-lookup"><span data-stu-id="d6969-122">After you download the sample code, open the Visual Studio .sln file to get started.</span></span> <span data-ttu-id="d6969-123">Soubor řešení obsahuje dva projekty: `TaskWebApp` a `TaskService`.</span><span class="sxs-lookup"><span data-stu-id="d6969-123">The solution file contains two projects: `TaskWebApp` and `TaskService`.</span></span> <span data-ttu-id="d6969-124">`TaskWebApp` je webová aplikace MVC, který uživatel komunikuje.</span><span class="sxs-lookup"><span data-stu-id="d6969-124">`TaskWebApp` is a MVC web application that the user interacts with.</span></span> <span data-ttu-id="d6969-125">`TaskService` je back-endové webové rozhraní API aplikace, které ukládá seznam úkolů každého uživatele.</span><span class="sxs-lookup"><span data-stu-id="d6969-125">`TaskService` is the app's back-end web API that stores each user's to-do list.</span></span> <span data-ttu-id="d6969-126">Tento článek nepopisuje sestavování `TaskWebApp` webovou aplikaci nebo `TaskService` webové rozhraní api.</span><span class="sxs-lookup"><span data-stu-id="d6969-126">This article does not cover building the `TaskWebApp` web app or the `TaskService` web api.</span></span> <span data-ttu-id="d6969-127">Naučte se vytvářet webové aplikace .NET pomocí Azure AD B2C, najdete v tématu naše [kurz .NET pro webové aplikace](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span><span class="sxs-lookup"><span data-stu-id="d6969-127">To learn how to build the .NET web app using Azure AD B2C, see our [.NET web app tutorial](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span></span> <span data-ttu-id="d6969-128">Naučte se vytvářet rozhraní .NET webové rozhraní API zabezpečené pomocí Azure AD B2C, najdete v tématu naše [kurz webové rozhraní API .NET](active-directory-b2c-devquickstarts-api-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="d6969-128">To learn how to build the .NET web API secured using Azure AD B2C, see our [.NET web API tutorial](active-directory-b2c-devquickstarts-api-dotnet.md).</span></span>

### <a name="update-the-azure-ad-b2c-configuration"></a><span data-ttu-id="d6969-129">Aktualizace konfigurace Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="d6969-129">Update the Azure AD B2C configuration</span></span>

<span data-ttu-id="d6969-130">Naše ukázka je nakonfigurovaná k použití zásad a ID klienta naše ukázkového tenanta.</span><span class="sxs-lookup"><span data-stu-id="d6969-130">Our sample is configured to use the policies and client ID of our demo tenant.</span></span> <span data-ttu-id="d6969-131">Pokud chcete použít vlastního klienta:</span><span class="sxs-lookup"><span data-stu-id="d6969-131">If you would like to use your own tenant:</span></span>

1. <span data-ttu-id="d6969-132">Otevřete `web.config` v projektu `TaskService` a nahraďte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="d6969-132">Open `web.config` in the `TaskService` project and replace the values for</span></span>

    * <span data-ttu-id="d6969-133">`ida:Tenant` názvem vašeho tenanta</span><span class="sxs-lookup"><span data-stu-id="d6969-133">`ida:Tenant` with your tenant name</span></span>
    * <span data-ttu-id="d6969-134">`ida:ClientId` pomocí svého ID aplikace webového rozhraní api</span><span class="sxs-lookup"><span data-stu-id="d6969-134">`ida:ClientId` with your web api application ID</span></span>
    * <span data-ttu-id="d6969-135">`ida:SignUpSignInPolicyId` názvem zásady registrace/přihlášení</span><span class="sxs-lookup"><span data-stu-id="d6969-135">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>

2. <span data-ttu-id="d6969-136">Otevřete `web.config` v projektu `TaskWebApp` a nahraďte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="d6969-136">Open `web.config` in the `TaskWebApp` project and replace the values for</span></span>

    * <span data-ttu-id="d6969-137">`ida:Tenant` názvem vašeho tenanta</span><span class="sxs-lookup"><span data-stu-id="d6969-137">`ida:Tenant` with your tenant name</span></span>
    * <span data-ttu-id="d6969-138">`ida:ClientId` identifikátorem webového aplikace</span><span class="sxs-lookup"><span data-stu-id="d6969-138">`ida:ClientId` with your web app application ID</span></span>
    * <span data-ttu-id="d6969-139">`ida:ClientSecret` tajným klíčem webové aplikace</span><span class="sxs-lookup"><span data-stu-id="d6969-139">`ida:ClientSecret` with your web app secret key</span></span>
    * <span data-ttu-id="d6969-140">`ida:SignUpSignInPolicyId` názvem zásady registrace/přihlášení</span><span class="sxs-lookup"><span data-stu-id="d6969-140">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>
    * <span data-ttu-id="d6969-141">`ida:EditProfilePolicyId` názvem zásady pro úpravu profilu</span><span class="sxs-lookup"><span data-stu-id="d6969-141">`ida:EditProfilePolicyId` with your "Edit Profile" policy name</span></span>
    * <span data-ttu-id="d6969-142">`ida:ResetPasswordPolicyId` názvem zásady pro resetování hesla</span><span class="sxs-lookup"><span data-stu-id="d6969-142">`ida:ResetPasswordPolicyId` with your "Reset Password" policy name</span></span>



## <a name="requesting-and-saving-an-access-token"></a><span data-ttu-id="d6969-143">Požaduje a ukládání token přístupu</span><span class="sxs-lookup"><span data-stu-id="d6969-143">Requesting and saving an access token</span></span>

### <a name="specify-the-permissions"></a><span data-ttu-id="d6969-144">Zadejte oprávnění</span><span class="sxs-lookup"><span data-stu-id="d6969-144">Specify the permissions</span></span>

<span data-ttu-id="d6969-145">Aby bylo možné volání webového rozhraní API, budete muset ověřit uživatele (pomocí zásad služby registrace-množství nebo přihlášení) a [přijímat přístupový token](active-directory-b2c-access-tokens.md) z Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="d6969-145">In order to make the call to the web API, you need to authenticate the user (using your sign-up/sign-in policy) and [receive an access token](active-directory-b2c-access-tokens.md) from Azure AD B2C.</span></span> <span data-ttu-id="d6969-146">Chcete-li získat přístupový token, nejprve musíte zadat chcete přístupový token k udělení oprávnění.</span><span class="sxs-lookup"><span data-stu-id="d6969-146">In order to receive an access token, you first must specify the permissions you would like the access token to grant.</span></span> <span data-ttu-id="d6969-147">Oprávnění jsou určené v `scope` parametr při provádění požadavku `/authorize` koncový bod.</span><span class="sxs-lookup"><span data-stu-id="d6969-147">The permissions are specified in the `scope` parameter when you make the request to the `/authorize` endpoint.</span></span> <span data-ttu-id="d6969-148">Chcete-li například získat přístupový token s oprávnění "číst" prostředků aplikace, kterou je identifikátor ID URI aplikace z `https://contoso.onmicrosoft.com/tasks`, oboru by `https://contoso.onmicrosoft.com/tasks/read`.</span><span class="sxs-lookup"><span data-stu-id="d6969-148">For example, to acquire an access token with the “read” permission to the resource application that has the App ID URI of `https://contoso.onmicrosoft.com/tasks`, the scope would be `https://contoso.onmicrosoft.com/tasks/read`.</span></span>

<span data-ttu-id="d6969-149">K určení oboru ve výběru, otevřete soubor `App_Start\Startup.Auth.cs` a definovat `Scope` proměnné v OpenIdConnectAuthenticationOptions.</span><span class="sxs-lookup"><span data-stu-id="d6969-149">To specify the scope in our sample, open the file `App_Start\Startup.Auth.cs` and define the `Scope` variable in OpenIdConnectAuthenticationOptions.</span></span>

```CSharp
// App_Start\Startup.Auth.cs

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {
            ...

            // Specify the scope by appending all of the scopes requested into one string (seperated by a blank space)
            Scope = $"{OpenIdConnectScopes.OpenId} {ReadTasksScope} {WriteTasksScope}"
        }
    );
}
```

### <a name="exchange-the-authorization-code-for-an-access-token"></a><span data-ttu-id="d6969-150">Exchange autorizační kód pro token přístupu</span><span class="sxs-lookup"><span data-stu-id="d6969-150">Exchange the authorization code for an access token</span></span>

<span data-ttu-id="d6969-151">Po dokončení registrace nebo přihlášení zkušeností uživatele s vaší aplikace obdrží autorizační kód z Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="d6969-151">After an user completes the sign-up or sign-in experience, your app will receive an authorization code from Azure AD B2C.</span></span> <span data-ttu-id="d6969-152">Middleware OWIN OpenID Connect uloží kód, ale nebude exchange pro přístupový token.</span><span class="sxs-lookup"><span data-stu-id="d6969-152">The OWIN OpenID Connect middleware will store the code, but will not exchange it for an access token.</span></span> <span data-ttu-id="d6969-153">Můžete použít [MSAL knihovny](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) aby exchange.</span><span class="sxs-lookup"><span data-stu-id="d6969-153">You can use the [MSAL library](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) to make the exchange.</span></span> <span data-ttu-id="d6969-154">V našem ukázce jsme nakonfigurovali zpětné volání oznámení do middleware OpenID Connect vždy, když je obdržena autorizační kód.</span><span class="sxs-lookup"><span data-stu-id="d6969-154">In our sample, we configured a notification callback into the OpenID Connect middleware whenever an authorization code is received.</span></span> <span data-ttu-id="d6969-155">Zpětné volání používáme MSAL exchange kód pro token a uložení tokenu do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="d6969-155">In the callback, we use MSAL to exchange the code for a token and save the token into the cache.</span></span>

```CSharp
/*
* Callback function when an authorization code is received
*/
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
    // Extract the code from the response notification
    var code = notification.Code;

    var userObjectId = notification.AuthenticationTicket.Identity.FindFirst(ObjectIdElement).Value;
    var authority = String.Format(AadInstance, Tenant, DefaultPolicy);
    var httpContext = notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase;

    // Exchange the code for a token. Make sure to specify the necessary scopes
    ClientCredential cred = new ClientCredential(ClientSecret);
    ConfidentialClientApplication app = new ConfidentialClientApplication(authority, Startup.ClientId,
                                            RedirectUri, cred, new NaiveSessionCache(userObjectId, httpContext));
    var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { ReadTasksScope, WriteTasksScope }, code, DefaultPolicy);
}
```

## <a name="calling-the-web-api"></a><span data-ttu-id="d6969-156">Volání webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="d6969-156">Calling the web API</span></span>

<span data-ttu-id="d6969-157">Tato část popisuje postup použití tokenu obdržel během registrace-množství nebo přihlášení s Azure AD B2C, aby měli přístup na webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d6969-157">This section discusses how to use the token received during sign-up/sign-in with Azure AD B2C in order to access the web API.</span></span>

### <a name="retrieve-the-saved-token-in-the-controllers"></a><span data-ttu-id="d6969-158">Načtení uložené token v na řadiče</span><span class="sxs-lookup"><span data-stu-id="d6969-158">Retrieve the saved token in the controllers</span></span>

<span data-ttu-id="d6969-159">`TasksController` Je zodpovědný za komunikaci s webové rozhraní API a pro odesílání požadavků HTTP do rozhraní API číst, vytvářet a odstraňovat úlohy.</span><span class="sxs-lookup"><span data-stu-id="d6969-159">The `TasksController` is responsible for communicating with the web API and for sending HTTP requests to the API to read, create, and delete tasks.</span></span> <span data-ttu-id="d6969-160">Protože rozhraní API je zabezpečená službou Azure AD B2C, musíte nejdřív načíst token, který jste uložili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="d6969-160">Because the API is secured by Azure AD B2C, you need to first retrieve the token you saved in the above step.</span></span>

```CSharp
// Controllers\TasksController.cs

/*
* Uses MSAL to retrieve the token from the cache
*/
private async void acquireToken(String[] scope)
{
    string userObjectID = ClaimsPrincipal.Current.FindFirst(Startup.ObjectIdElement).Value;
    string authority = String.Format(Startup.AadInstance, Startup.Tenant, Startup.DefaultPolicy);

    ClientCredential credential = new ClientCredential(Startup.ClientSecret);

    // Retrieve the token using the provided scopes
    ConfidentialClientApplication app = new ConfidentialClientApplication(authority, Startup.ClientId,
                                        Startup.RedirectUri, credential,
                                        new NaiveSessionCache(userObjectID, this.HttpContext));
    AuthenticationResult result = await app.AcquireTokenSilentAsync(scope);

    accessToken = result.Token;
}
```

### <a name="read-tasks-from-the-web-api"></a><span data-ttu-id="d6969-161">Čtení úlohy z webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="d6969-161">Read tasks from the web API</span></span>

<span data-ttu-id="d6969-162">Když máte token, abyste ho připojili k HTTP `GET` v požadavku `Authorization` záhlaví bezpečně volat `TaskService`:</span><span class="sxs-lookup"><span data-stu-id="d6969-162">When you have a token, you can attach it to the HTTP `GET` request in the `Authorization` header to securely call `TaskService`:</span></span>

```CSharp
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try {

        // Retrieve the token with the specified scopes
        acquireToken(new string[] { Startup.ReadTasksScope });

        HttpClient client = new HttpClient();
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, apiEndpoint);

        // Add token to the Authorization header and make the request
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
        HttpResponseMessage response = await client.SendAsync(request);

        // Handle response ...
}

```

### <a name="create-and-delete-tasks-on-the-web-api"></a><span data-ttu-id="d6969-163">Vytvářet a odstraňovat úlohy na webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="d6969-163">Create and delete tasks on the web API</span></span>

<span data-ttu-id="d6969-164">Postupujte podle stejného vzoru při odesílání `POST` a `DELETE` požadavky na webové rozhraní API, pomocí MSAL k získání přístupového tokenu z mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="d6969-164">Follow the same pattern when you send `POST` and `DELETE` requests to the web API, using MSAL to retrieve the access token from the cache.</span></span>

## <a name="run-the-sample-app"></a><span data-ttu-id="d6969-165">Spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="d6969-165">Run the sample app</span></span>

<span data-ttu-id="d6969-166">Nakonec sestavte a spusťte obě aplikace.</span><span class="sxs-lookup"><span data-stu-id="d6969-166">Finally, build and run both the apps.</span></span> <span data-ttu-id="d6969-167">Registrace a přihlášení a vytvořte úkoly pro přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="d6969-167">Sign up and sign in, and create tasks for the signed-in user.</span></span> <span data-ttu-id="d6969-168">Odhlásit se a přihlaste se jako jiný uživatel.</span><span class="sxs-lookup"><span data-stu-id="d6969-168">Sign out and sign in as a different user.</span></span> <span data-ttu-id="d6969-169">Vytvořte úkoly pro tohoto uživatele.</span><span class="sxs-lookup"><span data-stu-id="d6969-169">Create tasks for that user.</span></span> <span data-ttu-id="d6969-170">Všimněte si, jak jsou úlohy ukládají pro každého uživatele na volání rozhraní API, protože rozhraní API extrahuje identitu uživatele z tokenu obdrží.</span><span class="sxs-lookup"><span data-stu-id="d6969-170">Notice how the tasks are stored per-user on the API, because the API extracts the user's identity from the token it receives.</span></span> <span data-ttu-id="d6969-171">Zkuste taky přehrávání s obory.</span><span class="sxs-lookup"><span data-stu-id="d6969-171">Also try playing with the scopes.</span></span> <span data-ttu-id="d6969-172">Odeberte oprávnění k "zápisu" a potom zkuste přidat úloha.</span><span class="sxs-lookup"><span data-stu-id="d6969-172">Remove the permission to "write" and then try adding a task.</span></span> <span data-ttu-id="d6969-173">Ujistěte se odhlásit se při každé změně oboru.</span><span class="sxs-lookup"><span data-stu-id="d6969-173">Just make sure to sign out each time you change the scope.</span></span>

