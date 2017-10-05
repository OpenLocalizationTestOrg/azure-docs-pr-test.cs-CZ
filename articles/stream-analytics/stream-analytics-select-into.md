---
title: "Ladění Azure Stream Analytics dotazy pomocí SELECT INTO | Microsoft Docs"
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
ms.openlocfilehash: b05222c6d6f4fc2c5b847dd75ff7e29352cd538c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="debug-queries-by-using-select-into-statements"></a><span data-ttu-id="295a2-103">Ladění dotazy pomocí příkazů SELECT INTO</span><span class="sxs-lookup"><span data-stu-id="295a2-103">Debug queries by using SELECT INTO statements</span></span>

<span data-ttu-id="295a2-104">Při zpracování dat v reálném čase zároveň budete vědět, co data vypadá uprostřed dotazu může být užitečné.</span><span class="sxs-lookup"><span data-stu-id="295a2-104">In real-time data processing, knowing what the data looks like in the middle of the query can be helpful.</span></span> <span data-ttu-id="295a2-105">Protože vstupů nebo kroky úlohu služby Azure Stream Analytics lze číst vícekrát, můžete napsat doplňující příkazy SELECT INTO.</span><span class="sxs-lookup"><span data-stu-id="295a2-105">Because inputs or steps of an Azure Stream Analytics job can be read multiple times, you can write extra SELECT INTO statements.</span></span> <span data-ttu-id="295a2-106">Díky tomu výstupy mezilehlá data do úložiště a umožňuje vám zkontrolujte správnost dat, stejně jako *sledovat proměnné* provést při ladění programu.</span><span class="sxs-lookup"><span data-stu-id="295a2-106">Doing so outputs intermediate data into storage and lets you inspect the correctness of the data, just as *watch variables* do when you debug a program.</span></span>

## <a name="use-select-into-to-check-the-data-stream"></a><span data-ttu-id="295a2-107">Pomocí SELECT INTO zkontrolujte datový proud</span><span class="sxs-lookup"><span data-stu-id="295a2-107">Use SELECT INTO to check the data stream</span></span>

<span data-ttu-id="295a2-108">Následující příklad dotazu v úloze služby Azure Stream Analytics má jeden datový proud vstup, dvěma vstupy referenční data a výstup do úložiště tabulek Azure.</span><span class="sxs-lookup"><span data-stu-id="295a2-108">The following example query in an Azure Stream Analytics job has one stream input, two reference data inputs, and an output to Azure Table Storage.</span></span> <span data-ttu-id="295a2-109">Dotaz propojuje dat z centra událostí a odkaz na dva objekty BLOB se získat informace o názvu a kategorie:</span><span class="sxs-lookup"><span data-stu-id="295a2-109">The query joins data from the event hub and two reference blobs to get the name and category information:</span></span>

![Příklad SELECT INTO dotazu](./media/stream-analytics-select-into/stream-analytics-select-into-query1.png)

<span data-ttu-id="295a2-111">Všimněte si, že úloha je spuštěná, ale žádné události se vytváří ve výstupu.</span><span class="sxs-lookup"><span data-stu-id="295a2-111">Note that the job is running, but no events are being produced in the output.</span></span> <span data-ttu-id="295a2-112">Na **monitorování** dlaždici zobrazeny zde, uvidíte, že vstup je generovala data, ale nevíte, který krok **připojení** způsobila všechny události, které chcete vyřadit.</span><span class="sxs-lookup"><span data-stu-id="295a2-112">On the **Monitoring** tile, shown here, you can see that the input is producing data, but you don’t know which step of the **JOIN** caused all the events to be dropped.</span></span>

![Na dlaždici monitorování](./media/stream-analytics-select-into/stream-analytics-select-into-monitor.png)
 
<span data-ttu-id="295a2-114">V takovém případě můžete přidat několik navíc příkazech SELECT INTO se "přihlásit" mezilehlých výsledků spojení a data, která je načtený ze vstupu.</span><span class="sxs-lookup"><span data-stu-id="295a2-114">In this situation, you can add a few extra SELECT INTO statements to “log” the intermediate JOIN results and the data that's read from the input.</span></span>

<span data-ttu-id="295a2-115">V tomto příkladu jsme přidali dvě nové "dočasné výstupy."</span><span class="sxs-lookup"><span data-stu-id="295a2-115">In this example, we've added two new “temporary outputs.”</span></span> <span data-ttu-id="295a2-116">Může se jednat žádné podřízený, které se vám líbí.</span><span class="sxs-lookup"><span data-stu-id="295a2-116">They can be any sink you like.</span></span> <span data-ttu-id="295a2-117">Tady používáme Azure Storage jako příklad:</span><span class="sxs-lookup"><span data-stu-id="295a2-117">Here we use Azure Storage as an example:</span></span>

![Přidání další příkazy SELECT INTO](./media/stream-analytics-select-into/stream-analytics-select-into-outputs.png)

<span data-ttu-id="295a2-119">Pak je možné znovu vytvořit dotaz takto:</span><span class="sxs-lookup"><span data-stu-id="295a2-119">You can then rewrite the query like this:</span></span>

![Přepsaná dotaz SELECT INTO](./media/stream-analytics-select-into/stream-analytics-select-into-query2.png)

<span data-ttu-id="295a2-121">Nyní znovu spustit úlohu a nechat ji spustit několik minut.</span><span class="sxs-lookup"><span data-stu-id="295a2-121">Now start the job again, and let it run for a few minutes.</span></span> <span data-ttu-id="295a2-122">Potom budete dotazovat temp1 a temp2 s cloudu Průzkumníka Visual Studio k vytvoření následujících tabulek:</span><span class="sxs-lookup"><span data-stu-id="295a2-122">Then query temp1 and temp2 with Visual Studio Cloud Explorer to produce the following tables:</span></span>

<span data-ttu-id="295a2-123">**Tabulka temp1**
![SELECT INTO temp1 tabulky](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-1.png)</span><span class="sxs-lookup"><span data-stu-id="295a2-123">**temp1 table**
![SELECT INTO temp1 table](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-1.png)</span></span>

<span data-ttu-id="295a2-124">**Tabulka temp2**
![SELECT INTO temp2 tabulky](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-2.png)</span><span class="sxs-lookup"><span data-stu-id="295a2-124">**temp2 table**
![SELECT INTO temp2 table](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-2.png)</span></span>

<span data-ttu-id="295a2-125">Jak vidíte, temp1 a temp2 mají data, a název sloupce je v temp2 správně zadána.</span><span class="sxs-lookup"><span data-stu-id="295a2-125">As you can see, temp1 and temp2 both have data, and the name column is populated correctly in temp2.</span></span> <span data-ttu-id="295a2-126">Ale protože ve výstupu stále neexistuje žádná data, je něco špatně:</span><span class="sxs-lookup"><span data-stu-id="295a2-126">However, because there is still no data in output, something is wrong:</span></span>

![Vyberte do tabulky output1 bez dat](./media/stream-analytics-select-into/stream-analytics-select-into-out-table-1.png)

<span data-ttu-id="295a2-128">Vzorkování dat, můžete si být téměř jisti, že problém s druhou spojení.</span><span class="sxs-lookup"><span data-stu-id="295a2-128">By sampling the data, you can be almost certain that the issue is with the second JOIN.</span></span> <span data-ttu-id="295a2-129">Můžete stáhnout z objektu blob referenčních dat a podívejte se:</span><span class="sxs-lookup"><span data-stu-id="295a2-129">You can download the reference data from the blob and take a look:</span></span>

![SELECT INTO ref tabulky](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-1.png)

<span data-ttu-id="295a2-131">Jak vidíte, se liší od formát formát identifikátoru GUID v těchto datech odkaz [sloupec v temp2 z].</span><span class="sxs-lookup"><span data-stu-id="295a2-131">As you can see, the format of the GUID in this reference data is different from the format of the [from] column in temp2.</span></span> <span data-ttu-id="295a2-132">To je důvod, proč data nebyla doručení output1 podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="295a2-132">That’s why the data didn’t arrive in output1 as expected.</span></span>

<span data-ttu-id="295a2-133">Můžete opravit formát dat, nahrajte ho odkazovat na objekt blob a potom akci opakujte:</span><span class="sxs-lookup"><span data-stu-id="295a2-133">You can fix the data format, upload it to reference blob, and try again:</span></span>

![Vyberte do dočasné tabulky](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-2.png)

<span data-ttu-id="295a2-135">Tento čas data ve výstupu je naformátován a vyplní podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="295a2-135">This time, the data in the output is formatted and populated as expected.</span></span>

![SELECT INTO poslední tabulky](./media/stream-analytics-select-into/stream-analytics-select-into-final-table.png)


## <a name="get-help"></a><span data-ttu-id="295a2-137">Podpora</span><span class="sxs-lookup"><span data-stu-id="295a2-137">Get help</span></span>

<span data-ttu-id="295a2-138">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="295a2-138">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="295a2-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="295a2-139">Next steps</span></span>

* [<span data-ttu-id="295a2-140">Úvod do služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="295a2-140">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="295a2-141">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="295a2-141">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="295a2-142">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="295a2-142">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="295a2-143">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="295a2-143">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="295a2-144">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="295a2-144">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

