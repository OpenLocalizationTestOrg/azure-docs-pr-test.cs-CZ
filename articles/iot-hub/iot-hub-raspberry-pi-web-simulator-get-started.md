---
title: "Simulated malin pí do cloudu (Node.js) – simulátoru webové platformy malin připojit ke službě Azure IoT Hub | Microsoft Docs"
description: "Simulátor webové platformy malin připojte ke službě Azure IoT Hub malin pí k odesílání dat do cloudu Azure."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Simulátor Malinová pi, platformy azure iot Malinová, malinová platformy iot hub, malinová pí odesílání dat do cloudu, malinová pí do cloudu"
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/28/2017
ms.author: xshi
ms.openlocfilehash: 3b80bf35d6af91d5bdb196d97668dc0f837b92cc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="connect-raspberry-pi-online-simulator-to-azure-iot-hub-nodejs"></a><span data-ttu-id="d712a-104">Připojení k Azure IoT Hub (Node.js) online simulátoru malin platformy</span><span class="sxs-lookup"><span data-stu-id="d712a-104">Connect Raspberry Pi online simulator to Azure IoT Hub (Node.js)</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="d712a-105">V tomto kurzu zahájíte učení základní informace o práci s online simulátoru malin pí.</span><span class="sxs-lookup"><span data-stu-id="d712a-105">In this tutorial, you begin by learning the basics of working with Raspberry Pi online simulator.</span></span> <span data-ttu-id="d712a-106">Pak zjistíte, jak se bezproblémově připojit simulátoru pí do cloudu pomocí [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="d712a-106">You then learn how to seamlessly connect the Pi simulator to the cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span> 

<span data-ttu-id="d712a-107">Pokud máte fyzické zařízení, navštivte [pí malin připojit ke službě Azure IoT Hub](iot-hub-raspberry-pi-kit-node-get-started.md) začít pracovat.</span><span class="sxs-lookup"><span data-stu-id="d712a-107">If you have physical devices, visit [Connect Raspberry Pi to Azure IoT Hub](iot-hub-raspberry-pi-kit-node-get-started.md) to get started.</span></span> 

<p>
<div id="diag" style="width:100%; text-align:center">
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#getstarted" target="_blank">
<img src="media/iot-hub-raspberry-pi-web-simulator/3_banner.png" alt="Connect Raspberry Pi web simulator to Azure IoT Hub" width="400">
</div>
<p>
<div id="button" style="width:100%; text-align:center">
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#Getstarted" target="_blank">
<img src="media/iot-hub-raspberry-pi-web-simulator/6_button_default.png" alt="Start Raspberry Pi simulator" width="400" onmouseover="this.src='media/iot-hub-raspberry-pi-web-simulator/5_button_click.png';" onmouseout="this.src='media/iot-hub-raspberry-pi-web-simulator/6_button_default.png';">
</div>

## <a name="what-you-do"></a><span data-ttu-id="d712a-108">Co dělat</span><span class="sxs-lookup"><span data-stu-id="d712a-108">What you do</span></span>

* <span data-ttu-id="d712a-109">Přečtěte si základní informace o online simulátoru malin pí.</span><span class="sxs-lookup"><span data-stu-id="d712a-109">Learn the basics of Raspberry Pi online simulator.</span></span>
* <span data-ttu-id="d712a-110">Vytvoření služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d712a-110">Create an IoT hub.</span></span>
* <span data-ttu-id="d712a-111">Registrovat zařízení pro platformy ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d712a-111">Register a device for Pi in your IoT hub.</span></span>
* <span data-ttu-id="d712a-112">Spuštění ukázkové aplikace na platformy k odesílání dat simulované senzor do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d712a-112">Run a sample application on Pi to send simulated sensor data to your IoT hub.</span></span>

<span data-ttu-id="d712a-113">Simulované pí malin připojení do služby IoT hub, který vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="d712a-113">Connect simulated Raspberry Pi to an IoT hub that you create.</span></span> <span data-ttu-id="d712a-114">Pak spusťte ukázkovou aplikaci s simulátoru ke generování dat snímačů.</span><span class="sxs-lookup"><span data-stu-id="d712a-114">Then you run a sample application with the simulator to generate sensor data.</span></span> <span data-ttu-id="d712a-115">Nakonec odeslat data snímačů do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d712a-115">Finally, you send the sensor data to your IoT hub.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="d712a-116">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="d712a-116">What you learn</span></span>

* <span data-ttu-id="d712a-117">Postup vytvoření služby Azure IoT hub a získat nový připojovací řetězec zařízení.</span><span class="sxs-lookup"><span data-stu-id="d712a-117">How to create an Azure IoT hub and get your new device connection string.</span></span> <span data-ttu-id="d712a-118">Pokud nemáte účet Azure [vytvořit Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="d712a-118">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="d712a-119">Jak pracovat s online simulátoru malin pí.</span><span class="sxs-lookup"><span data-stu-id="d712a-119">How to work with Raspberry Pi online simulator.</span></span>
* <span data-ttu-id="d712a-120">Jak odesílat data snímačů do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d712a-120">How to send sensor data to your IoT hub.</span></span>

## <a name="overview-of-raspberry-pi-web-simulator"></a><span data-ttu-id="d712a-121">Přehled simulátoru webové platformy malin</span><span class="sxs-lookup"><span data-stu-id="d712a-121">Overview of Raspberry Pi web simulator</span></span>

<span data-ttu-id="d712a-122">Klikněte na tlačítko spustit online simulátoru malin pí.</span><span class="sxs-lookup"><span data-stu-id="d712a-122">Click the button to launch Raspberry Pi online simulator.</span></span>

> [!div class="button"]
<span data-ttu-id="d712a-123"><a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted" target="_blank">Spustit simulátoru Malinová platformy</a></span><span class="sxs-lookup"><span data-stu-id="d712a-123"><a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted" target="_blank">Start Raspberry Pi Simulator</a></span></span>

<span data-ttu-id="d712a-124">Existují tři oblasti v simulátoru web.</span><span class="sxs-lookup"><span data-stu-id="d712a-124">There are three areas in the web simulator.</span></span>
1. <span data-ttu-id="d712a-125">Sestavení oblast – výchozí okruh je, že na Pi připojí senzor BME280 a Indikátor.</span><span class="sxs-lookup"><span data-stu-id="d712a-125">Assembly area - The default circuit is that a Pi connects with a BME280 sensor and an LED.</span></span> <span data-ttu-id="d712a-126">Oblasti uzamčeno ve verzi preview proto nyní nemůže provádět přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="d712a-126">The area is locked in preview version so currently you cannot do customization.</span></span>
2. <span data-ttu-id="d712a-127">Kódování oblast – editor online kód pro vás kódu s použitím malin pí.</span><span class="sxs-lookup"><span data-stu-id="d712a-127">Coding area - An online code editor for you to code with Raspberry Pi.</span></span> <span data-ttu-id="d712a-128">Ukázkovou aplikaci výchozí pomáhá ke shromažďování dat snímačů z BME280 senzor a odesílá do služby Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d712a-128">The default sample application helps to collect sensor data from BME280 sensor and sends to your Azure IoT Hub.</span></span> <span data-ttu-id="d712a-129">Aplikace je plně kompatibilní se skutečné platformy zařízení.</span><span class="sxs-lookup"><span data-stu-id="d712a-129">The application is fully compatible with real Pi devices.</span></span> 
3. <span data-ttu-id="d712a-130">Okno integrovaného konzoly - zobrazuje výstup z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="d712a-130">Integrated console window - It shows the output of your code.</span></span> <span data-ttu-id="d712a-131">V horní části tohoto okna existují tři tlačítka.</span><span class="sxs-lookup"><span data-stu-id="d712a-131">At the top of this window, there are three buttons.</span></span>
   * <span data-ttu-id="d712a-132">**Spustit** -aplikaci spustit v oblasti kódování.</span><span class="sxs-lookup"><span data-stu-id="d712a-132">**Run** - Run the application in the coding area.</span></span>
   * <span data-ttu-id="d712a-133">**Resetování** -oblasti kódování resetovat na výchozí ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d712a-133">**Reset** - Reset the coding area to the default sample application.</span></span>
   * <span data-ttu-id="d712a-134">**Násobek/rozšířit** -na pravé straně je tlačítko pro vás násobek nebo rozbalte v okně konzoly.</span><span class="sxs-lookup"><span data-stu-id="d712a-134">**Fold/Expand** - On the right side there is a button for you to fold/expand the console window.</span></span>

> [!NOTE] 
<span data-ttu-id="d712a-135">Simulátor webové platformy malin je teď dostupná ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="d712a-135">The Raspberry Pi web simulator is now available in preview version.</span></span> <span data-ttu-id="d712a-136">Rádi bychom se slyšet váš hlas v [Gitter Chatroom](https://gitter.im/Microsoft/raspberry-pi-web-simulator).</span><span class="sxs-lookup"><span data-stu-id="d712a-136">We'd like to hear your voice in the [Gitter Chatroom](https://gitter.im/Microsoft/raspberry-pi-web-simulator).</span></span> <span data-ttu-id="d712a-137">Zdrojový kód je veřejný na [Githubu](https://github.com/Azure-Samples/raspberry-pi-web-simulator).</span><span class="sxs-lookup"><span data-stu-id="d712a-137">The source code is public on [Github](https://github.com/Azure-Samples/raspberry-pi-web-simulator).</span></span>

![Přehled platformy online simulátoru](media/iot-hub-raspberry-pi-web-simulator/0_overview.png)

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]


## <a name="run-a-sample-application-on-pi-web-simulator"></a><span data-ttu-id="d712a-139">Spouštět ukázkovou aplikaci v simulátoru webové platformy</span><span class="sxs-lookup"><span data-stu-id="d712a-139">Run a sample application on Pi web simulator</span></span>

1. <span data-ttu-id="d712a-140">V oblasti kódování, zkontrolujte, zda že pracujete na výchozí ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d712a-140">In coding area, make sure you are working on the default sample application.</span></span> <span data-ttu-id="d712a-141">Nahraďte zástupný symbol v řádku 15 připojovací řetězec pro zařízení Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d712a-141">Replace the placeholder in Line 15 with the Azure IoT hub device connection string.</span></span>
   <span data-ttu-id="d712a-142">![Nahraďte řetězec připojení zařízení](media/iot-hub-raspberry-pi-web-simulator/1_connectionstring.png)</span><span class="sxs-lookup"><span data-stu-id="d712a-142">![Replace the device connection string](media/iot-hub-raspberry-pi-web-simulator/1_connectionstring.png)</span></span>

2. <span data-ttu-id="d712a-143">Klikněte na tlačítko **spustit** nebo typ `npm start` ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="d712a-143">Click **Run** or type `npm start` to run the application.</span></span>


<span data-ttu-id="d712a-144">Byste měli vidět následující výstup, který popisuje data snímačů a zprávy, které se odesílají do služby IoT hub ![výstup - data snímačů odeslaný malin pí do služby IoT hub](media/iot-hub-raspberry-pi-web-simulator/2_run_application.png)</span><span class="sxs-lookup"><span data-stu-id="d712a-144">You should see the following output that shows the sensor data and the messages that are sent to your IoT hub ![Output - sensor data sent from Raspberry Pi to your IoT hub](media/iot-hub-raspberry-pi-web-simulator/2_run_application.png)</span></span>


## <a name="next-steps"></a><span data-ttu-id="d712a-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d712a-145">Next steps</span></span>

<span data-ttu-id="d712a-146">Spustíte ukázkovou aplikaci pro shromažďování dat snímačů a odeslat do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d712a-146">You’ve run a sample application to collect sensor data and send it to your IoT hub.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
