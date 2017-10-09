---
title: "Slabá místa zabezpečení aaaRemediate operačního systému v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak tooimplement hello Azure Security Center doporučení ** ohrožení zabezpečení operačního systému opravit **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 991d41f5-1d17-468d-a66d-83ec1308ab79
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: 704103f7fb15835943d74b665d2bd56cb5e0a36d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="remediate-os-vulnerabilities-in-azure-security-center"></a><span data-ttu-id="8d1de-103">Oprava chyb zabezpečení operačního systému v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="8d1de-103">Remediate OS vulnerabilities in Azure Security Center</span></span>
<span data-ttu-id="8d1de-104">Azure Security Center analyzuje denně virtuální počítač (VM) operačního systému (OS) pro konfigurace, které by mohly zvýšit hello virtuálních počítačů zranitelnější konfigurace tooattack a doporučuje změny tooaddress tyto chyby zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="8d1de-104">Azure Security Center analyzes daily your virtual machine (VM) operating system (OS) for configurations that could make hello VM more vulnerable tooattack and recommends configuration changes tooaddress these vulnerabilities.</span></span> <span data-ttu-id="8d1de-105">Security Center doporučí, vyřešte chyby zabezpečení, pokud konfigurace operačního systému Virtuálního počítače neodpovídá hello doporučená pravidla konfigurace.</span><span class="sxs-lookup"><span data-stu-id="8d1de-105">Security Center recommends that you resolve vulnerabilities when your VM’s OS configuration does not match hello recommended configuration rules.</span></span>

> [!NOTE]
> <span data-ttu-id="8d1de-106">Další informace o hello konkrétní konfigurace se sledují, najdete v části hello [seznam doporučenou konfiguraci pravidel](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).</span><span class="sxs-lookup"><span data-stu-id="8d1de-106">For more information on hello specific configurations being monitored, see hello [list of recommended configuration rules](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="8d1de-107">Implementace doporučení hello</span><span class="sxs-lookup"><span data-stu-id="8d1de-107">Implement hello recommendation</span></span>

> [!NOTE]
> <span data-ttu-id="8d1de-108">Toto téma představuje hello služby pomocí příklad nasazení.</span><span class="sxs-lookup"><span data-stu-id="8d1de-108">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="8d1de-109">Tento dokument není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="8d1de-109">This document is not a step-by-step guide.</span></span>
>
>

1. <span data-ttu-id="8d1de-110">V hello **doporučení** vyberte **ohrožení zabezpečení operačního systému opravit**.</span><span class="sxs-lookup"><span data-stu-id="8d1de-110">In hello **Recommendations** blade, select **Remediate OS vulnerabilities**.</span></span>
   <span data-ttu-id="8d1de-111">![Náprava ohrožení zabezpečení operačního systému][1]</span><span class="sxs-lookup"><span data-stu-id="8d1de-111">![Remediate OS vulnerabilities][1]</span></span>

    <span data-ttu-id="8d1de-112">Hello **ohrožení zabezpečení operačního systému opravit** okno k otevření a virtuální počítače s konfigurací operačního systému, které neodpovídají hello doporučená konfigurační pravidla.</span><span class="sxs-lookup"><span data-stu-id="8d1de-112">hello **Remediate OS vulnerabilities** blade opens and lists your VMs with OS configurations that do not match hello recommended configuration rules.</span></span>  <span data-ttu-id="8d1de-113">Pro každý virtuální počítač identifikuje hello okno:</span><span class="sxs-lookup"><span data-stu-id="8d1de-113">For each VM, hello blade identifies:</span></span>

   * <span data-ttu-id="8d1de-114">**PRAVIDLA se nezdařilo** – hello počet pravidel, které hello konfigurace operačního systému Virtuálního počítače se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="8d1de-114">**FAILED RULES** -- hello number of rules that hello VM's OS configuration failed.</span></span>
   * <span data-ttu-id="8d1de-115">**ČAS poslední kontroly** – hello datum a čas, Security Center naposledy hledala konfigurace operačního systému hello Virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8d1de-115">**LAST SCAN TIME** -- hello date and time that Security Center last scanned hello VM’s OS configuration.</span></span>
   * <span data-ttu-id="8d1de-116">**Stav** – hello aktuální stav hello ohrožení zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="8d1de-116">**STATE** -- hello current state of hello vulnerability:</span></span>

     * <span data-ttu-id="8d1de-117">Otevřete: ohrožení zabezpečení hello dosud nebylo řešeno.</span><span class="sxs-lookup"><span data-stu-id="8d1de-117">Open: hello vulnerability has not been addressed yet</span></span>
     * <span data-ttu-id="8d1de-118">V průběhu: Ohrožení zabezpečení hello se aktuálně, není třeba žádné akce</span><span class="sxs-lookup"><span data-stu-id="8d1de-118">In Progress: hello vulnerability is currently being applied, no action is required by you</span></span>
     * <span data-ttu-id="8d1de-119">Přeložit: ohrožení zabezpečení hello byl již řešit (když hello problém vyřešen, hello položka je zobrazena šedě)</span><span class="sxs-lookup"><span data-stu-id="8d1de-119">Resolved: hello vulnerability was already addressed (when hello issue has been resolved, hello entry is grayed out)</span></span>
   * <span data-ttu-id="8d1de-120">**ZÁVAŽNOST** – všechna ohrožení zabezpečení. jsou nastaveny tooa závažnost nízká, což znamená, ohrožení zabezpečení, mělo by se řešit, ale nevyžaduje okamžitou pozornost.</span><span class="sxs-lookup"><span data-stu-id="8d1de-120">**SEVERITY** -- All vulnerabilities are set tooa severity of Low, meaning a vulnerability should be addressed but does not require immediate attention.</span></span>

2. <span data-ttu-id="8d1de-121">Vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="8d1de-121">Select a VM.</span></span> <span data-ttu-id="8d1de-122">Okno pro tento virtuální počítač se zobrazí hello pravidla, které selhaly.</span><span class="sxs-lookup"><span data-stu-id="8d1de-122">A blade for that VM opens and displays hello rules that have failed.</span></span>
   <span data-ttu-id="8d1de-123">![Pravidla konfigurace, které selhaly][2]</span><span class="sxs-lookup"><span data-stu-id="8d1de-123">![Configuration rules that have failed][2]</span></span>

3. <span data-ttu-id="8d1de-124">Vyberte pravidlo.</span><span class="sxs-lookup"><span data-stu-id="8d1de-124">Select a rule.</span></span> <span data-ttu-id="8d1de-125">V tomto příkladu umožní vybrat **heslo musí splňovat požadavky na složitost**.</span><span class="sxs-lookup"><span data-stu-id="8d1de-125">In this example, lets select **Password must meet complexity requirements**.</span></span> <span data-ttu-id="8d1de-126">Otevře se okno popisující dopad pravidlo a hello hello se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="8d1de-126">A blade opens describing hello failed rule and hello impact.</span></span> <span data-ttu-id="8d1de-127">Zkontrolujte podrobnosti hello a zvažte, jak se používají konfigurace operačního systému.</span><span class="sxs-lookup"><span data-stu-id="8d1de-127">Review hello details and consider how operating system configurations are applied.</span></span>
  <span data-ttu-id="8d1de-128">![Popis pravidla pro neúspěšné hello][3]</span><span class="sxs-lookup"><span data-stu-id="8d1de-128">![Description for hello failed rule][3]</span></span>

  <span data-ttu-id="8d1de-129">Security Center používá společné konfigurace – výčet (CCE) tooassign jedinečné identifikátory pro konfigurační pravidla.</span><span class="sxs-lookup"><span data-stu-id="8d1de-129">Security Center uses Common Configuration Enumeration (CCE) tooassign unique identifiers for configuration rules.</span></span> <span data-ttu-id="8d1de-130">v tomto okně je k dispozici Hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="8d1de-130">hello following information is provided on this blade:</span></span>

  - <span data-ttu-id="8d1de-131">NÁZEV – Název pravidla</span><span class="sxs-lookup"><span data-stu-id="8d1de-131">NAME -- Name of rule</span></span>
  - <span data-ttu-id="8d1de-132">ZÁVAŽNOST – Hodnota závažnosti CCE kritická, důležité nebo upozornění</span><span class="sxs-lookup"><span data-stu-id="8d1de-132">SEVERITY -- CCE severity value of critical, important, or warning</span></span>
  - <span data-ttu-id="8d1de-133">CCIED – CCE jedinečný identifikátor pro pravidlo hello</span><span class="sxs-lookup"><span data-stu-id="8d1de-133">CCIED -- CCE unique identifier for hello rule</span></span>
  - <span data-ttu-id="8d1de-134">Popis – Popis pravidla</span><span class="sxs-lookup"><span data-stu-id="8d1de-134">DESCRIPTION -- Description of rule</span></span>
  - <span data-ttu-id="8d1de-135">Ohrožení zabezpečení – Vysvětlení ohrožení zabezpečení nebo riziko, pokud není použita pravidla</span><span class="sxs-lookup"><span data-stu-id="8d1de-135">VULNERABILITY -- Explanation of vulnerability or risk if rule is not applied</span></span>
  - <span data-ttu-id="8d1de-136">DOPAD – Dopad na chod firmy při použití pravidla</span><span class="sxs-lookup"><span data-stu-id="8d1de-136">IMPACT -- Business impact when rule is applied</span></span>
  - <span data-ttu-id="8d1de-137">OČEKÁVANÁ hodnota – Hodnota očekávané při Security Center analyzuje konfiguraci operačního systému virtuálního počítače podle pravidla hello</span><span class="sxs-lookup"><span data-stu-id="8d1de-137">EXPECTED VALUE -- Value expected when Security Center analyzes your VM OS configuration against hello rule</span></span>
  - <span data-ttu-id="8d1de-138">– PRAVIDLO pravidlo operace použije pomocí služby Security Center při analýze konfigurace operačního systému virtuálního počítače podle pravidla hello</span><span class="sxs-lookup"><span data-stu-id="8d1de-138">RULE OPERATION -- Rule operation used by Security Center during analysis of your VM OS configuration against hello rule</span></span>
  - <span data-ttu-id="8d1de-139">Skutečná hodnota – Hodnota vrácena po dokončení analýzy konfigurace operačního systému virtuálního počítače podle pravidla hello</span><span class="sxs-lookup"><span data-stu-id="8d1de-139">ACTUAL VALUE -- Value returned after analysis of your VM OS configuration against hello rule</span></span>
  - <span data-ttu-id="8d1de-140">Výsledek vyhodnocení –-výsledek analýzy: předání služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="8d1de-140">EVALUATION RESULT –- Result of analysis: Pass, Fail</span></span>

## <a name="see-also"></a><span data-ttu-id="8d1de-141">Viz také</span><span class="sxs-lookup"><span data-stu-id="8d1de-141">See also</span></span>
<span data-ttu-id="8d1de-142">Tento článek ukázal, jak tooimplement hello Security Center doporučení "napravit OS ohrožení zabezpečení."</span><span class="sxs-lookup"><span data-stu-id="8d1de-142">This article showed you how tooimplement hello Security Center recommendation "Remediate OS vulnerabilities."</span></span> <span data-ttu-id="8d1de-143">Můžete zkontrolovat hello sadu pravidel konfigurace [zde](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).</span><span class="sxs-lookup"><span data-stu-id="8d1de-143">You can review hello set of configuration rules [here](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).</span></span> <span data-ttu-id="8d1de-144">Security Center používá CCE (Common Configuration výčtu) tooassign jedinečné identifikátory pro konfigurační pravidla.</span><span class="sxs-lookup"><span data-stu-id="8d1de-144">Security Center uses CCE (Common Configuration Enumeration) tooassign unique identifiers for configuration rules.</span></span> <span data-ttu-id="8d1de-145">Navštivte hello [CCE](https://nvd.nist.gov/cce/index.cfm) lokality pro další informace.</span><span class="sxs-lookup"><span data-stu-id="8d1de-145">Visit hello [CCE](https://nvd.nist.gov/cce/index.cfm) site for more information.</span></span>

<span data-ttu-id="8d1de-146">toolearn Další informace o Security Center, najdete v části hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="8d1de-146">toolearn more about Security Center, see hello following resources:</span></span>

* <span data-ttu-id="8d1de-147">[Podporované platformy v Azure Security Center](security-center-os-coverage.md) -obsahuje seznam podporovaných Windows a virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="8d1de-147">[Supported platforms in Azure Security Center](security-center-os-coverage.md) - Provides a list of supported Windows and Linux VMs.</span></span>
* <span data-ttu-id="8d1de-148">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="8d1de-148">[Setting security policies in Azure Security Center](security-center-policies.md) - Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="8d1de-149">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="8d1de-149">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) - Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="8d1de-150">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="8d1de-150">[Security health monitoring in Azure Security Center](security-center-monitoring.md) - Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="8d1de-151">[Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.</span><span class="sxs-lookup"><span data-stu-id="8d1de-151">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) - Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="8d1de-152">[Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="8d1de-152">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) - Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="8d1de-153">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.</span><span class="sxs-lookup"><span data-stu-id="8d1de-153">[Azure Security Center FAQ](security-center-faq.md) - Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="8d1de-154">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="8d1de-154">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) - Find blog posts about Azure security and compliance.</span></span>

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/recommendation.png
[2]:./media/security-center-remediate-os-vulnerabilities/vm-remediate-os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
