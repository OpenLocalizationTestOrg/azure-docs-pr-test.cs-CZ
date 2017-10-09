---
title: "aaaGet začít s Azure Advisor | Microsoft Docs"
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
ms.openlocfilehash: 30fc8b8f3823f6f047e46cb9000189f3ccb3d514
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-advisor"></a><span data-ttu-id="db86f-103">Začínáme se službou Azure Advisor</span><span class="sxs-lookup"><span data-stu-id="db86f-103">Get started with Azure Advisor</span></span>

<span data-ttu-id="db86f-104">Zjistěte, jak implementovat tooaccess Advisor prostřednictvím hello portál Azure, get doporučení, doporučení, vyhledejte doporučení a doporučení aktualizace.</span><span class="sxs-lookup"><span data-stu-id="db86f-104">Learn how tooaccess Advisor through hello Azure portal, get recommendations, implement recommendations, search for recommendations, and refresh recommendations.</span></span>

## <a name="get-advisor-recommendations"></a><span data-ttu-id="db86f-105">Doporučení Advisoru</span><span class="sxs-lookup"><span data-stu-id="db86f-105">Get Advisor recommendations</span></span>

1. <span data-ttu-id="db86f-106">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="db86f-106">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="db86f-107">V levém podokně hello, klikněte na **další služby**.</span><span class="sxs-lookup"><span data-stu-id="db86f-107">In hello left pane, click **More services**.</span></span>

3. <span data-ttu-id="db86f-108">V hello služby nabídky podokně v části **monitorování a správu**, klikněte na tlačítko **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="db86f-108">In hello service menu pane, under **Monitoring and Management**, click **Azure Advisor**.</span></span>  
 <span data-ttu-id="db86f-109">se zobrazí řídicí panel Advisor Hello.</span><span class="sxs-lookup"><span data-stu-id="db86f-109">hello Advisor dashboard is displayed.</span></span>

   ![Přístup k Azure Advisor pomocí hello portálu Azure](./media/advisor-overview/advisor-azure-portal-menu.png) 

4. <span data-ttu-id="db86f-111">Na řídicím panelu Advisor hello vyberte hello předplatné, pro které chcete tooreceive doporučení.</span><span class="sxs-lookup"><span data-stu-id="db86f-111">On hello Advisor dashboard, select hello subscription for which you want tooreceive recommendations.</span></span>  
<span data-ttu-id="db86f-112">řídicí panel Advisor Hello zobrazuje přizpůsobené doporučení pro vybrané předplatné.</span><span class="sxs-lookup"><span data-stu-id="db86f-112">hello Advisor dashboard displays personalized recommendations for a selected subscription.</span></span> 

5. <span data-ttu-id="db86f-113">tooget doporučení pro určité kategorie, klepněte na kartu hello: **vysokou dostupnost**, **zabezpečení**, **výkonu**, nebo **náklady**.</span><span class="sxs-lookup"><span data-stu-id="db86f-113">tooget recommendations for a particular category, click one of hello tabs: **High Availability**, **Security**, **Performance**, or **Cost**.</span></span>
 
> [!NOTE]
> <span data-ttu-id="db86f-114">tooaccess doporučení služby Advisor, musíte nejdřív *zaregistrovat předplatné* službou Advisor.</span><span class="sxs-lookup"><span data-stu-id="db86f-114">tooaccess Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="db86f-115">Předplatné je zaregistrován při *předplatné vlastníka* spustí hello Advisor řídicí panel a klikne na tlačítko hello **získat doporučení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="db86f-115">A subscription is registered when a *subscription Owner* launches hello Advisor dashboard and clicks hello **Get recommendations** button.</span></span> <span data-ttu-id="db86f-116">Toto je *jednorázovou operaci*.</span><span class="sxs-lookup"><span data-stu-id="db86f-116">This is a *one-time operation*.</span></span> <span data-ttu-id="db86f-117">Po registraci předplatného hello dostanete doporučení služby Advisor jako *vlastníka*, *Přispěvatel*, nebo *čtečky* pro předplatné, skupinu prostředků nebo konkrétní prostředek.</span><span class="sxs-lookup"><span data-stu-id="db86f-117">After hello subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

  ![Řídicí panel Azure Advisor](./media/advisor-overview/advisor-all-tab.png)

## <a name="get-advisor-recommendation-details-and-implement-a-solution"></a><span data-ttu-id="db86f-119">Získat podrobnosti o doporučení služby Advisor a implementovat řešení</span><span class="sxs-lookup"><span data-stu-id="db86f-119">Get Advisor recommendation details, and implement a solution</span></span>

<span data-ttu-id="db86f-120">Hello **doporučení** okno v Advisor nabízí další informace o hello doporučení.</span><span class="sxs-lookup"><span data-stu-id="db86f-120">hello **Recommendation** blade in Advisor offers additional information about hello recommendation.</span></span> 

1. <span data-ttu-id="db86f-121">Přihlaste se toohello [portál Azure](https://portal.azure.com)a pak spusťte [Azure Advisor](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="db86f-121">Sign in toohello [Azure portal](https://portal.azure.com), and then start [Azure Advisor](https://aka.ms/azureadvisordashboard).</span></span>

2. <span data-ttu-id="db86f-122">Na hello **Advisor doporučení** řídicí panel, klikněte na tlačítko **získat doporučení**.</span><span class="sxs-lookup"><span data-stu-id="db86f-122">On hello **Advisor recommendations** dashboard, click **Get recommendations**.</span></span>

3. <span data-ttu-id="db86f-123">V seznamu hello doporučení klikněte na doporučení, které chcete tooreview podrobně.</span><span class="sxs-lookup"><span data-stu-id="db86f-123">In hello list of recommendations, click a recommendation that you want tooreview in detail.</span></span>  
<span data-ttu-id="db86f-124">Hello **doporučení** zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="db86f-124">hello **Recommendation** blade is displayed.</span></span>

4. <span data-ttu-id="db86f-125">Na hello **doporučení** okno, zkontrolujte informace o akcích, můžete provést tooresolve potenciální problém nebo využít možnost náklady na ukládání.</span><span class="sxs-lookup"><span data-stu-id="db86f-125">On hello **Recommendations** blade, review information about actions that you can perform tooresolve a potential issue, or take advantage of a cost-saving opportunity.</span></span> 
  
  ![okně doporučení služby Advisor Hello](./media/advisor-overview/advisor-recommendation-action-example.png)

## <a name="search-for-advisor-recommendations"></a><span data-ttu-id="db86f-127">Vyhledejte doporučení služby Advisor</span><span class="sxs-lookup"><span data-stu-id="db86f-127">Search for Advisor recommendations</span></span>

<span data-ttu-id="db86f-128">Můžete vyhledat doporučení pro konkrétní skupinu předplatné nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="db86f-128">You can search for recommendations for a particular subscription or resource group.</span></span> <span data-ttu-id="db86f-129">Doporučení můžete taky vyhledat podle stavu.</span><span class="sxs-lookup"><span data-stu-id="db86f-129">You can also search for recommendations by status.</span></span>

1. <span data-ttu-id="db86f-130">Přihlaste se toohello [portál Azure](https://portal.azure.com)a pak spusťte [Azure Advisor](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="db86f-130">Sign in toohello [Azure portal](https://portal.azure.com), and then start [Azure Advisor](https://aka.ms/azureadvisordashboard).</span></span>

2. <span data-ttu-id="db86f-131">Vyhledejte doporučení pomocí filtrování pro stav doporučení, skupiny prostředků a předplatná (**Active** nebo **Snoozed**).</span><span class="sxs-lookup"><span data-stu-id="db86f-131">Search for recommendations by filtering for subscriptions, resource groups, and recommendation status (**Active** or **Snoozed**).</span></span>

3. <span data-ttu-id="db86f-132">Klikněte na tlačítko toodisplay seznam doporučení služby Advisor, které jsou založeny na kritéria vyhledávání filtru **získat doporučení**.</span><span class="sxs-lookup"><span data-stu-id="db86f-132">toodisplay a list of Advisor recommendations that are based on your search-filter criteria, click **Get recommendations**.</span></span>

  ![Kritéria vyhledávací filtr služby Advisor](./media/advisor-get-started/advisor-search.png)

## <a name="snooze-or-dismiss-advisor-recommendations"></a><span data-ttu-id="db86f-134">Připomenout znovu nebo zrušit doporučení služby Advisor</span><span class="sxs-lookup"><span data-stu-id="db86f-134">Snooze or dismiss Advisor recommendations</span></span>

1. <span data-ttu-id="db86f-135">Přihlaste se toohello [portál Azure](https://portal.azure.com)a pak spusťte [Azure Advisor](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="db86f-135">Sign in toohello [Azure portal](https://portal.azure.com), and then start [Azure Advisor](https://aka.ms/azureadvisordashboard).</span></span>

2. <span data-ttu-id="db86f-136">Klikněte na tlačítko **získat doporučení**a pak hello seznam doporučení, klikněte na doporučení.</span><span class="sxs-lookup"><span data-stu-id="db86f-136">Click **Get recommendations**, and then, in hello list of recommendations, click a recommendation.</span></span>

3. <span data-ttu-id="db86f-137">Na hello **doporučení** okně klikněte na tlačítko **připomenout znovu**.</span><span class="sxs-lookup"><span data-stu-id="db86f-137">On hello **Recommendation** blade, click **Snooze**.</span></span>  

   ![Příklad akce doporučení služby Advisor](./media/advisor-get-started/advisor-snooze.png)

4. <span data-ttu-id="db86f-139">Zadejte připomenutí časové období, nebo vyberte **nikdy** toodismiss hello doporučení.</span><span class="sxs-lookup"><span data-stu-id="db86f-139">Specify a snooze time period, or select **Never** toodismiss hello recommendation.</span></span>


## <a name="next-steps"></a><span data-ttu-id="db86f-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="db86f-140">Next steps</span></span>

<span data-ttu-id="db86f-141">toolearn víc o službě Advisor, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="db86f-141">toolearn more about Advisor, see:</span></span>
* [<span data-ttu-id="db86f-142">Úvod tooAzure Advisor</span><span class="sxs-lookup"><span data-stu-id="db86f-142">Introduction tooAzure Advisor</span></span>](advisor-overview.md)
* [<span data-ttu-id="db86f-143">Doporučení pro vysokou dostupnost služby Advisor</span><span class="sxs-lookup"><span data-stu-id="db86f-143">Advisor High Availability recommendations</span></span>](advisor-high-availability-recommendations.md)
* [<span data-ttu-id="db86f-144">Doporučení zabezpečení Advisor</span><span class="sxs-lookup"><span data-stu-id="db86f-144">Advisor Security recommendations</span></span>](advisor-security-recommendations.md)
-  [<span data-ttu-id="db86f-145">Poradce při hodnocení výkonu doporučení</span><span class="sxs-lookup"><span data-stu-id="db86f-145">Advisor Performance recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="db86f-146">Náklady na doporučení služby Advisor</span><span class="sxs-lookup"><span data-stu-id="db86f-146">Advisor Cost recommendations</span></span>](advisor-performance-recommendations.md)
