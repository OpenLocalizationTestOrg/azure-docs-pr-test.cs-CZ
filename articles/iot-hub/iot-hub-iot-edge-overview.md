---
title: "Přehled Azure IoT Edge | Microsoft Docs"
description: "Popisuje klíčové architektury koncepty v Azure IoT Edge, jako jsou brány, moduly a zprostředkovatelé."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: dobett
ms.openlocfilehash: ecdd56c91a8fc2011b3d7abe93b9d27c1e1e0bef
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-iot-edge-architectural-concepts"></a><span data-ttu-id="451c0-103">Koncepty architektury služby Azure IoT Edge</span><span class="sxs-lookup"><span data-stu-id="451c0-103">Azure IoT Edge architectural concepts</span></span>

<span data-ttu-id="451c0-104">Před zkontrolujte všechny ukázkový kód nebo vytvořit vlastní pole bránu pomocí IoT Edge, byste měli porozumět klíčové koncepty, které jsou základem architektuře IoT okraj.</span><span class="sxs-lookup"><span data-stu-id="451c0-104">Before you examine any sample code or create your own field gateway using IoT Edge, you should understand the key concepts that underpin the architecture of IoT Edge.</span></span>

## <a name="iot-edge-modules"></a><span data-ttu-id="451c0-105">Moduly IoT Edge</span><span class="sxs-lookup"><span data-stu-id="451c0-105">IoT Edge modules</span></span>

<span data-ttu-id="451c0-106">Vytvořit bránu s Azure IoT hranou vytvořením a ty se *IoT Edge moduly*.</span><span class="sxs-lookup"><span data-stu-id="451c0-106">You build a gateway with Azure IoT Edge by creating and assembling *IoT Edge modules*.</span></span> <span data-ttu-id="451c0-107">Moduly používají pro výměnu dat mezi sebou *zprávy*.</span><span class="sxs-lookup"><span data-stu-id="451c0-107">Modules use *messages* to exchange data with each other.</span></span> <span data-ttu-id="451c0-108">Modul obdrží zprávu a provede s ní určitou akci, například ji může transformovat na novou zprávu a pak ji publikovat pro ostatní moduly ke zpracování.</span><span class="sxs-lookup"><span data-stu-id="451c0-108">A module receives a message, performs some action on it, optionally transforms it into a new message, and then publishes it for other modules to process.</span></span> <span data-ttu-id="451c0-109">Některé moduly pouze vytvářejí nové zprávy a nezpracovávají zprávy příchozí.</span><span class="sxs-lookup"><span data-stu-id="451c0-109">Some modules might only produce new messages and never process incoming messages.</span></span> <span data-ttu-id="451c0-110">Řetěz modulů vytvoří kanál zpracování dat, ve kterém každý modul provádí transformaci dat v daném místě kanálu.</span><span class="sxs-lookup"><span data-stu-id="451c0-110">A chain of modules creates a data processing pipeline with each module performing a transformation on the data at one point in that pipeline.</span></span>

![Řetěz modulů v bráně vytvořené s použitím služby Azure IoT Edge][1]

<span data-ttu-id="451c0-112">Okraj IoT obsahuje následující součásti:</span><span class="sxs-lookup"><span data-stu-id="451c0-112">IoT Edge contains the following components:</span></span>

* <span data-ttu-id="451c0-113">Předem napsané moduly, které provádějí běžné funkce brány.</span><span class="sxs-lookup"><span data-stu-id="451c0-113">Pre-written modules that perform common gateway functions.</span></span>
* <span data-ttu-id="451c0-114">Rozhraní, které vývojář může použít k vytvoření vlastních modulů.</span><span class="sxs-lookup"><span data-stu-id="451c0-114">The interfaces a developer can use to write custom modules.</span></span>
* <span data-ttu-id="451c0-115">Infrastrukturu nutnou pro nasazení a spuštění sady modulů.</span><span class="sxs-lookup"><span data-stu-id="451c0-115">The infrastructure necessary to deploy and run a set of modules.</span></span>

<span data-ttu-id="451c0-116">Sada SDK poskytuje abstraktní vrstvu, která vám umožní sestavovat bran pro spouštění v různých operačních systémů a platformy.</span><span class="sxs-lookup"><span data-stu-id="451c0-116">The SDK provides an abstraction layer that enables you to build gateways to run on various operating systems and platforms.</span></span>

![Vrstva abstrakce služby Azure IoT Edge][2]

## <a name="messages"></a><span data-ttu-id="451c0-118">Zprávy</span><span class="sxs-lookup"><span data-stu-id="451c0-118">Messages</span></span>

<span data-ttu-id="451c0-119">Ačkoli je představa modulů posílajících zprávy pohodlným způsobem, jak popsat koncepci funkce brány, neodráží přesně samotnou její činnost.</span><span class="sxs-lookup"><span data-stu-id="451c0-119">Although thinking about modules passing messages to each other is a convenient way to conceptualize how a gateway functions, it does not accurately reflect what happens.</span></span> <span data-ttu-id="451c0-120">Moduly IoT Edge pomocí zprostředkovatele vzájemně komunikovat.</span><span class="sxs-lookup"><span data-stu-id="451c0-120">IoT Edge modules use a broker to communicate with each other.</span></span> <span data-ttu-id="451c0-121">Moduly publikování zpráv zprostředkovatele (pomocí zasílání zpráv vzory, třeba sběrnice nebo publikování a přihlášení k odběru) a nechte zprostředkovatele směrovat zprávy na moduly, které k němu připojená.</span><span class="sxs-lookup"><span data-stu-id="451c0-121">Modules publish messages to the broker (using messaging patterns such as bus, or publish/subscribe) and then let the broker route the message to the modules connected to it.</span></span>

<span data-ttu-id="451c0-122">K publikování zpráv do zprostředkovatele používá modul funkci **Broker_Publish**.</span><span class="sxs-lookup"><span data-stu-id="451c0-122">A module uses the **Broker_Publish** function to publish a message to the broker.</span></span> <span data-ttu-id="451c0-123">Zprostředkovatel předává zprávy ostatním modulům zavoláním funkce zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="451c0-123">The broker delivers messages to a module by invoking a callback function.</span></span> <span data-ttu-id="451c0-124">Zpráva se skládá ze sady vlastností klíč/hodnota a obsah se předává jako blok paměti.</span><span class="sxs-lookup"><span data-stu-id="451c0-124">A message consists of a set of key/value properties and content passed as a block of memory.</span></span>

![Role zprostředkovatele ve službě Azure IoT Edge][3]

## <a name="message-routing-and-filtering"></a><span data-ttu-id="451c0-126">Směrování a filtrování zpráv</span><span class="sxs-lookup"><span data-stu-id="451c0-126">Message routing and filtering</span></span>

<span data-ttu-id="451c0-127">Přímé zpráv, které mají správnou moduly IoT hraniční dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="451c0-127">There are two ways to direct messages to the correct IoT Edge modules:</span></span>

* <span data-ttu-id="451c0-128">Můžete předat sadu odkazy mohou být předána do zprostředkovatele, aby věděl, zprostředkovatel může zdrojový a podřízený pro každý modul.</span><span class="sxs-lookup"><span data-stu-id="451c0-128">You can pass a set of links can be passed to the broker so the broker knows the source and sink for each module.</span></span>
* <span data-ttu-id="451c0-129">Modul můžete filtrovat podle vlastnosti zprávy.</span><span class="sxs-lookup"><span data-stu-id="451c0-129">A module can filter on the properties of the message.</span></span>

<span data-ttu-id="451c0-130">Modul by měl postupovat pouze podle zpráv, které mu jsou určené.</span><span class="sxs-lookup"><span data-stu-id="451c0-130">A module should only act upon a message if the message is intended for it.</span></span> <span data-ttu-id="451c0-131">Odkazy a efektivně filtrování zpráv vytvoření kanálu zprávy.</span><span class="sxs-lookup"><span data-stu-id="451c0-131">Links and message filtering effectively create a message pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="451c0-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="451c0-132">Next steps</span></span>

<span data-ttu-id="451c0-133">Tyto koncepty použijí v ukázku, můžete spustit najdete v sekci [prozkoumat Azure IoT Edge architektura][lnk-hello-world].</span><span class="sxs-lookup"><span data-stu-id="451c0-133">To see these concepts applied in a sample you can run, see [Explore Azure IoT Edge architecture][lnk-hello-world].</span></span>

<!-- Images -->
[1]: media/iot-hub-iot-edge-overview/modules.png
[2]: media/iot-hub-iot-edge-overview/modules_2.png
[3]: media/iot-hub-iot-edge-overview/messages_1.png

<!-- Links -->
[lnk-hello-world]: ./iot-hub-linux-iot-edge-get-started.md