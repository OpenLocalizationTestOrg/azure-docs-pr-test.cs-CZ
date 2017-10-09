---
title: aaaAzure Mobile Engagement iOS SDK postup upgradu | Microsoft Docs
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
ms.openlocfilehash: 5a81bcaaec72aec665b3334e6400d520454d56a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-procedures"></a><span data-ttu-id="c52f0-103">Postupy upgradu</span><span class="sxs-lookup"><span data-stu-id="c52f0-103">Upgrade procedures</span></span>
<span data-ttu-id="c52f0-104">Pokud již mít integrovanou starší verze zapojení do své aplikace, musíte tooconsider hello následující body při upgradu hello SDK.</span><span class="sxs-lookup"><span data-stu-id="c52f0-104">If you already have integrated an older version of Engagement into your application, you have tooconsider hello following points when upgrading hello SDK.</span></span>

<span data-ttu-id="c52f0-105">Pro každou novou verzi hello SDK je třeba nejprve nahradit (odebrat a znovu importujte v xcode) hello EngagementSDK a EngagementReach složky.</span><span class="sxs-lookup"><span data-stu-id="c52f0-105">For each new version of hello SDK you must first replace (remove and re-import in xcode) hello EngagementSDK and EngagementReach folders.</span></span>

## <a name="from-300-too400"></a><span data-ttu-id="c52f0-106">Z 3.0.0 too4.0.0</span><span class="sxs-lookup"><span data-stu-id="c52f0-106">From 3.0.0 too4.0.0</span></span>
### <a name="xcode-8"></a><span data-ttu-id="c52f0-107">XCode 8</span><span class="sxs-lookup"><span data-stu-id="c52f0-107">XCode 8</span></span>
<span data-ttu-id="c52f0-108">XCode 8 je povinný, od verze 4.0.0 hello SDK.</span><span class="sxs-lookup"><span data-stu-id="c52f0-108">XCode 8 is mandatory starting from version 4.0.0 of hello SDK.</span></span>

> [!NOTE]
> <span data-ttu-id="c52f0-109">Pokud ve skutečnosti závisí na XCode 7 pak můžete použít hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span><span class="sxs-lookup"><span data-stu-id="c52f0-109">If you really depend on XCode 7 then you may use hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span></span> <span data-ttu-id="c52f0-110">Je známého problému na modul reach hello tato předchozí verze při spuštění v zařízení s iOS 10: systémová oznámení nejsou reagovali.</span><span class="sxs-lookup"><span data-stu-id="c52f0-110">There is a known bug on hello reach module of this previous version while running on iOS 10 devices:  system notifications are not actioned.</span></span> <span data-ttu-id="c52f0-111">toofix to budete mít tooimplement hello zastaralá rozhraní API `application:didReceiveRemoteNotification:` ve vaší aplikaci delegovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c52f0-111">toofix this you will have tooimplement hello deprecated API `application:didReceiveRemoteNotification:` in your app delegate as follows:</span></span>
> 
> 

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [!IMPORTANT]
> <span data-ttu-id="c52f0-112">**Toto řešení nedoporučujeme** jako toto chování můžete změnit v jakéhokoli upgradu verze iOS nadcházející (i dílčí), protože tato rozhraní API pro iOS je zastaralý.</span><span class="sxs-lookup"><span data-stu-id="c52f0-112">**We do not recommend this workaround** as this behavior can change in any upcoming (even minor) iOS version upgrade because this iOS API is deprecated.</span></span> <span data-ttu-id="c52f0-113">TooXCode 8 musí přepnout co nejdříve.</span><span class="sxs-lookup"><span data-stu-id="c52f0-113">You should switch tooXCode 8 as soon as possible.</span></span>
> 
> 

### <a name="usernotifications-framework"></a><span data-ttu-id="c52f0-114">UserNotifications framework</span><span class="sxs-lookup"><span data-stu-id="c52f0-114">UserNotifications framework</span></span>
<span data-ttu-id="c52f0-115">Je třeba tooadd hello `UserNotifications` framework ve vašem fáze buildu.</span><span class="sxs-lookup"><span data-stu-id="c52f0-115">You need tooadd hello `UserNotifications` framework in your Build Phases.</span></span>

<span data-ttu-id="c52f0-116">v prohlížeči projektu hello otevřete podokno váš projekt a vyberte správné cílové hello.</span><span class="sxs-lookup"><span data-stu-id="c52f0-116">in hello project explorer, open your project pane and select hello correct target.</span></span> <span data-ttu-id="c52f0-117">Potom otevřete hello **"Fáze sestavení"** kartě a v hello **"Odkaz binárních souborů a knihoven"** nabídce Přidat framework `UserNotifications.framework` -sadu hello odkaz jako`Optional`</span><span class="sxs-lookup"><span data-stu-id="c52f0-117">Then, open hello **"Build phases"** tab and in hello **"Link Binary With Libraries"** menu, add framework `UserNotifications.framework` - set hello link as `Optional`</span></span>

### <a name="application-push-capability"></a><span data-ttu-id="c52f0-118">Funkce nabízené aplikace</span><span class="sxs-lookup"><span data-stu-id="c52f0-118">Application push capability</span></span>
<span data-ttu-id="c52f0-119">XCode 8 může resetovat vaše aplikace push schopnosti, Překontrolujte ji prosím v hello `capability` kartě vybraný cílový.</span><span class="sxs-lookup"><span data-stu-id="c52f0-119">XCode 8 may reset your app push capability, please double check it in hello `capability` tab of your selected target.</span></span>

### <a name="add-hello-new-ios-10-notification-registration-code"></a><span data-ttu-id="c52f0-120">Přidejte hello nové iOS 10 oznámení registrace kód</span><span class="sxs-lookup"><span data-stu-id="c52f0-120">Add hello new iOS 10 notification registration code</span></span>
<span data-ttu-id="c52f0-121">Hello starší kód fragment kódu tooregister hello aplikace toonotifications stále funguje, ale používá zastaralá rozhraní API při spuštění v iOS 10.</span><span class="sxs-lookup"><span data-stu-id="c52f0-121">hello older code snippet tooregister hello app toonotifications still works but is using deprecated APIs while running on iOS 10.</span></span>

<span data-ttu-id="c52f0-122">Import hello `User Notification` framework:</span><span class="sxs-lookup"><span data-stu-id="c52f0-122">Import hello `User Notification` framework:</span></span>

        #import <UserNotifications/UserNotifications.h> 

<span data-ttu-id="c52f0-123">V delegáta aplikace `application:didFinishLaunchingWithOptions` nahradit metoda:</span><span class="sxs-lookup"><span data-stu-id="c52f0-123">In your application delegate `application:didFinishLaunchingWithOptions` method replace:</span></span>

    if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
        [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
        [application registerForRemoteNotifications];
    }
    else {

        [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
    }

<span data-ttu-id="c52f0-124">podle:</span><span class="sxs-lookup"><span data-stu-id="c52f0-124">by :</span></span>

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

### <a name="resolve-unusernotificationcenter-delegate-conflicts"></a><span data-ttu-id="c52f0-125">Řešení konfliktů UNUserNotificationCenter delegáta</span><span class="sxs-lookup"><span data-stu-id="c52f0-125">Resolve UNUserNotificationCenter delegate conflicts</span></span>

<span data-ttu-id="c52f0-126">*Pokud se implementuje ani aplikace, nebo jeden z vašich knihovnách třetích stran `UNUserNotificationCenterDelegate` pak můžete tuto část přeskočit.*</span><span class="sxs-lookup"><span data-stu-id="c52f0-126">*If neither your application or one of your third party libraries implements a `UNUserNotificationCenterDelegate` then you can skip this part.*</span></span>

<span data-ttu-id="c52f0-127">A `UNUserNotificationCenter` delegát používá hello SDK toomonitor hello životní cyklus Engagement oznámení na zařízení se systémem iOS, 10 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="c52f0-127">A `UNUserNotificationCenter` delegate is used by hello SDK toomonitor hello life cycle of Engagement notifications on devices running on iOS 10 or greater.</span></span> <span data-ttu-id="c52f0-128">Hello SDK má svou vlastní implementace hello `UNUserNotificationCenterDelegate` protokolu, ale může být jen jedna `UNUserNotificationCenter` delegovat na aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c52f0-128">hello SDK has its own implementation of hello `UNUserNotificationCenterDelegate` protocol but there can be only one `UNUserNotificationCenter` delegate per application.</span></span> <span data-ttu-id="c52f0-129">Přidat další delegáta toohello `UNUserNotificationCenter` budou v konfliktu s hello Engagement jeden objekt.</span><span class="sxs-lookup"><span data-stu-id="c52f0-129">Any other delegate added toohello `UNUserNotificationCenter` object will conflict with hello Engagement one.</span></span> <span data-ttu-id="c52f0-130">Pokud hello SDK zjistí delegáta nebo jakékoli jiné třetí strany, pak jej nebude používat vlastní toogive implementaci vám tooresolve prvního hello je v konfliktu.</span><span class="sxs-lookup"><span data-stu-id="c52f0-130">If hello SDK detects your or any other third party's delegate then it will not use its own implementation toogive you a chance tooresolve hello conflicts.</span></span> <span data-ttu-id="c52f0-131">Budete mít tooadd hello Engagement logiku tooyour vlastní delegáta v pořadí tooresolve hello konflikty.</span><span class="sxs-lookup"><span data-stu-id="c52f0-131">You will have tooadd hello Engagement logic tooyour own delegate in order tooresolve hello conflicts.</span></span>

<span data-ttu-id="c52f0-132">Existují dva způsoby tooachieve to.</span><span class="sxs-lookup"><span data-stu-id="c52f0-132">There are two ways tooachieve this.</span></span>

<span data-ttu-id="c52f0-133">Návrh 1, jednoduše tak, že předávání delegát volání toohello SDK:</span><span class="sxs-lookup"><span data-stu-id="c52f0-133">Proposal 1, simply by forwarding your delegate calls toohello SDK:</span></span>

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

<span data-ttu-id="c52f0-134">Návrh 2, která dědí z hello nebo `AEUserNotificationHandler` – třída</span><span class="sxs-lookup"><span data-stu-id="c52f0-134">Or proposal 2, by inheriting from hello `AEUserNotificationHandler` class</span></span>

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
> <span data-ttu-id="c52f0-135">Můžete určit, zda oznámení pochází z Engagement nebo není předáním jeho `userInfo` slovník toohello agenta `isEngagementPushPayload:` třídy metoda.</span><span class="sxs-lookup"><span data-stu-id="c52f0-135">You can determine whether a notification comes from Engagement or not by passing its `userInfo` dictionary toohello Agent `isEngagementPushPayload:` class method.</span></span>

<span data-ttu-id="c52f0-136">Ujistěte se, že hello `UNUserNotificationCenter` objektu delegáta nastavena delegáta tooyour v rámci buď hello `application:willFinishLaunchingWithOptions:` nebo hello `application:didFinishLaunchingWithOptions:` metoda delegáta aplikace.</span><span class="sxs-lookup"><span data-stu-id="c52f0-136">Make sure that hello `UNUserNotificationCenter` object's delegate is set tooyour delegate within either hello `application:willFinishLaunchingWithOptions:` or hello `application:didFinishLaunchingWithOptions:` method of your application delegate.</span></span>
<span data-ttu-id="c52f0-137">Například pokud jste implementovali hello výše návrh 1:</span><span class="sxs-lookup"><span data-stu-id="c52f0-137">For instance, if you implemented hello above proposal 1:</span></span>

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        // Any other code
  
        [UNUserNotificationCenter currentNotificationCenter].delegate = self;
        return YES;
      }

## <a name="from-200-too300"></a><span data-ttu-id="c52f0-138">Z 2.0.0 too3.0.0</span><span class="sxs-lookup"><span data-stu-id="c52f0-138">From 2.0.0 too3.0.0</span></span>
<span data-ttu-id="c52f0-139">Podpora pro iOS vyřadit 4.X.</span><span class="sxs-lookup"><span data-stu-id="c52f0-139">Dropped support for iOS 4.X.</span></span> <span data-ttu-id="c52f0-140">Od této verze hello nasazení cílové aplikace musí být minimálně iOS 6.</span><span class="sxs-lookup"><span data-stu-id="c52f0-140">Starting from this version hello deployment target of your application must be at least iOS 6.</span></span>

<span data-ttu-id="c52f0-141">Pokud používáte Reach ve vaší aplikaci, musíte přidat `remote-notification` toohello hodnotu `UIBackgroundModes` pole v souboru Info.plist v pořadí tooreceive vzdáleného oznámení.</span><span class="sxs-lookup"><span data-stu-id="c52f0-141">If you are using Reach in your application, you must add `remote-notification` value toohello `UIBackgroundModes` array in your Info.plist file in order tooreceive remote notifications.</span></span>

<span data-ttu-id="c52f0-142">Hello metoda `application:didReceiveRemoteNotification:` musí toobe nahrazuje `application:didReceiveRemoteNotification:fetchCompletionHandler:` v delegáta aplikace.</span><span class="sxs-lookup"><span data-stu-id="c52f0-142">hello method `application:didReceiveRemoteNotification:` needs toobe replaced by `application:didReceiveRemoteNotification:fetchCompletionHandler:` in your application delegate.</span></span>

<span data-ttu-id="c52f0-143">"AEPushDelegate.h" je zastaralá rozhraní a je nutné tooremove všechny odkazy.</span><span class="sxs-lookup"><span data-stu-id="c52f0-143">"AEPushDelegate.h" is deprecated interface and you need tooremove all references.</span></span> <span data-ttu-id="c52f0-144">To zahrnuje odstranění `[[EngagementAgent shared] setPushDelegate:self]` a hello delegovat metody z delegáta aplikace:</span><span class="sxs-lookup"><span data-stu-id="c52f0-144">This includes removing `[[EngagementAgent shared] setPushDelegate:self]` and hello delegate methods from your application delegate:</span></span>

    -(void)willRetrieveLaunchMessage;
    -(void)didFailToRetrieveLaunchMessage;
    -(void)didReceiveLaunchMessage:(AEPushMessage*)launchMessage;

## <a name="from-1160-too200"></a><span data-ttu-id="c52f0-145">Z 1.16.0 too2.0.0</span><span class="sxs-lookup"><span data-stu-id="c52f0-145">From 1.16.0 too2.0.0</span></span>
<span data-ttu-id="c52f0-146">Hello následující text popisuje, jak toomigrate integraci sady SDK z hello Capptain služby nabízených Capptain SAS do aplikace používá technologii Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="c52f0-146">hello following describes how toomigrate an SDK integration from hello Capptain service offered by Capptain SAS into an app powered by Azure Mobile Engagement.</span></span>
<span data-ttu-id="c52f0-147">Pokud provádíte migraci ze starší verze, nejprve najdete hello Capptain webu toomigrate too1.16 potom použít hello následující postup.</span><span class="sxs-lookup"><span data-stu-id="c52f0-147">If you are migrating from an earlier version, please consult hello Capptain web site toomigrate too1.16 first then apply hello following procedure.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c52f0-148">Capptain a Mobile Engagement nejsou hello stejné služby a postup hello níže uvedené jenom označuje, jak toomigrate hello klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c52f0-148">Capptain and Mobile Engagement are not hello same services and hello procedure given below only highlights how toomigrate hello client app.</span></span> <span data-ttu-id="c52f0-149">Migrace hello SDK v aplikaci hello není migrace dat ze sady Mobile Engagement pro toohello hello Capptain servery</span><span class="sxs-lookup"><span data-stu-id="c52f0-149">Migrating hello SDK in hello app will NOT migrate your data from hello Capptain servers toohello Mobile Engagement servers</span></span>
> 
> 

### <a name="agent"></a><span data-ttu-id="c52f0-150">Agent</span><span class="sxs-lookup"><span data-stu-id="c52f0-150">Agent</span></span>
<span data-ttu-id="c52f0-151">Hello metoda `registerApp:` nahradila nová metoda hello `init:`.</span><span class="sxs-lookup"><span data-stu-id="c52f0-151">hello method `registerApp:` has been replaced by hello new method `init:`.</span></span> <span data-ttu-id="c52f0-152">Delegáta aplikace musí být příslušným způsobem aktualizuje a použít připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="c52f0-152">Your application delegate must be updated accordingly and use connection string:</span></span>

            - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
            {
              [...]
              [EngagementAgent init:@"YOUR_CONNECTION_STRING"];
              [...]
            }

<span data-ttu-id="c52f0-153">Sledování SmartAd byl odebrán z SDK právě máte tooremove všechny instance `AETrackModule` – třída</span><span class="sxs-lookup"><span data-stu-id="c52f0-153">SmartAd tracking has been removed from SDK you just have tooremove all instances of `AETrackModule` class</span></span>

### <a name="class-name-changes"></a><span data-ttu-id="c52f0-154">Změny názvu – třída</span><span class="sxs-lookup"><span data-stu-id="c52f0-154">Class Name Changes</span></span>
<span data-ttu-id="c52f0-155">Jako součást hello rebranding existuje několik názvů třídy nebo souborů, které je třeba změnit toobe.</span><span class="sxs-lookup"><span data-stu-id="c52f0-155">As part of hello rebranding, there are couple of class/file names that need toobe changed.</span></span>

<span data-ttu-id="c52f0-156">Všechny třídy předponu "CP" přejmenování s předponou "AE".</span><span class="sxs-lookup"><span data-stu-id="c52f0-156">All classes prefixed with "CP" are renamed with "AE" prefix.</span></span>

<span data-ttu-id="c52f0-157">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c52f0-157">Example:</span></span>

* <span data-ttu-id="c52f0-158">`CPModule.h`Přejmenování příliš`AEModule.h`.</span><span class="sxs-lookup"><span data-stu-id="c52f0-158">`CPModule.h` is renamed too`AEModule.h`.</span></span>

<span data-ttu-id="c52f0-159">Všechny třídy předponu "Capptain" přejmenování s předponou "Engagement".</span><span class="sxs-lookup"><span data-stu-id="c52f0-159">All classes prefixed with "Capptain" are renamed with "Engagement" prefix.</span></span>

<span data-ttu-id="c52f0-160">Příklady:</span><span class="sxs-lookup"><span data-stu-id="c52f0-160">Examples:</span></span>

* <span data-ttu-id="c52f0-161">Hello třída `CapptainAgent` příliš přejmenován`EngagementAgent`.</span><span class="sxs-lookup"><span data-stu-id="c52f0-161">hello class `CapptainAgent` is renamed too`EngagementAgent`.</span></span>
* <span data-ttu-id="c52f0-162">Hello třída `CapptainTableViewController` příliš přejmenován`EngagementTableViewController`.</span><span class="sxs-lookup"><span data-stu-id="c52f0-162">hello class `CapptainTableViewController` is renamed too`EngagementTableViewController`.</span></span>
* <span data-ttu-id="c52f0-163">Hello třída `CapptainUtils` příliš přejmenován`EngagementUtils`.</span><span class="sxs-lookup"><span data-stu-id="c52f0-163">hello class `CapptainUtils` is renamed too`EngagementUtils`.</span></span>
* <span data-ttu-id="c52f0-164">Hello třída `CapptainViewController` příliš přejmenován`EngagementViewController`.</span><span class="sxs-lookup"><span data-stu-id="c52f0-164">hello class `CapptainViewController` is renamed too`EngagementViewController`.</span></span>

