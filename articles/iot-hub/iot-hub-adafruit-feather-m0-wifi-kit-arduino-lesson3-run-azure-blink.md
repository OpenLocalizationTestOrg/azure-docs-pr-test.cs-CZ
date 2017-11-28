---
title: "Connect Arduino (C) tooAzure IoT - Lekce 3: spuštění ukázkových | Microsoft Docs"
description: "Nasazení a spuštění ukázkové aplikace tooAdafruit prolnutí M0 WiFi, která odesílá zprávy tooyour IoT hub a bliká DIODU hello."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "cloudové služby IOT, arduino odesílat data toocloud"
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
ms.openlocfilehash: ddca015a3655f8a1a9de2a00e718ec0d28a5affb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-toosend-device-to-cloud-messages"></a><span data-ttu-id="6fa53-104">Spuštění ukázkové aplikace toosend zpráv typu zařízení cloud</span><span class="sxs-lookup"><span data-stu-id="6fa53-104">Run a sample application toosend device-to-cloud messages</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="6fa53-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="6fa53-105">What you will do</span></span>
<span data-ttu-id="6fa53-106">Tento článek vám ukáže, jak toodeploy a spusťte ukázkové aplikace na vašem Adafruit prolnutí M0 Wi-Fi Arduino board že odešle zprávy tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="6fa53-106">This article will show you how toodeploy and run a sample application on your Adafruit Feather M0 WiFi Arduino board that sends messages tooyour IoT hub.</span></span>

<span data-ttu-id="6fa53-107">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="6fa53-107">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="6fa53-108">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="6fa53-108">What you will learn</span></span>
<span data-ttu-id="6fa53-109">Se dozvíte, jak toouse hello gulp toodeploy nástroje a spusťte hello ukázkové Arduino aplikace na vaší kartě Arduino.</span><span class="sxs-lookup"><span data-stu-id="6fa53-109">You will learn how toouse hello gulp tool toodeploy and run hello sample Arduino application on your Arduino board.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="6fa53-110">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="6fa53-110">What you need</span></span>
* <span data-ttu-id="6fa53-111">Než začnete tuto úlohu, musí úspěšně jste dokončili [vytvořit aplikaci Azure funkce a úložiště účtu úložiště a tooprocess IoT hub zprávy][process-and-store-iot-hub-messages].</span><span class="sxs-lookup"><span data-stu-id="6fa53-111">Before you start this task, you must have successfully completed [Create an Azure function app and a storage account tooprocess and store IoT hub messages][process-and-store-iot-hub-messages].</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="6fa53-112">Získat IoT hub a zařízení připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="6fa53-112">Get your IoT hub and device connection strings</span></span>
<span data-ttu-id="6fa53-113">Hello zařízení připojovací řetězec je použité tooconnect Arduino Tabule tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="6fa53-113">hello device connection string is used tooconnect your Arduino board tooyour IoT hub.</span></span> <span data-ttu-id="6fa53-114">Hello IoT hub připojovací řetězec je použité tooconnect vaše IoT hub toohello zařízení identita, která představuje váš Arduino board hello IoT hub.</span><span class="sxs-lookup"><span data-stu-id="6fa53-114">hello IoT hub connection string is used tooconnect your IoT hub toohello device identity that represents your Arduino board in hello IoT hub.</span></span>

* <span data-ttu-id="6fa53-115">Seznam všech centra IoT ve vaší skupině prostředků tak, že spustíte následující příkaz rozhraní příkazového řádku Azure hello:</span><span class="sxs-lookup"><span data-stu-id="6fa53-115">List all your IoT hubs in your resource group by running hello following Azure CLI command:</span></span>

```bash
az iot hub list -g iot-sample --query [].name
```

<span data-ttu-id="6fa53-116">Použití `iot-sample` jako hodnota hello `{resource group name}` Pokud hodnota hello nebyla změněna.</span><span class="sxs-lookup"><span data-stu-id="6fa53-116">Use `iot-sample` as hello value of `{resource group name}` if you didn't change hello value.</span></span>

* <span data-ttu-id="6fa53-117">Spuštěním následujícího příkazu příkazového řádku Azure CLI hello získáte hello IoT hub, připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="6fa53-117">Get hello IoT hub connection string by running hello following Azure CLI command:</span></span>

```bash
az iot hub show-connection-string --name {my hub name}
```

<span data-ttu-id="6fa53-118">`{my hub name}`je název hello, které jste zadali při vytvoření služby IoT hub a registraci Arduino panel.</span><span class="sxs-lookup"><span data-stu-id="6fa53-118">`{my hub name}` is hello name that you specified when you created your IoT hub and registered your Arduino board.</span></span>

* <span data-ttu-id="6fa53-119">Získáte hello zařízení připojovací řetězec tak, že spustíte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="6fa53-119">Get hello device connection string by running hello following command:</span></span>

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id mym0wifi
```

<span data-ttu-id="6fa53-120">Použití `mym0wifi` jako hodnota hello `{device id}` Pokud hodnota hello nebyla změněna.</span><span class="sxs-lookup"><span data-stu-id="6fa53-120">Use `mym0wifi` as hello value of `{device id}` if you didn't change hello value.</span></span>
## <a name="configure-hello-device-connection"></a><span data-ttu-id="6fa53-121">Nakonfigurujte připojení zařízení hello</span><span class="sxs-lookup"><span data-stu-id="6fa53-121">Configure hello device connection</span></span>
<span data-ttu-id="6fa53-122">tooconfigure hello připojení zařízení, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="6fa53-122">tooconfigure hello device connection, follow these steps:</span></span>

1. <span data-ttu-id="6fa53-123">Získejte hello sériového portu zařízení hello s hello zařízení zjišťování rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="6fa53-123">Obtain hello serial port of hello device with hello device discovery cli:</span></span>

   ```bash
   devdisco list --usb
   ```

   <span data-ttu-id="6fa53-124">Měli byste zobrazený výstup, který je podobný toohello následující a najít hello usb COM port pro panel Arduino:</span><span class="sxs-lookup"><span data-stu-id="6fa53-124">You should see an output that is similar toohello following and find hello usb COM port for your Arduino board:</span></span>

   ![Zjišťování zařízení][device-discovery]

2. <span data-ttu-id="6fa53-126">Soubor otevřete hello `config.json` v hello lekce složky a přidat hello hodnotu hello najít číslo portu COM:</span><span class="sxs-lookup"><span data-stu-id="6fa53-126">Open hello file `config.json` in hello lesson folder and add hello value of hello found COM port number:</span></span>

   ```json
   {
       "device_port" : "COM1"
   }
   ```

   ![Config.JSON][config-json]

   > [!NOTE]
   > <span data-ttu-id="6fa53-128">Pro port hello COM na platformě Windows má formát hello `COM1, COM2, ...`.</span><span class="sxs-lookup"><span data-stu-id="6fa53-128">For hello COM port, on Windows platform, it has hello format of `COM1, COM2, ...`.</span></span> <span data-ttu-id="6fa53-129">V systému macOS nebo Ubuntu začíná `/dev/`.</span><span class="sxs-lookup"><span data-stu-id="6fa53-129">On macOS or Ubuntu, it starts with `/dev/`.</span></span>

3. <span data-ttu-id="6fa53-130">Inicializovat konfigurační soubor hello spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="6fa53-130">Initialize hello configuration file by running hello following commands:</span></span>

   ```bash
   npm install
   gulp init
   gulp install-tools
   ```
4. <span data-ttu-id="6fa53-131">Otevřete hello zařízení konfigurační soubor `config-arduino.json` ve Visual Studio Code spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6fa53-131">Open hello device configuration file `config-arduino.json` in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-arduino.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-arduino.json
   ```

   ![Konfigurace arduino.json][config-arduino-json]

5. <span data-ttu-id="6fa53-133">Ujistěte se, hello následující nahrazení v hello `config-arduino.json` souboru:</span><span class="sxs-lookup"><span data-stu-id="6fa53-133">Make hello following replacements in hello `config-arduino.json` file:</span></span>

   * <span data-ttu-id="6fa53-134">Nahraďte **[Wi-Fi SSID]** s vaší SSID Wi-Fi, který připojený toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="6fa53-134">Replace **[Wi-Fi SSID]** with your Wi-Fi SSID that connected toohello Internet.</span></span>
   * <span data-ttu-id="6fa53-135">Nahraďte **[Wi-Fi heslo]** pomocí hesla Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="6fa53-135">Replace **[Wi-Fi password]** with your Wi-Fi password.</span></span> <span data-ttu-id="6fa53-136">Odeberte hello řetězec, pokud Wi-Fi nevyžaduje heslo.</span><span class="sxs-lookup"><span data-stu-id="6fa53-136">Remove hello string if your Wi-Fi doesn't require password.</span></span>
   * <span data-ttu-id="6fa53-137">Nahraďte **[řetězec připojení zařízení IoT]** s hello `device connection string` jste získali.</span><span class="sxs-lookup"><span data-stu-id="6fa53-137">Replace **[IoT device connection string]** with hello `device connection string` you obtained.</span></span>
   * <span data-ttu-id="6fa53-138">Nahraďte **[IoT hub, připojovací řetězec]** s hello `iot hub connection string` jste získali.</span><span class="sxs-lookup"><span data-stu-id="6fa53-138">Replace **[IoT hub connection string]** with hello `iot hub connection string` you obtained.</span></span>

   > [!NOTE]
   > <span data-ttu-id="6fa53-139">Nepotřebujete `azure_storage_connection_string` v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="6fa53-139">You don't need `azure_storage_connection_string` in this article.</span></span> <span data-ttu-id="6fa53-140">Dodržte jako je.</span><span class="sxs-lookup"><span data-stu-id="6fa53-140">Keep it as is.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="6fa53-141">Nasazení a spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="6fa53-141">Deploy and run hello sample application</span></span>
<span data-ttu-id="6fa53-142">Nasazení a spuštění ukázkové aplikace hello na panel Arduino spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6fa53-142">Deploy and run hello sample application on your Arduino board by running hello following command:</span></span>

```bash
gulp run
# You can monitor hello serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

> [!NOTE]
> <span data-ttu-id="6fa53-143">Hello výchozí gulp úloha spustí `install-tools` a `run` úlohy postupně.</span><span class="sxs-lookup"><span data-stu-id="6fa53-143">hello default gulp task runs `install-tools` and `run` tasks sequentially.</span></span> <span data-ttu-id="6fa53-144">Pokud jste [hello blikání aplikace nasazená][deployed-the-blink-app], jste spustili tyto úlohy samostatně.</span><span class="sxs-lookup"><span data-stu-id="6fa53-144">When you [deployed hello blink app][deployed-the-blink-app], you ran these tasks separately.</span></span>

## <a name="verify-that-hello-sample-application-works"></a><span data-ttu-id="6fa53-145">Ověřte, že funguje hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="6fa53-145">Verify that hello sample application works</span></span>
<span data-ttu-id="6fa53-146">Měli byste vidět integrovaného blikat DIODU hello GPIO #0 každé dvě sekundy.</span><span class="sxs-lookup"><span data-stu-id="6fa53-146">You should see hello GPIO #0 on-board LED blinking every two seconds.</span></span> <span data-ttu-id="6fa53-147">Pokaždé, když hello DIODU bliká, hello ukázkové aplikace odešle zpráva tooyour IoT hub a ověřuje, zda je že tento hello byla úspěšně odeslána zpráva tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="6fa53-147">Every time hello LED blinks, hello sample application sends a message tooyour IoT hub and verifies that hello message has been successfully sent tooyour IoT hub.</span></span> <span data-ttu-id="6fa53-148">Kromě toho každou zprávu přijme hello IoT hub je vytištěna v okně konzoly hello.</span><span class="sxs-lookup"><span data-stu-id="6fa53-148">In addition, each message received by hello IoT hub is printed in hello console window.</span></span> <span data-ttu-id="6fa53-149">Ukázková aplikace Hello automaticky ukončí po odeslání 20 zpráv.</span><span class="sxs-lookup"><span data-stu-id="6fa53-149">hello sample application terminates automatically after sending 20 messages.</span></span>

![Ukázkovou aplikaci s odeslané a přijaté zprávy][sample-application-with-sent-and-received-messages]

## <a name="summary"></a><span data-ttu-id="6fa53-151">Souhrn</span><span class="sxs-lookup"><span data-stu-id="6fa53-151">Summary</span></span>
<span data-ttu-id="6fa53-152">Jste nasazení a spusťte hello nové blikání ukázkovou aplikaci ve službě Arduino Tabule toosend zpráv typu zařízení cloud tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="6fa53-152">You've deployed and run hello new blink sample application on your Arduino board toosend device-to-cloud messages tooyour IoT hub.</span></span> <span data-ttu-id="6fa53-153">Vaše zprávy se teď sledovat, jak jsou napsány toohello účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="6fa53-153">You now monitor your messages as they are written toohello storage account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6fa53-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6fa53-154">Next steps</span></span>
<span data-ttu-id="6fa53-155">[Čtení zprávy uchovávané v úložišti Azure][read-messages-persisted-in-azure-storage]</span><span class="sxs-lookup"><span data-stu-id="6fa53-155">[Read messages persisted in Azure Storage][read-messages-persisted-in-azure-storage]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[config-arduino-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/config-arduino.png
[deployed-the-blink-app]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-deploy-blink-app.md
[sample-application-with-sent-and-received-messages]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/gulp_run_arduino.png
[read-messages-persisted-in-azure-storage]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-read-table-storage.md