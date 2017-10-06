---
title: "aaaDebug Azure Stream Analytics dotazy pomocí SELECT INTO | Microsoft Docs"
description: "Ukázkový dotaz střední data pomocí příkazů SELECT INTO v Stream Analytics"
keywords: 
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 9952e2cf-b335-4a5c-8f45-8d3e1eda2e20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: 27e41af1a6ea06b4509d07a3a67087490d0ec1fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="debug-queries-by-using-select-into-statements"></a>Ladění dotazy pomocí příkazů SELECT INTO

Při zpracování dat v reálném čase zároveň budete vědět, jaká data hello vypadá uprostřed hello hello dotazu může být užitečné. Protože vstupů nebo kroky úlohu služby Azure Stream Analytics lze číst vícekrát, můžete napsat doplňující příkazy SELECT INTO. Díky tomu výstupy mezilehlá data do úložiště a umožňuje vám zkontrolujte správnost hello hello dat, stejně jako *sledovat proměnné* provést při ladění programu.

## <a name="use-select-into-toocheck-hello-data-stream"></a>Použití SELECT INTO toocheck hello datový proud

Hello následující příklad dotazu v úloze služby Azure Stream Analytics má jeden datový proud vstup, dvěma vstupy referenční data a tooAzure výstupní tabulka úložiště. dotaz Hello spojí dat z centra událostí hello a odkaz na dva objekty BLOB tooget hello název a informace o kategorii:

![Příklad SELECT INTO dotazu](./media/stream-analytics-select-into/stream-analytics-select-into-query1.png)

Všimněte si, že je spuštěná úloha hello, ale žádné události se vytváří ve výstupu hello. Na hello **monitorování** dlaždici zobrazeny zde, uvidíte, že vstup hello je generovala data, ale nevíte, který krok hello **připojení** způsobeny všechny hello toobe události vyřadit.

![dlaždice monitorování Hello](./media/stream-analytics-select-into/stream-analytics-select-into-monitor.png)
 
V takovém případě můžete přidat několik navíc příkazech SELECT INTO příliš "" hello mezilehlých výsledků spojení a protokolu hello data, která je pro čtení z hello vstup.

V tomto příkladu jsme přidali dvě nové "dočasné výstupy." Může se jednat žádné podřízený, které se vám líbí. Tady používáme Azure Storage jako příklad:

![Přidání další příkazy SELECT INTO](./media/stream-analytics-select-into/stream-analytics-select-into-outputs.png)

Potom můžete přepsat pomocí dotazu hello takto:

![Přepsaná dotaz SELECT INTO](./media/stream-analytics-select-into/stream-analytics-select-into-query2.png)

Teď hello úlohu znovu spustit a nechat ji spustit několik minut. Potom budete dotazovat temp1 a temp2 s cloudu Průzkumníka Visual Studio tooproduce hello následujících tabulek:

**Tabulka temp1**
![SELECT INTO temp1 tabulky](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-1.png)

**Tabulka temp2**
![SELECT INTO temp2 tabulky](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-2.png)

Jak vidíte, temp1 a temp2 mají data a hello název sloupce je v temp2 správně zadána. Ale protože ve výstupu stále neexistuje žádná data, je něco špatně:

![Vyberte do tabulky output1 bez dat](./media/stream-analytics-select-into/stream-analytics-select-into-out-table-1.png)

Vzorkování dat hello, můžete si být téměř jisti, že hello problém s hello druhé připojení. Můžete stáhnout hello referenční data z objektu blob hello a podívejte se:

![SELECT INTO ref tabulky](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-1.png)

Jak vidíte, se liší od hello formát hello [z] sloupce v temp2 hello formát hello identifikátor GUID v této referenční data. To je důvod, proč hello data nebyla doručení output1 podle očekávání.

Opravte formát dat hello, nahrajte ho tooreference objektů blob a opakujte akci:

![Vyberte do dočasné tabulky](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-2.png)

Tento čas hello data ve výstupu hello je naformátován a vyplní podle očekávání.

![SELECT INTO poslední tabulky](./media/stream-analytics-select-into/stream-analytics-select-into-final-table.png)


## <a name="get-help"></a>Podpora

Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Další kroky

* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

