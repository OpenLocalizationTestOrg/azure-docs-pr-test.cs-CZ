---
title: "Správa partnerských řešení v Azure Security Center | Microsoft Docs"
description: "V tomto dokumentu se dozvíte, jak vám Azure Security Center umožňuje přehledně sledovat stav partnerských řešení integrovaných ve vašem předplatném Azure."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2017
ms.author: terrylan
ms.openlocfilehash: 2ebb930e877c5027f4d7b0a316a7f5ebe84471b1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="monitoring-partner-solutions-with-azure-security-center"></a><span data-ttu-id="3d20f-103">Sledování partnerských řešení pomocí Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="3d20f-103">Monitoring partner solutions with Azure Security Center</span></span>
<span data-ttu-id="3d20f-104">V tomto dokumentu se dozvíte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="3d20f-104">This document walks you through how to monitor the health status of your partner solutions in Azure Security Center.</span></span>

> [!NOTE]
> <span data-ttu-id="3d20f-105">Tento dokument vám tuto službu představí formou ukázkového nasazení.</span><span class="sxs-lookup"><span data-stu-id="3d20f-105">This document introduces the service by using an example deployment.</span></span> <span data-ttu-id="3d20f-106">Tento dokument není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="3d20f-106">This document is not a step-by-step guide.</span></span>
>
>

## <a name="monitoring-partner-solutions"></a><span data-ttu-id="3d20f-107">Sledování partnerských řešení</span><span class="sxs-lookup"><span data-stu-id="3d20f-107">Monitoring partner solutions</span></span>
<span data-ttu-id="3d20f-108">**Partner solutions** na dlaždici **Security Center** okno umožňuje přehledně sledovat stav vašich partnerských řešení, které jsou integrované s předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="3d20f-108">The **Partner solutions** tile on the **Security Center** blade lets you monitor at a glance the health status of your partner solutions that are integrated with your Azure subscription.</span></span>

![Dlaždice Partner solutions (Partnerská řešení)][1]

<span data-ttu-id="3d20f-110">**Partner solutions** dlaždice zobrazí počet partnerských řešení integrovaných ve vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="3d20f-110">The **Partner solutions** tile displays the number of partner solutions integrated with your subscription.</span></span> <span data-ttu-id="3d20f-111">Pokud integrovaná žádná řešení se zobrazí na dlaždici hodnotu nula.</span><span class="sxs-lookup"><span data-stu-id="3d20f-111">If there are no solutions integrated, the tile displays the number zero.</span></span>

<span data-ttu-id="3d20f-112">Stav partnerských řešení zobrazíte takto:</span><span class="sxs-lookup"><span data-stu-id="3d20f-112">To view the health of your partner solutions:</span></span>

1. <span data-ttu-id="3d20f-113">Vyberte dlaždici **Partner solutions** (Partnerská řešení).</span><span class="sxs-lookup"><span data-stu-id="3d20f-113">Select the **Partner solutions** tile.</span></span> <span data-ttu-id="3d20f-114">**Partner solutions** otevře se okno se seznamem partnerských řešení připojených k Security Center.</span><span class="sxs-lookup"><span data-stu-id="3d20f-114">The **Partner solutions** blade opens displaying a list of your partner solutions connected to Security Center.</span></span>

   ![Partnerská řešení][3]

   <span data-ttu-id="3d20f-116">Stav partnerských řešení může být:</span><span class="sxs-lookup"><span data-stu-id="3d20f-116">The status of a partner solution can be:</span></span>

   * <span data-ttu-id="3d20f-117">Chráněné (zelená) – Stav je zcela v pořádku.</span><span class="sxs-lookup"><span data-stu-id="3d20f-117">Protected (green) - there is no health issue.</span></span>
   * <span data-ttu-id="3d20f-118">Není v pořádku (červená) – Existuje problém stavu, které si žádá okamžitou pozornost.</span><span class="sxs-lookup"><span data-stu-id="3d20f-118">Unhealthy (red) - there is a health issue that requires immediate attention.</span></span>
   * <span data-ttu-id="3d20f-119">Nehlásí se (oranžová) – Řešení přestalo hlásit svůj stav.</span><span class="sxs-lookup"><span data-stu-id="3d20f-119">Stopped reporting (orange) - the solution has stopped reporting its health.</span></span>
   * <span data-ttu-id="3d20f-120">Neznámý stav ochrany (oranžová) – Stav řešení teď není známý, protože selhal proces přidávání nového prostředku do stávajícího řešení.</span><span class="sxs-lookup"><span data-stu-id="3d20f-120">Unknown protection status (orange) - the health of the solution is unknown at this time due to a failed process of adding a new resource to the existing solution.</span></span>
   * <span data-ttu-id="3d20f-121">Neuveden (šedá) – řešení ještě nenahlásilo nic ještě stav řešení nemusí být uvedený, pokud se nedávno připojil a ještě probíhá jeho nasazení.</span><span class="sxs-lookup"><span data-stu-id="3d20f-121">Not reported (gray) - the solution has not reported anything yet, a solution's status may be unreported if it has recently been connected and is still deploying.</span></span>

2. <span data-ttu-id="3d20f-122">Vyberte jedno partnerské řešení.</span><span class="sxs-lookup"><span data-stu-id="3d20f-122">Select a partner solution.</span></span> <span data-ttu-id="3d20f-123">V tomto příkladu umožní vybrat **Qualys** řešení.</span><span class="sxs-lookup"><span data-stu-id="3d20f-123">In this example, lets select the **Qualys** solution.</span></span>  <span data-ttu-id="3d20f-124">Otevře se okno, které obsahuje stav tohoto partnerského řešení a jeho přidružené prostředky.</span><span class="sxs-lookup"><span data-stu-id="3d20f-124">A blade opens showing you the status of the partner solution and the solution's associated resources.</span></span> <span data-ttu-id="3d20f-125">Výběrem možnosti **Solution console** (Konzola řešení) otevřete prostředí pro správu tohoto partnerského řešení.</span><span class="sxs-lookup"><span data-stu-id="3d20f-125">Select **Solution console** to open the partner management experience for this solution.</span></span>

   ![Podrobné zobrazení partnerského řešení][4]
3. <span data-ttu-id="3d20f-127">Přejděte zpět **Qualys** a vyberte **propojení virtuálních počítačů**.</span><span class="sxs-lookup"><span data-stu-id="3d20f-127">Go back to the **Qualys** blade and select **Link VM**.</span></span> <span data-ttu-id="3d20f-128">Otevře se okno **Link Applications** (Připojit aplikace).</span><span class="sxs-lookup"><span data-stu-id="3d20f-128">The **Link Applications** blade opens.</span></span> <span data-ttu-id="3d20f-129">Tady můžete ke svému partnerskému řešení připojit prostředky.</span><span class="sxs-lookup"><span data-stu-id="3d20f-129">Here you can connect resources to the partner solution.</span></span>

   ![Připojení prostředků k partnerskému řešení][5]

## <a name="next-steps"></a><span data-ttu-id="3d20f-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3d20f-131">Next steps</span></span>
<span data-ttu-id="3d20f-132">V tomto dokumentu jste se seznámili s dlaždicí **Partner Solutions** (Partnerská řešení) ve službě Security Center.</span><span class="sxs-lookup"><span data-stu-id="3d20f-132">In this document, you were introduced to the **Partner Solutions** tile in Security Center.</span></span> <span data-ttu-id="3d20f-133">Další informace o službě Security Center najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="3d20f-133">To learn more about Security Center, see the following articles:</span></span>

* <span data-ttu-id="3d20f-134">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak nakonfigurovat zásady zabezpečení pro skupiny prostředků a předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="3d20f-134">[Setting security policies in Azure Security Center](security-center-policies.md) — Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="3d20f-135">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="3d20f-135">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) — Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="3d20f-136">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se sledovat stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="3d20f-136">[Security health monitoring in Azure Security Center](security-center-monitoring.md) — Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="3d20f-137">[Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak spravovat a reakce na výstrahy zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="3d20f-137">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) — Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="3d20f-138">[Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby.</span><span class="sxs-lookup"><span data-stu-id="3d20f-138">[Azure Security Center FAQ](security-center-faq.md) — Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="3d20f-139">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – získejte nejnovější informace zabezpečení Azure a informace.</span><span class="sxs-lookup"><span data-stu-id="3d20f-139">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) — Get the latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[3]: ./media/security-center-partner-solutions/partner-solutions.png
[4]: ./media/security-center-partner-solutions/partner-solutions-detail.png
[5]: ./media/security-center-partner-solutions/link-applications.png
