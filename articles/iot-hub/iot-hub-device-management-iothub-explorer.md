---
title: "Správa zařízení IoT pomocí Průzkumníka iothub aaaAzure | Microsoft Docs"
description: "Nástroj hello iothub-explorer rozhraní příkazového řádku pro správu zařízení Azure IoT Hub, poskytuje funkci hello přímé metody a možnosti správy hello Twin požadované vlastnosti."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Správa zařízení Azure iot, správou zařízení azure iot hub, iot správy zařízení, správou zařízení iot hub"
ms.assetid: b34f799a-fc14-41b9-bf45-54751163fffe
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/12/2017
ms.author: xshi
ms.openlocfilehash: e0a5e6120db5c4fb12f7f8b605a56e0e4aad9217
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-iothub-explorer-for-azure-iot-hub-device-management"></a>Použití iothub Průzkumníka pro správu zařízení Azure IoT Hub

![Diagram začátku do konce](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[iothub-explorer](https://github.com/azure/iothub-explorer) je nástroj rozhraní příkazového řádku, který jste spustili identit zařízení toomanage počítač na hostitele v registru centra IoT. Se dodává s možností správy, které můžete použít tooperform různé úlohy.

| Možnost správy          | Úkol                                                                                                                            |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Přímé metody             | Nastavit zařízení fungovat jako je například spuštění nebo zastavení odesílání zpráv nebo restartování zařízení hello.                                        |
| Vlastnosti Twin potřeby    | Umístit zařízení do určité stavy, jako je třeba nastavení DIODU toogreen nebo nastavení telemetrie hello poslat too30 minut určený v intervalu.         |
| Twin hlášené vlastnosti   | Získat hello hlášené stav zařízení. Například hello zařízení hlásí hello je nyní blikat Indikátor.                                    |
| Značky Twin                  | Metadata specifická pro zařízení ukládat v cloudu hello. Například hello počítače prodejních umístění nasazení.                         |
| Zprávy typu cloud zařízení   | Odešlete oznámení tooa zařízení. Například "je velmi pravděpodobně toorain ještě dnes. Nezapomeňte toobring zastřešující."              |
| Dotazy twin zařízení        | Dotaz všechny tooretrieve dvojčata zařízení ty s libovolné podmínky, jako je identifikace hello zařízení, které jsou k dispozici pro použití. |

Podrobnější vysvětlení na hello rozdíly a pokyny k použití těchto možností najdete v článku [pokyny komunikace zařízení cloud](iot-hub-devguide-d2c-guidance.md) a [Cloud zařízení komunikace pokyny](iot-hub-devguide-c2d-guidance.md).

> [!NOTE]
> Dvojčata zařízení jsou dokumenty JSON, které obsahují informace o stavu zařízení (metadata, konfigurace a podmínky). IoT Hub trvá dvojče zařízení pro každé zařízení, která se připojuje tooit. Další informace o dvojčata zařízení najdete v tématu [začít pracovat s dvojčata zařízení](iot-hub-node-node-twin-getstarted.md).

## <a name="what-you-learn"></a>Co se naučíte

Zjistíte pomocí různých možností správy na vývojovém počítači iothub-explorer.

## <a name="what-you-do"></a>Co dělat

Spusťte Průzkumníka iothub s různé možnosti správy.

## <a name="what-you-need"></a>Co potřebujete

- Kurz [nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md) dokončit, která zahrnuje hello následující požadavky:
  - Aktivní předplatné Azure.
  - V rámci svého předplatného služby Azure IoT hub.
  - Klientská aplikace, která odesílá zprávy tooyour Azure IoT hub.
- Zajistěte, aby že vaše zařízení se systémem s hello klientská aplikace během tohoto kurzu.
- iothub-explorer [nainstalovat iothub-explorer](https://github.com/azure/iothub-explorer) na vývojovém počítači.

## <a name="connect-tooyour-iot-hub"></a>Připojit tooyour IoT hub

Připojte tooyour IoT hub spuštěním hello následující příkaz:

```bash
iothub-explorer login <your IoT hub connection string>
```

## <a name="use-iothub-explorer-with-direct-methods"></a>Pomocí Průzkumníka iothub přímé metody

Vyvolání hello `start` metoda hello zařízení aplikaci toosend zprávy tooyour IoT hub spuštěním hello následující příkaz:

```bash
iothub-explorer device-method <your device Id> start
```

Vyvolání hello `stop` metoda v hello zařízení aplikaci toostop odesílání zpráv tooyour IoT hub spuštěním hello následující příkaz:

```bash
iothub-explorer device-method <your device Id> stop
```

## <a name="use-iothub-explorer-with-twins-desired-properties"></a>Pomocí Průzkumníka iothub požadované vlastnosti pro dvojici

Nastavení intervalu požadovanou vlastnost = 3000 spuštěním hello následující příkaz:

```bash
iothub-explorer update-twin <your device id> {\"properties\":{\"desired\":{\"interval\":3000}}}
```

Tuto vlastnost lze číst zařízením.

## <a name="use-iothub-explorer-with-twins-reported-properties"></a>Použití iothub Průzkumníka s vlastnostmi hlášené pro dvojici

Získat hello hlášené vlastnosti zařízení hello spuštěním hello následující příkaz:

```bash
iothub-explorer get-twin <your device id>
```

Jedna z vlastností hello je $metadata. $lastUpdated které ukazuje hello čas poslední toto zařízení odeslání nebo přijetí zprávy.

## <a name="use-iothub-explorer-with-twins-tags"></a>Použití iothub Průzkumníka pomocí značek pro dvojici

Zobrazení značek hello a vlastnosti zařízení hello spuštěním hello následující příkaz:

```bash
iothub-explorer get-twin <your device id>
```

Přidání role pole = teploty a vlhkosti toohello zařízení tak, že spustíte následující příkaz hello:

```bash
iothub-explorer update-twin <your device id> "{\"tags\":{\"role\":\"temperature&humidity\"}}"

```

## <a name="use-iothub-explorer-with-cloud-to-device-messages"></a>Pomocí Průzkumníka iothub zprávy typu Cloud zařízení

Odeslat zařízení toohello zprávu "Hello, World" tak, že spustíte následující příkaz hello:

```bash
iothub-explorer send <device-id> "Hello World"
```

V tématu [použijte toosend iothub-explorer a příjem zpráv mezi zařízením a IoT Hub](iot-hub-explorer-cloud-device-messaging.md) pro skutečné scénář použití tohoto příkazu.

## <a name="use-iothub-explorer-with-device-twins-queries"></a>Použití iothub Průzkumníka s dotazy dvojčata zařízení

Dotaz na zařízení s značkou role = 'teploty a vlhkosti' spuštěním hello následující příkaz:

```bash
iothub-explorer query-twin "SELECT * FROM devices WHERE tags.role = 'temperature&humidity'"
```

Dotaz na všechna zařízení kromě těch, které se značkou role = 'teploty a vlhkosti' spuštěním hello následující příkaz:

```bash
iothub-explorer query-twin "SELECT * FROM devices WHERE tags.role != 'temperature&humidity'"
```

## <a name="next-steps"></a>Další kroky

Když jste se naučili jak toouse iothub-explorer se různé možnosti správy.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
