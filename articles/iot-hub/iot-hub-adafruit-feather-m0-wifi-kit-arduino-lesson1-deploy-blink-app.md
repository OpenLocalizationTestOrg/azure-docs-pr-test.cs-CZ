---
title: "Připojit Arduino tooAzure IoT - lekci 1: nasazení aplikace | Microsoft Docs"
description: "Klonování hello ukázkovou aplikaci Arduino z Githubu a spusťte gulp toodeploy této aplikace tooyour Adafruit prolnutí M0 Wi-Fi. Tato ukázková aplikace bliká hello GPIO"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino vedla projekty, arduino vedla blikání, arduino vedla blikání kód, program blikání arduino, příklad arduino blikání"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: b0a7d076-d580-4686-9f7d-c0712750b615
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 5bf8e4ae88e070aeacf34bfc43b8d2daeeb1a2fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-hello-blink-application"></a><span data-ttu-id="36016-105">Vytvoření a nasazení aplikace hello blikání</span><span class="sxs-lookup"><span data-stu-id="36016-105">Create and deploy hello blink application</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="36016-106">Co provedete</span><span class="sxs-lookup"><span data-stu-id="36016-106">What you will do</span></span>
<span data-ttu-id="36016-107">Klonování hello ukázkovou aplikaci Arduino z Githubu a používat hello gulp nástroj toodeploy hello ukázkové aplikace tooyour Adafruit prolnutí M0 Wi-Fi Arduino panelu.</span><span class="sxs-lookup"><span data-stu-id="36016-107">Clone hello sample Arduino application from GitHub, and use hello gulp tool toodeploy hello sample application tooyour Adafruit Feather M0 WiFi Arduino board.</span></span> <span data-ttu-id="36016-108">Hello ukázkové aplikace bliká hello GPIO č. 13 barod VEDLA každé dvě sekundy.</span><span class="sxs-lookup"><span data-stu-id="36016-108">hello sample application blinks hello GPIO #13 on-barod LED every two seconds.</span></span>

<span data-ttu-id="36016-109">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting-page].</span><span class="sxs-lookup"><span data-stu-id="36016-109">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting-page].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="36016-110">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="36016-110">What you will learn</span></span>
* <span data-ttu-id="36016-111">Jak toodeploy a spuštění hello ukázkové aplikace na vaší kartě Arduino.</span><span class="sxs-lookup"><span data-stu-id="36016-111">How toodeploy and run hello sample application on your Arduino board.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="36016-112">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="36016-112">What you need</span></span>
<span data-ttu-id="36016-113">Musíte úspěšně jste dokončili hello následující operace:</span><span class="sxs-lookup"><span data-stu-id="36016-113">You must have successfully completed hello following operations:</span></span>

* <span data-ttu-id="36016-114">[Konfigurace zařízení][configure-your-device]</span><span class="sxs-lookup"><span data-stu-id="36016-114">[Configure your device][configure-your-device]</span></span>
* <span data-ttu-id="36016-115">[Získat nástroje hello][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="36016-115">[Get hello tools][get-the-tools]</span></span>

## <a name="open-hello-sample-application"></a><span data-ttu-id="36016-116">Ukázkové aplikace otevřete hello</span><span class="sxs-lookup"><span data-stu-id="36016-116">Open hello sample application</span></span>
<span data-ttu-id="36016-117">tooopen hello ukázkové aplikace, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="36016-117">tooopen hello sample application, follow these steps:</span></span>

1. <span data-ttu-id="36016-118">Klonování úložiště ukázkové hello z Githubu spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="36016-118">Clone hello sample repository from GitHub by running hello following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-feather-m0-getting-started.git
   ```
2. <span data-ttu-id="36016-119">Otevřete hello ukázkovou aplikaci v aplikaci Visual Studio Code spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="36016-119">Open hello sample application in Visual Studio Code by running hello following commands:</span></span>

   ```bash
   cd iot-hub-c-feather-m0-getting-started
   cd Lesson1
   code .
   ```

   ![Struktura úložiště][repo-structure]

<span data-ttu-id="36016-121">Hello `app.ino` souboru v hello `app` podsložky je hello klíče zdrojový soubor, který obsahuje hello kód toocontrol hello Indikátor.</span><span class="sxs-lookup"><span data-stu-id="36016-121">hello `app.ino` file in hello `app` subfolder is hello key source file that contains hello code toocontrol hello LED.</span></span>

### <a name="install-application-dependencies"></a><span data-ttu-id="36016-122">Instalace závislostí aplikací</span><span class="sxs-lookup"><span data-stu-id="36016-122">Install application dependencies</span></span>
<span data-ttu-id="36016-123">Nainstalujte hello knihovny a z ostatních modulů, které potřebujete pro ukázkovou aplikaci hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="36016-123">Install hello libraries and other modules you need for hello sample application by running hello following command:</span></span>

```bash
npm install
```

## <a name="configure-hello-device-connection"></a><span data-ttu-id="36016-124">Nakonfigurujte připojení zařízení hello</span><span class="sxs-lookup"><span data-stu-id="36016-124">Configure hello device connection</span></span>
<span data-ttu-id="36016-125">tooconfigure hello připojení zařízení, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="36016-125">tooconfigure hello device connection, follow these steps:</span></span>

1. <span data-ttu-id="36016-126">Získejte hello sériového portu zařízení hello s hello zařízení zjišťování rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="36016-126">Obtain hello serial port of hello device with hello device discovery cli:</span></span>

   ```bash
   devdisco list --usb
   ```

   <span data-ttu-id="36016-127">Měli byste zobrazený výstup, který je podobný následující toohello a najít hello usb COM port pro panel Arduino: ![zjišťování zařízení][device-discovery]</span><span class="sxs-lookup"><span data-stu-id="36016-127">You should see an output that is similar toohello following and find hello usb COM port for your Arduino board: ![Device discovery][device-discovery]</span></span>

2. <span data-ttu-id="36016-128">Soubor otevřete hello `config.json` v hello lekce složky a přidat hello hodnotu hello najít číslo portu COM:</span><span class="sxs-lookup"><span data-stu-id="36016-128">Open hello file `config.json` in hello lesson folder and add hello value of hello found COM port number:</span></span>

   ```json
   {
       "device_port" : "COM1"
   }
   ```
   ![Config.JSON][config-json]
   > [!NOTE]
   > <span data-ttu-id="36016-130">Pro port hello COM na platformě Windows má formát hello `COM1, COM2, ...`.</span><span class="sxs-lookup"><span data-stu-id="36016-130">For hello COM port, on Windows platform, it has hello format of `COM1, COM2, ...`.</span></span> <span data-ttu-id="36016-131">V systému macOS nebo Ubuntu začíná `/dev/`.</span><span class="sxs-lookup"><span data-stu-id="36016-131">On macOS or Ubuntu, it starts with `/dev/`.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="36016-132">Nasazení a spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="36016-132">Deploy and run hello sample application</span></span>
### <a name="install-hello-required-tools-for-your-arduino-board"></a><span data-ttu-id="36016-133">Instalace nástrojů pro hello požadované pro panel Arduino</span><span class="sxs-lookup"><span data-stu-id="36016-133">Install hello required tools for your Arduino board</span></span>

<span data-ttu-id="36016-134">Nainstalujte hello sady SDK Azure IoT Hub pro panel Arduino spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="36016-134">Install hello Azure IoT Hub SDK for your Arduino board by running hello following command:</span></span>

```bash
gulp install-tools
```

<span data-ttu-id="36016-135">Tato úloha může trvat dlouhou dobu toocomplete, v závislosti na připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="36016-135">This task might take a long time toocomplete, depending on your network connection.</span></span>

> [!NOTE]
> <span data-ttu-id="36016-136">Ukončete prosím hello spuštěna instance Arduino IDE při spuštění úlohy gulp: `install-tools`, `run`.</span><span class="sxs-lookup"><span data-stu-id="36016-136">Please exit hello running Arduino IDE instance when running gulp tasks: `install-tools`, `run`.</span></span>

### <a name="deploy-and-run-hello-sample-app"></a><span data-ttu-id="36016-137">Nasazení a spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="36016-137">Deploy and run hello sample app</span></span>
<span data-ttu-id="36016-138">Nasazení a spuštění ukázkové aplikace hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="36016-138">Deploy and run hello sample application by running hello following command:</span></span>

```bash
gulp run

# You can monitor hello serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

### <a name="verify-hello-app-works"></a><span data-ttu-id="36016-139">Ověřte hello aplikace funguje</span><span class="sxs-lookup"><span data-stu-id="36016-139">Verify hello app works</span></span>
<span data-ttu-id="36016-140">Pokud nevidíte hello DIODU blikat, přečtěte si téma hello [Průvodce odstraňováním potíží s] [ troubleshooting-page] řešení toocommon problémů.</span><span class="sxs-lookup"><span data-stu-id="36016-140">If you don’t see hello LED blinking, see hello [troubleshooting guide][troubleshooting-page] for solutions toocommon problems.</span></span>

![Indikátor blikat][led-blinking]

## <a name="summary"></a><span data-ttu-id="36016-142">Souhrn</span><span class="sxs-lookup"><span data-stu-id="36016-142">Summary</span></span>
<span data-ttu-id="36016-143">Nainstalujete hello požadované nástroje toowork s panel Arduino a nasadit ukázkové aplikace tooyour Arduino Tabule tooblink hello Indikátor.</span><span class="sxs-lookup"><span data-stu-id="36016-143">You've installed hello required tools toowork with your Arduino board and deployed a sample application tooyour Arduino board tooblink hello LED.</span></span> <span data-ttu-id="36016-144">Teď můžete vytvořit, nasadit a spusťte jinou ukázkovou aplikaci, která se připojí vaše Arduino Tabule tooAzure toosend IoT Hub a příjem zpráv.</span><span class="sxs-lookup"><span data-stu-id="36016-144">You can now create, deploy, and run another sample application that connects your Arduino board tooAzure IoT Hub toosend and receive messages.</span></span>

## <a name="next-steps"></a><span data-ttu-id="36016-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="36016-145">Next steps</span></span>
<span data-ttu-id="36016-146">[Získat hello Azure nástroje][get-the-azure-tools]</span><span class="sxs-lookup"><span data-stu-id="36016-146">[Get hello Azure tools][get-the-azure-tools]</span></span>

<!-- Images and links -->

[troubleshooting-page]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[configure-your-device]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-blink-arduino-mac.png
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[led-blinking]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/led_blinking.png
[get-the-azure-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md