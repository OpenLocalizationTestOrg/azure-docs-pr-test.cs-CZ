---
title: "aaaGet Začínáme s Azure Mobile Engagementem pro iOS ve Swiftu | Microsoft Docs"
description: "Zjistěte, jak toouse Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro aplikace pro iOS."
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 196c282d-6f2f-4cbc-aeee-6517c5ad866d
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: swift
ms.topic: hero-article
ms.date: 09/20/2016
ms.author: piyushjo
ms.openlocfilehash: 9a3841d305745f8b80c6b0c86aabe18e0c7c0e59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-swift"></a><span data-ttu-id="18e97-103">Začínáme s Azure Mobile Engagementem pro aplikace pro iOS ve Swiftu</span><span class="sxs-lookup"><span data-stu-id="18e97-103">Get Started with Azure Mobile Engagement for iOS Apps in Swift</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="18e97-104">Toto téma ukazuje, jak Azure Mobile Engagement toounderstand toouse využití aplikací a odesílat nabízená oznámení toosegmented uživatelé tooan iOS aplikace.</span><span class="sxs-lookup"><span data-stu-id="18e97-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and send push notifications toosegmented users tooan iOS application.</span></span>
<span data-ttu-id="18e97-105">V tomto kurzu vytvoříte prázdnou aplikaci iOS, která bude shromažďovat základní data a přijímat nabízená oznámení pomocí systému nabízených oznámení Apple (APNS). </span><span class="sxs-lookup"><span data-stu-id="18e97-105">In this tutorial, you create a blank iOS app that collects basic data and receives push notifications using Apple Push Notification System (APNS).</span></span>

<span data-ttu-id="18e97-106">Tento kurz vyžaduje hello následující:</span><span class="sxs-lookup"><span data-stu-id="18e97-106">This tutorial requires hello following:</span></span>

* <span data-ttu-id="18e97-107">XCode 8, který si můžete nainstalovat z MAC App Storu</span><span class="sxs-lookup"><span data-stu-id="18e97-107">XCode 8, which you can install from your MAC App Store</span></span>
* <span data-ttu-id="18e97-108">Hello [Mobile Engagement iOS SDK]</span><span class="sxs-lookup"><span data-stu-id="18e97-108">hello [Mobile Engagement iOS SDK]</span></span>
* <span data-ttu-id="18e97-109">Certifikát nabízených oznámení (.p12), který můžete získat ve vývojářském centru Apple</span><span class="sxs-lookup"><span data-stu-id="18e97-109">Push notification certificate (.p12) that you can obtain on your Apple Dev Center</span></span>

> [!NOTE]
> <span data-ttu-id="18e97-110">V tomto kurzu budeme používat Swift verze 3.0.</span><span class="sxs-lookup"><span data-stu-id="18e97-110">This tutorial uses Swift version 3.0.</span></span> 
> 
> 

<span data-ttu-id="18e97-111">Ve všech dalších kurzech k Mobile Engagement týkajících se aplikací pro iOS se předpokládá dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="18e97-111">Completing this tutorial is a prerequisite for all other Mobile Engagement tutorials for iOS apps.</span></span>

> [!NOTE]
> <span data-ttu-id="18e97-112">toocomplete tento kurz, musíte mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="18e97-112">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="18e97-113">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="18e97-113">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="18e97-114">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).</span><span class="sxs-lookup"><span data-stu-id="18e97-114">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).</span></span>
> 
> 

## <span data-ttu-id="18e97-115"><a id="setup-azme"></a>Nastavení Mobile Engagementu pro vaši aplikaci pro iOS</span><span class="sxs-lookup"><span data-stu-id="18e97-115"><a id="setup-azme"></a>Setup Mobile Engagement for your iOS app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="18e97-116"><a id="connecting-app"></a>Připojit vaše aplikace toohello Mobile Engagement back-end</span><span class="sxs-lookup"><span data-stu-id="18e97-116"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="18e97-117">Tento kurzu si představíme "základní integraci", což je minimální hello nastavte požadované toocollect dat a odesílání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="18e97-117">This tutorial presents a "basic integration", which is hello minimal set required toocollect data and send a push notification.</span></span> <span data-ttu-id="18e97-118">Hello kompletní dokumentaci k integraci najdete v hello [integraci sady Mobile Engagement iOS SDK](mobile-engagement-ios-sdk-overview.md)</span><span class="sxs-lookup"><span data-stu-id="18e97-118">hello complete integration documentation can be found in hello [Mobile Engagement iOS SDK integration](mobile-engagement-ios-sdk-overview.md)</span></span>

<span data-ttu-id="18e97-119">Pomocí integrace hello toodemonstrate XCode vytvoříme základní aplikaci:</span><span class="sxs-lookup"><span data-stu-id="18e97-119">We will create a basic app with XCode toodemonstrate hello integration:</span></span>

### <a name="create-a-new-ios-project"></a><span data-ttu-id="18e97-120">Vytvoření nového projektu iOS</span><span class="sxs-lookup"><span data-stu-id="18e97-120">Create a new iOS project</span></span>
[!INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

### <a name="connect-your-app-toomobile-engagement-backend"></a><span data-ttu-id="18e97-121">Připojit vaše tooMobile Engagement back-end aplikace</span><span class="sxs-lookup"><span data-stu-id="18e97-121">Connect your app tooMobile Engagement backend</span></span>
1. <span data-ttu-id="18e97-122">Stáhnout hello [Mobile Engagement iOS SDK]</span><span class="sxs-lookup"><span data-stu-id="18e97-122">Download hello [Mobile Engagement iOS SDK]</span></span>
2. <span data-ttu-id="18e97-123">Extrahování hello. tar.gz souboru tooa složky v počítači</span><span class="sxs-lookup"><span data-stu-id="18e97-123">Extract hello .tar.gz file tooa folder in your computer</span></span>
3. <span data-ttu-id="18e97-124">Klikněte pravým tlačítkem na projekt hello a vyberte možnost "Přidat soubory příliš..."</span><span class="sxs-lookup"><span data-stu-id="18e97-124">Right click hello project and select "Add files too..."</span></span>
   
    ![][1]
4. <span data-ttu-id="18e97-125">Přejděte toohello složky, které jste extrahovali hello SDK a vyberte hello `EngagementSDK` složky poté stiskněte klávesu OK.</span><span class="sxs-lookup"><span data-stu-id="18e97-125">Navigate toohello folder where you extracted hello SDK and select hello `EngagementSDK` folder then press OK.</span></span>
   
    ![][2]
5. <span data-ttu-id="18e97-126">Otevřete hello `Build Phases` kartě a v hello `Link Binary With Libraries` nabídky přidat hello rozhraní, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="18e97-126">Open hello `Build Phases` tab and in hello `Link Binary With Libraries` menu add hello frameworks as shown below:</span></span>
   
    ![][3]
6. <span data-ttu-id="18e97-127">Vytvoření přemostění záhlaví toobe možné toouse hello sady SDK API jazyka Objective C tak, že zvolíte Soubor > Nový > Soubor > iOS > zdroj > hlavičkový soubor.</span><span class="sxs-lookup"><span data-stu-id="18e97-127">Create a Bridging header toobe able toouse hello SDK's Objective C APIs by choosing File > New > File > iOS > Source > Header File.</span></span>
   
    ![][4]
7. <span data-ttu-id="18e97-128">Upravit hello přemostění hlavičky souboru tooexpose Mobile Engagement Objective-C kód tooyour kód se Swift, přidejte následující importy hello:</span><span class="sxs-lookup"><span data-stu-id="18e97-128">Edit hello bridging header file tooexpose Mobile Engagement Objective-C code tooyour Swift code, add hello following imports :</span></span>
   
        /* Mobile Engagement Agent */
        #import "AEModule.h"
        #import "AEPushMessage.h"
        #import "AEStorage.h"
        #import "EngagementAgent.h"
        #import "EngagementTableViewController.h"
        #import "EngagementViewController.h"
        #import "AEUserNotificationHandler.h"
        #import "AEIdfaProvider.h"
8. <span data-ttu-id="18e97-129">V části nastavení sestavení zkontrolujte, zda hello nastavení v části kompilátoru Swift – generování kódu sestavení hlavičky přemostění jazyka Objective-C má hlavičku toothis cesta.</span><span class="sxs-lookup"><span data-stu-id="18e97-129">Under Build Settings, make sure hello Objective-C Bridging Header build setting under Swift Compiler - Code Generation has a path toothis header.</span></span> <span data-ttu-id="18e97-130">Tady je příklad cesty: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (v závislosti na hello cesta)**</span><span class="sxs-lookup"><span data-stu-id="18e97-130">Here is a path example: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (depending on hello path)**</span></span>
   
   ![][6]
9. <span data-ttu-id="18e97-131">Přejděte zpět toohello portál Azure ve vaší aplikaci *informace o připojení* stránky a zkopírujte připojovací řetězec hello</span><span class="sxs-lookup"><span data-stu-id="18e97-131">Go back toohello Azure portal in your app's *Connection Info* page and copy hello Connection String</span></span>
   
   ![][5]
10. <span data-ttu-id="18e97-132">Nyní vložte připojovací řetězec hello hello `didFinishLaunchingWithOptions` delegovat</span><span class="sxs-lookup"><span data-stu-id="18e97-132">Now paste hello connection string in hello `didFinishLaunchingWithOptions` delegate</span></span>
    
        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool
        {
              [...]
                EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}")
              [...]
        }

## <span data-ttu-id="18e97-133"><a id="monitor"></a>Povolení sledování v reálném čase</span><span class="sxs-lookup"><span data-stu-id="18e97-133"><a id="monitor"></a>Enabling real-time monitoring</span></span>
<span data-ttu-id="18e97-134">V pořadí toostart odesílat data a zajistit, že hello uživatelé jsou aktivní musíte odeslat alespoň jednu obrazovku (aktivitu) toohello Mobile Engagement back-end.</span><span class="sxs-lookup"><span data-stu-id="18e97-134">In order toostart sending data and ensuring that hello users are active, you must send at least one screen (Activity) toohello Mobile Engagement backend.</span></span>

1. <span data-ttu-id="18e97-135">Otevřete hello **ViewController.swift** souboru a nahraďte základní třídu hello **ViewController** toobe **EngagementViewController**:</span><span class="sxs-lookup"><span data-stu-id="18e97-135">Open hello **ViewController.swift** file and replace hello base class of **ViewController** toobe **EngagementViewController**:</span></span>
   
    `class ViewController : EngagementViewController {`

## <span data-ttu-id="18e97-136"><a id="monitor"></a>Připojení aplikace se sledováním v reálném čase</span><span class="sxs-lookup"><span data-stu-id="18e97-136"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="18e97-137"><a id="integrate-push"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci</span><span class="sxs-lookup"><span data-stu-id="18e97-137"><a id="integrate-push"></a>Enabling Push Notifications and in-app messaging</span></span>
<span data-ttu-id="18e97-138">Mobile Engagement umožňuje toointeract a REACH s uživateli pomocí nabízených oznámení a zasílání zpráv v aplikaci v kontextu hello kampaní.</span><span class="sxs-lookup"><span data-stu-id="18e97-138">Mobile Engagement allows you toointeract and REACH with your users with Push Notifications and In-app Messaging in hello context of campaigns.</span></span> <span data-ttu-id="18e97-139">Tento modul je hello portálu Mobile Engagement nazývá REACH.</span><span class="sxs-lookup"><span data-stu-id="18e97-139">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="18e97-140">Hello následujících sekcích nastavíte tooreceive vaše aplikace je.</span><span class="sxs-lookup"><span data-stu-id="18e97-140">hello following sections will setup your app tooreceive them.</span></span>

### <a name="enable-your-app-tooreceive-silent-push-notifications"></a><span data-ttu-id="18e97-141">Povolit vaší aplikace tooreceive bezobslužných nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="18e97-141">Enable your app tooreceive Silent Push Notifications</span></span>
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-hello-reach-library-tooyour-project"></a><span data-ttu-id="18e97-142">Přidání projektu tooyour knihovny Reach hello</span><span class="sxs-lookup"><span data-stu-id="18e97-142">Add hello Reach library tooyour project</span></span>
1. <span data-ttu-id="18e97-143">Klikněte pravým tlačítkem na projekt.</span><span class="sxs-lookup"><span data-stu-id="18e97-143">Right click your project</span></span>
2. <span data-ttu-id="18e97-144">Vyberte `Add file too...`</span><span class="sxs-lookup"><span data-stu-id="18e97-144">Select `Add file too...`</span></span>
3. <span data-ttu-id="18e97-145">Přejděte toohello složky, které jste extrahovali hello SDK</span><span class="sxs-lookup"><span data-stu-id="18e97-145">Navigate toohello folder where you extracted hello SDK</span></span>
4. <span data-ttu-id="18e97-146">Vyberte hello `EngagementReach` složky</span><span class="sxs-lookup"><span data-stu-id="18e97-146">Select hello `EngagementReach` folder</span></span>
5. <span data-ttu-id="18e97-147">Klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="18e97-147">Click Add</span></span>
6. <span data-ttu-id="18e97-148">Upravit hello přemostění hlavičky Reach jazyka Objective-C Mobile Engagement záhlaví tooexpose souborů a přidejte následující importy hello:</span><span class="sxs-lookup"><span data-stu-id="18e97-148">Edit hello bridging header file tooexpose Mobile Engagement Objective-C Reach headers and add hello following imports :</span></span>
   
        /* Mobile Engagement Reach */
        #import "AEAnnouncementViewController.h"
        #import "AEAutorotateView.h"
        #import "AEContentViewController.h"
        #import "AEDefaultAnnouncementViewController.h"
        #import "AEDefaultNotifier.h"
        #import "AEDefaultPollViewController.h"
        #import "AEInteractiveContent.h"
        #import "AENotificationView.h"
        #import "AENotifier.h"
        #import "AEPollViewController.h"
        #import "AEReachAbstractAnnouncement.h"
        #import "AEReachAnnouncement.h"
        #import "AEReachContent.h"
        #import "AEReachDataPush.h"
        #import "AEReachDataPushDelegate.h"
        #import "AEReachModule.h"
        #import "AEReachNotifAnnouncement.h"
        #import "AEReachPoll.h"
        #import "AEReachPollQuestion.h"
        #import "AEViewControllerUtil.h"
        #import "AEWebAnnouncementJsBridge.h"

### <a name="modify-your-application-delegate"></a><span data-ttu-id="18e97-149">Úprava delegáta aplikace</span><span class="sxs-lookup"><span data-stu-id="18e97-149">Modify your Application Delegate</span></span>
1. <span data-ttu-id="18e97-150">Uvnitř hello `didFinishLaunchingWithOptions` – Vytvořte modul kampaně reach a předejte ji tooyour existujícího inicializačního řádku Engagement:</span><span class="sxs-lookup"><span data-stu-id="18e97-150">Inside  hello `didFinishLaunchingWithOptions` -  create a reach module and pass it tooyour existing Engagement initialization line:</span></span>
   
        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool 
        {
            let reach = AEReachModule.module(withNotificationIcon: UIImage(named:"icon.png")) as! AEReachModule
            EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}", modulesArray:[reach])
            [...]
            return true
        }

### <a name="enable-your-app-tooreceive-apns-push-notifications"></a><span data-ttu-id="18e97-151">Povolit vaší aplikace tooreceive nabízených oznámení APNS</span><span class="sxs-lookup"><span data-stu-id="18e97-151">Enable your app tooreceive APNS Push Notifications</span></span>
1. <span data-ttu-id="18e97-152">Přidejte následující řádek toohello hello `didFinishLaunchingWithOptions` metoda:</span><span class="sxs-lookup"><span data-stu-id="18e97-152">Add hello following line toohello `didFinishLaunchingWithOptions` method:</span></span>
   
        if #available(iOS 8.0, *)
        {
            if #available(iOS 10.0, *)
            {
                UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .badge, .sound]) { (granted, error) in }
            }else
            {
                let settings = UIUserNotificationSettings(types: [.alert, .badge, .sound], categories: nil)
                application.registerUserNotificationSettings(settings)
            }
            application.registerForRemoteNotifications()
        }
        else
        {
            application.registerForRemoteNotifications(matching: [.alert, .badge, .sound])
        }
2. <span data-ttu-id="18e97-153">Přidat hello `didRegisterForRemoteNotificationsWithDeviceToken` metoda následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="18e97-153">Add hello `didRegisterForRemoteNotificationsWithDeviceToken` method as follows:</span></span>
   
        func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
            EngagementAgent.shared().registerDeviceToken(deviceToken)
        }
3. <span data-ttu-id="18e97-154">Přidat hello `didReceiveRemoteNotification:fetchCompletionHandler:` metoda následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="18e97-154">Add hello `didReceiveRemoteNotification:fetchCompletionHandler:` method as follows:</span></span>
   
        func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
            EngagementAgent.shared().applicationDidReceiveRemoteNotification(userInfo, fetchCompletionHandler:completionHandler)
        }

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-swift-get-started/add-header-file.png
[5]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
[6]: ./media/mobile-engagement-ios-swift-get-started/add-bridging-header.png
