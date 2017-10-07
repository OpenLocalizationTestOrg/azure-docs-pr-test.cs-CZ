---
title: "aaaSending nabízená oznámení tooAndroid pomocí Azure Notification Hubs | Microsoft Docs"
description: "V tomto kurzu zjistíte, jak toouse Azure Notification Hubs toopush oznámení tooAndroid zařízení."
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
ms.openlocfilehash: 6b15a477d86cf1e6efffb21c5bcef0de7761af79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sending-push-notifications-tooandroid-with-azure-notification-hubs"></a><span data-ttu-id="49baf-104">Odesílání nabízených oznámení tooAndroid pomocí Azure Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="49baf-104">Sending push notifications tooAndroid with Azure Notification Hubs</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="49baf-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="49baf-105">Overview</span></span>
> [!IMPORTANT]
> <span data-ttu-id="49baf-106">Toto téma popisuje nabízená oznámení ve službě Google Cloud Messaging (GCM).</span><span class="sxs-lookup"><span data-stu-id="49baf-106">This topic demonstrates push notifications with Google Cloud Messaging (GCM).</span></span> <span data-ttu-id="49baf-107">Pokud používáte Google zasílání zpráv cloudu Firebase (FCM), přečtěte si téma [odesílající nabízená oznámení tooAndroid pomocí Azure Notification Hubs a FCM](notification-hubs-android-push-notification-google-fcm-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="49baf-107">If you are using Google's Firebase Cloud Messaging (FCM), see [Sending push notifications tooAndroid with Azure Notification Hubs and FCM](notification-hubs-android-push-notification-google-fcm-get-started.md).</span></span>
> 
> 

<span data-ttu-id="49baf-108">Tento kurz ukazuje, jak Azure Notification Hubs toosend toouse nabízená oznámení tooan aplikace pro Android.</span><span class="sxs-lookup"><span data-stu-id="49baf-108">This tutorial shows you how toouse Azure Notification Hubs toosend push notifications tooan Android application.</span></span>
<span data-ttu-id="49baf-109">Vytvoříte prázdnou aplikaci systému Android, která bude přijímat nabízená oznámení pomocí služby GCM (Google Cloud Messaging).</span><span class="sxs-lookup"><span data-stu-id="49baf-109">You'll create a blank Android app that receives push notifications by using Google Cloud Messaging (GCM).</span></span>

[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

<span data-ttu-id="49baf-110">Hello dokončit kód v tomto kurzu lze stáhnout z webu GitHub [zde](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStarted).</span><span class="sxs-lookup"><span data-stu-id="49baf-110">hello completed code for this tutorial can be downloaded from GitHub [here](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStarted).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="49baf-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="49baf-111">Prerequisites</span></span>
> [!IMPORTANT]
> <span data-ttu-id="49baf-112">toocomplete tento kurz, musíte mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="49baf-112">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="49baf-113">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="49baf-113">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="49baf-114">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).</span><span class="sxs-lookup"><span data-stu-id="49baf-114">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).</span></span>
> 
> 

<span data-ttu-id="49baf-115">Kromě toho aktivní účet Azure tooan uvedených výše, tento kurz vyžaduje pouze nejnovější verzi hello [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797).</span><span class="sxs-lookup"><span data-stu-id="49baf-115">In addition tooan active Azure account mentioned above, this tutorial only requires hello latest version of [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797).</span></span>

<span data-ttu-id="49baf-116">Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy Notification Hubs pro Android Apps.</span><span class="sxs-lookup"><span data-stu-id="49baf-116">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for Android apps.</span></span>

## <a name="creating-a-project-that-supports-google-cloud-messaging"></a><span data-ttu-id="49baf-117">Vytvoření projektu, který podporuje službu GCM (Google Cloud Messaging)</span><span class="sxs-lookup"><span data-stu-id="49baf-117">Creating a project that supports Google Cloud Messaging</span></span>
[!INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

## <a name="configure-a-new-notification-hub"></a><span data-ttu-id="49baf-118">Konfigurace nového centra oznámení</span><span class="sxs-lookup"><span data-stu-id="49baf-118">Configure a new notification hub</span></span>
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<span data-ttu-id="49baf-119">&emsp;&emsp;6.</span><span class="sxs-lookup"><span data-stu-id="49baf-119">&emsp;&emsp;6.</span></span>   <span data-ttu-id="49baf-120">V hello **nastavení** vyberte **služby oznámení** a potom **Google (GCM)**.</span><span class="sxs-lookup"><span data-stu-id="49baf-120">In hello **Settings** blade, select **Notification Services** and then **Google (GCM)**.</span></span> <span data-ttu-id="49baf-121">Zadejte klíč hello rozhraní API a klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="49baf-121">Enter hello API key and click **Save**.</span></span>

&emsp;&emsp;![Azure Notification Hubs – Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

<span data-ttu-id="49baf-123">Vaše centrum oznámení je teď nakonfigurovaná toowork s GCM a máte hello připojovací řetězce tooboth registraci vaší aplikace tooreceive a odesílání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="49baf-123">Your notification hub is now configured toowork with GCM, and you have hello connection strings tooboth register your app tooreceive and send push notifications.</span></span>

## <span data-ttu-id="49baf-124"><a id="connecting-app"></a>Připojit vaše Centrum oznámení toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="49baf-124"><a id="connecting-app"></a>Connect your app toohello notification hub</span></span>
### <a name="create-a-new-android-project"></a><span data-ttu-id="49baf-125">Vytvořte nový projekt Android</span><span class="sxs-lookup"><span data-stu-id="49baf-125">Create a new Android project</span></span>
1. <span data-ttu-id="49baf-126">V nástroji Android Studio spusťte nový projekt Android Studio.</span><span class="sxs-lookup"><span data-stu-id="49baf-126">In Android Studio, start a new Android Studio project.</span></span>
   
   ![Android Studio – nový projekt][13]
2. <span data-ttu-id="49baf-128">Zvolte hello **telefon i Tablet** formuláři faktor a hello **minimální SDK** , které chcete toosupport.</span><span class="sxs-lookup"><span data-stu-id="49baf-128">Choose hello **Phone and Tablet** form factor and hello **Minimum SDK** that you want toosupport.</span></span> <span data-ttu-id="49baf-129">Pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="49baf-129">Then click **Next**.</span></span>
   
   ![Android Studio – pracovní postup vytvoření projektu][14]
3. <span data-ttu-id="49baf-131">Zvolte **prázdná aktivita** hello hlavní aktivitu, klikněte na tlačítko **Další**a potom klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="49baf-131">Choose **Empty Activity** for hello main activity, click **Next**, and then click **Finish**.</span></span>

### <a name="add-google-play-services-toohello-project"></a><span data-ttu-id="49baf-132">Přidání projektu toohello služby Google Play</span><span class="sxs-lookup"><span data-stu-id="49baf-132">Add Google Play services toohello project</span></span>
[!INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

### <a name="adding-azure-notification-hubs-libraries"></a><span data-ttu-id="49baf-133">Přidání knihoven Azure Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="49baf-133">Adding Azure Notification Hubs libraries</span></span>
1. <span data-ttu-id="49baf-134">V hello `Build.Gradle` souboru hello **aplikace**, přidejte následující řádky do hello hello **závislosti** části.</span><span class="sxs-lookup"><span data-stu-id="49baf-134">In hello `Build.Gradle` file for hello **app**, add hello following lines in hello **dependencies** section.</span></span>
   
        compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
        compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
2. <span data-ttu-id="49baf-135">Přidejte následující úložiště po hello hello **závislosti** části.</span><span class="sxs-lookup"><span data-stu-id="49baf-135">Add hello following repository after hello **dependencies** section.</span></span>
   
        repositories {
            maven {
                url "http://dl.bintray.com/microsoftazuremobile/SDK"
            }
        }

### <a name="updating-hello-androidmanifestxml"></a><span data-ttu-id="49baf-136">Aktualizace hello AndroidManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="49baf-136">Updating hello AndroidManifest.xml.</span></span>
1. <span data-ttu-id="49baf-137">toosupport GCM musíme implementovat Instance ID naslouchací proces služby v našem kódu, který se používá příliš[získání registrace tokenů](https://developers.google.com/cloud-messaging/android/client#sample-register) pomocí [rozhraní API ID Instance Google](https://developers.google.com/instance-id/).</span><span class="sxs-lookup"><span data-stu-id="49baf-137">toosupport GCM, we must implement a Instance ID listener service in our code which is used too[obtain registration tokens](https://developers.google.com/cloud-messaging/android/client#sample-register) using [Google's Instance ID API](https://developers.google.com/instance-id/).</span></span> <span data-ttu-id="49baf-138">V tomto kurzu pojmenujeme třídu hello `MyInstanceIDService`.</span><span class="sxs-lookup"><span data-stu-id="49baf-138">In this tutorial we will name hello class `MyInstanceIDService`.</span></span> 
   
    <span data-ttu-id="49baf-139">Přidejte následující služby definice toohello souboru AndroidManifest.xml uvnitř hello hello `<application>` značky.</span><span class="sxs-lookup"><span data-stu-id="49baf-139">Add hello following service definition toohello AndroidManifest.xml file, inside hello `<application>` tag.</span></span> <span data-ttu-id="49baf-140">Nahraďte hello `<your package>` zástupný symbol hello vaší skutečného názvu balíčku zobrazeného v horní hello části hello `AndroidManifest.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="49baf-140">Replace hello `<your package>` placeholder with hello your actual package name shown at hello top of hello `AndroidManifest.xml` file.</span></span>
   
        <service android:name="<your package>.MyInstanceIDService" android:exported="false">
            <intent-filter>
                <action android:name="com.google.android.gms.iid.InstanceID"/>
            </intent-filter>
        </service>
2. <span data-ttu-id="49baf-141">Po obdržení tokenu registrace GCM z hello rozhraní API ID Instance ho použijeme příliš[zaregistrovat hello centra oznámení Azure](notification-hubs-push-notification-registration-management.md).</span><span class="sxs-lookup"><span data-stu-id="49baf-141">Once we have received our GCM registration token from hello Instance ID API, we will use it too[register with hello Azure Notification Hub](notification-hubs-push-notification-registration-management.md).</span></span> <span data-ttu-id="49baf-142">Tato registrace podpoříme pomocí pozadí hello `IntentService` s názvem `RegistrationIntentService`.</span><span class="sxs-lookup"><span data-stu-id="49baf-142">We will support this registration in hello background using an `IntentService` named `RegistrationIntentService`.</span></span> <span data-ttu-id="49baf-143">Tato služba bude také zodpovědná za [aktualizace tokenu registrace GCM](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens).</span><span class="sxs-lookup"><span data-stu-id="49baf-143">This service will also be responsible for [refreshing our GCM registration token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens).</span></span>
   
    <span data-ttu-id="49baf-144">Přidejte následující služby definice toohello souboru AndroidManifest.xml uvnitř hello hello `<application>` značky.</span><span class="sxs-lookup"><span data-stu-id="49baf-144">Add hello following service definition toohello AndroidManifest.xml file, inside hello `<application>` tag.</span></span> <span data-ttu-id="49baf-145">Nahraďte hello `<your package>` zástupný symbol hello vaší skutečného názvu balíčku zobrazeného v horní hello části hello `AndroidManifest.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="49baf-145">Replace hello `<your package>` placeholder with hello your actual package name shown at hello top of hello `AndroidManifest.xml` file.</span></span> 
   
        <service
            android:name="<your package>.RegistrationIntentService"
            android:exported="false">
        </service>
3. <span data-ttu-id="49baf-146">Také definujeme tooreceive oznámení příjemce.</span><span class="sxs-lookup"><span data-stu-id="49baf-146">We will also define a receiver tooreceive notifications.</span></span> <span data-ttu-id="49baf-147">Přidejte následující příjemce definice toohello souboru AndroidManifest.xml uvnitř hello hello `<application>` značky.</span><span class="sxs-lookup"><span data-stu-id="49baf-147">Add hello following receiver definition toohello AndroidManifest.xml file, inside hello `<application>` tag.</span></span> <span data-ttu-id="49baf-148">Nahraďte hello `<your package>` zástupný symbol hello vaší skutečného názvu balíčku zobrazeného v horní hello části hello `AndroidManifest.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="49baf-148">Replace hello `<your package>` placeholder with hello your actual package name shown at hello top of hello `AndroidManifest.xml` file.</span></span>
   
        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your package name>" />
            </intent-filter>
        </receiver>
4. <span data-ttu-id="49baf-149">Přidání oprávnění níže hello související s hello následující nezbytné GCM `</application>` značky.</span><span class="sxs-lookup"><span data-stu-id="49baf-149">Add hello following necessary GCM related permissions below hello  `</application>` tag.</span></span> <span data-ttu-id="49baf-150">Ujistěte se, že tooreplace `<your package>` s hello názvu balíčku zobrazeného v horní hello části hello `AndroidManifest.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="49baf-150">Make sure tooreplace `<your package>` with hello package name shown at hello top of hello `AndroidManifest.xml` file.</span></span>
   
    <span data-ttu-id="49baf-151">Další informace o těchto oprávnění naleznete v tématu [Nastavení klientské aplikace GCM pro Android](https://developers.google.com/cloud-messaging/android/client#manifest).</span><span class="sxs-lookup"><span data-stu-id="49baf-151">For more information on these permissions, see [Setup a GCM Client app for Android](https://developers.google.com/cloud-messaging/android/client#manifest).</span></span>
   
        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
   
        <permission android:name="<your package>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
        <uses-permission android:name="<your package>.permission.C2D_MESSAGE"/>

### <a name="adding-code"></a><span data-ttu-id="49baf-152">Přidání kódu</span><span class="sxs-lookup"><span data-stu-id="49baf-152">Adding code</span></span>
1. <span data-ttu-id="49baf-153">V zobrazení projektu hello, rozbalte položku **aplikace** > **src** > **hlavní** > **java**.</span><span class="sxs-lookup"><span data-stu-id="49baf-153">In hello Project View, expand **app** > **src** > **main** > **java**.</span></span> <span data-ttu-id="49baf-154">Klikněte pravým tlačítkem na váš balíček ve složce **java**, klikněte na tlačítko **Nový** a pak klikněte na tlačítko **třída jazyka Java**.</span><span class="sxs-lookup"><span data-stu-id="49baf-154">Right-click your package folder under **java**, click **New**, and then click **Java Class**.</span></span> <span data-ttu-id="49baf-155">Přidejte novou třídu s názvem `NotificationSettings`.</span><span class="sxs-lookup"><span data-stu-id="49baf-155">Add a new class named `NotificationSettings`.</span></span> 
   
    ![Android Studio – nová třída Java][6]
   
    <span data-ttu-id="49baf-157">Zajistěte, aby tooupdate hello tyto tři zástupné symboly v následujícím kódu pro hello hello `NotificationSettings` třídy:</span><span class="sxs-lookup"><span data-stu-id="49baf-157">Make sure tooupdate hello these three placeholders in hello following code for hello `NotificationSettings` class:</span></span>
   
   * <span data-ttu-id="49baf-158">**ID odesílatele**: hello číslo projektu, které jste získali dříve v hello [Google Cloud Console](http://cloud.google.com/console).</span><span class="sxs-lookup"><span data-stu-id="49baf-158">**SenderId**: hello project number you obtained earlier in hello [Google Cloud Console](http://cloud.google.com/console).</span></span>
   * <span data-ttu-id="49baf-159">**HubListenConnectionString**: hello **DefaultListenAccessSignature** připojovací řetězec pro vaše centrum.</span><span class="sxs-lookup"><span data-stu-id="49baf-159">**HubListenConnectionString**: hello **DefaultListenAccessSignature** connection string for your hub.</span></span> <span data-ttu-id="49baf-160">Tento připojovací řetězec můžete zkopírovat kliknutím **zásady přístupu** na hello **nastavení** rozbočovače na hello [portálu Azure].</span><span class="sxs-lookup"><span data-stu-id="49baf-160">You can copy that connection string by clicking **Access Policies** on hello **Settings** blade of your hub on hello [Azure Portal].</span></span>
   * <span data-ttu-id="49baf-161">**HubName**: použití hello název centra oznámení, který se zobrazí v centru okna hello hello [portálu Azure].</span><span class="sxs-lookup"><span data-stu-id="49baf-161">**HubName**: Use hello name of your notification hub that appears in hello hub blade in hello [Azure Portal].</span></span>
     
     <span data-ttu-id="49baf-162">Kód `NotificationSettings`:</span><span class="sxs-lookup"><span data-stu-id="49baf-162">`NotificationSettings` code:</span></span>
     
       <span data-ttu-id="49baf-163">public class NotificationSettings {</span><span class="sxs-lookup"><span data-stu-id="49baf-163">public class NotificationSettings {</span></span>
     
           public static String SenderId = "<Your project number>";
           public static String HubName = "<Your HubName>";
           public static String HubListenConnectionString = "<Your default listen connection string>";
       <span data-ttu-id="49baf-164">}</span><span class="sxs-lookup"><span data-stu-id="49baf-164">}</span></span>
2. <span data-ttu-id="49baf-165">Pomocí kroků hello výše, přidejte další novou třídu s názvem `MyInstanceIDService`.</span><span class="sxs-lookup"><span data-stu-id="49baf-165">Using hello steps above, add another new class named `MyInstanceIDService`.</span></span> <span data-ttu-id="49baf-166">Toto bude naše implementace služby procesu naslouchání Instance ID.</span><span class="sxs-lookup"><span data-stu-id="49baf-166">This will be our Instance ID listener service implementation.</span></span>
   
    <span data-ttu-id="49baf-167">Hello kód pro tuto třídu bude volat naše `IntentService` příliš[obnovení tokenu GCM hello](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) hello pozadí.</span><span class="sxs-lookup"><span data-stu-id="49baf-167">hello code for this class will call our `IntentService` too[refresh hello GCM token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) in hello background.</span></span>
   
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


1. <span data-ttu-id="49baf-168">Přidejte jiný nový projekt tooyour třída s názvem `RegistrationIntentService`.</span><span class="sxs-lookup"><span data-stu-id="49baf-168">Add another new class tooyour project named, `RegistrationIntentService`.</span></span> <span data-ttu-id="49baf-169">To bude hello implementace pro naše `IntentService` která zpracuje [aktualizace tokenu GCM hello](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) a [registrace centra oznámení hello](notification-hubs-push-notification-registration-management.md).</span><span class="sxs-lookup"><span data-stu-id="49baf-169">This will be hello implementation for our `IntentService` that will handle [refreshing hello GCM token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) and [registering with hello notification hub](notification-hubs-push-notification-registration-management.md).</span></span>
   
    <span data-ttu-id="49baf-170">Použijte následující kód pro tuto třídu hello.</span><span class="sxs-lookup"><span data-stu-id="49baf-170">Use hello following code for this class.</span></span>
   
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
   
                    // Storing hello registration id that indicates whether hello generated token has been
                    // sent tooyour server. If it is not stored, send hello token tooyour server,
                    // otherwise your server should have already received hello token.
                    if ((regID=sharedPreferences.getString("registrationID", null)) == null) {        
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.i(TAG, "Attempting tooregister with NH using token : " + token);
   
                        regID = hub.register(token).getRegistrationId();
   
                        // If you want toouse tags...
                        // Refer too: https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1", "tag2").getRegistrationId();
   
                        resultString = "Registered Successfully - RegId : " + regID;
                        Log.i(TAG, resultString);        
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                    } else {
                        resultString = "Previously Registered Successfully - RegId : " + regID;
                    }
                } catch (Exception e) {
                    Log.e(TAG, resultString="Failed toocomplete token refresh", e);
                    // If an exception happens while fetching hello new token or updating our registration data
                    // on a third-party server, this ensures that we'll attempt hello update at a later time.
                }
   
                // Notify UI that registration has completed.
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(resultString);
                }
            }
        }
2. <span data-ttu-id="49baf-171">Ve vaší `MainActivity` třídy, přidejte následující hello `import` příkazů výše hello třídy deklarace.</span><span class="sxs-lookup"><span data-stu-id="49baf-171">In your `MainActivity` class, add hello following `import` statements above hello class declaration.</span></span>
   
        import com.google.android.gms.common.ConnectionResult;
        import com.google.android.gms.common.GoogleApiAvailability;
        import com.google.android.gms.gcm.*;
        import com.microsoft.windowsazure.notifications.NotificationsManager;
        import android.util.Log;
        import android.widget.TextView;
        import android.widget.Toast;
3. <span data-ttu-id="49baf-172">Přidejte následující soukromé členy v horní části hello třídy hello hello.</span><span class="sxs-lookup"><span data-stu-id="49baf-172">Add hello following private members at hello top of hello class.</span></span> <span data-ttu-id="49baf-173">Použijeme tyto [zkontrolovat hello dostupnost služeb Google Play dle doporučení Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).</span><span class="sxs-lookup"><span data-stu-id="49baf-173">We will use these [check hello availability of Google Play Services as recommended by Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).</span></span>
   
        public static MainActivity mainActivity;
        public static Boolean isVisible = false;    
        private GoogleCloudMessaging gcm;
        private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;
4. <span data-ttu-id="49baf-174">Ve vašem `MainActivity` třídy, přidejte následující metodu toohello dostupnost služeb Google Play hello.</span><span class="sxs-lookup"><span data-stu-id="49baf-174">In your `MainActivity` class, add hello following method toohello availability of Google Play Services.</span></span> 
   
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
5. <span data-ttu-id="49baf-175">Ve vašem `MainActivity` třídy, přidejte následující kód, který zkontrolujte služby Google Play před voláním hello vaše `IntentService` tooget tokenu registrace GCM a registraci pomocí centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="49baf-175">In your `MainActivity` class, add hello following code that will check for Google Play Services before calling your `IntentService` tooget your GCM registration token and register with your notification hub.</span></span>
   
        public void registerWithNotificationHubs()
        {
            Log.i(TAG, " Registering with Notification Hubs");
   
            if (checkPlayServices()) {
                // Start IntentService tooregister this application with GCM.
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        }
6. <span data-ttu-id="49baf-176">V hello `OnCreate` metoda hello `MainActivity` třídy, přidejte následující kód toostart hello registraci při vytvoření aktivity hello.</span><span class="sxs-lookup"><span data-stu-id="49baf-176">In hello `OnCreate` method of hello `MainActivity` class, add hello following code toostart hello registration process when activity is created.</span></span>
   
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
   
            mainActivity = this;
            NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
            registerWithNotificationHubs();
        }
7. <span data-ttu-id="49baf-177">Přidejte tyto další metody toohello `MainActivity` tooverify aplikace stavu a stavu sestavy ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="49baf-177">Add these additional methods toohello `MainActivity` tooverify app state and report status in your app.</span></span>
   
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
8. <span data-ttu-id="49baf-178">Hello `ToastNotify` metoda používá hello *"Hello, World"* `TextView` řízení tooreport stavu a oznámení trvale v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="49baf-178">hello `ToastNotify` method uses hello *"Hello World"* `TextView` control tooreport status and notifications persistently in hello app.</span></span> <span data-ttu-id="49baf-179">Do rozložení activity_main.xml přidejte následující id pro ovládací prvek hello.</span><span class="sxs-lookup"><span data-stu-id="49baf-179">In your activity_main.xml layout, add hello following id for that control.</span></span>
   
       android:id="@+id/text_hello"
9. <span data-ttu-id="49baf-180">Další přidáme podtřídy pro naše příjemce, které jsme definovali v hello AndroidManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="49baf-180">Next we will add a subclass for our receiver we defined in hello AndroidManifest.xml.</span></span> <span data-ttu-id="49baf-181">Přidejte jiný nový projekt tooyour třída s názvem `MyHandler`.</span><span class="sxs-lookup"><span data-stu-id="49baf-181">Add another new class tooyour project named `MyHandler`.</span></span>
10. <span data-ttu-id="49baf-182">Přidejte následující příkazy pro import hello horní části hello `MyHandler.java`:</span><span class="sxs-lookup"><span data-stu-id="49baf-182">Add hello following import statements at hello top of `MyHandler.java`:</span></span>
    
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
        import com.microsoft.windowsazure.notifications.NotificationsHandler;
11. <span data-ttu-id="49baf-183">Přidejte následující kód pro hello hello `MyHandler` třídy, takže je podtřídou třídy `com.microsoft.windowsazure.notifications.NotificationsHandler`.</span><span class="sxs-lookup"><span data-stu-id="49baf-183">Add hello following code for hello `MyHandler` class making it a subclass of `com.microsoft.windowsazure.notifications.NotificationsHandler`.</span></span>
    
    <span data-ttu-id="49baf-184">Tento kód přepíše hello `OnReceive` proto hello obslužná rutina nahlásila oznámení, které jsou přijaty.</span><span class="sxs-lookup"><span data-stu-id="49baf-184">This code overrides hello `OnReceive` method, so hello handler will report notifications that are received.</span></span> <span data-ttu-id="49baf-185">Hello obslužná rutina také odesílá správci oznámení Android hello nabízená oznámení toohello pomocí hello `sendNotification()` metoda.</span><span class="sxs-lookup"><span data-stu-id="49baf-185">hello handler also sends hello push notification toohello Android notification manager by using hello `sendNotification()` method.</span></span> <span data-ttu-id="49baf-186">Hello `sendNotification()` metoda se má provést, když není hello aplikace spuštěna a není přijato oznámení.</span><span class="sxs-lookup"><span data-stu-id="49baf-186">hello `sendNotification()` method should be executed when hello app is not running and a notification is received.</span></span>
    
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
12. <span data-ttu-id="49baf-187">V Android Studio na řádku nabídek hello, klikněte na tlačítko **sestavení** > **znovu sestavit projekt** toomake se, že jsou ve vašem kódu nenachází žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="49baf-187">In Android Studio on hello menu bar, click **Build** > **Rebuild Project** toomake sure that no errors are present in your code.</span></span>

## <a name="sending-push-notifications"></a><span data-ttu-id="49baf-188">Odeslání nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="49baf-188">Sending push notifications</span></span>
<span data-ttu-id="49baf-189">Můžete otestovat přijímání nabízených oznámení ve vaší aplikaci jejich odesláním prostřednictvím hello [portálu Azure] -vyhledejte hello **Poradce při potížích s** kapitoly hello okno centra, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="49baf-189">You can test receiving push notifications in your app by sending them via hello [Azure Portal] - look for hello **Troubleshooting** Section in hello hub blade, as shown below.</span></span>

![Azure Notification Hubs – testovací odeslání](./media/notification-hubs-android-get-started/notification-hubs-test-send.png)

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-directly-from-hello-app"></a><span data-ttu-id="49baf-191">(Volitelné) Odesílání nabízených oznámení přímo z aplikace hello</span><span class="sxs-lookup"><span data-stu-id="49baf-191">(Optional) Send push notifications directly from hello app</span></span>
<span data-ttu-id="49baf-192">Za normálních okolností byste odesílali oznámení pomocí serveru backend.</span><span class="sxs-lookup"><span data-stu-id="49baf-192">Normally, you would send notifications using a backend server.</span></span> <span data-ttu-id="49baf-193">V některých případech můžete chtít toobe možné toosend nabízených oznámení přímo z klientské aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="49baf-193">For some cases, you might want toobe able toosend push notifications directly from hello client application.</span></span> <span data-ttu-id="49baf-194">Tato část vysvětluje, jak toosend oznámení z klienta hello pomocí hello [API služby REST centra oznámení Azure](https://msdn.microsoft.com/library/azure/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="49baf-194">This section explains how toosend notifications from hello client using hello [Azure Notification Hub REST API](https://msdn.microsoft.com/library/azure/dn223264.aspx).</span></span>

1. <span data-ttu-id="49baf-195">V zobrazení projektu Android Studio rozbalte možnost **App** > **src** > **main** > **res** > **layout**.</span><span class="sxs-lookup"><span data-stu-id="49baf-195">In Android Studio Project View, expand **App** > **src** > **main** > **res** > **layout**.</span></span> <span data-ttu-id="49baf-196">Otevřete hello `activity_main.xml` rozložení souboru a klikněte na tlačítko hello **Text** kartě obsah textu hello tooupdate hello souboru.</span><span class="sxs-lookup"><span data-stu-id="49baf-196">Open hello `activity_main.xml` layout file and click hello **Text** tab tooupdate hello text contents of hello file.</span></span> <span data-ttu-id="49baf-197">Aktualizujte jej s hello kódu níže, který přidává nové `Button` a `EditText` ovládací prvky pro zasílání oznámení centra oznámení toohello zprávy oznámení.</span><span class="sxs-lookup"><span data-stu-id="49baf-197">Update it with hello code below, which adds new `Button` and `EditText` controls for sending push notification messages toohello notification hub.</span></span> <span data-ttu-id="49baf-198">Přidejte tento kód v dolní části hello, těsně před `</RelativeLayout>`.</span><span class="sxs-lookup"><span data-stu-id="49baf-198">Add this code at hello bottom, just before `</RelativeLayout>`.</span></span>
   
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
2. <span data-ttu-id="49baf-199">V zobrazení projektu Android Studio rozbalte **App** > **src** > **main** > **res** > **values**.</span><span class="sxs-lookup"><span data-stu-id="49baf-199">In Android Studio Project View, expand **App** > **src** > **main** > **res** > **values**.</span></span> <span data-ttu-id="49baf-200">Otevřete hello `strings.xml` souboru a přidejte hello řetězcové hodnoty, které jsou odkazovány pomocí nového hello `Button` a `EditText` ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="49baf-200">Open hello `strings.xml` file and add hello string values that are referenced by hello new `Button` and `EditText` controls.</span></span> <span data-ttu-id="49baf-201">Přidejte tyto dolnímu hello hello souboru, těsně před `</resources>`.</span><span class="sxs-lookup"><span data-stu-id="49baf-201">Add these at hello bottom of hello file, just before `</resources>`.</span></span>
   
        <string name="send_button">Send Notification</string>
        <string name="notification_message_hint">Enter notification message text</string>
3. <span data-ttu-id="49baf-202">Ve vaší `NotificationSetting.java` soubor, přidejte následující nastavení toohello hello `NotificationSettings` třídy.</span><span class="sxs-lookup"><span data-stu-id="49baf-202">In your `NotificationSetting.java` file, add hello following setting toohello `NotificationSettings` class.</span></span>
   
    <span data-ttu-id="49baf-203">Aktualizace `HubFullAccess` s hello **DefaultFullSharedAccessSignature** připojovací řetězec pro vaše centrum.</span><span class="sxs-lookup"><span data-stu-id="49baf-203">Update `HubFullAccess` with hello **DefaultFullSharedAccessSignature** connection string for your hub.</span></span> <span data-ttu-id="49baf-204">Tento připojovací řetězec lze kopírovat z hello [portálu Azure] kliknutím **zásady přístupu** na hello **nastavení** okna pro vaše Centrum oznámení.</span><span class="sxs-lookup"><span data-stu-id="49baf-204">This connection string can be copied from hello [Azure Portal] by clicking **Access Policies** on hello **Settings** blade for your notification hub.</span></span>
   
        public static String HubFullAccess = "<Enter Your DefaultFullSharedAccess Connection string>";
4. <span data-ttu-id="49baf-205">Ve vaší `MainActivity.java` soubor, přidejte následující hello `import` příkazy výše hello `MainActivity` třídy.</span><span class="sxs-lookup"><span data-stu-id="49baf-205">In your `MainActivity.java` file, add hello following `import` statements above hello `MainActivity` class.</span></span>
   
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
5. <span data-ttu-id="49baf-206">Ve vaší `MainActivity.java` soubor, přidejte následující členy hello horní části hello hello `MainActivity` třídy.</span><span class="sxs-lookup"><span data-stu-id="49baf-206">In your `MainActivity.java` file, add hello following members at hello top of hello `MainActivity` class.</span></span>    
   
        private String HubEndpoint = null;
        private String HubSasKeyName = null;
        private String HubSasKeyValue = null;
6. <span data-ttu-id="49baf-207">Centrum POST požadavek toosend zprávy tooyour oznámení, musíte vytvořit tokenu tooauthenticate softwaru přístupový podpis (SaS).</span><span class="sxs-lookup"><span data-stu-id="49baf-207">You must create a Software Access Signature (SaS) token tooauthenticate a POST request toosend messages tooyour notification hub.</span></span> <span data-ttu-id="49baf-208">K tomu je potřeba analýza hello klíčová data z hello připojovací řetězec a pak vytvořit hello tokenu SaS, jak je uvedeno v hello [běžné koncepty](http://msdn.microsoft.com/library/azure/dn495627.aspx) odkazu k REST API.</span><span class="sxs-lookup"><span data-stu-id="49baf-208">This is done by parsing hello key data from hello connection string and then creating hello SaS token, as mentioned in hello [Common Concepts](http://msdn.microsoft.com/library/azure/dn495627.aspx) REST API reference.</span></span> <span data-ttu-id="49baf-209">Hello následující kód představuje příklad implementace.</span><span class="sxs-lookup"><span data-stu-id="49baf-209">hello following code is an example implementation.</span></span>
   
    <span data-ttu-id="49baf-210">V `MainActivity.java`, přidejte následující metodu toohello hello `MainActivity` třídy tooparse připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="49baf-210">In `MainActivity.java`, add hello following method toohello `MainActivity` class tooparse your connection string.</span></span>
   
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
7. <span data-ttu-id="49baf-211">V `MainActivity.java`, přidejte následující metodu toohello hello `MainActivity` třída toocreate ověřovacího tokenu SaS.</span><span class="sxs-lookup"><span data-stu-id="49baf-211">In `MainActivity.java`, add hello following method toohello `MainActivity` class toocreate a SaS authentication token.</span></span>
   
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
8. <span data-ttu-id="49baf-212">V `MainActivity.java`, přidejte následující metodu toohello hello `MainActivity` třída toohandle hello **odeslat oznámení** tlačítko a odesílání nabízených oznámení hello hello zpráva toohello centra pomocí předdefinovaného REST API.</span><span class="sxs-lookup"><span data-stu-id="49baf-212">In `MainActivity.java`, add hello following method toohello `MainActivity` class toohandle hello **Send Notification** button click and send hello push notification message toohello hub by using hello built-in REST API.</span></span>
   
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

## <a name="testing-your-app"></a><span data-ttu-id="49baf-213">Testování vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="49baf-213">Testing your app</span></span>
#### <a name="push-notifications-in-hello-emulator"></a><span data-ttu-id="49baf-214">Nabízená oznámení v emulátoru hello</span><span class="sxs-lookup"><span data-stu-id="49baf-214">Push notifications in hello emulator</span></span>
<span data-ttu-id="49baf-215">Pokud chcete, aby tootest nabízená oznámení uvnitř emulátoru, ujistěte se, že bitová kopie emulátoru podporuje úroveň rozhraní Google API hello, kterou jste zvolili pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="49baf-215">If you want tootest push notifications inside an emulator, make sure that your emulator image supports hello Google API level that you chose for your app.</span></span> <span data-ttu-id="49baf-216">Pokud bitová kopie nepodporuje nativní rozhraní Google API, budete mít s hello **služby\_není\_dostupné** výjimka.</span><span class="sxs-lookup"><span data-stu-id="49baf-216">If your image doesn't support native Google APIs, you will end up with hello **SERVICE\_NOT\_AVAILABLE** exception.</span></span>

<span data-ttu-id="49baf-217">Kromě toho toohello výše, zkontrolujte, že jste přidali vaší tooyour účet Google spuštěný emulátoru pod **nastavení** > **účty**.</span><span class="sxs-lookup"><span data-stu-id="49baf-217">In addition toohello above, ensure that you have added your Google account tooyour running emulator under **Settings** > **Accounts**.</span></span> <span data-ttu-id="49baf-218">Jinak, může vaše pokusy o tooregister s GCM výsledkem hello **ověřování\_se nezdařilo** výjimka.</span><span class="sxs-lookup"><span data-stu-id="49baf-218">Otherwise, your attempts tooregister with GCM may result in hello **AUTHENTICATION\_FAILED** exception.</span></span>

#### <a name="running-hello-application"></a><span data-ttu-id="49baf-219">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="49baf-219">Running hello application</span></span>
1. <span data-ttu-id="49baf-220">Spuštění aplikace hello a Všimněte si, že hello ID registrace hlášené pro úspěšnou registraci.</span><span class="sxs-lookup"><span data-stu-id="49baf-220">Run hello app and notice that hello registration ID is reported for a successful registration.</span></span>
   
      ![Testování v systému Android – registrace kanálu][18]
2. <span data-ttu-id="49baf-222">Zadejte toobe zpráv oznámení odeslaných tooall zařízení Android, která byla zaregistrovaná hello rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="49baf-222">Enter a notification message toobe sent tooall Android devices that have registered with hello hub.</span></span>
   
      ![Testování v systému Android – odesílání zprávy][19]

3. <span data-ttu-id="49baf-224">Stiskněte tlačítko **Odeslat oznámení**.</span><span class="sxs-lookup"><span data-stu-id="49baf-224">Press **Send Notification**.</span></span> <span data-ttu-id="49baf-225">Zobrazí všechna zařízení, které aplikace běžet hello `AlertDialog` instance s hello nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="49baf-225">Any devices that have hello app running will show an `AlertDialog` instance with hello push notification message.</span></span> <span data-ttu-id="49baf-226">Zařízení, které nemají hello aplikace spuštěná, ale byla dříve registrována pro nabízená oznámení se zobrazí oznámení v hello správci oznámení Android.</span><span class="sxs-lookup"><span data-stu-id="49baf-226">Devices that don't have hello app running but were previously registered for push notifications will receive a notification in hello Android Notification Manager.</span></span> <span data-ttu-id="49baf-227">Ta lze zobrazit potažením dolů z levého horního rohu hello.</span><span class="sxs-lookup"><span data-stu-id="49baf-227">Those can be viewed by swiping down from hello upper-left corner.</span></span>
   
      ![Testování v systému Android – oznámení][21]

## <a name="next-steps"></a><span data-ttu-id="49baf-229">Další kroky</span><span class="sxs-lookup"><span data-stu-id="49baf-229">Next steps</span></span>
<span data-ttu-id="49baf-230">Doporučujeme, abyste hello [použití centra oznámení toopush oznámení toousers] kurz jako další krok hello.</span><span class="sxs-lookup"><span data-stu-id="49baf-230">We recommend hello [Use Notification Hubs toopush notifications toousers] tutorial as hello next step.</span></span> <span data-ttu-id="49baf-231">Zobrazí se jak toosend oznámení z back-end ASP.NET pomocí značky tootarget konkrétním uživatelům.</span><span class="sxs-lookup"><span data-stu-id="49baf-231">It will show you how toosend notifications from an ASP.NET backend using tags tootarget specific users.</span></span>

<span data-ttu-id="49baf-232">Pokud chcete toosegment uživatele podle zájmových skupin, podívejte se na hello [toosend použití centra oznámení nejnovější zprávy přes] kurzu.</span><span class="sxs-lookup"><span data-stu-id="49baf-232">If you want toosegment your users by interest groups, check out hello [Use Notification Hubs toosend breaking news] tutorial.</span></span>

<span data-ttu-id="49baf-233">toolearn další obecné informace o centrech oznámení naleznete v našem [pokyny centra oznámení].</span><span class="sxs-lookup"><span data-stu-id="49baf-233">toolearn more general information about Notification Hubs, see our [Notification Hubs Guidance].</span></span>

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
[pokyny centra oznámení]: http://msdn.microsoft.com/library/jj927170.aspx
[použití centra oznámení toopush oznámení toousers]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[toosend použití centra oznámení nejnovější zprávy přes]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[portálu Azure]: https://portal.azure.com
