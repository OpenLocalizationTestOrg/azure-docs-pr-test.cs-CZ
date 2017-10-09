---
title: "Možnosti aaaAzure IoT Hub cloud zařízení | Microsoft Docs"
description: "Příručka vývojáře – pokyny k při toouse přímé metody, dvojče zařízení požadované vlastnosti nebo zprávy typu cloud zařízení pro komunikaci typu cloud zařízení."
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: 1ac90923-1edf-4134-bbd4-77fee9b68d24
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: elioda
ms.openlocfilehash: bb95445054fa2711e34fc1d928c3665e0246c81c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="cloud-to-device-communications-guidance"></a>Pokyny komunikace cloud zařízení
Služba IoT Hub zajišťuje tři možnosti zařízení aplikace tooexpose funkce tooa back-end aplikace:

* [Přímé metody] [ lnk-methods] pro komunikaci, které vyžadují okamžitou potvrzení hello výsledku. Přímé metody se často používají pro interaktivní kontroly zařízení, například zapnutí ventilátoru.
* [Twin je potřeby vlastnosti] [ lnk-twins] pro dlouhodobé příkazy určené tooput hello zařízení do určitého požadovaného stavu. Například nastavte minut too30 určený v intervalu odeslání telemetrie hello.
* [Zprávy typu cloud zařízení] [ lnk-c2d] pro aplikaci zařízení toohello jednosměrný oznámení.

Zde je podrobné porovnání hello různé možnosti komunikace cloud zařízení.

|  | Přímé metody | Požadované vlastnosti pro dvojici | Zprávy typu cloud zařízení |
| ---- | ------- | ---------- | ---- |
| Scénář | Příkazy, které vyžadují okamžitou potvrzení, například zapnutí ventilátoru. | Dlouho běžící příkazy určené tooput hello zařízení do určité požadovaného stavu. Například nastavte minut too30 určený v intervalu odeslání telemetrie hello. | Jednosměrné oznámení toohello zařízení aplikace. |
| Tok dat | Obousměrné. aplikace Hello zařízení může reagovat toohello metoda hned. back-end Hello řešení obdrží hello výsledek kontextově toohello požadavku. | Jednosměrná. aplikace Hello zařízení obdrží oznámení s změnu vlastnosti hello. | Jednosměrná. aplikace Hello zařízení obdrží zprávu hello
| Stálost | Odpojené zařízení nejsou kontaktovat. back-end Hello řešení je oznámeno, že toto zařízení hello není připojený. | Hodnoty vlastností v hello dvojče zařízení zůstanou zachovány. Zařízení se budou číst na další opětovné připojení. Hodnoty vlastností se dá načíst pomocí hello [IoT Hub dotazovací jazyk][lnk-query]. | Zprávy mohou být uchovávány službou IoT Hub pro too48 hodin. |
| Cíle | Pomocí jednoho zařízení **deviceId**, nebo více zařízení prostřednictvím [úlohy][lnk-jobs]. | Pomocí jednoho zařízení **deviceId**, nebo více zařízení prostřednictvím [úlohy][lnk-jobs]. | Jedno zařízení podle **deviceId**. |
| Velikost | Až too8KB požadavky a odpovědi 8KB. | Maximální požadované vlastnosti velikost je 8 KB. | Až too64KB zprávy. |
| frekvence | Vysoká. Další informace najdete v tématu [IoT Hub omezuje][lnk-quotas]. | Střední. Další informace najdete v tématu [IoT Hub omezuje][lnk-quotas]. | Nízká. Další informace najdete v tématu [IoT Hub omezuje][lnk-quotas]. |
| Protocol (Protokol) | Aktuálně dostupná jenom při použití MQTT. | Aktuálně dostupná jenom při použití MQTT. | K dispozici ve všech protokolů. Zařízení se musí dotazovat při použití protokolu HTTP. |

Zjistěte, jak toouse přímé metody, požadované vlastnosti a zprávy typu cloud zařízení v hello následující kurzy:

* [Používat přímé metody][lnk-methods-tutorial], pro direct metody;
* [Použití zařízení tooconfigure požadované vlastnosti][lnk-twin-properties]pro dvojče zařízení je potřeby vlastnosti; 
* [Odesílání zpráv typu cloud zařízení][lnk-c2d-tutorial], pro zprávy typu cloud zařízení.

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-tutorial]: iot-hub-node-node-c2d.md
