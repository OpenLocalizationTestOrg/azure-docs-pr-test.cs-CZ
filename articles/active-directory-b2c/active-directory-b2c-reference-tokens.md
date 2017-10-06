---
title: 'Azure Active Directory B2C: Odkaz tokenu | Microsoft Docs'
description: "typy Hello tokeny vydané v Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 6df79878-65cb-4dfc-98bb-2b328055bc2e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/17/2017
ms.author: dastrock
ms.openlocfilehash: 776bc88cc0308875ac6e576f19ac9edf50319d08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-token-reference"></a>Azure AD B2C: Odkaz tokenu
Azure Active Directory B2C (Azure AD B2C) vysílá několik typů tokeny zabezpečení, jako zpracovává každý [tok ověřování](active-directory-b2c-apps.md). Tento dokument popisuje hello formátu zabezpečení vlastnosti a obsah každého typu token.

## <a name="types-of-tokens"></a>Typy tokenů
Azure AD B2C podporuje hello [protokol autorizace OAuth 2.0](active-directory-b2c-reference-protocols.md), které využívá oba přístup tokeny a obnovovacích tokenů. Také podporuje ověřování a přihlásit se přes [OpenID Connect](active-directory-b2c-reference-protocols.md), který představuje třetí typ tokenu: hello ID token. Všechny tyto tokeny je reprezentován jako token nosiče.

Token lightweight zabezpečení, že uděluje hello "nosiče" přístup tooa chráněný prostředek, je token nosiče. nosiče Hello je jakékoli strany, který může být hello token. Azure AD musí je nejdřív ověřit a stranou předtím, než mohl přijímat token nosiče. Ale pokud hello požadované kroky nezavedou toosecure hello token v přenosu a úložiště, můžete zachytit a použít nezamýšleným strana. Některé tokeny zabezpečení mají integrované mechanismus, který brání neoprávněným stranám jejich používání, ale nosné tokeny nemají tento mechanismus. Musí být přenosu v zabezpečený kanál, jako je například transport layer security (HTTPS).

Pokud je token nosiče přenesen mimo zabezpečený kanál, škodlivý strany můžete token útok man-in-the-middle tooacquire hello a použít je toogain neoprávněného přístupu tooa chráněných prostředků. Hello stejné zásady zabezpečení platí, když jsou nosné tokeny uložené nebo ukládání do mezipaměti pro pozdější použití. Vždy zajistěte, aby vaše aplikace odesílá a ukládá nosné tokeny zabezpečeným způsobem.

Další důležité informace o zabezpečení na nosné tokeny, najdete v části [RFC 6750 část 5](http://tools.ietf.org/html/rfc6750).

Řadu hello tokeny, které Azure AD B2C problémy jsou implementované jako webové tokeny JSON (Jwt). Token JWT je compact, adresa URL bezpečných způsob přenosu informací mezi dvěma účastníky. Tokeny Jwt obsahují informace, které jsou známé jako deklarace identity. Jsou to kontrolní výrazy informací o hello nosiče a hello subjektu hello tokenu. deklarace identity Hello v Jwt jsou objekty JSON, které jsou zakódovány a serializovat pro přenos. Protože jsou hello vystavený Azure AD B2C tokeny Jwt podepsán, ale nejsou šifrovaná, si můžete prohlédnout snadno hello obsah JWT toodebug ho. K dispozici je několik nástrojů, můžete to provést, včetně [calebb.net](http://calebb.net). Další informace o Jwt, najdete v části příliš[JWT specifikace](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>ID tokeny
ID token je formulář tokenu zabezpečení, která vaše aplikace přijímá od hello Azure AD B2C `authorize` a `token` koncové body. ID tokeny jsou reprezentovány jako [Jwt](#types-of-tokens), a obsahují deklarace identity, které můžete tooidentify uživatelé ve vaší aplikaci. Když jsou tokeny typu ID získali z hello `authorize` koncový bod, jsou často používané toosign v aplikacích tooweb uživatele. Když jsou tokeny typu ID získali z hello `token` koncový bod, lze je odeslat v požadavcích HTTP při komunikaci mezi dvě součásti hello stejné aplikaci nebo službě. Hello deklarací identity můžete použít v tokenu ID podle svých potřeb. Jsou běžně používané toodisplay účet informace nebo toomake rozhodnutí o řízení přístupu v aplikaci.  

Jsou podepsané tokeny typu ID, ale nejsou aktuálně zašifrovány. Když aplikaci nebo API obdrží ID token, musí [ověřit podpis hello](#token-validation) tooprove, který hello tokenu je platná. Aplikaci nebo API musíte také ověřit pár deklarací identity v tokenu tooprove hello je platná. V závislosti na požadavcích scénáře hello, se může lišit hello deklarace ověřené aplikací, ale aplikace musí provést některé [běžné ověření deklarace identity](#token-validation) v každý scénář.

#### <a name="sample-id-token"></a>Ukázkový token pro ID
```
// Line breaks for display purposes only

eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IklkVG9rZW5TaWduaW5nS2V5Q29udGFpbmVyIn0.
eyJleHAiOjE0NDIzNjAwMzQsIm5iZiI6MTQ0MjM1NjQzNCwidmVyIjoiMS4wIiwiaXNzIjoiaHR0cHM6Ly9s
b2dpbi5taWNyb3NvZnRvbmxpbmUuY29tLzc3NTUyN2ZmLTlhMzctNDMwNy04YjNkLWNjMzExZjU4ZDkyNS92
Mi4wLyIsImFjciI6ImIyY18xX3NpZ25faW5fc3RvY2siLCJzdWIiOiJOb3Qgc3VwcG9ydGVkIGN1cnJlbnRs
eS4gVXNlIG9pZCBjbGFpbS4iLCJhdWQiOiI5MGMwZmU2My1iY2YyLTQ0ZDUtOGZiNy1iOGJiYzBiMjlkYzYi
LCJpYXQiOjE0NDIzNTY0MzQsImF1dGhfdGltZSI6MTQ0MjM1NjQzNCwiaWRwIjoiZmFjZWJvb2suY29tIn0.
h-uiKcrT882pSKUtWCpj-_3b3vPs3bOWsESAhPMrL-iIIacKc6_uZrWxaWvIYkLra5czBcGKWrYwrAC8ZvQe
DJWZ50WXQrZYODEW1OUwzaD_I1f1HE0c2uvaWdGXBpDEVdsD3ExKaFlKGjFR2V7F-fPThkVDdKmkUDQX3bVc
yyj2V2nlCQ9jd7aGnokTPfLfpOjuIrTsAdPcGpe5hfSEuwYDmqOJjGs9Jp1f-eSNEiCDQOaTBSvr479L5ptP
XWeQZyX2SypN05Rjr05bjZh3j70ZUimiocfJzjibeoDCaQTz907yAg91WYuFOrQxb-5BaUoR7K-O7vxr2M-_
CQhoFA

```

### <a name="access-tokens"></a>Přístupové tokeny
Přístupový token je také formulář tokenu zabezpečení, která vaše aplikace přijímá od hello Azure AD B2C `authorize` a `token` koncové body. Přístupové tokeny, které jsou také reprezentované jako [Jwt](#types-of-tokens), a obsahují deklarace identity, které můžete použít tooidentify hello udělena oprávnění tooyour rozhraní API. Přístupové tokeny jsou podepsané, ale nejsou aktuálně zašifrovány. Přístupové tokeny by měl být použité tooprovide servery tooAPIs a prostředku přístup. Další informace o příliš[pomocí přístupových tokenů](active-directory-b2c-access-tokens.md). 

Rozhraní API obdrží token přístupu, musí [ověřit podpis hello](#token-validation) tooprove, který hello tokenu je platná. Rozhraní API musíte také ověřit pár deklarací identity v tokenu tooprove hello je platná. V závislosti na požadavcích scénáře hello, se může lišit hello deklarace ověřené aplikací, ale aplikace musí provést některé [běžné ověření deklarace identity](#token-validation) v každý scénář.

### <a name="claims-in-id-and-access-tokens"></a>Deklarace identity v ID a přístupové tokeny
Pokud používáte Azure AD B2C, máte jemně odstupňovanou kontrolu nad hello obsah z vašich tokenů. Můžete nakonfigurovat [zásady](active-directory-b2c-reference-policies.md) toosend určité nastaví dat uživatele v deklaracích identity, které vaše aplikace vyžaduje pro její operace. Tyto deklarace identity můžou obsahovat standardní vlastnosti, například hello uživatele `displayName` a `emailAddress`. Může také obsahovat [vlastní uživatelské atributy](active-directory-b2c-reference-custom-attr.md) definující ve svém adresáři B2C. Každý ID a přístup token, který se zobrazí obsahuje sadu deklarací identity související se zabezpečením. Aplikace můžete použít tyto deklarace toosecurely ověření uživatelů a požadavky.

Všimněte si, že hello deklarací identity v tokenech ID nebudou zobrazeny v libovolném pořadí. Kromě toho může být nových deklarací identity zavedená v tokeny typu ID kdykoli. Aplikace by neměl rozdělit zavedeném nových deklarací identity. Tady jsou hello deklarací, které předpokládáte tooexist ID a přístupové tokeny vydané pomocí Azure AD B2C. Zásady jsou určeny žádné další deklarace identity. Postup kontroly hello deklarací identity v hello ukázkový token pro ID vložením do vyzkoušejte [calebb.net](http://calebb.net). Další podrobnosti naleznete v hello [OpenID Connect specifikace](http://openid.net/specs/openid-connect-core-1_0.html).

| Name (Název) | Deklarovat | Příklad hodnoty | Popis |
| --- | --- | --- | --- |
| Cílová skupina |`aud` |`90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` |Deklaraci identity cílovou skupinu identifikuje hello určené příjemce hello token. Pro Azure AD B2C cílová skupina hello je ID aplikace vaší aplikace, jako přiřazené tooyour aplikace v portálu pro registraci aplikace hello. Vaše aplikace by měl ověřit tuto hodnotu a odmítnout hello token, pokud neodpovídá. |
| Vystavitel |`iss` |`https://login.microsoftonline.com/775527ff-9a37-4307-8b3d-cc311f58d925/v2.0/` |Toto tvrzení identifikuje hello služby tokenů zabezpečení (STS), vytvoří a vrátí hello token. Také identifikuje hello adresář Azure AD ve které hello došlo k ověření uživatele. Aplikace by měl ověřit hello vystavitele deklarace, že tooensure, který hello tokenu pochází koncového bodu v2.0 hello Azure Active Directory. |
| vydané v |`iat` |`1438535543` |Touto deklarací identity je hello čas, na které hello byl vydán token, v čase epoch. |
| čas vypršení platnosti |`exp` |`1438539443` |Hello deklarace čas vypršení platnosti je čas hello, na které hello token stane neplatný, reprezentována v čase epoch. Aplikace by měla používat tato deklarace identity tooverify hello platnosti tokenu životnosti hello. |
| Neplatný před |`nbf` |`1438535543` |Touto deklarací identity je hello čas, na které hello stane token platný, reprezentována v čase epoch. To je obvykle hello stejné jako hello čas hello token vydán. Aplikace by měla používat tato deklarace identity tooverify hello platnosti tokenu životnosti hello. |
| Verze |`ver` |`1.0` |Je to hello verze hello ID tokenu, jak je definované ve službě Azure AD. |
| Kód hash |`c_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |Hodnota hash kódu je součástí ID token jenom v případě, že je hello token vydán společně s autorizační kód OAuth 2.0. Kód hash se dá použít toovalidate hello pravost autorizační kód. Další podrobnosti o tom tooperform toto ověření najdete v části hello [OpenID Connect specifikace](http://openid.net/specs/openid-connect-core-1_0.html).  |
| Hodnota hash tokenu přístupu |`at_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |Algoritmus hash tokenu přístupu je součástí ID token jenom v případě, že je hello token vydán společně s přístupový token OAuth 2.0. Algoritmus hash tokenu přístupu se dá použít toovalidate hello pravost přístupový token. Další podrobnosti o tom tooperform toto ověření najdete v části hello [OpenID Connect specifikace](http://openid.net/specs/openid-connect-core-1_0.html)  |
| hodnotu Nonce |`nonce` |`12345` |Hodnotu nonce je že strategie používá toomitigate opětovného přehrání tokenu útoky. Vaše aplikace může určit hodnotu nonce v jednom požadavku autorizace pomocí hello `nonce` parametr dotazu. Hodnota Hello zadáte v žádosti o hello bude vygenerované ponechat beze změny v hello `nonce` deklarací z tokenu ID. To umožňuje vaše aplikace tooverify hello hodnota proti hello hodnotu, kterou není zadán u hello požadavku, která přidruží dané ID token aplikace hello relace. Aplikace by měla provést toto ověření během procesu ověření tokenu ID hello. |
| Předmět |`sub` |`884408e1-2918-4cz0-b12d-3aa027d7563b` |Toto je hlavní hello, o které hello token vyhodnotí informace, například hello uživatele aplikace. Tato hodnota se nedá změnit a nemůže být přiřazeny nebo znovu použít. Lze použít tooperform kontroly autorizace, bezpečně, například když hello token je použité tooaccess prostředku. Deklarace identity subjektu hello se ve výchozím nastavení, zobrazí v ID objektu hello hello uživatele v adresáři hello. Další, najdete v části toolearn [Azure Active Directory B2C: Token, relace a konfigurace přihlášení](active-directory-b2c-token-session-sso.md). |
| Informace o ověřování kontextu – třída |`acr` |Neuvedeno |Není právě používána, s výjimkou v případě hello starší zásad. Další, najdete v části toolearn [Azure Active Directory B2C: Token, relace a konfigurace přihlášení](active-directory-b2c-token-session-sso.md). |
| Framework zásady důvěryhodnosti |`tfp` |`b2c_1_sign_in` |Toto je název hello hello zásady, které byly použité tooacquire hello ID token. |
| Doba ověřování |`auth_time` |`1438535543` |Tento požadavek je hello čas, kdy uživatel poslední zadaná pověření v epoch čase. |

### <a name="refresh-tokens"></a>Obnovovacích tokenů
Aktualizujte tokeny jsou tokeny zabezpečení, aplikace můžete použít nové tokeny ID tooacquire a přístup tokeny v tok OAuth 2.0. Poskytují aplikace s dlouhodobé tooresources přístup jménem uživatelů bez nutnosti interakci s tyto uživatele.

tooreceive obnovovací token v odpovědi tokenu aplikace musíte požádat o hello `offline_acesss` oboru. Další informace o hello toolearn `offline_access` obor, získáte toohello [referenční informace o Azure AD B2C protokolu](active-directory-b2c-reference-protocols.md).

Obnovovacích tokenů jsou a bude vždy, zcela neprůhledný tooyour aplikace. Vydal Azure AD a mohou být prověřovány a interpretovat jenom služby Azure AD. Jsou dlohotrvající, ale aplikace nemá zapisovat s hello očekávání, který bude poslední obnovovací token pro určité časové období. V každém okamžiku pro celou řadu důvodů může být zneplatněné obnovovacích tokenů. Hello tooattempt tooredeem je způsob, jak vaše aplikace tooknow pokud obnovovací token je platný pouze ji tak, že žádost o token tooAzure AD.

Pokud uplatníte obnovovací token pro nový token (a v případě, že aplikace byla udělena hello `offline_access` oboru), zobrazí se nové obnovovací token v odpovědi tokenu hello. Byste měli uložit hello nově vydané aktualizace tokenu. By mělo nahradit hello obnovovací token, který dřív používal v žádosti o hello. To pomáhá zaručit, že jsou dál platné pro stejně dlouho obnovovacích tokenů.

## <a name="token-validation"></a>Ověření tokenu
toovalidate token aplikace měli zkontrolovat hello podpis i deklarace identity hello tokenu.

Mnoho opensourcové knihovny jsou k dispozici pro ověření tokeny Jwt, v závislosti na vašem preferovaném jazyce. Doporučujeme místo před implementaci vlastní logiky ověřování prozkoumat tyto možnosti. Hello informací v tomto průvodci můžete zjistěte, jak tooproperly pomocí těchto knihoven.

### <a name="validate-hello-signature"></a>Ověření podpisu hello
Token JWT obsahuje tři segmenty, oddělených hello `.` znak. první segment Hello je hello *záhlaví*, hello druhou je hello *textu*, a třetí hello hello *podpis*. segment podpis Hello lze použít toovalidate hello pravosti tokenu hello tak, aby důvěryhodné aplikace.

Azure AD B2C tokeny jsou podepsány pomocí standardní asymetrických šifrovacích algoritmů, například RSA 256. Hlavička Hello hello tokenu obsahuje informace o klíči hello a metodu šifrování použít toosign hello token:

```
{
        "typ": "JWT",
        "alg": "RS256",
        "kid": "GvnPApfWMdLRi8PDmisFn7bprKg"
}
```

Hello `alg` deklarace identity označuje hello algoritmus, který byl použité toosign hello token. Hello `kid` deklarace identity označuje hello konkrétní veřejný klíč, který byl použité toosign hello token.

V každém okamžiku může Azure AD přihlášení token pomocí jedné sady páry klíčů veřejný soukromý. Azure AD otočí hello možných sadu klíče pravidelně, tak vaše aplikace by měly být zapsány toohandle tyto klíče se automaticky změní. Přiměřené frekvence toocheck pro aktualizace toohello veřejných klíčů používaných službou Azure AD je každých 24 hodin.

Azure AD B2C má koncový bod metadat OpenID Connect. To umožňuje aplikacím toofetch informace o Azure AD B2C za běhu. Tyto informace zahrnují koncových bodů, obsah tokenu a token podpisových klíčů. Dokument metadat JSON pro každou zásadu obsahuje adresáři B2C. Například dokumentu metadat hello hello `b2c_1_sign_in` zásad v `fabrikamb2c.onmicrosoft.com` se nachází na:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in
```

`fabrikamb2c.onmicrosoft.com`adresář hello B2C používá tooauthenticate hello uživatele, a `b2c_1_sign_in` zásad hello používá tooacquire hello token. toodetermine zásadu, pomocí které byla použité toosign token (a kde toogo toofetch hello metadata), máte dvě možnosti. Nejprve je název zásady hello součástí hello `acr` deklarací identity v tokenu hello. Můžete analyzovat deklarace identity z textu hello hello JWT podle textu hello dekódování kódování base-64 a deserializaci hello JSON řetězec této výsledky. Hello `acr` deklarací identity bude hello název hello zásady, které byly použité tooissue hello token.  Jinak se zásady hello tooencode v hello hodnotu hello `state` parametr při vydání hello požadavku a pak dekódovat toodetermine zásadu, které byl použit. Buď metoda je platná.

Dokument metadat Hello je objekt JSON, který obsahuje několik užitečné informací. Mezi ně patří hello umístění hello koncových bodů vyžaduje tooperform ověřování OpenID Connect. Zahrnují taky, že `jwks_uri`, což dává hello umístění sady hello veřejné klíče, které jsou používané toosign tokeny. Toto umístění je k dispozici zde ale dynamicky je nejlepší umístění hello toofetch pomocí dokument metadat hello a analýze se `jwks_uri`:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in
```

dokument JSON Hello umístěný na této adrese URL obsahuje všechny hello informací veřejného klíče používán v určitém okamžiku. Aplikace můžete použít hello `kid` deklarací identity v hello JWT záhlaví tooselect hello veřejný klíč v dokumentu JSON hello, který je použité toosign konkrétní token. Ověření podpisu je pak můžete provádět pomocí hello správný veřejný klíč a algoritmus uvedené hello.

Popis jak ověřit podpis tooperform je mimo rámec tohoto dokumentu hello. Mnoho opensourcové knihovny jsou k dispozici toohelp vám tato Pokud to potřebujete.

### <a name="validate-hello-claims"></a>Ověřit hello deklarace identity
Když aplikaci nebo API obdrží ID token, má také provést několik kontrol před hello nároky v tokenu ID hello. Ty zahrnují, ale nejsou omezeny na:

* Hello **cílovou skupinu** deklarací identity: tím ověříte, že hello ID token byl určený toobe zadané tooyour aplikace.
* Hello **neplatný před** a **čas vypršení platnosti** deklarací identity: tyto ověřte, že nevypršela platnost tohoto tokenu ID hello.
* Hello **vystavitele** deklarací: Tato ověřuje, že hello token vydán tooyour aplikace Azure AD.
* Hello **hodnotu nonce**: Toto je strategie pro snížení rizika útoku opětovného přehrání tokenu.

Úplný seznam ověření proveďte vaší aplikace, najdete v části toohello [OpenID Connect specifikace](https://openid.net). Podrobnosti o hello očekávaných hodnot pro tyto deklarace identity jsou součástí hello předchozí [token části](#types-of-tokens).  

## <a name="token-lifetimes"></a>Token životnosti
Následující token životnosti Hello jsou zadané toofurther vašeho vědomí. Můžou vám pomoct při vývoji a ladění aplikací. Všimněte si, že vaše aplikace by neměl být zapsána tooexpect některé z těchto životnosti tooremain konstanta. Mohou a změní. Další informace o hello [přizpůsobení tokenu životnosti](active-directory-b2c-token-session-sso.md) v Azure AD B2C.

| Token | Doba platnosti | Popis |
| --- | --- | --- |
| ID tokeny |Jedna hodina. |ID tokeny jsou obvykle platný jednu hodinu. Webové aplikace můžete použít tento životnost toomaintain vlastní relací uživatelů (doporučeno). Můžete také dobu platnosti jiné relace. Pokud aplikace potřebuje tooget nový token ID, jednoduše musí toomake nové žádosti o přihlášení tooAzure AD. Pokud má uživatel relace platná prohlížeče s Azure AD, tento uživatel nemusí být požadované tooenter přihlašovací údaje znovu. |
| Obnovovacích tokenů |Až too14 dnů |Jeden obnovovací token je platný pro maximálně 14 dní. Token obnovení však můžete stane neplatnou kdykoli z několika příčin. Aplikace by měly pokračovat tootry toouse obnovovací token, dokud hello požadavek selže, nebo dokud aplikace nahradí nový token obnovení hello. Obnovovací token se může stát také neplatné, pokud od uživatelů hello naposledy zadali přihlašovací údaje, uplynulo 90 dní. |
| Autorizačních kódů |Pět minut |Kódy ověřování jsou záměrně krátkodobou. Jejich by měl být uplatněn okamžitě přístupové tokeny, tokeny typu ID nebo tokeny obnovení při příjmu. |

