---
title: "Odesílání nabízených oznámení do systému Android pomocí Azure Notification Hubs | Dokumentace Microsoftu"
description: "V tomto kurzu zjistíte, jak používat Azure Notification Hubs k odesílání nabízených oznámení do zařízení se systémem Android."
services: notification-hubs
documentationcenter: android
keywords: "nabízená oznámení;nabízené oznámení;nabízené oznámení Android"
author: ysxu
manager: erikre
editor: 
ms.assetid: 8268c6ef-af63-433c-b14e-a20b04a0342a
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: hero-article
ms.date: 07/05/2016
ms.author: yuaxu
ms.openlocfilehash: 808fc10ef1ebb3288facbdf2e9e817b27d4fc6bc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sending-push-notifications-to-android-with-azure-notification-hubs"></a><span data-ttu-id="f74bc-104">Odesílání nabízených oznámení do systému Android pomocí Azure Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="f74bc-104">Sending push notifications to Android with Azure Notification Hubs</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="f74bc-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="f74bc-105">Overview</span></span>
> [!IMPORTANT]
> <span data-ttu-id="f74bc-106">Toto téma popisuje nabízená oznámení ve službě Google Cloud Messaging (GCM).</span><span class="sxs-lookup"><span data-stu-id="f74bc-106">This topic demonstrates push notifications with Google Cloud Messaging (GCM).</span></span> <span data-ttu-id="f74bc-107">Pokud používáte Firebase Cloud Messaging (FCM) od Googlu, přečtěte si článek [Odesílání nabízených oznámení do systému Android pomocí služeb Azure Notification Hubs a FCM](notification-hubs-android-push-notification-google-fcm-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="f74bc-107">If you are using Google's Firebase Cloud Messaging (FCM), see [Sending push notifications to Android with Azure Notification Hubs and FCM](notification-hubs-android-push-notification-google-fcm-get-started.md).</span></span>
> 
> 

<span data-ttu-id="f74bc-108">V tomto kurzu zjistíte, jak používat Azure Notification Hubs k odesílání nabízených oznámení do aplikace systému Android.</span><span class="sxs-lookup"><span data-stu-id="f74bc-108">This tutorial shows you how to use Azure Notification Hubs to send push notifications to an Android application.</span></span>
<span data-ttu-id="f74bc-109">Vytvoříte prázdnou aplikaci systému Android, která bude přijímat nabízená oznámení pomocí služby GCM (Google Cloud Messaging).</span><span class="sxs-lookup"><span data-stu-id="f74bc-109">You'll create a blank Android app that receives push notifications by using Google Cloud Messaging (GCM).</span></span>

[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

<span data-ttu-id="f74bc-110">Dokončený kód v tomto kurzu lze stáhnout z portálu Github [zde](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStarted).</span><span class="sxs-lookup"><span data-stu-id="f74bc-110">The completed code for this tutorial can be downloaded from GitHub [here](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStarted).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f74bc-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f74bc-111">Prerequisites</span></span>
> [!IMPORTANT]
> <span data-ttu-id="f74bc-112">K dokončení tohoto kurzu potřebujete mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="f74bc-112">To complete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="f74bc-113">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="f74bc-113">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="f74bc-114">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).</span><span class="sxs-lookup"><span data-stu-id="f74bc-114">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).</span></span>
> 
> 

<span data-ttu-id="f74bc-115">Kromě aktivního účtu Azure uvedeného výše budete v tomto kurzu potřebovat pouze nejnovější verzi [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797).</span><span class="sxs-lookup"><span data-stu-id="f74bc-115">In addition to an active Azure account mentioned above, this tutorial only requires the latest version of [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797).</span></span>

<span data-ttu-id="f74bc-116">Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy Notification Hubs pro Android Apps.</span><span class="sxs-lookup"><span data-stu-id="f74bc-116">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for Android apps.</span></span>

## <a name="creating-a-project-that-supports-google-cloud-messaging"></a><span data-ttu-id="f74bc-117">Vytvoření projektu, který podporuje službu GCM (Google Cloud Messaging)</span><span class="sxs-lookup"><span data-stu-id="f74bc-117">Creating a project that supports Google Cloud Messaging</span></span>
[!INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

## <a name="configure-a-new-notification-hub"></a><span data-ttu-id="f74bc-118">Konfigurace nového centra oznámení</span><span class="sxs-lookup"><span data-stu-id="f74bc-118">Configure a new notification hub</span></span>
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<span data-ttu-id="f74bc-119">&emsp;&emsp;6.</span><span class="sxs-lookup"><span data-stu-id="f74bc-119">&emsp;&emsp;6.</span></span>   <span data-ttu-id="f74bc-120">V okně **Nastavení** vyberte **Notification Services** a pak **Google (GCM)**.</span><span class="sxs-lookup"><span data-stu-id="f74bc-120">In the **Settings** blade, select **Notification Services** and then **Google (GCM)**.</span></span> <span data-ttu-id="f74bc-121">Zadejte klíč rozhraní API a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f74bc-121">Enter the API key and click **Save**.</span></span>

&emsp;&emsp;![Azure Notification Hubs – Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

<span data-ttu-id="f74bc-123">Vaše centrum oznámení je nyní nakonfigurováno pro práci se službou GCM. Zároveň máte připojovací řetězce, pomocí kterých můžete svou aplikaci zaregistrovat pro příjem a odesílání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="f74bc-123">Your notification hub is now configured to work with GCM, and you have the connection strings to both register your app to receive and send push notifications.</span></span>

## <span data-ttu-id="f74bc-124"><a id="connecting-app"></a>Připojte aplikaci k centru oznámení</span><span class="sxs-lookup"><span data-stu-id="f74bc-124"><a id="connecting-app"></a>Connect your app to the notification hub</span></span>
### <a name="create-a-new-android-project"></a><span data-ttu-id="f74bc-125">Vytvořte nový projekt Android</span><span class="sxs-lookup"><span data-stu-id="f74bc-125">Create a new Android project</span></span>
1. <span data-ttu-id="f74bc-126">V nástroji Android Studio spusťte nový projekt Android Studio.</span><span class="sxs-lookup"><span data-stu-id="f74bc-126">In Android Studio, start a new Android Studio project.</span></span>
   
   ![Android Studio – nový projekt][13]
2. <span data-ttu-id="f74bc-128">Zvolte faktor formuláře **Telefon i tablet** a hodnotu **Minimální SDK**, které chcete podporovat.</span><span class="sxs-lookup"><span data-stu-id="f74bc-128">Choose the **Phone and Tablet** form factor and the **Minimum SDK** that you want to support.</span></span> <span data-ttu-id="f74bc-129">Pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="f74bc-129">Then click **Next**.</span></span>
   
   ![Android Studio – pracovní postup vytvoření projektu][14]
3. <span data-ttu-id="f74bc-131">Zvolte možnost **Prázdná aktivita** pro hlavní aktivitu, klikněte na tlačítko **Další** a pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="f74bc-131">Choose **Empty Activity** for the main activity, click **Next**, and then click **Finish**.</span></span>

### <a name="add-google-play-services-to-the-project"></a><span data-ttu-id="f74bc-132">Přidejte do projektu služby Google Play</span><span class="sxs-lookup"><span data-stu-id="f74bc-132">Add Google Play services to the project</span></span>
[!INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

### <a name="adding-azure-notification-hubs-libraries"></a><span data-ttu-id="f74bc-133">Přidání knihoven Azure Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="f74bc-133">Adding Azure Notification Hubs libraries</span></span>
1. <span data-ttu-id="f74bc-134">Do souboru `Build.Gradle` pro **aplikaci** přidejte následující řádky v části **závislosti**.</span><span class="sxs-lookup"><span data-stu-id="f74bc-134">In the `Build.Gradle` file for the **app**, add the following lines in the **dependencies** section.</span></span>
   
        compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
        compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
2. <span data-ttu-id="f74bc-135">Přidejte následující úložiště za část **závislosti**.</span><span class="sxs-lookup"><span data-stu-id="f74bc-135">Add the following repository after the **dependencies** section.</span></span>
   
        repositories {
            maven {
                url "http://dl.bintray.com/microsoftazuremobile/SDK"
            }
        }

### <a name="updating-the-androidmanifestxml"></a><span data-ttu-id="f74bc-136">Probíhá aktualizace souboru AndroidManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="f74bc-136">Updating the AndroidManifest.xml.</span></span>
1. <span data-ttu-id="f74bc-137">Pro podporu GCM musíme implementovat ID instanci procesu naslouchání služby v našem kódu, který se používá k [získání registrace tokenů](https://developers.google.com/cloud-messaging/android/client#sample-register) pomocí [rozhraní API ID instance Google](https://developers.google.com/instance-id/).</span><span class="sxs-lookup"><span data-stu-id="f74bc-137">To support GCM, we must implement a Instance ID listener service in our code which is used to [obtain registration tokens](https://developers.google.com/cloud-messaging/android/client#sample-register) using [Google's Instance ID API](https://developers.google.com/instance-id/).</span></span> <span data-ttu-id="f74bc-138">V tomto kurzu pojmenujeme třídu `MyInstanceIDService`.</span><span class="sxs-lookup"><span data-stu-id="f74bc-138">In this tutorial we will name the class `MyInstanceIDService`.</span></span> 
   
    <span data-ttu-id="f74bc-139">Přidejte následující definice služby do souboru AndroidManifest.xml uvnitř značky `<application>`.</span><span class="sxs-lookup"><span data-stu-id="f74bc-139">Add the following service definition to the AndroidManifest.xml file, inside the `<application>` tag.</span></span> <span data-ttu-id="f74bc-140">Nahraďte zástupný symbol `<your package>` pomocí skutečného názvu balíčku zobrazeného v horní části souboru `AndroidManifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="f74bc-140">Replace the `<your package>` placeholder with the your actual package name shown at the top of the `AndroidManifest.xml` file.</span></span>
   
        <service android:name="<your package>.MyInstanceIDService" android:exported="false">
            <intent-filter>
                <action android:name="com.google.android.gms.iid.InstanceID"/>
            </intent-filter>
        </service>
2. <span data-ttu-id="f74bc-141">Po obdržení tokenu registrace GCM z rozhraní Instance ID API ho použijeme k [registraci do Centra oznámení Azure](notification-hubs-push-notification-registration-management.md).</span><span class="sxs-lookup"><span data-stu-id="f74bc-141">Once we have received our GCM registration token from the Instance ID API, we will use it to [register with the Azure Notification Hub](notification-hubs-push-notification-registration-management.md).</span></span> <span data-ttu-id="f74bc-142">Tuto registrace podpoříme na pozadí pomocí `IntentService` s názvem `RegistrationIntentService`.</span><span class="sxs-lookup"><span data-stu-id="f74bc-142">We will support this registration in the background using an `IntentService` named `RegistrationIntentService`.</span></span> <span data-ttu-id="f74bc-143">Tato služba bude také zodpovědná za [aktualizace tokenu registrace GCM](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens).</span><span class="sxs-lookup"><span data-stu-id="f74bc-143">This service will also be responsible for [refreshing our GCM registration token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens).</span></span>
   
    <span data-ttu-id="f74bc-144">Přidejte následující definice služby do souboru AndroidManifest.xml uvnitř značky `<application>`.</span><span class="sxs-lookup"><span data-stu-id="f74bc-144">Add the following service definition to the AndroidManifest.xml file, inside the `<application>` tag.</span></span> <span data-ttu-id="f74bc-145">Nahraďte zástupný symbol `<your package>` pomocí skutečného názvu balíčku zobrazeného v horní části souboru `AndroidManifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="f74bc-145">Replace the `<your package>` placeholder with the your actual package name shown at the top of the `AndroidManifest.xml` file.</span></span> 
   
        <service
            android:name="<your package>.RegistrationIntentService"
            android:exported="false">
        </service>
3. <span data-ttu-id="f74bc-146">Také definujeme příjemce pro příjem oznámení.</span><span class="sxs-lookup"><span data-stu-id="f74bc-146">We will also define a receiver to receive notifications.</span></span> <span data-ttu-id="f74bc-147">Přidejte následující definice příjemce do souboru AndroidManifest.xml uvnitř značky `<application>`.</span><span class="sxs-lookup"><span data-stu-id="f74bc-147">Add the following receiver definition to the AndroidManifest.xml file, inside the `<application>` tag.</span></span> <span data-ttu-id="f74bc-148">Nahraďte zástupný symbol `<your package>` pomocí skutečného názvu balíčku zobrazeného v horní části souboru `AndroidManifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="f74bc-148">Replace the `<your package>` placeholder with the your actual package name shown at the top of the `AndroidManifest.xml` file.</span></span>
   
        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your package name>" />
            </intent-filter>
        </receiver>
4. <span data-ttu-id="f74bc-149">Přidejte následující nezbytná oprávnění související s GCM pod značkou `</application>`.</span><span class="sxs-lookup"><span data-stu-id="f74bc-149">Add the following necessary GCM related permissions below the  `</application>` tag.</span></span> <span data-ttu-id="f74bc-150">Nezapomeňte nahradit `<your package>` názvem balíčku, který je zobrazen v horní části souboru `AndroidManifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="f74bc-150">Make sure to replace `<your package>` with the package name shown at the top of the `AndroidManifest.xml` file.</span></span>
   
    <span data-ttu-id="f74bc-151">Další informace o těchto oprávnění naleznete v tématu [Nastavení klientské aplikace GCM pro Android](https://developers.google.com/cloud-messaging/android/client#manifest).</span><span class="sxs-lookup"><span data-stu-id="f74bc-151">For more information on these permissions, see [Setup a GCM Client app for Android](https://developers.google.com/cloud-messaging/android/client#manifest).</span></span>
   
        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
   
        <permission android:name="<your package>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
        <uses-permission android:name="<your package>.permission.C2D_MESSAGE"/>

### <a name="adding-code"></a><span data-ttu-id="f74bc-152">Přidání kódu</span><span class="sxs-lookup"><span data-stu-id="f74bc-152">Adding code</span></span>
1. <span data-ttu-id="f74bc-153">V zobrazení projektu rozbalte **app** > **src** > **main** > **java**.</span><span class="sxs-lookup"><span data-stu-id="f74bc-153">In the Project View, expand **app** > **src** > **main** > **java**.</span></span> <span data-ttu-id="f74bc-154">Klikněte pravým tlačítkem na váš balíček ve složce **java**, klikněte na tlačítko **Nový** a pak klikněte na tlačítko **třída jazyka Java**.</span><span class="sxs-lookup"><span data-stu-id="f74bc-154">Right-click your package folder under **java**, click **New**, and then click **Java Class**.</span></span> <span data-ttu-id="f74bc-155">Přidejte novou třídu s názvem `NotificationSettings`.</span><span class="sxs-lookup"><span data-stu-id="f74bc-155">Add a new class named `NotificationSettings`.</span></span> 
   
    ![Android Studio – nová třída Java][6]
   
    <span data-ttu-id="f74bc-157">Nezapomeňte aktualizovat tyto tři zástupné symboly v následujícím kódu pro třídu `NotificationSettings`:</span><span class="sxs-lookup"><span data-stu-id="f74bc-157">Make sure to update the these three placeholders in the following code for the `NotificationSettings` class:</span></span>
   
   * <span data-ttu-id="f74bc-158">**ID odesílatele**: číslo projektu, které jste získali výše u [konzoly Google Cloud](http://cloud.google.com/console).</span><span class="sxs-lookup"><span data-stu-id="f74bc-158">**SenderId**: The project number you obtained earlier in the [Google Cloud Console](http://cloud.google.com/console).</span></span>
   * <span data-ttu-id="f74bc-159">**HubListenConnectionString**: připojovací řetězec **DefaultListenAccessSignature** pro rozbočovač.</span><span class="sxs-lookup"><span data-stu-id="f74bc-159">**HubListenConnectionString**: The **DefaultListenAccessSignature** connection string for your hub.</span></span> <span data-ttu-id="f74bc-160">Tento připojovací řetězec můžete zkopírovat kliknutím na položku **Zásady přístupu** v okně **Nastavení** rozbočovače na [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="f74bc-160">You can copy that connection string by clicking **Access Policies** on the **Settings** blade of your hub on the [Azure Portal].</span></span>
   * <span data-ttu-id="f74bc-161">**HubName**: použije název centra oznámení, který se zobrazí v centru okna na webu [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="f74bc-161">**HubName**: Use the name of your notification hub that appears in the hub blade in the [Azure Portal].</span></span>
     
     <span data-ttu-id="f74bc-162">Kód `NotificationSettings`:</span><span class="sxs-lookup"><span data-stu-id="f74bc-162">`NotificationSettings` code:</span></span>
     
       <span data-ttu-id="f74bc-163">public class NotificationSettings {</span><span class="sxs-lookup"><span data-stu-id="f74bc-163">public class NotificationSettings {</span></span>
     
           public static String SenderId = "<Your project number>";
           public static String HubName = "<Your HubName>";
           public static String HubListenConnectionString = "<Your default listen connection string>";
       <span data-ttu-id="f74bc-164">}</span><span class="sxs-lookup"><span data-stu-id="f74bc-164">}</span></span>
2. <span data-ttu-id="f74bc-165">Pomocí kroků výše přidejte další novou třídu s názvem `MyInstanceIDService`.</span><span class="sxs-lookup"><span data-stu-id="f74bc-165">Using the steps above, add another new class named `MyInstanceIDService`.</span></span> <span data-ttu-id="f74bc-166">Toto bude naše implementace služby procesu naslouchání Instance ID.</span><span class="sxs-lookup"><span data-stu-id="f74bc-166">This will be our Instance ID listener service implementation.</span></span>
   
    <span data-ttu-id="f74bc-167">Kód pro tuto třídu bude volat naše `IntentService` k [obnovení tokenu GCM](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) na pozadí.</span><span class="sxs-lookup"><span data-stu-id="f74bc-167">The code for this class will call our `IntentService` to [refresh the GCM token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) in the background.</span></span>
   
        import android.content.Intent;
        import android.util.Log;
        import com.google.android.gms.iid.InstanceIDListenerService;

        public class MyInstanceIDService extends InstanceIDListenerService {

            private static final String TAG = "MyInstanceIDService";

            @Override
            public void onTokenRefresh() {

                Log.i(TAG, "Refreshing GCM Registration Token");

                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        };


1. <span data-ttu-id="f74bc-168">Přidejte další novou třídu do projektu s názvem, `RegistrationIntentService`.</span><span class="sxs-lookup"><span data-stu-id="f74bc-168">Add another new class to your project named, `RegistrationIntentService`.</span></span> <span data-ttu-id="f74bc-169">Toto bude implementace pro službu `IntentService`, která zpracuje [aktualizaci tokenu GCM](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) a [registraci do centra oznámení](notification-hubs-push-notification-registration-management.md).</span><span class="sxs-lookup"><span data-stu-id="f74bc-169">This will be the implementation for our `IntentService` that will handle [refreshing the GCM token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) and [registering with the notification hub](notification-hubs-push-notification-registration-management.md).</span></span>
   
    <span data-ttu-id="f74bc-170">Pro tuto třídu použijte následující kód.</span><span class="sxs-lookup"><span data-stu-id="f74bc-170">Use the following code for this class.</span></span>
   
        import android.app.IntentService;
        import android.content.Intent;
        import android.content.SharedPreferences;
        import android.preference.PreferenceManager;
        import android.util.Log;
   
        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.google.android.gms.iid.InstanceID;
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
   
                try {
                    InstanceID instanceID = InstanceID.getInstance(this);
                    String token = instanceID.getToken(NotificationSettings.SenderId,
                            GoogleCloudMessaging.INSTANCE_ID_SCOPE);        
                    Log.i(TAG, "Got GCM Registration Token: " + token);
   
                    // Storing the registration id that indicates whether the generated token has been
                    // sent to your server. If it is not stored, send the token to your server,
                    // otherwise your server should have already received the token.
                    if ((regID=sharedPreferences.getString("registrationID", null)) == null) {        
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.i(TAG, "Attempting to register with NH using token : " + token);
   
                        regID = hub.register(token).getRegistrationId();
   
                        // If you want to use tags...
                        // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1", "tag2").getRegistrationId();
   
                        resultString = "Registered Successfully - RegId : " + regID;
                        Log.i(TAG, resultString);        
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                    } else {
                        resultString = "Previously Registered Successfully - RegId : " + regID;
                    }
                } catch (Exception e) {
                    Log.e(TAG, resultString="Failed to complete token refresh", e);
                    // If an exception happens while fetching the new token or updating our registration data
                    // on a third-party server, this ensures that we'll attempt the update at a later time.
                }
   
                // Notify UI that registration has completed.
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(resultString);
                }
            }
        }
2. <span data-ttu-id="f74bc-171">Do vaší třídy `MainActivity` přidejte následující prohlášení `import` nad deklaraci třídy.</span><span class="sxs-lookup"><span data-stu-id="f74bc-171">In your `MainActivity` class, add the following `import` statements above the class declaration.</span></span>
   
        import com.google.android.gms.common.ConnectionResult;
        import com.google.android.gms.common.GoogleApiAvailability;
        import com.google.android.gms.gcm.*;
        import com.microsoft.windowsazure.notifications.NotificationsManager;
        import android.util.Log;
        import android.widget.TextView;
        import android.widget.Toast;
3. <span data-ttu-id="f74bc-172">Přidejte následující soukromé členy v horní části třídy.</span><span class="sxs-lookup"><span data-stu-id="f74bc-172">Add the following private members at the top of the class.</span></span> <span data-ttu-id="f74bc-173">Tyto [budeme používat ke kontrole dostupnosti služby Google Play dle doporučení Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).</span><span class="sxs-lookup"><span data-stu-id="f74bc-173">We will use these [check the availability of Google Play Services as recommended by Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).</span></span>
   
        public static MainActivity mainActivity;
        public static Boolean isVisible = false;    
        private GoogleCloudMessaging gcm;
        private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;
4. <span data-ttu-id="f74bc-174">Ve své třídě `MainActivity` přidejte následující metodu pro dostupnost služeb Google Play.</span><span class="sxs-lookup"><span data-stu-id="f74bc-174">In your `MainActivity` class, add the following method to the availability of Google Play Services.</span></span> 
   
        /**
         * Check the device to make sure it has the Google Play Services APK. If
         * it doesn't, display a dialog that allows users to download the APK from
         * the Google Play Store or enable it in the device's system settings.
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
5. <span data-ttu-id="f74bc-175">Ve své třídě `MainActivity` přidejte následující kód, který zkontrolujte služby Google Play před voláním vašeho `IntentService` pro získání tokenu registrace GCM a registraci pomocí vašeho centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="f74bc-175">In your `MainActivity` class, add the following code that will check for Google Play Services before calling your `IntentService` to get your GCM registration token and register with your notification hub.</span></span>
   
        public void registerWithNotificationHubs()
        {
            Log.i(TAG, " Registering with Notification Hubs");
   
            if (checkPlayServices()) {
                // Start IntentService to register this application with GCM.
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        }
6. <span data-ttu-id="f74bc-176">Do metody `OnCreate` třídy `MainActivity` přidejte následující kód pro spuštění procesu registrace při vytvoření aktivity.</span><span class="sxs-lookup"><span data-stu-id="f74bc-176">In the `OnCreate` method of the `MainActivity` class, add the following code to start the registration process when activity is created.</span></span>
   
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
   
            mainActivity = this;
            NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
            registerWithNotificationHubs();
        }
7. <span data-ttu-id="f74bc-177">Přidejte  tyto další metody do `MainActivity` pro ověření stavu aplikace a stavu sestavy ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f74bc-177">Add these additional methods to the `MainActivity` to verify app state and report status in your app.</span></span>
   
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
8. <span data-ttu-id="f74bc-178">Metoda `ToastNotify` používá ovládání *„Hello World“* `TextView` k trvalému hlášení stavu a oznámení v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f74bc-178">The `ToastNotify` method uses the *"Hello World"* `TextView` control to report status and notifications persistently in the app.</span></span> <span data-ttu-id="f74bc-179">Do rozložení activity_main.xml přidejte následující id pro ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="f74bc-179">In your activity_main.xml layout, add the following id for that control.</span></span>
   
       android:id="@+id/text_hello"
9. <span data-ttu-id="f74bc-180">Vedle přidáme podtřídu pro našeho příjemce, kterého jsme definovali v souboru AndroidManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="f74bc-180">Next we will add a subclass for our receiver we defined in the AndroidManifest.xml.</span></span> <span data-ttu-id="f74bc-181">Přidejte další novou třídu do projektu s názvem `MyHandler`.</span><span class="sxs-lookup"><span data-stu-id="f74bc-181">Add another new class to your project named `MyHandler`.</span></span>
10. <span data-ttu-id="f74bc-182">Nad `MyHandler.java` přidejte následující příkazy pro import:</span><span class="sxs-lookup"><span data-stu-id="f74bc-182">Add the following import statements at the top of `MyHandler.java`:</span></span>
    
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
        import com.microsoft.windowsazure.notifications.NotificationsHandler;
11. <span data-ttu-id="f74bc-183">Přidejte následující kód pro třídu `MyHandler` a vytvořte tak podtřídu `com.microsoft.windowsazure.notifications.NotificationsHandler`.</span><span class="sxs-lookup"><span data-stu-id="f74bc-183">Add the following code for the `MyHandler` class making it a subclass of `com.microsoft.windowsazure.notifications.NotificationsHandler`.</span></span>
    
    <span data-ttu-id="f74bc-184">Tento kód přepíše metodu `OnReceive`, aby obslužná rutina nahlásila oznámení, která byla přijata.</span><span class="sxs-lookup"><span data-stu-id="f74bc-184">This code overrides the `OnReceive` method, so the handler will report notifications that are received.</span></span> <span data-ttu-id="f74bc-185">Obslužná rutina také odesílá nabízená oznámení správci oznámení Android pomocí metody `sendNotification()`.</span><span class="sxs-lookup"><span data-stu-id="f74bc-185">The handler also sends the push notification to the Android notification manager by using the `sendNotification()` method.</span></span> <span data-ttu-id="f74bc-186">Metoda `sendNotification()` musí být spouštěna, když není aplikace spuštěna a není přijato oznámení.</span><span class="sxs-lookup"><span data-stu-id="f74bc-186">The `sendNotification()` method should be executed when the app is not running and a notification is received.</span></span>
    
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
12. <span data-ttu-id="f74bc-187">V Android Studio na řádku nabídek klikněte na tlačítko **Sestavit** > **Znovu sestavit projekt** a ujistěte se, zda se ve vašem kódu nenachází žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="f74bc-187">In Android Studio on the menu bar, click **Build** > **Rebuild Project** to make sure that no errors are present in your code.</span></span>

## <a name="sending-push-notifications"></a><span data-ttu-id="f74bc-188">Odeslání nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="f74bc-188">Sending push notifications</span></span>
<span data-ttu-id="f74bc-189">Můžete otestovat přijímání nabízených oznámení ve vaší aplikaci jejich odesláním prostřednictvím [Azure Portal] – hledejte část **Poradce při potížích** v okně centra, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="f74bc-189">You can test receiving push notifications in your app by sending them via the [Azure Portal] - look for the **Troubleshooting** Section in the hub blade, as shown below.</span></span>

![Azure Notification Hubs – testovací odeslání](./media/notification-hubs-android-get-started/notification-hubs-test-send.png)

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-directly-from-the-app"></a><span data-ttu-id="f74bc-191">(Volitelné) Zasílání nabízených oznámení přímo z aplikace</span><span class="sxs-lookup"><span data-stu-id="f74bc-191">(Optional) Send push notifications directly from the app</span></span>
<span data-ttu-id="f74bc-192">Za normálních okolností byste odesílali oznámení pomocí serveru backend.</span><span class="sxs-lookup"><span data-stu-id="f74bc-192">Normally, you would send notifications using a backend server.</span></span> <span data-ttu-id="f74bc-193">V některých případech můžete chtít možnost zasílání nabízených oznámení přímo z klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="f74bc-193">For some cases, you might want to be able to send push notifications directly from the client application.</span></span> <span data-ttu-id="f74bc-194">Tato část vysvětluje postup odesílání oznámení z klienta pomocí [API služby REST centra oznámení Azure](https://msdn.microsoft.com/library/azure/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="f74bc-194">This section explains how to send notifications from the client using the [Azure Notification Hub REST API](https://msdn.microsoft.com/library/azure/dn223264.aspx).</span></span>

1. <span data-ttu-id="f74bc-195">V zobrazení projektu Android Studio rozbalte možnost **App** > **src** > **main** > **res** > **layout**.</span><span class="sxs-lookup"><span data-stu-id="f74bc-195">In Android Studio Project View, expand **App** > **src** > **main** > **res** > **layout**.</span></span> <span data-ttu-id="f74bc-196">Otevřete soubor rozložení `activity_main.xml` a klikněte na kartu **Text** pro aktualizaci textového obsahu souboru.</span><span class="sxs-lookup"><span data-stu-id="f74bc-196">Open the `activity_main.xml` layout file and click the **Text** tab to update the text contents of the file.</span></span> <span data-ttu-id="f74bc-197">Aktualizujte ho pomocí kódu níže, který přidává nové `Button` a `EditText` ovládací prvky pro zasílání zpráv s nabízeným oznámením centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="f74bc-197">Update it with the code below, which adds new `Button` and `EditText` controls for sending push notification messages to the notification hub.</span></span> <span data-ttu-id="f74bc-198">Přidejte tento kód v dolní části, těsně před `</RelativeLayout>`.</span><span class="sxs-lookup"><span data-stu-id="f74bc-198">Add this code at the bottom, just before `</RelativeLayout>`.</span></span>
   
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
2. <span data-ttu-id="f74bc-199">V zobrazení projektu Android Studio rozbalte **App** > **src** > **main** > **res** > **values**.</span><span class="sxs-lookup"><span data-stu-id="f74bc-199">In Android Studio Project View, expand **App** > **src** > **main** > **res** > **values**.</span></span> <span data-ttu-id="f74bc-200">Otevřete soubor `strings.xml` a přidejte řetězcové hodnoty, které jsou odkazovány pomocí nového `Button` a `EditText` ovládacími prvky.</span><span class="sxs-lookup"><span data-stu-id="f74bc-200">Open the `strings.xml` file and add the string values that are referenced by the new `Button` and `EditText` controls.</span></span> <span data-ttu-id="f74bc-201">Přidejte tyto položky ve spodní části souboru, těsně před `</resources>`.</span><span class="sxs-lookup"><span data-stu-id="f74bc-201">Add these at the bottom of the file, just before `</resources>`.</span></span>
   
        <string name="send_button">Send Notification</string>
        <string name="notification_message_hint">Enter notification message text</string>
3. <span data-ttu-id="f74bc-202">Do souboru `NotificationSetting.java` přidejte následující nastavení na třídu `NotificationSettings`.</span><span class="sxs-lookup"><span data-stu-id="f74bc-202">In your `NotificationSetting.java` file, add the following setting to the `NotificationSettings` class.</span></span>
   
    <span data-ttu-id="f74bc-203">Aktualizujte `HubFullAccess` pomocí připojovacího řetězce **DefaultFullSharedAccessSignature** pro centrum.</span><span class="sxs-lookup"><span data-stu-id="f74bc-203">Update `HubFullAccess` with the **DefaultFullSharedAccessSignature** connection string for your hub.</span></span> <span data-ttu-id="f74bc-204">Tento připojovací řetězec lze kopírovat z [Azure Portal] kliknutím na položku **zásady přístupu** v okně **Nastavení** pro vaše centrum oznámení.</span><span class="sxs-lookup"><span data-stu-id="f74bc-204">This connection string can be copied from the [Azure Portal] by clicking **Access Policies** on the **Settings** blade for your notification hub.</span></span>
   
        public static String HubFullAccess = "<Enter Your DefaultFullSharedAccess Connection string>";
4. <span data-ttu-id="f74bc-205">Do souboru `MainActivity.java` přidejte následující prohlášení `import` nad třídu `MainActivity`.</span><span class="sxs-lookup"><span data-stu-id="f74bc-205">In your `MainActivity.java` file, add the following `import` statements above the `MainActivity` class.</span></span>
   
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
5. <span data-ttu-id="f74bc-206">Do souboru `MainActivity.java` přidejte následující členy v horní části třídy `MainActivity`.</span><span class="sxs-lookup"><span data-stu-id="f74bc-206">In your `MainActivity.java` file, add the following members at the top of the `MainActivity` class.</span></span>    
   
        private String HubEndpoint = null;
        private String HubSasKeyName = null;
        private String HubSasKeyValue = null;
6. <span data-ttu-id="f74bc-207">Je třeba vytvořit token SaS (Software Access Signature) k ověření požadavku POST k odesílání zpráv do vašeho centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="f74bc-207">You must create a Software Access Signature (SaS) token to authenticate a POST request to send messages to your notification hub.</span></span> <span data-ttu-id="f74bc-208">To se provede analýzou klíčových dat z připojovacího řetězce a pak vytvořením SaS tokenu, jak je uvedeno v referenci rozhraní REST API [Běžné koncepty](http://msdn.microsoft.com/library/azure/dn495627.aspx).</span><span class="sxs-lookup"><span data-stu-id="f74bc-208">This is done by parsing the key data from the connection string and then creating the SaS token, as mentioned in the [Common Concepts](http://msdn.microsoft.com/library/azure/dn495627.aspx) REST API reference.</span></span> <span data-ttu-id="f74bc-209">Následující kód představuje příklad implementace.</span><span class="sxs-lookup"><span data-stu-id="f74bc-209">The following code is an example implementation.</span></span>
   
    <span data-ttu-id="f74bc-210">Do `MainActivity.java` přidejte následující metodu do třídy `MainActivity` k analýze připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="f74bc-210">In `MainActivity.java`, add the following method to the `MainActivity` class to parse your connection string.</span></span>
   
        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx
         * to parse the connection string so a SaS authentication token can be
         * constructed.
         *
         * @param connectionString This must be the DefaultFullSharedAccess connection
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
7. <span data-ttu-id="f74bc-211">Do `MainActivity.java` přidejte následující metodu do třídy `MainActivity` k vytvoření ověřovacího tokenu SaS.</span><span class="sxs-lookup"><span data-stu-id="f74bc-211">In `MainActivity.java`, add the following method to the `MainActivity` class to create a SaS authentication token.</span></span>
   
        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx to
         * construct a SaS token from the access key to authenticate a request.
         *
         * @param uri The unencoded resource URI string for this operation. The resource
         *            URI is the full URI of the Service Bus resource to which access is
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
   
                // Get an hmac_sha1 key from the raw key bytes
                byte[] keyBytes = HubSasKeyValue.getBytes("UTF-8");
                SecretKeySpec signingKey = new SecretKeySpec(keyBytes, "HmacSHA256");
   
                // Get an hmac_sha1 Mac instance and initialize with the signing key
                Mac mac = Mac.getInstance("HmacSHA256");
                mac.init(signingKey);
   
                // Compute the hmac on input data bytes
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
8. <span data-ttu-id="f74bc-212">Do `MainActivity.java` přidejte následující metodu do třídy `MainActivity` pro zajištění kliknutí na tlačítko **Odeslat oznámení** a odešlete zprávu nabízeného oznámení do centra pomocí předdefinovaného REST API.</span><span class="sxs-lookup"><span data-stu-id="f74bc-212">In `MainActivity.java`, add the following method to the `MainActivity` class to handle the **Send Notification** button click and send the push notification message to the hub by using the built-in REST API.</span></span>
   
        /**
         * Send Notification button click handler. This method parses the
         * DefaultFullSharedAccess connection string and generates a SaS token. The
         * token is added to the Authorization header on the POST request to the
         * notification hub. The text in the editTextNotificationMessage control
         * is added as the JSON body for the request to add a GCM message to the hub.
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
   
                            // Authenticate the POST request with the SaS token
                            urlConnection.setRequestProperty("Authorization", 
                                generateSasToken(url.toString()));
   
                            // Notification format should be GCM
                            urlConnection.setRequestProperty("ServiceBusNotification-Format", "gcm");
   
                            // Include any tags
                            // Example below targets 3 specific tags
                            // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
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

## <a name="testing-your-app"></a><span data-ttu-id="f74bc-213">Testování vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="f74bc-213">Testing your app</span></span>
#### <a name="push-notifications-in-the-emulator"></a><span data-ttu-id="f74bc-214">Nabízená oznámení v emulátoru</span><span class="sxs-lookup"><span data-stu-id="f74bc-214">Push notifications in the emulator</span></span>
<span data-ttu-id="f74bc-215">Pokud chcete testovat nabízená oznámení uvnitř emulátoru, ověřte, zda bitová kopie emulátoru podporuje úroveň rozhraní Google API, kterou jste zvolili pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f74bc-215">If you want to test push notifications inside an emulator, make sure that your emulator image supports the Google API level that you chose for your app.</span></span> <span data-ttu-id="f74bc-216">Pokud bitová kopie nepodporuje nativní rozhraní Google API, zobrazí se výjimka **SLUŽBA\_NENÍ\_K DISPOZICI**.</span><span class="sxs-lookup"><span data-stu-id="f74bc-216">If your image doesn't support native Google APIs, you will end up with the **SERVICE\_NOT\_AVAILABLE** exception.</span></span>

<span data-ttu-id="f74bc-217">Kromě výše uvedeného zajistěte, že jste přidali účet Google do svého spuštěného emulátoru pod položkou **Nastavení** > **účtů**.</span><span class="sxs-lookup"><span data-stu-id="f74bc-217">In addition to the above, ensure that you have added your Google account to your running emulator under **Settings** > **Accounts**.</span></span> <span data-ttu-id="f74bc-218">V opačném případě mohou vaše pokusy o registraci s GCM mít za následek výjimku **OVĚŘOVÁNÍ\_SE NEZDAŘILO**.</span><span class="sxs-lookup"><span data-stu-id="f74bc-218">Otherwise, your attempts to register with GCM may result in the **AUTHENTICATION\_FAILED** exception.</span></span>

#### <a name="running-the-application"></a><span data-ttu-id="f74bc-219">Spouštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="f74bc-219">Running the application</span></span>
1. <span data-ttu-id="f74bc-220">Spusťte aplikaci a všimněte si, že je ID registrace hlášené pro úspěšnou registraci.</span><span class="sxs-lookup"><span data-stu-id="f74bc-220">Run the app and notice that the registration ID is reported for a successful registration.</span></span>
   
      ![Testování v systému Android – registrace kanálu][18]
2. <span data-ttu-id="f74bc-222">Zadejte zprávu oznámení k odeslání do všech zařízení Android, která byla zaregistrovaná v centru.</span><span class="sxs-lookup"><span data-stu-id="f74bc-222">Enter a notification message to be sent to all Android devices that have registered with the hub.</span></span>
   
      ![Testování v systému Android – odesílání zprávy][19]

3. <span data-ttu-id="f74bc-224">Stiskněte tlačítko **Odeslat oznámení**.</span><span class="sxs-lookup"><span data-stu-id="f74bc-224">Press **Send Notification**.</span></span> <span data-ttu-id="f74bc-225">Všechna zařízení, které mají spuštěné aplikace, zobrazí instance `AlertDialog` se zprávou nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="f74bc-225">Any devices that have the app running will show an `AlertDialog` instance with the push notification message.</span></span> <span data-ttu-id="f74bc-226">Zařízení, která nemají spuštěnou aplikaci, ale byla dříve registrována pro nabízená oznámení, obdrží oznámení ve správci oznámení Android.</span><span class="sxs-lookup"><span data-stu-id="f74bc-226">Devices that don't have the app running but were previously registered for push notifications will receive a notification in the Android Notification Manager.</span></span> <span data-ttu-id="f74bc-227">Ta lze zobrazit potažením dolů z levého horního rohu.</span><span class="sxs-lookup"><span data-stu-id="f74bc-227">Those can be viewed by swiping down from the upper-left corner.</span></span>
   
      ![Testování v systému Android – oznámení][21]

## <a name="next-steps"></a><span data-ttu-id="f74bc-229">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f74bc-229">Next steps</span></span>
<span data-ttu-id="f74bc-230">Jako další krok doporučujeme tutoriál [Použití centra oznámení pro nabízená oznámení uživatelům].</span><span class="sxs-lookup"><span data-stu-id="f74bc-230">We recommend the [Use Notification Hubs to push notifications to users] tutorial as the next step.</span></span> <span data-ttu-id="f74bc-231">Zobrazí se postup odesílání oznámení z ASP.NET back-end pomocí značek pro cílové konkrétní uživatele.</span><span class="sxs-lookup"><span data-stu-id="f74bc-231">It will show you how to send notifications from an ASP.NET backend using tags to target specific users.</span></span>

<span data-ttu-id="f74bc-232">Pokud chcete segmentovat uživatele podle zájmových skupin, podívejte se na tutoriál [Používání centra oznámení k odesílání novinek].</span><span class="sxs-lookup"><span data-stu-id="f74bc-232">If you want to segment your users by interest groups, check out the [Use Notification Hubs to send breaking news] tutorial.</span></span>

<span data-ttu-id="f74bc-233">Další obecné informace o centrech oznámení naleznete v tématu naše [Pokyny centra oznámení].</span><span class="sxs-lookup"><span data-stu-id="f74bc-233">To learn more general information about Notification Hubs, see our [Notification Hubs Guidance].</span></span>

<!-- Images. -->
[6]: ./media/notification-hubs-android-get-started/notification-hub-android-new-class.png

[12]: ./media/notification-hubs-android-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-new-project.png
[14]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-choose-form-factor.png
[15]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app4.png
[16]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app5.png
[17]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app6.png

[18]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-registered.png
[19]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-set-message.png

[20]: ./media/notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-received-message.png
[22]: ./media/notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/notification-hubs-android-get-started/notification-hub-scheduler2.png
[29]: ./media/mobile-services-android-get-started-push/mobile-eclipse-import-Play-library.png

[30]: ./media/notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png

[31]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-add-ui.png


<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Azure Classic Portal]: https://manage.windowsazure.com/
<span data-ttu-id="f74bc-234">[Pokyny centra oznámení]: http://msdn.microsoft.com/library/jj927170.aspx</span><span class="sxs-lookup"><span data-stu-id="f74bc-234">[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx</span></span>
<span data-ttu-id="f74bc-235">[Použití centra oznámení pro nabízená oznámení uživatelům]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md</span><span class="sxs-lookup"><span data-stu-id="f74bc-235">[Use Notification Hubs to push notifications to users]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md</span></span>
<span data-ttu-id="f74bc-236">[Používání centra oznámení k odesílání novinek]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md</span><span class="sxs-lookup"><span data-stu-id="f74bc-236">[Use Notification Hubs to send breaking news]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md</span></span>
<span data-ttu-id="f74bc-237">[Azure Portal]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="f74bc-237">[Azure Portal]: https://portal.azure.com</span></span>
