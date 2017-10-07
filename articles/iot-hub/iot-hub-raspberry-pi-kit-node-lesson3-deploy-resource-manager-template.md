---
title: "Připojit malin platformy (uzel) tooAzure IoT - Lekce 3: nasazení šablony | Microsoft Docs"
description: "aplikaci Azure Funkce Hello naslouchá tooAzure IoT Centrum událostí, zpracuje příchozí zprávy a zapíše tooAzure tabulka úložiště."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "ukládání dat v cloudu hello, data uložená v cloudu, iot cloudové služby"
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
ms.openlocfilehash: b6c0a9530cb80e3f78c0e96037f6f3942b602aea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="9c563-104">Vytvořte aplikaci Azure funkce a účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="9c563-104">Create an Azure function app and Azure storage account</span></span>
<span data-ttu-id="9c563-105">[Azure Functions](../azure-functions/functions-overview.md) je řešení umožňující snadno spouštět *funkce* (malé části kódu) v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="9c563-105">[Azure Functions](../azure-functions/functions-overview.md) is a solution for easily running *functions* (small pieces of code) in hello cloud.</span></span> <span data-ttu-id="9c563-106">Aplikace Azure funkce hostuje hello provádění funkcí v Azure.</span><span class="sxs-lookup"><span data-stu-id="9c563-106">An Azure function app hosts hello execution of your functions in Azure.</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="9c563-107">Co provedete</span><span class="sxs-lookup"><span data-stu-id="9c563-107">What you will do</span></span>
<span data-ttu-id="9c563-108">Aplikace Azure funkce a účet úložiště Azure, použijte toocreate šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="9c563-108">Use an Azure Resource Manager template toocreate an Azure function app and an Azure storage account.</span></span> <span data-ttu-id="9c563-109">aplikaci Azure Funkce Hello naslouchá tooAzure IoT Centrum událostí, zpracuje příchozí zprávy a zapíše tooAzure tabulka úložiště.</span><span class="sxs-lookup"><span data-stu-id="9c563-109">hello Azure function app listens tooAzure IoT hub events, processes incoming messages, and writes them tooAzure Table storage.</span></span> <span data-ttu-id="9c563-110">Pokud máte potíže, hledají řešení na hello [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="9c563-110">If you have any problems, seek solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="9c563-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="9c563-111">What you will learn</span></span>
<span data-ttu-id="9c563-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="9c563-112">In this article, you will learn:</span></span>

* <span data-ttu-id="9c563-113">Jak toouse [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) toodeploy Azure prostředky.</span><span class="sxs-lookup"><span data-stu-id="9c563-113">How toouse [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) toodeploy Azure resources.</span></span>
* <span data-ttu-id="9c563-114">Jak toouse Azure funkce aplikace tooprocess zprávy centra IoT a jejich zápis tooa tabulky ve službě Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="9c563-114">How toouse an Azure function app tooprocess IoT hub messages and write them tooa table in Azure Table storage.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="9c563-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="9c563-115">What you need</span></span>
<span data-ttu-id="9c563-116">Musíte úspěšně jste dokončili:</span><span class="sxs-lookup"><span data-stu-id="9c563-116">You must have successfully completed:</span></span>
* [<span data-ttu-id="9c563-117">Začínáme s malin pí 3</span><span class="sxs-lookup"><span data-stu-id="9c563-117">Get started with Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-node-get-started.md)
* [<span data-ttu-id="9c563-118">Vytvoření služby Azure IoT hub</span><span class="sxs-lookup"><span data-stu-id="9c563-118">Create your Azure IoT hub</span></span>](iot-hub-raspberry-pi-kit-node-get-started.md)

## <a name="open-hello-sample-app"></a><span data-ttu-id="9c563-119">Ukázkové aplikace otevřete hello</span><span class="sxs-lookup"><span data-stu-id="9c563-119">Open hello sample app</span></span>
<span data-ttu-id="9c563-120">Otevřete hello ukázkový projekt ve Visual Studio Code spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="9c563-120">Open hello sample project in Visual Studio Code by running hello following commands:</span></span>

```bash
cd Lesson3
code .
```

![Struktura úložiště](media/iot-hub-raspberry-pi-lessons/lesson3/repo_structure.png)

* <span data-ttu-id="9c563-122">Hello `app.js` souboru v hello `app` podsložky je hello klíče zdrojového souboru.</span><span class="sxs-lookup"><span data-stu-id="9c563-122">hello `app.js` file in hello `app` subfolder is hello key source file.</span></span> <span data-ttu-id="9c563-123">Tento zdrojový soubor obsahuje kód toosend hello zprávu 20krát tooyour IoT hub a blikání hello DIODU pro každou zprávu, že odešle.</span><span class="sxs-lookup"><span data-stu-id="9c563-123">This source file contains hello code toosend a message 20 times tooyour IoT hub and blink hello LED for each message it sends.</span></span>
* <span data-ttu-id="9c563-124">Hello `arm-template.json` soubor je hello šablony Azure Resource Manageru, která obsahuje aplikaci Azure funkce a účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="9c563-124">hello `arm-template.json` file is hello Azure Resource Manager template that contains an Azure function app and an Azure storage account.</span></span>
* <span data-ttu-id="9c563-125">Hello `arm-template-param.json` soubor je soubor konfigurace hello používá šablony Azure Resource Manageru hello.</span><span class="sxs-lookup"><span data-stu-id="9c563-125">hello `arm-template-param.json` file is hello configuration file used by hello Azure Resource Manager template.</span></span>
* <span data-ttu-id="9c563-126">Hello `ReceiveDeviceMessages` podsložky obsahuje kód Node.js hello pro hello Azure funkce.</span><span class="sxs-lookup"><span data-stu-id="9c563-126">hello `ReceiveDeviceMessages` subfolder contains hello Node.js code for hello Azure function.</span></span>

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a><span data-ttu-id="9c563-127">Konfigurace šablon Azure Resource Manager a vytváření prostředků v Azure</span><span class="sxs-lookup"><span data-stu-id="9c563-127">Configure Azure Resource Manager templates and create resources in Azure</span></span>
<span data-ttu-id="9c563-128">Aktualizace hello `arm-template-param.json` soubor ve Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="9c563-128">Update hello `arm-template-param.json` file in Visual Studio Code.</span></span>

![Parametry šablony Azure Resource Manager](media/iot-hub-raspberry-pi-lessons/lesson3/arm_para.png)

* <span data-ttu-id="9c563-130">Nahraďte **[název služby IoT Hub]** s **{Moje název centra}** kterou jste zadali při jste [vytvoření služby IoT hub a registraci malin pí 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="9c563-130">Replace **[your IoT Hub name]** with **{my hub name}** that you specified when you [created your IoT hub and registered Raspberry Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).</span></span>
* <span data-ttu-id="9c563-131">Nahraďte **[předpony řetězec pro nové prostředky]** s jakoukoli předponu chcete.</span><span class="sxs-lookup"><span data-stu-id="9c563-131">Replace **[prefix string for new resources]** with any prefix you want.</span></span> <span data-ttu-id="9c563-132">Předpona Hello zajistí, že tento název zdroje hello je globálně jedinečný tooavoid konflikt.</span><span class="sxs-lookup"><span data-stu-id="9c563-132">hello prefix ensures that hello resource name is globally unique tooavoid conflict.</span></span> <span data-ttu-id="9c563-133">Nepoužívejte dash nebo číslo počáteční v hello předponu.</span><span class="sxs-lookup"><span data-stu-id="9c563-133">Do not use a dash or number initial in hello prefix.</span></span>

<span data-ttu-id="9c563-134">Po dokončení aktualizace hello `arm-template-param.json` souboru, nasazení hello prostředky tooAzure spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9c563-134">After you update hello `arm-template-param.json` file, deploy hello resources tooAzure by running hello following command:</span></span>

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

<span data-ttu-id="9c563-135">Trvá asi 5 minut toocreate tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="9c563-135">It takes about five minutes toocreate these resources.</span></span> <span data-ttu-id="9c563-136">Při vytvoření prostředku hello probíhá, můžete přesunout na následující článek toohello.</span><span class="sxs-lookup"><span data-stu-id="9c563-136">While hello resource creation is in progress, you can move on toohello next article.</span></span>

## <a name="summary"></a><span data-ttu-id="9c563-137">Souhrn</span><span class="sxs-lookup"><span data-stu-id="9c563-137">Summary</span></span>
<span data-ttu-id="9c563-138">Jste vytvořili vaší aplikaci tooprocess funkce Azure IoT hub zprávy a Azure storage účet toostore tyto zprávy.</span><span class="sxs-lookup"><span data-stu-id="9c563-138">You've created your Azure function app tooprocess IoT hub messages and an Azure storage account toostore these messages.</span></span> <span data-ttu-id="9c563-139">Teď můžete nasadit a spustit zpráv typu zařízení cloud hello ukázkové toosend na pí.</span><span class="sxs-lookup"><span data-stu-id="9c563-139">You can now deploy and run hello sample toosend device-to-cloud messages on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9c563-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9c563-140">Next steps</span></span>
[<span data-ttu-id="9c563-141">Spuštění ukázkové aplikace toosend zpráv typu zařízení cloud na malin pí 3</span><span class="sxs-lookup"><span data-stu-id="9c563-141">Run a sample application toosend device-to-cloud messages on Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

