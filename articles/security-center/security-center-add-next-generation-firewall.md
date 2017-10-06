---
title: "aaaAdd brána firewall příští generace v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak tooimplement hello doporučení služby Azure Security Center ** přidat další generace brány Firewall ** a ** trasy traffice prostřednictvím NGFW pouze **."
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
ms.openlocfilehash: 9a80f12571ba08eadf3361728c6321388c863235
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-next-generation-firewall-in-azure-security-center"></a><span data-ttu-id="dc875-103">Přidat Brána Firewall příští generace v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="dc875-103">Add a Next Generation Firewall in Azure Security Center</span></span>
<span data-ttu-id="dc875-104">Azure Security Center může doporučujeme přidat brána firewall příští generace (NGFW) z tooincrease partnera Microsoft vaše ochranu zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="dc875-104">Azure Security Center may recommend that you add a next generation firewall (NGFW) from a Microsoft partner tooincrease your security protections.</span></span> <span data-ttu-id="dc875-105">Tento dokument vás příklad provede toodo to.</span><span class="sxs-lookup"><span data-stu-id="dc875-105">This document walks you through an example of how toodo this.</span></span>

> [!NOTE]
> <span data-ttu-id="dc875-106">Toto téma představuje hello služby pomocí příklad nasazení.</span><span class="sxs-lookup"><span data-stu-id="dc875-106">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="dc875-107">Není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="dc875-107">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="dc875-108">Implementace doporučení hello</span><span class="sxs-lookup"><span data-stu-id="dc875-108">Implement hello recommendation</span></span>
1. <span data-ttu-id="dc875-109">V hello **doporučení** vyberte **přidat Brána Firewall příští generace**.</span><span class="sxs-lookup"><span data-stu-id="dc875-109">In hello **Recommendations** blade, select **Add a Next Generation Firewall**.</span></span>
   <span data-ttu-id="dc875-110">![Přidání brány firewall příští generace][1]</span><span class="sxs-lookup"><span data-stu-id="dc875-110">![Add a Next Generation Firewall][1]</span></span>
2. <span data-ttu-id="dc875-111">V hello **přidat Brána Firewall příští generace** okně vyberte koncový bod.</span><span class="sxs-lookup"><span data-stu-id="dc875-111">In hello **Add a Next Generation Firewall** blade, select an endpoint.</span></span>
   <span data-ttu-id="dc875-112">![Vyberte koncový bod][2]</span><span class="sxs-lookup"><span data-stu-id="dc875-112">![Select an endpoint][2]</span></span>
3. <span data-ttu-id="dc875-113">Druhý **přidat Brána Firewall příští generace** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="dc875-113">A second **Add a Next Generation Firewall** blade opens.</span></span> <span data-ttu-id="dc875-114">Můžete zvolit toouse do stávajícího řešení Pokud je k dispozici nebo můžete vytvořit nový.</span><span class="sxs-lookup"><span data-stu-id="dc875-114">You can choose toouse an existing solution if available or you can create a new one.</span></span> <span data-ttu-id="dc875-115">V tomto příkladu nejsou žádná existující řešení k dispozici, vytvoříme NGFW.</span><span class="sxs-lookup"><span data-stu-id="dc875-115">In this example, there are no existing solutions available so we create an NGFW.</span></span>
   <span data-ttu-id="dc875-116">![Vytvoření Brána Firewall příští generace][3]</span><span class="sxs-lookup"><span data-stu-id="dc875-116">![Create Next Generation Firewall][3]</span></span>
4. <span data-ttu-id="dc875-117">toocreate NGFW, vybrat řešení z hello seznam integrovaných partnerů.</span><span class="sxs-lookup"><span data-stu-id="dc875-117">toocreate an NGFW, select a solution from hello list of integrated partners.</span></span> <span data-ttu-id="dc875-118">V tomto příkladu jsme vyberte **kontrolní bod**.</span><span class="sxs-lookup"><span data-stu-id="dc875-118">In this example, we select **Check Point**.</span></span>
   <span data-ttu-id="dc875-119">![Vybrat řešení Brána Firewall příští generace][4]</span><span class="sxs-lookup"><span data-stu-id="dc875-119">![Select Next Generation Firewall solution][4]</span></span>
5. <span data-ttu-id="dc875-120">Hello **kontrolní bod** otevře se okno poskytování informací o hello partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="dc875-120">hello **Check Point** blade opens providing you information about hello partner solution.</span></span> <span data-ttu-id="dc875-121">Vyberte **vytvořit** v okně informace hello.</span><span class="sxs-lookup"><span data-stu-id="dc875-121">Select **Create** in hello information blade.</span></span>
   <span data-ttu-id="dc875-122">![Okno informace o brány firewall][5]</span><span class="sxs-lookup"><span data-stu-id="dc875-122">![Firewall information blade][5]</span></span>
6. <span data-ttu-id="dc875-123">Hello **vytvořit virtuální počítač** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="dc875-123">hello **Create virtual machine** blade opens.</span></span> <span data-ttu-id="dc875-124">Zde můžete zadat informace požadované toospin virtuálního počítače (VM), který spouští hello NGFW.</span><span class="sxs-lookup"><span data-stu-id="dc875-124">Here you can enter information required toospin up a virtual machine (VM) that runs hello NGFW.</span></span> <span data-ttu-id="dc875-125">Postupujte podle kroků hello a zadejte požadované informace NGFW hello.</span><span class="sxs-lookup"><span data-stu-id="dc875-125">Follow hello steps and provide hello NGFW information required.</span></span> <span data-ttu-id="dc875-126">Vyberte OK tooapply.</span><span class="sxs-lookup"><span data-stu-id="dc875-126">Select OK tooapply.</span></span>
   <span data-ttu-id="dc875-127">![Vytvoření virtuálního počítače toorun NGFW][6]</span><span class="sxs-lookup"><span data-stu-id="dc875-127">![Create virtual machine toorun NGFW][6]</span></span>

## <a name="route-traffic-through-ngfw-only"></a><span data-ttu-id="dc875-128">Směrovat přenosy jenom přes firewall nové generace</span><span class="sxs-lookup"><span data-stu-id="dc875-128">Route traffic through NGFW only</span></span>
<span data-ttu-id="dc875-129">Vrátí toohello **doporučení** okno.</span><span class="sxs-lookup"><span data-stu-id="dc875-129">Return toohello **Recommendations** blade.</span></span> <span data-ttu-id="dc875-130">Nový záznam vygenerovalo po přidání NGFW prostřednictvím Security Center, nazývá **směrování provozu prostřednictvím NGFW pouze**.</span><span class="sxs-lookup"><span data-stu-id="dc875-130">A new entry was generated after you added an NGFW via Security Center, called **Route traffic through NGFW only**.</span></span> <span data-ttu-id="dc875-131">Toto doporučení je vytvořen jen v případě, že jste nainstalovali vaší NGFW prostřednictvím Security Center.</span><span class="sxs-lookup"><span data-stu-id="dc875-131">This recommendation is created only if you installed your NGFW through Security Center.</span></span> <span data-ttu-id="dc875-132">Pokud máte internetových koncových bodů, Security Center doporučí konfiguraci pravidel skupin zabezpečení sítě, které vynutit tooyour příchozí přenosy virtuálních počítačů prostřednictvím vaší NGFW.</span><span class="sxs-lookup"><span data-stu-id="dc875-132">If you have Internet-facing endpoints, Security Center recommends that you configure Network Security Group rules that force inbound traffic tooyour VM through your NGFW.</span></span>

1. <span data-ttu-id="dc875-133">V hello **doporučení okno**, vyberte **směrování provozu prostřednictvím NGFW pouze**.</span><span class="sxs-lookup"><span data-stu-id="dc875-133">In hello **Recommendations blade**, select **Route traffic through NGFW only**.</span></span>
   <span data-ttu-id="dc875-134">![Směrování provozu jenom přes NGFW][7]</span><span class="sxs-lookup"><span data-stu-id="dc875-134">![Route traffic through NGFW only][7]</span></span>
2. <span data-ttu-id="dc875-135">Otevře se okno hello **směrování provozu prostřednictvím NGFW pouze**, který obsahuje seznam virtuálních počítačů, které je možné směrovat provoz.</span><span class="sxs-lookup"><span data-stu-id="dc875-135">This opens hello blade **Route traffic through NGFW only**, which lists VMs that you can route traffic to.</span></span> <span data-ttu-id="dc875-136">Vyberte virtuální počítač ze seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="dc875-136">Select a VM from hello list.</span></span>
   <span data-ttu-id="dc875-137">![Vyberte virtuální počítač][8]</span><span class="sxs-lookup"><span data-stu-id="dc875-137">![Select a VM][8]</span></span>
3. <span data-ttu-id="dc875-138">Okno pro hello vybraný virtuální počítač se otevře, zobrazení související příchozích pravidel.</span><span class="sxs-lookup"><span data-stu-id="dc875-138">A blade for hello selected VM opens, displaying related inbound rules.</span></span> <span data-ttu-id="dc875-139">Popis vám poskytne další informace o možných další kroky.</span><span class="sxs-lookup"><span data-stu-id="dc875-139">A description provides you with more information on possible next steps.</span></span> <span data-ttu-id="dc875-140">Vyberte **upravit příchozí pravidla** tooproceed s úpravy příchozího pravidla.</span><span class="sxs-lookup"><span data-stu-id="dc875-140">Select **Edit inbound rules** tooproceed with editing an inbound rule.</span></span> <span data-ttu-id="dc875-141">Hello předpokládají, že **zdroj** není nastaven příliš**žádné** pro koncové body internetového hello propojené s hello NGFW.</span><span class="sxs-lookup"><span data-stu-id="dc875-141">hello expectation is that **Source** is not set too**Any** for hello Internet-facing endpoints linked with hello NGFW.</span></span> <span data-ttu-id="dc875-142">toolearn Další informace o vlastnosti hello hello příchozí pravidlo, najdete v části [pravidla NSG](../virtual-network/virtual-networks-nsg.md#nsg-rules).</span><span class="sxs-lookup"><span data-stu-id="dc875-142">toolearn more about hello properties of hello inbound rule, see [NSG rules](../virtual-network/virtual-networks-nsg.md#nsg-rules).</span></span>
   <span data-ttu-id="dc875-143">![Konfigurace pravidel přístupu toolimit][9]
   ![příchozí pravidlo úpravy][10]</span><span class="sxs-lookup"><span data-stu-id="dc875-143">![Configure rules toolimit access][9]
![Edit inbound rule][10]</span></span>

## <a name="see-also"></a><span data-ttu-id="dc875-144">Viz také</span><span class="sxs-lookup"><span data-stu-id="dc875-144">See also</span></span>
<span data-ttu-id="dc875-145">Tento dokument ukázal, jak tooimplement hello Security Center doporučení "Přidat Brána Firewall příští generace".</span><span class="sxs-lookup"><span data-stu-id="dc875-145">This document showed you how tooimplement hello Security Center recommendation "Add a Next Generation Firewall."</span></span> <span data-ttu-id="dc875-146">toolearn informace o NGFWs a hello kontrolní bod partnerských řešení, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="dc875-146">toolearn more about NGFWs and hello Check Point partner solution, see hello following:</span></span>

* [<span data-ttu-id="dc875-147">Brána Firewall příští generace</span><span class="sxs-lookup"><span data-stu-id="dc875-147">Next-Generation Firewall</span></span>](https://en.wikipedia.org/wiki/Next-Generation_Firewall)
* [<span data-ttu-id="dc875-148">Kontrolní bod vSEC</span><span class="sxs-lookup"><span data-stu-id="dc875-148">Check Point vSEC</span></span>](https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/)

<span data-ttu-id="dc875-149">toolearn Další informace o Security Center, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="dc875-149">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="dc875-150">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="dc875-150">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies.</span></span>
* <span data-ttu-id="dc875-151">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="dc875-151">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="dc875-152">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="dc875-152">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="dc875-153">[Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.</span><span class="sxs-lookup"><span data-stu-id="dc875-153">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="dc875-154">[Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="dc875-154">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="dc875-155">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.</span><span class="sxs-lookup"><span data-stu-id="dc875-155">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="dc875-156">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="dc875-156">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Find blog posts about Azure security and compliance.</span></span>

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
