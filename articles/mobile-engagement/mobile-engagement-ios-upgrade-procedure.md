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
# <a name="upgrade-procedures"></a>Postupy upgradu
Pokud již mít integrovanou starší verze zapojení do své aplikace, musíte tooconsider hello následující body při upgradu hello SDK.

Pro každou novou verzi hello SDK je třeba nejprve nahradit (odebrat a znovu importujte v xcode) hello EngagementSDK a EngagementReach složky.

## <a name="from-300-too400"></a>Z 3.0.0 too4.0.0
### <a name="xcode-8"></a>XCode 8
XCode 8 je povinný, od verze 4.0.0 hello SDK.

> [!NOTE]
> Pokud ve skutečnosti závisí na XCode 7 pak můžete použít hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh). Je známého problému na modul reach hello tato předchozí verze při spuštění v zařízení s iOS 10: systémová oznámení nejsou reagovali. toofix to budete mít tooimplement hello zastaralá rozhraní API `application:didReceiveRemoteNotification:` ve vaší aplikaci delegovat následujícím způsobem:
> 
> 

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [!IMPORTANT]
> **Toto řešení nedoporučujeme** jako toto chování můžete změnit v jakéhokoli upgradu verze iOS nadcházející (i dílčí), protože tato rozhraní API pro iOS je zastaralý. TooXCode 8 musí přepnout co nejdříve.
> 
> 

### <a name="usernotifications-framework"></a>UserNotifications framework
Je třeba tooadd hello `UserNotifications` framework ve vašem fáze buildu.

v prohlížeči projektu hello otevřete podokno váš projekt a vyberte správné cílové hello. Potom otevřete hello **"Fáze sestavení"** kartě a v hello **"Odkaz binárních souborů a knihoven"** nabídce Přidat framework `UserNotifications.framework` -sadu hello odkaz jako`Optional`

### <a name="application-push-capability"></a>Funkce nabízené aplikace
XCode 8 může resetovat vaše aplikace push schopnosti, Překontrolujte ji prosím v hello `capability` kartě vybraný cílový.

### <a name="add-hello-new-ios-10-notification-registration-code"></a>Přidejte hello nové iOS 10 oznámení registrace kód
Hello starší kód fragment kódu tooregister hello aplikace toonotifications stále funguje, ale používá zastaralá rozhraní API při spuštění v iOS 10.

Import hello `User Notification` framework:

        #import <UserNotifications/UserNotifications.h> 

V delegáta aplikace `application:didFinishLaunchingWithOptions` nahradit metoda:

    if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
        [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
        [application registerForRemoteNotifications];
    }
    else {

        [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
    }

podle:

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

### <a name="resolve-unusernotificationcenter-delegate-conflicts"></a>Řešení konfliktů UNUserNotificationCenter delegáta

*Pokud se implementuje ani aplikace, nebo jeden z vašich knihovnách třetích stran `UNUserNotificationCenterDelegate` pak můžete tuto část přeskočit.*

A `UNUserNotificationCenter` delegát používá hello SDK toomonitor hello životní cyklus Engagement oznámení na zařízení se systémem iOS, 10 nebo vyšší. Hello SDK má svou vlastní implementace hello `UNUserNotificationCenterDelegate` protokolu, ale může být jen jedna `UNUserNotificationCenter` delegovat na aplikaci. Přidat další delegáta toohello `UNUserNotificationCenter` budou v konfliktu s hello Engagement jeden objekt. Pokud hello SDK zjistí delegáta nebo jakékoli jiné třetí strany, pak jej nebude používat vlastní toogive implementaci vám tooresolve prvního hello je v konfliktu. Budete mít tooadd hello Engagement logiku tooyour vlastní delegáta v pořadí tooresolve hello konflikty.

Existují dva způsoby tooachieve to.

Návrh 1, jednoduše tak, že předávání delegát volání toohello SDK:

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

Návrh 2, která dědí z hello nebo `AEUserNotificationHandler` – třída

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
> Můžete určit, zda oznámení pochází z Engagement nebo není předáním jeho `userInfo` slovník toohello agenta `isEngagementPushPayload:` třídy metoda.

Ujistěte se, že hello `UNUserNotificationCenter` objektu delegáta nastavena delegáta tooyour v rámci buď hello `application:willFinishLaunchingWithOptions:` nebo hello `application:didFinishLaunchingWithOptions:` metoda delegáta aplikace.
Například pokud jste implementovali hello výše návrh 1:

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        // Any other code
  
        [UNUserNotificationCenter currentNotificationCenter].delegate = self;
        return YES;
      }

## <a name="from-200-too300"></a>Z 2.0.0 too3.0.0
Podpora pro iOS vyřadit 4.X. Od této verze hello nasazení cílové aplikace musí být minimálně iOS 6.

Pokud používáte Reach ve vaší aplikaci, musíte přidat `remote-notification` toohello hodnotu `UIBackgroundModes` pole v souboru Info.plist v pořadí tooreceive vzdáleného oznámení.

Hello metoda `application:didReceiveRemoteNotification:` musí toobe nahrazuje `application:didReceiveRemoteNotification:fetchCompletionHandler:` v delegáta aplikace.

"AEPushDelegate.h" je zastaralá rozhraní a je nutné tooremove všechny odkazy. To zahrnuje odstranění `[[EngagementAgent shared] setPushDelegate:self]` a hello delegovat metody z delegáta aplikace:

    -(void)willRetrieveLaunchMessage;
    -(void)didFailToRetrieveLaunchMessage;
    -(void)didReceiveLaunchMessage:(AEPushMessage*)launchMessage;

## <a name="from-1160-too200"></a>Z 1.16.0 too2.0.0
Hello následující text popisuje, jak toomigrate integraci sady SDK z hello Capptain služby nabízených Capptain SAS do aplikace používá technologii Azure Mobile Engagement.
Pokud provádíte migraci ze starší verze, nejprve najdete hello Capptain webu toomigrate too1.16 potom použít hello následující postup.

> [!IMPORTANT]
> Capptain a Mobile Engagement nejsou hello stejné služby a postup hello níže uvedené jenom označuje, jak toomigrate hello klientskou aplikaci. Migrace hello SDK v aplikaci hello není migrace dat ze sady Mobile Engagement pro toohello hello Capptain servery
> 
> 

### <a name="agent"></a>Agent
Hello metoda `registerApp:` nahradila nová metoda hello `init:`. Delegáta aplikace musí být příslušným způsobem aktualizuje a použít připojovací řetězec:

            - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
            {
              [...]
              [EngagementAgent init:@"YOUR_CONNECTION_STRING"];
              [...]
            }

Sledování SmartAd byl odebrán z SDK právě máte tooremove všechny instance `AETrackModule` – třída

### <a name="class-name-changes"></a>Změny názvu – třída
Jako součást hello rebranding existuje několik názvů třídy nebo souborů, které je třeba změnit toobe.

Všechny třídy předponu "CP" přejmenování s předponou "AE".

Příklad:

* `CPModule.h`Přejmenování příliš`AEModule.h`.

Všechny třídy předponu "Capptain" přejmenování s předponou "Engagement".

Příklady:

* Hello třída `CapptainAgent` příliš přejmenován`EngagementAgent`.
* Hello třída `CapptainTableViewController` příliš přejmenován`EngagementTableViewController`.
* Hello třída `CapptainUtils` příliš přejmenován`EngagementUtils`.
* Hello třída `CapptainViewController` příliš přejmenován`EngagementViewController`.

