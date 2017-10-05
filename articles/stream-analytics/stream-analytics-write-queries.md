---
title: "Tom, jak psát dotazy v Stream Analytics | Microsoft Docs"
description: "Zápis dotazů v Stream Analytics a dotazování dat | učení segmentu cesty."
keywords: "tom, jak psát dotazy, dotaz na data, napsat dotaz, zápis dotazů"
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 0e9cdadd-0ee0-4bee-b65b-4a06fb863c95
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: b44b0658a06761a805708e7fdeba9e3b2cf9d3ab
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-write-queries-in-stream-analytics"></a><span data-ttu-id="27782-104">Tom, jak psát dotazy v Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="27782-104">How to write queries in Stream Analytics</span></span>
<span data-ttu-id="27782-105">Zápis dotazů pro zpracování logiky v Azure Stream Analytics datového proudu je implementovaný jako "stojící dotaz", který je definován před úloha spustí a u dat provést, protože nedosáhne úlohy.</span><span class="sxs-lookup"><span data-stu-id="27782-105">Writing queries for stream processing logic in Azure Stream Analytics is implemented as a "standing query" that is defined before a job starts and executed on data as it reaches the job.</span></span> <span data-ttu-id="27782-106">Transformaci dat je vyjádřenou v jazyce SQL jako dotaz, který je z velké části podmnožinou T-SQL s některými přidat jazyková rozšíření jako [Oddílová](https://msdn.microsoft.com/library/azure/dn835019.aspx) použije pro vyjádření dočasné sémantiku.</span><span class="sxs-lookup"><span data-stu-id="27782-106">The data transformation is expressed in a SQL-like query language, which is largely a subset of T-SQL with some added language extensions like [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx) used to express temporal semantics.</span></span>

## <a name="writing-queries"></a><span data-ttu-id="27782-107">Zápis dotazů:</span><span class="sxs-lookup"><span data-stu-id="27782-107">Writing Queries:</span></span>
1. <span data-ttu-id="27782-108">V vaše úlohy Stream Analytics na portálu Azure Management portal, klikněte na **dotazu**.</span><span class="sxs-lookup"><span data-stu-id="27782-108">In your Stream Analytics Job in the Azure Management portal, click **Query**.</span></span>
   
    ![Vybrat dotaz](./media/stream-analytics-write-queries/1-stream-analytics-write-queries.png)  
   
    <span data-ttu-id="27782-110">Na portálu Azure klikněte na tlačítko **dotazu**.</span><span class="sxs-lookup"><span data-stu-id="27782-110">In the Azure Portal, click **Query**.</span></span>
   
    ![Vyberte Náhled dotazu](./media/stream-analytics-write-queries/query-preview-portal.png)  
2. <span data-ttu-id="27782-112">Nové úlohy mají šablonu dotazu pro vám pomůžou začít.</span><span class="sxs-lookup"><span data-stu-id="27782-112">New jobs have a query template to help get you started.</span></span> <span data-ttu-id="27782-113">Šablonu dotazu provádí "předávací" dotaz, který projekty všech polí ze vstupu události do výstupu.</span><span class="sxs-lookup"><span data-stu-id="27782-113">The query template performs a "pass-through" query that projects all fields from input events into the output.</span></span>  
   
   * <span data-ttu-id="27782-114">Pokud jste definovali alespoň jeden vstup a výstup pro úlohu, můžete nahraďte zástupný symbol "[YourOutputAlias]" a "[YourInputAlias]" pole s aliasy vstupu a výstupu, který chcete použít jako první.</span><span class="sxs-lookup"><span data-stu-id="27782-114">If you have defined at least one input and output for your job, you can replace the placeholder "[YourOutputAlias]" and "[YourInputAlias]" fields with the aliases of the input and output that you wish use first.</span></span> <span data-ttu-id="27782-115">Kromě toho můžete stále vytvářet a testovat dotaz na portálu Azure Classic bez definování vstupů a výstupů v úloze.</span><span class="sxs-lookup"><span data-stu-id="27782-115">In addition, you can still author and test your query in the Azure Classic Portal without defining inputs and outputs on the job.</span></span>
   * <span data-ttu-id="27782-116">Pokud chcete provést další zpracování než jednoduchý průchozí, můžete upravit definice dotazu.</span><span class="sxs-lookup"><span data-stu-id="27782-116">If you wish to perform more processing than a simple pass-through, you can edit the query definition.</span></span> <span data-ttu-id="27782-117">Začít s vytváření dotazů, podívejte se na některé běžné dotazu zaznamenání vzory [zde](stream-analytics-stream-analytics-query-patterns.md).</span><span class="sxs-lookup"><span data-stu-id="27782-117">To get started with query authoring, take a look at some common query patterns are captured [here](stream-analytics-stream-analytics-query-patterns.md).</span></span>  
   
   ![Dotaz na data okna](./media/stream-analytics-write-queries/2-stream-analytics-write-queries.png)  

## <a name="to-validate-query-data-is-working"></a><span data-ttu-id="27782-119">Ověřit data dotazu funguje:</span><span class="sxs-lookup"><span data-stu-id="27782-119">To validate query data is working:</span></span>
<span data-ttu-id="27782-120">Můžete otestovat, zda dotaz chová podle očekávání spuštěním v prohlížeči přes jeden nebo více místní soubory JSON obsahující testovací data.</span><span class="sxs-lookup"><span data-stu-id="27782-120">You can test that your query behaves as expected by running it in the browser over one or more local JSON files containing test data.</span></span> <span data-ttu-id="27782-121">To nebude spustit úlohu nebo mají vliv na všechny fakturace.</span><span class="sxs-lookup"><span data-stu-id="27782-121">This will not start the job or have any billing implications.</span></span>

> [!NOTE]
> <span data-ttu-id="27782-122">Aktuálně testování dotazů v prohlížeči není podporována na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="27782-122">Currently in-browser query testing is not supported in the Azure Portal.</span></span>  
> 
> 

1. <span data-ttu-id="27782-123">Ujistěte se, že nejsou žádné chyby v dotazu (jinak tlačítko Testovat bude zakázáno) a potom klikněte na tlačítko Test.</span><span class="sxs-lookup"><span data-stu-id="27782-123">Make sure that there are no errors in the query (otherwise the Test button will be disabled) and then click the Test button.</span></span>  
   
   ![Dotaz na data testu](./media/stream-analytics-write-queries/3-stream-analytics-write-queries.png)  
2. <span data-ttu-id="27782-125">Zobrazí se výzva k zadání souborů pro každou z vstupů v dotazu odkazovat.</span><span class="sxs-lookup"><span data-stu-id="27782-125">You will be prompted to specify files for each of the inputs referenced in the query.</span></span> <span data-ttu-id="27782-126">V tomto příkladu je dotaz šablony ponechané jako-se, takže dialogu je dotaz na vstup s názvem "yourinputalias".</span><span class="sxs-lookup"><span data-stu-id="27782-126">In this example, the template query is left as-is, so the dialog is prompting for an input named "yourinputalias".</span></span>  
   
   ![Dotaz na Data testu](./media/stream-analytics-write-queries/4-stream-analytics-write-queries.png)  
3. <span data-ttu-id="27782-128">Vyhledejte soubor testu.</span><span class="sxs-lookup"><span data-stu-id="27782-128">Browse to a test file.</span></span> <span data-ttu-id="27782-129">Několik ukázkových souborů, které jsou k dispozici na [githubu](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) a můžete také načíst ukázková data z vlastního datového proudu vstupy dat prostřednictvím funkce ukázková Data na kartě vstupy.</span><span class="sxs-lookup"><span data-stu-id="27782-129">Several sample files are available on [github](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) and you can also retrieve sample data from your own data stream inputs via the Sample Data function on the inputs tab.</span></span>  
   
   ![Vstupní dotaz](./media/stream-analytics-write-queries/5-stream-analytics-write-queries.png)  
4. <span data-ttu-id="27782-131">Po zavření dialogu, dotaz se spustí přes testovací data a zobrazí výsledky v dolní části stránky dotazu.</span><span class="sxs-lookup"><span data-stu-id="27782-131">After closing the dialog, your query will be run over the test data and you will see the results at the bottom of the Query page.</span></span>  
   
   ![Souhrn dotazu](./media/stream-analytics-write-queries/6-stream-analytics-write-queries.png)  

## <a name="get-help"></a><span data-ttu-id="27782-133">Podpora</span><span class="sxs-lookup"><span data-stu-id="27782-133">Get help</span></span>
<span data-ttu-id="27782-134">Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="27782-134">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="27782-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="27782-135">Next steps</span></span>
* [<span data-ttu-id="27782-136">Úvod do služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="27782-136">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="27782-137">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="27782-137">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="27782-138">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="27782-138">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="27782-139">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="27782-139">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="27782-140">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="27782-140">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

