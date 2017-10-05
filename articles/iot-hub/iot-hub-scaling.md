---
title: "Škálování Azure IoT Hub | Microsoft Docs"
description: "Postup škálování služby IoT hub pro podporu vašeho propustnost předpokládaného zpráv. Obsahuje souhrn podporovaných propustnosti pro každou vrstvu a možnosti pro horizontálního dělení."
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: e7bd4968-db46-46cf-865d-9c944f683832
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: elioda
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2cb263103da05b10c24aab71d81c43eb25987565
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="scale-your-iot-hub-solution"></a><span data-ttu-id="14cfa-104">Škálování řešení IoT hub</span><span class="sxs-lookup"><span data-stu-id="14cfa-104">Scale your IoT hub solution</span></span>
<span data-ttu-id="14cfa-105">Azure IoT Hub může podporovat až miliony současně připojených zařízení.</span><span class="sxs-lookup"><span data-stu-id="14cfa-105">Azure IoT Hub can support up to a million simultaneously connected devices.</span></span> <span data-ttu-id="14cfa-106">Další informace najdete v tématu [IoT Hub ceny][lnk-pricing].</span><span class="sxs-lookup"><span data-stu-id="14cfa-106">For more information, see [IoT Hub pricing][lnk-pricing].</span></span> <span data-ttu-id="14cfa-107">Jednotlivé jednotky služby IoT Hub umožňuje počet denní zprávy.</span><span class="sxs-lookup"><span data-stu-id="14cfa-107">Each IoT Hub unit allows a certain number of daily messages.</span></span>

<span data-ttu-id="14cfa-108">Chcete-li správně škálování řešení, zvažte konkrétní používání služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="14cfa-108">To properly scale your solution, consider your particular use of IoT Hub.</span></span> <span data-ttu-id="14cfa-109">Zejména zvažte propustnosti vyžaduje ve špičce operací v těchto kategoriích:</span><span class="sxs-lookup"><span data-stu-id="14cfa-109">In particular, consider the required peak throughput for the following categories of operations:</span></span>

* <span data-ttu-id="14cfa-110">Zprávy typu zařízení-cloud</span><span class="sxs-lookup"><span data-stu-id="14cfa-110">Device-to-cloud messages</span></span>
* <span data-ttu-id="14cfa-111">Zprávy typu cloud zařízení</span><span class="sxs-lookup"><span data-stu-id="14cfa-111">Cloud-to-device messages</span></span>
* <span data-ttu-id="14cfa-112">Operace registru identit</span><span class="sxs-lookup"><span data-stu-id="14cfa-112">Identity registry operations</span></span>

<span data-ttu-id="14cfa-113">Kromě těchto informací propustnost, najdete v části [IoT Hub kvóty a omezení] [ IoT Hub quotas and throttles] a navrhněte řešení odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="14cfa-113">In addition to this throughput information, see [IoT Hub quotas and throttles][IoT Hub quotas and throttles] and design your solution accordingly.</span></span>

## <a name="device-to-cloud-and-cloud-to-device-message-throughput"></a><span data-ttu-id="14cfa-114">Propustnost zpráv typu zařízení cloud a z cloudu do zařízení</span><span class="sxs-lookup"><span data-stu-id="14cfa-114">Device-to-cloud and cloud-to-device message throughput</span></span>
<span data-ttu-id="14cfa-115">Nejlepší způsob, jak velikost do řešení IoT Hub je vyhodnotit provozu na základě za jednotku.</span><span class="sxs-lookup"><span data-stu-id="14cfa-115">The best way to size an IoT Hub solution is to evaluate the traffic on a per-unit basis.</span></span>

<span data-ttu-id="14cfa-116">Zprávy typu zařízení cloud postupujte podle těchto pokynů propustnost.</span><span class="sxs-lookup"><span data-stu-id="14cfa-116">Device-to-cloud messages follow these sustained throughput guidelines.</span></span>

| <span data-ttu-id="14cfa-117">Úroveň</span><span class="sxs-lookup"><span data-stu-id="14cfa-117">Tier</span></span> | <span data-ttu-id="14cfa-118">Propustnost</span><span class="sxs-lookup"><span data-stu-id="14cfa-118">Sustained throughput</span></span> | <span data-ttu-id="14cfa-119">Udržovanou rychlost</span><span class="sxs-lookup"><span data-stu-id="14cfa-119">Sustained send rate</span></span> |
| --- | --- | --- |
| <span data-ttu-id="14cfa-120">S1</span><span class="sxs-lookup"><span data-stu-id="14cfa-120">S1</span></span> |<span data-ttu-id="14cfa-121">Až 1111 KB za minutu za jednotku</span><span class="sxs-lookup"><span data-stu-id="14cfa-121">Up to 1111 KB/minute per unit</span></span><br/><span data-ttu-id="14cfa-122">(1,5 GB/den/unit)</span><span class="sxs-lookup"><span data-stu-id="14cfa-122">(1.5 GB/day/unit)</span></span> |<span data-ttu-id="14cfa-123">Průměr 278 zpráv za minutu za jednotku</span><span class="sxs-lookup"><span data-stu-id="14cfa-123">Average of 278 messages/minute per unit</span></span><br/><span data-ttu-id="14cfa-124">(400 000 zprávy dny a hodiny za jednotku)</span><span class="sxs-lookup"><span data-stu-id="14cfa-124">(400,000 messages/day per unit)</span></span> |
| <span data-ttu-id="14cfa-125">S2</span><span class="sxs-lookup"><span data-stu-id="14cfa-125">S2</span></span> |<span data-ttu-id="14cfa-126">Až 16 MB za minutu za jednotku</span><span class="sxs-lookup"><span data-stu-id="14cfa-126">Up to 16 MB/minute per unit</span></span><br/><span data-ttu-id="14cfa-127">(22.8 GB/den/unit)</span><span class="sxs-lookup"><span data-stu-id="14cfa-127">(22.8 GB/day/unit)</span></span> |<span data-ttu-id="14cfa-128">Průměr 4,167 zpráv za minutu za jednotku</span><span class="sxs-lookup"><span data-stu-id="14cfa-128">Average of 4,167 messages/minute per unit</span></span><br/><span data-ttu-id="14cfa-129">(6 milionu zprávy dny a hodiny za jednotku)</span><span class="sxs-lookup"><span data-stu-id="14cfa-129">(6 million messages/day per unit)</span></span> |
| <span data-ttu-id="14cfa-130">S3</span><span class="sxs-lookup"><span data-stu-id="14cfa-130">S3</span></span> |<span data-ttu-id="14cfa-131">Až 814 MB za minutu za jednotku</span><span class="sxs-lookup"><span data-stu-id="14cfa-131">Up to 814 MB/minute per unit</span></span><br/><span data-ttu-id="14cfa-132">(1144.4 GB/den/unit)</span><span class="sxs-lookup"><span data-stu-id="14cfa-132">(1144.4 GB/day/unit)</span></span> |<span data-ttu-id="14cfa-133">Průměr 208,333 zpráv za minutu za jednotku</span><span class="sxs-lookup"><span data-stu-id="14cfa-133">Average of 208,333 messages/minute per unit</span></span><br/><span data-ttu-id="14cfa-134">(300 milionů zprávy dny a hodiny za jednotku)</span><span class="sxs-lookup"><span data-stu-id="14cfa-134">(300 million messages/day per unit)</span></span> |

## <a name="identity-registry-operation-throughput"></a><span data-ttu-id="14cfa-135">Propustnost operaci registru identit</span><span class="sxs-lookup"><span data-stu-id="14cfa-135">Identity registry operation throughput</span></span>
<span data-ttu-id="14cfa-136">Operace s registrem identit služby IoT Hub není by mělo být spuštění operace, jako se většinou vztahují k zřizování zařízení.</span><span class="sxs-lookup"><span data-stu-id="14cfa-136">IoT Hub identity registry operations are not supposed to be run-time operations, as they are mostly related to device provisioning.</span></span>

<span data-ttu-id="14cfa-137">Pro konkrétní shluků čísla výkonu, najdete v části [IoT Hub kvóty a omezení][IoT Hub quotas and throttles].</span><span class="sxs-lookup"><span data-stu-id="14cfa-137">For specific burst performance numbers, see [IoT Hub quotas and throttles][IoT Hub quotas and throttles].</span></span>

## <a name="sharding"></a><span data-ttu-id="14cfa-138">Horizontálního dělení</span><span class="sxs-lookup"><span data-stu-id="14cfa-138">Sharding</span></span>
<span data-ttu-id="14cfa-139">Během jedné IoT hub lze škálovat na miliony zařízení, někdy řešení vyžaduje konkrétní výkonové charakteristiky, které nemůže zaručit jeden IoT hub.</span><span class="sxs-lookup"><span data-stu-id="14cfa-139">While a single IoT hub can scale to millions of devices, sometimes your solution requires specific performance characteristics that a single IoT hub cannot guarantee.</span></span> <span data-ttu-id="14cfa-140">V takovém případě se doporučuje, aby oddílu zařízení do více centra IoT.</span><span class="sxs-lookup"><span data-stu-id="14cfa-140">In that case, it is recommended that you partition your devices into multiple IoT hubs.</span></span> <span data-ttu-id="14cfa-141">Více centra IoT funkce smooth shluky provoz a získat požadovaná propustnost nebo sazby operace, které jsou požadovány.</span><span class="sxs-lookup"><span data-stu-id="14cfa-141">Multiple IoT hubs smooth traffic bursts and obtain the required throughput or operation rates that are required.</span></span>

## <a name="next-steps"></a><span data-ttu-id="14cfa-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="14cfa-142">Next steps</span></span>
<span data-ttu-id="14cfa-143">Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="14cfa-143">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="14cfa-144">[Příručka vývojáře pro službu IoT Hub][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="14cfa-144">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="14cfa-145">[Simulaci zařízení s Azure IoT Edge][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="14cfa-145">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT Hub quotas and throttles]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
