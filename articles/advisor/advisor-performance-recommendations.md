---
title: "Azure doporučení služby Advisor výkonu | Microsoft Docs"
description: "Použití Advisor k optimalizaci výkonu vašich Azure nasazení."
services: advisor
documentationcenter: NA
author: KumudD
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
ms.openlocfilehash: e32723cd3ef13829890a630f4bff308164e17674
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="advisor-performance-recommendations"></a><span data-ttu-id="2c09f-103">Poradce při hodnocení výkonu doporučení</span><span class="sxs-lookup"><span data-stu-id="2c09f-103">Advisor Performance recommendations</span></span>

<span data-ttu-id="2c09f-104">Azure doporučení výkonu služby Advisor napomáhají, a zvýšit rychlost a reakce důležitých podnikových aplikací.</span><span class="sxs-lookup"><span data-stu-id="2c09f-104">Azure Advisor performance recommendations help improve the speed and responsiveness of your business-critical applications.</span></span> <span data-ttu-id="2c09f-105">Výkon doporučení služby Advisor můžete získat **výkonu** Advisor řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="2c09f-105">You can get performance recommendations from Advisor on the **Performance** tab of the Advisor dashboard.</span></span>

## <a name="improve-database-performance-with-sql-db-advisor"></a><span data-ttu-id="2c09f-106">Zlepšení výkonu databáze službou SQL DB Advisor</span><span class="sxs-lookup"><span data-stu-id="2c09f-106">Improve database performance with SQL DB Advisor</span></span>

<span data-ttu-id="2c09f-107">Advisor vám poskytne konzistentní, konsolidované zobrazení doporučení pro všechny prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="2c09f-107">Advisor provides you with a consistent, consolidated view of recommendations for all your Azure resources.</span></span> <span data-ttu-id="2c09f-108">Integruje se službou Advisor databáze SQL, aby vám doporučení pro zlepšení výkonu databáze SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="2c09f-108">It integrates with SQL Database Advisor to bring you recommendations for improving the performance of your SQL Azure database.</span></span> <span data-ttu-id="2c09f-109">Poradce pro funkci SQL Database vyhodnocuje analýzou historii využití výkon vaší databáze SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="2c09f-109">SQL Database Advisor assesses the performance of your SQL Azure databases by analyzing your usage history.</span></span> <span data-ttu-id="2c09f-110">Potom nabízí doporučení, která jsou nejvhodnější pro spuštění typické zatížení databáze.</span><span class="sxs-lookup"><span data-stu-id="2c09f-110">It then offers recommendations that are best suited for running the database’s typical workload.</span></span> 

> [!NOTE]
> <span data-ttu-id="2c09f-111">Doporučení získáte databáze musí obsahovat o týden využití, a v daném týdnu musí být některé konzistentní aktivity.</span><span class="sxs-lookup"><span data-stu-id="2c09f-111">To get recommendations, a database must have about a week of usage, and within that week there must be some consistent activity.</span></span> <span data-ttu-id="2c09f-112">Poradce pro databáze SQL můžete optimalizovat snadněji konzistentní dotazu v případě vzorů než pro náhodné shluky aktivity.</span><span class="sxs-lookup"><span data-stu-id="2c09f-112">SQL Database Advisor can optimize more easily for consistent query patterns than for random bursts of activity.</span></span>

<span data-ttu-id="2c09f-113">Další informace o službě Advisor databáze SQL najdete v tématu [Poradce pro funkci SQL Database](https://azure.microsoft.com/en-us/documentation/articles/sql-database-advisor/).</span><span class="sxs-lookup"><span data-stu-id="2c09f-113">For more information about SQL Database Advisor, see [SQL Database Advisor](https://azure.microsoft.com/en-us/documentation/articles/sql-database-advisor/).</span></span>

## <a name="improve-redis-cache-performance-and-reliability"></a><span data-ttu-id="2c09f-114">Zlepšení výkonu Redis Cache a spolehlivosti</span><span class="sxs-lookup"><span data-stu-id="2c09f-114">Improve Redis Cache performance and reliability</span></span>

<span data-ttu-id="2c09f-115">Advisor identifikuje instance služby Redis Cache kde výkon může být nepříznivě ovlivněn velké množství paměti, zatížení serveru, šířky pásma sítě nebo velký počet připojení klientů.</span><span class="sxs-lookup"><span data-stu-id="2c09f-115">Advisor identifies Redis Cache instances where performance may be adversely affected by high memory usage, server load, network bandwidth, or a large number of client connections.</span></span> <span data-ttu-id="2c09f-116">Osvědčené postupy Advisor také poskytuje doporučení, která umožňuje vyhnout se možným problémům.</span><span class="sxs-lookup"><span data-stu-id="2c09f-116">Advisor also provides best practices recommendations to help you avoid potential issues.</span></span> <span data-ttu-id="2c09f-117">Další informace o doporučení Redis Cache najdete v tématu [Redis Cache Advisor](https://azure.microsoft.com/en-us/documentation/articles/cache-configure/#redis-cache-advisor).</span><span class="sxs-lookup"><span data-stu-id="2c09f-117">For more information about Redis Cache recommendations, see [Redis Cache Advisor](https://azure.microsoft.com/en-us/documentation/articles/cache-configure/#redis-cache-advisor).</span></span>


## <a name="improve-app-service-performance-and-reliability"></a><span data-ttu-id="2c09f-118">Zlepšení výkonu služby App Service a spolehlivosti</span><span class="sxs-lookup"><span data-stu-id="2c09f-118">Improve App Service performance and reliability</span></span>

<span data-ttu-id="2c09f-119">Azure Advisor integruje doporučení pro zlepšení prostředí aplikační služby a zjišťování možnosti příslušné platformy.</span><span class="sxs-lookup"><span data-stu-id="2c09f-119">Azure Advisor integrates best practices recommendations for improving your App Services experience and discovering relevant platform capabilities.</span></span> <span data-ttu-id="2c09f-120">Příklady App Services doporučení:</span><span class="sxs-lookup"><span data-stu-id="2c09f-120">Examples of App Services recommendations are:</span></span>
* <span data-ttu-id="2c09f-121">Zjišťování instancí, kde se pomocí aplikace moduly runtime s možnostmi zmírnění vyčerpání paměti nebo prostředků procesoru.</span><span class="sxs-lookup"><span data-stu-id="2c09f-121">Detection of instances where memory or CPU resources are exhausted by app runtimes with mitigation options.</span></span>
* <span data-ttu-id="2c09f-122">Zjišťování instancí, které tyto prostředky jako webové aplikace a databáze může zvýšit výkon a nižší náklady.</span><span class="sxs-lookup"><span data-stu-id="2c09f-122">Detection of instances where collocating resources like web apps and databases can improve performance and lower cost.</span></span> 

<span data-ttu-id="2c09f-123">Další informace o App Services doporučení najdete v tématu [osvědčené postupy pro službu Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/app-service-best-practices/).</span><span class="sxs-lookup"><span data-stu-id="2c09f-123">For more information about App Services recommendations, see [Best Practices for Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/app-service-best-practices/).</span></span>

## <a name="how-to-access-performance-recommendations-in-advisor"></a><span data-ttu-id="2c09f-124">Jak získat přístup k výkonu doporučení v Advisor</span><span class="sxs-lookup"><span data-stu-id="2c09f-124">How to access Performance recommendations in Advisor</span></span>

1. <span data-ttu-id="2c09f-125">Přihlaste se k [portál Azure](https://portal.azure.com)a pak otevřete [Advisor](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="2c09f-125">Sign in to the [Azure portal](https://portal.azure.com), and then open [Advisor](https://aka.ms/azureadvisordashboard).</span></span>

2.  <span data-ttu-id="2c09f-126">Na řídicím panelu služby Advisor, klikněte na **výkonu** kartě.</span><span class="sxs-lookup"><span data-stu-id="2c09f-126">On the Advisor dashboard, click the **Performance** tab.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2c09f-127">Další postup</span><span class="sxs-lookup"><span data-stu-id="2c09f-127">Next steps</span></span>

<span data-ttu-id="2c09f-128">Další informace o doporučení služby Advisor najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="2c09f-128">To learn more about Advisor recommendations, see:</span></span>

* [<span data-ttu-id="2c09f-129">Úvod do služby Advisor</span><span class="sxs-lookup"><span data-stu-id="2c09f-129">Introduction to Advisor</span></span>](advisor-overview.md)
* [<span data-ttu-id="2c09f-130">Začínáme se službou Advisor</span><span class="sxs-lookup"><span data-stu-id="2c09f-130">Get started with Advisor</span></span>](advisor-get-started.md)
* [<span data-ttu-id="2c09f-131">Náklady na doporučení služby Advisor</span><span class="sxs-lookup"><span data-stu-id="2c09f-131">Advisor Cost recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="2c09f-132">Doporučení pro vysokou dostupnost služby Advisor</span><span class="sxs-lookup"><span data-stu-id="2c09f-132">Advisor High Availability recommendations</span></span>](advisor-high-availability-recommendations.md)
* [<span data-ttu-id="2c09f-133">Doporučení zabezpečení Advisor</span><span class="sxs-lookup"><span data-stu-id="2c09f-133">Advisor Security recommendations</span></span>](advisor-security-recommendations.md)

