---
title: "aaaGet začít s Azure Notification Hubs pro aplikace Kindle | Microsoft Docs"
description: "V tomto kurzu zjistíte, jak Azure Notification Hubs toosend toouse nabízená oznámení aplikace Kindle tooa."
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 346fc8e5-294b-4e4f-9f27-7a82d9626e93
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-kindle
ms.devlang: Java
ms.topic: hero-article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 7c28d64372cd2d90bab9cd9bf818d333f3478f7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-notification-hubs-for-kindle-apps"></a><span data-ttu-id="11407-103">Začínáme s použitím Notification Hubs pro aplikace Kindle</span><span class="sxs-lookup"><span data-stu-id="11407-103">Get started with Notification Hubs for Kindle apps</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="11407-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="11407-104">Overview</span></span>
<span data-ttu-id="11407-105">Tento kurz ukazuje, jak Azure Notification Hubs toosend toouse nabízená oznámení aplikace Kindle tooa.</span><span class="sxs-lookup"><span data-stu-id="11407-105">This tutorial shows you how toouse Azure Notification Hubs toosend push notifications tooa Kindle application.</span></span>
<span data-ttu-id="11407-106">Vytvoříte prázdnou aplikaci systému Kindle, která bude přijímat nabízená oznámení pomocí služby ADM (Amazon Device Messaging).</span><span class="sxs-lookup"><span data-stu-id="11407-106">You'll create a blank Kindle app that receives push notifications by using Amazon Device Messaging (ADM).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="11407-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="11407-107">Prerequisites</span></span>
<span data-ttu-id="11407-108">Tento kurz vyžaduje hello následující:</span><span class="sxs-lookup"><span data-stu-id="11407-108">This tutorial requires hello following:</span></span>

* <span data-ttu-id="11407-109">Hello Android SDK (předpokládáme, že použijete Eclipse) získejte z hello <a href="http://go.microsoft.com/fwlink/?LinkId=389797">lokality Android</a>.</span><span class="sxs-lookup"><span data-stu-id="11407-109">Get hello Android SDK (we assume that you will use Eclipse) from hello <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android site</a>.</span></span>
* <span data-ttu-id="11407-110">Postupujte podle kroků hello v <a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">nastavení vašeho vývojového prostředí</a> tooset vývojového prostředí pro Kindle.</span><span class="sxs-lookup"><span data-stu-id="11407-110">Follow hello steps in <a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">Setting Up Your Development Environment</a> tooset up your development environment for Kindle.</span></span>

## <a name="add-a-new-app-toohello-developer-portal"></a><span data-ttu-id="11407-111">Přidat nový portál pro vývojáře aplikací toohello</span><span class="sxs-lookup"><span data-stu-id="11407-111">Add a new app toohello developer portal</span></span>
1. <span data-ttu-id="11407-112">Nejprve vytvořte aplikaci v hello [portálu pro vývojáře Amazon].</span><span class="sxs-lookup"><span data-stu-id="11407-112">First, create an app in hello [Amazon developer portal].</span></span>
   
    ![][0]
2. <span data-ttu-id="11407-113">Kopírování hello **klíč aplikace**.</span><span class="sxs-lookup"><span data-stu-id="11407-113">Copy hello **Application Key**.</span></span>
   
    ![][1]
3. <span data-ttu-id="11407-114">Hello portálu klikněte hello název vaší aplikace a pak klikněte na hello **zasílání zpráv zařízení** kartě.</span><span class="sxs-lookup"><span data-stu-id="11407-114">In hello portal, click hello name of your app, and then click hello **Device Messaging** tab.</span></span>
   
    ![][2]
4. <span data-ttu-id="11407-115">Klikněte na tlačítko **Vytvoření nového profilu zabezpečení** a pak vytvořte nový profil zabezpečení (například **profil zabezpečení TestAdm**).</span><span class="sxs-lookup"><span data-stu-id="11407-115">Click **Create a New Security Profile**, and then create a new security profile (for example, **TestAdm security profile**).</span></span> <span data-ttu-id="11407-116">Potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="11407-116">Then click **Save**.</span></span>
   
    ![][3]
5. <span data-ttu-id="11407-117">Klikněte na tlačítko **profily zabezpečení** profil tooview hello zabezpečení, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="11407-117">Click **Security Profiles** tooview hello security profile that you just created.</span></span> <span data-ttu-id="11407-118">Kopírování hello **ID klienta** a **tajný klíč klienta** hodnoty pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="11407-118">Copy hello **Client ID** and **Client Secret** values for later use.</span></span>
   
    ![][4]

## <a name="create-an-api-key"></a><span data-ttu-id="11407-119">Vytvořte klíč rozhraní API</span><span class="sxs-lookup"><span data-stu-id="11407-119">Create an API key</span></span>
1. <span data-ttu-id="11407-120">Otevřete příkazový řádek s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="11407-120">Open a command prompt with administrator privileges.</span></span>
2. <span data-ttu-id="11407-121">Přejděte toohello složky sady SDK pro Android.</span><span class="sxs-lookup"><span data-stu-id="11407-121">Navigate toohello Android SDK folder.</span></span>
3. <span data-ttu-id="11407-122">Zadejte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="11407-122">Enter hello following command:</span></span>
   
        keytool -list -v -alias androiddebugkey -keystore ./debug.keystore
   
    ![][5]
4. <span data-ttu-id="11407-123">Pro hello **úložiště klíčů** heslo, typ **android**.</span><span class="sxs-lookup"><span data-stu-id="11407-123">For hello **keystore** password, type **android**.</span></span>
5. <span data-ttu-id="11407-124">Kopírování hello **MD5** otisků prstů.</span><span class="sxs-lookup"><span data-stu-id="11407-124">Copy hello **MD5** fingerprint.</span></span>
6. <span data-ttu-id="11407-125">Zpět v portálu pro vývojáře hello na hello **zasílání zpráv** , klikněte na **Android/Kindle** a zadejte název hello hello balíčku pro vaši aplikaci (například **com.sample.notificationhubtest**) a hello **MD5** hodnotu a pak klikněte na **vygenerovat klíč rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="11407-125">Back in hello developer portal, on hello **Messaging** tab, click **Android/Kindle** and enter hello name of hello package for your app (for example, **com.sample.notificationhubtest**) and hello **MD5** value, and then click **Generate API Key**.</span></span>

## <a name="add-credentials-toohello-hub"></a><span data-ttu-id="11407-126">Přidat přihlašovací údaje toohello rozbočovače</span><span class="sxs-lookup"><span data-stu-id="11407-126">Add credentials toohello hub</span></span>
<span data-ttu-id="11407-127">Hello portálu přidejte hello klienta tajný klíč a klient ID toohello **konfigurace** centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="11407-127">In hello portal, add hello client secret and client ID toohello **Configure** tab of your notification hub.</span></span>

## <a name="set-up-your-application"></a><span data-ttu-id="11407-128">Nastavte aplikaci</span><span class="sxs-lookup"><span data-stu-id="11407-128">Set up your application</span></span>
> [!NOTE]
> <span data-ttu-id="11407-129">Při vytváření aplikace použijte alespoň 17 úrovní rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="11407-129">When you're creating an application, use at least API Level 17.</span></span>
> 
> 

<span data-ttu-id="11407-130">Přidání projektu Eclipse tooyour knihovny ADM hello:</span><span class="sxs-lookup"><span data-stu-id="11407-130">Add hello ADM libraries tooyour Eclipse project:</span></span>

1. <span data-ttu-id="11407-131">knihovny ADM hello tooobtain [stáhnout hello SDK].</span><span class="sxs-lookup"><span data-stu-id="11407-131">tooobtain hello ADM library, [download hello SDK].</span></span> <span data-ttu-id="11407-132">Extrahujte soubor zip sady SDK hello.</span><span class="sxs-lookup"><span data-stu-id="11407-132">Extract hello SDK zip file.</span></span>
2. <span data-ttu-id="11407-133">V Eclipse klikněte pravým tlačítkem na projekt a pak klikněte na tlačítko **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="11407-133">In Eclipse, right-click your project, and then click **Properties**.</span></span> <span data-ttu-id="11407-134">Vyberte **cesta sestavení Java** na hello vlevo a pak vyberte hello ** knihovny ** karty v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="11407-134">Select **Java Build Path** on hello left, and then select hello **Libraries **tab at hello top.</span></span> <span data-ttu-id="11407-135">Klikněte na tlačítko **přidat externí Jar**a vyberte hello soubor `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` z hello adresáře, do kterého jste extrahovali Amazon SDK hello.</span><span class="sxs-lookup"><span data-stu-id="11407-135">Click **Add External Jar**, and select hello file `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` from hello directory in which you extracted hello Amazon SDK.</span></span>
3. <span data-ttu-id="11407-136">Stáhněte si hello SDK NotificationHubs pro Android (odkaz).</span><span class="sxs-lookup"><span data-stu-id="11407-136">Download hello NotificationHubs Android SDK (link).</span></span>
4. <span data-ttu-id="11407-137">Rozbalte balíček hello a pak přetáhněte soubor hello `notification-hubs-sdk.jar` do hello `libs` složky v prostředí Eclipse.</span><span class="sxs-lookup"><span data-stu-id="11407-137">Unzip hello package, and then drag hello file `notification-hubs-sdk.jar` into hello `libs` folder in Eclipse.</span></span>

<span data-ttu-id="11407-138">Upravte manifest toosupport aplikace ADM:</span><span class="sxs-lookup"><span data-stu-id="11407-138">Edit your app manifest toosupport ADM:</span></span>

1. <span data-ttu-id="11407-139">Přidáte obor názvů Amazon hello v hello kořenového prvku manifestu:</span><span class="sxs-lookup"><span data-stu-id="11407-139">Add hello Amazon namespace in hello root manifest element:</span></span>

        xmlns:amazon="http://schemas.amazon.com/apk/res/android"

1. <span data-ttu-id="11407-140">Přidejte oprávnění jako první prvek hello v rámci prvku manifestu hello.</span><span class="sxs-lookup"><span data-stu-id="11407-140">Add permissions as hello first element under hello manifest element.</span></span> <span data-ttu-id="11407-141">SUBSTITUTE **[název vašeho balíčku]** s balíčkem hello používá toocreate vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="11407-141">Substitute **[YOUR PACKAGE NAME]** with hello package that you used toocreate your app.</span></span>
   
        <permission
         android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE"
         android:protectionLevel="signature" />
   
        <uses-permission android:name="android.permission.INTERNET"/>
   
        <uses-permission android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE" />
   
        <!-- This permission allows your app access tooreceive push notifications
        from ADM. -->
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE" />
   
        <!-- ADM uses WAKE_LOCK tookeep hello processor from sleeping when a message is received. -->
        <uses-permission android:name="android.permission.WAKE_LOCK" />
2. <span data-ttu-id="11407-142">Vložte následující prvek jako první podřízený prvek aplikace hello hello hello.</span><span class="sxs-lookup"><span data-stu-id="11407-142">Insert hello following element as hello first child of hello application element.</span></span> <span data-ttu-id="11407-143">Mějte na paměti, toosubstitute **[svůj název služby]** s hello názvem vaší obslužné rutiny zpráv ADM, které vytvoříte v další části hello (včetně hello balíček) a nahraďte **[název vašeho balíčku]** s hello Název balíčku, pomocí kterého jste vytvořili vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="11407-143">Remember toosubstitute **[YOUR SERVICE NAME]** with hello name of your ADM message handler that you create in hello next section (including hello package), and replace **[YOUR PACKAGE NAME]** with hello package name with which you created your app.</span></span>
   
        <amazon:enable-feature
              android:name="com.amazon.device.messaging"
                     android:required="true"/>
        <service
            android:name="[YOUR SERVICE NAME]"
            android:exported="false" />
   
        <receiver
            android:name="[YOUR SERVICE NAME]$Receiver" />
   
            <!-- This permission ensures that only ADM can send your app registration broadcasts. -->
            android:permission="com.amazon.device.messaging.permission.SEND" >
   
            <!-- toointeract with ADM, your app must listen for hello following intents. -->
            <intent-filter>
          <action android:name="com.amazon.device.messaging.intent.REGISTRATION" />
          <action android:name="com.amazon.device.messaging.intent.RECEIVE" />
   
          <!-- Replace hello name in hello category tag with your app's package name. -->
          <category android:name="[YOUR PACKAGE NAME]" />
            </intent-filter>
        </receiver>

## <a name="create-your-adm-message-handler"></a><span data-ttu-id="11407-144">Vytvořte obslužnou rutinu zpráv ADM</span><span class="sxs-lookup"><span data-stu-id="11407-144">Create your ADM message handler</span></span>
1. <span data-ttu-id="11407-145">Vytvořte novou třídu, která dědí z `com.amazon.device.messaging.ADMMessageHandlerBase` a pojmenujte ji `MyADMMessageHandler`, jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="11407-145">Create a new class that inherits from `com.amazon.device.messaging.ADMMessageHandlerBase` and name it `MyADMMessageHandler`, as shown in hello following figure:</span></span>
   
    ![][6]
2. <span data-ttu-id="11407-146">Přidejte následující hello `import` příkazy:</span><span class="sxs-lookup"><span data-stu-id="11407-146">Add hello following `import` statements:</span></span>
   
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.support.v4.app.NotificationCompat;
        import com.amazon.device.messaging.ADMMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub
3. <span data-ttu-id="11407-147">Přidejte následující kód ve třídě hello, kterou jste vytvořili hello.</span><span class="sxs-lookup"><span data-stu-id="11407-147">Add hello following code in hello class that you created.</span></span> <span data-ttu-id="11407-148">Mějte na paměti, toosubstitute hello, rozbočovače název a připojovací řetězec (naslouchání):</span><span class="sxs-lookup"><span data-stu-id="11407-148">Remember toosubstitute hello hub name and connection string (listen):</span></span>
   
        public static final int NOTIFICATION_ID = 1;
        private NotificationManager mNotificationManager;
        NotificationCompat.Builder builder;
          private static NotificationHub hub;
        public static NotificationHub getNotificationHub(Context context) {
            Log.v("com.wa.hellokindlefire", "getNotificationHub");
            if (hub == null) {
                hub = new NotificationHub("[hub name]", "[listen connection string]", context);
            }
            return hub;
        }
   
        public MyADMMessageHandler() {
                super("MyADMMessageHandler");
            }
   
            public static class Receiver extends ADMMessageReceiver
            {
                public Receiver()
                {
                    super(MyADMMessageHandler.class);
                }
            }
   
            private void sendNotification(String msg) {
                Context ctx = getApplicationContext();
   
                mNotificationManager = (NotificationManager)
                    ctx.getSystemService(Context.NOTIFICATION_SERVICE);
   
            PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                  new Intent(ctx, MainActivity.class), 0);
   
            NotificationCompat.Builder mBuilder =
                  new NotificationCompat.Builder(ctx)
                  .setSmallIcon(R.mipmap.ic_launcher)
                  .setContentTitle("Notification Hub Demo")
                  .setStyle(new NotificationCompat.BigTextStyle()
                         .bigText(msg))
                  .setContentText(msg);
   
             mBuilder.setContentIntent(contentIntent);
             mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
        }
4. <span data-ttu-id="11407-149">Přidejte následující kód toohello hello `OnMessage()` metoda:</span><span class="sxs-lookup"><span data-stu-id="11407-149">Add hello following code toohello `OnMessage()` method:</span></span>
   
        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);
5. <span data-ttu-id="11407-150">Přidejte následující kód toohello hello `OnRegistered` metoda:</span><span class="sxs-lookup"><span data-stu-id="11407-150">Add hello following code toohello `OnRegistered` method:</span></span>
   
            try {
        getNotificationHub(getApplicationContext()).register(registrationId);
            } catch (Exception e) {
        Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
            }
6. <span data-ttu-id="11407-151">Přidejte následující kód toohello hello `OnUnregistered` metoda:</span><span class="sxs-lookup"><span data-stu-id="11407-151">Add hello following code toohello `OnUnregistered` method:</span></span>
   
         try {
             getNotificationHub(getApplicationContext()).unregister();
         } catch (Exception e) {
             Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
         }
7. <span data-ttu-id="11407-152">V hello `MainActivity` metody přidat hello následující příkaz importu:</span><span class="sxs-lookup"><span data-stu-id="11407-152">In hello `MainActivity` method, add hello following import statement:</span></span>
   
        import com.amazon.device.messaging.ADM;
8. <span data-ttu-id="11407-153">Přidejte následující kód na konci hello hello hello `OnCreate` metoda:</span><span class="sxs-lookup"><span data-stu-id="11407-153">Add hello following code at hello end of hello `OnCreate` method:</span></span>
   
        final ADM adm = new ADM(this);
        if (adm.getRegistrationId() == null)
        {
           adm.startRegister();
        } else {
            new AsyncTask() {
                  @Override
                  protected Object doInBackground(Object... params) {
                     try {                         MyADMMessageHandler.getNotificationHub(getApplicationContext()).register(adm.getRegistrationId());
                     } catch (Exception e) {
                         Log.e("com.wa.hellokindlefire", "Failed registration with hub", e);
                         return e;
                     }
                     return null;
                 }
               }.execute(null, null, null);
        }

## <a name="add-your-api-key-tooyour-app"></a><span data-ttu-id="11407-154">Přidání klíče tooyour aplikace API</span><span class="sxs-lookup"><span data-stu-id="11407-154">Add your API key tooyour app</span></span>
1. <span data-ttu-id="11407-155">V nástroji Eclipse vytvořte nový soubor s názvem **api_key.txt** v hello majetku adresáře projektu.</span><span class="sxs-lookup"><span data-stu-id="11407-155">In Eclipse, create a new file named **api_key.txt** in hello directory assets of your project.</span></span>
2. <span data-ttu-id="11407-156">Otevřete hello soubor a zkopírujte hello klíč rozhraní API generovaný na portálu pro vývojáře Amazon hello.</span><span class="sxs-lookup"><span data-stu-id="11407-156">Open hello file and copy hello API key that you generated in hello Amazon developer portal.</span></span>

## <a name="run-hello-app"></a><span data-ttu-id="11407-157">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="11407-157">Run hello app</span></span>
1. <span data-ttu-id="11407-158">Spusťte emulátor hello.</span><span class="sxs-lookup"><span data-stu-id="11407-158">Start hello emulator.</span></span>
2. <span data-ttu-id="11407-159">V emulátoru hello prstem z horní hello a klikněte na tlačítko **nastavení**a potom klikněte na **Můj účet** a zaregistrujte se pomocí platného účtu Amazon.</span><span class="sxs-lookup"><span data-stu-id="11407-159">In hello emulator, swipe from hello top and click **Settings**, and then click **My account** and register with a valid Amazon account.</span></span>
3. <span data-ttu-id="11407-160">V nástroji Eclipse spusťte aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="11407-160">In Eclipse, run hello app.</span></span>

> [!NOTE]
> <span data-ttu-id="11407-161">Pokud dojde k potížím, zkontrolujte hello čas emulátoru hello (nebo zařízení).</span><span class="sxs-lookup"><span data-stu-id="11407-161">If a problem occurs, check hello time of hello emulator (or device).</span></span> <span data-ttu-id="11407-162">Hello časová hodnota musí být přesná.</span><span class="sxs-lookup"><span data-stu-id="11407-162">hello time value must be accurate.</span></span> <span data-ttu-id="11407-163">toochange hello čas emulátoru Kindle hello, můžete spustit následující příkaz z adresáře nástrojů platformy Android SDK hello:</span><span class="sxs-lookup"><span data-stu-id="11407-163">toochange hello time of hello Kindle emulator, you can run hello following command from your Android SDK platform-tools directory:</span></span>
> 
> 

        adb shell  date -s "yyyymmdd.hhmmss"

## <a name="send-a-message"></a><span data-ttu-id="11407-164">Odeslat zprávu</span><span class="sxs-lookup"><span data-stu-id="11407-164">Send a message</span></span>
<span data-ttu-id="11407-165">toosend zprávy pomocí .NET:</span><span class="sxs-lookup"><span data-stu-id="11407-165">toosend a message by using .NET:</span></span>

        static void Main(string[] args)
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("[conn string]", "[hub name]");

            hub.SendAdmNativeNotificationAsync("{\"data\":{\"msg\" : \"Hello from .NET!\"}}").Wait();
        }

![][7]

<!-- URLs. -->
[portálu pro vývojáře Amazon]: https://developer.amazon.com/home.html
[Stáhnout hello SDK]: https://developer.amazon.com/public/resources/development-tools/sdk

[0]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal1.png
[1]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal2.png
[2]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal3.png
[3]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal4.png
[4]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal5.png
[5]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-cmd-window.png
[6]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-new-java-class.png
[7]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-notification.png
