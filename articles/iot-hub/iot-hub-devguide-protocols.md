---
title: "aaaAzure IoT Hub komunikační protokoly a porty | Microsoft Docs"
description: "Příručka vývojáře - popisuje hello podporované komunikační protokoly komunikace zařízení cloud a z cloudu do zařízení a hello čísla portů, která musí být otevřený."
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
ms.openlocfilehash: 31cb948f1d30edd87edb13ad0dd859c02bcc3239
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# Reference – volba komunikační protokol

IoT Hub umožňuje zařízení toouse [MQTT][lnk-mqtt], MQTT přes Websocket, [AMQP][lnk-amqp], protokoly AMQP prostřednictvím technologie WebSockets a HTTP pro zařízení na straně komunikace. Informace o tom, jak tyto protokoly podporují konkrétní funkce služby IoT Hub naleznete v tématu [pokyny komunikace zařízení cloud] [ lnk-d2c-guidance] a [Cloud zařízení komunikace pokyny] [lnk-c2d-guidance].

Hello následující tabulka obsahuje hello vysoké úrovně doporučení pro zvoleného protokolu:

| Protocol (Protokol) | Pokud byste měli zvolit tento protokol |
| --- | --- |
| MQTT <br> MQTT přes protokol WebSocket |Použít na všech zařízeních, které nevyžadují tooconnect více zařízení (každý má svoje vlastní přihlašovací údaje podle zařízení) přes hello stejné připojení protokol TLS. |
| AMQP <br> AMQP přes protokol WebSocket |Použít na pole a cloudové brány tootake výhod připojení multiplexní mezi zařízeními. |
| HTTP |Používejte pro zařízení, které nemohou podporovat jiné protokoly. |

Mějte na paměti následující body, když zvolíte váš protokol pro komunikaci straně zařízení hello:

* **Vzor cloud zařízení**. HTTP nemá nabízené účinný způsob tooimplement serveru. Jako takový při použití protokolu HTTP, zařízení dotazovat služby IoT Hub pro zprávy typu cloud zařízení. Tento přístup je neefektivní hello zařízení a služby IoT Hub. V části aktuální HTTP pokyny musí každé zařízení dotazování na zprávy každých 25 minut nebo déle. Na hello druhé straně, MQTT a AMQP podporu nabízených serveru při přijímání zprávy typu cloud zařízení. Umožňují okamžitou nabízených oznámení zpráv ze zařízení IoT Hub toohello. Pokud vám záleží hlavně doručení latence, MQTT nebo AMQP jsou nejlepší toouse protokoly hello. Pro připojené zřídka zařízení funguje i HTTP.
* **Pole brány**. Při použití MQTT a HTTP, nemůžete se připojit více zařízení (každý má svoje vlastní přihlašovací údaje podle zařízení) pomocí hello stejné připojení protokol TLS. Proto pro [pole scénáře brány][lnk-azure-gateway-guidance], tyto protokoly jsou zhoršené, protože vyžadují jeden TLS připojení mezi brána pole hello a IoT Hub pro každé zařízení připojených toohello pole bránu.
* **Nedostatek prostředků zařízení**. Hello MQTT a knihovny HTTP mít menší nároky než hello AMQP knihovny. Jako takový, pokud hello zařízení má omezené prostředky (například méně než 1 MB paměti RAM), tyto protokoly mohou být hello pouze implementace protokolu k dispozici.
* **Sítě traversal**. standardní protokol AMQP Hello používá port 5671, zatímco MQTT naslouchá na portu 8883, která by mohla způsobovat problémy v sítích, které jsou uzavřené toonon HTTP protokoly. MQTT přes objekty WebSockets AMQP prostřednictvím technologie WebSockets a HTTP jsou k dispozici toobe v tomto scénáři používají.
* **Velikost datové části**. MQTT a AMQP jsou binární protokoly, které mít za následek kompaktnější datových částí než HTTP.

> [!WARNING]
> Při použití protokolu HTTP, každé zařízení musí dotazovat zprávy typu cloud zařízení každých 25 minut nebo déle. Během vývoje, je však přijatelné toopoll častěji, než každých 25 minut.

## Čísla portů

Zařízení může komunikovat s centrem IoT v Azure pomocí různých protokolů. Volba hello protokolu obvykle doprovází hello specifických požadavků hello řešení. Hello následující tabulka uvádí hello Odchozí porty, které musí být otevřené pro možnost toouse zařízení toobe určitý protokol:

| Protocol (Protokol) | Port |
| --- | --- |
| MQTT |8883 |
| MQTT přes objekty WebSockets |443 |
| AMQP |5671 |
| AMQP přes objekty WebSockets |443 |
| HTTP |443 |

Po vytvoření služby IoT hub v oblasti Azure, hello IoT hub udržuje hello stejnou IP adresu pro hello životnost této služby IoT hub. Toomaintain kvality služeb, pokud se přesune Microsoft hello IoT hub tooa jiné měřítko jednotky mělo je však přiřadit novou IP adresu.


## Další kroky

toolearn Další informace o tom, jak IoT Hub implementuje protokol MQTT hello, najdete v části [komunikace službou IoT hub pomocí protokolu MQTT hello][lnk-mqtt-support].

[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-mqtt-support]: iot-hub-mqtt-support.md
[lnk-amqp]: http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf
[lnk-mqtt]: http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.pdf
[lnk-azure-gateway-guidance]: iot-hub-devguide-endpoints.md#field-gateways
