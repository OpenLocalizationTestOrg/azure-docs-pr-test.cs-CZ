---
title: "Connect Intel Edison (C) tooAzure IoT - Lekce 3: vytvoření aplikace pro funkce | Microsoft Docs"
description: "aplikaci Azure Funkce Hello naslouchá tooAzure IoT Centrum událostí, zpracuje příchozí zprávy a zapíše tooAzure tabulka úložiště."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "ukládání dat v cloudu hello, data uložená v cloudu, iot cloudové služby"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 739b82e9-5d4e-4485-8971-f57cbb682faf
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: ef045ec2f44fe379a5e6c777d1bfb97de8b965a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="d9ea9-104">Vytvořte aplikaci Azure funkce a účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="d9ea9-104">Create an Azure function app and Azure storage account</span></span>
<span data-ttu-id="d9ea9-105">[Azure Functions](../../articles/azure-functions/functions-overview.md) je řešení umožňující snadno spouštět *funkce* (malé části kódu) v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="d9ea9-105">[Azure Functions](../../articles/azure-functions/functions-overview.md) is a solution for easily running *functions* (small pieces of code) in hello cloud.</span></span> <span data-ttu-id="d9ea9-106">Aplikace Azure funkce hostuje hello provádění funkcí v Azure.</span><span class="sxs-lookup"><span data-stu-id="d9ea9-106">An Azure function app hosts hello execution of your functions in Azure.</span></span>

## <a name="what-will-you-do"></a><span data-ttu-id="d9ea9-107">Co se dělat</span><span class="sxs-lookup"><span data-stu-id="d9ea9-107">What will you do</span></span>
<span data-ttu-id="d9ea9-108">Aplikace Azure funkce a účet úložiště Azure, použijte toocreate šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d9ea9-108">Use an Azure Resource Manager template toocreate an Azure function app and an Azure storage account.</span></span> <span data-ttu-id="d9ea9-109">aplikaci Azure Funkce Hello naslouchá tooAzure IoT Centrum událostí, zpracuje příchozí zprávy a zapíše tooAzure tabulka úložiště.</span><span class="sxs-lookup"><span data-stu-id="d9ea9-109">hello Azure function app listens tooAzure IoT hub events, processes incoming messages, and writes them tooAzure Table storage.</span></span> <span data-ttu-id="d9ea9-110">Hello účet úložiště se používá ke čtení hello trvalé kopie zpráv z tabulky Azure.</span><span class="sxs-lookup"><span data-stu-id="d9ea9-110">hello storage account is used for reading hello persisted copies of messages from Azure table.</span></span> <span data-ttu-id="d9ea9-111">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="d9ea9-111">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-will-you-learn"></a><span data-ttu-id="d9ea9-112">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="d9ea9-112">What will you learn</span></span>
<span data-ttu-id="d9ea9-113">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="d9ea9-113">In this article, you will learn:</span></span>
* <span data-ttu-id="d9ea9-114">Jak toouse [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) toodeploy Azure prostředky.</span><span class="sxs-lookup"><span data-stu-id="d9ea9-114">How toouse [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) toodeploy Azure resources.</span></span>
* <span data-ttu-id="d9ea9-115">Jak toouse Azure funkce aplikace tooprocess zprávy centra IoT a jejich zápis tooa tabulky ve službě Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="d9ea9-115">How toouse an Azure function app tooprocess IoT hub messages and write them tooa table in Azure Table storage.</span></span>

## <a name="what-do-you-need"></a><span data-ttu-id="d9ea9-116">Co je potřeba</span><span class="sxs-lookup"><span data-stu-id="d9ea9-116">What do you need</span></span>
<span data-ttu-id="d9ea9-117">Musíte úspěšně jste dokončili:</span><span class="sxs-lookup"><span data-stu-id="d9ea9-117">You must have successfully completed:</span></span>
- <span data-ttu-id="d9ea9-118">[Začínáme s vaší Edison Intel][get-started-with-your-intel-edison]</span><span class="sxs-lookup"><span data-stu-id="d9ea9-118">[Get started with your Intel Edison][get-started-with-your-intel-edison]</span></span>
- <span data-ttu-id="d9ea9-119">[Vytvoření služby Azure IoT hub][create-your-azure-iot-hub]</span><span class="sxs-lookup"><span data-stu-id="d9ea9-119">[Create your Azure IoT hub][create-your-azure-iot-hub]</span></span>

## <a name="open-hello-sample-app"></a><span data-ttu-id="d9ea9-120">Ukázkové aplikace otevřete hello</span><span class="sxs-lookup"><span data-stu-id="d9ea9-120">Open hello sample app</span></span>
<span data-ttu-id="d9ea9-121">Otevřete hello ukázkový projekt ve Visual Studio Code spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="d9ea9-121">Open hello sample project in Visual Studio Code by running hello following commands:</span></span>

```bash
cd Lesson3
code .
```

![Struktura úložiště][repo-structure]

* <span data-ttu-id="d9ea9-123">soubor Hello v hello `app` podsložky je hello klíče zdrojového souboru.</span><span class="sxs-lookup"><span data-stu-id="d9ea9-123">hello file in hello `app` subfolder is hello key source file.</span></span> <span data-ttu-id="d9ea9-124">Tento zdrojový soubor obsahuje kód toosend hello zprávu 20krát tooyour IoT hub a blikání hello DIODU pro každou zprávu, že odešle.</span><span class="sxs-lookup"><span data-stu-id="d9ea9-124">This source file contains hello code toosend a message 20 times tooyour IoT hub and blink hello LED for each message it sends.</span></span>
* <span data-ttu-id="d9ea9-125">Hello `arm-template.json` soubor je hello šablony Azure Resource Manageru, která obsahuje aplikaci Azure funkce a účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="d9ea9-125">hello `arm-template.json` file is hello Azure Resource Manager template that contains an Azure function app and an Azure storage account.</span></span>
* <span data-ttu-id="d9ea9-126">Hello `arm-template-param.json` soubor je soubor konfigurace hello používá šablony Azure Resource Manageru hello.</span><span class="sxs-lookup"><span data-stu-id="d9ea9-126">hello `arm-template-param.json` file is hello configuration file used by hello Azure Resource Manager template.</span></span>
* <span data-ttu-id="d9ea9-127">Hello `ReceiveDeviceMessages` podsložky obsahuje kód Node.js hello pro hello Azure funkce.</span><span class="sxs-lookup"><span data-stu-id="d9ea9-127">hello `ReceiveDeviceMessages` subfolder contains hello Node.js code for hello Azure function.</span></span>

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a><span data-ttu-id="d9ea9-128">Konfigurace šablon Azure Resource Manager a vytváření prostředků v Azure</span><span class="sxs-lookup"><span data-stu-id="d9ea9-128">Configure Azure Resource Manager templates and create resources in Azure</span></span>
<span data-ttu-id="d9ea9-129">Aktualizace hello `arm-template-param.json` soubor ve Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d9ea9-129">Update hello `arm-template-param.json` file in Visual Studio Code.</span></span>

![Parametry šablony Azure Resource Manager][arm-template-parameters]

* <span data-ttu-id="d9ea9-131">Nahraďte **[název služby IoT Hub]** s **{Moje název centra}** kterou jste zadali při jste [vytvoření služby IoT hub a registraci Intel Edison][created-your-iot-hub-and-registered-intel-edison].</span><span class="sxs-lookup"><span data-stu-id="d9ea9-131">Replace **[your IoT Hub name]** with **{my hub name}** that you specified when you [created your IoT hub and registered Intel Edison][created-your-iot-hub-and-registered-intel-edison].</span></span>
* <span data-ttu-id="d9ea9-132">Nahraďte **[předpony řetězec pro nové prostředky]** s jakoukoli předponu chcete.</span><span class="sxs-lookup"><span data-stu-id="d9ea9-132">Replace **[prefix string for new resources]** with any prefix you want.</span></span> <span data-ttu-id="d9ea9-133">Předpona Hello zajistí, že tento název zdroje hello je globálně jedinečný tooavoid konflikt.</span><span class="sxs-lookup"><span data-stu-id="d9ea9-133">hello prefix ensures that hello resource name is globally unique tooavoid conflict.</span></span> <span data-ttu-id="d9ea9-134">Nepoužívejte dash nebo číslo počáteční v hello předponu.</span><span class="sxs-lookup"><span data-stu-id="d9ea9-134">Do not use a dash or number initial in hello prefix.</span></span>

<span data-ttu-id="d9ea9-135">Po dokončení aktualizace hello `arm-template-param.json` souboru, nasazení hello prostředky tooAzure spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d9ea9-135">After you update hello `arm-template-param.json` file, deploy hello resources tooAzure by running hello following command:</span></span>

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

<span data-ttu-id="d9ea9-136">Trvá asi 5 minut toocreate tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="d9ea9-136">It takes about five minutes toocreate these resources.</span></span> <span data-ttu-id="d9ea9-137">Při vytvoření prostředku hello probíhá, můžete přesunout na následující článek toohello.</span><span class="sxs-lookup"><span data-stu-id="d9ea9-137">While hello resource creation is in progress, you can move on toohello next article.</span></span>

## <a name="summary"></a><span data-ttu-id="d9ea9-138">Souhrn</span><span class="sxs-lookup"><span data-stu-id="d9ea9-138">Summary</span></span>
<span data-ttu-id="d9ea9-139">Jste vytvořili vaší aplikaci tooprocess funkce Azure IoT hub zprávy a Azure storage účet toostore tyto zprávy.</span><span class="sxs-lookup"><span data-stu-id="d9ea9-139">You've created your Azure function app tooprocess IoT hub messages and an Azure storage account toostore these messages.</span></span> <span data-ttu-id="d9ea9-140">Teď můžete nasadit a spustit zpráv typu zařízení cloud hello ukázkové toosend na Edison.</span><span class="sxs-lookup"><span data-stu-id="d9ea9-140">You can now deploy and run hello sample toosend device-to-cloud messages on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d9ea9-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d9ea9-141">Next steps</span></span>
<span data-ttu-id="d9ea9-142">[Spuštění ukázkové aplikace toosend zpráv typu zařízení cloud na Intel Edison][send-device-to-cloud-messages].</span><span class="sxs-lookup"><span data-stu-id="d9ea9-142">[Run a sample application toosend device-to-cloud messages on Intel Edison][send-device-to-cloud-messages].</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[get-started-with-your-intel-edison]: iot-hub-intel-edison-kit-c-get-started.md
[create-your-azure-iot-hub]: iot-hub-intel-edison-kit-c-get-started.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson3/repo_structure_c.png
[arm-template-parameters]: /media/iot-hub-intel-edison-lessons/lesson3/arm_para_c.png
[created-your-iot-hub-and-registered-intel-edison]: iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md
[send-device-to-cloud-messages]: iot-hub-intel-edison-kit-c-lesson3-run-azure-blink.md