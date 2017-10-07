---
title: "hello aaaUnderstand SDK služby Azure IoT | Microsoft Docs"
description: "Příručka vývojáře – informace a odkazy toohello různé Azure IoT zařízení a služby sady SDK můžete použít aplikace zařízení toobuild a back-end aplikace."
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
ms.openlocfilehash: e319451ca97876666e1c011ee0e1a73d0a0f1ed1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-and-use-azure-iot-sdks"></a><span data-ttu-id="cae22-103">Rady pro pochopení a použití sady SDK služby Azure IoT</span><span class="sxs-lookup"><span data-stu-id="cae22-103">Understand and use Azure IoT SDKs</span></span>

<span data-ttu-id="cae22-104">Existují tři kategorie sady SDK pro práci se službou IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="cae22-104">There are three categories of SDK for working with IoT Hub:</span></span>

* <span data-ttu-id="cae22-105">**Sady SDK zařízení** umožňují toobuild aplikace, které běží na zařízení IoT.</span><span class="sxs-lookup"><span data-stu-id="cae22-105">**Device SDKs** enable you toobuild apps that run on your IoT devices.</span></span> <span data-ttu-id="cae22-106">Tyto aplikace odesílat telemetrii tooyour IoT hub a volitelně přijímat zprávy pomocí služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="cae22-106">These apps send telemetry tooyour IoT hub, and optionally receive messages from your IoT hub.</span></span>

* <span data-ttu-id="cae22-107">**Služba SDK** povolit jste toomanage služby IoT hub a volitelně odesílat zprávy tooyour zařízení IoT.</span><span class="sxs-lookup"><span data-stu-id="cae22-107">**Service SDKs** enable you toomanage your IoT hub, and optionally send messages tooyour IoT devices.</span></span>

* <span data-ttu-id="cae22-108">**Azure IoT Edge** vám umožní toobuild brány tooenable zařízení, nepoužívejte jednoho z protokolů hello podporováno, nebo když potřebujete tooprocess zprávy v hraniční hello.</span><span class="sxs-lookup"><span data-stu-id="cae22-108">**Azure IoT Edge** enables you toobuild gateways tooenable devices that don't use one of hello supported protocols, or when you need tooprocess messages on hello edge.</span></span>

<span data-ttu-id="cae22-109">Sady SDK jsou zadané toosupport více programovacích jazyků.</span><span class="sxs-lookup"><span data-stu-id="cae22-109">SDKs are provided toosupport multiple programming languages.</span></span>

## <a name="azure-iot-device-sdks"></a><span data-ttu-id="cae22-110">Azure SDK zařízení IoT</span><span class="sxs-lookup"><span data-stu-id="cae22-110">Azure IoT device SDKs</span></span>

<span data-ttu-id="cae22-111">Hello sady SDK služby Microsoft Azure IoT zařízení obsahovat kód, který usnadňuje vytváření zařízení a aplikací, které se připojují tooand spravuje služby Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="cae22-111">hello Microsoft Azure IoT device SDKs contain code that facilitates building devices and applications that connect tooand are managed by Azure IoT Hub services.</span></span>

<span data-ttu-id="cae22-112">Hello následující SDK pro zařízení Azure IoT jsou k dispozici toodownload z Githubu:</span><span class="sxs-lookup"><span data-stu-id="cae22-112">hello following Azure IoT device SDKs are available toodownload from GitHub:</span></span>

* <span data-ttu-id="cae22-113">[Pro zařízení Azure IoT SDK pro jazyk C] [ lnk-c-device-sdk] napsané v jazyce ANSI C (C99) přenositelnost široký kompatibilitu a platformy.</span><span class="sxs-lookup"><span data-stu-id="cae22-113">[Azure IoT device SDK for C][lnk-c-device-sdk] written in ANSI C (C99) for portability and broad platform compatibility.</span></span> <span data-ttu-id="cae22-114">Existují dva klientské knihovny zařízení pro jazyk C hello nízké úrovně **iothub_client** a hello **serializátor**.</span><span class="sxs-lookup"><span data-stu-id="cae22-114">There are two device client libraries for C, hello low-level **iothub_client** and hello **serializer**.</span></span>
* <span data-ttu-id="cae22-115">[Pro zařízení Azure IoT SDK pro .NET][lnk-dotnet-device-sdk]</span><span class="sxs-lookup"><span data-stu-id="cae22-115">[Azure IoT device SDK for .NET][lnk-dotnet-device-sdk]</span></span>
* <span data-ttu-id="cae22-116">[Pro zařízení Azure IoT SDK pro jazyk Java][lnk-java-device-sdk]</span><span class="sxs-lookup"><span data-stu-id="cae22-116">[Azure IoT device SDK for Java][lnk-java-device-sdk]</span></span>
* <span data-ttu-id="cae22-117">[Pro zařízení Azure IoT SDK pro Node.js][lnk-node-device-sdk]</span><span class="sxs-lookup"><span data-stu-id="cae22-117">[Azure IoT device SDK for Node.js][lnk-node-device-sdk]</span></span>
* <span data-ttu-id="cae22-118">[Pro zařízení Azure IoT SDK pro jazyk Python][lnk-python-device-sdk]</span><span class="sxs-lookup"><span data-stu-id="cae22-118">[Azure IoT device SDK for Python][lnk-python-device-sdk]</span></span>

> [!NOTE]
> <span data-ttu-id="cae22-119">Najdete v souborech readme hello v úložišť GitHub hello informace o použití jazyka a binární soubory tooinstall správci specifické pro platformu balíček a závislosti na vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="cae22-119">See hello readme files in hello GitHub repositories for information about using language and platform-specific package managers tooinstall binaries and dependencies on your development machine.</span></span>
> 
> 

### <a name="os-platform-and-hardware-compatibility"></a><span data-ttu-id="cae22-120">Kompatibilita operačního systému platformy a hardwaru</span><span class="sxs-lookup"><span data-stu-id="cae22-120">OS platform and hardware compatibility</span></span>

<span data-ttu-id="cae22-121">Další informace o kompatibilitě sady SDK s konkrétním hardwarovým zařízením najdete v tématu hello [Azure certifikované pro katalog zařízení IoT][lnk-certified].</span><span class="sxs-lookup"><span data-stu-id="cae22-121">For more information about SDK compatibility with specific hardware devices, see hello [Azure Certified for IoT device catalog][lnk-certified].</span></span>

## <a name="azure-iot-service-sdks"></a><span data-ttu-id="cae22-122">Služby sady SDK služby Azure IoT</span><span class="sxs-lookup"><span data-stu-id="cae22-122">Azure IoT service SDKs</span></span>

<span data-ttu-id="cae22-123">Služba Azure IoT Hello sady SDK obsahovat kód toofacilitate vytváření aplikací, které komunikovat přímo s toomanage zařízení IoT Hub a zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="cae22-123">hello Azure IoT service SDKs contain code toofacilitate building applications that interact directly with IoT Hub toomanage devices and security.</span></span>

<span data-ttu-id="cae22-124">Hello následující sady SDK služby Azure IoT jsou k dispozici toodownload z Githubu:</span><span class="sxs-lookup"><span data-stu-id="cae22-124">hello following Azure IoT service SDKs are available toodownload from GitHub:</span></span>

* <span data-ttu-id="cae22-125">[IoT služba Azure SDK pro .NET][lnk-dotnet-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="cae22-125">[Azure IoT service SDK for .NET][lnk-dotnet-service-sdk]</span></span>
* <span data-ttu-id="cae22-126">[IoT služba Azure SDK pro Node.js][lnk-node-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="cae22-126">[Azure IoT service SDK for Node.js][lnk-node-service-sdk]</span></span>
* <span data-ttu-id="cae22-127">[IoT služba Azure SDK pro jazyk Java][lnk-java-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="cae22-127">[Azure IoT service SDK for Java][lnk-java-service-sdk]</span></span>
* <span data-ttu-id="cae22-128">[IoT služba Azure SDK pro jazyk Python][lnk-python-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="cae22-128">[Azure IoT service SDK for Python][lnk-python-service-sdk]</span></span>
* <span data-ttu-id="cae22-129">[IoT služba Azure SDK pro jazyk C][lnk-c-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="cae22-129">[Azure IoT service SDK for C][lnk-c-service-sdk]</span></span>

> [!NOTE]
> <span data-ttu-id="cae22-130">Najdete v souborech readme hello v úložišť GitHub hello informace o použití jazyka a binární soubory tooinstall správci specifické pro platformu balíček a závislosti na vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="cae22-130">See hello readme files in hello GitHub repositories for information about using language and platform-specific package managers tooinstall binaries and dependencies on your development machine.</span></span>

## <a name="azure-iot-edge"></a><span data-ttu-id="cae22-131">Azure IoT Edge</span><span class="sxs-lookup"><span data-stu-id="cae22-131">Azure IoT Edge</span></span>

<span data-ttu-id="cae22-132">Azure IoT Edge obsahuje hello infrastruktury a moduly toocreate brány řešení IoT.</span><span class="sxs-lookup"><span data-stu-id="cae22-132">Azure IoT Edge contains hello infrastructure and modules toocreate IoT gateway solutions.</span></span> <span data-ttu-id="cae22-133">Můžete rozšířit IoT Edge toocreate brány šité na míru tooany začátku do konce scénář.</span><span class="sxs-lookup"><span data-stu-id="cae22-133">You can extend IoT Edge toocreate gateways tailored tooany end-to-end scenario.</span></span>

<span data-ttu-id="cae22-134">Můžete si stáhnout [Azure IoT Edge] [ lnk-iot-edge] z Githubu.</span><span class="sxs-lookup"><span data-stu-id="cae22-134">You can download [Azure IoT Edge][lnk-iot-edge] from GitHub.</span></span>

## <a name="online-api-reference-documentation"></a><span data-ttu-id="cae22-135">Online referenční dokumentace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="cae22-135">Online API reference documentation</span></span>

<span data-ttu-id="cae22-136">Hello následující seznam obsahuje referenční dokumentace rozhraní API odkazů tooonline pro zařízení Azure IoT, služby a knihovny brány:</span><span class="sxs-lookup"><span data-stu-id="cae22-136">hello following list contains links tooonline API reference documentation for Azure IoT device, service, and gateway libraries:</span></span>

* <span data-ttu-id="cae22-137">[Internet věcí (IoT) rozhraní .NET][lnk-dotnet-ref]</span><span class="sxs-lookup"><span data-stu-id="cae22-137">[Internet of Things (IoT) .NET][lnk-dotnet-ref]</span></span>
* <span data-ttu-id="cae22-138">[Centrum IoT REST][lnk-rest-ref]</span><span class="sxs-lookup"><span data-stu-id="cae22-138">[IoT Hub REST][lnk-rest-ref]</span></span>
* <span data-ttu-id="cae22-139">[Pro zařízení Azure IoT SDK pro jazyk C][lnk-c-ref]</span><span class="sxs-lookup"><span data-stu-id="cae22-139">[Azure IoT device SDK for C][lnk-c-ref]</span></span>
* <span data-ttu-id="cae22-140">[Pro zařízení Azure IoT SDK pro jazyk Java][lnk-java-ref]</span><span class="sxs-lookup"><span data-stu-id="cae22-140">[Azure IoT device SDK for Java][lnk-java-ref]</span></span>
* <span data-ttu-id="cae22-141">[IoT služba Azure SDK pro jazyk Java][lnk-java-service-ref]</span><span class="sxs-lookup"><span data-stu-id="cae22-141">[Azure IoT service SDK for Java][lnk-java-service-ref]</span></span>
* <span data-ttu-id="cae22-142">[Pro zařízení Azure IoT SDK pro Node.js][lnk-node-ref]</span><span class="sxs-lookup"><span data-stu-id="cae22-142">[Azure IoT device SDK for Node.js][lnk-node-ref]</span></span>
* <span data-ttu-id="cae22-143">[IoT služba Azure SDK pro Node.js][lnk-node-service-ref]</span><span class="sxs-lookup"><span data-stu-id="cae22-143">[Azure IoT service SDK for Node.js][lnk-node-service-ref]</span></span>
* <span data-ttu-id="cae22-144">[Azure IoT Edge][lnk-gateway-ref]</span><span class="sxs-lookup"><span data-stu-id="cae22-144">[Azure IoT Edge][lnk-gateway-ref]</span></span>

## <a name="next-steps"></a><span data-ttu-id="cae22-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cae22-145">Next steps</span></span>

<span data-ttu-id="cae22-146">Zahrnout další referenční témata v této příručce pro vývojáře IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="cae22-146">Other reference topics in this IoT Hub developer guide include:</span></span>

* <span data-ttu-id="cae22-147">[Koncové body centra IoT][lnk-devguide-endpoints]</span><span class="sxs-lookup"><span data-stu-id="cae22-147">[IoT Hub endpoints][lnk-devguide-endpoints]</span></span>
* <span data-ttu-id="cae22-148">[IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv][lnk-devguide-query]</span><span class="sxs-lookup"><span data-stu-id="cae22-148">[IoT Hub query language for device twins, jobs, and message routing][lnk-devguide-query]</span></span>
* <span data-ttu-id="cae22-149">[Kvóty a omezení][lnk-devguide-quotas]</span><span class="sxs-lookup"><span data-stu-id="cae22-149">[Quotas and throttling][lnk-devguide-quotas]</span></span>
* <span data-ttu-id="cae22-150">[Podpora MQTT centra IoT][lnk-devguide-mqtt]</span><span class="sxs-lookup"><span data-stu-id="cae22-150">[IoT Hub MQTT support][lnk-devguide-mqtt]</span></span>

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
