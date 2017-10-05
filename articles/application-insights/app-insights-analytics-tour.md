---
title: "Prohlídka prostřednictvím analýzy ve službě Azure Application Insights | Microsoft Docs"
description: "Krátký ukázky všechny hlavní dotazů v analýzy, nástroj výkonné vyhledávání služby Application Insights."
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
ms.openlocfilehash: f5650d212eb2f8c460f062b3c11ae14c1e026ba6
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="a-tour-of-analytics-in-application-insights"></a><span data-ttu-id="0d180-103">Prohlídka Analytics ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="0d180-103">A tour of Analytics in Application Insights</span></span>
<span data-ttu-id="0d180-104">[Analýza](app-insights-analytics.md) je výkonný vyhledávání funkcí [Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0d180-104">[Analytics](app-insights-analytics.md) is the powerful search feature of [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="0d180-105">Tyto stránek popisují dotazovací jazyk analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="0d180-105">These pages describe the Log Analytics query language.</span></span>

* <span data-ttu-id="0d180-106">**[Podívejte se na úvodní video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span><span class="sxs-lookup"><span data-stu-id="0d180-106">**[Watch the introductory video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span></span>
* <span data-ttu-id="0d180-107">**[Vyzkoušejte Analytics na naše simulované data](https://analytics.applicationinsights.io/demo)**  Pokud aplikace není odesílání dat do služby Application Insights ještě.</span><span class="sxs-lookup"><span data-stu-id="0d180-107">**[Test drive Analytics on our simulated data](https://analytics.applicationinsights.io/demo)** if your app isn't sending data to Application Insights yet.</span></span>
* <span data-ttu-id="0d180-108">**[SQL-uživatelů tahák](https://aka.ms/sql-analytics)**  překládá nejběžnější idioms.</span><span class="sxs-lookup"><span data-stu-id="0d180-108">**[SQL-users' cheat sheet](https://aka.ms/sql-analytics)** translates the most common idioms.</span></span>

<span data-ttu-id="0d180-109">Podívejme procházení prostřednictvím některé základní dotazy, které vám pomůžou začít.</span><span class="sxs-lookup"><span data-stu-id="0d180-109">Let's take a walk through some basic queries to get you started.</span></span>

## <a name="connect-to-your-application-insights-data"></a><span data-ttu-id="0d180-110">Připojit se k datům Application Insights</span><span class="sxs-lookup"><span data-stu-id="0d180-110">Connect to your Application Insights data</span></span>
<span data-ttu-id="0d180-111">Otevřete Analytics z vaší aplikace [okno Přehled](app-insights-dashboards.md) ve službě Application Insights:</span><span class="sxs-lookup"><span data-stu-id="0d180-111">Open Analytics from your app's [overview blade](app-insights-dashboards.md) in Application Insights:</span></span>

![Otevřete portal.azure.com otevřete prostředek Application Insights a klikněte na Analytics.](./media/app-insights-analytics-tour/001.png)

## <a name="takehttpsdocsloganalyticsioquerylanguagequerylanguagetakeoperatorhtml-show-me-n-rows"></a><span data-ttu-id="0d180-113">[Trvat](https://docs.loganalytics.io/queryLanguage/query_language_takeoperator.html): Zobrazit mi n řádků</span><span class="sxs-lookup"><span data-stu-id="0d180-113">[Take](https://docs.loganalytics.io/queryLanguage/query_language_takeoperator.html): show me n rows</span></span>
<span data-ttu-id="0d180-114">Datové body, které protokolu operace uživatele (obvykle HTTP přijatých požadavků ve vaší webové aplikace) jsou uložené v tabulce s názvem `requests`.</span><span class="sxs-lookup"><span data-stu-id="0d180-114">Data points that log user operations (typically HTTP requests received by your web app) are stored in a table called `requests`.</span></span> <span data-ttu-id="0d180-115">Každý řádek je telemetrie datový bod, který přijal od Application Insights SDK ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0d180-115">Each row is a telemetry data point received from the Application Insights SDK in your app.</span></span>

<span data-ttu-id="0d180-116">Začněme prověřením několik ukázkových řádky v tabulce:</span><span class="sxs-lookup"><span data-stu-id="0d180-116">Let's start by examining a few sample rows of the table:</span></span>

![výsledky](./media/app-insights-analytics-tour/010.png)

> [!NOTE]
> <span data-ttu-id="0d180-118">Umístěte kurzor někde v příkazu před kliknutím na Přejít.</span><span class="sxs-lookup"><span data-stu-id="0d180-118">Put the cursor somewhere in the statement before you click Go.</span></span> <span data-ttu-id="0d180-119">Příkaz můžete rozdělit přes více než jeden řádek, ale nemáte Vložit prázdné řádky v příkazu.</span><span class="sxs-lookup"><span data-stu-id="0d180-119">You can split a statement over more than one line, but don't put blank lines in a statement.</span></span> <span data-ttu-id="0d180-120">Prázdné řádky jsou vhodné pro zachování několik samostatné dotazy v okně.</span><span class="sxs-lookup"><span data-stu-id="0d180-120">Blank lines are a convenient way to keep several separate queries in the window.</span></span>
>
>

<span data-ttu-id="0d180-121">Vyberte sloupce, přetáhněte je seskupit podle sloupce a filtrování:</span><span class="sxs-lookup"><span data-stu-id="0d180-121">Choose columns, drag them, group by columns, and filter:</span></span>

![Klikněte na výběr sloupců v pravé horní části výsledků](./media/app-insights-analytics-tour/030.png)

<span data-ttu-id="0d180-123">Rozbalením libovolné položky zobrazíte podrobností:</span><span class="sxs-lookup"><span data-stu-id="0d180-123">Expand any item to see the detail:</span></span>

![Vyberte tabulky a použití konfigurace sloupců](./media/app-insights-analytics-tour/040.png)

> [!NOTE]
> <span data-ttu-id="0d180-125">Klikněte na hlavičku sloupce Chcete-li změnit pořadí výsledky, které jsou dostupné ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="0d180-125">Click the head of a column to re-order the results available in the web browser.</span></span> <span data-ttu-id="0d180-126">Ale uvědomte si, že pro sadu výsledků velký počet řádků, které jsou staženy do prohlížeče je omezená.</span><span class="sxs-lookup"><span data-stu-id="0d180-126">But be aware that for a large result set, the number of rows downloaded to the browser is limited.</span></span> <span data-ttu-id="0d180-127">Řazení tímto způsobem není vždy zobrazí můžete skutečné nejvyšší nebo nejnižší položky.</span><span class="sxs-lookup"><span data-stu-id="0d180-127">Sorting this way doesn't always show you the actual highest or lowest items.</span></span> <span data-ttu-id="0d180-128">Chcete-li řadit položky spolehlivě, použijte `top` nebo `sort` operátor.</span><span class="sxs-lookup"><span data-stu-id="0d180-128">To sort items reliably, use the `top` or `sort` operator.</span></span>
>
>

## <a name="tophttpsdocsloganalyticsioquerylanguagequerylanguagetopoperatorhtml-and-sorthttpsdocsloganalyticsioquerylanguagequerylanguagesortoperatorhtml"></a><span data-ttu-id="0d180-129">[Horní](https://docs.loganalytics.io/queryLanguage/query_language_topoperator.html) a [řazení](https://docs.loganalytics.io/queryLanguage/query_language_sortoperator.html)</span><span class="sxs-lookup"><span data-stu-id="0d180-129">[Top](https://docs.loganalytics.io/queryLanguage/query_language_topoperator.html) and [sort](https://docs.loganalytics.io/queryLanguage/query_language_sortoperator.html)</span></span>
<span data-ttu-id="0d180-130">`take`je užitečné k získání ukázku rychlé výsledků, ale zobrazuje řádky z tabulky seřazeny.</span><span class="sxs-lookup"><span data-stu-id="0d180-130">`take` is useful to get a quick sample of a result, but it shows rows from the table in no particular order.</span></span> <span data-ttu-id="0d180-131">Chcete-li získat seřazené zobrazení, použijte `top` (pro ukázku) nebo `sort` (přes celé tabulky).</span><span class="sxs-lookup"><span data-stu-id="0d180-131">To get an ordered view, use `top` (for a sample) or `sort` (over the whole table).</span></span>

<span data-ttu-id="0d180-132">Zobrazte první n řádky, seřazené podle konkrétního sloupce:</span><span class="sxs-lookup"><span data-stu-id="0d180-132">Show me the first n rows, ordered by a particular column:</span></span>

```AIQL

    requests | top 10 by timestamp desc
```

* <span data-ttu-id="0d180-133">*Syntaxe:* většina operátory mít parametry – klíčové slovo jako `by`.</span><span class="sxs-lookup"><span data-stu-id="0d180-133">*Syntax:* Most operators have keyword parameters such as `by`.</span></span>
* <span data-ttu-id="0d180-134">`desc`= sestupné řazení `asc` = vzestupně.</span><span class="sxs-lookup"><span data-stu-id="0d180-134">`desc` = descending order, `asc` = ascending.</span></span>

![](./media/app-insights-analytics-tour/260.png)

<span data-ttu-id="0d180-135">`top...`je další způsob původce o tom, že `sort ... | take...`.</span><span class="sxs-lookup"><span data-stu-id="0d180-135">`top...` is a more performant way of saying `sort ... | take...`.</span></span> <span data-ttu-id="0d180-136">Budeme mít zapsat:</span><span class="sxs-lookup"><span data-stu-id="0d180-136">We could have written:</span></span>

```AIQL

    requests | sort by timestamp desc | take 10
```

<span data-ttu-id="0d180-137">Výsledkem bude stejná, ale bude spuštěná trochu pomaleji.</span><span class="sxs-lookup"><span data-stu-id="0d180-137">The result would be the same, but it would run a bit more slowly.</span></span> <span data-ttu-id="0d180-138">(Můžete také napsat `order`, což je zástupce `sort`.)</span><span class="sxs-lookup"><span data-stu-id="0d180-138">(You could also write `order`, which is an alias of `sort`.)</span></span>

<span data-ttu-id="0d180-139">Záhlaví sloupců v tabulce zobrazení lze také seřadit výsledky na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="0d180-139">The column headers in the table view can also be used to sort the results on the screen.</span></span> <span data-ttu-id="0d180-140">Ale samozřejmě platí, pokud jste použili `take` nebo `top` načíst jenom část tabulky, můžete budete pouze změnit pořadí záznamy jste načíst.</span><span class="sxs-lookup"><span data-stu-id="0d180-140">But of course, if you've used `take` or `top` to retrieve just part of a table, you'll only re-order the records you've retrieved.</span></span>

## <a name="wherehttpsdocsloganalyticsioquerylanguagequerylanguagewhereoperatorhtml-filtering-on-a-condition"></a><span data-ttu-id="0d180-141">[Kde](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html): filtrování na podmínce</span><span class="sxs-lookup"><span data-stu-id="0d180-141">[Where](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html): filtering on a condition</span></span>

<span data-ttu-id="0d180-142">Podíváme se, jenom požadavků, které vrátil kód konkrétní výsledku:</span><span class="sxs-lookup"><span data-stu-id="0d180-142">Let's see just requests that returned a particular result code:</span></span>

```AIQL

    requests
    | where resultCode  == "404"
    | take 10
```

![](./media/app-insights-analytics-tour/250.png)

<span data-ttu-id="0d180-143">`where` Operátor má logický výraz.</span><span class="sxs-lookup"><span data-stu-id="0d180-143">The `where` operator takes a Boolean expression.</span></span> <span data-ttu-id="0d180-144">Zde jsou některé klíčové body o nich:</span><span class="sxs-lookup"><span data-stu-id="0d180-144">Here are some key points about them:</span></span>

* <span data-ttu-id="0d180-145">`and`, `or`: Logické operátory</span><span class="sxs-lookup"><span data-stu-id="0d180-145">`and`, `or`: Boolean operators</span></span>
* <span data-ttu-id="0d180-146">`==`, `<>`, `!=` : a nejsou rovny.</span><span class="sxs-lookup"><span data-stu-id="0d180-146">`==`, `<>`, `!=` : equal and not equal</span></span>
* <span data-ttu-id="0d180-147">`=~`, `!~` : velká a malá písmena řetězec stejná a není rovno.</span><span class="sxs-lookup"><span data-stu-id="0d180-147">`=~`, `!~` : case-insensitive string equal and not equal.</span></span> <span data-ttu-id="0d180-148">Existují mnoha další operátory porovnání řetězce.</span><span class="sxs-lookup"><span data-stu-id="0d180-148">There are lots more string comparison operators.</span></span>

<!---Read all about [scalar expressions]().--->

### <a name="getting-the-right-type"></a><span data-ttu-id="0d180-149">Získávání správný typ.</span><span class="sxs-lookup"><span data-stu-id="0d180-149">Getting the right type</span></span>
<span data-ttu-id="0d180-150">Vyhledá neúspěšné požadavky:</span><span class="sxs-lookup"><span data-stu-id="0d180-150">Find unsuccessful requests:</span></span>

```AIQL

    requests
    | where isnotempty(resultCode) and toint(resultCode) >= 400
```
<!---
`resultCode` has type string, so we must cast it app-insights-analytics-reference.md#casts for a numeric comparison.
--->

## <a name="time"></a><span data-ttu-id="0d180-151">Čas</span><span class="sxs-lookup"><span data-stu-id="0d180-151">Time</span></span>

<span data-ttu-id="0d180-152">Ve výchozím vaše dotazy jsou omezeny na poslední 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="0d180-152">By default, your queries are restricted to the last 24 hours.</span></span> <span data-ttu-id="0d180-153">Můžete však změnit tento rozsah:</span><span class="sxs-lookup"><span data-stu-id="0d180-153">But you can change this range:</span></span>

![](./media/app-insights-analytics-tour/change-time-range.png)

<span data-ttu-id="0d180-154">Časové rozmezí potlačit zápis jakýkoli dotaz, který uvádí `timestamp` v klauzuli where.</span><span class="sxs-lookup"><span data-stu-id="0d180-154">Override the time range by writing any query that mentions `timestamp` in a where-clause.</span></span> <span data-ttu-id="0d180-155">Například:</span><span class="sxs-lookup"><span data-stu-id="0d180-155">For example:</span></span>

```AIQL

    // What were the slowest requests over the past 3 days?
    requests
    | where timestamp > ago(3d)  // Override the time range
    | top 5 by duration
```

<span data-ttu-id="0d180-156">Funkci rozsah čas je ekvivalentní volání za každou zmínky jednoho ze zdrojové tabulky vložit klauzuli 'where'.</span><span class="sxs-lookup"><span data-stu-id="0d180-156">The time range feature is equivalent to a 'where' clause inserted after each mention of one of the source tables.</span></span>

<span data-ttu-id="0d180-157">`ago(3d)`znamená tří dnů před.</span><span class="sxs-lookup"><span data-stu-id="0d180-157">`ago(3d)` means 'three days ago'.</span></span> <span data-ttu-id="0d180-158">Jiné jednotky doby zahrnují hodin (`2h`, `2.5h`), minut (`25m`) a sekund (`10s`).</span><span class="sxs-lookup"><span data-stu-id="0d180-158">Other units of time include hours (`2h`, `2.5h`), minutes (`25m`), and seconds (`10s`).</span></span>

<span data-ttu-id="0d180-159">Další příklady:</span><span class="sxs-lookup"><span data-stu-id="0d180-159">Other examples:</span></span>

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

<span data-ttu-id="0d180-160">[Dat a časů odkaz](https://docs.loganalytics.io/concepts/concepts_datatypes_datetime.html).</span><span class="sxs-lookup"><span data-stu-id="0d180-160">[Dates and times reference](https://docs.loganalytics.io/concepts/concepts_datatypes_datetime.html).</span></span>


## <a name="projecthttpsdocsloganalyticsioquerylanguagequerylanguageprojectoperatorhtml-select-rename-and-compute-columns"></a><span data-ttu-id="0d180-161">[Projekt](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html): vyberte, přejmenování a výpočetní sloupců</span><span class="sxs-lookup"><span data-stu-id="0d180-161">[Project](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html): select, rename, and compute columns</span></span>
<span data-ttu-id="0d180-162">Použití [ `project` ](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) k výběru pouze sloupce, které chcete:</span><span class="sxs-lookup"><span data-stu-id="0d180-162">Use [`project`](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) to pick out just the columns you want:</span></span>

```AIQL

    requests | top 10 by timestamp desc
             | project timestamp, name, resultCode
```

![](./media/app-insights-analytics-tour/240.png)

<span data-ttu-id="0d180-163">Také můžete přejmenovat sloupce a definovat nové:</span><span class="sxs-lookup"><span data-stu-id="0d180-163">You can also rename columns and define new ones:</span></span>

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

* <span data-ttu-id="0d180-165">Názvy sloupců může obsahovat mezery nebo symboly v případě jejich jsou v závorkách, jako to: `['...']` nebo`["..."]`</span><span class="sxs-lookup"><span data-stu-id="0d180-165">Column names can include spaces or symbols if they are bracketed like this: `['...']` or `["..."]`</span></span>
* <span data-ttu-id="0d180-166">`%`je obvyklé Operátor modulo.</span><span class="sxs-lookup"><span data-stu-id="0d180-166">`%` is the usual modulo operator.</span></span>
* <span data-ttu-id="0d180-167">`1d`(která je číslice, pak měl ') je časový interval literálu znamená jeden den.</span><span class="sxs-lookup"><span data-stu-id="0d180-167">`1d` (that's a digit one, then a 'd') is a timespan literal meaning one day.</span></span> <span data-ttu-id="0d180-168">Tady jsou některé další literály časový interval: `12h`, `30m`, `10s`, `0.01s`.</span><span class="sxs-lookup"><span data-stu-id="0d180-168">Here are some more timespan literals: `12h`, `30m`, `10s`, `0.01s`.</span></span>
* <span data-ttu-id="0d180-169">`floor`(alias `bin`) zaokrouhlí dolů na nejbližší násobek hodnotu zadáte hodnotu.</span><span class="sxs-lookup"><span data-stu-id="0d180-169">`floor` (alias `bin`) rounds a value down to the nearest multiple of the base value you provide.</span></span> <span data-ttu-id="0d180-170">Proto `floor(aTime, 1s)` zaokrouhlí dolů nejbližší sekundu čas.</span><span class="sxs-lookup"><span data-stu-id="0d180-170">So `floor(aTime, 1s)` rounds a time down to the nearest second.</span></span>

<span data-ttu-id="0d180-171">Výrazy může zahrnovat všechny běžných operátorů (`+`, `-`,...), a rozsah užitečné funkce.</span><span class="sxs-lookup"><span data-stu-id="0d180-171">Expressions can include all the usual operators (`+`, `-`, ...), and there's a range of useful functions.</span></span>

## <a name="extend"></a><span data-ttu-id="0d180-172">Rozšíření</span><span class="sxs-lookup"><span data-stu-id="0d180-172">Extend</span></span>
<span data-ttu-id="0d180-173">Pokud chcete přidat sloupce do již existující, použijte [ `extend` ](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html):</span><span class="sxs-lookup"><span data-stu-id="0d180-173">If you just want to add columns to the existing ones, use [`extend`](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html):</span></span>

```AIQL

    requests
    | top 10 by timestamp desc
    | extend timeOfDay = floor(timestamp % 1d, 1s)
```

<span data-ttu-id="0d180-174">Pomocí [ `extend` ](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html) je míň podrobné než [ `project` ](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) Pokud chcete zachovat existující sloupce.</span><span class="sxs-lookup"><span data-stu-id="0d180-174">Using [`extend`](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html) is less verbose than [`project`](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) if you want to keep all the existing columns.</span></span>

### <a name="convert-to-local-time"></a><span data-ttu-id="0d180-175">Převést na místní čas</span><span class="sxs-lookup"><span data-stu-id="0d180-175">Convert to local time</span></span>

<span data-ttu-id="0d180-176">Časová razítka jsou vždycky ve standardu UTC.</span><span class="sxs-lookup"><span data-stu-id="0d180-176">Timestamps are always in UTC.</span></span> <span data-ttu-id="0d180-177">Pokud jste na pobřeží Tichomoří USA a se jedná o zimní, může to jako:</span><span class="sxs-lookup"><span data-stu-id="0d180-177">So if you're on the US Pacific coast and it's winter, you might like this:</span></span>

```AIQL

    requests
    | top 10 by timestamp desc
    | extend localTime = timestamp - 8h
```


## <a name="summarizehttpsdocsloganalyticsioquerylanguagequerylanguagesummarizeoperatorhtml-aggregate-groups-of-rows"></a><span data-ttu-id="0d180-178">[Shrnout](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html): agregovat skupiny řádků</span><span class="sxs-lookup"><span data-stu-id="0d180-178">[Summarize](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html): aggregate groups of rows</span></span>
<span data-ttu-id="0d180-179">`Summarize`použije zadanou *agregační funkce* přes skupiny řádků.</span><span class="sxs-lookup"><span data-stu-id="0d180-179">`Summarize` applies a specified *aggregation function* over groups of rows.</span></span>

<span data-ttu-id="0d180-180">Například čas trvá odpovědět na požadavek webové aplikace je uvedená v poli `duration`.</span><span class="sxs-lookup"><span data-stu-id="0d180-180">For example, the time your web app takes to respond to a request is reported in the field `duration`.</span></span> <span data-ttu-id="0d180-181">Podívejme se, Průměrná doba odezvy pro všechny požadavky:</span><span class="sxs-lookup"><span data-stu-id="0d180-181">Let's see the average response time to all requests:</span></span>

![](./media/app-insights-analytics-tour/410.png)

<span data-ttu-id="0d180-182">Nebo výsledek jsme může oddělení do požadavků odlišné názvy:</span><span class="sxs-lookup"><span data-stu-id="0d180-182">Or we could separate the result into requests of different names:</span></span>

![](./media/app-insights-analytics-tour/420.png)

<span data-ttu-id="0d180-183">`Summarize`shromažďuje datových bodů v datovém proudu do skupin, pro kterou `by` klauzule vyhodnotí stejně.</span><span class="sxs-lookup"><span data-stu-id="0d180-183">`Summarize` collects the data points in the stream into groups for which the `by` clause evaluates equally.</span></span> <span data-ttu-id="0d180-184">Každá hodnota v `by` výrazu - každý je název operace v předchozím příkladu - výsledkem řádek v tabulce výsledků.</span><span class="sxs-lookup"><span data-stu-id="0d180-184">Each value in the `by` expression - each operation name in the above example - results in a row in the result table.</span></span>

<span data-ttu-id="0d180-185">Nebo jsme může seskupení výsledků podle denní dobu:</span><span class="sxs-lookup"><span data-stu-id="0d180-185">Or we could group results by time of day:</span></span>

![](./media/app-insights-analytics-tour/430.png)

<span data-ttu-id="0d180-186">Všimněte si, jak používáme `bin` – funkce (neboli `floor`).</span><span class="sxs-lookup"><span data-stu-id="0d180-186">Notice how we're using the `bin` function (aka `floor`).</span></span> <span data-ttu-id="0d180-187">Pokud jsme použili `by timestamp`, každý řádek vstupu by skončit ve své vlastní malé skupiny.</span><span class="sxs-lookup"><span data-stu-id="0d180-187">If we just used `by timestamp`, every input row would end up in its own little group.</span></span> <span data-ttu-id="0d180-188">Pro všechny průběžné skalárních jako časy nebo čísla, musíme průběžné rozsahu rozdělit spravovat počet jednotlivých hodnot.</span><span class="sxs-lookup"><span data-stu-id="0d180-188">For any continuous scalar like times or numbers, we have to break the continuous range into a manageable number of discrete values.</span></span> <span data-ttu-id="0d180-189">`bin`-který je právě známé zaokrouhlení nižší `floor` funkce – je nejjednodušší způsob, jak to udělat.</span><span class="sxs-lookup"><span data-stu-id="0d180-189">`bin` - which is just the familiar rounding-down `floor` function - is the easiest way to do that.</span></span>

<span data-ttu-id="0d180-190">Můžeme použít stejný postup ke snížení rozsahy řetězců:</span><span class="sxs-lookup"><span data-stu-id="0d180-190">We can use the same technique to reduce ranges of strings:</span></span>

![](./media/app-insights-analytics-tour/440.png)

<span data-ttu-id="0d180-191">Všimněte si, že můžete použít `name=` nastavit název sloupec výsledků, buď ve výrazech agregace nebo klauzuli by.</span><span class="sxs-lookup"><span data-stu-id="0d180-191">Notice that you can use `name=` to set the name of a result column, either in the aggregation expressions or the by-clause.</span></span>

## <a name="counting-sampled-data"></a><span data-ttu-id="0d180-192">Počítání vzorků dat</span><span class="sxs-lookup"><span data-stu-id="0d180-192">Counting sampled data</span></span>
<span data-ttu-id="0d180-193">`sum(itemCount)`je doporučené agregace počítat události.</span><span class="sxs-lookup"><span data-stu-id="0d180-193">`sum(itemCount)` is the recommended aggregation to count events.</span></span> <span data-ttu-id="0d180-194">V mnoha případech itemCount = 1, takže funkce jednoduše spočítá počet řádků ve skupině.</span><span class="sxs-lookup"><span data-stu-id="0d180-194">In many cases, itemCount==1, so the function simply counts up the number of rows in the group.</span></span> <span data-ttu-id="0d180-195">Ale když [vzorkování](app-insights-sampling.md) je v provozu se pouze část původní události zachová jako datových bodů ve službě Application Insights, aby byl pro každý datový bod, uvidíte, `itemCount` události.</span><span class="sxs-lookup"><span data-stu-id="0d180-195">But when [sampling](app-insights-sampling.md) is in operation, only a fraction of the original events are retained as data points in Application Insights, so that for each data point you see, there are `itemCount` events.</span></span>

<span data-ttu-id="0d180-196">Například pokud vzorkování zahodí 75 % původní události, pak itemCount == 4 v udržených záznamy – to znamená, pro každý záznam zachované existovaly čtyři původní záznamy.</span><span class="sxs-lookup"><span data-stu-id="0d180-196">For example, if sampling discards 75% of the original events, then itemCount==4 in the retained records - that is, for every retained record, there were four original records.</span></span>

<span data-ttu-id="0d180-197">Adaptivního vzorkování způsobí, že itemCount být v obdobích, kdy aplikace je se nejčastěji používá vyšší.</span><span class="sxs-lookup"><span data-stu-id="0d180-197">Adaptive sampling causes itemCount to be higher during periods when your application is being heavily used.</span></span>

<span data-ttu-id="0d180-198">Shrnutí itemCount proto poskytuje dobrý odhad původní počet událostí.</span><span class="sxs-lookup"><span data-stu-id="0d180-198">Summing up itemCount therefore gives a good estimate of the original number of events.</span></span>

![](./media/app-insights-analytics-tour/510.png)

<span data-ttu-id="0d180-199">K dispozici je také `count()` agregace (a počet operaci) pro případy, kdy Opravdu chcete počet řádků ve skupině.</span><span class="sxs-lookup"><span data-stu-id="0d180-199">There's also a `count()` aggregation (and a count operation), for cases where you really do want to count the number of rows in a group.</span></span>

<span data-ttu-id="0d180-200">Je rozsah [funkce agregace](https://docs.loganalytics.io/learn/tutorials/aggregations.html).</span><span class="sxs-lookup"><span data-stu-id="0d180-200">There's a range of [aggregation functions](https://docs.loganalytics.io/learn/tutorials/aggregations.html).</span></span>

## <a name="charting-the-results"></a><span data-ttu-id="0d180-201">Vytváření grafů výsledky</span><span class="sxs-lookup"><span data-stu-id="0d180-201">Charting the results</span></span>
```AIQL

    exceptions
       | summarize count=sum(itemCount)  
         by bin(timestamp, 1h)
```

<span data-ttu-id="0d180-202">Ve výchozím nastavení zobrazí výsledky jako tabulku:</span><span class="sxs-lookup"><span data-stu-id="0d180-202">By default, results display as a table:</span></span>

![](./media/app-insights-analytics-tour/225.png)

<span data-ttu-id="0d180-203">Můžeme dělat líp, než tabulka zobrazení.</span><span class="sxs-lookup"><span data-stu-id="0d180-203">We can do better than the table view.</span></span> <span data-ttu-id="0d180-204">Podívejme se na výsledky v zobrazení grafu se svislicí panel možnost:</span><span class="sxs-lookup"><span data-stu-id="0d180-204">Let's look at the results in the chart view with the vertical bar option:</span></span>

![Klikněte na graf, vyberte svislý pruhový graf a přiřadit x a y osy](./media/app-insights-analytics-tour/230.png)

<span data-ttu-id="0d180-206">Všimněte si, že i když jsme nebyla seřadit výsledky podle času (jak je vidět v zobrazení tabulky), zobrazení grafu vždy zobrazuje data a času ve správném pořadí.</span><span class="sxs-lookup"><span data-stu-id="0d180-206">Notice that although we didn't sort the results by time (as you can see in the table display), the chart display always shows datetimes in correct order.</span></span>


## <a name="timecharts"></a><span data-ttu-id="0d180-207">Timecharts</span><span class="sxs-lookup"><span data-stu-id="0d180-207">Timecharts</span></span>
<span data-ttu-id="0d180-208">Zobrazit, kolik události existuje se každou hodinu:</span><span class="sxs-lookup"><span data-stu-id="0d180-208">Show how many events there are each hour:</span></span>

```AIQL

    requests
      | summarize event_count=sum(itemCount)
        by bin(timestamp, 1h)
```

<span data-ttu-id="0d180-209">Vyberte možnost zobrazení grafu:</span><span class="sxs-lookup"><span data-stu-id="0d180-209">Select the Chart display option:</span></span>

![timechart](./media/app-insights-analytics-tour/080.png)

## <a name="multiple-series"></a><span data-ttu-id="0d180-211">Více řad</span><span class="sxs-lookup"><span data-stu-id="0d180-211">Multiple series</span></span>
<span data-ttu-id="0d180-212">Více výrazů v `summarize` klauzule vytvoří více sloupců.</span><span class="sxs-lookup"><span data-stu-id="0d180-212">Multiple expressions in the `summarize` clause creates multiple columns.</span></span>

<span data-ttu-id="0d180-213">Více výrazů v `by` klauzule vytvoří více řádků, jeden pro každou kombinaci hodnot.</span><span class="sxs-lookup"><span data-stu-id="0d180-213">Multiple expressions in the `by` clause creates multiple rows, one for each combination of values.</span></span>

```AIQL

    requests
    | summarize count_=sum(itemCount), avg(duration)
      by bin(timestamp, 1h), client_StateOrProvince, client_City
    | order by timestamp asc, client_StateOrProvince, client_City
```

![Tabulka požadavků podle hodin a umístění](./media/app-insights-analytics-tour/090.png)

### <a name="segment-a-chart-by-dimensions"></a><span data-ttu-id="0d180-215">Rozdělit grafu pomocí dimenzí</span><span class="sxs-lookup"><span data-stu-id="0d180-215">Segment a chart by dimensions</span></span>
<span data-ttu-id="0d180-216">Pokud jste grafu tabulku, která má sloupec řetězce a je číselný sloupec, řetězec slouží k rozdělení číselná data do samostatné řady bodů.</span><span class="sxs-lookup"><span data-stu-id="0d180-216">If you chart a table that has a string column and a numeric column, the string can be used to split the numeric data into separate series of points.</span></span> <span data-ttu-id="0d180-217">Pokud existuje více než jeden sloupec řetězec, můžete použít jako diskriminátoru sloupec.</span><span class="sxs-lookup"><span data-stu-id="0d180-217">If there's more than one string column, you can choose which column to use as the discriminator.</span></span>

![Segment graf analýzy](./media/app-insights-analytics-tour/100.png)

#### <a name="bounce-rate"></a><span data-ttu-id="0d180-219">Odraz rychlost</span><span class="sxs-lookup"><span data-stu-id="0d180-219">Bounce rate</span></span>

<span data-ttu-id="0d180-220">Převeďte na logickou hodnotu na řetězec, můžete použít jako diskriminátoru:</span><span class="sxs-lookup"><span data-stu-id="0d180-220">Convert a boolean to a string to use it as a discriminator:</span></span>

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

### <a name="display-multiple-metrics"></a><span data-ttu-id="0d180-221">Zobrazení více metriky</span><span class="sxs-lookup"><span data-stu-id="0d180-221">Display multiple metrics</span></span>
<span data-ttu-id="0d180-222">Pokud grafu tabulku, která má více než jeden číselný sloupec, kromě časové razítko, můžete zobrazit libovolnou kombinací.</span><span class="sxs-lookup"><span data-stu-id="0d180-222">If you chart a table that has more than one numeric column, in addition to the timestamp, you can display any combination of them.</span></span>

![Segment graf analýzy](./media/app-insights-analytics-tour/110.png)

<span data-ttu-id="0d180-224">Je nutné vybrat **nemáte rozdělení** mohli vybrat více číselné sloupce.</span><span class="sxs-lookup"><span data-stu-id="0d180-224">You must select **Don't Split** before you can select multiple numeric columns.</span></span> <span data-ttu-id="0d180-225">Nelze rozdělit sloupec řetězce ve stejnou dobu jako zobrazení více než jeden číselný sloupec.</span><span class="sxs-lookup"><span data-stu-id="0d180-225">You can't split by a string column at the same time as displaying more than one numeric column.</span></span>

## <a name="daily-average-cycle"></a><span data-ttu-id="0d180-226">Průměrný denní cyklu</span><span class="sxs-lookup"><span data-stu-id="0d180-226">Daily average cycle</span></span>
<span data-ttu-id="0d180-227">Jak se liší využití přes průměrného dne?</span><span class="sxs-lookup"><span data-stu-id="0d180-227">How does usage vary over the average day?</span></span>

<span data-ttu-id="0d180-228">Počet požadavků podle času modulo jeden den, binned do hodiny:</span><span class="sxs-lookup"><span data-stu-id="0d180-228">Count requests by the time modulo one day, binned into hours:</span></span>

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
> <span data-ttu-id="0d180-230">Všimněte si, že máme aktuálně převést dobách trvání na data a času, aby bylo možné zobrazit na spojnicový graf.</span><span class="sxs-lookup"><span data-stu-id="0d180-230">Notice that we currently have to convert time durations to datetimes in order to display on a line chart.</span></span>
>
>

## <a name="compare-multiple-daily-series"></a><span data-ttu-id="0d180-231">Porovnání více denní řad</span><span class="sxs-lookup"><span data-stu-id="0d180-231">Compare multiple daily series</span></span>
<span data-ttu-id="0d180-232">Jak využití liší přes čas, kdy v různých zemí?</span><span class="sxs-lookup"><span data-stu-id="0d180-232">How does usage vary over the time of day in different countries?</span></span>

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

## <a name="plot-a-distribution"></a><span data-ttu-id="0d180-234">Vykreslení distribuce</span><span class="sxs-lookup"><span data-stu-id="0d180-234">Plot a distribution</span></span>
<span data-ttu-id="0d180-235">Kolik relací dochází z různých délek?</span><span class="sxs-lookup"><span data-stu-id="0d180-235">How many sessions are there of different lengths?</span></span>

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

<span data-ttu-id="0d180-236">Poslední řádek je potřeba převést na typ datetime.</span><span class="sxs-lookup"><span data-stu-id="0d180-236">The last line is required to convert to datetime.</span></span> <span data-ttu-id="0d180-237">Aktuálně osy x grafu se zobrazuje jako skalární hodnota, pouze pokud je hodnota datetime.</span><span class="sxs-lookup"><span data-stu-id="0d180-237">Currently the x axis of a chart is displayed as a scalar only if it is a datetime.</span></span>

<span data-ttu-id="0d180-238">`where` Klauzule vyloučí jednorázové relací (sessionDuration == 0) a nastaví délku osy x.</span><span class="sxs-lookup"><span data-stu-id="0d180-238">The `where` clause excludes one-shot sessions (sessionDuration==0) and sets the length of the x-axis.</span></span>

![](./media/app-insights-analytics-tour/290.png)

## <a name="percentileshttpsdocsloganalyticsioquerylanguagequerylanguagepercentilesaggfunctionhtml"></a>[<span data-ttu-id="0d180-239">Percentily</span><span class="sxs-lookup"><span data-stu-id="0d180-239">Percentiles</span></span>](https://docs.loganalytics.io/queryLanguage/query_language_percentiles_aggfunction.html)
<span data-ttu-id="0d180-240">Jaké rozsahy doby trvání zahrnují různá procenta relace?</span><span class="sxs-lookup"><span data-stu-id="0d180-240">What ranges of durations cover different percentages of sessions?</span></span>

<span data-ttu-id="0d180-241">Pomocí výše uvedeném dotazu, ale nahraďte poslední řádek:</span><span class="sxs-lookup"><span data-stu-id="0d180-241">Use the above query, but replace the last line:</span></span>

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

<span data-ttu-id="0d180-242">Také jsme odebrali horní limit v where klauzule, aby bylo možné získat správné údaje včetně všechny relace s více než jeden požadavek:</span><span class="sxs-lookup"><span data-stu-id="0d180-242">We also removed the upper limit in the where clause, in order to get correct figures including all sessions with more than one request:</span></span>

![výsledek](./media/app-insights-analytics-tour/180.png)

<span data-ttu-id="0d180-244">Odkud jsme uvidíte, že:</span><span class="sxs-lookup"><span data-stu-id="0d180-244">From which we can see that:</span></span>

* <span data-ttu-id="0d180-245">5 % relací být méně než tři minuty 34s;</span><span class="sxs-lookup"><span data-stu-id="0d180-245">5% of sessions have a duration of less than 3 minutes 34s;</span></span>
* <span data-ttu-id="0d180-246">50 % relací poslední nejvýše 36 minut;</span><span class="sxs-lookup"><span data-stu-id="0d180-246">50% of sessions last less than 36 minutes;</span></span>
* <span data-ttu-id="0d180-247">5 % relací poslední více než 7 dní</span><span class="sxs-lookup"><span data-stu-id="0d180-247">5% of sessions last more than 7 days</span></span>

<span data-ttu-id="0d180-248">Získat samostatné rozpis pro každé země, jsme právě mít aby shrnout sloupci client_CountryOrRegion samostatně prostřednictvím obě operátory:</span><span class="sxs-lookup"><span data-stu-id="0d180-248">To get a separate breakdown for each country, we just have to bring the client_CountryOrRegion column separately through both summarize operators:</span></span>

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

## <a name="join"></a><span data-ttu-id="0d180-249">Spojit</span><span class="sxs-lookup"><span data-stu-id="0d180-249">Join</span></span>
<span data-ttu-id="0d180-250">Budeme mít přístup k několika tabulky, včetně požadavky a výjimkami.</span><span class="sxs-lookup"><span data-stu-id="0d180-250">We have access to several tables, including requests and exceptions.</span></span>

<span data-ttu-id="0d180-251">Výjimky související s požadavek, který vrátil neplatnou odpověď najdete jsme se můžete zapojit do tabulky na `session_Id`:</span><span class="sxs-lookup"><span data-stu-id="0d180-251">To find the exceptions related to a request that returned a failure response, we can join the tables on `session_Id`:</span></span>

```AIQL

    requests
    | where toint(resultCode) >= 500
    | join (exceptions) on operation_Id
    | take 30
```


<span data-ttu-id="0d180-252">Je vhodné použít `project` vyberte právě sloupce je třeba před provedením spojení.</span><span class="sxs-lookup"><span data-stu-id="0d180-252">It's good practice to use `project` to select just the columns we need before performing the join.</span></span>
<span data-ttu-id="0d180-253">V klauzulích stejné jsme přejmenovat sloupec časového razítka.</span><span class="sxs-lookup"><span data-stu-id="0d180-253">In the same clauses, we rename the timestamp column.</span></span>

## <a name="lethttpsdocsloganalyticsioquerylanguagequerylanguageletstatementhtml-assign-a-result-to-a-variable"></a><span data-ttu-id="0d180-254">[Umožní](https://docs.loganalytics.io/queryLanguage/query_language_letstatement.html): přiřadit výsledek proměnné</span><span class="sxs-lookup"><span data-stu-id="0d180-254">[Let](https://docs.loganalytics.io/queryLanguage/query_language_letstatement.html): Assign a result to a variable</span></span>

<span data-ttu-id="0d180-255">Použití `let` oddělit části předchozí výrazu.</span><span class="sxs-lookup"><span data-stu-id="0d180-255">Use `let` to separate out the parts of the previous expression.</span></span> <span data-ttu-id="0d180-256">Výsledky jsou stejné jako:</span><span class="sxs-lookup"><span data-stu-id="0d180-256">The results are unchanged:</span></span>

```AIQL

    let bad_requests =
      requests
        | where  toint(resultCode) >= 500  ;
    bad_requests
    | join (exceptions) on session_Id
    | take 30
```

> [!Tip] 
> <span data-ttu-id="0d180-257">V klientovi analýzy, umístěte prázdné řádky mezi částí dotazu.</span><span class="sxs-lookup"><span data-stu-id="0d180-257">In the Analytics client, don't put blank lines between the parts of the query.</span></span> <span data-ttu-id="0d180-258">Ujistěte se, že všechny jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="0d180-258">Make sure to execute all of it.</span></span>
>

<span data-ttu-id="0d180-259">Použití `toscalar` převést na hodnotu jedna buňka tabulky:</span><span class="sxs-lookup"><span data-stu-id="0d180-259">Use `toscalar` to convert a single table cell to a value:</span></span>

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


### <a name="functions"></a><span data-ttu-id="0d180-260">Funkce</span><span class="sxs-lookup"><span data-stu-id="0d180-260">Functions</span></span>

<span data-ttu-id="0d180-261">Použití *Let* definice funkce:</span><span class="sxs-lookup"><span data-stu-id="0d180-261">Use *Let* to define a function:</span></span>

```AIQL

    let usdate = (t:datetime)
    {
      strcat(getmonth(t), "/", dayofmonth(t),"/", getyear(t), " ",
      bin((t-1h)%12h+1h,1s), iff(t%24h<12h, "AM", "PM"))
    };
    requests  
    | extend PST = usdate(timestamp-8h)
```

## <a name="accessing-nested-objects"></a><span data-ttu-id="0d180-262">Přístup k vnořených objektů</span><span class="sxs-lookup"><span data-stu-id="0d180-262">Accessing nested objects</span></span>
<span data-ttu-id="0d180-263">Vnořených objektů lze snadno přistupovat.</span><span class="sxs-lookup"><span data-stu-id="0d180-263">Nested objects can be accessed easily.</span></span> <span data-ttu-id="0d180-264">V datovém proudu výjimky, můžete například zjistit strukturovaných objekty takto:</span><span class="sxs-lookup"><span data-stu-id="0d180-264">For example, in the exceptions stream you can see structured objects like this:</span></span>

![výsledek](./media/app-insights-analytics-tour/520.png)

<span data-ttu-id="0d180-266">Můžete ho vyrovnání výběrem vlastnosti, které vás zajímají:</span><span class="sxs-lookup"><span data-stu-id="0d180-266">You can flatten it by choosing the properties you're interested in:</span></span>

```AIQL

    exceptions | take 10
    | extend method1 = tostring(details[0].parsedStack[1].method)
```

<span data-ttu-id="0d180-267">Všimněte si, že budete muset přetypování výsledek, který má příslušného typu.</span><span class="sxs-lookup"><span data-stu-id="0d180-267">Note that you need to cast the result to the appropriate type.</span></span>


## <a name="custom-properties-and-measurements"></a><span data-ttu-id="0d180-268">Vlastní vlastnosti a měření</span><span class="sxs-lookup"><span data-stu-id="0d180-268">Custom properties and measurements</span></span>
<span data-ttu-id="0d180-269">Pokud vaše aplikace připojí [vlastní dimenze (Vlastnosti) a vlastní měření](app-insights-api-custom-events-metrics.md#properties) na události, pak se zobrazí je v `customDimensions` a `customMeasurements` objekty.</span><span class="sxs-lookup"><span data-stu-id="0d180-269">If your application attaches [custom dimensions (properties) and custom measurements](app-insights-api-custom-events-metrics.md#properties) to events, then you will see them in the `customDimensions` and `customMeasurements` objects.</span></span>

<span data-ttu-id="0d180-270">Například pokud vaše aplikace obsahuje:</span><span class="sxs-lookup"><span data-stu-id="0d180-270">For example, if your app includes:</span></span>

```C#

    var dimensions = new Dictionary<string, string>
                     {{"p1", "v1"},{"p2", "v2"}};
    var measurements = new Dictionary<string, double>
                     {{"m1", 42.0}, {"m2", 43.2}};
    telemetryClient.TrackEvent("myEvent", dimensions, measurements);
```

<span data-ttu-id="0d180-271">K extrakci v Analytics tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="0d180-271">To extract these values in Analytics:</span></span>

```AIQL

    customEvents
    | extend p1 = customDimensions.p1,
      m1 = todouble(customMeasurements.m1) // cast to expected type

```

<span data-ttu-id="0d180-272">Chcete-li ověřit, zda je vlastní dimenze určitého typu:</span><span class="sxs-lookup"><span data-stu-id="0d180-272">To verify whether a custom dimension is of a particular type:</span></span>

```AIQL

    customEvents
    | extend p1 = customDimensions.p1,
      iff(notnull(todouble(customMeasurements.m1)), ...
```

## <a name="dashboards"></a><span data-ttu-id="0d180-273">Řídicí panely</span><span class="sxs-lookup"><span data-stu-id="0d180-273">Dashboards</span></span>
<span data-ttu-id="0d180-274">Aby bylo možné shromáždit všechny vaše nejdůležitější grafů a tabulek je budete moct připnout výsledky na řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="0d180-274">You can pin your results to a dashboard in order to bring together all your most important charts and tables.</span></span>

* <span data-ttu-id="0d180-275">[Řídicí panel Azure sdílené](app-insights-dashboards.md#share-dashboards): Kliknutím na ikonu Připnutí.</span><span class="sxs-lookup"><span data-stu-id="0d180-275">[Azure shared dashboard](app-insights-dashboards.md#share-dashboards): Click the pin icon.</span></span> <span data-ttu-id="0d180-276">Než to uděláte, musíte mít sdílené řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="0d180-276">Before you do this, you must have a shared dashboard.</span></span> <span data-ttu-id="0d180-277">Na portálu Azure otevřete nebo vytvořit řídicí panel a klikněte na sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="0d180-277">In the Azure portal, open or create a dashboard and click Share.</span></span>
* <span data-ttu-id="0d180-278">[Řídicí panel Power BI](app-insights-export-power-bi.md): klikněte na exportovat, Power BI dotazu.</span><span class="sxs-lookup"><span data-stu-id="0d180-278">[Power BI dashboard](app-insights-export-power-bi.md): Click Export, Power BI Query.</span></span> <span data-ttu-id="0d180-279">Výhodou této alternativní je, že můžete zobrazit dotaz spolu s další výsledky z široké škály zdrojů.</span><span class="sxs-lookup"><span data-stu-id="0d180-279">An advantage of this alternative is that you can display your query alongside other results from a wide range of sources.</span></span>

## <a name="combine-with-imported-data"></a><span data-ttu-id="0d180-280">Kombinovat s importovanými daty</span><span class="sxs-lookup"><span data-stu-id="0d180-280">Combine with imported data</span></span>

<span data-ttu-id="0d180-281">Sestavy analýzy vypadat skvělé na řídicím panelu, ale někdy chcete přeložit data do více stravitelné formuláře.</span><span class="sxs-lookup"><span data-stu-id="0d180-281">Analytics reports look great on the dashboard, but sometimes you want to translate the data to a more digestible form.</span></span> <span data-ttu-id="0d180-282">Předpokládejme například, že ověření uživatelé se budou identifikovat telemetrie podle alias.</span><span class="sxs-lookup"><span data-stu-id="0d180-282">For example, suppose your authenticated users are identified in the telemetry by an alias.</span></span> <span data-ttu-id="0d180-283">Chcete zobrazit jejich skutečné názvy ve výsledcích.</span><span class="sxs-lookup"><span data-stu-id="0d180-283">You'd like to show their real names in your results.</span></span> <span data-ttu-id="0d180-284">Chcete-li to provést, musíte soubor CSV, který se mapuje na skutečné názvy z aliasy.</span><span class="sxs-lookup"><span data-stu-id="0d180-284">To do this, you need a CSV file that maps from the aliases to the real names.</span></span>

<span data-ttu-id="0d180-285">Můžete importovat soubor dat a použít ho stejně jako všechny standardní tabulek (požadavky, výjimky a tak dále).</span><span class="sxs-lookup"><span data-stu-id="0d180-285">You can import a data file and use it just like any of the standard tables (requests, exceptions, and so on).</span></span> <span data-ttu-id="0d180-286">Buď dotaz na svůj vlastní nebo se připojte s jinou tabulkou.</span><span class="sxs-lookup"><span data-stu-id="0d180-286">Either query it on its own, or join it with other tables.</span></span> <span data-ttu-id="0d180-287">Například, pokud máte tabulku s názvem usermap a má sloupce `realName` a `userId`, můžete ho použít k překladu `user_AuthenticatedId` pole v požadavku telemetrie:</span><span class="sxs-lookup"><span data-stu-id="0d180-287">For example, if you have a table named usermap, and it has columns `realName` and `userId`, then you can use it to translate the `user_AuthenticatedId` field in the request telemetry:</span></span>

```AIQL

    requests
    | where notempty(user_AuthenticatedId)
    | project userId = user_AuthenticatedId
      // get the realName field from the usermap table:
    | join kind=leftouter ( usermap ) on userId
      // count transactions by name:
    | summarize count() by realName
```

<span data-ttu-id="0d180-288">V části Import tabulky, v okně schématu **jiných zdrojů dat**, postupujte podle pokynů a přidat nový zdroj dat, tím, že nahrajete ukázková data.</span><span class="sxs-lookup"><span data-stu-id="0d180-288">To import a table, in the Schema blade, under **Other Data Sources**, follow the instructions to add a new data source, by uploading a sample of your data.</span></span> <span data-ttu-id="0d180-289">Pak můžete použít tuto definici k nahrání tabulky.</span><span class="sxs-lookup"><span data-stu-id="0d180-289">Then you can use this definition to upload tables.</span></span>

<span data-ttu-id="0d180-290">Funkce importu je aktuálně ve verzi preview, takže původně uvidíte odkaz "Kontaktujte nás" v části "Jiné zdroje dat."</span><span class="sxs-lookup"><span data-stu-id="0d180-290">The import feature is currently in preview, so you will initially see a "Contact us" link under "Other data sources."</span></span> <span data-ttu-id="0d180-291">Tato možnost slouží k zaregistrovat do programu preview a na odkaz se nahradí klikněte tlačítko "Přidat nový zdroj dat".</span><span class="sxs-lookup"><span data-stu-id="0d180-291">Use this to sign up to the preview program, and the link will then be replaced by an "Add new data source" button.</span></span>


## <a name="tables"></a><span data-ttu-id="0d180-292">Tabulky</span><span class="sxs-lookup"><span data-stu-id="0d180-292">Tables</span></span>
<span data-ttu-id="0d180-293">Datový proud telemetrie přijaté z vaší aplikace je přístupná prostřednictvím několik tabulek.</span><span class="sxs-lookup"><span data-stu-id="0d180-293">The stream of telemetry received from your app is accessible through several tables.</span></span> <span data-ttu-id="0d180-294">Schéma vlastnosti, které jsou k dispozici pro každou tabulku je viditelný v levé části okna.</span><span class="sxs-lookup"><span data-stu-id="0d180-294">The schema of properties available for each table is visible at the left of the window.</span></span>

### <a name="requests-table"></a><span data-ttu-id="0d180-295">Tabulka požadavků</span><span class="sxs-lookup"><span data-stu-id="0d180-295">Requests table</span></span>
<span data-ttu-id="0d180-296">Počet HTTP žádosti na vaší webové aplikace a segment podle názvu stránky:</span><span class="sxs-lookup"><span data-stu-id="0d180-296">Count HTTP requests to your web app and segment by page name:</span></span>

![Počet požadavků segmentované podle názvu](./media/app-insights-analytics-tour/analytics-count-requests.png)

<span data-ttu-id="0d180-298">Vyhledá požadavky, které nesplní většinu:</span><span class="sxs-lookup"><span data-stu-id="0d180-298">Find the requests that fail most:</span></span>

![Počet požadavků segmentované podle názvu](./media/app-insights-analytics-tour/analytics-failed-requests.png)

### <a name="custom-events-table"></a><span data-ttu-id="0d180-300">Tabulka vlastní události</span><span class="sxs-lookup"><span data-stu-id="0d180-300">Custom events table</span></span>
<span data-ttu-id="0d180-301">Pokud používáte [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent) odesílat vlastní události, můžete přečíst je z této tabulky.</span><span class="sxs-lookup"><span data-stu-id="0d180-301">If you use [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent) to send your own events, you can read them from this table.</span></span>

<span data-ttu-id="0d180-302">Podívejme příklad kde kódu aplikace obsahuje tyto řádky:</span><span class="sxs-lookup"><span data-stu-id="0d180-302">Let's take an example where your app code contains these lines:</span></span>

```C#

    telemetry.TrackEvent("Query",
       new Dictionary<string,string> {{"query", sqlCmd}},
       new Dictionary<string,double> {
           {"retry", retryCount},
           {"querytime", totalTime}})
```

<span data-ttu-id="0d180-303">Zobrazí četnost tyto události:</span><span class="sxs-lookup"><span data-stu-id="0d180-303">Display the frequency of these events:</span></span>

![Zobrazení počet vlastní události](./media/app-insights-analytics-tour/analytics-custom-events-rate.png)

<span data-ttu-id="0d180-305">Extrahujte měření a dimenze z události:</span><span class="sxs-lookup"><span data-stu-id="0d180-305">Extract measurements and dimensions from the events:</span></span>

![Zobrazení počet vlastní události](./media/app-insights-analytics-tour/analytics-custom-events-dimensions.png)

### <a name="custom-metrics-table"></a><span data-ttu-id="0d180-307">Vlastní metriky tabulky</span><span class="sxs-lookup"><span data-stu-id="0d180-307">Custom metrics table</span></span>
<span data-ttu-id="0d180-308">Pokud používáte [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) k odeslání vlastní metriky hodnoty, najdete své výsledky v **customMetrics** datového proudu.</span><span class="sxs-lookup"><span data-stu-id="0d180-308">If you are using [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) to send your own metric values, you’ll find its results in the **customMetrics** stream.</span></span> <span data-ttu-id="0d180-309">Například:</span><span class="sxs-lookup"><span data-stu-id="0d180-309">For example:</span></span>  

![Vlastní metriky v analytics Application Insights](./media/app-insights-analytics-tour/analytics-custom-metrics.png)

> [!NOTE]
> <span data-ttu-id="0d180-311">V [Průzkumníku metrik](app-insights-metrics-explorer.md), všechny vlastní měření připojit k libovolnému typu telemetrie objeví spolu v okně metriky společně s metriky odeslaných pomocí `TrackMetric()`.</span><span class="sxs-lookup"><span data-stu-id="0d180-311">In [Metrics Explorer](app-insights-metrics-explorer.md), all custom measurements attached to any type of telemetry appear together in the metrics blade along with metrics sent using `TrackMetric()`.</span></span> <span data-ttu-id="0d180-312">Ale v analýzy, vlastní měření připojených stále libovolného typu byly provedeny na - události nebo požadavky a tak dále - při poslal TrackMetric metrika se zobrazí v vlastní datový proud telemetrie.</span><span class="sxs-lookup"><span data-stu-id="0d180-312">But in Analytics, custom measurements are still attached to whichever type of telemetry they were carried on - events or requests, and so on - while metrics sent by TrackMetric appear in their own stream.</span></span>
>
>

### <a name="performance-counters-table"></a><span data-ttu-id="0d180-313">Tabulka čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="0d180-313">Performance counters table</span></span>
<span data-ttu-id="0d180-314">[Čítače výkonu](app-insights-performance-counters.md) můžete zobrazit základní systémové metriky pro vaši aplikaci, například CPU, paměť a využití sítě.</span><span class="sxs-lookup"><span data-stu-id="0d180-314">[Performance counters](app-insights-performance-counters.md) show you basic system metrics for your app, such as CPU, memory, and network utilization.</span></span> <span data-ttu-id="0d180-315">Můžete nakonfigurovat SDK k odesílání další čítače, včetně vlastní vlastní čítače.</span><span class="sxs-lookup"><span data-stu-id="0d180-315">You can configure the SDK to send additional counters, including your own custom counters.</span></span>

<span data-ttu-id="0d180-316">**Čítače výkonu** schématu zpřístupní `category`, `counter` název, a `instance` název jednotlivých čítačů výkonu.</span><span class="sxs-lookup"><span data-stu-id="0d180-316">The **performanceCounters** schema exposes the `category`, `counter` name, and `instance` name of each performance counter.</span></span> <span data-ttu-id="0d180-317">Názvy instancí čítače platí jenom pro některé čítače výkonu a obvykle označení názvu procesu, k němuž se vztahuje na počet.</span><span class="sxs-lookup"><span data-stu-id="0d180-317">Counter instance names are only applicable to some performance counters, and typically indicate the name of the process to which the count relates.</span></span> <span data-ttu-id="0d180-318">V telemetrii pro každou aplikaci zobrazí se pouze čítačů pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0d180-318">In the telemetry for each application, you’ll see only the counters for that application.</span></span> <span data-ttu-id="0d180-319">Například pokud chcete zobrazit jsou k dispozici co čítače:</span><span class="sxs-lookup"><span data-stu-id="0d180-319">For example, to see what counters are available:</span></span>

![Čítače výkonu v analytics Application Insights](./media/app-insights-analytics-tour/analytics-performance-counters.png)

<span data-ttu-id="0d180-321">Pokud chcete získat graf dostupné paměti za vybrané období:</span><span class="sxs-lookup"><span data-stu-id="0d180-321">To get a chart of available memory over the selected period:</span></span>

![Paměť timechart v analytics Application Insights](./media/app-insights-analytics-tour/analytics-available-memory.png)

<span data-ttu-id="0d180-323">Jako další telemetrií **čítače výkonu** také má sloupec `cloud_RoleInstance` určující identitu hostitelský počítač, na kterém aplikace běží.</span><span class="sxs-lookup"><span data-stu-id="0d180-323">Like other telemetry, **performanceCounters** also has a column `cloud_RoleInstance` that indicates the identity of the host machine on which your app is running.</span></span> <span data-ttu-id="0d180-324">Chcete-li například porovnat výkon vaší aplikace na různé počítače:</span><span class="sxs-lookup"><span data-stu-id="0d180-324">For example, to compare the performance of your app on the different machines:</span></span>

![Výkon oddělených instance role ve službě Application Insights analytics](./media/app-insights-analytics-tour/analytics-metrics-role-instance.png)

### <a name="exceptions-table"></a><span data-ttu-id="0d180-326">Výjimky tabulky</span><span class="sxs-lookup"><span data-stu-id="0d180-326">Exceptions table</span></span>
<span data-ttu-id="0d180-327">[Výjimky hlášených aplikace](app-insights-asp-net-exceptions.md) jsou k dispozici v této tabulce.</span><span class="sxs-lookup"><span data-stu-id="0d180-327">[Exceptions reported by your app](app-insights-asp-net-exceptions.md) are available in this table.</span></span>

<span data-ttu-id="0d180-328">Pokud chcete najít požadavek HTTP, který aplikace byla zpracování, pokud byla vyvolána výjimka, připojení na operation_Id:</span><span class="sxs-lookup"><span data-stu-id="0d180-328">To find the HTTP request that your app was handling when the exception was raised, join on operation_Id:</span></span>

![Připojení výjimky s požadavky na operation_Id](./media/app-insights-analytics-tour/analytics-exception-request.png)

### <a name="browser-timings-table"></a><span data-ttu-id="0d180-330">Tabulka časování prohlížeče</span><span class="sxs-lookup"><span data-stu-id="0d180-330">Browser timings table</span></span>
<span data-ttu-id="0d180-331">`browserTimings`Zobrazuje data zatížení stránky shromažďují v prohlížečích vašich uživatelů.</span><span class="sxs-lookup"><span data-stu-id="0d180-331">`browserTimings` shows page load data collected in your users' browsers.</span></span>

<span data-ttu-id="0d180-332">[Nastavit aplikaci pro telemetrických dat na straně klienta](app-insights-javascript.md) Chcete-li zobrazit tyto metriky.</span><span class="sxs-lookup"><span data-stu-id="0d180-332">[Set up your app for client-side telemetry](app-insights-javascript.md) in order to see these metrics.</span></span>

<span data-ttu-id="0d180-333">Schéma zahrnuje [metrika označující délek různých fázích načítání proces stránky](app-insights-javascript.md#page-load-performance).</span><span class="sxs-lookup"><span data-stu-id="0d180-333">The schema includes [metrics indicating the lengths of different stages of the page loading process](app-insights-javascript.md#page-load-performance).</span></span> <span data-ttu-id="0d180-334">(Není indikují dobu, kterou uživatelé přečíst na stránce.)</span><span class="sxs-lookup"><span data-stu-id="0d180-334">(They don’t indicate the length of time your users read a page.)</span></span>  

<span data-ttu-id="0d180-335">Zobrazit popularities různých stránek a načíst časy pro jednotlivé stránky:</span><span class="sxs-lookup"><span data-stu-id="0d180-335">Show the popularities of different pages, and load times for each page:</span></span>

![Časů načtení stránky v Analytics](./media/app-insights-analytics-tour/analytics-page-load.png)

### <a name="availability-results-table"></a><span data-ttu-id="0d180-337">Tabulky výsledků dostupnosti</span><span class="sxs-lookup"><span data-stu-id="0d180-337">Availability results table</span></span>
<span data-ttu-id="0d180-338">`availabilityResults`Zobrazuje výsledky vaše [webové testy](app-insights-monitor-web-app-availability.md).</span><span class="sxs-lookup"><span data-stu-id="0d180-338">`availabilityResults` shows the results of your [web tests](app-insights-monitor-web-app-availability.md).</span></span> <span data-ttu-id="0d180-339">Každé spuštění testů z umístění každého testu se hlásí samostatně.</span><span class="sxs-lookup"><span data-stu-id="0d180-339">Each run of your tests from each test location is reported separately.</span></span>

![Časů načtení stránky v Analytics](./media/app-insights-analytics-tour/analytics-availability.png)

### <a name="dependencies-table"></a><span data-ttu-id="0d180-341">Tabulka závislosti</span><span class="sxs-lookup"><span data-stu-id="0d180-341">Dependencies table</span></span>
<span data-ttu-id="0d180-342">Obsahuje výsledky volání, že vaše aplikace vytvoří databáze a rozhraní REST API a dalších žádostí na TrackDependency().</span><span class="sxs-lookup"><span data-stu-id="0d180-342">Contains results of calls that your app makes to databases and REST APIs, and other calls to TrackDependency().</span></span> <span data-ttu-id="0d180-343">Zahrnuje taky volání AJAX provedená z prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="0d180-343">Also includes AJAX calls made from the browser.</span></span>

<span data-ttu-id="0d180-344">Volání AJAX z prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="0d180-344">AJAX calls from the browser:</span></span>

```AIQL

    dependencies | where client_Type == "Browser"
    | take 10
```

<span data-ttu-id="0d180-345">Závislost volání ze serveru:</span><span class="sxs-lookup"><span data-stu-id="0d180-345">Dependency calls from the server:</span></span>

```AIQL

    dependencies | where client_Type == "PC"
    | take 10
```

<span data-ttu-id="0d180-346">Vždy zobrazovat výsledky v závislosti na straně serveru `success==False` Pokud není nainstalován Application Insights Agent.</span><span class="sxs-lookup"><span data-stu-id="0d180-346">Server-side dependency results always show `success==False` if the Application Insights Agent is not installed.</span></span> <span data-ttu-id="0d180-347">Dalších dat, ale jsou správné.</span><span class="sxs-lookup"><span data-stu-id="0d180-347">However, the other data are correct.</span></span>

### <a name="traces-table"></a><span data-ttu-id="0d180-348">Tabulka trasování</span><span class="sxs-lookup"><span data-stu-id="0d180-348">Traces table</span></span>
<span data-ttu-id="0d180-349">Obsahuje telemetrické zprávy odesílané vaší aplikace pomocí TrackTrace(), nebo [jiných rozhraní protokolování](app-insights-asp-net-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="0d180-349">Contains the telemetry sent by your app using TrackTrace(), or [other logging frameworks](app-insights-asp-net-trace-logs.md).</span></span>

## <a name="video"></a><span data-ttu-id="0d180-350">Video</span><span class="sxs-lookup"><span data-stu-id="0d180-350">Video</span></span> 

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

<span data-ttu-id="0d180-351">Pokročilé dotazy:</span><span class="sxs-lookup"><span data-stu-id="0d180-351">Advanced queries:</span></span>

> [!VIDEO https://channel9.msdn.com/Events/Build/2016/P591/player]


## <a name="next-steps"></a><span data-ttu-id="0d180-352">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0d180-352">Next steps</span></span>
* [<span data-ttu-id="0d180-353">Referenční dokumentace jazyka Analytics</span><span class="sxs-lookup"><span data-stu-id="0d180-353">Analytics language reference</span></span>](app-insights-analytics-reference.md)
* <span data-ttu-id="0d180-354">[SQL-uživatelů tahák](https://aka.ms/sql-analytics) překládá nejběžnější idioms.</span><span class="sxs-lookup"><span data-stu-id="0d180-354">[SQL-users' cheat sheet](https://aka.ms/sql-analytics) translates the most common idioms.</span></span>

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]
