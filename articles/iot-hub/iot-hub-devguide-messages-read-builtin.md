---
title: "vestavěným koncovým bodem aaaUnderstand hello Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře – popisuje, jak toouse hello integrovaných zpráv typu zařízení cloud prečíst koncový bod kompatibilní s centrem událostí."
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
ms.openlocfilehash: 15484c1b1828151ffcae5f4a1407264374223da1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="read-device-to-cloud-messages-from-hello-built-in-endpoint"></a>Číst zprávy typu zařízení cloud ze vestavěným koncovým bodem hello

Ve výchozím nastavení, zprávy jsou směrované toohello vestavěným koncovým bodem service přístupem (**zprávy nebo události**), který je kompatibilní s [Event Hubs][lnk-event-hubs]. Tento koncový bod je momentálně dostupná jenom v případě použití hello [AMQP] [ lnk-amqp] protokolu na portu 5671. IoT hub zpřístupní hello následující vlastnosti tooenable jste toocontrol hello předdefinované zasílání zpráv koncového bodu kompatibilního s centrem událostí **zprávy nebo události**.

| Vlastnost            | Popis |
| ------------------- | ----------- |
| **Počet oddílů** | Nastavte tuto vlastnost Po vytvoření toodefine hello počtu [oddíly] [ lnk-event-hub-partitions] pro přijímání událostí zařízení cloud. |
| **Doba uchování**  | Tato vlastnost určuje, jak dlouho ve dnech, po které zprávy jsou uchována službou IoT Hub. Výchozí hodnota Hello je jeden den, ale může být vyšší tooseven dnů. |

IoT Hub můžete také skupiny příjemců toomanage na hello integrované zařízení cloud přijímat koncový bod.

Ve výchozím nastavení jsou všechny zprávy, které neodpovídají explicitně pravidel směrování zpráv zapisují toohello vestavěným koncovým bodem. Pokud zakážete toto záložní směrování, jsou zprávy, které neodpovídají explicitně všechna pravidla pro směrování zpráv vyřadit.

Můžete upravit dobu uchování hello, buď prostřednictvím kódu programu prostřednictvím hello [zprostředkovatele prostředků služby IoT Hub rozhraní REST API][lnk-resource-provider-apis], nebo pomocí hello [portál Azure] [ lnk-management-portal].

IoT Hub zpřístupní hello **zprávy nebo události** vestavěným koncovým bodem pro back-end služby přijatých vašeho centra zpráv typu zařízení cloud tooread hello. Tento koncový bod je událost kompatibilní s centrem, což vám umožní toouse žádné služby Event Hubs hello mechanismy hello podporuje pro čtení zpráv.

## <a name="read-from-hello-built-in-endpoint"></a>Čtení z koncového bodu předdefinované hello

Při použití hello [Azure Service Bus SDK pro .NET] [ lnk-servicebus-sdk] nebo hello [Event Hubs - Event Processor Host][lnk-eventprocessorhost], můžete použít jakékoli připojení, IoT Hub řetězce s hello správná oprávnění. Potom pomocí **zprávy nebo události** jako název centra událostí hello.

Při použití sady SDK (nebo Integrace produktu), nebudou o IoT Hub, musíte koncový bod kompatibilní s centrem událostí a název kompatibilní s centrem událostí z nastavení hello IoT Hub v hello [portál Azure] [ lnk-management-portal]:

1. V okně centra IoT hello, klikněte na tlačítko **koncové body**.
1. V hello **předdefinované koncové body** klikněte na tlačítko **události**. Hello okno obsahuje hello následující hodnoty: **koncový bod kompatibilní s centrem událostí**, **název kompatibilní s centrem událostí**, **oddíly**, **dobu uchování**, a **skupiny příjemců**.

    ![Nastavení zařízení cloud][img-eventhubcompatible]

Hello sady SDK centra IoT vyžaduje hello název koncového bodu služby IoT Hub, což je **zprávy nebo události** jak je znázorněno v hello **koncové body** okno.

Pokud vyžaduje hello používáte sady SDK **Hostname** nebo **Namespace** hodnotu, odeberte hello schéma z hello **koncový bod kompatibilní s centrem událostí**. Například, pokud je váš koncový bod kompatibilní s centrem událostí **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**, hello **Hostname** by  **iothub-ns-myiothub-1234.servicebus.windows.net**a hello **Namespace** by **iothub-ns-myiothub-1234**.

Pak můžete použít všechny zásady sdíleného přístupu, který má hello **ServiceConnect** toohello tooconnect oprávnění zadaný centra událostí.

Pokud potřebujete toobuild připojovací řetězec Centru událostí pomocí hello předchozí informace, použijte následující vzor hello:

`Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}`

Hello sady SDK a integrace, které můžete použít s koncovými body kompatibilní s centrem událostí, které IoT Hub zpřístupní zahrnuje hello položky v hello následující seznamu:

* [Java Event Hubs klienta](https://github.com/hdinsight/eventhubs-client).
* [Apache Storm spout](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md). Můžete zobrazit hello [spout zdroje](https://github.com/apache/storm/tree/master/external/storm-eventhubs) na Githubu.
* [Apache Spark integrace](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md).

## <a name="next-steps"></a>Další kroky

Další informace o koncové body centra IoT najdete v tématu [koncové body centra IoT][lnk-endpoints].

Hello [Začínáme] [ lnk-get-started] kurzy vám ukážou, jak zpráv typu zařízení cloud toosend z simulované zařízení a čtení zpráv hello z vestavěným koncovým bodem hello. Další podrobnosti naleznete v hello [zpráv typu zařízení cloud proces IoT Hub pomocí trasy] [ lnk-d2c-tutorial] kurzu.

Pokud chcete, aby tooroute vaše zařízení cloud zprávy toocustom koncové body, najdete v části [použít vlastní koncové body a směrování zpráv pro zprávy typu zařízení cloud][lnk-custom].

[img-eventhubcompatible]: ./media/iot-hub-devguide-messages-read-builtin/eventhubcompatible.png

[lnk-custom]: iot-hub-devguide-messages-read-custom.md
[lnk-get-started]: iot-hub-get-started.md
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-management-portal]: https://portal.azure.com
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-event-hub-partitions]: ../event-hubs/event-hubs-features.md#partitions
[lnk-servicebus-sdk]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-eventprocessorhost]: http://blogs.msdn.com/b/servicebus/archive/2015/01/16/event-processor-host-best-practices-part-1.aspx
[lnk-amqp]: https://www.amqp.org/
