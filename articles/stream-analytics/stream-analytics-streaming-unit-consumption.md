---
title: "Azure Stream Analytics: Optimalizace vaše úlohy toouse jednotek Streaming efektivně | Microsoft Docs"
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
ms.openlocfilehash: 5ad98b34d625190a879260f54c9eff0294e230cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-your-job-toouse-streaming-units-efficiently"></a>Efektivní optimalizovat vaše úlohy toouse jednotek Streaming

Azure Stream Analytics agreguje hello výkonu "váhu" běžící úlohy do jednotek streamování (SUs). Služba SUs představují hello výpočetní prostředky, které jsou spotřebované tooexecute úlohu. Služba SUs poskytují způsob toodescribe hello relativní zpracování událostí založené na kombinaci měření procesoru, paměti, kapacity a čtení a zápisu sazby. Tato kapacita vám umožní soustředit na logiku hello dotazu a odebere z tooknow aspekty výkonu vrstvy úložiště, které se musí přidělit paměť pro úlohu ručně a přibližná hello procesoru core-počet potřeby toorun úlohu v časovém limitu.

## <a name="how-many-sus-are-required-for-a-job"></a>Kolik služby SUs jsou potřeba pro úlohu?

Volba hello počet požadované služby SUs pro konkrétní úlohy závisí na konfiguraci hello oddílů pro vstupy hello a hello dotazu, který je definován v rámci úlohy hello. Hello **škálování** okno vám umožní tooset hello právo počet služby SUs. Je představuje dobrou praxi tooallocate další služby SUs, než je potřeba. modul zpracování Stream Analytics Hello optimalizuje latence a propustnosti na hello náklady přidělování další paměť.

Obecně platí, hello osvědčeným postupem je toostart s 6 služby SUs pro dotazy, které nepoužívají *PARTITION BY*. Pak určete hello paprika místo pomocí metody omyl a ve kterém můžete upravit hello počet služby SUs po předání reprezentativní objemy dat a prozkoumat hello SU % využití metrika.

Azure Stream Analytics udržuje v okně hello "změnit pořadí vyrovnávací paměti" volá se před spuštěním jakékoli zpracovávání událostí. Události jsou v rámci okna změnit pořadí hello seřazené podle času a další operace jsou prováděny v hello dočasně seřadit události. Změna pořadí událostí podle času zajistí, že hello operátor má získat přehled o všech hello události v hello stanovený časový rámec. Umožňuje také operátor hello provádějí hello zpracování požadavků a vytvářejí výstup. Vedlejším účinkem tento mechanismus je, že je zpracování zpožděn hello doba trvání okna hello změnit pořadí. spotřeba paměti Hello hello úlohy (která ovlivňuje SU spotřeby) je funkce hello velikosti toto změnit pořadí událostí, které jsou v něm obsažena číslo okno a hello.

> [!NOTE]
> Při změně hello počet čtenářů při upgradech úlohy se zapisují protokoly tooaudit přechodný upozornění. Úlohy Stream Analytics automaticky zotavit tyto přechodné problémy.

## <a name="common-high-memory-causes-for-high-su-usage-for-running-jobs"></a>Běžné příčiny vysoké paměti na vysoké využití SU pro spuštění úlohy

### <a name="high-cardinality-for-group-by"></a>Vysokou kardinalitou Seskupit podle

Mohutnost Hello příchozích událostí, které určuje využití paměti pro úlohu hello.

Například v `SELECT count(*) from input group by clustered, tumblingwindow (minutes, 5)`, hello číslo přidružené k **clusterové** je Kardinalita hello hello dotazu.

toomitigate problémy, které jsou způsobeny vysokou kardinalitou škálovat hello dotazu zvýšením oddíly používající **PARTITION BY**.

```
Select count(*) from input
Partition By clusterid
GROUP BY clustered tumblingwindow (minutes, 5)
```

Hello počet *clusterové* je hello Kardinalita GROUP BY v tomto poli.

Po dotazu hello je rozdělena na oddíly, je šíří přes víc uzlů. V důsledku toho se snižuje hello počet událostí, než dorazí do každého uzlu, který pak snižuje hello velikost vyrovnávací paměti hello změnit pořadí. Oddíly centra událostí by také oddílu podle partitionid.

### <a name="high-unmatched-event-count-for-join"></a>Počet událostí vysokou neodpovídající ke spojení

Hello počet neodpovídající událostí ve spojení ovlivňuje využití paměti hello hello dotazu. Například proveďte dotaz, který je vyhledávání toofind hello počet otisky ad vygeneruje klikne na:

```
SELECT id from clicks INNER JOIN,
impressions on impressions.id = clicks.id AND DATEDIFF(hour, impressions, clicks) between 0 AND 10
```

V tomto scénáři je možné, že se zobrazují mnoho reklamy a jsou generovány několika kliknutími. V důsledku toho by vyžadovaly hello úlohy tookeep všechny hello události v rámci hello časový interval. Hello množství paměti spotřebované je přímo úměrná toohello okno velikost a událostí rychlost. 

toomitigate této situaci škálování hello dotazu zvýšením oddíly pomocí PARTITION BY. 

Po dotazu hello je rozdělena na oddíly, je šíří přes víc uzlů zpracování. V důsledku toho se snižuje hello počet událostí, než dorazí do každého uzlu, který pak snižuje hello velikost vyrovnávací paměti hello změnit pořadí.

### <a name="large-number-of-out-of-order-events"></a>Velké množství mimo pořadí událostí 

Velký počet události mimo pořadí v rámci velké časové okno způsobí, že velikost hello hello "Změna pořadí vyrovnávací paměti" toobe větší. toomitigate této situaci škálování hello dotazu zvýšením oddíly pomocí PARTITION BY. Po dotazu hello je rozdělena na oddíly, je šíří přes víc uzlů. V důsledku toho se snižuje hello počet událostí, než dorazí do každého uzlu, který pak snižuje hello velikost vyrovnávací paměti hello změnit pořadí. 


## <a name="get-help"></a>Podpora
Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Další kroky
* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics query language – referenční informace](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční dokumentace Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
