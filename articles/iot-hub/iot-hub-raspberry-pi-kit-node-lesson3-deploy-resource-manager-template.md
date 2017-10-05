---
title: "Připojení k Azure IoT - lekci 3 malin platformy (uzel): nasazení šablony | Microsoft Docs"
description: "Aplikace Azure funkce naslouchá událostem, Azure IoT hub, zpracuje příchozí zprávy a zapisuje je do Azure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "ukládání dat v cloudu, data uložená v cloudu, iot cloudové služby"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 6c58de85-c5c4-4989-bb5e-08c45c549966
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 44901faea37a847a418e6d2b4097302cdb610495
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="612e0-104">Vytvořte aplikaci Azure funkce a účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="612e0-104">Create an Azure function app and Azure storage account</span></span>
<span data-ttu-id="612e0-105">[Azure Functions](../azure-functions/functions-overview.md) je řešení umožňující snadno spouštět *funkce* (malé části kódu) v cloudu.</span><span class="sxs-lookup"><span data-stu-id="612e0-105">[Azure Functions](../azure-functions/functions-overview.md) is a solution for easily running *functions* (small pieces of code) in the cloud.</span></span> <span data-ttu-id="612e0-106">Aplikace Azure funkce hostuje provádění funkcí v Azure.</span><span class="sxs-lookup"><span data-stu-id="612e0-106">An Azure function app hosts the execution of your functions in Azure.</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="612e0-107">Co provedete</span><span class="sxs-lookup"><span data-stu-id="612e0-107">What you will do</span></span>
<span data-ttu-id="612e0-108">Vytvoření aplikace Azure funkce a účet úložiště Azure pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="612e0-108">Use an Azure Resource Manager template to create an Azure function app and an Azure storage account.</span></span> <span data-ttu-id="612e0-109">Aplikace Azure funkce naslouchá událostem, Azure IoT hub, zpracuje příchozí zprávy a zapisuje je do Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="612e0-109">The Azure function app listens to Azure IoT hub events, processes incoming messages, and writes them to Azure Table storage.</span></span> <span data-ttu-id="612e0-110">Pokud máte potíže, hledají řešení na [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="612e0-110">If you have any problems, seek solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="612e0-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="612e0-111">What you will learn</span></span>
<span data-ttu-id="612e0-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="612e0-112">In this article, you will learn:</span></span>

* <span data-ttu-id="612e0-113">Jak používat [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) nasazení prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="612e0-113">How to use [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) to deploy Azure resources.</span></span>
* <span data-ttu-id="612e0-114">Jak používat aplikaci Azure funkce zpracování zpráv centra IoT a jejich zápis do tabulky v Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="612e0-114">How to use an Azure function app to process IoT hub messages and write them to a table in Azure Table storage.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="612e0-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="612e0-115">What you need</span></span>
<span data-ttu-id="612e0-116">Musíte úspěšně jste dokončili:</span><span class="sxs-lookup"><span data-stu-id="612e0-116">You must have successfully completed:</span></span>
* [<span data-ttu-id="612e0-117">Začínáme s malin pí 3</span><span class="sxs-lookup"><span data-stu-id="612e0-117">Get started with Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-node-get-started.md)
* [<span data-ttu-id="612e0-118">Vytvoření služby Azure IoT hub</span><span class="sxs-lookup"><span data-stu-id="612e0-118">Create your Azure IoT hub</span></span>](iot-hub-raspberry-pi-kit-node-get-started.md)

## <a name="open-the-sample-app"></a><span data-ttu-id="612e0-119">Otevřete ukázková aplikace</span><span class="sxs-lookup"><span data-stu-id="612e0-119">Open the sample app</span></span>
<span data-ttu-id="612e0-120">Otevřete ukázkový projekt ve Visual Studio Code spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="612e0-120">Open the sample project in Visual Studio Code by running the following commands:</span></span>

```bash
cd Lesson3
code .
```

![Struktura úložiště](media/iot-hub-raspberry-pi-lessons/lesson3/repo_structure.png)

* <span data-ttu-id="612e0-122">`app.js` v soubor `app` podsložky je klíče zdrojového souboru.</span><span class="sxs-lookup"><span data-stu-id="612e0-122">The `app.js` file in the `app` subfolder is the key source file.</span></span> <span data-ttu-id="612e0-123">Tento zdrojový soubor obsahuje kód, který odešle zprávu 20krát do služby IoT hub a blink DIODU pro každou zprávu, kterou odešle.</span><span class="sxs-lookup"><span data-stu-id="612e0-123">This source file contains the code to send a message 20 times to your IoT hub and blink the LED for each message it sends.</span></span>
* <span data-ttu-id="612e0-124">`arm-template.json` Soubor je šablony Azure Resource Manager, která obsahuje aplikaci Azure funkce a účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="612e0-124">The `arm-template.json` file is the Azure Resource Manager template that contains an Azure function app and an Azure storage account.</span></span>
* <span data-ttu-id="612e0-125">`arm-template-param.json` Soubor je soubor konfigurace používané šablony Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="612e0-125">The `arm-template-param.json` file is the configuration file used by the Azure Resource Manager template.</span></span>
* <span data-ttu-id="612e0-126">`ReceiveDeviceMessages` Podsložky obsahuje kód Node.js pro funkci Azure.</span><span class="sxs-lookup"><span data-stu-id="612e0-126">The `ReceiveDeviceMessages` subfolder contains the Node.js code for the Azure function.</span></span>

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a><span data-ttu-id="612e0-127">Konfigurace šablon Azure Resource Manager a vytváření prostředků v Azure</span><span class="sxs-lookup"><span data-stu-id="612e0-127">Configure Azure Resource Manager templates and create resources in Azure</span></span>
<span data-ttu-id="612e0-128">Aktualizace `arm-template-param.json` soubor ve Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="612e0-128">Update the `arm-template-param.json` file in Visual Studio Code.</span></span>

![Parametry šablony Azure Resource Manager](media/iot-hub-raspberry-pi-lessons/lesson3/arm_para.png)

* <span data-ttu-id="612e0-130">Nahraďte **[název služby IoT Hub]** s **{Moje název centra}** kterou jste zadali při jste [vytvoření služby IoT hub a registraci malin pí 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="612e0-130">Replace **[your IoT Hub name]** with **{my hub name}** that you specified when you [created your IoT hub and registered Raspberry Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).</span></span>
* <span data-ttu-id="612e0-131">Nahraďte **[předpony řetězec pro nové prostředky]** s jakoukoli předponu chcete.</span><span class="sxs-lookup"><span data-stu-id="612e0-131">Replace **[prefix string for new resources]** with any prefix you want.</span></span> <span data-ttu-id="612e0-132">Předpona, která zajišťuje, že název prostředku globálně jedinečný, aby nedošlo ke konfliktu.</span><span class="sxs-lookup"><span data-stu-id="612e0-132">The prefix ensures that the resource name is globally unique to avoid conflict.</span></span> <span data-ttu-id="612e0-133">Nepoužívejte pomlčku nebo číslo počáteční v předponu.</span><span class="sxs-lookup"><span data-stu-id="612e0-133">Do not use a dash or number initial in the prefix.</span></span>

<span data-ttu-id="612e0-134">Po provedení aktualizace `arm-template-param.json` souboru, nasadit tyto prostředky do Azure tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="612e0-134">After you update the `arm-template-param.json` file, deploy the resources to Azure by running the following command:</span></span>

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

<span data-ttu-id="612e0-135">Vytvořte tyto prostředky trvá asi 5 minut.</span><span class="sxs-lookup"><span data-stu-id="612e0-135">It takes about five minutes to create these resources.</span></span> <span data-ttu-id="612e0-136">Při vytvoření prostředku probíhá, můžete přesunout další článek.</span><span class="sxs-lookup"><span data-stu-id="612e0-136">While the resource creation is in progress, you can move on to the next article.</span></span>

## <a name="summary"></a><span data-ttu-id="612e0-137">Souhrn</span><span class="sxs-lookup"><span data-stu-id="612e0-137">Summary</span></span>
<span data-ttu-id="612e0-138">Vytvoření aplikace Azure funkce pro zpracování zprávy IoT hub a účet úložiště Azure slouží k uložení těchto zpráv.</span><span class="sxs-lookup"><span data-stu-id="612e0-138">You've created your Azure function app to process IoT hub messages and an Azure storage account to store these messages.</span></span> <span data-ttu-id="612e0-139">Teď můžete nasadit a spustit ukázkový k odesílání zpráv typu zařízení cloud na pí.</span><span class="sxs-lookup"><span data-stu-id="612e0-139">You can now deploy and run the sample to send device-to-cloud messages on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="612e0-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="612e0-140">Next steps</span></span>
[<span data-ttu-id="612e0-141">Spuštění ukázkové aplikace k odesílání zpráv typu zařízení cloud na malin pí 3</span><span class="sxs-lookup"><span data-stu-id="612e0-141">Run a sample application to send device-to-cloud messages on Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

