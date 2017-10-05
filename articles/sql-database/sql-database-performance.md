---
title: "Monitorování a zlepšit výkon - Azure SQL Database | Microsoft Docs"
description: "Databáze SQL Azure poskytuje nástroje pro sledování výkonu, který vám pomůže identifikovat oblasti, které může zlepšit výkon aktuální dotaz."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: a60b75ac-cf27-4d73-8322-ee4d4c448aa2
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/19/2016
ms.author: sstein
ms.openlocfilehash: 522b932ab055978c52f085dbaa36095bb6b77962
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-and-improve-performance"></a><span data-ttu-id="3c753-103">Monitorování a zlepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="3c753-103">Monitor and improve performance</span></span>
<span data-ttu-id="3c753-104">Azure SQL Database identifikuje potenciální problémy ve vaší databázi a doporučuje akce, které může zlepšit výkon vašich úloh zadáním inteligentního vyladění akce a doporučení.</span><span class="sxs-lookup"><span data-stu-id="3c753-104">Azure SQL Database identifies potential problems in your database and recommends actions that can improve performance of your workload by providing intelligent tuning actions and recommendations.</span></span>

<span data-ttu-id="3c753-105">Chcete-li zkontrolovat výkon databáze, použijte **výkonu** dlaždici na stránce Přehled, nebo přejděte dolů "Podpora + řešení potíží" část:</span><span class="sxs-lookup"><span data-stu-id="3c753-105">To review your database performance, use the **Performance** tile on the Overview page, or navigate down to "Support + troubleshooting" section:</span></span>

   ![Zobrazení výkonu](./media/sql-database-performance/entries.png)

<span data-ttu-id="3c753-107">V "Podpora + řešení potíží" části, můžete použít následující stránky:</span><span class="sxs-lookup"><span data-stu-id="3c753-107">In the "Support + troubleshooting" section, you can use the following pages:</span></span>


1. <span data-ttu-id="3c753-108">[Přehled výkonnostní](#performance-overview) sledovat výkon vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="3c753-108">[Performance overview](#performance-overview) to monitor performance of your database.</span></span> 
2. <span data-ttu-id="3c753-109">[Doporučení pro optimální výkon](#performance-recommendations) najít doporučení výkonu, které může zlepšit výkon vašich úloh.</span><span class="sxs-lookup"><span data-stu-id="3c753-109">[Performance recommendations](#performance-recommendations) to find performance recommendations that can improve performance of your workload.</span></span>
3. <span data-ttu-id="3c753-110">[Dotaz na informace o výkonu](#query-performance-insight) najít dotazy na nejvyšší prostředky.</span><span class="sxs-lookup"><span data-stu-id="3c753-110">[Query Performance Insight](#query-performance-insight) to find top resource consuming queries.</span></span>
4. <span data-ttu-id="3c753-111">[Automatické ladění](#automatic-tuning) umožníte automaticky optimalizací databáze Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="3c753-111">[Automatic tuning](#automatic-tuning) to let Azure SQL Database automatically optimize your database.</span></span>

## <a name="performance-overview"></a><span data-ttu-id="3c753-112">Přehled výkonnostní</span><span class="sxs-lookup"><span data-stu-id="3c753-112">Performance Overview</span></span>
<span data-ttu-id="3c753-113">Toto zobrazení obsahuje souhrn výkon databáze a pomůže vám s výkonem, ladění a řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="3c753-113">This view provides a summary of your database performance, and helps you with performance tuning and troubleshooting.</span></span> 

![Výkon](./media/sql-database-performance/performance.png)

* <span data-ttu-id="3c753-115">**Doporučení** dlaždice obsahuje rozpis systémů ladění doporučení pro vaši databázi (první tři doporučení jsou uvedené. Pokud existují další).</span><span class="sxs-lookup"><span data-stu-id="3c753-115">The **Recommendations** tile provides a breakdown of tuning recommendations for your database (top three recommendations are shown if there are more).</span></span> <span data-ttu-id="3c753-116">Kliknutím na tuto dlaždici přejdete k  **[výkonu doporučení](#performance-recommendations)**.</span><span class="sxs-lookup"><span data-stu-id="3c753-116">Clicking this tile takes you to **[Performance recommendations](#performance-recommendations)**.</span></span> 
* <span data-ttu-id="3c753-117">**Ladění aktivity** dlaždice poskytuje souhrn probíhající a dokončené ladění akce pro vaši databázi, která poskytuje rychlý přehled do historie ladění aktivity.</span><span class="sxs-lookup"><span data-stu-id="3c753-117">The **Tuning activity** tile provides a summary of the ongoing and completed tuning actions for your database, giving you a quick view into the history of tuning activity.</span></span> <span data-ttu-id="3c753-118">Kliknutím na tuto dlaždici přejdete na úplné ladění zobrazení historie pro vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="3c753-118">Clicking this tile takes you to the full tuning history view for your database.</span></span>
* <span data-ttu-id="3c753-119">**Automatické ladění** dlaždici ukazuje [automatické ladění konfigurace](sql-database-automatic-tuning-enable.md) pro vaši databázi (optimalizace pro možnosti, které budou automaticky použita pro vaši databázi).</span><span class="sxs-lookup"><span data-stu-id="3c753-119">The **Auto-tuning** tile shows the [auto-tuning configuration](sql-database-automatic-tuning-enable.md) for your database (tuning options that are automatically applied to your database).</span></span> <span data-ttu-id="3c753-120">Kliknutím na tuto dlaždici, otevře se dialogové okno Konfigurace automatizace.</span><span class="sxs-lookup"><span data-stu-id="3c753-120">Clicking this tile opens the automation configuration dialog.</span></span>
* <span data-ttu-id="3c753-121">**Dotazy na databázi** dlaždice zobrazuje souhrn výkon dotazů pro databázi (celkový počet jednotek DTU využití a horní na prostředky dotazy).</span><span class="sxs-lookup"><span data-stu-id="3c753-121">The **Database queries** tile shows the summary of the query performance for your database (overall DTU usage and top resource consuming queries).</span></span> <span data-ttu-id="3c753-122">Kliknutím na tuto dlaždici přejdete k  **[Query Performance Insight](#query-performance-insight)**.</span><span class="sxs-lookup"><span data-stu-id="3c753-122">Clicking this tile takes you to **[Query Performance Insight](#query-performance-insight)**.</span></span>

## <a name="performance-recommendations"></a><span data-ttu-id="3c753-123">Doporučení výkonu</span><span class="sxs-lookup"><span data-stu-id="3c753-123">Performance recommendations</span></span>
<span data-ttu-id="3c753-124">Tato stránka obsahuje inteligentního [ladění doporučení](sql-database-advisor.md) , může zlepšit výkon vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="3c753-124">This page provides intelligent [tuning recommendations](sql-database-advisor.md) that can improve your database's performance.</span></span> <span data-ttu-id="3c753-125">Na této stránce se zobrazují následující typy doporučení:</span><span class="sxs-lookup"><span data-stu-id="3c753-125">The following types of recommendations are shown on this page:</span></span>

* <span data-ttu-id="3c753-126">Doporučení pro indexy, které se vytvořit nebo vyřadit.</span><span class="sxs-lookup"><span data-stu-id="3c753-126">Recommendations on which indexes to create or drop.</span></span>
* <span data-ttu-id="3c753-127">Doporučení, když jsou zjištěny problémy schématu v databázi.</span><span class="sxs-lookup"><span data-stu-id="3c753-127">Recommendations when schema issues are identified in the database.</span></span>
* <span data-ttu-id="3c753-128">Doporučení pro dotazy využívat parametrizované dotazy.</span><span class="sxs-lookup"><span data-stu-id="3c753-128">Recommendations when queries can benefit from parameterized queries.</span></span>

![Výkon](./media/sql-database-performance/recommendations.png)

<span data-ttu-id="3c753-130">Můžete také získat úplnou historii ladění akce, které byly použity v minulosti.</span><span class="sxs-lookup"><span data-stu-id="3c753-130">You can also find complete history of tuning actions that were applied in the past.</span></span>

<span data-ttu-id="3c753-131">Zjistěte, jak najít výkonu doporučení v operátoru apply [najít a použít výkonu doporučení](sql-database-advisor-portal.md) článku.</span><span class="sxs-lookup"><span data-stu-id="3c753-131">Learn how to find an apply performance recommendations in [Find and apply performance recommendations](sql-database-advisor-portal.md) article.</span></span>

## <a name="automatic-tuning"></a><span data-ttu-id="3c753-132">Automatické ladění</span><span class="sxs-lookup"><span data-stu-id="3c753-132">Automatic tuning</span></span>
<span data-ttu-id="3c753-133">Databáze Azure SQL můžete vyladit výkon databáze automaticky použitím [výkonu doporučení](sql-database-advisor.md).</span><span class="sxs-lookup"><span data-stu-id="3c753-133">Azure SQL Databases can automatically tune database performance by applying [performance recommendations](sql-database-advisor.md).</span></span> <span data-ttu-id="3c753-134">Další informace, přečtěte si [automatické ladění článku](sql-database-automatic-tuning.md).</span><span class="sxs-lookup"><span data-stu-id="3c753-134">To learn more, read [Automatic tuning article](sql-database-automatic-tuning.md).</span></span> <span data-ttu-id="3c753-135">Chcete-li ji povolit, přečtěte si [povolení automatické ladění](sql-database-automatic-tuning-enable.md).</span><span class="sxs-lookup"><span data-stu-id="3c753-135">To enable it, read [how to enable automatic tuning](sql-database-automatic-tuning-enable.md).</span></span>

## <a name="query-performance-insight"></a><span data-ttu-id="3c753-136">Query Performance Insight</span><span class="sxs-lookup"><span data-stu-id="3c753-136">Query Performance Insight</span></span>
<span data-ttu-id="3c753-137">[Dotaz na informace o výkonu](sql-database-query-performance.md) umožňuje trávit méně času tím, že poskytuje řešení potíží s výkonem databáze:</span><span class="sxs-lookup"><span data-stu-id="3c753-137">[Query Performance Insight](sql-database-query-performance.md) allows you to spend less time troubleshooting database performance by providing:</span></span>

* <span data-ttu-id="3c753-138">Podrobnější přehled o vaší spotřeby prostředků (DTU) databází.</span><span class="sxs-lookup"><span data-stu-id="3c753-138">Deeper insight into your databases resource (DTU) consumption.</span></span> 
* <span data-ttu-id="3c753-139">Nejvíce využívají procesor dotazů, které lze ladit potenciálně pro zlepšení výkonu.</span><span class="sxs-lookup"><span data-stu-id="3c753-139">The top CPU consuming queries, which can potentially be tuned for improved performance.</span></span> 
* <span data-ttu-id="3c753-140">Možnost k podrobnostem na podrobné informace o dotazu.</span><span class="sxs-lookup"><span data-stu-id="3c753-140">The ability to drill down into the details of a query.</span></span> 

  ![řídicí panel výkonu](./media/sql-database-query-performance/performance.png)

<span data-ttu-id="3c753-142">Další informace o této stránce naleznete v článku  **[použití Query Performance Insight](sql-database-query-performance.md)**.</span><span class="sxs-lookup"><span data-stu-id="3c753-142">Find more information about this page in the article **[How to use Query Performance Insight](sql-database-query-performance.md)**.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3c753-143">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3c753-143">Additional resources</span></span>
* [<span data-ttu-id="3c753-144">Azure SQL Database – Průvodce výkonem pro izolované databáze</span><span class="sxs-lookup"><span data-stu-id="3c753-144">Azure SQL Database performance guidance for single databases</span></span>](sql-database-performance-guidance.md)
* [<span data-ttu-id="3c753-145">Pokud má být použita fondu elastické databáze?</span><span class="sxs-lookup"><span data-stu-id="3c753-145">When should an elastic pool be used?</span></span>](sql-database-elastic-pool-guidance.md)

