---
title: "Azure IoT Hub komunikační protokoly a porty | Microsoft Docs"
description: "Příručka vývojáře - popisuje podporované komunikační protokoly komunikace zařízení cloud a z cloudu do zařízení a čísla portů, která musí být otevřený."
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
ms.openlocfilehash: 98004a48734e33f85eebf8f6213d9f0751dea843
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# Reference – volba komunikační protokol

IoT Hub umožňuje zařízení používat [MQTT][lnk-mqtt], MQTT přes Websocket, [AMQP][lnk-amqp], protokoly AMQP prostřednictvím technologie WebSockets a HTTP pro zařízení na straně komunikace. Informace o tom, jak tyto protokoly podporují konkrétní funkce služby IoT Hub naleznete v tématu [pokyny komunikace zařízení cloud] [ lnk-d2c-guidance] a [Cloud zařízení komunikace pokyny] [lnk-c2d-guidance].

Následující tabulka poskytuje podrobný doporučení pro zvoleného protokolu:

| Protocol (Protokol) | Pokud byste měli zvolit tento protokol |
| --- | --- |
| MQTT <br> MQTT přes protokol WebSocket |Použít na všech zařízeních, které nevyžadují připojení více zařízení (každý má svoje vlastní přihlašovací údaje podle zařízení) pomocí stejné připojení protokol TLS. |
| AMQP <br> AMQP přes protokol WebSocket |Použít na pole a cloudové brány využívat výhod připojení multiplexní mezi zařízeními. |
| HTTP |Používejte pro zařízení, které nemohou podporovat jiné protokoly. |

Když zvolíte váš protokol pro komunikaci straně zařízení, zvažte následující body:

* **Vzor cloud zařízení**. HTTP nemá účinný způsob, jak implementovat nabízené serveru. Jako takový při použití protokolu HTTP, zařízení dotazovat služby IoT Hub pro zprávy typu cloud zařízení. Tento přístup je neefektivní pro zařízení a služby IoT Hub. V části aktuální HTTP pokyny musí každé zařízení dotazování na zprávy každých 25 minut nebo déle. Na druhé straně MQTT a AMQP podporují nabízené serveru při přijímání zprávy typu cloud zařízení. Umožňují okamžitou nabízela zpráv ze služby IoT Hub zařízení. Pokud vám záleží hlavně doručení latence, MQTT nebo AMQP protokoly nejlepší používat. Pro připojené zřídka zařízení funguje i HTTP.
* **Pole brány**. Při použití MQTT a HTTP, nemůžete se připojit více zařízení (každý má svoje vlastní přihlašovací údaje podle zařízení) pomocí stejné připojení protokol TLS. Proto pro [pole scénáře brány][lnk-azure-gateway-guidance], tyto protokoly jsou zhoršené, protože vyžadují jeden TLS připojení mezi brána pole a IoT Hub pro každé zařízení připojené k bráně pole.
* **Nedostatek prostředků zařízení**. Knihovny MQTT a HTTP mít menší nároky než knihovny AMQP. Pokud zařízení má omezené prostředky (například méně než 1 MB paměti RAM), může být tyto protokoly jako takový pouze implementace protokolu, která je k dispozici.
* **Sítě traversal**. Standardní protokol AMQP používá port 5671, zatímco MQTT naslouchá na portu 8883, která by mohla způsobovat problémy v sítích, které jsou uzavřeny do jiných protokolů než HTTP. MQTT přes objekty WebSockets AMQP prostřednictvím technologie WebSockets a HTTP jsou k dispozici pro použití v tomto scénáři.
* **Velikost datové části**. MQTT a AMQP jsou binární protokoly, které mít za následek kompaktnější datových částí než HTTP.

> [!WARNING]
> Při použití protokolu HTTP, každé zařízení musí dotazovat zprávy typu cloud zařízení každých 25 minut nebo déle. Během vývoje, je přijatelné pro cyklické dotazování častěji, než každých 25 minut.

## Čísla portů

Zařízení může komunikovat s centrem IoT v Azure pomocí různých protokolů. Obvykle volba protokol vycházejí z konkrétní požadavky na řešení. Následující tabulka uvádí Odchozí porty, které musí být otevřené pro zařízení a mohli používat konkrétní protokolu:

| Protocol (Protokol) | Port |
| --- | --- |
| MQTT |8883 |
| MQTT přes objekty WebSockets |443 |
| AMQP |5671 |
| AMQP přes objekty WebSockets |443 |
| HTTP |443 |

Po vytvoření služby IoT hub v oblasti Azure IoT hub uchovává stejnou IP adresu po dobu jeho existence tohoto centra IoT. Ale spravuje kvality služeb, pokud Microsoft přesune na jednotky různých škálování služby IoT hub poté je přiřazen novou IP adresu.


## Další kroky

Další informace o tom, jak IoT Hub implementuje protokol MQTT najdete v tématu [komunikace službou IoT hub pomocí protokolu MQTT][lnk-mqtt-support].

[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-mqtt-support]: iot-hub-mqtt-support.md
[lnk-amqp]: http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf
[lnk-mqtt]: http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.pdf
[lnk-azure-gateway-guidance]: iot-hub-devguide-endpoints.md#field-gateways
