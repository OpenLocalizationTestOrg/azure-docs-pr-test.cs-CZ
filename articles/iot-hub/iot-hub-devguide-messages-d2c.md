---
title: "zasílání zpráv typu zařízení cloud Azure IoT Hub aaaUnderstand | Microsoft Docs"
description: "Příručka vývojáře – jak toouse zařízení cloud zasílání zpráv službou IoT Hub. Obsahuje informace o odesílání telemetrických dat i bez telemtry data a pomocí směrování zpráv toodeliver."
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
ms.openlocfilehash: 07dc8a6be747365c7efbc528ab2762b0d9790758
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="send-device-to-cloud-messages-tooiot-hub"></a>Odesílání zpráv typu zařízení cloud tooIoT rozbočovače

toosend časové řady telemetrie a výstrahy z zařízení tooyour back-end vašeho řešení, odesílání zpráv typu zařízení cloud ze služby IoT hub tooyour zařízení. Informace o dalších zařízení cloud možnosti podporované službou IoT Hub, najdete v části [pokyny komunikace zařízení cloud][lnk-d2c-guidance].

Odesílání zpráv typu zařízení cloud prostřednictvím koncového bodu směřujících zařízení (**/devices/ {deviceId} / zprávy/události**). Pravidla směrování pak směrovat vaše zprávy tooone koncových bodů služby směřujících hello ve službě IoT hub. Pravidla směrování použití hello záhlaví a tělo zprávy typu zařízení cloud hello předávaných mezi vašeho centra toodetermine kde tooroute je. Ve výchozím nastavení, zprávy jsou směrované toohello vestavěným koncovým bodem service přístupem (**zprávy nebo události**), který je kompatibilní s [Event Hubs][lnk-event-hubs]. Proto můžete použít standardní [integrace služby Event Hubs a sady SDK] [ lnk-compatible-endpoint] zpráv typu zařízení cloud tooreceive ve vašem řešení back-end.

IoT Hub implementuje zařízení cloud zasílání zpráv pomocí streamování vzorcem zasílání zpráv. Zprávy typu zařízení cloud IoT Hub se více podobají [Event Hubs] [ lnk-event-hubs] *události* než [Service Bus] [ lnk-servicebus] *zprávy* v, aby nedocházelo k velkému počtu událostí prošla hello služby, který může číst několik čtečky.

Zařízení cloud zasílání zpráv službou IoT Hub má hello následující vlastnosti:

* Zprávy typu zařízení cloud jsou odolné a udržení ve výchozím nastavení služby IoT hub **zprávy nebo události** koncový bod pro až tooseven dnů.
* Zprávy typu zařízení cloud může být maximálně 256 KB a mohou být seskupeny do balíků, které odesílá toooptimize. Dávky může být maximálně 256 KB.
* Jak je popsáno v hello [řízení přístupu tooIoT rozbočovače] [ lnk-devguide-security] oddíl, IoT Hub umožňuje na zařízení ověřování a řízení přístupu.
* Centrum IoT vám umožní toocreate až too10 vlastní koncové body. Koncové body toohello podle trasy nakonfigurované ve službě IoT hub doručování zpráv. Další informace najdete v tématu [pravidla směrování](#routing-rules).
* IoT Hub umožňuje miliony současně připojených zařízení (viz [kvóty a omezení][lnk-quotas]).
* IoT Hub neumožňuje libovolný dělení. Zprávy typu zařízení cloud jsou rozdělena na oddíly na základě jejich zdrojového **deviceId**.

Další informace o hello rozdíly mezi hello IoT Hub a Event Hubs služby najdete v tématu [porovnání služeb Azure IoT Hub a Azure Event Hubs][lnk-comparison].

## <a name="send-non-telemetry-traffic"></a>Odesílat provoz telemetrická data

Často kromě tootelemetry datových bodů, zařízení odesílat zprávy a požadavky, které vyžadují samostatné spuštění a zpracování v back-end hello řešení. Například kritické výstrahy, které musí aktivovat určité akci v hello back-end. Můžete napsat snadno [pravidlo směrování] [ lnk-devguide-custom] toosend tyto typy koncového bodu tooan zprávy vyhrazené tootheir zpracování na základě buď hlavičku na uvítací zprávu nebo hodnotu v textu zprávy hello.

Další informace o hello nejlepší způsob, jak tooprocess tento druh zprávy naleznete v tématu hello [kurz: jak tooprocess zpráv typu zařízení cloud IoT Hub] [ lnk-d2c-tutorial] kurzu.

## <a name="route-device-to-cloud-messages"></a>Směrování zprávy typu zařízení cloud

Máte dvě možnosti pro směrování zpráv typu zařízení cloud tooyour back-end aplikace:

* Použijte integrovanou hello [koncový bod kompatibilní s centrem událostí] [ lnk-compatible-endpoint] tooenable back-end aplikace tooread hello zařízení cloud zprávy přijaté službou hello rozbočovače. toolearn o hello předdefinované kompatibilní s centrem událostí koncový bod, najdete v části [číst zprávy typu zařízení cloud ze vestavěným koncovým bodem hello][lnk-devguide-builtin].
* Použití směrování koncové body toocustom pravidla toosend zpráv ve službě IoT hub. Vlastní koncové body povolit zprávy typu zařízení cloud tooread si back-end aplikace pomocí Event Hubs, fronty sběrnice nebo témata Service Bus. toolearn o směrování a vlastní koncové body, najdete v části [použít vlastní koncové body a pravidla směrování pro zprávy typu zařízení cloud][lnk-devguide-custom].

## <a name="anti-spoofing-properties"></a>Ochranu proti falšování vlastnosti

zařízení tooavoid falšování identity ve zpráv typu zařízení cloud, IoT Hub razítka všechny zprávy s hello následující vlastnosti:

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

Hello první dva obsahovat hello **deviceId** a **generationId** z hello pocházející zařízení, jako za [vlastnosti identity zařízení][lnk-device-properties].

Hello **ConnectionAuthMethod** vlastnost obsahuje objekt serializován do formátu JSON, s hello následující vlastnosti:

```json
{
  "scope": "{ hub | device}",
  "type": "{ symkey | sas}",
  "issuer": "iothub"
}
```

## <a name="next-steps"></a>Další kroky

Informace o hello sady SDK můžete použít zpráv typu zařízení cloud toosend najdete v tématu [SDK služby Azure IoT][lnk-sdks].

Hello [Začínáme] [ lnk-get-started] kurzy vám ukážou, jak toosend zařízení cloud zprávy ze simulovaného a fyzické zařízení. Další podrobnosti naleznete v hello [zpráv typu zařízení cloud proces IoT Hub pomocí trasy] [ lnk-d2c-tutorial] kurzu.

[lnk-devguide-builtin]: iot-hub-devguide-messages-read-builtin.md
[lnk-devguide-custom]: iot-hub-devguide-messages-read-custom.md
[lnk-comparison]: iot-hub-compare-event-hubs.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[lnk-get-started]: iot-hub-get-started.md

[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-servicebus]: http://azure.microsoft.com/documentation/services/service-bus/
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-compatible-endpoint]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
