---
title: "Azure IoT Hub - začít pracovat s připojením zařízení IoT ke cloudu | Microsoft Docs"
description: "Zjistěte, jak se připojit ke službě Azure IoT Hub panely IoT a Startovní sady. Zařízení může odesílat telemetrická data do služby IoT Hub a IoT Hub můžete sledovat a spravovat vaše zařízení."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
keywords: Azure iot hub kurzu
ms.assetid: 24376318-5344-4a81-a1e6-0003ed587d53
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: dobett
ms.openlocfilehash: 45016e6383761ffe78f13ccef1112ab3d9753498
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-iot-hub-get-started-tutorials"></a><span data-ttu-id="1c2f4-105">Azure IoT Hub Začínáme kurzy</span><span class="sxs-lookup"><span data-stu-id="1c2f4-105">Azure IoT Hub get started tutorials</span></span>

<span data-ttu-id="1c2f4-106">Azure IoT Hub a sady SDK zařízení Azure IoT můžete použít k vytvoření řešení Internetu věcí (IoT):</span><span class="sxs-lookup"><span data-stu-id="1c2f4-106">You can use Azure IoT Hub and the Azure IoT device SDKs to build Internet of Things (IoT) solutions:</span></span>

* <span data-ttu-id="1c2f4-107">Azure IoT Hub je plně spravovaná služba v cloudu, které bezpečně připojit, monitoruje a spravuje zařízení IoT.</span><span class="sxs-lookup"><span data-stu-id="1c2f4-107">Azure IoT Hub is a fully managed service in the cloud that securely connects, monitors, and manages your IoT devices.</span></span> <span data-ttu-id="1c2f4-108">Implementace zařízení IoT pomocí sady SDK zařízení IoT v Azure.</span><span class="sxs-lookup"><span data-stu-id="1c2f4-108">Use the Azure IoT Device SDKs to implement your IoT devices.</span></span>
* <span data-ttu-id="1c2f4-109">Používejte bránu IoT v složitější scénáře IoT.</span><span class="sxs-lookup"><span data-stu-id="1c2f4-109">Use an IoT gateway in more complex IoT scenarios.</span></span> <span data-ttu-id="1c2f4-110">Například, kdy potřebujete vezměte v úvahu faktory, jako jsou zařízení se starší verzí, náklady na šířku pásma, zásady zabezpečení a ochrana osobních údajů nebo zpracování dat okraj.</span><span class="sxs-lookup"><span data-stu-id="1c2f4-110">For example, where you need to consider factors such as legacy devices, bandwidth costs, security and privacy policies, or edge data processing.</span></span> <span data-ttu-id="1c2f4-111">V těchto scénářích použijete k implementaci bránu, která se připojuje zařízení do služby IoT hub Azure IoT okraj.</span><span class="sxs-lookup"><span data-stu-id="1c2f4-111">In these scenarios, you use Azure IoT Edge to implement a gateway that connects devices to your IoT hub.</span></span>

## <a name="what-the-tutorials-cover"></a><span data-ttu-id="1c2f4-112">Co kurzy zahrnují</span><span class="sxs-lookup"><span data-stu-id="1c2f4-112">What the tutorials cover</span></span>

<span data-ttu-id="1c2f4-113">Tyto kurzy vám představí Azure IoT Hub a sady SDK pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="1c2f4-113">These tutorials introduce you to Azure IoT Hub and the device SDKs.</span></span> <span data-ttu-id="1c2f4-114">Kurzů k zahrnují běžné scénáře IoT k předvedení funkcí služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="1c2f4-114">The tutorials cover common IoT scenarios to demonstrate the capabilities of IoT Hub.</span></span> <span data-ttu-id="1c2f4-115">Kurzů k také znázorňují, jak kombinovat s jinými službami Azure a nástroje pro vytváření výkonnější IoT řešení IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="1c2f4-115">The tutorials also illustrate how to combine IoT Hub with other Azure services and tools to build more powerful IoT solutions.</span></span> <span data-ttu-id="1c2f4-116">V kurzech můžete použít simulované nebo skutečné zařízení IoT.</span><span class="sxs-lookup"><span data-stu-id="1c2f4-116">In the tutorials, you can choose to use either simulated or real IoT devices.</span></span> <span data-ttu-id="1c2f4-117">Kromě toho můžete další informace o použití brány postup pro povolení zařízením pro připojení do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="1c2f4-117">In addition, you can learn how to use a gateway to enable devices to connect to your IoT hub.</span></span>

## <a name="set-up-your-device"></a><span data-ttu-id="1c2f4-118">Nastavení zařízení</span><span class="sxs-lookup"><span data-stu-id="1c2f4-118">Set up your device</span></span>

<span data-ttu-id="1c2f4-119">Připojte zařízení IoT nebo brány Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="1c2f4-119">Connect an IoT device or gateway to Azure IoT Hub.</span></span> <span data-ttu-id="1c2f4-120">Můžete vybrat fyzické nebo simulované zařízení začít:</span><span class="sxs-lookup"><span data-stu-id="1c2f4-120">You can choose a physical or simulated device to get started:</span></span>

| <span data-ttu-id="1c2f4-121">Zařízení IoT</span><span class="sxs-lookup"><span data-stu-id="1c2f4-121">IoT device</span></span>                       | <span data-ttu-id="1c2f4-122">Programovací jazyk</span><span class="sxs-lookup"><span data-stu-id="1c2f4-122">Programming language</span></span> |
|----------------------------------|----------------------|
| <span data-ttu-id="1c2f4-123">Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="1c2f4-123">Raspberry Pi</span></span>                     | <span data-ttu-id="1c2f4-124">[Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]</span><span class="sxs-lookup"><span data-stu-id="1c2f4-124">[Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]</span></span>  |
| <span data-ttu-id="1c2f4-125">IoT DevKit</span><span class="sxs-lookup"><span data-stu-id="1c2f4-125">IoT DevKit</span></span>                       | <span data-ttu-id="1c2f4-126">[Arduino v VSCode][DevKit]</span><span class="sxs-lookup"><span data-stu-id="1c2f4-126">[Arduino in VSCode][DevKit]</span></span>     |
| <span data-ttu-id="1c2f4-127">Intel Edison</span><span class="sxs-lookup"><span data-stu-id="1c2f4-127">Intel Edison</span></span>                     | <span data-ttu-id="1c2f4-128">[Node.js][Ed_Nd], [C][Ed_C]</span><span class="sxs-lookup"><span data-stu-id="1c2f4-128">[Node.js][Ed_Nd], [C][Ed_C]</span></span>    |
| <span data-ttu-id="1c2f4-129">Prolnutí Adafruit HUZZAH ESP8266</span><span class="sxs-lookup"><span data-stu-id="1c2f4-129">Adafruit Feather HUZZAH ESP8266</span></span>  | <span data-ttu-id="1c2f4-130">[Arduino][Hu_Ard]</span><span class="sxs-lookup"><span data-stu-id="1c2f4-130">[Arduino][Hu_Ard]</span></span>              |
| <span data-ttu-id="1c2f4-131">Sparkfun ESP8266 věc vývojářů</span><span class="sxs-lookup"><span data-stu-id="1c2f4-131">Sparkfun ESP8266 Thing Dev</span></span>       | <span data-ttu-id="1c2f4-132">[Arduino][Th_Ard]</span><span class="sxs-lookup"><span data-stu-id="1c2f4-132">[Arduino][Th_Ard]</span></span>              |
| <span data-ttu-id="1c2f4-133">Prolnutí Adafruit M0</span><span class="sxs-lookup"><span data-stu-id="1c2f4-133">Adafruit Feather M0</span></span>              | <span data-ttu-id="1c2f4-134">[Arduino][M0_Ard]</span><span class="sxs-lookup"><span data-stu-id="1c2f4-134">[Arduino][M0_Ard]</span></span>              |
| <span data-ttu-id="1c2f4-135">Simulované zařízení na počítačích</span><span class="sxs-lookup"><span data-stu-id="1c2f4-135">Simulated device on PC</span></span>           | <span data-ttu-id="1c2f4-136">[Rozhraní .NET][Sim_NET], [Java][Sim_Jav], [Node.js][Sim_Nd], [Python][Sim_Pyth]</span><span class="sxs-lookup"><span data-stu-id="1c2f4-136">[.NET][Sim_NET], [Java][Sim_Jav], [Node.js][Sim_Nd], [Python][Sim_Pyth]</span></span> |
| <span data-ttu-id="1c2f4-137">Simulátor online zařízení</span><span class="sxs-lookup"><span data-stu-id="1c2f4-137">Online device simulator</span></span>         | <span data-ttu-id="1c2f4-138">[Malinová platformy (Node.js)][Ol_Sim]</span><span class="sxs-lookup"><span data-stu-id="1c2f4-138">[Raspberry Pi (Node.js)][Ol_Sim]</span></span> |

<span data-ttu-id="1c2f4-139">Kromě toho můžete IoT vstupní brána k zařízením povolit, aby připojení do služby IoT hub:</span><span class="sxs-lookup"><span data-stu-id="1c2f4-139">In addition, you can use an IoT Edge gateway to enable devices to connect to your IoT hub:</span></span>

| <span data-ttu-id="1c2f4-140">Zařízení brány</span><span class="sxs-lookup"><span data-stu-id="1c2f4-140">Gateway device</span></span>               | <span data-ttu-id="1c2f4-141">Programovací jazyk</span><span class="sxs-lookup"><span data-stu-id="1c2f4-141">Programming language</span></span> | <span data-ttu-id="1c2f4-142">Platforma</span><span class="sxs-lookup"><span data-stu-id="1c2f4-142">Platform</span></span>         |
|------------------------------|----------------------|------------------|
| <span data-ttu-id="1c2f4-143">Intel NUC (model DE3815TYKE)</span><span class="sxs-lookup"><span data-stu-id="1c2f4-143">Intel NUC (model DE3815TYKE)</span></span> | <span data-ttu-id="1c2f4-144">C</span><span class="sxs-lookup"><span data-stu-id="1c2f4-144">C</span></span>                    | <span data-ttu-id="1c2f4-145">[Linux větru oblasti][NUC_Lnx]</span><span class="sxs-lookup"><span data-stu-id="1c2f4-145">[Wind River Linux][NUC_Lnx]</span></span> |
| <span data-ttu-id="1c2f4-146">Simulované brány</span><span class="sxs-lookup"><span data-stu-id="1c2f4-146">Simulated gateway</span></span>            | <span data-ttu-id="1c2f4-147">C</span><span class="sxs-lookup"><span data-stu-id="1c2f4-147">C</span></span>                    | <span data-ttu-id="1c2f4-148">[Linux][Sim_Lnx], [Windows][Sim_Win]</span><span class="sxs-lookup"><span data-stu-id="1c2f4-148">[Linux][Sim_Lnx], [Windows][Sim_Win]</span></span> |

[!INCLUDE [iot-hub-get-started-extended](../../includes/iot-hub-get-started-extended.md)]

[Pi_Nd]: iot-hub-raspberry-pi-kit-node-get-started.md
[Pi_C]: iot-hub-raspberry-pi-kit-c-get-started.md
[Pi_Py]: iot-hub-raspberry-pi-kit-python-get-started.md
[DevKit]: iot-hub-arduino-iot-devkit-az3166-get-started.md
[Ed_Nd]: iot-hub-intel-edison-kit-node-get-started.md
[Ed_C]: iot-hub-intel-edison-kit-c-get-started.md
[Hu_Ard]: iot-hub-arduino-huzzah-esp8266-get-started.md
[Th_Ard]: iot-hub-sparkfun-esp8266-thing-dev-get-started.md
[M0_Ard]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[Sim_NET]: iot-hub-csharp-csharp-getstarted.md
[Sim_Jav]: iot-hub-java-java-getstarted.md
[Sim_Nd]: iot-hub-node-node-getstarted.md
[Sim_Pyth]: iot-hub-python-getstarted.md
[NUC_Lnx]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
[Sim_Lnx]: iot-hub-linux-iot-edge-get-started.md
[Sim_Win]: iot-hub-windows-iot-edge-get-started.md
[Ol_Sim]: iot-hub-raspberry-pi-web-simulator-get-started.md
