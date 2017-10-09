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
# <a name="app-types-for-hello-azure-active-directory-v20-endpoint"></a>Typy aplikací pro koncový bod v2.0 hello Azure Active Directory
Hello koncového bodu v2.0 Azure Active Directory (Azure AD) podporuje ověřování pro celou řadu architektur moderní aplikace, všechny z nich založené na standardních protokolech [OAuth 2.0 nebo OpenID Connect](active-directory-v2-protocols.md). Tento článek popisuje hello typy aplikací, které můžete vytvořit pomocí Azure AD v2.0, bez ohledu na to upřednostňovaný jazyk nebo platformu. Hello informace v tomto článku je navrženou toohelp pochopit scénáře vysoké úrovně před [zahájení práce s kódem hello](active-directory-appmodel-v2-overview.md#getting-started).

> [!NOTE]
> koncový bod v2.0 Hello nepodporuje všechny scénáře Azure Active Directory a funkce. toodetermine zda byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).
> 
> 

## <a name="hello-basics"></a>Základy Hello
Je nutné zaregistrovat každou aplikaci, která používá koncového bodu v2.0 hello v hello [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com). Hello proces registrace aplikace shromáždí a přiřadí tyto hodnoty pro svou aplikaci:

* **ID aplikace** jednoznačně identifikuje vaši aplikaci
* A **identifikátor URI pro přesměrování** , které můžete použít toodirect odpovědí zpět tooyour aplikace
* Pár dalších hodnot konkrétní scénáře

Podrobnosti najdete další informace jak příliš[zaregistrovat aplikaci](active-directory-v2-app-registration.md).

Po registraci aplikace hello hello aplikace komunikuje se službou Azure AD zasláním požadavků koncového bodu v2.0 toohello Azure AD. Poskytujeme open-source architektury a knihovny, které zpracovávají hello podrobnosti o tyto požadavky. Můžete si taky logika ověřování hello možnost tooimplement hello sami vytvořením požadavky toothese koncové body:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```
<!-- TODO: Need a page for libraries toolink too-->

## <a name="web-apps"></a>Webové aplikace
Pro webové aplikace (.NET, PHP, Java, Ruby, Python, uzel) které hello uživatel přistupuje prostřednictvím prohlížeče, můžete použít [OpenID Connect](active-directory-v2-protocols.md) pro přihlášení uživatele. Hello webové aplikace v OpenID Connect, obdrží ID token. ID token je token zabezpečení, která ověřuje identitu uživatele hello a poskytuje informace o uživateli hello v podobě hello deklarací identity:

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

Informace o všech hello typy tokenů a deklaracích identity, které jsou k dispozici tooan aplikace v hello najdete [v2.0 tokeny odkaz](active-directory-v2-tokens.md).

V serveru webové aplikace trvá tok ověřování přihlášení hello těchto kroků:

![Tok ověřování webové aplikace](../../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

Identita uživatele hello můžete zajistit ověřením hello ID token pomocí veřejného podpisového klíče přijatého z koncového bodu v2.0 hello. Soubor cookie relace je nastavena, které lze použít tooidentify hello uživatelské požadavky na dalších stránkách.

toosee ukázky tento scénář v akci, zkuste jeden hello webové aplikace přihlášení kódu v našem v2.0 [Začínáme](active-directory-appmodel-v2-overview.md#getting-started) části.

Kromě toho toosimple přihlášení, může webová aplikace může být nutné tooaccess jiné webové služby, jako je například rozhraní REST API. V takovém případě webová aplikace hello zapojí v kombinovaném OpenID Connect a OAuth 2.0 toku pomocí hello [toku kódu autorizace OAuth 2.0](active-directory-v2-protocols.md). Další informace o tomto scénáři, přečtěte si informace o [Začínáme s webovými aplikacemi a webovým rozhraním API](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).

## <a name="web-apis"></a>Webová rozhraní API
Můžete použít hello v2.0 koncový bod toosecure webové služby, jako je vaše aplikace RESTful webového rozhraní API. Místo ID tokeny a soubory cookie relace, webového rozhraní API používá toosecure tokenu přístupu OAuth 2.0 jeho data a tooauthenticate příchozí požadavky. Hello volající webového rozhraní API připojí token přístupu v hello autorizační hlavičky požadavku HTTP, například takto:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Hello webového rozhraní API používá hello přístup tokenu tooverify hello volajícího identitu a tooextract informace o hello volajícím z deklarací identity zakódovaných v tokenu přístupu hello. toolearn o všechny hello typy tokenů a deklaracích identity, které jsou k dispozici tooan aplikace, najdete v části hello [v2.0 tokeny odkaz](active-directory-v2-tokens.md).

Webové rozhraní API můžete poskytnout uživatelům hello power tooopt nebo vyjádření výslovného nesouhlasu s konkrétní funkce nebo data zveřejněním oprávnění, také známé jako [obory](active-directory-v2-scopes.md). Pro volání aplikace tooacquire oprávnění tooa obor hello uživatel musí vyjádřit souhlas toohello oboru během k toku. koncový bod v2.0 Hello hello uživatel požádá o oprávnění a potom zaznamenává všechny přístupových tokenů oprávnění tohoto hello, které obdrží webového rozhraní API. Hello webového rozhraní API ověří hello přístupové tokeny přijetí při každém volání a provádí kontroly autorizace.

Webové rozhraní API může přijímat tokeny přístupu ze všech typů aplikací, včetně webových aplikací serveru, desktop a mobilní aplikace, jednostránkové aplikace, démonů na straně serveru a i další webovým rozhraním API. Hello podrobný postup pro webového rozhraní API vypadá takto:

![Tok ověření webového rozhraní API](../../media/active-directory-v2-flows/convergence_scenarios_webapi.png)

toolearn jak ukázky toosecure webového rozhraní API pomocí přístupových tokenů OAuth2, podívejte se na hello kódu webového rozhraní API v našem [Začínáme](active-directory-appmodel-v2-overview.md#getting-started) části.

Webové rozhraní API v mnoha případech také potřebovat toomake odchozí požadavky po proudu tooother webové rozhraní API pro zabezpečené službou Azure Active Directory.  toodo tedy webových rozhraní API můžete využít výhod Azure AD **na jménem z** tok, který umožňuje hello webového rozhraní API tooexchange příchozí přístupový token pro jiné toobe tokenu přístupu, které se používá v odchozí požadavky.  Hello v2.0 pro koncový bod jménem postup je popsán v [podrobností zde](active-directory-v2-protocols-oauth-on-behalf-of.md).

## <a name="mobile-and-native-apps"></a>Mobilní a nativní aplikace
Zařízení nainstalované aplikace, jako je například mobilních a desktopových aplikací, často potřebují tooaccess back endovým službám nebo webovým rozhraním API, která ukládají data a provádět operace jménem uživatele. Tyto aplikace můžete přidat přihlašování a autorizaci tooback endové služby pomocí hello [toku kódu autorizace OAuth 2.0](active-directory-v2-protocols-oauth-code.md).

V tomto toku aplikace hello obdrží autorizační kód z koncového bodu v2.0 hello při přihlášení uživatele hello. Hello autorizační kód představuje hello aplikace oprávnění toocall back endové služby jménem uživatele hello, kdo je přihlášený. aplikace Hello může provést výměnu hello autorizační kód hello pozadí pro přístupový token OAuth 2.0 a obnovovací token. aplikace Hello můžete využít hello přístup tokenu tooauthenticate tooWeb rozhraní API v požadavcích HTTP a pomocí hello aktualizace tokenu tooget nové přístupové tokeny, když vyprší platnost starší přístupových tokenů.

![Tok ověřování nativní aplikace](../../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="single-page-apps-javascript"></a>Jednostránkové aplikace (JavaScript)
Řada moderních aplikací využívá jednostránkový front-end, který je primárně napsané v jazyce JavaScript. Často je zapsán pomocí rozhraní jako AngularJS, Ember.js nebo Durandal.js. koncový bod v2.0 Hello Azure AD podporuje tyto aplikace pomocí hello [implicitního toku OAuth 2.0](active-directory-v2-protocols-implicit.md).

V tomto toku aplikace hello obdrží tokeny přímo z hello v2.0 zajistí autorizaci koncového bodu, bez jakékoli výměnu serveru na server. Všechny logiku ověřování a relace trvá zpracování umístit zcela v hello JavaScript klienta, a to bez přesměrování další stránky.

![Tok implicitní ověřování](../../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

toosee tento scénář v akci, vyzkoušejte některý z hello jednostránkové aplikace ukázky kódu v našem [Začínáme](active-directory-appmodel-v2-overview.md#getting-started) části.

## <a name="daemons-and-server-side-apps"></a>Démoni a serverové aplikace
Aplikace, které mají dlouho běžící procesy nebo které pracují bez interakce s uživatelem také potřebovat způsob, jakým tooaccess zabezpečeným prostředkům, jako jsou webová rozhraní API. Tyto aplikace se můžou ověřovat a získat tokeny pomocí identity aplikace hello namísto uživatele delegované identity s tok přihlašovacích údajů klienta hello OAuth 2.0.

V tomto toku aplikace hello komunikuje přímo s hello `/token` koncové body tooobtain koncový bod:

![Démon tok ověřování aplikace](../../media/active-directory-v2-flows/convergence_scenarios_daemon.png)

toobuild démon aplikace, najdete v dokumentaci pověření klienta hello v našem [Začínáme](active-directory-appmodel-v2-overview.md#getting-started) část, nebo se pokuste [ukázkové aplikace .NET](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).
