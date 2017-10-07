---
title: "výkon aaaTroubleshoot problémy a optimalizovat databázi | Microsoft Docs"
description: "Použít tooyour doporučení výkonu databáze SQL a také mazat jak hello toogain přehled o výkonu dotazů hello spuštěným pro vaši databázi"
metakeywords: azure sql database performance monitoring recommendation
services: sql-database
documentationcenter: 
manager: jhubbard
author: jan-eng
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,monitor & tune
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: janeng
ms.openlocfilehash: e948d30ac74eecf45420d5d77ef55e3c0b6f3f47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-performance-issues-and-optimize-your-database"></a><span data-ttu-id="54f18-103">Řešení potíží s výkonem a optimalizací databáze</span><span class="sxs-lookup"><span data-stu-id="54f18-103">Troubleshoot performance issues and optimize your database</span></span>

<span data-ttu-id="54f18-104">Častým důvodem toho, že databáze je pomalá, jsou chybějící indexy a nedostatečně optimalizované dotazy.</span><span class="sxs-lookup"><span data-stu-id="54f18-104">Missing indexes and poorly optimized queries are common reasons for poor database performance.</span></span> <span data-ttu-id="54f18-105">V tomto kurzu jste postup:</span><span class="sxs-lookup"><span data-stu-id="54f18-105">In this tutorial you learn to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="54f18-106">Zkontrolujte, použití a vracet doporučení pro zlepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="54f18-106">Review, apply and revert performance improvement recommendations</span></span>
> * <span data-ttu-id="54f18-107">Najít dotazy s využitím vysoké prostředků</span><span class="sxs-lookup"><span data-stu-id="54f18-107">Find queries with high resource utilization</span></span>
> * <span data-ttu-id="54f18-108">Najít dlouho běžící dotazy</span><span class="sxs-lookup"><span data-stu-id="54f18-108">Find long running queries</span></span>

> <span data-ttu-id="54f18-109">Je nutné nepřetržité úlohy na databázi s problémy s výkonem – chybí například tooreceive doporučení indexu.</span><span class="sxs-lookup"><span data-stu-id="54f18-109">You need a continuous workload on a database with performance issues – missing an index for example tooreceive a recommendation.</span></span>
>

## <a name="log-in-toohello-azure-portal"></a><span data-ttu-id="54f18-110">Přihlaste se toohello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="54f18-110">Log in toohello Azure portal</span></span>

<span data-ttu-id="54f18-111">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="54f18-111">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>

## <a name="review-and-apply-a-recommendation"></a><span data-ttu-id="54f18-112">Zkontrolovat a použít doporučení</span><span class="sxs-lookup"><span data-stu-id="54f18-112">Review and apply a recommendation</span></span>

<span data-ttu-id="54f18-113">Postupujte podle těchto kroků tooapply doporučení z hello systému pro vaši databázi:</span><span class="sxs-lookup"><span data-stu-id="54f18-113">Follow these steps tooapply a recommendation from hello system for your database:</span></span>

1. <span data-ttu-id="54f18-114">Klikněte na tlačítko hello **výkonu doporučení** nabídky v okně databáze hello.</span><span class="sxs-lookup"><span data-stu-id="54f18-114">Click hello **Performance recommendations** menu in hello database blade.</span></span>

    ![doporučení výkonu](./media/sql-database-performance-tutorial/perf_recommendations.png)

2. <span data-ttu-id="54f18-116">Hello seznam doporučení vyberte active doporučení.</span><span class="sxs-lookup"><span data-stu-id="54f18-116">From hello list of recommendations, select an active recommendation.</span></span> <span data-ttu-id="54f18-117">V tomto příkladu Create Index.</span><span class="sxs-lookup"><span data-stu-id="54f18-117">In this example, Create Index.</span></span>

    ![Vyberte doporučení](./media/sql-database-performance-tutorial/create_index.png)

3. <span data-ttu-id="54f18-119">Kliknutím na hello použít hello doporučení **použít** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="54f18-119">Apply hello recommendation by clicking hello **Apply** button.</span></span> <span data-ttu-id="54f18-120">Volitelně můžete zkontrolovat podrobnosti o doporučení hello a zjistit hello T-SQL skriptu příliš provést kliknutím na **zobrazit skript** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="54f18-120">Optionally, review hello recommendation details and see hello T-SQL script too be executed by clicking on **View Script** button.</span></span>

    ![použít doporučení](./media/sql-database-performance-tutorial/apply.png)

4. <span data-ttu-id="54f18-122">[Nepovinné] Povolte automatické ladění pro toobe doporučení použít automaticky.</span><span class="sxs-lookup"><span data-stu-id="54f18-122">[Optional] Enable automatic tuning for recommendations toobe applied automatically.</span></span>

    ![Automatické ladění](./media/sql-database-performance-tutorial/auto_tuning.png)

## <a name="revert-a-recommendation"></a><span data-ttu-id="54f18-124">Vrátit doporučení</span><span class="sxs-lookup"><span data-stu-id="54f18-124">Revert a recommendation</span></span>

<span data-ttu-id="54f18-125">Hello databáze Advisor monitoruje každé doporučení implementována.</span><span class="sxs-lookup"><span data-stu-id="54f18-125">hello Database Advisor monitors every recommendation implemented.</span></span> <span data-ttu-id="54f18-126">Pokud doporučení nedojde ke zlepšení hello zatížení se budou automaticky zrušeny.</span><span class="sxs-lookup"><span data-stu-id="54f18-126">If a recommendation doesn't improve hello workload it will be automatically reverted.</span></span> <span data-ttu-id="54f18-127">Ručně vrácení doporučení je možné, ale ve většině případů není nezbytné.</span><span class="sxs-lookup"><span data-stu-id="54f18-127">Manually reverting a recommendation is possible, but not necessary in most cases.</span></span> <span data-ttu-id="54f18-128">toorevert doporučení:</span><span class="sxs-lookup"><span data-stu-id="54f18-128">toorevert a recommendation:</span></span>

1. <span data-ttu-id="54f18-129">Přejděte toohello výkonu doporučení nabídku a vyberte jednu z hello použít doporučení.</span><span class="sxs-lookup"><span data-stu-id="54f18-129">Go toohello performance recommendations menu and select one of hello applied recommendations.</span></span>

    ![Vyberte doporučení](./media/sql-database-performance-tutorial/select.png)

2. <span data-ttu-id="54f18-131">V zobrazení Podrobnosti o hello, klikněte na tlačítko **vrátit**.</span><span class="sxs-lookup"><span data-stu-id="54f18-131">In hello details view, click **Revert**.</span></span>

    ![vrátit doporučení](./media/sql-database-performance-tutorial/revert.png)

## <a name="find-hello-query-that-consumes-hello-most-resources"></a><span data-ttu-id="54f18-133">Najít hello dotazu, který využívá hello nejvíce zdrojů</span><span class="sxs-lookup"><span data-stu-id="54f18-133">Find hello query that consumes hello most resources</span></span>

<span data-ttu-id="54f18-134">Postupujte podle těchto kroků dotazu hello toofind využívání hello nejvíce zdrojů:</span><span class="sxs-lookup"><span data-stu-id="54f18-134">Follow these steps toofind hello query consuming hello most resources:</span></span>

1. <span data-ttu-id="54f18-135">Klikněte na hello **Query Performance Insight** nabídky v okně databáze hello.</span><span class="sxs-lookup"><span data-stu-id="54f18-135">Click on hello **Query Performance Insight** menu in hello database blade.</span></span>

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/query_perf_insights.png)

2. <span data-ttu-id="54f18-137">Vyberte typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="54f18-137">Select a resource type.</span></span>

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/select_resource_type.png)

3. <span data-ttu-id="54f18-139">Vyberte první dotaz hello v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="54f18-139">Select hello first query in hello table.</span></span>

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/select_query.png)

4. <span data-ttu-id="54f18-141">Zkontrolujte podrobnosti hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="54f18-141">Review hello query details.</span></span>

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/query_details.png)

## <a name="find-hello-longest-running-query"></a><span data-ttu-id="54f18-143">Vyhledat nejdelší spuštěné dotaz hello</span><span class="sxs-lookup"><span data-stu-id="54f18-143">Find hello longest running query</span></span>

1. <span data-ttu-id="54f18-144">Přejděte tooQuery informace o výkonu a vyberte možnost hello **dlouho běžící dotazy** kartě.</span><span class="sxs-lookup"><span data-stu-id="54f18-144">Go tooQuery Performance Insight and select hello **Long running queries** tab.</span></span>

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/long_running.png)

3. <span data-ttu-id="54f18-146">Vyberte první dotaz hello v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="54f18-146">Select hello first query in hello table.</span></span>

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/select_first_query.png)

4. <span data-ttu-id="54f18-148">Zkontrolujte podrobnosti hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="54f18-148">Review hello query details.</span></span>

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/review_query_details.png)



## <a name="next-steps"></a><span data-ttu-id="54f18-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="54f18-150">Next steps</span></span> 
<span data-ttu-id="54f18-151">Častým důvodem toho, že databáze je pomalá, jsou chybějící indexy a nedostatečně optimalizované dotazy.</span><span class="sxs-lookup"><span data-stu-id="54f18-151">Missing indexes and poorly optimized queries are common reasons for poor database performance.</span></span> <span data-ttu-id="54f18-152">V tomto kurzu jste se dozvěděli na:</span><span class="sxs-lookup"><span data-stu-id="54f18-152">In this tutorial you learned to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="54f18-153">Zkontrolujte, použití a vracet doporučení pro zlepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="54f18-153">Review, apply and revert performance improvement recommendations</span></span>
> * <span data-ttu-id="54f18-154">Najít dotazy s využitím vysoké prostředků</span><span class="sxs-lookup"><span data-stu-id="54f18-154">Find queries with high resource utilization</span></span>
> * <span data-ttu-id="54f18-155">Najít dlouho běžící dotazy</span><span class="sxs-lookup"><span data-stu-id="54f18-155">Find long running queries</span></span>

[<span data-ttu-id="54f18-156">Tipy pro optimalizaci výkonu databáze SQL</span><span class="sxs-lookup"><span data-stu-id="54f18-156">SQL Database performance tuning tips</span></span>](https://docs.microsoft.com/azure/sql-database/sql-database-troubleshoot-performance)
