---
title: "Zařízení SensorTag & brány Azure IoT - Lekce 3: spuštění ukázkové aplikace | Microsoft Docs"
description: "Spusťte zakázat ukázková aplikace tooreceive data z SensorTag zakázat a ze služby IoT hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Povolit aplikaci, senzor monitorování aplikace, shromažďování dat snímačů, dat ze senzorů, toocloud data snímačů"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: b33e53a1-1df7-4412-ade1-45185aec5bef
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 4a8acdeadd402ffc82d3b766e1ec03a77ddcebb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-run-a-ble-sample-application"></a><span data-ttu-id="61d6f-104">Nakonfigurujte a spusťte ukázkové aplikace zakázat</span><span class="sxs-lookup"><span data-stu-id="61d6f-104">Configure and run a BLE sample application</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="61d6f-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="61d6f-105">What you will do</span></span>

- <span data-ttu-id="61d6f-106">Klon hello Ukázka úložiště.</span><span class="sxs-lookup"><span data-stu-id="61d6f-106">Clone hello sample repository.</span></span> 
- <span data-ttu-id="61d6f-107">Nastavte hello propojení mezi SensorTag a Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="61d6f-107">Set up hello connectivity between SensorTag and Intel NUC.</span></span> 
- <span data-ttu-id="61d6f-108">Použijte tooget hello rozhraní příkazového řádku Azure IoT hub a SensorTag informace pro ukázkovou aplikaci zakázat (Bluetooth nízkou energie).</span><span class="sxs-lookup"><span data-stu-id="61d6f-108">Use hello Azure CLI tooget your IoT hub and SensorTag information for a BLE(Bluetooth Low Energy) sample application.</span></span> <span data-ttu-id="61d6f-109">A nakonfigurujte a spusťte hello zakázat ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="61d6f-109">And configure and run hello BLE sample application.</span></span> 

<span data-ttu-id="61d6f-110">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="61d6f-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="61d6f-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="61d6f-111">What you will learn</span></span>

<span data-ttu-id="61d6f-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="61d6f-112">In this article, you will learn:</span></span>

- <span data-ttu-id="61d6f-113">Jak tooconfigure a spuštění hello zakázat ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="61d6f-113">How tooconfigure and run hello BLE sample application.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="61d6f-114">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="61d6f-114">What you need</span></span>

<span data-ttu-id="61d6f-115">Musí byl úspěšně dokončen</span><span class="sxs-lookup"><span data-stu-id="61d6f-115">You must have successfully completed</span></span>

- [<span data-ttu-id="61d6f-116">Vytvoření služby IoT hub a zaregistrujte SensorTag</span><span class="sxs-lookup"><span data-stu-id="61d6f-116">Create an IoT hub and register SensorTag</span></span>](iot-hub-gateway-kit-c-lesson2-register-device.md)

## <a name="clone-hello-sample-repository-toohello-host-computer"></a><span data-ttu-id="61d6f-117">Klon hello Ukázka úložiště toohello hostitelském počítači</span><span class="sxs-lookup"><span data-stu-id="61d6f-117">Clone hello sample repository toohello host computer</span></span>

<span data-ttu-id="61d6f-118">tooclone hello Ukázka úložiště, postupujte podle těchto kroků v hostitelském počítači hello:</span><span class="sxs-lookup"><span data-stu-id="61d6f-118">tooclone hello sample repository, follow these steps on hello host computer:</span></span>

1. <span data-ttu-id="61d6f-119">Otevřete okno příkazového řádku v systému Windows nebo otevřete terminál v systému macOS nebo Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="61d6f-119">Open a Command Prompt window in Windows or open a terminal in macOS or Ubuntu.</span></span>
2. <span data-ttu-id="61d6f-120">Spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="61d6f-120">Run hello following commands:</span></span>

   ```bash
   git clone https://github.com/Azure-samples/iot-hub-c-intel-nuc-gateway-getting-started
   cd iot-hub-c-intel-nuc-gateway-getting-started
   ```

## <a name="set-up-hello-connectivity-between-sensortag-and-intel-nuc"></a><span data-ttu-id="61d6f-121">Nastavit hello propojení mezi SensorTag a Intel NUC</span><span class="sxs-lookup"><span data-stu-id="61d6f-121">Set up hello connectivity between SensorTag and Intel NUC</span></span>

<span data-ttu-id="61d6f-122">tooset hello připojením pomocí těchto kroků v hostitelském počítači hello:</span><span class="sxs-lookup"><span data-stu-id="61d6f-122">tooset up hello connectivity, follow these steps on hello host computer:</span></span>

1. <span data-ttu-id="61d6f-123">Inicializovat konfigurační soubor hello spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="61d6f-123">Initialize hello configuration file by running hello following commands:</span></span>

   ```bash
   cd Lesson3
   npm install
   gulp init
   ```

2. <span data-ttu-id="61d6f-124">Otevřete `config-gateway.json` ve Visual Studio Code spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="61d6f-124">Open `config-gateway.json` in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-gateway.json
   ```

3. <span data-ttu-id="61d6f-125">Vyhledejte následující řádek kódu hello a nahraďte `[device hostname or IP address]` s hello IP adresu nebo název hostitele Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="61d6f-125">Locate hello following line of code and replace `[device hostname or IP address]` with hello IP address or host name of Intel NUC.</span></span>
   <span data-ttu-id="61d6f-126">![snímek obrazovky konfigurace brány](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)</span><span class="sxs-lookup"><span data-stu-id="61d6f-126">![screenshot of config gateway](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)</span></span>

4. <span data-ttu-id="61d6f-127">Instalace pomocné nástroje na Intel NUC spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="61d6f-127">Install helper tools on Intel NUC by running hello following command:</span></span>

   ```bash
   gulp install-tools
   ```

5. <span data-ttu-id="61d6f-128">Zapněte SensorTag stisknutím tlačítka napájení hello jako hello následující obrázek a měla blikat DIODU hello zelená.</span><span class="sxs-lookup"><span data-stu-id="61d6f-128">Turn on SensorTag by pressing hello power button as hello following picture, and hello green LED should blink.</span></span>

   ![zapnout Sensortag](media/iot-hub-gateway-kit-lessons/lesson3/turn on_off sensortag.jpg)

6. <span data-ttu-id="61d6f-130">Kontrolovat zařízení SensorTag spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="61d6f-130">Scan SensorTag devices by running hello following commands:</span></span>

   ```bash
   gulp discover-sensortag
   ```

7. <span data-ttu-id="61d6f-131">Otestujte připojení hello mezi hello SensorTag a Intel NUC spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="61d6f-131">Test hello connectivity between hello SensorTag and Intel NUC by running hello following command:</span></span>

   ```bash
   gulp test-connectivity --mac {mac address}
   ```

   <span data-ttu-id="61d6f-132">Nahraďte `{mac address}` s hello adresu MAC, který jste získali v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="61d6f-132">Replace `{mac address}` with hello MAC address that you obtained in hello previous step.</span></span>

## <a name="get-hello-connection-string-of-sensortag"></a><span data-ttu-id="61d6f-133">Získat připojovací řetězec hello SensorTag</span><span class="sxs-lookup"><span data-stu-id="61d6f-133">Get hello connection string of SensorTag</span></span>

<span data-ttu-id="61d6f-134">tooget hello Azure IoT hub připojovací řetězec SensorTag, spusťte následující příkaz v hostitelském počítači hello hello:</span><span class="sxs-lookup"><span data-stu-id="61d6f-134">tooget hello Azure IoT hub connection string of SensorTag, run hello following command on hello host computer:</span></span>

```bash
az iot device show-connection-string --hub-name {IoT hub name} --device-id mydevice --resource-group iot-gateway
```

<span data-ttu-id="61d6f-135">`{IoT hub name}`je název centra IoT hello, který jste použili.</span><span class="sxs-lookup"><span data-stu-id="61d6f-135">`{IoT hub name}` is hello IoT hub name that you used.</span></span> <span data-ttu-id="61d6f-136">Použít iot brány jako hodnotu hello `{resource group name}` a použijte mydevice jako hodnota hello `{device id}` Pokud hodnota hello v lekci 2 nebyla změněna.</span><span class="sxs-lookup"><span data-stu-id="61d6f-136">Use iot-gateway as hello value of `{resource group name}` and use mydevice as hello value of `{device id}` if you didn't change hello value in Lesson 2.</span></span>

## <a name="configure-hello-ble-sample-application"></a><span data-ttu-id="61d6f-137">Konfigurace hello zakázat ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="61d6f-137">Configure hello BLE sample application</span></span>

<span data-ttu-id="61d6f-138">tooconfigure a spuštění hello zakázat ukázkovou aplikaci, pomocí těchto kroků v hostitelském počítači hello:</span><span class="sxs-lookup"><span data-stu-id="61d6f-138">tooconfigure and run hello BLE sample application, follow these steps on hello host computer:</span></span>

1. <span data-ttu-id="61d6f-139">Otevřete `config-sensortag.json` ve Visual Studio Code spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="61d6f-139">Open `config-sensortag.json` in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-sensortag.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-sensortag.json
   ```

   ![snímek obrazovky konfigurace sensortag](media/iot-hub-gateway-kit-lessons/lesson3/config_sensortag.png)

2. <span data-ttu-id="61d6f-141">Ujistěte se, hello následující nahrazení v hello kódu:</span><span class="sxs-lookup"><span data-stu-id="61d6f-141">Make hello following replacements in hello code:</span></span>
   - <span data-ttu-id="61d6f-142">Nahraďte `[IoT hub name]` s hello název centra IoT, které jste použili.</span><span class="sxs-lookup"><span data-stu-id="61d6f-142">Replace `[IoT hub name]` with hello IoT hub name that you used.</span></span>
   - <span data-ttu-id="61d6f-143">Nahraďte `[IoT device connection string]` s hello připojovací řetězec SensorTag, který jste získali.</span><span class="sxs-lookup"><span data-stu-id="61d6f-143">Replace `[IoT device connection string]` with hello connection string of SensorTag that you obtained.</span></span>
   - <span data-ttu-id="61d6f-144">Nahraďte `[device_mac_address]` s hello adresu MAC hello SensorTag, který jste získali.</span><span class="sxs-lookup"><span data-stu-id="61d6f-144">Replace `[device_mac_address]` with hello MAC address of hello SensorTag that you obtained.</span></span>

3. <span data-ttu-id="61d6f-145">Spusťte hello zakázat ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="61d6f-145">Run hello BLE sample application.</span></span>

   <span data-ttu-id="61d6f-146">toorun hello zakázat ukázkovou aplikaci, pomocí těchto kroků v hostitelském počítači hello:</span><span class="sxs-lookup"><span data-stu-id="61d6f-146">toorun hello BLE sample application, follow these steps on hello host computer:</span></span>

   1. <span data-ttu-id="61d6f-147">Zapněte SensorTag.</span><span class="sxs-lookup"><span data-stu-id="61d6f-147">Turn on SensorTag.</span></span>

   2. <span data-ttu-id="61d6f-148">Nasazení a spuštění hello zakázat ukázkovou aplikaci na Intel NUC spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="61d6f-148">Deploy and run hello BLE sample application on Intel NUC by running hello following command:</span></span>
   
      ```bash
      gulp run
      ```

## <a name="verify-that-hello-ble-sample-application-works"></a><span data-ttu-id="61d6f-149">Ověřte, že funguje hello zakázat ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="61d6f-149">Verify that hello BLE sample application works</span></span>

<span data-ttu-id="61d6f-150">Teď byste měli vidět výstup jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="61d6f-150">You should now see an output like hello following:</span></span>

![Zakázat ukázkové aplikace výstup](media/iot-hub-gateway-kit-lessons/lesson3/BLE_running.png)

<span data-ttu-id="61d6f-152">Ukázková aplikace Hello udržuje shromažďování dat teploty a ji odeslala tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="61d6f-152">hello sample application keeps collecting temperature data and sent it tooyour IoT hub.</span></span> <span data-ttu-id="61d6f-153">Ukázková aplikace Hello automaticky ukončí po odeslání 40 sekund.</span><span class="sxs-lookup"><span data-stu-id="61d6f-153">hello sample application terminates automatically after sending 40 seconds.</span></span>

## <a name="summary"></a><span data-ttu-id="61d6f-154">Souhrn</span><span class="sxs-lookup"><span data-stu-id="61d6f-154">Summary</span></span>

<span data-ttu-id="61d6f-155">Úspěšně jste nastavit hello propojení mezi SensorTag a Intel NUC a spusťte zakázat ukázkovou aplikaci, která shromažďuje a odesílá data ze služby IoT hub SensorTag tooyour.</span><span class="sxs-lookup"><span data-stu-id="61d6f-155">You've successfully set up hello connectivity between SensorTag and Intel NUC, and run a BLE sample application which collects and sends data from SensorTag tooyour IoT hub.</span></span> <span data-ttu-id="61d6f-156">Jste připravené toolearn jak tooverify, který přijal služby IoT hub hello data.</span><span class="sxs-lookup"><span data-stu-id="61d6f-156">You're ready toolearn how tooverify that your IoT hub has received hello data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="61d6f-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="61d6f-157">Next steps</span></span>
[<span data-ttu-id="61d6f-158">Čtení zpráv ze služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="61d6f-158">Read messages from your IoT hub</span></span>](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)