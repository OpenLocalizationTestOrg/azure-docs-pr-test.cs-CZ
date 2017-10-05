---
title: "Monitorování a optimalizace výkonu – Azure SQL Database | Microsoft Docs"
description: "Tipy k ladění ve službě Azure SQL Database pomocí vyhodnocení a zlepšování výkonu."
services: sql-database
documentationcenter: 
author: v-shysun
manager: felixwu
editor: 
keywords: "ladění, ladění, ladění tipy, výkonu sql databáze výkonu výkonu SQL optimalizace výkonu databáze sql"
ms.assetid: eb7b3f66-3b33-4e1b-84fb-424a928a6672
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: v-shysun
ms.openlocfilehash: 7a2f1199a56e0bd32eafef9f420879c756673e7f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="monitoring-and-performance-tuning"></a><span data-ttu-id="ab069-104">Monitorování a optimalizace výkonu</span><span class="sxs-lookup"><span data-stu-id="ab069-104">Monitoring and performance tuning</span></span>

<span data-ttu-id="ab069-105">Databáze SQL Azure je automaticky prováděna a flexibilní datová služba, kde můžete snadno sledovat využití, přidat nebo odebrat prostředky (procesor, paměť, vstupně-výstupní operace), najít doporučení, které může zlepšit výkon vaší databáze, nebo nechat databáze přizpůsobit pro vaši úlohu a automaticky optimalizace výkonu.</span><span class="sxs-lookup"><span data-stu-id="ab069-105">Azure SQL Database is automatically managed and flexible data service where you can easily monitor usage, add or remove resources (CPU, memory, io), find recommendations that can improve performance of your database, or let database adapt to your workload and automatically optimize performance.</span></span>

<span data-ttu-id="ab069-106">Tento článek obsahuje přehled monitorování a možností, které jsou k dispozici ve službě Azure SQL Database ladění výkonu.</span><span class="sxs-lookup"><span data-stu-id="ab069-106">This article provides overview of monitoring and performance tuning options that are available in Azure SQL Database.</span></span>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="monitoring-and-troubleshooting-database-performance"></a><span data-ttu-id="ab069-107">Monitorování a řešení potíží s výkonem databáze</span><span class="sxs-lookup"><span data-stu-id="ab069-107">Monitoring and troubleshooting database performance</span></span>

<span data-ttu-id="ab069-108">Databáze SQL Azure umožňuje snadno monitorovat využití vaší databáze a identifikovat dotazy, které by mohly způsobit problémy s výkonem.</span><span class="sxs-lookup"><span data-stu-id="ab069-108">Azure SQL Database enables you to easily monitor your database usage and identify queries that might cause the performance issues.</span></span> <span data-ttu-id="ab069-109">Můžete sledovat výkon databáze pomocí Azure zobrazení portálu nebo systému.</span><span class="sxs-lookup"><span data-stu-id="ab069-109">You can monitor database performance using Azure portal or system views.</span></span> <span data-ttu-id="ab069-110">Máte následující možnosti pro monitorování a řešení potíží s výkonem databáze:</span><span class="sxs-lookup"><span data-stu-id="ab069-110">You have the following options for monitoring and troubleshooting database performance:</span></span>

1. <span data-ttu-id="ab069-111">V [portál Azure](https://portal.azure.com), klikněte na tlačítko **databází SQL**, vyberte databázi a pak vyhledejte prostředky blíží jejich maximální pomocí graf sledování.</span><span class="sxs-lookup"><span data-stu-id="ab069-111">In the [Azure portal](https://portal.azure.com), click **SQL databases**, select the database, and then use the Monitoring chart to look for resources approaching their maximum.</span></span> <span data-ttu-id="ab069-112">Spotřeba DTU se zobrazí ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="ab069-112">DTU consumption is shown by default.</span></span> <span data-ttu-id="ab069-113">Klikněte na tlačítko **upravit** změnit časový rozsah a hodnoty zobrazené.</span><span class="sxs-lookup"><span data-stu-id="ab069-113">Click **Edit** to change the time range and values shown.</span></span>
2. <span data-ttu-id="ab069-114">Použití [Query Performance Insight](sql-database-query-performance.md) k identifikaci dotazy, které potřebují nejvíc prostředků.</span><span class="sxs-lookup"><span data-stu-id="ab069-114">Use [Query Performance Insight](sql-database-query-performance.md) to identify the queries that spend the most of resources.</span></span>
3. <span data-ttu-id="ab069-115">Můžete použít zobrazení dynamické správy (zobrazení dynamické správy), rozšířených událostí (`XEvents`) a úložiště dotazů v aplikaci SSMS se získat parametry výkonu v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="ab069-115">You can use dynamic management views (DMVs), Extended Events (`XEvents`), and the Query Store in SSMS to get performance parameters in real time.</span></span>

<span data-ttu-id="ab069-116">Najdete v článku [výkonu pokyny tématu](sql-database-performance-guidance.md) najít technik, které můžete použít ke zlepšení výkonu databáze SQL Azure, je-li identifikovat některé problém pomocí těchto sestav a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="ab069-116">See the [performance guidance topic](sql-database-performance-guidance.md) to find techniques that you can use to improve performance of Azure SQL Database if you identify some issue using these reports or views.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="ab069-117">Doporučujeme vám vždy používat nejnovější verzi aplikace Management Studio, aby se zajistila synchronizovanost s aktualizacemi Microsoft Azure a SQL Database.</span><span class="sxs-lookup"><span data-stu-id="ab069-117">It is recommended that you always use the latest version of Management Studio to remain synchronized with updates to Microsoft Azure and SQL Database.</span></span> <span data-ttu-id="ab069-118">[Aktualizovat aplikaci SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="ab069-118">[Update SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).</span></span>
>

## <a name="optimize-database-to-improve-performance"></a><span data-ttu-id="ab069-119">Optimalizace databáze za účelem zvýšení výkonu</span><span class="sxs-lookup"><span data-stu-id="ab069-119">Optimize database to improve performance</span></span>

<span data-ttu-id="ab069-120">Databáze SQL Azure umožňuje identifikovat příležitosti pro zlepšení a optimalizovat výkon dotazů beze změny prostředky kontrolou [doporučení ladění výkonu](sql-database-advisor.md).</span><span class="sxs-lookup"><span data-stu-id="ab069-120">Azure SQL Database enables you to identify opportunities to improve and optimize query performance without changing resources by reviewing [performance tuning recommendations](sql-database-advisor.md).</span></span> <span data-ttu-id="ab069-121">Častým důvodem toho, že databáze je pomalá, jsou chybějící indexy a nedostatečně optimalizované dotazy.</span><span class="sxs-lookup"><span data-stu-id="ab069-121">Missing indexes and poorly optimized queries are common reasons for poor database performance.</span></span> <span data-ttu-id="ab069-122">Můžete použít tyto vyladění doporučení pro zlepšení výkonu vašich úloh.</span><span class="sxs-lookup"><span data-stu-id="ab069-122">You can apply these tuning recommendations to improve performance of your workload.</span></span>
<span data-ttu-id="ab069-123">Můžete také umožňují Azure SQL database k [automaticky optimalizovat výkon vašich dotazů](sql-database-automatic-tuning.md) použitím všechny identifikovat doporučení a ověřování, že se zlepšit výkon databáze.</span><span class="sxs-lookup"><span data-stu-id="ab069-123">You can also let Azure SQL database to [automatically optimize performance of your queries](sql-database-automatic-tuning.md) by applying all identified recommendations and verifying that they improve database performance.</span></span> <span data-ttu-id="ab069-124">Pro zlepšení výkonu databáze můžete použít následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="ab069-124">You can use the following options to improve performance of your database:</span></span>

1. <span data-ttu-id="ab069-125">Použití [Poradce pro funkci SQL Database](sql-database-advisor-portal.md) zobrazíte doporučení k vytváření a rušení indexů, parametrické dotazy a opravit problémy schématu.</span><span class="sxs-lookup"><span data-stu-id="ab069-125">Use [SQL Database Advisor](sql-database-advisor-portal.md) to view recommendations for creating and dropping indexes, parameterizing queries, and fixing schema issues.</span></span>
2. <span data-ttu-id="ab069-126">[Povolit automatické ladění](sql-database-automatic-tuning-enable.md) a nechat Azure SQL databáze automaticky oprava identifikovat problémy s výkonem.</span><span class="sxs-lookup"><span data-stu-id="ab069-126">[Enable automatic tuning](sql-database-automatic-tuning-enable.md) and let Azure SQL database automatically fix identified performance issues.</span></span>

## <a name="improving-database-performance-with-more-resources"></a><span data-ttu-id="ab069-127">Zvýšení výkonu databáze s další prostředky</span><span class="sxs-lookup"><span data-stu-id="ab069-127">Improving database performance with more resources</span></span>

<span data-ttu-id="ab069-128">Nakonec pokud nejsou žádné řešitelné položky, které může zlepšit výkon vaší databáze, můžete změnit objem prostředků, které jsou k dispozici ve službě Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="ab069-128">Finally, if there are no actionable items that can improve performance of your database, you can change the amount of resources available in Azure SQL Database.</span></span> <span data-ttu-id="ab069-129">Další zdroje informací můžete přiřadit změnou [vrstvy služby](sql-database-service-tiers.md) samostatná databáze nebo zvýšení jednotky Edtu fondu elastické databáze kdykoli.</span><span class="sxs-lookup"><span data-stu-id="ab069-129">You can assign more resources by changing the [service tier](sql-database-service-tiers.md) of a standalone database or increase the eDTUs of an elastic pool at any time.</span></span>
1. <span data-ttu-id="ab069-130">Pro samostatné databáze, můžete [Změna úrovně služeb](sql-database-service-tiers.md) na vyžádání ke zlepšení výkonu databáze.</span><span class="sxs-lookup"><span data-stu-id="ab069-130">For standalone databases, you can [change service tiers](sql-database-service-tiers.md) on-demand to improve database performance.</span></span>
2. <span data-ttu-id="ab069-131">V případě více databází, zvažte použití [elastické fondy](sql-database-elastic-pool-guidance.md) prostředky automaticky škálovat.</span><span class="sxs-lookup"><span data-stu-id="ab069-131">For multiple databases, consider using [elastic pools](sql-database-elastic-pool-guidance.md) to scale resources automatically.</span></span>

## <a name="tune-and-refactor-application-or-database-code"></a><span data-ttu-id="ab069-132">Ladění a refactor aplikace nebo kódu databáze</span><span class="sxs-lookup"><span data-stu-id="ab069-132">Tune and refactor application or database code</span></span>

<span data-ttu-id="ab069-133">Kód aplikace více optimálně použít databázi, změňte indexy, vynutit plány nebo ručně přizpůsobit databázi do úlohy pomocí odkazů na servery, můžete změnit.</span><span class="sxs-lookup"><span data-stu-id="ab069-133">You can change application code to more optimally use the database, change indexes, force plans, or use hints to manually adapt the database to your workload.</span></span> <span data-ttu-id="ab069-134">Najít některé pokyny a tipy pro ruční ladění a přepisování kód [výkonu pokyny tématu](sql-database-performance-guidance.md) článku.</span><span class="sxs-lookup"><span data-stu-id="ab069-134">Find some guidance and tips for manual tuning and rewriting the code in the [performance guidance topic](sql-database-performance-guidance.md) article.</span></span>


## <a name="next-steps"></a><span data-ttu-id="ab069-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ab069-135">Next steps</span></span>

- <span data-ttu-id="ab069-136">Povolit automatické ladění ve službě Azure SQL Database a nechat automatické ladění funkce plně spravovat vaše úlohy najdete v tématu [povolit automatické ladění](sql-database-automatic-tuning-enable.md).</span><span class="sxs-lookup"><span data-stu-id="ab069-136">To enable automatic tuning in Azure SQL Database and let automatic tuning feature fully manage your workload, see [Enable automatic tuning](sql-database-automatic-tuning-enable.md).</span></span>
- <span data-ttu-id="ab069-137">Chcete-li použít ruční ladění, můžete zjistit [ladění doporučení na portálu Azure](sql-database-advisor-portal.md) a ty, které zlepšit výkon vašich dotazů použít ručně.</span><span class="sxs-lookup"><span data-stu-id="ab069-137">To use manual tuning, you can review [Tuning recommendations in Azure portal](sql-database-advisor-portal.md) and manually apply the ones that improve performance of your queries.</span></span>
- <span data-ttu-id="ab069-138">Změnit prostředky, které jsou k dispozici ve vaší databázi změnou [úrovních služby databáze SQL Azure](sql-database-performance-guidance.md)</span><span class="sxs-lookup"><span data-stu-id="ab069-138">Change resources that are available in your database by changing [Azure SQL Database service tiers](sql-database-performance-guidance.md)</span></span>