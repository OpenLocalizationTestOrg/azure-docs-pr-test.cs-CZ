---
title: "Připojení k Azure IoT - Lekce 4 Intel Edison (uzel): Blink Indikátor | Microsoft Docs"
description: "Přizpůsobení zprávy a pokuste se změnit LED zapnout a vypnout chování."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "ovládací prvek vedla s arduino"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 387cd97e-b05e-43c4-b252-f68ad45d524a
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: fa99050dad62534e2825e93f1170d2f3ecf5a3ba
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="change-the-on-and-off-behavior-of-the-led"></a><span data-ttu-id="5b714-104">Změnit zapnuto a vypnuto chování Indikátor</span><span class="sxs-lookup"><span data-stu-id="5b714-104">Change the on and off behavior of the LED</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="5b714-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="5b714-105">What you will do</span></span>
<span data-ttu-id="5b714-106">Přizpůsobení zprávy a pokuste se změnit LED zapnout a vypnout chování.</span><span class="sxs-lookup"><span data-stu-id="5b714-106">Customize the messages to change the LED’s on and off behavior.</span></span> <span data-ttu-id="5b714-107">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="5b714-107">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="5b714-108">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="5b714-108">What you will learn</span></span>
<span data-ttu-id="5b714-109">Na další funkce můžete změnit LED zapnout a vypnout chování.</span><span class="sxs-lookup"><span data-stu-id="5b714-109">Use additional functions to change the LED’s on and off behavior.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="5b714-110">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="5b714-110">What you need</span></span>
<span data-ttu-id="5b714-111">Musíte úspěšně jste dokončili [spuštění ukázkové aplikace na Intel Edison přijímat cloudu na zařízení zprávy][receive-cloud-to-device-messages].</span><span class="sxs-lookup"><span data-stu-id="5b714-111">You must have successfully completed [Run a sample application on Intel Edison to receive cloud to device messages][receive-cloud-to-device-messages].</span></span>

## <a name="add-functions-to-appjs-and-gulpfilejs"></a><span data-ttu-id="5b714-112">Přidání funkce do app.js a gulpfile.js</span><span class="sxs-lookup"><span data-stu-id="5b714-112">Add functions to app.js and gulpfile.js</span></span>
1. <span data-ttu-id="5b714-113">Otevřete ukázkovou aplikaci v sadě Visual Studio kódu spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="5b714-113">Open the sample application in Visual Studio code by running the following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```
2. <span data-ttu-id="5b714-114">Otevřete `app.js` souboru a poté přidejte následující funkce po blinkLED() funkce:</span><span class="sxs-lookup"><span data-stu-id="5b714-114">Open the `app.js` file, and then add the following functions after blinkLED() function:</span></span>

   ```javascript
   function turnOnLED() {
     myLed.write(1);
   }

   function turnOffLED() {
     myLed.write(0);
   }
   ```

   ![Souboru app.js s přidané funkce](media/iot-hub-intel-edison-lessons/lesson4/updated_app_node.png)
3. <span data-ttu-id="5b714-116">Přidejte následující podmínky před 'blikání, tak v případy switch blok `receiveMessageCallback` funkce:</span><span class="sxs-lookup"><span data-stu-id="5b714-116">Add the following conditions before the 'blink' case in the switch-case block of the `receiveMessageCallback` function:</span></span>

   ```javascript
   case 'on':
     turnOnLED();
     break;
   case 'off':
     turnOffLED();
     break;
   ```

   <span data-ttu-id="5b714-117">Nyní jste nakonfigurovali ukázkovou aplikaci reagovat na další pokyny prostřednictvím zprávy.</span><span class="sxs-lookup"><span data-stu-id="5b714-117">Now you’ve configured the sample application to respond to more instructions through messages.</span></span> <span data-ttu-id="5b714-118">Instrukce "na" zapne Indikátor a instrukce "off" vypne Indikátor.</span><span class="sxs-lookup"><span data-stu-id="5b714-118">The "on" instruction turns on the LED, and the "off" instruction turns off the LED.</span></span>
4. <span data-ttu-id="5b714-119">Otevřete soubor gulpfile.js a pak přidejte novou funkci před funkce `sendMessage`:</span><span class="sxs-lookup"><span data-stu-id="5b714-119">Open the gulpfile.js file, and then add a new function before the function `sendMessage`:</span></span>

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

   ![Soubor Gulpfile.js s přidané – funkce][gulpfile]
5. <span data-ttu-id="5b714-121">V `sendMessage` fungovat, nahraďte řádku `var message = buildMessage(sentMessageCount);` s novým řádkem uvedené v následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="5b714-121">In the `sendMessage` function, replace the line `var message = buildMessage(sentMessageCount);` with the new line shown in the following snippet:</span></span>

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. <span data-ttu-id="5b714-122">Změny uložte.</span><span class="sxs-lookup"><span data-stu-id="5b714-122">Save all the changes.</span></span>

### <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="5b714-123">Nasazení a spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="5b714-123">Deploy and run the sample application</span></span>
<span data-ttu-id="5b714-124">Nasazení a spuštění ukázkové aplikace na Edison spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="5b714-124">Deploy and run the sample application on Edison by running the following command:</span></span>

```bash
gulp deploy && gulp run
```

<span data-ttu-id="5b714-125">Měli byste vidět DIODU zapněte na 2 sekundy a poté vypněte pro jiné dvou sekund.</span><span class="sxs-lookup"><span data-stu-id="5b714-125">You should see the LED turn on for two seconds, and then turn off for another two seconds.</span></span> <span data-ttu-id="5b714-126">Poslední zprávu "stop" zastaví spuštění ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="5b714-126">The last "stop" message stops the sample application from running.</span></span>

![zapnout a vypnout][on-and-off]

<span data-ttu-id="5b714-128">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="5b714-128">Congratulations!</span></span> <span data-ttu-id="5b714-129">Úspěšně jste přizpůsobili zprávy, které se odesílají do Edison ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5b714-129">You’ve successfully customized the messages that are sent to Edison from your IoT hub.</span></span>

### <a name="summary"></a><span data-ttu-id="5b714-130">Souhrn</span><span class="sxs-lookup"><span data-stu-id="5b714-130">Summary</span></span>
<span data-ttu-id="5b714-131">Této volitelné části ukazuje, jak přizpůsobit zpráv tak, aby ukázkovou aplikaci může řídit zapnuto a vypnuto chování Indikátor jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="5b714-131">This optional section demonstrates how to customize messages so that the sample application can control the on and off behavior of the LED in a different way.</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[receive-cloud-to-device-messages]: iot-hub-intel-edison-kit-node-lesson4-send-cloud-to-device-messages.md
[gulpfile]: media/iot-hub-intel-edison-lessons/lesson4/updated_gulpfile_node.png
[on-and-off]: media/iot-hub-intel-edison-lessons/lesson4/gulp_on_and_off_node.png
