---
title: "aaaAzure doporučení zabezpečení Advisor | Microsoft Docs"
description: "Použití Azure Advisor toohelp zlepšit zabezpečení hello vašich Azure nasazení."
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
ms.openlocfilehash: e01ac29eb6e02bff0b1e846e320e7c36f85c7343
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="advisor-security-recommendations"></a><span data-ttu-id="1f968-103">Doporučení zabezpečení Advisor</span><span class="sxs-lookup"><span data-stu-id="1f968-103">Advisor Security recommendations</span></span>

<span data-ttu-id="1f968-104">Azure Advisor vám poskytne konzistentní, konsolidované zobrazení doporučení pro všechny prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="1f968-104">Azure Advisor provides you with a consistent, consolidated view of recommendations for all your Azure resources.</span></span> <span data-ttu-id="1f968-105">Se integruje s Azure Security Center toobring vám doporučení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="1f968-105">It integrates with Azure Security Center toobring you security recommendations.</span></span> <span data-ttu-id="1f968-106">Doporučení zabezpečení můžete získat z hello **zabezpečení** karty na řídicím panelu Advisor hello.</span><span class="sxs-lookup"><span data-stu-id="1f968-106">You can get security recommendations from hello **Security** tab on hello Advisor dashboard.</span></span>

![tlačítko Advisor zabezpečení Hello](./media/advisor-security-recommendations/advisor-security-tab.png)

<span data-ttu-id="1f968-108">Security Center pomáhá zabránit, zjistit a reagovat toothreats nabízí lepší přehled a kontrolu nad hello zabezpečení vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="1f968-108">Security Center helps you prevent, detect, and respond toothreats with increased visibility into and control over hello security of your Azure resources.</span></span> <span data-ttu-id="1f968-109">Pravidelně analyzuje stav zabezpečení hello vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="1f968-109">It periodically analyzes hello security state of your Azure resources.</span></span> <span data-ttu-id="1f968-110">Když Security Center identifikuje potenciální ohrožení zabezpečení, vytvoří doporučení.</span><span class="sxs-lookup"><span data-stu-id="1f968-110">When Security Center identifies potential security vulnerabilities, it creates recommendations.</span></span> <span data-ttu-id="1f968-111">Hello doporučení vás provede procesem hello konfigurace hello ovládací prvky, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="1f968-111">hello recommendations guide you through hello process of configuring hello controls you need.</span></span> 

![Karta zabezpečení Advisor Hello](./media/advisor-security-recommendations/advisor-security-recommendations.png)

<span data-ttu-id="1f968-113">Další informace o doporučení zabezpečení najdete v tématu [Správa doporučení zabezpečení v Azure Security Center](https://azure.microsoft.com/en-us/documentation/articles/security-center-recommendations/).</span><span class="sxs-lookup"><span data-stu-id="1f968-113">For more information about security recommendations, see [Managing security recommendations in Azure Security Center](https://azure.microsoft.com/en-us/documentation/articles/security-center-recommendations/).</span></span>

## <a name="how-tooaccess-security-recommendations-in-azure-advisor"></a><span data-ttu-id="1f968-114">Jak tooaccess doporučení zabezpečení v Azure Advisor</span><span class="sxs-lookup"><span data-stu-id="1f968-114">How tooaccess Security recommendations in Azure Advisor</span></span>

1. <span data-ttu-id="1f968-115">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1f968-115">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="1f968-116">V levém podokně hello, klikněte na **další služby**.</span><span class="sxs-lookup"><span data-stu-id="1f968-116">In hello left pane, click **More services**.</span></span>

3. <span data-ttu-id="1f968-117">V hello služby nabídky podokně v části **monitorování a správu**, klikněte na tlačítko **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="1f968-117">In hello service menu pane, under **Monitoring and Management**, click **Azure Advisor**.</span></span>  
 <span data-ttu-id="1f968-118">se zobrazí řídicí panel Advisor Hello.</span><span class="sxs-lookup"><span data-stu-id="1f968-118">hello Advisor dashboard is displayed.</span></span>

4. <span data-ttu-id="1f968-119">Na řídicím panelu hello Advisor, klikněte na tlačítko hello **zabezpečení** kartě.</span><span class="sxs-lookup"><span data-stu-id="1f968-119">On hello Advisor dashboard, click hello **Security** tab.</span></span>

5. <span data-ttu-id="1f968-120">Vyberte hello předplatné, pro který chcete tooreceive doporučení a pak klikněte na tlačítko **získat doporučení**.</span><span class="sxs-lookup"><span data-stu-id="1f968-120">Select hello subscription for which you want tooreceive recommendations, and then click **Get recommendations**.</span></span>

> [!NOTE]
> <span data-ttu-id="1f968-121">tooaccess doporučení služby Advisor, musíte nejdřív *zaregistrovat předplatné* službou Advisor.</span><span class="sxs-lookup"><span data-stu-id="1f968-121">tooaccess Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="1f968-122">Předplatné je zaregistrován při *předplatné vlastníka* spustí hello Advisor řídicí panel a klikne na tlačítko hello **získat doporučení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1f968-122">A subscription is registered when a *subscription Owner* launches hello Advisor dashboard and clicks hello **Get recommendations** button.</span></span> <span data-ttu-id="1f968-123">Toto je *jednorázovou operaci*.</span><span class="sxs-lookup"><span data-stu-id="1f968-123">This is a *one-time operation*.</span></span> <span data-ttu-id="1f968-124">Po registraci předplatného hello dostanete doporučení služby Advisor jako *vlastníka*, *Přispěvatel*, nebo *čtečky* pro předplatné, skupinu prostředků nebo konkrétní prostředek.</span><span class="sxs-lookup"><span data-stu-id="1f968-124">After hello subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1f968-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1f968-125">Next steps</span></span>

<span data-ttu-id="1f968-126">toolearn Další informace o doporučení služby Advisor, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="1f968-126">toolearn more about Advisor recommendations, see:</span></span>
* [<span data-ttu-id="1f968-127">TooAdvisor Úvod</span><span class="sxs-lookup"><span data-stu-id="1f968-127">Introduction tooAdvisor</span></span>](advisor-overview.md)
* [<span data-ttu-id="1f968-128">Začínáme se službou Advisor</span><span class="sxs-lookup"><span data-stu-id="1f968-128">Get started with Advisor</span></span>](advisor-get-started.md)
* [<span data-ttu-id="1f968-129">Náklady na doporučení služby Advisor</span><span class="sxs-lookup"><span data-stu-id="1f968-129">Advisor Cost recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="1f968-130">Poradce při hodnocení výkonu doporučení</span><span class="sxs-lookup"><span data-stu-id="1f968-130">Advisor Performance recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="1f968-131">Doporučení pro vysokou dostupnost služby Advisor</span><span class="sxs-lookup"><span data-stu-id="1f968-131">Advisor High Availability recommendations</span></span>](advisor-high-availability-recommendations.md)


 
