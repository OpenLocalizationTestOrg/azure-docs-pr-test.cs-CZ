---
title: "Připojit Intel Edison (uzel) tooAzure IoT - Lekce 4: Blink hello DIODU | Microsoft Docs"
description: "Přizpůsobte hello zprávy toochange hello DIODU je zapnout a vypnout chování."
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
ms.openlocfilehash: caeabe311fd1698f298c6d2b4a203ecad80ef7df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-on-and-off-behavior-of-hello-led"></a><span data-ttu-id="3a6a1-104">Změnit hello zapnout a vypnout chování hello DIODU</span><span class="sxs-lookup"><span data-stu-id="3a6a1-104">Change hello on and off behavior of hello LED</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="3a6a1-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="3a6a1-105">What you will do</span></span>
<span data-ttu-id="3a6a1-106">Přizpůsobte hello zprávy toochange hello DIODU je zapnout a vypnout chování.</span><span class="sxs-lookup"><span data-stu-id="3a6a1-106">Customize hello messages toochange hello LED’s on and off behavior.</span></span> <span data-ttu-id="3a6a1-107">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="3a6a1-107">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="3a6a1-108">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="3a6a1-108">What you will learn</span></span>
<span data-ttu-id="3a6a1-109">Použijte další funkce toochange hello DIODU je zapnout a vypnout chování.</span><span class="sxs-lookup"><span data-stu-id="3a6a1-109">Use additional functions toochange hello LED’s on and off behavior.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="3a6a1-110">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="3a6a1-110">What you need</span></span>
<span data-ttu-id="3a6a1-111">Musíte úspěšně jste dokončili [spusťte ukázkovou aplikaci v cloudu tooreceive Intel Edison toodevice zprávy][receive-cloud-to-device-messages].</span><span class="sxs-lookup"><span data-stu-id="3a6a1-111">You must have successfully completed [Run a sample application on Intel Edison tooreceive cloud toodevice messages][receive-cloud-to-device-messages].</span></span>

## <a name="add-functions-tooappjs-and-gulpfilejs"></a><span data-ttu-id="3a6a1-112">Přidání funkce tooapp.js a gulpfile.js</span><span class="sxs-lookup"><span data-stu-id="3a6a1-112">Add functions tooapp.js and gulpfile.js</span></span>
1. <span data-ttu-id="3a6a1-113">Otevřete hello ukázkovou aplikaci v sadě Visual Studio kódu spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="3a6a1-113">Open hello sample application in Visual Studio code by running hello following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```
2. <span data-ttu-id="3a6a1-114">Otevřete hello `app.js` souboru a poté přidejte hello po blinkLED() funkce následující funkce:</span><span class="sxs-lookup"><span data-stu-id="3a6a1-114">Open hello `app.js` file, and then add hello following functions after blinkLED() function:</span></span>

   ```javascript
   function turnOnLED() {
     myLed.write(1);
   }

   function turnOffLED() {
     myLed.write(0);
   }
   ```

   ![Souboru app.js s přidané funkce](media/iot-hub-intel-edison-lessons/lesson4/updated_app_node.png)
3. <span data-ttu-id="3a6a1-116">Přidat hello následující podmínky před hello 'blink"hello případy switch blok hello v takovém případě `receiveMessageCallback` funkce:</span><span class="sxs-lookup"><span data-stu-id="3a6a1-116">Add hello following conditions before hello 'blink' case in hello switch-case block of hello `receiveMessageCallback` function:</span></span>

   ```javascript
   case 'on':
     turnOnLED();
     break;
   case 'off':
     turnOffLED();
     break;
   ```

   <span data-ttu-id="3a6a1-117">Nyní jste nakonfigurovali hello ukázkové aplikace toorespond toomore pokyny prostřednictvím zprávy.</span><span class="sxs-lookup"><span data-stu-id="3a6a1-117">Now you’ve configured hello sample application toorespond toomore instructions through messages.</span></span> <span data-ttu-id="3a6a1-118">Hello "na" instrukce zapne hello DIODU a hello "off" instrukce vypne hello Indikátor.</span><span class="sxs-lookup"><span data-stu-id="3a6a1-118">hello "on" instruction turns on hello LED, and hello "off" instruction turns off hello LED.</span></span>
4. <span data-ttu-id="3a6a1-119">Otevřete soubor gulpfile.js hello a pak přidejte novou funkci před hello funkce `sendMessage`:</span><span class="sxs-lookup"><span data-stu-id="3a6a1-119">Open hello gulpfile.js file, and then add a new function before hello function `sendMessage`:</span></span>

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
5. <span data-ttu-id="3a6a1-121">V hello `sendMessage` fungovat, nahraďte hello řádku `var message = buildMessage(sentMessageCount);` s hello nový řádek ukazuje hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="3a6a1-121">In hello `sendMessage` function, replace hello line `var message = buildMessage(sentMessageCount);` with hello new line shown in hello following snippet:</span></span>

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. <span data-ttu-id="3a6a1-122">Uložte všechny změny hello.</span><span class="sxs-lookup"><span data-stu-id="3a6a1-122">Save all hello changes.</span></span>

### <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="3a6a1-123">Nasazení a spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="3a6a1-123">Deploy and run hello sample application</span></span>
<span data-ttu-id="3a6a1-124">Nasazení a spuštění ukázkové aplikace hello na Edison spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="3a6a1-124">Deploy and run hello sample application on Edison by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

<span data-ttu-id="3a6a1-125">Měli byste vidět hello DIODU zapněte na 2 sekundy a poté vypněte pro jiné dvou sekund.</span><span class="sxs-lookup"><span data-stu-id="3a6a1-125">You should see hello LED turn on for two seconds, and then turn off for another two seconds.</span></span> <span data-ttu-id="3a6a1-126">poslední zprávu "stop" Hello zastaví hello spuštění ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3a6a1-126">hello last "stop" message stops hello sample application from running.</span></span>

![zapnout a vypnout][on-and-off]

<span data-ttu-id="3a6a1-128">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="3a6a1-128">Congratulations!</span></span> <span data-ttu-id="3a6a1-129">Úspěšně jste přizpůsobili hello zpráv, které jsou odeslány tooEdison ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="3a6a1-129">You’ve successfully customized hello messages that are sent tooEdison from your IoT hub.</span></span>

### <a name="summary"></a><span data-ttu-id="3a6a1-130">Souhrn</span><span class="sxs-lookup"><span data-stu-id="3a6a1-130">Summary</span></span>
<span data-ttu-id="3a6a1-131">V této volitelné části ukážeme, jak toocustomize zpráv tak, že hello ukázkovou aplikaci může řídit hello zapnout a vypnout chování hello DIODU jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="3a6a1-131">This optional section demonstrates how toocustomize messages so that hello sample application can control hello on and off behavior of hello LED in a different way.</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[receive-cloud-to-device-messages]: iot-hub-intel-edison-kit-node-lesson4-send-cloud-to-device-messages.md
[gulpfile]: media/iot-hub-intel-edison-lessons/lesson4/updated_gulpfile_node.png
[on-and-off]: media/iot-hub-intel-edison-lessons/lesson4/gulp_on_and_off_node.png
