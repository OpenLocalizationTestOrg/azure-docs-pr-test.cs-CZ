---
title: "Malinová Pi (C) připojit k Azure IoT - Lekce 1: nasazení aplikace | Microsoft Docs"
description: "Klonování C ukázkovou aplikaci z webu GitHub a gulp k nasazení této aplikace na panel malin pí 3. Tato ukázková aplikace bliká DIODU připojené k panelu každé dvě sekundy."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Malinová platformy vyvolala blikání, blikání vedla s Malinová platformy"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: f601d1e1-2bc3-4cc5-a6b1-0467e5304dcf
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 2ae409c6a39521711777ec329d2507a2801cc985
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-deploy-the-blink-application"></a><span data-ttu-id="032df-105">Vytvoření a nasazení aplikace blink</span><span class="sxs-lookup"><span data-stu-id="032df-105">Create and deploy the blink application</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="032df-106">Co provedete</span><span class="sxs-lookup"><span data-stu-id="032df-106">What you will do</span></span>
<span data-ttu-id="032df-107">Klonování C ukázkovou aplikaci z webu GitHub a pomocí nástroje gulp nasadit ukázkovou aplikaci pro malin pí 3.</span><span class="sxs-lookup"><span data-stu-id="032df-107">Clone the sample C application from GitHub, and use the gulp tool to deploy the sample application to Raspberry Pi 3.</span></span> <span data-ttu-id="032df-108">Ukázkovou aplikaci bliká DIODU připojené k panelu každé dvě sekundy.</span><span class="sxs-lookup"><span data-stu-id="032df-108">The sample application blinks the LED connected to the board every two seconds.</span></span> <span data-ttu-id="032df-109">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="032df-109">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="032df-110">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="032df-110">What you will learn</span></span>
<span data-ttu-id="032df-111">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="032df-111">In this article, you will learn:</span></span>

* <span data-ttu-id="032df-112">Postup použití `device-discover-cli` nástroj k načtení sítě informace o pí.</span><span class="sxs-lookup"><span data-stu-id="032df-112">How to use the `device-discover-cli` tool to retrieve networking information about Pi.</span></span>
* <span data-ttu-id="032df-113">Postup nasazení a spuštění ukázkové aplikace na pí.</span><span class="sxs-lookup"><span data-stu-id="032df-113">How to deploy and run the sample application on Pi.</span></span>
* <span data-ttu-id="032df-114">Postup nasazení a ladění aplikací běžících na platformy vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="032df-114">How to deploy and debug applications running remotely on Pi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="032df-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="032df-115">What you need</span></span>
<span data-ttu-id="032df-116">Musíte úspěšně jste dokončili následující operace:</span><span class="sxs-lookup"><span data-stu-id="032df-116">You must have successfully completed the following operations:</span></span>

* [<span data-ttu-id="032df-117">Konfigurace zařízení</span><span class="sxs-lookup"><span data-stu-id="032df-117">Configure your device</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-configure-your-device.md)
* [<span data-ttu-id="032df-118">Získat nástroje</span><span class="sxs-lookup"><span data-stu-id="032df-118">Get the tools</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)

## <a name="obtain-the-ip-address-and-host-name-of-pi"></a><span data-ttu-id="032df-119">Získat IP adresu a hostitele název platformy</span><span class="sxs-lookup"><span data-stu-id="032df-119">Obtain the IP address and host name of Pi</span></span>
<span data-ttu-id="032df-120">Otevřete příkazový řádek v systému Windows nebo terminál v systému macOS nebo Ubuntu a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="032df-120">Open a command prompt in Windows or a terminal in macOS or Ubuntu, and then run the following command:</span></span>

```bash
devdisco list --eth
```

<span data-ttu-id="032df-121">Měli byste vidět výstup podobný tomuto:</span><span class="sxs-lookup"><span data-stu-id="032df-121">You should see an output that is similar to the following:</span></span>

![Zjišťování zařízení](media/iot-hub-raspberry-pi-lessons/lesson1/device_discovery.png)

<span data-ttu-id="032df-123">Poznamenejte si `IP address` a `hostname` pí.</span><span class="sxs-lookup"><span data-stu-id="032df-123">Take note of the `IP address` and `hostname` of Pi.</span></span> <span data-ttu-id="032df-124">Je třeba tyto informace dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="032df-124">You need this information later in this article.</span></span>

> [!NOTE]
> <span data-ttu-id="032df-125">Ujistěte se, že platformy je připojený ke stejné síti jako počítače.</span><span class="sxs-lookup"><span data-stu-id="032df-125">Make sure that Pi is connected to the same network as your computer.</span></span> <span data-ttu-id="032df-126">Například pokud je počítač připojen k bezdrátové síti a platformy je připojeno k drátové síti, nemusíte to vidět IP adresu ve výstupu devdisco.</span><span class="sxs-lookup"><span data-stu-id="032df-126">For example, if your computer is connected to a wireless network while Pi is connected to a wired network, you might not see the IP address in the devdisco output.</span></span>

## <a name="open-the-sample-application"></a><span data-ttu-id="032df-127">Otevřete ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="032df-127">Open the sample application</span></span>
<span data-ttu-id="032df-128">Chcete-li otevřít ukázkové aplikace, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="032df-128">To open the sample application, follow these steps:</span></span>

1. <span data-ttu-id="032df-129">Naklonujte úložiště ukázkové z Githubu spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="032df-129">Clone the sample repository from GitHub by running the following command:</span></span>
   
    ```bash
    git clone https://github.com/Azure-Samples/iot-hub-c-raspberrypi-getting-started.git
    ```
2. <span data-ttu-id="032df-130">Otevřete ukázkovou aplikaci v aplikaci Visual Studio Code spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="032df-130">Open the sample application in Visual Studio Code by running the following commands:</span></span>
   
    ```bash
    cd iot-hub-c-raspberrypi-getting-started
    cd Lesson1
    code .
    ```

![Struktura úložiště](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-blink-c-mac.png)

<span data-ttu-id="032df-132">`main.c` v soubor `app` podsložky je klíče zdrojového souboru, který obsahuje kód, který ovládacího prvku Indikátor.</span><span class="sxs-lookup"><span data-stu-id="032df-132">The `main.c` file in the `app` subfolder is the key source file that contains the code to control the LED.</span></span>

### <a name="install-application-dependencies"></a><span data-ttu-id="032df-133">Instalace závislostí aplikací</span><span class="sxs-lookup"><span data-stu-id="032df-133">Install application dependencies</span></span>
<span data-ttu-id="032df-134">Nainstalujte v knihovnách a dalších modulů, které potřebujete pro ukázkovou aplikaci tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="032df-134">Install the libraries and other modules you need for the sample application by running the following command:</span></span>

```bash
npm install
```

## <a name="configure-the-device-connection"></a><span data-ttu-id="032df-135">Nakonfigurujte připojení zařízení</span><span class="sxs-lookup"><span data-stu-id="032df-135">Configure the device connection</span></span>
<span data-ttu-id="032df-136">Konfigurace připojení zařízení, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="032df-136">To configure the device connection, follow these steps:</span></span>

1. <span data-ttu-id="032df-137">Generujte konfiguračního souboru zařízení tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="032df-137">Generate the device configuration file by running the following command:</span></span>
   
   ```bash
   gulp init
   ```
   
   <span data-ttu-id="032df-138">Konfigurační soubor `config-raspberrypi.json` obsahuje uživatelská pověření, které používáte k přihlášení do pí.</span><span class="sxs-lookup"><span data-stu-id="032df-138">The configuration file `config-raspberrypi.json` contains the user credentials you use to log in to Pi.</span></span> <span data-ttu-id="032df-139">Aby se zabránilo úniku přihlašovacích údajů uživatele, je generována konfiguračního souboru v podsložce `.iot-hub-getting-started` domovské složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="032df-139">To avoid the leak of user credentials, the configuration file is generated in the subfolder `.iot-hub-getting-started` of the home folder on your computer.</span></span>

2. <span data-ttu-id="032df-140">Otevřete konfigurační soubor zařízení v aplikaci Visual Studio Code spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="032df-140">Open the device configuration file in Visual Studio Code by running the following command:</span></span>
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```

3. <span data-ttu-id="032df-141">Nahraďte zástupný symbol `[device hostname or IP address]` se IP adresa nebo název hostitele, který jste získali dříve v "získat IP adresu a hostitele název pí."</span><span class="sxs-lookup"><span data-stu-id="032df-141">Replace the placeholder `[device hostname or IP address]` with the IP address or the host name that you got previously in "Obtain the IP address and host name of Pi."</span></span>
   
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-config-mac.png)

> [!NOTE]
> <span data-ttu-id="032df-143">Při připojování k malin platformy, můžete použít klíč SSH místo uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="032df-143">You can use SSH key instead of user name and password when connecting to Raspberry Pi.</span></span> <span data-ttu-id="032df-144">Pokud to chcete provést budete muset vygenerovat klíč pomocí **ssh-keygen** a **ssh kopie id platformy @\<adresa zařízení\>**.</span><span class="sxs-lookup"><span data-stu-id="032df-144">In order to do this you will have to generate the key using **ssh-keygen** and **ssh-copy-id pi@\<device address\>**.</span></span>
>
> <span data-ttu-id="032df-145">V systému Windows jsou k dispozici v těchto příkazů **Git Bash**.</span><span class="sxs-lookup"><span data-stu-id="032df-145">On Windows these commands are available in **Git Bash**.</span></span>
>
> <span data-ttu-id="032df-146">V systému MacOS, budete muset spustit **brew nainstalovat ssh kopie id**.</span><span class="sxs-lookup"><span data-stu-id="032df-146">On MacOS you need to run **brew install ssh-copy-id**.</span></span>
>
> <span data-ttu-id="032df-147">Po nahrání úspěšně klíč malin PI, nahraďte **device_password** s **device_key_path** vlastnost **konfigurace raspberrypi.json**.</span><span class="sxs-lookup"><span data-stu-id="032df-147">After successfully uploading the key to the Raspberry Pi, replace **device_password** with **device_key_path** property in **config-raspberrypi.json**.</span></span>
>
> <span data-ttu-id="032df-148">Aktualizované řádky by měl vypadat jako níže:</span><span class="sxs-lookup"><span data-stu-id="032df-148">Updated lines should look as below:</span></span>
> ```javascript
> "device_user_name": "pi",
> "device_key_path": "id_rsa",
> ```

<span data-ttu-id="032df-149">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="032df-149">Congratulations!</span></span> <span data-ttu-id="032df-150">Úspěšně jste vytvořili první ukázkovou aplikaci pro platformy.</span><span class="sxs-lookup"><span data-stu-id="032df-150">You've successfully created the first sample application for Pi.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="032df-151">Nasazení a spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="032df-151">Deploy and run the sample application</span></span>
### <a name="install-the-azure-iot-hub-sdk-on-pi"></a><span data-ttu-id="032df-152">Nainstalujte službu Azure IoT Hub SDK na platformy</span><span class="sxs-lookup"><span data-stu-id="032df-152">Install the Azure IoT Hub SDK on Pi</span></span>
<span data-ttu-id="032df-153">Instalace sady SDK Azure IoT Hub na platformy spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="032df-153">Install the Azure IoT Hub SDK on Pi by running the following command:</span></span>

```bash
gulp install-tools
```

<span data-ttu-id="032df-154">Tato úloha může trvat několik minut na dokončení při prvním spuštění.</span><span class="sxs-lookup"><span data-stu-id="032df-154">This task might take a few minutes to complete the first time you run it.</span></span>

### <a name="deploy-and-run-the-sample-app"></a><span data-ttu-id="032df-155">Nasazení a spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="032df-155">Deploy and run the sample app</span></span>
<span data-ttu-id="032df-156">Nasazení a spuštění ukázkové aplikace tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="032df-156">Deploy and run the sample application by running the following command:</span></span>

```bash
gulp deploy && gulp run
```

### <a name="verify-the-app-works"></a><span data-ttu-id="032df-157">Ověřte aplikace funguje</span><span class="sxs-lookup"><span data-stu-id="032df-157">Verify the app works</span></span>
<span data-ttu-id="032df-158">Ukázkovou aplikaci se po bliká Indikátor pro 20krát automaticky ukončí.</span><span class="sxs-lookup"><span data-stu-id="032df-158">The sample application terminates automatically after the LED blinks for 20 times.</span></span> <span data-ttu-id="032df-159">Pokud nevidíte DIODU blikat, najdete v článku [Průvodce odstraňováním potíží s](iot-hub-raspberry-pi-kit-c-troubleshooting.md) pro řešení běžných potíží.</span><span class="sxs-lookup"><span data-stu-id="032df-159">If you don’t see the LED blinking, see the [troubleshooting guide](iot-hub-raspberry-pi-kit-c-troubleshooting.md) for solutions to common problems.</span></span>
<span data-ttu-id="032df-160">![Indikátor blikat](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)</span><span class="sxs-lookup"><span data-stu-id="032df-160">![LED blinking](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)</span></span>

## <a name="summary"></a><span data-ttu-id="032df-161">Souhrn</span><span class="sxs-lookup"><span data-stu-id="032df-161">Summary</span></span>
<span data-ttu-id="032df-162">Nainstalujete požadované nástroje pro práci s platformy a nasadit ukázkovou aplikaci pro platformy blikání Indikátor.</span><span class="sxs-lookup"><span data-stu-id="032df-162">You've installed the required tools to work with Pi and deployed a sample application to Pi to blink the LED.</span></span> <span data-ttu-id="032df-163">Teď můžete vytvořit, nasadit a spustit jinou ukázkovou aplikaci, která připojuje platformy Azure IoT Hub odesílat a přijímat zprávy.</span><span class="sxs-lookup"><span data-stu-id="032df-163">You can now create, deploy, and run another sample application that connects Pi to Azure IoT Hub to send and receive messages.</span></span>

## <a name="next-steps"></a><span data-ttu-id="032df-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="032df-164">Next steps</span></span>
[<span data-ttu-id="032df-165">Získat nástroje Azure</span><span class="sxs-lookup"><span data-stu-id="032df-165">Get Azure tools</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md)

