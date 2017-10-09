---
title: aaaUse metriky toomonitor Azure IoT Hub | Microsoft Docs
description: "Jak tooassess metriky toouse Azure IoT Hub a monitorování hello celkový stav vašeho centra IoT."
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: a47108fd-f994-4105-b21d-5b8f697b699c
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: nberdy
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7d045013fb0229f488e72c93a6f668048b9d5c25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-iot-hub-metrics"></a>Pochopení metriky služby IoT Hub
IoT Hub metriky získáte lepší data o stavu hello hello Azure IoT prostředků ve vašem předplatném Azure. Povolení služby IoT Hub metriky můžete tooassess hello celkový stav hello služby IoT Hub a hello zařízení připojená tooit. Uživatelsky orientovaný statistiky jsou důležité, protože umožňují můžete zjistit, co se děje s vaší problémy IoT hub a nápovědy příčin bez nutnosti toocontact podporu Azure.

Ve výchozím nastavení jsou povolené metriky. Můžete zobrazit metriky služby IoT Hub z hello portálu Azure.

## <a name="how-tooview-iot-hub-metrics"></a>Jak tooview metriky služby IoT Hub
1. Vytvoření služby IoT hub. Pokyny naleznete v tom toocreate služby IoT hub v hello [Začínáme] [ lnk-get-started] průvodce.
2. Otevřete okno hello služby IoT hub. Zde, klikněte na tlačítko **metriky**.
   
    ![][1]
3. V okně metriky hello můžete zobrazit hello metriky pro službu IoT hub a vytváření vlastních pohledů vaše metriky. Můžete si vybrat toosend koncový bod služby Event Hubs tooan data metriky nebo účtu Azure Storage kliknutím **nastavení diagnostiky**.
   
    ![][2]

## <a name="iot-hub-metrics-and-how-toouse-them"></a>Metriky služby IoT Hub a jak toouse je
Služba IoT Hub zajišťuje několik metriky toogive vám přehled o stavu hello rozbočovače a hello celkový počet připojená zařízení. Informace z více metriky toopaint větší přehled o stavu hello hello IoT hub můžete kombinovat. Hello následující tabulka popisuje hello metriky, které sleduje každé služby IoT hub a jak jednotlivé metriky má vztah toohello celkový stav hello služby IoT hub.

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Pokusy o odeslání zprávy telemetrie|Počet|Celkem|Počet zařízení cloud telemetrické zprávy pokus o toobe odeslaných tooyour IoT hub|
|d2c.telemetry.ingress.Success|Telemetrické zprávy odeslané|Počet|Celkem|Počet zařízení cloud telemetrické zprávy úspěšně odeslat tooyour IoT hub|
|c2d.Commands.egress.COMPLETE.Success|Příkazy byla dokončena|Počet|Celkem|Počet příkazů typu cloud zařízení hello zařízení byla úspěšně dokončena|
|c2d.Commands.egress.Abandon.Success|Příkazy opuštění|Počet|Celkem|Počet příkazů typu cloud zařízení opuštěny v rámci hello zařízení|
|c2d.Commands.egress.Reject.Success|Příkazy odmítnut|Počet|Celkem|Počet příkazů typu cloud zařízení hello zařízení byl odmítnut|
|devices.totalDevices|Celkový počet zařízení|Počet|Celkem|Počet zařízení registrovaných tooyour IoT hub|
|devices.connectedDevices.allProtocol|Připojená zařízení|Počet|Celkem|Počet zařízení připojených tooyour IoT hub|
|d2c.telemetry.egress.Success|Telemetrické zprávy doručit|Počet|Celkem|Počet zpráv byla úspěšně zapsána tooendpoints (celkem)|
|d2c.telemetry.egress.dropped|Vyřazené zprávy|Počet|Celkem|Počet zpráv, vyřadit, protože neodpovídá žádné trasy a trasy záložní hello byla zakázána.|
|d2c.telemetry.egress.orphaned|Osamocené zprávy|Počet|Celkem|Hello počet zpráv, na které se neshodují žádné cesty, včetně hello záložní trasy|
|d2c.telemetry.egress.invalid|Neplatné zprávy|Počet|Celkem|Hello počet zpráv nejsou doručeny kvůli tooincompatibility s koncovým bodem hello|
|d2c.telemetry.egress.Fallback|Zprávy odpovídající záložního stavu|Počet|Celkem|Počet zpráv zapsaných toohello záložní koncový bod|
|d2c.endpoints.egress.eventHubs|Doručeny zprávy tooEvent koncové body centra|Počet|Celkem|Počet zpráv byly úspěšně napsané tooEvent koncové body centra|
|d2c.endpoints.latency.eventHubs|Latence zprávy pro koncové body centra událostí|počet milisekund|Průměr|Průměrná latence Hello mezi zpráv příjem příchozích dat toohello IoT hub a příjem příchozích dat zprávy do koncového bodu centra událostí, v milisekundách|
|d2c.endpoints.egress.serviceBusQueues|Doručeny zprávy tooService koncové body fronty sběrnice|Počet|Celkem|Počet zpráv byly úspěšně napsané tooService fronty sběrnice koncové body|
|d2c.endpoints.latency.serviceBusQueues|Latence zprávy pro koncové body frontou Service Bus|počet milisekund|Průměr|Průměrná latence Hello mezi zpráv příjem příchozích dat toohello IoT hub a příjem příchozích dat zprávy do fronty Service Bus koncový bod, v milisekundách|
|d2c.endpoints.egress.serviceBusTopics|Doručeny zprávy tooService tématu sběrnice koncové body|Počet|Celkem|Počet zpráv byly úspěšně napsané tooService tématu sběrnice koncové body|
|d2c.endpoints.latency.serviceBusTopics|Latence zprávy pro koncové body témata sběrnice|počet milisekund|Průměr|Průměrná latence Hello mezi zpráv příjem příchozích dat toohello IoT hub a příjem příchozích dat zprávy do tématu Service Bus koncový bod, v milisekundách|
|d2c.endpoints.egress.builtIn.events|Zpráv doručených toohello vestavěným koncovým bodem (zprávy nebo události)|Počet|Celkem|Počet zpráv byly úspěšně napsané toohello vestavěným koncovým bodem (zprávy nebo události)|
|d2c.endpoints.latency.builtIn.events|Zpráva latence pro hello vestavěným koncovým bodem (zprávy nebo události)|počet milisekund|Průměr|Průměrná latence Hello mezi zpráv příjem příchozích dat toohello IoT hub a příjem příchozích dat zpráva do hello vestavěným koncovým bodem (zprávy událostí), v milisekundách |
|d2c.Twin.Read.Success|Úspěšné twin čte ze zařízení|Počet|Celkem|přečte Hello počet všechny úspěšné twin spouštěná zařízení.|
|d2c.Twin.Read.failure|Čtení twin ze zařízení se nezdařila|Počet|Celkem|Hello počet všech nepodařilo spouštěná zařízení twin čtení.|
|d2c.Twin.Read.Size|Velikost odpovědi twin čtení ze zařízení|Bajty|Průměr|Hello průměr, min a max všechny úspěšné spouštěná zařízení twin čtení.|
|d2c.Twin.Update.Success|Aktualizace úspěšná twin ze zařízení|Počet|Celkem|Hello počet všechny úspěšné spouštěná zařízení twin aktualizací.|
|d2c.Twin.Update.failure|Aktualizace twin ze zařízení se nezdařila|Počet|Celkem|Hello počet všech se nezdařila aktualizace twin spouštěná zařízení.|
|d2c.Twin.Update.Size|Velikost twin aktualizace ze zařízení|Bajty|Průměr|průměr Hello a minimální a maximální velikost všech úspěšné spouštěná zařízení twin aktualizace.|
|c2d.Methods.Success|Volání úspěšné přímá metoda|Počet|Celkem|Hello počet všech úspěšných volání přímá metoda.|
|c2d.Methods.failure|Přímá metoda volání se nezdařilo|Počet|Celkem|Hello počet všech se nezdařila, volání metod direct.|
|c2d.Methods.requestSize|Žádost o velikosti volání přímá metoda|Bajty|Průměr|Hello průměr, min a max všechny úspěšné směrovat požadavky metod.|
|c2d.Methods.responseSize|Velikost odpovědi volání přímá metoda|Bajty|Průměr|Hello průměr, min a max všechny úspěšné přímá metoda odpovědí.|
|c2d.Twin.Read.Success|Úspěšné twin čte z back-end|Počet|Celkem|přečte Hello počet všechny úspěšné twin iniciované back-end.|
|c2d.Twin.Read.failure|Neúspěšné twin čte z back-end|Počet|Celkem|Hello počet všech se nezdařilo čtení twin iniciované back-end.|
|c2d.Twin.Read.Size|Velikost odpovědi twin čtení z back-end|Bajty|Průměr|Hello průměr, min a max všechny úspěšné iniciované back-end twin čtení.|
|c2d.Twin.Update.Success|Aktualizace úspěšná twin z back-end|Počet|Celkem|Hello počet všechny úspěšně spustil back-end twin aktualizací.|
|c2d.Twin.Update.failure|Aktualizace se nezdařila twin z back-end|Počet|Celkem|Hello počet všech se nezdařila aktualizace twin iniciované back-end.|
|c2d.Twin.Update.Size|Velikost twin aktualizace z back-end|Bajty|Průměr|průměr Hello a minimální a maximální velikost všech úspěšně spustil back-end twin aktualizace.|
|twinQueries.success|Úspěšné twin dotazy|Počet|Celkem|Hello počet všechny úspěšné twin dotazy.|
|twinQueries.failure|Neúspěšné twin dotazy|Počet|Celkem|Hello počet všechny neúspěšné twin dotazy.|
|twinQueries.resultSize|Objem výsledků dotazů Twin|Bajty|Průměr|Hello průměr, minimální a maximální velikosti výsledek hello všechny úspěšné twin dotazy.|
|jobs.createTwinUpdateJob.success|Úspěšné vytvoření úloh aktualizace twin|Počet|Celkem|Hello počet všech úspěšném vytvoření twin aktualizace úloh.|
|jobs.createTwinUpdateJob.failure|Neúspěšné vytvoření úloh aktualizace twin|Počet|Celkem|Hello počet všech se nezdařilo vytvoření twin aktualizace úlohy.|
|jobs.createDirectMethodJob.success|Úspěšné vytvoření úloh volání – metoda|Počet|Celkem|Hello počet všech úspěšném vytvoření přímá metoda volání úloh.|
|jobs.createDirectMethodJob.failure|Neúspěšné vytvoření úloh volání – metoda|Počet|Celkem|Hello počet všech se nezdařilo vytvoření přímá metoda vyvolání úlohy.|
|jobs.listJobs.success|Úlohy toolist úspěšných volání|Počet|Celkem|Hello počet všech úloh toolist úspěšných volání.|
|jobs.listJobs.failure|Selhání volání toolist úlohy|Počet|Celkem|Hello počet všech úloh toolist volání se nezdařilo.|
|jobs.cancelJob.success|Zrušení úloh úspěšné|Počet|Celkem|Hello počet všechny úspěšné volání toocancel úlohu.|
|jobs.cancelJob.failure|Informace o neúspěšné úloze zrušení|Počet|Celkem|Hello počet selhání volání toocancel všechny úlohy.|
|jobs.queryJobs.success|Až se úloha úspěšně dotazy|Počet|Celkem|Hello počet všech úloh tooquery úspěšných volání.|
|jobs.queryJobs.failure|Úloha se dotazuje se nezdařilo|Počet|Celkem|Hello počet všech úloh tooquery volání se nezdařilo.|
|jobs.completed|Dokončené úlohy|Počet|Celkem|Hello počet všech dokončené úlohy.|
|jobs.Failed|Neúspěšné úlohy|Počet|Celkem|Hello počet všechny neúspěšné úlohy.|

## <a name="next-steps"></a>Další kroky
Teď, když jste se seznámili přehled metriky služby IoT Hub, použijte tento odkaz toolearn informace o správě Azure IoT Hub:

* [Monitorování operací][lnk-monitor]

toofurther prozkoumat hello služby IoT Hub, najdete v tématu:

* [Příručka vývojáře pro službu IoT Hub][lnk-devguide]
* [Simulaci zařízení s Azure IoT Edge][lnk-iotedge]

<!-- Links and images -->
[1]: media/iot-hub-metrics/enable-metrics-1.png
[2]: media/iot-hub-metrics/enable-metrics-2.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-operations-monitoring]: iot-hub-operations-monitoring.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
