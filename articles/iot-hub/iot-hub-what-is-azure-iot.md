---
title: "aaaAzure řešení pro Internet věcí (IoT Suite) | Microsoft Docs"
description: "Přehled ukázkové architektury řešení IoT a jak se týká toodevices, hello služby Azure IoT Hub, SDK pro zařízení Azure IoT, sady SDK služby Azure IoT a jinými službami Azure."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: a859e379-dca7-42fa-bdf6-1125c86ad140
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.openlocfilehash: 2d934e3f988c530de6a242869c021712d2aa1576
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
[!INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a><span data-ttu-id="a470f-103">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a470f-103">Next steps</span></span>

<span data-ttu-id="a470f-104">Azure IoT Hub je služba Azure, která umožňuje zabezpečenou a spolehlivou obousměrnou komunikaci mezi back-endem řešení a miliony zařízení.</span><span class="sxs-lookup"><span data-stu-id="a470f-104">Azure IoT Hub is an Azure service that enables secure and reliable bi-directional communications between your solution back end and millions of devices.</span></span> <span data-ttu-id="a470f-105">Umožňuje hello back-end řešení pro:</span><span class="sxs-lookup"><span data-stu-id="a470f-105">It enables hello solution back end to:</span></span>

* <span data-ttu-id="a470f-106">Přijímat škálovaná telemetrická data z vašich zařízení.</span><span class="sxs-lookup"><span data-stu-id="a470f-106">Receive telemetry at scale from your devices.</span></span>
* <span data-ttu-id="a470f-107">Data trasy z zpracovatele událostí datového proudu tooa vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="a470f-107">Route data from your devices tooa stream event processor.</span></span>
* <span data-ttu-id="a470f-108">Přijímat nahrávání souborů ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="a470f-108">Receive file uploads from devices.</span></span>
* <span data-ttu-id="a470f-109">Odesílání zpráv typu cloud zařízení toospecific zařízení.</span><span class="sxs-lookup"><span data-stu-id="a470f-109">Send cloud-to-device messages toospecific devices.</span></span>

<span data-ttu-id="a470f-110">Můžete použít vlastní back-end řešení tooimplement IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="a470f-110">You can use IoT Hub tooimplement your own solution back end.</span></span> <span data-ttu-id="a470f-111">Kromě toho zahrnuje IoT Hub identity registru použít tooprovision zařízení, jejich zabezpečených přihlašovacích údajů a jejich práva tooconnect toohello IoT hub.</span><span class="sxs-lookup"><span data-stu-id="a470f-111">In addition, IoT Hub includes an identity registry used tooprovision devices, their security credentials, and their rights tooconnect toohello IoT hub.</span></span> <span data-ttu-id="a470f-112">toolearn Další informace o IoT Hub, najdete v části [co je služba IoT Hub?] [lnk-iot-hub].</span><span class="sxs-lookup"><span data-stu-id="a470f-112">toolearn more about IoT Hub, see [What is IoT Hub?][lnk-iot-hub].</span></span>

<span data-ttu-id="a470f-113">toolearn jak Azure IoT Hub umožňuje správu zařízení založených na standardech pro tooremotely můžete spravovat, konfigurovat a aktualizovat vaše zařízení, najdete v části [přehled správy zařízení s centrem IoT][lnk-device-management].</span><span class="sxs-lookup"><span data-stu-id="a470f-113">toolearn how Azure IoT Hub enables standards-based device management for you tooremotely manage, configure, and update your devices, see [Overview of device management with IoT Hub][lnk-device-management].</span></span>

<span data-ttu-id="a470f-114">tooimplement klientským aplikacím na širokou škálu zařízení hardwarových platforem a operačních systémů, můžete použít sady SDK pro zařízení Azure IoT hello.</span><span class="sxs-lookup"><span data-stu-id="a470f-114">tooimplement client applications on a wide variety of device hardware platforms and operating systems, you can use hello Azure IoT device SDKs.</span></span> <span data-ttu-id="a470f-115">sady SDK pro zařízení Hello zahrnují knihovny, které usnadňují odesílání telemetrie tooan IoT hub a příjem cloud zařízení zpráv.</span><span class="sxs-lookup"><span data-stu-id="a470f-115">hello device SDKs include libraries that facilitate sending telemetry tooan IoT hub and receiving cloud-to-device messages.</span></span> <span data-ttu-id="a470f-116">Při použití sady SDK, hello zařízení můžete vybrat z několika síťových protokolů toocommunicate službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="a470f-116">When you use hello device SDKs, you can choose from several network protocols toocommunicate with IoT Hub.</span></span> <span data-ttu-id="a470f-117">toolearn více, viz hello [informace o zařízení sady SDK][lnk-device-sdks].</span><span class="sxs-lookup"><span data-stu-id="a470f-117">toolearn more, see hello [information about device SDKs][lnk-device-sdks].</span></span>

<span data-ttu-id="a470f-118">tooget spuštění psaní nějaký kód a spuštění některých ukázek najdete v části hello [Začínáme se službou IoT Hub] [ lnk-getstarted] kurzu.</span><span class="sxs-lookup"><span data-stu-id="a470f-118">tooget started writing some code and running some samples, see hello [Get started with IoT Hub][lnk-getstarted] tutorial.</span></span>

<span data-ttu-id="a470f-119">Možná by vás také mohla zajímat sada [Azure IoT Suite][lnk-iot-suite], což je kolekce předkonfigurovaných řešení.</span><span class="sxs-lookup"><span data-stu-id="a470f-119">You may also be interested in [Azure IoT Suite][lnk-iot-suite], which is a collection of preconfigured solutions.</span></span> <span data-ttu-id="a470f-120">Sada IoT Suite umožňuje tooget rychle začít a škálování IoT projekty tooaddress běžné scénáře IoT – například vzdálené monitorování, Správa assetu či prediktivní údržby.</span><span class="sxs-lookup"><span data-stu-id="a470f-120">IoT Suite enables you tooget started quickly and scale IoT projects tooaddress common IoT scenarios--such as remote monitoring, asset management, and predictive maintenance.</span></span>

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md
