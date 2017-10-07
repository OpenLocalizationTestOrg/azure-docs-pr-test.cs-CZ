---
title: "aaaMonitor a zlepšit výkon - Azure SQL Database | Microsoft Docs"
description: "Dobrý den, které poskytuje Azure SQL Database výkon nástroje toohelp určit oblasti, které může zlepšit výkon aktuální dotaz."
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
ms.openlocfilehash: 84b8a1bc62698a29deb49e765f208bd7e14d0870
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-improve-performance"></a><span data-ttu-id="5fe54-103">Monitorování a zlepšení výkonu</span><span class="sxs-lookup"><span data-stu-id="5fe54-103">Monitor and improve performance</span></span>
<span data-ttu-id="5fe54-104">Azure SQL Database identifikuje potenciální problémy ve vaší databázi a doporučuje akce, které může zlepšit výkon vašich úloh zadáním inteligentního vyladění akce a doporučení.</span><span class="sxs-lookup"><span data-stu-id="5fe54-104">Azure SQL Database identifies potential problems in your database and recommends actions that can improve performance of your workload by providing intelligent tuning actions and recommendations.</span></span>

<span data-ttu-id="5fe54-105">tooreview výkon databáze, použijte hello **výkonu** dlaždici na stránce Přehled hello nebo přejděte dolů příliš "Podpora + řešení potíží" část:</span><span class="sxs-lookup"><span data-stu-id="5fe54-105">tooreview your database performance, use hello **Performance** tile on hello Overview page, or navigate down too"Support + troubleshooting" section:</span></span>

   ![Zobrazení výkonu](./media/sql-database-performance/entries.png)

<span data-ttu-id="5fe54-107">V části "Podpora + řešení potíží" hello můžete použít hello následující stránky:</span><span class="sxs-lookup"><span data-stu-id="5fe54-107">In hello "Support + troubleshooting" section, you can use hello following pages:</span></span>


1. <span data-ttu-id="5fe54-108">[Přehled výkonnostní](#performance-overview) toomonitor výkon vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="5fe54-108">[Performance overview](#performance-overview) toomonitor performance of your database.</span></span> 
2. <span data-ttu-id="5fe54-109">[Doporučení pro optimální výkon](#performance-recommendations) toofind výkonu doporučení, které může zlepšit výkon vašich úloh.</span><span class="sxs-lookup"><span data-stu-id="5fe54-109">[Performance recommendations](#performance-recommendations) toofind performance recommendations that can improve performance of your workload.</span></span>
3. <span data-ttu-id="5fe54-110">[Dotaz na informace o výkonu](#query-performance-insight) toofind nejvyšší na prostředky dotazy.</span><span class="sxs-lookup"><span data-stu-id="5fe54-110">[Query Performance Insight](#query-performance-insight) toofind top resource consuming queries.</span></span>
4. <span data-ttu-id="5fe54-111">[Automatické ladění](#automatic-tuning) toolet Azure SQL Database automaticky optimalizací databáze.</span><span class="sxs-lookup"><span data-stu-id="5fe54-111">[Automatic tuning](#automatic-tuning) toolet Azure SQL Database automatically optimize your database.</span></span>

## <a name="performance-overview"></a><span data-ttu-id="5fe54-112">Přehled výkonnostní</span><span class="sxs-lookup"><span data-stu-id="5fe54-112">Performance Overview</span></span>
<span data-ttu-id="5fe54-113">Toto zobrazení obsahuje souhrn výkon databáze a pomůže vám s výkonem, ladění a řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="5fe54-113">This view provides a summary of your database performance, and helps you with performance tuning and troubleshooting.</span></span> 

![Výkon](./media/sql-database-performance/performance.png)

* <span data-ttu-id="5fe54-115">Hello **doporučení** dlaždice obsahuje rozpis systémů ladění doporučení pro vaši databázi (první tři doporučení jsou uvedené. Pokud existují další).</span><span class="sxs-lookup"><span data-stu-id="5fe54-115">hello **Recommendations** tile provides a breakdown of tuning recommendations for your database (top three recommendations are shown if there are more).</span></span> <span data-ttu-id="5fe54-116">Kliknutím na tuto dlaždici přejdete příliš**[výkonu doporučení](#performance-recommendations)**.</span><span class="sxs-lookup"><span data-stu-id="5fe54-116">Clicking this tile takes you too**[Performance recommendations](#performance-recommendations)**.</span></span> 
* <span data-ttu-id="5fe54-117">Hello **ladění aktivity** dlaždice poskytuje souhrn hello probíhající a dokončené ladění akce pro vaši databázi, která poskytuje rychlý přehled do historie hello ladění aktivity.</span><span class="sxs-lookup"><span data-stu-id="5fe54-117">hello **Tuning activity** tile provides a summary of hello ongoing and completed tuning actions for your database, giving you a quick view into hello history of tuning activity.</span></span> <span data-ttu-id="5fe54-118">Kliknutím na tuto dlaždici přejdete toohello úplná optimalizací zobrazení historie pro vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="5fe54-118">Clicking this tile takes you toohello full tuning history view for your database.</span></span>
* <span data-ttu-id="5fe54-119">Hello **automatické ladění** dlaždice ukazuje hello [automatické ladění konfigurace](sql-database-automatic-tuning-enable.md) pro vaši databázi (optimalizace pro možnosti, které jsou automaticky použité tooyour databáze).</span><span class="sxs-lookup"><span data-stu-id="5fe54-119">hello **Auto-tuning** tile shows hello [auto-tuning configuration](sql-database-automatic-tuning-enable.md) for your database (tuning options that are automatically applied tooyour database).</span></span> <span data-ttu-id="5fe54-120">Kliknutím na tuto dlaždici, otevře se dialogové okno Konfigurace automatizace hello.</span><span class="sxs-lookup"><span data-stu-id="5fe54-120">Clicking this tile opens hello automation configuration dialog.</span></span>
* <span data-ttu-id="5fe54-121">Hello **dotazy na databázi** dlaždice zobrazuje hello souhrn hello výkon dotazů pro databázi (celkový počet jednotek DTU využití a horní na prostředky dotazy).</span><span class="sxs-lookup"><span data-stu-id="5fe54-121">hello **Database queries** tile shows hello summary of hello query performance for your database (overall DTU usage and top resource consuming queries).</span></span> <span data-ttu-id="5fe54-122">Kliknutím na tuto dlaždici přejdete příliš**[Query Performance Insight](#query-performance-insight)**.</span><span class="sxs-lookup"><span data-stu-id="5fe54-122">Clicking this tile takes you too**[Query Performance Insight](#query-performance-insight)**.</span></span>

## <a name="performance-recommendations"></a><span data-ttu-id="5fe54-123">Doporučení výkonu</span><span class="sxs-lookup"><span data-stu-id="5fe54-123">Performance recommendations</span></span>
<span data-ttu-id="5fe54-124">Tato stránka obsahuje inteligentního [ladění doporučení](sql-database-advisor.md) , může zlepšit výkon vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="5fe54-124">This page provides intelligent [tuning recommendations](sql-database-advisor.md) that can improve your database's performance.</span></span> <span data-ttu-id="5fe54-125">na této stránce se zobrazují následující typy doporučení Hello:</span><span class="sxs-lookup"><span data-stu-id="5fe54-125">hello following types of recommendations are shown on this page:</span></span>

* <span data-ttu-id="5fe54-126">Doporučení, na které toocreate indexy nebo drop.</span><span class="sxs-lookup"><span data-stu-id="5fe54-126">Recommendations on which indexes toocreate or drop.</span></span>
* <span data-ttu-id="5fe54-127">Doporučení, když jsou v databázi hello zjištěny problémy schématu.</span><span class="sxs-lookup"><span data-stu-id="5fe54-127">Recommendations when schema issues are identified in hello database.</span></span>
* <span data-ttu-id="5fe54-128">Doporučení pro dotazy využívat parametrizované dotazy.</span><span class="sxs-lookup"><span data-stu-id="5fe54-128">Recommendations when queries can benefit from parameterized queries.</span></span>

![Výkon](./media/sql-database-performance/recommendations.png)

<span data-ttu-id="5fe54-130">Můžete také získat úplnou historii ladění akce, které byly použity v posledních hello.</span><span class="sxs-lookup"><span data-stu-id="5fe54-130">You can also find complete history of tuning actions that were applied in hello past.</span></span>

<span data-ttu-id="5fe54-131">Zjistěte, jak toofind operátoru apply výkonu doporučení v [najít a použít výkonu doporučení](sql-database-advisor-portal.md) článku.</span><span class="sxs-lookup"><span data-stu-id="5fe54-131">Learn how toofind an apply performance recommendations in [Find and apply performance recommendations](sql-database-advisor-portal.md) article.</span></span>

## <a name="automatic-tuning"></a><span data-ttu-id="5fe54-132">Automatické ladění</span><span class="sxs-lookup"><span data-stu-id="5fe54-132">Automatic tuning</span></span>
<span data-ttu-id="5fe54-133">Databáze Azure SQL můžete vyladit výkon databáze automaticky použitím [výkonu doporučení](sql-database-advisor.md).</span><span class="sxs-lookup"><span data-stu-id="5fe54-133">Azure SQL Databases can automatically tune database performance by applying [performance recommendations](sql-database-advisor.md).</span></span> <span data-ttu-id="5fe54-134">toolearn víc, přečtěte si [automatické ladění článku](sql-database-automatic-tuning.md).</span><span class="sxs-lookup"><span data-stu-id="5fe54-134">toolearn more, read [Automatic tuning article](sql-database-automatic-tuning.md).</span></span> <span data-ttu-id="5fe54-135">tooenable, přečtěte si [jak tooenable automatické ladění](sql-database-automatic-tuning-enable.md).</span><span class="sxs-lookup"><span data-stu-id="5fe54-135">tooenable it, read [how tooenable automatic tuning](sql-database-automatic-tuning-enable.md).</span></span>

## <a name="query-performance-insight"></a><span data-ttu-id="5fe54-136">Query Performance Insight</span><span class="sxs-lookup"><span data-stu-id="5fe54-136">Query Performance Insight</span></span>
<span data-ttu-id="5fe54-137">[Dotaz na informace o výkonu](sql-database-query-performance.md) vám umožní toospend méně času tím, že poskytuje řešení potíží s výkonem databáze:</span><span class="sxs-lookup"><span data-stu-id="5fe54-137">[Query Performance Insight](sql-database-query-performance.md) allows you toospend less time troubleshooting database performance by providing:</span></span>

* <span data-ttu-id="5fe54-138">Podrobnější přehled o vaší spotřeby prostředků (DTU) databází.</span><span class="sxs-lookup"><span data-stu-id="5fe54-138">Deeper insight into your databases resource (DTU) consumption.</span></span> 
* <span data-ttu-id="5fe54-139">Hello nejvíce využívají procesor dotazů, které lze ladit potenciálně pro zlepšení výkonu.</span><span class="sxs-lookup"><span data-stu-id="5fe54-139">hello top CPU consuming queries, which can potentially be tuned for improved performance.</span></span> 
* <span data-ttu-id="5fe54-140">Hello možnost toodrill dolů do hello podrobnosti dotazu.</span><span class="sxs-lookup"><span data-stu-id="5fe54-140">hello ability toodrill down into hello details of a query.</span></span> 

  ![řídicí panel výkonu](./media/sql-database-query-performance/performance.png)

<span data-ttu-id="5fe54-142">Další informace o této stránce naleznete v článku hello  **[jak toouse Query Performance Insight](sql-database-query-performance.md)**.</span><span class="sxs-lookup"><span data-stu-id="5fe54-142">Find more information about this page in hello article **[How toouse Query Performance Insight](sql-database-query-performance.md)**.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5fe54-143">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5fe54-143">Additional resources</span></span>
* [<span data-ttu-id="5fe54-144">Azure SQL Database – Průvodce výkonem pro izolované databáze</span><span class="sxs-lookup"><span data-stu-id="5fe54-144">Azure SQL Database performance guidance for single databases</span></span>](sql-database-performance-guidance.md)
* [<span data-ttu-id="5fe54-145">Pokud má být použita fondu elastické databáze?</span><span class="sxs-lookup"><span data-stu-id="5fe54-145">When should an elastic pool be used?</span></span>](sql-database-elastic-pool-guidance.md)

