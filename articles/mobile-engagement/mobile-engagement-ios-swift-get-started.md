---
title: "aaaGet Začínáme s Azure Mobile Engagementem pro iOS ve Swiftu | Microsoft Docs"
description: "Zjistěte, jak toouse Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro aplikace pro iOS."
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
ms.openlocfilehash: 9a3841d305745f8b80c6b0c86aabe18e0c7c0e59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-swift"></a>Začínáme s Azure Mobile Engagementem pro aplikace pro iOS ve Swiftu
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Toto téma ukazuje, jak Azure Mobile Engagement toounderstand toouse využití aplikací a odesílat nabízená oznámení toosegmented uživatelé tooan iOS aplikace.
V tomto kurzu vytvoříte prázdnou aplikaci iOS, která bude shromažďovat základní data a přijímat nabízená oznámení pomocí systému nabízených oznámení Apple (APNS). 

Tento kurz vyžaduje hello následující:

* XCode 8, který si můžete nainstalovat z MAC App Storu
* Hello [Mobile Engagement iOS SDK]
* Certifikát nabízených oznámení (.p12), který můžete získat ve vývojářském centru Apple

> [!NOTE]
> V tomto kurzu budeme používat Swift verze 3.0. 
> 
> 

Ve všech dalších kurzech k Mobile Engagement týkajících se aplikací pro iOS se předpokládá dokončení tohoto kurzu.

> [!NOTE]
> toocomplete tento kurz, musíte mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).
> 
> 

## <a id="setup-azme"></a>Nastavení Mobile Engagementu pro vaši aplikaci pro iOS
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Připojit vaše aplikace toohello Mobile Engagement back-end
Tento kurzu si představíme "základní integraci", což je minimální hello nastavte požadované toocollect dat a odesílání nabízených oznámení. Hello kompletní dokumentaci k integraci najdete v hello [integraci sady Mobile Engagement iOS SDK](mobile-engagement-ios-sdk-overview.md)

Pomocí integrace hello toodemonstrate XCode vytvoříme základní aplikaci:

### <a name="create-a-new-ios-project"></a>Vytvoření nového projektu iOS
[!INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

### <a name="connect-your-app-toomobile-engagement-backend"></a>Připojit vaše tooMobile Engagement back-end aplikace
1. Stáhnout hello [Mobile Engagement iOS SDK]
2. Extrahování hello. tar.gz souboru tooa složky v počítači
3. Klikněte pravým tlačítkem na projekt hello a vyberte možnost "Přidat soubory příliš..."
   
    ![][1]
4. Přejděte toohello složky, které jste extrahovali hello SDK a vyberte hello `EngagementSDK` složky poté stiskněte klávesu OK.
   
    ![][2]
5. Otevřete hello `Build Phases` kartě a v hello `Link Binary With Libraries` nabídky přidat hello rozhraní, jak je uvedeno níže:
   
    ![][3]
6. Vytvoření přemostění záhlaví toobe možné toouse hello sady SDK API jazyka Objective C tak, že zvolíte Soubor > Nový > Soubor > iOS > zdroj > hlavičkový soubor.
   
    ![][4]
7. Upravit hello přemostění hlavičky souboru tooexpose Mobile Engagement Objective-C kód tooyour kód se Swift, přidejte následující importy hello:
   
        /* Mobile Engagement Agent */
        #import "AEModule.h"
        #import "AEPushMessage.h"
        #import "AEStorage.h"
        #import "EngagementAgent.h"
        #import "EngagementTableViewController.h"
        #import "EngagementViewController.h"
        #import "AEUserNotificationHandler.h"
        #import "AEIdfaProvider.h"
8. V části nastavení sestavení zkontrolujte, zda hello nastavení v části kompilátoru Swift – generování kódu sestavení hlavičky přemostění jazyka Objective-C má hlavičku toothis cesta. Tady je příklad cesty: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (v závislosti na hello cesta)**
   
   ![][6]
9. Přejděte zpět toohello portál Azure ve vaší aplikaci *informace o připojení* stránky a zkopírujte připojovací řetězec hello
   
   ![][5]
10. Nyní vložte připojovací řetězec hello hello `didFinishLaunchingWithOptions` delegovat
    
        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool
        {
              [...]
                EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}")
              [...]
        }

## <a id="monitor"></a>Povolení sledování v reálném čase
V pořadí toostart odesílat data a zajistit, že hello uživatelé jsou aktivní musíte odeslat alespoň jednu obrazovku (aktivitu) toohello Mobile Engagement back-end.

1. Otevřete hello **ViewController.swift** souboru a nahraďte základní třídu hello **ViewController** toobe **EngagementViewController**:
   
    `class ViewController : EngagementViewController {`

## <a id="monitor"></a>Připojení aplikace se sledováním v reálném čase
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci
Mobile Engagement umožňuje toointeract a REACH s uživateli pomocí nabízených oznámení a zasílání zpráv v aplikaci v kontextu hello kampaní. Tento modul je hello portálu Mobile Engagement nazývá REACH.
Hello následujících sekcích nastavíte tooreceive vaše aplikace je.

### <a name="enable-your-app-tooreceive-silent-push-notifications"></a>Povolit vaší aplikace tooreceive bezobslužných nabízených oznámení
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-hello-reach-library-tooyour-project"></a>Přidání projektu tooyour knihovny Reach hello
1. Klikněte pravým tlačítkem na projekt.
2. Vyberte `Add file too...`
3. Přejděte toohello složky, které jste extrahovali hello SDK
4. Vyberte hello `EngagementReach` složky
5. Klikněte na tlačítko Přidat.
6. Upravit hello přemostění hlavičky Reach jazyka Objective-C Mobile Engagement záhlaví tooexpose souborů a přidejte následující importy hello:
   
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

### <a name="modify-your-application-delegate"></a>Úprava delegáta aplikace
1. Uvnitř hello `didFinishLaunchingWithOptions` – Vytvořte modul kampaně reach a předejte ji tooyour existujícího inicializačního řádku Engagement:
   
        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool 
        {
            let reach = AEReachModule.module(withNotificationIcon: UIImage(named:"icon.png")) as! AEReachModule
            EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}", modulesArray:[reach])
            [...]
            return true
        }

### <a name="enable-your-app-tooreceive-apns-push-notifications"></a>Povolit vaší aplikace tooreceive nabízených oznámení APNS
1. Přidejte následující řádek toohello hello `didFinishLaunchingWithOptions` metoda:
   
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
2. Přidat hello `didRegisterForRemoteNotificationsWithDeviceToken` metoda následujícím způsobem:
   
        func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
            EngagementAgent.shared().registerDeviceToken(deviceToken)
        }
3. Přidat hello `didReceiveRemoteNotification:fetchCompletionHandler:` metoda následujícím způsobem:
   
        func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
            EngagementAgent.shared().applicationDidReceiveRemoteNotification(userInfo, fetchCompletionHandler:completionHandler)
        }

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-swift-get-started/add-header-file.png
[5]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
[6]: ./media/mobile-engagement-ios-swift-get-started/add-bridging-header.png
