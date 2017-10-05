---
title: "Vyřešení výstrahy stavu aplikace endpoint protection v Azure Security Center | Microsoft Docs"
description: "Tento dokument se dozvíte, jak provést doporučení Azure Security Center ** výstrahy stavu řešení Endpoint Protection **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 4050f453-98fc-4314-8438-d476469757fb
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2016
ms.author: terrylan
ms.openlocfilehash: 5e6b136d6bd3b11fb82126d104fd0cb149255118
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="resolve-endpoint-protection-health-alerts-in-azure-security-center"></a><span data-ttu-id="a0ff9-103">Vyřešení výstrahy stavu aplikace endpoint protection v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="a0ff9-103">Resolve endpoint protection health alerts in Azure Security Center</span></span>
<span data-ttu-id="a0ff9-104">Azure Security Center doporučí, vyřešení výstrahy stavu zjištěné endpoint protection.</span><span class="sxs-lookup"><span data-stu-id="a0ff9-104">Azure Security Center will recommend that you resolve detected endpoint protection health alerts.</span></span>  <span data-ttu-id="a0ff9-105">Security Center vám umožní zobrazit virtuálních počítačů (VM) mají selhání endpoint protection a kolik selhání.</span><span class="sxs-lookup"><span data-stu-id="a0ff9-105">Security Center enables you to see which virtual machines (VMs) have endpoint protection failures and how many failures.</span></span>

> [!NOTE]
> <span data-ttu-id="a0ff9-106">Tento dokument vám tuto službu představí formou ukázkového nasazení.</span><span class="sxs-lookup"><span data-stu-id="a0ff9-106">This document introduces the service by using an example deployment.</span></span> <span data-ttu-id="a0ff9-107">Není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="a0ff9-107">This is not a step-by-step guide.</span></span>
> 
> 

## <a name="implement-the-recommendation"></a><span data-ttu-id="a0ff9-108">Implementace doporučení</span><span class="sxs-lookup"><span data-stu-id="a0ff9-108">Implement the recommendation</span></span>
1. <span data-ttu-id="a0ff9-109">V **doporučení okno**, vyberte **výstrahy stavu řešení Endpoint Protection**.</span><span class="sxs-lookup"><span data-stu-id="a0ff9-109">In the **Recommendations blade**, select **Resolve Endpoint Protection health alerts**.</span></span>
   <span data-ttu-id="a0ff9-110">![Vyřešení výstrah stavu služby Endpoint Protection][1]</span><span class="sxs-lookup"><span data-stu-id="a0ff9-110">![Resolve endpoint protection health alerts][1]</span></span>
2. <span data-ttu-id="a0ff9-111">Otevře se okno pro **Endpoint Protection selhání** které seznamy virtuálních počítačů s chybami a počet selhání pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a0ff9-111">This opens the blade **Endpoint Protection failure** which lists VMs with failures and the number of failures for each VM.</span></span> <span data-ttu-id="a0ff9-112">Vyberte virtuální počítač ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="a0ff9-112">Select a VM from the list.</span></span>
   <span data-ttu-id="a0ff9-113">![Selhání ochrany koncového bodu][2]</span><span class="sxs-lookup"><span data-stu-id="a0ff9-113">![Endpoint protection failure][2]</span></span>
3. <span data-ttu-id="a0ff9-114">A **selhání seznamu** okno otevře pro vybraný virtuální počítač, zobrazení seznamu chyb.</span><span class="sxs-lookup"><span data-stu-id="a0ff9-114">A **Failures List** blade opens for the selected VM, displaying a list of failures.</span></span> <span data-ttu-id="a0ff9-115">Vyberte ze seznamu Další selhání.</span><span class="sxs-lookup"><span data-stu-id="a0ff9-115">Select a failure from the list to learn more.</span></span> <span data-ttu-id="a0ff9-116">Otevře se okno s informacemi o vybrané selhání.</span><span class="sxs-lookup"><span data-stu-id="a0ff9-116">This opens a blade with information about the selected failure.</span></span>
   <span data-ttu-id="a0ff9-117">![Seznam selhání][3]
   ![událostí selhání][4]</span><span class="sxs-lookup"><span data-stu-id="a0ff9-117">![Failures list][3]
![Failure event][4]</span></span>

## <a name="see-also"></a><span data-ttu-id="a0ff9-118">Viz také</span><span class="sxs-lookup"><span data-stu-id="a0ff9-118">See also</span></span>
<span data-ttu-id="a0ff9-119">Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:</span><span class="sxs-lookup"><span data-stu-id="a0ff9-119">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="a0ff9-120">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – Zjistěte, jak se konfigurují zásady zabezpečení pro vaše předplatné Azure a skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="a0ff9-120">[Setting security policies in Azure Security Center](security-center-policies.md)--Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="a0ff9-121">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – Zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="a0ff9-121">[Managing security recommendations in Azure Security Center](security-center-recommendations.md)--Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="a0ff9-122">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="a0ff9-122">[Security health monitoring in Azure Security Center](security-center-monitoring.md)--Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="a0ff9-123">[Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně.</span><span class="sxs-lookup"><span data-stu-id="a0ff9-123">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md)--Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="a0ff9-124">[Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="a0ff9-124">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how to monitor the health status of your partner solutions.</span></span>
* <span data-ttu-id="a0ff9-125">[Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby.</span><span class="sxs-lookup"><span data-stu-id="a0ff9-125">[Azure Security Center FAQ](security-center-faq.md)--Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="a0ff9-126">[Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) – Získejte nejnovější informace o zabezpečení Azure.</span><span class="sxs-lookup"><span data-stu-id="a0ff9-126">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/)--Get the latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-resolve-endpoint-protection/resolve-endpoint-protection.png
[2]: ./media/security-center-resolve-endpoint-protection/endpoint-protection-failure.png
[3]: ./media/security-center-resolve-endpoint-protection/failure-list.png
[4]: ./media/security-center-resolve-endpoint-protection/failure-event.png
