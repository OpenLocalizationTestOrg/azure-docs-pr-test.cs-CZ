---
title: "Azure IoT Hub - Začínáme připojení cloudu toohello zařízení IoT | Microsoft Docs"
description: "Zjistěte, jak tooconnect panely IoT a starter Kit tooAzure IoT Hub. Zařízení může odesílat telemetrii tooIoT rozbočovače a IoT Hub můžete sledovat a spravovat vaše zařízení."
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
ms.openlocfilehash: 6dc956308009091532019ff84aec881f042f0104
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-hub-get-started-tutorials"></a><span data-ttu-id="4bda2-105">Azure IoT Hub Začínáme kurzy</span><span class="sxs-lookup"><span data-stu-id="4bda2-105">Azure IoT Hub get started tutorials</span></span>

<span data-ttu-id="4bda2-106">Můžete použít Azure IoT Hub a řešení Internetu věcí (IoT) toobuild zařízení sady SDK aplikace hello Azure IoT:</span><span class="sxs-lookup"><span data-stu-id="4bda2-106">You can use Azure IoT Hub and hello Azure IoT device SDKs toobuild Internet of Things (IoT) solutions:</span></span>

* <span data-ttu-id="4bda2-107">Azure IoT Hub je plně spravovaná služba ve hello cloudu, který bezpečně připojit, monitoruje a spravuje zařízení IoT.</span><span class="sxs-lookup"><span data-stu-id="4bda2-107">Azure IoT Hub is a fully managed service in hello cloud that securely connects, monitors, and manages your IoT devices.</span></span> <span data-ttu-id="4bda2-108">Použijte hello SDK pro zařízení Azure IoT tooimplement zařízení IoT.</span><span class="sxs-lookup"><span data-stu-id="4bda2-108">Use hello Azure IoT Device SDKs tooimplement your IoT devices.</span></span>
* <span data-ttu-id="4bda2-109">Používejte bránu IoT v složitější scénáře IoT.</span><span class="sxs-lookup"><span data-stu-id="4bda2-109">Use an IoT gateway in more complex IoT scenarios.</span></span> <span data-ttu-id="4bda2-110">Například, kde musíte tooconsider faktorům, jako např. zařízení se starší verzí, náklady na šířku pásma, zásady zabezpečení a ochrana osobních údajů nebo zpracování dat okraj.</span><span class="sxs-lookup"><span data-stu-id="4bda2-110">For example, where you need tooconsider factors such as legacy devices, bandwidth costs, security and privacy policies, or edge data processing.</span></span> <span data-ttu-id="4bda2-111">V těchto scénářích použijete tooimplement Azure IoT hraniční bránu, která se připojuje zařízení tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="4bda2-111">In these scenarios, you use Azure IoT Edge tooimplement a gateway that connects devices tooyour IoT hub.</span></span>

## <a name="what-hello-tutorials-cover"></a><span data-ttu-id="4bda2-112">Co zahrnují kurzy hello</span><span class="sxs-lookup"><span data-stu-id="4bda2-112">What hello tutorials cover</span></span>

<span data-ttu-id="4bda2-113">Tyto kurzy vzniku tooAzure IoT Hub a sady SDK pro zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="4bda2-113">These tutorials introduce you tooAzure IoT Hub and hello device SDKs.</span></span> <span data-ttu-id="4bda2-114">kurzy Hello zahrnují běžné IoT scénáře toodemonstrate hello funkce služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="4bda2-114">hello tutorials cover common IoT scenarios toodemonstrate hello capabilities of IoT Hub.</span></span> <span data-ttu-id="4bda2-115">Hello kurzy také ilustrují jak toocombine IoT Hub s další Azure služby a další nástroje toobuild výkonné řešení IoT.</span><span class="sxs-lookup"><span data-stu-id="4bda2-115">hello tutorials also illustrate how toocombine IoT Hub with other Azure services and tools toobuild more powerful IoT solutions.</span></span> <span data-ttu-id="4bda2-116">V kurzech hello můžete toouse simulované nebo skutečné zařízení IoT.</span><span class="sxs-lookup"><span data-stu-id="4bda2-116">In hello tutorials, you can choose toouse either simulated or real IoT devices.</span></span> <span data-ttu-id="4bda2-117">Kromě toho můžete získat informace jak toouse brány tooenable zařízení tooconnect tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="4bda2-117">In addition, you can learn how toouse a gateway tooenable devices tooconnect tooyour IoT hub.</span></span>

## <a name="set-up-your-device"></a><span data-ttu-id="4bda2-118">Nastavení zařízení</span><span class="sxs-lookup"><span data-stu-id="4bda2-118">Set up your device</span></span>

<span data-ttu-id="4bda2-119">Připojte IoT zařízení nebo brány tooAzure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="4bda2-119">Connect an IoT device or gateway tooAzure IoT Hub.</span></span> <span data-ttu-id="4bda2-120">Můžete vybrat fyzické nebo simulované zařízení tooget, spustit:</span><span class="sxs-lookup"><span data-stu-id="4bda2-120">You can choose a physical or simulated device tooget started:</span></span>

| <span data-ttu-id="4bda2-121">Zařízení IoT</span><span class="sxs-lookup"><span data-stu-id="4bda2-121">IoT device</span></span>                       | <span data-ttu-id="4bda2-122">Programovací jazyk</span><span class="sxs-lookup"><span data-stu-id="4bda2-122">Programming language</span></span> |
|----------------------------------|----------------------|
| <span data-ttu-id="4bda2-123">Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="4bda2-123">Raspberry Pi</span></span>                     | <span data-ttu-id="4bda2-124">[Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]</span><span class="sxs-lookup"><span data-stu-id="4bda2-124">[Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]</span></span>  |
| <span data-ttu-id="4bda2-125">IoT DevKit</span><span class="sxs-lookup"><span data-stu-id="4bda2-125">IoT DevKit</span></span>                       | <span data-ttu-id="4bda2-126">[Arduino v VSCode][DevKit]</span><span class="sxs-lookup"><span data-stu-id="4bda2-126">[Arduino in VSCode][DevKit]</span></span>     |
| <span data-ttu-id="4bda2-127">Intel Edison</span><span class="sxs-lookup"><span data-stu-id="4bda2-127">Intel Edison</span></span>                     | <span data-ttu-id="4bda2-128">[Node.js][Ed_Nd], [C][Ed_C]</span><span class="sxs-lookup"><span data-stu-id="4bda2-128">[Node.js][Ed_Nd], [C][Ed_C]</span></span>    |
| <span data-ttu-id="4bda2-129">Prolnutí Adafruit HUZZAH ESP8266</span><span class="sxs-lookup"><span data-stu-id="4bda2-129">Adafruit Feather HUZZAH ESP8266</span></span>  | <span data-ttu-id="4bda2-130">[Arduino][Hu_Ard]</span><span class="sxs-lookup"><span data-stu-id="4bda2-130">[Arduino][Hu_Ard]</span></span>              |
| <span data-ttu-id="4bda2-131">Sparkfun ESP8266 věc vývojářů</span><span class="sxs-lookup"><span data-stu-id="4bda2-131">Sparkfun ESP8266 Thing Dev</span></span>       | <span data-ttu-id="4bda2-132">[Arduino][Th_Ard]</span><span class="sxs-lookup"><span data-stu-id="4bda2-132">[Arduino][Th_Ard]</span></span>              |
| <span data-ttu-id="4bda2-133">Prolnutí Adafruit M0</span><span class="sxs-lookup"><span data-stu-id="4bda2-133">Adafruit Feather M0</span></span>              | <span data-ttu-id="4bda2-134">[Arduino][M0_Ard]</span><span class="sxs-lookup"><span data-stu-id="4bda2-134">[Arduino][M0_Ard]</span></span>              |
| <span data-ttu-id="4bda2-135">Simulované zařízení na počítačích</span><span class="sxs-lookup"><span data-stu-id="4bda2-135">Simulated device on PC</span></span>           | <span data-ttu-id="4bda2-136">[Rozhraní .NET][Sim_NET], [Java][Sim_Jav], [Node.js][Sim_Nd], [Python][Sim_Pyth]</span><span class="sxs-lookup"><span data-stu-id="4bda2-136">[.NET][Sim_NET], [Java][Sim_Jav], [Node.js][Sim_Nd], [Python][Sim_Pyth]</span></span> |
| <span data-ttu-id="4bda2-137">Simulátor online zařízení</span><span class="sxs-lookup"><span data-stu-id="4bda2-137">Online device simulator</span></span>         | <span data-ttu-id="4bda2-138">[Malinová platformy (Node.js)][Ol_Sim]</span><span class="sxs-lookup"><span data-stu-id="4bda2-138">[Raspberry Pi (Node.js)][Ol_Sim]</span></span> |

<span data-ttu-id="4bda2-139">Kromě toho můžete Centrum IoT hraniční brány tooenable zařízení tooconnect tooyour IoT:</span><span class="sxs-lookup"><span data-stu-id="4bda2-139">In addition, you can use an IoT Edge gateway tooenable devices tooconnect tooyour IoT hub:</span></span>

| <span data-ttu-id="4bda2-140">Zařízení brány</span><span class="sxs-lookup"><span data-stu-id="4bda2-140">Gateway device</span></span>               | <span data-ttu-id="4bda2-141">Programovací jazyk</span><span class="sxs-lookup"><span data-stu-id="4bda2-141">Programming language</span></span> | <span data-ttu-id="4bda2-142">Platforma</span><span class="sxs-lookup"><span data-stu-id="4bda2-142">Platform</span></span>         |
|------------------------------|----------------------|------------------|
| <span data-ttu-id="4bda2-143">Intel NUC (model DE3815TYKE)</span><span class="sxs-lookup"><span data-stu-id="4bda2-143">Intel NUC (model DE3815TYKE)</span></span> | <span data-ttu-id="4bda2-144">C</span><span class="sxs-lookup"><span data-stu-id="4bda2-144">C</span></span>                    | <span data-ttu-id="4bda2-145">[Linux větru oblasti][NUC_Lnx]</span><span class="sxs-lookup"><span data-stu-id="4bda2-145">[Wind River Linux][NUC_Lnx]</span></span> |
| <span data-ttu-id="4bda2-146">Simulované brány</span><span class="sxs-lookup"><span data-stu-id="4bda2-146">Simulated gateway</span></span>            | <span data-ttu-id="4bda2-147">C</span><span class="sxs-lookup"><span data-stu-id="4bda2-147">C</span></span>                    | <span data-ttu-id="4bda2-148">[Linux][Sim_Lnx], [Windows][Sim_Win]</span><span class="sxs-lookup"><span data-stu-id="4bda2-148">[Linux][Sim_Lnx], [Windows][Sim_Win]</span></span> |

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
