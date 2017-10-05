---
title: "Ochrana osobních dat pomocí Azure Security Center | Microsoft Docs"
description: "Ochrana osobních dat pomocí Azure security center"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 3a941389713a4d3dbffbbfe8a717409927d85c6d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="protect-personal-data-from-breaches-and-attacks-azure-security-center"></a><span data-ttu-id="52070-103">Ochrana osobních údajů z narušení a útoky: Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="52070-103">Protect personal data from breaches and attacks: Azure Security Center</span></span>

<span data-ttu-id="52070-104">Tento článek vám pomůže pochopit, jak používat Azure Security Center k ochraně osobních údajů z narušení a útoky.</span><span class="sxs-lookup"><span data-stu-id="52070-104">This article will help you understand how to use Azure Security Center to protect personal data from breaches and attacks.</span></span>

## <a name="scenario"></a><span data-ttu-id="52070-105">Scénář</span><span class="sxs-lookup"><span data-stu-id="52070-105">Scenario</span></span> 

<span data-ttu-id="52070-106">Velké výletních společnosti, centrálou ve Spojených státech amerických, je rozšířit jeho operací a nabídnout itineráře v Středomoří a baltský moři, jakož i Britské ostrovy.</span><span class="sxs-lookup"><span data-stu-id="52070-106">A large cruise company, headquartered in the United States, is expanding its operations to offer itineraries in the Mediterranean, and Baltic seas, as well as the British Isles.</span></span> <span data-ttu-id="52070-107">Abyste v tomto úsilí získala menší výletních Víceřádkový na základě v Itálii, Německo, Dánsko a Spojené království</span><span class="sxs-lookup"><span data-stu-id="52070-107">To help in those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark, and the U.K.</span></span>

<span data-ttu-id="52070-108">Společnost používá Microsoft Azure k ukládání firemních dat v cloudu.</span><span class="sxs-lookup"><span data-stu-id="52070-108">The company uses Microsoft Azure to store corporate data in the cloud.</span></span> <span data-ttu-id="52070-109">To zahrnuje osobní identifikovatelné údaje, jako je například jména, adresy, telefonních čísel a informace o kreditní kartě.</span><span class="sxs-lookup"><span data-stu-id="52070-109">This includes personal identifiable information such as names, addresses, phone numbers, and credit card information.</span></span> <span data-ttu-id="52070-110">Také obsahuje informace o lidských zdrojů, jako:</span><span class="sxs-lookup"><span data-stu-id="52070-110">It also includes Human Resources information such as:</span></span>

- <span data-ttu-id="52070-111">Adresy</span><span class="sxs-lookup"><span data-stu-id="52070-111">Addresses</span></span>
- <span data-ttu-id="52070-112">telefonní čísla</span><span class="sxs-lookup"><span data-stu-id="52070-112">Phone numbers</span></span>
- <span data-ttu-id="52070-113">daň identifikační čísla</span><span class="sxs-lookup"><span data-stu-id="52070-113">Tax identification numbers</span></span>
- <span data-ttu-id="52070-114">lékařské informace</span><span class="sxs-lookup"><span data-stu-id="52070-114">Medical information</span></span>

<span data-ttu-id="52070-115">Na řádku výletních také udržuje velké databáze potřebu a věrnost program členů.</span><span class="sxs-lookup"><span data-stu-id="52070-115">The cruise line also maintains a large database of reward and loyalty program members.</span></span> <span data-ttu-id="52070-116">Zaměstnanci společnosti přístup k síti ze vzdálených pobočkách společnosti a agenty cesta umístěné po celém světě mají přístup k některým prostředkům společnosti.</span><span class="sxs-lookup"><span data-stu-id="52070-116">Corporate employees access the network from the company’s remote offices and travel agents located around the world have access to some company resources.</span></span>
<span data-ttu-id="52070-117">Osobní data se přenáší po síti mezi tyto umístění a datovém centru společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="52070-117">Personal data travels across the network between these locations and the Microsoft data center.</span></span>

## <a name="problem-statement"></a><span data-ttu-id="52070-118">Popis problému</span><span class="sxs-lookup"><span data-stu-id="52070-118">Problem statement</span></span>

<span data-ttu-id="52070-119">Společnost se zajímá riziko útoků na svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="52070-119">The company is concerned about the threat of attacks on their Azure resources.</span></span> <span data-ttu-id="52070-120">Chtějí, aby se zabránilo ohrožení zaměstnanců a zákazníků osobních údajů na neoprávněné osoby.</span><span class="sxs-lookup"><span data-stu-id="52070-120">They want to prevent exposure of customers’ and employees’ personal data to unauthorized persons.</span></span> <span data-ttu-id="52070-121">Chtějí pokyny k zabránění a odpovědi nebo nápravná, jak efektivní způsob, jak sledovat probíhající zabezpečení cloudových prostředků.</span><span class="sxs-lookup"><span data-stu-id="52070-121">They want guidance on both prevention and response/remediation, as well as an effective way to monitor the ongoing security of their cloud resources.</span></span>
<span data-ttu-id="52070-122">Potřebují silné linií obrany před dnešní sofistikovaná a uspořádány útočníkům.</span><span class="sxs-lookup"><span data-stu-id="52070-122">They need a strong line of defense against today’s sophisticated and organized attackers.</span></span>

## <a name="company-goal"></a><span data-ttu-id="52070-123">Cílem společnosti</span><span class="sxs-lookup"><span data-stu-id="52070-123">Company goal</span></span>

<span data-ttu-id="52070-124">Jedním z cílů společnosti je k zajištění ochrany osobních údajů zaměstnanců a zákazníků osobních údajů a chrání před hrozbami.</span><span class="sxs-lookup"><span data-stu-id="52070-124">One of the company’s goals is to ensure the privacy of customers’ and employees’ personal data by protecting it from threats.</span></span> <span data-ttu-id="52070-125">Jeden z jejich cílů je okamžitě reagovat na známky porušení zabezpečení, aby se zmírnil dopad.</span><span class="sxs-lookup"><span data-stu-id="52070-125">One of their goals is to respond immediately to signs of breach to mitigate the impact.</span></span> <span data-ttu-id="52070-126">Způsob, jak vyhodnotit aktuální stav zabezpečení, identifikovat citlivé konfigurace a opravit, vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="52070-126">It requires a way to assess the current state of security, identify vulnerable configurations, and remediate them.</span></span>

## <a name="solutions"></a><span data-ttu-id="52070-127">Řešení</span><span class="sxs-lookup"><span data-stu-id="52070-127">Solutions</span></span>

<span data-ttu-id="52070-128">Microsoft Azure Security Center (ASC) poskytuje do řešení pro správu zásad a monitorování integrované zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="52070-128">Microsoft Azure Security Center (ASC) provides an integrated security monitoring and policy management solution.</span></span> <span data-ttu-id="52070-129">Zajišťuje threat snadno použitelné a efektivní funkce prevence, zjišťování a odpovědi.</span><span class="sxs-lookup"><span data-stu-id="52070-129">It delivers easy-to-use and effective threat prevention, detection, and response capabilities.</span></span>

### <a name="prevention"></a><span data-ttu-id="52070-130">Prevention (Prevence)</span><span class="sxs-lookup"><span data-stu-id="52070-130">Prevention</span></span>

<span data-ttu-id="52070-131">ASC pomáhá zabránit narušení tím, že vám nastavit zásady zabezpečení, poskytují přístup za běhu a implementovat doporučení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="52070-131">ASC helps you prevent breaches by enabling you to set security policies, provide just-in-time access, and implement security recommendations.</span></span>

<span data-ttu-id="52070-132">Zásady zabezpečení definuje sadu ovládacích prvků, které jsou doporučené pro prostředky v rámci předplatného.</span><span class="sxs-lookup"><span data-stu-id="52070-132">A security policy defines the set of controls recommended for resources within the specified subscription.</span></span> <span data-ttu-id="52070-133">Zamknout příchozí přenosy na virtuální počítače Azure, snižuje riziko napadení lze použít pouze ve čas přístupu.</span><span class="sxs-lookup"><span data-stu-id="52070-133">Just in time access can be used to lock down inbound traffic to your Azure VMs, reducing exposure to attacks.</span></span> <span data-ttu-id="52070-134">Doporučení zabezpečení jsou vytvořené pomocí ASC po analýze stav zabezpečení vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="52070-134">Security recommendations are created by ASC after analyzing the security state of your Azure resources.</span></span>

#### <a name="how-do-i-set-security-policies-in-asc"></a><span data-ttu-id="52070-135">Jak nastavit zásady zabezpečení v ASC?</span><span class="sxs-lookup"><span data-stu-id="52070-135">How do I set security policies in ASC?</span></span>

<span data-ttu-id="52070-136">Zásady zabezpečení můžete nakonfigurovat pro každé předplatné.</span><span class="sxs-lookup"><span data-stu-id="52070-136">You can configure security policies for each subscription.</span></span> <span data-ttu-id="52070-137">Pokud chcete určitou zásadu zabezpečení upravit, musíte mít roli vlastníka nebo přispěvatele daného předplatného.</span><span class="sxs-lookup"><span data-stu-id="52070-137">To modify a security policy, you must be an owner or contributor of that subscription.</span></span> <span data-ttu-id="52070-138">Na portálu Azure postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="52070-138">In the Azure portal, do the following:</span></span>

1. <span data-ttu-id="52070-139">Vyberte **zásad** v řídicím panelu ASC.</span><span class="sxs-lookup"><span data-stu-id="52070-139">Select **Policy** in the ASC dashboard.</span></span>

2. <span data-ttu-id="52070-140">Vyberte předplatné, na kterém chcete povolit zásady.</span><span class="sxs-lookup"><span data-stu-id="52070-140">Select the subscription on which you want to enable the policy.</span></span>

3. <span data-ttu-id="52070-141">Zvolte **zásada Zabránění** ke konfiguraci zásad podle předplatného.</span><span class="sxs-lookup"><span data-stu-id="52070-141">Choose **Prevention policy** to configure policies per subscription.</span></span> <span data-ttu-id="52070-142">**Shromažďování dat z virtuálních počítačů** musí být nastavena na **na.**</span><span class="sxs-lookup"><span data-stu-id="52070-142">**Collect data from virtual machines** should be set to **On.**</span></span>

4. <span data-ttu-id="52070-143">V **zásada Zabránění** možnosti, vyberte **na** povolit doporučení zabezpečení, které jsou relevantní pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="52070-143">In the **Prevention policy** options, select **On** to enable the security recommendations that are relevant for the subscription.</span></span>

![](media/protect-personal-data-azure-security-center/prevention-policy.png)

<span data-ttu-id="52070-144">Podrobné pokyny a vysvětlení jednotlivých zásad doporučení, které je možné povolit, najdete v části [nastavení zásad zabezpečení v Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-policies#set-security-policies).</span><span class="sxs-lookup"><span data-stu-id="52070-144">For more detailed instructions and an explanation of each of the policy recommendations that can be enabled, see [Set security policies in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-policies#set-security-policies).</span></span>

#### <a name="how-do-i-configure-just-in-time-access-jit"></a><span data-ttu-id="52070-145">Jak lze nakonfigurovat pouze v přístup čas (JIT)?</span><span class="sxs-lookup"><span data-stu-id="52070-145">How do I configure Just in Time Access (JIT)?</span></span>

<span data-ttu-id="52070-146">Pokud je povoleno JIT, Security Center zamyká příchozí přenosy na virtuální počítače Azure pomocí vytvoření pravidla NSG.</span><span class="sxs-lookup"><span data-stu-id="52070-146">When JIT is enabled, Security Center locks down inbound traffic to your Azure VMs by creating an NSG rule.</span></span> <span data-ttu-id="52070-147">Můžete vybrat porty na virtuálním počítači, do které se uzamkne příchozí provoz směrem dolů.</span><span class="sxs-lookup"><span data-stu-id="52070-147">You select the ports on the VM to which inbound traffic will be locked down.</span></span> <span data-ttu-id="52070-148">Pokud chcete používat JIT přístup, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="52070-148">To use JIT access, do the following:</span></span>

1. <span data-ttu-id="52070-149">Vyberte **těsně čas virtuálních počítačů přístup k dlaždici** v okně ASC.</span><span class="sxs-lookup"><span data-stu-id="52070-149">Select the **Just in time VM access tile** on the ASC blade.</span></span>

2. <span data-ttu-id="52070-150">Vyberte **doporučeno** kartě.</span><span class="sxs-lookup"><span data-stu-id="52070-150">Select the **Recommended** tab.</span></span>

3. <span data-ttu-id="52070-151">V části **virtuální počítače**, vyberte virtuální počítače, které chcete povolit.</span><span class="sxs-lookup"><span data-stu-id="52070-151">Under **VMs**, select the VMs that you want to enable.</span></span> <span data-ttu-id="52070-152">To převádí zatržení vedle virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="52070-152">This puts a checkmark next to a VM.</span></span> 
4. <span data-ttu-id="52070-153">Vyberte **povolení JIT** na virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="52070-153">Select **Enable JIT** on VMs.</span></span>
5. <span data-ttu-id="52070-154">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="52070-154">Select **Save**.</span></span>

<span data-ttu-id="52070-155">Potom můžete zobrazit výchozí porty, které doporučuje ASC povolený pro JIT.</span><span class="sxs-lookup"><span data-stu-id="52070-155">Then you can see the default ports that ASC recommends being enabled for JIT.</span></span> <span data-ttu-id="52070-156">Můžete také přidávat a konfigurovat nový port, na kterém chcete povolit právě v době řešení.</span><span class="sxs-lookup"><span data-stu-id="52070-156">You can also add and configure a new port on which you want to enable the just in time solution.</span></span> <span data-ttu-id="52070-157">**Těsně v čas virtuálních počítačů přístup** dlaždice v Centru zabezpečení zobrazuje počet virtuálních počítačů nakonfigurovaná pro přístup za běhu.</span><span class="sxs-lookup"><span data-stu-id="52070-157">The **Just in time VM access** tile in the Security Center shows the number of VMs configured for JIT access.</span></span> <span data-ttu-id="52070-158">Také ukazuje počet požadavků schválené přístup provedených během posledního týdne.</span><span class="sxs-lookup"><span data-stu-id="52070-158">It also shows the number of approved access requests made in the last week.</span></span>

![](media/protect-personal-data-azure-security-center/jit-vm.png)

<span data-ttu-id="52070-159">Pokyny o tom, jak to provést a další informace o těsně v čas přístup, zobrazit [spravovat přístup k virtuálním počítačům pomocí právě v čase.](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)</span><span class="sxs-lookup"><span data-stu-id="52070-159">For instructions on how to do this, and additional information about Just in Time access, see [Manage virtual machine access using just in time.](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)</span></span>

#### <a name="how-do-i-implement-asc-security-recommendations"></a><span data-ttu-id="52070-160">Jak implementace doporučení zabezpečení ASC?</span><span class="sxs-lookup"><span data-stu-id="52070-160">How do I implement ASC security recommendations?</span></span>

<span data-ttu-id="52070-161">Když Security Center identifikuje potenciální ohrožení zabezpečení, vytvoří doporučení.</span><span class="sxs-lookup"><span data-stu-id="52070-161">When Security Center identifies potential security vulnerabilities, it creates recommendations.</span></span> <span data-ttu-id="52070-162">Doporučení vás provedou procesem konfigurace potřebných kontrol.</span><span class="sxs-lookup"><span data-stu-id="52070-162">The recommendations guide you through the process of configuring the needed controls.</span></span> 
1. <span data-ttu-id="52070-163">Vyberte **doporučení** dlaždici na řídicím panelu ASC.</span><span class="sxs-lookup"><span data-stu-id="52070-163">Select the **Recommendations** tile on the ASC dashboard.</span></span>

2. <span data-ttu-id="52070-164">Zobrazte doporučení, které jsou uvedeny ve formátu tabulky, kde každý řádek představuje jeden doporučení.</span><span class="sxs-lookup"><span data-stu-id="52070-164">View the recommendations, which are shown in a table format where each line represents one recommendation.</span></span>

3. <span data-ttu-id="52070-165">K filtru doporučení, vyberte **filtru** a vyberte stav a závažnost hodnoty, které chcete vidět.</span><span class="sxs-lookup"><span data-stu-id="52070-165">To filter recommendations, select **Filter** and select the severity and state values you wish to see.</span></span>

4. <span data-ttu-id="52070-166">Pro zavření doporučení, která se nedá použít, klikněte pravým tlačítkem a vyberte **přeskočení.**</span><span class="sxs-lookup"><span data-stu-id="52070-166">To dismiss a recommendation that is not applicable, you can right click and select **Dismiss.**</span></span>

5. <span data-ttu-id="52070-167">Vyhodnoťte, které doporučení by měla být použije první.</span><span class="sxs-lookup"><span data-stu-id="52070-167">Evaluate which recommendation should be applied first.</span></span>

6. <span data-ttu-id="52070-168">Použijte doporučení v pořadí podle priority.</span><span class="sxs-lookup"><span data-stu-id="52070-168">Apply the recommendations in order of priority.</span></span>

<span data-ttu-id="52070-169">Seznam možných doporučení a návodů na tom, jak používat každý, naleznete v části [Správa doporučení zabezpečení v Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-recommendations)</span><span class="sxs-lookup"><span data-stu-id="52070-169">For a list of possible recommendations and walk-throughs on how to apply each, see [Managing security recommendations in Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-recommendations)</span></span>

### <a name="detection-and-response"></a><span data-ttu-id="52070-170">Detekce a reakce</span><span class="sxs-lookup"><span data-stu-id="52070-170">Detection and Response</span></span>

<span data-ttu-id="52070-171">Detekce a reakce přejděte společně, jak byste chtěli reagovat, co nejrychleji zjištěno ohrožení.</span><span class="sxs-lookup"><span data-stu-id="52070-171">Detection and response go together, as you want to respond as quickly as possible after a threat is detected.</span></span>
<span data-ttu-id="52070-172">Detekce hrozeb ASC funguje tak, že automaticky shromažďování informací o zabezpečení z vašich prostředků Azure, sítě a připojených partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="52070-172">ASC threat detection works by automatically collecting security information from your Azure resources, the network, and connected partner solutions.</span></span> <span data-ttu-id="52070-173">ASC můžete rychle aktualizovat své detekční algoritmy, protože útočníci vydání nové a stále sofistikované zneužití.</span><span class="sxs-lookup"><span data-stu-id="52070-173">ASC can rapidly update its detection algorithms as attackers release new and increasingly sophisticated exploits.</span></span> <span data-ttu-id="52070-174">Podrobnější informace o tom, jak funguje služba detekce hrozeb je ASC, najdete v části [funkce zjišťování služby Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities).</span><span class="sxs-lookup"><span data-stu-id="52070-174">For more detailed information on how ASC’s threat detection works, see [Azure Security Center detection capabilities](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities).</span></span>

#### <a name="how-do-i-manage-and-respond-to-security-alerts"></a><span data-ttu-id="52070-175">Jak spravovat a reakce na výstrahy zabezpečení?</span><span class="sxs-lookup"><span data-stu-id="52070-175">How do I manage and respond to security alerts?</span></span>

<span data-ttu-id="52070-176">Seznam upřednostňovaných výstrah zabezpečení se zobrazí v Centru zabezpečení společně s informacemi, které potřebujete k rychlému prozkoumání problém.</span><span class="sxs-lookup"><span data-stu-id="52070-176">A list of prioritized security alerts is shown in Security Center along with the information you need to quickly investigate the problem.</span></span> <span data-ttu-id="52070-177">Security Center také obsahuje doporučení, jak se řešení útoku.</span><span class="sxs-lookup"><span data-stu-id="52070-177">Security Center also includes recommendations for how to remediate an attack.</span></span> <span data-ttu-id="52070-178">Pokud chcete spravovat výstrahy zabezpečení, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="52070-178">To manage your security alerts, do the following:</span></span>

1. <span data-ttu-id="52070-179">Vyberte **výstrahy zabezpečení** dlaždici na řídicím panelu ASC.</span><span class="sxs-lookup"><span data-stu-id="52070-179">Select the **Security alerts** tile in the ASC dashboard.</span></span> <span data-ttu-id="52070-180">To obsahuje podrobnosti pro každou výstrahu.</span><span class="sxs-lookup"><span data-stu-id="52070-180">This shows details for each alert.</span></span>

2. <span data-ttu-id="52070-181">Chcete-li filtrovat podle data, stav a závažnost výstrahy, vyberte **filtru** a potom vyberte hodnoty, které chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="52070-181">To filter alerts based on date, state, and severity, select **Filter** and then select the values you want to see.</span></span>

3. <span data-ttu-id="52070-182">Reagovat na upozornění, vyberte ho a zkontrolujte informace a potom vyberte prostředek, který byl napaden.</span><span class="sxs-lookup"><span data-stu-id="52070-182">To respond to an alert, select it and review the information, then select the resource that was attacked.</span></span>

4. <span data-ttu-id="52070-183">V **popis** poli se zobrazí podrobnosti, včetně doporučené nápravy.</span><span class="sxs-lookup"><span data-stu-id="52070-183">In the **Description** field, you’ll see details, including recommended remediation.</span></span>

![](media/protect-personal-data-azure-security-center/security-alerts.png)

<span data-ttu-id="52070-184">Podrobné pokyny pro reakce na výstrahy zabezpečení, najdete v části [Správa a zpracování výstrah zabezpečení v Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)</span><span class="sxs-lookup"><span data-stu-id="52070-184">For more detailed instructions on responding to security alerts, see [Managing and responding to security alerts in Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)</span></span>

<span data-ttu-id="52070-185">Další informace v prošetřování výstrah zabezpečení, společnost může integrovat ASC výstrahy vlastní řešení SIEM pomocí [integrace se službou Azure protokolu](https://aka.ms/AzLog).</span><span class="sxs-lookup"><span data-stu-id="52070-185">For further help in investigating security alerts, the company can integrate ASC alerts with its own SIEM solution, using [Azure Log Integration](https://aka.ms/AzLog).</span></span>

#### <a name="how-do-i-manage-security-incidents"></a><span data-ttu-id="52070-186">Jak lze spravovat incidenty zabezpečení?</span><span class="sxs-lookup"><span data-stu-id="52070-186">How do I manage security incidents?</span></span>

<span data-ttu-id="52070-187">V ASC je incidentu zabezpečení agregaci všechny výstrahy pro prostředek, které zarovnat kill řetězu vzory.</span><span class="sxs-lookup"><span data-stu-id="52070-187">In ASC, a security incident is an aggregation of all alerts for a resource that align with kill chain patterns.</span></span> <span data-ttu-id="52070-188">Incident odhalí seznam souvisejících výstrah, který vám umožní získat další informace o každém výskytu.</span><span class="sxs-lookup"><span data-stu-id="52070-188">An Incident will reveal the list of related alerts, which enables you to obtain more information about each occurrence.</span></span> <span data-ttu-id="52070-189">Dlaždice výstrahy zabezpečení a okno zobrazí incidenty.</span><span class="sxs-lookup"><span data-stu-id="52070-189">Incidents appear in the Security Alerts tile and blade.</span></span>

<span data-ttu-id="52070-190">Lze prohlížet a spravovat incidenty zabezpečení, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="52070-190">To review and manage security incidents, do the following:</span></span>

1. <span data-ttu-id="52070-191">Vyberte **výstrahy zabezpečení** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="52070-191">Select the **Security alerts** tile.</span></span> <span data-ttu-id="52070-192">Pokud je zjištěn bezpečnostního incidentu, zobrazí se v grafu výstrahy zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="52070-192">if a security incident is detected, it will appear under the security alerts graph.</span></span> <span data-ttu-id="52070-193">Bude mít ikonu, která se liší od dalších výstrah.</span><span class="sxs-lookup"><span data-stu-id="52070-193">It will have an icon that’s different from other alerts.</span></span>

2. <span data-ttu-id="52070-194">Vyberte incident, chcete-li zobrazit další podrobnosti o této incidentu zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="52070-194">Select the incident to see more details about this security incident.</span></span> <span data-ttu-id="52070-195">Další podrobnosti jsou jeho úplný popis jeho závažnost, jeho aktuální stav, attacked prostředků, nápravy kroky pro incident a výstrahy, které byly do tohoto incidentu.</span><span class="sxs-lookup"><span data-stu-id="52070-195">Additional details include its full description, its severity, its current state, the attacked resource, the remediation steps for the incident, and the alerts that were included in this incident.</span></span>

<span data-ttu-id="52070-196">Můžete filtrovat zobrazíte **pouze incidenty**, **výstrahy pouze**, nebo **i**.</span><span class="sxs-lookup"><span data-stu-id="52070-196">You can filter to see **incidents only**, **alerts only**, or **both**.</span></span>

#### <a name="how-do-i-access-the-threat-intelligence-report"></a><span data-ttu-id="52070-197">Přístupu Threat Intelligence sestavy</span><span class="sxs-lookup"><span data-stu-id="52070-197">How do I access the Threat Intelligence Report?</span></span>

<span data-ttu-id="52070-198">ASC analyzují informace z více zdrojů k identifikaci hrozeb.</span><span class="sxs-lookup"><span data-stu-id="52070-198">ASC analyzes information from multiple sources to identify threats.</span></span> <span data-ttu-id="52070-199">Security Center pomáhá s reakcí na incidenty týmy prozkoumat ohrožení a oprava, zahrnuje sestavu intelligence hrozba, která obsahuje informace o ohrožení, která byla zjištěna.</span><span class="sxs-lookup"><span data-stu-id="52070-199">To assist incident response teams investigate and remediate threats, Security Center includes a threat intelligence report that contains information about the threat that was detected.</span></span>

<span data-ttu-id="52070-200">Security Center má tři typy sestav hrozeb, které se může lišit na útok.</span><span class="sxs-lookup"><span data-stu-id="52070-200">Security Center has three types of threat reports, which can vary per attack.</span></span>
<span data-ttu-id="52070-201">K dispozici jsou tyto sestavy:</span><span class="sxs-lookup"><span data-stu-id="52070-201">The reports available are:</span></span>

- <span data-ttu-id="52070-202">Sestava skupiny aktivit: obsahuje přímý dives do útočníci, jejich cíle a svoji.</span><span class="sxs-lookup"><span data-stu-id="52070-202">Activity Group Report: provides deep dives into attackers, their objectives and tactics.</span></span>

- <span data-ttu-id="52070-203">Sestava kampaně: zaměřuje se na podrobnosti o konkrétní útoku kampaně.</span><span class="sxs-lookup"><span data-stu-id="52070-203">Campaign Report: focuses on details of specific attack campaigns.</span></span>

- <span data-ttu-id="52070-204">Souhrnná sestava Threat: obsahuje všechny položky v předchozích dvou sestav.</span><span class="sxs-lookup"><span data-stu-id="52070-204">Threat Summary Report: covers all items in the previous two reports.</span></span>

<span data-ttu-id="52070-205">Tento typ informací je velmi užitečná během reakce na incidenty, kde je probíhajícím vyšetřování pochopit zdroji útoku, útočníka motivace a co dělat ke zmírnění tohoto problému postoupíte.</span><span class="sxs-lookup"><span data-stu-id="52070-205">This type of information is very useful during the incident response process, where there is an ongoing investigation to understand the source of the attack, the attacker’s motivations, and what to do to mitigate this issue moving forward.</span></span>

1. <span data-ttu-id="52070-206">Pro přístup k sestavě intelligence hrozby, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="52070-206">To access the threat intelligence report, do the following:</span></span>

2. <span data-ttu-id="52070-207">Vyberte **výstrahy zabezpečení** dlaždici na řídicím panelu ASC.</span><span class="sxs-lookup"><span data-stu-id="52070-207">Select the **Security alerts** tile on the ASC dashboard.</span></span>

3. <span data-ttu-id="52070-208">Vyberte výstrahu zabezpečení, pro které chcete získat další informace.</span><span class="sxs-lookup"><span data-stu-id="52070-208">Select the security alert for which you want to obtain more information.</span></span>

4. <span data-ttu-id="52070-209">V **sestavy** pole, klikněte na odkaz na sestavu intelligence hrozeb.</span><span class="sxs-lookup"><span data-stu-id="52070-209">In the **Reports** field, click the link to the threat intelligence report.</span></span>

5. <span data-ttu-id="52070-210">Otevře se soubor PDF, kterou si můžete stáhnout.</span><span class="sxs-lookup"><span data-stu-id="52070-210">This will open the PDF file, which you can download.</span></span>

![](media/protect-personal-data-azure-security-center/security-alerts-suspicious-process.png)

<span data-ttu-id="52070-211">Další informace o sestavě ASC threat intelligence najdete v tématu [Azure Security Center Threat Intelligence sestavy.](https://docs.microsoft.com/azure/security-center/security-center-threat-report)</span><span class="sxs-lookup"><span data-stu-id="52070-211">For additional information about the ASC threat intelligence report, see [Azure Security Center Threat Intelligence Report.](https://docs.microsoft.com/azure/security-center/security-center-threat-report)</span></span>

### <a name="assessment"></a><span data-ttu-id="52070-212">Hodnocení</span><span class="sxs-lookup"><span data-stu-id="52070-212">Assessment</span></span>

<span data-ttu-id="52070-213">Usnadní testování, hodnocení a vyhodnocení lepšímu zabezpečení, ASC poskytuje pro vyhodnocení integrované ohrožení zabezpečení s Qualys cloudu agentů, jako součást své doporučení součásti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="52070-213">To help with testing, assessment and evaluation of your security posture, ASC provides for integrated vulnerability assessment with Qualys cloud agents, as a part of its virtual machine recommendations component.</span></span>

<span data-ttu-id="52070-214">Qualys agent sestav ohrožení zabezpečení dat Qualys platformy pro správu, které pak odešle ohrožení zabezpečení a sledování dat stavu zpět do ASC.</span><span class="sxs-lookup"><span data-stu-id="52070-214">The Qualys agent reports vulnerability data to the Qualys management platform, which then sends vulnerability and health monitoring data back to ASC.</span></span> <span data-ttu-id="52070-215">Doporučení k přidání řešení pro vyhodnocení ohrožení zabezpečení se zobrazí v **doporučení** okno na řídicí panel ASC.</span><span class="sxs-lookup"><span data-stu-id="52070-215">The recommendation to add a vulnerability assessment solution is displayed in the **Recommendations** blade on the ASC dashboard.</span></span>

<span data-ttu-id="52070-216">Po nainstalování řešení posouzení ohrožení zabezpečení na cílovém virtuálním počítači Security Center tento virtuální počítač prohledá a zjistí ohrožení zabezpečení systému a aplikací.</span><span class="sxs-lookup"><span data-stu-id="52070-216">After the vulnerability assessment solution is installed on the target VM, Security Center scans the VM to detect and identify system and application vulnerabilities.</span></span> <span data-ttu-id="52070-217">Zjištěné problémy se zobrazí pod možností **Doporučení pro virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="52070-217">Detected issues are shown under the **Virtual Machines Recommendations** option.</span></span>

#### <a name="how-do-i-implement-a-vulnerability-assessment-solution"></a><span data-ttu-id="52070-218">Jak implementovat řešení pro vyhodnocení ohrožení zabezpečení?</span><span class="sxs-lookup"><span data-stu-id="52070-218">How do I implement a vulnerability assessment solution?</span></span> 

<span data-ttu-id="52070-219">Pokud virtuální počítač nemá řešení pro vyhodnocení integrované ohrožení zabezpečení už nasazená, Security Center doporučuje ji nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="52070-219">If a Virtual Machine does not have an integrated vulnerability assessment solution already deployed, Security Center recommends that it be installed.</span></span>

1. <span data-ttu-id="52070-220">Na řídicím panelu ASC na **doporučení** vyberte **přidat řešení pro vyhodnocení ohrožení zabezpečení.**</span><span class="sxs-lookup"><span data-stu-id="52070-220">In the ASC dashboard, on the **Recommendations** blade, select **Add a vulnerability assessment solution.**</span></span>

2. <span data-ttu-id="52070-221">Vyberte virtuální počítače, ve které chcete nainstalovat řešení pro vyhodnocení ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="52070-221">Select the VMs where you want to install the vulnerability assessment solution.</span></span>

3. <span data-ttu-id="52070-222">Klikněte na **nainstalovat na virtuální počítače [počet].**</span><span class="sxs-lookup"><span data-stu-id="52070-222">Click on **Install on [number of] VMs.**</span></span>

4. <span data-ttu-id="52070-223">Vyberte jedno partnerské řešení v Azure Marketplace, nebo pod **použít existující řešení** vyberte **Qualys.**</span><span class="sxs-lookup"><span data-stu-id="52070-223">Select a partner solution in the Azure Marketplace, or under **Use existing solution,** select **Qualys.**</span></span>

5. <span data-ttu-id="52070-224">Aktualizace nastavení automatického můžete zapnout nebo vypnout **partnerských řešení** okno.</span><span class="sxs-lookup"><span data-stu-id="52070-224">You can turn the auto update settings on or off in the **Partner Solutions** blade.</span></span>

<span data-ttu-id="52070-225">Další pokyny o tom, jak implementovat řešení pro vyhodnocení ohrožení zabezpečení najdete v tématu [vyhodnocení ohrožení zabezpečení v Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)</span><span class="sxs-lookup"><span data-stu-id="52070-225">For further instructions on how to implement a vulnerability assessment solution, see [Vulnerability Assessment in Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)</span></span>

## <a name="next-steps"></a><span data-ttu-id="52070-226">Další kroky</span><span class="sxs-lookup"><span data-stu-id="52070-226">Next steps</span></span>

- [<span data-ttu-id="52070-227">Úvodní příručka Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="52070-227">Azure Security Center quick start guide</span></span>](https://docs.microsoft.com/azure/security-center/security-center-get-started)

- [<span data-ttu-id="52070-228">Úvod do Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="52070-228">Introduction to Azure Security Center</span></span>](https://docs.microsoft.com/azure/security-center/security-center-intro)

- [<span data-ttu-id="52070-229">Integrace Azure Security Center výstrahy s integrací protokolů Azure</span><span class="sxs-lookup"><span data-stu-id="52070-229">Integrating Azure Security Center alerts with Azure log integration</span></span>](https://docs.microsoft.com/azure/security-center/security-center-integrating-alerts-with-log-integration)

- [<span data-ttu-id="52070-230">Nárůst Azure Security Center s vyhodnocení integrované ohrožení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="52070-230">Boost Azure Security Center with Integrated Vulnerability Assessment</span></span>](https://blogs.msdn.microsoft.com/azuresecurity/2016/12/16/boost-azure-security-center-with-integrated-vulnerability-assessment/)
