---
title: "Přidání nabízených oznámení do vaší aplikace Xamarin.iOS pomocí služby Azure App Service"
description: "Naučte se používat Azure App Service k odesílání nabízených oznámení do aplikace Xamarin.iOS"
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
ms.openlocfilehash: bf922e49c4c92d0065817a5dd6c7d10a04737304
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="add-push-notifications-to-your-xamarinios-app"></a><span data-ttu-id="203e4-103">Přidání nabízených oznámení do aplikace Xamarin.iOS</span><span class="sxs-lookup"><span data-stu-id="203e4-103">Add push notifications to your Xamarin.iOS App</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="203e4-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="203e4-104">Overview</span></span>
<span data-ttu-id="203e4-105">V tomto kurzu přidáte nabízená oznámení [Xamarin.iOS úvodní](app-service-mobile-xamarin-ios-get-started.md) projektu tak, aby nabízených oznámení se odešle do zařízení pokaždé, když vložení záznamu.</span><span class="sxs-lookup"><span data-stu-id="203e4-105">In this tutorial, you add push notifications to the [Xamarin.iOS quick start](app-service-mobile-xamarin-ios-get-started.md) project so that a push notification is sent to the device every time a record is inserted.</span></span>

<span data-ttu-id="203e4-106">Pokud použijete serverový projekt stažené rychlý start, budete potřebovat balíček rozšíření nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="203e4-106">If you do not use the downloaded quick start server project, you will need the push notification extension package.</span></span> <span data-ttu-id="203e4-107">V tématu [pracovat s .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="203e4-107">See [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="203e4-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="203e4-108">Prerequisites</span></span>
* <span data-ttu-id="203e4-109">Dokončení [rychlý start Xamarin.iOS](app-service-mobile-xamarin-ios-get-started.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="203e4-109">Complete the [Xamarin.iOS quickstart](app-service-mobile-xamarin-ios-get-started.md) tutorial.</span></span>
* <span data-ttu-id="203e4-110">Fyzickém zařízení iOS.</span><span class="sxs-lookup"><span data-stu-id="203e4-110">A physical iOS device.</span></span> <span data-ttu-id="203e4-111">Nabízená oznámení nepodporuje simulátoru iOS.</span><span class="sxs-lookup"><span data-stu-id="203e4-111">Push notifications are not supported by the iOS simulator.</span></span>

## <a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a><span data-ttu-id="203e4-112">Registraci aplikace pro nabízená oznámení na portál pro vývojáře společnosti Apple</span><span class="sxs-lookup"><span data-stu-id="203e4-112">Register the app for push notifications on Apple's developer portal</span></span>
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-your-mobile-app-to-send-push-notifications"></a><span data-ttu-id="203e4-113">Konfigurace mobilní aplikace k odesílání nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="203e4-113">Configure your Mobile App to send push notifications</span></span>
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <a name="update-the-server-project-to-send-push-notifications"></a><span data-ttu-id="203e4-114">Aktualizace serverový projekt k odesílání nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="203e4-114">Update the server project to send push notifications</span></span>
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-your-xamarinios-project"></a><span data-ttu-id="203e4-115">Konfigurace projektu Xamarin.iOS</span><span class="sxs-lookup"><span data-stu-id="203e4-115">Configure your Xamarin.iOS project</span></span>
[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

## <a name="add-push-notifications-to-your-app"></a><span data-ttu-id="203e4-116">Přidání nabízených oznámení do aplikace</span><span class="sxs-lookup"><span data-stu-id="203e4-116">Add push notifications to your app</span></span>
1. <span data-ttu-id="203e4-117">V **QSTodoService**, přidejte následující vlastnost tak, aby **AppDelegate** můžete získat mobilního klienta:</span><span class="sxs-lookup"><span data-stu-id="203e4-117">In **QSTodoService**, add the following property so that **AppDelegate** can acquire the mobile client:</span></span>
   
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
2. <span data-ttu-id="203e4-118">Přidejte následující `using` příkaz do horní části **AppDelegate.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="203e4-118">Add the following `using` statement to the top of the **AppDelegate.cs** file.</span></span>
   
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
3. <span data-ttu-id="203e4-119">V **AppDelegate**, přepsat **FinishedLaunching** událostí:</span><span class="sxs-lookup"><span data-stu-id="203e4-119">In **AppDelegate**, override the **FinishedLaunching** event:</span></span>
   
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
4. <span data-ttu-id="203e4-120">Ve stejném souboru přepsat **RegisteredForRemoteNotifications** událostí.</span><span class="sxs-lookup"><span data-stu-id="203e4-120">In the same file, override the **RegisteredForRemoteNotifications** event.</span></span> <span data-ttu-id="203e4-121">V tomto kódu jsou registrace pro jednoduchou šablonu oznámení, která bude odeslána pro všechny podporované platformy serverem.</span><span class="sxs-lookup"><span data-stu-id="203e4-121">In this code you are registering for a simple template notification that will be sent across all supported platforms by the server.</span></span>
   
    <span data-ttu-id="203e4-122">Další informace o šablonách s Notification Hubs najdete v tématu [šablony](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).</span><span class="sxs-lookup"><span data-stu-id="203e4-122">For more information on templates with Notification Hubs, see [Templates](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).</span></span>

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


1. <span data-ttu-id="203e4-123">Potom přepsat **DidReceivedRemoteNotification** událostí:</span><span class="sxs-lookup"><span data-stu-id="203e4-123">Then, override the **DidReceivedRemoteNotification** event:</span></span>
   
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

<span data-ttu-id="203e4-124">Aplikace je nyní aktualizovat o podporu nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="203e4-124">Your app is now updated to support push notifications.</span></span>

## <span data-ttu-id="203e4-125"><a name="test"></a>Nabízená oznámení v aplikaci</span><span class="sxs-lookup"><span data-stu-id="203e4-125"><a name="test"></a>Test push notifications in your app</span></span>
1. <span data-ttu-id="203e4-126">Stiskněte **spustit** tlačítko pro sestavení projektu a spusťte aplikaci v zařízení podporující iOS a pak klikněte na **OK** přijímat nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="203e4-126">Press the **Run** button to build the project and start the app in an iOS capable device, then click **OK** to accept push notifications.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="203e4-127">Je nutné explicitně přijmout nabízená oznámení z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="203e4-127">You must explicitly accept push notifications from your app.</span></span> <span data-ttu-id="203e4-128">Tento požadavek dochází pouze při prvním spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="203e4-128">This request only occurs the first time that the app runs.</span></span>
   > 
   > 
2. <span data-ttu-id="203e4-129">V aplikaci zadejte úlohu a potom klikněte na tlačítko plus (**+**) ikona.</span><span class="sxs-lookup"><span data-stu-id="203e4-129">In the app, type a task, and then click the plus (**+**) icon.</span></span>
3. <span data-ttu-id="203e4-130">Ověřte, že přijetí oznámení a pak klikněte na **OK** k zavření oznámení.</span><span class="sxs-lookup"><span data-stu-id="203e4-130">Verify that a notification is received, then click **OK** to dismiss the notification.</span></span>
4. <span data-ttu-id="203e4-131">Opakujte krok 2 a okamžitě zavřete aplikaci a pak ověřte, zda je zobrazen oznámení.</span><span class="sxs-lookup"><span data-stu-id="203e4-131">Repeat step 2 and immediately close the app, then verify that a notification is shown.</span></span>

<span data-ttu-id="203e4-132">Úspěšně jste dokončili tento kurz.</span><span class="sxs-lookup"><span data-stu-id="203e4-132">You have successfully completed this tutorial.</span></span>

<!-- Images. -->

<!-- URLs. -->



