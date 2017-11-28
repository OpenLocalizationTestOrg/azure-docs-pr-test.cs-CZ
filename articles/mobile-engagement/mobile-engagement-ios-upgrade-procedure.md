---
title: Azure Mobile Engagement iOS SDK postup upgradu | Microsoft Docs
description: "Nejnovější aktualizace a postupy pro iOS SDK pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 72a9e493-3f14-4e52-b6e2-0490fd04b184
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 12/13/2016
ms.author: piyushjo
ms.openlocfilehash: 37c7f133d079186f828d58cabce0d2a259efd085
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="upgrade-procedures"></a><span data-ttu-id="78bc4-103">Postupy upgradu</span><span class="sxs-lookup"><span data-stu-id="78bc4-103">Upgrade procedures</span></span>
<span data-ttu-id="78bc4-104">Pokud již jste spojili starší verze zapojení do své aplikace, je nutné zvážit následující body při upgradu sady SDK.</span><span class="sxs-lookup"><span data-stu-id="78bc4-104">If you already have integrated an older version of Engagement into your application, you have to consider the following points when upgrading the SDK.</span></span>

<span data-ttu-id="78bc4-105">Pro každou novou verzi sady SDK je třeba nejprve nahradit (odebrat a znovu importujte v xcode) EngagementSDK a EngagementReach složky.</span><span class="sxs-lookup"><span data-stu-id="78bc4-105">For each new version of the SDK you must first replace (remove and re-import in xcode) the EngagementSDK and EngagementReach folders.</span></span>

## <a name="from-300-to-400"></a><span data-ttu-id="78bc4-106">Z 3.0.0 k 4.0.0</span><span class="sxs-lookup"><span data-stu-id="78bc4-106">From 3.0.0 to 4.0.0</span></span>
### <a name="xcode-8"></a><span data-ttu-id="78bc4-107">XCode 8</span><span class="sxs-lookup"><span data-stu-id="78bc4-107">XCode 8</span></span>
<span data-ttu-id="78bc4-108">XCode 8 je povinný, od verze 4.0.0 sady SDK.</span><span class="sxs-lookup"><span data-stu-id="78bc4-108">XCode 8 is mandatory starting from version 4.0.0 of the SDK.</span></span>

> [!NOTE]
> <span data-ttu-id="78bc4-109">Pokud jste ve skutečnosti závisí na XCode 7 pak můžete použít [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span><span class="sxs-lookup"><span data-stu-id="78bc4-109">If you really depend on XCode 7 then you may use the [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span></span> <span data-ttu-id="78bc4-110">Je známého problému na modul reach této předchozí verze při spuštění v zařízení s iOS 10: systémová oznámení nejsou reagovali.</span><span class="sxs-lookup"><span data-stu-id="78bc4-110">There is a known bug on the reach module of this previous version while running on iOS 10 devices:  system notifications are not actioned.</span></span> <span data-ttu-id="78bc4-111">Chcete-li tomu máte k implementaci nepoužívané rozhraní API `application:didReceiveRemoteNotification:` ve vaší aplikaci delegovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="78bc4-111">To fix this you will have to implement the deprecated API `application:didReceiveRemoteNotification:` in your app delegate as follows:</span></span>
> 
> 

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [!IMPORTANT]
> <span data-ttu-id="78bc4-112">**Toto řešení nedoporučujeme** jako toto chování můžete změnit v jakéhokoli upgradu verze iOS nadcházející (i dílčí), protože tato rozhraní API pro iOS je zastaralý.</span><span class="sxs-lookup"><span data-stu-id="78bc4-112">**We do not recommend this workaround** as this behavior can change in any upcoming (even minor) iOS version upgrade because this iOS API is deprecated.</span></span> <span data-ttu-id="78bc4-113">Můžete přepnout do XCode 8 co nejdříve.</span><span class="sxs-lookup"><span data-stu-id="78bc4-113">You should switch to XCode 8 as soon as possible.</span></span>
> 
> 

### <a name="usernotifications-framework"></a><span data-ttu-id="78bc4-114">UserNotifications framework</span><span class="sxs-lookup"><span data-stu-id="78bc4-114">UserNotifications framework</span></span>
<span data-ttu-id="78bc4-115">Je nutné přidat `UserNotifications` framework ve vašem fáze buildu.</span><span class="sxs-lookup"><span data-stu-id="78bc4-115">You need to add the `UserNotifications` framework in your Build Phases.</span></span>

<span data-ttu-id="78bc4-116">v prohlížeči projektu otevřete podokno váš projekt a vyberte správný cíl.</span><span class="sxs-lookup"><span data-stu-id="78bc4-116">in the project explorer, open your project pane and select the correct target.</span></span> <span data-ttu-id="78bc4-117">Potom otevřete **"Fáze sestavení"** kartě a v **"Odkaz binárních souborů a knihoven"** nabídce Přidat framework `UserNotifications.framework` -nastavit odkaz jako`Optional`</span><span class="sxs-lookup"><span data-stu-id="78bc4-117">Then, open the **"Build phases"** tab and in the **"Link Binary With Libraries"** menu, add framework `UserNotifications.framework` - set the link as `Optional`</span></span>

### <a name="application-push-capability"></a><span data-ttu-id="78bc4-118">Funkce nabízené aplikace</span><span class="sxs-lookup"><span data-stu-id="78bc4-118">Application push capability</span></span>
<span data-ttu-id="78bc4-119">XCode 8 může resetovat vaše aplikace push schopnosti, Překontrolujte ji prosím `capability` kartě vybraný cílový.</span><span class="sxs-lookup"><span data-stu-id="78bc4-119">XCode 8 may reset your app push capability, please double check it in the `capability` tab of your selected target.</span></span>

### <a name="add-the-new-ios-10-notification-registration-code"></a><span data-ttu-id="78bc4-120">Přidat nový kód registrace iOS 10 oznámení</span><span class="sxs-lookup"><span data-stu-id="78bc4-120">Add the new iOS 10 notification registration code</span></span>
<span data-ttu-id="78bc4-121">Starší fragment kódu pro registraci aplikace pro oznámení stále funguje, ale používá zastaralá rozhraní API při spuštění v iOS 10.</span><span class="sxs-lookup"><span data-stu-id="78bc4-121">The older code snippet to register the app to notifications still works but is using deprecated APIs while running on iOS 10.</span></span>

<span data-ttu-id="78bc4-122">Import `User Notification` framework:</span><span class="sxs-lookup"><span data-stu-id="78bc4-122">Import the `User Notification` framework:</span></span>

        #import <UserNotifications/UserNotifications.h> 

<span data-ttu-id="78bc4-123">V delegáta aplikace `application:didFinishLaunchingWithOptions` nahradit metoda:</span><span class="sxs-lookup"><span data-stu-id="78bc4-123">In your application delegate `application:didFinishLaunchingWithOptions` method replace:</span></span>

    if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
        [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
        [application registerForRemoteNotifications];
    }
    else {

        [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
    }

<span data-ttu-id="78bc4-124">podle:</span><span class="sxs-lookup"><span data-stu-id="78bc4-124">by :</span></span>

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

### <a name="resolve-unusernotificationcenter-delegate-conflicts"></a><span data-ttu-id="78bc4-125">Řešení konfliktů UNUserNotificationCenter delegáta</span><span class="sxs-lookup"><span data-stu-id="78bc4-125">Resolve UNUserNotificationCenter delegate conflicts</span></span>

<span data-ttu-id="78bc4-126">*Pokud se implementuje ani aplikace, nebo jeden z vašich knihovnách třetích stran `UNUserNotificationCenterDelegate` pak můžete tuto část přeskočit.*</span><span class="sxs-lookup"><span data-stu-id="78bc4-126">*If neither your application or one of your third party libraries implements a `UNUserNotificationCenterDelegate` then you can skip this part.*</span></span>

<span data-ttu-id="78bc4-127">A `UNUserNotificationCenter` delegáta se sadou SDK používá k monitorování životní cyklus Engagement oznámení na zařízení se systémem iOS, 10 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="78bc4-127">A `UNUserNotificationCenter` delegate is used by the SDK to monitor the life cycle of Engagement notifications on devices running on iOS 10 or greater.</span></span> <span data-ttu-id="78bc4-128">Sada SDK obsahuje vlastní implementaci `UNUserNotificationCenterDelegate` protokolu, ale může být jen jedna `UNUserNotificationCenter` delegovat na aplikaci.</span><span class="sxs-lookup"><span data-stu-id="78bc4-128">The SDK has its own implementation of the `UNUserNotificationCenterDelegate` protocol but there can be only one `UNUserNotificationCenter` delegate per application.</span></span> <span data-ttu-id="78bc4-129">Všechny ostatní delegáta přidán do `UNUserNotificationCenter` budou v konfliktu s zapojení jeden objekt.</span><span class="sxs-lookup"><span data-stu-id="78bc4-129">Any other delegate added to the `UNUserNotificationCenter` object will conflict with the Engagement one.</span></span> <span data-ttu-id="78bc4-130">Pokud sada SDK zjistí nebo jakékoli jiné třetí strany delegáta jej nebude získáte možnost vyřešte konflikty používat vlastní implementaci.</span><span class="sxs-lookup"><span data-stu-id="78bc4-130">If the SDK detects your or any other third party's delegate then it will not use its own implementation to give you a chance to resolve the conflicts.</span></span> <span data-ttu-id="78bc4-131">Budete muset přidat logiku Engagement vlastní delegáta za účelem vyřešení konfliktů.</span><span class="sxs-lookup"><span data-stu-id="78bc4-131">You will have to add the Engagement logic to your own delegate in order to resolve the conflicts.</span></span>

<span data-ttu-id="78bc4-132">Existují dva způsoby, jak dosáhnout.</span><span class="sxs-lookup"><span data-stu-id="78bc4-132">There are two ways to achieve this.</span></span>

<span data-ttu-id="78bc4-133">Návrh 1, jednoduše tak, že předávání delegát volání sady SDK:</span><span class="sxs-lookup"><span data-stu-id="78bc4-133">Proposal 1, simply by forwarding your delegate calls to the SDK:</span></span>

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

<span data-ttu-id="78bc4-134">Návrh 2, která dědí z nebo `AEUserNotificationHandler` – třída</span><span class="sxs-lookup"><span data-stu-id="78bc4-134">Or proposal 2, by inheriting from the `AEUserNotificationHandler` class</span></span>

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
> <span data-ttu-id="78bc4-135">Můžete určit, zda oznámení pochází z Engagement nebo není předáním jeho `userInfo` slovníku agentovi `isEngagementPushPayload:` třídy metoda.</span><span class="sxs-lookup"><span data-stu-id="78bc4-135">You can determine whether a notification comes from Engagement or not by passing its `userInfo` dictionary to the Agent `isEngagementPushPayload:` class method.</span></span>

<span data-ttu-id="78bc4-136">Ujistěte se, že `UNUserNotificationCenter` objektu delegáta je nastaven na vaše delegáta v rámci buď `application:willFinishLaunchingWithOptions:` nebo `application:didFinishLaunchingWithOptions:` metoda delegáta aplikace.</span><span class="sxs-lookup"><span data-stu-id="78bc4-136">Make sure that the `UNUserNotificationCenter` object's delegate is set to your delegate within either the `application:willFinishLaunchingWithOptions:` or the `application:didFinishLaunchingWithOptions:` method of your application delegate.</span></span>
<span data-ttu-id="78bc4-137">Například pokud jste implementovali výše uvedený návrh 1:</span><span class="sxs-lookup"><span data-stu-id="78bc4-137">For instance, if you implemented the above proposal 1:</span></span>

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        // Any other code
  
        [UNUserNotificationCenter currentNotificationCenter].delegate = self;
        return YES;
      }

## <a name="from-200-to-300"></a><span data-ttu-id="78bc4-138">Z 2.0.0 k 3.0.0</span><span class="sxs-lookup"><span data-stu-id="78bc4-138">From 2.0.0 to 3.0.0</span></span>
<span data-ttu-id="78bc4-139">Podpora pro iOS vyřadit 4.X.</span><span class="sxs-lookup"><span data-stu-id="78bc4-139">Dropped support for iOS 4.X.</span></span> <span data-ttu-id="78bc4-140">Od této verze cíl nasazení vaší aplikace musí být minimálně iOS 6.</span><span class="sxs-lookup"><span data-stu-id="78bc4-140">Starting from this version the deployment target of your application must be at least iOS 6.</span></span>

<span data-ttu-id="78bc4-141">Pokud používáte Reach ve vaší aplikaci, musíte přidat `remote-notification` hodnotu `UIBackgroundModes` pole v souboru Info.plist příjem vzdáleného oznámení vyžaduje, aby.</span><span class="sxs-lookup"><span data-stu-id="78bc4-141">If you are using Reach in your application, you must add `remote-notification` value to the `UIBackgroundModes` array in your Info.plist file in order to receive remote notifications.</span></span>

<span data-ttu-id="78bc4-142">Metoda `application:didReceiveRemoteNotification:` musí být nahrazen `application:didReceiveRemoteNotification:fetchCompletionHandler:` v delegáta aplikace.</span><span class="sxs-lookup"><span data-stu-id="78bc4-142">The method `application:didReceiveRemoteNotification:` needs to be replaced by `application:didReceiveRemoteNotification:fetchCompletionHandler:` in your application delegate.</span></span>

<span data-ttu-id="78bc4-143">"AEPushDelegate.h" je zastaralá rozhraní a je nutné odebrat všechny odkazy.</span><span class="sxs-lookup"><span data-stu-id="78bc4-143">"AEPushDelegate.h" is deprecated interface and you need to remove all references.</span></span> <span data-ttu-id="78bc4-144">To zahrnuje odstranění `[[EngagementAgent shared] setPushDelegate:self]` a metody delegáta z delegáta aplikace:</span><span class="sxs-lookup"><span data-stu-id="78bc4-144">This includes removing `[[EngagementAgent shared] setPushDelegate:self]` and the delegate methods from your application delegate:</span></span>

    -(void)willRetrieveLaunchMessage;
    -(void)didFailToRetrieveLaunchMessage;
    -(void)didReceiveLaunchMessage:(AEPushMessage*)launchMessage;

## <a name="from-1160-to-200"></a><span data-ttu-id="78bc4-145">Z 1.16.0 k 2.0.0</span><span class="sxs-lookup"><span data-stu-id="78bc4-145">From 1.16.0 to 2.0.0</span></span>
<span data-ttu-id="78bc4-146">Následující část popisuje postup migrace integraci sady SDK z Capptain služby, které do aplikace používá technologii Azure Mobile Engagement nabízí Capptain SAS.</span><span class="sxs-lookup"><span data-stu-id="78bc4-146">The following describes how to migrate an SDK integration from the Capptain service offered by Capptain SAS into an app powered by Azure Mobile Engagement.</span></span>
<span data-ttu-id="78bc4-147">Pokud provádíte migraci ze starší verze, najdete na webu společnosti Capptain nejdřív přenést 1.16 pak použije následující postup.</span><span class="sxs-lookup"><span data-stu-id="78bc4-147">If you are migrating from an earlier version, please consult the Capptain web site to migrate to 1.16 first then apply the following procedure.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="78bc4-148">Capptain a Mobile Engagement nejsou stejné služby a postup níže uvedené jenom dozvíte, jak migrovat klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="78bc4-148">Capptain and Mobile Engagement are not the same services and the procedure given below only highlights how to migrate the client app.</span></span> <span data-ttu-id="78bc4-149">Migrace sady SDK v aplikaci není migrovat data ze serverů Capptain na servery Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="78bc4-149">Migrating the SDK in the app will NOT migrate your data from the Capptain servers to the Mobile Engagement servers</span></span>
> 
> 

### <a name="agent"></a><span data-ttu-id="78bc4-150">Agent</span><span class="sxs-lookup"><span data-stu-id="78bc4-150">Agent</span></span>
<span data-ttu-id="78bc4-151">Metoda `registerApp:` nahradila nová metoda `init:`.</span><span class="sxs-lookup"><span data-stu-id="78bc4-151">The method `registerApp:` has been replaced by the new method `init:`.</span></span> <span data-ttu-id="78bc4-152">Delegáta aplikace musí být příslušným způsobem aktualizuje a použít připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="78bc4-152">Your application delegate must be updated accordingly and use connection string:</span></span>

            - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
            {
              [...]
              [EngagementAgent init:@"YOUR_CONNECTION_STRING"];
              [...]
            }

<span data-ttu-id="78bc4-153">Sledování SmartAd byla odebrána ze sady SDK, musíte se odebrat všechny instance `AETrackModule` – třída</span><span class="sxs-lookup"><span data-stu-id="78bc4-153">SmartAd tracking has been removed from SDK you just have to remove all instances of `AETrackModule` class</span></span>

### <a name="class-name-changes"></a><span data-ttu-id="78bc4-154">Změny názvu – třída</span><span class="sxs-lookup"><span data-stu-id="78bc4-154">Class Name Changes</span></span>
<span data-ttu-id="78bc4-155">Jako součást rebranding existuje několik názvů souboru nebo třídy, které se musí změnit.</span><span class="sxs-lookup"><span data-stu-id="78bc4-155">As part of the rebranding, there are couple of class/file names that need to be changed.</span></span>

<span data-ttu-id="78bc4-156">Všechny třídy předponu "CP" přejmenování s předponou "AE".</span><span class="sxs-lookup"><span data-stu-id="78bc4-156">All classes prefixed with "CP" are renamed with "AE" prefix.</span></span>

<span data-ttu-id="78bc4-157">Příklad:</span><span class="sxs-lookup"><span data-stu-id="78bc4-157">Example:</span></span>

* <span data-ttu-id="78bc4-158">`CPModule.h`je přejmenován na `AEModule.h`.</span><span class="sxs-lookup"><span data-stu-id="78bc4-158">`CPModule.h` is renamed to `AEModule.h`.</span></span>

<span data-ttu-id="78bc4-159">Všechny třídy předponu "Capptain" přejmenování s předponou "Engagement".</span><span class="sxs-lookup"><span data-stu-id="78bc4-159">All classes prefixed with "Capptain" are renamed with "Engagement" prefix.</span></span>

<span data-ttu-id="78bc4-160">Příklady:</span><span class="sxs-lookup"><span data-stu-id="78bc4-160">Examples:</span></span>

* <span data-ttu-id="78bc4-161">Třída `CapptainAgent` je přejmenován na `EngagementAgent`.</span><span class="sxs-lookup"><span data-stu-id="78bc4-161">The class `CapptainAgent` is renamed to `EngagementAgent`.</span></span>
* <span data-ttu-id="78bc4-162">Třída `CapptainTableViewController` je přejmenován na `EngagementTableViewController`.</span><span class="sxs-lookup"><span data-stu-id="78bc4-162">The class `CapptainTableViewController` is renamed to `EngagementTableViewController`.</span></span>
* <span data-ttu-id="78bc4-163">Třída `CapptainUtils` je přejmenován na `EngagementUtils`.</span><span class="sxs-lookup"><span data-stu-id="78bc4-163">The class `CapptainUtils` is renamed to `EngagementUtils`.</span></span>
* <span data-ttu-id="78bc4-164">Třída `CapptainViewController` je přejmenován na `EngagementViewController`.</span><span class="sxs-lookup"><span data-stu-id="78bc4-164">The class `CapptainViewController` is renamed to `EngagementViewController`.</span></span>

