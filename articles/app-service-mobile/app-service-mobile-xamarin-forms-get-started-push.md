---
title: "Přidání nabízených oznámení do vaší aplikace na platformě Xamarin.Forms | Microsoft Docs"
description: "Naučte se používat k odesílání více platformami nabízená oznámení do aplikace Xamarin.Forms služby Azure."
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
ms.openlocfilehash: 912367636f1b26b3b07fbd5fe3fe8ed053218fd5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="add-push-notifications-to-your-xamarinforms-app"></a><span data-ttu-id="41260-103">Přidání nabízených oznámení do vaší aplikace na platformě Xamarin.Forms</span><span class="sxs-lookup"><span data-stu-id="41260-103">Add push notifications to your Xamarin.Forms app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="41260-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="41260-104">Overview</span></span>
<span data-ttu-id="41260-105">V tomto kurzu přidání nabízených oznámení na všechny projekty, které je výsledkem [Xamarin.Forms úvodní](app-service-mobile-xamarin-forms-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="41260-105">In this tutorial, you add push notifications to all the projects that resulted from the [Xamarin.Forms quick start](app-service-mobile-xamarin-forms-get-started.md).</span></span> <span data-ttu-id="41260-106">To znamená, že nabízených oznámení se neodesílají do všech klientů napříč platformami pokaždé, když vložení záznamu.</span><span class="sxs-lookup"><span data-stu-id="41260-106">This means that a push notification is sent to all cross-platform clients every time a record is inserted.</span></span>

<span data-ttu-id="41260-107">Pokud použijete serverový projekt stažené rychlý start, budete potřebovat balíček rozšíření nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="41260-107">If you do not use the downloaded quick start server project, you will need the push notification extension package.</span></span> <span data-ttu-id="41260-108">Další informace najdete v tématu [pracovat s .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="41260-108">For more information, see [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="41260-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="41260-109">Prerequisites</span></span>
<span data-ttu-id="41260-110">Pro iOS, budete potřebovat [programu pro vývojáře Apple členství](https://developer.apple.com/programs/ios/) a fyzickém zařízení iOS.</span><span class="sxs-lookup"><span data-stu-id="41260-110">For iOS, you will need an [Apple Developer Program membership](https://developer.apple.com/programs/ios/) and a physical iOS device.</span></span> <span data-ttu-id="41260-111">[Simulátoru iOS nabízená oznámení nepodporuje](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).</span><span class="sxs-lookup"><span data-stu-id="41260-111">The [iOS simulator does not support push notifications](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).</span></span>

## <span data-ttu-id="41260-112"><a name="configure-hub"></a>Konfigurace centra oznámení</span><span class="sxs-lookup"><span data-stu-id="41260-112"><a name="configure-hub"></a>Configure a notification hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="update-the-server-project-to-send-push-notifications"></a><span data-ttu-id="41260-113">Aktualizace serverový projekt k odesílání nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="41260-113">Update the server project to send push notifications</span></span>
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-and-run-the-android-project-optional"></a><span data-ttu-id="41260-114">Konfiguraci a spuštění projektu pro Android (volitelné)</span><span class="sxs-lookup"><span data-stu-id="41260-114">Configure and run the Android project (optional)</span></span>
<span data-ttu-id="41260-115">Dokončení této části ke zprovoznění nabízených oznámení pro Android Xamarin.Forms projekt pro Android.</span><span class="sxs-lookup"><span data-stu-id="41260-115">Complete this section to enable push notifications for the Xamarin.Forms Droid project for Android.</span></span>

### <a name="enable-firebase-cloud-messaging-fcm"></a><span data-ttu-id="41260-116">Povolit Firebase Cloud Messaging (FCM)</span><span class="sxs-lookup"><span data-stu-id="41260-116">Enable Firebase Cloud Messaging (FCM)</span></span>
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

### <a name="configure-the-mobile-apps-back-end-to-send-push-requests-by-using-fcm"></a><span data-ttu-id="41260-117">Konfigurace mobilní aplikace back-endu odesílat žádosti o nabízenou pomocí FCM</span><span class="sxs-lookup"><span data-stu-id="41260-117">Configure the Mobile Apps back end to send push requests by using FCM</span></span>
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

### <a name="add-push-notifications-to-the-android-project"></a><span data-ttu-id="41260-118">Přidání nabízených oznámení do projektu pro Android</span><span class="sxs-lookup"><span data-stu-id="41260-118">Add push notifications to the Android project</span></span>
<span data-ttu-id="41260-119">S back-end FCM nakonfigurované můžete přidat součásti a kódy pro klienta pro registraci se FCM.</span><span class="sxs-lookup"><span data-stu-id="41260-119">With the back end configured with FCM, you can add components and codes to the client to register with FCM.</span></span> <span data-ttu-id="41260-120">Můžete také zaregistrovat pro nabízená oznámení pomocí Azure Notification Hubs prostřednictvím back-end mobilní aplikace a přijímat oznámení.</span><span class="sxs-lookup"><span data-stu-id="41260-120">You can also register for push notifications with Azure Notification Hubs through the Mobile Apps back end, and receive notifications.</span></span>

1. <span data-ttu-id="41260-121">V **Droid** projektu, klikněte pravým tlačítkem myši **součásti** složku a klikněte na tlačítko **získat další komponenty...** .</span><span class="sxs-lookup"><span data-stu-id="41260-121">In the **Droid** project, right-click the **Components** folder, and click **Get More Components...**.</span></span> <span data-ttu-id="41260-122">Poté vyhledejte **klient Google Cloud Messaging** součástí a přidejte ji do projektu.</span><span class="sxs-lookup"><span data-stu-id="41260-122">Then search for the **Google Cloud Messaging Client** component and add it to the project.</span></span> <span data-ttu-id="41260-123">Tato součást podporuje nabízená oznámení pro projekt Xamarin Android.</span><span class="sxs-lookup"><span data-stu-id="41260-123">This component supports push notifications for a Xamarin Android project.</span></span>
2. <span data-ttu-id="41260-124">Otevřete soubor projektu MainActivity.cs a v horní části souboru přidejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="41260-124">Open the MainActivity.cs project file, and add the following statement at the top of the file:</span></span>

        using Gcm.Client;
3. <span data-ttu-id="41260-125">Přidejte následující kód, který **OnCreate** metoda po zavolání **LoadApplication**:</span><span class="sxs-lookup"><span data-stu-id="41260-125">Add the following code to the **OnCreate** method after the call to **LoadApplication**:</span></span>

        try
        {
            // Check to ensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            System.Diagnostics.Debug.WriteLine("Registering...");
            GcmClient.Register(this, PushHandlerBroadcastReceiver.SENDER_IDS);
        }
        catch (Java.Net.MalformedURLException)
        {
            CreateAndShowDialog("There was an error creating the client. Verify the URL.", "Error");
        }
        catch (Exception e)
        {
            CreateAndShowDialog(e.Message, "Error");
        }
4. <span data-ttu-id="41260-126">Přidejte nový **CreateAndShowDialog** Pomocná metoda, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="41260-126">Add a new **CreateAndShowDialog** helper method, as follows:</span></span>

        private void CreateAndShowDialog(String message, String title)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);

            builder.SetMessage (message);
            builder.SetTitle (title);
            builder.Create().Show ();
        }
5. <span data-ttu-id="41260-127">Přidejte následující kód, který **MainActivity** třídy:</span><span class="sxs-lookup"><span data-stu-id="41260-127">Add the following code to the **MainActivity** class:</span></span>

        // Create a new instance field for this activity.
        static MainActivity instance = null;

        // Return the current activity instance.
        public static MainActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }

    <span data-ttu-id="41260-128">To zpřístupňuje aktuální **MainActivity** instance, proto můžete provést na hlavního vlákna uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="41260-128">This exposes the current **MainActivity** instance, so we can execute on the main UI thread.</span></span>
6. <span data-ttu-id="41260-129">Inicializace `instance` proměnné na začátku **OnCreate** metoda následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="41260-129">Initialize the `instance` variable at the beginning of the **OnCreate** method, as follows.</span></span>

        // Set the current instance of MainActivity.
        instance = this;
7. <span data-ttu-id="41260-130">Přidat nový soubor třídy k **Droid** projektu s názvem `GcmService.cs`a zajistěte, aby následující **pomocí** příkazy nejsou v horní části souboru:</span><span class="sxs-lookup"><span data-stu-id="41260-130">Add a new class file to the **Droid** project named `GcmService.cs`, and make sure the following **using** statements are present at the top of the file:</span></span>

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
8. <span data-ttu-id="41260-131">Přidejte následující žádosti oprávnění v horní části souboru po **pomocí** příkazy a před **obor názvů** deklarace.</span><span class="sxs-lookup"><span data-stu-id="41260-131">Add the following permission requests at the top of the file, after the **using** statements and before the **namespace** declaration.</span></span>

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
9. <span data-ttu-id="41260-132">Přidejte následující definice tříd do oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="41260-132">Add the following class definition to the namespace.</span></span>

       [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, Categories = new string[] { "@PACKAGE_NAME@" })]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, Categories = new string[] { "@PACKAGE_NAME@" })]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, Categories = new string[] { "@PACKAGE_NAME@" })]
       public class PushHandlerBroadcastReceiver : GcmBroadcastReceiverBase<GcmService>
       {
           public static string[] SENDER_IDS = new string[] { "<PROJECT_NUMBER>" };
       }

   > [!NOTE]
   > <span data-ttu-id="41260-133">Nahraďte **< PROJECT_NUMBER >** s vaše číslo projektu, který jste si předtím poznamenali.</span><span class="sxs-lookup"><span data-stu-id="41260-133">Replace **<PROJECT_NUMBER>** with your project number you noted earlier.</span></span>    
   >
   >
10. <span data-ttu-id="41260-134">Nahraďte prázdné **GcmService** třída, která využívá nové všesměrového vysílání příjemce následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="41260-134">Replace the empty **GcmService** class with the following code, which uses the new broadcast receiver:</span></span>

         [Service]
         public class GcmService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }

             public GcmService()
                 : base(PushHandlerBroadcastReceiver.SENDER_IDS){}
         }
11. <span data-ttu-id="41260-135">Přidejte následující kód, který **GcmService** třídy.</span><span class="sxs-lookup"><span data-stu-id="41260-135">Add the following code to the **GcmService** class.</span></span> <span data-ttu-id="41260-136">Přepíše **OnRegistered** obslužné rutiny události a implementuje **zaregistrovat** metoda.</span><span class="sxs-lookup"><span data-stu-id="41260-136">This overrides the **OnRegistered** event handler and implements a **Register** method.</span></span>

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

    <span data-ttu-id="41260-137">Všimněte si, že tento kód používá `messageParam` parametr v registraci šablony.</span><span class="sxs-lookup"><span data-stu-id="41260-137">Note that this code uses the `messageParam` parameter in the template registration.</span></span>
12. <span data-ttu-id="41260-138">Přidejte následující kód, který implementuje **OnMessage**:</span><span class="sxs-lookup"><span data-stu-id="41260-138">Add the following code that implements **OnMessage**:</span></span>

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info("PushHandlerBroadcastReceiver", "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            //Store the message
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

            //Create an intent to show ui
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Use Notification Builder
            NotificationCompat.Builder builder = new NotificationCompat.Builder(this);

            //Create the notification
            //we use the pending intent, passing our ui intent over which will get called
            //when the notification is tapped.
            var notification = builder.SetContentIntent(PendingIntent.GetActivity(this, 0, uiIntent, 0))
                    .SetSmallIcon(Android.Resource.Drawable.SymActionEmail)
                    .SetTicker(title)
                    .SetContentTitle(title)
                    .SetContentText(desc)

                    //Set the notification sound
                    .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))

                    //Auto cancel will remove the notification once the user touches it
                    .SetAutoCancel(true).Build();

            //Show the notification
            notificationManager.Notify(1, notification);
        }

    <span data-ttu-id="41260-139">To zpracovává příchozí oznámení a odesílá je do Správce oznámení, který se má zobrazit.</span><span class="sxs-lookup"><span data-stu-id="41260-139">This handles incoming notifications and sends them to the notification manager to be displayed.</span></span>
13. <span data-ttu-id="41260-140">**GcmServiceBase** také vyžaduje, abyste implementovat **OnUnRegistered** a **OnError** metody obslužné rutiny, které můžete provést následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="41260-140">**GcmServiceBase** also requires you to implement the **OnUnRegistered** and **OnError** handler methods, which you can do as follows:</span></span>

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "Unregistered RegisterationId : " + registrationId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "GCM Error: " + errorId);
        }

<span data-ttu-id="41260-141">Teď můžete je připraven nabízená oznámení v aplikaci spuštěnou na zařízení se systémem Android nebo emulátor.</span><span class="sxs-lookup"><span data-stu-id="41260-141">Now, you are ready test push notifications in the app running on an Android device or the emulator.</span></span>

### <a name="test-push-notifications-in-your-android-app"></a><span data-ttu-id="41260-142">Nabízená oznámení v aplikacích pro Android</span><span class="sxs-lookup"><span data-stu-id="41260-142">Test push notifications in your Android app</span></span>
<span data-ttu-id="41260-143">První dva kroky jsou povinné, jenom v případě, že testujete na emulátor.</span><span class="sxs-lookup"><span data-stu-id="41260-143">The first two steps are required only when you're testing on an emulator.</span></span>

1. <span data-ttu-id="41260-144">Ujistěte se, že nasazení nebo ladění na virtuální zařízení, které má rozhraní Google API nastavenou jako cíl, jak je znázorněno níže ve Správci virtuálního zařízení se systémem Android.</span><span class="sxs-lookup"><span data-stu-id="41260-144">Make sure that you are deploying to or debugging on a virtual device that has Google APIs set as the target, as shown below in the Android Virtual Device manager.</span></span>
2. <span data-ttu-id="41260-145">Kliknutím na Přidat účet Google do zařízení s Androidem **aplikace** > **nastavení** > **přidejte účet**.</span><span class="sxs-lookup"><span data-stu-id="41260-145">Add a Google account to the Android device by clicking **Apps** > **Settings** > **Add account**.</span></span> <span data-ttu-id="41260-146">Potom postupujte podle pokynů, které chcete přidat existující účet Google do zařízení, nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="41260-146">Then follow the prompts to add an existing Google account to the device, or to create a new one.</span></span>
3. <span data-ttu-id="41260-147">V sadě Visual Studio nebo Xamarin Studio, klikněte pravým tlačítkem **Droid** projektu a klikněte na tlačítko **nastavit jako spouštěný projekt**.</span><span class="sxs-lookup"><span data-stu-id="41260-147">In Visual Studio or Xamarin Studio, right-click the **Droid** project and click **Set as startup project**.</span></span>
4. <span data-ttu-id="41260-148">Klikněte na tlačítko **spustit** pro sestavení projektu a spusťte aplikaci v emulátoru nebo zařízení s Androidem.</span><span class="sxs-lookup"><span data-stu-id="41260-148">Click **Run** to build the project and start the app on your Android device or emulator.</span></span>
5. <span data-ttu-id="41260-149">V aplikaci zadejte úlohu a potom klikněte na tlačítko plus (**+**) ikona.</span><span class="sxs-lookup"><span data-stu-id="41260-149">In the app, type a task, and then click the plus (**+**) icon.</span></span>
6. <span data-ttu-id="41260-150">Ověřte, že se při přidání položky přijato oznámení.</span><span class="sxs-lookup"><span data-stu-id="41260-150">Verify that a notification is received when an item is added.</span></span>

## <a name="configure-and-run-the-ios-project-optional"></a><span data-ttu-id="41260-151">Konfiguraci a spuštění projektu pro iOS (volitelné)</span><span class="sxs-lookup"><span data-stu-id="41260-151">Configure and run the iOS project (optional)</span></span>
<span data-ttu-id="41260-152">Tato část se týká spuštění projektu Xamarin iOS pro zařízení s iOS.</span><span class="sxs-lookup"><span data-stu-id="41260-152">This section is for running the Xamarin iOS project for iOS devices.</span></span> <span data-ttu-id="41260-153">Můžete ji přeskočit, pokud s takovými zařízeními nepracujete.</span><span class="sxs-lookup"><span data-stu-id="41260-153">You can skip this section if you are not working with iOS devices.</span></span>

[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

#### <a name="configure-the-notification-hub-for-apns"></a><span data-ttu-id="41260-154">Konfigurace centra oznámení pro služby APN</span><span class="sxs-lookup"><span data-stu-id="41260-154">Configure the notification hub for APNS</span></span>
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

<span data-ttu-id="41260-155">Dále nakonfigurujete nastavení projektu iOS v nástroji Xamarin Studio nebo Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="41260-155">Next, you will configure the iOS project setting in Xamarin Studio or Visual Studio.</span></span>

[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

#### <a name="add-push-notifications-to-your-ios-app"></a><span data-ttu-id="41260-156">Přidání nabízených oznámení do vaší aplikace pro iOS</span><span class="sxs-lookup"><span data-stu-id="41260-156">Add push notifications to your iOS app</span></span>
1. <span data-ttu-id="41260-157">V **iOS** projektu, otevřete AppDelegate.cs a přidejte následující příkaz na začátek souboru kódu.</span><span class="sxs-lookup"><span data-stu-id="41260-157">In the **iOS** project, open AppDelegate.cs and add the following statement to the top of the code file.</span></span>

        using Newtonsoft.Json.Linq;
2. <span data-ttu-id="41260-158">V **AppDelegate** třídy, přidejte přepsání pro **RegisteredForRemoteNotifications** události k registraci pro oznámení:</span><span class="sxs-lookup"><span data-stu-id="41260-158">In the **AppDelegate** class, add an override for the **RegisteredForRemoteNotifications** event to register for notifications:</span></span>

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
3. <span data-ttu-id="41260-159">V **AppDelegate**, také přidat následující přepsání pro **DidReceiveRemoteNotification** obslužné rutiny události:</span><span class="sxs-lookup"><span data-stu-id="41260-159">In **AppDelegate**, also add the following override for the **DidReceiveRemoteNotification** event handler:</span></span>

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

    <span data-ttu-id="41260-160">Tato metoda zpracovává příchozí oznámení, když aplikace běží.</span><span class="sxs-lookup"><span data-stu-id="41260-160">This method handles incoming notifications while the app is running.</span></span>
4. <span data-ttu-id="41260-161">V **AppDelegate** třídy, přidejte následující kód, který **FinishedLaunching** metoda:</span><span class="sxs-lookup"><span data-stu-id="41260-161">In the **AppDelegate** class, add the following code to the **FinishedLaunching** method:</span></span>

        // Register for push notifications.
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

    <span data-ttu-id="41260-162">To umožňuje podporu pro vzdálené oznámení a registrace nabízených požadavky.</span><span class="sxs-lookup"><span data-stu-id="41260-162">This enables support for remote notifications and requests push registration.</span></span>

<span data-ttu-id="41260-163">Aplikace je nyní aktualizovat o podporu nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="41260-163">Your app is now updated to support push notifications.</span></span>

#### <a name="test-push-notifications-in-your-ios-app"></a><span data-ttu-id="41260-164">Nabízená oznámení v aplikaci s iOS</span><span class="sxs-lookup"><span data-stu-id="41260-164">Test push notifications in your iOS app</span></span>
1. <span data-ttu-id="41260-165">Klikněte pravým tlačítkem na projekt pro iOS a klikněte na tlačítko **nastavit jako spouštěný projekt**.</span><span class="sxs-lookup"><span data-stu-id="41260-165">Right-click the iOS project, and click **Set as StartUp Project**.</span></span>
2. <span data-ttu-id="41260-166">Stiskněte **spustit** tlačítko nebo **F5** v sadě Visual Studio pro sestavení projektu a spusťte aplikaci v zařízení se systémem iOS.</span><span class="sxs-lookup"><span data-stu-id="41260-166">Press the **Run** button or **F5** in Visual Studio to build the project and start the app in an iOS device.</span></span> <span data-ttu-id="41260-167">Pak klikněte na tlačítko **OK** přijímat nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="41260-167">Then click **OK** to accept push notifications.</span></span>

   > [!NOTE]
   > <span data-ttu-id="41260-168">Je nutné explicitně přijmout nabízená oznámení z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="41260-168">You must explicitly accept push notifications from your app.</span></span> <span data-ttu-id="41260-169">Tento požadavek dochází pouze při prvním spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="41260-169">This request only occurs the first time that the app runs.</span></span>
   >
   >
3. <span data-ttu-id="41260-170">V aplikaci zadejte úlohu a potom klikněte na tlačítko plus (**+**) ikona.</span><span class="sxs-lookup"><span data-stu-id="41260-170">In the app, type a task, and then click the plus (**+**) icon.</span></span>
4. <span data-ttu-id="41260-171">Ověřte, zda přijetí oznámení a pak klikněte na tlačítko **OK** k zavření oznámení.</span><span class="sxs-lookup"><span data-stu-id="41260-171">Verify that a notification is received, and then click **OK** to dismiss the notification.</span></span>

## <a name="configure-and-run-windows-projects-optional"></a><span data-ttu-id="41260-172">Nakonfigurujte a spusťte projekty pro Windows (volitelné)</span><span class="sxs-lookup"><span data-stu-id="41260-172">Configure and run Windows projects (optional)</span></span>
<span data-ttu-id="41260-173">Tato část se týká spuštění Xamarin.Forms WinApp a WinPhone81 projekty pro zařízení se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="41260-173">This section is for running the Xamarin.Forms WinApp and WinPhone81 projects for Windows devices.</span></span> <span data-ttu-id="41260-174">Tyto kroky také podporují projekty univerzální platformu Windows (UWP).</span><span class="sxs-lookup"><span data-stu-id="41260-174">These steps also support Universal Windows Platform (UWP) projects.</span></span> <span data-ttu-id="41260-175">Můžete ji přeskočit, pokud s takovými zařízeními nepracujete.</span><span class="sxs-lookup"><span data-stu-id="41260-175">You can skip this section if you are not working with Windows devices.</span></span>

#### <a name="register-your-windows-app-for-push-notifications-with-windows-notification-service-wns"></a><span data-ttu-id="41260-176">Registrace aplikace systému Windows pro nabízená oznámení pomocí služby oznámení Windows (WNS)</span><span class="sxs-lookup"><span data-stu-id="41260-176">Register your Windows app for push notifications with Windows Notification Service (WNS)</span></span>
[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

#### <a name="configure-the-notification-hub-for-wns"></a><span data-ttu-id="41260-177">Konfigurace centra oznámení pro WNS</span><span class="sxs-lookup"><span data-stu-id="41260-177">Configure the notification hub for WNS</span></span>
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="add-push-notifications-to-your-windows-app"></a><span data-ttu-id="41260-178">Přidání nabízených oznámení do aplikace pro Windows</span><span class="sxs-lookup"><span data-stu-id="41260-178">Add push notifications to your Windows app</span></span>
1. <span data-ttu-id="41260-179">V sadě Visual Studio otevřete **App.xaml.cs** v systému Windows projekt a přidejte následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="41260-179">In Visual Studio, open **App.xaml.cs** in a Windows project, and add the following statements.</span></span>

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
        using <your_TodoItemManager_portable_class_namespace>;

    <span data-ttu-id="41260-180">Nahraďte `<your_TodoItemManager_portable_class_namespace>` s oborem názvů vašeho přenosného projektu, který obsahuje `TodoItemManager` třídy.</span><span class="sxs-lookup"><span data-stu-id="41260-180">Replace `<your_TodoItemManager_portable_class_namespace>` with the namespace of your portable project that contains the `TodoItemManager` class.</span></span>
2. <span data-ttu-id="41260-181">V souboru App.xaml.cs přidejte následující **InitNotificationsAsync** metoda:</span><span class="sxs-lookup"><span data-stu-id="41260-181">In App.xaml.cs, add the following **InitNotificationsAsync** method:</span></span>

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

    <span data-ttu-id="41260-182">Tato metoda získá kanál nabízená oznámení a zaregistruje šablonu do šablony oznámení dostávat, vaše Centrum oznámení.</span><span class="sxs-lookup"><span data-stu-id="41260-182">This method gets the push notification channel, and registers a template to receive template notifications from your notification hub.</span></span> <span data-ttu-id="41260-183">Šablony oznámení, která podporuje *messageParam* budou doručeny do tohoto klienta.</span><span class="sxs-lookup"><span data-stu-id="41260-183">A template notification that supports *messageParam* will be delivered to this client.</span></span>
3. <span data-ttu-id="41260-184">V souboru App.xaml.cs, aktualizovat **OnLaunched** definici metody obslužné rutiny událostí přidáním `async` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="41260-184">In App.xaml.cs, update the **OnLaunched** event handler method definition by adding the `async` modifier.</span></span> <span data-ttu-id="41260-185">Pak přidejte následující řádek kódu na konci metody:</span><span class="sxs-lookup"><span data-stu-id="41260-185">Then add the following line of code at the end of the method:</span></span>

        await InitNotificationsAsync();

    <span data-ttu-id="41260-186">Tím se zajistí, že registrace nabízených oznámení je vytvořit nebo aktualizovat pokaždé, když je aplikace spuštěná.</span><span class="sxs-lookup"><span data-stu-id="41260-186">This ensures that the push notification registration is created or refreshed every time the app is launched.</span></span> <span data-ttu-id="41260-187">Je důležité k tomu zaručit, že kanál nabízené WNS je vždy aktivní.</span><span class="sxs-lookup"><span data-stu-id="41260-187">It's important to do this to guarantee that the WNS push channel is always active.</span></span>  
4. <span data-ttu-id="41260-188">V Průzkumníku řešení pro sadu Visual Studio, otevřete **Package.appxmanifest** souboru a nastavit **informační podporující** k **Ano** pod **oznámení**.</span><span class="sxs-lookup"><span data-stu-id="41260-188">In Solution Explorer for Visual Studio, open the **Package.appxmanifest** file, and set **Toast Capable** to **Yes** under **Notifications**.</span></span>
5. <span data-ttu-id="41260-189">Sestavte aplikaci a zkontrolujte, že jste žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="41260-189">Build the app and verify you have no errors.</span></span> <span data-ttu-id="41260-190">Klientská aplikace by teď zaregistrovat pro šablony oznámení z back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="41260-190">Your client app should now register for the template notifications from the Mobile Apps back end.</span></span> <span data-ttu-id="41260-191">Tato část opakujte pro každý projekt Windows ve vašem řešení.</span><span class="sxs-lookup"><span data-stu-id="41260-191">Repeat this section for every Windows project in your solution.</span></span>

#### <a name="test-push-notifications-in-your-windows-app"></a><span data-ttu-id="41260-192">Nabízená oznámení v aplikaci Windows</span><span class="sxs-lookup"><span data-stu-id="41260-192">Test push notifications in your Windows app</span></span>
1. <span data-ttu-id="41260-193">V sadě Visual Studio, klikněte pravým tlačítkem na projekt Windows a klikněte na tlačítko **nastavit jako spouštěný projekt**.</span><span class="sxs-lookup"><span data-stu-id="41260-193">In Visual Studio, right-click a Windows project, and click **Set as startup project**.</span></span>
2. <span data-ttu-id="41260-194">Stiskněte tlačítko **Spustit** a sestavte projekt a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="41260-194">Press the **Run** button to build the project and start the app.</span></span>
3. <span data-ttu-id="41260-195">V aplikaci, zadejte název nové todoitem a pak klikněte na tlačítko plus (**+**) ikona Přidat.</span><span class="sxs-lookup"><span data-stu-id="41260-195">In the app, type a name for a new todoitem, and then click the plus (**+**) icon to add it.</span></span>
4. <span data-ttu-id="41260-196">Ověřte, že se při přidání položky přijato oznámení.</span><span class="sxs-lookup"><span data-stu-id="41260-196">Verify that a notification is received when the item is added.</span></span>

## <a name="next-steps"></a><span data-ttu-id="41260-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="41260-197">Next steps</span></span>
<span data-ttu-id="41260-198">Další informace o nabízených oznámení:</span><span class="sxs-lookup"><span data-stu-id="41260-198">You can learn more about push notifications:</span></span>

* [<span data-ttu-id="41260-199">Diagnostikovat problémy nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="41260-199">Diagnose push notification issues</span></span>](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  <span data-ttu-id="41260-200">Existují různé důvody, proč oznámení může získat vyřadit ani nekončí na zařízení.</span><span class="sxs-lookup"><span data-stu-id="41260-200">There are various reasons why notifications may get dropped or do not end up on devices.</span></span> <span data-ttu-id="41260-201">Toto téma ukazuje, jak analyzovat a zjistěte příčinu selhání nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="41260-201">This topic shows you how to analyze and figure out the root cause of push notification failures.</span></span>

<span data-ttu-id="41260-202">Můžete také pokračovat na jednu z následujících kurzů:</span><span class="sxs-lookup"><span data-stu-id="41260-202">You can also continue on to one of the following tutorials:</span></span>

* [<span data-ttu-id="41260-203">Přidání ověřování do aplikace</span><span class="sxs-lookup"><span data-stu-id="41260-203">Add authentication to your app </span></span>](app-service-mobile-xamarin-forms-get-started-users.md)  
  <span data-ttu-id="41260-204">Zjistěte, jak ověřovat uživatele vaší aplikace pomocí zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="41260-204">Learn how to authenticate users of your app with an identity provider.</span></span>
* [<span data-ttu-id="41260-205">Povolení offline synchronizace u aplikace</span><span class="sxs-lookup"><span data-stu-id="41260-205">Enable offline sync for your app</span></span>](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  <span data-ttu-id="41260-206">Zjistěte, jak pomocí back-endu Mobile Apps přidat do aplikace podporu offline režimu.</span><span class="sxs-lookup"><span data-stu-id="41260-206">Learn how to add offline support for your app by using a Mobile Apps back end.</span></span> <span data-ttu-id="41260-207">S offline synchronizací, mohou uživatelé komunikovat s mobilní aplikací&mdash;zobrazení, přidávat a upravovat data&mdash;i v případě, že není žádné síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="41260-207">With offline sync, users can interact with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
