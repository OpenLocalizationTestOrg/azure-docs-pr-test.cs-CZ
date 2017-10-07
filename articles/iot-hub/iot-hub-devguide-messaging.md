---
title: "zasílání zpráv Azure IoT Hub aaaUnderstand | Microsoft Docs"
description: "Příručka vývojáře - zařízení cloud a cloud zařízení zasílání zpráv službou IoT Hub. Obsahuje informace o formáty zpráv a protokoly podporované komunikace."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3fc5f1a3-3711-4611-9897-d4db079b4250
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.openlocfilehash: a610741e23e243f392f1c042f9ab4a00d42f734f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="device-to-cloud-and-cloud-to-device-messaging-with-iot-hub"></a><span data-ttu-id="763e1-104">Zařízení cloud a cloud zařízení zasílání zpráv s centrem IoT</span><span class="sxs-lookup"><span data-stu-id="763e1-104">Device-to-cloud and cloud-to-device messaging with IoT Hub</span></span>

<span data-ttu-id="763e1-105">Pomocí služby IoT Hub ve vašich zařízeních pomocí zasílání zpráv toocommunicate:</span><span class="sxs-lookup"><span data-stu-id="763e1-105">Use IoT Hub messaging toocommunicate with your devices by:</span></span>

* <span data-ttu-id="763e1-106">Odesílání [zařízení cloud] [ lnk-d2c] zprávy z vašeho zařízení tooyour řešení back-end.</span><span class="sxs-lookup"><span data-stu-id="763e1-106">Sending [device-to-cloud][lnk-d2c] messages from your devices tooyour solution back end.</span></span>
* <span data-ttu-id="763e1-107">Odesílání [cloud zařízení] [ lnk-c2d] zprávy z hello řešení zpět ukončení tooyour zařízení.</span><span class="sxs-lookup"><span data-stu-id="763e1-107">Sending [cloud-to-device][lnk-c2d] messages from hello solution back end tooyour devices.</span></span>

<span data-ttu-id="763e1-108">Základní vlastnosti funkce zasílání zpráv služby IoT Hub jsou hello spolehlivost a odolnost zpráv.</span><span class="sxs-lookup"><span data-stu-id="763e1-108">Core properties of IoT Hub messaging functionality are hello reliability and durability of messages.</span></span> <span data-ttu-id="763e1-109">Tyto vlastnosti umožňují odolnost toointermittent připojení na straně zařízení hello a tooload špičky v případě zpracování na straně cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="763e1-109">These properties enable resilience toointermittent connectivity on hello device side, and tooload spikes in event processing on hello cloud side.</span></span> <span data-ttu-id="763e1-110">IoT Hub implementuje *alespoň jednou* doručení zaručuje pro zasílání zpráv typu zařízení cloud a cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="763e1-110">IoT Hub implements *at least once* delivery guarantees for both device-to-cloud and cloud-to-device messaging.</span></span>

<span data-ttu-id="763e1-111">Možnosti Úvod toohello IoT Hub, najdete v článcích hello [Azure a Internet věcí] [ lnk-azure-iot] a [přehled hello služby Azure IoT Hub] [lnk-iot-hub-overview].</span><span class="sxs-lookup"><span data-stu-id="763e1-111">For an introduction toohello capabilities of IoT Hub, see hello articles [Azure and Internet of Things][lnk-azure-iot] and [Overview of hello Azure IoT Hub service][lnk-iot-hub-overview].</span></span>

## <a name="when-toouse-iot-hub-messaging"></a><span data-ttu-id="763e1-112">Při zasílání zpráv služby IoT Hub toouse</span><span class="sxs-lookup"><span data-stu-id="763e1-112">When toouse IoT Hub messaging</span></span>

<span data-ttu-id="763e1-113">Použijte zpráv typu zařízení cloud pro odesílání výstrahy a časové řady telemetrie z vaší aplikace zařízení a cloud zařízení zprávy pro aplikaci zařízení tooyour jednosměrný oznámení.</span><span class="sxs-lookup"><span data-stu-id="763e1-113">Use device-to-cloud messages for sending time series telemetry and alerts from your device app, and cloud-to-device messages for one-way notifications tooyour device app.</span></span>

* <span data-ttu-id="763e1-114">Odkazovat příliš[pokyny komunikace zařízení cloud] [ lnk-d2c-guidance] Pokud máte pochybnosti mezi pomocí zpráv typu zařízení cloud, hlášen vlastnostech nebo nahrávání souborů.</span><span class="sxs-lookup"><span data-stu-id="763e1-114">Refer too[Device-to-cloud communication guidance][lnk-d2c-guidance] if in doubt between using device-to-cloud messages, reported properties, or file upload.</span></span>
* <span data-ttu-id="763e1-115">Odkazovat příliš[Cloud zařízení komunikace pokyny] [ lnk-c2d-guidance] Pokud nejistých mezi pomocí zpráv typu cloud zařízení, požadované vlastnosti nebo metody direct.</span><span class="sxs-lookup"><span data-stu-id="763e1-115">Refer too[Cloud-to-device communication guidance][lnk-c2d-guidance] if in doubt between using cloud-to-device messages, desired properties, or direct methods.</span></span>

## <a name="next-steps"></a><span data-ttu-id="763e1-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="763e1-116">Next steps</span></span>

* <span data-ttu-id="763e1-117">Další informace o IoT Hub [zasílání zpráv typu zařízení cloud][lnk-d2c].</span><span class="sxs-lookup"><span data-stu-id="763e1-117">Learn about IoT Hub [device-to-cloud messaging][lnk-d2c].</span></span>
* <span data-ttu-id="763e1-118">Další informace o IoT Hub [zasílání zpráv typu cloud zařízení][lnk-c2d].</span><span class="sxs-lookup"><span data-stu-id="763e1-118">Learn about IoT Hub [cloud-to-device messaging][lnk-c2d].</span></span>

[lnk-azure-iot]: iot-hub-what-is-azure-iot.md
[lnk-iot-hub-overview]: iot-hub-what-is-iot-hub.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md