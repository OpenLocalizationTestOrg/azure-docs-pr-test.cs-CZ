---
title: "Connect Arduino (C) k Azure IoT - Lekce 3: spuštění ukázkových | Microsoft Docs"
description: "Nasazení a spuštění ukázkové aplikace pro Adafruit prolnutí M0 WiFi, která odesílá zprávy do služby IoT hub a bliká Indikátor."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "cloudové služby IOT, arduino odesílat data do cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 92cce319-2b17-4c9b-889d-deac959e3e7c
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 0c17fe74dbd78abca955f7789a1674ed6333367f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a><span data-ttu-id="59930-104">Spuštění ukázkové aplikace k odesílání zpráv typu zařízení cloud</span><span class="sxs-lookup"><span data-stu-id="59930-104">Run a sample application to send device-to-cloud messages</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="59930-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="59930-105">What you will do</span></span>
<span data-ttu-id="59930-106">Tento článek vám ukáže, jak nasadit a spustit ukázkovou aplikaci na vaší kartě Adafruit prolnutí M0 Wi-Fi Arduino, která odesílá zprávy do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="59930-106">This article will show you how to deploy and run a sample application on your Adafruit Feather M0 WiFi Arduino board that sends messages to your IoT hub.</span></span>

<span data-ttu-id="59930-107">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="59930-107">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="59930-108">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="59930-108">What you will learn</span></span>
<span data-ttu-id="59930-109">Se dozvíte, jak používat nástroj gulp k nasazení a spuštění ukázkové aplikace Arduino na panel Arduino.</span><span class="sxs-lookup"><span data-stu-id="59930-109">You will learn how to use the gulp tool to deploy and run the sample Arduino application on your Arduino board.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="59930-110">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="59930-110">What you need</span></span>
* <span data-ttu-id="59930-111">Než začnete tuto úlohu, musí úspěšně jste dokončili [vytvořit aplikaci Azure funkce a účet úložiště pro zpracování a ukládání zpráv centra IoT][process-and-store-iot-hub-messages].</span><span class="sxs-lookup"><span data-stu-id="59930-111">Before you start this task, you must have successfully completed [Create an Azure function app and a storage account to process and store IoT hub messages][process-and-store-iot-hub-messages].</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="59930-112">Získat IoT hub a zařízení připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="59930-112">Get your IoT hub and device connection strings</span></span>
<span data-ttu-id="59930-113">Připojovací řetězec zařízení slouží k připojení vaší karty Arduino do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="59930-113">The device connection string is used to connect your Arduino board to your IoT hub.</span></span> <span data-ttu-id="59930-114">IoT hub připojovací řetězec se používá pro připojení služby IoT hub pro identitu zařízení, která představuje panel Arduino ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="59930-114">The IoT hub connection string is used to connect your IoT hub to the device identity that represents your Arduino board in the IoT hub.</span></span>

* <span data-ttu-id="59930-115">Seznam všech centra IoT ve vaší skupině prostředků spuštěním následujícího příkazu příkazového řádku Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="59930-115">List all your IoT hubs in your resource group by running the following Azure CLI command:</span></span>

```bash
az iot hub list -g iot-sample --query [].name
```

<span data-ttu-id="59930-116">Použití `iot-sample` jako hodnotu `{resource group name}` Pokud nebylo změňte hodnotu.</span><span class="sxs-lookup"><span data-stu-id="59930-116">Use `iot-sample` as the value of `{resource group name}` if you didn't change the value.</span></span>

* <span data-ttu-id="59930-117">Spuštěním následujícího příkazu příkazového řádku Azure CLI získáte připojovací řetězec centra IoT:</span><span class="sxs-lookup"><span data-stu-id="59930-117">Get the IoT hub connection string by running the following Azure CLI command:</span></span>

```bash
az iot hub show-connection-string --name {my hub name}
```

<span data-ttu-id="59930-118">`{my hub name}`je název, který jste zadali při vytvoření služby IoT hub a registraci Arduino panel.</span><span class="sxs-lookup"><span data-stu-id="59930-118">`{my hub name}` is the name that you specified when you created your IoT hub and registered your Arduino board.</span></span>

* <span data-ttu-id="59930-119">Získáte připojovací řetězec zařízení tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="59930-119">Get the device connection string by running the following command:</span></span>

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id mym0wifi
```

<span data-ttu-id="59930-120">Použití `mym0wifi` jako hodnotu `{device id}` Pokud nebylo změňte hodnotu.</span><span class="sxs-lookup"><span data-stu-id="59930-120">Use `mym0wifi` as the value of `{device id}` if you didn't change the value.</span></span>
## <a name="configure-the-device-connection"></a><span data-ttu-id="59930-121">Nakonfigurujte připojení zařízení</span><span class="sxs-lookup"><span data-stu-id="59930-121">Configure the device connection</span></span>
<span data-ttu-id="59930-122">Konfigurace připojení zařízení, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="59930-122">To configure the device connection, follow these steps:</span></span>

1. <span data-ttu-id="59930-123">Získání sériového portu zařízení pomocí rozhraní příkazového řádku zjišťování zařízení:</span><span class="sxs-lookup"><span data-stu-id="59930-123">Obtain the serial port of the device with the device discovery cli:</span></span>

   ```bash
   devdisco list --usb
   ```

   <span data-ttu-id="59930-124">Měli byste zobrazený výstup, který je podobný následujícímu a najít usb COM port pro panel Arduino:</span><span class="sxs-lookup"><span data-stu-id="59930-124">You should see an output that is similar to the following and find the usb COM port for your Arduino board:</span></span>

   ![Zjišťování zařízení][device-discovery]

2. <span data-ttu-id="59930-126">Otevřete soubor `config.json` ve složce lekce a přidejte hodnotu nalezen číslo portu COM:</span><span class="sxs-lookup"><span data-stu-id="59930-126">Open the file `config.json` in the lesson folder and add the value of the found COM port number:</span></span>

   ```json
   {
       "device_port" : "COM1"
   }
   ```

   ![Config.JSON][config-json]

   > [!NOTE]
   > <span data-ttu-id="59930-128">Pro port COM, na platformě Windows má formát `COM1, COM2, ...`.</span><span class="sxs-lookup"><span data-stu-id="59930-128">For the COM port, on Windows platform, it has the format of `COM1, COM2, ...`.</span></span> <span data-ttu-id="59930-129">V systému macOS nebo Ubuntu začíná `/dev/`.</span><span class="sxs-lookup"><span data-stu-id="59930-129">On macOS or Ubuntu, it starts with `/dev/`.</span></span>

3. <span data-ttu-id="59930-130">Inicializace konfiguračního souboru spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="59930-130">Initialize the configuration file by running the following commands:</span></span>

   ```bash
   npm install
   gulp init
   gulp install-tools
   ```
4. <span data-ttu-id="59930-131">Otevřete konfigurační soubor zařízení `config-arduino.json` ve Visual Studio Code spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="59930-131">Open the device configuration file `config-arduino.json` in Visual Studio Code by running the following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-arduino.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-arduino.json
   ```

   ![Konfigurace arduino.json][config-arduino-json]

5. <span data-ttu-id="59930-133">Zkontrolujte následující nahrazení v `config-arduino.json` souboru:</span><span class="sxs-lookup"><span data-stu-id="59930-133">Make the following replacements in the `config-arduino.json` file:</span></span>

   * <span data-ttu-id="59930-134">Nahraďte **[Wi-Fi SSID]** s vaší SSID Wi-Fi, který připojení k Internetu.</span><span class="sxs-lookup"><span data-stu-id="59930-134">Replace **[Wi-Fi SSID]** with your Wi-Fi SSID that connected to the Internet.</span></span>
   * <span data-ttu-id="59930-135">Nahraďte **[Wi-Fi heslo]** pomocí hesla Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="59930-135">Replace **[Wi-Fi password]** with your Wi-Fi password.</span></span> <span data-ttu-id="59930-136">Odeberte řetězec, pokud Wi-Fi nevyžaduje heslo.</span><span class="sxs-lookup"><span data-stu-id="59930-136">Remove the string if your Wi-Fi doesn't require password.</span></span>
   * <span data-ttu-id="59930-137">Nahraďte **[řetězec připojení zařízení IoT]** s `device connection string` jste získali.</span><span class="sxs-lookup"><span data-stu-id="59930-137">Replace **[IoT device connection string]** with the `device connection string` you obtained.</span></span>
   * <span data-ttu-id="59930-138">Nahraďte **[IoT hub, připojovací řetězec]** s `iot hub connection string` jste získali.</span><span class="sxs-lookup"><span data-stu-id="59930-138">Replace **[IoT hub connection string]** with the `iot hub connection string` you obtained.</span></span>

   > [!NOTE]
   > <span data-ttu-id="59930-139">Nepotřebujete `azure_storage_connection_string` v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="59930-139">You don't need `azure_storage_connection_string` in this article.</span></span> <span data-ttu-id="59930-140">Dodržte jako je.</span><span class="sxs-lookup"><span data-stu-id="59930-140">Keep it as is.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="59930-141">Nasazení a spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="59930-141">Deploy and run the sample application</span></span>
<span data-ttu-id="59930-142">Nasazení a spuštění ukázkové aplikace na panel Arduino spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="59930-142">Deploy and run the sample application on your Arduino board by running the following command:</span></span>

```bash
gulp run
# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

> [!NOTE]
> <span data-ttu-id="59930-143">Spuštění úlohy gulp výchozí `install-tools` a `run` úlohy postupně.</span><span class="sxs-lookup"><span data-stu-id="59930-143">The default gulp task runs `install-tools` and `run` tasks sequentially.</span></span> <span data-ttu-id="59930-144">Když jste [blikání aplikace nasazená][deployed-the-blink-app], jste spustili tyto úlohy samostatně.</span><span class="sxs-lookup"><span data-stu-id="59930-144">When you [deployed the blink app][deployed-the-blink-app], you ran these tasks separately.</span></span>

## <a name="verify-that-the-sample-application-works"></a><span data-ttu-id="59930-145">Ověřte, že funguje ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="59930-145">Verify that the sample application works</span></span>
<span data-ttu-id="59930-146">Měli byste vidět GPIO #0 integrovaného DIODU blikat, které každý dvou sekund.</span><span class="sxs-lookup"><span data-stu-id="59930-146">You should see the GPIO #0 on-board LED blinking every two seconds.</span></span> <span data-ttu-id="59930-147">Pokaždé, když DIODU bliká, ukázkové aplikace odešle zprávu do služby IoT hub a ověřuje, že zpráva byla úspěšně odeslána do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="59930-147">Every time the LED blinks, the sample application sends a message to your IoT hub and verifies that the message has been successfully sent to your IoT hub.</span></span> <span data-ttu-id="59930-148">Kromě toho každou zprávu přijme služby IoT hub je vytištěna v okně konzoly.</span><span class="sxs-lookup"><span data-stu-id="59930-148">In addition, each message received by the IoT hub is printed in the console window.</span></span> <span data-ttu-id="59930-149">Ukázkové aplikace automaticky ukončí po odeslání 20 zpráv.</span><span class="sxs-lookup"><span data-stu-id="59930-149">The sample application terminates automatically after sending 20 messages.</span></span>

![Ukázkovou aplikaci s odeslané a přijaté zprávy][sample-application-with-sent-and-received-messages]

## <a name="summary"></a><span data-ttu-id="59930-151">Souhrn</span><span class="sxs-lookup"><span data-stu-id="59930-151">Summary</span></span>
<span data-ttu-id="59930-152">Jste nasazení a spuštění nové aplikace ukázka blikání na panel Arduino k odesílání zpráv typu zařízení cloud do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="59930-152">You've deployed and run the new blink sample application on your Arduino board to send device-to-cloud messages to your IoT hub.</span></span> <span data-ttu-id="59930-153">Zprávy si můžete nyní sledovat, jak se zapisují na účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="59930-153">You now monitor your messages as they are written to the storage account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="59930-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="59930-154">Next steps</span></span>
<span data-ttu-id="59930-155">[Čtení zprávy uchovávané v úložišti Azure][read-messages-persisted-in-azure-storage]</span><span class="sxs-lookup"><span data-stu-id="59930-155">[Read messages persisted in Azure Storage][read-messages-persisted-in-azure-storage]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[config-arduino-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/config-arduino.png
[deployed-the-blink-app]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-deploy-blink-app.md
[sample-application-with-sent-and-received-messages]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/gulp_run_arduino.png
[read-messages-persisted-in-azure-storage]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-read-table-storage.md