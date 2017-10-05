---
title: "Verze aktualizace operačního systému v Azure Security Center | Microsoft Docs"
description: "V tomto článku se dozvíte, jak provést doporučení Azure Security Center ** aktualizace operačního systému verze **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: aa372492-ecdb-4368-8fdd-d8ed31e216ee
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2016
ms.author: terrylan
ms.openlocfilehash: ce0d178914907750e5da59f223a4b1e04b9bb6fb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="update-os-version-in-azure-security-center"></a><span data-ttu-id="e5131-103">Aktualizovat verzi operačního systému v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="e5131-103">Update OS version in Azure Security Center</span></span>
<span data-ttu-id="e5131-104">Pro virtuální počítače (VM) v cloudové služby Azure Security Center vám doporučí aktualizovat operační systém (OS) Pokud není k dispozici novější verze.</span><span class="sxs-lookup"><span data-stu-id="e5131-104">For virtual machines (VMs) in cloud services, Azure Security Center will recommend that the operating system (OS) be updated if there is a more recent version available.</span></span>  <span data-ttu-id="e5131-105">Pouze cloudových služeb webové a pracovní role spuštění v produkčním prostředí, které jsou monitorovány sloty.</span><span class="sxs-lookup"><span data-stu-id="e5131-105">Only cloud services web and worker roles running in production slots are monitored.</span></span>

> [!NOTE]
> <span data-ttu-id="e5131-106">Tento dokument vám tuto službu představí formou ukázkového nasazení.</span><span class="sxs-lookup"><span data-stu-id="e5131-106">This document introduces the service by using an example deployment.</span></span>  <span data-ttu-id="e5131-107">Není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="e5131-107">This is not a step-by-step guide.</span></span>
> 
> 

## <a name="implement-the-recommendation"></a><span data-ttu-id="e5131-108">Implementace doporučení</span><span class="sxs-lookup"><span data-stu-id="e5131-108">Implement the recommendation</span></span>
1. <span data-ttu-id="e5131-109">V **doporučení** vyberte **verze operačního systému aktualizaci**.</span><span class="sxs-lookup"><span data-stu-id="e5131-109">In the **Recommendations** blade, select **Update OS version**.</span></span>
   <span data-ttu-id="e5131-110">![Aktualizace verze operačního systému][1]</span><span class="sxs-lookup"><span data-stu-id="e5131-110">![Update OS version][1]</span></span>
2. <span data-ttu-id="e5131-111">Otevře se okno pro **verze operačního systému aktualizaci**.</span><span class="sxs-lookup"><span data-stu-id="e5131-111">This opens the blade **Update OS version**.</span></span> <span data-ttu-id="e5131-112">Postupujte podle kroků v tomto okně Aktualizovat verzi operačního systému.</span><span class="sxs-lookup"><span data-stu-id="e5131-112">Follow the steps in this blade to update the OS version.</span></span>

## <a name="see-also"></a><span data-ttu-id="e5131-113">Viz také</span><span class="sxs-lookup"><span data-stu-id="e5131-113">See also</span></span>
<span data-ttu-id="e5131-114">Tento článek vám ukázal, jak provést doporučení Security Center "Verze aktualizace operačního systému."</span><span class="sxs-lookup"><span data-stu-id="e5131-114">This article showed you how to implement the Security Center recommendation "Update OS version."</span></span> <span data-ttu-id="e5131-115">Další informace o službách cloud services a aktualizace verze operačního systému pro cloudovou službu, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="e5131-115">To learn more about cloud services and updating the OS version for a cloud service, see:</span></span>

* [<span data-ttu-id="e5131-116">Přehled Cloud Services</span><span class="sxs-lookup"><span data-stu-id="e5131-116">Cloud Services overview</span></span>](../cloud-services/cloud-services-choose-me.md)
* [<span data-ttu-id="e5131-117">Postup aktualizace cloudové služby</span><span class="sxs-lookup"><span data-stu-id="e5131-117">How to update a cloud service</span></span>](../cloud-services/cloud-services-update-azure-service.md)
* [<span data-ttu-id="e5131-118">Jak konfigurovat Cloud Services</span><span class="sxs-lookup"><span data-stu-id="e5131-118">How to Configure Cloud Services</span></span>](../cloud-services/cloud-services-how-to-configure-portal.md)

<span data-ttu-id="e5131-119">Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:</span><span class="sxs-lookup"><span data-stu-id="e5131-119">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="e5131-120">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – Zjistěte, jak konfigurovat zásady zabezpečení pro svá předplatná Azure a skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="e5131-120">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="e5131-121">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="e5131-121">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="e5131-122">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="e5131-122">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="e5131-123">[Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně.</span><span class="sxs-lookup"><span data-stu-id="e5131-123">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="e5131-124">[Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="e5131-124">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how to monitor the health status of your partner solutions.</span></span>
* <span data-ttu-id="e5131-125">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – Přečtěte si nejčastější dotazy k používání této služby.</span><span class="sxs-lookup"><span data-stu-id="e5131-125">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="e5131-126">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – získejte nejnovější informace zabezpečení Azure a informace.</span><span class="sxs-lookup"><span data-stu-id="e5131-126">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get the latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-update-os-version/update-os-version.png
