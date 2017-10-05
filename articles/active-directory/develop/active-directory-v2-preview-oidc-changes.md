---
title: "Změní na koncový bod v2.0 Azure AD | Microsoft Docs"
description: "Popis změn provedených u aplikace model v2.0 verzi public preview protokolů."
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
ms.openlocfilehash: ae73833a68db14804dc40eaf07ff7d3effaa9052
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="important-updates-to-the-v20-authentication-protocols"></a><span data-ttu-id="40d66-103">Důležité aktualizace pro ověřovací protokoly v2.0</span><span class="sxs-lookup"><span data-stu-id="40d66-103">Important Updates to the v2.0 Authentication Protocols</span></span>
<span data-ttu-id="40d66-104">Vývojáři pozornost!</span><span class="sxs-lookup"><span data-stu-id="40d66-104">Attention developers!</span></span> <span data-ttu-id="40d66-105">Za další dva týdny budeme do našich v2.0 ověřovací protokoly, které může to znamenat nejnovější změny pro všechny aplikace, který jste napsali naše období preview provádět několik aktualizací.</span><span class="sxs-lookup"><span data-stu-id="40d66-105">Over the next two weeks, we will be making a few updates to our v2.0 authentication protocols that may mean breaking changes for any apps you have written during our preview period.</span></span>  

## <a name="who-does-this-affect"></a><span data-ttu-id="40d66-106">Kdo to vliv?</span><span class="sxs-lookup"><span data-stu-id="40d66-106">Who does this affect?</span></span>
<span data-ttu-id="40d66-107">Žádné aplikace, které byly napsány pro použití v2.0 konvergované koncový bod ověřování,</span><span class="sxs-lookup"><span data-stu-id="40d66-107">Any apps that have been written to use the v2.0 converged authentication endpoint,</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
```

<span data-ttu-id="40d66-108">Další informace o koncový bod v2.0 můžete najít [zde](active-directory-appmodel-v2-overview.md).</span><span class="sxs-lookup"><span data-stu-id="40d66-108">More information on the v2.0 endpoint can be found [here](active-directory-appmodel-v2-overview.md).</span></span>

<span data-ttu-id="40d66-109">Pokud jste vytvořili aplikaci pomocí kódování přímo na protokol v2.0 koncový bod v2.0, pomocí některé z našich webových middlewares OpenID Connect nebo OAuth, nebo pomocí jiné 3. stran knihovny k provedení ověřování, by měl být přípravy pro testování vašich projektů a v případě potřeby proveďte změny.</span><span class="sxs-lookup"><span data-stu-id="40d66-109">If you have built an app using the v2.0 endpoint by coding directly to the v2.0 protocol, using any of our OpenID Connect or OAuth web middlewares, or using other 3rd party libraries to perform authentication, you should be prepared to test your projects and make changes if necessary.</span></span>

## <a name="who-doesnt-this-affect"></a><span data-ttu-id="40d66-110">Kdo nemá to vliv?</span><span class="sxs-lookup"><span data-stu-id="40d66-110">Who doesn\`t this affect?</span></span>
<span data-ttu-id="40d66-111">Všechny aplikace, která byla zapsána na koncový bod ověřování produkční Azure AD</span><span class="sxs-lookup"><span data-stu-id="40d66-111">Any apps that have been written against the production Azure AD authentication endpoint,</span></span>

```
https://login.microsoftonline.com/common/oauth2/authorize
```

<span data-ttu-id="40d66-112">Tento protokol je nastaven trvalé a nebude zaznamenat všechny změny.</span><span class="sxs-lookup"><span data-stu-id="40d66-112">This protocol is set in stone and will not be experiencing any changes.</span></span>

<span data-ttu-id="40d66-113">Kromě toho pokud vaše aplikace **pouze** používá naše knihovna ADAL pro ověřování, nebudete muset nic nezmění.</span><span class="sxs-lookup"><span data-stu-id="40d66-113">Furthermore, if your app **only** uses our ADAL library to perform authentication, you won\`t have to change anything.</span></span>  <span data-ttu-id="40d66-114">ADAL má Stíněný aplikaci z změny.</span><span class="sxs-lookup"><span data-stu-id="40d66-114">ADAL has shielded your app from the changes.</span></span>  

## <a name="what-are-the-changes"></a><span data-ttu-id="40d66-115">Jaké jsou změny?</span><span class="sxs-lookup"><span data-stu-id="40d66-115">What are the changes?</span></span>
### <a name="removing-the-x5t-value-from-jwt-headers"></a><span data-ttu-id="40d66-116">Odebráním x5t hodnota hlavičky JWT</span><span class="sxs-lookup"><span data-stu-id="40d66-116">Removing the x5t value from JWT headers</span></span>
<span data-ttu-id="40d66-117">Koncový bod v2.0 používá tokeny JWT hojně, které obsahují záhlaví parametry s relevantní metadata o token.</span><span class="sxs-lookup"><span data-stu-id="40d66-117">The v2.0 endpoint uses JWT tokens extensively, which contain a header parameters section with relevant metadata about the token.</span></span>  <span data-ttu-id="40d66-118">Pokud jste dekódovat záhlaví jednoho z našich aktuální tokeny Jwt, by se najít něco podobného jako:</span><span class="sxs-lookup"><span data-stu-id="40d66-118">If you decode the header of one of our current JWTs, you would find something like:</span></span>

```
{ 
    "type": "JWT",
    "alg": "RS256",
    "x5t": "MnC_VZcATfM5pOYiJHMba9goEKY",
    "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

<span data-ttu-id="40d66-119">Kde "x5t" i "dětský" vlastnosti identifikovat veřejný klíč, který se má použít k ověření podpisu tokenu, která byla načtena z koncový bod metadat OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="40d66-119">Where both the "x5t" and "kid" properties identify the public key that should be used to validate the token\`s signature, as retrieved from the OpenID Connect metadata endpoint.</span></span>

<span data-ttu-id="40d66-120">Změna, kterou tady vydáváme je odeberte vlastnost "x5t".</span><span class="sxs-lookup"><span data-stu-id="40d66-120">The change we are making here is to remove the "x5t" property.</span></span>  <span data-ttu-id="40d66-121">Mohl nadále používat stejné mechanismy k ověření tokenů, ale spoléhat na vlastnost "dětský" načíst správný veřejný klíč, jak je uvedeno v protokolu OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="40d66-121">You may continue to use the same mechanisms to validate tokens, but should rely only on the "kid" property to retrieve the correct public key, as specified in the OpenID Connect protocol.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="40d66-122">**Úlohu: Zajistěte, aby vaše aplikace není závislá na existenci x5t hodnota.**</span><span class="sxs-lookup"><span data-stu-id="40d66-122">**Your job: Make sure your app does not depend on the existence of the x5t value.**</span></span>
> 
> 

### <a name="removing-profileinfo"></a><span data-ttu-id="40d66-123">Odebrání profile_info</span><span class="sxs-lookup"><span data-stu-id="40d66-123">Removing profile_info</span></span>
<span data-ttu-id="40d66-124">Dříve, koncový bod v2.0 má byla vrací objekt JSON kódováním base64 v tokenu odpovědí názvem `profile_info`.</span><span class="sxs-lookup"><span data-stu-id="40d66-124">Previously, the v2.0 endpoint has been returning a base64 encoded JSON object in token responses called `profile_info`.</span></span>  <span data-ttu-id="40d66-125">Žádosti o token přístupu z koncového bodu v2.0 odesláním požadavku na:</span><span class="sxs-lookup"><span data-stu-id="40d66-125">When requesting an access token from the v2.0 endpoint by sending a request to:</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

<span data-ttu-id="40d66-126">Odpověď bude vypadat následující objekt JSON:</span><span class="sxs-lookup"><span data-stu-id="40d66-126">The response would look like the following JSON object:</span></span>

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

<span data-ttu-id="40d66-127">`profile_info` Hodnota obsažená informace o uživateli, který podepsal do aplikace - jejich název zobrazení, křestní jméno, příjmení, e-mailovou adresu, identifikátor a tak dále.</span><span class="sxs-lookup"><span data-stu-id="40d66-127">The `profile_info` value contained information about the user who signed into the app - their display name, first name, surname, email address, identifier, and so on.</span></span>  <span data-ttu-id="40d66-128">Především se stává `profile_info` byl použit pro ukládání tokenu do mezipaměti a zobrazit účely.</span><span class="sxs-lookup"><span data-stu-id="40d66-128">Primarily, the `profile_info` was used for token caching and display purposes.</span></span>

<span data-ttu-id="40d66-129">Jsou nyní odebrání `profile_info` hodnotu – ale nemusíte si dělat starosti, stále umožňujeme tyto informace pro vývojáře na místě, mírně lišit.</span><span class="sxs-lookup"><span data-stu-id="40d66-129">We are now removing the `profile_info` value – but don't worry, we are still providing this information to developers in a slightly different place.</span></span>  <span data-ttu-id="40d66-130">Místo `profile_info`, koncový bod v2.0 bude nyní se vraťte `id_token` v každé odpovědi tokenu:</span><span class="sxs-lookup"><span data-stu-id="40d66-130">Instead of `profile_info`, the v2.0 endpoint will now return an `id_token` in each token response:</span></span>

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

<span data-ttu-id="40d66-131">Může dekódovat a analyzovat požadavku id_token načíst stejné informace, které jste dostali od profile_info.</span><span class="sxs-lookup"><span data-stu-id="40d66-131">You may decode and parse the id_token to retrieve the same information that you received from profile_info.</span></span>  <span data-ttu-id="40d66-132">Požadavku id_token je JSON Web Token (JWT), s obsahem podle specifikace OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="40d66-132">The id_token is a JSON Web Token (JWT), with contents as specified by OpenID Connect.</span></span>  <span data-ttu-id="40d66-133">Kód pro tuto činnost proto by mělo být velmi podobné – jednoduše je potřeba extrahovat střední segment (text) požadavku id_token a base64 dekódování ho pro přístup k objektu JSON v rámci.</span><span class="sxs-lookup"><span data-stu-id="40d66-133">The code for doing so should be very similar – you simply need to extract the middle segment (the body) of the id_token and base64 decode it to access the JSON object within.</span></span>

<span data-ttu-id="40d66-134">Za další dva týdny, by měl kód aplikaci načíst informace o uživateli z buď `id_token` nebo `profile_info`; podle toho, co je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="40d66-134">Over the next two weeks, you should code your app to retrieve the user information from either the `id_token` or `profile_info`; whichever is present.</span></span>  <span data-ttu-id="40d66-135">Tímto způsobem při provádění změn, vaše aplikace může bezproblémově zpracovat přechod z `profile_info` k `id_token` bez přerušení.</span><span class="sxs-lookup"><span data-stu-id="40d66-135">That way when the change is made, your app can seamlessly handle the transition from `profile_info` to `id_token` without interruption.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="40d66-136">**Úlohu: Zajistěte, aby vaše aplikace není závislá na existenci `profile_info` hodnotu.**</span><span class="sxs-lookup"><span data-stu-id="40d66-136">**Your job: Make sure your app does not depend on the existence of the `profile_info` value.**</span></span>
> 
> 

### <a name="removing-idtokenexpiresin"></a><span data-ttu-id="40d66-137">Odebrání id_token_expires_in</span><span class="sxs-lookup"><span data-stu-id="40d66-137">Removing id_token_expires_in</span></span>
<span data-ttu-id="40d66-138">Podobně jako `profile_info`, jsou také odebrání `id_token_expires_in` parametr z odpovědi.</span><span class="sxs-lookup"><span data-stu-id="40d66-138">Similar to `profile_info`, we are also removing the `id_token_expires_in` parameter from responses.</span></span>  <span data-ttu-id="40d66-139">Dřív by koncového bodu v2.0 vrátit hodnotu `id_token_expires_in` společně s každou požadavku id_token odpověď, například v odpovědi autorizovat:</span><span class="sxs-lookup"><span data-stu-id="40d66-139">Previously, the v2.0 endpoint would return a value for `id_token_expires_in` along with each id_token response, for instance in an authorize response:</span></span>

```
https://myapp.com?id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...&id_token_expires_in=3599...
```

<span data-ttu-id="40d66-140">Nebo v odpovědi tokenu:</span><span class="sxs-lookup"><span data-stu-id="40d66-140">Or in a token response:</span></span>

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

<span data-ttu-id="40d66-141">`id_token_expires_in` Hodnotu by určit počet sekund požadavku id_token by zůstat platné pro.</span><span class="sxs-lookup"><span data-stu-id="40d66-141">The `id_token_expires_in` value would indicate the number of seconds the id_token would remain valid for.</span></span>  <span data-ttu-id="40d66-142">Nyní jsme se odebrat `id_token_expires_in` hodnotu úplně.</span><span class="sxs-lookup"><span data-stu-id="40d66-142">Now, we are removing the `id_token_expires_in` value completely.</span></span>  <span data-ttu-id="40d66-143">Místo toho můžete použít standardní OpenID Connect `nbf` a `exp` Zkontrolujte platnost požadavku id_token deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="40d66-143">Instead, you may use the OpenID Connect standard `nbf` and `exp` claims to examine the validity of an id_token.</span></span>  <span data-ttu-id="40d66-144">Najdete v článku [odkaz tokenu v2.0](active-directory-v2-tokens.md) Další informace o těchto deklaracích identity.</span><span class="sxs-lookup"><span data-stu-id="40d66-144">See the [v2.0 token reference](active-directory-v2-tokens.md) for more information on these claims.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="40d66-145">**Úlohu: Zajistěte, aby vaše aplikace není závislá na existenci `id_token_expires_in` hodnotu.**</span><span class="sxs-lookup"><span data-stu-id="40d66-145">**Your job: Make sure your app does not depend on the existence of the `id_token_expires_in` value.**</span></span>
> 
> 

### <a name="changing-the-claims-returned-by-scopeopenid"></a><span data-ttu-id="40d66-146">Změna deklarace identity vrátí oborem = openid</span><span class="sxs-lookup"><span data-stu-id="40d66-146">Changing the claims returned by scope=openid</span></span>
<span data-ttu-id="40d66-147">Tato změna bude nejvýznamnější – ve skutečnosti, bude to mít vliv téměř každé aplikaci, která používá koncový bod v2.0.</span><span class="sxs-lookup"><span data-stu-id="40d66-147">This change will be the most significant – in fact, it will affect almost every app that uses the v2.0 endpoint.</span></span>  <span data-ttu-id="40d66-148">Mnoho aplikací pomocí koncového bodu v2.0 odesílat požadavky `openid` nastavit obor, jako je třeba:</span><span class="sxs-lookup"><span data-stu-id="40d66-148">Many applications send requests to the v2.0 endpoint using the `openid` scope, like:</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid offline_access https://outlook.office.com/mail.read
```

<span data-ttu-id="40d66-149">Dnes, když uživatel uděluje souhlas pro `openid` oboru, vaše aplikace obdrží velkému množství informací o uživateli v výsledné požadavku id_token.</span><span class="sxs-lookup"><span data-stu-id="40d66-149">Today, when the user grants consent for the `openid` scope, your app receives a wealth of information about the user in the resulting id_token.</span></span>  <span data-ttu-id="40d66-150">Tyto deklarace identity můžou obsahovat jména, upřednostňované uživatelské jméno, e-mailovou adresu, ID objektu a další.</span><span class="sxs-lookup"><span data-stu-id="40d66-150">These claims can include their name, preferred username, email address, object ID, and more.</span></span>

<span data-ttu-id="40d66-151">V této aktualizaci měníme informace, `openid` oboru poskytuje přístup vaší aplikace, k lepší comform specifikace OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="40d66-151">In this update, we are changing the information that the `openid` scope affords your app access to, to better comform with the OpenID Connect specification.</span></span>  <span data-ttu-id="40d66-152">`openid` Obor bude pouze povolit přihlášení uživatele ve vaší aplikaci a přijímat identifikátor specifický pro aplikace pro uživatele v `sub` deklarace identity služby požadavku id_token.</span><span class="sxs-lookup"><span data-stu-id="40d66-152">The `openid` scope will only allow your app to sign the user in, and receive an app-specific identifier for the user in the `sub` claim of the id_token.</span></span>  <span data-ttu-id="40d66-153">Deklarací identity ve požadavku id_token se pouze `openid` rozsah povolen bude nemá žádné identifikovatelné osobní údaje.</span><span class="sxs-lookup"><span data-stu-id="40d66-153">The claims in an id_token with only the `openid` scope granted will be devoid of any personally identifiable information.</span></span>  <span data-ttu-id="40d66-154">Příklad požadavku id_token deklarace jsou:</span><span class="sxs-lookup"><span data-stu-id="40d66-154">Example id_token claims are:</span></span>

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

<span data-ttu-id="40d66-155">Pokud chcete získat identifikovatelné osobní údaje (PII) o uživateli ve vaší aplikaci, vaše aplikace bude muset požádat o další oprávnění od uživatele.</span><span class="sxs-lookup"><span data-stu-id="40d66-155">If you want to obtain personally identifiable information (PII) about the user in your app, your app will need to request additional permissions from the user.</span></span>  <span data-ttu-id="40d66-156">Představujeme z specifikace OpenID Connect – podpora pro dva nové obory `email` a `profile` obory – které umožňují učinit.</span><span class="sxs-lookup"><span data-stu-id="40d66-156">We are introducing support for two new scopes from the OpenID Connect spec – the `email` and `profile` scopes – which allow you to do so.</span></span>

* <span data-ttu-id="40d66-157">`email` Obor je velmi jednoduchá – umožňuje vám přístup aplikace k primární e-mailovou adresu uživatele prostřednictvím `email` deklarací identity v požadavku id_token.</span><span class="sxs-lookup"><span data-stu-id="40d66-157">The `email` scope is very straightforward – it allows your app access to the user's primary email address via the `email` claim in the id_token.</span></span>  <span data-ttu-id="40d66-158">Všimněte si, že `email` deklarace identity vždycky nebude přítomný v id_tokens – pouze budou zahrnuty Pokud je k dispozici v profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="40d66-158">Note that the `email` claim will not always be present in id_tokens – it will only be included if available in the user's profile.</span></span>
* <span data-ttu-id="40d66-159">`profile` Oboru poskytuje přístup k vaší aplikaci pro všechny ostatní základní informace o uživateli – jejich název, upřednostňované uživatelské jméno, ID objektu a tak dále.</span><span class="sxs-lookup"><span data-stu-id="40d66-159">The `profile` scope affords your app access to all other basic information about the user – their name, preferred username, object ID, and so on.</span></span>

<span data-ttu-id="40d66-160">To umožňuje kód aplikace způsobem minimální zpřístupnění – uživatel může požádat o právě sadu informace, které vaše aplikace vyžaduje, aby svou úlohu.</span><span class="sxs-lookup"><span data-stu-id="40d66-160">This allows you to code your app in a minimal-disclosure fashion – you can ask the user for just the set of information that your app requires to do its job.</span></span>  <span data-ttu-id="40d66-161">Pokud chcete pokračovat, získávání kompletní informace o uživateli, který aktuálně přijímá aplikace, by měla obsahovat všechny tři obory žádostí autorizace:</span><span class="sxs-lookup"><span data-stu-id="40d66-161">If you want to continue getting the full set of user information that your app is currently receiving, you should include all three scopes in your authorization requests:</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid profile email offline_access https://outlook.office.com/mail.read
```

<span data-ttu-id="40d66-162">Aplikace můžete zahájit odesílání `email` a `profile` obory okamžitě a koncového bodu v2.0 přijmout tyto dva obory a začít vyžadování oprávnění od uživatelů podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="40d66-162">Your app can begin sending the `email` and `profile` scopes immediately, and the v2.0 endpoint will accept these two scopes and begin requesting permissions from users as necessary.</span></span>  <span data-ttu-id="40d66-163">Však změnu v hodnotě výklad `openid` oboru se neprojeví pro několik týdnů.</span><span class="sxs-lookup"><span data-stu-id="40d66-163">However, the change in the interpretation of the `openid` scope will not take effect for a few weeks.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="40d66-164">**Úlohu: Přidat `profile` a `email` rozsahy, pokud vaše aplikace vyžaduje informace o uživateli.**</span><span class="sxs-lookup"><span data-stu-id="40d66-164">**Your job: Add the `profile` and `email` scopes if your app requires information about the user.**</span></span>  <span data-ttu-id="40d66-165">Všimněte si, že ADAL bude obsahovat obě tato oprávnění v žádostech o ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="40d66-165">Note that ADAL will include both of these permissions in requests by default.</span></span> 
> 
> 

### <a name="removing-the-issuer-trailing-slash"></a><span data-ttu-id="40d66-166">Odebrání vystavitele koncové lomítko.</span><span class="sxs-lookup"><span data-stu-id="40d66-166">Removing the issuer trailing slash.</span></span>
<span data-ttu-id="40d66-167">Dříve vystavitele hodnotu, která se zobrazí v tokeny z koncového bodu v2.0 trvalo formuláře</span><span class="sxs-lookup"><span data-stu-id="40d66-167">Previously, the issuer value that appears in tokens from the v2.0 endpoint took the form</span></span>

```
https://login.microsoftonline.com/{some-guid}/v2.0/
```

<span data-ttu-id="40d66-168">Kde tenantId služby Azure AD byl identifikátor guid klienta, který vydal token.</span><span class="sxs-lookup"><span data-stu-id="40d66-168">Where the guid was the tenantId of the Azure AD tenant which issued the token.</span></span>  <span data-ttu-id="40d66-169">Tyto změny se změní na hodnotu vystavitele</span><span class="sxs-lookup"><span data-stu-id="40d66-169">With these changes, the issuer value becomes</span></span>

```
https://login.microsoftonline.com/{some-guid}/v2.0 
```

<span data-ttu-id="40d66-170">v obou tokeny a zjišťování dokumentu OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="40d66-170">in both tokens and in the OpenID Connect discovery document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="40d66-171">**Úlohu: Zajistěte, aby vaše aplikace přijímá vystavitele hodnotu s i bez koncové lomítko během ověřování vystavitele.**</span><span class="sxs-lookup"><span data-stu-id="40d66-171">**Your job: Make sure your app accepts the issuer value both with and without a trailing slash during issuer validation.**</span></span>
> 
> 

## <a name="why-change"></a><span data-ttu-id="40d66-172">Proč změnit?</span><span class="sxs-lookup"><span data-stu-id="40d66-172">Why change?</span></span>
<span data-ttu-id="40d66-173">Primární motivace pro představení tyto změny se má být kompatibilní se specifikací standardní OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="40d66-173">The primary motivation for introducing these changes is to be compliant with the OpenID Connect standard specification.</span></span>  <span data-ttu-id="40d66-174">Díky OpenID Connect kompatibilní Doufáme, minimalizovat rozdíly mezi integrace se službou Microsoft identity services a s jinými službami identity v odvětví.</span><span class="sxs-lookup"><span data-stu-id="40d66-174">By being OpenID Connect compliant, we hope to minimize differences between integrating with Microsoft identity services and with other identity services in the industry.</span></span>  <span data-ttu-id="40d66-175">Chceme umožňují vývojářům používat své oblíbené opensourcové knihovny ověřování bez nutnosti ke změně v knihovnách k přizpůsobení rozdílů společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="40d66-175">We want to enable developers to use their favorite open source authentication libraries without having to alter the libraries to accommodate Microsoft differences.</span></span>

## <a name="what-can-you-do"></a><span data-ttu-id="40d66-176">Co můžete dělat?</span><span class="sxs-lookup"><span data-stu-id="40d66-176">What can you do?</span></span>
<span data-ttu-id="40d66-177">K dnešnímu dni můžete začít vytváření všechny změny popsané výše.</span><span class="sxs-lookup"><span data-stu-id="40d66-177">As of today, you can begin making all of the changes described above.</span></span>  <span data-ttu-id="40d66-178">Měli byste okamžitě:</span><span class="sxs-lookup"><span data-stu-id="40d66-178">You should immediately:</span></span>

1. <span data-ttu-id="40d66-179">**Odeberte všechny závislosti pro `x5t` parametr záhlaví.**</span><span class="sxs-lookup"><span data-stu-id="40d66-179">**Remove any dependencies on the `x5t` header parameter.**</span></span>
2. <span data-ttu-id="40d66-180">**Pohodlné zpracování přechod z `profile_info` k `id_token` v odpovědi tokenu.**</span><span class="sxs-lookup"><span data-stu-id="40d66-180">**Gracefully handle the transition from `profile_info` to `id_token` in token responses.**</span></span>
3. <span data-ttu-id="40d66-181">**Odeberte všechny závislosti pro `id_token_expires_in` parametr odpovědi.**</span><span class="sxs-lookup"><span data-stu-id="40d66-181">**Remove any dependencies on the `id_token_expires_in` response parameter.**</span></span>
4. <span data-ttu-id="40d66-182">**Přidat `profile` a `email` rozsahy do vaší aplikace, pokud aplikace potřebuje základní uživatelské informace.**</span><span class="sxs-lookup"><span data-stu-id="40d66-182">**Add the `profile` and `email` scopes to your app if your app needs basic user information.**</span></span>
5. <span data-ttu-id="40d66-183">**Přijměte vystavitele hodnoty v tokenech s i bez koncové lomítko.**</span><span class="sxs-lookup"><span data-stu-id="40d66-183">**Accept issuer values in tokens both with and without a trailing slash.**</span></span>

<span data-ttu-id="40d66-184">Naše [dokumentace k protokolu v2.0](active-directory-v2-protocols.md) se už aktualizovala tak, aby odrážela tyto změny, můžete ho použít jako odkaz v pomáhá aktualizace kódu.</span><span class="sxs-lookup"><span data-stu-id="40d66-184">Our [v2.0 protocol documentation](active-directory-v2-protocols.md) has already been updated to reflect these changes, so you may use it as reference in helping update your code.</span></span>

<span data-ttu-id="40d66-185">Pokud máte další dotazy na obor změny, kontaktujte oslovení nám na Twitteru v @AzureAD.</span><span class="sxs-lookup"><span data-stu-id="40d66-185">If you have any further questions on the scope of the changes, please feel free to reach out to us on Twitter at @AzureAD.</span></span>

## <a name="how-often-will-protocol-changes-occur"></a><span data-ttu-id="40d66-186">Jak často bude protokol změnách?</span><span class="sxs-lookup"><span data-stu-id="40d66-186">How often will protocol changes occur?</span></span>
<span data-ttu-id="40d66-187">Není jsme předvídáte, že žádné další nejnovější změny ověřovací protokoly.</span><span class="sxs-lookup"><span data-stu-id="40d66-187">We do not foresee any further breaking changes to the authentication protocols.</span></span>  <span data-ttu-id="40d66-188">Tyto změny do jedné verze jsme jsou sdružování záměrně, takže nebudete muset projít tento typ procesu aktualizace znovu kdykoli brzy.</span><span class="sxs-lookup"><span data-stu-id="40d66-188">We are intentionally bundling these changes into one release so that you won\`t have to go through this type of update process again any time soon.</span></span>  <span data-ttu-id="40d66-189">Samozřejmě budeme nadále přidání funkcí do sblíženého v2.0 ověřovací služba, která můžete využít výhod, ale tyto změny by měl být doplňkové a není zalomení existující kód.</span><span class="sxs-lookup"><span data-stu-id="40d66-189">Of course, we will continue to add features to the converged v2.0 authentication service that you can take advantage of, but those changes should be additive and not break existing code.</span></span>

<span data-ttu-id="40d66-190">Nakonec bychom rádi vyslovení Děkujeme vám za vyzkoušení věcí během období preview.</span><span class="sxs-lookup"><span data-stu-id="40d66-190">Lastly, we would like to say thank you for trying things out during the preview period.</span></span>  <span data-ttu-id="40d66-191">Přehledy a zkušenosti naše inovátoři být neocenitelnou pomocí doposud a Věříme, že budete nadále sdílet své názory a návrhy.</span><span class="sxs-lookup"><span data-stu-id="40d66-191">The insights and experiences of our early adopters have been invaluable thus far, and we hope you\`ll continue to share your opinions and ideas.</span></span>

<span data-ttu-id="40d66-192">Kódování radostí!</span><span class="sxs-lookup"><span data-stu-id="40d66-192">Happy coding!</span></span>

<span data-ttu-id="40d66-193">Microsoft Identity dělení</span><span class="sxs-lookup"><span data-stu-id="40d66-193">The Microsoft Identity Division</span></span>

