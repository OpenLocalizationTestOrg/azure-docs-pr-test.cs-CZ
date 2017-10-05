---
title: "Začínáme fyzického zařízení připojovat k službě Azure IoT Hub | Microsoft Docs"
description: "Zjistěte, jak se připojit ke službě Azure IoT Hub fyzických zařízení a na panely. Zařízení může odesílat telemetrická data do služby IoT Hub a IoT Hub můžete sledovat a spravovat vaše zařízení."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
keywords: Azure iot hub kurzu
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: dobett
ms.openlocfilehash: f4128b6b049aa876e170c56dcf2e40720644dc3d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-iot-hub-get-started-with-physical-devices-tutorials"></a><span data-ttu-id="db5b0-105">Azure IoT Hub začít pracovat s kurzy fyzických zařízení</span><span class="sxs-lookup"><span data-stu-id="db5b0-105">Azure IoT Hub get started with physical devices tutorials</span></span>

<span data-ttu-id="db5b0-106">Tyto kurzy vám představí Azure IoT Hub a sady SDK pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="db5b0-106">These tutorials introduce you to Azure IoT Hub and the device SDKs.</span></span> <span data-ttu-id="db5b0-107">Kurzů k zahrnují běžné scénáře IoT k předvedení funkcí služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="db5b0-107">The tutorials cover common IoT scenarios to demonstrate the capabilities of IoT Hub.</span></span> <span data-ttu-id="db5b0-108">Kurzů k také znázorňují, jak kombinovat s jinými službami Azure a nástroje pro vytváření výkonnější IoT řešení IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="db5b0-108">The tutorials also illustrate how to combine IoT Hub with other Azure services and tools to build more powerful IoT solutions.</span></span> <span data-ttu-id="db5b0-109">Kurzy uvedené v následující tabulce ukazují, jak vytvořit fyzického zařízení IoT.</span><span class="sxs-lookup"><span data-stu-id="db5b0-109">The tutorials listed in the following table show you how to create physical IoT devices.</span></span>

| <span data-ttu-id="db5b0-110">Zařízení IoT</span><span class="sxs-lookup"><span data-stu-id="db5b0-110">IoT device</span></span>                       | <span data-ttu-id="db5b0-111">Programovací jazyk</span><span class="sxs-lookup"><span data-stu-id="db5b0-111">Programming language</span></span> |
|---------------------------------|----------------------|
| <span data-ttu-id="db5b0-112">Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="db5b0-112">Raspberry Pi</span></span>                    | <span data-ttu-id="db5b0-113">[Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]</span><span class="sxs-lookup"><span data-stu-id="db5b0-113">[Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]</span></span>  |
| <span data-ttu-id="db5b0-114">IoT DevKit</span><span class="sxs-lookup"><span data-stu-id="db5b0-114">IoT DevKit</span></span>                      | <span data-ttu-id="db5b0-115">[Arduino v VSCode][DevKit]</span><span class="sxs-lookup"><span data-stu-id="db5b0-115">[Arduino in VSCode][DevKit]</span></span>     |
| <span data-ttu-id="db5b0-116">Intel Edison</span><span class="sxs-lookup"><span data-stu-id="db5b0-116">Intel Edison</span></span>                    | <span data-ttu-id="db5b0-117">[Node.js][Ed_Nd], [C][Ed_C]</span><span class="sxs-lookup"><span data-stu-id="db5b0-117">[Node.js][Ed_Nd], [C][Ed_C]</span></span>           |
| <span data-ttu-id="db5b0-118">Prolnutí Adafruit HUZZAH ESP8266</span><span class="sxs-lookup"><span data-stu-id="db5b0-118">Adafruit Feather HUZZAH ESP8266</span></span> | <span data-ttu-id="db5b0-119">[Arduino][Hu_Ard]</span><span class="sxs-lookup"><span data-stu-id="db5b0-119">[Arduino][Hu_Ard]</span></span>              |
| <span data-ttu-id="db5b0-120">Sparkfun ESP8266 věc vývojářů</span><span class="sxs-lookup"><span data-stu-id="db5b0-120">Sparkfun ESP8266 Thing Dev</span></span>      | <span data-ttu-id="db5b0-121">[Arduino][Th_Ard]</span><span class="sxs-lookup"><span data-stu-id="db5b0-121">[Arduino][Th_Ard]</span></span>              |
| <span data-ttu-id="db5b0-122">Prolnutí Adafruit M0</span><span class="sxs-lookup"><span data-stu-id="db5b0-122">Adafruit Feather M0</span></span>             | <span data-ttu-id="db5b0-123">[Arduino][M0_Ard]</span><span class="sxs-lookup"><span data-stu-id="db5b0-123">[Arduino][M0_Ard]</span></span>              |

<span data-ttu-id="db5b0-124">Kromě toho můžete IoT vstupní brána k zařízením povolit, aby připojení do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="db5b0-124">In addition, you can use an IoT Edge gateway to enable devices to connect to your IoT hub.</span></span>

| <span data-ttu-id="db5b0-125">Zařízení brány</span><span class="sxs-lookup"><span data-stu-id="db5b0-125">Gateway device</span></span>               | <span data-ttu-id="db5b0-126">Programovací jazyk</span><span class="sxs-lookup"><span data-stu-id="db5b0-126">Programming language</span></span> | <span data-ttu-id="db5b0-127">Platforma</span><span class="sxs-lookup"><span data-stu-id="db5b0-127">Platform</span></span>         |
|------------------------------|----------------------|------------------|
| <span data-ttu-id="db5b0-128">Intel NUC (model DE3815TYKE)</span><span class="sxs-lookup"><span data-stu-id="db5b0-128">Intel NUC (model DE3815TYKE)</span></span> | <span data-ttu-id="db5b0-129">C</span><span class="sxs-lookup"><span data-stu-id="db5b0-129">C</span></span>                    | <span data-ttu-id="db5b0-130">[Linux větru oblasti][NUC_Lnx]</span><span class="sxs-lookup"><span data-stu-id="db5b0-130">[Wind River Linux][NUC_Lnx]</span></span> |

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
[NUC_Lnx]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
