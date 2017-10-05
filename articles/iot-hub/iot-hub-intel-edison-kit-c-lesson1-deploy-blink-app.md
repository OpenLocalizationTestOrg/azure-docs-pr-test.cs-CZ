---
title: "Connect Intel Edison (C) k Azure IoT - lekci 1: nasazení aplikace | Microsoft Docs"
description: "Klonování C ukázkovou aplikaci z webu GitHub a spusťte gulp k nasazení této aplikace na panel Intel Edison. Tato ukázková aplikace bliká DIODU připojené k panelu každé dvě sekundy."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino vedla projekty, arduino vedla blikání, arduino vedla blikání kód, program blikání arduino, příklad arduino blikání"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: b02dfd3f-28fd-4b52-8775-eb0eaf74d707
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c45ff5f41bdbc78da8532ffdcaaeec15c695f531
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-deploy-the-blink-application"></a><span data-ttu-id="54c52-105">Vytvoření a nasazení aplikace blink</span><span class="sxs-lookup"><span data-stu-id="54c52-105">Create and deploy the blink application</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="54c52-106">Co provedete</span><span class="sxs-lookup"><span data-stu-id="54c52-106">What you will do</span></span>
<span data-ttu-id="54c52-107">Klonování C ukázkovou aplikaci z webu GitHub a pomocí nástroje gulp nasadit ukázkovou aplikaci pro Intel Edison.</span><span class="sxs-lookup"><span data-stu-id="54c52-107">Clone the sample C application from GitHub, and use the gulp tool to deploy the sample application to Intel Edison.</span></span> <span data-ttu-id="54c52-108">Ukázkovou aplikaci bliká DIODU připojené k panelu každé dvě sekundy.</span><span class="sxs-lookup"><span data-stu-id="54c52-108">The sample application blinks the LED connected to the board every two seconds.</span></span> <span data-ttu-id="54c52-109">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="54c52-109">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="54c52-110">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="54c52-110">What you will learn</span></span>
* <span data-ttu-id="54c52-111">Postup nasazení a spuštění ukázkové aplikace na Edison.</span><span class="sxs-lookup"><span data-stu-id="54c52-111">How to deploy and run the sample application on Edison.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="54c52-112">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="54c52-112">What you need</span></span>
<span data-ttu-id="54c52-113">Musíte úspěšně jste dokončili následující operace:</span><span class="sxs-lookup"><span data-stu-id="54c52-113">You must have successfully completed the following operations:</span></span>

* <span data-ttu-id="54c52-114">[Konfigurace zařízení][configure-your-device]</span><span class="sxs-lookup"><span data-stu-id="54c52-114">[Configure your device][configure-your-device]</span></span>
* <span data-ttu-id="54c52-115">[Získat nástroje][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="54c52-115">[Get the tools][get-the-tools]</span></span>

## <a name="open-the-sample-application"></a><span data-ttu-id="54c52-116">Otevřete ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="54c52-116">Open the sample application</span></span>
<span data-ttu-id="54c52-117">Chcete-li otevřít ukázkové aplikace, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="54c52-117">To open the sample application, follow these steps:</span></span>

1. <span data-ttu-id="54c52-118">Naklonujte úložiště ukázkové z Githubu spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="54c52-118">Clone the sample repository from GitHub by running the following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-edison-getting-started.git
   ```
2. <span data-ttu-id="54c52-119">Otevřete ukázkovou aplikaci v aplikaci Visual Studio Code spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="54c52-119">Open the sample application in Visual Studio Code by running the following commands:</span></span>

   ```bash
   cd iot-hub-c-edison-getting-started
   cd Lesson1
   code .
   ```

   ![Struktura úložiště][repo-structure]

<span data-ttu-id="54c52-121">Soubor v `app` podsložky je klíče zdrojového souboru, který obsahuje kód, který ovládacího prvku Indikátor.</span><span class="sxs-lookup"><span data-stu-id="54c52-121">The file in the `app` subfolder is the key source file that contains the code to control the LED.</span></span>

### <a name="install-application-dependencies"></a><span data-ttu-id="54c52-122">Instalace závislostí aplikací</span><span class="sxs-lookup"><span data-stu-id="54c52-122">Install application dependencies</span></span>
<span data-ttu-id="54c52-123">Nainstalujte v knihovnách a dalších modulů, které potřebujete pro ukázkovou aplikaci tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="54c52-123">Install the libraries and other modules you need for the sample application by running the following command:</span></span>

```bash
npm install
```

## <a name="configure-the-device-connection"></a><span data-ttu-id="54c52-124">Nakonfigurujte připojení zařízení</span><span class="sxs-lookup"><span data-stu-id="54c52-124">Configure the device connection</span></span>
<span data-ttu-id="54c52-125">Konfigurace připojení zařízení, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="54c52-125">To configure the device connection, follow these steps:</span></span>

1. <span data-ttu-id="54c52-126">Generujte konfiguračního souboru zařízení tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="54c52-126">Generate the device configuration file by running the following command:</span></span>

   ```bash
   gulp init
   ```

   <span data-ttu-id="54c52-127">Konfigurační soubor `config-edison.json` obsahuje uživatelská pověření, které používáte k přihlášení do Edison.</span><span class="sxs-lookup"><span data-stu-id="54c52-127">The configuration file `config-edison.json` contains the user credentials you use to log in to Edison.</span></span> <span data-ttu-id="54c52-128">Aby se zabránilo úniku přihlašovacích údajů uživatele, je generována konfiguračního souboru v podsložce `.iot-hub-getting-started` domovské složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="54c52-128">To avoid the leak of user credentials, the configuration file is generated in the subfolder `.iot-hub-getting-started` of the home folder on your computer.</span></span>

2. <span data-ttu-id="54c52-129">Otevřete konfigurační soubor zařízení v aplikaci Visual Studio Code spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="54c52-129">Open the device configuration file in Visual Studio Code by running the following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

3. <span data-ttu-id="54c52-130">Nahraďte zástupný symbol `[device hostname or IP address]` a `[device password]` se IP adresa a heslo, které jste označili v předchozí lekci.</span><span class="sxs-lookup"><span data-stu-id="54c52-130">Replace the placeholder `[device hostname or IP address]` and `[device password]` with the IP address and password that you marked down in previous lesson.</span></span>

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson1/vscode-config-mac.png)

<span data-ttu-id="54c52-132">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="54c52-132">Congratulations!</span></span> <span data-ttu-id="54c52-133">Úspěšně jste vytvořili první ukázkovou aplikaci pro Edison.</span><span class="sxs-lookup"><span data-stu-id="54c52-133">You've successfully created the first sample application for Edison.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="54c52-134">Nasazení a spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="54c52-134">Deploy and run the sample application</span></span>
### <a name="install-the-azure-iot-hub-sdk-on-edison"></a><span data-ttu-id="54c52-135">Nainstalujte službu Azure IoT Hub SDK Edison</span><span class="sxs-lookup"><span data-stu-id="54c52-135">Install the Azure IoT Hub SDK on Edison</span></span>
<span data-ttu-id="54c52-136">Instalace sady SDK Azure IoT Hub na Edison spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="54c52-136">Install the Azure IoT Hub SDK on Edison by running the following command:</span></span>

```bash
gulp install-tools
```

<span data-ttu-id="54c52-137">Tato úloha může trvat dlouhou dobu v závislosti na připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="54c52-137">This task might take a long time to complete, depending on your network connection.</span></span> <span data-ttu-id="54c52-138">Musí být spuštěn pouze jednou pro jeden Edison.</span><span class="sxs-lookup"><span data-stu-id="54c52-138">It needs to be run only once for one Edison.</span></span>

### <a name="deploy-and-run-the-sample-app"></a><span data-ttu-id="54c52-139">Nasazení a spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="54c52-139">Deploy and run the sample app</span></span>
<span data-ttu-id="54c52-140">Nasazení a spuštění ukázkové aplikace tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="54c52-140">Deploy and run the sample application by running the following command:</span></span>

```bash
gulp deploy && gulp run
```

### <a name="verify-the-app-works"></a><span data-ttu-id="54c52-141">Ověřte aplikace funguje</span><span class="sxs-lookup"><span data-stu-id="54c52-141">Verify the app works</span></span>
<span data-ttu-id="54c52-142">Ukázkovou aplikaci se po bliká Indikátor pro 20krát automaticky ukončí.</span><span class="sxs-lookup"><span data-stu-id="54c52-142">The sample application terminates automatically after the LED blinks for 20 times.</span></span> <span data-ttu-id="54c52-143">Pokud nevidíte DIODU blikat, najdete v článku [Průvodce odstraňováním potíží s] [ troubleshooting] pro řešení běžných potíží.</span><span class="sxs-lookup"><span data-stu-id="54c52-143">If you don’t see the LED blinking, see the [troubleshooting guide][troubleshooting] for solutions to common problems.</span></span>

![Indikátor blikat][led-blinking]

## <a name="summary"></a><span data-ttu-id="54c52-145">Souhrn</span><span class="sxs-lookup"><span data-stu-id="54c52-145">Summary</span></span>
<span data-ttu-id="54c52-146">Nainstalujete požadované nástroje pro práci s Edison a nasadit ukázkovou aplikaci pro Edison blikání Indikátor.</span><span class="sxs-lookup"><span data-stu-id="54c52-146">You've installed the required tools to work with Edison and deployed a sample application to Edison to blink the LED.</span></span> <span data-ttu-id="54c52-147">Teď můžete vytvořit, nasadit a spustit jinou ukázkovou aplikaci, která Edison se připojuje ke službě Azure IoT Hub odesílat a přijímat zprávy.</span><span class="sxs-lookup"><span data-stu-id="54c52-147">You can now create, deploy, and run another sample application that connects Edison to Azure IoT Hub to send and receive messages.</span></span>

## <a name="next-steps"></a><span data-ttu-id="54c52-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="54c52-148">Next steps</span></span>
<span data-ttu-id="54c52-149">[Získat nástroje Azure][get-the-azure-tools]</span><span class="sxs-lookup"><span data-stu-id="54c52-149">[Get the Azure tools][get-the-azure-tools]</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[Configure-your-device]: iot-hub-intel-edison-kit-c-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson1/repo_structure_c.png
[led-blinking]: media/iot-hub-intel-edison-lessons/lesson1/led_blinking_c.jpg
[get-the-azure-tools]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-win32.md
