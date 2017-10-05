---
title: "Optimalizace prostředí služby Active Directory s Azure Log Analytics | Microsoft Docs"
description: "Active Directory Assessment řešení můžete použít k vyhodnocení rizik a stavu serveru prostředí v pravidelných intervalech."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 81eb41b8-eb62-4eb2-9f7b-fde5c89c9b47
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 97368f0b9e89ffd0cd982b6e8670d5a1f62ad42c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="optimize-your-active-directory-environment-with-the-active-directory-assessment-solution-in-log-analytics"></a><span data-ttu-id="48859-103">Optimalizace prostředí služby Active Directory s řešením Active Directory Assessment v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="48859-103">Optimize your Active Directory environment with the Active Directory Assessment solution in Log Analytics</span></span>

![Symbol hodnocení AD](./media/log-analytics-ad-assessment/ad-assessment-symbol.png)

<span data-ttu-id="48859-105">Active Directory Assessment řešení můžete použít k vyhodnocení rizik a stavu serveru prostředí v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="48859-105">You can use the Active Directory Assessment solution to assess the risk and health of your server environments on a regular interval.</span></span> <span data-ttu-id="48859-106">Tento článek vám pomůže nainstalovat a použít řešení tak, že lze provádět opravné akce pro potenciální problémy.</span><span class="sxs-lookup"><span data-stu-id="48859-106">This article helps you install and use the solution so that you can take corrective actions for potential problems.</span></span>

<span data-ttu-id="48859-107">Toto řešení poskytuje seznam doporučení, které jsou specifické pro infrastrukturu nasazené serverů seřazený podle priority.</span><span class="sxs-lookup"><span data-stu-id="48859-107">This solution provides a prioritized list of recommendations specific to your deployed server infrastructure.</span></span> <span data-ttu-id="48859-108">Doporučení jsou rozdělené mezi čtyři konkrétní oblasti, které vám pomůžou rychle pochopit riziko a provést akci.</span><span class="sxs-lookup"><span data-stu-id="48859-108">The recommendations are categorized across four focus areas, which help you quickly understand the risk and take action.</span></span>

<span data-ttu-id="48859-109">Doporučení jsou založené na znalosti a zkušenosti technici Microsoft z tisíce zákazníka návštěvách.</span><span class="sxs-lookup"><span data-stu-id="48859-109">The recommendations are based on the knowledge and experience gained by Microsoft engineers from thousands of customer visits.</span></span> <span data-ttu-id="48859-110">Každé doporučení obsahuje informace, proč může pro vás důležitá problém a implementaci navrhované změny.</span><span class="sxs-lookup"><span data-stu-id="48859-110">Each recommendation provides guidance about why an issue might matter to you and how to implement the suggested changes.</span></span>

<span data-ttu-id="48859-111">Můžete vybrat konkrétní oblasti, které jsou důležité pro vaši organizaci a sledovat průběh směrem k spuštění prostředí riziko volné a v pořádku.</span><span class="sxs-lookup"><span data-stu-id="48859-111">You can choose focus areas that are most important to your organization and track your progress toward running a risk free and healthy environment.</span></span>

<span data-ttu-id="48859-112">Když jste přidali řešení a posouzení je dokončené, souhrnné informace pro konkrétní oblasti je zobrazena na **hodnocení AD** řídicí panel infrastruktury ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="48859-112">After you've added the solution and an assessment is completed, summary information for focus areas is shown on the **AD Assessment** dashboard for the infrastructure in your environment.</span></span> <span data-ttu-id="48859-113">Následující části popisují, jak používat informace na **hodnocení AD** řídicí panel, kde můžete zobrazit a pak proveďte doporučené akce pro serverové infrastruktury služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="48859-113">The following sections describe how to use the information on the **AD Assessment** dashboard, where you can view and then take recommended actions for your Active Directory server infrastructure.</span></span>

![Obrázek dlaždice vyhodnocení SQL](./media/log-analytics-ad-assessment/ad-tile.png)

![bitové kopie řídicího panelu, vyhodnocení SQL](./media/log-analytics-ad-assessment/ad-assessment.png)

## <a name="installing-and-configuring-the-solution"></a><span data-ttu-id="48859-116">Instalace a konfigurace řešení</span><span class="sxs-lookup"><span data-stu-id="48859-116">Installing and configuring the solution</span></span>
<span data-ttu-id="48859-117">Použijte následující informace k instalaci a konfiguraci řešení.</span><span class="sxs-lookup"><span data-stu-id="48859-117">Use the following information to install and configure the solutions.</span></span>

* <span data-ttu-id="48859-118">Na řadičích domény, které jsou členy domény, který se má vyhodnotit, je nutné nainstalovat agenty.</span><span class="sxs-lookup"><span data-stu-id="48859-118">Agents must be installed on domain controllers that are members of the domain to be evaluated.</span></span>
* <span data-ttu-id="48859-119">Active Directory Assessment řešení vyžaduje podporovanou verzi rozhraní .NET Framework 4 (4.5.2 nebo novější) nainstalován na každý počítač, který má OMS agent.</span><span class="sxs-lookup"><span data-stu-id="48859-119">The Active Directory Assessment solution requires a supported version of .NET Framework 4 (4.5.2 or above) installed on each computer that has an OMS agent.</span></span>
* <span data-ttu-id="48859-120">Přidat řešení Active Directory Assessment do pracovního prostoru OMS z [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ADAssessmentOMS?tab=Overview) nebo pomocí procesu popsaného v tématu [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="48859-120">Add the Active Directory Assessment solution to your OMS workspace from [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ADAssessmentOMS?tab=Overview) or by using the process described in [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span>  <span data-ttu-id="48859-121">Není nutná žádná další konfigurace.</span><span class="sxs-lookup"><span data-stu-id="48859-121">There is no further configuration required.</span></span>

  > [!NOTE]
  > <span data-ttu-id="48859-122">Po přidání řešení, soubor AdvisorAssessment.exe je přidán na servery s agenty.</span><span class="sxs-lookup"><span data-stu-id="48859-122">After you've added the solution, the AdvisorAssessment.exe file is added to servers with agents.</span></span> <span data-ttu-id="48859-123">Konfigurační data je čtení a pak se odešle službě OMS v cloudu pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="48859-123">Configuration data is read and then sent to the OMS service in the cloud for processing.</span></span> <span data-ttu-id="48859-124">Logika se použije pro přijatá data a cloudové služby zaznamenává data.</span><span class="sxs-lookup"><span data-stu-id="48859-124">Logic is applied to the received data and the cloud service records the data.</span></span>
  >
  >

## <a name="active-directory-assessment-data-collection-details"></a><span data-ttu-id="48859-125">Informace o kolekci datových Active Directory hodnocení</span><span class="sxs-lookup"><span data-stu-id="48859-125">Active Directory Assessment data collection details</span></span>

<span data-ttu-id="48859-126">Active Directory Assessment shromažďuje data z agentů, které jste povolili pomocí následujících zdrojů:</span><span class="sxs-lookup"><span data-stu-id="48859-126">Active Directory Assessment collects data from the following sources using the agents that you have enabled:</span></span>

- <span data-ttu-id="48859-127">Kolektory registru</span><span class="sxs-lookup"><span data-stu-id="48859-127">Registry collectors</span></span>
- <span data-ttu-id="48859-128">Kolektory LDAP</span><span class="sxs-lookup"><span data-stu-id="48859-128">LDAP collectors</span></span>
- <span data-ttu-id="48859-129">Rozhraní .NET framework</span><span class="sxs-lookup"><span data-stu-id="48859-129">.NET Framework</span></span>
- <span data-ttu-id="48859-130">Kolektory protokolů událostí</span><span class="sxs-lookup"><span data-stu-id="48859-130">Event log collectors</span></span>
- <span data-ttu-id="48859-131">Služba Active Directory rozhraní (ADSI)</span><span class="sxs-lookup"><span data-stu-id="48859-131">Active Directory Service interfaces (ADSI)</span></span>
- <span data-ttu-id="48859-132">Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="48859-132">Windows PowerShell</span></span>
- <span data-ttu-id="48859-133">Kolekce dat souboru</span><span class="sxs-lookup"><span data-stu-id="48859-133">File data collectors</span></span>
- <span data-ttu-id="48859-134">Windows Management Instrumentation (WMI)</span><span class="sxs-lookup"><span data-stu-id="48859-134">Windows Management Instrumentation (WMI)</span></span>
- <span data-ttu-id="48859-135">Nástroj DCDIAG rozhraní API</span><span class="sxs-lookup"><span data-stu-id="48859-135">DCDIAG tool API</span></span>
- <span data-ttu-id="48859-136">Rozhraní API služby (NTFRS) replikace souborů</span><span class="sxs-lookup"><span data-stu-id="48859-136">File Replication Service (NTFRS) API</span></span>
- <span data-ttu-id="48859-137">Kód vlastní C#</span><span class="sxs-lookup"><span data-stu-id="48859-137">Custom C# code</span></span>


<span data-ttu-id="48859-138">Následující tabulka uvádí metody shromažďování dat pro agenty, jestli se vyžaduje Operations Manager (SCOM) a jak často data jsou shromažďována agentem.</span><span class="sxs-lookup"><span data-stu-id="48859-138">The following table shows data collection methods for agents, whether Operations Manager (SCOM) is required, and how often data is collected by an agent.</span></span>

| <span data-ttu-id="48859-139">Platforma</span><span class="sxs-lookup"><span data-stu-id="48859-139">platform</span></span> | <span data-ttu-id="48859-140">Přímé agenta</span><span class="sxs-lookup"><span data-stu-id="48859-140">Direct Agent</span></span> | <span data-ttu-id="48859-141">Agenta nástroje SCOM</span><span class="sxs-lookup"><span data-stu-id="48859-141">SCOM agent</span></span> | <span data-ttu-id="48859-142">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="48859-142">Azure Storage</span></span> | <span data-ttu-id="48859-143">SCOM vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="48859-143">SCOM required?</span></span> | <span data-ttu-id="48859-144">Data agenta SCOM odeslána prostřednictvím skupiny pro správu</span><span class="sxs-lookup"><span data-stu-id="48859-144">SCOM agent data sent via management group</span></span> | <span data-ttu-id="48859-145">Frekvence kolekce</span><span class="sxs-lookup"><span data-stu-id="48859-145">collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="48859-146">Windows</span><span class="sxs-lookup"><span data-stu-id="48859-146">Windows</span></span> |<span data-ttu-id="48859-147">&#8226;</span><span class="sxs-lookup"><span data-stu-id="48859-147">&#8226;</span></span> |<span data-ttu-id="48859-148">&#8226;</span><span class="sxs-lookup"><span data-stu-id="48859-148">&#8226;</span></span> |  |  |<span data-ttu-id="48859-149">&#8226;</span><span class="sxs-lookup"><span data-stu-id="48859-149">&#8226;</span></span> |<span data-ttu-id="48859-150">7 dní</span><span class="sxs-lookup"><span data-stu-id="48859-150">7 days</span></span> |

## <a name="understanding-how-recommendations-are-prioritized"></a><span data-ttu-id="48859-151">Pochopení, jak budou doporučení mít vyšší prioritu</span><span class="sxs-lookup"><span data-stu-id="48859-151">Understanding how recommendations are prioritized</span></span>
<span data-ttu-id="48859-152">Každé doporučení je zadána hodnota vyvážení, která určuje relativní důležitost doporučení.</span><span class="sxs-lookup"><span data-stu-id="48859-152">Every recommendation made is given a weighting value that identifies the relative importance of the recommendation.</span></span> <span data-ttu-id="48859-153">Jsou zobrazeny pouze 10 nejdůležitějších doporučení.</span><span class="sxs-lookup"><span data-stu-id="48859-153">Only the 10 most important recommendations are shown.</span></span>

### <a name="how-weights-are-calculated"></a><span data-ttu-id="48859-154">Jak jsou vypočítávány váhu</span><span class="sxs-lookup"><span data-stu-id="48859-154">How weights are calculated</span></span>
<span data-ttu-id="48859-155">Váhy jsou agregovaných hodnot založena na tři klíčové faktory:</span><span class="sxs-lookup"><span data-stu-id="48859-155">Weightings are aggregate values based on three key factors:</span></span>

* <span data-ttu-id="48859-156">*Pravděpodobnosti* , způsobuje problémy, problém identifikovat.</span><span class="sxs-lookup"><span data-stu-id="48859-156">The *probability* that an issue identified causes problems.</span></span> <span data-ttu-id="48859-157">Vyšší pravděpodobnost rovná větší celkové skóre pro doporučení.</span><span class="sxs-lookup"><span data-stu-id="48859-157">A higher probability equates to a larger overall score for the recommendation.</span></span>
* <span data-ttu-id="48859-158">*Dopad* problému na vaší organizaci, pokud ji způsobovat problémy.</span><span class="sxs-lookup"><span data-stu-id="48859-158">The *impact* of the issue on your organization if it does cause a problem.</span></span> <span data-ttu-id="48859-159">Vyšší dopad rovná větší celkové skóre pro doporučení.</span><span class="sxs-lookup"><span data-stu-id="48859-159">A higher impact equates to a larger overall score for the recommendation.</span></span>
* <span data-ttu-id="48859-160">*Úsilí* potřebnou k implementaci doporučení.</span><span class="sxs-lookup"><span data-stu-id="48859-160">The *effort* required to implement the recommendation.</span></span> <span data-ttu-id="48859-161">Vyšší úsilí rovná menší celkové skóre pro doporučení.</span><span class="sxs-lookup"><span data-stu-id="48859-161">A higher effort equates to a smaller overall score for the recommendation.</span></span>

<span data-ttu-id="48859-162">Vyvážení pro každé doporučení je vyjádřený jako procentní podíl celkové skóre, které jsou k dispozici pro každou oblast fokus.</span><span class="sxs-lookup"><span data-stu-id="48859-162">The weighting for each recommendation is expressed as a percentage of the total score available for each focus area.</span></span> <span data-ttu-id="48859-163">Například pokud doporučení v oblasti zabezpečení a dodržování předpisů fokus je skóre % 5, implementace tímto doporučením zvyšuje celkové skóre podle 5 % zabezpečení a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="48859-163">For example, if a recommendation in the Security and Compliance focus area has a score of 5%, implementing that recommendation increases your overall Security and Compliance score by 5%.</span></span>

### <a name="focus-areas"></a><span data-ttu-id="48859-164">Konkrétní oblasti</span><span class="sxs-lookup"><span data-stu-id="48859-164">Focus areas</span></span>
<span data-ttu-id="48859-165">**Zabezpečení a dodržování předpisů** – v tomto poli fokus zobrazí doporučení pro potenciální bezpečnostní hrozby a narušení, podnikové zásady a dodržování předpisů technických, právních i regulačních požadavků.</span><span class="sxs-lookup"><span data-stu-id="48859-165">**Security and Compliance** - This focus area shows recommendations for potential security threats and breaches, corporate policies, and technical, legal and regulatory compliance requirements.</span></span>

<span data-ttu-id="48859-166">**Dostupnost a provozní kontinuita** – v tomto poli fokus zobrazí doporučení pro dostupnost služeb, odolnost vaší infrastruktury a obchodní ochrany.</span><span class="sxs-lookup"><span data-stu-id="48859-166">**Availability and Business Continuity** - This focus area shows recommendations for service availability, resiliency of your infrastructure, and business protection.</span></span>

<span data-ttu-id="48859-167">**Výkon a škálovatelnost** – v tomto poli fokus zobrazí doporučení, která pomůžou vaší organizace IT infrastruktury růst, zajistěte, aby vaše IT prostředí splňuje aktuální požadavky na výkon a schopné reagovat na měnící se infrastruktury potřebuje.</span><span class="sxs-lookup"><span data-stu-id="48859-167">**Performance and Scalability** - This focus area shows recommendations to help your organization's IT infrastructure grow, ensure that your IT environment meets current performance requirements, and is able to respond to changing infrastructure needs.</span></span>

<span data-ttu-id="48859-168">**Upgrade, nasazení a migrace** – v tomto poli fokus zobrazí doporučení, která vám pomohou upgradovat, migrace a nasazení služby Active Directory vaší stávající infrastruktuře.</span><span class="sxs-lookup"><span data-stu-id="48859-168">**Upgrade, Migration and Deployment** - This focus area shows recommendations to help you upgrade, migrate, and deploy Active Directory to your existing infrastructure.</span></span>

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a><span data-ttu-id="48859-169">Mají usilovat o stanovení skóre 100 % v každé oblasti fokus?</span><span class="sxs-lookup"><span data-stu-id="48859-169">Should you aim to score 100% in every focus area?</span></span>
<span data-ttu-id="48859-170">Ne nutně.</span><span class="sxs-lookup"><span data-stu-id="48859-170">Not necessarily.</span></span> <span data-ttu-id="48859-171">Doporučení jsou založené na prostředí, které nebyly získány prostřednictvím Microsoft technici napříč tisíce návštěvy zákazníka a znalostní báze.</span><span class="sxs-lookup"><span data-stu-id="48859-171">The recommendations are based on the knowledge and experiences gained by Microsoft engineers across thousands of customer visits.</span></span> <span data-ttu-id="48859-172">Ale žádné serverové infrastruktury jsou stejné, a konkrétní doporučení může být vyšší nebo nižší relevantní pro vás.</span><span class="sxs-lookup"><span data-stu-id="48859-172">However, no two server infrastructures are the same, and specific recommendations may be more or less relevant to you.</span></span> <span data-ttu-id="48859-173">Například může být několik doporučení zabezpečení méně důležité, pokud vaše virtuální počítače nejsou vystaveny v Internetu.</span><span class="sxs-lookup"><span data-stu-id="48859-173">For example, some security recommendations might be less relevant if your virtual machines are not exposed to the Internet.</span></span> <span data-ttu-id="48859-174">Některá doporučení, jaké dostupnosti může být méně důležité pro služby, které poskytují kolekce s nízkou prioritou ad hoc dat a vytváření sestav.</span><span class="sxs-lookup"><span data-stu-id="48859-174">Some availability recommendations may be less relevant for services that provide low priority ad hoc data collection and reporting.</span></span> <span data-ttu-id="48859-175">Problémy, které jsou důležité, abyste vyspělých firmy může být méně důležité Startup.</span><span class="sxs-lookup"><span data-stu-id="48859-175">Issues that are important to a mature business may be less important to a start-up.</span></span> <span data-ttu-id="48859-176">Můžete určit, které konkrétní oblasti mají vašich priorit a podívejte se na tom, jak se vaše skóre časem změnit.</span><span class="sxs-lookup"><span data-stu-id="48859-176">You may want to identify which focus areas are your priorities and then look at how your scores change over time.</span></span>

<span data-ttu-id="48859-177">Každé doporučení obsahuje pokyny o tom, proč je důležité.</span><span class="sxs-lookup"><span data-stu-id="48859-177">Every recommendation includes guidance about why it is important.</span></span> <span data-ttu-id="48859-178">Měli byste použít tyto pokyny k vyhodnocení, zda implementace doporučení je vhodné pro vás, daná povaze vašich IT služeb a obchodním potřebám vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="48859-178">You should use this guidance to evaluate whether implementing the recommendation is appropriate for you, given the nature of your IT services and the business needs of your organization.</span></span>

## <a name="use-assessment-focus-area-recommendations"></a><span data-ttu-id="48859-179">Použít assessment fokus oblasti doporučení</span><span class="sxs-lookup"><span data-stu-id="48859-179">Use assessment focus area recommendations</span></span>
<span data-ttu-id="48859-180">Než v OMS můžete použít řešení pro vyhodnocení, musíte mít nainstalován řešení.</span><span class="sxs-lookup"><span data-stu-id="48859-180">Before you can use an assessment solution in OMS, you must have the solution installed.</span></span> <span data-ttu-id="48859-181">Další informace o instalaci řešení, najdete v části [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="48859-181">To read more about installing solutions, see [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span> <span data-ttu-id="48859-182">Po instalaci, zobrazí se souhrn doporučení pomocí dlaždice hodnocení na stránce Přehled v OMS.</span><span class="sxs-lookup"><span data-stu-id="48859-182">After it is installed, you can view the summary of recommendations by using the Assessment tile on the Overview page in OMS.</span></span>

<span data-ttu-id="48859-183">Zobrazte vyhodnocování souhrnné dodržování předpisů pro infrastrukturu a potom přejít k podrobnostem doporučení.</span><span class="sxs-lookup"><span data-stu-id="48859-183">View the summarized compliance assessments for your infrastructure and then drill-into recommendations.</span></span>

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a><span data-ttu-id="48859-184">Proveďte opravné akce a zobrazit doporučení pro oblastí zájmu</span><span class="sxs-lookup"><span data-stu-id="48859-184">To view recommendations for a focus area and take corrective action</span></span>
1. <span data-ttu-id="48859-185">Na **přehled** klikněte na tlačítko **Assessment** dlaždici pro váš server infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="48859-185">On the **Overview** page, click the **Assessment** tile for your server infrastructure.</span></span>
2. <span data-ttu-id="48859-186">Na **Assessment** zkontrolujte souhrnné informace v jednom z okna oblasti fokus a pak klikněte na jednu zobrazíte doporučení pro tuto oblast fokus.</span><span class="sxs-lookup"><span data-stu-id="48859-186">On the **Assessment** page, review the summary information in one of the focus area blades and then click one to view recommendations for that focus area.</span></span>
3. <span data-ttu-id="48859-187">Na všech stránkách oblasti fokus můžete zobrazit seřazený podle priority doporučení, která se pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="48859-187">On any of the focus area pages, you can view the prioritized recommendations made for your environment.</span></span> <span data-ttu-id="48859-188">Klikněte na tlačítko doporučení v části **vliv na objekty** Chcete-li zobrazit podrobnosti, proč se provádí doporučení.</span><span class="sxs-lookup"><span data-stu-id="48859-188">Click a recommendation under **Affected Objects** to view details about why the recommendation is made.</span></span>  
    <span data-ttu-id="48859-189">![Obrázek doporučení pro interní hodnocení](./media/log-analytics-ad-assessment/ad-focus.png)</span><span class="sxs-lookup"><span data-stu-id="48859-189">![image of Assessment recommendations](./media/log-analytics-ad-assessment/ad-focus.png)</span></span>
4. <span data-ttu-id="48859-190">Můžete provést nápravné akce navržený v **doporučované akce**.</span><span class="sxs-lookup"><span data-stu-id="48859-190">You can take corrective actions suggested in **Suggested Actions**.</span></span> <span data-ttu-id="48859-191">Když položka byla řešit, novější vyhodnocování záznamy, které doporučené akce provedené a zvýší vaše skóre dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="48859-191">When the item has been addressed, later assessments records that recommended actions were taken and your compliance score will increase.</span></span> <span data-ttu-id="48859-192">Opravené položky se zobrazí jako **předán objekty**.</span><span class="sxs-lookup"><span data-stu-id="48859-192">Corrected items appear as **Passed Objects**.</span></span>

## <a name="ignore-recommendations"></a><span data-ttu-id="48859-193">Ignorovat doporučení</span><span class="sxs-lookup"><span data-stu-id="48859-193">Ignore recommendations</span></span>
<span data-ttu-id="48859-194">Pokud máte doporučení, které chcete ignorovat, můžete vytvořit textový soubor, který OMS použije k zabránění doporučení ze storu ve výsledky hodnocení.</span><span class="sxs-lookup"><span data-stu-id="48859-194">If you have recommendations that you want to ignore, you can create a text file that OMS will use to prevent recommendations from appearing in your assessment results.</span></span>

### <a name="to-identify-recommendations-that-you-will-ignore"></a><span data-ttu-id="48859-195">K identifikaci doporučení, které se budou ignorovat.</span><span class="sxs-lookup"><span data-stu-id="48859-195">To identify recommendations that you will ignore</span></span>
1. <span data-ttu-id="48859-196">Přihlaste se do pracovního prostoru a otevřete vyhledávání protokolu.</span><span class="sxs-lookup"><span data-stu-id="48859-196">Sign in to your workspace and open Log Search.</span></span> <span data-ttu-id="48859-197">Pro počítače ve vašem prostředí použijte následující dotaz, který seznam doporučení, které selhaly.</span><span class="sxs-lookup"><span data-stu-id="48859-197">Use the following query to list recommendations that have failed for computers in your environment.</span></span>

   ```
   Type=ADAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```
>[!NOTE]
> <span data-ttu-id="48859-198">Pokud pracovní prostor byl upgradován na verzi [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak výše uvedeném dotazu by změnit na následující.</span><span class="sxs-lookup"><span data-stu-id="48859-198">If your workspace has been upgraded to the [new Log Analytics query language](log-analytics-log-search-upgrade.md), then the above query would change to the following.</span></span>
>
> `ADAssessmentRecommendation | where RecommendationResult == "Failed" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

   <span data-ttu-id="48859-199">Zde je snímek obrazovky zobrazující protokolu vyhledávací dotaz: ![doporučení se nezdařilo](./media/log-analytics-ad-assessment/ad-failed-recommendations.png)</span><span class="sxs-lookup"><span data-stu-id="48859-199">Here's a screen shot showing the Log Search query: ![failed recommendations](./media/log-analytics-ad-assessment/ad-failed-recommendations.png)</span></span>
2. <span data-ttu-id="48859-200">Zvolte doporučení, které chcete ignorovat.</span><span class="sxs-lookup"><span data-stu-id="48859-200">Choose recommendations that you want to ignore.</span></span> <span data-ttu-id="48859-201">Hodnoty pro RecommendationId budete používat v dalším postupu.</span><span class="sxs-lookup"><span data-stu-id="48859-201">You’ll use the values for RecommendationId in the next procedure.</span></span>

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a><span data-ttu-id="48859-202">Vytváření a používání textový soubor s IgnoreRecommendations.txt</span><span class="sxs-lookup"><span data-stu-id="48859-202">To create and use an IgnoreRecommendations.txt text file</span></span>
1. <span data-ttu-id="48859-203">Vytvořte soubor s názvem IgnoreRecommendations.txt.</span><span class="sxs-lookup"><span data-stu-id="48859-203">Create a file named IgnoreRecommendations.txt.</span></span>
2. <span data-ttu-id="48859-204">Vložit nebo zadejte každou RecommendationId pro jednotlivá doporučení, které chcete analýzy protokolů ignorovat na samostatném řádku a potom uložte a zavřete soubor.</span><span class="sxs-lookup"><span data-stu-id="48859-204">Paste or type each RecommendationId for each recommendation that you want Log Analytics to ignore on a separate line and then save and close the file.</span></span>
3. <span data-ttu-id="48859-205">Uložte soubor v následující složce na každém počítači, kam chcete OMS ignorovat doporučení.</span><span class="sxs-lookup"><span data-stu-id="48859-205">Put the file in the following folder on each computer where you want OMS to ignore recommendations.</span></span>
   * <span data-ttu-id="48859-206">Na počítačích s Microsoft Monitoring Agent (připojené přímo nebo prostřednictvím nástroje Operations Manager) - *SystemDrive*: \Program Files\Microsoft Agent\Agent monitorování</span><span class="sxs-lookup"><span data-stu-id="48859-206">On computers with the Microsoft Monitoring Agent (connected directly or through Operations Manager) - *SystemDrive*:\Program Files\Microsoft Monitoring Agent\Agent</span></span>
   * <span data-ttu-id="48859-207">Na serveru pro správu nástroje Operations Manager - *SystemDrive*: \Program Files\Microsoft System Center 2012 R2\Operations Manager\Server</span><span class="sxs-lookup"><span data-stu-id="48859-207">On the Operations Manager management server - *SystemDrive*:\Program Files\Microsoft System Center 2012 R2\Operations Manager\Server</span></span>

### <a name="to-verify-that-recommendations-are-ignored"></a><span data-ttu-id="48859-208">Chcete-li ověřit, že se ignorovat doporučení</span><span class="sxs-lookup"><span data-stu-id="48859-208">To verify that recommendations are ignored</span></span>
<span data-ttu-id="48859-209">Po na další naplánované vyhodnocení běží ve výchozím nastavení každých 7 dní, jsou označené zadanou doporučení *ignorovaná* a nezobrazí se na řídicím panelu hodnocení.</span><span class="sxs-lookup"><span data-stu-id="48859-209">After the next scheduled assessment runs, by default every 7 days, the specified recommendations are marked *Ignored* and will not appear on the assessment dashboard.</span></span>

1. <span data-ttu-id="48859-210">Následující dotazy hledání protokolů můžete použít k zobrazení seznamu všech ignorováno doporučení.</span><span class="sxs-lookup"><span data-stu-id="48859-210">You can use the following Log Search queries to list all the ignored recommendations.</span></span>

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```
>[!NOTE]
> <span data-ttu-id="48859-211">Pokud pracovní prostor byl upgradován na verzi [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak výše uvedeném dotazu by změnit na následující.</span><span class="sxs-lookup"><span data-stu-id="48859-211">If your workspace has been upgraded to the [new Log Analytics query language](log-analytics-log-search-upgrade.md), then the above query would change to the following.</span></span>
>
> `ADAssessmentRecommendation | where RecommendationResult == "Ignored" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

2. <span data-ttu-id="48859-212">Pokud se později rozhodnete, zda chcete zobrazit ignorováno doporučení, odeberte všechny soubory IgnoreRecommendations.txt nebo RecommendationIDs můžete odebrat z nich.</span><span class="sxs-lookup"><span data-stu-id="48859-212">If you decide later that you want to see ignored recommendations, remove any IgnoreRecommendations.txt files, or you can remove RecommendationIDs from them.</span></span>

## <a name="ad-assessment-solutions-faq"></a><span data-ttu-id="48859-213">Řešení hodnocení AD – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="48859-213">AD Assessment solutions FAQ</span></span>
<span data-ttu-id="48859-214">*Jak často posouzení spustit?*</span><span class="sxs-lookup"><span data-stu-id="48859-214">*How often does an assessment run?*</span></span>

* <span data-ttu-id="48859-215">Posouzení spouští každých 7 dní.</span><span class="sxs-lookup"><span data-stu-id="48859-215">The assessment runs every 7 days.</span></span>

<span data-ttu-id="48859-216">*Existuje způsob, jak nakonfigurovat, jak často se hodnocení spouštět?*</span><span class="sxs-lookup"><span data-stu-id="48859-216">*Is there a way to configure how often the assessment runs?*</span></span>

* <span data-ttu-id="48859-217">V tuto chvíli to není možné.</span><span class="sxs-lookup"><span data-stu-id="48859-217">Not at this time.</span></span>

<span data-ttu-id="48859-218">*Pokud jiný server pro je zjištěno po byly přidány řešení pro vyhodnocení, bude ho vyhodnocena?*</span><span class="sxs-lookup"><span data-stu-id="48859-218">*If another server for is discovered after I’ve added an assessment solution, will it be assessed?*</span></span>

* <span data-ttu-id="48859-219">Ano, jakmile se zjistí, že se hodnotí z pak, každých 7 dní.</span><span class="sxs-lookup"><span data-stu-id="48859-219">Yes, once it is discovered it is assessed from then on, every 7 days.</span></span>

<span data-ttu-id="48859-220">*Pokud dojde k deaktivaci serveru, když ho se odebere z hodnocení?*</span><span class="sxs-lookup"><span data-stu-id="48859-220">*If a server is decommissioned, when will it be removed from the assessment?*</span></span>

* <span data-ttu-id="48859-221">Pokud server není odesílání dat 3 týdny, bude odebrán.</span><span class="sxs-lookup"><span data-stu-id="48859-221">If a server does not submit data for 3 weeks, it is removed.</span></span>

<span data-ttu-id="48859-222">*Jaký je název procesu, který nemá shromažďování dat?*</span><span class="sxs-lookup"><span data-stu-id="48859-222">*What is the name of the process that does the data collection?*</span></span>

* <span data-ttu-id="48859-223">AdvisorAssessment.exe</span><span class="sxs-lookup"><span data-stu-id="48859-223">AdvisorAssessment.exe</span></span>

<span data-ttu-id="48859-224">*Jak dlouho trvá na shromáždění dat?*</span><span class="sxs-lookup"><span data-stu-id="48859-224">*How long does it take for data to be collected?*</span></span>

* <span data-ttu-id="48859-225">Kolekce skutečná data na serveru trvá asi 1 hodina.</span><span class="sxs-lookup"><span data-stu-id="48859-225">The actual data collection on the server takes about 1 hour.</span></span> <span data-ttu-id="48859-226">Může trvat déle na serverech, které mají velký počet servery služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="48859-226">It may take longer on servers that have a large number of Active Directory servers.</span></span>

<span data-ttu-id="48859-227">*Existuje způsob, jak nakonfigurovat, když jsou shromažďována data?*</span><span class="sxs-lookup"><span data-stu-id="48859-227">*Is there a way to configure when data is collected?*</span></span>

* <span data-ttu-id="48859-228">V tuto chvíli to není možné.</span><span class="sxs-lookup"><span data-stu-id="48859-228">Not at this time.</span></span>

<span data-ttu-id="48859-229">*Proč zobrazit pouze prvních 10 doporučení?*</span><span class="sxs-lookup"><span data-stu-id="48859-229">*Why display only the top 10 recommendations?*</span></span>

* <span data-ttu-id="48859-230">Namísto udělení vyčerpávající čtenáře seznam úloh, doporučujeme můžete soustředit na první adresování seřazený podle priority doporučení.</span><span class="sxs-lookup"><span data-stu-id="48859-230">Instead of giving you an exhaustive overwhelming list of tasks, we recommend that you focus on addressing the prioritized recommendations first.</span></span> <span data-ttu-id="48859-231">Po jejich řešení, bude k dispozici další doporučení.</span><span class="sxs-lookup"><span data-stu-id="48859-231">After you address them, additional recommendations will become available.</span></span> <span data-ttu-id="48859-232">Pokud ji chcete zobrazit podrobný seznam, můžete zobrazit všechna doporučení pomocí hledání protokolů.</span><span class="sxs-lookup"><span data-stu-id="48859-232">If you prefer to see the detailed list, you can view all recommendations using Log Search.</span></span>

<span data-ttu-id="48859-233">*Existuje způsob, jak ignorovat doporučení?*</span><span class="sxs-lookup"><span data-stu-id="48859-233">*Is there a way to ignore a recommendation?*</span></span>

* <span data-ttu-id="48859-234">Ano, najdete v části [ignorovat doporučení](#ignore-recommendations) část výše.</span><span class="sxs-lookup"><span data-stu-id="48859-234">Yes, see [Ignore recommendations](#ignore-recommendations) section above.</span></span>

## <a name="next-steps"></a><span data-ttu-id="48859-235">Další kroky</span><span class="sxs-lookup"><span data-stu-id="48859-235">Next steps</span></span>
* <span data-ttu-id="48859-236">Použití [přihlásit analýzy protokolů hledání](log-analytics-log-searches.md) zobrazíte podrobné dat hodnocení AD a doporučení.</span><span class="sxs-lookup"><span data-stu-id="48859-236">Use [Log searches in Log Analytics](log-analytics-log-searches.md) to view detailed AD Assessment data and recommendations.</span></span>
