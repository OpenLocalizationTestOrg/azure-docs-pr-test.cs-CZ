---
title: "Intel Edison (uzel) připojit k Azure IoT - Lekce 1: nasazení aplikace | Microsoft Docs"
description: "Klonování C ukázkovou aplikaci z webu GitHub a spusťte gulp k nasazení této aplikace na panel Intel Edison. Tato ukázková aplikace bliká DIODU připojené k panelu každé dvě sekundy."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino vedla projekty, arduino vedla blikání, arduino vedla blikání kód, program blikání arduino, příklad arduino blikání"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: ed2c21d0-c72c-4ac2-9e70-347e9a0711c0
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 8490fbbf14183432c665165412f00955d6323580
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-deploy-the-blink-application"></a><span data-ttu-id="18fb1-105">Vytvoření a nasazení aplikace blink</span><span class="sxs-lookup"><span data-stu-id="18fb1-105">Create and deploy the blink application</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="18fb1-106">Co provedete</span><span class="sxs-lookup"><span data-stu-id="18fb1-106">What you will do</span></span>
<span data-ttu-id="18fb1-107">Klonování C ukázkovou aplikaci z webu GitHub a pomocí nástroje gulp nasadit ukázkovou aplikaci pro Intel Edison.</span><span class="sxs-lookup"><span data-stu-id="18fb1-107">Clone the sample C application from GitHub, and use the gulp tool to deploy the sample application to Intel Edison.</span></span> <span data-ttu-id="18fb1-108">Ukázkovou aplikaci bliká DIODU připojené k panelu každé dvě sekundy.</span><span class="sxs-lookup"><span data-stu-id="18fb1-108">The sample application blinks the LED connected to the board every two seconds.</span></span> <span data-ttu-id="18fb1-109">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="18fb1-109">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="18fb1-110">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="18fb1-110">What you will learn</span></span>
* <span data-ttu-id="18fb1-111">Postup nasazení a spuštění ukázkové aplikace na Edison.</span><span class="sxs-lookup"><span data-stu-id="18fb1-111">How to deploy and run the sample application on Edison.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="18fb1-112">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="18fb1-112">What you need</span></span>
<span data-ttu-id="18fb1-113">Musíte úspěšně jste dokončili následující operace:</span><span class="sxs-lookup"><span data-stu-id="18fb1-113">You must have successfully completed the following operations:</span></span>

* <span data-ttu-id="18fb1-114">[Konfigurace zařízení][configure-your-device]</span><span class="sxs-lookup"><span data-stu-id="18fb1-114">[Configure your device][configure-your-device]</span></span>
* <span data-ttu-id="18fb1-115">[Získat nástroje][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="18fb1-115">[Get the tools][get-the-tools]</span></span>

## <a name="open-the-sample-application"></a><span data-ttu-id="18fb1-116">Otevřete ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="18fb1-116">Open the sample application</span></span>
<span data-ttu-id="18fb1-117">Chcete-li otevřít ukázkové aplikace, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="18fb1-117">To open the sample application, follow these steps:</span></span>

1. <span data-ttu-id="18fb1-118">Naklonujte úložiště ukázkové z Githubu spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="18fb1-118">Clone the sample repository from GitHub by running the following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-edison-getting-started.git
   ```
2. <span data-ttu-id="18fb1-119">Otevřete ukázkovou aplikaci v aplikaci Visual Studio Code spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="18fb1-119">Open the sample application in Visual Studio Code by running the following commands:</span></span>

   ```bash
   cd iot-hub-node-edison-getting-started
   cd Lesson1
   code .
   ```

   ![Struktura úložiště][repo-structure]

<span data-ttu-id="18fb1-121">Soubor v `app` podsložky je klíče zdrojového souboru, který obsahuje kód, který ovládacího prvku Indikátor.</span><span class="sxs-lookup"><span data-stu-id="18fb1-121">The file in the `app` subfolder is the key source file that contains the code to control the LED.</span></span>

### <a name="install-application-dependencies"></a><span data-ttu-id="18fb1-122">Instalace závislostí aplikací</span><span class="sxs-lookup"><span data-stu-id="18fb1-122">Install application dependencies</span></span>
<span data-ttu-id="18fb1-123">Nainstalujte v knihovnách a dalších modulů, které potřebujete pro ukázkovou aplikaci tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="18fb1-123">Install the libraries and other modules you need for the sample application by running the following command:</span></span>

```bash
npm install
```

## <a name="configure-the-device-connection"></a><span data-ttu-id="18fb1-124">Nakonfigurujte připojení zařízení</span><span class="sxs-lookup"><span data-stu-id="18fb1-124">Configure the device connection</span></span>
<span data-ttu-id="18fb1-125">Konfigurace připojení zařízení, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="18fb1-125">To configure the device connection, follow these steps:</span></span>

1. <span data-ttu-id="18fb1-126">Generujte konfiguračního souboru zařízení tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="18fb1-126">Generate the device configuration file by running the following command:</span></span>

   ```bash
   gulp init
   ```

   <span data-ttu-id="18fb1-127">Konfigurační soubor `config-edison.json` obsahuje uživatelská pověření, které používáte k přihlášení do Edison.</span><span class="sxs-lookup"><span data-stu-id="18fb1-127">The configuration file `config-edison.json` contains the user credentials you use to log in to Edison.</span></span> <span data-ttu-id="18fb1-128">Aby se zabránilo úniku přihlašovacích údajů uživatele, je generována konfiguračního souboru v podsložce `.iot-hub-getting-started` domovské složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="18fb1-128">To avoid the leak of user credentials, the configuration file is generated in the subfolder `.iot-hub-getting-started` of the home folder on your computer.</span></span>

2. <span data-ttu-id="18fb1-129">Otevřete konfigurační soubor zařízení v aplikaci Visual Studio Code spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="18fb1-129">Open the device configuration file in Visual Studio Code by running the following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

3. <span data-ttu-id="18fb1-130">Nahraďte zástupný symbol `[device hostname or IP address]` a `[device password]` se IP adresa a heslo, které jste označili v předchozí lekci.</span><span class="sxs-lookup"><span data-stu-id="18fb1-130">Replace the placeholder `[device hostname or IP address]` and `[device password]` with the IP address and password that you marked down in previous lesson.</span></span>

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson1/vscode-config-mac.png)

<span data-ttu-id="18fb1-132">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="18fb1-132">Congratulations!</span></span> <span data-ttu-id="18fb1-133">Úspěšně jste vytvořili první ukázkovou aplikaci pro Edison.</span><span class="sxs-lookup"><span data-stu-id="18fb1-133">You've successfully created the first sample application for Edison.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="18fb1-134">Nasazení a spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="18fb1-134">Deploy and run the sample application</span></span>

### <a name="deploy-and-run-the-sample-app"></a><span data-ttu-id="18fb1-135">Nasazení a spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="18fb1-135">Deploy and run the sample app</span></span>
<span data-ttu-id="18fb1-136">Nasazení a spuštění ukázkové aplikace tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="18fb1-136">Deploy and run the sample application by running the following command:</span></span>

```bash
gulp deploy && gulp run
```

### <a name="verify-the-app-works"></a><span data-ttu-id="18fb1-137">Ověřte aplikace funguje</span><span class="sxs-lookup"><span data-stu-id="18fb1-137">Verify the app works</span></span>
<span data-ttu-id="18fb1-138">Ukázkovou aplikaci se po bliká Indikátor pro 20krát automaticky ukončí.</span><span class="sxs-lookup"><span data-stu-id="18fb1-138">The sample application terminates automatically after the LED blinks for 20 times.</span></span> <span data-ttu-id="18fb1-139">Pokud nevidíte DIODU blikat, najdete v článku [Průvodce odstraňováním potíží s] [ troubleshooting] pro řešení běžných potíží.</span><span class="sxs-lookup"><span data-stu-id="18fb1-139">If you don’t see the LED blinking, see the [troubleshooting guide][troubleshooting] for solutions to common problems.</span></span>

![Indikátor blikat][led-blinking]

## <a name="summary"></a><span data-ttu-id="18fb1-141">Souhrn</span><span class="sxs-lookup"><span data-stu-id="18fb1-141">Summary</span></span>
<span data-ttu-id="18fb1-142">Nainstalujete požadované nástroje pro práci s Edison a nasadit ukázkovou aplikaci pro Edison blikání Indikátor.</span><span class="sxs-lookup"><span data-stu-id="18fb1-142">You've installed the required tools to work with Edison and deployed a sample application to Edison to blink the LED.</span></span> <span data-ttu-id="18fb1-143">Teď můžete vytvořit, nasadit a spustit jinou ukázkovou aplikaci, která Edison se připojuje ke službě Azure IoT Hub odesílat a přijímat zprávy.</span><span class="sxs-lookup"><span data-stu-id="18fb1-143">You can now create, deploy, and run another sample application that connects Edison to Azure IoT Hub to send and receive messages.</span></span>

## <a name="next-steps"></a><span data-ttu-id="18fb1-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="18fb1-144">Next steps</span></span>
<span data-ttu-id="18fb1-145">[Získat nástroje Azure][get-the-azure-tools]</span><span class="sxs-lookup"><span data-stu-id="18fb1-145">[Get the Azure tools][get-the-azure-tools]</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[Configure-your-device]: iot-hub-intel-edison-kit-node-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson1/repo_structure.png
[led-blinking]: media/iot-hub-intel-edison-lessons/lesson1/led_blinking.png
[get-the-azure-tools]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-win32.md
