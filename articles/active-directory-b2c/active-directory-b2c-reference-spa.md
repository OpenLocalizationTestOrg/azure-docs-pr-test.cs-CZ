---
title: "Azure Active Directory B2C: Jednostránkové aplikace pomocí implicitního toku | Microsoft Docs"
description: "Zjistěte, jak toobuild jednostránkové aplikace přímo pomocí standardu OAuth 2.0 implicitní tok s Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: a45cc74c-a37e-453f-b08b-af75855e0792
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: parakhj
ms.openlocfilehash: 5e52abc18fe545f0f6d1745cdb2ea42c5b2698cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-single-page-app-sign-in-by-using-oauth-20-implicit-flow"></a>Azure AD B2C: Jednostránkové aplikace přihlášení pomocí implicitního toku OAuth 2.0

> [!NOTE]
> Tato funkce je ve verzi preview.
> 

Řada moderních aplikací využívá jednostránkový front-end, který je primárně napsané v jazyce JavaScript. Aplikace hello je často zapsány pomocí rozhraní jako AngularJS, Ember.js nebo Durandal. Jednostránkové aplikace a další aplikace JavaScript, které běží především v prohlížeči mají některé další problémy týkající se ověřování:

* Vlastnosti zabezpečení Hello tyto aplikace se výrazně liší od tradiční na serveru webové aplikace.
* Mnoho serverů autorizace a zprostředkovatelů identity nepodporují požadavků (CORS) pro sdílení prostředků různého původu.
* Přesměrování celou stránku prohlížeče směrem od hello aplikace může být výrazně invazivní toohello uživatelské prostředí.

toosupport tyto aplikace Azure Active Directory B2C (Azure AD B2C) používá implicitní tok hello OAuth 2.0. Hello OAuth 2.0 autorizace implicitní grant postup je popsán v [části 4.2 hello OAuth 2.0 specifikace](http://tools.ietf.org/html/rfc6749). V implicitní tok, hello aplikace přijímá tokeny přímo z hello Azure Active Directory (Azure AD) zajistí autorizaci koncového bodu, bez jakékoli exchange serveru na server. Všechny logiku ověřování a relace trvá zpracování umístit zcela v hello JavaScript klienta, a to bez přesměrování další stránky.

Azure AD B2C rozšiřuje hello standardní OAuth 2.0 implicitního toku toomore než jednoduché ověřování a autorizace. Azure AD B2C zavádí hello [zásad parametr](active-directory-b2c-reference-policies.md). Parametr hello zásady, můžete využít OAuth 2.0 tooadd uživatele vyskytne tooyour aplikace, například registrace, přihlašování a správy profilů. V tomto článku jsme ukážeme, jak toouse hello implicitního toku a Azure AD tooimplement každý z těchto prostředí ve svých aplikacích jednostránkové. toohelp začnete, prohlédněte si naše [Node.js](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-nodejs-webapi) a [rozhraní Microsoft .NET](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-dotnet-webapi) ukázky.

V příkladu HTTP požadavků hello v tomto článku, použijeme naše ukázka adresáře Azure AD B2C, **fabrikamb2c.onmicrosoft.com**. Také používáme vlastní ukázkové aplikace a zásady. Hello požadavků můžete také zkusit sami pomocí těchto hodnot, nebo je můžete nahradit vlastními hodnotami.
Zjistěte, jak příliš[získat vlastní adresář Azure AD B2C, aplikace a zásady](#use-your-own-b2c-tenant).


## <a name="protocol-diagram"></a>Diagram protokolu

Hello implicitní přihlášení toku podobný hello následující obrázek. Každý krok je podrobně popsaná v dále v článku hello.

![OpenID Connect plaveckých drah](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## <a name="send-authentication-requests"></a>Odeslání žádosti o ověření
Když webové aplikace musí tooauthenticate hello uživatele a spustit zásadu, směruje uživatele toohello hello `/authorize` koncový bod. Toto je část interaktivní hello hello toku, kde hello uživatel provede akci, v závislosti na zásadách hello. Hello uživatel získá ID tokenu z koncového bodu Azure AD hello.

V této žádosti o označuje hello klienta v hello `scope` parametr hello oprávnění, že tato služba vyžaduje tooacquire od uživatele hello. V hello `p` parametr, znamená to tooexecute zásad hello. Hello následující tři příklady (pomocí konců řádků čitelnější) každou pomocí různých zásad. tooget chování pro každou žádost o tom, jak funguje, zkuste žádost hello vkládání do prohlížeče a jejím spuštěním.

### <a name="use-a-sign-in-policy"></a>Použít zásadu přihlášení
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

### <a name="use-a-sign-up-policy"></a>Použít zásadu registrace
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

### <a name="use-an-edit-profile-policy"></a>Pomocí zásady úpravy profilu
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| Parametr | Povinné? | Popis |
| --- | --- | --- |
| client_id |Požaduje se |ID aplikace Hello přiřazené tooyour aplikace v hello [portál Azure](https://portal.azure.com). |
| response_type |Požaduje se |Musí zahrnovat `id_token` pro OpenID Connect přihlášení. Typ odpovědi hello také může zahrnovat `token`. Pokud používáte `token`, vaše aplikace může přijímat okamžitě přístupový token z hello zajistí autorizaci koncového bodu, bez provedení druhý toohello požadavek zajistí autorizaci koncového bodu.  Pokud používáte hello `token` typ odpovědi, hello `scope` parametr musí obsahovat obor, který určuje, které prostředků tooissue hello token pro. |
| redirect_uri |Doporučené |identifikátor URI aplikace, kde můžete odesílat a přijímat aplikace ověřování odpovědi přesměrování Hello. Ho musí přesně shodovat s jedním z hello přesměrování identifikátory URI, který je zaregistrovaný hello portálu, s tím rozdílem, že musí být kódovaná adresou URL. |
| response_mode |Doporučené |Určuje hello metoda toouse toosend hello výsledný token back tooyour aplikace.  Implicitní toky, použijte `fragment`. |
| Obor |Požaduje se |Seznam obory oddělených mezerami. Hodnota jeden obor označuje tooAzure AD obou hello oprávnění, které jsou požadovány. Hello `openid` oboru označuje toosign oprávnění v hello uživatele a získání dat o hello uživatele v podobě hello ID tokenů. (Budeme mluvit o tom víc dále v článku hello.) Hello `offline_access` obor je volitelné pro webové aplikace. Označuje, že vaše aplikace musí obnovovací token pro tooresources dlohotrvající přístup. |
| state |Doporučené |Hodnota součástí hello požadavek, který je také vrácený v odpovědi tokenu hello. Může být řetězec o délce veškerý obsah, který má toouse. Obvykle se používá hodnotu náhodně generované, jedinečné, útoky padělání požadavku posílaného mezi weby tooprevent. Hello stav je také použít tooencode informace o stavu hello uživatele v aplikaci hello před hello ověřování požadavku došlo k chybě, jako je stránku hello byly na. |
| hodnotu Nonce |Požaduje se |Hodnota, zahrnuté v požadavku hello (vygenerovaný hello aplikací), který je součástí hello výsledný token ID jako deklarace identity. aplikace Hello můžete pak ověřte, že tato hodnota toomitigate opětovného přehrání tokenu útoků. Hodnota hello je obvykle náhodnou jedinečného řetězce, které můžou být použité tooidentify hello původ hello požadavku. |
| P |Požaduje se |Hello tooexecute zásad. Jeho hello název zásady, který je vytvořen v klientovi Azure AD B2C. Hodnota názvu Hello zásad by měl začínat obráceným **b2c\_1\_**. Další informace najdete v tématu [integrovaných zásad Azure AD B2C](active-directory-b2c-reference-policies.md). |
| řádku |Nepovinné |Typ Hello interakce s uživatelem, které je nutné. V současné době hello jediná platná hodnota je `login`. Vynutí se tak hello uživatele tooenter přihlašovacích údajů tohoto požadavku. Jednotné přihlašování se neprojeví. |

V tomto okamžiku hello uživateli se zobrazí výzva pracovní postup pro zásady toocomplete hello. To může zahrnovat uživatele hello zadáním uživatelského jména a hesla, přihlášení pomocí identitu sociálních registrací hello adresář, nebo jakékoli jiné číslo kroky. Uživatel akce závisí na tom, jak je definována zásada hello.

Po dokončení hello zásad hello uživatele Azure AD vrátí tooyour aplikace na odpovědi na hodnotu hello jste použili pro `redirect_uri`. Používá hello metody popsané v hello `response_mode` parametr. odpověď Hello je přesně hello stejné pro všechny hello akce scénáře uživatele, nezávisle na hello zásadu, která byla spuštěna.

### <a name="successful-response"></a>Úspěšná odpověď
Úspěšná odpověď, který používá `response_mode=fragment` a `response_type=id_token+token` vypadá jako následující hello pomocí konců řádků pro čitelnost:

```
GET https://aadb2cplayground.azurewebsites.net/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=arbitrary_data_you_sent_earlier
```

| Parametr | Popis |
| --- | --- |
| access_token |Hello přístupový token, který hello požadované aplikace.  přístupový token Hello by neměly být dekódovat, nebo v opačném případě prověřovány. Může být považovány za neprůhledný řetězec. |
| token_type |Hodnota Hello typ tokenu. Hello pouze zadejte, že podporuje Azure AD je nosiče. |
| expires_in |Délka Hello dobu, po kterou hello přístupový token je platný (v sekundách). |
| Obor |Hello obory, které hello token je platný pro. Také můžete použít obory toocache tokeny pro pozdější použití. |
| požadavku id_token |Hello ID token, který hello požadované aplikace. Můžete použít hello ID tokenu tooverify hello identitu uživatele a zahájit relaci s hello uživatele. Další informace o ID tokeny a jejich obsah, najdete v části hello [odkaz tokenu Azure AD B2C](active-directory-b2c-reference-tokens.md). |
| state |Pokud `state` parametr je zahrnuta v žádosti o hello hello stejnou hodnotu by se měla objevit v odpovědi hello. Hello aplikace by měl ověřit, že hello `state` hodnoty v hello žádosti a odpovědi jsou identické. |

### <a name="error-response"></a>Chybové odpovědi
Chybové odpovědi také odesláním identifikátor URI pro přesměrování toohello tak, aby hello aplikace dokáže zpracovat je odpovídajícím způsobem:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametr | Popis |
| --- | --- |
| error |Řetězec v kódu chyby použít tooclassify typů chyb, ke kterým dochází. Kód chyby hello také můžete použít pro zpracování chyb. |
| error_description |Konkrétní chybová zpráva, která vám může pomoct identifikovat hello hlavní příčinu chyby ověřování. |
| state |Viz úplný popis hello v předcházející tabulce hello. Pokud `state` parametr je zahrnuta v žádosti o hello hello stejnou hodnotu by se měla objevit v odpovědi hello. Hello aplikace by měl ověřit, že hello `state` hodnoty v hello žádosti a odpovědi jsou identické.|

## <a name="validate-hello-id-token"></a>Ověřit hello ID token
Přijetí tokenu ID není dostatek tooauthenticate hello uživatele. Je nutné ověřit podpis tokenu ID hello a ověřte hello deklarace identity ve hello tokenu podle požadavků vaší aplikace. Používá Azure AD B2C [webové tokeny JSON (Jwt)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) a veřejný klíč kryptografie toosign tokeny a ověřte, zda je platný.

Mnoho knihovny open-source jsou k dispozici pro ověřování tokeny Jwt, v závislosti na jazyk hello dáváte přednost toouse. Vezměte v úvahu zkoumat dostupné knihovny open-source než implementace vlastní logiky ověřování. Hello informace můžete použít v této toohelp článku zjistíte, jak používat tooproperly tyto knihovny.

Azure AD B2C má koncový bod metadat OpenID Connect. Aplikace můžete použít hello koncový bod toofetch informace o Azure AD B2C za běhu. Tyto informace zahrnují koncových bodů, obsah tokenu a token podpisových klíčů. Je dokument metadat JSON pro každou zásadu v klientovi služby Azure AD B2C. Například dokument metadat hello hello b2c_1_sign_in zásady v klientovi fabrikamb2c.onmicrosoft.com hello je umístěn zde:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Jedna z vlastností hello tohoto dokumentu konfigurace je hello `jwks_uri`. Hello hodnotu pro dobrý den, které by bylo stejné zásady:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`

toodetermine zásadu, pomocí které byla použité toosign ID token (a kde toofetch hello metadata z), máte dvě možnosti. Nejprve je název zásady hello součástí hello `acr` deklarací identity v `id_token`. Informace o tom, jak tooparse hello deklarací z tokenu ID najdete v tématu hello [odkaz tokenu Azure AD B2C](active-directory-b2c-reference-tokens.md). Jinak se zásady hello tooencode v hello hodnotu hello `state` parametr, pokud vydáte požadavek hello. Potom dekódovat hello `state` parametr toodetermine zásadu, které byl použit. Buď metoda je platná.

Poté, co jste získali hello dokument metadat z hello OpenID Connect metadat koncového bodu, můžete použít hello RSA 256 veřejných klíčů (nachází se na tento koncový bod) toovalidate hello podpis tokenu ID hello. Může být více klíčů uvedený na tento koncový bod v každém okamžiku, každý se identifikovanou pomocí `kid`. záhlaví Hello `id_token` také obsahuje `kid` deklarací identity. Označuje, která z těchto klíčů byla použité toosign hello ID token. Další informace, včetně informací o [ověřování tokenů](active-directory-b2c-reference-tokens.md#token-validation), najdete v části hello [odkaz tokenu Azure AD B2C](active-directory-b2c-reference-tokens.md).
<!--TODO: Improve hello information on this-->

Po ověření podpisu hello hello ID tokenu deklarací několik vyžadovat ověření. Například:

* Ověření hello `nonce` deklarace identity tooprevent opětovného přehrání tokenu útoky. Její hodnota musí být zadaný v hello požádat o přihlášení.
* Ověření hello `aud` deklarace identity tooensure, který hello ID token byl vydán pro vaši aplikaci. Její hodnota musí být ID aplikace hello vaší aplikace.
* Ověření hello `iat` a `exp` nevypršela platnost tooensure, který hello ID tokenu deklarací identity.

Několik další ověření, které byste měli provést jsou podrobně popsány v hello [OpenID Connect základní specifikace](http://openid.net/specs/openid-connect-core-1_0.html). Můžete také toovalidate další deklarace identity, v závislosti na vašem scénáři. Některé běžné ověření patří:

* Zajistit, že hello uživatele nebo organizace má zaregistrovali do aplikace hello.
* Zajištění tohoto uživatele hello má příslušná oprávnění a oprávnění.
* Zajištění, že sílu ověřování došlo k chybě, tak, jak pomocí Azure Multi-Factor Authentication.

Další informace o deklaracích identity hello v tokenu ID najdete v tématu hello [odkaz tokenu Azure AD B2C](active-directory-b2c-reference-tokens.md).

Po zcela ověření tokenu ID hello můžete začít relaci s hello uživatelem. Ve vaší aplikaci použijte deklarace identity hello v hello ID tokenu tooobtain informace o uživateli hello. Tyto informace můžete použít pro zobrazení, záznamy, autorizace a tak dále.

## <a name="get-access-tokens"></a>Získat přístupové tokeny
Pokud hello pouze jedinou toodo potřebám vaší webové aplikace je provést zásady, můžete přeskočit hello několik dalších částech. Hello informace v následující části hello se použít jenom tooweb aplikace vyžadující ověření toomake volání tooa webového rozhraní API a které jsou chráněné službou Azure AD B2C.

Teď, když jste podepsané hello uživatele v tooyour jednostránkové aplikace, můžete získat přístupové tokeny pro volání webových rozhraní API, která jsou zabezpečená službou Azure AD. I v případě, že jste už dostali token pomocí hello `token` typ odpovědi, můžete použít tato metoda tooacquire tokeny další zdroje bez přesměrování hello toosign uživatele v akci.

V toku typické webové aplikace, by to uděláte tak, že žádost o toohello `/token` koncový bod.  Koncový bod hello však nemá CORS žádosti o podporu, tak, aby provedení AJAX volá tooget a tokeny obnovení není možné. Místo toho můžete vytvořit implicitního toku hello v skryté HTML iframe element tooget nové tokeny pro jiné webové rozhraní API. Tady je příklad, pomocí konců řádků pro čitelnost:

```

https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&scope=https%3A%2F%2Fapi.contoso.com%2Ftasks.read
&response_mode=fragment
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
&p=b2c_1_sign_in
```

| Parametr | Povinné? | Popis |
| --- | --- | --- |
| client_id |Požaduje se |ID aplikace Hello přiřazené tooyour aplikace v hello [portál Azure](https://portal.azure.com). |
| response_type |Požaduje se |Musí zahrnovat `id_token` pro OpenID Connect přihlášení.  Může také obsahovat typ odpovědi hello `token`. Pokud používáte `token` zde, vaše aplikace může okamžitě přijímat přístupový token z hello zajistí autorizaci koncového bodu, bez provedení druhý toohello požadavek zajistí autorizaci koncového bodu. Pokud používáte hello `token` typ odpovědi, hello `scope` parametr musí obsahovat obor, který určuje, které prostředků tooissue hello token pro. |
| redirect_uri |Doporučené |identifikátor URI aplikace, kde můžete odesílat a přijímat aplikace ověřování odpovědi přesměrování Hello. Ho musí přesně shodovat s jedním hello přesměrování identifikátory URI registrován hello portálu, s tím rozdílem, že musí být kódovaná adresou URL. |
| Obor |Požaduje se |Seznam obory oddělených mezerami.  Jak získat tokeny, zahrnují všechny obory, které požadujete pro hello určený prostředek. |
| response_mode |Doporučené |Určuje hello metod, které jsou používané toosend hello výsledný token back tooyour aplikace.  Může být `query`, `form_post`, nebo `fragment`. |
| state |Doporučené |Hodnota součástí hello požadavek, který je vrácený v odpovědi tokenu hello.  Může být řetězec o délce veškerý obsah, který má toouse.  Obvykle se používá hodnotu náhodně generované, jedinečné, útoky padělání požadavku posílaného mezi weby tooprevent.  Stav Hello taky je použité tooencode informace o stavu hello uživatele v aplikaci hello před hello ověřování požadavku došlo k chybě. Například hello stránky nebo zobrazení hello uživatel byl na. |
| hodnotu Nonce |Požaduje se |Hodnota, zahrnuté v žádosti o hello vygenerovaný hello aplikací, který je součástí hello výsledný token ID jako deklarace identity.  aplikace Hello můžete pak ověřte, že tato hodnota toomitigate opětovného přehrání tokenu útoků. Hodnota hello je obvykle náhodnou, jedinečný řetězec, který identifikuje hello původ hello požadavku. |
| řádku |Požaduje se |tokeny toorefresh a získání přístupu v skrytá iframe, použijte `prompt=none` tooensure, který hello iframe není uváznout na přihlašovací stránku hello a vrátí okamžitě. |
| login_hint |Požaduje se |tokeny toorefresh a získání přístupu v skrytá iframe, zahrnout hello uživatelské jméno uživatele hello Tento pomocný parametr toodistinguish mezi více relací, které uživatel hello může mít v daném okamžiku. Uživatelské jméno hello z dřívějších sign-in můžete rozbalit pomocí hello `preferred_username` deklarací identity. |
| domain_hint |Požaduje se |Může být `consumers` nebo `organizations`.  Pro aktualizaci a jak získat tokeny v skrytá iframe, musí obsahovat hello `domain_hint` hello žádost hodnotu.  Extrahování hello `tid` deklarací z tokenu ID hello dříve přihlášení toodetermine které toouse hodnotu.  Pokud hello `tid` deklarace, hodnota je `9188040d-6c67-4c5b-b112-36a304b66dad`, použijte `domain_hint=consumers`.  Jinak použijte `domain_hint=organizations`. |

Pomocí nastavení hello `prompt=none` parametr tuto žádost buď úspěšné nebo selže okamžitě a vrátí tooyour aplikace.  Úspěšná odpověď je odeslána tooyour aplikace na uvedené hello identifikátor URI pro přesměrování, pomocí hello metody popsané v hello `response_mode` parametr.

### <a name="successful-response"></a>Úspěšná odpověď
Úspěšná odpověď pomocí `response_mode=fragment` vypadá podobně jako tento:

```
GET https://aadb2cplayground.azurewebsites.net/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=arbitrary_data_you_sent_earlier
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fapi.contoso.com%2Ftasks.read
```

| Parametr | Popis |
| --- | --- |
| access_token |Hello token, který hello požadované aplikace. |
| token_type |Typ tokenu Hello bude vždy nosiče. |
| state |Pokud `state` parametr je zahrnuta v žádosti o hello hello stejnou hodnotu by se měla objevit v odpovědi hello. Hello aplikace by měl ověřit, že hello `state` hodnoty v hello žádosti a odpovědi jsou identické. |
| expires_in |Jak dlouho hello přístupový token je platný (v sekundách). |
| Obor |Hello obory, které hello přístupový token je platný pro. |

### <a name="error-response"></a>Chybové odpovědi
Chybové odpovědi také možné odeslat identifikátor URI pro přesměrování toohello tak, aby hello aplikace můžete je správně zpracovat.  Pro `prompt=none`, chybu očekávané vypadá takto:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parametr | Popis |
| --- | --- |
| error |Řetězec kódu chyby, které můžou být použité tooclassify typů chyb, ke kterým dochází. Můžete taky použít tooerrors tooreact řetězec hello. |
| error_description |Konkrétní chybová zpráva, která vám může pomoct identifikovat hello hlavní příčinu chyby ověřování. |

Pokud tato chyba se zobrazí v požadavku iframe hello, hello uživatel musí interaktivně přihlásit znovu tooretrieve nový token. To může zpracovávat způsobem, který dává smysl pro vaši aplikaci.

## <a name="refresh-tokens"></a>Obnovovacích tokenů
ID tokeny a přístupové tokeny vyprší po krátkou dobu. Aplikace musí být připraveny toorefresh tyto tokeny pravidelně.  provádět buď typ tokenu, toorefresh hello stejné skrytá iframe žádost jsme použili v předchozí příklad, a to pomocí hello `prompt=none` kroky parametr toocontrol Azure AD.  tooreceive nový `id_token` hodnoty, zda toouse být `response_type=id_token` a `scope=openid`a `nonce` parametr.

## <a name="send-a-sign-out-request"></a>Poslat žádost o odhlášení
Když chcete toosign hello uživatele mimo aplikaci hello, přesměruje hello uživatele tooAzure AD toosign limitu. Pokud to neuděláte, hello uživatel může být schopný tooreauthenticate tooyour aplikace bez opětovného zadávání svých přihlašovacích údajů. Je to proto, že budou mít platný jedné přihlášení relace s Azure AD.

Jednoduše můžete přesměrovat uživatele toohello hello `end_session_endpoint` který je uvedené v hello stejné dokument metadat OpenID Connect popsané v [ověřením hello ID token](#validate-the-id-token). Například:

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parametr | Povinné? | Popis |
| --- | --- | --- |
| P |Požaduje se |uživatel hello Hello zásad toouse toosign mimo vaší aplikace. |
| post_logout_redirect_uri |Doporučené |Hello tento uživatel hello adresa URL by měla být přesměrovaného tooafter úspěšné odhlášení. Pokud nezadáte, Azure AD B2C zobrazí obecná zpráva toohello uživatele. |

> [!NOTE]
> Odkazovat hello uživatele toohello `end_session_endpoint` vymaže některé hello jeden přihlašování stavu uživatele v Azure AD B2C. Ale odhlášeni hello uživatele mimo hello sociálních identity zprostředkovatele relaci. Pokud uživatel hello vybere hello stejné při následné přihlášení k identifikaci zprostředkovatele, hello uživatele k novému ověření, bez nutnosti zadávat své přihlašovací údaje. Pokud chce uživatel toosign mimo aplikaci Azure AD B2C, je však nemusí znamenat, že chtějí například toocompletely odhlaste se od svého účtu sítě Facebook. Ale pro místní účty hello uživatelské relace bude ukončen správně.
> 
> 

## <a name="use-your-own-azure-ad-b2c-tenant"></a>Použití vlastního klienta Azure AD B2C
Tyto požadavky sami, dokončení tootry hello následující tři kroky. Nahraďte hello ukázkové hodnoty, které používáme v tomto článku vlastními hodnotami:

1. [Vytvoření klienta Azure AD B2C](active-directory-b2c-get-started.md). Použití hello název vašeho klienta v žádostech o hello.
2. [Vytvoření aplikace](active-directory-b2c-app-registration.md) tooobtain ID aplikací a `redirect_uri` hodnotu. Zahrňte do vaší aplikace na webovou aplikaci nebo webové rozhraní API. Volitelně můžete vytvořit tajný klíč aplikace.
3. [Vytvoření vlastních zásad](active-directory-b2c-reference-policies.md) tooobtain názvy zásad.

## <a name="samples"></a>Ukázky

* [Vytvoření jednostránkové aplikace pomocí Node.js](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-nodejs-webapi)
* [Vytvoření jednostránkové aplikace s použitím rozhraní .NET](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-dotnet-webapi)

