---
title: "Simulované zařízení & brány Azure IoT - Lekce 3: spuštění ukázkové aplikace | Microsoft Docs"
description: "Spustit rozbočovači simulované zařízení ukázkové aplikace toosend teploty data tooyour IoT"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: toocloud dat
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
ms.openlocfilehash: bc2c97919e95e4e3977a8b6ac75162bf2b5017be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-run-a-simulated-device-sample-app"></a><span data-ttu-id="98896-104">Nakonfigurujte a spusťte ukázkovou aplikaci simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="98896-104">Configure and run a simulated device sample app</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="98896-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="98896-105">What you will do</span></span>

- <span data-ttu-id="98896-106">Klon hello Ukázka úložiště.</span><span class="sxs-lookup"><span data-stu-id="98896-106">Clone hello sample repository.</span></span>
- <span data-ttu-id="98896-107">Použijte tooget hello rozhraní příkazového řádku Azure IoT hub a informace o logické zařízení pro ukázkovou aplikaci simulovaného zařízení.</span><span class="sxs-lookup"><span data-stu-id="98896-107">Use hello Azure CLI tooget your IoT hub and logical device information for simulated device sample application.</span></span> <span data-ttu-id="98896-108">Nakonfigurujte a spusťte ukázkové aplikace hello simulované zařízení.</span><span class="sxs-lookup"><span data-stu-id="98896-108">Configure and run hello simulated device sample application.</span></span>

<span data-ttu-id="98896-109">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="98896-109">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="98896-110">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="98896-110">What you will learn</span></span>

<span data-ttu-id="98896-111">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="98896-111">In this article, you will learn:</span></span>

- <span data-ttu-id="98896-112">Jak tooconfigure a spuštění hello simulované zařízení ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="98896-112">How tooconfigure and run hello simulated device sample application.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="98896-113">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="98896-113">What you need</span></span>

<span data-ttu-id="98896-114">Musí byl úspěšně dokončen</span><span class="sxs-lookup"><span data-stu-id="98896-114">You must have successfully completed</span></span>

- [<span data-ttu-id="98896-115">Vytvoření služby IoT Hub a registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="98896-115">Create an IoT hub and register your device</span></span>](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)

## <a name="clone-hello-sample-repository-toohello-host-computer"></a><span data-ttu-id="98896-116">Klon hello Ukázka úložiště toohello hostitelském počítači</span><span class="sxs-lookup"><span data-stu-id="98896-116">Clone hello sample repository toohello host computer</span></span>

<span data-ttu-id="98896-117">tooclone hello Ukázka úložiště, postupujte podle těchto kroků v hostitelském počítači hello:</span><span class="sxs-lookup"><span data-stu-id="98896-117">tooclone hello sample repository, follow these steps on hello host computer:</span></span>

1. <span data-ttu-id="98896-118">Otevřete příkazový řádek v systému Windows nebo otevřete terminál v systému macOS nebo Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="98896-118">Open a Command Prompt in Windows or open a terminal in macOS or Ubuntu.</span></span>
2. <span data-ttu-id="98896-119">Spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="98896-119">Run hello following commands:</span></span>

   ```bash
   git clone https://github.com/Azure-samples/iot-hub-c-intel-nuc-gateway-getting-started
   cd iot-hub-c-intel-nuc-gateway-getting-started
   ```

## <a name="configure-hello-simulated-device-and-your-nuc"></a><span data-ttu-id="98896-120">Konfigurace hello simulované zařízení a vaše NUC</span><span class="sxs-lookup"><span data-stu-id="98896-120">Configure hello simulated device and your NUC</span></span>

1. <span data-ttu-id="98896-121">Konfigurační soubor otevřete hello `config.json` ve Visual Studio Code spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="98896-121">Open hello configuration file `config.json` in Visual Studio Code by running hello following command:</span></span>

   ```bash
   code config.json
   ```

2. <span data-ttu-id="98896-122">Nahraďte `"has_sensortag": true` s`"has_sensortag": false`</span><span class="sxs-lookup"><span data-stu-id="98896-122">Replace `"has_sensortag": true` with `"has_sensortag": false`</span></span>

   ![Nemáte zařízení TI SensorTag konfigurace](media/iot-hub-gateway-kit-lessons/lesson3/config_no_sensortag.png)

3. <span data-ttu-id="98896-124">Inicializovat konfigurační soubor hello spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="98896-124">Initialize hello configuration file by running hello following commands:</span></span>

   ```bash
   cd Lesson3
   npm install
   gulp init
   ```

4. <span data-ttu-id="98896-125">Otevřete `config-gateway.json` ve Visual Studio Code spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="98896-125">Open `config-gateway.json` in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-gateway.json
   ```

5. <span data-ttu-id="98896-126">Vyhledejte následující řádek kódu hello a nahraďte `[device hostname or IP address]` se IP adresa nebo název hostitele hello Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="98896-126">Locate hello following line of code and replace `[device hostname or IP address]` with IP address or host name of hello Intel NUC.</span></span>
   <span data-ttu-id="98896-127">![snímek obrazovky konfigurace brány](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)</span><span class="sxs-lookup"><span data-stu-id="98896-127">![screenshot of config gateway](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)</span></span>

## <a name="get-hello-connection-string-of-your-iot-hub-logical-device"></a><span data-ttu-id="98896-128">Získat připojovací řetězec hello logické zařízení IoT hub</span><span class="sxs-lookup"><span data-stu-id="98896-128">Get hello connection string of your IoT hub logical device</span></span>

<span data-ttu-id="98896-129">tooget hello Azure IoT hub připojovací řetězec logické zařízení, spusťte následující příkaz v hostitelském počítači hello hello:</span><span class="sxs-lookup"><span data-stu-id="98896-129">tooget hello Azure IoT hub connection string of your logical device, run hello following command on hello host computer:</span></span>

```bash
az iot device show-connection-string --hub-name {IoT hub name} --device-id mydevice --resource-group iot-gateway
```

<span data-ttu-id="98896-130">`{IoT hub name}`je název centra IoT hello, který jste použili.</span><span class="sxs-lookup"><span data-stu-id="98896-130">`{IoT hub name}` is hello IoT hub name that you used.</span></span> <span data-ttu-id="98896-131">Použít iot brány jako hodnotu hello `{resource group name}` a použijte mydevice jako hodnota hello `{device id}` Pokud hodnota hello v lekci 2 nebyla změněna.</span><span class="sxs-lookup"><span data-stu-id="98896-131">Use iot-gateway as hello value of `{resource group name}` and use mydevice as hello value of `{device id}` if you didn't change hello value in Lesson 2.</span></span>

## <a name="configure-hello-simulated-device-cloud-upload-sample-application"></a><span data-ttu-id="98896-132">Konfigurace hello simulované zařízení cloudu nahrávání ukázkovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="98896-132">Configure hello simulated device cloud upload sample application</span></span>

<span data-ttu-id="98896-133">tooconfigure a spuštění hello simulované zařízení cloud nahrát ukázková aplikace, postupujte podle těchto kroků v hostitelském počítači hello:</span><span class="sxs-lookup"><span data-stu-id="98896-133">tooconfigure and run hello simulated device cloud upload sample application, follow these steps on hello host computer:</span></span>

1. <span data-ttu-id="98896-134">Otevřete `config-sensortag.json` ve Visual Studio Code spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="98896-134">Open `config-sensortag.json` in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-sensortag.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-sensortag.json
   ```

   ![snímek obrazovky konfigurace sensortag](media/iot-hub-gateway-kit-lessons/lesson3/config_simulated_device.png)

2. <span data-ttu-id="98896-136">Ujistěte se, hello následující nahrazení v hello kódu:</span><span class="sxs-lookup"><span data-stu-id="98896-136">Make hello following replacements in hello code:</span></span>
   - <span data-ttu-id="98896-137">Nahraďte `[IoT hub name]` názvem hello IoT hub.</span><span class="sxs-lookup"><span data-stu-id="98896-137">Replace `[IoT hub name]` with hello IoT hub name.</span></span>
   - <span data-ttu-id="98896-138">Nahraďte `[IoT device connection string]` s hello připojovací řetězec logické zařízení IoT hub.</span><span class="sxs-lookup"><span data-stu-id="98896-138">Replace `[IoT device connection string]` with hello connection string of your IoT hub logical device.</span></span>

3. <span data-ttu-id="98896-139">Spusťte aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="98896-139">Run hello application.</span></span>

   <span data-ttu-id="98896-140">Nasazení a spuštění aplikace hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="98896-140">Deploy and run hello application by running hello following command:</span></span>

   ```bash
   gulp run
   ```

## <a name="verify-hello-sample-application-works"></a><span data-ttu-id="98896-141">Ověřte hello ukázkové aplikace funguje</span><span class="sxs-lookup"><span data-stu-id="98896-141">Verify hello sample application works</span></span>

<span data-ttu-id="98896-142">Teď byste měli vidět výstup podobný hello následující:</span><span class="sxs-lookup"><span data-stu-id="98896-142">You should now see output like hello following:</span></span>

![výstupu ukázkové aplikace simulovaného zařízení](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_simudev.png)

<span data-ttu-id="98896-144">aplikace Hello odešle teploty data tooyour IoT hub, která trvá po dobu 40 sekund.</span><span class="sxs-lookup"><span data-stu-id="98896-144">hello application sends temperature data tooyour IoT hub, which lasts for 40 seconds.</span></span>

## <a name="summary"></a><span data-ttu-id="98896-145">Souhrn</span><span class="sxs-lookup"><span data-stu-id="98896-145">Summary</span></span>

<span data-ttu-id="98896-146">Úspěšně jste nakonfigurovali a spuštění hello simulované zařízení cloudu nahrávání ukázkovou aplikaci, který odesílá data tooyour IoT hub s simulované zařízení.</span><span class="sxs-lookup"><span data-stu-id="98896-146">You've successfully configured and run hello simulated device cloud upload sample application which sends data tooyour IoT hub with simulated device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="98896-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="98896-147">Next steps</span></span>
[<span data-ttu-id="98896-148">Čtení zpráv ze služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="98896-148">Read messages from your IoT hub</span></span>](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md)