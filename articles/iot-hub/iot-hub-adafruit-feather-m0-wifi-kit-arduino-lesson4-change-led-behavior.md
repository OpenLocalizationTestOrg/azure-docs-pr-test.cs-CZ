---
title: "Connect Arduino (C) tooAzure IoT - Lekce 4: úpravě aplikace | Microsoft Docs"
description: "Přizpůsobte hello zprávy toochange hello DIODU je zapnout a vypnout chování."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "ovládací prvek vedla s arduino"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: d7a25430-450e-43c4-a3ed-1eed995b8b7e
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 8cc438650f01ae4335d91c94df6a29e0ffbdc508
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-on-and-off-behavior-of-hello-led"></a><span data-ttu-id="bd3ec-104">Změnit hello zapnout a vypnout chování hello DIODU</span><span class="sxs-lookup"><span data-stu-id="bd3ec-104">Change hello on and off behavior of hello LED</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="bd3ec-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="bd3ec-105">What you will do</span></span>
<span data-ttu-id="bd3ec-106">Přizpůsobte hello zprávy toochange hello DIODU je zapnout a vypnout chování.</span><span class="sxs-lookup"><span data-stu-id="bd3ec-106">Customize hello messages toochange hello LED’s on and off behavior.</span></span> <span data-ttu-id="bd3ec-107">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) pro panel Adafruit prolnutí M0 Wi-Fi Arduino.</span><span class="sxs-lookup"><span data-stu-id="bd3ec-107">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) for your Adafruit Feather M0 WiFi Arduino board.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="bd3ec-108">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="bd3ec-108">What you will learn</span></span>
<span data-ttu-id="bd3ec-109">Použijte další Arduino funkce toochange hello DIODU je zapnout a vypnout chování.</span><span class="sxs-lookup"><span data-stu-id="bd3ec-109">Use additional Arduino functions toochange hello LED’s on and off behavior.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="bd3ec-110">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="bd3ec-110">What you need</span></span>
<span data-ttu-id="bd3ec-111">Musíte úspěšně jste dokončili [spusťte ukázkovou aplikaci v cloudu Arduino Tabule tooreceive toodevice zprávy][receive-cloud-to-device-messages].</span><span class="sxs-lookup"><span data-stu-id="bd3ec-111">You must have successfully completed [Run a sample application on your Arduino board tooreceive cloud toodevice messages][receive-cloud-to-device-messages].</span></span>

## <a name="add-functions-toomainc-and-gulpfilejs"></a><span data-ttu-id="bd3ec-112">Přidání funkce toomain.c a gulpfile.js</span><span class="sxs-lookup"><span data-stu-id="bd3ec-112">Add functions toomain.c and gulpfile.js</span></span>
1. <span data-ttu-id="bd3ec-113">Otevřete hello ukázkovou aplikaci v sadě Visual Studio kódu spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="bd3ec-113">Open hello sample application in Visual Studio code by running hello following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```
2. <span data-ttu-id="bd3ec-114">Otevřete hello `app.ino` souboru a poté přidejte hello po blinkLED() funkce následující funkce:</span><span class="sxs-lookup"><span data-stu-id="bd3ec-114">Open hello `app.ino` file, and then add hello following functions after blinkLED() function:</span></span>

   ```arduino
   static void turnOnLED()
   {
     digitalWrite(LED_PIN, HIGH);
   }

   static void turnOffLED()
   {
     digitalWrite(LED_PIN, LOW);
   }
   ```

   ![soubor App.ino s přidané funkce][app-ino-file]
3. <span data-ttu-id="bd3ec-116">Přidejte následující podmínky před hello hello `else if` blok hello `receiveMessageCallback` funkce:</span><span class="sxs-lookup"><span data-stu-id="bd3ec-116">Add hello following conditions before hello `else if` block of hello `receiveMessageCallback` function:</span></span>

   ```arduino
   else if (strcmp((const char*)value, "\"on\"") == 0)
   {
       turnOnLED();
   }
   else if (0 == strcmp((const char*)value, "\"off\"") == 0)
   {
       turnOffLED();
   }
   ```

   <span data-ttu-id="bd3ec-117">Nyní jste nakonfigurovali hello ukázkové aplikace toorespond toomore pokyny prostřednictvím zprávy.</span><span class="sxs-lookup"><span data-stu-id="bd3ec-117">Now you’ve configured hello sample application toorespond toomore instructions through messages.</span></span> <span data-ttu-id="bd3ec-118">Hello "na" instrukce zapne hello DIODU a hello "off" instrukce vypne hello Indikátor.</span><span class="sxs-lookup"><span data-stu-id="bd3ec-118">hello "on" instruction turns on hello LED, and hello "off" instruction turns off hello LED.</span></span>
4. <span data-ttu-id="bd3ec-119">Otevřete soubor gulpfile.js hello a pak přidejte novou funkci před hello funkce `sendMessage`:</span><span class="sxs-lookup"><span data-stu-id="bd3ec-119">Open hello gulpfile.js file, and then add a new function before hello function `sendMessage`:</span></span>

   ```javascript
   var buildCustomMessage = function (messageId) {
     if ((messageId & 1) && (messageId < MAX_MESSAGE_COUNT)) {
       return new Message(JSON.stringify({ command: 'on', messageId: messageId }));
     } else if (messageId < MAX_MESSAGE_COUNT) {
       return new Message(JSON.stringify({ command: 'off', messageId: messageId }));
     } else {
       return new Message(JSON.stringify({ command: 'stop', messageId: messageId }));
     }
   };
   ```

   ![Soubor Gulpfile.js s přidané – funkce][gulp-file-js]
5. <span data-ttu-id="bd3ec-121">V hello `sendMessage` fungovat, nahraďte hello řádku `var message = buildMessage(sentMessageCount);` s hello nový řádek ukazuje hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="bd3ec-121">In hello `sendMessage` function, replace hello line `var message = buildMessage(sentMessageCount);` with hello new line shown in hello following snippet:</span></span>

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. <span data-ttu-id="bd3ec-122">Uložte všechny změny hello.</span><span class="sxs-lookup"><span data-stu-id="bd3ec-122">Save all hello changes.</span></span>

### <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="bd3ec-123">Nasazení a spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="bd3ec-123">Deploy and run hello sample application</span></span>
<span data-ttu-id="bd3ec-124">Nasazení a spuštění ukázkové aplikace hello na panel Arduino spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="bd3ec-124">Deploy and run hello sample application on your Arduino board by running hello following command:</span></span>

```bash
gulp run
# You can monitor hello serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

<span data-ttu-id="bd3ec-125">Měli byste vidět hello DIODU zapněte na 2 sekundy a poté vypněte pro jiné dvou sekund.</span><span class="sxs-lookup"><span data-stu-id="bd3ec-125">You should see hello LED turn on for two seconds, and then turn off for another two seconds.</span></span> <span data-ttu-id="bd3ec-126">poslední zprávu "stop" Hello zastaví hello spuštění ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="bd3ec-126">hello last "stop" message stops hello sample application from running.</span></span>

![zapnout a vypnout][on-and-off]

<span data-ttu-id="bd3ec-128">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="bd3ec-128">Congratulations!</span></span> <span data-ttu-id="bd3ec-129">Úspěšně jste přizpůsobili hello zpráv, které jsou odeslány tooyour Arduino Tabule ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="bd3ec-129">You’ve successfully customized hello messages that are sent tooyour Arduino board from your IoT hub.</span></span>

### <a name="summary"></a><span data-ttu-id="bd3ec-130">Souhrn</span><span class="sxs-lookup"><span data-stu-id="bd3ec-130">Summary</span></span>
<span data-ttu-id="bd3ec-131">V této volitelné části ukážeme, jak toocustomize zpráv tak, že hello ukázkovou aplikaci může řídit hello zapnout a vypnout chování hello DIODU jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="bd3ec-131">This optional section demonstrates how toocustomize messages so that hello sample application can control hello on and off behavior of hello LED in a different way.</span></span>

<!-- Images and links -->

[receive-cloud-to-device-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-send-cloud-to-device-messages.md
[app-ino-file]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/updated_app_ino.png
[gulp-file-js]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/updated_gulpfile_js.png
[on-and-off]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/gulp_on_and_off_arduino.png