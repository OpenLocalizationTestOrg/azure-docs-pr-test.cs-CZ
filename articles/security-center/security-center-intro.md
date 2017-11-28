---
title: aaaIntroduction tooAzure Security Center | Microsoft Docs
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
ms.openlocfilehash: 287dbaaa7e2004c522f103595bc316261daf05b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-security-center"></a><span data-ttu-id="7d19c-103">Úvod tooAzure Security Center</span><span class="sxs-lookup"><span data-stu-id="7d19c-103">Introduction tooAzure Security Center</span></span>
<span data-ttu-id="7d19c-104">Můžete se dozvědět o službě Azure Security Center, jejích klíčových funkcích a způsobu práce.</span><span class="sxs-lookup"><span data-stu-id="7d19c-104">Learn about Azure Security Center, its key capabilities, and how it works.</span></span>

> [!NOTE]
> <span data-ttu-id="7d19c-105">Počínaje časná 2017 června, Security Center použije hello agenta Microsoft Monitoring Agent toocollect a ukládat data.</span><span class="sxs-lookup"><span data-stu-id="7d19c-105">Beginning in early June 2017, Security Center will use hello Microsoft Monitoring Agent toocollect and store data.</span></span> <span data-ttu-id="7d19c-106">V tématu [Azure Security Center platformy migrace](security-center-platform-migration.md) toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="7d19c-106">See [Azure Security Center Platform Migration](security-center-platform-migration.md) toolearn more.</span></span> <span data-ttu-id="7d19c-107">Hello informace v tomto článku představuje funkce Security Center po přechodu toohello agenta Microsoft Monitoring Agent.</span><span class="sxs-lookup"><span data-stu-id="7d19c-107">hello information in this article represents Security Center functionality after transition toohello Microsoft Monitoring Agent.</span></span>
>
>

## <a name="what-is-azure-security-center"></a><span data-ttu-id="7d19c-108">Co je Azure Security Center?</span><span class="sxs-lookup"><span data-stu-id="7d19c-108">What is Azure Security Center?</span></span>
 <span data-ttu-id="7d19c-109">Security Center pomáhá zabránit, zjistit a reagovat toothreats nabízí lepší přehled a kontrolu nad hello zabezpečení vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="7d19c-109">Security Center helps you prevent, detect, and respond toothreats with increased visibility into and control over hello security of your Azure resources.</span></span> <span data-ttu-id="7d19c-110">Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných Azure, pomáhá zjišťovat hrozby, kterých byste si jinak nevšimli, a spolupracuje s řadou řešení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="7d19c-110">It provides integrated security monitoring and policy management across your Azure subscriptions, helps detect threats that might otherwise go unnoticed, and works with a broad ecosystem of security solutions.</span></span>

## <a name="key-capabilities"></a><span data-ttu-id="7d19c-111">Klíčové funkce</span><span class="sxs-lookup"><span data-stu-id="7d19c-111">Key capabilities</span></span>
 <span data-ttu-id="7d19c-112">Security Center nabízí snadno použitelné a efektivní threat prevence, zjišťování a reakce na funkce, které jsou součástí tooAzure.</span><span class="sxs-lookup"><span data-stu-id="7d19c-112">Security Center delivers easy-to-use and effective threat prevention, detection, and response capabilities that are built in tooAzure.</span></span> <span data-ttu-id="7d19c-113">Klíčové funkce:</span><span class="sxs-lookup"><span data-stu-id="7d19c-113">Key capabilities are:</span></span>

| <span data-ttu-id="7d19c-114">Krok</span><span class="sxs-lookup"><span data-stu-id="7d19c-114">Stage</span></span> | <span data-ttu-id="7d19c-115">Schopnost</span><span class="sxs-lookup"><span data-stu-id="7d19c-115">Capability</span></span> |
| --- | --- |
| <span data-ttu-id="7d19c-116">Prevence</span><span class="sxs-lookup"><span data-stu-id="7d19c-116">Prevent</span></span> |<span data-ttu-id="7d19c-117">Monitorování hello stav zabezpečení vašich prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="7d19c-117">Monitors hello security state of your Azure resources</span></span> |
| <span data-ttu-id="7d19c-118">Prevence</span><span class="sxs-lookup"><span data-stu-id="7d19c-118">Prevent</span></span> | <span data-ttu-id="7d19c-119">Definuje zásady pro předplatné Azure, na základě požadavků zabezpečení vaší společnosti, hello typy aplikací, které používáte a citlivosti vašich data hello</span><span class="sxs-lookup"><span data-stu-id="7d19c-119">Defines policies for your Azure subscriptions based on your company’s security requirements, hello types of applications that you use, and hello sensitivity of your data</span></span> |
| <span data-ttu-id="7d19c-120">Prevence</span><span class="sxs-lookup"><span data-stu-id="7d19c-120">Prevent</span></span> | <span data-ttu-id="7d19c-121">Vlastníky služby používá řízená zásadami zabezpečení doporučení tooguide hello procesem implementace potřebných kontrol.</span><span class="sxs-lookup"><span data-stu-id="7d19c-121">Uses policy-driven security recommendations tooguide service owners through hello process of implementing needed controls</span></span> |
| <span data-ttu-id="7d19c-122">Prevence</span><span class="sxs-lookup"><span data-stu-id="7d19c-122">Prevent</span></span> | <span data-ttu-id="7d19c-123">Rychle nasazuje bezpečnostní služby a zařízení Microsoftu a partnerů.</span><span class="sxs-lookup"><span data-stu-id="7d19c-123">Rapidly deploys security services and appliances from Microsoft and partners</span></span> |
| <span data-ttu-id="7d19c-124">Zjišťování</span><span class="sxs-lookup"><span data-stu-id="7d19c-124">Detect</span></span> |<span data-ttu-id="7d19c-125">Automaticky shromažďuje a analyzuje data zabezpečení z vaše prostředky Azure, hello sítě a řešení partnerů, jako jsou antimalwarové programy a brány firewall</span><span class="sxs-lookup"><span data-stu-id="7d19c-125">Automatically collects and analyzes security data from your Azure resources, hello network, and partner solutions like antimalware programs and firewalls</span></span> |
| <span data-ttu-id="7d19c-126">Zjišťování</span><span class="sxs-lookup"><span data-stu-id="7d19c-126">Detect</span></span> | <span data-ttu-id="7d19c-127">Využívá globální hrozby intelligence z Microsoft produktů a služeb společnosti Microsoft digitální činů jednotky (DCU), hello hello Microsoft Security Response Center (MSRC) a externích informačních kanálů.</span><span class="sxs-lookup"><span data-stu-id="7d19c-127">Uses global threat intelligence from Microsoft products and services, hello Microsoft Digital Crimes Unit (DCU), hello Microsoft Security Response Center (MSRC), and external feeds</span></span> |
| <span data-ttu-id="7d19c-128">Zjišťování</span><span class="sxs-lookup"><span data-stu-id="7d19c-128">Detect</span></span> | <span data-ttu-id="7d19c-129">Používá pokročilou analýzu, včetně machine learningu a analýzy chování.</span><span class="sxs-lookup"><span data-stu-id="7d19c-129">Applies advanced analytics, including machine learning and behavioral analysis</span></span> |
| <span data-ttu-id="7d19c-130">Reakce</span><span class="sxs-lookup"><span data-stu-id="7d19c-130">Respond</span></span> |<span data-ttu-id="7d19c-131">Poskytuje incidenty a výstrahy zabezpečení seřazené podle priority.</span><span class="sxs-lookup"><span data-stu-id="7d19c-131">Provides prioritized security incidents/alerts</span></span> |
| <span data-ttu-id="7d19c-132">Reakce</span><span class="sxs-lookup"><span data-stu-id="7d19c-132">Respond</span></span> | <span data-ttu-id="7d19c-133">Nabízí podrobný náhled na zdroj hello hello útoku a zasažené prostředky.</span><span class="sxs-lookup"><span data-stu-id="7d19c-133">Offers insights into hello source of hello attack and impacted resources</span></span> |
| <span data-ttu-id="7d19c-134">Reakce</span><span class="sxs-lookup"><span data-stu-id="7d19c-134">Respond</span></span> | <span data-ttu-id="7d19c-135">Navrhuje způsoby toostop hello aktuální útok a pomáhá zabránit budoucím útokům.</span><span class="sxs-lookup"><span data-stu-id="7d19c-135">Suggests ways toostop hello current attack and help prevent future attacks</span></span> |

## <a name="introductory-walkthrough"></a><span data-ttu-id="7d19c-136">Úvodní prohlídka</span><span class="sxs-lookup"><span data-stu-id="7d19c-136">Introductory walkthrough</span></span>

> [!NOTE]
> <span data-ttu-id="7d19c-137">Toto téma představuje hello služby pomocí příklad nasazení.</span><span class="sxs-lookup"><span data-stu-id="7d19c-137">This document introduces hello service by using an example deployment.</span></span> <span data-ttu-id="7d19c-138">Tento dokument není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="7d19c-138">This document is not a step-by-step guide.</span></span>
>
>

 <span data-ttu-id="7d19c-139">Security Center je přístupná z hello [portál Azure](https://azure.microsoft.com/features/azure-portal/).</span><span class="sxs-lookup"><span data-stu-id="7d19c-139">You access Security Center from hello [Azure portal](https://azure.microsoft.com/features/azure-portal/).</span></span> <span data-ttu-id="7d19c-140">[Přihlaste se toohello portál](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7d19c-140">[Sign in toohello portal](https://portal.azure.com).</span></span> <span data-ttu-id="7d19c-141">Posuňte se v hlavní nabídce portálu hello toohello **Security Center** možnost nebo vyberte hello **Security Center** dlaždici dříve připnuli toohello řídicí panel portálu.</span><span class="sxs-lookup"><span data-stu-id="7d19c-141">Under hello main portal menu, scroll toohello **Security Center** option or select hello **Security Center** tile that you previously pinned toohello portal dashboard.</span></span>

![Dlaždice zabezpečení na portálu Azure][1]

<span data-ttu-id="7d19c-143">Ze služby Security Center můžete nastavovat zásady zabezpečení, sledovat konfigurace zabezpečení a zobrazovat výstrahy zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="7d19c-143">From Security Center, you can set security policies, monitor security configurations, and view security alerts.</span></span>

### <a name="security-policies"></a><span data-ttu-id="7d19c-144">Zásady zabezpečení</span><span class="sxs-lookup"><span data-stu-id="7d19c-144">Security policies</span></span>
<span data-ttu-id="7d19c-145">Můžete definovat zásady pro vaše předplatná Azure podle tooyour požadavky na zabezpečení společnosti.</span><span class="sxs-lookup"><span data-stu-id="7d19c-145">You can define policies for your Azure subscriptions according tooyour company's security requirements.</span></span> <span data-ttu-id="7d19c-146">Vám může také přizpůsobit toohello typy aplikací, které používáte nebo citlivosti toohello hello dat v každém předplatném.</span><span class="sxs-lookup"><span data-stu-id="7d19c-146">You can also tailor them toohello types of applications you're using or toohello sensitivity of hello data in each subscription.</span></span> <span data-ttu-id="7d19c-147">Například prostředky používané pro vývoj nebo testování můžou mít jiné požadavky na zabezpečení než ty, které se používají v aplikacích v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="7d19c-147">For example, resources used for development or testing may have different security requirements than those used for production applications.</span></span> <span data-ttu-id="7d19c-148">Aplikace pracující s regulovanými daty, třeba s osobními údaji, zase můžou vyžadovat jinou úroveň zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="7d19c-148">Likewise, applications with regulated data like PII may require a higher level of security.</span></span>

> [!NOTE]
> <span data-ttu-id="7d19c-149">toomodify zásady zabezpečení, musíte být vlastníkem nebo přispěvatelem správce zabezpečení nebo hello předplatného.</span><span class="sxs-lookup"><span data-stu-id="7d19c-149">toomodify a security policy, you must be a Security Administrator or hello subscription's Owner or Contributor.</span></span> <span data-ttu-id="7d19c-150">toolearn Další informace o rolí a povolených akcí v Centru zabezpečení, najdete v části [oprávnění v Azure Security Center](security-center-permissions.md).</span><span class="sxs-lookup"><span data-stu-id="7d19c-150">toolearn more about roles and allowed actions in Security Center, see [Permissions in Azure Security Center](security-center-permissions.md).</span></span>
>
>

<span data-ttu-id="7d19c-151">Na hello **Security Center** okně, vyberte hello **zásad** dlaždici seznam předplatných a skupin prostředků.</span><span class="sxs-lookup"><span data-stu-id="7d19c-151">On hello **Security Center** blade, select hello **Policy** tile for a list of your subscriptions and resource groups.</span></span>   

![Okno Security Center][2]

<span data-ttu-id="7d19c-153">Na hello **zásady zabezpečení** okně vyberte předplatné tooview hello zásad podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="7d19c-153">On hello **Security policy** blade, select a subscription tooview hello policy details.</span></span>

<span data-ttu-id="7d19c-154">**Shromažďování dat** umožňuje shromažďování dat pro zásady zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="7d19c-154">**Data collection** enables data collection for a security policy.</span></span> <span data-ttu-id="7d19c-155">Povolením získáte následující:</span><span class="sxs-lookup"><span data-stu-id="7d19c-155">Enabling provides:</span></span>

* <span data-ttu-id="7d19c-156">Denní kontrolu všech podporovaných virtuálních počítačů (VM) pro monitorování zabezpečení a doporučení.</span><span class="sxs-lookup"><span data-stu-id="7d19c-156">Daily scanning of all supported virtual machines (VMs) for security monitoring and recommendations.</span></span>
* <span data-ttu-id="7d19c-157">Shromažďování událostí zabezpečení pro analýzu a zjišťování hrozeb.</span><span class="sxs-lookup"><span data-stu-id="7d19c-157">Collection of security events for analysis and threat detection.</span></span>

> [!NOTE]
> <span data-ttu-id="7d19c-158">Shromažďování dat je konfigurována na úrovni předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="7d19c-158">Data collection is configured at hello subscription level.</span></span>
>
>

<span data-ttu-id="7d19c-159">Vyberte **zásada Zabránění** tooopen hello **zásada Zabránění** okno.</span><span class="sxs-lookup"><span data-stu-id="7d19c-159">Select **Prevention policy** tooopen hello **Prevention policy** blade.</span></span> <span data-ttu-id="7d19c-160">**Zobrazit doporučení pro** umožňuje vybrat hello ovládacích prvků zabezpečení, které chcete toomonitor a hello doporučení, které chcete toosee na základě potřeb zabezpečení hello hello prostředků v rámci předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="7d19c-160">**Show recommendations for** lets you choose hello security controls that you want toomonitor and hello recommendations that you want toosee based on hello security needs of hello resources within hello subscription.</span></span>

### <a name="security-recommendations"></a><span data-ttu-id="7d19c-161">Doporučení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="7d19c-161">Security recommendations</span></span>
 <span data-ttu-id="7d19c-162">Security Center analyzuje stav zabezpečení hello vaše prostředky Azure tooidentify potenciální ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="7d19c-162">Security Center analyzes hello security state of your Azure resources tooidentify potential security vulnerabilities.</span></span> <span data-ttu-id="7d19c-163">Seznam doporučení vás provede procesem konfigurace potřebných kontrol hello.</span><span class="sxs-lookup"><span data-stu-id="7d19c-163">A list of recommendations guides you through hello process of configuring needed controls.</span></span> <span data-ttu-id="7d19c-164">Příklady obsahují:</span><span class="sxs-lookup"><span data-stu-id="7d19c-164">Examples include:</span></span>

* <span data-ttu-id="7d19c-165">Zřizování antimalwaru, toohelp identifikovat a odebrat škodlivý software</span><span class="sxs-lookup"><span data-stu-id="7d19c-165">Provisioning antimalware toohelp identify and remove malicious software</span></span>
* <span data-ttu-id="7d19c-166">Konfigurace sítě zabezpečení skupiny a pravidel toocontrol provoz tooVMs</span><span class="sxs-lookup"><span data-stu-id="7d19c-166">Configuring network security groups and rules toocontrol traffic tooVMs</span></span>
* <span data-ttu-id="7d19c-167">Zřizování firewallů webových aplikací toohelp bránit proti útokům, které cílí na vaše webové aplikace</span><span class="sxs-lookup"><span data-stu-id="7d19c-167">Provisioning of web application firewalls toohelp defend against attacks that target your web applications</span></span>
* <span data-ttu-id="7d19c-168">Nasazení chybějících aktualizací systému</span><span class="sxs-lookup"><span data-stu-id="7d19c-168">Deploying missing system updates</span></span>
* <span data-ttu-id="7d19c-169">Adresování konfigurací operačního systému, které neodpovídají hello doporučená standardních hodnot</span><span class="sxs-lookup"><span data-stu-id="7d19c-169">Addressing OS configurations that do not match hello recommended baselines</span></span>

<span data-ttu-id="7d19c-170">Klikněte na tlačítko hello **doporučení** dlaždici seznam doporučení.</span><span class="sxs-lookup"><span data-stu-id="7d19c-170">Click hello **Recommendations** tile for a list of recommendations.</span></span> <span data-ttu-id="7d19c-171">Klikněte na každou doporučení tooview Další informace nebo tootake akce tooresolve hello problém.</span><span class="sxs-lookup"><span data-stu-id="7d19c-171">Click each recommendation tooview additional information or tootake action tooresolve hello issue.</span></span>

![Doporučení zabezpečení v Azure Security Center][5]

### <a name="security-state-of-azure-resources"></a><span data-ttu-id="7d19c-173">Stav zabezpečení prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="7d19c-173">Security state of Azure resources</span></span>
<span data-ttu-id="7d19c-174">Hello **prevence** část řídicího panelu hello hello ukazuje celkové postavení zabezpečení prostředí hello podle typů prostředků, včetně virtuálních počítačů, webových aplikací a dalším prostředkům.</span><span class="sxs-lookup"><span data-stu-id="7d19c-174">hello **Prevention** section of hello dashboard shows hello overall security posture of hello environment by resource type, including VMs, web applications, and other resources.</span></span>   

<span data-ttu-id="7d19c-175">Vyberte typ prostředku v rámci **prevence** tooview Další informace, včetně seznamu možných ohrožení zabezpečení, které byly identifikovány.</span><span class="sxs-lookup"><span data-stu-id="7d19c-175">Select a resource type under **Prevention** tooview more information, including a list of any potential security vulnerabilities that have been identified.</span></span> <span data-ttu-id="7d19c-176">(**Výpočetní** je vybraný v následujícím příkladu hello.)</span><span class="sxs-lookup"><span data-stu-id="7d19c-176">(**Compute** is selected in hello example below.)</span></span>

![Dlaždice Stav prostředků][6]

### <a name="security-alerts"></a><span data-ttu-id="7d19c-178">Výstrahy zabezpečení</span><span class="sxs-lookup"><span data-stu-id="7d19c-178">Security alerts</span></span>
 <span data-ttu-id="7d19c-179">Security Center automaticky shromažďuje, analyzuje a integruje data protokolu z vaše prostředky Azure, hello sítě a řešení partnerů, jako jsou antimalwarové programy a brány firewall.</span><span class="sxs-lookup"><span data-stu-id="7d19c-179">Security Center automatically collects, analyzes, and integrates log data from your Azure resources, hello network, and partner solutions like antimalware programs and firewalls.</span></span> <span data-ttu-id="7d19c-180">Při zjištění ohrožení zabezpečení se vytvoří výstraha zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="7d19c-180">When threats are detected, a security alert is created.</span></span> <span data-ttu-id="7d19c-181">Příklady zahrnují zjišťování následujících situací:</span><span class="sxs-lookup"><span data-stu-id="7d19c-181">Examples include detection of:</span></span>

* <span data-ttu-id="7d19c-182">Ohrožené virtuální počítače komunikaci s známé škodlivé IP adresy</span><span class="sxs-lookup"><span data-stu-id="7d19c-182">Compromised VMs communicating with known malicious IP addresses</span></span>
* <span data-ttu-id="7d19c-183">Pokročilý malware zjištěný pomocí zasílání zpráv o chybách systému Windows</span><span class="sxs-lookup"><span data-stu-id="7d19c-183">Advanced malware detected by using Windows error reporting</span></span>
* <span data-ttu-id="7d19c-184">Útoky hrubou silou na virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="7d19c-184">Brute force attacks against VMs</span></span>
* <span data-ttu-id="7d19c-185">Výstrahy zabezpečení z integrovaných antimalwarových programů a bran firewall</span><span class="sxs-lookup"><span data-stu-id="7d19c-185">Security alerts from integrated antimalware programs and firewalls</span></span>

<span data-ttu-id="7d19c-186">Kliknutím na hello **výstrahy zabezpečení** dlaždice zobrazí seznam výstrah seřazený podle priority.</span><span class="sxs-lookup"><span data-stu-id="7d19c-186">Clicking hello **Security alerts** tile displays a list of prioritized alerts.</span></span>

![Výstrahy zabezpečení][7]

<span data-ttu-id="7d19c-188">Výběrem výstrahy zobrazíte další informace o hello útoku a návrhy, jak tooremediate ho.</span><span class="sxs-lookup"><span data-stu-id="7d19c-188">Selecting an alert shows more information about hello attack and suggestions for how tooremediate it.</span></span>

![Podrobnosti výstrahy zabezpečení][8]

### <a name="partner-solutions"></a><span data-ttu-id="7d19c-190">Partnerská řešení</span><span class="sxs-lookup"><span data-stu-id="7d19c-190">Partner solutions</span></span>
<span data-ttu-id="7d19c-191">Hello **Partner solutions** dlaždice umožňuje sledovat na první pohled hello zabezpečení stav vašich partnerských řešení integrovaných ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="7d19c-191">hello **Partner solutions** tile lets you monitor at a glance hello security state of your partner solutions integrated with your Azure subscription.</span></span> <span data-ttu-id="7d19c-192">Security Center zobrazuje výstrahy, které pocházejí z hello řešení.</span><span class="sxs-lookup"><span data-stu-id="7d19c-192">Security Center displays alerts coming from hello solutions.</span></span>

<span data-ttu-id="7d19c-193">Vyberte hello **Partner solutions** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="7d19c-193">Select hello **Partner solutions** tile.</span></span> <span data-ttu-id="7d19c-194">Otevře se okno se seznamem všech připojených partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="7d19c-194">A blade opens displaying a list of all connected partner solutions.</span></span>

![Partnerská řešení][9]

## <a name="get-started"></a><span data-ttu-id="7d19c-196">Začínáme</span><span class="sxs-lookup"><span data-stu-id="7d19c-196">Get started</span></span>
<span data-ttu-id="7d19c-197">tooget začít s Security Center, je nutné tooMicrosoft předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="7d19c-197">tooget started with Security Center, you need a subscription tooMicrosoft Azure.</span></span> <span data-ttu-id="7d19c-198">Služba Security Center je povolená s vaším předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="7d19c-198">Security Center is enabled with your Azure subscription.</span></span> <span data-ttu-id="7d19c-199">Pokud nemáte předplatné, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7d19c-199">If you do not have a subscription, you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

 <span data-ttu-id="7d19c-200">Security Center je přístupná z hello [portál Azure](https://azure.microsoft.com/features/azure-portal/).</span><span class="sxs-lookup"><span data-stu-id="7d19c-200">You access Security Center from hello [Azure portal](https://azure.microsoft.com/features/azure-portal/).</span></span> <span data-ttu-id="7d19c-201">V tématu hello [portálu dokumentace](https://azure.microsoft.com/documentation/services/azure-portal/) toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="7d19c-201">See hello [portal documentation](https://azure.microsoft.com/documentation/services/azure-portal/) toolearn more.</span></span>

<span data-ttu-id="7d19c-202">[Začínáme s Azure Security Center](security-center-get-started.md) vás rychle provede hello monitorování zabezpečení a správu zásad součásti služby Security Center.</span><span class="sxs-lookup"><span data-stu-id="7d19c-202">[Getting started with Azure Security Center](security-center-get-started.md) quickly guides you through hello security-monitoring and policy-management components of Security Center.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7d19c-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7d19c-203">Next steps</span></span>
<span data-ttu-id="7d19c-204">V tomto dokumentu jste přináší tooSecurity Center, jejími klíčovými funkcemi a způsobu tooget spuštění.</span><span class="sxs-lookup"><span data-stu-id="7d19c-204">In this document, you were introduced tooSecurity Center, its key capabilities, and how tooget started.</span></span> <span data-ttu-id="7d19c-205">toolearn více, najdete v části hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="7d19c-205">toolearn more, see hello following resources:</span></span>

* <span data-ttu-id="7d19c-206">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – Další informace jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="7d19c-206">[Setting security policies in Azure Security Center](security-center-policies.md) — Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="7d19c-207">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="7d19c-207">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) — Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="7d19c-208">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="7d19c-208">[Security health monitoring in Azure Security Center](security-center-monitoring.md) — Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="7d19c-209">[Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – Další informace jak toomanage a reakce toosecurity výstrahy.</span><span class="sxs-lookup"><span data-stu-id="7d19c-209">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) — Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="7d19c-210">[Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="7d19c-210">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) — Learn how toomonitor hello health status of your partner solutions.</span></span>
- <span data-ttu-id="7d19c-211">[Zabezpečení dat v Azure Security Center](security-center-data-security.md) -další způsob správy a zabezpečení ve službě Security Center.</span><span class="sxs-lookup"><span data-stu-id="7d19c-211">[Azure Security Center data security](security-center-data-security.md) - Learn how data is managed and safeguarded in Security Center.</span></span>
* <span data-ttu-id="7d19c-212">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.</span><span class="sxs-lookup"><span data-stu-id="7d19c-212">[Azure Security Center FAQ](security-center-faq.md) — Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="7d19c-213">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – získejte nejnovější informace zabezpečení Azure hello a informace.</span><span class="sxs-lookup"><span data-stu-id="7d19c-213">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) — Get hello latest Azure security news and information.</span></span>

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
