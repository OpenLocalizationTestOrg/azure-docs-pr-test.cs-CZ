---
title: "Arduino připojit k Azure IoT - Lekce 1: nasazení aplikace | Microsoft Docs"
description: "Naklonujte ukázkovou aplikaci Arduino z Githubu a spusťte gulp k nasazení této aplikace na vašem Adafruit prolnutí M0 Wi-Fi. Tato ukázková aplikace bliká GPIO"
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
ms.openlocfilehash: 4431808ac6182d194e841c087c8f89f1a12b1911
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-deploy-the-blink-application"></a><span data-ttu-id="901c2-105">Vytvoření a nasazení aplikace blink</span><span class="sxs-lookup"><span data-stu-id="901c2-105">Create and deploy the blink application</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="901c2-106">Co provedete</span><span class="sxs-lookup"><span data-stu-id="901c2-106">What you will do</span></span>
<span data-ttu-id="901c2-107">Naklonujte ukázkovou aplikaci Arduino z Githubu a pomocí nástroje gulp nasadit ukázkovou aplikaci pro panel Adafruit prolnutí M0 Wi-Fi Arduino.</span><span class="sxs-lookup"><span data-stu-id="901c2-107">Clone the sample Arduino application from GitHub, and use the gulp tool to deploy the sample application to your Adafruit Feather M0 WiFi Arduino board.</span></span> <span data-ttu-id="901c2-108">Aplikace bliká ukázka GPIO č. 13 na barod VEDLA každé dvě sekundy.</span><span class="sxs-lookup"><span data-stu-id="901c2-108">The sample application blinks the GPIO #13 on-barod LED every two seconds.</span></span>

<span data-ttu-id="901c2-109">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting-page].</span><span class="sxs-lookup"><span data-stu-id="901c2-109">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting-page].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="901c2-110">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="901c2-110">What you will learn</span></span>
* <span data-ttu-id="901c2-111">Postup nasazení a spuštění ukázkové aplikace na panel Arduino.</span><span class="sxs-lookup"><span data-stu-id="901c2-111">How to deploy and run the sample application on your Arduino board.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="901c2-112">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="901c2-112">What you need</span></span>
<span data-ttu-id="901c2-113">Musíte úspěšně jste dokončili následující operace:</span><span class="sxs-lookup"><span data-stu-id="901c2-113">You must have successfully completed the following operations:</span></span>

* <span data-ttu-id="901c2-114">[Konfigurace zařízení][configure-your-device]</span><span class="sxs-lookup"><span data-stu-id="901c2-114">[Configure your device][configure-your-device]</span></span>
* <span data-ttu-id="901c2-115">[Získat nástroje][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="901c2-115">[Get the tools][get-the-tools]</span></span>

## <a name="open-the-sample-application"></a><span data-ttu-id="901c2-116">Otevřete ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="901c2-116">Open the sample application</span></span>
<span data-ttu-id="901c2-117">Chcete-li otevřít ukázkové aplikace, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="901c2-117">To open the sample application, follow these steps:</span></span>

1. <span data-ttu-id="901c2-118">Naklonujte úložiště ukázkové z Githubu spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="901c2-118">Clone the sample repository from GitHub by running the following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-feather-m0-getting-started.git
   ```
2. <span data-ttu-id="901c2-119">Otevřete ukázkovou aplikaci v aplikaci Visual Studio Code spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="901c2-119">Open the sample application in Visual Studio Code by running the following commands:</span></span>

   ```bash
   cd iot-hub-c-feather-m0-getting-started
   cd Lesson1
   code .
   ```

   ![Struktura úložiště][repo-structure]

<span data-ttu-id="901c2-121">`app.ino` v soubor `app` podsložky je klíče zdrojového souboru, který obsahuje kód, který ovládacího prvku Indikátor.</span><span class="sxs-lookup"><span data-stu-id="901c2-121">The `app.ino` file in the `app` subfolder is the key source file that contains the code to control the LED.</span></span>

### <a name="install-application-dependencies"></a><span data-ttu-id="901c2-122">Instalace závislostí aplikací</span><span class="sxs-lookup"><span data-stu-id="901c2-122">Install application dependencies</span></span>
<span data-ttu-id="901c2-123">Nainstalujte v knihovnách a dalších modulů, které potřebujete pro ukázkovou aplikaci tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="901c2-123">Install the libraries and other modules you need for the sample application by running the following command:</span></span>

```bash
npm install
```

## <a name="configure-the-device-connection"></a><span data-ttu-id="901c2-124">Nakonfigurujte připojení zařízení</span><span class="sxs-lookup"><span data-stu-id="901c2-124">Configure the device connection</span></span>
<span data-ttu-id="901c2-125">Konfigurace připojení zařízení, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="901c2-125">To configure the device connection, follow these steps:</span></span>

1. <span data-ttu-id="901c2-126">Získání sériového portu zařízení pomocí rozhraní příkazového řádku zjišťování zařízení:</span><span class="sxs-lookup"><span data-stu-id="901c2-126">Obtain the serial port of the device with the device discovery cli:</span></span>

   ```bash
   devdisco list --usb
   ```

   <span data-ttu-id="901c2-127">Měli byste zobrazený výstup, který je podobný následujícímu a najít usb COM port pro panel Arduino: ![zjišťování zařízení][device-discovery]</span><span class="sxs-lookup"><span data-stu-id="901c2-127">You should see an output that is similar to the following and find the usb COM port for your Arduino board: ![Device discovery][device-discovery]</span></span>

2. <span data-ttu-id="901c2-128">Otevřete soubor `config.json` ve složce lekce a přidejte hodnotu nalezen číslo portu COM:</span><span class="sxs-lookup"><span data-stu-id="901c2-128">Open the file `config.json` in the lesson folder and add the value of the found COM port number:</span></span>

   ```json
   {
       "device_port" : "COM1"
   }
   ```
   ![Config.JSON][config-json]
   > [!NOTE]
   > <span data-ttu-id="901c2-130">Pro port COM, na platformě Windows má formát `COM1, COM2, ...`.</span><span class="sxs-lookup"><span data-stu-id="901c2-130">For the COM port, on Windows platform, it has the format of `COM1, COM2, ...`.</span></span> <span data-ttu-id="901c2-131">V systému macOS nebo Ubuntu začíná `/dev/`.</span><span class="sxs-lookup"><span data-stu-id="901c2-131">On macOS or Ubuntu, it starts with `/dev/`.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="901c2-132">Nasazení a spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="901c2-132">Deploy and run the sample application</span></span>
### <a name="install-the-required-tools-for-your-arduino-board"></a><span data-ttu-id="901c2-133">Nainstalujte požadované nástroje pro panel Arduino</span><span class="sxs-lookup"><span data-stu-id="901c2-133">Install the required tools for your Arduino board</span></span>

<span data-ttu-id="901c2-134">Nainstalujte sadu SDK Azure IoT Hub pro panel Arduino spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="901c2-134">Install the Azure IoT Hub SDK for your Arduino board by running the following command:</span></span>

```bash
gulp install-tools
```

<span data-ttu-id="901c2-135">Tato úloha může trvat dlouhou dobu v závislosti na připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="901c2-135">This task might take a long time to complete, depending on your network connection.</span></span>

> [!NOTE]
> <span data-ttu-id="901c2-136">Při spuštění úlohy gulp prosím Ukončete spuštěnou instanci Arduino IDE: `install-tools`, `run`.</span><span class="sxs-lookup"><span data-stu-id="901c2-136">Please exit the running Arduino IDE instance when running gulp tasks: `install-tools`, `run`.</span></span>

### <a name="deploy-and-run-the-sample-app"></a><span data-ttu-id="901c2-137">Nasazení a spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="901c2-137">Deploy and run the sample app</span></span>
<span data-ttu-id="901c2-138">Nasazení a spuštění ukázkové aplikace tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="901c2-138">Deploy and run the sample application by running the following command:</span></span>

```bash
gulp run

# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

### <a name="verify-the-app-works"></a><span data-ttu-id="901c2-139">Ověřte aplikace funguje</span><span class="sxs-lookup"><span data-stu-id="901c2-139">Verify the app works</span></span>
<span data-ttu-id="901c2-140">Pokud nevidíte DIODU blikat, najdete v článku [Průvodce odstraňováním potíží s] [ troubleshooting-page] pro řešení běžných potíží.</span><span class="sxs-lookup"><span data-stu-id="901c2-140">If you don’t see the LED blinking, see the [troubleshooting guide][troubleshooting-page] for solutions to common problems.</span></span>

![Indikátor blikat][led-blinking]

## <a name="summary"></a><span data-ttu-id="901c2-142">Souhrn</span><span class="sxs-lookup"><span data-stu-id="901c2-142">Summary</span></span>
<span data-ttu-id="901c2-143">Nainstalujete požadované nástroje pro práci s panel Arduino a nasadit ukázkovou aplikaci pro panel Arduino blikání Indikátor.</span><span class="sxs-lookup"><span data-stu-id="901c2-143">You've installed the required tools to work with your Arduino board and deployed a sample application to your Arduino board to blink the LED.</span></span> <span data-ttu-id="901c2-144">Teď můžete vytvořit, nasadit a spustit jinou ukázkovou aplikaci, která panel Arduino se připojuje ke službě Azure IoT Hub odesílat a přijímat zprávy.</span><span class="sxs-lookup"><span data-stu-id="901c2-144">You can now create, deploy, and run another sample application that connects your Arduino board to Azure IoT Hub to send and receive messages.</span></span>

## <a name="next-steps"></a><span data-ttu-id="901c2-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="901c2-145">Next steps</span></span>
<span data-ttu-id="901c2-146">[Získat nástroje Azure][get-the-azure-tools]</span><span class="sxs-lookup"><span data-stu-id="901c2-146">[Get the Azure tools][get-the-azure-tools]</span></span>

<!-- Images and links -->

[troubleshooting-page]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[configure-your-device]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-blink-arduino-mac.png
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[led-blinking]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/led_blinking.png
[get-the-azure-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md