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
# <a name="azure-ad-b2c-call-a-net-web-api-from-a-net-web-app"></a>Azure AD B2C: Volání webového rozhraní API .NET z webové aplikace .NET

Pomocí Azure AD B2C můžete přidat výkonné identity management funkce tooyour webových aplikací a webových rozhraní API. Tento článek popisuje, jak toorequest přístup tokeny a volání z .NET "seznam úkolů" tooa webové aplikace .NET webové rozhraní api.

Tento článek nepopisuje jak tooimplement přihlášení, registraci a profil správy s Azure AD B2C. Zaměřuje se na volání webových rozhraní API po hello uživatel je již ověřen. Pokud jste to ještě neudělali, měli byste:

* Začínáme s [webové aplikace .NET](active-directory-b2c-devquickstarts-web-dotnet-susi.md)
* Začínáme s [webové rozhraní api .NET](active-directory-b2c-devquickstarts-api-dotnet.md)

## <a name="prerequisite"></a>Požadavek

toobuild webové aplikace, která volá webové rozhraní api, potřebujete:

1. [Vytvoření klienta Azure AD B2C](active-directory-b2c-get-started.md).
2. [Zaregistrovat webového rozhraní api](active-directory-b2c-app-registration.md#register-a-web-api).
3. [Webovou aplikaci zaregistrovat](active-directory-b2c-app-registration.md#register-a-web-app).
4. [Nastavit zásady](active-directory-b2c-reference-policies.md).
5. [Udělení hello webové aplikace oprávnění toouse hello webové rozhraní api](active-directory-b2c-access-tokens.md#publishing-permissions).

> [!IMPORTANT]
> Hello klientská aplikace a webové rozhraní API musí používat adresář hello stejnou službou Azure AD B2C.
>

## <a name="download-hello-code"></a>Stáhněte si kód hello

Hello kódu pro účely tohoto kurzu je udržovaný na [Githubu](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi). Ukázka hello může klonovat spuštěním:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Po stažení ukázkového kódu hello hello otevřete Visual Studio .sln souboru tooget spustit. Hello soubor řešení obsahuje dva projekty: `TaskWebApp` a `TaskService`. `TaskWebApp`je webová aplikace MVC, která hello uživatel komunikuje se službou. `TaskService`je back endové webové rozhraní API hello aplikace, které ukládá seznam úkolů každého uživatele. Tento článek nepopisuje vytváření hello `TaskWebApp` webovou aplikaci nebo hello `TaskService` webové rozhraní api. toolearn způsobu toobuild hello .NET webové aplikace pomocí Azure AD B2C, najdete v našich [kurz .NET pro webové aplikace](active-directory-b2c-devquickstarts-web-dotnet-susi.md). toolearn jak toobuild hello .NET webové rozhraní API zabezpečené pomocí Azure AD B2C, najdete v našich [kurz webové rozhraní API .NET](active-directory-b2c-devquickstarts-api-dotnet.md).

### <a name="update-hello-azure-ad-b2c-configuration"></a>Aktualizace konfigurace hello Azure AD B2C

Naše ukázka je ID zásad a klienta hello nakonfigurované toouse naše ukázka klienta. Pokud chcete toouse vlastního klienta:

1. Otevřete `web.config` v hello `TaskService` projektu a nahraďte hodnoty hello

    * `ida:Tenant` názvem vašeho tenanta
    * `ida:ClientId`pomocí svého ID aplikace webového rozhraní api
    * `ida:SignUpSignInPolicyId` názvem zásady registrace/přihlášení

2. Otevřete `web.config` v hello `TaskWebApp` projektu a nahraďte hodnoty hello

    * `ida:Tenant` názvem vašeho tenanta
    * `ida:ClientId` identifikátorem webového aplikace
    * `ida:ClientSecret` tajným klíčem webové aplikace
    * `ida:SignUpSignInPolicyId` názvem zásady registrace/přihlášení
    * `ida:EditProfilePolicyId` názvem zásady pro úpravu profilu
    * `ida:ResetPasswordPolicyId` názvem zásady pro resetování hesla



## <a name="requesting-and-saving-an-access-token"></a>Požaduje a ukládání token přístupu

### <a name="specify-hello-permissions"></a>Zadejte oprávnění hello

V pořadí toomake hello volání toohello webové rozhraní API, musíte uživatele hello tooauthenticate (pomocí zásad služby registrace-množství nebo přihlášení) a [přijímat přístupový token](active-directory-b2c-access-tokens.md) z Azure AD B2C. V pořadí tooreceive přístupový token je nejprve nutné zadat hello oprávnění, které byste chtěli toogrant tokenu přístupu hello. Hello oprávnění jsou určené v hello `scope` parametr při provádění požadavku toohello hello `/authorize` koncový bod. Například tooacquire přístupový token s hello "číst" oprávnění toohello prostředků aplikace, která má hello identifikátor ID URI aplikace z `https://contoso.onmicrosoft.com/tasks`, bude hello oboru `https://contoso.onmicrosoft.com/tasks/read`.

obor hello toospecify v našem hello ukázku, otevřete soubor `App_Start\Startup.Auth.cs` a definovat hello `Scope` proměnné v OpenIdConnectAuthenticationOptions.

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

### <a name="exchange-hello-authorization-code-for-an-access-token"></a>Exchange hello autorizační kód pro token přístupu

Po dokončení registrace nebo přihlášení prostředí hello uživatelé obdrží aplikace autorizační kód z Azure AD B2C. middleware OWIN OpenID Connect Hello uloží hello kód, ale nebude exchange pro přístupový token. Můžete použít hello [MSAL knihovny](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) toomake hello exchange. V našem ukázce jsme nakonfigurovali zpětné volání oznámení do hello OpenID Connect middleware vždy, když je obdržena autorizační kód. V hello zpětné volání můžeme použít MSAL tooexchange hello kód pro token a uložte hello token do mezipaměti hello.

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

## <a name="calling-hello-web-api"></a>Volání webového rozhraní API hello

Tato část popisuje, jak toouse hello token obdržel během registrace-množství nebo přihlášení s Azure AD B2C v pořadí tooaccess hello webového rozhraní API.

### <a name="retrieve-hello-saved-token-in-hello-controllers"></a>Získat token hello uložit v hello řadiče

Hello `TasksController` zodpovídá za komunikaci s hello webového rozhraní API a pro odesílání tooread toohello rozhraní API žádosti HTTP, vytvářet a odstraňovat úlohy. Protože hello rozhraní API je zabezpečená službou Azure AD B2C, je třeba toofirst načtení hello tokenu, který jste uložili v hello výše krok.

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

### <a name="read-tasks-from-hello-web-api"></a>Čtení úlohy z hello webového rozhraní API

Když máte token, můžete ji připojit toohello HTTP `GET` požadavku v hello `Authorization` záhlaví toosecurely volání `TaskService`:

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

### <a name="create-and-delete-tasks-on-hello-web-api"></a>Vytvářet a odstraňovat úlohy na hello webového rozhraní API

Postupujte podle hello stejný vzor při odesílání `POST` a `DELETE` požadavky toohello webového rozhraní API, pomocí MSAL tooretrieve hello přístupového tokenu z mezipaměti hello.

## <a name="run-hello-sample-app"></a>Spuštění ukázkové aplikace hello

Nakonec sestavte a spusťte obě aplikace hello. Registrace a přihlášení a vytvořte úkoly pro hello přihlášeného uživatele. Odhlásit se a přihlaste se jako jiný uživatel. Vytvořte úkoly pro tohoto uživatele. Všimněte si, jak hello úloh se ukládají pro každého uživatele na hello rozhraní API, protože hello rozhraní API extrahuje identitu uživatele hello z tokenu hello, které obdrží. Zkuste taky přehrávání s obory hello. Odeberte oprávnění hello příliš "zápisu" a potom zkuste přidat úloha. Ještě se ujistěte, že toosign se při každé změně hello oboru.

