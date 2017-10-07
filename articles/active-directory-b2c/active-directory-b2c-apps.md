---
title: aaaAzure AD B2C | Microsoft Docs
description: "Hello typy aplikací, které můžete sestavit v Azure Active Directory B2C hello."
services: active-directory-b2c
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: bb9d4abe-0db7-4bd9-b0c4-2f43b2c9cf33
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/06/2016
ms.author: dastrock
ms.openlocfilehash: 7dd3dac781fb7e1553dd0f2d112b1956489a7dfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-types-of-applications"></a>Azure Active Directory B2C: Typy aplikací
Azure Active Directory (Azure AD) B2C podporuje ověřování pro celou řadu architektur moderních aplikací. Všechny z nich jsou založeny na standardních oborových protokolech hello [OAuth 2.0](active-directory-b2c-reference-protocols.md) nebo [OpenID Connect](active-directory-b2c-reference-protocols.md). Tento dokument stručně popisuje hello typy aplikací, které můžete sestavit, nezávisle na hello jazyk nebo platformu dáváte přednost. Také pomáhá pochopit scénáře vysoké úrovně hello před [začnete sestavovat aplikace](active-directory-b2c-overview.md#get-started).

## <a name="hello-basics"></a>Základy Hello
Každá aplikace používající Azure AD B2C musí být zaregistrovaný ve vaší [adresáři B2C](active-directory-b2c-get-started.md) prostřednictvím hello [portálu Azure](https://portal.azure.com/). Hello proces registrace aplikace shromáždí a přiřadí aplikace tooyour několik hodnot:

* **ID aplikace**, které jednoznačně identifikuje vaši aplikaci.
* A **identifikátor URI pro přesměrování** který lze použít toodirect odpovědí zpět tooyour aplikace.
* Další hodnoty v závislosti na scénáři. Další podrobnosti získáte další informace jak příliš[zaregistrovat aplikaci](active-directory-b2c-app-registration.md).

Po registraci aplikace hello komunikuje se službou Azure AD pomocí odesílání žádosti koncového bodu v2.0 toohello Azure AD:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Každý požadavek zaslaný tooAzure AD B2C Určuje **zásad**. Zásady řídí chování hello Azure AD. Můžete také použít tyto koncové body toocreate vysoce přizpůsobitelnou sadu činností koncového uživatele. Mezi běžné zásady patří zásady registrace, přihlášení a úpravy profilu. Pokud si nejste obeznámeni se zásadami, měli byste si přečíst o hello Azure AD B2C [rozšiřitelném rozhraní zásad](active-directory-b2c-reference-policies.md) než budete pokračovat.

Hello interakce každé aplikace s koncovým bodem v2.0 postupuje podle podobného vzoru:

1. aplikace Hello přesměruje hello uživatele toohello v2.0 koncový bod tooexecute [zásad](active-directory-b2c-reference-policies.md).
2. Hello uživatel vykoná zásadu hello podle definice zásady toohello.
3. Hello aplikace obdrží z koncového bodu hello v2.0 nějaký druh tokenu zabezpečení.
4. aplikace Hello používá informace o tokenu tooaccess chráněné hello zabezpečení nebo chráněného prostředku.
5. server prostředků Hello ověří zabezpečení hello tokenu se tooverify, který lze udělit přístup.
6. aplikace Hello se pravidelně aktualizuje token zabezpečení hello.

<!-- TODO: Need a page for libraries toolink too-->
Tyto kroky můžou mírně lišit v závislosti na typu aplikace, kterou sestavujete hello. Opensourcové knihovny můžete vyřešit hello podrobnosti pro vás.

## <a name="web-apps"></a>Webové aplikace
U webových aplikací (včetně .NET, PHP, Javy, Ruby, Pythonu a Node.js) hostovaných na serveru a přístupných prostřednictvím prohlížeče Azure AD B2C podporuje [OpenID Connect](active-directory-b2c-reference-protocols.md) pro všechny činnosti koncového uživatele. To zahrnuje přihlášení, registraci a správu profilů. V implementaci OpenID Connect hello Azure AD B2C zahajuje webová aplikace tyto činnosti koncového uživatele zasláním požadavků tooAzure ověřování AD. je Hello výsledek požadavku hello `id_token`. Tento token zabezpečení představuje identitu uživatele hello. Nabízí taky informace o uživateli hello v podobě hello deklarací identity:

```
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

Další informace o typech tokenů a deklaracích identity aplikace k dispozici tooan v hello hello [odkaz tokenu B2C](active-directory-b2c-reference-tokens.md).

Ve webové aplikaci se každé spuštění [zásady](active-directory-b2c-reference-policies.md) skládá z těchto kroků:

![Obrázek plaveckých drah webové aplikace](./media/active-directory-b2c-apps/webapp.png)

Ověření hello `id_token` pomocí veřejného podpisového klíče přijatého z Azure AD je dostatečná tooverify hello identitu uživatele hello. Zároveň se nastaví soubor cookie relace, kterou lze použít tooidentify hello uživatelské požadavky na dalších stránkách.

toosee tento scénář v akci, vyzkoušejte některý z hello webové aplikace přihlašovací kód ukázky v našem [oddílu Začínáme](active-directory-b2c-overview.md#get-started).

V přidání toofacilitating jednoduchého přihlášení může webová aplikace může být také nutné tooaccess back endové webové službě. V takovém případě hello webová aplikace provádět mírně odlišný [tok OpenID Connect](active-directory-b2c-reference-oidc.md) a získávat tokeny pomocí autorizačních kódů a obnovovacích tokenů. Tento scénář je znázorněn v následující hello [oddílu webová rozhraní API](#web-apis).

<!--, and in our [WebApp-WebAPI Getting started topic](active-directory-b2c-devquickstarts-web-api-dotnet.md).-->

## <a name="web-apis"></a>Webová rozhraní API
Můžete použít Azure AD B2C toosecure webové služby jako jsou vaše RESTful webová rozhraní API. Webové rozhraní API můžete použít OAuth 2.0 toosecure svá data tak, že příchozí požadavky HTTP pomocí tokenů. Hello volající webového rozhraní API připojí token v hello autorizační hlavičky požadavku HTTP:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Hello webového rozhraní API pak můžete použít hello tokenu tooverify hello API volajícího identitu a tooextract informace o hello volajícím z deklarací identity zakódovaných v tokenu hello. Další informace o typech tokenů a deklaracích identity aplikace k dispozici tooan v hello hello [odkaz tokenu Azure AD B2C](active-directory-b2c-reference-tokens.md).

> [!NOTE]
> Azure AD B2C v současné době podporuje pouze webová rozhraní API, ke kterým přistupují vlastní, dobře známí klienti. Kompletní aplikace může zahrnovat aplikaci pro iOS, aplikaci pro Android a back-end v podobě webové rozhraní API. Tato architektura je plně podporovaná. Povolení partnerského klienta, například další aplikace pro iOS, tooaccess hello stejnému webovému rozhraní API není aktuálně podporována. Všechny součásti hello dokončení aplikace musejí sdílet jednu aplikaci ID.
>
>

Webové rozhraní API může přijímat tokeny z řady typů klientů, včetně webových aplikací, počítačových a mobilních aplikací, jednostránkových aplikací, démonů na straně serveru a dalších webových rozhraní API. Tady je příklad celého toku hello pro webovou aplikaci, která volá webové rozhraní API:

![Obrázek plaveckých drah webového rozhraní API webové aplikace](./media/active-directory-b2c-apps/webapi.png)

toolearn Další informace o kódech autorizace, tokeny obnovení a hello návod, jak získat tokeny, přečtěte si informace o hello [protokol OAuth 2.0](active-directory-b2c-reference-oauth-code.md).

toolearn jak toosecure webového rozhraní API pomocí Azure AD B2C, podívejte se na hello webové rozhraní API kurzy v našem [oddílu Začínáme](active-directory-b2c-overview.md#get-started).

## <a name="mobile-and-native-apps"></a>Mobilní a nativní aplikace
Aplikace, které jsou nainstalovány na zařízeních, jako je například mobilních a desktopových aplikací, často potřebují tooaccess back endovým službám nebo webovým rozhraním API jménem uživatele. Můžete přidat vlastní identity management prostředí tooyour nativní aplikace a bezpečně volat back endové služby pomocí Azure AD B2C a hello [toku kódu autorizace OAuth 2.0](active-directory-b2c-reference-oauth-code.md).  

V tomto toku aplikace hello spouští [zásady](active-directory-b2c-reference-policies.md) a přijímá `authorization_code` z Azure AD po hello uživatel vykoná zásadu hello. Hello `authorization_code` představuje hello aplikace oprávnění toocall back endové služby jménem hello uživatele, který je aktuálně přihlášený. aplikace Hello můžete pak exchange hello `authorization_code` hello pozadí pro `id_token` a `refresh_token`.  Hello aplikace můžete používat hello `id_token` tooauthenticate tooa back endové webové rozhraní API v požadavcích HTTP. Můžete také hello `refresh_token` tooget a nové `id_token` vypršení platnosti starší.

> [!NOTE]
> Azure AD B2C aktuálně podporuje pouze tokeny, které jsou používané tooaccess služby back endové webové aplikace je vlastní. Kompletní aplikace může zahrnovat aplikaci pro iOS, aplikaci pro Android a back-end v podobě webové rozhraní API. Tato architektura je plně podporovaná. Povolení vaší tooaccess aplikaci iOS partnerskému webovému rozhraní API pomocí přístupových tokenů OAuth 2.0 není aktuálně podporován. Všechny součásti hello dokončení aplikace musejí sdílet jednu aplikaci ID.
>
>

![Obrázek plaveckých drah nativní aplikace](./media/active-directory-b2c-apps/native.png)

## <a name="current-limitations"></a>Aktuální omezení
Azure AD B2C aktuálně nepodporuje hello následující typy aplikací, ale jsou v hello plán. 

### <a name="daemonsserver-side-apps"></a>Démoni nebo serverové aplikace
Aplikace, které obsahují dlouho běžící procesy nebo které pracují bez přítomnosti hello uživatele také potřebují způsob, který tooaccess zabezpečeným prostředkům, například webových rozhraní API. Tyto aplikace můžete ověřit a získat tokeny pomocí identity aplikace hello (nikoli uživatelovy delegované identity) a pomocí toku přihlašovacích údajů klienta hello OAuth 2.0.

Azure AD B2C v současné době nepodporuje tento tok. Tyto aplikace mohou získat tokeny pouze poté, co došlo k interaktivnímu uživatelskému toku.

### <a name="web-api-chains-on-behalf-of-flow"></a>Řetězení webových rozhraní API (tok on-behalf-of)
Mnoho architektur zahrnuje webové rozhraní API, které potřebuje toocall jiné podřízené webové rozhraní API, přičemž obě jsou zabezpečené pomocí Azure AD B2C. Tento scénář je častý u nativních klientů s back-endem v podobě webového rozhraní API. To poté zavolá online službu Microsoftu, jako je například hello Azure AD Graph API.

Tento scénář zřetězených webových rozhraní API může být podporován pomocí udělení přihlašovacích údajů nosiče OAuth 2.0 JWT hello, také známé jako hello on-behalf-of toku.  Hello tok on-behalf-of však není aktuálně implementována v hello Azure AD B2C.
