---
title: "aplikace Xamarin.iOS aaaAdd nabízená oznámení tooyour službou Azure App Service"
description: "Zjistěte, jak Azure App Service toosend toouse nabízená oznámení aplikace Xamarin.iOS tooyour"
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 2921214a-49f8-45e1-a306-a85ce21defca
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: glenga
ms.openlocfilehash: 3e6439aee4f3fe0f60b9786d0bbfd74c4f5e52d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-push-notifications-tooyour-xamarinios-app"></a>Přidat nabízená oznámení tooyour aplikace Xamarin.iOS
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Přehled
V tomto kurzu přidáte nabízená oznámení toohello [Xamarin.iOS úvodní](app-service-mobile-xamarin-ios-get-started.md) projektu tak, aby nabízených oznámení je odesláno toohello zařízení pokaždé, když vložení záznamu.

Pokud nepoužijete hello stáhli úvodní serverový projekt, bude nutné hello nabízených oznámení v balíčku rozšíření. V tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) Další informace.

## <a name="prerequisites"></a>Požadavky
* Dokončení hello [rychlý start Xamarin.iOS](app-service-mobile-xamarin-ios-get-started.md) kurzu.
* Fyzickém zařízení iOS. Nabízená oznámení nepodporuje hello simulátoru iOS.

## <a name="register-hello-app-for-push-notifications-on-apples-developer-portal"></a>Zaregistrovat hello aplikace pro nabízená oznámení na portál pro vývojáře společnosti Apple
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-your-mobile-app-toosend-push-notifications"></a>Konfigurace mobilní aplikace toosend nabízených oznámení
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <a name="update-hello-server-project-toosend-push-notifications"></a>Aktualizovat hello serveru projektu toosend nabízená oznámení
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-your-xamarinios-project"></a>Konfigurace projektu Xamarin.iOS
[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

## <a name="add-push-notifications-tooyour-app"></a>Přidat nabízená oznámení tooyour aplikaci
1. V **QSTodoService**, přidejte následující vlastnost hello tak, aby **AppDelegate** můžete získat hello mobilního klienta:
   
            public MobileServiceClient GetClient {
            get
            {
                return client;
            }
            private set
            {
                client = value;
            }
        }
2. Přidejte následující hello `using` příkaz toohello začátek hello **AppDelegate.cs** souboru.
   
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
3. V **AppDelegate**, přepsání hello **FinishedLaunching** událostí:
   
        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            // registers for push for iOS8
            var settings = UIUserNotificationSettings.GetSettingsForTypes(
                UIUserNotificationType.Alert
                | UIUserNotificationType.Badge
                | UIUserNotificationType.Sound,
                new NSSet());
   
            UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications();
   
            return true;
        }
4. V hello stejného souboru, přepsání hello **RegisteredForRemoteNotifications** událostí. V tomto kódu jsou registrace pro jednoduchou šablonu oznámení, která bude odeslána pro všechny podporované platformy serverem hello.
   
    Další informace o šablonách s Notification Hubs najdete v tématu [šablony](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).

        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            MobileServiceClient client = QSTodoService.DefaultService.GetClient;

            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyAPNS}
            };

            // Register for push with your mobile app
            var push = client.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }


1. Potom přepsat hello **DidReceivedRemoteNotification** událostí:
   
        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;
   
            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps [new NSString("alert")] as NSString).ToString();
   
            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

Vaše aplikace je teď aktualizovaný toosupport nabízená oznámení.

## <a name="test"></a>Nabízená oznámení v aplikaci
1. Stiskněte klávesu hello **spustit** tlačítko toobuild hello projektu a spusťte aplikaci hello v podporuje zařízení s iOS a pak klikněte na **OK** tooaccept nabízená oznámení.
   
   > [!NOTE]
   > Je nutné explicitně přijmout nabízená oznámení z vaší aplikace. Tento požadavek dochází pouze v hello prvním hello aplikace běží.
   > 
   > 
2. Aplikace hello typ úlohy a pak klikněte na hello plus (**+**) ikona.
3. Ověřte, že přijetí oznámení a pak klikněte na **OK** toodismiss hello oznámení.
4. Opakujte krok 2 a okamžitě zavřít hello aplikaci a potom ověřte, zda je zobrazen oznámení.

Úspěšně jste dokončili tento kurz.

<!-- Images. -->

<!-- URLs. -->



