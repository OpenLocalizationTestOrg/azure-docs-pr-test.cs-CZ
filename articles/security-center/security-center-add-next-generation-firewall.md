---
title: "Přidat brána firewall příští generace v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak implementovat Azure Security Center doporučení ** přidat další generace brány Firewall ** a ** trasy traffice prostřednictvím NGFW pouze **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 48b99015-4db8-4ce8-85e4-b544c0fa203e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 30589d0a943517c03394a3aae7c03c8094e78c1f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-a-next-generation-firewall-in-azure-security-center"></a><span data-ttu-id="b472c-103">Přidat Brána Firewall příští generace v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="b472c-103">Add a Next Generation Firewall in Azure Security Center</span></span>
<span data-ttu-id="b472c-104">Azure Security Center může doporučujeme, abyste přidali brána firewall příští generace (NGFW) od partnera Microsoftu ke zvýšení ochrany vaší zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="b472c-104">Azure Security Center may recommend that you add a next generation firewall (NGFW) from a Microsoft partner to increase your security protections.</span></span> <span data-ttu-id="b472c-105">Tento dokument vás příklad toho, jak to udělat provede.</span><span class="sxs-lookup"><span data-stu-id="b472c-105">This document walks you through an example of how to do this.</span></span>

> [!NOTE]
> <span data-ttu-id="b472c-106">Tento dokument vám tuto službu představí formou ukázkového nasazení.</span><span class="sxs-lookup"><span data-stu-id="b472c-106">This document introduces the service by using an example deployment.</span></span>  <span data-ttu-id="b472c-107">Není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="b472c-107">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-the-recommendation"></a><span data-ttu-id="b472c-108">Implementace doporučení</span><span class="sxs-lookup"><span data-stu-id="b472c-108">Implement the recommendation</span></span>
1. <span data-ttu-id="b472c-109">V **doporučení** vyberte **přidat Brána Firewall příští generace**.</span><span class="sxs-lookup"><span data-stu-id="b472c-109">In the **Recommendations** blade, select **Add a Next Generation Firewall**.</span></span>
   <span data-ttu-id="b472c-110">![Přidání brány firewall příští generace][1]</span><span class="sxs-lookup"><span data-stu-id="b472c-110">![Add a Next Generation Firewall][1]</span></span>
2. <span data-ttu-id="b472c-111">V **přidat Brána Firewall příští generace** okně vyberte koncový bod.</span><span class="sxs-lookup"><span data-stu-id="b472c-111">In the **Add a Next Generation Firewall** blade, select an endpoint.</span></span>
   <span data-ttu-id="b472c-112">![Vyberte koncový bod][2]</span><span class="sxs-lookup"><span data-stu-id="b472c-112">![Select an endpoint][2]</span></span>
3. <span data-ttu-id="b472c-113">Druhý **přidat Brána Firewall příští generace** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="b472c-113">A second **Add a Next Generation Firewall** blade opens.</span></span> <span data-ttu-id="b472c-114">Můžete použít existující řešení, pokud je k dispozici nebo můžete vytvořit nový.</span><span class="sxs-lookup"><span data-stu-id="b472c-114">You can choose to use an existing solution if available or you can create a new one.</span></span> <span data-ttu-id="b472c-115">V tomto příkladu nejsou žádná existující řešení k dispozici, vytvoříme NGFW.</span><span class="sxs-lookup"><span data-stu-id="b472c-115">In this example, there are no existing solutions available so we create an NGFW.</span></span>
   <span data-ttu-id="b472c-116">![Vytvoření Brána Firewall příští generace][3]</span><span class="sxs-lookup"><span data-stu-id="b472c-116">![Create Next Generation Firewall][3]</span></span>
4. <span data-ttu-id="b472c-117">Vytvořit NGFW, vyberte ze seznamu partnerů integrované řešení.</span><span class="sxs-lookup"><span data-stu-id="b472c-117">To create an NGFW, select a solution from the list of integrated partners.</span></span> <span data-ttu-id="b472c-118">V tomto příkladu jsme vyberte **kontrolní bod**.</span><span class="sxs-lookup"><span data-stu-id="b472c-118">In this example, we select **Check Point**.</span></span>
   <span data-ttu-id="b472c-119">![Vybrat řešení Brána Firewall příští generace][4]</span><span class="sxs-lookup"><span data-stu-id="b472c-119">![Select Next Generation Firewall solution][4]</span></span>
5. <span data-ttu-id="b472c-120">**Kontrolní bod** otevře se okno, takže získáte informace o partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="b472c-120">The **Check Point** blade opens providing you information about the partner solution.</span></span> <span data-ttu-id="b472c-121">Vyberte **vytvořit** v okně informace.</span><span class="sxs-lookup"><span data-stu-id="b472c-121">Select **Create** in the information blade.</span></span>
   <span data-ttu-id="b472c-122">![Okno informace o brány firewall][5]</span><span class="sxs-lookup"><span data-stu-id="b472c-122">![Firewall information blade][5]</span></span>
6. <span data-ttu-id="b472c-123">**Vytvořit virtuální počítač** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="b472c-123">The **Create virtual machine** blade opens.</span></span> <span data-ttu-id="b472c-124">Zde můžete zadat informace požadované pro číselníku virtuálního počítače (VM), který spouští NGFW.</span><span class="sxs-lookup"><span data-stu-id="b472c-124">Here you can enter information required to spin up a virtual machine (VM) that runs the NGFW.</span></span> <span data-ttu-id="b472c-125">Postupujte podle kroků a zadejte požadované informace NGFW.</span><span class="sxs-lookup"><span data-stu-id="b472c-125">Follow the steps and provide the NGFW information required.</span></span> <span data-ttu-id="b472c-126">Kliknutím na tlačítko OK použít.</span><span class="sxs-lookup"><span data-stu-id="b472c-126">Select OK to apply.</span></span>
   <span data-ttu-id="b472c-127">![Vytvoření virtuálního počítače ke spuštění NGFW][6]</span><span class="sxs-lookup"><span data-stu-id="b472c-127">![Create virtual machine to run NGFW][6]</span></span>

## <a name="route-traffic-through-ngfw-only"></a><span data-ttu-id="b472c-128">Směrovat přenosy jenom přes firewall nové generace</span><span class="sxs-lookup"><span data-stu-id="b472c-128">Route traffic through NGFW only</span></span>
<span data-ttu-id="b472c-129">Vraťte se na **doporučení** okno.</span><span class="sxs-lookup"><span data-stu-id="b472c-129">Return to the **Recommendations** blade.</span></span> <span data-ttu-id="b472c-130">Nový záznam vygenerovalo po přidání NGFW prostřednictvím Security Center, nazývá **směrování provozu prostřednictvím NGFW pouze**.</span><span class="sxs-lookup"><span data-stu-id="b472c-130">A new entry was generated after you added an NGFW via Security Center, called **Route traffic through NGFW only**.</span></span> <span data-ttu-id="b472c-131">Toto doporučení je vytvořen jen v případě, že jste nainstalovali vaší NGFW prostřednictvím Security Center.</span><span class="sxs-lookup"><span data-stu-id="b472c-131">This recommendation is created only if you installed your NGFW through Security Center.</span></span> <span data-ttu-id="b472c-132">Pokud máte internetových koncových bodů, Security Center doporučí konfiguraci pravidel skupin zabezpečení sítě, které vynutit příchozí provoz do virtuálního počítače prostřednictvím vaší NGFW.</span><span class="sxs-lookup"><span data-stu-id="b472c-132">If you have Internet-facing endpoints, Security Center recommends that you configure Network Security Group rules that force inbound traffic to your VM through your NGFW.</span></span>

1. <span data-ttu-id="b472c-133">V **doporučení okno**, vyberte **směrování provozu prostřednictvím NGFW pouze**.</span><span class="sxs-lookup"><span data-stu-id="b472c-133">In the **Recommendations blade**, select **Route traffic through NGFW only**.</span></span>
   <span data-ttu-id="b472c-134">![Směrování provozu jenom přes NGFW][7]</span><span class="sxs-lookup"><span data-stu-id="b472c-134">![Route traffic through NGFW only][7]</span></span>
2. <span data-ttu-id="b472c-135">Otevře se okno pro **směrování provozu prostřednictvím NGFW pouze**, který obsahuje seznam virtuálních počítačů, které je možné směrovat provoz.</span><span class="sxs-lookup"><span data-stu-id="b472c-135">This opens the blade **Route traffic through NGFW only**, which lists VMs that you can route traffic to.</span></span> <span data-ttu-id="b472c-136">Vyberte virtuální počítač ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="b472c-136">Select a VM from the list.</span></span>
   <span data-ttu-id="b472c-137">![Vyberte virtuální počítač][8]</span><span class="sxs-lookup"><span data-stu-id="b472c-137">![Select a VM][8]</span></span>
3. <span data-ttu-id="b472c-138">Otevře se okno pro vybraný virtuální počítač, zobrazení související příchozích pravidel.</span><span class="sxs-lookup"><span data-stu-id="b472c-138">A blade for the selected VM opens, displaying related inbound rules.</span></span> <span data-ttu-id="b472c-139">Popis vám poskytne další informace o možných další kroky.</span><span class="sxs-lookup"><span data-stu-id="b472c-139">A description provides you with more information on possible next steps.</span></span> <span data-ttu-id="b472c-140">Vyberte **upravit příchozí pravidla** pokračujte úpravy příchozího pravidla.</span><span class="sxs-lookup"><span data-stu-id="b472c-140">Select **Edit inbound rules** to proceed with editing an inbound rule.</span></span> <span data-ttu-id="b472c-141">Předpokládají, že **zdroj** není nastavený na **žádné** pro koncové body internetového propojené s NGFW.</span><span class="sxs-lookup"><span data-stu-id="b472c-141">The expectation is that **Source** is not set to **Any** for the Internet-facing endpoints linked with the NGFW.</span></span> <span data-ttu-id="b472c-142">Další informace o vlastnostech příchozí pravidlo, najdete v části [pravidla NSG](../virtual-network/virtual-networks-nsg.md#nsg-rules).</span><span class="sxs-lookup"><span data-stu-id="b472c-142">To learn more about the properties of the inbound rule, see [NSG rules](../virtual-network/virtual-networks-nsg.md#nsg-rules).</span></span>
   <span data-ttu-id="b472c-143">![Umožňuje konfigurovat pravidla k omezení přístupu][9]
   ![příchozí pravidlo úpravy][10]</span><span class="sxs-lookup"><span data-stu-id="b472c-143">![Configure rules to limit access][9]
![Edit inbound rule][10]</span></span>

## <a name="see-also"></a><span data-ttu-id="b472c-144">Viz také</span><span class="sxs-lookup"><span data-stu-id="b472c-144">See also</span></span>
<span data-ttu-id="b472c-145">Tento dokument vám ukázal, jak provést doporučení Security Center "Přidat Brána Firewall příští generace".</span><span class="sxs-lookup"><span data-stu-id="b472c-145">This document showed you how to implement the Security Center recommendation "Add a Next Generation Firewall."</span></span> <span data-ttu-id="b472c-146">Další informace o NGFWs a kontrolní bod partnerského řešení, naleznete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="b472c-146">To learn more about NGFWs and the Check Point partner solution, see the following:</span></span>

* [<span data-ttu-id="b472c-147">Brána Firewall příští generace</span><span class="sxs-lookup"><span data-stu-id="b472c-147">Next-Generation Firewall</span></span>](https://en.wikipedia.org/wiki/Next-Generation_Firewall)
* [<span data-ttu-id="b472c-148">Kontrolní bod vSEC</span><span class="sxs-lookup"><span data-stu-id="b472c-148">Check Point vSEC</span></span>](https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/)

<span data-ttu-id="b472c-149">Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:</span><span class="sxs-lookup"><span data-stu-id="b472c-149">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="b472c-150">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak nakonfigurovat zásady zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="b472c-150">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how to configure security policies.</span></span>
* <span data-ttu-id="b472c-151">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="b472c-151">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="b472c-152">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="b472c-152">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="b472c-153">[Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně.</span><span class="sxs-lookup"><span data-stu-id="b472c-153">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="b472c-154">[Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="b472c-154">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how to monitor the health status of your partner solutions.</span></span>
* <span data-ttu-id="b472c-155">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – Přečtěte si nejčastější dotazy k používání této služby.</span><span class="sxs-lookup"><span data-stu-id="b472c-155">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="b472c-156">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="b472c-156">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Find blog posts about Azure security and compliance.</span></span>

<!--Image references-->
[1]: ./media/security-center-add-next-gen-firewall/add-next-gen-firewall.png
[2]: ./media/security-center-add-next-gen-firewall/select-an-endpoint.png
[3]: ./media/security-center-add-next-gen-firewall/create-new-next-gen-firewall.png
[4]: ./media/security-center-add-next-gen-firewall/select-next-gen-firewall.png
[5]: ./media/security-center-add-next-gen-firewall/firewall-solution-info-blade.png
[6]: ./media/security-center-add-next-gen-firewall/create-virtual-machine.png
[7]: ./media/security-center-add-next-gen-firewall/route-traffic-through-ngfw.png
[8]: ./media/security-center-add-next-gen-firewall/select-vm.png
[9]: ./media/security-center-add-next-gen-firewall/configure-rules-to-limit-access.png
[10]: ./media/security-center-add-next-gen-firewall/edit-inbound-rule.png
