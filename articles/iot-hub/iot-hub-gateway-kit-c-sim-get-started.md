---
title: "Simulované zařízení & brány Azure IoT - Začínáme | Microsoft Docs"
description: "Začínáme s IoT brány Starter Kit, vytvoření služby Azure IoT hub a připojení brány do služby IoT hub"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Centrum Azure iot hub iot brány, Začínáme se službou internet věcí, iot toolkit"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 0c110b8b-bee4-4aec-a18a-dfc292aa17a3
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 916fa40d9ac857dfa72197b40c232834593d3891
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-iot-gateway-starter-kit-with-a-simulated-device"></a><span data-ttu-id="79f63-104">Začínáme s IoT brány Starter Kit s simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="79f63-104">Get started with IoT Gateway Starter Kit with a simulated device</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="79f63-105">SensorTag</span><span class="sxs-lookup"><span data-stu-id="79f63-105">SensorTag</span></span>](iot-hub-gateway-kit-c-get-started.md)
> * [<span data-ttu-id="79f63-106">Simulované zařízení</span><span class="sxs-lookup"><span data-stu-id="79f63-106">Simulated Device</span></span>](iot-hub-gateway-kit-c-sim-get-started.md)

<span data-ttu-id="79f63-107">V tomto kurzu začnete podle učení základní informace o práci s [IoT brány Starter Kit](https://aka.ms/gateway-kit).</span><span class="sxs-lookup"><span data-stu-id="79f63-107">In this tutorial, you begin by learning the basics of working with [IoT Gateway Starter Kit](https://aka.ms/gateway-kit).</span></span> <span data-ttu-id="79f63-108">Budete pracovat s NUC Intel, se systémem Linux větru oblasti.</span><span class="sxs-lookup"><span data-stu-id="79f63-108">You will be working with Intel NUC that's running Wind River Linux.</span></span> <span data-ttu-id="79f63-109">Se dozvíte, jak k seamleesly připojení zařízení do cloudu pomocí Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="79f63-109">You will learn how to seamleesly connect your devices to the cloud by using Azure IoT Hub.</span></span>

***
<span data-ttu-id="79f63-110">**Ještě není hotová kit?:** klikněte na tlačítko [zde](https://aka.ms/gateway-kit).</span><span class="sxs-lookup"><span data-stu-id="79f63-110">**Don't have a kit yet?:** Click [here](https://aka.ms/gateway-kit).</span></span>
***

## <a name="lesson-1-configure-your-nuc"></a><span data-ttu-id="79f63-111">Lekce 1: Konfigurace NUC</span><span class="sxs-lookup"><span data-stu-id="79f63-111">Lesson 1: Configure your NUC</span></span>
![Diagram začátku do konce Lesson1](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson1.png)

<span data-ttu-id="79f63-113">V této lekci nastavit NUC Intel (Další jednotky Computing) v sadě Kit jako bránu Azure IoT, nainstalovat balíček Azure IoT Edge na NUC a spusťte ukázkové aplikace, pokud chcete ověřit funkčnost brány.</span><span class="sxs-lookup"><span data-stu-id="79f63-113">In this lesson, you set up Intel NUC (Next Unit of Computing) in the Kit as an Azure IoT gateway, install the Azure IoT Edge package on NUC, and run a sample app to verify the gateway functionality.</span></span>

<span data-ttu-id="79f63-114">*Odhadovaný čas dokončení: 15 minut*</span><span class="sxs-lookup"><span data-stu-id="79f63-114">*Estimated time to complete: 15 minutes*</span></span>

<span data-ttu-id="79f63-115">Přejděte na [nastavit Intel NUC jako bránu IoT](iot-hub-gateway-kit-c-sim-lesson1-set-up-nuc.md)</span><span class="sxs-lookup"><span data-stu-id="79f63-115">Go to [Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-sim-lesson1-set-up-nuc.md)</span></span>

## <a name="lesson-2-create-your-iot-hub"></a><span data-ttu-id="79f63-116">Lekce 2: Vytvoření služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="79f63-116">Lesson 2: Create your IoT Hub</span></span>
![Diagram začátku do konce Lesson2](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson2.png)

<span data-ttu-id="79f63-118">V této lekci nainstalovat nástroje a software v hostitelském počítači.</span><span class="sxs-lookup"><span data-stu-id="79f63-118">In this lesson, you install the tools and software on your host computer.</span></span> <span data-ttu-id="79f63-119">Pak vytvořte účet Azure zdarma, zřídit služby Azure IoT hub a vytvoření vaší první zařízení ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="79f63-119">Then you create your free Azure account, provision your Azure IoT hub and create your first device in the IoT hub.</span></span>

<span data-ttu-id="79f63-120">Před zahájením této lekci, proveďte Lekce 1.</span><span class="sxs-lookup"><span data-stu-id="79f63-120">Complete Lesson 1 before you start this lesson.</span></span>

### <a name="get-the-tools"></a><span data-ttu-id="79f63-121">Získat nástroje</span><span class="sxs-lookup"><span data-stu-id="79f63-121">Get the tools</span></span>
<span data-ttu-id="79f63-122">Nainstaluje nástroje a software v hostitelském počítači.</span><span class="sxs-lookup"><span data-stu-id="79f63-122">Install the tools and software on your host computer.</span></span>

<span data-ttu-id="79f63-123">*Odhadovaný čas dokončení: 20 minut*</span><span class="sxs-lookup"><span data-stu-id="79f63-123">*Estimated time to complete: 20 minutes*</span></span>

<span data-ttu-id="79f63-124">Přejděte na [získat nástroje](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)</span><span class="sxs-lookup"><span data-stu-id="79f63-124">Go to [Get the tools](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)</span></span>

### <a name="create-an-iot-hub-and-register-your-device"></a><span data-ttu-id="79f63-125">Vytvoření služby IoT hub a registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="79f63-125">Create an IoT hub and register your device</span></span>
<span data-ttu-id="79f63-126">Vytvoření skupiny prostředků, zřídit první Azure IoT hub a přidat svoje první zařízení ke službě IoT hub pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="79f63-126">Create your resource group, provision your first Azure IoT hub, and add your first device to the IoT hub using the Azure CLI.</span></span>

<span data-ttu-id="79f63-127">*Odhadovaný čas dokončení: 10 minut*</span><span class="sxs-lookup"><span data-stu-id="79f63-127">*Estimated time to complete: 10 minutes*</span></span>

<span data-ttu-id="79f63-128">Přejděte na [vytvoření služby IoT hub a registrace zařízení](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)</span><span class="sxs-lookup"><span data-stu-id="79f63-128">Go to [Create an IoT hub and register your device](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)</span></span>

## <a name="lesson-3-receive-messages-from-the-simulated-device-and-read-messages-from-your-iot-hub"></a><span data-ttu-id="79f63-129">Lekce 3: Přijímat zprávy ze simulovaného zařízení a čtení zpráv z centra IoT</span><span class="sxs-lookup"><span data-stu-id="79f63-129">Lesson 3: Receive messages from the simulated device and read messages from your IoT hub</span></span>
<span data-ttu-id="79f63-130">V této lekci použijete skripty pro automatizaci konfigurace a spuštění aplikace simulovaného zařízení ve vaší brány.</span><span class="sxs-lookup"><span data-stu-id="79f63-130">In this lesson, you will use scripts to automate the configuration and execution of a simulated device app in your gateway.</span></span> <span data-ttu-id="79f63-131">Aplikaci simulovaného zařízení vygeneruje ukázková teploty data a odešle ji do modul služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="79f63-131">The simulated device app generates sample temperature data and sends it to an IoT hub module.</span></span> <span data-ttu-id="79f63-132">Modul IoT hub balíčky získaná data a odešle ji do služby IoT hub prostřednictvím rozhraní brány v Azure IoT okraj.</span><span class="sxs-lookup"><span data-stu-id="79f63-132">The IoT hub module packages the data received and sends it to your IoT hub through the gateway framework provided in Azure IoT Edge.</span></span>

![Diagram začátku do konce Lekce 3](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson3.png)

### <a name="configure-and-run-a-simulated-device"></a><span data-ttu-id="79f63-134">Nakonfigurujte a spusťte simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="79f63-134">Configure and run a simulated device</span></span>
<span data-ttu-id="79f63-135">Připravte ukázkové kódy.</span><span class="sxs-lookup"><span data-stu-id="79f63-135">Prepare the sample codes.</span></span> <span data-ttu-id="79f63-136">Potom nakonfigurujte a spusťte ukázkovou aplikaci simulovaného zařízení.</span><span class="sxs-lookup"><span data-stu-id="79f63-136">Then configure and run the simulated device sample application.</span></span>

<span data-ttu-id="79f63-137">*Odhadovaný čas dokončení: 15 minut*</span><span class="sxs-lookup"><span data-stu-id="79f63-137">*Estimated time to complete: 15 minutes*</span></span>

<span data-ttu-id="79f63-138">Přejděte na [konfigurace a spuštění simulovaného zařízení](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md)</span><span class="sxs-lookup"><span data-stu-id="79f63-138">Go to [Configure and run a simulated device](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md)</span></span>

### <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="79f63-139">Čtení zpráv z centra IoT</span><span class="sxs-lookup"><span data-stu-id="79f63-139">Read messages from your IoT hub</span></span>
<span data-ttu-id="79f63-140">Spustíte ukázkový kód v hostitelském počítači ke čtení zpráv ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="79f63-140">Run a sample code on your host computer to read the messages from your IoT hub.</span></span>

<span data-ttu-id="79f63-141">*Odhadovaný čas dokončení: 15 minut*</span><span class="sxs-lookup"><span data-stu-id="79f63-141">*Estimated time to complete: 15 minutes*</span></span>

<span data-ttu-id="79f63-142">Přejděte na [čtení zpráv z centra IoT](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md)</span><span class="sxs-lookup"><span data-stu-id="79f63-142">Go to [Read messages from your IoT hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md)</span></span>

## <a name="lesson-4-save-messages-to-azure-table-storage"></a><span data-ttu-id="79f63-143">Lekce 4: Uložení zpráv do Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="79f63-143">Lesson 4: Save messages to Azure Table storage</span></span>
<span data-ttu-id="79f63-144">Vytvořte aplikaci Azure funkce, která se získá příchozích zpráv ze služby IoT hub a zapisuje je do Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="79f63-144">Create an Azure function app that gets incoming messages from your IoT hub and writes them to Azure Table storage.</span></span>

![Diagram začátku do konce Lekce 4](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson4.png)

### <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="79f63-146">Vytvořte aplikaci Azure funkce a účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="79f63-146">Create an Azure function app and Azure Storage account</span></span>
<span data-ttu-id="79f63-147">Vytvoření aplikace Azure funkce a účet úložiště Azure pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="79f63-147">Use an Azure Resource Manager template to create an Azure function app and an Azure Storage account.</span></span>

<span data-ttu-id="79f63-148">*Odhadovaný čas dokončení: 10 minut*</span><span class="sxs-lookup"><span data-stu-id="79f63-148">*Estimated time to complete: 10 minutes*</span></span>

<span data-ttu-id="79f63-149">Přejděte na [vytvořte aplikaci Azure funkce a účet úložiště Azure](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md)</span><span class="sxs-lookup"><span data-stu-id="79f63-149">Go to [Create an Azure function app and Azure Storage account](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md)</span></span>

### <a name="read-messages-persisted-in-azure-table-storage"></a><span data-ttu-id="79f63-150">Čtení zprávy uchovávané v úložišti Azure Table</span><span class="sxs-lookup"><span data-stu-id="79f63-150">Read messages persisted in Azure Table storage</span></span>
<span data-ttu-id="79f63-151">Sledujte zprávy odesílané brány do cloudu, jako jsou zapsány do Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="79f63-151">Monitor the gateway-to-cloud messages as they are written to Azure Table storage.</span></span>

<span data-ttu-id="79f63-152">*Odhadovaný čas dokončení: 5 minut*</span><span class="sxs-lookup"><span data-stu-id="79f63-152">*Estimated time to complete: 5 minutes*</span></span>

<span data-ttu-id="79f63-153">Přejděte na [číst zprávy uchovávané v úložišti Azure Table](iot-hub-gateway-kit-c-sim-lesson4-read-table-storage.md).</span><span class="sxs-lookup"><span data-stu-id="79f63-153">Go to [Read messages persisted in Azure Table storage](iot-hub-gateway-kit-c-sim-lesson4-read-table-storage.md).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="79f63-154">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="79f63-154">Troubleshooting</span></span>
<span data-ttu-id="79f63-155">Pokud máte potíže při poznatky, vyhledejte řešení v [Poradce při potížích s](iot-hub-gateway-kit-c-sim-troubleshooting.md) článku.</span><span class="sxs-lookup"><span data-stu-id="79f63-155">If you have any problems during the lessons, look for solutions in the [Troubleshooting](iot-hub-gateway-kit-c-sim-troubleshooting.md) article.</span></span>

## <a name="explore-more"></a><span data-ttu-id="79f63-156">Prozkoumat další</span><span class="sxs-lookup"><span data-stu-id="79f63-156">Explore more</span></span>
<span data-ttu-id="79f63-157">Přejděte [zóny developer Kit brány IoT Intel](https://software.intel.com/en-us/iot/hardware/gateways/dev-kit) Další informace.</span><span class="sxs-lookup"><span data-stu-id="79f63-157">Visit the [Intel IoT Gateway Kit developer zone](https://software.intel.com/en-us/iot/hardware/gateways/dev-kit) to learn more.</span></span>