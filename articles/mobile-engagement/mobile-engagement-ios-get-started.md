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
# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-objective-c"></a>Začínáme s Azure Mobile Engagementem pro aplikace pro iOS v Objective C
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Toto téma ukazuje, jak Azure Mobile Engagement toounderstand toouse využití aplikací a odesílat nabízená oznámení toosegmented uživatelé tooan iOS aplikace.
V tomto kurzu vytvoříte prázdnou aplikaci iOS, která bude shromažďovat základní data a přijímat nabízená oznámení pomocí systému nabízených oznámení Apple (APNS). 

Tento kurz vyžaduje hello následující:

* XCode 8, který si můžete nainstalovat z MAC App Storu
* Hello [Mobile Engagement iOS SDK]

Ve všech dalších kurzech k Mobile Engagement týkajících se aplikací pro iOS se předpokládá dokončení tohoto kurzu.

> [!NOTE]
> toocomplete tento kurz, musíte mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).
>
>

## <a id="setup-azme"></a>Nastavení Mobile Engagementu pro vaši aplikaci pro iOS
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Připojit vaše aplikace toohello Mobile Engagement back-end
Tento kurzu si představíme "základní integraci", což je minimální hello nastavte požadované toocollect dat a odesílání nabízených oznámení. Hello kompletní dokumentaci k integraci najdete v hello [integraci sady Mobile Engagement iOS SDK](mobile-engagement-ios-sdk-overview.md)

Vytvoříme základní aplikaci s integrací hello toodemonstrate XCode.

### <a name="create-a-new-ios-project"></a>Vytvoření nového projektu iOS
[!INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

### <a name="connect-your-app-toohello-mobile-engagement-backend"></a>Připojit vaše aplikace toohello Mobile Engagement back-end
1. Stáhnout hello [Mobile Engagement iOS SDK].
2. Extrahování hello. tar.gz souboru tooa složky v počítači.
3. Klikněte pravým tlačítkem na projekt hello a potom vyberte **přidat soubory do**.

    ![][1]
4. Přejděte toohello složky, které jste extrahovali hello SDK, vyberte hello `EngagementSDK` složku, klikněte na tlačítko **možnosti** hello levém dolním rohu a ujistěte se, toto zaškrtávací políčko hello **kopírovat položky v případě potřeby** a hello zaškrtávací políčko k cílovému jsou zaškrtnutá políčka stiskněte klávesu **OK**.

    ![][2]
5. Otevřete hello **fáze buildu** kartě a v hello **odkaz binárních souborů a knihoven** nabídky, přidejte hello architektury, jak je uvedeno níže:

    ![][3]
6. Přejděte zpět toohello portál Azure ve vaší aplikaci **informace o připojení** stránky a zkopírujte připojovací řetězec hello.

    ![][4]
7. Přidejte následující řádek kódu hello vaše **AppDelegate.m** souboru.

        #import "EngagementAgent.h"
8. Nyní vložte připojovací řetězec hello hello `didFinishLaunchingWithOptions` delegovat.

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
              [...]   
              [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
              [...]
        }
9. `setTestLogEnabled`je volitelný příkaz, který protokolům SDK pro vás tooidentify problémy.

## <a id="monitor"></a>Povolení sledování v reálném čase
V pořadí toostart odesílat data a zajistit, že hello uživatelé jsou aktivní musíte odeslat alespoň jednu obrazovku (aktivitu) toohello Mobile Engagement back-end.

1. Otevřete hello **ViewController.h** souboru a import **EngagementViewController.h**:

    `#import "EngagementViewController.h"`
2. Nyní nahraďte super třídu hello hello **ViewController** rozhraní podle `EngagementViewController`:

    `@interface ViewController : EngagementViewController`

## <a id="monitor"></a>Připojení aplikace se sledováním v reálném čase
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci
Mobile Engagement umožňuje toointeract s uživateli a REACH s nabízená oznámení a zasílání zpráv v kontextu hello kampaní v aplikaci. Tento modul je hello portálu Mobile Engagement nazývá REACH.
Hello následujících sekcích nastavíte tooreceive vaše aplikace je.

### <a name="enable-your-app-tooreceive-silent-push-notifications"></a>Povolit vaší aplikace tooreceive bezobslužných nabízených oznámení
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-hello-reach-library-tooyour-project"></a>Přidání projektu tooyour knihovny Reach hello
1. Klikněte pravým tlačítkem na projekt.
2. Vyberte **Add file to** (Přidat soubor).
3. Přejděte toohello složky, které jste extrahovali hello SDK.
4. Vyberte hello `EngagementReach` složky.
5. Klikněte na tlačítko **Přidat**.

### <a name="modify-your-application-delegate"></a>Úprava delegáta aplikace
1. Zpět v **AppDeletegate.m** souboru, importujte modul Engagement Reach hello.

        #import "AEReachModule.h"
        #import <UserNotifications/UserNotifications.h>
2. Uvnitř hello `application:didFinishLaunchingWithOptions` metoda, vytvořte modul kampaně Reach a předejte jej tooyour existujícího inicializačního řádku Engagement:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

### <a name="enable-your-app-tooreceive-apns-push-notifications"></a>Povolit vaší aplikace tooreceive nabízených oznámení APNS
1. Přidejte následující řádek toohello hello `application:didFinishLaunchingWithOptions` metoda:

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
2. Přidat hello `application:didRegisterForRemoteNotificationsWithDeviceToken` metoda následujícím způsobem:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
             [[EngagementAgent shared] registerDeviceToken:deviceToken];
            NSLog(@"Registered Token: %@", deviceToken);
        }
3. Přidat hello `didFailToRegisterForRemoteNotificationsWithError` metoda následujícím způsobem:

        - (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
        {
           NSLog(@"Failed tooget token, error: %@", error);
        }
4. Přidat hello `didReceiveRemoteNotification:fetchCompletionHandler` metoda následujícím způsobem:

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
