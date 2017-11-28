---
featureFlags: usabilla
title: "Připojit malin platformy (uzel) tooAzure IoT - Lekce 4: Cloud zařízení | Microsoft Docs"
description: "Ukázková aplikace Hello běží na platformy a sleduje příchozí zprávy ze služby IoT hub. Nová úloha gulp odešle tooPi zprávy z vaší IoT hub tooblink hello Indikátor."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "cloud toodevice, zpráv z cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 6ae6539e-1163-4490-8d72-fdf7803e3054
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: d69ded4e30c27378481ab2a4fb9c5b73be7bd44e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-hello-sample-application-tooreceive-cloud-to-device-messages"></a><span data-ttu-id="d8692-105">Spustit hello ukázkové aplikace tooreceive cloud zařízení zpráv</span><span class="sxs-lookup"><span data-stu-id="d8692-105">Run hello sample application tooreceive cloud-to-device messages</span></span>
<span data-ttu-id="d8692-106">V tomto článku nasadit ukázkovou aplikaci na malin pí 3.</span><span class="sxs-lookup"><span data-stu-id="d8692-106">In this article, you deploy a sample application on Raspberry Pi 3.</span></span> <span data-ttu-id="d8692-107">Ukázková aplikace Hello monitoruje příchozích zpráv ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d8692-107">hello sample application monitors incoming messages from your IoT hub.</span></span> <span data-ttu-id="d8692-108">Také spustit úlohu gulp na váš počítač toosend zprávy tooPi ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d8692-108">You also run a gulp task on your computer toosend messages tooPi from your IoT hub.</span></span> <span data-ttu-id="d8692-109">Zprávy hello přijetí hello ukázkovou aplikaci bliká DIODU hello.</span><span class="sxs-lookup"><span data-stu-id="d8692-109">When hello sample application receives hello messages, it blinks hello LED.</span></span> <span data-ttu-id="d8692-110">Pokud máte potíže, hledají řešení na hello [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="d8692-110">If you have any problems, seek solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="d8692-111">Co provedete</span><span class="sxs-lookup"><span data-stu-id="d8692-111">What you will do</span></span>
* <span data-ttu-id="d8692-112">Připojte hello ukázkové aplikace tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d8692-112">Connect hello sample application tooyour IoT hub.</span></span>
* <span data-ttu-id="d8692-113">Nasazení a spuštění hello ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d8692-113">Deploy and run hello sample application.</span></span>
* <span data-ttu-id="d8692-114">Odesílat zprávy z vaší IoT hub tooPi tooblink hello Indikátor.</span><span class="sxs-lookup"><span data-stu-id="d8692-114">Send messages from your IoT hub tooPi tooblink hello LED.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="d8692-115">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="d8692-115">What you will learn</span></span>
<span data-ttu-id="d8692-116">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="d8692-116">In this article, you will learn:</span></span>
* <span data-ttu-id="d8692-117">Jak toomonitor příchozích zpráv ze služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="d8692-117">How toomonitor incoming messages from your IoT hub</span></span>
* <span data-ttu-id="d8692-118">Jak toosend cloud zařízení zprávy z vaší tooPi centra IoT.</span><span class="sxs-lookup"><span data-stu-id="d8692-118">How toosend cloud-to-device messages from your IoT hub tooPi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="d8692-119">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="d8692-119">What you need</span></span>
* <span data-ttu-id="d8692-120">Malinová 3 Pi, nastavte pro použití.</span><span class="sxs-lookup"><span data-stu-id="d8692-120">Raspberry Pi 3, set up for use.</span></span> <span data-ttu-id="d8692-121">jak zjistit, tooset až pí, toolearn [konfigurace zařízení](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md).</span><span class="sxs-lookup"><span data-stu-id="d8692-121">toolearn how tooset up Pi, see [Configure your device](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md).</span></span>
* <span data-ttu-id="d8692-122">Služby IoT hub, který se vytvoří ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="d8692-122">An IoT hub that is created in your Azure subscription.</span></span> <span data-ttu-id="d8692-123">toolearn jak toocreate služby IoT hub, najdete v části [vytvoření služby IoT hub a zaregistrujte malin pí 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="d8692-123">toolearn how toocreate your IoT hub, see [Create your IoT hub and register Raspberry Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).</span></span>

## <a name="connect-hello-sample-application-tooyour-iot-hub"></a><span data-ttu-id="d8692-124">Připojit hello ukázkové aplikace tooyour IoT hub</span><span class="sxs-lookup"><span data-stu-id="d8692-124">Connect hello sample application tooyour IoT hub</span></span>
1. <span data-ttu-id="d8692-125">Ujistěte se, že jste ve složce úložišti hello `iot-hub-node-raspberrypi-getting-started`.</span><span class="sxs-lookup"><span data-stu-id="d8692-125">Make sure that you're in hello repo folder `iot-hub-node-raspberrypi-getting-started`.</span></span> <span data-ttu-id="d8692-126">Otevřete hello ukázkovou aplikaci v aplikaci Visual Studio Code spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="d8692-126">Open hello sample application in Visual Studio Code by running hello following commands:</span></span>
   
   ```bash
   cd Lesson4
   code .
   ```
   
   <span data-ttu-id="d8692-127">Všimněte si hello `app.js` souboru v hello `app` podsložky.</span><span class="sxs-lookup"><span data-stu-id="d8692-127">Notice hello `app.js` file in hello `app` subfolder.</span></span> <span data-ttu-id="d8692-128">Hello `app.js` soubor je soubor hello zdroj klíče, který obsahuje hello kód toomonitor příchozí zprávy ze služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="d8692-128">hello `app.js` file is hello key source file that contains hello code toomonitor incoming messages from hello IoT hub.</span></span> <span data-ttu-id="d8692-129">Hello `blinkLED` funkce bliká DIODU hello.</span><span class="sxs-lookup"><span data-stu-id="d8692-129">hello `blinkLED` function blinks hello LED.</span></span>
   
   ![Struktura úložišti v ukázkové aplikaci hello](media/iot-hub-raspberry-pi-lessons/lesson4/repo_structure.png)
2. <span data-ttu-id="d8692-131">Inicializace hello konfiguračního souboru pomocí hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="d8692-131">Initialize hello configuration file by using hello following commands:</span></span>
   
   ```bash
   npm install
   gulp init
   ```
   
   <span data-ttu-id="d8692-132">Pokud jste dokončili postup hello v [vytvoření účtu úložiště a aplikace Azure funkce](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md) v tomto počítači jsou děděné všechny konfigurace hello, takže toohello úloh nasazení a spuštění hello ukázkovou aplikaci, můžete přeskočit.</span><span class="sxs-lookup"><span data-stu-id="d8692-132">If you completed hello steps in [Create an Azure function app and storage account](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md) on this computer, all hello configurations are inherited, so you can skip toohello task of deploying and running hello sample application.</span></span> <span data-ttu-id="d8692-133">Pokud jste dokončili postup hello v [vytvoření účtu Azure funkce aplikace a úložiště](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md) na jiném počítači, musíte tooreplace hello zástupné symboly v hello `config-raspberrypi.json` souboru.</span><span class="sxs-lookup"><span data-stu-id="d8692-133">If you completed hello steps in [Create an Azure function app and storage account](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md) on a different computer, you need tooreplace hello placeholders in hello `config-raspberrypi.json` file.</span></span> <span data-ttu-id="d8692-134">Hello `config-raspberrypi.json` soubor je v podsložce hello domovskou složku.</span><span class="sxs-lookup"><span data-stu-id="d8692-134">hello `config-raspberrypi.json` file is in hello subfolder of your home folder.</span></span>
   
   ![Obsah souboru config raspberrypi.json hello](media/iot-hub-raspberry-pi-lessons/lesson4/config_raspberrypi.png)

* <span data-ttu-id="d8692-136">Nahraďte **[zařízení hostitele nebo IP adresa]** s IP adresou hello platformy nebo hello názvu hostitele, kterou můžete získat spuštěním hello `devdisco list --eth` příkaz.</span><span class="sxs-lookup"><span data-stu-id="d8692-136">Replace **[device hostname or IP address]** with hello IP address of Pi or hello host name that you get by running hello `devdisco list --eth` command.</span></span>
* <span data-ttu-id="d8692-137">Nahraďte **[řetězec připojení zařízení IoT]** s hello zařízení připojovací řetězec, který můžete získat spuštěním hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id} -g iot-sample {resource group name}` příkaz.</span><span class="sxs-lookup"><span data-stu-id="d8692-137">Replace **[IoT device connection string]** with hello device connection string that you get by running hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id} -g iot-sample {resource group name}` command.</span></span>
* <span data-ttu-id="d8692-138">Nahraďte **[IoT hub, připojovací řetězec]** s hello IoT hub připojovací řetězec, který můžete získat spuštěním hello `az iot hub show-connection-string --name {my hub name} -g iot-sample {resource group name}` příkaz.</span><span class="sxs-lookup"><span data-stu-id="d8692-138">Replace **[IoT hub connection string]** with hello IoT hub connection string that you get by running hello `az iot hub show-connection-string --name {my hub name} -g iot-sample {resource group name}` command.</span></span>

> [!NOTE]
> <span data-ttu-id="d8692-139">Spustit **gulp instalace nástroje** i, pokud jste neučinili v lekci 1.</span><span class="sxs-lookup"><span data-stu-id="d8692-139">Run **gulp install-tools** as well, if you haven't done it in Lesson 1.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="d8692-140">Nasazení a spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="d8692-140">Deploy and run hello sample application</span></span>
<span data-ttu-id="d8692-141">Nasazení a spuštění ukázkové aplikace hello na platformy spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d8692-141">Deploy and run hello sample application on Pi by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

<span data-ttu-id="d8692-142">příkaz Hello nasadí hello ukázkové aplikace tooPi.</span><span class="sxs-lookup"><span data-stu-id="d8692-142">hello command deploys hello sample application tooPi.</span></span> <span data-ttu-id="d8692-143">Pak je spuštěna aplikace hello na platformy a samostatná úloha na hostiteli počítače toosend 20 blikání zprávy tooPi z služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d8692-143">Then, it runs hello application on Pi and a separate task on your host computer toosend 20 blink messages tooPi from your IoT hub.</span></span>

<span data-ttu-id="d8692-144">Po spuštění ukázkové aplikace hello začne naslouchat toomessages ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d8692-144">After hello sample application runs, it starts listening toomessages from your IoT hub.</span></span> <span data-ttu-id="d8692-145">Mezitím hello gulp úloh odešle několik "blink" zprávy z vaší tooPi centra IoT.</span><span class="sxs-lookup"><span data-stu-id="d8692-145">Meanwhile, hello gulp task sends several "blink" messages from your IoT hub tooPi.</span></span> <span data-ttu-id="d8692-146">Pro každou blikání zprávu, která přijímá Pi, hello ukázkové aplikace volá hello `blinkLED` funkce tooblink hello Indikátor.</span><span class="sxs-lookup"><span data-stu-id="d8692-146">For each blink message that Pi receives, hello sample application calls hello `blinkLED` function tooblink hello LED.</span></span>

<span data-ttu-id="d8692-147">Měli byste vidět blikání DIODU hello každé dvě sekundy jako hello gulp úloh zasílá 20 zprávy z vaší tooPi centra IoT.</span><span class="sxs-lookup"><span data-stu-id="d8692-147">You should see hello LED blink every two seconds as hello gulp task sends 20 messages from your IoT hub tooPi.</span></span> <span data-ttu-id="d8692-148">Hello poslední jedním je "stop" zprávu, že toostop hello aplikace spuštěna.</span><span class="sxs-lookup"><span data-stu-id="d8692-148">hello last one is a "stop" message that tells hello application toostop running.</span></span>

![Ukázkovou aplikaci s gulp příkazu a blink – zprávy](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_blink.png)

## <a name="summary"></a><span data-ttu-id="d8692-150">Souhrn</span><span class="sxs-lookup"><span data-stu-id="d8692-150">Summary</span></span>
<span data-ttu-id="d8692-151">Z vaší IoT hub tooPi tooblink hello DIODU úspěšně odeslat zprávy.</span><span class="sxs-lookup"><span data-stu-id="d8692-151">You’ve successfully sent messages from your IoT hub tooPi tooblink hello LED.</span></span> <span data-ttu-id="d8692-152">Další úlohou Hello je volitelné: Změna hello zapnout a vypnout chování hello Indikátor.</span><span class="sxs-lookup"><span data-stu-id="d8692-152">hello next task is optional: change hello on and off behavior of hello LED.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d8692-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d8692-153">Next steps</span></span>
[<span data-ttu-id="d8692-154">Změnit hello zapnout a vypnout chování hello DIODU</span><span class="sxs-lookup"><span data-stu-id="d8692-154">Change hello on and off behavior of hello LED</span></span>](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md)

