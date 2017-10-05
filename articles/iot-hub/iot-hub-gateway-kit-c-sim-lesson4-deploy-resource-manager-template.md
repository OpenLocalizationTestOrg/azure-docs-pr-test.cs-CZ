---
title: "Simulované zařízení & brány Azure IoT - Lekce 4: uložení zpráv | Microsoft Docs"
description: "Ukládat zprávy z Intel NUC do služby IoT hub, zapisuje je do úložiště Azure Table a číst je z cloudu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "ukládání dat v cloudu, data uložená v cloudu, iot cloudové služby"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: ffed0c2e-b092-40e1-9113-8196ec057d67
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c7fc47b07acede28ffe790debca7e38521726011
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-function-app-and-storage-account"></a><span data-ttu-id="94f5c-104">Vytvoření Azure Function App a účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="94f5c-104">Create an Azure function app and storage account</span></span>

<span data-ttu-id="94f5c-105">Azure Functions je řešení umožňující snadno spouštět _funkce_ (malé části kódu) v cloudu.</span><span class="sxs-lookup"><span data-stu-id="94f5c-105">Azure Functions is a solution for easily running _functions_ (small pieces of code) in the cloud.</span></span> <span data-ttu-id="94f5c-106">Aplikace Azure funkce hostuje provádění funkcí v Azure.</span><span class="sxs-lookup"><span data-stu-id="94f5c-106">An Azure function app hosts the execution of your functions in Azure.</span></span> 

## <a name="what-you-will-do"></a><span data-ttu-id="94f5c-107">Co provedete</span><span class="sxs-lookup"><span data-stu-id="94f5c-107">What you will do</span></span>

- <span data-ttu-id="94f5c-108">Vytvoření aplikace Azure funkce a účet úložiště Azure pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="94f5c-108">Use an Azure Resource Manager template to create an Azure function app and an Azure storage account.</span></span> <span data-ttu-id="94f5c-109">Aplikace Azure funkce naslouchá událostem, Azure IoT hub, zpracuje příchozí zprávy a zapisuje je do Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="94f5c-109">The Azure function app listens to Azure IoT hub events, processes incoming messages, and writes them to Azure Table storage.</span></span>

<span data-ttu-id="94f5c-110">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="94f5c-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>


## <a name="what-you-will-learn"></a><span data-ttu-id="94f5c-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="94f5c-111">What you will learn</span></span>

<span data-ttu-id="94f5c-112">V této lekci se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="94f5c-112">In this lesson, you will learn:</span></span>

- <span data-ttu-id="94f5c-113">Jak používat Azure Resource Manager k nasazení prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="94f5c-113">How to use Azure Resource Manager to deploy Azure resources.</span></span>
- <span data-ttu-id="94f5c-114">Jak používat aplikaci Azure funkce zpracování zpráv služby IoT Hub a jejich zápis do tabulky v Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="94f5c-114">How to use an Azure function app to process IoT Hub messages and write them to a table in Azure Table storage.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="94f5c-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="94f5c-115">What you need</span></span>

<span data-ttu-id="94f5c-116">Musíte úspěšně jste dokončili předchozí lekce:</span><span class="sxs-lookup"><span data-stu-id="94f5c-116">You must have successfully completed the previous lessons:</span></span>

- [<span data-ttu-id="94f5c-117">Lekce 1: Nastavení vaší Intel NUC jako bránu IoT</span><span class="sxs-lookup"><span data-stu-id="94f5c-117">Lesson 1: Set up your Intel NUC as an IoT gateway</span></span>](iot-hub-gateway-kit-c-sim-lesson1-set-up-nuc.md)
- [<span data-ttu-id="94f5c-118">Lekce 2: Příprava hostitelský počítač a Azure IoT hub</span><span class="sxs-lookup"><span data-stu-id="94f5c-118">Lesson 2: Get your host computer and Azure IoT hub ready</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
- [<span data-ttu-id="94f5c-119">Lekce 3: Přijímat zprávy ze simulovaného zařízení a čtení zpráv z centra IoT</span><span class="sxs-lookup"><span data-stu-id="94f5c-119">Lesson 3: Receive messages from the simulated device and read messages from your IoT hub</span></span>](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md)

## <a name="open-a-sample-app"></a><span data-ttu-id="94f5c-120">Otevřete ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="94f5c-120">Open a sample app</span></span>

<span data-ttu-id="94f5c-121">Přejděte na vaše `iot-hub-c-intel-nuc-gateway-getting-started` úložišti složky inicializovat konfigurační soubory a pak otevřete ukázkový projekt ve Visual Studio Code spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="94f5c-121">Go to your `iot-hub-c-intel-nuc-gateway-getting-started` repo folder, initialize the configuration files, and then open the sample project in Visual Studio Code by running the following command:</span></span>

```bash
cd Lesson4
npm install
gulp init
code .
```

![Struktura úložiště](media/iot-hub-gateway-kit-lessons/lesson4/arm_template.png)

- <span data-ttu-id="94f5c-123">`arm-template.json` Soubor je šablony Azure Resource Manager, která obsahuje aplikaci Azure funkce a účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="94f5c-123">The `arm-template.json` file is the Azure Resource Manager template that contains an Azure function app and an Azure storage account.</span></span>
- <span data-ttu-id="94f5c-124">`arm-template-param.json` Soubor je soubor konfigurace používané šablony Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="94f5c-124">The `arm-template-param.json` file is the configuration file used by the Azure Resource Manager template.</span></span>
- <span data-ttu-id="94f5c-125">`ReceiveDeviceMessages` Podsložky obsahuje kód Node.js pro funkci Azure.</span><span class="sxs-lookup"><span data-stu-id="94f5c-125">The `ReceiveDeviceMessages` subfolder contains the Node.js code for the Azure function.</span></span>

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a><span data-ttu-id="94f5c-126">Konfigurace šablon Azure Resource Manager a vytváření prostředků v Azure</span><span class="sxs-lookup"><span data-stu-id="94f5c-126">Configure Azure Resource Manager templates and create resources in Azure</span></span>

<span data-ttu-id="94f5c-127">Aktualizace `arm-template-param.json` soubor ve Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="94f5c-127">Update the `arm-template-param.json` file in Visual Studio Code.</span></span>

![json šablony arm](media/iot-hub-gateway-kit-lessons/lesson4/arm_template_param.png)

- <span data-ttu-id="94f5c-129">Nahraďte `[your IoT Hub name]` s `{my hub name}` který jste zadali v lekci 2.</span><span class="sxs-lookup"><span data-stu-id="94f5c-129">Replace `[your IoT Hub name]` with `{my hub name}` that you specified in Lesson 2.</span></span>

<span data-ttu-id="94f5c-130">Po provedení aktualizace `arm-template-param.json` souboru, nasadit tyto prostředky do Azure tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="94f5c-130">After you update the `arm-template-param.json` file, deploy the resources to Azure by running the following command:</span></span>

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-gateway
```

<span data-ttu-id="94f5c-131">Použití `iot-gateway` jako hodnotu `{resource group name}` Pokud nebylo změňte hodnotu v lekci 2.</span><span class="sxs-lookup"><span data-stu-id="94f5c-131">Use `iot-gateway` as the value of `{resource group name}` if you didn't change the value in Lesson 2.</span></span>

## <a name="summary"></a><span data-ttu-id="94f5c-132">Souhrn</span><span class="sxs-lookup"><span data-stu-id="94f5c-132">Summary</span></span>

<span data-ttu-id="94f5c-133">Vytvoření aplikace Azure funkce pro zpracování zprávy IoT hub a účet úložiště Azure slouží k uložení těchto zpráv.</span><span class="sxs-lookup"><span data-stu-id="94f5c-133">You've created your Azure function app to process IoT hub messages and an Azure storage account to store these messages.</span></span> <span data-ttu-id="94f5c-134">Teď si můžete přečíst zprávy, které odesílá bránu do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="94f5c-134">You can now read messages that are sent by your gateway to your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="94f5c-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="94f5c-135">Next steps</span></span>
<span data-ttu-id="94f5c-136">[Čtení zprávy uchovávané v úložišti Azure](iot-hub-gateway-kit-c-sim-lesson4-read-table-storage.md).</span><span class="sxs-lookup"><span data-stu-id="94f5c-136">[Read messages persisted in Azure Storage](iot-hub-gateway-kit-c-sim-lesson4-read-table-storage.md).</span></span>
