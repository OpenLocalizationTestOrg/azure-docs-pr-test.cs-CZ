---
title: "aaaGet Začínáme s Azure Mobile Engagementem pro iOS v Objective C | Microsoft Docs"
description: "Zjistěte, jak toouse Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro aplikace iOS."
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 117b5742-522b-41de-98c5-f432da2d98f8
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: hero-article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: 51a5013f23d2d04a7b9b30c83b47017898b2bb00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-objective-c"></a><span data-ttu-id="2470f-103">Začínáme s Azure Mobile Engagementem pro aplikace pro iOS v Objective C</span><span class="sxs-lookup"><span data-stu-id="2470f-103">Get Started with Azure Mobile Engagement for iOS apps in Objective C</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="2470f-104">Toto téma ukazuje, jak Azure Mobile Engagement toounderstand toouse využití aplikací a odesílat nabízená oznámení toosegmented uživatelé tooan iOS aplikace.</span><span class="sxs-lookup"><span data-stu-id="2470f-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and send push notifications toosegmented users tooan iOS application.</span></span>
<span data-ttu-id="2470f-105">V tomto kurzu vytvoříte prázdnou aplikaci iOS, která bude shromažďovat základní data a přijímat nabízená oznámení pomocí systému nabízených oznámení Apple (APNS). </span><span class="sxs-lookup"><span data-stu-id="2470f-105">In this tutorial, you create a blank iOS app that collects basic data and receives push notifications using Apple Push Notification System (APNS).</span></span>

<span data-ttu-id="2470f-106">Tento kurz vyžaduje hello následující:</span><span class="sxs-lookup"><span data-stu-id="2470f-106">This tutorial requires hello following:</span></span>

* <span data-ttu-id="2470f-107">XCode 8, který si můžete nainstalovat z MAC App Storu</span><span class="sxs-lookup"><span data-stu-id="2470f-107">XCode 8, which you can install from your MAC App Store</span></span>
* <span data-ttu-id="2470f-108">Hello [Mobile Engagement iOS SDK]</span><span class="sxs-lookup"><span data-stu-id="2470f-108">hello [Mobile Engagement iOS SDK]</span></span>

<span data-ttu-id="2470f-109">Ve všech dalších kurzech k Mobile Engagement týkajících se aplikací pro iOS se předpokládá dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="2470f-109">Completing this tutorial is a prerequisite for all other Mobile Engagement tutorials for iOS apps.</span></span>

> [!NOTE]
> <span data-ttu-id="2470f-110">toocomplete tento kurz, musíte mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="2470f-110">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="2470f-111">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="2470f-111">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="2470f-112">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="2470f-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).</span></span>
>
>

## <span data-ttu-id="2470f-113"><a id="setup-azme"></a>Nastavení Mobile Engagementu pro vaši aplikaci pro iOS</span><span class="sxs-lookup"><span data-stu-id="2470f-113"><a id="setup-azme"></a>Setup Mobile Engagement for your iOS app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="2470f-114"><a id="connecting-app"></a>Připojit vaše aplikace toohello Mobile Engagement back-end</span><span class="sxs-lookup"><span data-stu-id="2470f-114"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="2470f-115">Tento kurzu si představíme "základní integraci", což je minimální hello nastavte požadované toocollect dat a odesílání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="2470f-115">This tutorial presents a "basic integration", which is hello minimal set required toocollect data and send a push notification.</span></span> <span data-ttu-id="2470f-116">Hello kompletní dokumentaci k integraci najdete v hello [integraci sady Mobile Engagement iOS SDK](mobile-engagement-ios-sdk-overview.md)</span><span class="sxs-lookup"><span data-stu-id="2470f-116">hello complete integration documentation can be found in hello [Mobile Engagement iOS SDK integration](mobile-engagement-ios-sdk-overview.md)</span></span>

<span data-ttu-id="2470f-117">Vytvoříme základní aplikaci s integrací hello toodemonstrate XCode.</span><span class="sxs-lookup"><span data-stu-id="2470f-117">We will create a basic app with XCode toodemonstrate hello integration.</span></span>

### <a name="create-a-new-ios-project"></a><span data-ttu-id="2470f-118">Vytvoření nového projektu iOS</span><span class="sxs-lookup"><span data-stu-id="2470f-118">Create a new iOS project</span></span>
[!INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

### <a name="connect-your-app-toohello-mobile-engagement-backend"></a><span data-ttu-id="2470f-119">Připojit vaše aplikace toohello Mobile Engagement back-end</span><span class="sxs-lookup"><span data-stu-id="2470f-119">Connect your app toohello Mobile Engagement backend</span></span>
1. <span data-ttu-id="2470f-120">Stáhnout hello [Mobile Engagement iOS SDK].</span><span class="sxs-lookup"><span data-stu-id="2470f-120">Download hello [Mobile Engagement iOS SDK].</span></span>
2. <span data-ttu-id="2470f-121">Extrahování hello. tar.gz souboru tooa složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="2470f-121">Extract hello .tar.gz file tooa folder in your computer.</span></span>
3. <span data-ttu-id="2470f-122">Klikněte pravým tlačítkem na projekt hello a potom vyberte **přidat soubory do**.</span><span class="sxs-lookup"><span data-stu-id="2470f-122">Right-click hello project, and then select **Add files to**.</span></span>

    ![][1]
4. <span data-ttu-id="2470f-123">Přejděte toohello složky, které jste extrahovali hello SDK, vyberte hello `EngagementSDK` složku, klikněte na tlačítko **možnosti** hello levém dolním rohu a ujistěte se, toto zaškrtávací políčko hello **kopírovat položky v případě potřeby** a hello zaškrtávací políčko k cílovému jsou zaškrtnutá políčka stiskněte klávesu **OK**.</span><span class="sxs-lookup"><span data-stu-id="2470f-123">Navigate toohello folder where you extracted hello SDK, select hello `EngagementSDK` folder, click **Options** on hello bottom left corner and make sure that hello checkbox **Copy items if needed** and hello checkbox for your target are checked then press **OK**.</span></span>

    ![][2]
5. <span data-ttu-id="2470f-124">Otevřete hello **fáze buildu** kartě a v hello **odkaz binárních souborů a knihoven** nabídky, přidejte hello architektury, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="2470f-124">Open hello **Build Phases** tab, and in hello **Link Binary With Libraries** menu, add hello frameworks as shown below:</span></span>

    ![][3]
6. <span data-ttu-id="2470f-125">Přejděte zpět toohello portál Azure ve vaší aplikaci **informace o připojení** stránky a zkopírujte připojovací řetězec hello.</span><span class="sxs-lookup"><span data-stu-id="2470f-125">Go back toohello Azure portal in your app's **Connection Info** page and copy hello connection string.</span></span>

    ![][4]
7. <span data-ttu-id="2470f-126">Přidejte následující řádek kódu hello vaše **AppDelegate.m** souboru.</span><span class="sxs-lookup"><span data-stu-id="2470f-126">Add hello following line of code in your **AppDelegate.m** file.</span></span>

        #import "EngagementAgent.h"
8. <span data-ttu-id="2470f-127">Nyní vložte připojovací řetězec hello hello `didFinishLaunchingWithOptions` delegovat.</span><span class="sxs-lookup"><span data-stu-id="2470f-127">Now paste hello connection string in hello `didFinishLaunchingWithOptions` delegate.</span></span>

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
              [...]   
              [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
              [...]
        }
9. <span data-ttu-id="2470f-128">`setTestLogEnabled`je volitelný příkaz, který protokolům SDK pro vás tooidentify problémy.</span><span class="sxs-lookup"><span data-stu-id="2470f-128">`setTestLogEnabled` is an optional statement which enables SDK logs for you tooidentify issues.</span></span>

## <span data-ttu-id="2470f-129"><a id="monitor"></a>Povolení sledování v reálném čase</span><span class="sxs-lookup"><span data-stu-id="2470f-129"><a id="monitor"></a>Enable real-time monitoring</span></span>
<span data-ttu-id="2470f-130">V pořadí toostart odesílat data a zajistit, že hello uživatelé jsou aktivní musíte odeslat alespoň jednu obrazovku (aktivitu) toohello Mobile Engagement back-end.</span><span class="sxs-lookup"><span data-stu-id="2470f-130">In order toostart sending data and ensuring that hello users are active, you must send at least one screen (Activity) toohello Mobile Engagement backend.</span></span>

1. <span data-ttu-id="2470f-131">Otevřete hello **ViewController.h** souboru a import **EngagementViewController.h**:</span><span class="sxs-lookup"><span data-stu-id="2470f-131">Open hello **ViewController.h** file and import **EngagementViewController.h**:</span></span>

    `#import "EngagementViewController.h"`
2. <span data-ttu-id="2470f-132">Nyní nahraďte super třídu hello hello **ViewController** rozhraní podle `EngagementViewController`:</span><span class="sxs-lookup"><span data-stu-id="2470f-132">Now replace hello super class of hello **ViewController** interface by `EngagementViewController`:</span></span>

    `@interface ViewController : EngagementViewController`

## <span data-ttu-id="2470f-133"><a id="monitor"></a>Připojení aplikace se sledováním v reálném čase</span><span class="sxs-lookup"><span data-stu-id="2470f-133"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="2470f-134"><a id="integrate-push"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci</span><span class="sxs-lookup"><span data-stu-id="2470f-134"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="2470f-135">Mobile Engagement umožňuje toointeract s uživateli a REACH s nabízená oznámení a zasílání zpráv v kontextu hello kampaní v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2470f-135">Mobile Engagement allows you toointeract with your users and REACH with push notifications and in-app messaging in hello context of campaigns.</span></span> <span data-ttu-id="2470f-136">Tento modul je hello portálu Mobile Engagement nazývá REACH.</span><span class="sxs-lookup"><span data-stu-id="2470f-136">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="2470f-137">Hello následujících sekcích nastavíte tooreceive vaše aplikace je.</span><span class="sxs-lookup"><span data-stu-id="2470f-137">hello following sections set up your app tooreceive them.</span></span>

### <a name="enable-your-app-tooreceive-silent-push-notifications"></a><span data-ttu-id="2470f-138">Povolit vaší aplikace tooreceive bezobslužných nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="2470f-138">Enable your app tooreceive Silent Push Notifications</span></span>
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-hello-reach-library-tooyour-project"></a><span data-ttu-id="2470f-139">Přidání projektu tooyour knihovny Reach hello</span><span class="sxs-lookup"><span data-stu-id="2470f-139">Add hello Reach library tooyour project</span></span>
1. <span data-ttu-id="2470f-140">Klikněte pravým tlačítkem na projekt.</span><span class="sxs-lookup"><span data-stu-id="2470f-140">Right-click your project.</span></span>
2. <span data-ttu-id="2470f-141">Vyberte **Add file to** (Přidat soubor).</span><span class="sxs-lookup"><span data-stu-id="2470f-141">Select **Add file to**.</span></span>
3. <span data-ttu-id="2470f-142">Přejděte toohello složky, které jste extrahovali hello SDK.</span><span class="sxs-lookup"><span data-stu-id="2470f-142">Navigate toohello folder where you extracted hello SDK.</span></span>
4. <span data-ttu-id="2470f-143">Vyberte hello `EngagementReach` složky.</span><span class="sxs-lookup"><span data-stu-id="2470f-143">Select hello `EngagementReach` folder.</span></span>
5. <span data-ttu-id="2470f-144">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="2470f-144">Click **Add**.</span></span>

### <a name="modify-your-application-delegate"></a><span data-ttu-id="2470f-145">Úprava delegáta aplikace</span><span class="sxs-lookup"><span data-stu-id="2470f-145">Modify your Application Delegate</span></span>
1. <span data-ttu-id="2470f-146">Zpět v **AppDeletegate.m** souboru, importujte modul Engagement Reach hello.</span><span class="sxs-lookup"><span data-stu-id="2470f-146">Back in **AppDeletegate.m** file, import hello Engagement Reach module.</span></span>

        #import "AEReachModule.h"
        #import <UserNotifications/UserNotifications.h>
2. <span data-ttu-id="2470f-147">Uvnitř hello `application:didFinishLaunchingWithOptions` metoda, vytvořte modul kampaně Reach a předejte jej tooyour existujícího inicializačního řádku Engagement:</span><span class="sxs-lookup"><span data-stu-id="2470f-147">Inside hello `application:didFinishLaunchingWithOptions` method, create a Reach module and pass it tooyour existing Engagement initialization line:</span></span>

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

### <a name="enable-your-app-tooreceive-apns-push-notifications"></a><span data-ttu-id="2470f-148">Povolit vaší aplikace tooreceive nabízených oznámení APNS</span><span class="sxs-lookup"><span data-stu-id="2470f-148">Enable your app tooreceive APNS Push Notifications</span></span>
1. <span data-ttu-id="2470f-149">Přidejte následující řádek toohello hello `application:didFinishLaunchingWithOptions` metoda:</span><span class="sxs-lookup"><span data-stu-id="2470f-149">Add hello following line toohello `application:didFinishLaunchingWithOptions` method:</span></span>

        if (NSFoundationVersionNumber >= NSFoundationVersionNumber_iOS_8_0)
        {
            if (NSFoundationVersionNumber > NSFoundationVersionNumber_iOS_9_x_Max)
            {
                [UNUserNotificationCenter.currentNotificationCenter requestAuthorizationWithOptions:(UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionAlert) completionHandler:^(BOOL granted, NSError * _Nullable error) {}];
            }else
            {
                [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert)   categories:nil]];
            }
            [application registerForRemoteNotifications];
        }
        else
        {
            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }
2. <span data-ttu-id="2470f-150">Přidat hello `application:didRegisterForRemoteNotificationsWithDeviceToken` metoda následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2470f-150">Add hello `application:didRegisterForRemoteNotificationsWithDeviceToken` method as follows:</span></span>

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
             [[EngagementAgent shared] registerDeviceToken:deviceToken];
            NSLog(@"Registered Token: %@", deviceToken);
        }
3. <span data-ttu-id="2470f-151">Přidat hello `didFailToRegisterForRemoteNotificationsWithError` metoda následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2470f-151">Add hello `didFailToRegisterForRemoteNotificationsWithError` method as follows:</span></span>

        - (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
        {
           NSLog(@"Failed tooget token, error: %@", error);
        }
4. <span data-ttu-id="2470f-152">Přidat hello `didReceiveRemoteNotification:fetchCompletionHandler` metoda následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2470f-152">Add hello `didReceiveRemoteNotification:fetchCompletionHandler` method as follows:</span></span>

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
        {
            [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
        }

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
