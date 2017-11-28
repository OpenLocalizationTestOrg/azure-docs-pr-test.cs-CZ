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
# <a name="get-started-with-notification-hubs-with-xamarin-for-android"></a><span data-ttu-id="3bbd0-103">Začínáme s centry oznámení se sadou Xamarin pro Android</span><span class="sxs-lookup"><span data-stu-id="3bbd0-103">Get started with Notification Hubs with Xamarin for Android</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="3bbd0-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="3bbd0-104">Overview</span></span>
<span data-ttu-id="3bbd0-105">Tento kurz ukazuje, jak Azure Notification Hubs toosend toouse nabízená oznámení tooa Xamarin.Android aplikace.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-105">This tutorial shows you how toouse Azure Notification Hubs toosend push notifications tooa Xamarin.Android application.</span></span>
<span data-ttu-id="3bbd0-106">Vytvoříte prázdnou aplikaci systému Xamarin.Android, která bude přijímat nabízená oznámení pomocí služby GCM (Google Cloud Messaging).</span><span class="sxs-lookup"><span data-stu-id="3bbd0-106">You'll create a blank Xamarin.Android app that receives push notifications by using Google Cloud Messaging (GCM).</span></span> <span data-ttu-id="3bbd0-107">Jakmile budete hotovi, budete moct toouse vaše toobroadcast centra oznámení nabízená oznámení tooall hello zařízení používající vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-107">When you're finished, you'll be able toouse your notification hub toobroadcast push notifications tooall hello devices running your app.</span></span> <span data-ttu-id="3bbd0-108">Hello Dokončený kód je k dispozici v hello [aplikace NotificationHubs] [ GitHub] ukázka.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-108">hello finished code is available in hello [NotificationHubs app][GitHub] sample.</span></span>

<span data-ttu-id="3bbd0-109">Tento kurz představuje scénář jednoduchého vysílání hello přes centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-109">This tutorial demonstrates hello simple broadcast scenario in using Notification Hubs.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="3bbd0-110">Než začnete</span><span class="sxs-lookup"><span data-stu-id="3bbd0-110">Before you begin</span></span>
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

<span data-ttu-id="3bbd0-111">Hello dokončit kód v tomto kurzu lze nalézt na Githubu [zde](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid).</span><span class="sxs-lookup"><span data-stu-id="3bbd0-111">hello completed code for this tutorial can be found on GitHub [here](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3bbd0-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3bbd0-112">Prerequisites</span></span>
<span data-ttu-id="3bbd0-113">Tento kurz vyžaduje hello následující:</span><span class="sxs-lookup"><span data-stu-id="3bbd0-113">This tutorial requires hello following:</span></span>

* <span data-ttu-id="3bbd0-114">Visual Studio s Xamarinem v systému Windows nebo Xamarin Studio v systému Mac OS X. Úplné pokyny k instalaci najdete v tématu [Instalační program a instalace Visual Studia a Xamarinu](https://msdn.microsoft.com/library/mt613162.aspx).</span><span class="sxs-lookup"><span data-stu-id="3bbd0-114">Visual Studio with Xamarin on Windows or Xamarin Studio on Mac OS X. Complete installation instructions are on [Setup and Install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span></span>
* <span data-ttu-id="3bbd0-115">Aktivní účet Google</span><span class="sxs-lookup"><span data-stu-id="3bbd0-115">Active Google account</span></span>
* <span data-ttu-id="3bbd0-116">[Komponenta zasílání zpráv Azure]</span><span class="sxs-lookup"><span data-stu-id="3bbd0-116">[Azure Messaging Component]</span></span>
* <span data-ttu-id="3bbd0-117">[Komponenta klienta zasílání zpráv cloudu Google]</span><span class="sxs-lookup"><span data-stu-id="3bbd0-117">[Google Cloud Messaging Client Component]</span></span>

<span data-ttu-id="3bbd0-118">Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy Notification Hubs pro Xamarin.Android Apps.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-118">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for Xamarin.Android apps.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3bbd0-119">toocomplete tento kurz, musíte mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-119">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="3bbd0-120">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-120">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="3bbd0-121">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).</span><span class="sxs-lookup"><span data-stu-id="3bbd0-121">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).</span></span>
> 
> 

## <a name="enable-google-cloud-messaging"></a><span data-ttu-id="3bbd0-122">Povolení služby GCM (Google Cloud Messaging)</span><span class="sxs-lookup"><span data-stu-id="3bbd0-122">Enable Google Cloud Messaging</span></span>
[!INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

## <a name="configure-your-notification-hub"></a><span data-ttu-id="3bbd0-123">Konfigurace centra oznámení</span><span class="sxs-lookup"><span data-stu-id="3bbd0-123">Configure your notification hub</span></span>
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="7">

<li><p><span data-ttu-id="3bbd0-124">Klikněte na tlačítko hello <b>konfigurace</b> hello horní, zadejte hello <b>klíč rozhraní API</b> hodnotou, kterou jste získali v předchozí části hello a pak klikněte na <b>Uložit</b>.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-124">Click hello <b>Configure</b> tab at hello top, enter hello <b>API Key</b> value you obtained in hello previous section, and then click <b>Save</b>.</span></span></p>
</li>
</ol>
<span data-ttu-id="3bbd0-125">&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)</span><span class="sxs-lookup"><span data-stu-id="3bbd0-125">&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)</span></span>

<span data-ttu-id="3bbd0-126">Vaše centrum oznámení je teď nakonfigurovaná toowork s GCM a máte hello připojovací řetězce tooboth zaregistrovat aplikaci tooreceive oznámení a toosend nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-126">Your notification hub is now configured toowork with GCM, and you have hello connection strings tooboth register your app tooreceive notifications and toosend push notifications.</span></span>

## <a name="connect-your-app-toohello-notification-hub"></a><span data-ttu-id="3bbd0-127">Připojit vaše Centrum oznámení toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="3bbd0-127">Connect your app toohello notification hub</span></span>
### <a name="create-a-new-project"></a><span data-ttu-id="3bbd0-128">Vytvoření nového projektu</span><span class="sxs-lookup"><span data-stu-id="3bbd0-128">Create a new project</span></span>
1. <span data-ttu-id="3bbd0-129">V Xamarin Studiu klikněte na tlačítko **Nové řešení**, klikněte na tlačítko **Aplikace pro Android** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-129">In Xamarin Studio, click **New Solution**, click **Android App**, and click **Next**.</span></span>
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png)

2. <span data-ttu-id="3bbd0-130">Zadejte **Název aplikace** a **Identifikátor**.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-130">Enter your **App Name** and **Identifier**.</span></span> <span data-ttu-id="3bbd0-131">Klikněte na tlačítko hello **cílové plaformy** toosupport a pak klikněte na **Další** a **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-131">Click hello **Target Plaforms** you want toosupport and then click **Next** and **Create**.</span></span>
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png)

    <span data-ttu-id="3bbd0-132">Dojde k vytvoření nového projektu pro Android.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-132">This creates a new Android project.</span></span>

1. <span data-ttu-id="3bbd0-133">Otevřete vlastnosti projektu hello tak, že kliknete pravým tlačítkem na nový projekt v hello zobrazení řešení a zvolte **možnosti**.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-133">Open hello project properties by right-clicking your new project in hello Solution view and choosing **Options**.</span></span> <span data-ttu-id="3bbd0-134">Vyberte hello **aplikace pro Android** položky v hello **sestavení** části.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-134">Select hello **Android Application** item in hello **Build** section.</span></span>
   
    <span data-ttu-id="3bbd0-135">Ujistěte se, že hello první písmeno vaše **název balíčku** je malá písmena.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-135">Ensure that hello first letter of your **Package name** is lowercase.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="3bbd0-136">první písmeno názvu balíčku hello Hello musí být malé.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-136">hello first letter of hello package name must be lowercase.</span></span> <span data-ttu-id="3bbd0-137">Jinak se zobrazí chyby manifestu aplikace při registraci vašeho **BroadcastReceiver** a **IntentFilter** pro nabízená oznámení níže.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-137">Otherwise, you will receive application manifest errors when you register your **BroadcastReceiver** and **IntentFilter** for push notifications below.</span></span>
   > 
   > 
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub--xamarin-android-app-options.png)
2. <span data-ttu-id="3bbd0-138">Volitelně můžete sadu hello **minimální verzi systému Android** tooanother úroveň rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-138">Optionally, set hello **Minimum Android version** tooanother API Level.</span></span>
3. <span data-ttu-id="3bbd0-139">Volitelně můžete sadu hello **cílovou verzi systému Android** toohello jinou verzi rozhraní API, které chcete tootarget (musí být úroveň rozhraní API 8 nebo vyšší).</span><span class="sxs-lookup"><span data-stu-id="3bbd0-139">Optionally, set hello **Target Android version** toohello another API version that you want tootarget (must be API level 8 or higher).</span></span>

<span data-ttu-id="3bbd0-140">Klikněte na tlačítko **OK** a dialogové okno Možnosti projektu zavřít hello.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-140">Click **OK** and close hello Project Options dialog.</span></span>

### <a name="add-hello-required-components-tooyour-project"></a><span data-ttu-id="3bbd0-141">Přidání hello požadované součásti tooyour projektu</span><span class="sxs-lookup"><span data-stu-id="3bbd0-141">Add hello required components tooyour project</span></span>
<span data-ttu-id="3bbd0-142">Hello Google Cloud zasílání zpráv klienta k dispozici na hello úložišti součástí Xamarin zjednodušuje proces podpory nabízených oznámení v Xamarin.Android hello.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-142">hello Google Cloud Messaging Client available on hello Xamarin Component Store simplifies hello process of supporting push notifications in Xamarin.Android.</span></span>

1. <span data-ttu-id="3bbd0-143">Klikněte pravým tlačítkem na složku komponenty hello v aplikaci Xamarin.Android a vyberte **získat více komponent**.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-143">Right-click hello Components folder in Xamarin.Android app and choose **Get More Components**.</span></span>
2. <span data-ttu-id="3bbd0-144">Vyhledejte hello **zasílání zpráv Azure** součástí a přidejte ji toohello projektu.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-144">Search for hello **Azure Messaging** component and add it toohello project.</span></span>
3. <span data-ttu-id="3bbd0-145">Vyhledejte hello **klient Google Cloud Messaging** součástí a přidejte ji toohello projektu.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-145">Search for hello **Google Cloud Messaging Client** component and add it toohello project.</span></span>

### <a name="set-up-notification-hubs-in-your-project"></a><span data-ttu-id="3bbd0-146">Nastavte centra oznámení v projektu</span><span class="sxs-lookup"><span data-stu-id="3bbd0-146">Set up notification hubs in your project</span></span>
1. <span data-ttu-id="3bbd0-147">Shromážděte následující informace pro vaše aplikace a oznámení Android Centrum hello:</span><span class="sxs-lookup"><span data-stu-id="3bbd0-147">Gather hello following information for your Android app and notification hub:</span></span>
   
   * <span data-ttu-id="3bbd0-148">**GoogleProjectNumber**: získat tuto hodnotu čísla projektu z hello přehledu své aplikace na hello portál pro vývojáře Google.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-148">**GoogleProjectNumber**:  Get this Project Number value from hello overview of your app on hello Google Developer Portal.</span></span> <span data-ttu-id="3bbd0-149">Jste si poznamenali tuto hodnotu dříve při vytvoření aplikace hello na portálu hello.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-149">You made a note of this value earlier when you created hello app on hello portal.</span></span>
   * <span data-ttu-id="3bbd0-150">**Připojovací řetězec naslouchání**: na řídicím panelu hello v hello [portálu Azure Classic], klikněte na tlačítko **zobrazit připojovací řetězce**.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-150">**Listen connection string**: On hello dashboard in hello [Azure Classic Portal], click **View connection strings**.</span></span> <span data-ttu-id="3bbd0-151">Kopírování hello *DefaultListenSharedAccessSignature* připojovací řetězec pro tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-151">Copy hello *DefaultListenSharedAccessSignature* connection string for this value.</span></span>
   * <span data-ttu-id="3bbd0-152">**Název centra**: Toto je název centra z hello hello [portálu Azure Classic].</span><span class="sxs-lookup"><span data-stu-id="3bbd0-152">**Hub name**: This is hello name of your hub from hello [Azure Classic Portal].</span></span> <span data-ttu-id="3bbd0-153">Například *mynotificationhub2*.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-153">For example, *mynotificationhub2*.</span></span>
     
     <span data-ttu-id="3bbd0-154">Vytvoření **Constants.cs** třídy pro váš projekt Xamarin a definujte následující hodnoty konstant ve třídě hello hello.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-154">Create a **Constants.cs** class for your Xamarin project and define hello following constant values in hello class.</span></span> <span data-ttu-id="3bbd0-155">Nahraďte zástupné symboly hello svoje hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-155">Replace hello placeholders with your values.</span></span>
     
       <span data-ttu-id="3bbd0-156">public static class Constants   {</span><span class="sxs-lookup"><span data-stu-id="3bbd0-156">public static class Constants   {</span></span>
     
           public const string SenderID = "<GoogleProjectNumber>"; // Google API Project Number
           public const string ListenConnectionString = "<Listen connection string>";
           public const string NotificationHubName = "<hub name>";
       <span data-ttu-id="3bbd0-157">}</span><span class="sxs-lookup"><span data-stu-id="3bbd0-157">}</span></span>
2. <span data-ttu-id="3bbd0-158">Přidat hello následující příkazy using příliš**MainActivity.cs**:</span><span class="sxs-lookup"><span data-stu-id="3bbd0-158">Add hello following using statements too**MainActivity.cs**:</span></span>
   
        using Android.Util;
        using Gcm.Client;
3. <span data-ttu-id="3bbd0-159">Přidat proměnné toohello instance `MainActivity` třídu, která bude použité tooshow dialogového okna výstrah při spuštěné aplikaci hello:</span><span class="sxs-lookup"><span data-stu-id="3bbd0-159">Add an instance variable toohello `MainActivity` class that will be used tooshow an alert dialog when hello app is running:</span></span>
   
        public static MainActivity instance;
4. <span data-ttu-id="3bbd0-160">Vytvořte následující metodu v hello hello **MainActivity** třídy:</span><span class="sxs-lookup"><span data-stu-id="3bbd0-160">Create hello following method in hello **MainActivity** class:</span></span>
   
        private void RegisterWithGCM()
        {
            // Check tooensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);
   
            // Register for push notifications
            Log.Info("MainActivity", "Registering...");
            GcmClient.Register(this, Constants.SenderID);
        }
5. <span data-ttu-id="3bbd0-161">V hello `OnCreate` metodu **MainActivity.cs**, inicializovat hello `instance` proměnnou a přidejte volání příliš`RegisterWithGCM`:</span><span class="sxs-lookup"><span data-stu-id="3bbd0-161">In hello `OnCreate` method of **MainActivity.cs**, initialize hello `instance` variable and add a call too`RegisterWithGCM`:</span></span>
   
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
6. <span data-ttu-id="3bbd0-162">Vytvořte novou třídu **MyBroadcastReceiver**.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-162">Create a new class, **MyBroadcastReceiver**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="3bbd0-163">Projdeme s vámi vytvoření třídy **BroadcastReceiver** od začátku níže.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-163">We will walk through creating a **BroadcastReceiver** class from scratch below.</span></span> <span data-ttu-id="3bbd0-164">Ale rychlé alternativní toomanually vytváření **MyBroadcastReceiver.cs** je toorefer toohello **GcmService.cs** nacházející se v projektu vzorku Xamarin.Android hello součástí hello [Vzorků NotificationHubs][GitHub].</span><span class="sxs-lookup"><span data-stu-id="3bbd0-164">However, a quick alternative toomanually creating **MyBroadcastReceiver.cs** is toorefer toohello **GcmService.cs** file found in hello sample Xamarin.Android project included with hello [NotificationHubs samples][GitHub].</span></span> <span data-ttu-id="3bbd0-165">Duplikování **GcmService.cs** a změna názvů tříd může být také toostart skvělým místem.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-165">Duplicating **GcmService.cs** and changing class names can be a great place toostart as well.</span></span>
   > 
   > 
7. <span data-ttu-id="3bbd0-166">Přidat hello následující příkazy using příliš**MyBroadcastReceiver.cs** (odkazující toohello komponentu a sestavení, které jste přidali dříve):</span><span class="sxs-lookup"><span data-stu-id="3bbd0-166">Add hello following using statements too**MyBroadcastReceiver.cs** (referring toohello component and assembly that you added earlier):</span></span>
   
        using System.Collections.Generic;
        using System.Text;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Gcm.Client;
        using WindowsAzure.Messaging;
8. <span data-ttu-id="3bbd0-167">V **MyBroadcastReceiver.cs**, přidejte následující žádosti oprávnění mezi hello hello **pomocí** příkazů a hello **obor názvů** deklarace:</span><span class="sxs-lookup"><span data-stu-id="3bbd0-167">In **MyBroadcastReceiver.cs**, add hello following permission requests between hello **using** statements and hello **namespace** declaration:</span></span>
   
        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
   
        //GET_ACCOUNTS is needed only for Android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
9. <span data-ttu-id="3bbd0-168">V **MyBroadcastReceiver.cs**, změňte hello **MyBroadcastReceiver** toomatch hello následující třídy:</span><span class="sxs-lookup"><span data-stu-id="3bbd0-168">In **MyBroadcastReceiver.cs**, change hello **MyBroadcastReceiver** class toomatch hello following:</span></span>
   
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
10. <span data-ttu-id="3bbd0-169">Přidejte jinou třídu do souboru **MyBroadcastReceiver.cs** s názvem **PushHandlerService**, která je odvozena z **GcmServiceBase**.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-169">Add another class in **MyBroadcastReceiver.cs** named **PushHandlerService**, which derives from **GcmServiceBase**.</span></span> <span data-ttu-id="3bbd0-170">Ujistěte se, zda text hello tooapply **služby** toohello třídy atributů:</span><span class="sxs-lookup"><span data-stu-id="3bbd0-170">Make sure tooapply hello **Service** attribute toohello class:</span></span>
    
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
11. <span data-ttu-id="3bbd0-171">**GcmServiceBase** implementuje metody **OnRegistered()**, **OnUnRegistered()**, **OnMessage()**, **OnRecoverableError()** a **OnError()**.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-171">**GcmServiceBase** implements methods **OnRegistered()**, **OnUnRegistered()**, **OnMessage()**, **OnRecoverableError()**, and **OnError()**.</span></span> <span data-ttu-id="3bbd0-172">Naše **PushHandlerService** třída implementace musí přepsat tyto metody a tyto metody se aktivují v odpovědi toointeracting hello centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-172">Our **PushHandlerService** implementation class must override these methods, and these methods will fire in response toointeracting with hello notification hub.</span></span>
12. <span data-ttu-id="3bbd0-173">Přepsání hello **OnRegistered()** metoda v **PushHandlerService** pomocí hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="3bbd0-173">Override hello **OnRegistered()** method in **PushHandlerService** by using hello following code:</span></span>
    
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
    > <span data-ttu-id="3bbd0-174">V hello **OnRegistered()** kódu výše, nezapomeňte hello možnost toospecify značky tooregister konkrétních kanálů zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-174">In hello **OnRegistered()** code above, you should note hello ability toospecify tags tooregister for specific messaging channels.</span></span>
    > 
    > 
13. <span data-ttu-id="3bbd0-175">Přepsání hello **OnMessage** metoda v **PushHandlerService** pomocí hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="3bbd0-175">Override hello **OnMessage** method in **PushHandlerService** by using hello following code:</span></span>
    
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
14. <span data-ttu-id="3bbd0-176">Přidejte následující hello **createNotification** a **dialogNotify** metody příliš**PushHandlerService** pro upozornění uživatelů při obdržení oznámení.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-176">Add hello following **createNotification** and **dialogNotify** methods too**PushHandlerService** for notifying users when a notification is received.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="3bbd0-177">Návrh oznámení v systému Android verze 5.0 a novější představuje významné odchýlení od předchozích verzí.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-177">Notification design in Android version 5.0 and later represents a significant departure from that of previous versions.</span></span> <span data-ttu-id="3bbd0-178">Pokud testujete to v systému Android 5.0 nebo novější, bude nutné aplikace hello toobe spuštěny tooreceive hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-178">If you test this on Android 5.0 or later, hello app will need toobe running tooreceive hello notification.</span></span> <span data-ttu-id="3bbd0-179">Další informace naleznete v tématu [Oznámení systému Android](http://go.microsoft.com/fwlink/?LinkId=615880).</span><span class="sxs-lookup"><span data-stu-id="3bbd0-179">For more information, see [Android Notifications](http://go.microsoft.com/fwlink/?LinkId=615880).</span></span>
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
15. <span data-ttu-id="3bbd0-180">Potlačte abstraktní členy **OnUnRegistered()**, **OnRecoverableError()** a **OnError()** tak, aby se váš kód zkompiloval:</span><span class="sxs-lookup"><span data-stu-id="3bbd0-180">Override abstract members **OnUnRegistered()**, **OnRecoverableError()**, and **OnError()** so that your code compiles:</span></span>
    
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

## <a name="run-your-app-in-hello-emulator"></a><span data-ttu-id="3bbd0-181">Spusťte aplikaci v emulátoru hello</span><span class="sxs-lookup"><span data-stu-id="3bbd0-181">Run your app in hello emulator</span></span>
<span data-ttu-id="3bbd0-182">Pokud spustíte tuto aplikaci v emulátoru hello, ujistěte se, že používáte virtuální zařízení Android (AVD) podporující rozhraní API Google.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-182">If you run this app in hello emulator, make sure that you use an Android Virtual Device (AVD) that supports Google APIs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3bbd0-183">V pořadí tooreceive nabízená oznámení musíte nastavit účet Google na virtuální zařízení se systémem Android.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-183">In order tooreceive push notifications, you must set up a Google account on your Android Virtual Device.</span></span> <span data-ttu-id="3bbd0-184">(V emulátoru hello přejděte příliš**nastavení** a klikněte na tlačítko **přidat účet**.) Taky se ujistěte, že hello emulátor je připojený toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-184">(In hello emulator, navigate too**Settings** and click **Add Account**.) Also, make sure that hello emulator is connected toohello Internet.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="3bbd0-185">Návrh oznámení v systému Android verze 5.0 a novější představuje významné odchýlení od předchozích verzí.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-185">Notification design in Android version 5.0 and later represents a significant departure from that of previous versions.</span></span> <span data-ttu-id="3bbd0-186">Další informace naleznete v tématu [Oznámení systému Android](http://go.microsoft.com/fwlink/?LinkId=615880).</span><span class="sxs-lookup"><span data-stu-id="3bbd0-186">For more information, see [Android Notifications](http://go.microsoft.com/fwlink/?LinkId=615880).</span></span>
> 
> 

1. <span data-ttu-id="3bbd0-187">Z části **Nástroje** klikněte na tlačítko **Otevřít správce emulátoru Android**, vyberte zařízení a pak klikněte na tlačítko **Upravit**.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-187">From **Tools**, click **Open Android Emulator Manager**, select your device, and then click **Edit**.</span></span>
   
      ![][18]
2. <span data-ttu-id="3bbd0-188">Vyberte **rozhraní API Google** v části **Cíl** a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-188">Select **Google APIs** in **Target**, and then click **OK**.</span></span>
   
      ![][19]
3. <span data-ttu-id="3bbd0-189">Na horním panelu nástrojů hello, klikněte na tlačítko **spustit**a pak vyberte svou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-189">On hello top toolbar, click **Run**, and then select your app.</span></span> <span data-ttu-id="3bbd0-190">To spustí hello emulátor a spustí aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-190">This starts hello emulator and runs hello app.</span></span>
   
   <span data-ttu-id="3bbd0-191">aplikace Hello načte hello *registrationId* z GCM a zaregistruje se pomocí centra oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-191">hello app retrieves hello *registrationId* from GCM and registers with hello notification hub.</span></span>

## <a name="send-notifications-from-your-backend"></a><span data-ttu-id="3bbd0-192">Odesílání oznámení z backendu</span><span class="sxs-lookup"><span data-stu-id="3bbd0-192">Send notifications from your backend</span></span>
<span data-ttu-id="3bbd0-193">Příjem oznámení ve vaší aplikaci odesláním oznámení hello můžete otestovat [portálu Azure Classic] prostřednictvím hello ladění karty v centru oznámení hello, jak je znázorněno níže úvodní obrazovka.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-193">You can test receiving notifications in your app by sending notifications in hello [Azure Classic Portal] via hello debug tab on hello notification hub, as shown in hello screen below.</span></span>

![][30]

<span data-ttu-id="3bbd0-194">Nabízená oznámení se většinou posílají ve službě backend, jako je služba Mobile Services, nebo v technologii ASP.NET pomocí kompatibilní knihovny.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-194">Push notifications are normally sent in a backend service like Mobile Services or ASP.NET through a compatible library.</span></span> <span data-ttu-id="3bbd0-195">Můžete taky hello REST API přímo toosend oznámení zpráv, pokud není žádná knihovna k dispozici pro váš back-end.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-195">You can also use hello REST API directly toosend notification messages if a library is not available for your backend.</span></span>

<span data-ttu-id="3bbd0-196">Tady je seznam některých dalších kurzů, které bude pravděpodobně třeba tooreview pro zasílání oznámení:</span><span class="sxs-lookup"><span data-stu-id="3bbd0-196">Here is a list of some other tutorials that you may want tooreview for sending notifications:</span></span>

* <span data-ttu-id="3bbd0-197">ASP.NET: Viz [použití centra oznámení toopush oznámení toousers].</span><span class="sxs-lookup"><span data-stu-id="3bbd0-197">ASP.NET: See [Use Notification Hubs toopush notifications toousers].</span></span>
* <span data-ttu-id="3bbd0-198">Azure Notification Hubs Java SDK: najdete v části [jak toouse centra oznámení z Java](notification-hubs-java-push-notification-tutorial.md) pro odesílání oznámení z Java.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-198">Azure Notification Hubs Java SDK: See [How toouse Notification Hubs from Java](notification-hubs-java-push-notification-tutorial.md) for sending notifications from Java.</span></span> <span data-ttu-id="3bbd0-199">Tato metoda prošla pro potřeby vývoje pro Android testováním v Eclipse.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-199">This has been tested in Eclipse for Android Development.</span></span>
* <span data-ttu-id="3bbd0-200">PHP: Viz [jak toouse Notification Hubs z PHP](notification-hubs-php-push-notification-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="3bbd0-200">PHP: See [How toouse Notification Hubs from PHP](notification-hubs-php-push-notification-tutorial.md).</span></span>

<span data-ttu-id="3bbd0-201">V dalším pododdílu hello hello kurzu odesílání oznámení pomocí konzolové aplikace .NET a pomocí mobilních služeb prostřednictvím skriptu uzlu.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-201">In hello next subsections of hello tutorial, you send notifications by using a .NET console app, and by using a mobile service through a node script.</span></span>

#### <a name="optional-send-notifications-by-using-a-net-app"></a><span data-ttu-id="3bbd0-202">(Volitelné) Odesílání oznámení pomocí aplikace .NET</span><span class="sxs-lookup"><span data-stu-id="3bbd0-202">(Optional) Send notifications by using a .NET app</span></span>
<span data-ttu-id="3bbd0-203">V této části si ukážeme odesílání oznámení pomocí konzolové aplikace .NET.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-203">In this section, we will send notifications by using a .NET console app</span></span>

1. <span data-ttu-id="3bbd0-204">Vytvořte novou konzolovou aplikaci Visual C#:</span><span class="sxs-lookup"><span data-stu-id="3bbd0-204">Create a new Visual C# console application:</span></span>
   
      ![][20]
2. <span data-ttu-id="3bbd0-205">Ve Visual Studiu klikněte na položku **Nástroje**, klikněte na **Správce balíčků NuGet** a pak klikněte na **Konzola Správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-205">In Visual Studio, click **Tools**, click **NuGet Package Manager**, and then click **Package Manager Console**.</span></span>
   
    <span data-ttu-id="3bbd0-206">V sadě Visual Studio se zobrazí hello Konzola správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-206">This displays hello Package Manager Console in Visual Studio.</span></span>
3. <span data-ttu-id="3bbd0-207">V okně konzoly Správce balíčků hello, nastavte hello **výchozí projekt** tooyour nové konzoly projekt aplikace a potom v okně konzoly hello, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="3bbd0-207">In hello Package Manager Console window, set hello **Default project** tooyour new console application project, and then in hello console window, execute hello following command:</span></span>
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    <span data-ttu-id="3bbd0-208">Tento postup přidá odkaz toohello SDK centra oznámení Azure pomocí hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">balíčku Microsoft.Azure.Notification Hubs NuGet</a>.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-208">This adds a reference toohello Azure Notification Hubs SDK using hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>.</span></span>
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. <span data-ttu-id="3bbd0-209">Otevřete soubor Program.cs hello a přidejte následující hello `using` příkaz:</span><span class="sxs-lookup"><span data-stu-id="3bbd0-209">Open hello Program.cs file and add hello following `using` statement:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
5. <span data-ttu-id="3bbd0-210">Ve vašem `Program` třídy, přidejte následující metodu hello.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-210">In your `Program` class, add hello following method.</span></span> <span data-ttu-id="3bbd0-211">Aktualizujte zástupný text hello s vaší *DefaultFullSharedAccessSignature* připojovací řetězec a názvu centra z hello [portálu Azure Classic].</span><span class="sxs-lookup"><span data-stu-id="3bbd0-211">Update hello placeholder text with your *DefaultFullSharedAccessSignature* connection string and hub name from hello [Azure Classic Portal].</span></span>
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            await hub.SendGcmNativeNotificationAsync("{ \"data\" : {\"message\":\"Hello from Azure!\"}}");
        }
6. <span data-ttu-id="3bbd0-212">Přidejte následující řádky do hello vaše **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="3bbd0-212">Add hello following lines in your **Main** method:</span></span>
   
         SendNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="3bbd0-213">Stisknutím klávesy hello F5 klíče toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-213">Press hello F5 key toorun hello app.</span></span> <span data-ttu-id="3bbd0-214">Měli byste obdržet oznámení v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-214">You should receive a notification in hello app.</span></span>
   
      ![][21]

#### <a name="optional-send-notifications-by-using-a-mobile-service"></a><span data-ttu-id="3bbd0-215">(Volitelné) Odesílání oznámení pomocí mobilní služby</span><span class="sxs-lookup"><span data-stu-id="3bbd0-215">(Optional) Send notifications by using a mobile service</span></span>
1. <span data-ttu-id="3bbd0-216">Sledujte [Začínáme používat Mobile Services]</span><span class="sxs-lookup"><span data-stu-id="3bbd0-216">Follow [Get started with Mobile Services].</span></span>
2. <span data-ttu-id="3bbd0-217">Přihlaste se toohello [portálu Azure Classic]a vyberte mobilní služby.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-217">Sign in toohello [Azure Classic Portal], and select your mobile service.</span></span>
3. <span data-ttu-id="3bbd0-218">Vyberte hello **Plánovač** karty v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-218">Select hello **Scheduler** tab on hello top.</span></span>
   
      ![][22]
4. <span data-ttu-id="3bbd0-219">Vytvořte novou naplánovanou úlohu, vložte název a vyberte **Na vyžádání**.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-219">Create a new scheduled job, insert a name, and select **On demand**.</span></span>
   
      ![][23]
5. <span data-ttu-id="3bbd0-220">Když se vytvoří úloha hello, klikněte na název úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-220">When hello job is created, click hello job name.</span></span> <span data-ttu-id="3bbd0-221">Pak klikněte na tlačítko hello **skriptu** karty na horním panelu hello.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-221">Then click hello **Script** tab on hello top bar.</span></span>
6. <span data-ttu-id="3bbd0-222">Vložte následující skript dovnitř funkce plánovače hello.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-222">Insert hello following script inside your scheduler function.</span></span> <span data-ttu-id="3bbd0-223">Ujistěte se, že tooreplace zástupné symboly hello oznámení centra název a hello připojovacím řetězcem pro *DefaultFullSharedAccessSignature* kterou jste získali dříve.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-223">Make sure tooreplace hello placeholders with your notification hub name and hello connection string for *DefaultFullSharedAccessSignature* that you obtained earlier.</span></span> <span data-ttu-id="3bbd0-224">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-224">Click **Save**.</span></span>
   
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
7. <span data-ttu-id="3bbd0-225">Klikněte na tlačítko **spustit jednou** na dolním panelu hello.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-225">Click **Run Once** on hello bottom bar.</span></span> <span data-ttu-id="3bbd0-226">Měli byste obdržet oznámení informační zprávy.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-226">You should receive a toast notification.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3bbd0-227">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3bbd0-227">Next steps</span></span>
<span data-ttu-id="3bbd0-228">V tomto jednoduchém příkladu jste vysílali oznámení tooall vaše zařízení se systémem Android.</span><span class="sxs-lookup"><span data-stu-id="3bbd0-228">In this simple example, you broadcasted notifications tooall your Android devices.</span></span> <span data-ttu-id="3bbd0-229">V pořadí tootarget konkrétní uživatele, najdete v kurzu toohello [použití centra oznámení toopush oznámení toousers].</span><span class="sxs-lookup"><span data-stu-id="3bbd0-229">In order tootarget specific users, refer toohello tutorial [Use Notification Hubs toopush notifications toousers].</span></span> <span data-ttu-id="3bbd0-230">Pokud chcete toosegment uživatele podle zájmových skupin, můžete si přečíst [toosend použití centra oznámení nejnovější zprávy přes].</span><span class="sxs-lookup"><span data-stu-id="3bbd0-230">If you want toosegment your users by interest groups, you can read [Use Notification Hubs toosend breaking news].</span></span> <span data-ttu-id="3bbd0-231">Další informace o Notification Hubs toouse v [pokyny centra oznámení] a v hello [Android toofor postupy centra oznámení].</span><span class="sxs-lookup"><span data-stu-id="3bbd0-231">Learn more about how toouse Notification Hubs in [Notification Hubs Guidance] and in hello [Notification Hubs How-toofor Android].</span></span>

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
