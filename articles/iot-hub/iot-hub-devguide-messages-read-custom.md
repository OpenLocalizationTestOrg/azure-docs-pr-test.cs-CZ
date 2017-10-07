---
title: "Vlastní koncové body Azure IoT Hub aaaUnderstand | Microsoft Docs"
description: "Příručka vývojáře - použití směrování pravidla tooroute zpráv typu zařízení cloud toocustom koncové body."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.openlocfilehash: daa9cfb35d0853e316bbf469b034d4dadbd4e85d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-message-routes-and-custom-endpoints-for-device-to-cloud-messages"></a>Použít vlastní koncové body a směrování zpráv pro zprávy typu zařízení cloud

Centrum IoT vám umožní tooroute [zpráv typu zařízení cloud] [ lnk-device-to-cloud] tooIoT centra služby přístupem koncových bodů podle vlastnosti zprávy. Směrování pravidla udělení hello flexibilitu toosend zprávy, které potřebují toogo bez hello potřebovat pro další služby tooprocess zprávy nebo toowrite další kód. Každé pravidlo směrování, která můžete konfigurovat má hello následující vlastnosti:

| Vlastnost      | Popis |
| ------------- | ----------- |
| **Název**      | Hello jedinečný název, který identifikuje pravidlo hello. |
| **Zdroj**    | Hello původu dat hello stream toobe reagovali na ni. Například zařízení telemetrie. |
| **Podmínka** | Hello výraz dotazu pro hello směrování pravidlo, které běží na uvítací zprávu hlavičky a text a použít toodetermine, zda je shoda pro koncový bod hello. Další informace o vytváření podmínku trasy, najdete v části hello [odkaz - dotazovacího jazyka pro úlohy a dvojčata zařízení][lnk-devguide-query-language]. |
| **Koncový bod**  | Název Hello hello koncového bodu, kde IoT Hub odešle zprávy, které vyhovují podmínce hello. Koncové body musí být v hello stejné oblasti jako hello IoT hub, jinak může podléhat pro zapíše mezi oblastmi. |

Do jedné zprávy může odpovídat na více pravidel směrování, ve kterých případ IoT Hub zajišťuje hello toohello koncový bod zprávy spojené s každou odpovídající pravidlo podmínku hello. Centrum IoT také automaticky deduplicates doručení zpráv, takže pokud zpráva odpovídá více pravidel, které mají hello stejný cíl pouze zapsané toothat cílové jednou.

Centrum IoT má výchozí [vestavěným koncovým bodem][lnk-built-in]. Můžete vytvořit vlastní koncové body tooroute zprávy tooby propojování s jinými službami v centru toohello předplatné. IoT Hub aktuálně podporuje Event Hubs, fronty Service Bus a témat sběrnice Service Bus jako vlastní koncové body.

> [!WARNING]
> Fronty sběrnice a témata s **relací** nebo **duplicitní detekce** povoleno nejsou podporovány jako vlastní koncové body.

Další informace o vytváření vlastní koncové body v centru IoT najdete v tématu [koncové body centra IoT][lnk-devguide-endpoints].

Další informace o čtení ze vlastní koncové body najdete v tématu:

* Čtení z [služby Event Hubs][lnk-getstarted-eh].
* Čtení z [fronty Service Bus][lnk-getstarted-queue].
* Čtení z [témat sběrnice Service Bus][lnk-getstarted-topic].

### <a name="next-steps"></a>Další kroky

Další informace o koncové body centra IoT najdete v tématu [koncové body centra IoT][lnk-devguide-endpoints].

Další informace o hello dotazovací jazyk použít pravidla směrování toodefine, najdete v části [IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv][lnk-devguide-query-language].

Hello [zpráv typu zařízení cloud proces IoT Hub pomocí trasy] [ lnk-d2c-tutorial] kurz ukazuje, jak směrování toouse pravidla a vlastní koncové body.

[lnk-built-in]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-to-cloud]: iot-hub-devguide-messages-d2c.md
[lnk-devguide-query-language]: iot-hub-devguide-query-language.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-getstarted-eh]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-getstarted-queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[lnk-getstarted-topic]: ../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md
