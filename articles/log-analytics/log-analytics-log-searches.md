---
title: "aaaFind dat pomocí protokolu hledání v Azure Log Analytics | Microsoft Docs"
description: "Protokol hledání umožňují toocombine a korelovat žádná data počítače z více zdrojů ve vašem prostředí."
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
ms.openlocfilehash: 1161857a0027f05726492417362cb24a8fe21ef8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="find-data-using-log-searches-in-log-analytics"></a><span data-ttu-id="fc7be-103">Najít data pomocí protokolu hledání v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="fc7be-103">Find data using log searches in Log Analytics</span></span>

>[!NOTE]
> <span data-ttu-id="fc7be-104">Tento článek popisuje vyhledávání protokolu pomocí dotazovacího jazyka pro aktuální hello v analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="fc7be-104">This article describes log searches using hello current query language in Log Analytics.</span></span>  <span data-ttu-id="fc7be-105">Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak by měl odkazovat příliš[principy protokolu vyhledá v analýzy protokolů (Nový)](log-analytics-log-search-new.md).</span><span class="sxs-lookup"><span data-stu-id="fc7be-105">If your workspace has been upgraded toohello [new Log Analytics query language](log-analytics-log-search-upgrade.md), then you should refer too[Understanding log searches in Log Analytics (new)](log-analytics-log-search-new.md).</span></span>


<span data-ttu-id="fc7be-106">Jádrem hello analýzy protokolů je hello protokolu vyhledávací funkce, která vám umožní toocombine a korelovat žádná data počítače z více zdrojů ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="fc7be-106">At hello core of Log Analytics is hello log search feature which allows you toocombine and correlate any machine data from multiple sources within your environment.</span></span> <span data-ttu-id="fc7be-107">Řešení se také používá technologii toobring vyhledávání protokolu metriky můžete seskupit kolem oblasti konkrétní problém.</span><span class="sxs-lookup"><span data-stu-id="fc7be-107">Solutions are also powered by log search toobring you metrics pivoted around a particular problem area.</span></span>

<span data-ttu-id="fc7be-108">Na stránce hello vyhledávání můžete vytvořit dotaz a pak při hledání, můžete filtrovat výsledky hello pomocí ovládacích prvků omezující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="fc7be-108">On hello Search page, you can create a query, and then when you search, you can filter hello results by using facet controls.</span></span> <span data-ttu-id="fc7be-109">Můžete také vytvořit tootransform pokročilými dotazy, filtr a sestavy na výsledky.</span><span class="sxs-lookup"><span data-stu-id="fc7be-109">You can also create advanced queries tootransform, filter, and report on your results.</span></span>

<span data-ttu-id="fc7be-110">Běžné dotazy vyhledávání protokolu se zobrazí na většina stránek řešení.</span><span class="sxs-lookup"><span data-stu-id="fc7be-110">Common log search queries appear on most solution pages.</span></span> <span data-ttu-id="fc7be-111">V rámci konzoly hello OMS můžete kliknutím na tlačítko dlaždice nebo přejít k podrobnostem tooother položky tooview podrobnosti o položce hello pomocí protokolu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="fc7be-111">Throughout hello OMS console, you can click tiles or drill in tooother items tooview details about hello item by using log search.</span></span>

<span data-ttu-id="fc7be-112">V tomto kurzu budeme zabývat příklady toocover všechny základy hello při použití hledání protokolů.</span><span class="sxs-lookup"><span data-stu-id="fc7be-112">In this tutorial, we'll walk through examples toocover all hello basics when you use log search.</span></span>

<span data-ttu-id="fc7be-113">Jsme budete začínat jednoduchý, praktické příklady a potom stavět na je, abyste měli představu o případy praktická použití o toouse hello syntaxe tooextract hello Statistika způsob z dat hello.</span><span class="sxs-lookup"><span data-stu-id="fc7be-113">We'll start with simple, practical examples and then build on them so that you can get an understanding of practical use cases about how toouse hello syntax tooextract hello insights you want from hello data.</span></span>

<span data-ttu-id="fc7be-114">Poté, co jste obeznámeni s vyhledávání techniky, můžete zkontrolovat hello [analýzy protokolů protokolu vyhledávání odkaz](log-analytics-search-reference.md).</span><span class="sxs-lookup"><span data-stu-id="fc7be-114">After you've familiar with search techniques, you can review hello [Log Analytics log search reference](log-analytics-search-reference.md).</span></span>

## <a name="use-basic-filters"></a><span data-ttu-id="fc7be-115">Použití základní filtrů</span><span class="sxs-lookup"><span data-stu-id="fc7be-115">Use basic filters</span></span>
<span data-ttu-id="fc7be-116">Hello nejprve thing tooknow je, že hello první část dotazu vyhledávání, před spuštěním "|" znak svislé čáry znak, je vždy *filtru*.</span><span class="sxs-lookup"><span data-stu-id="fc7be-116">hello first thing tooknow is that hello first part of a search query, before any "|" vertical pipe character, is always a *filter*.</span></span> <span data-ttu-id="fc7be-117">Můžete si ho představit jako klauzule WHERE v TSQL – Určuje *co* podmnožinu toopull dat z úložiště dat hello OMS.</span><span class="sxs-lookup"><span data-stu-id="fc7be-117">You can think of it as a WHERE clause in TSQL--it determines *what* subset of data toopull out of hello OMS data store.</span></span> <span data-ttu-id="fc7be-118">Hledání v úložišti dat hello je z velké části o zadání vlastnosti hello hello dat, který chcete tooextract, takže je přirozené dotazu by začínat hello klauzule WHERE.</span><span class="sxs-lookup"><span data-stu-id="fc7be-118">Searching in hello data store is largely about specifying hello characteristics of hello data that you want tooextract, so it is natural that a query would start with hello WHERE clause.</span></span>

<span data-ttu-id="fc7be-119">Hello nejzákladnější filtry můžete použít jsou *klíčová slova*, jako je například "Chyba" nebo "časový limit nebo název počítače.</span><span class="sxs-lookup"><span data-stu-id="fc7be-119">hello most basic filters you can use are *keywords*, such as ‘error’ or ‘timeout’, or a computer name.</span></span> <span data-ttu-id="fc7be-120">Tyto typy dotazů jednoduché obecně vrátit různých tvarů dat v rámci hello stejná sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="fc7be-120">These types of simple queries generally return diverse shapes of data within hello same result set.</span></span> <span data-ttu-id="fc7be-121">Je to proto analýzy protokolů má jiný *typy* dat v systému hello.</span><span class="sxs-lookup"><span data-stu-id="fc7be-121">This is because Log Analytics has different *types* of data in hello system.</span></span>

### <a name="tooconduct-a-simple-search"></a><span data-ttu-id="fc7be-122">tooconduct jednoduché hledání</span><span class="sxs-lookup"><span data-stu-id="fc7be-122">tooconduct a simple search</span></span>
1. <span data-ttu-id="fc7be-123">Na portálu OMS hello, klikněte na tlačítko **hledání protokolů**.</span><span class="sxs-lookup"><span data-stu-id="fc7be-123">In hello OMS portal, click **Log Search**.</span></span>  
    <span data-ttu-id="fc7be-124">![dlaždice hledání](./media/log-analytics-log-searches/oms-overview-log-search.png)</span><span class="sxs-lookup"><span data-stu-id="fc7be-124">![search tile](./media/log-analytics-log-searches/oms-overview-log-search.png)</span></span>
2. <span data-ttu-id="fc7be-125">V poli hello dotazů, zadejte `error` a pak klikněte na **vyhledávání**.</span><span class="sxs-lookup"><span data-stu-id="fc7be-125">In hello query field, type `error` and then click **Search**.</span></span>  
    <span data-ttu-id="fc7be-126">![došlo k chybě hledání](./media/log-analytics-log-searches/oms-search-error.png)</span><span class="sxs-lookup"><span data-stu-id="fc7be-126">![search error](./media/log-analytics-log-searches/oms-search-error.png)</span></span>  
    <span data-ttu-id="fc7be-127">Například hello dotazu pro `error` v hello následující obrázek vrátí 100 000 **událostí** záznamy (shromažďují Správa protokolu), 18 **ConfigurationAlert** záznamů (vygenerovaných konfigurace. Vyhodnocení) a 12 **ConfigurationChange** záznamy (zachycenou hello sledování změn).</span><span class="sxs-lookup"><span data-stu-id="fc7be-127">For example, hello query for `error` in hello following image returned 100,000 **Event** records (collected by Log Management), 18 **ConfigurationAlert** records (generated by Configuration Assessment) and 12 **ConfigurationChange** records (captured by hello Change Tracking).</span></span>   
    <span data-ttu-id="fc7be-128">![výsledky hledání](./media/log-analytics-log-searches/oms-search-results01.png)</span><span class="sxs-lookup"><span data-stu-id="fc7be-128">![search results](./media/log-analytics-log-searches/oms-search-results01.png)</span></span>  

<span data-ttu-id="fc7be-129">Tyto filtry nejsou skutečně typy/tříd objektů.</span><span class="sxs-lookup"><span data-stu-id="fc7be-129">These filters are not really object types/classes.</span></span> <span data-ttu-id="fc7be-130">*Typ* je právě značku, vlastnost nebo řetězec nebo název nebo kategorii, která je připojena tooa část data.</span><span class="sxs-lookup"><span data-stu-id="fc7be-130">*Type* is just a tag, or a property, or a string/name/category, that is attached tooa piece of data.</span></span> <span data-ttu-id="fc7be-131">Některé dokumenty v systému hello jsou označené jako **typu: ConfigurationAlert** a některé jsou označené jako **typu: výkonu**, nebo **typu: událost**a tak dále.</span><span class="sxs-lookup"><span data-stu-id="fc7be-131">Some documents in hello system are tagged as **Type:ConfigurationAlert** and some are tagged as **Type:Perf**, or **Type:Event**, and so on.</span></span> <span data-ttu-id="fc7be-132">Každý výsledek hledání, dokument, záznamu nebo položka zobrazí všechny vlastnosti nezpracovaná hello a jejich hodnoty pro každou z těchto položek dat, a můžete použít tyto názvy toospecify pole ve filtru hello Pokud chcete pouze záznamy hello tooretrieve kde hello pole má, který přiřazen hodnota.</span><span class="sxs-lookup"><span data-stu-id="fc7be-132">Each search result, document, record, or entry displays all hello raw properties and their values for each of those pieces of data, and you can use those field names toospecify in hello filter when you want tooretrieve only hello records where hello field has that given value.</span></span>

<span data-ttu-id="fc7be-133">*Typ* je pouze pole, které mají všechny záznamy, není liší od jiné pole.</span><span class="sxs-lookup"><span data-stu-id="fc7be-133">*Type* is really just a field that all records have, it is not different from any other field.</span></span> <span data-ttu-id="fc7be-134">To bylo vytvořeno na základě hodnoty hello hello typ pole.</span><span class="sxs-lookup"><span data-stu-id="fc7be-134">This was established based on hello value of hello Type field.</span></span> <span data-ttu-id="fc7be-135">Tento záznam bude mít různých formě.</span><span class="sxs-lookup"><span data-stu-id="fc7be-135">That record will have a different shape or form.</span></span> <span data-ttu-id="fc7be-136">Náhodně **typ = výkonu**, nebo **typ = událostí** je také hello syntaxe je třeba toolearn tooquery pro data výkonu nebo události.</span><span class="sxs-lookup"><span data-stu-id="fc7be-136">Incidentally, **Type=Perf**, or **Type=Event** is also hello syntax that you need toolearn tooquery for performance data or events.</span></span>

<span data-ttu-id="fc7be-137">Po název pole hello a před hello hodnotu, můžete použít dvojtečkou (:) nebo symbol rovná se (=).</span><span class="sxs-lookup"><span data-stu-id="fc7be-137">You can use either a colon (:) or an equal sign (=) after hello field name and before hello value.</span></span> <span data-ttu-id="fc7be-138">**Typ: událost** a **typ = událostí** odpovídají v význam, můžete zvolit hello styl dáváte přednost.</span><span class="sxs-lookup"><span data-stu-id="fc7be-138">**Type:Event** and **Type=Event** are equivalent in meaning, you can choose hello style you prefer.</span></span>

<span data-ttu-id="fc7be-139">Ano, pokud hello zadejte = výkonu záznamy mají pole s názvem "název_čítače, a pak můžete napsat dotaz tvaru `Type=Perf CounterName="% Processor Time"`.</span><span class="sxs-lookup"><span data-stu-id="fc7be-139">So, if hello Type=Perf records have a field called 'CounterName', then you can write a query resembling `Type=Perf CounterName="% Processor Time"`.</span></span>

<span data-ttu-id="fc7be-140">Tím získáte pouze údaje o výkonu hello kde název čítače výkonu hello je "% času procesoru".</span><span class="sxs-lookup"><span data-stu-id="fc7be-140">This will give you only hello performance data where hello performance counter name is "% Processor Time".</span></span>

### <a name="toosearch-for-processor-time-performance-data"></a><span data-ttu-id="fc7be-141">toosearch pro data výkonu času procesoru</span><span class="sxs-lookup"><span data-stu-id="fc7be-141">toosearch for processor time performance data</span></span>
* <span data-ttu-id="fc7be-142">Do pole vyhledávání dotazu hello zadejte`Type=Perf CounterName="% Processor Time"`</span><span class="sxs-lookup"><span data-stu-id="fc7be-142">In hello search query field, type `Type=Perf CounterName="% Processor Time"`</span></span>

<span data-ttu-id="fc7be-143">Můžete také vybrat určité a použít **InstanceName = _ "Celkem"** v hello dotazu, který je čítačů výkonu systému Windows.</span><span class="sxs-lookup"><span data-stu-id="fc7be-143">You can also be more specific and use **InstanceName=_'Total'** in hello query, which is a Windows performance counter.</span></span> <span data-ttu-id="fc7be-144">Můžete také vybrat omezující vlastnost a jiné **: hodnota pole**.</span><span class="sxs-lookup"><span data-stu-id="fc7be-144">You can also select a facet and another **field:value**.</span></span> <span data-ttu-id="fc7be-145">Filtr Hello je automaticky přidán filtr tooyour panelu hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="fc7be-145">hello filter is automatically added tooyour filter in hello query bar.</span></span> <span data-ttu-id="fc7be-146">Uvidíte to v hello následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="fc7be-146">You can see this in hello following image.</span></span> <span data-ttu-id="fc7be-147">Zobrazuje kde tooclick tooadd **InstanceName: "_Total"** toohello dotazu, aniž by museli zadávat nic.</span><span class="sxs-lookup"><span data-stu-id="fc7be-147">It shows you where tooclick tooadd **InstanceName:’_Total’** toohello query without typing anything.</span></span>

![omezující vlastnost vyhledávání](./media/log-analytics-log-searches/oms-search-facet.png)

<span data-ttu-id="fc7be-149">Stává dotazu`Type=Perf CounterName="% Processor Time" InstanceName="_Total"`</span><span class="sxs-lookup"><span data-stu-id="fc7be-149">Your query now becomes `Type=Perf CounterName="% Processor Time" InstanceName="_Total"`</span></span>

<span data-ttu-id="fc7be-150">V tomto příkladu nemáte toospecify **typ = výkonu** tooget toothis výsledek.</span><span class="sxs-lookup"><span data-stu-id="fc7be-150">In this example, you don't have toospecify **Type=Perf** tooget toothis result.</span></span> <span data-ttu-id="fc7be-151">Protože hello pole název_čítače a InstanceName existovat pouze pro záznamy typu = výkonu, hello dotazu je dost konkrétní tooreturn hello stejné výsledky jako hello déle, předchozí jeden:</span><span class="sxs-lookup"><span data-stu-id="fc7be-151">Because hello fields CounterName and InstanceName only exist for records of Type=Perf, hello query is specific enough tooreturn hello same results as hello longer, previous one:</span></span>

```
CounterName="% Processor Time" InstanceName="_Total"
```

<span data-ttu-id="fc7be-152">Důvodem je, že všechny filtry hello v dotazu hello, jsou vyhodnoceny jako používán *a* mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="fc7be-152">This is because all hello filters in hello query are evaluated as being in *AND* with each other.</span></span> <span data-ttu-id="fc7be-153">Prakticky hello další pole přidat toohello kritéria, můžete získat menší, konkrétní a přesnějších výsledků.</span><span class="sxs-lookup"><span data-stu-id="fc7be-153">Effectively, hello more fields you add toohello criteria, you get less, more specific and refined results.</span></span>

<span data-ttu-id="fc7be-154">Například hello dotazu `Type=Event EventLog="Windows PowerShell"` je stejný jako příliš`Type=Event AND EventLog="Windows PowerShell"`.</span><span class="sxs-lookup"><span data-stu-id="fc7be-154">For example, hello query `Type=Event EventLog="Windows PowerShell"` is identical too`Type=Event AND EventLog="Windows PowerShell"`.</span></span> <span data-ttu-id="fc7be-155">Vrátí všechny události, které byly přihlášení a shromažďují z protokolu událostí hello prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fc7be-155">It returns all events that were logged in and collected from hello Windows PowerShell event log.</span></span> <span data-ttu-id="fc7be-156">Pokud filtr přidáte opakovaně výběrem hello stejné charakteristiky, pak problém hello je čistě kosmetické – ho může zaplnit panelu Hledat text hello, ale stále vrátí hello stejné výsledky protože implicitní operátor AND hello je vždycky k dispozici více než jednou.</span><span class="sxs-lookup"><span data-stu-id="fc7be-156">If you add a filter multiple times by repeatedly selecting hello same facet, then hello issue is purely cosmetic--it might clutter hello Search bar, but it still returns hello same results because hello implicit AND operator is always there.</span></span>

<span data-ttu-id="fc7be-157">Implicitní operátor AND hello můžete snadno vrátit explicitně pomocí operátoru NOT.</span><span class="sxs-lookup"><span data-stu-id="fc7be-157">You can easily reverse hello implicit AND operator by using a NOT operator explicitly.</span></span> <span data-ttu-id="fc7be-158">Například:</span><span class="sxs-lookup"><span data-stu-id="fc7be-158">For example:</span></span>

<span data-ttu-id="fc7be-159">`Type:Event NOT(EventLog:"Windows PowerShell")`nebo jeho ekvivalent `Type=Event EventLog!="Windows PowerShell"` vrátí všechny události z všechny protokoly, které nejsou protokolu hello prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fc7be-159">`Type:Event NOT(EventLog:"Windows PowerShell")` or its equivalent `Type=Event EventLog!="Windows PowerShell"` return all events from all other logs that are NOT hello Windows PowerShell log.</span></span>

<span data-ttu-id="fc7be-160">Nebo můžete například použít jiné logický operátor 'Nebo'.</span><span class="sxs-lookup"><span data-stu-id="fc7be-160">Or, you can use other Boolean operator such as ‘OR’.</span></span> <span data-ttu-id="fc7be-161">Hello následující dotaz vrátí záznamy, pro které hello protokolu událostí buď systém nebo aplikace.</span><span class="sxs-lookup"><span data-stu-id="fc7be-161">hello following query returns records for which hello EventLog is either Application OR System.</span></span>

```
EventLog=Application OR EventLog=System
```

<span data-ttu-id="fc7be-162">Pomocí hello výše dotazu, získáte položky pro oba protokoly v hello stejná sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="fc7be-162">Using hello above query, you’ll get entries for both logs in hello same result set.</span></span>

<span data-ttu-id="fc7be-163">Ale pokud odeberete hello nebo můžete nechat hello implicitní a na místě, pak hello následující dotaz nebude vytváří výsledky protože není k dispozici položku protokolu událostí, které patří tooBOTH protokoly.</span><span class="sxs-lookup"><span data-stu-id="fc7be-163">However, if you remove hello OR by leaving hello implicit AND in place, then hello following query will not produce any results because there isn’t an event log entry that belongs tooBOTH logs.</span></span> <span data-ttu-id="fc7be-164">Každá položka protokolu událostí byla zapsána tooonly mezi dva protokoly hello.</span><span class="sxs-lookup"><span data-stu-id="fc7be-164">Each event log entry was written tooonly one of hello two logs.</span></span>

```
EventLog=Application EventLog=System
```


## <a name="use-additional-filters"></a><span data-ttu-id="fc7be-165">Použít další filtry</span><span class="sxs-lookup"><span data-stu-id="fc7be-165">Use additional filters</span></span>
<span data-ttu-id="fc7be-166">Hello následující dotaz vrátí položky pro 2 protokoly událostí pro všechny počítače hello, které jste odeslali data.</span><span class="sxs-lookup"><span data-stu-id="fc7be-166">hello following query returns entries for 2 event logs for all hello computers that have sent data.</span></span>

```
EventLog=Application OR EventLog=System
```

![Výsledky vyhledávání](./media/log-analytics-log-searches/oms-search-results03.png)

<span data-ttu-id="fc7be-168">Výběrem jedné z pole hello nebo filtry zúží hello dotazu tooa konkrétní počítač, s výjimkou všechny ostatní výsledky.</span><span class="sxs-lookup"><span data-stu-id="fc7be-168">Selecting one of hello fields or filters will narrow hello query tooa specific computer, excluding all other ones.</span></span> <span data-ttu-id="fc7be-169">Výsledný dotaz Hello by vypadat podobně jako následující hello.</span><span class="sxs-lookup"><span data-stu-id="fc7be-169">hello resulting query would resemble hello following.</span></span>

```
EventLog=Application OR EventLog=System Computer=SERVER1.contoso.com
```

<span data-ttu-id="fc7be-170">Ekvivalentní toohello následující příkaz, který je z důvodu hello implicitní operátorem a.</span><span class="sxs-lookup"><span data-stu-id="fc7be-170">Which is equivalent toohello following, because of hello implicit AND.</span></span>

```
EventLog=Application OR EventLog=System AND Computer=SERVER1.contoso.com
```

<span data-ttu-id="fc7be-171">Každý dotaz je vyhodnocován v hello následující explicitní pořadí.</span><span class="sxs-lookup"><span data-stu-id="fc7be-171">Each query is evaluated in hello following explicit order.</span></span> <span data-ttu-id="fc7be-172">Poznámka: hello závorky.</span><span class="sxs-lookup"><span data-stu-id="fc7be-172">Note hello parenthesis.</span></span>

```
(EventLog=Application OR EventLog=System) AND Computer=SERVER1.contoso.com
```

<span data-ttu-id="fc7be-173">Stejně jako pole hello protokolu událostí, je možné načíst data pouze pro sadu konkrétní počítače přidáním nebo.</span><span class="sxs-lookup"><span data-stu-id="fc7be-173">Just like hello event log field, you can retrieve data only for a set of specific computers by adding OR.</span></span> <span data-ttu-id="fc7be-174">Například:</span><span class="sxs-lookup"><span data-stu-id="fc7be-174">For example:</span></span>

```
(EventLog=Application OR EventLog=System) AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com OR Computer=SERVER3.contoso.com)
```

<span data-ttu-id="fc7be-175">Podobně platí, tento hello následující dotaz vrátí **% času procesoru** pro hello vybrané pouze dva počítače.</span><span class="sxs-lookup"><span data-stu-id="fc7be-175">Similarly, this hello following query return **% CPU Time** for hello selected two computers only.</span></span>

```
CounterName="% Processor Time"  AND InstanceName="_Total" AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com)
```

### <a name="field-types"></a><span data-ttu-id="fc7be-176">Typy polí</span><span class="sxs-lookup"><span data-stu-id="fc7be-176">Field types</span></span>
<span data-ttu-id="fc7be-177">Při vytváření filtrů, byste měli porozumět hello rozdíly při práci s různými typy pole vrácené protokolu hledání.</span><span class="sxs-lookup"><span data-stu-id="fc7be-177">When creating filters, you should understand hello differences in working with different types of fields returned by log searches.</span></span>

<span data-ttu-id="fc7be-178">**Prohledatelná pole** zobrazit modře ve výsledcích hledání.</span><span class="sxs-lookup"><span data-stu-id="fc7be-178">**Searchable fields** show in blue in search results.</span></span>  <span data-ttu-id="fc7be-179">Prohledatelná pole můžete použít v poli konkrétní toohello podmínek vyhledávání například hello následující:</span><span class="sxs-lookup"><span data-stu-id="fc7be-179">You can use searchable fields in search conditions specific toohello field such as hello following:</span></span>

```
Type: Event EventLevelName: "Error"
Type: SecurityEvent Computer:Contains("contoso.com")
Type: Event EventLevelName IN {"Error","Warning"}
```

<span data-ttu-id="fc7be-180">**Uvolněte prohledatelná pole text** jsou uvedeny šedě ve výsledcích hledání.</span><span class="sxs-lookup"><span data-stu-id="fc7be-180">**Free text searchable fields** are shown in grey in search results.</span></span>  <span data-ttu-id="fc7be-181">Je nelze použít s konkrétní toohello pole podmínky hledání jako prohledávatelné pole.</span><span class="sxs-lookup"><span data-stu-id="fc7be-181">They cannot be used with search conditions specific toohello field like searchable fields.</span></span>  <span data-ttu-id="fc7be-182">Jejich prohledávání pouze při provádění dotazu ve všech oblastech, jako je například následující hello.</span><span class="sxs-lookup"><span data-stu-id="fc7be-182">They are only searched when performing a query across all fields such as hello following.</span></span>

```
"Error"
Type: Event "Exception"
```


### <a name="boolean-operators"></a><span data-ttu-id="fc7be-183">Logické operátory</span><span class="sxs-lookup"><span data-stu-id="fc7be-183">Boolean operators</span></span>
<span data-ttu-id="fc7be-184">Číselná pole a data a času, můžete vyhledat pomocí hodnoty *větší než*, *menší než*, a *menší než nebo rovna*.</span><span class="sxs-lookup"><span data-stu-id="fc7be-184">With datetime and numeric fields, you can search for values using *greater than*, *lesser than*, and *lesser than or equal*.</span></span> <span data-ttu-id="fc7be-185">Jednoduché operátory můžete použít jako >, <>, =, < =,! = v panelu vyhledávání dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="fc7be-185">You can use simple operators such as >, < , >=, <= , != in hello query search bar.</span></span>

<span data-ttu-id="fc7be-186">Pro určité časové období se můžete dotazovat specifickém protokolu událostí.</span><span class="sxs-lookup"><span data-stu-id="fc7be-186">You can query a specific event log for a specific period of time.</span></span> <span data-ttu-id="fc7be-187">Například hello je vyjádřen posledních 24 hodin s hello následující klávesovými výraz.</span><span class="sxs-lookup"><span data-stu-id="fc7be-187">For example, hello last 24 hours is expressed with hello following mnemonic expression.</span></span>

```
EventLog=System TimeGenerated>NOW-24HOURS
```


#### <a name="toosearch-using-a-boolean-operator"></a><span data-ttu-id="fc7be-188">toosearch pomocí logický operátor</span><span class="sxs-lookup"><span data-stu-id="fc7be-188">toosearch using a boolean operator</span></span>
* <span data-ttu-id="fc7be-189">Do pole vyhledávání dotazu hello zadejte`EventLog=System TimeGenerated>NOW-24HOURS`</span><span class="sxs-lookup"><span data-stu-id="fc7be-189">In hello search query field, type `EventLog=System TimeGenerated>NOW-24HOURS`</span></span>  
    <span data-ttu-id="fc7be-190">![hledání s logická hodnota](./media/log-analytics-log-searches/oms-search-boolean.png)</span><span class="sxs-lookup"><span data-stu-id="fc7be-190">![search with boolean](./media/log-analytics-log-searches/oms-search-boolean.png)</span></span>

<span data-ttu-id="fc7be-191">I když můžete řídit hello časový interval graficky a většinu doby můžete chtít toodo, že existují výhody tooincluding filtr času přímo do dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="fc7be-191">Although you can control hello time interval graphically, and most times you might want toodo that, there are advantages tooincluding a time filter directly into hello query.</span></span> <span data-ttu-id="fc7be-192">Například to vyhovující postup pomocí řídicích panelů, kde můžete přepsat hello čas pro každou dlaždici, bez ohledu na to hello *globální* selektor čas na stránce řídicího panelu hello.</span><span class="sxs-lookup"><span data-stu-id="fc7be-192">For example, this works great with dashboards where you can override hello time for each tile, regardless of hello *global* time selector on hello dashboard page.</span></span> <span data-ttu-id="fc7be-193">Další informace najdete v tématu [čas záleží na řídicím panelu](http://cloudadministrator.wordpress.com/2014/10/19/system-center-advisor-restarted-time-matters-in-dashboard-part-6/).</span><span class="sxs-lookup"><span data-stu-id="fc7be-193">For more information, see [Time Matters in Dashboard](http://cloudadministrator.wordpress.com/2014/10/19/system-center-advisor-restarted-time-matters-in-dashboard-part-6/).</span></span>

<span data-ttu-id="fc7be-194">Při filtrování podle času, mějte na paměti, získat výsledky pro hello *průnik* z hello dva časových období: hello uvedené na portálu OMS hello (S1) a hello jeden zadaný v dotazu hello (S2).</span><span class="sxs-lookup"><span data-stu-id="fc7be-194">When filtering by time, keep in mind that you get results for hello *intersection* of hello two time periods: hello one specified in hello OMS portal (S1) and hello one specified in hello query (S2).</span></span>

![průnik](./media/log-analytics-log-searches/oms-search-intersection.png)

<span data-ttu-id="fc7be-196">To znamená, pokud hello časových období nekříží, například na portálu OMS hello, kde si zvolíte **tato týdnu** a v dotazu hello, kde můžete definovat **minulého týdne**, nejsou k dispozici žádné průnik a vám nebude Zobrazí všechny výsledky.</span><span class="sxs-lookup"><span data-stu-id="fc7be-196">This means, if hello time periods don’t intersect, for example in hello OMS portal where you choose **This week** and in hello query where you define **last week**, then there is no intersection and you won't receive any results.</span></span>

<span data-ttu-id="fc7be-197">Operátory porovnání použitý pro hello TimeGenerated pole jsou také užitečné v jiných situacích.</span><span class="sxs-lookup"><span data-stu-id="fc7be-197">Comparison operators used for hello TimeGenerated field are also useful in other situations.</span></span> <span data-ttu-id="fc7be-198">Například s číselná pole.</span><span class="sxs-lookup"><span data-stu-id="fc7be-198">For example, with numeric fields.</span></span>

<span data-ttu-id="fc7be-199">Například uděleno, že konfigurace Assessment výstrahy mají hello následující hodnoty závažnosti:</span><span class="sxs-lookup"><span data-stu-id="fc7be-199">For example, given that Configuration Assessment’s alerts have hello following severity values:</span></span>

* <span data-ttu-id="fc7be-200">0 = informace</span><span class="sxs-lookup"><span data-stu-id="fc7be-200">0 = Information</span></span>
* <span data-ttu-id="fc7be-201">1 = upozornění</span><span class="sxs-lookup"><span data-stu-id="fc7be-201">1 = Warning</span></span>
* <span data-ttu-id="fc7be-202">2 = kritický</span><span class="sxs-lookup"><span data-stu-id="fc7be-202">2 = Critical</span></span>

<span data-ttu-id="fc7be-203">Můžete dotazovat na upozornění a kritickou výstrahy a také vyloučit informační ty, které jsou s hello následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="fc7be-203">You can query for both warning and critical alerts and also exclude informational ones with hello following query:</span></span>

```
Type=ConfigurationAlert  Severity>=1
```


<span data-ttu-id="fc7be-204">Můžete také použít dotazy na rozsah.</span><span class="sxs-lookup"><span data-stu-id="fc7be-204">You can also use range queries.</span></span> <span data-ttu-id="fc7be-205">To znamená, že může poskytovat hello začátek a konec rozsahu hodnot v pořadí.</span><span class="sxs-lookup"><span data-stu-id="fc7be-205">This means that you can provide hello beginning and end range of values in a sequence.</span></span> <span data-ttu-id="fc7be-206">Například pokud chcete zobrazit události z protokolu událostí nástroje Operations Manager hello kde hello ID události je větší než nebo rovna too2100, ale není větší než. 2199, pak hello následující dotaz vrátí je.</span><span class="sxs-lookup"><span data-stu-id="fc7be-206">For example, if you want events from hello Operations Manager event log where hello EventID is greater than or equal too2100 but not greater than 2199, then hello following query would return them.</span></span>

```
Type=Event EventLog="Operations Manager" EventID:[2100..2199]
```


> [!NOTE]
> <span data-ttu-id="fc7be-207">Syntaxe rozsah Hello je nutné použít je oddělovače: hodnota pole hello dvojtečkou (:) a *není* hello rovná se (=).</span><span class="sxs-lookup"><span data-stu-id="fc7be-207">hello range syntax you must use is hello colon (:) field:value separator and *not* hello equal sign (=).</span></span> <span data-ttu-id="fc7be-208">Uzavřete hello horní a dolní konec rozsahu hello do hranatých závorek a je oddělit dvě tečky (.).</span><span class="sxs-lookup"><span data-stu-id="fc7be-208">Enclose hello lower and upper end of hello range in square brackets and separate them with two periods (..).</span></span>
>
>

## <a name="manipulate-search-results"></a><span data-ttu-id="fc7be-209">Manipulace s výsledky hledání</span><span class="sxs-lookup"><span data-stu-id="fc7be-209">Manipulate search results</span></span>
<span data-ttu-id="fc7be-210">Pokud chcete vyhledat data, budete má toorefine vyhledávací dotaz a mít funkční úroveň kontroly nad hello výsledky.</span><span class="sxs-lookup"><span data-stu-id="fc7be-210">When you're searching for data, you'll want toorefine your search query and have a good level of control over hello results.</span></span> <span data-ttu-id="fc7be-211">Pokud jsou načteny výsledků, můžete použít příkazy tootransform je.</span><span class="sxs-lookup"><span data-stu-id="fc7be-211">When results are retrieved, you can apply commands tootransform them.</span></span>

<span data-ttu-id="fc7be-212">Příkazy v analýzy protokolů hledání *musí* podle za znak hello znak svislé čáry (|).</span><span class="sxs-lookup"><span data-stu-id="fc7be-212">Commands in Log Analytics searches *must* follow after hello vertical pipe character (|).</span></span> <span data-ttu-id="fc7be-213">Filtr musí být vždy hello první část řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="fc7be-213">A filter must always be hello first part of a query string.</span></span> <span data-ttu-id="fc7be-214">Definuje hello datové sady, kterou právě pracujete s a následně "prostřednictvím kanálu předá" výsledků do příkazu.</span><span class="sxs-lookup"><span data-stu-id="fc7be-214">It defines hello data set you're working with and then "pipes" those results into a command.</span></span> <span data-ttu-id="fc7be-215">Pak můžete použít další příkazy tooadd hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="fc7be-215">You can then use hello pipe tooadd additional commands.</span></span> <span data-ttu-id="fc7be-216">Toto je volně podobné toohello zřetězením příkazů Windows Powershellu.</span><span class="sxs-lookup"><span data-stu-id="fc7be-216">This is loosely similar toohello Windows PowerShell pipeline.</span></span>

<span data-ttu-id="fc7be-217">Obecně platí, hello analýzy protokolů hledání jazyk pokusí toofollow prostředí PowerShell styl a pokyny toomake it podobné toohello IT Profesionálové a tooease hello křivku.</span><span class="sxs-lookup"><span data-stu-id="fc7be-217">In general, hello Log Analytics search language tries toofollow PowerShell style and guidelines toomake it similar toohello IT pros, and tooease hello learning curve.</span></span>

<span data-ttu-id="fc7be-218">Příkazy mají názvy příkazů, takže lze snadno určit, co dělají.</span><span class="sxs-lookup"><span data-stu-id="fc7be-218">Commands have names of verbs so you can easily tell what they do.</span></span>  

### <a name="sort"></a><span data-ttu-id="fc7be-219">Seřadit</span><span class="sxs-lookup"><span data-stu-id="fc7be-219">Sort</span></span>
<span data-ttu-id="fc7be-220">příkaz Hello řazení vám umožní toodefine hello řazení order by – jeden nebo více polí.</span><span class="sxs-lookup"><span data-stu-id="fc7be-220">hello sort command allows you toodefine hello sorting order by one or multiple fields.</span></span> <span data-ttu-id="fc7be-221">I když nepoužijete, ve výchozím nastavení, se vynucuje čas sestupném pořadí.</span><span class="sxs-lookup"><span data-stu-id="fc7be-221">Even if you don’t use it, by default, a time descending order is enforced.</span></span> <span data-ttu-id="fc7be-222">výsledky nejnovější Hello jsou vždy v horní části hello výsledků vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="fc7be-222">hello most recent results are always at hello top of search results.</span></span> <span data-ttu-id="fc7be-223">To znamená, že při spuštění vyhledávání, s `Type=Event EventID=1234` co je pro vás skutečně proveden je:</span><span class="sxs-lookup"><span data-stu-id="fc7be-223">This means that when you run a search, with `Type=Event EventID=1234` what really is executed for you is:</span></span>

```
Type=Event EventID=1234 **| Sort TimeGenerated desc**
```

<span data-ttu-id="fc7be-224">Je to způsobeno je hello typ možnosti, které jste se seznámili s v protokolech.</span><span class="sxs-lookup"><span data-stu-id="fc7be-224">That's because it is hello type of experience you are familiar with in logs.</span></span> <span data-ttu-id="fc7be-225">Například v prohlížeči událostí systému Windows hello.</span><span class="sxs-lookup"><span data-stu-id="fc7be-225">For example, in hello Windows Event Viewer.</span></span>

<span data-ttu-id="fc7be-226">Můžete použít řazení toochange hello způsob výsledky se vrátí.</span><span class="sxs-lookup"><span data-stu-id="fc7be-226">You can use Sort toochange hello way results are returned.</span></span> <span data-ttu-id="fc7be-227">Hello následující příklady ukazují, jak to funguje.</span><span class="sxs-lookup"><span data-stu-id="fc7be-227">hello following examples show how this works.</span></span>

```
Type=Event EventID=1234 | Sort TimeGenerated asc
```

```
Type=Event EventID=1234 | Sort Computer asc
```

```
Type=Event EventID=1234 | Sort Computer asc,TimeGenerated desc
```


<span data-ttu-id="fc7be-228">Hello jednoduché výše uvedených příkladech můžete fungování příkazy – se změní hello tvar hello výsledky, které hello filtr vrátil.</span><span class="sxs-lookup"><span data-stu-id="fc7be-228">hello simple examples above show you how commands work--they change hello shape of hello results that hello filter returned.</span></span>

### <a name="limit-and-top"></a><span data-ttu-id="fc7be-229">Omezení a nahoře</span><span class="sxs-lookup"><span data-stu-id="fc7be-229">Limit and top</span></span>
<span data-ttu-id="fc7be-230">Jiného méně známé příkazu je omezení.</span><span class="sxs-lookup"><span data-stu-id="fc7be-230">Another less known command is LIMIT.</span></span> <span data-ttu-id="fc7be-231">Limit je příkaz prostředí PowerShell jako.</span><span class="sxs-lookup"><span data-stu-id="fc7be-231">Limit is a PowerShell-like verb.</span></span> <span data-ttu-id="fc7be-232">Limit je funkčně identické toohello HORNÍM příkazovém.</span><span class="sxs-lookup"><span data-stu-id="fc7be-232">Limit is functionally identical toohello TOP command.</span></span> <span data-ttu-id="fc7be-233">Hello následující dotazy vrátí hello stejné výsledky.</span><span class="sxs-lookup"><span data-stu-id="fc7be-233">hello following queries return hello same results.</span></span>

```
Type=Event EventID=600 | Limit 1
```

```
Type=Event EventID=600 | Top 1
```


#### <a name="toosearch-using-top"></a><span data-ttu-id="fc7be-234">toosearch pomocí nahoře</span><span class="sxs-lookup"><span data-stu-id="fc7be-234">toosearch using top</span></span>
* <span data-ttu-id="fc7be-235">Do pole vyhledávání dotazu hello zadejte`Type=Event EventID=600 | Top 1` </span><span class="sxs-lookup"><span data-stu-id="fc7be-235">In hello search query field, type `Type=Event EventID=600 | Top 1` </span></span>  
    <span data-ttu-id="fc7be-236">![horní vyhledávání](./media/log-analytics-log-searches/oms-search-top.png)</span><span class="sxs-lookup"><span data-stu-id="fc7be-236">![search top](./media/log-analytics-log-searches/oms-search-top.png)</span></span>

<span data-ttu-id="fc7be-237">V obrázku hello výše, jsou 358 tisíc záznamy s ID události = 600.</span><span class="sxs-lookup"><span data-stu-id="fc7be-237">In hello image above, there are 358 thousand records with EventID=600.</span></span> <span data-ttu-id="fc7be-238">Hello polí, omezující vlastnosti a filtry na hello zbývajících vždy zobrazit informace o hello výsledky vrácené *pomocí filtru část hello* hello dotazu, která je součástí hello před všechny znakem.</span><span class="sxs-lookup"><span data-stu-id="fc7be-238">hello fields, facets, and filters on hello left always show information about hello results returned *by hello filter portion* of hello query, which is hello part before any pipe character.</span></span> <span data-ttu-id="fc7be-239">Hello **výsledky** podokně pouze vrátí výsledek hello poslední 1, protože hello Ukázkový příkaz ve tvaru a transformovat hello výsledky.</span><span class="sxs-lookup"><span data-stu-id="fc7be-239">hello **Results** pane only returns hello most recent 1 result, because hello example command shaped and transformed hello results.</span></span>

### <a name="select"></a><span data-ttu-id="fc7be-240">Vyberte</span><span class="sxs-lookup"><span data-stu-id="fc7be-240">Select</span></span>
<span data-ttu-id="fc7be-241">Vyberte příkaz Hello se chová jako Select-Object v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fc7be-241">hello SELECT command behaves like Select-Object in PowerShell.</span></span> <span data-ttu-id="fc7be-242">Vrátí filtrované výsledky, které nemají všechny své původní vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="fc7be-242">It returns filtered results that do not have all of their original properties.</span></span> <span data-ttu-id="fc7be-243">Místo toho vybere pouze hello vlastnosti, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="fc7be-243">Instead, it selects only hello properties that you specify.</span></span>

#### <a name="toorun-a-search-using-hello-select-command"></a><span data-ttu-id="fc7be-244">toorun a vyhledávání pomocí příkazu select hello</span><span class="sxs-lookup"><span data-stu-id="fc7be-244">toorun a search using hello select command</span></span>
1. <span data-ttu-id="fc7be-245">Do pole hledání zadejte `Type=Event` a pak klikněte na **vyhledávání**.</span><span class="sxs-lookup"><span data-stu-id="fc7be-245">In Search, type `Type=Event` and then click **Search**.</span></span>
2. <span data-ttu-id="fc7be-246">Klikněte na tlačítko **+ zobrazit další** v jednom z tooview výsledky hello mají všechny vlastnosti hello hello výsledky.</span><span class="sxs-lookup"><span data-stu-id="fc7be-246">Click **+ show more** in one of hello results tooview all hello properties that hello results have.</span></span>
3. <span data-ttu-id="fc7be-247">Vyberte některým z nich explicitně a hello změny dotazů příliš`Type=Event | Select Computer,EventID,RenderedDescription`.</span><span class="sxs-lookup"><span data-stu-id="fc7be-247">Select some of those explicitly, and hello query changes too`Type=Event | Select Computer,EventID,RenderedDescription`.</span></span>  
    <span data-ttu-id="fc7be-248">![Vyberte vyhledávání](./media/log-analytics-log-searches/oms-search-select.png)</span><span class="sxs-lookup"><span data-stu-id="fc7be-248">![search select](./media/log-analytics-log-searches/oms-search-select.png)</span></span>

<span data-ttu-id="fc7be-249">Tento příkaz je zvláště užitečné, když chcete toocontrol vyhledávání výstup a vybrat jenom hello části data, která skutečně vás pro vaše zkoumání, což často není úplný záznam hello.</span><span class="sxs-lookup"><span data-stu-id="fc7be-249">This command is particularly useful when you want toocontrol search output and choose only hello portions of data that really matter for your exploration, which often isn’t hello full record.</span></span> <span data-ttu-id="fc7be-250">To je také užitečné, když mají záznamy různých typů *některé* společných vlastností, ale ne *všechny* jejich vlastnosti jsou společné.</span><span class="sxs-lookup"><span data-stu-id="fc7be-250">This is also useful when records of different types have *some* common properties, but not *all* of their properties are common.</span></span> <span data-ttu-id="fc7be-251">Můžete generovat výstup, který může vypadat více přirozeně tabulku nebo fungovat, i když exportovaný soubor CSV tooa a pak massaged v aplikaci Excel.</span><span class="sxs-lookup"><span data-stu-id="fc7be-251">The, you can generate output that looks more naturally like a table, or work well when exported tooa CSV file and then massaged in Excel.</span></span>

## <a name="use-hello-measure-command"></a><span data-ttu-id="fc7be-252">Použijte příkaz měr hello</span><span class="sxs-lookup"><span data-stu-id="fc7be-252">Use hello measure command</span></span>
<span data-ttu-id="fc7be-253">MÍRA je jedním z hello nejvíce univerzální příkazy v analýzy protokolů hledání.</span><span class="sxs-lookup"><span data-stu-id="fc7be-253">MEASURE is one of hello most versatile commands in Log Analytics searches.</span></span> <span data-ttu-id="fc7be-254">Umožňuje vám tooapply statistické *funkce* tooyour dat a agregačních výsledků seskupených podle dané pole.</span><span class="sxs-lookup"><span data-stu-id="fc7be-254">It allows you tooapply statistical *functions* tooyour data and aggregate results grouped by a given field.</span></span> <span data-ttu-id="fc7be-255">Existuje více statistické funkce, které podporuje měr.</span><span class="sxs-lookup"><span data-stu-id="fc7be-255">There are multiple statistical functions that Measure supports.</span></span>

### <a name="measure-count"></a><span data-ttu-id="fc7be-256">Míra count()</span><span class="sxs-lookup"><span data-stu-id="fc7be-256">Measure count()</span></span>
<span data-ttu-id="fc7be-257">Hello první toowork statistické funkce s a jedním z nejjednodušší toounderstand hello je hello *count()* funkce.</span><span class="sxs-lookup"><span data-stu-id="fc7be-257">hello first statistical function toowork with, and one of hello simplest toounderstand is hello *count()* function.</span></span>

<span data-ttu-id="fc7be-258">Výsledkem všechny vyhledávací dotaz, jako `Type=Event`, zobrazeny filtry na levé straně výsledků vyhledávání hello zkratka omezující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="fc7be-258">Results from any search query such as `Type=Event`, show filters also called facets on hello left side of search results.</span></span> <span data-ttu-id="fc7be-259">distribuce hodnoty podle dané pole hello výsledků zobrazí Hello filtry v hledání hello provést.</span><span class="sxs-lookup"><span data-stu-id="fc7be-259">hello filters show a distribution of values by a given field for hello results in hello search executed.</span></span>

![počet měr vyhledávání](./media/log-analytics-log-searches/oms-search-measure-count01.png)

<span data-ttu-id="fc7be-261">Například v hello obrázku výše je zobrazí hello **počítače** pole a uvádí, že v rámci hello téměř 739 tisíc událostí ve výsledcích hello, jsou 68 jedinečné a jiné hodnoty pro hello **počítače** pole v těchto záznamech.</span><span class="sxs-lookup"><span data-stu-id="fc7be-261">For example, in hello image above you'll see hello **Computer** field and it shows that within hello almost 739 thousand events in hello results, there are 68 unique and distinct values for hello **Computer** field in those records.</span></span> <span data-ttu-id="fc7be-262">dlaždice Hello se zobrazí pouze hello top 5, který jsou hello nejběžnější 5 hodnoty, které jsou napsané v hello **počítače** polí), seřazené podle hello počet dokumentů, které obsahují tuto konkrétní hodnotu v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="fc7be-262">hello tile only shows hello top 5, which are hello most common 5 values that are written in hello **Computer** fields), sorted by hello number of documents that contain that specific value in that field.</span></span> <span data-ttu-id="fc7be-263">V bitové kopii hello uvidíte, že – mezi tyto události téměř 369 tisíc – 90 tisíc pocházet z hello OpsInsights04.contoso.com počítači 83 tisíc z hello DB03.contoso.com počítače a tak dále.</span><span class="sxs-lookup"><span data-stu-id="fc7be-263">In hello image you can see that – among those almost 369 thousand events – 90 thousand come from hello OpsInsights04.contoso.com computer, 83 thousand from hello DB03.contoso.com computer, and so on.</span></span>

<span data-ttu-id="fc7be-264">Co dělat, když chcete toosee všechny hodnoty, protože hello dlaždice se zobrazí pouze pouze hello top 5?</span><span class="sxs-lookup"><span data-stu-id="fc7be-264">What if you want toosee all values, since hello tile only shows only hello top 5?</span></span>

<span data-ttu-id="fc7be-265">Jaké hello měr se příkaz můžete udělat pomocí funkce count() hello.</span><span class="sxs-lookup"><span data-stu-id="fc7be-265">That’s what hello measure command can do with hello count() function.</span></span> <span data-ttu-id="fc7be-266">Tato funkce nepoužívá žádné parametry.</span><span class="sxs-lookup"><span data-stu-id="fc7be-266">This function doesn't use any parameters.</span></span> <span data-ttu-id="fc7be-267">Můžete zadat pouze hello pole, podle kterého chcete pomocí – hello toogroup **počítače** pole v tomto případě:</span><span class="sxs-lookup"><span data-stu-id="fc7be-267">You just specify hello field by which you want toogroup by – hello **Computer** field in this case:</span></span>

`Type=Event | Measure count() by Computer`

![počet měr vyhledávání](./media/log-analytics-log-searches/oms-search-measure-count-computer.png)

<span data-ttu-id="fc7be-269">Ale **počítače** je právě používá pole *v* každá položka dat – nejsou spojené žádné relační databáze a neexistuje žádný samostatné **počítače** objektu kdekoli.</span><span class="sxs-lookup"><span data-stu-id="fc7be-269">However, **Computer** is just a field used *in* each piece of data – there are no relational databases involved and there is no separate **Computer** object anywhere.</span></span> <span data-ttu-id="fc7be-270">Právě hello hodnoty *v* hello popíše dat generované které entitě je a řadu dalších vlastností a aspektů hello dat – proto hello termín *omezující vlastnost*.</span><span class="sxs-lookup"><span data-stu-id="fc7be-270">Just hello values *in* hello data can describe which entity generated them, and a number of other characteristics and aspects of hello data – hence hello term *facet*.</span></span> <span data-ttu-id="fc7be-271">Však můžete stejně dobře seskupovat podle další pole.</span><span class="sxs-lookup"><span data-stu-id="fc7be-271">However, you can just as well group by other fields.</span></span> <span data-ttu-id="fc7be-272">Protože hello původní výsledky téměř 739 tisíc události, které jsou přesměrovat do příkazu hello měr také obsahovat pole s názvem **EventID**, můžete použít hello stejným způsobem toogroup podle tohoto pole a získat počet událostí podle ID události:</span><span class="sxs-lookup"><span data-stu-id="fc7be-272">Because hello original results of almost 739 thousand events that are piped into hello measure command also have a field called **EventID**, you can apply hello same technique toogroup by that field and get a count of events by EventID:</span></span>

```
Type=Event | Measure count() by EventID
```

<span data-ttu-id="fc7be-273">Pokud si nejste zájem o počet hello skutečné záznamů, které obsahují konkrétní hodnotu, ale místo toho pokud chcete pouze seznam hello hodnoty sami, můžete přidat *vyberte* příkaz na konci hello ho a první sloupec právě vyberte hello:</span><span class="sxs-lookup"><span data-stu-id="fc7be-273">If you're not interested in hello actual record count that contain a specific value, but instead if you only want a list of hello values themselves, you can add a *Select* command at hello end of it and just select hello first column:</span></span>

```
Type=Event | Measure count() by EventID | Select EventID
```

<span data-ttu-id="fc7be-274">Potom můžete získat komplikovanější a předem řazení výsledků hello v dotazu hello nebo klepnutí hello sloupce v mřížce hello příliš.</span><span class="sxs-lookup"><span data-stu-id="fc7be-274">Then you can get more intricate and pre-sort hello results in hello query, or you can just click hello columns in hello grid, too.</span></span>

```
Type=Event | Measure count() by EventID | Select EventID | Sort EventID asc
```

#### <a name="toosearch-using-measure-count"></a><span data-ttu-id="fc7be-275">toosearch pomocí míru count</span><span class="sxs-lookup"><span data-stu-id="fc7be-275">toosearch using measure count</span></span>
* <span data-ttu-id="fc7be-276">Do pole vyhledávání dotazu hello zadejte`Type=Event | Measure count() by EventID`</span><span class="sxs-lookup"><span data-stu-id="fc7be-276">In hello search query field, type `Type=Event | Measure count() by EventID`</span></span>
* <span data-ttu-id="fc7be-277">Připojit `| Select EventID` toohello konec hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="fc7be-277">Append `| Select EventID` toohello end of hello query.</span></span>
* <span data-ttu-id="fc7be-278">Nakonec připojit `| Sort EventID asc` toohello konec hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="fc7be-278">Finally, append `| Sort EventID asc` toohello end of hello query.</span></span>

<span data-ttu-id="fc7be-279">Existuje několik důležitých bodů toonotice a zdůraznil:</span><span class="sxs-lookup"><span data-stu-id="fc7be-279">There are a couple important points toonotice and emphasize:</span></span>

<span data-ttu-id="fc7be-280">Nejprve hello výsledky, které vidíte nejsou původní nezpracovaná výsledky hello už.</span><span class="sxs-lookup"><span data-stu-id="fc7be-280">First, hello results you see are not hello original raw results anymore.</span></span> <span data-ttu-id="fc7be-281">Místo toho jsou agregované výsledky – v podstatě skupiny výsledků.</span><span class="sxs-lookup"><span data-stu-id="fc7be-281">Instead, they are aggregated results – essentially groups of results.</span></span> <span data-ttu-id="fc7be-282">Tato akce není problém, ale byste měli vědět, že jste interakci se příliš neliší tvaru dat, která se liší od hello původní nezpracovaná tvar, který se vytvoří v chodu hello v důsledku hello agregace a statistické funkce.</span><span class="sxs-lookup"><span data-stu-id="fc7be-282">This isn't a problem, but you should understand that you're interacting with a very different shape of data that differs from hello original raw shape that gets created on hello fly as a result of hello aggregation/statistical function.</span></span>

<span data-ttu-id="fc7be-283">Druhý, **měření počtu** aktuálně vrátí pouze hello nejvyšší 100 odlišné výsledky.</span><span class="sxs-lookup"><span data-stu-id="fc7be-283">Second, **Measure count** currently returns only hello top 100 distinct results.</span></span> <span data-ttu-id="fc7be-284">Toto omezení neplatí toohello jiných statistických funkcí.</span><span class="sxs-lookup"><span data-stu-id="fc7be-284">This limit does not apply toohello other statistical functions.</span></span> <span data-ttu-id="fc7be-285">Ano obvykle musíte toouse přesnější toosearch první filtr pro konkrétní položky před použitím count() měr.</span><span class="sxs-lookup"><span data-stu-id="fc7be-285">So, you'll usually need toouse a more precise filter first toosearch for specific items before you apply measure count().</span></span>

## <a name="use-hello-max-and-min-functions-with-hello-measure-command"></a><span data-ttu-id="fc7be-286">Pomocí funkce max a min hello příkazem měr hello</span><span class="sxs-lookup"><span data-stu-id="fc7be-286">Use hello max and min functions with hello measure command</span></span>
<span data-ttu-id="fc7be-287">Existují různé scénáře, kde **měr Max()** a **měr Min()** jsou užitečné.</span><span class="sxs-lookup"><span data-stu-id="fc7be-287">There are various scenarios where **Measure Max()** and **Measure Min()** are useful.</span></span> <span data-ttu-id="fc7be-288">Ale vzhledem k tomu, že jednotlivé funkce je opačné vzájemně, jsme budete ilustrují Max() a můžete experimentovat s Min() sami.</span><span class="sxs-lookup"><span data-stu-id="fc7be-288">However, since each function is opposite of each other, we'll illustrate Max() and you can experiment with Min() on your own.</span></span>

<span data-ttu-id="fc7be-289">Pokud jste odeslat dotaz pro události zabezpečení, mají **úroveň** vlastnost, která se může lišit.</span><span class="sxs-lookup"><span data-stu-id="fc7be-289">If you query for security events, they have a **Level** property that can vary.</span></span> <span data-ttu-id="fc7be-290">Například:</span><span class="sxs-lookup"><span data-stu-id="fc7be-290">For example:</span></span>

```
Type=SecurityEvent
```

![Počáteční počet měr vyhledávání](./media/log-analytics-log-searches/oms-search-measure-max01.png)

<span data-ttu-id="fc7be-292">Pokud chcete pro všechny hello zabezpečení tooview hello nejvyšší hodnotu události, které jsou uvedeny běžné počítače, hello Seskupit podle pole, můžete použít</span><span class="sxs-lookup"><span data-stu-id="fc7be-292">If you want tooview hello highest value for all of hello security events given a common Computer, hello group by field, you can use</span></span>

```
Type=ConfigurationAlert | Measure Max(Level) by Computer
```

![hledání měr maximální počítače](./media/log-analytics-log-searches/oms-search-measure-max02.png)

<span data-ttu-id="fc7be-294">Se zobrazí, který pro hello počítačů, které obsahovaly **úroveň** záznamy, většina z nich má aspoň 8, na úrovni mnoho měl úroveň 16.</span><span class="sxs-lookup"><span data-stu-id="fc7be-294">It will display that for hello computers that had **Level** records, most of them have at least level 8, many had a level of 16.</span></span>

```
Type=ConfigurationAlert | Measure Max(Severity) by Computer
```

![Maximální doba měr vyhledávání vygenerované počítači](./media/log-analytics-log-searches/oms-search-measure-max03.png)

<span data-ttu-id="fc7be-296">Tato funkce pracuje s čísla, ale spolupracuje také s pole data a času.</span><span class="sxs-lookup"><span data-stu-id="fc7be-296">This function works well with numbers, but it also works with DateTime fields.</span></span> <span data-ttu-id="fc7be-297">Je užitečné toocheck pro hello poslední nebo poslední časové razítko pro jakékoliv dat indexované pro každý počítač.</span><span class="sxs-lookup"><span data-stu-id="fc7be-297">It is useful toocheck for hello last or most recent time stamp for any piece of data indexed for each computer.</span></span> <span data-ttu-id="fc7be-298">Například: Pokud nejnovější událostí zabezpečení hello ohlásil pro každý počítač?</span><span class="sxs-lookup"><span data-stu-id="fc7be-298">For example: When was hello most recent security event reported for each machine?</span></span>

```
Type=ConfigurationChange | Measure Max(TimeGenerated) by Computer
```

## <a name="use-hello-avg-function-with-hello-measure-command"></a><span data-ttu-id="fc7be-299">Pomocí funkce AVG. hello příkazem měr hello</span><span class="sxs-lookup"><span data-stu-id="fc7be-299">Use hello avg function with hello measure command</span></span>
<span data-ttu-id="fc7be-300">Hello Avg() použít s měr statistické funkce vám umožní toocalculate hello průměrnou hodnotu pro některé pole a skupiny výsledků podle hello stejné nebo jiné pole.</span><span class="sxs-lookup"><span data-stu-id="fc7be-300">hello Avg() statistical function used with measure allows you toocalculate hello average value for some field, and group results by hello same or other field.</span></span> <span data-ttu-id="fc7be-301">To je užitečné v různých případech, například údaje o výkonu.</span><span class="sxs-lookup"><span data-stu-id="fc7be-301">This is useful in a variety of cases, such as performance data.</span></span>

<span data-ttu-id="fc7be-302">Začneme s údaje o výkonu.</span><span class="sxs-lookup"><span data-stu-id="fc7be-302">We'll start with performance data.</span></span> <span data-ttu-id="fc7be-303">Všimněte si, že OMS aktuálně shromažďuje čítače výkonu pro počítače s Windows a Linux.</span><span class="sxs-lookup"><span data-stu-id="fc7be-303">Note that OMS currently collects performance counters for both Windows and Linux machines.</span></span>

<span data-ttu-id="fc7be-304">toosearch pro *všechny* údaje o výkonu, nejzákladnější dotaz hello je:</span><span class="sxs-lookup"><span data-stu-id="fc7be-304">toosearch for *all* performance data, hello most basic query is:</span></span>

```
Type=Perf
```

![spuštění vyhledávání průměr](./media/log-analytics-log-searches/oms-search-avg01.png)

<span data-ttu-id="fc7be-306">Hello první věcí, můžete si všimnout je, že analýzy protokolů ukazuje tři perspektivy: seznam, který ukazuje, který zobrazuje skutečné záznamy hello za hello grafy; Tabulky, který ukazuje tabulkového zobrazení data čítače výkonu; a metriky, které zobrazuje grafy pro hello čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="fc7be-306">hello first thing you'll notice is that Log Analytics shows you three perspectives: List, which shows you which shows hello actual records behind hello charts; Table, which shows a tabular view of performance counter data; and Metrics, which shows charts for hello performance counters.</span></span>

<span data-ttu-id="fc7be-307">V obrázku hello výše existují dvě sady pole označené, které indikují hello následující:</span><span class="sxs-lookup"><span data-stu-id="fc7be-307">In hello image above, there are two sets of fields marked that indicate hello following:</span></span>

* <span data-ttu-id="fc7be-308">První sada Hello identifikuje název čítače výkonu systému Windows, název objektu a název Instance ve filtru dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="fc7be-308">hello first set identifies Windows Performance Counter Name, Object Name, and Instance Name in hello query filter.</span></span> <span data-ttu-id="fc7be-309">Jedná se o hello pole, které pravděpodobně budete nejčastěji používat jako omezující vlastnosti nebo filtry</span><span class="sxs-lookup"><span data-stu-id="fc7be-309">These are hello fields you probably will most commonly use as facets/filters</span></span>
* <span data-ttu-id="fc7be-310">**Přepočtené** je hello skutečná hodnota čítače hello.</span><span class="sxs-lookup"><span data-stu-id="fc7be-310">**CounterValue** is hello actual value of hello counter.</span></span> <span data-ttu-id="fc7be-311">V tomto příkladu je hodnota hello *75*.</span><span class="sxs-lookup"><span data-stu-id="fc7be-311">In this example, hello value is *75*.</span></span>
* <span data-ttu-id="fc7be-312">**TimeGenerated** je 12:51, ve 24hodinovém formátu.</span><span class="sxs-lookup"><span data-stu-id="fc7be-312">**TimeGenerated** is 12:51, in 24-hour time format.</span></span>

<span data-ttu-id="fc7be-313">Zde je zobrazení hello metriky v grafu.</span><span class="sxs-lookup"><span data-stu-id="fc7be-313">Here's a view of hello metrics in a graph.</span></span>

![spuštění vyhledávání průměr](./media/log-analytics-log-searches/oms-search-avg02.png)

<span data-ttu-id="fc7be-315">Po výklad o hello výkonu záznam tvar a nutnosti přečtěte si informace o jinými technikami, hledání můžete míry Avg() tooaggregate číselná data tohoto typu.</span><span class="sxs-lookup"><span data-stu-id="fc7be-315">After reading about hello Perf record shape, and having read about other search techniques, you can use measure Avg() tooaggregate this type of numerical data.</span></span>

<span data-ttu-id="fc7be-316">Zde je jednoduchý příklad:</span><span class="sxs-lookup"><span data-stu-id="fc7be-316">Here's a simple example:</span></span>

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" | Measure Avg(CounterValue) by Computer
```

![hledání samplevalue průměr](./media/log-analytics-log-searches/oms-search-avg03.png)

<span data-ttu-id="fc7be-318">V tomto příkladu vyberte čítače výkonu hello celkový čas procesoru a průměr počítačem.</span><span class="sxs-lookup"><span data-stu-id="fc7be-318">In this example, you select hello CPU Total Time performance counter and average by Computer.</span></span> <span data-ttu-id="fc7be-319">Pokud chcete toonarrow dolů vaše výsledky tooonly hello posledních 6 hodin, můžete použít ovládací prvek filtru čas hello nebo zadat v dotazu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fc7be-319">If you want toonarrow down your results tooonly hello last 6 hours, you can either use hello time filter control or specify in your query as follows:</span></span>

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer
```

### <a name="toosearch-using-hello-avg-function-with-hello-measure-command"></a><span data-ttu-id="fc7be-320">pomocí funkce AVG. hello hello měr příkaz toosearch</span><span class="sxs-lookup"><span data-stu-id="fc7be-320">toosearch using hello avg function with hello measure command</span></span>
* <span data-ttu-id="fc7be-321">Hello vyhledávacího dotazu pole, zadejte `Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer`.</span><span class="sxs-lookup"><span data-stu-id="fc7be-321">In hello Search query box, type `Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer`.</span></span>

<span data-ttu-id="fc7be-322">Můžete agregovat a chybě korelace dat. *napříč* počítače.</span><span class="sxs-lookup"><span data-stu-id="fc7be-322">You can aggregate and correlate data *across* computers.</span></span> <span data-ttu-id="fc7be-323">Představte si například, že máte na skupinu hostitelů v nějaká farmy, kde každý uzel je rovna tooany jiného a stejně tak všechny hello přibližně rovnoměrně stejný typ pracovní a zatížení.</span><span class="sxs-lookup"><span data-stu-id="fc7be-323">For example, imagine that you have a set of hosts in some sort of farm where each node is equal tooany other one and they just do all hello same type of work and load should be roughly balanced.</span></span> <span data-ttu-id="fc7be-324">Může dojít, jejich čítače, které vše v jednom přejděte s hello následující dotaz a získat průměry pro celou farmu hello.</span><span class="sxs-lookup"><span data-stu-id="fc7be-324">You could get their counters all in one go with hello following query and get averages for hello entire farm.</span></span> <span data-ttu-id="fc7be-325">Můžete spustit výběrem hello počítače s hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="fc7be-325">You can start by choosing hello computers with hello following example:</span></span>

```
Type=Perf AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

<span data-ttu-id="fc7be-326">Teď, když máte hello počítače, také pouze chcete tooselect dvě klíčové ukazatele výkonu (KPI): % využití procesoru a % volného místa na disku.</span><span class="sxs-lookup"><span data-stu-id="fc7be-326">Now that you have hello computers, you also only want tooselect two key performance indicators (KPIs): % CPU Usage and % Free Disk Space.</span></span> <span data-ttu-id="fc7be-327">Ano tuto část dotazu hello se změní na:</span><span class="sxs-lookup"><span data-stu-id="fc7be-327">So, that part of hello query becomes:</span></span>

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS
```

<span data-ttu-id="fc7be-328">Teď můžete přidat počítače a čítače s hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="fc7be-328">Now you can add computers and counters with hello following example:</span></span>

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

<span data-ttu-id="fc7be-329">Protože máte velmi specifických výběrových, hello **měření Avg()** příkaz může vrátit hello průměr není počítačem, ale napříč hello farmy, jednoduše tak, že seskupení podle název_čítače.</span><span class="sxs-lookup"><span data-stu-id="fc7be-329">Because you have a very specific selection, hello **measure Avg()** command can return hello average not by computer, but across hello farm, simply by grouping by CounterName.</span></span> <span data-ttu-id="fc7be-330">Například:</span><span class="sxs-lookup"><span data-stu-id="fc7be-330">For example:</span></span>

```
Type=Perf  InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03") | Measure Avg(CounterValue) by CounterName
```

<span data-ttu-id="fc7be-331">To vám dává užitečné compact zobrazení několik klíčových ukazatelů výkonu vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="fc7be-331">This gives you a useful compact view of a couple of your environment's KPIs.</span></span>

![seskupení průměr vyhledávání](./media/log-analytics-log-searches/oms-search-avg04.png)

<span data-ttu-id="fc7be-333">Na řídicím panelu můžete snadno použít hello vyhledávací dotaz.</span><span class="sxs-lookup"><span data-stu-id="fc7be-333">You can easily use hello search query in a dashboard.</span></span> <span data-ttu-id="fc7be-334">Můžete například uložit hello vyhledávací dotaz a vytvořit řídicí panel z něj s názvem *webové farmy klíčových ukazatelů výkonu*.</span><span class="sxs-lookup"><span data-stu-id="fc7be-334">For example, you could save hello search query and create a dashboard from it named *Web Farm KPIs*.</span></span> <span data-ttu-id="fc7be-335">toolearn Další informace o použití řídicích panelů, najdete v části [vytvořit vlastní řídicí panel v analýzy protokolů](log-analytics-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="fc7be-335">toolearn more about using dashboards, see [Create a custom dashboard in Log Analytics](log-analytics-dashboards.md).</span></span>

![řídicí panel vyhledávání průměr](./media/log-analytics-log-searches/oms-search-avg05.png)

### <a name="use-hello-sum-function-with-hello-measure-command"></a><span data-ttu-id="fc7be-337">Pomocí funkce sum hello příkazem měr hello</span><span class="sxs-lookup"><span data-stu-id="fc7be-337">Use hello sum function with hello measure command</span></span>
<span data-ttu-id="fc7be-338">Funkce sum Hello je podobné funkce jako tooother hello měr příkazu.</span><span class="sxs-lookup"><span data-stu-id="fc7be-338">hello sum function is similar tooother functions of hello measure command.</span></span> <span data-ttu-id="fc7be-339">Můžete zobrazit příklad o jak toouse hello funkce sum na [W3C IIS protokoly vyhledávání ve statistice provozu Microsoft Azure](http://blogs.msdn.com/b/dmuscett/archive/2014/09/20/w3c-iis-logs-search-in-system-center-advisor-limited-preview.aspx).</span><span class="sxs-lookup"><span data-stu-id="fc7be-339">You can see an example about how toouse hello sum function at [W3C IIS Logs Search in Microsoft Azure Operational Insights](http://blogs.msdn.com/b/dmuscett/archive/2014/09/20/w3c-iis-logs-search-in-system-center-advisor-limited-preview.aspx).</span></span>

<span data-ttu-id="fc7be-340">Můžete použít Max() a Min() s textového řetězce, hodnoty data a času a čísla.</span><span class="sxs-lookup"><span data-stu-id="fc7be-340">You can use Max() and Min() with numbers, date times and text strings.</span></span> <span data-ttu-id="fc7be-341">Pomocí textového řetězce jsou seřazeny podle abecedy a získat první a poslední.</span><span class="sxs-lookup"><span data-stu-id="fc7be-341">With text strings, they are sorted alphabetically and you get first and last.</span></span>

<span data-ttu-id="fc7be-342">Sum() však nelze použít s jakoukoli jinou hodnotu než číselné pole.</span><span class="sxs-lookup"><span data-stu-id="fc7be-342">However, you cannot use Sum() with anything other than numerical fields.</span></span> <span data-ttu-id="fc7be-343">To platí také tooAvg().</span><span class="sxs-lookup"><span data-stu-id="fc7be-343">This also applies tooAvg().</span></span>

### <a name="use-hello-percentile-function-with-hello-measure-command"></a><span data-ttu-id="fc7be-344">Pomocí funkce percentilu hello příkazem měr hello</span><span class="sxs-lookup"><span data-stu-id="fc7be-344">Use hello percentile function with hello measure command</span></span>
<span data-ttu-id="fc7be-345">Funkce percentilu Hello je podobné tooAvg() a Sum(), můžete ji použít pouze pro číselné pole.</span><span class="sxs-lookup"><span data-stu-id="fc7be-345">hello percentile function is similar tooAvg() and Sum() in that you can only use it for numerical fields.</span></span> <span data-ttu-id="fc7be-346">Můžete použít všechny percentilu mezi 1 too99 na číselné pole.</span><span class="sxs-lookup"><span data-stu-id="fc7be-346">You can use any percentile between 1 too99 on a numeric field.</span></span> <span data-ttu-id="fc7be-347">Můžete také použít obě **percentilu** a **pct** příkazy.</span><span class="sxs-lookup"><span data-stu-id="fc7be-347">You can also use both **percentile** and **pct** commands.</span></span> <span data-ttu-id="fc7be-348">Zde je několik příkladů:</span><span class="sxs-lookup"><span data-stu-id="fc7be-348">Here are few examples:</span></span>  

```
Type:Perf CounterName:"DiskTransers/sec" |measure percentile95(CurrentValue) by Computer
```
```
Type:Perf ObjectName=LogicalDisk CounterName="Current Disk Queue Length" Computer="MyComputerName" | measure pct65(CurrentValue) by InstanceName
```

## <a name="use-hello-where-command"></a><span data-ttu-id="fc7be-349">Pokud je příkaz používejte hello</span><span class="sxs-lookup"><span data-stu-id="fc7be-349">Use hello where command</span></span>
<span data-ttu-id="fc7be-350">Dobrý den, kdy příkaz lze použít jako filtr, ale ji jde použít ve filtru toofurther kanálu hello agregován výsledky, které je tvořen příkazem měr – jako názvem na rozdíl od tooraw výsledky, které jsou filtrovány na začátku hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="fc7be-350">hello where command works like a filter, but it can be applied in hello pipeline toofurther filter aggregated results that have been produced by a Measure command – as opposed tooraw results that are filtered at hello beginning of a query.</span></span>

<span data-ttu-id="fc7be-351">Například:</span><span class="sxs-lookup"><span data-stu-id="fc7be-351">For example:</span></span>

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer
```

<span data-ttu-id="fc7be-352">Můžete přidat další kanálu "|" znak a hello, kde získat příkaz tooonly počítače, jejichž průměrné využití procesoru je nad 80 %, s hello v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="fc7be-352">You can add another pipe "|" character and hello Where command tooonly get computers whose average CPU is above 80%, with hello following example:</span></span>

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer | Where AVGCPU>80
```

<span data-ttu-id="fc7be-353">Pokud jste obeznámeni s nástrojem Microsoft System Center - Operations Manager si můžete představit hello tam, kde příkazu v podmínkách management pack.</span><span class="sxs-lookup"><span data-stu-id="fc7be-353">If you're familiar with Microsoft System Center - Operations Manager, you can think of hello where command in management pack terms.</span></span> <span data-ttu-id="fc7be-354">Pravidlo kdyby hello příklad hello první část dotazu hello by hello zdroj dat a hello kde by příkaz vypadal detekce stavu hello.</span><span class="sxs-lookup"><span data-stu-id="fc7be-354">If hello example were a rule, hello first part of hello query would be hello data source and hello where command would be hello condition detection.</span></span>

<span data-ttu-id="fc7be-355">Můžete použít hello dotazu jako dlaždici v **vlastní řídicí panel**, jako monitorování řazení, toosee po přetížené počítače procesory.</span><span class="sxs-lookup"><span data-stu-id="fc7be-355">You can use hello query as a tile in **My Dashboard**, as a monitor of sorts, toosee when computer CPUs are over-utilized.</span></span> <span data-ttu-id="fc7be-356">toolearn Další informace o řídicím panelům, najdete v části [vytvořit vlastní řídicí panel v analýzy protokolů](log-analytics-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="fc7be-356">toolearn more about dashboards, see [Create a custom dashboard in Log Analytics](log-analytics-dashboards.md).</span></span> <span data-ttu-id="fc7be-357">Můžete také vytvořit a používat řídicí panely pomocí mobilní aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="fc7be-357">You can also create and use dashboards using hello mobile app.</span></span> <span data-ttu-id="fc7be-358">Další informace najdete v tématu [OMS mobilní aplikace ](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865).</span><span class="sxs-lookup"><span data-stu-id="fc7be-358">For more information, see [OMS Mobile App ](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865).</span></span> <span data-ttu-id="fc7be-359">V dlaždicích dolní dva hello hello následující bitové kopie, uvidíte hello monitorování zobrazí seznam, jako číslo.</span><span class="sxs-lookup"><span data-stu-id="fc7be-359">In hello bottom two tiles of hello following image, you can see hello monitor displayed a list and as a number.</span></span> <span data-ttu-id="fc7be-360">V podstatě vždy chcete hello číslo toobe nula a hello toobe seznam prázdný.</span><span class="sxs-lookup"><span data-stu-id="fc7be-360">Essentially, you always want hello number toobe zero and hello list toobe empty.</span></span> <span data-ttu-id="fc7be-361">Jinak hodnota označuje podmínku výstrahy.</span><span class="sxs-lookup"><span data-stu-id="fc7be-361">Otherwise, it indicates an alert condition.</span></span> <span data-ttu-id="fc7be-362">V případě potřeby ho můžete tootake, podívejte se na počítače, které jsou přetížena.</span><span class="sxs-lookup"><span data-stu-id="fc7be-362">If needed, you can use it tootake a look at which machines are under pressure.</span></span>

![mobilní řídicí panel](./media/log-analytics-log-searches/oms-search-mobile.png)

## <a name="use-hello-in-operator"></a><span data-ttu-id="fc7be-364">Použití hello v operátoru</span><span class="sxs-lookup"><span data-stu-id="fc7be-364">Use hello in operator</span></span>
<span data-ttu-id="fc7be-365">Hello *IN* operátor, spolu s *NOT IN* vám umožní toouse subsearches, které jsou hledání, které obsahují další hledání jako argument.</span><span class="sxs-lookup"><span data-stu-id="fc7be-365">hello *IN* operator, along with *NOT IN* allows you toouse subsearches, which are searches that include another search as an argument.</span></span> <span data-ttu-id="fc7be-366">Jsou obsaženy v složené závorky {} v rámci jiného *primární* nebo *vnější* vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="fc7be-366">They are contained in braces {} within another *primary* or *outer* search.</span></span> <span data-ttu-id="fc7be-367">výsledek Hello subsearch, často seznam odlišné výsledky, se pak použije jako argument v jeho primární vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="fc7be-367">hello result of a subsearch, often a list of distinct results, is then used as an argument in its primary search.</span></span>

<span data-ttu-id="fc7be-368">Můžete použít subsearches toomatch podmnožiny dat, nelze popisují přímo v hledaný výraz, ale který se dá vygenerovat z vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="fc7be-368">You can use subsearches toomatch subsets of your data that you cannot describe directly in a search expression, but which can be generated from a search.</span></span> <span data-ttu-id="fc7be-369">Například, pokud vás zajímá pomocí jednoho vyhledávání toofind všechny události z *počítače s chybějícími aktualizacemi zabezpečení*, pak je nutné toodesign subsearch, který nejprve identifikuje, že *počítače s chybějícími aktualizacemi zabezpečení*  předtím, než nalezne události patřící toothose hostitele.</span><span class="sxs-lookup"><span data-stu-id="fc7be-369">For example, if you’re interested in using one search toofind all events from *computers missing security updates*, then you need toodesign a subsearch that first identifies that *computers missing security updates* before it finds events belonging toothose hosts.</span></span>

<span data-ttu-id="fc7be-370">Ano, může express *počítače s aktuálně chybějícími požadovanými aktualizacemi zabezpečení* s hello následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="fc7be-370">So, you could express *computers currently missing required security updates* with hello following query:</span></span>

```
Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer
```    

![V příkladu vyhledávání](./media/log-analytics-log-searches/oms-search-in01-revised.png)

<span data-ttu-id="fc7be-372">Jakmile máte seznam hello, můžete použít vyhledávání hello jako vnitřní vyhledávání toofeed hello seznam počítačů, do vnější (primární) vyhledávání, který bude hledat události pro tyto počítače.</span><span class="sxs-lookup"><span data-stu-id="fc7be-372">Once you have hello list, you can use hello search as an inner search toofeed hello list of computers into an outer (primary) search that will look for events for those computers.</span></span> <span data-ttu-id="fc7be-373">Můžete to udělat obklopuje hello vnitřní vyhledávání do složených závorek a napájení své výsledky jako možné hodnoty nebo pole filtru v hello vnější vyhledávání pomocí operátoru IN hello.</span><span class="sxs-lookup"><span data-stu-id="fc7be-373">You do this by enclosing hello inner search in braces and feeding its results as possible values for a filter/field in hello outer search using hello IN operator.</span></span> <span data-ttu-id="fc7be-374">Hello dotazu by vypadat podobně jako:</span><span class="sxs-lookup"><span data-stu-id="fc7be-374">hello query would resemble:</span></span>

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer}
```
![V příkladu vyhledávání](./media/log-analytics-log-searches/oms-search-in02-revised.png)

<span data-ttu-id="fc7be-376">Také čas hello upozornění filtru v informacích o vnitřní vyhledávání hello použít, protože hello vyhodnocení aktualizací systému pořídí snímek všech počítačů každých 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="fc7be-376">Also notice hello time filter used in hello inner search because hello System Update Assessment takes a snapshot of all computers every 24 hours.</span></span> <span data-ttu-id="fc7be-377">Vnitřní dotaz hello můžete nastavit víc jednoduchý a přesné tak, že pouze jeden den.</span><span class="sxs-lookup"><span data-stu-id="fc7be-377">You can make hello inner query more lightweight and precise by only searching for a day.</span></span> <span data-ttu-id="fc7be-378">vnější vyhledávání Hello místo toho použije hello čas výběr hello uživatelské rozhraní, načítání událostí z hello posledních 7 dnů.</span><span class="sxs-lookup"><span data-stu-id="fc7be-378">hello outer search instead uses hello time selection in hello user interface, retrieving events from hello last 7 days.</span></span> <span data-ttu-id="fc7be-379">V tématu [logické operátory](#boolean-operators) Další informace o času operátory.</span><span class="sxs-lookup"><span data-stu-id="fc7be-379">See [Boolean operators](#boolean-operators) for more information about time operators.</span></span>

<span data-ttu-id="fc7be-380">Protože jste skutečně pouze výsledky hello použití hello vnitřní hledání jako hodnota filtru pro hello vnější jeden, můžete také použít příkazy ve vnější vyhledávání hello.</span><span class="sxs-lookup"><span data-stu-id="fc7be-380">Because you really only use hello results of hello inner search as a filter value for hello outer one, you can still apply commands in hello outer search.</span></span> <span data-ttu-id="fc7be-381">Například můžete stále skupiny hello výše události s jiného příkazu měr:</span><span class="sxs-lookup"><span data-stu-id="fc7be-381">For example, you can still group hello above events with another measure command:</span></span>

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer} | measure count() by Source
```

![V příkladu vyhledávání](./media/log-analytics-log-searches/oms-search-in03-revised.png)

<span data-ttu-id="fc7be-383">Obecně platí budete chtít vaší vnitřní dotaz tooexecute rychle protože analýzy protokolů má straně služby časové limity pro ho a také tooreturn malé množství výsledků.</span><span class="sxs-lookup"><span data-stu-id="fc7be-383">Generally, you want your inner query tooexecute quickly because Log Analytics has service-side timeouts for it and also tooreturn a small amount of results.</span></span> <span data-ttu-id="fc7be-384">Pokud hello vnitřní dotaz vrátí více výsledků, získá zkrácen hello seznam výsledků, které by mohla způsobit hello vnější vyhledávání tooreturn nesprávné výsledky.</span><span class="sxs-lookup"><span data-stu-id="fc7be-384">If hello inner query returns more results, hello result list gets truncated, which could potentially cause hello outer search tooreturn incorrect results.</span></span>

<span data-ttu-id="fc7be-385">Jiné pravidlo je, že vnitřní hledání hello aktuálně musí tooprovide *agregované* výsledky.</span><span class="sxs-lookup"><span data-stu-id="fc7be-385">Another rule is that hello inner search currently needs tooprovide *aggregated* results.</span></span> <span data-ttu-id="fc7be-386">Jinými slovy, musí obsahovat *měr* příkaz; je nelze kanálu aktuálně nezpracovaná výsledky do vnějšího vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="fc7be-386">In other words, it must contain a *measure* command; you cannot currently feed raw results into an outer search.</span></span>

<span data-ttu-id="fc7be-387">Navíc může být pouze jeden operátor IN a musí být hello poslední filtru v dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="fc7be-387">Also, there can be only one IN operator and it must be hello last filter in hello query.</span></span> <span data-ttu-id="fc7be-388">Nemůže být více operátorů v nebo by – to v podstatě brání spuštění více subsearches: hello důležité bod je toto pouze jednu odebíraného nebo vnitřní hledání je možné pro každé vnější vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="fc7be-388">Multiple IN operators cannot be OR’d – this essentially prevents running multiple subsearches: hello important point is that only one sub/inner search is possible for each outer search.</span></span>

<span data-ttu-id="fc7be-389">I když tyto limity umožňuje různé druhy korelační hledání v a můžete toodefine něco podobné toogroups třeba počítače, uživatele nebo soubory – ať hello pole ve vašich datech obsahovat.</span><span class="sxs-lookup"><span data-stu-id="fc7be-389">Even with these limits, IN enables many kinds of correlated searches, and allows you toodefine something similar toogroups such as computers, users, or files – whatever hello fields in your data contain.</span></span> <span data-ttu-id="fc7be-390">Zde jsou další příklady:</span><span class="sxs-lookup"><span data-stu-id="fc7be-390">Here are more examples:</span></span>

<span data-ttu-id="fc7be-391">**Všechny aktualizace chybí z počítačů, kde je zakázaný nastavení automatických aktualizací**</span><span class="sxs-lookup"><span data-stu-id="fc7be-391">**All updates missing from computers where Automatic Update setting is disabled**</span></span>

```
Type=Update UpdateState=Needed Optional=false Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual | measure count() by Computer} | measure count() by KBID
```

<span data-ttu-id="fc7be-392">**Všechny chybové události z počítače se systémem SQL Server (=, kde se má spustit vyhodnocení SQL)**</span><span class="sxs-lookup"><span data-stu-id="fc7be-392">**All error events from computers running SQL Server (=where SQL Assessment has run)**</span></span>

```
Type=Event EventLevelName=error Computer IN {Type=SQLAssessmentRecommendation | measure count() by Computer}
```

<span data-ttu-id="fc7be-393">**Všechny události zabezpečení z počítačů, které jsou řadiče domény se (=, kde byla spuštěna hodnocení AD)**</span><span class="sxs-lookup"><span data-stu-id="fc7be-393">**All security events from computers that are Domain Controllers (=where AD Assessment has run)**</span></span>

```
Type=SecurityEvent Computer IN { Type=ADAssessmentRecommendation | measure count() by Computer }
```

<span data-ttu-id="fc7be-394">**Které jiné účty přihlášení toohello stejných počítačů, kde má přihlášený účet BACONLAND\jochan?**</span><span class="sxs-lookup"><span data-stu-id="fc7be-394">**Which other accounts have logged on toohello same computers where account BACONLAND\jochan has logged on?**</span></span>

```
Type=SecurityEvent EventID=4624   Account!="BACONLAND\\jochan" Computer IN { Type=SecurityEvent EventID=4624   Account="BACONLAND\\jochan" | measure count() by Computer } | measure count() by Account
```

## <a name="use-hello-distinct-command"></a><span data-ttu-id="fc7be-395">Použijte příkaz distinct hello</span><span class="sxs-lookup"><span data-stu-id="fc7be-395">Use hello distinct command</span></span>
<span data-ttu-id="fc7be-396">Jako název hello naznačuje, tento příkaz poskytuje seznam jedinečných hodnot pro pole.</span><span class="sxs-lookup"><span data-stu-id="fc7be-396">As hello name suggests, this command provides a list of distinct values for a field.</span></span> <span data-ttu-id="fc7be-397">Je překvapivě jednoduchý, ale velmi užitečné.</span><span class="sxs-lookup"><span data-stu-id="fc7be-397">It's surprisingly simple but quite useful.</span></span> <span data-ttu-id="fc7be-398">Hello stejné bylo možné dosáhnout pomocí příkazu count() měr stejně, jako vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="fc7be-398">hello same could be achieved with measure count() command as well, as shown below.</span></span>

```
Type=Event | Measure count() by Computer
```

![Ukázka příkazu odlišné vyhledávání](./media/log-analytics-log-searches/oms-search-distinct01-revised.png)

<span data-ttu-id="fc7be-400">Ale pokud všechny, které vás zajímají právě seznam jedinečných hodnot a není hello počet dokumentů, které mají této hodnoty, pak DISTINCT může poskytovat čisticí a snadnější tooread výstup a kratší syntaxe, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="fc7be-400">However, if all you're interested in is just a list of distinct values and not hello count of documents that have that values, then DISTINCT can provide cleaner and easier tooread output, and shorter syntax, as shown below.</span></span>

```
Type=Event | Distinct Computer
```
![Ukázka příkazu odlišné vyhledávání](./media/log-analytics-log-searches/oms-search-distinct02-revised.png)

## <a name="use-hello-countdistinct-function-with-hello-measure-command"></a><span data-ttu-id="fc7be-402">Pomocí funkce countdistinct hello příkazem měr hello</span><span class="sxs-lookup"><span data-stu-id="fc7be-402">Use hello countdistinct function with hello measure command</span></span>
<span data-ttu-id="fc7be-403">Funkce countdistinct Hello počty hello počet jedinečných hodnot v rámci jednotlivých skupin.</span><span class="sxs-lookup"><span data-stu-id="fc7be-403">hello countdistinct function counts hello number of distinct values within each group.</span></span> <span data-ttu-id="fc7be-404">Například může být použit toocount hello počet jedinečných počítačů, vytváření sestav pro každý typ:</span><span class="sxs-lookup"><span data-stu-id="fc7be-404">For example, it could be used toocount hello number of unique computers reporting for each Type:</span></span>

```
* | measure countdistinct(Computer) by Type
```

![OMS countdistinct](./media/log-analytics-log-searches/oms-countdistinct.png)

## <a name="use-hello-measure-interval-command"></a><span data-ttu-id="fc7be-406">Pomocí příkazu interval měr hello</span><span class="sxs-lookup"><span data-stu-id="fc7be-406">Use hello measure interval command</span></span>
<span data-ttu-id="fc7be-407">S téměř v reálném čase výkonu shromažďování dat, můžete shromažďovat a vizualizovat všechny čítače výkonu v analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="fc7be-407">With near real-time performance data collection, you can collect and visualize any performance counter in Log Analytics.</span></span> <span data-ttu-id="fc7be-408">Jednoduše vstup dotazu hello **typu: výkonu** vrátí tisíce metriky grafy na základě počtu hello čítače a serverů ve vašem prostředí analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="fc7be-408">Simply entering hello query **Type:Perf** will return thousands of metric graphs based on hello number of counters and servers in your Log Analytics environment.</span></span> <span data-ttu-id="fc7be-409">S průběhem metriky na vyžádání, můžete se podívat na hello metriky celkového ve vašem prostředí na vysoké úrovni a podrobné informace na podrobnější data, jako je třeba.</span><span class="sxs-lookup"><span data-stu-id="fc7be-409">With on-demand metric aggregation, you can look at hello overall metrics in your environment at a high level, and deep dive into more granular data as you need to.</span></span>

<span data-ttu-id="fc7be-410">Řekněme, že chcete tooknow co hello průměrné využití procesoru je pro všechny počítače.</span><span class="sxs-lookup"><span data-stu-id="fc7be-410">Let’s say that you want tooknow what is hello average CPU across all your computers.</span></span> <span data-ttu-id="fc7be-411">Prohlížení hello průměrné využití procesoru pro každý počítač nemusí být užitečné, protože může získat vyhlazené výsledky. toolook do další podrobnosti, můžete agregovat sady výsledků v menší časové okno bloky a podívejte se do časové řady mezi různými dimenzemi.</span><span class="sxs-lookup"><span data-stu-id="fc7be-411">Looking at hello average CPU for every computer might not be helpful because results may get smoothed out. toolook into more details, you can aggregate your result in a smaller time window chunks, and look into a time series across different dimensions.</span></span> <span data-ttu-id="fc7be-412">Například můžete provádět hello hodinové průměr využití procesoru pro všechny počítače takto:</span><span class="sxs-lookup"><span data-stu-id="fc7be-412">For example, you can perform hello hourly average of CPU usage across all your computers as follows:</span></span>

```
Type:Perf CounterName="% Processor Time" InstanceName="_Total" | measure avg(CounterValue) by Computer Interval 1HOUR
```

![Průměrný interval měr](./media/log-analytics-log-searches/oms-measure-avg-interval.png)

<span data-ttu-id="fc7be-414">Ve výchozím nastavení tyto výsledky se zobrazí v řádku interaktivního více řad grafu.</span><span class="sxs-lookup"><span data-stu-id="fc7be-414">By default these results will be displayed in a multi-series interactive line chart.</span></span>  <span data-ttu-id="fc7be-415">Tento graf podporuje řady přepnutím (s změny měřítka osy y), přibližování a ukazatele.</span><span class="sxs-lookup"><span data-stu-id="fc7be-415">This chart supports series toggling (with y-axis rescaling), zooming, and hovering.</span></span>  <span data-ttu-id="fc7be-416">možnost zobrazení tabulky Hello je stále k dispozici pro prohlížení hello nezpracovaných dat v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="fc7be-416">hello table display option is still available for viewing hello raw data if necessary.</span></span>

<span data-ttu-id="fc7be-417">Můžete taky Seskupit podle jiných polí.</span><span class="sxs-lookup"><span data-stu-id="fc7be-417">You can also group by other fields.</span></span> <span data-ttu-id="fc7be-418">V tomto příkladu dívám se na všech čítačů % hello jeden konkrétní počítač a chcete tooknow co je hello každou hodinu 70 percentily každých čítače:</span><span class="sxs-lookup"><span data-stu-id="fc7be-418">In this example, I am looking at all hello % counters for one specific computer, and I want tooknow what is hello hourly 70 percentiles of every counter:</span></span>

```
Type:Perf Computer=beefpatty4 CounterName=%* InstanceName=_Total | measure percentile70(CounterValue) by CounterName Interval 1HOUR
```
<span data-ttu-id="fc7be-419">Jednu věc toonote je, že tyto dotazy nejsou omezené tooperformance čítače.</span><span class="sxs-lookup"><span data-stu-id="fc7be-419">One thing toonote is that these queries are not limited tooperformance counters.</span></span> <span data-ttu-id="fc7be-420">Můžete je použít tooany metriku.</span><span class="sxs-lookup"><span data-stu-id="fc7be-420">You can apply them tooany metric.</span></span> <span data-ttu-id="fc7be-421">V tomto příkladu dívám se na protokoly služby IIS W3C.</span><span class="sxs-lookup"><span data-stu-id="fc7be-421">In this example, I’m looking at W3C IIS logs.</span></span> <span data-ttu-id="fc7be-422">Chci tooknow, co je hello maximální doba, kterou trvá v intervalu 5 minut pro zpracování každé žádosti:</span><span class="sxs-lookup"><span data-stu-id="fc7be-422">I want tooknow what is hello maximum time it takes over a 5-minute interval for processing each request:</span></span>

```
Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES
```

### <a name="use-multiple-aggregates-in-one-query"></a><span data-ttu-id="fc7be-423">Použití více agregace v jednom dotazu</span><span class="sxs-lookup"><span data-stu-id="fc7be-423">Use multiple aggregates in one query</span></span>
<span data-ttu-id="fc7be-424">Více klauzulí agregační můžete zadat v příkazu měr.</span><span class="sxs-lookup"><span data-stu-id="fc7be-424">You can specify multiple aggregate clauses in a measure command.</span></span>  <span data-ttu-id="fc7be-425">Každé z nich může být alias nezávisle.</span><span class="sxs-lookup"><span data-stu-id="fc7be-425">Each one can be aliased independently.</span></span>  <span data-ttu-id="fc7be-426">Pokud není zadána, hello alias výsledná hello agregační funkci, která byla použita bude název pole (tj. "avg(CounterValue)" pro avg(CounterValue)).</span><span class="sxs-lookup"><span data-stu-id="fc7be-426">If it is not given an alias hello resulting field name will be hello aggregate function that was used (i.e. "avg(CounterValue)" for avg(CounterValue)).</span></span>

 ```
Type=WireData | measure avg(ReceivedBytes), avg(SentBytes) by Direction interval 1hour
```
![OMS multiaggregates1](./media/log-analytics-log-searches/oms-multiaggregates1.png)

<span data-ttu-id="fc7be-428">Tady je další příklad:</span><span class="sxs-lookup"><span data-stu-id="fc7be-428">Here is another example:</span></span>

 ```
* | measure countdistinct(Computer) as Computers, count() as TotalRecords by Type
```


## <a name="next-steps"></a><span data-ttu-id="fc7be-429">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fc7be-429">Next steps</span></span>
<span data-ttu-id="fc7be-430">Další informace o protokolu hledání najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="fc7be-430">For additional information about log searches, see:</span></span>

* <span data-ttu-id="fc7be-431">Použití [vlastní pole v analýzy protokolů](log-analytics-custom-fields.md) tooextend protokolu hledání.</span><span class="sxs-lookup"><span data-stu-id="fc7be-431">Use [Custom fields in Log Analytics](log-analytics-custom-fields.md) tooextend log searches.</span></span>
* <span data-ttu-id="fc7be-432">Zkontrolujte hello [analýzy protokolů protokolu vyhledávání odkaz](log-analytics-search-reference.md) tooview všechny hello vyhledávání polí a vlastností, které jsou k dispozici v analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="fc7be-432">Review hello [Log Analytics log search reference](log-analytics-search-reference.md) tooview all of hello search fields and facets available in Log Analytics.</span></span>
