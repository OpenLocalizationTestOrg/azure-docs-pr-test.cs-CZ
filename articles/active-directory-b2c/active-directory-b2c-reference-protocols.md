---
title: "Azure Active Directory B2C: Protokoly pro ověřování | Microsoft Docs"
description: "Jak aplikace toobuild přímo pomocí hello protokoly, které podporuje Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 5e407d0a-73a2-4d74-ac81-3aa6c31ddcee
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.openlocfilehash: 8fa4cbebe711841d410b3ae43b78f893c06d9b63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# Azure AD B2C: Protokoly pro ověřování
Azure Active Directory B2C (Azure AD B2C) poskytuje identitu jako služba pro aplikací díky podpoře dvou standardních protokolů: OpenID Connect a OAuth 2.0. Služba Hello je kompatibilní se standardy, ale žádné dvě implementace těchto protokolů můžete mít jemně lišit. 

Hello informace v této příručce je užitečné, pokud napíšete kód přímo odeslání a zpracování žádostí HTTP, nikoli pomocí knihovny otevřeným zdrojem. Doporučujeme, abyste si přečetli tuto stránku před podrobně hello podrobnosti o každém konkrétní protokolu. Ale pokud jste již obeznámeni s Azure AD B2C, můžete přejít rovnou příliš[hello protokol referenční příručky](#protocols).

<!-- TODO: Need link toolibraries above -->

## Základy Hello
Každá aplikace používající Azure AD B2C musí toobe registrované ve svém adresáři B2C v hello [portál Azure](https://portal.azure.com). Hello proces registrace aplikace shromáždí a přiřadí aplikace tooyour několik hodnot:

* **ID aplikace**, které jednoznačně identifikuje vaši aplikaci.
* A **identifikátor URI pro přesměrování** nebo **balíček identifikátor** který lze použít toodirect odpovědí zpět tooyour aplikace.
* Pár dalších hodnot konkrétní scénáře. Další informace najdete další [jak tooregister aplikace](active-directory-b2c-app-registration.md).

Po registraci aplikace komunikuje se službou Azure Active Directory (Azure AD) odesláním koncový bod toohello požadavky:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

V téměř všechny toky OAuth a OpenID Connect se součástí hello exchange čtyři strany:

![Role OAuth 2.0](./media/active-directory-b2c-reference-protocols/protocols_roles.png)

* Hello **serveru ověřování** je koncový bod hello Azure AD. Bezpečně zpracovává cokoli související toouser informace a přístup. Také obstará hello vztahy důvěryhodnosti mezi stranami hello v toku. Zodpovídá za ověření identity uživatele hello, udělení a odvolání přístupu tooresources a vystavování tokenů. Je také označované jako zprostředkovatele identity hello.

* Hello **vlastník prostředku** je obvykle hello koncového uživatele. Je hello stranu, která je vlastníkem dat hello a má hello power tooallow třetím stranám tooaccess tímto data nebo prostředků.

* Hello **klienta OAuth** je vaše aplikace. Je identifikována jeho ID aplikace. Je obvykle hello stranu, která koncovým uživatelům interakci s. Také vyžádá tokeny ze serveru ověřování hello. vlastník prostředku Hello musí udělit hello klienta oprávnění tooaccess hello prostředků.

* Hello **server prostředků** je, kde se nachází hello prostředků nebo data. Důvěřovat hello autorizace server toosecurely ověřování a autorizaci hello klienta OAuth. Používá také nosiče přístupu, kterou lze udělit tooensure tokeny, které přístup k prostředku tooa.

## Zásady
Pravděpodobně Azure AD B2C zásady jsou hello nejdůležitější funkce služby hello. Azure AD B2C rozšiřuje protokoly standardní OAuth 2.0 a OpenID Connect hello zavedením zásad. Tyto rutiny mnohem víc než jednoduché ověřování a autorizace umožňují tooperform Azure AD B2C. 

Zásady plně popisují činnosti identity uživatelů, včetně registrace, přihlášení a úpravy profilu. Zásady můžete být definován v správu uživatelského rozhraní. Mohou být provedeny s použitím parametru speciální dotazu v žádosti o ověření protokolu HTTP. 

Zásady nejsou standardní funkce OAuth 2.0 a OpenID Connect, proto byste měli vzít toounderstand čas hello je. Další informace najdete v tématu hello [Azure AD B2C zásad referenční příručka](active-directory-b2c-reference-policies.md).

## Tokeny
implementace Hello Azure AD B2C OAuth 2.0 a OpenID Connect díky rozsáhlé používání tokenů nosiče, včetně nosné tokeny, které jsou reprezentovány jako webové tokeny JSON (Jwt). Token lightweight zabezpečení, že uděluje hello "nosiče" přístup tooa chráněný prostředek, je token nosiče.

nosiče Hello je jakékoli strany, který může být hello token. Azure AD musí je nejdřív ověřit a stranou předtím, než mohl přijímat token nosiče. Ale pokud hello požadované kroky nezavedou toosecure hello token v přenosu a úložiště, můžete zachytit a použít nezamýšleným strana.

Některé tokeny zabezpečení mají integrovaný mechanismus, které brání neoprávněným stranám jejich používání, ale nosné tokeny nemají tento mechanismus. Musí být přenosu v zabezpečený kanál, jako je například transportní vrstvy zabezpečení (HTTPS). 

Pokud je token nosiče přenesen mimo zabezpečený kanál, škodlivý strany můžete token útok man-in-the-middle tooacquire hello a použít je toogain neoprávněného přístupu tooa chráněných prostředků. Hello stejné zásady zabezpečení platí, když jsou nosné tokeny uložené nebo ukládání do mezipaměti pro pozdější použití. Vždy zajistěte, aby vaše aplikace odesílá a ukládá nosné tokeny zabezpečeným způsobem.

Důležité informace o dalších nosiče tokenu zabezpečení, najdete v části [RFC 6750 část 5](http://tools.ietf.org/html/rfc6750).

Další informace o různých typech tokenů, které se používají v Azure AD B2C hello jsou k dispozici v [hello odkaz tokenu Azure AD](active-directory-b2c-reference-tokens.md).

## Protokoly
Až budete připraveni tooreview některé příklad požádá, můžete spustit některý z následujících kurzů hello. Každý odpovídá tooa konkrétní ověřovacím scénáři. Pokud potřebujete pomoc při rozhodování, které tok je pro vás správný, podívejte se na [hello typy aplikací můžete vytvořit pomocí Azure AD B2C](active-directory-b2c-apps.md).

* [Vytvářet aplikace, mobilní a nativní pomocí standardu OAuth 2.0](active-directory-b2c-reference-oauth-code.md)
* [Vytvoření webové aplikace pomocí OpenID Connect](active-directory-b2c-reference-oidc.md)
* [Vytvářet jednostránkové aplikace pomocí implicitního toku OAuth 2.0 hello](active-directory-b2c-reference-spa.md)

