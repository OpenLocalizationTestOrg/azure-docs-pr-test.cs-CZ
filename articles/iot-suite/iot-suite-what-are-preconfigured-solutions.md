---
title: "aaaAzure IoT předkonfigurovanými řešeními | Microsoft Docs"
description: "Popis hello Azure IoT předkonfigurovaného řešení a jejich architektury spolu s odkazy tooadditional prostředky."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 59009f37-9ba0-4e17-a189-7ea354a858a2
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: bd059d08ab458fdb0b6f49b3ac469db930dab09e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-are-hello-azure-iot-suite-preconfigured-solutions"></a>Jaké jsou hello Azure předkonfigurovaná řešení IoT Suite?

Hello Azure předkonfigurovaná řešení IoT Suite jsou implementací běžných vzorů řešení IoT, můžete nasadit tooAzure pomocí svého předplatného. Můžete použít hello předkonfigurované řešení:

* Jako výchozí bod pro své vlastní řešení IoT.
* toolearn o běžných vzorech v návrhu řešení IoT a vývoj.

Každé předkonfigurované řešení je kompletní, začátku do konce implementace, používá simulované zařízení toogenerate telemetrie.

Můžete stáhnout hello úplný zdrojový kód toocustomize a rozšířit řešení toomeet hello vašich specifických požadavků IoT.

> [!NOTE]
> toodeploy jeden hello předkonfigurovaných řešení, navštivte [Microsoft Azure IoT Suite][lnk-azureiotsuite]. článek Hello [začít pracovat s hello přednastavenými řešeními IoT] [ lnk-getstarted-preconfigured] poskytuje další informace o tom, jak toodeploy a spusťte jeden z hello řešení.

Hello následující tabulka ukazuje, jak hello řešení mapují toospecific IoT funkce:

| Řešení | Přijímání dat | Identita zařízení | Správa zařízení | Příkazy a ovládání | Pravidla a akce | Prediktivní analýza |
| --- | --- | --- | --- | --- | --- | --- |
| [Vzdálené monitorování][lnk-getstarted-preconfigured] |Ano |Ano |Ano |Ano |Ano |- |
| [Prediktivní údržba][lnk-predictive-maintenance] |Ano |Ano |- |Ano |Ano |Ano |
| [Propojená továrna][lnk-getstarted-factory] |Ano |Ano |Ano |Ano |Ano |- |

* *Přijímání dat*: příchozí přenos dat v cloudu toohello škálování.
* *Identita zařízení*: Správa identit jedinečný zařízení a řídit řešení toohello přístupu k zařízení.
* *Správa zařízení:* Správa metadat zařízení a provádění operací, jako například restartování zařízení a upgrady firmwaru.
* *Příkazy a ovládání*: toocause hello zařízení tootake na akci, odesílat zprávy tooa zařízení z cloudu hello.
* *Pravidla a akce*: tooact na konkrétní zařízení cloud data, back-end hello řešení používá pravidla.
* *Prediktivní analýzy*: back-end hello řešení analyzuje data typu zařízení cloud toopredict při konkrétní akce má být provedena. Například analýza toodetermine telemetrická data motoru letadla po platnosti stroj údržby.

## <a name="remote-monitoring-preconfigured-solution-overview"></a>Přehled předkonfigurovaného řešení vzdáleného monitorování

Rozhodli jsme, že toodiscuss hello předkonfigurovaného řešení vzdáleného monitorování v tomto článku, protože ilustruje mnoho běžných prvků návrhu, které hello jiné sdílené složce řešení.

Hello následující diagram ilustruje klíčové prvky řešení vzdáleného sledování hello hello. Hello následující části obsahují další informace o těchto prvcích.

![Architektura předkonfigurovaného řešení vzdáleného monitorování][img-remote-monitoring-arch]

## <a name="devices"></a>Zařízení

Když nasadíte předkonfigurované řešení vzdáleného sledování hello, čtyři Simulovaná zařízení jsou předem zřízená hello řešení, které simulující chladicí zařízení. Tato simulovaná zařízení mají předdefinovaný model teploty a vlhkosti, který vysílá telemetrická data. Tato simulovaná zařízení jsou zahrnuta, protože:

- Znázornění hello začátku do konce tok dat hello řešení.
- Poskytují praktický zdroj telemetrie.
- Zadejte cíl pro metody nebo příkazy, pokud jste vývojář back-end pomocí hello řešení jako výchozí bod pro vlastní implementaci.

Hello simulované zařízení do řešení hello může reagovat toohello následující komunikace cloud zařízení:

- *Metody ([přímé metody][lnk-direct-methods])*: kde připojeném zařízení je očekávané toorespond okamžitě metodu obousměrné komunikace.
- *Příkazy (zpráv typu cloud zařízení)*: kde zařízení načte hello příkaz z trvalé fronty metodu jednosměrnou komunikaci.

Porovnání těchto rozdílných přístupů najdete v [doprovodných materiálech ke komunikaci typu cloud-zařízení][lnk-c2d-guidance].

Když se zařízení poprvé připojí tooIoT centra v hello předkonfigurované řešení, odešle rozbočovači toohello zpráva informace o zařízení. Tato zpráva zobrazí hello metod, které hello zařízení může reagovat. V hello předkonfigurovanému řešení vzdáleného monitorování Simulovaná zařízení podporuje tyto metody:

* *Zahájit aktualizaci firmwaru*: Tato metoda zahájí asynchronní úlohu na hello zařízení tooperform aktualizaci firmwaru. asynchronní úloha Hello používá hlášené vlastnosti toodeliver stav aktualizace toohello řídicí panel řešení.
* *Restartovat*: Tato metoda způsobí, že tooreboot hello simulované zařízení.
* *FactoryReset*: Tato metoda se aktivuje tovární nastavení hello simulované zařízení.

Když se zařízení poprvé připojí tooIoT centra v hello předkonfigurované řešení, odešle rozbočovači toohello zpráva informace o zařízení. Tato zpráva zobrazí hello příkazy, které hello zařízení může reagovat. Hello předkonfigurovanému řešení vzdáleného monitorování Simulovaná zařízení podporují tyto příkazy:

* *Příkaz ping zařízení*: hello zařízení odpovídá toothis příkaz potvrzením. Tento příkaz je užitečné pro kontrolu, že toto zařízení hello je stále aktivní a naslouchá.
* *Spustit Telemetrii*: dá pokyn toostart hello zařízení odesílat telemetrii.
* *Zastavit Telemetrii*: dá pokyn toostop hello zařízení odesílat telemetrii.
* *Změňte nastavení bodu teploty*: ovládací prvky hello simulované teplotní telemetrie hodnoty hello zařízení odesílá. Tímto příkazem snadno otestujete logiku back-endu.
* *Diagnostická Telemetrie*: ovládá, zda text hello zařízení odesílat hello externí teplotu jako telemetrický údaj.
* *Změny stavu zařízení*: Nastaví hello vlastnost metadata stavu v zařízení, která hello sestavy zařízení. Tímto příkazem snadno otestujete logiku back-endu.

Můžete přidat další Simulovaná zařízení toohello řešení, které emitování hello stejná telemetrická data a reakce toohello stejné metody a příkazy.

Kromě toho tooresponding toocommands a metody hello řešení používá [dvojčata zařízení][lnk-device-twin]. Zařízení používají zařízení dvojčata tooreport vlastnost hodnoty toohello back-end řešení. řídicí panel řešení Hello používá hodnoty vlastností toonew potřeby tooset dvojčata zařízení na zařízení. Například během hello firmware aktualizace proces hello simulované zařízení sestavy hello stav hello aktualizovat pomocí hlášené vlastností.

## <a name="iot-hub"></a>IoT Hub

V tomto předkonfigurovaném řešení hello odpovídá instance IoT Hubu toohello *Cloudová brána* v typické [architektuře řešení IoT][lnk-what-is-azure-iot].

Centrum IoT získává telemetrická data ze zařízení hello v jednom koncovém bodě. Centrum IoT také udržuje zařízení specifické koncové body, kde každé zařízení může načíst hello příkazy, které se odesílají tooit.

Hello IoT hub zpřístupní telemetrie hello přijatých prostřednictvím koncový bod čtení telemetrických dat na hello straně služby.

Hello možnosti správy zařízení služby IoT Hub umožňuje vám toomanage vlastnosti vaše zařízení z hello řešení portálu a plán úlohy, které provádění operací, jako:

- Restartování zařízení
- Změna stavu zařízení
- Aktualizace firmwaru

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

Hello předkonfigurované řešení používá tři [Azure Stream Analytics] [ lnk-asa] datový proud (ASA) úlohy toofilter hello telemetrie ze zařízení hello:

* *Úloha DeviceInfo* -výstupy data tooan události rozbočovače, který směruje zprávy specifické pro registraci zařízení, toohello registru zařízení řešení. Tento registr zařízení je databáze Azure Cosmos DB. Tyto zprávy jsou odesílány, když se zařízení poprvé připojí nebo v odpovědi tooa **změnit stav zařízení** příkaz.
* *Úloha telemetrie* – odesílá všechny úložiště objektů blob tooAzure nezpracovaná telemetrická data uloží málo používaná data a vypočítá telemetrie agregace, které se zobrazují na řídicí panel řešení hello.
* *Úloha pravidla* – filtry hello datový proud telemetrie podle hodnot, které přesahují stanovené mezní hodnoty pravidel a výstupy hello centra událostí tooan data. Jakmile se spustí pravidlo hello řídicím panelu portálu řešení Tato událost objeví jako nový řádek v tabulce historie alarmů hello. Tato pravidla mohou také spustit akci založenou na nastaveních hello definovaných hello **pravidla** a **akce** zobrazení na portálu řešení hello.

V tomto předkonfigurovaném řešení hello ASA úlohy jsou součástí toohello **back-endu IoT řešení** v typické [architektuře řešení IoT][lnk-what-is-azure-iot].

## <a name="event-processor"></a>Procesor událostí

V tomto předkonfigurovaném řešení hello procesor událostí součástí hello **back-endu IoT řešení** v typické [architektuře řešení IoT][lnk-what-is-azure-iot].

Hello **DeviceInfo** a **pravidla** úlohy ASA odeslat jejich výstup tooEvent centra pro doručení tooother back endové služby. řešení Hello používá [EventProcessorHost] [ lnk-event-processor] běhu instance [webové úlohy][lnk-web-job], tooread hello zprávy z těchto Center událostí. Hello **EventProcessorHost** používá:
- Hello **DeviceInfo** data tooupdate hello zařízení dat v databázi Cosmos DB hello.
- Hello **pravidla** tooinvoke hello logiku aplikace a aktualizace hello upozornění na data zobrazit na portálu řešení hello.

## <a name="device-identity-registry-device-twin-and-cosmos-db"></a>Registr identit zařízení, dvojče zařízení a služba Cosmos DB

Každá služba IoT Hub obsahuje [registr identit zařízení][lnk-identity-registry], který ukládá klíče zařízení. IoT Hub používá tuto informaci ověřování zařízení – zařízení musí být registrováno a mít platný klíč, aby se může připojit toohello rozbočovače.

A [dvojče zařízení] [ lnk-device-twin] je dokument JSON spravuje hello IoT Hub. Dvojče každého zařízení obsahuje:

- Hlášené vlastnosti poslal hello zařízení toohello rozbočovače. Tyto vlastnosti můžete zobrazit na portálu řešení hello.
- Požadované vlastnosti, které mají toosend toohello zařízení. Tyto vlastnosti můžete nastavit v portálu řešení hello.
- Značky, které existují pouze v hello dvojče zařízení a ne na hello zařízení. Tyto seznamy toofilter značky zařízení můžete použít na portálu řešení hello.

Toto řešení používá metadat zařízení toomanage dvojčata zařízení. Hello řešení používá také dat Cosmos DB databáze toostore další zařízení, specifická řešení například hello příkazy podporované jednotlivých zařízení a hello historii příkazů.

Hello řešení musí také udržovat informace hello v registru identit zařízení hello synchronizované s obsahem hello hello Cosmos DB databáze. Hello **EventProcessorHost** hello používá data z **DeviceInfo** úlohy stream analytics se toomanage hello synchronizace.

## <a name="solution-portal"></a>Portál řešení

![portál řešení][img-dashboard]

portál řešení Hello je uživatelské rozhraní založené na webu, který je nasazen jako součást hello předkonfigurované řešení toohello cloudu. Umožňuje:

* Zobrazovat historii telemetrie a alarmů na řídicím panelu.
* Zřizovat nová zařízení.
* Spravovat a sledovat zařízení.
* Odesílat příkazy toospecific zařízení.
* Vyvolávat metody v konkrétních zařízeních.
* Upravovat pravidla a akce.
* Plán úlohy toorun na jeden nebo více zařízení.

V tomto předkonfigurovaném řešení hello portál řešení součástí hello **back-endu IoT řešení** a součástí hello **zpracování a obchodní připojení** v typické hello [řešení IoT Architektura][lnk-what-is-azure-iot].

## <a name="next-steps"></a>Další kroky

Další informace o architekturách řešení IoT najdete v tématu [Služby Microsoft Azure IoT: Referenční architektura][lnk-refarch].

Teď víte, jaké předkonfigurované řešení je, abyste mohli začít nasazením hello *vzdálené monitorování* předkonfigurované řešení: [začít pracovat s hello předkonfigurované řešení] [ lnk-getstarted-preconfigured].

[img-remote-monitoring-arch]: ./media/iot-suite-what-are-preconfigured-solutions/remote-monitoring-arch1.png
[img-dashboard]: ./media/iot-suite-what-are-preconfigured-solutions/dashboard.png
[lnk-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-processor]: ../event-hubs/event-hubs-programming-guide.md#event-processor-host
[lnk-web-job]: ../app-service-web/web-sites-create-web-jobs.md
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-getstarted-preconfigured]: iot-suite-getstarted-preconfigured-solutions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-getstarted-factory]: iot-suite-connected-factory-overview.md
