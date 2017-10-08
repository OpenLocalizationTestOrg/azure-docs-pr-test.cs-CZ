---
title: "AAA Azure Stream Analytics řízené daty ladění pomocí hello úlohy diagram | Microsoft Docs"
description: "Řešení potíží úlohu služby Stream Analytics pomocí diagram hello úlohy a metriky."
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 05/01/2017
ms.author: jeffstok
ms.openlocfilehash: 1af884d485bebb06b034da01a13f7f8240516571
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="data-driven-debugging-by-using-hello-job-diagram"></a>Řízené daty ladění pomocí hello úlohy diagram

diagram Hello úlohy na hello **monitorování** okno v hello portálu Azure můžete vizualizovat vaše úlohy kanálu. Zobrazuje vstupy, výstupy a kroky dotazu. Hello úlohy diagram tooexamine hello metriky můžete použít pro každý krok, toomore rychle izolovat hello zdroj problému při odstraňování potíží.

## <a name="using-hello-job-diagram"></a>Pomocí úlohy diagram hello

V hello portálu Azure při v úloze Stream Analytics, v části **podporu + Poradce při potížích s**, vyberte **úlohy diagram**:

![Diagram úlohy s metriky - umístění](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-1.png)

V dotazu úpravy podokně vyberte každý krok toosee hello odpovídající oddílů dotazu. Metriky graf pro krok hello se zobrazí v dolním podokně na stránce hello.

![Diagram úlohy s metriky – základní úlohy](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-2.png)

Vyberte toosee hello oddíly hello vstupu Azure Event Hubs, **...** Zobrazí se kontextová nabídka. Také můžete zobrazit vstupních fúzí hello.

![Diagram úlohy s metriky - rozbalte oddíl](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-3.png)

Graf metriky toosee hello pro pouze jeden oddíl, vyberte hello oddílu uzlu. metriky Hello se zobrazí v dolní části hello hello stránky.

![Diagram úlohy s metriky - další metriky](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-4.png)

Graf toosee hello metriky pro fúze, vyberte hello fúze uzlu. Hello následující graf ukazuje, že žádné události byly zrušené nebo upravena.

![Diagram úlohy s metriky - mřížky](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-5.png)

Podrobnosti o hello toosee hello hodnota metriky a času, bod toohello grafu.

![Úloha diagram s metriky – ukazatele](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-6.png)

## <a name="troubleshoot-by-using-metrics"></a>Řešení potíží pomocí metrik

Hello **QueryLastProcessedTime** Metrika udává, kdy konkrétní krok přijal data. Prohlížením hello topologie, je možné pracovat zpátky z toosee procesoru výstup hello, který krok nepřijímá data. Pokud krok není získávání dat, přejděte toohello dotazu krok těsně před. Zkontrolujte, jestli hello předchozí krok dotazu má časový interval, a pokud uplynutí dostatek času pro něj toooutput data. (Poznámka: Tento čas systému windows jsou zaseknutá toohello hodinu.)
 
Pokud hello předchozí krok dotazu vstupní procesoru, cílem použití hello vstupní metriky toohelp odpovědí hello následující otázky. Můžou pomoct určit, zda je úloha získání dat z jeho zadávání. Pokud se dotaz hello je rozdělena na oddíly, zkontrolujte každý oddíl.
 
### <a name="how-much-data-is-being-read"></a>Kolik dat se čte?

*   **InputEventsSourcesTotal** je hello počet jednotek dat pro čtení. Například hello počet objektů BLOB.
*   **InputEventsTotal** je hello počet událostí pro čtení. Tato metrika je dostupná pro jednotlivé oddíly.
*   **InputEventsInBytesTotal** je hello počet přečtených bajtů.
*   **InputEventsLastArrivalTime** je aktualizováno každých přijatou událost čas zařazených do fronty.
 
### <a name="is-time-moving-forward-if-actual-events-are-read-punctuation-might-not-be-issued"></a>Je čas postoupíte? Pokud se čtou skutečné události, nemusí být vydaný interpunkce.

*   **InputEventsLastPunctuationTime** označuje interpunkce byl vydán při přesouvání čas tookeep dál. Pokud není objeví interpunkční znaménka, může zablokování datový tok.
 
### <a name="are-there-any-errors-in-hello-input"></a>Jsou všechny chyby hello vstup?

*   **InputEventsEventDataNullTotal** je počet událostí, které mají hodnotu null data.
*   **InputEventsSerializerErrorsTotal** je počet událostí, které nelze deserializovat správně.
*   **InputEventsDegradedTotal** je počet událostí, které se za potíže než s deserializace.
 
### <a name="are-events-being-dropped-or-adjusted"></a>Jsou vyřazeny nebo upravena události?

*   **InputEventsEarlyTotal** je hello počet událostí, které mají časové razítko aplikace před hello horní meze.
*   **InputEventsLateTotal** je hello počet událostí, které mají časové razítko aplikace po hello horní meze.
*   **InputEventsDroppedBeforeApplicationStartTimeTotal** je číslo události hello vyřazen dříve, než čas spuštění úlohy hello.
 
### <a name="are-we-falling-behind-in-reading-data"></a>Jsme padají při čtení dat?

*   **InputEventsSourcesBackloggedTotal** zjistíte, kolik další zprávy nutné toobe čtení pro službu Event Hubs a Azure IoT Hub vstupy.


## <a name="get-help"></a>Podpora
Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Další kroky
* [Úvod tooStream Analytics](stream-analytics-introduction.md)
* [Začínáme s Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování úlohy Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka jazyka dotazů Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Stream Analytics správu odkazu k REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
