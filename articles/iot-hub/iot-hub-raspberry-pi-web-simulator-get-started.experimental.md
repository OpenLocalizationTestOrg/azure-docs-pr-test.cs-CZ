---
title: "aaaSimulated malin pí toocloud (Node.js) – tooAzure simulátoru webové připojení malin platformy IoT Hub | Microsoft Docs"
description: "Připojte malin pí webové simulátoru tooAzure IoT Hub pro platformy malin toosend data toohello cloudu Azure."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Malinová pí simulátoru, azure Malinová pi, malinová platformy iot Centrum iot, malinová pí odesílat data toocloud Malinová pí toocloud"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/7/2017
ms.author: xshi
ms.openlocfilehash: 0ebe1004e96ff4e64fdd1b05b7e35192ec42f7d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-raspberry-pi-online-simulator-tooazure-iot-hub-nodejs"></a><span data-ttu-id="8a07d-104">Připojit tooAzure online simulátoru malin platformy IoT Hub (Node.js)</span><span class="sxs-lookup"><span data-stu-id="8a07d-104">Connect Raspberry Pi online simulator tooAzure IoT Hub (Node.js)</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="8a07d-105">V tomto kurzu zahájíte učení hello základy práce s online simulátoru malin pí.</span><span class="sxs-lookup"><span data-stu-id="8a07d-105">In this tutorial, you begin by learning hello basics of working with Raspberry Pi online simulator.</span></span> <span data-ttu-id="8a07d-106">Pak zjistíte, jak připojit tooseamlessly hello pí simulátoru toohello cloudu pomocí [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="8a07d-106">You then learn how tooseamlessly connect hello Pi simulator toohello cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span> 

<span data-ttu-id="8a07d-107">Pokud máte fyzické zařízení, navštivte [tooAzure připojit malin platformy IoT Hub](iot-hub-raspberry-pi-kit-node-get-started.md) tooget spuštěna.</span><span class="sxs-lookup"><span data-stu-id="8a07d-107">If you have physical devices, visit [Connect Raspberry Pi tooAzure IoT Hub](iot-hub-raspberry-pi-kit-node-get-started.md) tooget started.</span></span> 

<p>
<div id="diag" style="width:100%; text-align:center">
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#getstarted">
<img src="media/iot-hub-raspberry-pi-web-simulator/3_banner.png" alt="Connect Raspberry Pi web simulator tooAzure IoT Hub" width="400">
</div>
<p>
<div id="button" style="width:100%; text-align:center">
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#Getstarted">
<img src="media/iot-hub-raspberry-pi-web-simulator/6_button_default.png" alt="Start Raspberry Pi simulator" width="400" onmouseover="this.src='media/iot-hub-raspberry-pi-web-simulator/5_button_click.png';" onmouseout="this.src='media/iot-hub-raspberry-pi-web-simulator/6_button_default.png';">
</div>

## <a name="what-you-do"></a><span data-ttu-id="8a07d-108">Co dělat</span><span class="sxs-lookup"><span data-stu-id="8a07d-108">What you do</span></span>

* <span data-ttu-id="8a07d-109">Přečtěte si základy hello online simulátoru malin pí.</span><span class="sxs-lookup"><span data-stu-id="8a07d-109">Learn hello basics of Raspberry Pi online simulator.</span></span>
* <span data-ttu-id="8a07d-110">Vytvoření služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="8a07d-110">Create an IoT hub.</span></span>
* <span data-ttu-id="8a07d-111">Registrovat zařízení pro platformy ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="8a07d-111">Register a device for Pi in your IoT hub.</span></span>
* <span data-ttu-id="8a07d-112">Spuštění ukázkové aplikace na platformy toosend simulated senzor data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="8a07d-112">Run a sample application on Pi toosend simulated sensor data tooyour IoT hub.</span></span>

<span data-ttu-id="8a07d-113">Připojte simulované malin pí tooan IoT hub, který vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="8a07d-113">Connect simulated Raspberry Pi tooan IoT hub that you create.</span></span> <span data-ttu-id="8a07d-114">Pak spusťte ukázkovou aplikaci s dat snímačů toogenerate simulátoru hello.</span><span class="sxs-lookup"><span data-stu-id="8a07d-114">Then you run a sample application with hello simulator toogenerate sensor data.</span></span> <span data-ttu-id="8a07d-115">Nakonec odeslat hello senzor data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="8a07d-115">Finally, you send hello sensor data tooyour IoT hub.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="8a07d-116">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="8a07d-116">What you learn</span></span>

* <span data-ttu-id="8a07d-117">Jak toocreate služby Azure IoT hub a získat nový připojovací řetězec zařízení.</span><span class="sxs-lookup"><span data-stu-id="8a07d-117">How toocreate an Azure IoT hub and get your new device connection string.</span></span> <span data-ttu-id="8a07d-118">Pokud nemáte účet Azure [vytvořit Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="8a07d-118">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="8a07d-119">Jak toowork s online simulátoru malin pí.</span><span class="sxs-lookup"><span data-stu-id="8a07d-119">How toowork with Raspberry Pi online simulator.</span></span>
* <span data-ttu-id="8a07d-120">Jak toosend senzor data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="8a07d-120">How toosend sensor data tooyour IoT hub.</span></span>

## <a name="overview-of-raspberry-pi-web-simulator"></a><span data-ttu-id="8a07d-121">Přehled simulátoru webové platformy malin</span><span class="sxs-lookup"><span data-stu-id="8a07d-121">Overview of Raspberry Pi web simulator</span></span>

<span data-ttu-id="8a07d-122">Klikněte na tlačítko hello tlačítko toolaunch malin pí online simulátoru.</span><span class="sxs-lookup"><span data-stu-id="8a07d-122">Click hello button toolaunch Raspberry Pi online simulator.</span></span>

> [!div class="button"]
[<span data-ttu-id="8a07d-123">Spustit simulátoru malin platformy</span><span class="sxs-lookup"><span data-stu-id="8a07d-123">Start Raspberry Pi simulator</span></span>](https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted)

<span data-ttu-id="8a07d-124">Existují tři oblasti v simulátoru webové hello.</span><span class="sxs-lookup"><span data-stu-id="8a07d-124">There are three areas in hello web simulator.</span></span>
* <span data-ttu-id="8a07d-125">Sestavení oblast – hello výchozí okruh je, že na Pi připojí senzor BME280 a Indikátor.</span><span class="sxs-lookup"><span data-stu-id="8a07d-125">Assembly area - hello default circuit is that a Pi connects with a BME280 sensor and an LED.</span></span> <span data-ttu-id="8a07d-126">Hello oblasti uzamčeno ve verzi preview proto nyní nemůže provádět přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="8a07d-126">hello area is locked in preview version so currently you cannot do customization.</span></span>
* <span data-ttu-id="8a07d-127">Kódování oblast – editor online kód pro toocode s malin pí.</span><span class="sxs-lookup"><span data-stu-id="8a07d-127">Coding area - An online code editor for you toocode with Raspberry Pi.</span></span> <span data-ttu-id="8a07d-128">Hello výchozí ukázkovou aplikaci pomáhá toocollect data snímačů z BME280 senzor a odešle tooyour Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8a07d-128">hello default sample application helps toocollect sensor data from BME280 sensor and sends tooyour Azure IoT Hub.</span></span> <span data-ttu-id="8a07d-129">aplikace Hello je plně kompatibilní se skutečné platformy zařízení.</span><span class="sxs-lookup"><span data-stu-id="8a07d-129">hello application is fully compatible with real Pi devices.</span></span> 
* <span data-ttu-id="8a07d-130">Okno integrovaného konzoly - zobrazuje výstup hello kódu.</span><span class="sxs-lookup"><span data-stu-id="8a07d-130">Integrated console window - It shows hello output of your code.</span></span> <span data-ttu-id="8a07d-131">Hello horní části tohoto okna existují tři tlačítka.</span><span class="sxs-lookup"><span data-stu-id="8a07d-131">At hello top of this window, there are three buttons.</span></span>
   * <span data-ttu-id="8a07d-132">**Spustit** -hello aplikaci spustit v hello kódování oblasti.</span><span class="sxs-lookup"><span data-stu-id="8a07d-132">**Run** - Run hello application in hello coding area.</span></span>
   * <span data-ttu-id="8a07d-133">**Resetování** -resetování hello kódování oblasti toohello výchozí ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8a07d-133">**Reset** - Reset hello coding area toohello default sample application.</span></span>
   * <span data-ttu-id="8a07d-134">**Násobek/rozšířit** – na pravé straně existuje je tlačítko pro můžete rozšířit nebo toofold hello okna konzoly hello.</span><span class="sxs-lookup"><span data-stu-id="8a07d-134">**Fold/Expand** - On hello right side there is a button for you toofold/expand hello console window.</span></span>

> [!NOTE] 
<span data-ttu-id="8a07d-135">Simulátor webové platformy malin Hello je teď dostupná ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="8a07d-135">hello Raspberry Pi web simulator is now available in preview version.</span></span> <span data-ttu-id="8a07d-136">Rádi bychom znali toohear váš hlas v hello [Gitter Chatroom](https://gitter.im/Microsoft/raspberry-pi-web-simulator).</span><span class="sxs-lookup"><span data-stu-id="8a07d-136">We'd like toohear your voice in hello [Gitter Chatroom](https://gitter.im/Microsoft/raspberry-pi-web-simulator).</span></span> <span data-ttu-id="8a07d-137">Hello zdrojový kód je veřejný na [Githubu](https://github.com/Azure-Samples/raspberry-pi-web-simulator).</span><span class="sxs-lookup"><span data-stu-id="8a07d-137">hello source code is public on [Github](https://github.com/Azure-Samples/raspberry-pi-web-simulator).</span></span>

![Přehled platformy online simulátoru](media/iot-hub-raspberry-pi-web-simulator/0_overview.png)

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]


## <a name="run-a-sample-application-on-pi-web-simulator"></a><span data-ttu-id="8a07d-139">Spouštět ukázkovou aplikaci v simulátoru webové platformy</span><span class="sxs-lookup"><span data-stu-id="8a07d-139">Run a sample application on Pi web simulator</span></span>

1. <span data-ttu-id="8a07d-140">V oblasti kódování, zkontrolujte, zda že pracujete na hello výchozí ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8a07d-140">In coding area, make sure you are working on hello default sample application.</span></span> <span data-ttu-id="8a07d-141">Nahraďte zástupný symbol hello v řádku 15 hello Azure IoT hub zařízení připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="8a07d-141">Replace hello placeholder in Line 15 with hello Azure IoT hub device connection string.</span></span>
   <span data-ttu-id="8a07d-142">![Nahraďte řetězec připojení zařízení hello](media/iot-hub-raspberry-pi-web-simulator/1_connectionstring.png)</span><span class="sxs-lookup"><span data-stu-id="8a07d-142">![Replace hello device connection string](media/iot-hub-raspberry-pi-web-simulator/1_connectionstring.png)</span></span>

2. <span data-ttu-id="8a07d-143">Klikněte na tlačítko **spustit** nebo typ `npm start` toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a07d-143">Click **Run** or type `npm start` toorun hello application.</span></span>


<span data-ttu-id="8a07d-144">Měli byste vidět hello následující výstup, který zobrazuje data snímačů hello a hello zpráv, které jsou odeslány tooyour IoT hub ![výstup - senzor dat odesílaných ze služby IoT hub malin pí tooyour](media/iot-hub-raspberry-pi-web-simulator/2_run_application.png)</span><span class="sxs-lookup"><span data-stu-id="8a07d-144">You should see hello following output that shows hello sensor data and hello messages that are sent tooyour IoT hub ![Output - sensor data sent from Raspberry Pi tooyour IoT hub](media/iot-hub-raspberry-pi-web-simulator/2_run_application.png)</span></span>


## <a name="next-steps"></a><span data-ttu-id="8a07d-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8a07d-145">Next steps</span></span>

<span data-ttu-id="8a07d-146">Spustil jsem ukázková data snímačů toocollect aplikace a odešlete ji tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="8a07d-146">You’ve run a sample application toocollect sensor data and send it tooyour IoT hub.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
