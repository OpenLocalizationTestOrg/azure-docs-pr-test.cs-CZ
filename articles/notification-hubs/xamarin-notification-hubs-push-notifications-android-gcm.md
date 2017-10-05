---
title: "Začínáme s centrem oznámení pro aplikace Xamarin Android | Dokumentace Microsoftu"
description: "V tomto kurzu zjistíte, jak používat Azure Notification Hubs k odesílání nabízených oznámení do aplikace Xamarin Android."
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
ms.openlocfilehash: e0ef1b006a2b202c08a71caaff4ef4d763d50d0a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-notification-hubs-with-xamarin-for-android"></a><span data-ttu-id="b6f01-103">Začínáme s centry oznámení se sadou Xamarin pro Android</span><span class="sxs-lookup"><span data-stu-id="b6f01-103">Get started with Notification Hubs with Xamarin for Android</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="b6f01-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="b6f01-104">Overview</span></span>
<span data-ttu-id="b6f01-105">V tomto kurzu zjistíte, jak používat Azure Notification Hubs k odesílání nabízených oznámení do aplikace systému Xamarin.Android.</span><span class="sxs-lookup"><span data-stu-id="b6f01-105">This tutorial shows you how to use Azure Notification Hubs to send push notifications to a Xamarin.Android application.</span></span>
<span data-ttu-id="b6f01-106">Vytvoříte prázdnou aplikaci systému Xamarin.Android, která bude přijímat nabízená oznámení pomocí služby GCM (Google Cloud Messaging).</span><span class="sxs-lookup"><span data-stu-id="b6f01-106">You'll create a blank Xamarin.Android app that receives push notifications by using Google Cloud Messaging (GCM).</span></span> <span data-ttu-id="b6f01-107">Jakmile budete hotovi, budete moci používat vaše centra oznámení k vysílání nabízených oznámení pro všechna zařízení používající vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b6f01-107">When you're finished, you'll be able to use your notification hub to broadcast push notifications to all the devices running your app.</span></span> <span data-ttu-id="b6f01-108">Dokončený kód je k dispozici v ukázce [aplikace NotificationHubs][GitHub].</span><span class="sxs-lookup"><span data-stu-id="b6f01-108">The finished code is available in the [NotificationHubs app][GitHub] sample.</span></span>

<span data-ttu-id="b6f01-109">Tento kurz představuje scénář jednoduchého vysílání přes centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="b6f01-109">This tutorial demonstrates the simple broadcast scenario in using Notification Hubs.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="b6f01-110">Než začnete</span><span class="sxs-lookup"><span data-stu-id="b6f01-110">Before you begin</span></span>
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

<span data-ttu-id="b6f01-111">Dokončený kód v tomto kurzu lze najít v části Github [zde](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid).</span><span class="sxs-lookup"><span data-stu-id="b6f01-111">The completed code for this tutorial can be found on GitHub [here](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b6f01-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b6f01-112">Prerequisites</span></span>
<span data-ttu-id="b6f01-113">V tomto kurzu budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="b6f01-113">This tutorial requires the following:</span></span>

* <span data-ttu-id="b6f01-114">Visual Studio s Xamarinem v systému Windows nebo Xamarin Studio v systému Mac OS X. Úplné pokyny k instalaci najdete v tématu [Instalační program a instalace Visual Studia a Xamarinu](https://msdn.microsoft.com/library/mt613162.aspx).</span><span class="sxs-lookup"><span data-stu-id="b6f01-114">Visual Studio with Xamarin on Windows or Xamarin Studio on Mac OS X. Complete installation instructions are on [Setup and Install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span></span>
* <span data-ttu-id="b6f01-115">Aktivní účet Google</span><span class="sxs-lookup"><span data-stu-id="b6f01-115">Active Google account</span></span>
* <span data-ttu-id="b6f01-116">[Komponenta zasílání zpráv Azure]</span><span class="sxs-lookup"><span data-stu-id="b6f01-116">[Azure Messaging Component]</span></span>
* <span data-ttu-id="b6f01-117">[Komponenta klienta zasílání zpráv cloudu Google]</span><span class="sxs-lookup"><span data-stu-id="b6f01-117">[Google Cloud Messaging Client Component]</span></span>

<span data-ttu-id="b6f01-118">Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy Notification Hubs pro Xamarin.Android Apps.</span><span class="sxs-lookup"><span data-stu-id="b6f01-118">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for Xamarin.Android apps.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b6f01-119">K dokončení tohoto kurzu potřebujete mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="b6f01-119">To complete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="b6f01-120">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="b6f01-120">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="b6f01-121">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).</span><span class="sxs-lookup"><span data-stu-id="b6f01-121">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).</span></span>
> 
> 

## <a name="enable-google-cloud-messaging"></a><span data-ttu-id="b6f01-122">Povolení služby GCM (Google Cloud Messaging)</span><span class="sxs-lookup"><span data-stu-id="b6f01-122">Enable Google Cloud Messaging</span></span>
[!INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

## <a name="configure-your-notification-hub"></a><span data-ttu-id="b6f01-123">Konfigurace centra oznámení</span><span class="sxs-lookup"><span data-stu-id="b6f01-123">Configure your notification hub</span></span>
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="7">

<li><p><span data-ttu-id="b6f01-124">Klikněte na kartu <b>Konfigurovat</b>, zadejte hodnotu <b>Klíč rozhraní API</b> získanou v předchozí části a pak klikněte na tlačítko <b>Uložit</b>.</span><span class="sxs-lookup"><span data-stu-id="b6f01-124">Click the <b>Configure</b> tab at the top, enter the <b>API Key</b> value you obtained in the previous section, and then click <b>Save</b>.</span></span></p>
</li>
</ol>
<span data-ttu-id="b6f01-125">&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)</span><span class="sxs-lookup"><span data-stu-id="b6f01-125">&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)</span></span>

<span data-ttu-id="b6f01-126">Vaše centrum oznámení je nyní nakonfigurováno pro práci se službou GCM. Zároveň máte připojovací řetězce, pomocí kterých můžete svou aplikaci zaregistrovat pro příjem oznámení a odesílání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="b6f01-126">Your notification hub is now configured to work with GCM, and you have the connection strings to both register your app to receive notifications and to send push notifications.</span></span>

## <a name="connect-your-app-to-the-notification-hub"></a><span data-ttu-id="b6f01-127">Připojte aplikaci k centru oznámení</span><span class="sxs-lookup"><span data-stu-id="b6f01-127">Connect your app to the notification hub</span></span>
### <a name="create-a-new-project"></a><span data-ttu-id="b6f01-128">Vytvoření nového projektu</span><span class="sxs-lookup"><span data-stu-id="b6f01-128">Create a new project</span></span>
1. <span data-ttu-id="b6f01-129">V Xamarin Studiu klikněte na tlačítko **Nové řešení**, klikněte na tlačítko **Aplikace pro Android** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="b6f01-129">In Xamarin Studio, click **New Solution**, click **Android App**, and click **Next**.</span></span>
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png)

2. <span data-ttu-id="b6f01-130">Zadejte **Název aplikace** a **Identifikátor**.</span><span class="sxs-lookup"><span data-stu-id="b6f01-130">Enter your **App Name** and **Identifier**.</span></span> <span data-ttu-id="b6f01-131">Klikněte na tlačítko **Cílové plaformy**, které chcete podporovat, a pak klikněte na tlačítko **Další** a **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b6f01-131">Click the **Target Plaforms** you want to support and then click **Next** and **Create**.</span></span>
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png)

    <span data-ttu-id="b6f01-132">Dojde k vytvoření nového projektu pro Android.</span><span class="sxs-lookup"><span data-stu-id="b6f01-132">This creates a new Android project.</span></span>

1. <span data-ttu-id="b6f01-133">Otevřete vlastnosti projektu kliknutím pravým tlačítkem myši na nový projekt v zobrazení Řešení a zvolte **Možnosti**.</span><span class="sxs-lookup"><span data-stu-id="b6f01-133">Open the project properties by right-clicking your new project in the Solution view and choosing **Options**.</span></span> <span data-ttu-id="b6f01-134">Vyberte položku **Aplikace pro Android** v části **Sestavení**.</span><span class="sxs-lookup"><span data-stu-id="b6f01-134">Select the **Android Application** item in the **Build** section.</span></span>
   
    <span data-ttu-id="b6f01-135">Zajistěte, aby první písmeno **Názvu balíčku** bylo malé písmeno.</span><span class="sxs-lookup"><span data-stu-id="b6f01-135">Ensure that the first letter of your **Package name** is lowercase.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="b6f01-136">První písmeno názvu balíčku musí být malé.</span><span class="sxs-lookup"><span data-stu-id="b6f01-136">The first letter of the package name must be lowercase.</span></span> <span data-ttu-id="b6f01-137">Jinak se zobrazí chyby manifestu aplikace při registraci vašeho **BroadcastReceiver** a **IntentFilter** pro nabízená oznámení níže.</span><span class="sxs-lookup"><span data-stu-id="b6f01-137">Otherwise, you will receive application manifest errors when you register your **BroadcastReceiver** and **IntentFilter** for push notifications below.</span></span>
   > 
   > 
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub--xamarin-android-app-options.png)
2. <span data-ttu-id="b6f01-138">Volitelně můžete nastavit **Minimální verzi systému Android** na jinou úroveň rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b6f01-138">Optionally, set the **Minimum Android version** to another API Level.</span></span>
3. <span data-ttu-id="b6f01-139">Volitelně můžete nastavit **Cílovou verzi systému Android** na jinou verzi rozhraní API, kterou chcete zacílit (musí se jednat o úroveň rozhraní API 8 nebo vyšší).</span><span class="sxs-lookup"><span data-stu-id="b6f01-139">Optionally, set the **Target Android version** to the another API version that you want to target (must be API level 8 or higher).</span></span>

<span data-ttu-id="b6f01-140">Klikněte na tlačítko **OK** a zavřete dialogové okno Možnosti projektu.</span><span class="sxs-lookup"><span data-stu-id="b6f01-140">Click **OK** and close the Project Options dialog.</span></span>

### <a name="add-the-required-components-to-your-project"></a><span data-ttu-id="b6f01-141">Přidejte do projektu požadované součásti</span><span class="sxs-lookup"><span data-stu-id="b6f01-141">Add the required components to your project</span></span>
<span data-ttu-id="b6f01-142">Klient služby GCM (Google Cloud Messaging) dostupný na úložišti součástí Xamarin zjednodušuje proces podpory nabízených oznámení v Xamarin.Android.</span><span class="sxs-lookup"><span data-stu-id="b6f01-142">The Google Cloud Messaging Client available on the Xamarin Component Store simplifies the process of supporting push notifications in Xamarin.Android.</span></span>

1. <span data-ttu-id="b6f01-143">Klikněte pravým tlačítkem na složku Komponenty v aplikaci Xamarin.Android a vyberte možnost **Získat více komponent**.</span><span class="sxs-lookup"><span data-stu-id="b6f01-143">Right-click the Components folder in Xamarin.Android app and choose **Get More Components**.</span></span>
2. <span data-ttu-id="b6f01-144">Vyhledejte komponentu **Zasílání zpráv Azure** a přidejte ji do projektu.</span><span class="sxs-lookup"><span data-stu-id="b6f01-144">Search for the **Azure Messaging** component and add it to the project.</span></span>
3. <span data-ttu-id="b6f01-145">Vyhledejte komponentu **Klient služby GCM (Google Cloud Messaging)** a přidejte ji do projektu.</span><span class="sxs-lookup"><span data-stu-id="b6f01-145">Search for the **Google Cloud Messaging Client** component and add it to the project.</span></span>

### <a name="set-up-notification-hubs-in-your-project"></a><span data-ttu-id="b6f01-146">Nastavte centra oznámení v projektu</span><span class="sxs-lookup"><span data-stu-id="b6f01-146">Set up notification hubs in your project</span></span>
1. <span data-ttu-id="b6f01-147">Shromážděte následující informace pro aplikaci Android a centrum oznámení:</span><span class="sxs-lookup"><span data-stu-id="b6f01-147">Gather the following information for your Android app and notification hub:</span></span>
   
   * <span data-ttu-id="b6f01-148">**GoogleProjectNumber**: tuto hodnotu čísla projektu získáte z přehledu své aplikace na portálu pro vývojáře Google.</span><span class="sxs-lookup"><span data-stu-id="b6f01-148">**GoogleProjectNumber**:  Get this Project Number value from the overview of your app on the Google Developer Portal.</span></span> <span data-ttu-id="b6f01-149">Poznámku o této hodnotě jste vytvořili dříve při vytvoření aplikace na portálu.</span><span class="sxs-lookup"><span data-stu-id="b6f01-149">You made a note of this value earlier when you created the app on the portal.</span></span>
   * <span data-ttu-id="b6f01-150">**Připojovací řetězec naslouchání**: na řídicím panelu na [portál Azure Classic] klikněte na tlačítko **Zobrazit připojovací řetězce**.</span><span class="sxs-lookup"><span data-stu-id="b6f01-150">**Listen connection string**: On the dashboard in the [Azure Classic Portal], click **View connection strings**.</span></span> <span data-ttu-id="b6f01-151">Zkopírujte připojovací řetězec *DefaultListenSharedAccessSignature* pro tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b6f01-151">Copy the *DefaultListenSharedAccessSignature* connection string for this value.</span></span>
   * <span data-ttu-id="b6f01-152">**Název centra**: Toto je název centra z [portál Azure Classic].</span><span class="sxs-lookup"><span data-stu-id="b6f01-152">**Hub name**: This is the name of your hub from the [Azure Classic Portal].</span></span> <span data-ttu-id="b6f01-153">Například *mynotificationhub2*.</span><span class="sxs-lookup"><span data-stu-id="b6f01-153">For example, *mynotificationhub2*.</span></span>
     
     <span data-ttu-id="b6f01-154">Vytvořte třídu **Constants.cs** pro váš projekt Xamarin a definujte následující hodnoty konstant ve třídě.</span><span class="sxs-lookup"><span data-stu-id="b6f01-154">Create a **Constants.cs** class for your Xamarin project and define the following constant values in the class.</span></span> <span data-ttu-id="b6f01-155">Nahraďte zástupné symboly vašimi hodnotami.</span><span class="sxs-lookup"><span data-stu-id="b6f01-155">Replace the placeholders with your values.</span></span>
     
       <span data-ttu-id="b6f01-156">public static class Constants   {</span><span class="sxs-lookup"><span data-stu-id="b6f01-156">public static class Constants   {</span></span>
     
           public const string SenderID = "<GoogleProjectNumber>"; // Google API Project Number
           public const string ListenConnectionString = "<Listen connection string>";
           public const string NotificationHubName = "<hub name>";
       <span data-ttu-id="b6f01-157">}</span><span class="sxs-lookup"><span data-stu-id="b6f01-157">}</span></span>
2. <span data-ttu-id="b6f01-158">Přidejte následující hodnoty pomocí příkazů do souboru **MainActivity.cs**:</span><span class="sxs-lookup"><span data-stu-id="b6f01-158">Add the following using statements to **MainActivity.cs**:</span></span>
   
        using Android.Util;
        using Gcm.Client;
3. <span data-ttu-id="b6f01-159">Přidejte proměnnou instance do třídy `MainActivity`, která se použije k zobrazení dialogového okna výstrah při spuštění aplikace:</span><span class="sxs-lookup"><span data-stu-id="b6f01-159">Add an instance variable to the `MainActivity` class that will be used to show an alert dialog when the app is running:</span></span>
   
        public static MainActivity instance;
4. <span data-ttu-id="b6f01-160">Vytvořte následující metodu v třídě **MainActivity**:</span><span class="sxs-lookup"><span data-stu-id="b6f01-160">Create the following method in the **MainActivity** class:</span></span>
   
        private void RegisterWithGCM()
        {
            // Check to ensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);
   
            // Register for push notifications
            Log.Info("MainActivity", "Registering...");
            GcmClient.Register(this, Constants.SenderID);
        }
5. <span data-ttu-id="b6f01-161">V metodě `OnCreate` souboru **MainActivity.cs** inicializujte proměnnou `instance` a přidejte volání do `RegisterWithGCM`:</span><span class="sxs-lookup"><span data-stu-id="b6f01-161">In the `OnCreate` method of **MainActivity.cs**, initialize the `instance` variable and add a call to `RegisterWithGCM`:</span></span>
   
        protected override void OnCreate (Bundle bundle)
        {
            instance = this;
   
            base.OnCreate (bundle);
   
            // Set your view from the "main" layout resource
            SetContentView (Resource.Layout.Main);
   
            // Get your button from the layout resource,
            // and attach an event to it
            Button button = FindViewById<Button> (Resource.Id.myButton);
   
            RegisterWithGCM();
        }
6. <span data-ttu-id="b6f01-162">Vytvořte novou třídu **MyBroadcastReceiver**.</span><span class="sxs-lookup"><span data-stu-id="b6f01-162">Create a new class, **MyBroadcastReceiver**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="b6f01-163">Projdeme s vámi vytvoření třídy **BroadcastReceiver** od začátku níže.</span><span class="sxs-lookup"><span data-stu-id="b6f01-163">We will walk through creating a **BroadcastReceiver** class from scratch below.</span></span> <span data-ttu-id="b6f01-164">Rychlou alternativou k ručnímu vytváření souboru **MyBroadcastReceiver.cs** je použít soubor **GcmService.cs** nacházející se v ukázkovém projektu Xamarin.Android, který je součástí [ukázek NotificationHubs][GitHub].</span><span class="sxs-lookup"><span data-stu-id="b6f01-164">However, a quick alternative to manually creating **MyBroadcastReceiver.cs** is to refer to the **GcmService.cs** file found in the sample Xamarin.Android project included with the [NotificationHubs samples][GitHub].</span></span> <span data-ttu-id="b6f01-165">Duplikování souboru **GcmService.cs** a změna názvů tříd může být skvělým místem, kde lze také začít.</span><span class="sxs-lookup"><span data-stu-id="b6f01-165">Duplicating **GcmService.cs** and changing class names can be a great place to start as well.</span></span>
   > 
   > 
7. <span data-ttu-id="b6f01-166">Přidejte následující možnosti pomocí příkazů do souboru **MyBroadcastReceiver.cs** (odkazující na komponentu a sestavení, které jste přidali dříve):</span><span class="sxs-lookup"><span data-stu-id="b6f01-166">Add the following using statements to **MyBroadcastReceiver.cs** (referring to the component and assembly that you added earlier):</span></span>
   
        using System.Collections.Generic;
        using System.Text;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Gcm.Client;
        using WindowsAzure.Messaging;
8. <span data-ttu-id="b6f01-167">V souboru **MyBroadcastReceiver.cs** přidejte následující žádosti oprávnění mezi příkazy **pomocí** a prohlášením **obor názvů**:</span><span class="sxs-lookup"><span data-stu-id="b6f01-167">In **MyBroadcastReceiver.cs**, add the following permission requests between the **using** statements and the **namespace** declaration:</span></span>
   
        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
   
        //GET_ACCOUNTS is needed only for Android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
9. <span data-ttu-id="b6f01-168">V souboru **MyBroadcastReceiver.cs** změňte třídu **MyBroadcastReceiver** tak, aby odpovídala následující:</span><span class="sxs-lookup"><span data-stu-id="b6f01-168">In **MyBroadcastReceiver.cs**, change the **MyBroadcastReceiver** class to match the following:</span></span>
   
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
10. <span data-ttu-id="b6f01-169">Přidejte jinou třídu do souboru **MyBroadcastReceiver.cs** s názvem **PushHandlerService**, která je odvozena z **GcmServiceBase**.</span><span class="sxs-lookup"><span data-stu-id="b6f01-169">Add another class in **MyBroadcastReceiver.cs** named **PushHandlerService**, which derives from **GcmServiceBase**.</span></span> <span data-ttu-id="b6f01-170">Nezapomeňte použít atribut **Služby** na třídu:</span><span class="sxs-lookup"><span data-stu-id="b6f01-170">Make sure to apply the **Service** attribute to the class:</span></span>
    
         [Service] // Must use the service tag
         public class PushHandlerService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }
             private NotificationHub Hub { get; set; }
    
             public PushHandlerService() : base(Constants.SenderID)
                {
                 Log.Info(MyBroadcastReceiver.TAG, "PushHandlerService() constructor");
             }
         }
11. <span data-ttu-id="b6f01-171">**GcmServiceBase** implementuje metody **OnRegistered()**, **OnUnRegistered()**, **OnMessage()**, **OnRecoverableError()** a **OnError()**.</span><span class="sxs-lookup"><span data-stu-id="b6f01-171">**GcmServiceBase** implements methods **OnRegistered()**, **OnUnRegistered()**, **OnMessage()**, **OnRecoverableError()**, and **OnError()**.</span></span> <span data-ttu-id="b6f01-172">Naše třída implementace **PushHandlerService** musí přepsat tyto metody a tyto metody se aktivují v odezvě na interakci s centrem oznámení.</span><span class="sxs-lookup"><span data-stu-id="b6f01-172">Our **PushHandlerService** implementation class must override these methods, and these methods will fire in response to interacting with the notification hub.</span></span>
12. <span data-ttu-id="b6f01-173">Potlačte metodu **OnRegistered()** v **PushHandlerService** pomocí následujícího kódu:</span><span class="sxs-lookup"><span data-stu-id="b6f01-173">Override the **OnRegistered()** method in **PushHandlerService** by using the following code:</span></span>
    
         protected override void OnRegistered(Context context, string registrationId)
         {
             Log.Verbose(MyBroadcastReceiver.TAG, "GCM Registered: " + registrationId);
             RegistrationID = registrationId;
    
             createNotification("PushHandlerService-GCM Registered...",
                                 "The device has been Registered!");
    
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
    > <span data-ttu-id="b6f01-174">V kódu **OnRegistered()** výše si všimněte schopnosti určit značky pro registraci do konkrétních kanálů zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="b6f01-174">In the **OnRegistered()** code above, you should note the ability to specify tags to register for specific messaging channels.</span></span>
    > 
    > 
13. <span data-ttu-id="b6f01-175">Potlačte metodu **OnMessage()** v **PushHandlerService** pomocí následujícího kódu:</span><span class="sxs-lookup"><span data-stu-id="b6f01-175">Override the **OnMessage** method in **PushHandlerService** by using the following code:</span></span>
    
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
14. <span data-ttu-id="b6f01-176">Přidejte následující metody **createNotification** a **dialogNotify** do **PushHandlerService** pro upozornění uživatelů při obdržení oznámení.</span><span class="sxs-lookup"><span data-stu-id="b6f01-176">Add the following **createNotification** and **dialogNotify** methods to **PushHandlerService** for notifying users when a notification is received.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="b6f01-177">Návrh oznámení v systému Android verze 5.0 a novější představuje významné odchýlení od předchozích verzí.</span><span class="sxs-lookup"><span data-stu-id="b6f01-177">Notification design in Android version 5.0 and later represents a significant departure from that of previous versions.</span></span> <span data-ttu-id="b6f01-178">Pokud toto vyzkoušíte v systému Android 5.0 nebo novějším, aplikaci bude nutné spouštět pro příjem oznámení.</span><span class="sxs-lookup"><span data-stu-id="b6f01-178">If you test this on Android 5.0 or later, the app will need to be running to receive the notification.</span></span> <span data-ttu-id="b6f01-179">Další informace naleznete v tématu [Oznámení systému Android](http://go.microsoft.com/fwlink/?LinkId=615880).</span><span class="sxs-lookup"><span data-stu-id="b6f01-179">For more information, see [Android Notifications](http://go.microsoft.com/fwlink/?LinkId=615880).</span></span>
    > 
    > 
    
        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;
    
            //Create an intent to show UI
            var uiIntent = new Intent(this, typeof(MainActivity));
    
            //Create the notification
            var notification = new Notification(Android.Resource.Drawable.SymActionEmail, title);
    
            //Auto-cancel will remove the notification once the user touches it
            notification.Flags = NotificationFlags.AutoCancel;
    
            //Set the notification info
            //we use the pending intent, passing our ui intent over, which will get called
            //when the notification is tapped.
            notification.SetLatestEventInfo(this, title, desc, PendingIntent.GetActivity(this, 0, uiIntent, 0));
    
            //Show the notification
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
15. <span data-ttu-id="b6f01-180">Potlačte abstraktní členy **OnUnRegistered()**, **OnRecoverableError()** a **OnError()** tak, aby se váš kód zkompiloval:</span><span class="sxs-lookup"><span data-stu-id="b6f01-180">Override abstract members **OnUnRegistered()**, **OnRecoverableError()**, and **OnError()** so that your code compiles:</span></span>
    
        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Unregistered: " + registrationId);
    
            createNotification("GCM Unregistered...", "The device has been unregistered!");
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

## <a name="run-your-app-in-the-emulator"></a><span data-ttu-id="b6f01-181">Spusťte aplikaci v emulátoru</span><span class="sxs-lookup"><span data-stu-id="b6f01-181">Run your app in the emulator</span></span>
<span data-ttu-id="b6f01-182">Pokud spustíte tuto aplikaci v emulátoru, ujistěte se, že používáte virtuální zařízení Android (AVD) podporující rozhraní API Google.</span><span class="sxs-lookup"><span data-stu-id="b6f01-182">If you run this app in the emulator, make sure that you use an Android Virtual Device (AVD) that supports Google APIs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b6f01-183">Aby bylo možné přijímat nabízená oznámení, musíte vytvořit účet Google na virtuálním zařízení se systémem Android.</span><span class="sxs-lookup"><span data-stu-id="b6f01-183">In order to receive push notifications, you must set up a Google account on your Android Virtual Device.</span></span> <span data-ttu-id="b6f01-184">(V emulátoru přejděte na **Nastavení** a klikněte na tlačítko **Přidat účet**.) Ujistěte se také, že je emulátor připojen k internetu.</span><span class="sxs-lookup"><span data-stu-id="b6f01-184">(In the emulator, navigate to **Settings** and click **Add Account**.) Also, make sure that the emulator is connected to the Internet.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="b6f01-185">Návrh oznámení v systému Android verze 5.0 a novější představuje významné odchýlení od předchozích verzí.</span><span class="sxs-lookup"><span data-stu-id="b6f01-185">Notification design in Android version 5.0 and later represents a significant departure from that of previous versions.</span></span> <span data-ttu-id="b6f01-186">Další informace naleznete v tématu [Oznámení systému Android](http://go.microsoft.com/fwlink/?LinkId=615880).</span><span class="sxs-lookup"><span data-stu-id="b6f01-186">For more information, see [Android Notifications](http://go.microsoft.com/fwlink/?LinkId=615880).</span></span>
> 
> 

1. <span data-ttu-id="b6f01-187">Z části **Nástroje** klikněte na tlačítko **Otevřít správce emulátoru Android**, vyberte zařízení a pak klikněte na tlačítko **Upravit**.</span><span class="sxs-lookup"><span data-stu-id="b6f01-187">From **Tools**, click **Open Android Emulator Manager**, select your device, and then click **Edit**.</span></span>
   
      ![][18]
2. <span data-ttu-id="b6f01-188">Vyberte **rozhraní API Google** v části **Cíl** a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="b6f01-188">Select **Google APIs** in **Target**, and then click **OK**.</span></span>
   
      ![][19]
3. <span data-ttu-id="b6f01-189">Na horním panelu nástrojů klikněte na tlačítko **Spustit** a pak vyberte svou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b6f01-189">On the top toolbar, click **Run**, and then select your app.</span></span> <span data-ttu-id="b6f01-190">Spustí se emulátor a pak se spustí aplikace.</span><span class="sxs-lookup"><span data-stu-id="b6f01-190">This starts the emulator and runs the app.</span></span>
   
   <span data-ttu-id="b6f01-191">Aplikace načte *registrationId* z GCM a zaregistruje se pomocí centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="b6f01-191">The app retrieves the *registrationId* from GCM and registers with the notification hub.</span></span>

## <a name="send-notifications-from-your-backend"></a><span data-ttu-id="b6f01-192">Odesílání oznámení z backendu</span><span class="sxs-lookup"><span data-stu-id="b6f01-192">Send notifications from your backend</span></span>
<span data-ttu-id="b6f01-193">Příjem oznámení ve vaší aplikaci můžete otestovat zasláním oznámení na [portál Azure Classic] prostřednictvím karty ladění v centru oznámení, jak je znázorněno na obrazovce níže.</span><span class="sxs-lookup"><span data-stu-id="b6f01-193">You can test receiving notifications in your app by sending notifications in the [Azure Classic Portal] via the debug tab on the notification hub, as shown in the screen below.</span></span>

![][30]

<span data-ttu-id="b6f01-194">Nabízená oznámení se většinou posílají ve službě backend, jako je služba Mobile Services, nebo v technologii ASP.NET pomocí kompatibilní knihovny.</span><span class="sxs-lookup"><span data-stu-id="b6f01-194">Push notifications are normally sent in a backend service like Mobile Services or ASP.NET through a compatible library.</span></span> <span data-ttu-id="b6f01-195">Můžete také použít rozhraní API REST k přímému zasílání oznámení, pokud pro vaše prostředí backend není dostupná žádná knihovna.</span><span class="sxs-lookup"><span data-stu-id="b6f01-195">You can also use the REST API directly to send notification messages if a library is not available for your backend.</span></span>

<span data-ttu-id="b6f01-196">Tady je seznam některých dalších kurzů, které se týkají zasílání oznámení:</span><span class="sxs-lookup"><span data-stu-id="b6f01-196">Here is a list of some other tutorials that you may want to review for sending notifications:</span></span>

* <span data-ttu-id="b6f01-197">ASP.NET: Viz [Použití centra oznámení pro nabízená oznámení uživatelům].</span><span class="sxs-lookup"><span data-stu-id="b6f01-197">ASP.NET: See [Use Notification Hubs to push notifications to users].</span></span>
* <span data-ttu-id="b6f01-198">Sada Azure Notification Hubs Java SDK: Informace o odesílání oznámení z Javy najdete v článku [Jak používat Notification Hubs z Javy](notification-hubs-java-push-notification-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="b6f01-198">Azure Notification Hubs Java SDK: See [How to use Notification Hubs from Java](notification-hubs-java-push-notification-tutorial.md) for sending notifications from Java.</span></span> <span data-ttu-id="b6f01-199">Tato metoda prošla pro potřeby vývoje pro Android testováním v Eclipse.</span><span class="sxs-lookup"><span data-stu-id="b6f01-199">This has been tested in Eclipse for Android Development.</span></span>
* <span data-ttu-id="b6f01-200">PHP: Viz [Jak používat Notification Hubs z PHP](notification-hubs-php-push-notification-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="b6f01-200">PHP: See [How to use Notification Hubs from PHP](notification-hubs-php-push-notification-tutorial.md).</span></span>

<span data-ttu-id="b6f01-201">V dalším pododdílu kurzu odešlete oznámení pomocí konzolové aplikace .NET a pomocí mobilních služeb prostřednictvím skriptu uzlu.</span><span class="sxs-lookup"><span data-stu-id="b6f01-201">In the next subsections of the tutorial, you send notifications by using a .NET console app, and by using a mobile service through a node script.</span></span>

#### <a name="optional-send-notifications-by-using-a-net-app"></a><span data-ttu-id="b6f01-202">(Volitelné) Odesílání oznámení pomocí aplikace .NET</span><span class="sxs-lookup"><span data-stu-id="b6f01-202">(Optional) Send notifications by using a .NET app</span></span>
<span data-ttu-id="b6f01-203">V této části si ukážeme odesílání oznámení pomocí konzolové aplikace .NET.</span><span class="sxs-lookup"><span data-stu-id="b6f01-203">In this section, we will send notifications by using a .NET console app</span></span>

1. <span data-ttu-id="b6f01-204">Vytvořte novou konzolovou aplikaci Visual C#:</span><span class="sxs-lookup"><span data-stu-id="b6f01-204">Create a new Visual C# console application:</span></span>
   
      ![][20]
2. <span data-ttu-id="b6f01-205">Ve Visual Studiu klikněte na položku **Nástroje**, klikněte na **Správce balíčků NuGet** a pak klikněte na **Konzola Správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="b6f01-205">In Visual Studio, click **Tools**, click **NuGet Package Manager**, and then click **Package Manager Console**.</span></span>
   
    <span data-ttu-id="b6f01-206">Tím se zobrazí Konzola Správce balíčků ve Visual Studiu.</span><span class="sxs-lookup"><span data-stu-id="b6f01-206">This displays the Package Manager Console in Visual Studio.</span></span>
3. <span data-ttu-id="b6f01-207">V okně konzoly Správce balíčků nastavte **Výchozí projekt** na nový projekt konzolové aplikace a pak v okně konzoly spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b6f01-207">In the Package Manager Console window, set the **Default project** to your new console application project, and then in the console window, execute the following command:</span></span>
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    <span data-ttu-id="b6f01-208">Ten přidá odkaz na sadu SDK centra oznámení Azure pomocí <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">balíčku Microsoft.Azure.Notification Hubs NuGet</a>.</span><span class="sxs-lookup"><span data-stu-id="b6f01-208">This adds a reference to the Azure Notification Hubs SDK using the <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>.</span></span>
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. <span data-ttu-id="b6f01-209">Otevřete soubor Program.cs a přidejte následující příkaz `using`:</span><span class="sxs-lookup"><span data-stu-id="b6f01-209">Open the Program.cs file and add the following `using` statement:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
5. <span data-ttu-id="b6f01-210">Do třídy `Program` přidejte následující metodu:</span><span class="sxs-lookup"><span data-stu-id="b6f01-210">In your `Program` class, add the following method.</span></span> <span data-ttu-id="b6f01-211">Aktualizujte zástupný text pomocí připojovacího řetězce *DefaultFullSharedAccessSignature* a názvu centra z [portál Azure Classic].</span><span class="sxs-lookup"><span data-stu-id="b6f01-211">Update the placeholder text with your *DefaultFullSharedAccessSignature* connection string and hub name from the [Azure Classic Portal].</span></span>
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            await hub.SendGcmNativeNotificationAsync("{ \"data\" : {\"message\":\"Hello from Azure!\"}}");
        }
6. <span data-ttu-id="b6f01-212">Přidejte následující řádky do vaší **Hlavní** metody:</span><span class="sxs-lookup"><span data-stu-id="b6f01-212">Add the following lines in your **Main** method:</span></span>
   
         SendNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="b6f01-213">Stiskněte klávesu F5 a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b6f01-213">Press the F5 key to run the app.</span></span> <span data-ttu-id="b6f01-214">Měli byste obdržet oznámení v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b6f01-214">You should receive a notification in the app.</span></span>
   
      ![][21]

#### <a name="optional-send-notifications-by-using-a-mobile-service"></a><span data-ttu-id="b6f01-215">(Volitelné) Odesílání oznámení pomocí mobilní služby</span><span class="sxs-lookup"><span data-stu-id="b6f01-215">(Optional) Send notifications by using a mobile service</span></span>
1. <span data-ttu-id="b6f01-216">Sledujte [Začínáme používat Mobile Services]</span><span class="sxs-lookup"><span data-stu-id="b6f01-216">Follow [Get started with Mobile Services].</span></span>
2. <span data-ttu-id="b6f01-217">Přihlaste se do [portál Azure Classic] a vyberte mobilní služby.</span><span class="sxs-lookup"><span data-stu-id="b6f01-217">Sign in to the [Azure Classic Portal], and select your mobile service.</span></span>
3. <span data-ttu-id="b6f01-218">Vyberte kartu **Plánovač** nahoře.</span><span class="sxs-lookup"><span data-stu-id="b6f01-218">Select the **Scheduler** tab on the top.</span></span>
   
      ![][22]
4. <span data-ttu-id="b6f01-219">Vytvořte novou naplánovanou úlohu, vložte název a vyberte **Na vyžádání**.</span><span class="sxs-lookup"><span data-stu-id="b6f01-219">Create a new scheduled job, insert a name, and select **On demand**.</span></span>
   
      ![][23]
5. <span data-ttu-id="b6f01-220">Po vytvoření úlohy klikněte na název úlohy.</span><span class="sxs-lookup"><span data-stu-id="b6f01-220">When the job is created, click the job name.</span></span> <span data-ttu-id="b6f01-221">Klikněte na kartu **Skript** v horním panelu.</span><span class="sxs-lookup"><span data-stu-id="b6f01-221">Then click the **Script** tab on the top bar.</span></span>
6. <span data-ttu-id="b6f01-222">Vložte následující skript dovnitř funkce plánovače.</span><span class="sxs-lookup"><span data-stu-id="b6f01-222">Insert the following script inside your scheduler function.</span></span> <span data-ttu-id="b6f01-223">Ujistěte se, zda jste nahradili zástupné symboly pomocí názvu centra oznámení a připojovacího řetězce pro *DefaultFullSharedAccessSignature* získaného dříve.</span><span class="sxs-lookup"><span data-stu-id="b6f01-223">Make sure to replace the placeholders with your notification hub name and the connection string for *DefaultFullSharedAccessSignature* that you obtained earlier.</span></span> <span data-ttu-id="b6f01-224">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b6f01-224">Click **Save**.</span></span>
   
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
7. <span data-ttu-id="b6f01-225">Klikněte na tlačítko **Spustit jednou** na dolním panelu.</span><span class="sxs-lookup"><span data-stu-id="b6f01-225">Click **Run Once** on the bottom bar.</span></span> <span data-ttu-id="b6f01-226">Měli byste obdržet oznámení informační zprávy.</span><span class="sxs-lookup"><span data-stu-id="b6f01-226">You should receive a toast notification.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b6f01-227">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b6f01-227">Next steps</span></span>
<span data-ttu-id="b6f01-228">V tomto jednoduchém příkladu jste vysílali oznámení pro všechna zařízení Android.</span><span class="sxs-lookup"><span data-stu-id="b6f01-228">In this simple example, you broadcasted notifications to all your Android devices.</span></span> <span data-ttu-id="b6f01-229">Chcete-li se zaměřit na konkrétní uživatele, využijte kurz [Použití centra oznámení pro nabízená oznámení uživatelům].</span><span class="sxs-lookup"><span data-stu-id="b6f01-229">In order to target specific users, refer to the tutorial [Use Notification Hubs to push notifications to users].</span></span> <span data-ttu-id="b6f01-230">Pokud chcete segmentovat uživatele podle zájmových skupin, můžete si přečíst kurz [Používání centra oznámení k odesílání novinek].</span><span class="sxs-lookup"><span data-stu-id="b6f01-230">If you want to segment your users by interest groups, you can read [Use Notification Hubs to send breaking news].</span></span> <span data-ttu-id="b6f01-231">Další informace o tom, jak používat centra oznámení, naleznete v tématu [Průvodce centry oznámení] a v tématu [Centra oznámení s postupy pro Android].</span><span class="sxs-lookup"><span data-stu-id="b6f01-231">Learn more about how to use Notification Hubs in [Notification Hubs Guidance] and in the [Notification Hubs How-To for Android].</span></span>

<!-- Anchors. -->
[Enable Google Cloud Messaging]: #register
[Configure your Notification Hub]: #configure-hub
[Connecting your app to the Notification Hub]: #connecting-app
[Run your app with the emulator]: #run-app
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
<span data-ttu-id="b6f01-232">[Začínáme používat Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service</span><span class="sxs-lookup"><span data-stu-id="b6f01-232">[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service</span></span>
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

<span data-ttu-id="b6f01-233">[portál Azure Classic]: https://manage.windowsazure.com/</span><span class="sxs-lookup"><span data-stu-id="b6f01-233">[Azure Classic Portal]: https://manage.windowsazure.com/</span></span>
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
<span data-ttu-id="b6f01-234">[Průvodce centry oznámení]: http://msdn.microsoft.com/library/jj927170.aspx</span><span class="sxs-lookup"><span data-stu-id="b6f01-234">[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx</span></span>
<span data-ttu-id="b6f01-235">[Centra oznámení s postupy pro Android]: http://msdn.microsoft.com/library/dn282661.aspx</span><span class="sxs-lookup"><span data-stu-id="b6f01-235">[Notification Hubs How-To for Android]: http://msdn.microsoft.com/library/dn282661.aspx</span></span>

<span data-ttu-id="b6f01-236">[Použití centra oznámení pro nabízená oznámení uživatelům]: /manage/services/notification-hubs/notify-users-aspnet</span><span class="sxs-lookup"><span data-stu-id="b6f01-236">[Use Notification Hubs to push notifications to users]: /manage/services/notification-hubs/notify-users-aspnet</span></span>
<span data-ttu-id="b6f01-237">[Používání centra oznámení k odesílání novinek]: /manage/services/notification-hubs/breaking-news-dotnet</span><span class="sxs-lookup"><span data-stu-id="b6f01-237">[Use Notification Hubs to send breaking news]: /manage/services/notification-hubs/breaking-news-dotnet</span></span>
[GCMClient Component page]: http://components.xamarin.com/view/GCMClient
[Xamarin.NotificationHub GitHub page]: https://github.com/SaschaDittmann/Xamarin.NotificationHub
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
<span data-ttu-id="b6f01-238">[Komponenta klienta zasílání zpráv cloudu Google]: http://components.xamarin.com/view/GCMClient/</span><span class="sxs-lookup"><span data-stu-id="b6f01-238">[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/</span></span>
<span data-ttu-id="b6f01-239">[Komponenta zasílání zpráv Azure]: http://components.xamarin.com/view/azure-messaging</span><span class="sxs-lookup"><span data-stu-id="b6f01-239">[Azure Messaging Component]: http://components.xamarin.com/view/azure-messaging</span></span>
