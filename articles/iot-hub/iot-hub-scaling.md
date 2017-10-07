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
# <a name="scale-your-iot-hub-solution"></a>Škálování řešení IoT hub
Azure IoT Hub může podporovat až tooa miliony současně připojených zařízení. Další informace najdete v tématu [IoT Hub ceny][lnk-pricing]. Jednotlivé jednotky služby IoT Hub umožňuje počet denní zprávy.

tooproperly škálování řešení, zvažte konkrétní používání služby IoT Hub. Zvažte především propustnost ve špičce hello požadované pro hello následující kategorie operací:

* Zprávy typu zařízení-cloud
* Zprávy typu cloud zařízení
* Operace registru identit

V další toothis propustnost informace naleznete v části [IoT Hub kvóty a omezení] [ IoT Hub quotas and throttles] a navrhněte řešení odpovídajícím způsobem.

## <a name="device-to-cloud-and-cloud-to-device-message-throughput"></a>Propustnost zpráv typu zařízení cloud a z cloudu do zařízení
nejlepší způsob, jak toosize Hello řešení IoT Hub je tooevaluate hello provozu na základě za jednotku.

Zprávy typu zařízení cloud postupujte podle těchto pokynů propustnost.

| Úroveň | Propustnost | Udržovanou rychlost |
| --- | --- | --- |
| S1 |Až too1111 KB za minutu za jednotku<br/>(1,5 GB/den/unit) |Průměr 278 zpráv za minutu za jednotku<br/>(400 000 zprávy dny a hodiny za jednotku) |
| S2 |Až too16 MB za minutu za jednotku<br/>(22.8 GB/den/unit) |Průměr 4,167 zpráv za minutu za jednotku<br/>(6 milionu zprávy dny a hodiny za jednotku) |
| S3 |Až too814 MB za minutu za jednotku<br/>(1144.4 GB/den/unit) |Průměr 208,333 zpráv za minutu za jednotku<br/>(300 milionů zprávy dny a hodiny za jednotku) |

## <a name="identity-registry-operation-throughput"></a>Propustnost operaci registru identit
Operace s registrem identit služby IoT Hub nejsou by měl toobe spuštění operace, jako jsou většinou související toodevice zřizování.

Pro konkrétní shluků čísla výkonu, najdete v části [IoT Hub kvóty a omezení][IoT Hub quotas and throttles].

## <a name="sharding"></a>Horizontálního dělení
Při jednom IoT hub můžete škálovat toomillions zařízení, někdy řešení vyžaduje konkrétní výkonové charakteristiky, které nemůže zaručit jeden IoT hub. V takovém případě se doporučuje, aby oddílu zařízení do více centra IoT. Více centra IoT funkce smooth shluky provoz a získat požadovaná propustnost hello nebo sazby operace, které jsou požadovány.

## <a name="next-steps"></a>Další kroky
toofurther prozkoumat hello služby IoT Hub, najdete v tématu:

* [Příručka vývojáře pro službu IoT Hub][lnk-devguide]
* [Simulaci zařízení s Azure IoT Edge][lnk-iotedge]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT Hub quotas and throttles]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
