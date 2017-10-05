---
title: "Pomocí portálu hledání protokolů v Azure Log Analytics | Microsoft Docs"
description: "Tento článek obsahuje kurz, který popisuje, jak vytvořit protokolu hledání a analyzovat data uložená v pracovním prostoru analýzy protokolů pomocí portálu hledání protokolů.  Tento kurz zahrnuje spustíte pár jednoduchých dotazů vrátit různé typy dat a analýza výsledků."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: bwren
ms.openlocfilehash: 6fc556ceb34cde26d5f3789a2397cdaa34b0b84d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-log-searches-in-azure-log-analytics-using-the-log-search-portal"></a><span data-ttu-id="08df3-104">Vytvoření protokolu hledání v Azure Log Analytics pomocí portálu hledání protokolů</span><span class="sxs-lookup"><span data-stu-id="08df3-104">Create log searches in Azure Log Analytics using the Log Search portal</span></span>

> [!NOTE]
> <span data-ttu-id="08df3-105">Tento článek popisuje portálu hledání protokolů v Azure Log Analytics jazykem nový dotaz.</span><span class="sxs-lookup"><span data-stu-id="08df3-105">This article describes the Log Search portal in Azure Log Analytics using the new query language.</span></span>  <span data-ttu-id="08df3-106">Můžete získat další informace o nový jazyk a získat postup upgradu pracovního prostoru v [upgradu pracovní prostor analýzy protokolů Azure na nové hledání protokolu](log-analytics-log-search-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="08df3-106">You can learn more about the new language and get the procedure to upgrade your workspace at [Upgrade your Azure Log Analytics workspace to new log search](log-analytics-log-search-upgrade.md).</span></span>  
>
> <span data-ttu-id="08df3-107">Pokud pracovní prostor nebyla upgradována, aby nové dotazovací jazyk, se seznamte s [najít data pomocí protokolu hledání v analýzy protokolů](log-analytics-log-searches.md) informace o aktuální verzi portálu hledání protokolů.</span><span class="sxs-lookup"><span data-stu-id="08df3-107">If your workspace hasn't been upgraded to the new query language, you should refer to [Find data using log searches in Log Analytics](log-analytics-log-searches.md) for information on the current version of the Log Search portal.</span></span>

<span data-ttu-id="08df3-108">Tento článek obsahuje kurz, který popisuje, jak vytvořit protokolu hledání a analyzovat data uložená v pracovním prostoru analýzy protokolů pomocí portálu hledání protokolů.</span><span class="sxs-lookup"><span data-stu-id="08df3-108">This article includes a tutorial that describes how to create log searches and analyze data stored in your Log Analytics workspace using the Log Search portal.</span></span>  <span data-ttu-id="08df3-109">Tento kurz zahrnuje spustíte pár jednoduchých dotazů vrátit různé typy dat a analýza výsledků.</span><span class="sxs-lookup"><span data-stu-id="08df3-109">The tutorial includes running some simple queries to return different types of data and analyzing results.</span></span>  <span data-ttu-id="08df3-110">Zaměřuje se na funkce na portálu hledání protokolů pro úpravy dotazu a nikoli změny přímo.</span><span class="sxs-lookup"><span data-stu-id="08df3-110">It focuses on features in the Log Search portal for modifying the query rather than modifying it directly.</span></span>  <span data-ttu-id="08df3-111">Podrobnosti o přímou úpravou dotazu, najdete v článku [referenční příručka jazyka dotazů](https://go.microsoft.com/fwlink/?linkid=856079).</span><span class="sxs-lookup"><span data-stu-id="08df3-111">For details on directly editing the query, see the [Query Language reference](https://go.microsoft.com/fwlink/?linkid=856079).</span></span>

<span data-ttu-id="08df3-112">Vytvoření vyhledávání v portálu pokročilé analýzy místo portálu vyhledávání protokolu naleznete v tématu [Začínáme s portálu analýza](https://go.microsoft.com/fwlink/?linkid=856587).</span><span class="sxs-lookup"><span data-stu-id="08df3-112">To create searches in the Advanced Analytics portal instead of the Log Search portal, see [Getting Started with the Analytics Portal](https://go.microsoft.com/fwlink/?linkid=856587).</span></span>  <span data-ttu-id="08df3-113">Oba Portály pomocí stejné dotazovací jazyk přístup ke stejným datům v pracovním prostoru analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="08df3-113">Both portals use the same query language to access the same data in the Log Analytics workspace.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="08df3-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="08df3-114">Prerequisites</span></span>
<span data-ttu-id="08df3-115">Tento kurz předpokládá, že už máte pracovní prostor analýzy protokolů s alespoň jeden připojený zdroj, který generuje data pro dotazy k analýze.</span><span class="sxs-lookup"><span data-stu-id="08df3-115">This tutorial assumes that you already have a Log Analytics workspace with at least one connected source that generates data for the queries to analyze.</span></span>  

- <span data-ttu-id="08df3-116">Pokud nemáte pracovní prostor, můžete vytvořit volné jeden pomocí postupu v [začít pracovat s pracovní prostor analýzy protokolů](log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="08df3-116">If you don't have a workspace, you can create a free one using the procedure at [Get started with a Log Analytics workspace](log-analytics-get-started.md).</span></span>
- <span data-ttu-id="08df3-117">Připojit aspoň jeden [agenta Windows](log-analytics-windows-agents.md) nebo jednu [agenta systému Linux](log-analytics-linux-agents.md) do pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="08df3-117">Connect least one [Windows agent](log-analytics-windows-agents.md) or one [Linux agent](log-analytics-linux-agents.md) to the workspace.</span></span>  

## <a name="open-the-log-search-portal"></a><span data-ttu-id="08df3-118">Otevřete portál hledání protokolů</span><span class="sxs-lookup"><span data-stu-id="08df3-118">Open the Log Search portal</span></span>
<span data-ttu-id="08df3-119">Začněte otevřením portálu hledání protokolů.</span><span class="sxs-lookup"><span data-stu-id="08df3-119">Start by opening the Log Search portal.</span></span>  <span data-ttu-id="08df3-120">Můžete k němu přístup v portálu Azure nebo na portálu OMS.</span><span class="sxs-lookup"><span data-stu-id="08df3-120">You can access it in either the Azure portal or the OMS portal.</span></span>

1. <span data-ttu-id="08df3-121">Otevřete portál Azure.</span><span class="sxs-lookup"><span data-stu-id="08df3-121">Open the Azure portal.</span></span>
2. <span data-ttu-id="08df3-122">Přejděte k analýze protokolů a vyberte pracovní prostor.</span><span class="sxs-lookup"><span data-stu-id="08df3-122">Navigate to Log Analytics and select your workspace.</span></span>
3. <span data-ttu-id="08df3-123">Vyberte buď **hledání protokolů** zůstat na webu Azure portal nebo spusťte na portálu OMS výběrem **portálu OMS** a kliknutím na tlačítko Hledat protokolu.</span><span class="sxs-lookup"><span data-stu-id="08df3-123">Either select **Log Search** to stay in the Azure portal or launch the OMS portal by selecting **OMS Portal** and then clicking the Log Search button.</span></span>

![Tlačítko vyhledat protokolu](media/log-analytics-log-search-log-search-portal/log-search-button.png)

## <a name="create-a-simple-search"></a><span data-ttu-id="08df3-125">Vytvoření jednoduché hledání</span><span class="sxs-lookup"><span data-stu-id="08df3-125">Create a simple search</span></span>
<span data-ttu-id="08df3-126">Nejrychlejší způsob, jak načíst některá data pro práci s je jednoduchý dotaz, který vrátí všechny záznamy v tabulce.</span><span class="sxs-lookup"><span data-stu-id="08df3-126">The quickest way to retrieve some data to work with is a simple query that returns all records in table.</span></span>  <span data-ttu-id="08df3-127">Pokud máte jakékoli klienti systému Windows nebo Linux připojení do pracovního prostoru, pak budete mít data v událostí (Windows) nebo tabulka Syslog (Linux).</span><span class="sxs-lookup"><span data-stu-id="08df3-127">If you have any Windows or Linux clients connected to your workspace, then you'll have data in either the Event (Windows) or Syslog (Linux) table.</span></span>

<span data-ttu-id="08df3-128">Zadejte jednu následující dotazy do vyhledávacího pole a klikněte na tlačítko Hledat.</span><span class="sxs-lookup"><span data-stu-id="08df3-128">Type one the following queries in the search box and click the search button.</span></span>  

```
Event
```
```
Syslog
```

<span data-ttu-id="08df3-129">Data jsou vrácena ve výchozím zobrazení seznamu a můžete zobrazit celkový počet záznamů vrácených.</span><span class="sxs-lookup"><span data-stu-id="08df3-129">Data is returned in the default list view, and you can see how many total records were returned.</span></span>

![Jednoduchý dotaz](media/log-analytics-log-search-log-search-portal/log-search-portal-01.png)

<span data-ttu-id="08df3-131">Zobrazí se pouze první několik vlastnosti každý záznam.</span><span class="sxs-lookup"><span data-stu-id="08df3-131">Only the first few properties of each record are displayed.</span></span>  <span data-ttu-id="08df3-132">Klikněte na tlačítko **zobrazit další** zobrazíte všechny vlastnosti pro konkrétní záznam.</span><span class="sxs-lookup"><span data-stu-id="08df3-132">Click **show more** to display all properties for a particular record.</span></span>

![Podrobnosti záznamu](media/log-analytics-log-search-log-search-portal/log-search-portal-02.png)

## <a name="set-the-time-scope"></a><span data-ttu-id="08df3-134">Nastavit obor čas</span><span class="sxs-lookup"><span data-stu-id="08df3-134">Set the time scope</span></span>
<span data-ttu-id="08df3-135">Každý záznam shromažďují analýzy protokolů má **TimeGenerated** vlastnost, která obsahuje datum a čas, který byl vytvořen záznam.</span><span class="sxs-lookup"><span data-stu-id="08df3-135">Every record collected by Log Analytics has a **TimeGenerated** property that contains the date and time that the record was created.</span></span>  <span data-ttu-id="08df3-136">Dotaz na portálu hledání protokolů pouze vrátí záznamy s **TimeGenerated** v rámci oboru čas, který se zobrazí na levé straně obrazovky.</span><span class="sxs-lookup"><span data-stu-id="08df3-136">A query in the Log Search portal only returns records with a **TimeGenerated** within the time scope that's displayed on the left side of the screen.</span></span>  

<span data-ttu-id="08df3-137">Filtr času můžete změnit tak, že vyberete rozevíracího seznamu nebo změnou posuvníku.</span><span class="sxs-lookup"><span data-stu-id="08df3-137">You can change the time filter either by selecting the dropdown or by modifying the slider.</span></span>  <span data-ttu-id="08df3-138">Posuvník Zobrazí pruhový graf, který se zobrazuje číslo relativní záznamů pro každý segment čas v rozsahu.</span><span class="sxs-lookup"><span data-stu-id="08df3-138">The slider displays a bar graph that shows the relative number of records for each time segment within the range.</span></span>  <span data-ttu-id="08df3-139">Tento segment se bude lišit v závislosti na rozsahu.</span><span class="sxs-lookup"><span data-stu-id="08df3-139">This segment will vary depending on the range.</span></span>

<span data-ttu-id="08df3-140">Rozsah výchozí doba je **1 den**.</span><span class="sxs-lookup"><span data-stu-id="08df3-140">The default time scope is **1 day**.</span></span>  <span data-ttu-id="08df3-141">Změna této hodnoty na **7 dní**, a celkový počet záznamů měli zvýšit.</span><span class="sxs-lookup"><span data-stu-id="08df3-141">Change this value to **7 days**, and the total number of records should increase.</span></span>

![Datum čas oboru](media/log-analytics-log-search-log-search-portal/log-search-portal-03.png)

## <a name="filter-results-of-the-query"></a><span data-ttu-id="08df3-143">Filtrování výsledků dotazu</span><span class="sxs-lookup"><span data-stu-id="08df3-143">Filter results of the query</span></span>
<span data-ttu-id="08df3-144">Na levé straně obrazovky je podokno filtru, která umožňuje filtrování chcete do dotazu přidat bez změny přímo.</span><span class="sxs-lookup"><span data-stu-id="08df3-144">On the left side of the screen is the filter pane which allows you to add filtering to the query without modifying it directly.</span></span>  <span data-ttu-id="08df3-145">Několik vlastností vrácené záznamy jsou zobrazit s jejich hodnoty prvních deset s jejich počet záznamů.</span><span class="sxs-lookup"><span data-stu-id="08df3-145">Several properties of the records returned are displayed with their top ten values with their record count.</span></span>

<span data-ttu-id="08df3-146">Pokud pracujete s **událostí**, zaškrtněte políčko vedle **chyba** pod **EVENTLEVELNAME**.</span><span class="sxs-lookup"><span data-stu-id="08df3-146">If you're working with **Event**, select the checkbox next to **Error** under **EVENTLEVELNAME**.</span></span>   <span data-ttu-id="08df3-147">Pokud pracujete s **Syslog**, zaškrtněte políčko vedle **chyba** pod **úroveň ZÁVAŽNOSTI**.</span><span class="sxs-lookup"><span data-stu-id="08df3-147">If you're working with **Syslog**, select the checkbox next to **err** under **SEVERITYLEVEL**.</span></span>  <span data-ttu-id="08df3-148">Tato operace změní dotaz na jednu z těchto omezit výsledky do chybové události.</span><span class="sxs-lookup"><span data-stu-id="08df3-148">This changes the query to one of the following to limit the results to error events.</span></span>

```
Event | where (EventLevelName == "Error")
```
```
Syslog | where (SeverityLevel == "err")
```

![Filtr](media/log-analytics-log-search-log-search-portal/log-search-portal-04.png)

<span data-ttu-id="08df3-150">Přidání vlastnosti do podokna filtru tak, že vyberete **přidat do filtry** na jeden ze záznamů v nabídce vlastnost.</span><span class="sxs-lookup"><span data-stu-id="08df3-150">Add properties to the filter pane by selecting **Add to filters** from the property menu on one of the records.</span></span>

![Přidání do nabídky filtru](media/log-analytics-log-search-log-search-portal/log-search-portal-02a.png)

<span data-ttu-id="08df3-152">Stejný filtr můžete nastavit tak, že vyberete **filtru** z nabídky vlastnost pro záznam s hodnotou, který chcete filtrovat.</span><span class="sxs-lookup"><span data-stu-id="08df3-152">You can set the same filter by selecting **Filter** from the property menu for a record with the value you want to filter.</span></span>  

<span data-ttu-id="08df3-153">Můžete mít pouze **filtru** možnost Vlastnosti s jejich název modře.</span><span class="sxs-lookup"><span data-stu-id="08df3-153">You only have the **Filter** option for properties with their name in blue.</span></span>  <span data-ttu-id="08df3-154">Jedná se o *prohledávatelné* pole, které jsou indexované pro podmínky vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="08df3-154">These are *searchable* fields which are indexed for search conditions.</span></span>  <span data-ttu-id="08df3-155">Pole šedě jsou *volné text prohledávatelné* pole, které mají jenom **zobrazit odkazy** možnost.</span><span class="sxs-lookup"><span data-stu-id="08df3-155">Fields in grey are *free text searchable* fields which only have the **Show references** option.</span></span>  <span data-ttu-id="08df3-156">Tato možnost vrátí záznamy, které mají tuto hodnotu v libovolné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="08df3-156">This option returns records that have that value in any property.</span></span>

![Filtr nabídky](media/log-analytics-log-search-log-search-portal/log-search-portal-01a.png)

<span data-ttu-id="08df3-158">Výsledky na jedinou vlastností můžete seskupovat podle výběru **Seskupit podle** možnost v nabídce záznam.</span><span class="sxs-lookup"><span data-stu-id="08df3-158">You can group the results on a single property by selecting the **Group by** option in the record menu.</span></span>  <span data-ttu-id="08df3-159">Bude přidáno [shrnout](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) operátor do dotazu, který zobrazí výsledky v grafu.</span><span class="sxs-lookup"><span data-stu-id="08df3-159">This will add a [summarize](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) operator to your query that displays the results in a chart.</span></span>  <span data-ttu-id="08df3-160">Můžete seskupit na více než jednu vlastnost, ale budete muset upravit dotaz přímo.</span><span class="sxs-lookup"><span data-stu-id="08df3-160">You can group on more than one property, but you would need to edit the query directly.</span></span>  <span data-ttu-id="08df3-161">Vyberte nabídku záznam Další **počítače** vlastnost a vyberte **Seskupit podle "Počítač"**.</span><span class="sxs-lookup"><span data-stu-id="08df3-161">Select the record menu next the the **Computer** property and select **Group by 'Computer'**.</span></span>  

![Seskupit podle počítače](media/log-analytics-log-search-log-search-portal/log-search-portal-10.png)

## <a name="work-with-results"></a><span data-ttu-id="08df3-163">Práce s výsledky</span><span class="sxs-lookup"><span data-stu-id="08df3-163">Work with results</span></span>
<span data-ttu-id="08df3-164">Portál vyhledávání protokolu obsahuje řadu funkcí pro práci s výsledky dotazu.</span><span class="sxs-lookup"><span data-stu-id="08df3-164">The Log Search portal has a variety of features for working with the results of a query.</span></span>  <span data-ttu-id="08df3-165">Můžete řadit, filtr a výsledky skupiny k analýze dat bez úpravy skutečné dotazu.</span><span class="sxs-lookup"><span data-stu-id="08df3-165">You can sort, filter, and group results to analyze the data without modifying the actual query.</span></span>  <span data-ttu-id="08df3-166">Ve výchozím nastavení nejsou seřazeny výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="08df3-166">Results of a query are not sorted by default.</span></span>

<span data-ttu-id="08df3-167">Chcete-li zobrazit data v tabulce formulář, který nabízí další možnosti pro filtrování a řazení, klikněte na tlačítko **tabulky**.</span><span class="sxs-lookup"><span data-stu-id="08df3-167">To view the data in table form which provides additional options for filtering and sorting, click **Table**.</span></span>  

![Zobrazení tabulky](media/log-analytics-log-search-log-search-portal/log-search-portal-05.png)

<span data-ttu-id="08df3-169">Klikněte na šipku podle záznam zobrazíte podrobnosti pro tento záznam.</span><span class="sxs-lookup"><span data-stu-id="08df3-169">Click the arrow by a record to view the details for that record.</span></span>

![Řazení výsledků](media/log-analytics-log-search-log-search-portal/log-search-portal-06.png)

<span data-ttu-id="08df3-171">Řazení v některém poli kliknutím na záhlaví sloupce.</span><span class="sxs-lookup"><span data-stu-id="08df3-171">Sort on any field by clicking on its column header.</span></span>

![Řazení výsledků](media/log-analytics-log-search-log-search-portal/log-search-portal-07.png)

<span data-ttu-id="08df3-173">Filtrování výsledků na konkrétní hodnotu ve sloupci klepnutím na tlačítko filtru a poskytnutím podmínku filtrování.</span><span class="sxs-lookup"><span data-stu-id="08df3-173">Filter the results on a specific value in the column by clicking the filter button and providing a filter condition.</span></span>

![Filtrování výsledků](media/log-analytics-log-search-log-search-portal/log-search-portal-08.png)

<span data-ttu-id="08df3-175">Skupiny na sloupci, přetáhněte záhlaví sloupce na začátek výsledky.</span><span class="sxs-lookup"><span data-stu-id="08df3-175">Group on a column by dragging its column header to the top of the results.</span></span>  <span data-ttu-id="08df3-176">Přetažením více sloupců do horní části můžete seskupit více polí.</span><span class="sxs-lookup"><span data-stu-id="08df3-176">You can group on multiple fields by dragging multiple columns to the top.</span></span>

![Výsledky skupiny](media/log-analytics-log-search-log-search-portal/log-search-portal-09.png)



## <a name="work-with-performance-data"></a><span data-ttu-id="08df3-178">Práce s data výkonu</span><span class="sxs-lookup"><span data-stu-id="08df3-178">Work with performance data</span></span>
<span data-ttu-id="08df3-179">Údaje o výkonu pro systém Windows a Linux agentů je uložen v prostoru analýzy protokolů **výkonu** tabulky.</span><span class="sxs-lookup"><span data-stu-id="08df3-179">Performance data for both Windows and Linux agents is stored in the Log Analytics workspace in the **Perf** table.</span></span>  <span data-ttu-id="08df3-180">Zaznamenává výkonu vypadají stejně jako jakýkoli jiný záznam a jsme může zapisovat jednoduchý dotaz, který vrátí že všechny záznamy výkonu stejně jako s událostmi.</span><span class="sxs-lookup"><span data-stu-id="08df3-180">Performance records look just like any other record, and we can write a simple query that returns all performance records just like with events.</span></span>

```
Perf
```

![Údaje o výkonu](media/log-analytics-log-search-log-search-portal/log-search-portal-11.png)

<span data-ttu-id="08df3-182">Vrácení miliony záznamů pro všechny objekty výkonu a čítače, když není velmi užitečné.</span><span class="sxs-lookup"><span data-stu-id="08df3-182">Returning millions of records for all performance objects and counters though isn't very useful.</span></span>  <span data-ttu-id="08df3-183">Můžete použít stejné metody, které jste použili výše a filtrujte data nebo právě zadejte následující dotaz přímo do vyhledávacího pole protokolu.</span><span class="sxs-lookup"><span data-stu-id="08df3-183">You can use the same methods you used above to filter the data or just type the following query directly into the log search box.</span></span>  <span data-ttu-id="08df3-184">Tento příkaz vrátí jenom procesoru záznamů využití pro počítače se systémy Windows a Linux.</span><span class="sxs-lookup"><span data-stu-id="08df3-184">This returns only processor utilization records for both Windows and Linux computers.</span></span>

```
Perf | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time")
```

![Využití procesoru](media/log-analytics-log-search-log-search-portal/log-search-portal-12.png)

<span data-ttu-id="08df3-186">Toto nastavení omezuje data na konkrétní čítače, ale je stále není pro něj formulář, který je obzvláště užitečné.</span><span class="sxs-lookup"><span data-stu-id="08df3-186">This limits the data to a particular counter, but it still doesn't put it in a form that's particularly useful.</span></span>  <span data-ttu-id="08df3-187">Můžete zobrazit data ve spojnicovém grafu, ale nejprve skupiny tak, že počítač a TimeGenerated.</span><span class="sxs-lookup"><span data-stu-id="08df3-187">You can display the data in a line chart, but first need to group it by Computer and TimeGenerated.</span></span>  <span data-ttu-id="08df3-188">K seskupení více polí, je nutné upravit dotaz přímo, takže upravit dotaz pro následující.</span><span class="sxs-lookup"><span data-stu-id="08df3-188">To group on multiple fields, you need to modify the query directly, so modify the query to the following.</span></span>  <span data-ttu-id="08df3-189">Tato služba využívá [průměr](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/avg()) na fungovat **přepočtené** vlastnost pro výpočet průměrné hodnoty přes každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="08df3-189">This uses the [avg](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/avg()) function on the **CounterValue** property to calculate the average value over each hour.</span></span>

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated
```

![Data grafu výkonu](media/log-analytics-log-search-log-search-portal/log-search-portal-13.png)

<span data-ttu-id="08df3-191">Teď, když vhodně seskupuje data, můžete zobrazit jeho v visual graf tak, že přidáte [vykreslení](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) operátor.</span><span class="sxs-lookup"><span data-stu-id="08df3-191">Now that the data is suitably grouped, you can display it in a visual chart by adding the [render](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) operator.</span></span>  

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated | render timechart
```

![Spojnicový graf](media/log-analytics-log-search-log-search-portal/log-search-portal-14.png)

## <a name="next-steps"></a><span data-ttu-id="08df3-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="08df3-193">Next steps</span></span>

- <span data-ttu-id="08df3-194">Další informace o analýzy protokolů dotazovací jazyk v [Začínáme s portálu analýza](https://go.microsoft.com/fwlink/?linkid=856079).</span><span class="sxs-lookup"><span data-stu-id="08df3-194">Learn more about the Log Analytics query language at [Getting Started with the Analytics Portal](https://go.microsoft.com/fwlink/?linkid=856079).</span></span>
- <span data-ttu-id="08df3-195">Provede kurz pomocí [Advanced Analytics portál](https://go.microsoft.com/fwlink/?linkid=856587) která umožňuje spouštět stejné dotazy a přístup ke stejným datům jako portál hledání protokolů.</span><span class="sxs-lookup"><span data-stu-id="08df3-195">Walk through a tutorial using the [Advanced Analytics portal](https://go.microsoft.com/fwlink/?linkid=856587) which allows you to run the same queries and access the same data as the Log Search portal.</span></span>
