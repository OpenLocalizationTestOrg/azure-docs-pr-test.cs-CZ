---
title: "propustnost tooincrease úlohy Stream Analytics aaaScale | Microsoft Docs"
description: "Zjistěte, jak úlohy Stream Analytics tooscale nakonfigurováním vstupní oddíly, ladění hello Definice dotazu a nastavení úlohy jednotek streamování."
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
ms.openlocfilehash: 4ba8f6b2f8bfebd52cfa07696b501b42cda21f75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scale-azure-stream-analytics-jobs-tooincrease-stream-data-processing-throughput"></a>Škálování Azure Stream Analytics úlohy tooincrease datového proudu zpracování dat propustnosti
Tento článek ukazuje, jak tootune Stream Analytics dotaz tooincrease propustnost pro úlohy streamování Analytics. Zjistíte, jak tooscale Stream Analytics úlohy nakonfigurováním vstupní oddíly, definici dotazu vyladění hello analýzy a výpočet a nastavení úlohy *jednotek streaming* (SUs). 

## <a name="what-are-hello-parts-of-a-stream-analytics-job"></a>Jaké jsou části hello úlohy Stream Analytics?
Definice úlohy Stream Analytics obsahuje vstupy, dotaz a výstup. Vstupní hodnoty jsou, kde úloha hello čte hello datový proud z. Hello dotazu je použité tootransform hello dat vstupní datový proud a výstup hello je, kde hello odesílá výsledky úlohy hello.  

Úloha vyžaduje alespoň jeden vstupní zdroj pro streamování data. Hello vstupní zdroj dat datového proudu může být uložená v Centru událostí Azure nebo v Azure blob storage. Další informace najdete v tématu [tooAzure Úvod Stream Analytics](stream-analytics-introduction.md) a [začít používat Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md).

## <a name="partitions-in-event-hubs-and-azure-storage"></a>Oddíly v centrům událostí a úložiště Azure
Škálování úloha Stream Analytics využívá výhod oddíly v hello vstup nebo výstup. Vytváření oddílů umožňuje data rozdělíte do podmnožin podle klíč oddílu. Proces, který využívá hello data (například úlohu streamování Analytics) může spotřebovávat a zápis různých oddílů souběžně, což zvyšuje propustnost. Při práci s streamování analýzy, můžete využít výhod vytváření oddílů v centra událostí a v úložišti objektů Blob. 

Další informace o oddílech najdete hello následující články:

* [Přehled funkcí služby Event Hubs](../event-hubs/event-hubs-features.md#partitions)
* [Segmentace dat](https://docs.microsoft.com/azure/architecture/best-practices/data-partitioning#partitioning-azure-blob-storage)


## <a name="streaming-units-sus"></a>Jednotky streamování (SUs)
Streamování jednotky (SUs) představují hello prostředků a výpočetních napájení, které jsou nutné v pořadí tooexecute úlohu služby Azure Stream Analytics. Služba SUs poskytují způsob toodescribe hello relativní zpracování událostí založené na kombinaci měření procesoru, paměti, kapacity a čtení a zápisu sazby. Každý SU odpovídá tooroughly 1 MB za sekundu, propustnosti. 

Výběr kolik služby SUs jsou požadovány pro konkrétní úlohy závisí na konfiguraci hello oddílů pro vstupy hello a na dotaz hello definovaný pro úlohy hello. Můžete vybrat až tooyour kvótu v služby SUs pro úlohu. Ve výchozím nastavení má každé předplatné Azure kvótu až služby SUs too50 pro všechny úlohy analýzy hello v určité oblasti. tooincrease služby SUs pro vaše předplatné nad rámec této kvóty, obraťte se na [Microsoft Support](http://support.microsoft.com). Platné hodnoty pro služby SUs na úlohu jsou 1, 3, 6 a až v přírůstcích po 6.

## <a name="embarrassingly-parallel-jobs"></a>Trapně paralelní úlohy
*Paralelně zpracovatelné* úlohy je nejvíce škálovatelným scénář hello máme v Azure Stream Analytics. Připojí se jeden oddíl hello vstupní tooone instance hello dotazu tooone oddílu hello výstupu. Tato paralelismus má hello následující požadavky:

1. Pokud logika dotazu závisí na stejný klíč zpracovává hello podle hello stejný dotaz instance, ujistěte se, že události hello přejděte toohello stejného oddílu váš vstup. Pro službu event hubs to znamená, že data události hello musí mít hello **PartitionKey** hodnotu sady. Alternativně můžete použít odesílatelé oddílů. Úložiště objektů blob, to znamená, že hello události se posílají toohello stejné složce oddílu. Pokud logika dotazu nevyžaduje hello stejný klíč toobe zpracovat podle hello stejný dotaz instance, můžete ignorovat tento požadavek. Příkladem této logiky, může být jednoduchý dotaz vyberte filtr projektu.  

2. Jakmile hello data na straně vstupní hello rozložená, musí se ujistěte, že váš dotaz je rozdělený do oddílů. To vyžaduje, abyste toouse **Partition By** v všechny kroky hello. Jsou povoleny několik kroků, ale musí mít všechny oddíly podle hello stejný klíč. V současné době hello klíč rozdělení do oddílů musí být nastaven příliš**PartitionId** v pořadí pro plně paralelní toobe úlohy hello.  

3. Pouze služba event hubs a úložiště objektů blob v současné době podporují dělené výstup. Pro výstupu centra událostí, musíte nakonfigurovat toobe klíče oddílu hello **PartitionId**. Pro výstup úložiště objektů blob nemáte toodo nic.  

4. Hello počet vstupních oddílů musí být roven hello počet oddílů výstup. Výstup úložiště objektů BLOB aktuálně nepodporuje oddíly. Ale to nevadí, protože dědí hello dělení schéma hello nadřazeného dotazu. Zde jsou příklady oddílu hodnot, které umožňují plně paralelní úlohy:  

   * 8 oddíly vstupní centra událostí a Centrum událostí 8 výstupní oddíly
   * 8 oddíly vstupní centra událostí a výstup úložiště objektů blob  
   * 8 oddíly vstupní úložiště objektů blob a výstup úložiště objektů blob  
   * 8 blob oddílů pro úložiště a 8 oddíly výstupu centra událostí  

Hello následující části popisují některé ukázkové scénáře, které jsou jednoduše paralelně zpracovatelné.

### <a name="simple-query"></a>Jednoduchý dotaz

* Vstup: Centra událostí s 8 oddíly
* Výstup: Centra událostí s 8 oddíly

Dotaz:

    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100

Dotaz je jednoduchý filtr. Proto jsme nepotřebují tooworry o vytváření oddílů hello vstupu, který je odesílán toohello centra událostí. Všimněte si, že dotaz hello obsahuje **oddílu podle PartitionId**, takže se splní požadavek #2 z dříve. Pro výstup hello potřebujeme výstupu centra událostí tooconfigure hello sady hello úlohy toohave hello vytvoření oddílu klíče příliš**PartitionId**. Jeden poslední kontrola je toomake se, že hello počet vstupních oddílů je rovna toohello počet oddílů výstup.

### <a name="query-with-a-grouping-key"></a>Dotaz s klíčem seskupení

* Vstup: Centra událostí s 8 oddíly
* Výstup: Úložiště objektů Blob

Dotaz:

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Tento dotaz obsahuje seskupení klíč. Proto hello stejné toobe klíčových požadavků zpracovává hello stejný dotaz instance, což znamená, že události, musí se poslat centra událostí toohello oddílů způsobem. Ale které klíč by měl použít? **PartitionId** je koncept úlohy logiku. klíč Hello skutečně záleží nám je **TollBoothId**, takže hello **PartitionKey** hodnota dat událostí hello by měla být **TollBoothId**. Můžeme provést v dotazu hello nastavením **Partition By** příliš**PartitionId**. Vzhledem k tomu, že výstup hello je úložiště objektů blob, společnost Microsoft nepotřebuje tooworry o konfiguraci hodnotu klíče oddílu, podle požadavků #4.

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

Tento dotaz obsahuje seskupení klíč, takže hello stejné toobe klíčových požadavků zpracovává hello stejnou instanci dotazu. Můžeme použít hello stejné strategie jako v předchozím příkladu hello. V takovém případě hello dotaz má několik kroků. Má každý krok **oddílu podle PartitionId**? Ano, takže hello dotazu splní požadavek #3. Pro výstup hello potřebujeme klíč oddílu hello tooset příliš**PartitionId**, jak je popsáno výše. Také uvidíme, že má hello stejný počet oddílů jako vstup hello.

## <a name="example-scenarios-that-are-not-embarrassingly-parallel"></a>Ukázkové scénáře, které jsou *není* paralelně zpracovatelné

V předchozí části hello jsme vám ukázal některé paralelně zpracovatelné scénáře. V této části probereme scénáře, které nesplňují všechny požadavky hello toobe paralelně zpracovatelné. 

### <a name="mismatched-partition-count"></a>Počet neodpovídající oddílů
* Vstup: Centra událostí s 8 oddíly
* Výstup: Centra událostí s 32 oddílů

V takovém případě nezávisle na tom, jaký dotaz hello je. Pokud počet vstupních oddílů hello se neshoduje se počet oddílů výstup hello, není paralelně zpracovatelné hello topologie.

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

Jak vidíte, druhý krok hello používá **TollBoothId** jako hello klíč rozdělení do oddílů. Tento krok není hello stejné jako první krok text hello, a proto vyžaduje nám toodo náhodně. 

Hello předchozí příklady ukazují, některé úlohy Stream Analytics, které vyhovují příliš (nebo nemusíte) paralelně zpracovatelné topologie. Pokud jsou v souladu, mají hello potenciální pro maximální měřítko. Pro úlohy, které se nehodí jeden z těchto profilů škálování pokyny bude k dispozici v budoucích aktualizací. Prozatím použijte hello obecné pokyny v následující části hello.

## <a name="calculate-hello-maximum-streaming-units-of-a-job"></a>Vypočítat hello maximální jednotek streamování úlohy
Celkový počet jednotek streamování, které lze použít v rámci úlohy Stream Analytics Hello závisí na hello počet kroků v dotazu hello definovány pro hello úlohy a hello počet oddílů pro každý krok.

### <a name="steps-in-a-query"></a>Kroky v dotazu
Dotaz může mít jeden nebo mnoho kroků. Každý krok je poddotaz definované hello **WITH** – klíčové slovo. Hello dotazu, který je mimo hello **WITH** – klíčové slovo (pouze jeden dotaz) se také počítá jako krok, jako je například hello **vyberte** příkaz v hello následující dotaz:

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
> Tento dotaz je podrobněji popsána dále v článku hello.
>  

### <a name="partition-a-step"></a>Oddílu krok
Vytváření oddílů krok vyžaduje hello následující podmínky:

* vstupní zdroj Hello musí mít oddíly. 
* Hello **vyberte** výraz dotazu hello musí ke čtení z oddílů vstupní zdroj.
* dotaz Hello v rámci kroku hello musí mít hello **Partition By** – klíčové slovo.

Při dotazu je rozdělena na oddíly, hello vstupních událostech se zpracování a agregované v samostatném oddílu skupiny a výstupy události jsou generovány pro každou ze skupin hello. Pokud chcete, aby součet agregace, je nutné vytvořit druhý tooaggregate krok bez oddílů.

### <a name="calculate-hello-max-streaming-units-for-a-job"></a>Vypočítat hello maximální počet jednotek pro úlohu streamování
Všechny kroky bez oddílů společně můžete postupně škálovat toosix jednotky (SUs) u úlohy Stream Analytics streamování. Služba SUs tooadd, krok musí mít oddíly. Každý oddíl můžete mít šest služby SUs.

<table border="1">
<tr><th>Dotaz</th><th>Služba SUs Max pro úlohu hello</th></td>

<tr><td>
<ul>
<li>Hello dotaz obsahuje jeden krok.</li>
<li>Krok Hello není rozdělena na oddíly.</li>
</ul>
</td>
<td>6</td></tr>

<tr><td>
<ul>
<li>Hello vstupní datový proud je rozdělena na oddíly 3.</li>
<li>Hello dotaz obsahuje jeden krok.</li>
<li>Krok Hello je rozdělena na oddíly.</li>
</ul>
</td>
<td>18</td></tr>

<tr><td>
<ul>
<li>Hello dotaz obsahuje dva kroky.</li>
<li>Ani jeden z kroků hello je rozdělena na oddíly.</li>
</ul>
</td>
<td>6</td></tr>

<tr><td>
<ul>
<li>Hello vstupní datový proud je rozdělena na oddíly 3.</li>
<li>Hello dotaz obsahuje dva kroky. vstupní krok Hello je rozdělena na oddíly a druhý krok hello není.</li>
<li>Hello <strong>vyberte</strong> příkaz čte ze vstupu hello rozdělena na oddíly.</li>
</ul>
</td>
<td>24 (oddílů kroky 18) + 6 pokyny bez oddílů</td></tr>
</table>

### <a name="examples-of-scaling"></a>Příklady škálování

Hello následující dotaz vypočítá hello počet aut, v rámci průchodu přes projedou stanice, která má tři tollbooths časového období tři minuty. Tento dotaz je možné škálovat toosix služby SUs.

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

toouse další služby SUs pro hello dotazu, obě hello vstupního datového proudu a musí mít oddíly hello dotazu. Vzhledem k tomu, že hello datový proud oddíl nastavena too3, hello následující změny dotazu je možné škálovat služby SUs too18:

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Při dotazu je rozdělena na oddíly, hello vstupních událostech se zpracovat a agregovat v samostatném oddílu skupiny. Výstup události jsou také vytvářeny pro každou ze skupin hello. Dělení může způsobit, že některé neočekávané výsledky při hello **GROUP BY** pole není klíč oddílu hello v hello vstupního datového proudu. Například hello **TollBoothId** pole v hello předchozího dotazu není klíč oddílu hello **Input1**. Hello výsledkem je, že hello data z mýtná celnice č. 1 možné rozdělit do několika oddílů.

Každý z hello **Input1** oddíly se zpracovávají odděleně podle Stream Analytics. V důsledku toho více záznamů počtu hello car pro hello stejné mýtná celnice v hello stejné Přeskakující okno bude vytvořen. Pokud klíč hello vstupní oddílu nelze změnit, můžete tento problém opravit tak, že přidáte krok bez oddílu, jako hello následující ukázka:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId

Tento dotaz lze škálovat too24 SUs.

> [!NOTE]
> Pokud jsou připojení dvě datové proudy, ujistěte se, že datové proudy hello jsou rozdělena na oddíly pomocí klíče oddílu hello hello sloupce použít toocreate hello spojení. Také se ujistěte, že máte hello stejný počet oddílů v obou datových proudů.
> 
> 

## <a name="configure-stream-analytics-streaming-units"></a>Konfigurace služby Stream Analytics jednotky streamování

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. V seznamu hello prostředků najdete úlohu Stream Analytics hello má tooscale a potom ho otevřete.
3. V hello úlohy okno, v části **konfigurace**, klikněte na tlačítko **škálování**.

    ![Azure konfigurace portálu úlohy Stream Analytics][img.stream.analytics.preview.portal.settings.scale]

4. Použijte hello posuvníku tooset hello služby SUs pro úlohu hello. Všimněte si, že jste omezené toospecific SU nastavení.


## <a name="monitor-job-performance"></a>Monitorování výkonu úlohy
Pomocí hello portálu Azure, můžete sledovat hello propustnost úlohy:

![Azure Stream Analytics monitorování úloh][img.stream.analytics.monitor.job]

Vypočítejte propustnost hello očekávání pro zatížení hello. Pokud hello propustnost je menší než se očekávalo, ladit hello vstupní oddílu, ladit hello dotazu a přidat úloha tooyour služby SUs.


## <a name="visualize-stream-analytics-throughput-at-scale-hello-raspberry-pi-scenario"></a>Vizualizace Stream Analytics propustnost škálované: hello malin pí scénář
toohelp víte, jak úlohy Stream Analytics škálování, jsme provedli experimentu založené na vstupu z malin platformy zařízení. Tento experiment dejte nám najdete v části hello vliv na propustnost několika streamování jednotky a oddíly.

V tomto scénáři hello zařízení odesílá centra událostí tooan senzor dat (klientů). Streamování Analytics zpracovává hello data a odešle výstrahu nebo statistiky jako výstup tooanother centra událostí. 

Hello klient odešle data snímačů ve formátu JSON. výstup Hello dat je také ve formátu JSON. Hello data vypadá takto:

    {"devicetime":"2014-12-11T02:24:56.8850110Z","hmdt":42.7,"temp":72.6,"prss":98187.75,"lght":0.38,"dspl":"R-PI Olivier's Office"}

Hello následující dotaz je použité toosend výstrahu, když je vypnuté světlý:

    SELECT AVG(lght),
     "LightOff" as AlertText
    FROM input TIMESTAMP
    BY devicetime
     WHERE
        lght< 0.05 GROUP BY TumblingWindow(second, 1)

### <a name="measure-throughput"></a>Míra propustnost

V tomto kontextu je propustnost hello množství vstupních dat zpracovávaných Stream Analytics pevné množství času. (Jsme měří 10 minut.) tooachieve hello nejlepší zpracování propustnost pro hello vstupní data, data i hello stream vstup a hello dotazu byly rozdělena na oddíly. Jsme zahrnuté **COUNT()** v toomeasure hello dotazu byly zpracovány kolik vstupních událostech. zda úlohy hello toomake nebyl jednoduše čekání toocome vstupních událostech, každý oddíl centra událostí vstupní hello se předem načtou přibližně 300 MB vstupní data.

Hello následující tabulka uvádí hello výsledků, které jsme viděli při odpovídající oddíl hello počty ve službě event hubs a jsme vyšší hello počet jednotek streamování.  

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

A hello následující graf ukazuje vizualizaci hello vztah mezi službou SUs a propustnosti.

![IMG.Stream.Analytics.perfgraph][img.stream.analytics.perfgraph]

## <a name="get-help"></a>Podpora
Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Další kroky
* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
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

