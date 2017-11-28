---
title: "Najít data pomocí protokolu hledání v Azure Log Analytics | Microsoft Docs"
description: "Prohledávání protokolů umožňuje zjišťovat kombinace a korelace jakýchkoli dat o počítačích z více zdrojů v rámci prostředí."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: 0d7b6712-1722-423b-a60f-05389cde3625
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: bwren
ms.openlocfilehash: bf237a837297cb8f1ab3a3340139133adcd2b244
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="find-data-using-log-searches-in-log-analytics"></a><span data-ttu-id="f2a86-103">Najít data pomocí protokolu hledání v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="f2a86-103">Find data using log searches in Log Analytics</span></span>

>[!NOTE]
> <span data-ttu-id="f2a86-104">Tento článek popisuje protokolu vyhledávání v aktuální jazyk dotazu analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="f2a86-104">This article describes log searches using the current query language in Log Analytics.</span></span>  <span data-ttu-id="f2a86-105">Pokud pracovní prostor byl upgradován na verzi [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak se seznamte s [principy protokolu vyhledá v analýzy protokolů (Nový)](log-analytics-log-search-new.md).</span><span class="sxs-lookup"><span data-stu-id="f2a86-105">If your workspace has been upgraded to the [new Log Analytics query language](log-analytics-log-search-upgrade.md), then you should refer to [Understanding log searches in Log Analytics (new)](log-analytics-log-search-new.md).</span></span>


<span data-ttu-id="f2a86-106">Základem analýzy protokolů je funkce vyhledávání protokolu, která umožňuje zkombinovat a korelovat žádná data počítače z více zdrojů ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="f2a86-106">At the core of Log Analytics is the log search feature which allows you to combine and correlate any machine data from multiple sources within your environment.</span></span> <span data-ttu-id="f2a86-107">Řešení se také používá technologii protokolu vyhledávání, aby vám metriky seskupit kolem oblasti konkrétní problém.</span><span class="sxs-lookup"><span data-stu-id="f2a86-107">Solutions are also powered by log search to bring you metrics pivoted around a particular problem area.</span></span>

<span data-ttu-id="f2a86-108">Na stránce hledání můžete vytvořit dotaz a pak při hledání, můžete filtrovat výsledky pomocí ovládacích prvků omezující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f2a86-108">On the Search page, you can create a query, and then when you search, you can filter the results by using facet controls.</span></span> <span data-ttu-id="f2a86-109">Můžete také vytvořit pokročilými dotazy transformace, filtr a sestavy na výsledky.</span><span class="sxs-lookup"><span data-stu-id="f2a86-109">You can also create advanced queries to transform, filter, and report on your results.</span></span>

<span data-ttu-id="f2a86-110">Běžné dotazy vyhledávání protokolu se zobrazí na většina stránek řešení.</span><span class="sxs-lookup"><span data-stu-id="f2a86-110">Common log search queries appear on most solution pages.</span></span> <span data-ttu-id="f2a86-111">V konzoli OMS můžete kliknutím na tlačítko dlaždice nebo přejít k podrobnostem na jiné položky k zobrazení podrobností o položce pomocí protokolu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="f2a86-111">Throughout the OMS console, you can click tiles or drill in to other items to view details about the item by using log search.</span></span>

<span data-ttu-id="f2a86-112">V tomto kurzu budeme zabývat příklady nepokrývají všechny základní informace při použití hledání protokolů.</span><span class="sxs-lookup"><span data-stu-id="f2a86-112">In this tutorial, we'll walk through examples to cover all the basics when you use log search.</span></span>

<span data-ttu-id="f2a86-113">Jsme budete začínat jednoduchý, praktické příklady a potom stavět na je, abyste měli představu o případy praktická použití o tom, jak pomocí syntaxe statistiky, které chcete z data.</span><span class="sxs-lookup"><span data-stu-id="f2a86-113">We'll start with simple, practical examples and then build on them so that you can get an understanding of practical use cases about how to use the syntax to extract the insights you want from the data.</span></span>

<span data-ttu-id="f2a86-114">Poté, co jste obeznámeni s vyhledávání techniky, můžete zkontrolovat [analýzy protokolů protokolu vyhledávání odkaz](log-analytics-search-reference.md).</span><span class="sxs-lookup"><span data-stu-id="f2a86-114">After you've familiar with search techniques, you can review the [Log Analytics log search reference](log-analytics-search-reference.md).</span></span>

## <a name="use-basic-filters"></a><span data-ttu-id="f2a86-115">Použití základní filtrů</span><span class="sxs-lookup"><span data-stu-id="f2a86-115">Use basic filters</span></span>
<span data-ttu-id="f2a86-116">První věc, kterou potřebujete vědět, je, že první část vyhledávání dotazu před spuštěním "|" znak svislé čáry znak, je vždy *filtru*.</span><span class="sxs-lookup"><span data-stu-id="f2a86-116">The first thing to know is that the first part of a search query, before any "|" vertical pipe character, is always a *filter*.</span></span> <span data-ttu-id="f2a86-117">Můžete si ho představit jako klauzule WHERE v TSQL – Určuje *co* podmnožinu dat načítat z úložiště dat OMS.</span><span class="sxs-lookup"><span data-stu-id="f2a86-117">You can think of it as a WHERE clause in TSQL--it determines *what* subset of data to pull out of the OMS data store.</span></span> <span data-ttu-id="f2a86-118">Hledání v úložišti dat je z velké části o zadání charakteristiky data, která mají být extrahovány, takže je přirozené dotazu by začínat klauzuli WHERE.</span><span class="sxs-lookup"><span data-stu-id="f2a86-118">Searching in the data store is largely about specifying the characteristics of the data that you want to extract, so it is natural that a query would start with the WHERE clause.</span></span>

<span data-ttu-id="f2a86-119">Nejzákladnější filtry, které můžete použít jsou *klíčová slova*, jako je například "Chyba" nebo "časový limit nebo název počítače.</span><span class="sxs-lookup"><span data-stu-id="f2a86-119">The most basic filters you can use are *keywords*, such as ‘error’ or ‘timeout’, or a computer name.</span></span> <span data-ttu-id="f2a86-120">Tyto typy dotazů jednoduché obecně vrátí různých tvarů dat v rámci stejné sadu výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="f2a86-120">These types of simple queries generally return diverse shapes of data within the same result set.</span></span> <span data-ttu-id="f2a86-121">Je to proto analýzy protokolů má jiný *typy* dat v systému.</span><span class="sxs-lookup"><span data-stu-id="f2a86-121">This is because Log Analytics has different *types* of data in the system.</span></span>

### <a name="to-conduct-a-simple-search"></a><span data-ttu-id="f2a86-122">Umožňuje provádět jednoduché hledání</span><span class="sxs-lookup"><span data-stu-id="f2a86-122">To conduct a simple search</span></span>
1. <span data-ttu-id="f2a86-123">Na portálu OMS, klikněte na tlačítko **hledání protokolů**.</span><span class="sxs-lookup"><span data-stu-id="f2a86-123">In the OMS portal, click **Log Search**.</span></span>  
    <span data-ttu-id="f2a86-124">![dlaždice hledání](./media/log-analytics-log-searches/oms-overview-log-search.png)</span><span class="sxs-lookup"><span data-stu-id="f2a86-124">![search tile](./media/log-analytics-log-searches/oms-overview-log-search.png)</span></span>
2. <span data-ttu-id="f2a86-125">V poli dotazu zadejte `error` a pak klikněte na **vyhledávání**.</span><span class="sxs-lookup"><span data-stu-id="f2a86-125">In the query field, type `error` and then click **Search**.</span></span>  
    <span data-ttu-id="f2a86-126">![došlo k chybě hledání](./media/log-analytics-log-searches/oms-search-error.png)</span><span class="sxs-lookup"><span data-stu-id="f2a86-126">![search error](./media/log-analytics-log-searches/oms-search-error.png)</span></span>  
    <span data-ttu-id="f2a86-127">Příklad dotazu pro `error` na následujícím obrázku vrátil 100 000 **událostí** záznamy (shromažďují Správa protokolu), 18 **ConfigurationAlert** záznamů (generované konfigurace Assessment) a 12 **ConfigurationChange** záznamy (zachycenou sledování změn).</span><span class="sxs-lookup"><span data-stu-id="f2a86-127">For example, the query for `error` in the following image returned 100,000 **Event** records (collected by Log Management), 18 **ConfigurationAlert** records (generated by Configuration Assessment) and 12 **ConfigurationChange** records (captured by the Change Tracking).</span></span>   
    <span data-ttu-id="f2a86-128">![výsledky hledání](./media/log-analytics-log-searches/oms-search-results01.png)</span><span class="sxs-lookup"><span data-stu-id="f2a86-128">![search results](./media/log-analytics-log-searches/oms-search-results01.png)</span></span>  

<span data-ttu-id="f2a86-129">Tyto filtry nejsou skutečně typy/tříd objektů.</span><span class="sxs-lookup"><span data-stu-id="f2a86-129">These filters are not really object types/classes.</span></span> <span data-ttu-id="f2a86-130">*Typ* je právě značku, vlastnost nebo řetězec nebo název nebo kategorie, který je připojen k část data.</span><span class="sxs-lookup"><span data-stu-id="f2a86-130">*Type* is just a tag, or a property, or a string/name/category, that is attached to a piece of data.</span></span> <span data-ttu-id="f2a86-131">Některé dokumenty v systému jsou označené jako **typu: ConfigurationAlert** a některé jsou označené jako **typu: výkonu**, nebo **typu: událost**a tak dále.</span><span class="sxs-lookup"><span data-stu-id="f2a86-131">Some documents in the system are tagged as **Type:ConfigurationAlert** and some are tagged as **Type:Perf**, or **Type:Event**, and so on.</span></span> <span data-ttu-id="f2a86-132">Každý výsledek hledání, dokument, záznamu nebo položka zobrazí všechny nezpracované vlastnosti a jejich hodnoty pro každou z těchto položek dat, a tyto názvy polí můžete použít k určení ve filtru, pokud chcete načíst pouze záznamy, kde toto danou hodnotu pole má.</span><span class="sxs-lookup"><span data-stu-id="f2a86-132">Each search result, document, record, or entry displays all the raw properties and their values for each of those pieces of data, and you can use those field names to specify in the filter when you want to retrieve only the records where the field has that given value.</span></span>

<span data-ttu-id="f2a86-133">*Typ* je pouze pole, které mají všechny záznamy, není liší od jiné pole.</span><span class="sxs-lookup"><span data-stu-id="f2a86-133">*Type* is really just a field that all records have, it is not different from any other field.</span></span> <span data-ttu-id="f2a86-134">To bylo vytvořeno na základě hodnoty pole typu.</span><span class="sxs-lookup"><span data-stu-id="f2a86-134">This was established based on the value of the Type field.</span></span> <span data-ttu-id="f2a86-135">Tento záznam bude mít různých formě.</span><span class="sxs-lookup"><span data-stu-id="f2a86-135">That record will have a different shape or form.</span></span> <span data-ttu-id="f2a86-136">Náhodně **typ = výkonu**, nebo **typ = událostí** je také syntaxi, která budete muset naučit se dotázat na údaje o výkonu nebo události.</span><span class="sxs-lookup"><span data-stu-id="f2a86-136">Incidentally, **Type=Perf**, or **Type=Event** is also the syntax that you need to learn to query for performance data or events.</span></span>

<span data-ttu-id="f2a86-137">Za názvem pole a před hodnotu, můžete použít dvojtečkou (:) nebo symbol rovná se (=).</span><span class="sxs-lookup"><span data-stu-id="f2a86-137">You can use either a colon (:) or an equal sign (=) after the field name and before the value.</span></span> <span data-ttu-id="f2a86-138">**Typ: událost** a **typ = událostí** jsou ekvivalentní v význam, můžete zvolit styl dáváte přednost.</span><span class="sxs-lookup"><span data-stu-id="f2a86-138">**Type:Event** and **Type=Event** are equivalent in meaning, you can choose the style you prefer.</span></span>

<span data-ttu-id="f2a86-139">Pokud ano, typ = výkonu záznamy mají pole s názvem "název_čítače, a pak můžete napsat dotaz tvaru `Type=Perf CounterName="% Processor Time"`.</span><span class="sxs-lookup"><span data-stu-id="f2a86-139">So, if the Type=Perf records have a field called 'CounterName', then you can write a query resembling `Type=Perf CounterName="% Processor Time"`.</span></span>

<span data-ttu-id="f2a86-140">Tím získáte pouze data výkonu kde název čítače výkonu je "% času procesoru".</span><span class="sxs-lookup"><span data-stu-id="f2a86-140">This will give you only the performance data where the performance counter name is "% Processor Time".</span></span>

### <a name="to-search-for-processor-time-performance-data"></a><span data-ttu-id="f2a86-141">K vyhledání údaje o výkonu času procesoru</span><span class="sxs-lookup"><span data-stu-id="f2a86-141">To search for processor time performance data</span></span>
* <span data-ttu-id="f2a86-142">Do pole vyhledávání dotazu zadejte`Type=Perf CounterName="% Processor Time"`</span><span class="sxs-lookup"><span data-stu-id="f2a86-142">In the search query field, type `Type=Perf CounterName="% Processor Time"`</span></span>

<span data-ttu-id="f2a86-143">Můžete také vybrat určité a použít **InstanceName = _ "Celkem"** v dotazu, který je čítačů výkonu systému Windows.</span><span class="sxs-lookup"><span data-stu-id="f2a86-143">You can also be more specific and use **InstanceName=_'Total'** in the query, which is a Windows performance counter.</span></span> <span data-ttu-id="f2a86-144">Můžete také vybrat omezující vlastnost a jiné **: hodnota pole**.</span><span class="sxs-lookup"><span data-stu-id="f2a86-144">You can also select a facet and another **field:value**.</span></span> <span data-ttu-id="f2a86-145">Filtr se automaticky přidá do filtru na panelu dotazů.</span><span class="sxs-lookup"><span data-stu-id="f2a86-145">The filter is automatically added to your filter in the query bar.</span></span> <span data-ttu-id="f2a86-146">To vidíte na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="f2a86-146">You can see this in the following image.</span></span> <span data-ttu-id="f2a86-147">Zobrazuje kde klikněte na tlačítko Přidat **InstanceName: "_Total"** na dotaz, aniž by museli zadávat nic.</span><span class="sxs-lookup"><span data-stu-id="f2a86-147">It shows you where to click to add **InstanceName:’_Total’** to the query without typing anything.</span></span>

![omezující vlastnost vyhledávání](./media/log-analytics-log-searches/oms-search-facet.png)

<span data-ttu-id="f2a86-149">Stává dotazu`Type=Perf CounterName="% Processor Time" InstanceName="_Total"`</span><span class="sxs-lookup"><span data-stu-id="f2a86-149">Your query now becomes `Type=Perf CounterName="% Processor Time" InstanceName="_Total"`</span></span>

<span data-ttu-id="f2a86-150">V tomto příkladu nemusíte určit **typ = výkonu** získat k těmto výsledkům.</span><span class="sxs-lookup"><span data-stu-id="f2a86-150">In this example, you don't have to specify **Type=Perf** to get to this result.</span></span> <span data-ttu-id="f2a86-151">Vzhledem k tomu, že pole název_čítače a InstanceName existovat pouze pro záznamy typu = výkonu, dotaz je dost konkrétní, aby vrátí stejné výsledky jako déle, předchozí:</span><span class="sxs-lookup"><span data-stu-id="f2a86-151">Because the fields CounterName and InstanceName only exist for records of Type=Perf, the query is specific enough to return the same results as the longer, previous one:</span></span>

```
CounterName="% Processor Time" InstanceName="_Total"
```

<span data-ttu-id="f2a86-152">Důvodem je, že všechny filtry v dotazu, jsou vyhodnoceny jako používán *a* mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="f2a86-152">This is because all the filters in the query are evaluated as being in *AND* with each other.</span></span> <span data-ttu-id="f2a86-153">Efektivní, další pole přidat kritéria, můžete získat menší, konkrétní a přesnějších výsledků.</span><span class="sxs-lookup"><span data-stu-id="f2a86-153">Effectively, the more fields you add to the criteria, you get less, more specific and refined results.</span></span>

<span data-ttu-id="f2a86-154">Například dotaz `Type=Event EventLog="Windows PowerShell"` je stejný jako `Type=Event AND EventLog="Windows PowerShell"`.</span><span class="sxs-lookup"><span data-stu-id="f2a86-154">For example, the query `Type=Event EventLog="Windows PowerShell"` is identical to `Type=Event AND EventLog="Windows PowerShell"`.</span></span> <span data-ttu-id="f2a86-155">Vrátí všechny události, které byly přihlášení a shromažďují z protokolu událostí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f2a86-155">It returns all events that were logged in and collected from the Windows PowerShell event log.</span></span> <span data-ttu-id="f2a86-156">Pokud přidáte filtr několikrát opakovaně výběrem stejné omezující vlastnost, pak tento problém je čistě kosmetické – ho může zaplnit panelu Hledat, ale stále vrátí stejné výsledky protože implicitní operátor AND je vždycky k dispozici.</span><span class="sxs-lookup"><span data-stu-id="f2a86-156">If you add a filter multiple times by repeatedly selecting the same facet, then the issue is purely cosmetic--it might clutter the Search bar, but it still returns the same results because the implicit AND operator is always there.</span></span>

<span data-ttu-id="f2a86-157">Implicitní operátor AND můžete snadno vrátit explicitně pomocí operátoru NOT.</span><span class="sxs-lookup"><span data-stu-id="f2a86-157">You can easily reverse the implicit AND operator by using a NOT operator explicitly.</span></span> <span data-ttu-id="f2a86-158">Například:</span><span class="sxs-lookup"><span data-stu-id="f2a86-158">For example:</span></span>

<span data-ttu-id="f2a86-159">`Type:Event NOT(EventLog:"Windows PowerShell")`nebo jeho ekvivalent `Type=Event EventLog!="Windows PowerShell"` vrátí všechny události z všechny protokoly, které nejsou v prostředí Windows PowerShell protokolu.</span><span class="sxs-lookup"><span data-stu-id="f2a86-159">`Type:Event NOT(EventLog:"Windows PowerShell")` or its equivalent `Type=Event EventLog!="Windows PowerShell"` return all events from all other logs that are NOT the Windows PowerShell log.</span></span>

<span data-ttu-id="f2a86-160">Nebo můžete například použít jiné logický operátor 'Nebo'.</span><span class="sxs-lookup"><span data-stu-id="f2a86-160">Or, you can use other Boolean operator such as ‘OR’.</span></span> <span data-ttu-id="f2a86-161">Následující dotaz vrátí záznamy, pro které protokolu událostí buď systém nebo aplikace.</span><span class="sxs-lookup"><span data-stu-id="f2a86-161">The following query returns records for which the EventLog is either Application OR System.</span></span>

```
EventLog=Application OR EventLog=System
```

<span data-ttu-id="f2a86-162">Pomocí výše uvedeném dotazu, získáte položky pro oba protokoly ve stejné sadě výsledků.</span><span class="sxs-lookup"><span data-stu-id="f2a86-162">Using the above query, you’ll get entries for both logs in the same result set.</span></span>

<span data-ttu-id="f2a86-163">Ale pokud odeberete nebo ponecháním implicitní a na místě, pak následující dotaz nebude vytváří výsledky protože není k dispozici položku protokolu událostí, který patří do oba protokoly.</span><span class="sxs-lookup"><span data-stu-id="f2a86-163">However, if you remove the OR by leaving the implicit AND in place, then the following query will not produce any results because there isn’t an event log entry that belongs to BOTH logs.</span></span> <span data-ttu-id="f2a86-164">Každá položka protokolu událostí byla zapsána do pouze jeden z dva protokoly.</span><span class="sxs-lookup"><span data-stu-id="f2a86-164">Each event log entry was written to only one of the two logs.</span></span>

```
EventLog=Application EventLog=System
```


## <a name="use-additional-filters"></a><span data-ttu-id="f2a86-165">Použít další filtry</span><span class="sxs-lookup"><span data-stu-id="f2a86-165">Use additional filters</span></span>
<span data-ttu-id="f2a86-166">Následující dotaz vrátí položky pro 2 protokoly událostí pro všechny počítače, které jste odeslali data.</span><span class="sxs-lookup"><span data-stu-id="f2a86-166">The following query returns entries for 2 event logs for all the computers that have sent data.</span></span>

```
EventLog=Application OR EventLog=System
```

![Výsledky vyhledávání](./media/log-analytics-log-searches/oms-search-results03.png)

<span data-ttu-id="f2a86-168">Výběrem jedné z pole nebo filtry budou upřesněte dotaz pro konkrétní počítač, s výjimkou všechny ostatní výsledky.</span><span class="sxs-lookup"><span data-stu-id="f2a86-168">Selecting one of the fields or filters will narrow the query to a specific computer, excluding all other ones.</span></span> <span data-ttu-id="f2a86-169">Výsledný dotaz bude vypadat takto.</span><span class="sxs-lookup"><span data-stu-id="f2a86-169">The resulting query would resemble the following.</span></span>

```
EventLog=Application OR EventLog=System Computer=SERVER1.contoso.com
```

<span data-ttu-id="f2a86-170">Což je totéž následujícím z důvodu implicitní operátorem a.</span><span class="sxs-lookup"><span data-stu-id="f2a86-170">Which is equivalent to the following, because of the implicit AND.</span></span>

```
EventLog=Application OR EventLog=System AND Computer=SERVER1.contoso.com
```

<span data-ttu-id="f2a86-171">V následujícím pořadí explicitní vyhodnotí každý dotaz.</span><span class="sxs-lookup"><span data-stu-id="f2a86-171">Each query is evaluated in the following explicit order.</span></span> <span data-ttu-id="f2a86-172">Poznámka: v závorkách.</span><span class="sxs-lookup"><span data-stu-id="f2a86-172">Note the parenthesis.</span></span>

```
(EventLog=Application OR EventLog=System) AND Computer=SERVER1.contoso.com
```

<span data-ttu-id="f2a86-173">Stejně jako pole v protokolu událostí, je možné načíst data pouze pro sadu konkrétní počítače přidáním nebo.</span><span class="sxs-lookup"><span data-stu-id="f2a86-173">Just like the event log field, you can retrieve data only for a set of specific computers by adding OR.</span></span> <span data-ttu-id="f2a86-174">Například:</span><span class="sxs-lookup"><span data-stu-id="f2a86-174">For example:</span></span>

```
(EventLog=Application OR EventLog=System) AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com OR Computer=SERVER3.contoso.com)
```

<span data-ttu-id="f2a86-175">Podobně to následující dotaz vrátí **% času procesoru** pro pouze vybrané dva počítače.</span><span class="sxs-lookup"><span data-stu-id="f2a86-175">Similarly, this the following query return **% CPU Time** for the selected two computers only.</span></span>

```
CounterName="% Processor Time"  AND InstanceName="_Total" AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com)
```

### <a name="field-types"></a><span data-ttu-id="f2a86-176">Typy polí</span><span class="sxs-lookup"><span data-stu-id="f2a86-176">Field types</span></span>
<span data-ttu-id="f2a86-177">Při vytváření filtrů, byste měli porozumět rozdíly při práci s různými typy pole vrácené protokolu hledání.</span><span class="sxs-lookup"><span data-stu-id="f2a86-177">When creating filters, you should understand the differences in working with different types of fields returned by log searches.</span></span>

<span data-ttu-id="f2a86-178">**Prohledatelná pole** zobrazit modře ve výsledcích hledání.</span><span class="sxs-lookup"><span data-stu-id="f2a86-178">**Searchable fields** show in blue in search results.</span></span>  <span data-ttu-id="f2a86-179">Prohledatelná pole v konkrétní podmínek vyhledávání můžete použít na pole, jako jsou následující:</span><span class="sxs-lookup"><span data-stu-id="f2a86-179">You can use searchable fields in search conditions specific to the field such as the following:</span></span>

```
Type: Event EventLevelName: "Error"
Type: SecurityEvent Computer:Contains("contoso.com")
Type: Event EventLevelName IN {"Error","Warning"}
```

<span data-ttu-id="f2a86-180">**Uvolněte prohledatelná pole text** jsou uvedeny šedě ve výsledcích hledání.</span><span class="sxs-lookup"><span data-stu-id="f2a86-180">**Free text searchable fields** are shown in grey in search results.</span></span>  <span data-ttu-id="f2a86-181">Není možné použít s podmínek vyhledávání, které jsou specifické pro pole jako pole s možností vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="f2a86-181">They cannot be used with search conditions specific to the field like searchable fields.</span></span>  <span data-ttu-id="f2a86-182">Jejich prohledávání pouze při provádění dotazu napříč všechna pole, podobně jako tento.</span><span class="sxs-lookup"><span data-stu-id="f2a86-182">They are only searched when performing a query across all fields such as the following.</span></span>

```
"Error"
Type: Event "Exception"
```


### <a name="boolean-operators"></a><span data-ttu-id="f2a86-183">Logické operátory</span><span class="sxs-lookup"><span data-stu-id="f2a86-183">Boolean operators</span></span>
<span data-ttu-id="f2a86-184">Číselná pole a data a času, můžete vyhledat pomocí hodnoty *větší než*, *menší než*, a *menší než nebo rovna*.</span><span class="sxs-lookup"><span data-stu-id="f2a86-184">With datetime and numeric fields, you can search for values using *greater than*, *lesser than*, and *lesser than or equal*.</span></span> <span data-ttu-id="f2a86-185">Jednoduché operátory můžete použít jako >, <>, =, < =,! = v panelu vyhledávání dotazu.</span><span class="sxs-lookup"><span data-stu-id="f2a86-185">You can use simple operators such as >, < , >=, <= , != in the query search bar.</span></span>

<span data-ttu-id="f2a86-186">Pro určité časové období se můžete dotazovat specifickém protokolu událostí.</span><span class="sxs-lookup"><span data-stu-id="f2a86-186">You can query a specific event log for a specific period of time.</span></span> <span data-ttu-id="f2a86-187">Například následující klávesovými výraz vyjádřený za posledních 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="f2a86-187">For example, the last 24 hours is expressed with the following mnemonic expression.</span></span>

```
EventLog=System TimeGenerated>NOW-24HOURS
```


#### <a name="to-search-using-a-boolean-operator"></a><span data-ttu-id="f2a86-188">Hledání s použitím logický operátor</span><span class="sxs-lookup"><span data-stu-id="f2a86-188">To search using a boolean operator</span></span>
* <span data-ttu-id="f2a86-189">Do pole vyhledávání dotazu zadejte`EventLog=System TimeGenerated>NOW-24HOURS`</span><span class="sxs-lookup"><span data-stu-id="f2a86-189">In the search query field, type `EventLog=System TimeGenerated>NOW-24HOURS`</span></span>  
    <span data-ttu-id="f2a86-190">![hledání s logická hodnota](./media/log-analytics-log-searches/oms-search-boolean.png)</span><span class="sxs-lookup"><span data-stu-id="f2a86-190">![search with boolean](./media/log-analytics-log-searches/oms-search-boolean.png)</span></span>

<span data-ttu-id="f2a86-191">I když můžete řídit časový interval graficky a většina časy, můžete chtít udělat, existují výhody včetně filtr času přímo do dotazu.</span><span class="sxs-lookup"><span data-stu-id="f2a86-191">Although you can control the time interval graphically, and most times you might want to do that, there are advantages to including a time filter directly into the query.</span></span> <span data-ttu-id="f2a86-192">Například to vyhovující postup pomocí řídicích panelů, kde můžete přepsat čas pro každou dlaždici, bez ohledu na to *globální* selektor čas na stránce řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="f2a86-192">For example, this works great with dashboards where you can override the time for each tile, regardless of the *global* time selector on the dashboard page.</span></span> <span data-ttu-id="f2a86-193">Další informace najdete v tématu [čas záleží na řídicím panelu](http://cloudadministrator.wordpress.com/2014/10/19/system-center-advisor-restarted-time-matters-in-dashboard-part-6/).</span><span class="sxs-lookup"><span data-stu-id="f2a86-193">For more information, see [Time Matters in Dashboard](http://cloudadministrator.wordpress.com/2014/10/19/system-center-advisor-restarted-time-matters-in-dashboard-part-6/).</span></span>

<span data-ttu-id="f2a86-194">Při filtrování podle času, mějte na paměti, kterou můžete získat výsledky pro *průnik* dvou časových období: verze zadaná v portálu OMS (S1) a jeden zadaný v dotazu (S2).</span><span class="sxs-lookup"><span data-stu-id="f2a86-194">When filtering by time, keep in mind that you get results for the *intersection* of the two time periods: the one specified in the OMS portal (S1) and the one specified in the query (S2).</span></span>

![průnik](./media/log-analytics-log-searches/oms-search-intersection.png)

<span data-ttu-id="f2a86-196">To znamená, pokud časových období nekříží, například na portálu OMS, kde si zvolíte **tato týdnu** a v dotazu, kde můžete definovat **minulého týdne**, nejsou k dispozici žádné průnik a neobdržíte žádné výsledky.</span><span class="sxs-lookup"><span data-stu-id="f2a86-196">This means, if the time periods don’t intersect, for example in the OMS portal where you choose **This week** and in the query where you define **last week**, then there is no intersection and you won't receive any results.</span></span>

<span data-ttu-id="f2a86-197">Operátory porovnání použitý pro pole TimeGenerated jsou také užitečná v jiných situacích.</span><span class="sxs-lookup"><span data-stu-id="f2a86-197">Comparison operators used for the TimeGenerated field are also useful in other situations.</span></span> <span data-ttu-id="f2a86-198">Například s číselná pole.</span><span class="sxs-lookup"><span data-stu-id="f2a86-198">For example, with numeric fields.</span></span>

<span data-ttu-id="f2a86-199">Například uděleno, že konfigurace Assessment výstrahy mít následující hodnoty závažnosti:</span><span class="sxs-lookup"><span data-stu-id="f2a86-199">For example, given that Configuration Assessment’s alerts have the following severity values:</span></span>

* <span data-ttu-id="f2a86-200">0 = informace</span><span class="sxs-lookup"><span data-stu-id="f2a86-200">0 = Information</span></span>
* <span data-ttu-id="f2a86-201">1 = upozornění</span><span class="sxs-lookup"><span data-stu-id="f2a86-201">1 = Warning</span></span>
* <span data-ttu-id="f2a86-202">2 = kritický</span><span class="sxs-lookup"><span data-stu-id="f2a86-202">2 = Critical</span></span>

<span data-ttu-id="f2a86-203">Můžete dotazovat na upozornění a kritickou výstrahy a také vyloučit informační ty, které jsou s následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="f2a86-203">You can query for both warning and critical alerts and also exclude informational ones with the following query:</span></span>

```
Type=ConfigurationAlert  Severity>=1
```


<span data-ttu-id="f2a86-204">Můžete také použít dotazy na rozsah.</span><span class="sxs-lookup"><span data-stu-id="f2a86-204">You can also use range queries.</span></span> <span data-ttu-id="f2a86-205">To znamená, že může poskytovat začátek a konec rozsahu hodnot v pořadí.</span><span class="sxs-lookup"><span data-stu-id="f2a86-205">This means that you can provide the beginning and end range of values in a sequence.</span></span> <span data-ttu-id="f2a86-206">Například pokud chcete události z protokolu událostí nástroje Operations Manager, kde ID události je větší než nebo rovna hodnotě 2100 ale není větší než. 2199, pak následující dotaz vrátí je.</span><span class="sxs-lookup"><span data-stu-id="f2a86-206">For example, if you want events from the Operations Manager event log where the EventID is greater than or equal to 2100 but not greater than 2199, then the following query would return them.</span></span>

```
Type=Event EventLog="Operations Manager" EventID:[2100..2199]
```


> [!NOTE]
> <span data-ttu-id="f2a86-207">Je nutné použít syntaxi rozsah je oddělovače: hodnota pole dvojtečkou (:) a *není* rovná se (=).</span><span class="sxs-lookup"><span data-stu-id="f2a86-207">The range syntax you must use is the colon (:) field:value separator and *not* the equal sign (=).</span></span> <span data-ttu-id="f2a86-208">Horní a dolní hranice rozsahu uzavřít do hranatých závorek a je oddělit dvě tečky (.).</span><span class="sxs-lookup"><span data-stu-id="f2a86-208">Enclose the lower and upper end of the range in square brackets and separate them with two periods (..).</span></span>
>
>

## <a name="manipulate-search-results"></a><span data-ttu-id="f2a86-209">Manipulace s výsledky hledání</span><span class="sxs-lookup"><span data-stu-id="f2a86-209">Manipulate search results</span></span>
<span data-ttu-id="f2a86-210">Pokud chcete vyhledat data, budete chtít Upřesnit vyhledávací dotaz a mít funkční úroveň kontroly nad výsledky.</span><span class="sxs-lookup"><span data-stu-id="f2a86-210">When you're searching for data, you'll want to refine your search query and have a good level of control over the results.</span></span> <span data-ttu-id="f2a86-211">Pokud jsou načteny výsledky, můžete použít příkazy k jejich transformaci.</span><span class="sxs-lookup"><span data-stu-id="f2a86-211">When results are retrieved, you can apply commands to transform them.</span></span>

<span data-ttu-id="f2a86-212">Příkazy v analýzy protokolů hledání *musí* podle za znak znak svislé čáry (|).</span><span class="sxs-lookup"><span data-stu-id="f2a86-212">Commands in Log Analytics searches *must* follow after the vertical pipe character (|).</span></span> <span data-ttu-id="f2a86-213">Filtr musí být vždy první část řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="f2a86-213">A filter must always be the first part of a query string.</span></span> <span data-ttu-id="f2a86-214">Definuje sadu dat, kterou právě pracujete s a následně "prostřednictvím kanálu předá" výsledků do příkazu.</span><span class="sxs-lookup"><span data-stu-id="f2a86-214">It defines the data set you're working with and then "pipes" those results into a command.</span></span> <span data-ttu-id="f2a86-215">Kanál potom můžete přidat další příkazy.</span><span class="sxs-lookup"><span data-stu-id="f2a86-215">You can then use the pipe to add additional commands.</span></span> <span data-ttu-id="f2a86-216">Toto je volně podobná zřetězením příkazů Windows Powershellu.</span><span class="sxs-lookup"><span data-stu-id="f2a86-216">This is loosely similar to the Windows PowerShell pipeline.</span></span>

<span data-ttu-id="f2a86-217">Obecně platí jazyka vyhledávání analýzy protokolů se pokusí postupujte podle styl prostředí PowerShell a pokyny, chcete-li podobná IT specialisté a k usnadnění křivku.</span><span class="sxs-lookup"><span data-stu-id="f2a86-217">In general, the Log Analytics search language tries to follow PowerShell style and guidelines to make it similar to the IT pros, and to ease the learning curve.</span></span>

<span data-ttu-id="f2a86-218">Příkazy mají názvy příkazů, takže lze snadno určit, co dělají.</span><span class="sxs-lookup"><span data-stu-id="f2a86-218">Commands have names of verbs so you can easily tell what they do.</span></span>  

### <a name="sort"></a><span data-ttu-id="f2a86-219">Seřadit</span><span class="sxs-lookup"><span data-stu-id="f2a86-219">Sort</span></span>
<span data-ttu-id="f2a86-220">Příkaz řazení umožňuje definovat pořadí řazení podle jednoho nebo více polí.</span><span class="sxs-lookup"><span data-stu-id="f2a86-220">The sort command allows you to define the sorting order by one or multiple fields.</span></span> <span data-ttu-id="f2a86-221">I když nepoužijete, ve výchozím nastavení, se vynucuje čas sestupném pořadí.</span><span class="sxs-lookup"><span data-stu-id="f2a86-221">Even if you don’t use it, by default, a time descending order is enforced.</span></span> <span data-ttu-id="f2a86-222">Nejnovější výsledky jsou vždy v horní části výsledků vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="f2a86-222">The most recent results are always at the top of search results.</span></span> <span data-ttu-id="f2a86-223">To znamená, že při spuštění vyhledávání, s `Type=Event EventID=1234` co je pro vás skutečně proveden je:</span><span class="sxs-lookup"><span data-stu-id="f2a86-223">This means that when you run a search, with `Type=Event EventID=1234` what really is executed for you is:</span></span>

```
Type=Event EventID=1234 **| Sort TimeGenerated desc**
```

<span data-ttu-id="f2a86-224">Je to způsobeno je typ možnosti, které jste se seznámili s v protokolech.</span><span class="sxs-lookup"><span data-stu-id="f2a86-224">That's because it is the type of experience you are familiar with in logs.</span></span> <span data-ttu-id="f2a86-225">Například v prohlížeči událostí systému Windows.</span><span class="sxs-lookup"><span data-stu-id="f2a86-225">For example, in the Windows Event Viewer.</span></span>

<span data-ttu-id="f2a86-226">Řazení můžete změnit způsob, jakým budou vráceny výsledky.</span><span class="sxs-lookup"><span data-stu-id="f2a86-226">You can use Sort to change the way results are returned.</span></span> <span data-ttu-id="f2a86-227">Následující příklady ukazují, jak to funguje.</span><span class="sxs-lookup"><span data-stu-id="f2a86-227">The following examples show how this works.</span></span>

```
Type=Event EventID=1234 | Sort TimeGenerated asc
```

```
Type=Event EventID=1234 | Sort Computer asc
```

```
Type=Event EventID=1234 | Sort Computer asc,TimeGenerated desc
```


<span data-ttu-id="f2a86-228">Výše uvedené jednoduché příklady ukazují, jak fungují příkazy – se změní obrazec výsledky, které filtr.</span><span class="sxs-lookup"><span data-stu-id="f2a86-228">The simple examples above show you how commands work--they change the shape of the results that the filter returned.</span></span>

### <a name="limit-and-top"></a><span data-ttu-id="f2a86-229">Omezení a nahoře</span><span class="sxs-lookup"><span data-stu-id="f2a86-229">Limit and top</span></span>
<span data-ttu-id="f2a86-230">Jiného méně známé příkazu je omezení.</span><span class="sxs-lookup"><span data-stu-id="f2a86-230">Another less known command is LIMIT.</span></span> <span data-ttu-id="f2a86-231">Limit je příkaz prostředí PowerShell jako.</span><span class="sxs-lookup"><span data-stu-id="f2a86-231">Limit is a PowerShell-like verb.</span></span> <span data-ttu-id="f2a86-232">Limit je funkčně totožný s příkazem nejvyšší.</span><span class="sxs-lookup"><span data-stu-id="f2a86-232">Limit is functionally identical to the TOP command.</span></span> <span data-ttu-id="f2a86-233">Následující dotazy vrátí stejné výsledky.</span><span class="sxs-lookup"><span data-stu-id="f2a86-233">The following queries return the same results.</span></span>

```
Type=Event EventID=600 | Limit 1
```

```
Type=Event EventID=600 | Top 1
```


#### <a name="to-search-using-top"></a><span data-ttu-id="f2a86-234">Hledání s použitím horní</span><span class="sxs-lookup"><span data-stu-id="f2a86-234">To search using top</span></span>
* <span data-ttu-id="f2a86-235">Do pole vyhledávání dotazu zadejte`Type=Event EventID=600 | Top 1` </span><span class="sxs-lookup"><span data-stu-id="f2a86-235">In the search query field, type `Type=Event EventID=600 | Top 1` </span></span>  
    <span data-ttu-id="f2a86-236">![horní vyhledávání](./media/log-analytics-log-searches/oms-search-top.png)</span><span class="sxs-lookup"><span data-stu-id="f2a86-236">![search top](./media/log-analytics-log-searches/oms-search-top.png)</span></span>

<span data-ttu-id="f2a86-237">Na předchozím obrázku jsou záznamy 358 tisíc s ID události = 600.</span><span class="sxs-lookup"><span data-stu-id="f2a86-237">In the image above, there are 358 thousand records with EventID=600.</span></span> <span data-ttu-id="f2a86-238">Pole, omezující vlastnosti a filtry na levé straně vždy zobrazovat informace o výsledky vrácené *podle části filtru* dotazu, která je součástí před všechny znakem.</span><span class="sxs-lookup"><span data-stu-id="f2a86-238">The fields, facets, and filters on the left always show information about the results returned *by the filter portion* of the query, which is the part before any pipe character.</span></span> <span data-ttu-id="f2a86-239">**Výsledky** podokně pouze vrátí poslední 1 výsledek, protože v ukázkovém příkazu ve tvaru a transformovat výsledky.</span><span class="sxs-lookup"><span data-stu-id="f2a86-239">The **Results** pane only returns the most recent 1 result, because the example command shaped and transformed the results.</span></span>

### <a name="select"></a><span data-ttu-id="f2a86-240">Vyberte</span><span class="sxs-lookup"><span data-stu-id="f2a86-240">Select</span></span>
<span data-ttu-id="f2a86-241">Příkaz SELECT se chová jako Select-Object v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f2a86-241">The SELECT command behaves like Select-Object in PowerShell.</span></span> <span data-ttu-id="f2a86-242">Vrátí filtrované výsledky, které nemají všechny své původní vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f2a86-242">It returns filtered results that do not have all of their original properties.</span></span> <span data-ttu-id="f2a86-243">Místo toho vybere pouze vlastnosti, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="f2a86-243">Instead, it selects only the properties that you specify.</span></span>

#### <a name="to-run-a-search-using-the-select-command"></a><span data-ttu-id="f2a86-244">Ke spuštění vyhledávání pomocí příkazu select</span><span class="sxs-lookup"><span data-stu-id="f2a86-244">To run a search using the select command</span></span>
1. <span data-ttu-id="f2a86-245">Do pole hledání zadejte `Type=Event` a pak klikněte na **vyhledávání**.</span><span class="sxs-lookup"><span data-stu-id="f2a86-245">In Search, type `Type=Event` and then click **Search**.</span></span>
2. <span data-ttu-id="f2a86-246">Klikněte na tlačítko **+ zobrazit další** v jednom z výsledků a zobrazit všechny vlastnosti, které mají výsledky.</span><span class="sxs-lookup"><span data-stu-id="f2a86-246">Click **+ show more** in one of the results to view all the properties that the results have.</span></span>
3. <span data-ttu-id="f2a86-247">Vyberte některým z nich explicitně a dotaz se změní na `Type=Event | Select Computer,EventID,RenderedDescription`.</span><span class="sxs-lookup"><span data-stu-id="f2a86-247">Select some of those explicitly, and the query changes to `Type=Event | Select Computer,EventID,RenderedDescription`.</span></span>  
    <span data-ttu-id="f2a86-248">![Vyberte vyhledávání](./media/log-analytics-log-searches/oms-search-select.png)</span><span class="sxs-lookup"><span data-stu-id="f2a86-248">![search select](./media/log-analytics-log-searches/oms-search-select.png)</span></span>

<span data-ttu-id="f2a86-249">Tento příkaz je zvláště užitečné, pokud chcete řídit výstupní vyhledávání a vyberte pouze ty části data, která skutečně vás pro vaše zkoumání, což často není úplný záznam.</span><span class="sxs-lookup"><span data-stu-id="f2a86-249">This command is particularly useful when you want to control search output and choose only the portions of data that really matter for your exploration, which often isn’t the full record.</span></span> <span data-ttu-id="f2a86-250">To je také užitečné, když mají záznamy různých typů *některé* společných vlastností, ale ne *všechny* jejich vlastnosti jsou společné.</span><span class="sxs-lookup"><span data-stu-id="f2a86-250">This is also useful when records of different types have *some* common properties, but not *all* of their properties are common.</span></span> <span data-ttu-id="f2a86-251">Můžete generovat výstup, který může vypadat více přirozeně tabulku nebo fungovat, i když exportován do souboru CSV a pak massaged v aplikaci Excel.</span><span class="sxs-lookup"><span data-stu-id="f2a86-251">The, you can generate output that looks more naturally like a table, or work well when exported to a CSV file and then massaged in Excel.</span></span>

## <a name="use-the-measure-command"></a><span data-ttu-id="f2a86-252">Použijte příkaz měr</span><span class="sxs-lookup"><span data-stu-id="f2a86-252">Use the measure command</span></span>
<span data-ttu-id="f2a86-253">MÍRA je jedním z nejvíce univerzální příkazy v analýzy protokolů hledání.</span><span class="sxs-lookup"><span data-stu-id="f2a86-253">MEASURE is one of the most versatile commands in Log Analytics searches.</span></span> <span data-ttu-id="f2a86-254">Umožňuje použít statistické *funkce* dat a agregačních výsledků seskupených podle dané pole.</span><span class="sxs-lookup"><span data-stu-id="f2a86-254">It allows you to apply statistical *functions* to your data and aggregate results grouped by a given field.</span></span> <span data-ttu-id="f2a86-255">Existuje více statistické funkce, které podporuje měr.</span><span class="sxs-lookup"><span data-stu-id="f2a86-255">There are multiple statistical functions that Measure supports.</span></span>

### <a name="measure-count"></a><span data-ttu-id="f2a86-256">Míra count()</span><span class="sxs-lookup"><span data-stu-id="f2a86-256">Measure count()</span></span>
<span data-ttu-id="f2a86-257">První statistické funkce pro práci s a jeden z nejjednodušší zjistit, je *count()* funkce.</span><span class="sxs-lookup"><span data-stu-id="f2a86-257">The first statistical function to work with, and one of the simplest to understand is the *count()* function.</span></span>

<span data-ttu-id="f2a86-258">Výsledkem všechny vyhledávací dotaz, jako `Type=Event`, zobrazeny filtry také nazývané omezující vlastnosti na levé straně výsledků vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="f2a86-258">Results from any search query such as `Type=Event`, show filters also called facets on the left side of search results.</span></span> <span data-ttu-id="f2a86-259">Filtry zobrazit distribuční hodnot pomocí dané pole pro výsledky vyhledávání provést.</span><span class="sxs-lookup"><span data-stu-id="f2a86-259">The filters show a distribution of values by a given field for the results in the search executed.</span></span>

![počet měr vyhledávání](./media/log-analytics-log-searches/oms-search-measure-count01.png)

<span data-ttu-id="f2a86-261">Například na předchozím obrázku se zobrazí **počítače** pole a uvádí, že v rámci téměř 739 tisíc události ve výsledcích, jsou 68 jedinečné a jiné hodnoty pro **počítače** pole v těchto záznamech.</span><span class="sxs-lookup"><span data-stu-id="f2a86-261">For example, in the image above you'll see the **Computer** field and it shows that within the almost 739 thousand events in the results, there are 68 unique and distinct values for the **Computer** field in those records.</span></span> <span data-ttu-id="f2a86-262">Na dlaždici se zobrazí pouze 5, které jsou nejběžnější 5 hodnot, které jsou zapsány v horní části **počítače** polí), seřazené podle počtu dokumentů, které obsahují tuto konkrétní hodnotu v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="f2a86-262">The tile only shows the top 5, which are the most common 5 values that are written in the **Computer** fields), sorted by the number of documents that contain that specific value in that field.</span></span> <span data-ttu-id="f2a86-263">Na obrázku uvidíte, že – mezi tyto události téměř 369 tisíc – 90 tisíc pocházet z počítače OpsInsights04.contoso.com, 83 tisíc z DB03.contoso.com počítače a tak dále.</span><span class="sxs-lookup"><span data-stu-id="f2a86-263">In the image you can see that – among those almost 369 thousand events – 90 thousand come from the OpsInsights04.contoso.com computer, 83 thousand from the DB03.contoso.com computer, and so on.</span></span>

<span data-ttu-id="f2a86-264">Co dělat, když chcete zobrazit všechny hodnoty, protože dlaždice se zobrazí pouze pouze první 5?</span><span class="sxs-lookup"><span data-stu-id="f2a86-264">What if you want to see all values, since the tile only shows only the top 5?</span></span>

<span data-ttu-id="f2a86-265">To je, co dělat s funkcí count() příkaz měr.</span><span class="sxs-lookup"><span data-stu-id="f2a86-265">That’s what the measure command can do with the count() function.</span></span> <span data-ttu-id="f2a86-266">Tato funkce nepoužívá žádné parametry.</span><span class="sxs-lookup"><span data-stu-id="f2a86-266">This function doesn't use any parameters.</span></span> <span data-ttu-id="f2a86-267">Můžete zadat pouze pole, podle kterého chcete seskupit podle – **počítače** pole v tomto případě:</span><span class="sxs-lookup"><span data-stu-id="f2a86-267">You just specify the field by which you want to group by – the **Computer** field in this case:</span></span>

`Type=Event | Measure count() by Computer`

![počet měr vyhledávání](./media/log-analytics-log-searches/oms-search-measure-count-computer.png)

<span data-ttu-id="f2a86-269">Ale **počítače** je právě používá pole *v* každá položka dat – nejsou spojené žádné relační databáze a neexistuje žádný samostatné **počítače** objektu kdekoli.</span><span class="sxs-lookup"><span data-stu-id="f2a86-269">However, **Computer** is just a field used *in* each piece of data – there are no relational databases involved and there is no separate **Computer** object anywhere.</span></span> <span data-ttu-id="f2a86-270">Pouze hodnoty *v* data můžete popisují, které entitě generované je a řadu dalších vlastností a aspekty dat – proto termín *omezující vlastnost*.</span><span class="sxs-lookup"><span data-stu-id="f2a86-270">Just the values *in* the data can describe which entity generated them, and a number of other characteristics and aspects of the data – hence the term *facet*.</span></span> <span data-ttu-id="f2a86-271">Však můžete stejně dobře seskupovat podle další pole.</span><span class="sxs-lookup"><span data-stu-id="f2a86-271">However, you can just as well group by other fields.</span></span> <span data-ttu-id="f2a86-272">Vzhledem k tomu, že původní výsledky téměř 739 tisíc události, které jsou přesměrovat do příkazu měr také mít pole s názvem **EventID**, můžete použít stejné techniky k seskupení podle tohoto pole a získat počet událostí podle ID události:</span><span class="sxs-lookup"><span data-stu-id="f2a86-272">Because the original results of almost 739 thousand events that are piped into the measure command also have a field called **EventID**, you can apply the same technique to group by that field and get a count of events by EventID:</span></span>

```
Type=Event | Measure count() by EventID
```

<span data-ttu-id="f2a86-273">Pokud si nejste zájem o počet skutečné záznamů, které obsahují konkrétní hodnotu, ale místo toho pokud chcete pouze seznam hodnot sami, můžete přidat *vyberte* příkaz na konci ho a právě vyberte první sloupec:</span><span class="sxs-lookup"><span data-stu-id="f2a86-273">If you're not interested in the actual record count that contain a specific value, but instead if you only want a list of the values themselves, you can add a *Select* command at the end of it and just select the first column:</span></span>

```
Type=Event | Measure count() by EventID | Select EventID
```

<span data-ttu-id="f2a86-274">Potom můžete získat komplikovanější a předem řazení výsledků v dotazu nebo klepnutí na sloupce v mřížce, příliš.</span><span class="sxs-lookup"><span data-stu-id="f2a86-274">Then you can get more intricate and pre-sort the results in the query, or you can just click the columns in the grid, too.</span></span>

```
Type=Event | Measure count() by EventID | Select EventID | Sort EventID asc
```

#### <a name="to-search-using-measure-count"></a><span data-ttu-id="f2a86-275">Hledání s použitím míru count</span><span class="sxs-lookup"><span data-stu-id="f2a86-275">To search using measure count</span></span>
* <span data-ttu-id="f2a86-276">Do pole vyhledávání dotazu zadejte`Type=Event | Measure count() by EventID`</span><span class="sxs-lookup"><span data-stu-id="f2a86-276">In the search query field, type `Type=Event | Measure count() by EventID`</span></span>
* <span data-ttu-id="f2a86-277">Připojit `| Select EventID` na konec dotazu.</span><span class="sxs-lookup"><span data-stu-id="f2a86-277">Append `| Select EventID` to the end of the query.</span></span>
* <span data-ttu-id="f2a86-278">Nakonec připojit `| Sort EventID asc` na konec dotazu.</span><span class="sxs-lookup"><span data-stu-id="f2a86-278">Finally, append `| Sort EventID asc` to the end of the query.</span></span>

<span data-ttu-id="f2a86-279">Existuje několik důležitých bodů zdůraznil a Všimněte si:</span><span class="sxs-lookup"><span data-stu-id="f2a86-279">There are a couple important points to notice and emphasize:</span></span>

<span data-ttu-id="f2a86-280">Nejprve výsledky, které vidíte nejsou původní nezpracovaná výsledky už.</span><span class="sxs-lookup"><span data-stu-id="f2a86-280">First, the results you see are not the original raw results anymore.</span></span> <span data-ttu-id="f2a86-281">Místo toho jsou agregované výsledky – v podstatě skupiny výsledků.</span><span class="sxs-lookup"><span data-stu-id="f2a86-281">Instead, they are aggregated results – essentially groups of results.</span></span> <span data-ttu-id="f2a86-282">Tato akce není problém, ale byste měli vědět, že jste interakci se příliš neliší tvaru dat, která se liší od původní nezpracovaná obrazec, který se vytvoří za chodu v důsledku agregace a statistické funkce.</span><span class="sxs-lookup"><span data-stu-id="f2a86-282">This isn't a problem, but you should understand that you're interacting with a very different shape of data that differs from the original raw shape that gets created on the fly as a result of the aggregation/statistical function.</span></span>

<span data-ttu-id="f2a86-283">Druhý, **měření počtu** aktuálně vrací hodnotu pouze prvních 100 odlišné výsledky.</span><span class="sxs-lookup"><span data-stu-id="f2a86-283">Second, **Measure count** currently returns only the top 100 distinct results.</span></span> <span data-ttu-id="f2a86-284">Toto omezení se nevztahuje na jiné statistických funkcí.</span><span class="sxs-lookup"><span data-stu-id="f2a86-284">This limit does not apply to the other statistical functions.</span></span> <span data-ttu-id="f2a86-285">Ano obvykle musíte nejprve pomocí přesnější filtru k vyhledání konkrétní položky před použitím count() měr.</span><span class="sxs-lookup"><span data-stu-id="f2a86-285">So, you'll usually need to use a more precise filter first to search for specific items before you apply measure count().</span></span>

## <a name="use-the-max-and-min-functions-with-the-measure-command"></a><span data-ttu-id="f2a86-286">Pomocí funkce max a min příkaz měr</span><span class="sxs-lookup"><span data-stu-id="f2a86-286">Use the max and min functions with the measure command</span></span>
<span data-ttu-id="f2a86-287">Existují různé scénáře, kde **měr Max()** a **měr Min()** jsou užitečné.</span><span class="sxs-lookup"><span data-stu-id="f2a86-287">There are various scenarios where **Measure Max()** and **Measure Min()** are useful.</span></span> <span data-ttu-id="f2a86-288">Ale vzhledem k tomu, že jednotlivé funkce je opačné vzájemně, jsme budete ilustrují Max() a můžete experimentovat s Min() sami.</span><span class="sxs-lookup"><span data-stu-id="f2a86-288">However, since each function is opposite of each other, we'll illustrate Max() and you can experiment with Min() on your own.</span></span>

<span data-ttu-id="f2a86-289">Pokud jste odeslat dotaz pro události zabezpečení, mají **úroveň** vlastnost, která se může lišit.</span><span class="sxs-lookup"><span data-stu-id="f2a86-289">If you query for security events, they have a **Level** property that can vary.</span></span> <span data-ttu-id="f2a86-290">Například:</span><span class="sxs-lookup"><span data-stu-id="f2a86-290">For example:</span></span>

```
Type=SecurityEvent
```

![Počáteční počet měr vyhledávání](./media/log-analytics-log-searches/oms-search-measure-max01.png)

<span data-ttu-id="f2a86-292">Pokud chcete zobrazit nejvyšší hodnotu pro všechny zabezpečení události, které jsou uvedeny běžné počítače, Seskupit podle pole, můžete použít</span><span class="sxs-lookup"><span data-stu-id="f2a86-292">If you want to view the highest value for all of the security events given a common Computer, the group by field, you can use</span></span>

```
Type=ConfigurationAlert | Measure Max(Level) by Computer
```

![hledání měr maximální počítače](./media/log-analytics-log-searches/oms-search-measure-max02.png)

<span data-ttu-id="f2a86-294">Se zobrazí, který pro počítače, které měl **úroveň** záznamy, většina z nich má aspoň 8, na úrovni mnoho měl úroveň 16.</span><span class="sxs-lookup"><span data-stu-id="f2a86-294">It will display that for the computers that had **Level** records, most of them have at least level 8, many had a level of 16.</span></span>

```
Type=ConfigurationAlert | Measure Max(Severity) by Computer
```

![Maximální doba měr vyhledávání vygenerované počítači](./media/log-analytics-log-searches/oms-search-measure-max03.png)

<span data-ttu-id="f2a86-296">Tato funkce pracuje s čísla, ale spolupracuje také s pole data a času.</span><span class="sxs-lookup"><span data-stu-id="f2a86-296">This function works well with numbers, but it also works with DateTime fields.</span></span> <span data-ttu-id="f2a86-297">Je vhodné zkontrolovat poslední nebo poslední časové razítko pro jakékoliv dat indexované pro každý počítač.</span><span class="sxs-lookup"><span data-stu-id="f2a86-297">It is useful to check for the last or most recent time stamp for any piece of data indexed for each computer.</span></span> <span data-ttu-id="f2a86-298">Například: Pokud poslední události zabezpečení ohlásil pro každý počítač?</span><span class="sxs-lookup"><span data-stu-id="f2a86-298">For example: When was the most recent security event reported for each machine?</span></span>

```
Type=ConfigurationChange | Measure Max(TimeGenerated) by Computer
```

## <a name="use-the-avg-function-with-the-measure-command"></a><span data-ttu-id="f2a86-299">Příkaz měr pomocí funkce AVG.</span><span class="sxs-lookup"><span data-stu-id="f2a86-299">Use the avg function with the measure command</span></span>
<span data-ttu-id="f2a86-300">Statistické funkce Avg() použít s měr můžete vypočítat průměrnou hodnotu pro některé pole a seskupení výsledků podle stejné nebo jiné pole.</span><span class="sxs-lookup"><span data-stu-id="f2a86-300">The Avg() statistical function used with measure allows you to calculate the average value for some field, and group results by the same or other field.</span></span> <span data-ttu-id="f2a86-301">To je užitečné v různých případech, například údaje o výkonu.</span><span class="sxs-lookup"><span data-stu-id="f2a86-301">This is useful in a variety of cases, such as performance data.</span></span>

<span data-ttu-id="f2a86-302">Začneme s údaje o výkonu.</span><span class="sxs-lookup"><span data-stu-id="f2a86-302">We'll start with performance data.</span></span> <span data-ttu-id="f2a86-303">Všimněte si, že OMS aktuálně shromažďuje čítače výkonu pro počítače s Windows a Linux.</span><span class="sxs-lookup"><span data-stu-id="f2a86-303">Note that OMS currently collects performance counters for both Windows and Linux machines.</span></span>

<span data-ttu-id="f2a86-304">K vyhledání *všechny* údaje o výkonu, je nejzákladnější dotazu:</span><span class="sxs-lookup"><span data-stu-id="f2a86-304">To search for *all* performance data, the most basic query is:</span></span>

```
Type=Perf
```

![spuštění vyhledávání průměr](./media/log-analytics-log-searches/oms-search-avg01.png)

<span data-ttu-id="f2a86-306">První věc, můžete si všimnout je, že analýzy protokolů ukazuje tři perspektivy: seznam, který ukazuje, který zobrazuje skutečné záznamy za grafy; Tabulky, který ukazuje tabulkového zobrazení data čítače výkonu; a metriky, které zobrazuje grafy pro čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="f2a86-306">The first thing you'll notice is that Log Analytics shows you three perspectives: List, which shows you which shows the actual records behind the charts; Table, which shows a tabular view of performance counter data; and Metrics, which shows charts for the performance counters.</span></span>

<span data-ttu-id="f2a86-307">Na předchozím obrázku jsou dvě sady pole označené, které označují následující:</span><span class="sxs-lookup"><span data-stu-id="f2a86-307">In the image above, there are two sets of fields marked that indicate the following:</span></span>

* <span data-ttu-id="f2a86-308">První sady identifikuje název čítače výkonu systému Windows, název objektu a název Instance filtru dotazu.</span><span class="sxs-lookup"><span data-stu-id="f2a86-308">The first set identifies Windows Performance Counter Name, Object Name, and Instance Name in the query filter.</span></span> <span data-ttu-id="f2a86-309">Jedná se o pole, které pravděpodobně budete nejčastěji používat jako omezující vlastnosti nebo filtry</span><span class="sxs-lookup"><span data-stu-id="f2a86-309">These are the fields you probably will most commonly use as facets/filters</span></span>
* <span data-ttu-id="f2a86-310">**Přepočtené** je skutečná hodnota čítače.</span><span class="sxs-lookup"><span data-stu-id="f2a86-310">**CounterValue** is the actual value of the counter.</span></span> <span data-ttu-id="f2a86-311">V tomto příkladu je hodnota *75*.</span><span class="sxs-lookup"><span data-stu-id="f2a86-311">In this example, the value is *75*.</span></span>
* <span data-ttu-id="f2a86-312">**TimeGenerated** je 12:51, ve 24hodinovém formátu.</span><span class="sxs-lookup"><span data-stu-id="f2a86-312">**TimeGenerated** is 12:51, in 24-hour time format.</span></span>

<span data-ttu-id="f2a86-313">Zde je zobrazení metriky v grafu.</span><span class="sxs-lookup"><span data-stu-id="f2a86-313">Here's a view of the metrics in a graph.</span></span>

![spuštění vyhledávání průměr](./media/log-analytics-log-searches/oms-search-avg02.png)

<span data-ttu-id="f2a86-315">Po výklad o záznamů tvaru výkonu a nutnosti přečtěte si informace o jinými technikami, hledání můžete míry Avg() k agregaci číselná data tohoto typu.</span><span class="sxs-lookup"><span data-stu-id="f2a86-315">After reading about the Perf record shape, and having read about other search techniques, you can use measure Avg() to aggregate this type of numerical data.</span></span>

<span data-ttu-id="f2a86-316">Zde je jednoduchý příklad:</span><span class="sxs-lookup"><span data-stu-id="f2a86-316">Here's a simple example:</span></span>

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" | Measure Avg(CounterValue) by Computer
```

![hledání samplevalue průměr](./media/log-analytics-log-searches/oms-search-avg03.png)

<span data-ttu-id="f2a86-318">V tomto příkladu vyberte výkonu celkový čas procesoru čítač a průměrné počítačem.</span><span class="sxs-lookup"><span data-stu-id="f2a86-318">In this example, you select the CPU Total Time performance counter and average by Computer.</span></span> <span data-ttu-id="f2a86-319">Pokud chcete zúžit výsledky, aby pouze posledních 6 hodin, můžete použít ovládací prvek filtru čas nebo v dotazu zadat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f2a86-319">If you want to narrow down your results to only the last 6 hours, you can either use the time filter control or specify in your query as follows:</span></span>

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer
```

### <a name="to-search-using-the-avg-function-with-the-measure-command"></a><span data-ttu-id="f2a86-320">K vyhledávání pomocí příkazu míry pomocí funkce AVG.</span><span class="sxs-lookup"><span data-stu-id="f2a86-320">To search using the avg function with the measure command</span></span>
* <span data-ttu-id="f2a86-321">Dotaz do vyhledávacího pole zadejte `Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer`.</span><span class="sxs-lookup"><span data-stu-id="f2a86-321">In the Search query box, type `Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer`.</span></span>

<span data-ttu-id="f2a86-322">Můžete agregovat a chybě korelace dat. *napříč* počítače.</span><span class="sxs-lookup"><span data-stu-id="f2a86-322">You can aggregate and correlate data *across* computers.</span></span> <span data-ttu-id="f2a86-323">Představte si například, že máte na skupinu hostitelů v nějaká farmy, kde každý uzel je jakékoli jiné jednu a se právě všechny stejného typu práce a zhruba vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="f2a86-323">For example, imagine that you have a set of hosts in some sort of farm where each node is equal to any other one and they just do all the same type of work and load should be roughly balanced.</span></span> <span data-ttu-id="f2a86-324">Může dojít, jejich čítače, které vše v jednom přejděte s následující dotaz a získat průměry pro celou farmu.</span><span class="sxs-lookup"><span data-stu-id="f2a86-324">You could get their counters all in one go with the following query and get averages for the entire farm.</span></span> <span data-ttu-id="f2a86-325">Můžete spustit výběrem počítače s následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="f2a86-325">You can start by choosing the computers with the following example:</span></span>

```
Type=Perf AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

<span data-ttu-id="f2a86-326">Teď, když máte počítače, také pouze chcete vybrat dvě klíčové ukazatele výkonu (KPI): % využití procesoru a % volného místa na disku.</span><span class="sxs-lookup"><span data-stu-id="f2a86-326">Now that you have the computers, you also only want to select two key performance indicators (KPIs): % CPU Usage and % Free Disk Space.</span></span> <span data-ttu-id="f2a86-327">Ano tuto část dotazu se změní na:</span><span class="sxs-lookup"><span data-stu-id="f2a86-327">So, that part of the query becomes:</span></span>

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS
```

<span data-ttu-id="f2a86-328">Teď můžete přidat počítače a čítače s v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="f2a86-328">Now you can add computers and counters with the following example:</span></span>

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

<span data-ttu-id="f2a86-329">Protože máte velmi konkrétní výběru, **měření Avg()** příkaz může vrátit průměr není počítačem, ale celou farmu, jednoduše tak, že seskupení podle název_čítače.</span><span class="sxs-lookup"><span data-stu-id="f2a86-329">Because you have a very specific selection, the **measure Avg()** command can return the average not by computer, but across the farm, simply by grouping by CounterName.</span></span> <span data-ttu-id="f2a86-330">Například:</span><span class="sxs-lookup"><span data-stu-id="f2a86-330">For example:</span></span>

```
Type=Perf  InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03") | Measure Avg(CounterValue) by CounterName
```

<span data-ttu-id="f2a86-331">To vám dává užitečné compact zobrazení několik klíčových ukazatelů výkonu vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="f2a86-331">This gives you a useful compact view of a couple of your environment's KPIs.</span></span>

![seskupení průměr vyhledávání](./media/log-analytics-log-searches/oms-search-avg04.png)

<span data-ttu-id="f2a86-333">Na řídicím panelu můžete snadno použít vyhledávací dotaz.</span><span class="sxs-lookup"><span data-stu-id="f2a86-333">You can easily use the search query in a dashboard.</span></span> <span data-ttu-id="f2a86-334">Můžete například uložit vyhledávací dotaz a vytvořit řídicí panel z něj s názvem *webové farmy klíčových ukazatelů výkonu*.</span><span class="sxs-lookup"><span data-stu-id="f2a86-334">For example, you could save the search query and create a dashboard from it named *Web Farm KPIs*.</span></span> <span data-ttu-id="f2a86-335">Další informace o použití řídicích panelů najdete v tématu [vytvořit vlastní řídicí panel v analýzy protokolů](log-analytics-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="f2a86-335">To learn more about using dashboards, see [Create a custom dashboard in Log Analytics](log-analytics-dashboards.md).</span></span>

![řídicí panel vyhledávání průměr](./media/log-analytics-log-searches/oms-search-avg05.png)

### <a name="use-the-sum-function-with-the-measure-command"></a><span data-ttu-id="f2a86-337">Příkaz míry pomocí funkce sum</span><span class="sxs-lookup"><span data-stu-id="f2a86-337">Use the sum function with the measure command</span></span>
<span data-ttu-id="f2a86-338">Funkce sum je podobná jiných funkcí příkazu měr.</span><span class="sxs-lookup"><span data-stu-id="f2a86-338">The sum function is similar to other functions of the measure command.</span></span> <span data-ttu-id="f2a86-339">Můžete zobrazit příklad o tom, jak používat funkci sum na [W3C IIS protokoly vyhledávání ve statistice provozu Microsoft Azure](http://blogs.msdn.com/b/dmuscett/archive/2014/09/20/w3c-iis-logs-search-in-system-center-advisor-limited-preview.aspx).</span><span class="sxs-lookup"><span data-stu-id="f2a86-339">You can see an example about how to use the sum function at [W3C IIS Logs Search in Microsoft Azure Operational Insights](http://blogs.msdn.com/b/dmuscett/archive/2014/09/20/w3c-iis-logs-search-in-system-center-advisor-limited-preview.aspx).</span></span>

<span data-ttu-id="f2a86-340">Můžete použít Max() a Min() s textového řetězce, hodnoty data a času a čísla.</span><span class="sxs-lookup"><span data-stu-id="f2a86-340">You can use Max() and Min() with numbers, date times and text strings.</span></span> <span data-ttu-id="f2a86-341">Pomocí textového řetězce jsou seřazeny podle abecedy a získat první a poslední.</span><span class="sxs-lookup"><span data-stu-id="f2a86-341">With text strings, they are sorted alphabetically and you get first and last.</span></span>

<span data-ttu-id="f2a86-342">Sum() však nelze použít s jakoukoli jinou hodnotu než číselné pole.</span><span class="sxs-lookup"><span data-stu-id="f2a86-342">However, you cannot use Sum() with anything other than numerical fields.</span></span> <span data-ttu-id="f2a86-343">To platí také pro Avg().</span><span class="sxs-lookup"><span data-stu-id="f2a86-343">This also applies to Avg().</span></span>

### <a name="use-the-percentile-function-with-the-measure-command"></a><span data-ttu-id="f2a86-344">Pomocí funkce percentilu pomocí příkazu míry</span><span class="sxs-lookup"><span data-stu-id="f2a86-344">Use the percentile function with the measure command</span></span>
<span data-ttu-id="f2a86-345">Funkce percentilu je podobná Avg() a Sum() v, můžete ji použít pouze pro číselné pole.</span><span class="sxs-lookup"><span data-stu-id="f2a86-345">The percentile function is similar to Avg() and Sum() in that you can only use it for numerical fields.</span></span> <span data-ttu-id="f2a86-346">Můžete použít všechny percentilu mezi 1 až 99 na číselné pole.</span><span class="sxs-lookup"><span data-stu-id="f2a86-346">You can use any percentile between 1 to 99 on a numeric field.</span></span> <span data-ttu-id="f2a86-347">Můžete také použít obě **percentilu** a **pct** příkazy.</span><span class="sxs-lookup"><span data-stu-id="f2a86-347">You can also use both **percentile** and **pct** commands.</span></span> <span data-ttu-id="f2a86-348">Zde je několik příkladů:</span><span class="sxs-lookup"><span data-stu-id="f2a86-348">Here are few examples:</span></span>  

```
Type:Perf CounterName:"DiskTransers/sec" |measure percentile95(CurrentValue) by Computer
```
```
Type:Perf ObjectName=LogicalDisk CounterName="Current Disk Queue Length" Computer="MyComputerName" | measure pct65(CurrentValue) by InstanceName
```

## <a name="use-the-where-command"></a><span data-ttu-id="f2a86-349">Použít where příkazu</span><span class="sxs-lookup"><span data-stu-id="f2a86-349">Use the where command</span></span>
<span data-ttu-id="f2a86-350">Tam, kde příkaz lze použít jako filtr, ale můžete použijí v kanálu pro další filtrování agregované výsledky, které vyrobila příkaz měr – oproti nezpracovaná výsledky, jsou filtrovány na začátku dotazu.</span><span class="sxs-lookup"><span data-stu-id="f2a86-350">The where command works like a filter, but it can be applied in the pipeline to further filter aggregated results that have been produced by a Measure command – as opposed to raw results that are filtered at the beginning of a query.</span></span>

<span data-ttu-id="f2a86-351">Například:</span><span class="sxs-lookup"><span data-stu-id="f2a86-351">For example:</span></span>

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer
```

<span data-ttu-id="f2a86-352">Můžete přidat další kanálu "|" znak a kde příkaz, který má získat pouze počítače, jejichž průměrné využití procesoru je větší než 80 %, se v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="f2a86-352">You can add another pipe "|" character and the Where command to only get computers whose average CPU is above 80%, with the following example:</span></span>

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer | Where AVGCPU>80
```

<span data-ttu-id="f2a86-353">Pokud jste obeznámeni s nástrojem Microsoft System Center - Operations Manager si můžete představit where příkazu v podmínkách management pack.</span><span class="sxs-lookup"><span data-stu-id="f2a86-353">If you're familiar with Microsoft System Center - Operations Manager, you can think of the where command in management pack terms.</span></span> <span data-ttu-id="f2a86-354">Pokud v příkladu bylo pravidlo, první část dotazu by byl zdroj dat a where by příkaz vypadal detekce stavu.</span><span class="sxs-lookup"><span data-stu-id="f2a86-354">If the example were a rule, the first part of the query would be the data source and the where command would be the condition detection.</span></span>

<span data-ttu-id="f2a86-355">Dotaz můžete použít jako dlaždici v **vlastní řídicí panel**, jako monitorování řazení zobrazíte, když jsou přetíženy počítače procesory.</span><span class="sxs-lookup"><span data-stu-id="f2a86-355">You can use the query as a tile in **My Dashboard**, as a monitor of sorts, to see when computer CPUs are over-utilized.</span></span> <span data-ttu-id="f2a86-356">Další informace o řídicí panely, najdete v části [vytvořit vlastní řídicí panel v analýzy protokolů](log-analytics-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="f2a86-356">To learn more about dashboards, see [Create a custom dashboard in Log Analytics](log-analytics-dashboards.md).</span></span> <span data-ttu-id="f2a86-357">Můžete také vytvořit a používat řídicí panely pomocí mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="f2a86-357">You can also create and use dashboards using the mobile app.</span></span> <span data-ttu-id="f2a86-358">Další informace najdete v tématu [OMS mobilní aplikace ](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865).</span><span class="sxs-lookup"><span data-stu-id="f2a86-358">For more information, see [OMS Mobile App ](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865).</span></span> <span data-ttu-id="f2a86-359">V dlaždicích dolní dva na následujícím obrázku, uvidíte monitorování zobrazí seznam, jako číslo.</span><span class="sxs-lookup"><span data-stu-id="f2a86-359">In the bottom two tiles of the following image, you can see the monitor displayed a list and as a number.</span></span> <span data-ttu-id="f2a86-360">V podstatě chcete vždy číslo, které má být nula a seznamu byly prázdné.</span><span class="sxs-lookup"><span data-stu-id="f2a86-360">Essentially, you always want the number to be zero and the list to be empty.</span></span> <span data-ttu-id="f2a86-361">Jinak hodnota označuje podmínku výstrahy.</span><span class="sxs-lookup"><span data-stu-id="f2a86-361">Otherwise, it indicates an alert condition.</span></span> <span data-ttu-id="f2a86-362">V případě potřeby můžete se podívat na které počítače jsou přetížena.</span><span class="sxs-lookup"><span data-stu-id="f2a86-362">If needed, you can use it to take a look at which machines are under pressure.</span></span>

![mobilní řídicí panel](./media/log-analytics-log-searches/oms-search-mobile.png)

## <a name="use-the-in-operator"></a><span data-ttu-id="f2a86-364">Použití v operátoru</span><span class="sxs-lookup"><span data-stu-id="f2a86-364">Use the in operator</span></span>
<span data-ttu-id="f2a86-365">*IN* operátor, spolu s *NOT IN* umožňuje používat subsearches, které jsou hledání, které obsahují další hledání jako argument.</span><span class="sxs-lookup"><span data-stu-id="f2a86-365">The *IN* operator, along with *NOT IN* allows you to use subsearches, which are searches that include another search as an argument.</span></span> <span data-ttu-id="f2a86-366">Jsou obsaženy v složené závorky {} v rámci jiného *primární* nebo *vnější* vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="f2a86-366">They are contained in braces {} within another *primary* or *outer* search.</span></span> <span data-ttu-id="f2a86-367">Výsledek subsearch, často seznam odlišné výsledky, se pak použije jako argument v jeho primární vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="f2a86-367">The result of a subsearch, often a list of distinct results, is then used as an argument in its primary search.</span></span>

<span data-ttu-id="f2a86-368">Můžete subsearches tak, aby odpovídaly podmnožiny dat, nelze popisují přímo v hledaný výraz, ale který se dá vygenerovat z vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="f2a86-368">You can use subsearches to match subsets of your data that you cannot describe directly in a search expression, but which can be generated from a search.</span></span> <span data-ttu-id="f2a86-369">Například, pokud vás zajímá pomocí jednoho hledání najít všechny události z *počítače s chybějícími aktualizacemi zabezpečení*, pak je třeba navrhnout subsearch, který nejprve identifikuje, že *počítače s chybějícími aktualizacemi zabezpečení* předtím, než nalezne události, které patří do těchto hostitelích.</span><span class="sxs-lookup"><span data-stu-id="f2a86-369">For example, if you’re interested in using one search to find all events from *computers missing security updates*, then you need to design a subsearch that first identifies that *computers missing security updates* before it finds events belonging to those hosts.</span></span>

<span data-ttu-id="f2a86-370">Ano, může express *počítače s aktuálně chybějícími požadovanými aktualizacemi zabezpečení* s následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="f2a86-370">So, you could express *computers currently missing required security updates* with the following query:</span></span>

```
Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer
```    

![V příkladu vyhledávání](./media/log-analytics-log-searches/oms-search-in01-revised.png)

<span data-ttu-id="f2a86-372">Jakmile se v seznamu, můžete jako vnitřní vyhledávání hledání ke kanálu seznam počítačů, do vnější (primární) vyhledávání, který bude hledat události pro tyto počítače.</span><span class="sxs-lookup"><span data-stu-id="f2a86-372">Once you have the list, you can use the search as an inner search to feed the list of computers into an outer (primary) search that will look for events for those computers.</span></span> <span data-ttu-id="f2a86-373">To uděláte tak, že uzavření vnitřní hledání do složených závorek a napájení své výsledky jako možné hodnoty nebo pole filtru do vnějšího vyhledávání pomocí operátor.</span><span class="sxs-lookup"><span data-stu-id="f2a86-373">You do this by enclosing the inner search in braces and feeding its results as possible values for a filter/field in the outer search using the IN operator.</span></span> <span data-ttu-id="f2a86-374">Dotaz by vypadat podobně jako:</span><span class="sxs-lookup"><span data-stu-id="f2a86-374">The query would resemble:</span></span>

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer}
```
![V příkladu vyhledávání](./media/log-analytics-log-searches/oms-search-in02-revised.png)

<span data-ttu-id="f2a86-376">Také oznámení filtr času v informacích o vnitřní hledání použít, protože vyhodnocení aktualizací systému pořídí snímek všech počítačů každých 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="f2a86-376">Also notice the time filter used in the inner search because the System Update Assessment takes a snapshot of all computers every 24 hours.</span></span> <span data-ttu-id="f2a86-377">Vnitřní dotaz můžete nastavit víc jednoduchý a přesné tak, že pouze jeden den.</span><span class="sxs-lookup"><span data-stu-id="f2a86-377">You can make the inner query more lightweight and precise by only searching for a day.</span></span> <span data-ttu-id="f2a86-378">Vnější hledání místo toho použije se čas výběr v uživatelském rozhraní načítání událostí z posledních 7 dnů.</span><span class="sxs-lookup"><span data-stu-id="f2a86-378">The outer search instead uses the time selection in the user interface, retrieving events from the last 7 days.</span></span> <span data-ttu-id="f2a86-379">V tématu [logické operátory](#boolean-operators) Další informace o času operátory.</span><span class="sxs-lookup"><span data-stu-id="f2a86-379">See [Boolean operators](#boolean-operators) for more information about time operators.</span></span>

<span data-ttu-id="f2a86-380">Protože jste skutečně používejte pouze výsledky hledání vnitřní jako filtr hodnotu pro jeden vnější, můžete také použít příkazy ve vnější vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="f2a86-380">Because you really only use the results of the inner search as a filter value for the outer one, you can still apply commands in the outer search.</span></span> <span data-ttu-id="f2a86-381">Například můžete seskupit stále výše zmíněných událostí s jiného příkazu měr:</span><span class="sxs-lookup"><span data-stu-id="f2a86-381">For example, you can still group the above events with another measure command:</span></span>

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer} | measure count() by Source
```

![V příkladu vyhledávání](./media/log-analytics-log-searches/oms-search-in03-revised.png)

<span data-ttu-id="f2a86-383">Obecně platí budete chtít vnitřní dotaz rychle spustit, protože analýzy protokolů má straně služby časové limity pro něj a také vrácení malé množství výsledků.</span><span class="sxs-lookup"><span data-stu-id="f2a86-383">Generally, you want your inner query to execute quickly because Log Analytics has service-side timeouts for it and also to return a small amount of results.</span></span> <span data-ttu-id="f2a86-384">Pokud vnitřní dotaz vrátí více výsledků, získá zkrátí seznam výsledků, které by mohly způsobit potenciálně vnější vyhledávání vrací nesprávné výsledky.</span><span class="sxs-lookup"><span data-stu-id="f2a86-384">If the inner query returns more results, the result list gets truncated, which could potentially cause the outer search to return incorrect results.</span></span>

<span data-ttu-id="f2a86-385">Jiným pravidlem je, že vnitřní hledání aktuálně nutné poskytnout *agregované* výsledky.</span><span class="sxs-lookup"><span data-stu-id="f2a86-385">Another rule is that the inner search currently needs to provide *aggregated* results.</span></span> <span data-ttu-id="f2a86-386">Jinými slovy, musí obsahovat *měr* příkaz; je nelze kanálu aktuálně nezpracovaná výsledky do vnějšího vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="f2a86-386">In other words, it must contain a *measure* command; you cannot currently feed raw results into an outer search.</span></span>

<span data-ttu-id="f2a86-387">Navíc může být pouze jeden operátor IN a musí být poslední filtru v dotazu.</span><span class="sxs-lookup"><span data-stu-id="f2a86-387">Also, there can be only one IN operator and it must be the last filter in the query.</span></span> <span data-ttu-id="f2a86-388">Nemůže být více operátorů v nebo by – to v podstatě brání spuštění více subsearches: důležité je, že pouze jeden dílčí/vnitřní hledání je možné pro každé vnější vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="f2a86-388">Multiple IN operators cannot be OR’d – this essentially prevents running multiple subsearches: the important point is that only one sub/inner search is possible for each outer search.</span></span>

<span data-ttu-id="f2a86-389">I když tyto limity IN umožňuje různé druhy korelační hledání a umožňuje definovat podobný skupin jako je například počítače, uživatele nebo soubory – ať pole ve vašich datech obsahovat.</span><span class="sxs-lookup"><span data-stu-id="f2a86-389">Even with these limits, IN enables many kinds of correlated searches, and allows you to define something similar to groups such as computers, users, or files – whatever the fields in your data contain.</span></span> <span data-ttu-id="f2a86-390">Zde jsou další příklady:</span><span class="sxs-lookup"><span data-stu-id="f2a86-390">Here are more examples:</span></span>

<span data-ttu-id="f2a86-391">**Všechny aktualizace chybí z počítačů, kde je zakázaný nastavení automatických aktualizací**</span><span class="sxs-lookup"><span data-stu-id="f2a86-391">**All updates missing from computers where Automatic Update setting is disabled**</span></span>

```
Type=Update UpdateState=Needed Optional=false Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual | measure count() by Computer} | measure count() by KBID
```

<span data-ttu-id="f2a86-392">**Všechny chybové události z počítače se systémem SQL Server (=, kde se má spustit vyhodnocení SQL)**</span><span class="sxs-lookup"><span data-stu-id="f2a86-392">**All error events from computers running SQL Server (=where SQL Assessment has run)**</span></span>

```
Type=Event EventLevelName=error Computer IN {Type=SQLAssessmentRecommendation | measure count() by Computer}
```

<span data-ttu-id="f2a86-393">**Všechny události zabezpečení z počítačů, které jsou řadiče domény se (=, kde byla spuštěna hodnocení AD)**</span><span class="sxs-lookup"><span data-stu-id="f2a86-393">**All security events from computers that are Domain Controllers (=where AD Assessment has run)**</span></span>

```
Type=SecurityEvent Computer IN { Type=ADAssessmentRecommendation | measure count() by Computer }
```

<span data-ttu-id="f2a86-394">**Které jiné účty přihlášení do stejných počítačů, kde má přihlášený účet BACONLAND\jochan?**</span><span class="sxs-lookup"><span data-stu-id="f2a86-394">**Which other accounts have logged on to the same computers where account BACONLAND\jochan has logged on?**</span></span>

```
Type=SecurityEvent EventID=4624   Account!="BACONLAND\\jochan" Computer IN { Type=SecurityEvent EventID=4624   Account="BACONLAND\\jochan" | measure count() by Computer } | measure count() by Account
```

## <a name="use-the-distinct-command"></a><span data-ttu-id="f2a86-395">Použít příkaz distinct</span><span class="sxs-lookup"><span data-stu-id="f2a86-395">Use the distinct command</span></span>
<span data-ttu-id="f2a86-396">Jak již název naznačuje, tento příkaz poskytuje seznam jedinečných hodnot pro pole.</span><span class="sxs-lookup"><span data-stu-id="f2a86-396">As the name suggests, this command provides a list of distinct values for a field.</span></span> <span data-ttu-id="f2a86-397">Je překvapivě jednoduchý, ale velmi užitečné.</span><span class="sxs-lookup"><span data-stu-id="f2a86-397">It's surprisingly simple but quite useful.</span></span> <span data-ttu-id="f2a86-398">Stejné bylo možné dosáhnout pomocí příkazu count() měr stejně, jako vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="f2a86-398">The same could be achieved with measure count() command as well, as shown below.</span></span>

```
Type=Event | Measure count() by Computer
```

![Ukázka příkazu odlišné vyhledávání](./media/log-analytics-log-searches/oms-search-distinct01-revised.png)

<span data-ttu-id="f2a86-400">Pokud však všechny vás zajímá je právě seznam jedinečných hodnot a není počet dokumentů, které mají, že může poskytovat hodnoty a potom DISTINCT čisticí a snadnější čtení výstup a kratší syntaxe, jako vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="f2a86-400">However, if all you're interested in is just a list of distinct values and not the count of documents that have that values, then DISTINCT can provide cleaner and easier to read output, and shorter syntax, as shown below.</span></span>

```
Type=Event | Distinct Computer
```
![Ukázka příkazu odlišné vyhledávání](./media/log-analytics-log-searches/oms-search-distinct02-revised.png)

## <a name="use-the-countdistinct-function-with-the-measure-command"></a><span data-ttu-id="f2a86-402">Použijte funkci countdistinct pomocí příkazu míry</span><span class="sxs-lookup"><span data-stu-id="f2a86-402">Use the countdistinct function with the measure command</span></span>
<span data-ttu-id="f2a86-403">Funkce countdistinct spočítá počet jedinečných hodnot v rámci jednotlivých skupin.</span><span class="sxs-lookup"><span data-stu-id="f2a86-403">The countdistinct function counts the number of distinct values within each group.</span></span> <span data-ttu-id="f2a86-404">Například může být použít počítat počet jedinečných počítačů, vytváření sestav pro každý typ:</span><span class="sxs-lookup"><span data-stu-id="f2a86-404">For example, it could be used to count the number of unique computers reporting for each Type:</span></span>

```
* | measure countdistinct(Computer) by Type
```

![OMS countdistinct](./media/log-analytics-log-searches/oms-countdistinct.png)

## <a name="use-the-measure-interval-command"></a><span data-ttu-id="f2a86-406">Pomocí příkazu interval měr</span><span class="sxs-lookup"><span data-stu-id="f2a86-406">Use the measure interval command</span></span>
<span data-ttu-id="f2a86-407">S téměř v reálném čase výkonu shromažďování dat, můžete shromažďovat a vizualizovat všechny čítače výkonu v analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="f2a86-407">With near real-time performance data collection, you can collect and visualize any performance counter in Log Analytics.</span></span> <span data-ttu-id="f2a86-408">Jednoduše zadat dotaz **typu: výkonu** vrátí tisíce metriky grafy na základě počtu čítače a serverů ve vašem prostředí analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="f2a86-408">Simply entering the query **Type:Perf** will return thousands of metric graphs based on the number of counters and servers in your Log Analytics environment.</span></span> <span data-ttu-id="f2a86-409">S průběhem metriky na vyžádání můžete si prohlédnout metriky celkového ve vašem prostředí na vysoké úrovni a podrobné informace na podrobnější data, jako je třeba.</span><span class="sxs-lookup"><span data-stu-id="f2a86-409">With on-demand metric aggregation, you can look at the overall metrics in your environment at a high level, and deep dive into more granular data as you need to.</span></span>

<span data-ttu-id="f2a86-410">Řekněme, že budete chtít vědět, co je průměrné využití procesoru pro všechny počítače.</span><span class="sxs-lookup"><span data-stu-id="f2a86-410">Let’s say that you want to know what is the average CPU across all your computers.</span></span> <span data-ttu-id="f2a86-411">Prohlížení průměr využití procesoru pro každý počítač nemusí být užitečné, protože může získat vyhlazené výsledky.</span><span class="sxs-lookup"><span data-stu-id="f2a86-411">Looking at the average CPU for every computer might not be helpful because results may get smoothed out.</span></span> <span data-ttu-id="f2a86-412">Aby viděl další podrobnosti, můžete agregovat sady výsledků v menší časové okno bloky a podívejte se do časové řady mezi různými dimenzemi.</span><span class="sxs-lookup"><span data-stu-id="f2a86-412">To look into more details, you can aggregate your result in a smaller time window chunks, and look into a time series across different dimensions.</span></span> <span data-ttu-id="f2a86-413">Například můžete provést po hodinách průměr využití procesoru pro všechny počítače takto:</span><span class="sxs-lookup"><span data-stu-id="f2a86-413">For example, you can perform the hourly average of CPU usage across all your computers as follows:</span></span>

```
Type:Perf CounterName="% Processor Time" InstanceName="_Total" | measure avg(CounterValue) by Computer Interval 1HOUR
```

![Průměrný interval měr](./media/log-analytics-log-searches/oms-measure-avg-interval.png)

<span data-ttu-id="f2a86-415">Ve výchozím nastavení tyto výsledky se zobrazí v řádku interaktivního více řad grafu.</span><span class="sxs-lookup"><span data-stu-id="f2a86-415">By default these results will be displayed in a multi-series interactive line chart.</span></span>  <span data-ttu-id="f2a86-416">Tento graf podporuje řady přepnutím (s změny měřítka osy y), přibližování a ukazatele.</span><span class="sxs-lookup"><span data-stu-id="f2a86-416">This chart supports series toggling (with y-axis rescaling), zooming, and hovering.</span></span>  <span data-ttu-id="f2a86-417">Možnost zobrazení tabulky je stále k dispozici pro prohlížení nezpracovaných dat v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="f2a86-417">The table display option is still available for viewing the raw data if necessary.</span></span>

<span data-ttu-id="f2a86-418">Můžete taky Seskupit podle jiných polí.</span><span class="sxs-lookup"><span data-stu-id="f2a86-418">You can also group by other fields.</span></span> <span data-ttu-id="f2a86-419">V tomto příkladu dívám se na všech čítačů v % pro jeden určitý počítač a chcete vědět, co je percentily každou hodinu 70 každých čítače:</span><span class="sxs-lookup"><span data-stu-id="f2a86-419">In this example, I am looking at all the % counters for one specific computer, and I want to know what is the hourly 70 percentiles of every counter:</span></span>

```
Type:Perf Computer=beefpatty4 CounterName=%* InstanceName=_Total | measure percentile70(CounterValue) by CounterName Interval 1HOUR
```
<span data-ttu-id="f2a86-420">Poznámka je, že tyto dotazy nejsou omezeny na čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="f2a86-420">One thing to note is that these queries are not limited to performance counters.</span></span> <span data-ttu-id="f2a86-421">Lze následně použít na všechny metriky.</span><span class="sxs-lookup"><span data-stu-id="f2a86-421">You can apply them to any metric.</span></span> <span data-ttu-id="f2a86-422">V tomto příkladu dívám se na protokoly služby IIS W3C.</span><span class="sxs-lookup"><span data-stu-id="f2a86-422">In this example, I’m looking at W3C IIS logs.</span></span> <span data-ttu-id="f2a86-423">Chci vědět, co je maximální doba, kterou trvá v intervalu 5 minut pro zpracování každé žádosti:</span><span class="sxs-lookup"><span data-stu-id="f2a86-423">I want to know what is the maximum time it takes over a 5-minute interval for processing each request:</span></span>

```
Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES
```

### <a name="use-multiple-aggregates-in-one-query"></a><span data-ttu-id="f2a86-424">Použití více agregace v jednom dotazu</span><span class="sxs-lookup"><span data-stu-id="f2a86-424">Use multiple aggregates in one query</span></span>
<span data-ttu-id="f2a86-425">Více klauzulí agregační můžete zadat v příkazu měr.</span><span class="sxs-lookup"><span data-stu-id="f2a86-425">You can specify multiple aggregate clauses in a measure command.</span></span>  <span data-ttu-id="f2a86-426">Každé z nich může být alias nezávisle.</span><span class="sxs-lookup"><span data-stu-id="f2a86-426">Each one can be aliased independently.</span></span>  <span data-ttu-id="f2a86-427">Není-li ji alias výsledný název pole bude, že agregační funkce, která se používá (tj. "avg(CounterValue)" pro avg(CounterValue)).</span><span class="sxs-lookup"><span data-stu-id="f2a86-427">If it is not given an alias the resulting field name will be the aggregate function that was used (i.e. "avg(CounterValue)" for avg(CounterValue)).</span></span>

 ```
Type=WireData | measure avg(ReceivedBytes), avg(SentBytes) by Direction interval 1hour
```
![OMS multiaggregates1](./media/log-analytics-log-searches/oms-multiaggregates1.png)

<span data-ttu-id="f2a86-429">Tady je další příklad:</span><span class="sxs-lookup"><span data-stu-id="f2a86-429">Here is another example:</span></span>

 ```
* | measure countdistinct(Computer) as Computers, count() as TotalRecords by Type
```


## <a name="next-steps"></a><span data-ttu-id="f2a86-430">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f2a86-430">Next steps</span></span>
<span data-ttu-id="f2a86-431">Další informace o protokolu hledání najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="f2a86-431">For additional information about log searches, see:</span></span>

* <span data-ttu-id="f2a86-432">Použití [vlastní pole v analýzy protokolů](log-analytics-custom-fields.md) rozšířit vyhledávání protokolu.</span><span class="sxs-lookup"><span data-stu-id="f2a86-432">Use [Custom fields in Log Analytics](log-analytics-custom-fields.md) to extend log searches.</span></span>
* <span data-ttu-id="f2a86-433">Zkontrolujte [analýzy protokolů protokolu vyhledávání odkaz](log-analytics-search-reference.md) zobrazíte všechna pole hledání a omezující vlastnosti, které jsou k dispozici v analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="f2a86-433">Review the [Log Analytics log search reference](log-analytics-search-reference.md) to view all of the search fields and facets available in Log Analytics.</span></span>
