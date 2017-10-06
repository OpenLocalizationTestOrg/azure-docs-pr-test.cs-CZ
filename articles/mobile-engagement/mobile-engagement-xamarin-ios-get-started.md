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
# <a name="get-started-with-azure-mobile-engagement-for-xamarinios-apps"></a><span data-ttu-id="0d7e7-103">Začínáme s Azure Mobile Engagementem pro aplikace Xamarin.iOS</span><span class="sxs-lookup"><span data-stu-id="0d7e7-103">Get Started with Azure Mobile Engagement for Xamarin.iOS Apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="0d7e7-104">Toto téma ukazuje, jak Azure Mobile Engagement toounderstand toouse využití aplikací a odesílat nabízená oznámení toosegmented uživatelům aplikace pro Xamarin.iOS.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and send push notifications toosegmented users in a Xamarin.iOS application.</span></span>
<span data-ttu-id="0d7e7-105">V tomto kurzu vytvoříte prázdnou aplikaci Xamarin.iOS, která bude shromažďovat základní data a přijímat nabízená oznámení pomocí systému nabízených oznámení Apple (APNS). </span><span class="sxs-lookup"><span data-stu-id="0d7e7-105">In this tutorial, you create a blank Xamarin.iOS app that collects basic data and receives push notifications using Apple Push Notification System (APNS).</span></span>

> [!NOTE]
> <span data-ttu-id="0d7e7-106">Služba Azure Mobile Engagement Hello vyřadí března 2018 a je aktuálně pouze k dispozici tooexisting zákazníků.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-106">hello Azure Mobile Engagement service will be retired March 2018 and is currently only available tooexisting customers.</span></span> <span data-ttu-id="0d7e7-107">Další informace najdete v tématu [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="0d7e7-107">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

<span data-ttu-id="0d7e7-108">Tento kurz vyžaduje hello následující:</span><span class="sxs-lookup"><span data-stu-id="0d7e7-108">This tutorial requires hello following:</span></span>

* <span data-ttu-id="0d7e7-109">[Xamarin Studio](http://xamarin.com/studio).</span><span class="sxs-lookup"><span data-stu-id="0d7e7-109">[Xamarin Studio](http://xamarin.com/studio).</span></span> <span data-ttu-id="0d7e7-110">Můžete také použít Visual Studio s Xamarinem, ale v tomto kurzu používáme Xamarin Studio.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-110">You can also use Visual Studio with Xamarin but this tutorial uses Xamarin Studio.</span></span> <span data-ttu-id="0d7e7-111">Instalační pokyny najdete v tématu o [nastavení a instalaci pro Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span><span class="sxs-lookup"><span data-stu-id="0d7e7-111">For installation instructions, see [Setup and Install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span></span> 
* [<span data-ttu-id="0d7e7-112">Mobile Engagement Xamarin SDK</span><span class="sxs-lookup"><span data-stu-id="0d7e7-112">Mobile Engagement Xamarin SDK</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [!NOTE]
> <span data-ttu-id="0d7e7-113">toocomplete tento kurz, musíte mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-113">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="0d7e7-114">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-114">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="0d7e7-115">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="0d7e7-115">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).</span></span>
> 
> 

## <span data-ttu-id="0d7e7-116"><a id="setup-azme"></a>Nastavení Mobile Engagementu pro vaši aplikaci pro iOS</span><span class="sxs-lookup"><span data-stu-id="0d7e7-116"><a id="setup-azme"></a>Setup Mobile Engagement for your iOS app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="0d7e7-117"><a id="connecting-app"></a>Připojit vaše aplikace toohello Mobile Engagement back-end</span><span class="sxs-lookup"><span data-stu-id="0d7e7-117"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="0d7e7-118">Tento kurzu si představíme "základní integraci", což je minimální hello nastavte požadované toocollect dat a odesílání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-118">This tutorial presents a "basic integration" which is hello minimal set required toocollect data and send a push notification.</span></span>

<span data-ttu-id="0d7e7-119">Pomocí Xamarin toodemonstrate hello integrace vytvoříme základní aplikaci:</span><span class="sxs-lookup"><span data-stu-id="0d7e7-119">We will create a basic app with Xamarin toodemonstrate hello integration:</span></span>

### <a name="create-a-new-xamarinios-project"></a><span data-ttu-id="0d7e7-120">Vytvoření nového projektu Xamarin.iOS</span><span class="sxs-lookup"><span data-stu-id="0d7e7-120">Create a new Xamarin.iOS project</span></span>
1. <span data-ttu-id="0d7e7-121">Spusťte Xamarin Studio.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-121">Launch Xamarin Studio.</span></span> <span data-ttu-id="0d7e7-122">Přejděte příliš**soubor** -> **nový** -> **řešení**</span><span class="sxs-lookup"><span data-stu-id="0d7e7-122">Go too**File** -> **New** -> **Solution**</span></span> 
   
    ![][1]
2. <span data-ttu-id="0d7e7-123">Vyberte **jediné zobrazení aplikace**, zkontrolujte, zda je hello vybraný jazyk **C#** a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-123">Select **Single View App**, make sure hello selected language is **C#** and then click **Next**.</span></span>
   
    ![][2]
3. <span data-ttu-id="0d7e7-124">Vyplňte hello **název aplikace** a hello **identifikátor organizace** a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-124">Fill in hello **App Name** and hello **Organization Identifier** and then click **Next**.</span></span> 
   
    ![][3]
   
   > [!IMPORTANT]
   > <span data-ttu-id="0d7e7-125">Ujistěte se, že hello publikování profil nakonec použijete toodeploy vaší aplikace pro iOS je pomocí ID aplikace, které odpovídá přesně hello zde máte identifikátor svazku.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-125">Make sure that hello publishing profile you eventually use toodeploy your iOS app is using an App ID which matches exactly with hello Bundle Identifier you have here.</span></span> 
   > 
   > 
4. <span data-ttu-id="0d7e7-126">Aktualizace hello **název projektu**, **název řešení** a **umístění** dle potřeby **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-126">Update hello **Project Name**, **Solution Name** and **Location** if required and click **Create**.</span></span>
   
    ![][4]

<span data-ttu-id="0d7e7-127">Xamarin Studio vytvoří hello ukázkovou aplikaci, do které budeme integrovat Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-127">Xamarin Studio will create hello demo app in which we will integrate Mobile Engagement.</span></span> 

### <a name="connect-your-app-toomobile-engagement-backend"></a><span data-ttu-id="0d7e7-128">Připojit vaše tooMobile Engagement back-end aplikace</span><span class="sxs-lookup"><span data-stu-id="0d7e7-128">Connect your app tooMobile Engagement backend</span></span>
1. <span data-ttu-id="0d7e7-129">Klikněte pravým tlačítkem na hello **balíčky** složku v systému windows hello řešení a vyberte **přidat balíčky...**</span><span class="sxs-lookup"><span data-stu-id="0d7e7-129">Right click hello **Packages** folder in hello Solution windows and select **Add Packages...**</span></span>
   
    ![][5]
2. <span data-ttu-id="0d7e7-130">Vyhledejte hello **Microsoft Azure Mobile Engagement Xamarin SDK** a přidejte ji tooyour řešení.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-130">Search for hello **Microsoft Azure Mobile Engagement Xamarin SDK** and add it tooyour solution.</span></span>  
   
    ![][6]
3. <span data-ttu-id="0d7e7-131">Otevřete **AppDelegate.cs** a přidejte hello následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="0d7e7-131">Open **AppDelegate.cs** and add hello following using statement:</span></span>
   
        using Microsoft.Azure.Engagement.Xamarin;
4. <span data-ttu-id="0d7e7-132">V hello **FinishedLaunching** metoda, přidejte následující tooinitialize hello připojení s back-end Mobile Engagementu hello.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-132">In hello **FinishedLaunching** method, add hello following tooinitialize hello connection with Mobile Engagement backend.</span></span> <span data-ttu-id="0d7e7-133">Ujistěte se, že tooadd vaše **ConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-133">Make sure tooadd your **ConnectionString**.</span></span> <span data-ttu-id="0d7e7-134">Tento kód také používá fiktivní **NotificationIcon** který přidává Mobile Engagement SDK, který může být vhodné hello tooreplace.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-134">This code also uses a dummy **NotificationIcon** which is added by hello Mobile Engagement SDK which you may want tooreplace.</span></span> 
   
        EngagementConfiguration config = new EngagementConfiguration {
                        ConnectionString = "YourConnectionStringFromAzurePortal",
                        NotificationIcon = UIImage.FromBundle("close")
                    };
        EngagementAgent.Init (config);

## <span data-ttu-id="0d7e7-135"><a id="monitor"></a>Povolení sledování v reálném čase</span><span class="sxs-lookup"><span data-stu-id="0d7e7-135"><a id="monitor"></a>Enabling real-time monitoring</span></span>
<span data-ttu-id="0d7e7-136">V pořadí toostart odesílat data a zajistit, že hello uživatelé jsou aktivní musíte odeslat alespoň jednu obrazovku toohello Mobile Engagement back-end.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-136">In order toostart sending data and ensuring hello users are active, you must send at least one screen toohello Mobile Engagement backend.</span></span>

1. <span data-ttu-id="0d7e7-137">Otevřete **ViewController.cs** a přidejte hello následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="0d7e7-137">Open **ViewController.cs** and add hello following using statement:</span></span>
   
        using Microsoft.Azure.Engagement.Xamarin;
2. <span data-ttu-id="0d7e7-138">Nahraďte hello třídu, ze které `ViewController` dědí z `UIViewController` příliš`EngagementViewController`.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-138">Replace hello class from which `ViewController` inherits from `UIViewController` too`EngagementViewController`.</span></span> 

## <span data-ttu-id="0d7e7-139"><a id="monitor"></a>Připojení aplikace se sledováním v reálném čase</span><span class="sxs-lookup"><span data-stu-id="0d7e7-139"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="0d7e7-140"><a id="integrate-push"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci</span><span class="sxs-lookup"><span data-stu-id="0d7e7-140"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="0d7e7-141">Mobile Engagement umožňuje toointeract s uživateli a REACH s nabízená oznámení a zasílání zpráv v kontextu hello kampaní v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-141">Mobile Engagement allows you toointeract with your users and REACH with push notifications and in-app messaging in hello context of campaigns.</span></span> <span data-ttu-id="0d7e7-142">Tento modul je hello portálu Mobile Engagement nazývá REACH.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-142">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="0d7e7-143">Hello následujících sekcích nastavíte tooreceive vaše aplikace je.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-143">hello following sections set up your app tooreceive them.</span></span>

### <a name="modify-your-application-delegate"></a><span data-ttu-id="0d7e7-144">Úprava delegáta aplikace</span><span class="sxs-lookup"><span data-stu-id="0d7e7-144">Modify your Application Delegate</span></span>
1. <span data-ttu-id="0d7e7-145">Otevřete hello **AppDelegate.cs** a přidejte hello následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="0d7e7-145">Open hello **AppDelegate.cs** and add hello following using statement:</span></span>
   
        using System; 
2. <span data-ttu-id="0d7e7-146">Nyní uvnitř hello `FinishedLaunching` metody přidat hello následující tooregister nabízená oznámení`EngagementAgent.init(...)`</span><span class="sxs-lookup"><span data-stu-id="0d7e7-146">Now inside hello `FinishedLaunching` method, add hello following tooregister for push messages after `EngagementAgent.init(...)`</span></span>
   
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
3. <span data-ttu-id="0d7e7-147">Nakonec - aktualizujte nebo přidejte hello následující metody:</span><span class="sxs-lookup"><span data-stu-id="0d7e7-147">Finally - update or add hello following methods:</span></span>
   
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
4. <span data-ttu-id="0d7e7-148">Ve vaší **Info.plist** souboru v hello řešení, potvrďte, že hello **identifikátor svazku** shoduje s hello **ID aplikace** máte ve svém profilu pro zřizování v hello vývojáře Apple Center.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-148">In your **Info.plist** file in hello solution, confirm that hello **Bundle Identifier** matches with hello **App ID** you have in your provisioning profile in hello Apple Dev Center.</span></span> 
   
    ![][7]
5. <span data-ttu-id="0d7e7-149">V hello stejné **Info.plist** souboru, ujistěte se, že jste zkontrolovali hello **povolit režimy pozadí** a **vzdáleného oznámení**.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-149">In hello same **Info.plist** file, make sure that you have checked hello **Enable Background Modes** and **Remote Notifications**.</span></span> 
   
     ![][8]
6. <span data-ttu-id="0d7e7-150">Spuštění aplikace hello na hello zařízení, kterého jste přidružili s tímto profilem publikování.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-150">Run hello app on hello device you have associated with this publishing profile.</span></span> 

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
