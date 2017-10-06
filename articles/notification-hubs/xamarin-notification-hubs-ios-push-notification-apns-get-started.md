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
# <a name="ios-push-notifications-with-notification-hubs-for-xamarin-apps"></a><span data-ttu-id="ae069-104">Nabízená oznámení iOS s centry oznámení pro aplikace Xamarin</span><span class="sxs-lookup"><span data-stu-id="ae069-104">iOS Push Notifications with Notification Hubs for Xamarin apps</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="ae069-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="ae069-105">Overview</span></span>
> [!IMPORTANT]
> <span data-ttu-id="ae069-106">toocomplete tento kurz, musíte mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="ae069-106">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="ae069-107">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="ae069-107">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="ae069-108">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="ae069-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).</span></span>
> 
> 

<span data-ttu-id="ae069-109">Tento kurz ukazuje, jak Azure Notification Hubs toosend toouse nabízená oznámení tooan iOS aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae069-109">This tutorial shows you how toouse Azure Notification Hubs toosend push notifications tooan iOS application.</span></span>
<span data-ttu-id="ae069-110">Vytvoříte prázdnou aplikaci Xamarin.iOS, která přijímá nabízená oznámení pomocí hello [Apple Push Notification Service (APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html).</span><span class="sxs-lookup"><span data-stu-id="ae069-110">You'll create a blank Xamarin.iOS app that receives push notifications by using hello [Apple Push Notification Service (APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html).</span></span> <span data-ttu-id="ae069-111">Jakmile budete hotovi, budete moct toouse vaše toobroadcast centra oznámení nabízená oznámení tooall hello zařízení používající vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ae069-111">When you're finished, you'll be able toouse your notification hub toobroadcast push notifications tooall hello devices running your app.</span></span> <span data-ttu-id="ae069-112">Hello Dokončený kód je k dispozici v hello [aplikace NotificationHubs] [ GitHub] ukázka.</span><span class="sxs-lookup"><span data-stu-id="ae069-112">hello finished code is available in hello [NotificationHubs app][GitHub] sample.</span></span>

<span data-ttu-id="ae069-113">Tento kurz představuje hello jednoduchého zprávy všesměrového vysílání scénář s Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="ae069-113">This tutorial demonstrates hello simple push message broadcast scenario with Notification Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ae069-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ae069-114">Prerequisites</span></span>
<span data-ttu-id="ae069-115">Tento kurz vyžaduje hello následující:</span><span class="sxs-lookup"><span data-stu-id="ae069-115">This tutorial requires hello following:</span></span>

* <span data-ttu-id="ae069-116">[Xcode 6.0][Install Xcode]</span><span class="sxs-lookup"><span data-stu-id="ae069-116">[Xcode 6.0][Install Xcode]</span></span>
* <span data-ttu-id="ae069-117">Zařízení kompatibilní s iOS 7.0 (nebo novější verze)</span><span class="sxs-lookup"><span data-stu-id="ae069-117">An iOS 7.0 (or later version) compatible device</span></span>
* <span data-ttu-id="ae069-118">Členství v programu pro vývojáře iOS.</span><span class="sxs-lookup"><span data-stu-id="ae069-118">iOS Developer Program membership</span></span>
* <span data-ttu-id="ae069-119">[Xamarin Studio]</span><span class="sxs-lookup"><span data-stu-id="ae069-119">[Xamarin Studio]</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="ae069-120">Z důvodu požadavky na konfiguraci pro nabízená oznámení iOS musíte nasadit a otestovat vzorovou aplikaci hello na fyzickém zařízení iOS (iPhone nebo iPad) místo v simulátoru hello.</span><span class="sxs-lookup"><span data-stu-id="ae069-120">Because of configuration requirements for iOS push notifications, you must deploy and test hello sample application on a physical iOS device (iPhone or iPad) instead of in hello simulator.</span></span>
  > 
  > 

<span data-ttu-id="ae069-121">Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy Notification Hubs pro Xamarin iOS Apps.</span><span class="sxs-lookup"><span data-stu-id="ae069-121">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for Xamarin iOS apps.</span></span>

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub"></a><span data-ttu-id="ae069-122">Konfigurace centra oznámení</span><span class="sxs-lookup"><span data-stu-id="ae069-122">Configure your notification hub</span></span>
<span data-ttu-id="ae069-123">Tato část vás provede vytvořením nového centra oznámení a konfiguraci ověřování pomocí služby APNS pomocí hello **.p12** nabízený certifikát, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="ae069-123">This section walks you through creating a new notification hub and configuring authentication with APNS using hello **.p12** push certificate that you created.</span></span> <span data-ttu-id="ae069-124">Pokud chcete toouse Centrum oznámení, které jste už vytvořili, můžete přeskočit toostep 5.</span><span class="sxs-lookup"><span data-stu-id="ae069-124">If you want toouse a notification hub that you have already created, you can skip toostep 5.</span></span>

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="7">

<li>

<p><span data-ttu-id="ae069-125">Jak chceme připojení služby APN hello tooconfigure, v hello portálu Azure, otevřete své nastavení centra oznámení, a klikněte na <b>služby oznámení</b>a potom klikněte na hello <b>Apple (APNS)</b> položku v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="ae069-125">As we want tooconfigure hello APNS connection, in hello Azure Portal, open your Notification Hub settings, ande click on <b>Notification Services</b>, and then click hello <b>Apple (APNS)</b> item in hello list.</span></span> <span data-ttu-id="ae069-126">Po dokončení klikněte na <b>nahrát certifikát</b> a vyberte hello <b>.p12</b> certifikát, který jste exportovali dříve, a také hello heslo pro certifikát hello.</span><span class="sxs-lookup"><span data-stu-id="ae069-126">Once done, click on <b>Upload Certificate</b> and select hello <b>.p12</b> certificate that you exported earlier, as well as hello password for hello certificate.</span></span></p>

<p><span data-ttu-id="ae069-127">Ujistěte se, že tooselect <b>izolovaného prostoru</b> režimu vzhledem k tomu, že budete odesílat nabízené zprávy ve vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="ae069-127">Make sure tooselect <b>Sandbox</b> mode since you will be sending push messages in a development environment.</span></span> <span data-ttu-id="ae069-128">Používat pouze hello <b>produkční</b> nastavení, pokud chcete, aby toousers toosend nabízená oznámení, který již zakoupili aplikaci z úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="ae069-128">Only use hello <b>Production</b> setting if you want toosend push notifications toousers who already purchased your app from hello store.</span></span></p>
</li>
</ol>
<span data-ttu-id="ae069-129">&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-apns.png)</span><span class="sxs-lookup"><span data-stu-id="ae069-129">&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-apns.png)</span></span>

&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-sandbox.png)

<span data-ttu-id="ae069-130">Vaše centrum oznámení je nyní nakonfigurované toowork pomocí služby APNS a máte hello připojovací řetězce tooregister vaší aplikace a odesílání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="ae069-130">Your notification hub is now configured toowork with APNS, and you have hello connection strings tooregister your app and send push notifications.</span></span>

## <a name="connect-your-app-toohello-notification-hub"></a><span data-ttu-id="ae069-131">Připojit vaše Centrum oznámení toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="ae069-131">Connect your app toohello notification hub</span></span>
#### <a name="create-a-new-project"></a><span data-ttu-id="ae069-132">Vytvoření nového projektu</span><span class="sxs-lookup"><span data-stu-id="ae069-132">Create a new project</span></span>
1. <span data-ttu-id="ae069-133">V Xamarin studiu vytvořte nový projekt iOS a vyberte hello **unifikované API** > **jediné zobrazení aplikace** šablony.</span><span class="sxs-lookup"><span data-stu-id="ae069-133">In Xamarin Studio, create a new iOS project and select hello **Unified API** > **Single View Application** template.</span></span>
   
     ![Xamarin Studio – výběr typu aplikace][31]
2. <span data-ttu-id="ae069-135">Přidáte součást zasílání zpráv Azure toohello odkaz.</span><span class="sxs-lookup"><span data-stu-id="ae069-135">Add a reference toohello Azure Messaging component.</span></span> <span data-ttu-id="ae069-136">V zobrazení řešení hello, klikněte pravým tlačítkem na hello **součásti** složky pro váš projekt a zvolte **získat více komponent**.</span><span class="sxs-lookup"><span data-stu-id="ae069-136">In hello Solution view, right-click hello **Components** folder for your project and choose **Get More Components**.</span></span> <span data-ttu-id="ae069-137">Vyhledejte hello **zasílání zpráv Azure** součástí a přidejte hello součást tooyour projektu.</span><span class="sxs-lookup"><span data-stu-id="ae069-137">Search for hello **Azure Messaging** component and add hello component tooyour project.</span></span>
3. <span data-ttu-id="ae069-138">V **AppDelegate.cs**, přidejte hello následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="ae069-138">In **AppDelegate.cs**, add hello following using statement:</span></span>
   
        using WindowsAzure.Messaging;
4. <span data-ttu-id="ae069-139">Deklarujte instanci **SBNotificationHub**:</span><span class="sxs-lookup"><span data-stu-id="ae069-139">Declare an instance of **SBNotificationHub**:</span></span>
   
        private SBNotificationHub Hub { get; set; }
5. <span data-ttu-id="ae069-140">Vytvoření **Constants.cs** se hello následující proměnné:</span><span class="sxs-lookup"><span data-stu-id="ae069-140">Create a **Constants.cs** class with hello following variables:</span></span>
   
        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";
6. <span data-ttu-id="ae069-141">V **AppDelegate.cs**, aktualizovat **FinishedLaunching()** toomatch hello následující:</span><span class="sxs-lookup"><span data-stu-id="ae069-141">In **AppDelegate.cs**, update **FinishedLaunching()** toomatch hello following:</span></span>
   
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
7. <span data-ttu-id="ae069-142">Přepsání hello **RegisteredForRemoteNotifications()** metoda v **AppDelegate.cs**:</span><span class="sxs-lookup"><span data-stu-id="ae069-142">Override hello **RegisteredForRemoteNotifications()** method in **AppDelegate.cs**:</span></span>
   
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
8. <span data-ttu-id="ae069-143">Přepsání hello **Receivedremotenotifications()** metoda v **AppDelegate.cs**:</span><span class="sxs-lookup"><span data-stu-id="ae069-143">Override hello **ReceivedRemoteNotification()** method in **AppDelegate.cs**:</span></span>
   
        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }
9. <span data-ttu-id="ae069-144">Vytvořte následující hello **ProcessNotification()** metoda v **AppDelegate.cs**:</span><span class="sxs-lookup"><span data-stu-id="ae069-144">Create hello following **ProcessNotification()** method in **AppDelegate.cs**:</span></span>
   
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
   > <span data-ttu-id="ae069-145">Můžete zvolit toooverride **FailedToRegisterForRemoteNotifications()** toohandle situací jako chybějící síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="ae069-145">You can choose toooverride **FailedToRegisterForRemoteNotifications()** toohandle situations such as no network connection.</span></span> <span data-ttu-id="ae069-146">To je obzvláště důležité, kde hello uživatel mohl spustit aplikaci v režimu offline (například letadlo) a chcete zpráv scénáře konkrétní tooyour aplikace toohandle oznámení.</span><span class="sxs-lookup"><span data-stu-id="ae069-146">This is especially important where hello user might start your application in offline mode (e.g. Airplane) and you want toohandle push messaging scenarios specific tooyour app.</span></span>
   > 
   > 
10. <span data-ttu-id="ae069-147">Spuštění aplikace hello na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="ae069-147">Run hello app on your device.</span></span>

## <a name="sending-push-notifications"></a><span data-ttu-id="ae069-148">Odeslání nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="ae069-148">Sending Push Notifications</span></span>
<span data-ttu-id="ae069-149">Můžete otestovat přijímání nabízených oznámení ve vaší aplikaci odesláním oznámení hello [portálu Azure] prostřednictvím hello **testovací odeslání** funkci hello **Poradce při potížích s** Sada nástrojů, přímo na stránce centra oznámení hello, jak je uvedené v hello obrazovce níže.</span><span class="sxs-lookup"><span data-stu-id="ae069-149">You can test receiving push notifications in your app by sending notifications in hello [Azure Portal] via hello **Test Send** capability in hello **Troubleshooting** toolset, right in hello notification hub page, as shown in hello screen below.</span></span>

![](./media/notification-hubs-ios-get-started/notification-hubs-test-send.png)

<span data-ttu-id="ae069-150">Nabízená oznámení se většinou posílají pomocí služby backend, jako je služba Mobile Services, nebo v technologii ASP.NET pomocí kompatibilní knihovny.</span><span class="sxs-lookup"><span data-stu-id="ae069-150">Push notifications are normally sent through a back-end service like Mobile Services or ASP.NET using a compatible library.</span></span> <span data-ttu-id="ae069-151">Můžete taky hello REST API přímo toosend nabízené zprávy, pokud není k dispozici ve vašem scénáři knihovny.</span><span class="sxs-lookup"><span data-stu-id="ae069-151">You can also use hello REST API directly toosend push messages if a library is not available in your scenario.</span></span> 

<span data-ttu-id="ae069-152">V tomto kurzu jsme dělat nic složitého a jednoduše předvedeme testování vaší klientské aplikace pomocí odesílání oznámení hello .NET SDK pro centra oznámení v konzolové aplikaci, místo back-end službu.</span><span class="sxs-lookup"><span data-stu-id="ae069-152">In this tutorial, we will keep it simple and just demonstrate testing your client app by sending notifications using hello .NET SDK for notification hubs in a console application instead of a backend service.</span></span> <span data-ttu-id="ae069-153">Doporučujeme, abyste hello [použití centra oznámení toopush oznámení toousers](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) kurz jako hello další krok pro odesílání oznámení z backendu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ae069-153">We recommend hello [Use Notification Hubs toopush notifications toousers](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) tutorial as hello next step for sending notifications from an ASP.NET backend.</span></span> <span data-ttu-id="ae069-154">Hello následující přístupy lze však použít pro zasílání oznámení:</span><span class="sxs-lookup"><span data-stu-id="ae069-154">However, hello following approaches can be used for sending notifications:</span></span>

* <span data-ttu-id="ae069-155">**Rozhraní REST**: nabízené oznámení můžete podporovat na jakékoli platformě back-end pomocí hello [rozhraní REST](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="ae069-155">**REST Interface**:  You can support push notification on any backend platform using  hello [REST interface](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).</span></span>
* <span data-ttu-id="ae069-156">**Microsoft Azure oznámení centra .NET SDK**: hello Správce balíčků Nuget pro Visual Studio, spusťte [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="ae069-156">**Microsoft Azure Notification Hubs .NET SDK**: In hello Nuget Package Manager for Visual Studio, run [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>
* <span data-ttu-id="ae069-157">**Node.js** : [jak toouse Notification Hubs z Node.js](notification-hubs-nodejs-push-notification-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="ae069-157">**Node.js** : [How toouse Notification Hubs from Node.js](notification-hubs-nodejs-push-notification-tutorial.md).</span></span>

<span data-ttu-id="ae069-158">**Mobile Apps**: pro příklad toosend oznámení z backendu Azure App Service Mobile Apps, které jsou integrovány v centrech oznámení najdete v části [přidat nabízená oznámení tooyour mobilní aplikace](../app-service-mobile/app-service-mobile-ios-get-started-push.md).</span><span class="sxs-lookup"><span data-stu-id="ae069-158">**Mobile Apps**: For an example of how toosend notifications from an Azure App Service Mobile Apps backend that's integrated with Notification Hubs, see [Add push notifications tooyour mobile app](../app-service-mobile/app-service-mobile-ios-get-started-push.md).</span></span>

* <span data-ttu-id="ae069-159">**Java / PHP**: Příklad jak hello toosend nabízená oznámení pomocí rozhraní REST API, naleznete v části "jak toouse centra oznámení z Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).</span><span class="sxs-lookup"><span data-stu-id="ae069-159">**Java / PHP**: For an example of how toosend push notifications by using hello REST APIs, see "How toouse Notification Hubs from Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).</span></span>

#### <a name="optional-send-push-notifications-from-a-net-console-app"></a><span data-ttu-id="ae069-160">(Volitelné) Odesílání nabízených oznámení z konzoly aplikace .NET</span><span class="sxs-lookup"><span data-stu-id="ae069-160">(Optional) Send Push Notifications from a .NET Console App</span></span>
<span data-ttu-id="ae069-161">V této části odešleme nabízená oznámení pomocí konzolové aplikace .NET</span><span class="sxs-lookup"><span data-stu-id="ae069-161">In this section, we will send push notifications by using a simple .NET console app.</span></span> <span data-ttu-id="ae069-162">Pro účely hello tohoto příkladu přepněme tooa vývojového prostředí systému Windows, který má nainstalované Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ae069-162">For hello purposes of this example, we will switch tooa Windows development environment that has Visual Studio already installed.</span></span>

1. <span data-ttu-id="ae069-163">Ve Visual Studiu vytvořte novou konzolovou aplikaci Visual C#:</span><span class="sxs-lookup"><span data-stu-id="ae069-163">In Visual Studio, create a new Visual C# console application:</span></span>
   
       ![Visual Studio - Create a new console application][213]
2. <span data-ttu-id="ae069-164">Ve Visual Studiu klikněte na položku **Nástroje**, klikněte na **Správce balíčků NuGet** a pak klikněte na **Konzola Správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="ae069-164">In Visual Studio, click **Tools**, click **NuGet Package Manager**, and then click **Package Manager Console**.</span></span>
   
    <span data-ttu-id="ae069-165">konzole Správce balíčků Hello by se zobrazit ukotvený toohello dolní části pracovního prostoru Visual Studia.</span><span class="sxs-lookup"><span data-stu-id="ae069-165">hello package manager console should appear docked toohello bottom of your Visual Studio workspace.</span></span>
3. <span data-ttu-id="ae069-166">V okně konzoly Správce balíčků hello, nastavte hello **výchozí projekt** tooyour nové konzoly projekt aplikace a potom v okně konzoly hello, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="ae069-166">In hello Package Manager Console window, set hello **Default project** tooyour new console application project, and then in hello console window, execute hello following command:</span></span>
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    <span data-ttu-id="ae069-167">Tento postup přidá odkaz toohello SDK centra oznámení Azure pomocí hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">balíčku Microsoft.Azure.Notification Hubs NuGet</a>.</span><span class="sxs-lookup"><span data-stu-id="ae069-167">This adds a reference toohello Azure Notification Hubs SDK using hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>.</span></span>
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. <span data-ttu-id="ae069-168">Otevřete hello `Program.cs` souboru a přidejte následující hello `using` prohlášení, zajistíte, že budeme moci použít třídy a funkce v rámci hlavní třídy Azure:</span><span class="sxs-lookup"><span data-stu-id="ae069-168">Open hello `Program.cs` file and add hello following `using` statement, ensuring that we can use Azure classes and functions within your main class:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
5. <span data-ttu-id="ae069-169">Ve vašem `Program` třídy, přidejte následující metodu hello (Nezapomeňte tooreplace hello **připojovací řetězec** a **název centra**):</span><span class="sxs-lookup"><span data-stu-id="ae069-169">In your `Program` class, add hello following method (don't forget tooreplace hello **connection string** and **hub name**):</span></span>
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var alert = "{\"aps\":{\"alert\":\"Hello from .NET!\"}}";
            await hub.SendAppleNativeNotificationAsync(alert);
        }
6. <span data-ttu-id="ae069-170">Přidejte následující řádky do hello vaší `Main` metoda:</span><span class="sxs-lookup"><span data-stu-id="ae069-170">Add hello following lines in your `Main` method:</span></span>
   
         SendNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="ae069-171">Stisknutím klávesy hello F5 klíče toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae069-171">Press hello F5 key toorun hello app.</span></span> <span data-ttu-id="ae069-172">Během několika sekund měli byste vidět nabízená oznámení v zařízení.</span><span class="sxs-lookup"><span data-stu-id="ae069-172">Within seconds, you should see a push notification appear on your device.</span></span> <span data-ttu-id="ae069-173">Ať používáte Wi-Fi nebo mobilních datovou síť, ujistěte se, že máte na hello zařízení aktivní připojení k Internetu.</span><span class="sxs-lookup"><span data-stu-id="ae069-173">Whether you are using Wi-Fi or a cellular data network, make sure that you have an active internet connection on hello device.</span></span>

<span data-ttu-id="ae069-174">Můžete najít všechny možné datové části hello v hello Apple [průvodci místních a nabízených oznámení programování].</span><span class="sxs-lookup"><span data-stu-id="ae069-174">You can find all hello possible payloads in hello Apple [Local and Push Notification Programming Guide].</span></span>

#### <a name="optional-send-notifications-from-a-mobile-service"></a><span data-ttu-id="ae069-175">(Volitelné) Odesílání oznámení z mobilní služby</span><span class="sxs-lookup"><span data-stu-id="ae069-175">(Optional) Send Notifications from a Mobile Service</span></span>
<span data-ttu-id="ae069-176">V této části vám odešleme nabízená oznámení pomocí mobilních služeb prostřednictvím skriptu uzlu.</span><span class="sxs-lookup"><span data-stu-id="ae069-176">In this section, we will send push notifications using a mobile service through a node script.</span></span>

<span data-ttu-id="ae069-177">postupujte podle toosend oznámení pomocí mobilních služeb, [Začínáme s Mobile Services]a pak:</span><span class="sxs-lookup"><span data-stu-id="ae069-177">toosend a notification by using a mobile service, follow [Get started with Mobile Services], and then:</span></span>

1. <span data-ttu-id="ae069-178">Přihlaste se toohello [portálu Azure Classic]a vyberte mobilní služby.</span><span class="sxs-lookup"><span data-stu-id="ae069-178">Sign in toohello [Azure Classic Portal], and select your mobile service.</span></span>
2. <span data-ttu-id="ae069-179">Vyberte hello **Plánovač** karty v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="ae069-179">Select hello **Scheduler** tab on hello top.</span></span>
   
       ![Azure Classic Portal - Scheduler][215]
3. <span data-ttu-id="ae069-180">Vytvořte novou naplánovanou úlohu, vložte název a vyberte **Na vyžádání**.</span><span class="sxs-lookup"><span data-stu-id="ae069-180">Create a new scheduled job, insert a name, and select **On demand**.</span></span>
   
       ![Azure Classic Portal - Create new job][216]
4. <span data-ttu-id="ae069-181">Když se vytvoří úloha hello, klikněte na název úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="ae069-181">When hello job is created, click hello job name.</span></span> <span data-ttu-id="ae069-182">Pak klikněte na tlačítko hello **skriptu** karty na horním panelu hello.</span><span class="sxs-lookup"><span data-stu-id="ae069-182">Then click hello **Script** tab on hello top bar.</span></span>
5. <span data-ttu-id="ae069-183">Vložte následující skript dovnitř funkce plánovače hello.</span><span class="sxs-lookup"><span data-stu-id="ae069-183">Insert hello following script inside your scheduler function.</span></span> <span data-ttu-id="ae069-184">Ujistěte se, že tooreplace zástupné symboly hello oznámení centra název a hello připojovacím řetězcem pro *DefaultFullSharedAccessSignature* kterou jste získali dříve.</span><span class="sxs-lookup"><span data-stu-id="ae069-184">Make sure tooreplace hello placeholders with your notification hub name and hello connection string for *DefaultFullSharedAccessSignature* that you obtained earlier.</span></span> <span data-ttu-id="ae069-185">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ae069-185">Click **Save**.</span></span>
   
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
6. <span data-ttu-id="ae069-186">Klikněte na tlačítko **spustit jednou** na dolním panelu hello.</span><span class="sxs-lookup"><span data-stu-id="ae069-186">Click **Run Once** on hello bottom bar.</span></span> <span data-ttu-id="ae069-187">Měli byste obdržet upozornění na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="ae069-187">You should receive an alert on your device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ae069-188">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ae069-188">Next steps</span></span>
<span data-ttu-id="ae069-189">V tomto jednoduchém příkladu jste vysílali nabízená oznámení tooall zařízení iOS.</span><span class="sxs-lookup"><span data-stu-id="ae069-189">In this simple example, you broadcasted push notifications tooall your iOS devices.</span></span> <span data-ttu-id="ae069-190">V pořadí tootarget konkrétní uživatele, najdete v kurzu toohello [použití centra oznámení toopush oznámení toousers].</span><span class="sxs-lookup"><span data-stu-id="ae069-190">In order tootarget specific users, refer toohello tutorial [Use Notification Hubs toopush notifications toousers].</span></span> <span data-ttu-id="ae069-191">Pokud chcete toosegment uživatele podle zájmových skupin, můžete si přečíst [toosend použití centra oznámení nejnovější zprávy přes].</span><span class="sxs-lookup"><span data-stu-id="ae069-191">If you want toosegment your users by interest groups, you can read [Use Notification Hubs toosend breaking news].</span></span> <span data-ttu-id="ae069-192">Další informace o toouse centra oznámení v [pokyny centra oznámení] a v hello [iOS Notification Hubs postupy toofor].</span><span class="sxs-lookup"><span data-stu-id="ae069-192">Learn more about how toouse Notification Hubs in [Notification Hubs Guidance] and in hello [Notification Hubs How-toofor iOS].</span></span>

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
