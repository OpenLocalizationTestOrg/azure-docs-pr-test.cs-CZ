---
title: "Přidání nabízených oznámení do aplikace Apache Cordova s Azure Mobile Apps | Microsoft Docs"
description: "Naučte se používat Azure Mobile Apps k odesílání nabízených oznámení do vaší aplikace Apache Cordova."
services: app-service\mobile
documentationcenter: javascript
manager: syntaxc4
editor: 
author: ggailey777
ms.assetid: 92c596a9-875c-4840-b0e1-69198817576f
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: dc3cab0a6a8b4a56ab0fba1a02e5bba9d0ed1b1f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="add-push-notifications-to-your-apache-cordova-app"></a><span data-ttu-id="809bb-103">Přidání nabízených oznámení do vaší aplikace Apache Cordova</span><span class="sxs-lookup"><span data-stu-id="809bb-103">Add push notifications to your Apache Cordova app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="809bb-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="809bb-104">Overview</span></span>
<span data-ttu-id="809bb-105">V tomto kurzu přidání nabízených oznámení do projektu [Apache Cordova úvodní] tak, aby nabízených oznámení se odešle do zařízení pokaždé, když vložení záznamu.</span><span class="sxs-lookup"><span data-stu-id="809bb-105">In this tutorial, you add push notifications to the [Apache Cordova quick start] project so that a push notification is sent to the device every time a record is inserted.</span></span>

<span data-ttu-id="809bb-106">Pokud použijete serverový projekt stažené rychlý start, je třeba balíček rozšíření nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="809bb-106">If you do not use the downloaded quick start server project, you need the push notification extension package.</span></span> <span data-ttu-id="809bb-107">Další informace najdete v tématu [pracovat s .NET back-end serveru SDK pro Azure Mobile Apps][1].</span><span class="sxs-lookup"><span data-stu-id="809bb-107">For more information, see [Work with the .NET backend server SDK for Azure Mobile Apps][1].</span></span>

## <span data-ttu-id="809bb-108"><a name="prerequisites"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="809bb-108"><a name="prerequisites"></a>Prerequisites</span></span>
<span data-ttu-id="809bb-109">Tento kurz se zaměřuje Apache Cordova aplikace vytvořené s Visual Studiem 2015, která běží na emulátor Google Android, zařízení se systémem Android, zařízení se systémem Windows a zařízení s iOS.</span><span class="sxs-lookup"><span data-stu-id="809bb-109">This tutorial covers an Apache Cordova application developed with Visual Studio 2015 that runs on the Google Android Emulator, an Android device, a Windows device, and an iOS device.</span></span>

<span data-ttu-id="809bb-110">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="809bb-110">To complete this tutorial, you need:</span></span>

* <span data-ttu-id="809bb-111">Počítač s nástrojem [Visual Studio Community 2015] [ 2] nebo novější verze.</span><span class="sxs-lookup"><span data-stu-id="809bb-111">A PC with [Visual Studio Community 2015][2] or later versions.</span></span>
* <span data-ttu-id="809bb-112">[Nástroje sady Visual Studio pro Apache Cordova][4].</span><span class="sxs-lookup"><span data-stu-id="809bb-112">[Visual Studio Tools for Apache Cordova][4].</span></span>
* <span data-ttu-id="809bb-113">[Aktivní účet Azure][3].</span><span class="sxs-lookup"><span data-stu-id="809bb-113">An [active Azure account][3].</span></span>
* <span data-ttu-id="809bb-114">Dokončené [úvodní Apache Cordova] [ 5] projektu.</span><span class="sxs-lookup"><span data-stu-id="809bb-114">A completed [Apache Cordova quick start][5] project.</span></span>
* <span data-ttu-id="809bb-115">(Android) A [účet Google] [ 6] s ověřenou e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="809bb-115">(Android) A [Google account][6] with a verified email address.</span></span>
* <span data-ttu-id="809bb-116">(iOS) [Programu pro vývojáře Apple členství] [ 7] a zařízení se systémem iOS (simulátoru iOS push nepodporuje).</span><span class="sxs-lookup"><span data-stu-id="809bb-116">(iOS) An [Apple Developer Program membership][7] and an iOS device (iOS Simulator does not support push).</span></span>
* <span data-ttu-id="809bb-117">(Windows) A [Windows Store vývojářský účet] [ 8] a zařízením s Windows 10.</span><span class="sxs-lookup"><span data-stu-id="809bb-117">(Windows) A [Windows Store Developer Account][8] and a Windows 10 device.</span></span>

## <span data-ttu-id="809bb-118"><a name="configure-hub"></a>Konfigurace centra oznámení</span><span class="sxs-lookup"><span data-stu-id="809bb-118"><a name="configure-hub"></a>Configure a notification hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

<span data-ttu-id="809bb-119">[Přehrát video, zobrazující kroky v této části][9]</span><span class="sxs-lookup"><span data-stu-id="809bb-119">[Watch a video showing steps in this section][9]</span></span>

## <a name="update-the-server-project"></a><span data-ttu-id="809bb-120">Aktualizace server project</span><span class="sxs-lookup"><span data-stu-id="809bb-120">Update the server project</span></span>
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <span data-ttu-id="809bb-121"><a name="add-push-to-app"></a>Upravit aplikaci Cordova</span><span class="sxs-lookup"><span data-stu-id="809bb-121"><a name="add-push-to-app"></a>Modify your Cordova app</span></span>
<span data-ttu-id="809bb-122">Zkontrolujte, zda že je připraven ke zpracování nabízených oznámení nainstalováním nabízené cordovu plus žádné specifické pro platformu nabízené služby projektu aplikace Apache Cordova.</span><span class="sxs-lookup"><span data-stu-id="809bb-122">Ensure your Apache Cordova app project is ready to handle push notifications by installing the Cordova push plugin plus any platform-specific push services.</span></span>

#### <a name="update-the-cordova-version-in-your-project"></a><span data-ttu-id="809bb-123">Aktualizujte verzi Cordova ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="809bb-123">Update the Cordova version in your project.</span></span>
<span data-ttu-id="809bb-124">Pokud váš projekt používá starší než v6.1.1 verzi aplikace Apache Cordova, aktualizujte projektu klienta.</span><span class="sxs-lookup"><span data-stu-id="809bb-124">If your project uses a version of Apache Cordova earlier than v6.1.1, update the client project.</span></span> <span data-ttu-id="809bb-125">Aktualizace projektu:</span><span class="sxs-lookup"><span data-stu-id="809bb-125">To update the project:</span></span>

* <span data-ttu-id="809bb-126">Klikněte pravým tlačítkem na `config.xml` otevřete návrháře konfigurace.</span><span class="sxs-lookup"><span data-stu-id="809bb-126">Right-click `config.xml` to open the configuration designer.</span></span>
* <span data-ttu-id="809bb-127">Vyberte kartu platformy.</span><span class="sxs-lookup"><span data-stu-id="809bb-127">Select the Platforms tab.</span></span>
* <span data-ttu-id="809bb-128">Zvolte 6.1.1 v **Cordova CLI** textové pole.</span><span class="sxs-lookup"><span data-stu-id="809bb-128">Choose 6.1.1 in the **Cordova CLI** text box.</span></span>
* <span data-ttu-id="809bb-129">Zvolte **sestavení**, pak **sestavit řešení** aktualizujte projekt.</span><span class="sxs-lookup"><span data-stu-id="809bb-129">Choose **Build**, then **Build Solution** to update the project.</span></span>

#### <a name="install-the-push-plugin"></a><span data-ttu-id="809bb-130">Instalace modulu plug-in push</span><span class="sxs-lookup"><span data-stu-id="809bb-130">Install the push plugin</span></span>
<span data-ttu-id="809bb-131">Aplikace Apache Cordova nezpracuje nativně možnosti sítě nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="809bb-131">Apache Cordova applications do not natively handle device or network capabilities.</span></span>  <span data-ttu-id="809bb-132">Tyto možnosti jsou poskytovány buď modulů plug-in, které jsou publikovány na [npm] [ 10] nebo na Githubu.</span><span class="sxs-lookup"><span data-stu-id="809bb-132">These capabilities are provided by plugins that are published either on [npm][10] or on GitHub.</span></span>  <span data-ttu-id="809bb-133">`phonegap-plugin-push` Modulu plug-in se používá ke zpracování nabízených oznámení sítě.</span><span class="sxs-lookup"><span data-stu-id="809bb-133">The `phonegap-plugin-push` plugin is used to handle network push notifications.</span></span>

<span data-ttu-id="809bb-134">Modul plug-in nabízené můžete nainstalovat jedním z těchto způsobů:</span><span class="sxs-lookup"><span data-stu-id="809bb-134">You can install the push plugin in one of these ways:</span></span>

<span data-ttu-id="809bb-135">**Z příkazového řádku:**</span><span class="sxs-lookup"><span data-stu-id="809bb-135">**From the command-prompt:**</span></span>

<span data-ttu-id="809bb-136">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="809bb-136">Execute the following command:</span></span>

    cordova plugin add phonegap-plugin-push

<span data-ttu-id="809bb-137">**Z v sadě Visual Studio:**</span><span class="sxs-lookup"><span data-stu-id="809bb-137">**From within Visual Studio:**</span></span>

1. <span data-ttu-id="809bb-138">V Průzkumníku řešení otevřete `config.xml` klikněte na soubor **modulů plug-in** > **vlastní**, vyberte **Git** jako zdroj instalace pak zadejte `https://github.com/phonegap/phonegap-plugin-push`jako zdroj.</span><span class="sxs-lookup"><span data-stu-id="809bb-138">In Solution Explorer, open the `config.xml` file click **Plugins** > **Custom**, select **Git** as the  installation source, then enter `https://github.com/phonegap/phonegap-plugin-push` as the source.</span></span>

   ![][img1]

2. <span data-ttu-id="809bb-139">Klikněte na šipku vedle zdroje instalace.</span><span class="sxs-lookup"><span data-stu-id="809bb-139">Click the arrow next to the installation source.</span></span>
3. <span data-ttu-id="809bb-140">V **SENDER_ID**, pokud již máte ID číselné projektu pro projekt vývojářské konzole Google, můžete přidat sem.</span><span class="sxs-lookup"><span data-stu-id="809bb-140">In **SENDER_ID**, if you already have a numeric project ID for the Google Developer Console project, you can add it here.</span></span> <span data-ttu-id="809bb-141">Jinak zadejte hodnotu zástupného symbolu, jako je 777777.</span><span class="sxs-lookup"><span data-stu-id="809bb-141">Otherwise, enter a placeholder value, like 777777.</span></span>  <span data-ttu-id="809bb-142">Pokud cílíte na Android, můžete je aktualizovat tuto hodnotu v config.xml později.</span><span class="sxs-lookup"><span data-stu-id="809bb-142">If you are targeting Android, you can update this value in config.xml later.</span></span>
4. <span data-ttu-id="809bb-143">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="809bb-143">Click **Add**.</span></span>

<span data-ttu-id="809bb-144">Modul plug-in nabízené je nyní nainstalován.</span><span class="sxs-lookup"><span data-stu-id="809bb-144">The push plugin is now installed.</span></span>

#### <a name="install-the-device-plugin"></a><span data-ttu-id="809bb-145">Instalace modulu plug-in zařízení</span><span class="sxs-lookup"><span data-stu-id="809bb-145">Install the device plugin</span></span>
<span data-ttu-id="809bb-146">Postupujte podle stejného postupu, který jste použili k instalaci modulu plug-in push.</span><span class="sxs-lookup"><span data-stu-id="809bb-146">Follow the same procedure you used to install the push plugin.</span></span>  <span data-ttu-id="809bb-147">Přidání modulu plug-in zařízení ze seznamu modulů plug-in jádra (klikněte na tlačítko **modulů plug-in** > **základní** ji najít).</span><span class="sxs-lookup"><span data-stu-id="809bb-147">Add the Device plugin from the Core plugins list (click **Plugins** > **Core** to find it).</span></span> <span data-ttu-id="809bb-148">Je nutné tento modul plug-in získat název platformy.</span><span class="sxs-lookup"><span data-stu-id="809bb-148">You need this plugin to obtain the platform name.</span></span>

#### <a name="register-your-device-on-application-start-up"></a><span data-ttu-id="809bb-149">Zaregistrovat zařízení při spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="809bb-149">Register your device on application start-up</span></span>
<span data-ttu-id="809bb-150">Na začátku jsme obsahovat určitý minimální kód pro Android.</span><span class="sxs-lookup"><span data-stu-id="809bb-150">Initially, we include some minimal code for Android.</span></span> <span data-ttu-id="809bb-151">Později upravte aplikaci spustit v iOS nebo Windows 10.</span><span class="sxs-lookup"><span data-stu-id="809bb-151">Later, modify the app to run on iOS or Windows 10.</span></span>

1. <span data-ttu-id="809bb-152">Přidejte volání **registerForPushNotifications** během zpětného volání pro proces přihlášení nebo v dolní části **onDeviceReady** metoda:</span><span class="sxs-lookup"><span data-stu-id="809bb-152">Add a call to **registerForPushNotifications** during the callback for the login process, or at the bottom of  the **onDeviceReady** method:</span></span>

        // Login to the service.
        client.login('google')
            .then(function () {
                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                    // Added to register for push notifications.
                registerForPushNotifications();

            }, handleError);

    <span data-ttu-id="809bb-153">Tento příklad ukazuje volání **registerForPushNotifications** po úspěšném provedení ověřování.</span><span class="sxs-lookup"><span data-stu-id="809bb-153">This example shows calling **registerForPushNotifications** after authentication succeeds.</span></span>  <span data-ttu-id="809bb-154">Můžete volat `registerForPushNotifications()` tak často, jako je povinný.</span><span class="sxs-lookup"><span data-stu-id="809bb-154">You can call `registerForPushNotifications()` as often as is required.</span></span>

2. <span data-ttu-id="809bb-155">Přidejte nové **registerForPushNotifications** metoda následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="809bb-155">Add the new **registerForPushNotifications** method as follows:</span></span>

        // Register for Push Notifications. Requires that phonegap-plugin-push be installed.
        var pushRegistration = null;
        function registerForPushNotifications() {
          pushRegistration = PushNotification.init({
              android: { senderID: 'Your_Project_ID' },
              ios: { alert: 'true', badge: 'true', sound: 'true' },
              wns: {}
          });

        // Handle the registration event.
        pushRegistration.on('registration', function (data) {
          // Get the native platform of the device.
          var platform = device.platform;
          // Get the handle returned during registration.
          var handle = data.registrationId;
          // Set the device-specific message template.
          if (platform == 'android' || platform == 'Android') {
              // Register for GCM notifications.
              client.push.register('gcm', handle, {
                  mytemplate: { body: { data: { message: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'iOS') {
              // Register for notifications.
              client.push.register('apns', handle, {
                  mytemplate: { body: { aps: { alert: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'windows') {
              // Register for WNS notifications.
              client.push.register('wns', handle, {
                  myTemplate: {
                      body: '<toast><visual><binding template="ToastText01"><text id="1">$(messageParam)</text></binding></visual></toast>',
                      headers: { 'X-WNS-Type': 'wns/toast' } }
              });
          }
        });

        pushRegistration.on('notification', function (data, d2) {
          alert('Push Received: ' + data.message);
        });

        pushRegistration.on('error', handleError);
        }
3. <span data-ttu-id="809bb-156">(Android) V předchozím kódu, nahraďte `Your_Project_ID` s numerická projektu ID pro vaši aplikaci z [vývojářské konzole Google][18].</span><span class="sxs-lookup"><span data-stu-id="809bb-156">(Android) In the preceding code, replace `Your_Project_ID` with the numeric project ID for your app from the  [Google Developer Console][18].</span></span>

## <a name="optional-configure-and-run-the-app-on-android"></a><span data-ttu-id="809bb-157">(Volitelné) Konfigurace a spuštění aplikace v systému Android</span><span class="sxs-lookup"><span data-stu-id="809bb-157">(Optional) Configure and run the app on Android</span></span>
<span data-ttu-id="809bb-158">Dokončení této části ke zprovoznění nabízených oznámení pro Android.</span><span class="sxs-lookup"><span data-stu-id="809bb-158">Complete this section to enable push notifications for Android.</span></span>

#### <span data-ttu-id="809bb-159"><a name="enable-gcm"></a>Povolit Firebase cloudu zasílání zpráv</span><span class="sxs-lookup"><span data-stu-id="809bb-159"><a name="enable-gcm"></a>Enable Firebase Cloud Messaging</span></span>
<span data-ttu-id="809bb-160">Vzhledem k tomu, že jsme se původně cílené na platformu Google Android, musíte povolit zasílání zpráv cloudu Firebase.</span><span class="sxs-lookup"><span data-stu-id="809bb-160">Since we are targeting the Google Android platform initially, you must enable Firebase Cloud Messaging.</span></span>

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

#### <span data-ttu-id="809bb-161"><a name="configure-backend"></a>Konfigurace back-end mobilní aplikace k odeslání žádosti o nabízenou pomocí FCM</span><span class="sxs-lookup"><span data-stu-id="809bb-161"><a name="configure-backend"></a>Configure the Mobile App backend to send push requests using FCM</span></span>
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

#### <a name="configure-your-cordova-app-for-android"></a><span data-ttu-id="809bb-162">Konfigurace aplikace Cordova pro Android</span><span class="sxs-lookup"><span data-stu-id="809bb-162">Configure your Cordova app for Android</span></span>
<span data-ttu-id="809bb-163">V aplikaci Cordova otevřete config.xml a nahraďte `Your_Project_ID` s numerická projektu ID pro vaši aplikaci z [vývojářské konzole Google][18].</span><span class="sxs-lookup"><span data-stu-id="809bb-163">In your Cordova app, open config.xml and replace `Your_Project_ID` with the numeric project ID for your app from the [Google Developer Console][18].</span></span>

        <plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
            <variable name="SENDER_ID" value="Your_Project_ID" />
        </plugin>

<span data-ttu-id="809bb-164">Otevřete index.js a aktualizujte kód, který použije vaše ID číselné projektu.</span><span class="sxs-lookup"><span data-stu-id="809bb-164">Open index.js and update the code to use your numeric project ID.</span></span>

        pushRegistration = PushNotification.init({
            android: { senderID: 'Your_Project_ID' },
            ios: { alert: 'true', badge: 'true', sound: 'true' },
            wns: {}
        });

#### <span data-ttu-id="809bb-165"><a name="configure-device"></a>Konfigurace zařízení s Androidem pro ladění USB</span><span class="sxs-lookup"><span data-stu-id="809bb-165"><a name="configure-device"></a>Configure your Android device for USB debugging</span></span>
<span data-ttu-id="809bb-166">Než bude možné nasadit aplikace do zařízení se systémem Android, budete muset povolit ladění USB.</span><span class="sxs-lookup"><span data-stu-id="809bb-166">Before you can deploy your application to your Android Device, you need to enable USB Debugging.</span></span>  <span data-ttu-id="809bb-167">Na váš telefon se systémem Android, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="809bb-167">Perform the following steps on your Android phone:</span></span>

1. <span data-ttu-id="809bb-168">Přejděte na **nastavení** > **o telefonu**, klepněte **číslo sestavení** dokud režim vývojáře je povolen (o sedm časy).</span><span class="sxs-lookup"><span data-stu-id="809bb-168">Go to **Settings** > **About phone**, then tap the **Build number** until developer mode is enabled  (about seven times).</span></span>
2. <span data-ttu-id="809bb-169">Zpět v **nastavení** > **možnosti pro vývojáře** povolit **ladění USB**, telefon s Androidem se potom připojují k vaší vývoj počítači pomocí kabelu USB.</span><span class="sxs-lookup"><span data-stu-id="809bb-169">Back in **Settings** > **Developer Options** enable **USB debugging**, then connect your Android phone  to your development PC with a USB Cable.</span></span>

<span data-ttu-id="809bb-170">Jsme testovali to pomocí Google Nexus 5 X zařízení se systémem Android 6.0 (Marshmallow).</span><span class="sxs-lookup"><span data-stu-id="809bb-170">We tested this using a Google Nexus 5X device running Android 6.0 (Marshmallow).</span></span>  <span data-ttu-id="809bb-171">Technik jsou však společné pro všechny moderní Android verze.</span><span class="sxs-lookup"><span data-stu-id="809bb-171">However, the techniques are common across any modern Android release.</span></span>

#### <a name="install-google-play-services"></a><span data-ttu-id="809bb-172">Nainstalujte služby Google Play</span><span class="sxs-lookup"><span data-stu-id="809bb-172">Install Google Play Services</span></span>
<span data-ttu-id="809bb-173">Modul plug-in nabízené spoléhá na Android služby Google Play pro nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="809bb-173">The push plugin relies on Android Google Play Services for push notifications.</span></span>

1. <span data-ttu-id="809bb-174">V sadě Visual Studio, klikněte na tlačítko **nástroje** > **Android** > **Android SDK Manager**, rozbalte **funkce** složka a zaškrtněte políčka zajistí každé z následujících sad SDK je nainstalována.</span><span class="sxs-lookup"><span data-stu-id="809bb-174">In Visual Studio, click **Tools** > **Android** > **Android SDK Manager**, expand the **Extras** folder and  check the box to make sure that each of the following SDKs is installed.</span></span>

   * <span data-ttu-id="809bb-175">Android 2.3 nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="809bb-175">Android 2.3 or higher</span></span>
   * <span data-ttu-id="809bb-176">Revize Google úložiště 27 nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="809bb-176">Google Repository revision 27 or higher</span></span>
   * <span data-ttu-id="809bb-177">Služby Google Play 9.0.2 nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="809bb-177">Google Play Services 9.0.2 or higher</span></span>

2. <span data-ttu-id="809bb-178">Klikněte na tlačítko **instalace balíčků** a počkejte na dokončení instalace.</span><span class="sxs-lookup"><span data-stu-id="809bb-178">Click **Install Packages** and wait for the installation to complete.</span></span>

<span data-ttu-id="809bb-179">Aktuální požadované knihovny jsou uvedeny v [phonegap-plugin nabízené instalace dokumentace][19].</span><span class="sxs-lookup"><span data-stu-id="809bb-179">The current required libraries are listed in the [phonegap-plugin-push installation documentation][19].</span></span>

#### <a name="test-push-notifications-in-the-app-on-android"></a><span data-ttu-id="809bb-180">Nabízená oznámení v aplikaci pro Android</span><span class="sxs-lookup"><span data-stu-id="809bb-180">Test push notifications in the app on Android</span></span>
<span data-ttu-id="809bb-181">Spuštěním aplikace a vložení položek v tabulce TodoItem můžete nyní nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="809bb-181">You can now test push notifications by running the app and inserting items in the TodoItem table.</span></span> <span data-ttu-id="809bb-182">Ze stejného zařízení nebo z druhé zařízení, můžete otestovat tak dlouho, dokud používají stejný back-end.</span><span class="sxs-lookup"><span data-stu-id="809bb-182">You can test from the same device or from a second device, as long as you are using the same backend.</span></span> <span data-ttu-id="809bb-183">Testování aplikace Cordova na platformě Android v jednom z následujících způsobů:</span><span class="sxs-lookup"><span data-stu-id="809bb-183">Test your Cordova app on the Android platform in one of the following ways:</span></span>

* <span data-ttu-id="809bb-184">**Na fyzické zařízení:** přiřadit vývojovém počítači pomocí kabelu USB zařízení se systémem Android.</span><span class="sxs-lookup"><span data-stu-id="809bb-184">**On a physical device:** Attach your Android device to your development computer with a USB cable.</span></span>  <span data-ttu-id="809bb-185">Místo **emulátor Google Android**, vyberte **zařízení**.</span><span class="sxs-lookup"><span data-stu-id="809bb-185">Instead of **Google Android Emulator**, select **Device**.</span></span> <span data-ttu-id="809bb-186">Visual Studio nasadí aplikaci do zařízení a pak aplikaci spustí.</span><span class="sxs-lookup"><span data-stu-id="809bb-186">Visual Studio deploys the application to the device and then runs the application.</span></span>  <span data-ttu-id="809bb-187">Potom můžete pracovat s aplikací na zařízení.</span><span class="sxs-lookup"><span data-stu-id="809bb-187">You can then interact with the application on the device.</span></span>

  <span data-ttu-id="809bb-188">Zlepšení vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="809bb-188">Improve your development experience.</span></span>  <span data-ttu-id="809bb-189">Sdílení aplikací, jako obrazovky [Mobizen] [ 20] vám může pomoci při vývoji aplikace platformy Android.</span><span class="sxs-lookup"><span data-stu-id="809bb-189">Screen sharing applications such as [Mobizen][20] can assist you in developing an Android application.</span></span>  <span data-ttu-id="809bb-190">Mobizen projektů Android obrazovky na webový prohlížeč ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="809bb-190">Mobizen projects your Android screen to a web browser on your PC.</span></span>

* <span data-ttu-id="809bb-191">**V emulátoru Androidu:** existují další konfigurační kroky potřebné při spuštění v emulátoru.</span><span class="sxs-lookup"><span data-stu-id="809bb-191">**On an Android emulator:** There are additional configuration steps required when running on an emulator.</span></span>

    <span data-ttu-id="809bb-192">Ujistěte se, že nasazujete virtuální zařízení, která má rozhraní Google API nastavenou jako cíl, jak je vidět ve Správci virtuální zařízení Android (AVD).</span><span class="sxs-lookup"><span data-stu-id="809bb-192">Make sure you are deploying to a virtual device that has Google APIs set as the target, as shown in the Android Virtual Device (AVD) manager.</span></span>

    ![](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

    <span data-ttu-id="809bb-193">Pokud chcete použít rychlejší x86 emulátoru, můžete [nainstalujte ovladač HAXM] [ 11] a nakonfigurujte emulátoru ji použít.</span><span class="sxs-lookup"><span data-stu-id="809bb-193">If you want to use a faster x86 emulator, you [install the HAXM driver][11] and configure the emulator to use it.</span></span>

    <span data-ttu-id="809bb-194">Kliknutím na Přidat účet Google do zařízení s Androidem **aplikace** > **nastavení** > **přidejte účet**, postupujte podle pokynů.</span><span class="sxs-lookup"><span data-stu-id="809bb-194">Add a Google account to the Android device by clicking **Apps** > **Settings** > **Add account**, then follow the prompts.</span></span>

    ![](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

    <span data-ttu-id="809bb-195">Spusťte aplikaci seznamu úkolů jako před a vložit novou položku úkolů.</span><span class="sxs-lookup"><span data-stu-id="809bb-195">Run the todolist app as before and insert a new todo item.</span></span> <span data-ttu-id="809bb-196">Tentokrát ikonu oznámení se zobrazí v oznamovací oblasti.</span><span class="sxs-lookup"><span data-stu-id="809bb-196">This time, a notification icon is displayed in the notification area.</span></span> <span data-ttu-id="809bb-197">Můžete otevřít panel oznámení k zobrazení textu v plném znění oznámení.</span><span class="sxs-lookup"><span data-stu-id="809bb-197">You can open the notification drawer to view the full text of the notification.</span></span>

    ![](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

## <a name="optional-configure-and-run-on-ios"></a><span data-ttu-id="809bb-198">(Volitelné) Nakonfigurujte a spusťte v systému iOS</span><span class="sxs-lookup"><span data-stu-id="809bb-198">(Optional) Configure and run on iOS</span></span>
<span data-ttu-id="809bb-199">Tato část se týká spuštění projektu Cordova na zařízení s iOS.</span><span class="sxs-lookup"><span data-stu-id="809bb-199">This section is for running the Cordova project on iOS devices.</span></span> <span data-ttu-id="809bb-200">Pokud nepracujete s zařízení s iOS, můžete tuto část přeskočit.</span><span class="sxs-lookup"><span data-stu-id="809bb-200">If you are not working with iOS devices, you can skip this section.</span></span>

#### <a name="install-and-run-the-ios-remote-build-agent-on-a-mac-or-cloud-service"></a><span data-ttu-id="809bb-201">Nainstalujte a spusťte agenta vzdáleného sestavení iOS Mac nebo cloudové služby</span><span class="sxs-lookup"><span data-stu-id="809bb-201">Install and run the iOS remote build agent on a Mac or cloud service</span></span>
<span data-ttu-id="809bb-202">Před spuštěním aplikace Cordova v iOS pomocí sady Visual Studio projít kroky v [Průvodce nastavením iOS] [ 12] k instalaci a spuštění agenta vzdáleného sestavení.</span><span class="sxs-lookup"><span data-stu-id="809bb-202">Before you can run a Cordova app on iOS using Visual Studio, go through the steps in the [iOS Setup Guide][12] to install and run the remote build agent.</span></span>

<span data-ttu-id="809bb-203">Ujistěte se, že můžete vytvořit aplikaci pro iOS.</span><span class="sxs-lookup"><span data-stu-id="809bb-203">Make sure you can build the app for iOS.</span></span> <span data-ttu-id="809bb-204">K sestavení pro iOS ze sady Visual Studio je potřeba udělat kroky v Průvodci instalací.</span><span class="sxs-lookup"><span data-stu-id="809bb-204">The steps in the setup guide are required to build for iOS from Visual Studio.</span></span> <span data-ttu-id="809bb-205">Pokud nemáte algoritmu Mac, můžete vytvořit pro iOS pomocí agenta vzdáleného sestavení na službě, jako je MacInCloud.</span><span class="sxs-lookup"><span data-stu-id="809bb-205">If you do not have a Mac, you can build for iOS using the remote build agent on a service like MacInCloud.</span></span> <span data-ttu-id="809bb-206">Další informace najdete v tématu [spuštění aplikace pro iOS v cloudu][21].</span><span class="sxs-lookup"><span data-stu-id="809bb-206">For more info, see [Run your iOS app in the cloud][21].</span></span>

> [!NOTE]
> <span data-ttu-id="809bb-207">XCode 7 nebo novější, je potřeba použít modul plug-in nabízené v systému iOS.</span><span class="sxs-lookup"><span data-stu-id="809bb-207">XCode 7 or greater is required to use the push plugin on iOS.</span></span>

#### <a name="find-the-id-to-use-as-your-app-id"></a><span data-ttu-id="809bb-208">Najít ID, aby vám poskytla vaše ID aplikace</span><span class="sxs-lookup"><span data-stu-id="809bb-208">Find the ID to use as your App ID</span></span>
<span data-ttu-id="809bb-209">Najít před registrace aplikace pro nabízená oznámení, otevřete config.xml v aplikaci Cordova `id` atribut hodnota v elementu pomůcky a zkopírujte jej pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="809bb-209">Before you register your app for push notifications, open config.xml in your Cordova app, find the `id` attribute value in the widget element, and copy it for later use.</span></span> <span data-ttu-id="809bb-210">V následující soubor XML, je ID `io.cordova.myapp7777777`.</span><span class="sxs-lookup"><span data-stu-id="809bb-210">In the following XML, the ID is `io.cordova.myapp7777777`.</span></span>

        <widget defaultlocale="en-US" id="io.cordova.myapp7777777"
          version="1.0.0" windows-packageVersion="1.1.0.0" xmlns="http://www.w3.org/ns/widgets"
            xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:vs="http://schemas.microsoft.com/appx/2014/htmlapps">

<span data-ttu-id="809bb-211">Později použijte tento identifikátor při vytvoření ID aplikace na portálu pro vývojáře Apple.</span><span class="sxs-lookup"><span data-stu-id="809bb-211">Later, use this identifier when you create an App ID on Apple's developer portal.</span></span> <span data-ttu-id="809bb-212">Pokud vytvoříte jiné ID aplikace na portálu pro vývojáře, vyžaduje několik kroků navíc později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="809bb-212">If you create a different App ID on the developer portal, you must take a few extra steps later in this tutorial.</span></span> <span data-ttu-id="809bb-213">ID v elementu pomůcky se musí shodovat s ID aplikace na portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="809bb-213">The ID in the widget element must match the App ID on the developer portal.</span></span>

#### <a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a><span data-ttu-id="809bb-214">Registraci aplikace pro nabízená oznámení na portál pro vývojáře společnosti Apple</span><span class="sxs-lookup"><span data-stu-id="809bb-214">Register the app for push notifications on Apple's developer portal</span></span>
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

[<span data-ttu-id="809bb-215">Podívejte se na video zobrazující podobný postup.</span><span class="sxs-lookup"><span data-stu-id="809bb-215">Watch a video showing similar steps</span></span>](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-5-Set-up-apns-for-push)

#### <a name="configure-azure-to-send-push-notifications"></a><span data-ttu-id="809bb-216">Konfigurace Azure k odesílání nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="809bb-216">Configure Azure to send push notifications</span></span>
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

#### <a name="verify-that-your-app-id-matches-your-cordova-app"></a><span data-ttu-id="809bb-217">Zkontrolujte, zda vaše ID aplikace odpovídá vaší aplikace Cordova</span><span class="sxs-lookup"><span data-stu-id="809bb-217">Verify that your App ID matches your Cordova app</span></span>
<span data-ttu-id="809bb-218">Pokud jste vytvořili už ve vašem účtu vývojáře Apple ID aplikace odpovídá ID elementu pomůcka v config.xml, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="809bb-218">If the App ID you created in your Apple Developer Account already matches the ID of the widget element in config.xml, you can skip this step.</span></span> <span data-ttu-id="809bb-219">Pokud ID neshodují, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="809bb-219">However, if the IDs don't match, take the following steps:</span></span>

1. <span data-ttu-id="809bb-220">Odstraňte složku platformy ze svého projektu.</span><span class="sxs-lookup"><span data-stu-id="809bb-220">Delete the platforms folder from your project.</span></span>
2. <span data-ttu-id="809bb-221">Odstraňte složku moduly plug-in z projektu.</span><span class="sxs-lookup"><span data-stu-id="809bb-221">Delete the plugins folder from your project.</span></span>
3. <span data-ttu-id="809bb-222">Odstraňte složku node_modules ze svého projektu.</span><span class="sxs-lookup"><span data-stu-id="809bb-222">Delete the node_modules folder from your project.</span></span>
4. <span data-ttu-id="809bb-223">Aktualizujte atributu id elementu pomůcka v config.xml používat ID aplikace, kterou jste vytvořili ve vašem účtu vývojáře Apple.</span><span class="sxs-lookup"><span data-stu-id="809bb-223">Update the id attribute of the widget element in config.xml to use the App ID that you created in your  Apple Developer Account.</span></span>
5. <span data-ttu-id="809bb-224">Znovu sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="809bb-224">Rebuild your project.</span></span>

##### <a name="test-push-notifications-in-your-ios-app"></a><span data-ttu-id="809bb-225">Nabízená oznámení v aplikaci s iOS</span><span class="sxs-lookup"><span data-stu-id="809bb-225">Test push notifications in your iOS app</span></span>
1. <span data-ttu-id="809bb-226">V sadě Visual Studio, ujistěte se, že **iOS** je vybrán jako cíl nasazení a potom vyberte **zařízení** ke spuštění na vašem připojené zařízení s iOS.</span><span class="sxs-lookup"><span data-stu-id="809bb-226">In Visual Studio, make sure that **iOS** is selected as the deployment target, and then choose **Device** to run on your connected iOS device.</span></span>

    <span data-ttu-id="809bb-227">Můžete spustit na zařízení s iOS připojené k vašemu počítači pomocí iTunes.</span><span class="sxs-lookup"><span data-stu-id="809bb-227">You can run on an iOS device connected to your PC using iTunes.</span></span> <span data-ttu-id="809bb-228">Simulátoru iOS nabízená oznámení nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="809bb-228">The iOS Simulator does not support push notifications.</span></span>

2. <span data-ttu-id="809bb-229">Stiskněte **spustit** tlačítko nebo **F5** v sadě Visual Studio pro sestavení projektu a spusťte aplikaci v zařízení s iOS, pak klikněte na tlačítko **OK** přijímat nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="809bb-229">Press the **Run** button or **F5** in Visual Studio to build the project and start the app in an iOS  device, then click **OK** to accept push notifications.</span></span>

   > [!NOTE]
   > <span data-ttu-id="809bb-230">Aplikace požádá o potvrzení pro nabízená oznámení při prvním spuštění.</span><span class="sxs-lookup"><span data-stu-id="809bb-230">The app requests confirmation for push notifications during the first run.</span></span>

3. <span data-ttu-id="809bb-231">V aplikaci zadejte úlohu a potom klikněte na tlačítko plus (+) ikona.</span><span class="sxs-lookup"><span data-stu-id="809bb-231">In the app, type a task, and then click the plus (+) icon.</span></span>
4. <span data-ttu-id="809bb-232">Ověřte, že přijetí oznámení a potom kliknutím na tlačítko OK zavření oznámení.</span><span class="sxs-lookup"><span data-stu-id="809bb-232">Verify that a notification is received, then click OK to dismiss the notification.</span></span>

## <a name="optional-configure-and-run-on-windows"></a><span data-ttu-id="809bb-233">(Volitelné) Nakonfigurujte a spusťte v systému Windows</span><span class="sxs-lookup"><span data-stu-id="809bb-233">(Optional) Configure and run on Windows</span></span>
<span data-ttu-id="809bb-234">Tato část se týká spuštění projektu aplikace Apache Cordova na zařízení s Windows 10 (modul plug-in nabízené PhoneGap podporuje se ve Windows 10).</span><span class="sxs-lookup"><span data-stu-id="809bb-234">This section is for running the Apache Cordova app project on Windows 10 devices (the PhoneGap push plugin is supported on Windows 10).</span></span> <span data-ttu-id="809bb-235">Pokud nepracujete s zařízení se systémem Windows, můžete tuto část přeskočit.</span><span class="sxs-lookup"><span data-stu-id="809bb-235">If you are not working with Windows devices, you can skip this section.</span></span>

#### <a name="register-your-windows-app-for-push-notifications-with-wns"></a><span data-ttu-id="809bb-236">Registrace aplikace systému Windows pro nabízená oznámení s WNS</span><span class="sxs-lookup"><span data-stu-id="809bb-236">Register your Windows app for push notifications with WNS</span></span>
<span data-ttu-id="809bb-237">Použití možností úložiště v sadě Visual Studio, vyberte cíl Windows ze seznamu řešení platformy, jako je třeba **Windows x64** nebo **Windows x86** (vyhnout **Windows AnyCPU** pro nabízená oznámení).</span><span class="sxs-lookup"><span data-stu-id="809bb-237">To use the Store options in Visual Studio, select a Windows target from the Solution Platforms list, like **Windows-x64** or **Windows-x86** (avoid **Windows-AnyCPU** for push notifications).</span></span>

[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

<span data-ttu-id="809bb-238">[Podívejte se na video s podobným způsobem][13]</span><span class="sxs-lookup"><span data-stu-id="809bb-238">[Watch a video showing similar steps][13]</span></span>

#### <a name="configure-the-notification-hub-for-wns"></a><span data-ttu-id="809bb-239">Konfigurace centra oznámení pro WNS</span><span class="sxs-lookup"><span data-stu-id="809bb-239">Configure the notification hub for WNS</span></span>
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="configure-your-cordova-app-to-support-windows-push-notifications"></a><span data-ttu-id="809bb-240">Konfigurace aplikace Cordova pro podporu nabízených oznámení Windows</span><span class="sxs-lookup"><span data-stu-id="809bb-240">Configure your Cordova app to support Windows push notifications</span></span>
<span data-ttu-id="809bb-241">Otevřete návrháře konfigurace (klikněte pravým tlačítkem na config.xml a vyberte **Návrhář zobrazení**), vyberte **Windows** a klikněte na příkaz **Windows 10** pod  **Cílová verze Windows**.</span><span class="sxs-lookup"><span data-stu-id="809bb-241">Open the configuration designer (right-click on config.xml and select **View Designer**), select the **Windows** tab, and choose **Windows 10** under **Windows Target Version**.</span></span>

<span data-ttu-id="809bb-242">Pro podporu nabízených oznámení do výchozí (ladění) sestaveních otevřete build.json souboru.</span><span class="sxs-lookup"><span data-stu-id="809bb-242">To support push notifications in your default (debug) builds, open build.json file.</span></span> <span data-ttu-id="809bb-243">Konfigurace ladění zkopírujte konfigurace "verze".</span><span class="sxs-lookup"><span data-stu-id="809bb-243">Copy the "release" configuration to your debug configuration.</span></span>

        "windows": {
            "release": {
                "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
                "publisherId": "CN=yourpublisherID"
            }
        }

<span data-ttu-id="809bb-244">Po aktualizaci build.json by měl obsahovat následující kód:</span><span class="sxs-lookup"><span data-stu-id="809bb-244">After the update, the build.json should contain the following code:</span></span>

    "windows": {
        "release": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            },
        "debug": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            }
        }

<span data-ttu-id="809bb-245">Sestavte aplikaci a ověřte, že máte žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="809bb-245">Build the app and verify that you have no errors.</span></span> <span data-ttu-id="809bb-246">Klientská aplikace by teď zaregistrovat pro oznámení z back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="809bb-246">Your client app should now register for the notifications from the Mobile App backend.</span></span> <span data-ttu-id="809bb-247">Tato část opakujte pro každý projekt Windows ve vašem řešení.</span><span class="sxs-lookup"><span data-stu-id="809bb-247">Repeat this section for every Windows project in your solution.</span></span>

#### <a name="test-push-notifications-in-your-windows-app"></a><span data-ttu-id="809bb-248">Nabízená oznámení v aplikaci Windows</span><span class="sxs-lookup"><span data-stu-id="809bb-248">Test push notifications in your Windows app</span></span>
<span data-ttu-id="809bb-249">V sadě Visual Studio, musí být vybrána, platforma Windows cíl nasazení, jako například **Windows x64** nebo **Windows x86**.</span><span class="sxs-lookup"><span data-stu-id="809bb-249">In Visual Studio, make sure that a Windows platform is selected as the deployment target, such as **Windows-x64** or **Windows-x86**.</span></span> <span data-ttu-id="809bb-250">Spustit aplikaci na počítač s Windows 10 hostování sady Visual Studio, použijte příkaz **místního počítače**.</span><span class="sxs-lookup"><span data-stu-id="809bb-250">To run the app on a Windows 10 PC hosting Visual Studio, choose **Local Machine**.</span></span>

<span data-ttu-id="809bb-251">Klikněte na tlačítko spustit pro sestavení projektu a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="809bb-251">Press the Run button to build the project and start the app.</span></span>

<span data-ttu-id="809bb-252">V aplikaci, zadejte název nové todoitem a pak klikněte na tlačítko plus (+) ikona Přidat.</span><span class="sxs-lookup"><span data-stu-id="809bb-252">In the app, type a name for a new todoitem, and then click the plus (+) icon to add it.</span></span>

<span data-ttu-id="809bb-253">Ověřte, že se při přidání položky přijato oznámení.</span><span class="sxs-lookup"><span data-stu-id="809bb-253">Verify that a notification is received when the item is added.</span></span>

## <span data-ttu-id="809bb-254"><a name="next-steps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="809bb-254"><a name="next-steps"></a>Next Steps</span></span>
* <span data-ttu-id="809bb-255">Přečtěte si informace o [Notification Hubs] [ 17] Další informace o nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="809bb-255">Read about [Notification Hubs][17] to learn about push notifications.</span></span>
* <span data-ttu-id="809bb-256">Pokud jste tak již neučinili, pokračovat v kurzu podle [přidání ověřování] [ 14] do vaší aplikace Apache Cordova.</span><span class="sxs-lookup"><span data-stu-id="809bb-256">If you have not already done so, continue the tutorial by [Adding Authentication][14] to your Apache Cordova app.</span></span>

<span data-ttu-id="809bb-257">Zjistěte, jak používat sady SDK.</span><span class="sxs-lookup"><span data-stu-id="809bb-257">Learn how to use the SDKs.</span></span>

* <span data-ttu-id="809bb-258">[Apache Cordova SDK][15]</span><span class="sxs-lookup"><span data-stu-id="809bb-258">[Apache Cordova SDK][15]</span></span>
* <span data-ttu-id="809bb-259">[ASP.NET Server SDK][1]</span><span class="sxs-lookup"><span data-stu-id="809bb-259">[ASP.NET Server SDK][1]</span></span>
* <span data-ttu-id="809bb-260">[Node.js Server SDK][16]</span><span class="sxs-lookup"><span data-stu-id="809bb-260">[Node.js Server SDK][16]</span></span>

<!-- Images -->
[img1]: ./media/app-service-mobile-cordova-get-started-push/add-push-plugin.png

<!-- URLs -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: http://www.visualstudio.com/
[3]: https://azure.microsoft.com/pricing/free-trial/
[4]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[5]: app-service-mobile-cordova-get-started.md
[6]: http://go.microsoft.com/fwlink/p/?LinkId=268302
[7]: https://developer.apple.com/programs/
[8]: https://developer.microsoft.com/en-us/store/register
[9]: https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-3-Create-azure-notification-hub
[10]: https://www.npmjs.com/
[11]: https://taco.visualstudio.com/en-us/docs/run-app-apache/#HAXM
[12]: http://taco.visualstudio.com/en-us/docs/ios-guide/
[13]: https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-6-Set-up-wns-for-push
[14]: app-service-mobile-cordova-get-started-users.md
[15]: app-service-mobile-cordova-how-to-use-client-library.md
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[17]: ../notification-hubs/notification-hubs-push-notification-overview.md
[18]: https://console.developers.google.com/home/dashboard
[19]: https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md
[20]: https://www.mobizen.com/
[21]: http://taco.visualstudio.com/en-us/docs/build_ios_cloud/
