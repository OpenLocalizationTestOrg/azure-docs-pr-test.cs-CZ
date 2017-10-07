---
title: "aaaAzure Mobile Engagement iOS SDK přehled | Microsoft Docs"
description: "Nejnovější aktualizace a postupy pro iOS SDK pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 3a03bbd6-bcf8-436c-9775-5a8188629252
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: 38f0da2f84df9c62f8fbca233bfda8b9936fdc0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="ios-sdk-for-azure-mobile-engagement"></a><span data-ttu-id="fd3da-103">iOS SDK pro Azure Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="fd3da-103">iOS SDK for Azure Mobile Engagement</span></span>
<span data-ttu-id="fd3da-104">Začněte zde tooget všechny hello podrobnosti o tom, toointegrate Azure Mobile Engagement v aplikaci pro iOS.</span><span class="sxs-lookup"><span data-stu-id="fd3da-104">Start here tooget all hello details on how toointegrate Azure Mobile Engagement in an iOS App.</span></span> <span data-ttu-id="fd3da-105">Pokud byste chtěli toogive ho zkuste to nejprve zkontrolujte, zda můžete dělat naše [15 minut kurzu](mobile-engagement-ios-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="fd3da-105">If you'd like toogive it a try first, make sure you do our [15 minutes tutorial](mobile-engagement-ios-get-started.md).</span></span>

<span data-ttu-id="fd3da-106">Klikněte na tlačítko toosee hello [SDK obsahu](mobile-engagement-ios-sdk-content.md)</span><span class="sxs-lookup"><span data-stu-id="fd3da-106">Click toosee hello [SDK Content](mobile-engagement-ios-sdk-content.md)</span></span>

## <a name="integration-procedures"></a><span data-ttu-id="fd3da-107">Integrace procedury</span><span class="sxs-lookup"><span data-stu-id="fd3da-107">Integration procedures</span></span>
1. <span data-ttu-id="fd3da-108">Začněte zde: [jak toointegrate Mobile Engagement v aplikaci s iOS](mobile-engagement-ios-integrate-engagement.md)</span><span class="sxs-lookup"><span data-stu-id="fd3da-108">Start here: [How toointegrate Mobile Engagement in your iOS app](mobile-engagement-ios-integrate-engagement.md)</span></span>
2. <span data-ttu-id="fd3da-109">Pro oznámení: [jak toointegrate Reach (oznámení) v aplikaci s iOS](mobile-engagement-ios-integrate-engagement-reach.md)</span><span class="sxs-lookup"><span data-stu-id="fd3da-109">For Notifications: [How toointegrate Reach (Notifications) in your iOS app](mobile-engagement-ios-integrate-engagement-reach.md)</span></span>
3. <span data-ttu-id="fd3da-110">Značka plán implementace: [jak toouse hello advanced Mobile Engagement označování rozhraní API v aplikaci s iOS](mobile-engagement-ios-use-engagement-api.md)</span><span class="sxs-lookup"><span data-stu-id="fd3da-110">Tag plan implementation: [How toouse hello advanced Mobile Engagement tagging API in your iOS app](mobile-engagement-ios-use-engagement-api.md)</span></span>

## <a name="release-notes"></a><span data-ttu-id="fd3da-111">Poznámky k verzi</span><span class="sxs-lookup"><span data-stu-id="fd3da-111">Release notes</span></span>
### <a name="410-07172017"></a><span data-ttu-id="fd3da-112">4.1.0 (07/17/2017)</span><span class="sxs-lookup"><span data-stu-id="fd3da-112">4.1.0 (07/17/2017)</span></span>
* <span data-ttu-id="fd3da-113">Odznaky s pevnou vymazat v pozadí.</span><span class="sxs-lookup"><span data-stu-id="fd3da-113">Fixed badges cleared in background.</span></span>
* <span data-ttu-id="fd3da-114">Opravené upozornění na XCode 9 o volání rozhraní API není z hlavní fronty.</span><span class="sxs-lookup"><span data-stu-id="fd3da-114">Fixed warnings on XCode 9 about APIs not called in main queue.</span></span>
* <span data-ttu-id="fd3da-115">Vyřešený nevracení paměti v hlasování Reach.</span><span class="sxs-lookup"><span data-stu-id="fd3da-115">Fixed a memory leak in Reach polls.</span></span>
* <span data-ttu-id="fd3da-116">Podpora pro iOS vyřadit 6.X.</span><span class="sxs-lookup"><span data-stu-id="fd3da-116">Dropped support for iOS 6.X.</span></span> <span data-ttu-id="fd3da-117">Od této verze hello nasazení cílové aplikace musí být minimálně iOS 7.</span><span class="sxs-lookup"><span data-stu-id="fd3da-117">Starting from this version hello deployment target of your application must be at least iOS 7.</span></span>

<span data-ttu-id="fd3da-118">Starší verze najdete v tématu hello [dokončení poznámky k verzi](mobile-engagement-ios-release-notes.md)</span><span class="sxs-lookup"><span data-stu-id="fd3da-118">For earlier version please see hello [complete release notes](mobile-engagement-ios-release-notes.md)</span></span>

## <a name="upgrade-procedures"></a><span data-ttu-id="fd3da-119">Postupy upgradu</span><span class="sxs-lookup"><span data-stu-id="fd3da-119">Upgrade procedures</span></span>
<span data-ttu-id="fd3da-120">Pokud již mít integrovanou starší verze zapojení do své aplikace, musíte tooconsider hello následující body při upgradu hello SDK.</span><span class="sxs-lookup"><span data-stu-id="fd3da-120">If you already have integrated an older version of Engagement into your application, you have tooconsider hello following points when upgrading hello SDK.</span></span>

<span data-ttu-id="fd3da-121">Můžete mít toofollow několika postupů Pokud provedena několik verzí sady SDK najdete v části hello hello dokončení [postupy upgradu](mobile-engagement-ios-upgrade-procedure.md).</span><span class="sxs-lookup"><span data-stu-id="fd3da-121">You may have toofollow several procedures if you missed several versions of hello SDK see hello complete [Upgrade Procedures](mobile-engagement-ios-upgrade-procedure.md).</span></span>

<span data-ttu-id="fd3da-122">Pro každou novou verzi hello SDK je třeba nejprve nahradit (odebrat a znovu importujte v xcode) hello EngagementSDK a EngagementReach složky.</span><span class="sxs-lookup"><span data-stu-id="fd3da-122">For each new version of hello SDK you must first replace (remove and re-import in xcode) hello EngagementSDK and EngagementReach folders.</span></span>

### <a name="from-300-too400"></a><span data-ttu-id="fd3da-123">Z 3.0.0 too4.0.0</span><span class="sxs-lookup"><span data-stu-id="fd3da-123">From 3.0.0 too4.0.0</span></span>
### <a name="xcode-8"></a><span data-ttu-id="fd3da-124">XCode 8</span><span class="sxs-lookup"><span data-stu-id="fd3da-124">XCode 8</span></span>
<span data-ttu-id="fd3da-125">XCode 8 je povinný, od verze 4.0.0 hello SDK.</span><span class="sxs-lookup"><span data-stu-id="fd3da-125">XCode 8 is mandatory starting from version 4.0.0 of hello SDK.</span></span>

> [!NOTE]
> <span data-ttu-id="fd3da-126">Pokud ve skutečnosti závisí na XCode 7 pak můžete použít hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span><span class="sxs-lookup"><span data-stu-id="fd3da-126">If you really depend on XCode 7 then you may use hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span></span> <span data-ttu-id="fd3da-127">Je známého problému na modul reach hello tato předchozí verze při spuštění v zařízení s iOS 10: systémová oznámení nejsou reagovali.</span><span class="sxs-lookup"><span data-stu-id="fd3da-127">There is a known bug on hello reach module of this previous version while running on iOS 10 devices:  system notifications are not actioned.</span></span> <span data-ttu-id="fd3da-128">toofix to budete mít tooimplement hello zastaralá rozhraní API `application:didReceiveRemoteNotification:` ve vaší aplikaci delegovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fd3da-128">toofix this you will have tooimplement hello deprecated API `application:didReceiveRemoteNotification:` in your app delegate as follows:</span></span>
>
>

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [!IMPORTANT]
> <span data-ttu-id="fd3da-129">**Toto řešení nedoporučujeme** jako toto chování můžete změnit v jakéhokoli upgradu verze iOS nadcházející (i dílčí), protože tato rozhraní API pro iOS je zastaralý.</span><span class="sxs-lookup"><span data-stu-id="fd3da-129">**We do not recommend this workaround** as this behavior can change in any upcoming (even minor) iOS version upgrade because this iOS API is deprecated.</span></span> <span data-ttu-id="fd3da-130">TooXCode 8 musí přepnout co nejdříve.</span><span class="sxs-lookup"><span data-stu-id="fd3da-130">You should switch tooXCode 8 as soon as possible.</span></span>
>
>

#### <a name="usernotifications-framework"></a><span data-ttu-id="fd3da-131">UserNotifications framework</span><span class="sxs-lookup"><span data-stu-id="fd3da-131">UserNotifications framework</span></span>
<span data-ttu-id="fd3da-132">Je třeba tooadd hello `UserNotifications` framework ve vašem fáze buildu.</span><span class="sxs-lookup"><span data-stu-id="fd3da-132">You need tooadd hello `UserNotifications` framework in your Build Phases.</span></span>

<span data-ttu-id="fd3da-133">v prohlížeči projektu hello otevřete podokno váš projekt a vyberte správné cílové hello.</span><span class="sxs-lookup"><span data-stu-id="fd3da-133">in hello project explorer, open your project pane and select hello correct target.</span></span> <span data-ttu-id="fd3da-134">Potom otevřete hello **"Fáze sestavení"** kartě a v hello **"Odkaz binárních souborů a knihoven"** nabídce Přidat framework `UserNotifications.framework` -sadu hello odkaz jako`Optional`</span><span class="sxs-lookup"><span data-stu-id="fd3da-134">Then, open hello **"Build phases"** tab and in hello **"Link Binary With Libraries"** menu, add framework `UserNotifications.framework` - set hello link as `Optional`</span></span>

#### <a name="application-push-capability"></a><span data-ttu-id="fd3da-135">Funkce nabízené aplikace</span><span class="sxs-lookup"><span data-stu-id="fd3da-135">Application push capability</span></span>
<span data-ttu-id="fd3da-136">XCode 8 může resetovat vaše aplikace push schopnosti, Překontrolujte ji prosím v hello `capability` kartě vybraný cílový.</span><span class="sxs-lookup"><span data-stu-id="fd3da-136">XCode 8 may reset your app push capability, please double check it in hello `capability` tab of your selected target.</span></span>

#### <a name="add-hello-new-ios-10-notification-registration-code"></a><span data-ttu-id="fd3da-137">Přidejte hello nové iOS 10 oznámení registrace kód</span><span class="sxs-lookup"><span data-stu-id="fd3da-137">Add hello new iOS 10 notification registration code</span></span>
<span data-ttu-id="fd3da-138">Hello starší kód fragment kódu tooregister hello aplikace toonotifications stále funguje, ale používá zastaralá rozhraní API při spuštění v iOS 10.</span><span class="sxs-lookup"><span data-stu-id="fd3da-138">hello older code snippet tooregister hello app toonotifications still works but is using deprecated APIs while running on iOS 10.</span></span>

<span data-ttu-id="fd3da-139">Import hello `User Notification` framework:</span><span class="sxs-lookup"><span data-stu-id="fd3da-139">Import hello `User Notification` framework:</span></span>

        #import <UserNotifications/UserNotifications.h>

<span data-ttu-id="fd3da-140">V delegáta aplikace `application:didFinishLaunchingWithOptions` nahradit metoda:</span><span class="sxs-lookup"><span data-stu-id="fd3da-140">In your application delegate `application:didFinishLaunchingWithOptions` method replace:</span></span>

        if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
            [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
            [application registerForRemoteNotifications];
        }
        else {

            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

<span data-ttu-id="fd3da-141">podle:</span><span class="sxs-lookup"><span data-stu-id="fd3da-141">by :</span></span>

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

#### <a name="resolve-unusernotificationcenter-delegate-conflicts"></a><span data-ttu-id="fd3da-142">Řešení konfliktů UNUserNotificationCenter delegáta</span><span class="sxs-lookup"><span data-stu-id="fd3da-142">Resolve UNUserNotificationCenter delegate conflicts</span></span>

<span data-ttu-id="fd3da-143">*Pokud se implementuje ani aplikace, nebo jeden z vašich knihovnách třetích stran `UNUserNotificationCenterDelegate` pak můžete tuto část přeskočit.*</span><span class="sxs-lookup"><span data-stu-id="fd3da-143">*If neither your application or one of your third party libraries implements a `UNUserNotificationCenterDelegate` then you can skip this part.*</span></span>

<span data-ttu-id="fd3da-144">A `UNUserNotificationCenter` delegát používá hello SDK toomonitor hello životní cyklus Engagement oznámení na zařízení se systémem iOS, 10 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="fd3da-144">A `UNUserNotificationCenter` delegate is used by hello SDK toomonitor hello life cycle of Engagement notifications on devices running on iOS 10 or greater.</span></span> <span data-ttu-id="fd3da-145">Hello SDK má svou vlastní implementace hello `UNUserNotificationCenterDelegate` protokolu, ale může být jen jedna `UNUserNotificationCenter` delegovat na aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fd3da-145">hello SDK has its own implementation of hello `UNUserNotificationCenterDelegate` protocol but there can be only one `UNUserNotificationCenter` delegate per application.</span></span> <span data-ttu-id="fd3da-146">Přidat další delegáta toohello `UNUserNotificationCenter` budou v konfliktu s hello Engagement jeden objekt.</span><span class="sxs-lookup"><span data-stu-id="fd3da-146">Any other delegate added toohello `UNUserNotificationCenter` object will conflict with hello Engagement one.</span></span> <span data-ttu-id="fd3da-147">Pokud hello SDK zjistí delegáta nebo jakékoli jiné třetí strany, pak jej nebude používat vlastní toogive implementaci vám tooresolve prvního hello je v konfliktu.</span><span class="sxs-lookup"><span data-stu-id="fd3da-147">If hello SDK detects your or any other third party's delegate then it will not use its own implementation toogive you a chance tooresolve hello conflicts.</span></span> <span data-ttu-id="fd3da-148">Budete mít tooadd hello Engagement logiku tooyour vlastní delegáta v pořadí tooresolve hello konflikty.</span><span class="sxs-lookup"><span data-stu-id="fd3da-148">You will have tooadd hello Engagement logic tooyour own delegate in order tooresolve hello conflicts.</span></span>

<span data-ttu-id="fd3da-149">Existují dva způsoby tooachieve to.</span><span class="sxs-lookup"><span data-stu-id="fd3da-149">There are two ways tooachieve this.</span></span>

<span data-ttu-id="fd3da-150">Návrh 1, jednoduše tak, že předávání delegát volání toohello SDK:</span><span class="sxs-lookup"><span data-stu-id="fd3da-150">Proposal 1, simply by forwarding your delegate calls toohello SDK:</span></span>

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

<span data-ttu-id="fd3da-151">Návrh 2, která dědí z hello nebo `AEUserNotificationHandler` – třída</span><span class="sxs-lookup"><span data-stu-id="fd3da-151">Or proposal 2, by inheriting from hello `AEUserNotificationHandler` class</span></span>

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
> <span data-ttu-id="fd3da-152">Můžete určit, zda oznámení pochází z Engagement nebo není předáním jeho `userInfo` slovník toohello agenta `isEngagementPushPayload:` třídy metoda.</span><span class="sxs-lookup"><span data-stu-id="fd3da-152">You can determine whether a notification comes from Engagement or not by passing its `userInfo` dictionary toohello Agent `isEngagementPushPayload:` class method.</span></span>

<span data-ttu-id="fd3da-153">Ujistěte se, že hello `UNUserNotificationCenter` objektu delegáta nastavena delegáta tooyour v rámci buď hello `application:willFinishLaunchingWithOptions:` nebo hello `application:didFinishLaunchingWithOptions:` metoda delegáta aplikace.</span><span class="sxs-lookup"><span data-stu-id="fd3da-153">Make sure that hello `UNUserNotificationCenter` object's delegate is set tooyour delegate within either hello `application:willFinishLaunchingWithOptions:` or hello `application:didFinishLaunchingWithOptions:` method of your application delegate.</span></span>
<span data-ttu-id="fd3da-154">Například pokud jste implementovali hello výše návrh 1:</span><span class="sxs-lookup"><span data-stu-id="fd3da-154">For instance, if you implemented hello above proposal 1:</span></span>

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        // Any other code

        [UNUserNotificationCenter currentNotificationCenter].delegate = self;
        return YES;
      }
