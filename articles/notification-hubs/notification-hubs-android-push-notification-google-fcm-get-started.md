---
title: "aaaSending nabízená oznámení tooAndroid pomocí Azure Notification Hubs a zasílání zpráv cloudu Firebase | Microsoft Docs"
description: "V tomto kurzu zjistíte, jak toouse Azure Notification Hubs a zasílání zpráv cloudu Firebase toopush oznámení tooAndroid zařízení."
services: notification-hubs
documentationcenter: android
keywords: "nabízená oznámení, nabízené oznámení, nabízené oznámení android, fcm, firebase cloud messaging"
author: ysxu
manager: erikre
editor: 
ms.assetid: 02298560-da61-4bbb-b07c-e79bd520e420
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: hero-article
ms.date: 07/14/2016
ms.author: yuaxu
ms.openlocfilehash: d2e57437ac7b0ef77abf048f991043620621e58d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sending-push-notifications-tooandroid-with-azure-notification-hubs"></a><span data-ttu-id="79bb8-104">Odesílání nabízených oznámení tooAndroid pomocí Azure Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="79bb8-104">Sending push notifications tooAndroid with Azure Notification Hubs</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="79bb8-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="79bb8-105">Overview</span></span>
> [!IMPORTANT]
> <span data-ttu-id="79bb8-106">Toto téma popisuje nabízená oznámení ve službě Google Firebase Cloud Messaging (FCM).</span><span class="sxs-lookup"><span data-stu-id="79bb8-106">This topic demonstrates push notifications with Google Firebase Cloud Messaging (FCM).</span></span> <span data-ttu-id="79bb8-107">Pokud stále používáte zasílání zpráv cloudu Google (GCM), přečtěte si téma [odesílající nabízená oznámení tooAndroid pomocí Azure Notification Hubs a GCM](notification-hubs-android-push-notification-google-gcm-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="79bb8-107">If you are still using Google Cloud Messaging (GCM), see [Sending push notifications tooAndroid with Azure Notification Hubs and GCM](notification-hubs-android-push-notification-google-gcm-get-started.md).</span></span>
> 
> 

<span data-ttu-id="79bb8-108">Tento kurz ukazuje, jak toouse Azure Notification Hubs a zasílání zpráv cloudu Firebase toosend nabízená oznámení tooan aplikace pro Android.</span><span class="sxs-lookup"><span data-stu-id="79bb8-108">This tutorial shows you how toouse Azure Notification Hubs and Firebase Cloud Messaging toosend push notifications tooan Android application.</span></span>
<span data-ttu-id="79bb8-109">Vytvoříte prázdnou aplikaci systému Android, která bude přijímat nabízená oznámení pomocí služby Firebase Cloud Messaging (FCM).</span><span class="sxs-lookup"><span data-stu-id="79bb8-109">You'll create a blank Android app that receives push notifications by using Firebase Cloud Messaging (FCM).</span></span>

[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

<span data-ttu-id="79bb8-110">Hello dokončit kód v tomto kurzu lze stáhnout z webu GitHub [zde](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStartedFirebase).</span><span class="sxs-lookup"><span data-stu-id="79bb8-110">hello completed code for this tutorial can be downloaded from GitHub [here](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStartedFirebase).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="79bb8-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="79bb8-111">Prerequisites</span></span>
> [!IMPORTANT]
> <span data-ttu-id="79bb8-112">toocomplete tento kurz, musíte mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="79bb8-112">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="79bb8-113">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="79bb8-113">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="79bb8-114">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).</span><span class="sxs-lookup"><span data-stu-id="79bb8-114">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).</span></span>
> 
> 

* <span data-ttu-id="79bb8-115">Kromě toho aktivní účet Azure tooan uvedených výše, tento kurz vyžaduje hello nejnovější verze [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797).</span><span class="sxs-lookup"><span data-stu-id="79bb8-115">In addition tooan active Azure account mentioned above, this tutorial requires hello latest version of [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797).</span></span>
* <span data-ttu-id="79bb8-116">Android 2.3 nebo novější pro službu Firebase Cloud Messaging.</span><span class="sxs-lookup"><span data-stu-id="79bb8-116">Android 2.3 or higher for Firebase Cloud Messaging.</span></span>
* <span data-ttu-id="79bb8-117">Služba Firebase Cloud Messaging vyžaduje úložiště Google verze 27 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="79bb8-117">Google Repository revision 27 or higher is required for Firebase Cloud Messaging.</span></span>
* <span data-ttu-id="79bb8-118">Služby Google Play 9.0.2 nebo novější pro službu Firebase Cloud Messaging.</span><span class="sxs-lookup"><span data-stu-id="79bb8-118">Google Play Services 9.0.2 or higher for Firebase Cloud Messaging.</span></span>
* <span data-ttu-id="79bb8-119">Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy Notification Hubs pro Android Apps.</span><span class="sxs-lookup"><span data-stu-id="79bb8-119">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for Android apps.</span></span>

## <a name="create-a-new-android-studio-project"></a><span data-ttu-id="79bb8-120">Vytvoření nového projektu v Android Studiu</span><span class="sxs-lookup"><span data-stu-id="79bb8-120">Create a new Android Studio Project</span></span>
1. <span data-ttu-id="79bb8-121">V nástroji Android Studio spusťte nový projekt Android Studio.</span><span class="sxs-lookup"><span data-stu-id="79bb8-121">In Android Studio, start a new Android Studio project.</span></span>
   
       ![Android Studio - new project](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-new-project.png)
2. <span data-ttu-id="79bb8-122">Zvolte hello **telefon i Tablet** formuláři faktor a hello **minimální SDK** , které chcete toosupport.</span><span class="sxs-lookup"><span data-stu-id="79bb8-122">Choose hello **Phone and Tablet** form factor and hello **Minimum SDK** that you want toosupport.</span></span> <span data-ttu-id="79bb8-123">Pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="79bb8-123">Then click **Next**.</span></span>
   
       ![Android Studio - project creation workflow](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-choose-form-factor.png)
3. <span data-ttu-id="79bb8-124">Zvolte **prázdná aktivita** hello hlavní aktivitu, klikněte na tlačítko **Další**a potom klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="79bb8-124">Choose **Empty Activity** for hello main activity, click **Next**, and then click **Finish**.</span></span>

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a><span data-ttu-id="79bb8-125">Vytvoření projektu, který podporuje službu Firebase Cloud Messaging</span><span class="sxs-lookup"><span data-stu-id="79bb8-125">Create a project that supports Firebase Cloud Messaging</span></span>
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-new-notification-hub"></a><span data-ttu-id="79bb8-126">Konfigurace nového centra oznámení</span><span class="sxs-lookup"><span data-stu-id="79bb8-126">Configure a new notification hub</span></span>
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<span data-ttu-id="79bb8-127">&emsp;&emsp;6.</span><span class="sxs-lookup"><span data-stu-id="79bb8-127">&emsp;&emsp;6.</span></span> <span data-ttu-id="79bb8-128">V hello **nastavení** okno centra oznámení, vyberte **služby oznámení** a potom **Google (GCM)**.</span><span class="sxs-lookup"><span data-stu-id="79bb8-128">In hello **Settings** blade of your notification hub, select **Notification Services** and then **Google (GCM)**.</span></span> <span data-ttu-id="79bb8-129">Zadejte klíč serveru FCM hello jste zkopírovali dříve z hello [Firebase konzoly](https://firebase.google.com/console/) a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="79bb8-129">Enter hello FCM server key you copied earlier from hello [Firebase console](https://firebase.google.com/console/) and click **Save**.</span></span>

&emsp;&emsp;![Azure Notification Hubs – Google (GCM)](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-gcm-api.png)

<span data-ttu-id="79bb8-131">Vaše centrum oznámení je nyní nakonfigurované toowork s Firebase cloudu Messagin a máte hello připojovací řetězce tooboth registraci vaší aplikace tooreceive a odesílání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="79bb8-131">Your notification hub is now configured toowork with Firebase Cloud Messagin, and you have hello connection strings tooboth register your app tooreceive and send push notifications.</span></span>

## <span data-ttu-id="79bb8-132"><a id="connecting-app"></a>Připojit vaše Centrum oznámení toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="79bb8-132"><a id="connecting-app"></a>Connect your app toohello notification hub</span></span>
### <a name="add-google-play-services-toohello-project"></a><span data-ttu-id="79bb8-133">Přidání projektu toohello služby Google Play</span><span class="sxs-lookup"><span data-stu-id="79bb8-133">Add Google Play services toohello project</span></span>
[!INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

### <a name="adding-azure-notification-hubs-libraries"></a><span data-ttu-id="79bb8-134">Přidání knihoven Azure Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="79bb8-134">Adding Azure Notification Hubs libraries</span></span>
1. <span data-ttu-id="79bb8-135">V hello `Build.Gradle` souboru hello **aplikace**, přidejte následující řádky do hello hello **závislosti** části.</span><span class="sxs-lookup"><span data-stu-id="79bb8-135">In hello `Build.Gradle` file for hello **app**, add hello following lines in hello **dependencies** section.</span></span>
   
        compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
        compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
2. <span data-ttu-id="79bb8-136">Přidejte následující úložiště po hello hello **závislosti** části.</span><span class="sxs-lookup"><span data-stu-id="79bb8-136">Add hello following repository after hello **dependencies** section.</span></span>
   
        repositories {
            maven {
                url "http://dl.bintray.com/microsoftazuremobile/SDK"
            }
        }

### <a name="updating-hello-androidmanifestxml"></a><span data-ttu-id="79bb8-137">Aktualizace hello AndroidManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="79bb8-137">Updating hello AndroidManifest.xml.</span></span>
1. <span data-ttu-id="79bb8-138">toosupport FCM, musíme implementovat Instance ID naslouchací proces služby v našem kódu, který se používá příliš[získání registrace tokenů](https://firebase.google.com/docs/cloud-messaging/android/client#sample-register) pomocí [rozhraní API Google FirebaseInstanceId](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId).</span><span class="sxs-lookup"><span data-stu-id="79bb8-138">toosupport FCM, we must implement a Instance ID listener service in our code which is used too[obtain registration tokens](https://firebase.google.com/docs/cloud-messaging/android/client#sample-register) using [Google's FirebaseInstanceId API](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId).</span></span> <span data-ttu-id="79bb8-139">V tomto kurzu pojmenujeme třídu hello `MyInstanceIDService`.</span><span class="sxs-lookup"><span data-stu-id="79bb8-139">In this tutorial we will name hello class `MyInstanceIDService`.</span></span> 
   
    <span data-ttu-id="79bb8-140">Přidejte následující služby definice toohello souboru AndroidManifest.xml uvnitř hello hello `<application>` značky.</span><span class="sxs-lookup"><span data-stu-id="79bb8-140">Add hello following service definition toohello AndroidManifest.xml file, inside hello `<application>` tag.</span></span> 
   
        <service android:name=".MyInstanceIDService">
            <intent-filter>
                <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
            </intent-filter>
        </service>
2. <span data-ttu-id="79bb8-141">Po obdržení tokenu registrace FCM z hello FirebaseInstanceId rozhraní API, budeme ji používat příliš[zaregistrovat hello centra oznámení Azure](notification-hubs-push-notification-registration-management.md).</span><span class="sxs-lookup"><span data-stu-id="79bb8-141">Once we have received our FCM registration token from hello FirebaseInstanceId API, we will use it too[register with hello Azure Notification Hub](notification-hubs-push-notification-registration-management.md).</span></span> <span data-ttu-id="79bb8-142">Tato registrace podpoříme pomocí pozadí hello `IntentService` s názvem `RegistrationIntentService`.</span><span class="sxs-lookup"><span data-stu-id="79bb8-142">We will support this registration in hello background using an `IntentService` named `RegistrationIntentService`.</span></span> <span data-ttu-id="79bb8-143">Tato služba bude také odpovědná za aktualizaci registračního tokenu FCM.</span><span class="sxs-lookup"><span data-stu-id="79bb8-143">This service will also be responsible for refreshing our FCM registration token.</span></span>
   
    <span data-ttu-id="79bb8-144">Přidejte následující služby definice toohello souboru AndroidManifest.xml uvnitř hello hello `<application>` značky.</span><span class="sxs-lookup"><span data-stu-id="79bb8-144">Add hello following service definition toohello AndroidManifest.xml file, inside hello `<application>` tag.</span></span> 
   
        <service
            android:name=".RegistrationIntentService"
            android:exported="false">
        </service>
3. <span data-ttu-id="79bb8-145">Také definujeme tooreceive oznámení příjemce.</span><span class="sxs-lookup"><span data-stu-id="79bb8-145">We will also define a receiver tooreceive notifications.</span></span> <span data-ttu-id="79bb8-146">Přidejte následující příjemce definice toohello souboru AndroidManifest.xml uvnitř hello hello `<application>` značky.</span><span class="sxs-lookup"><span data-stu-id="79bb8-146">Add hello following receiver definition toohello AndroidManifest.xml file, inside hello `<application>` tag.</span></span> <span data-ttu-id="79bb8-147">Nahraďte hello `<your package>` zástupný symbol hello vaší skutečného názvu balíčku zobrazeného v horní hello části hello `AndroidManifest.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="79bb8-147">Replace hello `<your package>` placeholder with hello your actual package name shown at hello top of hello `AndroidManifest.xml` file.</span></span>
   
        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your package name>" />
            </intent-filter>
        </receiver>
4. <span data-ttu-id="79bb8-148">Přidání oprávnění níže hello související s hello následující nezbytné FCM `</application>` značky.</span><span class="sxs-lookup"><span data-stu-id="79bb8-148">Add hello following necessary FCM related permissions below hello  `</application>` tag.</span></span> <span data-ttu-id="79bb8-149">Ujistěte se, že tooreplace `<your package>` s hello názvu balíčku zobrazeného v horní hello části hello `AndroidManifest.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="79bb8-149">Make sure tooreplace `<your package>` with hello package name shown at hello top of hello `AndroidManifest.xml` file.</span></span>
   
    <span data-ttu-id="79bb8-150">Další informace o těchto oprávnění najdete v tématu [nastavení klientské aplikace GCM pro Android](https://developers.google.com/cloud-messaging/android/client#manifest) a [migrace klientské aplikace GCM pro Android tooFirebase Cloud Messaging](https://developers.google.com/cloud-messaging/android/android-migrate-fcm#remove_the_permissions_required_by_gcm).</span><span class="sxs-lookup"><span data-stu-id="79bb8-150">For more information on these permissions, see [Setup a GCM Client app for Android](https://developers.google.com/cloud-messaging/android/client#manifest) and [Migrate a GCM Client App for Android tooFirebase Cloud Messaging](https://developers.google.com/cloud-messaging/android/android-migrate-fcm#remove_the_permissions_required_by_gcm).</span></span>
   
        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />

### <a name="adding-code"></a><span data-ttu-id="79bb8-151">Přidání kódu</span><span class="sxs-lookup"><span data-stu-id="79bb8-151">Adding code</span></span>
1. <span data-ttu-id="79bb8-152">V zobrazení projektu hello, rozbalte položku **aplikace** > **src** > **hlavní** > **java**.</span><span class="sxs-lookup"><span data-stu-id="79bb8-152">In hello Project View, expand **app** > **src** > **main** > **java**.</span></span> <span data-ttu-id="79bb8-153">Klikněte pravým tlačítkem na váš balíček ve složce **java**, klikněte na tlačítko **Nový** a pak klikněte na tlačítko **třída jazyka Java**.</span><span class="sxs-lookup"><span data-stu-id="79bb8-153">Right-click your package folder under **java**, click **New**, and then click **Java Class**.</span></span> <span data-ttu-id="79bb8-154">Přidejte novou třídu s názvem `NotificationSettings`.</span><span class="sxs-lookup"><span data-stu-id="79bb8-154">Add a new class named `NotificationSettings`.</span></span> 
   
    ![Android Studio – nová třída Java](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hub-android-new-class.png)
   
    <span data-ttu-id="79bb8-156">Zajistěte, aby tooupdate hello tyto tři zástupné symboly v následujícím kódu pro hello hello `NotificationSettings` třídy:</span><span class="sxs-lookup"><span data-stu-id="79bb8-156">Make sure tooupdate hello these three placeholders in hello following code for hello `NotificationSettings` class:</span></span>
   
   * <span data-ttu-id="79bb8-157">**ID odesílatele**: hello Id odesílatele, které jste získali dříve v hello **Cloud Messaging** kartě nastavení projektu v hello [Firebase konzoly](https://firebase.google.com/console/).</span><span class="sxs-lookup"><span data-stu-id="79bb8-157">**SenderId**: hello Sender Id you obtained earlier in hello **Cloud Messaging** tab of your project settings in hello [Firebase console](https://firebase.google.com/console/).</span></span>
   * <span data-ttu-id="79bb8-158">**HubListenConnectionString**: hello **DefaultListenAccessSignature** připojovací řetězec pro vaše centrum.</span><span class="sxs-lookup"><span data-stu-id="79bb8-158">**HubListenConnectionString**: hello **DefaultListenAccessSignature** connection string for your hub.</span></span> <span data-ttu-id="79bb8-159">Tento připojovací řetězec můžete zkopírovat kliknutím **zásady přístupu** na hello **nastavení** rozbočovače na hello [portálu Azure].</span><span class="sxs-lookup"><span data-stu-id="79bb8-159">You can copy that connection string by clicking **Access Policies** on hello **Settings** blade of your hub on hello [Azure Portal].</span></span>
   * <span data-ttu-id="79bb8-160">**HubName**: použití hello název centra oznámení, který se zobrazí v centru okna hello hello [portálu Azure].</span><span class="sxs-lookup"><span data-stu-id="79bb8-160">**HubName**: Use hello name of your notification hub that appears in hello hub blade in hello [Azure Portal].</span></span>
     
     <span data-ttu-id="79bb8-161">Kód `NotificationSettings`:</span><span class="sxs-lookup"><span data-stu-id="79bb8-161">`NotificationSettings` code:</span></span>
     
       <span data-ttu-id="79bb8-162">public class NotificationSettings {</span><span class="sxs-lookup"><span data-stu-id="79bb8-162">public class NotificationSettings {</span></span>
     
           public static String SenderId = "<Your project number>";
           public static String HubName = "<Your HubName>";
           public static String HubListenConnectionString = "<Enter your DefaultListenSharedAccessSignature connection string>";
       <span data-ttu-id="79bb8-163">}</span><span class="sxs-lookup"><span data-stu-id="79bb8-163">}</span></span>
2. <span data-ttu-id="79bb8-164">Pomocí kroků hello výše, přidejte další novou třídu s názvem `MyInstanceIDService`.</span><span class="sxs-lookup"><span data-stu-id="79bb8-164">Using hello steps above, add another new class named `MyInstanceIDService`.</span></span> <span data-ttu-id="79bb8-165">Toto bude naše implementace služby procesu naslouchání Instance ID.</span><span class="sxs-lookup"><span data-stu-id="79bb8-165">This will be our Instance ID listener service implementation.</span></span>
   
    <span data-ttu-id="79bb8-166">Hello kód pro tuto třídu bude volat naše `IntentService` příliš[tokenu obnovení hello FCM](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) hello pozadí.</span><span class="sxs-lookup"><span data-stu-id="79bb8-166">hello code for this class will call our `IntentService` too[refresh hello FCM token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) in hello background.</span></span>
   
        import android.content.Intent;
        import android.util.Log;
        import com.google.firebase.iid.FirebaseInstanceIdService;

        public class MyInstanceIDService extends FirebaseInstanceIdService {

            private static final String TAG = "MyInstanceIDService";

            @Override
            public void onTokenRefresh() {

                Log.d(TAG, "Refreshing GCM Registration Token");

                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        };


1. <span data-ttu-id="79bb8-167">Přidejte jiný nový projekt tooyour třída s názvem `RegistrationIntentService`.</span><span class="sxs-lookup"><span data-stu-id="79bb8-167">Add another new class tooyour project named, `RegistrationIntentService`.</span></span> <span data-ttu-id="79bb8-168">To bude hello implementace pro naše `IntentService` která zpracuje [aktualizuje token FCM hello](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) a [registrace centra oznámení hello](notification-hubs-push-notification-registration-management.md).</span><span class="sxs-lookup"><span data-stu-id="79bb8-168">This will be hello implementation for our `IntentService` that will handle [refreshing hello FCM token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) and [registering with hello notification hub](notification-hubs-push-notification-registration-management.md).</span></span>
   
    <span data-ttu-id="79bb8-169">Použijte následující kód pro tuto třídu hello.</span><span class="sxs-lookup"><span data-stu-id="79bb8-169">Use hello following code for this class.</span></span>
   
        import android.app.IntentService;
        import android.content.Intent;
        import android.content.SharedPreferences;
        import android.preference.PreferenceManager;
        import android.util.Log;        
        import com.google.firebase.iid.FirebaseInstanceId;
        import com.microsoft.windowsazure.messaging.NotificationHub;
   
        public class RegistrationIntentService extends IntentService {
   
            private static final String TAG = "RegIntentService";
   
            private NotificationHub hub;
   
            public RegistrationIntentService() {
                super(TAG);
            }
   
            @Override
            protected void onHandleIntent(Intent intent) {
   
                SharedPreferences sharedPreferences = PreferenceManager.getDefaultSharedPreferences(this);
                String resultString = null;
                String regID = null;
                String storedToken = null;
   
                try {
                    String FCM_token = FirebaseInstanceId.getInstance().getToken();
                    Log.d(TAG, "FCM Registration Token: " + FCM_token);
   
                    // Storing hello registration id that indicates whether hello generated token has been
                    // sent tooyour server. If it is not stored, send hello token tooyour server,
                    // otherwise your server should have already received hello token.
                    if (((regID=sharedPreferences.getString("registrationID", null)) == null)){
   
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.d(TAG, "Attempting a new registration with NH using FCM token : " + FCM_token);
                        regID = hub.register(FCM_token).getRegistrationId();
   
                        // If you want toouse tags...
                        // Refer too: https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1,tag2").getRegistrationId();
   
                        resultString = "New NH Registration Successfully - RegId : " + regID;
                        Log.d(TAG, resultString);
   
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                        sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                    }
   
                    // Check if hello token may have been compromised and needs refreshing.
                    else if ((storedToken=sharedPreferences.getString("FCMtoken", "")) != FCM_token) {
   
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.d(TAG, "NH Registration refreshing with token : " + FCM_token);
                        regID = hub.register(FCM_token).getRegistrationId();
   
                        // If you want toouse tags...
                        // Refer too: https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1,tag2").getRegistrationId();
   
                        resultString = "New NH Registration Successfully - RegId : " + regID;
                        Log.d(TAG, resultString);
   
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                        sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                    }
   
                    else {
                        resultString = "Previously Registered Successfully - RegId : " + regID;
                    }
                } catch (Exception e) {
                    Log.e(TAG, resultString="Failed toocomplete registration", e);
                    // If an exception happens while fetching hello new token or updating our registration data
                    // on a third-party server, this ensures that we'll attempt hello update at a later time.
                }
   
                // Notify UI that registration has completed.
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(resultString);
                }
            }
        }
2. <span data-ttu-id="79bb8-170">Ve vaší `MainActivity` třídy, přidejte následující hello `import` příkazů výše hello třídy deklarace.</span><span class="sxs-lookup"><span data-stu-id="79bb8-170">In your `MainActivity` class, add hello following `import` statements above hello class declaration.</span></span>
   
        import com.google.android.gms.common.ConnectionResult;
        import com.google.android.gms.common.GoogleApiAvailability;
        import com.microsoft.windowsazure.notifications.NotificationsManager;
        import android.content.Intent;
        import android.util.Log;
        import android.widget.TextView;
        import android.widget.Toast;
3. <span data-ttu-id="79bb8-171">Přidejte následující soukromé členy v horní části hello třídy hello hello.</span><span class="sxs-lookup"><span data-stu-id="79bb8-171">Add hello following private members at hello top of hello class.</span></span> <span data-ttu-id="79bb8-172">Použijeme tyto [zkontrolovat hello dostupnost služeb Google Play dle doporučení Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).</span><span class="sxs-lookup"><span data-stu-id="79bb8-172">We will use these [check hello availability of Google Play Services as recommended by Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).</span></span>
   
        public static MainActivity mainActivity;
        public static Boolean isVisible = false;    
        private static final String TAG = "MainActivity";
        private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;
4. <span data-ttu-id="79bb8-173">Ve vašem `MainActivity` třídy, přidejte následující metodu toohello dostupnost služeb Google Play hello.</span><span class="sxs-lookup"><span data-stu-id="79bb8-173">In your `MainActivity` class, add hello following method toohello availability of Google Play Services.</span></span> 
   
        /**
         * Check hello device toomake sure it has hello Google Play Services APK. If
         * it doesn't, display a dialog that allows users toodownload hello APK from
         * hello Google Play Store or enable it in hello device's system settings.
         */
        private boolean checkPlayServices() {
            GoogleApiAvailability apiAvailability = GoogleApiAvailability.getInstance();
            int resultCode = apiAvailability.isGooglePlayServicesAvailable(this);
            if (resultCode != ConnectionResult.SUCCESS) {
                if (apiAvailability.isUserResolvableError(resultCode)) {
                    apiAvailability.getErrorDialog(this, resultCode, PLAY_SERVICES_RESOLUTION_REQUEST)
                            .show();
                } else {
                    Log.i(TAG, "This device is not supported by Google Play Services.");
                    ToastNotify("This device is not supported by Google Play Services.");
                    finish();
                }
                return false;
            }
            return true;
        }
5. <span data-ttu-id="79bb8-174">Ve vašem `MainActivity` třídy, přidejte následující kód, který zkontrolujte služby Google Play před voláním hello vaše `IntentService` tooget FCM registrační token a registrace pomocí centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="79bb8-174">In your `MainActivity` class, add hello following code that will check for Google Play Services before calling your `IntentService` tooget your FCM registration token and register with your notification hub.</span></span>
   
        public void registerWithNotificationHubs()
        {
            if (checkPlayServices()) {
                // Start IntentService tooregister this application with FCM.
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        }
6. <span data-ttu-id="79bb8-175">V hello `OnCreate` metoda hello `MainActivity` třídy, přidejte následující kód toostart hello registraci při vytvoření aktivity hello.</span><span class="sxs-lookup"><span data-stu-id="79bb8-175">In hello `OnCreate` method of hello `MainActivity` class, add hello following code toostart hello registration process when activity is created.</span></span>
   
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
   
            mainActivity = this;
            NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
            registerWithNotificationHubs();
        }
7. <span data-ttu-id="79bb8-176">Přidejte tyto další metody toohello `MainActivity` tooverify aplikace stavu a stavu sestavy ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="79bb8-176">Add these additional methods toohello `MainActivity` tooverify app state and report status in your app.</span></span>
   
        @Override
        protected void onStart() {
            super.onStart();
            isVisible = true;
        }
   
        @Override
        protected void onPause() {
            super.onPause();
            isVisible = false;
        }
   
        @Override
        protected void onResume() {
            super.onResume();
            isVisible = true;
        }
   
        @Override
        protected void onStop() {
            super.onStop();
            isVisible = false;
        }
   
        public void ToastNotify(final String notificationMessage) {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    Toast.makeText(MainActivity.this, notificationMessage, Toast.LENGTH_LONG).show();
                    TextView helloText = (TextView) findViewById(R.id.text_hello);
                    helloText.setText(notificationMessage);
                }
            });
        }
8. <span data-ttu-id="79bb8-177">Hello `ToastNotify` metoda používá hello *"Hello, World"* `TextView` řízení tooreport stavu a oznámení trvale v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="79bb8-177">hello `ToastNotify` method uses hello *"Hello World"* `TextView` control tooreport status and notifications persistently in hello app.</span></span> <span data-ttu-id="79bb8-178">Do rozložení activity_main.xml přidejte následující id pro ovládací prvek hello.</span><span class="sxs-lookup"><span data-stu-id="79bb8-178">In your activity_main.xml layout, add hello following id for that control.</span></span>
   
       android:id="@+id/text_hello"
9. <span data-ttu-id="79bb8-179">Další přidáme podtřídy pro naše příjemce, které jsme definovali v hello AndroidManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="79bb8-179">Next we will add a subclass for our receiver we defined in hello AndroidManifest.xml.</span></span> <span data-ttu-id="79bb8-180">Přidejte jiný nový projekt tooyour třída s názvem `MyHandler`.</span><span class="sxs-lookup"><span data-stu-id="79bb8-180">Add another new class tooyour project named `MyHandler`.</span></span>
10. <span data-ttu-id="79bb8-181">Přidejte následující příkazy pro import hello horní části hello `MyHandler.java`:</span><span class="sxs-lookup"><span data-stu-id="79bb8-181">Add hello following import statements at hello top of `MyHandler.java`:</span></span>
    
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.media.RingtoneManager;
        import android.net.Uri;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
        import com.microsoft.windowsazure.notifications.NotificationsHandler;
11. <span data-ttu-id="79bb8-182">Přidejte následující kód pro hello hello `MyHandler` třídy, takže je podtřídou třídy `com.microsoft.windowsazure.notifications.NotificationsHandler`.</span><span class="sxs-lookup"><span data-stu-id="79bb8-182">Add hello following code for hello `MyHandler` class making it a subclass of `com.microsoft.windowsazure.notifications.NotificationsHandler`.</span></span>
    
    <span data-ttu-id="79bb8-183">Tento kód přepíše hello `OnReceive` proto hello obslužná rutina nahlásila oznámení, které jsou přijaty.</span><span class="sxs-lookup"><span data-stu-id="79bb8-183">This code overrides hello `OnReceive` method, so hello handler will report notifications that are received.</span></span> <span data-ttu-id="79bb8-184">Hello obslužná rutina také odesílá správci oznámení Android hello nabízená oznámení toohello pomocí hello `sendNotification()` metoda.</span><span class="sxs-lookup"><span data-stu-id="79bb8-184">hello handler also sends hello push notification toohello Android notification manager by using hello `sendNotification()` method.</span></span> <span data-ttu-id="79bb8-185">Hello `sendNotification()` metoda se má provést, když není hello aplikace spuštěna a není přijato oznámení.</span><span class="sxs-lookup"><span data-stu-id="79bb8-185">hello `sendNotification()` method should be executed when hello app is not running and a notification is received.</span></span>
    
        public class MyHandler extends NotificationsHandler {
            public static final int NOTIFICATION_ID = 1;
            private NotificationManager mNotificationManager;
            NotificationCompat.Builder builder;
            Context ctx;
    
            @Override
            public void onReceive(Context context, Bundle bundle) {
                ctx = context;
                String nhMessage = bundle.getString("message");
                sendNotification(nhMessage);
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(nhMessage);
                }
            }
    
            private void sendNotification(String msg) {
    
                Intent intent = new Intent(ctx, MainActivity.class);
                intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
    
                mNotificationManager = (NotificationManager)
                        ctx.getSystemService(Context.NOTIFICATION_SERVICE);
    
                PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                        intent, PendingIntent.FLAG_ONE_SHOT);
    
                Uri defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
                NotificationCompat.Builder mBuilder =
                        new NotificationCompat.Builder(ctx)
                                .setSmallIcon(R.mipmap.ic_launcher)
                                .setContentTitle("Notification Hub Demo")
                                .setStyle(new NotificationCompat.BigTextStyle()
                                        .bigText(msg))
                                .setSound(defaultSoundUri)
                                .setContentText(msg);
    
                mBuilder.setContentIntent(contentIntent);
                mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
            }
        }
12. <span data-ttu-id="79bb8-186">V Android Studio na řádku nabídek hello, klikněte na tlačítko **sestavení** > **znovu sestavit projekt** toomake se, že jsou ve vašem kódu nenachází žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="79bb8-186">In Android Studio on hello menu bar, click **Build** > **Rebuild Project** toomake sure that no errors are present in your code.</span></span>
13. <span data-ttu-id="79bb8-187">Spuštění aplikace hello na vaše zařízení a ověřte, zda že zaregistruje úspěšně hello centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="79bb8-187">Run hello app on your device and verify it registers successfully with hello notification hub.</span></span> 
    
    > [!NOTE]
    > <span data-ttu-id="79bb8-188">Registrace může při spuštění počáteční hello neúspěšné, dokud nebude hello `onTokenRefresh()` je volána metoda instance Id služby.</span><span class="sxs-lookup"><span data-stu-id="79bb8-188">Registration may fail on hello initial launch until hello `onTokenRefresh()` method of instance Id service is called.</span></span> <span data-ttu-id="79bb8-189">aktualizace Hello musí inicializovat úspěšnou registraci hello centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="79bb8-189">hello refresh should intiate a successful registration with hello notification hub.</span></span>
    > 
    > 

## <a name="sending-push-notifications"></a><span data-ttu-id="79bb8-190">Odeslání nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="79bb8-190">Sending push notifications</span></span>
<span data-ttu-id="79bb8-191">Můžete otestovat přijímání nabízených oznámení ve vaší aplikaci jejich odesláním prostřednictvím hello [portálu Azure] -vyhledejte hello **Poradce při potížích s** kapitoly hello okno centra, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="79bb8-191">You can test receiving push notifications in your app by sending them via hello [Azure Portal] - look for hello **Troubleshooting** Section in hello hub blade, as shown below.</span></span>

![Azure Notification Hubs – testovací odeslání](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-test-send.png)

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-directly-from-hello-app"></a><span data-ttu-id="79bb8-193">(Volitelné) Odesílání nabízených oznámení přímo z aplikace hello</span><span class="sxs-lookup"><span data-stu-id="79bb8-193">(Optional) Send push notifications directly from hello app</span></span>
> [!IMPORTANT]
> <span data-ttu-id="79bb8-194">Tady je příklad odesílání oznámení z klienta aplikace hello se poskytuje pro učení pouze pro účely.</span><span class="sxs-lookup"><span data-stu-id="79bb8-194">This example of sending notifications from hello client app is provided for learning purposes only.</span></span> <span data-ttu-id="79bb8-195">Vzhledem k tomu, že to bude vyžadovat hello `DefaultFullSharedAccessSignature` toobe v hello klientskou aplikaci, zpřístupňuje riziko toohello centra oznámení, že uživatel může získat oznámení toosend neoprávněného přístupu tooyour klientů.</span><span class="sxs-lookup"><span data-stu-id="79bb8-195">Since this will require hello `DefaultFullSharedAccessSignature` toobe present on hello client app, it exposes your notification hub toohello risk that a user may gain access toosend unauthorized notifications tooyour clients.</span></span>
> 
> 

<span data-ttu-id="79bb8-196">Za normálních okolností byste odesílali oznámení pomocí serveru backend.</span><span class="sxs-lookup"><span data-stu-id="79bb8-196">Normally, you would send notifications using a backend server.</span></span> <span data-ttu-id="79bb8-197">V některých případech můžete chtít toobe možné toosend nabízených oznámení přímo z klientské aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="79bb8-197">For some cases, you might want toobe able toosend push notifications directly from hello client application.</span></span> <span data-ttu-id="79bb8-198">Tato část vysvětluje, jak toosend oznámení z klienta hello pomocí hello [API služby REST centra oznámení Azure](https://msdn.microsoft.com/library/azure/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="79bb8-198">This section explains how toosend notifications from hello client using hello [Azure Notification Hub REST API](https://msdn.microsoft.com/library/azure/dn223264.aspx).</span></span>

1. <span data-ttu-id="79bb8-199">V zobrazení projektu Android Studio rozbalte možnost **App** > **src** > **main** > **res** > **layout**.</span><span class="sxs-lookup"><span data-stu-id="79bb8-199">In Android Studio Project View, expand **App** > **src** > **main** > **res** > **layout**.</span></span> <span data-ttu-id="79bb8-200">Otevřete hello `activity_main.xml` rozložení souboru a klikněte na tlačítko hello **Text** kartě obsah textu hello tooupdate hello souboru.</span><span class="sxs-lookup"><span data-stu-id="79bb8-200">Open hello `activity_main.xml` layout file and click hello **Text** tab tooupdate hello text contents of hello file.</span></span> <span data-ttu-id="79bb8-201">Aktualizujte jej s hello kódu níže, který přidává nové `Button` a `EditText` ovládací prvky pro zasílání oznámení centra oznámení toohello zprávy oznámení.</span><span class="sxs-lookup"><span data-stu-id="79bb8-201">Update it with hello code below, which adds new `Button` and `EditText` controls for sending push notification messages toohello notification hub.</span></span> <span data-ttu-id="79bb8-202">Přidejte tento kód v dolní části hello, těsně před `</RelativeLayout>`.</span><span class="sxs-lookup"><span data-stu-id="79bb8-202">Add this code at hello bottom, just before `</RelativeLayout>`.</span></span>
   
        <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/send_button"
        android:id="@+id/sendbutton"
        android:layout_centerVertical="true"
        android:layout_centerHorizontal="true"
        android:onClick="sendNotificationButtonOnClick" />
   
        <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/editTextNotificationMessage"
        android:layout_above="@+id/sendbutton"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="42dp"
        android:hint="@string/notification_message_hint" />
2. <span data-ttu-id="79bb8-203">V zobrazení projektu Android Studio rozbalte **App** > **src** > **main** > **res** > **values**.</span><span class="sxs-lookup"><span data-stu-id="79bb8-203">In Android Studio Project View, expand **App** > **src** > **main** > **res** > **values**.</span></span> <span data-ttu-id="79bb8-204">Otevřete hello `strings.xml` souboru a přidejte hello řetězcové hodnoty, které jsou odkazovány pomocí nového hello `Button` a `EditText` ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="79bb8-204">Open hello `strings.xml` file and add hello string values that are referenced by hello new `Button` and `EditText` controls.</span></span> <span data-ttu-id="79bb8-205">Přidejte tyto dolnímu hello hello souboru, těsně před `</resources>`.</span><span class="sxs-lookup"><span data-stu-id="79bb8-205">Add these at hello bottom of hello file, just before `</resources>`.</span></span>
   
        <string name="send_button">Send Notification</string>
        <string name="notification_message_hint">Enter notification message text</string>
3. <span data-ttu-id="79bb8-206">Ve vaší `NotificationSetting.java` soubor, přidejte následující nastavení toohello hello `NotificationSettings` třídy.</span><span class="sxs-lookup"><span data-stu-id="79bb8-206">In your `NotificationSetting.java` file, add hello following setting toohello `NotificationSettings` class.</span></span>
   
    <span data-ttu-id="79bb8-207">Aktualizace `HubFullAccess` s hello **DefaultFullSharedAccessSignature** připojovací řetězec pro vaše centrum.</span><span class="sxs-lookup"><span data-stu-id="79bb8-207">Update `HubFullAccess` with hello **DefaultFullSharedAccessSignature** connection string for your hub.</span></span> <span data-ttu-id="79bb8-208">Tento připojovací řetězec lze kopírovat z hello [portálu Azure] kliknutím **zásady přístupu** na hello **nastavení** okna pro vaše Centrum oznámení.</span><span class="sxs-lookup"><span data-stu-id="79bb8-208">This connection string can be copied from hello [Azure Portal] by clicking **Access Policies** on hello **Settings** blade for your notification hub.</span></span>
   
        public static String HubFullAccess = "<Enter Your DefaultFullSharedAccessSignature Connection string>";
4. <span data-ttu-id="79bb8-209">Ve vaší `MainActivity.java` soubor, přidejte následující hello `import` příkazy výše hello `MainActivity` třídy.</span><span class="sxs-lookup"><span data-stu-id="79bb8-209">In your `MainActivity.java` file, add hello following `import` statements above hello `MainActivity` class.</span></span>
   
        import java.io.BufferedOutputStream;
        import java.io.BufferedReader;
        import java.io.InputStreamReader;
        import java.io.OutputStream;
        import java.net.HttpURLConnection;
        import java.net.URL;
        import java.net.URLEncoder;
        import javax.crypto.Mac;
        import javax.crypto.spec.SecretKeySpec;
        import android.util.Base64;
        import android.view.View;
        import android.widget.EditText;
5. <span data-ttu-id="79bb8-210">Ve vaší `MainActivity.java` soubor, přidejte následující členy hello horní části hello hello `MainActivity` třídy.</span><span class="sxs-lookup"><span data-stu-id="79bb8-210">In your `MainActivity.java` file, add hello following members at hello top of hello `MainActivity` class.</span></span>    
   
        private String HubEndpoint = null;
        private String HubSasKeyName = null;
        private String HubSasKeyValue = null;
6. <span data-ttu-id="79bb8-211">Centrum POST požadavek toosend zprávy tooyour oznámení, musíte vytvořit tokenu tooauthenticate softwaru přístupový podpis (SaS).</span><span class="sxs-lookup"><span data-stu-id="79bb8-211">You must create a Software Access Signature (SaS) token tooauthenticate a POST request toosend messages tooyour notification hub.</span></span> <span data-ttu-id="79bb8-212">K tomu je potřeba analýza hello klíčová data z hello připojovací řetězec a pak vytvořit hello tokenu SaS, jak je uvedeno v hello [běžné koncepty](http://msdn.microsoft.com/library/azure/dn495627.aspx) odkazu k REST API.</span><span class="sxs-lookup"><span data-stu-id="79bb8-212">This is done by parsing hello key data from hello connection string and then creating hello SaS token, as mentioned in hello [Common Concepts](http://msdn.microsoft.com/library/azure/dn495627.aspx) REST API reference.</span></span> <span data-ttu-id="79bb8-213">Hello následující kód představuje příklad implementace.</span><span class="sxs-lookup"><span data-stu-id="79bb8-213">hello following code is an example implementation.</span></span>
   
    <span data-ttu-id="79bb8-214">V `MainActivity.java`, přidejte následující metodu toohello hello `MainActivity` třídy tooparse připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="79bb8-214">In `MainActivity.java`, add hello following method toohello `MainActivity` class tooparse your connection string.</span></span>
   
        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx
         * tooparse hello connection string so a SaS authentication token can be
         * constructed.
         *
         * @param connectionString This must be hello DefaultFullSharedAccess connection
         *                         string for this example.
         */
        private void ParseConnectionString(String connectionString)
        {
            String[] parts = connectionString.split(";");
            if (parts.length != 3)
                throw new RuntimeException("Error parsing connection string: "
                        + connectionString);
   
            for (int i = 0; i < parts.length; i++) {
                if (parts[i].startsWith("Endpoint")) {
                    this.HubEndpoint = "https" + parts[i].substring(11);
                } else if (parts[i].startsWith("SharedAccessKeyName")) {
                    this.HubSasKeyName = parts[i].substring(20);
                } else if (parts[i].startsWith("SharedAccessKey")) {
                    this.HubSasKeyValue = parts[i].substring(16);
                }
            }
        }
7. <span data-ttu-id="79bb8-215">V `MainActivity.java`, přidejte následující metodu toohello hello `MainActivity` třída toocreate ověřovacího tokenu SaS.</span><span class="sxs-lookup"><span data-stu-id="79bb8-215">In `MainActivity.java`, add hello following method toohello `MainActivity` class toocreate a SaS authentication token.</span></span>
   
        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx to
         * construct a SaS token from hello access key tooauthenticate a request.
         *
         * @param uri hello unencoded resource URI string for this operation. hello resource
         *            URI is hello full URI of hello Service Bus resource toowhich access is
         *            claimed. For example,
         *            "http://<namespace>.servicebus.windows.net/<hubName>"
         */
        private String generateSasToken(String uri) {
   
            String targetUri;
            String token = null;
            try {
                targetUri = URLEncoder
                        .encode(uri.toString().toLowerCase(), "UTF-8")
                        .toLowerCase();
   
                long expiresOnDate = System.currentTimeMillis();
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60 * 1000;
                long expires = expiresOnDate / 1000;
                String toSign = targetUri + "\n" + expires;
   
                // Get an hmac_sha1 key from hello raw key bytes
                byte[] keyBytes = HubSasKeyValue.getBytes("UTF-8");
                SecretKeySpec signingKey = new SecretKeySpec(keyBytes, "HmacSHA256");
   
                // Get an hmac_sha1 Mac instance and initialize with hello signing key
                Mac mac = Mac.getInstance("HmacSHA256");
                mac.init(signingKey);
   
                // Compute hello hmac on input data bytes
                byte[] rawHmac = mac.doFinal(toSign.getBytes("UTF-8"));
   
                // Using android.util.Base64 for Android Studio instead of
                // Apache commons codec
                String signature = URLEncoder.encode(
                        Base64.encodeToString(rawHmac, Base64.NO_WRAP).toString(), "UTF-8");
   
                // Construct authorization string
                token = "SharedAccessSignature sr=" + targetUri + "&sig="
                        + signature + "&se=" + expires + "&skn=" + HubSasKeyName;
            } catch (Exception e) {
                if (isVisible) {
                    ToastNotify("Exception Generating SaS : " + e.getMessage().toString());
                }
            }
   
            return token;
        }
8. <span data-ttu-id="79bb8-216">V `MainActivity.java`, přidejte následující metodu toohello hello `MainActivity` třída toohandle hello **odeslat oznámení** tlačítko a odesílání nabízených oznámení hello hello zpráva toohello centra pomocí předdefinovaného REST API.</span><span class="sxs-lookup"><span data-stu-id="79bb8-216">In `MainActivity.java`, add hello following method toohello `MainActivity` class toohandle hello **Send Notification** button click and send hello push notification message toohello hub by using hello built-in REST API.</span></span>
   
        /**
         * Send Notification button click handler. This method parses the
         * DefaultFullSharedAccess connection string and generates a SaS token. The
         * token is added toohello Authorization header on hello POST request toothe
         * notification hub. hello text in hello editTextNotificationMessage control
         * is added as hello JSON body for hello request tooadd a GCM message toohello hub.
         *
         * @param v
         */
        public void sendNotificationButtonOnClick(View v) {
            EditText notificationText = (EditText) findViewById(R.id.editTextNotificationMessage);
            final String json = "{\"data\":{\"message\":\"" + notificationText.getText().toString() + "\"}}";
   
            new Thread()
            {
                public void run()
                {
                    try
                    {
                        // Based on reference documentation...
                        // http://msdn.microsoft.com/library/azure/dn223273.aspx
                        ParseConnectionString(NotificationSettings.HubFullAccess);
                        URL url = new URL(HubEndpoint + NotificationSettings.HubName +
                                "/messages/?api-version=2015-01");
   
                        HttpURLConnection urlConnection = (HttpURLConnection)url.openConnection();
   
                        try {
                            // POST request
                            urlConnection.setDoOutput(true);
   
                            // Authenticate hello POST request with hello SaS token
                            urlConnection.setRequestProperty("Authorization", 
                                generateSasToken(url.toString()));
   
                            // Notification format should be GCM
                            urlConnection.setRequestProperty("ServiceBusNotification-Format", "gcm");
   
                            // Include any tags
                            // Example below targets 3 specific tags
                            // Refer too: https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                            // urlConnection.setRequestProperty("ServiceBusNotification-Tags", 
                            //        "tag1 || tag2 || tag3");
   
                            // Send notification message
                            urlConnection.setFixedLengthStreamingMode(json.length());
                            OutputStream bodyStream = new BufferedOutputStream(urlConnection.getOutputStream());
                            bodyStream.write(json.getBytes());
                            bodyStream.close();
   
                            // Get reponse
                            urlConnection.connect();
                            int responseCode = urlConnection.getResponseCode();
                            if ((responseCode != 200) && (responseCode != 201)) {
                                BufferedReader br = new BufferedReader(new InputStreamReader((urlConnection.getErrorStream())));
                                String line;
                                StringBuilder builder = new StringBuilder("Send Notification returned " +
                                        responseCode + " : ")  ;
                                while ((line = br.readLine()) != null) {
                                    builder.append(line);
                                }
   
                                ToastNotify(builder.toString());
                            }
                        } finally {
                            urlConnection.disconnect();
                        }
                    }
                    catch(Exception e)
                    {
                        if (isVisible) {
                            ToastNotify("Exception Sending Notification : " + e.getMessage().toString());
                        }
                    }
                }
            }.start();
        }

## <a name="testing-your-app"></a><span data-ttu-id="79bb8-217">Testování vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="79bb8-217">Testing your app</span></span>
#### <a name="push-notifications-in-hello-emulator"></a><span data-ttu-id="79bb8-218">Nabízená oznámení v emulátoru hello</span><span class="sxs-lookup"><span data-stu-id="79bb8-218">Push notifications in hello emulator</span></span>
<span data-ttu-id="79bb8-219">Pokud chcete, aby tootest nabízená oznámení uvnitř emulátoru, ujistěte se, že bitová kopie emulátoru podporuje úroveň rozhraní Google API hello, kterou jste zvolili pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="79bb8-219">If you want tootest push notifications inside an emulator, make sure that your emulator image supports hello Google API level that you chose for your app.</span></span> <span data-ttu-id="79bb8-220">Pokud bitová kopie nepodporuje nativní rozhraní Google API, budete mít s hello **služby\_není\_dostupné** výjimka.</span><span class="sxs-lookup"><span data-stu-id="79bb8-220">If your image doesn't support native Google APIs, you will end up with hello **SERVICE\_NOT\_AVAILABLE** exception.</span></span>

<span data-ttu-id="79bb8-221">Kromě toho toohello výše, zkontrolujte, že jste přidali vaší tooyour účet Google spuštěný emulátoru pod **nastavení** > **účty**.</span><span class="sxs-lookup"><span data-stu-id="79bb8-221">In addition toohello above, ensure that you have added your Google account tooyour running emulator under **Settings** > **Accounts**.</span></span> <span data-ttu-id="79bb8-222">Jinak, může vaše pokusy o tooregister s GCM výsledkem hello **ověřování\_se nezdařilo** výjimka.</span><span class="sxs-lookup"><span data-stu-id="79bb8-222">Otherwise, your attempts tooregister with GCM may result in hello **AUTHENTICATION\_FAILED** exception.</span></span>

#### <a name="running-hello-application"></a><span data-ttu-id="79bb8-223">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="79bb8-223">Running hello application</span></span>
1. <span data-ttu-id="79bb8-224">Spuštění aplikace hello a Všimněte si, že hello ID registrace hlášené pro úspěšnou registraci.</span><span class="sxs-lookup"><span data-stu-id="79bb8-224">Run hello app and notice that hello registration ID is reported for a successful registration.</span></span>
   
       ![Testing on Android - Channel registration](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-registered.png)
2. <span data-ttu-id="79bb8-225">Zadejte toobe zpráv oznámení odeslaných tooall zařízení Android, která byla zaregistrovaná hello rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="79bb8-225">Enter a notification message toobe sent tooall Android devices that have registered with hello hub.</span></span>
   
       ![Testing on Android - sending a message](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-set-message.png)
3. <span data-ttu-id="79bb8-226">Stiskněte tlačítko **Odeslat oznámení**.</span><span class="sxs-lookup"><span data-stu-id="79bb8-226">Press **Send Notification**.</span></span> <span data-ttu-id="79bb8-227">Zobrazí všechna zařízení, které aplikace běžet hello `AlertDialog` instance s hello nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="79bb8-227">Any devices that have hello app running will show an `AlertDialog` instance with hello push notification message.</span></span> <span data-ttu-id="79bb8-228">Zařízení, které nemají hello aplikace spuštěná, ale byla dříve registrována pro nabízená oznámení se zobrazí oznámení v hello správci oznámení Android.</span><span class="sxs-lookup"><span data-stu-id="79bb8-228">Devices that don't have hello app running but were previously registered for push notifications will receive a notification in hello Android Notification Manager.</span></span> <span data-ttu-id="79bb8-229">Ta lze zobrazit potažením dolů z levého horního rohu hello.</span><span class="sxs-lookup"><span data-stu-id="79bb8-229">Those can be viewed by swiping down from hello upper-left corner.</span></span>
   
       ![Testing on Android - notifications](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-received-message.png)

## <a name="next-steps"></a><span data-ttu-id="79bb8-230">Další kroky</span><span class="sxs-lookup"><span data-stu-id="79bb8-230">Next steps</span></span>
<span data-ttu-id="79bb8-231">Doporučujeme, abyste hello [použití centra oznámení toopush oznámení toousers] kurz jako další krok hello.</span><span class="sxs-lookup"><span data-stu-id="79bb8-231">We recommend hello [Use Notification Hubs toopush notifications toousers] tutorial as hello next step.</span></span> <span data-ttu-id="79bb8-232">Zobrazí se jak toosend oznámení z back-end ASP.NET pomocí značky tootarget konkrétním uživatelům.</span><span class="sxs-lookup"><span data-stu-id="79bb8-232">It will show you how toosend notifications from an ASP.NET backend using tags tootarget specific users.</span></span>

<span data-ttu-id="79bb8-233">Pokud chcete toosegment uživatele podle zájmových skupin, podívejte se na hello [toosend použití centra oznámení nejnovější zprávy přes] kurzu.</span><span class="sxs-lookup"><span data-stu-id="79bb8-233">If you want toosegment your users by interest groups, check out hello [Use Notification Hubs toosend breaking news] tutorial.</span></span>

<span data-ttu-id="79bb8-234">toolearn další obecné informace o centrech oznámení naleznete v našem [pokyny centra oznámení].</span><span class="sxs-lookup"><span data-stu-id="79bb8-234">toolearn more general information about Notification Hubs, see our [Notification Hubs Guidance].</span></span>

<!-- Images. -->



<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Azure Classic Portal]: https://manage.windowsazure.com/
[pokyny centra oznámení]: notification-hubs-push-notification-overview.md
[použití centra oznámení toopush oznámení toousers]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[toosend použití centra oznámení nejnovější zprávy přes]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[portálu Azure]: https://portal.azure.com
