---
title: "koncový bod v2.0 aaaChanges toohello Azure AD | Microsoft Docs"
description: "Popis změny, které jsou určeny jako toohello aplikace modelu v2.0 verzi public preview protokoly."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: e6c5b891-0b5d-47dc-8184-5d0ef6a1a006
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2016
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: d7b28a481e12d5dbbc4a10110193bdbd754f4929
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="important-updates-toohello-v20-authentication-protocols"></a>Důležité aktualizace toohello v2.0 ověřovací protokoly
Vývojáři pozornost! Přes hello další dva týdny, budeme provádět několik aktualizací tooour v2.0 ověřovací protokoly, které může to znamenat nejnovější změny pro všechny aplikace, který jste napsali naše období preview.  

## <a name="who-does-this-affect"></a>Kdo to vliv?
Všechny aplikace, který byl toouse hello v2.0 konvergované koncový bod ověřování,

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
```

Další informace o koncového bodu v2.0 hello můžete najít [zde](active-directory-appmodel-v2-overview.md).

Pokud jste vytvořili aplikaci pomocí koncového bodu v2.0 hello kódování přímo toohello v2.0 protokol, pomocí některé z našich webových middlewares OpenID Connect nebo OAuth, nebo pomocí jiné 3. stran knihovny tooperform ověřování, musí být připravené tootest své projekty a zpřístupnění změny v případě potřeby.

## <a name="who-doesnt-this-affect"></a>Kdo nemá to vliv?
Žádné aplikace, které byly napsaných pro koncový bod ověřování hello produkční Azure AD,

```
https://login.microsoftonline.com/common/oauth2/authorize
```

Tento protokol je nastaven trvalé a nebude zaznamenat všechny změny.

Kromě toho pokud vaše aplikace **pouze** používá ověřování tooperform naše knihovny ADAL, nebudete mít toochange nic.  ADAL má Stíněný aplikaci z hello změny.  

## <a name="what-are-hello-changes"></a>Jaké jsou hello změny?
### <a name="removing-hello-x5t-value-from-jwt-headers"></a>Odebráním hello x5t hodnotu hlavičky JWT
koncový bod v2.0 Hello používá tokeny JWT hojně, které obsahují záhlaví parametry s relevantní metadata o hello token.  Pokud jste dekódovat hello záhlaví jednoho z našich aktuální tokeny Jwt, by se najít něco podobného jako:

```
{ 
    "type": "JWT",
    "alg": "RS256",
    "x5t": "MnC_VZcATfM5pOYiJHMba9goEKY",
    "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

Kde obě vlastnosti "x5t" a "kid" hello identifikovat hello veřejný klíč, měl by být podpis tokenu hello použité toovalidate načítají koncový bod metadat OpenID Connect hello.

Hello změny, které zde vydáváme je vlastnost tooremove hello "x5t".  Můžete pokračovat v toouse hello stejné mechanismy toovalidate tokeny, ale spoléhat na hello "kid" vlastnost tooretrieve hello správný veřejný klíč, jako zadaný v hello protokolu OpenID Connect. 

> [!IMPORTANT]
> **Úlohu: Zajistěte, aby vaše aplikace není závislá na existenci hello hello x5t hodnotu.**
> 
> 

### <a name="removing-profileinfo"></a>Odebrání profile_info
Dříve, koncového bodu v2.0 hello má byla vrací objekt JSON kódováním base64 v tokenu odpovědí názvem `profile_info`.  Žádosti o token přístupu z koncového bodu v2.0 hello odesláním požadavku na:

```
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Hello odpověď bude vypadat hello následující objekt JSON:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Hello `profile_info` hodnota obsažená informace o hello uživatele, který podepsal do aplikace hello - jejich název zobrazení, křestní jméno, příjmení, e-mailovou adresu, identifikátor a tak dále.  Především se stává, hello `profile_info` byl použit pro ukládání tokenu do mezipaměti a zobrazit účely.

Nyní jsme se odebrat hello `profile_info` hodnotu – ale nemusíte si dělat starosti, stále poskytujeme tento toodevelopers informace na místě, mírně lišit.  Místo `profile_info`, koncového bodu v2.0 hello bude nyní se vraťte `id_token` v každé odpovědi tokenu:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Může dekódovat a analyzovat požadavku id_token hello tooretrieve hello stejné informace, které jste dostali od profile_info.  požadavku id_token Hello je JSON Web Token (JWT), s obsahem podle specifikace OpenID Connect.  Hello kód pro to by měl být velmi podobné – stačí tooextract hello střední segmentu (textu hello) požadavku id_token hello a base64 dekódovat tooaccess hello objekt JSON v rámci.

Přes hello další dva týdny, můžete by měl kód aplikace tooretrieve hello informace o uživateli z buď hello `id_token` nebo `profile_info`; podle toho, co je k dispozici.  Tímto způsobem hello se při změně, vaše aplikace může bezproblémově zpracovat hello přechod z `profile_info` příliš`id_token` bez přerušení.

> [!IMPORTANT]
> **Úlohu: Zajistěte, aby vaše aplikace není závislá na existenci hello hello `profile_info` hodnotu.**
> 
> 

### <a name="removing-idtokenexpiresin"></a>Odebrání id_token_expires_in
Podobně jako příliš`profile_info`, jsme také odebírání hello `id_token_expires_in` parametr z odpovědi.  Dřív by koncového bodu v2.0 hello vrátit hodnotu `id_token_expires_in` společně s každou požadavku id_token odpověď, například v odpovědi autorizovat:

```
https://myapp.com?id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...&id_token_expires_in=3599...
```

Nebo v odpovědi tokenu:

```
{ 
    "token_type": "Bearer",
    "id_token_expires_in": 3599,
    "scope": "openid",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Hello `id_token_expires_in` hodnota by znamenala hello počet sekund, po požadavku id_token hello by zůstat platné pro.  Nyní jsme se odebrat hello `id_token_expires_in` hodnotu úplně.  Místo toho můžete použít standardní OpenID Connect hello `nbf` a `exp` tooexamine platnost hello požadavku id_token deklarace identity.  V tématu hello [odkaz tokenu v2.0](active-directory-v2-tokens.md) Další informace o těchto deklaracích identity.

> [!IMPORTANT]
> **Úlohu: Zajistěte, aby vaše aplikace není závislá na existenci hello hello `id_token_expires_in` hodnotu.**
> 
> 

### <a name="changing-hello-claims-returned-by-scopeopenid"></a>Změna hello deklarací identity vrátí oborem = openid
Tato změna bude hello nejvýznamnějších – ve skutečnosti, bude to mít vliv téměř každé aplikaci, která používá koncového bodu v2.0 hello.  Mnoho aplikací odeslání žádosti o toohello koncového bodu v2.0 pomocí hello `openid` nastavit obor, jako je třeba:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid offline_access https://outlook.office.com/mail.read
```

Dnes, když uživatel hello uděluje souhlas pro hello `openid` oboru, vaše aplikace obdrží velkému množství informací o uživateli hello v požadavku id_token výsledná hello.  Tyto deklarace identity můžou obsahovat jména, upřednostňované uživatelské jméno, e-mailovou adresu, ID objektu a další.

V této aktualizaci měníme hello informace této hello `openid` oboru umožňuje vaší aplikace přístup k toobetter comform s hello specifikace OpenID Connect.  Hello `openid` obor bude pouze povolit vaší aplikace toosign hello uživateli v a přijímat identifikátor specifický pro aplikace pro uživatele hello v hello `sub` deklarace identity z požadavku id_token hello.  Hello deklarací identity v požadavku id_token s pouze hello `openid` rozsah povolen bude nemá žádné identifikovatelné osobní údaje.  Příklad požadavku id_token deklarace jsou:

```
{ 
    "aud": "580e250c-8f26-49d0-bee8-1c078add1609",
    "iss": "https://login.microsoftonline.com/b9410318-09af-49c2-b0c3-653adc1f376e/v2.0 ",
    "iat": 1449520283,
    "nbf": 1449520283,
    "exp": 1449524183,
    "nonce": "12345",
    "sub": "MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q",
    "tid": "b9410318-09af-49c2-b0c3-653adc1f376e",
    "ver": "2.0",
}
```

Pokud chcete tooobtain identifikovatelné osobní údaje (PII) o hello uživateli ve vaší aplikaci, vaše aplikace bude potřebovat další oprávnění toorequest od uživatele hello.  Představujeme podporu pro dva nové obory z OpenID Connect specifikace hello – hello `email` a `profile` obory – které umožňují toodo tak.

* Hello `email` obor je velmi jednoduchá – umožňuje vaší aplikace přístup toohello primární e-mailovou adresu uživatele prostřednictvím hello `email` deklarací identity v požadavku id_token hello.  Všimněte si, že hello `email` deklarace identity vždycky nebude přítomný v id_tokens – pouze budou zahrnuty Pokud je k dispozici v profilu uživatele hello.
* Hello `profile` oboru poskytuje tooall přístup k vaší aplikaci další základní informace o uživateli hello – jejich název, upřednostňované uživatelského jména, ID objektu a tak dále.

To vám umožní toocode aplikace způsobem minimální zpřístupnění – můžete požádat hello uživatele pro právě hello sadu informace, že vaše aplikace vyžaduje toodo úlohy.  Pokud chcete toocontinue získávání hello kompletní informace o uživateli, který aktuálně přijímá aplikace, by měla obsahovat všechny tři obory žádostí autorizace:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid profile email offline_access https://outlook.office.com/mail.read
```

Aplikace můžete zahájit odesílání hello `email` a `profile` obory okamžitě a koncového bodu v2.0 hello přijmout tyto dva obory a začít vyžadování oprávnění od uživatelů podle potřeby.  Ale hello změnit v hello výklad hello `openid` oboru se neprojeví pro několik týdnů.

> [!IMPORTANT]
> **Úlohu: přidejte hello `profile` a `email` rozsahy, pokud vaše aplikace vyžaduje informace o uživateli hello.**  Všimněte si, že ADAL bude obsahovat obě tato oprávnění v žádostech o ve výchozím nastavení. 
> 
> 

### <a name="removing-hello-issuer-trailing-slash"></a>Odebrání hello vystavitele koncové lomítko.
Dříve hello vystavitele hodnotu, která se zobrazí v tokenech z koncového bodu v2.0 hello trvalo hello formuláře

```
https://login.microsoftonline.com/{some-guid}/v2.0/
```

Kde hello guid je tenantId hello klienta hello Azure AD, který vydal hello token.  Tyto změny se změní na hodnotu vystavitele hello

```
https://login.microsoftonline.com/{some-guid}/v2.0 
```

oba tokeny a v dokumentu zjišťování OpenID Connect hello.

> [!IMPORTANT]
> **Úlohu: Zajistěte, aby vaše aplikace přijímá během ověřování vystavitele hello vystavitele hodnotu s i bez koncové lomítko.**
> 
> 

## <a name="why-change"></a>Proč změnit?
Hello primární motivace pro představení těchto změn je kompatibilní s hello OpenID Connect standardní specifikace toobe.  Díky OpenID Connect kompatibilní Doufáme toominimize rozdíly mezi integrace se službou Microsoft identity services a s jinými službami identity v odvětví hello.  Vývojáři toouse tooenable chceme bez nutnosti tooalter hello knihovny tooaccommodate Microsoft rozdíly své oblíbené opensourcové knihovny ověřování.

## <a name="what-can-you-do"></a>Co můžete dělat?
K dnešnímu dni můžete začít vytváření všechny změny hello popsané výše.  Měli byste okamžitě:

1. **Odeberte všechny závislosti hello `x5t` parametr záhlaví.**
2. **Pohodlné zpracování hello přechod z `profile_info` příliš`id_token` v odpovědi tokenu.**
3. **Odeberte všechny závislosti hello `id_token_expires_in` parametr odpovědi.**
4. **Přidat hello `profile` a `email` obory tooyour aplikace, pokud aplikace potřebuje základní uživatelské informace.**
5. **Přijměte vystavitele hodnoty v tokenech s i bez koncové lomítko.**

Naše [dokumentace k protokolu v2.0](active-directory-v2-protocols.md) již bylo aktualizované tooreflect tyto změny, můžete ho použít jako odkaz v pomáhá aktualizace kódu.

Pokud máte další dotazy na obor hello hello změn, můžete volné tooreach out toous na Twitteru v @AzureAD.

## <a name="how-often-will-protocol-changes-occur"></a>Jak často bude protokol změnách?
Není jsme předvídáte, že žádné další nejnovější změny toohello ověřovací protokoly.  Tyto změny do jedné verze jsme jsou sdružování záměrně, takže nebudete mít toogo prostřednictvím tento typ procesu aktualizace znovu kdykoli brzy.  Samozřejmě, bude se pokračovat tooadd funkce toohello konvergované v2.0 ověřovací služba, která můžete využít výhod, ale tyto změny by měl být doplňkové a není zalomení existující kód.

Nakonec rádi bychom znali toosay Děkujeme vám za vyzkoušení věcí během období preview hello.  Statistika Hello a zkušenosti naše inovátoři být neocenitelnou pomocí doposud a Věříme, že tooshare budete pokračovat, vaše názory a návrhy.

Kódování radostí!

Hello Microsoft Identity dělení

