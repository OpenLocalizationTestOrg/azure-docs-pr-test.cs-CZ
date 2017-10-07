---
title: "aaaAzure služby Active Directory omezení a omezení koncového bodu v2.0 | Microsoft Docs"
description: "Seznam omezení a limity pro koncový bod v2.0 hello Azure AD."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: a99289c0-e6ce-410c-94f6-c279387b4f66
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: bcbb7239f1d117002d16ac23dca8f1feb13a9161
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="should-i-use-hello-v20-endpoint"></a>Použít koncového bodu v2.0 hello?
Při vytváření aplikací, které se integrují s Azure Active Directory, je nutné toodecide, zda protokoly pro koncový bod a ověřování v2.0 hello podle svých potřeb. Původní koncový bod Azure služby Active Directory je stále plně podporovaná a v některých ohledech je další bohaté funkce než v2.0. Ale hello koncového bodu v2.0 [představuje významné výhody](active-directory-v2-compare.md) pro vývojáře.

Tady je naše zjednodušené doporučení pro vývojáře v tuto chvíli:

* Jestliže musíte podporovat osobní účty Microsoft v aplikaci, použijte koncového bodu v2.0 hello. Ale předtím, než provedete, ujistěte se, že rozumíte hello omezení, které v tomto článku probereme.
* Pokud aplikace potřebuje pouze toosupport Microsoft pracovní a školní účty, nepoužívejte koncového bodu v2.0 hello. Místo toho najdete tooour [Příručka pro vývojáře Azure AD](active-directory-developers-guide.md).

V průběhu času koncového bodu v2.0 hello poroste tooeliminate hello omezení zde uvedeny, tak, aby vždy jen potřebujete koncového bodu v2.0 toouse hello. V hello té doby, tento článek je určený toohelp můžete určit, zda je pro vás nejvhodnější koncového bodu v2.0 hello. Bude se pokračovat tooupdate Tento článek tooreflect hello aktuální stav koncového bodu v2.0 hello. Zkontrolujte back tooreevaluate požadavků na možnosti v2.0.

Pokud máte existující aplikaci Azure AD, která nepoužívá koncového bodu v2.0 hello, neexistuje žádný toostart nutné od začátku. V budoucích hello poskytujeme způsob, jak pro toouse můžete existující aplikace Azure AD s koncovým bodem v2.0 hello.

## <a name="restrictions-on-app-types"></a>Omezení typů aplikací
V současné době hello následující typy aplikací nepodporuje koncového bodu v2.0 hello. Popis typů podporovaných aplikací najdete v tématu [typy aplikací pro koncový bod v2.0 Azure Active Directory hello](active-directory-v2-flows.md).

### <a name="standalone-web-apis"></a>Samostatné webové rozhraní API
Můžete použít koncového bodu v2.0 hello příliš[sestavit Web API, která je zabezpečená pomocí OAuth 2.0](active-directory-v2-flows.md#web-apis). Ale že webové rozhraní API může přijímat tokeny pouze z aplikace, která má hello stejné ID aplikace Nelze získat přístup webového rozhraní API z klienta, který má jiné ID aplikace Hello klienta nebude se moct toorequest nebo získat oprávnění tooyour webového rozhraní API.

jak toobuild webového rozhraní API, který přijímá tokeny z klienta, který má hello stejným ID aplikace, najdete v části toosee hello ukázky webového rozhraní API koncový bod v2.0 v našem [Začínáme](active-directory-appmodel-v2-overview.md#getting-started) části.

## <a name="restrictions-on-app-registrations"></a>Omezení při registraci aplikace
V současné době pro každou aplikaci, které chcete toointegrate s koncovým bodem v2.0 hello, je nutné vytvořit registraci aplikace ve hello nové [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). Existující služby Azure AD, nebo nejsou kompatibilní s koncovým bodem v2.0 hello aplikace účtu Microsoft. Aplikace, které jsou registrovány libovolnému portálu než hello portálu pro registraci aplikace nejsou kompatibilní s koncovým bodem v2.0 hello. V budoucích hello plánujeme tooprovide způsob toouse stávající aplikace jako v2.0 aplikaci. V současné době ale neexistuje žádná cesta migrace pro existující aplikaci toowork s koncovým bodem v2.0 hello.

Kromě toho registrace aplikace, které vytvoříte v hello [portálu pro registraci aplikace](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) mít hello následující upozornění:

* Jsou povoleny pouze dva tajné klíče aplikace na ID aplikace.
* Registraci aplikace registrovaný uživatel s osobní účet Microsoft můžete zobrazit a spravovat pouze podle jednoho vývojářského účtu. Nesmí se sdílet mezi více vývojářů.  Pokud chcete tooshare registrace aplikací mezi více vývojářů, můžete vytvořit aplikaci hello po přihlášení k portálu pro registraci hello pomocí účtu Azure AD.
* Existuje několik omezení hello formátu hello přesměrování identifikátor URI, který je povolen. Další informace o přesměrování identifikátory URI najdete v další části hello.

## <a name="restrictions-on-redirect-uris"></a>Omezení přesměrování identifikátory URI
V současné době jsou aplikace, které jsou registrovány hello portálu pro registraci aplikace s omezeným přístupem tooa omezenou sadu hodnot identifikátoru URI přesměrování. Hello přesměrování identifikátor URI pro webové aplikace a služby musí začínat řetězcem schématu hello `https`, a všechny hodnoty identifikátoru URI přesměrování musí sdílí jedinou doménu DNS. Například nelze zaregistrovat webové aplikace, který obsahuje jedno z těchto přesměrování identifikátory URI:

`https://login-east.contoso.com`  
`https://login-west.contoso.com`

systém registrace Hello porovná hello celý název DNS hello existující přesměrování URI toohello název DNS hello přesměrování identifikátor URI, který chcete přidat. název DNS Hello požadavek tooadd hello se nezdaří, pokud je splněna jedna z hello následující podmínky:  

* Hello celý název DNS nový identifikátor URI přesměrování hello neodpovídá názvu DNS hello existující identifikátor URI přesměrování hello.
* Hello celý název DNS nový identifikátor URI přesměrování hello není subdoména existující identifikátor URI přesměrování hello.

Například, pokud má tento identifikátor URI pro přesměrování aplikace hello:

`https://login.contoso.com`

Můžete přidat tooit takto:

`https://login.contoso.com/new`

V tomto případě název DNS hello přesně odpovídá. Nebo můžete provést toto:

`https://new.login.contoso.com`

V takovém případě odkazujete tooa DNS subdoménou login.contoso.com. Pokud chcete toohave aplikaci, která má přihlášení east.contoso.com a west.contoso.com přihlášení jako identifikátory URI přesměrování, je nutné přidat že ty přesměrování identifikátory URI v tomto pořadí:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

Můžete přidat hello pozdější dva vzhledem k tomu, že jsou subdomény hello nejprve identifikátor URI pro přesměrování, contoso.com. Toto omezení se odeberou v nadcházející verzi.

jak zjistit, tooregister na aplikace v portálu pro registraci aplikace hello toolearn [jak tooregister aplikace s koncovým bodem v2.0 hello](active-directory-v2-app-registration.md).

## <a name="restrictions-on-services-and-apis"></a>Omezení služeb a rozhraní API
V současné době podporuje koncového bodu v2.0 hello přihlášení pro všechny aplikace, který je zaregistrován v hello portálu pro registraci aplikace a který spadá v seznamu hello [podporované ověřování toky](active-directory-v2-flows.md). Tyto aplikace však můžete získat přístupových tokenů OAuth 2.0 pro velmi omezená sada prostředků. problémy koncový bod v2.0 Hello přístup jenom pro tokeny:

* Hello aplikaci, která požadovaný hello token. Aplikaci můžete získat přístupový token pro sebe, pokud logickou aplikaci hello se skládá z několika různých komponent nebo vrstev. toosee tento scénář v akci, podívejte se na naše [Začínáme](active-directory-appmodel-v2-overview.md#getting-started) kurzy.
* Hello Outlook pošta, kalendář a kontakty REST API, které jsou umístěné na https://outlook.office.com. toolearn jak toowrite aplikaci, která přistupuje k tato rozhraní API, najdete v části hello [Office Začínáme](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) kurzy.
* Microsoft Graph API. Další informace o [Microsoft Graph](https://graph.microsoft.io) a hello data, která je k dispozici tooyou.

Žádné jiné služby jsou nyní podporovány. Další Microsoft Online Services bude přidán v hello budoucí, kromě toosupport vlastní uživatelské rozhraní Web API a služby.

## <a name="restrictions-on-libraries-and-sdks"></a>Omezení knihovny a sady SDK
Podpora knihovny pro koncový bod v2.0 hello je v současné době omezené. Pokud chcete koncového bodu v2.0 hello toouse v produkční aplikace, máte tyto možnosti:

* Pokud vytváříte webovou aplikaci, můžete bezpečně Microsoft všeobecně dostupná middleware serverové tooperform přihlášení a token ověření. Mezi ně patří middleware OWIN Open ID Connect hello ASP.NET a hello Node.js Passport modulu plug-in. Ukázky kódu, které používají Microsoft middleware, najdete v tématu naše [Začínáme](active-directory-appmodel-v2-overview.md#getting-started) části.
* Pokud vytváříte aplikace desktop či mobile, můžete použít jeden z našich preview Microsoft ověřování knihovny (MSAL).  Tyto knihovny jsou v produkční podporované náhledu, tak, aby byl bezpečné toouse je v produkční aplikace. Můžete si přečíst další informace o hello podmínky hello náhled a hello dostupných knihoven v našem [referenční dokumentace knihoven ověřování](active-directory-v2-libraries.md).
* Pro platformy, které nejsou pokryty knihovny Microsoft můžete integrovat s koncovým bodem v2.0 hello přímo přijatých a odeslaných zpráv protokolu v kódu aplikace. Hello protokoly OpenID Connect a OAuth v2.0 [explicitně popsané](active-directory-v2-protocols.md) toohelp provádět takové integrační.
* Nakonec můžete použít open-source otevřete ID Connect a OAuth knihovny toointegrate s koncovým bodem v2.0 hello. Hello v2.0 protokol musí být kompatibilní s mnoha knihovny open-source protokolu bez hlavní změny. dostupnost Hello tohoto typu knihovny se liší podle jazyka a platformu. Hello [Open ID Connect](http://openid.net/connect/) a [OAuth 2.0](http://oauth.net/2/) weby udržovat seznam oblíbených implementace. Další informace najdete v tématu [v2.0 a ověřování knihovny Azure Active Directory](active-directory-v2-libraries.md)a seznam hello open-source klientské knihovny a ukázky, které byly testovány s koncovým bodem v2.0 hello.

## <a name="restrictions-on-protocols"></a>Omezení pro protokoly
koncový bod v2.0 Hello nepodporuje SAML nebo WS-Federation; podporuje pouze Open ID Connect a OAuth 2.0.  Ne všechny funkce a možnosti OAuth protokolů byly zahrnuty do koncového bodu v2.0 hello. Aktuálně jsou tyto funkce protokolu a možnosti *není k dispozici* v koncového bodu v2.0 hello:

* ID tokeny, které jsou vydány koncového bodu v2.0 hello neobsahují `email` deklarace pro uživatele hello i v případě, že získat oprávnění z hello uživatele tooview e-mailu.
* koncový bod Hello OpenID Connect uživateli není implementována pro koncový bod v2.0 hello. Všechna data profilu, které se by potenciálně zobrazit na tento koncový bod je však k dispozici z hello Microsoft Graph `/me` koncový bod.
* koncového bodu v2.0 Hello nepodporuje vystavující role nebo skupiny deklarací identity v tokenech ID.
* Hello [udělení pověření heslo vlastníka prostředku OAuth 2.0](https://tools.ietf.org/html/rfc6749#section-4.3) nepodporuje koncového bodu v2.0 hello.

V addtion nepodporuje koncového bodu v2.0 hello jakoukoli formu hello SAML nebo protokoly WS-Federation.

toobetter pochopit hello oboru protokol funkcí podporovaných v koncového bodu v2.0 hello, přečíst naše [referenční informace o protokolu OpenID Connect a OAuth 2.0](active-directory-v2-protocols.md).

## <a name="restrictions-for-work-and-school-accounts"></a>Omezení pro pracovní a školní účty
Pokud jste použili Active Directory Authentication Library (ADAL) v aplikacích Windows, může provedli výhod integrované ověřování Windows, který používá grant assertion hello Security (Assertion Markup Language SAML). S Tento grant federované uživatele Azure AD klienty můžete bezobslužně ověřit pomocí jejich místní instanci Active Directory bez nutnosti zadávat přihlašovací údaje. Udělení kontrolního výrazu SAML hello v současné době nepodporuje koncového bodu v2.0 hello.
