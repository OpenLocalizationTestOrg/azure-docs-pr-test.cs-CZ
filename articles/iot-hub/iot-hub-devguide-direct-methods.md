---
title: "metody přímé aaaUnderstand Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře - použít přímé metody tooinvoke kód na zařízení z aplikace služby."
services: iot-hub
documentationcenter: .net
author: nberdy
manager: timlt
editor: 
ms.assetid: 9f0535f1-02e6-467a-9fc4-c0950702102d
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: nberdy
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0d15d44a0c3e1d1cda1669c1ed011c2f932e3d92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-and-invoke-direct-methods-from-iot-hub"></a>Rady pro pochopení a volat přímé metody ze služby IoT Hub
## <a name="overview"></a>Přehled
Centrum IoT vám dává možnost tooinvoke přímé metody na zařízení z cloudu hello. Přímé metody představují požadavku a odpovědi interakci s zařízení podobné tooan HTTP volání v, že jejich úspěch nebo neúspěch okamžitě (po časový limit definované uživatelem). To je užitečné pro scénáře, kde se liší v závislosti na tom, jestli zařízení hello byl schopný toorespond, například odeslání zařízení s SMS funkce wake-up tooa, pokud je zařízení offline (Probíhá dražší než volání metody SMS) během hello okamžitý zásah.

Každá metoda zařízení cílí jedno zařízení. [Úlohy] [ lnk-devguide-jobs] poskytují způsob, jak tooinvoke přímé metody na několika zařízeních a naplánovat volání metody pro odpojené zařízení.

Každý, kdo má **služba připojit** oprávnění na IoT Hub může vyvolat metodu na zařízení.

### <a name="when-toouse"></a>Když toouse
Přímé metody postupujte podle vzoru požadavků a odpovědí a jsou určené pro komunikaci, které vyžadují okamžitou potvrzení jejich výsledku, obvykle interaktivní kontrolu nad hello zařízení, například tooturn na ventilátoru.

Odkazovat příliš[Cloud zařízení komunikace pokyny] [ lnk-c2d-guidance] v případě pochybností mezi použitím požadované vlastnosti přímé metody nebo zprávy typu cloud zařízení.

## <a name="method-lifecycle"></a>Životní cyklus – metoda
Přímé metody jsou definovány pro hello zařízení a může vyžadovat nula nebo více vstupů v hello metoda datové části toocorrectly vytvoření instance. Vyvolání přímá metoda prostřednictvím identifikátoru URI služby přístupem (`{iot hub}/twins/{device id}/methods/`). Zařízení obdrží přímé metod v tématu MQTT konkrétní zařízení (`$iothub/methods/POST/{method name}/`). Podporujeme může přímé metody dalších síťových protokolech straně zařízení v budoucí hello.

> [!NOTE]
> Při vyvolání přímá metoda na zařízení, názvů a hodnot vlastností mohou obsahovat pouze US-ASCII tisknutelná alfanumerické znaky, s výjimkou těch hello následující sady: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.
> 
> 

Přímé metody jsou synchronní a buď úspěšné nebo neúspěšné po hello časový limit (výchozí: 30 sekund, nastavit až too3600 sekund). Přímé metody jsou užitečné v interaktivní scénářích, kde chcete zařízení tooact jenom v případě hello zařízení je online a přijímající příkazy, například zapnutí light z telefonního čísla. V těchto scénářích chcete toosee okamžité úspěšné nebo neúspěšné, takže hello cloudové služby může fungovat pro výsledek hello co nejdříve. Hello zařízení může vrátit některé tělo zprávy v důsledku hello metoda, ale není nutné u hello metoda toodo tak. Neexistuje žádná záruka, řazení v nebo všechny souběžnosti sémantiky pro volání metody.

Přímá metoda jsou ze strany cloudu hello jen HTTP a MQTT jen ze strany zařízení hello.

datová část Hello metoda požadavky a odpovědi je dokument JSON až too8KB.

## <a name="reference-topics"></a>Témata odkazů:
Hello následující odkazy na témata poskytují další informace o používání přímé metody.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>Volání metody přímé z back-end aplikace
### <a name="method-invocation"></a>Volání metody
Přímá metoda volání na zařízení jsou volání protokolu HTTP, které tvoří:

* Hello *URI* konkrétní toohello zařízení (`{iot hub}/twins/{device id}/methods/`)
* Hello POST *– metoda*
* *Hlavičky* který obsahovat hello autorizace, žádat o ID, typu obsahu a kódování obsahu
* Transparentní JSON *textu* v hello následující formát:

```
{
    "methodName": "reboot",
    "responseTimeoutInSeconds": 200,
    "payload": {
        "input1": "someInput",
        "input2": "anotherInput"
    }
}
```

Časový limit je počet sekund. Pokud není nastavený časový limit, nastaví se jako výchozí too30 sekund.

### <a name="response"></a>Odpověď
Hello back-end aplikace obdrží odpověď, která zahrnuje:

* *Stavový kód HTTP*, který se používá pro chyby pocházejících z hello IoT Hub, včetně chybu 404 pro zařízení není aktuálně připojený
* *Hlavičky* který obsahovat hello značka ETag, žádat o ID, typu obsahu a kódování obsahu
* JSON *textu* v hello následující formát:

```
{
    "status" : 201,
    "payload" : {...}
}
```

   Obě `status` a `body` jsou poskytovány hello zařízení a použít toorespond s hello zařízení vlastní stavový kód a popis.

## <a name="handle-a-direct-method-on-a-device"></a>Popisovač přímá metoda na zařízení
### <a name="method-invocation"></a>Volání metody
Zařízení obdrží přímá metoda požadavky na hello MQTT tématu:`$iothub/methods/POST/{method name}/?$rid={request id}`

tělo zprávy Hello které hello zařízení obdrží je ve formátu hello:

```
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

Požadavky metod jsou QoS 0.

### <a name="response"></a>Odpověď
Hello zařízení odesílá odpovědí příliš`$iothub/methods/res/{status}/?$rid={request id}`, kde:

* Hello `status` vlastnost je hello stav spuštění metody zadané zařízení.
* Hello `$rid` vlastnost je ID žádosti hello z volání metody hello přijatých ze služby IoT Hub.

textu Hello je nastaven podle hello zařízení a může být jakékoli stavu.

## <a name="additional-reference-material"></a>Odkaz na další materiály
Další témata referenční příručka vývojáře pro službu IoT Hub hello patří:

* [Koncové body centra IoT] [ lnk-endpoints] popisuje hello různých koncových bodů, které každý IoT hub zpřístupní pro spuštění a management operace.
* [Omezování a kvóty] [ lnk-quotas] popisuje hello kvóty, které se vztahují toohello služby IoT Hub a hello omezení tooexpect chování při použití služby hello.
* [Azure IoT zařízení a služby sady SDK] [ lnk-sdks] seznamy hello různé jazykové sady SDK můžete použít při vývoji aplikace zařízení a služby, které interakci s centrem IoT.
* [IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv] [ lnk-query] popisuje hello dotazovací jazyk IoT Hub můžete tooretrieve informací ze služby IoT Hub o úlohách a dvojčata zařízení.
* [Podpora IoT Hub MQTT] [ lnk-devguide-mqtt] poskytuje další informace o podpoře služby IoT Hub pro protokol MQTT hello.

## <a name="next-steps"></a>Další kroky
Nyní jste se naučili, jak toouse přímé metody, vás může zajímat hello následující tématu Průvodce vývojáře IoT Hub:

* [Plánování úloh na několika zařízeních][lnk-devguide-jobs]

Pokud chcete tootry některé z hello konceptů popsaných v tomto článku, může být zájem o hello následující kurzu IoT Hub:

* [Používat přímé metody][lnk-methods-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-devguide-messages]: iot-hub-devguide-messaging.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
