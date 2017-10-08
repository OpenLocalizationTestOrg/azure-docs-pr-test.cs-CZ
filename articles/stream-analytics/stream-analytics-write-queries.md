---
title: dotazy toowrite aaaHow v Stream Analytics | Microsoft Docs
description: "Zápis dotazů v Stream Analytics a dotazování dat | učení segmentu cesty."
keywords: "jak toowrite dotazy, dotaz na data, napsat dotaz, zápis dotazů"
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
ms.openlocfilehash: b943c34f10afd2b21789afbd341c471a5f168729
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toowrite-queries-in-stream-analytics"></a><span data-ttu-id="d4ea0-104">Jak toowrite dotazy v Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d4ea0-104">How toowrite queries in Stream Analytics</span></span>
<span data-ttu-id="d4ea0-105">Zápis dotazů pro zpracování logiky v Azure Stream Analytics datového proudu je implementovaný jako "stojící dotaz", který je definován před úloha spustí a u dat provést, protože nedosáhne hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="d4ea0-105">Writing queries for stream processing logic in Azure Stream Analytics is implemented as a "standing query" that is defined before a job starts and executed on data as it reaches hello job.</span></span> <span data-ttu-id="d4ea0-106">transformaci dat Hello je vyjádřenou v jazyce SQL jako dotaz, který je z velké části podmnožinou T-SQL s některými přidat jazyková rozšíření jako [Oddílová](https://msdn.microsoft.com/library/azure/dn835019.aspx) použít dočasnou sémantiku tooexpress.</span><span class="sxs-lookup"><span data-stu-id="d4ea0-106">hello data transformation is expressed in a SQL-like query language, which is largely a subset of T-SQL with some added language extensions like [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx) used tooexpress temporal semantics.</span></span>

## <a name="writing-queries"></a><span data-ttu-id="d4ea0-107">Zápis dotazů:</span><span class="sxs-lookup"><span data-stu-id="d4ea0-107">Writing Queries:</span></span>
1. <span data-ttu-id="d4ea0-108">V úloze Stream Analytics na portálu pro správu Azure hello, klikněte na tlačítko **dotazu**.</span><span class="sxs-lookup"><span data-stu-id="d4ea0-108">In your Stream Analytics Job in hello Azure Management portal, click **Query**.</span></span>
   
    ![Vybrat dotaz](./media/stream-analytics-write-queries/1-stream-analytics-write-queries.png)  
   
    <span data-ttu-id="d4ea0-110">V hello portálu Azure, klikněte na **dotazu**.</span><span class="sxs-lookup"><span data-stu-id="d4ea0-110">In hello Azure Portal, click **Query**.</span></span>
   
    ![Vyberte Náhled dotazu](./media/stream-analytics-write-queries/query-preview-portal.png)  
2. <span data-ttu-id="d4ea0-112">Mají nové úlohy dotaz toohelp šablony vám pomůžou začít.</span><span class="sxs-lookup"><span data-stu-id="d4ea0-112">New jobs have a query template toohelp get you started.</span></span> <span data-ttu-id="d4ea0-113">Hello dotazů šablony provede "předávací" dotaz této projekty všechna pole ze vstupních událostech do výstup hello.</span><span class="sxs-lookup"><span data-stu-id="d4ea0-113">hello query template performs a "pass-through" query that projects all fields from input events into hello output.</span></span>  
   
   * <span data-ttu-id="d4ea0-114">Pokud jste definovali alespoň jeden vstup a výstup pro úlohu, můžete nahradit hello zástupný symbol "[YourOutputAlias]" a pole "[YourInputAlias]" hello aliasy hello vstup a výstup nejprve chcete použít.</span><span class="sxs-lookup"><span data-stu-id="d4ea0-114">If you have defined at least one input and output for your job, you can replace hello placeholder "[YourOutputAlias]" and "[YourInputAlias]" fields with hello aliases of hello input and output that you wish use first.</span></span> <span data-ttu-id="d4ea0-115">Kromě toho můžete pořád vytvářet a testování dotazu v hello portálu Azure Classic bez definování vstupy a výstupy u úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="d4ea0-115">In addition, you can still author and test your query in hello Azure Classic Portal without defining inputs and outputs on hello job.</span></span>
   * <span data-ttu-id="d4ea0-116">Pokud chcete tooperform větším objemem zpracování než jednoduchý průchozí, můžete upravit definice dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="d4ea0-116">If you wish tooperform more processing than a simple pass-through, you can edit hello query definition.</span></span> <span data-ttu-id="d4ea0-117">tooget začít s pro tvorbu dotazu, prohlédněte si některé běžný dotaz na zaznamenání vzory [zde](stream-analytics-stream-analytics-query-patterns.md).</span><span class="sxs-lookup"><span data-stu-id="d4ea0-117">tooget started with query authoring, take a look at some common query patterns are captured [here](stream-analytics-stream-analytics-query-patterns.md).</span></span>  
   
   ![Dotaz na data okna](./media/stream-analytics-write-queries/2-stream-analytics-write-queries.png)  

## <a name="toovalidate-query-data-is-working"></a><span data-ttu-id="d4ea0-119">dotaz na data toovalidate funguje:</span><span class="sxs-lookup"><span data-stu-id="d4ea0-119">toovalidate query data is working:</span></span>
<span data-ttu-id="d4ea0-120">Můžete otestovat, zda dotaz chová podle očekávání spuštěním v prohlížeči hello přes jeden nebo více místní soubory JSON obsahující testovací data.</span><span class="sxs-lookup"><span data-stu-id="d4ea0-120">You can test that your query behaves as expected by running it in hello browser over one or more local JSON files containing test data.</span></span> <span data-ttu-id="d4ea0-121">To nebude spuštění úlohy hello nebo mají vliv na všechny fakturace.</span><span class="sxs-lookup"><span data-stu-id="d4ea0-121">This will not start hello job or have any billing implications.</span></span>

> [!NOTE]
> <span data-ttu-id="d4ea0-122">Aktuálně testování dotazů v prohlížeči není podporována hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d4ea0-122">Currently in-browser query testing is not supported in hello Azure Portal.</span></span>  
> 
> 

1. <span data-ttu-id="d4ea0-123">Ujistěte se, že nejsou žádné chyby v dotazu hello (jinak hello tlačítko Testovat bude zakázáno) a potom klikněte na tlačítko Testovat hello.</span><span class="sxs-lookup"><span data-stu-id="d4ea0-123">Make sure that there are no errors in hello query (otherwise hello Test button will be disabled) and then click hello Test button.</span></span>  
   
   ![Dotaz na data testu](./media/stream-analytics-write-queries/3-stream-analytics-write-queries.png)  
2. <span data-ttu-id="d4ea0-125">Bude výzvami toospecify soubory pro každou z hello vstupů odkazovaná v dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="d4ea0-125">You will be prompted toospecify files for each of hello inputs referenced in hello query.</span></span> <span data-ttu-id="d4ea0-126">V tomto příkladu je ponechán hello šablony dotazu jako-se, takže hello dialogu je dotaz na vstup s názvem "yourinputalias".</span><span class="sxs-lookup"><span data-stu-id="d4ea0-126">In this example, hello template query is left as-is, so hello dialog is prompting for an input named "yourinputalias".</span></span>  
   
   ![Dotaz na Data testu](./media/stream-analytics-write-queries/4-stream-analytics-write-queries.png)  
3. <span data-ttu-id="d4ea0-128">Procházejte tooa testovací soubor.</span><span class="sxs-lookup"><span data-stu-id="d4ea0-128">Browse tooa test file.</span></span> <span data-ttu-id="d4ea0-129">Několik ukázkových souborů, které jsou k dispozici na [githubu](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) a můžete také načíst ukázková data z vlastního datového proudu vstupy data prostřednictvím hello funkce ukázková Data na kartě hello vstupy.</span><span class="sxs-lookup"><span data-stu-id="d4ea0-129">Several sample files are available on [github](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) and you can also retrieve sample data from your own data stream inputs via hello Sample Data function on hello inputs tab.</span></span>  
   
   ![Vstupní dotaz](./media/stream-analytics-write-queries/5-stream-analytics-write-queries.png)  
4. <span data-ttu-id="d4ea0-131">Po zavření dialogu hello, se spustí dotaz přes hello testovací data a zobrazí se hello výsledků v dolní části hello hello dotazu stránky.</span><span class="sxs-lookup"><span data-stu-id="d4ea0-131">After closing hello dialog, your query will be run over hello test data and you will see hello results at hello bottom of hello Query page.</span></span>  
   
   ![Souhrn dotazu](./media/stream-analytics-write-queries/6-stream-analytics-write-queries.png)  

## <a name="get-help"></a><span data-ttu-id="d4ea0-133">Podpora</span><span class="sxs-lookup"><span data-stu-id="d4ea0-133">Get help</span></span>
<span data-ttu-id="d4ea0-134">Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="d4ea0-134">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="d4ea0-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d4ea0-135">Next steps</span></span>
* [<span data-ttu-id="d4ea0-136">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d4ea0-136">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="d4ea0-137">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d4ea0-137">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="d4ea0-138">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d4ea0-138">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="d4ea0-139">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="d4ea0-139">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="d4ea0-140">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d4ea0-140">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

