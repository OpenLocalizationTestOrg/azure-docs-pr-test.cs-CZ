---
title: aaaOverview Azure IoT Edge | Microsoft Docs
description: "Popisuje hello klíče architektury koncepty v Azure IoT Edge například brány, moduly a zprostředkovatelé."
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
ms.openlocfilehash: 32debc0d4f40cfd7f2cce7cf8c76b12ec18ee2dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-edge-architectural-concepts"></a><span data-ttu-id="22a11-103">Koncepty architektury služby Azure IoT Edge</span><span class="sxs-lookup"><span data-stu-id="22a11-103">Azure IoT Edge architectural concepts</span></span>

<span data-ttu-id="22a11-104">Před zkontrolujte všechny ukázkový kód nebo vytvořit vlastní pole bránu pomocí IoT Edge, byste měli porozumět hello klíčové koncepty, které jsou základem hello architektura IoT okraj.</span><span class="sxs-lookup"><span data-stu-id="22a11-104">Before you examine any sample code or create your own field gateway using IoT Edge, you should understand hello key concepts that underpin hello architecture of IoT Edge.</span></span>

## <a name="iot-edge-modules"></a><span data-ttu-id="22a11-105">Moduly IoT Edge</span><span class="sxs-lookup"><span data-stu-id="22a11-105">IoT Edge modules</span></span>

<span data-ttu-id="22a11-106">Vytvořit bránu s Azure IoT hranou vytvořením a ty se *IoT Edge moduly*.</span><span class="sxs-lookup"><span data-stu-id="22a11-106">You build a gateway with Azure IoT Edge by creating and assembling *IoT Edge modules*.</span></span> <span data-ttu-id="22a11-107">Použijte moduly *zprávy* tooexchange data mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="22a11-107">Modules use *messages* tooexchange data with each other.</span></span> <span data-ttu-id="22a11-108">Modul přijme zprávu o, provádí některé akce, případně ho transformuje na novou zprávu a pak publikuje pro ostatní moduly tooprocess.</span><span class="sxs-lookup"><span data-stu-id="22a11-108">A module receives a message, performs some action on it, optionally transforms it into a new message, and then publishes it for other modules tooprocess.</span></span> <span data-ttu-id="22a11-109">Některé moduly pouze vytvářejí nové zprávy a nezpracovávají zprávy příchozí.</span><span class="sxs-lookup"><span data-stu-id="22a11-109">Some modules might only produce new messages and never process incoming messages.</span></span> <span data-ttu-id="22a11-110">Řetěz modulů vytvoří kanál zpracování dat se každý modul provádění transformace na text hello data na jednom místě v tomto kanálu.</span><span class="sxs-lookup"><span data-stu-id="22a11-110">A chain of modules creates a data processing pipeline with each module performing a transformation on hello data at one point in that pipeline.</span></span>

![Řetěz modulů v bráně vytvořené s použitím služby Azure IoT Edge][1]

<span data-ttu-id="22a11-112">Okraj IoT obsahuje hello následující součásti:</span><span class="sxs-lookup"><span data-stu-id="22a11-112">IoT Edge contains hello following components:</span></span>

* <span data-ttu-id="22a11-113">Předem napsané moduly, které provádějí běžné funkce brány.</span><span class="sxs-lookup"><span data-stu-id="22a11-113">Pre-written modules that perform common gateway functions.</span></span>
* <span data-ttu-id="22a11-114">Hello rozhraní a vývojáře, můžete použít vlastní moduly toowrite.</span><span class="sxs-lookup"><span data-stu-id="22a11-114">hello interfaces a developer can use toowrite custom modules.</span></span>
* <span data-ttu-id="22a11-115">Hello toodeploy nezbytné infrastruktury a spustit sadu moduly.</span><span class="sxs-lookup"><span data-stu-id="22a11-115">hello infrastructure necessary toodeploy and run a set of modules.</span></span>

<span data-ttu-id="22a11-116">Hello SDK poskytuje abstraktní vrstvu, která vám umožní toobuild brány toorun v různých operačních systémů a platformy.</span><span class="sxs-lookup"><span data-stu-id="22a11-116">hello SDK provides an abstraction layer that enables you toobuild gateways toorun on various operating systems and platforms.</span></span>

![Vrstva abstrakce služby Azure IoT Edge][2]

## <a name="messages"></a><span data-ttu-id="22a11-118">Zprávy</span><span class="sxs-lookup"><span data-stu-id="22a11-118">Messages</span></span>

<span data-ttu-id="22a11-119">I když přemýšlíte o moduly předávání zpráv tooeach jiných je tooconceptualize pohodlný způsob, jak funkce brány, se nemusí odpovídat přesně co se stane.</span><span class="sxs-lookup"><span data-stu-id="22a11-119">Although thinking about modules passing messages tooeach other is a convenient way tooconceptualize how a gateway functions, it does not accurately reflect what happens.</span></span> <span data-ttu-id="22a11-120">Moduly IoT Edge pomocí zprostředkovatele toocommunicate mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="22a11-120">IoT Edge modules use a broker toocommunicate with each other.</span></span> <span data-ttu-id="22a11-121">Moduly publikování zpráv toohello zprostředkovatele (pomocí zasílání zpráv vzory, třeba sběrnice nebo publikování a přihlášení k odběru) a nechte hello zprostředkovatele trasy hello zpráva toohello moduly připojené tooit.</span><span class="sxs-lookup"><span data-stu-id="22a11-121">Modules publish messages toohello broker (using messaging patterns such as bus, or publish/subscribe) and then let hello broker route hello message toohello modules connected tooit.</span></span>

<span data-ttu-id="22a11-122">Modul používá hello **Broker_Publish** funkce toopublish toohello zprostředkovatele zpráv.</span><span class="sxs-lookup"><span data-stu-id="22a11-122">A module uses hello **Broker_Publish** function toopublish a message toohello broker.</span></span> <span data-ttu-id="22a11-123">Zprostředkovatel Hello přináší zprávy tooa modulu vyvoláním funkce zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="22a11-123">hello broker delivers messages tooa module by invoking a callback function.</span></span> <span data-ttu-id="22a11-124">Zpráva se skládá ze sady vlastností klíč/hodnota a obsah se předává jako blok paměti.</span><span class="sxs-lookup"><span data-stu-id="22a11-124">A message consists of a set of key/value properties and content passed as a block of memory.</span></span>

![Hello role hello zprostředkovatele v Azure IoT Edge][3]

## <a name="message-routing-and-filtering"></a><span data-ttu-id="22a11-126">Směrování a filtrování zpráv</span><span class="sxs-lookup"><span data-stu-id="22a11-126">Message routing and filtering</span></span>

<span data-ttu-id="22a11-127">Existují dva způsoby toodirect zprávy toohello správné IoT Edge moduly:</span><span class="sxs-lookup"><span data-stu-id="22a11-127">There are two ways toodirect messages toohello correct IoT Edge modules:</span></span>

* <span data-ttu-id="22a11-128">Můžete předat sadu odkazy mohou být předány toohello zprostředkovatele tak hello zprostředkovatele zná hello zdroj a jímka pro každý modul.</span><span class="sxs-lookup"><span data-stu-id="22a11-128">You can pass a set of links can be passed toohello broker so hello broker knows hello source and sink for each module.</span></span>
* <span data-ttu-id="22a11-129">Modul můžete filtrovat podle vlastnosti hello hello zprávy.</span><span class="sxs-lookup"><span data-stu-id="22a11-129">A module can filter on hello properties of hello message.</span></span>

<span data-ttu-id="22a11-130">Modul by měl jednat pouze na základě zprávu, pokud zpráva hello je určený pro ni.</span><span class="sxs-lookup"><span data-stu-id="22a11-130">A module should only act upon a message if hello message is intended for it.</span></span> <span data-ttu-id="22a11-131">Odkazy a efektivně filtrování zpráv vytvoření kanálu zprávy.</span><span class="sxs-lookup"><span data-stu-id="22a11-131">Links and message filtering effectively create a message pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="22a11-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="22a11-132">Next steps</span></span>

<span data-ttu-id="22a11-133">v tématu tyto koncepty použijí v ukázku, můžete spustit, toosee [architektura prozkoumat Edge Azure IoT][lnk-hello-world].</span><span class="sxs-lookup"><span data-stu-id="22a11-133">toosee these concepts applied in a sample you can run, see [Explore Azure IoT Edge architecture][lnk-hello-world].</span></span>

<!-- Images -->
[1]: media/iot-hub-iot-edge-overview/modules.png
[2]: media/iot-hub-iot-edge-overview/modules_2.png
[3]: media/iot-hub-iot-edge-overview/messages_1.png

<!-- Links -->
[lnk-hello-world]: ./iot-hub-linux-iot-edge-get-started.md