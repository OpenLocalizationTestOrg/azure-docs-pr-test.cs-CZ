---
title: "aplikace pro aaaAzure služby Active Directory v2.0 Android | Microsoft Docs"
description: "Jak toobuild aplikace pro Android s přihlašováním uživatelů s osobní účet Microsoft a pracovní nebo školní účty a volání hello rozhraní Graph API pomocí knihoven jiných dodavatelů."
services: active-directory
documentationcenter: 
author: danieldobalian
manager: mbaldwin
editor: 
ms.assetid: 16294c07-f27d-45c9-833f-7dbb12083794
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: 1dd40bd3bcea28c629abce09abaed66b38774162
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooan-android-app-using-a-third-party-library-with-graph-api-using-hello-v20-endpoint"></a><span data-ttu-id="c4def-103">Přidat aplikaci pro Android tooan přihlášení pomocí rozhraní Graph API pomocí koncového bodu v2.0 hello knihovně třetích stran</span><span class="sxs-lookup"><span data-stu-id="c4def-103">Add sign-in tooan Android app using a third-party library with Graph API using hello v2.0 endpoint</span></span>
<span data-ttu-id="c4def-104">Platforma identity Microsoft Hello používá otevřete standardy, jako je OAuth2 a OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="c4def-104">hello Microsoft identity platform uses open standards such as OAuth2 and OpenID Connect.</span></span> <span data-ttu-id="c4def-105">Vývojáři mohou použít žádnou knihovnu chtějí toointegrate s našich služeb.</span><span class="sxs-lookup"><span data-stu-id="c4def-105">Developers can use any library they want toointegrate with our services.</span></span> <span data-ttu-id="c4def-106">Vývojáři toohelp používat naše platforma s další knihovny, jsme jste zapsána několik návody jako tento jeden toodemonstrate jak tooconfigure třetích stran knihovny tooconnect toohello Microsoft identity platformy.</span><span class="sxs-lookup"><span data-stu-id="c4def-106">toohelp developers use our platform with other libraries, we've written a few walkthroughs like this one toodemonstrate how tooconfigure third-party libraries tooconnect toohello Microsoft identity platform.</span></span> <span data-ttu-id="c4def-107">Většina knihovny, které implementují [specifikace hello RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) můžete připojení toohello Microsoft identity platformy.</span><span class="sxs-lookup"><span data-stu-id="c4def-107">Most libraries that implement [hello RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) can connect toohello Microsoft identity platform.</span></span>

<span data-ttu-id="c4def-108">Hello aplikaci, která vytváří Tento názorný postup mohou uživatelé přihlásit tootheir organizace a poté vyhledejte sami ve své organizaci pomocí hello rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="c4def-108">With hello application that this walkthrough creates, users can sign in tootheir organization and then search for themselves in their organization by using hello Graph API.</span></span>

<span data-ttu-id="c4def-109">Pokud jste nový tooOAuth2 nebo OpenID Connect, mnohem této ukázkové konfigurace nemusí mít smysl tooyou.</span><span class="sxs-lookup"><span data-stu-id="c4def-109">If you're new tooOAuth2 or OpenID Connect, much of this sample configuration may not make sense tooyou.</span></span> <span data-ttu-id="c4def-110">Doporučujeme, abyste si přečetli [2.0 protokoly - toku OAuth 2.0 autorizační kód](active-directory-v2-protocols-oauth-code.md) pozadí.</span><span class="sxs-lookup"><span data-stu-id="c4def-110">We recommend that you read [2.0 Protocols - OAuth 2.0 Authorization Code Flow](active-directory-v2-protocols-oauth-code.md) for background.</span></span>

> [!NOTE]
> <span data-ttu-id="c4def-111">Některé funkce naše platformy, které mají výraz v hello OAuth2 nebo OpenID Connect standardy, jako je například podmíněný přístup a správu zásad Intune, vyžadují jste toouse naše s otevřeným zdrojem Identity pro knihovny Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="c4def-111">Some features of our platform that do have an expression in hello OAuth2 or OpenID Connect standards, such as Conditional Access and Intune policy management, require you toouse our open source Microsoft Azure Identity Libraries.</span></span>
> 
> 

<span data-ttu-id="c4def-112">koncový bod v2.0 Hello nepodporuje všechny scénáře Azure Active Directory a funkce.</span><span class="sxs-lookup"><span data-stu-id="c4def-112">hello v2.0 endpoint does not support all Azure Active Directory scenarios and features.</span></span>

> [!NOTE]
> <span data-ttu-id="c4def-113">toodetermine Pokud byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="c4def-113">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="download-hello-code-from-github"></a><span data-ttu-id="c4def-114">Stáhněte si kód hello z Githubu</span><span class="sxs-lookup"><span data-stu-id="c4def-114">Download hello code from GitHub</span></span>
<span data-ttu-id="c4def-115">Hello kód v tomto kurzu se udržuje [na Githubu](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2).</span><span class="sxs-lookup"><span data-stu-id="c4def-115">hello code for this tutorial is maintained [on GitHub](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2).</span></span>  <span data-ttu-id="c4def-116">toofollow společně, můžete [stáhnout kostru aplikace hello jako ZIP](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) nebo hello kostru klonovat:</span><span class="sxs-lookup"><span data-stu-id="c4def-116">toofollow along, you can  [download hello app's skeleton as a .zip](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) or clone hello skeleton:</span></span>

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

<span data-ttu-id="c4def-117">Můžete také právě stažení ukázky hello a rovnou začít:</span><span class="sxs-lookup"><span data-stu-id="c4def-117">You can also just download hello sample and get started right away:</span></span>

```
git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

## <a name="register-an-app"></a><span data-ttu-id="c4def-118">Registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="c4def-118">Register an app</span></span>
<span data-ttu-id="c4def-119">Vytvoření nové aplikace v hello [portálu pro registraci aplikace](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo postupujte podle hello podrobné kroky v [jak tooregister aplikace s koncovým bodem v2.0 hello](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="c4def-119">Create a new app at hello [Application registration portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow hello detailed steps at [How tooregister an app with hello v2.0 endpoint](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="c4def-120">Zkontrolujte, že:</span><span class="sxs-lookup"><span data-stu-id="c4def-120">Make sure to:</span></span>

* <span data-ttu-id="c4def-121">Kopírování hello **Id aplikace** aplikace přiřazené tooyour důvodem je, že ho budete potřebovat brzy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="c4def-121">Copy hello **Application Id** that's assigned tooyour app because you'll need it soon.</span></span>
* <span data-ttu-id="c4def-122">Přidat hello **Mobile** platformu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c4def-122">Add hello **Mobile** platform for your app.</span></span>

> <span data-ttu-id="c4def-123">Poznámka: portál pro registraci aplikace hello poskytuje **identifikátor URI pro přesměrování** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c4def-123">Note: hello Application registration portal provides a **Redirect URI** value.</span></span> <span data-ttu-id="c4def-124">Ale v tomto příkladu musí používat výchozí hodnotu hello `https://login.microsoftonline.com/common/oauth2/nativeclient`.</span><span class="sxs-lookup"><span data-stu-id="c4def-124">However, in this example you must use hello default value of `https://login.microsoftonline.com/common/oauth2/nativeclient`.</span></span>
> 
> 

## <a name="download-hello-nxoauth2-third-party-library-and-create-a-workspace"></a><span data-ttu-id="c4def-125">Stažení hello NXOAuth2 třetích stran knihovny a vytvořit pracovní prostor</span><span class="sxs-lookup"><span data-stu-id="c4def-125">Download hello NXOAuth2 third-party library and create a workspace</span></span>
<span data-ttu-id="c4def-126">V tomto návodu budete používat hello OIDCAndroidLib z Githubu, která je OAuth2 knihovny založené na hello kód Google OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="c4def-126">For this walkthrough, you will use hello OIDCAndroidLib from GitHub, which is an OAuth2 library based on hello OpenID Connect code of Google.</span></span> <span data-ttu-id="c4def-127">Profil nativní aplikace hello implementuje a podporuje koncový bod autorizace hello hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="c4def-127">It implements hello native application profile and supports hello authorization endpoint of hello user.</span></span> <span data-ttu-id="c4def-128">To je vše, co hello, že potřebujete toointegrate s platformou identity Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="c4def-128">These are all hello things that you'll need toointegrate with hello Microsoft identity platform.</span></span>

<span data-ttu-id="c4def-129">Klonování hello OIDCAndroidLib úložišti tooyour počítače.</span><span class="sxs-lookup"><span data-stu-id="c4def-129">Clone hello OIDCAndroidLib repo tooyour computer.</span></span>

```
git@github.com:kalemontes/OIDCAndroidLib.git
```

![androidStudio](../media/active-directory-android-native-oidcandroidlib-v2/emotes-url.png)

## <a name="set-up-your-android-studio-environment"></a><span data-ttu-id="c4def-131">Nastavení prostředí Android Studio</span><span class="sxs-lookup"><span data-stu-id="c4def-131">Set up your Android Studio environment</span></span>
1. <span data-ttu-id="c4def-132">Vytvořte nový projekt Android Studio a přijměte výchozí hodnoty hello v Průvodci hello.</span><span class="sxs-lookup"><span data-stu-id="c4def-132">Create a new Android Studio project and accept hello defaults in hello wizard.</span></span>
   
    ![Vytvořit nový projekt v Android Studio](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample1.PNG)
   
    ![Cílové zařízení se systémem Android](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample2.PNG)
   
    ![Přidat toomobile aktivity](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample3.PNG)
2. <span data-ttu-id="c4def-136">tooset až moduly vašeho projektu přesunout umístění projektu toohello hello klonovat úložiště.</span><span class="sxs-lookup"><span data-stu-id="c4def-136">tooset up your project modules, move hello cloned repo toohello project location.</span></span> <span data-ttu-id="c4def-137">Můžete také vytvořit hello projekt a poté klonovat ji přímo toohello umístění projektu.</span><span class="sxs-lookup"><span data-stu-id="c4def-137">You can also create hello project and then clone it directly toohello project location.</span></span>
   
    ![Moduly projektu](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4_1.PNG)
3. <span data-ttu-id="c4def-139">Otevřete nastavení moduly hello projektu pomocí hello kontextové nabídky, nebo pomocí zástupce Ctrl + Alt + avní + S hello.</span><span class="sxs-lookup"><span data-stu-id="c4def-139">Open hello project modules settings by using hello context menu or by using hello Ctrl+Alt+Maj+S shortcut.</span></span>
   
    ![Nastavení projektu moduly](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4.PNG)
4. <span data-ttu-id="c4def-141">Modul aplikace hello výchozí odeberte, protože chcete nastavení kontejneru projektu hello.</span><span class="sxs-lookup"><span data-stu-id="c4def-141">Remove hello default app module because you only want hello project container settings.</span></span>
   
    ![modul aplikace výchozí Hello](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample5.PNG)
5. <span data-ttu-id="c4def-143">Naimportovat moduly z hello klonovaný úložišti toohello aktuálního projektu.</span><span class="sxs-lookup"><span data-stu-id="c4def-143">Import modules from hello cloned repo toohello current project.</span></span>
   
    <span data-ttu-id="c4def-144">![Import projektu gradle](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG) ![vytvořit novou stránku modulu](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)</span><span class="sxs-lookup"><span data-stu-id="c4def-144">![Import gradle project](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG) ![Create new module page](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)</span></span>
6. <span data-ttu-id="c4def-145">Opakujte tyto kroky u hello `oidlib-sample` modulu.</span><span class="sxs-lookup"><span data-stu-id="c4def-145">Repeat these steps for hello `oidlib-sample` module.</span></span>
7. <span data-ttu-id="c4def-146">Zkontrolujte hello oidclib závislostí na hello `oidlib-sample` modulu.</span><span class="sxs-lookup"><span data-stu-id="c4def-146">Check hello oidclib dependencies on hello `oidlib-sample` module.</span></span>
   
    ![oidclib závislosti na modulu hello oidlib – ukázka](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8.PNG)
8. <span data-ttu-id="c4def-148">Klikněte na tlačítko **OK** a počkejte, než pro synchronizaci gradle.</span><span class="sxs-lookup"><span data-stu-id="c4def-148">Click **OK** and wait for gradle sync.</span></span>
   
    <span data-ttu-id="c4def-149">Vaše settings.gradle by měl vypadat podobně jako:</span><span class="sxs-lookup"><span data-stu-id="c4def-149">Your settings.gradle should look like:</span></span>
   
    ![Snímek obrazovky settings.gradle](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8_1.PNG)
9. <span data-ttu-id="c4def-151">Sestavení hello ukázkové aplikace toomake se, že hello ukázka funguje správně.</span><span class="sxs-lookup"><span data-stu-id="c4def-151">Build hello sample app toomake sure that hello sample running correctly.</span></span>
   
    <span data-ttu-id="c4def-152">Můžete nebudou moct toouse to s Azure Active Directory ještě.</span><span class="sxs-lookup"><span data-stu-id="c4def-152">You won't be able toouse this with Azure Active Directory yet.</span></span> <span data-ttu-id="c4def-153">Budeme potřebovat tooconfigure některé koncové body nejdřív.</span><span class="sxs-lookup"><span data-stu-id="c4def-153">We'll need tooconfigure some endpoints first.</span></span> <span data-ttu-id="c4def-154">Toto je tooensure nemáte oprávnění ke Android Studio než začneme přizpůsobení hello ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c4def-154">This is tooensure you don't have an Android Studio issues before we start customizing hello sample app.</span></span>
10. <span data-ttu-id="c4def-155">Sestavení a spuštění `oidlib-sample` jako cíl hello v Android Studio.</span><span class="sxs-lookup"><span data-stu-id="c4def-155">Build and run `oidlib-sample` as hello target in Android Studio.</span></span>
    
    ![Průběh sestavení oidlib – ukázka](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample9.png)
11. <span data-ttu-id="c4def-157">Odstranit hello `app ` adresáře, které bylo při odebrání hello modul z projektu hello protože Android Studio neodstraní, je pro zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="c4def-157">Delete hello `app ` directory that was left when you removed hello module from hello project because Android Studio doesn't delete it for safety.</span></span>
    
    ![Struktura souborů, která obsahuje adresář aplikace hello](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample12.PNG)
12. <span data-ttu-id="c4def-159">Otevřete hello **upravit konfigurace** nabídky tooremove hello spustit konfigurace, které bylo také při odebrání hello modul z projektu hello.</span><span class="sxs-lookup"><span data-stu-id="c4def-159">Open hello **Edit Configurations** menu tooremove hello run configuration that was also left when you removed hello module from hello project.</span></span>
    
    <span data-ttu-id="c4def-160">![Upravit nabídku konfigurace](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
    ![spustit konfigurace aplikace](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)</span><span class="sxs-lookup"><span data-stu-id="c4def-160">![Edit configurations menu](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
![Run configuration of app](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)</span></span>

## <a name="configure-hello-endpoints-of-hello-sample"></a><span data-ttu-id="c4def-161">Nakonfigurovat koncové body hello hello vzorku</span><span class="sxs-lookup"><span data-stu-id="c4def-161">Configure hello endpoints of hello sample</span></span>
<span data-ttu-id="c4def-162">Teď, když máte hello `oidlib-sample` spuštěna úspěšně, tady upravit některé koncové body tooget tato práce s Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c4def-162">Now that you have hello `oidlib-sample` running successfully, let's edit some endpoints tooget this working with Azure Active Directory.</span></span>

### <a name="configure-your-client-by-editing-hello-oidcclientconfxml-file"></a><span data-ttu-id="c4def-163">Nakonfigurujte klienta tak, že upravíte soubor oidc_clientconf.xml hello</span><span class="sxs-lookup"><span data-stu-id="c4def-163">Configure your client by editing hello oidc_clientconf.xml file</span></span>
1. <span data-ttu-id="c4def-164">Vzhledem k tomu, že používáte OAuth2 toky pouze tooget token a volání hello rozhraní Graph API, nastavte hello toodo klienta OAuth2 pouze.</span><span class="sxs-lookup"><span data-stu-id="c4def-164">Because you are using OAuth2 flows only tooget a token and call hello Graph API, set hello client toodo OAuth2 only.</span></span> <span data-ttu-id="c4def-165">OIDC vrátí se v pozdější příkladu.</span><span class="sxs-lookup"><span data-stu-id="c4def-165">OIDC will come in a later example.</span></span>
   
    ```xml
        <bool name="oidc_oauth2only">true</bool>
    ```
2. <span data-ttu-id="c4def-166">Nakonfigurujte vaše ID klienta, který jste obdrželi z portálu pro registraci hello.</span><span class="sxs-lookup"><span data-stu-id="c4def-166">Configure your client ID that you received from hello registration portal.</span></span>
   
    ```xml
        <string name="oidc_clientId">86172f9d-a1ae-4348-aafa-7b3e5d1b36f5</string>
        <string name="oidc_clientSecret"></string>
    ```
3. <span data-ttu-id="c4def-167">Nakonfigurujte váš identifikátor URI přesměrování s hello jeden níže.</span><span class="sxs-lookup"><span data-stu-id="c4def-167">Configure your redirect URI with hello one below.</span></span>
   
    ```xml
        <string name="oidc_redirectUrl">https://login.microsoftonline.com/common/oauth2/nativeclient</string>
    ```
4. <span data-ttu-id="c4def-168">Konfigurace vaší oborů, že budete potřebovat v pořadí tooaccess hello rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="c4def-168">Configure your scopes that you need in order tooaccess hello Graph API.</span></span>
   
    ```xml
        <string-array name="oidc_scopes">
            <item>openid</item>
            <item>https://graph.microsoft.com/User.Read</item>
            <item>offline_access</item>
        </string-array>
    ```

<span data-ttu-id="c4def-169">Hello `User.Read` hodnotu `oidc_scopes` umožňuje vám tooread hello základní profil hello přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="c4def-169">hello `User.Read` value in `oidc_scopes` allows you tooread hello basic profile hello signed in user.</span></span>
<span data-ttu-id="c4def-170">Další informace o všech dostupných oborů hello v [obory oprávnění Microsoft Graph](https://graph.microsoft.io/docs/authorization/permission_scopes).</span><span class="sxs-lookup"><span data-stu-id="c4def-170">You can learn more about all hello available scopes at [Microsoft Graph permission scopes](https://graph.microsoft.io/docs/authorization/permission_scopes).</span></span>

<span data-ttu-id="c4def-171">Pokud chcete o vysvětlení `openid` nebo `offline_access` jako obory v OpenID Connect, najdete v části [2.0 protokoly - toku OAuth 2.0 autorizační kód](active-directory-v2-protocols-oauth-code.md).</span><span class="sxs-lookup"><span data-stu-id="c4def-171">If you'd like explanations about `openid` or `offline_access` as scopes in OpenID Connect, see [2.0 Protocols - OAuth 2.0 Authorization Code Flow](active-directory-v2-protocols-oauth-code.md).</span></span>

### <a name="configure-your-client-endpoints-by-editing-hello-oidcendpointsxml-file"></a><span data-ttu-id="c4def-172">Nakonfigurovat koncové body klienta úpravou souboru oidc_endpoints.xml hello</span><span class="sxs-lookup"><span data-stu-id="c4def-172">Configure your client endpoints by editing hello oidc_endpoints.xml file</span></span>
* <span data-ttu-id="c4def-173">Otevřete hello `oidc_endpoints.xml` souboru a nastavit hello následující změny:</span><span class="sxs-lookup"><span data-stu-id="c4def-173">Open hello `oidc_endpoints.xml` file and make hello following changes:</span></span>
  
    ```xml
    <!-- Stores OpenID Connect provider endpoints. -->
    <resources>
        <string name="op_authorizationEnpoint">https://login.microsoftonline.com/common/oauth2/v2.0/authorize</string>
        <string name="op_tokenEndpoint">https://login.microsoftonline.com/common/oauth2/v2.0/token</string>
        <string name="op_userInfoEndpoint">https://www.example.com/oauth2/userinfo</string>
        <string name="op_revocationEndpoint">https://www.example.com/oauth2/revoketoken</string>
    </resources>
    ```

<span data-ttu-id="c4def-174">Tyto koncové body nikdy změnit v případě, že používáte OAuth2 jako protokol.</span><span class="sxs-lookup"><span data-stu-id="c4def-174">These endpoints should never change if you are using OAuth2 as your protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="c4def-175">Hello koncové body pro `userInfoEndpoint` a `revocationEndpoint` nejsou aktuálně podporovány službou Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c4def-175">hello endpoints for `userInfoEndpoint` and `revocationEndpoint` are currently not supported by Azure Active Directory.</span></span> <span data-ttu-id="c4def-176">Pokud tyto výchozí example.com hodnotou hello, budete upozorněni, že nejsou k dispozici v ukázce hello :-)</span><span class="sxs-lookup"><span data-stu-id="c4def-176">If you leave these with hello default example.com value, you will be reminded that they are not available in hello sample :-)</span></span>
> 
> 

## <a name="configure-a-graph-api-call"></a><span data-ttu-id="c4def-177">Konfigurace volání rozhraní Graph API</span><span class="sxs-lookup"><span data-stu-id="c4def-177">Configure a Graph API call</span></span>
* <span data-ttu-id="c4def-178">Otevřete hello `HomeActivity.java` souboru a nastavit hello následující změny:</span><span class="sxs-lookup"><span data-stu-id="c4def-178">Open hello `HomeActivity.java` file and make hello following changes:</span></span>
  
    ```Java
       //TODO: set your protected resource url
        private static final String protectedResUrl = "https://graph.microsoft.com/v1.0/me/";
    ```

<span data-ttu-id="c4def-179">Zde jednoduché volání rozhraní Graph API vrací naše informace.</span><span class="sxs-lookup"><span data-stu-id="c4def-179">Here a simple Graph API call returns our information.</span></span>

<span data-ttu-id="c4def-180">Ty jsou všechny změny hello, je nutné, aby toodo.</span><span class="sxs-lookup"><span data-stu-id="c4def-180">Those are all hello changes that you need toodo.</span></span> <span data-ttu-id="c4def-181">Spustit hello `oidlib-sample` aplikace a klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="c4def-181">Run hello `oidlib-sample` application, and click **Sign in**.</span></span>

<span data-ttu-id="c4def-182">Když jste úspěšně ověřen, vyberte hello **požadavku chráněných prostředků** tlačítko tootest toohello vaše volání rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="c4def-182">After you've successfully authenticated, select hello **Request Protected Resource** button tootest your call toohello Graph API.</span></span>

## <a name="get-security-updates-for-our-product"></a><span data-ttu-id="c4def-183">Získejte bezpečnostní aktualizace našich produktů</span><span class="sxs-lookup"><span data-stu-id="c4def-183">Get security updates for our product</span></span>
<span data-ttu-id="c4def-184">Doporučujeme vám tooget oznámení o bezpečnostní incidenty v oblasti návštěvou hello [Web Security TechCenter](https://technet.microsoft.com/security/dd252948) a přihlášení k odběru tooSecurity Advisory Alerts.</span><span class="sxs-lookup"><span data-stu-id="c4def-184">We encourage you tooget notifications about security incidents by visiting hello [Security TechCenter](https://technet.microsoft.com/security/dd252948) and subscribing tooSecurity Advisory Alerts.</span></span>

