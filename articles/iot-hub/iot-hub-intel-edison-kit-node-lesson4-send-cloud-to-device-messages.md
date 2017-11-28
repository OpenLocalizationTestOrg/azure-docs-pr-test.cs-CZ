---
title: "Připojení k Azure IoT - lekci 4 Intel Edison (uzel): přijímat zprávy | Microsoft Docs"
description: "Ukázková aplikace běží na Edison a sleduje příchozí zprávy ze služby IoT hub. Nová úloha gulp odesílá zprávy Edison ze služby IoT hub blikání Indikátor."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "ovládací prvek arduino vedla z webové, arduino řízení vedla přes web"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: bc738bf6-e38d-4024-82d7-39b6c2d4bacb
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 76ea59acd848f60663a0c821bff42166aac5823a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="run-a-sample-application-to-receive-cloud-to-device-messages"></a><span data-ttu-id="e8445-105">Spuštění ukázkové aplikace pro příjem zpráv typu cloud zařízení</span><span class="sxs-lookup"><span data-stu-id="e8445-105">Run a sample application to receive cloud-to-device messages</span></span>
<span data-ttu-id="e8445-106">V tomto článku nasadit ukázkovou aplikaci na Intel Edison.</span><span class="sxs-lookup"><span data-stu-id="e8445-106">In this article, you deploy a sample application on Intel Edison.</span></span> <span data-ttu-id="e8445-107">Ukázkovou aplikaci monitoruje příchozích zpráv ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e8445-107">The sample application monitors incoming messages from your IoT hub.</span></span> <span data-ttu-id="e8445-108">Můžete taky spustit úlohu gulp ve vašem počítači k odesílání zpráv do Edison ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e8445-108">You also run a gulp task on your computer to send messages to Edison from your IoT hub.</span></span> <span data-ttu-id="e8445-109">Zprávy přijetí ukázkovou aplikaci bliká Indikátor.</span><span class="sxs-lookup"><span data-stu-id="e8445-109">When the sample application receives the messages, it blinks the LED.</span></span> <span data-ttu-id="e8445-110">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="e8445-110">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="e8445-111">Co provedete</span><span class="sxs-lookup"><span data-stu-id="e8445-111">What you will do</span></span>
* <span data-ttu-id="e8445-112">Připojte ukázkovou aplikaci do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e8445-112">Connect the sample application to your IoT hub.</span></span>
* <span data-ttu-id="e8445-113">Nasazení a spuštění ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e8445-113">Deploy and run the sample application.</span></span>
* <span data-ttu-id="e8445-114">Odesílat zprávy ze služby IoT hub Edison blikání Indikátor.</span><span class="sxs-lookup"><span data-stu-id="e8445-114">Send messages from your IoT hub to Edison to blink the LED.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="e8445-115">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="e8445-115">What you will learn</span></span>
<span data-ttu-id="e8445-116">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="e8445-116">In this article, you will learn:</span></span>
* <span data-ttu-id="e8445-117">Postup sledování příchozích zpráv ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e8445-117">How to monitor incoming messages from your IoT hub.</span></span>
* <span data-ttu-id="e8445-118">Postupy pro odesílání zpráv typu cloud zařízení ze služby IoT hub pro Edison.</span><span class="sxs-lookup"><span data-stu-id="e8445-118">How to send cloud-to-device messages from your IoT hub to Edison.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="e8445-119">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="e8445-119">What you need</span></span>
* <span data-ttu-id="e8445-120">Intel Edison, nastavte pro použití.</span><span class="sxs-lookup"><span data-stu-id="e8445-120">Intel Edison, set up for use.</span></span> <span data-ttu-id="e8445-121">Další informace o nastavení Edison naleznete v tématu [konfigurace zařízení][configure-your-device].</span><span class="sxs-lookup"><span data-stu-id="e8445-121">To learn how to set up Edison, see [Configure your device][configure-your-device].</span></span>
* <span data-ttu-id="e8445-122">Služby IoT hub, který se vytvoří ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="e8445-122">An IoT hub that is created in your Azure subscription.</span></span> <span data-ttu-id="e8445-123">Informace o vytvoření služby IoT hub, najdete v tématu [vytvoření služby Azure IoT Hub][create-your-azure-iot-hub].</span><span class="sxs-lookup"><span data-stu-id="e8445-123">To learn how to create your IoT hub, see [Create your Azure IoT Hub][create-your-azure-iot-hub].</span></span>

## <a name="connect-the-sample-application-to-your-iot-hub"></a><span data-ttu-id="e8445-124">Připojit ukázkovou aplikaci do služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="e8445-124">Connect the sample application to your IoT hub</span></span>
1. <span data-ttu-id="e8445-125">Ujistěte se, že jste ve složce úložišti `iot-hub-node-edison-getting-started`.</span><span class="sxs-lookup"><span data-stu-id="e8445-125">Make sure that you're in the repo folder `iot-hub-node-edison-getting-started`.</span></span> <span data-ttu-id="e8445-126">Otevřete ukázkovou aplikaci v aplikaci Visual Studio Code spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="e8445-126">Open the sample application in Visual Studio Code by running the following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```

   <span data-ttu-id="e8445-127">Soubor v `app` podsložky je klíče zdrojový soubor, který obsahuje kód ke sledování příchozích zpráv ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e8445-127">The file in the `app` subfolder is the key source file that contains the code to monitor incoming messages from the IoT hub.</span></span> <span data-ttu-id="e8445-128">`blinkLED` Funkce bliká Indikátor.</span><span class="sxs-lookup"><span data-stu-id="e8445-128">The `blinkLED` function blinks the LED.</span></span>

   ![Struktura úložišti v ukázkové aplikace][repo-structure]
2. <span data-ttu-id="e8445-130">Inicializace konfiguračního souboru spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="e8445-130">Initialize the configuration file by running the following commands:</span></span>

   ```bash
   npm install
   gulp init
   ```

   <span data-ttu-id="e8445-131">Pokud jste dokončili kroky v [vytvoření účtu Azure funkce aplikace a úložiště] [ create-an-azure-function-app-and-storage-account] v tomto počítači se dědí všechny konfigurace, takže můžete přeskočit na krok úlohy nasazení a spuštění ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e8445-131">If you completed the steps in [Create an Azure function app and storage account][create-an-azure-function-app-and-storage-account] on this computer, all the configurations are inherited, so you can skip the step to the task of deploying and running the sample application.</span></span> <span data-ttu-id="e8445-132">Pokud jste dokončili kroky v [vytvoření účtu Azure funkce aplikace a úložiště] [ create-an-azure-function-app-and-storage-account] na jiném počítači, budete muset nahraďte zástupné symboly v `config-edison.json` souboru.</span><span class="sxs-lookup"><span data-stu-id="e8445-132">If you completed the steps in [Create an Azure function app and storage account][create-an-azure-function-app-and-storage-account] on a different computer, you need to replace the placeholders in the `config-edison.json` file.</span></span> <span data-ttu-id="e8445-133">`config-edison.json` Soubor je v podsložce domovskou složku.</span><span class="sxs-lookup"><span data-stu-id="e8445-133">The `config-edison.json` file is in the subfolder of your home folder.</span></span>

   ![Obsah souboru config edison.json](media/iot-hub-intel-edison-lessons/lesson4/config-edison.png)

   * <span data-ttu-id="e8445-135">Nahraďte **[zařízení hostitele nebo IP adresa]** s IP adresou zařízení označit dolů, při konfiguraci zařízení.</span><span class="sxs-lookup"><span data-stu-id="e8445-135">Replace **[device hostname or IP address]** with the device IP address you marked down when you configured your device.</span></span>
   * <span data-ttu-id="e8445-136">Nahraďte **[řetězec připojení zařízení IoT]** připojovacím řetězcem zařízení, kterou můžete získat spuštěním `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` příkaz.</span><span class="sxs-lookup"><span data-stu-id="e8445-136">Replace **[IoT device connection string]** with the device connection string that you get by running the `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` command.</span></span>
   * <span data-ttu-id="e8445-137">Nahraďte **[IoT hub, připojovací řetězec]** s IoT hub připojovací řetězec, který získáte spuštěním `az iot hub show-connection-string --name {my hub name}` příkaz.</span><span class="sxs-lookup"><span data-stu-id="e8445-137">Replace **[IoT hub connection string]** with the IoT hub connection string that you get by running the `az iot hub show-connection-string --name {my hub name}` command.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="e8445-138">Nasazení a spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="e8445-138">Deploy and run the sample application</span></span>
<span data-ttu-id="e8445-139">Nasazení a spuštění ukázkové aplikace na Edison spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="e8445-139">Deploy and run the sample application on Edison by running the following commands:</span></span>

```bash
gulp deploy && gulp run
```

<span data-ttu-id="e8445-140">Příkaz gulp nasadí ukázkovou aplikaci pro Edison.</span><span class="sxs-lookup"><span data-stu-id="e8445-140">The gulp command deploys the sample application to Edison.</span></span> <span data-ttu-id="e8445-141">Pak spustí aplikaci na Edison a samostatná úloha v hostitelském počítači odesílat zprávy 20 blikání Edison ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e8445-141">Then, it runs the application on Edison and a separate task on your host computer to send 20 blink messages to Edison from your IoT hub.</span></span>

<span data-ttu-id="e8445-142">Po spuštění ukázkové aplikace, začne naslouchání zpráv ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e8445-142">After the sample application runs, it starts listening to messages from your IoT hub.</span></span> <span data-ttu-id="e8445-143">Úloha gulp mezitím, odešle do Edison několik "blink" zpráv ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e8445-143">Meanwhile, the gulp task sends several "blink" messages from your IoT hub to Edison.</span></span> <span data-ttu-id="e8445-144">Pro každou blikání zprávu, která přijímá Edison, volá ukázkovou aplikaci `blinkLED` funkce blikání Indikátor.</span><span class="sxs-lookup"><span data-stu-id="e8445-144">For each blink message that Edison receives, the sample application calls the `blinkLED` function to blink the LED.</span></span>

<span data-ttu-id="e8445-145">Jako úlohu gulp odešle 20 zpráv ze služby IoT hub Edison, měli byste vidět blikání DIODU každé dvě sekundy.</span><span class="sxs-lookup"><span data-stu-id="e8445-145">You should see the LED blink every two seconds as the gulp task sends 20 messages from your IoT hub to Edison.</span></span> <span data-ttu-id="e8445-146">Poslední je "stop" zprávy, která zabrání spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="e8445-146">The last one is a "stop" message that stops the application from running.</span></span>

![Ukázkovou aplikaci s gulp příkazu a blink – zprávy][gulp-command-and-blink-messages]

## <a name="summary"></a><span data-ttu-id="e8445-148">Souhrn</span><span class="sxs-lookup"><span data-stu-id="e8445-148">Summary</span></span>
<span data-ttu-id="e8445-149">Úspěšně jste odeslali zpráv ze služby IoT hub do Edison blikání Indikátor.</span><span class="sxs-lookup"><span data-stu-id="e8445-149">You’ve successfully sent messages from your IoT hub to Edison to blink the LED.</span></span> <span data-ttu-id="e8445-150">Dalším krokem je volitelné: Změna zapnuto a vypnuto chování Indikátor.</span><span class="sxs-lookup"><span data-stu-id="e8445-150">The next task is optional: change the on and off behavior of the LED.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e8445-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e8445-151">Next steps</span></span>
<span data-ttu-id="e8445-152">[Změnit zapnuto a vypnuto chování Indikátor][change-the-on-and-off-behavior-of-the-led]</span><span class="sxs-lookup"><span data-stu-id="e8445-152">[Change the on and off behavior of the LED][change-the-on-and-off-behavior-of-the-led]</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[configure-your-device]: iot-hub-intel-edison-kit-node-lesson1-configure-your-device.md
[create-your-azure-iot-hub]: iot-hub-intel-edison-kit-node-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson4/repo_structure.png
[create-an-azure-function-app-and-storage-account]: iot-hub-intel-edison-kit-node-lesson3-deploy-resource-manager-template.md
[gulp-command-and-blink-messages]: media/iot-hub-intel-edison-lessons/lesson4/gulp_blink.png
[change-the-on-and-off-behavior-of-the-led]: iot-hub-intel-edison-kit-node-lesson4-change-led-behavior.md