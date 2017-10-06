---
title: "aaaQuery příkladů běžných vzorů využití v Stream Analytics | Microsoft Docs"
description: "Běžné typy dotazů Azure Stream Analytics"
keywords: "Příklady dotazů"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jenniehubbard
editor: cgronlun
ms.assetid: 6b9a7d00-fbcc-42f6-9cbb-8bbf0bbd3d0e
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/08/2017
ms.author: jenniehubbard
ms.openlocfilehash: c8f7a8ac661eaf0281f4140b02c42141b73040fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="query-examples-for-common-stream-analytics-usage-patterns"></a>Dotaz příkladů běžných vzorů využití Stream Analytics
## <a name="introduction"></a>Úvod
Dotazy v Azure Stream Analytics jsou vyjádřeny v jazyce SQL jako dotaz. Tyto dotazy jsou popsané v hello [Stream Analytics query referenční informace k jazyku](https://msdn.microsoft.com/library/azure/dn834998.aspx) průvodce. Tento článek obsahuje přehled řešení tooseveral běžné typy dotazů, na základě reálných scénářů. Je pracuje a pokračuje toobe nové vzory průběžně aktualizovat.

## <a name="query-example-convert-data-types"></a>Příklad dotazu: Převést datové typy
**Popis**: definování typů hello vlastností na hello vstupního datového proudu.
Například hello car váhy pochází na hello vstupního datového proudu jako řetězce a je třeba toobe převést příliš**INT** tooperform **součet** ho nahoru.

**Vstup**:

| Ujistěte se | Čas | Váha |
| --- | --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |"1000" |
| Honda |2015-01-01T00:00:02.0000000Z |"2000" |

**Výstup**:

| Ujistěte se | Váha |
| --- | --- |
| Honda |3000 |

**Řešení**:

    SELECT
        Make,
        SUM(CAST(Weight AS BIGINT)) AS Weight
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Vysvětlení**: použití **PŘETYPOVÁNÍ** příkaz v hello **váhy** pole toospecify jeho datového typu. Zobrazit seznam hello podporované datové typy v [datové typy (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835065.aspx).

## <a name="query-example-use-likenot-like-toodo-pattern-matching"></a>Příklad dotazu: použijte jako nebo nebyla jako toodo shoda vzoru
**Popis**: Zkontrolujte, jestli hodnotu pole na hello události odpovídá určitého.
Například zkontrolujte, že hello výsledek vrátí desky licencí, které se začínat a končit 9.

**Vstup**:

| Ujistěte se | LicensePlate | Čas |
| --- | --- | --- |
| Honda |ABC 123 |2015-01-01T00:00:01.0000000Z |
| Toyota |AAA-999 |2015-01-01T00:00:02.0000000Z |
| Nissan |ABC 369 |2015-01-01T00:00:03.0000000Z |

**Výstup**:

| Ujistěte se | LicensePlate | Čas |
| --- | --- | --- |
| Toyota |AAA-999 |2015-01-01T00:00:02.0000000Z |
| Nissan |ABC 369 |2015-01-01T00:00:03.0000000Z |

**Řešení**:

    SELECT
        *
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LicensePlate LIKE 'A%9'

**Vysvětlení**: použití hello **jako** příkaz toocheck hello **LicensePlate** pole hodnota. By měl začínat A potom mít libovolný řetězec nula nebo více znaků a pak končit 9. 

## <a name="query-example-specify-logic-for-different-casesvalues-case-statements"></a>Příklad dotazu: Zadejte logiku pro různé případech nebo hodnoty (CASE – příkazy)
**Popis**: Zadejte jiný výpočet pro pole, v závislosti na konkrétní kritérium.
Například zadejte popis řetězce, pro kolik aut hello stejné zkontrolujte byla dokončena s ve speciálním případě pro 1.

**Vstup**:

| Ujistěte se | Čas |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Výstup**:

| CarsPassed | Čas |
| --- | --- | --- |
| 1 Honda |2015-01-01T00:00:10.0000000Z |
| 2 Toyotas |2015-01-01T00:00:10.0000000Z |

**Řešení**:

    SELECT
        CASE
            WHEN COUNT(*) = 1 THEN CONCAT('1 ', Make)
            ELSE CONCAT(CAST(COUNT(*) AS NVARCHAR(MAX)), ' ', Make, 's')
        END AS CarsPassed,
        System.TimeStamp AS Time
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Vysvětlení**: hello **případ** klauzule umožňuje tooprovide různých výpočtů, na základě některé kritérií (v našem případě hello počet aut hello v okně agregační hello).

## <a name="query-example-send-data-toomultiple-outputs"></a>Příklad dotazu: Odeslat data toomultiple výstupy
**Popis**: odesílání dat toomultiple výstup cíle z jediné úlohy.
Například analyzovat data pro upozornění na základě prahové hodnoty a všechny události tooblob úložiště archivu.

**Vstup**:

| Ujistěte se | Čas |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Output1**:

| Ujistěte se | Čas |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Output2**:

| Ujistěte se | Čas | Počet |
| --- | --- | --- |
| Toyota |2015-01-01T00:00:10.0000000Z |3 |

**Řešení**:

    SELECT
        *
    INTO
        ArchiveOutput
    FROM
        Input TIMESTAMP BY Time

    SELECT
        Make,
        System.TimeStamp AS Time,
        COUNT(*) AS [Count]
    INTO
        AlertOutput
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)
    HAVING
        [Count] >= 3

**Vysvětlení**: hello **INTO** klauzule informuje Stream Analytics, které hello výstupy toowrite hello data toofrom tohoto prohlášení.
první dotaz Hello je průchozí hello dat dostali jsme tooan výstupu, který jsme pojmenovali **ArchiveOutput**.
druhý dotaz Hello nepodporuje některé jednoduché agregace a filtrování a odešle výsledky hello tooa podřízené výstrahy systému.

Všimněte si, že můžete také znovu použít hello výsledky hello běžných výrazech tabulky (odkazu Cte) (například **WITH** příkazy) ve více příkazů výstup. Tato možnost je hello dodatečná výhoda otevření méně čtečky toohello vstupní zdroj.
Například: 

    WITH AllRedCars AS (
        SELECT
            *
        FROM
            Input TIMESTAMP BY Time
        WHERE
            Color = 'red'
    )
    SELECT * INTO HondaOutput FROM AllRedCars WHERE Make = 'Honda'
    SELECT * INTO ToyotaOutput FROM AllRedCars WHERE Make = 'Toyota'

## <a name="query-example-count-unique-values"></a>Příklad dotazu: počet jedinečné hodnoty
**Popis**: počet hello jedinečné pole hodnoty, které se zobrazují v datovém proudu hello v rámci časové okno.
Například kolik jedinečný díky automobilů předána hello projedou stánek v okně sekundu 2?

**Vstup**:

| Ujistěte se | Čas |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Výstup:**

| Počet | Čas |
| --- | --- |
| 2 |2015-01-01T00:00:02.000Z |
| 1 |2015-01-01T00:00:04.000Z |

**Řešení:**

````
SELECT
     COUNT(DISTINCT Make) AS CountMake,
     System.TIMESTAMP AS TIME
FROM Input TIMESTAMP BY TIME
GROUP BY 
     TumblingWindow(second, 2)
````


**Vysvětlení:**
**COUNT (odlišné ověřte)** hello vrátí počet jedinečných hodnot v hello **zkontrolujte** sloupec v rámci časové okno.

## <a name="query-example-determine-if-a-value-has-changed"></a>Příklad dotazu: určení, pokud byla hodnota změněna
**Popis**: Podívejte se na předchozí toodetermine hodnotu, pokud se liší od aktuální hodnota hello.
Je třeba předchozí car hello na hello projedou silniční hello, které stejná jako aktuální car hello provést?

**Vstup**:

| Ujistěte se | Čas |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |

**Výstup**:

| Ujistěte se | Čas |
| --- | --- |
| Toyota |2015-01-01T00:00:02.0000000Z |

**Řešení**:

    SELECT
        Make,
        Time
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(minute, 1)) <> Make

**Vysvětlení**: použití **PRODLEVA** toopeek do hello vstupní datový proud zpět jedné události a získat hello **zkontrolujte** hodnotu. Porovnejte je toohello **zkontrolujte** hodnotu na aktuální a výstup hello události hello, pokud se liší.

## <a name="query-example-find-hello-first-event-in-a-window"></a>Příklad dotazu: najít hello první událost v okně
**Popis**: najít hello prvního auta v intervalu každých 10 minut.

**Vstup**:

| LicensePlate | Ujistěte se | Čas |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| YZK 5704 |Ford |2015-07-27T00:02:17.0000000Z |
| RMV 8282 |Honda |2015-07-27T00:05:01.0000000Z |
| YHN 6970 |Toyota |2015-07-27T00:06:00.0000000Z |
| VFE 1616 |Toyota |2015-07-27T00:09:31.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Výstup**:

| LicensePlate | Ujistěte se | Čas |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |

**Řešení**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) = 1

Teď vytvoříme změnu hello problému a najít hello prvního auta konkrétní třídy v intervalu každých 10 minut.

| LicensePlate | Ujistěte se | Čas |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| YZK 5704 |Ford |2015-07-27T00:02:17.0000000Z |
| YHN 6970 |Toyota |2015-07-27T00:06:00.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Řešení**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) OVER (PARTITION BY Make) = 1

## <a name="query-example-find-hello-last-event-in-a-window"></a>Příklad dotazu: najít hello poslední událost v okně
**Popis**: najít hello poslední car v intervalu každých 10 minut.

**Vstup**:

| LicensePlate | Ujistěte se | Čas |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| YZK 5704 |Ford |2015-07-27T00:02:17.0000000Z |
| RMV 8282 |Honda |2015-07-27T00:05:01.0000000Z |
| YHN 6970 |Toyota |2015-07-27T00:06:00.0000000Z |
| VFE 1616 |Toyota |2015-07-27T00:09:31.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Výstup**:

| LicensePlate | Ujistěte se | Čas |
| --- | --- | --- |
| VFE 1616 |Toyota |2015-07-27T00:09:31.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Řešení**:

    WITH LastInWindow AS
    (
        SELECT 
            MAX(Time) AS LastEventTime
        FROM 
            Input TIMESTAMP BY Time
        GROUP BY 
            TumblingWindow(minute, 10)
    )
    SELECT 
        Input.LicensePlate,
        Input.Make,
        Input.Time
    FROM
        Input TIMESTAMP BY Time 
        INNER JOIN LastInWindow
        ON DATEDIFF(minute, Input, LastInWindow) BETWEEN 0 AND 10
        AND Input.Time = LastInWindow.LastEventTime

**Vysvětlení**: existují dva kroky v dotazu hello. Hello první jeden najde hello nejnovější časové razítko v systému windows 10 minut. druhý krok spojení Hello hello výsledky hello první dotaz s hello původní datový proud toofind hello události, které odpovídají hello poslední časová razítka v každém okně. 

## <a name="query-example-detect-hello-absence-of-events"></a>Příklad dotazu: zjištění hello absence událostí
**Popis**: Zkontrolujte, že datový proud má žádná hodnota, která odpovídá určité kritérium.
Například 2 po sobě jdoucích automobily od hello stejné provedený zadali hello projedou silniční v rámci hello posledních 90 sekund?

**Vstup**:

| Ujistěte se | LicensePlate | Čas |
| --- | --- | --- |
| Honda |ABC 123 |2015-01-01T00:00:01.0000000Z |
| Honda |AAA-999 |2015-01-01T00:00:02.0000000Z |
| Toyota |DEF 987 |2015-01-01T00:00:03.0000000Z |
| Honda |GHI 345 |2015-01-01T00:00:04.0000000Z |

**Výstup**:

| Ujistěte se | Čas | CurrentCarLicensePlate | FirstCarLicensePlate | FirstCarTime |
| --- | --- | --- | --- | --- |
| Honda |2015-01-01T00:00:02.0000000Z |AAA-999 |ABC 123 |2015-01-01T00:00:01.0000000Z |

**Řešení**:

    SELECT
        Make,
        Time,
        LicensePlate AS CurrentCarLicensePlate,
        LAG(LicensePlate, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarLicensePlate,
        LAG(Time, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarTime
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(second, 90)) = Make

**Vysvětlení**: použití **PRODLEVA** toopeek do hello vstupní datový proud zpět jedné události a získat hello **zkontrolujte** hodnotu. Porovnejte je toohello **ZKONTROLUJTE** hodnota hello aktuální událost a potom výstupní hello událost, pokud jejich jsou stejné hello. Můžete také použít **PRODLEVA** tooget data o předchozí car hello.

## <a name="query-example-detect-hello-duration-between-events"></a>Příklad dotazu: zjistit dobu trvání hello mezi událostmi
**Popis**: najít hello trvání dané události. Zadané webové clickstream, například určete hello čas strávený na funkce.

**Vstup**:  

| Uživatel | Funkce | Událost | Čas |
| --- | --- | --- | --- |
| user@location.com |RightMenu |Start |2015-01-01T00:00:01.0000000Z |
| user@location.com |RightMenu |End |2015-01-01T00:00:08.0000000Z |

**Výstup**:  

| Uživatel | Funkce | Doba trvání |
| --- | --- | --- |
| user@location.com |RightMenu |7 |

**Řešení**:

````
    SELECT
        [user], feature, DATEDIFF(second, LAST(Time) OVER (PARTITION BY [user], feature LIMIT DURATION(hour, 1) WHEN Event = 'start'), Time) as duration
    FROM input TIMESTAMP BY Time
    WHERE
        Event = 'end'
````

**Vysvětlení**: použití hello **poslední** funkce tooretrieve hello poslední **čas** hodnota při typ události hello **spustit**. Hello **poslední** využívá **PARTITION BY [user]** tooindicate, který hello výsledek se počítá na jedinečný uživatele. dotaz Hello má maximální prahovou hodnotu 1 hodina hello časový rozdíl mezi **spustit** a **Zastavit** události, ale je možné konfigurovat podle potřeby **(LIMIT DURATION(hour, 1)**.

## <a name="query-example-detect-hello-duration-of-a-condition"></a>Příklad dotazu: zjistit dobu trvání hello podmínku
**Popis**: Najít se na jak dlouho podmínku došlo k chybě.
Předpokládejme například, že chyby výsledkem všechny aut, které mají nesprávné váhu (nad 20 000 libra). Chceme toocompute hello trvání hello chyb.

**Vstup**:

| Ujistěte se | Čas | Váha |
| --- | --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |2000 |
| Toyota |2015-01-01T00:00:02.0000000Z |25000 |
| Honda |2015-01-01T00:00:03.0000000Z |26000 |
| Toyota |2015-01-01T00:00:04.0000000Z |25000 |
| Honda |2015-01-01T00:00:05.0000000Z |26000 |
| Toyota |2015-01-01T00:00:06.0000000Z |25000 |
| Honda |2015-01-01T00:00:07.0000000Z |26000 |
| Toyota |2015-01-01T00:00:08.0000000Z |2000 |

**Výstup**:

| StartFault | EndFault |
| --- | --- |
| 2015-01-01T00:00:02.000Z |2015-01-01T00:00:07.000Z |

**Řešení**:

````
    WITH SelectPreviousEvent AS
    (
    SELECT
    *,
        LAG([time]) OVER (LIMIT DURATION(hour, 24)) as previousTime,
        LAG([weight]) OVER (LIMIT DURATION(hour, 24)) as previousWeight
    FROM input TIMESTAMP BY [time]
    )

    SELECT 
        LAG(time) OVER (LIMIT DURATION(hour, 24) WHEN previousWeight < 20000 ) [StartFault],
        previousTime [EndFault]
    FROM SelectPreviousEvent
    WHERE
        [weight] < 20000
        AND previousWeight > 20000
````

**Vysvětlení**: použití **PRODLEVA** tooview hello vstupního datového proudu pro 24 hodin a podívejte se na instance, kde **StartFault** a **StopFault** jsou předané hello Váha < 20000.

## <a name="query-example-fill-missing-values"></a>Příklad dotazu: vyplnění chybějící hodnoty
**Popis**: pro hello datový proud události, které mají chybějící hodnoty, vytvořit datový proud událostí s pravidelných intervalech.
Například generovat událost každých 5 sekund, který ohlašuje hello naposledy vidět datového bodu.

**Vstup**:

| T | hodnota |
| --- | --- |
| "2014-01-01T06:01:00" |1 |
| "2014-01-01T06:01:05" |2 |
| "2014-01-01T06:01:10" |3 |
| "2014-01-01T06:01:15" |4 |
| "2014-01-01T06:01:30" |5 |
| "2014-01-01T06:01:35" |6 |

**Výstup (prvních 10 řádků)**:

| windowend | lastevent.t | lastevent.Value |
| --- | --- | --- |
| 2014-01-01T14:01:00.000Z |2014-01-01T14:01:00.000Z |1 |
| 2014-01-01T14:01:05.000Z |2014-01-01T14:01:05.000Z |2 |
| 2014-01-01T14:01:10.000Z |2014-01-01T14:01:10.000Z |3 |
| 2014-01-01T14:01:15.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:20.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:25.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:30.000Z |2014-01-01T14:01:30.000Z |5 |
| 2014-01-01T14:01:35.000Z |2014-01-01T14:01:35.000Z |6 |
| 2014-01-01T14:01:40.000Z |2014-01-01T14:01:35.000Z |6 |
| 2014-01-01T14:01:45.000Z |2014-01-01T14:01:35.000Z |6 |

**Řešení**:

    SELECT
        System.Timestamp AS windowEnd,
        TopOne() OVER (ORDER BY t DESC) AS lastEvent
    FROM
        input TIMESTAMP BY t
    GROUP BY HOPPINGWINDOW(second, 300, 5)


**Vysvětlení**: Tento dotaz vygeneruje události každých 5 sekund a výstupy hello poslední událost, který jste získali dříve. Hello [Hopping okno](https://msdn.microsoft.com/library/dn835041.aspx "Hopping okno – Azure Stream Analytics") doba trvání Určuje, jak daleko back hello dotazu vypadá toofind hello nejnovější událost (v tomto příkladu je 300 sekund).

## <a name="get-help"></a>Podpora
Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Další kroky
* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

