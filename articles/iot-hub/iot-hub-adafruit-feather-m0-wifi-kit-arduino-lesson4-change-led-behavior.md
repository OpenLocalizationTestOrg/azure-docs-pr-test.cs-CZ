---
title: "Připojení k Azure IoT - Lekce 4 Arduino (C): úpravě aplikace | Microsoft Docs"
description: "Přizpůsobení zprávy a pokuste se změnit LED zapnout a vypnout chování."
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
ms.openlocfilehash: 5009a0466f2c5689b8ab426049f4c4f02272512b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="change-the-on-and-off-behavior-of-the-led"></a><span data-ttu-id="eba3b-104">Změnit zapnuto a vypnuto chování Indikátor</span><span class="sxs-lookup"><span data-stu-id="eba3b-104">Change the on and off behavior of the LED</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="eba3b-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="eba3b-105">What you will do</span></span>
<span data-ttu-id="eba3b-106">Přizpůsobení zprávy a pokuste se změnit LED zapnout a vypnout chování.</span><span class="sxs-lookup"><span data-stu-id="eba3b-106">Customize the messages to change the LED’s on and off behavior.</span></span> <span data-ttu-id="eba3b-107">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) pro panel Adafruit prolnutí M0 Wi-Fi Arduino.</span><span class="sxs-lookup"><span data-stu-id="eba3b-107">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) for your Adafruit Feather M0 WiFi Arduino board.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="eba3b-108">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="eba3b-108">What you will learn</span></span>
<span data-ttu-id="eba3b-109">Na další funkce Arduino můžete změnit LED zapnout a vypnout chování.</span><span class="sxs-lookup"><span data-stu-id="eba3b-109">Use additional Arduino functions to change the LED’s on and off behavior.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="eba3b-110">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="eba3b-110">What you need</span></span>
<span data-ttu-id="eba3b-111">Musíte úspěšně jste dokončili [spuštění ukázkové aplikace na vaší kartě Arduino přijímat cloudu na zařízení zprávy][receive-cloud-to-device-messages].</span><span class="sxs-lookup"><span data-stu-id="eba3b-111">You must have successfully completed [Run a sample application on your Arduino board to receive cloud to device messages][receive-cloud-to-device-messages].</span></span>

## <a name="add-functions-to-mainc-and-gulpfilejs"></a><span data-ttu-id="eba3b-112">Přidání funkce do main.c a gulpfile.js</span><span class="sxs-lookup"><span data-stu-id="eba3b-112">Add functions to main.c and gulpfile.js</span></span>
1. <span data-ttu-id="eba3b-113">Otevřete ukázkovou aplikaci v sadě Visual Studio kódu spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="eba3b-113">Open the sample application in Visual Studio code by running the following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```
2. <span data-ttu-id="eba3b-114">Otevřete `app.ino` souboru a poté přidejte následující funkce po blinkLED() funkce:</span><span class="sxs-lookup"><span data-stu-id="eba3b-114">Open the `app.ino` file, and then add the following functions after blinkLED() function:</span></span>

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
3. <span data-ttu-id="eba3b-116">Přidejte následující podmínky před `else if` blokování z `receiveMessageCallback` funkce:</span><span class="sxs-lookup"><span data-stu-id="eba3b-116">Add the following conditions before the `else if` block of the `receiveMessageCallback` function:</span></span>

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

   <span data-ttu-id="eba3b-117">Nyní jste nakonfigurovali ukázkovou aplikaci reagovat na další pokyny prostřednictvím zprávy.</span><span class="sxs-lookup"><span data-stu-id="eba3b-117">Now you’ve configured the sample application to respond to more instructions through messages.</span></span> <span data-ttu-id="eba3b-118">Instrukce "na" zapne Indikátor a instrukce "off" vypne Indikátor.</span><span class="sxs-lookup"><span data-stu-id="eba3b-118">The "on" instruction turns on the LED, and the "off" instruction turns off the LED.</span></span>
4. <span data-ttu-id="eba3b-119">Otevřete soubor gulpfile.js a pak přidejte novou funkci před funkce `sendMessage`:</span><span class="sxs-lookup"><span data-stu-id="eba3b-119">Open the gulpfile.js file, and then add a new function before the function `sendMessage`:</span></span>

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
5. <span data-ttu-id="eba3b-121">V `sendMessage` fungovat, nahraďte řádku `var message = buildMessage(sentMessageCount);` s novým řádkem uvedené v následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="eba3b-121">In the `sendMessage` function, replace the line `var message = buildMessage(sentMessageCount);` with the new line shown in the following snippet:</span></span>

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. <span data-ttu-id="eba3b-122">Změny uložte.</span><span class="sxs-lookup"><span data-stu-id="eba3b-122">Save all the changes.</span></span>

### <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="eba3b-123">Nasazení a spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="eba3b-123">Deploy and run the sample application</span></span>
<span data-ttu-id="eba3b-124">Nasazení a spuštění ukázkové aplikace na panel Arduino spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="eba3b-124">Deploy and run the sample application on your Arduino board by running the following command:</span></span>

```bash
gulp run
# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

<span data-ttu-id="eba3b-125">Měli byste vidět DIODU zapněte na 2 sekundy a poté vypněte pro jiné dvou sekund.</span><span class="sxs-lookup"><span data-stu-id="eba3b-125">You should see the LED turn on for two seconds, and then turn off for another two seconds.</span></span> <span data-ttu-id="eba3b-126">Poslední zprávu "stop" zastaví spuštění ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="eba3b-126">The last "stop" message stops the sample application from running.</span></span>

![zapnout a vypnout][on-and-off]

<span data-ttu-id="eba3b-128">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="eba3b-128">Congratulations!</span></span> <span data-ttu-id="eba3b-129">Úspěšně jste přizpůsobili zprávy, které se odesílají do vaší karty Arduino ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="eba3b-129">You’ve successfully customized the messages that are sent to your Arduino board from your IoT hub.</span></span>

### <a name="summary"></a><span data-ttu-id="eba3b-130">Souhrn</span><span class="sxs-lookup"><span data-stu-id="eba3b-130">Summary</span></span>
<span data-ttu-id="eba3b-131">Této volitelné části ukazuje, jak přizpůsobit zpráv tak, aby ukázkovou aplikaci může řídit zapnuto a vypnuto chování Indikátor jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="eba3b-131">This optional section demonstrates how to customize messages so that the sample application can control the on and off behavior of the LED in a different way.</span></span>

<!-- Images and links -->

[receive-cloud-to-device-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-send-cloud-to-device-messages.md
[app-ino-file]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/updated_app_ino.png
[gulp-file-js]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/updated_gulpfile_js.png
[on-and-off]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/gulp_on_and_off_arduino.png