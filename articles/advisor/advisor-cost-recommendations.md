---
title: "doporučení služby Advisor náklady aaaAzure | Microsoft Docs"
description: "Pomocí Azure Advisor toooptimize hello náklady na nasazení Azure."
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
ms.openlocfilehash: 50f70c33a17f550c8753795435cdfddd51e409f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="advisor-cost-recommendations"></a><span data-ttu-id="e0715-103">Náklady na doporučení služby Advisor</span><span class="sxs-lookup"><span data-stu-id="e0715-103">Advisor Cost recommendations</span></span>

<span data-ttu-id="e0715-104">Pomáhá Advisor optimalizovat a snížit vaše celkové Azure tráví určením nečinnosti a nedostatečně prostředky.</span><span class="sxs-lookup"><span data-stu-id="e0715-104">Advisor helps you optimize and reduce your overall Azure spend by identifying idle and underutilized resources.</span></span> <span data-ttu-id="e0715-105">Můžete získat náklady doporučení z hello **náklady** karty na řídicím panelu Advisor hello.</span><span class="sxs-lookup"><span data-stu-id="e0715-105">You can get cost recommendations from hello **Cost** tab on hello Advisor dashboard.</span></span>

![Karta náklady na Advisor](./media/advisor-cost-recommendations/advisor-cost-tab2.png)

## <a name="optimize-virtual-machine-spend-by-resizing-underutilized-instances"></a><span data-ttu-id="e0715-107">Optimalizovat virtuální počítač tráví změnou velikosti nedostatečně instancí</span><span class="sxs-lookup"><span data-stu-id="e0715-107">Optimize virtual machine spend by resizing underutilized instances</span></span> 
<span data-ttu-id="e0715-108">I když některé scénáře aplikací může být nízkou míru využívání návrhu, můžete často ušetřit peníze pomocí správy hello velikost a počet virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e0715-108">Although certain application scenarios can result in low utilization by design, you can often save money by managing hello size and number of your virtual machines.</span></span> <span data-ttu-id="e0715-109">Advisor monitoruje vaše využití virtuálního počítače po dobu 14 dnů a pak identifikuje nízké využití virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e0715-109">Advisor monitors your virtual machine usage for 14 days and then identifies low-utilization virtual machines.</span></span> <span data-ttu-id="e0715-110">Virtuální počítače, jejichž využití procesoru je 5 % nebo méně a využití sítě je 7 MB nebo méně čtyři nebo více dní jsou považovány za nízké využití virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e0715-110">Virtual machines whose CPU utilization is 5 percent or less and network usage is 7 MB or less for four or more days are considered low-utilization virtual machines.</span></span>

<span data-ttu-id="e0715-111">Zobrazuje Advisor hello odhadované náklady pokračováním toorun virtuálního počítače, takže můžete tooshut ho dolů nebo jeho velikost.</span><span class="sxs-lookup"><span data-stu-id="e0715-111">Advisor shows you hello estimated cost of continuing toorun your virtual machine, so that you can choose tooshut it down or resize it.</span></span>  

![Advisor náklady doporučení pro změnu velikosti virtuálních počítačů](./media/advisor-cost-recommendations/advisor-cost-resizevms.png)

## <a name="use-a-cost-effective-solution-toomanage-performance-goals-of-multiple-sql-databases"></a><span data-ttu-id="e0715-113">Použít výkonnostní cíle toomanage nákladově efektivní řešení více databází SQL</span><span class="sxs-lookup"><span data-stu-id="e0715-113">Use a cost effective solution toomanage performance goals of multiple SQL databases</span></span>
<span data-ttu-id="e0715-114">Advisor identifikuje instance serveru SQL, které můžete využít možnost vytvoření fondů elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="e0715-114">Advisor identifies SQL server instances that can benefit from creating elastic database pools.</span></span> <span data-ttu-id="e0715-115">Fondy elastické databáze poskytují jednoduché a nákladově efektivní řešení toomanage hello výkonu cílů více databází, které mají různou vzorce používání.</span><span class="sxs-lookup"><span data-stu-id="e0715-115">Elastic database pools provide a simple, cost-effective solution toomanage hello performance goals of multiple databases that have varying usage patterns.</span></span> <span data-ttu-id="e0715-116">Další informace o Azure elastické fondy najdete v tématu [co je Azure Elastickém fondu?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/).</span><span class="sxs-lookup"><span data-stu-id="e0715-116">For more information about Azure elastic pools, see [What is an Azure Elastic pool?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/).</span></span>

![Advisor náklady doporučení pro fondy elastické databáze](./media/advisor-cost-recommendations/advisor-cost-elasticdbpools.png)

## <a name="how-tooaccess-cost-recommendations-in-azure-advisor"></a><span data-ttu-id="e0715-118">Jak tooaccess náklady doporučení v Azure Advisor</span><span class="sxs-lookup"><span data-stu-id="e0715-118">How tooaccess cost recommendations in Azure Advisor</span></span>

1. <span data-ttu-id="e0715-119">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e0715-119">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="e0715-120">V levém podokně hello, klikněte na **další služby**.</span><span class="sxs-lookup"><span data-stu-id="e0715-120">In hello left pane, click **More services**.</span></span>

3. <span data-ttu-id="e0715-121">V hello služby nabídky podokně v části **monitorování a správu**, klikněte na tlačítko **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="e0715-121">In hello service menu pane, under **Monitoring and Management**, click **Azure Advisor**.</span></span>  
 <span data-ttu-id="e0715-122">se zobrazí řídicí panel Advisor Hello.</span><span class="sxs-lookup"><span data-stu-id="e0715-122">hello Advisor dashboard is displayed.</span></span>

4. <span data-ttu-id="e0715-123">Na řídicím panelu hello Advisor, klikněte na tlačítko hello **náklady** kartě.</span><span class="sxs-lookup"><span data-stu-id="e0715-123">On hello Advisor dashboard, click hello **Cost** tab.</span></span>

5. <span data-ttu-id="e0715-124">Vyberte hello předplatné, pro který chcete tooreceive doporučení a pak klikněte na tlačítko **získat doporučení**.</span><span class="sxs-lookup"><span data-stu-id="e0715-124">Select hello subscription for which you want tooreceive recommendations, and then click **Get recommendations**.</span></span>

> [!NOTE]
> <span data-ttu-id="e0715-125">tooaccess doporučení služby Advisor, musíte nejdřív *zaregistrovat předplatné* službou Advisor.</span><span class="sxs-lookup"><span data-stu-id="e0715-125">tooaccess Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="e0715-126">Předplatné je zaregistrován při *předplatné vlastníka* spustí hello Advisor řídicí panel a klikne na tlačítko hello **získat doporučení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e0715-126">A subscription is registered when a *subscription Owner* launches hello Advisor dashboard and clicks hello **Get recommendations** button.</span></span> <span data-ttu-id="e0715-127">Toto je *jednorázovou operaci*.</span><span class="sxs-lookup"><span data-stu-id="e0715-127">This is a *one-time operation*.</span></span> <span data-ttu-id="e0715-128">Po registraci předplatného hello dostanete doporučení služby Advisor jako *vlastníka*, *Přispěvatel*, nebo *čtečky* pro předplatné, skupinu prostředků nebo konkrétní prostředek.</span><span class="sxs-lookup"><span data-stu-id="e0715-128">After hello subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e0715-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e0715-129">Next steps</span></span>

<span data-ttu-id="e0715-130">toolearn Další informace o doporučení služby Advisor, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="e0715-130">toolearn more about Advisor recommendations, see:</span></span>
* [<span data-ttu-id="e0715-131">TooAdvisor Úvod</span><span class="sxs-lookup"><span data-stu-id="e0715-131">Introduction tooAdvisor</span></span>](advisor-overview.md)
* [<span data-ttu-id="e0715-132">Začínáme</span><span class="sxs-lookup"><span data-stu-id="e0715-132">Get Started</span></span>](advisor-get-started.md)
* [<span data-ttu-id="e0715-133">Poradce při hodnocení výkonu doporučení</span><span class="sxs-lookup"><span data-stu-id="e0715-133">Advisor Performance recommendations</span></span>](advisor-cost-recommendations.md)
* [<span data-ttu-id="e0715-134">Doporučení pro vysokou dostupnost služby Advisor</span><span class="sxs-lookup"><span data-stu-id="e0715-134">Advisor High Availability recommendations</span></span>](advisor-cost-recommendations.md)
* [<span data-ttu-id="e0715-135">Doporučení zabezpečení Advisor</span><span class="sxs-lookup"><span data-stu-id="e0715-135">Advisor Security recommendations</span></span>](advisor-cost-recommendations.md)
