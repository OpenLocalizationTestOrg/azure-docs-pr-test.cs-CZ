---
title: "Připojit Intel Edison (uzel) tooAzure IoT - lekci 1: nasazení aplikace | Microsoft Docs"
description: "Klonování hello ukázkové C aplikace z webu GitHub a spusťte gulp toodeploy této aplikace tooyour Intel Edison panelu. Tato ukázková aplikace bliká hello DIODU připojené toohello Tabule každé dvě sekundy."
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
ms.openlocfilehash: bc03c7e45bd1ba9e9b2c8f2fec70a1be647e96b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-hello-blink-application"></a><span data-ttu-id="50d9a-105">Vytvoření a nasazení aplikace hello blikání</span><span class="sxs-lookup"><span data-stu-id="50d9a-105">Create and deploy hello blink application</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="50d9a-106">Co provedete</span><span class="sxs-lookup"><span data-stu-id="50d9a-106">What you will do</span></span>
<span data-ttu-id="50d9a-107">Klonování hello ukázkové C aplikace z webu GitHub a používat hello gulp nástroj toodeploy hello ukázkové aplikace tooIntel Edison.</span><span class="sxs-lookup"><span data-stu-id="50d9a-107">Clone hello sample C application from GitHub, and use hello gulp tool toodeploy hello sample application tooIntel Edison.</span></span> <span data-ttu-id="50d9a-108">Ukázková aplikace Hello bliká hello DIODU připojené toohello Tabule každé dvě sekundy.</span><span class="sxs-lookup"><span data-stu-id="50d9a-108">hello sample application blinks hello LED connected toohello board every two seconds.</span></span> <span data-ttu-id="50d9a-109">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="50d9a-109">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="50d9a-110">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="50d9a-110">What you will learn</span></span>
* <span data-ttu-id="50d9a-111">Jak toodeploy a spuštění hello ukázkové aplikace na Edison.</span><span class="sxs-lookup"><span data-stu-id="50d9a-111">How toodeploy and run hello sample application on Edison.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="50d9a-112">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="50d9a-112">What you need</span></span>
<span data-ttu-id="50d9a-113">Musíte úspěšně jste dokončili hello následující operace:</span><span class="sxs-lookup"><span data-stu-id="50d9a-113">You must have successfully completed hello following operations:</span></span>

* <span data-ttu-id="50d9a-114">[Konfigurace zařízení][configure-your-device]</span><span class="sxs-lookup"><span data-stu-id="50d9a-114">[Configure your device][configure-your-device]</span></span>
* <span data-ttu-id="50d9a-115">[Získat nástroje hello][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="50d9a-115">[Get hello tools][get-the-tools]</span></span>

## <a name="open-hello-sample-application"></a><span data-ttu-id="50d9a-116">Ukázkové aplikace otevřete hello</span><span class="sxs-lookup"><span data-stu-id="50d9a-116">Open hello sample application</span></span>
<span data-ttu-id="50d9a-117">tooopen hello ukázkové aplikace, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="50d9a-117">tooopen hello sample application, follow these steps:</span></span>

1. <span data-ttu-id="50d9a-118">Klonování úložiště ukázkové hello z Githubu spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="50d9a-118">Clone hello sample repository from GitHub by running hello following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-edison-getting-started.git
   ```
2. <span data-ttu-id="50d9a-119">Otevřete hello ukázkovou aplikaci v aplikaci Visual Studio Code spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="50d9a-119">Open hello sample application in Visual Studio Code by running hello following commands:</span></span>

   ```bash
   cd iot-hub-node-edison-getting-started
   cd Lesson1
   code .
   ```

   ![Struktura úložiště][repo-structure]

<span data-ttu-id="50d9a-121">soubor Hello v hello `app` podsložky je hello klíče zdrojový soubor, který obsahuje hello kód toocontrol hello Indikátor.</span><span class="sxs-lookup"><span data-stu-id="50d9a-121">hello file in hello `app` subfolder is hello key source file that contains hello code toocontrol hello LED.</span></span>

### <a name="install-application-dependencies"></a><span data-ttu-id="50d9a-122">Instalace závislostí aplikací</span><span class="sxs-lookup"><span data-stu-id="50d9a-122">Install application dependencies</span></span>
<span data-ttu-id="50d9a-123">Nainstalujte hello knihovny a z ostatních modulů, které potřebujete pro ukázkovou aplikaci hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="50d9a-123">Install hello libraries and other modules you need for hello sample application by running hello following command:</span></span>

```bash
npm install
```

## <a name="configure-hello-device-connection"></a><span data-ttu-id="50d9a-124">Nakonfigurujte připojení zařízení hello</span><span class="sxs-lookup"><span data-stu-id="50d9a-124">Configure hello device connection</span></span>
<span data-ttu-id="50d9a-125">tooconfigure hello připojení zařízení, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="50d9a-125">tooconfigure hello device connection, follow these steps:</span></span>

1. <span data-ttu-id="50d9a-126">Generovat soubor konfigurace zařízení hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="50d9a-126">Generate hello device configuration file by running hello following command:</span></span>

   ```bash
   gulp init
   ```

   <span data-ttu-id="50d9a-127">konfigurační soubor Hello `config-edison.json` obsahuje uživatelská pověření hello použijete toolog v tooEdison.</span><span class="sxs-lookup"><span data-stu-id="50d9a-127">hello configuration file `config-edison.json` contains hello user credentials you use toolog in tooEdison.</span></span> <span data-ttu-id="50d9a-128">tooavoid hello úniku přihlašovacích údajů uživatele, je generována hello konfigurační soubor v podsložce hello `.iot-hub-getting-started` hello domovské složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="50d9a-128">tooavoid hello leak of user credentials, hello configuration file is generated in hello subfolder `.iot-hub-getting-started` of hello home folder on your computer.</span></span>

2. <span data-ttu-id="50d9a-129">Otevřete soubor konfigurace zařízení hello v Visual Studio Code spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="50d9a-129">Open hello device configuration file in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

3. <span data-ttu-id="50d9a-130">Nahraďte zástupný symbol hello `[device hostname or IP address]` a `[device password]` s hello IP adresu a heslo, které jste označili v předchozí lekci.</span><span class="sxs-lookup"><span data-stu-id="50d9a-130">Replace hello placeholder `[device hostname or IP address]` and `[device password]` with hello IP address and password that you marked down in previous lesson.</span></span>

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson1/vscode-config-mac.png)

<span data-ttu-id="50d9a-132">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="50d9a-132">Congratulations!</span></span> <span data-ttu-id="50d9a-133">Úspěšně jste vytvořili hello první ukázkovou aplikaci pro Edison.</span><span class="sxs-lookup"><span data-stu-id="50d9a-133">You've successfully created hello first sample application for Edison.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="50d9a-134">Nasazení a spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="50d9a-134">Deploy and run hello sample application</span></span>

### <a name="deploy-and-run-hello-sample-app"></a><span data-ttu-id="50d9a-135">Nasazení a spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="50d9a-135">Deploy and run hello sample app</span></span>
<span data-ttu-id="50d9a-136">Nasazení a spuštění ukázkové aplikace hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="50d9a-136">Deploy and run hello sample application by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

### <a name="verify-hello-app-works"></a><span data-ttu-id="50d9a-137">Ověřte hello aplikace funguje</span><span class="sxs-lookup"><span data-stu-id="50d9a-137">Verify hello app works</span></span>
<span data-ttu-id="50d9a-138">Hello ukázkové aplikaci se po hello DIODU bliká pro 20krát automaticky ukončí.</span><span class="sxs-lookup"><span data-stu-id="50d9a-138">hello sample application terminates automatically after hello LED blinks for 20 times.</span></span> <span data-ttu-id="50d9a-139">Pokud nevidíte hello DIODU blikat, přečtěte si téma hello [Průvodce odstraňováním potíží s] [ troubleshooting] řešení toocommon problémů.</span><span class="sxs-lookup"><span data-stu-id="50d9a-139">If you don’t see hello LED blinking, see hello [troubleshooting guide][troubleshooting] for solutions toocommon problems.</span></span>

![Indikátor blikat][led-blinking]

## <a name="summary"></a><span data-ttu-id="50d9a-141">Souhrn</span><span class="sxs-lookup"><span data-stu-id="50d9a-141">Summary</span></span>
<span data-ttu-id="50d9a-142">Nainstalujete hello požadované nástroje toowork s Edison a nasazení ukázkové aplikace tooEdison tooblink hello Indikátor.</span><span class="sxs-lookup"><span data-stu-id="50d9a-142">You've installed hello required tools toowork with Edison and deployed a sample application tooEdison tooblink hello LED.</span></span> <span data-ttu-id="50d9a-143">Teď můžete vytvořit, nasadit a spouštět jiný ukázkovou aplikaci, která se připojuje Edison tooAzure toosend IoT Hub a příjem zpráv.</span><span class="sxs-lookup"><span data-stu-id="50d9a-143">You can now create, deploy, and run another sample application that connects Edison tooAzure IoT Hub toosend and receive messages.</span></span>

## <a name="next-steps"></a><span data-ttu-id="50d9a-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="50d9a-144">Next steps</span></span>
<span data-ttu-id="50d9a-145">[Získat hello Azure nástroje][get-the-azure-tools]</span><span class="sxs-lookup"><span data-stu-id="50d9a-145">[Get hello Azure tools][get-the-azure-tools]</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[Configure-your-device]: iot-hub-intel-edison-kit-node-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson1/repo_structure.png
[led-blinking]: media/iot-hub-intel-edison-lessons/lesson1/led_blinking.png
[get-the-azure-tools]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-win32.md
