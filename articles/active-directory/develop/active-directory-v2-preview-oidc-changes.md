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
# <a name="important-updates-toohello-v20-authentication-protocols"></a><span data-ttu-id="d0ddb-103">Důležité aktualizace toohello v2.0 ověřovací protokoly</span><span class="sxs-lookup"><span data-stu-id="d0ddb-103">Important Updates toohello v2.0 Authentication Protocols</span></span>
<span data-ttu-id="d0ddb-104">Vývojáři pozornost!</span><span class="sxs-lookup"><span data-stu-id="d0ddb-104">Attention developers!</span></span> <span data-ttu-id="d0ddb-105">Přes hello další dva týdny, budeme provádět několik aktualizací tooour v2.0 ověřovací protokoly, které může to znamenat nejnovější změny pro všechny aplikace, který jste napsali naše období preview.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-105">Over hello next two weeks, we will be making a few updates tooour v2.0 authentication protocols that may mean breaking changes for any apps you have written during our preview period.</span></span>  

## <a name="who-does-this-affect"></a><span data-ttu-id="d0ddb-106">Kdo to vliv?</span><span class="sxs-lookup"><span data-stu-id="d0ddb-106">Who does this affect?</span></span>
<span data-ttu-id="d0ddb-107">Všechny aplikace, který byl toouse hello v2.0 konvergované koncový bod ověřování,</span><span class="sxs-lookup"><span data-stu-id="d0ddb-107">Any apps that have been written toouse hello v2.0 converged authentication endpoint,</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
```

<span data-ttu-id="d0ddb-108">Další informace o koncového bodu v2.0 hello můžete najít [zde](active-directory-appmodel-v2-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d0ddb-108">More information on hello v2.0 endpoint can be found [here](active-directory-appmodel-v2-overview.md).</span></span>

<span data-ttu-id="d0ddb-109">Pokud jste vytvořili aplikaci pomocí koncového bodu v2.0 hello kódování přímo toohello v2.0 protokol, pomocí některé z našich webových middlewares OpenID Connect nebo OAuth, nebo pomocí jiné 3. stran knihovny tooperform ověřování, musí být připravené tootest své projekty a zpřístupnění změny v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-109">If you have built an app using hello v2.0 endpoint by coding directly toohello v2.0 protocol, using any of our OpenID Connect or OAuth web middlewares, or using other 3rd party libraries tooperform authentication, you should be prepared tootest your projects and make changes if necessary.</span></span>

## <a name="who-doesnt-this-affect"></a><span data-ttu-id="d0ddb-110">Kdo nemá to vliv?</span><span class="sxs-lookup"><span data-stu-id="d0ddb-110">Who doesn\`t this affect?</span></span>
<span data-ttu-id="d0ddb-111">Žádné aplikace, které byly napsaných pro koncový bod ověřování hello produkční Azure AD,</span><span class="sxs-lookup"><span data-stu-id="d0ddb-111">Any apps that have been written against hello production Azure AD authentication endpoint,</span></span>

```
https://login.microsoftonline.com/common/oauth2/authorize
```

<span data-ttu-id="d0ddb-112">Tento protokol je nastaven trvalé a nebude zaznamenat všechny změny.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-112">This protocol is set in stone and will not be experiencing any changes.</span></span>

<span data-ttu-id="d0ddb-113">Kromě toho pokud vaše aplikace **pouze** používá ověřování tooperform naše knihovny ADAL, nebudete mít toochange nic.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-113">Furthermore, if your app **only** uses our ADAL library tooperform authentication, you won\`t have toochange anything.</span></span>  <span data-ttu-id="d0ddb-114">ADAL má Stíněný aplikaci z hello změny.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-114">ADAL has shielded your app from hello changes.</span></span>  

## <a name="what-are-hello-changes"></a><span data-ttu-id="d0ddb-115">Jaké jsou hello změny?</span><span class="sxs-lookup"><span data-stu-id="d0ddb-115">What are hello changes?</span></span>
### <a name="removing-hello-x5t-value-from-jwt-headers"></a><span data-ttu-id="d0ddb-116">Odebráním hello x5t hodnotu hlavičky JWT</span><span class="sxs-lookup"><span data-stu-id="d0ddb-116">Removing hello x5t value from JWT headers</span></span>
<span data-ttu-id="d0ddb-117">koncový bod v2.0 Hello používá tokeny JWT hojně, které obsahují záhlaví parametry s relevantní metadata o hello token.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-117">hello v2.0 endpoint uses JWT tokens extensively, which contain a header parameters section with relevant metadata about hello token.</span></span>  <span data-ttu-id="d0ddb-118">Pokud jste dekódovat hello záhlaví jednoho z našich aktuální tokeny Jwt, by se najít něco podobného jako:</span><span class="sxs-lookup"><span data-stu-id="d0ddb-118">If you decode hello header of one of our current JWTs, you would find something like:</span></span>

```
{ 
    "type": "JWT",
    "alg": "RS256",
    "x5t": "MnC_VZcATfM5pOYiJHMba9goEKY",
    "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

<span data-ttu-id="d0ddb-119">Kde obě vlastnosti "x5t" a "kid" hello identifikovat hello veřejný klíč, měl by být podpis tokenu hello použité toovalidate načítají koncový bod metadat OpenID Connect hello.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-119">Where both hello "x5t" and "kid" properties identify hello public key that should be used toovalidate hello token\`s signature, as retrieved from hello OpenID Connect metadata endpoint.</span></span>

<span data-ttu-id="d0ddb-120">Hello změny, které zde vydáváme je vlastnost tooremove hello "x5t".</span><span class="sxs-lookup"><span data-stu-id="d0ddb-120">hello change we are making here is tooremove hello "x5t" property.</span></span>  <span data-ttu-id="d0ddb-121">Můžete pokračovat v toouse hello stejné mechanismy toovalidate tokeny, ale spoléhat na hello "kid" vlastnost tooretrieve hello správný veřejný klíč, jako zadaný v hello protokolu OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-121">You may continue toouse hello same mechanisms toovalidate tokens, but should rely only on hello "kid" property tooretrieve hello correct public key, as specified in hello OpenID Connect protocol.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="d0ddb-122">**Úlohu: Zajistěte, aby vaše aplikace není závislá na existenci hello hello x5t hodnotu.**</span><span class="sxs-lookup"><span data-stu-id="d0ddb-122">**Your job: Make sure your app does not depend on hello existence of hello x5t value.**</span></span>
> 
> 

### <a name="removing-profileinfo"></a><span data-ttu-id="d0ddb-123">Odebrání profile_info</span><span class="sxs-lookup"><span data-stu-id="d0ddb-123">Removing profile_info</span></span>
<span data-ttu-id="d0ddb-124">Dříve, koncového bodu v2.0 hello má byla vrací objekt JSON kódováním base64 v tokenu odpovědí názvem `profile_info`.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-124">Previously, hello v2.0 endpoint has been returning a base64 encoded JSON object in token responses called `profile_info`.</span></span>  <span data-ttu-id="d0ddb-125">Žádosti o token přístupu z koncového bodu v2.0 hello odesláním požadavku na:</span><span class="sxs-lookup"><span data-stu-id="d0ddb-125">When requesting an access token from hello v2.0 endpoint by sending a request to:</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

<span data-ttu-id="d0ddb-126">Hello odpověď bude vypadat hello následující objekt JSON:</span><span class="sxs-lookup"><span data-stu-id="d0ddb-126">hello response would look like hello following JSON object:</span></span>

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

<span data-ttu-id="d0ddb-127">Hello `profile_info` hodnota obsažená informace o hello uživatele, který podepsal do aplikace hello - jejich název zobrazení, křestní jméno, příjmení, e-mailovou adresu, identifikátor a tak dále.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-127">hello `profile_info` value contained information about hello user who signed into hello app - their display name, first name, surname, email address, identifier, and so on.</span></span>  <span data-ttu-id="d0ddb-128">Především se stává, hello `profile_info` byl použit pro ukládání tokenu do mezipaměti a zobrazit účely.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-128">Primarily, hello `profile_info` was used for token caching and display purposes.</span></span>

<span data-ttu-id="d0ddb-129">Nyní jsme se odebrat hello `profile_info` hodnotu – ale nemusíte si dělat starosti, stále poskytujeme tento toodevelopers informace na místě, mírně lišit.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-129">We are now removing hello `profile_info` value – but don't worry, we are still providing this information toodevelopers in a slightly different place.</span></span>  <span data-ttu-id="d0ddb-130">Místo `profile_info`, koncového bodu v2.0 hello bude nyní se vraťte `id_token` v každé odpovědi tokenu:</span><span class="sxs-lookup"><span data-stu-id="d0ddb-130">Instead of `profile_info`, hello v2.0 endpoint will now return an `id_token` in each token response:</span></span>

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

<span data-ttu-id="d0ddb-131">Může dekódovat a analyzovat požadavku id_token hello tooretrieve hello stejné informace, které jste dostali od profile_info.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-131">You may decode and parse hello id_token tooretrieve hello same information that you received from profile_info.</span></span>  <span data-ttu-id="d0ddb-132">požadavku id_token Hello je JSON Web Token (JWT), s obsahem podle specifikace OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-132">hello id_token is a JSON Web Token (JWT), with contents as specified by OpenID Connect.</span></span>  <span data-ttu-id="d0ddb-133">Hello kód pro to by měl být velmi podobné – stačí tooextract hello střední segmentu (textu hello) požadavku id_token hello a base64 dekódovat tooaccess hello objekt JSON v rámci.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-133">hello code for doing so should be very similar – you simply need tooextract hello middle segment (hello body) of hello id_token and base64 decode it tooaccess hello JSON object within.</span></span>

<span data-ttu-id="d0ddb-134">Přes hello další dva týdny, můžete by měl kód aplikace tooretrieve hello informace o uživateli z buď hello `id_token` nebo `profile_info`; podle toho, co je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-134">Over hello next two weeks, you should code your app tooretrieve hello user information from either hello `id_token` or `profile_info`; whichever is present.</span></span>  <span data-ttu-id="d0ddb-135">Tímto způsobem hello se při změně, vaše aplikace může bezproblémově zpracovat hello přechod z `profile_info` příliš`id_token` bez přerušení.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-135">That way when hello change is made, your app can seamlessly handle hello transition from `profile_info` too`id_token` without interruption.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d0ddb-136">**Úlohu: Zajistěte, aby vaše aplikace není závislá na existenci hello hello `profile_info` hodnotu.**</span><span class="sxs-lookup"><span data-stu-id="d0ddb-136">**Your job: Make sure your app does not depend on hello existence of hello `profile_info` value.**</span></span>
> 
> 

### <a name="removing-idtokenexpiresin"></a><span data-ttu-id="d0ddb-137">Odebrání id_token_expires_in</span><span class="sxs-lookup"><span data-stu-id="d0ddb-137">Removing id_token_expires_in</span></span>
<span data-ttu-id="d0ddb-138">Podobně jako příliš`profile_info`, jsme také odebírání hello `id_token_expires_in` parametr z odpovědi.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-138">Similar too`profile_info`, we are also removing hello `id_token_expires_in` parameter from responses.</span></span>  <span data-ttu-id="d0ddb-139">Dřív by koncového bodu v2.0 hello vrátit hodnotu `id_token_expires_in` společně s každou požadavku id_token odpověď, například v odpovědi autorizovat:</span><span class="sxs-lookup"><span data-stu-id="d0ddb-139">Previously, hello v2.0 endpoint would return a value for `id_token_expires_in` along with each id_token response, for instance in an authorize response:</span></span>

```
https://myapp.com?id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...&id_token_expires_in=3599...
```

<span data-ttu-id="d0ddb-140">Nebo v odpovědi tokenu:</span><span class="sxs-lookup"><span data-stu-id="d0ddb-140">Or in a token response:</span></span>

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

<span data-ttu-id="d0ddb-141">Hello `id_token_expires_in` hodnota by znamenala hello počet sekund, po požadavku id_token hello by zůstat platné pro.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-141">hello `id_token_expires_in` value would indicate hello number of seconds hello id_token would remain valid for.</span></span>  <span data-ttu-id="d0ddb-142">Nyní jsme se odebrat hello `id_token_expires_in` hodnotu úplně.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-142">Now, we are removing hello `id_token_expires_in` value completely.</span></span>  <span data-ttu-id="d0ddb-143">Místo toho můžete použít standardní OpenID Connect hello `nbf` a `exp` tooexamine platnost hello požadavku id_token deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-143">Instead, you may use hello OpenID Connect standard `nbf` and `exp` claims tooexamine hello validity of an id_token.</span></span>  <span data-ttu-id="d0ddb-144">V tématu hello [odkaz tokenu v2.0](active-directory-v2-tokens.md) Další informace o těchto deklaracích identity.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-144">See hello [v2.0 token reference](active-directory-v2-tokens.md) for more information on these claims.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d0ddb-145">**Úlohu: Zajistěte, aby vaše aplikace není závislá na existenci hello hello `id_token_expires_in` hodnotu.**</span><span class="sxs-lookup"><span data-stu-id="d0ddb-145">**Your job: Make sure your app does not depend on hello existence of hello `id_token_expires_in` value.**</span></span>
> 
> 

### <a name="changing-hello-claims-returned-by-scopeopenid"></a><span data-ttu-id="d0ddb-146">Změna hello deklarací identity vrátí oborem = openid</span><span class="sxs-lookup"><span data-stu-id="d0ddb-146">Changing hello claims returned by scope=openid</span></span>
<span data-ttu-id="d0ddb-147">Tato změna bude hello nejvýznamnějších – ve skutečnosti, bude to mít vliv téměř každé aplikaci, která používá koncového bodu v2.0 hello.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-147">This change will be hello most significant – in fact, it will affect almost every app that uses hello v2.0 endpoint.</span></span>  <span data-ttu-id="d0ddb-148">Mnoho aplikací odeslání žádosti o toohello koncového bodu v2.0 pomocí hello `openid` nastavit obor, jako je třeba:</span><span class="sxs-lookup"><span data-stu-id="d0ddb-148">Many applications send requests toohello v2.0 endpoint using hello `openid` scope, like:</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid offline_access https://outlook.office.com/mail.read
```

<span data-ttu-id="d0ddb-149">Dnes, když uživatel hello uděluje souhlas pro hello `openid` oboru, vaše aplikace obdrží velkému množství informací o uživateli hello v požadavku id_token výsledná hello.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-149">Today, when hello user grants consent for hello `openid` scope, your app receives a wealth of information about hello user in hello resulting id_token.</span></span>  <span data-ttu-id="d0ddb-150">Tyto deklarace identity můžou obsahovat jména, upřednostňované uživatelské jméno, e-mailovou adresu, ID objektu a další.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-150">These claims can include their name, preferred username, email address, object ID, and more.</span></span>

<span data-ttu-id="d0ddb-151">V této aktualizaci měníme hello informace této hello `openid` oboru umožňuje vaší aplikace přístup k toobetter comform s hello specifikace OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-151">In this update, we are changing hello information that hello `openid` scope affords your app access to, toobetter comform with hello OpenID Connect specification.</span></span>  <span data-ttu-id="d0ddb-152">Hello `openid` obor bude pouze povolit vaší aplikace toosign hello uživateli v a přijímat identifikátor specifický pro aplikace pro uživatele hello v hello `sub` deklarace identity z požadavku id_token hello.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-152">hello `openid` scope will only allow your app toosign hello user in, and receive an app-specific identifier for hello user in hello `sub` claim of hello id_token.</span></span>  <span data-ttu-id="d0ddb-153">Hello deklarací identity v požadavku id_token s pouze hello `openid` rozsah povolen bude nemá žádné identifikovatelné osobní údaje.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-153">hello claims in an id_token with only hello `openid` scope granted will be devoid of any personally identifiable information.</span></span>  <span data-ttu-id="d0ddb-154">Příklad požadavku id_token deklarace jsou:</span><span class="sxs-lookup"><span data-stu-id="d0ddb-154">Example id_token claims are:</span></span>

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

<span data-ttu-id="d0ddb-155">Pokud chcete tooobtain identifikovatelné osobní údaje (PII) o hello uživateli ve vaší aplikaci, vaše aplikace bude potřebovat další oprávnění toorequest od uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-155">If you want tooobtain personally identifiable information (PII) about hello user in your app, your app will need toorequest additional permissions from hello user.</span></span>  <span data-ttu-id="d0ddb-156">Představujeme podporu pro dva nové obory z OpenID Connect specifikace hello – hello `email` a `profile` obory – které umožňují toodo tak.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-156">We are introducing support for two new scopes from hello OpenID Connect spec – hello `email` and `profile` scopes – which allow you toodo so.</span></span>

* <span data-ttu-id="d0ddb-157">Hello `email` obor je velmi jednoduchá – umožňuje vaší aplikace přístup toohello primární e-mailovou adresu uživatele prostřednictvím hello `email` deklarací identity v požadavku id_token hello.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-157">hello `email` scope is very straightforward – it allows your app access toohello user's primary email address via hello `email` claim in hello id_token.</span></span>  <span data-ttu-id="d0ddb-158">Všimněte si, že hello `email` deklarace identity vždycky nebude přítomný v id_tokens – pouze budou zahrnuty Pokud je k dispozici v profilu uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-158">Note that hello `email` claim will not always be present in id_tokens – it will only be included if available in hello user's profile.</span></span>
* <span data-ttu-id="d0ddb-159">Hello `profile` oboru poskytuje tooall přístup k vaší aplikaci další základní informace o uživateli hello – jejich název, upřednostňované uživatelského jména, ID objektu a tak dále.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-159">hello `profile` scope affords your app access tooall other basic information about hello user – their name, preferred username, object ID, and so on.</span></span>

<span data-ttu-id="d0ddb-160">To vám umožní toocode aplikace způsobem minimální zpřístupnění – můžete požádat hello uživatele pro právě hello sadu informace, že vaše aplikace vyžaduje toodo úlohy.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-160">This allows you toocode your app in a minimal-disclosure fashion – you can ask hello user for just hello set of information that your app requires toodo its job.</span></span>  <span data-ttu-id="d0ddb-161">Pokud chcete toocontinue získávání hello kompletní informace o uživateli, který aktuálně přijímá aplikace, by měla obsahovat všechny tři obory žádostí autorizace:</span><span class="sxs-lookup"><span data-stu-id="d0ddb-161">If you want toocontinue getting hello full set of user information that your app is currently receiving, you should include all three scopes in your authorization requests:</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid profile email offline_access https://outlook.office.com/mail.read
```

<span data-ttu-id="d0ddb-162">Aplikace můžete zahájit odesílání hello `email` a `profile` obory okamžitě a koncového bodu v2.0 hello přijmout tyto dva obory a začít vyžadování oprávnění od uživatelů podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-162">Your app can begin sending hello `email` and `profile` scopes immediately, and hello v2.0 endpoint will accept these two scopes and begin requesting permissions from users as necessary.</span></span>  <span data-ttu-id="d0ddb-163">Ale hello změnit v hello výklad hello `openid` oboru se neprojeví pro několik týdnů.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-163">However, hello change in hello interpretation of hello `openid` scope will not take effect for a few weeks.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d0ddb-164">**Úlohu: přidejte hello `profile` a `email` rozsahy, pokud vaše aplikace vyžaduje informace o uživateli hello.**</span><span class="sxs-lookup"><span data-stu-id="d0ddb-164">**Your job: Add hello `profile` and `email` scopes if your app requires information about hello user.**</span></span>  <span data-ttu-id="d0ddb-165">Všimněte si, že ADAL bude obsahovat obě tato oprávnění v žádostech o ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-165">Note that ADAL will include both of these permissions in requests by default.</span></span> 
> 
> 

### <a name="removing-hello-issuer-trailing-slash"></a><span data-ttu-id="d0ddb-166">Odebrání hello vystavitele koncové lomítko.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-166">Removing hello issuer trailing slash.</span></span>
<span data-ttu-id="d0ddb-167">Dříve hello vystavitele hodnotu, která se zobrazí v tokenech z koncového bodu v2.0 hello trvalo hello formuláře</span><span class="sxs-lookup"><span data-stu-id="d0ddb-167">Previously, hello issuer value that appears in tokens from hello v2.0 endpoint took hello form</span></span>

```
https://login.microsoftonline.com/{some-guid}/v2.0/
```

<span data-ttu-id="d0ddb-168">Kde hello guid je tenantId hello klienta hello Azure AD, který vydal hello token.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-168">Where hello guid was hello tenantId of hello Azure AD tenant which issued hello token.</span></span>  <span data-ttu-id="d0ddb-169">Tyto změny se změní na hodnotu vystavitele hello</span><span class="sxs-lookup"><span data-stu-id="d0ddb-169">With these changes, hello issuer value becomes</span></span>

```
https://login.microsoftonline.com/{some-guid}/v2.0 
```

<span data-ttu-id="d0ddb-170">oba tokeny a v dokumentu zjišťování OpenID Connect hello.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-170">in both tokens and in hello OpenID Connect discovery document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d0ddb-171">**Úlohu: Zajistěte, aby vaše aplikace přijímá během ověřování vystavitele hello vystavitele hodnotu s i bez koncové lomítko.**</span><span class="sxs-lookup"><span data-stu-id="d0ddb-171">**Your job: Make sure your app accepts hello issuer value both with and without a trailing slash during issuer validation.**</span></span>
> 
> 

## <a name="why-change"></a><span data-ttu-id="d0ddb-172">Proč změnit?</span><span class="sxs-lookup"><span data-stu-id="d0ddb-172">Why change?</span></span>
<span data-ttu-id="d0ddb-173">Hello primární motivace pro představení těchto změn je kompatibilní s hello OpenID Connect standardní specifikace toobe.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-173">hello primary motivation for introducing these changes is toobe compliant with hello OpenID Connect standard specification.</span></span>  <span data-ttu-id="d0ddb-174">Díky OpenID Connect kompatibilní Doufáme toominimize rozdíly mezi integrace se službou Microsoft identity services a s jinými službami identity v odvětví hello.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-174">By being OpenID Connect compliant, we hope toominimize differences between integrating with Microsoft identity services and with other identity services in hello industry.</span></span>  <span data-ttu-id="d0ddb-175">Vývojáři toouse tooenable chceme bez nutnosti tooalter hello knihovny tooaccommodate Microsoft rozdíly své oblíbené opensourcové knihovny ověřování.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-175">We want tooenable developers toouse their favorite open source authentication libraries without having tooalter hello libraries tooaccommodate Microsoft differences.</span></span>

## <a name="what-can-you-do"></a><span data-ttu-id="d0ddb-176">Co můžete dělat?</span><span class="sxs-lookup"><span data-stu-id="d0ddb-176">What can you do?</span></span>
<span data-ttu-id="d0ddb-177">K dnešnímu dni můžete začít vytváření všechny změny hello popsané výše.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-177">As of today, you can begin making all of hello changes described above.</span></span>  <span data-ttu-id="d0ddb-178">Měli byste okamžitě:</span><span class="sxs-lookup"><span data-stu-id="d0ddb-178">You should immediately:</span></span>

1. <span data-ttu-id="d0ddb-179">**Odeberte všechny závislosti hello `x5t` parametr záhlaví.**</span><span class="sxs-lookup"><span data-stu-id="d0ddb-179">**Remove any dependencies on hello `x5t` header parameter.**</span></span>
2. <span data-ttu-id="d0ddb-180">**Pohodlné zpracování hello přechod z `profile_info` příliš`id_token` v odpovědi tokenu.**</span><span class="sxs-lookup"><span data-stu-id="d0ddb-180">**Gracefully handle hello transition from `profile_info` too`id_token` in token responses.**</span></span>
3. <span data-ttu-id="d0ddb-181">**Odeberte všechny závislosti hello `id_token_expires_in` parametr odpovědi.**</span><span class="sxs-lookup"><span data-stu-id="d0ddb-181">**Remove any dependencies on hello `id_token_expires_in` response parameter.**</span></span>
4. <span data-ttu-id="d0ddb-182">**Přidat hello `profile` a `email` obory tooyour aplikace, pokud aplikace potřebuje základní uživatelské informace.**</span><span class="sxs-lookup"><span data-stu-id="d0ddb-182">**Add hello `profile` and `email` scopes tooyour app if your app needs basic user information.**</span></span>
5. <span data-ttu-id="d0ddb-183">**Přijměte vystavitele hodnoty v tokenech s i bez koncové lomítko.**</span><span class="sxs-lookup"><span data-stu-id="d0ddb-183">**Accept issuer values in tokens both with and without a trailing slash.**</span></span>

<span data-ttu-id="d0ddb-184">Naše [dokumentace k protokolu v2.0](active-directory-v2-protocols.md) již bylo aktualizované tooreflect tyto změny, můžete ho použít jako odkaz v pomáhá aktualizace kódu.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-184">Our [v2.0 protocol documentation](active-directory-v2-protocols.md) has already been updated tooreflect these changes, so you may use it as reference in helping update your code.</span></span>

<span data-ttu-id="d0ddb-185">Pokud máte další dotazy na obor hello hello změn, můžete volné tooreach out toous na Twitteru v @AzureAD.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-185">If you have any further questions on hello scope of hello changes, please feel free tooreach out toous on Twitter at @AzureAD.</span></span>

## <a name="how-often-will-protocol-changes-occur"></a><span data-ttu-id="d0ddb-186">Jak často bude protokol změnách?</span><span class="sxs-lookup"><span data-stu-id="d0ddb-186">How often will protocol changes occur?</span></span>
<span data-ttu-id="d0ddb-187">Není jsme předvídáte, že žádné další nejnovější změny toohello ověřovací protokoly.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-187">We do not foresee any further breaking changes toohello authentication protocols.</span></span>  <span data-ttu-id="d0ddb-188">Tyto změny do jedné verze jsme jsou sdružování záměrně, takže nebudete mít toogo prostřednictvím tento typ procesu aktualizace znovu kdykoli brzy.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-188">We are intentionally bundling these changes into one release so that you won\`t have toogo through this type of update process again any time soon.</span></span>  <span data-ttu-id="d0ddb-189">Samozřejmě, bude se pokračovat tooadd funkce toohello konvergované v2.0 ověřovací služba, která můžete využít výhod, ale tyto změny by měl být doplňkové a není zalomení existující kód.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-189">Of course, we will continue tooadd features toohello converged v2.0 authentication service that you can take advantage of, but those changes should be additive and not break existing code.</span></span>

<span data-ttu-id="d0ddb-190">Nakonec rádi bychom znali toosay Děkujeme vám za vyzkoušení věcí během období preview hello.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-190">Lastly, we would like toosay thank you for trying things out during hello preview period.</span></span>  <span data-ttu-id="d0ddb-191">Statistika Hello a zkušenosti naše inovátoři být neocenitelnou pomocí doposud a Věříme, že tooshare budete pokračovat, vaše názory a návrhy.</span><span class="sxs-lookup"><span data-stu-id="d0ddb-191">hello insights and experiences of our early adopters have been invaluable thus far, and we hope you\`ll continue tooshare your opinions and ideas.</span></span>

<span data-ttu-id="d0ddb-192">Kódování radostí!</span><span class="sxs-lookup"><span data-stu-id="d0ddb-192">Happy coding!</span></span>

<span data-ttu-id="d0ddb-193">Hello Microsoft Identity dělení</span><span class="sxs-lookup"><span data-stu-id="d0ddb-193">hello Microsoft Identity Division</span></span>

