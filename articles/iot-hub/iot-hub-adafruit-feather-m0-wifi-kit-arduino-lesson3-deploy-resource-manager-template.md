---
title: "Connect Arduino (C) k Azure IoT - Lekce 3: nasazení šablony | Microsoft Docs"
description: "Aplikace Azure funkce naslouchá událostem, Azure IoT hub, zpracuje příchozí zprávy a zapisuje je do Azure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "ukládání dat v cloudu, data uložená v cloudu, iot cloudové služby"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 9c8f4cd1-9511-4601-ad7e-51761a986753
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: be6105927645ae2ec56f6885c61dbcb6faf5b11f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="cd624-104">Vytvořte aplikaci Azure funkce a účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="cd624-104">Create an Azure function app and Azure storage account</span></span>
<span data-ttu-id="cd624-105">[Azure Functions](../../articles/azure-functions/functions-overview.md) je řešení umožňující snadno spouštět *funkce* (malé části kódu) v cloudu.</span><span class="sxs-lookup"><span data-stu-id="cd624-105">[Azure Functions](../../articles/azure-functions/functions-overview.md) is a solution for easily running *functions* (small pieces of code) in the cloud.</span></span> <span data-ttu-id="cd624-106">Aplikace Azure funkce hostuje provádění funkcí v Azure.</span><span class="sxs-lookup"><span data-stu-id="cd624-106">An Azure function app hosts the execution of your functions in Azure.</span></span>

## <a name="what-will-you-do"></a><span data-ttu-id="cd624-107">Co se dělat</span><span class="sxs-lookup"><span data-stu-id="cd624-107">What will you do</span></span>
<span data-ttu-id="cd624-108">Vytvoření aplikace Azure funkce a účet úložiště Azure pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="cd624-108">Use an Azure Resource Manager template to create an Azure function app and an Azure storage account.</span></span> <span data-ttu-id="cd624-109">Aplikace Azure funkce naslouchá událostem, Azure IoT hub, zpracuje příchozí zprávy a zapisuje je do Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="cd624-109">The Azure function app listens to Azure IoT hub events, processes incoming messages, and writes them to Azure Table storage.</span></span>

<span data-ttu-id="cd624-110">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky pro panel Adafruit prolnutí M0 Wi-Fi Arduino](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="cd624-110">If you have any problems, look for solutions on the [troubleshooting page for your Adafruit Feather M0 WiFi Arduino board](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).</span></span>

## <a name="what-will-you-learn"></a><span data-ttu-id="cd624-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="cd624-111">What will you learn</span></span>
<span data-ttu-id="cd624-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="cd624-112">In this article, you will learn:</span></span>
* <span data-ttu-id="cd624-113">Jak používat [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) nasazení prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="cd624-113">How to use [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) to deploy Azure resources.</span></span>
* <span data-ttu-id="cd624-114">Jak používat aplikaci Azure funkce zpracování zpráv centra IoT a jejich zápis do tabulky v Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="cd624-114">How to use an Azure function app to process IoT hub messages and write them to a table in Azure Table storage.</span></span>

## <a name="what-do-you-need"></a><span data-ttu-id="cd624-115">Co je potřeba</span><span class="sxs-lookup"><span data-stu-id="cd624-115">What do you need</span></span>
<span data-ttu-id="cd624-116">Musíte úspěšně jste dokončili:</span><span class="sxs-lookup"><span data-stu-id="cd624-116">You must have successfully completed:</span></span>
- <span data-ttu-id="cd624-117">[Začínáme s Arduino panel][get-started]</span><span class="sxs-lookup"><span data-stu-id="cd624-117">[Get started with your Arduino board][get-started]</span></span>
- <span data-ttu-id="cd624-118">[Vytvoření služby Azure IoT hub][create-iot-hub]</span><span class="sxs-lookup"><span data-stu-id="cd624-118">[Create your Azure IoT hub][create-iot-hub]</span></span>

## <a name="open-the-sample-app"></a><span data-ttu-id="cd624-119">Otevřete ukázková aplikace</span><span class="sxs-lookup"><span data-stu-id="cd624-119">Open the sample app</span></span>
<span data-ttu-id="cd624-120">Otevřete ukázkový projekt ve Visual Studio Code spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="cd624-120">Open the sample project in Visual Studio Code by running the following commands:</span></span>

```bash
cd Lesson3
code .
```

![Struktura úložiště][repo-structure]

* <span data-ttu-id="cd624-122">`app.ino` v soubor `app` podsložky je klíče zdrojového souboru.</span><span class="sxs-lookup"><span data-stu-id="cd624-122">The `app.ino` file in the `app` subfolder is the key source file.</span></span> <span data-ttu-id="cd624-123">Tento zdrojový soubor obsahuje kód, který odešle zprávu 20krát do služby IoT hub a blink DIODU pro každou zprávu, kterou odešle.</span><span class="sxs-lookup"><span data-stu-id="cd624-123">This source file contains the code to send a message 20 times to your IoT hub and blink the LED for each message it sends.</span></span>
* <span data-ttu-id="cd624-124">`config.json` Obsahuje požadovaná nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="cd624-124">The `config.json` contains required configuration settings.</span></span>
* <span data-ttu-id="cd624-125">`arm-template.json` Soubor je šablony Azure Resource Manager, která obsahuje aplikaci Azure funkce a účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="cd624-125">The `arm-template.json` file is the Azure Resource Manager template that contains an Azure function app and an Azure storage account.</span></span>
* <span data-ttu-id="cd624-126">`arm-template-param.json` Soubor je soubor konfigurace používané šablony Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="cd624-126">The `arm-template-param.json` file is the configuration file used by the Azure Resource Manager template.</span></span>
* <span data-ttu-id="cd624-127">`ReceiveDeviceMessages` Podsložky obsahuje kód Node.js pro funkci Azure.</span><span class="sxs-lookup"><span data-stu-id="cd624-127">The `ReceiveDeviceMessages` subfolder contains the Node.js code for the Azure function.</span></span>

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a><span data-ttu-id="cd624-128">Konfigurace šablon Azure Resource Manager a vytváření prostředků v Azure</span><span class="sxs-lookup"><span data-stu-id="cd624-128">Configure Azure Resource Manager templates and create resources in Azure</span></span>
<span data-ttu-id="cd624-129">Aktualizace `arm-template-param.json` soubor ve Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="cd624-129">Update the `arm-template-param.json` file in Visual Studio Code.</span></span>

![Parametry šablony Azure Resource Manager][arm-template-params]

* <span data-ttu-id="cd624-131">Nahraďte **[název služby IoT Hub]** s **{Moje název centra}** kterou jste zadali při jste [vytvoření služby IoT hub a registraci panel Arduino][created-iot-hub-and-registered-arduino-board].</span><span class="sxs-lookup"><span data-stu-id="cd624-131">Replace **[your IoT Hub name]** with **{my hub name}** that you specified when you [created your IoT hub and registered your Arduino board][created-iot-hub-and-registered-arduino-board].</span></span>
* <span data-ttu-id="cd624-132">Nahraďte **[předpony řetězec pro nové prostředky]** s jakoukoli předponu chcete.</span><span class="sxs-lookup"><span data-stu-id="cd624-132">Replace **[prefix string for new resources]** with any prefix you want.</span></span> <span data-ttu-id="cd624-133">Předpona, která zajišťuje, že název prostředku globálně jedinečný, aby nedošlo ke konfliktu.</span><span class="sxs-lookup"><span data-stu-id="cd624-133">The prefix ensures that the resource name is globally unique to avoid conflict.</span></span> <span data-ttu-id="cd624-134">Nepoužívejte pomlčku nebo číslo počáteční v předponu.</span><span class="sxs-lookup"><span data-stu-id="cd624-134">Do not use a dash or number initial in the prefix.</span></span>

<span data-ttu-id="cd624-135">Po provedení aktualizace `arm-template-param.json` souboru, nasadit tyto prostředky do Azure tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="cd624-135">After you update the `arm-template-param.json` file, deploy the resources to Azure by running the following command:</span></span>

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

<span data-ttu-id="cd624-136">Vytvořte tyto prostředky trvá asi 5 minut.</span><span class="sxs-lookup"><span data-stu-id="cd624-136">It takes about five minutes to create these resources.</span></span> <span data-ttu-id="cd624-137">Při vytvoření prostředku probíhá, můžete přesunout další článek.</span><span class="sxs-lookup"><span data-stu-id="cd624-137">While the resource creation is in progress, you can move on to the next article.</span></span>

## <a name="summary"></a><span data-ttu-id="cd624-138">Souhrn</span><span class="sxs-lookup"><span data-stu-id="cd624-138">Summary</span></span>
<span data-ttu-id="cd624-139">Vytvoření aplikace Azure funkce pro zpracování zprávy IoT hub a účet úložiště Azure slouží k uložení těchto zpráv.</span><span class="sxs-lookup"><span data-stu-id="cd624-139">You've created your Azure function app to process IoT hub messages and an Azure storage account to store these messages.</span></span> <span data-ttu-id="cd624-140">Teď můžete nasadit a spustit ukázkový k odesílání zpráv typu zařízení cloud na vaší kartě Arduino.</span><span class="sxs-lookup"><span data-stu-id="cd624-140">You can now deploy and run the sample to send device-to-cloud messages on your Arduino board.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cd624-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cd624-141">Next steps</span></span>
<span data-ttu-id="cd624-142">[Spuštění ukázkové aplikace k odesílání zpráv typu zařízení cloud na vaší kartě Arduino][send-device-to-cloud-messages]</span><span class="sxs-lookup"><span data-stu-id="cd624-142">[Run a sample application to send device-to-cloud messages on your Arduino board][send-device-to-cloud-messages]</span></span>

<!-- Images and links -->

[get-started]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[create-iot-hub]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/repo_structure_c.png
[arm-template-params]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/arm_para_arduino.png
[created-iot-hub-and-registered-arduino-board]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[send-device-to-cloud-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-run-azure-blink.md