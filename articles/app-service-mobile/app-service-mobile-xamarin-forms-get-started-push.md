---
title: "aaaAdd nabízená oznámení tooyour Xamarin.Forms aplikace | Microsoft Docs"
description: "Zjistěte, jak toouse Azure services toosend více platformami nabízená oznámení tooyour Xamarin.Forms aplikace."
services: app-service\mobile
documentationcenter: xamarin
author: ysxu
manager: syntaxc4
editor: 
ms.assetid: d9b1ba9a-b3f2-4d12-affc-2ee34311538b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: yuaxu
ms.openlocfilehash: 9133a0b6dd99c01def525607c20ce5a9c19b9502
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-push-notifications-tooyour-xamarinforms-app"></a>Přidat nabízená oznámení tooyour Xamarin.Forms aplikaci
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Přehled
V tomto kurzu přidáte nabízená oznámení tooall hello projekty, které je výsledkem hello [Xamarin.Forms úvodní](app-service-mobile-xamarin-forms-get-started.md). To znamená, že nabízených oznámení se neodesílají klientů napříč platformami tooall pokaždé, když vložení záznamu.

Pokud nepoužijete hello stáhli úvodní serverový projekt, bude nutné hello nabízených oznámení v balíčku rozšíření. Další informace najdete v tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Požadavky
Pro iOS, budete potřebovat [programu pro vývojáře Apple členství](https://developer.apple.com/programs/ios/) a fyzickém zařízení iOS. Hello [simulátoru iOS nabízená oznámení nepodporuje](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).

## <a name="configure-hub"></a>Konfigurace centra oznámení
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="update-hello-server-project-toosend-push-notifications"></a>Aktualizovat hello serveru projektu toosend nabízená oznámení
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-and-run-hello-android-project-optional"></a>Nakonfigurujte a spusťte hello projekt Android (volitelné)
Dokončení této části tooenable nabízená oznámení pro hello Xamarin.Forms Droid projektu pro Android.

### <a name="enable-firebase-cloud-messaging-fcm"></a>Povolit Firebase Cloud Messaging (FCM)
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

### <a name="configure-hello-mobile-apps-back-end-toosend-push-requests-by-using-fcm"></a>Konfigurace hello Mobile Apps back-end toosend nabízené požadavky pomocí FCM
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

### <a name="add-push-notifications-toohello-android-project"></a>Přidat nabízená oznámení toohello projekt Android
S hello back-end FCM nakonfigurované můžete přidat součásti a kódy tooregister klienta toohello s FCM. Také můžete zaregistrovat pro nabízená oznámení pomocí Azure Notification Hubs prostřednictvím hello Mobile Apps zpět ukončení a přijímat oznámení.

1. V hello **Droid** projektu, klikněte pravým tlačítkem na hello **součásti** složku a klikněte na tlačítko **získat další komponenty...** . Poté vyhledejte hello **klient Google Cloud Messaging** součástí a přidejte ji toohello projektu. Tato součást podporuje nabízená oznámení pro projekt Xamarin Android.
2. Otevřete soubor projektu MainActivity.cs hello a přidejte následující příkaz hello horní části souboru hello hello:

        using Gcm.Client;
3. Přidejte následující kód toohello hello **OnCreate** volání metody po hello příliš**LoadApplication**:

        try
        {
            // Check tooensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            System.Diagnostics.Debug.WriteLine("Registering...");
            GcmClient.Register(this, PushHandlerBroadcastReceiver.SENDER_IDS);
        }
        catch (Java.Net.MalformedURLException)
        {
            CreateAndShowDialog("There was an error creating hello client. Verify hello URL.", "Error");
        }
        catch (Exception e)
        {
            CreateAndShowDialog(e.Message, "Error");
        }
4. Přidejte nový **CreateAndShowDialog** Pomocná metoda, následujícím způsobem:

        private void CreateAndShowDialog(String message, String title)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);

            builder.SetMessage (message);
            builder.SetTitle (title);
            builder.Create().Show ();
        }
5. Přidejte následující kód toohello hello **MainActivity** třídy:

        // Create a new instance field for this activity.
        static MainActivity instance = null;

        // Return hello current activity instance.
        public static MainActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }

    To zpřístupňuje hello aktuální **MainActivity** instance, proto můžete provést na hello hlavního vlákna uživatelského rozhraní.
6. Inicializace hello `instance` proměnné na začátku hello hello **OnCreate** metoda následujícím způsobem.

        // Set hello current instance of MainActivity.
        instance = this;
7. Přidat nové toohello souboru třída **Droid** projektu s názvem `GcmService.cs`a ujistěte se, zda text hello následující **pomocí** příkazy nejsou hello horní části souboru hello:

        using Android.App;
        using Android.Content;
        using Android.Media;
        using Android.Support.V4.App;
        using Android.Util;
        using Gcm.Client;
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Text;
8. Přidejte následující žádosti o oprávnění hello horní části souboru hello po hello hello **pomocí** příkazy a před hello **obor názvů** deklarace.

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
9. Přidejte následující obor názvů toohello definice třídy hello.

       [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, Categories = new string[] { "@PACKAGE_NAME@" })]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, Categories = new string[] { "@PACKAGE_NAME@" })]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, Categories = new string[] { "@PACKAGE_NAME@" })]
       public class PushHandlerBroadcastReceiver : GcmBroadcastReceiverBase<GcmService>
       {
           public static string[] SENDER_IDS = new string[] { "<PROJECT_NUMBER>" };
       }

   > [!NOTE]
   > Nahraďte **< PROJECT_NUMBER >** s vaše číslo projektu, který jste si předtím poznamenali.    
   >
   >
10. Nahraďte hello prázdný **GcmService** se hello následující kód, který používá novou všesměrového vysílání příjemce hello:

         [Service]
         public class GcmService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }

             public GcmService()
                 : base(PushHandlerBroadcastReceiver.SENDER_IDS){}
         }
11. Přidejte následující kód toohello hello **GcmService** třídy. Přepíše hello **OnRegistered** obslužné rutiny události a implementuje **zaregistrovat** metoda.

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose("PushHandlerBroadcastReceiver", "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            var push = TodoItemManager.DefaultManager.CurrentClient.GetPush();

            MainActivity.CurrentActivity.RunOnUiThread(() => Register(push, null));
        }

        public async void Register(Microsoft.WindowsAzure.MobileServices.Push push, IEnumerable<string> tags)
        {
            try
            {
                const string templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";

                JObject templates = new JObject();
                templates["genericMessage"] = new JObject
                {
                    {"body", templateBodyGCM}
                };

                await push.RegisterAsync(RegistrationID, templates);
                Log.Info("Push Installation Id", push.InstallationId.ToString());
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(ex.Message);
                Debugger.Break();
            }
        }

    Všimněte si, že tento kód používá hello `messageParam` parametr v registraci šablony hello.
12. Přidejte následující kód, který implementuje hello **OnMessage**:

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info("PushHandlerBroadcastReceiver", "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            //Store hello message
            var prefs = GetSharedPreferences(context.PackageName, FileCreationMode.Private);
            var edit = prefs.Edit();
            edit.PutString("last_msg", msg.ToString());
            edit.Commit();

            string message = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty(message))
            {
                createNotification("New todo item!", "Todo item: " + message);
                return;
            }

            string msg2 = intent.Extras.GetString("msg");
            if (!string.IsNullOrEmpty(msg2))
            {
                createNotification("New hub message!", msg2);
                return;
            }

            createNotification("Unknown message details", msg.ToString());
        }

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent tooshow ui
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Use Notification Builder
            NotificationCompat.Builder builder = new NotificationCompat.Builder(this);

            //Create hello notification
            //we use hello pending intent, passing our ui intent over which will get called
            //when hello notification is tapped.
            var notification = builder.SetContentIntent(PendingIntent.GetActivity(this, 0, uiIntent, 0))
                    .SetSmallIcon(Android.Resource.Drawable.SymActionEmail)
                    .SetTicker(title)
                    .SetContentTitle(title)
                    .SetContentText(desc)

                    //Set hello notification sound
                    .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))

                    //Auto cancel will remove hello notification once hello user touches it
                    .SetAutoCancel(true).Build();

            //Show hello notification
            notificationManager.Notify(1, notification);
        }

    To zpracovává příchozí oznámení a odešle je toohello oznámení správce toobe zobrazí.
13. **GcmServiceBase** také vyžaduje, abyste tooimplement hello **OnUnRegistered** a **OnError** metody obslužné rutiny, které můžete provést následujícím způsobem:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "Unregistered RegisterationId : " + registrationId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "GCM Error: " + errorId);
        }

Nyní jsou připravené testovací nabízená oznámení v hello aplikaci spuštěnou na zařízení se systémem Android nebo hello emulátor.

### <a name="test-push-notifications-in-your-android-app"></a>Nabízená oznámení v aplikacích pro Android
Hello první dva kroky jsou povinné jenom v případě, že testujete na emulátor.

1. Ujistěte se, že nasazujete tooor ladění na virtuální zařízení, které se má nastavit jako cíl hello rozhraní Google API, jak je vidět níže ve Správci zařízení se systémem Android virtuálním hello.
2. Kliknutím na Přidat zařízení Android toohello účet Google **aplikace** > **nastavení** > **přidejte účet**. Potom postupujte podle pokynů tooadd hello ze stávajících zařízení toohello účet Google nebo toocreate nový.
3. V sadě Visual Studio nebo Xamarin Studio, klikněte pravým tlačítkem na hello **Droid** projektu a klikněte na tlačítko **nastavit jako spouštěný projekt**.
4. Klikněte na tlačítko **spustit** toobuild hello projektu a spusťte aplikaci hello na emulátoru nebo zařízení s Androidem.
5. Aplikace hello typ úlohy a pak klikněte na hello plus (**+**) ikona.
6. Ověřte, že se při přidání položky přijato oznámení.

## <a name="configure-and-run-hello-ios-project-optional"></a>Konfiguraci a spuštění projektu iOS hello (volitelné)
Tato část se týká spuštění projektu Xamarin iOS pro zařízení s iOS hello. Můžete ji přeskočit, pokud s takovými zařízeními nepracujete.

[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

#### <a name="configure-hello-notification-hub-for-apns"></a>Konfigurace centra oznámení hello pro služby APN
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

Dále nakonfigurujete nastavení projektu iOS hello v nástroji Xamarin Studio nebo Visual Studio.

[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

#### <a name="add-push-notifications-tooyour-ios-app"></a>Přidání nabízených oznámení tooyour iOS aplikace
1. V hello **iOS** projektu, otevřete AppDelegate.cs a přidejte následující příkaz toohello horní části souboru kódu hello hello.

        using Newtonsoft.Json.Linq;
2. V hello **AppDelegate** třídy, přidejte přepsání pro hello **RegisteredForRemoteNotifications** tooregister události pro oznámení:

        public override void RegisteredForRemoteNotifications(UIApplication application,
            NSData deviceToken)
        {
            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
                {
                  {"body", templateBodyAPNS}
                };

            // Register for push with your mobile app
            Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }
3. V **AppDelegate**, také přidat hello následující přepsání pro hello **DidReceiveRemoteNotification** obslužné rutiny události:

        public override void DidReceiveRemoteNotification(UIApplication application,
            NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps[new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

    Tato metoda zpracovává příchozí oznámení, když běží aplikace hello.
4. V hello **AppDelegate** třídy, přidejte následující kód toohello hello **FinishedLaunching** metoda:

        // Register for push notifications.
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

    To umožňuje podporu pro vzdálené oznámení a registrace nabízených požadavky.

Vaše aplikace je teď aktualizovaný toosupport nabízená oznámení.

#### <a name="test-push-notifications-in-your-ios-app"></a>Nabízená oznámení v aplikaci s iOS
1. Klikněte pravým tlačítkem na projekt pro iOS hello a klikněte na tlačítko **nastavit jako spouštěný projekt**.
2. Stiskněte klávesu hello **spustit** tlačítko nebo **F5** v sadě Visual Studio toobuild hello projektu a spusťte aplikaci hello v zařízení se systémem iOS. Pak klikněte na tlačítko **OK** tooaccept nabízená oznámení.

   > [!NOTE]
   > Je nutné explicitně přijmout nabízená oznámení z vaší aplikace. Tento požadavek dochází pouze v hello prvním hello aplikace běží.
   >
   >
3. Aplikace hello typ úlohy a pak klikněte na hello plus (**+**) ikona.
4. Ověřte, zda přijetí oznámení a pak klikněte na tlačítko **OK** toodismiss hello oznámení.

## <a name="configure-and-run-windows-projects-optional"></a>Nakonfigurujte a spusťte projekty pro Windows (volitelné)
Tato část se týká spuštění hello Xamarin.Forms WinApp a WinPhone81 projekty pro zařízení se systémem Windows. Tyto kroky také podporují projekty univerzální platformu Windows (UWP). Můžete ji přeskočit, pokud s takovými zařízeními nepracujete.

#### <a name="register-your-windows-app-for-push-notifications-with-windows-notification-service-wns"></a>Registrace aplikace systému Windows pro nabízená oznámení pomocí služby oznámení Windows (WNS)
[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

#### <a name="configure-hello-notification-hub-for-wns"></a>Konfigurace centra oznámení hello u WNS
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="add-push-notifications-tooyour-windows-app"></a>Přidat nabízená oznámení tooyour Windows aplikaci
1. V sadě Visual Studio otevřete **App.xaml.cs** v systému Windows projekt a přidejte následující příkazy hello.

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
        using <your_TodoItemManager_portable_class_namespace>;

    Nahraďte `<your_TodoItemManager_portable_class_namespace>` s oborem názvů hello přenosné projektu obsahující hello `TodoItemManager` třídy.
2. V souboru App.xaml.cs přidejte následující hello **InitNotificationsAsync** metoda:

        private async Task InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            const string templateBodyWNS =
                "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            JObject headers = new JObject();
            headers["X-WNS-Type"] = "wns/toast";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyWNS},
                {"headers", headers} // Needed for WNS.
            };

            await TodoItemManager.DefaultManager.CurrentClient.GetPush()
                .RegisterAsync(channel.Uri, templates);
        }

    Tato metoda získá kanál hello nabízená oznámení a zaregistruje šablony tooreceive šablony oznámení z centra oznámení. Šablony oznámení, která podporuje *messageParam* budou doručeny toothis klienta.
3. V souboru App.xaml.cs aktualizace hello **OnLaunched** definici metody obslužné rutiny událostí přidáním hello `async` modifikátor. Pak přidejte následující řádek kódu na konci hello hello metody hello:

        await InitNotificationsAsync();

    Zajistíte tak, že se vytvoří nebo aktualizují při každém spuštění aplikace hello hello registrace nabízených oznámení. Je důležité toodo tento tooguarantee, který hello WNS nabízené kanál je vždy aktivní.  
4. V Průzkumníku řešení pro sadu Visual Studio otevřete hello **Package.appxmanifest** souboru a nastavit **informační podporující** příliš**Ano** pod **oznámení**.
5. Sestavení aplikace hello a ověřte, že máte žádné chyby. Klientská aplikace by měl nyní registrovat hello šablony oznámení od hello, že end Mobile Apps zpět. Tato část opakujte pro každý projekt Windows ve vašem řešení.

#### <a name="test-push-notifications-in-your-windows-app"></a>Nabízená oznámení v aplikaci Windows
1. V sadě Visual Studio, klikněte pravým tlačítkem na projekt Windows a klikněte na tlačítko **nastavit jako spouštěný projekt**.
2. Stiskněte klávesu hello **spustit** tlačítko toobuild hello projektu a spusťte aplikaci hello.
3. V aplikaci hello, zadejte název nové todoitem a pak klikněte na tlačítko hello plus (**+**) ikona tooadd ho.
4. Ověřte, že se při přidání položky hello přijato oznámení.

## <a name="next-steps"></a>Další kroky
Další informace o nabízených oznámení:

* [Diagnostikovat problémy nabízená oznámení](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  Existují různé důvody, proč oznámení může získat vyřadit ani nekončí na zařízení. Toto téma ukazuje, jak tooanalyze a vyřešení hello kořenové způsobit selhání nabízená oznámení.

Můžete také pokračovat na tooone hello následující kurzy:

* [Přidat aplikaci tooyour ověřování](app-service-mobile-xamarin-forms-get-started-users.md)  
  Zjistěte, jak tooauthenticate uživatele vaší aplikace pomocí zprostředkovatele identity.
* [Povolení offline synchronizace u aplikace](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Zjistěte, jak tooadd offline podporu pro vaši aplikaci pomocí Mobile Apps back-end. S offline synchronizací, mohou uživatelé komunikovat s mobilní aplikací&mdash;zobrazení, přidávat a upravovat data&mdash;i v případě, že není žádné síťové připojení.

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
