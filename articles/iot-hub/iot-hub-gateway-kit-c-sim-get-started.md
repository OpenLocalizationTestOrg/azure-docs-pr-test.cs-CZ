---
title: "Simulované zařízení & brány Azure IoT - Začínáme | Microsoft Docs"
description: "Začínáme s IoT brány Starter Kit, vytvoření služby Azure IoT hub a připojení brány toohello IoT hub"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Centrum Azure iot hub iot brány, Začínáme se službou hello internet věcí, iot toolkit"
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
ms.openlocfilehash: 1a54a5e5f1c1d9b2e657c9e4448274256e2533f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-iot-gateway-starter-kit-with-a-simulated-device"></a><span data-ttu-id="6c434-104">Začínáme s IoT brány Starter Kit s simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="6c434-104">Get started with IoT Gateway Starter Kit with a simulated device</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="6c434-105">SensorTag</span><span class="sxs-lookup"><span data-stu-id="6c434-105">SensorTag</span></span>](iot-hub-gateway-kit-c-get-started.md)
> * [<span data-ttu-id="6c434-106">Simulované zařízení</span><span class="sxs-lookup"><span data-stu-id="6c434-106">Simulated Device</span></span>](iot-hub-gateway-kit-c-sim-get-started.md)

<span data-ttu-id="6c434-107">V tomto kurzu začnete podle učení hello základy práce s [IoT brány Starter Kit](https://aka.ms/gateway-kit).</span><span class="sxs-lookup"><span data-stu-id="6c434-107">In this tutorial, you begin by learning hello basics of working with [IoT Gateway Starter Kit](https://aka.ms/gateway-kit).</span></span> <span data-ttu-id="6c434-108">Budete pracovat s NUC Intel, se systémem Linux větru oblasti.</span><span class="sxs-lookup"><span data-stu-id="6c434-108">You will be working with Intel NUC that's running Wind River Linux.</span></span> <span data-ttu-id="6c434-109">Se dozvíte, jak tooseamleesly propojit své cloudové toohello zařízení pomocí služby Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="6c434-109">You will learn how tooseamleesly connect your devices toohello cloud by using Azure IoT Hub.</span></span>

***
<span data-ttu-id="6c434-110">**Ještě není hotová kit?:** klikněte na tlačítko [zde](https://aka.ms/gateway-kit).</span><span class="sxs-lookup"><span data-stu-id="6c434-110">**Don't have a kit yet?:** Click [here](https://aka.ms/gateway-kit).</span></span>
***

## <a name="lesson-1-configure-your-nuc"></a><span data-ttu-id="6c434-111">Lekce 1: Konfigurace NUC</span><span class="sxs-lookup"><span data-stu-id="6c434-111">Lesson 1: Configure your NUC</span></span>
![Diagram začátku do konce Lesson1](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson1.png)

<span data-ttu-id="6c434-113">V této lekci nastavit Intel NUC (Další jednotky Computing) v hello Kit jako bránu Azure IoT, nainstalovat balíček Azure IoT Edge hello na NUC a spusťte funkci brány tooverify hello ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="6c434-113">In this lesson, you set up Intel NUC (Next Unit of Computing) in hello Kit as an Azure IoT gateway, install hello Azure IoT Edge package on NUC, and run a sample app tooverify hello gateway functionality.</span></span>

<span data-ttu-id="6c434-114">*Odhadovaný čas toocomplete: 15 minut*</span><span class="sxs-lookup"><span data-stu-id="6c434-114">*Estimated time toocomplete: 15 minutes*</span></span>

<span data-ttu-id="6c434-115">Přejděte příliš[nastavit Intel NUC jako bránu IoT](iot-hub-gateway-kit-c-sim-lesson1-set-up-nuc.md)</span><span class="sxs-lookup"><span data-stu-id="6c434-115">Go too[Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-sim-lesson1-set-up-nuc.md)</span></span>

## <a name="lesson-2-create-your-iot-hub"></a><span data-ttu-id="6c434-116">Lekce 2: Vytvoření služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="6c434-116">Lesson 2: Create your IoT Hub</span></span>
![Diagram začátku do konce Lesson2](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson2.png)

<span data-ttu-id="6c434-118">V této lekci nainstalovat nástroje hello a software na hostitelském počítači.</span><span class="sxs-lookup"><span data-stu-id="6c434-118">In this lesson, you install hello tools and software on your host computer.</span></span> <span data-ttu-id="6c434-119">Pak vytvoříte účet Azure zdarma, zřídit služby Azure IoT hub a vytvořit první zařízení IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="6c434-119">Then you create your free Azure account, provision your Azure IoT hub and create your first device in hello IoT hub.</span></span>

<span data-ttu-id="6c434-120">Před zahájením této lekci, proveďte Lekce 1.</span><span class="sxs-lookup"><span data-stu-id="6c434-120">Complete Lesson 1 before you start this lesson.</span></span>

### <a name="get-hello-tools"></a><span data-ttu-id="6c434-121">Získat nástroje hello</span><span class="sxs-lookup"><span data-stu-id="6c434-121">Get hello tools</span></span>
<span data-ttu-id="6c434-122">Nainstalujte nástroje hello a software v hostitelském počítači.</span><span class="sxs-lookup"><span data-stu-id="6c434-122">Install hello tools and software on your host computer.</span></span>

<span data-ttu-id="6c434-123">*Odhadovaný čas toocomplete: 20 minut*</span><span class="sxs-lookup"><span data-stu-id="6c434-123">*Estimated time toocomplete: 20 minutes*</span></span>

<span data-ttu-id="6c434-124">Přejděte příliš[získat nástroje hello](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)</span><span class="sxs-lookup"><span data-stu-id="6c434-124">Go too[Get hello tools](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)</span></span>

### <a name="create-an-iot-hub-and-register-your-device"></a><span data-ttu-id="6c434-125">Vytvoření služby IoT hub a registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="6c434-125">Create an IoT hub and register your device</span></span>
<span data-ttu-id="6c434-126">Vytvoření skupiny prostředků, zřídit první Azure IoT hub a přidejte první zařízení toohello IoT hub pomocí hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="6c434-126">Create your resource group, provision your first Azure IoT hub, and add your first device toohello IoT hub using hello Azure CLI.</span></span>

<span data-ttu-id="6c434-127">*Odhadovaný čas toocomplete: 10 minut*</span><span class="sxs-lookup"><span data-stu-id="6c434-127">*Estimated time toocomplete: 10 minutes*</span></span>

<span data-ttu-id="6c434-128">Přejděte příliš[vytvoření služby IoT hub a registrace zařízení](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)</span><span class="sxs-lookup"><span data-stu-id="6c434-128">Go too[Create an IoT hub and register your device](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)</span></span>

## <a name="lesson-3-receive-messages-from-hello-simulated-device-and-read-messages-from-your-iot-hub"></a><span data-ttu-id="6c434-129">Lekce 3: Přijetí zpráv z hello simulované zařízení a čtení zpráv z centra IoT</span><span class="sxs-lookup"><span data-stu-id="6c434-129">Lesson 3: Receive messages from hello simulated device and read messages from your IoT hub</span></span>
<span data-ttu-id="6c434-130">V této lekci použijete skripty tooautomate hello konfiguraci a spuštění aplikace simulovaného zařízení ve vaší brány.</span><span class="sxs-lookup"><span data-stu-id="6c434-130">In this lesson, you will use scripts tooautomate hello configuration and execution of a simulated device app in your gateway.</span></span> <span data-ttu-id="6c434-131">aplikaci simulovaného zařízení Hello vygeneruje ukázková teploty data a odešle ji tooan IoT hub modulu.</span><span class="sxs-lookup"><span data-stu-id="6c434-131">hello simulated device app generates sample temperature data and sends it tooan IoT hub module.</span></span> <span data-ttu-id="6c434-132">Hello IoT hub modulu balíčky hello dat přijatých a odešle ji tooyour IoT hub prostřednictvím hello brány rámec poskytovaný v Azure IoT Edge.</span><span class="sxs-lookup"><span data-stu-id="6c434-132">hello IoT hub module packages hello data received and sends it tooyour IoT hub through hello gateway framework provided in Azure IoT Edge.</span></span>

![Diagram začátku do konce Lekce 3](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson3.png)

### <a name="configure-and-run-a-simulated-device"></a><span data-ttu-id="6c434-134">Nakonfigurujte a spusťte simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="6c434-134">Configure and run a simulated device</span></span>
<span data-ttu-id="6c434-135">Připravte hello ukázkové kódy.</span><span class="sxs-lookup"><span data-stu-id="6c434-135">Prepare hello sample codes.</span></span> <span data-ttu-id="6c434-136">Potom nakonfigurujte a spusťte ukázkové aplikace hello simulované zařízení.</span><span class="sxs-lookup"><span data-stu-id="6c434-136">Then configure and run hello simulated device sample application.</span></span>

<span data-ttu-id="6c434-137">*Odhadovaný čas toocomplete: 15 minut*</span><span class="sxs-lookup"><span data-stu-id="6c434-137">*Estimated time toocomplete: 15 minutes*</span></span>

<span data-ttu-id="6c434-138">Přejděte příliš[konfigurace a spuštění simulovaného zařízení](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md)</span><span class="sxs-lookup"><span data-stu-id="6c434-138">Go too[Configure and run a simulated device](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md)</span></span>

### <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="6c434-139">Čtení zpráv z centra IoT</span><span class="sxs-lookup"><span data-stu-id="6c434-139">Read messages from your IoT hub</span></span>
<span data-ttu-id="6c434-140">Spusťte ukázkový kód na hostitele počítače tooread hello zpráv ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="6c434-140">Run a sample code on your host computer tooread hello messages from your IoT hub.</span></span>

<span data-ttu-id="6c434-141">*Odhadovaný čas toocomplete: 15 minut*</span><span class="sxs-lookup"><span data-stu-id="6c434-141">*Estimated time toocomplete: 15 minutes*</span></span>

<span data-ttu-id="6c434-142">Přejděte příliš[čtení zpráv z centra IoT](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md)</span><span class="sxs-lookup"><span data-stu-id="6c434-142">Go too[Read messages from your IoT hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md)</span></span>

## <a name="lesson-4-save-messages-tooazure-table-storage"></a><span data-ttu-id="6c434-143">Lekce 4: Uložení zpráv tooAzure Table storage</span><span class="sxs-lookup"><span data-stu-id="6c434-143">Lesson 4: Save messages tooAzure Table storage</span></span>
<span data-ttu-id="6c434-144">Vytvořte aplikaci Azure funkce, která se získá příchozích zpráv ze služby IoT hub a zapíše tooAzure tabulka úložiště.</span><span class="sxs-lookup"><span data-stu-id="6c434-144">Create an Azure function app that gets incoming messages from your IoT hub and writes them tooAzure Table storage.</span></span>

![Diagram začátku do konce Lekce 4](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson4.png)

### <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="6c434-146">Vytvořte aplikaci Azure funkce a účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="6c434-146">Create an Azure function app and Azure Storage account</span></span>
<span data-ttu-id="6c434-147">Aplikace Azure funkce a účet úložiště Azure, použijte toocreate šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6c434-147">Use an Azure Resource Manager template toocreate an Azure function app and an Azure Storage account.</span></span>

<span data-ttu-id="6c434-148">*Odhadovaný čas toocomplete: 10 minut*</span><span class="sxs-lookup"><span data-stu-id="6c434-148">*Estimated time toocomplete: 10 minutes*</span></span>

<span data-ttu-id="6c434-149">Přejděte příliš[vytvořte aplikaci Azure funkce a účet úložiště Azure](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md)</span><span class="sxs-lookup"><span data-stu-id="6c434-149">Go too[Create an Azure function app and Azure Storage account](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md)</span></span>

### <a name="read-messages-persisted-in-azure-table-storage"></a><span data-ttu-id="6c434-150">Čtení zprávy uchovávané v úložišti Azure Table</span><span class="sxs-lookup"><span data-stu-id="6c434-150">Read messages persisted in Azure Table storage</span></span>
<span data-ttu-id="6c434-151">Zprávy brány cloud hello sledovat, jak je, jak jsou napsány tooAzure Table storage.</span><span class="sxs-lookup"><span data-stu-id="6c434-151">Monitor hello gateway-to-cloud messages as they are written tooAzure Table storage.</span></span>

<span data-ttu-id="6c434-152">*Odhadovaný čas toocomplete: 5 minut*</span><span class="sxs-lookup"><span data-stu-id="6c434-152">*Estimated time toocomplete: 5 minutes*</span></span>

<span data-ttu-id="6c434-153">Přejděte příliš[číst zprávy uchovávané v úložišti Azure Table](iot-hub-gateway-kit-c-sim-lesson4-read-table-storage.md).</span><span class="sxs-lookup"><span data-stu-id="6c434-153">Go too[Read messages persisted in Azure Table storage](iot-hub-gateway-kit-c-sim-lesson4-read-table-storage.md).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="6c434-154">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="6c434-154">Troubleshooting</span></span>
<span data-ttu-id="6c434-155">Pokud máte potíže při hello lekce, vyhledejte řešení v hello [Poradce při potížích s](iot-hub-gateway-kit-c-sim-troubleshooting.md) článku.</span><span class="sxs-lookup"><span data-stu-id="6c434-155">If you have any problems during hello lessons, look for solutions in hello [Troubleshooting](iot-hub-gateway-kit-c-sim-troubleshooting.md) article.</span></span>

## <a name="explore-more"></a><span data-ttu-id="6c434-156">Prozkoumat další</span><span class="sxs-lookup"><span data-stu-id="6c434-156">Explore more</span></span>
<span data-ttu-id="6c434-157">Navštivte hello [zóny developer Kit brány IoT Intel](https://software.intel.com/en-us/iot/hardware/gateways/dev-kit) toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="6c434-157">Visit hello [Intel IoT Gateway Kit developer zone](https://software.intel.com/en-us/iot/hardware/gateways/dev-kit) toolearn more.</span></span>
