---
title: "Connect Raspberry PI (C) tooAzure IoT - Lekce 3: nasazení šablony | Microsoft Docs"
description: "aplikaci Azure Funkce Hello naslouchá tooAzure IoT Centrum událostí, zpracuje příchozí zprávy a zapíše tooAzure tabulka úložiště."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "ukládání dat v cloudu hello, data uložená v cloudu, iot cloudové služby"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 4bcfb071-b3ae-48cc-8ea5-7e7434732287
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 11aaad4935681d8b3d338779eec1b19d77cb11e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="51668-104">Vytvořte aplikaci Azure funkce a účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="51668-104">Create an Azure function app and Azure storage account</span></span>
<span data-ttu-id="51668-105">[Azure Functions](../../articles/azure-functions/functions-overview.md) je řešení umožňující snadno spouštět *funkce* (malé části kódu) v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="51668-105">[Azure Functions](../../articles/azure-functions/functions-overview.md) is a solution for easily running *functions* (small pieces of code) in hello cloud.</span></span> <span data-ttu-id="51668-106">Aplikace Azure funkce hostuje hello provádění funkcí v Azure.</span><span class="sxs-lookup"><span data-stu-id="51668-106">An Azure function app hosts hello execution of your functions in Azure.</span></span>

## <a name="what-will-you-do"></a><span data-ttu-id="51668-107">Co se dělat</span><span class="sxs-lookup"><span data-stu-id="51668-107">What will you do</span></span>
<span data-ttu-id="51668-108">Aplikace Azure funkce a účet úložiště Azure, použijte toocreate šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="51668-108">Use an Azure Resource Manager template toocreate an Azure function app and an Azure storage account.</span></span> <span data-ttu-id="51668-109">aplikaci Azure Funkce Hello naslouchá tooAzure IoT Centrum událostí, zpracuje příchozí zprávy a zapíše tooAzure tabulka úložiště.</span><span class="sxs-lookup"><span data-stu-id="51668-109">hello Azure function app listens tooAzure IoT hub events, processes incoming messages, and writes them tooAzure Table storage.</span></span> <span data-ttu-id="51668-110">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="51668-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-will-you-learn"></a><span data-ttu-id="51668-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="51668-111">What will you learn</span></span>
<span data-ttu-id="51668-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="51668-112">In this article, you will learn:</span></span>
* <span data-ttu-id="51668-113">Jak toouse [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) toodeploy Azure prostředky.</span><span class="sxs-lookup"><span data-stu-id="51668-113">How toouse [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) toodeploy Azure resources.</span></span>
* <span data-ttu-id="51668-114">Jak toouse Azure funkce aplikace tooprocess zprávy centra IoT a jejich zápis tooa tabulky ve službě Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="51668-114">How toouse an Azure function app tooprocess IoT hub messages and write them tooa table in Azure Table storage.</span></span>

## <a name="what-do-you-need"></a><span data-ttu-id="51668-115">Co je potřeba</span><span class="sxs-lookup"><span data-stu-id="51668-115">What do you need</span></span>
* <span data-ttu-id="51668-116">Musíte úspěšně jste dokončili:</span><span class="sxs-lookup"><span data-stu-id="51668-116">You must have successfully completed:</span></span>
- [<span data-ttu-id="51668-117">Začínáme s vaší malin pí 3</span><span class="sxs-lookup"><span data-stu-id="51668-117">Get started with your Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-c-get-started.md)
- [<span data-ttu-id="51668-118">Vytvoření služby Azure IoT hub</span><span class="sxs-lookup"><span data-stu-id="51668-118">Create your Azure IoT hub</span></span>](iot-hub-raspberry-pi-kit-c-get-started.md)

## <a name="open-hello-sample-app"></a><span data-ttu-id="51668-119">Ukázkové aplikace otevřete hello</span><span class="sxs-lookup"><span data-stu-id="51668-119">Open hello sample app</span></span>
<span data-ttu-id="51668-120">Otevřete hello ukázkový projekt ve Visual Studio Code spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="51668-120">Open hello sample project in Visual Studio Code by running hello following commands:</span></span>

```bash
cd Lesson3
code .
```

![Struktura úložiště](media/iot-hub-raspberry-pi-lessons/lesson3/repo_structure_c.png)

* <span data-ttu-id="51668-122">Hello `main.c` souboru v hello `app` podsložky je hello klíče zdrojového souboru.</span><span class="sxs-lookup"><span data-stu-id="51668-122">hello `main.c` file in hello `app` subfolder is hello key source file.</span></span> <span data-ttu-id="51668-123">Tento zdrojový soubor obsahuje kód toosend hello zprávu 20krát tooyour IoT hub a blikání hello DIODU pro každou zprávu, že odešle.</span><span class="sxs-lookup"><span data-stu-id="51668-123">This source file contains hello code toosend a message 20 times tooyour IoT hub and blink hello LED for each message it sends.</span></span>
* <span data-ttu-id="51668-124">Hello `arm-template.json` soubor je hello šablony Azure Resource Manageru, která obsahuje aplikaci Azure funkce a účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="51668-124">hello `arm-template.json` file is hello Azure Resource Manager template that contains an Azure function app and an Azure storage account.</span></span>
* <span data-ttu-id="51668-125">Hello `arm-template-param.json` soubor je soubor konfigurace hello používá šablony Azure Resource Manageru hello.</span><span class="sxs-lookup"><span data-stu-id="51668-125">hello `arm-template-param.json` file is hello configuration file used by hello Azure Resource Manager template.</span></span>
* <span data-ttu-id="51668-126">Hello `ReceiveDeviceMessages` podsložky obsahuje kód Node.js hello pro hello Azure funkce.</span><span class="sxs-lookup"><span data-stu-id="51668-126">hello `ReceiveDeviceMessages` subfolder contains hello Node.js code for hello Azure function.</span></span>

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a><span data-ttu-id="51668-127">Konfigurace šablon Azure Resource Manager a vytváření prostředků v Azure</span><span class="sxs-lookup"><span data-stu-id="51668-127">Configure Azure Resource Manager templates and create resources in Azure</span></span>
<span data-ttu-id="51668-128">Aktualizace hello `arm-template-param.json` soubor ve Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="51668-128">Update hello `arm-template-param.json` file in Visual Studio Code.</span></span>

![Parametry šablony Azure Resource Manager](media/iot-hub-raspberry-pi-lessons/lesson3/arm_para_c.png)

* <span data-ttu-id="51668-130">Nahraďte **[název služby IoT Hub]** s **{Moje název centra}** kterou jste zadali při jste [vytvoření služby IoT hub a registraci malin pí 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="51668-130">Replace **[your IoT Hub name]** with **{my hub name}** that you specified when you [created your IoT hub and registered Raspberry Pi 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md).</span></span>
* <span data-ttu-id="51668-131">Nahraďte **[předpony řetězec pro nové prostředky]** s jakoukoli předponu chcete.</span><span class="sxs-lookup"><span data-stu-id="51668-131">Replace **[prefix string for new resources]** with any prefix you want.</span></span> <span data-ttu-id="51668-132">Předpona Hello zajistí, že tento název zdroje hello je globálně jedinečný tooavoid konflikt.</span><span class="sxs-lookup"><span data-stu-id="51668-132">hello prefix ensures that hello resource name is globally unique tooavoid conflict.</span></span> <span data-ttu-id="51668-133">Nepoužívejte dash nebo číslo počáteční v hello předponu.</span><span class="sxs-lookup"><span data-stu-id="51668-133">Do not use a dash or number initial in hello prefix.</span></span>

<span data-ttu-id="51668-134">Po dokončení aktualizace hello `arm-template-param.json` souboru, nasazení hello prostředky tooAzure spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="51668-134">After you update hello `arm-template-param.json` file, deploy hello resources tooAzure by running hello following command:</span></span>

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

<span data-ttu-id="51668-135">Trvá asi 5 minut toocreate tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="51668-135">It takes about five minutes toocreate these resources.</span></span> <span data-ttu-id="51668-136">Při vytvoření prostředku hello probíhá, můžete přesunout na následující článek toohello.</span><span class="sxs-lookup"><span data-stu-id="51668-136">While hello resource creation is in progress, you can move on toohello next article.</span></span>

## <a name="summary"></a><span data-ttu-id="51668-137">Souhrn</span><span class="sxs-lookup"><span data-stu-id="51668-137">Summary</span></span>
<span data-ttu-id="51668-138">Jste vytvořili vaší aplikaci tooprocess funkce Azure IoT hub zprávy a Azure storage účet toostore tyto zprávy.</span><span class="sxs-lookup"><span data-stu-id="51668-138">You've created your Azure function app tooprocess IoT hub messages and an Azure storage account toostore these messages.</span></span> <span data-ttu-id="51668-139">Teď můžete nasadit a spustit zpráv typu zařízení cloud hello ukázkové toosend na pí.</span><span class="sxs-lookup"><span data-stu-id="51668-139">You can now deploy and run hello sample toosend device-to-cloud messages on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="51668-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="51668-140">Next steps</span></span>
[<span data-ttu-id="51668-141">Spuštění ukázkové aplikace toosend zpráv typu zařízení cloud</span><span class="sxs-lookup"><span data-stu-id="51668-141">Run a sample application toosend device-to-cloud messages</span></span>](iot-hub-raspberry-pi-kit-c-lesson3-run-azure-blink.md)

