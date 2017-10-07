---
title: "zasílání zpráv aaaUnderstand Azure IoT Hub cloud zařízení | Microsoft Docs"
description: "Příručka vývojáře – jak toouse cloud zařízení zasílání zpráv službou IoT Hub. Obsahuje informace o životní cyklus zpráv hello a možnosti konfigurace."
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
ms.openlocfilehash: 5c747b50163873d823556a8baa769c4b8f7f8c44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="send-cloud-to-device-messages-from-iot-hub"></a>Odesílání zpráv typu cloud zařízení ze služby IoT Hub

toosend jednosměrný oznámení toohello zařízení aplikaci z back-end vašeho řešení, odesílání zpráv typu cloud zařízení ze zařízení IoT hub tooyour. Informace dalších možností cloudu zařízení podporované službou IoT Hub, naleznete v [Cloud zařízení komunikace pokyny][lnk-c2d-guidance].

Odesílání zpráv typu cloud zařízení prostřednictvím koncového bodu služby přístupem (**/zprávy/devicebound**). Zařízení pak přijímá zprávy hello prostřednictvím koncového bodu konkrétní zařízení (**/devices/ {deviceId} / zprávy/devicebound**).

Každá zpráva cloud zařízení je zaměřený na jedno zařízení podle nastavení hello **k** vlastnost příliš**/devices/ {deviceId} / zprávy/devicebound**.

Každá fronta zařízení obsahuje maximálně 50 zprávy typu cloud zařízení. Při operaci toosend další zprávy toohello stejného zařízení výsledkem chyba.

## <a name="hello-cloud-to-device-message-lifecycle"></a>životní cyklus zpráv typu cloud zařízení Hello

doručení zpráv v aspoň jednou tooguarantee, IoT Hub trvá zpráv typu cloud zařízení ve frontách vázané na zařízení. Zařízení musí explicitně potvrdit *dokončení* pro tooremove IoT Hub je z fronty hello. Tento postup zaručuje odolnost proti připojení a selhání zařízení.

Hello následující obrázek znázorňuje grafu stavu hello životního cyklu zprávy typu cloud zařízení IoT Hub.

![Životní cyklus zpráv typu cloud zařízení][img-lifecycle]

Když hello služby IoT Hub odešle zprávu tooa zařízení, služba hello nastaví stav zprávy hello příliš**zařazených do fronty**. Když chce zařízení příliš*přijímat* zprávu IoT Hub *zámky* uvítací zprávu (nastavením stavu hello příliš**neviditelná**), což umožňuje jiná vlákna na toostart zařízení hello přijetí další zprávy. Po dokončení vlákna zařízení hello zpracování zprávy upozorní IoT Hub pomocí *dokončení* uvítací zprávu. IoT Hub poté nastaví stav hello příliš**dokončeno**.

Zařízení můžete také zvolit:

* *Odmítnout* uvítací zprávu, což způsobí, že tooset IoT Hub je toohello **Deadlettered** stavu. Zařízení, která se připojují přes hello MQTT protokol nelze odmítnout zprávy typu cloud zařízení.
* *Zrušte* uvítací zprávu, což způsobí, že IoT Hub tooput uvítací zprávu zpět ve frontě hello, se stavem hello nastavit příliš**zařazených do fronty**.

Vlákno se možná nepovede tooprocess zprávu bez upozornění služby IoT Hub. V takovém případě zprávy automaticky přechod z hello **neviditelná** stavu zpět toohello **zařazených do fronty** stavu po *časový limit viditelnosti (nebo zámku)*. Výchozí hodnota Hello tohoto limitu je jedna minuta.

Zpráva může přecházet mezi hello **zařazených do fronty** a **neviditelná** stavy, maximálně hello zadaného v hello **maximální počet doručení** vlastnost na IoT Hub. Po tomto počtu přechodů IoT Hub nastaví hello stav uvítací zprávu příliš**Deadlettered**. Podobně se IoT Hub nastaví stav hello zprávy příliš**Deadlettered** po jeho čas vypršení platnosti (najdete v části [čas toolive][lnk-ttl]).

Hello [jak toosend cloud zařízení zpráv službou IoT Hub] [ lnk-c2d-tutorial] se dozvíte, jak toosend zprávy typu cloud zařízení z hello cloud a přijímat na zařízení.

Obvykle se zařízení dokončí zprávy typu cloud zařízení po ztrátě hello uvítací zprávu nemá vliv na logiku aplikace hello. Pokud například zařízení hello obsahuje trvalé hello zpráva obsah místně nebo byl úspěšně proveden operace. Hello zpráva může mít také přechodný informace, jejichž ztrátě nebude mít vliv na funkci hello aplikace hello. V některých případech pro dlouhotrvající úlohy, můžete dokončit uvítací zprávu cloud zařízení po uložením hello popis úlohy v místním úložišti. Potom můžete v různých fázích průběh úlohy hello upozornit hello back-end řešení s jeden nebo více zpráv typu zařízení cloud.

## <a name="message-expiration-time-toolive"></a>Vypršení platnosti zprávy (čas toolive)

Všechny zprávy typu cloud zařízení mají čas vypršení platnosti. Buď službou hello nastavení tato doba (v hello **ExpiryTimeUtc** vlastnost), nebo službou IoT Hub pomocí výchozí hello *čas toolive* zadán jako vlastnost IoT Hub. V tématu [možnosti konfigurace Cloud zařízení][lnk-c2d-configuration].

Běžný způsob, tootake výhod zprávy vypršení platnosti a vyhnout odesílání zpráv toodisconnected zařízení, je tooset toolive hodnoty krátkou dobu. Tuto metodu lze dosáhnout hello stejný výsledek jako zachování hello stav připojení zařízení, aniž by byly efektivnější. Pokud budete požadovat potvrzení zprávy, IoT Hub vás upozorní, zařízení, která jsou možné tooreceive zprávy a zařízení, která nejsou v režimu online nebo se nezdařilo.

## <a name="message-feedback"></a>Zpráva zpětné vazby

Při odeslání zprávy typu cloud zařízení, můžete žádost o služby hello hello doručování zpráv zpětnou vazbu týkající se hello konečného stavu této zprávy.

| Vlastnost objektu ACK. | Chování |
| ------------ | -------- |
| **kladné** | IoT Hub generuje zprávu zpětné vazby, pokud a pouze v případě, hello cloud zařízení zpráva dorazila hello **dokončeno** stavu. |
| **Záporná** | IoT Hub vytvoří zprávu zpětnou vazbu jenom v případě, zprávy typu cloud zařízení hello dosáhne hello **Deadlettered** stavu. |
| **Úplná**     | IoT Hub vytvoří zprávu zpětné vazby v obou případech. |

Pokud **Ack** je **úplné**a jste neobdrželi zprávu zpětné vazby, což znamená, že vypršela platnost zprávy hello zpětnou vazbu. Služba Hello nemůže vědět, co se stalo toohello původní zprávy. V praxi služba zkontrolujte zpětnou vazbu hello ji může zpracovat, než vyprší její platnost. čas vypršení platnosti maximální Hello je dva dny, poskytuje dost času tooget hello spuštěna služba znovu Pokud dojde k chybě.

Jak je popsáno v [koncové body][lnk-endpoints], IoT Hub zajišťuje zpětnou vazbu prostřednictvím koncový bod služby přístupem (**/messages/servicebound/feedback**) jako zprávy. Hello sémantiku pro příjem zpětné vazby jsou hello stejné jako u zprávy typu cloud zařízení a mít stejný hello [životní cyklus zpráv][lnk-lifecycle]. Kdykoli je to možné, zpětná vazba zpráva v dávce do jedné zprávy s hello následující formát:

| Vlastnost     | Popis |
| ------------ | ----------- |
| EnqueuedTime | Časové razítko označující vytvoření uvítací zprávu. |
| ID uživatele       | `{iot hub name}` |
| Typ obsahu  | `application/vnd.microsoft.iothub.feedback.json` |

textu Hello je serializací JSON pole záznamů, každý s hello následující vlastnosti:

| Vlastnost           | Popis |
| ------------------ | ----------- |
| EnqueuedTimeUtc    | Časové razítko označující, kdy výsledek hello hello zprávy došlo. Například hello zařízení bylo dokončeno, nebo jeho platnost uvítací zprávu. |
| OriginalMessageId  | **MessageId** hello zpráv typu cloud zařízení toowhich tato zpětná vazba informace týkají. |
| statusCode         | Požadovaný řetězec. Použít v zpětnou vazbu zprávy generované IoT Hub. <br/> 'Success' <br/> "Platnost vypršela. <br/> 'DeliveryCountExceeded. <br/> 'Odmítnut. <br/> 'Vyprázdní. |
| Popis        | Řetězce hodnoty pro **StatusCode**. |
| ID zařízení           | **DeviceId** hello cílové zařízení z hello zpráv typu cloud zařízení toowhich tento připomínek souvisí. |
| DeviceGenerationId | **DeviceGenerationId** hello cílové zařízení z hello zpráv typu cloud zařízení toowhich tento připomínek souvisí. |

musíte zadat Hello služby **MessageId** pro hello cloud zařízení zpráv možné toocorrelate toobe jeho zpětnou vazbu s hello původní zprávy.

Hello následující příklad ukazuje hello tělo zprávy zpětnou vazbu.

```json
[
  {
    "OriginalMessageId": "0987654321",
    "EnqueuedTimeUtc": "2015-07-28T16:24:48.789Z",
    "StatusCode": 0,
    "Description": "Success",
    "DeviceId": "123",
    "DeviceGenerationId": "abcdefghijklmnopqrstuvwxyz"
  },
  {
    ...
  },
  ...
]
```

## <a name="cloud-to-device-configuration-options"></a>Možnosti konfigurace cloudu na zařízení

Každý IoT hub zpřístupní hello následující možnosti konfigurace pro zasílání zpráv typu cloud zařízení:

| Vlastnost                  | Popis | Rozsah a výchozí |
| ------------------------- | ----------- | ----------------- |
| defaultTtlAsIso8601       | Výchozí hodnota TTL zprávy typu cloud zařízení. | Interval ISO_8601 až too2D (minimální 1 minutu). Výchozí hodnota: 1 hodina. |
| maxDeliveryCount          | Doručení maximální počet pro fronty cloud zařízení na zařízení. | 1 too100. Výchozí: 10. |
| feedback.ttlAsIso8601     | Uchování pro zpětnou vazbu služby vázané zprávy. | Interval ISO_8601 až too2D (minimální 1 minutu). Výchozí hodnota: 1 hodina. |
| feedback.maxDeliveryCount |Doručení maximální počet pro frontu zpětné vazby. | 1 too100. Výchozí: 100. |

Další informace o tom, jak tooset možnosti konfigurace, najdete v části [centra IoT vytvořit][lnk-portal].

## <a name="next-steps"></a>Další kroky

Informace o sadách SDK, hello můžete používat tooreceive zprávy typu cloud zařízení najdete v tématu [SDK služby Azure IoT][lnk-sdks].

tootry na příjem zpráv typu cloud zařízení, najdete v části hello [odeslat cloud zařízení] [ lnk-c2d-tutorial] kurzu.

[img-lifecycle]: ./media/iot-hub-devguide-messages-c2d/lifecycle.png

[lnk-portal]: iot-hub-create-through-portal.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-ttl]: #message-expiration-time-to-live
[lnk-c2d-configuration]: #cloud-to-device-configuration-options
[lnk-lifecycle]: #message-lifecycle
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
