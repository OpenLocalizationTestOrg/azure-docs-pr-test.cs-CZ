---
title: "aaaAzure služby Active Directory v2.0 tokeny odkaz | Microsoft Docs"
description: "Hello typy tokenů a deklaracích identity vysílaných hello koncového bodu Azure AD v2.0"
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: dc58c282-9684-4b38-b151-f3e079f034fd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 29eb5c3402aeae302ee7c6234488520495f85904
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-v20-tokens-reference"></a>Přehled v2.0 tokeny služby Azure Active Directory
Hello koncového bodu v2.0 Azure Active Directory (Azure AD) vysílá několik typů tokeny zabezpečení v každé [tok ověřování](active-directory-v2-flows.md). Tento odkaz popisuje hello formátu, zabezpečení vlastnosti a obsah každého typu token.

> [!NOTE]
> koncový bod v2.0 Hello nepodporuje všechny scénáře Azure Active Directory a funkce. toodetermine zda byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).
>
>

## <a name="types-of-tokens"></a>Typy tokenů
Hello koncového bodu v2.0 podporuje hello [protokol autorizace OAuth 2.0](active-directory-v2-protocols.md), které používá přístup tokeny a obnovovacích tokenů. Hello koncového bodu v2.0 také podporuje ověřování a přihlásit se přes [OpenID Connect](active-directory-v2-protocols.md). OpenID Connect zavádí třetí typ tokenu, hello ID tokenu. Všechny tyto tokeny je reprezentován jako *nosiče* tokenu.

Token nosiče je chráněný prostředek, že uděluje hello tooa přístup nosiče tokenu lightweight zabezpečení. nosiče Hello je jakékoli strany, který může být hello token. I když může strana musí provést ověření pomocí Azure AD tooreceive hello nosný token, pokud kroky nejsou přijata toosecure hello token během přenosu a uložení, může být zachyceny a použity nezamýšleným strana. Některé tokeny zabezpečení mít předdefinované mechanismus tooprevent Neautorizováno stran pomocí, ale nezadávejte tokenů nosiče není. Nosné tokeny musí přenosu v zabezpečený kanál, jako je například transport layer security (HTTPS). Pokud bez tento typ zabezpečení je přenesen token nosiče, škodlivý strany může token "útok man-in-the-middle" tooacquire hello a použít je pro prostředek tooa chráněné neoprávněného přístupu. Hello stejné zabezpečení zásady platí při ukládání nebo ukládání do mezipaměti nosné tokeny pro pozdější použití. Vždy zajistěte, aby vaše aplikace bezpečně přenáší a ukládá nosné tokeny. Další informace o zabezpečení pro nosné tokeny, najdete v části [RFC 6750 část 5](http://tools.ietf.org/html/rfc6750).

Řadu hello tokeny vydané koncovým bodem v2.0 hello jsou implementované jako webové tokeny JSON (Jwt). Token JWT je tootransfer informací o compact a adresa URL bezpečných způsob, jak mezi dvěma účastníky. je volána Hello informace v token JWT *deklarace identity*. Je kontrolní výrazy informací o hello nosiče a předmět hello tokenu. deklarace identity Hello v token JWT jsou JavaScript Object Notation (JSON) objektů, které jsou zakódovány a serializovat pro přenos. Vzhledem k tomu hello Jwt vydán koncového bodu v2.0 hello jsou podepsán, ale nejsou šifrovaná, snadno si můžete prohlédnout hello obsah token JWT pro účely ladění. Další informace o Jwt, najdete v části hello [JWT specifikace](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>ID tokeny
ID token je formulář tokenu zabezpečení přihlášení, která vaše aplikace obdrží při ověřování se provádí pomocí [OpenID Connect](active-directory-v2-protocols.md). ID tokeny jsou reprezentovány jako [Jwt](#types-of-tokens), a obsahují deklarace identity, které můžete toosign hello uživatele v aplikaci tooyour. Hello deklarací identity můžete použít v tokenu ID různými způsoby. Správci obvykle používají informace o účtu pro ID tokeny toodisplay nebo toomake rozhodnutí o řízení přístupu v aplikaci. koncový bod v2.0 Hello vydává pouze jeden typ ID token, který má konzistentní sadu deklarací identity, bez ohledu na typ hello uživatele, který je přihlášený. Hello formát a obsah ID tokeny jsou text hello stejné pro osobní uživatele účtu Microsoft a pro pracovní nebo školní účty.

V současné době jsou tokeny typu ID podepsán, ale nejsou šifrovaná. Pokud vaše aplikace obdrží ID token, musí [ověřit podpis hello](#validating-tokens) tooprove hello pravosti tokenu a ověřit pár deklarace identity v tokenu tooprove hello jeho platnosti. Hello deklarace ověřené aplikací se liší v závislosti na požadavcích scénáře, ale aplikace musí provést některé [běžné ověření deklarace identity](#validating-tokens) v každý scénář.

Jsme získáte hello hello úplné podrobnosti o deklaracích identity v tokenech ID v následující části, kromě tooa ukázkový ID token. Všimněte si, že deklarace identity v tokenech ID nebudou zobrazeny v určitém pořadí. Navíc můžete nových deklarací identity zavedená do tokenů ID kdykoli. Aplikace by neměl přerušení, když jsou zavedené nových deklarací identity. Hello následující seznam obsahuje hello deklarace identity, které vaše aplikace aktuálně můžete spolehlivě interpretovat. Další informace naleznete v hello [OpenID Connect specifikace](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-id-token"></a>Ukázkový token pro ID
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiI2NzMxZGU3Ni0xNGE2LTQ5YWUtOTdiYy02ZWJhNjkxNDM5MWUiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vYjk0MTk4MTgtMDlhZi00OWMyLWIwYzMtNjUzYWRjMWYzNzZlL3YyLjAiLCJpYXQiOjE0NTIyODUzMzEsIm5iZiI6MTQ1MjI4NTMzMSwiZXhwIjoxNDUyMjg5MjMxLCJuYW1lIjoiQmFiZSBSdXRoIiwibm9uY2UiOiIxMjM0NSIsIm9pZCI6ImExZGJkZGU4LWU0ZjktNDU3MS1hZDkzLTMwNTllMzc1MGQyMyIsInByZWZlcnJlZF91c2VybmFtZSI6InRoZWdyZWF0YmFtYmlub0BueXkub25taWNyb3NvZnQuY29tIiwic3ViIjoiTUY0Zi1nZ1dNRWppMTJLeW5KVU5RWnBoYVVUdkxjUXVnNWpkRjJubDAxUSIsInRpZCI6ImI5NDE5ODE4LTA5YWYtNDljMi1iMGMzLTY1M2FkYzFmMzc2ZSIsInZlciI6IjIuMCJ9.p_rYdrtJ1oCmgDBggNHB9O38KTnLCMGbMDODdirdmZbmJcTHiZDdtTc-hguu3krhbtOsoYM2HJeZM3Wsbp_YcfSKDY--X_NobMNsxbT7bqZHxDnA2jTMyrmt5v2EKUnEeVtSiJXyO3JWUq9R0dO-m4o9_8jGP6zHtR62zLaotTBYHmgeKpZgTFB9WtUq8DVdyMn_HSvQEfz-LWqckbcTwM_9RNKoGRVk38KChVJo4z5LkksYRarDo8QgQ7xEKmYmPvRr_I7gvM2bmlZQds2OeqWLB1NSNbFZqyFOCgYn3bAQ-nEQSKwBaA36jYGPOVG2r2Qv1uKcpSOxzxaQybzYpQ
```

> [!TIP]
> Pro postup tooinspect hello deklarace identity ve hello ukázkový ID token, vložte hello ukázkový ID, token do [calebb.net](http://calebb.net/).
>
>

#### <a name="claims-in-id-tokens"></a>Deklarace identity v tokenech ID
| Name (Název) | Deklarovat | Příklad hodnoty | Popis |
| --- | --- | --- | --- |
| Cílová skupina |`aud` |`6731de76-14a6-49ae-97bc-6eba6914391e` |Identifikuje hello určené příjemce hello token. V ID tokeny cílová skupina hello je ID aplikace vaší aplikace, přidělené tooyour aplikace v portálu pro registraci aplikace Microsoft hello. Vaše aplikace by měl ověřit tuto hodnotu a odmítnout hello token, pokud hodnota hello neodpovídá. |
| Vystavitel |`iss` |`https://login.microsoftonline.com/b9419818-09af-49c2-b0c3-653adc1f376e/v2.0 ` |Identifikuje hello služby tokenů zabezpečení (STS), vytvoří a vrátí hello token a klient Azure AD hello, ve které hello došlo k ověření uživatele. Aplikace by měl ověřit hello vystavitele deklarace, že tooensure, který hello tokenu pochází koncového bodu v2.0 hello. Používejte ani hello GUID část hello toorestrict hello sadu deklarací klienty, kteří se můžete přihlásit toohello aplikace. Hello identifikátor GUID, který označuje, že uživatele hello je příjemce uživatel ze společnosti Microsoft je účet `9188040d-6c67-4c5b-b112-36a304b66dad`. |
| vydané v |`iat` |`1452285331` |Hello čas, na které hello byl vydán token, v čase epoch. |
| čas vypršení platnosti |`exp` |`1452289231` |v čase epoch reprezentované Hello čas, na které hello stává neplatným, token. Aplikace by měla používat tato deklarace identity tooverify hello platnosti tokenu životnosti hello. |
| Neplatný před |`nbf` |`1452285331` |v čase epoch reprezentované Hello čas, na které hello vstupuje v platnost, token. Je obvykle stejné jako čas vystavení hello hello. Aplikace by měla používat tato deklarace identity tooverify hello platnosti tokenu životnosti hello. |
| Verze |`ver` |`2.0` |verze Hello hello ID tokenu, jak je definované ve službě Azure AD. Pro koncový bod v2.0 hello, hodnota hello je `2.0`. |
| ID klienta |`tid` |`b9419818-09af-49c2-b0c3-653adc1f376e` |Identifikátor GUID, který představuje klienta hello Azure AD, která hello uživatele je z. Pro pracovní a školní účty je hello GUID hello neměnné klienta, ke které patří ID hello organizace, která hello uživatele. Pro osobní účty, hodnota hello je `9188040d-6c67-4c5b-b112-36a304b66dad`. Hello `profile` obor je požadován v pořadí tooreceive tuto deklaraci. |
| Kód hash |`c_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |Kód hash Hello je součástí tokeny typu ID jenom v případě, že je hello ID token vydán s kódem autorizace OAuth 2.0. Dá se použít toovalidate hello pravost autorizační kód. Podrobnosti o provedení tohoto ověření naleznete v tématu hello [OpenID Connect specifikace](http://openid.net/specs/openid-connect-core-1_0.html). |
| Hodnota hash tokenu přístupu |`at_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |hash tokenu přístupu Hello je součástí tokeny typu ID jenom v případě, že je hello ID token vydán s přístupový token OAuth 2.0. Dá se použít toovalidate hello pravost přístupový token. Podrobnosti o provedení tohoto ověření naleznete v tématu hello [OpenID Connect specifikace](http://openid.net/specs/openid-connect-core-1_0.html). |
| hodnotu Nonce |`nonce` |`12345` |hodnotu nonce Hello je strategie pro minimalizaci útoky opětovného přehrání tokenu. Vaše aplikace může určit hodnotu nonce v jednom požadavku autorizace pomocí hello `nonce` parametr dotazu. Hodnota Hello zadáte v žádosti o hello je vygenerované v tokenu ID hello `nonce` deklarace identity, ponechat beze změny. Aplikace můžete ověřit hodnoty hello s hello hodnotu, kterou není zadán u hello požadavku, která přidruží token konkrétní ID relace hello aplikace. Aplikace by měla provést toto ověření během procesu ověření tokenu ID hello. |
| jméno |`name` |`Babe Ruth` |deklarace identity názvu Hello poskytuje čitelná pro člověka hodnotu, která identifikuje hello subjektu hello tokenu. Hodnota Hello není zaručeno, že toobe jedinečné, je měnitelný a je určen toobe použít pouze pro účely zobrazení. Hello `profile` obor je požadován v pořadí tooreceive tuto deklaraci. |
| E-mailu |`email` |`thegreatbambino@nyy.onmicrosoft.com` |Hello primární e-mailová adresa přidružená hello uživatelský účet, pokud nějaká existuje. Její hodnota je měnitelný a může časem změnit. Hello `email` obor je požadován v pořadí tooreceive tuto deklaraci. |
| upřednostňované uživatelské jméno |`preferred_username` |`thegreatbambino@nyy.onmicrosoft.com` |Hello primární uživatelské jméno, který reprezentuje uživatele hello v koncového bodu v2.0 hello. To může být e-mailovou adresu, telefonní číslo nebo obecné uživatelské jméno bez zadaného formátu. Její hodnota je měnitelný a může časem změnit. Hello `profile` obor je požadován v pořadí tooreceive tuto deklaraci. |
| Předmět |`sub` |`MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q` | objekt zabezpečení Hello, o které hello token vyhodnotí informace, například hello uživatele aplikace. Tato hodnota se nedá změnit a nemůže být přiřazeny nebo znovu použít. Lze použít tooperform kontroly autorizace, bezpečně, například když je použité tooaccess prostředek hello token a slouží jako klíč v tabulkách databáze. Vzhledem k tomu, že je vždy k dispozici v tokenech hello hello předmětu, že Azure AD vydá, doporučujeme používat tuto hodnotu v systému pro obecné účely autorizace. předmět Hello je však identifikátor pairwise – je ID jedinečný tooa konkrétní aplikace.  Proto pokud jeden uživatel přihlásí do dvou různých aplikací pomocí dva identifikátory ID jiného klienta, tyto aplikace se zobrazí dvě různé hodnoty pro deklarace identity subjektu hello.  To se může nebo nemusí být potřeby v závislosti na požadavcích vaší architektury a ochrana osobních údajů. |
| ID objektu |`oid` |`a1dbdde8-e4f9-4571-ad93-3059e3750d23` | Hello neměnné identifikátor pro objekt v hello Microsoft identity systému, v takovém případě uživatelský účet.  Lze ji použít tooperform kontroly autorizace bezpečně a jako klíč v tabulkách databáze. Toto ID jednoznačně identifikuje uživatele hello napříč aplikacemi – přihlášení hello stejný uživatel obdrží dvě různé aplikace hello stejnou hodnotu v hello `oid` deklarací identity.  To znamená, že ho lze použít při zadávání dotazů tooMicrosoft online služby, jako je Microsoft Graph hello.  Toto ID hello vrátí Hello Microsoft Graph `id` vlastnost pro daný uživatelský účet.  Protože hello `oid` uživatelům víc aplikací toocorrelate, hello `profile` obor je požadován v pořadí tooreceive tuto deklaraci. Všimněte si, že pokud jeden uživatel existuje v několika klientech, hello uživatele bude obsahovat ID jiný objekt v každém klientovi – Přestože hello uživatel přihlásí do každého účtu s hello stejné přihlašovací údaje jsou považovány za různé účty. |

### <a name="access-tokens"></a>Přístupové tokeny
V současné době přístupové tokeny vydané koncového bodu v2.0 hello mohou být spotřebovávána pouze Microsoft Services. Vaše aplikace by neměl potřebovat tooperform žádné ověření nebo kontroly přístupové tokeny hello aktuálně podporované scénáře. Přístupové tokeny lze považovat za zcela neprůhledný. Jsou jenom řetězce, že vaše aplikace můžete předat tooMicrosoft požadavky HTTP.

V blízkosti budoucí, v2.0 hello hello koncový bod zavedete hello možnost pro vaše aplikace tooreceive přístupové tokeny z jiných klientů. V ten moment se aktualizují hello informace v tomto tématu odkaz na hello informace potřebné pro vaše aplikace tooperform přístup k ověření tokenu a další podobné úlohy.

Pokud budete požadovat token přístupu z koncového bodu v2.0 hello, hello koncového bodu v2.0 také vrátí hodnotu metadata o hello přístupový token pro toouse vaší aplikace. Tyto informace zahrnují čas vypršení platnosti hello hello přístupový token a hello oborů, pro které je platný. Vaše aplikace používá tato metadata tooperform inteligentního ukládání do mezipaměti přístupové tokeny bez nutnosti tooparse otevřete hello přístupový token, sám sebe.

### <a name="refresh-tokens"></a>Obnovovacích tokenů
Obnovovacích tokenů jsou tokeny zabezpečení, vaše aplikace můžete použít tooget nové přístup tokeny v tok OAuth 2.0. Aplikace můžete použít aktualizace tokeny tooachieve dlouhodobé přístup tooresources jménem uživatele bez nutnosti interakce s uživatelem hello.

Tokeny obnovení jsou více prostředků. Token obnovení obdržel během žádosti o token pro jeden prostředek lze uplatnit pro přístup k tokeny tooa úplně jiný prostředek.

tooreceive aktualizace v odpovědi tokenu, vaše aplikace musí požádat a udělit hello `offline_acesss` oboru. Další informace o hello toolearn `offline_access` obor najdete v tématu hello [souhlasu a obory](active-directory-v2-scopes.md) článku.

Obnovovacích tokenů jsou a vždy budou, zcela neprůhledný tooyour aplikace. Se vydávají koncového bodu v2.0 hello Azure AD a můžete být prověřovány a interpretovat koncového bodu v2.0 hello. Jsou dlohotrvající, ale aplikace nemá zapisovat tooexpect, který token obnovení vydrží období. V každém okamžiku z různých důvodů může být zneplatněné obnovovacích tokenů. Hello tooattempt tooredeem je způsob, jak vaše aplikace tooknow pokud obnovovací token je platný pouze ji tak, že koncový bod žádosti o token toohello v2.0.

Pokud uplatníte obnovovací token pro nový přístupový token (a pokud vaše aplikace byl udělen hello `offline_access` oboru), se zobrazí nový token aktualizace v odpovědi tokenu hello. Uložte hello nově vydané aktualizace tokenu, tooreplace hello jeden, které jste použili v žádosti o hello. To zaručuje, že jsou dál platné pro stejně dlouho obnovovacích tokenů.

## <a name="validating-tokens"></a>Ověřování tokenů
V současné době hello pouze ověření tokenu aplikace měli tooperform ověřuje tokeny typu ID. toovalidate ID token aplikace by měl ověřit token obou hello ID podpisu hello deklarace identity a v tokenu ID hello.

<!-- TODO: Link -->
Společnost Microsoft poskytuje knihovny a ukázky kódu, které ukazují, jak pracovat s tooeasily ověření tokenu. V dalších částech hello jsme popisují základní proces hello. Několik knihovny open-source třetí strany jsou také k dispozici pro ověřování JWT. Není k dispozici nejméně jedna knihovna možnost pro téměř každé platformě a jazyk.

### <a name="validate-hello-signature"></a>Ověření podpisu hello
Token JWT obsahuje tři segmenty, které jsou odděleny hello `.` znak. první segment Hello se označuje jako hello *záhlaví*, druhý segment hello je hello *textu*, a třetí segment hello je hello *podpis*. segment podpis Hello lze použít toovalidate hello pravosti tokenu ID hello tak, aby důvěryhodné aplikace.

ID tokeny jsou podepsány pomocí standardní asymetrických šifrovacích algoritmů, například RSA 256. Hello hlavička hello ID token obsahuje informace o klíči hello a metodu šifrování použít toosign hello token. Například:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

Hello `alg` deklarace identity označuje hello algoritmus, který byl použité toosign hello token. Hello `kid` deklarace identity označuje hello veřejný klíč, který byl použité toosign hello token.

V každém okamžiku může být koncového bodu v2.0 hello podepsat ID token pomocí kterékoli z konkrétní sadu páry klíčů veřejný soukromý. koncového bodu v2.0 Hello pravidelně otočí hello možné sady klíčů, takže vaše aplikace by měly být zapsány toohandle tyto klíče se automaticky změní. Přiměřené frekvence toocheck pro aktualizace toohello veřejné klíče používané koncového bodu v2.0 hello je každých 24 hodin.

Můžete získat hello podepisování klíčová data, které potřebujete toovalidate hello podpis pomocí hello OpenID Connect dokument metadat nacházející se v:

```
https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration
```

> [!TIP]
> Vyzkoušejte hello adresy URL v prohlížeči.
>
>

Tento dokument metadat je objekt JSON, který má několik užitečné informací, jako je například umístění hello hello různé koncové body požadované pro ověřování OpenID Connect.  Hello dokument taky obsahuje *jwks_uri*, což dává hello umístění sady hello veřejných klíčů, používá toosign tokeny. dokument JSON Hello nacházející se v hello jwks_uri má všechny hello informací veřejného klíče, který je aktuálně používán. Aplikace můžete použít hello `kid` deklarací identity v hello JWT záhlaví tooselect které veřejný klíč v tomto dokumentu bylo použité toosign token. Potom provede ověření podpisu pomocí hello správný veřejný klíč a algoritmus uvedené hello.

Provádění ověřování podpisu je mimo rámec tohoto dokumentu hello. Mnoho knihovny open-source jsou k dispozici toohelp vám to.

### <a name="validate-hello-claims"></a>Ověřit hello deklarace identity
Pokud vaše aplikace obdrží token ID při přihlášení uživatele, má také provést několik kontrol před hello nároky v tokenu ID hello. Patří mezi ně nejsou omezeny na:

* **Cílová skupina** deklarace identity, tooverify, který hello ID token byl určený toobe zadané tooyour aplikace
* **Neplatný před** a **čas vypršení platnosti** deklarací, jestli nevypršela platnost tooverify, který hello ID token
* **Vystavitel** deklarace identity, tooverify, který hello token byl vydán tooyour aplikace koncového bodu v2.0 hello
* **hodnotu Nonce**, jako token přehráním snížení rizika útoku

Úplný seznam ověření deklarace identity, které vaše aplikace by měla provádět, naleznete v části hello [OpenID Connect specifikace](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Podrobnosti o hello očekávaných hodnot pro tyto deklarace identity jsou součástí hello [tokeny typu ID](# ID tokens) části.

## <a name="token-lifetimes"></a>Token životnosti
Poskytujeme hello následující životnosti tokenu pro vás pouze informační charakter. Hello informace vám mohou pomoci při vývoji a ladění aplikací. Vaše aplikace by neměl být zapsána tooexpect některé z těchto životnosti tooremain konstanta. Token může životnosti a bude kdykoli změnit.

| Token | Doba platnosti | Popis |
| --- | --- | --- |
| ID tokeny (pracovní nebo školní účty) |1 hodina |Tokeny typu ID obvykle jsou platné po dobu 1 hodina. Webové aplikace můžete použít tento stejný toomaintain životního cyklu, které vlastní relaci s hello uživatele (doporučeno), nebo se můžete rozhodnout životnost úplně jiné relace. Pokud aplikace potřebuje tooget nový token ID, musí žádosti toomake nové přihlášení toohello v2.0 zajistí autorizaci koncového bodu. Pokud má uživatel hello relace platná prohlížeče s koncovým bodem v2.0 hello, hello uživatele není může být požadované tooenter své přihlašovací údaje znovu. |
| ID tokeny (osobní účty) |24 hodin |ID tokeny pro osobní účty obvykle jsou platné po dobu 24 hodin. Webové aplikace můžete použít tento stejný toomaintain životního cyklu, které vlastní relaci s hello uživatele (doporučeno), nebo se můžete rozhodnout životnost úplně jiné relace. Pokud aplikace potřebuje tooget nový token ID, musí žádosti toomake nové přihlášení toohello v2.0 zajistí autorizaci koncového bodu. Pokud má uživatel hello relace platná prohlížeče s koncovým bodem v2.0 hello, hello uživatele není může být požadované tooenter své přihlašovací údaje znovu. |
| Přístupové tokeny (pracovní nebo školní účty) |1 hodina |Jako součást tokenu metadata hello uvedené v odpovědi tokenu. |
| Přístupové tokeny (osobní účty) |1 hodina |Jako součást tokenu metadata hello uvedené v odpovědi tokenu. Přístupové tokeny, které jsou vydány jménem osobní účty je možné nakonfigurovat pro různé životnost, ale je typické 1 hodina. |
| Obnovovacích tokenů (pracovní nebo školní účet) |Až too14 dnů |Jeden obnovovací token je platný pro maximálně 14 dní. Token obnovení hello však může stane neplatnou kdykoli z různých důvodů, tak vaše aplikace by měla tootry toouse token obnovení pokračovat, dokud se nezdaří, nebo dokud aplikace ho nahradí nový token obnovení. Token obnovení také stává neplatným, pokud to bylo 90 dnů od hello uživatel zadá své přihlašovací údaje. |
| Obnovovacích tokenů (osobní účty) |Až too1 roku |Jeden obnovovací token je platný pro nesmí být delší než 1 rok. Ale hello obnovovací token může zneplatní kdykoli z různých důvodů, tak vaše aplikace by měly pokračovat ve tootry toouse token obnovení, dokud se nezdaří. |
| Autorizačních kódů (pracovní nebo školní účty) |10 minut |Autorizace kódy jsou účelově krátkodobou a by měl být okamžitě uplatněn přístupové tokeny a obnovovacích tokenů při přijetí hello tokeny. |
| Autorizačních kódů (osobní účty) |5 minut |Autorizace kódy jsou účelově krátkodobou a by měl být okamžitě uplatněn přístupové tokeny a obnovovacích tokenů při přijetí hello tokeny. Autorizace kódy, které jsou vydány jménem osobní účty, jsou pro jednorázové použití. |
