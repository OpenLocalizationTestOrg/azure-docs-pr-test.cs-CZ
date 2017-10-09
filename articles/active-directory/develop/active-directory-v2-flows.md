---
title: "typy aaaApp pro koncový bod v2.0 hello Azure Active Directory | Microsoft Docs"
description: "Hello typy aplikací a scénáře podporované koncovým bodem v2.0 hello Azure Active Directory."
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
ms.openlocfilehash: db95c88d6865abac8ce80378ccd6b63cb38e0a01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="app-types-for-hello-azure-active-directory-v20-endpoint"></a><span data-ttu-id="73545-103">Typy aplikací pro koncový bod v2.0 hello Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="73545-103">App types for hello Azure Active Directory v2.0 endpoint</span></span>
<span data-ttu-id="73545-104">Hello koncového bodu v2.0 Azure Active Directory (Azure AD) podporuje ověřování pro celou řadu architektur moderní aplikace, všechny z nich založené na standardních protokolech [OAuth 2.0 nebo OpenID Connect](active-directory-v2-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="73545-104">hello Azure Active Directory (Azure AD) v2.0 endpoint supports authentication for a variety of modern app architectures, all of them based on industry-standard protocols [OAuth 2.0 or OpenID Connect](active-directory-v2-protocols.md).</span></span> <span data-ttu-id="73545-105">Tento článek popisuje hello typy aplikací, které můžete vytvořit pomocí Azure AD v2.0, bez ohledu na to upřednostňovaný jazyk nebo platformu.</span><span class="sxs-lookup"><span data-stu-id="73545-105">This article describes hello types of apps that you can build by using Azure AD v2.0, regardless of your preferred language or platform.</span></span> <span data-ttu-id="73545-106">Hello informace v tomto článku je navrženou toohelp pochopit scénáře vysoké úrovně před [zahájení práce s kódem hello](active-directory-appmodel-v2-overview.md#getting-started).</span><span class="sxs-lookup"><span data-stu-id="73545-106">hello information in this article is designed toohelp you understand high-level scenarios before you [start working with hello code](active-directory-appmodel-v2-overview.md#getting-started).</span></span>

> [!NOTE]
> <span data-ttu-id="73545-107">koncový bod v2.0 Hello nepodporuje všechny scénáře Azure Active Directory a funkce.</span><span class="sxs-lookup"><span data-stu-id="73545-107">hello v2.0 endpoint doesn't support all Azure Active Directory scenarios and features.</span></span> <span data-ttu-id="73545-108">toodetermine zda byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="73545-108">toodetermine whether you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="hello-basics"></a><span data-ttu-id="73545-109">Základy Hello</span><span class="sxs-lookup"><span data-stu-id="73545-109">hello basics</span></span>
<span data-ttu-id="73545-110">Je nutné zaregistrovat každou aplikaci, která používá koncového bodu v2.0 hello v hello [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="73545-110">You must register each app that uses hello v2.0 endpoint in hello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com).</span></span> <span data-ttu-id="73545-111">Hello proces registrace aplikace shromáždí a přiřadí tyto hodnoty pro svou aplikaci:</span><span class="sxs-lookup"><span data-stu-id="73545-111">hello app registration process collects and assigns these values for your app:</span></span>

* <span data-ttu-id="73545-112">**ID aplikace** jednoznačně identifikuje vaši aplikaci</span><span class="sxs-lookup"><span data-stu-id="73545-112">An **Application ID** that uniquely identifies your app</span></span>
* <span data-ttu-id="73545-113">A **identifikátor URI pro přesměrování** , které můžete použít toodirect odpovědí zpět tooyour aplikace</span><span class="sxs-lookup"><span data-stu-id="73545-113">A **Redirect URI** that you can use toodirect responses back tooyour app</span></span>
* <span data-ttu-id="73545-114">Pár dalších hodnot konkrétní scénáře</span><span class="sxs-lookup"><span data-stu-id="73545-114">A few other scenario-specific values</span></span>

<span data-ttu-id="73545-115">Podrobnosti najdete další informace jak příliš[zaregistrovat aplikaci](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="73545-115">For details, learn how too[register an app](active-directory-v2-app-registration.md).</span></span>

<span data-ttu-id="73545-116">Po registraci aplikace hello hello aplikace komunikuje se službou Azure AD zasláním požadavků koncového bodu v2.0 toohello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="73545-116">After hello app is registered, hello app communicates with Azure AD by sending requests toohello Azure AD v2.0 endpoint.</span></span> <span data-ttu-id="73545-117">Poskytujeme open-source architektury a knihovny, které zpracovávají hello podrobnosti o tyto požadavky.</span><span class="sxs-lookup"><span data-stu-id="73545-117">We provide open-source frameworks and libraries that handle hello details of these requests.</span></span> <span data-ttu-id="73545-118">Můžete si taky logika ověřování hello možnost tooimplement hello sami vytvořením požadavky toothese koncové body:</span><span class="sxs-lookup"><span data-stu-id="73545-118">You also have hello option tooimplement hello authentication logic yourself by creating requests toothese endpoints:</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```
<!-- TODO: Need a page for libraries toolink too-->

## <a name="web-apps"></a><span data-ttu-id="73545-119">Webové aplikace</span><span class="sxs-lookup"><span data-stu-id="73545-119">Web apps</span></span>
<span data-ttu-id="73545-120">Pro webové aplikace (.NET, PHP, Java, Ruby, Python, uzel) které hello uživatel přistupuje prostřednictvím prohlížeče, můžete použít [OpenID Connect](active-directory-v2-protocols.md) pro přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="73545-120">For web apps (.NET, PHP, Java, Ruby, Python, Node) that hello user accesses through a browser, you can use [OpenID Connect](active-directory-v2-protocols.md) for user sign-in.</span></span> <span data-ttu-id="73545-121">Hello webové aplikace v OpenID Connect, obdrží ID token.</span><span class="sxs-lookup"><span data-stu-id="73545-121">In OpenID Connect, hello web app receives an ID token.</span></span> <span data-ttu-id="73545-122">ID token je token zabezpečení, která ověřuje identitu uživatele hello a poskytuje informace o uživateli hello v podobě hello deklarací identity:</span><span class="sxs-lookup"><span data-stu-id="73545-122">An ID token is a security token that verifies hello user's identity and provides information about hello user in hello form of claims:</span></span>

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

<span data-ttu-id="73545-123">Informace o všech hello typy tokenů a deklaracích identity, které jsou k dispozici tooan aplikace v hello najdete [v2.0 tokeny odkaz](active-directory-v2-tokens.md).</span><span class="sxs-lookup"><span data-stu-id="73545-123">You can learn about all hello types of tokens and claims that are available tooan app in hello [v2.0 tokens reference](active-directory-v2-tokens.md).</span></span>

<span data-ttu-id="73545-124">V serveru webové aplikace trvá tok ověřování přihlášení hello těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="73545-124">In web server apps, hello sign-in authentication flow takes these high-level steps:</span></span>

![Tok ověřování webové aplikace](../../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

<span data-ttu-id="73545-126">Identita uživatele hello můžete zajistit ověřením hello ID token pomocí veřejného podpisového klíče přijatého z koncového bodu v2.0 hello.</span><span class="sxs-lookup"><span data-stu-id="73545-126">You can ensure hello user's identity by validating hello ID token with a public signing key that is received from hello v2.0 endpoint.</span></span> <span data-ttu-id="73545-127">Soubor cookie relace je nastavena, které lze použít tooidentify hello uživatelské požadavky na dalších stránkách.</span><span class="sxs-lookup"><span data-stu-id="73545-127">A session cookie is set, which can be used tooidentify hello user on subsequent page requests.</span></span>

<span data-ttu-id="73545-128">toosee ukázky tento scénář v akci, zkuste jeden hello webové aplikace přihlášení kódu v našem v2.0 [Začínáme](active-directory-appmodel-v2-overview.md#getting-started) části.</span><span class="sxs-lookup"><span data-stu-id="73545-128">toosee this scenario in action, try one of hello web app sign-in code samples in our v2.0 [Getting Started](active-directory-appmodel-v2-overview.md#getting-started) section.</span></span>

<span data-ttu-id="73545-129">Kromě toho toosimple přihlášení, může webová aplikace může být nutné tooaccess jiné webové služby, jako je například rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="73545-129">In addition toosimple sign-in, a web server app might need tooaccess another web service, such as a REST API.</span></span> <span data-ttu-id="73545-130">V takovém případě webová aplikace hello zapojí v kombinovaném OpenID Connect a OAuth 2.0 toku pomocí hello [toku kódu autorizace OAuth 2.0](active-directory-v2-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="73545-130">In this case, hello web server app engages in a combined OpenID Connect and OAuth 2.0 flow, by using hello [OAuth 2.0 authorization code flow](active-directory-v2-protocols.md).</span></span> <span data-ttu-id="73545-131">Další informace o tomto scénáři, přečtěte si informace o [Začínáme s webovými aplikacemi a webovým rozhraním API](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="73545-131">For more information about this scenario, read about [getting started with web apps and Web APIs](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).</span></span>

## <a name="web-apis"></a><span data-ttu-id="73545-132">Webová rozhraní API</span><span class="sxs-lookup"><span data-stu-id="73545-132">Web APIs</span></span>
<span data-ttu-id="73545-133">Můžete použít hello v2.0 koncový bod toosecure webové služby, jako je vaše aplikace RESTful webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="73545-133">You can use hello v2.0 endpoint toosecure web services, such as your app's RESTful Web API.</span></span> <span data-ttu-id="73545-134">Místo ID tokeny a soubory cookie relace, webového rozhraní API používá toosecure tokenu přístupu OAuth 2.0 jeho data a tooauthenticate příchozí požadavky.</span><span class="sxs-lookup"><span data-stu-id="73545-134">Instead of ID tokens and session cookies, a Web API uses an OAuth 2.0 access token toosecure its data and tooauthenticate incoming requests.</span></span> <span data-ttu-id="73545-135">Hello volající webového rozhraní API připojí token přístupu v hello autorizační hlavičky požadavku HTTP, například takto:</span><span class="sxs-lookup"><span data-stu-id="73545-135">hello caller of a Web API appends an access token in hello authorization header of an HTTP request, like this:</span></span>

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

<span data-ttu-id="73545-136">Hello webového rozhraní API používá hello přístup tokenu tooverify hello volajícího identitu a tooextract informace o hello volajícím z deklarací identity zakódovaných v tokenu přístupu hello.</span><span class="sxs-lookup"><span data-stu-id="73545-136">hello Web API uses hello access token tooverify hello API caller's identity and tooextract information about hello caller from claims that are encoded in hello access token.</span></span> <span data-ttu-id="73545-137">toolearn o všechny hello typy tokenů a deklaracích identity, které jsou k dispozici tooan aplikace, najdete v části hello [v2.0 tokeny odkaz](active-directory-v2-tokens.md).</span><span class="sxs-lookup"><span data-stu-id="73545-137">toolearn about all hello types of tokens and claims that are available tooan app, see hello [v2.0 tokens reference](active-directory-v2-tokens.md).</span></span>

<span data-ttu-id="73545-138">Webové rozhraní API můžete poskytnout uživatelům hello power tooopt nebo vyjádření výslovného nesouhlasu s konkrétní funkce nebo data zveřejněním oprávnění, také známé jako [obory](active-directory-v2-scopes.md).</span><span class="sxs-lookup"><span data-stu-id="73545-138">A Web API can give users hello power tooopt in or opt out of specific functionality or data by exposing permissions, also known as [scopes](active-directory-v2-scopes.md).</span></span> <span data-ttu-id="73545-139">Pro volání aplikace tooacquire oprávnění tooa obor hello uživatel musí vyjádřit souhlas toohello oboru během k toku.</span><span class="sxs-lookup"><span data-stu-id="73545-139">For a calling app tooacquire permission tooa scope, hello user must consent toohello scope during a flow.</span></span> <span data-ttu-id="73545-140">koncový bod v2.0 Hello hello uživatel požádá o oprávnění a potom zaznamenává všechny přístupových tokenů oprávnění tohoto hello, které obdrží webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="73545-140">hello v2.0 endpoint asks hello user for permission, and then records permissions in all access tokens that hello Web API receives.</span></span> <span data-ttu-id="73545-141">Hello webového rozhraní API ověří hello přístupové tokeny přijetí při každém volání a provádí kontroly autorizace.</span><span class="sxs-lookup"><span data-stu-id="73545-141">hello Web API validates hello access tokens it receives on each call and performs authorization checks.</span></span>

<span data-ttu-id="73545-142">Webové rozhraní API může přijímat tokeny přístupu ze všech typů aplikací, včetně webových aplikací serveru, desktop a mobilní aplikace, jednostránkové aplikace, démonů na straně serveru a i další webovým rozhraním API.</span><span class="sxs-lookup"><span data-stu-id="73545-142">A Web API can receive access tokens from all types of apps, including web server apps, desktop and mobile apps, single-page apps, server-side daemons, and even other Web APIs.</span></span> <span data-ttu-id="73545-143">Hello podrobný postup pro webového rozhraní API vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="73545-143">hello high-level flow for a Web API looks like this:</span></span>

![Tok ověření webového rozhraní API](../../media/active-directory-v2-flows/convergence_scenarios_webapi.png)

<span data-ttu-id="73545-145">toolearn jak ukázky toosecure webového rozhraní API pomocí přístupových tokenů OAuth2, podívejte se na hello kódu webového rozhraní API v našem [Začínáme](active-directory-appmodel-v2-overview.md#getting-started) části.</span><span class="sxs-lookup"><span data-stu-id="73545-145">toolearn how toosecure a Web API by using OAuth2 access tokens, check out hello Web API code samples in our [Getting Started](active-directory-appmodel-v2-overview.md#getting-started) section.</span></span>

<span data-ttu-id="73545-146">Webové rozhraní API v mnoha případech také potřebovat toomake odchozí požadavky po proudu tooother webové rozhraní API pro zabezpečené službou Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="73545-146">In many cases, web APIs also need toomake outbound requests tooother downstream web APIs secured by Azure Active Directory.</span></span>  <span data-ttu-id="73545-147">toodo tedy webových rozhraní API můžete využít výhod Azure AD **na jménem z** tok, který umožňuje hello webového rozhraní API tooexchange příchozí přístupový token pro jiné toobe tokenu přístupu, které se používá v odchozí požadavky.</span><span class="sxs-lookup"><span data-stu-id="73545-147">toodo so, web APIs can take advantage of Azure AD's **On Behalf Of** flow, which allows hello web API tooexchange an incoming access token for another access token toobe used in outbound requests.</span></span>  <span data-ttu-id="73545-148">Hello v2.0 pro koncový bod jménem postup je popsán v [podrobností zde](active-directory-v2-protocols-oauth-on-behalf-of.md).</span><span class="sxs-lookup"><span data-stu-id="73545-148">hello v2.0 endpoint's On Behalf Of flow is described in [detail here](active-directory-v2-protocols-oauth-on-behalf-of.md).</span></span>

## <a name="mobile-and-native-apps"></a><span data-ttu-id="73545-149">Mobilní a nativní aplikace</span><span class="sxs-lookup"><span data-stu-id="73545-149">Mobile and native apps</span></span>
<span data-ttu-id="73545-150">Zařízení nainstalované aplikace, jako je například mobilních a desktopových aplikací, často potřebují tooaccess back endovým službám nebo webovým rozhraním API, která ukládají data a provádět operace jménem uživatele.</span><span class="sxs-lookup"><span data-stu-id="73545-150">Device-installed apps, such as mobile and desktop apps, often need tooaccess back-end services or Web APIs that store data and perform functions on behalf of a user.</span></span> <span data-ttu-id="73545-151">Tyto aplikace můžete přidat přihlašování a autorizaci tooback endové služby pomocí hello [toku kódu autorizace OAuth 2.0](active-directory-v2-protocols-oauth-code.md).</span><span class="sxs-lookup"><span data-stu-id="73545-151">These apps can add sign-in and authorization tooback-end services by using hello [OAuth 2.0 authorization code flow](active-directory-v2-protocols-oauth-code.md).</span></span>

<span data-ttu-id="73545-152">V tomto toku aplikace hello obdrží autorizační kód z koncového bodu v2.0 hello při přihlášení uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="73545-152">In this flow, hello app receives an authorization code from hello v2.0 endpoint when hello user signs in.</span></span> <span data-ttu-id="73545-153">Hello autorizační kód představuje hello aplikace oprávnění toocall back endové služby jménem uživatele hello, kdo je přihlášený.</span><span class="sxs-lookup"><span data-stu-id="73545-153">hello authorization code represents hello app's permission toocall back-end services on behalf of hello user who is signed in.</span></span> <span data-ttu-id="73545-154">aplikace Hello může provést výměnu hello autorizační kód hello pozadí pro přístupový token OAuth 2.0 a obnovovací token.</span><span class="sxs-lookup"><span data-stu-id="73545-154">hello app can exchange hello authorization code in hello background for an OAuth 2.0 access token and a refresh token.</span></span> <span data-ttu-id="73545-155">aplikace Hello můžete využít hello přístup tokenu tooauthenticate tooWeb rozhraní API v požadavcích HTTP a pomocí hello aktualizace tokenu tooget nové přístupové tokeny, když vyprší platnost starší přístupových tokenů.</span><span class="sxs-lookup"><span data-stu-id="73545-155">hello app can use hello access token tooauthenticate tooWeb APIs in HTTP requests, and use hello refresh token tooget new access tokens when older access tokens expire.</span></span>

![Tok ověřování nativní aplikace](../../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="single-page-apps-javascript"></a><span data-ttu-id="73545-157">Jednostránkové aplikace (JavaScript)</span><span class="sxs-lookup"><span data-stu-id="73545-157">Single-page apps (JavaScript)</span></span>
<span data-ttu-id="73545-158">Řada moderních aplikací využívá jednostránkový front-end, který je primárně napsané v jazyce JavaScript.</span><span class="sxs-lookup"><span data-stu-id="73545-158">Many modern apps have a single-page app front end that primarily is written in JavaScript.</span></span> <span data-ttu-id="73545-159">Často je zapsán pomocí rozhraní jako AngularJS, Ember.js nebo Durandal.js.</span><span class="sxs-lookup"><span data-stu-id="73545-159">Often, it's written by using a framework like AngularJS, Ember.js, or Durandal.js.</span></span> <span data-ttu-id="73545-160">koncový bod v2.0 Hello Azure AD podporuje tyto aplikace pomocí hello [implicitního toku OAuth 2.0](active-directory-v2-protocols-implicit.md).</span><span class="sxs-lookup"><span data-stu-id="73545-160">hello Azure AD v2.0 endpoint supports these apps by using hello [OAuth 2.0 implicit flow](active-directory-v2-protocols-implicit.md).</span></span>

<span data-ttu-id="73545-161">V tomto toku aplikace hello obdrží tokeny přímo z hello v2.0 zajistí autorizaci koncového bodu, bez jakékoli výměnu serveru na server.</span><span class="sxs-lookup"><span data-stu-id="73545-161">In this flow, hello app receives tokens directly from hello v2.0 authorize endpoint, without any server-to-server exchanges.</span></span> <span data-ttu-id="73545-162">Všechny logiku ověřování a relace trvá zpracování umístit zcela v hello JavaScript klienta, a to bez přesměrování další stránky.</span><span class="sxs-lookup"><span data-stu-id="73545-162">All authentication logic and session handling takes place entirely in hello JavaScript client, without extra page redirects.</span></span>

![Tok implicitní ověřování](../../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

<span data-ttu-id="73545-164">toosee tento scénář v akci, vyzkoušejte některý z hello jednostránkové aplikace ukázky kódu v našem [Začínáme](active-directory-appmodel-v2-overview.md#getting-started) části.</span><span class="sxs-lookup"><span data-stu-id="73545-164">toosee this scenario in action, try one of hello single-page app code samples in our [Getting Started](active-directory-appmodel-v2-overview.md#getting-started) section.</span></span>

## <a name="daemons-and-server-side-apps"></a><span data-ttu-id="73545-165">Démoni a serverové aplikace</span><span class="sxs-lookup"><span data-stu-id="73545-165">Daemons and server-side apps</span></span>
<span data-ttu-id="73545-166">Aplikace, které mají dlouho běžící procesy nebo které pracují bez interakce s uživatelem také potřebovat způsob, jakým tooaccess zabezpečeným prostředkům, jako jsou webová rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="73545-166">Apps that have long-running processes or that operate without interaction with a user also need a way tooaccess secured resources, such as Web APIs.</span></span> <span data-ttu-id="73545-167">Tyto aplikace se můžou ověřovat a získat tokeny pomocí identity aplikace hello namísto uživatele delegované identity s tok přihlašovacích údajů klienta hello OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="73545-167">These apps can authenticate and get tokens by using hello app's identity, rather than a user's delegated identity, with hello OAuth 2.0 client credentials flow.</span></span>

<span data-ttu-id="73545-168">V tomto toku aplikace hello komunikuje přímo s hello `/token` koncové body tooobtain koncový bod:</span><span class="sxs-lookup"><span data-stu-id="73545-168">In this flow, hello app interacts directly with hello `/token` endpoint tooobtain endpoints:</span></span>

![Démon tok ověřování aplikace](../../media/active-directory-v2-flows/convergence_scenarios_daemon.png)

<span data-ttu-id="73545-170">toobuild démon aplikace, najdete v dokumentaci pověření klienta hello v našem [Začínáme](active-directory-appmodel-v2-overview.md#getting-started) část, nebo se pokuste [ukázkové aplikace .NET](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).</span><span class="sxs-lookup"><span data-stu-id="73545-170">toobuild a daemon app, see hello client credentials documentation in our [Getting Started](active-directory-appmodel-v2-overview.md#getting-started) section, or try a [.NET sample app](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).</span></span>
