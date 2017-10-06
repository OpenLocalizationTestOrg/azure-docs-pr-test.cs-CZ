---
title: "úlohy Azure IoT Hub aaaUnderstand | Microsoft Docs"
description: "Příručka vývojáře - plánování úloh toorun na několika zařízeních připojen tooyour IoT hub. Úlohy můžete aktualizovat značky a požadované vlastnosti a volat přímé metody na několika zařízeních."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: fe78458f-4f14-4358-ac83-4f7bd14ee8da
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/30/2016
ms.author: juanpere
ms.openlocfilehash: 8be134e6c379feae5087df8f562a74505c57afee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="schedule-jobs-on-multiple-devices"></a>Plánování úloh na několika zařízeních
## <a name="overview"></a>Přehled
Popsané v předchozí články Azure IoT Hub umožňuje počet stavební bloky ([zařízení twin vlastnosti a značky] [ lnk-twin-devguide] a [přímé metody] [ lnk-dev-methods]).  Back-end aplikace obvykle povolit tooupdate operátory a Správci zařízení a interagovat se zařízení IoT hromadně a v naplánovaném čase.  Úlohy pro zapouzdření hello provádění aktualizací twin zařízení a přímé metody oproti sadě zařízení současně plán.  Operátor byste například použili back-end aplikaci, která by zahájení a sledovat úlohy tooreboot skupiny zařízení při vytváření 43 a podlaží 3 v čase, který nebude rušivý toohello operace vytváření hello.

### <a name="when-toouse"></a>Když toouse
Zvažte použití úlohy při: tooschedule potřebám back-end řešení a sledování průběhu některé z následujících aktivit na sadu zařízení hello:

* Aktualizace požadovaných vlastností
* Aktualizace značky
* Vyvolání metody přímé

## <a name="job-lifecycle"></a>Životní cyklus úlohy
Úlohy jsou iniciovaná back-end hello řešení a udržované pomocí služby IoT Hub.  Můžete spustit úlohu pomocí identifikátoru URI služby přístupem (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-11-14`) a dotaz na průběh na úlohu prováděné prostřednictvím identifikátoru URI služby přístupem (`{iot hub}/jobs/v2/<jobId>?api-version=2016-11-14`).  Jakmile je zahájena úlohu, a umožňuje dotazování pro úlohy hello back-end aplikačním toorefresh hello stavu spuštěných úloh.

> [!NOTE]
> Při zahájení úlohy, názvů a hodnot vlastností mohou obsahovat pouze US-ASCII tisknutelná alfanumerické znaky, s výjimkou těch hello následující sady: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.
> 
> 

## <a name="reference-topics"></a>Témata odkazů:
Následující témata s referenčními informacemi Hello poskytují další informace o používání úlohy.

## <a name="jobs-tooexecute-direct-methods"></a>Přímé metody tooexecute úlohy
Hello následuje podrobnosti požadavku hello HTTP 1.1 pro provádění [přímá metoda] [ lnk-dev-methods] na sadu zařízení pomocí úlohy:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-11-14

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleDirectRequest', 
        cloudToDeviceMethod: {
            methodName: '<methodName>',
            payload: <payload>,                 
            responseTimeoutInSeconds: methodTimeoutInSeconds 
        },
        queryCondition: '<queryOrDevices>', // query condition
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        
    }
    ```
Podmínka Hello dotazu může být také na jedno zařízení Id nebo na seznam identifikátorů zařízení jak je uvedeno níže

**Příklady**
```
queryCondition = "deviceId = 'MyDevice1'"
queryCondition = "deviceId IN ['MyDevice1','MyDevice2']"
queryCondition = "deviceId IN ['MyDevice1']
```
[IoT Hub dotazovací jazyk] [ lnk-query] obsahuje dotazovací jazyk IoT Hub v další podrobnosti.

## <a name="jobs-tooupdate-device-twin-properties"></a>Vlastnosti úlohy tooupdate zařízení twin
Hello následuje podrobnosti požadavku hello HTTP 1.1 pro aktualizace zařízení dvojici vlastností pomocí úlohy:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-11-14
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleTwinUpdate', 
        updateTwin: <patch>                 // Valid JSON object
        queryCondition: '<queryOrDevices>', // query condition
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        // format TBD
    }
    ```

## <a name="querying-for-progress-on-jobs"></a>Dotaz na průběh na úlohách
Hello následuje hello podrobnosti požadavku HTTP 1.1 pro [dotazování pro úlohy][lnk-query]:

    ```
    GET /jobs/v2/query?api-version=2016-11-14[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>
    ```

z odpovědi hello je k dispozici Hello continuationToken.  

## <a name="jobs-properties"></a>Vlastnosti úlohy
Hello následuje seznam vlastností a odpovídající popisy, které se dají použít při dotazování pro úlohy nebo výsledky úlohy.

| Vlastnost | Popis |
| --- | --- |
| **jobId** |Aplikace zadat ID pro úlohu hello. |
| **čas spuštění** |Aplikace zadat čas spuštění (ISO-8601) pro úlohu hello. |
| **čas ukončení** |Po dokončení úlohy hello IoT Hub zadat datum (ISO-8601) pro. Platné jenom po hello úlohy dosáhne stavu hello 'Dokončit'. |
| **Typ** |Typy úloh: |
| **scheduledUpdateTwin**: úloha používá tooupdate sadu požadované vlastnosti a značky. | |
| **scheduledDeviceMethod**: úloha používá tooinvoke metoda zařízení na sadu dvojčata zařízení. | |
| **Stav** |Aktuální stav úlohy hello. Možné hodnoty pro stav: |
| **Čekající** : naplánováno a čekací toobe zachyceny službou hello úlohy. | |
| **naplánované** : naplánováno na dobu v budoucnu hello. | |
| **spuštění** : aktuálně aktivní úlohy. | |
| **zrušena** : Úloha byla zrušena. | |
| **se nezdařilo** : Nepodařilo se provést úlohu. | |
| **Dokončit** : Úloha byla dokončena. | |
| **deviceJobStatistics** |Statistika týkající se provádění úlohy hello. |

**deviceJobStatistics** vlastnosti.

| Vlastnost | Popis |
| --- | --- |
| **deviceJobStatistics.deviceCount** |Počet zařízení v úloze hello. |
| **deviceJobStatistics.failedCount** |Počet zařízení, kde hello úloha se nezdařila. |
| **deviceJobStatistics.succeededCount** |Počet zařízení, kde hello úloha úspěšně. |
| **deviceJobStatistics.runningCount** |Počet zařízení, které jsou aktuálně spuštěné úlohy hello. |
| **deviceJobStatistics.pendingCount** |Počet zařízení, kterých se čeká na toorun hello úlohy. |

### <a name="additional-reference-material"></a>Odkaz na další materiály
Další témata referenční příručka vývojáře pro službu IoT Hub hello patří:

* [Koncové body centra IoT] [ lnk-endpoints] popisuje hello různých koncových bodů, které každý IoT hub zpřístupní pro spuštění a management operace.
* [Omezování a kvóty] [ lnk-quotas] popisuje hello kvóty, které se vztahují toohello služby IoT Hub a hello omezení tooexpect chování při použití služby hello.
* [Azure IoT zařízení a služby sady SDK] [ lnk-sdks] seznamy hello různé jazykové sady SDK můžete použití při vývoji aplikace zařízení a služby, které interakci s centrem IoT.
* [IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv] [ lnk-query] popisuje hello dotazovací jazyk IoT Hub můžete tooretrieve informací ze služby IoT Hub o úlohách a dvojčata zařízení.
* [Podpora IoT Hub MQTT] [ lnk-devguide-mqtt] poskytuje další informace o podpoře služby IoT Hub pro protokol MQTT hello.

## <a name="next-steps"></a>Další kroky
Pokud chcete tootry některé z hello konceptů popsaných v tomto článku, může být zájem o hello následující kurzu IoT Hub:

* [Plán a všesměrovým úlohy][lnk-jobs-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-jobs-tutorial]: iot-hub-node-node-schedule-jobs.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-devguide]: iot-hub-devguide-device-twins.md
