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
# <a name="send-push-notifications-toochrome-apps-with-azure-notification-hubs"></a><span data-ttu-id="26e91-104">Odesílání nabízených oznámení tooChrome aplikace pomocí Azure Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="26e91-104">Send push notifications tooChrome apps with Azure Notification Hubs</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

<span data-ttu-id="26e91-105">Toto téma ukazuje, jak Azure Notification Hubs toosend toouse nabízená oznámení tooa aplikace pro Chrome, která se zobrazí v kontextu hello hello prohlížeč Google Chrome.</span><span class="sxs-lookup"><span data-stu-id="26e91-105">This topic shows you how toouse Azure Notification Hubs toosend push notifications tooa Chrome App, which will be displayed within hello context of hello Google Chrome browser.</span></span> <span data-ttu-id="26e91-106">V tomto kurzu vytvoříte aplikaci pro Chrome, která bude přijímat nabízená oznámení pomocí služby [GCM (Google Cloud Messaging)](https://developers.google.com/cloud-messaging/).</span><span class="sxs-lookup"><span data-stu-id="26e91-106">In this tutorial, we will create a Chrome app that receives push notifications by using [Google Cloud Messaging (GCM)](https://developers.google.com/cloud-messaging/).</span></span> 

> [!NOTE]
> <span data-ttu-id="26e91-107">toocomplete tento kurz, musíte mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="26e91-107">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="26e91-108">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="26e91-108">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="26e91-109">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F).</span><span class="sxs-lookup"><span data-stu-id="26e91-109">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F).</span></span>
> 
> 

<span data-ttu-id="26e91-110">Hello kurz vás provede těmito základními kroky tooenable nabízených oznámení:</span><span class="sxs-lookup"><span data-stu-id="26e91-110">hello tutorial walks you through these basic steps tooenable push notifications:</span></span>

* [<span data-ttu-id="26e91-111">Povolení služby GCM (Google Cloud Messaging)</span><span class="sxs-lookup"><span data-stu-id="26e91-111">Enable Google Cloud Messaging</span></span>](#register)
* [<span data-ttu-id="26e91-112">Konfigurace centra oznámení</span><span class="sxs-lookup"><span data-stu-id="26e91-112">Configure your notification hub</span></span>](#configure-hub)
* [<span data-ttu-id="26e91-113">Připojit vaše Centrum oznámení toohello aplikace pro Chrome</span><span class="sxs-lookup"><span data-stu-id="26e91-113">Connect your Chrome App toohello notification hub</span></span>](#connect-app)
* [<span data-ttu-id="26e91-114">Odesílání nabízených oznámení tooyour aplikace pro Chrome</span><span class="sxs-lookup"><span data-stu-id="26e91-114">Send a push notification tooyour Chrome App</span></span>](#send)
* [<span data-ttu-id="26e91-115">Další funkce a schopnosti</span><span class="sxs-lookup"><span data-stu-id="26e91-115">Additional functionality & capabilities</span></span>](#next-steps)

> [!NOTE]
> <span data-ttu-id="26e91-116">Nabízená oznámení v aplikaci Chrome nejsou obecná oznámení v prohlížeči – jsou konkrétní toohello model rozšiřitelnosti prohlížeče (viz [přehledu aplikací pro Chrome] podrobnosti).</span><span class="sxs-lookup"><span data-stu-id="26e91-116">Chrome app push notifications are not generic in-browser notifications - they are specific toohello browser extensibility model (see [Chrome Apps Overview] for details).</span></span> <span data-ttu-id="26e91-117">Kromě toho toohello desktopového prohlížeče běží aplikace pro Chrome na mobilních zařízeních (Android a iOS) prostřednictvím Apache Cordovy.</span><span class="sxs-lookup"><span data-stu-id="26e91-117">In addition toohello desktop browser, Chrome apps run on mobile (Android and iOS) through Apache Cordova.</span></span> <span data-ttu-id="26e91-118">V tématu [aplikace pro Chrome na mobilních zařízeních] toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="26e91-118">See [Chrome Apps on Mobile] toolearn more.</span></span>
> 
> 

<span data-ttu-id="26e91-119">Konfigurace služby GCM a Azure Notification Hubs je identické tooconfiguring pro Android, protože [Google Cloud Messaging pro Chrome] se už nepoužívá a hello tatáž služba GCM nyní podporuje jak zařízení se systémem Android tak instance chromu.</span><span class="sxs-lookup"><span data-stu-id="26e91-119">Configuring GCM and Azure Notification Hubs is identical tooconfiguring for Android, since [Google Cloud Messaging for Chrome] has been deprecated and hello same GCM now supports both Android devices and Chrome instances.</span></span>

## <span data-ttu-id="26e91-120"><a id="register"></a>Povolení služby GCM (Google Cloud Messaging)</span><span class="sxs-lookup"><span data-stu-id="26e91-120"><a id="register"></a>Enable Google Cloud Messaging</span></span>
1. <span data-ttu-id="26e91-121">Přejděte toohello [Google Cloud Console] web, přihlaste se pomocí svých přihlašovacích údajů účtu Google a potom klikněte na hello **vytvoření projektu** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="26e91-121">Navigate toohello [Google Cloud Console] website, sign in with your Google account credentials, and then click hello **Create Project** button.</span></span> <span data-ttu-id="26e91-122">Zadejte odpovídající **název projektu**a potom klikněte na hello **vytvořit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="26e91-122">Provide an appropriate **Project Name**, and then click hello **Create** button.</span></span>
   
       ![Google Cloud Console - Create Project][1]
2. <span data-ttu-id="26e91-123">Poznamenejte si hello **číslo projektu** na hello **projekty** stránku hello projektu, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="26e91-123">Make a note of hello **Project Number** on hello **Projects** page for hello project that you just created.</span></span> <span data-ttu-id="26e91-124">To bude používat jako hello **ID odesílatele GCM** v tooregister hello aplikace pro Chrome s GCM.</span><span class="sxs-lookup"><span data-stu-id="26e91-124">You will use this as hello **GCM Sender ID** in hello Chrome App tooregister with GCM.</span></span>
   
       ![Google Cloud Console - Project Number][2]
3. <span data-ttu-id="26e91-125">V levém podokně hello, klikněte na **rozhraní API & auth**, posuňte se dolů a klikněte na tlačítko hello přepnutí tooenable **Google Cloud Messaging pro Android**.</span><span class="sxs-lookup"><span data-stu-id="26e91-125">In hello left pane, click **APIs & auth**, and then scroll down and click hello toggle tooenable **Google Cloud Messaging for Android**.</span></span> <span data-ttu-id="26e91-126">Nemáte tooenable **Google Cloud Messaging pro Chrome**.</span><span class="sxs-lookup"><span data-stu-id="26e91-126">You don't have tooenable **Google Cloud Messaging for Chrome**.</span></span>
   
       ![Google Cloud Console - Server Key][3]
4. <span data-ttu-id="26e91-127">V levém podokně hello, klikněte na **pověření** > **vytvořit nový klíč** > **klíč serveru** > **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="26e91-127">In hello left pane, click **Credentials** > **Create New Key** > **Server Key** > **Create**.</span></span>
   
       ![Google Cloud Console - Credentials][4]
5. <span data-ttu-id="26e91-128">Poznamenejte si hello server **klíč rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="26e91-128">Make a note of hello server **API Key**.</span></span> <span data-ttu-id="26e91-129">Toto nakonfigurujete v tooenable další, vaše oznámení centra ho toosend nabízená oznámení tooGCM.</span><span class="sxs-lookup"><span data-stu-id="26e91-129">You will configure this in your notification hub next, tooenable it toosend push notifications tooGCM.</span></span>
   
       ![Google Cloud Console - API Key][5]

## <span data-ttu-id="26e91-130"><a id="configure-hub"></a>Konfigurace centra oznámení</span><span class="sxs-lookup"><span data-stu-id="26e91-130"><a id="configure-hub"></a>Configure your notification hub</span></span>
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<span data-ttu-id="26e91-131">&emsp;&emsp;6.</span><span class="sxs-lookup"><span data-stu-id="26e91-131">&emsp;&emsp;6.</span></span>   <span data-ttu-id="26e91-132">V hello **nastavení** vyberte **služby oznámení** a potom **Google (GCM)**.</span><span class="sxs-lookup"><span data-stu-id="26e91-132">In hello **Settings** blade, select **Notification Services** and then **Google (GCM)**.</span></span> <span data-ttu-id="26e91-133">Zadejte klíč rozhraní API hello a uložte.</span><span class="sxs-lookup"><span data-stu-id="26e91-133">Enter hello API key and save.</span></span>

&emsp;&emsp;![Azure Notification Hubs – Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

## <span data-ttu-id="26e91-135"><a id="connect-app"></a>Připojit vaše Centrum oznámení toohello aplikace pro Chrome</span><span class="sxs-lookup"><span data-stu-id="26e91-135"><a id="connect-app"></a>Connect your Chrome App toohello notification hub</span></span>
<span data-ttu-id="26e91-136">Vaše centrum oznámení je teď nakonfigurovaná toowork s GCM a máte hello připojovací řetězce tooregister vaší aplikace tooboth příjem a odesílání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="26e91-136">Your notification hub is now configured toowork with GCM, and you have hello connection strings tooregister your app tooboth receive and send push notifications.</span></span> <span data-ttu-id="26e91-137">LK</span><span class="sxs-lookup"><span data-stu-id="26e91-137">LK</span></span>

### <a name="create-a-new-chrome-app"></a><span data-ttu-id="26e91-138">Vytvoření nové aplikace pro Chrome</span><span class="sxs-lookup"><span data-stu-id="26e91-138">Create a new Chrome App</span></span>
<span data-ttu-id="26e91-139">Následující ukázka Hello je založena na hello [ukázce GCM aplikace pro Chrome] a hello používá doporučené způsob toocreate aplikace pro Chrome.</span><span class="sxs-lookup"><span data-stu-id="26e91-139">hello sample below is based on hello [Chrome App GCM Sample] and uses hello recommended way toocreate a Chrome App.</span></span> <span data-ttu-id="26e91-140">Zvýrazníme hello kroky konkrétně související tooAzure centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="26e91-140">We will highlight hello steps specifically related tooAzure Notification Hubs.</span></span> 

> [!NOTE]
> <span data-ttu-id="26e91-141">Doporučujeme, abyste si stáhli hello zdroje pro tato aplikace pro Chrome z [ukázky využití Notification aplikace Chrome].</span><span class="sxs-lookup"><span data-stu-id="26e91-141">We recommend that you download hello source for this Chrome App from [Chrome App Notification Hub Sample].</span></span>
> 
> 

<span data-ttu-id="26e91-142">Hello aplikace pro Chrome se vytváří prostřednictvím JavaScriptu a můžete použít některý z textový editor pro její vytvoření.</span><span class="sxs-lookup"><span data-stu-id="26e91-142">hello Chrome App is created via JavaScript, and you can use any of your preferred word editors for creating it.</span></span> <span data-ttu-id="26e91-143">Níže je znázorněno, jak tato aplikace pro Chrome bude vypadat.</span><span class="sxs-lookup"><span data-stu-id="26e91-143">Below is what this Chrome App will look like.</span></span>

![Aplikace pro Google Chrome][15]

1. <span data-ttu-id="26e91-145">Vytvořte složku a pojmenujte ji `ChromePushApp`.</span><span class="sxs-lookup"><span data-stu-id="26e91-145">Create a folder and name it `ChromePushApp`.</span></span> <span data-ttu-id="26e91-146">Název hello je samozřejmě libovolný – Pokud jste ji pojmenujete jinak, zkontrolujte, zda že nahraďte hello cestu hello požadované segmenty kódu.</span><span class="sxs-lookup"><span data-stu-id="26e91-146">Of course, hello name is arbitrary - if you name it something different, make sure you substitute hello path in hello required code segments.</span></span>
2. <span data-ttu-id="26e91-147">Stáhnout hello [knihovnu crypto-js] ve složce hello jste vytvořili v druhém kroku hello.</span><span class="sxs-lookup"><span data-stu-id="26e91-147">Download hello [crypto-js library] in hello folder you created in hello second step.</span></span> <span data-ttu-id="26e91-148">Tato složka knihovny bude obsahovat dvě podsložky: `components` a `rollups`.</span><span class="sxs-lookup"><span data-stu-id="26e91-148">This library folder will contain two subfolders: `components` and `rollups`.</span></span>
3. <span data-ttu-id="26e91-149">Vytvořte soubor `manifest.json`.</span><span class="sxs-lookup"><span data-stu-id="26e91-149">Create a `manifest.json` file.</span></span> <span data-ttu-id="26e91-150">Všechny aplikace pro Chrome jsou zajišťované soubor manifestu, který obsahuje metadata aplikace hello a většina hlavně všechna oprávnění udělená aplikaci toohello při ji hello uživatel nainstaluje.</span><span class="sxs-lookup"><span data-stu-id="26e91-150">All Chrome Apps are backed by a manifest file that contains hello app metadata and, most importantly, all permissions that are granted toohello app when hello user installs it.</span></span>
   
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
   
    <span data-ttu-id="26e91-151">Všimněte si hello `permissions` element, který určuje, že tato aplikace pro Chrome bude možné tooreceive nabízená oznámení ze služby GCM.</span><span class="sxs-lookup"><span data-stu-id="26e91-151">Notice hello `permissions` element, which specifies that this Chrome App will be able tooreceive push notifications from GCM.</span></span> <span data-ttu-id="26e91-152">Kromě toho musí určovat URI Azure Notification Hubs kde hello aplikace pro Chrome bude tooregister volání REST hello.</span><span class="sxs-lookup"><span data-stu-id="26e91-152">It must also specify hello Azure Notification Hubs URI where hello Chrome App will make a REST call tooregister.</span></span>
    <span data-ttu-id="26e91-153">Naše ukázková aplikace také používá soubor ikony `gcm_128.png`, že zjistíte u hello zdroje, který již byl použit hello původní ukázce GCM.</span><span class="sxs-lookup"><span data-stu-id="26e91-153">Our sample app also uses an icon file, `gcm_128.png`, that you will find at hello source that's reused from hello original GCM sample.</span></span> <span data-ttu-id="26e91-154">Můžete jej nahradit libovolným obrázkem, která odpovídá hello [kritéria pro ikonu](https://developer.chrome.com/apps/manifest/icons).</span><span class="sxs-lookup"><span data-stu-id="26e91-154">You can substitute it for any image that fits hello [icon criteria](https://developer.chrome.com/apps/manifest/icons).</span></span>
4. <span data-ttu-id="26e91-155">Vytvořte soubor s názvem `background.js` s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="26e91-155">Create a file called `background.js` with hello following code:</span></span>
   
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
   
    <span data-ttu-id="26e91-156">Toto je soubor hello, které se automaticky otevře HTML okna aplikace pro Chrome hello (**register.html**) a také definuje obslužnou rutinu hello **messageReceived** toohandle hello příchozí nabízené oznámení.</span><span class="sxs-lookup"><span data-stu-id="26e91-156">This is hello file that pops up hello Chrome App window HTML (**register.html**) and also defines hello handler **messageReceived** toohandle hello incoming push notification.</span></span>
5. <span data-ttu-id="26e91-157">Vytvořte soubor s názvem `register.html` -definuje hello uživatelské hello aplikace pro Chrome.</span><span class="sxs-lookup"><span data-stu-id="26e91-157">Create a file called `register.html` - this defines hello UI of hello Chrome App.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="26e91-158">V této ukázce se používá **CryptoJS v3.1.2**.</span><span class="sxs-lookup"><span data-stu-id="26e91-158">This sample uses **CryptoJS v3.1.2**.</span></span> <span data-ttu-id="26e91-159">Pokud jste si stáhli jinou verzi aplikace hello knihovně, ujistěte se, správně nahradit verzi hello v hello `src` cesta.</span><span class="sxs-lookup"><span data-stu-id="26e91-159">If you downloaded another version of hello library, make sure you properly substitute hello version in hello `src` path.</span></span>
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
6. <span data-ttu-id="26e91-160">Vytvořte soubor s názvem `register.js` s hello kód níže.</span><span class="sxs-lookup"><span data-stu-id="26e91-160">Create a file called `register.js` with hello code below.</span></span> <span data-ttu-id="26e91-161">Tento soubor obsahuje skript hello za `register.html`.</span><span class="sxs-lookup"><span data-stu-id="26e91-161">This file specifies hello script behind `register.html`.</span></span> <span data-ttu-id="26e91-162">Aplikace pro Chrome neumožňují vložené spouštění, takže máte samostatný skript na pozadí pro toocreate pro uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="26e91-162">Chrome Apps do not allow inline execution, so you have toocreate a separate backing script for your UI.</span></span>
   
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
   
    <span data-ttu-id="26e91-163">Hello výše skript má hello následující klíčové parametry:</span><span class="sxs-lookup"><span data-stu-id="26e91-163">hello above script has hello following key parameters:</span></span>
   
   * <span data-ttu-id="26e91-164">**Window.OnLoad** definuje události kliknutí na tlačítko hello hello dva tlačítek na hello uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="26e91-164">**window.onload** defines hello button-click events of hello two buttons on hello UI.</span></span> <span data-ttu-id="26e91-165">Jeden registruje GCM a hello jiných používá hello ID registrace vrácené po registraci s GCM tooregister pomocí Azure Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="26e91-165">One registers with GCM, and hello other uses hello registration ID that's returned after registration with GCM tooregister with Azure Notification Hubs.</span></span>
   * <span data-ttu-id="26e91-166">**updateLog** je hello funkce, která umožňuje nám toohandle jednoduché schopnosti protokolování.</span><span class="sxs-lookup"><span data-stu-id="26e91-166">**updateLog** is hello function that allows us toohandle simple logging capabilities.</span></span>
   * <span data-ttu-id="26e91-167">**registerWithGCM** je hello první kliknutí na tlačítko obslužná rutina, která umožňuje hello `chrome.gcm.register` volání tooGCM tooregister hello aktuální instance aplikace pro Chrome.</span><span class="sxs-lookup"><span data-stu-id="26e91-167">**registerWithGCM** is hello first button-click handler, which makes hello `chrome.gcm.register` call tooGCM tooregister hello current Chrome App instance.</span></span>
   * <span data-ttu-id="26e91-168">**registerCallback** je hello funkce zpětného volání, která je volána, když se vrátí hello volání registrace služby GCM.</span><span class="sxs-lookup"><span data-stu-id="26e91-168">**registerCallback** is hello callback function that gets called when hello GCM registration call returns.</span></span>
   * <span data-ttu-id="26e91-169">**registerWithNH** je hello druhý kliknutí na tlačítko obslužná rutina, která centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="26e91-169">**registerWithNH** is hello second button-click handler, which registers with Notification Hubs.</span></span> <span data-ttu-id="26e91-170">Získá `hubName` a `connectionString` (které hello uživatele byla zadána) a řemeslníci hello Notification Hubs registrace volání rozhraní API REST.</span><span class="sxs-lookup"><span data-stu-id="26e91-170">It gets `hubName` and `connectionString` (which hello user has specified) and crafts hello Notification Hubs Registration REST API call.</span></span>
   * <span data-ttu-id="26e91-171">**splitConnectionString** a **generateSaSToken** jsou pomocné rutiny, které představují Javascriptovou implementaci procesu vytvoření tokenu SaS, která bude použita ve všech voláních REST API hello.</span><span class="sxs-lookup"><span data-stu-id="26e91-171">**splitConnectionString** and **generateSaSToken** are helpers that represent hello JavaScript implementation of a SaS token creation process, that must be used in all REST API calls.</span></span> <span data-ttu-id="26e91-172">Další informace najdete v tématu [Běžné koncepty](http://msdn.microsoft.com/library/dn495627.aspx).</span><span class="sxs-lookup"><span data-stu-id="26e91-172">For more information, see [Common Concepts](http://msdn.microsoft.com/library/dn495627.aspx).</span></span>
   * <span data-ttu-id="26e91-173">**sendNHRegistrationRequest** je hello funkce, která provádí HTTP REST volání tooAzure Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="26e91-173">**sendNHRegistrationRequest** is hello function that makes a HTTP REST call tooAzure Notification Hubs.</span></span>
   * <span data-ttu-id="26e91-174">**registrationPayload** definuje datovou část registrace XML hello.</span><span class="sxs-lookup"><span data-stu-id="26e91-174">**registrationPayload** defines hello registration XML payload.</span></span> <span data-ttu-id="26e91-175">Další informace najdete v tématu o [vytvoření NH REST API registrace].</span><span class="sxs-lookup"><span data-stu-id="26e91-175">For more information, see [Create Registration NH REST API].</span></span> <span data-ttu-id="26e91-176">Aktualizujeme hello ID registrace v ní aktualizujeme údajem získaným ze služby GCM.</span><span class="sxs-lookup"><span data-stu-id="26e91-176">We update hello registration ID in it with what we received from GCM.</span></span>
   * <span data-ttu-id="26e91-177">**Klient** je instance **XMLHttpRequest** že používáme požadavku HTTP POST toomake hello.</span><span class="sxs-lookup"><span data-stu-id="26e91-177">**client** is an instance of **XMLHttpRequest** that we use toomake hello HTTP POST request.</span></span> <span data-ttu-id="26e91-178">Všimněte si, že jsme aktualizovat hello `Authorization` hlavička s `sasToken`.</span><span class="sxs-lookup"><span data-stu-id="26e91-178">Note that we update hello `Authorization` header with `sasToken`.</span></span> <span data-ttu-id="26e91-179">Pokud se toto volání dokončí úspěšně, bude instance aplikace pro Chrome zaregistrována do Azure Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="26e91-179">Successful completion of this call will register this Chrome App instance with Azure Notification Hubs.</span></span>

<span data-ttu-id="26e91-180">Hello celková struktura složek tohoto projektu by měl vypadat přibližně takto: ![aplikace pro Google Chrome – struktura složek][21]</span><span class="sxs-lookup"><span data-stu-id="26e91-180">hello overall folder structure for this project should resemble this: ![Google Chrome App - Folder Structure][21]</span></span>

### <a name="set-up-and-test-your-chrome-app"></a><span data-ttu-id="26e91-181">Nastavení a testování aplikace pro Chrome</span><span class="sxs-lookup"><span data-stu-id="26e91-181">Set up and test your Chrome App</span></span>
1. <span data-ttu-id="26e91-182">Otevřete prohlížeč Chrome.</span><span class="sxs-lookup"><span data-stu-id="26e91-182">Open your Chrome browser.</span></span> <span data-ttu-id="26e91-183">Otevřete **Rozšíření Chromu** a povolte **Režim pro vývojáře**.</span><span class="sxs-lookup"><span data-stu-id="26e91-183">Open **Chrome extensions** and enable **Developer mode**.</span></span>
   
       ![Google Chrome - Enable Developer Mode][16]
2. <span data-ttu-id="26e91-184">Klikněte na tlačítko **načíst rozbalené rozšíření** a přejděte toohello složku, kde jste vytvořili soubory hello.</span><span class="sxs-lookup"><span data-stu-id="26e91-184">Click **Load unpacked extension** and navigate toohello folder where you created hello files.</span></span> <span data-ttu-id="26e91-185">Volitelně také můžete použít hello **Chrome Apps & Extensions Developer Tool**.</span><span class="sxs-lookup"><span data-stu-id="26e91-185">You can also optionally use hello **Chrome Apps & Extensions Developer Tool**.</span></span> <span data-ttu-id="26e91-186">Tento nástroj je aplikace pro Chrome sám o sobě (nainstalovanou z internetového obchodu Chrome hello) a poskytuje pokročilé schopnosti ladění pro vývoj vaší aplikace pro Chrome.</span><span class="sxs-lookup"><span data-stu-id="26e91-186">This tool is a Chrome App in itself (installed from hello Chrome Web Store) and provides advanced debugging capabilities for your Chrome App development.</span></span>
   
       ![Google Chrome - Load Unpacked Extension][17]
3. <span data-ttu-id="26e91-187">Pokud je hello aplikace pro Chrome vytvoří bez chyb, uvidíte zobrazí aplikace pro Chrome.</span><span class="sxs-lookup"><span data-stu-id="26e91-187">If hello Chrome App is created without any errors, then you will see your Chrome App show up.</span></span>
   
       ![Google Chrome - Chrome App Display][18]
4. <span data-ttu-id="26e91-188">Zadejte hello **číslo projektu** jste předtím získali z hello **Google Cloud Console** jako ID odesílatele hello a klikněte na tlačítko **registraci s GCM**.</span><span class="sxs-lookup"><span data-stu-id="26e91-188">Enter hello **Project Number** that you got earlier from hello **Google Cloud Console** as hello sender ID, and click **Register with GCM**.</span></span> <span data-ttu-id="26e91-189">Musí se zobrazit zpráva hello **registraci s GCM byla úspěšná.**</span><span class="sxs-lookup"><span data-stu-id="26e91-189">You must see hello message **Registration with GCM succeeded.**</span></span>
   
       ![Google Chrome - Chrome App Customization][19]
5. <span data-ttu-id="26e91-190">Zadejte vaše **název centra oznámení** a hello **DefaultListenSharedAccessSignature** jste získali z portálu hello dříve a klikněte na **zaregistrovat do Azure Notification Hubs**.</span><span class="sxs-lookup"><span data-stu-id="26e91-190">Enter your **Notification Hub Name** and hello **DefaultListenSharedAccessSignature** that you obtained from hello portal earlier, and click **Register with Azure Notification Hub**.</span></span> <span data-ttu-id="26e91-191">Musí se zobrazit zpráva hello **registrace do Notification Hubs úspěšné!**</span><span class="sxs-lookup"><span data-stu-id="26e91-191">You must see hello message **Notification Hub Registration successful!**</span></span> <span data-ttu-id="26e91-192">a ID hello podrobnosti o odpovědi registrace hello, která obsahuje hello registrace Azure Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="26e91-192">and hello details of hello registration response, which contains hello Azure Notification Hubs registration ID.</span></span>
   
       ![Google Chrome - Specify Notification Hub Details][20]  

## <span data-ttu-id="26e91-193"><a name="send"></a>Odeslat oznámení tooyour aplikace pro Chrome</span><span class="sxs-lookup"><span data-stu-id="26e91-193"><a name="send"></a>Send a notification tooyour Chrome App</span></span>
<span data-ttu-id="26e91-194">Pro účely testování pošleme nabízené oznámení Chrome pomocí konzolové aplikace .NET.</span><span class="sxs-lookup"><span data-stu-id="26e91-194">For testing purposes, we will send Chrome push notifications by using a .NET console application.</span></span> 

> [!NOTE]
> <span data-ttu-id="26e91-195">Pomocí našeho veřejného <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">rozhraní REST</a> je možné pomocí Notification Hubs posílat nabízená oznámení z jakéhokoli back-endu.</span><span class="sxs-lookup"><span data-stu-id="26e91-195">You can send push notifications with Notification Hubs from any backend via our public <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST interface</a>.</span></span> <span data-ttu-id="26e91-196">Další ukázky pro více platforem najdete na našem [dokumentačním portálu](https://azure.microsoft.com/documentation/services/notification-hubs/).</span><span class="sxs-lookup"><span data-stu-id="26e91-196">Check out our [documentation portal](https://azure.microsoft.com/documentation/services/notification-hubs/) for more cross-platform examples.</span></span>
> 
> 

1. <span data-ttu-id="26e91-197">V sadě Visual Studio, z hello **soubor** nabídce vyberte možnost **nový** a potom **projektu**.</span><span class="sxs-lookup"><span data-stu-id="26e91-197">In Visual Studio, from hello **File** menu, select **New** and then **Project**.</span></span> <span data-ttu-id="26e91-198">V části **Visual C#** klikněte na **Windows** a **Konzolová aplikace** a pak na **OK**.</span><span class="sxs-lookup"><span data-stu-id="26e91-198">Under **Visual C#**, click **Windows** and **Console Application**, and then click **OK**.</span></span>  <span data-ttu-id="26e91-199">Vytvoří se nový projekt konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="26e91-199">This creates a new console application project.</span></span>
2. <span data-ttu-id="26e91-200">Z hello **nástroje** nabídky, klikněte na tlačítko **Správce balíčků knihoven** a potom **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="26e91-200">From hello **Tools** menu, click **Library Package Manager** and then **Package Manager Console**.</span></span> <span data-ttu-id="26e91-201">Zobrazí se hello Konzola správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="26e91-201">This displays hello Package Manager Console.</span></span>
3. <span data-ttu-id="26e91-202">V okně konzoly hello spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="26e91-202">In hello console window, execute hello following command:</span></span>
   
        Install-Package Microsoft.Azure.NotificationHubs
   
       This adds a reference toohello Azure Service Bus SDK with hello <a href="http://nuget.org/packages/  WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet package</a>.
4. <span data-ttu-id="26e91-203">Otevřete `Program.cs` a přidejte následující hello `using` příkaz:</span><span class="sxs-lookup"><span data-stu-id="26e91-203">Open `Program.cs` and add hello following `using` statement:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
5. <span data-ttu-id="26e91-204">V hello `Program` třídy, přidejte následující metodu hello:</span><span class="sxs-lookup"><span data-stu-id="26e91-204">In hello `Program` class, add hello following method:</span></span>
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
            await hub.SendGcmNativeNotificationAsync(message);
        }
   
       Make sure tooreplace hello `<hub name>` placeholder with hello name of hello notification hub that appears in hello [portal](https://portal.azure.com) in your Notification Hub blade. Also, replace hello connection string placeholder with hello connection string called `DefaultFullSharedAccessSignature` that you obtained in hello notification hub configuration section.
   
   > [!NOTE]
   > <span data-ttu-id="26e91-205">Ujistěte se, že používáte připojovací řetězec hello s **úplné** přístupem, nikoli **naslouchání** přístup.</span><span class="sxs-lookup"><span data-stu-id="26e91-205">Make sure that you use hello connection string with **Full** access, not **Listen** access.</span></span> <span data-ttu-id="26e91-206">Hello **naslouchání** připojovací řetězec s přístupem nejsou udělena oprávnění toosend nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="26e91-206">hello **Listen** access connection string does not grant permissions toosend push notifications.</span></span>
   > 
   > 
6. <span data-ttu-id="26e91-207">Přidejte následující hello volá v hello `Main` metoda:</span><span class="sxs-lookup"><span data-stu-id="26e91-207">Add hello following calls in hello `Main` method:</span></span>
   
         SendNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="26e91-208">Ujistěte se, že je spuštěn Chrome a spusťte konzolovou aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="26e91-208">Make sure that Chrome is running, and run hello console application.</span></span>
8. <span data-ttu-id="26e91-209">Měli byste vidět následující hello oznámení otevíraném na ploše.</span><span class="sxs-lookup"><span data-stu-id="26e91-209">You should see hello following notification pop up on your desktop.</span></span>
   
       ![Google Chrome - Notification][13]
9. <span data-ttu-id="26e91-210">Můžete také zobrazit všechna oznámení pomocí okna oznámení Chrome hello panelu hello (v systému Windows) když je Chrome spuštěn.</span><span class="sxs-lookup"><span data-stu-id="26e91-210">You can also see all your notifications by using hello Chrome Notifications window in hello taskbar (in Windows) when Chrome is running.</span></span>
   
       ![Google Chrome - Notifications List][14]

> [!NOTE]
> <span data-ttu-id="26e91-211">Toohave hello aplikace pro Chrome není nutné spuštěné nebo otevřít v prohlížeči hello (i když hello prohlížeč samotný běžet musí).</span><span class="sxs-lookup"><span data-stu-id="26e91-211">You don't need toohave hello Chrome App running or open in hello browser (though hello Chrome browser itself must be running).</span></span> <span data-ttu-id="26e91-212">V okně oznámení pro Chrome hello také získáte celkový pohled na všechna oznámení.</span><span class="sxs-lookup"><span data-stu-id="26e91-212">You also get a consolidated view of all your notifications in hello Chrome Notifications window.</span></span>
> 
> 

## <span data-ttu-id="26e91-213"><a name="next-steps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="26e91-213"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="26e91-214">Další informace o Notification Hubs najdete v [přehledu této služby].</span><span class="sxs-lookup"><span data-stu-id="26e91-214">Learn more about Notification Hubs in [Notification Hubs Overview].</span></span>

<span data-ttu-id="26e91-215">tootarget konkrétní uživatele, najdete v toohello [Azure upozornění uživatelů centra oznámení] kurzu.</span><span class="sxs-lookup"><span data-stu-id="26e91-215">tootarget specific users, refer toohello [Azure Notification Hubs Notify Users] tutorial.</span></span> 

<span data-ttu-id="26e91-216">Pokud chcete toosegment uživatele podle zájmových skupin, můžete postupovat podle hello [nejnovější zprávy přes Azure Notification Hubs] kurzu.</span><span class="sxs-lookup"><span data-stu-id="26e91-216">If you want toosegment your users by interest groups, you can follow hello [Azure Notification Hubs breaking news] tutorial.</span></span>

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
