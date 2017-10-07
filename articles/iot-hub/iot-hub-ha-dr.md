---
title: "aaaAzure IoT Hub vysoké dostupnosti a zotavení po havárii | Microsoft Docs"
description: "Popisuje funkce hello Azure a IoT Hub, které vám pomůžou toobuild vysoce dostupné řešení Azure IoT s možností obnovení po havárii."
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: ae320e58-aa20-45b9-abdc-fa4faae8e6dd
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: elioda
ms.openlocfilehash: 74e1fa1682258819ffb3fd84eb79e42fc6e4120f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="iot-hub-high-availability-and-disaster-recovery"></a>IoT Hub vysoké dostupnosti a zotavení po havárii
Jako služby Azure IoT Hub poskytuje vysokou dostupnost (HA) pomocí redundance na úrovni hello oblast Azure, bez další zátěže vyžadují hello řešení. Platforma Microsoft Azure Hello také zahrnuje funkce toohelp sestavování řešení s možností zotavení po havárii nebo mezi oblastmi dostupnosti. Pokud chcete tooprovide globální, mezi oblastmi vysoká dostupnost pro zařízení nebo uživatelů, návrh a připravte řešení tootake výhod těchto funkcí Azure zotavení po Havárii. článek Hello [Azure obchodní kontinuity technické pokyny](../resiliency/resiliency-technical-guidance.md) popisuje hello integrované funkce v Azure pro provozní kontinuitu a zotavení po Havárii. Hello [zotavení po havárii a vysoká dostupnost pro aplikace Azure] [ Disaster recovery and high availability for Azure applications] dokumentu poskytuje doporučení ohledně architektury na strategie pro aplikace Azure tooachieve HA a zotavení po Havárii.

## <a name="azure-iot-hub-dr"></a>Azure IoT Hub DR
Kromě toho HA toointra oblasti IoT Hub implementuje mechanismy převzetí služeb při selhání pro zotavení po havárii, které vyžadují bez zásahu uživatele hello. IoT Hub DR samoobslužné inicializaci a má cíli času obnovení (RTO) 2-26 hodin a hello následující plánovaných bodů obnovení (rpo).

| Funkce | PLÁNOVANÝ BOD OBNOVENÍ |
| --- | --- |
| Dostupnost služeb pro operace registru a komunikace |Případné ztrátě CName |
| Data identit v registru identit |0 – 5 minut ztrátě dat. |
| Zprávy typu zařízení-cloud |Dojde ke ztrátě všech nepřečtených zpráv |
| Operace sledování zpráv |Dojde ke ztrátě všech nepřečtených zpráv |
| Zprávy typu cloud zařízení |0 – 5 minut ztrátě dat. |
| Frontu zpětné vazby cloud zařízení |Dojde ke ztrátě všech nepřečtených zpráv |

## <a name="regional-failover-with-iot-hub"></a>Místní převzetí služeb při selhání s centrem IoT
Dokončení zpracování topologií nasazení v řešeních pro IoT je mimo rámec tohoto článku hello. Hello článek popisuje hello *regionální převzetí služeb při selhání* modelu nasazení hello za účelem vysoké dostupnosti a zotavení po havárii.

V modelu regionální převzetí služeb při selhání hello řešení back end spustí především v umístění jednoho datového centra a sekundární IoT hub a back-end jsou nasazeny v jiném umístění datového centra. Pokud centra IoT hello primární datové centrum hello vyskytne výpadku nebo dojde k přerušení připojení k síti hello z primární datové centrum aplikace hello zařízení toohello. Zařízení používají koncového bodu služby sekundární vždy, když primární brány hello nelze získat přístup. Pomocí funkce převzetí služeb při selhání mezi oblastmi je možné zlepšit hello řešení dostupnosti nad rámec hello vysokou dostupnost jedné oblasti.

Na vysoké úrovni, tooimplement model místní převzetí služeb při selhání službou IoT Hub, je třeba hello následující:

* **Sekundární IoT hub a zařízení směrování logiku**: V případě hello přerušení služby ve vaší primární oblasti, musíte spustit zařízení připojující tooyour sekundární oblast. Zadané hello stav povaha většina služeb související se situací, je běžné řešení správci tootrigger hello převzetí služeb při selhání mezi oblast procesu. Hello nejlepší způsob, jak toocommunicate hello nový koncový bod toodevices, při zachování kontroly nad hello procesu, je toohave je pravidelně kontrolovat *concierge* hello aktuální aktivní koncový bod služby. Hello concierge služba může být webové aplikace, která je replikována a uchovávat dosažitelný pomocí technik DNS přesměrování (například pomocí [Azure Traffic Manager][Azure Traffic Manager]).
* **Replikace registru identit** -toobe použitelný, hello sekundární IoT hub musí obsahovat všechny identity zařízení, které se můžou připojit toohello řešení. Hello řešení musí zachovat geograficky replikované zálohy identit zařízení a odešlete toohello sekundární IoT hub před přepnutím hello aktivní koncový bod pro hello zařízení. funkce exportu identity Hello zařízení služby IoT Hub je užitečné v tomto kontextu. Další informace najdete v tématu [Příručka vývojáře pro službu IoT Hub - registru identit][IoT Hub developer guide - identity registry].
* **Slučování logiku** – když primární oblasti hello opět k dispozici, všechny hello stav a data, která byla vytvořena v hello sekundární lokality musí být migrované back toohello primární oblasti. Tento stav a data se týká hlavně toodevice identit a metadata aplikace, které je potřeba sloučit s hello primární IoT hub a jiných úložišť specifické pro aplikaci v primární oblasti hello. toosimplify tento krok byste měli používat idempotent operace. Operace Idempotent minimalizovat hello vedlejší účinky od hello následné konzistentní distribuci událostí a od duplikáty nebo doručení na více systémů pořadí událostí. Kromě toho logiku aplikace hello musí být nekonzistence potenciální navrženou tootolerate nebo "mírně" datum stavu. Tato situace může nastat z důvodu toohello další čas potřebný pro systém hello příliš "retušovat" podle plánovaných bodů obnovení (RPO).

## <a name="next-steps"></a>Další kroky
Použijte tyto odkazy toolearn Další informace o službě Azure IoT Hub:

* [Začínáme s centra IoT (kurz)][lnk-get-started]
* [Co je Azure IoT Hub?][What is Azure IoT Hub?]

[Disaster recovery and high availability for Azure applications]: ../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md
[Azure Business Continuity Technical Guidance]: https://azure.microsoft.com/documentation/articles/resiliency-technical-guidance/
[Azure Traffic Manager]: https://azure.microsoft.com/documentation/services/traffic-manager/
[IoT Hub developer guide - identity registry]: iot-hub-devguide-identity-registry.md

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[What is Azure IoT Hub?]: iot-hub-what-is-iot-hub.md
