---
title: "aaaGet Začínáme s Azure Mobile Engagementem pro Cordovu/Phonegap"
description: "Zjistěte, jak toouse Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro aplikace Cordova/Phonegap."
services: mobile-engagement
documentationcenter: Mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 54fe9113-e239-4ed7-9fd1-a502d7ac7f47
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-phonegap
ms.devlang: js
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: e67dabbdf7886802bb058f38964e558d5ae6854c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-cordovaphonegap"></a><span data-ttu-id="82438-103">Začínáme s Azure Mobile Engagementem pro Cordovu/Phonegap</span><span class="sxs-lookup"><span data-stu-id="82438-103">Get Started with Azure Mobile Engagement for Cordova/Phonegap</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="82438-104">Toto téma ukazuje, jak toounderstand Azure Mobile Engagement toouse aplikace využití a odesílání nabízených oznámení toosegmented uživatele pro mobilní aplikace vyvinuté pomocí Cordovy.</span><span class="sxs-lookup"><span data-stu-id="82438-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and send push notifications toosegmented users for a mobile application developed with Cordova.</span></span>

<span data-ttu-id="82438-105">V tomto kurzu si pomocí Macu vytvoříme prázdnou aplikaci Cordova a pak do ní integrujeme sadu SDK Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="82438-105">In this tutorial, we will create a blank Cordova app using Mac and then integrate Mobile Engagement SDK.</span></span> <span data-ttu-id="82438-106">Ta shromažďuje základní analytická data a přijímá nabízená oznámení přes APNS ( Apple Push Notification System) pro iOS nebo GCM (Google Cloud Messaging) pro Android.</span><span class="sxs-lookup"><span data-stu-id="82438-106">It collects basic analytics data and receives push notifications using Apple Push Notification System (APNS) for iOS and Google Cloud Messaging (GCM) for Android.</span></span> <span data-ttu-id="82438-107">Nasadíme tuto tooan zařízení iOS nebo Android pro testování.</span><span class="sxs-lookup"><span data-stu-id="82438-107">We will deploy this tooan iOS or Android device for testing.</span></span> 

> [!NOTE]
> <span data-ttu-id="82438-108">toocomplete tento kurz, musíte mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="82438-108">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="82438-109">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="82438-109">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="82438-110">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-cordova-get-started).</span><span class="sxs-lookup"><span data-stu-id="82438-110">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-cordova-get-started).</span></span>
> 
> 

<span data-ttu-id="82438-111">Tento kurz vyžaduje hello následující:</span><span class="sxs-lookup"><span data-stu-id="82438-111">This tutorial requires hello following:</span></span>

* <span data-ttu-id="82438-112">XCode, který můžete nainstalovat z Mac App Storu (pro nasazení tooiOS)</span><span class="sxs-lookup"><span data-stu-id="82438-112">XCode, which you can install from Mac App Store (for deploying tooiOS)</span></span>
* <span data-ttu-id="82438-113">[Android SDK a emulátor](http://developer.android.com/sdk/installing/index.html) (pro nasazení tooAndroid)</span><span class="sxs-lookup"><span data-stu-id="82438-113">[Android SDK & Emulator](http://developer.android.com/sdk/installing/index.html) (for deploying tooAndroid)</span></span>
* <span data-ttu-id="82438-114">Certifikát nabízených oznámení (.p12), který můžete získat ve vývojářském centru Apple, pro APNS</span><span class="sxs-lookup"><span data-stu-id="82438-114">Push notification certificate (.p12) that you can obtain from Apple Dev Center for APNS</span></span>
* <span data-ttu-id="82438-115">Číslo GCM projektu, které můžete získat z Vývojářské konzole Google, pro GCM</span><span class="sxs-lookup"><span data-stu-id="82438-115">GCM Project number that you can obtain from your Google Developer Console for GCM</span></span>
* [<span data-ttu-id="82438-116">Plugin Mobile Engagement pro Cordovu</span><span class="sxs-lookup"><span data-stu-id="82438-116">Mobile Engagement Cordova Plugin</span></span>](https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-engagement)

> [!NOTE]
> <span data-ttu-id="82438-117">Můžete najít hello zdrojového kódu a hello – soubor ReadMe pro cordovu hello na [Githubu](https://github.com/Azure/azure-mobile-engagement-cordova)</span><span class="sxs-lookup"><span data-stu-id="82438-117">You can find hello source code and hello ReadMe for hello Cordova plugin on [GitHub](https://github.com/Azure/azure-mobile-engagement-cordova)</span></span>
> 
> 

## <span data-ttu-id="82438-118"><a id="setup-azme"></a>Nastavení Mobile Engagementu pro aplikaci Cordova</span><span class="sxs-lookup"><span data-stu-id="82438-118"><a id="setup-azme"></a>Setup Mobile Engagement for your Cordova app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="82438-119"><a id="connecting-app"></a>Připojení vaší aplikace toohello Mobile Engagement back-end</span><span class="sxs-lookup"><span data-stu-id="82438-119"><a id="connecting-app"></a>Connecting your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="82438-120">Tento kurzu si představíme "základní integraci", což je minimální hello nastavte požadované toocollect dat a odesílání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="82438-120">This tutorial presents a "basic integration" which is hello minimal set required toocollect data and send a push notification.</span></span> 

<span data-ttu-id="82438-121">Pomocí integrace hello toodemonstrate Cordova vytvoříme základní aplikaci:</span><span class="sxs-lookup"><span data-stu-id="82438-121">We will create a basic app with Cordova toodemonstrate hello integration:</span></span>

### <a name="create-a-new-cordova-project"></a><span data-ttu-id="82438-122">Vytvoření nového projektu Cordova</span><span class="sxs-lookup"><span data-stu-id="82438-122">Create a new Cordova project</span></span>
1. <span data-ttu-id="82438-123">Spusťte *Terminálové* okno na vaše Mac počítače a typ hello následující, která se vytvoří nový projekt Cordova z hello výchozí šablony.</span><span class="sxs-lookup"><span data-stu-id="82438-123">Launch *Terminal* window on your Mac machine and type hello following which will create a new Cordova project from hello default template.</span></span> <span data-ttu-id="82438-124">Ujistěte se, že publikování hello profilu můžete nakonec použití toodeploy vaše aplikace iOS používá "com.mycompany.myapp" jako hello ID aplikace.</span><span class="sxs-lookup"><span data-stu-id="82438-124">Make sure that hello publishing profile you eventually use toodeploy your iOS app is using 'com.mycompany.myapp' as hello App ID.</span></span> 
   
        $ cordova create azme-cordova com.mycompany.myapp
        $ cd azme-cordova
2. <span data-ttu-id="82438-125">Spuštění projektu pro hello následující tooconfigure **iOS** a spustíte ho v simulátoru iOS hello:</span><span class="sxs-lookup"><span data-stu-id="82438-125">Execute hello following tooconfigure your project for **iOS** and run it in hello iOS Simulator:</span></span>
   
        $ cordova platform add ios 
        $ cordova run ios
3. <span data-ttu-id="82438-126">Spuštění projektu pro hello následující tooconfigure **Android** a spustíte ho v emulátoru Android hello.</span><span class="sxs-lookup"><span data-stu-id="82438-126">Execute hello following tooconfigure your project for **Android** and run it in hello Android emulator.</span></span> <span data-ttu-id="82438-127">Ujistěte se, že nastavení emulátoru sady SDK pro Android nastaven cíl jako rozhraní Google API (Google Inc.) s hello CPU / ABI jako Google APIs ARM.</span><span class="sxs-lookup"><span data-stu-id="82438-127">Make sure that your Android SDK Emulator settings have its Target as Google APIs (Google Inc.) with hello CPU / ABI as Google APIs ARM.</span></span>  
   
        $ cordova platform add android
        $ cordova run android
4. <span data-ttu-id="82438-128">Přidejte plugin Cordova Console hello.</span><span class="sxs-lookup"><span data-stu-id="82438-128">Add hello Cordova Console plugin.</span></span> 

    ```
    $ cordova plugin add cordova-plugin-console
    ``` 

### <a name="connect-your-app-toomobile-engagement-backend"></a><span data-ttu-id="82438-129">Připojit vaše tooMobile Engagement back-end aplikace</span><span class="sxs-lookup"><span data-stu-id="82438-129">Connect your app tooMobile Engagement backend</span></span>
1. <span data-ttu-id="82438-130">Nainstalujte plugin Azure Mobile Engagement pro Cordovu hello při současném poskytování hello modulu plug-in hello tooconfigure hodnoty proměnné:</span><span class="sxs-lookup"><span data-stu-id="82438-130">Install hello Azure Mobile Engagement Cordova plugin while providing hello variable values tooconfigure hello plugin:</span></span>
   
        cordova plugin add cordova-plugin-ms-azure-mobile-engagement    
             --variable AZME_IOS_CONNECTION_STRING=<iOS Connection String> 
            --variable AZME_IOS_REACH_ICON=... (icon name WITH extension) 
            --variable AZME_ANDROID_CONNECTION_STRING=<Android Connection String> 
            --variable AZME_ANDROID_REACH_ICON=... (icon name WITHOUT extension)       
            --variable AZME_ANDROID_GOOGLE_PROJECT_NUMBER=... (From your Google Cloud console for sending push notifications) 
            --variable AZME_ACTION_URL =... (URL scheme which triggers hello app for deep linking)
            --variable AZME_ENABLE_NATIVE_LOG=true|false
            --variable AZME_ENABLE_PLUGIN_LOG=true|false

<span data-ttu-id="82438-131">*Ikona Reach pro Android* : musí být název hello hello prostředku bez žádné přípony ani předpony (např:. mynotificationicon), a hello soubor ikony musí být zkopírován do vašeho projektu android (platformy/android/res/drawable)</span><span class="sxs-lookup"><span data-stu-id="82438-131">*Android Reach Icon* : must be hello name of hello resource without any extension, nor drawable prefix (ex: mynotificationicon), and hello icon file must be copied into your android project (platforms/android/res/drawable)</span></span>

<span data-ttu-id="82438-132">*Ikona Reach pro iOS* : musí být název hello hello prostředku včetně přípony (např: mojeikonanotifikace.PNG), a hello soubor ikony musí být přidán do projektu iOS s XCode (přes hello nabídky přidat soubory)</span><span class="sxs-lookup"><span data-stu-id="82438-132">*iOS Reach Icon*  : must be hello name of hello resource with its extension (ex:  mynotificationicon.png), and hello icon file must be added into your iOS project with XCode (using hello Add Files Menu)</span></span>

## <span data-ttu-id="82438-133"><a id="monitor"></a>Povolení sledování v reálném čase</span><span class="sxs-lookup"><span data-stu-id="82438-133"><a id="monitor"></a>Enabling real-time monitoring</span></span>
1. <span data-ttu-id="82438-134">V projektu Cordova hello - upravit **www/js/index.js** volání hello tooadd tooMobile Engagement toodeclare nová aktivita jednou hello *deviceReady* události.</span><span class="sxs-lookup"><span data-stu-id="82438-134">In hello Cordova project - edit **www/js/index.js** tooadd hello call tooMobile Engagement toodeclare a new activity once hello *deviceReady* event is received.</span></span>
   
         onDeviceReady: function() {
                Engagement.startActivity("myPage",{});
            }
2. <span data-ttu-id="82438-135">Spuštění aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="82438-135">Run hello application:</span></span>
   
   * <span data-ttu-id="82438-136">**Pro iOS**</span><span class="sxs-lookup"><span data-stu-id="82438-136">**For iOS**</span></span>
     
       <span data-ttu-id="82438-137">V `Terminal` okno spusťte aplikaci v nové instanci simulátoru spuštěním hello následující:</span><span class="sxs-lookup"><span data-stu-id="82438-137">In `Terminal` window launch your app in a new Simulator instance by executing hello following:</span></span>
     
           cordova run ios
   * <span data-ttu-id="82438-138">**Pro Android**</span><span class="sxs-lookup"><span data-stu-id="82438-138">**For Android**</span></span>
     
       <span data-ttu-id="82438-139">V `Terminal` okno spusťte aplikaci v nové instanci emulátoru spuštěním hello následující:</span><span class="sxs-lookup"><span data-stu-id="82438-139">In `Terminal` window launch your app in a new emulator instance by executing hello following:</span></span>
     
           cordova run android
3. <span data-ttu-id="82438-140">Zobrazí se následující hello v protokolech konzoly hello:</span><span class="sxs-lookup"><span data-stu-id="82438-140">You can see hello following in hello console logs:</span></span>
   
        [Engagement] Agent: Session started
        [Engagement] Agent: Activity 'myPage' started
        [Engagement] Connection: Established
        [Engagement] Connection: Sent: appInfo
        [Engagement] Connection: Sent: startSession
        [Engagement] Connection: Sent: activity name='myPage'

## <span data-ttu-id="82438-141"><a id="monitor"></a>Připojení aplikace se sledováním v reálném čase</span><span class="sxs-lookup"><span data-stu-id="82438-141"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="82438-142"><a id="integrate-push"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci</span><span class="sxs-lookup"><span data-stu-id="82438-142"><a id="integrate-push"></a>Enabling Push Notifications and in-app messaging</span></span>
<span data-ttu-id="82438-143">Mobile Engagement vám umožní toointeract s uživateli přes nabízená oznámení a zasílání zpráv v kontextu hello kampaní v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="82438-143">Mobile Engagement allows you toointeract with your users using Push Notifications and in-app messaging in hello context of campaigns.</span></span> <span data-ttu-id="82438-144">Tento modul je hello portálu Mobile Engagement nazývá REACH.</span><span class="sxs-lookup"><span data-stu-id="82438-144">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="82438-145">Hello následujících sekcích nastavíte tooreceive vaše aplikace je.</span><span class="sxs-lookup"><span data-stu-id="82438-145">hello following sections will setup your app tooreceive them.</span></span>

### <a name="configure-push-credentials-for-mobile-engagement"></a><span data-ttu-id="82438-146">Konfigurace přihlašovacích údajů nabízených oznámení pro Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="82438-146">Configure Push credentials for Mobile Engagement</span></span>
<span data-ttu-id="82438-147">tooallow Mobile Engagement toosend nabízená oznámení vaším jménem, je nutné toogrant, že přístup tooyour certifikátu IOS(Apple) nebo klíči rozhraní API serveru GCM.</span><span class="sxs-lookup"><span data-stu-id="82438-147">tooallow Mobile Engagement toosend Push Notifications on your behalf, you need toogrant it access tooyour Apple iOS certificate or GCM Server API Key.</span></span> 

1. <span data-ttu-id="82438-148">Přejděte tooyour portál Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="82438-148">Navigate tooyour Mobile Engagement portal.</span></span> <span data-ttu-id="82438-149">Zkontrolujte, zda pracujete v aplikaci hello používáte pro tento projekt a potom klikněte na hello **Engage** tlačítko dole v hello:</span><span class="sxs-lookup"><span data-stu-id="82438-149">Ensure you're in hello app we're using for this project and then click on hello **Engage** button at hello bottom:</span></span>
   
    ![][1]
2. <span data-ttu-id="82438-150">Přejdete na stránku hello nastavení na portálu Engagement.</span><span class="sxs-lookup"><span data-stu-id="82438-150">You will land in hello settings page in your Engagement Portal.</span></span> <span data-ttu-id="82438-151">Zde klikněte na hello **nativní oznámení** části:</span><span class="sxs-lookup"><span data-stu-id="82438-151">From there click on hello **Native Push** section:</span></span>
   
    ![][2]
3. <span data-ttu-id="82438-152">Konfigurace certifikátu iOS / klíče rozhraní API serveru GCM</span><span class="sxs-lookup"><span data-stu-id="82438-152">Configure iOS Certificate/GCM Server API Key</span></span>
   
    <span data-ttu-id="82438-153">**[iOS]**</span><span class="sxs-lookup"><span data-stu-id="82438-153">**[iOS]**</span></span>
   
    <span data-ttu-id="82438-154">a.</span><span class="sxs-lookup"><span data-stu-id="82438-154">a.</span></span> <span data-ttu-id="82438-155">Vyberte svůj soubor .p12, nahrajte ho a zadejte heslo:</span><span class="sxs-lookup"><span data-stu-id="82438-155">Select your .p12, upload it and type your password:</span></span>
   
    ![][3]
   
    <span data-ttu-id="82438-156">**[Android]**</span><span class="sxs-lookup"><span data-stu-id="82438-156">**[Android]**</span></span>
   
    <span data-ttu-id="82438-157">a.</span><span class="sxs-lookup"><span data-stu-id="82438-157">a.</span></span> <span data-ttu-id="82438-158">Klikněte na ikonu pro úpravu hello před **klíč rozhraní API** v části Nastavení GCM hello v automaticky otevřeném okně hello, který se zobrazí, vložte hello klíč serveru GCM a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="82438-158">Click hello edit icon in front of **API Key** in hello GCM Settings section and in hello popup which shows up, paste hello GCM Server Key and click **OK**.</span></span> 
   
    ![][4]

### <a name="enable-push-notifications-in-hello-cordova-app"></a><span data-ttu-id="82438-159">Povolení nabízených oznámení v aplikaci Cordova hello</span><span class="sxs-lookup"><span data-stu-id="82438-159">Enable push notifications in hello Cordova app</span></span>
<span data-ttu-id="82438-160">Upravit **www/js/index.js** tooadd hello volání tooMobile Engagement toorequest nabízená oznámení a deklaruje obslužnou rutinu:</span><span class="sxs-lookup"><span data-stu-id="82438-160">Edit **www/js/index.js** tooadd hello call tooMobile Engagement toorequest push notifications and declare a handler:</span></span>

     onDeviceReady: function() {
           Engagement.initializeReach(  
                 // on OpenUrl  
                 function(_url) {   
                 alert(_url);   
                 });  
            Engagement.startActivity("myPage",{});  
        }

### <a name="run-hello-app"></a><span data-ttu-id="82438-161">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="82438-161">Run hello app</span></span>
<span data-ttu-id="82438-162">**[iOS]**</span><span class="sxs-lookup"><span data-stu-id="82438-162">**[iOS]**</span></span>

1. <span data-ttu-id="82438-163">Jsme použije XCode toobuild a nasazení aplikace hello na hello zařízení tootest nabízená oznámení, protože iOS povoluje nabízená oznámení tooan skutečné zařízení.</span><span class="sxs-lookup"><span data-stu-id="82438-163">We will use XCode toobuild and deploy hello app on hello device tootest push notifications since iOS only allows push notifications tooan actual device.</span></span> <span data-ttu-id="82438-164">Přejděte toohello umístění, kde se má vytvořit projekt Cordova a přejděte příliš**...\platforms\ios** umístění.</span><span class="sxs-lookup"><span data-stu-id="82438-164">Go toohello location where your Cordova project is created and navigate too**...\platforms\ios** location.</span></span> <span data-ttu-id="82438-165">Otevřete soubor nativní .xcodeproj hello v XCode.</span><span class="sxs-lookup"><span data-stu-id="82438-165">Open up hello native .xcodeproj file in XCode.</span></span> 
2. <span data-ttu-id="82438-166">Vytváření a nasazování hello Cordova aplikace toohello zařízení s iOS pomocí hello účet, který má hello obsahující hello certifikát, že jste právě nahráli portál Mobile Engagement toohello a hello Id aplikace, který by odpovídal hello jeden, který jste zadali při vytváření profilu pro zřizování aplikace Cordova Hello.</span><span class="sxs-lookup"><span data-stu-id="82438-166">Build and deploy hello Cordova app toohello iOS device using hello account which has hello provisioning profile containing hello certificate you just uploaded toohello Mobile Engagement portal and hello App Id which matches hello one you provided while creating hello Cordova app.</span></span> <span data-ttu-id="82438-167">Je možné rezervovat hello *identifikátor svazku* v vaše **prostředky\*-info.plist** souboru v XCode toomatch ho nahoru.</span><span class="sxs-lookup"><span data-stu-id="82438-167">You can check out hello *Bundle identifier* in your **Resources\*-info.plist** file in XCode toomatch it up.</span></span> 
3. <span data-ttu-id="82438-168">Uvidíte, že hello standardní místní nabídka iOS na zařízení s informacemi o tom, že aplikace hello požadavky oprávnění toosend oznámení.</span><span class="sxs-lookup"><span data-stu-id="82438-168">You will see hello standard iOS popup on your device saying that hello app requests permission toosend notifications.</span></span> <span data-ttu-id="82438-169">Udělení oprávnění hello.</span><span class="sxs-lookup"><span data-stu-id="82438-169">Grant hello permission.</span></span> 

<span data-ttu-id="82438-170">**[Android]**</span><span class="sxs-lookup"><span data-stu-id="82438-170">**[Android]**</span></span>

<span data-ttu-id="82438-171">Jednoduše můžete hello emulátoru toorun hello Android aplikaci jako oznámení GCM jsou podporovány v emulátoru Android hello.</span><span class="sxs-lookup"><span data-stu-id="82438-171">You can simply use hello emulator toorun hello Android app as GCM notifications are supported on hello Android emulator.</span></span> 

    cordova run android

## <span data-ttu-id="82438-172"><a id="send"></a>Poslat tooyour aplikaci oznámení</span><span class="sxs-lookup"><span data-stu-id="82438-172"><a id="send"></a>Send a notification tooyour app</span></span>
<span data-ttu-id="82438-173">Teď vytvoříme jednoduchou kampaň nabízených oznámení, který bude odesílat nabízených tooyour aplikaci spuštěnou na zařízení hello:</span><span class="sxs-lookup"><span data-stu-id="82438-173">We will now create a simple Push Notification campaign that will send a push tooyour app running on hello device:</span></span>

1. <span data-ttu-id="82438-174">Přejděte toohello **dosáhnout** na portálu Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="82438-174">Navigate toohello **Reach** tab in your Mobile Engagement portal</span></span>
2. <span data-ttu-id="82438-175">Klikněte na tlačítko **nové oznámení** toocreate kampaň nabízených</span><span class="sxs-lookup"><span data-stu-id="82438-175">Click **New Announcement** toocreate your push campaign</span></span>
   
    ![][6]
3. <span data-ttu-id="82438-176">Zadejte vstupy toocreate kampaň **[Android]**</span><span class="sxs-lookup"><span data-stu-id="82438-176">Provide inputs toocreate your campaign **[Android]**</span></span>
   
   * <span data-ttu-id="82438-177">Zadejte **Název** kampaně.</span><span class="sxs-lookup"><span data-stu-id="82438-177">Provide a **Name** for your campaign.</span></span> 
   * <span data-ttu-id="82438-178">Vyberte hello **typ doručení** jako *systémové oznámení* *jednoduché*</span><span class="sxs-lookup"><span data-stu-id="82438-178">Select hello **Delivery Type** as *System notification* *Simple*</span></span>
   * <span data-ttu-id="82438-179">Vyberte hello **čas doručení** jako *"Kdykoliv"*</span><span class="sxs-lookup"><span data-stu-id="82438-179">Select hello **Delivery time** as *"Any Time"*</span></span>
   * <span data-ttu-id="82438-180">Zadejte **název** oznámení, který bude hello první řádek v nabízené hello.</span><span class="sxs-lookup"><span data-stu-id="82438-180">Provide a **Title** for your notification which will be hello first line in hello push.</span></span>
   * <span data-ttu-id="82438-181">Zadejte **zpráva** oznámení, který bude sloužit jako text zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="82438-181">Provide a **Message** for your notification which will serve as hello message body.</span></span> 
     
     ![][11]
4. <span data-ttu-id="82438-182">Zadejte vstupy toocreate kampaň **[iOS]**</span><span class="sxs-lookup"><span data-stu-id="82438-182">Provide inputs toocreate your campaign **[iOS]**</span></span>
   
   * <span data-ttu-id="82438-183">Zadejte **Název** kampaně.</span><span class="sxs-lookup"><span data-stu-id="82438-183">Provide a **Name** for your campaign.</span></span> 
   * <span data-ttu-id="82438-184">Vyberte hello **čas doručení** jako *"pouze mimo aplikaci"*</span><span class="sxs-lookup"><span data-stu-id="82438-184">Select hello **Delivery time** as *"Out of app only"*</span></span>
   * <span data-ttu-id="82438-185">Zadejte **název** oznámení, který bude hello první řádek v nabízené hello.</span><span class="sxs-lookup"><span data-stu-id="82438-185">Provide a **Title** for your notification which will be hello first line in hello push.</span></span>
   * <span data-ttu-id="82438-186">Zadejte **zpráva** oznámení, který bude sloužit jako text zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="82438-186">Provide a **Message** for your notification which will serve as hello message body.</span></span> 
     
     ![][12]
5. <span data-ttu-id="82438-187">Posuňte se dolů a v hello části obsahu vyberte **pouze oznámení**</span><span class="sxs-lookup"><span data-stu-id="82438-187">Scroll down, and in hello content section select **Notification only**</span></span>
   
    ![][8]
6. <span data-ttu-id="82438-188">[Nepovinné] Můžete také zadat adresu URL akce.</span><span class="sxs-lookup"><span data-stu-id="82438-188">[Optional] You can also provide an Action URL.</span></span> <span data-ttu-id="82438-189">Ujistěte se, že používá schéma URL zadat při konfiguraci hello plugin **AZME\_PŘESMĚROVÁNÍ\_URL** proměnná například *myapp://test*.</span><span class="sxs-lookup"><span data-stu-id="82438-189">Make sure that it uses a URL scheme provided while configuring hello plugin's **AZME\_REDIRECT\_URL** variable e.g. *myapp://test*.</span></span>  
7. <span data-ttu-id="82438-190">Dokončení nastavení hello nejzákladnější kampaně možné.</span><span class="sxs-lookup"><span data-stu-id="82438-190">You're done setting hello most basic campaign possible.</span></span> <span data-ttu-id="82438-191">Nyní znovu přejděte dolů a klikněte na tlačítko hello **vytvořit** tlačítko toosave kampaně.</span><span class="sxs-lookup"><span data-stu-id="82438-191">Now scroll down again and click hello **Create** button toosave your campaign.</span></span>
8. <span data-ttu-id="82438-192">Nakonec svou kampaň můžete **Aktivovat**</span><span class="sxs-lookup"><span data-stu-id="82438-192">Finally **Activate** your campaign</span></span>
   
    ![][10]
9. <span data-ttu-id="82438-193">Nyní byste měli na svém zařízení nebo emulátoru vidět nabízené oznámení z této kampaně.</span><span class="sxs-lookup"><span data-stu-id="82438-193">You should now see a push notification on your device or emulator as part of this campaign.</span></span> 

## <span data-ttu-id="82438-194"><a id="next-steps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="82438-194"><a id="next-steps"></a>Next Steps</span></span>
[<span data-ttu-id="82438-195">Přehled všech metod, které jsou k dispozici pro sadu Cordova Mobile Engagement SDK</span><span class="sxs-lookup"><span data-stu-id="82438-195">Overview of all methods available with Cordova Mobile Engagement SDK</span></span>](https://github.com/Azure/azure-mobile-engagement-cordova)

<!-- Images. -->

[1]: ./media/mobile-engagement-cordova-get-started/engage-button.png
[2]: ./media/mobile-engagement-cordova-get-started/engagement-portal.png
[3]: ./media/mobile-engagement-cordova-get-started/native-push-settings.png
[4]: ./media/mobile-engagement-cordova-get-started/api-key.png
[6]: ./media/mobile-engagement-cordova-get-started/new-announcement.png
[8]: ./media/mobile-engagement-cordova-get-started/campaign-content.png
[10]: ./media/mobile-engagement-cordova-get-started/campaign-activate.png
[11]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-android.png
[12]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-ios.png

