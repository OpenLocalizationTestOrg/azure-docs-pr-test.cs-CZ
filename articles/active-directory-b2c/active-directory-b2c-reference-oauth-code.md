---
title: "Tok autorizačního kódu – Azure AD B2C | Microsoft Docs"
description: "Zjistěte, jak toobuild webové aplikace pomocí Azure AD B2C a OpenID Connect ověřovací protokol."
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: c371aaab-813a-4317-97df-b62e2f53d865
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: 6bf9d37310bd45b39bda346441413556f9fd4fdc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-oauth-20-authorization-code-flow"></a>Azure Active Directory B2C: Toku kódu autorizace OAuth 2.0
Udělení autorizačního kódu OAuth 2.0 hello můžete použít v aplikace nainstalované v zařízení toogain přístup tooprotected prostředky, například webových rozhraní API. Pomocí Azure Active Directory B2C hello (Azure AD B2C) provádění OAuth 2.0, můžete přidat registrace, přihlašování a jiných správu identity úloh tooyour mobilních a desktopových aplikací. Tento článek je nezávislé na jazyku. V článku hello jsme popisují, jak toosend a přijímat zprávy HTTP bez použití žádné knihovny open-source.

<!-- TODO: Need link toolibraries -->

Hello toku kódu autorizace OAuth 2.0 je popsaná v [části 4.1 hello OAuth 2.0 specifikace](http://tools.ietf.org/html/rfc6749). Můžete ji použít k ověřování a autorizaci většinu typů aplikací, včetně [webové aplikace](active-directory-b2c-apps.md#web-apps) a [nativně nainstalované aplikace](active-directory-b2c-apps.md#mobile-and-native-apps). Můžete použít hello získat toosecurely toku kódu autorizace OAuth 2.0 *přístup tokeny* pro vaše aplikace může být použité tooaccess prostředky, které jsou zabezpečeny [serveru ověřování](active-directory-b2c-reference-protocols.md#the-basics).

Tento článek zaměřuje na hello **veřejné klienti** toku kódu autorizace OAuth 2.0. Veřejné klienta je klientská aplikace, která nemůže být důvěryhodný toosecurely zachovat integritu hello tajný hesla. To zahrnuje mobilní aplikace, aplikace klasické pracovní plochy a v podstatě jakékoli aplikace, která běží na zařízení a je třeba tooget přístupových tokenů. 

> [!NOTE]
> tooadd identity management tooa webové aplikace pomocí Azure AD B2C, použijte [OpenID Connect](active-directory-b2c-reference-oidc.md) místo OAuth 2.0.

Azure AD B2C rozšiřuje OAuth 2.0 toků standardního hello toodo víc než jednoduché ověřování a autorizace. Zavádí hello [zásad parametr](active-directory-b2c-reference-policies.md). Předdefinované zásady, můžete využít OAuth 2.0 tooadd uživatele vyskytne tooyour aplikace, například registrace, přihlašování a správy profilů. V tomto článku jsme ukazují, jak toouse OAuth 2.0 a zásady tooimplement každá z těchto vyskytne v nativních aplikací. Můžeme také ukazují, jak tooget přístupových tokenů pro přístup k webovým rozhraním API.

V příkladu HTTP požadavků hello v tomto článku, použijeme naše ukázka adresáře Azure AD B2C, **fabrikamb2c.onmicrosoft.com**. Používáme se naše ukázková aplikace a zásady. Hello požadavků můžete také zkusit sami pomocí těchto hodnot, nebo je můžete nahradit vlastními hodnotami.
Zjistěte, jak příliš[získat vlastní adresář Azure AD B2C, aplikace a zásady](#use-your-own-azure-ad-b2c-directory).

## <a name="1-get-an-authorization-code"></a>1. Získat autorizační kód
tok autorizačního kódu Hello začíná textem hello klienta odkazovat hello uživatele toohello `/authorize` koncový bod. Toto je část interaktivní hello hello toku, kde hello uživatel provede akci. V této žádosti o označuje hello klienta v hello `scope` parametr hello oprávnění, že tato služba vyžaduje tooacquire od uživatele hello. V hello `p` parametr, znamená to tooexecute zásad hello. Hello následující tři příklady (pomocí konců řádků čitelnější) každou pomocí různých zásad.

### <a name="use-a-sign-in-policy"></a>Použít zásadu přihlášení
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_in
```

### <a name="use-a-sign-up-policy"></a>Použít zásadu registrace
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_up
```

### <a name="use-an-edit-profile-policy"></a>Pomocí zásady úpravy profilu
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_edit_profile
```

| Parametr | Povinné? | Popis |
| --- | --- | --- |
| client_id |Požaduje se |ID aplikace Hello přiřazené tooyour aplikace v hello [portál Azure](https://portal.azure.com). |
| response_type |Požaduje se |Typ odpovědi Hello, která musí zahrnovat `code` pro tok autorizačního kódu hello. |
| redirect_uri |Požaduje se |Hello identifikátor URI přesměrování vaší aplikace, kde odesílání a přijímání aplikace odpovědi ověřování. Ho musí přesně shodovat s jedním z hello přesměrování identifikátory URI, který je zaregistrovaný hello portálu, s tím rozdílem, že musí být kódovaná adresou URL. |
| Obor |Požaduje se |Seznam obory oddělených mezerami. Hodnota jeden obor označuje tooAzure služby Active Directory (Azure AD) i hello oprávnění, které jsou požadovány. Pomocí klienta hello ID jako obor hello ukazuje, že vaše aplikace vyžaduje přístupový token, který můžete použít u službou nebo webové rozhraní API, reprezentována hello stejné ID klienta.  Hello `offline_access` oboru ukazuje, že vaše aplikace vyžaduje obnovovací token pro tooresources dlohotrvající přístup. Můžete taky použít hello `openid` oboru toorequest token ID z Azure AD B2C. |
| response_mode |Doporučené |Metoda Hello použít toosend hello výsledné autorizační kód back tooyour aplikace. Může být `query`, `form_post`, nebo `fragment`. |
| state |Doporučené |Hodnota součástí hello požadavek, který je vrácený v odpovědi tokenu hello. Může být řetězec o délce veškerý obsah, který má toouse. Náhodně generované jedinečné hodnoty se obvykle používá, útoky padělání požadavku posílaného mezi weby tooprevent. Stav Hello taky je použité tooencode informace o stavu hello uživatele v aplikaci hello před hello ověřování požadavku došlo k chybě. Například hello stránku hello uživatele na nebo hello zásad, který byl vykonáván. |
| P |Požaduje se |Hello zásady, které se spustí. Jeho hello název zásady, které se vytvoří ve vašem adresáři Azure AD B2C. Hodnota názvu Hello zásad by měl začínat obráceným **b2c\_1\_**. toolearn Další informace o zásady, najdete v části [integrovaných zásad Azure AD B2C](active-directory-b2c-reference-policies.md). |
| řádku |Nepovinné |Typ Hello interakce s uživatelem, který je vyžadován. V současné době hello jediná platná hodnota je `login`, které vynutí hello uživatele tooenter přihlašovacích údajů tohoto požadavku. Jednotné přihlašování se neprojeví. |

V tomto okamžiku hello uživateli se zobrazí výzva pracovní postup pro zásady toocomplete hello. To může zahrnovat uživatele hello zadáním uživatelského jména a hesla, přihlášení pomocí identitu sociálních registrací hello adresář, nebo jakékoli jiné číslo kroky. Uživatel akce závisí na tom, jak je definována zásada hello.

Po dokončení hello zásad hello uživatele Azure AD vrátí tooyour aplikace na odpovědi na hodnotu hello jste použili pro `redirect_uri`. Používá hello metody popsané v hello `response_mode` parametr. odpověď Hello je přesně hello stejné pro všechny hello akce scénáře uživatele, nezávisle na hello zásadu, která byla spuštěna.

Úspěšná odpověď, který používá `response_mode=query` vypadá podobně jako tento:

```
GET urn:ietf:wg:oauth:2.0:oob?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...        // hello authorization_code, truncated
&state=arbitrary_data_you_can_receive_in_the_response                // hello value provided in hello request
```

| Parametr | Popis |
| --- | --- |
| Kód |autorizační kód Hello hello požadované aplikace. pro cílový prostředek, můžete použít aplikaci Hello hello autorizační kód toorequest přístupový token. Kódy ověřování jsou velmi krátkodobou. Obvykle se jejich platnost vyprší po přibližně 10 minut. |
| state |Viz úplný popis hello v tabulce hello v předcházející části hello. Pokud `state` parametr je zahrnuta v žádosti o hello hello stejnou hodnotu by se měla objevit v odpovědi hello. Hello aplikace by měl ověřit, že hello `state` hodnoty v hello žádosti a odpovědi jsou identické. |

Chybové odpovědi také odesláním identifikátor URI pro přesměrování toohello tak, aby hello aplikace dokáže zpracovat je odpovídajícím způsobem:

```
GET urn:ietf:wg:oauth:2.0:oob?
error=access_denied
&error_description=The+user+has+cancelled+entering+self-asserted+information
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametr | Popis |
| --- | --- |
| error |Řetězec kódu chyby, kterou můžete použít tooclassify hello typů chyb, ke kterým dochází. Můžete taky použít tooerrors tooreact řetězec hello. |
| error_description |Konkrétní chybová zpráva, která vám může pomoct identifikovat hello hlavní příčinu chyby ověřování. |
| state |Viz úplný popis hello v předcházející tabulce hello. Pokud `state` parametr je zahrnuta v žádosti o hello hello stejnou hodnotu by se měla objevit v odpovědi hello. Hello aplikace by měl ověřit, že hello `state` hodnoty v hello žádosti a odpovědi jsou identické. |

## <a name="2-get-a-token"></a>2. Získání tokenu
Teď, když jste získali autorizační kód, lze uplatnit hello `code` pro token toohello určený prostředku odesíláním toohello požadavek POST `/token` koncový bod. V Azure AD B2C hello pouze prostředek, který můžete požádat o token pro je vaše aplikace vlastní back endové webové rozhraní API. Hello názvů, který se používá pro požadování tokenu tooyourself je toouse ID klienta aplikace jako obor hello:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob

```

| Parametr | Povinné? | Popis |
| --- | --- | --- |
| P |Požaduje se |Hello zásady, které byly použité tooacquire hello autorizační kód. V této žádosti nelze použít jinou zásadu. Všimněte si, že přidáte tento parametr toohello *řetězec dotazu*, ne hello textu POST. |
| client_id |Požaduje se |ID aplikace Hello přiřazené tooyour aplikace v hello [portál Azure](https://portal.azure.com). |
| grant_type |Požaduje se |Hello typ udělení. Hello tok autorizačního kódu, musí být typ udělení hello `authorization_code`. |
| Obor |Doporučené |Seznam obory oddělených mezerami. Hodnota jeden obor označuje tooAzure AD obou hello oprávnění, které jsou požadovány. Pomocí klienta hello ID jako obor hello ukazuje, že vaše aplikace vyžaduje přístupový token, který můžete použít u službou nebo webové rozhraní API, reprezentována hello stejné ID klienta.  Hello `offline_access` oboru ukazuje, že vaše aplikace vyžaduje obnovovací token pro tooresources dlohotrvající přístup.  Můžete taky použít hello `openid` oboru toorequest token ID z Azure AD B2C. |
| Kód |Požaduje se |Hello autorizační kód, který jste získali v prvním větev hello hello toku. |
| redirect_uri |Požaduje se |identifikátor URI hello aplikace, kterou jste dostali hello autorizační kód přesměrování Hello. |

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
| token_type |Hodnota Hello typ tokenu. Hello pouze zadejte, že podporuje Azure AD je nosiče. |
| access_token |Hello podepsané JSON Web Token (JWT) požadovaná. |
| Obor |Hello obory, které hello token je platný pro. Také můžete použít obory toocache tokeny pro pozdější použití. |
| expires_in |Délka Hello dobu, po kterou hello token je platný (v sekundách). |
| refresh_token |Aktualizace tokenu OAuth 2.0. Hello aplikace můžete použít tento token tooacquire další tokeny po vypršení platnosti tokenu aktuální hello. Tokeny obnovení je dlouhodobé. Můžete je tooresources tooretain přístup pro dlouhou dobu. Další informace najdete v tématu hello [odkaz tokenu Azure AD B2C](active-directory-b2c-reference-tokens.md). |

Chybové odpovědi bude vypadat takto:

```
{
    "error": "access_denied",
    "error_description": "hello user revoked access toohello app.",
}
```

| Parametr | Popis |
| --- | --- |
| error |Řetězec kódu chyby, kterou můžete použít tooclassify hello typů chyb, ke kterým dochází. Můžete taky použít tooerrors tooreact řetězec hello. |
| error_description |Konkrétní chybová zpráva, která vám může pomoct identifikovat hello hlavní příčinu chyby ověřování. |

## <a name="3-use-hello-token"></a>3. Použití tokenu hello
Teď, když jste úspěšně získat přístupový token, můžete použít hello token v žádosti o tooyour back endové webové rozhraní API včetně v hello `Authorization` hlavičky:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="4-refresh-hello-token"></a>4. Aktualizovat hello token
Přístupové tokeny a ID tokeny jsou krátkodobou. Po vypršení jejich platnosti, je nutné je aktualizovat toocontinue tooaccess prostředky. toodo toho odeslat další požadavek toohello POST `/token` koncový bod. Tuto chvíli zadejte hello `refresh_token` místo hello `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

| Parametr | Povinné? | Popis |
| --- | --- | --- |
| P |Požaduje se |Hello zásady, které byly použité tooacquire hello původní obnovovací token. V této žádosti nelze použít jinou zásadu. Všimněte si, že přidáte tento parametr toohello *řetězec dotazu*, ne hello textu POST. |
| client_id |Doporučené |ID aplikace Hello přiřazené tooyour aplikace v hello [portál Azure](https://portal.azure.com). |
| grant_type |Požaduje se |Hello typ udělení. Pro tuto větev hello tok autorizačního kódu, musí být typ udělení hello `refresh_token`. |
| Obor |Doporučené |Seznam obory oddělených mezerami. Hodnota jeden obor označuje tooAzure AD obou hello oprávnění, které jsou požadovány. Pomocí klienta hello ID jako obor hello ukazuje, že vaše aplikace vyžaduje přístupový token, který můžete použít u službou nebo webové rozhraní API, reprezentována hello stejné ID klienta.  Hello `offline_access` oboru označuje, že vaše aplikace budete potřebovat obnovovací token pro tooresources dlohotrvající přístup.  Můžete taky použít hello `openid` oboru toorequest token ID z Azure AD B2C. |
| redirect_uri |Nepovinné |identifikátor URI hello aplikace, kterou jste dostali hello autorizační kód přesměrování Hello. |
| refresh_token |Požaduje se |Hello původní aktualizace token, který jste získali v druhé větev hello hello toku. |

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
| token_type |Hodnota Hello typ tokenu. Hello pouze zadejte, že podporuje Azure AD je nosiče. |
| access_token |Hello podepsaný token JWT, které jste žádali. |
| Obor |Hello obory, které hello token je platný pro. Také můžete použít hello obory toocache tokeny pro pozdější použití. |
| expires_in |Délka Hello dobu, po kterou hello token je platný (v sekundách). |
| refresh_token |Aktualizace tokenu OAuth 2.0. Hello aplikace můžete použít tento token tooacquire další tokeny po vypršení platnosti tokenu aktuální hello. Aktualizujte tokeny je dlouhodobé a může být použité tooretain přístup tooresources pro dlouhou dobu. Další informace najdete v tématu hello [odkaz tokenu Azure AD B2C](active-directory-b2c-reference-tokens.md). |

Chybové odpovědi bude vypadat takto:

```
{
    "error": "access_denied",
    "error_description": "hello user revoked access toohello app.",
}
```

| Parametr | Popis |
| --- | --- |
| error |Řetězec kódu chyby, kterou můžete použít tooclassify typů chyb, ke kterým dochází. Můžete taky použít tooerrors tooreact řetězec hello. |
| error_description |Konkrétní chybová zpráva, která vám může pomoct identifikovat hello hlavní příčinu chyby ověřování. |

## <a name="use-your-own-azure-ad-b2c-directory"></a>Použít vlastní adresáře Azure AD B2C
tootry tyto požadavky sami, dokončení hello následující kroky. Nahraďte hello ukázkové hodnoty, které jsme použili v tomto článku vlastními hodnotami.

1. [Vytvořte adresář služby Azure AD B2C](active-directory-b2c-get-started.md). Použijte název hello adresáře v žádostech o hello.
2. [Vytvoření aplikace](active-directory-b2c-app-registration.md) tooobtain ID aplikací a identifikátor URI pro přesměrování. Zahrňte nativního klienta ve vaší aplikaci.
3. [Vytvoření vlastních zásad](active-directory-b2c-reference-policies.md) tooobtain názvy zásad.

