---
title: "Úvod do Azure Security Center | Microsoft Docs"
description: "Můžete se dozvědět o službě Azure Security Center, jejích klíčových funkcích a způsobu práce."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: 8951167213da6ab5341c1ca420353ec625ef5424
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-azure-security-center"></a><span data-ttu-id="b35a3-103">Úvod do Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="b35a3-103">Introduction to Azure Security Center</span></span>
<span data-ttu-id="b35a3-104">Můžete se dozvědět o službě Azure Security Center, jejích klíčových funkcích a způsobu práce.</span><span class="sxs-lookup"><span data-stu-id="b35a3-104">Learn about Azure Security Center, its key capabilities, and how it works.</span></span>

> [!NOTE]
> <span data-ttu-id="b35a3-105">Od začátku června 2017 bude Security Center používat ke shromažďování a ukládání dat agenta Microsoft Monitoring Agent.</span><span class="sxs-lookup"><span data-stu-id="b35a3-105">Beginning in early June 2017, Security Center will use the Microsoft Monitoring Agent to collect and store data.</span></span> <span data-ttu-id="b35a3-106">Další informace najdete v článku o [migraci platformy pro Azure Security Center](security-center-platform-migration.md).</span><span class="sxs-lookup"><span data-stu-id="b35a3-106">See [Azure Security Center Platform Migration](security-center-platform-migration.md) to learn more.</span></span> <span data-ttu-id="b35a3-107">Informace v tomto článku představují funkce služby Security Center po přechodu na agenta Microsoft Monitoring Agent.</span><span class="sxs-lookup"><span data-stu-id="b35a3-107">The information in this article represents Security Center functionality after transition to the Microsoft Monitoring Agent.</span></span>
>
>

## <a name="what-is-azure-security-center"></a><span data-ttu-id="b35a3-108">Co je Azure Security Center?</span><span class="sxs-lookup"><span data-stu-id="b35a3-108">What is Azure Security Center?</span></span>
 <span data-ttu-id="b35a3-109">Security Center pomáhá předcházet hrozbám, zjišťovat je a reagovat na ně a nabízí lepší přehled o zabezpečení prostředků Azure a kontrolu nad nimi.</span><span class="sxs-lookup"><span data-stu-id="b35a3-109">Security Center helps you prevent, detect, and respond to threats with increased visibility into and control over the security of your Azure resources.</span></span> <span data-ttu-id="b35a3-110">Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných Azure, pomáhá zjišťovat hrozby, kterých byste si jinak nevšimli, a spolupracuje s řadou řešení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="b35a3-110">It provides integrated security monitoring and policy management across your Azure subscriptions, helps detect threats that might otherwise go unnoticed, and works with a broad ecosystem of security solutions.</span></span>

## <a name="key-capabilities"></a><span data-ttu-id="b35a3-111">Klíčové funkce</span><span class="sxs-lookup"><span data-stu-id="b35a3-111">Key capabilities</span></span>
 <span data-ttu-id="b35a3-112">Security Center nabízí snadno použitelné a efektivní funkce prevence, zjišťování a reakce na ohrožení, které jsou integrované v Azure.</span><span class="sxs-lookup"><span data-stu-id="b35a3-112">Security Center delivers easy-to-use and effective threat prevention, detection, and response capabilities that are built in to Azure.</span></span> <span data-ttu-id="b35a3-113">Klíčové funkce:</span><span class="sxs-lookup"><span data-stu-id="b35a3-113">Key capabilities are:</span></span>

| <span data-ttu-id="b35a3-114">Krok</span><span class="sxs-lookup"><span data-stu-id="b35a3-114">Stage</span></span> | <span data-ttu-id="b35a3-115">Schopnost</span><span class="sxs-lookup"><span data-stu-id="b35a3-115">Capability</span></span> |
| --- | --- |
| <span data-ttu-id="b35a3-116">Prevence</span><span class="sxs-lookup"><span data-stu-id="b35a3-116">Prevent</span></span> |<span data-ttu-id="b35a3-117">Sleduje stav zabezpečení vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="b35a3-117">Monitors the security state of your Azure resources</span></span> |
| <span data-ttu-id="b35a3-118">Prevence</span><span class="sxs-lookup"><span data-stu-id="b35a3-118">Prevent</span></span> | <span data-ttu-id="b35a3-119">Definuje zásady pro předplatné Azure na základě požadavků zabezpečení vaší společnosti, typů aplikací, které používá a citlivosti vašich dat</span><span class="sxs-lookup"><span data-stu-id="b35a3-119">Defines policies for your Azure subscriptions based on your company’s security requirements, the types of applications that you use, and the sensitivity of your data</span></span> |
| <span data-ttu-id="b35a3-120">Prevence</span><span class="sxs-lookup"><span data-stu-id="b35a3-120">Prevent</span></span> | <span data-ttu-id="b35a3-121">Používá doporučení zabezpečení řízená zásadami, aby vedla vlastníky služby procesem implementace potřebných kontrol.</span><span class="sxs-lookup"><span data-stu-id="b35a3-121">Uses policy-driven security recommendations to guide service owners through the process of implementing needed controls</span></span> |
| <span data-ttu-id="b35a3-122">Prevence</span><span class="sxs-lookup"><span data-stu-id="b35a3-122">Prevent</span></span> | <span data-ttu-id="b35a3-123">Rychle nasazuje bezpečnostní služby a zařízení Microsoftu a partnerů.</span><span class="sxs-lookup"><span data-stu-id="b35a3-123">Rapidly deploys security services and appliances from Microsoft and partners</span></span> |
| <span data-ttu-id="b35a3-124">Zjišťování</span><span class="sxs-lookup"><span data-stu-id="b35a3-124">Detect</span></span> |<span data-ttu-id="b35a3-125">Automaticky shromažďuje a analyzuje data zabezpečení z vašich prostředků Azure, sítě a řešení partnerů, jako jsou antimalwarové programy a brány firewall.</span><span class="sxs-lookup"><span data-stu-id="b35a3-125">Automatically collects and analyzes security data from your Azure resources, the network, and partner solutions like antimalware programs and firewalls</span></span> |
| <span data-ttu-id="b35a3-126">Zjišťování</span><span class="sxs-lookup"><span data-stu-id="b35a3-126">Detect</span></span> | <span data-ttu-id="b35a3-127">Využívá globální hrozby intelligence z produktů společnosti Microsoft a služeb, Microsoft digitální činů jednotky (přicházejí týmu DCU), Microsoft Security Response Center (MSRC) a externích informačních kanálů.</span><span class="sxs-lookup"><span data-stu-id="b35a3-127">Uses global threat intelligence from Microsoft products and services, the Microsoft Digital Crimes Unit (DCU), the Microsoft Security Response Center (MSRC), and external feeds</span></span> |
| <span data-ttu-id="b35a3-128">Zjišťování</span><span class="sxs-lookup"><span data-stu-id="b35a3-128">Detect</span></span> | <span data-ttu-id="b35a3-129">Používá pokročilou analýzu, včetně machine learningu a analýzy chování.</span><span class="sxs-lookup"><span data-stu-id="b35a3-129">Applies advanced analytics, including machine learning and behavioral analysis</span></span> |
| <span data-ttu-id="b35a3-130">Reakce</span><span class="sxs-lookup"><span data-stu-id="b35a3-130">Respond</span></span> |<span data-ttu-id="b35a3-131">Poskytuje incidenty a výstrahy zabezpečení seřazené podle priority.</span><span class="sxs-lookup"><span data-stu-id="b35a3-131">Provides prioritized security incidents/alerts</span></span> |
| <span data-ttu-id="b35a3-132">Reakce</span><span class="sxs-lookup"><span data-stu-id="b35a3-132">Respond</span></span> | <span data-ttu-id="b35a3-133">Nabízí podrobný náhled na zdroj útoku a zasažené prostředky.</span><span class="sxs-lookup"><span data-stu-id="b35a3-133">Offers insights into the source of the attack and impacted resources</span></span> |
| <span data-ttu-id="b35a3-134">Reakce</span><span class="sxs-lookup"><span data-stu-id="b35a3-134">Respond</span></span> | <span data-ttu-id="b35a3-135">Navrhuje způsoby, jak aktuální útok zastavit, a pomáhá zabránit budoucím útokům.</span><span class="sxs-lookup"><span data-stu-id="b35a3-135">Suggests ways to stop the current attack and help prevent future attacks</span></span> |

## <a name="introductory-walkthrough"></a><span data-ttu-id="b35a3-136">Úvodní prohlídka</span><span class="sxs-lookup"><span data-stu-id="b35a3-136">Introductory walkthrough</span></span>

> [!NOTE]
> <span data-ttu-id="b35a3-137">Tento dokument vám tuto službu představí formou ukázkového nasazení.</span><span class="sxs-lookup"><span data-stu-id="b35a3-137">This document introduces the service by using an example deployment.</span></span> <span data-ttu-id="b35a3-138">Tento dokument není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="b35a3-138">This document is not a step-by-step guide.</span></span>
>
>

 <span data-ttu-id="b35a3-139">Služba Security Center je přístupná prostřednictvím [portálu Azure](https://azure.microsoft.com/features/azure-portal/).</span><span class="sxs-lookup"><span data-stu-id="b35a3-139">You access Security Center from the [Azure portal](https://azure.microsoft.com/features/azure-portal/).</span></span> <span data-ttu-id="b35a3-140">[Přihlaste se k portálu](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b35a3-140">[Sign in to the portal](https://portal.azure.com).</span></span> <span data-ttu-id="b35a3-141">V hlavní nabídce portálu, přejděte k položce **Security Center** nebo vyberte **Security Center** dlaždici, kterou jste dříve připnuli k řídicímu panelu portálu.</span><span class="sxs-lookup"><span data-stu-id="b35a3-141">Under the main portal menu, scroll to the **Security Center** option or select the **Security Center** tile that you previously pinned to the portal dashboard.</span></span>

![Dlaždice zabezpečení na portálu Azure][1]

<span data-ttu-id="b35a3-143">Ze služby Security Center můžete nastavovat zásady zabezpečení, sledovat konfigurace zabezpečení a zobrazovat výstrahy zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="b35a3-143">From Security Center, you can set security policies, monitor security configurations, and view security alerts.</span></span>

### <a name="security-policies"></a><span data-ttu-id="b35a3-144">Zásady zabezpečení</span><span class="sxs-lookup"><span data-stu-id="b35a3-144">Security policies</span></span>
<span data-ttu-id="b35a3-145">Můžete definovat zásady pro vaše předplatná Azure podle požadavků zabezpečení vaší společnosti.</span><span class="sxs-lookup"><span data-stu-id="b35a3-145">You can define policies for your Azure subscriptions according to your company's security requirements.</span></span> <span data-ttu-id="b35a3-146">Můžete je také přizpůsobit typům aplikací, které používáte, nebo citlivosti dat v každém předplatném.</span><span class="sxs-lookup"><span data-stu-id="b35a3-146">You can also tailor them to the types of applications you're using or to the sensitivity of the data in each subscription.</span></span> <span data-ttu-id="b35a3-147">Například prostředky používané pro vývoj nebo testování můžou mít jiné požadavky na zabezpečení než ty, které se používají v aplikacích v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b35a3-147">For example, resources used for development or testing may have different security requirements than those used for production applications.</span></span> <span data-ttu-id="b35a3-148">Aplikace pracující s regulovanými daty, třeba s osobními údaji, zase můžou vyžadovat jinou úroveň zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="b35a3-148">Likewise, applications with regulated data like PII may require a higher level of security.</span></span>

> [!NOTE]
> <span data-ttu-id="b35a3-149">Pokud chcete upravit zásadu zabezpečení, musí být správce zabezpečení nebo odběru vlastníka nebo přispěvatele.</span><span class="sxs-lookup"><span data-stu-id="b35a3-149">To modify a security policy, you must be a Security Administrator or the subscription's Owner or Contributor.</span></span> <span data-ttu-id="b35a3-150">Další informace o rolích a povolené akce v Centru zabezpečení, najdete v části [oprávnění v Azure Security Center](security-center-permissions.md).</span><span class="sxs-lookup"><span data-stu-id="b35a3-150">To learn more about roles and allowed actions in Security Center, see [Permissions in Azure Security Center](security-center-permissions.md).</span></span>
>
>

<span data-ttu-id="b35a3-151">V okně **Security Center** vyberte dlaždici **Zásady**, kde zobrazíte seznam předplatných a skupin prostředků.</span><span class="sxs-lookup"><span data-stu-id="b35a3-151">On the **Security Center** blade, select the **Policy** tile for a list of your subscriptions and resource groups.</span></span>   

![Okno Security Center][2]

<span data-ttu-id="b35a3-153">Na **zásady zabezpečení** okně, vyberte předplatné, abyste zobrazili podrobnosti zásady.</span><span class="sxs-lookup"><span data-stu-id="b35a3-153">On the **Security policy** blade, select a subscription to view the policy details.</span></span>

<span data-ttu-id="b35a3-154">**Shromažďování dat** umožňuje shromažďování dat pro zásady zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="b35a3-154">**Data collection** enables data collection for a security policy.</span></span> <span data-ttu-id="b35a3-155">Povolením získáte následující:</span><span class="sxs-lookup"><span data-stu-id="b35a3-155">Enabling provides:</span></span>

* <span data-ttu-id="b35a3-156">Denní kontrolu všech podporovaných virtuálních počítačů (VM) pro monitorování zabezpečení a doporučení.</span><span class="sxs-lookup"><span data-stu-id="b35a3-156">Daily scanning of all supported virtual machines (VMs) for security monitoring and recommendations.</span></span>
* <span data-ttu-id="b35a3-157">Shromažďování událostí zabezpečení pro analýzu a zjišťování hrozeb.</span><span class="sxs-lookup"><span data-stu-id="b35a3-157">Collection of security events for analysis and threat detection.</span></span>

> [!NOTE]
> <span data-ttu-id="b35a3-158">Shromažďování dat je nakonfigurován na úrovni předplatného.</span><span class="sxs-lookup"><span data-stu-id="b35a3-158">Data collection is configured at the subscription level.</span></span>
>
>

<span data-ttu-id="b35a3-159">Vyberte **zásada Zabránění** otevřete **zásada Zabránění** okno.</span><span class="sxs-lookup"><span data-stu-id="b35a3-159">Select **Prevention policy** to open the **Prevention policy** blade.</span></span> <span data-ttu-id="b35a3-160">**Zobrazit doporučení pro** umožňuje vybrat ovládacích prvků zabezpečení, které chcete monitorovat a doporučení, které chcete zobrazit podle potřeb zabezpečení prostředků v rámci předplatného.</span><span class="sxs-lookup"><span data-stu-id="b35a3-160">**Show recommendations for** lets you choose the security controls that you want to monitor and the recommendations that you want to see based on the security needs of the resources within the subscription.</span></span>

### <a name="security-recommendations"></a><span data-ttu-id="b35a3-161">Doporučení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="b35a3-161">Security recommendations</span></span>
 <span data-ttu-id="b35a3-162">Služba Security Center analyzuje stav zabezpečení vašich prostředků Azure, aby identifikovala potenciální ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="b35a3-162">Security Center analyzes the security state of your Azure resources to identify potential security vulnerabilities.</span></span> <span data-ttu-id="b35a3-163">Seznam doporučení vás provede procesem konfigurace potřebných kontrol.</span><span class="sxs-lookup"><span data-stu-id="b35a3-163">A list of recommendations guides you through the process of configuring needed controls.</span></span> <span data-ttu-id="b35a3-164">Příklady obsahují:</span><span class="sxs-lookup"><span data-stu-id="b35a3-164">Examples include:</span></span>

* <span data-ttu-id="b35a3-165">Zřizování antimalwaru, aby se pomohl identifikovat a odebrat škodlivý software</span><span class="sxs-lookup"><span data-stu-id="b35a3-165">Provisioning antimalware to help identify and remove malicious software</span></span>
* <span data-ttu-id="b35a3-166">Konfigurace skupin zabezpečení sítě a pravidla pro řízení přenosu do virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="b35a3-166">Configuring network security groups and rules to control traffic to VMs</span></span>
* <span data-ttu-id="b35a3-167">Zřizování firewallů webových aplikací, které pomáhají bránit útokům zaměřeným na vaše webové aplikace</span><span class="sxs-lookup"><span data-stu-id="b35a3-167">Provisioning of web application firewalls to help defend against attacks that target your web applications</span></span>
* <span data-ttu-id="b35a3-168">Nasazení chybějících aktualizací systému</span><span class="sxs-lookup"><span data-stu-id="b35a3-168">Deploying missing system updates</span></span>
* <span data-ttu-id="b35a3-169">Adresování konfigurací operačního systému, které neodpovídají doporučeným standardním hodnotám</span><span class="sxs-lookup"><span data-stu-id="b35a3-169">Addressing OS configurations that do not match the recommended baselines</span></span>

<span data-ttu-id="b35a3-170">Seznam doporučení získáte kliknutím na dlaždici **Doporučení**.</span><span class="sxs-lookup"><span data-stu-id="b35a3-170">Click the **Recommendations** tile for a list of recommendations.</span></span> <span data-ttu-id="b35a3-171">Kliknutím na jednotlivá doporučení zobrazíte další informace nebo provedete akce vedoucí k vyřešení problému.</span><span class="sxs-lookup"><span data-stu-id="b35a3-171">Click each recommendation to view additional information or to take action to resolve the issue.</span></span>

![Doporučení zabezpečení v Azure Security Center][5]

### <a name="security-state-of-azure-resources"></a><span data-ttu-id="b35a3-173">Stav zabezpečení prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="b35a3-173">Security state of Azure resources</span></span>
<span data-ttu-id="b35a3-174">**Prevence** část řídicího panelu ukazuje celkové postavení zabezpečení prostředí podle typů prostředků, včetně virtuálních počítačů, webových aplikací a dalším prostředkům.</span><span class="sxs-lookup"><span data-stu-id="b35a3-174">The **Prevention** section of the dashboard shows the overall security posture of the environment by resource type, including VMs, web applications, and other resources.</span></span>   

<span data-ttu-id="b35a3-175">Vyberte typ prostředku v rámci **prevence** zobrazíte další informace, včetně seznamu možných ohrožení zabezpečení, které byly identifikovány.</span><span class="sxs-lookup"><span data-stu-id="b35a3-175">Select a resource type under **Prevention** to view more information, including a list of any potential security vulnerabilities that have been identified.</span></span> <span data-ttu-id="b35a3-176">(**Výpočetní** je vybraný v následujícím příkladu.)</span><span class="sxs-lookup"><span data-stu-id="b35a3-176">(**Compute** is selected in the example below.)</span></span>

![Dlaždice Stav prostředků][6]

### <a name="security-alerts"></a><span data-ttu-id="b35a3-178">Výstrahy zabezpečení</span><span class="sxs-lookup"><span data-stu-id="b35a3-178">Security alerts</span></span>
 <span data-ttu-id="b35a3-179">Security Center automaticky shromažďuje, analyzuje a integruje data protokolu z vašich prostředků Azure, sítě a řešení partnerů, jako jsou antimalwarové programy a brány firewall.</span><span class="sxs-lookup"><span data-stu-id="b35a3-179">Security Center automatically collects, analyzes, and integrates log data from your Azure resources, the network, and partner solutions like antimalware programs and firewalls.</span></span> <span data-ttu-id="b35a3-180">Při zjištění ohrožení zabezpečení se vytvoří výstraha zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="b35a3-180">When threats are detected, a security alert is created.</span></span> <span data-ttu-id="b35a3-181">Příklady zahrnují zjišťování následujících situací:</span><span class="sxs-lookup"><span data-stu-id="b35a3-181">Examples include detection of:</span></span>

* <span data-ttu-id="b35a3-182">Ohrožené virtuální počítače komunikaci s známé škodlivé IP adresy</span><span class="sxs-lookup"><span data-stu-id="b35a3-182">Compromised VMs communicating with known malicious IP addresses</span></span>
* <span data-ttu-id="b35a3-183">Pokročilý malware zjištěný pomocí zasílání zpráv o chybách systému Windows</span><span class="sxs-lookup"><span data-stu-id="b35a3-183">Advanced malware detected by using Windows error reporting</span></span>
* <span data-ttu-id="b35a3-184">Útoky hrubou silou na virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="b35a3-184">Brute force attacks against VMs</span></span>
* <span data-ttu-id="b35a3-185">Výstrahy zabezpečení z integrovaných antimalwarových programů a bran firewall</span><span class="sxs-lookup"><span data-stu-id="b35a3-185">Security alerts from integrated antimalware programs and firewalls</span></span>

<span data-ttu-id="b35a3-186">Kliknutím na dlaždici **Výstrahy zabezpečení** zobrazíte seznam výstrah seřazený podle priority.</span><span class="sxs-lookup"><span data-stu-id="b35a3-186">Clicking the **Security alerts** tile displays a list of prioritized alerts.</span></span>

![Výstrahy zabezpečení][7]

<span data-ttu-id="b35a3-188">Výběrem výstrahy zobrazíte další informace o útoku a návrhy na jeho řešení.</span><span class="sxs-lookup"><span data-stu-id="b35a3-188">Selecting an alert shows more information about the attack and suggestions for how to remediate it.</span></span>

![Podrobnosti výstrahy zabezpečení][8]

### <a name="partner-solutions"></a><span data-ttu-id="b35a3-190">Partnerská řešení</span><span class="sxs-lookup"><span data-stu-id="b35a3-190">Partner solutions</span></span>
<span data-ttu-id="b35a3-191">**Partner solutions** dlaždici umožňuje sledovat na první pohled stav zabezpečení vašich partnerských řešení integrovaných ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="b35a3-191">The **Partner solutions** tile lets you monitor at a glance the security state of your partner solutions integrated with your Azure subscription.</span></span> <span data-ttu-id="b35a3-192">Security Center zobrazuje výstrahy, které pocházejí z těchto řešení.</span><span class="sxs-lookup"><span data-stu-id="b35a3-192">Security Center displays alerts coming from the solutions.</span></span>

<span data-ttu-id="b35a3-193">Vyberte dlaždici **Partner solutions** (Partnerská řešení).</span><span class="sxs-lookup"><span data-stu-id="b35a3-193">Select the **Partner solutions** tile.</span></span> <span data-ttu-id="b35a3-194">Otevře se okno se seznamem všech připojených partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="b35a3-194">A blade opens displaying a list of all connected partner solutions.</span></span>

![Partnerská řešení][9]

## <a name="get-started"></a><span data-ttu-id="b35a3-196">Začínáme</span><span class="sxs-lookup"><span data-stu-id="b35a3-196">Get started</span></span>
<span data-ttu-id="b35a3-197">Chcete-li začít využívat Security Center, potřebujete předplatné služby Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="b35a3-197">To get started with Security Center, you need a subscription to Microsoft Azure.</span></span> <span data-ttu-id="b35a3-198">Služba Security Center je povolená s vaším předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="b35a3-198">Security Center is enabled with your Azure subscription.</span></span> <span data-ttu-id="b35a3-199">Pokud nemáte předplatné, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b35a3-199">If you do not have a subscription, you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

 <span data-ttu-id="b35a3-200">Služba Security Center je přístupná prostřednictvím [portálu Azure](https://azure.microsoft.com/features/azure-portal/).</span><span class="sxs-lookup"><span data-stu-id="b35a3-200">You access Security Center from the [Azure portal](https://azure.microsoft.com/features/azure-portal/).</span></span> <span data-ttu-id="b35a3-201">Další informace najdete v [dokumentaci k portálu](https://azure.microsoft.com/documentation/services/azure-portal/).</span><span class="sxs-lookup"><span data-stu-id="b35a3-201">See the [portal documentation](https://azure.microsoft.com/documentation/services/azure-portal/) to learn more.</span></span>

<span data-ttu-id="b35a3-202">Téma [Začínáme s Azure Security Center](security-center-get-started.md) vás rychle provede komponentami pro monitorování zabezpečení a správu zásad služby Security Center.</span><span class="sxs-lookup"><span data-stu-id="b35a3-202">[Getting started with Azure Security Center](security-center-get-started.md) quickly guides you through the security-monitoring and policy-management components of Security Center.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b35a3-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b35a3-203">Next steps</span></span>
<span data-ttu-id="b35a3-204">V tomto dokumentu jste se seznámili se službou Security Center, jejími klíčovými funkcemi a způsobu zahájení práce.</span><span class="sxs-lookup"><span data-stu-id="b35a3-204">In this document, you were introduced to Security Center, its key capabilities, and how to get started.</span></span> <span data-ttu-id="b35a3-205">Další informace najdete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="b35a3-205">To learn more, see the following resources:</span></span>

* <span data-ttu-id="b35a3-206">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak nakonfigurovat zásady zabezpečení pro skupiny prostředků a předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="b35a3-206">[Setting security policies in Azure Security Center](security-center-policies.md) — Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="b35a3-207">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="b35a3-207">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) — Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="b35a3-208">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se sledovat stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="b35a3-208">[Security health monitoring in Azure Security Center](security-center-monitoring.md) — Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="b35a3-209">[Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak spravovat a reakce na výstrahy zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="b35a3-209">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) — Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="b35a3-210">[Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="b35a3-210">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) — Learn how to monitor the health status of your partner solutions.</span></span>
- <span data-ttu-id="b35a3-211">[Zabezpečení dat v Azure Security Center](security-center-data-security.md) -další způsob správy a zabezpečení ve službě Security Center.</span><span class="sxs-lookup"><span data-stu-id="b35a3-211">[Azure Security Center data security](security-center-data-security.md) - Learn how data is managed and safeguarded in Security Center.</span></span>
* <span data-ttu-id="b35a3-212">[Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby.</span><span class="sxs-lookup"><span data-stu-id="b35a3-212">[Azure Security Center FAQ](security-center-faq.md) — Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="b35a3-213">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – získejte nejnovější informace zabezpečení Azure a informace.</span><span class="sxs-lookup"><span data-stu-id="b35a3-213">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) — Get the latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-intro/security-tile.png
[2]: ./media/security-center-intro/security-center.png
[3]: ./media/security-center-intro/security-policy.png
[4]: ./media/security-center-intro/security-policy-blade.png
[5]: ./media/security-center-intro/recommendations.png
[6]: ./media/security-center-intro/resources-health.png
[7]: ./media/security-center-intro/security-alert.png
[8]: ./media/security-center-intro/security-alert-detail.png
[9]: ./media/security-center-intro/partner-solutions.png
