---
title: "Connect Arduino (C) tooAzure IoT - Lekce 4: Cloud zařízení | Microsoft Docs"
description: "Ukázková aplikace spustí na Adafruit prolnutí M0 Wi-Fi a monitorování příchozích zpráv ze služby IoT hub. Nová úloha gulp odešle zprávy tooAdafruit prolnutí M0 Wi-Fi z vaší IoT hub tooblink hello Indikátor."
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
ms.openlocfilehash: dcddd61ff684f49436103675938d719cb227c409
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-tooreceive-cloud-to-device-messages"></a><span data-ttu-id="f299f-105">Spuštění ukázkové aplikace tooreceive zprávy typu cloud zařízení</span><span class="sxs-lookup"><span data-stu-id="f299f-105">Run a sample application tooreceive cloud-to-device messages</span></span>
<span data-ttu-id="f299f-106">V tomto článku nasazení ukázkové aplikace na vaší kartě Adafruit prolnutí M0 Wi-Fi Arduino.</span><span class="sxs-lookup"><span data-stu-id="f299f-106">In this article, you deploy a sample application on your Adafruit Feather M0 WiFi Arduino board.</span></span>

<span data-ttu-id="f299f-107">Ukázková aplikace Hello monitoruje příchozích zpráv ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="f299f-107">hello sample application monitors incoming messages from your IoT hub.</span></span> <span data-ttu-id="f299f-108">Také spustit úlohu gulp na váš počítač toosend zprávy tooyour Arduino Tabule ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="f299f-108">You also run a gulp task on your computer toosend messages tooyour Arduino board from your IoT hub.</span></span> <span data-ttu-id="f299f-109">Zprávy hello přijetí hello ukázkovou aplikaci bliká DIODU hello.</span><span class="sxs-lookup"><span data-stu-id="f299f-109">When hello sample application receives hello messages, it blinks hello LED.</span></span> <span data-ttu-id="f299f-110">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="f299f-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="f299f-111">Co provedete</span><span class="sxs-lookup"><span data-stu-id="f299f-111">What you will do</span></span>
* <span data-ttu-id="f299f-112">Připojte hello ukázkové aplikace tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="f299f-112">Connect hello sample application tooyour IoT hub.</span></span>
* <span data-ttu-id="f299f-113">Nasazení a spuštění hello ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f299f-113">Deploy and run hello sample application.</span></span>
* <span data-ttu-id="f299f-114">Odesílat zprávy z vaší IoT hub tooyour Arduino Tabule tooblink hello Indikátor.</span><span class="sxs-lookup"><span data-stu-id="f299f-114">Send messages from your IoT hub tooyour Arduino board tooblink hello LED.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="f299f-115">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="f299f-115">What you will learn</span></span>
<span data-ttu-id="f299f-116">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="f299f-116">In this article, you will learn:</span></span>
* <span data-ttu-id="f299f-117">Jak toomonitor příchozích zpráv ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="f299f-117">How toomonitor incoming messages from your IoT hub.</span></span>
* <span data-ttu-id="f299f-118">Jak toosend cloud zařízení zprávy z vaší IoT hub tooyour Arduino panelu.</span><span class="sxs-lookup"><span data-stu-id="f299f-118">How toosend cloud-to-device messages from your IoT hub tooyour Arduino board.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="f299f-119">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="f299f-119">What you need</span></span>
* <span data-ttu-id="f299f-120">Panel Arduino, nastavte pro použití.</span><span class="sxs-lookup"><span data-stu-id="f299f-120">Your Arduino board, set up for use.</span></span> <span data-ttu-id="f299f-121">jak zjistit, tooset až vaší karty Arduino toolearn [konfigurace zařízení][configure-your-device].</span><span class="sxs-lookup"><span data-stu-id="f299f-121">toolearn how tooset up your Arduino board, see [Configure your device][configure-your-device].</span></span>
* <span data-ttu-id="f299f-122">Služby IoT hub, který se vytvoří ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="f299f-122">An IoT hub that is created in your Azure subscription.</span></span> <span data-ttu-id="f299f-123">toolearn jak toocreate služby IoT hub, najdete v části [vytvoření služby Azure IoT Hub][create-your-azure-iot-hub].</span><span class="sxs-lookup"><span data-stu-id="f299f-123">toolearn how toocreate your IoT hub, see [Create your Azure IoT Hub][create-your-azure-iot-hub].</span></span>

## <a name="connect-hello-sample-application-tooyour-iot-hub"></a><span data-ttu-id="f299f-124">Připojit hello ukázkové aplikace tooyour IoT hub</span><span class="sxs-lookup"><span data-stu-id="f299f-124">Connect hello sample application tooyour IoT hub</span></span>

1. <span data-ttu-id="f299f-125">Ujistěte se, že jste ve složce úložišti hello `iot-hub-c-feather-m0-getting-started`.</span><span class="sxs-lookup"><span data-stu-id="f299f-125">Make sure that you're in hello repo folder `iot-hub-c-feather-m0-getting-started`.</span></span>

   <span data-ttu-id="f299f-126">Otevřete hello ukázkovou aplikaci v aplikaci Visual Studio Code spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="f299f-126">Open hello sample application in Visual Studio Code by running hello following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```

   <span data-ttu-id="f299f-127">Všimněte si hello `app.ino` souboru v hello `app` podsložky.</span><span class="sxs-lookup"><span data-stu-id="f299f-127">Notice hello `app.ino` file in hello `app` subfolder.</span></span> <span data-ttu-id="f299f-128">Hello `app.ino` soubor je soubor hello zdroj klíče, který obsahuje hello kód toomonitor příchozí zprávy ze služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="f299f-128">hello `app.ino` file is hello key source file that contains hello code toomonitor incoming messages from hello IoT hub.</span></span> <span data-ttu-id="f299f-129">Hello `blinkLED` funkce bliká DIODU hello.</span><span class="sxs-lookup"><span data-stu-id="f299f-129">hello `blinkLED` function blinks hello LED.</span></span>

   ![Struktura úložišti v ukázkové aplikaci hello][repo-structure]

2. <span data-ttu-id="f299f-131">Získejte hello sériového portu zařízení hello s hello zařízení zjišťování rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="f299f-131">Obtain hello serial port of hello device with hello device discovery cli:</span></span>

   ```bash
   devdisco list --usb
   ```

   <span data-ttu-id="f299f-132">Měli byste zobrazený výstup, který je podobný toohello následující a najít hello usb COM port pro panel Arduino:</span><span class="sxs-lookup"><span data-stu-id="f299f-132">You should see an output that is similar toohello following and find hello usb COM port for your Arduino board:</span></span>

   ![Zjišťování zařízení][device-discovery]

3. <span data-ttu-id="f299f-134">Soubor otevřete hello `config.json` ve složce lekce hello a hodnotu vstupního hello hello najít číslo portu COM:</span><span class="sxs-lookup"><span data-stu-id="f299f-134">Open hello file `config.json` in hello lesson folder and input hello value of hello found COM port number:</span></span>

   ```json
   {
       "device_port" : "COM1"
   }
   ```

   ![Config.JSON][config-json]

   > [!NOTE]
   > <span data-ttu-id="f299f-136">Pro port hello COM na platformě Windows má formát hello `COM1, COM2, ...`.</span><span class="sxs-lookup"><span data-stu-id="f299f-136">For hello COM port, on Windows platform, it has hello format of `COM1, COM2, ...`.</span></span> <span data-ttu-id="f299f-137">V systému macOS nebo Ubuntu, bude začínat `/dev/`.</span><span class="sxs-lookup"><span data-stu-id="f299f-137">On macOS or Ubuntu, it will start with `/dev/`.</span></span>

4. <span data-ttu-id="f299f-138">Inicializovat konfigurační soubor hello spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="f299f-138">Initialize hello configuration file by running hello following commands:</span></span>

   ```bash
   # For Windows command prompt
   npm install
   gulp init
   gulp install-tools
   ```

5. <span data-ttu-id="f299f-139">Ujistěte se, hello následující nahrazení v hello `config-arduino.json` souboru:</span><span class="sxs-lookup"><span data-stu-id="f299f-139">Make hello following replacements in hello `config-arduino.json` file:</span></span>

   <span data-ttu-id="f299f-140">Pokud jste dokončili postup hello v [vytvoření účtu Azure funkce aplikace a úložiště] [ create-an-azure-function-app-and-storage-account] v tomto počítači se dědí všechny konfigurace hello, takže hello krok toohello úloh nasazení, můžete přeskočit a spuštění ukázkové aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="f299f-140">If you completed hello steps in [Create an Azure function app and storage account][create-an-azure-function-app-and-storage-account] on this computer, all hello configurations are inherited, so you can skip hello step toohello task of deploying and running hello sample application.</span></span> <span data-ttu-id="f299f-141">Pokud jste dokončili postup hello v [vytvoření účtu Azure funkce aplikace a úložiště] [ create-an-azure-function-app-and-storage-account] na jiném počítači, musíte tooreplace hello zástupné symboly v hello `config-arduino.json` souboru.</span><span class="sxs-lookup"><span data-stu-id="f299f-141">If you completed hello steps in [Create an Azure function app and storage account][create-an-azure-function-app-and-storage-account] on a different computer, you need tooreplace hello placeholders in hello `config-arduino.json` file.</span></span> <span data-ttu-id="f299f-142">Hello `config-arduino.json` soubor je v podsložce hello domovskou složku.</span><span class="sxs-lookup"><span data-stu-id="f299f-142">hello `config-arduino.json` file is in hello subfolder of your home folder.</span></span>

   ![Obsah souboru config arduino.json hello][config-arduino-json]

   * <span data-ttu-id="f299f-144">Nahraďte **[Wi-Fi SSID]** s vaší SSID Wi-Fi, který připojený toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="f299f-144">Replace **[Wi-Fi SSID]** with your Wi-Fi SSID that connected toohello Internet.</span></span>
   * <span data-ttu-id="f299f-145">Nahraďte **[Wi-Fi heslo]** pomocí hesla Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="f299f-145">Replace **[Wi-Fi password]** with your Wi-Fi password.</span></span> <span data-ttu-id="f299f-146">Odeberte hello řetězec, pokud Wi-Fi nevyžaduje heslo.</span><span class="sxs-lookup"><span data-stu-id="f299f-146">Remove hello string if your Wi-Fi doesn't require password.</span></span>
   * <span data-ttu-id="f299f-147">Nahraďte **[řetězec připojení zařízení IoT]** s hello zařízení připojovací řetězec, který můžete získat spuštěním hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` příkaz.</span><span class="sxs-lookup"><span data-stu-id="f299f-147">Replace **[IoT device connection string]** with hello device connection string that you get by running hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` command.</span></span>
   * <span data-ttu-id="f299f-148">Nahraďte **[IoT hub, připojovací řetězec]** s hello IoT hub připojovací řetězec, který můžete získat spuštěním hello `az iot hub show-connection-string --name {my hub name}` příkaz.</span><span class="sxs-lookup"><span data-stu-id="f299f-148">Replace **[IoT hub connection string]** with hello IoT hub connection string that you get by running hello `az iot hub show-connection-string --name {my hub name}` command.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="f299f-149">Nasazení a spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="f299f-149">Deploy and run hello sample application</span></span>
<span data-ttu-id="f299f-150">Nasazení a spuštění ukázkové aplikace hello na panel Arduino spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="f299f-150">Deploy and run hello sample application on your Arduino board by running hello following commands:</span></span>

```bash
gulp run
# You can monitor hello serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

<span data-ttu-id="f299f-151">příkaz gulp Hello nasadí hello ukázkové aplikace tooyour Arduino panelu.</span><span class="sxs-lookup"><span data-stu-id="f299f-151">hello gulp command deploys hello sample application tooyour Arduino board.</span></span> <span data-ttu-id="f299f-152">Pak je spuštěna aplikace hello na panel Arduino a samostatná úloha na hostiteli toosend 20 blikání zprávy tooyour Arduino základní desky z služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="f299f-152">Then, it runs hello application on your Arduino board and a separate task on your host computer toosend 20 blink messages tooyour Arduino board from your IoT hub.</span></span>

<span data-ttu-id="f299f-153">Po spuštění ukázkové aplikace hello začne naslouchat toomessages ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="f299f-153">After hello sample application runs, it starts listening toomessages from your IoT hub.</span></span> <span data-ttu-id="f299f-154">Mezitím hello gulp úloh odešle několik "blink" zprávy z vaší IoT hub tooyour Arduino panelu.</span><span class="sxs-lookup"><span data-stu-id="f299f-154">Meanwhile, hello gulp task sends several "blink" messages from your IoT hub tooyour Arduino board.</span></span> <span data-ttu-id="f299f-155">Pro každou zprávu blikání, který hello Tabule obdrží, hello ukázkové aplikace volá hello `blinkLED` funkce tooblink hello Indikátor.</span><span class="sxs-lookup"><span data-stu-id="f299f-155">For each blink message that hello board receives, hello sample application calls hello `blinkLED` function tooblink hello LED.</span></span>

<span data-ttu-id="f299f-156">Měli byste vidět hello DIODU jako úloha gulp hello odešle 20 zprávy z vaší IoT hub tooyour Arduino Tabule blink každé dvě sekundy.</span><span class="sxs-lookup"><span data-stu-id="f299f-156">You should see hello LED blink every two seconds as hello gulp task sends 20 messages from your IoT hub tooyour Arduino board.</span></span> <span data-ttu-id="f299f-157">Hello poslední jedním je zpráva "stop", která ukončí hello spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="f299f-157">hello last one is a "stop" message that stops hello application from running.</span></span>

![Ukázkovou aplikaci s gulp příkazu a blink – zprávy][sample-application]

## <a name="summary"></a><span data-ttu-id="f299f-159">Souhrn</span><span class="sxs-lookup"><span data-stu-id="f299f-159">Summary</span></span>
<span data-ttu-id="f299f-160">Z vaší IoT hub tooyour Arduino Tabule tooblink hello DIODU úspěšně odeslat zprávy.</span><span class="sxs-lookup"><span data-stu-id="f299f-160">You’ve successfully sent messages from your IoT hub tooyour Arduino board tooblink hello LED.</span></span> <span data-ttu-id="f299f-161">Další úlohou Hello je volitelné: Změna hello zapnout a vypnout chování hello Indikátor.</span><span class="sxs-lookup"><span data-stu-id="f299f-161">hello next task is optional: change hello on and off behavior of hello LED.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f299f-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f299f-162">Next steps</span></span>
<span data-ttu-id="f299f-163">[Změnit hello zapnout a vypnout chování hello DIODU][change-the-on-and-off-led-behavior]</span><span class="sxs-lookup"><span data-stu-id="f299f-163">[Change hello on and off behavior of hello LED][change-the-on-and-off-led-behavior]</span></span>


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