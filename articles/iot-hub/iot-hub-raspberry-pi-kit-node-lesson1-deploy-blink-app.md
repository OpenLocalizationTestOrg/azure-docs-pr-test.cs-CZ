---
featureFlags: usabilla
title: "Připojit malin platformy (uzel) tooAzure IoT - lekci 1: nasazení aplikace | Microsoft Docs"
description: "Klonování aplikace Node.js hello ukázka z Githubu a gulp toodeploy Tabule tooyour malin pí 3 této aplikace. Tato ukázková aplikace bliká hello DIODU připojené toohello Tabule každé dvě sekundy."
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
ms.openlocfilehash: 9732df3009b8342d4872fe2318a975a6251e772b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-hello-blink-application"></a><span data-ttu-id="2b48f-105">Vytvoření a nasazení aplikace hello blikání</span><span class="sxs-lookup"><span data-stu-id="2b48f-105">Create and deploy hello blink application</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="2b48f-106">Co provedete</span><span class="sxs-lookup"><span data-stu-id="2b48f-106">What you will do</span></span>
<span data-ttu-id="2b48f-107">Klonování aplikace Node.js hello ukázka z Githubu a použít hello gulp nástroj toodeploy hello ukázkové aplikace tooyour malin pí 3.</span><span class="sxs-lookup"><span data-stu-id="2b48f-107">Clone hello sample Node.js application from GitHub and use hello gulp tool toodeploy hello sample application tooyour Raspberry Pi 3.</span></span> <span data-ttu-id="2b48f-108">Ukázková aplikace Hello bliká hello DIODU připojené toohello Tabule každé dvě sekundy.</span><span class="sxs-lookup"><span data-stu-id="2b48f-108">hello sample application blinks hello LED connected toohello board every two seconds.</span></span> <span data-ttu-id="2b48f-109">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="2b48f-109">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="2b48f-110">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="2b48f-110">What you will learn</span></span>
<span data-ttu-id="2b48f-111">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="2b48f-111">In this article, you will learn:</span></span>

* <span data-ttu-id="2b48f-112">Jak toouse hello `device-discover-cli` nástroj tooretrieve sítě informace o pí.</span><span class="sxs-lookup"><span data-stu-id="2b48f-112">How toouse hello `device-discover-cli` tool tooretrieve networking information about Pi.</span></span>
* <span data-ttu-id="2b48f-113">Jak toodeploy a spuštění hello ukázkové aplikace na pí.</span><span class="sxs-lookup"><span data-stu-id="2b48f-113">How toodeploy and run hello sample application on Pi.</span></span>
* <span data-ttu-id="2b48f-114">Jak toodeploy a ladění aplikací spuštěných vzdáleně na pí.</span><span class="sxs-lookup"><span data-stu-id="2b48f-114">How toodeploy and debug applications running remotely on Pi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="2b48f-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="2b48f-115">What you need</span></span>
<span data-ttu-id="2b48f-116">Musíte úspěšně jste dokončili hello následující operace:</span><span class="sxs-lookup"><span data-stu-id="2b48f-116">You must have successfully completed hello following operations:</span></span>

* [<span data-ttu-id="2b48f-117">Konfigurace zařízení</span><span class="sxs-lookup"><span data-stu-id="2b48f-117">Configure your device</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md)
* [<span data-ttu-id="2b48f-118">Získat nástroje hello</span><span class="sxs-lookup"><span data-stu-id="2b48f-118">Get hello tools</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

## <a name="obtain-hello-ip-address-and-host-name-of-pi"></a><span data-ttu-id="2b48f-119">Získat hello IP adresy a názvu hostitele pí</span><span class="sxs-lookup"><span data-stu-id="2b48f-119">Obtain hello IP address and host name of Pi</span></span>
<span data-ttu-id="2b48f-120">Otevřete příkazový řádek v systému Windows nebo terminál v systému macOS nebo Ubuntu a spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="2b48f-120">Open a command prompt in Windows or a terminal in macOS or Ubuntu, and then run hello following command:</span></span>

```bash
devdisco list --eth
```

<span data-ttu-id="2b48f-121">Měli byste vidět výstupu, který je podobný toohello následující:</span><span class="sxs-lookup"><span data-stu-id="2b48f-121">You should see an output that is similar toohello following:</span></span>

![Zjišťování zařízení](media/iot-hub-raspberry-pi-lessons/lesson1/device_discovery.png)

<span data-ttu-id="2b48f-123">Poznamenejte si hello `IP address` a `hostname` pí.</span><span class="sxs-lookup"><span data-stu-id="2b48f-123">Take note of hello `IP address` and `hostname` of Pi.</span></span> <span data-ttu-id="2b48f-124">Je třeba tyto informace dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="2b48f-124">You need this information later in this article.</span></span>

> [!NOTE]
> <span data-ttu-id="2b48f-125">Ujistěte se, že platformy je připojený toohello stejné sítě jako váš počítač.</span><span class="sxs-lookup"><span data-stu-id="2b48f-125">Make sure that Pi is connected toohello same network as your computer.</span></span> <span data-ttu-id="2b48f-126">Například pokud je počítač připojený tooa bezdrátové sítě připojené tooa drátové síti při instalaci platformy, nemusíte to vidět hello IP adresu ve výstupu devdisco hello.</span><span class="sxs-lookup"><span data-stu-id="2b48f-126">For example, if your computer is connected tooa wireless network while Pi is connected tooa wired network, you might not see hello IP address in hello devdisco output.</span></span>

## <a name="clone-hello-sample-application"></a><span data-ttu-id="2b48f-127">Klonování hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="2b48f-127">Clone hello sample application</span></span>
<span data-ttu-id="2b48f-128">tooopen hello ukázkový kód, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="2b48f-128">tooopen hello sample code, follow these steps:</span></span>

1. <span data-ttu-id="2b48f-129">Klonování úložiště ukázkové hello z Githubu spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2b48f-129">Clone hello sample repository from GitHub by running hello following command:</span></span>
   
   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started.git
   ```
2. <span data-ttu-id="2b48f-130">Otevřete hello ukázkovou aplikaci v aplikaci Visual Studio Code spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="2b48f-130">Open hello sample application in Visual Studio Code by running hello following commands:</span></span>
   
   ```bash
   cd iot-hub-node-raspberrypi-getting-started
   cd Lesson1
   code .
   ```

![Struktura úložiště](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-blink-mac.png)

<span data-ttu-id="2b48f-132">Hello `app.js` souboru v hello `app` podsložky je hello klíče zdrojový soubor, který obsahuje hello kód toocontrol hello Indikátor.</span><span class="sxs-lookup"><span data-stu-id="2b48f-132">hello `app.js` file in hello `app` subfolder is hello key source file that contains hello code toocontrol hello LED.</span></span>

### <a name="install-application-dependencies"></a><span data-ttu-id="2b48f-133">Instalace závislostí aplikací</span><span class="sxs-lookup"><span data-stu-id="2b48f-133">Install application dependencies</span></span>
<span data-ttu-id="2b48f-134">Nainstalujte hello knihovny a z ostatních modulů, které potřebujete pro ukázkovou aplikaci hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2b48f-134">Install hello libraries and other modules you need for hello sample application by running hello following command:</span></span>

```bash
npm install
```

## <a name="configure-hello-device-connection"></a><span data-ttu-id="2b48f-135">Nakonfigurujte připojení zařízení hello</span><span class="sxs-lookup"><span data-stu-id="2b48f-135">Configure hello device connection</span></span>
<span data-ttu-id="2b48f-136">tooconfigure hello připojení zařízení, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="2b48f-136">tooconfigure hello device connection, follow these steps:</span></span>

1. <span data-ttu-id="2b48f-137">Generovat soubor konfigurace zařízení hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2b48f-137">Generate hello device configuration file by running hello following command:</span></span>
   
   ```bash
   gulp init
   ```
   
   <span data-ttu-id="2b48f-138">konfigurační soubor Hello `config-raspberrypi.json` obsahuje uživatelská pověření hello použijete toolog v tooPi.</span><span class="sxs-lookup"><span data-stu-id="2b48f-138">hello configuration file `config-raspberrypi.json` contains hello user credentials you use toolog in tooPi.</span></span> <span data-ttu-id="2b48f-139">tooavoid hello úniku přihlašovacích údajů uživatele, je generována hello konfigurační soubor v podsložce hello `.iot-hub-getting-started` hello domovské složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="2b48f-139">tooavoid hello leak of user credentials, hello configuration file is generated in hello subfolder `.iot-hub-getting-started` of hello home folder on your computer.</span></span>

2. <span data-ttu-id="2b48f-140">Otevřete soubor konfigurace zařízení hello v Visual Studio Code spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2b48f-140">Open hello device configuration file in Visual Studio Code by running hello following command:</span></span>
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
   
3. <span data-ttu-id="2b48f-141">Nahraďte zástupný symbol hello `[device hostname or IP address]` s hello IP adresu nebo název hostitele hello, který jste získali dříve "Získat hello IP adresy a názvu hostitele pí."</span><span class="sxs-lookup"><span data-stu-id="2b48f-141">Replace hello placeholder `[device hostname or IP address]` with hello IP address or hello host name that you got previously in "Obtain hello IP address and host name of Pi."</span></span>
   
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-config-mac.png)

> [!NOTE]
> <span data-ttu-id="2b48f-143">Při připojování tooRaspberry platformy, můžete použít klíč SSH místo uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="2b48f-143">You can use SSH key instead of user name and password when connecting tooRaspberry Pi.</span></span> <span data-ttu-id="2b48f-144">V pořadí toodo, to budete mít klíč pomocí toogenerate hello **ssh-keygen** a **ssh kopie id platformy @\<adresa zařízení\>**.</span><span class="sxs-lookup"><span data-stu-id="2b48f-144">In order toodo this you will have toogenerate hello key using **ssh-keygen** and **ssh-copy-id pi@\<device address\>**.</span></span>
>
> <span data-ttu-id="2b48f-145">V systému Windows jsou k dispozici v těchto příkazů **Git Bash**.</span><span class="sxs-lookup"><span data-stu-id="2b48f-145">On Windows these commands are available in **Git Bash**.</span></span>
>
> <span data-ttu-id="2b48f-146">V systému MacOS potřebujete toorun **brew nainstalovat ssh kopie id**.</span><span class="sxs-lookup"><span data-stu-id="2b48f-146">On MacOS you need toorun **brew install ssh-copy-id**.</span></span>
>
> <span data-ttu-id="2b48f-147">Po nahrání úspěšně hello klíče toohello malin Pi, nahraďte **device_password** s **device_key_path** vlastnost **konfigurace raspberrypi.json**.</span><span class="sxs-lookup"><span data-stu-id="2b48f-147">After successfully uploading hello key toohello Raspberry Pi, replace **device_password** with **device_key_path** property in **config-raspberrypi.json**.</span></span>
>
> <span data-ttu-id="2b48f-148">Aktualizované řádky by měl vypadat jako níže:</span><span class="sxs-lookup"><span data-stu-id="2b48f-148">Updated lines should look as below:</span></span>
> ```javascript
> "device_user_name": "pi",
> "device_key_path": "id_rsa",
> ```

<span data-ttu-id="2b48f-149">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="2b48f-149">Congratulations!</span></span> <span data-ttu-id="2b48f-150">Úspěšně jste vytvořili hello první ukázkovou aplikaci pro platformy.</span><span class="sxs-lookup"><span data-stu-id="2b48f-150">You've successfully created hello first sample application for Pi.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="2b48f-151">Nasazení a spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="2b48f-151">Deploy and run hello sample application</span></span>
### <a name="install-nodejs-and-npm-on-pi"></a><span data-ttu-id="2b48f-152">Nainstalovat Node.js a NPM na platformy</span><span class="sxs-lookup"><span data-stu-id="2b48f-152">Install Node.js and NPM on Pi</span></span>
<span data-ttu-id="2b48f-153">Nainstalujte Node.js a NPM pí spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2b48f-153">Install Node.js and NPM on Pi by running hello following command:</span></span>

```bash
gulp install-tools
```

<span data-ttu-id="2b48f-154">Tato úloha může trvat 10 minut toocomplete hello při prvním spuštění.</span><span class="sxs-lookup"><span data-stu-id="2b48f-154">This task might take 10 minutes toocomplete hello first time you run it.</span></span>

### <a name="deploy-and-run-hello-sample-app"></a><span data-ttu-id="2b48f-155">Nasazení a spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="2b48f-155">Deploy and run hello sample app</span></span>
<span data-ttu-id="2b48f-156">Nasazení a spuštění ukázkové aplikace hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2b48f-156">Deploy and run hello sample application by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

### <a name="verify-hello-app-works"></a><span data-ttu-id="2b48f-157">Ověřte hello aplikace funguje</span><span class="sxs-lookup"><span data-stu-id="2b48f-157">Verify hello app works</span></span>
<span data-ttu-id="2b48f-158">Teď byste měli vidět hello DIODU na platformy blikat každé dvě sekundy.</span><span class="sxs-lookup"><span data-stu-id="2b48f-158">You should now see hello LED on Pi blinking every two seconds.</span></span>  <span data-ttu-id="2b48f-159">Pokud nevidíte hello DIODU blikat, přečtěte si téma hello [Průvodce odstraňováním potíží s](iot-hub-raspberry-pi-kit-node-troubleshooting.md) řešení toocommon problémů.</span><span class="sxs-lookup"><span data-stu-id="2b48f-159">If you don’t see hello LED blinking, see hello [troubleshooting guide](iot-hub-raspberry-pi-kit-node-troubleshooting.md) for solutions toocommon problems.</span></span>
<span data-ttu-id="2b48f-160">![Indikátor blikat](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)</span><span class="sxs-lookup"><span data-stu-id="2b48f-160">![LED blinking](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)</span></span>

## <a name="summary"></a><span data-ttu-id="2b48f-161">Souhrn</span><span class="sxs-lookup"><span data-stu-id="2b48f-161">Summary</span></span>
<span data-ttu-id="2b48f-162">Nainstalujete hello požadované nástroje toowork s platformy a nasazení ukázkové aplikace tooPi tooblink hello Indikátor.</span><span class="sxs-lookup"><span data-stu-id="2b48f-162">You've installed hello required tools toowork with Pi and deployed a sample application tooPi tooblink hello LED.</span></span> <span data-ttu-id="2b48f-163">Teď můžete vytvořit, nasadit a spusťte jinou ukázkovou aplikaci, která připojí tooAzure platformy IoT Hub toosend a přijímat zprávy.</span><span class="sxs-lookup"><span data-stu-id="2b48f-163">You can now create, deploy, and run another sample application that connects Pi tooAzure IoT Hub toosend and receive messages.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2b48f-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2b48f-164">Next steps</span></span>
[<span data-ttu-id="2b48f-165">Získat hello Azure nástroje</span><span class="sxs-lookup"><span data-stu-id="2b48f-165">Get hello Azure tools</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)

