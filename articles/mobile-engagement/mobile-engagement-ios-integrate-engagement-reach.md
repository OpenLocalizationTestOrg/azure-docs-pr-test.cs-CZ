---
title: aaaAzure integraci sady Reach SDK Mobile Engagement iOS | Microsoft Docs
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
ms.openlocfilehash: 40c9bfbdb475ab0b97bdbc9cea798a59cb8a71ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-engagement-reach-on-ios"></a><span data-ttu-id="19727-103">Jak tooIntegrate Engagement Reach v systému iOS</span><span class="sxs-lookup"><span data-stu-id="19727-103">How tooIntegrate Engagement Reach on iOS</span></span>
<span data-ttu-id="19727-104">Je třeba postupovat podle hello integrace postup popsaný v hello [jak tooIntegrate Engagement iOS dokumentu](mobile-engagement-ios-integrate-engagement.md) před těchto pokynů.</span><span class="sxs-lookup"><span data-stu-id="19727-104">You must follow hello integration procedure described in hello [How tooIntegrate Engagement on iOS document](mobile-engagement-ios-integrate-engagement.md) before following this guide.</span></span>

<span data-ttu-id="19727-105">Tato dokumentace vyžaduje XCode 8.</span><span class="sxs-lookup"><span data-stu-id="19727-105">This documentation requires XCode 8.</span></span> <span data-ttu-id="19727-106">Pokud ve skutečnosti závisí na XCode 7 pak můžete použít hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span><span class="sxs-lookup"><span data-stu-id="19727-106">If you really depend on XCode 7 then you may use hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span></span> <span data-ttu-id="19727-107">Je známého problému na tato předchozí verze při spuštění v zařízení s iOS 10: systémová oznámení nejsou reagovali.</span><span class="sxs-lookup"><span data-stu-id="19727-107">There is a known bug on this previous version while running on iOS 10 devices:  system notifications are not actioned.</span></span> <span data-ttu-id="19727-108">toofix to budete mít tooimplement hello zastaralá rozhraní API `application:didReceiveRemoteNotification:` ve vaší aplikaci delegovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="19727-108">toofix this you will have tooimplement hello deprecated API `application:didReceiveRemoteNotification:` in your app delegate as follows:</span></span>

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [!IMPORTANT]
> <span data-ttu-id="19727-109">**Toto řešení nedoporučujeme** jako toto chování můžete změnit v jakéhokoli upgradu verze iOS nadcházející (i dílčí), protože tato rozhraní API pro iOS je zastaralý.</span><span class="sxs-lookup"><span data-stu-id="19727-109">**We do not recommend this workaround** as this behavior can change in any upcoming (even minor) iOS version upgrade because this iOS API is deprecated.</span></span> <span data-ttu-id="19727-110">TooXCode 8 musí přepnout co nejdříve.</span><span class="sxs-lookup"><span data-stu-id="19727-110">You should switch tooXCode 8 as soon as possible.</span></span>
>
>

### <a name="enable-your-app-tooreceive-silent-push-notifications"></a><span data-ttu-id="19727-111">Povolit vaší aplikace tooreceive bezobslužných nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="19727-111">Enable your app tooreceive Silent Push Notifications</span></span>
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

## <a name="integration-steps"></a><span data-ttu-id="19727-112">Kroky integrace</span><span class="sxs-lookup"><span data-stu-id="19727-112">Integration steps</span></span>
### <a name="embed-hello-engagement-reach-sdk-into-your-ios-project"></a><span data-ttu-id="19727-113">Vložení hello Engagement Reach SDK do projektu iOS</span><span class="sxs-lookup"><span data-stu-id="19727-113">Embed hello Engagement Reach SDK into your iOS project</span></span>
* <span data-ttu-id="19727-114">Přidejte hello Reach sdk do projektu Xcode.</span><span class="sxs-lookup"><span data-stu-id="19727-114">Add hello Reach sdk in your Xcode project.</span></span> <span data-ttu-id="19727-115">V prostředí Xcode přejděte příliš**projektu \> přidat tooproject** a zvolte hello `EngagementReach` složky.</span><span class="sxs-lookup"><span data-stu-id="19727-115">In Xcode, go too**Project \> Add tooproject** and choose hello `EngagementReach` folder.</span></span>

### <a name="modify-your-application-delegate"></a><span data-ttu-id="19727-116">Úprava delegáta aplikace</span><span class="sxs-lookup"><span data-stu-id="19727-116">Modify your Application Delegate</span></span>
* <span data-ttu-id="19727-117">Hello horní části souboru implementace importujte modul Engagement Reach hello:</span><span class="sxs-lookup"><span data-stu-id="19727-117">At hello top of your implementation file, import hello Engagement Reach module:</span></span>

      [...]
      #import "AEReachModule.h"
* <span data-ttu-id="19727-118">Uvnitř metody `applicationDidFinishLaunching:` nebo `application:didFinishLaunchingWithOptions:`, vytvořte modul kampaně reach a předejte jej tooyour existujícího inicializačního řádku Engagement:</span><span class="sxs-lookup"><span data-stu-id="19727-118">Inside method `applicationDidFinishLaunching:` or `application:didFinishLaunchingWithOptions:`, create a reach module and pass it tooyour existing Engagement initialization line:</span></span>

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
        [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
        [...]

        return YES;
      }
* <span data-ttu-id="19727-119">Upravit **'icon.png'** řetězec s názvem hello image chcete použít jako ikona oznámení.</span><span class="sxs-lookup"><span data-stu-id="19727-119">Modify **'icon.png'** string with hello image name you want as your notification icon.</span></span>
* <span data-ttu-id="19727-120">Pokud chcete, aby možnost hello toouse *aktualizace oznámení "BADGE" hodnota* v kampaně Reach nebo pokud chcete, aby toouse nativního nabízení \<SaaS/Reach API nebo kampaň formátu/nativní nabízená\> kampaní, je nutné nechat modul Reach hello spravovat Hello oznámení "BADGE" ikonu samotné (bude automaticky vymaže oznámení "BADGE" hello aplikace a také obnovit hello hodnotě uložené Engagement pokaždé, když je aplikace hello spuštěn nebo foregrounded).</span><span class="sxs-lookup"><span data-stu-id="19727-120">If you want toouse hello option *Update badge value* in Reach campaigns or if you want toouse native push \</SaaS/Reach API/Campaign format/Native Push\> campaigns, you must let hello Reach module manage hello badge icon itself (it will automatically clear hello application badge and also reset hello value stored by Engagement every time hello application is started or foregrounded).</span></span> <span data-ttu-id="19727-121">To se provádí přidáním hello po inicializaci modulu Reach následující řádek:</span><span class="sxs-lookup"><span data-stu-id="19727-121">This is done by adding hello following line after Reach module initialization:</span></span>

      [reach setAutoBadgeEnabled:YES];
* <span data-ttu-id="19727-122">Pokud chcete toohandle Reach datová oznámení, je nutné nechat delegáta aplikace odpovídat toohello `AEReachDataPushDelegate` protokolu.</span><span class="sxs-lookup"><span data-stu-id="19727-122">If you want toohandle Reach data push, you must let your Application delegate conform toohello `AEReachDataPushDelegate` protocol.</span></span> <span data-ttu-id="19727-123">Přidejte následující řádek po inicializaci modulu Reach hello:</span><span class="sxs-lookup"><span data-stu-id="19727-123">Add hello following line after Reach module initialization:</span></span>

      [reach setDataPushDelegate:self];
* <span data-ttu-id="19727-124">Pak můžete implementovat hello metody `onDataPushStringReceived:` a `onDataPushBase64ReceivedWithDecodedBody:andEncodedBody:` v delegáta aplikace:</span><span class="sxs-lookup"><span data-stu-id="19727-124">Then you can implement hello methods `onDataPushStringReceived:` and `onDataPushBase64ReceivedWithDecodedBody:andEncodedBody:` in your application delegate:</span></span>

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

### <a name="category"></a><span data-ttu-id="19727-125">Kategorie</span><span class="sxs-lookup"><span data-stu-id="19727-125">Category</span></span>
<span data-ttu-id="19727-126">Hello kategorie parametr je volitelný při vytváření kampaň nabízených Data a umožňuje že vám toofilter data nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="19727-126">hello category parameter is optional when you create a Data Push campaign and allows you toofilter data pushes.</span></span> <span data-ttu-id="19727-127">To je užitečné, pokud chcete různé typy toopush z `Base64` dat a chcete tooidentify jejich typů před analýzou je.</span><span class="sxs-lookup"><span data-stu-id="19727-127">This is useful if you want toopush different kinds of `Base64` data and want tooidentify their type before parsing them.</span></span>

<span data-ttu-id="19727-128">**Aplikace je nyní připraven tooreceive a zobrazení dosáhnout obsah!**</span><span class="sxs-lookup"><span data-stu-id="19727-128">**Your application is now ready tooreceive and display reach contents!**</span></span>

## <a name="how-tooreceive-announcements-and-polls-at-any-time"></a><span data-ttu-id="19727-129">Jak tooreceive oznámení a hlasování kdykoli</span><span class="sxs-lookup"><span data-stu-id="19727-129">How tooreceive announcements and polls at any time</span></span>
<span data-ttu-id="19727-130">Zapojení oznámení můžete odesílat Reach tooyour koncoví uživatelé kdykoli pomocí hello Apple Push Notification Service.</span><span class="sxs-lookup"><span data-stu-id="19727-130">Engagement can send Reach notifications tooyour end users at any time by using hello Apple Push Notification Service.</span></span>

<span data-ttu-id="19727-131">tooenable tuto funkci, budete mít tooprepare aplikace pro Apple nabízená oznámení a úprava delegáta aplikace.</span><span class="sxs-lookup"><span data-stu-id="19727-131">tooenable this functionality, you'll have tooprepare your application for Apple push notifications and modify your application delegate.</span></span>

### <a name="prepare-your-application-for-apple-push-notifications"></a><span data-ttu-id="19727-132">Příprava aplikace pro nabízená oznámení Apple</span><span class="sxs-lookup"><span data-stu-id="19727-132">Prepare your application for Apple push notifications</span></span>
<span data-ttu-id="19727-133">Postupujte podle průvodce hello: [jak tooPrepare aplikace pro nabízená oznámení Apple](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)</span><span class="sxs-lookup"><span data-stu-id="19727-133">Please follow hello guide : [How tooPrepare your Application for Apple Push Notifications](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)</span></span>

### <a name="add-hello-necessary-client-code"></a><span data-ttu-id="19727-134">Přidejte kód klienta hello</span><span class="sxs-lookup"><span data-stu-id="19727-134">Add hello necessary client code</span></span>
<span data-ttu-id="19727-135">*Aplikace v tomto okamžiku měli registrované nabízeného certifikátu Apple hello Engagement front-endu.*</span><span class="sxs-lookup"><span data-stu-id="19727-135">*At this point your application should have a registered Apple push certificate in hello Engagement frontend.*</span></span>

<span data-ttu-id="19727-136">Pokud už není potřeba, musíte tooregister aplikace tooreceive nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="19727-136">If it's not done already, you need tooregister your application tooreceive push notifications.</span></span>

* <span data-ttu-id="19727-137">Import hello `User Notification` framework:</span><span class="sxs-lookup"><span data-stu-id="19727-137">Import hello `User Notification` framework:</span></span>

        #import <UserNotifications/UserNotifications.h>
* <span data-ttu-id="19727-138">Přidejte následující řádek při spuštění aplikace hello (obvykle ve `application:didFinishLaunchingWithOptions:`):</span><span class="sxs-lookup"><span data-stu-id="19727-138">Add hello following line when your application starts (typically in `application:didFinishLaunchingWithOptions:`):</span></span>

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

<span data-ttu-id="19727-139">Pak musíte tooprovide tooEngagement hello zařízení token vrácený Apple servery.</span><span class="sxs-lookup"><span data-stu-id="19727-139">Then, You need tooprovide tooEngagement hello device token returned by Apple servers.</span></span> <span data-ttu-id="19727-140">To se provádí v hello metodu s názvem `application:didRegisterForRemoteNotificationsWithDeviceToken:` v delegáta aplikace:</span><span class="sxs-lookup"><span data-stu-id="19727-140">This is done in hello method named `application:didRegisterForRemoteNotificationsWithDeviceToken:` in your application delegate:</span></span>

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
        [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

<span data-ttu-id="19727-141">Nakonec máte tooinform hello Engagement SDK, když aplikace přijímá vzdáleného oznámení.</span><span class="sxs-lookup"><span data-stu-id="19727-141">Finally, you have tooinform hello Engagement SDK when your application receives a remote notification.</span></span> <span data-ttu-id="19727-142">toodo, které volat hello metoda `applicationDidReceiveRemoteNotification:fetchCompletionHandler:` v delegáta aplikace:</span><span class="sxs-lookup"><span data-stu-id="19727-142">toodo that, call hello method `applicationDidReceiveRemoteNotification:fetchCompletionHandler:` in your application delegate:</span></span>

    - (void)application:(UIApplication*)application didReceiveRemoteNotification:(NSDictionary*)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

> [!IMPORTANT]
> <span data-ttu-id="19727-143">Ve výchozím nastavení řídí Engagement Reach hello completionHandler.</span><span class="sxs-lookup"><span data-stu-id="19727-143">By default, Engagement Reach controls hello completionHandler.</span></span> <span data-ttu-id="19727-144">Pokud chcete, aby toomanually reakce toohello `handler` blokovat ve vašem kódu, můžete předat hodnotu nil pro hello `handler` argument a řízení dokončení hello blokovat sami.</span><span class="sxs-lookup"><span data-stu-id="19727-144">If you want toomanually respond toohello `handler` block in your code, you can pass nil for hello `handler` argument and control hello completion block yourself.</span></span> <span data-ttu-id="19727-145">V tématu hello `UIBackgroundFetchResult` typu seznam možných hodnot.</span><span class="sxs-lookup"><span data-stu-id="19727-145">See hello `UIBackgroundFetchResult` type for a list of possible values.</span></span>
>
>

### <a name="full-example"></a><span data-ttu-id="19727-146">Úplný příklad</span><span class="sxs-lookup"><span data-stu-id="19727-146">Full example</span></span>
<span data-ttu-id="19727-147">Zde je úplná ukázka integrace:</span><span class="sxs-lookup"><span data-stu-id="19727-147">Here is a full example of integration:</span></span>

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

### <a name="resolve-unusernotificationcenter-delegate-conflicts"></a><span data-ttu-id="19727-148">Řešení konfliktů UNUserNotificationCenter delegáta</span><span class="sxs-lookup"><span data-stu-id="19727-148">Resolve UNUserNotificationCenter delegate conflicts</span></span>

<span data-ttu-id="19727-149">*Pokud se implementuje ani aplikace, nebo jeden z vašich knihovnách třetích stran `UNUserNotificationCenterDelegate` pak můžete tuto část přeskočit.*</span><span class="sxs-lookup"><span data-stu-id="19727-149">*If neither your application or one of your third party libraries implements a `UNUserNotificationCenterDelegate` then you can skip this part.*</span></span>

<span data-ttu-id="19727-150">A `UNUserNotificationCenter` delegát používá hello SDK toomonitor hello životní cyklus Engagement oznámení na zařízení se systémem iOS, 10 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="19727-150">A `UNUserNotificationCenter` delegate is used by hello SDK toomonitor hello life cycle of Engagement notifications on devices running on iOS 10 or greater.</span></span> <span data-ttu-id="19727-151">Hello SDK má svou vlastní implementace hello `UNUserNotificationCenterDelegate` protokolu, ale může být jen jedna `UNUserNotificationCenter` delegovat na aplikaci.</span><span class="sxs-lookup"><span data-stu-id="19727-151">hello SDK has its own implementation of hello `UNUserNotificationCenterDelegate` protocol but there can be only one `UNUserNotificationCenter` delegate per application.</span></span> <span data-ttu-id="19727-152">Přidat další delegáta toohello `UNUserNotificationCenter` budou v konfliktu s hello Engagement jeden objekt.</span><span class="sxs-lookup"><span data-stu-id="19727-152">Any other delegate added toohello `UNUserNotificationCenter` object will conflict with hello Engagement one.</span></span> <span data-ttu-id="19727-153">Pokud hello SDK zjistí delegáta nebo jakékoli jiné třetí strany, pak jej nebude používat vlastní toogive implementaci vám tooresolve prvního hello je v konfliktu.</span><span class="sxs-lookup"><span data-stu-id="19727-153">If hello SDK detects your or any other third party's delegate then it will not use its own implementation toogive you a chance tooresolve hello conflicts.</span></span> <span data-ttu-id="19727-154">Budete mít tooadd hello Engagement logiku tooyour vlastní delegáta v pořadí tooresolve hello konflikty.</span><span class="sxs-lookup"><span data-stu-id="19727-154">You will have tooadd hello Engagement logic tooyour own delegate in order tooresolve hello conflicts.</span></span>

<span data-ttu-id="19727-155">Existují dva způsoby tooachieve to.</span><span class="sxs-lookup"><span data-stu-id="19727-155">There are two ways tooachieve this.</span></span>

<span data-ttu-id="19727-156">Návrh 1, jednoduše tak, že předávání delegát volání toohello SDK:</span><span class="sxs-lookup"><span data-stu-id="19727-156">Proposal 1, simply by forwarding your delegate calls toohello SDK:</span></span>

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

<span data-ttu-id="19727-157">Návrh 2, která dědí z hello nebo `AEUserNotificationHandler` – třída</span><span class="sxs-lookup"><span data-stu-id="19727-157">Or proposal 2, by inheriting from hello `AEUserNotificationHandler` class</span></span>

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
> <span data-ttu-id="19727-158">Můžete určit, zda oznámení pochází z Engagement nebo není předáním jeho `userInfo` slovník toohello agenta `isEngagementPushPayload:` třídy metoda.</span><span class="sxs-lookup"><span data-stu-id="19727-158">You can determine whether a notification comes from Engagement or not by passing its `userInfo` dictionary toohello Agent `isEngagementPushPayload:` class method.</span></span>

<span data-ttu-id="19727-159">Ujistěte se, že hello `UNUserNotificationCenter` objektu delegáta nastavena delegáta tooyour v rámci buď hello `application:willFinishLaunchingWithOptions:` nebo hello `application:didFinishLaunchingWithOptions:` metoda delegáta aplikace.</span><span class="sxs-lookup"><span data-stu-id="19727-159">Make sure that hello `UNUserNotificationCenter` object's delegate is set tooyour delegate within either hello `application:willFinishLaunchingWithOptions:` or hello `application:didFinishLaunchingWithOptions:` method of your application delegate.</span></span>
<span data-ttu-id="19727-160">Například pokud jste implementovali hello výše návrh 1:</span><span class="sxs-lookup"><span data-stu-id="19727-160">For instance, if you implemented hello above proposal 1:</span></span>

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        // Any other code

        [UNUserNotificationCenter currentNotificationCenter].delegate = self;
        return YES;
      }

## <a name="how-toocustomize-campaigns"></a><span data-ttu-id="19727-161">Jak toocustomize kampaně</span><span class="sxs-lookup"><span data-stu-id="19727-161">How toocustomize campaigns</span></span>
### <a name="notifications"></a><span data-ttu-id="19727-162">Oznámení</span><span class="sxs-lookup"><span data-stu-id="19727-162">Notifications</span></span>
<span data-ttu-id="19727-163">Existují dva typy oznámení: oznámení systému a v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="19727-163">There are two types of notifications: system and in-app notifications.</span></span>

<span data-ttu-id="19727-164">Systémová oznámení jsou zpracovávány iOS a nelze upravovat.</span><span class="sxs-lookup"><span data-stu-id="19727-164">System notifications are handled by iOS, and cannot be customized.</span></span>

<span data-ttu-id="19727-165">Oznámení v aplikaci probíhají zobrazení, která se dynamicky přidá toohello aktuální okno aplikace.</span><span class="sxs-lookup"><span data-stu-id="19727-165">In-app notifications are made of a view that is dynamically added toohello current application window.</span></span> <span data-ttu-id="19727-166">Tomu se říká překrytí oznámení.</span><span class="sxs-lookup"><span data-stu-id="19727-166">This is called a notification overlay.</span></span> <span data-ttu-id="19727-167">Překryvy oznámení jsou výborně hodí pro rychlé integraci, protože se nevyžaduje, aby vám toomodify všechna zobrazení v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="19727-167">Notification overlays are great for a fast integration because they does not require you toomodify any view in your application.</span></span>

#### <a name="layout"></a><span data-ttu-id="19727-168">Rozložení</span><span class="sxs-lookup"><span data-stu-id="19727-168">Layout</span></span>
<span data-ttu-id="19727-169">Vzhled hello toomodify oznámení v aplikaci, můžete jednoduše upravit soubor hello `AENotificationView.xib` tooyour potřebuje, dokud Udržovat hodnoty značky hello a typy dílčích hello existující zobrazení.</span><span class="sxs-lookup"><span data-stu-id="19727-169">toomodify hello look of your in-app notifications, you can simply modify hello file `AENotificationView.xib` tooyour needs, as long as you keep hello tag values and types of hello existing subviews.</span></span>

<span data-ttu-id="19727-170">Ve výchozím nastavení se zobrazí oznámení v aplikaci v hello dolní části obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="19727-170">By default, in-app notifications are presented at hello bottom of hello screen.</span></span> <span data-ttu-id="19727-171">Pokud dáváte přednost toodisplay je hello horní části obrazovky, upravit hello poskytuje `AENotificationView.xib` a změňte hello `AutoSizing` vlastnost hello hlavního zobrazení, může být udržována na hello začátku jeho superview.</span><span class="sxs-lookup"><span data-stu-id="19727-171">If you prefer toodisplay them at hello top of screen, edit hello provided `AENotificationView.xib` and change hello `AutoSizing` property of hello main view so it can be kept at hello top of its superview.</span></span>

#### <a name="categories"></a><span data-ttu-id="19727-172">Kategorie</span><span class="sxs-lookup"><span data-stu-id="19727-172">Categories</span></span>
<span data-ttu-id="19727-173">Při úpravě hello zadat rozložení upravíte vzhled hello všechna oznámení.</span><span class="sxs-lookup"><span data-stu-id="19727-173">When you modify hello provided layout, you modify hello look of all your notifications.</span></span> <span data-ttu-id="19727-174">Kategorie povolit, že jste toodefine, které se různé cílové hledá oznámení (pravděpodobně chování).</span><span class="sxs-lookup"><span data-stu-id="19727-174">Categories allow you toodefine various targeted looks (possibly behaviors) for notifications.</span></span> <span data-ttu-id="19727-175">Kategorie lze při vytváření kampaně Reach.</span><span class="sxs-lookup"><span data-stu-id="19727-175">A category can be specified when you create a Reach campaign.</span></span> <span data-ttu-id="19727-176">Mějte na paměti, že kategorie vám také umožní přizpůsobit oznámení a hlasování, který je popsán dále v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="19727-176">Keep in mind that categories also let you customize announcements and polls, that is described later in this document.</span></span>

<span data-ttu-id="19727-177">tooregister kategorie obslužnou rutinu pro oznámení, musíte tooadd inicializaci volání po dosažení hello modulu.</span><span class="sxs-lookup"><span data-stu-id="19727-177">tooregister a category handler for your notifications, you need tooadd a call once hello reach module is initialized.</span></span>

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:@"my_category"];
    ...

<span data-ttu-id="19727-178">`myNotifier`musí být instancí objektu, který vyhovuje protokolu toohello `AENotifier`.</span><span class="sxs-lookup"><span data-stu-id="19727-178">`myNotifier` must be an instance of an object that conforms toohello protocol `AENotifier`.</span></span>

<span data-ttu-id="19727-179">Hello protokol metody můžete implementovat podle sami nebo můžete zvolit existující třídy tooreimplement hello `AEDefaultNotifier` kterém již provádí většinu práce hello.</span><span class="sxs-lookup"><span data-stu-id="19727-179">You can implement hello protocol methods by yourself or you can choose tooreimplement hello existing class `AEDefaultNotifier` which already performs most of hello work.</span></span>

<span data-ttu-id="19727-180">Například pokud chcete zobrazit oznámení hello tooredefine pro určitou kategorii, proveďte tento příklad:</span><span class="sxs-lookup"><span data-stu-id="19727-180">For example, if you want tooredefine hello notification view for a specific category, you can follow this example:</span></span>

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

<span data-ttu-id="19727-181">Tento jednoduchý příklad kategorie Předpokládejme, že máte soubor s názvem `MyNotificationView.xib` ve vaší sady hlavní aplikace.</span><span class="sxs-lookup"><span data-stu-id="19727-181">This simple example of category assume that you have a file named `MyNotificationView.xib` in your main application bundle.</span></span> <span data-ttu-id="19727-182">Pokud metoda hello není možné toofind odpovídající `.xib`, hello oznámení se nezobrazí a Engagement na výstup zprávu, v konzole hello.</span><span class="sxs-lookup"><span data-stu-id="19727-182">If hello method is not able toofind a corresponding `.xib`, hello notification will not be displayed and Engagement will output a message in hello console.</span></span>

<span data-ttu-id="19727-183">soubor nib Hello poskytuje by měly dodržovat hello následující pravidla:</span><span class="sxs-lookup"><span data-stu-id="19727-183">hello provided nib file should respect hello following rules:</span></span>

* <span data-ttu-id="19727-184">Měl by obsahovat pouze jedno zobrazení.</span><span class="sxs-lookup"><span data-stu-id="19727-184">It should only contain one view.</span></span>
* <span data-ttu-id="19727-185">Dílčích zobrazení by měla být ve stejné typy jako hello těch, které jsou uvnitř hello poskytuje nib soubor s názvem hello`AENotificationView.xib`</span><span class="sxs-lookup"><span data-stu-id="19727-185">Subviews should be of hello same types as hello ones inside hello provided nib file named `AENotificationView.xib`</span></span>
* <span data-ttu-id="19727-186">Dílčích zobrazení musí mít stejné značky jako hello těch, které jsou uvnitř hello poskytuje nib soubor s názvem hello`AENotificationView.xib`</span><span class="sxs-lookup"><span data-stu-id="19727-186">Subviews should have hello same tags as hello ones inside hello provided nib file named `AENotificationView.xib`</span></span>

> [!TIP]
> <span data-ttu-id="19727-187">Jednoduše zkopírujete soubor nib hello zadaný s názvem `AENotificationView.xib`a mohli začít pracovat z ní.</span><span class="sxs-lookup"><span data-stu-id="19727-187">Just copy hello provided nib file, named `AENotificationView.xib`, and start working from there.</span></span> <span data-ttu-id="19727-188">Ale buďte opatrní, hello zobrazení uvnitř tento soubor nib je přidruženou třídu toohello `AENotificationView`.</span><span class="sxs-lookup"><span data-stu-id="19727-188">But be careful, hello view inside this nib file is associated toohello class `AENotificationView`.</span></span> <span data-ttu-id="19727-189">Tato třída předefinovat hello metoda `layoutSubViews` toomove a změňte velikost jeho dílčích zobrazení podle toocontext.</span><span class="sxs-lookup"><span data-stu-id="19727-189">This class redefined hello method `layoutSubViews` toomove and resize its subviews according toocontext.</span></span> <span data-ttu-id="19727-190">Může být vhodné tooreplace její `UIView` nebo vlastní zobrazení třída.</span><span class="sxs-lookup"><span data-stu-id="19727-190">You may want tooreplace it with an `UIView` or you custom view class.</span></span>
>
>

<span data-ttu-id="19727-191">Pokud potřebujete podrobnější přizpůsobení oznámení (Pokud chcete například tooload zobrazení přímo z kódu hello), se doporučuje tootake, podívejte se na hello zadaný zdroj kódu a třída dokumentaci `Protocol ReferencesDefaultNotifier` a `AENotifier`.</span><span class="sxs-lookup"><span data-stu-id="19727-191">If you need deeper customization of your notifications(if you want for instance tooload your view directly from hello code), it is recommended tootake a look at hello provided source code and class documentation of `Protocol ReferencesDefaultNotifier` and `AENotifier`.</span></span>

<span data-ttu-id="19727-192">Všimněte si, že můžete použít hello téhož oznamovatele pro více kategorií.</span><span class="sxs-lookup"><span data-stu-id="19727-192">Note that you can use hello same notifier for multiple categories.</span></span>

<span data-ttu-id="19727-193">Můžete také Předefinovaná hello výchozí oznamovatel takto:</span><span class="sxs-lookup"><span data-stu-id="19727-193">You can also redefined hello default notifier like this:</span></span>

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:kAEReachDefaultCategory];

##### <a name="notification-handling"></a><span data-ttu-id="19727-194">Zpracování oznámení</span><span class="sxs-lookup"><span data-stu-id="19727-194">Notification handling</span></span>
<span data-ttu-id="19727-195">Při použití hello výchozí kategorie, se nazývají některé metody životní cyklus na hello `AEReachContent` objektu tooreport statistiky a aktualizace hello kampaň stavu:</span><span class="sxs-lookup"><span data-stu-id="19727-195">When using hello default category, some life cycle methods are called on hello `AEReachContent` object tooreport statistics and update hello campaign state:</span></span>

* <span data-ttu-id="19727-196">Když se zobrazí hello oznámení v aplikaci, hello `displayNotification` metoda je volána (který sestavy statistik) podle `AEReachModule` Pokud `handleNotification:` vrátí `YES`.</span><span class="sxs-lookup"><span data-stu-id="19727-196">When hello notification is displayed in application, hello `displayNotification` method is called (which reports statistics) by `AEReachModule` if `handleNotification:` returns `YES`.</span></span>
* <span data-ttu-id="19727-197">Pokud se zavře hello oznámení, hello `exitNotification` metoda je volána, statistiky se použije v hlášení a další kampaně lze nyní zpracovat.</span><span class="sxs-lookup"><span data-stu-id="19727-197">If hello notification is dismissed, hello `exitNotification` method is called, statistic is reported and next campaigns can now be processed.</span></span>
* <span data-ttu-id="19727-198">Po kliknutí na oznámení hello `actionNotification` je volána, statistiky se použije v hlášení a hello přidružené akce se provádí.</span><span class="sxs-lookup"><span data-stu-id="19727-198">If hello notification is clicked, `actionNotification` is called, statistic is reported and hello associated action is performed.</span></span>

<span data-ttu-id="19727-199">Pokud vaši implementaci `AENotifier` přeskočení hello výchozí chování, máte tyto metody životního cyklu pro toocall samotnými.</span><span class="sxs-lookup"><span data-stu-id="19727-199">If your implementation of `AENotifier` bypasses hello default behavior, you have toocall these life cycle methods by yourself.</span></span> <span data-ttu-id="19727-200">Hello následující příklady ilustrují některých případech, kde přeskočí hello výchozí chování:</span><span class="sxs-lookup"><span data-stu-id="19727-200">hello following examples illustrate some cases where hello default behavior is bypassed:</span></span>

* <span data-ttu-id="19727-201">Nerozšíříte `AEDefaultNotifier`, například implementována kategorie zpracování od začátku.</span><span class="sxs-lookup"><span data-stu-id="19727-201">You don't extend `AEDefaultNotifier`, e.g. you implemented category handling from scratch.</span></span>
* <span data-ttu-id="19727-202">Můžete overrode `prepareNotificationView:forContent:`, že toomap alespoň `onNotificationActioned` nebo `onNotificationExited` tooone U.I ovládacích prvků.</span><span class="sxs-lookup"><span data-stu-id="19727-202">You overrode `prepareNotificationView:forContent:`, be sure toomap at least `onNotificationActioned` or `onNotificationExited` tooone of your U.I controls.</span></span>

> [!WARNING]
> <span data-ttu-id="19727-203">Pokud `handleNotification:` vyvolá výjimku, hello obsahu se odstraní a `drop` je volána, to je uvedený v statistiky a další kampaně lze nyní zpracovat.</span><span class="sxs-lookup"><span data-stu-id="19727-203">If `handleNotification:` throws an exception, hello content is deleted and `drop` is called, this is reported in statistics and next campaigns can now be processed.</span></span>
>
>

#### <a name="include-notification-as-part-of-an-existing-view"></a><span data-ttu-id="19727-204">Zahrnout oznámení jako součást stávající zobrazení</span><span class="sxs-lookup"><span data-stu-id="19727-204">Include notification as part of an existing view</span></span>
<span data-ttu-id="19727-205">Překryvy se výborně hodí pro rychlé integrace, ale může být někdy není vhodné nebo může mít nežádoucí vedlejší účinky.</span><span class="sxs-lookup"><span data-stu-id="19727-205">Overlays are great for a fast integration but can be sometimes not convenient, or can have unwanted side effects.</span></span>

<span data-ttu-id="19727-206">Pokud nejste s hello překrytí systému v některé z vaší zobrazení, můžete jej přizpůsobit pro tato zobrazení.</span><span class="sxs-lookup"><span data-stu-id="19727-206">If you're not satisfied with hello overlay system in some of your views, you can customize it for these views.</span></span>

<span data-ttu-id="19727-207">Můžete rozhodnout, tooinclude naše rozložení oznámení ve vaší stávající zobrazení.</span><span class="sxs-lookup"><span data-stu-id="19727-207">You can decide tooinclude our notification layout in your existing views.</span></span> <span data-ttu-id="19727-208">toodo, se tedy dvě styly implementace:</span><span class="sxs-lookup"><span data-stu-id="19727-208">toodo so, there is two implementation styles:</span></span>

1. <span data-ttu-id="19727-209">Přidání zobrazení hello oznámení pomocí rozhraní tvůrce</span><span class="sxs-lookup"><span data-stu-id="19727-209">Add hello notification view using interface builder</span></span>

   * <span data-ttu-id="19727-210">Otevřete *rozhraní tvůrce*</span><span class="sxs-lookup"><span data-stu-id="19727-210">Open *Interface Builder*</span></span>
   * <span data-ttu-id="19727-211">Umístěte 320 x 60 (nebo 768 x 60, pokud jste na zařízení iPad) `UIView` místo tooappear oznámení hello</span><span class="sxs-lookup"><span data-stu-id="19727-211">Place a 320x60 (or 768x60 if you are on iPad) `UIView` where you want hello notification tooappear</span></span>
   * <span data-ttu-id="19727-212">Nastavte hodnotu hello značky pro toto zobrazení příliš: **36822491**</span><span class="sxs-lookup"><span data-stu-id="19727-212">Set hello Tag value for this view too: **36822491**</span></span>
2. <span data-ttu-id="19727-213">Přidání zobrazení oznámení hello prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="19727-213">Add hello notification view programmatically.</span></span> <span data-ttu-id="19727-214">Stačí přidáte hello následující kód, pokud byl inicializován zobrazení:</span><span class="sxs-lookup"><span data-stu-id="19727-214">Just add hello following code when your view has been initialized:</span></span>

       UIView* notificationView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 60)]; //Replace x and y coordinate values tooyour needs.
       notificationView.tag = NOTIFICATION_AREA_VIEW_TAG;
       [self.view addSubview:notificationView];

<span data-ttu-id="19727-215">`NOTIFICATION_AREA_VIEW_TAG`Makro lze nalézt v `AEDefaultNotifier.h`.</span><span class="sxs-lookup"><span data-stu-id="19727-215">`NOTIFICATION_AREA_VIEW_TAG` macro can be found in `AEDefaultNotifier.h`.</span></span>

> [!NOTE]
> <span data-ttu-id="19727-216">Hello výchozí oznamovatel automaticky rozpozná tohoto oznámení rozložení hello je zahrnuta v tomto zobrazení a nebude pro něj přidat překrytí.</span><span class="sxs-lookup"><span data-stu-id="19727-216">hello default notifier automatically detects that hello notification layout is included in this view and will not add an overlay for it.</span></span>
>
>

### <a name="announcements-and-polls"></a><span data-ttu-id="19727-217">Oznámení a hlasování</span><span class="sxs-lookup"><span data-stu-id="19727-217">Announcements and polls</span></span>
#### <a name="layouts"></a><span data-ttu-id="19727-218">Rozložení</span><span class="sxs-lookup"><span data-stu-id="19727-218">Layouts</span></span>
<span data-ttu-id="19727-219">Můžete upravit soubory hello `AEDefaultAnnouncementView.xib` a `AEDefaultPollView.xib` tak dlouho, dokud ponechat hodnoty značky hello a typy dílčích hello existující zobrazení.</span><span class="sxs-lookup"><span data-stu-id="19727-219">You can modify hello files `AEDefaultAnnouncementView.xib` and `AEDefaultPollView.xib` as long as you keep hello tag values and types of hello existing subviews.</span></span>

#### <a name="categories"></a><span data-ttu-id="19727-220">Kategorie</span><span class="sxs-lookup"><span data-stu-id="19727-220">Categories</span></span>
##### <a name="alternate-layouts"></a><span data-ttu-id="19727-221">Alternativní rozložení</span><span class="sxs-lookup"><span data-stu-id="19727-221">Alternate layouts</span></span>
<span data-ttu-id="19727-222">Jako oznámení může být hello kampaň kategorie používané toohave alternativní rozložení pro oznámení a hlasování.</span><span class="sxs-lookup"><span data-stu-id="19727-222">Like notifications, hello campaign's category can be used toohave alternate layouts for your announcements and polls.</span></span>

<span data-ttu-id="19727-223">toocreate kategorii pro oznámení, musíte rozšířit **AEAnnouncementViewController** a zaregistrovat ji, jakmile byla inicializována modul reach hello:</span><span class="sxs-lookup"><span data-stu-id="19727-223">toocreate a category for an announcement, you must extend **AEAnnouncementViewController** and register it once hello reach module has been initialized:</span></span>

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:@"my_category"];

> [!NOTE]
> <span data-ttu-id="19727-224">Pokaždé, když se uživatel kliknutím na oznámení pro oznámení s hello kategorie "Moje\_kategorie", řadiči registrované zobrazení (v takovém případě `MyCustomAnnouncementViewController`) bude inicializována pomocí volání metody hello `initWithAnnouncement:` a bude hello zobrazení Přidání toohello aktuální okno aplikace.</span><span class="sxs-lookup"><span data-stu-id="19727-224">Each time a user will click on a notification for an announcement with hello category "my\_category", your registered view controller (in that case `MyCustomAnnouncementViewController`) will be initialized by calling hello method `initWithAnnouncement:` and hello view will be added toohello current application window.</span></span>
>
>

<span data-ttu-id="19727-225">V implementaci hello `AEAnnouncementViewController` třída bude mít vlastnost hello tooread `announcement` tooinitialize vaše dílčích zobrazení.</span><span class="sxs-lookup"><span data-stu-id="19727-225">In your implementation of hello `AEAnnouncementViewController` class you will have tooread hello property `announcement` tooinitialize your subviews.</span></span> <span data-ttu-id="19727-226">Vezměte v úvahu hello níže uvedeném příkladu, kde dva popisky jsou inicializována pomocí `title` a `body` vlastnosti hello `AEReachAnnouncement` třídy:</span><span class="sxs-lookup"><span data-stu-id="19727-226">Consider hello example below, where two labels are initialized using `title` and `body` properties of hello `AEReachAnnouncement` class:</span></span>

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

<span data-ttu-id="19727-227">Pokud nechcete, aby tooload zobrazení sami podle, ale chcete jenom tooreuse hello výchozí oznámení zobrazení rozložení, můžete jednoduše provést řadiči vlastní zobrazení rozšiřuje hello Zadaná třída `AEDefaultAnnouncementViewController`.</span><span class="sxs-lookup"><span data-stu-id="19727-227">If you don't want tooload your views by yourself but you just want tooreuse hello default announcement view layout, you can simply make your custom view controller extends hello provided class `AEDefaultAnnouncementViewController`.</span></span> <span data-ttu-id="19727-228">V takovém případě duplicitní hello nib soubor `AEDefaultAnnouncementView.xib` a přejmenujte ji, mohou být načteny ve vašem řadiči vlastní zobrazení (pro řadič s názvem `CustomAnnouncementViewController`, by měly volat souboru nib `CustomAnnouncementView.xib`).</span><span class="sxs-lookup"><span data-stu-id="19727-228">In that case, duplicate hello nib file `AEDefaultAnnouncementView.xib` and rename it so it can be loaded by your custom view controller (for a controller named `CustomAnnouncementViewController`, you should call your nib file `CustomAnnouncementView.xib`).</span></span>

<span data-ttu-id="19727-229">kategorie výchozí hello tooreplace oznámení, jednoduše zaregistrovat řadiči vlastní zobrazení pro kategorii hello definované v `kAEReachDefaultCategory`:</span><span class="sxs-lookup"><span data-stu-id="19727-229">tooreplace hello default category of announcements, simply register your custom view controller for hello category defined in `kAEReachDefaultCategory`:</span></span>

    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:kAEReachDefaultCategory];

<span data-ttu-id="19727-230">Hlasování může být vlastní hello stejným způsobem jako:</span><span class="sxs-lookup"><span data-stu-id="19727-230">Polls can be customized hello same way :</span></span>

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerPollController:[MyCustomPollViewController class] forCategory:@"my_category"];

<span data-ttu-id="19727-231">Tento čas, hello poskytuje `MyCustomPollViewController` musíte rozšířit `AEPollViewController`.</span><span class="sxs-lookup"><span data-stu-id="19727-231">This time, hello provided `MyCustomPollViewController` must extend `AEPollViewController`.</span></span> <span data-ttu-id="19727-232">Nebo můžete zvolit tooextend z řadiče výchozí hello: `AEDefaultPollViewController`.</span><span class="sxs-lookup"><span data-stu-id="19727-232">Or you can choose tooextend from hello default controller: `AEDefaultPollViewController`.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="19727-233">Nezapomeňte toocall buď `action` (`submitAnswers:` pro vlastní dotazování řadiče zobrazení) nebo `exit` metoda před řadiče zobrazení hello se zavře.</span><span class="sxs-lookup"><span data-stu-id="19727-233">Don't forget toocall either `action` (`submitAnswers:` for custom poll view controllers) or `exit` method before hello view controller is dismissed.</span></span> <span data-ttu-id="19727-234">Jinak statistiky nebude odeslána (tj. žádné analýzy kampaň hello) a další je nejdůležitější další kampaně nebudete upozorněni, až po restartování procesu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="19727-234">Otherwise, statistics won't be sent (i.e. no analytics on hello campaign) and more importantly next campaigns will not be notified until hello application process is restarted.</span></span>
>
>

##### <a name="implementation-example"></a><span data-ttu-id="19727-235">Příklad implementace</span><span class="sxs-lookup"><span data-stu-id="19727-235">Implementation example</span></span>
<span data-ttu-id="19727-236">V této implementaci hello vlastní oznámení zobrazení je načten z externí xib souboru.</span><span class="sxs-lookup"><span data-stu-id="19727-236">In this implementation hello custom announcement view is loaded from an external xib file.</span></span>

<span data-ttu-id="19727-237">Jako přizpůsobení pokročilé oznámení se doporučuje toolook v hello zdrojový kód standardní implementace hello.</span><span class="sxs-lookup"><span data-stu-id="19727-237">Like for advanced notification customization, it is recommended toolook at hello source code of hello standard implementation.</span></span>

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
