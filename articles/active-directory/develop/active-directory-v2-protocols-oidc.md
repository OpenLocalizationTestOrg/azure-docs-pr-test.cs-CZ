---
title: "aaaAzure služby Active Directory v2.0 a hello protokolu OpenID Connect | Microsoft Docs"
description: "Vytvoření webové aplikace pomocí hello Azure AD v2.0 implementace ověřovacího protokolu OpenID Connect hello."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: a4875997-3aac-4e4c-b7fe-2b4b829151ce
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 277bb406dbb24ccefb09e4e4c928f0421755cf61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# Azure Active Directory v2.0 a hello protokolu OpenID Connect
OpenID Connect je ověřovací protokol založený na OAuth 2.0, které jsou používané přihlašovací toosecurely ve webové aplikaci tooa uživatele. Pokud používáte hello koncového bodu v2.0 pro implementaci OpenID Connect, můžete přidat přihlášení a rozhraní API přístup tooyour webové aplikace. V tomto článku jsme ukazují, jak toodo tomto nezávislé na jazyku. Jsme popisují, jak toosend a přijímat zprávy HTTP bez použití žádné knihovny Microsoft open-source.

> [!NOTE]
> koncový bod v2.0 Hello nepodporuje všechny scénáře Azure Active Directory a funkce. toodetermine zda byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).
> 
> 

[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) rozšiřuje hello OAuth 2.0 *autorizace* toouse protokol jako *ověřování* protokolu, takže můžete provést jeden přihlašování pomocí metody OAuth. OpenID Connect zavádí koncepci hello *ID token*, což je token zabezpečení, která umožňuje klientovi hello tooverify hello identitu uživatele hello. Hello ID token také získá profil základní informace o uživateli hello. Protože OpenID Connect rozšiřuje OAuth 2.0, můžete bezpečně získat aplikace *přístup tokeny*, které lze použít tooaccess prostředky, které jsou zabezpečeny [serveru ověřování](active-directory-v2-protocols.md#the-basics). Doporučujeme vám, že používáte OpenID Connect, pokud vytváříte [webové aplikace](active-directory-v2-flows.md#web-apps) která je hostovaná na serveru a získat přístup prostřednictvím prohlížeče.

## Diagram protokolu: přihlášení
Hello nejzákladnější toku přihlášení má hello kroky uvedené v další diagram hello. Jsme popisují každý krok podrobně v tomto článku.

![Protokolu OpenID Connect: přihlášení](../../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

## Načtení dokumentu metadat OpenID Connect hello
Popisuje dokument metadat, která obsahuje většinu hello informace požadované pro aplikaci tooperform přihlášení, OpenID Connect. To zahrnuje informace, jako je toouse hello adresy URL a hello umístění služby hello veřejné podpisové klíče. U koncového bodu v2.0 hello je to hello OpenID Connect dokument metadat, které byste měli používat:

```
https://login.microsoftonline.com/{tenant}/v2.0/.well-known/openid-configuration
```

Hello `{tenant}` může trvat jednu ze čtyř hodnot:

| Hodnota | Popis |
| --- | --- |
| `common` |Uživatelé s účtem Microsoft osobní i pracovní nebo školní účet ze služby Azure Active Directory (Azure AD) můžete přihlásit toohello aplikace. |
| `organizations` |Pouze uživatelé s pracovní nebo školní účty z Azure AD můžete přihlásit toohello aplikace. |
| `consumers` |Pouze uživatelé s osobní účet Microsoft můžete přihlásit toohello aplikace. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` nebo `contoso.onmicrosoft.com` |Jenom uživatelé s pracovní nebo školní účet z konkrétní Azure AD můžete klienta přihlásit toohello aplikace. Buď hello domény popisný název hello klienta Azure AD, nebo můžete použít identifikátor GUID klienta hello. |

Hello metadata je jednoduchý dokument JavaScript Object Notation (JSON). Viz následující fragment kódu pro příklad hello. Hello fragment kódu na obsah je podrobně popsán v hello [OpenID Connect specifikace](https://openid.net).

```
{
  "authorization_endpoint": "https:\/\/login.microsoftonline.com\/common\/oauth2\/v2.0\/authorize",
  "token_endpoint": "https:\/\/login.microsoftonline.com\/common\/oauth2\/v2.0\/token",
  "token_endpoint_auth_methods_supported": [
    "client_secret_post",
    "private_key_jwt"
  ],
  "jwks_uri": "https:\/\/login.microsoftonline.com\/common\/discovery\/v2.0\/keys",

  ...

}
```

Obvykle byste použili tento tooconfigure dokumentu metadat OpenID Connect knihovny nebo SDK; Hello knihovně využije hello metadata toodo svou práci. Ale pokud nepoužíváte knihovnu před sestavením OpenID Connect, můžete podle kroků hello hello zbývající část tohoto článku tooperform přihlášení ve webové aplikaci pomocí koncového bodu v2.0 hello.

## Odeslání požadavku na hello přihlášení
Pokud vaše webová aplikace musí tooauthenticate hello uživatele, je přímé hello uživatele toohello `/authorize` koncový bod. Tento požadavek je podobné toohello první rameno hello [toku kódu autorizace OAuth 2.0](active-directory-v2-protocols-oauth-code.md), se tyto důležité rozdíly:

* Hello žádost musí obsahovat hello `openid` obor v hello `scope` parametr.
* Hello `response_type` musí obsahovat parametr `id_token`.
* Hello žádost musí obsahovat hello `nonce` parametr.

Například:

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=form_post
&scope=openid
&state=12345
&nonce=678910
```

> [!TIP]
> Klikněte na následující odkaz tooexecute hello tuto žádost. Po přihlášení, bude váš prohlížeč přesměrovaného toohttps://localhost/myapp/ k tokenu ID v panelu Adresa hello. Všimněte si, že tento požadavek používá `response_mode=query` (pouze pro demonstrační účely). Doporučujeme vám, že používáte `response_mode=form_post`.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid&response_mode=query&state=12345&nonce=678910" target="_blank">https://Login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>
> 
> 

| Parametr | Podmínka | Popis |
| --- | --- | --- |
| Klienta |Požaduje se |Můžete použít hello `{tenant}` hodnota hello cesta toocontrol hello požadavek, který může přihlásit toohello aplikace. Hello povolené hodnoty jsou `common`, `organizations`, `consumers`a identifikátory klientů. Další informace najdete v tématu [protokolu Základy](active-directory-v2-protocols.md#endpoints). |
| client_id |Požaduje se |ID tohoto hello Hello aplikace [portálu pro registraci aplikace](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) přiřazené tooyour aplikace. |
| response_type |Požaduje se |Musí zahrnovat `id_token` pro OpenID Connect přihlášení. Může také obsahovat jiné `response_types` hodnoty, jako například `code`. |
| redirect_uri |Doporučené |identifikátor URI aplikace, kde můžete odesílat a přijímat aplikace ověřování odpovědi přesměrování Hello. Se musí přesně shodovat s jedním z hello přesměrování identifikátory URI registrován hello portálu, s tím rozdílem, že adresa URL musí být kódovaný. |
| Obor |Požaduje se |Seznam obory oddělených mezerami. Pro OpenID Connect, musí obsahovat hello oboru `openid`, což znamená, že oprávnění "Přihlásit" toohello v hello souhlasu uživatelského rozhraní. Také mohou zahrnovat jiné obory v této žádosti o žádosti o souhlas. |
| hodnotu Nonce |Požaduje se |Hodnota, zahrnuté v žádosti o hello vygenerovaný hello aplikací, který bude zahrnut v hello výsledná hodnota požadavku id_token jako deklarace identity. aplikace Hello můžete ověřit, že tato hodnota toomitigate opětovného přehrání tokenu útoků. Hodnota Hello je obvykle náhodnou jedinečného řetězce, které můžou být použité tooidentify hello původ hello požadavku. |
| response_mode |Doporučené |Určuje hello metodu, která má být použít toosend hello výsledné autorizační kód back tooyour aplikace. Může být jedna z `query`, `form_post`, nebo `fragment`. U webových aplikací, doporučujeme použít `response_mode=form_post`, tooensure hello nejbezpečnější přenos tokeny tooyour aplikace. |
| state |Doporučené |Hodnota součástí hello požadavek, který bude také vrácen v odpovědi tokenu hello. Může být řetězec o délce požadovaný obsah. Náhodně generované jedinečné hodnoty se obvykle používá příliš[zabránit útokům padělání požadavku posílaného mezi weby](http://tools.ietf.org/html/rfc6749#section-10.12). Stav Hello také je použité tooencode informace o stavu hello uživatele v aplikaci hello před hello ověřování požadavku došlo k chybě, například hello stránky nebo zobrazení hello uživatel byl na. |
| řádku |Nepovinné |Označuje typ hello interakce s uživatelem, který je vyžadován. Hello pouze platné hodnoty v tuto chvíli jsou `login`, `none`, a `consent`. Hello `prompt=login` deklarace identity vynutí hello uživatele tooenter přihlašovacích údajů tohoto požadavku, která Neguje jednotné přihlašování. Hello `prompt=none` deklarace identity je opačné hello. Tento požadavek zajistí, že tento hello uživatel není uveden s žádné jakkoli interaktivní výzvu. Pokud hello požadavek nemohl být dokončen bezobslužně přes jednotné přihlašování, koncového bodu v2.0 hello vrátí chybu. Hello `prompt=consent` deklarace aktivační události hello OAuth dialogové okno po přihlášení uživatele hello. Dialogové okno Hello požádá hello uživatele toogrant oprávnění toohello aplikace. |
| login_hint |Nepovinné |Pole můžete použít tento parametr toopre výplně hello uživatelské jméno a e-mailovou adresu hello přihlašovací stránky pro uživatele hello, pokud víte, uživatelské jméno hello předem. Aplikace často, použijte tento parametr během opětovné ověření po již extrahování hello uživatelského jména z dřívějších sign-in pomocí hello `preferred_username` deklarací identity. |
| domain_hint |Nepovinné |Tato hodnota může být `consumers` nebo `organizations`. Pokud zahrnuty, vynechává procesu zjišťování na základě e-mailu hello tento uživatel hello prochází na hello v2.0 přihlašovací stránce pro mírně zefektivnění činnost koncového uživatele. Často aplikace použijte tento parametr během opětovné ověření extrahováním hello `tid` deklarací z tokenu ID hello. Pokud hello `tid` deklarace, hodnota je `9188040d-6c67-4c5b-b112-36a304b66dad`, použijte `domain_hint=consumers`. Jinak použijte `domain_hint=organizations`. |

V tomto okamžiku hello uživatele je výzvami tooenter své přihlašovací údaje a dokončení hello ověřování. Hello koncového bodu v2.0 ověřuje, že hello uživatel souhlasí toohello oprávnění uvedené v hello `scope` parametr dotazu. Pokud uživatel hello nedala souhlas tooany tato oprávnění, vyzve koncového bodu v2.0 hello hello uživatele tooconsent toohello požadované oprávnění. Další informace o [oprávnění, souhlasu a víceklientské aplikace](active-directory-v2-scopes.md).

Po hello uživatel ověří a uděluje souhlas, vrátí koncový bod v2.0 hello uvedené aplikace tooyour na odpovědi na hello identifikátor URI přesměrování pomocí metody hello stanovené v hello `response_mode` parametr.

### Úspěšná odpověď
Úspěšná odpověď při použití `response_mode=form_post` vypadá podobně jako tento:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parametr | Popis |
| --- | --- |
| požadavku id_token |Hello ID token, který hello požadované aplikace. Můžete použít hello `id_token` parametr tooverify hello identitu uživatele a zahájit relaci s hello uživatele. Další podrobnosti o ID tokeny a jejich obsah najdete v tématu hello [koncového bodu v2.0 tokeny odkaz](active-directory-v2-tokens.md). |
| state |Pokud `state` parametr je zahrnuta v žádosti o hello hello stejnou hodnotu by se měla objevit v odpovědi hello. Hello aplikace by měla ověřte, zda jsou identické hodnoty stavu hello v hello žádostí a odpovědí. |

### Chybové odpovědi
Chybové odpovědi může také poslat identifikátor URI pro přesměrování toohello tak, že hello aplikace dokáže zpracovat je. Chybnou odpověď vypadá takto:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parametr | Popis |
| --- | --- |
| error |Řetězec kódu chyby, kterou můžete použít tooclassify typy chyb, ke kterým došlo, a tooreact tooerrors. |
| error_description |Konkrétní chybová zpráva, která vám může pomoct identifikovat hello hlavní příčinu chyby ověřování. |

### Kódy chyb pro chyb koncový bod autorizace
Hello následující tabulka popisuje chybové kódy, které mohou být vráceny v hello `error` parametr hello chybové odpovědi:

| Kód chyby | Popis | Akce klienta |
| --- | --- | --- |
| invalid_request |Chyba protokolu, například chybějící, vyžaduje parametr. |Vyřešte a znovu odešlete žádost hello. Jedná se o chybu vývoj, který je obvykle zachycena během počáteční testování. |
| unauthorized_client |klientská aplikace Hello nemůže požádat o autorizační kód. |K tomu obvykle dojde, když hello klientská aplikace není registrovaný ve službě Azure AD nebo klienta Azure AD toohello uživatele nebyla přidána. Hello aplikace můžete zobrazit výzvu uživateli hello s pokyny tooinstall hello aplikací a přidat jej tooAzure AD. |
| ACCESS_DENIED |vlastník prostředku Hello odepřen souhlasu. |klientská aplikace Hello uvědomí hello uživatele, který nemůže pokračovat, dokud hello uživatel souhlasí. |
| unsupported_response_type |Hello autorizace server nepodporuje typ odpovědi hello v žádosti o hello. |Vyřešte a znovu odešlete žádost hello. Jedná se o chybu vývoj, který je obvykle zachycena během počáteční testování. |
| server_error |Hello serveru došlo k neočekávané chybě. |Opakujte žádost hello. Tyto chyby může být důsledkem dočasné stavy. klientská aplikace Hello toohello uživatele mohou vysvětlit, že odpovědi je zpožděno z důvodu tooa dočasné chybě. |
| temporarily_unavailable |Hello server je dočasně zaneprázdněn toohandle hello požadavku. |Opakujte žádost hello. klientská aplikace Hello mohou vysvětlit toohello uživatele, odpovědi je zpožděno z důvodu tooa dočasné podmínce. |
| invalid_resource |Hello cílového prostředku je neplatný, protože buď neexistuje, Azure AD ji nemůže najít, nebo není správně nakonfigurována. |To znamená, že prostředek hello, pokud existuje, nebyl nakonfigurován v klientovi hello. aplikace Hello můžete vyzvat uživatele hello s pokyny pro instalaci aplikace hello a její přidání tooAzure AD. |

## Ověřit hello ID token
Přijetí tokenu ID není dostatečná tooauthenticate hello uživatele. Musíte také ověřit podpis tokenu ID hello a ověřit hello deklarace identity ve hello tokenu podle požadavků vaší aplikace. koncový bod v2.0 Hello používá [webové tokeny JSON (Jwt)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) a veřejný klíč kryptografie toosign tokeny a ověřte, zda je platný.

V klientském kódu můžete toovalidate hello ID token, ale běžnou praxí je toosend hello ID tokenu tooa back-end serverů a provést ověření hello existuje. Poté, co jste ověřit podpis hello hello ID tokenu, budete potřebovat tooverify několik deklarací identity. Další informace, včetně informace o [ověřování tokenů](active-directory-v2-tokens.md#validating-tokens) a [důležité informace o podepisování výměna klíče](active-directory-v2-tokens.md#validating-tokens), najdete v části hello [v2.0 tokeny odkaz](active-directory-v2-tokens.md). Doporučujeme pomocí knihovny tooparse a ověření tokenů. Nejméně jedna z těchto knihoven k dispozici je pro většinu jazyky a platformy.
<!--TODO: Improve hello information on this-->

Můžete také chtít toovalidate další deklarace identity, v závislosti na vašem scénáři. Některé běžné ověření patří:

* Zkontrolujte, zda hello uživatele nebo organizace má přihlášeni pro aplikaci hello.
* Zkontrolujte, zda že má tento uživatel hello požadované oprávnění nebo autorizace.
* Ujistěte se, že došlo k sílu ověřování, jako je vícefaktorové ověřování.

Další informace o deklaracích identity hello v tokenu ID najdete v tématu hello [koncového bodu v2.0 tokeny odkaz](active-directory-v2-tokens.md).

Po zcela ověření tokenu ID hello můžete začít relaci s hello uživatelem. Použití deklarací identity hello v hello ID tokenu tooget informace o hello uživatele ve vaší aplikaci. Tyto informace můžete použít pro zobrazení, záznamy, oprávnění a tak dále.

## Poslat žádost o odhlášení
Když chcete toosign out hello uživatele z vaší aplikace, není dostatek tooclear soubory cookie vaší aplikace nebo v opačném případě ukončení relace uživatele hello. Také je nutné přesměrovat hello uživatele toohello v2.0 koncový bod toosign se. Pokud to neuděláte, hello znovu ověří se uživatel tooyour aplikace bez nutnosti zadávat své přihlašovací údaje znovu, protože budou mít platný jedné přihlašovací relace s koncovým bodem v2.0 hello.

Můžete přesměrovat uživatele toohello hello `end_session_endpoint` uvedené v dokumentu metadat OpenID Connect hello:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
```

| Parametr | Podmínka | Popis |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | Doporučené | Adresa URL Hello, který hello uživatele je přesměrovaného tooafter úspěšně odhlásit. Pokud hello parametr neuvedete, zobrazí se uživatelské hello obecnou zprávu, která je generován koncového bodu v2.0 hello. Tato adresa URL se musí shodovat s jedním hello přesměrování, identifikátory URI zaregistrované pro svoji aplikaci v portálu pro registraci aplikace hello.  |

## Jednotné odhlašování
Při přesměrování hello uživatele toohello `end_session_endpoint`, koncového bodu v2.0 hello vymaže relace hello uživatele z prohlížeče hello. Hello uživatel však stále může být podepsané tooother aplikace, které používají účty Microsoft pro ověřování. tooenable tyto aplikace toosign hello uživatele se současně, koncového bodu v2.0 hello odešle požadavek HTTP GET toohello, zaregistrovaný `LogoutUrl` všech aplikací hello tento uživatel hello je aktuálně přihlášený do. Aplikace musí odpovědět toothis požadavek zrušením jakékoli relace, který identifikuje hello uživatele a vrácení `200` odpovědi.  Pokud chcete v aplikaci toosupport jednotné přihlašování odhlašování, je nutné implementovat, `LogoutUrl` v kódu aplikace.  Můžete nastavit hello `LogoutUrl` z portálu pro registraci aplikace hello.

## Diagram protokolu: získání tokenu
Mnoho webových aplikací potřebovat toonot pouze přihlašovací hello uživatele v, ale také tooaccess webové služby jménem uživatele hello pomocí OAuth. Tento scénář kombinuje OpenID Connect pro ověřování uživatelů při získávání současně o autorizační kód, které můžete použít tooget přístup tokeny, pokud používáte toku kódu autorizace OAuth hello.

Hello úplné OpenID Connect přihlášení a tok tokenu pořízení vypadá podobně jako toohello další diagram. Jsme popisují každý krok podrobně v dalších částech článku hello hello.

![Protokolu OpenID Connect: získání tokenu](../../media/active-directory-v2-flows/convergence_scenarios_webapp_webapi.png)

## Získat přístupové tokeny
tooacquire přístupových tokenů, upravte hello požádat o přihlášení:

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e        // Your registered Application ID
&response_type=id_token%20code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F       // Your registered redirect URI, URL encoded
&response_mode=form_post                              // 'query', 'form_post', or 'fragment'
&scope=openid%20                                      // Include both 'openid' and scopes that your app needs  
offline_access%20                                         
https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345                                          // Any value, provided by your app
&nonce=678910                                         // Any value, provided by your app
```

> [!TIP]
> Klikněte na následující odkaz tooexecute hello tuto žádost. Po přihlášení, je váš prohlížeč přesměrovaného toohttps://localhost/myapp/ s ID token a kódu v panelu Adresa hello. Všimněte si, že tento požadavek používá `response_mode=query` (pouze pro demonstrační účely). Doporučujeme vám, že používáte `response_mode=form_post`.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token%20code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345&nonce=678910" target="_blank">https://Login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>
> 
> 

Zahrnutím obory oprávnění v žádosti o hello a pomocí `response_type=id_token code`, koncového bodu v2.0 hello zajistí, že hello uživatel souhlasí toohello oprávnění uvedené v hello `scope` parametr dotazu. Vrátí autorizační kód tooyour aplikace tooexchange pro přístupový token.

### Úspěšná odpověď
Úspěšná odpověď z pomocí `response_mode=form_post` vypadá podobně jako tento:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Parametr | Popis |
| --- | --- |
| požadavku id_token |Hello ID token, který hello požadované aplikace. Můžete použít hello ID tokenu tooverify hello identitu uživatele a zahájit relaci s hello uživatele. Další informace o ID tokeny a jejich obsah najdete v hello [koncového bodu v2.0 tokeny odkaz](active-directory-v2-tokens.md). |
| Kód |autorizační kód Hello hello požadované aplikace. Hello aplikace můžete používat hello autorizační kód toorequest přístupový token hello cílový prostředek. Je velmi krátkodobou autorizační kód. Autorizační kód se obvykle vyprší během přibližně 10 minut. |
| state |Pokud parametr stavu je zahrnuta v žádosti o hello, hello stejnou hodnotu by se zobrazit v odpovědi hello. Hello aplikace by měla ověřte, zda jsou identické hodnoty stavu hello v hello žádostí a odpovědí. |

### Chybové odpovědi
Chybové odpovědi může také poslat identifikátor URI pro přesměrování toohello tak, že hello aplikace můžete je správně zpracovat. Chybnou odpověď vypadá takto:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parametr | Popis |
| --- | --- |
| error |Řetězec kódu chyby, kterou můžete použít tooclassify typy chyb, ke kterým došlo, a tooreact tooerrors. |
| error_description |Konkrétní chybová zpráva, která vám může pomoct identifikovat hello hlavní příčinu chyby ověřování. |

Popis možných kódů chyb a klienta doporučenou odpovědí najdete v tématu [kódy chyb pro chyb koncový bod autorizace](#error-codes-for-authorization-endpoint-errors).

Pokud máte autorizační kód a ID token, můžete hello uživatele přihlásit a získat tokeny přístupu jejich jménem. uživatel hello toosign v, musíte ověřit hello ID token [přesně tak, jak je popsáno](#validate-the-id-token). tooget přístupových tokenů, postupujte podle kroků hello popsaných v našich [dokumentace k protokolu OAuth](active-directory-v2-protocols-oauth-code.md#request-an-access-token).
