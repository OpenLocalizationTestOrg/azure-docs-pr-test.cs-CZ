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
# <a name="add-push-notifications-tooyour-xamarinios-app"></a><span data-ttu-id="fd840-103">Přidat nabízená oznámení tooyour aplikace Xamarin.iOS</span><span class="sxs-lookup"><span data-stu-id="fd840-103">Add push notifications tooyour Xamarin.iOS App</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="fd840-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="fd840-104">Overview</span></span>
<span data-ttu-id="fd840-105">V tomto kurzu přidáte nabízená oznámení toohello [Xamarin.iOS úvodní](app-service-mobile-xamarin-ios-get-started.md) projektu tak, aby nabízených oznámení je odesláno toohello zařízení pokaždé, když vložení záznamu.</span><span class="sxs-lookup"><span data-stu-id="fd840-105">In this tutorial, you add push notifications toohello [Xamarin.iOS quick start](app-service-mobile-xamarin-ios-get-started.md) project so that a push notification is sent toohello device every time a record is inserted.</span></span>

<span data-ttu-id="fd840-106">Pokud nepoužijete hello stáhli úvodní serverový projekt, bude nutné hello nabízených oznámení v balíčku rozšíření.</span><span class="sxs-lookup"><span data-stu-id="fd840-106">If you do not use hello downloaded quick start server project, you will need hello push notification extension package.</span></span> <span data-ttu-id="fd840-107">V tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="fd840-107">See [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fd840-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fd840-108">Prerequisites</span></span>
* <span data-ttu-id="fd840-109">Dokončení hello [rychlý start Xamarin.iOS](app-service-mobile-xamarin-ios-get-started.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="fd840-109">Complete hello [Xamarin.iOS quickstart](app-service-mobile-xamarin-ios-get-started.md) tutorial.</span></span>
* <span data-ttu-id="fd840-110">Fyzickém zařízení iOS.</span><span class="sxs-lookup"><span data-stu-id="fd840-110">A physical iOS device.</span></span> <span data-ttu-id="fd840-111">Nabízená oznámení nepodporuje hello simulátoru iOS.</span><span class="sxs-lookup"><span data-stu-id="fd840-111">Push notifications are not supported by hello iOS simulator.</span></span>

## <a name="register-hello-app-for-push-notifications-on-apples-developer-portal"></a><span data-ttu-id="fd840-112">Zaregistrovat hello aplikace pro nabízená oznámení na portál pro vývojáře společnosti Apple</span><span class="sxs-lookup"><span data-stu-id="fd840-112">Register hello app for push notifications on Apple's developer portal</span></span>
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-your-mobile-app-toosend-push-notifications"></a><span data-ttu-id="fd840-113">Konfigurace mobilní aplikace toosend nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="fd840-113">Configure your Mobile App toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <a name="update-hello-server-project-toosend-push-notifications"></a><span data-ttu-id="fd840-114">Aktualizovat hello serveru projektu toosend nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="fd840-114">Update hello server project toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-your-xamarinios-project"></a><span data-ttu-id="fd840-115">Konfigurace projektu Xamarin.iOS</span><span class="sxs-lookup"><span data-stu-id="fd840-115">Configure your Xamarin.iOS project</span></span>
[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

## <a name="add-push-notifications-tooyour-app"></a><span data-ttu-id="fd840-116">Přidat nabízená oznámení tooyour aplikaci</span><span class="sxs-lookup"><span data-stu-id="fd840-116">Add push notifications tooyour app</span></span>
1. <span data-ttu-id="fd840-117">V **QSTodoService**, přidejte následující vlastnost hello tak, aby **AppDelegate** můžete získat hello mobilního klienta:</span><span class="sxs-lookup"><span data-stu-id="fd840-117">In **QSTodoService**, add hello following property so that **AppDelegate** can acquire hello mobile client:</span></span>
   
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
2. <span data-ttu-id="fd840-118">Přidejte následující hello `using` příkaz toohello začátek hello **AppDelegate.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="fd840-118">Add hello following `using` statement toohello top of hello **AppDelegate.cs** file.</span></span>
   
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
3. <span data-ttu-id="fd840-119">V **AppDelegate**, přepsání hello **FinishedLaunching** událostí:</span><span class="sxs-lookup"><span data-stu-id="fd840-119">In **AppDelegate**, override hello **FinishedLaunching** event:</span></span>
   
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
4. <span data-ttu-id="fd840-120">V hello stejného souboru, přepsání hello **RegisteredForRemoteNotifications** událostí.</span><span class="sxs-lookup"><span data-stu-id="fd840-120">In hello same file, override hello **RegisteredForRemoteNotifications** event.</span></span> <span data-ttu-id="fd840-121">V tomto kódu jsou registrace pro jednoduchou šablonu oznámení, která bude odeslána pro všechny podporované platformy serverem hello.</span><span class="sxs-lookup"><span data-stu-id="fd840-121">In this code you are registering for a simple template notification that will be sent across all supported platforms by hello server.</span></span>
   
    <span data-ttu-id="fd840-122">Další informace o šablonách s Notification Hubs najdete v tématu [šablony](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).</span><span class="sxs-lookup"><span data-stu-id="fd840-122">For more information on templates with Notification Hubs, see [Templates](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).</span></span>

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


1. <span data-ttu-id="fd840-123">Potom přepsat hello **DidReceivedRemoteNotification** událostí:</span><span class="sxs-lookup"><span data-stu-id="fd840-123">Then, override hello **DidReceivedRemoteNotification** event:</span></span>
   
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

<span data-ttu-id="fd840-124">Vaše aplikace je teď aktualizovaný toosupport nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="fd840-124">Your app is now updated toosupport push notifications.</span></span>

## <span data-ttu-id="fd840-125"><a name="test"></a>Nabízená oznámení v aplikaci</span><span class="sxs-lookup"><span data-stu-id="fd840-125"><a name="test"></a>Test push notifications in your app</span></span>
1. <span data-ttu-id="fd840-126">Stiskněte klávesu hello **spustit** tlačítko toobuild hello projektu a spusťte aplikaci hello v podporuje zařízení s iOS a pak klikněte na **OK** tooaccept nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="fd840-126">Press hello **Run** button toobuild hello project and start hello app in an iOS capable device, then click **OK** tooaccept push notifications.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="fd840-127">Je nutné explicitně přijmout nabízená oznámení z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="fd840-127">You must explicitly accept push notifications from your app.</span></span> <span data-ttu-id="fd840-128">Tento požadavek dochází pouze v hello prvním hello aplikace běží.</span><span class="sxs-lookup"><span data-stu-id="fd840-128">This request only occurs hello first time that hello app runs.</span></span>
   > 
   > 
2. <span data-ttu-id="fd840-129">Aplikace hello typ úlohy a pak klikněte na hello plus (**+**) ikona.</span><span class="sxs-lookup"><span data-stu-id="fd840-129">In hello app, type a task, and then click hello plus (**+**) icon.</span></span>
3. <span data-ttu-id="fd840-130">Ověřte, že přijetí oznámení a pak klikněte na **OK** toodismiss hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="fd840-130">Verify that a notification is received, then click **OK** toodismiss hello notification.</span></span>
4. <span data-ttu-id="fd840-131">Opakujte krok 2 a okamžitě zavřít hello aplikaci a potom ověřte, zda je zobrazen oznámení.</span><span class="sxs-lookup"><span data-stu-id="fd840-131">Repeat step 2 and immediately close hello app, then verify that a notification is shown.</span></span>

<span data-ttu-id="fd840-132">Úspěšně jste dokončili tento kurz.</span><span class="sxs-lookup"><span data-stu-id="fd840-132">You have successfully completed this tutorial.</span></span>

<!-- Images. -->

<!-- URLs. -->



