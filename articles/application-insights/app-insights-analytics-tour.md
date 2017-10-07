---
title: "prohlídka aaaA prostřednictvím analýzy ve službě Azure Application Insights | Microsoft Docs"
description: "Krátký vzorky všechny dotazy hlavní hello v analýzy, hello vyhledávání výkonné nástroje Application Insights."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: bddf4a6d-ea8d-4607-8531-1fe197cc57ad
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/06/2017
ms.author: bwren
ms.openlocfilehash: c268e26c6bf93ac2ee2a9d5e83613150dcf90b04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="a-tour-of-analytics-in-application-insights"></a>Prohlídka Analytics ve službě Application Insights
[Analýza](app-insights-analytics.md) je výkonný vyhledávání funkcí hello [Application Insights](app-insights-overview.md). Tyto stránek popisují dotazovací jazyk analýzy protokolů.

* **[Podívejte se na úvodní video hello](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Vyzkoušejte Analytics na naše simulované data](https://analytics.applicationinsights.io/demo)**  Pokud aplikace není ještě odesílá data tooApplication statistiky.
* **[SQL-uživatelů tahák](https://aka.ms/sql-analytics)**  překládá nejběžnější idioms hello.

Podívejme procházení prostřednictvím některé tooget základní dotazy, které jste spustili.

## <a name="connect-tooyour-application-insights-data"></a>Připojení dat tooyour Application Insights
Otevřete Analytics z vaší aplikace [okno Přehled](app-insights-dashboards.md) ve službě Application Insights:

![Otevřete portal.azure.com otevřete prostředek Application Insights a klikněte na Analytics.](./media/app-insights-analytics-tour/001.png)

## <a name="takehttpsdocsloganalyticsioquerylanguagequerylanguagetakeoperatorhtml-show-me-n-rows"></a>[Trvat](https://docs.loganalytics.io/queryLanguage/query_language_takeoperator.html): Zobrazit mi n řádků
Datové body, které protokolu operace uživatele (obvykle HTTP přijatých požadavků ve vaší webové aplikace) jsou uložené v tabulce s názvem `requests`. Každý řádek je telemetrie datový bod, který přijal od hello Application Insights SDK ve vaší aplikaci.

Začněme prověřením několik ukázkových řádky tabulky hello:

![výsledky](./media/app-insights-analytics-tour/010.png)

> [!NOTE]
> Uveďte hello kurzoru někde v příkazu hello před kliknutím na Přejít. Příkaz můžete rozdělit přes více než jeden řádek, ale nemáte Vložit prázdné řádky v příkazu. Prázdné řádky jsou pohodlný způsob tookeep několik jednotlivých dotazů v okně hello.
>
>

Vyberte sloupce, přetáhněte je seskupit podle sloupce a filtrování:

![Klikněte na výběr sloupců v pravé horní části výsledků](./media/app-insights-analytics-tour/030.png)

Rozbalte všechny podrobnosti hello toosee položek:

![Vyberte tabulky a použití konfigurace sloupců](./media/app-insights-analytics-tour/040.png)

> [!NOTE]
> Klikněte na tlačítko hello head k dispozici ve webovém prohlížeči hello sloupec výsledků hello toore pořadí. Ale uvědomte si, že pro sadu velké výsledek hello počet řádků stažené toohello prohlížeče je omezená. Řazení tímto způsobem není vždy zobrazí hello skutečné nejvyšší nebo nejnižší položky. položky toosort spolehlivě, použijte hello `top` nebo `sort` operátor.
>
>

## <a name="tophttpsdocsloganalyticsioquerylanguagequerylanguagetopoperatorhtml-and-sorthttpsdocsloganalyticsioquerylanguagequerylanguagesortoperatorhtml"></a>[Horní](https://docs.loganalytics.io/queryLanguage/query_language_topoperator.html) a [řazení](https://docs.loganalytics.io/queryLanguage/query_language_sortoperator.html)
`take`je užitečné tooget rychlé ukázka výsledků, ale zobrazuje řádky z tabulky hello seřazeny. použít tooget seřazené zobrazení, `top` (pro ukázku) nebo `sort` (přes hello celé tabulky).

Zobrazte hello první n řádky, seřazené podle konkrétního sloupce:

```AIQL

    requests | top 10 by timestamp desc
```

* *Syntaxe:* většina operátory mít parametry – klíčové slovo jako `by`.
* `desc`= sestupné řazení `asc` = vzestupně.

![](./media/app-insights-analytics-tour/260.png)

`top...`je další způsob původce o tom, že `sort ... | take...`. Budeme mít zapsat:

```AIQL

    requests | sort by timestamp desc | take 10
```

výsledek Hello by hello stejné, ale bude spuštěná trochu pomaleji. (Můžete také napsat `order`, což je zástupce `sort`.)

Hello záhlaví sloupců v tabulce zobrazení hello může být také použít toosort hello výsledky na obrazovce hello. Ale samozřejmě platí, pokud jste použili `take` nebo `top` tooretrieve jenom část tabulky, budete jenom změnit pořadí hello záznamy jste načíst.

## <a name="wherehttpsdocsloganalyticsioquerylanguagequerylanguagewhereoperatorhtml-filtering-on-a-condition"></a>[Kde](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html): filtrování na podmínce

Podíváme se, jenom požadavků, které vrátil kód konkrétní výsledku:

```AIQL

    requests
    | where resultCode  == "404"
    | take 10
```

![](./media/app-insights-analytics-tour/250.png)

Hello `where` operátor má logický výraz. Zde jsou některé klíčové body o nich:

* `and`, `or`: Logické operátory
* `==`, `<>`, `!=` : a nejsou rovny.
* `=~`, `!~` : velká a malá písmena řetězec stejná a není rovno. Existují mnoha další operátory porovnání řetězce.

<!---Read all about [scalar expressions]().--->

### <a name="getting-hello-right-type"></a>Získávání hello správný typ.
Vyhledá neúspěšné požadavky:

```AIQL

    requests
    | where isnotempty(resultCode) and toint(resultCode) >= 400
```
<!---
`resultCode` has type string, so we must cast it app-insights-analytics-reference.md#casts for a numeric comparison.
--->

## <a name="time"></a>Čas

Vaše dotazy nejsou ve výchozím nastavení s omezeným přístupem toohello posledních 24 hodin. Můžete však změnit tento rozsah:

![](./media/app-insights-analytics-tour/change-time-range.png)

Přepsat hello časový rozsah tak, že zápis jakýkoli dotaz, který uvádí `timestamp` v klauzuli where. Například:

```AIQL

    // What were hello slowest requests over hello past 3 days?
    requests
    | where timestamp > ago(3d)  // Override hello time range
    | top 5 by duration
```

Hello čas rozsah funkce je ekvivalentní tooa 'where' klauzule vložit po každé zmínky jednoho hello zdrojové tabulky.

`ago(3d)`znamená tří dnů před. Jiné jednotky doby zahrnují hodin (`2h`, `2.5h`), minut (`25m`) a sekund (`10s`).

Další příklady:

```AIQL

    // Last calendar week:
    requests
    | where timestamp > startofweek(now()-7d)
        and timestamp < startofweek(now())
    | top 5 by duration

    // First hour of every day in past seven days:
    requests
    | where timestamp > ago(7d) and timestamp % 1d < 1h
    | top 5 by duration

    // Specific dates:
    requests
    | where timestamp > datetime(2016-11-19) and timestamp < datetime(2016-11-21)
    | top 5 by duration

```

[Dat a časů odkaz](https://docs.loganalytics.io/concepts/concepts_datatypes_datetime.html).


## <a name="projecthttpsdocsloganalyticsioquerylanguagequerylanguageprojectoperatorhtml-select-rename-and-compute-columns"></a>[Projekt](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html): vyberte, přejmenování a výpočetní sloupců
Použití [ `project` ](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) toopick se právě hello sloupce, které chcete:

```AIQL

    requests | top 10 by timestamp desc
             | project timestamp, name, resultCode
```

![](./media/app-insights-analytics-tour/240.png)

Také můžete přejmenovat sloupce a definovat nové:

```AIQL

    requests
    | top 10 by timestamp desc
    | project  
            name,
            response = resultCode,
            timestamp,
            ['time of day'] = floor(timestamp % 1d, 1s)
```

![výsledek](./media/app-insights-analytics-tour/270.png)

* Názvy sloupců může obsahovat mezery nebo symboly v případě jejich jsou v závorkách, jako to: `['...']` nebo`["..."]`
* `%`je obvyklé Operátor modulo hello.
* `1d`(která je číslice, pak měl ') je časový interval literálu znamená jeden den. Tady jsou některé další literály časový interval: `12h`, `30m`, `10s`, `0.01s`.
* `floor`(alias `bin`) zaokrouhlí dolů toohello nejbližší násobek hello základní hodnoty můžete zadat hodnotu. Proto `floor(aTime, 1s)` zaokrouhlí na dobu mimo provoz toohello nejbližší sekundu.

Výrazy může zahrnovat všechny hello běžných operátorů (`+`, `-`,...), a rozsah užitečné funkce.

## <a name="extend"></a>Rozšíření
Pokud chcete toohello sloupce tooadd existující, použijte [ `extend` ](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html):

```AIQL

    requests
    | top 10 by timestamp desc
    | extend timeOfDay = floor(timestamp % 1d, 1s)
```

Pomocí [ `extend` ](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html) je míň podrobné než [ `project` ](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) Pokud chcete, aby tookeep všechny hello existující sloupce.

### <a name="convert-toolocal-time"></a>Převést toolocal čas

Časová razítka jsou vždycky ve standardu UTC. Takže pokud jste na pobřeží hello Tichomoří USA a se jedná o zimní, se vám mohly líbit toto:

```AIQL

    requests
    | top 10 by timestamp desc
    | extend localTime = timestamp - 8h
```


## <a name="summarizehttpsdocsloganalyticsioquerylanguagequerylanguagesummarizeoperatorhtml-aggregate-groups-of-rows"></a>[Shrnout](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html): agregovat skupiny řádků
`Summarize`použije zadanou *agregační funkce* přes skupiny řádků.

Například čas hello vaše webová aplikace přijímá požadavky tooa toorespond údajně v poli hello `duration`. Podívejme se, průměrná odpovědi hello čas tooall požadavky:

![](./media/app-insights-analytics-tour/410.png)

Nebo výsledek hello jsme může oddělení do požadavků odlišné názvy:

![](./media/app-insights-analytics-tour/420.png)

`Summarize`shromažďuje hello datových bodů v datovém proudu hello do skupin, pro které hello `by` klauzule vyhodnotí stejně. Každá hodnota v hello `by` výrazu - každý je název operace v hello výše příklad - výsledkem řádek v tabulce výsledků hello.

Nebo jsme může seskupení výsledků podle denní dobu:

![](./media/app-insights-analytics-tour/430.png)

Všimněte si, jak používáme hello `bin` – funkce (neboli `floor`). Pokud jsme použili `by timestamp`, každý řádek vstupu by skončit ve své vlastní malé skupiny. Pro všechny průběžné skalárních například časy nebo čísla, máme toobreak hello průběžné rozsah do spravovat počet jednotlivých hodnot. `bin`-který je právě hello známé zaokrouhlení rozbalovací `floor` funkce – je nejjednodušší způsob, jak toodo hello který.

Můžeme použít hello stejné rozsahy tooreduce technika řetězců:

![](./media/app-insights-analytics-tour/440.png)

Všimněte si, že můžete použít `name=` tooset hello název sloupec výsledků, buď ve výrazech agregace hello nebo hello pomocí klauzule.

## <a name="counting-sampled-data"></a>Počítání vzorků dat
`sum(itemCount)`je hello doporučená agregace toocount události. V mnoha případech itemCount = 1, takže hello funkce jednoduše počty až hello počet řádků ve skupině hello. Ale když [vzorkování](app-insights-sampling.md) je v operaci pouze část původní události hello zachová jako datových bodů ve službě Application Insights, aby byl pro každý datový bod, uvidíte, `itemCount` události.

Například pokud vzorkování zahodí 75 % hello původní události, pak se itemCount == 4 v záznamech hello uchovávají – to znamená, pro každý záznam zachované nebyly čtyři původní záznamy.

Adaptivního vzorkování způsobí, že itemCount toobe vyšší během období, kdy aplikace je se nejčastěji používá.

Shrnutí itemCount proto poskytuje dobrý odhad hello původní počet událostí.

![](./media/app-insights-analytics-tour/510.png)

K dispozici je také `count()` agregace (a počet operaci) pro případy, kdy Opravdu chcete toocount hello počet řádků ve skupině.

Je rozsah [funkce agregace](https://docs.loganalytics.io/learn/tutorials/aggregations.html).

## <a name="charting-hello-results"></a>Vytváření grafů hello výsledky
```AIQL

    exceptions
       | summarize count=sum(itemCount)  
         by bin(timestamp, 1h)
```

Ve výchozím nastavení zobrazí výsledky jako tabulku:

![](./media/app-insights-analytics-tour/225.png)

Můžeme udělat lépe než hello tabulka zobrazení. Podívejme se na výsledky hello v zobrazení grafu hello s možností svislá čára hello:

![Klikněte na graf, vyberte svislý pruhový graf a přiřadit x a y osy](./media/app-insights-analytics-tour/230.png)

Všimněte si, že i když jsme nebyla hello výsledky seřadit podle času (jak je vidět v zobrazení tabulky hello), zobrazení grafu hello vždy zobrazuje data a času ve správném pořadí.


## <a name="timecharts"></a>Timecharts
Zobrazit, kolik události existuje se každou hodinu:

```AIQL

    requests
      | summarize event_count=sum(itemCount)
        by bin(timestamp, 1h)
```

Vyberte možnost zobrazení grafu hello:

![timechart](./media/app-insights-analytics-tour/080.png)

## <a name="multiple-series"></a>Více řad
Více výrazů v hello `summarize` klauzule vytvoří více sloupců.

Více výrazů v hello `by` klauzule vytvoří více řádků, jeden pro každou kombinaci hodnot.

```AIQL

    requests
    | summarize count_=sum(itemCount), avg(duration)
      by bin(timestamp, 1h), client_StateOrProvince, client_City
    | order by timestamp asc, client_StateOrProvince, client_City
```

![Tabulka požadavků podle hodin a umístění](./media/app-insights-analytics-tour/090.png)

### <a name="segment-a-chart-by-dimensions"></a>Rozdělit grafu pomocí dimenzí
Pokud graf tabulku, která má sloupec řetězce a je číselný sloupec hello řetězec lze použít toosplit hello číselná data do samostatné řady bodů. Pokud existuje více než jeden sloupec řetězec, můžete jako hello diskriminátoru které toouse sloupce.

![Segment graf analýzy](./media/app-insights-analytics-tour/100.png)

#### <a name="bounce-rate"></a>Odraz rychlost

Převést řetězec toouse boolean tooa jej jako diskriminátoru:

```AIQL

    // Bounce rate: sessions with only one page view
    requests
    | where notempty(session_Id)
    | where tostring(operation_SyntheticSource) == "" // real users
    | summarize pagesInSession=sum(itemCount), sessionEnd=max(timestamp)
               by session_Id
    | extend isbounce= pagesInSession == 1
    | summarize count()
               by tostring(isbounce), bin (sessionEnd, 1h)
    | render timechart
```

### <a name="display-multiple-metrics"></a>Zobrazení více metriky
Pokud jste grafu tabulku, která má více než jeden číselný sloupec v přidání toohello časové razítko, můžete zobrazit libovolnou kombinací.

![Segment graf analýzy](./media/app-insights-analytics-tour/110.png)

Je nutné vybrat **nemáte rozdělení** mohli vybrat více číselné sloupce. Nelze rozdělit podle sloupce řetězec v hello stejný čas jako zobrazení více než jeden číselný sloupec.

## <a name="daily-average-cycle"></a>Průměrný denní cyklu
Jak se liší využití přes hello průměrného dne?

Počet požadavků podle času hello modulo jeden den, binned do hodiny:

```AIQL

    requests
    | where timestamp > ago(30d)  // Override "Last 24h"
    | where tostring(operation_SyntheticSource) == "" // real users
    | extend hour = bin(timestamp % 1d , 1h)
          + datetime("2016-01-01") // Allow render on line chart
    | summarize event_count=sum(itemCount) by hour
```

![Spojnicový graf hodin v průměrného dne](./media/app-insights-analytics-tour/120.png)

> [!NOTE]
> Všimněte si, že se aktuálně musí tooconvert čas doby trvání toodatetimes v pořadí toodisplay na spojnicový graf.
>
>

## <a name="compare-multiple-daily-series"></a>Porovnání více denní řad
Jak využití liší časem hello dne v různých zemí?

```AIQL

     requests  
     | where timestamp > ago(30d)  // Override "Last 24h"
     | where tostring(operation_SyntheticSource) == "" // real users
     | extend hour= floor( timestamp % 1d , 1h)
           + datetime("2001-01-01")
     | summarize event_count=sum(itemCount)
       by hour, client_CountryOrRegion
     | render timechart
```

![Rozdělení podle client_CountryOrRegion](./media/app-insights-analytics-tour/130.png)

## <a name="plot-a-distribution"></a>Vykreslení distribuce
Kolik relací dochází z různých délek?

```AIQL

    requests
    | where timestamp > ago(30d) // override "Last 24h"
    | where isnotnull(session_Id) and isnotempty(session_Id)
    | summarize min(timestamp), max(timestamp)
      by session_Id
    | extend sessionDuration = max_timestamp - min_timestamp
    | where sessionDuration > 1s and sessionDuration < 3m
    | summarize count() by floor(sessionDuration, 3s)
    | project d = sessionDuration + datetime("2016-01-01"), count_
```

poslední řádek Hello je požadovaná tooconvert toodatetime. Aktuálně hello x osy grafu se zobrazuje jako skalární hodnota, pouze pokud je hodnota datetime.

Hello `where` klauzule vyloučí jednorázové relací (sessionDuration == 0) a nastaví hello délce osy x hello.

![](./media/app-insights-analytics-tour/290.png)

## <a name="percentileshttpsdocsloganalyticsioquerylanguagequerylanguagepercentilesaggfunctionhtml"></a>[Percentily](https://docs.loganalytics.io/queryLanguage/query_language_percentiles_aggfunction.html)
Jaké rozsahy doby trvání zahrnují různá procenta relace?

Použít hello výše dotazu, ale nahraďte hello poslední řádek:

```AIQL

    requests
    | where isnotnull(session_Id) and isnotempty(session_Id)
    | summarize min(timestamp), max(timestamp)
      by session_Id
    | extend sesh = max_timestamp - min_timestamp
    | where sesh > 1s
    | summarize count() by floor(sesh, 3s)
    | summarize percentiles(sesh, 5, 20, 50, 80, 95)
```

Také jsme odebrali hello horní meze v hello kde klauzuli, v pořadí tooget opravte údaje včetně všechny relace s více než jeden požadavek:

![výsledek](./media/app-insights-analytics-tour/180.png)

Odkud jsme uvidíte, že:

* 5 % relací být méně než tři minuty 34s;
* 50 % relací poslední nejvýše 36 minut;
* 5 % relací poslední více než 7 dní

tooget samostatné rozpis pro každou zemi, jsme právě mít toobring hello client_CountryOrRegion sloupec samostatně prostřednictvím obě shrnout operátory:

```AIQL

    requests
    | where isnotnull(session_Id) and isnotempty(session_Id)
    | summarize min(timestamp), max(timestamp)
      by session_Id, client_CountryOrRegion
    | extend sesh = max_timestamp - min_timestamp
    | where sesh > 1s
    | summarize count() by floor(sesh, 3s), client_CountryOrRegion
    | summarize percentiles(sesh, 5, 20, 50, 80, 95)
      by client_CountryOrRegion
```

![](./media/app-insights-analytics-tour/190.png)

## <a name="join"></a>Spojit
Máme přístup tooseveral tabulky, včetně požadavky a výjimkami.

toofind hello výjimky související tooa požadavek, který vrátil neplatnou odpověď, jsme se můžete zapojit do tabulky hello na `session_Id`:

```AIQL

    requests
    | where toint(resultCode) >= 500
    | join (exceptions) on operation_Id
    | take 30
```


Je dobrým zvykem toouse `project` tooselect jenom hello sloupce je třeba před provedením hello spojení.
V hello stejné klauzule jsme přejmenovat sloupec časového razítka hello.

## <a name="lethttpsdocsloganalyticsioquerylanguagequerylanguageletstatementhtml-assign-a-result-tooa-variable"></a>[Umožní](https://docs.loganalytics.io/queryLanguage/query_language_letstatement.html): přiřaďte proměnnou tooa výsledek

Použití `let` tooseparate si části hello hello předchozí výrazu. Hello výsledky jsou stejné jako:

```AIQL

    let bad_requests =
      requests
        | where  toint(resultCode) >= 500  ;
    bad_requests
    | join (exceptions) on session_Id
    | take 30
```

> [!Tip] 
> V klientovi hello analýzy, umístěte prázdné řádky mezi částí hello hello dotazu. Zkontrolujte že tooexecute všechny.
>

Použití `toscalar` tooconvert tooa hodnotu buňky jedné tabulky:

```AIQL
let topCities =  toscalar (
   requests
   | summarize count() by client_City 
   | top n by count_ 
   | summarize makeset(client_City));
requests
| where client_City in (topCities(3)) 
| summarize count() by client_City;
```


### <a name="functions"></a>Funkce

Použití *umožní* toodefine funkce:

```AIQL

    let usdate = (t:datetime)
    {
      strcat(getmonth(t), "/", dayofmonth(t),"/", getyear(t), " ",
      bin((t-1h)%12h+1h,1s), iff(t%24h<12h, "AM", "PM"))
    };
    requests  
    | extend PST = usdate(timestamp-8h)
```

## <a name="accessing-nested-objects"></a>Přístup k vnořených objektů
Vnořených objektů lze snadno přistupovat. Například v datovém proudu výjimky hello vidíte strukturovaných objekty takto:

![výsledek](./media/app-insights-analytics-tour/520.png)

Můžete ho vyrovnání výběrem hello vlastnosti, které vás zajímají:

```AIQL

    exceptions | take 10
    | extend method1 = tostring(details[0].parsedStack[1].method)
```

Všimněte si, že je potřeba toocast hello výsledek toohello příslušného typu.


## <a name="custom-properties-and-measurements"></a>Vlastní vlastnosti a měření
Pokud vaše aplikace připojí [vlastní dimenze (Vlastnosti) a vlastní měření](app-insights-api-custom-events-metrics.md#properties) tooevents, pak bude zobrazovat v hello `customDimensions` a `customMeasurements` objekty.

Například pokud vaše aplikace obsahuje:

```C#

    var dimensions = new Dictionary<string, string>
                     {{"p1", "v1"},{"p2", "v2"}};
    var measurements = new Dictionary<string, double>
                     {{"m1", 42.0}, {"m2", 43.2}};
    telemetryClient.TrackEvent("myEvent", dimensions, measurements);
```

tooextract v Analytics tyto hodnoty:

```AIQL

    customEvents
    | extend p1 = customDimensions.p1,
      m1 = todouble(customMeasurements.m1) // cast tooexpected type

```

tooverify jestli vlastní dimenze je určitého typu:

```AIQL

    customEvents
    | extend p1 = customDimensions.p1,
      iff(notnull(todouble(customMeasurements.m1)), ...
```

## <a name="dashboards"></a>Řídicí panely
Řídicí panel tooa výsledky v pořadí toobring budete moct připnout společně všechna vaše nejdůležitější grafů a tabulek.

* [Řídicí panel Azure sdílené](app-insights-dashboards.md#share-dashboards): klikněte na ikonu Připnutí hello. Než to uděláte, musíte mít sdílené řídicího panelu. Otevřete hello portálu Azure, nebo vytvořit řídicí panel a klikněte na sdílenou složku.
* [Řídicí panel Power BI](app-insights-export-power-bi.md): klikněte na exportovat, Power BI dotazu. Výhodou této alternativní je, že můžete zobrazit dotaz spolu s další výsledky z široké škály zdrojů.

## <a name="combine-with-imported-data"></a>Kombinovat s importovanými daty

Podívejte se na řídicím panelu hello skvělé sestavy analýzy, ale občas můžete chtít tootranslate hello data tooa další stravitelné formuláře. Předpokládejme například, že ověření uživatelé se budou identifikovat hello telemetrie podle alias. Chcete tooshow jejich reálné názvy ve výsledcích. toodo, budete potřebovat soubor CSV, který mapuje z hello aliasy toohello skutečné názvy.

Můžete importovat soubor dat a použít ho stejně jako všechny standardní tabulek hello (požadavky, výjimky a tak dále). Buď dotaz na svůj vlastní nebo se připojte s jinou tabulkou. Například, pokud máte tabulku s názvem usermap a má sloupce `realName` a `userId`, můžete ji pomocí tootranslate hello `user_AuthenticatedId` pole telemetrická žádost hello:

```AIQL

    requests
    | where notempty(user_AuthenticatedId)
    | project userId = user_AuthenticatedId
      // get hello realName field from hello usermap table:
    | join kind=leftouter ( usermap ) on userId
      // count transactions by name:
    | summarize count() by realName
```

tooimport tabulky, v okně schématu hello pod **jiných zdrojů dat**, postupujte podle pokynů tooadd hello nový zdroj dat, tím, že nahrajete ukázková data. Pak můžete použít tuto definici tooupload tabulky.

Funkce importu Hello je aktuálně ve verzi preview, zobrazí se původně odkaz "Kontaktujte nás" v části "Jiné zdroje dat." Pomocí této toosign do programu preview toohello a hello odkaz se nahradí klikněte tlačítko "Přidat nový zdroj dat".


## <a name="tables"></a>Tabulky
Hello datový proud telemetrie přijaté z vaší aplikace je přístupná prostřednictvím několik tabulek. Hello schéma vlastnosti, které jsou k dispozici pro každou tabulku je viditelná vlevo hello okna hello.

### <a name="requests-table"></a>Tabulka požadavků
Počet HTTP žádostí tooyour webové aplikace a segment podle názvu stránky:

![Počet požadavků segmentované podle názvu](./media/app-insights-analytics-tour/analytics-count-requests.png)

Vyhledá hello požadavky, které nesplní většinu:

![Počet požadavků segmentované podle názvu](./media/app-insights-analytics-tour/analytics-failed-requests.png)

### <a name="custom-events-table"></a>Tabulka vlastní události
Pokud používáte [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent) toosend vlastní události si můžete přečíst je z této tabulky.

Podívejme příklad kde kódu aplikace obsahuje tyto řádky:

```C#

    telemetry.TrackEvent("Query",
       new Dictionary<string,string> {{"query", sqlCmd}},
       new Dictionary<string,double> {
           {"retry", retryCount},
           {"querytime", totalTime}})
```

Zobrazí hello frekvenci tyto události:

![Zobrazení počet vlastní události](./media/app-insights-analytics-tour/analytics-custom-events-rate.png)

Extrahujte měření a dimenze z hello události:

![Zobrazení počet vlastní události](./media/app-insights-analytics-tour/analytics-custom-events-dimensions.png)

### <a name="custom-metrics-table"></a>Vlastní metriky tabulky
Pokud používáte [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) toosend vlastní hodnoty metriky zjistíte své výsledky v hello **customMetrics** datového proudu. Například:  

![Vlastní metriky v analytics Application Insights](./media/app-insights-analytics-tour/analytics-custom-metrics.png)

> [!NOTE]
> V [Průzkumníku metrik](app-insights-metrics-explorer.md), všechny vlastní měření připojené tooany typ telemetrie objeví spolu v okně metriky hello společně s metriky odeslaných pomocí `TrackMetric()`. Ale v analýzy, vlastní měření jsou stále připojen typ toowhichever byly provedeny na - události nebo požadavky a tak dále - při poslal TrackMetric metrika se zobrazí v vlastní datový proud telemetrie.
>
>

### <a name="performance-counters-table"></a>Tabulka čítače výkonu
[Čítače výkonu](app-insights-performance-counters.md) můžete zobrazit základní systémové metriky pro vaši aplikaci, například CPU, paměť a využití sítě. Můžete nakonfigurovat hello SDK toosend další čítače, včetně vlastní vlastní čítače.

Hello **čítače výkonu** schématu zpřístupní hello `category`, `counter` název, a `instance` název jednotlivých čítačů výkonu. Názvy instancí čítače jsou pouze použít toosome čítače výkonu a obvykle označují, že má název hello počtu hello toowhich proces hello vztah. V hello telemetrických dat pro každou aplikaci zobrazí se pouze hello čítače pro tuto aplikaci. Například toosee čítače, které jsou k dispozici:

![Čítače výkonu v analytics Application Insights](./media/app-insights-analytics-tour/analytics-performance-counters.png)

tooget graf dostupné paměti přes hello vybrané období:

![Paměť timechart v analytics Application Insights](./media/app-insights-analytics-tour/analytics-available-memory.png)

Jako další telemetrií **čítače výkonu** také má sloupec `cloud_RoleInstance` určující hello identitu hello hostitelský počítač, na kterém aplikace běží. Například toocompare hello výkonu vaší aplikace na různé počítače hello:

![Výkon oddělených instance role ve službě Application Insights analytics](./media/app-insights-analytics-tour/analytics-metrics-role-instance.png)

### <a name="exceptions-table"></a>Výjimky tabulky
[Výjimky hlášených aplikace](app-insights-asp-net-exceptions.md) jsou k dispozici v této tabulce.

toofind hello požadavek HTTP, který aplikace byla zpracování při hello došlo k výjimce, připojte na operation_Id:

![Připojení výjimky s požadavky na operation_Id](./media/app-insights-analytics-tour/analytics-exception-request.png)

### <a name="browser-timings-table"></a>Tabulka časování prohlížeče
`browserTimings`Zobrazuje data zatížení stránky shromažďují v prohlížečích vašich uživatelů.

[Nastavit aplikaci pro telemetrických dat na straně klienta](app-insights-javascript.md) v pořadí toosee tyto metriky.

zahrnuje Hello schématu [metrika označující hello délek různých fázích načítání proces stránky hello](app-insights-javascript.md#page-load-performance). (Není indikují hello časový úsek, který uživatelé přečíst na stránce.)  

Zobrazit hello popularities různých stránek a načíst časy pro jednotlivé stránky:

![Časů načtení stránky v Analytics](./media/app-insights-analytics-tour/analytics-page-load.png)

### <a name="availability-results-table"></a>Tabulky výsledků dostupnosti
`availabilityResults`ukazuje hello výsledky vaše [webové testy](app-insights-monitor-web-app-availability.md). Každé spuštění testů z umístění každého testu se hlásí samostatně.

![Časů načtení stránky v Analytics](./media/app-insights-analytics-tour/analytics-availability.png)

### <a name="dependencies-table"></a>Tabulka závislosti
Obsahuje výsledky volání, aby vaše aplikace umožňuje toodatabases a rozhraní REST API a dalších volá tooTrackDependency(). Zahrnuje taky volání AJAX provedená z prohlížeče hello.

Volání AJAX z prohlížeče hello:

```AIQL

    dependencies | where client_Type == "Browser"
    | take 10
```

Závislost volání ze serveru hello:

```AIQL

    dependencies | where client_Type == "PC"
    | take 10
```

Vždy zobrazovat výsledky v závislosti na straně serveru `success==False` Pokud hello Application Insights Agent není nainstalován. Ale hello další data jsou správné.

### <a name="traces-table"></a>Tabulka trasování
Obsahuje hello telemetrické zprávy odesílané vaší aplikace pomocí TrackTrace(), nebo [jiných rozhraní protokolování](app-insights-asp-net-trace-logs.md).

## <a name="video"></a>Video 

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

Pokročilé dotazy:

> [!VIDEO https://channel9.msdn.com/Events/Build/2016/P591/player]


## <a name="next-steps"></a>Další kroky
* [Referenční dokumentace jazyka Analytics](app-insights-analytics-reference.md)
* [SQL-uživatelů tahák](https://aka.ms/sql-analytics) překládá nejběžnější idioms hello.

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]
