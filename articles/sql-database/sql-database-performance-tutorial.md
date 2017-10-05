---
title: "Řešení potíží s výkonem a optimalizací databáze | Microsoft Docs"
description: "Platí doporučení výkonu pro vaše databáze SQL a také mazat získáte přehled o výkonu dotazů spuštěným pro vaši databázi"
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
ms.openlocfilehash: f9ae96cdc80c347593f229cb2fce3f2d4d8e7caf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-performance-issues-and-optimize-your-database"></a><span data-ttu-id="a165d-103">Řešení potíží s výkonem a optimalizací databáze</span><span class="sxs-lookup"><span data-stu-id="a165d-103">Troubleshoot performance issues and optimize your database</span></span>

<span data-ttu-id="a165d-104">Častým důvodem toho, že databáze je pomalá, jsou chybějící indexy a nedostatečně optimalizované dotazy.</span><span class="sxs-lookup"><span data-stu-id="a165d-104">Missing indexes and poorly optimized queries are common reasons for poor database performance.</span></span> <span data-ttu-id="a165d-105">V tomto kurzu jste postup:</span><span class="sxs-lookup"><span data-stu-id="a165d-105">In this tutorial you learn to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="a165d-106">Zkontrolujte, použití a vracet doporučení pro zlepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="a165d-106">Review, apply and revert performance improvement recommendations</span></span>
> * <span data-ttu-id="a165d-107">Najít dotazy s využitím vysoké prostředků</span><span class="sxs-lookup"><span data-stu-id="a165d-107">Find queries with high resource utilization</span></span>
> * <span data-ttu-id="a165d-108">Najít dlouho běžící dotazy</span><span class="sxs-lookup"><span data-stu-id="a165d-108">Find long running queries</span></span>

> <span data-ttu-id="a165d-109">Je nutné nepřetržité úlohy na databázi s problémy s výkonem – chybí indexu třeba přijímat doporučení.</span><span class="sxs-lookup"><span data-stu-id="a165d-109">You need a continuous workload on a database with performance issues – missing an index for example to receive a recommendation.</span></span>
>

## <a name="log-in-to-the-azure-portal"></a><span data-ttu-id="a165d-110">Přihlášení k portálu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a165d-110">Log in to the Azure portal</span></span>

<span data-ttu-id="a165d-111">Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="a165d-111">Log in to the [Azure portal](https://portal.azure.com/).</span></span>

## <a name="review-and-apply-a-recommendation"></a><span data-ttu-id="a165d-112">Zkontrolovat a použít doporučení</span><span class="sxs-lookup"><span data-stu-id="a165d-112">Review and apply a recommendation</span></span>

<span data-ttu-id="a165d-113">Použijte následující postup platí doporučení ze systému pro vaši databázi:</span><span class="sxs-lookup"><span data-stu-id="a165d-113">Follow these steps to apply a recommendation from the system for your database:</span></span>

1. <span data-ttu-id="a165d-114">Klikněte **výkonu doporučení** nabídky v okně databáze.</span><span class="sxs-lookup"><span data-stu-id="a165d-114">Click the **Performance recommendations** menu in the database blade.</span></span>

    ![doporučení výkonu](./media/sql-database-performance-tutorial/perf_recommendations.png)

2. <span data-ttu-id="a165d-116">Seznam doporučení vyberte active doporučení.</span><span class="sxs-lookup"><span data-stu-id="a165d-116">From the list of recommendations, select an active recommendation.</span></span> <span data-ttu-id="a165d-117">V tomto příkladu Create Index.</span><span class="sxs-lookup"><span data-stu-id="a165d-117">In this example, Create Index.</span></span>

    ![Vyberte doporučení](./media/sql-database-performance-tutorial/create_index.png)

3. <span data-ttu-id="a165d-119">Doporučení použít kliknutím **použít** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a165d-119">Apply the recommendation by clicking the **Apply** button.</span></span> <span data-ttu-id="a165d-120">Volitelně můžete zkontrolovat podrobnosti o doporučení a zjistit skriptu T-SQL, které by šlo spustit kliknutím na **zobrazit skript** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a165d-120">Optionally, review the recommendation details and see the T-SQL script to  be executed by clicking on **View Script** button.</span></span>

    ![použít doporučení](./media/sql-database-performance-tutorial/apply.png)

4. <span data-ttu-id="a165d-122">[Nepovinné] Povolte automatické ladění pro doporučení automaticky použije.</span><span class="sxs-lookup"><span data-stu-id="a165d-122">[Optional] Enable automatic tuning for recommendations to be applied automatically.</span></span>

    ![Automatické ladění](./media/sql-database-performance-tutorial/auto_tuning.png)

## <a name="revert-a-recommendation"></a><span data-ttu-id="a165d-124">Vrátit doporučení</span><span class="sxs-lookup"><span data-stu-id="a165d-124">Revert a recommendation</span></span>

<span data-ttu-id="a165d-125">Databáze Advisor monitoruje každé doporučení implementována.</span><span class="sxs-lookup"><span data-stu-id="a165d-125">The Database Advisor monitors every recommendation implemented.</span></span> <span data-ttu-id="a165d-126">Pokud doporučení nedojde ke zvýšení zatížení se budou automaticky zrušeny.</span><span class="sxs-lookup"><span data-stu-id="a165d-126">If a recommendation doesn't improve the workload it will be automatically reverted.</span></span> <span data-ttu-id="a165d-127">Ručně vrácení doporučení je možné, ale ve většině případů není nezbytné.</span><span class="sxs-lookup"><span data-stu-id="a165d-127">Manually reverting a recommendation is possible, but not necessary in most cases.</span></span> <span data-ttu-id="a165d-128">Chcete-li vrátit doporučení:</span><span class="sxs-lookup"><span data-stu-id="a165d-128">To revert a recommendation:</span></span>

1. <span data-ttu-id="a165d-129">Přejděte do nabídky doporučení výkonu a vyberte jednu z použité doporučení.</span><span class="sxs-lookup"><span data-stu-id="a165d-129">Go to the performance recommendations menu and select one of the applied recommendations.</span></span>

    ![Vyberte doporučení](./media/sql-database-performance-tutorial/select.png)

2. <span data-ttu-id="a165d-131">V podrobném zobrazení, klikněte na tlačítko **vrátit**.</span><span class="sxs-lookup"><span data-stu-id="a165d-131">In the details view, click **Revert**.</span></span>

    ![vrátit doporučení](./media/sql-database-performance-tutorial/revert.png)

## <a name="find-the-query-that-consumes-the-most-resources"></a><span data-ttu-id="a165d-133">Vyhledat dotaz, který využívá nejvíce zdrojů</span><span class="sxs-lookup"><span data-stu-id="a165d-133">Find the query that consumes the most resources</span></span>

<span data-ttu-id="a165d-134">Použijte následující postup vyhledat dotaz využívání nejvíce zdrojů:</span><span class="sxs-lookup"><span data-stu-id="a165d-134">Follow these steps to find the query consuming the most resources:</span></span>

1. <span data-ttu-id="a165d-135">Klikněte na **Query Performance Insight** nabídky v okně databáze.</span><span class="sxs-lookup"><span data-stu-id="a165d-135">Click on the **Query Performance Insight** menu in the database blade.</span></span>

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/query_perf_insights.png)

2. <span data-ttu-id="a165d-137">Vyberte typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="a165d-137">Select a resource type.</span></span>

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/select_resource_type.png)

3. <span data-ttu-id="a165d-139">Vyberte první dotaz v tabulce.</span><span class="sxs-lookup"><span data-stu-id="a165d-139">Select the first query in the table.</span></span>

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/select_query.png)

4. <span data-ttu-id="a165d-141">Zkontrolujte podrobnosti o dotazu.</span><span class="sxs-lookup"><span data-stu-id="a165d-141">Review the query details.</span></span>

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/query_details.png)

## <a name="find-the-longest-running-query"></a><span data-ttu-id="a165d-143">Vyhledat nejdéle probíhající dotaz</span><span class="sxs-lookup"><span data-stu-id="a165d-143">Find the longest running query</span></span>

1. <span data-ttu-id="a165d-144">Přejděte na Query Performance Insight a vyberte **dlouho běžící dotazy** kartě.</span><span class="sxs-lookup"><span data-stu-id="a165d-144">Go to Query Performance Insight and select the **Long running queries** tab.</span></span>

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/long_running.png)

3. <span data-ttu-id="a165d-146">Vyberte první dotaz v tabulce.</span><span class="sxs-lookup"><span data-stu-id="a165d-146">Select the first query in the table.</span></span>

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/select_first_query.png)

4. <span data-ttu-id="a165d-148">Zkontrolujte podrobnosti o dotazu.</span><span class="sxs-lookup"><span data-stu-id="a165d-148">Review the query details.</span></span>

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/review_query_details.png)



## <a name="next-steps"></a><span data-ttu-id="a165d-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a165d-150">Next steps</span></span> 
<span data-ttu-id="a165d-151">Častým důvodem toho, že databáze je pomalá, jsou chybějící indexy a nedostatečně optimalizované dotazy.</span><span class="sxs-lookup"><span data-stu-id="a165d-151">Missing indexes and poorly optimized queries are common reasons for poor database performance.</span></span> <span data-ttu-id="a165d-152">V tomto kurzu jste se dozvěděli na:</span><span class="sxs-lookup"><span data-stu-id="a165d-152">In this tutorial you learned to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="a165d-153">Zkontrolujte, použití a vracet doporučení pro zlepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="a165d-153">Review, apply and revert performance improvement recommendations</span></span>
> * <span data-ttu-id="a165d-154">Najít dotazy s využitím vysoké prostředků</span><span class="sxs-lookup"><span data-stu-id="a165d-154">Find queries with high resource utilization</span></span>
> * <span data-ttu-id="a165d-155">Najít dlouho běžící dotazy</span><span class="sxs-lookup"><span data-stu-id="a165d-155">Find long running queries</span></span>

[<span data-ttu-id="a165d-156">Tipy pro optimalizaci výkonu databáze SQL</span><span class="sxs-lookup"><span data-stu-id="a165d-156">SQL Database performance tuning tips</span></span>](https://docs.microsoft.com/azure/sql-database/sql-database-troubleshoot-performance)
