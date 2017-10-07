---
title: "Webové přihlášení s OpenID Connect - Azure AD B2C | Microsoft Docs"
description: "Vytváření webové aplikace pomocí Azure Active Directory hello implementace ověřovacího protokolu OpenID Connect hello"
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: 21d420c8-3c10-4319-b681-adf2e89e7ede
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: 89e9cfa28e4e5c34304aea355cca2dd0c4b42abc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-web-sign-in-with-openid-connect"></a>Azure Active Directory B2C: Přihlášení Web s OpenID Connect
OpenID Connect je ověřovací protokol, nástavbou OAuth 2.0, které můžou být použité toosecurely přihlášení uživatelů tooweb aplikace. Pomocí Azure Active Directory B2C hello (Azure AD B2C) implementaci OpenID Connect, můžete externí registrace, přihlášení a prostředí pro další správu identit ve vaší webové aplikace tooAzure služby Active Directory (Azure AD). Tento průvodce vám ukáže, jak toodo Ano způsobem nezávislé na jazyku. Popisuje, jak toosend a přijímat zprávy HTTP bez použití některé z našich knihovny open-source.

[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) rozšiřuje hello OAuth 2.0 *autorizace* protokol pro použití jako *ověřování* protokolu. To vám umožní tooperform jednotné přihlašování pomocí OAuth. Zavádí koncepci hello *ID token*, což je token zabezpečení, který umožňuje hello klienta tooverify hello identitu uživatele hello a získat základní profil informace o uživateli hello.

Vzhledem k tomu, že ji rozšiřuje OAuth 2.0, taky umožňuje získat toosecurely aplikace *přístup tokeny*. Můžete použít access_tokens tooaccess prostředky, které jsou zabezpečeny [serveru ověřování](active-directory-b2c-reference-protocols.md#the-basics). Doporučujeme OpenID Connect, pokud vytváříte webovou aplikaci, která je hostovaná na serveru a přístupných prostřednictvím prohlížeče. Pokud chcete tooadd identity management tooyour mobilní nebo desktopové aplikace pomocí Azure AD B2C, měli byste použít [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) místo OpenID Connect.

Azure AD B2C rozšiřuje hello standardní OpenID Connect toodo protokol více než jednoduché ověřování a autorizace. Zavádí hello [zásad parametr](active-directory-b2c-reference-policies.md), což vám umožní toouse OpenID Connect tooadd uživatelského prostředí – například registrace, přihlašování a správy profilů – tooyour aplikace. Zde jsme ukazují, jak toouse OpenID Connect a zásady tooimplement každá z těchto vyskytne v webových aplikací. Také ukážeme jak tooget přístupových tokenů pro přístup k webovým rozhraním API.

Hello příklad HTTP požadavků v další části hello používat naše ukázka adresáři B2C, fabrikamb2c.onmicrosoft.com, a také naše ukázková aplikace, https://aadb2cplayground.azurewebsites.net a zásady. Jste volné tootry out hello sami požadavky s využitím těchto hodnot, nebo je můžete nahradit vlastními.
Zjistěte, jak příliš[získat klienta B2C, aplikace a zásady](#use-your-own-b2c-directory).

## <a name="send-authentication-requests"></a>Odeslání žádosti o ověření
Když webové aplikace musí tooauthenticate hello uživatele a spustit zásadu, se přímá hello uživatele toohello `/authorize` koncový bod. Toto je část interaktivní hello hello toku, kde hello uživatel provede akci, v závislosti na zásadách hello.

V této žádosti o hello klient určuje hello oprávnění, že tato služba vyžaduje tooacquire od uživatele hello v hello `scope` parametr a hello tooexecute zásad v hello `p` parametr. Tři příklady jsou uvedeny v následující části (s zalomení řádků čitelnější), hello každý pomocí jiné zásady. tooget chování pro každou žádost o tom, jak funguje, zkuste žádost hello vkládání do prohlížeče a jejím spuštěním.

#### <a name="use-a-sign-in-policy"></a>Použít zásadu přihlášení
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>Použít zásadu registrace
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Pomocí zásady úpravy profilu
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| Parametr | Povinné? | Popis |
| --- | --- | --- |
| client_id |Požaduje se |Hello aplikace ID této hello [portál Azure](https://portal.azure.com/) přiřazené tooyour aplikace. |
| response_type |Požaduje se |Typ odpovědi Hello, které musí obsahovat ID token pro OpenID Connect. Pokud vaše webová aplikace také musí tokeny pro volání webového rozhraní API, můžete použít `code+id_token`, jak jsme jste na jednom místě. |
| redirect_uri |Doporučené |Hello `redirect_uri` parametr vaší aplikace, kde můžete odesílat a přijímat aplikace odpovědi ověřování. Se musí přesně shodovat s jedním z hello `redirect_uri` parametry, které jste zaregistrovali hello portálu, s tím rozdílem, že musí být kódovaná adresou URL. |
| Obor |Požaduje se |Seznam obory oddělených mezerami. Hodnota jeden obor označuje tooAzure AD obě oprávnění, které jsou požadovány. Hello `openid` oboru označuje toosign oprávnění v hello uživatele a získání dat o hello uživatele v podobě hello ID tokenů (Další toocome na tomto dále v článku hello). Hello `offline_access` obor je volitelné pro webové aplikace. Označuje, že vaše aplikace bude potřebovat *obnovovací token* pro tooresources dlohotrvající přístup. |
| response_mode |Doporučené |Hello metoda, která by měla být použité toosend hello výsledné autorizační kód back tooyour aplikace. Může být buď `query`, `form_post`, nebo `fragment`.  Hello `form_post` režim odpovědi se doporučuje pro zvýšení zabezpečení. |
| state |Doporučené |Hodnota součástí hello požadavek, který je také vrácený v odpovědi tokenu hello. Může být řetězec o délce veškerý obsah, který chcete. Náhodně generované jedinečné hodnoty se obvykle používá pro prevence útoků padělání požadavku posílaného mezi weby. Stav Hello je také použít tooencode informace o stavu hello uživatele v aplikaci hello předtím, než požadavek na ověření hello došlo k chybě, jako je například hello stránky, které byly na. |
| hodnotu Nonce |Požaduje se |Hodnota, zahrnuté v požadavku hello (vygenerovaný hello aplikací), který bude zahrnut v hello výsledný token ID jako deklarace identity. aplikace Hello můžete pak ověřte, že tato hodnota toomitigate opětovného přehrání tokenu útoků. Hodnota Hello je obvykle náhodnou jedinečného řetězce, který lze použít tooidentify hello původ hello požadavku. |
| P |Požaduje se |Hello zásady, které budou spuštěny. Je hello název zásady, který je vytvořen v svého klienta B2C. Hodnota názvu Hello zásad by měl začínat obráceným `b2c\_1\_`. Další informace o zásadách a hello [rozšiřitelném rozhraní zásad](active-directory-b2c-reference-policies.md). |
| řádku |Nepovinné |Typ Hello interakce s uživatelem, který je vyžadován. Hello jediná platná hodnota v tuto chvíli je `login`, které vynutí hello uživatele tooenter přihlašovacích údajů tohoto požadavku. Jednotné přihlašování se neprojeví. |

V tomto okamžiku hello uživateli se zobrazí výzva pracovní postup pro zásady toocomplete hello. To může zahrnovat uživatele hello zadáním uživatelského jména a hesla, přihlášení pomocí identitu sociálních registrací hello adresář, nebo jakékoli jiné číslo kroků, v závislosti na tom, jak je definována zásada hello.

Po dokončení hello zásad hello uživatele Azure AD vrátí tooyour aplikace na odpovědi na hello uvedené `redirect_uri` parametr hello způsobem, který je uveden v hello `response_mode` parametr. pro každou z předchozích případech nezávislé hello zásad, která se provedla hello je hello stejné Hello odpovědi.

Úspěšná odpověď pomocí `response_mode=fragment` bude vypadat:

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametr | Popis |
| --- | --- |
| požadavku id_token |Hello ID token, který hello požadované aplikace. Můžete použít hello ID tokenu tooverify hello identitu uživatele a zahájit relaci s hello uživatele. Další informace o ID tokeny a jejich obsah jsou součástí hello [odkaz tokenu Azure AD B2C](active-directory-b2c-reference-tokens.md). |
| Kód |Hello autorizační kód hello aplikaci vyžaduje, pokud jste použili `response_type=code+id_token`. pro cílový prostředek, můžete použít aplikaci Hello hello autorizační kód toorequest přístupový token. Kódy ověřování jsou velmi krátkodobou. Obvykle se jejich platnost vyprší po přibližně 10 minut. |
| state |Pokud `state` parametr je zahrnuta v žádosti o hello hello stejnou hodnotu by se měla objevit v odpovědi hello. Hello aplikace by měl ověřit, že hello `state` hodnoty v hello žádosti a odpovědi jsou identické. |

Chybové odpovědi lze je také odeslat toohello `redirect_uri` parametr tak, že aplikace hello můžete správně zpracovat:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametr | Popis |
| --- | --- |
| error |Kód chyby řetězec, který lze použít tooclassify typů chyb, které nastat a který lze použít tooreact tooerrors. |
| error_description |Konkrétní chybová zpráva, která může pomoci vývojář identifikovat hello hlavní příčinu chyby ověřování. |
| state |Viz úplný popis hello v první tabulce hello v této části. Pokud `state` parametr je zahrnuta v žádosti o hello hello stejnou hodnotu by se měla objevit v odpovědi hello. Hello aplikace by měl ověřit, že hello `state` hodnoty v hello žádosti a odpovědi jsou identické. |

## <a name="validate-hello-id-token"></a>Ověřit hello ID token
Právě přijetí tokenu ID není dostatek tooauthenticate hello uživatele. Musíte ověřit podpis tokenu ID hello a ověřit hello deklarace identity ve hello tokenu podle požadavků vaší aplikace. Používá Azure AD B2C [webové tokeny JSON (Jwt)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) a veřejný klíč kryptografie toosign tokeny a ověřte, zda je platný.

Existuje mnoho knihovny open-source, které jsou k dispozici pro ověření tokeny Jwt, v závislosti na vaší jazykové předvolby. Doporučujeme, abyste zkoumat těchto možností než implementace vlastní logiky ověřování. Zde Hello informace budou užitečné v nad tím, jak používat tooproperly tyto knihovny.

Azure AD B2C má OpenID Connect metadata koncový bod, který umožňuje aplikaci toofetch informace o Azure AD B2C za běhu. Tyto informace zahrnují koncových bodů, obsah tokenu a token podpisových klíčů. Je dokument metadat JSON pro každé zásady v svého klienta B2C. Například dokumentu metadat hello hello `b2c_1_sign_in` zásad v `fabrikamb2c.onmicrosoft.com` se nachází na:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Jedna z vlastností hello tohoto dokumentu konfigurace je `jwks_uri`, jehož hodnota pro stejné zásady by hello:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`.

toodetermine zásadu, které byl použit v podpis tokenu ID (a z kde toofetch hello metadata), máte dvě možnosti. Nejprve je název zásady hello součástí hello `acr` deklarací identity v tokenu ID hello. Informace o tom, jak tooparse hello deklarací z tokenu ID najdete v tématu hello [odkaz tokenu Azure AD B2C](active-directory-b2c-reference-tokens.md). Jinak se zásady hello tooencode v hello hodnotu hello `state` parametr při vydání hello požadavku a pak dekódovat toodetermine zásadu, které byl použit. Buď metoda je platná.

Poté, co jste získali hello dokument metadat z hello OpenID Connect metadat koncového bodu, můžete použít hello RSA 256 veřejných klíčů (které jsou umístěné na tento koncový bod) toovalidate hello podpis tokenu ID hello. Může být více klíčů uvedený na tento koncový bod v libovolném bodě v čase, každý se identifikovanou pomocí `kid` deklarací identity. Hello hlavička hello ID tokenu také obsahuje `kid` deklarace identity, která označuje, která z těchto klíčů byla použité toosign hello ID token. Další informace najdete v tématu hello [odkaz tokenu Azure AD B2C](active-directory-b2c-reference-tokens.md) (hello části na [ověřování tokenů](active-directory-b2c-reference-tokens.md#token-validation), zejména).
<!--TODO: Improve hello information on this-->

Po jste ověření podpisu hello hello ID tokenu, existuje několik deklarace identity, je nutné, aby tooverify. Například:

* Měli byste ověřit, hello `nonce` deklarace identity tooprevent opětovného přehrání tokenu útoky. Její hodnota musí být zadaný v hello požádat o přihlášení.
* Měli byste ověřit, hello `aud` deklarace identity tooensure, který hello ID token byl vydán pro vaši aplikaci. Její hodnota musí být ID aplikace hello vaší aplikace.
* Měli byste ověřit, hello `iat` a `exp` nevypršela platnost tooensure, který hello ID tokenu deklarací identity.

Existují také několik další ověření, které byste měli provést. Tyto jsou podrobně popsány v hello [OpenID Connect základní specifikace](http://openid.net/specs/openid-connect-core-1_0.html).  Můžete také toovalidate další deklarace identity, v závislosti na vašem scénáři. Některé běžné ověření patří:

* Zajištění, že který hello uživatele či organizace má zaregistrovali do aplikace hello.
* Zajištění hello má správná oprávnění autorizace.
* Zajištění, že došlo k sílu ověřování, jako je například ověřování Azure Multi-Factor Authentication.

Další informace o deklaracích identity hello v tokenu ID najdete v tématu hello [odkaz tokenu Azure AD B2C](active-directory-b2c-reference-tokens.md).

Po ověření tokenu ID hello můžete začít relaci s hello uživatelem. Hello deklarací identity můžete použít v hello ID tokenu tooobtain informace o uživateli hello ve vaší aplikaci. Používá tyto informace zahrnují zobrazení, záznamy a autorizace.

## <a name="get-a-token"></a>Získání tokenu
Pokud je třeba vaší webové aplikace tooonly provést zásady, můžete přeskočit hello několik dalších částech. Tyto části jsou použít pouze tooweb aplikace, které potřebují toomake ověření volání tooa webového rozhraní API a také jsou chráněné službou Azure AD B2C.

Lze uplatnit hello autorizační kód, který jste získali (pomocí `response_type=code+id_token`) pro token toohello požadovaného prostředku odesíláním `POST` požadavku toohello `/token` koncový bod. V současné době hello pouze prostředek, který můžete požádat o token pro je vaše aplikace vlastní back endové webové rozhraní API. Hello konvence pro požadování tokenu tooyourself je toouse ID klienta aplikace jako obor hello:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>

```

| Parametr | Povinné? | Popis |
| --- | --- | --- |
| P |Požaduje se |Hello zásady, které byly použité tooacquire hello autorizační kód. V této žádosti nelze použít jinou zásadu. Všimněte si, že přidáte tento parametr řetězce dotazu toohello, není toohello `POST` textu. |
| client_id |Požaduje se |Hello aplikace ID této hello [portál Azure](https://portal.azure.com/) přiřazené tooyour aplikace. |
| grant_type |Požaduje se |Hello typ udělení, který musí být `authorization_code` pro tok autorizačního kódu hello. |
| Obor |Doporučené |Seznam obory oddělených mezerami. Hodnota jeden obor označuje tooAzure AD obě oprávnění, které jsou požadovány. Hello `openid` oboru označuje toosign oprávnění v hello uživatele a získání dat o hello uživatele v podobě hello požadavku id_token parametrů. Dá se použít tooget tokeny tooyour aplikace vlastní back endové webové rozhraní API, která je reprezentována hello stejným ID aplikace jako klient hello. Hello `offline_access` oboru označuje, že vaše aplikace budete potřebovat obnovovací token pro tooresources dlohotrvající přístup. |
| Kód |Požaduje se |Hello autorizační kód, který jste získali v prvním větev hello hello toku. |
| redirect_uri |Požaduje se |Hello `redirect_uri` parametr hello aplikace, kterou jste dostali hello autorizační kód. |
| tajný klíč client_secret |Požaduje se |tajný klíč aplikace Hello, který jste vygenerovali v hello [portál Azure](https://portal.azure.com/). Tento tajný klíč aplikace je důležité zabezpečení artefakt. Měli byste uložit ji bezpečně na vašem serveru. Musí také otočit tento tajný klíč klienta v pravidelných intervalech. |

Úspěšné odpovědi tokenu vypadá takto:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parametr | Popis |
| --- | --- |
| not_before |Hello čas, na které hello token je považován za platný v čase epoch. |
| token_type |Hodnota Hello typ tokenu. Hello pouze typ, který podporuje Azure AD je `Bearer`. |
| access_token |Hello podepsaný token JWT, které jste žádali. |
| Obor |Hello oborů, pro které hello token je platný. Ty lze použít pro ukládání do mezipaměti tokeny pro pozdější použití. |
| expires_in |Délka Hello dobu, po kterou hello přístupový token je platný (v sekundách). |
| refresh_token |Aktualizace tokenu OAuth 2.0. Hello aplikace můžete použít tento token tooacquire další tokeny po vypršení platnosti tokenu aktuální hello. Aktualizujte tokeny je dlouhodobé a může být použité tooretain přístup tooresources pro dlouhou dobu. Další podrobnosti najdete v části toohello [odkaz tokenu B2C](active-directory-b2c-reference-tokens.md). Všimněte si, co musí jste použili hello oboru `offline_access` v hello autorizace a token požadavků v pořadí tooreceive token obnovení. |

Chybové odpovědi vypadat podobně jako:

```
{
    "error": "access_denied",
    "error_description": "hello user revoked access toohello app.",
}
```

| Parametr | Popis |
| --- | --- |
| error |Kód chyby řetězec, který lze použít tooclassify typů chyb, které nastat a který lze použít tooreact tooerrors. |
| error_description |Konkrétní chybová zpráva, která může pomoci vývojář identifikovat hello hlavní příčinu chyby ověřování. |

## <a name="use-hello-token"></a>Použití tokenu hello
Teď, když jste úspěšně získat přístupový token, můžete použít hello token v žádosti o tooyour back endové webové rozhraní API včetně v hello `Authorization` hlavičky:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-hello-token"></a>Aktualizovat hello token
ID tokeny jsou krátkodobou. Je nutné aktualizovat po vypršení jejich platnosti toocontinue je možné tooaccess prostředky. Můžete to provést tak, že zadáte jiný `POST` požadavku toohello `/token` koncový bod. Tuto chvíli zadejte hello `refresh_token` parametr místo hello `code` parametr:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Parametr | Požaduje se | Popis |
| --- | --- | --- |
| P |Požaduje se |Hello zásady, které byly použité tooacquire hello původní obnovovací token. V této žádosti nelze použít jinou zásadu. Všimněte si, že přidáte tento parametr řetězce dotazu toohello, toohello textu POST. |
| client_id |Požaduje se |Hello aplikace ID této hello [portál Azure](https://portal.azure.com/) přiřazené tooyour aplikace. |
| grant_type |Požaduje se |Hello typ udělení, který musí být token obnovení pro tuto větev toku kódu autorizace hello. |
| Obor |Doporučené |Seznam obory oddělených mezerami. Hodnota jeden obor označuje tooAzure AD obě oprávnění, které jsou požadovány. Hello `openid` oboru označuje toosign oprávnění v hello uživatele a získání dat o hello uživatele v podobě hello ID tokenů. Dá se použít tooget tokeny tooyour aplikace vlastní back endové webové rozhraní API, která je reprezentována hello stejným ID aplikace jako klient hello. Hello `offline_access` oboru označuje, že vaše aplikace budete potřebovat obnovovací token pro tooresources dlohotrvající přístup. |
| redirect_uri |Doporučené |Hello `redirect_uri` parametr hello aplikace, kterou jste dostali hello autorizační kód. |
| refresh_token |Požaduje se |Hello původní aktualizace token, který jste získali v druhé větev hello hello toku. Všimněte si, co musí jste použili hello oboru `offline_access` v hello autorizace a token požadavků v pořadí tooreceive token obnovení. |
| tajný klíč client_secret |Požaduje se |tajný klíč aplikace Hello, který jste vygenerovali v hello [portál Azure](https://portal.azure.com/). Tento tajný klíč aplikace je důležité zabezpečení artefakt. Měli byste uložit ji bezpečně na vašem serveru. Musí také otočit tento tajný klíč klienta v pravidelných intervalech. |

Úspěšné odpovědi tokenu vypadá takto:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parametr | Popis |
| --- | --- |
| not_before |Hello čas, na které hello token je považován za platný v čase epoch. |
| token_type |Hodnota Hello typ tokenu. Hello pouze typ, který podporuje Azure AD je `Bearer`. |
| access_token |Hello podepsaný token JWT, které jste žádali. |
| Obor |Hello obor, který hello token je platný, který může být použit pro ukládání do mezipaměti tokeny pro pozdější použití. |
| expires_in |Délka Hello dobu, po kterou hello přístupový token je platný (v sekundách). |
| refresh_token |Aktualizace tokenu OAuth 2.0. Hello aplikace můžete použít tento token tooacquire další tokeny po vypršení platnosti tokenu aktuální hello.  Aktualizujte tokeny je dlouhodobé a může být použité tooretain přístup tooresources pro dlouhou dobu. Další podrobnosti najdete v části toohello [odkaz tokenu B2C](active-directory-b2c-reference-tokens.md). |

Chybové odpovědi vypadat podobně jako:

```
{
    "error": "access_denied",
    "error_description": "hello user revoked access toohello app.",
}
```

| Parametr | Popis |
| --- | --- |
| error |Kód chyby řetězec, který lze použít tooclassify typů chyb, které nastat a který lze použít tooreact tooerrors. |
| error_description |Konkrétní chybová zpráva, která může pomoci vývojář identifikovat hello hlavní příčinu chyby ověřování. |

## <a name="send-a-sign-out-request"></a>Poslat žádost o odhlášení
Když chcete toosign hello uživatele mimo aplikaci hello, je nedostatek tooclear soubory cookie vaší aplikace nebo v opačném případě ukončení relace hello hello uživatele. Také je nutné přesměrovat hello uživatele tooAzure AD toosign out. Pokud jsou selhání toodo tak, hello uživatel může být schopný tooreauthenticate tooyour aplikace bez opětovného zadávání svých přihlašovacích údajů. Je to proto, že budou mít platný jedné přihlášení relace s Azure AD.

Jednoduše můžete přesměrovat uživatele toohello hello `end_session` koncový bod, který je uveden v dokumentu metadat OpenID Connect hello popsáno výše v hello "Ověřit hello ID token" části:

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parametr | Povinné? | Popis |
| --- | --- | --- |
| P |Požaduje se |zásady Hello chcete toouse toosign hello uživatele z vaší aplikace. |
| post_logout_redirect_uri |Doporučené |Hello tento uživatel hello adresa URL by měla být přesměrovaného tooafter úspěšné odhlášení. Pokud není součástí, Azure AD B2C zobrazí obecná zpráva hello uživatele. |

> [!NOTE]
> I když odkazovat hello uživatele toohello `end_session` koncový bod vymaže některé hello jeden přihlašování stavu uživatele v Azure AD B2C, nebude ho přihlaste hello uživatele mimo jejich relace sociálních identity zprostředkovatele (IDP). Pokud hello uživatel vybere hello stejné IDP při následné přihlášení, že bude mít k novému ověření, bez zadávání přihlašovacích údajů. Pokud chce uživatel toosign mimo aplikaci B2C, je však nemusí znamenat, že chtějí toosign mimo jejich účtu sítě Facebook. Ale v případě hello místních účtů hello uživatelské relace bude ukončen správně.
> 
> 

## <a name="use-your-own-b2c-tenant"></a>Použití vlastního klienta B2C
Pokud chcete použít tootry tyto požadavky pro sami, musíte nejprve provést tyto tři kroky a potom můžete nahradit hello ukázkové hodnoty vlastními popsané výše:

1. [Vytvoření klienta B2C](active-directory-b2c-get-started.md)a použijte hello název vašeho klienta v žádostech o hello.
2. [Vytvoření aplikace](active-directory-b2c-app-registration.md) tooobtain identifikátor aplikace. Zahrňte webové aplikace nebo webové rozhraní API ve vaší aplikaci. Volitelně můžete vytvořte tajný klíč aplikace.
3. [Vytvoření vlastních zásad](active-directory-b2c-reference-policies.md) tooobtain názvy zásad.

