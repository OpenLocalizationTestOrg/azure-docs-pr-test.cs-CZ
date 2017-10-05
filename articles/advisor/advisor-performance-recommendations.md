---
title: "Azure doporučení služby Advisor výkonu | Microsoft Docs"
description: "Použití Advisor k optimalizaci výkonu vašich Azure nasazení."
services: advisor
documentationcenter: NA
author: kumudd
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 5fb86c60b2d1f258dde5636ff8854b6f30f7f1c8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="advisor-performance-recommendations"></a><span data-ttu-id="3604a-103">Poradce při hodnocení výkonu doporučení</span><span class="sxs-lookup"><span data-stu-id="3604a-103">Advisor Performance recommendations</span></span>

<span data-ttu-id="3604a-104">Azure doporučení výkonu služby Advisor napomáhají, a zvýšit rychlost a reakce důležitých podnikových aplikací.</span><span class="sxs-lookup"><span data-stu-id="3604a-104">Azure Advisor performance recommendations help improve the speed and responsiveness of your business-critical applications.</span></span> <span data-ttu-id="3604a-105">Výkon doporučení služby Advisor můžete získat **výkonu** Advisor řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="3604a-105">You can get performance recommendations from Advisor on the **Performance** tab of the Advisor dashboard.</span></span>

![Karta výkonu Advisor](./media/advisor-performance-recommendations/advisor-performance-tab.png)

## <a name="improve-database-performance-with-sql-db-advisor"></a><span data-ttu-id="3604a-107">Zlepšení výkonu databáze službou SQL DB Advisor</span><span class="sxs-lookup"><span data-stu-id="3604a-107">Improve database performance with SQL DB Advisor</span></span>

<span data-ttu-id="3604a-108">Advisor vám poskytne konzistentní, konsolidované zobrazení doporučení pro všechny prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="3604a-108">Advisor provides you with a consistent, consolidated view of recommendations for all your Azure resources.</span></span> <span data-ttu-id="3604a-109">Integruje se službou Advisor databáze SQL, aby vám doporučení pro zlepšení výkonu databáze SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="3604a-109">It integrates with SQL Database Advisor to bring you recommendations for improving the performance of your SQL Azure database.</span></span> <span data-ttu-id="3604a-110">Poradce pro funkci SQL Database vyhodnocuje analýzou historii využití výkon vaší databáze SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="3604a-110">SQL Database Advisor assesses the performance of your SQL Azure databases by analyzing your usage history.</span></span> <span data-ttu-id="3604a-111">Potom nabízí doporučení, která jsou nejvhodnější pro spuštění typické zatížení databáze.</span><span class="sxs-lookup"><span data-stu-id="3604a-111">It then offers recommendations that are best suited for running the database’s typical workload.</span></span> 

> [!NOTE]
> <span data-ttu-id="3604a-112">Doporučení získáte databáze musí obsahovat o týden využití, a v daném týdnu musí být některé konzistentní aktivity.</span><span class="sxs-lookup"><span data-stu-id="3604a-112">To get recommendations, a database must have about a week of usage, and within that week there must be some consistent activity.</span></span> <span data-ttu-id="3604a-113">Poradce pro databáze SQL můžete optimalizovat snadněji konzistentní dotazu v případě vzorů než pro náhodné shluky aktivity.</span><span class="sxs-lookup"><span data-stu-id="3604a-113">SQL Database Advisor can optimize more easily for consistent query patterns than for random bursts of activity.</span></span>

<span data-ttu-id="3604a-114">Další informace o službě Advisor databáze SQL najdete v tématu [Poradce pro funkci SQL Database](https://azure.microsoft.com/en-us/documentation/articles/sql-database-advisor/).</span><span class="sxs-lookup"><span data-stu-id="3604a-114">For more information about SQL Database Advisor, see [SQL Database Advisor](https://azure.microsoft.com/en-us/documentation/articles/sql-database-advisor/).</span></span>

![Doporučení k databázi SQL](./media/advisor-performance-recommendations/advisor-performance-sql.png)

## <a name="improve-redis-cache-performance-and-reliability"></a><span data-ttu-id="3604a-116">Zlepšení výkonu Redis Cache a spolehlivosti</span><span class="sxs-lookup"><span data-stu-id="3604a-116">Improve Redis Cache performance and reliability</span></span>

<span data-ttu-id="3604a-117">Advisor identifikuje instance služby Redis Cache kde výkon může být nepříznivě ovlivněn velké množství paměti, zatížení serveru, šířky pásma sítě nebo velký počet připojení klientů.</span><span class="sxs-lookup"><span data-stu-id="3604a-117">Advisor identifies Redis Cache instances where performance may be adversely affected by high memory usage, server load, network bandwidth, or a large number of client connections.</span></span> <span data-ttu-id="3604a-118">Osvědčené postupy Advisor také poskytuje doporučení, která umožňuje vyhnout se možným problémům.</span><span class="sxs-lookup"><span data-stu-id="3604a-118">Advisor also provides best practices recommendations to help you avoid potential issues.</span></span> <span data-ttu-id="3604a-119">Další informace o doporučení Redis Cache najdete v tématu [Redis Cache Advisor](https://azure.microsoft.com/en-us/documentation/articles/cache-configure/#redis-cache-advisor).</span><span class="sxs-lookup"><span data-stu-id="3604a-119">For more information about Redis Cache recommendations, see [Redis Cache Advisor](https://azure.microsoft.com/en-us/documentation/articles/cache-configure/#redis-cache-advisor).</span></span>


## <a name="improve-app-service-performance-and-reliability"></a><span data-ttu-id="3604a-120">Zlepšení výkonu služby App Service a spolehlivosti</span><span class="sxs-lookup"><span data-stu-id="3604a-120">Improve App Service performance and reliability</span></span>

<span data-ttu-id="3604a-121">Azure Advisor integruje doporučení pro zlepšení prostředí aplikační služby a zjišťování možnosti příslušné platformy.</span><span class="sxs-lookup"><span data-stu-id="3604a-121">Azure Advisor integrates best practices recommendations for improving your App Services experience and discovering relevant platform capabilities.</span></span> <span data-ttu-id="3604a-122">Příklady App Services doporučení:</span><span class="sxs-lookup"><span data-stu-id="3604a-122">Examples of App Services recommendations are:</span></span>
* <span data-ttu-id="3604a-123">Zjišťování instancí, kde se pomocí aplikace moduly runtime s možnostmi zmírnění vyčerpání paměti nebo prostředků procesoru.</span><span class="sxs-lookup"><span data-stu-id="3604a-123">Detection of instances where memory or CPU resources are exhausted by app runtimes with mitigation options.</span></span>
* <span data-ttu-id="3604a-124">Zjišťování instancí, které tyto prostředky jako webové aplikace a databáze může zvýšit výkon a nižší náklady.</span><span class="sxs-lookup"><span data-stu-id="3604a-124">Detection of instances where collocating resources like web apps and databases can improve performance and lower cost.</span></span> 

<span data-ttu-id="3604a-125">Další informace o App Services doporučení najdete v tématu [osvědčené postupy pro službu Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/app-service-best-practices/).</span><span class="sxs-lookup"><span data-stu-id="3604a-125">For more information about App Services recommendations, see [Best Practices for Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/app-service-best-practices/).</span></span>
<span data-ttu-id="3604a-126">![Doporučení služby aplikace](./media/advisor-performance-recommendations/advisor-performance-app-service.png)</span><span class="sxs-lookup"><span data-stu-id="3604a-126">![App Services recommendations](./media/advisor-performance-recommendations/advisor-performance-app-service.png)</span></span>

## <a name="how-to-access-performance-recommendations-in-advisor"></a><span data-ttu-id="3604a-127">Jak získat přístup k výkonu doporučení v Advisor</span><span class="sxs-lookup"><span data-stu-id="3604a-127">How to access Performance recommendations in Advisor</span></span>

1. <span data-ttu-id="3604a-128">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3604a-128">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="3604a-129">V levém podokně klikněte na **další služby**.</span><span class="sxs-lookup"><span data-stu-id="3604a-129">In the left pane, click **More services**.</span></span>

3. <span data-ttu-id="3604a-130">V podokně nabídky služby v rámci **monitorování a správu**, klikněte na tlačítko **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="3604a-130">In the service menu pane, under **Monitoring and Management**, click **Azure Advisor**.</span></span>  
 <span data-ttu-id="3604a-131">Se zobrazí řídicí panel služby Advisor.</span><span class="sxs-lookup"><span data-stu-id="3604a-131">The Advisor dashboard is displayed.</span></span>

4. <span data-ttu-id="3604a-132">Na řídicím panelu služby Advisor, klikněte na **výkonu** kartě.</span><span class="sxs-lookup"><span data-stu-id="3604a-132">On the Advisor dashboard, click the **Performance** tab.</span></span>

5. <span data-ttu-id="3604a-133">Vyberte předplatné, pro který chcete dostávat doporučení a potom klikněte na **získat doporučení**.</span><span class="sxs-lookup"><span data-stu-id="3604a-133">Select the subscription for which you want to receive recommendations, and then click **Get recommendations**.</span></span>

> [!NOTE]
> <span data-ttu-id="3604a-134">Chcete-li získat přístup k doporučení služby Advisor, je nutné nejprve *zaregistrovat předplatné* službou Advisor.</span><span class="sxs-lookup"><span data-stu-id="3604a-134">To access Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="3604a-135">Předplatné je zaregistrován při *předplatné vlastníka* spustí Advisor řídicího panelu a klikne na tlačítko **získat doporučení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3604a-135">A subscription is registered when a *subscription Owner* launches the Advisor dashboard and clicks the **Get recommendations** button.</span></span> <span data-ttu-id="3604a-136">Toto je *jednorázovou operaci*.</span><span class="sxs-lookup"><span data-stu-id="3604a-136">This is a *one-time operation*.</span></span> <span data-ttu-id="3604a-137">Po registraci předplatného dostanete doporučení služby Advisor jako *vlastníka*, *Přispěvatel*, nebo *čtečky* pro předplatné, skupinu prostředků nebo konkrétní prostředek.</span><span class="sxs-lookup"><span data-stu-id="3604a-137">After the subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3604a-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3604a-138">Next steps</span></span>

<span data-ttu-id="3604a-139">Další informace o doporučení služby Advisor najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="3604a-139">To learn more about Advisor recommendations, see:</span></span>

* [<span data-ttu-id="3604a-140">Úvod do služby Advisor</span><span class="sxs-lookup"><span data-stu-id="3604a-140">Introduction to Advisor</span></span>](advisor-overview.md)
* [<span data-ttu-id="3604a-141">Začínáme se službou Advisor</span><span class="sxs-lookup"><span data-stu-id="3604a-141">Get started with Advisor</span></span>](advisor-get-started.md)
* [<span data-ttu-id="3604a-142">Náklady na doporučení služby Advisor</span><span class="sxs-lookup"><span data-stu-id="3604a-142">Advisor Cost recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="3604a-143">Doporučení pro vysokou dostupnost služby Advisor</span><span class="sxs-lookup"><span data-stu-id="3604a-143">Advisor High Availability recommendations</span></span>](advisor-high-availability-recommendations.md)
* [<span data-ttu-id="3604a-144">Doporučení zabezpečení Advisor</span><span class="sxs-lookup"><span data-stu-id="3604a-144">Advisor Security recommendations</span></span>](advisor-security-recommendations.md)

