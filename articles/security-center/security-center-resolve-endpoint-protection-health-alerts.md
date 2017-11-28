---
title: "stav výstrahy aaaResolve endpoint protection v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak tooimplement hello Azure Security Center doporučení ** výstrahy stavu řešení Endpoint Protection **."
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
ms.openlocfilehash: 9631d15aa1dfa9003d56332363ae7911061ed0b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="resolve-endpoint-protection-health-alerts-in-azure-security-center"></a><span data-ttu-id="4a55c-103">Vyřešení výstrahy stavu aplikace endpoint protection v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="4a55c-103">Resolve endpoint protection health alerts in Azure Security Center</span></span>
<span data-ttu-id="4a55c-104">Azure Security Center doporučí, vyřešení výstrahy stavu zjištěné endpoint protection.</span><span class="sxs-lookup"><span data-stu-id="4a55c-104">Azure Security Center will recommend that you resolve detected endpoint protection health alerts.</span></span>  <span data-ttu-id="4a55c-105">Security Center umožňuje toosee virtuálních počítačů (VM) mají selhání endpoint protection a kolik selhání.</span><span class="sxs-lookup"><span data-stu-id="4a55c-105">Security Center enables you toosee which virtual machines (VMs) have endpoint protection failures and how many failures.</span></span>

> [!NOTE]
> <span data-ttu-id="4a55c-106">Toto téma představuje hello služby pomocí příklad nasazení.</span><span class="sxs-lookup"><span data-stu-id="4a55c-106">This document introduces hello service by using an example deployment.</span></span> <span data-ttu-id="4a55c-107">Není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="4a55c-107">This is not a step-by-step guide.</span></span>
> 
> 

## <a name="implement-hello-recommendation"></a><span data-ttu-id="4a55c-108">Implementace doporučení hello</span><span class="sxs-lookup"><span data-stu-id="4a55c-108">Implement hello recommendation</span></span>
1. <span data-ttu-id="4a55c-109">V hello **doporučení okno**, vyberte **výstrahy stavu řešení Endpoint Protection**.</span><span class="sxs-lookup"><span data-stu-id="4a55c-109">In hello **Recommendations blade**, select **Resolve Endpoint Protection health alerts**.</span></span>
   <span data-ttu-id="4a55c-110">![Vyřešení výstrah stavu služby Endpoint Protection][1]</span><span class="sxs-lookup"><span data-stu-id="4a55c-110">![Resolve endpoint protection health alerts][1]</span></span>
2. <span data-ttu-id="4a55c-111">Otevře se okno hello **Endpoint Protection selhání** který obsahuje seznam virtuálních počítačů s chybami a hello počet selhání pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="4a55c-111">This opens hello blade **Endpoint Protection failure** which lists VMs with failures and hello number of failures for each VM.</span></span> <span data-ttu-id="4a55c-112">Vyberte virtuální počítač ze seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="4a55c-112">Select a VM from hello list.</span></span>
   <span data-ttu-id="4a55c-113">![Selhání ochrany koncového bodu][2]</span><span class="sxs-lookup"><span data-stu-id="4a55c-113">![Endpoint protection failure][2]</span></span>
3. <span data-ttu-id="4a55c-114">A **selhání seznamu** otevře se okno pro hello vybrané virtuální počítač, zobrazení seznamu chyb.</span><span class="sxs-lookup"><span data-stu-id="4a55c-114">A **Failures List** blade opens for hello selected VM, displaying a list of failures.</span></span> <span data-ttu-id="4a55c-115">Vyberte další toolearn seznamu hello selhání.</span><span class="sxs-lookup"><span data-stu-id="4a55c-115">Select a failure from hello list toolearn more.</span></span> <span data-ttu-id="4a55c-116">Otevře se okno s informacemi o selhání hello vybrané.</span><span class="sxs-lookup"><span data-stu-id="4a55c-116">This opens a blade with information about hello selected failure.</span></span>
   <span data-ttu-id="4a55c-117">![Seznam selhání][3]
   ![událostí selhání][4]</span><span class="sxs-lookup"><span data-stu-id="4a55c-117">![Failures list][3]
![Failure event][4]</span></span>

## <a name="see-also"></a><span data-ttu-id="4a55c-118">Viz také</span><span class="sxs-lookup"><span data-stu-id="4a55c-118">See also</span></span>
<span data-ttu-id="4a55c-119">toolearn Další informace o Security Center, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="4a55c-119">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="4a55c-120">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md)– zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="4a55c-120">[Setting security policies in Azure Security Center](security-center-policies.md)--Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="4a55c-121">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – Zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="4a55c-121">[Managing security recommendations in Azure Security Center](security-center-recommendations.md)--Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="4a55c-122">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md)– zjistěte, jak toomonitor hello stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="4a55c-122">[Security health monitoring in Azure Security Center](security-center-monitoring.md)--Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="4a55c-123">[Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md)– zjistěte, jak toomanage a reakce toosecurity výstrahy.</span><span class="sxs-lookup"><span data-stu-id="4a55c-123">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md)--Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="4a55c-124">[Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="4a55c-124">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="4a55c-125">[Nejčastější dotazy k Azure Security Center](security-center-faq.md)– přečtěte si nejčastější dotazy o použití služby hello.</span><span class="sxs-lookup"><span data-stu-id="4a55c-125">[Azure Security Center FAQ](security-center-faq.md)--Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="4a55c-126">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/)– získejte nejnovější informace zabezpečení Azure hello a informace.</span><span class="sxs-lookup"><span data-stu-id="4a55c-126">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/)--Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-resolve-endpoint-protection/resolve-endpoint-protection.png
[2]: ./media/security-center-resolve-endpoint-protection/endpoint-protection-failure.png
[3]: ./media/security-center-resolve-endpoint-protection/failure-list.png
[4]: ./media/security-center-resolve-endpoint-protection/failure-event.png
