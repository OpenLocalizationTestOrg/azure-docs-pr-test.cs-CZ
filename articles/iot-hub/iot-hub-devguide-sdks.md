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
# <a name="understand-and-use-azure-iot-sdks"></a>Rady pro pochopení a použití sady SDK služby Azure IoT

Existují tři kategorie sady SDK pro práci se službou IoT Hub:

* **Sady SDK zařízení** umožňují toobuild aplikace, které běží na zařízení IoT. Tyto aplikace odesílat telemetrii tooyour IoT hub a volitelně přijímat zprávy pomocí služby IoT hub.

* **Služba SDK** povolit jste toomanage služby IoT hub a volitelně odesílat zprávy tooyour zařízení IoT.

* **Azure IoT Edge** vám umožní toobuild brány tooenable zařízení, nepoužívejte jednoho z protokolů hello podporováno, nebo když potřebujete tooprocess zprávy v hraniční hello.

Sady SDK jsou zadané toosupport více programovacích jazyků.

## <a name="azure-iot-device-sdks"></a>Azure SDK zařízení IoT

Hello sady SDK služby Microsoft Azure IoT zařízení obsahovat kód, který usnadňuje vytváření zařízení a aplikací, které se připojují tooand spravuje služby Azure IoT Hub.

Hello následující SDK pro zařízení Azure IoT jsou k dispozici toodownload z Githubu:

* [Pro zařízení Azure IoT SDK pro jazyk C] [ lnk-c-device-sdk] napsané v jazyce ANSI C (C99) přenositelnost široký kompatibilitu a platformy. Existují dva klientské knihovny zařízení pro jazyk C hello nízké úrovně **iothub_client** a hello **serializátor**.
* [Pro zařízení Azure IoT SDK pro .NET][lnk-dotnet-device-sdk]
* [Pro zařízení Azure IoT SDK pro jazyk Java][lnk-java-device-sdk]
* [Pro zařízení Azure IoT SDK pro Node.js][lnk-node-device-sdk]
* [Pro zařízení Azure IoT SDK pro jazyk Python][lnk-python-device-sdk]

> [!NOTE]
> Najdete v souborech readme hello v úložišť GitHub hello informace o použití jazyka a binární soubory tooinstall správci specifické pro platformu balíček a závislosti na vývojovém počítači.
> 
> 

### <a name="os-platform-and-hardware-compatibility"></a>Kompatibilita operačního systému platformy a hardwaru

Další informace o kompatibilitě sady SDK s konkrétním hardwarovým zařízením najdete v tématu hello [Azure certifikované pro katalog zařízení IoT][lnk-certified].

## <a name="azure-iot-service-sdks"></a>Služby sady SDK služby Azure IoT

Služba Azure IoT Hello sady SDK obsahovat kód toofacilitate vytváření aplikací, které komunikovat přímo s toomanage zařízení IoT Hub a zabezpečení.

Hello následující sady SDK služby Azure IoT jsou k dispozici toodownload z Githubu:

* [IoT služba Azure SDK pro .NET][lnk-dotnet-service-sdk]
* [IoT služba Azure SDK pro Node.js][lnk-node-service-sdk]
* [IoT služba Azure SDK pro jazyk Java][lnk-java-service-sdk]
* [IoT služba Azure SDK pro jazyk Python][lnk-python-service-sdk]
* [IoT služba Azure SDK pro jazyk C][lnk-c-service-sdk]

> [!NOTE]
> Najdete v souborech readme hello v úložišť GitHub hello informace o použití jazyka a binární soubory tooinstall správci specifické pro platformu balíček a závislosti na vývojovém počítači.

## <a name="azure-iot-edge"></a>Azure IoT Edge

Azure IoT Edge obsahuje hello infrastruktury a moduly toocreate brány řešení IoT. Můžete rozšířit IoT Edge toocreate brány šité na míru tooany začátku do konce scénář.

Můžete si stáhnout [Azure IoT Edge] [ lnk-iot-edge] z Githubu.

## <a name="online-api-reference-documentation"></a>Online referenční dokumentace rozhraní API

Hello následující seznam obsahuje referenční dokumentace rozhraní API odkazů tooonline pro zařízení Azure IoT, služby a knihovny brány:

* [Internet věcí (IoT) rozhraní .NET][lnk-dotnet-ref]
* [Centrum IoT REST][lnk-rest-ref]
* [Pro zařízení Azure IoT SDK pro jazyk C][lnk-c-ref]
* [Pro zařízení Azure IoT SDK pro jazyk Java][lnk-java-ref]
* [IoT služba Azure SDK pro jazyk Java][lnk-java-service-ref]
* [Pro zařízení Azure IoT SDK pro Node.js][lnk-node-ref]
* [IoT služba Azure SDK pro Node.js][lnk-node-service-ref]
* [Azure IoT Edge][lnk-gateway-ref]

## <a name="next-steps"></a>Další kroky

Zahrnout další referenční témata v této příručce pro vývojáře IoT Hub:

* [Koncové body centra IoT][lnk-devguide-endpoints]
* [IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv][lnk-devguide-query]
* [Kvóty a omezení][lnk-devguide-quotas]
* [Podpora MQTT centra IoT][lnk-devguide-mqtt]

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
