---
title: "aaaMonitoring a odpovídá tooSecurity výstrahy v Operations Management Suite zabezpečení a Audit řešení | Microsoft Docs"
description: "Tento dokument pomáhá vám toouse hello threat intelligence možnost k dispozici v OMS zabezpečení a Audit toomonitor a reagovat toosecurity výstrahy."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 7d45a32b-1341-4bb5-a436-1f42a8a2590a
ms.service: operations-management-suite
ms.custom: oms-security
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/13/2017
ms.author: yurid
ms.openlocfilehash: 3d92b6809b7bd934c889afc119e5e34ff2b85f1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-and-responding-toosecurity-alerts-in-operations-management-suite-security-and-audit-solution"></a><span data-ttu-id="6a146-103">Monitorování a zpracování výstrah toosecurity v Operations Management Suite zabezpečení a Audit řešení</span><span class="sxs-lookup"><span data-stu-id="6a146-103">Monitoring and responding toosecurity alerts in Operations Management Suite Security and Audit Solution</span></span>
<span data-ttu-id="6a146-104">Tento dokument vám pomůže používat hello threat intelligence možnost k dispozici v OMS zabezpečení a Audit toomonitor a reagovat toosecurity výstrahy.</span><span class="sxs-lookup"><span data-stu-id="6a146-104">This document helps you use hello threat intelligence option available in OMS Security and Audit toomonitor and respond toosecurity alerts.</span></span>

## <a name="what-is-oms"></a><span data-ttu-id="6a146-105">Co je OMS?</span><span class="sxs-lookup"><span data-stu-id="6a146-105">What is OMS?</span></span>
<span data-ttu-id="6a146-106">Microsoft Operations Management Suite (OMS) je řešení pro správu IT, která pomáhá spravovat a chránit místní a cloudové infrastruktury založené na službě cloud společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="6a146-106">Microsoft Operations Management Suite (OMS) is Microsoft's cloud based IT management solution that helps you manage and protect your on-premises and cloud infrastructure.</span></span> <span data-ttu-id="6a146-107">Další informace o OMS, přečtěte si článek hello [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).</span><span class="sxs-lookup"><span data-stu-id="6a146-107">For more information about OMS, read hello article [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).</span></span>

## <a name="threat-intelligence"></a><span data-ttu-id="6a146-108">Analýza hrozeb</span><span class="sxs-lookup"><span data-stu-id="6a146-108">Threat intelligence</span></span>
<span data-ttu-id="6a146-109">V prostředí podniku, kde uživatelé mají sítě toohello rozsáhlý přístup a použití různých zařízení tooconnect toocorporate dat je nutné, můžete aktivně sledovat vaše prostředky a rychle reagovat toosecurity incidenty.</span><span class="sxs-lookup"><span data-stu-id="6a146-109">In an enterprise environment where users have broad access toohello network and use a variety of devices tooconnect toocorporate data, it is imperative that you can actively monitor your resources and quickly respond toosecurity incidents.</span></span> <span data-ttu-id="6a146-110">To je zvláště důležité z perspektivy životního cyklu hello zabezpečení, protože některé počítačové bezpečnosti, které hrozeb nemusí vyvolat výstrahy nebo podezřelé aktivity, které lze identifikovat podle tradiční technické ovládací prvky zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="6a146-110">This is particular important from hello security lifecycle perspective because some cybersecurity threats may not raise alerts or suspicious activities that can be identified by traditional security technical controls.</span></span> 

<span data-ttu-id="6a146-111">Pomocí hello **analýzou hrozeb** možnost k dispozici v OMS zabezpečení a Audit, správci IT může identifikovat bezpečnostní hrozby proti hello prostředí, například, zjistit, zda je součástí určitého počítače [ botnet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection).</span><span class="sxs-lookup"><span data-stu-id="6a146-111">By using hello **Threat Intelligence** option available in OMS Security and Audit, IT administrators can identify security threats against hello environment, for example, identify if a particular computer is part of a [botnet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection).</span></span> <span data-ttu-id="6a146-112">Uzly v botnet počítačů se může stát, když útočníci nedovoleným způsobem nainstaluje malware, který tajně připojí tento příkaz toohello počítače a řízení.</span><span class="sxs-lookup"><span data-stu-id="6a146-112">Computers can become nodes in a botnet when attackers illicitly install malware that secretly connects this computer toohello command and control.</span></span> <span data-ttu-id="6a146-113">Můžete také poznat potenciální hrozby pocházejících z podzemní komunikačních kanálů, jako například [darknet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection_honeypots_darkents).</span><span class="sxs-lookup"><span data-stu-id="6a146-113">It can also identify potential threats coming from underground communication channels, such as [darknet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection_honeypots_darkents).</span></span> 

<span data-ttu-id="6a146-114">V pořadí toobuild použít tento analýzou hrozeb, OMS zabezpečení a Audit dat pocházejících z více zdrojů v rámci Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="6a146-114">In order toobuild this threat intelligence, OMS Security and Audit use data coming from multiple sources within Microsoft.</span></span> <span data-ttu-id="6a146-115">OMS zabezpečení a Audit využije tato data tooidentify potenciálních hrozeb pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="6a146-115">OMS Security and Audit will leverage this data tooidentify potential threats against your environment.</span></span>

<span data-ttu-id="6a146-116">Hello analýzou hrozeb podokně tvoří podle tři hlavní možnosti:</span><span class="sxs-lookup"><span data-stu-id="6a146-116">hello Threat Intelligence pane is composed by three major options:</span></span>

* <span data-ttu-id="6a146-117">Servery s odchozím škodlivým provozem</span><span class="sxs-lookup"><span data-stu-id="6a146-117">Servers with outbound malicious traffic</span></span>
* <span data-ttu-id="6a146-118">Typy zjištěnými hrozbami</span><span class="sxs-lookup"><span data-stu-id="6a146-118">Detected threats types</span></span>
* <span data-ttu-id="6a146-119">Mapa analýzy hrozeb</span><span class="sxs-lookup"><span data-stu-id="6a146-119">Threat intelligence map</span></span>

> [!NOTE]
> <span data-ttu-id="6a146-120">Přehled všechny tyto možnosti najdete v tématu [Začínáme s Operations Management Suite zabezpečení a Audit řešení](oms-security-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="6a146-120">for an overview of all these options, read [Getting started with Operations Management Suite Security and Audit Solution](oms-security-getting-started.md).</span></span>
> 
> 

### <a name="responding-toosecurity-alerts"></a><span data-ttu-id="6a146-121">Odpovídá toosecurity výstrahy</span><span class="sxs-lookup"><span data-stu-id="6a146-121">Responding toosecurity alerts</span></span>
<span data-ttu-id="6a146-122">Jeden z hello kroky [reakcí na incidenty zabezpečení](https://technet.microsoft.com/library/cc512623.aspx) proces je tooidentify hello závažnost ohrožení systémy hello.</span><span class="sxs-lookup"><span data-stu-id="6a146-122">One of hello steps of a [security incident response](https://technet.microsoft.com/library/cc512623.aspx) process is tooidentify hello severity of hello compromise system(s).</span></span> <span data-ttu-id="6a146-123">V této fázi proveďte hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="6a146-123">In this phase you should perform hello following tasks:</span></span>

* <span data-ttu-id="6a146-124">Určení hello povahy útoku hello</span><span class="sxs-lookup"><span data-stu-id="6a146-124">Determine hello nature of hello attack</span></span>
* <span data-ttu-id="6a146-125">Určení místo pro útok hello původu</span><span class="sxs-lookup"><span data-stu-id="6a146-125">Determine hello attack point of origin</span></span>
* <span data-ttu-id="6a146-126">Určení hello záměr hello útoku.</span><span class="sxs-lookup"><span data-stu-id="6a146-126">Determine hello intent of hello attack.</span></span> <span data-ttu-id="6a146-127">Byl hello útoků, konkrétně zaměřen na konkrétní informace tooacquire organizace, nebo byl náhodných?</span><span class="sxs-lookup"><span data-stu-id="6a146-127">Was hello attack specifically directed at your organization tooacquire specific information, or was it random?</span></span>
* <span data-ttu-id="6a146-128">Určit hello systémy, které ohrožený</span><span class="sxs-lookup"><span data-stu-id="6a146-128">Identify hello systems that have been compromised</span></span>
* <span data-ttu-id="6a146-129">Identifikovat hello soubory, které mají přístup k a určení hello citlivosti těchto souborů</span><span class="sxs-lookup"><span data-stu-id="6a146-129">Identify hello files that have been accessed and determine hello sensitivity of those files</span></span>

<span data-ttu-id="6a146-130">Můžete využít **analýzou hrozeb** informace v OMS zabezpečení a Audit toohelp řešení těchto úloh.</span><span class="sxs-lookup"><span data-stu-id="6a146-130">You can leverage **Threat Intelligence** information in OMS Security and Audit solution toohelp with these tasks.</span></span> <span data-ttu-id="6a146-131">Postupujte podle kroků hello tooaccess, to **analýzou hrozeb** možnosti:</span><span class="sxs-lookup"><span data-stu-id="6a146-131">Follow hello steps below tooaccess this **Threat Intelligence** options:</span></span>

1. <span data-ttu-id="6a146-132">V hello **Microsoft Operations Management Suite** klikněte na hlavním řídicím **zabezpečení a Audit** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="6a146-132">In hello **Microsoft Operations Management Suite** main dashboard click **Security and Audit** tile.</span></span>
   
    ![Zabezpečení a Audit](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)
2. <span data-ttu-id="6a146-134">V hello **zabezpečení a Audit** řídicího panelu, zobrazí se hello **analýzou hrozeb** možnosti v pravé hello, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="6a146-134">In hello **Security and Audit** dashboard, you will see hello **Threat Intelligence** options in hello right, as shown below:</span></span>
   
    ![Intel hrozeb](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig2-ga.png)

<span data-ttu-id="6a146-136">Tyto tři dlaždice získáte přehled o aktuální ohrožení hello.</span><span class="sxs-lookup"><span data-stu-id="6a146-136">These three tiles will give you an overview of hello current threats.</span></span> <span data-ttu-id="6a146-137">V hello **serveru s odchozím škodlivým provozem** Pokud je jakýkoli počítač, který monitorujete (uvnitř nebo mimo síť) bude možné tooidentify tedy odesílání toohello škodlivý přenos Internetu.</span><span class="sxs-lookup"><span data-stu-id="6a146-137">In hello **Server with outbound malicious traffic** you will be able tooidentify if there is any computer that you are monitoring (inside or outside of your network) that is sending malicious traffic toohello Internet.</span></span> 

<span data-ttu-id="6a146-138">Hello **zjistila hrozba typy** dlaždice zobrazuje souhrnné údaje o hello hrozeb, které jsou aktuální "v divoký hello", pokud kliknete na tuto dlaždici se zobrazí další podrobnosti o těchto hrozbách jako zobrazit níže:</span><span class="sxs-lookup"><span data-stu-id="6a146-138">hello **Detected threat types** tile shows a summary of hello threats that are current “in hello wild”, if you click on this tile you will see more details about these threats as show below:</span></span>

![Zjištěné typy hrozeb](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig3.png)

<span data-ttu-id="6a146-140">Další informace o jednotlivé hrozby můžete rozbalit kliknutím na.</span><span class="sxs-lookup"><span data-stu-id="6a146-140">You can extract more information about each threat by clicking on it.</span></span> <span data-ttu-id="6a146-141">Následující příklad Hello ukazuje další podrobnosti o Botnet:</span><span class="sxs-lookup"><span data-stu-id="6a146-141">hello example below shows more details about Botnet:</span></span>

![Další informace o ohrožení](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig4.png)

<span data-ttu-id="6a146-143">Jak je popsáno v hello začátku této části, tato informace může být velmi užitečná při s případem reakcí na incidenty.</span><span class="sxs-lookup"><span data-stu-id="6a146-143">As described in hello beginning of this section, this information can be very useful during an incident response case.</span></span> <span data-ttu-id="6a146-144">Mohou také být důležité během forenzního vyšetřování, kde je nutné toofind hello zdroj hello útoku, který systém došlo k ohrožení a hello časové osy.</span><span class="sxs-lookup"><span data-stu-id="6a146-144">It can also be important during a forensic investigation, where you need toofind hello source of hello attack, which system was compromised and hello timeline.</span></span> <span data-ttu-id="6a146-145">V této sestavě mohli snadno identifikovat některé klíčové podrobnosti o hello útok, jako například: hello zdroj hello útoku, hello místní IP, došlo k ohrožení a hello aktuální relace stav připojení hello.</span><span class="sxs-lookup"><span data-stu-id="6a146-145">In this report you can easily identify some key details about hello attack, such as: hello source of hello attack, hello local IP that was compromised and hello current session state of hello connection.</span></span> 

<span data-ttu-id="6a146-146">Hello **threat intelligence mapy** vám pomůže tooidentify hello aktuální umístění kolem hello zeměkouli mající škodlivý přenos.</span><span class="sxs-lookup"><span data-stu-id="6a146-146">hello **threat intelligence map** will help you tooidentify hello current locations around hello globe that have malicious traffic.</span></span> <span data-ttu-id="6a146-147">Existují oranžová (příchozí) a červený (odchozí) šipky v této mapě identifikují hello směr provozu, pokud v jednom z těchto šipky kliknete, zobrazí hello typ hrozbě a hello směr provozu jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="6a146-147">There are orange (incoming) and red (outgoing) arrows in this map that identify hello traffic direction, if you click in one of these arrows, it will show hello type of threat and hello traffic direction as shown below:</span></span>

![mapa analýzy hrozeb](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig5.png)

> [!NOTE]
> <span data-ttu-id="6a146-149">Ukázka uvidíte o tom, jak toouse tato funkce během odpověď incidentu procesu Výukové video prezentace hello [zmírnit hrozby zabezpečení datového centra s asistencí šetření pomocí služby Operations Management Suite](https://myignite.microsoft.com/videos/5000) doručit na Microsoft Ignite.</span><span class="sxs-lookup"><span data-stu-id="6a146-149">You can see a demonstration on how toouse this capability during an incident response process by watching hello presentation [Mitigate datacenter security threats with guided investigation using Operations Management Suite](https://myignite.microsoft.com/videos/5000) delivered at Microsoft Ignite.</span></span>
> 

### <a name="responding-toodistinct-malicious-ip-accessed"></a><span data-ttu-id="6a146-150">Odpovídá toodistinct škodlivé IP získat přístup</span><span class="sxs-lookup"><span data-stu-id="6a146-150">Responding toodistinct malicious IP accessed</span></span>
<span data-ttu-id="6a146-151">V některých scénářích si můžete všimnout potenciálně škodlivé IP adresy, ke které přistupoval jeden monitorovaný počítač:</span><span class="sxs-lookup"><span data-stu-id="6a146-151">In some scenarios, you may notice a potential malicious IP that was accessed from one monitored computer:</span></span>

![mapa analýzy hrozeb](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig6.png)

<span data-ttu-id="6a146-153">Tato výstraha a ostatní uživatele v hello stejnou kategorií, jsou generovány prostřednictvím zabezpečení OMS s využitím [analýzou hrozeb Microsoftu](https://youtu.be/O4WtxgUrDc8).</span><span class="sxs-lookup"><span data-stu-id="6a146-153">This alert and others within hello same category, are generated via OMS Security by leveraging [Microsoft Threat Intelligence](https://youtu.be/O4WtxgUrDc8).</span></span> <span data-ttu-id="6a146-154">Hello analýzou hrozeb dat je shromažďovaná společností Microsoft a také zakoupených od začátku poskytovatelů intelligence hrozeb.</span><span class="sxs-lookup"><span data-stu-id="6a146-154">hello Threat Intelligence data is collected by Microsoft as well as purchased from leading threat intelligence providers.</span></span> <span data-ttu-id="6a146-155">Tato data se často aktualizuje a přizpůsobit přesunutí toofast hrozeb.</span><span class="sxs-lookup"><span data-stu-id="6a146-155">This data is updated frequently and adapted toofast-moving threats.</span></span> <span data-ttu-id="6a146-156">Z důvodu tooits povahy, měly být spojeny s další zdroje informací o zabezpečení při [příčin](https://blogs.technet.microsoft.com/msoms/2016/12/08/investigating-suspicious-activity-in-a-hybrid-cloud-with-oms-security/) výstrahy zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="6a146-156">Due tooits nature, it should be combined with other sources of security information while [investigating](https://blogs.technet.microsoft.com/msoms/2016/12/08/investigating-suspicious-activity-in-a-hybrid-cloud-with-oms-security/) a security alert.</span></span> 

## <a name="customize-alerts-received-via-e-mail"></a><span data-ttu-id="6a146-157">Přizpůsobení obdržel prostřednictvím e-mailových upozornění</span><span class="sxs-lookup"><span data-stu-id="6a146-157">Customize alerts received via e-mail</span></span>

<span data-ttu-id="6a146-158">Můžete přizpůsobit, kteří uživatelé ve vaší organizaci bude upozorněn výstrahy zabezpečení jsou aktivovány OMS zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="6a146-158">You can customize which users in your organization will be notified when security alerts are triggered by OMS Security.</span></span> <span data-ttu-id="6a146-159">Tato možnost je k dispozici v části Přehled / řídicí panel OMS hello nastavení na:</span><span class="sxs-lookup"><span data-stu-id="6a146-159">This option is available under Overview / Settings at hello OMS dashboard:</span></span>

![E-mail](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig7.png)

## <a name="see-also"></a><span data-ttu-id="6a146-161">Viz také</span><span class="sxs-lookup"><span data-stu-id="6a146-161">See also</span></span>
<span data-ttu-id="6a146-162">V tomto dokumentu jste se naučili jak toouse hello **analýzou hrozeb** možnost v OMS zabezpečení a Audit řešení toorespond toosecurity výstrahy.</span><span class="sxs-lookup"><span data-stu-id="6a146-162">In this document, you learned how toouse hello **Threat Intelligence** option in OMS Security and Audit solution toorespond toosecurity alerts.</span></span> <span data-ttu-id="6a146-163">Další informace o zabezpečení OMS toolearn najdete hello následující články:</span><span class="sxs-lookup"><span data-stu-id="6a146-163">toolearn more about OMS Security, see hello following articles:</span></span>

* [<span data-ttu-id="6a146-164">Přehled Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="6a146-164">Operations Management Suite (OMS) overview</span></span>](operations-management-suite-overview.md)
* [<span data-ttu-id="6a146-165">Začínáme se službou Operations Management Suite zabezpečení a Audit řešení</span><span class="sxs-lookup"><span data-stu-id="6a146-165">Getting started with Operations Management Suite Security and Audit Solution</span></span>](oms-security-getting-started.md)
* [<span data-ttu-id="6a146-166">Monitorování prostředků v řešení Zabezpečení a audit v Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="6a146-166">Monitoring Resources in Operations Management Suite Security and Audit Solution</span></span>](oms-security-monitoring-resources.md)

