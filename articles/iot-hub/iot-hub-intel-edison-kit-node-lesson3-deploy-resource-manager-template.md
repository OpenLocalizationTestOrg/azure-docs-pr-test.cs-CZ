---
title: "Intel Edison (uzel) připojit k Azure IoT - Lekce 3: vytvoření aplikace pro funkce | Microsoft Docs"
description: "Aplikace Azure funkce naslouchá událostem, Azure IoT hub, zpracuje příchozí zprávy a zapisuje je do Azure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "ukládání dat v cloudu, data uložená v cloudu, iot cloudové služby"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 37ee5962-95ce-40e8-8162-17e735eaec21
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 6ada1cbbb560f1373346eca561dceb28d7ca7242
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="52dc4-104">Vytvořte aplikaci Azure funkce a účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="52dc4-104">Create an Azure function app and Azure storage account</span></span>
<span data-ttu-id="52dc4-105">[Azure Functions](../../articles/azure-functions/functions-overview.md) je řešení umožňující snadno spouštět *funkce* (malé části kódu) v cloudu.</span><span class="sxs-lookup"><span data-stu-id="52dc4-105">[Azure Functions](../../articles/azure-functions/functions-overview.md) is a solution for easily running *functions* (small pieces of code) in the cloud.</span></span> <span data-ttu-id="52dc4-106">Aplikace Azure funkce hostuje provádění funkcí v Azure.</span><span class="sxs-lookup"><span data-stu-id="52dc4-106">An Azure function app hosts the execution of your functions in Azure.</span></span>

## <a name="what-will-you-do"></a><span data-ttu-id="52dc4-107">Co se dělat</span><span class="sxs-lookup"><span data-stu-id="52dc4-107">What will you do</span></span>
<span data-ttu-id="52dc4-108">Vytvoření aplikace Azure funkce a účet úložiště Azure pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="52dc4-108">Use an Azure Resource Manager template to create an Azure function app and an Azure storage account.</span></span> <span data-ttu-id="52dc4-109">Aplikace Azure funkce naslouchá událostem, Azure IoT hub, zpracuje příchozí zprávy a zapisuje je do Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="52dc4-109">The Azure function app listens to Azure IoT hub events, processes incoming messages, and writes them to Azure Table storage.</span></span> <span data-ttu-id="52dc4-110">Účet úložiště se používá k přečtení trvalou kopie zprávy z tabulky Azure.</span><span class="sxs-lookup"><span data-stu-id="52dc4-110">The storage account is used for reading the persisted copies of messages from Azure table.</span></span> <span data-ttu-id="52dc4-111">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="52dc4-111">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-will-you-learn"></a><span data-ttu-id="52dc4-112">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="52dc4-112">What will you learn</span></span>
<span data-ttu-id="52dc4-113">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="52dc4-113">In this article, you will learn:</span></span>
* <span data-ttu-id="52dc4-114">Jak používat [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) nasazení prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="52dc4-114">How to use [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) to deploy Azure resources.</span></span>
* <span data-ttu-id="52dc4-115">Jak používat aplikaci Azure funkce zpracování zpráv centra IoT a jejich zápis do tabulky v Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="52dc4-115">How to use an Azure function app to process IoT hub messages and write them to a table in Azure Table storage.</span></span>

## <a name="what-do-you-need"></a><span data-ttu-id="52dc4-116">Co je potřeba</span><span class="sxs-lookup"><span data-stu-id="52dc4-116">What do you need</span></span>
<span data-ttu-id="52dc4-117">Musíte úspěšně jste dokončili:</span><span class="sxs-lookup"><span data-stu-id="52dc4-117">You must have successfully completed:</span></span>
- <span data-ttu-id="52dc4-118">[Začínáme s vaší Edison Intel][get-started-with-your-intel-edison]</span><span class="sxs-lookup"><span data-stu-id="52dc4-118">[Get started with your Intel Edison][get-started-with-your-intel-edison]</span></span>
- <span data-ttu-id="52dc4-119">[Vytvoření služby Azure IoT hub][create-your-azure-iot-hub]</span><span class="sxs-lookup"><span data-stu-id="52dc4-119">[Create your Azure IoT hub][create-your-azure-iot-hub]</span></span>

## <a name="open-the-sample-app"></a><span data-ttu-id="52dc4-120">Otevřete ukázková aplikace</span><span class="sxs-lookup"><span data-stu-id="52dc4-120">Open the sample app</span></span>
<span data-ttu-id="52dc4-121">Otevřete ukázkový projekt ve Visual Studio Code spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="52dc4-121">Open the sample project in Visual Studio Code by running the following commands:</span></span>

```bash
cd Lesson3
code .
```

![Struktura úložiště][repo-structure]

* <span data-ttu-id="52dc4-123">Soubor v `app` podsložky je klíče zdrojového souboru.</span><span class="sxs-lookup"><span data-stu-id="52dc4-123">The file in the `app` subfolder is the key source file.</span></span> <span data-ttu-id="52dc4-124">Tento zdrojový soubor obsahuje kód, který odešle zprávu 20krát do služby IoT hub a blink DIODU pro každou zprávu, kterou odešle.</span><span class="sxs-lookup"><span data-stu-id="52dc4-124">This source file contains the code to send a message 20 times to your IoT hub and blink the LED for each message it sends.</span></span>
* <span data-ttu-id="52dc4-125">`arm-template.json` Soubor je šablony Azure Resource Manager, která obsahuje aplikaci Azure funkce a účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="52dc4-125">The `arm-template.json` file is the Azure Resource Manager template that contains an Azure function app and an Azure storage account.</span></span>
* <span data-ttu-id="52dc4-126">`arm-template-param.json` Soubor je soubor konfigurace používané šablony Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="52dc4-126">The `arm-template-param.json` file is the configuration file used by the Azure Resource Manager template.</span></span>
* <span data-ttu-id="52dc4-127">`ReceiveDeviceMessages` Podsložky obsahuje kód Node.js pro funkci Azure.</span><span class="sxs-lookup"><span data-stu-id="52dc4-127">The `ReceiveDeviceMessages` subfolder contains the Node.js code for the Azure function.</span></span>

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a><span data-ttu-id="52dc4-128">Konfigurace šablon Azure Resource Manager a vytváření prostředků v Azure</span><span class="sxs-lookup"><span data-stu-id="52dc4-128">Configure Azure Resource Manager templates and create resources in Azure</span></span>
<span data-ttu-id="52dc4-129">Aktualizace `arm-template-param.json` soubor ve Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="52dc4-129">Update the `arm-template-param.json` file in Visual Studio Code.</span></span>

![Parametry šablony Azure Resource Manager][arm-template-parameters]

* <span data-ttu-id="52dc4-131">Nahraďte **[název služby IoT Hub]** s **{Moje název centra}** kterou jste zadali při jste [vytvoření služby IoT hub a registraci Intel Edison][created-your-iot-hub-and-registered-intel-edison].</span><span class="sxs-lookup"><span data-stu-id="52dc4-131">Replace **[your IoT Hub name]** with **{my hub name}** that you specified when you [created your IoT hub and registered Intel Edison][created-your-iot-hub-and-registered-intel-edison].</span></span>
* <span data-ttu-id="52dc4-132">Nahraďte **[předpony řetězec pro nové prostředky]** s jakoukoli předponu chcete.</span><span class="sxs-lookup"><span data-stu-id="52dc4-132">Replace **[prefix string for new resources]** with any prefix you want.</span></span> <span data-ttu-id="52dc4-133">Předpona, která zajišťuje, že název prostředku globálně jedinečný, aby nedošlo ke konfliktu.</span><span class="sxs-lookup"><span data-stu-id="52dc4-133">The prefix ensures that the resource name is globally unique to avoid conflict.</span></span> <span data-ttu-id="52dc4-134">Nepoužívejte pomlčku nebo číslo počáteční v předponu.</span><span class="sxs-lookup"><span data-stu-id="52dc4-134">Do not use a dash or number initial in the prefix.</span></span>

<span data-ttu-id="52dc4-135">Po provedení aktualizace `arm-template-param.json` souboru, nasadit tyto prostředky do Azure tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="52dc4-135">After you update the `arm-template-param.json` file, deploy the resources to Azure by running the following command:</span></span>

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

<span data-ttu-id="52dc4-136">Vytvořte tyto prostředky trvá asi 5 minut.</span><span class="sxs-lookup"><span data-stu-id="52dc4-136">It takes about five minutes to create these resources.</span></span> <span data-ttu-id="52dc4-137">Při vytvoření prostředku probíhá, můžete přesunout další článek.</span><span class="sxs-lookup"><span data-stu-id="52dc4-137">While the resource creation is in progress, you can move on to the next article.</span></span>

## <a name="summary"></a><span data-ttu-id="52dc4-138">Souhrn</span><span class="sxs-lookup"><span data-stu-id="52dc4-138">Summary</span></span>
<span data-ttu-id="52dc4-139">Vytvoření aplikace Azure funkce pro zpracování zprávy IoT hub a účet úložiště Azure slouží k uložení těchto zpráv.</span><span class="sxs-lookup"><span data-stu-id="52dc4-139">You've created your Azure function app to process IoT hub messages and an Azure storage account to store these messages.</span></span> <span data-ttu-id="52dc4-140">Teď můžete nasadit a spustit ukázkový k odesílání zpráv typu zařízení cloud na Edison.</span><span class="sxs-lookup"><span data-stu-id="52dc4-140">You can now deploy and run the sample to send device-to-cloud messages on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="52dc4-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="52dc4-141">Next steps</span></span>
<span data-ttu-id="52dc4-142">[Spuštění ukázkové aplikace k odesílání zpráv typu zařízení cloud na Intel Edison][send-device-to-cloud-messages].</span><span class="sxs-lookup"><span data-stu-id="52dc4-142">[Run a sample application to send device-to-cloud messages on Intel Edison][send-device-to-cloud-messages].</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[get-started-with-your-intel-edison]: iot-hub-intel-edison-kit-node-get-started.md
[create-your-azure-iot-hub]: iot-hub-intel-edison-kit-node-get-started.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson3/repo_structure.png
[arm-template-parameters]: media/iot-hub-intel-edison-lessons/lesson3/arm_para.png
[created-your-iot-hub-and-registered-intel-edison]: iot-hub-intel-edison-kit-node-lesson2-prepare-azure-iot-hub.md
[send-device-to-cloud-messages]: iot-hub-intel-edison-kit-node-lesson3-run-azure-blink.md