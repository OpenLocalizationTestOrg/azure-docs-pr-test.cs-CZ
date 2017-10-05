---
title: "Připojit Malinová platformy Azure IoT Suite | Microsoft Docs"
description: "Kurzy pomocí Node.js nebo C můžete další informace o použití Microsoft Azure IoT Starter Kit malin pí 3 a řešení vzdáleného sledování IoT Suite. Můžete zvolit kurz, který simuluje telemetrických dat, nebo skutečné senzorů, který používá nebo umožňující vzdálené firmware aktualizace."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: dobett
ms.openlocfilehash: eaa6a21a08bd9068b5335a8167f54c2aa387e0e5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="connect-your-microsoft-azure-iot-raspberry-pi-3-starter-kit-to-the-remote-monitoring-solution"></a><span data-ttu-id="fd865-104">Připojit vaše Microsoft Azure IoT malin pí 3 Starter Kit k řešení vzdáleného monitorování</span><span class="sxs-lookup"><span data-stu-id="fd865-104">Connect your Microsoft Azure IoT Raspberry Pi 3 Starter Kit to the remote monitoring solution</span></span>

<span data-ttu-id="fd865-105">Kurzy v této části vám pomůže naučit se připojit k řešení vzdáleného monitorování zařízení malin pí 3.</span><span class="sxs-lookup"><span data-stu-id="fd865-105">The tutorials in this section help you learn how to connect a Raspberry Pi 3 device to the remote monitoring solution.</span></span> <span data-ttu-id="fd865-106">Vyberte vhodné preferovaný programovací jazyk kurzu a jestli máte k dispozici pro použití s vaší malin pí senzor hardwaru.</span><span class="sxs-lookup"><span data-stu-id="fd865-106">Choose the tutorial appropriate to your preferred programming language and the whether you have the sensor hardware available to use with your Raspberry Pi.</span></span>

## <a name="the-tutorials"></a><span data-ttu-id="fd865-107">Kurzů k</span><span class="sxs-lookup"><span data-stu-id="fd865-107">The tutorials</span></span>

| <span data-ttu-id="fd865-108">Kurz</span><span class="sxs-lookup"><span data-stu-id="fd865-108">Tutorial</span></span> | <span data-ttu-id="fd865-109">Poznámky</span><span class="sxs-lookup"><span data-stu-id="fd865-109">Notes</span></span> | <span data-ttu-id="fd865-110">Jazyky</span><span class="sxs-lookup"><span data-stu-id="fd865-110">Languages</span></span> |
| -------- | ----- | --------- |
| <span data-ttu-id="fd865-111">Simulovanou telemetrii (Basic)</span><span class="sxs-lookup"><span data-stu-id="fd865-111">Simulated telemetry (Basic)</span></span>| <span data-ttu-id="fd865-112">Simuluje snímač data.</span><span class="sxs-lookup"><span data-stu-id="fd865-112">Simulates sensor data.</span></span> <span data-ttu-id="fd865-113">Používá samostatný malin pí.</span><span class="sxs-lookup"><span data-stu-id="fd865-113">Uses a standalone Raspberry Pi.</span></span> | <span data-ttu-id="fd865-114">[C][lnk-c-simulator], [Node.js][lnk-node-simulator]</span><span class="sxs-lookup"><span data-stu-id="fd865-114">[C][lnk-c-simulator], [Node.js][lnk-node-simulator]</span></span> |
| <span data-ttu-id="fd865-115">Skutečné senzor (střední)</span><span class="sxs-lookup"><span data-stu-id="fd865-115">Real sensor (Intermediate)</span></span> | <span data-ttu-id="fd865-116">Používá data ze senzoru BME280 připojené k vaší malin pí.</span><span class="sxs-lookup"><span data-stu-id="fd865-116">Uses data from a BME280 sensor connected to your Raspberry Pi.</span></span> | <span data-ttu-id="fd865-117">[C][lnk-c-basic], [Node.js][lnk-node-basic]</span><span class="sxs-lookup"><span data-stu-id="fd865-117">[C][lnk-c-basic], [Node.js][lnk-node-basic]</span></span> |
| <span data-ttu-id="fd865-118">Aktualizace firmwaru implementace (Upřesnit)</span><span class="sxs-lookup"><span data-stu-id="fd865-118">Implement firmware update (Advanced)</span></span>| <span data-ttu-id="fd865-119">Používá data ze senzoru BME280 připojené k vaší malin pí.</span><span class="sxs-lookup"><span data-stu-id="fd865-119">Uses data from a BME280 sensor connected to your Raspberry Pi.</span></span> <span data-ttu-id="fd865-120">Umožňuje při aktualizacích firmwaru vzdálené na vaše malin pí.</span><span class="sxs-lookup"><span data-stu-id="fd865-120">Enables remote firmware updates on your Raspberry Pi.</span></span> | <span data-ttu-id="fd865-121">[C][lnk-c-advanced], [Node.js][lnk-node-advanced]</span><span class="sxs-lookup"><span data-stu-id="fd865-121">[C][lnk-c-advanced], [Node.js][lnk-node-advanced]</span></span> |

## <a name="next-steps"></a><span data-ttu-id="fd865-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fd865-122">Next steps</span></span>

<span data-ttu-id="fd865-123">Přejděte [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) Další ukázky a dokumentace na Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="fd865-123">Visit the [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[lnk-node-simulator]: iot-suite-raspberry-pi-kit-node-get-started-simulator.md
[lnk-node-basic]: iot-suite-raspberry-pi-kit-node-get-started-basic.md
[lnk-node-advanced]: iot-suite-raspberry-pi-kit-node-get-started-advanced.md
[lnk-c-simulator]: iot-suite-raspberry-pi-kit-c-get-started-simulator.md
[lnk-c-basic]: iot-suite-raspberry-pi-kit-c-get-started-basic.md
[lnk-c-advanced]: iot-suite-raspberry-pi-kit-c-get-started-advanced.md