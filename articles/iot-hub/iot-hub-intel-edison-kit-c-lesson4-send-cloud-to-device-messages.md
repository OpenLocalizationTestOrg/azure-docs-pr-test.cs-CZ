---
title: "Connect Intel Edison (C) tooAzure IoT - Lekce 4: přijímat zprávy | Microsoft Docs"
description: "Ukázková aplikace běží na Edison a sleduje příchozí zprávy ze služby IoT hub. Nová úloha gulp odešle tooEdison zprávy z vaší IoT hub tooblink hello Indikátor."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "ovládací prvek arduino vedla z webové, arduino řízení vedla přes web"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 820d38f3-d3b8-4249-9e2b-f1b9b771e62f
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: f0424506ff755e0b9514684787b37584d406d320
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-tooreceive-cloud-to-device-messages"></a><span data-ttu-id="83224-105">Spuštění ukázkové aplikace tooreceive zprávy typu cloud zařízení</span><span class="sxs-lookup"><span data-stu-id="83224-105">Run a sample application tooreceive cloud-to-device messages</span></span>
<span data-ttu-id="83224-106">V tomto článku nasadit ukázkovou aplikaci na Intel Edison.</span><span class="sxs-lookup"><span data-stu-id="83224-106">In this article, you deploy a sample application on Intel Edison.</span></span> <span data-ttu-id="83224-107">Ukázková aplikace Hello monitoruje příchozích zpráv ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="83224-107">hello sample application monitors incoming messages from your IoT hub.</span></span> <span data-ttu-id="83224-108">Také spustit úlohu gulp na váš počítač toosend zprávy tooEdison ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="83224-108">You also run a gulp task on your computer toosend messages tooEdison from your IoT hub.</span></span> <span data-ttu-id="83224-109">Zprávy hello přijetí hello ukázkovou aplikaci bliká DIODU hello.</span><span class="sxs-lookup"><span data-stu-id="83224-109">When hello sample application receives hello messages, it blinks hello LED.</span></span> <span data-ttu-id="83224-110">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="83224-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="83224-111">Co provedete</span><span class="sxs-lookup"><span data-stu-id="83224-111">What you will do</span></span>
* <span data-ttu-id="83224-112">Připojte hello ukázkové aplikace tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="83224-112">Connect hello sample application tooyour IoT hub.</span></span>
* <span data-ttu-id="83224-113">Nasazení a spuštění hello ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="83224-113">Deploy and run hello sample application.</span></span>
* <span data-ttu-id="83224-114">Odesílat zprávy z vaší IoT hub tooEdison tooblink hello Indikátor.</span><span class="sxs-lookup"><span data-stu-id="83224-114">Send messages from your IoT hub tooEdison tooblink hello LED.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="83224-115">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="83224-115">What you will learn</span></span>
<span data-ttu-id="83224-116">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="83224-116">In this article, you will learn:</span></span>
* <span data-ttu-id="83224-117">Jak toomonitor příchozích zpráv ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="83224-117">How toomonitor incoming messages from your IoT hub.</span></span>
* <span data-ttu-id="83224-118">Jak toosend cloud zařízení zprávy z vaší tooEdison centra IoT.</span><span class="sxs-lookup"><span data-stu-id="83224-118">How toosend cloud-to-device messages from your IoT hub tooEdison.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="83224-119">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="83224-119">What you need</span></span>
* <span data-ttu-id="83224-120">Intel Edison, nastavte pro použití.</span><span class="sxs-lookup"><span data-stu-id="83224-120">Intel Edison, set up for use.</span></span> <span data-ttu-id="83224-121">jak zjistit, tooset až Edison, toolearn [konfigurace zařízení][configure-your-device].</span><span class="sxs-lookup"><span data-stu-id="83224-121">toolearn how tooset up Edison, see [Configure your device][configure-your-device].</span></span>
* <span data-ttu-id="83224-122">Služby IoT hub, který se vytvoří ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="83224-122">An IoT hub that is created in your Azure subscription.</span></span> <span data-ttu-id="83224-123">toolearn jak toocreate služby IoT hub, najdete v části [vytvoření služby Azure IoT Hub][create-your-azure-iot-hub].</span><span class="sxs-lookup"><span data-stu-id="83224-123">toolearn how toocreate your IoT hub, see [Create your Azure IoT Hub][create-your-azure-iot-hub].</span></span>

## <a name="connect-hello-sample-application-tooyour-iot-hub"></a><span data-ttu-id="83224-124">Připojit hello ukázkové aplikace tooyour IoT hub</span><span class="sxs-lookup"><span data-stu-id="83224-124">Connect hello sample application tooyour IoT hub</span></span>
1. <span data-ttu-id="83224-125">Ujistěte se, že jste ve složce úložišti hello `iot-hub-c-edison-getting-started`.</span><span class="sxs-lookup"><span data-stu-id="83224-125">Make sure that you're in hello repo folder `iot-hub-c-edison-getting-started`.</span></span> <span data-ttu-id="83224-126">Otevřete hello ukázkovou aplikaci v aplikaci Visual Studio Code spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="83224-126">Open hello sample application in Visual Studio Code by running hello following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```

   <span data-ttu-id="83224-127">soubor Hello v hello `app` podsložky je hello klíče zdrojový soubor, který obsahuje hello kód toomonitor příchozí zprávy ze služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="83224-127">hello file in hello `app` subfolder is hello key source file that contains hello code toomonitor incoming messages from hello IoT hub.</span></span> <span data-ttu-id="83224-128">Hello `blinkLED` funkce bliká DIODU hello.</span><span class="sxs-lookup"><span data-stu-id="83224-128">hello `blinkLED` function blinks hello LED.</span></span>

   ![Struktura úložišti v ukázkové aplikaci hello][repo-structure]
2. <span data-ttu-id="83224-130">Inicializovat konfigurační soubor hello spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="83224-130">Initialize hello configuration file by running hello following commands:</span></span>

   ```bash
   npm install
   gulp init
   ```

   <span data-ttu-id="83224-131">Pokud jste dokončili postup hello v [vytvoření účtu Azure funkce aplikace a úložiště] [ create-an-azure-function-app-and-storage-account] v tomto počítači se dědí všechny konfigurace hello, takže hello krok toohello úloh nasazení, můžete přeskočit a spuštění ukázkové aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="83224-131">If you completed hello steps in [Create an Azure function app and storage account][create-an-azure-function-app-and-storage-account] on this computer, all hello configurations are inherited, so you can skip hello step toohello task of deploying and running hello sample application.</span></span> <span data-ttu-id="83224-132">Pokud jste dokončili postup hello v [vytvoření účtu Azure funkce aplikace a úložiště] [ create-an-azure-function-app-and-storage-account] na jiném počítači, musíte tooreplace hello zástupné symboly v hello `config-edison.json` souboru.</span><span class="sxs-lookup"><span data-stu-id="83224-132">If you completed hello steps in [Create an Azure function app and storage account][create-an-azure-function-app-and-storage-account] on a different computer, you need tooreplace hello placeholders in hello `config-edison.json` file.</span></span> <span data-ttu-id="83224-133">Hello `config-edison.json` soubor je v podsložce hello domovskou složku.</span><span class="sxs-lookup"><span data-stu-id="83224-133">hello `config-edison.json` file is in hello subfolder of your home folder.</span></span>

   ![Obsah souboru config edison.json hello](media/iot-hub-intel-edison-lessons/lesson4/config-edison.png)

   * <span data-ttu-id="83224-135">Nahraďte **[zařízení hostitele nebo IP adresa]** s adresou IP zařízení hello označena dolů, při konfiguraci zařízení.</span><span class="sxs-lookup"><span data-stu-id="83224-135">Replace **[device hostname or IP address]** with hello device IP address you marked down when you configured your device.</span></span>
   * <span data-ttu-id="83224-136">Nahraďte **[řetězec připojení zařízení IoT]** s hello zařízení připojovací řetězec, který můžete získat spuštěním hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` příkaz.</span><span class="sxs-lookup"><span data-stu-id="83224-136">Replace **[IoT device connection string]** with hello device connection string that you get by running hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` command.</span></span>
   * <span data-ttu-id="83224-137">Nahraďte **[IoT hub, připojovací řetězec]** s hello IoT hub připojovací řetězec, který můžete získat spuštěním hello `az iot hub show-connection-string --name {my hub name}` příkaz.</span><span class="sxs-lookup"><span data-stu-id="83224-137">Replace **[IoT hub connection string]** with hello IoT hub connection string that you get by running hello `az iot hub show-connection-string --name {my hub name}` command.</span></span>

   > [!NOTE]
   > <span data-ttu-id="83224-138">Spustit **gulp instalace nástroje** i, pokud jste neučinili v lekci 1.</span><span class="sxs-lookup"><span data-stu-id="83224-138">Run **gulp install-tools** as well, if you haven't done it in Lesson 1.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="83224-139">Nasazení a spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="83224-139">Deploy and run hello sample application</span></span>
<span data-ttu-id="83224-140">Nasazení a spuštění ukázkové aplikace hello na Edison spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="83224-140">Deploy and run hello sample application on Edison by running hello following commands:</span></span>

```bash
gulp deploy && gulp run
```

<span data-ttu-id="83224-141">příkaz gulp Hello nasadí hello ukázkové aplikace tooEdison.</span><span class="sxs-lookup"><span data-stu-id="83224-141">hello gulp command deploys hello sample application tooEdison.</span></span> <span data-ttu-id="83224-142">Pak je spuštěna aplikace hello na Edison a samostatná úloha na hostiteli počítače toosend 20 blikání zprávy tooEdison z služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="83224-142">Then, it runs hello application on Edison and a separate task on your host computer toosend 20 blink messages tooEdison from your IoT hub.</span></span>

<span data-ttu-id="83224-143">Po spuštění ukázkové aplikace hello začne naslouchat toomessages ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="83224-143">After hello sample application runs, it starts listening toomessages from your IoT hub.</span></span> <span data-ttu-id="83224-144">Mezitím hello gulp úloh odešle několik "blink" zprávy z vaší tooEdison centra IoT.</span><span class="sxs-lookup"><span data-stu-id="83224-144">Meanwhile, hello gulp task sends several "blink" messages from your IoT hub tooEdison.</span></span> <span data-ttu-id="83224-145">Pro každou blikání zprávu, která přijímá Edison, hello ukázkové aplikace volá hello `blinkLED` funkce tooblink hello Indikátor.</span><span class="sxs-lookup"><span data-stu-id="83224-145">For each blink message that Edison receives, hello sample application calls hello `blinkLED` function tooblink hello LED.</span></span>

<span data-ttu-id="83224-146">Měli byste vidět blikání DIODU hello každé dvě sekundy jako hello gulp úloh zasílá 20 zprávy z vaší tooEdison centra IoT.</span><span class="sxs-lookup"><span data-stu-id="83224-146">You should see hello LED blink every two seconds as hello gulp task sends 20 messages from your IoT hub tooEdison.</span></span> <span data-ttu-id="83224-147">Hello poslední jedním je zpráva "stop", která ukončí hello spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="83224-147">hello last one is a "stop" message that stops hello application from running.</span></span>

![Ukázkovou aplikaci s gulp příkazu a blink – zprávy][gulp-command-and-blink-messages]

## <a name="summary"></a><span data-ttu-id="83224-149">Souhrn</span><span class="sxs-lookup"><span data-stu-id="83224-149">Summary</span></span>
<span data-ttu-id="83224-150">Z vaší IoT hub tooEdison tooblink hello DIODU úspěšně odeslat zprávy.</span><span class="sxs-lookup"><span data-stu-id="83224-150">You’ve successfully sent messages from your IoT hub tooEdison tooblink hello LED.</span></span> <span data-ttu-id="83224-151">Další úlohou Hello je volitelné: Změna hello zapnout a vypnout chování hello Indikátor.</span><span class="sxs-lookup"><span data-stu-id="83224-151">hello next task is optional: change hello on and off behavior of hello LED.</span></span>

## <a name="next-steps"></a><span data-ttu-id="83224-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="83224-152">Next steps</span></span>
<span data-ttu-id="83224-153">[Změnit hello zapnout a vypnout chování hello DIODU][change-the-on-and-off-behavior-of-the-led]</span><span class="sxs-lookup"><span data-stu-id="83224-153">[Change hello on and off behavior of hello LED][change-the-on-and-off-behavior-of-the-led]</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[configure-your-device]: iot-hub-intel-edison-kit-c-lesson1-configure-your-device.md
[create-your-azure-iot-hub]: iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson4/repo_structure_c.png
[create-an-azure-function-app-and-storage-account]: iot-hub-intel-edison-kit-c-lesson3-deploy-resource-manager-template.md
[gulp-command-and-blink-messages]: media/iot-hub-intel-edison-lessons/lesson4/gulp_blink_c.png
[change-the-on-and-off-behavior-of-the-led]: iot-hub-intel-edison-kit-c-lesson4-change-led-behavior.md