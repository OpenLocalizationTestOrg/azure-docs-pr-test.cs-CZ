---
title: "Začínáme s Azure Mobile Engagementem pro iOS ve Swiftu | Dokumentace Microsoftu"
description: "Naučte se používat Azure Mobile Engagement s analýzou a nabízenými oznámeními pro aplikace pro iOS."
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
ms.openlocfilehash: 1011b9823333e79a52cd2d187df4f8d063b1f799
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-swift"></a><span data-ttu-id="c12ec-103">Začínáme s Azure Mobile Engagementem pro aplikace pro iOS ve Swiftu</span><span class="sxs-lookup"><span data-stu-id="c12ec-103">Get Started with Azure Mobile Engagement for iOS Apps in Swift</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="c12ec-104">V tomto tématu si ukážeme, jak používat Azure Mobile Engagement, jak porozumět používání aplikace a jak odesílat nabízená oznámení segmentovaným uživatelům aplikace pro iOS.</span><span class="sxs-lookup"><span data-stu-id="c12ec-104">This topic shows you how to use Azure Mobile Engagement to understand your app usage and send push notifications to segmented users to an iOS application.</span></span>
<span data-ttu-id="c12ec-105">V tomto kurzu vytvoříte prázdnou aplikaci iOS, která bude shromažďovat základní data a přijímat nabízená oznámení pomocí systému nabízených oznámení Apple (APNS). </span><span class="sxs-lookup"><span data-stu-id="c12ec-105">In this tutorial, you create a blank iOS app that collects basic data and receives push notifications using Apple Push Notification System (APNS).</span></span>

<span data-ttu-id="c12ec-106">V tomto kurzu budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="c12ec-106">This tutorial requires the following:</span></span>

* <span data-ttu-id="c12ec-107">XCode 8, který si můžete nainstalovat z MAC App Storu</span><span class="sxs-lookup"><span data-stu-id="c12ec-107">XCode 8, which you can install from your MAC App Store</span></span>
* <span data-ttu-id="c12ec-108">[Mobile Engagement iOS SDK]</span><span class="sxs-lookup"><span data-stu-id="c12ec-108">the [Mobile Engagement iOS SDK]</span></span>
* <span data-ttu-id="c12ec-109">Certifikát nabízených oznámení (.p12), který můžete získat ve vývojářském centru Apple</span><span class="sxs-lookup"><span data-stu-id="c12ec-109">Push notification certificate (.p12) that you can obtain on your Apple Dev Center</span></span>

> [!NOTE]
> <span data-ttu-id="c12ec-110">V tomto kurzu budeme používat Swift verze 3.0.</span><span class="sxs-lookup"><span data-stu-id="c12ec-110">This tutorial uses Swift version 3.0.</span></span> 
> 
> 

<span data-ttu-id="c12ec-111">Ve všech dalších kurzech k Mobile Engagement týkajících se aplikací pro iOS se předpokládá dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="c12ec-111">Completing this tutorial is a prerequisite for all other Mobile Engagement tutorials for iOS apps.</span></span>

> [!NOTE]
> <span data-ttu-id="c12ec-112">K dokončení tohoto kurzu potřebujete mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="c12ec-112">To complete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="c12ec-113">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="c12ec-113">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="c12ec-114">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).</span><span class="sxs-lookup"><span data-stu-id="c12ec-114">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).</span></span>
> 
> 

## <span data-ttu-id="c12ec-115"><a id="setup-azme"></a>Nastavení Mobile Engagementu pro vaši aplikaci pro iOS</span><span class="sxs-lookup"><span data-stu-id="c12ec-115"><a id="setup-azme"></a>Setup Mobile Engagement for your iOS app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="c12ec-116"><a id="connecting-app"></a>Připojení aplikace k back-endu Mobile Engagementu</span><span class="sxs-lookup"><span data-stu-id="c12ec-116"><a id="connecting-app"></a>Connect your app to the Mobile Engagement backend</span></span>
<span data-ttu-id="c12ec-117">V tomto kurzu si představíme „základní integraci“, čili minimální sadu požadovanou pro shromažďování dat a odesílání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="c12ec-117">This tutorial presents a "basic integration", which is the minimal set required to collect data and send a push notification.</span></span> <span data-ttu-id="c12ec-118">Kompletní dokumentaci k integraci najdete v článku [Integrace sady Mobile Engagement iOS SDK](mobile-engagement-ios-sdk-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c12ec-118">The complete integration documentation can be found in the [Mobile Engagement iOS SDK integration](mobile-engagement-ios-sdk-overview.md)</span></span>

<span data-ttu-id="c12ec-119">Pomocí XCodu si vytvoříme základní aplikaci, na které si tuto integraci předvedeme.</span><span class="sxs-lookup"><span data-stu-id="c12ec-119">We will create a basic app with XCode to demonstrate the integration:</span></span>

### <a name="create-a-new-ios-project"></a><span data-ttu-id="c12ec-120">Vytvoření nového projektu iOS</span><span class="sxs-lookup"><span data-stu-id="c12ec-120">Create a new iOS project</span></span>
[!INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

### <a name="connect-your-app-to-mobile-engagement-backend"></a><span data-ttu-id="c12ec-121">Připojení aplikace k back-endu Mobile Engagementu</span><span class="sxs-lookup"><span data-stu-id="c12ec-121">Connect your app to Mobile Engagement backend</span></span>
1. <span data-ttu-id="c12ec-122">Stáhněte si [Mobile Engagement iOS SDK].</span><span class="sxs-lookup"><span data-stu-id="c12ec-122">Download the [Mobile Engagement iOS SDK]</span></span>
2. <span data-ttu-id="c12ec-123">Extrahujte soubor .tar.gz do složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="c12ec-123">Extract the .tar.gz file to a folder in your computer</span></span>
3. <span data-ttu-id="c12ec-124">Pravým tlačítkem myši klikněte na projekt a pak vyberte Přidat soubory do...</span><span class="sxs-lookup"><span data-stu-id="c12ec-124">Right click the project and select "Add files to ..."</span></span>
   
    ![][1]
4. <span data-ttu-id="c12ec-125">Přejděte do složky, do které jste extrahovali sadu SDK, vyberte složku `EngagementSDK` a poté stiskněte klávesu OK.</span><span class="sxs-lookup"><span data-stu-id="c12ec-125">Navigate to the folder where you extracted the SDK and select the `EngagementSDK` folder then press OK.</span></span>
   
    ![][2]
5. <span data-ttu-id="c12ec-126">Otevřete kartu `Build Phases` (Fáze sestavení) a v nabídce `Link Binary With Libraries` (Propojit binární kód s knihovnami) přidejte rozhraní, jak je uvedeno dál:</span><span class="sxs-lookup"><span data-stu-id="c12ec-126">Open the `Build Phases` tab and in the `Link Binary With Libraries` menu add the frameworks as shown below:</span></span>
   
    ![][3]
6. <span data-ttu-id="c12ec-127">Pomocí příkazu File (Soubor) > New (Nový) > File (Soubor) > iOS > Source (Zdroj) > Header File (Soubor hlavičky) vytvořte hlavičku přemostění, abyste mohli použít rozhraní API jazyka Objective C sady SDK.</span><span class="sxs-lookup"><span data-stu-id="c12ec-127">Create a Bridging header to be able to use the SDK's Objective C APIs by choosing File > New > File > iOS > Source > Header File.</span></span>
   
    ![][4]
7. <span data-ttu-id="c12ec-128">Upravte soubor hlavičky přemostění, aby se zpřístupnil kód jazyka Objective-C Mobile Engagementu pro kód jazyka Swift, a potom přidejte následující importy:</span><span class="sxs-lookup"><span data-stu-id="c12ec-128">Edit the bridging header file to expose Mobile Engagement Objective-C code to your Swift code, add the following imports :</span></span>
   
        /* Mobile Engagement Agent */
        #import "AEModule.h"
        #import "AEPushMessage.h"
        #import "AEStorage.h"
        #import "EngagementAgent.h"
        #import "EngagementTableViewController.h"
        #import "EngagementViewController.h"
        #import "AEUserNotificationHandler.h"
        #import "AEIdfaProvider.h"
8. <span data-ttu-id="c12ec-129">V části Nastavení sestavení zkontrolujte, zda má nastavení sestavení hlavičky přemostění jazyka Objective-C v části Swift Compiler - Code Generation (Kompilátor jazyka Swift – Generování kódu) cestu k této hlavičce.</span><span class="sxs-lookup"><span data-stu-id="c12ec-129">Under Build Settings, make sure the Objective-C Bridging Header build setting under Swift Compiler - Code Generation has a path to this header.</span></span> <span data-ttu-id="c12ec-130">Zde je příklad cesty: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (v závislosti na cestě)**</span><span class="sxs-lookup"><span data-stu-id="c12ec-130">Here is a path example: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (depending on the path)**</span></span>
   
   ![][6]
9. <span data-ttu-id="c12ec-131">Přejděte zpět na portál Azure na stránce *Connection Info* (Informace o připojení) vaší aplikace a zkopírujte připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="c12ec-131">Go back to the Azure portal in your app's *Connection Info* page and copy the Connection String</span></span>
   
   ![][5]
10. <span data-ttu-id="c12ec-132">Nyní vložte připojovací řetězec do delegáta `didFinishLaunchingWithOptions`.</span><span class="sxs-lookup"><span data-stu-id="c12ec-132">Now paste the connection string in the `didFinishLaunchingWithOptions` delegate</span></span>
    
        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool
        {
              [...]
                EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}")
              [...]
        }

## <span data-ttu-id="c12ec-133"><a id="monitor"></a>Povolení sledování v reálném čase</span><span class="sxs-lookup"><span data-stu-id="c12ec-133"><a id="monitor"></a>Enabling real-time monitoring</span></span>
<span data-ttu-id="c12ec-134">Pokud chcete začít odesílat data a zajistit, že uživatelé jsou aktivní, musíte odeslat alespoň jednu obrazovku (aktivitu) na back-end Mobile Engagementu.</span><span class="sxs-lookup"><span data-stu-id="c12ec-134">In order to start sending data and ensuring that the users are active, you must send at least one screen (Activity) to the Mobile Engagement backend.</span></span>

1. <span data-ttu-id="c12ec-135">Otevřete soubor **ViewController.swift** a nahraďte základní třídu **ViewController** třídou **EngagementViewController**:</span><span class="sxs-lookup"><span data-stu-id="c12ec-135">Open the **ViewController.swift** file and replace the base class of **ViewController** to be **EngagementViewController**:</span></span>
   
    `class ViewController : EngagementViewController {`

## <span data-ttu-id="c12ec-136"><a id="monitor"></a>Připojení aplikace se sledováním v reálném čase</span><span class="sxs-lookup"><span data-stu-id="c12ec-136"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="c12ec-137"><a id="integrate-push"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci</span><span class="sxs-lookup"><span data-stu-id="c12ec-137"><a id="integrate-push"></a>Enabling Push Notifications and in-app messaging</span></span>
<span data-ttu-id="c12ec-138">Mobile Engagement vám umožňuje v rámci kampaní oslovit uživatele a komunikovat s nimi prostřednictvím nabízených oznámení a zpráv v aplikacích.</span><span class="sxs-lookup"><span data-stu-id="c12ec-138">Mobile Engagement allows you to interact and REACH with your users with Push Notifications and In-app Messaging in the context of campaigns.</span></span> <span data-ttu-id="c12ec-139">Tento modul se na portálu Mobile Engagement nazývá REACH.</span><span class="sxs-lookup"><span data-stu-id="c12ec-139">This module is called REACH in the Mobile Engagement portal.</span></span>
<span data-ttu-id="c12ec-140">V následujících sekcích nastavíte aplikaci, aby tato nabízená oznámení a zprávy přijímala.</span><span class="sxs-lookup"><span data-stu-id="c12ec-140">The following sections will setup your app to receive them.</span></span>

### <a name="enable-your-app-to-receive-silent-push-notifications"></a><span data-ttu-id="c12ec-141">Povolení přijímání bezobslužných nabízených oznámení v aplikaci</span><span class="sxs-lookup"><span data-stu-id="c12ec-141">Enable your app to receive Silent Push Notifications</span></span>
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-the-reach-library-to-your-project"></a><span data-ttu-id="c12ec-142">Přidání knihovny Reach do projektu</span><span class="sxs-lookup"><span data-stu-id="c12ec-142">Add the Reach library to your project</span></span>
1. <span data-ttu-id="c12ec-143">Klikněte pravým tlačítkem na projekt.</span><span class="sxs-lookup"><span data-stu-id="c12ec-143">Right click your project</span></span>
2. <span data-ttu-id="c12ec-144">Vyberte `Add file to ...`</span><span class="sxs-lookup"><span data-stu-id="c12ec-144">Select `Add file to ...`</span></span>
3. <span data-ttu-id="c12ec-145">Přejděte do složky, do které jste extrahovali sadu SDK.</span><span class="sxs-lookup"><span data-stu-id="c12ec-145">Navigate to the folder where you extracted the SDK</span></span>
4. <span data-ttu-id="c12ec-146">Vyberte složku `EngagementReach`.</span><span class="sxs-lookup"><span data-stu-id="c12ec-146">Select the `EngagementReach` folder</span></span>
5. <span data-ttu-id="c12ec-147">Klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="c12ec-147">Click Add</span></span>
6. <span data-ttu-id="c12ec-148">Upravte soubor hlavičky přemostění, aby se zpřístupnily hlavičky Reach jazyka Objective-C Mobile Engagementu, a potom přidejte následující importy:</span><span class="sxs-lookup"><span data-stu-id="c12ec-148">Edit the bridging header file to expose Mobile Engagement Objective-C Reach headers and add the following imports :</span></span>
   
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

### <a name="modify-your-application-delegate"></a><span data-ttu-id="c12ec-149">Úprava delegáta aplikace</span><span class="sxs-lookup"><span data-stu-id="c12ec-149">Modify your Application Delegate</span></span>
1. <span data-ttu-id="c12ec-150">V metodě `didFinishLaunchingWithOptions` vytvořte modul kampaně Reach a předejte jej do existujícího inicializačního řádku Engagement:</span><span class="sxs-lookup"><span data-stu-id="c12ec-150">Inside  the `didFinishLaunchingWithOptions` -  create a reach module and pass it to your existing Engagement initialization line:</span></span>
   
        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool 
        {
            let reach = AEReachModule.module(withNotificationIcon: UIImage(named:"icon.png")) as! AEReachModule
            EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}", modulesArray:[reach])
            [...]
            return true
        }

### <a name="enable-your-app-to-receive-apns-push-notifications"></a><span data-ttu-id="c12ec-151">Povolení přijímání nabízených oznámení APNS v aplikaci</span><span class="sxs-lookup"><span data-stu-id="c12ec-151">Enable your app to receive APNS Push Notifications</span></span>
1. <span data-ttu-id="c12ec-152">Do metody `didFinishLaunchingWithOptions` přidejte následující řádek:</span><span class="sxs-lookup"><span data-stu-id="c12ec-152">Add the following line to the `didFinishLaunchingWithOptions` method:</span></span>
   
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
2. <span data-ttu-id="c12ec-153">Následujícím způsobem přidejte metodu `didRegisterForRemoteNotificationsWithDeviceToken`:</span><span class="sxs-lookup"><span data-stu-id="c12ec-153">Add the `didRegisterForRemoteNotificationsWithDeviceToken` method as follows:</span></span>
   
        func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
            EngagementAgent.shared().registerDeviceToken(deviceToken)
        }
3. <span data-ttu-id="c12ec-154">Následujícím způsobem přidejte metodu `didReceiveRemoteNotification:fetchCompletionHandler:`:</span><span class="sxs-lookup"><span data-stu-id="c12ec-154">Add the `didReceiveRemoteNotification:fetchCompletionHandler:` method as follows:</span></span>
   
        func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
            EngagementAgent.shared().applicationDidReceiveRemoteNotification(userInfo, fetchCompletionHandler:completionHandler)
        }

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
<span data-ttu-id="c12ec-155">[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj</span><span class="sxs-lookup"><span data-stu-id="c12ec-155">[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj</span></span>

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-swift-get-started/add-header-file.png
[5]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
[6]: ./media/mobile-engagement-ios-swift-get-started/add-bridging-header.png