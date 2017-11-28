---
title: "Oprava chyb zabezpečení operačního systému v Azure Security Center | Microsoft Docs"
description: "Tento dokument se dozvíte, jak provést doporučení Azure Security Center ** ohrožení zabezpečení operačního systému opravit **."
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
ms.openlocfilehash: e6b251d5b97c57b3b6f79d14e53fbed5ca37ecb0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="remediate-os-vulnerabilities-in-azure-security-center"></a><span data-ttu-id="b5fe0-103">Oprava chyb zabezpečení operačního systému v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="b5fe0-103">Remediate OS vulnerabilities in Azure Security Center</span></span>
<span data-ttu-id="b5fe0-104">Azure Security Center analyzuje denně virtuální počítač (VM) operačního systému (OS) pro konfigurace, které může zvýšit virtuálního počítače vůči útokům a doporučuje změny konfigurace, které tyto nedostatky.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-104">Azure Security Center analyzes daily your virtual machine (VM) operating system (OS) for configurations that could make the VM more vulnerable to attack and recommends configuration changes to address these vulnerabilities.</span></span> <span data-ttu-id="b5fe0-105">Security Center doporučí, řeší chyby zabezpečení, pokud konfigurace operačního systému Virtuálního počítače neodpovídá doporučenou konfiguraci pravidla.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-105">Security Center recommends that you resolve vulnerabilities when your VM’s OS configuration does not match the recommended configuration rules.</span></span>

> [!NOTE]
> <span data-ttu-id="b5fe0-106">Další informace o konkrétní konfigurace se sledují, najdete v článku [seznam doporučenou konfiguraci pravidel](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).</span><span class="sxs-lookup"><span data-stu-id="b5fe0-106">For more information on the specific configurations being monitored, see the [list of recommended configuration rules](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).</span></span>
>
>

## <a name="implement-the-recommendation"></a><span data-ttu-id="b5fe0-107">Implementace doporučení</span><span class="sxs-lookup"><span data-stu-id="b5fe0-107">Implement the recommendation</span></span>

> [!NOTE]
> <span data-ttu-id="b5fe0-108">Tento dokument vám tuto službu představí formou ukázkového nasazení.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-108">This document introduces the service by using an example deployment.</span></span>  <span data-ttu-id="b5fe0-109">Tento dokument není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-109">This document is not a step-by-step guide.</span></span>
>
>

1. <span data-ttu-id="b5fe0-110">V **doporučení** vyberte **ohrožení zabezpečení operačního systému opravit**.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-110">In the **Recommendations** blade, select **Remediate OS vulnerabilities**.</span></span>
   <span data-ttu-id="b5fe0-111">![Náprava ohrožení zabezpečení operačního systému][1]</span><span class="sxs-lookup"><span data-stu-id="b5fe0-111">![Remediate OS vulnerabilities][1]</span></span>

    <span data-ttu-id="b5fe0-112">**Ohrožení zabezpečení operačního systému opravit** okno k otevření a virtuální počítače s konfigurací operačního systému, které neodpovídají doporučenou konfiguraci pravidla.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-112">The **Remediate OS vulnerabilities** blade opens and lists your VMs with OS configurations that do not match the recommended configuration rules.</span></span>  <span data-ttu-id="b5fe0-113">Pro každý virtuální počítač v okně identifikuje:</span><span class="sxs-lookup"><span data-stu-id="b5fe0-113">For each VM, the blade identifies:</span></span>

   * <span data-ttu-id="b5fe0-114">**PRAVIDLA se nezdařilo** – počet pravidel, která konfigurace operačního systému Virtuálního počítače se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-114">**FAILED RULES** -- The number of rules that the VM's OS configuration failed.</span></span>
   * <span data-ttu-id="b5fe0-115">**ČAS poslední kontroly** – datum a čas, Security Center naposledy hledala konfigurace operačního systému Virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-115">**LAST SCAN TIME** -- The date and time that Security Center last scanned the VM’s OS configuration.</span></span>
   * <span data-ttu-id="b5fe0-116">**Stav** --aktuální stav chyby zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="b5fe0-116">**STATE** -- The current state of the vulnerability:</span></span>

     * <span data-ttu-id="b5fe0-117">Otevřete: Tuto chybu zabezpečení dosud nebylo řešeno.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-117">Open: The vulnerability has not been addressed yet</span></span>
     * <span data-ttu-id="b5fe0-118">V průběhu: Aktuálně se aplikuje ohrožení zabezpečení, není třeba žádné akce</span><span class="sxs-lookup"><span data-stu-id="b5fe0-118">In Progress: The vulnerability is currently being applied, no action is required by you</span></span>
     * <span data-ttu-id="b5fe0-119">Přeložit: Tuto chybu zabezpečení byl již řešit (Pokud problém vyřešen, položka je zobrazena šedě)</span><span class="sxs-lookup"><span data-stu-id="b5fe0-119">Resolved: The vulnerability was already addressed (when the issue has been resolved, the entry is grayed out)</span></span>
   * <span data-ttu-id="b5fe0-120">**ZÁVAŽNOST** – všechna ohrožení zabezpečení jsou nastaveny na závažnost nízká, což znamená, ohrožení zabezpečení, mělo by se řešit, ale nevyžaduje okamžitou pozornost.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-120">**SEVERITY** -- All vulnerabilities are set to a severity of Low, meaning a vulnerability should be addressed but does not require immediate attention.</span></span>

2. <span data-ttu-id="b5fe0-121">Vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-121">Select a VM.</span></span> <span data-ttu-id="b5fe0-122">Okno pro tento virtuální počítač se zobrazí pravidla, které selhaly.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-122">A blade for that VM opens and displays the rules that have failed.</span></span>
   <span data-ttu-id="b5fe0-123">![Pravidla konfigurace, které selhaly][2]</span><span class="sxs-lookup"><span data-stu-id="b5fe0-123">![Configuration rules that have failed][2]</span></span>

3. <span data-ttu-id="b5fe0-124">Vyberte pravidlo.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-124">Select a rule.</span></span> <span data-ttu-id="b5fe0-125">V tomto příkladu umožní vybrat **heslo musí splňovat požadavky na složitost**.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-125">In this example, lets select **Password must meet complexity requirements**.</span></span> <span data-ttu-id="b5fe0-126">Otevře se okno popisující neúspěšných pravidel a dopad.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-126">A blade opens describing the failed rule and the impact.</span></span> <span data-ttu-id="b5fe0-127">Zkontrolujte podrobnosti a zvažte, jak se používají konfigurace operačního systému.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-127">Review the details and consider how operating system configurations are applied.</span></span>
  <span data-ttu-id="b5fe0-128">![Popis pravidla se nezdařila][3]</span><span class="sxs-lookup"><span data-stu-id="b5fe0-128">![Description for the failed rule][3]</span></span>

  <span data-ttu-id="b5fe0-129">Security Center používá společné konfigurace – výčet (CCE) k přiřazení jedinečné identifikátory pro konfigurační pravidla.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-129">Security Center uses Common Configuration Enumeration (CCE) to assign unique identifiers for configuration rules.</span></span> <span data-ttu-id="b5fe0-130">V tomto okně se poskytuje následující informace:</span><span class="sxs-lookup"><span data-stu-id="b5fe0-130">The following information is provided on this blade:</span></span>

  - <span data-ttu-id="b5fe0-131">NÁZEV – Název pravidla</span><span class="sxs-lookup"><span data-stu-id="b5fe0-131">NAME -- Name of rule</span></span>
  - <span data-ttu-id="b5fe0-132">ZÁVAŽNOST – Hodnota závažnosti CCE kritická, důležité nebo upozornění</span><span class="sxs-lookup"><span data-stu-id="b5fe0-132">SEVERITY -- CCE severity value of critical, important, or warning</span></span>
  - <span data-ttu-id="b5fe0-133">CCIED – CCE jedinečný identifikátor pro pravidlo</span><span class="sxs-lookup"><span data-stu-id="b5fe0-133">CCIED -- CCE unique identifier for the rule</span></span>
  - <span data-ttu-id="b5fe0-134">Popis – Popis pravidla</span><span class="sxs-lookup"><span data-stu-id="b5fe0-134">DESCRIPTION -- Description of rule</span></span>
  - <span data-ttu-id="b5fe0-135">Ohrožení zabezpečení – Vysvětlení ohrožení zabezpečení nebo riziko, pokud není použita pravidla</span><span class="sxs-lookup"><span data-stu-id="b5fe0-135">VULNERABILITY -- Explanation of vulnerability or risk if rule is not applied</span></span>
  - <span data-ttu-id="b5fe0-136">DOPAD – Dopad na chod firmy při použití pravidla</span><span class="sxs-lookup"><span data-stu-id="b5fe0-136">IMPACT -- Business impact when rule is applied</span></span>
  - <span data-ttu-id="b5fe0-137">OČEKÁVANÁ hodnota – Hodnota očekávané při Security Center analyzuje konfiguraci operačního systému virtuálního počítače podle pravidla</span><span class="sxs-lookup"><span data-stu-id="b5fe0-137">EXPECTED VALUE -- Value expected when Security Center analyzes your VM OS configuration against the rule</span></span>
  - <span data-ttu-id="b5fe0-138">– PRAVIDLO pravidlo operace použije pomocí služby Security Center při analýze konfigurace operačního systému virtuálního počítače podle pravidla</span><span class="sxs-lookup"><span data-stu-id="b5fe0-138">RULE OPERATION -- Rule operation used by Security Center during analysis of your VM OS configuration against the rule</span></span>
  - <span data-ttu-id="b5fe0-139">Skutečná hodnota – Hodnota vrácena po dokončení analýzy konfigurace operačního systému virtuálního počítače podle pravidla</span><span class="sxs-lookup"><span data-stu-id="b5fe0-139">ACTUAL VALUE -- Value returned after analysis of your VM OS configuration against the rule</span></span>
  - <span data-ttu-id="b5fe0-140">Výsledek vyhodnocení –-výsledek analýzy: předání služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="b5fe0-140">EVALUATION RESULT –- Result of analysis: Pass, Fail</span></span>

## <a name="see-also"></a><span data-ttu-id="b5fe0-141">Viz také</span><span class="sxs-lookup"><span data-stu-id="b5fe0-141">See also</span></span>
<span data-ttu-id="b5fe0-142">Tento článek vám ukázal, jak provést doporučení Security Center "Napravit OS ohrožení zabezpečení."</span><span class="sxs-lookup"><span data-stu-id="b5fe0-142">This article showed you how to implement the Security Center recommendation "Remediate OS vulnerabilities."</span></span> <span data-ttu-id="b5fe0-143">Můžete zkontrolovat sadu pravidel, konfigurace [zde](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).</span><span class="sxs-lookup"><span data-stu-id="b5fe0-143">You can review the set of configuration rules [here](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).</span></span> <span data-ttu-id="b5fe0-144">Security Center používá CCE (Common Configuration výčtu) k přiřazení jedinečné identifikátory pro konfigurační pravidla.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-144">Security Center uses CCE (Common Configuration Enumeration) to assign unique identifiers for configuration rules.</span></span> <span data-ttu-id="b5fe0-145">Přejděte [CCE](https://nvd.nist.gov/cce/index.cfm) lokality pro další informace.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-145">Visit the [CCE](https://nvd.nist.gov/cce/index.cfm) site for more information.</span></span>

<span data-ttu-id="b5fe0-146">Další informace o službě Security Center, najdete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="b5fe0-146">To learn more about Security Center, see the following resources:</span></span>

* <span data-ttu-id="b5fe0-147">[Podporované platformy v Azure Security Center](security-center-os-coverage.md) -obsahuje seznam podporovaných Windows a virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-147">[Supported platforms in Azure Security Center](security-center-os-coverage.md) - Provides a list of supported Windows and Linux VMs.</span></span>
* <span data-ttu-id="b5fe0-148">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak nakonfigurovat zásady zabezpečení pro skupiny prostředků a předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-148">[Setting security policies in Azure Security Center](security-center-policies.md) - Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="b5fe0-149">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-149">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) - Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="b5fe0-150">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-150">[Security health monitoring in Azure Security Center](security-center-monitoring.md) - Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="b5fe0-151">[Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak spravovat a reakce na výstrahy zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-151">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) - Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="b5fe0-152">[Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak sledovat stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-152">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) - Learn how to monitor the health status of your partner solutions.</span></span>
* <span data-ttu-id="b5fe0-153">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití této služby.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-153">[Azure Security Center FAQ](security-center-faq.md) - Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="b5fe0-154">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-154">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) - Find blog posts about Azure security and compliance.</span></span>

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/recommendation.png
[2]:./media/security-center-remediate-os-vulnerabilities/vm-remediate-os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
