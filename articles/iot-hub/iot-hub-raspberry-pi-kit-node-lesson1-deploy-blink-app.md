---
featureFlags: usabilla
title: "Připojení k Azure IoT - Lekce 1 Malinová platformy (uzel): nasazení aplikace | Microsoft Docs"
description: "Naklonujte ukázkovou aplikaci Node.js z Githubu a gulp k nasazení této aplikace na panel malin pí 3. Tato ukázková aplikace bliká DIODU připojené k panelu každé dvě sekundy."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Malinová platformy vyvolala blikání, blikání vedla s Malinová platformy"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: a5a03a57-fe86-416f-90ff-6eca17775842
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 8b73000c166950172c07b8e188025dc9da5bc011
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-deploy-the-blink-application"></a><span data-ttu-id="42b8b-105">Vytvoření a nasazení aplikace blink</span><span class="sxs-lookup"><span data-stu-id="42b8b-105">Create and deploy the blink application</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="42b8b-106">Co provedete</span><span class="sxs-lookup"><span data-stu-id="42b8b-106">What you will do</span></span>
<span data-ttu-id="42b8b-107">Naklonujte ukázkovou aplikaci Node.js z Githubu a pomocí nástroje gulp nasadit ukázkovou aplikaci pro váš malin pí 3.</span><span class="sxs-lookup"><span data-stu-id="42b8b-107">Clone the sample Node.js application from GitHub and use the gulp tool to deploy the sample application to your Raspberry Pi 3.</span></span> <span data-ttu-id="42b8b-108">Ukázkovou aplikaci bliká DIODU připojené k panelu každé dvě sekundy.</span><span class="sxs-lookup"><span data-stu-id="42b8b-108">The sample application blinks the LED connected to the board every two seconds.</span></span> <span data-ttu-id="42b8b-109">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="42b8b-109">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="42b8b-110">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="42b8b-110">What you will learn</span></span>
<span data-ttu-id="42b8b-111">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="42b8b-111">In this article, you will learn:</span></span>

* <span data-ttu-id="42b8b-112">Postup použití `device-discover-cli` nástroj k načtení sítě informace o pí.</span><span class="sxs-lookup"><span data-stu-id="42b8b-112">How to use the `device-discover-cli` tool to retrieve networking information about Pi.</span></span>
* <span data-ttu-id="42b8b-113">Postup nasazení a spuštění ukázkové aplikace na pí.</span><span class="sxs-lookup"><span data-stu-id="42b8b-113">How to deploy and run the sample application on Pi.</span></span>
* <span data-ttu-id="42b8b-114">Postup nasazení a ladění aplikací běžících na platformy vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="42b8b-114">How to deploy and debug applications running remotely on Pi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="42b8b-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="42b8b-115">What you need</span></span>
<span data-ttu-id="42b8b-116">Musíte úspěšně jste dokončili následující operace:</span><span class="sxs-lookup"><span data-stu-id="42b8b-116">You must have successfully completed the following operations:</span></span>

* [<span data-ttu-id="42b8b-117">Konfigurace zařízení</span><span class="sxs-lookup"><span data-stu-id="42b8b-117">Configure your device</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md)
* [<span data-ttu-id="42b8b-118">Získat nástroje</span><span class="sxs-lookup"><span data-stu-id="42b8b-118">Get the tools</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

## <a name="obtain-the-ip-address-and-host-name-of-pi"></a><span data-ttu-id="42b8b-119">Získat IP adresu a hostitele název platformy</span><span class="sxs-lookup"><span data-stu-id="42b8b-119">Obtain the IP address and host name of Pi</span></span>
<span data-ttu-id="42b8b-120">Otevřete příkazový řádek v systému Windows nebo terminál v systému macOS nebo Ubuntu a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="42b8b-120">Open a command prompt in Windows or a terminal in macOS or Ubuntu, and then run the following command:</span></span>

```bash
devdisco list --eth
```

<span data-ttu-id="42b8b-121">Měli byste vidět výstup podobný tomuto:</span><span class="sxs-lookup"><span data-stu-id="42b8b-121">You should see an output that is similar to the following:</span></span>

![Zjišťování zařízení](media/iot-hub-raspberry-pi-lessons/lesson1/device_discovery.png)

<span data-ttu-id="42b8b-123">Poznamenejte si `IP address` a `hostname` pí.</span><span class="sxs-lookup"><span data-stu-id="42b8b-123">Take note of the `IP address` and `hostname` of Pi.</span></span> <span data-ttu-id="42b8b-124">Je třeba tyto informace dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="42b8b-124">You need this information later in this article.</span></span>

> [!NOTE]
> <span data-ttu-id="42b8b-125">Ujistěte se, že platformy je připojený ke stejné síti jako počítače.</span><span class="sxs-lookup"><span data-stu-id="42b8b-125">Make sure that Pi is connected to the same network as your computer.</span></span> <span data-ttu-id="42b8b-126">Například pokud je počítač připojen k bezdrátové síti a platformy je připojeno k drátové síti, nemusíte to vidět IP adresu ve výstupu devdisco.</span><span class="sxs-lookup"><span data-stu-id="42b8b-126">For example, if your computer is connected to a wireless network while Pi is connected to a wired network, you might not see the IP address in the devdisco output.</span></span>

## <a name="clone-the-sample-application"></a><span data-ttu-id="42b8b-127">Klonování ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="42b8b-127">Clone the sample application</span></span>
<span data-ttu-id="42b8b-128">Chcete-li spustit ukázkový kód, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="42b8b-128">To open the sample code, follow these steps:</span></span>

1. <span data-ttu-id="42b8b-129">Naklonujte úložiště ukázkové z Githubu spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="42b8b-129">Clone the sample repository from GitHub by running the following command:</span></span>
   
   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started.git
   ```
2. <span data-ttu-id="42b8b-130">Otevřete ukázkovou aplikaci v aplikaci Visual Studio Code spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="42b8b-130">Open the sample application in Visual Studio Code by running the following commands:</span></span>
   
   ```bash
   cd iot-hub-node-raspberrypi-getting-started
   cd Lesson1
   code .
   ```

![Struktura úložiště](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-blink-mac.png)

<span data-ttu-id="42b8b-132">`app.js` v soubor `app` podsložky je klíče zdrojového souboru, který obsahuje kód, který ovládacího prvku Indikátor.</span><span class="sxs-lookup"><span data-stu-id="42b8b-132">The `app.js` file in the `app` subfolder is the key source file that contains the code to control the LED.</span></span>

### <a name="install-application-dependencies"></a><span data-ttu-id="42b8b-133">Instalace závislostí aplikací</span><span class="sxs-lookup"><span data-stu-id="42b8b-133">Install application dependencies</span></span>
<span data-ttu-id="42b8b-134">Nainstalujte v knihovnách a dalších modulů, které potřebujete pro ukázkovou aplikaci tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="42b8b-134">Install the libraries and other modules you need for the sample application by running the following command:</span></span>

```bash
npm install
```

## <a name="configure-the-device-connection"></a><span data-ttu-id="42b8b-135">Nakonfigurujte připojení zařízení</span><span class="sxs-lookup"><span data-stu-id="42b8b-135">Configure the device connection</span></span>
<span data-ttu-id="42b8b-136">Konfigurace připojení zařízení, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="42b8b-136">To configure the device connection, follow these steps:</span></span>

1. <span data-ttu-id="42b8b-137">Generujte konfiguračního souboru zařízení tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="42b8b-137">Generate the device configuration file by running the following command:</span></span>
   
   ```bash
   gulp init
   ```
   
   <span data-ttu-id="42b8b-138">Konfigurační soubor `config-raspberrypi.json` obsahuje uživatelská pověření, které používáte k přihlášení do pí.</span><span class="sxs-lookup"><span data-stu-id="42b8b-138">The configuration file `config-raspberrypi.json` contains the user credentials you use to log in to Pi.</span></span> <span data-ttu-id="42b8b-139">Aby se zabránilo úniku přihlašovacích údajů uživatele, je generována konfiguračního souboru v podsložce `.iot-hub-getting-started` domovské složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="42b8b-139">To avoid the leak of user credentials, the configuration file is generated in the subfolder `.iot-hub-getting-started` of the home folder on your computer.</span></span>

2. <span data-ttu-id="42b8b-140">Otevřete konfigurační soubor zařízení v aplikaci Visual Studio Code spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="42b8b-140">Open the device configuration file in Visual Studio Code by running the following command:</span></span>
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
   
3. <span data-ttu-id="42b8b-141">Nahraďte zástupný symbol `[device hostname or IP address]` se IP adresa nebo název hostitele, který jste získali dříve v "získat IP adresu a hostitele název pí."</span><span class="sxs-lookup"><span data-stu-id="42b8b-141">Replace the placeholder `[device hostname or IP address]` with the IP address or the host name that you got previously in "Obtain the IP address and host name of Pi."</span></span>
   
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-config-mac.png)

> [!NOTE]
> <span data-ttu-id="42b8b-143">Při připojování k malin platformy, můžete použít klíč SSH místo uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="42b8b-143">You can use SSH key instead of user name and password when connecting to Raspberry Pi.</span></span> <span data-ttu-id="42b8b-144">Pokud to chcete provést budete muset vygenerovat klíč pomocí **ssh-keygen** a **ssh kopie id platformy @\<adresa zařízení\>**.</span><span class="sxs-lookup"><span data-stu-id="42b8b-144">In order to do this you will have to generate the key using **ssh-keygen** and **ssh-copy-id pi@\<device address\>**.</span></span>
>
> <span data-ttu-id="42b8b-145">V systému Windows jsou k dispozici v těchto příkazů **Git Bash**.</span><span class="sxs-lookup"><span data-stu-id="42b8b-145">On Windows these commands are available in **Git Bash**.</span></span>
>
> <span data-ttu-id="42b8b-146">V systému MacOS, budete muset spustit **brew nainstalovat ssh kopie id**.</span><span class="sxs-lookup"><span data-stu-id="42b8b-146">On MacOS you need to run **brew install ssh-copy-id**.</span></span>
>
> <span data-ttu-id="42b8b-147">Po nahrání úspěšně klíč malin PI, nahraďte **device_password** s **device_key_path** vlastnost **konfigurace raspberrypi.json**.</span><span class="sxs-lookup"><span data-stu-id="42b8b-147">After successfully uploading the key to the Raspberry Pi, replace **device_password** with **device_key_path** property in **config-raspberrypi.json**.</span></span>
>
> <span data-ttu-id="42b8b-148">Aktualizované řádky by měl vypadat jako níže:</span><span class="sxs-lookup"><span data-stu-id="42b8b-148">Updated lines should look as below:</span></span>
> ```javascript
> "device_user_name": "pi",
> "device_key_path": "id_rsa",
> ```

<span data-ttu-id="42b8b-149">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="42b8b-149">Congratulations!</span></span> <span data-ttu-id="42b8b-150">Úspěšně jste vytvořili první ukázkovou aplikaci pro platformy.</span><span class="sxs-lookup"><span data-stu-id="42b8b-150">You've successfully created the first sample application for Pi.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="42b8b-151">Nasazení a spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="42b8b-151">Deploy and run the sample application</span></span>
### <a name="install-nodejs-and-npm-on-pi"></a><span data-ttu-id="42b8b-152">Nainstalovat Node.js a NPM na platformy</span><span class="sxs-lookup"><span data-stu-id="42b8b-152">Install Node.js and NPM on Pi</span></span>
<span data-ttu-id="42b8b-153">Nainstalujte Node.js a NPM pí spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="42b8b-153">Install Node.js and NPM on Pi by running the following command:</span></span>

```bash
gulp install-tools
```

<span data-ttu-id="42b8b-154">Tato úloha může trvat 10 minut na dokončení při prvním spuštění.</span><span class="sxs-lookup"><span data-stu-id="42b8b-154">This task might take 10 minutes to complete the first time you run it.</span></span>

### <a name="deploy-and-run-the-sample-app"></a><span data-ttu-id="42b8b-155">Nasazení a spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="42b8b-155">Deploy and run the sample app</span></span>
<span data-ttu-id="42b8b-156">Nasazení a spuštění ukázkové aplikace tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="42b8b-156">Deploy and run the sample application by running the following command:</span></span>

```bash
gulp deploy && gulp run
```

### <a name="verify-the-app-works"></a><span data-ttu-id="42b8b-157">Ověřte aplikace funguje</span><span class="sxs-lookup"><span data-stu-id="42b8b-157">Verify the app works</span></span>
<span data-ttu-id="42b8b-158">Teď byste měli vidět Indikátor na platformy blikat každé dvě sekundy.</span><span class="sxs-lookup"><span data-stu-id="42b8b-158">You should now see the LED on Pi blinking every two seconds.</span></span>  <span data-ttu-id="42b8b-159">Pokud nevidíte DIODU blikat, najdete v článku [Průvodce odstraňováním potíží s](iot-hub-raspberry-pi-kit-node-troubleshooting.md) pro řešení běžných potíží.</span><span class="sxs-lookup"><span data-stu-id="42b8b-159">If you don’t see the LED blinking, see the [troubleshooting guide](iot-hub-raspberry-pi-kit-node-troubleshooting.md) for solutions to common problems.</span></span>
<span data-ttu-id="42b8b-160">![Indikátor blikat](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)</span><span class="sxs-lookup"><span data-stu-id="42b8b-160">![LED blinking](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)</span></span>

## <a name="summary"></a><span data-ttu-id="42b8b-161">Souhrn</span><span class="sxs-lookup"><span data-stu-id="42b8b-161">Summary</span></span>
<span data-ttu-id="42b8b-162">Nainstalujete požadované nástroje pro práci s platformy a nasadit ukázkovou aplikaci pro platformy blikání Indikátor.</span><span class="sxs-lookup"><span data-stu-id="42b8b-162">You've installed the required tools to work with Pi and deployed a sample application to Pi to blink the LED.</span></span> <span data-ttu-id="42b8b-163">Teď můžete vytvořit, nasadit a spustit jinou ukázkovou aplikaci, která připojuje platformy Azure IoT Hub odesílat a přijímat zprávy.</span><span class="sxs-lookup"><span data-stu-id="42b8b-163">You can now create, deploy, and run another sample application that connects Pi to Azure IoT Hub to send and receive messages.</span></span>

## <a name="next-steps"></a><span data-ttu-id="42b8b-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="42b8b-164">Next steps</span></span>
[<span data-ttu-id="42b8b-165">Získat nástroje Azure</span><span class="sxs-lookup"><span data-stu-id="42b8b-165">Get the Azure tools</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)

