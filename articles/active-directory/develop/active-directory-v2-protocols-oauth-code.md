---
title: "aaaAzure AD v2.0 toku kódu autorizace OAuth | Microsoft Docs"
description: "Vytváření webové aplikace pomocí Azure AD implementace ověřovacího protokolu hello OAuth 2.0."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: ae1d7d86-7098-468c-aa32-20df0a10ee3d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: dee58f2f5c627fef35cae279349728c3c7bf9421
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# Protokoly v2.0 - toku kódu autorizace OAuth 2.0
udělení autorizačního kódu OAuth 2.0 Hello lze použít v aplikacích, které jsou nainstalovány na zařízení toogain přístup tooprotected prostředky, například webových rozhraní API.  Pomocí implementace hello aplikace model v2.0 na OAuth 2.0, můžete přidat přihlásit a rozhraní API přístup tooyour mobilních a desktopových aplikací.  Tento průvodce je nezávislé na jazyku a popisuje, jak toosend a přijímat zprávy HTTP bez použití některé z našich knihovny open-source.

> [!NOTE]
> Ne všechny scénáře Azure Active Directory a funkce jsou podporovány koncového bodu v2.0 hello.  toodetermine Pokud byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).
> 
> 

Hello toku kódu autorizace OAuth 2.0 je popsáno v v [části 4.1 hello OAuth 2.0 specifikace](http://tools.ietf.org/html/rfc6749).  Je použité tooperform ověřování a autorizace ve hello většinu typů aplikací, včetně [webové aplikace](active-directory-v2-flows.md#web-apps) a [nativně nainstalované aplikace](active-directory-v2-flows.md#mobile-and-native-apps).  Umožňuje získat toosecurely access_tokens, který lze použít tooaccess prostředky, které jsou zabezpečené pomocí koncového bodu v2.0 hello aplikace.  

## Diagram protokolu
Na vysoké úrovni tok hello celý proces ověřování pro nativní nebo mobilní aplikace trochu vypadat třeba takto:

![Ověřování kódu toku OAuth](../../media/active-directory-v2-flows/convergence_scenarios_native.png)

## Žádost o autorizační kód
tok autorizačního kódu Hello začíná textem hello klienta odkazovat hello uživatele toohello `/authorize` koncový bod.  V této žádosti o hello klient určuje hello oprávnění potřebná tooacquire od hello uživatele:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345
```

> [!TIP]
> Kliknutím na odkaz hello níže tooexecute tuto žádost! Po přihlášení, by měl být prohlížeči přesměrován příliš`https://localhost/myapp/` s `code` v panelu Adresa hello.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345" target="_blank">https://Login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>
> 
> 

| Parametr |  | Popis |
| --- | --- | --- |
| Klienta |Požadované |Hello `{tenant}` hodnota v cestě hello hello žádosti může být použité toocontrol, který se můžete přihlásit do aplikace hello.  Hello povolené hodnoty jsou `common`, `organizations`, `consumers`a identifikátory klientů.  Další podrobnosti naleznete v [protokolu Základy](active-directory-v2-protocols.md#endpoints). |
| client_id |Požadované |Hello Id aplikace tohoto portálu registrace hello ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) přiřazené vaší aplikace. |
| response_type |Požadované |Musí zahrnovat `code` pro tok autorizačního kódu hello. |
| redirect_uri |Doporučená |Hello redirect_uri vaší aplikace, kde můžete odesílat a přijímat aplikace odpovědi ověřování.  Se musí přesně shodovat s jedním z hello redirect_uris, registrován v hello portál, s výjimkou musí být kódovaná adresou url.  Nativní & mobilní aplikace, měli byste použít výchozí hodnotu hello `https://login.microsoftonline.com/common/oauth2/nativeclient`. |
| Obor |Požadované |Seznam oddělených mezerami [obory](active-directory-v2-scopes.md) , které chcete tooconsent uživatele hello k. |
| response_mode |Doporučená |Určuje metodu hello, který by měl být použité toosend hello výsledný token back tooyour aplikace.  Může být `query` nebo `form_post`. |
| state |Doporučená |Hodnota součástí hello požadavek, který bude vrácen také v odpovědi tokenu hello.  Může být řetězec o délce veškerý obsah, který chcete.  Náhodně generované jedinečné hodnoty se obvykle používá u [prevence útoků padělání požadavku posílaného mezi weby](http://tools.ietf.org/html/rfc6749#section-10.12).  Stav Hello je také použít tooencode informace o stavu hello uživatele v aplikaci hello před došlo k požadavku hello ověřování, jako je například stránku hello nebo zobrazení, které byly na. |
| řádku |Volitelné |Označuje typ hello interakce s uživatelem, který je vyžadován.  Hello platné jsou pouze hodnoty v tuto chvíli "přihlášení", 'none' a 'souhlas'.  `prompt=login`bude platnost hello tooenter uživatele při jejich přihlašovacích údajů tohoto požadavku negace jednotné přihlašování na.  `prompt=none`je hello opačné – zajistí, že tento hello uživatel není uveden s žádné jakkoli interaktivní výzvu.  Pokud požadavek hello nemůže být dokončena bez upozornění pomocí jednotného přihlašování, koncového bodu v2.0 hello vrátí chybu.  `prompt=consent`bude aktivační událost hello OAuth souhlas dialogové okno po hello uživatel se přihlásí, dotazem hello uživatele toogrant oprávnění toohello aplikace. |
| login_hint |Volitelné |Můžou být použité toopre výplně hello uživatelského jména nebo e-mailovou adresu pole hello přihlásit stránku hello uživatele, pokud znáte svoje uživatelské jméno předem.  Často aplikace bude používat tento parametr během opětovné ověření, uživatelské jméno hello s z předchozí přihlášení už extrahovat pomocí hello `preferred_username` deklarací identity. |
| domain_hint |Volitelné |Může být jedna z `consumers` nebo `organizations`.  Pokud zahrnuty, přeskočí procesu zjišťování na základě e-mailu hello tento uživatel prochází na hello v2.0 stránku pro přihlášení, úvodní tooa něco víc zjednodušený činnost koncového uživatele.  Často aplikace bude používat tento parametr během opětovné ověření extrahováním hello `tid` z předchozí přihlášení.  Pokud hello `tid` deklarace, hodnota je `9188040d-6c67-4c5b-b112-36a304b66dad`, měli byste použít `domain_hint=consumers`.  Jinak použijte `domain_hint=organizations`. |

V tomto okamžiku hello uživatel se bude výzva tooenter své přihlašovací údaje a dokončení hello ověřování.  Hello koncového bodu v2.0 také zajistí, že hello uživatel souhlasí toohello oprávnění uvedené v hello `scope` parametr dotazu.  Pokud uživatel hello nedala souhlas tooany tato oprávnění, ho požádá uživatele tooconsent hello toohello vyžaduje oprávnění.  Podrobnosti o [oprávnění, souhlasu a víceklientské aplikace jsou tady uvedené](active-directory-v2-scopes.md).

Jakmile uživatel hello ověřuje a uděluje souhlas, koncového bodu v2.0 hello vrátí tooyour aplikace na odpovědi na hello uvedené `redirect_uri`, pomocí hello metody popsané v hello `response_mode` parametr.

#### Úspěšná odpověď
Úspěšná odpověď pomocí `response_mode=query` vypadá jako:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=12345
```

| Parametr | Popis |
| --- | --- |
| Kód |Hello authorization_code, který hello požadované aplikace. Hello aplikace můžete používat hello autorizační kód toorequest přístupový token hello cílový prostředek.  Authorization_codes jsou velmi krátkodobé povahy, obvykle vyprší po přibližně 10 minut. |
| state |Pokud parametr stavu je zahrnuta v žádosti o hello, hello stejnou hodnotu by se zobrazit v odpovědi hello. Hello aplikace by měla ověřte, zda jsou identické hodnoty stavu hello v hello žádostí a odpovědí. |

#### Chybové odpovědi
Chybové odpovědi se taky může odeslat toohello `redirect_uri` tak hello aplikace můžete správně zpracovat:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametr | Popis |
| --- | --- |
| error |Řetězec kódu chyby, který lze použít tooclassify typů chyb, ke kterým došlo a může být použité tooreact tooerrors. |
| error_description |Konkrétní chybová zpráva, která může pomoci vývojář identifikovat hello hlavní příčinu chyby ověřování. |

#### Kódy chyb pro chyb koncový bod autorizace
Hello následující tabulka popisuje hello různé kódy chyb, které mohou být vráceny v hello `error` parametr hello chybové odpovědi.

| Kód chyby | Popis | Akce klienta |
| --- | --- | --- |
| invalid_request |Chyba protokolu, například chybějící povinný parametr. |Vyřešte a znovu odešlete žádost hello. Toto je vývojovém chyba se obvykle zachycena během počáteční testování. |
| unauthorized_client |Hello klientská aplikace není povolená toorequest autorizační kód. |K tomu obvykle dojde, když hello klientská aplikace není registrovaný ve službě Azure AD nebo klienta Azure AD toohello uživatele nebyla přidána. aplikace Hello můžete vyzvat uživatele hello s pokyny pro instalaci aplikace hello a její přidání tooAzure AD. |
| ACCESS_DENIED |Vlastník prostředku odepřen souhlasu |klientská aplikace Hello uvědomí hello uživatele, který nemůže pokračovat, dokud hello uživatel souhlasí. |
| unsupported_response_type |Hello autorizace server nepodporuje typ odpovědi hello v žádosti o hello. |Vyřešte a znovu odešlete žádost hello. Toto je vývojovém chyba se obvykle zachycena během počáteční testování. |
| server_error |Hello serveru došlo k neočekávané chybě. |Opakujte žádost hello. Tyto chyby může být důsledkem dočasné stavy. klientská aplikace Hello toohello uživatele mohou vysvětlit, že odpověď je zpožděno kvůli dočasné chybě. |
| temporarily_unavailable |Hello server je dočasně zaneprázdněn toohandle hello požadavku. |Opakujte žádost hello. klientská aplikace Hello toohello uživatele mohou vysvětlit, že odpověď je zpožděno kvůli dočasné podmínce. |
| invalid_resource |Hello cílového prostředku je neplatný, protože neexistuje, Azure AD ji nemůže najít, nebo není správně nakonfigurována. |To znamená, že prostředek hello, pokud existuje, není nakonfigurované v klientovi hello. aplikace Hello můžete vyzvat uživatele hello s pokyny pro instalaci aplikace hello a její přidání tooAzure AD. |

## Žádost o token přístupu
Teď, když jste získali authorization_code a bylo uděleno oprávnění uživatelem hello, lze uplatnit hello `code` pro `access_token` toohello požadovaného prostředku odesíláním `POST` požadavku toohello `/token` koncový bod:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [!TIP]
> Zkuste provést tuto žádost v Postman! (Nezapomeňte tooreplace hello `code`) [ ![spustit v Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)
> 
> 

| Parametr |  | Popis |
| --- | --- | --- |
| Klienta |Požadované |Hello `{tenant}` hodnota v cestě hello hello žádosti může být použité toocontrol, který se můžete přihlásit do aplikace hello.  Hello povolené hodnoty jsou `common`, `organizations`, `consumers`a identifikátory klientů.  Další podrobnosti naleznete v [protokolu Základy](active-directory-v2-protocols.md#endpoints). |
| client_id |Požadované |Hello Id aplikace tohoto portálu registrace hello ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) přiřazené vaší aplikace. |
| grant_type |Požadované |Musí být `authorization_code` pro tok autorizačního kódu hello. |
| Obor |Požadované |Seznam obory oddělených mezerami.  v této větev musí být ekvivalentní tooor podmnožinu hello obory požadované v první větev hello si vyžádal Hello obory.  Pokud hello rozsahy určené v této žádosti o span více prostředků serverů, se vrátí koncového bodu v2.0 hello token pro prostředek hello zadaná v oboru první hello.  Podrobnější vysvětlení oborů, najdete v části příliš[oprávnění, souhlasu a obory](active-directory-v2-scopes.md). |
| Kód |Požadované |Hello authorization_code, kterou jste získali v prvním větev hello hello toku. |
| redirect_uri |Požadované |Hello stejnou hodnotu redirect_uri, který byl použité tooacquire hello authorization_code. |
| tajný klíč client_secret |vyžaduje se pro webové aplikace |Hello aplikace tajný klíč, který jste vytvořili v portálu pro registraci aplikace hello pro vaši aplikaci.  Není vhodné jej použít v nativní aplikaci, protože client_secrets nelze uložit spolehlivě na zařízení.  Je vyžadována pro webové aplikace a webové rozhraní API, které mají hello možnost toostore hello tajný klíč client_secret bezpečně na straně serveru hello. |

#### Úspěšná odpověď
Úspěšné odpovědi tokenu bude vypadat jako:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parametr | Popis |
| --- | --- |
| access_token |Hello požadovaný přístupový token. Hello aplikace můžete používat tento token tooauthenticate toohello zabezpečeným prostředkům, například webového rozhraní API. |
| token_type |Určuje hodnotu hello typ tokenu. Hello pouze typ, který podporuje Azure AD je nosiče |
| expires_in |Jak dlouho hello přístupový token je platný (v sekundách). |
| Obor |Hello obory, které hello access_token je platný pro. |
| refresh_token |Aktualizace tokenu OAuth 2.0. Hello aplikace můžete používat tento token získat další přístupové tokeny po vypršení platnosti hello aktuální přístupový token.  Refresh_tokens je dlouhodobé a může být použité tooretain přístup tooresources pro dlouhou dobu.  Další podrobnosti najdete v části toohello [odkaz tokenu v2.0](active-directory-v2-tokens.md). |
| požadavku id_token |Nepodepsané JSON Web Token (JWT). Hello aplikace může base64Url dekódovat hello segmenty tento token toorequest informací o hello uživatele, který přihlášení. aplikace Hello můžete hello hodnoty do mezipaměti a jejich zobrazení, ale na ně neměli spoléhat pro žádné hranice zabezpečení nebo autorizace.  Další informace o id_tokens najdete v části hello [odkaz tokenu koncový bod v2.0](active-directory-v2-tokens.md). |

#### Chybové odpovědi
Chybové odpovědi bude vypadat jako:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: hello provided value for hello input parameter 'scope' is not valid. hello scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parametr | Popis |
| --- | --- |
| error |Řetězec kódu chyby, který lze použít tooclassify typů chyb, ke kterým došlo a může být použité tooreact tooerrors. |
| error_description |Konkrétní chybová zpráva, která může pomoci vývojář identifikovat hello hlavní příčinu chyby ověřování. |
| error_codes |Seznam tokenů zabezpečení určité chybové kódy, které vám můžou pomoct při diagnostiky. |
| časové razítko |Hello čas, kdy došlo k chybě hello. |
| trace_id |Jedinečný identifikátor pro hello požadavek, který vám pomůže v diagnostice. |
| correlation_id |Jedinečný identifikátor pro hello požadavek, který vám pomůže v diagnostice mezi komponentami. |

#### Kódy chyb pro koncový bod tokenu chyby
| Kód chyby | Popis | Akce klienta |
| --- | --- | --- |
| invalid_request |Chyba protokolu, například chybějící povinný parametr. |Vyřešte a znovu odešlete žádost hello |
| invalid_grant |Hello autorizační kód je neplatná nebo skončila jeho platnost. |Zkuste nové žádosti o toohello `/authorize` koncový bod |
| unauthorized_client |Hello ověřený klient není autorizován toouse Tato autorizace udělit typu. |K tomu obvykle dojde, když hello klientská aplikace není registrovaný ve službě Azure AD nebo klienta Azure AD toohello uživatele nebyla přidána. aplikace Hello můžete vyzvat uživatele hello s pokyny pro instalaci aplikace hello a její přidání tooAzure AD. |
| invalid_client |Ověření klienta se nezdařilo. |pověření klienta Hello nejsou platné. toofix, Správce aplikací hello aktualizuje hello přihlašovací údaje. |
| unsupported_grant_type |autorizace serveru Hello nepodporuje typ udělení autorizace hello. |Změna hello udělit typu v žádosti o hello. Tento typ chyby provedeno pouze během vývoje a zjistit během počáteční testování. |
| invalid_resource |Hello cílového prostředku je neplatný, protože neexistuje, Azure AD ji nemůže najít, nebo není správně nakonfigurována. |To znamená, že prostředek hello, pokud existuje, není nakonfigurované v klientovi hello. aplikace Hello můžete vyzvat uživatele hello s pokyny pro instalaci aplikace hello a její přidání tooAzure AD. |
| interaction_required |žádost o Hello vyžaduje interakci uživatele. Na další ověřování krok je třeba požadovaný. |Opakovat žádost hello s hello stejného zdroje. |
| temporarily_unavailable |Hello server je dočasně zaneprázdněn toohandle hello požadavku. |Opakujte žádost hello. klientská aplikace Hello toohello uživatele mohou vysvětlit, že odpověď je zpožděno kvůli dočasné podmínce. |

## Použití tokenu přístupu hello
Teď, když jste byla úspěšně načtena `access_token`, můžete použít hello token v žádosti o tooWeb rozhraní API zahrnutím v hello `Authorization` hlavičky:

> [!TIP]
> Provedení této žádosti v Postman! (Nahraďte hello `Authorization` záhlaví první) [ ![spustit v Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)
> 
> 

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## Aktualizujte hello přístupový token
Access_tokens jsou krátkou životnost a je nutné je aktualizovat po vypršení jejich platnosti toocontinue přístupu k prostředkům.  Můžete to provést tak, že zadáte jiný `POST` požadavku toohello `/token` koncový bod, tentokrát poskytování hello `refresh_token` místo hello `code`:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=refresh_token
&client_secret=JqQX2PNo9bpM0uEihUPzyrh      // NOTE: Only required for web apps
```

> [!TIP]
> Zkuste provést tuto žádost v Postman! (Nezapomeňte tooreplace hello `refresh_token`) [ ![spustit v Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)
> 
> 

| Parametr |  | Popis |
| --- | --- | --- |
| Klienta |Požadované |Hello `{tenant}` hodnota v cestě hello hello žádosti může být použité toocontrol, který se můžete přihlásit do aplikace hello.  Hello povolené hodnoty jsou `common`, `organizations`, `consumers`a identifikátory klientů.  Další podrobnosti naleznete v [protokolu Základy](active-directory-v2-protocols.md#endpoints). |
| client_id |Požadované |Hello Id aplikace tohoto portálu registrace hello ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) přiřazené vaší aplikace. |
| grant_type |Požadované |Musí být `refresh_token` pro tuto větev toku kódu autorizace hello. |
| Obor |Požadované |Seznam obory oddělených mezerami.  v této větev musí být ekvivalentní tooor podmnožinu hello obory požadované v hello původní authorization_code požadavek větev si vyžádal Hello obory.  Pokud hello rozsahy určené v této žádosti o span více prostředků serverů, se vrátí koncového bodu v2.0 hello token pro prostředek hello zadaná v oboru první hello.  Podrobnější vysvětlení oborů, najdete v části příliš[oprávnění, souhlasu a obory](active-directory-v2-scopes.md). |
| refresh_token |Požadované |Hello refresh_token, kterou jste získali v druhé větev hello hello toku. |
| redirect_uri |Požadované |Hello stejnou hodnotu redirect_uri, který byl použité tooacquire hello authorization_code. |
| tajný klíč client_secret |vyžaduje se pro webové aplikace |Hello aplikace tajný klíč, který jste vytvořili v portálu pro registraci aplikace hello pro vaši aplikaci.  Není vhodné jej použít v nativní aplikaci, protože client_secrets nelze uložit spolehlivě na zařízení.  Je vyžadována pro webové aplikace a webové rozhraní API, které mají hello možnost toostore hello tajný klíč client_secret bezpečně na straně serveru hello. |

#### Úspěšná odpověď
Úspěšné odpovědi tokenu bude vypadat jako:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parametr | Popis |
| --- | --- |
| access_token |Hello požadovaný přístupový token. Hello aplikace můžete používat tento token tooauthenticate toohello zabezpečeným prostředkům, například webového rozhraní API. |
| token_type |Určuje hodnotu hello typ tokenu. Hello pouze typ, který podporuje Azure AD je nosiče |
| expires_in |Jak dlouho hello přístupový token je platný (v sekundách). |
| Obor |Hello obory, které hello access_token je platný pro. |
| refresh_token |Nové obnovovací token OAuth 2.0. Nahradíte hello staré aktualizace tokenu s tooensure token tuto nově získal aktualizace obnovovacích tokenů zůstávají platná po dobu. |
| požadavku id_token |Nepodepsané JSON Web Token (JWT). Hello aplikace může base64Url dekódovat hello segmenty tento token toorequest informací o hello uživatele, který přihlášení. aplikace Hello můžete hello hodnoty do mezipaměti a jejich zobrazení, ale na ně neměli spoléhat pro žádné hranice zabezpečení nebo autorizace.  Další informace o id_tokens najdete v části hello [odkaz tokenu koncový bod v2.0](active-directory-v2-tokens.md). |

#### Chybové odpovědi
```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: hello provided value for hello input parameter 'scope' is not valid. hello scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parametr | Popis |
| --- | --- |
| error |Řetězec kódu chyby, který lze použít tooclassify typů chyb, ke kterým došlo a může být použité tooreact tooerrors. |
| error_description |Konkrétní chybová zpráva, která může pomoci vývojář identifikovat hello hlavní příčinu chyby ověřování. |
| error_codes |Seznam tokenů zabezpečení určité chybové kódy, které vám můžou pomoct při diagnostiky. |
| časové razítko |Hello čas, kdy došlo k chybě hello. |
| trace_id |Jedinečný identifikátor pro hello požadavek, který vám pomůže v diagnostice. |
| correlation_id |Jedinečný identifikátor pro hello požadavek, který vám pomůže v diagnostice mezi komponentami. |

Popis hello kódy chyb a hello Doporučená akce klienta, najdete v tématu [kódy chyb pro koncový bod tokenu chyby](#error-codes-for-token-endpoint-errors).

