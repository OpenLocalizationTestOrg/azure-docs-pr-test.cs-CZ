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
# <a name="add-push-notifications-tooyour-xamarinforms-app"></a><span data-ttu-id="db9ff-103">Přidat nabízená oznámení tooyour Xamarin.Forms aplikaci</span><span class="sxs-lookup"><span data-stu-id="db9ff-103">Add push notifications tooyour Xamarin.Forms app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="db9ff-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="db9ff-104">Overview</span></span>
<span data-ttu-id="db9ff-105">V tomto kurzu přidáte nabízená oznámení tooall hello projekty, které je výsledkem hello [Xamarin.Forms úvodní](app-service-mobile-xamarin-forms-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="db9ff-105">In this tutorial, you add push notifications tooall hello projects that resulted from hello [Xamarin.Forms quick start](app-service-mobile-xamarin-forms-get-started.md).</span></span> <span data-ttu-id="db9ff-106">To znamená, že nabízených oznámení se neodesílají klientů napříč platformami tooall pokaždé, když vložení záznamu.</span><span class="sxs-lookup"><span data-stu-id="db9ff-106">This means that a push notification is sent tooall cross-platform clients every time a record is inserted.</span></span>

<span data-ttu-id="db9ff-107">Pokud nepoužijete hello stáhli úvodní serverový projekt, bude nutné hello nabízených oznámení v balíčku rozšíření.</span><span class="sxs-lookup"><span data-stu-id="db9ff-107">If you do not use hello downloaded quick start server project, you will need hello push notification extension package.</span></span> <span data-ttu-id="db9ff-108">Další informace najdete v tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="db9ff-108">For more information, see [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="db9ff-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="db9ff-109">Prerequisites</span></span>
<span data-ttu-id="db9ff-110">Pro iOS, budete potřebovat [programu pro vývojáře Apple členství](https://developer.apple.com/programs/ios/) a fyzickém zařízení iOS.</span><span class="sxs-lookup"><span data-stu-id="db9ff-110">For iOS, you will need an [Apple Developer Program membership](https://developer.apple.com/programs/ios/) and a physical iOS device.</span></span> <span data-ttu-id="db9ff-111">Hello [simulátoru iOS nabízená oznámení nepodporuje](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).</span><span class="sxs-lookup"><span data-stu-id="db9ff-111">hello [iOS simulator does not support push notifications](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).</span></span>

## <span data-ttu-id="db9ff-112"><a name="configure-hub"></a>Konfigurace centra oznámení</span><span class="sxs-lookup"><span data-stu-id="db9ff-112"><a name="configure-hub"></a>Configure a notification hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="update-hello-server-project-toosend-push-notifications"></a><span data-ttu-id="db9ff-113">Aktualizovat hello serveru projektu toosend nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="db9ff-113">Update hello server project toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-and-run-hello-android-project-optional"></a><span data-ttu-id="db9ff-114">Nakonfigurujte a spusťte hello projekt Android (volitelné)</span><span class="sxs-lookup"><span data-stu-id="db9ff-114">Configure and run hello Android project (optional)</span></span>
<span data-ttu-id="db9ff-115">Dokončení této části tooenable nabízená oznámení pro hello Xamarin.Forms Droid projektu pro Android.</span><span class="sxs-lookup"><span data-stu-id="db9ff-115">Complete this section tooenable push notifications for hello Xamarin.Forms Droid project for Android.</span></span>

### <a name="enable-firebase-cloud-messaging-fcm"></a><span data-ttu-id="db9ff-116">Povolit Firebase Cloud Messaging (FCM)</span><span class="sxs-lookup"><span data-stu-id="db9ff-116">Enable Firebase Cloud Messaging (FCM)</span></span>
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

### <a name="configure-hello-mobile-apps-back-end-toosend-push-requests-by-using-fcm"></a><span data-ttu-id="db9ff-117">Konfigurace hello Mobile Apps back-end toosend nabízené požadavky pomocí FCM</span><span class="sxs-lookup"><span data-stu-id="db9ff-117">Configure hello Mobile Apps back end toosend push requests by using FCM</span></span>
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

### <a name="add-push-notifications-toohello-android-project"></a><span data-ttu-id="db9ff-118">Přidat nabízená oznámení toohello projekt Android</span><span class="sxs-lookup"><span data-stu-id="db9ff-118">Add push notifications toohello Android project</span></span>
<span data-ttu-id="db9ff-119">S hello back-end FCM nakonfigurované můžete přidat součásti a kódy tooregister klienta toohello s FCM.</span><span class="sxs-lookup"><span data-stu-id="db9ff-119">With hello back end configured with FCM, you can add components and codes toohello client tooregister with FCM.</span></span> <span data-ttu-id="db9ff-120">Také můžete zaregistrovat pro nabízená oznámení pomocí Azure Notification Hubs prostřednictvím hello Mobile Apps zpět ukončení a přijímat oznámení.</span><span class="sxs-lookup"><span data-stu-id="db9ff-120">You can also register for push notifications with Azure Notification Hubs through hello Mobile Apps back end, and receive notifications.</span></span>

1. <span data-ttu-id="db9ff-121">V hello **Droid** projektu, klikněte pravým tlačítkem na hello **součásti** složku a klikněte na tlačítko **získat další komponenty...** . Poté vyhledejte hello **klient Google Cloud Messaging** součástí a přidejte ji toohello projektu.</span><span class="sxs-lookup"><span data-stu-id="db9ff-121">In hello **Droid** project, right-click hello **Components** folder, and click **Get More Components...**. Then search for hello **Google Cloud Messaging Client** component and add it toohello project.</span></span> <span data-ttu-id="db9ff-122">Tato součást podporuje nabízená oznámení pro projekt Xamarin Android.</span><span class="sxs-lookup"><span data-stu-id="db9ff-122">This component supports push notifications for a Xamarin Android project.</span></span>
2. <span data-ttu-id="db9ff-123">Otevřete soubor projektu MainActivity.cs hello a přidejte následující příkaz hello horní části souboru hello hello:</span><span class="sxs-lookup"><span data-stu-id="db9ff-123">Open hello MainActivity.cs project file, and add hello following statement at hello top of hello file:</span></span>

        using Gcm.Client;
3. <span data-ttu-id="db9ff-124">Přidejte následující kód toohello hello **OnCreate** volání metody po hello příliš**LoadApplication**:</span><span class="sxs-lookup"><span data-stu-id="db9ff-124">Add hello following code toohello **OnCreate** method after hello call too**LoadApplication**:</span></span>

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
4. <span data-ttu-id="db9ff-125">Přidejte nový **CreateAndShowDialog** Pomocná metoda, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="db9ff-125">Add a new **CreateAndShowDialog** helper method, as follows:</span></span>

        private void CreateAndShowDialog(String message, String title)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);

            builder.SetMessage (message);
            builder.SetTitle (title);
            builder.Create().Show ();
        }
5. <span data-ttu-id="db9ff-126">Přidejte následující kód toohello hello **MainActivity** třídy:</span><span class="sxs-lookup"><span data-stu-id="db9ff-126">Add hello following code toohello **MainActivity** class:</span></span>

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

    <span data-ttu-id="db9ff-127">To zpřístupňuje hello aktuální **MainActivity** instance, proto můžete provést na hello hlavního vlákna uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="db9ff-127">This exposes hello current **MainActivity** instance, so we can execute on hello main UI thread.</span></span>
6. <span data-ttu-id="db9ff-128">Inicializace hello `instance` proměnné na začátku hello hello **OnCreate** metoda následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="db9ff-128">Initialize hello `instance` variable at hello beginning of hello **OnCreate** method, as follows.</span></span>

        // Set hello current instance of MainActivity.
        instance = this;
7. <span data-ttu-id="db9ff-129">Přidat nové toohello souboru třída **Droid** projektu s názvem `GcmService.cs`a ujistěte se, zda text hello následující **pomocí** příkazy nejsou hello horní části souboru hello:</span><span class="sxs-lookup"><span data-stu-id="db9ff-129">Add a new class file toohello **Droid** project named `GcmService.cs`, and make sure hello following **using** statements are present at hello top of hello file:</span></span>

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
8. <span data-ttu-id="db9ff-130">Přidejte následující žádosti o oprávnění hello horní části souboru hello po hello hello **pomocí** příkazy a před hello **obor názvů** deklarace.</span><span class="sxs-lookup"><span data-stu-id="db9ff-130">Add hello following permission requests at hello top of hello file, after hello **using** statements and before hello **namespace** declaration.</span></span>

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
9. <span data-ttu-id="db9ff-131">Přidejte následující obor názvů toohello definice třídy hello.</span><span class="sxs-lookup"><span data-stu-id="db9ff-131">Add hello following class definition toohello namespace.</span></span>

       [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, Categories = new string[] { "@PACKAGE_NAME@" })]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, Categories = new string[] { "@PACKAGE_NAME@" })]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, Categories = new string[] { "@PACKAGE_NAME@" })]
       public class PushHandlerBroadcastReceiver : GcmBroadcastReceiverBase<GcmService>
       {
           public static string[] SENDER_IDS = new string[] { "<PROJECT_NUMBER>" };
       }

   > [!NOTE]
   > <span data-ttu-id="db9ff-132">Nahraďte **< PROJECT_NUMBER >** s vaše číslo projektu, který jste si předtím poznamenali.</span><span class="sxs-lookup"><span data-stu-id="db9ff-132">Replace **<PROJECT_NUMBER>** with your project number you noted earlier.</span></span>    
   >
   >
10. <span data-ttu-id="db9ff-133">Nahraďte hello prázdný **GcmService** se hello následující kód, který používá novou všesměrového vysílání příjemce hello:</span><span class="sxs-lookup"><span data-stu-id="db9ff-133">Replace hello empty **GcmService** class with hello following code, which uses hello new broadcast receiver:</span></span>

         [Service]
         public class GcmService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }

             public GcmService()
                 : base(PushHandlerBroadcastReceiver.SENDER_IDS){}
         }
11. <span data-ttu-id="db9ff-134">Přidejte následující kód toohello hello **GcmService** třídy.</span><span class="sxs-lookup"><span data-stu-id="db9ff-134">Add hello following code toohello **GcmService** class.</span></span> <span data-ttu-id="db9ff-135">Přepíše hello **OnRegistered** obslužné rutiny události a implementuje **zaregistrovat** metoda.</span><span class="sxs-lookup"><span data-stu-id="db9ff-135">This overrides hello **OnRegistered** event handler and implements a **Register** method.</span></span>

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

    <span data-ttu-id="db9ff-136">Všimněte si, že tento kód používá hello `messageParam` parametr v registraci šablony hello.</span><span class="sxs-lookup"><span data-stu-id="db9ff-136">Note that this code uses hello `messageParam` parameter in hello template registration.</span></span>
12. <span data-ttu-id="db9ff-137">Přidejte následující kód, který implementuje hello **OnMessage**:</span><span class="sxs-lookup"><span data-stu-id="db9ff-137">Add hello following code that implements **OnMessage**:</span></span>

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

    <span data-ttu-id="db9ff-138">To zpracovává příchozí oznámení a odešle je toohello oznámení správce toobe zobrazí.</span><span class="sxs-lookup"><span data-stu-id="db9ff-138">This handles incoming notifications and sends them toohello notification manager toobe displayed.</span></span>
13. <span data-ttu-id="db9ff-139">**GcmServiceBase** také vyžaduje, abyste tooimplement hello **OnUnRegistered** a **OnError** metody obslužné rutiny, které můžete provést následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="db9ff-139">**GcmServiceBase** also requires you tooimplement hello **OnUnRegistered** and **OnError** handler methods, which you can do as follows:</span></span>

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "Unregistered RegisterationId : " + registrationId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "GCM Error: " + errorId);
        }

<span data-ttu-id="db9ff-140">Nyní jsou připravené testovací nabízená oznámení v hello aplikaci spuštěnou na zařízení se systémem Android nebo hello emulátor.</span><span class="sxs-lookup"><span data-stu-id="db9ff-140">Now, you are ready test push notifications in hello app running on an Android device or hello emulator.</span></span>

### <a name="test-push-notifications-in-your-android-app"></a><span data-ttu-id="db9ff-141">Nabízená oznámení v aplikacích pro Android</span><span class="sxs-lookup"><span data-stu-id="db9ff-141">Test push notifications in your Android app</span></span>
<span data-ttu-id="db9ff-142">Hello první dva kroky jsou povinné jenom v případě, že testujete na emulátor.</span><span class="sxs-lookup"><span data-stu-id="db9ff-142">hello first two steps are required only when you're testing on an emulator.</span></span>

1. <span data-ttu-id="db9ff-143">Ujistěte se, že nasazujete tooor ladění na virtuální zařízení, které se má nastavit jako cíl hello rozhraní Google API, jak je vidět níže ve Správci zařízení se systémem Android virtuálním hello.</span><span class="sxs-lookup"><span data-stu-id="db9ff-143">Make sure that you are deploying tooor debugging on a virtual device that has Google APIs set as hello target, as shown below in hello Android Virtual Device manager.</span></span>
2. <span data-ttu-id="db9ff-144">Kliknutím na Přidat zařízení Android toohello účet Google **aplikace** > **nastavení** > **přidejte účet**.</span><span class="sxs-lookup"><span data-stu-id="db9ff-144">Add a Google account toohello Android device by clicking **Apps** > **Settings** > **Add account**.</span></span> <span data-ttu-id="db9ff-145">Potom postupujte podle pokynů tooadd hello ze stávajících zařízení toohello účet Google nebo toocreate nový.</span><span class="sxs-lookup"><span data-stu-id="db9ff-145">Then follow hello prompts tooadd an existing Google account toohello device, or toocreate a new one.</span></span>
3. <span data-ttu-id="db9ff-146">V sadě Visual Studio nebo Xamarin Studio, klikněte pravým tlačítkem na hello **Droid** projektu a klikněte na tlačítko **nastavit jako spouštěný projekt**.</span><span class="sxs-lookup"><span data-stu-id="db9ff-146">In Visual Studio or Xamarin Studio, right-click hello **Droid** project and click **Set as startup project**.</span></span>
4. <span data-ttu-id="db9ff-147">Klikněte na tlačítko **spustit** toobuild hello projektu a spusťte aplikaci hello na emulátoru nebo zařízení s Androidem.</span><span class="sxs-lookup"><span data-stu-id="db9ff-147">Click **Run** toobuild hello project and start hello app on your Android device or emulator.</span></span>
5. <span data-ttu-id="db9ff-148">Aplikace hello typ úlohy a pak klikněte na hello plus (**+**) ikona.</span><span class="sxs-lookup"><span data-stu-id="db9ff-148">In hello app, type a task, and then click hello plus (**+**) icon.</span></span>
6. <span data-ttu-id="db9ff-149">Ověřte, že se při přidání položky přijato oznámení.</span><span class="sxs-lookup"><span data-stu-id="db9ff-149">Verify that a notification is received when an item is added.</span></span>

## <a name="configure-and-run-hello-ios-project-optional"></a><span data-ttu-id="db9ff-150">Konfiguraci a spuštění projektu iOS hello (volitelné)</span><span class="sxs-lookup"><span data-stu-id="db9ff-150">Configure and run hello iOS project (optional)</span></span>
<span data-ttu-id="db9ff-151">Tato část se týká spuštění projektu Xamarin iOS pro zařízení s iOS hello.</span><span class="sxs-lookup"><span data-stu-id="db9ff-151">This section is for running hello Xamarin iOS project for iOS devices.</span></span> <span data-ttu-id="db9ff-152">Můžete ji přeskočit, pokud s takovými zařízeními nepracujete.</span><span class="sxs-lookup"><span data-stu-id="db9ff-152">You can skip this section if you are not working with iOS devices.</span></span>

[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

#### <a name="configure-hello-notification-hub-for-apns"></a><span data-ttu-id="db9ff-153">Konfigurace centra oznámení hello pro služby APN</span><span class="sxs-lookup"><span data-stu-id="db9ff-153">Configure hello notification hub for APNS</span></span>
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

<span data-ttu-id="db9ff-154">Dále nakonfigurujete nastavení projektu iOS hello v nástroji Xamarin Studio nebo Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="db9ff-154">Next, you will configure hello iOS project setting in Xamarin Studio or Visual Studio.</span></span>

[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

#### <a name="add-push-notifications-tooyour-ios-app"></a><span data-ttu-id="db9ff-155">Přidání nabízených oznámení tooyour iOS aplikace</span><span class="sxs-lookup"><span data-stu-id="db9ff-155">Add push notifications tooyour iOS app</span></span>
1. <span data-ttu-id="db9ff-156">V hello **iOS** projektu, otevřete AppDelegate.cs a přidejte následující příkaz toohello horní části souboru kódu hello hello.</span><span class="sxs-lookup"><span data-stu-id="db9ff-156">In hello **iOS** project, open AppDelegate.cs and add hello following statement toohello top of hello code file.</span></span>

        using Newtonsoft.Json.Linq;
2. <span data-ttu-id="db9ff-157">V hello **AppDelegate** třídy, přidejte přepsání pro hello **RegisteredForRemoteNotifications** tooregister události pro oznámení:</span><span class="sxs-lookup"><span data-stu-id="db9ff-157">In hello **AppDelegate** class, add an override for hello **RegisteredForRemoteNotifications** event tooregister for notifications:</span></span>

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
3. <span data-ttu-id="db9ff-158">V **AppDelegate**, také přidat hello následující přepsání pro hello **DidReceiveRemoteNotification** obslužné rutiny události:</span><span class="sxs-lookup"><span data-stu-id="db9ff-158">In **AppDelegate**, also add hello following override for hello **DidReceiveRemoteNotification** event handler:</span></span>

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

    <span data-ttu-id="db9ff-159">Tato metoda zpracovává příchozí oznámení, když běží aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="db9ff-159">This method handles incoming notifications while hello app is running.</span></span>
4. <span data-ttu-id="db9ff-160">V hello **AppDelegate** třídy, přidejte následující kód toohello hello **FinishedLaunching** metoda:</span><span class="sxs-lookup"><span data-stu-id="db9ff-160">In hello **AppDelegate** class, add hello following code toohello **FinishedLaunching** method:</span></span>

        // Register for push notifications.
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

    <span data-ttu-id="db9ff-161">To umožňuje podporu pro vzdálené oznámení a registrace nabízených požadavky.</span><span class="sxs-lookup"><span data-stu-id="db9ff-161">This enables support for remote notifications and requests push registration.</span></span>

<span data-ttu-id="db9ff-162">Vaše aplikace je teď aktualizovaný toosupport nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="db9ff-162">Your app is now updated toosupport push notifications.</span></span>

#### <a name="test-push-notifications-in-your-ios-app"></a><span data-ttu-id="db9ff-163">Nabízená oznámení v aplikaci s iOS</span><span class="sxs-lookup"><span data-stu-id="db9ff-163">Test push notifications in your iOS app</span></span>
1. <span data-ttu-id="db9ff-164">Klikněte pravým tlačítkem na projekt pro iOS hello a klikněte na tlačítko **nastavit jako spouštěný projekt**.</span><span class="sxs-lookup"><span data-stu-id="db9ff-164">Right-click hello iOS project, and click **Set as StartUp Project**.</span></span>
2. <span data-ttu-id="db9ff-165">Stiskněte klávesu hello **spustit** tlačítko nebo **F5** v sadě Visual Studio toobuild hello projektu a spusťte aplikaci hello v zařízení se systémem iOS.</span><span class="sxs-lookup"><span data-stu-id="db9ff-165">Press hello **Run** button or **F5** in Visual Studio toobuild hello project and start hello app in an iOS device.</span></span> <span data-ttu-id="db9ff-166">Pak klikněte na tlačítko **OK** tooaccept nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="db9ff-166">Then click **OK** tooaccept push notifications.</span></span>

   > [!NOTE]
   > <span data-ttu-id="db9ff-167">Je nutné explicitně přijmout nabízená oznámení z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="db9ff-167">You must explicitly accept push notifications from your app.</span></span> <span data-ttu-id="db9ff-168">Tento požadavek dochází pouze v hello prvním hello aplikace běží.</span><span class="sxs-lookup"><span data-stu-id="db9ff-168">This request only occurs hello first time that hello app runs.</span></span>
   >
   >
3. <span data-ttu-id="db9ff-169">Aplikace hello typ úlohy a pak klikněte na hello plus (**+**) ikona.</span><span class="sxs-lookup"><span data-stu-id="db9ff-169">In hello app, type a task, and then click hello plus (**+**) icon.</span></span>
4. <span data-ttu-id="db9ff-170">Ověřte, zda přijetí oznámení a pak klikněte na tlačítko **OK** toodismiss hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="db9ff-170">Verify that a notification is received, and then click **OK** toodismiss hello notification.</span></span>

## <a name="configure-and-run-windows-projects-optional"></a><span data-ttu-id="db9ff-171">Nakonfigurujte a spusťte projekty pro Windows (volitelné)</span><span class="sxs-lookup"><span data-stu-id="db9ff-171">Configure and run Windows projects (optional)</span></span>
<span data-ttu-id="db9ff-172">Tato část se týká spuštění hello Xamarin.Forms WinApp a WinPhone81 projekty pro zařízení se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="db9ff-172">This section is for running hello Xamarin.Forms WinApp and WinPhone81 projects for Windows devices.</span></span> <span data-ttu-id="db9ff-173">Tyto kroky také podporují projekty univerzální platformu Windows (UWP).</span><span class="sxs-lookup"><span data-stu-id="db9ff-173">These steps also support Universal Windows Platform (UWP) projects.</span></span> <span data-ttu-id="db9ff-174">Můžete ji přeskočit, pokud s takovými zařízeními nepracujete.</span><span class="sxs-lookup"><span data-stu-id="db9ff-174">You can skip this section if you are not working with Windows devices.</span></span>

#### <a name="register-your-windows-app-for-push-notifications-with-windows-notification-service-wns"></a><span data-ttu-id="db9ff-175">Registrace aplikace systému Windows pro nabízená oznámení pomocí služby oznámení Windows (WNS)</span><span class="sxs-lookup"><span data-stu-id="db9ff-175">Register your Windows app for push notifications with Windows Notification Service (WNS)</span></span>
[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

#### <a name="configure-hello-notification-hub-for-wns"></a><span data-ttu-id="db9ff-176">Konfigurace centra oznámení hello u WNS</span><span class="sxs-lookup"><span data-stu-id="db9ff-176">Configure hello notification hub for WNS</span></span>
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="add-push-notifications-tooyour-windows-app"></a><span data-ttu-id="db9ff-177">Přidat nabízená oznámení tooyour Windows aplikaci</span><span class="sxs-lookup"><span data-stu-id="db9ff-177">Add push notifications tooyour Windows app</span></span>
1. <span data-ttu-id="db9ff-178">V sadě Visual Studio otevřete **App.xaml.cs** v systému Windows projekt a přidejte následující příkazy hello.</span><span class="sxs-lookup"><span data-stu-id="db9ff-178">In Visual Studio, open **App.xaml.cs** in a Windows project, and add hello following statements.</span></span>

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
        using <your_TodoItemManager_portable_class_namespace>;

    <span data-ttu-id="db9ff-179">Nahraďte `<your_TodoItemManager_portable_class_namespace>` s oborem názvů hello přenosné projektu obsahující hello `TodoItemManager` třídy.</span><span class="sxs-lookup"><span data-stu-id="db9ff-179">Replace `<your_TodoItemManager_portable_class_namespace>` with hello namespace of your portable project that contains hello `TodoItemManager` class.</span></span>
2. <span data-ttu-id="db9ff-180">V souboru App.xaml.cs přidejte následující hello **InitNotificationsAsync** metoda:</span><span class="sxs-lookup"><span data-stu-id="db9ff-180">In App.xaml.cs, add hello following **InitNotificationsAsync** method:</span></span>

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

    <span data-ttu-id="db9ff-181">Tato metoda získá kanál hello nabízená oznámení a zaregistruje šablony tooreceive šablony oznámení z centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="db9ff-181">This method gets hello push notification channel, and registers a template tooreceive template notifications from your notification hub.</span></span> <span data-ttu-id="db9ff-182">Šablony oznámení, která podporuje *messageParam* budou doručeny toothis klienta.</span><span class="sxs-lookup"><span data-stu-id="db9ff-182">A template notification that supports *messageParam* will be delivered toothis client.</span></span>
3. <span data-ttu-id="db9ff-183">V souboru App.xaml.cs aktualizace hello **OnLaunched** definici metody obslužné rutiny událostí přidáním hello `async` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="db9ff-183">In App.xaml.cs, update hello **OnLaunched** event handler method definition by adding hello `async` modifier.</span></span> <span data-ttu-id="db9ff-184">Pak přidejte následující řádek kódu na konci hello hello metody hello:</span><span class="sxs-lookup"><span data-stu-id="db9ff-184">Then add hello following line of code at hello end of hello method:</span></span>

        await InitNotificationsAsync();

    <span data-ttu-id="db9ff-185">Zajistíte tak, že se vytvoří nebo aktualizují při každém spuštění aplikace hello hello registrace nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="db9ff-185">This ensures that hello push notification registration is created or refreshed every time hello app is launched.</span></span> <span data-ttu-id="db9ff-186">Je důležité toodo tento tooguarantee, který hello WNS nabízené kanál je vždy aktivní.</span><span class="sxs-lookup"><span data-stu-id="db9ff-186">It's important toodo this tooguarantee that hello WNS push channel is always active.</span></span>  
4. <span data-ttu-id="db9ff-187">V Průzkumníku řešení pro sadu Visual Studio otevřete hello **Package.appxmanifest** souboru a nastavit **informační podporující** příliš**Ano** pod **oznámení**.</span><span class="sxs-lookup"><span data-stu-id="db9ff-187">In Solution Explorer for Visual Studio, open hello **Package.appxmanifest** file, and set **Toast Capable** too**Yes** under **Notifications**.</span></span>
5. <span data-ttu-id="db9ff-188">Sestavení aplikace hello a ověřte, že máte žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="db9ff-188">Build hello app and verify you have no errors.</span></span> <span data-ttu-id="db9ff-189">Klientská aplikace by měl nyní registrovat hello šablony oznámení od hello, že end Mobile Apps zpět.</span><span class="sxs-lookup"><span data-stu-id="db9ff-189">Your client app should now register for hello template notifications from hello Mobile Apps back end.</span></span> <span data-ttu-id="db9ff-190">Tato část opakujte pro každý projekt Windows ve vašem řešení.</span><span class="sxs-lookup"><span data-stu-id="db9ff-190">Repeat this section for every Windows project in your solution.</span></span>

#### <a name="test-push-notifications-in-your-windows-app"></a><span data-ttu-id="db9ff-191">Nabízená oznámení v aplikaci Windows</span><span class="sxs-lookup"><span data-stu-id="db9ff-191">Test push notifications in your Windows app</span></span>
1. <span data-ttu-id="db9ff-192">V sadě Visual Studio, klikněte pravým tlačítkem na projekt Windows a klikněte na tlačítko **nastavit jako spouštěný projekt**.</span><span class="sxs-lookup"><span data-stu-id="db9ff-192">In Visual Studio, right-click a Windows project, and click **Set as startup project**.</span></span>
2. <span data-ttu-id="db9ff-193">Stiskněte klávesu hello **spustit** tlačítko toobuild hello projektu a spusťte aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="db9ff-193">Press hello **Run** button toobuild hello project and start hello app.</span></span>
3. <span data-ttu-id="db9ff-194">V aplikaci hello, zadejte název nové todoitem a pak klikněte na tlačítko hello plus (**+**) ikona tooadd ho.</span><span class="sxs-lookup"><span data-stu-id="db9ff-194">In hello app, type a name for a new todoitem, and then click hello plus (**+**) icon tooadd it.</span></span>
4. <span data-ttu-id="db9ff-195">Ověřte, že se při přidání položky hello přijato oznámení.</span><span class="sxs-lookup"><span data-stu-id="db9ff-195">Verify that a notification is received when hello item is added.</span></span>

## <a name="next-steps"></a><span data-ttu-id="db9ff-196">Další kroky</span><span class="sxs-lookup"><span data-stu-id="db9ff-196">Next steps</span></span>
<span data-ttu-id="db9ff-197">Další informace o nabízených oznámení:</span><span class="sxs-lookup"><span data-stu-id="db9ff-197">You can learn more about push notifications:</span></span>

* [<span data-ttu-id="db9ff-198">Diagnostikovat problémy nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="db9ff-198">Diagnose push notification issues</span></span>](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  <span data-ttu-id="db9ff-199">Existují různé důvody, proč oznámení může získat vyřadit ani nekončí na zařízení.</span><span class="sxs-lookup"><span data-stu-id="db9ff-199">There are various reasons why notifications may get dropped or do not end up on devices.</span></span> <span data-ttu-id="db9ff-200">Toto téma ukazuje, jak tooanalyze a vyřešení hello kořenové způsobit selhání nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="db9ff-200">This topic shows you how tooanalyze and figure out hello root cause of push notification failures.</span></span>

<span data-ttu-id="db9ff-201">Můžete také pokračovat na tooone hello následující kurzy:</span><span class="sxs-lookup"><span data-stu-id="db9ff-201">You can also continue on tooone of hello following tutorials:</span></span>

* [<span data-ttu-id="db9ff-202">Přidat aplikaci tooyour ověřování</span><span class="sxs-lookup"><span data-stu-id="db9ff-202">Add authentication tooyour app </span></span>](app-service-mobile-xamarin-forms-get-started-users.md)  
  <span data-ttu-id="db9ff-203">Zjistěte, jak tooauthenticate uživatele vaší aplikace pomocí zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="db9ff-203">Learn how tooauthenticate users of your app with an identity provider.</span></span>
* [<span data-ttu-id="db9ff-204">Povolení offline synchronizace u aplikace</span><span class="sxs-lookup"><span data-stu-id="db9ff-204">Enable offline sync for your app</span></span>](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  <span data-ttu-id="db9ff-205">Zjistěte, jak tooadd offline podporu pro vaši aplikaci pomocí Mobile Apps back-end.</span><span class="sxs-lookup"><span data-stu-id="db9ff-205">Learn how tooadd offline support for your app by using a Mobile Apps back end.</span></span> <span data-ttu-id="db9ff-206">S offline synchronizací, mohou uživatelé komunikovat s mobilní aplikací&mdash;zobrazení, přidávat a upravovat data&mdash;i v případě, že není žádné síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="db9ff-206">With offline sync, users can interact with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
