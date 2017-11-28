---
title: "Connect Raspberry PI (C) tooAzure IoT - lekci 1: nasazení aplikace | Microsoft Docs"
description: "Klonování hello ukázkové C aplikace z webu GitHub a gulp toodeploy Tabule tooyour malin pí 3 této aplikace. Tato ukázková aplikace bliká hello DIODU připojené toohello Tabule každé dvě sekundy."
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
ms.openlocfilehash: e90c3360c4de1873313db19561c781eb21dbf1d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-hello-blink-application"></a><span data-ttu-id="17ee9-105">Vytvoření a nasazení aplikace hello blikání</span><span class="sxs-lookup"><span data-stu-id="17ee9-105">Create and deploy hello blink application</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="17ee9-106">Co provedete</span><span class="sxs-lookup"><span data-stu-id="17ee9-106">What you will do</span></span>
<span data-ttu-id="17ee9-107">Klonování hello ukázkovou aplikaci C z Githubu a používat hello gulp nástroj toodeploy hello ukázkové aplikace tooRaspberry pí 3.</span><span class="sxs-lookup"><span data-stu-id="17ee9-107">Clone hello sample C application from GitHub, and use hello gulp tool toodeploy hello sample application tooRaspberry Pi 3.</span></span> <span data-ttu-id="17ee9-108">Ukázková aplikace Hello bliká hello DIODU připojené toohello Tabule každé dvě sekundy.</span><span class="sxs-lookup"><span data-stu-id="17ee9-108">hello sample application blinks hello LED connected toohello board every two seconds.</span></span> <span data-ttu-id="17ee9-109">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="17ee9-109">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="17ee9-110">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="17ee9-110">What you will learn</span></span>
<span data-ttu-id="17ee9-111">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="17ee9-111">In this article, you will learn:</span></span>

* <span data-ttu-id="17ee9-112">Jak toouse hello `device-discover-cli` nástroj tooretrieve sítě informace o pí.</span><span class="sxs-lookup"><span data-stu-id="17ee9-112">How toouse hello `device-discover-cli` tool tooretrieve networking information about Pi.</span></span>
* <span data-ttu-id="17ee9-113">Jak toodeploy a spuštění hello ukázkové aplikace na pí.</span><span class="sxs-lookup"><span data-stu-id="17ee9-113">How toodeploy and run hello sample application on Pi.</span></span>
* <span data-ttu-id="17ee9-114">Jak toodeploy a ladění aplikací spuštěných vzdáleně na pí.</span><span class="sxs-lookup"><span data-stu-id="17ee9-114">How toodeploy and debug applications running remotely on Pi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="17ee9-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="17ee9-115">What you need</span></span>
<span data-ttu-id="17ee9-116">Musíte úspěšně jste dokončili hello následující operace:</span><span class="sxs-lookup"><span data-stu-id="17ee9-116">You must have successfully completed hello following operations:</span></span>

* [<span data-ttu-id="17ee9-117">Konfigurace zařízení</span><span class="sxs-lookup"><span data-stu-id="17ee9-117">Configure your device</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-configure-your-device.md)
* [<span data-ttu-id="17ee9-118">Získat nástroje hello</span><span class="sxs-lookup"><span data-stu-id="17ee9-118">Get hello tools</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)

## <a name="obtain-hello-ip-address-and-host-name-of-pi"></a><span data-ttu-id="17ee9-119">Získat hello IP adresy a názvu hostitele pí</span><span class="sxs-lookup"><span data-stu-id="17ee9-119">Obtain hello IP address and host name of Pi</span></span>
<span data-ttu-id="17ee9-120">Otevřete příkazový řádek v systému Windows nebo terminál v systému macOS nebo Ubuntu a spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="17ee9-120">Open a command prompt in Windows or a terminal in macOS or Ubuntu, and then run hello following command:</span></span>

```bash
devdisco list --eth
```

<span data-ttu-id="17ee9-121">Měli byste vidět výstupu, který je podobný toohello následující:</span><span class="sxs-lookup"><span data-stu-id="17ee9-121">You should see an output that is similar toohello following:</span></span>

![Zjišťování zařízení](media/iot-hub-raspberry-pi-lessons/lesson1/device_discovery.png)

<span data-ttu-id="17ee9-123">Poznamenejte si hello `IP address` a `hostname` pí.</span><span class="sxs-lookup"><span data-stu-id="17ee9-123">Take note of hello `IP address` and `hostname` of Pi.</span></span> <span data-ttu-id="17ee9-124">Je třeba tyto informace dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="17ee9-124">You need this information later in this article.</span></span>

> [!NOTE]
> <span data-ttu-id="17ee9-125">Ujistěte se, že platformy je připojený toohello stejné sítě jako váš počítač.</span><span class="sxs-lookup"><span data-stu-id="17ee9-125">Make sure that Pi is connected toohello same network as your computer.</span></span> <span data-ttu-id="17ee9-126">Například pokud je počítač připojený tooa bezdrátové sítě připojené tooa drátové síti při instalaci platformy, nemusíte to vidět hello IP adresu ve výstupu devdisco hello.</span><span class="sxs-lookup"><span data-stu-id="17ee9-126">For example, if your computer is connected tooa wireless network while Pi is connected tooa wired network, you might not see hello IP address in hello devdisco output.</span></span>

## <a name="open-hello-sample-application"></a><span data-ttu-id="17ee9-127">Ukázkové aplikace otevřete hello</span><span class="sxs-lookup"><span data-stu-id="17ee9-127">Open hello sample application</span></span>
<span data-ttu-id="17ee9-128">tooopen hello ukázkové aplikace, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="17ee9-128">tooopen hello sample application, follow these steps:</span></span>

1. <span data-ttu-id="17ee9-129">Klonování úložiště ukázkové hello z Githubu spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="17ee9-129">Clone hello sample repository from GitHub by running hello following command:</span></span>
   
    ```bash
    git clone https://github.com/Azure-Samples/iot-hub-c-raspberrypi-getting-started.git
    ```
2. <span data-ttu-id="17ee9-130">Otevřete hello ukázkovou aplikaci v aplikaci Visual Studio Code spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="17ee9-130">Open hello sample application in Visual Studio Code by running hello following commands:</span></span>
   
    ```bash
    cd iot-hub-c-raspberrypi-getting-started
    cd Lesson1
    code .
    ```

![Struktura úložiště](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-blink-c-mac.png)

<span data-ttu-id="17ee9-132">Hello `main.c` souboru v hello `app` podsložky je hello klíče zdrojový soubor, který obsahuje hello kód toocontrol hello Indikátor.</span><span class="sxs-lookup"><span data-stu-id="17ee9-132">hello `main.c` file in hello `app` subfolder is hello key source file that contains hello code toocontrol hello LED.</span></span>

### <a name="install-application-dependencies"></a><span data-ttu-id="17ee9-133">Instalace závislostí aplikací</span><span class="sxs-lookup"><span data-stu-id="17ee9-133">Install application dependencies</span></span>
<span data-ttu-id="17ee9-134">Nainstalujte hello knihovny a z ostatních modulů, které potřebujete pro ukázkovou aplikaci hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="17ee9-134">Install hello libraries and other modules you need for hello sample application by running hello following command:</span></span>

```bash
npm install
```

## <a name="configure-hello-device-connection"></a><span data-ttu-id="17ee9-135">Nakonfigurujte připojení zařízení hello</span><span class="sxs-lookup"><span data-stu-id="17ee9-135">Configure hello device connection</span></span>
<span data-ttu-id="17ee9-136">tooconfigure hello připojení zařízení, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="17ee9-136">tooconfigure hello device connection, follow these steps:</span></span>

1. <span data-ttu-id="17ee9-137">Generovat soubor konfigurace zařízení hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="17ee9-137">Generate hello device configuration file by running hello following command:</span></span>
   
   ```bash
   gulp init
   ```
   
   <span data-ttu-id="17ee9-138">konfigurační soubor Hello `config-raspberrypi.json` obsahuje uživatelská pověření hello použijete toolog v tooPi.</span><span class="sxs-lookup"><span data-stu-id="17ee9-138">hello configuration file `config-raspberrypi.json` contains hello user credentials you use toolog in tooPi.</span></span> <span data-ttu-id="17ee9-139">tooavoid hello úniku přihlašovacích údajů uživatele, je generována hello konfigurační soubor v podsložce hello `.iot-hub-getting-started` hello domovské složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="17ee9-139">tooavoid hello leak of user credentials, hello configuration file is generated in hello subfolder `.iot-hub-getting-started` of hello home folder on your computer.</span></span>

2. <span data-ttu-id="17ee9-140">Otevřete soubor konfigurace zařízení hello v Visual Studio Code spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="17ee9-140">Open hello device configuration file in Visual Studio Code by running hello following command:</span></span>
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```

3. <span data-ttu-id="17ee9-141">Nahraďte zástupný symbol hello `[device hostname or IP address]` s hello IP adresu nebo název hostitele hello, který jste získali dříve "Získat hello IP adresy a názvu hostitele pí."</span><span class="sxs-lookup"><span data-stu-id="17ee9-141">Replace hello placeholder `[device hostname or IP address]` with hello IP address or hello host name that you got previously in "Obtain hello IP address and host name of Pi."</span></span>
   
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-config-mac.png)

> [!NOTE]
> <span data-ttu-id="17ee9-143">Při připojování tooRaspberry platformy, můžete použít klíč SSH místo uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="17ee9-143">You can use SSH key instead of user name and password when connecting tooRaspberry Pi.</span></span> <span data-ttu-id="17ee9-144">V pořadí toodo, to budete mít klíč pomocí toogenerate hello **ssh-keygen** a **ssh kopie id platformy @\<adresa zařízení\>**.</span><span class="sxs-lookup"><span data-stu-id="17ee9-144">In order toodo this you will have toogenerate hello key using **ssh-keygen** and **ssh-copy-id pi@\<device address\>**.</span></span>
>
> <span data-ttu-id="17ee9-145">V systému Windows jsou k dispozici v těchto příkazů **Git Bash**.</span><span class="sxs-lookup"><span data-stu-id="17ee9-145">On Windows these commands are available in **Git Bash**.</span></span>
>
> <span data-ttu-id="17ee9-146">V systému MacOS potřebujete toorun **brew nainstalovat ssh kopie id**.</span><span class="sxs-lookup"><span data-stu-id="17ee9-146">On MacOS you need toorun **brew install ssh-copy-id**.</span></span>
>
> <span data-ttu-id="17ee9-147">Po nahrání úspěšně hello klíče toohello malin Pi, nahraďte **device_password** s **device_key_path** vlastnost **konfigurace raspberrypi.json**.</span><span class="sxs-lookup"><span data-stu-id="17ee9-147">After successfully uploading hello key toohello Raspberry Pi, replace **device_password** with **device_key_path** property in **config-raspberrypi.json**.</span></span>
>
> <span data-ttu-id="17ee9-148">Aktualizované řádky by měl vypadat jako níže:</span><span class="sxs-lookup"><span data-stu-id="17ee9-148">Updated lines should look as below:</span></span>
> ```javascript
> "device_user_name": "pi",
> "device_key_path": "id_rsa",
> ```

<span data-ttu-id="17ee9-149">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="17ee9-149">Congratulations!</span></span> <span data-ttu-id="17ee9-150">Úspěšně jste vytvořili hello první ukázkovou aplikaci pro platformy.</span><span class="sxs-lookup"><span data-stu-id="17ee9-150">You've successfully created hello first sample application for Pi.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="17ee9-151">Nasazení a spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="17ee9-151">Deploy and run hello sample application</span></span>
### <a name="install-hello-azure-iot-hub-sdk-on-pi"></a><span data-ttu-id="17ee9-152">Instalace sady SDK Azure IoT Hub hello na platformy</span><span class="sxs-lookup"><span data-stu-id="17ee9-152">Install hello Azure IoT Hub SDK on Pi</span></span>
<span data-ttu-id="17ee9-153">Instalace sady SDK Azure IoT Hub hello na platformy spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="17ee9-153">Install hello Azure IoT Hub SDK on Pi by running hello following command:</span></span>

```bash
gulp install-tools
```

<span data-ttu-id="17ee9-154">Tato úloha může trvat několik minut toocomplete hello při prvním spuštění.</span><span class="sxs-lookup"><span data-stu-id="17ee9-154">This task might take a few minutes toocomplete hello first time you run it.</span></span>

### <a name="deploy-and-run-hello-sample-app"></a><span data-ttu-id="17ee9-155">Nasazení a spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="17ee9-155">Deploy and run hello sample app</span></span>
<span data-ttu-id="17ee9-156">Nasazení a spuštění ukázkové aplikace hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="17ee9-156">Deploy and run hello sample application by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

### <a name="verify-hello-app-works"></a><span data-ttu-id="17ee9-157">Ověřte hello aplikace funguje</span><span class="sxs-lookup"><span data-stu-id="17ee9-157">Verify hello app works</span></span>
<span data-ttu-id="17ee9-158">Hello ukázkové aplikaci se po hello DIODU bliká pro 20krát automaticky ukončí.</span><span class="sxs-lookup"><span data-stu-id="17ee9-158">hello sample application terminates automatically after hello LED blinks for 20 times.</span></span> <span data-ttu-id="17ee9-159">Pokud nevidíte hello DIODU blikat, přečtěte si téma hello [Průvodce odstraňováním potíží s](iot-hub-raspberry-pi-kit-c-troubleshooting.md) řešení toocommon problémů.</span><span class="sxs-lookup"><span data-stu-id="17ee9-159">If you don’t see hello LED blinking, see hello [troubleshooting guide](iot-hub-raspberry-pi-kit-c-troubleshooting.md) for solutions toocommon problems.</span></span>
<span data-ttu-id="17ee9-160">![Indikátor blikat](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)</span><span class="sxs-lookup"><span data-stu-id="17ee9-160">![LED blinking](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)</span></span>

## <a name="summary"></a><span data-ttu-id="17ee9-161">Souhrn</span><span class="sxs-lookup"><span data-stu-id="17ee9-161">Summary</span></span>
<span data-ttu-id="17ee9-162">Nainstalujete hello požadované nástroje toowork s platformy a nasazení ukázkové aplikace tooPi tooblink hello Indikátor.</span><span class="sxs-lookup"><span data-stu-id="17ee9-162">You've installed hello required tools toowork with Pi and deployed a sample application tooPi tooblink hello LED.</span></span> <span data-ttu-id="17ee9-163">Teď můžete vytvořit, nasadit a spusťte jinou ukázkovou aplikaci, která připojí tooAzure platformy IoT Hub toosend a přijímat zprávy.</span><span class="sxs-lookup"><span data-stu-id="17ee9-163">You can now create, deploy, and run another sample application that connects Pi tooAzure IoT Hub toosend and receive messages.</span></span>

## <a name="next-steps"></a><span data-ttu-id="17ee9-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="17ee9-164">Next steps</span></span>
[<span data-ttu-id="17ee9-165">Získat nástroje Azure</span><span class="sxs-lookup"><span data-stu-id="17ee9-165">Get Azure tools</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md)

