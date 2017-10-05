---
title: "Začínáme s Azure Mobile Engagementem pro iOS v Objective C | Dokumentace Microsoftu"
description: "Naučte se používat Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro aplikace pro iOS."
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
ms.openlocfilehash: 1b87a2ebb35b31ee3d3139ecead6267e62eb1033
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-objective-c"></a><span data-ttu-id="f93c8-103">Začínáme s Azure Mobile Engagementem pro aplikace pro iOS v Objective C</span><span class="sxs-lookup"><span data-stu-id="f93c8-103">Get Started with Azure Mobile Engagement for iOS apps in Objective C</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="f93c8-104">V tomto tématu si ukážeme, jak používat Azure Mobile Engagement, jak porozumět používání aplikace a jak odesílat nabízená oznámení segmentovaným uživatelům aplikace pro iOS.</span><span class="sxs-lookup"><span data-stu-id="f93c8-104">This topic shows you how to use Azure Mobile Engagement to understand your app usage and send push notifications to segmented users to an iOS application.</span></span>
<span data-ttu-id="f93c8-105">V tomto kurzu vytvoříte prázdnou aplikaci iOS, která bude shromažďovat základní data a přijímat nabízená oznámení pomocí systému nabízených oznámení Apple (APNS). </span><span class="sxs-lookup"><span data-stu-id="f93c8-105">In this tutorial, you create a blank iOS app that collects basic data and receives push notifications using Apple Push Notification System (APNS).</span></span>

<span data-ttu-id="f93c8-106">V tomto kurzu budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="f93c8-106">This tutorial requires the following:</span></span>

* <span data-ttu-id="f93c8-107">XCode 8, který si můžete nainstalovat z MAC App Storu</span><span class="sxs-lookup"><span data-stu-id="f93c8-107">XCode 8, which you can install from your MAC App Store</span></span>
* <span data-ttu-id="f93c8-108">[Mobile Engagement iOS SDK]</span><span class="sxs-lookup"><span data-stu-id="f93c8-108">the [Mobile Engagement iOS SDK]</span></span>

<span data-ttu-id="f93c8-109">Ve všech dalších kurzech k Mobile Engagement týkajících se aplikací pro iOS se předpokládá dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="f93c8-109">Completing this tutorial is a prerequisite for all other Mobile Engagement tutorials for iOS apps.</span></span>

> [!NOTE]
> <span data-ttu-id="f93c8-110">K dokončení tohoto kurzu potřebujete mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="f93c8-110">To complete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="f93c8-111">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="f93c8-111">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="f93c8-112">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="f93c8-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).</span></span>
>
>

## <span data-ttu-id="f93c8-113"><a id="setup-azme"></a>Nastavení Mobile Engagementu pro vaši aplikaci pro iOS</span><span class="sxs-lookup"><span data-stu-id="f93c8-113"><a id="setup-azme"></a>Setup Mobile Engagement for your iOS app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="f93c8-114"><a id="connecting-app"></a>Připojení aplikace k back-endu Mobile Engagementu</span><span class="sxs-lookup"><span data-stu-id="f93c8-114"><a id="connecting-app"></a>Connect your app to the Mobile Engagement backend</span></span>
<span data-ttu-id="f93c8-115">V tomto kurzu si představíme „základní integraci“, čili minimální sadu požadovanou pro shromažďování dat a odesílání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="f93c8-115">This tutorial presents a "basic integration", which is the minimal set required to collect data and send a push notification.</span></span> <span data-ttu-id="f93c8-116">Kompletní dokumentaci k integraci najdete v článku [Integrace sady Mobile Engagement iOS SDK](mobile-engagement-ios-sdk-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f93c8-116">The complete integration documentation can be found in the [Mobile Engagement iOS SDK integration](mobile-engagement-ios-sdk-overview.md)</span></span>

<span data-ttu-id="f93c8-117">Pomocí XCodu si vytvoříme základní aplikaci, na které si tuto integraci předvedeme.</span><span class="sxs-lookup"><span data-stu-id="f93c8-117">We will create a basic app with XCode to demonstrate the integration.</span></span>

### <a name="create-a-new-ios-project"></a><span data-ttu-id="f93c8-118">Vytvoření nového projektu iOS</span><span class="sxs-lookup"><span data-stu-id="f93c8-118">Create a new iOS project</span></span>
[!INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

### <a name="connect-your-app-to-the-mobile-engagement-backend"></a><span data-ttu-id="f93c8-119">Připojení aplikace k back-endu Mobile Engagementu</span><span class="sxs-lookup"><span data-stu-id="f93c8-119">Connect your app to the Mobile Engagement backend</span></span>
1. <span data-ttu-id="f93c8-120">Stáhněte si [Mobile Engagement iOS SDK].</span><span class="sxs-lookup"><span data-stu-id="f93c8-120">Download the [Mobile Engagement iOS SDK].</span></span>
2. <span data-ttu-id="f93c8-121">Extrahujte soubor .tar.gz do složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="f93c8-121">Extract the .tar.gz file to a folder in your computer.</span></span>
3. <span data-ttu-id="f93c8-122">Pravým tlačítkem myši klikněte na projekt a pak vyberte **Přidat soubory do**.</span><span class="sxs-lookup"><span data-stu-id="f93c8-122">Right-click the project, and then select **Add files to**.</span></span>

    ![][1]
4. <span data-ttu-id="f93c8-123">Přejděte do složky, do které jste extrahovali sadu SDK, vyberte složku `EngagementSDK`, klikněte na **Možnosti** v levém dolním rohu, ujistěte se, že je zaškrtnuté políčko **Kopírovat položky v případě potřeby** a políčko pro váš cíl a stiskněte **OK**.</span><span class="sxs-lookup"><span data-stu-id="f93c8-123">Navigate to the folder where you extracted the SDK, select the `EngagementSDK` folder, click **Options** on the bottom left corner and make sure that the checkbox **Copy items if needed** and the checkbox for your target are checked then press **OK**.</span></span>

    ![][2]
5. <span data-ttu-id="f93c8-124">Otevřete kartu **Build Phases** (Fáze sestavení) a v nabídce **Link Binary With Libraries** (Propojit binární kód s knihovnami) přidejte rozhraní, jak je zobrazeno níže:</span><span class="sxs-lookup"><span data-stu-id="f93c8-124">Open the **Build Phases** tab, and in the **Link Binary With Libraries** menu, add the frameworks as shown below:</span></span>

    ![][3]
6. <span data-ttu-id="f93c8-125">Přejděte zpět na portál Azure na stránku **Connection Info** (Informace o připojení) vaší aplikace a zkopírujte připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="f93c8-125">Go back to the Azure portal in your app's **Connection Info** page and copy the connection string.</span></span>

    ![][4]
7. <span data-ttu-id="f93c8-126">Do souboru **AppDelegate.m** přidejte následující řádek kódu.</span><span class="sxs-lookup"><span data-stu-id="f93c8-126">Add the following line of code in your **AppDelegate.m** file.</span></span>

        #import "EngagementAgent.h"
8. <span data-ttu-id="f93c8-127">Nyní vložte připojovací řetězec do delegáta `didFinishLaunchingWithOptions`.</span><span class="sxs-lookup"><span data-stu-id="f93c8-127">Now paste the connection string in the `didFinishLaunchingWithOptions` delegate.</span></span>

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
              [...]   
              [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
              [...]
        }
9. <span data-ttu-id="f93c8-128">`setTestLogEnabled`je volitelný příkaz, který protokolům SDK umožňuje identifikovat problémy.</span><span class="sxs-lookup"><span data-stu-id="f93c8-128">`setTestLogEnabled` is an optional statement which enables SDK logs for you to identify issues.</span></span>

## <span data-ttu-id="f93c8-129"><a id="monitor"></a>Povolení sledování v reálném čase</span><span class="sxs-lookup"><span data-stu-id="f93c8-129"><a id="monitor"></a>Enable real-time monitoring</span></span>
<span data-ttu-id="f93c8-130">Pokud chcete začít odesílat data a zajistit, že uživatelé jsou aktivní, musíte odeslat alespoň jednu obrazovku (aktivitu) na back-end Mobile Engagementu.</span><span class="sxs-lookup"><span data-stu-id="f93c8-130">In order to start sending data and ensuring that the users are active, you must send at least one screen (Activity) to the Mobile Engagement backend.</span></span>

1. <span data-ttu-id="f93c8-131">Otevřete soubor **ViewController.h** a importujte **EngagementViewController.h**:</span><span class="sxs-lookup"><span data-stu-id="f93c8-131">Open the **ViewController.h** file and import **EngagementViewController.h**:</span></span>

    `#import "EngagementViewController.h"`
2. <span data-ttu-id="f93c8-132">Nyní nahraďte super třídu rozhraní **ViewController** třídou `EngagementViewController`:</span><span class="sxs-lookup"><span data-stu-id="f93c8-132">Now replace the super class of the **ViewController** interface by `EngagementViewController`:</span></span>

    `@interface ViewController : EngagementViewController`

## <span data-ttu-id="f93c8-133"><a id="monitor"></a>Připojení aplikace se sledováním v reálném čase</span><span class="sxs-lookup"><span data-stu-id="f93c8-133"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="f93c8-134"><a id="integrate-push"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci</span><span class="sxs-lookup"><span data-stu-id="f93c8-134"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="f93c8-135">Mobile Engagement vám umožňuje v rámci kampaní oslovit uživatele a komunikovat s nimi prostřednictvím nabízených oznámení a zpráv v aplikacích.</span><span class="sxs-lookup"><span data-stu-id="f93c8-135">Mobile Engagement allows you to interact with your users and REACH with push notifications and in-app messaging in the context of campaigns.</span></span> <span data-ttu-id="f93c8-136">Tento modul se na portálu Mobile Engagement nazývá REACH.</span><span class="sxs-lookup"><span data-stu-id="f93c8-136">This module is called REACH in the Mobile Engagement portal.</span></span>
<span data-ttu-id="f93c8-137">V následujících sekcích nastavíte aplikaci, aby tato nabízená oznámení a zprávy přijímala.</span><span class="sxs-lookup"><span data-stu-id="f93c8-137">The following sections set up your app to receive them.</span></span>

### <a name="enable-your-app-to-receive-silent-push-notifications"></a><span data-ttu-id="f93c8-138">Povolení přijímání bezobslužných nabízených oznámení v aplikaci</span><span class="sxs-lookup"><span data-stu-id="f93c8-138">Enable your app to receive Silent Push Notifications</span></span>
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-the-reach-library-to-your-project"></a><span data-ttu-id="f93c8-139">Přidání knihovny Reach do projektu</span><span class="sxs-lookup"><span data-stu-id="f93c8-139">Add the Reach library to your project</span></span>
1. <span data-ttu-id="f93c8-140">Klikněte pravým tlačítkem na projekt.</span><span class="sxs-lookup"><span data-stu-id="f93c8-140">Right-click your project.</span></span>
2. <span data-ttu-id="f93c8-141">Vyberte **Add file to** (Přidat soubor).</span><span class="sxs-lookup"><span data-stu-id="f93c8-141">Select **Add file to**.</span></span>
3. <span data-ttu-id="f93c8-142">Přejděte do složky, do které jste extrahovali sadu SDK.</span><span class="sxs-lookup"><span data-stu-id="f93c8-142">Navigate to the folder where you extracted the SDK.</span></span>
4. <span data-ttu-id="f93c8-143">Vyberte složku `EngagementReach`.</span><span class="sxs-lookup"><span data-stu-id="f93c8-143">Select the `EngagementReach` folder.</span></span>
5. <span data-ttu-id="f93c8-144">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="f93c8-144">Click **Add**.</span></span>

### <a name="modify-your-application-delegate"></a><span data-ttu-id="f93c8-145">Úprava delegáta aplikace</span><span class="sxs-lookup"><span data-stu-id="f93c8-145">Modify your Application Delegate</span></span>
1. <span data-ttu-id="f93c8-146">V souboru **AppDeletegate.m** importujte modul Engagement Reach.</span><span class="sxs-lookup"><span data-stu-id="f93c8-146">Back in **AppDeletegate.m** file, import the Engagement Reach module.</span></span>

        #import "AEReachModule.h"
        #import <UserNotifications/UserNotifications.h>
2. <span data-ttu-id="f93c8-147">V metodě `application:didFinishLaunchingWithOptions` vytvořte modul kampaně Reach a předejte jej do existujícího inicializačního řádku Engagement:</span><span class="sxs-lookup"><span data-stu-id="f93c8-147">Inside the `application:didFinishLaunchingWithOptions` method, create a Reach module and pass it to your existing Engagement initialization line:</span></span>

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

### <a name="enable-your-app-to-receive-apns-push-notifications"></a><span data-ttu-id="f93c8-148">Povolení přijímání nabízených oznámení APNS v aplikaci</span><span class="sxs-lookup"><span data-stu-id="f93c8-148">Enable your app to receive APNS Push Notifications</span></span>
1. <span data-ttu-id="f93c8-149">Do metody `application:didFinishLaunchingWithOptions` přidejte následující řádek:</span><span class="sxs-lookup"><span data-stu-id="f93c8-149">Add the following line to the `application:didFinishLaunchingWithOptions` method:</span></span>

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
2. <span data-ttu-id="f93c8-150">Následujícím způsobem přidejte metodu `application:didRegisterForRemoteNotificationsWithDeviceToken`:</span><span class="sxs-lookup"><span data-stu-id="f93c8-150">Add the `application:didRegisterForRemoteNotificationsWithDeviceToken` method as follows:</span></span>

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
             [[EngagementAgent shared] registerDeviceToken:deviceToken];
            NSLog(@"Registered Token: %@", deviceToken);
        }
3. <span data-ttu-id="f93c8-151">Následujícím způsobem přidejte metodu `didFailToRegisterForRemoteNotificationsWithError`:</span><span class="sxs-lookup"><span data-stu-id="f93c8-151">Add the `didFailToRegisterForRemoteNotificationsWithError` method as follows:</span></span>

        - (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
        {
           NSLog(@"Failed to get token, error: %@", error);
        }
4. <span data-ttu-id="f93c8-152">Následujícím způsobem přidejte metodu `didReceiveRemoteNotification:fetchCompletionHandler`:</span><span class="sxs-lookup"><span data-stu-id="f93c8-152">Add the `didReceiveRemoteNotification:fetchCompletionHandler` method as follows:</span></span>

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
        {
            [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
        }

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
<span data-ttu-id="f93c8-153">[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj</span><span class="sxs-lookup"><span data-stu-id="f93c8-153">[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj</span></span>

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
