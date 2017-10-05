---
title: "Azure doporučení služby Advisor náklady | Microsoft Docs"
description: "Pomocí Azure Advisor optimalizovat náklady na vašich Azure nasazení."
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
ms.openlocfilehash: 5eef2116f238b477fa8de46ce7b25728c393739c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="advisor-cost-recommendations"></a><span data-ttu-id="f7dbc-103">Náklady na doporučení služby Advisor</span><span class="sxs-lookup"><span data-stu-id="f7dbc-103">Advisor Cost recommendations</span></span>

<span data-ttu-id="f7dbc-104">Pomáhá Advisor optimalizovat a snížit vaše celkové Azure tráví určením nečinnosti a nedostatečně prostředky.</span><span class="sxs-lookup"><span data-stu-id="f7dbc-104">Advisor helps you optimize and reduce your overall Azure spend by identifying idle and underutilized resources.</span></span> <span data-ttu-id="f7dbc-105">Můžete získat náklady doporučení z **náklady** na řídicím panelu služby Advisor.</span><span class="sxs-lookup"><span data-stu-id="f7dbc-105">You can get cost recommendations from the **Cost** tab on the Advisor dashboard.</span></span>

![Karta náklady na Advisor](./media/advisor-cost-recommendations/advisor-cost-tab2.png)

## <a name="optimize-virtual-machine-spend-by-resizing-underutilized-instances"></a><span data-ttu-id="f7dbc-107">Optimalizovat virtuální počítač tráví změnou velikosti nedostatečně instancí</span><span class="sxs-lookup"><span data-stu-id="f7dbc-107">Optimize virtual machine spend by resizing underutilized instances</span></span> 
<span data-ttu-id="f7dbc-108">I když některé scénáře aplikací může být nízkou míru využívání návrhu, můžete často ušetřit peníze pomocí správy velikost a počet virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f7dbc-108">Although certain application scenarios can result in low utilization by design, you can often save money by managing the size and number of your virtual machines.</span></span> <span data-ttu-id="f7dbc-109">Advisor monitoruje vaše využití virtuálního počítače po dobu 14 dnů a pak identifikuje nízké využití virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f7dbc-109">Advisor monitors your virtual machine usage for 14 days and then identifies low-utilization virtual machines.</span></span> <span data-ttu-id="f7dbc-110">Virtuální počítače, jejichž využití procesoru je 5 % nebo méně a využití sítě je 7 MB nebo méně čtyři nebo více dní jsou považovány za nízké využití virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f7dbc-110">Virtual machines whose CPU utilization is 5 percent or less and network usage is 7 MB or less for four or more days are considered low-utilization virtual machines.</span></span>

<span data-ttu-id="f7dbc-111">Advisor zobrazí odhadované náklady pokračovat ke spuštění virtuálního počítače, takže je možné ho vypnout, nebo jeho velikost.</span><span class="sxs-lookup"><span data-stu-id="f7dbc-111">Advisor shows you the estimated cost of continuing to run your virtual machine, so that you can choose to shut it down or resize it.</span></span>  

![Advisor náklady doporučení pro změnu velikosti virtuálních počítačů](./media/advisor-cost-recommendations/advisor-cost-resizevms.png)

## <a name="use-a-cost-effective-solution-to-manage-performance-goals-of-multiple-sql-databases"></a><span data-ttu-id="f7dbc-113">Použít nákladově efektivní řešení ke správě výkonnostní cíle více databází SQL</span><span class="sxs-lookup"><span data-stu-id="f7dbc-113">Use a cost effective solution to manage performance goals of multiple SQL databases</span></span>
<span data-ttu-id="f7dbc-114">Advisor identifikuje instance serveru SQL, které můžete využít možnost vytvoření fondů elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="f7dbc-114">Advisor identifies SQL server instances that can benefit from creating elastic database pools.</span></span> <span data-ttu-id="f7dbc-115">Fondy elastické databáze poskytují jednoduché a nákladově efektivní řešení pro správu výkonu cílů více databází, které mají různou vzorce používání.</span><span class="sxs-lookup"><span data-stu-id="f7dbc-115">Elastic database pools provide a simple, cost-effective solution to manage the performance goals of multiple databases that have varying usage patterns.</span></span> <span data-ttu-id="f7dbc-116">Další informace o Azure elastické fondy najdete v tématu [co je Azure Elastickém fondu?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/).</span><span class="sxs-lookup"><span data-stu-id="f7dbc-116">For more information about Azure elastic pools, see [What is an Azure Elastic pool?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/).</span></span>

![Advisor náklady doporučení pro fondy elastické databáze](./media/advisor-cost-recommendations/advisor-cost-elasticdbpools.png)

## <a name="how-to-access-cost-recommendations-in-azure-advisor"></a><span data-ttu-id="f7dbc-118">Jak získat přístup k náklady na doporučení v Azure Advisor</span><span class="sxs-lookup"><span data-stu-id="f7dbc-118">How to access cost recommendations in Azure Advisor</span></span>

1. <span data-ttu-id="f7dbc-119">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f7dbc-119">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="f7dbc-120">V levém podokně klikněte na **další služby**.</span><span class="sxs-lookup"><span data-stu-id="f7dbc-120">In the left pane, click **More services**.</span></span>

3. <span data-ttu-id="f7dbc-121">V podokně nabídky služby v rámci **monitorování a správu**, klikněte na tlačítko **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="f7dbc-121">In the service menu pane, under **Monitoring and Management**, click **Azure Advisor**.</span></span>  
 <span data-ttu-id="f7dbc-122">Se zobrazí řídicí panel služby Advisor.</span><span class="sxs-lookup"><span data-stu-id="f7dbc-122">The Advisor dashboard is displayed.</span></span>

4. <span data-ttu-id="f7dbc-123">Na řídicím panelu služby Advisor, klikněte na **náklady** kartě.</span><span class="sxs-lookup"><span data-stu-id="f7dbc-123">On the Advisor dashboard, click the **Cost** tab.</span></span>

5. <span data-ttu-id="f7dbc-124">Vyberte předplatné, pro který chcete dostávat doporučení a potom klikněte na **získat doporučení**.</span><span class="sxs-lookup"><span data-stu-id="f7dbc-124">Select the subscription for which you want to receive recommendations, and then click **Get recommendations**.</span></span>

> [!NOTE]
> <span data-ttu-id="f7dbc-125">Chcete-li získat přístup k doporučení služby Advisor, je nutné nejprve *zaregistrovat předplatné* službou Advisor.</span><span class="sxs-lookup"><span data-stu-id="f7dbc-125">To access Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="f7dbc-126">Předplatné je zaregistrován při *předplatné vlastníka* spustí Advisor řídicího panelu a klikne na tlačítko **získat doporučení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f7dbc-126">A subscription is registered when a *subscription Owner* launches the Advisor dashboard and clicks the **Get recommendations** button.</span></span> <span data-ttu-id="f7dbc-127">Toto je *jednorázovou operaci*.</span><span class="sxs-lookup"><span data-stu-id="f7dbc-127">This is a *one-time operation*.</span></span> <span data-ttu-id="f7dbc-128">Po registraci předplatného dostanete doporučení služby Advisor jako *vlastníka*, *Přispěvatel*, nebo *čtečky* pro předplatné, skupinu prostředků nebo konkrétní prostředek.</span><span class="sxs-lookup"><span data-stu-id="f7dbc-128">After the subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f7dbc-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f7dbc-129">Next steps</span></span>

<span data-ttu-id="f7dbc-130">Další informace o doporučení služby Advisor najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="f7dbc-130">To learn more about Advisor recommendations, see:</span></span>
* [<span data-ttu-id="f7dbc-131">Úvod do služby Advisor</span><span class="sxs-lookup"><span data-stu-id="f7dbc-131">Introduction to Advisor</span></span>](advisor-overview.md)
* [<span data-ttu-id="f7dbc-132">Začínáme</span><span class="sxs-lookup"><span data-stu-id="f7dbc-132">Get Started</span></span>](advisor-get-started.md)
* [<span data-ttu-id="f7dbc-133">Poradce při hodnocení výkonu doporučení</span><span class="sxs-lookup"><span data-stu-id="f7dbc-133">Advisor Performance recommendations</span></span>](advisor-cost-recommendations.md)
* [<span data-ttu-id="f7dbc-134">Doporučení pro vysokou dostupnost služby Advisor</span><span class="sxs-lookup"><span data-stu-id="f7dbc-134">Advisor High Availability recommendations</span></span>](advisor-cost-recommendations.md)
* [<span data-ttu-id="f7dbc-135">Doporučení zabezpečení Advisor</span><span class="sxs-lookup"><span data-stu-id="f7dbc-135">Advisor Security recommendations</span></span>](advisor-cost-recommendations.md)
