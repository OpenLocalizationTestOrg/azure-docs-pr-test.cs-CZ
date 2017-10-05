---
title: "Připojení k Azure IoT - Lekce 4 Malinová Pi (C): úpravě aplikace | Microsoft Docs"
description: "Přizpůsobení zprávy a pokuste se změnit LED zapnout a vypnout chování."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "ovládací prvek vedla pomocí Malinová pi, malinová platformy vyvolala ovládací prvek, malinová pí řízení vedla"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 0201b8ed-d5e6-4445-9a4d-1305003d1eff
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: b1e441b20e161f4a03d4c2c300b21aca4fedb2a2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="change-the-on-and-off-behavior-of-the-led"></a><span data-ttu-id="e71a0-104">Změnit zapnuto a vypnuto chování Indikátor</span><span class="sxs-lookup"><span data-stu-id="e71a0-104">Change the on and off behavior of the LED</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="e71a0-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="e71a0-105">What you will do</span></span>
<span data-ttu-id="e71a0-106">Přizpůsobení zprávy a pokuste se změnit LED zapnout a vypnout chování.</span><span class="sxs-lookup"><span data-stu-id="e71a0-106">Customize the messages to change the LED’s on and off behavior.</span></span> <span data-ttu-id="e71a0-107">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="e71a0-107">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="e71a0-108">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="e71a0-108">What you will learn</span></span>
<span data-ttu-id="e71a0-109">Na další funkce Node.js můžete změnit LED zapnout a vypnout chování.</span><span class="sxs-lookup"><span data-stu-id="e71a0-109">Use additional Node.js functions to change the LED’s on and off behavior.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="e71a0-110">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="e71a0-110">What you need</span></span>
<span data-ttu-id="e71a0-111">Musíte úspěšně jste dokončili [spuštění ukázkové aplikace na malin platformy pro příjem cloudu na zařízení zprávy](iot-hub-raspberry-pi-kit-c-lesson4-send-cloud-to-device-messages.md).</span><span class="sxs-lookup"><span data-stu-id="e71a0-111">You must have successfully completed [Run a sample application on Raspberry Pi to receive cloud to device messages](iot-hub-raspberry-pi-kit-c-lesson4-send-cloud-to-device-messages.md).</span></span>

## <a name="add-functions-to-mainc-and-gulpfilejs"></a><span data-ttu-id="e71a0-112">Přidání funkce do main.c a gulpfile.js</span><span class="sxs-lookup"><span data-stu-id="e71a0-112">Add functions to main.c and gulpfile.js</span></span>
1. <span data-ttu-id="e71a0-113">Otevřete ukázkovou aplikaci v sadě Visual Studio kódu spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="e71a0-113">Open the sample application in Visual Studio code by running the following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```
2. <span data-ttu-id="e71a0-114">Otevřete `main.c` souboru a poté přidejte následující funkce po blinkLED() funkce:</span><span class="sxs-lookup"><span data-stu-id="e71a0-114">Open the `main.c` file, and then add the following functions after blinkLED() function:</span></span>

   ```c
   static void turnOnLED()
   {
     digitalWrite(LED_PIN, HIGH);
   }

   static void turnOffLED()
   {
     digitalWrite(LED_PIN, LOW);
   }
   ```

   ![soubor Main.c s přidané funkce](media/iot-hub-raspberry-pi-lessons/lesson4/updated_app_c.png)
3. <span data-ttu-id="e71a0-116">Přidejte následující podmínky před výchozí nastavení v `if` blokování z `receiveMessageCallback` funkce:</span><span class="sxs-lookup"><span data-stu-id="e71a0-116">Add the following conditions before the default one in the `if` block of the `receiveMessageCallback` function:</span></span>

   ```c
   else if (0 == strcmp((const char*)value, "\"on\""))
   {
       turnOnLED();
   }
   else if (0 == strcmp((const char*)value, "\"off\""))
   {
       turnOffLED();
   }
   ```

   <span data-ttu-id="e71a0-117">Nyní jste nakonfigurovali ukázkovou aplikaci reagovat na další pokyny prostřednictvím zprávy.</span><span class="sxs-lookup"><span data-stu-id="e71a0-117">Now you’ve configured the sample application to respond to more instructions through messages.</span></span> <span data-ttu-id="e71a0-118">Instrukce "na" zapne Indikátor a instrukce "off" vypne Indikátor.</span><span class="sxs-lookup"><span data-stu-id="e71a0-118">The "on" instruction turns on the LED, and the "off" instruction turns off the LED.</span></span>
4. <span data-ttu-id="e71a0-119">Otevřete soubor gulpfile.js a pak přidejte novou funkci před funkce `sendMessage`:</span><span class="sxs-lookup"><span data-stu-id="e71a0-119">Open the gulpfile.js file, and then add a new function before the function `sendMessage`:</span></span>

   ```javascript
   var buildCustomMessage = function (messageId) {
     if ((messageId & 1) && (messageId < MAX_MESSAGE_COUNT)) {
       return new Message(JSON.stringify({ command: 'on', messageId: messageId }));
     } else if (messageId < MAX_MESSAGE_COUNT) {
       return new Message(JSON.stringify({ command: 'off', messageId: messageId }));
     } else {
       return new Message(JSON.stringify({ command: 'stop', messageId: messageId }));
     }
   }
   ```

   ![Soubor Gulpfile.js s přidané – funkce](media/iot-hub-raspberry-pi-lessons/lesson4/updated_gulpfile_c.png)
5. <span data-ttu-id="e71a0-121">V `sendMessage` fungovat, nahraďte řádku `var message = buildMessage(sentMessageCount);` s novým řádkem uvedené v následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="e71a0-121">In the `sendMessage` function, replace the line `var message = buildMessage(sentMessageCount);` with the new line shown in the following snippet:</span></span>

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. <span data-ttu-id="e71a0-122">Změny uložte.</span><span class="sxs-lookup"><span data-stu-id="e71a0-122">Save all the changes.</span></span>

### <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="e71a0-123">Nasazení a spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="e71a0-123">Deploy and run the sample application</span></span>
<span data-ttu-id="e71a0-124">Nasazení a spuštění ukázkové aplikace na platformy spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="e71a0-124">Deploy and run the sample application on Pi by running the following command:</span></span>

```bash
gulp deploy && gulp run
```

<span data-ttu-id="e71a0-125">Měli byste vidět DIODU zapněte na 2 sekundy a poté vypněte pro jiné dvou sekund.</span><span class="sxs-lookup"><span data-stu-id="e71a0-125">You should see the LED turn on for two seconds, and then turn off for another two seconds.</span></span> <span data-ttu-id="e71a0-126">Poslední zprávu "stop" zastaví spuštění ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e71a0-126">The last "stop" message stops the sample application from running.</span></span>

![Ukázkovou aplikaci s zapnout a vypnout zprávy](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off_c.png)

<span data-ttu-id="e71a0-128">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="e71a0-128">Congratulations!</span></span> <span data-ttu-id="e71a0-129">Úspěšně jste přizpůsobili zprávy, které se odesílají do pí ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e71a0-129">You’ve successfully customized the messages that are sent to Pi from your IoT hub.</span></span>

### <a name="summary"></a><span data-ttu-id="e71a0-130">Souhrn</span><span class="sxs-lookup"><span data-stu-id="e71a0-130">Summary</span></span>
<span data-ttu-id="e71a0-131">Této volitelné části ukazuje, jak přizpůsobit zpráv tak, aby ukázkovou aplikaci může řídit zapnuto a vypnuto chování Indikátor jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="e71a0-131">This optional section demonstrates how to customize messages so that the sample application can control the on and off behavior of the LED in a different way.</span></span>
