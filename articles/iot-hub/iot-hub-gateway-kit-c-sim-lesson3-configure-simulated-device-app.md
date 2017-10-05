---
title: "Simulované zařízení & brány Azure IoT - Lekce 3: spuštění ukázkové aplikace | Microsoft Docs"
description: "Spuštění ukázkové aplikace simulovaného zařízení k odesílání dat teploty do služby IoT hub"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: data do cloudu
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 5d051d99-9749-4150-b3c8-573b0bda9c52
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 7df2d730c38a9f715e0fd57b4d436724a5727760
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-and-run-a-simulated-device-sample-app"></a><span data-ttu-id="45c89-104">Nakonfigurujte a spusťte ukázkovou aplikaci simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="45c89-104">Configure and run a simulated device sample app</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="45c89-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="45c89-105">What you will do</span></span>

- <span data-ttu-id="45c89-106">Naklonujte úložiště ukázka.</span><span class="sxs-lookup"><span data-stu-id="45c89-106">Clone the sample repository.</span></span>
- <span data-ttu-id="45c89-107">Použijte rozhraní příkazového řádku Azure k získání IoT hub a informace o logické zařízení pro ukázkovou aplikaci simulovaného zařízení.</span><span class="sxs-lookup"><span data-stu-id="45c89-107">Use the Azure CLI to get your IoT hub and logical device information for simulated device sample application.</span></span> <span data-ttu-id="45c89-108">Nakonfigurujte a spusťte ukázkovou aplikaci simulovaného zařízení.</span><span class="sxs-lookup"><span data-stu-id="45c89-108">Configure and run the simulated device sample application.</span></span>

<span data-ttu-id="45c89-109">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="45c89-109">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="45c89-110">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="45c89-110">What you will learn</span></span>

<span data-ttu-id="45c89-111">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="45c89-111">In this article, you will learn:</span></span>

- <span data-ttu-id="45c89-112">Postup konfigurace a pak spusťte ukázkovou aplikaci simulovaného zařízení.</span><span class="sxs-lookup"><span data-stu-id="45c89-112">How to configure and run the simulated device sample application.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="45c89-113">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="45c89-113">What you need</span></span>

<span data-ttu-id="45c89-114">Musí byl úspěšně dokončen</span><span class="sxs-lookup"><span data-stu-id="45c89-114">You must have successfully completed</span></span>

- [<span data-ttu-id="45c89-115">Vytvoření služby IoT Hub a registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="45c89-115">Create an IoT hub and register your device</span></span>](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)

## <a name="clone-the-sample-repository-to-the-host-computer"></a><span data-ttu-id="45c89-116">Naklonujte úložiště ukázkové k hostitelskému počítači</span><span class="sxs-lookup"><span data-stu-id="45c89-116">Clone the sample repository to the host computer</span></span>

<span data-ttu-id="45c89-117">Klonovat úložiště ukázkové, postupujte takto na hostitelském počítači:</span><span class="sxs-lookup"><span data-stu-id="45c89-117">To clone the sample repository, follow these steps on the host computer:</span></span>

1. <span data-ttu-id="45c89-118">Otevřete příkazový řádek v systému Windows nebo otevřete terminál v systému macOS nebo Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="45c89-118">Open a Command Prompt in Windows or open a terminal in macOS or Ubuntu.</span></span>
2. <span data-ttu-id="45c89-119">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="45c89-119">Run the following commands:</span></span>

   ```bash
   git clone https://github.com/Azure-samples/iot-hub-c-intel-nuc-gateway-getting-started
   cd iot-hub-c-intel-nuc-gateway-getting-started
   ```

## <a name="configure-the-simulated-device-and-your-nuc"></a><span data-ttu-id="45c89-120">Konfigurace simulované zařízení a vaše NUC</span><span class="sxs-lookup"><span data-stu-id="45c89-120">Configure the simulated device and your NUC</span></span>

1. <span data-ttu-id="45c89-121">Otevřete konfigurační soubor `config.json` ve Visual Studio Code spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="45c89-121">Open the configuration file `config.json` in Visual Studio Code by running the following command:</span></span>

   ```bash
   code config.json
   ```

2. <span data-ttu-id="45c89-122">Nahraďte `"has_sensortag": true` s`"has_sensortag": false`</span><span class="sxs-lookup"><span data-stu-id="45c89-122">Replace `"has_sensortag": true` with `"has_sensortag": false`</span></span>

   ![Nemáte zařízení TI SensorTag konfigurace](media/iot-hub-gateway-kit-lessons/lesson3/config_no_sensortag.png)

3. <span data-ttu-id="45c89-124">Inicializace konfiguračního souboru spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="45c89-124">Initialize the configuration file by running the following commands:</span></span>

   ```bash
   cd Lesson3
   npm install
   gulp init
   ```

4. <span data-ttu-id="45c89-125">Otevřete `config-gateway.json` ve Visual Studio Code spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="45c89-125">Open `config-gateway.json` in Visual Studio Code by running the following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-gateway.json
   ```

5. <span data-ttu-id="45c89-126">Vyhledejte následující řádek kódu a nahraďte `[device hostname or IP address]` se IP adresa nebo název hostitele Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="45c89-126">Locate the following line of code and replace `[device hostname or IP address]` with IP address or host name of the Intel NUC.</span></span>
   <span data-ttu-id="45c89-127">![snímek obrazovky konfigurace brány](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)</span><span class="sxs-lookup"><span data-stu-id="45c89-127">![screenshot of config gateway](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)</span></span>

## <a name="get-the-connection-string-of-your-iot-hub-logical-device"></a><span data-ttu-id="45c89-128">Získat připojovací řetězec logické zařízení IoT hub</span><span class="sxs-lookup"><span data-stu-id="45c89-128">Get the connection string of your IoT hub logical device</span></span>

<span data-ttu-id="45c89-129">Chcete-li získat Azure IoT hub připojovací řetězec logického zařízení, spusťte následující příkaz na hostitelském počítači:</span><span class="sxs-lookup"><span data-stu-id="45c89-129">To get the Azure IoT hub connection string of your logical device, run the following command on the host computer:</span></span>

```bash
az iot device show-connection-string --hub-name {IoT hub name} --device-id mydevice --resource-group iot-gateway
```

<span data-ttu-id="45c89-130">`{IoT hub name}`je název centra IoT, které jste použili.</span><span class="sxs-lookup"><span data-stu-id="45c89-130">`{IoT hub name}` is the IoT hub name that you used.</span></span> <span data-ttu-id="45c89-131">Použít iot brány jako hodnotu `{resource group name}` a použít jako hodnotu mydevice `{device id}` Pokud nebylo změňte hodnotu v lekci 2.</span><span class="sxs-lookup"><span data-stu-id="45c89-131">Use iot-gateway as the value of `{resource group name}` and use mydevice as the value of `{device id}` if you didn't change the value in Lesson 2.</span></span>

## <a name="configure-the-simulated-device-cloud-upload-sample-application"></a><span data-ttu-id="45c89-132">Nakonfigurujte ukázkovou aplikaci simulovaného zařízení cloudu nahrávání</span><span class="sxs-lookup"><span data-stu-id="45c89-132">Configure the simulated device cloud upload sample application</span></span>

<span data-ttu-id="45c89-133">Pro konfiguraci a spuštění ukázkové aplikace simulovaného zařízení cloudu nahrávání, postupujte podle těchto kroků na hostitelském počítači:</span><span class="sxs-lookup"><span data-stu-id="45c89-133">To configure and run the simulated device cloud upload sample application, follow these steps on the host computer:</span></span>

1. <span data-ttu-id="45c89-134">Otevřete `config-sensortag.json` ve Visual Studio Code spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="45c89-134">Open `config-sensortag.json` in Visual Studio Code by running the following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-sensortag.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-sensortag.json
   ```

   ![snímek obrazovky konfigurace sensortag](media/iot-hub-gateway-kit-lessons/lesson3/config_simulated_device.png)

2. <span data-ttu-id="45c89-136">Zkontrolujte následující nahrazení v kódu:</span><span class="sxs-lookup"><span data-stu-id="45c89-136">Make the following replacements in the code:</span></span>
   - <span data-ttu-id="45c89-137">Nahraďte `[IoT hub name]` názvem IoT hub.</span><span class="sxs-lookup"><span data-stu-id="45c89-137">Replace `[IoT hub name]` with the IoT hub name.</span></span>
   - <span data-ttu-id="45c89-138">Nahraďte `[IoT device connection string]` připojovacím řetězcem logické zařízení IoT hub.</span><span class="sxs-lookup"><span data-stu-id="45c89-138">Replace `[IoT device connection string]` with the connection string of your IoT hub logical device.</span></span>

3. <span data-ttu-id="45c89-139">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="45c89-139">Run the application.</span></span>

   <span data-ttu-id="45c89-140">Nasazení a spuštění aplikace tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="45c89-140">Deploy and run the application by running the following command:</span></span>

   ```bash
   gulp run
   ```

## <a name="verify-the-sample-application-works"></a><span data-ttu-id="45c89-141">Ověřte ukázkové aplikace pracuje</span><span class="sxs-lookup"><span data-stu-id="45c89-141">Verify the sample application works</span></span>

<span data-ttu-id="45c89-142">Teď byste měli vidět výstup podobný tomuto:</span><span class="sxs-lookup"><span data-stu-id="45c89-142">You should now see output like the following:</span></span>

![výstupu ukázkové aplikace simulovaného zařízení](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_simudev.png)

<span data-ttu-id="45c89-144">Aplikace odešle teploty data do služby IoT hub, která trvá po dobu 40 sekund.</span><span class="sxs-lookup"><span data-stu-id="45c89-144">The application sends temperature data to your IoT hub, which lasts for 40 seconds.</span></span>

## <a name="summary"></a><span data-ttu-id="45c89-145">Souhrn</span><span class="sxs-lookup"><span data-stu-id="45c89-145">Summary</span></span>

<span data-ttu-id="45c89-146">Úspěšně jste nakonfigurované a spusťte simulované zařízení cloudu nahrávání ukázkové aplikace, který odesílá data do služby IoT hub s simulované zařízení.</span><span class="sxs-lookup"><span data-stu-id="45c89-146">You've successfully configured and run the simulated device cloud upload sample application which sends data to your IoT hub with simulated device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="45c89-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="45c89-147">Next steps</span></span>
[<span data-ttu-id="45c89-148">Čtení zpráv ze služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="45c89-148">Read messages from your IoT hub</span></span>](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md)