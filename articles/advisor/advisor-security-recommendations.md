---
title: "Doporučení zabezpečení Azure Advisor | Microsoft Docs"
description: "Pomocí Azure Advisor k vylepšení zabezpečení vašich Azure nasazení."
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
ms.openlocfilehash: 53be05593c3733ccb27979a3a026414013125779
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="advisor-security-recommendations"></a><span data-ttu-id="5c498-103">Doporučení zabezpečení Advisor</span><span class="sxs-lookup"><span data-stu-id="5c498-103">Advisor Security recommendations</span></span>

<span data-ttu-id="5c498-104">Azure Advisor vám poskytne konzistentní, konsolidované zobrazení doporučení pro všechny prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="5c498-104">Azure Advisor provides you with a consistent, consolidated view of recommendations for all your Azure resources.</span></span> <span data-ttu-id="5c498-105">Integruje se s Azure Security Center, aby vám doporučení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="5c498-105">It integrates with Azure Security Center to bring you security recommendations.</span></span> <span data-ttu-id="5c498-106">Doporučení zabezpečení z můžete získat **zabezpečení** na řídicím panelu služby Advisor.</span><span class="sxs-lookup"><span data-stu-id="5c498-106">You can get security recommendations from the **Security** tab on the Advisor dashboard.</span></span>

![Tlačítko Advisor zabezpečení](./media/advisor-security-recommendations/advisor-security-tab.png)

<span data-ttu-id="5c498-108">Security Center pomáhá předcházet hrozbám, zjišťovat je a reagovat na ně a nabízí lepší přehled o zabezpečení prostředků Azure a kontrolu nad nimi.</span><span class="sxs-lookup"><span data-stu-id="5c498-108">Security Center helps you prevent, detect, and respond to threats with increased visibility into and control over the security of your Azure resources.</span></span> <span data-ttu-id="5c498-109">Pravidelně analyzuje stav zabezpečení vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="5c498-109">It periodically analyzes the security state of your Azure resources.</span></span> <span data-ttu-id="5c498-110">Když Security Center identifikuje potenciální ohrožení zabezpečení, vytvoří doporučení.</span><span class="sxs-lookup"><span data-stu-id="5c498-110">When Security Center identifies potential security vulnerabilities, it creates recommendations.</span></span> <span data-ttu-id="5c498-111">Doporučení vás provede procesem konfigurace ovládacích prvků, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="5c498-111">The recommendations guide you through the process of configuring the controls you need.</span></span> 

![Na kartě zabezpečení Advisor](./media/advisor-security-recommendations/advisor-security-recommendations.png)

<span data-ttu-id="5c498-113">Další informace o doporučení zabezpečení najdete v tématu [Správa doporučení zabezpečení v Azure Security Center](https://azure.microsoft.com/en-us/documentation/articles/security-center-recommendations/).</span><span class="sxs-lookup"><span data-stu-id="5c498-113">For more information about security recommendations, see [Managing security recommendations in Azure Security Center](https://azure.microsoft.com/en-us/documentation/articles/security-center-recommendations/).</span></span>

## <a name="how-to-access-security-recommendations-in-azure-advisor"></a><span data-ttu-id="5c498-114">Jak získat přístup k doporučení zabezpečení v Azure Advisor</span><span class="sxs-lookup"><span data-stu-id="5c498-114">How to access Security recommendations in Azure Advisor</span></span>

1. <span data-ttu-id="5c498-115">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5c498-115">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="5c498-116">V levém podokně klikněte na **další služby**.</span><span class="sxs-lookup"><span data-stu-id="5c498-116">In the left pane, click **More services**.</span></span>

3. <span data-ttu-id="5c498-117">V podokně nabídky služby v rámci **monitorování a správu**, klikněte na tlačítko **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="5c498-117">In the service menu pane, under **Monitoring and Management**, click **Azure Advisor**.</span></span>  
 <span data-ttu-id="5c498-118">Se zobrazí řídicí panel služby Advisor.</span><span class="sxs-lookup"><span data-stu-id="5c498-118">The Advisor dashboard is displayed.</span></span>

4. <span data-ttu-id="5c498-119">Na řídicím panelu služby Advisor, klikněte na **zabezpečení** kartě.</span><span class="sxs-lookup"><span data-stu-id="5c498-119">On the Advisor dashboard, click the **Security** tab.</span></span>

5. <span data-ttu-id="5c498-120">Vyberte předplatné, pro který chcete dostávat doporučení a potom klikněte na **získat doporučení**.</span><span class="sxs-lookup"><span data-stu-id="5c498-120">Select the subscription for which you want to receive recommendations, and then click **Get recommendations**.</span></span>

> [!NOTE]
> <span data-ttu-id="5c498-121">Chcete-li získat přístup k doporučení služby Advisor, je nutné nejprve *zaregistrovat předplatné* službou Advisor.</span><span class="sxs-lookup"><span data-stu-id="5c498-121">To access Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="5c498-122">Předplatné je zaregistrován při *předplatné vlastníka* spustí Advisor řídicího panelu a klikne na tlačítko **získat doporučení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5c498-122">A subscription is registered when a *subscription Owner* launches the Advisor dashboard and clicks the **Get recommendations** button.</span></span> <span data-ttu-id="5c498-123">Toto je *jednorázovou operaci*.</span><span class="sxs-lookup"><span data-stu-id="5c498-123">This is a *one-time operation*.</span></span> <span data-ttu-id="5c498-124">Po registraci předplatného dostanete doporučení služby Advisor jako *vlastníka*, *Přispěvatel*, nebo *čtečky* pro předplatné, skupinu prostředků nebo konkrétní prostředek.</span><span class="sxs-lookup"><span data-stu-id="5c498-124">After the subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5c498-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5c498-125">Next steps</span></span>

<span data-ttu-id="5c498-126">Další informace o doporučení služby Advisor najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="5c498-126">To learn more about Advisor recommendations, see:</span></span>
* [<span data-ttu-id="5c498-127">Úvod do služby Advisor</span><span class="sxs-lookup"><span data-stu-id="5c498-127">Introduction to Advisor</span></span>](advisor-overview.md)
* [<span data-ttu-id="5c498-128">Začínáme se službou Advisor</span><span class="sxs-lookup"><span data-stu-id="5c498-128">Get started with Advisor</span></span>](advisor-get-started.md)
* [<span data-ttu-id="5c498-129">Náklady na doporučení služby Advisor</span><span class="sxs-lookup"><span data-stu-id="5c498-129">Advisor Cost recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="5c498-130">Poradce při hodnocení výkonu doporučení</span><span class="sxs-lookup"><span data-stu-id="5c498-130">Advisor Performance recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="5c498-131">Doporučení pro vysokou dostupnost služby Advisor</span><span class="sxs-lookup"><span data-stu-id="5c498-131">Advisor High Availability recommendations</span></span>](advisor-high-availability-recommendations.md)


 
