---
title: "aaaGet Začínáme s Azure Mobile Engagementem pro Xamarin.iOS"
description: "Zjistěte, jak toouse Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro aplikace pro Xamarin.iOS."
services: mobile-engagement
documentationcenter: xamarin
author: piyushjo
manager: erikre
editor: 
ms.assetid: 0448209e-fff6-47bd-985c-2cf074bac12f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 02340a744753dcc5cd1b6888a5fa87628be47b68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-xamarinios-apps"></a>Začínáme s Azure Mobile Engagementem pro aplikace Xamarin.iOS
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Toto téma ukazuje, jak Azure Mobile Engagement toounderstand toouse využití aplikací a odesílat nabízená oznámení toosegmented uživatelům aplikace pro Xamarin.iOS.
V tomto kurzu vytvoříte prázdnou aplikaci Xamarin.iOS, která bude shromažďovat základní data a přijímat nabízená oznámení pomocí systému nabízených oznámení Apple (APNS). 

> [!NOTE]
> Služba Azure Mobile Engagement Hello vyřadí března 2018 a je aktuálně pouze k dispozici tooexisting zákazníků. Další informace najdete v tématu [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).

Tento kurz vyžaduje hello následující:

* [Xamarin Studio](http://xamarin.com/studio). Můžete také použít Visual Studio s Xamarinem, ale v tomto kurzu používáme Xamarin Studio. Instalační pokyny najdete v tématu o [nastavení a instalaci pro Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx). 
* [Mobile Engagement Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [!NOTE]
> toocomplete tento kurz, musíte mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).
> 
> 

## <a id="setup-azme"></a>Nastavení Mobile Engagementu pro vaši aplikaci pro iOS
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Připojit vaše aplikace toohello Mobile Engagement back-end
Tento kurzu si představíme "základní integraci", což je minimální hello nastavte požadované toocollect dat a odesílání nabízených oznámení.

Pomocí Xamarin toodemonstrate hello integrace vytvoříme základní aplikaci:

### <a name="create-a-new-xamarinios-project"></a>Vytvoření nového projektu Xamarin.iOS
1. Spusťte Xamarin Studio. Přejděte příliš**soubor** -> **nový** -> **řešení** 
   
    ![][1]
2. Vyberte **jediné zobrazení aplikace**, zkontrolujte, zda je hello vybraný jazyk **C#** a pak klikněte na **Další**.
   
    ![][2]
3. Vyplňte hello **název aplikace** a hello **identifikátor organizace** a pak klikněte na **Další**. 
   
    ![][3]
   
   > [!IMPORTANT]
   > Ujistěte se, že hello publikování profil nakonec použijete toodeploy vaší aplikace pro iOS je pomocí ID aplikace, které odpovídá přesně hello zde máte identifikátor svazku. 
   > 
   > 
4. Aktualizace hello **název projektu**, **název řešení** a **umístění** dle potřeby **vytvořit**.
   
    ![][4]

Xamarin Studio vytvoří hello ukázkovou aplikaci, do které budeme integrovat Mobile Engagement. 

### <a name="connect-your-app-toomobile-engagement-backend"></a>Připojit vaše tooMobile Engagement back-end aplikace
1. Klikněte pravým tlačítkem na hello **balíčky** složku v systému windows hello řešení a vyberte **přidat balíčky...**
   
    ![][5]
2. Vyhledejte hello **Microsoft Azure Mobile Engagement Xamarin SDK** a přidejte ji tooyour řešení.  
   
    ![][6]
3. Otevřete **AppDelegate.cs** a přidejte hello následující příkaz using:
   
        using Microsoft.Azure.Engagement.Xamarin;
4. V hello **FinishedLaunching** metoda, přidejte následující tooinitialize hello připojení s back-end Mobile Engagementu hello. Ujistěte se, že tooadd vaše **ConnectionString**. Tento kód také používá fiktivní **NotificationIcon** který přidává Mobile Engagement SDK, který může být vhodné hello tooreplace. 
   
        EngagementConfiguration config = new EngagementConfiguration {
                        ConnectionString = "YourConnectionStringFromAzurePortal",
                        NotificationIcon = UIImage.FromBundle("close")
                    };
        EngagementAgent.Init (config);

## <a id="monitor"></a>Povolení sledování v reálném čase
V pořadí toostart odesílat data a zajistit, že hello uživatelé jsou aktivní musíte odeslat alespoň jednu obrazovku toohello Mobile Engagement back-end.

1. Otevřete **ViewController.cs** a přidejte hello následující příkaz using:
   
        using Microsoft.Azure.Engagement.Xamarin;
2. Nahraďte hello třídu, ze které `ViewController` dědí z `UIViewController` příliš`EngagementViewController`. 

## <a id="monitor"></a>Připojení aplikace se sledováním v reálném čase
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci
Mobile Engagement umožňuje toointeract s uživateli a REACH s nabízená oznámení a zasílání zpráv v kontextu hello kampaní v aplikaci. Tento modul je hello portálu Mobile Engagement nazývá REACH.
Hello následujících sekcích nastavíte tooreceive vaše aplikace je.

### <a name="modify-your-application-delegate"></a>Úprava delegáta aplikace
1. Otevřete hello **AppDelegate.cs** a přidejte hello následující příkaz using:
   
        using System; 
2. Nyní uvnitř hello `FinishedLaunching` metody přidat hello následující tooregister nabízená oznámení`EngagementAgent.init(...)`
   
        if (UIDevice.CurrentDevice.CheckSystemVersion(8,0))
        {
            var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                (UIUserNotificationType.Badge |
                    UIUserNotificationType.Sound |
                    UIUserNotificationType.Alert),
                null);
            UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications ();
        }
        else
        {
            UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (
                UIRemoteNotificationType.Badge |
                UIRemoteNotificationType.Sound |
                UIRemoteNotificationType.Alert);
        }
3. Nakonec - aktualizujte nebo přidejte hello následující metody:
   
        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, 
            Action<UIBackgroundFetchResult> completionHandler)
        {
            EngagementAgent.ApplicationDidReceiveRemoteNotification(userInfo, completionHandler);
        }
   
        public override void RegisteredForRemoteNotifications (UIApplication application, NSData deviceToken)
        {
            // Register device token on Engagement
            EngagementAgent.RegisterDeviceToken(deviceToken);
        }
   
        public override void FailedToRegisterForRemoteNotifications(UIApplication application, NSError error)
        {
            Console.WriteLine("Failed tooregister for remote notifications: Error '{0}'", error);
        }
4. Ve vaší **Info.plist** souboru v hello řešení, potvrďte, že hello **identifikátor svazku** shoduje s hello **ID aplikace** máte ve svém profilu pro zřizování v hello vývojáře Apple Center. 
   
    ![][7]
5. V hello stejné **Info.plist** souboru, ujistěte se, že jste zkontrolovali hello **povolit režimy pozadí** a **vzdáleného oznámení**. 
   
     ![][8]
6. Spuštění aplikace hello na hello zařízení, kterého jste přidružili s tímto profilem publikování. 

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[1]: ./media/mobile-engagement-xamarin-ios-get-started/new-solution.png
[2]: ./media/mobile-engagement-xamarin-ios-get-started/app-type.png
[3]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-name.png
[4]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-confirm.png
[5]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget.png
[6]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget-azme.png
[7]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-confirm-bundle.png
[8]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-configure-push.png
