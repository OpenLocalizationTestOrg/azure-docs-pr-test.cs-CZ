---
title: "Postup monitorování a snižování omezení v Azure časové řady přehledy | Microsoft Docs"
description: "Tento článek popisuje, jak monitorovat, diagnostikovat a zmírnit problémy s výkonem, které způsobí latenci a omezení v Azure časové řady přehledy."
services: time-series-insights
ms.service: time-series-insights
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: troubleshooting
ms.date: 11/27/2017
ms.openlocfilehash: ec16f20723e4a613c953363da6cf6b463de829a9
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="monitor-and-mitigate-throttling-to-reduce-latency-in-azure-time-series-insights"></a>Monitorování a zmírnit omezení ke snížení latence v Azure časové řady přehledy
Při konfiguraci vašeho prostředí překračuje příchozích dat, může zaznamenat latenci nebo omezení v Azure časové řady přehledy.

Latence a omezování tak, že správně nakonfigurujete prostředí pro množství dat, který chcete analyzovat se můžete vyhnout.

Můžete se nejpravděpodobněji zaznamenat latenci a při omezování můžete:

- Přidáte zdroje událostí obsahující stará data, která může být vyšší než vaše míra přiděleném příjem příchozích dat (časové řady Statistika muset aktualizovány).
- Přidejte další zdroje událostí do prostředí, což vede k Špička z další události (které může překročit kapacitu pro vaše prostředí).
- Push velkých objemů historické události se zdrojem událostí, což vede k prodleva (čas řady Statistika muset aktualizovány).
- Připojení referenčních dat pomocí telemetrie, což vede k větší velikost události.  Z hlediska omezení paketu ingressed data s velikost paketu 32 kB je považován za 32 události, každý s nastavenou velikostí 1 KB. Událost maximální povolená velikost je 32 KB; se zkrátí datových paketů větší než 32 KB.


## <a name="monitor-latency-and-throttling-with-alerts"></a>Latence monitorování a omezování s výstrahami

Výstrahy můžete pomoci diagnostikovat a zmírnit problémy s latencí způsobené prostředí. 

1. Na portálu Azure klikněte na tlačítko **metriky**. 

   ![Metriky](media/environment-mitigate-latency/add-metrics.png)

2. Klikněte na tlačítko **přidat metriky upozornění**.  

    ![Přidání metriky upozornění](media/environment-mitigate-latency/add-metric-alert.png)

Odtud můžete nakonfigurovat výstrahy pomocí následující metriky:

|Metrika  |Popis  |
|---------|---------|
|**Příjem příchozích dat přijatých bajtů**     | Počet nezpracovaných Bajty čtení z zdroje událostí. Nezpracovaná počet obvykle zahrnuje název vlastnosti a hodnotu.  |  
|**Příjem příchozích dat přijata neplatná zprávy**     | Počet zpráv neplatná přečíst ze všech zdrojů událostí Azure Event Hubs nebo Azure IoT Hub.      |
|**Příjem příchozích dat přijatých zpráv**   | Počet zpráv číst ze zdroje událostí pro všechny služby Event Hubs nebo centra IoT.        |
|**Příjem příchozích dat uložené bajtů**     | Celková velikost událostí uložených a k dispozici pro dotaz. Velikost se počítá pouze na hodnotu vlastnosti.        |
|**Příjem příchozích dat uložené události**     |   Počet plochou události uložené a k dispozici pro dotaz.      |

![Latence](media/environment-mitigate-latency/latency.png)

Jednoho technika je nastavit **uložené události příchozího** výstrahu > = prahová hodnota mírně nižší než vaše kapacita celkový prostředí pro 2 hodin.  Tato výstraha může pomoct pochopit, pokud jste neustále na kapacitě, který označuje vysokou pravděpodobnost latence.  

Například pokud máte tři jednotky S1 zřízený (nebo 2100 událostí za minutu příchozího kapacity), můžete nastavit **uložené události příchozího** výstrahy pro > = 1900 události další 2 hodiny. Pokud jsou neustále překročení této mezní hodnoty a proto, která aktivuje upozornění, budete pravděpodobně pod připravené.  

Navíc pokud máte podezření, je omezené, můžete porovnat vaše **příjem příchozích dat přijatých zpráv** s vaší událostí je egressed zdroj zprávy.  Pokud příjem příchozích dat do vašeho centra událostí je větší než vaše **příjem příchozích dat přijatých zpráv**, časové řady přehledy jsou pravděpodobně omezené.

## <a name="improving-performance"></a>Zvýšení výkonu 
Ke snížení omezení nebo zaznamenat latenci, je nejlepší způsob, jak opravte ji chcete zvýšit kapacitu pro vaše prostředí. 

Latence a omezování tak, že správně nakonfigurujete prostředí pro množství dat, který chcete analyzovat se můžete vyhnout. Další informace o tom, jak přidat kapacitu pro vaše prostředí najdete v tématu [škálování prostředí](time-series-insights-how-to-scale-your-environment.md).

## <a name="next-steps"></a>Další kroky
- Pro další kroky odstraňování [Diagnostikujte a řešení problémů ve vašem prostředí časové řady Insights](time-series-insights-diagnose-and-solve-problems.md).
- Další pomoc získáte zahájit konverzaci na [fórum MSDN](https://social.msdn.microsoft.com/Forums/home?forum=AzureTimeSeriesInsights) nebo [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-timeseries-insights). Můžete také kontaktovat [podporu Azure](https://azure.microsoft.com/support/options/) možnosti odborné pomoci řešení.
