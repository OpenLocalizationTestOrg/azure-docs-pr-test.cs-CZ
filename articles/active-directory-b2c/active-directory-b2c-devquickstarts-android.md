---
title: "Azure Active Directory B2C: Získávání tokenu pomocí aplikace pro Android | Microsoft Docs"
description: "Tento článek vám ukáže, jak vytvořit aplikaci pro Android používající AppAuth s Azure Active Directory B2C ke správě identit uživatelů a ověřování uživatelů."
services: active-directory-b2c
documentationcenter: android
author: parakhj
manager: mtillman
editor: 
ms.assetid: d00947c3-dcaa-4cb3-8c2e-d94e0746d8b2
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 03/06/2017
ms.author: parakhj
ms.openlocfilehash: dd08c666c09b651e6c0def72a89eda56ba73e34d
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="azure-ad-b2c-sign-in-using-an-android-application"></a><span data-ttu-id="34cbc-103">Azure AD B2C: Přihlaste se pomocí aplikace pro Android</span><span class="sxs-lookup"><span data-stu-id="34cbc-103">Azure AD B2C: Sign-in using an Android application</span></span>

<span data-ttu-id="34cbc-104">Platforma Microsoft identity používá otevřené standardy, jako je například OAuth2 nebo OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="34cbc-104">The Microsoft identity platform uses open standards such as OAuth2 and OpenID Connect.</span></span> <span data-ttu-id="34cbc-105">To umožňuje vývojářům využívat všechny knihovny, které chtějí integrovat do našich služeb.</span><span class="sxs-lookup"><span data-stu-id="34cbc-105">This allows developers to leverage any library they wish to integrate with our services.</span></span> <span data-ttu-id="34cbc-106">Pomoci vývojářům při použití naše platforma s další knihovny, jsme jste zapisuje několik návody podobné následujícímu abychom ukázali, jak nakonfigurovat 3. stran knihovny pro připojení k identitu platformy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="34cbc-106">To aid developers in using our platform with other libraries, we've written a few walkthroughs like this one to demonstrate how to configure 3rd party libraries to connect to the Microsoft identity platform.</span></span> <span data-ttu-id="34cbc-107">Většinu knihoven, které implementují [specifikace RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749), bude možné připojit k platformě Microsoft Identity.</span><span class="sxs-lookup"><span data-stu-id="34cbc-107">Most libraries that implement [the RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) will be able to connect to the Microsoft Identity platform.</span></span>

> [!WARNING]
> <span data-ttu-id="34cbc-108">Společnost Microsoft neposkytuje opravy pro 3. stran knihovny a nebyl provádí kontrolu těchto knihoven.</span><span class="sxs-lookup"><span data-stu-id="34cbc-108">Microsoft does not provide fixes for 3rd party libraries and has not done a review of those libraries.</span></span> <span data-ttu-id="34cbc-109">Tato ukázka používá knihovnu 3. stran volat AppAuth, který byl testován pro kompatibilitu v základní scénáře s Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="34cbc-109">This sample is using a 3rd party library called AppAuth that has been tested for compatibility in basic scenarios with the Azure AD B2C.</span></span> <span data-ttu-id="34cbc-110">Problémy a žádosti o funkce se mají směrovat knihovny open-source projekt.</span><span class="sxs-lookup"><span data-stu-id="34cbc-110">Issues and feature requests should be directed to the library's open-source project.</span></span> <span data-ttu-id="34cbc-111">Najdete v tématu [v tomto článku](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) Další informace.</span><span class="sxs-lookup"><span data-stu-id="34cbc-111">Please see [this article](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) for more information.</span></span>  
>
>

<span data-ttu-id="34cbc-112">Pokud jste ještě nikdy nepracovali s OAuth2 nebo OpenID Connect, pak vám tahle vzorová konfigurace možná nebude moc dávat smysl.</span><span class="sxs-lookup"><span data-stu-id="34cbc-112">If you're new to OAuth2 or OpenID Connect much of this sample configuration may not make much sense to you.</span></span> <span data-ttu-id="34cbc-113">Doporučujeme prohlédnout si stručný [přehled protokolu, který uvádíme tady](active-directory-b2c-reference-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="34cbc-113">We recommend you look at a brief [overview of the protocol we've documented here](active-directory-b2c-reference-protocols.md).</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="34cbc-114">Získání adresáře služby Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="34cbc-114">Get an Azure AD B2C directory</span></span>

<span data-ttu-id="34cbc-115">Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="34cbc-115">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span> <span data-ttu-id="34cbc-116">Adresář je kontejner pro všechny vaše uživatele, aplikace, skupiny a další.</span><span class="sxs-lookup"><span data-stu-id="34cbc-116">A directory is a container for all of your users, apps, groups, and more.</span></span> <span data-ttu-id="34cbc-117">Pokud ho ještě nemáte, [vytvořte adresář B2C](active-directory-b2c-get-started.md) předtím, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="34cbc-117">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="34cbc-118">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="34cbc-118">Create an application</span></span>

<span data-ttu-id="34cbc-119">Dále musíte vytvořit aplikaci v adresáři B2C.</span><span class="sxs-lookup"><span data-stu-id="34cbc-119">Next, you need to create an app in your B2C directory.</span></span> <span data-ttu-id="34cbc-120">Azure AD díky tomu získá informace potřebné k bezpečné komunikaci s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="34cbc-120">This gives Azure AD information that it needs to communicate securely with your app.</span></span> <span data-ttu-id="34cbc-121">Při vytváření mobilních aplikací, postupujte podle [tyto pokyny](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="34cbc-121">To create a mobile app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="34cbc-122">Ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="34cbc-122">Be sure to:</span></span>

* <span data-ttu-id="34cbc-123">Zahrnout **nativního klienta** v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="34cbc-123">Include a **Native Client** in the application.</span></span>
* <span data-ttu-id="34cbc-124">Poznamenejte si **ID aplikace** přiřazené vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="34cbc-124">Copy the **Application ID** that is assigned to your app.</span></span> <span data-ttu-id="34cbc-125">Budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="34cbc-125">You will need this later.</span></span>
* <span data-ttu-id="34cbc-126">Nastavit nativního klienta **identifikátor URI pro přesměrování** (např. com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect).</span><span class="sxs-lookup"><span data-stu-id="34cbc-126">Set up a native client **Redirect URI** (e.g. com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect).</span></span> <span data-ttu-id="34cbc-127">To také budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="34cbc-127">You will also need this later.</span></span>

## <a name="create-your-policies"></a><span data-ttu-id="34cbc-128">Vytvořte svoje zásady</span><span class="sxs-lookup"><span data-stu-id="34cbc-128">Create your policies</span></span>

<span data-ttu-id="34cbc-129">V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="34cbc-129">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="34cbc-130">Tato aplikace obsahuje jeden prostředí identit: například společné přihlášení a odhlášení.</span><span class="sxs-lookup"><span data-stu-id="34cbc-130">This app contains one identity experience: a combined sign-in and sign-up.</span></span> <span data-ttu-id="34cbc-131">Je potřeba vytvořit tuto zásadu, jak je popsáno v [článku o zásadách](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="34cbc-131">You need to create this policy, as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span> <span data-ttu-id="34cbc-132">Při vytváření zásady nezapomeňte na následující:</span><span class="sxs-lookup"><span data-stu-id="34cbc-132">When you create the policy, be sure to:</span></span>

* <span data-ttu-id="34cbc-133">Vyberte **zobrazovaný název** jako atribut registrace ve svojí zásadě.</span><span class="sxs-lookup"><span data-stu-id="34cbc-133">Choose the **Display name** as a sign-up attribute in your policy.</span></span>
* <span data-ttu-id="34cbc-134">Zvolit deklarace identity aplikace **Zobrazovaný název** a **ID objektu** v každé zásadě.</span><span class="sxs-lookup"><span data-stu-id="34cbc-134">Choose the **Display name** and **Object ID** application claims in every policy.</span></span> <span data-ttu-id="34cbc-135">Můžete zvolit i další deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="34cbc-135">You can choose other claims as well.</span></span>
* <span data-ttu-id="34cbc-136">Po vytvoření každé zásady si poznamenejte její **Název**.</span><span class="sxs-lookup"><span data-stu-id="34cbc-136">Copy the **Name** of each policy after you create it.</span></span> <span data-ttu-id="34cbc-137">Měl by mít předponu `b2c_1_`.</span><span class="sxs-lookup"><span data-stu-id="34cbc-137">It should have the prefix `b2c_1_`.</span></span>  <span data-ttu-id="34cbc-138">Název zásady budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="34cbc-138">You'll need the policy name later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="34cbc-139">Po vytvoření zásad jste připraveni k sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="34cbc-139">After you have created your policies, you're ready to build your app.</span></span>

## <a name="download-the-sample-code"></a><span data-ttu-id="34cbc-140">Stažení ukázkového kódu</span><span class="sxs-lookup"><span data-stu-id="34cbc-140">Download the sample code</span></span>

<span data-ttu-id="34cbc-141">Uvádíme pracovní vzorku, který používá AppAuth s Azure AD B2C [na Githubu](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c).</span><span class="sxs-lookup"><span data-stu-id="34cbc-141">We have provided a working sample that uses AppAuth with Azure AD B2C [on GitHub](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c).</span></span> <span data-ttu-id="34cbc-142">Můžete stáhnout kód a spusťte ho.</span><span class="sxs-lookup"><span data-stu-id="34cbc-142">You can download the code and run it.</span></span> <span data-ttu-id="34cbc-143">Můžete rychle začít pracovat s vlastní aplikace pomocí pokynů v konfiguraci Azure AD B2C [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="34cbc-143">You can quickly get started with your own app using your own Azure AD B2C configuration by following the instructions in the [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md).</span></span>

<span data-ttu-id="34cbc-144">Ukázka je změna vzorku poskytované [AppAuth](https://openid.github.io/AppAuth-Android/).</span><span class="sxs-lookup"><span data-stu-id="34cbc-144">The sample is a modification of the sample provided by [AppAuth](https://openid.github.io/AppAuth-Android/).</span></span> <span data-ttu-id="34cbc-145">Navštivte jejich stránky, další informace o AppAuth a jejich funkce.</span><span class="sxs-lookup"><span data-stu-id="34cbc-145">Please visit their page to learn more about AppAuth and its features.</span></span>

## <a name="modifying-your-app-to-use-azure-ad-b2c-with-appauth"></a><span data-ttu-id="34cbc-146">Úprava aplikaci pomocí Azure AD B2C AppAuth</span><span class="sxs-lookup"><span data-stu-id="34cbc-146">Modifying your app to use Azure AD B2C with AppAuth</span></span>

> [!NOTE]
> <span data-ttu-id="34cbc-147">AppAuth podporuje Android API 16 (Jellybean) a vyšší.</span><span class="sxs-lookup"><span data-stu-id="34cbc-147">AppAuth supports Android API 16 (Jellybean) and above.</span></span> <span data-ttu-id="34cbc-148">Doporučujeme používat rozhraní API 23 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="34cbc-148">We recommend using API 23 and above.</span></span>
>

### <a name="configuration"></a><span data-ttu-id="34cbc-149">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="34cbc-149">Configuration</span></span>

<span data-ttu-id="34cbc-150">Komunikaci s Azure AD B2C můžete nakonfigurovat tak, že zadáte zjišťování URI nebo zadáním koncový bod autorizace a koncový bod tokenu identifikátory URI.</span><span class="sxs-lookup"><span data-stu-id="34cbc-150">You can configure communication with Azure AD B2C by either specifying the discovery URI or by specifying both the authorization endpoint and token endpoint URIs.</span></span> <span data-ttu-id="34cbc-151">V obou případech budete potřebovat následující informace:</span><span class="sxs-lookup"><span data-stu-id="34cbc-151">In either case, you will need the following information:</span></span>

* <span data-ttu-id="34cbc-152">ID klienta (např. contoso.onmicrosoft.com)</span><span class="sxs-lookup"><span data-stu-id="34cbc-152">Tenant ID (e.g. contoso.onmicrosoft.com)</span></span>
* <span data-ttu-id="34cbc-153">Název zásady (například B2C\_1\_SignUpIn)</span><span class="sxs-lookup"><span data-stu-id="34cbc-153">Policy name (e.g. B2C\_1\_SignUpIn)</span></span>

<span data-ttu-id="34cbc-154">Pokud zvolíte možnost automaticky zjistit autorizace a koncový bod tokenu identifikátory URI, musíte se k načtení informací ze zjišťování identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="34cbc-154">If you choose to automatically discover the authorization and token endpoint URIs, you will need to fetch information from the discovery URI.</span></span> <span data-ttu-id="34cbc-155">Zjišťování může být generována URI nahrazení klienta\_ID a zásady\_názvem v adrese URL následující:</span><span class="sxs-lookup"><span data-stu-id="34cbc-155">The discovery URI can be generated by replacing the Tenant\_ID and the Policy\_Name in the following URL:</span></span>

```java
String mDiscoveryURI = "https://login.microsoftonline.com/<Tenant_ID>/v2.0/.well-known/openid-configuration?p=<Policy_Name>";
```

<span data-ttu-id="34cbc-156">Potom můžete získat autorizace a koncový bod tokenu identifikátory URI a vytvořit objekt AuthorizationServiceConfiguration spuštěním následující:</span><span class="sxs-lookup"><span data-stu-id="34cbc-156">You can then acquire the authorization and token endpoint URIs and create an AuthorizationServiceConfiguration object by running the following:</span></span>

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
            Log.w(TAG, "Failed to retrieve configuration for " + issuerUri, ex);
        } else {
            // service configuration retrieved, proceed to authorization...
        }
      }
  });
```

<span data-ttu-id="34cbc-157">Namísto použití zjišťování získat autorizace a koncový bod tokenu identifikátory URI, můžete také zadat je explicitně nahrazením klienta\_ID a zásady\_názvem v adrese URL níže:</span><span class="sxs-lookup"><span data-stu-id="34cbc-157">Instead of using discovery to obtain the authorization and token endpoint URIs, you can also specify them explicitly by replacing the Tenant\_ID and the Policy\_Name in the URL's below:</span></span>

```java
String mAuthEndpoint = "https://login.microsoftonline.com/<Tenant_ID>/oauth2/v2.0/authorize?p=<Policy_Name>";

String mTokenEndpoint = "https://login.microsoftonline.com/<Tenant_ID>/oauth2/v2.0/token?p=<Policy_Name>";
```

<span data-ttu-id="34cbc-158">Spusťte následující kód k vytvoření objektu AuthorizationServiceConfiguration:</span><span class="sxs-lookup"><span data-stu-id="34cbc-158">Run the following code to create your AuthorizationServiceConfiguration object:</span></span>

```java
AuthorizationServiceConfiguration config =
        new AuthorizationServiceConfiguration(name, mAuthEndpoint, mTokenEndpoint);

// perform the auth request...
```

### <a name="authorizing"></a><span data-ttu-id="34cbc-159">Probíhá autorizace.</span><span class="sxs-lookup"><span data-stu-id="34cbc-159">Authorizing</span></span>

<span data-ttu-id="34cbc-160">Po konfiguraci nebo načítání konfigurace povolení služby, lze sestavit požadavek autorizace.</span><span class="sxs-lookup"><span data-stu-id="34cbc-160">After configuring or retrieving an authorization service configuration, an authorization request can be constructed.</span></span> <span data-ttu-id="34cbc-161">Pokud chcete vytvořit žádost, budete potřebovat následující informace:</span><span class="sxs-lookup"><span data-stu-id="34cbc-161">To create the request, you will need the following information:</span></span>

* <span data-ttu-id="34cbc-162">ID klienta (například 00000000-0000-0000-0000-000000000000)</span><span class="sxs-lookup"><span data-stu-id="34cbc-162">Client ID (e.g. 00000000-0000-0000-0000-000000000000)</span></span>
* <span data-ttu-id="34cbc-163">Identifikátor URI pro přesměrování s vlastní schéma (např. com.onmicrosoft.fabrikamb2c.exampleapp://oauthredirect)</span><span class="sxs-lookup"><span data-stu-id="34cbc-163">Redirect URI with a custom scheme (e.g. com.onmicrosoft.fabrikamb2c.exampleapp://oauthredirect)</span></span>

<span data-ttu-id="34cbc-164">Obě položky by byly uloženy když jste byli [aplikaci zaregistrujete](#create-an-application).</span><span class="sxs-lookup"><span data-stu-id="34cbc-164">Both items should have been saved when you were [registering your app](#create-an-application).</span></span>

```java
AuthorizationRequest req = new AuthorizationRequest.Builder(
    config,
    clientId,
    ResponseTypeValues.CODE,
    redirectUri)
    .build();
```

<span data-ttu-id="34cbc-165">Naleznete [AppAuth průvodce](https://openid.github.io/AppAuth-Android/) o tom, jak dokončete proces.</span><span class="sxs-lookup"><span data-stu-id="34cbc-165">Please refer to the [AppAuth guide](https://openid.github.io/AppAuth-Android/) on how to complete the rest of the process.</span></span> <span data-ttu-id="34cbc-166">Pokud budete potřebovat rychle začít s pracovní aplikace, podívejte se na [naše ukázka](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c).</span><span class="sxs-lookup"><span data-stu-id="34cbc-166">If you need to quickly get started with a working app, check out [our sample](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c).</span></span> <span data-ttu-id="34cbc-167">Postupujte podle kroků v [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md) k zadání vlastní konfigurace Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="34cbc-167">Follow the steps in the [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md) to enter your own Azure AD B2C configuration.</span></span>

<span data-ttu-id="34cbc-168">Snažíme se vždy otevřený a názory a návrhy!</span><span class="sxs-lookup"><span data-stu-id="34cbc-168">We are always open to feedback and suggestions!</span></span> <span data-ttu-id="34cbc-169">Pokud máte jakékoli problémy s tímto tématem nebo doporučení pro zlepšení tohoto obsahu, by nám chcete sdělit svůj názor v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="34cbc-169">If you have any difficulties with this topic, or have recommendations for improving this content, we would appreciate your feedback at the bottom of the page.</span></span> <span data-ttu-id="34cbc-170">Pro žádosti o funkce, přidejte je do [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span><span class="sxs-lookup"><span data-stu-id="34cbc-170">For feature requests, add them to [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span></span>

