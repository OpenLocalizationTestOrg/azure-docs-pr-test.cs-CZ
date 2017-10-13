---
title: "aaaAzure Security Center a virtuální počítače Azure | Microsoft Docs"
description: "Tento dokument vám pomůže toounderstand jak Azure Security Center můžete zabezpečit můžete virtuální počítače Azure."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 5fe5a12c-5d25-430c-9d47-df9438b1d7c5
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/24/2017
ms.author: yurid
ms.openlocfilehash: d5e80e9341263a57f3100cb032a066f037e913a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-and-azure-virtual-machines"></a><span data-ttu-id="17a99-103">Azure Security Center a Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="17a99-103">Azure Security Center and Azure Virtual Machines</span></span>
<span data-ttu-id="17a99-104">[Azure Security Center](https://azure.microsoft.com/services/security-center/) pomáhá zabránit, zjistit a reagovat toothreats.</span><span class="sxs-lookup"><span data-stu-id="17a99-104">[Azure Security Center](https://azure.microsoft.com/services/security-center/) helps you prevent, detect, and respond toothreats.</span></span> <span data-ttu-id="17a99-105">Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných Azure, pomáhá zjišťovat hrozby, kterých byste si jinak nevšimli, a spolupracuje s řadou řešení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="17a99-105">It provides integrated security monitoring and policy management across your Azure subscriptions, helps detect threats that might otherwise go unnoticed, and works with a broad ecosystem of security solutions.</span></span>

<span data-ttu-id="17a99-106">Tento článek ukazuje, jak vám Security Center může pomoci zabezpečit službu Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="17a99-106">This article shows how Security Center can help you secure your Azure Virtual Machines (VM).</span></span>

## <a name="why-use-security-center"></a><span data-ttu-id="17a99-107">Proč používat Security Center?</span><span class="sxs-lookup"><span data-stu-id="17a99-107">Why use Security Center?</span></span>
<span data-ttu-id="17a99-108">Security Center vám pomůže chránit data virtuálních počítačů v Azure tím, že poskytuje vhled do nastavení zabezpečení vašich virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="17a99-108">Security Center helps you safeguard virtual machine data in Azure by providing visibility into your virtual machine’s security settings.</span></span> <span data-ttu-id="17a99-109">Security Center chrání virtuální počítače, nebudou mít k dispozici hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="17a99-109">When Security Center safeguards your VMs, hello following capabilities will be available:</span></span>

* <span data-ttu-id="17a99-110">Nastavení zabezpečení operačního systému (OS) s hello doporučená pravidla konfigurace</span><span class="sxs-lookup"><span data-stu-id="17a99-110">Operating System (OS) security settings with hello recommended configuration rules</span></span>
* <span data-ttu-id="17a99-111">Zabezpečení systému a chybějící kritické aktualizace</span><span class="sxs-lookup"><span data-stu-id="17a99-111">System security and critical updates that are missing</span></span>
* <span data-ttu-id="17a99-112">Doporučení ochrany koncových bodů</span><span class="sxs-lookup"><span data-stu-id="17a99-112">Endpoint protection recommendations</span></span>
* <span data-ttu-id="17a99-113">Ověření šifrování disku</span><span class="sxs-lookup"><span data-stu-id="17a99-113">Disk encryption validation</span></span>
* <span data-ttu-id="17a99-114">Posouzení a náprava ohrožení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="17a99-114">Vulnerability assessment and remediation</span></span>
* <span data-ttu-id="17a99-115">Detekce hrozeb</span><span class="sxs-lookup"><span data-stu-id="17a99-115">Threat detection</span></span>

<span data-ttu-id="17a99-116">Kromě toho toohelping chránit virtuální počítače Azure, Security Center nabízí také monitorování zabezpečení a správy pro cloudové služby, aplikační služby, virtuální sítě a další.</span><span class="sxs-lookup"><span data-stu-id="17a99-116">In addition toohelping protect your Azure VMs, Security Center also provides security monitoring and management for Cloud Services, App Services, Virtual Networks, and more.</span></span> 

> [!NOTE]
> <span data-ttu-id="17a99-117">V tématu [Úvod tooAzure Security Center](security-center-intro.md) toolearn Další informace o službě Azure Security Center.</span><span class="sxs-lookup"><span data-stu-id="17a99-117">See [Introduction tooAzure Security Center](security-center-intro.md) toolearn more about Azure Security Center.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="17a99-118">Požadavky</span><span class="sxs-lookup"><span data-stu-id="17a99-118">Prerequisites</span></span>
<span data-ttu-id="17a99-119">tooget začít s Azure Security Center, budete potřebovat tooknow a zvažte následující hello:</span><span class="sxs-lookup"><span data-stu-id="17a99-119">tooget started with Azure Security Center, you’ll need tooknow and consider hello following:</span></span>

* <span data-ttu-id="17a99-120">Musíte mít tooMicrosoft předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="17a99-120">You must have a subscription tooMicrosoft Azure.</span></span> <span data-ttu-id="17a99-121">V tématu [Ceny Security Center](https://azure.microsoft.com/pricing/details/security-center/) najdete další informace o úrovních Free a Standard služby Security Center.</span><span class="sxs-lookup"><span data-stu-id="17a99-121">See [Security Center Pricing](https://azure.microsoft.com/pricing/details/security-center/) for more information on Security Center’s free and standard tiers.</span></span>
* <span data-ttu-id="17a99-122">Plánování vašeho přijetí Security Center, najdete v části [Průvodce plánováním a operace Azure Security Center](security-center-planning-and-operations-guide.md) toolearn další aspekty plánování a operace.</span><span class="sxs-lookup"><span data-stu-id="17a99-122">Plan your Security Center adoption, see [Azure Security Center planning and operations guide](security-center-planning-and-operations-guide.md) toolearn more about planning and operations considerations.</span></span>
* <span data-ttu-id="17a99-123">Informace týkající se podpory operačních systémů najdete v tématu [Nejčastější dotazy k Azure Security Center](security-center-faq.md).</span><span class="sxs-lookup"><span data-stu-id="17a99-123">For information regarding operating system supportability, see [Azure Security Center frequently asked questions (FAQ)](security-center-faq.md).</span></span> 

## <a name="set-security-policy"></a><span data-ttu-id="17a99-124">Nastavení zásad zabezpečení</span><span class="sxs-lookup"><span data-stu-id="17a99-124">Set security policy</span></span>
<span data-ttu-id="17a99-125">Toobe potřeb kolekce dat povoleno, že Azure Security Center může shromažďovat hello informace, které potřebuje tooprovide doporučení a výstrahy, které jsou vytvořeny na základě hello zásad zabezpečení, které nakonfigurujete.</span><span class="sxs-lookup"><span data-stu-id="17a99-125">Data collection needs toobe enabled so that Azure Security Center can gather hello information it needs tooprovide recommendations and alerts that are generated based on hello security policy you configure.</span></span> <span data-ttu-id="17a99-126">Hello obrázek, můžete uvidíte, že **shromažďování dat** byla zapnuta **na**.</span><span class="sxs-lookup"><span data-stu-id="17a99-126">In hello figure below, you can see that **Data collection** has been turned **On**.</span></span>

<span data-ttu-id="17a99-127">Zásady zabezpečení definuje hello sadu ovládacích prvků, které se doporučují pro prostředky v rámci zadané předplatné nebo prostředek skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="17a99-127">A security policy defines hello set of controls which are recommended for resources within hello specified subscription or resource group.</span></span> <span data-ttu-id="17a99-128">Než povolíte zásady zabezpečení, musí mít povolené shromažďování dat, Security Center shromáždí data z virtuálních počítačů v pořadí tooassess jejich stavu zabezpečení zadejte doporučení zabezpečení a výstrahy toothreats.</span><span class="sxs-lookup"><span data-stu-id="17a99-128">Before enabling security policy, you must have data collection enabled, Security Center collects data from your virtual machines in order tooassess their security state, provide security recommendations, and alert you toothreats.</span></span> <span data-ttu-id="17a99-129">V Security Center určíte zásady pro vaše předplatná Azure nebo skupiny prostředků podle potřeb zabezpečení tooyour společnosti a hello typu aplikací nebo citlivosti dat hello v každém předplatném.</span><span class="sxs-lookup"><span data-stu-id="17a99-129">In Security Center, you define policies for your Azure subscriptions or resource groups according tooyour company’s security needs and hello type of applications or sensitivity of hello data in each subscription.</span></span> 

![Zásady zabezpečení](./media/security-center-virtual-machine/security-center-virtual-machine-fig1.png)

> [!NOTE]
> <span data-ttu-id="17a99-131">více o jednotlivých toolearn **zásada Zabránění** k dispozici, najdete v tématu [nastavovat zásady zabezpečení](security-center-policies.md) článku.</span><span class="sxs-lookup"><span data-stu-id="17a99-131">toolearn more about each **Prevention policy** available, see [Set security policies](security-center-policies.md) article.</span></span>
> 
> 

## <a name="manage-security-recommendations"></a><span data-ttu-id="17a99-132">Správa doporučení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="17a99-132">Manage security recommendations</span></span>
<span data-ttu-id="17a99-133">Security Center analyzuje stav zabezpečení hello vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="17a99-133">Security Center analyzes hello security state of your Azure resources.</span></span> <span data-ttu-id="17a99-134">Když Security Center identifikuje potenciální ohrožení zabezpečení, vytvoří doporučení.</span><span class="sxs-lookup"><span data-stu-id="17a99-134">When Security Center identifies potential security vulnerabilities, it creates recommendations.</span></span> <span data-ttu-id="17a99-135">Hello doporučení vás provede procesem hello konfigurace hello potřebné ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="17a99-135">hello recommendations guide you through hello process of configuring hello needed controls.</span></span>

<span data-ttu-id="17a99-136">Po nastavení zásad zabezpečení, Security Center analyzuje stav zabezpečení hello vaše prostředky tooidentify potenciální ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="17a99-136">After setting a security policy, Security Center analyzes hello security state of your resources tooidentify potential vulnerabilities.</span></span> <span data-ttu-id="17a99-137">Hello doporučení se zobrazí ve formátu tabulky, kde každý řádek představuje jeden konkrétní doporučení.</span><span class="sxs-lookup"><span data-stu-id="17a99-137">hello recommendations are shown in a table format where each line represents one particular recommendation.</span></span> <span data-ttu-id="17a99-138">Hello následující tabulka obsahuje některé příklady doporučení pro virtuální počítače Azure a co každé z nich bude dělat, když ho použijete.</span><span class="sxs-lookup"><span data-stu-id="17a99-138">hello table below provides some examples of recommendations for Azure VMs and what each one will do if you apply it.</span></span> <span data-ttu-id="17a99-139">Když vyberete doporučení, bude třeba zadat informace, které ukazuje, jak tooimplement hello doporučení ve službě Security Center.</span><span class="sxs-lookup"><span data-stu-id="17a99-139">When you select a recommendation, you will be provided information that shows you how tooimplement hello recommendation in Security Center.</span></span>

| <span data-ttu-id="17a99-140">Doporučení</span><span class="sxs-lookup"><span data-stu-id="17a99-140">Recommendation</span></span> | <span data-ttu-id="17a99-141">Popis</span><span class="sxs-lookup"><span data-stu-id="17a99-141">Description</span></span> |
| --- | --- |
| [<span data-ttu-id="17a99-142">Povolení shromažďování dat pro předplatná</span><span class="sxs-lookup"><span data-stu-id="17a99-142">Enable data collection for subscriptions</span></span>](security-center-enable-data-collection.md) |<span data-ttu-id="17a99-143">Doporučuje se v rámci vašich předplatných shromažďování dat v zásadách zabezpečení hello pro každé z vašich předplatných a všechny virtuální počítače (VM) zapnout.</span><span class="sxs-lookup"><span data-stu-id="17a99-143">Recommends that you turn on data collection in hello security policy for each of your subscriptions and all virtual machines (VMs) in your subscriptions.</span></span> |
| [<span data-ttu-id="17a99-144">Náprava ohrožení zabezpečení operačního systému</span><span class="sxs-lookup"><span data-stu-id="17a99-144">Remediate OS vulnerabilities</span></span>](security-center-remediate-os-vulnerabilities.md) |<span data-ttu-id="17a99-145">Doporučuje zarovnané vaše konfigurace operačního systému s hello doporučená pravidla konfigurace, například neumožňují toobe hesla uložit.</span><span class="sxs-lookup"><span data-stu-id="17a99-145">Recommends that you align your OS configurations with hello recommended configuration rules, e.g. do not allow passwords toobe saved.</span></span> |
| [<span data-ttu-id="17a99-146">Instalace aktualizací systému</span><span class="sxs-lookup"><span data-stu-id="17a99-146">Apply system updates</span></span>](security-center-apply-system-updates.md) |<span data-ttu-id="17a99-147">Doporučuje se nasadit chybějící zabezpečení systému a tooVMs důležité aktualizace.</span><span class="sxs-lookup"><span data-stu-id="17a99-147">Recommends that you deploy missing system security and critical updates tooVMs.</span></span> |
| [<span data-ttu-id="17a99-148">Restartování po aktualizacích systému</span><span class="sxs-lookup"><span data-stu-id="17a99-148">Reboot after system updates</span></span>](security-center-apply-system-updates.md#reboot-after-system-updates) |<span data-ttu-id="17a99-149">Doporučuje se, že restartujete proces virtuálních počítačů toocomplete hello použití aktualizací systému.</span><span class="sxs-lookup"><span data-stu-id="17a99-149">Recommends that you reboot a VM toocomplete hello process of applying system updates.</span></span> |
| [<span data-ttu-id="17a99-150">Instalace Endpoint Protection</span><span class="sxs-lookup"><span data-stu-id="17a99-150">Install Endpoint Protection</span></span>](security-center-install-endpoint-protection.md) |<span data-ttu-id="17a99-151">Doporučuje zřízení tooVMs antimalwarových programů (jenom Windows VM).</span><span class="sxs-lookup"><span data-stu-id="17a99-151">Recommends that you provision antimalware programs tooVMs (Windows VMs only).</span></span> |
| [<span data-ttu-id="17a99-152">Vyřešení výstrah stavu služby Endpoint Protection</span><span class="sxs-lookup"><span data-stu-id="17a99-152">Resolve Endpoint Protection health alerts</span></span>](security-center-resolve-endpoint-protection-health-alerts.md) |<span data-ttu-id="17a99-153">Doporučuje, abyste vyřešili selhání ochrany koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="17a99-153">Recommends that you resolve endpoint protection failures.</span></span> |
| [<span data-ttu-id="17a99-154">Povolení agenta virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="17a99-154">Enable VM Agent</span></span>](security-center-enable-vm-agent.md) |<span data-ttu-id="17a99-155">Umožňuje vám toosee které virtuální počítače vyžadují hello agenta virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="17a99-155">Enables you toosee which VMs require hello VM Agent.</span></span> <span data-ttu-id="17a99-156">Hello agenta virtuálního počítače musí být nainstalován na virtuálních počítačích v pořadí tooprovision oprava kontrolu, kontrolu směrného plánu a antimalwarových programů.</span><span class="sxs-lookup"><span data-stu-id="17a99-156">hello VM Agent must be installed on VMs in order tooprovision patch scanning, baseline scanning, and antimalware programs.</span></span> <span data-ttu-id="17a99-157">Hello agenta virtuálního počítače je nainstalována ve výchozím nastavení pro virtuální počítače, které byly nasazeny pomocí hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="17a99-157">hello VM Agent is installed by default for VMs that are deployed from hello Azure Marketplace.</span></span> <span data-ttu-id="17a99-158">článek Hello [agenta virtuálního počítače a rozšíření – část 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) poskytuje informace o tom, jak tooinstall hello agenta virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="17a99-158">hello article [VM Agent and Extensions – Part 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) provides information on how tooinstall hello VM Agent.</span></span> |
| [<span data-ttu-id="17a99-159">Použití šifrování disku</span><span class="sxs-lookup"><span data-stu-id="17a99-159">Apply disk encryption</span></span>](security-center-apply-disk-encryption.md) |<span data-ttu-id="17a99-160">Doporučuje, abyste disky svých virtuálních počítačů zašifrovali pomocí služby Azure Disk Encryption (platí pro virtuální počítače s Windows a Linuxem).</span><span class="sxs-lookup"><span data-stu-id="17a99-160">Recommends that you encrypt your VM disks using Azure Disk Encryption (Windows and Linux VMs).</span></span> <span data-ttu-id="17a99-161">Šifrování se doporučuje pro hello operačního systému a datové svazky na vašem virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="17a99-161">Encryption is recommended for both hello OS and data volumes on your VM.</span></span> |
| [<span data-ttu-id="17a99-162">Není nainstalováno posouzení ohrožení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="17a99-162">Vulnerability assessment not installed</span></span>](security-center-vulnerability-assessment-recommendations.md) |<span data-ttu-id="17a99-163">Doporučuje, abyste na vašem virtuálním počítači nainstalovali řešení posouzení ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="17a99-163">Recommends that you install a vulnerability assessment solution on your VM.</span></span> |
| [<span data-ttu-id="17a99-164">Náprava ohrožení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="17a99-164">Remediate vulnerabilities</span></span>](security-center-vulnerability-assessment-recommendations.md#review-the-recommendation) |<span data-ttu-id="17a99-165">Umožní vám toosee systému a aplikací chyb zabezpečení detekovaných službou nainstalovaná na vašem virtuálním počítači řešení pro vyhodnocení hello ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="17a99-165">Enables you toosee system and application vulnerabilities detected by hello vulnerability assessment solution installed on your VM.</span></span> |

> [!NOTE]
> <span data-ttu-id="17a99-166">toolearn Další informace o doporučení, najdete v části [Správa doporučení zabezpečení](security-center-recommendations.md) článku.</span><span class="sxs-lookup"><span data-stu-id="17a99-166">toolearn more about recommendations, see [Managing security recommendations](security-center-recommendations.md) article.</span></span>
> 
> 

## <a name="monitor-security-health"></a><span data-ttu-id="17a99-167">Monitorování stavu zabezpečení</span><span class="sxs-lookup"><span data-stu-id="17a99-167">Monitor security health</span></span>
<span data-ttu-id="17a99-168">Po povolení [zásady zabezpečení](security-center-policies.md) pro prostředky předplatného bude Security Center analyzovat zabezpečení hello vaše prostředky tooidentify potenciální ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="17a99-168">After you enable [security policies](security-center-policies.md) for a subscription’s resources, Security Center will analyze hello security of your resources tooidentify potential vulnerabilities.</span></span>  <span data-ttu-id="17a99-169">Můžete zobrazit stav zabezpečení vašich prostředků, spolu s případnými problémy v hello hello **stav zabezpečení prostředků** okno.</span><span class="sxs-lookup"><span data-stu-id="17a99-169">You can view hello security state of your resources, along with any issues in hello **Resource security health** blade.</span></span> <span data-ttu-id="17a99-170">Když kliknete na tlačítko **virtuální počítače** v hello **zabezpečení prostředků** dlaždice stavu, hello **virtuální počítače** otevře se okno s doporučeními pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="17a99-170">When you click **Virtual machines** in hello **Resource security** health tile, hello **Virtual machines** blade will open with recommendations for your VMs.</span></span> 

![Stav zabezpečení](./media/security-center-virtual-machine/security-center-virtual-machine-fig2.png)

## <a name="manage-and-respond-toosecurity-alerts"></a><span data-ttu-id="17a99-172">Spravovat a reagovat toosecurity výstrahy</span><span class="sxs-lookup"><span data-stu-id="17a99-172">Manage and respond toosecurity alerts</span></span>
<span data-ttu-id="17a99-173">Security Center automaticky shromažďuje, analyzuje a integruje data protokolu z vaše prostředky Azure, sítě hello a připojených partnerských řešení (jako jsou brány firewall a endpoint protection řešení), toodetect skutečné hrozby a snížil počet falešných poplachů.</span><span class="sxs-lookup"><span data-stu-id="17a99-173">Security Center automatically collects, analyzes, and integrates log data from your Azure resources, hello network, and connected partner solutions (like firewall and endpoint protection solutions), toodetect real threats and reduce false positives.</span></span> <span data-ttu-id="17a99-174">S využitím různých agregace [možností detekce](security-center-detection-capabilities.md), Security Center je možné toogenerate nastavovat zabezpečení výstrahy toohelp rychle hello problému a poskytovat doporučení, jak tooremediate možných útoků.</span><span class="sxs-lookup"><span data-stu-id="17a99-174">By leveraging a diverse aggregation of [detection capabilities](security-center-detection-capabilities.md), Security Center is able toogenerate prioritized security alerts toohelp you quickly investigate hello problem and provide recommendations for how tooremediate possible attacks.</span></span>

![Výstrahy zabezpečení](./media/security-center-virtual-machine/security-center-virtual-machine-fig3.png)

<span data-ttu-id="17a99-176">Vyberte toolearn výstrahy zabezpečení informace o hello událostí, který aktivoval výstrahu hello a co, pokud existuje, kroky je nutné tootake tooremediate útoku.</span><span class="sxs-lookup"><span data-stu-id="17a99-176">Select a security alert toolearn more about hello event(s) that triggered hello alert and what, if any, steps you need tootake tooremediate an attack.</span></span> <span data-ttu-id="17a99-177">Výstrahy zabezpečení jsou seskupené podle [typu](security-center-alerts-type.md) a data.</span><span class="sxs-lookup"><span data-stu-id="17a99-177">Security alerts are grouped by [type](security-center-alerts-type.md) and date.</span></span>

## <a name="see-also"></a><span data-ttu-id="17a99-178">Viz také</span><span class="sxs-lookup"><span data-stu-id="17a99-178">See also</span></span>
<span data-ttu-id="17a99-179">toolearn Další informace o Security Center, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="17a99-179">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="17a99-180">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="17a99-180">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="17a99-181">[Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.</span><span class="sxs-lookup"><span data-stu-id="17a99-181">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="17a99-182">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.</span><span class="sxs-lookup"><span data-stu-id="17a99-182">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
