---
title: "Připojit malin platformy (uzel) tooAzure IoT - Lekce 4: úpravě aplikace | Microsoft Docs"
description: "Přizpůsobte hello zprávy toochange hello DIODU je zapnout a vypnout chování."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "ovládací prvek vedla pomocí Malinová pi, malinová platformy vyvolala ovládací prvek, malinová pí řízení vedla"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 3b42a4ad-0197-42f6-8ca9-04c883e879e8
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 99b542fcb8639add0f5a0f7a49dd8abd0e224a51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-on-and-off-behavior-of-hello-led"></a><span data-ttu-id="09070-104">Změnit hello zapnout a vypnout chování hello DIODU</span><span class="sxs-lookup"><span data-stu-id="09070-104">Change hello on and off behavior of hello LED</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="09070-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="09070-105">What you will do</span></span>
<span data-ttu-id="09070-106">Přizpůsobte hello zprávy toochange hello DIODU je zapnout a vypnout chování.</span><span class="sxs-lookup"><span data-stu-id="09070-106">Customize hello messages toochange hello LED’s on and off behavior.</span></span> <span data-ttu-id="09070-107">Pokud máte potíže, hledají řešení na hello [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="09070-107">If you have any problems, seek solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="09070-108">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="09070-108">What you will learn</span></span>
<span data-ttu-id="09070-109">Použijte další Node.js funkce toochange hello DIODU je zapnout a vypnout chování.</span><span class="sxs-lookup"><span data-stu-id="09070-109">Use additional Node.js functions toochange hello LED’s on and off behavior.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="09070-110">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="09070-110">What you need</span></span>
<span data-ttu-id="09070-111">Musíte úspěšně jste dokončili [spuštění ukázkové aplikace na malin pí tooreceive zprávy typu cloud zařízení](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md).</span><span class="sxs-lookup"><span data-stu-id="09070-111">You must have successfully completed [Run a sample application on Raspberry Pi tooreceive cloud-to-device messages](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md).</span></span>

## <a name="add-nodejs-functions"></a><span data-ttu-id="09070-112">Přidání funkce Node.js</span><span class="sxs-lookup"><span data-stu-id="09070-112">Add Node.js functions</span></span>
1. <span data-ttu-id="09070-113">Otevřete hello ukázkovou aplikaci v sadě Visual Studio kódu spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="09070-113">Open hello sample application in Visual Studio code by running hello following commands:</span></span>
   
   ```bash
   cd Lesson4
   code .
   ```
2. <span data-ttu-id="09070-114">Otevřete hello `app.js` souboru a poté přidejte následující funkce na konci hello hello:</span><span class="sxs-lookup"><span data-stu-id="09070-114">Open hello `app.js` file, and then add hello following functions at hello end:</span></span>
   
   ```javascript
   function turnOnLED() {
     wpi.digitalWrite(CONFIG_PIN, 1);
   }
   
   function turnOffLED() {
     wpi.digitalWrite(CONFIG_PIN, 0);
   }
   ```
   
   ![Souboru app.js s přidané funkce](media/iot-hub-raspberry-pi-lessons/lesson4/updated_app_js.png)
3. <span data-ttu-id="09070-116">Přidejte následující podmínky před hello výchozí nastavení v bloku případy switch hello hello hello `receiveMessageCallback` funkce:</span><span class="sxs-lookup"><span data-stu-id="09070-116">Add hello following conditions before hello default one in hello switch-case block of hello `receiveMessageCallback` function:</span></span>
   
   ```javascript
   case 'on':
     turnOnLED();
     break;
   case 'off':
     turnOffLED();
     break;
   ```
   
   <span data-ttu-id="09070-117">Nyní jste nakonfigurovali hello ukázkové aplikace toorespond toomore pokyny prostřednictvím zprávy.</span><span class="sxs-lookup"><span data-stu-id="09070-117">Now you’ve configured hello sample application toorespond toomore instructions through messages.</span></span> <span data-ttu-id="09070-118">Hello "na" instrukce zapne hello DIODU a hello "off" instrukce vypne hello Indikátor.</span><span class="sxs-lookup"><span data-stu-id="09070-118">hello "on" instruction turns on hello LED, and hello "off" instruction turns off hello LED.</span></span>
4. <span data-ttu-id="09070-119">Otevřete soubor gulpfile.js hello a pak přidejte novou funkci před hello funkce `sendMessage`:</span><span class="sxs-lookup"><span data-stu-id="09070-119">Open hello gulpfile.js file, and then add a new function before hello function `sendMessage`:</span></span>
   
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
   
   ![Soubor Gulpfile.js s přidané – funkce](media/iot-hub-raspberry-pi-lessons/lesson4/updated_gulpfile.png)
5. <span data-ttu-id="09070-121">V hello `sendMessage` fungovat, nahraďte hello řádku `var message = buildMessage(sentMessageCount);` s hello nový řádek ukazuje hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="09070-121">In hello `sendMessage` function, replace hello line `var message = buildMessage(sentMessageCount);` with hello new line shown in hello following snippet:</span></span>
   
   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. <span data-ttu-id="09070-122">Uložte všechny změny hello.</span><span class="sxs-lookup"><span data-stu-id="09070-122">Save all hello changes.</span></span>

### <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="09070-123">Nasazení a spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="09070-123">Deploy and run hello sample application</span></span>
<span data-ttu-id="09070-124">Nasazení a spuštění ukázkové aplikace hello na platformy spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="09070-124">Deploy and run hello sample application on Pi by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

<span data-ttu-id="09070-125">Měli byste vidět hello DIODU zapněte na 2 sekundy a poté vypněte pro jiné dvou sekund.</span><span class="sxs-lookup"><span data-stu-id="09070-125">You should see hello LED turn on for two seconds, and then turn off for another two seconds.</span></span> <span data-ttu-id="09070-126">poslední zprávu "stop" Hello zastaví hello spuštění ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="09070-126">hello last "stop" message stops hello sample application from running.</span></span>

![Ukázkovou aplikaci s zapnout a vypnout zprávy](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off.png)

<span data-ttu-id="09070-128">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="09070-128">Congratulations!</span></span> <span data-ttu-id="09070-129">Úspěšně jste přizpůsobili hello zpráv, které jsou odeslány tooPi ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="09070-129">You’ve successfully customized hello messages that are sent tooPi from your IoT hub.</span></span>

### <a name="summary"></a><span data-ttu-id="09070-130">Souhrn</span><span class="sxs-lookup"><span data-stu-id="09070-130">Summary</span></span>
<span data-ttu-id="09070-131">V této volitelné části ukážeme, jak toocustomize zpráv tak, že hello ukázkovou aplikaci může řídit hello zapnout a vypnout chování hello DIODU jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="09070-131">This optional section demonstrates how toocustomize messages so that hello sample application can control hello on and off behavior of hello LED in a different way.</span></span>

