---
title: "aaaUse Azure AD v2.0 tooaccess zabezpečení prostředků bez zásahu uživatele | Microsoft Docs"
description: "Vytvoření webové aplikace pomocí hello Azure AD implementace ověřovacího protokolu hello OAuth 2.0."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 9b7cfbd7-f89f-4e33-aff2-414edd584b07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 0003ec836d633a5466c48033adedac1108f27203
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# Tok Azure Active Directory v2.0 a hello OAuth 2.0 pověření klienta
Můžete použít hello [udělení pověření klienta OAuth 2.0](http://tools.ietf.org/html/rfc6749#section-4.4), někdy volané *s rameny dva OAuth*, tooaccess hostuje webové prostředky pomocí hello identity aplikace. Tento typ udělení běžně se používá pro interakce serveru na server, které musí běžet v pozadí hello bez okamžitou interakce s uživatelem. Tyto aplikace jsou často označují tooas *démoni* nebo *účtům služby*.

> [!NOTE]
> koncový bod v2.0 Hello nepodporuje všechny scénáře Azure Active Directory a funkce. toodetermine zda byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).
>
>

V hello typičtější *s rameny tři OAuth*, klientská aplikace je udělené oprávnění tooaccess prostředku jménem konkrétního uživatele. oprávnění Hello je delegovaná z hello uživatelská toohello aplikace, obvykle během hello [souhlas](active-directory-v2-scopes.md) procesu. Ale v hello tok přihlašovacích údajů klienta, oprávnění přímo toohello vlastní aplikace. Když aplikace hello zobrazí tokenu tooa prostředků, prostředků hello vynucuje hello aplikace má autorizace tooperform akce, a není hello má autorizace.

## Diagram protokolu
tok přihlašovacích údajů Hello celý klienta vypadá podobně jako toohello další diagram. Jednotlivé kroky hello později v tomto článku jsme popsány.

![Tok přihlašovacích údajů klienta](../../media/active-directory-v2-flows/convergence_scenarios_client_creds.png)

## Získat přímý autorizaci
Aplikace obvykle obdrží přímé autorizace tooaccess prostředku v jednom ze dvou způsobů: prostřednictvím seznam řízení přístupu (ACL) na hello prostředků nebo přiřazení oprávnění aplikace v Azure Active Directory (Azure AD). Tyto dvě metody jsou hello nejběžnější ve službě Azure AD, a doporučujeme pro klienty a prostředky, které provádějí tok přihlašovacích údajů klienta hello. Prostředek můžete vybrat tooauthorize svým klientům jinými způsoby, ale. Každý server prostředků můžete zvolit hello metoda, která má hello většina smysl pro jeho aplikaci.

### Seznamy řízení přístupu
Zprostředkovatel prostředků může vynutit kontrolu autorizace na základě seznamu ID aplikace, který zná a uděluje přístup ke konkrétní úroveň. Když hello prostředků přijímá token z koncového bodu v2.0 hello, může dekódovat hello token a extrahovat ID aplikace hello klienta z hello `appid` a `iss` deklarací identity. Pak porovná aplikace hello před ACL, který udržuje. Hello seznam ACL členitosti a metoda se může podstatně liší mezi prostředky.

Běžné případ použití je toouse seznamu ACL toorun testy pro webovou aplikaci nebo webové rozhraní API. Hello webového rozhraní API může udělit jenom podmnožinu konkrétního klienta tooa úplná oprávnění. toorun začátku do konce testy na hello rozhraní API, vytvoření testovacího klienta, který získá tokeny z koncového bodu v2.0 hello a poté je odešle toohello rozhraní API. Hello rozhraní API pak hello kontroly seznamu ACL pro hello otestovat ID klienta aplikace pro úplný přístup k celé funkce toohello rozhraní API. Pokud použijete tento druh seznamu ACL, ujistěte se, toovalidate nejen hello volajícího `appid` hodnotu. Také ověřte, že hello `iss` hodnota tokenu hello je důvěryhodný.

Tento typ ověřování je běžné démoni a účty služby, které je třeba data tooaccess vlastníkem spotřebitelské uživatele, kteří mají osobní účty Microsoft. Pro data vlastněná organizací doporučujeme, abyste měli hello nezbytné autorizace prostřednictvím oprávnění k aplikaci.

### Oprávnění aplikací
Místo použití seznamy ACL, můžete použít rozhraní API tooexpose sadu oprávnění aplikací. Správce organizace je tooan aplikace udělit oprávnění aplikaci a můžete použít pouze tooaccess data vlastnit dané organizace a její zaměstnanci. Například Microsoft Graph zpřístupňuje několik aplikací oprávnění toodo hello následující:

* Čtení pošty ve všech poštovních schránkách
* Čtení a zápis pošty ve všech poštovních schránkách
* Odesílání pošty jménem libovolného uživatele
* Čtení dat adresáře

Další informace o aplikaci oprávnění, přejděte příliš[Microsoft Graph](https://graph.microsoft.io).

kroky, které se budeme zabývat v dalších částech hello hello toouse oprávnění aplikací ve vaší aplikaci.

#### Žádostí o oprávnění hello v portálu pro registraci aplikace hello
1. Přejděte tooyour aplikace hello [portálu pro registraci aplikace](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo [vytvořit aplikaci](active-directory-v2-app-registration.md), pokud jste tak ještě neučinili. Budete potřebovat toouse alespoň jeden tajný klíč aplikace při vytváření aplikace.
2. Vyhledejte hello **přímé oprávnění aplikací** části a poté přidejte hello oprávnění, která vaše aplikace vyžaduje.
3. **Uložit** hello registrace aplikací.

#### Doporučená: Přihlašovací hello uživatele v aplikaci tooyour
Obvykle když vytvoříte aplikaci, která používá oprávnění aplikací, aplikace hello vyžaduje stránku nebo zobrazení, na které hello správce schválí oprávnění aplikace hello. Tato stránka může být součástí aplikace hello přihlášení tok, součást aplikace hello nastavení, nebo může být vyhrazený "připojit" toku. V mnoha případech má smysl pro tooshow aplikace hello to "připojit" Zobrazit pouze po je uživatel přihlášený pomocí pracovního nebo školního účtu Microsoft.

Pokud podepíšete hello uživatele v aplikaci tooyour, můžete identifikovat hello organizace toowhich hello uživatel patří předtím, než se požádat uživatele tooapprove hello oprávnění aplikací hello. I když je to nezbytně nutné, ho můžete vytvořit více intuitivní prostředí pro uživatele. toosign hello uživatele v, postupujte podle našich [v2.0 protokol kurzy](active-directory-v2-protocols.md).

#### Požádat správce directory hello oprávnění
Pokud jste připravené toorequest oprávnění správce hello organizace, můžete přesměrovat hello uživatele toohello v2.0 *koncový bod admin souhlasu*.

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro tip: Try pasting hello following request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parametr | Podmínka | Popis |
| --- | --- | --- |
| Klienta |Požaduje se |Hello directory klienta, který má oprávnění toorequest z. To může být ve formátu popisný název nebo identifikátor GUID. Pokud si nejste jisti, který uživatel hello klienta patří tooand chcete toolet je Přihlaste se pomocí jakékoli klienta, použijte `common`. |
| client_id |Požaduje se |ID tohoto hello Hello aplikace [portálu pro registraci aplikace](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) přiřazené tooyour aplikace. |
| redirect_uri |Požaduje se |identifikátor URI, kam chcete toobe odpovědi hello odeslané pro vaše aplikace toohandle přesměrování Hello. Se musí přesně shodovat s jedním z hello přesměrování identifikátory URI, který je zaregistrovaný portálu hello, s tím rozdílem, že musí být kódovaná adresou URL, a může mít segmenty další cesty. |
| state |Doporučené |Hodnota, která je součástí hello požadavek, který je také vrácený v odpovědi tokenu hello. Může být řetězec o délce veškerý obsah, který chcete. Stav Hello je použité tooencode informace o stavu hello uživatele v aplikaci hello před došlo k požadavku hello ověřování, jako je například stránku hello nebo zobrazení, které byly na. |

V tomto okamžiku Azure AD vynucuje, že žádost hello toocomplete přihlásit pouze správce klienta. Správce Hello vyzve tooapprove všechny hello přímé aplikaci oprávnění, které jste požadovali pro aplikaci v portálu pro registraci aplikace hello.

##### Úspěšná odpověď
Pokud dobrý den, správce schválí hello oprávnění pro vaši aplikaci, úspěšné odpovědi hello vypadá takto:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parametr | Popis |
| --- | --- | --- |
| Klienta |Hello directory klienta, který udělena oprávnění hello vaší aplikace, který je požadovaný, ve formátu GUID. |
| state |Hodnota, která je součástí hello požadavek, který je také vrácený v odpovědi tokenu hello. Může být řetězec o délce veškerý obsah, který chcete. Stav Hello je použité tooencode informace o stavu hello uživatele v aplikaci hello před došlo k požadavku hello ověřování, jako je například stránku hello nebo zobrazení, které byly na. |
| admin_consent |Nastavení příliš**true**. |

##### Chybové odpovědi
Pokud dobrý den, správce nebude schvalovat hello oprávnění pro vaši aplikaci, se nezdařilo hello odpovědi vypadá takto:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parametr | Popis |
| --- | --- | --- |
| error |Řetězec v kódu chyby, které můžete použít tooclassify typy chyb a který můžete použít tooreact tooerrors. |
| error_description |Konkrétní chybová zpráva, která vám může pomoct identifikovat hello hlavní příčinu chyby. |

Po přijetí úspěšná odpověď z koncového bodu zřizování pro aplikace hello je hello přímé aplikaci oprávnění, která je požadována z vaší aplikace. Nyní můžete požádat o token pro hello prostředek, který chcete.

## Získání tokenu
Poté, co jste získali autorizace hello potřebné pro vaši aplikaci, pokračujte v získání přístupových tokenů pro rozhraní API. tooget token pomocí hello udělení pověření klienta, odeslání toohello požadavek POST `/token` koncového bodu v2.0:

### Nejprve případ: žádosti o token přístupu s sdílený tajný klíč

```
POST /common/oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials
```

```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials' 'https://login.microsoftonline.com/common/oauth2/v2.0/token'
```

| Parametr | Podmínka | Popis |
| --- | --- | --- |
| client_id |Požaduje se |ID tohoto hello Hello aplikace [portálu pro registraci aplikace](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) přiřazené tooyour aplikace. |
| Obor |Požaduje se |Hello hodnota předaná pro hello `scope` parametr v této žádosti o by měl být hello prostředků identifikátor aplikace ID URI prostředku hello chcete, opatřen s hello `.default` příponu. Například hello Microsoft Graph hello hodnota je `https://graph.microsoft.com/.default`. Tato hodnota informuje koncového bodu v2.0 hello, že všechny přímé aplikace oprávnění hello, které jste nakonfigurovali pro vaši aplikaci, ho měli vydání tokenu pro hello ty, které jsou přidružené k hello prostředek, který chcete toouse. |
| tajný klíč client_secret |Požaduje se |Hello tajný klíč aplikace, který jste vygenerovali pro aplikaci v portálu pro registraci aplikace hello. |
| grant_type |Požaduje se |Musí být `client_credentials`. |

### Druhé případ: tokenu žádosti o přístup pomocí certifikátu

```
POST /common/oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg&grant_type=client_credentials
```

| Parametr | Podmínka | Popis |
| --- | --- | --- |
| client_id |Požaduje se |ID tohoto hello Hello aplikace [portálu pro registraci aplikace](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) přiřazené tooyour aplikace. |
| Obor |Požaduje se |Hello hodnota předaná pro hello `scope` parametr v této žádosti o by měl být hello prostředků identifikátor aplikace ID URI prostředku hello chcete, opatřen s hello `.default` příponu. Například hello Microsoft Graph hello hodnota je `https://graph.microsoft.com/.default`. Tato hodnota informuje koncového bodu v2.0 hello, že všechny přímé aplikace oprávnění hello, které jste nakonfigurovali pro vaši aplikaci, ho měli vydání tokenu pro hello ty, které jsou přidružené k hello prostředek, který chcete toouse. |
| client_assertion_type |Požadované |musí být hodnota Hello`urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |Požadované | Kontrolní výrazy (webového tokenu JSON), je nutné toocreate a přihlášení s hello certifikátů můžete zaregistrovat jako přihlašovací údaje pro vaši aplikaci. Přečtěte si informace o [certifikát přihlašovacích údajů](active-directory-certificate-credentials.md) toolearn jak tooregister váš certifikát a hello formát hello assertion.|
| grant_type |Požaduje se |Musí být `client_credentials`. |

Všimněte si, že jsou parametry hello téměř hello stejné jako v případě hello hello požadavku podle sdílený tajný klíč s tím rozdílem, že parametr tajný klíč client_secret hello je nahrazena dva parametry: client_assertion_type a client_assertion.

### Úspěšná odpověď
Úspěšná odpověď vypadá takto:

```
{
  "token_type": "Bearer",
  "expires_in": 3599,
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBP..."
}
```

| Parametr | Popis |
| --- | --- |
| access_token |Hello požadovaný přístupový token. Hello aplikace můžete používat tento token tooauthenticate toohello zabezpečeným prostředkům, například tooa webového rozhraní API. |
| token_type |Určuje hodnotu hello typ tokenu. Hello pouze typ, který podporuje Azure AD je `bearer`. |
| expires_in |Jak dlouho hello přístupový token je platný (v sekundách). |

### Chybové odpovědi
Chybnou odpověď vypadá takto:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: hello provided value for hello input parameter 'scope' is not valid. hello scope https://foo.microsoft.com/.default is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
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
| error |Řetězec kódu chyby, kterou můžete použít tooclassify typy chyb, ke kterým došlo, a tooreact tooerrors. |
| error_description |Konkrétní chybová zpráva, které vám můžou pomoct identifikovat hello hlavní příčinu chyby ověřování. |
| error_codes |Seznam tokenů zabezpečení specifické chybové kódy, které vám můžou pomoct s diagnostiky. |
| časové razítko |Hello čas, kdy došlo k chybě hello. |
| trace_id |Jedinečný identifikátor pro hello žádosti, které můžou pomoct s diagnostikou. |
| correlation_id |Jedinečný identifikátor pro hello žádosti, které můžou pomoct s diagnostikou mezi komponentami. |

## Použít token
Teď, když jste získali token, použijte hello tokenu toomake požadavky toohello prostředků. Když vyprší platnost tokenu hello, opakujte žádost toohello hello `/token` koncový bod tooacquire nový přístupový token.

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

```
// Pro tip: Try hello following command! (Replace hello token with your own.)
```

```
curl -X GET -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q" 'https://graph.microsoft.com/v1.0/me/messages'
```

## Ukázka kódu
toosee příklad aplikace, implementuje hello udělení pověření klienta pomocí hello správce souhlasu koncový bod, najdete v našich [ukázka kódu démon v2.0](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).
