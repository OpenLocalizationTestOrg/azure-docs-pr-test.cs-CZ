---
title: "aaaUsing hello hledání protokolů portálu v Azure Log Analytics | Microsoft Docs"
description: "Tento článek obsahuje kurz, který popisuje, jak toocreate protokolu hledání a analyzovat data uložená v pracovním prostoru analýzy protokolů pomocí portálu hledání protokolů hello.  kurz Hello zahrnuje spouštění několik jednoduchých dotazů tooreturn různé typy dat a analýza výsledků."
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
ms.openlocfilehash: 2e6633d548bb508edc0c650d11d2c32fc6ee536c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-log-searches-in-azure-log-analytics-using-hello-log-search-portal"></a><span data-ttu-id="9f80d-104">Vytvoření protokolu hledání v Azure Log Analytics pomocí portálu hledání protokolů hello</span><span class="sxs-lookup"><span data-stu-id="9f80d-104">Create log searches in Azure Log Analytics using hello Log Search portal</span></span>

> [!NOTE]
> <span data-ttu-id="9f80d-105">Tento článek popisuje hello hledání protokolů portálu v Azure Log Analytics pomocí dotazovacího jazyka pro nové hello.</span><span class="sxs-lookup"><span data-stu-id="9f80d-105">This article describes hello Log Search portal in Azure Log Analytics using hello new query language.</span></span>  <span data-ttu-id="9f80d-106">Můžete získat další informace o nový jazyk hello a získat hello postup tooupgrade pracovního prostoru v [Upgrade vyhledávání protokolu toonew pracovní prostor analýzy protokolů Azure](log-analytics-log-search-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="9f80d-106">You can learn more about hello new language and get hello procedure tooupgrade your workspace at [Upgrade your Azure Log Analytics workspace toonew log search](log-analytics-log-search-upgrade.md).</span></span>  
>
> <span data-ttu-id="9f80d-107">Pokud pracovní prostor nebyla upgradovaná toohello nové dotazovací jazyk, se seznamte s příliš[najít data pomocí protokolu hledání v analýzy protokolů](log-analytics-log-searches.md) informace o aktuální verzi portálu hledání protokolů hello hello.</span><span class="sxs-lookup"><span data-stu-id="9f80d-107">If your workspace hasn't been upgraded toohello new query language, you should refer too[Find data using log searches in Log Analytics](log-analytics-log-searches.md) for information on hello current version of hello Log Search portal.</span></span>

<span data-ttu-id="9f80d-108">Tento článek obsahuje kurz, který popisuje, jak toocreate protokolu hledání a analyzovat data uložená v pracovním prostoru analýzy protokolů pomocí portálu hledání protokolů hello.</span><span class="sxs-lookup"><span data-stu-id="9f80d-108">This article includes a tutorial that describes how toocreate log searches and analyze data stored in your Log Analytics workspace using hello Log Search portal.</span></span>  <span data-ttu-id="9f80d-109">kurz Hello zahrnuje spouštění několik jednoduchých dotazů tooreturn různé typy dat a analýza výsledků.</span><span class="sxs-lookup"><span data-stu-id="9f80d-109">hello tutorial includes running some simple queries tooreturn different types of data and analyzing results.</span></span>  <span data-ttu-id="9f80d-110">Zaměřuje se na funkce portálu hello hledání protokolů pro úpravy hello dotazu a nikoli změny přímo.</span><span class="sxs-lookup"><span data-stu-id="9f80d-110">It focuses on features in hello Log Search portal for modifying hello query rather than modifying it directly.</span></span>  <span data-ttu-id="9f80d-111">Podrobnosti o přímou úpravou dotazu hello, najdete v části hello [referenční příručka jazyka dotazů](https://go.microsoft.com/fwlink/?linkid=856079).</span><span class="sxs-lookup"><span data-stu-id="9f80d-111">For details on directly editing hello query, see hello [Query Language reference](https://go.microsoft.com/fwlink/?linkid=856079).</span></span>

<span data-ttu-id="9f80d-112">hledání toocreate portálu hello pokročilé analýzy místo hello hledání protokolů portálu, najdete v části [Začínáme s hello portálu analýza](https://go.microsoft.com/fwlink/?linkid=856587).</span><span class="sxs-lookup"><span data-stu-id="9f80d-112">toocreate searches in hello Advanced Analytics portal instead of hello Log Search portal, see [Getting Started with hello Analytics Portal](https://go.microsoft.com/fwlink/?linkid=856587).</span></span>  <span data-ttu-id="9f80d-113">Oba Portály použít hello stejný dotaz jazyka tooaccess hello stejná data v pracovní prostor analýzy protokolů hello.</span><span class="sxs-lookup"><span data-stu-id="9f80d-113">Both portals use hello same query language tooaccess hello same data in hello Log Analytics workspace.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9f80d-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9f80d-114">Prerequisites</span></span>
<span data-ttu-id="9f80d-115">Tento kurz předpokládá, že už máte pracovní prostor analýzy protokolů s alespoň jeden připojený zdroj, který generuje data pro dotazy tooanalyze hello.</span><span class="sxs-lookup"><span data-stu-id="9f80d-115">This tutorial assumes that you already have a Log Analytics workspace with at least one connected source that generates data for hello queries tooanalyze.</span></span>  

- <span data-ttu-id="9f80d-116">Pokud nemáte pracovní prostor, můžete vytvořit volné jedním postupem hello v [začít pracovat s pracovní prostor analýzy protokolů](log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="9f80d-116">If you don't have a workspace, you can create a free one using hello procedure at [Get started with a Log Analytics workspace](log-analytics-get-started.md).</span></span>
- <span data-ttu-id="9f80d-117">Připojit aspoň jeden [agenta Windows](log-analytics-windows-agents.md) nebo jednu [agenta systému Linux](log-analytics-linux-agents.md) toohello prostoru.</span><span class="sxs-lookup"><span data-stu-id="9f80d-117">Connect least one [Windows agent](log-analytics-windows-agents.md) or one [Linux agent](log-analytics-linux-agents.md) toohello workspace.</span></span>  

## <a name="open-hello-log-search-portal"></a><span data-ttu-id="9f80d-118">Otevřete hello hledání protokolů portálu</span><span class="sxs-lookup"><span data-stu-id="9f80d-118">Open hello Log Search portal</span></span>
<span data-ttu-id="9f80d-119">Začněte otevřením portálu hledání protokolů hello.</span><span class="sxs-lookup"><span data-stu-id="9f80d-119">Start by opening hello Log Search portal.</span></span>  <span data-ttu-id="9f80d-120">Můžete k němu přístup v hello portál Azure nebo portálu OMS hello.</span><span class="sxs-lookup"><span data-stu-id="9f80d-120">You can access it in either hello Azure portal or hello OMS portal.</span></span>

1. <span data-ttu-id="9f80d-121">Otevřete hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9f80d-121">Open hello Azure portal.</span></span>
2. <span data-ttu-id="9f80d-122">Přejděte tooLog analýzy a vyberte pracovní prostor.</span><span class="sxs-lookup"><span data-stu-id="9f80d-122">Navigate tooLog Analytics and select your workspace.</span></span>
3. <span data-ttu-id="9f80d-123">Vyberte buď **hledání protokolů** toostay v hello Azure portal nebo spusťte hello portálu OMS výběrem **portálu OMS** a potom kliknutím na tlačítko hledání protokolů hello.</span><span class="sxs-lookup"><span data-stu-id="9f80d-123">Either select **Log Search** toostay in hello Azure portal or launch hello OMS portal by selecting **OMS Portal** and then clicking hello Log Search button.</span></span>

![Tlačítko vyhledat protokolu](media/log-analytics-log-search-log-search-portal/log-search-button.png)

## <a name="create-a-simple-search"></a><span data-ttu-id="9f80d-125">Vytvoření jednoduché hledání</span><span class="sxs-lookup"><span data-stu-id="9f80d-125">Create a simple search</span></span>
<span data-ttu-id="9f80d-126">Hello nejrychlejší způsob, jak tooretrieve některé toowork data s je jednoduchý dotaz, který vrátí všechny záznamy v tabulce.</span><span class="sxs-lookup"><span data-stu-id="9f80d-126">hello quickest way tooretrieve some data toowork with is a simple query that returns all records in table.</span></span>  <span data-ttu-id="9f80d-127">Pokud máte jakékoli systému Windows nebo Linux prostoru připojené tooyour klientů, budete mít data v buď hello událostí (Windows) nebo tabulka, Syslog (Linux).</span><span class="sxs-lookup"><span data-stu-id="9f80d-127">If you have any Windows or Linux clients connected tooyour workspace, then you'll have data in either hello Event (Windows) or Syslog (Linux) table.</span></span>

<span data-ttu-id="9f80d-128">Zadejte jeden hello následující dotazy hello vyhledávacího pole a klikněte na tlačítko Hledat hello.</span><span class="sxs-lookup"><span data-stu-id="9f80d-128">Type one hello following queries in hello search box and click hello search button.</span></span>  

```
Event
```
```
Syslog
```

<span data-ttu-id="9f80d-129">Data jsou vrácena v zobrazení seznamu výchozí hello a můžete zobrazit celkový počet záznamů vrácených.</span><span class="sxs-lookup"><span data-stu-id="9f80d-129">Data is returned in hello default list view, and you can see how many total records were returned.</span></span>

![Jednoduchý dotaz](media/log-analytics-log-search-log-search-portal/log-search-portal-01.png)

<span data-ttu-id="9f80d-131">Pouze hello první několik vlastností každý záznam se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="9f80d-131">Only hello first few properties of each record are displayed.</span></span>  <span data-ttu-id="9f80d-132">Klikněte na tlačítko **zobrazit další** toodisplay všechny vlastnosti pro konkrétní záznam.</span><span class="sxs-lookup"><span data-stu-id="9f80d-132">Click **show more** toodisplay all properties for a particular record.</span></span>

![Podrobnosti záznamu](media/log-analytics-log-search-log-search-portal/log-search-portal-02.png)

## <a name="set-hello-time-scope"></a><span data-ttu-id="9f80d-134">Nastavit obor čas hello</span><span class="sxs-lookup"><span data-stu-id="9f80d-134">Set hello time scope</span></span>
<span data-ttu-id="9f80d-135">Každý záznam shromažďují analýzy protokolů má **TimeGenerated** vlastnost, která obsahuje hello datum a čas vytvoření tohoto záznamu hello.</span><span class="sxs-lookup"><span data-stu-id="9f80d-135">Every record collected by Log Analytics has a **TimeGenerated** property that contains hello date and time that hello record was created.</span></span>  <span data-ttu-id="9f80d-136">Dotaz portálu hledání protokolů hello pouze vrátí záznamy s **TimeGenerated** v rámci oboru hello čas, který se zobrazí na levé straně obrazovky hello hello.</span><span class="sxs-lookup"><span data-stu-id="9f80d-136">A query in hello Log Search portal only returns records with a **TimeGenerated** within hello time scope that's displayed on hello left side of hello screen.</span></span>  

<span data-ttu-id="9f80d-137">Filtr času hello můžete změnit tak, že vyberete hello rozevírací nebo změnou hello posuvníku.</span><span class="sxs-lookup"><span data-stu-id="9f80d-137">You can change hello time filter either by selecting hello dropdown or by modifying hello slider.</span></span>  <span data-ttu-id="9f80d-138">posuvník Hello Zobrazí pruhový graf, který ukazuje hello relativní počet záznamů pro každý segment čas v rozsahu hello.</span><span class="sxs-lookup"><span data-stu-id="9f80d-138">hello slider displays a bar graph that shows hello relative number of records for each time segment within hello range.</span></span>  <span data-ttu-id="9f80d-139">Tento segment se bude lišit v závislosti na rozsahu hello.</span><span class="sxs-lookup"><span data-stu-id="9f80d-139">This segment will vary depending on hello range.</span></span>

<span data-ttu-id="9f80d-140">Hello výchozí čas obor je **1 den**.</span><span class="sxs-lookup"><span data-stu-id="9f80d-140">hello default time scope is **1 day**.</span></span>  <span data-ttu-id="9f80d-141">Tuto hodnotu změnit příliš**7 dní**, a celkový počet záznamů hello měli zvýšit.</span><span class="sxs-lookup"><span data-stu-id="9f80d-141">Change this value too**7 days**, and hello total number of records should increase.</span></span>

![Datum čas oboru](media/log-analytics-log-search-log-search-portal/log-search-portal-03.png)

## <a name="filter-results-of-hello-query"></a><span data-ttu-id="9f80d-143">Filtrování výsledků dotazu hello</span><span class="sxs-lookup"><span data-stu-id="9f80d-143">Filter results of hello query</span></span>
<span data-ttu-id="9f80d-144">Na hello je levé straně obrazovky hello hello podokno filtru, která vám umožní tooadd filtrování toohello dotazu bez změny přímo.</span><span class="sxs-lookup"><span data-stu-id="9f80d-144">On hello left side of hello screen is hello filter pane which allows you tooadd filtering toohello query without modifying it directly.</span></span>  <span data-ttu-id="9f80d-145">Zobrazí se několik vlastnosti hello záznamů vrácených s jejich top deset hodnoty s jejich počet záznamů.</span><span class="sxs-lookup"><span data-stu-id="9f80d-145">Several properties of hello records returned are displayed with their top ten values with their record count.</span></span>

<span data-ttu-id="9f80d-146">Pokud pracujete s **událostí**, vyberte hello zaškrtávací políčko vedle příliš**chyba** pod **EVENTLEVELNAME**.</span><span class="sxs-lookup"><span data-stu-id="9f80d-146">If you're working with **Event**, select hello checkbox next too**Error** under **EVENTLEVELNAME**.</span></span>   <span data-ttu-id="9f80d-147">Pokud pracujete s **Syslog**, vyberte hello zaškrtávací políčko vedle příliš**chyba** pod **úroveň ZÁVAŽNOSTI**.</span><span class="sxs-lookup"><span data-stu-id="9f80d-147">If you're working with **Syslog**, select hello checkbox next too**err** under **SEVERITYLEVEL**.</span></span>  <span data-ttu-id="9f80d-148">Tato operace změní hello dotazů tooone hello následující toolimit hello výsledků tooerror události.</span><span class="sxs-lookup"><span data-stu-id="9f80d-148">This changes hello query tooone of hello following toolimit hello results tooerror events.</span></span>

```
Event | where (EventLevelName == "Error")
```
```
Syslog | where (SeverityLevel == "err")
```

![Filtr](media/log-analytics-log-search-log-search-portal/log-search-portal-04.png)

<span data-ttu-id="9f80d-150">Přidat vlastnosti toohello podokno filtru tak, že vyberete **přidat toofilters** z nabídky vlastnost hello na jednom hello záznamů.</span><span class="sxs-lookup"><span data-stu-id="9f80d-150">Add properties toohello filter pane by selecting **Add toofilters** from hello property menu on one of hello records.</span></span>

![Přidat nabídku toofilter](media/log-analytics-log-search-log-search-portal/log-search-portal-02a.png)

<span data-ttu-id="9f80d-152">Můžete nastavit hello stejné filtrovat výběrem **filtru** nabídce hello vlastnost pro záznam s hodnotou hello chcete toofilter.</span><span class="sxs-lookup"><span data-stu-id="9f80d-152">You can set hello same filter by selecting **Filter** from hello property menu for a record with hello value you want toofilter.</span></span>  

<span data-ttu-id="9f80d-153">Máte hello **filtru** možnost Vlastnosti s jejich název modře.</span><span class="sxs-lookup"><span data-stu-id="9f80d-153">You only have hello **Filter** option for properties with their name in blue.</span></span>  <span data-ttu-id="9f80d-154">Jedná se o *prohledávatelné* pole, které jsou indexované pro podmínky vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="9f80d-154">These are *searchable* fields which are indexed for search conditions.</span></span>  <span data-ttu-id="9f80d-155">Pole šedě jsou *volné text prohledávatelné* pole, které mají jenom hello **zobrazit odkazy** možnost.</span><span class="sxs-lookup"><span data-stu-id="9f80d-155">Fields in grey are *free text searchable* fields which only have hello **Show references** option.</span></span>  <span data-ttu-id="9f80d-156">Tato možnost vrátí záznamy, které mají tuto hodnotu v libovolné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="9f80d-156">This option returns records that have that value in any property.</span></span>

![Filtr nabídky](media/log-analytics-log-search-log-search-portal/log-search-portal-01a.png)

<span data-ttu-id="9f80d-158">Můžete seskupit hello výsledky na jedinou vlastnost výběrem hello **Seskupit podle** možnost v nabídce záznam hello.</span><span class="sxs-lookup"><span data-stu-id="9f80d-158">You can group hello results on a single property by selecting hello **Group by** option in hello record menu.</span></span>  <span data-ttu-id="9f80d-159">Bude přidáno [shrnout](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) dotazu tooyour operátor, který zobrazí výsledky hello v grafu.</span><span class="sxs-lookup"><span data-stu-id="9f80d-159">This will add a [summarize](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) operator tooyour query that displays hello results in a chart.</span></span>  <span data-ttu-id="9f80d-160">Můžete seskupit na více než jednu vlastnost, ale měli byste tooedit hello dotaz přímo.</span><span class="sxs-lookup"><span data-stu-id="9f80d-160">You can group on more than one property, but you would need tooedit hello query directly.</span></span>  <span data-ttu-id="9f80d-161">Vyberte hello záznamů nabídky Další hello hello **počítače** vlastnost a vyberte **Seskupit podle "Počítač"**.</span><span class="sxs-lookup"><span data-stu-id="9f80d-161">Select hello record menu next hello hello **Computer** property and select **Group by 'Computer'**.</span></span>  

![Seskupit podle počítače](media/log-analytics-log-search-log-search-portal/log-search-portal-10.png)

## <a name="work-with-results"></a><span data-ttu-id="9f80d-163">Práce s výsledky</span><span class="sxs-lookup"><span data-stu-id="9f80d-163">Work with results</span></span>
<span data-ttu-id="9f80d-164">portál hledání protokolů Hello obsahuje řadu funkcí pro práci s hello výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="9f80d-164">hello Log Search portal has a variety of features for working with hello results of a query.</span></span>  <span data-ttu-id="9f80d-165">Můžete řadit, filtr a seskupení výsledků tooanalyze hello dat bez úpravy dotazu skutečné hello.</span><span class="sxs-lookup"><span data-stu-id="9f80d-165">You can sort, filter, and group results tooanalyze hello data without modifying hello actual query.</span></span>  <span data-ttu-id="9f80d-166">Ve výchozím nastavení nejsou seřazeny výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="9f80d-166">Results of a query are not sorted by default.</span></span>

<span data-ttu-id="9f80d-167">tooview hello dat v tabulce formulář, který nabízí další možnosti pro filtrování a řazení, klikněte na tlačítko **tabulky**.</span><span class="sxs-lookup"><span data-stu-id="9f80d-167">tooview hello data in table form which provides additional options for filtering and sorting, click **Table**.</span></span>  

![Zobrazení tabulky](media/log-analytics-log-search-log-search-portal/log-search-portal-05.png)

<span data-ttu-id="9f80d-169">Klikněte na šipku hello pomocí záznamu tooview hello podrobnosti pro tento záznam.</span><span class="sxs-lookup"><span data-stu-id="9f80d-169">Click hello arrow by a record tooview hello details for that record.</span></span>

![Řazení výsledků](media/log-analytics-log-search-log-search-portal/log-search-portal-06.png)

<span data-ttu-id="9f80d-171">Řazení v některém poli kliknutím na záhlaví sloupce.</span><span class="sxs-lookup"><span data-stu-id="9f80d-171">Sort on any field by clicking on its column header.</span></span>

![Řazení výsledků](media/log-analytics-log-search-log-search-portal/log-search-portal-07.png)

<span data-ttu-id="9f80d-173">Filtrování výsledků hello na konkrétní hodnotu ve sloupci hello kliknutím na tlačítko Filtrovat hello a poskytnutím podmínku filtrování.</span><span class="sxs-lookup"><span data-stu-id="9f80d-173">Filter hello results on a specific value in hello column by clicking hello filter button and providing a filter condition.</span></span>

![Filtrování výsledků](media/log-analytics-log-search-log-search-portal/log-search-portal-08.png)

<span data-ttu-id="9f80d-175">Skupiny na sloupci tak, že přetáhnete nahoře toohello záhlaví sloupce hello výsledků.</span><span class="sxs-lookup"><span data-stu-id="9f80d-175">Group on a column by dragging its column header toohello top of hello results.</span></span>  <span data-ttu-id="9f80d-176">Můžete seskupit více polí přetažením horní toohello více sloupců.</span><span class="sxs-lookup"><span data-stu-id="9f80d-176">You can group on multiple fields by dragging multiple columns toohello top.</span></span>

![Výsledky skupiny](media/log-analytics-log-search-log-search-portal/log-search-portal-09.png)



## <a name="work-with-performance-data"></a><span data-ttu-id="9f80d-178">Práce s data výkonu</span><span class="sxs-lookup"><span data-stu-id="9f80d-178">Work with performance data</span></span>
<span data-ttu-id="9f80d-179">Údaje o výkonu pro systém Windows a Linux agentů je uložen v prostoru analýzy protokolů hello hello **výkonu** tabulky.</span><span class="sxs-lookup"><span data-stu-id="9f80d-179">Performance data for both Windows and Linux agents is stored in hello Log Analytics workspace in hello **Perf** table.</span></span>  <span data-ttu-id="9f80d-180">Zaznamenává výkonu vypadají stejně jako jakýkoli jiný záznam a jsme může zapisovat jednoduchý dotaz, který vrátí že všechny záznamy výkonu stejně jako s událostmi.</span><span class="sxs-lookup"><span data-stu-id="9f80d-180">Performance records look just like any other record, and we can write a simple query that returns all performance records just like with events.</span></span>

```
Perf
```

![Údaje o výkonu](media/log-analytics-log-search-log-search-portal/log-search-portal-11.png)

<span data-ttu-id="9f80d-182">Vrácení miliony záznamů pro všechny objekty výkonu a čítače, když není velmi užitečné.</span><span class="sxs-lookup"><span data-stu-id="9f80d-182">Returning millions of records for all performance objects and counters though isn't very useful.</span></span>  <span data-ttu-id="9f80d-183">Hello stejné metody, které můžete použít výše toofilter hello dat nebo jednoduše zadejte hello následující dotaz můžete použít přímo do vyhledávacího pole hello protokolu.</span><span class="sxs-lookup"><span data-stu-id="9f80d-183">You can use hello same methods you used above toofilter hello data or just type hello following query directly into hello log search box.</span></span>  <span data-ttu-id="9f80d-184">Tento příkaz vrátí jenom procesoru záznamů využití pro počítače se systémy Windows a Linux.</span><span class="sxs-lookup"><span data-stu-id="9f80d-184">This returns only processor utilization records for both Windows and Linux computers.</span></span>

```
Perf | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time")
```

![Využití procesoru](media/log-analytics-log-search-log-search-portal/log-search-portal-12.png)

<span data-ttu-id="9f80d-186">Toto nastavení omezuje hello data tooa konkrétní čítače, ale je stále není pro něj formulář, který je obzvláště užitečné.</span><span class="sxs-lookup"><span data-stu-id="9f80d-186">This limits hello data tooa particular counter, but it still doesn't put it in a form that's particularly useful.</span></span>  <span data-ttu-id="9f80d-187">Můžete zobrazit hello data v spojnicový graf, ale nejprve toogroup ji tak, že počítač a TimeGenerated.</span><span class="sxs-lookup"><span data-stu-id="9f80d-187">You can display hello data in a line chart, but first need toogroup it by Computer and TimeGenerated.</span></span>  <span data-ttu-id="9f80d-188">toogroup více polí, je nutné dotaz hello toomodify přímo, takže upravit následující toohello hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="9f80d-188">toogroup on multiple fields, you need toomodify hello query directly, so modify hello query toohello following.</span></span>  <span data-ttu-id="9f80d-189">Tato služba využívá hello [průměr](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/avg()) funkce na hello **přepočtené** průměrnou hodnotu vlastnosti toocalculate hello přes každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="9f80d-189">This uses hello [avg](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/avg()) function on hello **CounterValue** property toocalculate hello average value over each hour.</span></span>

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated
```

![Data grafu výkonu](media/log-analytics-log-search-log-search-portal/log-search-portal-13.png)

<span data-ttu-id="9f80d-191">Teď, když jsou seskupena vhodně hello data, můžete zobrazit jeho v visual graf přidáním hello [vykreslení](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) operátor.</span><span class="sxs-lookup"><span data-stu-id="9f80d-191">Now that hello data is suitably grouped, you can display it in a visual chart by adding hello [render](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) operator.</span></span>  

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated | render timechart
```

![Spojnicový graf](media/log-analytics-log-search-log-search-portal/log-search-portal-14.png)

## <a name="next-steps"></a><span data-ttu-id="9f80d-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9f80d-193">Next steps</span></span>

- <span data-ttu-id="9f80d-194">Další informace o hello analýzy protokolů dotazu jazyka v [Začínáme s hello portálu analýza](https://go.microsoft.com/fwlink/?linkid=856079).</span><span class="sxs-lookup"><span data-stu-id="9f80d-194">Learn more about hello Log Analytics query language at [Getting Started with hello Analytics Portal](https://go.microsoft.com/fwlink/?linkid=856079).</span></span>
- <span data-ttu-id="9f80d-195">Provede kurz pomocí hello [Advanced Analytics portál](https://go.microsoft.com/fwlink/?linkid=856587) který vám umožní toorun hello stejné dotazů a přístup hello stejná data jako portál hledání protokolů hello.</span><span class="sxs-lookup"><span data-stu-id="9f80d-195">Walk through a tutorial using hello [Advanced Analytics portal](https://go.microsoft.com/fwlink/?linkid=856587) which allows you toorun hello same queries and access hello same data as hello Log Search portal.</span></span>
