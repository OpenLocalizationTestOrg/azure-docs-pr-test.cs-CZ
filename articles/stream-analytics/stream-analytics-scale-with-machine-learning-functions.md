---
title: "aaaJob škálování s Azure Stream Analytics & AzureML funkce | Microsoft Docs"
description: "Zjistěte, jak tooproperly škálování úlohy Stream Analytics (rozdělení do oddílů, SU množství a více) při použití funkce Azure Machine Learning."
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 47ce7c5e-1de1-41ca-9a26-b5ecce814743
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 3fbdfaf7e8e86896c56f1d18bbde3a10bd3dca04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scale-your-stream-analytics-job-with-azure-machine-learning-functions"></a>Škálovat vaše úloha Stream Analytics s funkcemi Azure Machine Learning
Často je poměrně snadné tooset až úlohu služby Stream Analytics a procházely ukázková data. Co můžeme udělat, když budeme potřebovat toorun hello stejné úlohy s vyšší datový svazek? To nám vyžaduje toounderstand jak tooconfigure hello Stream Analytics úlohy tak, aby se bude škálovat. V tomto dokumentu se zaměříme na hello zvláštní aspekty škálování služby Stream Analytics úlohy se funkce Machine Learning. Informace o tom, jak úlohy Stream Analytics tooscale obecně najdete článku hello [škálování úlohy](stream-analytics-scale-jobs.md).

## <a name="what-is-an-azure-machine-learning-function-in-stream-analytics"></a>Co je Azure Machine Learning funkce v Stream Analytics?
Funkce Machine Learning v Stream Analytics lze použít jako volání regulární funkce v hello Stream Analytics query language. Však za hello scény hello volání funkce je ve skutečnosti žádostí o webovou službu Azure Machine Learning. Machine Learning webové služby podpory "dávkování" více řádků, kterému se říká zkrácená batch, v hello stejné webové služby volání rozhraní API, tooimprove celkovou propustnost. Podrobnosti najdete na následující články pro další podrobnosti; hello [Funkce azure Machine Learning v Stream Analytics](https://blogs.technet.microsoft.com/machinelearning/2015/12/10/azure-ml-now-available-as-a-function-in-azure-stream-analytics/) a [webové služby Azure Machine Learning](../machine-learning/machine-learning-consume-web-services.md).

## <a name="configure-a-stream-analytics-job-with-machine-learning-functions"></a>Úloha Stream Analytics nakonfigurovat funkce Machine Learning
Při konfiguraci funkce Machine Learning úlohy Stream Analytics, existují dva parametry tooconsider, velikost dávky hello hello volání funkce Machine Learning a hello jednotky (SUs) zřízené pro úlohy služby Stream Analytics hello streamování. hello příslušnými hodnotami toodetermine pro tyto, nejprve musí být přijato rozhodnutí mezi latence a propustnosti, to znamená, latenci úlohy Stream Analytics hello a propustnost každý SU. Služby SUs vždy lze přidat tooa úlohy tooincrease propustnost dobře oddílů dotazu Stream Analytics, i když další služby SUs zvyšuje náklady hello spuštěná úloha hello.

Proto je důležité toodetermine hello *tolerance* latence ve spuštění úlohy Stream Analytics. Další spuštění žádosti o služby Azure Machine Learning zvýší se latence přirozeně s velikost dávky, která bude složené hello latence úlohy Stream Analytics hello. Na hello druhé straně, zvýšit velikost dávky umožňuje tooprocess úlohy Stream Analytics hello * další události s hello *stejné číslo* nástroje Machine Learning webové žádosti o služby. Proto je důležité tooconsider hello velikost na nejvíce cenově efektivní dávky pro webové služby Machine Learning v jakékoliv jiné situaci s danou, je často hello zvýšení latence služby Machine Learning webové dílčí lineární toohello zvýšení velikosti dávky. Hello výchozí velikost dávky pro webovou službu hello požadavky je 1000 a může být změněno buď pomocí hello [Stream Analytics REST API](https://msdn.microsoft.com/library/mt653706.aspx "Stream Analytics REST API") nebo hello [klienta PowerShell pro Stream Analytics](stream-analytics-monitor-and-manage-jobs-use-powershell.md "klienta PowerShell pro Stream Analytics").

Po určil velikost dávky množství hello streamování jednotky (SUs) může být určené, na základě hello počet událostí, které potřebuje funkce hello tooprocess za sekundu. Další informace o jednotky streamování najdete v tématu [škálování úlohy Stream Analytics](stream-analytics-scale-jobs.md).

Obecně je 20 souběžných připojení toohello webové službě Machine Learning pro každých 6 služby SUs s tím rozdílem, že 1 SU úlohy a úlohy 3 SU získají 20 souběžných připojení také.  Například pokud míra vstupních dat hello je 200 000 událostí za sekundu a velikost dávky hello je ponechán toohello výchozí 1000 hello výsledný web latence služby pomocí služby batch zkrácená 1000 událostí je 200 MS. To znamená, že každé připojení provádět 5 požadavky webové službě Machine Learning toohello za sekundu. S 20 připojení zpracovávat úlohy služby Stream Analytics hello 20 000 událostí ve 200 MS a proto 100 000 událostí za sekundu. Proto tooprocess 200 000 událostí za sekundu, hello úlohy služby Stream Analytics musí 40 souběžných připojení, která dodává se službou SUs too12. Hello obrázek níže znázorňuje hello požadavky od hello Stream Analytics úlohy toohello Machine Learning koncový bod webové služby – každých 6 SUs má 20 souběžných připojení tooMachine Learning webové služby v max.

![Škálování služby Stream Analytics se Machine Learning funkce 2 úlohy příklad](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-00.png "škálování Stream Analytics příklad úlohy Machine Learning funkce 2")

Obecně platí ***B*** pro velikost dávky ***L*** hello webové služby latence na velikost dávky B v milisekundách, text hello propustnost úlohu služby Stream Analytics s ***N*** služby SUs je:

![Škálování služby Stream Analytics s Machine Learning funkce vzorec](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-02.png "škálování služby Stream Analytics s vzorec funkce Machine Learning")

Další aspekt může být hello 'maximální počet souběžných volání' na hello Machine Learning webové služby straně, se doporučuje tooset tuto toohello maximální hodnotu (aktuálně 200).

Další informace o tomto nastavení najdete v tématu hello [škálování najdete v článku webové služby Machine Learning](../machine-learning/machine-learning-scaling-webservice.md).

## <a name="example--sentiment-analysis"></a>Příklad – postojích analýzy
Hello následující příklad obsahuje úlohy Stream Analytics hello postojích analýzy funkce Machine Learning, jak je popsáno v hello [Stream Analytics Machine Learning integrace kurzu](stream-analytics-machine-learning-integration-tutorial.md).

Hello dotaz je jednoduchý dotaz plně oddílů, který následuje hello **postojích** fungovat, jak je uvedeno níže:

    WITH subquery AS (
        SELECT text, sentiment(text) as result from input
    )

    Select text, result.[Score]
    Into output
    From subquery

Zvažte následující scénáře; hello s propustností 10 000 tweetů za sekundu úloha Stream Analytics se musí vytvořit tooperform postojích analýzu hello tweetů (událostí). Pomocí 1 SU, může tato úloha Stream Analytics být schopný toohandle hello provozu? Pomocí hello výchozí velikost dávky 1000 hello úlohy by měl být schopný tookeep až se vstupem hello. Další hello přidaná funkce Machine Learning měl generovat víc než jedna sekunda latence, což je výchozí obecné latence hello hello postojích analýzy webové službě Machine Learning (s výchozí velikost dávky 1000). Úloha Stream Analytics Hello **celkové** nebo latence začátku do konce by obvykle na několik sekund. Podrobnější podívejte se do této úlohy Stream Analytics *zejména* hello volání funkce Machine Learning. Velikost dávky hello s jako 1000, propustnost 10 000 událostí, které bude trvat asi 10 požadavky tooweb služby. I když 1 SU, jsou dostatečný počet souběžných připojení tooaccommodate to vstupní provoz.

Ale co když rychlost vstupní událostí hello zvyšuje úroveň 100 x a úloha Stream Analytics hello potřebuje teď tooprocess 1 000 000 tweetů za sekundu? Existují dvě možnosti:

1. Zvětšete velikost dávky hello, nebo
2. Oddíl hello vstupního datového proudu tooprocess hello události paralelně

První možností hello hello úlohy **latence** zvýší.

Další služby SUs s hello druhá možnost se by potřebovat toobe zřízený a proto generovat více souběžných žádosti webové služby Machine Learning. To znamená hello úlohy **náklady** zvýší.

Předpokládá, že hello latence hello postojích analýzy webové službě Machine Learning je 200 MS pro dávky 1000 událostí nebo níže 250ms pro dávky 5 000 událostí, 300ms pro dávky 10 000 událostí nebo 500ms pro dávky 25 000 událostí.

1. Pomocí hello první možnosti, (**není** zřizování další služby SUs), velikost dávky hello mohlo dojít ke zvýšení příliš**25 000**. To zase umožní hello úlohy tooprocess 1 000 000 událostí s 20 souběžných připojení toohello webové službě Machine Learning (s latencí 500ms za hovor). Proto hello další latence úlohy Stream Analytics hello kvůli toohello postojích funkce požadavky DHCP proti hello Machine Learning žádosti webové služby by být zvýšena z **200 MS** příliš**500ms**. Ale Všimněte si, že velikost dávky **nelze** být zvýšena nekonečnou jako hello webových služeb Machine Learning vyžaduje velikost datové části hello požadavku se 4 MB volného místa nebo menší web časový limit žádosti o službu po 100 sekundách operace.
2. Pomocí hello druhou možnost, velikost dávky hello je ponechány na 1000, s 200 MS webové služby latencí, každých 20 souběžných připojení toohello webová služba bude mít tooprocess 1000 * 20 * 5 události = 100 000 za sekundu. 1 000 000 událostí tooprocess na druhé, úlohu hello, takže by měli 60 služby SUs. Porovnání toohello první možnost se Stream Analytics úlohy by zkontrolujte další webové žádosti o služby batch, zase generování vyšší náklady.

Níže je tabulka pro hello propustnost hello Stream Analytics úlohy pro různé služby SUs a velikostí dávky (v počtu událostí za sekundu).

| velikost dávky (ML latence) | 500 (200 MS) | 1 000 (200 MS) | 5 000 (250ms) | 10 000 (300ms) | 25 000 (500ms) |
| --- | --- | --- | --- | --- | --- |
| **1 SU** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **3 služby SUs** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **6 služby SUs** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **12 služby SUs** |5,000 |10 000 |40,000 |60,000 |100,000 |
| **18 služby SUs** |7,500 |15,000 |60,000 |90,000 |150,000 |
| **24 služby SUs** |10 000 |20,000 |80,000 |120,000 |200 000 |
| **…** |… |… |… |… |… |
| **60 služby SUs** |25,000 |50,000 |200 000 |300,000 |500,000 |

Nyní by byste již měli mít dostatečné povědomí o tom, jak funkce Machine Learning v Stream Analytics fungovat. Je pravděpodobně také pochopit, že úlohy Stream Analytics "pro vyžádání obsahu" data ze zdroje dat a každý "vyžádání" vrátí dávky události pro hello tooprocess úlohy Stream Analytics. Tom, jak tento model pull ovlivnit žádosti webové služby hello Machine Learning?

Za normálních okolností hello velikost dávky nastavený pro Machine Learning funkce nesmí být přesně dělitelná hello počet událostí vrácených Každá úloha Stream Analytics "vyžádání". Když k tomu dojde, že hello webové službě Machine Learning bude volána s "částečné" dávky. Děje se tak toonot toho vám být účtovány další úlohy latence režie v slučování událostí z toopull vyžádání obsahu.

## <a name="new-function-related-monitoring-metrics"></a>Nové funkce související s monitorování metriky
V oblasti monitorování úlohy Stream Analytics hello byly přidány tři další metriky týkající se funkce. Jak je znázorněno v následujícím hello obrázku jsou žádosti o funkce, funkce události a NEÚSPĚŠNÉ požadavky funkce.

![Škálování služby Stream Analytics s Machine Learning funkce metriky](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-01.png "škálování služby Stream Analytics s Machine Learning funkce metriky")

Hello jsou definovány takto:

**POŽADAVKY funkce**: hello počet požadavků na funkce.

**Funkce události**: hello číslo události v hello funkce požadavky.

**NEÚSPĚŠNÉ požadavky funkce**: hello počet požadavků neúspěšné funkce.

## <a name="key-takeaways"></a>Klíče Takeaways
toosummarize hello hlavní body v pořadí tooscale úlohu služby Stream Analytics s funkcemi, Machine Learning, hello následující položky je třeba zvážit:

1. rychlost vstupní událostí Hello
2. Hello tolerovat latence pro spuštění úlohy Stream Analytics hello (a tedy velikost dávky hello webové služby Machine Learning hello požadavky)
3. Hello zřízení služby Stream Analytics SUs a hello počet žádosti webové služby Machine Learning (hello další funkce nákladů souvisejících s)

Jako příklad byl použit dotaz služby Stream Analytics plně oddílů. Pokud je potřeba komplexnější dotaz hello [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) je skvělým zdrojem pro získávání potřebujete další pomoc od týmu Stream Analytics hello.

## <a name="next-steps"></a>Další kroky
toolearn Další informace o Stream Analytics, najdete v části:

* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
