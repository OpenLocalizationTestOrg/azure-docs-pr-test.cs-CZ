---
title: "aaaSecure jednostránkové aplikace pomocí implicitního toku hello Azure AD v2.0 | Microsoft Docs"
description: "Vytváření webových aplikací pomocí služby Azure AD v2.0 provádění hello implicitní tok pro jednostránkové aplikace."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 3605931f-dc24-4910-bb50-5375defec6a8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 2cdce4eee88be4af54966d15204b79fa4992a58e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# v2.0 protokoly - SPA pomocí implicitního toku hello
S koncovým bodem v2.0 hello můžete přihlásit uživatele do vaší jednostránkové aplikace s osobní i pracovní nebo školní účty od společnosti Microsoft.  Jednostránkové a jinými aplikacemi JavaScript, které hlavně v prohlížeči řez pár zajímavé vyzve, když ho spouští dodává tooauthentication:

* Vlastnosti zabezpečení Hello tyto aplikace se výrazně liší od tradiční serverových webových aplikací.
* Mnoho serverů autorizace & zprostředkovatelů identity nepodporují požadavků CORS.
* Přesměrování prohlížeče celou stránku mimo aplikace hello stát zvlášť invazivní toohello uživatelské prostředí.

Pro tyto aplikace (vezměte v úvahu: AngularJS, Ember.js, React.js, atd) Azure AD podporuje toku OAuth 2.0 implicitní Grant hello.  implicitní tok Hello je popsaná v hello [specifikace OAuth 2.0](http://tools.ietf.org/html/rfc6749#section-4.2).  Jeho primární výhody je, že umožňuje hello aplikace tooget tokeny z Azure AD bez přihlašovacích údajů systému exchange back-end serveru.  To umožňuje toosign aplikace hello v hello uživatele, Udržovat relaci a získat tokeny tooother webových rozhraní API vše v rámci kód JavaScript klienta hello.  Při používání hello implicitního toku – konkrétně přibližně se několik tootake důležité informace důležité zabezpečení v úvahu [klienta](http://tools.ietf.org/html/rfc6749#section-10.3) a [vystupovat jako jiný uživatel](http://tools.ietf.org/html/rfc6749#section-10.3).

Pokud chcete toouse hello implicitního toku a Azure AD tooadd ověřování tooyour JavaScript aplikace, doporučujeme používat naše knihovna JavaScript s otevřeným zdrojem, [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js).  Nejsou k dispozici několik kurzy AngularJS [sem](active-directory-appmodel-v2-overview.md#getting-started) toohelp začít pracovat.  

Pokud si přejete toouse knihovny do jediné stránce aplikace a odesílání zpráv protokolu sami, ale postupujte hello obecné kroky.

> [!NOTE]
> Ne všechny scénáře Azure Active Directory a funkce jsou podporovány koncového bodu v2.0 hello.  toodetermine Pokud byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).
> 
> 

## Diagram protokolu
Hello celý implicitní přihlašovací v toku vypadá přibližně takto – každý hello kroky jsou podrobně popsány v níže.

![OpenId Connect plaveckých drah](../../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## Odeslání požadavku na hello přihlášení
přihlašovací tooinitially hello uživatele do vaší aplikace, můžete odeslat [OpenID Connect](active-directory-v2-protocols-oidc.md) požadavek ověřování a získání přístupu `id_token` z koncového bodu v2.0 hello:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token+token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&response_mode=fragment
&state=12345
&nonce=678910
```

> [!TIP]
> Kliknutím na odkaz hello níže tooexecute tuto žádost! Po přihlášení, by měl být prohlížeči přesměrován příliš`https://localhost/myapp/` s `id_token` v panelu Adresa hello.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token+token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910" target="_blank">https://Login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>
> 
> 

| Parametr |  | Popis |
| --- | --- | --- |
| Klienta |Požadované |Hello `{tenant}` hodnota v cestě hello hello žádosti může být použité toocontrol, který se můžete přihlásit do aplikace hello.  Hello povolené hodnoty jsou `common`, `organizations`, `consumers`a identifikátory klientů.  Další podrobnosti naleznete v [protokolu Základy](active-directory-v2-protocols.md#endpoints). |
| client_id |Požadované |Hello Id aplikace tohoto portálu registrace hello ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) přiřazené vaší aplikace. |
| response_type |Požadované |Musí zahrnovat `id_token` pro OpenID Connect přihlášení.  Může také zahrnovat hello response_type `token`. Pomocí `token` zde vám umožní vaší aplikace tooreceive přístupový token okamžitě z hello zajistí autorizaci koncového bodu bez nutnosti toomake druhý toohello žádost o autorizaci koncového bodu.  Pokud používáte hello `token` response_type, hello `scope` parametr musí obsahovat obor, která určuje, které prostředků tooissue hello token pro. |
| redirect_uri |Doporučená |Hello redirect_uri vaší aplikace, kde můžete odesílat a přijímat aplikace odpovědi ověřování.  Se musí přesně shodovat s jedním z hello redirect_uris, registrován v hello portál, s výjimkou musí být kódovaná adresou url. |
| Obor |Požadované |Seznam obory oddělených mezerami.  Pro OpenID Connect, musí obsahovat hello oboru `openid`, což znamená, že oprávnění "Přihlásit" toohello v hello souhlasu uživatelského rozhraní.  Volitelně můžete také tooinclude hello `email` nebo `profile` [obory](active-directory-v2-scopes.md) pro získání přístupu tooadditional uživatelská data.  V této žádosti o pro vyžádání souhlasu toovarious prostředky může také zahrnovat další obory. |
| response_mode |Doporučená |Určuje metodu hello, který by měl být použité toosend hello výsledný token back tooyour aplikace.  Musí být `fragment` pro implicitní tok hello. |
| state |Doporučená |Hodnota součástí hello požadavek, který bude vrácen také v odpovědi tokenu hello.  Může být řetězec o délce veškerý obsah, který chcete.  Náhodně generované jedinečné hodnoty se obvykle používá u [prevence útoků padělání požadavku posílaného mezi weby](http://tools.ietf.org/html/rfc6749#section-10.12).  Stav Hello je také použít tooencode informace o stavu hello uživatele v aplikaci hello před došlo k požadavku hello ověřování, jako je například stránku hello nebo zobrazení, které byly na. |
| hodnotu Nonce |Požadované |Hodnota, zahrnuté v žádosti o hello vygenerovaný hello aplikací, který bude zahrnut v výsledné požadavku id_token hello jako deklarace identity.  aplikace Hello můžete pak ověřte, že tato hodnota toomitigate opětovného přehrání tokenu útoků.  Hodnota Hello je obvykle náhodnou jedinečného řetězce, které můžou být použité tooidentify hello původ hello požadavku. |
| řádku |Volitelné |Označuje typ hello interakce s uživatelem, který je vyžadován.  Hello platné jsou pouze hodnoty v tuto chvíli "přihlášení", 'none' a 'souhlas'.  `prompt=login`bude platnost hello tooenter uživatele při jejich přihlašovacích údajů tohoto požadavku negace jednotné přihlašování na.  `prompt=none`je hello opačné – zajistí, že tento hello uživatel není uveden s žádné jakkoli interaktivní výzvu.  Pokud požadavek hello nemůže být dokončena bez upozornění pomocí jednotného přihlašování, koncového bodu v2.0 hello vrátí chybu.  `prompt=consent`bude aktivační událost hello OAuth souhlas dialogové okno po hello uživatel se přihlásí, dotazem hello uživatele toogrant oprávnění toohello aplikace. |
| login_hint |Volitelné |Můžou být použité toopre výplně hello uživatelského jména nebo e-mailovou adresu pole hello přihlásit stránku hello uživatele, pokud znáte svoje uživatelské jméno předem.  Často aplikace bude používat tento parametr během opětovné ověření, uživatelské jméno hello s z předchozí přihlášení už extrahovat pomocí hello `preferred_username` deklarací identity. |
| domain_hint |Volitelné |Může být jedna z `consumers` nebo `organizations`.  Pokud zahrnuty, přeskočí procesu zjišťování na základě e-mailu hello tento uživatel prochází na hello v2.0 stránku pro přihlášení, úvodní tooa něco víc zjednodušený činnost koncového uživatele.  Často aplikace bude používat tento parametr během opětovné ověření extrahováním hello `tid` deklarace identity z požadavku id_token hello.  Pokud hello `tid` deklarace, hodnota je `9188040d-6c67-4c5b-b112-36a304b66dad`, měli byste použít `domain_hint=consumers`.  Jinak použijte `domain_hint=organizations`. |

V tomto okamžiku hello uživatel se bude výzva tooenter své přihlašovací údaje a dokončení hello ověřování.  Hello koncového bodu v2.0 také zajistí, že hello uživatel souhlasí toohello oprávnění uvedené v hello `scope` parametr dotazu.  Pokud uživatel hello nedala souhlas tooany tato oprávnění, ho požádá uživatele tooconsent hello toohello vyžaduje oprávnění.  Podrobnosti o [oprávnění, souhlasu a víceklientské aplikace jsou tady uvedené](active-directory-v2-scopes.md).

Jakmile uživatel hello ověřuje a uděluje souhlas, koncového bodu v2.0 hello vrátí tooyour aplikace na odpovědi na hello uvedené `redirect_uri`, pomocí hello metody popsané v hello `response_mode` parametr.

#### Úspěšná odpověď
Úspěšná odpověď pomocí `response_mode=fragment` a `response_type=id_token+token` vypadá jako následující hello pomocí konců řádků pro čitelnost:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope=https%3a%2f%2fgraph.microsoft.com%2fmail.read 
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
```

| Parametr | Popis |
| --- | --- |
| access_token |Zahrnuté v případě `response_type` zahrnuje `token`. Hello přístupový token, který hello aplikace vyžaduje, v takovém případě pro hello Microsoft Graph.  přístupový token Hello by neměly být dekódovat, nebo v opačném případě prověřovány, lze považovat za neprůhledný řetězec. |
| token_type |Zahrnuté v případě `response_type` zahrnuje `token`.  Bude vždy `Bearer`. |
| expires_in |Zahrnuté v případě `response_type` zahrnuje `token`.  Označuje hello počet sekund, po které hello token je platný, pro ukládání do mezipaměti pro účely. |
| Obor |Zahrnuté v případě `response_type` zahrnuje `token`.  Označuje, pro které hello access_token budou platné rozsahy hello. |
| požadavku id_token |Hello požadavku id_token, který hello požadované aplikace. Můžete použít hello požadavku id_token tooverify hello identitu uživatele a zahájit relaci s hello uživatele.  Další informace o id_tokens a jejich obsah je součástí hello [odkaz tokenu koncový bod v2.0](active-directory-v2-tokens.md). |
| state |Pokud parametr stavu je zahrnuta v žádosti o hello, hello stejnou hodnotu by se zobrazit v odpovědi hello. Hello aplikace by měla ověřte, zda jsou identické hodnoty stavu hello v hello žádostí a odpovědí. |

#### Chybové odpovědi
Chybové odpovědi se taky může odeslat toohello `redirect_uri` tak hello aplikace můžete správně zpracovat:

```
GET https://localhost/myapp/#
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametr | Popis |
| --- | --- |
| error |Řetězec kódu chyby, který lze použít tooclassify typů chyb, ke kterým došlo a může být použité tooreact tooerrors. |
| error_description |Konkrétní chybová zpráva, která může pomoci vývojář identifikovat hello hlavní příčinu chyby ověřování. |

## Ověření požadavku id_token hello
Právě přijetí požadavku id_token není dostatečná uživatel hello tooauthenticate; musíte ověřit podpis hello požadavku id_token a ověřit hello deklarace identity ve hello tokenu podle požadavků vaší aplikace.  koncový bod v2.0 Hello používá [webové tokeny JSON (Jwt)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) a veřejný klíč kryptografie toosign tokeny a ověřte, zda je platný.

Můžete zvolit toovalidate hello `id_token` v kódu klienta, ale běžnou praxí je toosend hello `id_token` tooa back-end serveru a provést ověření hello existuje.  Jste ověření podpisu požadavku id_token hello hello budete několik deklarací identity bude požadované tooverify.  V tématu hello [odkaz tokenu v2.0](active-directory-v2-tokens.md) Další informace, včetně [ověřování tokenů](active-directory-v2-tokens.md#validating-tokens) a [důležité informace o podpisový klíč výměny](active-directory-v2-tokens.md#validating-tokens).  Doporučujeme, abyste tokeny využívající knihovnu analýzy a ověřování – existuje alespoň jeden pro většinu jazyky a platformy.
<!--TODO: Improve hello information on this-->

Také je možné toovalidate další deklarace identity v závislosti na vašem scénáři.  Některé běžné ověření patří:

* Zajištění hello uživatele či organizace má zaregistrovali do aplikace hello.
* Zajistit hello uživatel má správná oprávnění autorizace
* Zajištění sílu ověřování došlo k, jako je vícefaktorové ověřování.

Další informace o deklaracích identity hello v požadavku id_token najdete v tématu hello [odkaz tokenu koncový bod v2.0](active-directory-v2-tokens.md).

Jakmile úplně ověření požadavku id_token hello můžete zahájit relaci s hello uživatele a používat deklarace identity hello v požadavku id_token hello tooobtain. informace o hello uživatele ve vaší aplikaci.  Tyto informace můžete použít pro zobrazení, záznamy, oprávnění atd.

## Získat přístupové tokeny
Teď, když uživatel hello jste přihlášení jednostránkové aplikace, můžete získat přístupové tokeny pro volání webových rozhraní API, které jsou zabezpečené službou Azure AD, jako je například hello [Microsoft Graph](https://graph.microsoft.io).  I v případě, že jste už dostali token pomocí hello `token` response_type, můžete použít tuto metodu tooacquire tokeny tooadditional prostředky bez nutnosti tooredirect hello uživatele toosign akci.

V normálním toku OpenID Connect/OAuth hello by to uděláte tak, že žádost toohello v2.0 `/token` koncový bod.  Koncový bod v2.0 hello však nemá CORS žádosti o podporu, tak, aby provedení AJAX volá tooget a tokeny obnovení je mimo hello otázku.  Místo toho můžete použít hello implicitního toku v nové tokeny skrytá iframe tooget pro jiné webové rozhraní API: 

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment
&state=12345&nonce=678910
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
```

> [!TIP]
> Vyzkoušejte kopírování a vkládání hello níže požadavku do na záložce prohlížeče. (Nezapomeňte tooreplace hello `domain_hint` a hello `login_hint` hodnoty s hello opravte hodnoty pro vaše uživatele)
> 
> 

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910&prompt=none&domain_hint={{consumers-or-organizations}}&login_hint={{your-username}}
```

| Parametr |  | Popis |
| --- | --- | --- |
| Klienta |Požadované |Hello `{tenant}` hodnota v cestě hello hello žádosti může být použité toocontrol, který se můžete přihlásit do aplikace hello.  Hello povolené hodnoty jsou `common`, `organizations`, `consumers`a identifikátory klientů.  Další podrobnosti naleznete v [protokolu Základy](active-directory-v2-protocols.md#endpoints). |
| client_id |Požadované |Hello Id aplikace tohoto portálu registrace hello ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) přiřazené vaší aplikace. |
| response_type |Požadované |Musí zahrnovat `id_token` pro OpenID Connect přihlášení.  Může také obsahovat další response_types, jako například `code`. |
| redirect_uri |Doporučená |Hello redirect_uri vaší aplikace, kde můžete odesílat a přijímat aplikace odpovědi ověřování.  Se musí přesně shodovat s jedním z hello redirect_uris, registrován v hello portál, s výjimkou musí být kódovaná adresou url. |
| Obor |Požadované |Seznam obory oddělených mezerami.  Jak získat tokeny, zahrnout všechny [obory](active-directory-v2-scopes.md) budete vyžadovat pro hello prostředků, které vás zajímají. |
| response_mode |Doporučená |Určuje metodu hello, který by měl být použité toosend hello výsledný token back tooyour aplikace.  Může být jedna z `query`, `form_post`, nebo `fragment`. |
| state |Doporučená |Hodnota součástí hello požadavek, který bude vrácen také v odpovědi tokenu hello.  Může být řetězec o délce veškerý obsah, který chcete.  Náhodně generované jedinečné hodnoty se obvykle používá pro prevence útoků padělání požadavku posílaného mezi weby.  Stav Hello je také použít tooencode informace o stavu hello uživatele v aplikaci hello před došlo k požadavku hello ověřování, jako je například stránku hello nebo zobrazení, které byly na. |
| hodnotu Nonce |Požadované |Hodnota, zahrnuté v žádosti o hello vygenerovaný hello aplikací, který bude zahrnut v výsledné požadavku id_token hello jako deklarace identity.  aplikace Hello můžete pak ověřte, že tato hodnota toomitigate opětovného přehrání tokenu útoků.  Hodnota Hello je obvykle náhodnou jedinečného řetězce, které můžou být použité tooidentify hello původ hello požadavku. |
| řádku |Požadované |Aktualizovat & získávání tokeny ve skryté iframe, měli byste použít `prompt=none` tooensure, který hello iframe není zablokuje na hello v2.0 stránku pro přihlášení a vrátí okamžitě. |
| login_hint |Požadované |Pro aktualizaci a jak získat tokeny v skrytá iframe, je nutné zahrnout hello uživatelské jméno uživatele hello Tento pomocný v pořadí toodistinguish mezi více relací, které uživatel hello může mít k danému bodu v čase. Uživatelské jméno hello může extrahovat z předchozí přihlášení pomocí hello `preferred_username` deklarací identity. |
| domain_hint |Požadované |Může být jedna z `consumers` nebo `organizations`.  Pro aktualizaci a jak získat tokeny v skrytá iframe, je nutné zahrnout hello domain_hint hello požadavku.  Extrahujete hello `tid` deklarace identity z požadavku id_token hello z předchozí toodetermine přihlášení, které toouse hodnotu.  Pokud hello `tid` deklarace, hodnota je `9188040d-6c67-4c5b-b112-36a304b66dad`, měli byste použít `domain_hint=consumers`.  Jinak použijte `domain_hint=organizations`. |

Děkujeme vám toohello `prompt=none` parametr tento požadavek se buď úspěšné nebo nezdaří okamžitě a vrátí tooyour aplikace.  Úspěšná odpověď zašle tooyour aplikace na uvedené hello `redirect_uri`, pomocí hello metody popsané v hello `response_mode` parametr.

#### Úspěšná odpověď
Úspěšná odpověď pomocí `response_mode=fragment` vypadá jako:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read
```

| Parametr | Popis |
| --- | --- |
| access_token |Hello token, který hello požadované aplikace. |
| token_type |Bude vždy `Bearer`. |
| state |Pokud parametr stavu je zahrnuta v žádosti o hello, hello stejnou hodnotu by se zobrazit v odpovědi hello. Hello aplikace by měla ověřte, zda jsou identické hodnoty stavu hello v hello žádostí a odpovědí. |
| expires_in |Jak dlouho hello přístupový token je platný (v sekundách). |
| Obor |Hello obory, které hello přístupový token je platný pro. |

#### Chybové odpovědi
Chybové odpovědi se taky může odeslat toohello `redirect_uri` tak hello aplikace můžete správně zpracovat.  V případě hello `prompt=none`, bude očekávanou chybu:

```
GET https://localhost/myapp/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parametr | Popis |
| --- | --- |
| error |Řetězec kódu chyby, který lze použít tooclassify typů chyb, ke kterým došlo a může být použité tooreact tooerrors. |
| error_description |Konkrétní chybová zpráva, která může pomoci vývojář identifikovat hello hlavní příčinu chyby ověřování. |

Pokud tato chyba se zobrazí v požadavku iframe hello, hello uživatel musí interaktivně přihlásit znovu tooretrieve nový token.  Tento případ můžete zvolit toohandle v jakémkoli způsob, jak má smysl pro vaši aplikaci.

## Aktualizace tokeny
Obě `id_token`s a `access_token`s vyprší po krátkou dobu, tak vaše aplikace musí být připraveny toorefresh tyto tokeny pravidelně.  buď zadejte tokenu toorefresh, můžete provést hello stejném požadavku skrytá iframe výše pomocí hello `prompt=none` parametr chování toocontrol Azure AD.  Pokud chcete, aby tooreceive nový `id_token`, že toouse být `response_type=id_token` a `scope=openid`, a také `nonce` parametr.

## Odhlaste se žádost o odeslání
Hello OpenIdConnect `end_session_endpoint` umožňuje toosend vaší aplikace žádost o toohello v2.0 koncový bod tooend relace uživatele a Vymazat soubory cookie nastavte koncovým bodem v2.0 hello.  toofully přihlášení uživatele z webové aplikace, aplikace by měl ukončení vlastní relaci s hello uživatele (obvykle zaškrtnutím nebo zrušením mezipamětí tokenů odstranit soubory cookie) a pak přesměrovat hello prohlížeči:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/logout?post_logout_redirect_uri=https://localhost/myapp/
```

| Parametr |  | Popis |
| --- | --- | --- |
| Klienta |Požadované |Hello `{tenant}` hodnota v cestě hello hello žádosti může být použité toocontrol, který se můžete přihlásit do aplikace hello.  Hello povolené hodnoty jsou `common`, `organizations`, `consumers`a identifikátory klientů.  Další podrobnosti naleznete v [protokolu Základy](active-directory-v2-protocols.md#endpoints). |
| post_logout_redirect_uri | Doporučená | Hello URL, která hello uživatel má být vrácen, že dokončení tooafter odhlášení. Tato hodnota se musí shodovat s jedním hello přesměrování, identifikátory URI zaregistrované pro aplikace hello. Pokud není zahrnut, hello uživatele zobrazí obecná zpráva koncovým bodem v2.0 hello. |
