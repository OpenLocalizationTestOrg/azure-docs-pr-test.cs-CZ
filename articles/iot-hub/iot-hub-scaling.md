---
title: "škálování služby IoT Hub aaaAzure | Microsoft Docs"
description: "Jak tooscale vaše toosupport centra IoT propustnost vaší předpokládaného zpráv. Obsahuje souhrn hello podporované propustnosti pro každou vrstvu a možnosti pro horizontálního dělení."
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
ms.openlocfilehash: 3b8bf6c44631c65b34b69752d9043c21db24bb01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scale-your-iot-hub-solution"></a><span data-ttu-id="d4c5f-104">Škálování řešení IoT hub</span><span class="sxs-lookup"><span data-stu-id="d4c5f-104">Scale your IoT hub solution</span></span>
<span data-ttu-id="d4c5f-105">Azure IoT Hub může podporovat až tooa miliony současně připojených zařízení.</span><span class="sxs-lookup"><span data-stu-id="d4c5f-105">Azure IoT Hub can support up tooa million simultaneously connected devices.</span></span> <span data-ttu-id="d4c5f-106">Další informace najdete v tématu [IoT Hub ceny][lnk-pricing].</span><span class="sxs-lookup"><span data-stu-id="d4c5f-106">For more information, see [IoT Hub pricing][lnk-pricing].</span></span> <span data-ttu-id="d4c5f-107">Jednotlivé jednotky služby IoT Hub umožňuje počet denní zprávy.</span><span class="sxs-lookup"><span data-stu-id="d4c5f-107">Each IoT Hub unit allows a certain number of daily messages.</span></span>

<span data-ttu-id="d4c5f-108">tooproperly škálování řešení, zvažte konkrétní používání služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d4c5f-108">tooproperly scale your solution, consider your particular use of IoT Hub.</span></span> <span data-ttu-id="d4c5f-109">Zvažte především propustnost ve špičce hello požadované pro hello následující kategorie operací:</span><span class="sxs-lookup"><span data-stu-id="d4c5f-109">In particular, consider hello required peak throughput for hello following categories of operations:</span></span>

* <span data-ttu-id="d4c5f-110">Zprávy typu zařízení-cloud</span><span class="sxs-lookup"><span data-stu-id="d4c5f-110">Device-to-cloud messages</span></span>
* <span data-ttu-id="d4c5f-111">Zprávy typu cloud zařízení</span><span class="sxs-lookup"><span data-stu-id="d4c5f-111">Cloud-to-device messages</span></span>
* <span data-ttu-id="d4c5f-112">Operace registru identit</span><span class="sxs-lookup"><span data-stu-id="d4c5f-112">Identity registry operations</span></span>

<span data-ttu-id="d4c5f-113">V další toothis propustnost informace naleznete v části [IoT Hub kvóty a omezení] [ IoT Hub quotas and throttles] a navrhněte řešení odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="d4c5f-113">In addition toothis throughput information, see [IoT Hub quotas and throttles][IoT Hub quotas and throttles] and design your solution accordingly.</span></span>

## <a name="device-to-cloud-and-cloud-to-device-message-throughput"></a><span data-ttu-id="d4c5f-114">Propustnost zpráv typu zařízení cloud a z cloudu do zařízení</span><span class="sxs-lookup"><span data-stu-id="d4c5f-114">Device-to-cloud and cloud-to-device message throughput</span></span>
<span data-ttu-id="d4c5f-115">nejlepší způsob, jak toosize Hello řešení IoT Hub je tooevaluate hello provozu na základě za jednotku.</span><span class="sxs-lookup"><span data-stu-id="d4c5f-115">hello best way toosize an IoT Hub solution is tooevaluate hello traffic on a per-unit basis.</span></span>

<span data-ttu-id="d4c5f-116">Zprávy typu zařízení cloud postupujte podle těchto pokynů propustnost.</span><span class="sxs-lookup"><span data-stu-id="d4c5f-116">Device-to-cloud messages follow these sustained throughput guidelines.</span></span>

| <span data-ttu-id="d4c5f-117">Úroveň</span><span class="sxs-lookup"><span data-stu-id="d4c5f-117">Tier</span></span> | <span data-ttu-id="d4c5f-118">Propustnost</span><span class="sxs-lookup"><span data-stu-id="d4c5f-118">Sustained throughput</span></span> | <span data-ttu-id="d4c5f-119">Udržovanou rychlost</span><span class="sxs-lookup"><span data-stu-id="d4c5f-119">Sustained send rate</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d4c5f-120">S1</span><span class="sxs-lookup"><span data-stu-id="d4c5f-120">S1</span></span> |<span data-ttu-id="d4c5f-121">Až too1111 KB za minutu za jednotku</span><span class="sxs-lookup"><span data-stu-id="d4c5f-121">Up too1111 KB/minute per unit</span></span><br/><span data-ttu-id="d4c5f-122">(1,5 GB/den/unit)</span><span class="sxs-lookup"><span data-stu-id="d4c5f-122">(1.5 GB/day/unit)</span></span> |<span data-ttu-id="d4c5f-123">Průměr 278 zpráv za minutu za jednotku</span><span class="sxs-lookup"><span data-stu-id="d4c5f-123">Average of 278 messages/minute per unit</span></span><br/><span data-ttu-id="d4c5f-124">(400 000 zprávy dny a hodiny za jednotku)</span><span class="sxs-lookup"><span data-stu-id="d4c5f-124">(400,000 messages/day per unit)</span></span> |
| <span data-ttu-id="d4c5f-125">S2</span><span class="sxs-lookup"><span data-stu-id="d4c5f-125">S2</span></span> |<span data-ttu-id="d4c5f-126">Až too16 MB za minutu za jednotku</span><span class="sxs-lookup"><span data-stu-id="d4c5f-126">Up too16 MB/minute per unit</span></span><br/><span data-ttu-id="d4c5f-127">(22.8 GB/den/unit)</span><span class="sxs-lookup"><span data-stu-id="d4c5f-127">(22.8 GB/day/unit)</span></span> |<span data-ttu-id="d4c5f-128">Průměr 4,167 zpráv za minutu za jednotku</span><span class="sxs-lookup"><span data-stu-id="d4c5f-128">Average of 4,167 messages/minute per unit</span></span><br/><span data-ttu-id="d4c5f-129">(6 milionu zprávy dny a hodiny za jednotku)</span><span class="sxs-lookup"><span data-stu-id="d4c5f-129">(6 million messages/day per unit)</span></span> |
| <span data-ttu-id="d4c5f-130">S3</span><span class="sxs-lookup"><span data-stu-id="d4c5f-130">S3</span></span> |<span data-ttu-id="d4c5f-131">Až too814 MB za minutu za jednotku</span><span class="sxs-lookup"><span data-stu-id="d4c5f-131">Up too814 MB/minute per unit</span></span><br/><span data-ttu-id="d4c5f-132">(1144.4 GB/den/unit)</span><span class="sxs-lookup"><span data-stu-id="d4c5f-132">(1144.4 GB/day/unit)</span></span> |<span data-ttu-id="d4c5f-133">Průměr 208,333 zpráv za minutu za jednotku</span><span class="sxs-lookup"><span data-stu-id="d4c5f-133">Average of 208,333 messages/minute per unit</span></span><br/><span data-ttu-id="d4c5f-134">(300 milionů zprávy dny a hodiny za jednotku)</span><span class="sxs-lookup"><span data-stu-id="d4c5f-134">(300 million messages/day per unit)</span></span> |

## <a name="identity-registry-operation-throughput"></a><span data-ttu-id="d4c5f-135">Propustnost operaci registru identit</span><span class="sxs-lookup"><span data-stu-id="d4c5f-135">Identity registry operation throughput</span></span>
<span data-ttu-id="d4c5f-136">Operace s registrem identit služby IoT Hub nejsou by měl toobe spuštění operace, jako jsou většinou související toodevice zřizování.</span><span class="sxs-lookup"><span data-stu-id="d4c5f-136">IoT Hub identity registry operations are not supposed toobe run-time operations, as they are mostly related toodevice provisioning.</span></span>

<span data-ttu-id="d4c5f-137">Pro konkrétní shluků čísla výkonu, najdete v části [IoT Hub kvóty a omezení][IoT Hub quotas and throttles].</span><span class="sxs-lookup"><span data-stu-id="d4c5f-137">For specific burst performance numbers, see [IoT Hub quotas and throttles][IoT Hub quotas and throttles].</span></span>

## <a name="sharding"></a><span data-ttu-id="d4c5f-138">Horizontálního dělení</span><span class="sxs-lookup"><span data-stu-id="d4c5f-138">Sharding</span></span>
<span data-ttu-id="d4c5f-139">Při jednom IoT hub můžete škálovat toomillions zařízení, někdy řešení vyžaduje konkrétní výkonové charakteristiky, které nemůže zaručit jeden IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d4c5f-139">While a single IoT hub can scale toomillions of devices, sometimes your solution requires specific performance characteristics that a single IoT hub cannot guarantee.</span></span> <span data-ttu-id="d4c5f-140">V takovém případě se doporučuje, aby oddílu zařízení do více centra IoT.</span><span class="sxs-lookup"><span data-stu-id="d4c5f-140">In that case, it is recommended that you partition your devices into multiple IoT hubs.</span></span> <span data-ttu-id="d4c5f-141">Více centra IoT funkce smooth shluky provoz a získat požadovaná propustnost hello nebo sazby operace, které jsou požadovány.</span><span class="sxs-lookup"><span data-stu-id="d4c5f-141">Multiple IoT hubs smooth traffic bursts and obtain hello required throughput or operation rates that are required.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d4c5f-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d4c5f-142">Next steps</span></span>
<span data-ttu-id="d4c5f-143">toofurther prozkoumat hello služby IoT Hub, najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="d4c5f-143">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="d4c5f-144">[Příručka vývojáře pro službu IoT Hub][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="d4c5f-144">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="d4c5f-145">[Simulaci zařízení s Azure IoT Edge][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="d4c5f-145">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT Hub quotas and throttles]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
