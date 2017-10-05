---
title: "Typy aplikací pro koncový bod v2.0 Azure Active Directory | Microsoft Docs"
description: "Typy aplikací a scénáře podporované koncovým bodem v2.0 Azure Active Directory."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 494a06b8-0f9b-44e1-a7a2-d728cf2077ae
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 9d59e7f0e8f326c40be86e199d7712f6c565cc13
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="app-types-for-the-azure-active-directory-v20-endpoint"></a><span data-ttu-id="8cc35-103">Typy aplikací pro koncový bod v2.0 Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8cc35-103">App types for the Azure Active Directory v2.0 endpoint</span></span>
<span data-ttu-id="8cc35-104">Koncový bod v2.0 Azure Active Directory (Azure AD) podporuje ověřování pro celou řadu architektur moderní aplikace, všechny z nich založené na standardních protokolech [OAuth 2.0 nebo OpenID Connect](active-directory-v2-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="8cc35-104">The Azure Active Directory (Azure AD) v2.0 endpoint supports authentication for a variety of modern app architectures, all of them based on industry-standard protocols [OAuth 2.0 or OpenID Connect](active-directory-v2-protocols.md).</span></span> <span data-ttu-id="8cc35-105">Tento článek popisuje typy aplikací, které můžete vytvořit pomocí Azure AD v2.0, bez ohledu na to upřednostňovaný jazyk nebo platformu.</span><span class="sxs-lookup"><span data-stu-id="8cc35-105">This article describes the types of apps that you can build by using Azure AD v2.0, regardless of your preferred language or platform.</span></span> <span data-ttu-id="8cc35-106">Informace v tomto článku je navržená tak, které vám pomohou pochopit scénáře vysoké úrovně před [zahájení práce s kódem](active-directory-appmodel-v2-overview.md#getting-started).</span><span class="sxs-lookup"><span data-stu-id="8cc35-106">The information in this article is designed to help you understand high-level scenarios before you [start working with the code](active-directory-appmodel-v2-overview.md#getting-started).</span></span>

> [!NOTE]
> <span data-ttu-id="8cc35-107">Koncový bod v2.0 nepodporuje všechny scénáře Azure Active Directory a funkce.</span><span class="sxs-lookup"><span data-stu-id="8cc35-107">The v2.0 endpoint doesn't support all Azure Active Directory scenarios and features.</span></span> <span data-ttu-id="8cc35-108">Pokud chcete zjistit, zda byste měli používat koncový bod v2.0, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="8cc35-108">To determine whether you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="the-basics"></a><span data-ttu-id="8cc35-109">Základy</span><span class="sxs-lookup"><span data-stu-id="8cc35-109">The basics</span></span>
<span data-ttu-id="8cc35-110">Je nutné zaregistrovat každou aplikaci, která používá koncový bod v2.0 v [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="8cc35-110">You must register each app that uses the v2.0 endpoint in the [Microsoft Application Registration Portal](https://apps.dev.microsoft.com).</span></span> <span data-ttu-id="8cc35-111">Proces registrace aplikace shromáždí a přiřadí tyto hodnoty pro svou aplikaci:</span><span class="sxs-lookup"><span data-stu-id="8cc35-111">The app registration process collects and assigns these values for your app:</span></span>

* <span data-ttu-id="8cc35-112">**ID aplikace** jednoznačně identifikuje vaši aplikaci</span><span class="sxs-lookup"><span data-stu-id="8cc35-112">An **Application ID** that uniquely identifies your app</span></span>
* <span data-ttu-id="8cc35-113">A **identifikátor URI pro přesměrování** , můžete použít k cílení odpovědí zpět do aplikace</span><span class="sxs-lookup"><span data-stu-id="8cc35-113">A **Redirect URI** that you can use to direct responses back to your app</span></span>
* <span data-ttu-id="8cc35-114">Pár dalších hodnot konkrétní scénáře</span><span class="sxs-lookup"><span data-stu-id="8cc35-114">A few other scenario-specific values</span></span>

<span data-ttu-id="8cc35-115">Další podrobnosti, jak [zaregistrovat aplikaci](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="8cc35-115">For details, learn how to [register an app](active-directory-v2-app-registration.md).</span></span>

<span data-ttu-id="8cc35-116">Po registraci aplikace aplikace komunikuje se službou Azure AD pomocí zasílání požadavků do koncového bodu v2.0 Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8cc35-116">After the app is registered, the app communicates with Azure AD by sending requests to the Azure AD v2.0 endpoint.</span></span> <span data-ttu-id="8cc35-117">Poskytujeme open-source architektury a knihovny, které zpracovávají údaje o tyto požadavky.</span><span class="sxs-lookup"><span data-stu-id="8cc35-117">We provide open-source frameworks and libraries that handle the details of these requests.</span></span> <span data-ttu-id="8cc35-118">Máte také možnost implementovat logiku ověřování vytvořením požadavky s těmito koncovými body:</span><span class="sxs-lookup"><span data-stu-id="8cc35-118">You also have the option to implement the authentication logic yourself by creating requests to these endpoints:</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```
<!-- TODO: Need a page for libraries to link to -->

## <a name="web-apps"></a><span data-ttu-id="8cc35-119">Webové aplikace</span><span class="sxs-lookup"><span data-stu-id="8cc35-119">Web apps</span></span>
<span data-ttu-id="8cc35-120">Pro webové aplikace (.NET, PHP, Java, Ruby, Python, uzel) kterým uživatel přistupuje prostřednictvím prohlížeče, můžete použít [OpenID Connect](active-directory-v2-protocols.md) pro přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="8cc35-120">For web apps (.NET, PHP, Java, Ruby, Python, Node) that the user accesses through a browser, you can use [OpenID Connect](active-directory-v2-protocols.md) for user sign-in.</span></span> <span data-ttu-id="8cc35-121">Webové aplikace v OpenID Connect, obdrží ID token.</span><span class="sxs-lookup"><span data-stu-id="8cc35-121">In OpenID Connect, the web app receives an ID token.</span></span> <span data-ttu-id="8cc35-122">ID token je token zabezpečení, která ověřuje identitu uživatele a poskytuje informace o uživateli ve formě deklarací identity:</span><span class="sxs-lookup"><span data-stu-id="8cc35-122">An ID token is a security token that verifies the user's identity and provides information about the user in the form of claims:</span></span>

```
// Partial raw ID token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded ID token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

<span data-ttu-id="8cc35-123">Dozvíte se o všech typech tokenů a deklaracích identity, které jsou k dispozici pro aplikaci [v2.0 tokeny odkaz](active-directory-v2-tokens.md).</span><span class="sxs-lookup"><span data-stu-id="8cc35-123">You can learn about all the types of tokens and claims that are available to an app in the [v2.0 tokens reference](active-directory-v2-tokens.md).</span></span>

<span data-ttu-id="8cc35-124">V serveru webové aplikace trvá tok ověření přihlášení těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="8cc35-124">In web server apps, the sign-in authentication flow takes these high-level steps:</span></span>

![Tok ověřování webové aplikace](../../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

<span data-ttu-id="8cc35-126">Zajistíte tím, že ověří ID token pomocí veřejného podpisového klíče přijatého z koncového bodu v2.0 identitu uživatele.</span><span class="sxs-lookup"><span data-stu-id="8cc35-126">You can ensure the user's identity by validating the ID token with a public signing key that is received from the v2.0 endpoint.</span></span> <span data-ttu-id="8cc35-127">Soubor cookie relace je nastavit, které lze použít k identifikaci uživatele požadavky na dalších stránkách.</span><span class="sxs-lookup"><span data-stu-id="8cc35-127">A session cookie is set, which can be used to identify the user on subsequent page requests.</span></span>

<span data-ttu-id="8cc35-128">Pokud chcete zobrazit tento scénář v akci, vyzkoušejte některý z ukázky přihlášení kódu webové aplikace v našem v2.0 [Začínáme](active-directory-appmodel-v2-overview.md#getting-started) části.</span><span class="sxs-lookup"><span data-stu-id="8cc35-128">To see this scenario in action, try one of the web app sign-in code samples in our v2.0 [Getting Started](active-directory-appmodel-v2-overview.md#getting-started) section.</span></span>

<span data-ttu-id="8cc35-129">Kromě jednoduchého přihlášení může server webové aplikace potřebujete přístup k jiné webové služby, jako je například rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="8cc35-129">In addition to simple sign-in, a web server app might need to access another web service, such as a REST API.</span></span> <span data-ttu-id="8cc35-130">V takovém případě zapojí aplikace webového serveru v kombinovaném OpenID Connect a OAuth 2.0 toku, pomocí [toku kódu autorizace OAuth 2.0](active-directory-v2-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="8cc35-130">In this case, the web server app engages in a combined OpenID Connect and OAuth 2.0 flow, by using the [OAuth 2.0 authorization code flow](active-directory-v2-protocols.md).</span></span> <span data-ttu-id="8cc35-131">Další informace o tomto scénáři, přečtěte si informace o [Začínáme s webovými aplikacemi a webovým rozhraním API](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="8cc35-131">For more information about this scenario, read about [getting started with web apps and Web APIs](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).</span></span>

## <a name="web-apis"></a><span data-ttu-id="8cc35-132">Webová rozhraní API</span><span class="sxs-lookup"><span data-stu-id="8cc35-132">Web APIs</span></span>
<span data-ttu-id="8cc35-133">Koncový bod v2.0 můžete použít k zabezpečení webových služeb, jako je vaše aplikace RESTful webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8cc35-133">You can use the v2.0 endpoint to secure web services, such as your app's RESTful Web API.</span></span> <span data-ttu-id="8cc35-134">Místo ID tokeny a soubory cookie relace webového rozhraní API používá přístupový token OAuth 2.0 pro zabezpečené svoje data a ověřují příchozí požadavky.</span><span class="sxs-lookup"><span data-stu-id="8cc35-134">Instead of ID tokens and session cookies, a Web API uses an OAuth 2.0 access token to secure its data and to authenticate incoming requests.</span></span> <span data-ttu-id="8cc35-135">Volající webového rozhraní API připojí token přístupu v hlavičce autorizace požadavku HTTP, například takto:</span><span class="sxs-lookup"><span data-stu-id="8cc35-135">The caller of a Web API appends an access token in the authorization header of an HTTP request, like this:</span></span>

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

<span data-ttu-id="8cc35-136">Webového rozhraní API používá přístupový token ověřit identitu volajícího a extrahovat informace o volajícím z deklarací identity zakódovaných v tokenu přístupu.</span><span class="sxs-lookup"><span data-stu-id="8cc35-136">The Web API uses the access token to verify the API caller's identity and to extract information about the caller from claims that are encoded in the access token.</span></span> <span data-ttu-id="8cc35-137">Další informace o všech typech tokenů a deklaracích identity, které jsou k dispozici pro aplikace, najdete v článku [v2.0 tokeny odkaz](active-directory-v2-tokens.md).</span><span class="sxs-lookup"><span data-stu-id="8cc35-137">To learn about all the types of tokens and claims that are available to an app, see the [v2.0 tokens reference](active-directory-v2-tokens.md).</span></span>

<span data-ttu-id="8cc35-138">Webové rozhraní API můžete uživatelům udělit napájení vyjádřit výslovný souhlas nebo vyjádření výslovného nesouhlasu s konkrétní funkce nebo data zveřejněním oprávnění, také známé jako [obory](active-directory-v2-scopes.md).</span><span class="sxs-lookup"><span data-stu-id="8cc35-138">A Web API can give users the power to opt in or opt out of specific functionality or data by exposing permissions, also known as [scopes](active-directory-v2-scopes.md).</span></span> <span data-ttu-id="8cc35-139">Pro volání aplikaci získat oprávnění k oboru musí uživatel souhlasit do oboru během k toku.</span><span class="sxs-lookup"><span data-stu-id="8cc35-139">For a calling app to acquire permission to a scope, the user must consent to the scope during a flow.</span></span> <span data-ttu-id="8cc35-140">Koncový bod v2.0 požádá uživatele o oprávnění a potom zaznamenává oprávnění ve všech přístupové tokeny, které obdrží webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8cc35-140">The v2.0 endpoint asks the user for permission, and then records permissions in all access tokens that the Web API receives.</span></span> <span data-ttu-id="8cc35-141">Rozhraní Web API ověří přístupových tokenů přijetí při každém volání a provádí kontroly autorizace.</span><span class="sxs-lookup"><span data-stu-id="8cc35-141">The Web API validates the access tokens it receives on each call and performs authorization checks.</span></span>

<span data-ttu-id="8cc35-142">Webové rozhraní API může přijímat tokeny přístupu ze všech typů aplikací, včetně webových aplikací serveru, desktop a mobilní aplikace, jednostránkové aplikace, démonů na straně serveru a i další webovým rozhraním API.</span><span class="sxs-lookup"><span data-stu-id="8cc35-142">A Web API can receive access tokens from all types of apps, including web server apps, desktop and mobile apps, single-page apps, server-side daemons, and even other Web APIs.</span></span> <span data-ttu-id="8cc35-143">Podrobný postup pro webového rozhraní API vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="8cc35-143">The high-level flow for a Web API looks like this:</span></span>

![Tok ověření webového rozhraní API](../../media/active-directory-v2-flows/convergence_scenarios_webapi.png)

<span data-ttu-id="8cc35-145">Chcete-li se dozvědět, jak zabezpečit webové rozhraní API pomocí přístupových tokenů OAuth2, podívejte se na ukázky kódu webového rozhraní API v našich [Začínáme](active-directory-appmodel-v2-overview.md#getting-started) části.</span><span class="sxs-lookup"><span data-stu-id="8cc35-145">To learn how to secure a Web API by using OAuth2 access tokens, check out the Web API code samples in our [Getting Started](active-directory-appmodel-v2-overview.md#getting-started) section.</span></span>

<span data-ttu-id="8cc35-146">V mnoha případech webové rozhraní API také třeba, aby odchozí požadavky na jiné podřízené webové rozhraní API pro zabezpečené službou Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="8cc35-146">In many cases, web APIs also need to make outbound requests to other downstream web APIs secured by Azure Active Directory.</span></span>  <span data-ttu-id="8cc35-147">Uděláte to tak, webová rozhraní API můžete využít výhod Azure AD **na jménem z** tok, který umožňuje webové rozhraní API pro výměnu příchozí přístupový token pro dalšího přístupového tokenu, který se má použít v odchozí požadavky.</span><span class="sxs-lookup"><span data-stu-id="8cc35-147">To do so, web APIs can take advantage of Azure AD's **On Behalf Of** flow, which allows the web API to exchange an incoming access token for another access token to be used in outbound requests.</span></span>  <span data-ttu-id="8cc35-148">V2.0 pro koncový bod jménem postup je popsán v [podrobností zde](active-directory-v2-protocols-oauth-on-behalf-of.md).</span><span class="sxs-lookup"><span data-stu-id="8cc35-148">The v2.0 endpoint's On Behalf Of flow is described in [detail here](active-directory-v2-protocols-oauth-on-behalf-of.md).</span></span>

## <a name="mobile-and-native-apps"></a><span data-ttu-id="8cc35-149">Mobilní a nativní aplikace</span><span class="sxs-lookup"><span data-stu-id="8cc35-149">Mobile and native apps</span></span>
<span data-ttu-id="8cc35-150">Zařízení nainstalované aplikace, jako je například mobilních a desktopových aplikací, často potřebují přístup k back endovým službám nebo webovým rozhraním API, která ukládají data a provádět operace jménem uživatele.</span><span class="sxs-lookup"><span data-stu-id="8cc35-150">Device-installed apps, such as mobile and desktop apps, often need to access back-end services or Web APIs that store data and perform functions on behalf of a user.</span></span> <span data-ttu-id="8cc35-151">Tyto aplikace můžete přidat přihlašování a autorizaci k back endové služby pomocí [toku kódu autorizace OAuth 2.0](active-directory-v2-protocols-oauth-code.md).</span><span class="sxs-lookup"><span data-stu-id="8cc35-151">These apps can add sign-in and authorization to back-end services by using the [OAuth 2.0 authorization code flow](active-directory-v2-protocols-oauth-code.md).</span></span>

<span data-ttu-id="8cc35-152">V tomto toku aplikace obdrží autorizační kód z koncového bodu v2.0 při přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="8cc35-152">In this flow, the app receives an authorization code from the v2.0 endpoint when the user signs in.</span></span> <span data-ttu-id="8cc35-153">Autorizační kód představuje oprávnění aplikace volat back endové služby jménem uživatele, který je přihlášený.</span><span class="sxs-lookup"><span data-stu-id="8cc35-153">The authorization code represents the app's permission to call back-end services on behalf of the user who is signed in.</span></span> <span data-ttu-id="8cc35-154">Aplikace může provést výměnu autorizační kód na pozadí pro přístupový token OAuth 2.0 a obnovovací token.</span><span class="sxs-lookup"><span data-stu-id="8cc35-154">The app can exchange the authorization code in the background for an OAuth 2.0 access token and a refresh token.</span></span> <span data-ttu-id="8cc35-155">Aplikace můžete používat k ověřování k webovým rozhraním API v požadavcích HTTP přístupový token a použít obnovovací token získat nové přístupové tokeny, když vyprší platnost starší přístupové tokeny.</span><span class="sxs-lookup"><span data-stu-id="8cc35-155">The app can use the access token to authenticate to Web APIs in HTTP requests, and use the refresh token to get new access tokens when older access tokens expire.</span></span>

![Tok ověřování nativní aplikace](../../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="single-page-apps-javascript"></a><span data-ttu-id="8cc35-157">Jednostránkové aplikace (JavaScript)</span><span class="sxs-lookup"><span data-stu-id="8cc35-157">Single-page apps (JavaScript)</span></span>
<span data-ttu-id="8cc35-158">Řada moderních aplikací využívá jednostránkový front-end, který je primárně napsané v jazyce JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8cc35-158">Many modern apps have a single-page app front end that primarily is written in JavaScript.</span></span> <span data-ttu-id="8cc35-159">Často je zapsán pomocí rozhraní jako AngularJS, Ember.js nebo Durandal.js.</span><span class="sxs-lookup"><span data-stu-id="8cc35-159">Often, it's written by using a framework like AngularJS, Ember.js, or Durandal.js.</span></span> <span data-ttu-id="8cc35-160">Koncový bod v2.0 Azure AD podporuje tyto aplikace pomocí [implicitního toku OAuth 2.0](active-directory-v2-protocols-implicit.md).</span><span class="sxs-lookup"><span data-stu-id="8cc35-160">The Azure AD v2.0 endpoint supports these apps by using the [OAuth 2.0 implicit flow](active-directory-v2-protocols-implicit.md).</span></span>

<span data-ttu-id="8cc35-161">V tomto toku aplikace obdrží tokeny přímo z v2.0 zajistí autorizaci koncového bodu, bez jakékoli výměnu serveru na server.</span><span class="sxs-lookup"><span data-stu-id="8cc35-161">In this flow, the app receives tokens directly from the v2.0 authorize endpoint, without any server-to-server exchanges.</span></span> <span data-ttu-id="8cc35-162">Všechny logiku ověřování a relace trvá zpracování umístit zcela v JavaScript klienta, bez přesměrování další stránky.</span><span class="sxs-lookup"><span data-stu-id="8cc35-162">All authentication logic and session handling takes place entirely in the JavaScript client, without extra page redirects.</span></span>

![Tok implicitní ověřování](../../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

<span data-ttu-id="8cc35-164">Pokud chcete zobrazit tento scénář v akci, vyzkoušejte některý z ukázky kódu jednostránkové aplikace v našem [Začínáme](active-directory-appmodel-v2-overview.md#getting-started) části.</span><span class="sxs-lookup"><span data-stu-id="8cc35-164">To see this scenario in action, try one of the single-page app code samples in our [Getting Started](active-directory-appmodel-v2-overview.md#getting-started) section.</span></span>

## <a name="daemons-and-server-side-apps"></a><span data-ttu-id="8cc35-165">Démoni a serverové aplikace</span><span class="sxs-lookup"><span data-stu-id="8cc35-165">Daemons and server-side apps</span></span>
<span data-ttu-id="8cc35-166">Aplikace, které mají dlouho běžící procesy nebo které pracují bez interakce s uživatelem také potřebují způsob, jak přistupovat k zabezpečeným prostředkům, například webovým rozhraním API.</span><span class="sxs-lookup"><span data-stu-id="8cc35-166">Apps that have long-running processes or that operate without interaction with a user also need a way to access secured resources, such as Web APIs.</span></span> <span data-ttu-id="8cc35-167">Tyto aplikace se můžou ověřovat a získat tokeny pomocí identity aplikace, a nikoli uživatele delegované identity s tok přihlašovacích údajů klienta OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="8cc35-167">These apps can authenticate and get tokens by using the app's identity, rather than a user's delegated identity, with the OAuth 2.0 client credentials flow.</span></span>

<span data-ttu-id="8cc35-168">V tomto toku aplikace komunikuje přímo s `/token` koncový bod k získání koncové body:</span><span class="sxs-lookup"><span data-stu-id="8cc35-168">In this flow, the app interacts directly with the `/token` endpoint to obtain endpoints:</span></span>

![Démon tok ověřování aplikace](../../media/active-directory-v2-flows/convergence_scenarios_daemon.png)

<span data-ttu-id="8cc35-170">Vytvořit aplikaci démon, naleznete v dokumentaci pověření klienta v našich [Začínáme](active-directory-appmodel-v2-overview.md#getting-started) část, nebo se pokuste [ukázkové aplikace .NET](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).</span><span class="sxs-lookup"><span data-stu-id="8cc35-170">To build a daemon app, see the client credentials documentation in our [Getting Started](active-directory-appmodel-v2-overview.md#getting-started) section, or try a [.NET sample app](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).</span></span>
