---
title: aaaAzure sestavy intelligence threat Security Center | Microsoft Docs
description: "Tento dokument vám pomůže toouse Azure Security Center Threat inteligentního sestavy během šetření toofind Další informace týkající se výstraha zabezpečení."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 5662e312-e8c2-4736-974e-576eeb333484
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: c888cfac1dd8b057616a6b8e6c6f6b67b552f2e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-threat-intelligence-report"></a><span data-ttu-id="38a43-103">Sestava analýzy hrozeb v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="38a43-103">Azure Security Center Threat Intelligence Report</span></span>
<span data-ttu-id="38a43-104">Tento dokument vysvětluje, jakým způsobem vám mohou sestavy analýzy hrozeb v Azure Security Center pomoci zjistit více o hrozbě, který vygenerovala výstrahu zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="38a43-104">This document explains how Azure Security Center Threat Intelligent Reports can help you learn more about a threat that generated a security alert.</span></span>

## <a name="what-is-a-threat-intelligence-report"></a><span data-ttu-id="38a43-105">Co je sestava analýzy hrozeb?</span><span class="sxs-lookup"><span data-stu-id="38a43-105">What is a threat intelligence report?</span></span>
<span data-ttu-id="38a43-106">Detekce hrozeb Security Center funguje tak, že monitorování zabezpečení informací v prostředky Azure, sítě hello a připojených partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="38a43-106">Security Center threat detection works by monitoring security information from your Azure resources, hello network, and connected partner solutions.</span></span> <span data-ttu-id="38a43-107">Analyzuje tyto informace, často korelace informace z více zdrojů, tooidentify hrozeb.</span><span class="sxs-lookup"><span data-stu-id="38a43-107">It analyzes this information, often correlating information from multiple sources, tooidentify threats.</span></span> <span data-ttu-id="38a43-108">Tento proces je součástí hello Security Center [možností detekce](security-center-detection-capabilities.md).</span><span class="sxs-lookup"><span data-stu-id="38a43-108">This process is part of hello Security Center [detection capabilities](security-center-detection-capabilities.md).</span></span>

<span data-ttu-id="38a43-109">Když Security Center identifikuje hrozbu, aktivuje [výstrahu zabezpečení](security-center-managing-and-responding-alerts.md), která obsahuje podrobné informace týkající se konkrétní události, včetně návrhů na odstranění problémů.</span><span class="sxs-lookup"><span data-stu-id="38a43-109">When Security Center identifies a threat, it will trigger a [security alert](security-center-managing-and-responding-alerts.md), which contains detailed information regarding a particular event, including suggestions for remediation.</span></span> <span data-ttu-id="38a43-110">reakce na incidenty týmy tooassist prozkoumat ohrožení a oprava, Security Center obsahuje sestavu intelligence hrozba, která obsahuje informace o hello hrozba, která byla zjištěna, včetně informací, jako:</span><span class="sxs-lookup"><span data-stu-id="38a43-110">tooassist incident response teams investigate and remediate threats, Security Center includes a threat intelligence report that contains information about hello threat that was detected, including information such as the:</span></span>

* <span data-ttu-id="38a43-111">Identita nebo přidružení útočníka (pokud je tato informace k dispozici)</span><span class="sxs-lookup"><span data-stu-id="38a43-111">Attacker’s identity or associations (if this information is available)</span></span>
* <span data-ttu-id="38a43-112">Cíle útočníků</span><span class="sxs-lookup"><span data-stu-id="38a43-112">Attackers’ objectives</span></span>
* <span data-ttu-id="38a43-113">Současné a historické útočné kampaně (pokud je tato informace k dispozici)</span><span class="sxs-lookup"><span data-stu-id="38a43-113">Current and historical attack campaigns (if this information is available)</span></span>
* <span data-ttu-id="38a43-114">Taktika, nástroje a postupy útočníků</span><span class="sxs-lookup"><span data-stu-id="38a43-114">Attackers’ tactics, tools and procedures</span></span>
* <span data-ttu-id="38a43-115">Přidružené ukazatele ohrožení zabezpečení, například adresy URL a hodnoty hash souborů</span><span class="sxs-lookup"><span data-stu-id="38a43-115">Associated indicators of compromise (IoC) such as URLs and file hashes</span></span>
* <span data-ttu-id="38a43-116">Victimology, což je oborový hello a jejich zeměpisné rozšíření tooassist můžete při určování, zda vašich prostředků Azure jsou v ohrožení</span><span class="sxs-lookup"><span data-stu-id="38a43-116">Victimology, which is hello industry and geographic prevalence tooassist you in determining if your Azure resources are at risk</span></span>
* <span data-ttu-id="38a43-117">Informace o zmírnění a odstraňování problémů</span><span class="sxs-lookup"><span data-stu-id="38a43-117">Mitigation and remediation information</span></span>

> [!NOTE]
> <span data-ttu-id="38a43-118">Hello množství informací v konkrétní sestavách se bude lišit; Hello úroveň podrobností vychází z aktivity a jejich rozšíření hello malwaru.</span><span class="sxs-lookup"><span data-stu-id="38a43-118">hello amount of information in any particular report will vary; hello level of detail is based on hello malware’s activity and prevalence.</span></span>
>
>

<span data-ttu-id="38a43-119">Security Center má tři typy sestav hrozeb, které se může lišit podle toohello útoku.</span><span class="sxs-lookup"><span data-stu-id="38a43-119">Security Center has three types of threat reports, which can vary according toohello attack.</span></span> <span data-ttu-id="38a43-120">jsou k dispozici Hello sestavy:</span><span class="sxs-lookup"><span data-stu-id="38a43-120">hello reports available are:</span></span>

* <span data-ttu-id="38a43-121">**Sestava skupiny aktivit**: poskytuje podrobné informace o útočnících, jejich cílech a taktice.</span><span class="sxs-lookup"><span data-stu-id="38a43-121">**Activity Group Report**: provides deep dives into attackers, their objectives and tactics.</span></span>
* <span data-ttu-id="38a43-122">**Sestava kampaně**: zaměřuje se na podrobnosti o konkrétních útočných kampaních.</span><span class="sxs-lookup"><span data-stu-id="38a43-122">**Campaign Report**: focuses on details of specific attack campaigns.</span></span>
* <span data-ttu-id="38a43-123">**Souhrnná sestava hrozby**: obsahuje všechny položky hello v předchozí dva sestavách hello.</span><span class="sxs-lookup"><span data-stu-id="38a43-123">**Threat Summary Report**: covers all of hello items in hello previous two reports.</span></span>

<span data-ttu-id="38a43-124">Tento typ informací je velmi užitečná při hello [reakcí na incidenty](security-center-incident-response.md) procesy, kterých je zdrojem probíhající šetření toounderstand hello hello útoku, útočník hello motivace a co toodo toomitigate to problém postoupíte.</span><span class="sxs-lookup"><span data-stu-id="38a43-124">This type of information is very useful during hello [incident response](security-center-incident-response.md) process, where there is an ongoing investigation toounderstand hello source of hello attack, hello attacker’s motivations, and what toodo toomitigate this issue moving forward.</span></span>

## <a name="how-tooaccess-hello-threat-intelligence-report"></a><span data-ttu-id="38a43-125">Jak tooaccess hello threat intelligence sestavy?</span><span class="sxs-lookup"><span data-stu-id="38a43-125">How tooaccess hello threat intelligence report?</span></span>
<span data-ttu-id="38a43-126">Aktuální výstrahy můžete zkontrolovat prohlížením hello **výstrahy zabezpečení** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="38a43-126">You can review your current alerts by looking at hello **Security alerts** tile.</span></span> <span data-ttu-id="38a43-127">Otevřete hello portálu Azure a postupujte podle kroků hello toosee další podrobnosti o každé výstraze:</span><span class="sxs-lookup"><span data-stu-id="38a43-127">Open hello Azure Portal and follow hello steps below toosee more details about each alert:</span></span>

1. <span data-ttu-id="38a43-128">Na řídicím panelu Security Center hello, uvidíte hello **výstrahy zabezpečení** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="38a43-128">On hello Security Center dashboard, you will see hello **Security alerts** tile.</span></span>
2. <span data-ttu-id="38a43-129">Klikněte na tlačítko hello dlaždice tooopen hello **výstrahy zabezpečení** okno, které obsahuje další podrobnosti o výstrahách hello a klikněte na v hello výstrahu zabezpečení, které chcete tooobtain Další informace o.</span><span class="sxs-lookup"><span data-stu-id="38a43-129">Click hello tile tooopen hello **Security alerts** blade that contains more details about hello alerts and click in hello security alert that you want tooobtain more information about.</span></span>

    ![Výstrahy zabezpečení](./media/security-center-threat-report/security-center-threat-report-fig1.png)
3. <span data-ttu-id="38a43-131">V takovém případě hello **podezřelé proces spuštěný** hello podrobnosti o výstraze hello, jak je znázorněno na následujícím obrázku hello se zobrazí okno:</span><span class="sxs-lookup"><span data-stu-id="38a43-131">In this case hello **Suspicious process executed** blade shows hello details about hello alert as shown in hello figure below:</span></span>

    ![Podrobnosti výstrahy zabezpečení](./media/security-center-threat-report/security-center-threat-report-fig2.png)
4. <span data-ttu-id="38a43-133">Hello množství informací, které jsou k dispozici pro každou výstrahu zabezpečení se budou lišit podle typu toohello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="38a43-133">hello amount of information available for each security alert will vary according toohello type of alert.</span></span> <span data-ttu-id="38a43-134">V hello **sestavy** pole máte sestavu intelligence odkaz toohello hrozeb.</span><span class="sxs-lookup"><span data-stu-id="38a43-134">In hello **REPORTS** field you have a link toohello threat intelligence report.</span></span> <span data-ttu-id="38a43-135">Klikněte na něj a otevře se další okno prohlížeče se souborem PDF.</span><span class="sxs-lookup"><span data-stu-id="38a43-135">Click on it and another browser window will appear with PDF file.</span></span>

   ![Výběr úložiště](./media/security-center-threat-report/security-center-threat-report-fig3.png)

<span data-ttu-id="38a43-137">Odsud můžete stáhnout hello PDF pro tuto sestavu a číst informace o zabezpečení hello problém, která byla zjištěna a provádět akce založené na informacích hello.</span><span class="sxs-lookup"><span data-stu-id="38a43-137">From here you can download hello PDF for this report and read more about hello security issue that was detected and take actions based on hello information provided.</span></span>

## <a name="see-also"></a><span data-ttu-id="38a43-138">Viz také</span><span class="sxs-lookup"><span data-stu-id="38a43-138">See also</span></span>
<span data-ttu-id="38a43-139">V tomto dokumentu jste se dozvěděli, jak mohou sestavy analýzy hrozeb v Azure Security Center pomoci během vyšetřování výstrah zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="38a43-139">In this document, you learned how Azure Security Center Threat Intelligent Reports can help during an investigation about security alerts.</span></span> <span data-ttu-id="38a43-140">toolearn Další informace o službě Azure Security Center, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="38a43-140">toolearn more about Azure Security Center, see hello following:</span></span>

* <span data-ttu-id="38a43-141">[Azure Security Center – nejčastější dotazy](security-center-faq.md).</span><span class="sxs-lookup"><span data-stu-id="38a43-141">[Azure Security Center FAQ](security-center-faq.md).</span></span> <span data-ttu-id="38a43-142">Nejčastější dotazy o použití hello služby najít.</span><span class="sxs-lookup"><span data-stu-id="38a43-142">Find frequently asked questions about using hello service.</span></span>
* [<span data-ttu-id="38a43-143">Využití Azure Security Center při reakci na incidenty</span><span class="sxs-lookup"><span data-stu-id="38a43-143">Leveraging Azure Security Center for Incident Response</span></span>](security-center-incident-response.md)
* [<span data-ttu-id="38a43-144">Možnosti detekce v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="38a43-144">Azure Security Center detection capabilities</span></span>](security-center-detection-capabilities.md)
* <span data-ttu-id="38a43-145">[Průvodce plánováním a provozem služby Azure Security Center](security-center-planning-and-operations-guide.md).</span><span class="sxs-lookup"><span data-stu-id="38a43-145">[Azure Security Center planning and operations guide](security-center-planning-and-operations-guide.md).</span></span> <span data-ttu-id="38a43-146">Zjistěte, jak tooplan a pochopit hello návrhu aspekty tooadopt Azure Security Center.</span><span class="sxs-lookup"><span data-stu-id="38a43-146">Learn how tooplan and understand hello design considerations tooadopt Azure Security Center.</span></span>
* <span data-ttu-id="38a43-147">[Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="38a43-147">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md).</span></span> <span data-ttu-id="38a43-148">Zjistěte, jak toomanage a reakce toosecurity výstrahy.</span><span class="sxs-lookup"><span data-stu-id="38a43-148">Learn how toomanage and respond toosecurity alerts.</span></span>
* [<span data-ttu-id="38a43-149">Řešení bezpečnostních incidentů v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="38a43-149">Handling Security Incident in Azure Security Center</span></span>](security-center-incident.md)
* <span data-ttu-id="38a43-150">[Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/).</span><span class="sxs-lookup"><span data-stu-id="38a43-150">[Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/).</span></span> <span data-ttu-id="38a43-151">Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="38a43-151">Find blog posts about Azure security and compliance.</span></span>
