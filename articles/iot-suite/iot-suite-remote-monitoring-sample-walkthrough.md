---
title: "aaaRemote monitorování předkonfigurované řešení návod | Microsoft Docs"
description: "Popis hello Azure IoT předkonfigurovaného řešení vzdáleného monitorování a jeho architektura."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 31fe13af-0482-47be-b4c8-e98e36625855
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: 57a336bd94938c2b9ee5d3456ea8e45446cf3d33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="remote-monitoring-preconfigured-solution-walkthrough"></a>Návod pro předkonfigurované řešení vzdáleného monitorování

Hello vzdáleného sledování IoT Suite [předkonfigurované řešení] [ lnk-preconfigured-solutions] je implementace pro kompletní monitorování řešení pro více počítačů spuštěných ve vzdálených umístěních. Hello řešení kombinuje klíčové služby Azure tooprovide obecnou implementaci hello podnikový scénář. Hello řešení můžete použít jako výchozí bod pro vlastní implementaci a [přizpůsobit] [ lnk-customize] ho toomeet specifických podnikových požadavků.

Tento článek vás provede procesem některé klíčové prvky hello tooenable řešení vzdáleného monitorování hello toounderstand jak to funguje. Díky tomu budete moct:

* Řešení problémů v řešení hello.
* Plánování jak toocustomize toohello řešení toomeet vaše konkrétní požadavky. 
* Navrhněte vlastní řešení IoT, které používá služby Azure.

## <a name="logical-architecture"></a>Logická architektura

Hello následující diagram popisuje logické součásti hello hello předkonfigurované řešení:

![Logická architektura](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)

## <a name="simulated-devices"></a>Simulovaná zařízení

V hello předkonfigurované řešení hello simulované zařízení představuje chladicí zařízení (například budově nebo budovy klimatizační jednotku). Když nasadíte hello předkonfigurované řešení, také automaticky zřizovat čtyři Simulovaná zařízení, které běží v [webové úlohy Azure][lnk-webjobs]. Hello simulované zařízení usnadní vám tooexplore hello chování hello řešení bez nutnosti toodeploy hello žádné fyzické zařízení. toodeploy skutečných fyzické zařízení, najdete v části hello [připojit vaše zařízení toohello předkonfigurovanému řešení vzdáleného monitorování] [ lnk-connect-rm] kurzu.

### <a name="device-to-cloud-messages"></a>Zprávy typu zařízení-cloud

Každé simulované zařízení můžete odeslat následující typy tooIoT zprávu centra hello:

| Zpráva | Popis |
| --- | --- |
| Spuštění |Když se spustí hello zařízení, odešle **informace o zařízení** zprávu obsahující informace o samotné toohello back-end. Tato data zahrnují id zařízení hello a seznam příkazů hello a metody hello zařízení podporuje. |
| Přítomnost |Zařízení odesílá **přítomnosti** zprávy tooreport, zda zařízení hello rozpozná hello přítomnost senzoru. |
| Telemetrická data |Zařízení odesílá **telemetrie** zprávu, která hlásí simulované hodnoty pro hello teploty a vlhkosti shromážděných z hello zařízení je simulované senzorů. |

> [!NOTE]
> Hello řešení ukládá hello seznam příkazů hello zařízením v databázi Cosmos DB a není v dvojče zařízení hello podporován.

### <a name="properties-and-device-twins"></a>Vlastnosti a dvojčata zařízení

Hello Simulovaná zařízení odesílají hello následující zařízení vlastnosti toohello [twin] [ lnk-device-twins] hello IoT hub jako *hlášené vlastnosti*. Hello zasílá zařízení hlášené vlastnosti při spuštění a v odpovědi tooa **změny stavu zařízení** příkaz nebo metoda.

| Vlastnost | Účel |
| --- | --- |
| Config.TelemetryInterval | Frekvence (v sekundách) hello zařízení odesílá telemetrii |
| Config.TemperatureMeanValue | Určuje střední hodnotu hello hello simulované teplotní telemetrie |
| Device.DeviceID |ID, které je zadáno nebo přiřazeno při vytvoření zařízení v řešení hello |
| Device.DeviceState | Stav zařízení hello |
| Device.CreatedTime |Čas hello zařízení byl vytvořen v řešení hello |
| Device.StartupTime |Čas hello zařízení byla spuštěna. |
| Device.LastDesiredPropertyChange |Změňte číslo verze Hello hello poslední požadované vlastnosti. |
| Device.Location.Latitude |Zeměpisná šířka umístění zařízení hello |
| Device.Location.Longitude |Zeměpisná délka umístění zařízení hello |
| System.Manufacturer |Výrobce zařízení |
| System.ModelNumber |Číslo modelu zařízení hello |
| System.SerialNumber |Sériové číslo zařízení hello |
| System.FirmwareVersion |Aktuální verzi firmwaru v zařízení hello |
| System.Platform |Architektura platformy zařízení hello |
| System.Processor |Procesor spuštěné hello zařízení |
| System.InstalledRAM |Velikost paměti RAM nainstalované v zařízení hello |

Hello simulátor doplňuje pro tyto vlastnosti v simulovaném zařízení vzorové hodnoty. Pokaždé, když hello simulátor inicializuje simulované zařízení, hello zařízení hlásí hello předem definovaná metadata tooIoT rozbočovače jako hlášené vlastnosti. Hlášené vlastnosti lze aktualizovat pouze pomocí hello zařízení. toochange hlášené vlastnost, nastavte požadovanou vlastnost portálu řešení. Je zodpovědností hello hello zařízení:

1. Pravidelně načíst ze služby IoT hub hello požadované vlastnosti.
2. Hodnotou vlastnosti hello potřeby aktualizaci konfigurace.
3. Odešlete hello novou hodnotu back toohello hub jako hlášené vlastnost.

Z řídicího panelu řešení hello, můžete použít *potřeby vlastnosti* tooset vlastnosti na zařízení s použitím hello [dvojče zařízení][lnk-device-twins]. Obvykle zařízení čte hodnotu požadované vlastnosti z tooupdate hello rozbočovače, jeho vnitřní stav a sestavy hello změní zpět jako hlášené vlastnost.

> [!NOTE]
> Hello kód simulované zařízení používá jenom hello **Desired.Config.TemperatureMeanValue** a **Desired.Config.TelemetryInterval** požadované vlastnosti tooupdate hello hlášené vlastnosti odeslána zpět tooIoT rozbočovače. V hello simulované zařízení jsou ignorovány všechny ostatní žádosti o změnu požadovanou vlastnost.

### <a name="methods"></a>Metody

Hello Simulovaná zařízení mohou zpracovávat následující metody hello ([přímé metody][lnk-direct-methods]) volat z portálu řešení hello prostřednictvím hello IoT hub:

| Metoda | Popis |
| --- | --- |
| InitiateFirmwareUpdate |Dá pokyn hello zařízení tooperform aktualizaci firmwaru |
| Restartování |Dá pokyn tooreboot zařízení hello |
| FactoryReset |Dá pokyn tooperform hello zařízení resetovat výrobní nastavení |

Některé metody použijte hlášené vlastnosti tooreport na průběh. Například hello **InitiateFirmwareUpdate** metoda simuluje spuštěné hello aktualizaci asynchronně na hello zařízení. Hello metoda vrátí okamžitě na zařízení hello pomocí řídicího panelu řešení toohello zatímco hello asynchronní úkol stále toosend aktualizací stavu zpět hlášené vlastnosti.

### <a name="commands"></a>Příkazy

Hello simulované zařízení mohou zpracovávat následující příkazy (zpráv typu cloud zařízení), odeslané ze hello portál řešení prostřednictvím centra IoT hello hello:

| Příkaz | Popis |
| --- | --- |
| PingDevice |Odešle *ping* toocheck toohello zařízení je zachování připojení |
| StartTelemetry |Spustí hello zařízení odesílat telemetrii |
| StopTelemetry |Zastaví odesílání telemetrických dat zařízením hello |
| ChangeSetPointTemp |Hodnotu bodu hello změny kolem které hello se vytvářejí náhodná data |
| DiagnosticTelemetry |Aktivační události hello toosend simulátoru zařízení další telemetrické hodnoty (externalTemp) |
| ChangeDeviceState |Změní rozšířené stavové vlastnosti zařízení hello a odešle zprávu s informacemi o hello ze zařízení hello |

> [!NOTE]
> Porovnání těchto příkazů (zpráv typu cloud-zařízení) a metod (přímých metod) najdete v [doprovodných materiálech ke komunikaci typu cloud-zařízení][lnk-c2d-guidance].

## <a name="iot-hub"></a>IoT Hub

Hello [služby IoT hub] [ lnk-iothub] ingestuje dat odesílaných ze zařízení hello do cloudu hello a udělá z něj k dispozici toohello úlohy Azure Stream Analytics (ASA). Každá úloha datového proudu ASA používá samostatné služby IoT Hub příjemce skupiny tooread hello proud zpráv ze zařízení.

centra IoT řešení hello Hello také:

- Udržuje registru identit, která ukládá hello ID a ověřovací klíče všech hello zařízení povolené tooconnect toohello portálu. Můžete povolit nebo zakázat zařízení prostřednictvím registru identit hello.
- Odešle příkazy tooyour zařízení jménem portál řešení hello.
- Vyvolá metody v zařízeních jménem portál řešení hello.
- Udržuje dvojčata zařízení pro všechna registrovaná zařízení. Dvojče zařízení uloží hodnoty vlastností hello hlášených zařízení. Dvojče zařízení také ukládá nastaveny hello řešení portálu, tooretrieve hello zařízení, když se připojí další požadované vlastnosti.
- Plány úloh tooset vlastnosti pro více zařízení nebo volat metody na několika zařízeních.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

V řešení, vzdáleného sledování hello [Azure Stream Analytics] [ lnk-asa] (ASA) odešle zprávu zařízení zprávy přijaté službou IoT hub tooother back-end hello komponenty pro zpracování nebo úložiště. Různé úlohy ASA provést požadované funkce na základě obsahu hello hello zpráv.

**Úloha 1: Informace o zařízení** filtry zprávy z hello příchozího datového proudu zprávy s informacemi o zařízení a odešle je tooan koncového bodu centra událostí. Zařízení odesílá zprávy s informacemi o zařízení při spuštění a v odpovědi tooa **SendDeviceInfo** příkaz. Tato úloha používá následující tooidentify definice dotazu hello **informace o zařízení** zprávy:

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

Tuto úlohu odešle svůj výstupní tooan centra událostí pro další zpracování.

**Úloha 2: Pravidla** vyhodnotí příchozí telemetrická data o teplotě a vlhkosti v porovnání s mezními hodnotami na daném zařízení. Mezní hodnoty se nastavují v editoru pravidel hello v portálu řešení hello k dispozici. Každý pár zařízení/hodnota se ukládá pomocí časového razítka v objektu blob, který služba Stream Analytics načítá jako **referenční data**. Hello úloha porovná všechny neprázdnou hodnotu proti hello nastavenou prahovou hodnotu pro hello zařízení. Pokud překročí hello ' >' podmínky výstupy úlohy hello **alarmů** událost, která označuje, že prahová hodnota hello je překročena a poskytuje hello zařízení, hodnotu a hodnoty časového razítka. Tato úloha používá následující dotaz definice tooidentify telemetrické zprávy, které spustí alarm hello:

```
WITH AlarmsData AS 
(
SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Temperature' as ReadingType,
     Stream.Temperature as Reading,
     Ref.Temperature as Threshold,
     Ref.TemperatureRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature

UNION ALL

SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Humidity' as ReadingType,
     Stream.Humidity as Reading,
     Ref.Humidity as Threshold,
     Ref.HumidityRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
)

SELECT *
INTO DeviceRulesMonitoring
FROM AlarmsData

SELECT *
INTO DeviceRulesHub
FROM AlarmsData
```

Hello úloha odešle svůj výstupní tooan centra událostí pro další zpracování a ukládá podrobnosti o jednotlivých výstrah tooblob úložiště z kde hello portál řešení může číst informace o výstrahách hello.

**Úloha 3: Telemetrická** funguje na hello příchozí zařízení datový proud telemetrie dvěma způsoby. Hello nejprve odeslání všech telemetrických zpráv z úložiště objektů blob toopersistent hello zařízení pro dlouhodobé uložení. Hello druhý vypočítá vlhkosti průměrné, minimální a maximální hodnoty přes posuvné okno pět minut a odešle tato data tooblob úložiště. portál řešení Hello čte hello telemetrická data z objektu blob úložiště toopopulate hello grafy. Tato úloha používá následující definici dotazu hello:

```
WITH 
    [StreamData]
AS (
    SELECT
        *
    FROM [IoTHubStream]
    WHERE
        [ObjectType] IS NULL -- Filter out device info and command responses
) 

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    Temperature,
    Humidity,
    ExternalTemperature,
    EventProcessedUtcTime,
    PartitionId,
    EventEnqueuedUtcTime,
    * 
INTO
    [Telemetry]
FROM
    [StreamData]

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    AVG (Humidity) AS [AverageHumidity],
    MIN(Humidity) AS [MinimumHumidity],
    MAX(Humidity) AS [MaxHumidity],
    5.0 AS TimeframeMinutes 
INTO
    [TelemetrySummary]
FROM [StreamData]
WHERE
    [Humidity] IS NOT NULL
GROUP BY
    IoTHub.ConnectionDeviceId,
    SlidingWindow (mi, 5)
```

## <a name="event-hubs"></a>Event Hubs

Hello **informace o zařízení** a **pravidla** úlohy ASA výstup jejich data tooEvent centra tooreliably dál na toohello **procesor událostí** spuštěné v hello webové úlohy.

## <a name="azure-storage"></a>Úložiště Azure

Hello řešení používá Azure blob storage toopersist všechny hello nezpracovaná a souhrnné telemetrická data ze zařízení hello v řešení hello. portál Hello čte hello telemetrická data z objektu blob úložiště toopopulate hello grafy. toodisplay výstrahy, portál řešení hello čte hello data z úložiště objektů blob této záznamy při telemetrie překročila hodnoty hello nakonfigurované prahové hodnoty. řešení Hello také používá objekt blob úložiště toorecord hello prahové hodnoty, které se nastavují v portálu řešení hello.

## <a name="webjobs"></a>Webové úlohy

Kromě toho simulátorů zařízení hello toohosting, hello webové úlohy v řešení hello také hostitele hello **procesor událostí** spuštěné v Azure webová úloha, která zpracovává odezvy na příkazy. Používá příkaz odpovědi zprávy tooupdate hello zařízení historie příkazů (uložené v databázi Cosmos DB hello).

## <a name="cosmos-db"></a>Databáze Cosmos

řešení Hello používá informace o databázi Cosmos databáze toostore o hello zařízení připojených toohello řešení. Tyto informace zahrnují hello historie příkazů toodevices odeslaný portál řešení hello a metod volat z portálu řešení hello.

## <a name="solution-portal"></a>Portál řešení

portál řešení Hello je nasazen jako součást hello předkonfigurované řešení webové aplikace. Hello klíče stránky portálu řešení hello jsou hello řídicí panel a seznam zařízení hello.

### <a name="dashboard"></a>Řídicí panel

Tato stránka ve hello webové aplikaci používá ovládací prvky PowerBI jazyce javascript (viz [úložiště vizuálních prvků PowerBI](https://www.github.com/Microsoft/PowerBI-visuals)) toovisualize hello telemetrická data ze zařízení hello. řešení Hello používá hello ASA telemetrie úlohy toowrite hello telemetrická data tooblob úložiště.

### <a name="device-list"></a>Seznam zařízení

Z této stránky portálu řešení hello můžete:

* Zřízení nového zařízení. Tato akce nastaví hello jedinečné id zařízení a generuje hello ověřovací klíč. Zapíše informace o hello zařízení tooboth hello registru identit služby IoT Hub a hello specifické řešení Cosmos DB databáze.
* Správa vlastností zařízení. Tato akce zahrnuje zobrazení existujících vlastností a aktualizaci novými vlastnostmi.
* Odešlete příkazy tooa zařízení.
* Zobrazit historii příkazů hello pro zařízení.
* Povolení a zákaz zařízení.

## <a name="next-steps"></a>Další kroky

Hello následující příspěvky blogu TechNet poskytují další podrobnosti o hello předkonfigurovanému řešení vzdáleného monitorování:

* [IoT Suite - pod hello pokličkou - vzdálené monitorování](http://social.technet.microsoft.com/wiki/contents/articles/32941.iot-suite-under-the-hood-remote-monitoring.aspx)
* [Sada IoT Suite - Vzdálené monitorování - Přidávání skutečných a simulovaných zařízení](http://social.technet.microsoft.com/wiki/contents/articles/32975.iot-suite-remote-monitoring-adding-live-and-simulated-devices.aspx)

Budete-li pokračovat, Začínáme se službou IoT Suite načtením hello následující články:

* [Připojit vaše zařízení toohello předkonfigurovanému řešení vzdáleného monitorování][lnk-connect-rm]
* [Oprávnění na webu azureiotsuite.com hello][lnk-permissions]

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-iothub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-webjobs]: https://azure.microsoft.com/documentation/articles/websites-webjobs-resources/
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-device-twins]:  ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
