---
title: "Zařízení SensorTag & brány Azure IoT - Lekce 3: spuštění ukázkové aplikace | Microsoft Docs"
description: "Spuštění ukázkové aplikace zakázat přijímat data z SensorTag zakázat a ze služby IoT hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "zakázat aplikace, senzor monitorování aplikace, shromažďování dat senzor, data ze senzorů, data snímače do cloudu"
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
ms.openlocfilehash: f6fa158dbe1d48be7d493efa6217e1e0a759d2f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-and-run-a-ble-sample-application"></a><span data-ttu-id="30883-104">Nakonfigurujte a spusťte ukázkové aplikace zakázat</span><span class="sxs-lookup"><span data-stu-id="30883-104">Configure and run a BLE sample application</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="30883-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="30883-105">What you will do</span></span>

- <span data-ttu-id="30883-106">Naklonujte úložiště ukázka.</span><span class="sxs-lookup"><span data-stu-id="30883-106">Clone the sample repository.</span></span> 
- <span data-ttu-id="30883-107">Nastavte propojení mezi SensorTag a Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="30883-107">Set up the connectivity between SensorTag and Intel NUC.</span></span> 
- <span data-ttu-id="30883-108">Použijte rozhraní příkazového řádku Azure k získání IoT hub a informace o SensorTag pro ukázkovou aplikaci zakázat (Bluetooth nízkou energie).</span><span class="sxs-lookup"><span data-stu-id="30883-108">Use the Azure CLI to get your IoT hub and SensorTag information for a BLE(Bluetooth Low Energy) sample application.</span></span> <span data-ttu-id="30883-109">A nakonfigurujte a spusťte ukázkové aplikace zakázat.</span><span class="sxs-lookup"><span data-stu-id="30883-109">And configure and run the BLE sample application.</span></span> 

<span data-ttu-id="30883-110">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="30883-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="30883-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="30883-111">What you will learn</span></span>

<span data-ttu-id="30883-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="30883-112">In this article, you will learn:</span></span>

- <span data-ttu-id="30883-113">Postup konfigurace a pak spusťte ukázkovou aplikaci zakázat.</span><span class="sxs-lookup"><span data-stu-id="30883-113">How to configure and run the BLE sample application.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="30883-114">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="30883-114">What you need</span></span>

<span data-ttu-id="30883-115">Musí byl úspěšně dokončen</span><span class="sxs-lookup"><span data-stu-id="30883-115">You must have successfully completed</span></span>

- [<span data-ttu-id="30883-116">Vytvoření služby IoT hub a zaregistrujte SensorTag</span><span class="sxs-lookup"><span data-stu-id="30883-116">Create an IoT hub and register SensorTag</span></span>](iot-hub-gateway-kit-c-lesson2-register-device.md)

## <a name="clone-the-sample-repository-to-the-host-computer"></a><span data-ttu-id="30883-117">Naklonujte úložiště ukázkové k hostitelskému počítači</span><span class="sxs-lookup"><span data-stu-id="30883-117">Clone the sample repository to the host computer</span></span>

<span data-ttu-id="30883-118">Klonovat úložiště ukázkové, postupujte takto na hostitelském počítači:</span><span class="sxs-lookup"><span data-stu-id="30883-118">To clone the sample repository, follow these steps on the host computer:</span></span>

1. <span data-ttu-id="30883-119">Otevřete okno příkazového řádku v systému Windows nebo otevřete terminál v systému macOS nebo Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="30883-119">Open a Command Prompt window in Windows or open a terminal in macOS or Ubuntu.</span></span>
2. <span data-ttu-id="30883-120">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="30883-120">Run the following commands:</span></span>

   ```bash
   git clone https://github.com/Azure-samples/iot-hub-c-intel-nuc-gateway-getting-started
   cd iot-hub-c-intel-nuc-gateway-getting-started
   ```

## <a name="set-up-the-connectivity-between-sensortag-and-intel-nuc"></a><span data-ttu-id="30883-121">Nastavit propojení mezi SensorTag a Intel NUC</span><span class="sxs-lookup"><span data-stu-id="30883-121">Set up the connectivity between SensorTag and Intel NUC</span></span>

<span data-ttu-id="30883-122">Pokud chcete nastavit připojení, postupujte podle těchto kroků v hostitelském počítači:</span><span class="sxs-lookup"><span data-stu-id="30883-122">To set up the connectivity, follow these steps on the host computer:</span></span>

1. <span data-ttu-id="30883-123">Inicializace konfiguračního souboru spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="30883-123">Initialize the configuration file by running the following commands:</span></span>

   ```bash
   cd Lesson3
   npm install
   gulp init
   ```

2. <span data-ttu-id="30883-124">Otevřete `config-gateway.json` ve Visual Studio Code spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="30883-124">Open `config-gateway.json` in Visual Studio Code by running the following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-gateway.json
   ```

3. <span data-ttu-id="30883-125">Vyhledejte následující řádek kódu a nahraďte `[device hostname or IP address]` s IP adresu nebo název hostitele Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="30883-125">Locate the following line of code and replace `[device hostname or IP address]` with the IP address or host name of Intel NUC.</span></span>
   <span data-ttu-id="30883-126">![snímek obrazovky konfigurace brány](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)</span><span class="sxs-lookup"><span data-stu-id="30883-126">![screenshot of config gateway](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)</span></span>

4. <span data-ttu-id="30883-127">Instalace pomocné nástroje na Intel NUC spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="30883-127">Install helper tools on Intel NUC by running the following command:</span></span>

   ```bash
   gulp install-tools
   ```

5. <span data-ttu-id="30883-128">Zapněte SensorTag stisknutím tlačítka napájení jako na následujícím obrázku a zelená DIODU měla blikat.</span><span class="sxs-lookup"><span data-stu-id="30883-128">Turn on SensorTag by pressing the power button as the following picture, and the green LED should blink.</span></span>

   ![zapnout Sensortag](media/iot-hub-gateway-kit-lessons/lesson3/turn on_off sensortag.jpg)

6. <span data-ttu-id="30883-130">Kontrolovat zařízení SensorTag spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="30883-130">Scan SensorTag devices by running the following commands:</span></span>

   ```bash
   gulp discover-sensortag
   ```

7. <span data-ttu-id="30883-131">Testovací připojení mezi SensorTag a Intel NUC spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="30883-131">Test the connectivity between the SensorTag and Intel NUC by running the following command:</span></span>

   ```bash
   gulp test-connectivity --mac {mac address}
   ```

   <span data-ttu-id="30883-132">Nahraďte `{mac address}` s adresou MAC, který jste získali v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="30883-132">Replace `{mac address}` with the MAC address that you obtained in the previous step.</span></span>

## <a name="get-the-connection-string-of-sensortag"></a><span data-ttu-id="30883-133">Získat připojovací řetězec SensorTag</span><span class="sxs-lookup"><span data-stu-id="30883-133">Get the connection string of SensorTag</span></span>

<span data-ttu-id="30883-134">Chcete-li získat Azure IoT hub připojovací řetězec SensorTag, spusťte následující příkaz na hostitelském počítači:</span><span class="sxs-lookup"><span data-stu-id="30883-134">To get the Azure IoT hub connection string of SensorTag, run the following command on the host computer:</span></span>

```bash
az iot device show-connection-string --hub-name {IoT hub name} --device-id mydevice --resource-group iot-gateway
```

<span data-ttu-id="30883-135">`{IoT hub name}`je název centra IoT, které jste použili.</span><span class="sxs-lookup"><span data-stu-id="30883-135">`{IoT hub name}` is the IoT hub name that you used.</span></span> <span data-ttu-id="30883-136">Použít iot brány jako hodnotu `{resource group name}` a použít jako hodnotu mydevice `{device id}` Pokud nebylo změňte hodnotu v lekci 2.</span><span class="sxs-lookup"><span data-stu-id="30883-136">Use iot-gateway as the value of `{resource group name}` and use mydevice as the value of `{device id}` if you didn't change the value in Lesson 2.</span></span>

## <a name="configure-the-ble-sample-application"></a><span data-ttu-id="30883-137">Nakonfigurujte ukázkovou aplikaci zakázat</span><span class="sxs-lookup"><span data-stu-id="30883-137">Configure the BLE sample application</span></span>

<span data-ttu-id="30883-138">Pro konfiguraci a spuštění ukázkové aplikace zakázat, postupujte podle těchto kroků na hostitelském počítači:</span><span class="sxs-lookup"><span data-stu-id="30883-138">To configure and run the BLE sample application, follow these steps on the host computer:</span></span>

1. <span data-ttu-id="30883-139">Otevřete `config-sensortag.json` ve Visual Studio Code spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="30883-139">Open `config-sensortag.json` in Visual Studio Code by running the following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-sensortag.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-sensortag.json
   ```

   ![snímek obrazovky konfigurace sensortag](media/iot-hub-gateway-kit-lessons/lesson3/config_sensortag.png)

2. <span data-ttu-id="30883-141">Zkontrolujte následující nahrazení v kódu:</span><span class="sxs-lookup"><span data-stu-id="30883-141">Make the following replacements in the code:</span></span>
   - <span data-ttu-id="30883-142">Nahraďte `[IoT hub name]` názvem IoT hub, který jste použili.</span><span class="sxs-lookup"><span data-stu-id="30883-142">Replace `[IoT hub name]` with the IoT hub name that you used.</span></span>
   - <span data-ttu-id="30883-143">Nahraďte `[IoT device connection string]` připojovacím řetězcem SensorTag, který jste získali.</span><span class="sxs-lookup"><span data-stu-id="30883-143">Replace `[IoT device connection string]` with the connection string of SensorTag that you obtained.</span></span>
   - <span data-ttu-id="30883-144">Nahraďte `[device_mac_address]` s adresou MAC SensorTag, který jste získali.</span><span class="sxs-lookup"><span data-stu-id="30883-144">Replace `[device_mac_address]` with the MAC address of the SensorTag that you obtained.</span></span>

3. <span data-ttu-id="30883-145">Spuštění ukázkové aplikace zakázat.</span><span class="sxs-lookup"><span data-stu-id="30883-145">Run the BLE sample application.</span></span>

   <span data-ttu-id="30883-146">Pokud chcete spustit zakázat ukázkovou aplikaci, postupujte takto na hostitelském počítači:</span><span class="sxs-lookup"><span data-stu-id="30883-146">To run the BLE sample application, follow these steps on the host computer:</span></span>

   1. <span data-ttu-id="30883-147">Zapněte SensorTag.</span><span class="sxs-lookup"><span data-stu-id="30883-147">Turn on SensorTag.</span></span>

   2. <span data-ttu-id="30883-148">Nasazení a spuštění ukázkové aplikace lit na Intel NUC spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="30883-148">Deploy and run the BLE sample application on Intel NUC by running the following command:</span></span>
   
      ```bash
      gulp run
      ```

## <a name="verify-that-the-ble-sample-application-works"></a><span data-ttu-id="30883-149">Ověřte, že funguje zakázat ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="30883-149">Verify that the BLE sample application works</span></span>

<span data-ttu-id="30883-150">Teď byste měli vidět výstup takto:</span><span class="sxs-lookup"><span data-stu-id="30883-150">You should now see an output like the following:</span></span>

![Zakázat ukázkové aplikace výstup](media/iot-hub-gateway-kit-lessons/lesson3/BLE_running.png)

<span data-ttu-id="30883-152">Ukázkovou aplikaci udržuje shromažďování dat teploty a odeslaných do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="30883-152">The sample application keeps collecting temperature data and sent it to your IoT hub.</span></span> <span data-ttu-id="30883-153">Ukázkové aplikace automaticky ukončí po odeslání 40 sekund.</span><span class="sxs-lookup"><span data-stu-id="30883-153">The sample application terminates automatically after sending 40 seconds.</span></span>

## <a name="summary"></a><span data-ttu-id="30883-154">Souhrn</span><span class="sxs-lookup"><span data-stu-id="30883-154">Summary</span></span>

<span data-ttu-id="30883-155">Úspěšně jste nastavit propojení mezi SensorTag a Intel NUC a spusťte zakázat ukázkovou aplikaci, která shromažďuje a odesílá data z SensorTag do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="30883-155">You've successfully set up the connectivity between SensorTag and Intel NUC, and run a BLE sample application which collects and sends data from SensorTag to your IoT hub.</span></span> <span data-ttu-id="30883-156">Jste připravení zjistěte, jak ověřit, že služby IoT hub obdržel data.</span><span class="sxs-lookup"><span data-stu-id="30883-156">You're ready to learn how to verify that your IoT hub has received the data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="30883-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="30883-157">Next steps</span></span>
[<span data-ttu-id="30883-158">Čtení zpráv ze služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="30883-158">Read messages from your IoT hub</span></span>](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)