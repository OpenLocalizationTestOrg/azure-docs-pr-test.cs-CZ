---
title: "Získávání tokenu pomocí aplikace pro iOS – Azure AD B2C | Microsoft Docs"
description: "Tento článek vám ukáže, jak toocreate aplikaci iOS, která používá AppAuth s Azure Active Directory B2C toomanage uživatelských identit a ověření uživatelů."
services: active-directory-b2c
documentationcenter: ios
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: d818a634-42c2-4cbd-bf73-32fa0c8c69d3
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objectivec
ms.topic: article
ms.date: 03/07/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: e7cbe2de6e9ae3d45448cdd36292c457a0ef4887
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-sign-in-using-an-ios-application"></a><span data-ttu-id="3ef47-103">Azure AD B2C: Přihlaste se pomocí aplikace pro iOS</span><span class="sxs-lookup"><span data-stu-id="3ef47-103">Azure AD B2C: Sign-in using an iOS application</span></span>

<span data-ttu-id="3ef47-104">Platforma identity Microsoft Hello používá otevřete standardy, jako je OAuth2 a OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="3ef47-104">hello Microsoft identity platform uses open standards such as OAuth2 and OpenID Connect.</span></span> <span data-ttu-id="3ef47-105">Pomocí otevřený standardní protokol nabízí více možností pro vývojáře, při výběru toointegrate knihovny pomocí našich služeb.</span><span class="sxs-lookup"><span data-stu-id="3ef47-105">Using an open standard protocol offers more developer choice when selecting a library toointegrate with our services.</span></span> <span data-ttu-id="3ef47-106">Nabízíme tento návod a ostatní jako ho vývojáři tooaid s psaní aplikací, které se připojují toohello platformy Microsoft Identity.</span><span class="sxs-lookup"><span data-stu-id="3ef47-106">We've provided this walkthrough and others like it tooaid developers with writing applications that connect toohello Microsoft Identity platform.</span></span> <span data-ttu-id="3ef47-107">Většina knihovny, které implementují [specifikace hello RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) jsou možné tooconnect toohello Microsoft Identity platformy.</span><span class="sxs-lookup"><span data-stu-id="3ef47-107">Most libraries that implement [hello RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) are able tooconnect toohello Microsoft Identity platform.</span></span>

> [!WARNING]
> <span data-ttu-id="3ef47-108">Společnost Microsoft neposkytuje opravy pro knihovny třetích stran a nebyl provádí kontrolu těchto knihoven.</span><span class="sxs-lookup"><span data-stu-id="3ef47-108">Microsoft does not provide fixes for third-party libraries and has not done a review of those libraries.</span></span> <span data-ttu-id="3ef47-109">Tato ukázka je použití knihovny třetích stran volat AppAuth, který byl testován pro kompatibilitu v základní scénáře s hello Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="3ef47-109">This sample is using a third-party library called AppAuth that has been tested for compatibility in basic scenarios with hello Azure AD B2C.</span></span> <span data-ttu-id="3ef47-110">Problémy a žádosti o funkce by měla být směrovanou toohello knihovny open-source projekt.</span><span class="sxs-lookup"><span data-stu-id="3ef47-110">Issues and feature requests should be directed toohello library's open-source project.</span></span> <span data-ttu-id="3ef47-111">Další informace najdete v [tomto článku](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).</span><span class="sxs-lookup"><span data-stu-id="3ef47-111">For more information, see [this article](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).</span></span>
>
>

<span data-ttu-id="3ef47-112">Pokud jste nový tooOAuth2 nebo OpenID Connect, mnohem této ukázkové konfigurace nemusí mít mnoho tooyou smysl.</span><span class="sxs-lookup"><span data-stu-id="3ef47-112">If you're new tooOAuth2 or OpenID Connect, much of this sample configuration may not make much sense tooyou.</span></span> <span data-ttu-id="3ef47-113">Doporučujeme, abyste si prohlédnete brief [přehled protokolu hello jsme jsme tady popisujeme](active-directory-b2c-reference-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="3ef47-113">We recommend you look at a brief [overview of hello protocol we've documented here](active-directory-b2c-reference-protocols.md).</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="3ef47-114">Získání adresáře služby Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="3ef47-114">Get an Azure AD B2C directory</span></span>
<span data-ttu-id="3ef47-115">Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="3ef47-115">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span> <span data-ttu-id="3ef47-116">Adresář je kontejner pro všechny uživatele, aplikace, skupiny a další.</span><span class="sxs-lookup"><span data-stu-id="3ef47-116">A directory is a container for all your users, apps, groups, and more.</span></span> <span data-ttu-id="3ef47-117">Pokud ho ještě nemáte, [vytvořte adresář B2C](active-directory-b2c-get-started.md) předtím, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="3ef47-117">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="3ef47-118">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="3ef47-118">Create an application</span></span>
<span data-ttu-id="3ef47-119">Dále musíte toocreate aplikace ve svém adresáři B2C.</span><span class="sxs-lookup"><span data-stu-id="3ef47-119">Next, you need toocreate an app in your B2C directory.</span></span> <span data-ttu-id="3ef47-120">registrace aplikace Hello poskytuje informace o Azure AD, je nutné toocommunicate bezpečně s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="3ef47-120">hello app registration gives Azure AD information that it needs toocommunicate securely with your app.</span></span> <span data-ttu-id="3ef47-121">toocreate mobilní aplikace, postupujte podle [tyto pokyny](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="3ef47-121">toocreate a mobile app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="3ef47-122">Ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="3ef47-122">Be sure to:</span></span>

* <span data-ttu-id="3ef47-123">Zahrnout **nativního klienta** v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="3ef47-123">Include a **Native client** in hello application.</span></span>
* <span data-ttu-id="3ef47-124">Kopírování hello **ID aplikace** který je přiřazený tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="3ef47-124">Copy hello **Application ID** that is assigned tooyour app.</span></span> <span data-ttu-id="3ef47-125">Tento identifikátor GUID potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="3ef47-125">You need this GUID later.</span></span>
* <span data-ttu-id="3ef47-126">Nastavení **identifikátor URI pro přesměrování** s vlastní schéma (například com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect).</span><span class="sxs-lookup"><span data-stu-id="3ef47-126">Set up a **Redirect URI** with a custom scheme (for example, com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect).</span></span> <span data-ttu-id="3ef47-127">Tento identifikátor URI potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="3ef47-127">You need this URI later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="3ef47-128">Vytvořte svoje zásady</span><span class="sxs-lookup"><span data-stu-id="3ef47-128">Create your policies</span></span>
<span data-ttu-id="3ef47-129">V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="3ef47-129">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="3ef47-130">Tato aplikace obsahuje jeden prostředí identit: například společné přihlášení a odhlášení.</span><span class="sxs-lookup"><span data-stu-id="3ef47-130">This app contains one identity experience: a combined sign-in and sign-up.</span></span> <span data-ttu-id="3ef47-131">Vytvořte tuto zásadu, jak je popsáno v [článku o zásadách](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="3ef47-131">Create this policy as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span> <span data-ttu-id="3ef47-132">Když vytvoříte hello zásady, nezapomeňte:</span><span class="sxs-lookup"><span data-stu-id="3ef47-132">When you create hello policy, be sure to:</span></span>

* <span data-ttu-id="3ef47-133">V části **atributy registrace**, vyberte atribut hello **zobrazovaný název**.</span><span class="sxs-lookup"><span data-stu-id="3ef47-133">Under **Sign-up attributes**, select hello attribute **Display name**.</span></span>  <span data-ttu-id="3ef47-134">Můžete vybrat i další atributy.</span><span class="sxs-lookup"><span data-stu-id="3ef47-134">You can select other attributes as well.</span></span>
* <span data-ttu-id="3ef47-135">V části **deklarace identity aplikace**, vyberte hello deklarací **zobrazovaný název** a **ID objektu uživatele**.</span><span class="sxs-lookup"><span data-stu-id="3ef47-135">Under **Application claims**, select hello claims **Display name** and **User's Object ID**.</span></span> <span data-ttu-id="3ef47-136">Můžete vybrat i další deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="3ef47-136">You can select other claims as well.</span></span>
* <span data-ttu-id="3ef47-137">Kopírování hello **název** po jejím vytvoření každé zásady.</span><span class="sxs-lookup"><span data-stu-id="3ef47-137">Copy hello **Name** of each policy after you create it.</span></span> <span data-ttu-id="3ef47-138">Je název vaší zásady předponu `b2c_1_` při ukládání zásad hello.</span><span class="sxs-lookup"><span data-stu-id="3ef47-138">Your policy name is prefixed with `b2c_1_` when you save hello policy.</span></span>  <span data-ttu-id="3ef47-139">Název zásady hello potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="3ef47-139">You need hello policy name later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="3ef47-140">Po vytvoření zásad jste připravené toobuild vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3ef47-140">After you have created your policies, you're ready toobuild your app.</span></span>

## <a name="download-hello-sample-code"></a><span data-ttu-id="3ef47-141">Stáhněte si ukázkový kód hello</span><span class="sxs-lookup"><span data-stu-id="3ef47-141">Download hello sample code</span></span>
<span data-ttu-id="3ef47-142">Uvádíme pracovní vzorku, který používá AppAuth s Azure AD B2C [na Githubu](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c).</span><span class="sxs-lookup"><span data-stu-id="3ef47-142">We have provided a working sample that uses AppAuth with Azure AD B2C [on GitHub](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c).</span></span> <span data-ttu-id="3ef47-143">Můžete stáhnout hello kód a spusťte ho.</span><span class="sxs-lookup"><span data-stu-id="3ef47-143">You can download hello code and run it.</span></span> <span data-ttu-id="3ef47-144">toouse klienta vlastní Azure AD B2C, postupujte podle pokynů hello v hello [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="3ef47-144">toouse your own Azure AD B2C tenant, follow hello instructions in hello [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md).</span></span>

<span data-ttu-id="3ef47-145">Tato ukázka byla vytvořena podle pokynů README hello podle hello [iOS AppAuth projektu na Githubu](https://github.com/openid/AppAuth-iOS).</span><span class="sxs-lookup"><span data-stu-id="3ef47-145">This sample was created by following hello README instructions by hello [iOS AppAuth project on GitHub](https://github.com/openid/AppAuth-iOS).</span></span> <span data-ttu-id="3ef47-146">Další informace o fungování hello knihovně a hello ukázková odkazovat hello README AppAuth na Githubu.</span><span class="sxs-lookup"><span data-stu-id="3ef47-146">For more details on how hello sample and hello library work, reference hello AppAuth README on GitHub.</span></span>

## <a name="modifying-your-app-toouse-azure-ad-b2c-with-appauth"></a><span data-ttu-id="3ef47-147">Úprava aplikace toouse Azure AD B2C s AppAuth</span><span class="sxs-lookup"><span data-stu-id="3ef47-147">Modifying your app toouse Azure AD B2C with AppAuth</span></span>

> [!NOTE]
> <span data-ttu-id="3ef47-148">AppAuth podporuje iOS 7 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="3ef47-148">AppAuth supports iOS 7 and above.</span></span>  <span data-ttu-id="3ef47-149">Ale je potřeba toosupport sociálních přihlášení na webu Google, SFSafariViewController, které vyžaduje iOS 9 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="3ef47-149">However, toosupport social logins on Google, SFSafariViewController is needed which requires iOS 9 or higher.</span></span>
>

### <a name="configuration"></a><span data-ttu-id="3ef47-150">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="3ef47-150">Configuration</span></span>

<span data-ttu-id="3ef47-151">Komunikaci s Azure AD B2C můžete nakonfigurovat tak, že zadáte koncový bod autorizace hello a koncový bod tokenu identifikátory URI.</span><span class="sxs-lookup"><span data-stu-id="3ef47-151">You can configure communication with Azure AD B2C by specifying both hello authorization endpoint and token endpoint URIs.</span></span>  <span data-ttu-id="3ef47-152">toogenerate tyto identifikátory URI, budete potřebovat hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="3ef47-152">toogenerate these URIs, you need hello following information:</span></span>
* <span data-ttu-id="3ef47-153">ID klienta (například contoso.onmicrosoft.com)</span><span class="sxs-lookup"><span data-stu-id="3ef47-153">Tenant ID (for example, contoso.onmicrosoft.com)</span></span>
* <span data-ttu-id="3ef47-154">Název zásady (například B2C\_1\_SignUpIn)</span><span class="sxs-lookup"><span data-stu-id="3ef47-154">Policy name (for example, B2C\_1\_SignUpIn)</span></span>

<span data-ttu-id="3ef47-155">Hello koncový bod tokenu může být generována URI nahrazení hello klienta\_ID a hello zásad\_název v hello následující adresu URL:</span><span class="sxs-lookup"><span data-stu-id="3ef47-155">hello token endpoint URI can be generated by replacing hello Tenant\_ID and hello Policy\_Name in hello following URL:</span></span>

```objc
static NSString *const tokenEndpoint = @"https://login.microsoftonline.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/token";
```

<span data-ttu-id="3ef47-156">Hello koncový bod autorizace URI může být generována nahrazení hello klienta\_ID a hello zásad\_název v hello následující adresu URL:</span><span class="sxs-lookup"><span data-stu-id="3ef47-156">hello authorization endpoint URI can be generated by replacing hello Tenant\_ID and hello Policy\_Name in hello following URL:</span></span>

```objc
static NSString *const authorizationEndpoint = @"https://login.microsoftonline.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/authorize";
```

<span data-ttu-id="3ef47-157">Spusťte následující kód toocreate hello AuthorizationServiceConfiguration objektu:</span><span class="sxs-lookup"><span data-stu-id="3ef47-157">Run hello following code toocreate your AuthorizationServiceConfiguration object:</span></span>

```objc
OIDServiceConfiguration *configuration = 
    [[OIDServiceConfiguration alloc] initWithAuthorizationEndpoint:authorizationEndpoint tokenEndpoint:tokenEndpoint];
// now we are ready tooperform hello auth request...
```

### <a name="authorizing"></a><span data-ttu-id="3ef47-158">Autorizace</span><span class="sxs-lookup"><span data-stu-id="3ef47-158">Authorizing</span></span>

<span data-ttu-id="3ef47-159">Po konfiguraci nebo načítání konfigurace povolení služby, lze sestavit požadavek autorizace.</span><span class="sxs-lookup"><span data-stu-id="3ef47-159">After configuring or retrieving an authorization service configuration, an authorization request can be constructed.</span></span> <span data-ttu-id="3ef47-160">požádat o toocreate hello, budete potřebovat hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="3ef47-160">toocreate hello request, you need hello following information:</span></span>  
* <span data-ttu-id="3ef47-161">ID klienta (například 00000000-0000-0000-0000-000000000000)</span><span class="sxs-lookup"><span data-stu-id="3ef47-161">Client ID (for example, 00000000-0000-0000-0000-000000000000)</span></span>
* <span data-ttu-id="3ef47-162">Identifikátor URI pro přesměrování s vlastní schéma (například com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect)</span><span class="sxs-lookup"><span data-stu-id="3ef47-162">Redirect URI with a custom scheme (for example, com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect)</span></span>

<span data-ttu-id="3ef47-163">Obě položky by byly uloženy když jste byli [aplikaci zaregistrujete](#create-an-application).</span><span class="sxs-lookup"><span data-stu-id="3ef47-163">Both items should have been saved when you were [registering your app](#create-an-application).</span></span>

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

<span data-ttu-id="3ef47-164">tooset do vaší aplikace toohandle hello přesměrování toohello identifikátor URI s hello vlastní schéma, potřebujete tooupdate hello seznam 'schémata URL, ve vaší Info.pList:</span><span class="sxs-lookup"><span data-stu-id="3ef47-164">tooset up your application toohandle hello redirect toohello URI with hello custom scheme, you need tooupdate hello list of 'URL Schemes' in your Info.pList:</span></span>
* <span data-ttu-id="3ef47-165">Otevřete Info.pList.</span><span class="sxs-lookup"><span data-stu-id="3ef47-165">Open Info.pList.</span></span>
* <span data-ttu-id="3ef47-166">Pozastavte ukazatel myši nad řádek jako kód sady operačního systému typu a klikněte na tlačítko hello \+ symbol.</span><span class="sxs-lookup"><span data-stu-id="3ef47-166">Hover over a row like 'Bundle OS Type Code' and click hello \+ symbol.</span></span>
* <span data-ttu-id="3ef47-167">Přejmenujte hello nový řádek "adresa URL typů".</span><span class="sxs-lookup"><span data-stu-id="3ef47-167">Rename hello new row 'URL types'.</span></span>
* <span data-ttu-id="3ef47-168">Klikněte na tlačítko hello šipka doleva toohello "Adresa URL typů" tooopen hello stromu.</span><span class="sxs-lookup"><span data-stu-id="3ef47-168">Click hello arrow toohello left of 'URL types' tooopen hello tree.</span></span>
* <span data-ttu-id="3ef47-169">Klikněte na tlačítko hello šipku toohello nalevo od '0 položky' tooopen hello stromu.</span><span class="sxs-lookup"><span data-stu-id="3ef47-169">Click hello arrow toohello left of 'Item 0' tooopen hello tree.</span></span>
* <span data-ttu-id="3ef47-170">Přejmenujte první položka pod položku 0 too'URL schémat.</span><span class="sxs-lookup"><span data-stu-id="3ef47-170">Rename first item underneath Item 0 too'URL Schemes'.</span></span>
* <span data-ttu-id="3ef47-171">Klikněte na tlačítko hello šipka doleva toohello 'schémata URL, tooopen hello stromu.</span><span class="sxs-lookup"><span data-stu-id="3ef47-171">Click hello arrow toohello left of 'URL Schemes' tooopen hello tree.</span></span>
* <span data-ttu-id="3ef47-172">Ve sloupci 'Hodnota' hello je prázdné pole toohello nalevo od 'Položky 0' pod 'Adresy URL Schemes'.</span><span class="sxs-lookup"><span data-stu-id="3ef47-172">In hello 'Value' column, there is a blank field toohello left of 'Item 0' underneath 'URL Schemes'.</span></span>  <span data-ttu-id="3ef47-173">Nastavte hello hodnota tooyour aplikace na jedinečné schéma.</span><span class="sxs-lookup"><span data-stu-id="3ef47-173">Set hello value tooyour application's unique scheme.</span></span>  <span data-ttu-id="3ef47-174">Hello hodnota musí odpovídat používané v redirectURL při vytvoření objektu OIDAuthorizationRequest hello schéma hello.</span><span class="sxs-lookup"><span data-stu-id="3ef47-174">hello value must match hello scheme used in redirectURL when creating hello OIDAuthorizationRequest object.</span></span>  <span data-ttu-id="3ef47-175">V našem příkladu jsme použili hello schéma 'com.onmicrosoft.fabrikamb2c.exampleapp'.</span><span class="sxs-lookup"><span data-stu-id="3ef47-175">In our sample, we used hello scheme 'com.onmicrosoft.fabrikamb2c.exampleapp'.</span></span>

<span data-ttu-id="3ef47-176">Odkazovat toohello [AppAuth průvodce](https://openid.github.io/AppAuth-iOS/) na tom, jak toocomplete hello celého procesu hello.</span><span class="sxs-lookup"><span data-stu-id="3ef47-176">Refer toohello [AppAuth guide](https://openid.github.io/AppAuth-iOS/) on how toocomplete hello rest of hello process.</span></span> <span data-ttu-id="3ef47-177">Pokud potřebujete tooquickly začít pracovat s pracovní aplikace, podívejte se na [naše ukázka](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c).</span><span class="sxs-lookup"><span data-stu-id="3ef47-177">If you need tooquickly get started with a working app, check out [our sample](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c).</span></span> <span data-ttu-id="3ef47-178">Postupujte podle kroků hello v hello [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md) tooenter konfiguraci Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="3ef47-178">Follow hello steps in hello [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md) tooenter your own Azure AD B2C configuration.</span></span>

<span data-ttu-id="3ef47-179">Snažíme se vždy otevřete toofeedback a návrhy!</span><span class="sxs-lookup"><span data-stu-id="3ef47-179">We are always open toofeedback and suggestions!</span></span> <span data-ttu-id="3ef47-180">Pokud máte jakékoli problémy s tímto tématem nebo doporučení pro zlepšení tohoto obsahu, by nám chcete sdělit svůj názor hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="3ef47-180">If you have any difficulties with this topic, or have recommendations for improving this content, we would appreciate your feedback at hello bottom of hello page.</span></span> <span data-ttu-id="3ef47-181">Pro žádosti o funkce, přidejte je příliš[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span><span class="sxs-lookup"><span data-stu-id="3ef47-181">For feature requests, add them too[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span></span>
