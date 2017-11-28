---
title: "Azure Active Directory B2C: Získávání tokenu pomocí aplikace pro Android | Microsoft Docs"
description: "Tento článek vám ukáže, jak toocreate Android aplikaci, která používá AppAuth s Azure Active Directory B2C toomanage uživatelských identit a ověření uživatelů."
services: active-directory-b2c
documentationcenter: android
author: parakhj
manager: krassk
editor: 
ms.assetid: d00947c3-dcaa-4cb3-8c2e-d94e0746d8b2
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 03/06/2017
ms.author: parakhj
ms.openlocfilehash: 0236398673115a34951f035cb1e73e89417abf86
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-sign-in-using-an-android-application"></a><span data-ttu-id="07ad2-103">Azure AD B2C: Přihlaste se pomocí aplikace pro Android</span><span class="sxs-lookup"><span data-stu-id="07ad2-103">Azure AD B2C: Sign-in using an Android application</span></span>

<span data-ttu-id="07ad2-104">Platforma identity Microsoft Hello používá otevřete standardy, jako je OAuth2 a OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="07ad2-104">hello Microsoft identity platform uses open standards such as OAuth2 and OpenID Connect.</span></span> <span data-ttu-id="07ad2-105">To umožňuje vývojáři tooleverage žádnou knihovnu si přejí toointegrate s našich služeb.</span><span class="sxs-lookup"><span data-stu-id="07ad2-105">This allows developers tooleverage any library they wish toointegrate with our services.</span></span> <span data-ttu-id="07ad2-106">Vývojáři tooaid pomocí naše platforma s další knihovny, jsme jste zapsána několik návody jako tento jeden toodemonstrate jak tooconfigure 3. stran knihovny tooconnect toohello identity platformu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="07ad2-106">tooaid developers in using our platform with other libraries, we've written a few walkthroughs like this one toodemonstrate how tooconfigure 3rd party libraries tooconnect toohello Microsoft identity platform.</span></span> <span data-ttu-id="07ad2-107">Většina knihovny, které implementují [specifikace hello RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) bude mít tooconnect toohello Microsoft Identity platformy.</span><span class="sxs-lookup"><span data-stu-id="07ad2-107">Most libraries that implement [hello RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) will be able tooconnect toohello Microsoft Identity platform.</span></span>

> [!WARNING]
> <span data-ttu-id="07ad2-108">Společnost Microsoft neposkytuje opravy pro 3. stran knihovny a nebyl provádí kontrolu těchto knihoven.</span><span class="sxs-lookup"><span data-stu-id="07ad2-108">Microsoft does not provide fixes for 3rd party libraries and has not done a review of those libraries.</span></span> <span data-ttu-id="07ad2-109">Tato ukázka používá knihovnu 3. stran volat AppAuth, který byl testován pro kompatibilitu v základní scénáře s hello Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="07ad2-109">This sample is using a 3rd party library called AppAuth that has been tested for compatibility in basic scenarios with hello Azure AD B2C.</span></span> <span data-ttu-id="07ad2-110">Problémy a žádosti o funkce by měla být směrovanou toohello knihovny open-source projekt.</span><span class="sxs-lookup"><span data-stu-id="07ad2-110">Issues and feature requests should be directed toohello library's open-source project.</span></span> <span data-ttu-id="07ad2-111">Najdete v tématu [v tomto článku](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) Další informace.</span><span class="sxs-lookup"><span data-stu-id="07ad2-111">Please see [this article](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) for more information.</span></span>  
>
>

<span data-ttu-id="07ad2-112">Pokud jste nový tooOAuth2 nebo OpenID Connect mnohem této ukázkové konfigurace nemusí mít mnoho tooyou smysl.</span><span class="sxs-lookup"><span data-stu-id="07ad2-112">If you're new tooOAuth2 or OpenID Connect much of this sample configuration may not make much sense tooyou.</span></span> <span data-ttu-id="07ad2-113">Doporučujeme, abyste si prohlédnete brief [přehled protokolu hello jsme jsme tady popisujeme](active-directory-b2c-reference-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="07ad2-113">We recommend you look at a brief [overview of hello protocol we've documented here](active-directory-b2c-reference-protocols.md).</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="07ad2-114">Získání adresáře služby Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="07ad2-114">Get an Azure AD B2C directory</span></span>

<span data-ttu-id="07ad2-115">Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="07ad2-115">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span> <span data-ttu-id="07ad2-116">Adresář je kontejner pro všechny vaše uživatele, aplikace, skupiny a další.</span><span class="sxs-lookup"><span data-stu-id="07ad2-116">A directory is a container for all of your users, apps, groups, and more.</span></span> <span data-ttu-id="07ad2-117">Pokud ho ještě nemáte, [vytvořte adresář B2C](active-directory-b2c-get-started.md) předtím, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="07ad2-117">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="07ad2-118">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="07ad2-118">Create an application</span></span>

<span data-ttu-id="07ad2-119">Dále musíte toocreate aplikace ve svém adresáři B2C.</span><span class="sxs-lookup"><span data-stu-id="07ad2-119">Next, you need toocreate an app in your B2C directory.</span></span> <span data-ttu-id="07ad2-120">Díky tomu získá informace o Azure AD, je nutné toocommunicate bezpečně s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="07ad2-120">This gives Azure AD information that it needs toocommunicate securely with your app.</span></span> <span data-ttu-id="07ad2-121">toocreate mobilní aplikace, postupujte podle [tyto pokyny](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="07ad2-121">toocreate a mobile app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="07ad2-122">Ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="07ad2-122">Be sure to:</span></span>

* <span data-ttu-id="07ad2-123">Zahrnout **nativního klienta** v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="07ad2-123">Include a **Native Client** in hello application.</span></span>
* <span data-ttu-id="07ad2-124">Kopírování hello **ID aplikace** který je přiřazený tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="07ad2-124">Copy hello **Application ID** that is assigned tooyour app.</span></span> <span data-ttu-id="07ad2-125">Budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="07ad2-125">You will need this later.</span></span>
* <span data-ttu-id="07ad2-126">Nastavit nativního klienta **identifikátor URI pro přesměrování** (např. com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect).</span><span class="sxs-lookup"><span data-stu-id="07ad2-126">Set up a native client **Redirect URI** (e.g. com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect).</span></span> <span data-ttu-id="07ad2-127">To také budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="07ad2-127">You will also need this later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="07ad2-128">Vytvořte svoje zásady</span><span class="sxs-lookup"><span data-stu-id="07ad2-128">Create your policies</span></span>

<span data-ttu-id="07ad2-129">V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="07ad2-129">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="07ad2-130">Tato aplikace obsahuje jeden prostředí identit: například společné přihlášení a odhlášení.</span><span class="sxs-lookup"><span data-stu-id="07ad2-130">This app contains one identity experience: a combined sign-in and sign-up.</span></span> <span data-ttu-id="07ad2-131">Je třeba toocreate tuto zásadu, jak je popsáno v [článku o zásadách](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="07ad2-131">You need toocreate this policy, as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span> <span data-ttu-id="07ad2-132">Když vytvoříte hello zásady, nezapomeňte:</span><span class="sxs-lookup"><span data-stu-id="07ad2-132">When you create hello policy, be sure to:</span></span>

* <span data-ttu-id="07ad2-133">Zvolte hello **zobrazovaný název** jako atribut registrace ve svojí zásadě.</span><span class="sxs-lookup"><span data-stu-id="07ad2-133">Choose hello **Display name** as a sign-up attribute in your policy.</span></span>
* <span data-ttu-id="07ad2-134">Zvolte hello **zobrazovaný název** a **ID objektu** deklarace identity aplikace v každé zásadě.</span><span class="sxs-lookup"><span data-stu-id="07ad2-134">Choose hello **Display name** and **Object ID** application claims in every policy.</span></span> <span data-ttu-id="07ad2-135">Můžete zvolit i další deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="07ad2-135">You can choose other claims as well.</span></span>
* <span data-ttu-id="07ad2-136">Kopírování hello **název** po jejím vytvoření každé zásady.</span><span class="sxs-lookup"><span data-stu-id="07ad2-136">Copy hello **Name** of each policy after you create it.</span></span> <span data-ttu-id="07ad2-137">Měl by mít předponu hello `b2c_1_`.</span><span class="sxs-lookup"><span data-stu-id="07ad2-137">It should have hello prefix `b2c_1_`.</span></span>  <span data-ttu-id="07ad2-138">Název zásady hello budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="07ad2-138">You'll need hello policy name later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="07ad2-139">Po vytvoření zásad jste připravené toobuild vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="07ad2-139">After you have created your policies, you're ready toobuild your app.</span></span>

## <a name="download-hello-sample-code"></a><span data-ttu-id="07ad2-140">Stáhněte si ukázkový kód hello</span><span class="sxs-lookup"><span data-stu-id="07ad2-140">Download hello sample code</span></span>

<span data-ttu-id="07ad2-141">Uvádíme pracovní vzorku, který používá AppAuth s Azure AD B2C [na Githubu](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c).</span><span class="sxs-lookup"><span data-stu-id="07ad2-141">We have provided a working sample that uses AppAuth with Azure AD B2C [on GitHub](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c).</span></span> <span data-ttu-id="07ad2-142">Můžete stáhnout hello kód a spusťte ho.</span><span class="sxs-lookup"><span data-stu-id="07ad2-142">You can download hello code and run it.</span></span> <span data-ttu-id="07ad2-143">Můžete rychle začít pracovat s vlastní aplikace pomocí Azure AD B2C konfiguraci podle pokynů hello v hello [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="07ad2-143">You can quickly get started with your own app using your own Azure AD B2C configuration by following hello instructions in hello [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md).</span></span>

<span data-ttu-id="07ad2-144">Ukázka Hello je změna hello ukázky poskytované [AppAuth](https://openid.github.io/AppAuth-Android/).</span><span class="sxs-lookup"><span data-stu-id="07ad2-144">hello sample is a modification of hello sample provided by [AppAuth](https://openid.github.io/AppAuth-Android/).</span></span> <span data-ttu-id="07ad2-145">Navštivte toolearn jejich stránky, další informace o AppAuth a jejich funkce.</span><span class="sxs-lookup"><span data-stu-id="07ad2-145">Please visit their page toolearn more about AppAuth and its features.</span></span>

## <a name="modifying-your-app-toouse-azure-ad-b2c-with-appauth"></a><span data-ttu-id="07ad2-146">Úprava aplikace toouse Azure AD B2C s AppAuth</span><span class="sxs-lookup"><span data-stu-id="07ad2-146">Modifying your app toouse Azure AD B2C with AppAuth</span></span>

> [!NOTE]
> <span data-ttu-id="07ad2-147">AppAuth podporuje Android API 16 (Jellybean) a vyšší.</span><span class="sxs-lookup"><span data-stu-id="07ad2-147">AppAuth supports Android API 16 (Jellybean) and above.</span></span> <span data-ttu-id="07ad2-148">Doporučujeme používat rozhraní API 23 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="07ad2-148">We recommend using API 23 and above.</span></span>
>

### <a name="configuration"></a><span data-ttu-id="07ad2-149">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="07ad2-149">Configuration</span></span>

<span data-ttu-id="07ad2-150">Komunikaci s Azure AD B2C můžete nakonfigurovat buď zadání hello zjišťování URI nebo zadáním koncový bod autorizace hello a koncový bod tokenu identifikátory URI.</span><span class="sxs-lookup"><span data-stu-id="07ad2-150">You can configure communication with Azure AD B2C by either specifying hello discovery URI or by specifying both hello authorization endpoint and token endpoint URIs.</span></span> <span data-ttu-id="07ad2-151">V obou případech budete potřebovat hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="07ad2-151">In either case, you will need hello following information:</span></span>

* <span data-ttu-id="07ad2-152">ID klienta (např. contoso.onmicrosoft.com)</span><span class="sxs-lookup"><span data-stu-id="07ad2-152">Tenant ID (e.g. contoso.onmicrosoft.com)</span></span>
* <span data-ttu-id="07ad2-153">Název zásady (například B2C\_1\_SignUpIn)</span><span class="sxs-lookup"><span data-stu-id="07ad2-153">Policy name (e.g. B2C\_1\_SignUpIn)</span></span>

<span data-ttu-id="07ad2-154">Pokud zvolíte tooautomatically zjistit hello autorizace a token koncový bod identifikátory URI, budete potřebovat toofetch informace ze zjišťování hello identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="07ad2-154">If you choose tooautomatically discover hello authorization and token endpoint URIs, you will need toofetch information from hello discovery URI.</span></span> <span data-ttu-id="07ad2-155">identifikátor URI může být generována nahrazení hello klienta zjišťování Hello\_ID a hello zásad\_název v hello následující adresu URL:</span><span class="sxs-lookup"><span data-stu-id="07ad2-155">hello discovery URI can be generated by replacing hello Tenant\_ID and hello Policy\_Name in hello following URL:</span></span>

```java
String mDiscoveryURI = "https://login.microsoftonline.com/<Tenant_ID>/v2.0/.well-known/openid-configuration?p=<Policy_Name>";
```

<span data-ttu-id="07ad2-156">Potom můžete získat hello autorizace a koncový bod tokenu identifikátory URI a vytvořit objekt AuthorizationServiceConfiguration spuštěním hello následující:</span><span class="sxs-lookup"><span data-stu-id="07ad2-156">You can then acquire hello authorization and token endpoint URIs and create an AuthorizationServiceConfiguration object by running hello following:</span></span>

```java
final Uri issuerUri = Uri.parse(mDiscoveryURI);
AuthorizationServiceConfiguration config;

AuthorizationServiceConfiguration.fetchFromIssuer(
    issuerUri,
    new RetrieveConfigurationCallback() {
      @Override public void onFetchConfigurationCompleted(
          @Nullable AuthorizationServiceConfiguration serviceConfiguration,
          @Nullable AuthorizationException ex) {
        if (ex != null) {
            Log.w(TAG, "Failed tooretrieve configuration for " + issuerUri, ex);
        } else {
            // service configuration retrieved, proceed tooauthorization...
        }
      }
  });
```

<span data-ttu-id="07ad2-157">Místo použití zjišťování tooobtain hello autorizace a koncový bod tokenu identifikátory URI, je můžete také zadat explicitně nahrazením hello klienta\_ID a hello zásad\_názvem v adrese URL hello níže:</span><span class="sxs-lookup"><span data-stu-id="07ad2-157">Instead of using discovery tooobtain hello authorization and token endpoint URIs, you can also specify them explicitly by replacing hello Tenant\_ID and hello Policy\_Name in hello URL's below:</span></span>

```java
String mAuthEndpoint = "https://login.microsoftonline.com/<Tenant_ID>/oauth2/v2.0/authorize?p=<Policy_Name>";

String mTokenEndpoint = "https://login.microsoftonline.com/<Tenant_ID>/oauth2/v2.0/token?p=<Policy_Name>";
```

<span data-ttu-id="07ad2-158">Spusťte následující kód toocreate hello AuthorizationServiceConfiguration objektu:</span><span class="sxs-lookup"><span data-stu-id="07ad2-158">Run hello following code toocreate your AuthorizationServiceConfiguration object:</span></span>

```java
AuthorizationServiceConfiguration config =
        new AuthorizationServiceConfiguration(name, mAuthEndpoint, mTokenEndpoint);

// perform hello auth request...
```

### <a name="authorizing"></a><span data-ttu-id="07ad2-159">Autorizace</span><span class="sxs-lookup"><span data-stu-id="07ad2-159">Authorizing</span></span>

<span data-ttu-id="07ad2-160">Po konfiguraci nebo načítání konfigurace povolení služby, lze sestavit požadavek autorizace.</span><span class="sxs-lookup"><span data-stu-id="07ad2-160">After configuring or retrieving an authorization service configuration, an authorization request can be constructed.</span></span> <span data-ttu-id="07ad2-161">požádat o toocreate hello, budete potřebovat hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="07ad2-161">toocreate hello request, you will need hello following information:</span></span>

* <span data-ttu-id="07ad2-162">ID klienta (například 00000000-0000-0000-0000-000000000000)</span><span class="sxs-lookup"><span data-stu-id="07ad2-162">Client ID (e.g. 00000000-0000-0000-0000-000000000000)</span></span>
* <span data-ttu-id="07ad2-163">Identifikátor URI pro přesměrování s vlastní schéma (např. com.onmicrosoft.fabrikamb2c.exampleapp://oauthredirect)</span><span class="sxs-lookup"><span data-stu-id="07ad2-163">Redirect URI with a custom scheme (e.g. com.onmicrosoft.fabrikamb2c.exampleapp://oauthredirect)</span></span>

<span data-ttu-id="07ad2-164">Obě položky by byly uloženy když jste byli [aplikaci zaregistrujete](#create-an-application).</span><span class="sxs-lookup"><span data-stu-id="07ad2-164">Both items should have been saved when you were [registering your app](#create-an-application).</span></span>

```java
AuthorizationRequest req = new AuthorizationRequest.Builder(
    config,
    clientId,
    ResponseTypeValues.CODE,
    redirectUri)
    .build();
```

<span data-ttu-id="07ad2-165">Podrobnosti najdete toohello [AppAuth průvodce](https://openid.github.io/AppAuth-Android/) na tom, jak toocomplete hello celého procesu hello.</span><span class="sxs-lookup"><span data-stu-id="07ad2-165">Please refer toohello [AppAuth guide](https://openid.github.io/AppAuth-Android/) on how toocomplete hello rest of hello process.</span></span> <span data-ttu-id="07ad2-166">Pokud potřebujete tooquickly začít pracovat s pracovní aplikace, podívejte se na [naše ukázka](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c).</span><span class="sxs-lookup"><span data-stu-id="07ad2-166">If you need tooquickly get started with a working app, check out [our sample](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c).</span></span> <span data-ttu-id="07ad2-167">Postupujte podle kroků hello v hello [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md) tooenter konfiguraci Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="07ad2-167">Follow hello steps in hello [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md) tooenter your own Azure AD B2C configuration.</span></span>

<span data-ttu-id="07ad2-168">Snažíme se vždy otevřete toofeedback a návrhy!</span><span class="sxs-lookup"><span data-stu-id="07ad2-168">We are always open toofeedback and suggestions!</span></span> <span data-ttu-id="07ad2-169">Pokud máte jakékoli problémy s tímto tématem nebo doporučení pro zlepšení tohoto obsahu, by nám chcete sdělit svůj názor hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="07ad2-169">If you have any difficulties with this topic, or have recommendations for improving this content, we would appreciate your feedback at hello bottom of hello page.</span></span> <span data-ttu-id="07ad2-170">Pro žádosti o funkce, přidejte je příliš[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span><span class="sxs-lookup"><span data-stu-id="07ad2-170">For feature requests, add them too[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span></span>

