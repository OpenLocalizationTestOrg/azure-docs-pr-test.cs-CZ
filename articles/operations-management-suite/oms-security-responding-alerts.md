---
title: "Monitorování a zpracování výstrah zabezpečení v Operations Management Suite zabezpečení a Audit řešení | Microsoft Docs"
description: "Tento dokument vám umožňuje používat intelligence možnost hrozeb, které jsou k dispozici v OMS zabezpečení a Audit k monitorování a reakce na výstrahy zabezpečení."
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
ms.openlocfilehash: 0cf9b83d7023641ec445a59a5e61d3da038695fa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="monitoring-and-responding-to-security-alerts-in-operations-management-suite-security-and-audit-solution"></a><span data-ttu-id="41143-103">Monitorování a zpracování výstrah zabezpečení v Operations Management Suite zabezpečení a Audit řešení</span><span class="sxs-lookup"><span data-stu-id="41143-103">Monitoring and responding to security alerts in Operations Management Suite Security and Audit Solution</span></span>
<span data-ttu-id="41143-104">Tento dokument vám pomůže používat intelligence možnost hrozeb, které jsou k dispozici v OMS zabezpečení a Audit k monitorování a reakce na výstrahy zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="41143-104">This document helps you use the threat intelligence option available in OMS Security and Audit to monitor and respond to security alerts.</span></span>

## <a name="what-is-oms"></a><span data-ttu-id="41143-105">Co je OMS?</span><span class="sxs-lookup"><span data-stu-id="41143-105">What is OMS?</span></span>
<span data-ttu-id="41143-106">Microsoft Operations Management Suite (OMS) je řešení pro správu IT, která pomáhá spravovat a chránit místní a cloudové infrastruktury založené na službě cloud společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="41143-106">Microsoft Operations Management Suite (OMS) is Microsoft's cloud based IT management solution that helps you manage and protect your on-premises and cloud infrastructure.</span></span> <span data-ttu-id="41143-107">Další informace o OMS najdete v článku [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).</span><span class="sxs-lookup"><span data-stu-id="41143-107">For more information about OMS, read the article [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).</span></span>

## <a name="threat-intelligence"></a><span data-ttu-id="41143-108">Analýza hrozeb</span><span class="sxs-lookup"><span data-stu-id="41143-108">Threat intelligence</span></span>
<span data-ttu-id="41143-109">V prostředí podniku, kde uživatelé mají široký přístup k síti a použít na široké škále zařízení pro připojení k firemní data je nutné, můžete aktivně sledovat vaše prostředky a rychle reagovat na incidenty zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="41143-109">In an enterprise environment where users have broad access to the network and use a variety of devices to connect to corporate data, it is imperative that you can actively monitor your resources and quickly respond to security incidents.</span></span> <span data-ttu-id="41143-110">To je zvláště důležité z perspektivy životního cyklu zabezpečení, protože některé počítačové bezpečnosti, které hrozeb nemusí vyvolat výstrahy nebo podezřelé aktivity, které lze identifikovat podle tradiční technické ovládací prvky zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="41143-110">This is particular important from the security lifecycle perspective because some cybersecurity threats may not raise alerts or suspicious activities that can be identified by traditional security technical controls.</span></span> 

<span data-ttu-id="41143-111">Pomocí **analýzou hrozeb** možnost k dispozici v OMS zabezpečení a Audit, správci IT může identifikovat bezpečnostních hrozeb v prostředí, například, zjistit, zda je součástí určitého počítače [botnet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection).</span><span class="sxs-lookup"><span data-stu-id="41143-111">By using the **Threat Intelligence** option available in OMS Security and Audit, IT administrators can identify security threats against the environment, for example, identify if a particular computer is part of a [botnet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection).</span></span> <span data-ttu-id="41143-112">Z počítačů se můžou stát uzly v botnetu, když útočníci neoprávněně nainstalují malware, který tajně připojí počítač k řídicímu serveru.</span><span class="sxs-lookup"><span data-stu-id="41143-112">Computers can become nodes in a botnet when attackers illicitly install malware that secretly connects this computer to the command and control.</span></span> <span data-ttu-id="41143-113">Můžete také poznat potenciální hrozby pocházejících z podzemní komunikačních kanálů, jako například [darknet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection_honeypots_darkents).</span><span class="sxs-lookup"><span data-stu-id="41143-113">It can also identify potential threats coming from underground communication channels, such as [darknet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection_honeypots_darkents).</span></span> 

<span data-ttu-id="41143-114">Chcete-li vytvořit tuto analýzou hrozeb, OMS zabezpečení a Audit pomocí dat pocházejících z více zdrojů v rámci Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="41143-114">In order to build this threat intelligence, OMS Security and Audit use data coming from multiple sources within Microsoft.</span></span> <span data-ttu-id="41143-115">OMS zabezpečení a Audit bude tyto údaje využijte k identifikaci potenciálních hrozeb pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="41143-115">OMS Security and Audit will leverage this data to identify potential threats against your environment.</span></span>

<span data-ttu-id="41143-116">Podokno Analýza hrozeb se skládá ze tří hlavních možností:</span><span class="sxs-lookup"><span data-stu-id="41143-116">The Threat Intelligence pane is composed by three major options:</span></span>

* <span data-ttu-id="41143-117">Servery s odchozím škodlivým provozem</span><span class="sxs-lookup"><span data-stu-id="41143-117">Servers with outbound malicious traffic</span></span>
* <span data-ttu-id="41143-118">Typy zjištěnými hrozbami</span><span class="sxs-lookup"><span data-stu-id="41143-118">Detected threats types</span></span>
* <span data-ttu-id="41143-119">Mapa analýzy hrozeb</span><span class="sxs-lookup"><span data-stu-id="41143-119">Threat intelligence map</span></span>

> [!NOTE]
> <span data-ttu-id="41143-120">Přehled všechny tyto možnosti najdete v tématu [Začínáme s Operations Management Suite zabezpečení a Audit řešení](oms-security-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="41143-120">for an overview of all these options, read [Getting started with Operations Management Suite Security and Audit Solution](oms-security-getting-started.md).</span></span>
> 
> 

### <a name="responding-to-security-alerts"></a><span data-ttu-id="41143-121">Zpracování výstrah zabezpečení</span><span class="sxs-lookup"><span data-stu-id="41143-121">Responding to security alerts</span></span>
<span data-ttu-id="41143-122">Mezi jednotlivými kroky [reakcí na incidenty zabezpečení](https://technet.microsoft.com/library/cc512623.aspx) proces je identifikace závažnost ohrožení systémy.</span><span class="sxs-lookup"><span data-stu-id="41143-122">One of the steps of a [security incident response](https://technet.microsoft.com/library/cc512623.aspx) process is to identify the severity of the compromise system(s).</span></span> <span data-ttu-id="41143-123">V této fázi měli provádět následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="41143-123">In this phase you should perform the following tasks:</span></span>

* <span data-ttu-id="41143-124">Určit povahu útoku.</span><span class="sxs-lookup"><span data-stu-id="41143-124">Determine the nature of the attack</span></span>
* <span data-ttu-id="41143-125">Určit, odkud útok přišel.</span><span class="sxs-lookup"><span data-stu-id="41143-125">Determine the attack point of origin</span></span>
* <span data-ttu-id="41143-126">Určit záměr útoku.</span><span class="sxs-lookup"><span data-stu-id="41143-126">Determine the intent of the attack.</span></span> <span data-ttu-id="41143-127">Směřoval útok konkrétně na vaši organizaci za účelem získání určitých informací, nebo se jednalo o náhodný útok?</span><span class="sxs-lookup"><span data-stu-id="41143-127">Was the attack specifically directed at your organization to acquire specific information, or was it random?</span></span>
* <span data-ttu-id="41143-128">Identifikovat napadené systémy.</span><span class="sxs-lookup"><span data-stu-id="41143-128">Identify the systems that have been compromised</span></span>
* <span data-ttu-id="41143-129">Označení souborů, které mají přístup k a zjistit, zda rozlišuje těchto souborů</span><span class="sxs-lookup"><span data-stu-id="41143-129">Identify the files that have been accessed and determine the sensitivity of those files</span></span>

<span data-ttu-id="41143-130">Můžete využít **analýzou hrozeb** informace v OMS zabezpečení a Audit řešení umožňující těchto úloh.</span><span class="sxs-lookup"><span data-stu-id="41143-130">You can leverage **Threat Intelligence** information in OMS Security and Audit solution to help with these tasks.</span></span> <span data-ttu-id="41143-131">Použijte následující postup pro přístup k: **analýzou hrozeb** možnosti:</span><span class="sxs-lookup"><span data-stu-id="41143-131">Follow the steps below to access this **Threat Intelligence** options:</span></span>

1. <span data-ttu-id="41143-132">Na hlavním řídicím panelu **Microsoft Operations Management Suite** klikněte na dlaždici **Zabezpečení a audit**.</span><span class="sxs-lookup"><span data-stu-id="41143-132">In the **Microsoft Operations Management Suite** main dashboard click **Security and Audit** tile.</span></span>
   
    ![Zabezpečení a Audit](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)
2. <span data-ttu-id="41143-134">V **zabezpečení a Audit** řídicího panelu, zobrazí se **analýzou hrozeb** možnosti v pravém, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="41143-134">In the **Security and Audit** dashboard, you will see the **Threat Intelligence** options in the right, as shown below:</span></span>
   
    ![Intel hrozeb](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig2-ga.png)

<span data-ttu-id="41143-136">Tyto tři dlaždice získáte přehled o aktuální ohrožení.</span><span class="sxs-lookup"><span data-stu-id="41143-136">These three tiles will give you an overview of the current threats.</span></span> <span data-ttu-id="41143-137">V **serveru s odchozím škodlivým provozem** bude možné zjistit, zda je jakýkoli počítač, který monitorujete (uvnitř nebo mimo síť), je odesílání škodlivý přenos k Internetu.</span><span class="sxs-lookup"><span data-stu-id="41143-137">In the **Server with outbound malicious traffic** you will be able to identify if there is any computer that you are monitoring (inside or outside of your network) that is sending malicious traffic to the Internet.</span></span> 

<span data-ttu-id="41143-138">**Zjistila hrozba typy** dlaždice zobrazuje souhrnné údaje o hrozeb, které jsou aktuální "v zástupné", pokud kliknete na tuto dlaždici se zobrazí další podrobnosti o těchto hrozbách jako zobrazit níže:</span><span class="sxs-lookup"><span data-stu-id="41143-138">The **Detected threat types** tile shows a summary of the threats that are current “in the wild”, if you click on this tile you will see more details about these threats as show below:</span></span>

![Zjištěné typy hrozeb](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig3.png)

<span data-ttu-id="41143-140">Další informace o jednotlivé hrozby můžete rozbalit kliknutím na.</span><span class="sxs-lookup"><span data-stu-id="41143-140">You can extract more information about each threat by clicking on it.</span></span> <span data-ttu-id="41143-141">Následující příklad ukazuje další podrobnosti o Botnet:</span><span class="sxs-lookup"><span data-stu-id="41143-141">The example below shows more details about Botnet:</span></span>

![Další informace o ohrožení](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig4.png)

<span data-ttu-id="41143-143">Jak je popsáno na začátku této části, tato informace může být velmi užitečná při s případem reakcí na incidenty.</span><span class="sxs-lookup"><span data-stu-id="41143-143">As described in the beginning of this section, this information can be very useful during an incident response case.</span></span> <span data-ttu-id="41143-144">Může také být důležité během forenzního vyšetřování, potřebujete-li najít zdroj útoku, který systém došlo k ohrožení a časovou osu.</span><span class="sxs-lookup"><span data-stu-id="41143-144">It can also be important during a forensic investigation, where you need to find the source of the attack, which system was compromised and the timeline.</span></span> <span data-ttu-id="41143-145">V této sestavě mohli snadno identifikovat některé klíčové podrobnosti o útok, jako například: zdroji útoku, místní IP adresa, došlo k ohrožení a stavu aktuální relace připojení.</span><span class="sxs-lookup"><span data-stu-id="41143-145">In this report you can easily identify some key details about the attack, such as: the source of the attack, the local IP that was compromised and the current session state of the connection.</span></span> 

<span data-ttu-id="41143-146">**Threat intelligence mapy** vám pomůže identifikovat aktuální umístění po celém světě, které mají škodlivý přenos.</span><span class="sxs-lookup"><span data-stu-id="41143-146">The **threat intelligence map** will help you to identify the current locations around the globe that have malicious traffic.</span></span> <span data-ttu-id="41143-147">Existují oranžová (příchozí) a červený (odchozí) šipky v této mapě identifikují směr provozu, pokud v jednom z těchto šipky kliknete, zobrazí typ hrozbě a směr provozu, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="41143-147">There are orange (incoming) and red (outgoing) arrows in this map that identify the traffic direction, if you click in one of these arrows, it will show the type of threat and the traffic direction as shown below:</span></span>

![mapa analýzy hrozeb](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig5.png)

> [!NOTE]
> <span data-ttu-id="41143-149">Ukázka uvidíte o tom, jak použít tuto funkci během reakce na incidenty Výukové video prezentace [zmírnit hrozby zabezpečení datového centra s asistencí šetření pomocí služby Operations Management Suite](https://myignite.microsoft.com/videos/5000) doručit na Microsoft Ignite.</span><span class="sxs-lookup"><span data-stu-id="41143-149">You can see a demonstration on how to use this capability during an incident response process by watching the presentation [Mitigate datacenter security threats with guided investigation using Operations Management Suite](https://myignite.microsoft.com/videos/5000) delivered at Microsoft Ignite.</span></span>
> 

### <a name="responding-to-distinct-malicious-ip-accessed"></a><span data-ttu-id="41143-150">Neodpovídá na požadavky odlišné škodlivé IP získat přístup</span><span class="sxs-lookup"><span data-stu-id="41143-150">Responding to distinct malicious IP accessed</span></span>
<span data-ttu-id="41143-151">V některých scénářích si můžete všimnout potenciálně škodlivé IP adresy, ke které přistupoval jeden monitorovaný počítač:</span><span class="sxs-lookup"><span data-stu-id="41143-151">In some scenarios, you may notice a potential malicious IP that was accessed from one monitored computer:</span></span>

![mapa analýzy hrozeb](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig6.png)

<span data-ttu-id="41143-153">Tato výstraha a další v rámci stejné kategorie se generují prostřednictvím zabezpečení OMS s využitím [analýzy hrozeb Microsoftu](https://youtu.be/O4WtxgUrDc8).</span><span class="sxs-lookup"><span data-stu-id="41143-153">This alert and others within the same category, are generated via OMS Security by leveraging [Microsoft Threat Intelligence](https://youtu.be/O4WtxgUrDc8).</span></span> <span data-ttu-id="41143-154">Data analýzy hrozeb shromažďuje Microsoft a také se kupují od vedoucích poskytovatelů analýzy hrozeb.</span><span class="sxs-lookup"><span data-stu-id="41143-154">The Threat Intelligence data is collected by Microsoft as well as purchased from leading threat intelligence providers.</span></span> <span data-ttu-id="41143-155">Tato data se často aktualizují a přizpůsobují rychle se měnícím hrozbám.</span><span class="sxs-lookup"><span data-stu-id="41143-155">This data is updated frequently and adapted to fast-moving threats.</span></span> <span data-ttu-id="41143-156">Díky jejich povaze je třeba při [vyšetřování](https://blogs.technet.microsoft.com/msoms/2016/12/08/investigating-suspicious-activity-in-a-hybrid-cloud-with-oms-security/) výstrah zabezpečení kombinovat je s dalšími zdroji informací o zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="41143-156">Due to its nature, it should be combined with other sources of security information while [investigating](https://blogs.technet.microsoft.com/msoms/2016/12/08/investigating-suspicious-activity-in-a-hybrid-cloud-with-oms-security/) a security alert.</span></span> 

## <a name="customize-alerts-received-via-e-mail"></a><span data-ttu-id="41143-157">Přizpůsobení obdržel prostřednictvím e-mailových upozornění</span><span class="sxs-lookup"><span data-stu-id="41143-157">Customize alerts received via e-mail</span></span>

<span data-ttu-id="41143-158">Můžete přizpůsobit, kteří uživatelé ve vaší organizaci bude upozorněn výstrahy zabezpečení jsou aktivovány OMS zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="41143-158">You can customize which users in your organization will be notified when security alerts are triggered by OMS Security.</span></span> <span data-ttu-id="41143-159">Tato možnost je k dispozici v části Přehled nebo nastavení na řídicím panelu OMS:</span><span class="sxs-lookup"><span data-stu-id="41143-159">This option is available under Overview / Settings at the OMS dashboard:</span></span>

![E-mail](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig7.png)

## <a name="see-also"></a><span data-ttu-id="41143-161">Viz také</span><span class="sxs-lookup"><span data-stu-id="41143-161">See also</span></span>
<span data-ttu-id="41143-162">V tomto dokumentu jste zjistili, jak používat **analýzou hrozeb** možnost v OMS zabezpečení a Audit řešení reakce na výstrahy zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="41143-162">In this document, you learned how to use the **Threat Intelligence** option in OMS Security and Audit solution to respond to security alerts.</span></span> <span data-ttu-id="41143-163">Další informace o zabezpečení v OMS najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="41143-163">To learn more about OMS Security, see the following articles:</span></span>

* [<span data-ttu-id="41143-164">Přehled Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="41143-164">Operations Management Suite (OMS) overview</span></span>](operations-management-suite-overview.md)
* [<span data-ttu-id="41143-165">Začínáme se službou Operations Management Suite zabezpečení a Audit řešení</span><span class="sxs-lookup"><span data-stu-id="41143-165">Getting started with Operations Management Suite Security and Audit Solution</span></span>](oms-security-getting-started.md)
* [<span data-ttu-id="41143-166">Monitorování prostředků v řešení Zabezpečení a audit v Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="41143-166">Monitoring Resources in Operations Management Suite Security and Audit Solution</span></span>](oms-security-monitoring-resources.md)

