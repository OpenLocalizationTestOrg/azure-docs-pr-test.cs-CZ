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

## <a name="next-steps"></a>Další kroky

Azure IoT Hub je služba Azure, která umožňuje zabezpečenou a spolehlivou obousměrnou komunikaci mezi back-endem řešení a miliony zařízení. Umožňuje hello back-end řešení pro:

* Přijímat škálovaná telemetrická data z vašich zařízení.
* Data trasy z zpracovatele událostí datového proudu tooa vaše zařízení.
* Přijímat nahrávání souborů ze zařízení.
* Odesílání zpráv typu cloud zařízení toospecific zařízení.

Můžete použít vlastní back-end řešení tooimplement IoT Hub. Kromě toho zahrnuje IoT Hub identity registru použít tooprovision zařízení, jejich zabezpečených přihlašovacích údajů a jejich práva tooconnect toohello IoT hub. toolearn Další informace o IoT Hub, najdete v části [co je služba IoT Hub?] [lnk-iot-hub].

toolearn jak Azure IoT Hub umožňuje správu zařízení založených na standardech pro tooremotely můžete spravovat, konfigurovat a aktualizovat vaše zařízení, najdete v části [přehled správy zařízení s centrem IoT][lnk-device-management].

tooimplement klientským aplikacím na širokou škálu zařízení hardwarových platforem a operačních systémů, můžete použít sady SDK pro zařízení Azure IoT hello. sady SDK pro zařízení Hello zahrnují knihovny, které usnadňují odesílání telemetrie tooan IoT hub a příjem cloud zařízení zpráv. Při použití sady SDK, hello zařízení můžete vybrat z několika síťových protokolů toocommunicate službou IoT Hub. toolearn více, viz hello [informace o zařízení sady SDK][lnk-device-sdks].

tooget spuštění psaní nějaký kód a spuštění některých ukázek najdete v části hello [Začínáme se službou IoT Hub] [ lnk-getstarted] kurzu.

Možná by vás také mohla zajímat sada [Azure IoT Suite][lnk-iot-suite], což je kolekce předkonfigurovaných řešení. Sada IoT Suite umožňuje tooget rychle začít a škálování IoT projekty tooaddress běžné scénáře IoT – například vzdálené monitorování, Správa assetu či prediktivní údržby.

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md
