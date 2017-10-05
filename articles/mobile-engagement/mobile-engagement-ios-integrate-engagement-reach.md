---
title: "Azure Mobile Engagement iOS dosáhnout integraci sady SDK | Microsoft Docs"
description: "Nejnovější aktualizace a postupy pro iOS SDK pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 1f5f5857-867c-40c5-9d76-675a343a0296
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 12/13/2016
ms.author: piyushjo
ms.openlocfilehash: ba74e0c442ac10f096d465f989e03d2ceae8cd88
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-integrate-engagement-reach-on-ios"></a><span data-ttu-id="a68b5-103">Jak integrovat Engagement Reach v systému iOS</span><span class="sxs-lookup"><span data-stu-id="a68b5-103">How to Integrate Engagement Reach on iOS</span></span>
<span data-ttu-id="a68b5-104">Je třeba provést postup integrace popsaný v tématu [jak integrovat Engagement iOS dokumentu](mobile-engagement-ios-integrate-engagement.md) před těchto pokynů.</span><span class="sxs-lookup"><span data-stu-id="a68b5-104">You must follow the integration procedure described in the [How to Integrate Engagement on iOS document](mobile-engagement-ios-integrate-engagement.md) before following this guide.</span></span>

<span data-ttu-id="a68b5-105">Tato dokumentace vyžaduje XCode 8.</span><span class="sxs-lookup"><span data-stu-id="a68b5-105">This documentation requires XCode 8.</span></span> <span data-ttu-id="a68b5-106">Pokud jste ve skutečnosti závisí na XCode 7 pak můžete použít [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span><span class="sxs-lookup"><span data-stu-id="a68b5-106">If you really depend on XCode 7 then you may use the [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span></span> <span data-ttu-id="a68b5-107">Je známého problému na tato předchozí verze při spuštění v zařízení s iOS 10: systémová oznámení nejsou reagovali.</span><span class="sxs-lookup"><span data-stu-id="a68b5-107">There is a known bug on this previous version while running on iOS 10 devices:  system notifications are not actioned.</span></span> <span data-ttu-id="a68b5-108">Chcete-li tomu máte k implementaci nepoužívané rozhraní API `application:didReceiveRemoteNotification:` ve vaší aplikaci delegovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a68b5-108">To fix this you will have to implement the deprecated API `application:didReceiveRemoteNotification:` in your app delegate as follows:</span></span>

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [!IMPORTANT]
> <span data-ttu-id="a68b5-109">**Toto řešení nedoporučujeme** jako toto chování můžete změnit v jakéhokoli upgradu verze iOS nadcházející (i dílčí), protože tato rozhraní API pro iOS je zastaralý.</span><span class="sxs-lookup"><span data-stu-id="a68b5-109">**We do not recommend this workaround** as this behavior can change in any upcoming (even minor) iOS version upgrade because this iOS API is deprecated.</span></span> <span data-ttu-id="a68b5-110">Můžete přepnout do XCode 8 co nejdříve.</span><span class="sxs-lookup"><span data-stu-id="a68b5-110">You should switch to XCode 8 as soon as possible.</span></span>
>
>

### <a name="enable-your-app-to-receive-silent-push-notifications"></a><span data-ttu-id="a68b5-111">Povolení přijímání bezobslužných nabízených oznámení v aplikaci</span><span class="sxs-lookup"><span data-stu-id="a68b5-111">Enable your app to receive Silent Push Notifications</span></span>
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

## <a name="integration-steps"></a><span data-ttu-id="a68b5-112">Kroky integrace</span><span class="sxs-lookup"><span data-stu-id="a68b5-112">Integration steps</span></span>
### <a name="embed-the-engagement-reach-sdk-into-your-ios-project"></a><span data-ttu-id="a68b5-113">Vložení Engagement Reach SDK do projektu iOS</span><span class="sxs-lookup"><span data-stu-id="a68b5-113">Embed the Engagement Reach SDK into your iOS project</span></span>
* <span data-ttu-id="a68b5-114">Přidejte sady Reach sdk do projektu Xcode.</span><span class="sxs-lookup"><span data-stu-id="a68b5-114">Add the Reach sdk in your Xcode project.</span></span> <span data-ttu-id="a68b5-115">V prostředí Xcode přejděte na **projektu \> přidat do projektu** a zvolte `EngagementReach` složky.</span><span class="sxs-lookup"><span data-stu-id="a68b5-115">In Xcode, go to **Project \> Add to project** and choose the `EngagementReach` folder.</span></span>

### <a name="modify-your-application-delegate"></a><span data-ttu-id="a68b5-116">Úprava delegáta aplikace</span><span class="sxs-lookup"><span data-stu-id="a68b5-116">Modify your Application Delegate</span></span>
* <span data-ttu-id="a68b5-117">V horní části souboru implementace importujte modul Engagement Reach:</span><span class="sxs-lookup"><span data-stu-id="a68b5-117">At the top of your implementation file, import the Engagement Reach module:</span></span>

      [...]
      #import "AEReachModule.h"
* <span data-ttu-id="a68b5-118">Uvnitř metody `applicationDidFinishLaunching:` nebo `application:didFinishLaunchingWithOptions:`, vytvořte modul kampaně reach a předejte jej do existujícího inicializačního řádku Engagement:</span><span class="sxs-lookup"><span data-stu-id="a68b5-118">Inside method `applicationDidFinishLaunching:` or `application:didFinishLaunchingWithOptions:`, create a reach module and pass it to your existing Engagement initialization line:</span></span>

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
        [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
        [...]

        return YES;
      }
* <span data-ttu-id="a68b5-119">Upravit **'icon.png'** řetězec s názvem image chcete použít jako ikona oznámení.</span><span class="sxs-lookup"><span data-stu-id="a68b5-119">Modify **'icon.png'** string with the image name you want as your notification icon.</span></span>
* <span data-ttu-id="a68b5-120">Pokud budete chtít použít možnost *aktualizace oznámení "BADGE" hodnota* v kampaně Reach nebo pokud chcete použít nativního nabízení \<SaaS/Reach API nebo kampaň formátu/nativní nabízená\> kampaní, je nutné nechat modul Reach spravovat oznámení "BADGE" Ikona samotné (bude automaticky vymaže oznámení aplikace a také obnovit s hodnotou uloženou ve Engagement pokaždé, když je aplikace spuštěna nebo foregrounded).</span><span class="sxs-lookup"><span data-stu-id="a68b5-120">If you want to use the option *Update badge value* in Reach campaigns or if you want to use native push \</SaaS/Reach API/Campaign format/Native Push\> campaigns, you must let the Reach module manage the badge icon itself (it will automatically clear the application badge and also reset the value stored by Engagement every time the application is started or foregrounded).</span></span> <span data-ttu-id="a68b5-121">K tomu je potřeba po inicializaci modulu Reach přidáte tento řádek:</span><span class="sxs-lookup"><span data-stu-id="a68b5-121">This is done by adding the following line after Reach module initialization:</span></span>

      [reach setAutoBadgeEnabled:YES];
* <span data-ttu-id="a68b5-122">Pokud chcete zpracovávat Reach datová oznámení, je nutné nechat delegáta aplikace požadavkům `AEReachDataPushDelegate` protokolu.</span><span class="sxs-lookup"><span data-stu-id="a68b5-122">If you want to handle Reach data push, you must let your Application delegate conform to the `AEReachDataPushDelegate` protocol.</span></span> <span data-ttu-id="a68b5-123">Po inicializaci modulu Reach přidejte následující řádek:</span><span class="sxs-lookup"><span data-stu-id="a68b5-123">Add the following line after Reach module initialization:</span></span>

      [reach setDataPushDelegate:self];
* <span data-ttu-id="a68b5-124">Pak můžete implementovat metody `onDataPushStringReceived:` a `onDataPushBase64ReceivedWithDecodedBody:andEncodedBody:` v delegáta aplikace:</span><span class="sxs-lookup"><span data-stu-id="a68b5-124">Then you can implement the methods `onDataPushStringReceived:` and `onDataPushBase64ReceivedWithDecodedBody:andEncodedBody:` in your application delegate:</span></span>

      -(BOOL)didReceiveStringDataPushWithCategory:(NSString*)category body:(NSString*)body
      {
         NSLog(@"String data push message with category <%@> received: %@", category, body);
         return YES;
      }

      -(BOOL)didReceiveBase64DataPushWithCategory:(NSString*)category decodedBody:(NSData *)decodedBody encodedBody:(NSString *)encodedBody
      {
         NSLog(@"Base64 data push message with category <%@> received: %@", category, encodedBody);
         // Do something useful with decodedBody like updating an image view
         return YES;
      }

### <a name="category"></a><span data-ttu-id="a68b5-125">Kategorie</span><span class="sxs-lookup"><span data-stu-id="a68b5-125">Category</span></span>
<span data-ttu-id="a68b5-126">Kategorie parametr je volitelný při vytváření kampaň nabízených Data a umožňuje že vám data filtru nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="a68b5-126">The category parameter is optional when you create a Data Push campaign and allows you to filter data pushes.</span></span> <span data-ttu-id="a68b5-127">To je užitečné, pokud chcete push různých druhů z `Base64` dat a chcete určit jejich typů před analýzou je.</span><span class="sxs-lookup"><span data-stu-id="a68b5-127">This is useful if you want to push different kinds of `Base64` data and want to identify their type before parsing them.</span></span>

<span data-ttu-id="a68b5-128">**Aplikace je nyní připraven přijmout a zobrazit obsah reach!**</span><span class="sxs-lookup"><span data-stu-id="a68b5-128">**Your application is now ready to receive and display reach contents!**</span></span>

## <a name="how-to-receive-announcements-and-polls-at-any-time"></a><span data-ttu-id="a68b5-129">Jak přijmout oznámení a hlasování kdykoli</span><span class="sxs-lookup"><span data-stu-id="a68b5-129">How to receive announcements and polls at any time</span></span>
<span data-ttu-id="a68b5-130">Zapojení můžete koncovým uživatelům odeslat oznámení Reach kdykoli pomocí služby Apple Push Notification Service.</span><span class="sxs-lookup"><span data-stu-id="a68b5-130">Engagement can send Reach notifications to your end users at any time by using the Apple Push Notification Service.</span></span>

<span data-ttu-id="a68b5-131">Chcete-li povolit tuto funkci, budete muset připravit aplikace pro Apple nabízená oznámení a úprava delegáta aplikace.</span><span class="sxs-lookup"><span data-stu-id="a68b5-131">To enable this functionality, you'll have to prepare your application for Apple push notifications and modify your application delegate.</span></span>

### <a name="prepare-your-application-for-apple-push-notifications"></a><span data-ttu-id="a68b5-132">Příprava aplikace pro nabízená oznámení Apple</span><span class="sxs-lookup"><span data-stu-id="a68b5-132">Prepare your application for Apple push notifications</span></span>
<span data-ttu-id="a68b5-133">Postupujte podle průvodce: [postup přípravy aplikace pro nabízená oznámení Apple](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)</span><span class="sxs-lookup"><span data-stu-id="a68b5-133">Please follow the guide : [How to Prepare your Application for Apple Push Notifications](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)</span></span>

### <a name="add-the-necessary-client-code"></a><span data-ttu-id="a68b5-134">Přidejte kód klienta</span><span class="sxs-lookup"><span data-stu-id="a68b5-134">Add the necessary client code</span></span>
<span data-ttu-id="a68b5-135">*Aplikace v tomto okamžiku měli registrované nabízeného certifikátu Apple front-endu zapojení.*</span><span class="sxs-lookup"><span data-stu-id="a68b5-135">*At this point your application should have a registered Apple push certificate in the Engagement frontend.*</span></span>

<span data-ttu-id="a68b5-136">Pokud už není potřeba, budete muset registraci aplikace na příjem nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="a68b5-136">If it's not done already, you need to register your application to receive push notifications.</span></span>

* <span data-ttu-id="a68b5-137">Import `User Notification` framework:</span><span class="sxs-lookup"><span data-stu-id="a68b5-137">Import the `User Notification` framework:</span></span>

        #import <UserNotifications/UserNotifications.h>
* <span data-ttu-id="a68b5-138">Přidejte následující řádek při spuštění aplikace (obvykle ve `application:didFinishLaunchingWithOptions:`):</span><span class="sxs-lookup"><span data-stu-id="a68b5-138">Add the following line when your application starts (typically in `application:didFinishLaunchingWithOptions:`):</span></span>

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

<span data-ttu-id="a68b5-139">Pak musíte poskytnout Engagement token zařízení vrácený Apple servery.</span><span class="sxs-lookup"><span data-stu-id="a68b5-139">Then, You need to provide to Engagement the device token returned by Apple servers.</span></span> <span data-ttu-id="a68b5-140">To se provádí v metodu s názvem `application:didRegisterForRemoteNotificationsWithDeviceToken:` v delegáta aplikace:</span><span class="sxs-lookup"><span data-stu-id="a68b5-140">This is done in the method named `application:didRegisterForRemoteNotificationsWithDeviceToken:` in your application delegate:</span></span>

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
        [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

<span data-ttu-id="a68b5-141">Nakonec budete muset informovat sady Engagement SDK, až aplikace obdrží upozornění na vzdálené.</span><span class="sxs-lookup"><span data-stu-id="a68b5-141">Finally, you have to inform the Engagement SDK when your application receives a remote notification.</span></span> <span data-ttu-id="a68b5-142">K tomu, volejte metodu `applicationDidReceiveRemoteNotification:fetchCompletionHandler:` v delegáta aplikace:</span><span class="sxs-lookup"><span data-stu-id="a68b5-142">To do that, call the method `applicationDidReceiveRemoteNotification:fetchCompletionHandler:` in your application delegate:</span></span>

    - (void)application:(UIApplication*)application didReceiveRemoteNotification:(NSDictionary*)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

> [!IMPORTANT]
> <span data-ttu-id="a68b5-143">Ve výchozím nastavení ovládací prvky completionHandler Engagement Reach.</span><span class="sxs-lookup"><span data-stu-id="a68b5-143">By default, Engagement Reach controls the completionHandler.</span></span> <span data-ttu-id="a68b5-144">Pokud chcete ručně reagovat na `handler` blokovat ve vašem kódu, můžete předat hodnotu nil `handler` argument a řízení dokončení blokovat sami.</span><span class="sxs-lookup"><span data-stu-id="a68b5-144">If you want to manually respond to the `handler` block in your code, you can pass nil for the `handler` argument and control the completion block yourself.</span></span> <span data-ttu-id="a68b5-145">Najdete v článku `UIBackgroundFetchResult` typ seznam možných hodnot.</span><span class="sxs-lookup"><span data-stu-id="a68b5-145">See the `UIBackgroundFetchResult` type for a list of possible values.</span></span>
>
>

### <a name="full-example"></a><span data-ttu-id="a68b5-146">Úplný příklad</span><span class="sxs-lookup"><span data-stu-id="a68b5-146">Full example</span></span>
<span data-ttu-id="a68b5-147">Zde je úplná ukázka integrace:</span><span class="sxs-lookup"><span data-stu-id="a68b5-147">Here is a full example of integration:</span></span>

    #pragma mark -
    #pragma mark Application lifecycle

    - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
    {
      /* Reach module */
      AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
      [reach setAutoBadgeEnabled:YES];

      /* Engagement initialization */
      [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
      [[EngagementAgent shared] setPushDelegate:self];

      /* Views */
      [window addSubview:[tabBarController view]];
      [window makeKeyAndVisible];

      [application registerForRemoteNotificationTypes:UIRemoteNotificationTypeAlert|UIRemoteNotificationTypeBadge|UIRemoteNotificationTypeSound];
      return YES;
    }

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
      [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

### <a name="resolve-unusernotificationcenter-delegate-conflicts"></a><span data-ttu-id="a68b5-148">Řešení konfliktů UNUserNotificationCenter delegáta</span><span class="sxs-lookup"><span data-stu-id="a68b5-148">Resolve UNUserNotificationCenter delegate conflicts</span></span>

<span data-ttu-id="a68b5-149">*Pokud se implementuje ani aplikace, nebo jeden z vašich knihovnách třetích stran `UNUserNotificationCenterDelegate` pak můžete tuto část přeskočit.*</span><span class="sxs-lookup"><span data-stu-id="a68b5-149">*If neither your application or one of your third party libraries implements a `UNUserNotificationCenterDelegate` then you can skip this part.*</span></span>

<span data-ttu-id="a68b5-150">A `UNUserNotificationCenter` delegáta se sadou SDK používá k monitorování životní cyklus Engagement oznámení na zařízení se systémem iOS, 10 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="a68b5-150">A `UNUserNotificationCenter` delegate is used by the SDK to monitor the life cycle of Engagement notifications on devices running on iOS 10 or greater.</span></span> <span data-ttu-id="a68b5-151">Sada SDK obsahuje vlastní implementaci `UNUserNotificationCenterDelegate` protokolu, ale může být jen jedna `UNUserNotificationCenter` delegovat na aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a68b5-151">The SDK has its own implementation of the `UNUserNotificationCenterDelegate` protocol but there can be only one `UNUserNotificationCenter` delegate per application.</span></span> <span data-ttu-id="a68b5-152">Všechny ostatní delegáta přidán do `UNUserNotificationCenter` budou v konfliktu s zapojení jeden objekt.</span><span class="sxs-lookup"><span data-stu-id="a68b5-152">Any other delegate added to the `UNUserNotificationCenter` object will conflict with the Engagement one.</span></span> <span data-ttu-id="a68b5-153">Pokud sada SDK zjistí nebo jakékoli jiné třetí strany delegáta jej nebude získáte možnost vyřešte konflikty používat vlastní implementaci.</span><span class="sxs-lookup"><span data-stu-id="a68b5-153">If the SDK detects your or any other third party's delegate then it will not use its own implementation to give you a chance to resolve the conflicts.</span></span> <span data-ttu-id="a68b5-154">Budete muset přidat logiku Engagement vlastní delegáta za účelem vyřešení konfliktů.</span><span class="sxs-lookup"><span data-stu-id="a68b5-154">You will have to add the Engagement logic to your own delegate in order to resolve the conflicts.</span></span>

<span data-ttu-id="a68b5-155">Existují dva způsoby, jak dosáhnout.</span><span class="sxs-lookup"><span data-stu-id="a68b5-155">There are two ways to achieve this.</span></span>

<span data-ttu-id="a68b5-156">Návrh 1, jednoduše tak, že předávání delegát volání sady SDK:</span><span class="sxs-lookup"><span data-stu-id="a68b5-156">Proposal 1, simply by forwarding your delegate calls to the SDK:</span></span>

    #import <UIKit/UIKit.h>
    #import "EngagementAgent.h"
    #import <UserNotifications/UserNotifications.h>


    @interface MyAppDelegate : NSObject <UIApplicationDelegate, UNUserNotificationCenterDelegate>
    @end

    @implementation MyAppDelegate

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterWillPresentNotification:notification withCompletionHandler:completionHandler]
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterDidReceiveNotificationResponse:response withCompletionHandler:completionHandler]
    }
    @end

<span data-ttu-id="a68b5-157">Návrh 2, která dědí z nebo `AEUserNotificationHandler` – třída</span><span class="sxs-lookup"><span data-stu-id="a68b5-157">Or proposal 2, by inheriting from the `AEUserNotificationHandler` class</span></span>

    #import "AEUserNotificationHandler.h"
    #import "EngagementAgent.h"

    @interface CustomUserNotificationHandler :AEUserNotificationHandler
    @end

    @implementation CustomUserNotificationHandler

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center willPresentNotification:notification withCompletionHandler:completionHandler];
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse: UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
    }

    @end

> [!NOTE]
> <span data-ttu-id="a68b5-158">Můžete určit, zda oznámení pochází z Engagement nebo není předáním jeho `userInfo` slovníku agentovi `isEngagementPushPayload:` třídy metoda.</span><span class="sxs-lookup"><span data-stu-id="a68b5-158">You can determine whether a notification comes from Engagement or not by passing its `userInfo` dictionary to the Agent `isEngagementPushPayload:` class method.</span></span>

<span data-ttu-id="a68b5-159">Ujistěte se, že `UNUserNotificationCenter` objektu delegáta je nastaven na vaše delegáta v rámci buď `application:willFinishLaunchingWithOptions:` nebo `application:didFinishLaunchingWithOptions:` metoda delegáta aplikace.</span><span class="sxs-lookup"><span data-stu-id="a68b5-159">Make sure that the `UNUserNotificationCenter` object's delegate is set to your delegate within either the `application:willFinishLaunchingWithOptions:` or the `application:didFinishLaunchingWithOptions:` method of your application delegate.</span></span>
<span data-ttu-id="a68b5-160">Například pokud jste implementovali výše uvedený návrh 1:</span><span class="sxs-lookup"><span data-stu-id="a68b5-160">For instance, if you implemented the above proposal 1:</span></span>

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        // Any other code

        [UNUserNotificationCenter currentNotificationCenter].delegate = self;
        return YES;
      }

## <a name="how-to-customize-campaigns"></a><span data-ttu-id="a68b5-161">Postup přizpůsobení kampaně</span><span class="sxs-lookup"><span data-stu-id="a68b5-161">How to customize campaigns</span></span>
### <a name="notifications"></a><span data-ttu-id="a68b5-162">Oznámení</span><span class="sxs-lookup"><span data-stu-id="a68b5-162">Notifications</span></span>
<span data-ttu-id="a68b5-163">Existují dva typy oznámení: oznámení systému a v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a68b5-163">There are two types of notifications: system and in-app notifications.</span></span>

<span data-ttu-id="a68b5-164">Systémová oznámení jsou zpracovávány iOS a nelze upravovat.</span><span class="sxs-lookup"><span data-stu-id="a68b5-164">System notifications are handled by iOS, and cannot be customized.</span></span>

<span data-ttu-id="a68b5-165">Oznámení v aplikaci probíhají zobrazení, která se dynamicky přidá do okna aktuální aplikace.</span><span class="sxs-lookup"><span data-stu-id="a68b5-165">In-app notifications are made of a view that is dynamically added to the current application window.</span></span> <span data-ttu-id="a68b5-166">Tomu se říká překrytí oznámení.</span><span class="sxs-lookup"><span data-stu-id="a68b5-166">This is called a notification overlay.</span></span> <span data-ttu-id="a68b5-167">Překryvy oznámení jsou skvělý pro rychlé integraci, protože se nevyžaduje upravit všechna zobrazení v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a68b5-167">Notification overlays are great for a fast integration because they does not require you to modify any view in your application.</span></span>

#### <a name="layout"></a><span data-ttu-id="a68b5-168">Rozložení</span><span class="sxs-lookup"><span data-stu-id="a68b5-168">Layout</span></span>
<span data-ttu-id="a68b5-169">Pokud chcete upravit vzhled oznámení v aplikaci, můžete jednoduše upravit soubor `AENotificationView.xib` vašim potřebám, pokud ponechat hodnoty značky a typy dílčích existující zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a68b5-169">To modify the look of your in-app notifications, you can simply modify the file `AENotificationView.xib` to your needs, as long as you keep the tag values and types of the existing subviews.</span></span>

<span data-ttu-id="a68b5-170">Ve výchozím nastavení jsou oznámení v aplikaci zobrazí v dolní části obrazovky.</span><span class="sxs-lookup"><span data-stu-id="a68b5-170">By default, in-app notifications are presented at the bottom of the screen.</span></span> <span data-ttu-id="a68b5-171">Pokud chcete zobrazit v horní části obrazovky, upravit poskytnutého `AENotificationView.xib` a změňte `AutoSizing` vlastnosti hlavní zobrazení, může být uchováván v horní části jeho superview.</span><span class="sxs-lookup"><span data-stu-id="a68b5-171">If you prefer to display them at the top of screen, edit the provided `AENotificationView.xib` and change the `AutoSizing` property of the main view so it can be kept at the top of its superview.</span></span>

#### <a name="categories"></a><span data-ttu-id="a68b5-172">Kategorie</span><span class="sxs-lookup"><span data-stu-id="a68b5-172">Categories</span></span>
<span data-ttu-id="a68b5-173">Při úpravě zadané rozložení upravíte vzhledu všechna oznámení.</span><span class="sxs-lookup"><span data-stu-id="a68b5-173">When you modify the provided layout, you modify the look of all your notifications.</span></span> <span data-ttu-id="a68b5-174">Kategorie umožňují definovat různé cílové vypadá (pravděpodobně chování) pro oznámení.</span><span class="sxs-lookup"><span data-stu-id="a68b5-174">Categories allow you to define various targeted looks (possibly behaviors) for notifications.</span></span> <span data-ttu-id="a68b5-175">Kategorie lze při vytváření kampaně Reach.</span><span class="sxs-lookup"><span data-stu-id="a68b5-175">A category can be specified when you create a Reach campaign.</span></span> <span data-ttu-id="a68b5-176">Mějte na paměti, že kategorie vám také umožní přizpůsobit oznámení a hlasování, který je popsán dále v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="a68b5-176">Keep in mind that categories also let you customize announcements and polls, that is described later in this document.</span></span>

<span data-ttu-id="a68b5-177">Chcete-li zaregistrovat kategorie obslužnou rutinu pro oznámení, přidejte volání, jakmile se inicializovat modul reach.</span><span class="sxs-lookup"><span data-stu-id="a68b5-177">To register a category handler for your notifications, you need to add a call once the reach module is initialized.</span></span>

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:@"my_category"];
    ...

<span data-ttu-id="a68b5-178">`myNotifier`musí být instancí objektu, který vyhovuje protokolu `AENotifier`.</span><span class="sxs-lookup"><span data-stu-id="a68b5-178">`myNotifier` must be an instance of an object that conforms to the protocol `AENotifier`.</span></span>

<span data-ttu-id="a68b5-179">Můžete implementovat metodu protokolu pomocí sami nebo můžete přeimplementovat existující třída `AEDefaultNotifier` kterém již provádí většinu činností.</span><span class="sxs-lookup"><span data-stu-id="a68b5-179">You can implement the protocol methods by yourself or you can choose to reimplement the existing class `AEDefaultNotifier` which already performs most of the work.</span></span>

<span data-ttu-id="a68b5-180">Například pokud chcete znovu definovat zobrazení oznámení pro konkrétní kategorii, proveďte tento příklad:</span><span class="sxs-lookup"><span data-stu-id="a68b5-180">For example, if you want to redefine the notification view for a specific category, you can follow this example:</span></span>

    #import "AEDefaultNotifier.h"
    #import "AENotificationView.h"
    @interface MyNotifier : AEDefaultNotifier
    @end

    @implementation MyNotifier

    -(NSString*)nibNameForCategory:(NSString*)category
    {
      return "MyNotificationView";
    }

    @end

<span data-ttu-id="a68b5-181">Tento jednoduchý příklad kategorie Předpokládejme, že máte soubor s názvem `MyNotificationView.xib` ve vaší sady hlavní aplikace.</span><span class="sxs-lookup"><span data-stu-id="a68b5-181">This simple example of category assume that you have a file named `MyNotificationView.xib` in your main application bundle.</span></span> <span data-ttu-id="a68b5-182">Pokud není schopen najít odpovídající metodu `.xib`, se nezobrazí oznámení a Engagement na výstup zprávu, v konzole.</span><span class="sxs-lookup"><span data-stu-id="a68b5-182">If the method is not able to find a corresponding `.xib`, the notification will not be displayed and Engagement will output a message in the console.</span></span>

<span data-ttu-id="a68b5-183">Soubor poskytnutý nib by měly dodržovat následující pravidla:</span><span class="sxs-lookup"><span data-stu-id="a68b5-183">The provided nib file should respect the following rules:</span></span>

* <span data-ttu-id="a68b5-184">Měl by obsahovat pouze jedno zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a68b5-184">It should only contain one view.</span></span>
* <span data-ttu-id="a68b5-185">Dílčích zobrazení by měl být typů stejné jako ty, které v zadané nib souboru s názvem`AENotificationView.xib`</span><span class="sxs-lookup"><span data-stu-id="a68b5-185">Subviews should be of the same types as the ones inside the provided nib file named `AENotificationView.xib`</span></span>
* <span data-ttu-id="a68b5-186">Dílčích zobrazení musí mít stejné značky jako ty, které uvnitř poskytnutého nib soubor s názvem`AENotificationView.xib`</span><span class="sxs-lookup"><span data-stu-id="a68b5-186">Subviews should have the same tags as the ones inside the provided nib file named `AENotificationView.xib`</span></span>

> [!TIP]
> <span data-ttu-id="a68b5-187">Jednoduše zkopírujete soubor zadaný nib, s názvem `AENotificationView.xib`a mohli začít pracovat z ní.</span><span class="sxs-lookup"><span data-stu-id="a68b5-187">Just copy the provided nib file, named `AENotificationView.xib`, and start working from there.</span></span> <span data-ttu-id="a68b5-188">Ale buďte opatrní, zobrazení uvnitř tohoto souboru nib je přidružena k třídě `AENotificationView`.</span><span class="sxs-lookup"><span data-stu-id="a68b5-188">But be careful, the view inside this nib file is associated to the class `AENotificationView`.</span></span> <span data-ttu-id="a68b5-189">Tato třída předefinovat metodu `layoutSubViews` přesouvat a měnit velikost jeho dílčích zobrazení podle kontextu.</span><span class="sxs-lookup"><span data-stu-id="a68b5-189">This class redefined the method `layoutSubViews` to move and resize its subviews according to context.</span></span> <span data-ttu-id="a68b5-190">Můžete chtít nahraďte ho `UIView` nebo vlastní zobrazení třída.</span><span class="sxs-lookup"><span data-stu-id="a68b5-190">You may want to replace it with an `UIView` or you custom view class.</span></span>
>
>

<span data-ttu-id="a68b5-191">Pokud potřebujete podrobnější přizpůsobení oznámení (Pokud například chcete načíst zobrazení přímo z kódu), doporučuje se podívejte se na v zadaný zdroj kódu a třída dokumentaci `Protocol ReferencesDefaultNotifier` a `AENotifier`.</span><span class="sxs-lookup"><span data-stu-id="a68b5-191">If you need deeper customization of your notifications(if you want for instance to load your view directly from the code), it is recommended to take a look at the provided source code and class documentation of `Protocol ReferencesDefaultNotifier` and `AENotifier`.</span></span>

<span data-ttu-id="a68b5-192">Všimněte si, že můžete použít stejné oznamovatel pro více kategorií.</span><span class="sxs-lookup"><span data-stu-id="a68b5-192">Note that you can use the same notifier for multiple categories.</span></span>

<span data-ttu-id="a68b5-193">Můžete také předefinovat oznamovatel výchozí takto:</span><span class="sxs-lookup"><span data-stu-id="a68b5-193">You can also redefined the default notifier like this:</span></span>

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:kAEReachDefaultCategory];

##### <a name="notification-handling"></a><span data-ttu-id="a68b5-194">Zpracování oznámení</span><span class="sxs-lookup"><span data-stu-id="a68b5-194">Notification handling</span></span>
<span data-ttu-id="a68b5-195">Při použití výchozí kategorie, se nazývají některé metody životní cyklus na `AEReachContent` do sestavy statistik objektu a aktualizovat stav kampaně:</span><span class="sxs-lookup"><span data-stu-id="a68b5-195">When using the default category, some life cycle methods are called on the `AEReachContent` object to report statistics and update the campaign state:</span></span>

* <span data-ttu-id="a68b5-196">Když se zobrazí oznámení v aplikaci `displayNotification` metoda je volána (který sestavy statistik) podle `AEReachModule` Pokud `handleNotification:` vrátí `YES`.</span><span class="sxs-lookup"><span data-stu-id="a68b5-196">When the notification is displayed in application, the `displayNotification` method is called (which reports statistics) by `AEReachModule` if `handleNotification:` returns `YES`.</span></span>
* <span data-ttu-id="a68b5-197">Pokud se zavře oznámení, `exitNotification` metoda je volána, statistiky se použije v hlášení a další kampaně lze nyní zpracovat.</span><span class="sxs-lookup"><span data-stu-id="a68b5-197">If the notification is dismissed, the `exitNotification` method is called, statistic is reported and next campaigns can now be processed.</span></span>
* <span data-ttu-id="a68b5-198">Po kliknutí na oznámení `actionNotification` je volána, statistiky se použije v hlášení a přidružené akce se provádí.</span><span class="sxs-lookup"><span data-stu-id="a68b5-198">If the notification is clicked, `actionNotification` is called, statistic is reported and the associated action is performed.</span></span>

<span data-ttu-id="a68b5-199">Pokud vaši implementaci `AENotifier` obchází výchozí chování, budete muset pro volání těchto metod životní cyklus sami.</span><span class="sxs-lookup"><span data-stu-id="a68b5-199">If your implementation of `AENotifier` bypasses the default behavior, you have to call these life cycle methods by yourself.</span></span> <span data-ttu-id="a68b5-200">Následující příklady ilustrují některých případech, kde přeskočí výchozí chování:</span><span class="sxs-lookup"><span data-stu-id="a68b5-200">The following examples illustrate some cases where the default behavior is bypassed:</span></span>

* <span data-ttu-id="a68b5-201">Nerozšíříte `AEDefaultNotifier`, například implementována kategorie zpracování od začátku.</span><span class="sxs-lookup"><span data-stu-id="a68b5-201">You don't extend `AEDefaultNotifier`, e.g. you implemented category handling from scratch.</span></span>
* <span data-ttu-id="a68b5-202">Můžete overrode `prepareNotificationView:forContent:`, je nutné namapovat minimálně `onNotificationActioned` nebo `onNotificationExited` na jednu z vaší U.I ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="a68b5-202">You overrode `prepareNotificationView:forContent:`, be sure to map at least `onNotificationActioned` or `onNotificationExited` to one of your U.I controls.</span></span>

> [!WARNING]
> <span data-ttu-id="a68b5-203">Pokud `handleNotification:` vyvolá výjimku, obsah se odstraní a `drop` je volána, to je uvedený v statistiky a další kampaně lze nyní zpracovat.</span><span class="sxs-lookup"><span data-stu-id="a68b5-203">If `handleNotification:` throws an exception, the content is deleted and `drop` is called, this is reported in statistics and next campaigns can now be processed.</span></span>
>
>

#### <a name="include-notification-as-part-of-an-existing-view"></a><span data-ttu-id="a68b5-204">Zahrnout oznámení jako součást stávající zobrazení</span><span class="sxs-lookup"><span data-stu-id="a68b5-204">Include notification as part of an existing view</span></span>
<span data-ttu-id="a68b5-205">Překryvy se výborně hodí pro rychlé integrace, ale může být někdy není vhodné nebo může mít nežádoucí vedlejší účinky.</span><span class="sxs-lookup"><span data-stu-id="a68b5-205">Overlays are great for a fast integration but can be sometimes not convenient, or can have unwanted side effects.</span></span>

<span data-ttu-id="a68b5-206">Pokud nejste s systému překrytí v některé z vaší zobrazení, můžete jej přizpůsobit pro tato zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a68b5-206">If you're not satisfied with the overlay system in some of your views, you can customize it for these views.</span></span>

<span data-ttu-id="a68b5-207">Můžete rozhodnout pro zahrnutí naše rozložení oznámení do vaší stávající zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a68b5-207">You can decide to include our notification layout in your existing views.</span></span> <span data-ttu-id="a68b5-208">To pokud chcete udělat, je k dispozici dva styly implementace:</span><span class="sxs-lookup"><span data-stu-id="a68b5-208">To do so, there is two implementation styles:</span></span>

1. <span data-ttu-id="a68b5-209">Přidání zobrazení oznámení pomocí rozhraní tvůrce</span><span class="sxs-lookup"><span data-stu-id="a68b5-209">Add the notification view using interface builder</span></span>

   * <span data-ttu-id="a68b5-210">Otevřete *rozhraní tvůrce*</span><span class="sxs-lookup"><span data-stu-id="a68b5-210">Open *Interface Builder*</span></span>
   * <span data-ttu-id="a68b5-211">Umístěte 320 x 60 (nebo 768 x 60, pokud jste na zařízení iPad) `UIView` místo, kam chcete zobrazit oznámení</span><span class="sxs-lookup"><span data-stu-id="a68b5-211">Place a 320x60 (or 768x60 if you are on iPad) `UIView` where you want the notification to appear</span></span>
   * <span data-ttu-id="a68b5-212">Nastavit hodnotu značky pro toto zobrazení: **36822491**</span><span class="sxs-lookup"><span data-stu-id="a68b5-212">Set the Tag value for this view to : **36822491**</span></span>
2. <span data-ttu-id="a68b5-213">Přidání zobrazení oznámení prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="a68b5-213">Add the notification view programmatically.</span></span> <span data-ttu-id="a68b5-214">Pokud byl inicializován zobrazení právě přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="a68b5-214">Just add the following code when your view has been initialized:</span></span>

       UIView* notificationView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 60)]; //Replace x and y coordinate values to your needs.
       notificationView.tag = NOTIFICATION_AREA_VIEW_TAG;
       [self.view addSubview:notificationView];

<span data-ttu-id="a68b5-215">`NOTIFICATION_AREA_VIEW_TAG`Makro lze nalézt v `AEDefaultNotifier.h`.</span><span class="sxs-lookup"><span data-stu-id="a68b5-215">`NOTIFICATION_AREA_VIEW_TAG` macro can be found in `AEDefaultNotifier.h`.</span></span>

> [!NOTE]
> <span data-ttu-id="a68b5-216">Výchozí oznamovatel automaticky zjistí, že rozložení oznámení je zahrnuta v tomto zobrazení a nebude pro něj přidat překrytí.</span><span class="sxs-lookup"><span data-stu-id="a68b5-216">The default notifier automatically detects that the notification layout is included in this view and will not add an overlay for it.</span></span>
>
>

### <a name="announcements-and-polls"></a><span data-ttu-id="a68b5-217">Oznámení a hlasování</span><span class="sxs-lookup"><span data-stu-id="a68b5-217">Announcements and polls</span></span>
#### <a name="layouts"></a><span data-ttu-id="a68b5-218">Rozložení</span><span class="sxs-lookup"><span data-stu-id="a68b5-218">Layouts</span></span>
<span data-ttu-id="a68b5-219">Můžete upravit soubory `AEDefaultAnnouncementView.xib` a `AEDefaultPollView.xib` tak dlouho, dokud ponechat hodnoty značky a typy dílčích existující zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a68b5-219">You can modify the files `AEDefaultAnnouncementView.xib` and `AEDefaultPollView.xib` as long as you keep the tag values and types of the existing subviews.</span></span>

#### <a name="categories"></a><span data-ttu-id="a68b5-220">Kategorie</span><span class="sxs-lookup"><span data-stu-id="a68b5-220">Categories</span></span>
##### <a name="alternate-layouts"></a><span data-ttu-id="a68b5-221">Alternativní rozložení</span><span class="sxs-lookup"><span data-stu-id="a68b5-221">Alternate layouts</span></span>
<span data-ttu-id="a68b5-222">Jako oznámení kategorie kampaně umožňuje mít alternativní rozložení pro oznámení a hlasování.</span><span class="sxs-lookup"><span data-stu-id="a68b5-222">Like notifications, the campaign's category can be used to have alternate layouts for your announcements and polls.</span></span>

<span data-ttu-id="a68b5-223">Pokud chcete vytvořit kategorii pro oznámení, musíte rozšířit **AEAnnouncementViewController** a zaregistrovat ji, jakmile byla inicializována modul reach:</span><span class="sxs-lookup"><span data-stu-id="a68b5-223">To create a category for an announcement, you must extend **AEAnnouncementViewController** and register it once the reach module has been initialized:</span></span>

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:@"my_category"];

> [!NOTE]
> <span data-ttu-id="a68b5-224">Pokaždé, když se uživatel kliknutím na oznámení pro oznámení v kategorii "Moje\_kategorie", řadiči registrované zobrazení (v takovém případě `MyCustomAnnouncementViewController`) budou inicializována pomocí volání metody `initWithAnnouncement:` a zobrazení se zařadí do aktuální okno aplikace.</span><span class="sxs-lookup"><span data-stu-id="a68b5-224">Each time a user will click on a notification for an announcement with the category "my\_category", your registered view controller (in that case `MyCustomAnnouncementViewController`) will be initialized by calling the method `initWithAnnouncement:` and the view will be added to the current application window.</span></span>
>
>

<span data-ttu-id="a68b5-225">V implementaci `AEAnnouncementViewController` třída budete muset čtení vlastnosti `announcement` k chybě při inicializaci vaší dílčích zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a68b5-225">In your implementation of the `AEAnnouncementViewController` class you will have to read the property `announcement` to initialize your subviews.</span></span> <span data-ttu-id="a68b5-226">Podívejte se na příklad níže, kde dva popisků jsou inicializována pomocí `title` a `body` vlastnosti `AEReachAnnouncement` třídy:</span><span class="sxs-lookup"><span data-stu-id="a68b5-226">Consider the example below, where two labels are initialized using `title` and `body` properties of the `AEReachAnnouncement` class:</span></span>

    -(void)loadView
    {
        [super loadView];

        UILabel* titleLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        titleLabel.font = [UIFont systemFontOfSize:32.0];
        titleLabel.text = self.announcement.title;

        UILabel* bodyLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        bodyLabel.font = [UIFont systemFontOfSize:24.0];
        bodyLabel.text = self.announcement.body;

        [self.view addSubview:titleLabel];
        [self.view addSubview:bodyLabel];
    }

<span data-ttu-id="a68b5-227">Pokud nechcete, aby se načíst zobrazení samotnými, ale chcete znovu použít výchozí rozložení zobrazení oznámení, můžete jednoduše provést řadiči vlastní zobrazení rozšiřuje třídu zadaný `AEDefaultAnnouncementViewController`.</span><span class="sxs-lookup"><span data-stu-id="a68b5-227">If you don't want to load your views by yourself but you just want to reuse the default announcement view layout, you can simply make your custom view controller extends the provided class `AEDefaultAnnouncementViewController`.</span></span> <span data-ttu-id="a68b5-228">V takovém případě duplicitní soubor nib `AEDefaultAnnouncementView.xib` a přejmenujte ji, mohou být načteny ve vašem řadiči vlastní zobrazení (pro řadič s názvem `CustomAnnouncementViewController`, by měly volat souboru nib `CustomAnnouncementView.xib`).</span><span class="sxs-lookup"><span data-stu-id="a68b5-228">In that case, duplicate the nib file `AEDefaultAnnouncementView.xib` and rename it so it can be loaded by your custom view controller (for a controller named `CustomAnnouncementViewController`, you should call your nib file `CustomAnnouncementView.xib`).</span></span>

<span data-ttu-id="a68b5-229">Pokud chcete nahradit výchozí kategorii oznámení, jednoduše zaregistrovat řadiči vlastní zobrazení pro kategorii definované v `kAEReachDefaultCategory`:</span><span class="sxs-lookup"><span data-stu-id="a68b5-229">To replace the default category of announcements, simply register your custom view controller for the category defined in `kAEReachDefaultCategory`:</span></span>

    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:kAEReachDefaultCategory];

<span data-ttu-id="a68b5-230">Hlasování lze upravit stejným způsobem jako:</span><span class="sxs-lookup"><span data-stu-id="a68b5-230">Polls can be customized the same way :</span></span>

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerPollController:[MyCustomPollViewController class] forCategory:@"my_category"];

<span data-ttu-id="a68b5-231">Tentokrát, poskytnutého `MyCustomPollViewController` musíte rozšířit `AEPollViewController`.</span><span class="sxs-lookup"><span data-stu-id="a68b5-231">This time, the provided `MyCustomPollViewController` must extend `AEPollViewController`.</span></span> <span data-ttu-id="a68b5-232">Nebo můžete rozšířit z výchozí řadiče: `AEDefaultPollViewController`.</span><span class="sxs-lookup"><span data-stu-id="a68b5-232">Or you can choose to extend from the default controller: `AEDefaultPollViewController`.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a68b5-233">Nezapomeňte volat buď `action` (`submitAnswers:` pro vlastní dotazování řadiče zobrazení) nebo `exit` metoda před řadiče zobrazení se zavře.</span><span class="sxs-lookup"><span data-stu-id="a68b5-233">Don't forget to call either `action` (`submitAnswers:` for custom poll view controllers) or `exit` method before the view controller is dismissed.</span></span> <span data-ttu-id="a68b5-234">Jinak statistiky nebude odeslána (tj. žádné analýzy kampaň) a další je nejdůležitější další kampaně nebudete upozorněni, až po restartování procesu aplikace.</span><span class="sxs-lookup"><span data-stu-id="a68b5-234">Otherwise, statistics won't be sent (i.e. no analytics on the campaign) and more importantly next campaigns will not be notified until the application process is restarted.</span></span>
>
>

##### <a name="implementation-example"></a><span data-ttu-id="a68b5-235">Příklad implementace</span><span class="sxs-lookup"><span data-stu-id="a68b5-235">Implementation example</span></span>
<span data-ttu-id="a68b5-236">V této implementaci vlastní oznámení zobrazení je načten z externí xib souboru.</span><span class="sxs-lookup"><span data-stu-id="a68b5-236">In this implementation the custom announcement view is loaded from an external xib file.</span></span>

<span data-ttu-id="a68b5-237">Jako pro přizpůsobení pokročilé oznámení, doporučujeme prohlédnout si zdrojový kód standardní implementace.</span><span class="sxs-lookup"><span data-stu-id="a68b5-237">Like for advanced notification customization, it is recommended to look at the source code of the standard implementation.</span></span>

`CustomAnnouncementViewController.h`

    //Interface
    @interface CustomAnnouncementViewController : AEAnnouncementViewController {
      UILabel* titleLabel;
      UITextView* descTextView;
      UIWebView* htmlWebView;
      UIButton* okButton;
      UIButton* cancelButton;
    }

    @property (nonatomic, retain) IBOutlet UILabel* titleLabel;
    @property (nonatomic, retain) IBOutlet UITextView* descTextView;
    @property (nonatomic, retain) IBOutlet UIWebView* htmlWebView;
    @property (nonatomic, retain) IBOutlet UIButton* okButton;
    @property (nonatomic, retain) IBOutlet UIButton* cancelButton;

    -(IBAction)okButtonClicked:(id)sender;
    -(IBAction)cancelButtonClicked:(id)sender;

`CustomAnnouncementViewController.m`

    //Implementation
    @implementation CustomAnnouncementViewController
    @synthesize titleLabel;
    @synthesize descTextView;
    @synthesize htmlWebView;
    @synthesize okButton;
    @synthesize cancelButton;

    -(id)initWithAnnouncement:(AEReachAnnouncement*)anAnnouncement
    {
      self = [super initWithNibName:@"CustomAnnouncementViewController" bundle:nil];
      if (self != nil) {
        self.announcement = anAnnouncement;
      }
      return self;
    }

    - (void) dealloc
    {
      [titleLabel release];
      [descTextView release];
      [htmlWebView release];
      [okButton release];
      [cancelButton release];
      [super dealloc];
    }

    - (void)viewDidLoad {
      [super viewDidLoad];

      /* Init announcement title */
      titleLabel.text = self.announcement.title;

      /* Init announcement body */
      if(self.announcement.type == AEAnnouncementTypeHtml)
      {
        titleLabel.hidden = YES;
        htmlWebView.hidden = NO;
        [htmlWebView loadHTMLString:self.announcement.body baseURL:[NSURL URLWithString:@"http://localhost/"]];
      }
      else
      {
        titleLabel.hidden = NO;
        htmlWebView.hidden = YES;
        descTextView.text = self.announcement.body;
      }

      /* Set action button label */
      if([self.announcement.actionLabel length] > 0)
        [okButton setTitle:self.announcement.actionLabel forState:UIControlStateNormal];

      /* Set exit button label */
      if([self.announcement.exitLabel length] > 0)
        [cancelButton setTitle:self.announcement.exitLabel forState:UIControlStateNormal];
    }

    #pragma mark Actions

    -(IBAction)okButtonClicked:(id)sender
    {
        [self action];
    }

    -(IBAction)cancelButtonClicked:(id)sender
    {
        [self exit];
    }

    @end
