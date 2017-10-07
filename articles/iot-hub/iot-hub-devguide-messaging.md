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
# <a name="device-to-cloud-and-cloud-to-device-messaging-with-iot-hub"></a>Zařízení cloud a cloud zařízení zasílání zpráv s centrem IoT

Pomocí služby IoT Hub ve vašich zařízeních pomocí zasílání zpráv toocommunicate:

* Odesílání [zařízení cloud] [ lnk-d2c] zprávy z vašeho zařízení tooyour řešení back-end.
* Odesílání [cloud zařízení] [ lnk-c2d] zprávy z hello řešení zpět ukončení tooyour zařízení.

Základní vlastnosti funkce zasílání zpráv služby IoT Hub jsou hello spolehlivost a odolnost zpráv. Tyto vlastnosti umožňují odolnost toointermittent připojení na straně zařízení hello a tooload špičky v případě zpracování na straně cloudu hello. IoT Hub implementuje *alespoň jednou* doručení zaručuje pro zasílání zpráv typu zařízení cloud a cloud zařízení.

Možnosti Úvod toohello IoT Hub, najdete v článcích hello [Azure a Internet věcí] [ lnk-azure-iot] a [přehled hello služby Azure IoT Hub] [lnk-iot-hub-overview].

## <a name="when-toouse-iot-hub-messaging"></a>Při zasílání zpráv služby IoT Hub toouse

Použijte zpráv typu zařízení cloud pro odesílání výstrahy a časové řady telemetrie z vaší aplikace zařízení a cloud zařízení zprávy pro aplikaci zařízení tooyour jednosměrný oznámení.

* Odkazovat příliš[pokyny komunikace zařízení cloud] [ lnk-d2c-guidance] Pokud máte pochybnosti mezi pomocí zpráv typu zařízení cloud, hlášen vlastnostech nebo nahrávání souborů.
* Odkazovat příliš[Cloud zařízení komunikace pokyny] [ lnk-c2d-guidance] Pokud nejistých mezi pomocí zpráv typu cloud zařízení, požadované vlastnosti nebo metody direct.

## <a name="next-steps"></a>Další kroky

* Další informace o IoT Hub [zasílání zpráv typu zařízení cloud][lnk-d2c].
* Další informace o IoT Hub [zasílání zpráv typu cloud zařízení][lnk-c2d].

[lnk-azure-iot]: iot-hub-what-is-azure-iot.md
[lnk-iot-hub-overview]: iot-hub-what-is-iot-hub.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md