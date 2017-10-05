---
title: Azure Active Directory v2.0 aplikace pro Android | Microsoft Docs
description: "Postup vytvoření aplikace pro Android s přihlašováním uživatelů s osobní účet Microsoft a pracovní nebo školní účty a volání rozhraní Graph API pomocí knihoven jiných dodavatelů."
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
ms.openlocfilehash: c0a5a818c61f7af7ff04bf890b54e8364f3b21b1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-sign-in-to-an-android-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a><span data-ttu-id="d7ca1-103">Přidání přihlášení do aplikace pro Android pomocí rozhraní Graph API pomocí koncového bodu v2.0 knihovnu třetích stran</span><span class="sxs-lookup"><span data-stu-id="d7ca1-103">Add sign-in to an Android app using a third-party library with Graph API using the v2.0 endpoint</span></span>
<span data-ttu-id="d7ca1-104">Platforma Microsoft identity používá otevřené standardy, jako je například OAuth2 nebo OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-104">The Microsoft identity platform uses open standards such as OAuth2 and OpenID Connect.</span></span> <span data-ttu-id="d7ca1-105">Vývojáři mohou použít žádné knihovny, které chtějí integrovat našich služeb.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-105">Developers can use any library they want to integrate with our services.</span></span> <span data-ttu-id="d7ca1-106">Což vývojářům používat naše platforma s další knihovny, jsme jste zapisují několik návody podobné následujícímu abychom ukázali, jak nakonfigurovat třetích stran knihovny pro připojení k identitu platformy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-106">To help developers use our platform with other libraries, we've written a few walkthroughs like this one to demonstrate how to configure third-party libraries to connect to the Microsoft identity platform.</span></span> <span data-ttu-id="d7ca1-107">Většina knihovny, které implementují [specifikace RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) se může připojit k Microsoft identity platform.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-107">Most libraries that implement [the RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) can connect to the Microsoft identity platform.</span></span>

<span data-ttu-id="d7ca1-108">Aplikaci, která vytváří Tento názorný postup mohou uživatelé přihlásit k jejich organizace a poté vyhledejte sami ve své organizaci pomocí rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-108">With the application that this walkthrough creates, users can sign in to their organization and then search for themselves in their organization by using the Graph API.</span></span>

<span data-ttu-id="d7ca1-109">Pokud jste ještě OAuth2 nebo OpenID Connect, mnohem této ukázkové konfigurace nemusí mít smysl pro vás.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-109">If you're new to OAuth2 or OpenID Connect, much of this sample configuration may not make sense to you.</span></span> <span data-ttu-id="d7ca1-110">Doporučujeme, abyste si přečetli [2.0 protokoly - toku OAuth 2.0 autorizační kód](active-directory-v2-protocols-oauth-code.md) pozadí.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-110">We recommend that you read [2.0 Protocols - OAuth 2.0 Authorization Code Flow](active-directory-v2-protocols-oauth-code.md) for background.</span></span>

> [!NOTE]
> <span data-ttu-id="d7ca1-111">Některé funkce naše platformy, které mají výrazu v OAuth2 nebo OpenID Connect standardy, jako je například podmíněný přístup a správu zásad Intune, musíte používat naše otevřený zdroj knihovny Identity Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-111">Some features of our platform that do have an expression in the OAuth2 or OpenID Connect standards, such as Conditional Access and Intune policy management, require you to use our open source Microsoft Azure Identity Libraries.</span></span>
> 
> 

<span data-ttu-id="d7ca1-112">Koncový bod v2.0 nepodporuje všechny scénáře Azure Active Directory a funkce.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-112">The v2.0 endpoint does not support all Azure Active Directory scenarios and features.</span></span>

> [!NOTE]
> <span data-ttu-id="d7ca1-113">Pokud chcete zjistit, pokud byste měli používat koncový bod v2.0, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="d7ca1-113">To determine if you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="download-the-code-from-github"></a><span data-ttu-id="d7ca1-114">Stáhněte si kód z Githubu</span><span class="sxs-lookup"><span data-stu-id="d7ca1-114">Download the code from GitHub</span></span>
<span data-ttu-id="d7ca1-115">Kód k tomuto kurzu je udržovaný [na GitHubu](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2).</span><span class="sxs-lookup"><span data-stu-id="d7ca1-115">The code for this tutorial is maintained [on GitHub](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2).</span></span>  <span data-ttu-id="d7ca1-116">Chcete-li sledovat, můžete [stáhnout kostru aplikace jako ZIP](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) nebo tuto kostru klonovat:</span><span class="sxs-lookup"><span data-stu-id="d7ca1-116">To follow along, you can  [download the app's skeleton as a .zip](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) or clone the skeleton:</span></span>

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

<span data-ttu-id="d7ca1-117">Ukázku můžete také právě stáhnout a začít hned:</span><span class="sxs-lookup"><span data-stu-id="d7ca1-117">You can also just download the sample and get started right away:</span></span>

```
git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

## <a name="register-an-app"></a><span data-ttu-id="d7ca1-118">Registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="d7ca1-118">Register an app</span></span>
<span data-ttu-id="d7ca1-119">Vytvoření nové aplikace v [portálu pro registraci aplikace](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo použijte podrobný postup v [postup registrace aplikace s koncovým bodem v2.0](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="d7ca1-119">Create a new app at the [Application registration portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow the detailed steps at [How to register an app with the v2.0 endpoint](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="d7ca1-120">Zkontrolujte, že:</span><span class="sxs-lookup"><span data-stu-id="d7ca1-120">Make sure to:</span></span>

* <span data-ttu-id="d7ca1-121">Kopírování **Id aplikace** , je přiřazen do vaší aplikace, protože ho budete potřebovat brzy.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-121">Copy the **Application Id** that's assigned to your app because you'll need it soon.</span></span>
* <span data-ttu-id="d7ca1-122">Přidat **Mobile** platformu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-122">Add the **Mobile** platform for your app.</span></span>

> <span data-ttu-id="d7ca1-123">Poznámka: Poskytuje portálu pro registraci aplikace **identifikátor URI pro přesměrování** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-123">Note: The Application registration portal provides a **Redirect URI** value.</span></span> <span data-ttu-id="d7ca1-124">Ale v tomto příkladu musí používat výchozí hodnotu `https://login.microsoftonline.com/common/oauth2/nativeclient`.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-124">However, in this example you must use the default value of `https://login.microsoftonline.com/common/oauth2/nativeclient`.</span></span>
> 
> 

## <a name="download-the-nxoauth2-third-party-library-and-create-a-workspace"></a><span data-ttu-id="d7ca1-125">Stažení knihovně NXOAuth2 třetích stran a vytvořit pracovní prostor</span><span class="sxs-lookup"><span data-stu-id="d7ca1-125">Download the NXOAuth2 third-party library and create a workspace</span></span>
<span data-ttu-id="d7ca1-126">V tomto návodu budete používat OIDCAndroidLib z Githubu, která je OAuth2 knihovny založené na kódu Google OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-126">For this walkthrough, you will use the OIDCAndroidLib from GitHub, which is an OAuth2 library based on the OpenID Connect code of Google.</span></span> <span data-ttu-id="d7ca1-127">Profil nativní aplikace implementuje a podporuje koncový bod autorizace uživatele.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-127">It implements the native application profile and supports the authorization endpoint of the user.</span></span> <span data-ttu-id="d7ca1-128">Toto jsou všechny věci, které budete potřebovat k integraci s platformou identity Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-128">These are all the things that you'll need to integrate with the Microsoft identity platform.</span></span>

<span data-ttu-id="d7ca1-129">Naklonujte úložiště OIDCAndroidLib do vašeho počítače.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-129">Clone the OIDCAndroidLib repo to your computer.</span></span>

```
git@github.com:kalemontes/OIDCAndroidLib.git
```

![androidStudio](../media/active-directory-android-native-oidcandroidlib-v2/emotes-url.png)

## <a name="set-up-your-android-studio-environment"></a><span data-ttu-id="d7ca1-131">Nastavení prostředí Android Studio</span><span class="sxs-lookup"><span data-stu-id="d7ca1-131">Set up your Android Studio environment</span></span>
1. <span data-ttu-id="d7ca1-132">Vytvořte nový projekt Android Studio a přijměte výchozí nastavení v průvodci.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-132">Create a new Android Studio project and accept the defaults in the wizard.</span></span>
   
    ![Vytvořit nový projekt v Android Studio](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample1.PNG)
   
    ![Cílové zařízení se systémem Android](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample2.PNG)
   
    ![Přidat aktivitu do mobile](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample3.PNG)
2. <span data-ttu-id="d7ca1-136">Chcete-li nastavit moduly projektu, přesunete do umístění projektu klonovaný úložišti.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-136">To set up your project modules, move the cloned repo to the project location.</span></span> <span data-ttu-id="d7ca1-137">Můžete také vytvořit projekt a poté klonovat přímo do umístění projektu.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-137">You can also create the project and then clone it directly to the project location.</span></span>
   
    ![Moduly projektu](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4_1.PNG)
3. <span data-ttu-id="d7ca1-139">Otevřete nastavení projektu moduly pomocí místní nabídky nebo klávesové zkratky Ctrl + Alt + avní + S.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-139">Open the project modules settings by using the context menu or by using the Ctrl+Alt+Maj+S shortcut.</span></span>
   
    ![Nastavení projektu moduly](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4.PNG)
4. <span data-ttu-id="d7ca1-141">Výchozí modul aplikace odeberte, protože chcete kontejneru nastavení projektu.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-141">Remove the default app module because you only want the project container settings.</span></span>
   
    ![Výchozí modul aplikace](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample5.PNG)
5. <span data-ttu-id="d7ca1-143">Importujte moduly z klonovaného úložiště do aktuálního projektu.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-143">Import modules from the cloned repo to the current project.</span></span>
   
    <span data-ttu-id="d7ca1-144">![Import projektu gradle](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG) ![vytvořit novou stránku modulu](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)</span><span class="sxs-lookup"><span data-stu-id="d7ca1-144">![Import gradle project](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG) ![Create new module page](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)</span></span>
6. <span data-ttu-id="d7ca1-145">Opakujte tyto kroky `oidlib-sample` modulu.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-145">Repeat these steps for the `oidlib-sample` module.</span></span>
7. <span data-ttu-id="d7ca1-146">Zkontrolovat závislosti oidclib `oidlib-sample` modulu.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-146">Check the oidclib dependencies on the `oidlib-sample` module.</span></span>
   
    ![oidclib závislosti na modulu oidlib – ukázka](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8.PNG)
8. <span data-ttu-id="d7ca1-148">Klikněte na tlačítko **OK** a počkejte, než pro synchronizaci gradle.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-148">Click **OK** and wait for gradle sync.</span></span>
   
    <span data-ttu-id="d7ca1-149">Vaše settings.gradle by měl vypadat podobně jako:</span><span class="sxs-lookup"><span data-stu-id="d7ca1-149">Your settings.gradle should look like:</span></span>
   
    ![Snímek obrazovky settings.gradle](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8_1.PNG)
9. <span data-ttu-id="d7ca1-151">Vytvoření ukázkové aplikace k Ujistěte se, že ukázkový, funguje správně.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-151">Build the sample app to make sure that the sample running correctly.</span></span>
   
    <span data-ttu-id="d7ca1-152">Nebudete moci ještě použít s Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-152">You won't be able to use this with Azure Active Directory yet.</span></span> <span data-ttu-id="d7ca1-153">Budeme potřebovat nejprve nakonfigurovat některé koncové body.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-153">We'll need to configure some endpoints first.</span></span> <span data-ttu-id="d7ca1-154">To je potřeba zajistit, že nemáte oprávnění ke Android Studio než začneme přizpůsobení ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-154">This is to ensure you don't have an Android Studio issues before we start customizing the sample app.</span></span>
10. <span data-ttu-id="d7ca1-155">Sestavení a spuštění `oidlib-sample` jako cíl v Android Studio.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-155">Build and run `oidlib-sample` as the target in Android Studio.</span></span>
    
    ![Průběh sestavení oidlib – ukázka](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample9.png)
11. <span data-ttu-id="d7ca1-157">Odstranit `app ` adresáře, které bylo při odebrat modul z projektu, protože Android Studio neodstraní, je pro zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-157">Delete the `app ` directory that was left when you removed the module from the project because Android Studio doesn't delete it for safety.</span></span>
    
    ![Struktura souborů, která obsahuje adresář aplikace](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample12.PNG)
12. <span data-ttu-id="d7ca1-159">Otevřete **upravit konfigurace** nabídky k odebrání spuštění konfigurace, které bylo také při odebrání modul z projektu.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-159">Open the **Edit Configurations** menu to remove the run configuration that was also left when you removed the module from the project.</span></span>
    
    <span data-ttu-id="d7ca1-160">![Upravit nabídku konfigurace](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
    ![spustit konfigurace aplikace](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)</span><span class="sxs-lookup"><span data-stu-id="d7ca1-160">![Edit configurations menu](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
![Run configuration of app](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)</span></span>

## <a name="configure-the-endpoints-of-the-sample"></a><span data-ttu-id="d7ca1-161">Nakonfigurovat koncové body vzorku</span><span class="sxs-lookup"><span data-stu-id="d7ca1-161">Configure the endpoints of the sample</span></span>
<span data-ttu-id="d7ca1-162">Teď, když máte `oidlib-sample` spuštěna úspěšně, Pojďme upravit některé koncové body k získání tato práce s Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-162">Now that you have the `oidlib-sample` running successfully, let's edit some endpoints to get this working with Azure Active Directory.</span></span>

### <a name="configure-your-client-by-editing-the-oidcclientconfxml-file"></a><span data-ttu-id="d7ca1-163">Nakonfigurujte klienta tak, že upravíte soubor oidc_clientconf.xml</span><span class="sxs-lookup"><span data-stu-id="d7ca1-163">Configure your client by editing the oidc_clientconf.xml file</span></span>
1. <span data-ttu-id="d7ca1-164">Vzhledem k tomu, že používáte OAuth2 toky pouze k získání tokenu a zavolat rozhraní Graph API, nastavení klienta jenom udělat OAuth2.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-164">Because you are using OAuth2 flows only to get a token and call the Graph API, set the client to do OAuth2 only.</span></span> <span data-ttu-id="d7ca1-165">OIDC vrátí se v pozdější příkladu.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-165">OIDC will come in a later example.</span></span>
   
    ```xml
        <bool name="oidc_oauth2only">true</bool>
    ```
2. <span data-ttu-id="d7ca1-166">Nakonfigurujte vaše ID klienta, který jste obdrželi z portálu pro registraci.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-166">Configure your client ID that you received from the registration portal.</span></span>
   
    ```xml
        <string name="oidc_clientId">86172f9d-a1ae-4348-aafa-7b3e5d1b36f5</string>
        <string name="oidc_clientSecret"></string>
    ```
3. <span data-ttu-id="d7ca1-167">Nakonfigurujte váš identifikátor URI pro přesměrování jedna níže.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-167">Configure your redirect URI with the one below.</span></span>
   
    ```xml
        <string name="oidc_redirectUrl">https://login.microsoftonline.com/common/oauth2/nativeclient</string>
    ```
4. <span data-ttu-id="d7ca1-168">Konfigurace vaší obory, které potřebujete k přístup k rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-168">Configure your scopes that you need in order to access the Graph API.</span></span>
   
    ```xml
        <string-array name="oidc_scopes">
            <item>openid</item>
            <item>https://graph.microsoft.com/User.Read</item>
            <item>offline_access</item>
        </string-array>
    ```

<span data-ttu-id="d7ca1-169">`User.Read` Hodnotu `oidc_scopes` umožňuje číst základní profil podepsané v uživatele.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-169">The `User.Read` value in `oidc_scopes` allows you to read the basic profile the signed in user.</span></span>
<span data-ttu-id="d7ca1-170">Další informace o všechny obory, které jsou k dispozici na [obory oprávnění Microsoft Graph](https://graph.microsoft.io/docs/authorization/permission_scopes).</span><span class="sxs-lookup"><span data-stu-id="d7ca1-170">You can learn more about all the available scopes at [Microsoft Graph permission scopes](https://graph.microsoft.io/docs/authorization/permission_scopes).</span></span>

<span data-ttu-id="d7ca1-171">Pokud chcete o vysvětlení `openid` nebo `offline_access` jako obory v OpenID Connect, najdete v části [2.0 protokoly - toku OAuth 2.0 autorizační kód](active-directory-v2-protocols-oauth-code.md).</span><span class="sxs-lookup"><span data-stu-id="d7ca1-171">If you'd like explanations about `openid` or `offline_access` as scopes in OpenID Connect, see [2.0 Protocols - OAuth 2.0 Authorization Code Flow](active-directory-v2-protocols-oauth-code.md).</span></span>

### <a name="configure-your-client-endpoints-by-editing-the-oidcendpointsxml-file"></a><span data-ttu-id="d7ca1-172">Nakonfigurovat koncové body klienta úpravou souboru oidc_endpoints.xml</span><span class="sxs-lookup"><span data-stu-id="d7ca1-172">Configure your client endpoints by editing the oidc_endpoints.xml file</span></span>
* <span data-ttu-id="d7ca1-173">Otevřete `oidc_endpoints.xml` souboru a proveďte následující změny:</span><span class="sxs-lookup"><span data-stu-id="d7ca1-173">Open the `oidc_endpoints.xml` file and make the following changes:</span></span>
  
    ```xml
    <!-- Stores OpenID Connect provider endpoints. -->
    <resources>
        <string name="op_authorizationEnpoint">https://login.microsoftonline.com/common/oauth2/v2.0/authorize</string>
        <string name="op_tokenEndpoint">https://login.microsoftonline.com/common/oauth2/v2.0/token</string>
        <string name="op_userInfoEndpoint">https://www.example.com/oauth2/userinfo</string>
        <string name="op_revocationEndpoint">https://www.example.com/oauth2/revoketoken</string>
    </resources>
    ```

<span data-ttu-id="d7ca1-174">Tyto koncové body nikdy změnit v případě, že používáte OAuth2 jako protokol.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-174">These endpoints should never change if you are using OAuth2 as your protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="d7ca1-175">Koncové body pro `userInfoEndpoint` a `revocationEndpoint` nejsou aktuálně podporovány službou Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-175">The endpoints for `userInfoEndpoint` and `revocationEndpoint` are currently not supported by Azure Active Directory.</span></span> <span data-ttu-id="d7ca1-176">Pokud tyto výchozí hodnotou example.com, budete upozorněni, že nejsou k dispozici ve vzorku :-)</span><span class="sxs-lookup"><span data-stu-id="d7ca1-176">If you leave these with the default example.com value, you will be reminded that they are not available in the sample :-)</span></span>
> 
> 

## <a name="configure-a-graph-api-call"></a><span data-ttu-id="d7ca1-177">Konfigurace volání rozhraní Graph API</span><span class="sxs-lookup"><span data-stu-id="d7ca1-177">Configure a Graph API call</span></span>
* <span data-ttu-id="d7ca1-178">Otevřete `HomeActivity.java` souboru a proveďte následující změny:</span><span class="sxs-lookup"><span data-stu-id="d7ca1-178">Open the `HomeActivity.java` file and make the following changes:</span></span>
  
    ```Java
       //TODO: set your protected resource url
        private static final String protectedResUrl = "https://graph.microsoft.com/v1.0/me/";
    ```

<span data-ttu-id="d7ca1-179">Zde jednoduché volání rozhraní Graph API vrací naše informace.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-179">Here a simple Graph API call returns our information.</span></span>

<span data-ttu-id="d7ca1-180">Ty jsou všechny změny, které musíte udělat.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-180">Those are all the changes that you need to do.</span></span> <span data-ttu-id="d7ca1-181">Spustit `oidlib-sample` aplikace a klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-181">Run the `oidlib-sample` application, and click **Sign in**.</span></span>

<span data-ttu-id="d7ca1-182">Když jste úspěšně ověřen, vyberte **požadavku chráněných prostředků** tlačítko můžete otestovat volání rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-182">After you've successfully authenticated, select the **Request Protected Resource** button to test your call to the Graph API.</span></span>

## <a name="get-security-updates-for-our-product"></a><span data-ttu-id="d7ca1-183">Získejte bezpečnostní aktualizace našich produktů</span><span class="sxs-lookup"><span data-stu-id="d7ca1-183">Get security updates for our product</span></span>
<span data-ttu-id="d7ca1-184">Doporučujeme vám získávat oznámení o bezpečnostní incidenty v oblasti navštivte stránky [Web Security TechCenter](https://technet.microsoft.com/security/dd252948) a odběr Security Advisory Alerts.</span><span class="sxs-lookup"><span data-stu-id="d7ca1-184">We encourage you to get notifications about security incidents by visiting the [Security TechCenter](https://technet.microsoft.com/security/dd252948) and subscribing to Security Advisory Alerts.</span></span>

