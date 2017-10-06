---
title: "tooApache aaaAdd nabízených oznámení aplikace Cordova s Azure Mobile Apps | Microsoft Docs"
description: "Zjistěte, jak Azure Mobile Apps toosend toouse nabízená oznámení tooyour Apache Cordova app."
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
ms.openlocfilehash: 8e1b23d6145b446b6f01599337b677e2f2b31d7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-push-notifications-tooyour-apache-cordova-app"></a><span data-ttu-id="36c32-103">Přidat nabízená oznámení tooyour Apache Cordova aplikaci</span><span class="sxs-lookup"><span data-stu-id="36c32-103">Add push notifications tooyour Apache Cordova app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="36c32-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="36c32-104">Overview</span></span>
<span data-ttu-id="36c32-105">V tomto kurzu přidáte projekt nabízených oznámení toohello [Apache Cordova úvodní] tak, aby nabízených oznámení je odesláno toohello zařízení pokaždé, když vložení záznamu.</span><span class="sxs-lookup"><span data-stu-id="36c32-105">In this tutorial, you add push notifications toohello [Apache Cordova quick start] project so that a push notification is sent toohello device every time a record is inserted.</span></span>

<span data-ttu-id="36c32-106">Pokud nepoužijete hello stáhli úvodní serverový projekt, třeba hello nabízených oznámení v balíčku rozšíření.</span><span class="sxs-lookup"><span data-stu-id="36c32-106">If you do not use hello downloaded quick start server project, you need hello push notification extension package.</span></span> <span data-ttu-id="36c32-107">Další informace najdete v tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps][1].</span><span class="sxs-lookup"><span data-stu-id="36c32-107">For more information, see [Work with hello .NET backend server SDK for Azure Mobile Apps][1].</span></span>

## <span data-ttu-id="36c32-108"><a name="prerequisites"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="36c32-108"><a name="prerequisites"></a>Prerequisites</span></span>
<span data-ttu-id="36c32-109">Tento kurz se zaměřuje Apache Cordova aplikace vytvořené s Visual Studiem 2015, která běží na hello emulátor Google Android, zařízení se systémem Android, zařízení se systémem Windows a zařízení s iOS.</span><span class="sxs-lookup"><span data-stu-id="36c32-109">This tutorial covers an Apache Cordova application developed with Visual Studio 2015 that runs on hello Google Android Emulator, an Android device, a Windows device, and an iOS device.</span></span>

<span data-ttu-id="36c32-110">toocomplete tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="36c32-110">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="36c32-111">Počítač s nástrojem [Visual Studio Community 2015] [ 2] nebo novější verze.</span><span class="sxs-lookup"><span data-stu-id="36c32-111">A PC with [Visual Studio Community 2015][2] or later versions.</span></span>
* <span data-ttu-id="36c32-112">[Nástroje sady Visual Studio pro Apache Cordova][4].</span><span class="sxs-lookup"><span data-stu-id="36c32-112">[Visual Studio Tools for Apache Cordova][4].</span></span>
* <span data-ttu-id="36c32-113">[Aktivní účet Azure][3].</span><span class="sxs-lookup"><span data-stu-id="36c32-113">An [active Azure account][3].</span></span>
* <span data-ttu-id="36c32-114">Dokončené [úvodní Apache Cordova] [ 5] projektu.</span><span class="sxs-lookup"><span data-stu-id="36c32-114">A completed [Apache Cordova quick start][5] project.</span></span>
* <span data-ttu-id="36c32-115">(Android) A [účet Google] [ 6] s ověřenou e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="36c32-115">(Android) A [Google account][6] with a verified email address.</span></span>
* <span data-ttu-id="36c32-116">(iOS) [Programu pro vývojáře Apple členství] [ 7] a zařízení se systémem iOS (simulátoru iOS push nepodporuje).</span><span class="sxs-lookup"><span data-stu-id="36c32-116">(iOS) An [Apple Developer Program membership][7] and an iOS device (iOS Simulator does not support push).</span></span>
* <span data-ttu-id="36c32-117">(Windows) A [Windows Store vývojářský účet] [ 8] a zařízením s Windows 10.</span><span class="sxs-lookup"><span data-stu-id="36c32-117">(Windows) A [Windows Store Developer Account][8] and a Windows 10 device.</span></span>

## <span data-ttu-id="36c32-118"><a name="configure-hub"></a>Konfigurace centra oznámení</span><span class="sxs-lookup"><span data-stu-id="36c32-118"><a name="configure-hub"></a>Configure a notification hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

<span data-ttu-id="36c32-119">[Přehrát video, zobrazující kroky v této části][9]</span><span class="sxs-lookup"><span data-stu-id="36c32-119">[Watch a video showing steps in this section][9]</span></span>

## <a name="update-hello-server-project"></a><span data-ttu-id="36c32-120">Aktualizace projektu server hello</span><span class="sxs-lookup"><span data-stu-id="36c32-120">Update hello server project</span></span>
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <span data-ttu-id="36c32-121"><a name="add-push-to-app"></a>Upravit aplikaci Cordova</span><span class="sxs-lookup"><span data-stu-id="36c32-121"><a name="add-push-to-app"></a>Modify your Cordova app</span></span>
<span data-ttu-id="36c32-122">Zkontrolujte, zda projektu aplikace Apache Cordova připraven toohandle nabízená oznámení pomocí instalaci hello nabízené cordovu plus žádné specifické pro platformu nabízených služeb.</span><span class="sxs-lookup"><span data-stu-id="36c32-122">Ensure your Apache Cordova app project is ready toohandle push notifications by installing hello Cordova push plugin plus any platform-specific push services.</span></span>

#### <a name="update-hello-cordova-version-in-your-project"></a><span data-ttu-id="36c32-123">Aktualizujte verzi Cordova hello ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="36c32-123">Update hello Cordova version in your project.</span></span>
<span data-ttu-id="36c32-124">Pokud váš projekt používá starší než v6.1.1 verzi aplikace Apache Cordova, aktualizujte hello klientského projektu.</span><span class="sxs-lookup"><span data-stu-id="36c32-124">If your project uses a version of Apache Cordova earlier than v6.1.1, update hello client project.</span></span> <span data-ttu-id="36c32-125">tooupdate hello projektu:</span><span class="sxs-lookup"><span data-stu-id="36c32-125">tooupdate hello project:</span></span>

* <span data-ttu-id="36c32-126">Klikněte pravým tlačítkem na `config.xml` tooopen hello configuration designer.</span><span class="sxs-lookup"><span data-stu-id="36c32-126">Right-click `config.xml` tooopen hello configuration designer.</span></span>
* <span data-ttu-id="36c32-127">Vyberte kartu platformy hello.</span><span class="sxs-lookup"><span data-stu-id="36c32-127">Select hello Platforms tab.</span></span>
* <span data-ttu-id="36c32-128">Zvolte 6.1.1 v hello **Cordova CLI** textové pole.</span><span class="sxs-lookup"><span data-stu-id="36c32-128">Choose 6.1.1 in hello **Cordova CLI** text box.</span></span>
* <span data-ttu-id="36c32-129">Zvolte **sestavení**, pak **sestavit řešení** tooupdate hello projektu.</span><span class="sxs-lookup"><span data-stu-id="36c32-129">Choose **Build**, then **Build Solution** tooupdate hello project.</span></span>

#### <a name="install-hello-push-plugin"></a><span data-ttu-id="36c32-130">Instalace modulu plug-in nabízené hello</span><span class="sxs-lookup"><span data-stu-id="36c32-130">Install hello push plugin</span></span>
<span data-ttu-id="36c32-131">Aplikace Apache Cordova nezpracuje nativně možnosti sítě nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="36c32-131">Apache Cordova applications do not natively handle device or network capabilities.</span></span>  <span data-ttu-id="36c32-132">Tyto možnosti jsou poskytovány buď modulů plug-in, které jsou publikovány na [npm] [ 10] nebo na Githubu.</span><span class="sxs-lookup"><span data-stu-id="36c32-132">These capabilities are provided by plugins that are published either on [npm][10] or on GitHub.</span></span>  <span data-ttu-id="36c32-133">Hello `phonegap-plugin-push` modul plug-in je použité toohandle sítě nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="36c32-133">hello `phonegap-plugin-push` plugin is used toohandle network push notifications.</span></span>

<span data-ttu-id="36c32-134">Modul plug-in nabízené hello můžete nainstalovat jedním z těchto způsobů:</span><span class="sxs-lookup"><span data-stu-id="36c32-134">You can install hello push plugin in one of these ways:</span></span>

<span data-ttu-id="36c32-135">**Z hello příkazového řádku:**</span><span class="sxs-lookup"><span data-stu-id="36c32-135">**From hello command-prompt:**</span></span>

<span data-ttu-id="36c32-136">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="36c32-136">Execute hello following command:</span></span>

    cordova plugin add phonegap-plugin-push

<span data-ttu-id="36c32-137">**Z v sadě Visual Studio:**</span><span class="sxs-lookup"><span data-stu-id="36c32-137">**From within Visual Studio:**</span></span>

1. <span data-ttu-id="36c32-138">V Průzkumníku řešení otevřete hello `config.xml` klikněte na soubor **modulů plug-in** > **vlastní**, vyberte **Git** jako zdroj instalace pak zadejte `https://github.com/phonegap/phonegap-plugin-push`jako zdroj hello.</span><span class="sxs-lookup"><span data-stu-id="36c32-138">In Solution Explorer, open hello `config.xml` file click **Plugins** > **Custom**, select **Git** as the  installation source, then enter `https://github.com/phonegap/phonegap-plugin-push` as hello source.</span></span>

   ![][img1]

2. <span data-ttu-id="36c32-139">Klikněte na tlačítko zdroj další toohello instalace hello šipku.</span><span class="sxs-lookup"><span data-stu-id="36c32-139">Click hello arrow next toohello installation source.</span></span>
3. <span data-ttu-id="36c32-140">V **SENDER_ID**, pokud již máte ID číselné projektu pro projekt hello vývojářské konzole Google, můžete přidat sem.</span><span class="sxs-lookup"><span data-stu-id="36c32-140">In **SENDER_ID**, if you already have a numeric project ID for hello Google Developer Console project, you can add it here.</span></span> <span data-ttu-id="36c32-141">Jinak zadejte hodnotu zástupného symbolu, jako je 777777.</span><span class="sxs-lookup"><span data-stu-id="36c32-141">Otherwise, enter a placeholder value, like 777777.</span></span>  <span data-ttu-id="36c32-142">Pokud cílíte na Android, můžete je aktualizovat tuto hodnotu v config.xml později.</span><span class="sxs-lookup"><span data-stu-id="36c32-142">If you are targeting Android, you can update this value in config.xml later.</span></span>
4. <span data-ttu-id="36c32-143">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="36c32-143">Click **Add**.</span></span>

<span data-ttu-id="36c32-144">modul plug-in nabízené Hello je nyní nainstalován.</span><span class="sxs-lookup"><span data-stu-id="36c32-144">hello push plugin is now installed.</span></span>

#### <a name="install-hello-device-plugin"></a><span data-ttu-id="36c32-145">Instalace modulu plug-in hello zařízení</span><span class="sxs-lookup"><span data-stu-id="36c32-145">Install hello device plugin</span></span>
<span data-ttu-id="36c32-146">Postupujte podle hello stejný postup používá tooinstall hello nabízené modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="36c32-146">Follow hello same procedure you used tooinstall hello push plugin.</span></span>  <span data-ttu-id="36c32-147">Přidání modulu plug-in hello zařízení ze seznamu modulů plug-in základní hello (klikněte na tlačítko **modulů plug-in** > **základní** toofind ji).</span><span class="sxs-lookup"><span data-stu-id="36c32-147">Add hello Device plugin from hello Core plugins list (click **Plugins** > **Core** toofind it).</span></span> <span data-ttu-id="36c32-148">Je třeba tento název modulu plug-in tooobtain hello platformy.</span><span class="sxs-lookup"><span data-stu-id="36c32-148">You need this plugin tooobtain hello platform name.</span></span>

#### <a name="register-your-device-on-application-start-up"></a><span data-ttu-id="36c32-149">Zaregistrovat zařízení při spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="36c32-149">Register your device on application start-up</span></span>
<span data-ttu-id="36c32-150">Na začátku jsme obsahovat určitý minimální kód pro Android.</span><span class="sxs-lookup"><span data-stu-id="36c32-150">Initially, we include some minimal code for Android.</span></span> <span data-ttu-id="36c32-151">Později upravte hello toorun aplikace v iOS nebo Windows 10.</span><span class="sxs-lookup"><span data-stu-id="36c32-151">Later, modify hello app toorun on iOS or Windows 10.</span></span>

1. <span data-ttu-id="36c32-152">Přidejte volání příliš**registerForPushNotifications** během hello zpětného volání pro hello procesu přihlášení, nebo na konci hello hello **onDeviceReady** metoda:</span><span class="sxs-lookup"><span data-stu-id="36c32-152">Add a call too**registerForPushNotifications** during hello callback for hello login process, or at hello bottom of  hello **onDeviceReady** method:</span></span>

        // Login toohello service.
        client.login('google')
            .then(function () {
                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh hello todoItems
                refreshDisplay();

                // Wire up hello UI Event Handler for hello Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                    // Added tooregister for push notifications.
                registerForPushNotifications();

            }, handleError);

    <span data-ttu-id="36c32-153">Tento příklad ukazuje volání **registerForPushNotifications** po úspěšném provedení ověřování.</span><span class="sxs-lookup"><span data-stu-id="36c32-153">This example shows calling **registerForPushNotifications** after authentication succeeds.</span></span>  <span data-ttu-id="36c32-154">Můžete volat `registerForPushNotifications()` tak často, jako je povinný.</span><span class="sxs-lookup"><span data-stu-id="36c32-154">You can call `registerForPushNotifications()` as often as is required.</span></span>

2. <span data-ttu-id="36c32-155">Přidat nové hello **registerForPushNotifications** metoda následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="36c32-155">Add hello new **registerForPushNotifications** method as follows:</span></span>

        // Register for Push Notifications. Requires that phonegap-plugin-push be installed.
        var pushRegistration = null;
        function registerForPushNotifications() {
          pushRegistration = PushNotification.init({
              android: { senderID: 'Your_Project_ID' },
              ios: { alert: 'true', badge: 'true', sound: 'true' },
              wns: {}
          });

        // Handle hello registration event.
        pushRegistration.on('registration', function (data) {
          // Get hello native platform of hello device.
          var platform = device.platform;
          // Get hello handle returned during registration.
          var handle = data.registrationId;
          // Set hello device-specific message template.
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
3. <span data-ttu-id="36c32-156">(Android) V předchozích kód hello, nahraďte `Your_Project_ID` s hello číselného projektu ID pro vaši aplikaci z [vývojářské konzole Google][18].</span><span class="sxs-lookup"><span data-stu-id="36c32-156">(Android) In hello preceding code, replace `Your_Project_ID` with hello numeric project ID for your app from the  [Google Developer Console][18].</span></span>

## <a name="optional-configure-and-run-hello-app-on-android"></a><span data-ttu-id="36c32-157">(Volitelné) Konfigurace a spuštění aplikace hello v systému Android</span><span class="sxs-lookup"><span data-stu-id="36c32-157">(Optional) Configure and run hello app on Android</span></span>
<span data-ttu-id="36c32-158">Dokončení této části tooenable nabízená oznámení pro Android.</span><span class="sxs-lookup"><span data-stu-id="36c32-158">Complete this section tooenable push notifications for Android.</span></span>

#### <span data-ttu-id="36c32-159"><a name="enable-gcm"></a>Povolit Firebase cloudu zasílání zpráv</span><span class="sxs-lookup"><span data-stu-id="36c32-159"><a name="enable-gcm"></a>Enable Firebase Cloud Messaging</span></span>
<span data-ttu-id="36c32-160">Vzhledem k tomu, že původně jsme jsou cílem hello Google Android platformu, musíte povolit zasílání zpráv cloudu Firebase.</span><span class="sxs-lookup"><span data-stu-id="36c32-160">Since we are targeting hello Google Android platform initially, you must enable Firebase Cloud Messaging.</span></span>

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

#### <span data-ttu-id="36c32-161"><a name="configure-backend"></a>Konfigurace hello mobilní aplikace back-end toosend nabízené požadavky pomocí FCM</span><span class="sxs-lookup"><span data-stu-id="36c32-161"><a name="configure-backend"></a>Configure hello Mobile App backend toosend push requests using FCM</span></span>
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

#### <a name="configure-your-cordova-app-for-android"></a><span data-ttu-id="36c32-162">Konfigurace aplikace Cordova pro Android</span><span class="sxs-lookup"><span data-stu-id="36c32-162">Configure your Cordova app for Android</span></span>
<span data-ttu-id="36c32-163">V aplikaci Cordova otevřete config.xml a nahraďte `Your_Project_ID` s hello číselného projektu ID pro vaši aplikaci z hello [vývojářské konzole Google][18].</span><span class="sxs-lookup"><span data-stu-id="36c32-163">In your Cordova app, open config.xml and replace `Your_Project_ID` with hello numeric project ID for your app from hello [Google Developer Console][18].</span></span>

        <plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
            <variable name="SENDER_ID" value="Your_Project_ID" />
        </plugin>

<span data-ttu-id="36c32-164">Otevřete index.js a aktualizujte hello kód toouse vaše ID číselné projektu.</span><span class="sxs-lookup"><span data-stu-id="36c32-164">Open index.js and update hello code toouse your numeric project ID.</span></span>

        pushRegistration = PushNotification.init({
            android: { senderID: 'Your_Project_ID' },
            ios: { alert: 'true', badge: 'true', sound: 'true' },
            wns: {}
        });

#### <span data-ttu-id="36c32-165"><a name="configure-device"></a>Konfigurace zařízení s Androidem pro ladění USB</span><span class="sxs-lookup"><span data-stu-id="36c32-165"><a name="configure-device"></a>Configure your Android device for USB debugging</span></span>
<span data-ttu-id="36c32-166">Před nasazením vaší aplikace tooyour zařízení s Androidem, musíte tooenable ladění USB.</span><span class="sxs-lookup"><span data-stu-id="36c32-166">Before you can deploy your application tooyour Android Device, you need tooenable USB Debugging.</span></span>  <span data-ttu-id="36c32-167">Na váš telefon se systémem Android, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="36c32-167">Perform the following steps on your Android phone:</span></span>

1. <span data-ttu-id="36c32-168">Přejděte příliš**nastavení** > **o telefonu**, potom klepněte na hello **číslo sestavení** dokud režim vývojáře je povolen (o sedm časy).</span><span class="sxs-lookup"><span data-stu-id="36c32-168">Go too**Settings** > **About phone**, then tap hello **Build number** until developer mode is enabled  (about seven times).</span></span>
2. <span data-ttu-id="36c32-169">Zpět v **nastavení** > **možnosti pro vývojáře** povolit **ladění USB**, připojte se váš telefon se systémem Android tooyour vývoj počítači pomocí kabelu USB.</span><span class="sxs-lookup"><span data-stu-id="36c32-169">Back in **Settings** > **Developer Options** enable **USB debugging**, then connect your Android phone  tooyour development PC with a USB Cable.</span></span>

<span data-ttu-id="36c32-170">Jsme testovali to pomocí Google Nexus 5 X zařízení se systémem Android 6.0 (Marshmallow).</span><span class="sxs-lookup"><span data-stu-id="36c32-170">We tested this using a Google Nexus 5X device running Android 6.0 (Marshmallow).</span></span>  <span data-ttu-id="36c32-171">Techniky hello jsou však společné pro všechny moderní Android verze.</span><span class="sxs-lookup"><span data-stu-id="36c32-171">However, hello techniques are common across any modern Android release.</span></span>

#### <a name="install-google-play-services"></a><span data-ttu-id="36c32-172">Nainstalujte služby Google Play</span><span class="sxs-lookup"><span data-stu-id="36c32-172">Install Google Play Services</span></span>
<span data-ttu-id="36c32-173">modul plug-in nabízené Hello spoléhá na Android služby Google Play pro nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="36c32-173">hello push plugin relies on Android Google Play Services for push notifications.</span></span>

1. <span data-ttu-id="36c32-174">V sadě Visual Studio, klikněte na tlačítko **nástroje** > **Android** > **Android SDK Manager**, rozbalte položku hello **funkce** složky a zkontrolujte hello pole toomake jistí, že každý z následujících sad SDK hello je nainstalován.</span><span class="sxs-lookup"><span data-stu-id="36c32-174">In Visual Studio, click **Tools** > **Android** > **Android SDK Manager**, expand hello **Extras** folder and  check hello box toomake sure that each of hello following SDKs is installed.</span></span>

   * <span data-ttu-id="36c32-175">Android 2.3 nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="36c32-175">Android 2.3 or higher</span></span>
   * <span data-ttu-id="36c32-176">Revize Google úložiště 27 nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="36c32-176">Google Repository revision 27 or higher</span></span>
   * <span data-ttu-id="36c32-177">Služby Google Play 9.0.2 nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="36c32-177">Google Play Services 9.0.2 or higher</span></span>

2. <span data-ttu-id="36c32-178">Klikněte na tlačítko **instalace balíčků** a počkejte toocomplete instalace hello.</span><span class="sxs-lookup"><span data-stu-id="36c32-178">Click **Install Packages** and wait for hello installation toocomplete.</span></span>

<span data-ttu-id="36c32-179">Hello aktuální požadované knihovny jsou uvedeny v hello [phonegap-plugin nabízené instalace dokumentace][19].</span><span class="sxs-lookup"><span data-stu-id="36c32-179">hello current required libraries are listed in hello [phonegap-plugin-push installation documentation][19].</span></span>

#### <a name="test-push-notifications-in-hello-app-on-android"></a><span data-ttu-id="36c32-180">Nabízená oznámení v aplikaci hello v systému Android</span><span class="sxs-lookup"><span data-stu-id="36c32-180">Test push notifications in hello app on Android</span></span>
<span data-ttu-id="36c32-181">Můžete teď nabízená oznámení spuštěním hello aplikace a vložení položek v tabulce TodoItem hello.</span><span class="sxs-lookup"><span data-stu-id="36c32-181">You can now test push notifications by running hello app and inserting items in hello TodoItem table.</span></span> <span data-ttu-id="36c32-182">Můžete otestovat z hello stejné zařízení nebo z druhé zařízení, tak dlouho, dokud používáte hello stejný back-end.</span><span class="sxs-lookup"><span data-stu-id="36c32-182">You can test from hello same device or from a second device, as long as you are using hello same backend.</span></span> <span data-ttu-id="36c32-183">Testování aplikace Cordova na platformě Android hello v jednom z následujících způsobů hello:</span><span class="sxs-lookup"><span data-stu-id="36c32-183">Test your Cordova app on hello Android platform in one of hello following ways:</span></span>

* <span data-ttu-id="36c32-184">**Na fyzické zařízení:** připojit zařízení se systémem Android tooyour vývojovém počítači pomocí kabelu USB.</span><span class="sxs-lookup"><span data-stu-id="36c32-184">**On a physical device:** Attach your Android device tooyour development computer with a USB cable.</span></span>  <span data-ttu-id="36c32-185">Místo **emulátor Google Android**, vyberte **zařízení**.</span><span class="sxs-lookup"><span data-stu-id="36c32-185">Instead of **Google Android Emulator**, select **Device**.</span></span> <span data-ttu-id="36c32-186">Visual Studio nasadí hello aplikace toohello zařízení a pak spustí aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="36c32-186">Visual Studio deploys hello application toohello device and then runs hello application.</span></span>  <span data-ttu-id="36c32-187">Potom můžete pracovat s aplikací hello na hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="36c32-187">You can then interact with hello application on hello device.</span></span>

  <span data-ttu-id="36c32-188">Zlepšení vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="36c32-188">Improve your development experience.</span></span>  <span data-ttu-id="36c32-189">Sdílení aplikací, jako obrazovky [Mobizen] [ 20] vám může pomoci při vývoji aplikace platformy Android.</span><span class="sxs-lookup"><span data-stu-id="36c32-189">Screen sharing applications such as [Mobizen][20] can assist you in developing an Android application.</span></span>  <span data-ttu-id="36c32-190">Mobizen projekty webového prohlížeče tooa Android obrazovky ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="36c32-190">Mobizen projects your Android screen tooa web browser on your PC.</span></span>

* <span data-ttu-id="36c32-191">**V emulátoru Androidu:** existují další konfigurační kroky potřebné při spuštění v emulátoru.</span><span class="sxs-lookup"><span data-stu-id="36c32-191">**On an Android emulator:** There are additional configuration steps required when running on an emulator.</span></span>

    <span data-ttu-id="36c32-192">Ujistěte se, že nasazujete tooa virtuální zařízení, která má nastavit jako cíl hello rozhraní Google API, jak je vidět ve Správci hello virtuální zařízení Android (AVD).</span><span class="sxs-lookup"><span data-stu-id="36c32-192">Make sure you are deploying tooa virtual device that has Google APIs set as hello target, as shown in hello Android Virtual Device (AVD) manager.</span></span>

    ![](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

    <span data-ttu-id="36c32-193">Pokud chcete toouse rychlejší x86 emulátoru, můžete [nainstalovat ovladač HAXM hello] [ 11] a nakonfigurujte hello emulátoru toouse ho.</span><span class="sxs-lookup"><span data-stu-id="36c32-193">If you want toouse a faster x86 emulator, you [install hello HAXM driver][11] and configure hello emulator toouse it.</span></span>

    <span data-ttu-id="36c32-194">Kliknutím na Přidat zařízení Android toohello účet Google **aplikace** > **nastavení** > **přidejte účet**, postupujte podle pokynů hello.</span><span class="sxs-lookup"><span data-stu-id="36c32-194">Add a Google account toohello Android device by clicking **Apps** > **Settings** > **Add account**, then follow hello prompts.</span></span>

    ![](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

    <span data-ttu-id="36c32-195">Spusťte aplikaci seznamu úkolů hello jako před a vložit novou položku úkolů.</span><span class="sxs-lookup"><span data-stu-id="36c32-195">Run hello todolist app as before and insert a new todo item.</span></span> <span data-ttu-id="36c32-196">Tentokrát ikonu oznámení se zobrazí v oznamovací oblasti hello.</span><span class="sxs-lookup"><span data-stu-id="36c32-196">This time, a notification icon is displayed in hello notification area.</span></span> <span data-ttu-id="36c32-197">Můžete otevřít hello oznámení nástroj drawer tooview hello textu v plném znění hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="36c32-197">You can open hello notification drawer tooview hello full text of hello notification.</span></span>

    ![](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

## <a name="optional-configure-and-run-on-ios"></a><span data-ttu-id="36c32-198">(Volitelné) Nakonfigurujte a spusťte v systému iOS</span><span class="sxs-lookup"><span data-stu-id="36c32-198">(Optional) Configure and run on iOS</span></span>
<span data-ttu-id="36c32-199">Tato část se týká spuštění projektu Cordova hello na zařízeních s iOS.</span><span class="sxs-lookup"><span data-stu-id="36c32-199">This section is for running hello Cordova project on iOS devices.</span></span> <span data-ttu-id="36c32-200">Pokud nepracujete s zařízení s iOS, můžete tuto část přeskočit.</span><span class="sxs-lookup"><span data-stu-id="36c32-200">If you are not working with iOS devices, you can skip this section.</span></span>

#### <a name="install-and-run-hello-ios-remote-build-agent-on-a-mac-or-cloud-service"></a><span data-ttu-id="36c32-201">Instalace a spuštění hello iOS vzdálené sestavení agenta na Mac nebo cloudové služby</span><span class="sxs-lookup"><span data-stu-id="36c32-201">Install and run hello iOS remote build agent on a Mac or cloud service</span></span>
<span data-ttu-id="36c32-202">Před spuštěním aplikace Cordova v iOS pomocí sady Visual Studio projít kroky hello v hello [Průvodce nastavením iOS] [ 12] tooinstall a vzdálené spuštění hello sestavení agenta.</span><span class="sxs-lookup"><span data-stu-id="36c32-202">Before you can run a Cordova app on iOS using Visual Studio, go through hello steps in hello [iOS Setup Guide][12] tooinstall and run hello remote build agent.</span></span>

<span data-ttu-id="36c32-203">Ujistěte se, že můžete vytvořit hello aplikace pro iOS.</span><span class="sxs-lookup"><span data-stu-id="36c32-203">Make sure you can build hello app for iOS.</span></span> <span data-ttu-id="36c32-204">Hello kroky v Průvodci instalací hello jsou požadované toobuild pro iOS ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="36c32-204">hello steps in hello setup guide are required toobuild for iOS from Visual Studio.</span></span> <span data-ttu-id="36c32-205">Pokud nemáte algoritmu Mac, můžete vytvořit pro iOS pomocí agenta vzdáleného sestavení hello na službě, jako je MacInCloud.</span><span class="sxs-lookup"><span data-stu-id="36c32-205">If you do not have a Mac, you can build for iOS using hello remote build agent on a service like MacInCloud.</span></span> <span data-ttu-id="36c32-206">Další informace najdete v tématu [spuštění aplikace pro iOS v cloudu hello][21].</span><span class="sxs-lookup"><span data-stu-id="36c32-206">For more info, see [Run your iOS app in hello cloud][21].</span></span>

> [!NOTE]
> <span data-ttu-id="36c32-207">XCode 7 nebo novější, je modul plug-in nabízené hello požadované toouse v systému iOS.</span><span class="sxs-lookup"><span data-stu-id="36c32-207">XCode 7 or greater is required toouse hello push plugin on iOS.</span></span>

#### <a name="find-hello-id-toouse-as-your-app-id"></a><span data-ttu-id="36c32-208">Najít hello ID toouse jako vaše ID aplikace</span><span class="sxs-lookup"><span data-stu-id="36c32-208">Find hello ID toouse as your App ID</span></span>
<span data-ttu-id="36c32-209">Než si zaregistrujete aplikaci pro nabízená oznámení, otevřete config.xml v aplikaci Cordova, najde hello `id` atribut hodnota v elementu pomůcky hello a zkopírujte jej pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="36c32-209">Before you register your app for push notifications, open config.xml in your Cordova app, find hello `id` attribute value in hello widget element, and copy it for later use.</span></span> <span data-ttu-id="36c32-210">V hello následující XML, je hello ID `io.cordova.myapp7777777`.</span><span class="sxs-lookup"><span data-stu-id="36c32-210">In hello following XML, hello ID is `io.cordova.myapp7777777`.</span></span>

        <widget defaultlocale="en-US" id="io.cordova.myapp7777777"
          version="1.0.0" windows-packageVersion="1.1.0.0" xmlns="http://www.w3.org/ns/widgets"
            xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:vs="http://schemas.microsoft.com/appx/2014/htmlapps">

<span data-ttu-id="36c32-211">Později použijte tento identifikátor při vytvoření ID aplikace na portálu pro vývojáře Apple.</span><span class="sxs-lookup"><span data-stu-id="36c32-211">Later, use this identifier when you create an App ID on Apple's developer portal.</span></span> <span data-ttu-id="36c32-212">Pokud vytvoříte na portálu pro vývojáře hello jiné ID aplikace, musí provést několik kroků navíc později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="36c32-212">If you create a different App ID on hello developer portal, you must take a few extra steps later in this tutorial.</span></span> <span data-ttu-id="36c32-213">ID Hello v elementu pomůcky musí odpovídat hello ID aplikace na portálu pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="36c32-213">hello ID in the widget element must match hello App ID on hello developer portal.</span></span>

#### <a name="register-hello-app-for-push-notifications-on-apples-developer-portal"></a><span data-ttu-id="36c32-214">Zaregistrovat hello aplikace pro nabízená oznámení na portál pro vývojáře společnosti Apple</span><span class="sxs-lookup"><span data-stu-id="36c32-214">Register hello app for push notifications on Apple's developer portal</span></span>
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

[<span data-ttu-id="36c32-215">Podívejte se na video zobrazující podobný postup.</span><span class="sxs-lookup"><span data-stu-id="36c32-215">Watch a video showing similar steps</span></span>](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-5-Set-up-apns-for-push)

#### <a name="configure-azure-toosend-push-notifications"></a><span data-ttu-id="36c32-216">Konfigurace Azure toosend nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="36c32-216">Configure Azure toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

#### <a name="verify-that-your-app-id-matches-your-cordova-app"></a><span data-ttu-id="36c32-217">Zkontrolujte, zda vaše ID aplikace odpovídá vaší aplikace Cordova</span><span class="sxs-lookup"><span data-stu-id="36c32-217">Verify that your App ID matches your Cordova app</span></span>
<span data-ttu-id="36c32-218">Pokud hello ID aplikace, které jste vytvořili v Apple vývojářský účet již odpovídá hello ID elementu hello pomůcka v config.xml, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="36c32-218">If hello App ID you created in your Apple Developer Account already matches hello ID of hello widget element in config.xml, you can skip this step.</span></span> <span data-ttu-id="36c32-219">Ale pokud hello ID neshodují, proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="36c32-219">However, if hello IDs don't match, take hello following steps:</span></span>

1. <span data-ttu-id="36c32-220">Odstraňte složku platformy hello ze svého projektu.</span><span class="sxs-lookup"><span data-stu-id="36c32-220">Delete hello platforms folder from your project.</span></span>
2. <span data-ttu-id="36c32-221">Odstraňte složku modulů plug-in hello ze svého projektu.</span><span class="sxs-lookup"><span data-stu-id="36c32-221">Delete hello plugins folder from your project.</span></span>
3. <span data-ttu-id="36c32-222">Odstraňte složku node_modules hello ze svého projektu.</span><span class="sxs-lookup"><span data-stu-id="36c32-222">Delete hello node_modules folder from your project.</span></span>
4. <span data-ttu-id="36c32-223">Aktualizujte hello atributu id elementu pomůcky hello config.xml toouse hello ID aplikace, kterou jste vytvořili ve vašem účtu vývojáře Apple.</span><span class="sxs-lookup"><span data-stu-id="36c32-223">Update hello id attribute of hello widget element in config.xml toouse hello App ID that you created in your  Apple Developer Account.</span></span>
5. <span data-ttu-id="36c32-224">Znovu sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="36c32-224">Rebuild your project.</span></span>

##### <a name="test-push-notifications-in-your-ios-app"></a><span data-ttu-id="36c32-225">Nabízená oznámení v aplikaci s iOS</span><span class="sxs-lookup"><span data-stu-id="36c32-225">Test push notifications in your iOS app</span></span>
1. <span data-ttu-id="36c32-226">V sadě Visual Studio, ujistěte se, že **iOS** je vybrán jako cíl hello nasazení a potom vyberte **zařízení** toorun na zařízení s iOS připojené.</span><span class="sxs-lookup"><span data-stu-id="36c32-226">In Visual Studio, make sure that **iOS** is selected as hello deployment target, and then choose **Device** toorun on your connected iOS device.</span></span>

    <span data-ttu-id="36c32-227">Můžete spustit na tooyour zařízení připojené iOS počítači pomocí iTunes.</span><span class="sxs-lookup"><span data-stu-id="36c32-227">You can run on an iOS device connected tooyour PC using iTunes.</span></span> <span data-ttu-id="36c32-228">simulátoru iOS Hello nabízená oznámení nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="36c32-228">hello iOS Simulator does not support push notifications.</span></span>

2. <span data-ttu-id="36c32-229">Stiskněte klávesu hello **spustit** tlačítko nebo **F5** v sadě Visual Studio toobuild hello aplikaci v zařízení se systémem iOS hello projektu a spusťte a potom klikněte na **OK** tooaccept nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="36c32-229">Press hello **Run** button or **F5** in Visual Studio toobuild hello project and start hello app in an iOS  device, then click **OK** tooaccept push notifications.</span></span>

   > [!NOTE]
   > <span data-ttu-id="36c32-230">aplikace Hello požádá o potvrzení pro nabízená oznámení při prvním spuštění hello.</span><span class="sxs-lookup"><span data-stu-id="36c32-230">hello app requests confirmation for push notifications during hello first run.</span></span>

3. <span data-ttu-id="36c32-231">V aplikaci hello, zadejte úlohu a potom klikněte na hello plus (+) ikona.</span><span class="sxs-lookup"><span data-stu-id="36c32-231">In hello app, type a task, and then click hello plus (+) icon.</span></span>
4. <span data-ttu-id="36c32-232">Ověřte, že přijetí oznámení a pak klikněte na OK toodismiss hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="36c32-232">Verify that a notification is received, then click OK toodismiss hello notification.</span></span>

## <a name="optional-configure-and-run-on-windows"></a><span data-ttu-id="36c32-233">(Volitelné) Nakonfigurujte a spusťte v systému Windows</span><span class="sxs-lookup"><span data-stu-id="36c32-233">(Optional) Configure and run on Windows</span></span>
<span data-ttu-id="36c32-234">Tato část se týká spuštění projektu aplikace hello Apache Cordova na zařízení s Windows 10 (modul plug-in nabízené hello PhoneGap podporuje se ve Windows 10).</span><span class="sxs-lookup"><span data-stu-id="36c32-234">This section is for running hello Apache Cordova app project on Windows 10 devices (hello PhoneGap push plugin is supported on Windows 10).</span></span> <span data-ttu-id="36c32-235">Pokud nepracujete s zařízení se systémem Windows, můžete tuto část přeskočit.</span><span class="sxs-lookup"><span data-stu-id="36c32-235">If you are not working with Windows devices, you can skip this section.</span></span>

#### <a name="register-your-windows-app-for-push-notifications-with-wns"></a><span data-ttu-id="36c32-236">Registrace aplikace systému Windows pro nabízená oznámení s WNS</span><span class="sxs-lookup"><span data-stu-id="36c32-236">Register your Windows app for push notifications with WNS</span></span>
<span data-ttu-id="36c32-237">možnosti úložiště hello toouse v sadě Visual Studio, vyberte cíl Windows hello seznamu řešení platformy, jako je třeba **Windows x64** nebo **Windows x86** (vyhnout **Windows AnyCPU** nabízená oznámení).</span><span class="sxs-lookup"><span data-stu-id="36c32-237">toouse hello Store options in Visual Studio, select a Windows target from hello Solution Platforms list, like **Windows-x64** or **Windows-x86** (avoid **Windows-AnyCPU** for push notifications).</span></span>

[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

<span data-ttu-id="36c32-238">[Podívejte se na video s podobným způsobem][13]</span><span class="sxs-lookup"><span data-stu-id="36c32-238">[Watch a video showing similar steps][13]</span></span>

#### <a name="configure-hello-notification-hub-for-wns"></a><span data-ttu-id="36c32-239">Konfigurace centra oznámení hello u WNS</span><span class="sxs-lookup"><span data-stu-id="36c32-239">Configure hello notification hub for WNS</span></span>
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="configure-your-cordova-app-toosupport-windows-push-notifications"></a><span data-ttu-id="36c32-240">Konfigurace aplikace Cordova toosupport Windows nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="36c32-240">Configure your Cordova app toosupport Windows push notifications</span></span>
<span data-ttu-id="36c32-241">Otevřete hello configuration designer (klikněte pravým tlačítkem na config.xml a vyberte **Návrhář zobrazení**), vyberte hello **Windows** a klikněte na příkaz **Windows 10** pod **Windows cílové verze**.</span><span class="sxs-lookup"><span data-stu-id="36c32-241">Open hello configuration designer (right-click on config.xml and select **View Designer**), select hello **Windows** tab, and choose **Windows 10** under **Windows Target Version**.</span></span>

<span data-ttu-id="36c32-242">sestaví toosupport nabízená oznámení do výchozí (ladění), otevřete build.json soubor.</span><span class="sxs-lookup"><span data-stu-id="36c32-242">toosupport push notifications in your default (debug) builds, open build.json file.</span></span> <span data-ttu-id="36c32-243">Zkopírujte "verze" Konfigurace tooyour ladění.</span><span class="sxs-lookup"><span data-stu-id="36c32-243">Copy the "release" configuration tooyour debug configuration.</span></span>

        "windows": {
            "release": {
                "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
                "publisherId": "CN=yourpublisherID"
            }
        }

<span data-ttu-id="36c32-244">Po aktualizaci hello hello build.json by měl obsahovat hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="36c32-244">After hello update, hello build.json should contain hello following code:</span></span>

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

<span data-ttu-id="36c32-245">Sestavení aplikace hello a ověřte, zda máte žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="36c32-245">Build hello app and verify that you have no errors.</span></span> <span data-ttu-id="36c32-246">Klientská aplikace by měl nyní zaregistrovat hello oznámení z back-end mobilní aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="36c32-246">Your client app should now register for hello notifications from hello Mobile App backend.</span></span> <span data-ttu-id="36c32-247">Tato část opakujte pro každý projekt Windows ve vašem řešení.</span><span class="sxs-lookup"><span data-stu-id="36c32-247">Repeat this section for every Windows project in your solution.</span></span>

#### <a name="test-push-notifications-in-your-windows-app"></a><span data-ttu-id="36c32-248">Nabízená oznámení v aplikaci Windows</span><span class="sxs-lookup"><span data-stu-id="36c32-248">Test push notifications in your Windows app</span></span>
<span data-ttu-id="36c32-249">V sadě Visual Studio, musí být vybrána, platformu Windows hello cíl nasazení, jako například **Windows x64** nebo **Windows x86**.</span><span class="sxs-lookup"><span data-stu-id="36c32-249">In Visual Studio, make sure that a Windows platform is selected as hello deployment target, such as **Windows-x64** or **Windows-x86**.</span></span> <span data-ttu-id="36c32-250">Zvolte toorun hello aplikace na počítači s Windows 10 hostování Visual Studio, **místního počítače**.</span><span class="sxs-lookup"><span data-stu-id="36c32-250">toorun hello app on a Windows 10 PC hosting Visual Studio, choose **Local Machine**.</span></span>

<span data-ttu-id="36c32-251">Stiskněte klávesu hello spusťte tlačítko toobuild hello projekt a spusťte aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="36c32-251">Press hello Run button toobuild hello project and start hello app.</span></span>

<span data-ttu-id="36c32-252">V hello aplikace, zadejte název nové todoitem a pak klikněte na hello plus (+) ikona tooadd ho.</span><span class="sxs-lookup"><span data-stu-id="36c32-252">In hello app, type a name for a new todoitem, and then click hello plus (+) icon tooadd it.</span></span>

<span data-ttu-id="36c32-253">Ověřte, že se při přidání položky hello přijato oznámení.</span><span class="sxs-lookup"><span data-stu-id="36c32-253">Verify that a notification is received when hello item is added.</span></span>

## <span data-ttu-id="36c32-254"><a name="next-steps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="36c32-254"><a name="next-steps"></a>Next Steps</span></span>
* <span data-ttu-id="36c32-255">Přečtěte si informace o [Notification Hubs] [ 17] toolearn o nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="36c32-255">Read about [Notification Hubs][17] toolearn about push notifications.</span></span>
* <span data-ttu-id="36c32-256">Pokud jste tak již neučinili, pokračovat v kurzu hello podle [přidání ověřování] [ 14] tooyour aplikace Apache Cordova.</span><span class="sxs-lookup"><span data-stu-id="36c32-256">If you have not already done so, continue hello tutorial by [Adding Authentication][14] tooyour Apache Cordova app.</span></span>

<span data-ttu-id="36c32-257">Zjistěte, jak toouse hello sady SDK.</span><span class="sxs-lookup"><span data-stu-id="36c32-257">Learn how toouse hello SDKs.</span></span>

* <span data-ttu-id="36c32-258">[Apache Cordova SDK][15]</span><span class="sxs-lookup"><span data-stu-id="36c32-258">[Apache Cordova SDK][15]</span></span>
* <span data-ttu-id="36c32-259">[ASP.NET Server SDK][1]</span><span class="sxs-lookup"><span data-stu-id="36c32-259">[ASP.NET Server SDK][1]</span></span>
* <span data-ttu-id="36c32-260">[Node.js Server SDK][16]</span><span class="sxs-lookup"><span data-stu-id="36c32-260">[Node.js Server SDK][16]</span></span>

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
