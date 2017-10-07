---
title: "Zařízení SensorTag & brány Azure IoT - Začínáme | Microsoft Docs"
description: "Začínáme s IoT brány Starter Kit, vytvoření služby Azure IoT hub a připojit SensorTag a brány toohello IoT hub"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Centrum Azure iot hub iot brány, Začínáme se službou hello internet věcí, iot toolkit"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 56d05f4e-f2c1-4b22-8701-f01e14deead6
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: d8e4057e7774e43c069dd3f2f2e03f098c1ac844
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-iot-gateway-starter-kit-with-a-sensortag"></a><span data-ttu-id="cd45e-104">Začínáme s IoT brány Starter Kit s SensorTag</span><span class="sxs-lookup"><span data-stu-id="cd45e-104">Get started with IoT Gateway Starter Kit with a SensorTag</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="cd45e-105">SensorTag</span><span class="sxs-lookup"><span data-stu-id="cd45e-105">SensorTag</span></span>](iot-hub-gateway-kit-c-get-started.md)
> * [<span data-ttu-id="cd45e-106">Simulované zařízení</span><span class="sxs-lookup"><span data-stu-id="cd45e-106">Simulated Device</span></span>](iot-hub-gateway-kit-c-sim-get-started.md)

<span data-ttu-id="cd45e-107">V tomto kurzu začnete podle učení hello základy práce s [IoT brány Starter Kit](https://aka.ms/gateway-kit).</span><span class="sxs-lookup"><span data-stu-id="cd45e-107">In this tutorial, you begin by learning hello basics of working with [IoT Gateway Starter Kit](https://aka.ms/gateway-kit).</span></span> <span data-ttu-id="cd45e-108">Budete pracovat s NUC Intel, se systémem Linux větru oblasti a hello [TI SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html#main).</span><span class="sxs-lookup"><span data-stu-id="cd45e-108">You will be working with Intel NUC that's running Wind River Linux and hello [TI SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html#main).</span></span> <span data-ttu-id="cd45e-109">Se dozvíte, jak tooseamleesly propojit své cloudové toohello zařízení pomocí služby Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="cd45e-109">You will learn how tooseamleesly connect your devices toohello cloud by using Azure IoT Hub.</span></span>

***
<span data-ttu-id="cd45e-110">**Ještě není hotová kit?:** klikněte na tlačítko [zde](https://aka.ms/gateway-kit).</span><span class="sxs-lookup"><span data-stu-id="cd45e-110">**Don't have a kit yet?:** Click [here](https://aka.ms/gateway-kit).</span></span> <span data-ttu-id="cd45e-111">**Není k dispozici SensorTag?:** [začínat simulované zařízení](iot-hub-gateway-kit-c-sim-get-started.md) nebo [koupit SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/?INTC=SensorTag&HQS=sensortag)</span><span class="sxs-lookup"><span data-stu-id="cd45e-111">**Don't have a SensorTag?:** [Start with a simulated device](iot-hub-gateway-kit-c-sim-get-started.md) or [buy a SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/?INTC=SensorTag&HQS=sensortag)</span></span>
***

## <a name="lesson-1-configure-your-nuc"></a><span data-ttu-id="cd45e-112">Lekce 1: Konfigurace NUC</span><span class="sxs-lookup"><span data-stu-id="cd45e-112">Lesson 1: Configure your NUC</span></span>
![Diagram začátku do konce Lesson1](media/iot-hub-gateway-kit-lessons/e2e-lesson1.png)

<span data-ttu-id="cd45e-114">V této lekci nastavit Intel NUC (Další jednotky Computing) v hello Kit jako bránu Azure IoT, nainstalovat balíček Azure IoT Edge hello na NUC a spusťte funkci brány tooverify hello ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="cd45e-114">In this lesson, you set up Intel NUC (Next Unit of Computing) in hello Kit as an Azure IoT gateway, install hello Azure IoT Edge package on NUC, and run a sample app tooverify hello gateway functionality.</span></span>

<span data-ttu-id="cd45e-115">*Odhadovaný čas toocomplete: 15 minut*</span><span class="sxs-lookup"><span data-stu-id="cd45e-115">*Estimated time toocomplete: 15 minutes*</span></span>

<span data-ttu-id="cd45e-116">Přejděte příliš[nastavit Intel NUC jako bránu IoT](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)</span><span class="sxs-lookup"><span data-stu-id="cd45e-116">Go too[Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)</span></span>

## <a name="lesson-2-create-your-iot-hub"></a><span data-ttu-id="cd45e-117">Lekce 2: Vytvoření služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="cd45e-117">Lesson 2: Create your IoT Hub</span></span>
![Diagram začátku do konce Lesson2](media/iot-hub-gateway-kit-lessons/e2e-lesson2.png)

<span data-ttu-id="cd45e-119">V této lekci nainstalovat nástroje hello a software na hostitelském počítači.</span><span class="sxs-lookup"><span data-stu-id="cd45e-119">In this lesson, you install hello tools and software on your host computer.</span></span> <span data-ttu-id="cd45e-120">Pak vytvoříte účet Azure zdarma, zřídit služby Azure IoT hub a vytvořit první zařízení IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="cd45e-120">Then you create your free Azure account, provision your Azure IoT hub and create your first device in hello IoT hub.</span></span>

<span data-ttu-id="cd45e-121">Před zahájením této lekci, proveďte Lekce 1.</span><span class="sxs-lookup"><span data-stu-id="cd45e-121">Complete Lesson 1 before you start this lesson.</span></span>

### <a name="get-hello-tools"></a><span data-ttu-id="cd45e-122">Získat nástroje hello</span><span class="sxs-lookup"><span data-stu-id="cd45e-122">Get hello tools</span></span>
<span data-ttu-id="cd45e-123">Nainstalujte nástroje hello a software v hostitelském počítači.</span><span class="sxs-lookup"><span data-stu-id="cd45e-123">Install hello tools and software on your host computer.</span></span>

<span data-ttu-id="cd45e-124">*Odhadovaný čas toocomplete: 20 minut*</span><span class="sxs-lookup"><span data-stu-id="cd45e-124">*Estimated time toocomplete: 20 minutes*</span></span>

<span data-ttu-id="cd45e-125">Přejděte příliš[získat nástroje hello](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)</span><span class="sxs-lookup"><span data-stu-id="cd45e-125">Go too[Get hello tools](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)</span></span>

### <a name="create-an-iot-hub-and-register-your-device"></a><span data-ttu-id="cd45e-126">Vytvoření služby IoT hub a registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="cd45e-126">Create an IoT hub and register your device</span></span>
<span data-ttu-id="cd45e-127">Vytvoření skupiny prostředků, zřídit první Azure IoT hub a přidejte první zařízení toohello IoT hub pomocí hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="cd45e-127">Create your resource group, provision your first Azure IoT hub, and add your first device toohello IoT hub using hello Azure CLI.</span></span>

<span data-ttu-id="cd45e-128">*Odhadovaný čas toocomplete: 10 minut*</span><span class="sxs-lookup"><span data-stu-id="cd45e-128">*Estimated time toocomplete: 10 minutes*</span></span>

<span data-ttu-id="cd45e-129">Přejděte příliš[vytvoření služby IoT hub a registrace zařízení](iot-hub-gateway-kit-c-lesson2-register-device.md)</span><span class="sxs-lookup"><span data-stu-id="cd45e-129">Go too[Create an IoT hub and register your device](iot-hub-gateway-kit-c-lesson2-register-device.md)</span></span>

## <a name="lesson-3-receive-messages-from-sensortag-and-read-messages-from-your-iot-hub"></a><span data-ttu-id="cd45e-130">Lekce 3: Přijetí zpráv z SensorTag a čtení zpráv z centra IoT</span><span class="sxs-lookup"><span data-stu-id="cd45e-130">Lesson 3: Receive messages from SensorTag and read messages from your IoT hub</span></span>
<span data-ttu-id="cd45e-131">V této lekci použijete skripty tooautomate hello konfiguraci a spuštění ukázkové aplikace zakázat v bránu.</span><span class="sxs-lookup"><span data-stu-id="cd45e-131">In this lesson, you will use scripts tooautomate hello configuration and execution of a BLE sample application in your gateway.</span></span> <span data-ttu-id="cd45e-132">Tyto aplikace použít kolekci s moduly tooaggregate a transformace dat, zpracování příkazů nebo proveďte libovolný počet souvisejících úloh.</span><span class="sxs-lookup"><span data-stu-id="cd45e-132">Such applications use a collection of modules tooaggregate and transform data, process commands, or perform any number of related tasks.</span></span> <span data-ttu-id="cd45e-133">Moduly vzájemnou komunikaci přes zprostředkovatele zpráv.</span><span class="sxs-lookup"><span data-stu-id="cd45e-133">Modules communicate with one another via a message broker.</span></span> <span data-ttu-id="cd45e-134">Ukázková aplikace Hello má modul zakázat a modul služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="cd45e-134">hello sample application has a BLE module and an IoT hub module.</span></span> <span data-ttu-id="cd45e-135">Hello zakázat modul přijímá data z SensorTag zakázat.</span><span class="sxs-lookup"><span data-stu-id="cd45e-135">hello BLE module receives data from BLE SensorTag.</span></span> <span data-ttu-id="cd45e-136">Hello IoT hub modulu balíčky hello dat přijatých a odešle ji tooyour IoT hub prostřednictvím hello brány rámec poskytovaný v Azure IoT Edge.</span><span class="sxs-lookup"><span data-stu-id="cd45e-136">hello IoT hub module packages hello data received and sends it tooyour IoT hub through hello gateway framework provided in Azure IoT Edge.</span></span>

![Diagram začátku do konce Lekce 3](media/iot-hub-gateway-kit-lessons/e2e-lesson3.png)

### <a name="configure-and-run-hello-ble-sample-app"></a><span data-ttu-id="cd45e-138">Nakonfigurujte a spusťte hello zakázat ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="cd45e-138">Configure and run hello BLE sample app</span></span>
<span data-ttu-id="cd45e-139">Nastavte hello propojení mezi SensorTag a bránu.</span><span class="sxs-lookup"><span data-stu-id="cd45e-139">Set up hello connectivity between SensorTag and your gateway.</span></span> <span data-ttu-id="cd45e-140">Pak dokončit konfiguraci hello a spusťte hello zakázat ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cd45e-140">Then finish hello configuration and run hello BLE sample application.</span></span>

<span data-ttu-id="cd45e-141">*Odhadovaný čas toocomplete: 15 minut*</span><span class="sxs-lookup"><span data-stu-id="cd45e-141">*Estimated time toocomplete: 15 minutes*</span></span>

<span data-ttu-id="cd45e-142">Přejděte příliš[konfigurace a spuštění hello zakázat ukázková aplikace](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)</span><span class="sxs-lookup"><span data-stu-id="cd45e-142">Go too[Configure and run hello BLE sample app](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)</span></span>

### <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="cd45e-143">Čtení zpráv z centra IoT</span><span class="sxs-lookup"><span data-stu-id="cd45e-143">Read messages from your IoT hub</span></span>
<span data-ttu-id="cd45e-144">Spusťte ukázkový kód na hostiteli zprávy tooread počítače ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="cd45e-144">Run sample code on your host computer tooread messages from your IoT hub.</span></span>

<span data-ttu-id="cd45e-145">*Odhadovaný čas toocomplete: 15 minut*</span><span class="sxs-lookup"><span data-stu-id="cd45e-145">*Estimated time toocomplete: 15 minutes*</span></span>

<span data-ttu-id="cd45e-146">Přejděte příliš[čtení zpráv z centra IoT](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)</span><span class="sxs-lookup"><span data-stu-id="cd45e-146">Go too[Read messages from your IoT hub](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)</span></span>

## <a name="lesson-4-save-messages-tooazure-table-storage"></a><span data-ttu-id="cd45e-147">Lekce 4: Uložení zpráv tooAzure Table storage</span><span class="sxs-lookup"><span data-stu-id="cd45e-147">Lesson 4: Save messages tooAzure Table storage</span></span>
<span data-ttu-id="cd45e-148">Vytvořte aplikaci Azure funkce, která se získá příchozích zpráv ze služby IoT hub a zapíše tooAzure tabulka úložiště.</span><span class="sxs-lookup"><span data-stu-id="cd45e-148">Create an Azure function app that gets incoming messages from your IoT hub and writes them tooAzure Table storage.</span></span>

![Diagram začátku do konce Lekce 4](media/iot-hub-gateway-kit-lessons/e2e-lesson4.png)

### <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="cd45e-150">Vytvořte aplikaci Azure funkce a účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="cd45e-150">Create an Azure function app and Azure Storage account</span></span>
<span data-ttu-id="cd45e-151">Aplikace Azure funkce a účet úložiště Azure, použijte toocreate šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="cd45e-151">Use an Azure Resource Manager template toocreate an Azure function app and an Azure Storage account.</span></span>

<span data-ttu-id="cd45e-152">*Odhadovaný čas toocomplete: 10 minut*</span><span class="sxs-lookup"><span data-stu-id="cd45e-152">*Estimated time toocomplete: 10 minutes*</span></span>

<span data-ttu-id="cd45e-153">Přejděte příliš[vytvořte aplikaci Azure funkce a účet úložiště Azure](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md)</span><span class="sxs-lookup"><span data-stu-id="cd45e-153">Go too[Create an Azure function app and Azure Storage account](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md)</span></span>

### <a name="read-messages-persisted-in-azure-table-storage"></a><span data-ttu-id="cd45e-154">Čtení zprávy uchovávané v úložišti Azure Table</span><span class="sxs-lookup"><span data-stu-id="cd45e-154">Read messages persisted in Azure Table storage</span></span>
<span data-ttu-id="cd45e-155">Zprávy brány cloud hello sledovat, jak je, jak jsou napsány tooAzure Table storage.</span><span class="sxs-lookup"><span data-stu-id="cd45e-155">Monitor hello gateway-to-cloud messages as they are written tooAzure Table storage.</span></span>

<span data-ttu-id="cd45e-156">*Odhadovaný čas toocomplete: 5 minut*</span><span class="sxs-lookup"><span data-stu-id="cd45e-156">*Estimated time toocomplete: 5 minutes*</span></span>

<span data-ttu-id="cd45e-157">Přejděte příliš[číst zprávy uchovávané v úložišti Azure Table](iot-hub-gateway-kit-c-lesson4-read-table-storage.md).</span><span class="sxs-lookup"><span data-stu-id="cd45e-157">Go too[Read messages persisted in Azure Table storage](iot-hub-gateway-kit-c-lesson4-read-table-storage.md).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="cd45e-158">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="cd45e-158">Troubleshooting</span></span>
<span data-ttu-id="cd45e-159">Pokud máte potíže při hello lekce, vyhledejte řešení v hello [Poradce při potížích s](iot-hub-gateway-kit-c-troubleshooting.md) článku.</span><span class="sxs-lookup"><span data-stu-id="cd45e-159">If you have any problems during hello lessons, look for solutions in hello [Troubleshooting](iot-hub-gateway-kit-c-troubleshooting.md) article.</span></span>

## <a name="explore-more"></a><span data-ttu-id="cd45e-160">Prozkoumat další</span><span class="sxs-lookup"><span data-stu-id="cd45e-160">Explore more</span></span>
<span data-ttu-id="cd45e-161">Navštivte hello [zóny developer Kit brány IoT Intel](http://software.intel.com/iot/microsoft-azure) toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="cd45e-161">Visit hello [Intel IoT Gateway Kit developer zone](http://software.intel.com/iot/microsoft-azure) toolearn more.</span></span>
