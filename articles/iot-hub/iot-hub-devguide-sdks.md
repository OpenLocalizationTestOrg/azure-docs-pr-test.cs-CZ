---
title: "Pochopení SDK služby Azure IoT | Microsoft Docs"
description: "Příručka vývojáře – informace a odkazy na různých Azure IoT zařízení a služby sadách SDK, které můžete použít k vytváření aplikací pro zařízení a aplikací back-end."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: c5c9a497-bb03-4301-be2d-00edfb7d308f
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bcbf4b9633f58293edb19aeb33dec6602ac4ec8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="understand-and-use-azure-iot-sdks"></a><span data-ttu-id="59dec-103">Rady pro pochopení a použití sady SDK služby Azure IoT</span><span class="sxs-lookup"><span data-stu-id="59dec-103">Understand and use Azure IoT SDKs</span></span>

<span data-ttu-id="59dec-104">Existují tři kategorie sady SDK pro práci se službou IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="59dec-104">There are three categories of SDK for working with IoT Hub:</span></span>

* <span data-ttu-id="59dec-105">**Sady SDK zařízení** umožňují vytvářet aplikace, které běží na zařízení IoT.</span><span class="sxs-lookup"><span data-stu-id="59dec-105">**Device SDKs** enable you to build apps that run on your IoT devices.</span></span> <span data-ttu-id="59dec-106">Tyto aplikace odesílat telemetrická data do služby IoT hub a volitelně přijímat zprávy pomocí služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="59dec-106">These apps send telemetry to your IoT hub, and optionally receive messages from your IoT hub.</span></span>

* <span data-ttu-id="59dec-107">**Služba SDK** umožňují spravovat služby IoT hub a volitelně odesílání zpráv do vašeho zařízení IoT.</span><span class="sxs-lookup"><span data-stu-id="59dec-107">**Service SDKs** enable you to manage your IoT hub, and optionally send messages to your IoT devices.</span></span>

* <span data-ttu-id="59dec-108">**Azure IoT Edge** umožňuje vytvářet brány a povolit zařízení, nepoužívejte jedním z podporovaných protokolů, nebo když potřebujete ke zpracování zpráv na okraj.</span><span class="sxs-lookup"><span data-stu-id="59dec-108">**Azure IoT Edge** enables you to build gateways to enable devices that don't use one of the supported protocols, or when you need to process messages on the edge.</span></span>

<span data-ttu-id="59dec-109">Sady SDK jsou k dispozici pro podporu více programovacích jazyků.</span><span class="sxs-lookup"><span data-stu-id="59dec-109">SDKs are provided to support multiple programming languages.</span></span>

## <a name="azure-iot-device-sdks"></a><span data-ttu-id="59dec-110">Azure SDK zařízení IoT</span><span class="sxs-lookup"><span data-stu-id="59dec-110">Azure IoT device SDKs</span></span>

<span data-ttu-id="59dec-111">Microsoft Azure IoT zařízení sady SDK obsahovat kód, který usnadňuje vytváření zařízení a aplikací, které se připojují k a spravuje služby Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="59dec-111">The Microsoft Azure IoT device SDKs contain code that facilitates building devices and applications that connect to and are managed by Azure IoT Hub services.</span></span>

<span data-ttu-id="59dec-112">Toto zařízení Azure IoT sady SDK jsou k dispozici ke stažení z webu GitHub:</span><span class="sxs-lookup"><span data-stu-id="59dec-112">The following Azure IoT device SDKs are available to download from GitHub:</span></span>

* <span data-ttu-id="59dec-113">[Pro zařízení Azure IoT SDK pro jazyk C] [ lnk-c-device-sdk] napsané v jazyce ANSI C (C99) přenositelnost široký kompatibilitu a platformy.</span><span class="sxs-lookup"><span data-stu-id="59dec-113">[Azure IoT device SDK for C][lnk-c-device-sdk] written in ANSI C (C99) for portability and broad platform compatibility.</span></span> <span data-ttu-id="59dec-114">Existují dva klientské knihovny zařízení pro jazyk C nízké úrovně **iothub_client** a **serializátor**.</span><span class="sxs-lookup"><span data-stu-id="59dec-114">There are two device client libraries for C, the low-level **iothub_client** and the **serializer**.</span></span>
* <span data-ttu-id="59dec-115">[Pro zařízení Azure IoT SDK pro .NET][lnk-dotnet-device-sdk]</span><span class="sxs-lookup"><span data-stu-id="59dec-115">[Azure IoT device SDK for .NET][lnk-dotnet-device-sdk]</span></span>
* <span data-ttu-id="59dec-116">[Pro zařízení Azure IoT SDK pro jazyk Java][lnk-java-device-sdk]</span><span class="sxs-lookup"><span data-stu-id="59dec-116">[Azure IoT device SDK for Java][lnk-java-device-sdk]</span></span>
* <span data-ttu-id="59dec-117">[Pro zařízení Azure IoT SDK pro Node.js][lnk-node-device-sdk]</span><span class="sxs-lookup"><span data-stu-id="59dec-117">[Azure IoT device SDK for Node.js][lnk-node-device-sdk]</span></span>
* <span data-ttu-id="59dec-118">[Pro zařízení Azure IoT SDK pro jazyk Python][lnk-python-device-sdk]</span><span class="sxs-lookup"><span data-stu-id="59dec-118">[Azure IoT device SDK for Python][lnk-python-device-sdk]</span></span>

> [!NOTE]
> <span data-ttu-id="59dec-119">Prohlédněte si soubory readme v úložišť GitHub pro informace o použití jazyka a specifické pro platformu balíček správci nainstalovat binární soubory a závislosti na vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="59dec-119">See the readme files in the GitHub repositories for information about using language and platform-specific package managers to install binaries and dependencies on your development machine.</span></span>
> 
> 

### <a name="os-platform-and-hardware-compatibility"></a><span data-ttu-id="59dec-120">Kompatibilita operačního systému platformy a hardwaru</span><span class="sxs-lookup"><span data-stu-id="59dec-120">OS platform and hardware compatibility</span></span>

<span data-ttu-id="59dec-121">Další informace o kompatibilitě sady SDK s konkrétním hardwarovým zařízení najdete v tématu [Azure certifikované pro katalog zařízení IoT][lnk-certified].</span><span class="sxs-lookup"><span data-stu-id="59dec-121">For more information about SDK compatibility with specific hardware devices, see the [Azure Certified for IoT device catalog][lnk-certified].</span></span>

## <a name="azure-iot-service-sdks"></a><span data-ttu-id="59dec-122">Služby sady SDK služby Azure IoT</span><span class="sxs-lookup"><span data-stu-id="59dec-122">Azure IoT service SDKs</span></span>

<span data-ttu-id="59dec-123">Sady SDK služby Azure IoT obsahovat kód, který usnadňuje vytváření aplikace, které spolupracují přímo službou IoT Hub pro správu zařízení a zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="59dec-123">The Azure IoT service SDKs contain code to facilitate building applications that interact directly with IoT Hub to manage devices and security.</span></span>

<span data-ttu-id="59dec-124">Následující služby Azure IoT sady SDK jsou k dispozici ke stažení z webu GitHub:</span><span class="sxs-lookup"><span data-stu-id="59dec-124">The following Azure IoT service SDKs are available to download from GitHub:</span></span>

* <span data-ttu-id="59dec-125">[IoT služba Azure SDK pro .NET][lnk-dotnet-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="59dec-125">[Azure IoT service SDK for .NET][lnk-dotnet-service-sdk]</span></span>
* <span data-ttu-id="59dec-126">[IoT služba Azure SDK pro Node.js][lnk-node-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="59dec-126">[Azure IoT service SDK for Node.js][lnk-node-service-sdk]</span></span>
* <span data-ttu-id="59dec-127">[IoT služba Azure SDK pro jazyk Java][lnk-java-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="59dec-127">[Azure IoT service SDK for Java][lnk-java-service-sdk]</span></span>
* <span data-ttu-id="59dec-128">[IoT služba Azure SDK pro jazyk Python][lnk-python-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="59dec-128">[Azure IoT service SDK for Python][lnk-python-service-sdk]</span></span>
* <span data-ttu-id="59dec-129">[IoT služba Azure SDK pro jazyk C][lnk-c-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="59dec-129">[Azure IoT service SDK for C][lnk-c-service-sdk]</span></span>

> [!NOTE]
> <span data-ttu-id="59dec-130">Prohlédněte si soubory readme v úložišť GitHub pro informace o použití jazyka a specifické pro platformu balíček správci nainstalovat binární soubory a závislosti na vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="59dec-130">See the readme files in the GitHub repositories for information about using language and platform-specific package managers to install binaries and dependencies on your development machine.</span></span>

## <a name="azure-iot-edge"></a><span data-ttu-id="59dec-131">Azure IoT Edge</span><span class="sxs-lookup"><span data-stu-id="59dec-131">Azure IoT Edge</span></span>

<span data-ttu-id="59dec-132">Azure IoT Edge obsahuje infrastruktury a moduly vytvářet řešení IoT brány.</span><span class="sxs-lookup"><span data-stu-id="59dec-132">Azure IoT Edge contains the infrastructure and modules to create IoT gateway solutions.</span></span> <span data-ttu-id="59dec-133">Můžete rozšířit IoT Edge k vytvoření brány přizpůsobit každý scénář začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="59dec-133">You can extend IoT Edge to create gateways tailored to any end-to-end scenario.</span></span>

<span data-ttu-id="59dec-134">Můžete si stáhnout [Azure IoT Edge] [ lnk-iot-edge] z Githubu.</span><span class="sxs-lookup"><span data-stu-id="59dec-134">You can download [Azure IoT Edge][lnk-iot-edge] from GitHub.</span></span>

## <a name="online-api-reference-documentation"></a><span data-ttu-id="59dec-135">Online referenční dokumentace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="59dec-135">Online API reference documentation</span></span>

<span data-ttu-id="59dec-136">Následující seznam obsahuje odkazy na online referenční dokumentace rozhraní API pro zařízení Azure IoT, služby a knihovny brány:</span><span class="sxs-lookup"><span data-stu-id="59dec-136">The following list contains links to online API reference documentation for Azure IoT device, service, and gateway libraries:</span></span>

* <span data-ttu-id="59dec-137">[Internet věcí (IoT) rozhraní .NET][lnk-dotnet-ref]</span><span class="sxs-lookup"><span data-stu-id="59dec-137">[Internet of Things (IoT) .NET][lnk-dotnet-ref]</span></span>
* <span data-ttu-id="59dec-138">[Centrum IoT REST][lnk-rest-ref]</span><span class="sxs-lookup"><span data-stu-id="59dec-138">[IoT Hub REST][lnk-rest-ref]</span></span>
* <span data-ttu-id="59dec-139">[Pro zařízení Azure IoT SDK pro jazyk C][lnk-c-ref]</span><span class="sxs-lookup"><span data-stu-id="59dec-139">[Azure IoT device SDK for C][lnk-c-ref]</span></span>
* <span data-ttu-id="59dec-140">[Pro zařízení Azure IoT SDK pro jazyk Java][lnk-java-ref]</span><span class="sxs-lookup"><span data-stu-id="59dec-140">[Azure IoT device SDK for Java][lnk-java-ref]</span></span>
* <span data-ttu-id="59dec-141">[IoT služba Azure SDK pro jazyk Java][lnk-java-service-ref]</span><span class="sxs-lookup"><span data-stu-id="59dec-141">[Azure IoT service SDK for Java][lnk-java-service-ref]</span></span>
* <span data-ttu-id="59dec-142">[Pro zařízení Azure IoT SDK pro Node.js][lnk-node-ref]</span><span class="sxs-lookup"><span data-stu-id="59dec-142">[Azure IoT device SDK for Node.js][lnk-node-ref]</span></span>
* <span data-ttu-id="59dec-143">[IoT služba Azure SDK pro Node.js][lnk-node-service-ref]</span><span class="sxs-lookup"><span data-stu-id="59dec-143">[Azure IoT service SDK for Node.js][lnk-node-service-ref]</span></span>
* <span data-ttu-id="59dec-144">[Azure IoT Edge][lnk-gateway-ref]</span><span class="sxs-lookup"><span data-stu-id="59dec-144">[Azure IoT Edge][lnk-gateway-ref]</span></span>

## <a name="next-steps"></a><span data-ttu-id="59dec-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="59dec-145">Next steps</span></span>

<span data-ttu-id="59dec-146">Zahrnout další referenční témata v této příručce pro vývojáře IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="59dec-146">Other reference topics in this IoT Hub developer guide include:</span></span>

* <span data-ttu-id="59dec-147">[Koncové body centra IoT][lnk-devguide-endpoints]</span><span class="sxs-lookup"><span data-stu-id="59dec-147">[IoT Hub endpoints][lnk-devguide-endpoints]</span></span>
* <span data-ttu-id="59dec-148">[IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv][lnk-devguide-query]</span><span class="sxs-lookup"><span data-stu-id="59dec-148">[IoT Hub query language for device twins, jobs, and message routing][lnk-devguide-query]</span></span>
* <span data-ttu-id="59dec-149">[Kvóty a omezení][lnk-devguide-quotas]</span><span class="sxs-lookup"><span data-stu-id="59dec-149">[Quotas and throttling][lnk-devguide-quotas]</span></span>
* <span data-ttu-id="59dec-150">[Podpora MQTT centra IoT][lnk-devguide-mqtt]</span><span class="sxs-lookup"><span data-stu-id="59dec-150">[IoT Hub MQTT support][lnk-devguide-mqtt]</span></span>

<!-- Links and images -->

[lnk-c-device-sdk]: https://github.com/Azure/azure-iot-sdk-c
[lnk-c-service-sdk]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_service_client
[lnk-dotnet-device-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device
[lnk-java-device-sdk]: https://github.com/Azure/azure-iot-sdk-java/tree/master/device
[lnk-dotnet-service-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/service
[lnk-java-service-sdk]: https://github.com/Azure/azure-iot-sdk-java/tree/master/service
[lnk-node-device-sdk]: https://github.com/Azure/azure-iot-sdk-node/tree/master/device
[lnk-node-service-sdk]: https://github.com/Azure/azure-iot-sdk-node/tree/master/service
[lnk-python-device-sdk]: https://github.com/Azure/azure-iot-sdk-python/tree/master/device
[lnk-python-service-sdk]: https://github.com/Azure/azure-iot-sdk-python/tree/master/service
[lnk-certified]: https://catalog.azureiotsuite.com/
[lnk-iot-edge]: https://github.com/Azure/iot-edge

[lnk-dotnet-ref]: https://docs.microsoft.com/dotnet/api/microsoft.azure.devices
[lnk-c-ref]: https://azure.github.io/azure-iot-sdk-c/index.html
[lnk-java-ref]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device
[lnk-node-ref]: https://azure.github.io/azure-iot-sdk-node/
[lnk-rest-ref]: https://docs.microsoft.com/rest/api/iothub/
[lnk-java-service-ref]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service.auth
[lnk-node-service-ref]: https://azure.github.io/azure-iot-sdk-node/
[lnk-gateway-ref]: http://azure.github.io/iot-edge/api_reference/c/html/

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
