---
title: "Připojení k Azure IoT - lekci 3 Intel Edison (uzel): odesílání zpráv | Microsoft Docs"
description: "Nasazení a spuštění ukázkové aplikace pro Edison Intel, která odesílá zprávy do služby IoT hub a bliká Indikátor."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "cloudové služby IOT, arduino odesílat data do cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 1b3b1074-f4d4-42ac-b32c-55f18b304b44
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: d4b520b9a1852a285b1e10b5b35447a54313af9d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a><span data-ttu-id="75ab8-104">Spuštění ukázkové aplikace k odesílání zpráv typu zařízení cloud</span><span class="sxs-lookup"><span data-stu-id="75ab8-104">Run a sample application to send device-to-cloud messages</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="75ab8-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="75ab8-105">What you will do</span></span>
<span data-ttu-id="75ab8-106">Tento článek vám ukáže, jak nasadit a spustit ukázkovou aplikaci na Edison Intel, který odesílá zprávy do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="75ab8-106">This article will show you how to deploy and run a sample application on Intel Edison that sends messages to your IoT hub.</span></span> <span data-ttu-id="75ab8-107">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="75ab8-107">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="75ab8-108">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="75ab8-108">What you will learn</span></span>
<span data-ttu-id="75ab8-109">Se dozvíte, jak používat nástroj gulp k nasazení a spuštění ukázkové aplikace C na Edison.</span><span class="sxs-lookup"><span data-stu-id="75ab8-109">You will learn how to use the gulp tool to deploy and run the sample C application on Edison.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="75ab8-110">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="75ab8-110">What you need</span></span>
* <span data-ttu-id="75ab8-111">Než začnete tuto úlohu, musí úspěšně jste dokončili [vytvořit aplikaci Azure funkce a účet úložiště pro zpracování a ukládání zpráv centra IoT][process-and-store-iot-hub-messages].</span><span class="sxs-lookup"><span data-stu-id="75ab8-111">Before you start this task, you must have successfully completed [Create an Azure function app and a storage account to process and store IoT hub messages][process-and-store-iot-hub-messages].</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="75ab8-112">Získat IoT hub a zařízení připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="75ab8-112">Get your IoT hub and device connection strings</span></span>
<span data-ttu-id="75ab8-113">Připojovací řetězec zařízení slouží k připojení Edison do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="75ab8-113">The device connection string is used to connect Edison to your IoT hub.</span></span> <span data-ttu-id="75ab8-114">IoT hub připojovací řetězec se používá pro připojení služby IoT hub pro identitu zařízení, která představuje Edison ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="75ab8-114">The IoT hub connection string is used to connect your IoT hub to the device identity that represents Edison in the IoT hub.</span></span>

* <span data-ttu-id="75ab8-115">Seznam všech centra IoT ve vaší skupině prostředků spuštěním následujícího příkazu příkazového řádku Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="75ab8-115">List all your IoT hubs in your resource group by running the following Azure CLI command:</span></span>

```bash
az iot hub list -g iot-sample --query [].name
```

<span data-ttu-id="75ab8-116">Použití `iot-sample` jako hodnotu `{resource group name}` Pokud nebylo změňte hodnotu.</span><span class="sxs-lookup"><span data-stu-id="75ab8-116">Use `iot-sample` as the value of `{resource group name}` if you didn't change the value.</span></span>

* <span data-ttu-id="75ab8-117">Spuštěním následujícího příkazu příkazového řádku Azure CLI získáte připojovací řetězec centra IoT:</span><span class="sxs-lookup"><span data-stu-id="75ab8-117">Get the IoT hub connection string by running the following Azure CLI command:</span></span>

```bash
az iot hub show-connection-string --name {my hub name}
```

<span data-ttu-id="75ab8-118">`{my hub name}`je název, který jste zadali při vytvoření služby IoT hub a registraci Edison.</span><span class="sxs-lookup"><span data-stu-id="75ab8-118">`{my hub name}` is the name that you specified when you created your IoT hub and registered Edison.</span></span>

* <span data-ttu-id="75ab8-119">Získáte připojovací řetězec zařízení tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="75ab8-119">Get the device connection string by running the following command:</span></span>

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myinteledison
```

<span data-ttu-id="75ab8-120">Použití `myinteledison` jako hodnotu `{device id}` Pokud nebylo změňte hodnotu.</span><span class="sxs-lookup"><span data-stu-id="75ab8-120">Use `myinteledison` as the value of `{device id}` if you didn't change the value.</span></span>

## <a name="configure-the-device-connection"></a><span data-ttu-id="75ab8-121">Nakonfigurujte připojení zařízení</span><span class="sxs-lookup"><span data-stu-id="75ab8-121">Configure the device connection</span></span>
1. <span data-ttu-id="75ab8-122">Inicializace konfiguračního souboru spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="75ab8-122">Initialize the configuration file by running the following commands:</span></span>

   ```bash
   npm install
   gulp init
   ```

2. <span data-ttu-id="75ab8-123">Otevřete konfigurační soubor zařízení `config-edison.json` ve Visual Studio Code spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="75ab8-123">Open the device configuration file `config-edison.json` in Visual Studio Code by running the following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson3/config.png)
3. <span data-ttu-id="75ab8-125">Zkontrolujte následující nahrazení v `config-edison.json` souboru:</span><span class="sxs-lookup"><span data-stu-id="75ab8-125">Make the following replacements in the `config-edison.json` file:</span></span>

   * <span data-ttu-id="75ab8-126">Nahraďte **[zařízení hostitele nebo IP adresa]** s IP adresou zařízení označit dolů, při konfiguraci zařízení.</span><span class="sxs-lookup"><span data-stu-id="75ab8-126">Replace **[device hostname or IP address]** with the device IP address you marked down when you configured your device.</span></span>
   * <span data-ttu-id="75ab8-127">Nahraďte **[řetězec připojení zařízení IoT]** s `device connection string` jste získali.</span><span class="sxs-lookup"><span data-stu-id="75ab8-127">Replace **[IoT device connection string]** with the `device connection string` you obtained.</span></span>
   * <span data-ttu-id="75ab8-128">Nahraďte **[IoT hub, připojovací řetězec]** s `iot hub connection string` jste získali.</span><span class="sxs-lookup"><span data-stu-id="75ab8-128">Replace **[IoT hub connection string]** with the `iot hub connection string` you obtained.</span></span>

   > [!NOTE]
   > <span data-ttu-id="75ab8-129">Nepotřebujete `azure_storage_connection_string` v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="75ab8-129">You don't need `azure_storage_connection_string` in this article.</span></span> <span data-ttu-id="75ab8-130">Dodržte jako je.</span><span class="sxs-lookup"><span data-stu-id="75ab8-130">Keep it as is.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="75ab8-131">Nasazení a spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="75ab8-131">Deploy and run the sample application</span></span>
<span data-ttu-id="75ab8-132">Nasazení a spuštění ukázkové aplikace na Edison spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="75ab8-132">Deploy and run the sample application on Edison by running the following command:</span></span>

```bash
gulp deploy && gulp run
```

## <a name="verify-that-the-sample-application-works"></a><span data-ttu-id="75ab8-133">Ověřte, že funguje ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="75ab8-133">Verify that the sample application works</span></span>
<span data-ttu-id="75ab8-134">Měli byste vidět DIODU, která je připojena k Edison blikat každé dvě sekundy.</span><span class="sxs-lookup"><span data-stu-id="75ab8-134">You should see the LED that is connected to Edison blinking every two seconds.</span></span> <span data-ttu-id="75ab8-135">Pokaždé, když DIODU bliká, ukázkové aplikace odešle zprávu do služby IoT hub a ověřuje, že zpráva byla úspěšně odeslána do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="75ab8-135">Every time the LED blinks, the sample application sends a message to your IoT hub and verifies that the message has been successfully sent to your IoT hub.</span></span> <span data-ttu-id="75ab8-136">Kromě toho každou zprávu přijme služby IoT hub je vytištěna v okně konzoly.</span><span class="sxs-lookup"><span data-stu-id="75ab8-136">In addition, each message received by the IoT hub is printed in the console window.</span></span> <span data-ttu-id="75ab8-137">Ukázkové aplikace automaticky ukončí po odeslání 20 zpráv.</span><span class="sxs-lookup"><span data-stu-id="75ab8-137">The sample application terminates automatically after sending 20 messages.</span></span>

![Ukázkovou aplikaci s odeslané a přijaté zprávy][sample-application-with-sent-and-received-messages]

## <a name="summary"></a><span data-ttu-id="75ab8-139">Souhrn</span><span class="sxs-lookup"><span data-stu-id="75ab8-139">Summary</span></span>
<span data-ttu-id="75ab8-140">Jste nasazení a spuštění nové aplikace ukázka blikání na Edison k odesílání zpráv typu zařízení cloud do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="75ab8-140">You've deployed and run the new blink sample application on Edison to send device-to-cloud messages to your IoT hub.</span></span> <span data-ttu-id="75ab8-141">Zprávy si můžete nyní sledovat, jak se zapisují na účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="75ab8-141">You now monitor your messages as they are written to the storage account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="75ab8-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="75ab8-142">Next steps</span></span>
<span data-ttu-id="75ab8-143">[Čtení zprávy uchovávané v úložišti Azure][read-messages-persisted-in-azure-storage]</span><span class="sxs-lookup"><span data-stu-id="75ab8-143">[Read messages persisted in Azure Storage][read-messages-persisted-in-azure-storage]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-intel-edison-kit-node-lesson3-deploy-resource-manager-template.md
[sample-application-with-sent-and-received-messages]: media/iot-hub-intel-edison-lessons/lesson3/gulp_run.png
[read-messages-persisted-in-azure-storage]: iot-hub-intel-edison-kit-node-lesson3-read-table-storage.md