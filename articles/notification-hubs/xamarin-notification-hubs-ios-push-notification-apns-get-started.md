---
title: "aaaiOS nabízená oznámení pomocí Notification Hubs pro aplikace Xamarin | Microsoft Docs"
description: "V tomto kurzu zjistíte, jak Azure Notification Hubs toosend toouse nabízená oznámení tooa Xamarin iOS aplikace."
services: notification-hubs
keywords: "nabízená oznámení ios,nabízení zpráv,nabízená oznámení,nabízená zpráva"
documentationcenter: xamarin
author: ysxu
manager: erikre
editor: 
ms.assetid: 4d4dfd42-c5a5-4360-9d70-7812f96924d2
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 8db60338047dd53074b4d3d4bb127aa6d9f13a25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="ios-push-notifications-with-notification-hubs-for-xamarin-apps"></a>Nabízená oznámení iOS s centry oznámení pro aplikace Xamarin
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Přehled
> [!IMPORTANT]
> toocomplete tento kurz, musíte mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).
> 
> 

Tento kurz ukazuje, jak Azure Notification Hubs toosend toouse nabízená oznámení tooan iOS aplikace.
Vytvoříte prázdnou aplikaci Xamarin.iOS, která přijímá nabízená oznámení pomocí hello [Apple Push Notification Service (APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html). Jakmile budete hotovi, budete moct toouse vaše toobroadcast centra oznámení nabízená oznámení tooall hello zařízení používající vaši aplikaci. Hello Dokončený kód je k dispozici v hello [aplikace NotificationHubs] [ GitHub] ukázka.

Tento kurz představuje hello jednoduchého zprávy všesměrového vysílání scénář s Notification Hubs.

## <a name="prerequisites"></a>Požadavky
Tento kurz vyžaduje hello následující:

* [Xcode 6.0][Install Xcode]
* Zařízení kompatibilní s iOS 7.0 (nebo novější verze)
* Členství v programu pro vývojáře iOS.
* [Xamarin Studio]
  
  > [!NOTE]
  > Z důvodu požadavky na konfiguraci pro nabízená oznámení iOS musíte nasadit a otestovat vzorovou aplikaci hello na fyzickém zařízení iOS (iPhone nebo iPad) místo v simulátoru hello.
  > 
  > 

Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy Notification Hubs pro Xamarin iOS Apps.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub"></a>Konfigurace centra oznámení
Tato část vás provede vytvořením nového centra oznámení a konfiguraci ověřování pomocí služby APNS pomocí hello **.p12** nabízený certifikát, který jste vytvořili. Pokud chcete toouse Centrum oznámení, které jste už vytvořili, můžete přeskočit toostep 5.

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="7">

<li>

<p>Jak chceme připojení služby APN hello tooconfigure, v hello portálu Azure, otevřete své nastavení centra oznámení, a klikněte na <b>služby oznámení</b>a potom klikněte na hello <b>Apple (APNS)</b> položku v seznamu hello. Po dokončení klikněte na <b>nahrát certifikát</b> a vyberte hello <b>.p12</b> certifikát, který jste exportovali dříve, a také hello heslo pro certifikát hello.</p>

<p>Ujistěte se, že tooselect <b>izolovaného prostoru</b> režimu vzhledem k tomu, že budete odesílat nabízené zprávy ve vývojovém prostředí. Používat pouze hello <b>produkční</b> nastavení, pokud chcete, aby toousers toosend nabízená oznámení, který již zakoupili aplikaci z úložiště hello.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-apns.png)

&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-sandbox.png)

Vaše centrum oznámení je nyní nakonfigurované toowork pomocí služby APNS a máte hello připojovací řetězce tooregister vaší aplikace a odesílání nabízených oznámení.

## <a name="connect-your-app-toohello-notification-hub"></a>Připojit vaše Centrum oznámení toohello aplikace
#### <a name="create-a-new-project"></a>Vytvoření nového projektu
1. V Xamarin studiu vytvořte nový projekt iOS a vyberte hello **unifikované API** > **jediné zobrazení aplikace** šablony.
   
     ![Xamarin Studio – výběr typu aplikace][31]
2. Přidáte součást zasílání zpráv Azure toohello odkaz. V zobrazení řešení hello, klikněte pravým tlačítkem na hello **součásti** složky pro váš projekt a zvolte **získat více komponent**. Vyhledejte hello **zasílání zpráv Azure** součástí a přidejte hello součást tooyour projektu.
3. V **AppDelegate.cs**, přidejte hello následující příkaz using:
   
        using WindowsAzure.Messaging;
4. Deklarujte instanci **SBNotificationHub**:
   
        private SBNotificationHub Hub { get; set; }
5. Vytvoření **Constants.cs** se hello následující proměnné:
   
        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";
6. V **AppDelegate.cs**, aktualizovat **FinishedLaunching()** toomatch hello následující:
   
        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());
   
                UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
                UIApplication.SharedApplication.RegisterForRemoteNotifications ();
            } else {
                UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
                UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
            }
   
            return true;
        }
7. Přepsání hello **RegisteredForRemoteNotifications()** metoda v **AppDelegate.cs**:
   
        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            Hub = new SBNotificationHub(Constants.ConnectionString, Constants.NotificationHubPath);
   
            Hub.UnregisterAllAsync (deviceToken, (error) => {
                if (error != null)
                {
                    Console.WriteLine("Error calling Unregister: {0}", error.ToString());
                    return;
                }
   
                NSSet tags = null; // create tags if you want
                Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) => {
                    if (errorCallback != null)
                        Console.WriteLine("RegisterNativeAsync error: " + errorCallback.ToString());
                });
            });
        }
8. Přepsání hello **Receivedremotenotifications()** metoda v **AppDelegate.cs**:
   
        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }
9. Vytvořte následující hello **ProcessNotification()** metoda v **AppDelegate.cs**:
   
        void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
        {
            // Check toosee if hello dictionary has hello aps key.  This is hello notification payload you would have sent
            if (null != options && options.ContainsKey(new NSString("aps")))
            {
                //Get hello aps dictionary
                NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;
   
                string alert = string.Empty;
   
                //Extract hello alert text
                // NOTE: If you're using hello simple alert by just specifying
                // "  aps:{alert:"alert msg here"}  ", this will work fine.
                // But if you're using a complex alert with Localization keys, etc.,
                // your "alert" object from hello aps dictionary will be another NSDictionary.
                // Basically hello JSON gets dumped right into a NSDictionary,
                // so keep that in mind.
                if (aps.ContainsKey(new NSString("alert")))
                    alert = (aps [new NSString("alert")] as NSString).ToString();
   
                //If this came from hello ReceivedRemoteNotification while hello app was running,
                // we of course need toomanually process things like hello sound, badge, and alert.
                if (!fromFinishedLaunching)
                {
                    //Manually show an alert
                    if (!string.IsNullOrEmpty(alert))
                    {
                        UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                        avAlert.Show();
                    }
                }
            }
        }
   
   > [!NOTE]
   > Můžete zvolit toooverride **FailedToRegisterForRemoteNotifications()** toohandle situací jako chybějící síťové připojení. To je obzvláště důležité, kde hello uživatel mohl spustit aplikaci v režimu offline (například letadlo) a chcete zpráv scénáře konkrétní tooyour aplikace toohandle oznámení.
   > 
   > 
10. Spuštění aplikace hello na vašem zařízení.

## <a name="sending-push-notifications"></a>Odeslání nabízených oznámení
Můžete otestovat přijímání nabízených oznámení ve vaší aplikaci odesláním oznámení hello [portálu Azure] prostřednictvím hello **testovací odeslání** funkci hello **Poradce při potížích s** Sada nástrojů, přímo na stránce centra oznámení hello, jak je uvedené v hello obrazovce níže.

![](./media/notification-hubs-ios-get-started/notification-hubs-test-send.png)

Nabízená oznámení se většinou posílají pomocí služby backend, jako je služba Mobile Services, nebo v technologii ASP.NET pomocí kompatibilní knihovny. Můžete taky hello REST API přímo toosend nabízené zprávy, pokud není k dispozici ve vašem scénáři knihovny. 

V tomto kurzu jsme dělat nic složitého a jednoduše předvedeme testování vaší klientské aplikace pomocí odesílání oznámení hello .NET SDK pro centra oznámení v konzolové aplikaci, místo back-end službu. Doporučujeme, abyste hello [použití centra oznámení toopush oznámení toousers](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) kurz jako hello další krok pro odesílání oznámení z backendu ASP.NET. Hello následující přístupy lze však použít pro zasílání oznámení:

* **Rozhraní REST**: nabízené oznámení můžete podporovat na jakékoli platformě back-end pomocí hello [rozhraní REST](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).
* **Microsoft Azure oznámení centra .NET SDK**: hello Správce balíčků Nuget pro Visual Studio, spusťte [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).
* **Node.js** : [jak toouse Notification Hubs z Node.js](notification-hubs-nodejs-push-notification-tutorial.md).

**Mobile Apps**: pro příklad toosend oznámení z backendu Azure App Service Mobile Apps, které jsou integrovány v centrech oznámení najdete v části [přidat nabízená oznámení tooyour mobilní aplikace](../app-service-mobile/app-service-mobile-ios-get-started-push.md).

* **Java / PHP**: Příklad jak hello toosend nabízená oznámení pomocí rozhraní REST API, naleznete v části "jak toouse centra oznámení z Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

#### <a name="optional-send-push-notifications-from-a-net-console-app"></a>(Volitelné) Odesílání nabízených oznámení z konzoly aplikace .NET
V této části odešleme nabízená oznámení pomocí konzolové aplikace .NET Pro účely hello tohoto příkladu přepněme tooa vývojového prostředí systému Windows, který má nainstalované Visual Studio.

1. Ve Visual Studiu vytvořte novou konzolovou aplikaci Visual C#:
   
       ![Visual Studio - Create a new console application][213]
2. Ve Visual Studiu klikněte na položku **Nástroje**, klikněte na **Správce balíčků NuGet** a pak klikněte na **Konzola Správce balíčků**.
   
    konzole Správce balíčků Hello by se zobrazit ukotvený toohello dolní části pracovního prostoru Visual Studia.
3. V okně konzoly Správce balíčků hello, nastavte hello **výchozí projekt** tooyour nové konzoly projekt aplikace a potom v okně konzoly hello, spusťte následující příkaz hello:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Tento postup přidá odkaz toohello SDK centra oznámení Azure pomocí hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">balíčku Microsoft.Azure.Notification Hubs NuGet</a>.
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. Otevřete hello `Program.cs` souboru a přidejte následující hello `using` prohlášení, zajistíte, že budeme moci použít třídy a funkce v rámci hlavní třídy Azure:
   
        using Microsoft.Azure.NotificationHubs;
5. Ve vašem `Program` třídy, přidejte následující metodu hello (Nezapomeňte tooreplace hello **připojovací řetězec** a **název centra**):
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var alert = "{\"aps\":{\"alert\":\"Hello from .NET!\"}}";
            await hub.SendAppleNativeNotificationAsync(alert);
        }
6. Přidejte následující řádky do hello vaší `Main` metoda:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Stisknutím klávesy hello F5 klíče toorun hello aplikace. Během několika sekund měli byste vidět nabízená oznámení v zařízení. Ať používáte Wi-Fi nebo mobilních datovou síť, ujistěte se, že máte na hello zařízení aktivní připojení k Internetu.

Můžete najít všechny možné datové části hello v hello Apple [průvodci místních a nabízených oznámení programování].

#### <a name="optional-send-notifications-from-a-mobile-service"></a>(Volitelné) Odesílání oznámení z mobilní služby
V této části vám odešleme nabízená oznámení pomocí mobilních služeb prostřednictvím skriptu uzlu.

postupujte podle toosend oznámení pomocí mobilních služeb, [Začínáme s Mobile Services]a pak:

1. Přihlaste se toohello [portálu Azure Classic]a vyberte mobilní služby.
2. Vyberte hello **Plánovač** karty v horní části hello.
   
       ![Azure Classic Portal - Scheduler][215]
3. Vytvořte novou naplánovanou úlohu, vložte název a vyberte **Na vyžádání**.
   
       ![Azure Classic Portal - Create new job][216]
4. Když se vytvoří úloha hello, klikněte na název úlohy hello. Pak klikněte na tlačítko hello **skriptu** karty na horním panelu hello.
5. Vložte následující skript dovnitř funkce plánovače hello. Ujistěte se, že tooreplace zástupné symboly hello oznámení centra název a hello připojovacím řetězcem pro *DefaultFullSharedAccessSignature* kterou jste získali dříve. Klikněte na **Uložit**.
   
        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<Hubname>', '<SAS Full access >');
        notificationHubService.apns.send(
            null,
            {"aps":
                {
                  "alert": "Hello from Mobile Services!"
                }
            },
            function (error)
            {
                if (!error) {
                    console.warn("Notification successful");
                }
            }
        );
6. Klikněte na tlačítko **spustit jednou** na dolním panelu hello. Měli byste obdržet upozornění na vašem zařízení.

## <a name="next-steps"></a>Další kroky
V tomto jednoduchém příkladu jste vysílali nabízená oznámení tooall zařízení iOS. V pořadí tootarget konkrétní uživatele, najdete v kurzu toohello [použití centra oznámení toopush oznámení toousers]. Pokud chcete toosegment uživatele podle zájmových skupin, můžete si přečíst [toosend použití centra oznámení nejnovější zprávy přes]. Další informace o toouse centra oznámení v [pokyny centra oznámení] a v hello [iOS Notification Hubs postupy toofor].

<!-- Images. -->

[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png

[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png


[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png




<!-- URLs. -->
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Začínáme s Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-ios
[portálu Azure Classic]: https://manage.windowsazure.com/
[pokyny centra oznámení]: http://msdn.microsoft.com/library/jj927170.aspx
[iOS Notification Hubs postupy toofor]: http://msdn.microsoft.com/library/jj927168.aspx
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[použití centra oznámení toopush oznámení toousers]: /manage/services/notification-hubs/notify-users-aspnet
[toosend použití centra oznámení nejnovější zprávy přes]: /manage/services/notification-hubs/breaking-news-dotnet

[průvodci místních a nabízených oznámení programování]:https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/HandlingRemoteNotifications.html#//apple_ref/doc/uid/TP40008194-CH6-SW1
[Apple Push Notification Service]: http://go.microsoft.com/fwlink/p/?LinkId=272584

[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Xamarin Studio]: http://xamarin.com/download
[WindowsAzure.Messaging]: https://github.com/infosupport/WindowsAzure.Messaging.iOS
[portálu Azure]: https://portal.azure.com
