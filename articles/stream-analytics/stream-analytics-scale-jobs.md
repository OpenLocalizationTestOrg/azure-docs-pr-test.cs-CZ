---
title: "Škálování úlohy Stream Analytics, pokud chcete zvýšit propustnost | Microsoft Docs"
description: "Postup konfigurace vstupní oddíly, ladění definice dotazu a nastavení úlohu streamování jednotky škálování úlohy Stream Analytics."
keywords: "data streamování, streamování zpracování dat, optimalizovat analytics"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 7e857ddb-71dd-4537-b7ab-4524335d7b35
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/22/2017
ms.author: jeffstok
ms.openlocfilehash: ab894976c72ea3785d7f58e51b3dd64511e1e8e3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="scale-azure-stream-analytics-jobs-to-increase-stream-data-processing-throughput"></a>Škálování služby Stream Analytics chcete zvýšit propustnost dat zpracování datového proudu
Tento článek ukazuje, jak ladit dotaz služby Stream Analytics chcete zvýšit propustnost pro úlohy streamování Analytics. Další postup škálování úlohy Stream Analytics podle konfigurace vstupní oddíly, ladění definice dotazu analýzy a výpočet a nastavení úlohy *jednotek streaming* (SUs). 

## <a name="what-are-the-parts-of-a-stream-analytics-job"></a>Jaké jsou součástí úlohy Stream Analytics?
Definice úlohy Stream Analytics obsahuje vstupy, dotaz a výstup. Vstupní hodnoty jsou, kde úloha načte datový proud z. Dotaz se používá k transformaci dat vstupního datového proudu a výstup je, kde úloha odešle na výsledky úlohy.  

Úloha vyžaduje alespoň jeden vstupní zdroj pro streamování data. Vstupní zdroj dat datového proudu můžete ukládat v Centru událostí Azure nebo v Azure blob storage. Další informace najdete v tématu [Úvod do služby Azure Stream Analytics](stream-analytics-introduction.md) a [začít používat Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md).

## <a name="partitions-in-event-hubs-and-azure-storage"></a>Oddíly v centrům událostí a úložiště Azure
Škálování úloha Stream Analytics využívá oddílů v vstup nebo výstup. Vytváření oddílů umožňuje data rozdělíte do podmnožin podle klíč oddílu. Proces, který používá data (například úlohu streamování Analytics) může spotřebovávat a zápis různých oddílů souběžně, což zvyšuje propustnost. Při práci s streamování analýzy, můžete využít výhod vytváření oddílů v centra událostí a v úložišti objektů Blob. 

Další informace o oddílech najdete v následujících článcích:

* [Přehled funkcí služby Event Hubs](../event-hubs/event-hubs-features.md#partitions)
* [Segmentace dat](https://docs.microsoft.com/azure/architecture/best-practices/data-partitioning#partitioning-azure-blob-storage)


## <a name="streaming-units-sus"></a>Jednotky streamování (SUs)
Jednotky streamování (SUs) představují prostředků a výpočetní výkon, které jsou nutné, aby bylo možné spustit úlohu služby Azure Stream Analytics. Jednotky SU umožňují popsat relativní kapacitu zpracování událostí na základě výkonu procesoru, paměti a rychlosti čtení a zápisu. Každý SU odpovídá přibližně 1 MB za sekundu propustnost. 

Výběr kolik služby SUs jsou požadovány pro konkrétní úlohy závisí na konfiguraci oddílů pro vstupy a na dotaz definovaný pro úlohu. Můžete vybrat až vaší kvóty v služby SUs pro úlohu. Ve výchozím nastavení má každé předplatné Azure kvótu až 50 služby SUs pro všechny úlohy analýzy v určité oblasti. Chcete-li zvýšit služby SUs pro vaše předplatné nad rámec této kvóty, obraťte se na [Microsoft Support](http://support.microsoft.com). Platné hodnoty pro služby SUs na úlohu jsou 1, 3, 6 a až v přírůstcích po 6.

## <a name="embarrassingly-parallel-jobs"></a>Trapně paralelní úlohy
*Paralelně zpracovatelné* úlohy je nejvíce škálovatelným scénář máme v Azure Stream Analytics. Připojí se jeden oddíl vstupu do jedné instance dotazu do jednoho oddílu výstupu. Tato paralelismus má následující požadavky:

1. Pokud logika dotazu závisí na stejný klíč zpracovávaných stejnou instanci dotazu, musí se ujistěte, že události přejděte do stejného oddílu váš vstup. Pro službu event hubs to znamená, že data události musí mít **PartitionKey** hodnotu sady. Alternativně můžete použít odesílatelé oddílů. Pro úložiště objektů blob to znamená, že události budou odeslány do stejné složky oddílu. Pokud dotaz logiky nevyžaduje stejný klíč zpracování na stejnou instanci dotazu, můžete ignorovat tento požadavek. Příkladem této logiky, může být jednoduchý dotaz vyberte filtr projektu.  

2. Jakmile data rozložená na vstupní straně, musí se ujistěte, že váš dotaz je rozdělený do oddílů. To vyžaduje, abyste použili **Partition By** v všechny kroky. Jsou povoleny několik kroků, ale musí mít všechny oddíly pomocí stejného klíče. V současné době rozdělení klíč musí být nastavena na **PartitionId** v pořadí pro úlohu, která má být plně paralelní.  

3. Pouze služba event hubs a úložiště objektů blob v současné době podporují dělené výstup. Pro výstupu centra událostí, je nutné nakonfigurovat jako klíč oddílu **PartitionId**. Pro výstup úložiště objektů blob nemusíte provádět žádné kroky.  

4. Počet vstupních oddílů musí být roven počtu oddílů výstup. Výstup úložiště objektů BLOB aktuálně nepodporuje oddíly. Ale to nevadí, protože dědí schéma rozdělení oddílů nadřazeného dotazu. Zde jsou příklady oddílu hodnot, které umožňují plně paralelní úlohy:  

   * 8 oddíly vstupní centra událostí a Centrum událostí 8 výstupní oddíly
   * 8 oddíly vstupní centra událostí a výstup úložiště objektů blob  
   * 8 oddíly vstupní úložiště objektů blob a výstup úložiště objektů blob  
   * 8 blob oddílů pro úložiště a 8 oddíly výstupu centra událostí  

Následující části popisují některé ukázkové scénáře, které jsou jednoduše paralelně zpracovatelné.

### <a name="simple-query"></a>Jednoduchý dotaz

* Vstup: Centra událostí s 8 oddíly
* Výstup: Centra událostí s 8 oddíly

Dotaz:

    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100

Dotaz je jednoduchý filtr. Proto jsme nemusíte starat o vytváření oddílů vstup odeslaný do centra událostí. Všimněte si, že dotaz obsahuje **oddílu podle PartitionId**, takže se splní požadavek #2 z dříve. Pro výstup, je potřeba nakonfigurovat výstupu centra událostí v projektu na sadu klíče k vytvoření oddílu **PartitionId**. Jeden poslední kontrola je počet vstupních oddílů musí být roven počtu oddílů výstup.

### <a name="query-with-a-grouping-key"></a>Dotaz s klíčem seskupení

* Vstup: Centra událostí s 8 oddíly
* Výstup: Úložiště objektů Blob

Dotaz:

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Tento dotaz obsahuje seskupení klíč. Proto je potřeba zpracovat ve stejné instanci dotaz, což znamená, že musí události posílají do centra událostí oddílů způsobem stejný klíč. Ale které klíč by měl použít? **PartitionId** je koncept úlohy logiku. Klíč skutečně záleží nám je **TollBoothId**, proto **PartitionKey** hodnota data události by měla být **TollBoothId**. Můžeme provést v dotazu nastavením **Partition By** k **PartitionId**. Vzhledem k tomu, že výstupem je úložiště objektů blob, jsme nemusíte si dělat starosti o konfiguraci hodnotu klíče oddílu, podle požadavků #4.

### <a name="multi-step-query-with-a-grouping-key"></a>Vícekrokový dotazu s klíčem seskupení
* Vstup: Centra událostí s 8 oddíly
* Výstup: Instance rozbočovače událostí s 8 oddíly

Dotaz:

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Tento dotaz obsahuje seskupení klíč, takže stejný klíč je potřeba zpracovat stejné instance dotazu. Můžeme použít stejné strategie jako v předchozím příkladu. Dotaz v tomto případě má několik kroků. Má každý krok **oddílu podle PartitionId**? Ano, takže dotaz splní požadavek #3. Výstup, je nutné nastavit klíč oddílu na **PartitionId**, jak je popsáno výše. Jsme můžete také zjistit, že nemá stejný počet oddílů jako vstup.

## <a name="example-scenarios-that-are-not-embarrassingly-parallel"></a>Ukázkové scénáře, které jsou *není* paralelně zpracovatelné

V předchozí části jsme vám ukázal některé paralelně zpracovatelné scénáře. V této části probereme scénáře, které nejsou splněny všechny požadavky být jednoduše paralelně zpracovatelné. 

### <a name="mismatched-partition-count"></a>Počet neodpovídající oddílů
* Vstup: Centra událostí s 8 oddíly
* Výstup: Centra událostí s 32 oddílů

V takovém případě nezávisle na tom, co je dotaz. Pokud počet vstupních oddílů se neshoduje se počet oddílů výstup, není paralelně zpracovatelné topologii.

### <a name="not-using-event-hubs-or-blob-storage-as-output"></a>Jako výstup není pomocí služby event hubs nebo úložiště objektů blob
* Vstup: Centra událostí s 8 oddíly
* Výstup: PowerBI

Výstup PowerBI aktuálně nepodporuje vytváření oddílů. Proto tento scénář není paralelně zpracovatelné.

### <a name="multi-step-query-with-different-partition-by-values"></a>Vícekrokový dotazu s různými hodnotami Partition By
* Vstup: Centra událostí s 8 oddíly
* Výstup: Centra událostí s 8 oddíly

Dotaz:

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId

Jak vidíte, druhý krok používá **TollBoothId** jako klíč rozdělení. Tento krok není stejný jako v prvním kroku, a proto vyžaduje nám udělat náhodně. 

Některé úlohy Stream Analytics, které odpovídat (nebo nemusíte) paralelně zpracovatelné topologie v předchozích příkladech. Pokud jsou v souladu, mají potenciální pro maximální měřítko. Pro úlohy, které se nehodí jeden z těchto profilů škálování pokyny bude k dispozici v budoucích aktualizací. Prozatím použijte obecné pokyny v následujících částech.

## <a name="calculate-the-maximum-streaming-units-of-a-job"></a>Vypočítat maximální počet jednotek úlohy streamování
Celkový počet jednotek streamování, které lze použít v rámci úlohy Stream Analytics závisí na počtu kroků v dotazu definovány pro úlohy a počet oddílů pro každý krok.

### <a name="steps-in-a-query"></a>Kroky v dotazu
Dotaz může mít jeden nebo mnoho kroků. Každý krok je poddotaz definované **WITH** – klíčové slovo. Dotaz, který je mimo **WITH** – klíčové slovo (pouze jeden dotaz) také považován za krok, například **vyberte** příkaz v následujícím dotazu:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId

Tento dotaz má dva kroky.

> [!NOTE]
> Tento dotaz je podrobněji popsána dále v tomto článku.
>  

### <a name="partition-a-step"></a>Oddílu krok
Vytváření oddílů krok vyžaduje následující podmínky:

* Vstupní zdroj, musí mít oddíly. 
* **Vyberte** příkaz dotazu musí ke čtení z oddílů vstupní zdroj.
* Dotaz v rámci kroku, musí mít **Partition By** – klíčové slovo.

Při dotazu je rozdělena na oddíly, vstupních událostech se zpracování a agregované v samostatném oddílu skupiny a výstupy události jsou generovány pro každou skupinu. Pokud chcete, aby součet agregace, musíte vytvořit druhý krok bez oddílů k agregaci.

### <a name="calculate-the-max-streaming-units-for-a-job"></a>Vypočítat maximální počet jednotek pro úlohu streamování
Všechny kroky bez oddílů společně můžete škálovat až šest jednotky streamování (SUs) u úlohy Stream Analytics. Pokud chcete přidat služby SUs, musí mít oddíly krok. Každý oddíl můžete mít šest služby SUs.

<table border="1">
<tr><th>Dotaz</th><th>Služba SUs Max pro úlohu</th></td>

<tr><td>
<ul>
<li>Dotaz obsahuje jeden krok.</li>
<li>V kroku není rozdělena na oddíly.</li>
</ul>
</td>
<td>6</td></tr>

<tr><td>
<ul>
<li>Vstupní datový proud je rozdělena na oddíly 3.</li>
<li>Dotaz obsahuje jeden krok.</li>
<li>V kroku je rozdělena na oddíly.</li>
</ul>
</td>
<td>18</td></tr>

<tr><td>
<ul>
<li>Dotaz obsahuje dva kroky.</li>
<li>Ani jeden z kroků je rozdělena na oddíly.</li>
</ul>
</td>
<td>6</td></tr>

<tr><td>
<ul>
<li>Vstupní datový proud je rozdělena na oddíly 3.</li>
<li>Dotaz obsahuje dva kroky. Vstupní krok je rozdělena na oddíly a druhý krok není.</li>
<li><strong>Vyberte</strong> příkaz čte z oddílů vstup.</li>
</ul>
</td>
<td>24 (oddílů kroky 18) + 6 pokyny bez oddílů</td></tr>
</table>

### <a name="examples-of-scaling"></a>Příklady škálování

Následující dotaz vypočítá počet aut, v rámci průchodu přes projedou stanice, která má tři tollbooths časového období tři minuty. Tento dotaz je možné rozšířit až šest služby SUs.

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Pokud chcete používat další služby SUs pro dotaz, musí mít oddíly vstupní datový proud a dotazu. Vzhledem k tomu, že oddíl datový proud dat je nastaven na 3, tyto změny dotazu je možné rozšířit až 18 služby SUs:

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Při dotazu je rozdělena na oddíly, vstupních událostech se zpracovat a agregovat v samostatném oddílu skupiny. Výstup události jsou také vytvářeny pro každou skupinu. Dělení může způsobit, že některé neočekávané výsledky při **GROUP BY** pole není klíč oddílu v vstupní datový proud. Například **TollBoothId** pole v předchozího dotazu není klíč oddílu **Input1**. Výsledkem je, že data z mýtná celnice č. 1 možné rozdělit do několika oddílů.

Každý z **Input1** oddíly se zpracovávají odděleně podle Stream Analytics. V důsledku toho bude vytvořen více záznamů počtu car pro stejné mýtná celnice ve stejném okně Přeskakující. Pokud klíč vstupní oddílu nelze změnit, můžete tento problém opravit tak, že přidáte krok bez oddílu, jako v následujícím příkladu:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId

Tento dotaz můžete škálovat na 24 služby SUs.

> [!NOTE]
> Pokud jsou připojení dvě datové proudy, ujistěte se, že datové proudy jsou rozdělena na oddíly pomocí klíče oddílu sloupce, který použijete při jejich vytváření. Ujistěte se také mít stejný počet oddílů v obou datových proudů.
> 
> 

## <a name="configure-stream-analytics-streaming-units"></a>Konfigurace služby Stream Analytics jednotky streamování

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V seznamu prostředků najděte úlohu služby Stream Analytics, kterou chcete škálovat a potom ho otevřete.
3. V okně úlohy v části **konfigurace**, klikněte na tlačítko **škálování**.

    ![Azure konfigurace portálu úlohy Stream Analytics][img.stream.analytics.preview.portal.settings.scale]

4. K nastavení služby SUs úlohy použijte posuvníku. Všimněte si, že jste omezeni na konkrétní nastavení SU.


## <a name="monitor-job-performance"></a>Monitorování výkonu úlohy
Pomocí portálu Azure, můžete sledovat propustnost úlohy:

![Azure Stream Analytics monitorování úloh][img.stream.analytics.monitor.job]

Vypočítejte očekávané propustnost zatížení. Pokud propustnost je menší než se očekávalo, ladit vstupní oddílu, odladění dotazu a přidejte služby SUs na úlohu.


## <a name="visualize-stream-analytics-throughput-at-scale-the-raspberry-pi-scenario"></a>Vizualizace Stream Analytics propustnost škálované: scénář malin platformy
Abyste lépe pochopili, jak úlohy Stream Analytics škálování, jsme provedli experimentu založené na vstupu z malin platformy zařízení. Tento experiment dejte nám najdete v části vliv na propustnost několika streamování jednotky a oddíly.

V tomto scénáři zařízení odesílá data snímačů (klientů) do centra událostí. Streamování Analytics zpracovává data a odešle výstrahu nebo statistiky jako výstup do jiného centra událostí. 

Klient odešle data snímačů ve formátu JSON. Výstup dat je také ve formátu JSON. Data vypadá takto:

    {"devicetime":"2014-12-11T02:24:56.8850110Z","hmdt":42.7,"temp":72.6,"prss":98187.75,"lght":0.38,"dspl":"R-PI Olivier's Office"}

Následující dotaz se používá k odesílání výstrahu, pokud je vypnuté světlý:

    SELECT AVG(lght),
     "LightOff" as AlertText
    FROM input TIMESTAMP
    BY devicetime
     WHERE
        lght< 0.05 GROUP BY TumblingWindow(second, 1)

### <a name="measure-throughput"></a>Míra propustnost

V tomto kontextu propustnost je množství vstupních dat zpracovávaných Stream Analytics pevné množství času. (Jsme měří 10 minut.) K dosažení nejlepší propustnost zpracování vstupních dat, byly oddíly vstup datového streamu a dotazu. Jsme zahrnuté **COUNT()** v dotazu k měření počtu vstupních událostí byly zpracovány. Pokud chcete mít jistotu, že úloha nebyla jednoduše čekání vstupních událostech pochází, každý oddíl centra vstupní událostí se předem načtou přibližně 300 MB vstupní data.

V následující tabulce jsou uvedeny výsledků, které jsme viděli při odpovídající oddíl počty ve službě event hubs a jsme zvýšit počet jednotek streamování.  

<table border="1">
<tr><th>Vstupní oddíly</th><th>Výstup oddíly</th><th>Jednotky streamování</th><th>Propustnost
</th></td>

<tr><td>12</td>
<td>12</td>
<td>6</td>
<td>4.06 MB/s</td>
</tr>

<tr><td>12</td>
<td>12</td>
<td>12</td>
<td>8.06 MB/s</td>
</tr>

<tr><td>48</td>
<td>48</td>
<td>48</td>
<td>38.32 MB/s</td>
</tr>

<tr><td>192</td>
<td>192</td>
<td>192</td>
<td>172.67 MB/s</td>
</tr>

<tr><td>480</td>
<td>480</td>
<td>480</td>
<td>454.27 MB/s</td>
</tr>

<tr><td>720</td>
<td>720</td>
<td>720</td>
<td>609.69 MB/s</td>
</tr>
</table>

A následující graf ukazuje vizualizaci vztah mezi službou SUs a propustnosti.

![IMG.Stream.Analytics.perfgraph][img.stream.analytics.perfgraph]

## <a name="get-help"></a>Podpora
Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Další kroky
* [Úvod do služby Azure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor-NewPortal.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings-NewPortal.png   

<!--Link references-->

[microsoft.support]: http://support.microsoft.com
[azure.management.portal]: http://manage.windowsazure.com
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

