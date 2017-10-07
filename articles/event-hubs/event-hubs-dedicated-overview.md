---
title: "aaaOverview Azure Event Hubs vyhrazené kapacity | Microsoft Docs"
description: "Přehled Microsoft Azure Event Hubs vyhrazené kapacity."
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: sethm;babanisa
ms.openlocfilehash: 79c09975e5c0a6d4729c8f836576770abaaf322e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-event-hubs-dedicated"></a>Přehled služby Event Hubs vyhrazené

*Vyhrazené centra událostí* kapacity nabízí nasazení jednoho klienta pro zákazníky s hello nejnáročnější požadavky. Úplné škálované Azure Event Hubs můžete příjem příchozích dat než 2 miliony událostí za sekundu nebo si too2 GB za sekundu telemetrie s plně odolná úložiště a dílčí sekundu latencí. Tento také umožňuje integrované řešení zpracováním v reálném čase a batch na hello stejném systému. S součástí nabídky hello archivu centra událostí můžete snížit složitost hello vašeho řešení tak, že jeden datový proud v reálném čase i na základě batch kanály podpory.

Hello následující tabulka porovnává úrovních hello k dispozici služby Event Hubs. Nabídka vyhrazené centra událostí Hello je pevný měsíční poplatek, porovnání toousage ceny pro většinu funkcí Standard a Basic. Vyhrazené vrstvy Hello nabízí funkce hello hello standardní plán, ale s kapacitou škálování enterprise pro zákazníky s náročné úlohy. 

| Funkce | Basic | Standard | Vyhrazený |
| --- |:---:|:---:|:---:|
| Události příchozího přenosu dat | Platba za mil. události | Platba za mil. události | Zahrnuje |
| Jednotky propustnosti (1 MB za sekundu vstupní, výstupní 2 MB za sekundu) | Platit za hodinu | Platit za hodinu | Zahrnuje |
| Velikost zprávy | 256 kB | 256 kB | 1 MB |
| Zásady vydavatele | Není k dispozici | Ano | Ano |     
| Skupiny příjemců | 1 - výchozí | 20 | 20 |
| Přehrání zprávy | Ano | Ano | Ano |
| Maximální počet jednotek propustnosti | 20 | 20 (flexibilní too100)  | 1 kapacitní jednotka ≈ 200 |
| Zprostředkovaná připojení | zahrnuté 100 | 1000 zahrnuté | 100 tisíc zahrnuté |
| Další zprostředkovaná připojení | Není k dispozici | Ano | Ano |
| Uchovávání zpráv | 1 den v základu | 1 den v základu | Až dny too7 zahrnuté |
| Archiv (Preview) | Není k dispozici   | Platit za hodinu | Zahrnuje |

## <a name="benefits-of-event-hubs-dedicated-capacity"></a>Výhody kapacity vyhrazené centra událostí

Při použití vyhrazené centra událostí jsou k dispozici Hello následující výhody:

* Hostování s žádné šumu od ostatních klientů jednoho klienta.
* Zpráva roste too1 MB jako porovnání too256 KB pro Standard a Basic.
* Pokaždé, když Opakovatelný výkon.
* Toomeet zaručenou kapacitu, musí vaše shluků.
* Škálovatelná mezi 1 a 8 kapacitu jednotky (CU) – poskytuje too2 mil. příjem příchozích dat událostí za sekundu.
  * Vlas spravovat hello škálování pro vyhrazené centra událostí, kde každý Cu: můžete zadat přibližně hello ekvivalentní 200 jednotky propustnosti (TU).
* Nula údržby: jsme spravovat vyrovnávání zatížení, OS aktualizace, opravy zabezpečení a rozdělení do oddílů.
* Pevné ceny měsíčně.

Vyhrazené centra událostí taky odebere některé omezení propustnosti hello hello standardní nabídky. Jednotky propustnosti na úrovních Basic a Standard získat oprávnění too1000 událostí za sekundu nebo 1 MB za sekundu příjem příchozích dat za TU a double toto množství odchozí. Nabídka vyhrazené škálování Hello nemá žádné omezení na příjem příchozích dat a počet událostí odchozí. Těchto mezních hodnot se řídí pouze hello zpracování kapacitu hello zakoupili služby event hubs.

Tato služba je zaměřený na hello největší telemetrická data uživatele a je k dispozici toocustomers ke smlouvě enterprise.

## <a name="how-tooonboard"></a>Jak tooonboard

Platforma vyhrazené centra událostí Hello je nabízen smlouvu enterprise agreement s různou velikost vlas. Každý Cu: poskytuje přibližně hello ekvivalentní 200 jednotek propustnosti. Je možné škálovat vaše kapacita nahoru nebo dolů v rámci hello měsíc toomeet vašim potřebám přidáním nebo odebráním vlas. Vyhrazené plán Hello je jedinečný, v tom, že si všimnete víc praktických registrace z hello Event Hubs produktu team tooget hello flexibilní nasazení, které je pro vás nejvhodnější. 

## <a name="next-steps"></a>Další kroky
Obraťte se na obchodním zástupcem společnosti Microsoft nebo Microsoft Support tooget další podrobnosti týkající se kapacity vyhrazené centra událostí. Můžete také další informace o službě Event Hubs návštěvou hello následující odkazy:

Podrobné informace o cenách najdete na adrese hello následující odkazy:

- [Vyhrazené centra událostí ceny](https://azure.microsoft.com/pricing/details/event-hubs/). Můžete také kontaktovat obchodním zástupcem společnosti Microsoft nebo Microsoft Support tooget Další informace o kapacitě vyhrazené centra událostí.
- Hello [– nejčastější dotazy centra událostí](event-hubs-faq.md) obsahuje informace o cenách a odpovídá na některé nejčastější dotazy týkající se služby Event Hubs. 
