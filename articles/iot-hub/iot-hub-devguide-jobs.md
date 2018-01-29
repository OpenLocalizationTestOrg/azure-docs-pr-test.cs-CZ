---
title: "Pochopení úlohy Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře - plánování úloh spouštět na několika zařízeních připojení do služby IoT hub. Úlohy můžete aktualizovat značky a požadované vlastnosti a volat přímé metody na několika zařízeních."
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
ms.date: 10/24/2017
ms.author: juanpere
ms.openlocfilehash: f90ecb70ad12ed05d5d40f8b26a0a4e461c9f835
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/26/2017
---
# <a name="schedule-jobs-on-multiple-devices"></a>Plánování úloh na několika zařízeních

Azure IoT Hub umožňuje počet stavební bloky jako [zařízení twin vlastnosti a značky] [ lnk-twin-devguide] a [přímé metody][lnk-dev-methods].  Back-end aplikace obvykle povolit operátory a Správci zařízení k aktualizaci a komunikovat se zařízeními IoT hromadně a v naplánovaném čase.  Úlohy provést aktualizace twin zařízení a přímé metody oproti sadě zařízení v naplánovaném čase.  Operátor byste například použili back-end aplikace, který iniciuje a sleduje úlohu restartovat sadu zařízení při vytváření 43 a podlaží 3 v čase, který nebude rušivý pro operace vytvoření.

Zvažte použití úlohy když potřebujete plánovat a sledovat průběh některý z následujících aktivit na skupiny zařízení:

* Aktualizace požadovaných vlastností
* Aktualizace značky
* Vyvolání metody přímé

## <a name="job-lifecycle"></a>Životní cyklus úlohy
Úlohy jsou iniciovaná back-end řešení a udržované pomocí služby IoT Hub.  Můžete spustit úlohu pomocí identifikátoru URI služby přístupem (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-11-14`) a dotaz na průběh na úlohu prováděné prostřednictvím identifikátoru URI služby přístupem (`{iot hub}/jobs/v2/<jobId>?api-version=2016-11-14`). K aktualizaci stavu spuštěných úloh, jakmile se zahájí úlohu, spusťte dotaz úlohy.

> [!NOTE]
> Při zahájení úlohy, názvů a hodnot vlastností mohou obsahovat pouze US-ASCII tisknutelná alfanumerické znaky, s výjimkou těch následující sady: `$ ( ) < > @ , ; : \ " / [ ] ? = { } SP HT`.

## <a name="jobs-to-execute-direct-methods"></a>Úlohy provést přímý metody
Následující fragment kódu ukazuje podrobnosti požadavku HTTPS 1.1 pro provádění [přímá metoda] [ lnk-dev-methods] na sadu zařízení pomocí úlohy:

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

Podmínka dotazu může být také na ID jednoho zařízení nebo na seznam zařízení, ID, jak je znázorněno v následujících příkladech:

```
queryCondition = "deviceId = 'MyDevice1'"
queryCondition = "deviceId IN ['MyDevice1','MyDevice2']"
queryCondition = "deviceId IN ['MyDevice1']
```
[IoT Hub dotazovací jazyk] [ lnk-query] obsahuje dotazovací jazyk IoT Hub v další podrobnosti.

## <a name="jobs-to-update-device-twin-properties"></a>Úlohy aktualizace twin vlastnosti zařízení
Následující fragment kódu ukazuje podrobnosti požadavku HTTPS 1.1 pro aktualizace zařízení dvojici vlastností pomocí úlohy:

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

## <a name="querying-for-progress-on-jobs"></a>Dotaz na průběh na úlohách
Následující fragment kódu ukazuje podrobnosti požadavku HTTPS 1.1 pro [dotazování pro úlohy][lnk-query]:

    GET /jobs/v2/query?api-version=2016-11-14[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

Je k dispozici continuationToken z odpovědi.  

## <a name="jobs-properties"></a>Vlastnosti úlohy
Následující seznam obsahuje vlastnosti a odpovídající popisy, které se dají použít při dotazování pro úlohy nebo výsledky úlohy.

| Vlastnost | Popis |
| --- | --- |
| **jobId** |Aplikace zadat ID pro úlohu. |
| **čas spuštění** |Aplikace zadat čas spuštění (ISO-8601) pro úlohu. |
| **čas ukončení** |Centrum IoT k dispozici po dokončení úlohy datum (ISO-8601). Platné jenom po úlohu dosáhne stavu 'dokončení'. |
| **Typ** |Typy úloh: |
| | **scheduledUpdateTwin**: úlohu použít k aktualizaci sadu požadované vlastnosti a značky. |
| | **scheduledDeviceMethod**: úloha používá k volání metody zařízení na sadu dvojčata zařízení. |
| **Stav** |Aktuální stav úlohy. Možné hodnoty pro stav: |
| | **Čekající**: naplánováno a čeká se na být zachyceny pomocí služby úlohy. |
| | **naplánované**: naplánováno na dobu v budoucnu. |
| | **spuštění**: aktuálně aktivní úlohy. |
| | **došlo ke zrušení**: Úloha byla zrušena. |
| | **se nezdařilo**: Nepodařilo se provést úlohu. |
| | **Dokončit**: Úloha byla dokončena. |
| **deviceJobStatistics** |Statistika týkající se spouštění úlohy. |
| | **deviceJobStatistics** vlastnosti: |
| | **deviceJobStatistics.deviceCount**: počet zařízení pro úlohu. |
| | **deviceJobStatistics.failedCount**: počet zařízení, kde úloha se nezdařila. |
| | **deviceJobStatistics.succeededCount**: počet zařízení, kde byla úloha úspěšná. |
| | **deviceJobStatistics.runningCount**: počet zařízení, které jsou aktuálně spuštěné úlohy. |
| | **deviceJobStatistics.pendingCount**: počet zařízení, které čekají na vyřízení. Pokud chcete spustit úlohu. |

### <a name="additional-reference-material"></a>Odkaz na další materiály
Další témata referenční příručka vývojáře IoT Hub patří:

* [Koncové body centra IoT] [ lnk-endpoints] popisuje různé koncových bodů, které každý IoT hub zpřístupní pro spuštění a management operace.
* [Omezování a kvóty] [ lnk-quotas] popisuje kvóty, které platí pro službu IoT Hub a omezení chování se očekává při použití služby.
* [Azure IoT zařízení a služby sady SDK] [ lnk-sdks] uvádí různé jazykové sady SDK můžete použít při vývoji aplikace zařízení a služby, které interakci s centrem IoT.
* [IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv] [ lnk-query] popisuje dotazovací jazyk Centrum IoT, můžete použít k načtení informací ze služby IoT Hub o úlohách a dvojčata zařízení.
* [Podpora IoT Hub MQTT] [ lnk-devguide-mqtt] poskytuje další informace o podpoře služby IoT Hub pro protokol MQTT.

## <a name="next-steps"></a>Další kroky
Pokud chcete vyzkoušet některé konceptů popsaných v tomto článku, může zajímat v následujícím kurzu IoT Hub:

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
