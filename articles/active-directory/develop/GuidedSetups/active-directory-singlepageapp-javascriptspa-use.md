---
title: "aaaAzure AD v2 JS SPA na základě nastavení – použití | Microsoft Docs"
description: "Jak může aplikace JavaScript SPA volat rozhraní API, které vyžadují přístupové tokeny bodem v2 Azure Active Directory"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/01/2017
ms.author: andret
ms.openlocfilehash: 4f7f824ed787d998dc4aea3dc21c95d7dfe70ae0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="use-hello-microsoft-authentication-library-msal-toosign-in-hello-user"></a><span data-ttu-id="91a9e-103">Použití hello Microsoft ověřování knihovny (MSAL) toosign v hello uživatele</span><span class="sxs-lookup"><span data-stu-id="91a9e-103">Use hello Microsoft Authentication Library (MSAL) toosign-in hello user</span></span>

1.  <span data-ttu-id="91a9e-104">Vytvořte soubor s názvem `app.js`.</span><span class="sxs-lookup"><span data-stu-id="91a9e-104">Create a file named `app.js`.</span></span> <span data-ttu-id="91a9e-105">Pokud používáte Visual Studio, vyberte hello projekt (projektu do kořenové složky), klikněte pravým tlačítkem a vyberte: `Add`  >  `New Item`  >  `JavaScript File`:</span><span class="sxs-lookup"><span data-stu-id="91a9e-105">If you are using Visual Studio, select hello project (project root folder), right click and select: `Add` > `New Item` > `JavaScript File`:</span></span>
2.  <span data-ttu-id="91a9e-106">Přidejte následující kód tooyour hello `app.js` souboru:</span><span class="sxs-lookup"><span data-stu-id="91a9e-106">Add hello following code tooyour `app.js` file:</span></span>

```javascript
// Graph API endpoint tooshow user profile
var graphApiEndpoint = "https://graph.microsoft.com/v1.0/me";

// Graph API scope used tooobtain hello access token tooread user profile
var graphAPIScopes = ["https://graph.microsoft.com/user.read"];

// Initialize application
var userAgentApplication = new Msal.UserAgentApplication(msalconfig.clientID, null, loginCallback, {
    redirectUri: msalconfig.redirectUri
});

//Previous version of msal uses redirect url via a property
if (userAgentApplication.redirectUri) {
    userAgentApplication.redirectUri = msalconfig.redirectUri;
}

window.onload = function () {
    // If page is refreshed, continue toodisplay user info
    if (!userAgentApplication.isCallback(window.location.hash) && window.parent === window && !window.opener) {
        var user = userAgentApplication.getUser();
        if (user) {
            callGraphApi();
        }
    }
}

/**
 * Call hello Microsoft Graph API and display hello results on hello page. Sign hello user in if necessary
 */
function callGraphApi() {
    var user = userAgentApplication.getUser();
    if (!user) {
        // If user is not signed in, then prompt user toosign in via loginRedirect.
        // This will redirect user toohello Azure Active Directory v2 Endpoint
        userAgentApplication.loginRedirect(graphAPIScopes);
        // hello call toologinRedirect above frontloads hello consent tooquery Graph API during hello sign-in.
        // If you want toouse dynamic consent, just remove hello graphAPIScopes from loginRedirect call.
        // As such, user will be prompted toogive consent when requested access tooa resource that 
        // he/she hasn't consented before. In hello case of this application - 
        // hello first time hello Graph API call tooobtain user's profile is executed.
    } else {
        // If user is already signed in, display hello user info
        var userInfoElement = document.getElementById("userInfo");
        userInfoElement.parentElement.classList.remove("hidden");
        userInfoElement.innerHTML = JSON.stringify(user, null, 4);

        // Show Sign-Out button
        document.getElementById("signOutButton").classList.remove("hidden");

        // Now Call Graph API tooshow hello user profile information:
        var graphCallResponseElement = document.getElementById("graphResponse");
        graphCallResponseElement.parentElement.classList.remove("hidden");
        graphCallResponseElement.innerText = "Calling Graph ...";

        // In order toocall hello Graph API, an access token needs toobe acquired.
        // Try tooacquire hello token used tooquery Graph API silently first:
        userAgentApplication.acquireTokenSilent(graphAPIScopes)
            .then(function (token) {
                //After hello access token is acquired, call hello Web API, sending hello acquired token
                callWebApiWithToken(graphApiEndpoint, token, graphCallResponseElement, document.getElementById("accessToken"));

            }, function (error) {
                // If hello acquireTokenSilent() method fails, then acquire hello token interactively via acquireTokenRedirect().
                // In this case, hello browser will redirect user back toohello Azure Active Directory v2 Endpoint so hello user 
                // can reenter hello current username/ password and/ or give consent toonew permissions your application is requesting.
                // After authentication/ authorization completes, this page will be reloaded again and callGraphApi() will be executed on page load.
                // Then, acquireTokenSilent will then get hello token silently, hello Graph API call results will be made and results will be displayed in hello page.
                if (error) {
                    userAgentApplication.acquireTokenRedirect(graphAPIScopes);
                }
            });

    }
}

/**
 * Callback method from sign-in: if no errors, call callGraphApi() tooshow results.
 * @param {string} errorDesc - If error occur, hello error message
 * @param {object} token - hello token received from login
 * @param {object} error - hello error string
 * @param {string} tokenType - hello token type: usually id_token
 */
function loginCallback(errorDesc, token, error, tokenType) {
    if (errorDesc) {
        showError(msal.authority, error, errorDesc);
    } else {
        callGraphApi();
    }
}

/**
 * Show an error message in hello page
 * @param {string} endpoint - hello endpoint used for hello error message
 * @param {string} error - Error string
 * @param {string} errorDesc - Error description
 */
function showError(endpoint, error, errorDesc) {
    var formattedError = JSON.stringify(error, null, 4);
    if (formattedError.length < 3) {
        formattedError = error;
    }
    document.getElementById("errorMessage").innerHTML = "An error has occurred:<br/>Endpoint: " + endpoint + "<br/>Error: " + formattedError + "<br/>" + errorDesc;
    console.error(error);
}

```

<!--start-collapse-->
### <a name="more-information"></a><span data-ttu-id="91a9e-107">Další informace</span><span class="sxs-lookup"><span data-stu-id="91a9e-107">More Information</span></span>

<span data-ttu-id="91a9e-108">Po kliknutí hello *'volání Microsoft Graph API,* tlačítko hello poprvé, `callGraphApi` volání metod `loginRedirect` toosign v hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="91a9e-108">After a user clicks hello *‘Call Microsoft Graph API’* button for hello first time, `callGraphApi` method calls `loginRedirect` toosign in hello user.</span></span> <span data-ttu-id="91a9e-109">Výsledkem této metody přesměrování hello uživatele toohello *koncového bodu Microsoft Azure Active Directory v2* tooprompt a ověření přihlašovacích údajů uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="91a9e-109">This method results in redirecting hello user toohello *Microsoft Azure Active Directory v2 endpoint* tooprompt and validate hello user's credentials.</span></span> <span data-ttu-id="91a9e-110">V důsledku úspěšného přihlášení, je uživatel hello přesměrovaného back toohello původní *index.html* stránky a token byl přijat zpracovává `msal.js` a hello informací obsažených v tokenu hello se uloží do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="91a9e-110">As a result of a successful sign-in, hello user is redirected back toohello original *index.html* page, and a token is received, processed by `msal.js` and hello information contained in hello token is cached.</span></span> <span data-ttu-id="91a9e-111">Tento token se označuje jako hello *ID token* a obsahuje základní informace o hello uživateli, jako je například hello zobrazované uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="91a9e-111">This token is known as hello *ID token* and contains basic information about hello user, such as hello user display name.</span></span> <span data-ttu-id="91a9e-112">Pokud máte v plánu toouse žádná data zadaný pomocí tohoto tokenu k jakýmkoli jiným účelům, je nutné toomake se, že tento token je ověřen tooguarantee váš back-end serveru, který hello token byl vydán tooa platného uživatele pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="91a9e-112">If you plan toouse any data provided by this token for any purposes, you need toomake sure this token is validated by your backend server tooguarantee that hello token was issued tooa valid user for your application.</span></span>

<span data-ttu-id="91a9e-113">Hello SPA generované tímto průvodcem neprovede používat přímo z tokenu ID hello – místo toho volá `acquireTokenSilent` nebo `acquireTokenRedirect` tooacquire *přístupový token* používá tooquery hello Microsoft Graph API.</span><span class="sxs-lookup"><span data-stu-id="91a9e-113">hello SPA generated by this guide does not make use directly of hello ID token – instead, it calls `acquireTokenSilent` and/or `acquireTokenRedirect` tooacquire an *access token* used tooquery hello Microsoft Graph API.</span></span> <span data-ttu-id="91a9e-114">Pokud potřebujete vzorku, který ověří hello ID token, podívejte se na [to](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "Githubu active-directory-javascript-singlepageapp-dotnet-webapi-v2 ukázka") používá ukázkovou aplikaci v Githubu – ukázka hello pro ASP.NET Web API pro ověření tokenu.</span><span class="sxs-lookup"><span data-stu-id="91a9e-114">If you need a sample that validates hello ID token, take a look at [this](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "Github active-directory-javascript-singlepageapp-dotnet-webapi-v2 sample") sample application in GitHub – hello sample uses an ASP.NET Web API for token validation.</span></span>

#### <a name="getting-a-user-token-interactively"></a><span data-ttu-id="91a9e-115">Získání tokenu uživatele interaktivně</span><span class="sxs-lookup"><span data-stu-id="91a9e-115">Getting a user token interactively</span></span>

<span data-ttu-id="91a9e-116">Po hello počáteční přihlášení, které nechcete hello požádejte uživatele tooreauthenticate pokaždé, když potřebují toorequest tokenu tooaccess – prostředek tak *acquireTokenSilent* by měla být používá většina hello čas tooacquire tokenů.</span><span class="sxs-lookup"><span data-stu-id="91a9e-116">After hello initial sign-in, you do not want hello ask users tooreauthenticate every time they need toorequest a token tooaccess a resource – so *acquireTokenSilent* should be used most of hello time tooacquire tokens.</span></span> <span data-ttu-id="91a9e-117">Existují však situace, je nutné, aby uživatelům interakci s koncovým bodem v2 Azure Active Directory – tooforce mezi příklady patří:</span><span class="sxs-lookup"><span data-stu-id="91a9e-117">There are situations however that you need tooforce users interact with Azure Active Directory v2 endpoint – some examples include:</span></span>
-   <span data-ttu-id="91a9e-118">Uživatelé mohou potřebovat tooreenter přihlašovacích vzhledem k tomu, že vypršela platnost hesla hello</span><span class="sxs-lookup"><span data-stu-id="91a9e-118">Users may need tooreenter their credentials because hello password has expired</span></span>
-   <span data-ttu-id="91a9e-119">Vaše aplikace požaduje přístup tooa prostředek, který hello tooconsent potřebám uživatele pro</span><span class="sxs-lookup"><span data-stu-id="91a9e-119">Your application is requesting access tooa resource that hello user needs tooconsent to</span></span>
-   <span data-ttu-id="91a9e-120">Není třeba dvoufaktorové ověřování</span><span class="sxs-lookup"><span data-stu-id="91a9e-120">Two factor authentication is required</span></span>

<span data-ttu-id="91a9e-121">Volání hello *acquireTokenRedirect(scope)* mít za následek přesměrování koncový bod uživatelé toohello v2 Azure Active Directory (nebo *acquireTokenPopup(scope)* výsledek v automaticky otevíraném okně) kdy musí uživatelé toointeract s tak, že buď zkontrolujete přihlašovacích údajů, udělení souhlasu toohello hello vyžaduje prostředek nebo dokončuje hello dvoufaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="91a9e-121">Calling hello *acquireTokenRedirect(scope)* result in redirecting users toohello Azure Active Directory v2 endpoint (or *acquireTokenPopup(scope)* result on a popup window) where users need toointeract with by either confirming their credentials, giving hello consent toohello required resource, or completing hello two factor authentication.</span></span>

#### <a name="getting-a-user-token-silently"></a><span data-ttu-id="91a9e-122">Získání tokenu uživatele bez upozornění</span><span class="sxs-lookup"><span data-stu-id="91a9e-122">Getting a user token silently</span></span>
<span data-ttu-id="91a9e-123">Hello ` acquireTokenSilent` metoda zpracovává tokenu pořízení a obnovení bez nutnosti zásahu uživatele.</span><span class="sxs-lookup"><span data-stu-id="91a9e-123">hello ` acquireTokenSilent` method handles token acquisitions and renewal without any user interaction.</span></span> <span data-ttu-id="91a9e-124">Po `loginRedirect` (nebo `loginPopup`) je spouštěna hello poprvé, `acquireTokenSilent` je hello metoda běžně používané tooobtain použití tokenů tooaccess chráněné zdroje pro následující volání - jako toorequest volání nebo tokeny obnovení jsou vytvářeny bezobslužně.</span><span class="sxs-lookup"><span data-stu-id="91a9e-124">After `loginRedirect` (or `loginPopup`) is executed for hello first time, `acquireTokenSilent` is hello method commonly used tooobtain tokens used tooaccess protected resources for subsequent calls - as calls toorequest or renew tokens are made silently.</span></span>
<span data-ttu-id="91a9e-125">`acquireTokenSilent`může selhat v některých případech – například heslo uživatele hello vypršela platnost.</span><span class="sxs-lookup"><span data-stu-id="91a9e-125">`acquireTokenSilent` may fail in some cases – for example, hello user's password has expired.</span></span> <span data-ttu-id="91a9e-126">Aplikace může zpracovat výjimku dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="91a9e-126">Your application can handle this exception in two ways:</span></span>

1.  <span data-ttu-id="91a9e-127">Volání jsou příliš`acquireTokenRedirect` okamžitě, výsledkem je zobrazení výzvy hello toosign uživatele v.</span><span class="sxs-lookup"><span data-stu-id="91a9e-127">Make a call too`acquireTokenRedirect` immediately, which results in prompting hello user toosign in.</span></span> <span data-ttu-id="91a9e-128">Tento vzor se často používá v online aplikace tam, kde neexistuje žádný neověřené obsah v hello aplikace k dispozici toohello uživatele.</span><span class="sxs-lookup"><span data-stu-id="91a9e-128">This pattern is commonly used in online applications where there is no unauthenticated content in hello application available toohello user.</span></span> <span data-ttu-id="91a9e-129">Ukázka Hello generované touto s průvodcem instalací používá tento vzor.</span><span class="sxs-lookup"><span data-stu-id="91a9e-129">hello sample generated by this guided setup uses this pattern.</span></span>

2. <span data-ttu-id="91a9e-130">Aplikace lze také nastavit vizuální označení toohello uživatele, který interaktivní přihlášení je povinné, takže hello uživatele můžete vybrat hello správný čas toosign v, nebo můžete zkusit aplikace hello `acquireTokenSilent` později.</span><span class="sxs-lookup"><span data-stu-id="91a9e-130">Applications can also make a visual indication toohello user that an interactive sign-in is required, so hello user can select hello right time toosign in, or hello application can retry `acquireTokenSilent` at a later time.</span></span> <span data-ttu-id="91a9e-131">To se běžně používá, když uživatel hello můžete používat také další funkce aplikace hello bez narušení – například není neověřené obsah k dispozici v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="91a9e-131">This is commonly used when hello user can use other functionality of hello application without being disrupted - for example, there is unauthenticated content available in hello application.</span></span> <span data-ttu-id="91a9e-132">V takovém případě hello uživatele můžete rozhodnout, když chtějí toosign v tooaccess hello chráněný prostředek, nebo toorefresh hello zastaralé informace.</span><span class="sxs-lookup"><span data-stu-id="91a9e-132">In this case, hello user can decide when they want toosign in tooaccess hello protected resource, or toorefresh hello outdated information.</span></span>

<!--end-collapse-->

## <a name="call-hello-microsoft-graph-api-using-hello-token-you-just-obtained"></a><span data-ttu-id="91a9e-133">Volání rozhraní API Microsoft Graph hello pomocí hello tokenu, který jste obdrželi</span><span class="sxs-lookup"><span data-stu-id="91a9e-133">Call hello Microsoft Graph API using hello token you just obtained</span></span>

<span data-ttu-id="91a9e-134">Přidejte následující kód tooyour hello `app.js` souboru:</span><span class="sxs-lookup"><span data-stu-id="91a9e-134">Add hello following code tooyour `app.js` file:</span></span>

```javascript
/**
 * Call a Web API using an access token.
 * @param {any} endpoint - Web API endpoint
 * @param {any} token - Access token
 * @param {object} responseElement - HTML element used toodisplay hello results
 * @param {object} showTokenElement = HTML element used toodisplay hello RAW access token
 */
function callWebApiWithToken(endpoint, token, responseElement, showTokenElement) {
    var headers = new Headers();
    var bearer = "Bearer " + token;
    headers.append("Authorization", bearer);
    var options = {
        method: "GET",
        headers: headers
    };

    fetch(endpoint, options)
        .then(function (response) {
            var contentType = response.headers.get("content-type");
            if (response.status === 200 && contentType && contentType.indexOf("application/json") !== -1) {
                response.json()
                    .then(function (data) {
                        // Display response in hello page
                        console.log(data);
                        responseElement.innerHTML = JSON.stringify(data, null, 4);
                        if (showTokenElement) {
                            showTokenElement.parentElement.classList.remove("hidden");
                            showTokenElement.innerHTML = token;
                        }
                    })
                    .catch(function (error) {
                        showError(endpoint, error);
                    });
            } else {
                response.json()
                    .then(function (data) {
                        // Display response as error in hello page
                        showError(endpoint, data);
                    })
                    .catch(function (error) {
                        showError(endpoint, error);
                    });
            }
        })
        .catch(function (error) {
            showError(endpoint, error);
        });
}
```
<!--start-collapse-->

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a><span data-ttu-id="91a9e-135">Další informace o volání REST chráněné rozhraní API</span><span class="sxs-lookup"><span data-stu-id="91a9e-135">More information on making a REST call against a protected API</span></span>

<span data-ttu-id="91a9e-136">V ukázkové aplikaci hello vytvořit v této příručce hello `callWebApiWithToken()` metoda je použité toomake HTTP `GET` požadavku vůči chráněný prostředek, který vyžaduje volající obsahu toohello tokenu a potom se vraťte hello.</span><span class="sxs-lookup"><span data-stu-id="91a9e-136">In hello sample application created by this guide, hello `callWebApiWithToken()` method is used toomake an HTTP `GET` request against a protected resource that requires a token and then return hello content toohello caller.</span></span> <span data-ttu-id="91a9e-137">Tato metoda přidá hello získat token v hello *HTTP autorizační hlavičky*.</span><span class="sxs-lookup"><span data-stu-id="91a9e-137">This method adds hello acquired token in hello *HTTP Authorization header*.</span></span> <span data-ttu-id="91a9e-138">Pro ukázkovou aplikaci hello vytvořit v této příručce, hello prostředek je hello Microsoft Graph API *mi* koncového bodu – zobrazí informace o profilu uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="91a9e-138">For hello sample application created by this guide, hello resource is hello Microsoft Graph API *me* endpoint – which displays hello user's profile information.</span></span>

<!--end-collapse-->

## <a name="add-a-method-toosign-out-hello-user"></a><span data-ttu-id="91a9e-139">Přidat metoda toosign out hello uživatele</span><span class="sxs-lookup"><span data-stu-id="91a9e-139">Add a method toosign out hello user</span></span>

<span data-ttu-id="91a9e-140">Přidejte následující kód tooyour hello `app.js` souboru:</span><span class="sxs-lookup"><span data-stu-id="91a9e-140">Add hello following code tooyour `app.js` file:</span></span>

```javascript
/**
 * Sign-out hello user
 */
function signOut() {
    userAgentApplication.logout();
}
```
