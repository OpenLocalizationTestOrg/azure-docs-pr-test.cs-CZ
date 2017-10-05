---
title: "Azure Stream Analytics: Optimalizace na efektivně používat jednotek streamování vaše úlohy | Microsoft Docs"
description: "Dotaz osvědčené postupy pro škálování a výkon v Azure Stream Analytics."
keywords: "streamování jednotky, výkon dotazů"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: 1441a5df4fd92abf85763ca9a1512503b1a0da56
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="optimize-your-job-to-use-streaming-units-efficiently"></a>Optimalizace úlohu efektivně používat jednotky streamování

Azure Stream Analytics agreguje výkonu "váhu" běžící úlohy do jednotek streamování (SUs). Služba SUs představují výpočetní prostředky, které jsou využívat k provedení úlohy. Jednotky SU umožňují popsat relativní kapacitu zpracování událostí na základě výkonu procesoru, paměti a rychlosti čtení a zápisu. Tato kapacita vám umožní soustředit na logiku dotazu a odebere, museli jste z znáte úložiště vrstvy důležité informace o výkonu, přidělit paměť pro úlohu ručně a přibližná core-počet procesorů spouštění úloh v časovém limitu.

## <a name="how-many-sus-are-required-for-a-job"></a>Kolik služby SUs jsou potřeba pro úlohu?

Volba číslo požadované služby SUs pro konkrétní úlohy závisí na konfiguraci oddílů pro vstupy a dotaz, který je definován v rámci úlohy. **Škálování** okně můžete nastavit správný počet služby SUs. Je osvědčeným postupem přidělit další služby SUs, než je potřeba. Modul služby Stream Analytics zpracování optimalizuje latence a propustnosti cenu přidělování další paměť.

Obecně platí, osvědčeným postupem je začínat 6 služby SUs pro dotazy, které nepoužívají *PARTITION BY*. Pak určete místo paprika pomocí metody omyl a ve kterém můžete změnit počet SUs po předání reprezentativní objemy dat a prozkoumat metriky využití SU %.

Azure Stream Analytics udržuje v okně "vyrovnávací paměť změnit pořadí" volá se před spuštěním jakékoli zpracovávání událostí. Události jsou v rámci okna změnit pořadí seřazené podle času a další operace jsou prováděny dočasně seřazených událostí. Změna pořadí událostí podle času zajistí, že operátor mít přehled o všech událostí v stanovené časový rámec. Umožňuje také operátor provést zpracování požadavků a vytváření výstupu. Vedlejším efektem tohoto mechanismu je, že zpracování je zpožděné o dobu trvání tohoto okna. Nároky na paměť úlohy (která ovlivňuje SU spotřeby) je funkce, velikosti tohoto okna změnit pořadí a počet událostí, které jsou v něm obsažena.

> [!NOTE]
> Při změně počet čtenářů při upgradech úlohy přechodný upozornění se zapisují do protokolů auditů. Úlohy Stream Analytics automaticky zotavit tyto přechodné problémy.

## <a name="common-high-memory-causes-for-high-su-usage-for-running-jobs"></a>Běžné příčiny vysoké paměti na vysoké využití SU pro spuštění úlohy

### <a name="high-cardinality-for-group-by"></a>Vysokou kardinalitou Seskupit podle

Kardinalita příchozích událostí určuje využití paměti pro příslušnou úlohu.

Například v `SELECT count(*) from input group by clustered, tumblingwindow (minutes, 5)`, číslo přiřazené **clusterové** je Kardinalita dotazu.

Zmírnit problémy, které jsou způsobeny vysokou kardinalitou, škálovat dotaz zvýšením oddíly používající **PARTITION BY**.

```
Select count(*) from input
Partition By clusterid
GROUP BY clustered tumblingwindow (minutes, 5)
```

Počet *clusterové* je mohutnost GROUP BY v tomto poli.

Po dotazu je rozdělena na oddíly, je šíří přes víc uzlů. V důsledku toho se snižuje počet událostí, než dorazí do každého uzlu, který pak snižuje velikost vyrovnávací paměti změnit pořadí. Oddíly centra událostí by také oddílu podle partitionid.

### <a name="high-unmatched-event-count-for-join"></a>Počet událostí vysokou neodpovídající ke spojení

Počet neodpovídající událostí ve spojení ovlivňuje využití paměti v dotazu. Například proveďte dotaz, který je vyhledávání k nalezení čísla otisky ad, který generuje klikne na:

```
SELECT id from clicks INNER JOIN,
impressions on impressions.id = clicks.id AND DATEDIFF(hour, impressions, clicks) between 0 AND 10
```

V tomto scénáři je možné, že se zobrazují mnoho reklamy a jsou generovány několika kliknutími. V důsledku toho by vyžadovaly úlohu chcete zachovat všechny události v rámci časový interval. Množství paměti spotřebované je úměrná okno velikost a událostí rychlost. 

Pro zmírnění této situaci, škálovat zvýšením oddíly pomocí oddílu pomocí dotazu. 

Po dotazu je rozdělena na oddíly, je šíří přes víc uzlů zpracování. V důsledku toho se snižuje počet událostí, než dorazí do každého uzlu, který pak snižuje velikost vyrovnávací paměti změnit pořadí.

### <a name="large-number-of-out-of-order-events"></a>Velké množství mimo pořadí událostí 

Velký počet události mimo pořadí v rámci velké časové okno způsobí, že velikost "vyrovnávací paměti změnit pořadí" musí být větší. Pro zmírnění této situaci, škálovat zvýšením oddíly pomocí oddílu pomocí dotazu. Po dotazu je rozdělena na oddíly, je šíří přes víc uzlů. V důsledku toho se snižuje počet událostí, než dorazí do každého uzlu, který pak snižuje velikost vyrovnávací paměti změnit pořadí. 


## <a name="get-help"></a>Podpora
Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Další kroky
* [Úvod do služby Azure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics query language – referenční informace](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční dokumentace Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
