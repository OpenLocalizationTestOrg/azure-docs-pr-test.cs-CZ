---
title: "Zařízení SensorTag & brány Azure IoT - Lekce 4: vytvoření aplikace pro funkce | Microsoft Docs"
description: "Uložení zpráv ze služby IoT hub Intel NUC tooyour, zapisuje je úložiště Table tooAzure a číst je z cloudu hello."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "ukládání dat v cloudu hello, data uložená v cloudu, iot cloudové služby"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: f84f9a85-e2c4-4a92-8969-f65eb34c194e
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: efee3bdc15ced104651f4a500311a5fe614267c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-function-app-and-storage-account"></a><span data-ttu-id="59681-104">Vytvoření Azure Function App a účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="59681-104">Create an Azure function app and storage account</span></span>

<span data-ttu-id="59681-105">Azure Functions je řešení umožňující snadno spouštět _funkce_ (malé části kódu) v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="59681-105">Azure Functions is a solution for easily running _functions_ (small pieces of code) in hello cloud.</span></span> <span data-ttu-id="59681-106">Aplikace Azure funkce hostuje hello provádění funkcí v Azure.</span><span class="sxs-lookup"><span data-stu-id="59681-106">An Azure function app hosts hello execution of your functions in Azure.</span></span> 

## <a name="what-you-will-do"></a><span data-ttu-id="59681-107">Co provedete</span><span class="sxs-lookup"><span data-stu-id="59681-107">What you will do</span></span>

- <span data-ttu-id="59681-108">Aplikace Azure funkce a účet úložiště Azure, použijte toocreate šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="59681-108">Use an Azure Resource Manager template toocreate an Azure function app and an Azure storage account.</span></span> <span data-ttu-id="59681-109">aplikaci Azure Funkce Hello naslouchá tooAzure IoT Centrum událostí, zpracuje příchozí zprávy a zapíše tooAzure tabulka úložiště.</span><span class="sxs-lookup"><span data-stu-id="59681-109">hello Azure function app listens tooAzure IoT hub events, processes incoming messages, and writes them tooAzure Table storage.</span></span>

<span data-ttu-id="59681-110">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="59681-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>


## <a name="what-you-will-learn"></a><span data-ttu-id="59681-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="59681-111">What you will learn</span></span>

<span data-ttu-id="59681-112">V této lekci se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="59681-112">In this lesson, you will learn:</span></span>

- <span data-ttu-id="59681-113">Jak toouse Azure Resource Manager toodeploy prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="59681-113">How toouse Azure Resource Manager toodeploy Azure resources.</span></span>
- <span data-ttu-id="59681-114">Jak toouse Azure funkce aplikace tooprocess zprávy služby IoT Hub a jejich zápis tooa tabulky ve službě Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="59681-114">How toouse an Azure function app tooprocess IoT Hub messages and write them tooa table in Azure Table storage.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="59681-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="59681-115">What you need</span></span>

<span data-ttu-id="59681-116">Musíte úspěšně jste dokončili předchozí lekce hello:</span><span class="sxs-lookup"><span data-stu-id="59681-116">You must have successfully completed hello previous lessons:</span></span>

- [<span data-ttu-id="59681-117">Lekce 1: Nastavení vaší Intel NUC jako bránu IoT</span><span class="sxs-lookup"><span data-stu-id="59681-117">Lesson 1: Set up your Intel NUC as an IoT gateway</span></span>](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
- [<span data-ttu-id="59681-118">Lekce 2: Příprava hostitelský počítač a Azure IoT hub</span><span class="sxs-lookup"><span data-stu-id="59681-118">Lesson 2: Get your host computer and Azure IoT hub ready</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)
- [<span data-ttu-id="59681-119">Lekce 3: Přijetí zpráv z SensorTag a čtení zpráv ze služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="59681-119">Lesson 3: Receive messages from SensorTag and read messages from IoT hub</span></span>](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)

## <a name="open-a-sample-app"></a><span data-ttu-id="59681-120">Otevřete ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="59681-120">Open a sample app</span></span>

<span data-ttu-id="59681-121">Přejděte tooyour `iot-hub-c-intel-nuc-gateway-getting-started` složky úložišti, inicializovat hello konfigurační soubory a pak otevřete hello ukázkový projekt ve Visual Studio Code spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="59681-121">Go tooyour `iot-hub-c-intel-nuc-gateway-getting-started` repo folder, initialize hello configuration files, and then open hello sample project in Visual Studio Code by running hello following command:</span></span>

```bash
cd Lesson4
npm install
gulp init
code .
```

![Struktura úložiště](media/iot-hub-gateway-kit-lessons/lesson4/arm_template.png)

- <span data-ttu-id="59681-123">Hello `arm-template.json` soubor je hello šablony Azure Resource Manageru, která obsahuje aplikaci Azure funkce a účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="59681-123">hello `arm-template.json` file is hello Azure Resource Manager template that contains an Azure function app and an Azure storage account.</span></span>
- <span data-ttu-id="59681-124">Hello `arm-template-param.json` soubor je soubor konfigurace hello používá šablony Azure Resource Manageru hello.</span><span class="sxs-lookup"><span data-stu-id="59681-124">hello `arm-template-param.json` file is hello configuration file used by hello Azure Resource Manager template.</span></span>
- <span data-ttu-id="59681-125">Hello `ReceiveDeviceMessages` podsložky obsahuje kód Node.js hello pro hello Azure funkce.</span><span class="sxs-lookup"><span data-stu-id="59681-125">hello `ReceiveDeviceMessages` subfolder contains hello Node.js code for hello Azure function.</span></span>

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a><span data-ttu-id="59681-126">Konfigurace šablon Azure Resource Manager a vytváření prostředků v Azure</span><span class="sxs-lookup"><span data-stu-id="59681-126">Configure Azure Resource Manager templates and create resources in Azure</span></span>

<span data-ttu-id="59681-127">Aktualizace hello `arm-template-param.json` soubor ve Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="59681-127">Update hello `arm-template-param.json` file in Visual Studio Code.</span></span>

![json šablony arm](media/iot-hub-gateway-kit-lessons/lesson4/arm_template_param.png)

- <span data-ttu-id="59681-129">Nahraďte `[your IoT Hub name]` s `{my hub name}` který jste zadali v lekci 2.</span><span class="sxs-lookup"><span data-stu-id="59681-129">Replace `[your IoT Hub name]` with `{my hub name}` that you specified in Lesson 2.</span></span>

<span data-ttu-id="59681-130">Po dokončení aktualizace hello `arm-template-param.json` souboru, nasazení hello prostředky tooAzure spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="59681-130">After you update hello `arm-template-param.json` file, deploy hello resources tooAzure by running hello following command:</span></span>

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-gateway
```

<span data-ttu-id="59681-131">Použití `iot-gateway` jako hodnota hello `{resource group name}` Pokud hodnota hello v lekci 2 nebyla změněna.</span><span class="sxs-lookup"><span data-stu-id="59681-131">Use `iot-gateway` as hello value of `{resource group name}` if you didn't change hello value in Lesson 2.</span></span>

## <a name="summary"></a><span data-ttu-id="59681-132">Souhrn</span><span class="sxs-lookup"><span data-stu-id="59681-132">Summary</span></span>

<span data-ttu-id="59681-133">Jste vytvořili vaší aplikaci tooprocess funkce Azure IoT hub zprávy a Azure storage účet toostore tyto zprávy.</span><span class="sxs-lookup"><span data-stu-id="59681-133">You've created your Azure function app tooprocess IoT hub messages and an Azure storage account toostore these messages.</span></span> <span data-ttu-id="59681-134">Teď si můžete přečíst zprávy odesílané ze služby IoT hub tooyour brány.</span><span class="sxs-lookup"><span data-stu-id="59681-134">You can now read messages that are sent by your gateway tooyour IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="59681-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="59681-135">Next steps</span></span>
<span data-ttu-id="59681-136">[Čtení zprávy uchovávané v úložišti Azure](iot-hub-gateway-kit-c-lesson4-read-table-storage.md).</span><span class="sxs-lookup"><span data-stu-id="59681-136">[Read messages persisted in Azure Storage](iot-hub-gateway-kit-c-lesson4-read-table-storage.md).</span></span>
