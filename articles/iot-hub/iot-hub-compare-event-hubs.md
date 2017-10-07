---
title: tooAzure aaaCompare Azure IoT Hub Event Hubs | Microsoft Docs
description: "Porovnání hello IoT Hub a zvýraznění určité rozdíly a případy použití služby Event Hubs Azure. porovnání Hello obsahuje podporované protokoly, správu zařízení, monitorování, a nahrávání souborů."
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: aeddea62-8302-48e2-9aad-c5a0e5f5abe9
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: elioda
ms.openlocfilehash: e5f546b54e29860498d540abfc86a41c4662c0df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="comparison-of-azure-iot-hub-and-azure-event-hubs"></a>Porovnání Azure IoT Hub a Azure Event Hubs
Jedním z případů hello se využívá pro IoT Hub je toogather telemetrie ze zařízení. Z tohoto důvodu IoT Hub je často porovnání příliš[Azure Event Hubs][Azure Event Hubs]. Jako IoT Hub Event Hubs je služba, která umožňuje příjem příchozích dat událostí a telemetrie zpracování událostí toohello cloudu v masivním měřítku, s nízkou latencí a vysokou spolehlivostí.

Hello služby však mít mnoho rozdíly, které jsou popsané v následující tabulce hello:

| Oblast | IoT Hub | Event Hubs |
| --- | --- | --- |
| Vzory komunikace | Umožňuje [komunikace zařízení cloud] [ lnk-d2c-guidance] (zasílání zpráv, souborů nahrávání a hlášené vlastnosti) a [cloud zařízení komunikace] [ lnk-c2d-guidance] (přímé metody, požadované vlastnosti, zasílání zpráv). |Umožňuje pouze příchozí události (obvykle považovány za pro scénáře zařízení cloud). |
| Informace o stavu zařízení | [Dvojčata zařízení] [ lnk-twins] můžete ukládat a dotaz na informace o stavu zařízení. | Mohou být uloženy žádné informace o stavu zařízení. |
| Podpora protokolu zařízení |Podporuje MQTT, MQTT přes Websocket, AMQP, AMQP prostřednictvím technologie WebSockets a HTTP. Kromě toho IoT Hub funguje s hello [brány protokolu Azure IoT][lnk-azure-protocol-gateway], přizpůsobit protokolu brány implementace toosupport vlastní protokoly. |Podporuje AMQP, AMQP prostřednictvím technologie WebSockets a HTTP. |
| Zabezpečení |Poskytuje identitu na zařízení a řízení přístupu odvolatelné. V tématu hello [zabezpečení části hello Příručka vývojáře pro službu IoT Hub]. |Poskytuje celou centra událostí [sdílené zásady přístupu][Event Hubs - security], s omezenou odvolání podporují pomocí [zásad vydavatele][Event Hubs publisher policies]. Řešení IoT jsou často přihlašovací údaje požadované tooimplement vlastní řešení toosupport podle zařízení a ochranu proti falšování míry. |
| Monitorování operací |Umožňuje IoT řešení toosubscribe tooa bohatou sadu událostí správy a připojení identity zařízení například chybám při ověřování jednotlivých zařízení, omezení a výjimky chybný formát. Tyto události umožňují tooquickly identifikovat potíže s připojením k na úrovni jednotlivých zařízení hello. |Zpřístupní jenom agregační metriky. |
| Měřítko |Je optimalizované toosupport miliony současně připojených zařízení. |Měřidla hello připojení dle [kvóty Azure Event Hubs][Azure Event Hubs quotas]. Na dobrý den druhé straně, Event Hubs vám umožní toospecify hello oddílu pro každou zprávu odeslat. |
| Sady SDK zařízení |Poskytuje [sady SDK pro zařízení] [ Azure IoT SDKs] pro širokou škálu platformy a jazyky, přidání toodirect MQTT, AMQP a rozhraní API HTTP. |Je podporován v rozhraní .NET, Java a C, v přidání tooAMQP a odesílání rozhraní HTTP. |
| Nahrávání souborů |Povolí soubory tooupload řešení IoT z cloudu toohello zařízení. Obsahuje koncový bod souborové oznámení pro pracovní postup integrace a operace monitorování kategorie pro ladění podpory. | Není podporováno. |
| Směrování zprávy toomultiple koncové body | Až too10 jsou podporovány vlastní koncové body. Pravidla určují, jakým způsobem se koncové body směrované toocustom zprávy. Další informace najdete v tématu [odesílat a přijímat zprávy službou IoT Hub][lnk-devguide-messaging]. | Vyžaduje další kód toobe zapsané a hostované pro odeslání zpráv. |

V souhrnu i když je případ použití pouze hello příchozí telemetrická data zařízení cloud, IoT Hub poskytuje službu, která je určená pro připojení zařízení IoT. Pokračuje tooexpand hello funkce pro tyto scénáře pomocí funkce specifické pro IoT. Služba Event Hubs je určená pro příjem příchozích dat událostí v masivním měřítku, i v kontextu hello scénářů mezi datovým centrem a intra-datacenter.

Není neobvyklé toouse IoT Hub a Event Hubs ve stejném řešení hello. Komunikace zařízení cloud hello zpracovává IoT Hub a Event Hubs zpracovává události v pozdější fázi příchozího do stroji pro zpracování v reálném čase.

### <a name="next-steps"></a>Další kroky
toolearn Další informace o plánování nasazení služby IoT Hub, najdete v části [škálování, HA a zotavení po Havárii][lnk-scaling].

toofurther prozkoumat hello služby IoT Hub, najdete v tématu:

* [Příručka vývojáře pro službu IoT Hub][lnk-devguide]
* [Simulaci zařízení s hranou IoT][lnk-iotedge]

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[Azure Event Hubs]: ../event-hubs/event-hubs-what-is-event-hubs.md
[zabezpečení části hello Příručka vývojáře pro službu IoT Hub]: iot-hub-devguide-security.md
[Event Hubs - security]: ../event-hubs/event-hubs-authentication-and-security-model-overview.md
[Event Hubs publisher policies]: ../event-hubs/event-hubs-features.md#event-publishers
[Azure Event Hubs quotas]: ../event-hubs/event-hubs-quotas.md
[Azure IoT SDKs]: https://github.com/Azure/azure-iot-sdks
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
