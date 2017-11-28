---
title: "hello aaaUnderstand OpenID Connect kód tok ověřování ve službě Azure AD | Microsoft Docs"
description: "Tento článek popisuje, jak přistupovat k toouse HTTP zprávy tooauthorize tooweb aplikací a webových rozhraní API ve vašem klientovi pomocí Azure Active Directory a OpenID Connect."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 29142f7e-d862-4076-9a1a-ecae5bcd9d9b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: fafd8ab906ee576c584fec2ef1e9de83ddb1f6e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# Povolit přístup k aplikacím tooweb pomocí OpenID Connect a službou Azure Active Directory
[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) je vrstva jednoduché identity postavená na protokol hello OAuth 2.0. Definuje OAuth 2.0 tooobtain mechanismy a použít **přístup tokeny** tooaccess chráněné zdroje, ale nejsou definovány informací o identitě tooprovide standardní metody. Jako rozšíření toohello proces autorizace OAuth 2.0, OpenID Connect implementuje ověřování. Poskytuje informace o hello koncového uživatele ve formě hello `id_token` který ověřuje identitu hello hello uživatele a poskytuje základní profil informace o uživateli hello.

Naše doporučení OpenID Connect je, pokud vytváříte webovou aplikaci, která je hostovaná na serveru a k němu přistupovat prostřednictvím prohlížeče.


[!INCLUDE [active-directory-protocols-getting-started](../../../includes/active-directory-protocols-getting-started.md)] 

## Tok ověřování pomocí OpenID Connect
Hello nejzákladnější tok přihlášení obsahuje následující kroky hello – každý z nich je podrobně popsaná níže.

![OpenId Connect tok ověřování](media/active-directory-protocols-openid-connect-code/active-directory-oauth-code-flow-web-app.png)

## Dokument metadat OpenID Connect

Popisuje dokument metadat, která obsahuje většinu hello informace požadované pro aplikaci tooperform přihlášení, OpenID Connect. To zahrnuje informace, jako je toouse hello adresy URL a hello umístění služby hello veřejné podpisové klíče. Dokument metadat OpenID Connect Hello najdete tady:

```
https://login.microsoftonline.com/{tenant}/.well-known/openid-configuration
```
Hello metadata je jednoduchý dokument JavaScript Object Notation (JSON). Viz následující fragment kódu pro příklad hello. Hello fragment kódu na obsah je podrobně popsán v hello [OpenID Connect specifikace](https://openid.net).

```
{
    "authorization_endpoint": "https://login.microsoftonline.com/common/oauth2/authorize",
    "token_endpoint": "https://login.microsoftonline.com/common/oauth2/token",
    "token_endpoint_auth_methods_supported":
    [
        "client_secret_post",
        "private_key_jwt"
    ],
    "jwks_uri": "https://login.microsoftonline.com/common/discovery/keys"
    
    ...
}
```

## Odeslání požadavku na hello přihlášení
Pokud vaše webová aplikace, musí uživatel hello tooauthenticate, se musí přímé hello uživatele toohello `/authorize` koncový bod. Tento požadavek je podobné toohello první rameno hello [toku OAuth 2.0 autorizační kód](active-directory-protocols-oauth-code.md), s několika důležité rozdíly:

* Hello žádost musí obsahovat rozsah hello `openid` v hello `scope` parametr.
* Hello `response_type` musí obsahovat parametr `id_token`.
* Hello žádost musí obsahovat hello `nonce` parametr.

Takže ukázková žádost bude vypadat takto:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=form_post
&scope=openid
&state=12345
&nonce=7362CAEA-9CA5-4B43-9BA3-34D7C303EBA7
```

| Parametr |  | Popis |
| --- | --- | --- |
| Klienta |Požadované |Hello `{tenant}` hodnota v cestě hello hello žádosti může být použité toocontrol, který se můžete přihlásit do aplikace hello.  Hello povolené hodnoty jsou identifikátory klienta, například `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` nebo `contoso.onmicrosoft.com` nebo `common` pro tokeny nezávislé na klienta |
| client_id |Požadované |Hello Id aplikace přiřazené tooyour aplikace při registraci s Azure AD. Tento nástroj naleznete v hello portálu Azure. Klikněte na tlačítko **Azure Active Directory**, klikněte na tlačítko **registrace aplikace**, zvolte hello aplikace a vyhledejte hello Id aplikace na stránce aplikace hello. |
| response_type |Požadované |Musí zahrnovat `id_token` pro OpenID Connect přihlášení.  Může také obsahovat další response_types, jako například `code`. |
| Obor |Požadované |Seznam obory oddělených mezerami.  Pro OpenID Connect, musí obsahovat hello oboru `openid`, což znamená, že oprávnění "Přihlásit" toohello v hello souhlasu uživatelského rozhraní.  Může také zahrnovat další obory v této žádosti o žádosti o souhlas. |
| hodnotu Nonce |Požadované |Hodnotu zahrnuté v žádosti o hello vygenerovaný hello aplikací, který je součástí hello výsledná `id_token` jako deklarace identity.  aplikace Hello můžete pak ověřte, že tato hodnota toomitigate opětovného přehrání tokenu útoků.  Hodnota Hello je obvykle náhodnou jedinečného řetězce nebo identifikátor GUID, které můžou být použité tooidentify hello původ hello požadavku. |
| redirect_uri |Doporučená |Hello redirect_uri vaší aplikace, kde můžete odesílat a přijímat aplikace odpovědi ověřování.  Se musí přesně shodovat s jedním z hello redirect_uris, registrován v hello portál, s výjimkou musí být kódovaná adresou url. |
| response_mode |Doporučená |Určuje metodu hello, který by měl být použité toosend hello výsledné authorization_code back tooyour aplikace.  Podporované hodnoty jsou `form_post` pro *HTTP post formuláře* nebo `fragment` pro *fragment adresy URL*.  U webových aplikací, doporučujeme použít `response_mode=form_post` tooensure hello nejbezpečnější přenos tokeny tooyour aplikace. |
| state |Doporučená |Hodnota součástí hello požadavek, který je vrácený v odpovědi tokenu hello.  Může být řetězec o délce veškerý obsah, který chcete.  Náhodně generované jedinečné hodnoty se obvykle používá u [prevence útoků padělání požadavku posílaného mezi weby](http://tools.ietf.org/html/rfc6749#section-10.12).  Stav Hello je také použít tooencode informace o stavu hello uživatele v aplikaci hello před došlo k požadavku hello ověřování, jako je například stránku hello nebo zobrazení, které byly na. |
| řádku |Volitelné |Označuje typ hello interakce s uživatelem, který je vyžadován.  V současné době hello pouze platné hodnoty jsou "přihlášení", 'none' a 'souhlas'.  `prompt=login`Vynutí hello uživatele tooenter přihlašovacích údajů tohoto požadavku negace jednotného přihlašování.  `prompt=none`hello opačné – zajišťuje, že tento hello uživatel není uveden s žádné interaktivní výzvy jakékoli.  Pokud požadavek hello nemůže být dokončena bez upozornění pomocí jednotného přihlašování, koncový bod hello vrátí chybu.  `prompt=consent`Aktivuje hello OAuth dialogové okno po hello uživatel se přihlásí, dotazem hello uživatele toogrant oprávnění toohello aplikace. |
| login_hint |Volitelné |Může být použité toopre výplně hello uživatelského jména nebo e-mailovou adresu pole hello přihlašovací stránky pro uživatele hello, pokud znáte svoje uživatelské jméno předem.  Tento parametr použijte často aplikace během opětovné ověření, uživatelské jméno hello s z předchozí přihlášení už extrahovat pomocí hello `preferred_username` deklarací identity. |

V tomto okamžiku je uživatel hello výzva tooenter své přihlašovací údaje a dokončení hello ověřování.

### Ukázková odpověď
Ukázková odpověď po hello uživatel byl ověřen, může vypadat například takto:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parametr | Popis |
| --- | --- |
| požadavku id_token |Hello `id_token` požadovanou aplikaci hello. Můžete použít hello `id_token` tooverify hello identitu uživatele a zahájit relaci s hello uživatele. |
| state |Hodnota součástí hello požadavek, který je také vrácený v odpovědi tokenu hello. Náhodně generované jedinečné hodnoty se obvykle používá u [prevence útoků padělání požadavku posílaného mezi weby](http://tools.ietf.org/html/rfc6749#section-10.12).  Stav Hello je také použít tooencode informace o stavu hello uživatele v aplikaci hello před došlo k požadavku hello ověřování, jako je například stránku hello nebo zobrazení, které byly na. |

### Chybové odpovědi
Chybové odpovědi se taky může odeslat toohello `redirect_uri` tak hello aplikace můžete správně zpracovat:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parametr | Popis |
| --- | --- |
| error |Řetězec kódu chyby, který lze použít tooclassify typů chyb, ke kterým došlo a může být použité tooreact tooerrors. |
| error_description |Konkrétní chybová zpráva, která může pomoci vývojář identifikovat hello hlavní příčinu chyby ověřování. |

#### Kódy chyb pro chyb koncový bod autorizace
Hello následující tabulka popisuje hello různé kódy chyb, které mohou být vráceny v hello `error` parametr hello chybové odpovědi.

| Kód chyby | Popis | Akce klienta |
| --- | --- | --- |
| invalid_request |Chyba protokolu, například chybějící povinný parametr. |Vyřešte a znovu odešlete žádost hello. To je chyba vývoj a je obvykle zachycena během počáteční testování. |
| unauthorized_client |Hello klientská aplikace není povolená toorequest autorizační kód. |K tomu obvykle dojde, když hello klientská aplikace není registrovaný ve službě Azure AD nebo klienta Azure AD toohello uživatele nebyla přidána. aplikace Hello můžete vyzvat uživatele hello s pokyny pro instalaci aplikace hello a její přidání tooAzure AD. |
| ACCESS_DENIED |Vlastník prostředku odepřen souhlasu |klientská aplikace Hello uvědomí hello uživatele, který nemůže pokračovat, dokud hello uživatel souhlasí. |
| unsupported_response_type |Hello autorizace server nepodporuje typ odpovědi hello v žádosti o hello. |Vyřešte a znovu odešlete žádost hello. To je chyba vývoj a je obvykle zachycena během počáteční testování. |
| server_error |Hello serveru došlo k neočekávané chybě. |Opakujte žádost hello. Tyto chyby může být důsledkem dočasné stavy. klientská aplikace Hello toohello uživatele mohou vysvětlit, že odpovědi je zpožděno z důvodu tooa dočasné chybě. |
| temporarily_unavailable |Hello server je dočasně zaneprázdněn toohandle hello požadavku. |Opakujte žádost hello. klientská aplikace Hello mohou vysvětlit toohello uživatele, odpovědi je zpožděno z důvodu tooa dočasné podmínce. |
| invalid_resource |Hello cílového prostředku je neplatný, protože neexistuje, Azure AD ji nemůže najít, nebo není správně nakonfigurována. |To znamená, že prostředek hello, pokud existuje, není nakonfigurované v klientovi hello. aplikace Hello můžete vyzvat uživatele hello s pokyny pro instalaci aplikace hello a její přidání tooAzure AD. |

## Ověření požadavku id_token hello
Právě přijetí `id_token` není dostatečná tooauthenticate hello uživatel, je nutné ověřit podpis hello a ověřte hello deklarací identity v hello `id_token` podle požadavků vaší aplikace. koncový bod Azure AD Hello používá webové tokeny JSON (Jwt) a tokeny toosign kryptografie využívající veřejného klíče a ověřte, zda je platný.

Můžete zvolit toovalidate hello `id_token` v kódu klienta, ale běžnou praxí je toosend hello `id_token` tooa back-end serveru a provést ověření hello existuje. Jakmile jste ověřit podpis hello hello `id_token`, existuje několik deklarace identity, které jsou požadované tooverify.

Také je možné toovalidate další deklarace identity v závislosti na vašem scénáři. Některé běžné ověření patří:

* Zajištění hello uživatele či organizace má zaregistrovali do aplikace hello.
* Zajistit hello uživatel má správná oprávnění autorizace
* Zajištění sílu ověřování došlo k, jako je vícefaktorové ověřování.

Jakmile ověříte hello `id_token`, můžete začít relaci s hello uživatele a používat deklarace identity hello v hello `id_token` tooobtain informace o hello uživateli ve vaší aplikaci. Tyto informace můžete použít pro zobrazení, záznamy, oprávnění atd. Další informace o hello typy tokenů a deklarací identity, najdete v tématu [podporované tokenu a typy deklarací identity](active-directory-token-and-claims.md).

## Poslat žádost o odhlášení
Pokud chcete uživatele hello toosign mimo aplikaci hello, je dostatečná tooclear soubory cookie vaší aplikace nebo jinak ukončení relace hello hello uživatele.  Také je nutné přesměrovat uživatele toohello hello `end_session_endpoint` pro odhlášení.  Pokud selže toodo tak, hello uživatel nebude moct tooreauthenticate tooyour aplikace bez nutnosti zadávat své přihlašovací údaje znovu, protože budou mít platný jedné přihlášení relace s koncovým bodem hello Azure AD.

Jednoduše můžete přesměrovat uživatele toohello hello `end_session_endpoint` uvedené v dokumentu metadat OpenID Connect hello:

```
GET https://login.microsoftonline.com/common/oauth2/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F

```

| Parametr |  | Popis |
| --- | --- | --- |
| post_logout_redirect_uri |Doporučená |Hello URL, která hello uživatel by měl být přesměrovaného tooafter úspěšném odhlášení.  Pokud nejsou, zobrazí se hello uživatele obecnou zprávou. |

## Jednotné odhlašování
Při přesměrování hello uživatele toohello `end_session_endpoint`, Azure AD vymaže relace hello uživatele z prohlížeče hello. Hello uživatel však stále může být podepsané tooother aplikace, které používají Azure AD pro ověřování. tooenable toosign tyto aplikace hello uživatele se současně, Azure AD odešle požadavek HTTP GET toohello, zaregistrovaný `LogoutUrl` všech aplikací hello tento uživatel hello je aktuálně přihlášený do. Aplikace musí odpovědět toothis požadavek zrušením jakékoli relace, který identifikuje hello uživatele a vrácení `200` odpovědi.  Pokud chcete v aplikaci toosupport jednotné přihlašování odhlašování, je nutné implementovat, `LogoutUrl` v kódu aplikace.  Můžete nastavit hello `LogoutUrl` z hello portálu Azure:

1. Přejděte toohello [portálu Azure](https://portal.azure.com).
2. Kliknutím na vašem účtu v hello pravém horním rohu stránky hello zvolte vaší služby Active Directory.
3. Hello levém navigačním panelu, vyberte **Azure Active Directory**, zvolte **registrace aplikace** a vyberte svou aplikaci.
4. Klikněte na **vlastnosti** a najde hello **adresy URL odhlašovací** textové pole. 

## Získání tokenu
Mnoho webových aplikací potřebovat toonot pouze přihlašovací hello uživatele v, ale také přístup k webové službě jménem tohoto uživatele pomocí metody OAuth. Tento scénář kombinuje OpenID Connect pro ověřování uživatelů při získávání současně `authorization_code` který lze použít tooget `access_tokens` pomocí hello toku kódu autorizace OAuth.

## Získat přístupové tokeny
tooacquire přístupových tokenů, je třeba toomodify hello požádat o přihlášení z výše:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e        // Your registered Application Id
&response_type=id_token+code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F       // Your registered Redirect Uri, url encoded
&response_mode=form_post                              // form_post', or 'fragment'
&scope=openid
&resource=https%3A%2F%2Fservice.contoso.com%2F                                     
&state=12345                                          // Any value, provided by your app
&nonce=678910                                         // Any value, provided by your app
```

V žádosti o hello včetně obory oprávnění a použitím `response_type=code+id_token`, hello `authorize` koncový bod zajistí, že hello uživatel souhlasí toohello oprávnění uvedené v hello `scope` parametr dotazu a vracet autorizační kód aplikace tooexchange pro přístupový token.

### Úspěšná odpověď
Úspěšná odpověď pomocí `response_mode=form_post` vypadá jako:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Parametr | Popis |
| --- | --- |
| požadavku id_token |Hello `id_token` požadovanou aplikaci hello. Můžete použít hello `id_token` tooverify hello identitu uživatele a zahájit relaci s hello uživatele. |
| Kód |Hello authorization_code, který hello požadované aplikace. Hello aplikace můžete používat hello autorizační kód toorequest přístupový token hello cílový prostředek. Authorization_codes jsou krátkou životnost a obvykle vyprší po přibližně 10 minut. |
| state |Pokud parametr stavu je zahrnuta v žádosti o hello, hello stejnou hodnotu by se zobrazit v odpovědi hello. Hello aplikace by měla ověřte, zda jsou identické hodnoty stavu hello v hello žádostí a odpovědí. |

### Chybové odpovědi
Chybové odpovědi se taky může odeslat toohello `redirect_uri` tak hello aplikace můžete správně zpracovat:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parametr | Popis |
| --- | --- |
| error |Řetězec kódu chyby, který lze použít tooclassify typů chyb, ke kterým došlo a může být použité tooreact tooerrors. |
| error_description |Konkrétní chybová zpráva, která může pomoci vývojář identifikovat hello hlavní příčinu chyby ověřování. |

Popis hello možných kódů chyb a jejich klienta doporučenou akci najdete v tématu [kódy chyb pro chyb koncový bod autorizace](#error-codes-for-authorization-endpoint-errors).

Jakmile jste, že jste podmínky povolení `code` a `id_token`, můžete uživatele hello přihlásit a získat přístupové tokeny jejich jménem.  uživatel hello toosign v, musíte ověřit hello `id_token` přesně tak, jak je popsáno výše. tooget přístupových tokenů, můžete provést hello kroky popsané v části "Použití hello autorizační kód toorequest přístupový token" hello z našich [dokumentace k protokolu OAuth](active-directory-protocols-oauth-code.md).
