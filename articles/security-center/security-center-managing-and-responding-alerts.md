---
title: "aaaManage výstrah zabezpečení v Azure Security Center | Microsoft Docs"
description: "Tento dokument pomáhá vám toouse Azure Security Center možnosti toomanage a reagovat toosecurity výstrahy."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: b88a8df7-6979-479b-8039-04da1b8737a7
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: yurid
ms.openlocfilehash: f1cb7e4770776827b75ed15893914678c1f44216
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-and-responding-toosecurity-alerts-in-azure-security-center"></a><span data-ttu-id="2f9b0-103">Správa a zpracování výstrah toosecurity v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="2f9b0-103">Managing and responding toosecurity alerts in Azure Security Center</span></span>
<span data-ttu-id="2f9b0-104">Tento dokument vám pomůže používat Azure Security Center toomanage a reagovat toosecurity výstrahy.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-104">This document helps you use Azure Security Center toomanage and respond toosecurity alerts.</span></span>

> [!NOTE]
> <span data-ttu-id="2f9b0-105">detekce tooenable rozšířené, upgradu tooAzure Standard Center zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-105">tooenable advanced detections, upgrade tooAzure Security Center Standard.</span></span> <span data-ttu-id="2f9b0-106">K dispozici je bezplatná 60denní zkušební verze.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-106">A free 60-day trial is available.</span></span> <span data-ttu-id="2f9b0-107">tooupgrade, vyberte cenová úroveň v hello [zásady zabezpečení](security-center-policies.md).</span><span class="sxs-lookup"><span data-stu-id="2f9b0-107">tooupgrade, select Pricing Tier in hello [Security Policy](security-center-policies.md).</span></span> <span data-ttu-id="2f9b0-108">V tématu [ceny Azure Security Center](security-center-pricing.md) toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-108">See [Azure Security Center pricing](security-center-pricing.md) toolearn more.</span></span>
>
>

## <a name="what-are-security-alerts"></a><span data-ttu-id="2f9b0-109">Co jsou výstrahy zabezpečení?</span><span class="sxs-lookup"><span data-stu-id="2f9b0-109">What are security alerts?</span></span>
<span data-ttu-id="2f9b0-110">Security Center automaticky shromažďuje, analyzuje a integruje data protokolu z vašich prostředků Azure, sítě hello připojené partnerských řešení, jako jsou brány firewall a endpoint protection řešení, toodetect skutečné hrozby a snížil počet falešných poplachů.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-110">Security Center automatically collects, analyzes, and integrates log data from your Azure resources, hello network, and connected partner solutions, like firewall and endpoint protection solutions, toodetect real threats and reduce false positives.</span></span> <span data-ttu-id="2f9b0-111">Seznam upřednostňovaných výstrah zabezpečení se zobrazí v Centru zabezpečení společně s hello informace, které potřebujete tooquickly prozkoumat hello problému a doporučení, jak tooremediate útoku.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-111">A list of prioritized security alerts is shown in Security Center along with hello information you need tooquickly investigate hello problem and recommendations for how tooremediate an attack.</span></span>


> [!NOTE]
> <span data-ttu-id="2f9b0-112">Další informace o tom, jak detekce služby Security Center pracuje, najdete v článku [Funkce detekce ve službě Azure Security Center](security-center-detection-capabilities.md).</span><span class="sxs-lookup"><span data-stu-id="2f9b0-112">For more information about how Security Center detection capabilities work, read [Azure Security Center Detection Capabilities](security-center-detection-capabilities.md).</span></span>
>
>

## <a name="managing-security-alerts"></a><span data-ttu-id="2f9b0-113">Správa výstrah zabezpečení</span><span class="sxs-lookup"><span data-stu-id="2f9b0-113">Managing security alerts</span></span>
<span data-ttu-id="2f9b0-114">Aktuální výstrahy můžete zkontrolovat prohlížením hello **výstrahy zabezpečení** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-114">You can review your current alerts by looking at hello **Security alerts** tile.</span></span> <span data-ttu-id="2f9b0-115">Otevřete portál Azure a postupujte podle kroků hello toosee další podrobnosti o každé výstraze:</span><span class="sxs-lookup"><span data-stu-id="2f9b0-115">Open Azure Portal and follow hello steps below toosee more details about each alert:</span></span>

1. <span data-ttu-id="2f9b0-116">Na řídicím panelu Security Center hello, uvidíte hello **výstrahy zabezpečení** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-116">On hello Security Center dashboard, you will see hello **Security alerts** tile.</span></span>

    ![Dlaždice Výstrahy zabezpečení ve službě Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig1-ga.png)

2. <span data-ttu-id="2f9b0-118">Klikněte na tlačítko hello dlaždice tooopen hello **výstrahy zabezpečení** okno, které obsahuje další podrobnosti o hello výstrahy, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-118">Click hello tile tooopen hello **Security alerts** blade that contains more details about hello alerts as shown below.</span></span>

   ![okno výstrahy zabezpečení Hello v Centru zabezpečení](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig2-ga.png)

<span data-ttu-id="2f9b0-120">V dolní části tohoto okna hello jsou hello podrobnosti pro každou výstrahu.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-120">In hello bottom part of this blade are hello details for each alert.</span></span> <span data-ttu-id="2f9b0-121">toosort, klikněte na tlačítko hello sloupec, který chcete toosort podle.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-121">toosort, click hello column that you want toosort by.</span></span> <span data-ttu-id="2f9b0-122">Hello definice pro každý sloupec je uvedená dále:</span><span class="sxs-lookup"><span data-stu-id="2f9b0-122">hello definition for each column is given below:</span></span>

* <span data-ttu-id="2f9b0-123">**Popis**: stručné vysvětlení výstrahy hello.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-123">**Description**: A brief explanation of hello alert.</span></span>
* <span data-ttu-id="2f9b0-124">**Count** (Počet): Seznam všech výstrah tohoto konkrétního typu, které byly zjištěny v určitý den.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-124">**Count**: A list of all alerts of this specific type that were detected on a specific day.</span></span>
* <span data-ttu-id="2f9b0-125">**Zjištěné podle**: hello služba, která je zodpovědná za aktivaci výstrahy hello.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-125">**Detected by**: hello service that was responsible for triggering hello alert.</span></span>
* <span data-ttu-id="2f9b0-126">**Datum**: hello datum tuto událost hello došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-126">**Date**: hello date that hello event occurred.</span></span>
* <span data-ttu-id="2f9b0-127">**Stav**: hello aktuální stav výstrahy.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-127">**State**: hello current state for that alert.</span></span> <span data-ttu-id="2f9b0-128">Existují dva typy stavů:</span><span class="sxs-lookup"><span data-stu-id="2f9b0-128">There are two types of states:</span></span>
  * <span data-ttu-id="2f9b0-129">**Aktivní**: byla zjištěna výstraha zabezpečení hello.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-129">**Active**: hello security alert has been detected.</span></span>
* <span data-ttu-id="2f9b0-130">**Závažnost**: úroveň závažnosti hello, což může být vysoká, střední nebo Nízká.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-130">**Severity**: hello severity level, which can be high, medium or low.</span></span>

### <a name="filtering-alerts"></a><span data-ttu-id="2f9b0-131">Filtrování výstrah</span><span class="sxs-lookup"><span data-stu-id="2f9b0-131">Filtering alerts</span></span>
<span data-ttu-id="2f9b0-132">Výstrahy můžete filtrovat podle data, stavu nebo závažnosti.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-132">You can filter alerts based on date, state, and severity.</span></span> <span data-ttu-id="2f9b0-133">Filtrování výstrah může být užitečná pro scénáře, kde je nutné toonarrow hello obor zobrazených výstrah zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-133">Filtering alerts can be useful for scenarios where you need toonarrow hello scope of security alerts show.</span></span> <span data-ttu-id="2f9b0-134">Například můžete má tooaddress výstrahy zabezpečení, které nastaly v hello posledních 24 hodin, protože zjišťujete případný průnik systému hello.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-134">For example, you might you want tooaddress security alerts that occurred in hello last 24 hours because you are investigating a potential breach in hello system.</span></span>

1. <span data-ttu-id="2f9b0-135">Klikněte na tlačítko **filtru** na hello **výstrahy zabezpečení** okno.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-135">Click **Filter** on hello **Security Alerts** blade.</span></span> <span data-ttu-id="2f9b0-136">Hello **filtru** otevře se okno a vyberte hodnoty data, stav a závažnost hello chcete toosee.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-136">hello **Filter** blade opens and you select hello date, state, and severity values you wish toosee.</span></span>

    ![Filtrování výstrah ve službě Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig3-2017.png)

### <a name="respond-toosecurity-alerts"></a><span data-ttu-id="2f9b0-138">Odpověď toosecurity výstrahy</span><span class="sxs-lookup"><span data-stu-id="2f9b0-138">Respond toosecurity alerts</span></span>
<span data-ttu-id="2f9b0-139">Vyberte toolearn výstrahy zabezpečení informace o hello událostí, který aktivoval výstrahu hello a co, pokud existuje, kroky je nutné tootake tooremediate útoku.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-139">Select a security alert toolearn more about hello event(s) that triggered hello alert and what, if any, steps you need tootake tooremediate an attack.</span></span> <span data-ttu-id="2f9b0-140">Výstrahy zabezpečení jsou seskupené podle typu a data.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-140">Security alerts are grouped by type and date.</span></span> <span data-ttu-id="2f9b0-141">Kliknutím na výstrahu zabezpečení se otevře okno obsahující seznam hello seskupených výstrah.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-141">Clicking a security alert will open a blade containing a list of hello grouped alerts.</span></span>

![Odpověď toosecurity výstrah v Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig5-ga.png)

<span data-ttu-id="2f9b0-143">V takovém případě hello vygenerované výstrahy naleznete toosuspicious aktivity protokolu RDP (Remote Desktop).</span><span class="sxs-lookup"><span data-stu-id="2f9b0-143">In this case, hello alerts that were triggered refer toosuspicious Remote Desktop Protocol (RDP) activity.</span></span> <span data-ttu-id="2f9b0-144">Hello první sloupec zobrazuje, které prostředky byly napadeny; Hello druhý zobrazuje kolikrát byl napaden hello prostředků; Hello třetí zobrazuje čas hello hello útoku; Hello čtvrtý zobrazuje stav výstrahy hello; a hello páté zobrazuje závažnost útoku hello hello.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-144">hello first column shows which resources were attacked; hello second shows how many times hello resource was attacked; hello third shows hello time of hello attack; hello fourth shows state of hello alert; and hello fifth shows hello severity of hello attack.</span></span> <span data-ttu-id="2f9b0-145">Po zkontrolování těchto informací klikněte na tlačítko hello prostředek, který byl napaden a otevře se nové okno.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-145">After reviewing this information, click hello resource that was attacked and a new blade will open.</span></span>

![Doporučení pro výstrahy co toodo o zabezpečení v Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig6-ga.png)

<span data-ttu-id="2f9b0-147">V hello **popis** tohoto okna najdete další podrobnosti o této události.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-147">In hello **Description** field of this blade you will find more details about this event.</span></span> <span data-ttu-id="2f9b0-148">Tyto další podrobnosti nabízejí získání náhledu na jaké spouštěná hello zabezpečení výstrah, hello cílový prostředek, pokud příslušné hello zdrojovou IP adresu a doporučení, jak tooremediate.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-148">These additional details offer insight into what triggered hello security alert, hello target resource, when applicable hello source IP address, and recommendations about how tooremediate.</span></span>  <span data-ttu-id="2f9b0-149">V některých případech hello Zdrojová IP adresa prázdná (není k dispozici) protože ne všechny protokoly událostí zabezpečení systému Windows zahrnovat hello IP adresu.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-149">In some instances, hello source IP address will be empty (not available) because not all Windows security events logs include hello IP address.</span></span>

<span data-ttu-id="2f9b0-150">Hello náprava navrhovaná službou Security Center se bude lišit podle výstrahy zabezpečení toohello.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-150">hello remediation suggested by Security Center will vary according toohello security alert.</span></span> <span data-ttu-id="2f9b0-151">V některých případech můžete mít toouse další možnosti Azure tooimplement hello doporučené nápravy.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-151">In some cases, you may have toouse other Azure capabilities tooimplement hello recommended remediation.</span></span> <span data-ttu-id="2f9b0-152">Například hello nápravy pro tento útok je tooblacklist hello IP adresu, která generuje tento útok pomocí [seznamu ACL sítě](../virtual-network/virtual-networks-acl.md) nebo [skupinu zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) pravidlo.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-152">For example, hello remediation for this attack is tooblacklist hello IP address that is generating this attack by using a [network ACL](../virtual-network/virtual-networks-acl.md) or a [network security group](../virtual-network/virtual-networks-nsg.md) rule.</span></span>

> [!NOTE]
> <span data-ttu-id="2f9b0-153">Další informace o hello různé typy výstrah, najdete v tématu [výstrahy zabezpečení podle typu v Azure Security Center](security-center-alerts-type.md).</span><span class="sxs-lookup"><span data-stu-id="2f9b0-153">For more information on hello different types of alerts, read [Security Alerts by Type in Azure Security Center](security-center-alerts-type.md).</span></span>
>
>

## <a name="see-also"></a><span data-ttu-id="2f9b0-154">Viz také</span><span class="sxs-lookup"><span data-stu-id="2f9b0-154">See also</span></span>
<span data-ttu-id="2f9b0-155">V tomto dokumentu jste se naučili jak tooconfigure zásady zabezpečení ve službě Security Center.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-155">In this document, you learned how tooconfigure security policies in Security Center.</span></span> <span data-ttu-id="2f9b0-156">toolearn Další informace o Security Center, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="2f9b0-156">toolearn more about Security Center, see hello following:</span></span>

* [<span data-ttu-id="2f9b0-157">Řešení bezpečnostních incidentů v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="2f9b0-157">Handling Security Incident in Azure Security Center</span></span>](security-center-incident.md)
* [<span data-ttu-id="2f9b0-158">Funkce detekce ve službě Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="2f9b0-158">Azure Security Center Detection Capabilities</span></span>](security-center-detection-capabilities.md)
* [<span data-ttu-id="2f9b0-159">Průvodce plánováním a provozem služby Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="2f9b0-159">Azure Security Center Planning and Operations Guide</span></span>](security-center-planning-and-operations-guide.md)
* <span data-ttu-id="2f9b0-160">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-160">[Azure Security Center FAQ](security-center-faq.md) — Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="2f9b0-161">[Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="2f9b0-161">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) — Find blog posts about Azure security and compliance.</span></span>
