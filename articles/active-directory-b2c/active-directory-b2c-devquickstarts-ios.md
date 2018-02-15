---
title: "Použití AppAuth v aplikaci pro systém iOS – Azure Active Directory B2C"
description: "Tento článek ukazuje, jak vytvořit aplikaci iOS, která používá AppAuth s Azure Active Directory B2C ke správě identit uživatelů a ověřování uživatelů."
services: active-directory-b2c
documentationcenter: ios
author: PatAltimore
manager: mtillman
editor: parakhj
ms.custom: seo
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objectivec
ms.topic: article
ms.date: 03/07/2017
ms.author: saeeda
ms.openlocfilehash: b4f46129a7a18e4653d714599630d6cdddfff4ed
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="azure-ad-b2c-sign-in-using-an-ios-application"></a><span data-ttu-id="35424-103">Azure AD B2C: Přihlaste se pomocí aplikace pro iOS</span><span class="sxs-lookup"><span data-stu-id="35424-103">Azure AD B2C: Sign-in using an iOS application</span></span>

<span data-ttu-id="35424-104">Platforma Microsoft identity používá otevřené standardy, jako je například OAuth2 nebo OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="35424-104">The Microsoft identity platform uses open standards such as OAuth2 and OpenID Connect.</span></span> <span data-ttu-id="35424-105">Pomocí otevřený standardní protokol nabízí více možností pro vývojáře, při výběru knihovnu, kterou chcete integrovat s našich služeb.</span><span class="sxs-lookup"><span data-stu-id="35424-105">Using an open standard protocol offers more developer choice when selecting a library to integrate with our services.</span></span> <span data-ttu-id="35424-106">Nabízíme tento návod a ostatní jako ho a usnadňuje vývojářům s psaní aplikací, které se připojují k platformě Microsoft Identity.</span><span class="sxs-lookup"><span data-stu-id="35424-106">We've provided this walkthrough and others like it to aid developers with writing applications that connect to the Microsoft Identity platform.</span></span> <span data-ttu-id="35424-107">Většina knihovny, které implementují [specifikace RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) dokážou připojit k platformě Microsoft Identity.</span><span class="sxs-lookup"><span data-stu-id="35424-107">Most libraries that implement [the RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) are able to connect to the Microsoft Identity platform.</span></span>

> [!WARNING]
> <span data-ttu-id="35424-108">Společnost Microsoft neposkytuje opravy pro knihovny třetích stran a nebyl provádí kontrolu těchto knihoven.</span><span class="sxs-lookup"><span data-stu-id="35424-108">Microsoft does not provide fixes for third-party libraries and has not done a review of those libraries.</span></span> <span data-ttu-id="35424-109">Tato ukázka je použití knihovny třetích stran volat AppAuth, který byl testován pro kompatibilitu v základní scénáře s Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="35424-109">This sample is using a third-party library called AppAuth that has been tested for compatibility in basic scenarios with the Azure AD B2C.</span></span> <span data-ttu-id="35424-110">Problémy a žádosti o funkce se mají směrovat knihovny open-source projekt.</span><span class="sxs-lookup"><span data-stu-id="35424-110">Issues and feature requests should be directed to the library's open-source project.</span></span> <span data-ttu-id="35424-111">Další informace najdete v [tomto článku](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).</span><span class="sxs-lookup"><span data-stu-id="35424-111">For more information, see [this article](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).</span></span>
>
>

<span data-ttu-id="35424-112">Pokud jste ještě OAuth2 nebo OpenID Connect, mnohem této ukázkové konfigurace nemusí mít mnoho smysl pro vás.</span><span class="sxs-lookup"><span data-stu-id="35424-112">If you're new to OAuth2 or OpenID Connect, much of this sample configuration may not make much sense to you.</span></span> <span data-ttu-id="35424-113">Doporučujeme prohlédnout si stručný [přehled protokolu, který uvádíme tady](active-directory-b2c-reference-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="35424-113">We recommend you look at a brief [overview of the protocol we've documented here](active-directory-b2c-reference-protocols.md).</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="35424-114">Získání adresáře služby Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="35424-114">Get an Azure AD B2C directory</span></span>
<span data-ttu-id="35424-115">Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="35424-115">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span> <span data-ttu-id="35424-116">Adresář je kontejner pro všechny uživatele, aplikace, skupiny a další.</span><span class="sxs-lookup"><span data-stu-id="35424-116">A directory is a container for all your users, apps, groups, and more.</span></span> <span data-ttu-id="35424-117">Pokud ho ještě nemáte, [vytvořte adresář B2C](active-directory-b2c-get-started.md) předtím, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="35424-117">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="35424-118">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="35424-118">Create an application</span></span>
<span data-ttu-id="35424-119">Dále musíte vytvořit aplikaci v adresáři B2C.</span><span class="sxs-lookup"><span data-stu-id="35424-119">Next, you need to create an app in your B2C directory.</span></span> <span data-ttu-id="35424-120">Registrace aplikací poskytuje Azure AD informace potřebné k bezpečné komunikaci s vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="35424-120">The app registration gives Azure AD information that it needs to communicate securely with your app.</span></span> <span data-ttu-id="35424-121">Při vytváření mobilních aplikací, postupujte podle [tyto pokyny](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="35424-121">To create a mobile app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="35424-122">Ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="35424-122">Be sure to:</span></span>

* <span data-ttu-id="35424-123">Zahrnout **nativního klienta** v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="35424-123">Include a **Native client** in the application.</span></span>
* <span data-ttu-id="35424-124">Poznamenejte si **ID aplikace** přiřazené vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="35424-124">Copy the **Application ID** that is assigned to your app.</span></span> <span data-ttu-id="35424-125">Tento identifikátor GUID potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="35424-125">You need this GUID later.</span></span>
* <span data-ttu-id="35424-126">Nastavení **identifikátor URI pro přesměrování** s vlastní schéma (například com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect).</span><span class="sxs-lookup"><span data-stu-id="35424-126">Set up a **Redirect URI** with a custom scheme (for example, com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect).</span></span> <span data-ttu-id="35424-127">Tento identifikátor URI potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="35424-127">You need this URI later.</span></span>

## <a name="create-your-policies"></a><span data-ttu-id="35424-128">Vytvořte svoje zásady</span><span class="sxs-lookup"><span data-stu-id="35424-128">Create your policies</span></span>
<span data-ttu-id="35424-129">V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="35424-129">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="35424-130">Tato aplikace obsahuje jeden prostředí identit: například společné přihlášení a odhlášení.</span><span class="sxs-lookup"><span data-stu-id="35424-130">This app contains one identity experience: a combined sign-in and sign-up.</span></span> <span data-ttu-id="35424-131">Vytvořte tuto zásadu, jak je popsáno v [článku o zásadách](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="35424-131">Create this policy as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span> <span data-ttu-id="35424-132">Při vytváření zásady nezapomeňte na následující:</span><span class="sxs-lookup"><span data-stu-id="35424-132">When you create the policy, be sure to:</span></span>

* <span data-ttu-id="35424-133">V části **atributy registrace**, vyberte atribut **zobrazovaný název**.</span><span class="sxs-lookup"><span data-stu-id="35424-133">Under **Sign-up attributes**, select the attribute **Display name**.</span></span>  <span data-ttu-id="35424-134">Můžete vybrat i další atributy.</span><span class="sxs-lookup"><span data-stu-id="35424-134">You can select other attributes as well.</span></span>
* <span data-ttu-id="35424-135">V části **deklarace identity aplikace**, vyberte deklarace identity **zobrazovaný název** a **ID objektu uživatele**.</span><span class="sxs-lookup"><span data-stu-id="35424-135">Under **Application claims**, select the claims **Display name** and **User's Object ID**.</span></span> <span data-ttu-id="35424-136">Můžete vybrat i další deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="35424-136">You can select other claims as well.</span></span>
* <span data-ttu-id="35424-137">Po vytvoření každé zásady si poznamenejte její **Název**.</span><span class="sxs-lookup"><span data-stu-id="35424-137">Copy the **Name** of each policy after you create it.</span></span> <span data-ttu-id="35424-138">Je název vaší zásady předponu `b2c_1_` při ukládání zásad.</span><span class="sxs-lookup"><span data-stu-id="35424-138">Your policy name is prefixed with `b2c_1_` when you save the policy.</span></span>  <span data-ttu-id="35424-139">Název zásady potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="35424-139">You need the policy name later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="35424-140">Po vytvoření zásad jste připraveni k sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="35424-140">After you have created your policies, you're ready to build your app.</span></span>

## <a name="download-the-sample-code"></a><span data-ttu-id="35424-141">Stažení ukázkového kódu</span><span class="sxs-lookup"><span data-stu-id="35424-141">Download the sample code</span></span>
<span data-ttu-id="35424-142">Uvádíme pracovní vzorku, který používá AppAuth s Azure AD B2C [na Githubu](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c).</span><span class="sxs-lookup"><span data-stu-id="35424-142">We have provided a working sample that uses AppAuth with Azure AD B2C [on GitHub](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c).</span></span> <span data-ttu-id="35424-143">Můžete stáhnout kód a spusťte ho.</span><span class="sxs-lookup"><span data-stu-id="35424-143">You can download the code and run it.</span></span> <span data-ttu-id="35424-144">Vlastního klienta Azure AD B2C, postupujte podle pokynů v [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="35424-144">To use your own Azure AD B2C tenant, follow the instructions in the [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md).</span></span>

<span data-ttu-id="35424-145">Tato ukázka byla vytvořena podle těchto pokynů README pomocí [iOS AppAuth projektu na Githubu](https://github.com/openid/AppAuth-iOS).</span><span class="sxs-lookup"><span data-stu-id="35424-145">This sample was created by following the README instructions by the [iOS AppAuth project on GitHub](https://github.com/openid/AppAuth-iOS).</span></span> <span data-ttu-id="35424-146">Další informace o fungování vzorku a knihovně odkazovat v souboru README AppAuth na Githubu.</span><span class="sxs-lookup"><span data-stu-id="35424-146">For more details on how the sample and the library work, reference the AppAuth README on GitHub.</span></span>

## <a name="modifying-your-app-to-use-azure-ad-b2c-with-appauth"></a><span data-ttu-id="35424-147">Úprava aplikaci pomocí Azure AD B2C AppAuth</span><span class="sxs-lookup"><span data-stu-id="35424-147">Modifying your app to use Azure AD B2C with AppAuth</span></span>

> [!NOTE]
> <span data-ttu-id="35424-148">AppAuth podporuje iOS 7 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="35424-148">AppAuth supports iOS 7 and above.</span></span>  <span data-ttu-id="35424-149">Ale pro podporu sociálních přihlášení na webu Google, je potřeba SFSafariViewController což vyžaduje, aby iOS 9 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="35424-149">However, to support social logins on Google, SFSafariViewController is needed which requires iOS 9 or higher.</span></span>
>

### <a name="configuration"></a><span data-ttu-id="35424-150">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="35424-150">Configuration</span></span>

<span data-ttu-id="35424-151">Komunikaci s Azure AD B2C můžete nakonfigurovat tak, že zadáte koncový bod autorizace i koncový bod tokenu identifikátory URI.</span><span class="sxs-lookup"><span data-stu-id="35424-151">You can configure communication with Azure AD B2C by specifying both the authorization endpoint and token endpoint URIs.</span></span>  <span data-ttu-id="35424-152">Chcete-li vygenerovat tyto identifikátory URI, je třeba následující informace:</span><span class="sxs-lookup"><span data-stu-id="35424-152">To generate these URIs, you need the following information:</span></span>
* <span data-ttu-id="35424-153">ID klienta (například contoso.onmicrosoft.com)</span><span class="sxs-lookup"><span data-stu-id="35424-153">Tenant ID (for example, contoso.onmicrosoft.com)</span></span>
* <span data-ttu-id="35424-154">Název zásady (například B2C\_1\_SignUpIn)</span><span class="sxs-lookup"><span data-stu-id="35424-154">Policy name (for example, B2C\_1\_SignUpIn)</span></span>

<span data-ttu-id="35424-155">Koncový bod tokenu může být generována URI nahrazení klienta\_ID a zásady\_názvem v adrese URL následující:</span><span class="sxs-lookup"><span data-stu-id="35424-155">The token endpoint URI can be generated by replacing the Tenant\_ID and the Policy\_Name in the following URL:</span></span>

```objc
static NSString *const tokenEndpoint = @"https://login.microsoftonline.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/token";
```

<span data-ttu-id="35424-156">Koncový bod autorizace URI může být generována nahrazení klienta\_ID a zásady\_názvem v adrese URL následující:</span><span class="sxs-lookup"><span data-stu-id="35424-156">The authorization endpoint URI can be generated by replacing the Tenant\_ID and the Policy\_Name in the following URL:</span></span>

```objc
static NSString *const authorizationEndpoint = @"https://login.microsoftonline.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/authorize";
```

<span data-ttu-id="35424-157">Spusťte následující kód k vytvoření objektu AuthorizationServiceConfiguration:</span><span class="sxs-lookup"><span data-stu-id="35424-157">Run the following code to create your AuthorizationServiceConfiguration object:</span></span>

```objc
OIDServiceConfiguration *configuration = 
    [[OIDServiceConfiguration alloc] initWithAuthorizationEndpoint:authorizationEndpoint tokenEndpoint:tokenEndpoint];
// now we are ready to perform the auth request...
```

### <a name="authorizing"></a><span data-ttu-id="35424-158">Probíhá autorizace.</span><span class="sxs-lookup"><span data-stu-id="35424-158">Authorizing</span></span>

<span data-ttu-id="35424-159">Po konfiguraci nebo načítání konfigurace povolení služby, lze sestavit požadavek autorizace.</span><span class="sxs-lookup"><span data-stu-id="35424-159">After configuring or retrieving an authorization service configuration, an authorization request can be constructed.</span></span> <span data-ttu-id="35424-160">Chcete-li vytvořit požadavek, je třeba následující informace:</span><span class="sxs-lookup"><span data-stu-id="35424-160">To create the request, you need the following information:</span></span>  
* <span data-ttu-id="35424-161">ID klienta (například 00000000-0000-0000-0000-000000000000)</span><span class="sxs-lookup"><span data-stu-id="35424-161">Client ID (for example, 00000000-0000-0000-0000-000000000000)</span></span>
* <span data-ttu-id="35424-162">Identifikátor URI pro přesměrování s vlastní schéma (například com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect)</span><span class="sxs-lookup"><span data-stu-id="35424-162">Redirect URI with a custom scheme (for example, com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect)</span></span>

<span data-ttu-id="35424-163">Obě položky by byly uloženy když jste byli [aplikaci zaregistrujete](#create-an-application).</span><span class="sxs-lookup"><span data-stu-id="35424-163">Both items should have been saved when you were [registering your app](#create-an-application).</span></span>

```objc
OIDAuthorizationRequest *request = 
    [[OIDAuthorizationRequest alloc] initWithConfiguration:configuration
                                                  clientId:kClientId
                                                    scopes:@[OIDScopeOpenID, OIDScopeProfile]
                                               redirectURL:[NSURL URLWithString:kRedirectUri]
                                              responseType:OIDResponseTypeCode
                                      additionalParameters:nil];

AppDelegate *appDelegate = (AppDelegate *)[UIApplication sharedApplication].delegate;
appDelegate.currentAuthorizationFlow = 
    [OIDAuthState authStateByPresentingAuthorizationRequest:request
                                   presentingViewController:self
                                                   callback:^(OIDAuthState *_Nullable authState, NSError *_Nullable error) {
        if (authState) {
            NSLog(@"Got authorization tokens. Access token: %@", authState.lastTokenResponse.accessToken);
            [self setAuthState:authState];
        } else {
            NSLog(@"Authorization error: %@", [error localizedDescription]);
            [self setAuthState:nil];
        }
    }];
```

<span data-ttu-id="35424-164">Pokud chcete nastavit aplikaci pro zpracování přesměrování na identifikátor URI s vlastní schéma, budete muset aktualizovat seznam URL schémata ve vaší Info.pList:</span><span class="sxs-lookup"><span data-stu-id="35424-164">To set up your application to handle the redirect to the URI with the custom scheme, you need to update the list of 'URL Schemes' in your Info.pList:</span></span>
* <span data-ttu-id="35424-165">Otevřete Info.pList.</span><span class="sxs-lookup"><span data-stu-id="35424-165">Open Info.pList.</span></span>
* <span data-ttu-id="35424-166">Pozastavte ukazatel myši nad řádek jako kód sady operačního systému typu a klikněte na \+ symbol.</span><span class="sxs-lookup"><span data-stu-id="35424-166">Hover over a row like 'Bundle OS Type Code' and click the \+ symbol.</span></span>
* <span data-ttu-id="35424-167">Přejmenujte nový řádek "Adresa URL typů".</span><span class="sxs-lookup"><span data-stu-id="35424-167">Rename the new row 'URL types'.</span></span>
* <span data-ttu-id="35424-168">Klikněte na šipku nalevo od "Adresa URL typů" otevřete stromu.</span><span class="sxs-lookup"><span data-stu-id="35424-168">Click the arrow to the left of 'URL types' to open the tree.</span></span>
* <span data-ttu-id="35424-169">Klikněte na šipku nalevo od ' položky 0' otevřete stromu.</span><span class="sxs-lookup"><span data-stu-id="35424-169">Click the arrow to the left of 'Item 0' to open the tree.</span></span>
* <span data-ttu-id="35424-170">První položka pod položky 0, schémata URL, přejmenujte.</span><span class="sxs-lookup"><span data-stu-id="35424-170">Rename first item underneath Item 0 to 'URL Schemes'.</span></span>
* <span data-ttu-id="35424-171">Klikněte na šipku nalevo od adresy URL schémata otevřete stromu.</span><span class="sxs-lookup"><span data-stu-id="35424-171">Click the arrow to the left of 'URL Schemes' to open the tree.</span></span>
* <span data-ttu-id="35424-172">Ve sloupci 'Hodnota' je prázdné pole nalevo od 'Položky 0' pod 'Adresy URL Schemes'.</span><span class="sxs-lookup"><span data-stu-id="35424-172">In the 'Value' column, there is a blank field to the left of 'Item 0' underneath 'URL Schemes'.</span></span>  <span data-ttu-id="35424-173">Nastavte hodnotu na jedinečné schéma vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="35424-173">Set the value to your application's unique scheme.</span></span>  <span data-ttu-id="35424-174">Hodnota musí odpovídat používané v redirectURL při vytváření objektu OIDAuthorizationRequest schéma.</span><span class="sxs-lookup"><span data-stu-id="35424-174">The value must match the scheme used in redirectURL when creating the OIDAuthorizationRequest object.</span></span>  <span data-ttu-id="35424-175">V ukázce se používá schéma 'com.onmicrosoft.fabrikamb2c.exampleapp'.</span><span class="sxs-lookup"><span data-stu-id="35424-175">In the sample,  the scheme 'com.onmicrosoft.fabrikamb2c.exampleapp' is used.</span></span>

<span data-ttu-id="35424-176">Odkazovat [AppAuth průvodce](https://openid.github.io/AppAuth-iOS/) o tom, jak dokončete proces.</span><span class="sxs-lookup"><span data-stu-id="35424-176">Refer to the [AppAuth guide](https://openid.github.io/AppAuth-iOS/) on how to complete the rest of the process.</span></span> <span data-ttu-id="35424-177">Pokud budete potřebovat rychle začít s pracovní aplikace, podívejte se na [vzorku](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c).</span><span class="sxs-lookup"><span data-stu-id="35424-177">If you need to quickly get started with a working app, check out [the sample](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c).</span></span> <span data-ttu-id="35424-178">Postupujte podle kroků v [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md) k zadání vlastní konfigurace Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="35424-178">Follow the steps in the [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md) to enter your own Azure AD B2C configuration.</span></span>

<span data-ttu-id="35424-179">Snažíme se vždy otevřený a názory a návrhy!</span><span class="sxs-lookup"><span data-stu-id="35424-179">We are always open to feedback and suggestions!</span></span> <span data-ttu-id="35424-180">Pokud máte jakékoli problémy s tímto článkem nebo doporučení pro zlepšení tohoto obsahu, by nám chcete sdělit svůj názor v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="35424-180">If you have any difficulties with this article, or have recommendations for improving this content, we would appreciate your feedback at the bottom of the page.</span></span> <span data-ttu-id="35424-181">Pro žádosti o funkce, přidejte je do [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span><span class="sxs-lookup"><span data-stu-id="35424-181">For feature requests, add them to [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span></span>
