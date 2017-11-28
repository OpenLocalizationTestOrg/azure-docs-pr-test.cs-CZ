---
title: "Connect Arduino (C) tooAzure IoT - Lekce 3: nasazení šablony | Microsoft Docs"
description: "aplikaci Azure Funkce Hello naslouchá tooAzure IoT Centrum událostí, zpracuje příchozí zprávy a zapíše tooAzure tabulka úložiště."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "ukládání dat v cloudu hello, data uložená v cloudu, iot cloudové služby"
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
ms.openlocfilehash: 6a84a6d3c5263a85c8997cf69fe446d73ab7a5fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="5bb5e-104">Vytvořte aplikaci Azure funkce a účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="5bb5e-104">Create an Azure function app and Azure storage account</span></span>
<span data-ttu-id="5bb5e-105">[Azure Functions](../../articles/azure-functions/functions-overview.md) je řešení umožňující snadno spouštět *funkce* (malé části kódu) v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="5bb5e-105">[Azure Functions](../../articles/azure-functions/functions-overview.md) is a solution for easily running *functions* (small pieces of code) in hello cloud.</span></span> <span data-ttu-id="5bb5e-106">Aplikace Azure funkce hostuje hello provádění funkcí v Azure.</span><span class="sxs-lookup"><span data-stu-id="5bb5e-106">An Azure function app hosts hello execution of your functions in Azure.</span></span>

## <a name="what-will-you-do"></a><span data-ttu-id="5bb5e-107">Co se dělat</span><span class="sxs-lookup"><span data-stu-id="5bb5e-107">What will you do</span></span>
<span data-ttu-id="5bb5e-108">Aplikace Azure funkce a účet úložiště Azure, použijte toocreate šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5bb5e-108">Use an Azure Resource Manager template toocreate an Azure function app and an Azure storage account.</span></span> <span data-ttu-id="5bb5e-109">aplikaci Azure Funkce Hello naslouchá tooAzure IoT Centrum událostí, zpracuje příchozí zprávy a zapíše tooAzure tabulka úložiště.</span><span class="sxs-lookup"><span data-stu-id="5bb5e-109">hello Azure function app listens tooAzure IoT hub events, processes incoming messages, and writes them tooAzure Table storage.</span></span>

<span data-ttu-id="5bb5e-110">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky pro panel Adafruit prolnutí M0 Wi-Fi Arduino](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="5bb5e-110">If you have any problems, look for solutions on hello [troubleshooting page for your Adafruit Feather M0 WiFi Arduino board](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).</span></span>

## <a name="what-will-you-learn"></a><span data-ttu-id="5bb5e-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="5bb5e-111">What will you learn</span></span>
<span data-ttu-id="5bb5e-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="5bb5e-112">In this article, you will learn:</span></span>
* <span data-ttu-id="5bb5e-113">Jak toouse [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) toodeploy Azure prostředky.</span><span class="sxs-lookup"><span data-stu-id="5bb5e-113">How toouse [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) toodeploy Azure resources.</span></span>
* <span data-ttu-id="5bb5e-114">Jak toouse Azure funkce aplikace tooprocess zprávy centra IoT a jejich zápis tooa tabulky ve službě Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="5bb5e-114">How toouse an Azure function app tooprocess IoT hub messages and write them tooa table in Azure Table storage.</span></span>

## <a name="what-do-you-need"></a><span data-ttu-id="5bb5e-115">Co je potřeba</span><span class="sxs-lookup"><span data-stu-id="5bb5e-115">What do you need</span></span>
<span data-ttu-id="5bb5e-116">Musíte úspěšně jste dokončili:</span><span class="sxs-lookup"><span data-stu-id="5bb5e-116">You must have successfully completed:</span></span>
- <span data-ttu-id="5bb5e-117">[Začínáme s Arduino panel][get-started]</span><span class="sxs-lookup"><span data-stu-id="5bb5e-117">[Get started with your Arduino board][get-started]</span></span>
- <span data-ttu-id="5bb5e-118">[Vytvoření služby Azure IoT hub][create-iot-hub]</span><span class="sxs-lookup"><span data-stu-id="5bb5e-118">[Create your Azure IoT hub][create-iot-hub]</span></span>

## <a name="open-hello-sample-app"></a><span data-ttu-id="5bb5e-119">Ukázkové aplikace otevřete hello</span><span class="sxs-lookup"><span data-stu-id="5bb5e-119">Open hello sample app</span></span>
<span data-ttu-id="5bb5e-120">Otevřete hello ukázkový projekt ve Visual Studio Code spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="5bb5e-120">Open hello sample project in Visual Studio Code by running hello following commands:</span></span>

```bash
cd Lesson3
code .
```

![Struktura úložiště][repo-structure]

* <span data-ttu-id="5bb5e-122">Hello `app.ino` souboru v hello `app` podsložky je hello klíče zdrojového souboru.</span><span class="sxs-lookup"><span data-stu-id="5bb5e-122">hello `app.ino` file in hello `app` subfolder is hello key source file.</span></span> <span data-ttu-id="5bb5e-123">Tento zdrojový soubor obsahuje kód toosend hello zprávu 20krát tooyour IoT hub a blikání hello DIODU pro každou zprávu, že odešle.</span><span class="sxs-lookup"><span data-stu-id="5bb5e-123">This source file contains hello code toosend a message 20 times tooyour IoT hub and blink hello LED for each message it sends.</span></span>
* <span data-ttu-id="5bb5e-124">Hello `config.json` obsahuje požadovaná nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5bb5e-124">hello `config.json` contains required configuration settings.</span></span>
* <span data-ttu-id="5bb5e-125">Hello `arm-template.json` soubor je hello šablony Azure Resource Manageru, která obsahuje aplikaci Azure funkce a účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="5bb5e-125">hello `arm-template.json` file is hello Azure Resource Manager template that contains an Azure function app and an Azure storage account.</span></span>
* <span data-ttu-id="5bb5e-126">Hello `arm-template-param.json` soubor je soubor konfigurace hello používá šablony Azure Resource Manageru hello.</span><span class="sxs-lookup"><span data-stu-id="5bb5e-126">hello `arm-template-param.json` file is hello configuration file used by hello Azure Resource Manager template.</span></span>
* <span data-ttu-id="5bb5e-127">Hello `ReceiveDeviceMessages` podsložky obsahuje kód Node.js hello pro hello Azure funkce.</span><span class="sxs-lookup"><span data-stu-id="5bb5e-127">hello `ReceiveDeviceMessages` subfolder contains hello Node.js code for hello Azure function.</span></span>

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a><span data-ttu-id="5bb5e-128">Konfigurace šablon Azure Resource Manager a vytváření prostředků v Azure</span><span class="sxs-lookup"><span data-stu-id="5bb5e-128">Configure Azure Resource Manager templates and create resources in Azure</span></span>
<span data-ttu-id="5bb5e-129">Aktualizace hello `arm-template-param.json` soubor ve Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5bb5e-129">Update hello `arm-template-param.json` file in Visual Studio Code.</span></span>

![Parametry šablony Azure Resource Manager][arm-template-params]

* <span data-ttu-id="5bb5e-131">Nahraďte **[název služby IoT Hub]** s **{Moje název centra}** kterou jste zadali při jste [vytvoření služby IoT hub a registraci panel Arduino][created-iot-hub-and-registered-arduino-board].</span><span class="sxs-lookup"><span data-stu-id="5bb5e-131">Replace **[your IoT Hub name]** with **{my hub name}** that you specified when you [created your IoT hub and registered your Arduino board][created-iot-hub-and-registered-arduino-board].</span></span>
* <span data-ttu-id="5bb5e-132">Nahraďte **[předpony řetězec pro nové prostředky]** s jakoukoli předponu chcete.</span><span class="sxs-lookup"><span data-stu-id="5bb5e-132">Replace **[prefix string for new resources]** with any prefix you want.</span></span> <span data-ttu-id="5bb5e-133">Předpona Hello zajistí, že tento název zdroje hello je globálně jedinečný tooavoid konflikt.</span><span class="sxs-lookup"><span data-stu-id="5bb5e-133">hello prefix ensures that hello resource name is globally unique tooavoid conflict.</span></span> <span data-ttu-id="5bb5e-134">Nepoužívejte dash nebo číslo počáteční v hello předponu.</span><span class="sxs-lookup"><span data-stu-id="5bb5e-134">Do not use a dash or number initial in hello prefix.</span></span>

<span data-ttu-id="5bb5e-135">Po dokončení aktualizace hello `arm-template-param.json` souboru, nasazení hello prostředky tooAzure spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5bb5e-135">After you update hello `arm-template-param.json` file, deploy hello resources tooAzure by running hello following command:</span></span>

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

<span data-ttu-id="5bb5e-136">Trvá asi 5 minut toocreate tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="5bb5e-136">It takes about five minutes toocreate these resources.</span></span> <span data-ttu-id="5bb5e-137">Při vytvoření prostředku hello probíhá, můžete přesunout na následující článek toohello.</span><span class="sxs-lookup"><span data-stu-id="5bb5e-137">While hello resource creation is in progress, you can move on toohello next article.</span></span>

## <a name="summary"></a><span data-ttu-id="5bb5e-138">Souhrn</span><span class="sxs-lookup"><span data-stu-id="5bb5e-138">Summary</span></span>
<span data-ttu-id="5bb5e-139">Jste vytvořili vaší aplikaci tooprocess funkce Azure IoT hub zprávy a Azure storage účet toostore tyto zprávy.</span><span class="sxs-lookup"><span data-stu-id="5bb5e-139">You've created your Azure function app tooprocess IoT hub messages and an Azure storage account toostore these messages.</span></span> <span data-ttu-id="5bb5e-140">Teď můžete nasadit a spustit zpráv typu zařízení cloud hello ukázkové toosend vaší Arduino karty.</span><span class="sxs-lookup"><span data-stu-id="5bb5e-140">You can now deploy and run hello sample toosend device-to-cloud messages on your Arduino board.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5bb5e-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5bb5e-141">Next steps</span></span>
<span data-ttu-id="5bb5e-142">[Spuštění ukázkové aplikace toosend zpráv typu zařízení cloud na vaší kartě Arduino][send-device-to-cloud-messages]</span><span class="sxs-lookup"><span data-stu-id="5bb5e-142">[Run a sample application toosend device-to-cloud messages on your Arduino board][send-device-to-cloud-messages]</span></span>

<!-- Images and links -->

[get-started]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[create-iot-hub]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/repo_structure_c.png
[arm-template-params]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/arm_para_arduino.png
[created-iot-hub-and-registered-arduino-board]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[send-device-to-cloud-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-run-azure-blink.md