---
title: "aaaGet začít s Notification Hubs pro aplikace pro Xamarin.Android | Microsoft Docs"
description: "V tomto kurzu zjistíte, jak Azure Notification Hubs toosend toouse nabízená oznámení tooa aplikace Xamarin Android."
author: ysxu
manager: erikre
editor: 
services: notification-hubs
documentationcenter: xamarin
ms.assetid: 0be600fe-d5f3-43a5-9e5e-3135c9743e54
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: c5c7ead9a9381ab9fb6bbe86e930a8a9254813d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-notification-hubs-with-xamarin-for-android"></a>Začínáme s centry oznámení se sadou Xamarin pro Android
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Přehled
Tento kurz ukazuje, jak Azure Notification Hubs toosend toouse nabízená oznámení tooa Xamarin.Android aplikace.
Vytvoříte prázdnou aplikaci systému Xamarin.Android, která bude přijímat nabízená oznámení pomocí služby GCM (Google Cloud Messaging). Jakmile budete hotovi, budete moct toouse vaše toobroadcast centra oznámení nabízená oznámení tooall hello zařízení používající vaši aplikaci. Hello Dokončený kód je k dispozici v hello [aplikace NotificationHubs] [ GitHub] ukázka.

Tento kurz představuje scénář jednoduchého vysílání hello přes centra oznámení.

## <a name="before-you-begin"></a>Než začnete
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Hello dokončit kód v tomto kurzu lze nalézt na Githubu [zde](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid).

## <a name="prerequisites"></a>Požadavky
Tento kurz vyžaduje hello následující:

* Visual Studio s Xamarinem v systému Windows nebo Xamarin Studio v systému Mac OS X. Úplné pokyny k instalaci najdete v tématu [Instalační program a instalace Visual Studia a Xamarinu](https://msdn.microsoft.com/library/mt613162.aspx).
* Aktivní účet Google
* [Komponenta zasílání zpráv Azure]
* [Komponenta klienta zasílání zpráv cloudu Google]

Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy Notification Hubs pro Xamarin.Android Apps.

> [!IMPORTANT]
> toocomplete tento kurz, musíte mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).
> 
> 

## <a name="enable-google-cloud-messaging"></a>Povolení služby GCM (Google Cloud Messaging)
[!INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

## <a name="configure-your-notification-hub"></a>Konfigurace centra oznámení
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="7">

<li><p>Klikněte na tlačítko hello <b>konfigurace</b> hello horní, zadejte hello <b>klíč rozhraní API</b> hodnotou, kterou jste získali v předchozí části hello a pak klikněte na <b>Uložit</b>.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)

Vaše centrum oznámení je teď nakonfigurovaná toowork s GCM a máte hello připojovací řetězce tooboth zaregistrovat aplikaci tooreceive oznámení a toosend nabízená oznámení.

## <a name="connect-your-app-toohello-notification-hub"></a>Připojit vaše Centrum oznámení toohello aplikace
### <a name="create-a-new-project"></a>Vytvoření nového projektu
1. V Xamarin Studiu klikněte na tlačítko **Nové řešení**, klikněte na tlačítko **Aplikace pro Android** a klikněte na tlačítko **Další**.
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png)

2. Zadejte **Název aplikace** a **Identifikátor**. Klikněte na tlačítko hello **cílové plaformy** toosupport a pak klikněte na **Další** a **vytvořit**.
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png)

    Dojde k vytvoření nového projektu pro Android.

1. Otevřete vlastnosti projektu hello tak, že kliknete pravým tlačítkem na nový projekt v hello zobrazení řešení a zvolte **možnosti**. Vyberte hello **aplikace pro Android** položky v hello **sestavení** části.
   
    Ujistěte se, že hello první písmeno vaše **název balíčku** je malá písmena.
   
   > [!IMPORTANT]
   > první písmeno názvu balíčku hello Hello musí být malé. Jinak se zobrazí chyby manifestu aplikace při registraci vašeho **BroadcastReceiver** a **IntentFilter** pro nabízená oznámení níže.
   > 
   > 
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub--xamarin-android-app-options.png)
2. Volitelně můžete sadu hello **minimální verzi systému Android** tooanother úroveň rozhraní API.
3. Volitelně můžete sadu hello **cílovou verzi systému Android** toohello jinou verzi rozhraní API, které chcete tootarget (musí být úroveň rozhraní API 8 nebo vyšší).

Klikněte na tlačítko **OK** a dialogové okno Možnosti projektu zavřít hello.

### <a name="add-hello-required-components-tooyour-project"></a>Přidání hello požadované součásti tooyour projektu
Hello Google Cloud zasílání zpráv klienta k dispozici na hello úložišti součástí Xamarin zjednodušuje proces podpory nabízených oznámení v Xamarin.Android hello.

1. Klikněte pravým tlačítkem na složku komponenty hello v aplikaci Xamarin.Android a vyberte **získat více komponent**.
2. Vyhledejte hello **zasílání zpráv Azure** součástí a přidejte ji toohello projektu.
3. Vyhledejte hello **klient Google Cloud Messaging** součástí a přidejte ji toohello projektu.

### <a name="set-up-notification-hubs-in-your-project"></a>Nastavte centra oznámení v projektu
1. Shromážděte následující informace pro vaše aplikace a oznámení Android Centrum hello:
   
   * **GoogleProjectNumber**: získat tuto hodnotu čísla projektu z hello přehledu své aplikace na hello portál pro vývojáře Google. Jste si poznamenali tuto hodnotu dříve při vytvoření aplikace hello na portálu hello.
   * **Připojovací řetězec naslouchání**: na řídicím panelu hello v hello [portálu Azure Classic], klikněte na tlačítko **zobrazit připojovací řetězce**. Kopírování hello *DefaultListenSharedAccessSignature* připojovací řetězec pro tuto hodnotu.
   * **Název centra**: Toto je název centra z hello hello [portálu Azure Classic]. Například *mynotificationhub2*.
     
     Vytvoření **Constants.cs** třídy pro váš projekt Xamarin a definujte následující hodnoty konstant ve třídě hello hello. Nahraďte zástupné symboly hello svoje hodnoty.
     
       public static class Constants   {
     
           public const string SenderID = "<GoogleProjectNumber>"; // Google API Project Number
           public const string ListenConnectionString = "<Listen connection string>";
           public const string NotificationHubName = "<hub name>";
       }
2. Přidat hello následující příkazy using příliš**MainActivity.cs**:
   
        using Android.Util;
        using Gcm.Client;
3. Přidat proměnné toohello instance `MainActivity` třídu, která bude použité tooshow dialogového okna výstrah při spuštěné aplikaci hello:
   
        public static MainActivity instance;
4. Vytvořte následující metodu v hello hello **MainActivity** třídy:
   
        private void RegisterWithGCM()
        {
            // Check tooensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);
   
            // Register for push notifications
            Log.Info("MainActivity", "Registering...");
            GcmClient.Register(this, Constants.SenderID);
        }
5. V hello `OnCreate` metodu **MainActivity.cs**, inicializovat hello `instance` proměnnou a přidejte volání příliš`RegisterWithGCM`:
   
        protected override void OnCreate (Bundle bundle)
        {
            instance = this;
   
            base.OnCreate (bundle);
   
            // Set your view from hello "main" layout resource
            SetContentView (Resource.Layout.Main);
   
            // Get your button from hello layout resource,
            // and attach an event tooit
            Button button = FindViewById<Button> (Resource.Id.myButton);
   
            RegisterWithGCM();
        }
6. Vytvořte novou třídu **MyBroadcastReceiver**.
   
   > [!NOTE]
   > Projdeme s vámi vytvoření třídy **BroadcastReceiver** od začátku níže. Ale rychlé alternativní toomanually vytváření **MyBroadcastReceiver.cs** je toorefer toohello **GcmService.cs** nacházející se v projektu vzorku Xamarin.Android hello součástí hello [Vzorků NotificationHubs][GitHub]. Duplikování **GcmService.cs** a změna názvů tříd může být také toostart skvělým místem.
   > 
   > 
7. Přidat hello následující příkazy using příliš**MyBroadcastReceiver.cs** (odkazující toohello komponentu a sestavení, které jste přidali dříve):
   
        using System.Collections.Generic;
        using System.Text;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Gcm.Client;
        using WindowsAzure.Messaging;
8. V **MyBroadcastReceiver.cs**, přidejte následující žádosti oprávnění mezi hello hello **pomocí** příkazů a hello **obor názvů** deklarace:
   
        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
   
        //GET_ACCOUNTS is needed only for Android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
9. V **MyBroadcastReceiver.cs**, změňte hello **MyBroadcastReceiver** toomatch hello následující třídy:
   
        [BroadcastReceiver(Permission=Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        public class MyBroadcastReceiver : GcmBroadcastReceiverBase<PushHandlerService>
        {
            public static string[] SENDER_IDS = new string[] { Constants.SenderID };
   
            public const string TAG = "MyBroadcastReceiver-GCM";
        }
10. Přidejte jinou třídu do souboru **MyBroadcastReceiver.cs** s názvem **PushHandlerService**, která je odvozena z **GcmServiceBase**. Ujistěte se, zda text hello tooapply **služby** toohello třídy atributů:
    
         [Service] // Must use hello service tag
         public class PushHandlerService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }
             private NotificationHub Hub { get; set; }
    
             public PushHandlerService() : base(Constants.SenderID)
                {
                 Log.Info(MyBroadcastReceiver.TAG, "PushHandlerService() constructor");
             }
         }
11. **GcmServiceBase** implementuje metody **OnRegistered()**, **OnUnRegistered()**, **OnMessage()**, **OnRecoverableError()** a **OnError()**. Naše **PushHandlerService** třída implementace musí přepsat tyto metody a tyto metody se aktivují v odpovědi toointeracting hello centra oznámení.
12. Přepsání hello **OnRegistered()** metoda v **PushHandlerService** pomocí hello následující kód:
    
         protected override void OnRegistered(Context context, string registrationId)
         {
             Log.Verbose(MyBroadcastReceiver.TAG, "GCM Registered: " + registrationId);
             RegistrationID = registrationId;
    
             createNotification("PushHandlerService-GCM Registered...",
                                 "hello device has been Registered!");
    
             Hub = new NotificationHub(Constants.NotificationHubName, Constants.ListenConnectionString,
                                         context);
             try
             {
                 Hub.UnregisterAll(registrationId);
             }
             catch (Exception ex)
             {
                 Log.Error(MyBroadcastReceiver.TAG, ex.Message);
             }
    
             //var tags = new List<string>() { "falcons" }; // create tags if you want
             var tags = new List<string>() {};
    
             try
             {
                 var hubRegistration = Hub.Register(registrationId, tags.ToArray());
             }
             catch (Exception ex)
             {
                 Log.Error(MyBroadcastReceiver.TAG, ex.Message);
             }
         }
    
    > [!NOTE]
    > V hello **OnRegistered()** kódu výše, nezapomeňte hello možnost toospecify značky tooregister konkrétních kanálů zasílání zpráv.
    > 
    > 
13. Přepsání hello **OnMessage** metoda v **PushHandlerService** pomocí hello následující kód:
    
        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info(MyBroadcastReceiver.TAG, "GCM Message Received!");
    
            var msg = new StringBuilder();
    
            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }
    
            string messageText = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty (messageText))
            {
                createNotification ("New hub message!", messageText);
            }
            else
            {
                createNotification ("Unknown message details", msg.ToString ());
            }
        }
14. Přidejte následující hello **createNotification** a **dialogNotify** metody příliš**PushHandlerService** pro upozornění uživatelů při obdržení oznámení.
    
    > [!NOTE]
    > Návrh oznámení v systému Android verze 5.0 a novější představuje významné odchýlení od předchozích verzí. Pokud testujete to v systému Android 5.0 nebo novější, bude nutné aplikace hello toobe spuštěny tooreceive hello oznámení. Další informace naleznete v tématu [Oznámení systému Android](http://go.microsoft.com/fwlink/?LinkId=615880).
    > 
    > 
    
        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;
    
            //Create an intent tooshow UI
            var uiIntent = new Intent(this, typeof(MainActivity));
    
            //Create hello notification
            var notification = new Notification(Android.Resource.Drawable.SymActionEmail, title);
    
            //Auto-cancel will remove hello notification once hello user touches it
            notification.Flags = NotificationFlags.AutoCancel;
    
            //Set hello notification info
            //we use hello pending intent, passing our ui intent over, which will get called
            //when hello notification is tapped.
            notification.SetLatestEventInfo(this, title, desc, PendingIntent.GetActivity(this, 0, uiIntent, 0));
    
            //Show hello notification
            notificationManager.Notify(1, notification);
            dialogNotify (title, desc);
        }
    
        protected void dialogNotify(String title, String message)
        {
    
            MainActivity.instance.RunOnUiThread(() => {
                AlertDialog.Builder dlg = new AlertDialog.Builder(MainActivity.instance);
                AlertDialog alert = dlg.Create();
                alert.SetTitle(title);
                alert.SetButton("Ok", delegate {
                    alert.Dismiss();
                });
                alert.SetMessage(message);
                alert.Show();
            });
        }
15. Potlačte abstraktní členy **OnUnRegistered()**, **OnRecoverableError()** a **OnError()** tak, aby se váš kód zkompiloval:
    
        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Unregistered: " + registrationId);
    
            createNotification("GCM Unregistered...", "hello device has been unregistered!");
        }
    
        protected override bool OnRecoverableError(Context context, string errorId)
        {
            Log.Warn(MyBroadcastReceiver.TAG, "Recoverable Error: " + errorId);
    
            return base.OnRecoverableError (context, errorId);
        }
    
        protected override void OnError(Context context, string errorId)
        {
            Log.Error(MyBroadcastReceiver.TAG, "GCM Error: " + errorId);
        }

## <a name="run-your-app-in-hello-emulator"></a>Spusťte aplikaci v emulátoru hello
Pokud spustíte tuto aplikaci v emulátoru hello, ujistěte se, že používáte virtuální zařízení Android (AVD) podporující rozhraní API Google.

> [!IMPORTANT]
> V pořadí tooreceive nabízená oznámení musíte nastavit účet Google na virtuální zařízení se systémem Android. (V emulátoru hello přejděte příliš**nastavení** a klikněte na tlačítko **přidat účet**.) Taky se ujistěte, že hello emulátor je připojený toohello Internetu.
> 
> [!NOTE]
> Návrh oznámení v systému Android verze 5.0 a novější představuje významné odchýlení od předchozích verzí. Další informace naleznete v tématu [Oznámení systému Android](http://go.microsoft.com/fwlink/?LinkId=615880).
> 
> 

1. Z části **Nástroje** klikněte na tlačítko **Otevřít správce emulátoru Android**, vyberte zařízení a pak klikněte na tlačítko **Upravit**.
   
      ![][18]
2. Vyberte **rozhraní API Google** v části **Cíl** a pak klikněte na tlačítko **OK**.
   
      ![][19]
3. Na horním panelu nástrojů hello, klikněte na tlačítko **spustit**a pak vyberte svou aplikaci. To spustí hello emulátor a spustí aplikace hello.
   
   aplikace Hello načte hello *registrationId* z GCM a zaregistruje se pomocí centra oznámení hello.

## <a name="send-notifications-from-your-backend"></a>Odesílání oznámení z backendu
Příjem oznámení ve vaší aplikaci odesláním oznámení hello můžete otestovat [portálu Azure Classic] prostřednictvím hello ladění karty v centru oznámení hello, jak je znázorněno níže úvodní obrazovka.

![][30]

Nabízená oznámení se většinou posílají ve službě backend, jako je služba Mobile Services, nebo v technologii ASP.NET pomocí kompatibilní knihovny. Můžete taky hello REST API přímo toosend oznámení zpráv, pokud není žádná knihovna k dispozici pro váš back-end.

Tady je seznam některých dalších kurzů, které bude pravděpodobně třeba tooreview pro zasílání oznámení:

* ASP.NET: Viz [použití centra oznámení toopush oznámení toousers].
* Azure Notification Hubs Java SDK: najdete v části [jak toouse centra oznámení z Java](notification-hubs-java-push-notification-tutorial.md) pro odesílání oznámení z Java. Tato metoda prošla pro potřeby vývoje pro Android testováním v Eclipse.
* PHP: Viz [jak toouse Notification Hubs z PHP](notification-hubs-php-push-notification-tutorial.md).

V dalším pododdílu hello hello kurzu odesílání oznámení pomocí konzolové aplikace .NET a pomocí mobilních služeb prostřednictvím skriptu uzlu.

#### <a name="optional-send-notifications-by-using-a-net-app"></a>(Volitelné) Odesílání oznámení pomocí aplikace .NET
V této části si ukážeme odesílání oznámení pomocí konzolové aplikace .NET.

1. Vytvořte novou konzolovou aplikaci Visual C#:
   
      ![][20]
2. Ve Visual Studiu klikněte na položku **Nástroje**, klikněte na **Správce balíčků NuGet** a pak klikněte na **Konzola Správce balíčků**.
   
    V sadě Visual Studio se zobrazí hello Konzola správce balíčků.
3. V okně konzoly Správce balíčků hello, nastavte hello **výchozí projekt** tooyour nové konzoly projekt aplikace a potom v okně konzoly hello, spusťte následující příkaz hello:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Tento postup přidá odkaz toohello SDK centra oznámení Azure pomocí hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">balíčku Microsoft.Azure.Notification Hubs NuGet</a>.
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. Otevřete soubor Program.cs hello a přidejte následující hello `using` příkaz:
   
        using Microsoft.Azure.NotificationHubs;
5. Ve vašem `Program` třídy, přidejte následující metodu hello. Aktualizujte zástupný text hello s vaší *DefaultFullSharedAccessSignature* připojovací řetězec a názvu centra z hello [portálu Azure Classic].
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            await hub.SendGcmNativeNotificationAsync("{ \"data\" : {\"message\":\"Hello from Azure!\"}}");
        }
6. Přidejte následující řádky do hello vaše **hlavní** metoda:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Stisknutím klávesy hello F5 klíče toorun hello aplikace. Měli byste obdržet oznámení v aplikaci hello.
   
      ![][21]

#### <a name="optional-send-notifications-by-using-a-mobile-service"></a>(Volitelné) Odesílání oznámení pomocí mobilní služby
1. Sledujte [Začínáme používat Mobile Services]
2. Přihlaste se toohello [portálu Azure Classic]a vyberte mobilní služby.
3. Vyberte hello **Plánovač** karty v horní části hello.
   
      ![][22]
4. Vytvořte novou naplánovanou úlohu, vložte název a vyberte **Na vyžádání**.
   
      ![][23]
5. Když se vytvoří úloha hello, klikněte na název úlohy hello. Pak klikněte na tlačítko hello **skriptu** karty na horním panelu hello.
6. Vložte následující skript dovnitř funkce plánovače hello. Ujistěte se, že tooreplace zástupné symboly hello oznámení centra název a hello připojovacím řetězcem pro *DefaultFullSharedAccessSignature* kterou jste získali dříve. Klikněte na **Uložit**.
   
        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string>');
        notificationHubService.gcm.send(null,'{"data":{"message" : "Hello from Mobile Services!"}}',
          function (error)
          {
            if (!error) {
               console.warn("Notification successful");
            }
            else
            {
              console.warn("Notification failed" + error);
            }
          }
        );
7. Klikněte na tlačítko **spustit jednou** na dolním panelu hello. Měli byste obdržet oznámení informační zprávy.

## <a name="next-steps"></a>Další kroky
V tomto jednoduchém příkladu jste vysílali oznámení tooall vaše zařízení se systémem Android. V pořadí tootarget konkrétní uživatele, najdete v kurzu toohello [použití centra oznámení toopush oznámení toousers]. Pokud chcete toosegment uživatele podle zájmových skupin, můžete si přečíst [toosend použití centra oznámení nejnovější zprávy přes]. Další informace o Notification Hubs toouse v [pokyny centra oznámení] a v hello [Android toofor postupy centra oznámení].

<!-- Anchors. -->
[Enable Google Cloud Messaging]: #register
[Configure your Notification Hub]: #configure-hub
[Connecting your app toohello Notification Hub]: #connecting-app
[Run your app with hello emulator]: #run-app
[Send notifications from your back-end]: #send
[Next steps]:#next-steps

<!-- Images. -->

[11]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-configure-android.png

[13]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app1.png
[15]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app3.png

[18]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app7.png
[19]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app8.png

[20]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-android-toast.png
[22]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler2.png

[30]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Začínáme používat Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[Portál Azure Classic]: https://manage.windowsazure.com/
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Průvodce centry oznámení]: http://msdn.microsoft.com/library/jj927170.aspx
[Android toofor postupy centra oznámení]: http://msdn.microsoft.com/library/dn282661.aspx

[Použití centra oznámení toopush oznámení toousers]: /manage/services/notification-hubs/notify-users-aspnet
[Použít nejnovější zprávy přes toosend centra oznámení]: /manage/services/notification-hubs/breaking-news-dotnet
[GCMClient Component page]: http://components.xamarin.com/view/GCMClient
[Xamarin.NotificationHub GitHub page]: https://github.com/SaschaDittmann/Xamarin.NotificationHub
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Komponenta klienta zasílání zpráv cloudu Google]: http://components.xamarin.com/view/GCMClient/
[Komponenta zasílání zpráv Azure]: http://components.xamarin.com/view/azure-messaging
