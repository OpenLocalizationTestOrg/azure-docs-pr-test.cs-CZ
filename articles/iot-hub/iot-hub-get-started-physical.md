---
title: "Začněte připojením tooAzure fyzických zařízení IoT Hub | Microsoft Docs"
description: "Zjistěte, jak tooconnect fyzické zařízení a na panely tooAzure IoT Hub. Zařízení může odesílat telemetrii tooIoT rozbočovače a IoT Hub můžete sledovat a spravovat vaše zařízení."
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
ms.openlocfilehash: 47ce289c438b2f495d499d724c38ddc4b3307425
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-hub-get-started-with-physical-devices-tutorials"></a><span data-ttu-id="8d1d2-105">Azure IoT Hub začít pracovat s kurzy fyzických zařízení</span><span class="sxs-lookup"><span data-stu-id="8d1d2-105">Azure IoT Hub get started with physical devices tutorials</span></span>

<span data-ttu-id="8d1d2-106">Tyto kurzy vzniku tooAzure IoT Hub a sady SDK pro zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="8d1d2-106">These tutorials introduce you tooAzure IoT Hub and hello device SDKs.</span></span> <span data-ttu-id="8d1d2-107">kurzy Hello zahrnují běžné IoT scénáře toodemonstrate hello funkce služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8d1d2-107">hello tutorials cover common IoT scenarios toodemonstrate hello capabilities of IoT Hub.</span></span> <span data-ttu-id="8d1d2-108">Hello kurzy také ilustrují jak toocombine IoT Hub s další Azure služby a další nástroje toobuild výkonné řešení IoT.</span><span class="sxs-lookup"><span data-stu-id="8d1d2-108">hello tutorials also illustrate how toocombine IoT Hub with other Azure services and tools toobuild more powerful IoT solutions.</span></span> <span data-ttu-id="8d1d2-109">Hello kurzy uvedené v hello následující tabulku zobrazit můžete jak toocreate fyzického zařízení IoT.</span><span class="sxs-lookup"><span data-stu-id="8d1d2-109">hello tutorials listed in hello following table show you how toocreate physical IoT devices.</span></span>

| <span data-ttu-id="8d1d2-110">Zařízení IoT</span><span class="sxs-lookup"><span data-stu-id="8d1d2-110">IoT device</span></span>                       | <span data-ttu-id="8d1d2-111">Programovací jazyk</span><span class="sxs-lookup"><span data-stu-id="8d1d2-111">Programming language</span></span> |
|---------------------------------|----------------------|
| <span data-ttu-id="8d1d2-112">Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="8d1d2-112">Raspberry Pi</span></span>                    | <span data-ttu-id="8d1d2-113">[Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]</span><span class="sxs-lookup"><span data-stu-id="8d1d2-113">[Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]</span></span>  |
| <span data-ttu-id="8d1d2-114">IoT DevKit</span><span class="sxs-lookup"><span data-stu-id="8d1d2-114">IoT DevKit</span></span>                      | <span data-ttu-id="8d1d2-115">[Arduino v VSCode][DevKit]</span><span class="sxs-lookup"><span data-stu-id="8d1d2-115">[Arduino in VSCode][DevKit]</span></span>     |
| <span data-ttu-id="8d1d2-116">Intel Edison</span><span class="sxs-lookup"><span data-stu-id="8d1d2-116">Intel Edison</span></span>                    | <span data-ttu-id="8d1d2-117">[Node.js][Ed_Nd], [C][Ed_C]</span><span class="sxs-lookup"><span data-stu-id="8d1d2-117">[Node.js][Ed_Nd], [C][Ed_C]</span></span>           |
| <span data-ttu-id="8d1d2-118">Prolnutí Adafruit HUZZAH ESP8266</span><span class="sxs-lookup"><span data-stu-id="8d1d2-118">Adafruit Feather HUZZAH ESP8266</span></span> | <span data-ttu-id="8d1d2-119">[Arduino][Hu_Ard]</span><span class="sxs-lookup"><span data-stu-id="8d1d2-119">[Arduino][Hu_Ard]</span></span>              |
| <span data-ttu-id="8d1d2-120">Sparkfun ESP8266 věc vývojářů</span><span class="sxs-lookup"><span data-stu-id="8d1d2-120">Sparkfun ESP8266 Thing Dev</span></span>      | <span data-ttu-id="8d1d2-121">[Arduino][Th_Ard]</span><span class="sxs-lookup"><span data-stu-id="8d1d2-121">[Arduino][Th_Ard]</span></span>              |
| <span data-ttu-id="8d1d2-122">Prolnutí Adafruit M0</span><span class="sxs-lookup"><span data-stu-id="8d1d2-122">Adafruit Feather M0</span></span>             | <span data-ttu-id="8d1d2-123">[Arduino][M0_Ard]</span><span class="sxs-lookup"><span data-stu-id="8d1d2-123">[Arduino][M0_Ard]</span></span>              |

<span data-ttu-id="8d1d2-124">Kromě toho můžete Centrum IoT hraniční brány tooenable zařízení tooconnect tooyour IoT.</span><span class="sxs-lookup"><span data-stu-id="8d1d2-124">In addition, you can use an IoT Edge gateway tooenable devices tooconnect tooyour IoT hub.</span></span>

| <span data-ttu-id="8d1d2-125">Zařízení brány</span><span class="sxs-lookup"><span data-stu-id="8d1d2-125">Gateway device</span></span>               | <span data-ttu-id="8d1d2-126">Programovací jazyk</span><span class="sxs-lookup"><span data-stu-id="8d1d2-126">Programming language</span></span> | <span data-ttu-id="8d1d2-127">Platforma</span><span class="sxs-lookup"><span data-stu-id="8d1d2-127">Platform</span></span>         |
|------------------------------|----------------------|------------------|
| <span data-ttu-id="8d1d2-128">Intel NUC (model DE3815TYKE)</span><span class="sxs-lookup"><span data-stu-id="8d1d2-128">Intel NUC (model DE3815TYKE)</span></span> | <span data-ttu-id="8d1d2-129">C</span><span class="sxs-lookup"><span data-stu-id="8d1d2-129">C</span></span>                    | <span data-ttu-id="8d1d2-130">[Linux větru oblasti][NUC_Lnx]</span><span class="sxs-lookup"><span data-stu-id="8d1d2-130">[Wind River Linux][NUC_Lnx]</span></span> |

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
