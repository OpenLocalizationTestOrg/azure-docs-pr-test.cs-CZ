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
# <a name="add-push-notifications-tooyour-apache-cordova-app"></a>Přidat nabízená oznámení tooyour Apache Cordova aplikaci
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Přehled
V tomto kurzu přidáte projekt nabízených oznámení toohello [Apache Cordova úvodní] tak, aby nabízených oznámení je odesláno toohello zařízení pokaždé, když vložení záznamu.

Pokud nepoužijete hello stáhli úvodní serverový projekt, třeba hello nabízených oznámení v balíčku rozšíření. Další informace najdete v tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps][1].

## <a name="prerequisites"></a>Požadavky
Tento kurz se zaměřuje Apache Cordova aplikace vytvořené s Visual Studiem 2015, která běží na hello emulátor Google Android, zařízení se systémem Android, zařízení se systémem Windows a zařízení s iOS.

toocomplete tohoto kurzu potřebujete:

* Počítač s nástrojem [Visual Studio Community 2015] [ 2] nebo novější verze.
* [Nástroje sady Visual Studio pro Apache Cordova][4].
* [Aktivní účet Azure][3].
* Dokončené [úvodní Apache Cordova] [ 5] projektu.
* (Android) A [účet Google] [ 6] s ověřenou e-mailovou adresu.
* (iOS) [Programu pro vývojáře Apple členství] [ 7] a zařízení se systémem iOS (simulátoru iOS push nepodporuje).
* (Windows) A [Windows Store vývojářský účet] [ 8] a zařízením s Windows 10.

## <a name="configure-hub"></a>Konfigurace centra oznámení
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

[Přehrát video, zobrazující kroky v této části][9]

## <a name="update-hello-server-project"></a>Aktualizace projektu server hello
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="add-push-to-app"></a>Upravit aplikaci Cordova
Zkontrolujte, zda projektu aplikace Apache Cordova připraven toohandle nabízená oznámení pomocí instalaci hello nabízené cordovu plus žádné specifické pro platformu nabízených služeb.

#### <a name="update-hello-cordova-version-in-your-project"></a>Aktualizujte verzi Cordova hello ve vašem projektu.
Pokud váš projekt používá starší než v6.1.1 verzi aplikace Apache Cordova, aktualizujte hello klientského projektu. tooupdate hello projektu:

* Klikněte pravým tlačítkem na `config.xml` tooopen hello configuration designer.
* Vyberte kartu platformy hello.
* Zvolte 6.1.1 v hello **Cordova CLI** textové pole.
* Zvolte **sestavení**, pak **sestavit řešení** tooupdate hello projektu.

#### <a name="install-hello-push-plugin"></a>Instalace modulu plug-in nabízené hello
Aplikace Apache Cordova nezpracuje nativně možnosti sítě nebo zařízení.  Tyto možnosti jsou poskytovány buď modulů plug-in, které jsou publikovány na [npm] [ 10] nebo na Githubu.  Hello `phonegap-plugin-push` modul plug-in je použité toohandle sítě nabízená oznámení.

Modul plug-in nabízené hello můžete nainstalovat jedním z těchto způsobů:

**Z hello příkazového řádku:**

Spusťte následující příkaz hello:

    cordova plugin add phonegap-plugin-push

**Z v sadě Visual Studio:**

1. V Průzkumníku řešení otevřete hello `config.xml` klikněte na soubor **modulů plug-in** > **vlastní**, vyberte **Git** jako zdroj instalace pak zadejte `https://github.com/phonegap/phonegap-plugin-push`jako zdroj hello.

   ![][img1]

2. Klikněte na tlačítko zdroj další toohello instalace hello šipku.
3. V **SENDER_ID**, pokud již máte ID číselné projektu pro projekt hello vývojářské konzole Google, můžete přidat sem. Jinak zadejte hodnotu zástupného symbolu, jako je 777777.  Pokud cílíte na Android, můžete je aktualizovat tuto hodnotu v config.xml později.
4. Klikněte na tlačítko **Přidat**.

modul plug-in nabízené Hello je nyní nainstalován.

#### <a name="install-hello-device-plugin"></a>Instalace modulu plug-in hello zařízení
Postupujte podle hello stejný postup používá tooinstall hello nabízené modulu plug-in.  Přidání modulu plug-in hello zařízení ze seznamu modulů plug-in základní hello (klikněte na tlačítko **modulů plug-in** > **základní** toofind ji). Je třeba tento název modulu plug-in tooobtain hello platformy.

#### <a name="register-your-device-on-application-start-up"></a>Zaregistrovat zařízení při spuštění aplikace
Na začátku jsme obsahovat určitý minimální kód pro Android. Později upravte hello toorun aplikace v iOS nebo Windows 10.

1. Přidejte volání příliš**registerForPushNotifications** během hello zpětného volání pro hello procesu přihlášení, nebo na konci hello hello **onDeviceReady** metoda:

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

    Tento příklad ukazuje volání **registerForPushNotifications** po úspěšném provedení ověřování.  Můžete volat `registerForPushNotifications()` tak často, jako je povinný.

2. Přidat nové hello **registerForPushNotifications** metoda následujícím způsobem:

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
3. (Android) V předchozích kód hello, nahraďte `Your_Project_ID` s hello číselného projektu ID pro vaši aplikaci z [vývojářské konzole Google][18].

## <a name="optional-configure-and-run-hello-app-on-android"></a>(Volitelné) Konfigurace a spuštění aplikace hello v systému Android
Dokončení této části tooenable nabízená oznámení pro Android.

#### <a name="enable-gcm"></a>Povolit Firebase cloudu zasílání zpráv
Vzhledem k tomu, že původně jsme jsou cílem hello Google Android platformu, musíte povolit zasílání zpráv cloudu Firebase.

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

#### <a name="configure-backend"></a>Konfigurace hello mobilní aplikace back-end toosend nabízené požadavky pomocí FCM
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

#### <a name="configure-your-cordova-app-for-android"></a>Konfigurace aplikace Cordova pro Android
V aplikaci Cordova otevřete config.xml a nahraďte `Your_Project_ID` s hello číselného projektu ID pro vaši aplikaci z hello [vývojářské konzole Google][18].

        <plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
            <variable name="SENDER_ID" value="Your_Project_ID" />
        </plugin>

Otevřete index.js a aktualizujte hello kód toouse vaše ID číselné projektu.

        pushRegistration = PushNotification.init({
            android: { senderID: 'Your_Project_ID' },
            ios: { alert: 'true', badge: 'true', sound: 'true' },
            wns: {}
        });

#### <a name="configure-device"></a>Konfigurace zařízení s Androidem pro ladění USB
Před nasazením vaší aplikace tooyour zařízení s Androidem, musíte tooenable ladění USB.  Na váš telefon se systémem Android, proveďte následující kroky:

1. Přejděte příliš**nastavení** > **o telefonu**, potom klepněte na hello **číslo sestavení** dokud režim vývojáře je povolen (o sedm časy).
2. Zpět v **nastavení** > **možnosti pro vývojáře** povolit **ladění USB**, připojte se váš telefon se systémem Android tooyour vývoj počítači pomocí kabelu USB.

Jsme testovali to pomocí Google Nexus 5 X zařízení se systémem Android 6.0 (Marshmallow).  Techniky hello jsou však společné pro všechny moderní Android verze.

#### <a name="install-google-play-services"></a>Nainstalujte služby Google Play
modul plug-in nabízené Hello spoléhá na Android služby Google Play pro nabízená oznámení.

1. V sadě Visual Studio, klikněte na tlačítko **nástroje** > **Android** > **Android SDK Manager**, rozbalte položku hello **funkce** složky a zkontrolujte hello pole toomake jistí, že každý z následujících sad SDK hello je nainstalován.

   * Android 2.3 nebo vyšší
   * Revize Google úložiště 27 nebo vyšší
   * Služby Google Play 9.0.2 nebo vyšší

2. Klikněte na tlačítko **instalace balíčků** a počkejte toocomplete instalace hello.

Hello aktuální požadované knihovny jsou uvedeny v hello [phonegap-plugin nabízené instalace dokumentace][19].

#### <a name="test-push-notifications-in-hello-app-on-android"></a>Nabízená oznámení v aplikaci hello v systému Android
Můžete teď nabízená oznámení spuštěním hello aplikace a vložení položek v tabulce TodoItem hello. Můžete otestovat z hello stejné zařízení nebo z druhé zařízení, tak dlouho, dokud používáte hello stejný back-end. Testování aplikace Cordova na platformě Android hello v jednom z následujících způsobů hello:

* **Na fyzické zařízení:** připojit zařízení se systémem Android tooyour vývojovém počítači pomocí kabelu USB.  Místo **emulátor Google Android**, vyberte **zařízení**. Visual Studio nasadí hello aplikace toohello zařízení a pak spustí aplikace hello.  Potom můžete pracovat s aplikací hello na hello zařízení.

  Zlepšení vývojové prostředí.  Sdílení aplikací, jako obrazovky [Mobizen] [ 20] vám může pomoci při vývoji aplikace platformy Android.  Mobizen projekty webového prohlížeče tooa Android obrazovky ve vašem počítači.

* **V emulátoru Androidu:** existují další konfigurační kroky potřebné při spuštění v emulátoru.

    Ujistěte se, že nasazujete tooa virtuální zařízení, která má nastavit jako cíl hello rozhraní Google API, jak je vidět ve Správci hello virtuální zařízení Android (AVD).

    ![](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

    Pokud chcete toouse rychlejší x86 emulátoru, můžete [nainstalovat ovladač HAXM hello] [ 11] a nakonfigurujte hello emulátoru toouse ho.

    Kliknutím na Přidat zařízení Android toohello účet Google **aplikace** > **nastavení** > **přidejte účet**, postupujte podle pokynů hello.

    ![](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

    Spusťte aplikaci seznamu úkolů hello jako před a vložit novou položku úkolů. Tentokrát ikonu oznámení se zobrazí v oznamovací oblasti hello. Můžete otevřít hello oznámení nástroj drawer tooview hello textu v plném znění hello oznámení.

    ![](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

## <a name="optional-configure-and-run-on-ios"></a>(Volitelné) Nakonfigurujte a spusťte v systému iOS
Tato část se týká spuštění projektu Cordova hello na zařízeních s iOS. Pokud nepracujete s zařízení s iOS, můžete tuto část přeskočit.

#### <a name="install-and-run-hello-ios-remote-build-agent-on-a-mac-or-cloud-service"></a>Instalace a spuštění hello iOS vzdálené sestavení agenta na Mac nebo cloudové služby
Před spuštěním aplikace Cordova v iOS pomocí sady Visual Studio projít kroky hello v hello [Průvodce nastavením iOS] [ 12] tooinstall a vzdálené spuštění hello sestavení agenta.

Ujistěte se, že můžete vytvořit hello aplikace pro iOS. Hello kroky v Průvodci instalací hello jsou požadované toobuild pro iOS ze sady Visual Studio. Pokud nemáte algoritmu Mac, můžete vytvořit pro iOS pomocí agenta vzdáleného sestavení hello na službě, jako je MacInCloud. Další informace najdete v tématu [spuštění aplikace pro iOS v cloudu hello][21].

> [!NOTE]
> XCode 7 nebo novější, je modul plug-in nabízené hello požadované toouse v systému iOS.

#### <a name="find-hello-id-toouse-as-your-app-id"></a>Najít hello ID toouse jako vaše ID aplikace
Než si zaregistrujete aplikaci pro nabízená oznámení, otevřete config.xml v aplikaci Cordova, najde hello `id` atribut hodnota v elementu pomůcky hello a zkopírujte jej pro pozdější použití. V hello následující XML, je hello ID `io.cordova.myapp7777777`.

        <widget defaultlocale="en-US" id="io.cordova.myapp7777777"
          version="1.0.0" windows-packageVersion="1.1.0.0" xmlns="http://www.w3.org/ns/widgets"
            xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:vs="http://schemas.microsoft.com/appx/2014/htmlapps">

Později použijte tento identifikátor při vytvoření ID aplikace na portálu pro vývojáře Apple. Pokud vytvoříte na portálu pro vývojáře hello jiné ID aplikace, musí provést několik kroků navíc později v tomto kurzu. ID Hello v elementu pomůcky musí odpovídat hello ID aplikace na portálu pro vývojáře hello.

#### <a name="register-hello-app-for-push-notifications-on-apples-developer-portal"></a>Zaregistrovat hello aplikace pro nabízená oznámení na portál pro vývojáře společnosti Apple
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

[Podívejte se na video zobrazující podobný postup.](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-5-Set-up-apns-for-push)

#### <a name="configure-azure-toosend-push-notifications"></a>Konfigurace Azure toosend nabízená oznámení
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

#### <a name="verify-that-your-app-id-matches-your-cordova-app"></a>Zkontrolujte, zda vaše ID aplikace odpovídá vaší aplikace Cordova
Pokud hello ID aplikace, které jste vytvořili v Apple vývojářský účet již odpovídá hello ID elementu hello pomůcka v config.xml, můžete tento krok přeskočit. Ale pokud hello ID neshodují, proveďte hello následující kroky:

1. Odstraňte složku platformy hello ze svého projektu.
2. Odstraňte složku modulů plug-in hello ze svého projektu.
3. Odstraňte složku node_modules hello ze svého projektu.
4. Aktualizujte hello atributu id elementu pomůcky hello config.xml toouse hello ID aplikace, kterou jste vytvořili ve vašem účtu vývojáře Apple.
5. Znovu sestavte projekt.

##### <a name="test-push-notifications-in-your-ios-app"></a>Nabízená oznámení v aplikaci s iOS
1. V sadě Visual Studio, ujistěte se, že **iOS** je vybrán jako cíl hello nasazení a potom vyberte **zařízení** toorun na zařízení s iOS připojené.

    Můžete spustit na tooyour zařízení připojené iOS počítači pomocí iTunes. simulátoru iOS Hello nabízená oznámení nepodporuje.

2. Stiskněte klávesu hello **spustit** tlačítko nebo **F5** v sadě Visual Studio toobuild hello aplikaci v zařízení se systémem iOS hello projektu a spusťte a potom klikněte na **OK** tooaccept nabízená oznámení.

   > [!NOTE]
   > aplikace Hello požádá o potvrzení pro nabízená oznámení při prvním spuštění hello.

3. V aplikaci hello, zadejte úlohu a potom klikněte na hello plus (+) ikona.
4. Ověřte, že přijetí oznámení a pak klikněte na OK toodismiss hello oznámení.

## <a name="optional-configure-and-run-on-windows"></a>(Volitelné) Nakonfigurujte a spusťte v systému Windows
Tato část se týká spuštění projektu aplikace hello Apache Cordova na zařízení s Windows 10 (modul plug-in nabízené hello PhoneGap podporuje se ve Windows 10). Pokud nepracujete s zařízení se systémem Windows, můžete tuto část přeskočit.

#### <a name="register-your-windows-app-for-push-notifications-with-wns"></a>Registrace aplikace systému Windows pro nabízená oznámení s WNS
možnosti úložiště hello toouse v sadě Visual Studio, vyberte cíl Windows hello seznamu řešení platformy, jako je třeba **Windows x64** nebo **Windows x86** (vyhnout **Windows AnyCPU** nabízená oznámení).

[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

[Podívejte se na video s podobným způsobem][13]

#### <a name="configure-hello-notification-hub-for-wns"></a>Konfigurace centra oznámení hello u WNS
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="configure-your-cordova-app-toosupport-windows-push-notifications"></a>Konfigurace aplikace Cordova toosupport Windows nabízených oznámení
Otevřete hello configuration designer (klikněte pravým tlačítkem na config.xml a vyberte **Návrhář zobrazení**), vyberte hello **Windows** a klikněte na příkaz **Windows 10** pod **Windows cílové verze**.

sestaví toosupport nabízená oznámení do výchozí (ladění), otevřete build.json soubor. Zkopírujte "verze" Konfigurace tooyour ladění.

        "windows": {
            "release": {
                "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
                "publisherId": "CN=yourpublisherID"
            }
        }

Po aktualizaci hello hello build.json by měl obsahovat hello následující kód:

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

Sestavení aplikace hello a ověřte, zda máte žádné chyby. Klientská aplikace by měl nyní zaregistrovat hello oznámení z back-end mobilní aplikace hello. Tato část opakujte pro každý projekt Windows ve vašem řešení.

#### <a name="test-push-notifications-in-your-windows-app"></a>Nabízená oznámení v aplikaci Windows
V sadě Visual Studio, musí být vybrána, platformu Windows hello cíl nasazení, jako například **Windows x64** nebo **Windows x86**. Zvolte toorun hello aplikace na počítači s Windows 10 hostování Visual Studio, **místního počítače**.

Stiskněte klávesu hello spusťte tlačítko toobuild hello projekt a spusťte aplikace hello.

V hello aplikace, zadejte název nové todoitem a pak klikněte na hello plus (+) ikona tooadd ho.

Ověřte, že se při přidání položky hello přijato oznámení.

## <a name="next-steps"></a>Další kroky
* Přečtěte si informace o [Notification Hubs] [ 17] toolearn o nabízená oznámení.
* Pokud jste tak již neučinili, pokračovat v kurzu hello podle [přidání ověřování] [ 14] tooyour aplikace Apache Cordova.

Zjistěte, jak toouse hello sady SDK.

* [Apache Cordova SDK][15]
* [ASP.NET Server SDK][1]
* [Node.js Server SDK][16]

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
