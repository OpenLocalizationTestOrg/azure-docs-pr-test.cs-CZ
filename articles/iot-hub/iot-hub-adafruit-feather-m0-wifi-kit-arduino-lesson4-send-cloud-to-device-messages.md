---
title: "Připojení k Azure IoT - Lekce 4 Arduino (C): Cloud zařízení | Microsoft Docs"
description: "Ukázková aplikace spustí na Adafruit prolnutí M0 Wi-Fi a monitorování příchozích zpráv ze služby IoT hub. Nová úloha gulp odesílá zprávy Adafruit prolnutí M0 Wi-Fi ze služby IoT hub blikání Indikátor."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "ovládací prvek arduino vedla z webové, arduino řízení vedla přes web"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: a0bf53fb-29fb-485f-ba4a-6c715057b1a2
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: b4f4c1d86b10b64c104ac812d1f650d723322be8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="run-a-sample-application-to-receive-cloud-to-device-messages"></a><span data-ttu-id="55268-105">Spuštění ukázkové aplikace pro příjem zpráv typu cloud zařízení</span><span class="sxs-lookup"><span data-stu-id="55268-105">Run a sample application to receive cloud-to-device messages</span></span>
<span data-ttu-id="55268-106">V tomto článku nasazení ukázkové aplikace na vaší kartě Adafruit prolnutí M0 Wi-Fi Arduino.</span><span class="sxs-lookup"><span data-stu-id="55268-106">In this article, you deploy a sample application on your Adafruit Feather M0 WiFi Arduino board.</span></span>

<span data-ttu-id="55268-107">Ukázkovou aplikaci monitoruje příchozích zpráv ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="55268-107">The sample application monitors incoming messages from your IoT hub.</span></span> <span data-ttu-id="55268-108">Můžete taky spustit úlohu gulp ve vašem počítači k odesílání zpráv do vaší karty Arduino ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="55268-108">You also run a gulp task on your computer to send messages to your Arduino board from your IoT hub.</span></span> <span data-ttu-id="55268-109">Zprávy přijetí ukázkovou aplikaci bliká Indikátor.</span><span class="sxs-lookup"><span data-stu-id="55268-109">When the sample application receives the messages, it blinks the LED.</span></span> <span data-ttu-id="55268-110">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="55268-110">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="55268-111">Co provedete</span><span class="sxs-lookup"><span data-stu-id="55268-111">What you will do</span></span>
* <span data-ttu-id="55268-112">Připojte ukázkovou aplikaci do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="55268-112">Connect the sample application to your IoT hub.</span></span>
* <span data-ttu-id="55268-113">Nasazení a spuštění ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="55268-113">Deploy and run the sample application.</span></span>
* <span data-ttu-id="55268-114">Odesílat zprávy ze služby IoT hub panel Arduino blikání Indikátor.</span><span class="sxs-lookup"><span data-stu-id="55268-114">Send messages from your IoT hub to your Arduino board to blink the LED.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="55268-115">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="55268-115">What you will learn</span></span>
<span data-ttu-id="55268-116">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="55268-116">In this article, you will learn:</span></span>
* <span data-ttu-id="55268-117">Postup sledování příchozích zpráv ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="55268-117">How to monitor incoming messages from your IoT hub.</span></span>
* <span data-ttu-id="55268-118">Postupy pro odesílání zpráv typu cloud zařízení ze služby IoT hub pro panel Arduino.</span><span class="sxs-lookup"><span data-stu-id="55268-118">How to send cloud-to-device messages from your IoT hub to your Arduino board.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="55268-119">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="55268-119">What you need</span></span>
* <span data-ttu-id="55268-120">Panel Arduino, nastavte pro použití.</span><span class="sxs-lookup"><span data-stu-id="55268-120">Your Arduino board, set up for use.</span></span> <span data-ttu-id="55268-121">Zjistěte, jak nastavit panel Arduino, najdete v tématu [konfigurace zařízení][configure-your-device].</span><span class="sxs-lookup"><span data-stu-id="55268-121">To learn how to set up your Arduino board, see [Configure your device][configure-your-device].</span></span>
* <span data-ttu-id="55268-122">Služby IoT hub, který se vytvoří ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="55268-122">An IoT hub that is created in your Azure subscription.</span></span> <span data-ttu-id="55268-123">Informace o vytvoření služby IoT hub, najdete v tématu [vytvoření služby Azure IoT Hub][create-your-azure-iot-hub].</span><span class="sxs-lookup"><span data-stu-id="55268-123">To learn how to create your IoT hub, see [Create your Azure IoT Hub][create-your-azure-iot-hub].</span></span>

## <a name="connect-the-sample-application-to-your-iot-hub"></a><span data-ttu-id="55268-124">Připojit ukázkovou aplikaci do služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="55268-124">Connect the sample application to your IoT hub</span></span>

1. <span data-ttu-id="55268-125">Ujistěte se, že jste ve složce úložišti `iot-hub-c-feather-m0-getting-started`.</span><span class="sxs-lookup"><span data-stu-id="55268-125">Make sure that you're in the repo folder `iot-hub-c-feather-m0-getting-started`.</span></span>

   <span data-ttu-id="55268-126">Otevřete ukázkovou aplikaci v aplikaci Visual Studio Code spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="55268-126">Open the sample application in Visual Studio Code by running the following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```

   <span data-ttu-id="55268-127">Upozornění `app.ino` v soubor `app` podsložky.</span><span class="sxs-lookup"><span data-stu-id="55268-127">Notice the `app.ino` file in the `app` subfolder.</span></span> <span data-ttu-id="55268-128">`app.ino` Soubor je soubor zdroj klíče, který obsahuje kód ke sledování příchozích zpráv ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="55268-128">The `app.ino` file is the key source file that contains the code to monitor incoming messages from the IoT hub.</span></span> <span data-ttu-id="55268-129">`blinkLED` Funkce bliká Indikátor.</span><span class="sxs-lookup"><span data-stu-id="55268-129">The `blinkLED` function blinks the LED.</span></span>

   ![Struktura úložišti v ukázkové aplikace][repo-structure]

2. <span data-ttu-id="55268-131">Získání sériového portu zařízení pomocí rozhraní příkazového řádku zjišťování zařízení:</span><span class="sxs-lookup"><span data-stu-id="55268-131">Obtain the serial port of the device with the device discovery cli:</span></span>

   ```bash
   devdisco list --usb
   ```

   <span data-ttu-id="55268-132">Měli byste zobrazený výstup, který je podobný následujícímu a najít usb COM port pro panel Arduino:</span><span class="sxs-lookup"><span data-stu-id="55268-132">You should see an output that is similar to the following and find the usb COM port for your Arduino board:</span></span>

   ![Zjišťování zařízení][device-discovery]

3. <span data-ttu-id="55268-134">Otevřete soubor `config.json` ve složce lekce a vstupní hodnotu nalezen číslo portu COM:</span><span class="sxs-lookup"><span data-stu-id="55268-134">Open the file `config.json` in the lesson folder and input the value of the found COM port number:</span></span>

   ```json
   {
       "device_port" : "COM1"
   }
   ```

   ![Config.JSON][config-json]

   > [!NOTE]
   > <span data-ttu-id="55268-136">Pro port COM, na platformě Windows má formát `COM1, COM2, ...`.</span><span class="sxs-lookup"><span data-stu-id="55268-136">For the COM port, on Windows platform, it has the format of `COM1, COM2, ...`.</span></span> <span data-ttu-id="55268-137">V systému macOS nebo Ubuntu, bude začínat `/dev/`.</span><span class="sxs-lookup"><span data-stu-id="55268-137">On macOS or Ubuntu, it will start with `/dev/`.</span></span>

4. <span data-ttu-id="55268-138">Inicializace konfiguračního souboru spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="55268-138">Initialize the configuration file by running the following commands:</span></span>

   ```bash
   # For Windows command prompt
   npm install
   gulp init
   gulp install-tools
   ```

5. <span data-ttu-id="55268-139">Zkontrolujte následující nahrazení v `config-arduino.json` souboru:</span><span class="sxs-lookup"><span data-stu-id="55268-139">Make the following replacements in the `config-arduino.json` file:</span></span>

   <span data-ttu-id="55268-140">Pokud jste dokončili kroky v [vytvoření účtu Azure funkce aplikace a úložiště] [ create-an-azure-function-app-and-storage-account] v tomto počítači se dědí všechny konfigurace, takže můžete přeskočit na krok úlohy nasazení a spuštění ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="55268-140">If you completed the steps in [Create an Azure function app and storage account][create-an-azure-function-app-and-storage-account] on this computer, all the configurations are inherited, so you can skip the step to the task of deploying and running the sample application.</span></span> <span data-ttu-id="55268-141">Pokud jste dokončili kroky v [vytvoření účtu Azure funkce aplikace a úložiště] [ create-an-azure-function-app-and-storage-account] na jiném počítači, budete muset nahraďte zástupné symboly v `config-arduino.json` souboru.</span><span class="sxs-lookup"><span data-stu-id="55268-141">If you completed the steps in [Create an Azure function app and storage account][create-an-azure-function-app-and-storage-account] on a different computer, you need to replace the placeholders in the `config-arduino.json` file.</span></span> <span data-ttu-id="55268-142">`config-arduino.json` Soubor je v podsložce domovskou složku.</span><span class="sxs-lookup"><span data-stu-id="55268-142">The `config-arduino.json` file is in the subfolder of your home folder.</span></span>

   ![Obsah souboru config arduino.json][config-arduino-json]

   * <span data-ttu-id="55268-144">Nahraďte **[Wi-Fi SSID]** s vaší SSID Wi-Fi, který připojení k Internetu.</span><span class="sxs-lookup"><span data-stu-id="55268-144">Replace **[Wi-Fi SSID]** with your Wi-Fi SSID that connected to the Internet.</span></span>
   * <span data-ttu-id="55268-145">Nahraďte **[Wi-Fi heslo]** pomocí hesla Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="55268-145">Replace **[Wi-Fi password]** with your Wi-Fi password.</span></span> <span data-ttu-id="55268-146">Odeberte řetězec, pokud Wi-Fi nevyžaduje heslo.</span><span class="sxs-lookup"><span data-stu-id="55268-146">Remove the string if your Wi-Fi doesn't require password.</span></span>
   * <span data-ttu-id="55268-147">Nahraďte **[řetězec připojení zařízení IoT]** připojovacím řetězcem zařízení, kterou můžete získat spuštěním `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` příkaz.</span><span class="sxs-lookup"><span data-stu-id="55268-147">Replace **[IoT device connection string]** with the device connection string that you get by running the `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` command.</span></span>
   * <span data-ttu-id="55268-148">Nahraďte **[IoT hub, připojovací řetězec]** s IoT hub připojovací řetězec, který získáte spuštěním `az iot hub show-connection-string --name {my hub name}` příkaz.</span><span class="sxs-lookup"><span data-stu-id="55268-148">Replace **[IoT hub connection string]** with the IoT hub connection string that you get by running the `az iot hub show-connection-string --name {my hub name}` command.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="55268-149">Nasazení a spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="55268-149">Deploy and run the sample application</span></span>
<span data-ttu-id="55268-150">Nasazení a spuštění ukázkové aplikace na panel Arduino spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="55268-150">Deploy and run the sample application on your Arduino board by running the following commands:</span></span>

```bash
gulp run
# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

<span data-ttu-id="55268-151">Příkaz gulp nasadí ukázkovou aplikaci pro panel Arduino.</span><span class="sxs-lookup"><span data-stu-id="55268-151">The gulp command deploys the sample application to your Arduino board.</span></span> <span data-ttu-id="55268-152">Pak spustí aplikaci na panel Arduino a samostatná úloha v hostitelském počítači odesílat zprávy 20 blikání panel Arduino ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="55268-152">Then, it runs the application on your Arduino board and a separate task on your host computer to send 20 blink messages to your Arduino board from your IoT hub.</span></span>

<span data-ttu-id="55268-153">Po spuštění ukázkové aplikace, začne naslouchání zpráv ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="55268-153">After the sample application runs, it starts listening to messages from your IoT hub.</span></span> <span data-ttu-id="55268-154">Úloha gulp mezitím, odešle do panel Arduino několik "blink" zpráv ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="55268-154">Meanwhile, the gulp task sends several "blink" messages from your IoT hub to your Arduino board.</span></span> <span data-ttu-id="55268-155">Pro každou blikání zprávu, která přijímá panelu, volá ukázkovou aplikaci `blinkLED` funkce blikání Indikátor.</span><span class="sxs-lookup"><span data-stu-id="55268-155">For each blink message that the board receives, the sample application calls the `blinkLED` function to blink the LED.</span></span>

<span data-ttu-id="55268-156">Jako úlohu gulp odešle 20 zpráv ze služby IoT hub panel Arduino, měli byste vidět blikání DIODU každé dvě sekundy.</span><span class="sxs-lookup"><span data-stu-id="55268-156">You should see the LED blink every two seconds as the gulp task sends 20 messages from your IoT hub to your Arduino board.</span></span> <span data-ttu-id="55268-157">Poslední je "stop" zprávy, která zabrání spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="55268-157">The last one is a "stop" message that stops the application from running.</span></span>

![Ukázkovou aplikaci s gulp příkazu a blink – zprávy][sample-application]

## <a name="summary"></a><span data-ttu-id="55268-159">Souhrn</span><span class="sxs-lookup"><span data-stu-id="55268-159">Summary</span></span>
<span data-ttu-id="55268-160">Úspěšně jste odeslali zprávy ze služby IoT hub do vaší karty Arduino blikání Indikátor.</span><span class="sxs-lookup"><span data-stu-id="55268-160">You’ve successfully sent messages from your IoT hub to your Arduino board to blink the LED.</span></span> <span data-ttu-id="55268-161">Dalším krokem je volitelné: Změna zapnuto a vypnuto chování Indikátor.</span><span class="sxs-lookup"><span data-stu-id="55268-161">The next task is optional: change the on and off behavior of the LED.</span></span>

## <a name="next-steps"></a><span data-ttu-id="55268-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="55268-162">Next steps</span></span>
<span data-ttu-id="55268-163">[Změnit zapnuto a vypnuto chování Indikátor][change-the-on-and-off-led-behavior]</span><span class="sxs-lookup"><span data-stu-id="55268-163">[Change the on and off behavior of the LED][change-the-on-and-off-led-behavior]</span></span>


<!-- Images and links -->

[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[configure-your-device]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-configure-your-device.md
[create-your-azure-iot-hub]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/repo_structure_arduino.png
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[create-an-azure-function-app-and-storage-account]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md
[config-arduino-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/config-arduino.png
[sample-application]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/gulp_blink_arduino.png
[change-the-on-and-off-led-behavior]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-change-led-behavior.md