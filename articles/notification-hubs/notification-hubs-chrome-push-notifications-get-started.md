---
title: "aaaSend nabízená oznámení tooChrome aplikace pomocí Azure Notification Hubs | Microsoft Docs"
description: "Zjistěte, jak Azure Notification Hubs toosend toouse nabízená oznámení tooa aplikace pro Chrome."
services: notification-hubs
keywords: "mobilní nabízená oznámení,nabízená oznámení,nabízené oznámení,nabízená oznámení chrome"
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 75d4ff59-d04a-455f-bd44-0130a68e641f
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-chrome
ms.devlang: JavaScript
ms.topic: hero-article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: 7dec8ab02622563bc3730a2e96820da8932d22f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="send-push-notifications-toochrome-apps-with-azure-notification-hubs"></a>Odesílání nabízených oznámení tooChrome aplikace pomocí Azure Notification Hubs
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

Toto téma ukazuje, jak Azure Notification Hubs toosend toouse nabízená oznámení tooa aplikace pro Chrome, která se zobrazí v kontextu hello hello prohlížeč Google Chrome. V tomto kurzu vytvoříte aplikaci pro Chrome, která bude přijímat nabízená oznámení pomocí služby [GCM (Google Cloud Messaging)](https://developers.google.com/cloud-messaging/). 

> [!NOTE]
> toocomplete tento kurz, musíte mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F).
> 
> 

Hello kurz vás provede těmito základními kroky tooenable nabízených oznámení:

* [Povolení služby GCM (Google Cloud Messaging)](#register)
* [Konfigurace centra oznámení](#configure-hub)
* [Připojit vaše Centrum oznámení toohello aplikace pro Chrome](#connect-app)
* [Odesílání nabízených oznámení tooyour aplikace pro Chrome](#send)
* [Další funkce a schopnosti](#next-steps)

> [!NOTE]
> Nabízená oznámení v aplikaci Chrome nejsou obecná oznámení v prohlížeči – jsou konkrétní toohello model rozšiřitelnosti prohlížeče (viz [přehledu aplikací pro Chrome] podrobnosti). Kromě toho toohello desktopového prohlížeče běží aplikace pro Chrome na mobilních zařízeních (Android a iOS) prostřednictvím Apache Cordovy. V tématu [aplikace pro Chrome na mobilních zařízeních] toolearn Další.
> 
> 

Konfigurace služby GCM a Azure Notification Hubs je identické tooconfiguring pro Android, protože [Google Cloud Messaging pro Chrome] se už nepoužívá a hello tatáž služba GCM nyní podporuje jak zařízení se systémem Android tak instance chromu.

## <a id="register"></a>Povolení služby GCM (Google Cloud Messaging)
1. Přejděte toohello [Google Cloud Console] web, přihlaste se pomocí svých přihlašovacích údajů účtu Google a potom klikněte na hello **vytvoření projektu** tlačítko. Zadejte odpovídající **název projektu**a potom klikněte na hello **vytvořit** tlačítko.
   
       ![Google Cloud Console - Create Project][1]
2. Poznamenejte si hello **číslo projektu** na hello **projekty** stránku hello projektu, který jste právě vytvořili. To bude používat jako hello **ID odesílatele GCM** v tooregister hello aplikace pro Chrome s GCM.
   
       ![Google Cloud Console - Project Number][2]
3. V levém podokně hello, klikněte na **rozhraní API & auth**, posuňte se dolů a klikněte na tlačítko hello přepnutí tooenable **Google Cloud Messaging pro Android**. Nemáte tooenable **Google Cloud Messaging pro Chrome**.
   
       ![Google Cloud Console - Server Key][3]
4. V levém podokně hello, klikněte na **pověření** > **vytvořit nový klíč** > **klíč serveru** > **vytvořit**.
   
       ![Google Cloud Console - Credentials][4]
5. Poznamenejte si hello server **klíč rozhraní API**. Toto nakonfigurujete v tooenable další, vaše oznámení centra ho toosend nabízená oznámení tooGCM.
   
       ![Google Cloud Console - API Key][5]

## <a id="configure-hub"></a>Konfigurace centra oznámení
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

&emsp;&emsp;6.   V hello **nastavení** vyberte **služby oznámení** a potom **Google (GCM)**. Zadejte klíč rozhraní API hello a uložte.

&emsp;&emsp;![Azure Notification Hubs – Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

## <a id="connect-app"></a>Připojit vaše Centrum oznámení toohello aplikace pro Chrome
Vaše centrum oznámení je teď nakonfigurovaná toowork s GCM a máte hello připojovací řetězce tooregister vaší aplikace tooboth příjem a odesílání nabízených oznámení. LK

### <a name="create-a-new-chrome-app"></a>Vytvoření nové aplikace pro Chrome
Následující ukázka Hello je založena na hello [ukázce GCM aplikace pro Chrome] a hello používá doporučené způsob toocreate aplikace pro Chrome. Zvýrazníme hello kroky konkrétně související tooAzure centra oznámení. 

> [!NOTE]
> Doporučujeme, abyste si stáhli hello zdroje pro tato aplikace pro Chrome z [ukázky využití Notification aplikace Chrome].
> 
> 

Hello aplikace pro Chrome se vytváří prostřednictvím JavaScriptu a můžete použít některý z textový editor pro její vytvoření. Níže je znázorněno, jak tato aplikace pro Chrome bude vypadat.

![Aplikace pro Google Chrome][15]

1. Vytvořte složku a pojmenujte ji `ChromePushApp`. Název hello je samozřejmě libovolný – Pokud jste ji pojmenujete jinak, zkontrolujte, zda že nahraďte hello cestu hello požadované segmenty kódu.
2. Stáhnout hello [knihovnu crypto-js] ve složce hello jste vytvořili v druhém kroku hello. Tato složka knihovny bude obsahovat dvě podsložky: `components` a `rollups`.
3. Vytvořte soubor `manifest.json`. Všechny aplikace pro Chrome jsou zajišťované soubor manifestu, který obsahuje metadata aplikace hello a většina hlavně všechna oprávnění udělená aplikaci toohello při ji hello uživatel nainstaluje.
   
        {
          "name": "NH-GCM Notifications",
          "description": "Chrome platform app.",
          "manifest_version": 2,
          "version": "0.1",
          "app": {
            "background": {
              "scripts": ["background.js"]
            }
          },
          "permissions": ["gcm", "storage", "notifications", "https://*.servicebus.windows.net/*"],
          "icons": { "128": "gcm_128.png" }
        }
   
    Všimněte si hello `permissions` element, který určuje, že tato aplikace pro Chrome bude možné tooreceive nabízená oznámení ze služby GCM. Kromě toho musí určovat URI Azure Notification Hubs kde hello aplikace pro Chrome bude tooregister volání REST hello.
    Naše ukázková aplikace také používá soubor ikony `gcm_128.png`, že zjistíte u hello zdroje, který již byl použit hello původní ukázce GCM. Můžete jej nahradit libovolným obrázkem, která odpovídá hello [kritéria pro ikonu](https://developer.chrome.com/apps/manifest/icons).
4. Vytvořte soubor s názvem `background.js` s hello následující kód:
   
        // Returns a new notification ID used in hello notification.
        function getNotificationId() {
          var id = Math.floor(Math.random() * 9007199254740992) + 1;
          return id.toString();
        }
   
        function messageReceived(message) {
          // A message is an object with a data property that
          // consists of key-value pairs.
   
          // Concatenate all key-value pairs tooform a display string.
          var messageString = "";
          for (var key in message.data) {
            if (messageString != "")
              messageString += ", "
            messageString += key + ":" + message.data[key];
          }
          console.log("Message received: " + messageString);
   
          // Pop up a notification tooshow hello GCM message.
          chrome.notifications.create(getNotificationId(), {
            title: 'GCM Message',
            iconUrl: 'gcm_128.png',
            type: 'basic',
            message: messageString
          }, function() {});
        }
   
        var registerWindowCreated = false;
   
        function firstTimeRegistration() {
          chrome.storage.local.get("registered", function(result) {
   
            registerWindowCreated = true;
            chrome.app.window.create(
              "register.html",
              {  width: 520,
                 height: 500,
                 frame: 'chrome'
              },
              function(appWin) {}
            );
          });
        }
   
        // Set up a listener for GCM message event.
        chrome.gcm.onMessage.addListener(messageReceived);
   
        // Set up listeners tootrigger hello first-time registration.
        chrome.runtime.onInstalled.addListener(firstTimeRegistration);
        chrome.runtime.onStartup.addListener(firstTimeRegistration);
   
    Toto je soubor hello, které se automaticky otevře HTML okna aplikace pro Chrome hello (**register.html**) a také definuje obslužnou rutinu hello **messageReceived** toohandle hello příchozí nabízené oznámení.
5. Vytvořte soubor s názvem `register.html` -definuje hello uživatelské hello aplikace pro Chrome. 
   
   > [!NOTE]
   > V této ukázce se používá **CryptoJS v3.1.2**. Pokud jste si stáhli jinou verzi aplikace hello knihovně, ujistěte se, správně nahradit verzi hello v hello `src` cesta.
   > 
   > 
   
        <html>
   
        <head>
        <title>GCM Registration</title>
        <script src="register.js"></script>
        <script src="CryptoJS v3.1.2/rollups/hmac-sha256.js"></script>
        <script src="CryptoJS v3.1.2/components/enc-base64-min.js"></script>
        </head>
   
        <body>
   
        Sender ID:<br/><input id="senderId" type="TEXT" size="20"><br/>
        <button id="registerWithGCM">Register with GCM</button>
        <br/>
        <br/>
        <br/>
   
        Notification Hub Name:<br/><input id="hubName" type="TEXT" style="width:400px"><br/><br/>
        Connection String:<br/><textarea id="connectionString" type="TEXT" style="width:400px;height:60px"></textarea>
   
        <br/>
   
        <button id="registerWithNH" disabled="true">Register with Azure Notification Hubs</button>
   
        <br/>
        <br/>
   
        <textarea id="console" type="TEXT" readonly style="width:500px;height:200px;background-color:#e5e5e5;padding:5px"></textarea>
   
        </body>
   
        </html>
6. Vytvořte soubor s názvem `register.js` s hello kód níže. Tento soubor obsahuje skript hello za `register.html`. Aplikace pro Chrome neumožňují vložené spouštění, takže máte samostatný skript na pozadí pro toocreate pro uživatelské rozhraní.
   
        var registrationId = "";
        var hubName        = "", connectionString = "";
        var originalUri    = "", targetUri = "", endpoint = "", sasKeyName = "", sasKeyValue = "", sasToken = "";
   
        window.onload = function() {
           document.getElementById("registerWithGCM").onclick = registerWithGCM;  
           document.getElementById("registerWithNH").onclick = registerWithNH;
           updateLog("You have not registered yet. Please provider sender ID and register with GCM and then with  Notification Hubs.");
        }
   
        function updateLog(status) {
          currentStatus = document.getElementById("console").innerHTML;
          if (currentStatus != "") {
            currentStatus = currentStatus + "\n\n";
          }
   
          document.getElementById("console").innerHTML = currentStatus  + status;
        }
   
        function registerWithGCM() {
          var senderId = document.getElementById("senderId").value.trim();
          chrome.gcm.register([senderId], registerCallback);
   
          // Prevent register button from being clicked again before hello registration finishes.
          document.getElementById("registerWithGCM").disabled = true;
        }
   
        function registerCallback(regId) {
          registrationId = regId;
          document.getElementById("registerWithGCM").disabled = false;
   
          if (chrome.runtime.lastError) {
            // When hello registration fails, handle hello error and retry the
            // registration later.
            updateLog("Registration failed: " + chrome.runtime.lastError.message);
            return;
          }
   
          updateLog("Registration with GCM succeeded.");
          document.getElementById("registerWithNH").disabled = false;
   
          // Mark that hello first-time registration is done.
          chrome.storage.local.set({registered: true});
        }
   
        function registerWithNH() {
          hubName = document.getElementById("hubName").value.trim();
          connectionString = document.getElementById("connectionString").value.trim();
          splitConnectionString();
          generateSaSToken();
          sendNHRegistrationRequest();
        }
   
        // From http://msdn.microsoft.com/library/dn495627.aspx
        function splitConnectionString()
        {
          var parts = connectionString.split(';');
          if (parts.length != 3)
          throw "Error parsing connection string";
   
          parts.forEach(function(part) {
            if (part.indexOf('Endpoint') == 0) {
            endpoint = 'https' + part.substring(11);
            } else if (part.indexOf('SharedAccessKeyName') == 0) {
            sasKeyName = part.substring(20);
            } else if (part.indexOf('SharedAccessKey') == 0) {
            sasKeyValue = part.substring(16);
            }
          });
   
          originalUri = endpoint + hubName;
        }
   
        function generateSaSToken()
        {
          targetUri = encodeURIComponent(originalUri.toLowerCase()).toLowerCase();
          var expiresInMins = 10; // 10 minute expiration
   
          // Set expiration in seconds.
          var expireOnDate = new Date();
          expireOnDate.setMinutes(expireOnDate.getMinutes() + expiresInMins);
          var expires = Date.UTC(expireOnDate.getUTCFullYear(), expireOnDate
            .getUTCMonth(), expireOnDate.getUTCDate(), expireOnDate
            .getUTCHours(), expireOnDate.getUTCMinutes(), expireOnDate
            .getUTCSeconds()) / 1000;
          var tosign = targetUri + '\n' + expires;
   
          // Using CryptoJS.
          var signature = CryptoJS.HmacSHA256(tosign, sasKeyValue);
          var base64signature = signature.toString(CryptoJS.enc.Base64);
          var base64UriEncoded = encodeURIComponent(base64signature);
   
          // Construct authorization string.
          sasToken = "SharedAccessSignature sr=" + targetUri + "&sig="
                          + base64UriEncoded + "&se=" + expires + "&skn=" + sasKeyName;
        }
   
        function sendNHRegistrationRequest()
        {
          var registrationPayload =
          "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
          "<entry xmlns=\"http://www.w3.org/2005/Atom\">" +
              "<content type=\"application/xml\">" +
                  "<GcmRegistrationDescription xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/netservices/2010/10/servicebus/connect\">" +
                      "<GcmRegistrationId>{GCMRegistrationId}</GcmRegistrationId>" +
                  "</GcmRegistrationDescription>" +
              "</content>" +
          "</entry>";
   
          // Update hello payload with hello registration ID obtained earlier.
          registrationPayload = registrationPayload.replace("{GCMRegistrationId}", registrationId);
   
          var url = originalUri + "/registrations/?api-version=2014-09";
          var client = new XMLHttpRequest();
   
          client.onload = function () {
            if (client.readyState == 4) {
              if (client.status == 200) {
                updateLog("Notification Hub Registration succesful!");
                updateLog(client.responseText);
              } else {
                updateLog("Notification Hub Registration did not succeed!");
                updateLog("HTTP Status: " + client.status + " : " + client.statusText);
                updateLog("HTTP Response: " + "\n" + client.responseText);
              }
            }
          };
   
          client.onerror = function () {
                updateLog("ERROR - Notification Hub Registration did not succeed!");
          }
   
          client.open("POST", url, true);
          client.setRequestHeader("Content-Type", "application/atom+xml;type=entry;charset=utf-8");
          client.setRequestHeader("Authorization", sasToken);
          client.setRequestHeader("x-ms-version", "2014-09");
   
          try {
              client.send(registrationPayload);
          }
          catch(err) {
              updateLog(err.message);
          }
        }
   
    Hello výše skript má hello následující klíčové parametry:
   
   * **Window.OnLoad** definuje události kliknutí na tlačítko hello hello dva tlačítek na hello uživatelského rozhraní. Jeden registruje GCM a hello jiných používá hello ID registrace vrácené po registraci s GCM tooregister pomocí Azure Notification Hubs.
   * **updateLog** je hello funkce, která umožňuje nám toohandle jednoduché schopnosti protokolování.
   * **registerWithGCM** je hello první kliknutí na tlačítko obslužná rutina, která umožňuje hello `chrome.gcm.register` volání tooGCM tooregister hello aktuální instance aplikace pro Chrome.
   * **registerCallback** je hello funkce zpětného volání, která je volána, když se vrátí hello volání registrace služby GCM.
   * **registerWithNH** je hello druhý kliknutí na tlačítko obslužná rutina, která centra oznámení. Získá `hubName` a `connectionString` (které hello uživatele byla zadána) a řemeslníci hello Notification Hubs registrace volání rozhraní API REST.
   * **splitConnectionString** a **generateSaSToken** jsou pomocné rutiny, které představují Javascriptovou implementaci procesu vytvoření tokenu SaS, která bude použita ve všech voláních REST API hello. Další informace najdete v tématu [Běžné koncepty](http://msdn.microsoft.com/library/dn495627.aspx).
   * **sendNHRegistrationRequest** je hello funkce, která provádí HTTP REST volání tooAzure Notification Hubs.
   * **registrationPayload** definuje datovou část registrace XML hello. Další informace najdete v tématu o [vytvoření NH REST API registrace]. Aktualizujeme hello ID registrace v ní aktualizujeme údajem získaným ze služby GCM.
   * **Klient** je instance **XMLHttpRequest** že používáme požadavku HTTP POST toomake hello. Všimněte si, že jsme aktualizovat hello `Authorization` hlavička s `sasToken`. Pokud se toto volání dokončí úspěšně, bude instance aplikace pro Chrome zaregistrována do Azure Notification Hubs.

Hello celková struktura složek tohoto projektu by měl vypadat přibližně takto: ![aplikace pro Google Chrome – struktura složek][21]

### <a name="set-up-and-test-your-chrome-app"></a>Nastavení a testování aplikace pro Chrome
1. Otevřete prohlížeč Chrome. Otevřete **Rozšíření Chromu** a povolte **Režim pro vývojáře**.
   
       ![Google Chrome - Enable Developer Mode][16]
2. Klikněte na tlačítko **načíst rozbalené rozšíření** a přejděte toohello složku, kde jste vytvořili soubory hello. Volitelně také můžete použít hello **Chrome Apps & Extensions Developer Tool**. Tento nástroj je aplikace pro Chrome sám o sobě (nainstalovanou z internetového obchodu Chrome hello) a poskytuje pokročilé schopnosti ladění pro vývoj vaší aplikace pro Chrome.
   
       ![Google Chrome - Load Unpacked Extension][17]
3. Pokud je hello aplikace pro Chrome vytvoří bez chyb, uvidíte zobrazí aplikace pro Chrome.
   
       ![Google Chrome - Chrome App Display][18]
4. Zadejte hello **číslo projektu** jste předtím získali z hello **Google Cloud Console** jako ID odesílatele hello a klikněte na tlačítko **registraci s GCM**. Musí se zobrazit zpráva hello **registraci s GCM byla úspěšná.**
   
       ![Google Chrome - Chrome App Customization][19]
5. Zadejte vaše **název centra oznámení** a hello **DefaultListenSharedAccessSignature** jste získali z portálu hello dříve a klikněte na **zaregistrovat do Azure Notification Hubs**. Musí se zobrazit zpráva hello **registrace do Notification Hubs úspěšné!** a ID hello podrobnosti o odpovědi registrace hello, která obsahuje hello registrace Azure Notification Hubs.
   
       ![Google Chrome - Specify Notification Hub Details][20]  

## <a name="send"></a>Odeslat oznámení tooyour aplikace pro Chrome
Pro účely testování pošleme nabízené oznámení Chrome pomocí konzolové aplikace .NET. 

> [!NOTE]
> Pomocí našeho veřejného <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">rozhraní REST</a> je možné pomocí Notification Hubs posílat nabízená oznámení z jakéhokoli back-endu. Další ukázky pro více platforem najdete na našem [dokumentačním portálu](https://azure.microsoft.com/documentation/services/notification-hubs/).
> 
> 

1. V sadě Visual Studio, z hello **soubor** nabídce vyberte možnost **nový** a potom **projektu**. V části **Visual C#** klikněte na **Windows** a **Konzolová aplikace** a pak na **OK**.  Vytvoří se nový projekt konzolové aplikace.
2. Z hello **nástroje** nabídky, klikněte na tlačítko **Správce balíčků knihoven** a potom **Konzola správce balíčků**. Zobrazí se hello Konzola správce balíčků.
3. V okně konzoly hello spusťte následující příkaz hello:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
       This adds a reference toohello Azure Service Bus SDK with hello <a href="http://nuget.org/packages/  WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet package</a>.
4. Otevřete `Program.cs` a přidejte následující hello `using` příkaz:
   
        using Microsoft.Azure.NotificationHubs;
5. V hello `Program` třídy, přidejte následující metodu hello:
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
            await hub.SendGcmNativeNotificationAsync(message);
        }
   
       Make sure tooreplace hello `<hub name>` placeholder with hello name of hello notification hub that appears in hello [portal](https://portal.azure.com) in your Notification Hub blade. Also, replace hello connection string placeholder with hello connection string called `DefaultFullSharedAccessSignature` that you obtained in hello notification hub configuration section.
   
   > [!NOTE]
   > Ujistěte se, že používáte připojovací řetězec hello s **úplné** přístupem, nikoli **naslouchání** přístup. Hello **naslouchání** připojovací řetězec s přístupem nejsou udělena oprávnění toosend nabízená oznámení.
   > 
   > 
6. Přidejte následující hello volá v hello `Main` metoda:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Ujistěte se, že je spuštěn Chrome a spusťte konzolovou aplikaci hello.
8. Měli byste vidět následující hello oznámení otevíraném na ploše.
   
       ![Google Chrome - Notification][13]
9. Můžete také zobrazit všechna oznámení pomocí okna oznámení Chrome hello panelu hello (v systému Windows) když je Chrome spuštěn.
   
       ![Google Chrome - Notifications List][14]

> [!NOTE]
> Toohave hello aplikace pro Chrome není nutné spuštěné nebo otevřít v prohlížeči hello (i když hello prohlížeč samotný běžet musí). V okně oznámení pro Chrome hello také získáte celkový pohled na všechna oznámení.
> 
> 

## <a name="next-steps"></a>Další kroky
Další informace o Notification Hubs najdete v [přehledu této služby].

tootarget konkrétní uživatele, najdete v toohello [Azure upozornění uživatelů centra oznámení] kurzu. 

Pokud chcete toosegment uživatele podle zájmových skupin, můžete postupovat podle hello [nejnovější zprávy přes Azure Notification Hubs] kurzu.

<!-- Images. -->
[1]: ./media/notification-hubs-chrome-get-started/GoogleConsoleCreateProject.PNG
[2]: ./media/notification-hubs-chrome-get-started/GoogleProjectNumber.png
[3]: ./media/notification-hubs-chrome-get-started/EnableGCM.png
[4]: ./media/notification-hubs-chrome-get-started/CreateServerKey.png
[5]: ./media/notification-hubs-chrome-get-started/ServerKey.png
[6]: ./media/notification-hubs-chrome-get-started/CreateNH.png
[7]: ./media/notification-hubs-chrome-get-started/NHNamespace.png
[8]: ./media/notification-hubs-chrome-get-started/NamespaceConfigure.png
[9]: ./media/notification-hubs-chrome-get-started/NHConfigure.png
[10]: ./media/notification-hubs-chrome-get-started/NHConfigureGCM.png
[11]: ./media/notification-hubs-chrome-get-started/NHDashboard.png
[12]: ./media/notification-hubs-chrome-get-started/NHConnString.png
[13]: ./media/notification-hubs-chrome-get-started/ChromeNotification.png
[14]: ./media/notification-hubs-chrome-get-started/ChromeNotificationWindow.png
[15]: ./media/notification-hubs-chrome-get-started/ChromeApp.png
[16]: ./media/notification-hubs-chrome-get-started/ChromeExtensions.png
[17]: ./media/notification-hubs-chrome-get-started/ChromeLoadExtension.png
[18]: ./media/notification-hubs-chrome-get-started/ChromeAppLoaded.png
[19]: ./media/notification-hubs-chrome-get-started/ChromeAppGCM.png
[20]: ./media/notification-hubs-chrome-get-started/ChromeAppNH.png
[21]: ./media/notification-hubs-chrome-get-started/FinalFolderView.png

<!-- URLs. -->
[ukázky využití Notification aplikace Chrome]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToChromeApps
[Google Cloud Console]: http://cloud.google.com/console
[Azure Classic Portal]: https://manage.windowsazure.com/
[přehledu této služby]: notification-hubs-push-notification-overview.md
[přehledu aplikací pro Chrome]: https://developer.chrome.com/apps/about_apps
[ukázce GCM aplikace pro Chrome]: https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples/gcm-notifications
[Installable Web Apps]: https://developers.google.com/chrome/apps/docs/
[aplikace pro Chrome na mobilních zařízeních]: https://developer.chrome.com/apps/chrome_apps_on_mobile
[vytvoření NH REST API registrace]: http://msdn.microsoft.com/library/azure/dn223265.aspx
[knihovnu crypto-js]: http://code.google.com/p/crypto-js/
[GCM with Chrome Apps]: https://developer.chrome.com/apps/cloudMessaging
[Google Cloud Messaging pro Chrome]: https://developer.chrome.com/apps/cloudMessagingV1
[Azure upozornění uživatelů centra oznámení]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[nejnovější zprávy přes Azure Notification Hubs]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
