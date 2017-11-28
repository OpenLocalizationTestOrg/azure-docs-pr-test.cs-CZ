---
title: "Začínáme s Azure Advisor | Microsoft Docs"
description: "Začínáme s Azure Advisor."
services: advisor
documentationcenter: NA
author: manbeenkohli
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/10/2017
ms.author: makohli
ms.openlocfilehash: a662841bebda460d4225e080f16705b3f16fdc46
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-advisor"></a><span data-ttu-id="340dd-103">Začínáme se službou Azure Advisor</span><span class="sxs-lookup"><span data-stu-id="340dd-103">Get started with Azure Advisor</span></span>

<span data-ttu-id="340dd-104">Zjistěte, jak přístup Advisor prostřednictvím portálu Azure, získat doporučení, implementujte doporučení, vyhledejte doporučení a doporučení aktualizace.</span><span class="sxs-lookup"><span data-stu-id="340dd-104">Learn how to access Advisor through the Azure portal, get recommendations, implement recommendations, search for recommendations, and refresh recommendations.</span></span>

## <a name="get-advisor-recommendations"></a><span data-ttu-id="340dd-105">Doporučení Advisoru</span><span class="sxs-lookup"><span data-stu-id="340dd-105">Get Advisor recommendations</span></span>

1. <span data-ttu-id="340dd-106">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="340dd-106">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="340dd-107">V levém podokně klikněte na **další služby**.</span><span class="sxs-lookup"><span data-stu-id="340dd-107">In the left pane, click **More services**.</span></span>

3. <span data-ttu-id="340dd-108">V podokně nabídky služby v rámci **monitorování a správu**, klikněte na tlačítko **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="340dd-108">In the service menu pane, under **Monitoring and Management**, click **Azure Advisor**.</span></span>  
 <span data-ttu-id="340dd-109">Se zobrazí řídicí panel služby Advisor.</span><span class="sxs-lookup"><span data-stu-id="340dd-109">The Advisor dashboard is displayed.</span></span>

   ![Přístup k Azure Advisor pomocí portálu Azure](./media/advisor-overview/advisor-azure-portal-menu.png) 

4. <span data-ttu-id="340dd-111">Na řídicím panelu Advisor vyberte předplatné, pro který chcete dostávat doporučení.</span><span class="sxs-lookup"><span data-stu-id="340dd-111">On the Advisor dashboard, select the subscription for which you want to receive recommendations.</span></span>  
<span data-ttu-id="340dd-112">Řídicí panel Advisor zobrazuje přizpůsobené doporučení pro vybrané předplatné.</span><span class="sxs-lookup"><span data-stu-id="340dd-112">The Advisor dashboard displays personalized recommendations for a selected subscription.</span></span> 

5. <span data-ttu-id="340dd-113">Chcete-li získat doporučení pro určité kategorie, klikněte na jednu z karty: **vysokou dostupnost**, **zabezpečení**, **výkonu**, nebo **náklady**.</span><span class="sxs-lookup"><span data-stu-id="340dd-113">To get recommendations for a particular category, click one of the tabs: **High Availability**, **Security**, **Performance**, or **Cost**.</span></span>
 
> [!NOTE]
> <span data-ttu-id="340dd-114">Chcete-li získat přístup k doporučení služby Advisor, je nutné nejprve *zaregistrovat předplatné* službou Advisor.</span><span class="sxs-lookup"><span data-stu-id="340dd-114">To access Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="340dd-115">Předplatné je zaregistrován při *předplatné vlastníka* spustí Advisor řídicího panelu a klikne na tlačítko **získat doporučení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="340dd-115">A subscription is registered when a *subscription Owner* launches the Advisor dashboard and clicks the **Get recommendations** button.</span></span> <span data-ttu-id="340dd-116">Toto je *jednorázovou operaci*.</span><span class="sxs-lookup"><span data-stu-id="340dd-116">This is a *one-time operation*.</span></span> <span data-ttu-id="340dd-117">Po registraci předplatného dostanete doporučení služby Advisor jako *vlastníka*, *Přispěvatel*, nebo *čtečky* pro předplatné, skupinu prostředků nebo konkrétní prostředek.</span><span class="sxs-lookup"><span data-stu-id="340dd-117">After the subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

  ![Řídicí panel Azure Advisor](./media/advisor-overview/advisor-all-tab.png)

## <a name="get-advisor-recommendation-details-and-implement-a-solution"></a><span data-ttu-id="340dd-119">Získat podrobnosti o doporučení služby Advisor a implementovat řešení</span><span class="sxs-lookup"><span data-stu-id="340dd-119">Get Advisor recommendation details, and implement a solution</span></span>

<span data-ttu-id="340dd-120">**Doporučení** okno v Advisor nabízí další informace o doporučení.</span><span class="sxs-lookup"><span data-stu-id="340dd-120">The **Recommendation** blade in Advisor offers additional information about the recommendation.</span></span> 

1. <span data-ttu-id="340dd-121">Přihlaste se k [portál Azure](https://portal.azure.com)a pak spusťte [Azure Advisor](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="340dd-121">Sign in to the [Azure portal](https://portal.azure.com), and then start [Azure Advisor](https://aka.ms/azureadvisordashboard).</span></span>

2. <span data-ttu-id="340dd-122">Na **doporučení služby Advisor** řídicí panel, klikněte na tlačítko **získat doporučení**.</span><span class="sxs-lookup"><span data-stu-id="340dd-122">On the **Advisor recommendations** dashboard, click **Get recommendations**.</span></span>

3. <span data-ttu-id="340dd-123">V seznamu doporučení klikněte na doporučení, které chcete zkontrolovat podrobně.</span><span class="sxs-lookup"><span data-stu-id="340dd-123">In the list of recommendations, click a recommendation that you want to review in detail.</span></span>  
<span data-ttu-id="340dd-124">**Doporučení** zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="340dd-124">The **Recommendation** blade is displayed.</span></span>

4. <span data-ttu-id="340dd-125">Na **doporučení** okno, zkontrolujte informace o akcích, můžete provést vyřešit potenciální problém nebo využít možnost náklady na ukládání.</span><span class="sxs-lookup"><span data-stu-id="340dd-125">On the **Recommendations** blade, review information about actions that you can perform to resolve a potential issue, or take advantage of a cost-saving opportunity.</span></span> 
  
  ![V okně doporučení služby Advisor](./media/advisor-overview/advisor-recommendation-action-example.png)

## <a name="search-for-advisor-recommendations"></a><span data-ttu-id="340dd-127">Vyhledejte doporučení služby Advisor</span><span class="sxs-lookup"><span data-stu-id="340dd-127">Search for Advisor recommendations</span></span>

<span data-ttu-id="340dd-128">Můžete vyhledat doporučení pro konkrétní skupinu předplatné nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="340dd-128">You can search for recommendations for a particular subscription or resource group.</span></span> <span data-ttu-id="340dd-129">Doporučení můžete taky vyhledat podle stavu.</span><span class="sxs-lookup"><span data-stu-id="340dd-129">You can also search for recommendations by status.</span></span>

1. <span data-ttu-id="340dd-130">Přihlaste se k [portál Azure](https://portal.azure.com)a pak spusťte [Azure Advisor](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="340dd-130">Sign in to the [Azure portal](https://portal.azure.com), and then start [Azure Advisor](https://aka.ms/azureadvisordashboard).</span></span>

2. <span data-ttu-id="340dd-131">Vyhledejte doporučení pomocí filtrování pro stav doporučení, skupiny prostředků a předplatná (**Active** nebo **Snoozed**).</span><span class="sxs-lookup"><span data-stu-id="340dd-131">Search for recommendations by filtering for subscriptions, resource groups, and recommendation status (**Active** or **Snoozed**).</span></span>

3. <span data-ttu-id="340dd-132">Chcete-li zobrazit seznam doporučení Advisor, které jsou založeny na kritéria vyhledávání filtru, klikněte na tlačítko **získat doporučení**.</span><span class="sxs-lookup"><span data-stu-id="340dd-132">To display a list of Advisor recommendations that are based on your search-filter criteria, click **Get recommendations**.</span></span>

  ![Kritéria vyhledávací filtr služby Advisor](./media/advisor-get-started/advisor-search.png)

## <a name="snooze-or-dismiss-advisor-recommendations"></a><span data-ttu-id="340dd-134">Připomenout znovu nebo zrušit doporučení služby Advisor</span><span class="sxs-lookup"><span data-stu-id="340dd-134">Snooze or dismiss Advisor recommendations</span></span>

1. <span data-ttu-id="340dd-135">Přihlaste se k [portál Azure](https://portal.azure.com)a pak spusťte [Azure Advisor](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="340dd-135">Sign in to the [Azure portal](https://portal.azure.com), and then start [Azure Advisor](https://aka.ms/azureadvisordashboard).</span></span>

2. <span data-ttu-id="340dd-136">Klikněte na tlačítko **získat doporučení**a potom v seznamu doporučení, klikněte na doporučení.</span><span class="sxs-lookup"><span data-stu-id="340dd-136">Click **Get recommendations**, and then, in the list of recommendations, click a recommendation.</span></span>

3. <span data-ttu-id="340dd-137">Na **doporučení** okně klikněte na tlačítko **připomenout znovu**.</span><span class="sxs-lookup"><span data-stu-id="340dd-137">On the **Recommendation** blade, click **Snooze**.</span></span>  

   ![Příklad akce doporučení služby Advisor](./media/advisor-get-started/advisor-snooze.png)

4. <span data-ttu-id="340dd-139">Zadejte připomenutí časové období, nebo vyberte **nikdy** zrušíte doporučení.</span><span class="sxs-lookup"><span data-stu-id="340dd-139">Specify a snooze time period, or select **Never** to dismiss the recommendation.</span></span>


## <a name="next-steps"></a><span data-ttu-id="340dd-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="340dd-140">Next steps</span></span>

<span data-ttu-id="340dd-141">Další informace o službě Advisor najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="340dd-141">To learn more about Advisor, see:</span></span>
* [<span data-ttu-id="340dd-142">Úvod do Azure Advisor</span><span class="sxs-lookup"><span data-stu-id="340dd-142">Introduction to Azure Advisor</span></span>](advisor-overview.md)
* [<span data-ttu-id="340dd-143">Doporučení pro vysokou dostupnost služby Advisor</span><span class="sxs-lookup"><span data-stu-id="340dd-143">Advisor High Availability recommendations</span></span>](advisor-high-availability-recommendations.md)
* [<span data-ttu-id="340dd-144">Doporučení zabezpečení Advisor</span><span class="sxs-lookup"><span data-stu-id="340dd-144">Advisor Security recommendations</span></span>](advisor-security-recommendations.md)
-  [<span data-ttu-id="340dd-145">Poradce při hodnocení výkonu doporučení</span><span class="sxs-lookup"><span data-stu-id="340dd-145">Advisor Performance recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="340dd-146">Náklady na doporučení služby Advisor</span><span class="sxs-lookup"><span data-stu-id="340dd-146">Advisor Cost recommendations</span></span>](advisor-performance-recommendations.md)
