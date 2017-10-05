---
title: "Začínáme s Azure Mobile Engagementem pro Xamarin.iOS"
description: "Naučte se používat Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro aplikace pro Xamarin.iOS."
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
ms.openlocfilehash: 9938c3e994acf31244825b1afb347f8c9f90ebe3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-xamarinios-apps"></a><span data-ttu-id="a26c9-103">Začínáme s Azure Mobile Engagementem pro aplikace Xamarin.iOS</span><span class="sxs-lookup"><span data-stu-id="a26c9-103">Get Started with Azure Mobile Engagement for Xamarin.iOS Apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="a26c9-104">V tomto tématu si ukážeme, jak používat Azure Mobile Engagement, jak porozumět používání aplikace a jak odesílat nabízená oznámení segmentovaným uživatelům aplikace pro Xamarin.iOS.</span><span class="sxs-lookup"><span data-stu-id="a26c9-104">This topic shows you how to use Azure Mobile Engagement to understand your app usage and send push notifications to segmented users in a Xamarin.iOS application.</span></span>
<span data-ttu-id="a26c9-105">V tomto kurzu vytvoříte prázdnou aplikaci Xamarin.iOS, která bude shromažďovat základní data a přijímat nabízená oznámení pomocí systému nabízených oznámení Apple (APNS). </span><span class="sxs-lookup"><span data-stu-id="a26c9-105">In this tutorial, you create a blank Xamarin.iOS app that collects basic data and receives push notifications using Apple Push Notification System (APNS).</span></span>

> [!NOTE]
> <span data-ttu-id="a26c9-106">Službu Azure Mobile Engagement vyřadíme z provozu v březnu 2018. V současnosti je dostupná jenom pro stávající zákazníky.</span><span class="sxs-lookup"><span data-stu-id="a26c9-106">The Azure Mobile Engagement service will be retired March 2018 and is currently only available to existing customers.</span></span> <span data-ttu-id="a26c9-107">Další informace najdete v tématu [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="a26c9-107">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

<span data-ttu-id="a26c9-108">V tomto kurzu budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="a26c9-108">This tutorial requires the following:</span></span>

* <span data-ttu-id="a26c9-109">[Xamarin Studio](http://xamarin.com/studio).</span><span class="sxs-lookup"><span data-stu-id="a26c9-109">[Xamarin Studio](http://xamarin.com/studio).</span></span> <span data-ttu-id="a26c9-110">Můžete také použít Visual Studio s Xamarinem, ale v tomto kurzu používáme Xamarin Studio.</span><span class="sxs-lookup"><span data-stu-id="a26c9-110">You can also use Visual Studio with Xamarin but this tutorial uses Xamarin Studio.</span></span> <span data-ttu-id="a26c9-111">Instalační pokyny najdete v tématu o [nastavení a instalaci pro Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span><span class="sxs-lookup"><span data-stu-id="a26c9-111">For installation instructions, see [Setup and Install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span></span> 
* [<span data-ttu-id="a26c9-112">Mobile Engagement Xamarin SDK</span><span class="sxs-lookup"><span data-stu-id="a26c9-112">Mobile Engagement Xamarin SDK</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [!NOTE]
> <span data-ttu-id="a26c9-113">K dokončení tohoto kurzu potřebujete mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="a26c9-113">To complete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="a26c9-114">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="a26c9-114">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="a26c9-115">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="a26c9-115">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).</span></span>
> 
> 

## <span data-ttu-id="a26c9-116"><a id="setup-azme"></a>Nastavení Mobile Engagementu pro vaši aplikaci pro iOS</span><span class="sxs-lookup"><span data-stu-id="a26c9-116"><a id="setup-azme"></a>Setup Mobile Engagement for your iOS app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="a26c9-117"><a id="connecting-app"></a>Připojení aplikace k back-endu Mobile Engagementu</span><span class="sxs-lookup"><span data-stu-id="a26c9-117"><a id="connecting-app"></a>Connect your app to the Mobile Engagement backend</span></span>
<span data-ttu-id="a26c9-118">V tomto kurzu si představíme „základní integraci“, čili minimální sadu, která je zapotřebí pro shromažďování dat a odesílání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="a26c9-118">This tutorial presents a "basic integration" which is the minimal set required to collect data and send a push notification.</span></span>

<span data-ttu-id="a26c9-119">Pomocí Xamarinu si vytvoříme základní aplikaci, na které si tuto integraci předvedeme.</span><span class="sxs-lookup"><span data-stu-id="a26c9-119">We will create a basic app with Xamarin to demonstrate the integration:</span></span>

### <a name="create-a-new-xamarinios-project"></a><span data-ttu-id="a26c9-120">Vytvoření nového projektu Xamarin.iOS</span><span class="sxs-lookup"><span data-stu-id="a26c9-120">Create a new Xamarin.iOS project</span></span>
1. <span data-ttu-id="a26c9-121">Spusťte Xamarin Studio.</span><span class="sxs-lookup"><span data-stu-id="a26c9-121">Launch Xamarin Studio.</span></span> <span data-ttu-id="a26c9-122">Klikněte na položky **File** (Soubor)  -> **New** (Nové)  -> **Solution** (Řešení).</span><span class="sxs-lookup"><span data-stu-id="a26c9-122">Go to **File** -> **New** -> **Solution**</span></span> 
   
    ![][1]
2. <span data-ttu-id="a26c9-123">Vyberte **Single View App** (Aplikace s jedním zobrazením), zkontrolujte, zda je vybraný jazyk **C#** a klikněte na tlačítko **Next** (Další).</span><span class="sxs-lookup"><span data-stu-id="a26c9-123">Select **Single View App**, make sure the selected language is **C#** and then click **Next**.</span></span>
   
    ![][2]
3. <span data-ttu-id="a26c9-124">Vyplňte pole **App Name** (Název aplikace) a **Organization Identifier** (Identifikátor organizace) a potom klikněte na tlačítko **Next** (Další).</span><span class="sxs-lookup"><span data-stu-id="a26c9-124">Fill in the **App Name** and the **Organization Identifier** and then click **Next**.</span></span> 
   
    ![][3]
   
   > [!IMPORTANT]
   > <span data-ttu-id="a26c9-125">Ujistěte se, že profil publikování, který nakonec použijete k nasazení své aplikace pro iOS, používá ID aplikace, které přesně odpovídá vašemu identifikátoru svazku.</span><span class="sxs-lookup"><span data-stu-id="a26c9-125">Make sure that the publishing profile you eventually use to deploy your iOS app is using an App ID which matches exactly with the Bundle Identifier you have here.</span></span> 
   > 
   > 
4. <span data-ttu-id="a26c9-126">Podle potřeby aktualizujte pole **Project Name** (Název projektu), **Solution Name** (Název řešení) a **Location** (Umístění) a klikněte na **Create** (Vytvořit).</span><span class="sxs-lookup"><span data-stu-id="a26c9-126">Update the **Project Name**, **Solution Name** and **Location** if required and click **Create**.</span></span>
   
    ![][4]

<span data-ttu-id="a26c9-127">Xamarin Studio vytvoří ukázkovou aplikaci, do které budeme integrovat Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="a26c9-127">Xamarin Studio will create the demo app in which we will integrate Mobile Engagement.</span></span> 

### <a name="connect-your-app-to-mobile-engagement-backend"></a><span data-ttu-id="a26c9-128">Připojení aplikace k back-endu Mobile Engagementu</span><span class="sxs-lookup"><span data-stu-id="a26c9-128">Connect your app to Mobile Engagement backend</span></span>
1. <span data-ttu-id="a26c9-129">Pravým tlačítkem myši klikněte v oknech řešení na složku **Packages** (Balíčky) a vyberte **Add Packages...** (Přidat balíčky...).</span><span class="sxs-lookup"><span data-stu-id="a26c9-129">Right click the **Packages** folder in the Solution windows and select **Add Packages...**</span></span>
   
    ![][5]
2. <span data-ttu-id="a26c9-130">Vyhledejte **Microsoft Azure Mobile Engagement Xamarin SDK** a přidejte jej do řešení.</span><span class="sxs-lookup"><span data-stu-id="a26c9-130">Search for the **Microsoft Azure Mobile Engagement Xamarin SDK** and add it to your solution.</span></span>  
   
    ![][6]
3. <span data-ttu-id="a26c9-131">Otevřete **AppDelegate.cs** a přidejte následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="a26c9-131">Open **AppDelegate.cs** and add the following using statement:</span></span>
   
        using Microsoft.Azure.Engagement.Xamarin;
4. <span data-ttu-id="a26c9-132">V metodě **FinishedLaunching** přidejte následující, a inicializujte tak připojení s back-endem Mobile Engagementu.</span><span class="sxs-lookup"><span data-stu-id="a26c9-132">In the **FinishedLaunching** method, add the following to initialize the connection with Mobile Engagement backend.</span></span> <span data-ttu-id="a26c9-133">Nezapomeňte přidat řetězec **ConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="a26c9-133">Make sure to add your **ConnectionString**.</span></span> <span data-ttu-id="a26c9-134">Tento kód také používá fiktivní řetězec **NotificationIcon**, který přidává Mobile Engagement SDK a který možná budete chtít nahradit.</span><span class="sxs-lookup"><span data-stu-id="a26c9-134">This code also uses a dummy **NotificationIcon** which is added by the Mobile Engagement SDK which you may want to replace.</span></span> 
   
        EngagementConfiguration config = new EngagementConfiguration {
                        ConnectionString = "YourConnectionStringFromAzurePortal",
                        NotificationIcon = UIImage.FromBundle("close")
                    };
        EngagementAgent.Init (config);

## <span data-ttu-id="a26c9-135"><a id="monitor"></a>Povolení sledování v reálném čase</span><span class="sxs-lookup"><span data-stu-id="a26c9-135"><a id="monitor"></a>Enabling real-time monitoring</span></span>
<span data-ttu-id="a26c9-136">Pokud chcete začít odesílat data a zajistit, že uživatelé jsou aktivní, musíte odeslat alespoň jednu obrazovku na back-end Mobile Engagementu.</span><span class="sxs-lookup"><span data-stu-id="a26c9-136">In order to start sending data and ensuring the users are active, you must send at least one screen to the Mobile Engagement backend.</span></span>

1. <span data-ttu-id="a26c9-137">Otevřete **ViewController.cs** a přidejte následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="a26c9-137">Open **ViewController.cs** and add the following using statement:</span></span>
   
        using Microsoft.Azure.Engagement.Xamarin;
2. <span data-ttu-id="a26c9-138">Změňte třídu, ze které přebírá metoda `ViewController`, z `UIViewController` na `EngagementViewController`.</span><span class="sxs-lookup"><span data-stu-id="a26c9-138">Replace the class from which `ViewController` inherits from `UIViewController` to `EngagementViewController`.</span></span> 

## <span data-ttu-id="a26c9-139"><a id="monitor"></a>Připojení aplikace se sledováním v reálném čase</span><span class="sxs-lookup"><span data-stu-id="a26c9-139"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="a26c9-140"><a id="integrate-push"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci</span><span class="sxs-lookup"><span data-stu-id="a26c9-140"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="a26c9-141">Mobile Engagement vám umožňuje v rámci kampaní oslovit uživatele a komunikovat s nimi prostřednictvím nabízených oznámení a zpráv v aplikacích.</span><span class="sxs-lookup"><span data-stu-id="a26c9-141">Mobile Engagement allows you to interact with your users and REACH with push notifications and in-app messaging in the context of campaigns.</span></span> <span data-ttu-id="a26c9-142">Tento modul se na portálu Mobile Engagement nazývá REACH.</span><span class="sxs-lookup"><span data-stu-id="a26c9-142">This module is called REACH in the Mobile Engagement portal.</span></span>
<span data-ttu-id="a26c9-143">V následujících sekcích nastavíte aplikaci, aby tato nabízená oznámení a zprávy přijímala.</span><span class="sxs-lookup"><span data-stu-id="a26c9-143">The following sections set up your app to receive them.</span></span>

### <a name="modify-your-application-delegate"></a><span data-ttu-id="a26c9-144">Úprava delegáta aplikace</span><span class="sxs-lookup"><span data-stu-id="a26c9-144">Modify your Application Delegate</span></span>
1. <span data-ttu-id="a26c9-145">Otevřete **AppDelegate.cs** a přidejte následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="a26c9-145">Open the **AppDelegate.cs** and add the following using statement:</span></span>
   
        using System; 
2. <span data-ttu-id="a26c9-146">Nyní uvnitř metody `FinishedLaunching` přidejte následující, abyste mohli registrovat nabízená oznámení po `EngagementAgent.init(...)`</span><span class="sxs-lookup"><span data-stu-id="a26c9-146">Now inside the `FinishedLaunching` method, add the following to register for push messages after `EngagementAgent.init(...)`</span></span>
   
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
3. <span data-ttu-id="a26c9-147">Nakonec - aktualizujte nebo přidejte následující metody:</span><span class="sxs-lookup"><span data-stu-id="a26c9-147">Finally - update or add the following methods:</span></span>
   
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
            Console.WriteLine("Failed to register for remote notifications: Error '{0}'", error);
        }
4. <span data-ttu-id="a26c9-148">V souboru **Info.plist** v řešení zkontrolujte, zda se položka **Bundle Identifier** (Identifikátor svazku) shoduje s položkou **App ID** (ID aplikace), které máte v profilu zřizování v Centru pro vývojáře Apple.</span><span class="sxs-lookup"><span data-stu-id="a26c9-148">In your **Info.plist** file in the solution, confirm that the **Bundle Identifier** matches with the **App ID** you have in your provisioning profile in the Apple Dev Center.</span></span> 
   
    ![][7]
5. <span data-ttu-id="a26c9-149">Ve stejném souboru **Info.plist** nezapomeňte zaškrtnout **Enable Background Modes** (Povolit režimy pozadí) a **Remote Notifications** (Vzdálená oznámení).</span><span class="sxs-lookup"><span data-stu-id="a26c9-149">In the same **Info.plist** file, make sure that you have checked the **Enable Background Modes** and **Remote Notifications**.</span></span> 
   
     ![][8]
6. <span data-ttu-id="a26c9-150">Spusťte aplikaci v zařízení, které je propojené s tímto profilem publikování.</span><span class="sxs-lookup"><span data-stu-id="a26c9-150">Run the app on the device you have associated with this publishing profile.</span></span> 

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
