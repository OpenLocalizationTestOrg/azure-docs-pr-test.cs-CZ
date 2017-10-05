---
title: "Připojení k Azure IoT - lekci 3 malin platformy (uzel): ukázku spustit | Microsoft Docs"
description: "Nasazení a spuštění ukázkové aplikace na 3 malin platformy, která odesílá zprávy do služby IoT hub a bliká Indikátor."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "blikání vedla cloudové platformy, vedla blikání z cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 427d8e5e-8af8-4249-8607-44edc90a4972
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 1c03283ee276a954f822d6eca5f0a3d5f93ec64b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a><span data-ttu-id="81d63-104">Spuštění ukázkové aplikace k odesílání zpráv typu zařízení cloud</span><span class="sxs-lookup"><span data-stu-id="81d63-104">Run a sample application to send device-to-cloud messages</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="81d63-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="81d63-105">What you will do</span></span>
<span data-ttu-id="81d63-106">Tento článek vám ukáže, jak nasadit a spustit ukázkovou aplikaci na 3 malin platformy, která odesílá zprávy do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="81d63-106">This article will show you how to deploy and run a sample application on Raspberry Pi 3 that sends messages to your IoT hub.</span></span> <span data-ttu-id="81d63-107">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="81d63-107">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="81d63-108">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="81d63-108">What you will learn</span></span>
<span data-ttu-id="81d63-109">Se dozvíte, jak používat nástroj gulp k nasazení a spuštění ukázkové aplikace Node.js na pí.</span><span class="sxs-lookup"><span data-stu-id="81d63-109">You will learn how to use the gulp tool to deploy and run the sample Node.js application on Pi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="81d63-110">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="81d63-110">What you need</span></span>
* <span data-ttu-id="81d63-111">Než začnete tuto úlohu, musí úspěšně jste dokončili [vytvořit aplikaci Azure funkce a účet úložiště pro zpracování a ukládání IoT Centrum zpráv](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="81d63-111">Before you start this task, you must have successfully completed [Create an Azure function app and a storage account to process and store IoT hub messages](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md).</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="81d63-112">Získat IoT hub a zařízení připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="81d63-112">Get your IoT hub and device connection strings</span></span>
<span data-ttu-id="81d63-113">Zařízení připojovací řetězec se používá ve vašem platformy pro připojení do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="81d63-113">The device connection string is used by your Pi to connect to your IoT hub.</span></span> <span data-ttu-id="81d63-114">IoT hub připojovací řetězec se používá pro připojení k registru identit ve službě IoT hub pro správu zařízení, které se mohou připojit ke službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="81d63-114">The IoT hub connection string is used to connect to the identity registry in your IoT hub to manage the devices that are allowed to connect to your IoT hub.</span></span> 

* <span data-ttu-id="81d63-115">Seznam všech centra IoT ve vaší skupině prostředků spuštěním následujícího příkazu příkazového řádku Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="81d63-115">List all your IoT hubs in your resource group by running the following Azure CLI command:</span></span>

```bash
az iot hub list -g iot-sample --query [].name
```

<span data-ttu-id="81d63-116">Použití `iot-sample` jako hodnotu `{resource group name}` Pokud nebylo změňte hodnotu.</span><span class="sxs-lookup"><span data-stu-id="81d63-116">Use `iot-sample` as the value of `{resource group name}` if you didn't change the value.</span></span>

* <span data-ttu-id="81d63-117">Spuštěním následujícího příkazu příkazového řádku Azure CLI získáte připojovací řetězec centra IoT:</span><span class="sxs-lookup"><span data-stu-id="81d63-117">Get the IoT hub connection string by running the following Azure CLI command:</span></span>

```bash
az iot hub show-connection-string --name {my hub name} -g iot-sample
```

<span data-ttu-id="81d63-118">`{my hub name}`je název, který jste zadali při vytvoření služby IoT hub a registraci pí.</span><span class="sxs-lookup"><span data-stu-id="81d63-118">`{my hub name}` is the name that you specified when you created your IoT hub and registered Pi.</span></span>

* <span data-ttu-id="81d63-119">Získáte připojovací řetězec zařízení tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="81d63-119">Get the device connection string by running the following command:</span></span>

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myraspberrypi -g iot-sample
```

<span data-ttu-id="81d63-120">Použití `myraspberrypi` jako hodnotu `{device id}` Pokud nebylo změňte hodnotu.</span><span class="sxs-lookup"><span data-stu-id="81d63-120">Use `myraspberrypi` as the value of `{device id}` if you didn't change the value.</span></span>

## <a name="configure-the-device-connection"></a><span data-ttu-id="81d63-121">Nakonfigurujte připojení zařízení</span><span class="sxs-lookup"><span data-stu-id="81d63-121">Configure the device connection</span></span>
1. <span data-ttu-id="81d63-122">Inicializace konfiguračního souboru spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="81d63-122">Initialize the configuration file by running the following commands:</span></span>
   
   ```bash
   npm install
   gulp init
   ```
2. <span data-ttu-id="81d63-123">Otevřete konfigurační soubor zařízení `config-raspberrypi.json` ve Visual Studio Code spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="81d63-123">Open the device configuration file `config-raspberrypi.json` in Visual Studio Code by running the following command:</span></span>
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
  
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
  
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson3/config.png)
3. <span data-ttu-id="81d63-125">Zkontrolujte následující nahrazení v `config-raspberrypi.json` souboru:</span><span class="sxs-lookup"><span data-stu-id="81d63-125">Make the following replacements in the `config-raspberrypi.json` file:</span></span>
   
   * <span data-ttu-id="81d63-126">Nahraďte **[zařízení hostitele nebo IP adresa]** se zařízení IP adresu nebo název hostitele, který jste získali z `device-discovery-cli` nebo s hodnotou zděděná při konfiguraci zařízení.</span><span class="sxs-lookup"><span data-stu-id="81d63-126">Replace **[device hostname or IP address]** with the device IP address or host name you got from `device-discovery-cli` or with the value inherited when you configured your device.</span></span>
   * <span data-ttu-id="81d63-127">Nahraďte **[řetězec připojení zařízení IoT]** s `device connection string` jste získali.</span><span class="sxs-lookup"><span data-stu-id="81d63-127">Replace **[IoT device connection string]** with the `device connection string` you obtained.</span></span>
   * <span data-ttu-id="81d63-128">Nahraďte **[IoT hub, připojovací řetězec]** s `iot hub connection string` jste získali.</span><span class="sxs-lookup"><span data-stu-id="81d63-128">Replace **[IoT hub connection string]** with the `iot hub connection string` you obtained.</span></span>

<span data-ttu-id="81d63-129">Aktualizace `config-raspberrypi.json` souborů, abyste mohli nasadit ukázkovou aplikaci z vašeho počítače.</span><span class="sxs-lookup"><span data-stu-id="81d63-129">Update the `config-raspberrypi.json` file so that you can deploy the sample application from your computer.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="81d63-130">Nasazení a spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="81d63-130">Deploy and run the sample application</span></span>
<span data-ttu-id="81d63-131">Nasazení a spuštění ukázkové aplikace na platformy spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="81d63-131">Deploy and run the sample application on Pi by running the following command:</span></span>

```bash
gulp deploy && gulp run
```

## <a name="verify-that-the-sample-application-works"></a><span data-ttu-id="81d63-132">Ověřte, že funguje ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="81d63-132">Verify that the sample application works</span></span>
<span data-ttu-id="81d63-133">Měli byste vidět DIODU, která je připojena k pí blikat každé dvě sekundy.</span><span class="sxs-lookup"><span data-stu-id="81d63-133">You should see the LED that is connected to Pi blinking every two seconds.</span></span> <span data-ttu-id="81d63-134">Pokaždé, když DIODU bliká, ukázkové aplikace odešle zprávu do služby IoT hub a ověřuje, že zpráva byla úspěšně odeslána do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="81d63-134">Every time the LED blinks, the sample application sends a message to your IoT hub and verifies that the message has been successfully sent to your IoT hub.</span></span> <span data-ttu-id="81d63-135">Kromě toho každou zprávu přijme služby IoT hub je vytištěna v okně konzoly.</span><span class="sxs-lookup"><span data-stu-id="81d63-135">In addition, each message received by the IoT hub is printed in the console window.</span></span> <span data-ttu-id="81d63-136">Ukázkové aplikace automaticky ukončí po odeslání 20 zpráv.</span><span class="sxs-lookup"><span data-stu-id="81d63-136">The sample application terminates automatically after sending 20 messages.</span></span>

![Ukázkovou aplikaci s odeslané a přijaté zprávy](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_run.png)

## <a name="summary"></a><span data-ttu-id="81d63-138">Souhrn</span><span class="sxs-lookup"><span data-stu-id="81d63-138">Summary</span></span>
<span data-ttu-id="81d63-139">Jste nasazení a spuštění nové aplikace ukázka blikání na platformy k odesílání zpráv typu zařízení cloud do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="81d63-139">You've deployed and run the new blink sample application on Pi to send device-to-cloud messages to your IoT hub.</span></span> <span data-ttu-id="81d63-140">Nyní můžete sledovat vaše zprávy jako jsou zapsány do účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="81d63-140">You can now monitor your messages as they are written to the storage account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="81d63-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="81d63-141">Next steps</span></span>
[<span data-ttu-id="81d63-142">Čtení zprávy uchovávané v úložišti Azure</span><span class="sxs-lookup"><span data-stu-id="81d63-142">Read messages persisted in Azure Storage</span></span>](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)
