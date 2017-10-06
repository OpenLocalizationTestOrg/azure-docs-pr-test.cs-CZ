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
# <a name="debug-queries-by-using-select-into-statements"></a><span data-ttu-id="16b69-103">Ladění dotazy pomocí příkazů SELECT INTO</span><span class="sxs-lookup"><span data-stu-id="16b69-103">Debug queries by using SELECT INTO statements</span></span>

<span data-ttu-id="16b69-104">Při zpracování dat v reálném čase zároveň budete vědět, jaká data hello vypadá uprostřed hello hello dotazu může být užitečné.</span><span class="sxs-lookup"><span data-stu-id="16b69-104">In real-time data processing, knowing what hello data looks like in hello middle of hello query can be helpful.</span></span> <span data-ttu-id="16b69-105">Protože vstupů nebo kroky úlohu služby Azure Stream Analytics lze číst vícekrát, můžete napsat doplňující příkazy SELECT INTO.</span><span class="sxs-lookup"><span data-stu-id="16b69-105">Because inputs or steps of an Azure Stream Analytics job can be read multiple times, you can write extra SELECT INTO statements.</span></span> <span data-ttu-id="16b69-106">Díky tomu výstupy mezilehlá data do úložiště a umožňuje vám zkontrolujte správnost hello hello dat, stejně jako *sledovat proměnné* provést při ladění programu.</span><span class="sxs-lookup"><span data-stu-id="16b69-106">Doing so outputs intermediate data into storage and lets you inspect hello correctness of hello data, just as *watch variables* do when you debug a program.</span></span>

## <a name="use-select-into-toocheck-hello-data-stream"></a><span data-ttu-id="16b69-107">Použití SELECT INTO toocheck hello datový proud</span><span class="sxs-lookup"><span data-stu-id="16b69-107">Use SELECT INTO toocheck hello data stream</span></span>

<span data-ttu-id="16b69-108">Hello následující příklad dotazu v úloze služby Azure Stream Analytics má jeden datový proud vstup, dvěma vstupy referenční data a tooAzure výstupní tabulka úložiště.</span><span class="sxs-lookup"><span data-stu-id="16b69-108">hello following example query in an Azure Stream Analytics job has one stream input, two reference data inputs, and an output tooAzure Table Storage.</span></span> <span data-ttu-id="16b69-109">dotaz Hello spojí dat z centra událostí hello a odkaz na dva objekty BLOB tooget hello název a informace o kategorii:</span><span class="sxs-lookup"><span data-stu-id="16b69-109">hello query joins data from hello event hub and two reference blobs tooget hello name and category information:</span></span>

![Příklad SELECT INTO dotazu](./media/stream-analytics-select-into/stream-analytics-select-into-query1.png)

<span data-ttu-id="16b69-111">Všimněte si, že je spuštěná úloha hello, ale žádné události se vytváří ve výstupu hello.</span><span class="sxs-lookup"><span data-stu-id="16b69-111">Note that hello job is running, but no events are being produced in hello output.</span></span> <span data-ttu-id="16b69-112">Na hello **monitorování** dlaždici zobrazeny zde, uvidíte, že vstup hello je generovala data, ale nevíte, který krok hello **připojení** způsobeny všechny hello toobe události vyřadit.</span><span class="sxs-lookup"><span data-stu-id="16b69-112">On hello **Monitoring** tile, shown here, you can see that hello input is producing data, but you don’t know which step of hello **JOIN** caused all hello events toobe dropped.</span></span>

![dlaždice monitorování Hello](./media/stream-analytics-select-into/stream-analytics-select-into-monitor.png)
 
<span data-ttu-id="16b69-114">V takovém případě můžete přidat několik navíc příkazech SELECT INTO příliš "" hello mezilehlých výsledků spojení a protokolu hello data, která je pro čtení z hello vstup.</span><span class="sxs-lookup"><span data-stu-id="16b69-114">In this situation, you can add a few extra SELECT INTO statements too“log” hello intermediate JOIN results and hello data that's read from hello input.</span></span>

<span data-ttu-id="16b69-115">V tomto příkladu jsme přidali dvě nové "dočasné výstupy."</span><span class="sxs-lookup"><span data-stu-id="16b69-115">In this example, we've added two new “temporary outputs.”</span></span> <span data-ttu-id="16b69-116">Může se jednat žádné podřízený, které se vám líbí.</span><span class="sxs-lookup"><span data-stu-id="16b69-116">They can be any sink you like.</span></span> <span data-ttu-id="16b69-117">Tady používáme Azure Storage jako příklad:</span><span class="sxs-lookup"><span data-stu-id="16b69-117">Here we use Azure Storage as an example:</span></span>

![Přidání další příkazy SELECT INTO](./media/stream-analytics-select-into/stream-analytics-select-into-outputs.png)

<span data-ttu-id="16b69-119">Potom můžete přepsat pomocí dotazu hello takto:</span><span class="sxs-lookup"><span data-stu-id="16b69-119">You can then rewrite hello query like this:</span></span>

![Přepsaná dotaz SELECT INTO](./media/stream-analytics-select-into/stream-analytics-select-into-query2.png)

<span data-ttu-id="16b69-121">Teď hello úlohu znovu spustit a nechat ji spustit několik minut.</span><span class="sxs-lookup"><span data-stu-id="16b69-121">Now start hello job again, and let it run for a few minutes.</span></span> <span data-ttu-id="16b69-122">Potom budete dotazovat temp1 a temp2 s cloudu Průzkumníka Visual Studio tooproduce hello následujících tabulek:</span><span class="sxs-lookup"><span data-stu-id="16b69-122">Then query temp1 and temp2 with Visual Studio Cloud Explorer tooproduce hello following tables:</span></span>

<span data-ttu-id="16b69-123">**Tabulka temp1**
![SELECT INTO temp1 tabulky](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-1.png)</span><span class="sxs-lookup"><span data-stu-id="16b69-123">**temp1 table**
![SELECT INTO temp1 table](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-1.png)</span></span>

<span data-ttu-id="16b69-124">**Tabulka temp2**
![SELECT INTO temp2 tabulky](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-2.png)</span><span class="sxs-lookup"><span data-stu-id="16b69-124">**temp2 table**
![SELECT INTO temp2 table](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-2.png)</span></span>

<span data-ttu-id="16b69-125">Jak vidíte, temp1 a temp2 mají data a hello název sloupce je v temp2 správně zadána.</span><span class="sxs-lookup"><span data-stu-id="16b69-125">As you can see, temp1 and temp2 both have data, and hello name column is populated correctly in temp2.</span></span> <span data-ttu-id="16b69-126">Ale protože ve výstupu stále neexistuje žádná data, je něco špatně:</span><span class="sxs-lookup"><span data-stu-id="16b69-126">However, because there is still no data in output, something is wrong:</span></span>

![Vyberte do tabulky output1 bez dat](./media/stream-analytics-select-into/stream-analytics-select-into-out-table-1.png)

<span data-ttu-id="16b69-128">Vzorkování dat hello, můžete si být téměř jisti, že hello problém s hello druhé připojení.</span><span class="sxs-lookup"><span data-stu-id="16b69-128">By sampling hello data, you can be almost certain that hello issue is with hello second JOIN.</span></span> <span data-ttu-id="16b69-129">Můžete stáhnout hello referenční data z objektu blob hello a podívejte se:</span><span class="sxs-lookup"><span data-stu-id="16b69-129">You can download hello reference data from hello blob and take a look:</span></span>

![SELECT INTO ref tabulky](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-1.png)

<span data-ttu-id="16b69-131">Jak vidíte, se liší od hello formát hello [z] sloupce v temp2 hello formát hello identifikátor GUID v této referenční data.</span><span class="sxs-lookup"><span data-stu-id="16b69-131">As you can see, hello format of hello GUID in this reference data is different from hello format of hello [from] column in temp2.</span></span> <span data-ttu-id="16b69-132">To je důvod, proč hello data nebyla doručení output1 podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="16b69-132">That’s why hello data didn’t arrive in output1 as expected.</span></span>

<span data-ttu-id="16b69-133">Opravte formát dat hello, nahrajte ho tooreference objektů blob a opakujte akci:</span><span class="sxs-lookup"><span data-stu-id="16b69-133">You can fix hello data format, upload it tooreference blob, and try again:</span></span>

![Vyberte do dočasné tabulky](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-2.png)

<span data-ttu-id="16b69-135">Tento čas hello data ve výstupu hello je naformátován a vyplní podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="16b69-135">This time, hello data in hello output is formatted and populated as expected.</span></span>

![SELECT INTO poslední tabulky](./media/stream-analytics-select-into/stream-analytics-select-into-final-table.png)


## <a name="get-help"></a><span data-ttu-id="16b69-137">Podpora</span><span class="sxs-lookup"><span data-stu-id="16b69-137">Get help</span></span>

<span data-ttu-id="16b69-138">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="16b69-138">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="16b69-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="16b69-139">Next steps</span></span>

* [<span data-ttu-id="16b69-140">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="16b69-140">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="16b69-141">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="16b69-141">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="16b69-142">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="16b69-142">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="16b69-143">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="16b69-143">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="16b69-144">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="16b69-144">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

