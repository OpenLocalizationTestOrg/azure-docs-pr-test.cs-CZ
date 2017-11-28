---
title: "aaaLearn o hello autorizace protokoly podporované službou Azure AD v2.0 | Microsoft Docs"
description: "Průvodce tooprotocols, který nepodporuje koncového bodu v2.0 hello Azure AD."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 5fb4fa1b-8fc4-438e-b3b0-258d8c145f22
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: f90511b1880ff223f725c1f79df9f79bddccc7bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# v2.0 protokoly - OAuth 2.0 & OpenID Connect
koncový bod v2.0 Hello můžete použít Azure AD pro identity jako služba pomocí standardních protokolů, OpenID Connect a OAuth 2.0.  Sice standardům hello service, může být drobné rozdíly mezi jakékoli dvě implementace těchto protokolů.  Zde Hello informace budou užitečné, pokud zvolíte kódu pro toowrite přímo zasláním & zpracování požadavků HTTP nebo použití 3. stran otevřete Knihovna zdrojů, nikoli pomocí jednoho z našich opensourcové knihovny.
<!-- TODO: Need link toolibraries above -->

> [!NOTE]
> Ne všechny scénáře Azure Active Directory a funkce jsou podporovány koncového bodu v2.0 hello.  toodetermine Pokud byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).
>
>

## Hello základy
V téměř všechny toky OAuth a OpenID Connect existují čtyři strany účastnící se hello exchange:

![Role OAuth 2.0](../../media/active-directory-v2-flows/protocols_roles.png)

* Hello **serveru ověřování** je koncový bod v2.0 hello.  Je zodpovědná za zajištění identitu uživatele hello, udělení a odvolání přístupu tooresources a vystavování tokenů.  Je také označován jako zprostředkovatele identity hello – bezpečně zpracovává nic toodo s hello uživatele informace, jejich přístup a vztahy důvěryhodnosti hello mezi stranami v tok.
* Hello **vlastník prostředku** je obvykle hello koncového uživatele.  Je, že hello stranu, která vlastní hello dat a má hello power tooallow třetími stranami tooaccess dat, nebo na prostředek.
* Hello **klienta OAuth** je aplikaci identifikovanou pomocí jeho ID aplikace.  Je obvykle hello strany koncového uživatele hello komunikuje se službou, a požaduje tokeny ze serveru ověřování hello.  Hello klienta musí mít udělen, že oprávnění tooaccess hello prostředků vlastníkem prostředku hello.
* Hello **Server prostředků** je, kde se nachází hello prostředků nebo data.  Vztahy důvěryhodnosti hello serveru ověřování toosecurely ověřování a autorizaci hello klienta OAuth a používá tooensure access_tokens, který přístup tooa prostředku můžete udělit nosiče.

## Registrace aplikací
Každá aplikace používající koncového bodu v2.0 hello potřebovat toobe registrovat u [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) předtím, než může komunikovat pomocí účtu OAuth nebo OpenID Connect.  proces registrace aplikace Hello bude shromažďovat & přiřadit několik hodnot tooyour aplikace:

* **Id aplikace** jednoznačně identifikuje vaši aplikaci
* A **identifikátor URI pro přesměrování** nebo **identifikátor balíčku** který lze použít toodirect odpovědí zpět tooyour aplikace
* Pár dalších hodnot konkrétní scénáře.

Další podrobnosti získáte další informace jak příliš[zaregistrovat aplikaci](active-directory-v2-app-registration.md).

## Koncové body
Po registraci aplikace hello komunikuje s Azure AD odesláním koncového bodu v2.0 toohello požadavky:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

Kde hello `{tenant}` může trvat jednu ze čtyř různých hodnot:

| Hodnota | Popis |
| --- | --- |
| `common` |Umožňuje uživatelům s osobní účty Microsoft i pracovní nebo školní účty z Azure Active Directory toosign do aplikace hello. |
| `organizations` |Umožňuje pouze uživatelé s pracovní nebo školní účty z Azure Active Directory toosign do aplikace hello. |
| `consumers` |Umožňuje pouze uživatelé s osobní účty (MSA) toosign Microsoft do aplikace hello. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` nebo `contoso.onmicrosoft.com` |Umožňuje pouze uživatelé s pracovní nebo školní účty z konkrétní toosign klienta Azure Active Directory do aplikace hello.  Buď hello domény popisný název hello klienta Azure AD, nebo můžete použít identifikátor guid klienta hello. |

Další informace o tom, toointeract s těmito koncovými body, zvolte jeden z následujících typů konkrétní aplikace.

## Tokeny
implementace v2.0 Hello OAuth 2.0 a OpenID Connect využívají rozsáhlé nosné tokeny, včetně nosné tokeny vyjádřené tokeny Jwt. Token lightweight zabezpečení, že uděluje hello "nosiče" přístup tooa chráněný prostředek, je token nosiče. V tomto smyslu je hello "nosiče" libovolné strany, který může být hello token. I když může strana musí nejprve ověřit pomocí služby Azure AD tooreceive hello nosný token, zda text hello požadované kroky nejsou přijata toosecure hello token v přenos a ukládání, může být zachyceny a použity nezamýšleným strana. I když některé tokeny zabezpečení má integrovanou mechanismus, který brání neoprávněným stranám jejich používání, nosné tokeny nemají tento mechanismus a musí být přenosu v zabezpečený kanál, jako je například transport layer security (HTTPS). Pokud token nosiče je přenesen v hello jasné, střední útok man-in hello mohou být využívána token škodlivý strany tooacquire hello a použít jej pro prostředek tooa chráněné neoprávněného přístupu. Hello stejné zabezpečení zásady platí při ukládání nebo ukládání do mezipaměti nosné tokeny pro pozdější použití. Vždy zajistěte, aby vaše aplikace odesílá a ukládá nosné tokeny zabezpečeným způsobem. Další aspekty zabezpečení na nosné tokeny, najdete v části [RFC 6750 část 5](http://tools.ietf.org/html/rfc6750).

Další podrobnosti o různých typů tokeny, které jsou používány koncového bodu v2.0 hello je k dispozici v [hello odkaz tokenu koncový bod v2.0](active-directory-v2-tokens.md).

## Protokoly
Pokud jste připravené toosee požádá o několik příkladů, začít pracovat s jedním z hello níže kurzy.  Každé z nich odpovídá tooa konkrétní ověřovacím scénáři.  Pokud potřebujete pomoc při rozhodování, což je hello správné toku za vás, podívejte se na [hello typy aplikací můžete vytvořit s hello v2.0](active-directory-v2-flows.md).

* [Vytvoření mobilní a nativní aplikace s OAuth 2.0](active-directory-v2-protocols-oauth-code.md)
* [Vytvoření webové aplikace s Open ID Connect](active-directory-v2-protocols-oidc.md)
* [Vývoj aplikací pro jednu stránku s hello implicitního toku OAuth 2.0](active-directory-v2-protocols-implicit.md)
* [Sestavení démoni nebo serverové straně procesy s hello toku OAuth 2.0 klienta přihlašovací údaje](active-directory-v2-protocols-oauth-client-creds.md)
* [Získat tokeny v rozhraní Web API s hello tok On jménem Of OAuth 2.0](active-directory-v2-protocols-oauth-on-behalf-of.md)
