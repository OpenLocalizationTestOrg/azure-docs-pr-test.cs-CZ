---
title: "Connect Raspberry PI (C) tooAzure IoT - Lekce 4: Cloud zařízení | Microsoft Docs"
description: "Ukázková aplikace běží na platformy a sleduje příchozí zprávy ze služby IoT hub. Nová úloha gulp odešle tooPi zprávy z vaší IoT hub tooblink hello Indikátor."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "cloud toodevice, zpráv z cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: fcbc0dd0-cae3-47b0-8e58-240e4f406f75
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 5596bf3a83c21f2bd54b2f83e2a8fdad7a608b94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-tooreceive-cloud-to-device-messages"></a><span data-ttu-id="54cd2-105">Spuštění ukázkové aplikace tooreceive zprávy typu cloud zařízení</span><span class="sxs-lookup"><span data-stu-id="54cd2-105">Run a sample application tooreceive cloud-to-device messages</span></span>
<span data-ttu-id="54cd2-106">V tomto článku nasadit ukázkovou aplikaci na malin pí 3.</span><span class="sxs-lookup"><span data-stu-id="54cd2-106">In this article, you deploy a sample application on Raspberry Pi 3.</span></span> <span data-ttu-id="54cd2-107">Ukázková aplikace Hello monitoruje příchozích zpráv ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="54cd2-107">hello sample application monitors incoming messages from your IoT hub.</span></span> <span data-ttu-id="54cd2-108">Také spustit úlohu gulp na váš počítač toosend zprávy tooPi ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="54cd2-108">You also run a gulp task on your computer toosend messages tooPi from your IoT hub.</span></span> <span data-ttu-id="54cd2-109">Zprávy hello přijetí hello ukázkovou aplikaci bliká DIODU hello.</span><span class="sxs-lookup"><span data-stu-id="54cd2-109">When hello sample application receives hello messages, it blinks hello LED.</span></span> <span data-ttu-id="54cd2-110">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="54cd2-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="54cd2-111">Co provedete</span><span class="sxs-lookup"><span data-stu-id="54cd2-111">What you will do</span></span>
* <span data-ttu-id="54cd2-112">Připojte hello ukázkové aplikace tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="54cd2-112">Connect hello sample application tooyour IoT hub.</span></span>
* <span data-ttu-id="54cd2-113">Nasazení a spuštění hello ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="54cd2-113">Deploy and run hello sample application.</span></span>
* <span data-ttu-id="54cd2-114">Odesílat zprávy z vaší IoT hub tooPi tooblink hello Indikátor.</span><span class="sxs-lookup"><span data-stu-id="54cd2-114">Send messages from your IoT hub tooPi tooblink hello LED.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="54cd2-115">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="54cd2-115">What you will learn</span></span>
<span data-ttu-id="54cd2-116">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="54cd2-116">In this article, you will learn:</span></span>
* <span data-ttu-id="54cd2-117">Jak toomonitor příchozích zpráv ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="54cd2-117">How toomonitor incoming messages from your IoT hub.</span></span>
* <span data-ttu-id="54cd2-118">Jak toosend cloud zařízení zprávy z vaší tooPi centra IoT.</span><span class="sxs-lookup"><span data-stu-id="54cd2-118">How toosend cloud-to-device messages from your IoT hub tooPi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="54cd2-119">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="54cd2-119">What you need</span></span>
* <span data-ttu-id="54cd2-120">Malinová 3 Pi, nastavte pro použití.</span><span class="sxs-lookup"><span data-stu-id="54cd2-120">Raspberry Pi 3, set up for use.</span></span> <span data-ttu-id="54cd2-121">jak zjistit, tooset až pí, toolearn [konfigurace zařízení](iot-hub-raspberry-pi-kit-c-lesson1-configure-your-device.md).</span><span class="sxs-lookup"><span data-stu-id="54cd2-121">toolearn how tooset up Pi, see [Configure your device](iot-hub-raspberry-pi-kit-c-lesson1-configure-your-device.md).</span></span>
* <span data-ttu-id="54cd2-122">Služby IoT hub, který se vytvoří ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="54cd2-122">An IoT hub that is created in your Azure subscription.</span></span> <span data-ttu-id="54cd2-123">toolearn jak toocreate služby IoT hub, najdete v části [vytvoření služby IoT hub a zaregistrujte malin pí 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="54cd2-123">toolearn how toocreate your IoT hub, see [Create your IoT hub and register Raspberry Pi 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md).</span></span>

## <a name="connect-hello-sample-application-tooyour-iot-hub"></a><span data-ttu-id="54cd2-124">Připojit hello ukázkové aplikace tooyour IoT hub</span><span class="sxs-lookup"><span data-stu-id="54cd2-124">Connect hello sample application tooyour IoT hub</span></span>
1. <span data-ttu-id="54cd2-125">Ujistěte se, že jste ve složce úložišti hello `iot-hub-c-raspberrypi-getting-started`.</span><span class="sxs-lookup"><span data-stu-id="54cd2-125">Make sure that you're in hello repo folder `iot-hub-c-raspberrypi-getting-started`.</span></span> <span data-ttu-id="54cd2-126">Otevřete hello ukázkovou aplikaci v aplikaci Visual Studio Code spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="54cd2-126">Open hello sample application in Visual Studio Code by running hello following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```

   <span data-ttu-id="54cd2-127">Všimněte si hello `app.c` souboru v hello `app` podsložky.</span><span class="sxs-lookup"><span data-stu-id="54cd2-127">Notice hello `app.c` file in hello `app` subfolder.</span></span> <span data-ttu-id="54cd2-128">Hello `app.c` soubor je soubor hello zdroj klíče, který obsahuje hello kód toomonitor příchozí zprávy ze služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="54cd2-128">hello `app.c` file is hello key source file that contains hello code toomonitor incoming messages from hello IoT hub.</span></span> <span data-ttu-id="54cd2-129">Hello `blinkLED` funkce bliká DIODU hello.</span><span class="sxs-lookup"><span data-stu-id="54cd2-129">hello `blinkLED` function blinks hello LED.</span></span>

   ![Struktura úložišti v ukázkové aplikaci hello](media/iot-hub-raspberry-pi-lessons/lesson4/repo_structure_c.png)
2. <span data-ttu-id="54cd2-131">Inicializovat konfigurační soubor hello spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="54cd2-131">Initialize hello configuration file by running hello following commands:</span></span>

   ```bash
   npm install
   gulp init
   ```

   <span data-ttu-id="54cd2-132">Pokud jste dokončili postup hello v [vytvoření účtu úložiště a aplikace Azure funkce](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md) v tomto počítači jsou děděné všechny konfigurace hello, takže toostep toohello úloh nasazení a spuštění hello ukázkovou aplikaci, můžete přeskočit.</span><span class="sxs-lookup"><span data-stu-id="54cd2-132">If you completed hello steps in [Create an Azure function app and storage account](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md) on this computer, all hello configurations are inherited, so you can skip toostep toohello task of deploying and running hello sample application.</span></span> <span data-ttu-id="54cd2-133">Pokud jste dokončili postup hello v [vytvoření účtu Azure funkce aplikace a úložiště](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md) na jiném počítači, musíte tooreplace hello zástupné symboly v hello `config-raspberrypi.json` souboru.</span><span class="sxs-lookup"><span data-stu-id="54cd2-133">If you completed hello steps in [Create an Azure function app and storage account](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md) on a different computer, you need tooreplace hello placeholders in hello `config-raspberrypi.json` file.</span></span> <span data-ttu-id="54cd2-134">Hello `config-raspberrypi.json` soubor je v podsložce hello domovskou složku.</span><span class="sxs-lookup"><span data-stu-id="54cd2-134">hello `config-raspberrypi.json` file is in hello subfolder of your home folder.</span></span>

   ![Obsah souboru config raspberrypi.json hello](media/iot-hub-raspberry-pi-lessons/lesson4/config_raspberrypi.png)

* <span data-ttu-id="54cd2-136">Nahraďte **[zařízení hostitele nebo IP adresa]** s platformy na IP adresu nebo název hostitele, můžete získat spuštěním hello `devdisco list --eth` příkaz.</span><span class="sxs-lookup"><span data-stu-id="54cd2-136">Replace **[device hostname or IP address]** with Pi’s IP address or host name that you get by running hello `devdisco list --eth` command.</span></span>
* <span data-ttu-id="54cd2-137">Nahraďte **[řetězec připojení zařízení IoT]** s hello zařízení připojovací řetězec, který můžete získat spuštěním hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id} -g iot-sample {resource group name}` příkaz.</span><span class="sxs-lookup"><span data-stu-id="54cd2-137">Replace **[IoT device connection string]** with hello device connection string that you get by running hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id} -g iot-sample {resource group name}` command.</span></span>
* <span data-ttu-id="54cd2-138">Nahraďte **[IoT hub, připojovací řetězec]** s hello IoT hub připojovací řetězec, který můžete získat spuštěním hello `az iot hub show-connection-string --name {my hub name} -g iot-sample {resource group name}` příkaz.</span><span class="sxs-lookup"><span data-stu-id="54cd2-138">Replace **[IoT hub connection string]** with hello IoT hub connection string that you get by running hello `az iot hub show-connection-string --name {my hub name} -g iot-sample {resource group name}` command.</span></span>

> [!NOTE]
> <span data-ttu-id="54cd2-139">Spustit **gulp instalace nástroje** i, pokud jste neučinili v lekci 1.</span><span class="sxs-lookup"><span data-stu-id="54cd2-139">Run **gulp install-tools** as well, if you haven't done it in Lesson 1.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="54cd2-140">Nasazení a spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="54cd2-140">Deploy and run hello sample application</span></span>
<span data-ttu-id="54cd2-141">Nasazení a spuštění ukázkové aplikace hello na platformy spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="54cd2-141">Deploy and run hello sample application on Pi by running hello following commands:</span></span>

```
gulp deploy && gulp run
```

<span data-ttu-id="54cd2-142">Spustí příkaz gulp Hello nejprve hello úloh instalace nástroje.</span><span class="sxs-lookup"><span data-stu-id="54cd2-142">hello gulp command runs hello install-tools task first.</span></span> <span data-ttu-id="54cd2-143">Potom nasadí hello ukázkové aplikace tooPi.</span><span class="sxs-lookup"><span data-stu-id="54cd2-143">Then it deploys hello sample application tooPi.</span></span> <span data-ttu-id="54cd2-144">Nakonec je spuštěna aplikace hello na platformy a samostatná úloha na hostiteli počítače toosend 20 blikání zprávy tooPi z služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="54cd2-144">Finally, it runs hello application on Pi and a separate task on your host computer toosend 20 blink messages tooPi from your IoT hub.</span></span>

<span data-ttu-id="54cd2-145">Po spuštění ukázkové aplikace hello začne naslouchat toomessages ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="54cd2-145">After hello sample application runs, it starts listening toomessages from your IoT hub.</span></span> <span data-ttu-id="54cd2-146">Mezitím hello gulp úloh odešle několik "blink" zprávy z vaší tooPi centra IoT.</span><span class="sxs-lookup"><span data-stu-id="54cd2-146">Meanwhile, hello gulp task sends several "blink" messages from your IoT hub tooPi.</span></span> <span data-ttu-id="54cd2-147">Pro každou blikání zprávu, která přijímá Pi, hello ukázkové aplikace volá hello `blinkLED` funkce tooblink hello Indikátor.</span><span class="sxs-lookup"><span data-stu-id="54cd2-147">For each blink message that Pi receives, hello sample application calls hello `blinkLED` function tooblink hello LED.</span></span>

<span data-ttu-id="54cd2-148">Měli byste vidět blikání DIODU hello každé dvě sekundy jako hello gulp úloh zasílá 20 zprávy z vaší tooPi centra IoT.</span><span class="sxs-lookup"><span data-stu-id="54cd2-148">You should see hello LED blink every two seconds as hello gulp task sends 20 messages from your IoT hub tooPi.</span></span> <span data-ttu-id="54cd2-149">Hello poslední jedním je zpráva "stop", která ukončí hello spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="54cd2-149">hello last one is a "stop" message that stops hello application from running.</span></span>

![Ukázkovou aplikaci s gulp příkazu a blink – zprávy](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_blink_c.png)

## <a name="summary"></a><span data-ttu-id="54cd2-151">Souhrn</span><span class="sxs-lookup"><span data-stu-id="54cd2-151">Summary</span></span>
<span data-ttu-id="54cd2-152">Z vaší IoT hub tooPi tooblink hello DIODU úspěšně odeslat zprávy.</span><span class="sxs-lookup"><span data-stu-id="54cd2-152">You’ve successfully sent messages from your IoT hub tooPi tooblink hello LED.</span></span> <span data-ttu-id="54cd2-153">Další úlohou Hello je volitelné: Změna hello zapnout a vypnout chování hello Indikátor.</span><span class="sxs-lookup"><span data-stu-id="54cd2-153">hello next task is optional: change hello on and off behavior of hello LED.</span></span>

## <a name="next-steps"></a><span data-ttu-id="54cd2-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="54cd2-154">Next steps</span></span>
[<span data-ttu-id="54cd2-155">Změnit hello zapnout a vypnout chování hello DIODU</span><span class="sxs-lookup"><span data-stu-id="54cd2-155">Change hello on and off behavior of hello LED</span></span>](iot-hub-raspberry-pi-kit-c-lesson4-change-led-behavior.md)
