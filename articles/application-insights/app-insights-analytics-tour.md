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
# <a name="a-tour-of-analytics-in-application-insights"></a><span data-ttu-id="0d43c-103">Prohlídka Analytics ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="0d43c-103">A tour of Analytics in Application Insights</span></span>
<span data-ttu-id="0d43c-104">[Analýza](app-insights-analytics.md) je výkonný vyhledávání funkcí hello [Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0d43c-104">[Analytics](app-insights-analytics.md) is hello powerful search feature of [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="0d43c-105">Tyto stránek popisují dotazovací jazyk analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="0d43c-105">These pages describe the Log Analytics query language.</span></span>

* <span data-ttu-id="0d43c-106">**[Podívejte se na úvodní video hello](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span><span class="sxs-lookup"><span data-stu-id="0d43c-106">**[Watch hello introductory video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span></span>
* <span data-ttu-id="0d43c-107">**[Vyzkoušejte Analytics na naše simulované data](https://analytics.applicationinsights.io/demo)**  Pokud aplikace není ještě odesílá data tooApplication statistiky.</span><span class="sxs-lookup"><span data-stu-id="0d43c-107">**[Test drive Analytics on our simulated data](https://analytics.applicationinsights.io/demo)** if your app isn't sending data tooApplication Insights yet.</span></span>
* <span data-ttu-id="0d43c-108">**[SQL-uživatelů tahák](https://aka.ms/sql-analytics)**  překládá nejběžnější idioms hello.</span><span class="sxs-lookup"><span data-stu-id="0d43c-108">**[SQL-users' cheat sheet](https://aka.ms/sql-analytics)** translates hello most common idioms.</span></span>

<span data-ttu-id="0d43c-109">Podívejme procházení prostřednictvím některé tooget základní dotazy, které jste spustili.</span><span class="sxs-lookup"><span data-stu-id="0d43c-109">Let's take a walk through some basic queries tooget you started.</span></span>

## <a name="connect-tooyour-application-insights-data"></a><span data-ttu-id="0d43c-110">Připojení dat tooyour Application Insights</span><span class="sxs-lookup"><span data-stu-id="0d43c-110">Connect tooyour Application Insights data</span></span>
<span data-ttu-id="0d43c-111">Otevřete Analytics z vaší aplikace [okno Přehled](app-insights-dashboards.md) ve službě Application Insights:</span><span class="sxs-lookup"><span data-stu-id="0d43c-111">Open Analytics from your app's [overview blade](app-insights-dashboards.md) in Application Insights:</span></span>

![Otevřete portal.azure.com otevřete prostředek Application Insights a klikněte na Analytics.](./media/app-insights-analytics-tour/001.png)

## <a name="takehttpsdocsloganalyticsioquerylanguagequerylanguagetakeoperatorhtml-show-me-n-rows"></a><span data-ttu-id="0d43c-113">[Trvat](https://docs.loganalytics.io/queryLanguage/query_language_takeoperator.html): Zobrazit mi n řádků</span><span class="sxs-lookup"><span data-stu-id="0d43c-113">[Take](https://docs.loganalytics.io/queryLanguage/query_language_takeoperator.html): show me n rows</span></span>
<span data-ttu-id="0d43c-114">Datové body, které protokolu operace uživatele (obvykle HTTP přijatých požadavků ve vaší webové aplikace) jsou uložené v tabulce s názvem `requests`.</span><span class="sxs-lookup"><span data-stu-id="0d43c-114">Data points that log user operations (typically HTTP requests received by your web app) are stored in a table called `requests`.</span></span> <span data-ttu-id="0d43c-115">Každý řádek je telemetrie datový bod, který přijal od hello Application Insights SDK ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0d43c-115">Each row is a telemetry data point received from hello Application Insights SDK in your app.</span></span>

<span data-ttu-id="0d43c-116">Začněme prověřením několik ukázkových řádky tabulky hello:</span><span class="sxs-lookup"><span data-stu-id="0d43c-116">Let's start by examining a few sample rows of hello table:</span></span>

![výsledky](./media/app-insights-analytics-tour/010.png)

> [!NOTE]
> <span data-ttu-id="0d43c-118">Uveďte hello kurzoru někde v příkazu hello před kliknutím na Přejít.</span><span class="sxs-lookup"><span data-stu-id="0d43c-118">Put hello cursor somewhere in hello statement before you click Go.</span></span> <span data-ttu-id="0d43c-119">Příkaz můžete rozdělit přes více než jeden řádek, ale nemáte Vložit prázdné řádky v příkazu.</span><span class="sxs-lookup"><span data-stu-id="0d43c-119">You can split a statement over more than one line, but don't put blank lines in a statement.</span></span> <span data-ttu-id="0d43c-120">Prázdné řádky jsou pohodlný způsob tookeep několik jednotlivých dotazů v okně hello.</span><span class="sxs-lookup"><span data-stu-id="0d43c-120">Blank lines are a convenient way tookeep several separate queries in hello window.</span></span>
>
>

<span data-ttu-id="0d43c-121">Vyberte sloupce, přetáhněte je seskupit podle sloupce a filtrování:</span><span class="sxs-lookup"><span data-stu-id="0d43c-121">Choose columns, drag them, group by columns, and filter:</span></span>

![Klikněte na výběr sloupců v pravé horní části výsledků](./media/app-insights-analytics-tour/030.png)

<span data-ttu-id="0d43c-123">Rozbalte všechny podrobnosti hello toosee položek:</span><span class="sxs-lookup"><span data-stu-id="0d43c-123">Expand any item toosee hello detail:</span></span>

![Vyberte tabulky a použití konfigurace sloupců](./media/app-insights-analytics-tour/040.png)

> [!NOTE]
> <span data-ttu-id="0d43c-125">Klikněte na tlačítko hello head k dispozici ve webovém prohlížeči hello sloupec výsledků hello toore pořadí.</span><span class="sxs-lookup"><span data-stu-id="0d43c-125">Click hello head of a column toore-order hello results available in hello web browser.</span></span> <span data-ttu-id="0d43c-126">Ale uvědomte si, že pro sadu velké výsledek hello počet řádků stažené toohello prohlížeče je omezená.</span><span class="sxs-lookup"><span data-stu-id="0d43c-126">But be aware that for a large result set, hello number of rows downloaded toohello browser is limited.</span></span> <span data-ttu-id="0d43c-127">Řazení tímto způsobem není vždy zobrazí hello skutečné nejvyšší nebo nejnižší položky.</span><span class="sxs-lookup"><span data-stu-id="0d43c-127">Sorting this way doesn't always show you hello actual highest or lowest items.</span></span> <span data-ttu-id="0d43c-128">položky toosort spolehlivě, použijte hello `top` nebo `sort` operátor.</span><span class="sxs-lookup"><span data-stu-id="0d43c-128">toosort items reliably, use hello `top` or `sort` operator.</span></span>
>
>

## <a name="tophttpsdocsloganalyticsioquerylanguagequerylanguagetopoperatorhtml-and-sorthttpsdocsloganalyticsioquerylanguagequerylanguagesortoperatorhtml"></a><span data-ttu-id="0d43c-129">[Horní](https://docs.loganalytics.io/queryLanguage/query_language_topoperator.html) a [řazení](https://docs.loganalytics.io/queryLanguage/query_language_sortoperator.html)</span><span class="sxs-lookup"><span data-stu-id="0d43c-129">[Top](https://docs.loganalytics.io/queryLanguage/query_language_topoperator.html) and [sort](https://docs.loganalytics.io/queryLanguage/query_language_sortoperator.html)</span></span>
<span data-ttu-id="0d43c-130">`take`je užitečné tooget rychlé ukázka výsledků, ale zobrazuje řádky z tabulky hello seřazeny.</span><span class="sxs-lookup"><span data-stu-id="0d43c-130">`take` is useful tooget a quick sample of a result, but it shows rows from hello table in no particular order.</span></span> <span data-ttu-id="0d43c-131">použít tooget seřazené zobrazení, `top` (pro ukázku) nebo `sort` (přes hello celé tabulky).</span><span class="sxs-lookup"><span data-stu-id="0d43c-131">tooget an ordered view, use `top` (for a sample) or `sort` (over hello whole table).</span></span>

<span data-ttu-id="0d43c-132">Zobrazte hello první n řádky, seřazené podle konkrétního sloupce:</span><span class="sxs-lookup"><span data-stu-id="0d43c-132">Show me hello first n rows, ordered by a particular column:</span></span>

```AIQL

    requests | top 10 by timestamp desc
```

* <span data-ttu-id="0d43c-133">*Syntaxe:* většina operátory mít parametry – klíčové slovo jako `by`.</span><span class="sxs-lookup"><span data-stu-id="0d43c-133">*Syntax:* Most operators have keyword parameters such as `by`.</span></span>
* <span data-ttu-id="0d43c-134">`desc`= sestupné řazení `asc` = vzestupně.</span><span class="sxs-lookup"><span data-stu-id="0d43c-134">`desc` = descending order, `asc` = ascending.</span></span>

![](./media/app-insights-analytics-tour/260.png)

<span data-ttu-id="0d43c-135">`top...`je další způsob původce o tom, že `sort ... | take...`.</span><span class="sxs-lookup"><span data-stu-id="0d43c-135">`top...` is a more performant way of saying `sort ... | take...`.</span></span> <span data-ttu-id="0d43c-136">Budeme mít zapsat:</span><span class="sxs-lookup"><span data-stu-id="0d43c-136">We could have written:</span></span>

```AIQL

    requests | sort by timestamp desc | take 10
```

<span data-ttu-id="0d43c-137">výsledek Hello by hello stejné, ale bude spuštěná trochu pomaleji.</span><span class="sxs-lookup"><span data-stu-id="0d43c-137">hello result would be hello same, but it would run a bit more slowly.</span></span> <span data-ttu-id="0d43c-138">(Můžete také napsat `order`, což je zástupce `sort`.)</span><span class="sxs-lookup"><span data-stu-id="0d43c-138">(You could also write `order`, which is an alias of `sort`.)</span></span>

<span data-ttu-id="0d43c-139">Hello záhlaví sloupců v tabulce zobrazení hello může být také použít toosort hello výsledky na obrazovce hello.</span><span class="sxs-lookup"><span data-stu-id="0d43c-139">hello column headers in hello table view can also be used toosort hello results on hello screen.</span></span> <span data-ttu-id="0d43c-140">Ale samozřejmě platí, pokud jste použili `take` nebo `top` tooretrieve jenom část tabulky, budete jenom změnit pořadí hello záznamy jste načíst.</span><span class="sxs-lookup"><span data-stu-id="0d43c-140">But of course, if you've used `take` or `top` tooretrieve just part of a table, you'll only re-order hello records you've retrieved.</span></span>

## <a name="wherehttpsdocsloganalyticsioquerylanguagequerylanguagewhereoperatorhtml-filtering-on-a-condition"></a><span data-ttu-id="0d43c-141">[Kde](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html): filtrování na podmínce</span><span class="sxs-lookup"><span data-stu-id="0d43c-141">[Where](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html): filtering on a condition</span></span>

<span data-ttu-id="0d43c-142">Podíváme se, jenom požadavků, které vrátil kód konkrétní výsledku:</span><span class="sxs-lookup"><span data-stu-id="0d43c-142">Let's see just requests that returned a particular result code:</span></span>

```AIQL

    requests
    | where resultCode  == "404"
    | take 10
```

![](./media/app-insights-analytics-tour/250.png)

<span data-ttu-id="0d43c-143">Hello `where` operátor má logický výraz.</span><span class="sxs-lookup"><span data-stu-id="0d43c-143">hello `where` operator takes a Boolean expression.</span></span> <span data-ttu-id="0d43c-144">Zde jsou některé klíčové body o nich:</span><span class="sxs-lookup"><span data-stu-id="0d43c-144">Here are some key points about them:</span></span>

* <span data-ttu-id="0d43c-145">`and`, `or`: Logické operátory</span><span class="sxs-lookup"><span data-stu-id="0d43c-145">`and`, `or`: Boolean operators</span></span>
* <span data-ttu-id="0d43c-146">`==`, `<>`, `!=` : a nejsou rovny.</span><span class="sxs-lookup"><span data-stu-id="0d43c-146">`==`, `<>`, `!=` : equal and not equal</span></span>
* <span data-ttu-id="0d43c-147">`=~`, `!~` : velká a malá písmena řetězec stejná a není rovno.</span><span class="sxs-lookup"><span data-stu-id="0d43c-147">`=~`, `!~` : case-insensitive string equal and not equal.</span></span> <span data-ttu-id="0d43c-148">Existují mnoha další operátory porovnání řetězce.</span><span class="sxs-lookup"><span data-stu-id="0d43c-148">There are lots more string comparison operators.</span></span>

<!---Read all about [scalar expressions]().--->

### <a name="getting-hello-right-type"></a><span data-ttu-id="0d43c-149">Získávání hello správný typ.</span><span class="sxs-lookup"><span data-stu-id="0d43c-149">Getting hello right type</span></span>
<span data-ttu-id="0d43c-150">Vyhledá neúspěšné požadavky:</span><span class="sxs-lookup"><span data-stu-id="0d43c-150">Find unsuccessful requests:</span></span>

```AIQL

    requests
    | where isnotempty(resultCode) and toint(resultCode) >= 400
```
<!---
`resultCode` has type string, so we must cast it app-insights-analytics-reference.md#casts for a numeric comparison.
--->

## <a name="time"></a><span data-ttu-id="0d43c-151">Čas</span><span class="sxs-lookup"><span data-stu-id="0d43c-151">Time</span></span>

<span data-ttu-id="0d43c-152">Vaše dotazy nejsou ve výchozím nastavení s omezeným přístupem toohello posledních 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="0d43c-152">By default, your queries are restricted toohello last 24 hours.</span></span> <span data-ttu-id="0d43c-153">Můžete však změnit tento rozsah:</span><span class="sxs-lookup"><span data-stu-id="0d43c-153">But you can change this range:</span></span>

![](./media/app-insights-analytics-tour/change-time-range.png)

<span data-ttu-id="0d43c-154">Přepsat hello časový rozsah tak, že zápis jakýkoli dotaz, který uvádí `timestamp` v klauzuli where.</span><span class="sxs-lookup"><span data-stu-id="0d43c-154">Override hello time range by writing any query that mentions `timestamp` in a where-clause.</span></span> <span data-ttu-id="0d43c-155">Například:</span><span class="sxs-lookup"><span data-stu-id="0d43c-155">For example:</span></span>

```AIQL

    // What were hello slowest requests over hello past 3 days?
    requests
    | where timestamp > ago(3d)  // Override hello time range
    | top 5 by duration
```

<span data-ttu-id="0d43c-156">Hello čas rozsah funkce je ekvivalentní tooa 'where' klauzule vložit po každé zmínky jednoho hello zdrojové tabulky.</span><span class="sxs-lookup"><span data-stu-id="0d43c-156">hello time range feature is equivalent tooa 'where' clause inserted after each mention of one of hello source tables.</span></span>

<span data-ttu-id="0d43c-157">`ago(3d)`znamená tří dnů před.</span><span class="sxs-lookup"><span data-stu-id="0d43c-157">`ago(3d)` means 'three days ago'.</span></span> <span data-ttu-id="0d43c-158">Jiné jednotky doby zahrnují hodin (`2h`, `2.5h`), minut (`25m`) a sekund (`10s`).</span><span class="sxs-lookup"><span data-stu-id="0d43c-158">Other units of time include hours (`2h`, `2.5h`), minutes (`25m`), and seconds (`10s`).</span></span>

<span data-ttu-id="0d43c-159">Další příklady:</span><span class="sxs-lookup"><span data-stu-id="0d43c-159">Other examples:</span></span>

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

<span data-ttu-id="0d43c-160">[Dat a časů odkaz](https://docs.loganalytics.io/concepts/concepts_datatypes_datetime.html).</span><span class="sxs-lookup"><span data-stu-id="0d43c-160">[Dates and times reference](https://docs.loganalytics.io/concepts/concepts_datatypes_datetime.html).</span></span>


## <a name="projecthttpsdocsloganalyticsioquerylanguagequerylanguageprojectoperatorhtml-select-rename-and-compute-columns"></a><span data-ttu-id="0d43c-161">[Projekt](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html): vyberte, přejmenování a výpočetní sloupců</span><span class="sxs-lookup"><span data-stu-id="0d43c-161">[Project](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html): select, rename, and compute columns</span></span>
<span data-ttu-id="0d43c-162">Použití [ `project` ](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) toopick se právě hello sloupce, které chcete:</span><span class="sxs-lookup"><span data-stu-id="0d43c-162">Use [`project`](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) toopick out just hello columns you want:</span></span>

```AIQL

    requests | top 10 by timestamp desc
             | project timestamp, name, resultCode
```

![](./media/app-insights-analytics-tour/240.png)

<span data-ttu-id="0d43c-163">Také můžete přejmenovat sloupce a definovat nové:</span><span class="sxs-lookup"><span data-stu-id="0d43c-163">You can also rename columns and define new ones:</span></span>

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

* <span data-ttu-id="0d43c-165">Názvy sloupců může obsahovat mezery nebo symboly v případě jejich jsou v závorkách, jako to: `['...']` nebo`["..."]`</span><span class="sxs-lookup"><span data-stu-id="0d43c-165">Column names can include spaces or symbols if they are bracketed like this: `['...']` or `["..."]`</span></span>
* <span data-ttu-id="0d43c-166">`%`je obvyklé Operátor modulo hello.</span><span class="sxs-lookup"><span data-stu-id="0d43c-166">`%` is hello usual modulo operator.</span></span>
* <span data-ttu-id="0d43c-167">`1d`(která je číslice, pak měl ') je časový interval literálu znamená jeden den.</span><span class="sxs-lookup"><span data-stu-id="0d43c-167">`1d` (that's a digit one, then a 'd') is a timespan literal meaning one day.</span></span> <span data-ttu-id="0d43c-168">Tady jsou některé další literály časový interval: `12h`, `30m`, `10s`, `0.01s`.</span><span class="sxs-lookup"><span data-stu-id="0d43c-168">Here are some more timespan literals: `12h`, `30m`, `10s`, `0.01s`.</span></span>
* <span data-ttu-id="0d43c-169">`floor`(alias `bin`) zaokrouhlí dolů toohello nejbližší násobek hello základní hodnoty můžete zadat hodnotu.</span><span class="sxs-lookup"><span data-stu-id="0d43c-169">`floor` (alias `bin`) rounds a value down toohello nearest multiple of hello base value you provide.</span></span> <span data-ttu-id="0d43c-170">Proto `floor(aTime, 1s)` zaokrouhlí na dobu mimo provoz toohello nejbližší sekundu.</span><span class="sxs-lookup"><span data-stu-id="0d43c-170">So `floor(aTime, 1s)` rounds a time down toohello nearest second.</span></span>

<span data-ttu-id="0d43c-171">Výrazy může zahrnovat všechny hello běžných operátorů (`+`, `-`,...), a rozsah užitečné funkce.</span><span class="sxs-lookup"><span data-stu-id="0d43c-171">Expressions can include all hello usual operators (`+`, `-`, ...), and there's a range of useful functions.</span></span>

## <a name="extend"></a><span data-ttu-id="0d43c-172">Rozšíření</span><span class="sxs-lookup"><span data-stu-id="0d43c-172">Extend</span></span>
<span data-ttu-id="0d43c-173">Pokud chcete toohello sloupce tooadd existující, použijte [ `extend` ](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html):</span><span class="sxs-lookup"><span data-stu-id="0d43c-173">If you just want tooadd columns toohello existing ones, use [`extend`](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html):</span></span>

```AIQL

    requests
    | top 10 by timestamp desc
    | extend timeOfDay = floor(timestamp % 1d, 1s)
```

<span data-ttu-id="0d43c-174">Pomocí [ `extend` ](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html) je míň podrobné než [ `project` ](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) Pokud chcete, aby tookeep všechny hello existující sloupce.</span><span class="sxs-lookup"><span data-stu-id="0d43c-174">Using [`extend`](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html) is less verbose than [`project`](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) if you want tookeep all hello existing columns.</span></span>

### <a name="convert-toolocal-time"></a><span data-ttu-id="0d43c-175">Převést toolocal čas</span><span class="sxs-lookup"><span data-stu-id="0d43c-175">Convert toolocal time</span></span>

<span data-ttu-id="0d43c-176">Časová razítka jsou vždycky ve standardu UTC.</span><span class="sxs-lookup"><span data-stu-id="0d43c-176">Timestamps are always in UTC.</span></span> <span data-ttu-id="0d43c-177">Takže pokud jste na pobřeží hello Tichomoří USA a se jedná o zimní, se vám mohly líbit toto:</span><span class="sxs-lookup"><span data-stu-id="0d43c-177">So if you're on hello US Pacific coast and it's winter, you might like this:</span></span>

```AIQL

    requests
    | top 10 by timestamp desc
    | extend localTime = timestamp - 8h
```


## <a name="summarizehttpsdocsloganalyticsioquerylanguagequerylanguagesummarizeoperatorhtml-aggregate-groups-of-rows"></a><span data-ttu-id="0d43c-178">[Shrnout](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html): agregovat skupiny řádků</span><span class="sxs-lookup"><span data-stu-id="0d43c-178">[Summarize](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html): aggregate groups of rows</span></span>
<span data-ttu-id="0d43c-179">`Summarize`použije zadanou *agregační funkce* přes skupiny řádků.</span><span class="sxs-lookup"><span data-stu-id="0d43c-179">`Summarize` applies a specified *aggregation function* over groups of rows.</span></span>

<span data-ttu-id="0d43c-180">Například čas hello vaše webová aplikace přijímá požadavky tooa toorespond údajně v poli hello `duration`.</span><span class="sxs-lookup"><span data-stu-id="0d43c-180">For example, hello time your web app takes toorespond tooa request is reported in hello field `duration`.</span></span> <span data-ttu-id="0d43c-181">Podívejme se, průměrná odpovědi hello čas tooall požadavky:</span><span class="sxs-lookup"><span data-stu-id="0d43c-181">Let's see hello average response time tooall requests:</span></span>

![](./media/app-insights-analytics-tour/410.png)

<span data-ttu-id="0d43c-182">Nebo výsledek hello jsme může oddělení do požadavků odlišné názvy:</span><span class="sxs-lookup"><span data-stu-id="0d43c-182">Or we could separate hello result into requests of different names:</span></span>

![](./media/app-insights-analytics-tour/420.png)

<span data-ttu-id="0d43c-183">`Summarize`shromažďuje hello datových bodů v datovém proudu hello do skupin, pro které hello `by` klauzule vyhodnotí stejně.</span><span class="sxs-lookup"><span data-stu-id="0d43c-183">`Summarize` collects hello data points in hello stream into groups for which hello `by` clause evaluates equally.</span></span> <span data-ttu-id="0d43c-184">Každá hodnota v hello `by` výrazu - každý je název operace v hello výše příklad - výsledkem řádek v tabulce výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="0d43c-184">Each value in hello `by` expression - each operation name in hello above example - results in a row in hello result table.</span></span>

<span data-ttu-id="0d43c-185">Nebo jsme může seskupení výsledků podle denní dobu:</span><span class="sxs-lookup"><span data-stu-id="0d43c-185">Or we could group results by time of day:</span></span>

![](./media/app-insights-analytics-tour/430.png)

<span data-ttu-id="0d43c-186">Všimněte si, jak používáme hello `bin` – funkce (neboli `floor`).</span><span class="sxs-lookup"><span data-stu-id="0d43c-186">Notice how we're using hello `bin` function (aka `floor`).</span></span> <span data-ttu-id="0d43c-187">Pokud jsme použili `by timestamp`, každý řádek vstupu by skončit ve své vlastní malé skupiny.</span><span class="sxs-lookup"><span data-stu-id="0d43c-187">If we just used `by timestamp`, every input row would end up in its own little group.</span></span> <span data-ttu-id="0d43c-188">Pro všechny průběžné skalárních například časy nebo čísla, máme toobreak hello průběžné rozsah do spravovat počet jednotlivých hodnot.</span><span class="sxs-lookup"><span data-stu-id="0d43c-188">For any continuous scalar like times or numbers, we have toobreak hello continuous range into a manageable number of discrete values.</span></span> <span data-ttu-id="0d43c-189">`bin`-který je právě hello známé zaokrouhlení rozbalovací `floor` funkce – je nejjednodušší způsob, jak toodo hello který.</span><span class="sxs-lookup"><span data-stu-id="0d43c-189">`bin` - which is just hello familiar rounding-down `floor` function - is hello easiest way toodo that.</span></span>

<span data-ttu-id="0d43c-190">Můžeme použít hello stejné rozsahy tooreduce technika řetězců:</span><span class="sxs-lookup"><span data-stu-id="0d43c-190">We can use hello same technique tooreduce ranges of strings:</span></span>

![](./media/app-insights-analytics-tour/440.png)

<span data-ttu-id="0d43c-191">Všimněte si, že můžete použít `name=` tooset hello název sloupec výsledků, buď ve výrazech agregace hello nebo hello pomocí klauzule.</span><span class="sxs-lookup"><span data-stu-id="0d43c-191">Notice that you can use `name=` tooset hello name of a result column, either in hello aggregation expressions or hello by-clause.</span></span>

## <a name="counting-sampled-data"></a><span data-ttu-id="0d43c-192">Počítání vzorků dat</span><span class="sxs-lookup"><span data-stu-id="0d43c-192">Counting sampled data</span></span>
<span data-ttu-id="0d43c-193">`sum(itemCount)`je hello doporučená agregace toocount události.</span><span class="sxs-lookup"><span data-stu-id="0d43c-193">`sum(itemCount)` is hello recommended aggregation toocount events.</span></span> <span data-ttu-id="0d43c-194">V mnoha případech itemCount = 1, takže hello funkce jednoduše počty až hello počet řádků ve skupině hello.</span><span class="sxs-lookup"><span data-stu-id="0d43c-194">In many cases, itemCount==1, so hello function simply counts up hello number of rows in hello group.</span></span> <span data-ttu-id="0d43c-195">Ale když [vzorkování](app-insights-sampling.md) je v operaci pouze část původní události hello zachová jako datových bodů ve službě Application Insights, aby byl pro každý datový bod, uvidíte, `itemCount` události.</span><span class="sxs-lookup"><span data-stu-id="0d43c-195">But when [sampling](app-insights-sampling.md) is in operation, only a fraction of hello original events are retained as data points in Application Insights, so that for each data point you see, there are `itemCount` events.</span></span>

<span data-ttu-id="0d43c-196">Například pokud vzorkování zahodí 75 % hello původní události, pak se itemCount == 4 v záznamech hello uchovávají – to znamená, pro každý záznam zachované nebyly čtyři původní záznamy.</span><span class="sxs-lookup"><span data-stu-id="0d43c-196">For example, if sampling discards 75% of hello original events, then itemCount==4 in hello retained records - that is, for every retained record, there were four original records.</span></span>

<span data-ttu-id="0d43c-197">Adaptivního vzorkování způsobí, že itemCount toobe vyšší během období, kdy aplikace je se nejčastěji používá.</span><span class="sxs-lookup"><span data-stu-id="0d43c-197">Adaptive sampling causes itemCount toobe higher during periods when your application is being heavily used.</span></span>

<span data-ttu-id="0d43c-198">Shrnutí itemCount proto poskytuje dobrý odhad hello původní počet událostí.</span><span class="sxs-lookup"><span data-stu-id="0d43c-198">Summing up itemCount therefore gives a good estimate of hello original number of events.</span></span>

![](./media/app-insights-analytics-tour/510.png)

<span data-ttu-id="0d43c-199">K dispozici je také `count()` agregace (a počet operaci) pro případy, kdy Opravdu chcete toocount hello počet řádků ve skupině.</span><span class="sxs-lookup"><span data-stu-id="0d43c-199">There's also a `count()` aggregation (and a count operation), for cases where you really do want toocount hello number of rows in a group.</span></span>

<span data-ttu-id="0d43c-200">Je rozsah [funkce agregace](https://docs.loganalytics.io/learn/tutorials/aggregations.html).</span><span class="sxs-lookup"><span data-stu-id="0d43c-200">There's a range of [aggregation functions](https://docs.loganalytics.io/learn/tutorials/aggregations.html).</span></span>

## <a name="charting-hello-results"></a><span data-ttu-id="0d43c-201">Vytváření grafů hello výsledky</span><span class="sxs-lookup"><span data-stu-id="0d43c-201">Charting hello results</span></span>
```AIQL

    exceptions
       | summarize count=sum(itemCount)  
         by bin(timestamp, 1h)
```

<span data-ttu-id="0d43c-202">Ve výchozím nastavení zobrazí výsledky jako tabulku:</span><span class="sxs-lookup"><span data-stu-id="0d43c-202">By default, results display as a table:</span></span>

![](./media/app-insights-analytics-tour/225.png)

<span data-ttu-id="0d43c-203">Můžeme udělat lépe než hello tabulka zobrazení.</span><span class="sxs-lookup"><span data-stu-id="0d43c-203">We can do better than hello table view.</span></span> <span data-ttu-id="0d43c-204">Podívejme se na výsledky hello v zobrazení grafu hello s možností svislá čára hello:</span><span class="sxs-lookup"><span data-stu-id="0d43c-204">Let's look at hello results in hello chart view with hello vertical bar option:</span></span>

![Klikněte na graf, vyberte svislý pruhový graf a přiřadit x a y osy](./media/app-insights-analytics-tour/230.png)

<span data-ttu-id="0d43c-206">Všimněte si, že i když jsme nebyla hello výsledky seřadit podle času (jak je vidět v zobrazení tabulky hello), zobrazení grafu hello vždy zobrazuje data a času ve správném pořadí.</span><span class="sxs-lookup"><span data-stu-id="0d43c-206">Notice that although we didn't sort hello results by time (as you can see in hello table display), hello chart display always shows datetimes in correct order.</span></span>


## <a name="timecharts"></a><span data-ttu-id="0d43c-207">Timecharts</span><span class="sxs-lookup"><span data-stu-id="0d43c-207">Timecharts</span></span>
<span data-ttu-id="0d43c-208">Zobrazit, kolik události existuje se každou hodinu:</span><span class="sxs-lookup"><span data-stu-id="0d43c-208">Show how many events there are each hour:</span></span>

```AIQL

    requests
      | summarize event_count=sum(itemCount)
        by bin(timestamp, 1h)
```

<span data-ttu-id="0d43c-209">Vyberte možnost zobrazení grafu hello:</span><span class="sxs-lookup"><span data-stu-id="0d43c-209">Select hello Chart display option:</span></span>

![timechart](./media/app-insights-analytics-tour/080.png)

## <a name="multiple-series"></a><span data-ttu-id="0d43c-211">Více řad</span><span class="sxs-lookup"><span data-stu-id="0d43c-211">Multiple series</span></span>
<span data-ttu-id="0d43c-212">Více výrazů v hello `summarize` klauzule vytvoří více sloupců.</span><span class="sxs-lookup"><span data-stu-id="0d43c-212">Multiple expressions in hello `summarize` clause creates multiple columns.</span></span>

<span data-ttu-id="0d43c-213">Více výrazů v hello `by` klauzule vytvoří více řádků, jeden pro každou kombinaci hodnot.</span><span class="sxs-lookup"><span data-stu-id="0d43c-213">Multiple expressions in hello `by` clause creates multiple rows, one for each combination of values.</span></span>

```AIQL

    requests
    | summarize count_=sum(itemCount), avg(duration)
      by bin(timestamp, 1h), client_StateOrProvince, client_City
    | order by timestamp asc, client_StateOrProvince, client_City
```

![Tabulka požadavků podle hodin a umístění](./media/app-insights-analytics-tour/090.png)

### <a name="segment-a-chart-by-dimensions"></a><span data-ttu-id="0d43c-215">Rozdělit grafu pomocí dimenzí</span><span class="sxs-lookup"><span data-stu-id="0d43c-215">Segment a chart by dimensions</span></span>
<span data-ttu-id="0d43c-216">Pokud graf tabulku, která má sloupec řetězce a je číselný sloupec hello řetězec lze použít toosplit hello číselná data do samostatné řady bodů.</span><span class="sxs-lookup"><span data-stu-id="0d43c-216">If you chart a table that has a string column and a numeric column, hello string can be used toosplit hello numeric data into separate series of points.</span></span> <span data-ttu-id="0d43c-217">Pokud existuje více než jeden sloupec řetězec, můžete jako hello diskriminátoru které toouse sloupce.</span><span class="sxs-lookup"><span data-stu-id="0d43c-217">If there's more than one string column, you can choose which column toouse as hello discriminator.</span></span>

![Segment graf analýzy](./media/app-insights-analytics-tour/100.png)

#### <a name="bounce-rate"></a><span data-ttu-id="0d43c-219">Odraz rychlost</span><span class="sxs-lookup"><span data-stu-id="0d43c-219">Bounce rate</span></span>

<span data-ttu-id="0d43c-220">Převést řetězec toouse boolean tooa jej jako diskriminátoru:</span><span class="sxs-lookup"><span data-stu-id="0d43c-220">Convert a boolean tooa string toouse it as a discriminator:</span></span>

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

### <a name="display-multiple-metrics"></a><span data-ttu-id="0d43c-221">Zobrazení více metriky</span><span class="sxs-lookup"><span data-stu-id="0d43c-221">Display multiple metrics</span></span>
<span data-ttu-id="0d43c-222">Pokud jste grafu tabulku, která má více než jeden číselný sloupec v přidání toohello časové razítko, můžete zobrazit libovolnou kombinací.</span><span class="sxs-lookup"><span data-stu-id="0d43c-222">If you chart a table that has more than one numeric column, in addition toohello timestamp, you can display any combination of them.</span></span>

![Segment graf analýzy](./media/app-insights-analytics-tour/110.png)

<span data-ttu-id="0d43c-224">Je nutné vybrat **nemáte rozdělení** mohli vybrat více číselné sloupce.</span><span class="sxs-lookup"><span data-stu-id="0d43c-224">You must select **Don't Split** before you can select multiple numeric columns.</span></span> <span data-ttu-id="0d43c-225">Nelze rozdělit podle sloupce řetězec v hello stejný čas jako zobrazení více než jeden číselný sloupec.</span><span class="sxs-lookup"><span data-stu-id="0d43c-225">You can't split by a string column at hello same time as displaying more than one numeric column.</span></span>

## <a name="daily-average-cycle"></a><span data-ttu-id="0d43c-226">Průměrný denní cyklu</span><span class="sxs-lookup"><span data-stu-id="0d43c-226">Daily average cycle</span></span>
<span data-ttu-id="0d43c-227">Jak se liší využití přes hello průměrného dne?</span><span class="sxs-lookup"><span data-stu-id="0d43c-227">How does usage vary over hello average day?</span></span>

<span data-ttu-id="0d43c-228">Počet požadavků podle času hello modulo jeden den, binned do hodiny:</span><span class="sxs-lookup"><span data-stu-id="0d43c-228">Count requests by hello time modulo one day, binned into hours:</span></span>

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
> <span data-ttu-id="0d43c-230">Všimněte si, že se aktuálně musí tooconvert čas doby trvání toodatetimes v pořadí toodisplay na spojnicový graf.</span><span class="sxs-lookup"><span data-stu-id="0d43c-230">Notice that we currently have tooconvert time durations toodatetimes in order toodisplay on a line chart.</span></span>
>
>

## <a name="compare-multiple-daily-series"></a><span data-ttu-id="0d43c-231">Porovnání více denní řad</span><span class="sxs-lookup"><span data-stu-id="0d43c-231">Compare multiple daily series</span></span>
<span data-ttu-id="0d43c-232">Jak využití liší časem hello dne v různých zemí?</span><span class="sxs-lookup"><span data-stu-id="0d43c-232">How does usage vary over hello time of day in different countries?</span></span>

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

## <a name="plot-a-distribution"></a><span data-ttu-id="0d43c-234">Vykreslení distribuce</span><span class="sxs-lookup"><span data-stu-id="0d43c-234">Plot a distribution</span></span>
<span data-ttu-id="0d43c-235">Kolik relací dochází z různých délek?</span><span class="sxs-lookup"><span data-stu-id="0d43c-235">How many sessions are there of different lengths?</span></span>

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

<span data-ttu-id="0d43c-236">poslední řádek Hello je požadovaná tooconvert toodatetime.</span><span class="sxs-lookup"><span data-stu-id="0d43c-236">hello last line is required tooconvert toodatetime.</span></span> <span data-ttu-id="0d43c-237">Aktuálně hello x osy grafu se zobrazuje jako skalární hodnota, pouze pokud je hodnota datetime.</span><span class="sxs-lookup"><span data-stu-id="0d43c-237">Currently hello x axis of a chart is displayed as a scalar only if it is a datetime.</span></span>

<span data-ttu-id="0d43c-238">Hello `where` klauzule vyloučí jednorázové relací (sessionDuration == 0) a nastaví hello délce osy x hello.</span><span class="sxs-lookup"><span data-stu-id="0d43c-238">hello `where` clause excludes one-shot sessions (sessionDuration==0) and sets hello length of hello x-axis.</span></span>

![](./media/app-insights-analytics-tour/290.png)

## <a name="percentileshttpsdocsloganalyticsioquerylanguagequerylanguagepercentilesaggfunctionhtml"></a>[<span data-ttu-id="0d43c-239">Percentily</span><span class="sxs-lookup"><span data-stu-id="0d43c-239">Percentiles</span></span>](https://docs.loganalytics.io/queryLanguage/query_language_percentiles_aggfunction.html)
<span data-ttu-id="0d43c-240">Jaké rozsahy doby trvání zahrnují různá procenta relace?</span><span class="sxs-lookup"><span data-stu-id="0d43c-240">What ranges of durations cover different percentages of sessions?</span></span>

<span data-ttu-id="0d43c-241">Použít hello výše dotazu, ale nahraďte hello poslední řádek:</span><span class="sxs-lookup"><span data-stu-id="0d43c-241">Use hello above query, but replace hello last line:</span></span>

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

<span data-ttu-id="0d43c-242">Také jsme odebrali hello horní meze v hello kde klauzuli, v pořadí tooget opravte údaje včetně všechny relace s více než jeden požadavek:</span><span class="sxs-lookup"><span data-stu-id="0d43c-242">We also removed hello upper limit in hello where clause, in order tooget correct figures including all sessions with more than one request:</span></span>

![výsledek](./media/app-insights-analytics-tour/180.png)

<span data-ttu-id="0d43c-244">Odkud jsme uvidíte, že:</span><span class="sxs-lookup"><span data-stu-id="0d43c-244">From which we can see that:</span></span>

* <span data-ttu-id="0d43c-245">5 % relací být méně než tři minuty 34s;</span><span class="sxs-lookup"><span data-stu-id="0d43c-245">5% of sessions have a duration of less than 3 minutes 34s;</span></span>
* <span data-ttu-id="0d43c-246">50 % relací poslední nejvýše 36 minut;</span><span class="sxs-lookup"><span data-stu-id="0d43c-246">50% of sessions last less than 36 minutes;</span></span>
* <span data-ttu-id="0d43c-247">5 % relací poslední více než 7 dní</span><span class="sxs-lookup"><span data-stu-id="0d43c-247">5% of sessions last more than 7 days</span></span>

<span data-ttu-id="0d43c-248">tooget samostatné rozpis pro každou zemi, jsme právě mít toobring hello client_CountryOrRegion sloupec samostatně prostřednictvím obě shrnout operátory:</span><span class="sxs-lookup"><span data-stu-id="0d43c-248">tooget a separate breakdown for each country, we just have toobring hello client_CountryOrRegion column separately through both summarize operators:</span></span>

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

## <a name="join"></a><span data-ttu-id="0d43c-249">Spojit</span><span class="sxs-lookup"><span data-stu-id="0d43c-249">Join</span></span>
<span data-ttu-id="0d43c-250">Máme přístup tooseveral tabulky, včetně požadavky a výjimkami.</span><span class="sxs-lookup"><span data-stu-id="0d43c-250">We have access tooseveral tables, including requests and exceptions.</span></span>

<span data-ttu-id="0d43c-251">toofind hello výjimky související tooa požadavek, který vrátil neplatnou odpověď, jsme se můžete zapojit do tabulky hello na `session_Id`:</span><span class="sxs-lookup"><span data-stu-id="0d43c-251">toofind hello exceptions related tooa request that returned a failure response, we can join hello tables on `session_Id`:</span></span>

```AIQL

    requests
    | where toint(resultCode) >= 500
    | join (exceptions) on operation_Id
    | take 30
```


<span data-ttu-id="0d43c-252">Je dobrým zvykem toouse `project` tooselect jenom hello sloupce je třeba před provedením hello spojení.</span><span class="sxs-lookup"><span data-stu-id="0d43c-252">It's good practice toouse `project` tooselect just hello columns we need before performing hello join.</span></span>
<span data-ttu-id="0d43c-253">V hello stejné klauzule jsme přejmenovat sloupec časového razítka hello.</span><span class="sxs-lookup"><span data-stu-id="0d43c-253">In hello same clauses, we rename hello timestamp column.</span></span>

## <a name="lethttpsdocsloganalyticsioquerylanguagequerylanguageletstatementhtml-assign-a-result-tooa-variable"></a><span data-ttu-id="0d43c-254">[Umožní](https://docs.loganalytics.io/queryLanguage/query_language_letstatement.html): přiřaďte proměnnou tooa výsledek</span><span class="sxs-lookup"><span data-stu-id="0d43c-254">[Let](https://docs.loganalytics.io/queryLanguage/query_language_letstatement.html): Assign a result tooa variable</span></span>

<span data-ttu-id="0d43c-255">Použití `let` tooseparate si části hello hello předchozí výrazu.</span><span class="sxs-lookup"><span data-stu-id="0d43c-255">Use `let` tooseparate out hello parts of hello previous expression.</span></span> <span data-ttu-id="0d43c-256">Hello výsledky jsou stejné jako:</span><span class="sxs-lookup"><span data-stu-id="0d43c-256">hello results are unchanged:</span></span>

```AIQL

    let bad_requests =
      requests
        | where  toint(resultCode) >= 500  ;
    bad_requests
    | join (exceptions) on session_Id
    | take 30
```

> [!Tip] 
> <span data-ttu-id="0d43c-257">V klientovi hello analýzy, umístěte prázdné řádky mezi částí hello hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="0d43c-257">In hello Analytics client, don't put blank lines between hello parts of hello query.</span></span> <span data-ttu-id="0d43c-258">Zkontrolujte že tooexecute všechny.</span><span class="sxs-lookup"><span data-stu-id="0d43c-258">Make sure tooexecute all of it.</span></span>
>

<span data-ttu-id="0d43c-259">Použití `toscalar` tooconvert tooa hodnotu buňky jedné tabulky:</span><span class="sxs-lookup"><span data-stu-id="0d43c-259">Use `toscalar` tooconvert a single table cell tooa value:</span></span>

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


### <a name="functions"></a><span data-ttu-id="0d43c-260">Funkce</span><span class="sxs-lookup"><span data-stu-id="0d43c-260">Functions</span></span>

<span data-ttu-id="0d43c-261">Použití *umožní* toodefine funkce:</span><span class="sxs-lookup"><span data-stu-id="0d43c-261">Use *Let* toodefine a function:</span></span>

```AIQL

    let usdate = (t:datetime)
    {
      strcat(getmonth(t), "/", dayofmonth(t),"/", getyear(t), " ",
      bin((t-1h)%12h+1h,1s), iff(t%24h<12h, "AM", "PM"))
    };
    requests  
    | extend PST = usdate(timestamp-8h)
```

## <a name="accessing-nested-objects"></a><span data-ttu-id="0d43c-262">Přístup k vnořených objektů</span><span class="sxs-lookup"><span data-stu-id="0d43c-262">Accessing nested objects</span></span>
<span data-ttu-id="0d43c-263">Vnořených objektů lze snadno přistupovat.</span><span class="sxs-lookup"><span data-stu-id="0d43c-263">Nested objects can be accessed easily.</span></span> <span data-ttu-id="0d43c-264">Například v datovém proudu výjimky hello vidíte strukturovaných objekty takto:</span><span class="sxs-lookup"><span data-stu-id="0d43c-264">For example, in hello exceptions stream you can see structured objects like this:</span></span>

![výsledek](./media/app-insights-analytics-tour/520.png)

<span data-ttu-id="0d43c-266">Můžete ho vyrovnání výběrem hello vlastnosti, které vás zajímají:</span><span class="sxs-lookup"><span data-stu-id="0d43c-266">You can flatten it by choosing hello properties you're interested in:</span></span>

```AIQL

    exceptions | take 10
    | extend method1 = tostring(details[0].parsedStack[1].method)
```

<span data-ttu-id="0d43c-267">Všimněte si, že je potřeba toocast hello výsledek toohello příslušného typu.</span><span class="sxs-lookup"><span data-stu-id="0d43c-267">Note that you need toocast hello result toohello appropriate type.</span></span>


## <a name="custom-properties-and-measurements"></a><span data-ttu-id="0d43c-268">Vlastní vlastnosti a měření</span><span class="sxs-lookup"><span data-stu-id="0d43c-268">Custom properties and measurements</span></span>
<span data-ttu-id="0d43c-269">Pokud vaše aplikace připojí [vlastní dimenze (Vlastnosti) a vlastní měření](app-insights-api-custom-events-metrics.md#properties) tooevents, pak bude zobrazovat v hello `customDimensions` a `customMeasurements` objekty.</span><span class="sxs-lookup"><span data-stu-id="0d43c-269">If your application attaches [custom dimensions (properties) and custom measurements](app-insights-api-custom-events-metrics.md#properties) tooevents, then you will see them in hello `customDimensions` and `customMeasurements` objects.</span></span>

<span data-ttu-id="0d43c-270">Například pokud vaše aplikace obsahuje:</span><span class="sxs-lookup"><span data-stu-id="0d43c-270">For example, if your app includes:</span></span>

```C#

    var dimensions = new Dictionary<string, string>
                     {{"p1", "v1"},{"p2", "v2"}};
    var measurements = new Dictionary<string, double>
                     {{"m1", 42.0}, {"m2", 43.2}};
    telemetryClient.TrackEvent("myEvent", dimensions, measurements);
```

<span data-ttu-id="0d43c-271">tooextract v Analytics tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="0d43c-271">tooextract these values in Analytics:</span></span>

```AIQL

    customEvents
    | extend p1 = customDimensions.p1,
      m1 = todouble(customMeasurements.m1) // cast tooexpected type

```

<span data-ttu-id="0d43c-272">tooverify jestli vlastní dimenze je určitého typu:</span><span class="sxs-lookup"><span data-stu-id="0d43c-272">tooverify whether a custom dimension is of a particular type:</span></span>

```AIQL

    customEvents
    | extend p1 = customDimensions.p1,
      iff(notnull(todouble(customMeasurements.m1)), ...
```

## <a name="dashboards"></a><span data-ttu-id="0d43c-273">Řídicí panely</span><span class="sxs-lookup"><span data-stu-id="0d43c-273">Dashboards</span></span>
<span data-ttu-id="0d43c-274">Řídicí panel tooa výsledky v pořadí toobring budete moct připnout společně všechna vaše nejdůležitější grafů a tabulek.</span><span class="sxs-lookup"><span data-stu-id="0d43c-274">You can pin your results tooa dashboard in order toobring together all your most important charts and tables.</span></span>

* <span data-ttu-id="0d43c-275">[Řídicí panel Azure sdílené](app-insights-dashboards.md#share-dashboards): klikněte na ikonu Připnutí hello.</span><span class="sxs-lookup"><span data-stu-id="0d43c-275">[Azure shared dashboard](app-insights-dashboards.md#share-dashboards): Click hello pin icon.</span></span> <span data-ttu-id="0d43c-276">Než to uděláte, musíte mít sdílené řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="0d43c-276">Before you do this, you must have a shared dashboard.</span></span> <span data-ttu-id="0d43c-277">Otevřete hello portálu Azure, nebo vytvořit řídicí panel a klikněte na sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="0d43c-277">In hello Azure portal, open or create a dashboard and click Share.</span></span>
* <span data-ttu-id="0d43c-278">[Řídicí panel Power BI](app-insights-export-power-bi.md): klikněte na exportovat, Power BI dotazu.</span><span class="sxs-lookup"><span data-stu-id="0d43c-278">[Power BI dashboard](app-insights-export-power-bi.md): Click Export, Power BI Query.</span></span> <span data-ttu-id="0d43c-279">Výhodou této alternativní je, že můžete zobrazit dotaz spolu s další výsledky z široké škály zdrojů.</span><span class="sxs-lookup"><span data-stu-id="0d43c-279">An advantage of this alternative is that you can display your query alongside other results from a wide range of sources.</span></span>

## <a name="combine-with-imported-data"></a><span data-ttu-id="0d43c-280">Kombinovat s importovanými daty</span><span class="sxs-lookup"><span data-stu-id="0d43c-280">Combine with imported data</span></span>

<span data-ttu-id="0d43c-281">Podívejte se na řídicím panelu hello skvělé sestavy analýzy, ale občas můžete chtít tootranslate hello data tooa další stravitelné formuláře.</span><span class="sxs-lookup"><span data-stu-id="0d43c-281">Analytics reports look great on hello dashboard, but sometimes you want tootranslate hello data tooa more digestible form.</span></span> <span data-ttu-id="0d43c-282">Předpokládejme například, že ověření uživatelé se budou identifikovat hello telemetrie podle alias.</span><span class="sxs-lookup"><span data-stu-id="0d43c-282">For example, suppose your authenticated users are identified in hello telemetry by an alias.</span></span> <span data-ttu-id="0d43c-283">Chcete tooshow jejich reálné názvy ve výsledcích.</span><span class="sxs-lookup"><span data-stu-id="0d43c-283">You'd like tooshow their real names in your results.</span></span> <span data-ttu-id="0d43c-284">toodo, budete potřebovat soubor CSV, který mapuje z hello aliasy toohello skutečné názvy.</span><span class="sxs-lookup"><span data-stu-id="0d43c-284">toodo this, you need a CSV file that maps from hello aliases toohello real names.</span></span>

<span data-ttu-id="0d43c-285">Můžete importovat soubor dat a použít ho stejně jako všechny standardní tabulek hello (požadavky, výjimky a tak dále).</span><span class="sxs-lookup"><span data-stu-id="0d43c-285">You can import a data file and use it just like any of hello standard tables (requests, exceptions, and so on).</span></span> <span data-ttu-id="0d43c-286">Buď dotaz na svůj vlastní nebo se připojte s jinou tabulkou.</span><span class="sxs-lookup"><span data-stu-id="0d43c-286">Either query it on its own, or join it with other tables.</span></span> <span data-ttu-id="0d43c-287">Například, pokud máte tabulku s názvem usermap a má sloupce `realName` a `userId`, můžete ji pomocí tootranslate hello `user_AuthenticatedId` pole telemetrická žádost hello:</span><span class="sxs-lookup"><span data-stu-id="0d43c-287">For example, if you have a table named usermap, and it has columns `realName` and `userId`, then you can use it tootranslate hello `user_AuthenticatedId` field in hello request telemetry:</span></span>

```AIQL

    requests
    | where notempty(user_AuthenticatedId)
    | project userId = user_AuthenticatedId
      // get hello realName field from hello usermap table:
    | join kind=leftouter ( usermap ) on userId
      // count transactions by name:
    | summarize count() by realName
```

<span data-ttu-id="0d43c-288">tooimport tabulky, v okně schématu hello pod **jiných zdrojů dat**, postupujte podle pokynů tooadd hello nový zdroj dat, tím, že nahrajete ukázková data.</span><span class="sxs-lookup"><span data-stu-id="0d43c-288">tooimport a table, in hello Schema blade, under **Other Data Sources**, follow hello instructions tooadd a new data source, by uploading a sample of your data.</span></span> <span data-ttu-id="0d43c-289">Pak můžete použít tuto definici tooupload tabulky.</span><span class="sxs-lookup"><span data-stu-id="0d43c-289">Then you can use this definition tooupload tables.</span></span>

<span data-ttu-id="0d43c-290">Funkce importu Hello je aktuálně ve verzi preview, zobrazí se původně odkaz "Kontaktujte nás" v části "Jiné zdroje dat."</span><span class="sxs-lookup"><span data-stu-id="0d43c-290">hello import feature is currently in preview, so you will initially see a "Contact us" link under "Other data sources."</span></span> <span data-ttu-id="0d43c-291">Pomocí této toosign do programu preview toohello a hello odkaz se nahradí klikněte tlačítko "Přidat nový zdroj dat".</span><span class="sxs-lookup"><span data-stu-id="0d43c-291">Use this toosign up toohello preview program, and hello link will then be replaced by an "Add new data source" button.</span></span>


## <a name="tables"></a><span data-ttu-id="0d43c-292">Tabulky</span><span class="sxs-lookup"><span data-stu-id="0d43c-292">Tables</span></span>
<span data-ttu-id="0d43c-293">Hello datový proud telemetrie přijaté z vaší aplikace je přístupná prostřednictvím několik tabulek.</span><span class="sxs-lookup"><span data-stu-id="0d43c-293">hello stream of telemetry received from your app is accessible through several tables.</span></span> <span data-ttu-id="0d43c-294">Hello schéma vlastnosti, které jsou k dispozici pro každou tabulku je viditelná vlevo hello okna hello.</span><span class="sxs-lookup"><span data-stu-id="0d43c-294">hello schema of properties available for each table is visible at hello left of hello window.</span></span>

### <a name="requests-table"></a><span data-ttu-id="0d43c-295">Tabulka požadavků</span><span class="sxs-lookup"><span data-stu-id="0d43c-295">Requests table</span></span>
<span data-ttu-id="0d43c-296">Počet HTTP žádostí tooyour webové aplikace a segment podle názvu stránky:</span><span class="sxs-lookup"><span data-stu-id="0d43c-296">Count HTTP requests tooyour web app and segment by page name:</span></span>

![Počet požadavků segmentované podle názvu](./media/app-insights-analytics-tour/analytics-count-requests.png)

<span data-ttu-id="0d43c-298">Vyhledá hello požadavky, které nesplní většinu:</span><span class="sxs-lookup"><span data-stu-id="0d43c-298">Find hello requests that fail most:</span></span>

![Počet požadavků segmentované podle názvu](./media/app-insights-analytics-tour/analytics-failed-requests.png)

### <a name="custom-events-table"></a><span data-ttu-id="0d43c-300">Tabulka vlastní události</span><span class="sxs-lookup"><span data-stu-id="0d43c-300">Custom events table</span></span>
<span data-ttu-id="0d43c-301">Pokud používáte [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent) toosend vlastní události si můžete přečíst je z této tabulky.</span><span class="sxs-lookup"><span data-stu-id="0d43c-301">If you use [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent) toosend your own events, you can read them from this table.</span></span>

<span data-ttu-id="0d43c-302">Podívejme příklad kde kódu aplikace obsahuje tyto řádky:</span><span class="sxs-lookup"><span data-stu-id="0d43c-302">Let's take an example where your app code contains these lines:</span></span>

```C#

    telemetry.TrackEvent("Query",
       new Dictionary<string,string> {{"query", sqlCmd}},
       new Dictionary<string,double> {
           {"retry", retryCount},
           {"querytime", totalTime}})
```

<span data-ttu-id="0d43c-303">Zobrazí hello frekvenci tyto události:</span><span class="sxs-lookup"><span data-stu-id="0d43c-303">Display hello frequency of these events:</span></span>

![Zobrazení počet vlastní události](./media/app-insights-analytics-tour/analytics-custom-events-rate.png)

<span data-ttu-id="0d43c-305">Extrahujte měření a dimenze z hello události:</span><span class="sxs-lookup"><span data-stu-id="0d43c-305">Extract measurements and dimensions from hello events:</span></span>

![Zobrazení počet vlastní události](./media/app-insights-analytics-tour/analytics-custom-events-dimensions.png)

### <a name="custom-metrics-table"></a><span data-ttu-id="0d43c-307">Vlastní metriky tabulky</span><span class="sxs-lookup"><span data-stu-id="0d43c-307">Custom metrics table</span></span>
<span data-ttu-id="0d43c-308">Pokud používáte [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) toosend vlastní hodnoty metriky zjistíte své výsledky v hello **customMetrics** datového proudu.</span><span class="sxs-lookup"><span data-stu-id="0d43c-308">If you are using [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) toosend your own metric values, you’ll find its results in hello **customMetrics** stream.</span></span> <span data-ttu-id="0d43c-309">Například:</span><span class="sxs-lookup"><span data-stu-id="0d43c-309">For example:</span></span>  

![Vlastní metriky v analytics Application Insights](./media/app-insights-analytics-tour/analytics-custom-metrics.png)

> [!NOTE]
> <span data-ttu-id="0d43c-311">V [Průzkumníku metrik](app-insights-metrics-explorer.md), všechny vlastní měření připojené tooany typ telemetrie objeví spolu v okně metriky hello společně s metriky odeslaných pomocí `TrackMetric()`.</span><span class="sxs-lookup"><span data-stu-id="0d43c-311">In [Metrics Explorer](app-insights-metrics-explorer.md), all custom measurements attached tooany type of telemetry appear together in hello metrics blade along with metrics sent using `TrackMetric()`.</span></span> <span data-ttu-id="0d43c-312">Ale v analýzy, vlastní měření jsou stále připojen typ toowhichever byly provedeny na - události nebo požadavky a tak dále - při poslal TrackMetric metrika se zobrazí v vlastní datový proud telemetrie.</span><span class="sxs-lookup"><span data-stu-id="0d43c-312">But in Analytics, custom measurements are still attached toowhichever type of telemetry they were carried on - events or requests, and so on - while metrics sent by TrackMetric appear in their own stream.</span></span>
>
>

### <a name="performance-counters-table"></a><span data-ttu-id="0d43c-313">Tabulka čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="0d43c-313">Performance counters table</span></span>
<span data-ttu-id="0d43c-314">[Čítače výkonu](app-insights-performance-counters.md) můžete zobrazit základní systémové metriky pro vaši aplikaci, například CPU, paměť a využití sítě.</span><span class="sxs-lookup"><span data-stu-id="0d43c-314">[Performance counters](app-insights-performance-counters.md) show you basic system metrics for your app, such as CPU, memory, and network utilization.</span></span> <span data-ttu-id="0d43c-315">Můžete nakonfigurovat hello SDK toosend další čítače, včetně vlastní vlastní čítače.</span><span class="sxs-lookup"><span data-stu-id="0d43c-315">You can configure hello SDK toosend additional counters, including your own custom counters.</span></span>

<span data-ttu-id="0d43c-316">Hello **čítače výkonu** schématu zpřístupní hello `category`, `counter` název, a `instance` název jednotlivých čítačů výkonu.</span><span class="sxs-lookup"><span data-stu-id="0d43c-316">hello **performanceCounters** schema exposes hello `category`, `counter` name, and `instance` name of each performance counter.</span></span> <span data-ttu-id="0d43c-317">Názvy instancí čítače jsou pouze použít toosome čítače výkonu a obvykle označují, že má název hello počtu hello toowhich proces hello vztah.</span><span class="sxs-lookup"><span data-stu-id="0d43c-317">Counter instance names are only applicable toosome performance counters, and typically indicate hello name of hello process toowhich hello count relates.</span></span> <span data-ttu-id="0d43c-318">V hello telemetrických dat pro každou aplikaci zobrazí se pouze hello čítače pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0d43c-318">In hello telemetry for each application, you’ll see only hello counters for that application.</span></span> <span data-ttu-id="0d43c-319">Například toosee čítače, které jsou k dispozici:</span><span class="sxs-lookup"><span data-stu-id="0d43c-319">For example, toosee what counters are available:</span></span>

![Čítače výkonu v analytics Application Insights](./media/app-insights-analytics-tour/analytics-performance-counters.png)

<span data-ttu-id="0d43c-321">tooget graf dostupné paměti přes hello vybrané období:</span><span class="sxs-lookup"><span data-stu-id="0d43c-321">tooget a chart of available memory over hello selected period:</span></span>

![Paměť timechart v analytics Application Insights](./media/app-insights-analytics-tour/analytics-available-memory.png)

<span data-ttu-id="0d43c-323">Jako další telemetrií **čítače výkonu** také má sloupec `cloud_RoleInstance` určující hello identitu hello hostitelský počítač, na kterém aplikace běží.</span><span class="sxs-lookup"><span data-stu-id="0d43c-323">Like other telemetry, **performanceCounters** also has a column `cloud_RoleInstance` that indicates hello identity of hello host machine on which your app is running.</span></span> <span data-ttu-id="0d43c-324">Například toocompare hello výkonu vaší aplikace na různé počítače hello:</span><span class="sxs-lookup"><span data-stu-id="0d43c-324">For example, toocompare hello performance of your app on hello different machines:</span></span>

![Výkon oddělených instance role ve službě Application Insights analytics](./media/app-insights-analytics-tour/analytics-metrics-role-instance.png)

### <a name="exceptions-table"></a><span data-ttu-id="0d43c-326">Výjimky tabulky</span><span class="sxs-lookup"><span data-stu-id="0d43c-326">Exceptions table</span></span>
<span data-ttu-id="0d43c-327">[Výjimky hlášených aplikace](app-insights-asp-net-exceptions.md) jsou k dispozici v této tabulce.</span><span class="sxs-lookup"><span data-stu-id="0d43c-327">[Exceptions reported by your app](app-insights-asp-net-exceptions.md) are available in this table.</span></span>

<span data-ttu-id="0d43c-328">toofind hello požadavek HTTP, který aplikace byla zpracování při hello došlo k výjimce, připojte na operation_Id:</span><span class="sxs-lookup"><span data-stu-id="0d43c-328">toofind hello HTTP request that your app was handling when hello exception was raised, join on operation_Id:</span></span>

![Připojení výjimky s požadavky na operation_Id](./media/app-insights-analytics-tour/analytics-exception-request.png)

### <a name="browser-timings-table"></a><span data-ttu-id="0d43c-330">Tabulka časování prohlížeče</span><span class="sxs-lookup"><span data-stu-id="0d43c-330">Browser timings table</span></span>
<span data-ttu-id="0d43c-331">`browserTimings`Zobrazuje data zatížení stránky shromažďují v prohlížečích vašich uživatelů.</span><span class="sxs-lookup"><span data-stu-id="0d43c-331">`browserTimings` shows page load data collected in your users' browsers.</span></span>

<span data-ttu-id="0d43c-332">[Nastavit aplikaci pro telemetrických dat na straně klienta](app-insights-javascript.md) v pořadí toosee tyto metriky.</span><span class="sxs-lookup"><span data-stu-id="0d43c-332">[Set up your app for client-side telemetry](app-insights-javascript.md) in order toosee these metrics.</span></span>

<span data-ttu-id="0d43c-333">zahrnuje Hello schématu [metrika označující hello délek různých fázích načítání proces stránky hello](app-insights-javascript.md#page-load-performance).</span><span class="sxs-lookup"><span data-stu-id="0d43c-333">hello schema includes [metrics indicating hello lengths of different stages of hello page loading process](app-insights-javascript.md#page-load-performance).</span></span> <span data-ttu-id="0d43c-334">(Není indikují hello časový úsek, který uživatelé přečíst na stránce.)</span><span class="sxs-lookup"><span data-stu-id="0d43c-334">(They don’t indicate hello length of time your users read a page.)</span></span>  

<span data-ttu-id="0d43c-335">Zobrazit hello popularities různých stránek a načíst časy pro jednotlivé stránky:</span><span class="sxs-lookup"><span data-stu-id="0d43c-335">Show hello popularities of different pages, and load times for each page:</span></span>

![Časů načtení stránky v Analytics](./media/app-insights-analytics-tour/analytics-page-load.png)

### <a name="availability-results-table"></a><span data-ttu-id="0d43c-337">Tabulky výsledků dostupnosti</span><span class="sxs-lookup"><span data-stu-id="0d43c-337">Availability results table</span></span>
<span data-ttu-id="0d43c-338">`availabilityResults`ukazuje hello výsledky vaše [webové testy](app-insights-monitor-web-app-availability.md).</span><span class="sxs-lookup"><span data-stu-id="0d43c-338">`availabilityResults` shows hello results of your [web tests](app-insights-monitor-web-app-availability.md).</span></span> <span data-ttu-id="0d43c-339">Každé spuštění testů z umístění každého testu se hlásí samostatně.</span><span class="sxs-lookup"><span data-stu-id="0d43c-339">Each run of your tests from each test location is reported separately.</span></span>

![Časů načtení stránky v Analytics](./media/app-insights-analytics-tour/analytics-availability.png)

### <a name="dependencies-table"></a><span data-ttu-id="0d43c-341">Tabulka závislosti</span><span class="sxs-lookup"><span data-stu-id="0d43c-341">Dependencies table</span></span>
<span data-ttu-id="0d43c-342">Obsahuje výsledky volání, aby vaše aplikace umožňuje toodatabases a rozhraní REST API a dalších volá tooTrackDependency().</span><span class="sxs-lookup"><span data-stu-id="0d43c-342">Contains results of calls that your app makes toodatabases and REST APIs, and other calls tooTrackDependency().</span></span> <span data-ttu-id="0d43c-343">Zahrnuje taky volání AJAX provedená z prohlížeče hello.</span><span class="sxs-lookup"><span data-stu-id="0d43c-343">Also includes AJAX calls made from hello browser.</span></span>

<span data-ttu-id="0d43c-344">Volání AJAX z prohlížeče hello:</span><span class="sxs-lookup"><span data-stu-id="0d43c-344">AJAX calls from hello browser:</span></span>

```AIQL

    dependencies | where client_Type == "Browser"
    | take 10
```

<span data-ttu-id="0d43c-345">Závislost volání ze serveru hello:</span><span class="sxs-lookup"><span data-stu-id="0d43c-345">Dependency calls from hello server:</span></span>

```AIQL

    dependencies | where client_Type == "PC"
    | take 10
```

<span data-ttu-id="0d43c-346">Vždy zobrazovat výsledky v závislosti na straně serveru `success==False` Pokud hello Application Insights Agent není nainstalován.</span><span class="sxs-lookup"><span data-stu-id="0d43c-346">Server-side dependency results always show `success==False` if hello Application Insights Agent is not installed.</span></span> <span data-ttu-id="0d43c-347">Ale hello další data jsou správné.</span><span class="sxs-lookup"><span data-stu-id="0d43c-347">However, hello other data are correct.</span></span>

### <a name="traces-table"></a><span data-ttu-id="0d43c-348">Tabulka trasování</span><span class="sxs-lookup"><span data-stu-id="0d43c-348">Traces table</span></span>
<span data-ttu-id="0d43c-349">Obsahuje hello telemetrické zprávy odesílané vaší aplikace pomocí TrackTrace(), nebo [jiných rozhraní protokolování](app-insights-asp-net-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="0d43c-349">Contains hello telemetry sent by your app using TrackTrace(), or [other logging frameworks](app-insights-asp-net-trace-logs.md).</span></span>

## <a name="video"></a><span data-ttu-id="0d43c-350">Video</span><span class="sxs-lookup"><span data-stu-id="0d43c-350">Video</span></span> 

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

<span data-ttu-id="0d43c-351">Pokročilé dotazy:</span><span class="sxs-lookup"><span data-stu-id="0d43c-351">Advanced queries:</span></span>

> [!VIDEO https://channel9.msdn.com/Events/Build/2016/P591/player]


## <a name="next-steps"></a><span data-ttu-id="0d43c-352">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0d43c-352">Next steps</span></span>
* [<span data-ttu-id="0d43c-353">Referenční dokumentace jazyka Analytics</span><span class="sxs-lookup"><span data-stu-id="0d43c-353">Analytics language reference</span></span>](app-insights-analytics-reference.md)
* <span data-ttu-id="0d43c-354">[SQL-uživatelů tahák](https://aka.ms/sql-analytics) překládá nejběžnější idioms hello.</span><span class="sxs-lookup"><span data-stu-id="0d43c-354">[SQL-users' cheat sheet](https://aka.ms/sql-analytics) translates hello most common idioms.</span></span>

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]
