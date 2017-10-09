---
title: "aaaHandling výstrah zabezpečení v Azure Security Center | Microsoft Docs"
description: "Tento dokument vám pomůže bezpečnostní incidenty v možnosti toohandle toouse Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: e8feb669-8f30-49eb-ba38-046edf3f9656
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: yurid
ms.openlocfilehash: edb911c298a2ce93cd0ea5b22ce002005040090f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="handling-security-incidents-in-azure-security-center"></a><span data-ttu-id="477f4-103">Řešení bezpečnostních incidentů v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="477f4-103">Handling Security Incidents in Azure Security Center</span></span>
<span data-ttu-id="477f4-104">Triaging a prošetřování výstrah zabezpečení může být časově náročná i hello nejvíce zkušený analytikům zabezpečení a pro mnoho je pevný tooeven věděli, kde toobegin.</span><span class="sxs-lookup"><span data-stu-id="477f4-104">Triaging and investigating security alerts can be time consuming for even hello most skilled security analysts, and for many it is hard tooeven know where toobegin.</span></span> <span data-ttu-id="477f4-105">Pomocí [analytics](security-center-detection-capabilities.md) tooconnect hello informací mezi odlišné [výstrahy zabezpečení](security-center-managing-and-responding-alerts.md), Security Center můžete poskytnout jediné zobrazení kampaně útoku a všechny hello související výstrahy – můžete rychle pochopit, jaké akce hello útočník trvalo a jaké prostředky měla vliv na.</span><span class="sxs-lookup"><span data-stu-id="477f4-105">By using [analytics](security-center-detection-capabilities.md) tooconnect hello information between distinct [security alerts](security-center-managing-and-responding-alerts.md), Security Center can provide you with a single view of an attack campaign and all of hello related alerts – you can quickly understand what actions hello attacker took and what resources were impacted.</span></span>

<span data-ttu-id="477f4-106">Tento dokument popisuje, jak toouse zabezpečení funkci tooassist Security Center vás upozornit zpracování incidentů zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="477f4-106">This document discusses how toouse security alert capability in Security Center tooassist you handling security incidents.</span></span>

## <a name="what-is-a-security-incident"></a><span data-ttu-id="477f4-107">Co je bezpečnostní incident?</span><span class="sxs-lookup"><span data-stu-id="477f4-107">What is a security incident?</span></span>
<span data-ttu-id="477f4-108">Ve službě Security Center představuje bezpečnostní incident souhrn všech výstrah pro určitý prostředek, které odpovídají schématům [modelu kill chain](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/).</span><span class="sxs-lookup"><span data-stu-id="477f4-108">In Security Center, a security incident is an aggregation of all alerts for a resource that align with [kill chain](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) patterns.</span></span> <span data-ttu-id="477f4-109">Incident se zobrazí v hello [výstrahy zabezpečení](security-center-managing-and-responding-alerts.md) dlaždice a okno.</span><span class="sxs-lookup"><span data-stu-id="477f4-109">Incidents appear in hello [Security Alerts](security-center-managing-and-responding-alerts.md) tile and blade.</span></span> <span data-ttu-id="477f4-110">Další informace o každém výskytu bude hello seznamu související výstrahy, což umožňuje vám tooobtain odhalit incidentu.</span><span class="sxs-lookup"><span data-stu-id="477f4-110">An Incident will reveal hello list of related alerts, which enables you tooobtain more information about each occurrence.</span></span>

## <a name="managing-security-incidents"></a><span data-ttu-id="477f4-111">Správa incidentů zabezpečení</span><span class="sxs-lookup"><span data-stu-id="477f4-111">Managing security incidents</span></span>
<span data-ttu-id="477f4-112">Vaše aktuální incidenty zabezpečení můžete zkontrolovat prohlížením hello dlaždice výstrahy zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="477f4-112">You can review your current security incidents by looking at hello security alerts tile.</span></span> <span data-ttu-id="477f4-113">Přístup k hello portálu Azure a postupujte podle kroků hello toosee další podrobnosti o každém incidentu zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="477f4-113">Access hello Azure Portal and follow hello steps below toosee more details about each security incident:</span></span>

1. <span data-ttu-id="477f4-114">Na řídicím panelu Security Center hello, uvidíte hello **výstrahy zabezpečení** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="477f4-114">On hello Security Center dashboard, you will see hello **Security alerts** tile.</span></span>

    ![Dlaždice Výstrahy zabezpečení ve službě Security Center](./media/security-center-incident/security-center-incident-fig1.png)

2. <span data-ttu-id="477f4-116">Klikněte na tuto dlaždici tooexpand, a pokud je zjištěn bezpečnostního incidentu, se zobrazí v grafu výstrahy zabezpečení hello jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="477f4-116">Click on this tile tooexpand it and if a security incident is detected, it will appear under hello security alerts graph as shown  below:</span></span>

    ![Incident zabezpečení](./media/security-center-incident/security-center-incident-fig2.png)

3. <span data-ttu-id="477f4-118">Všimněte si, že popis incidentu hello zabezpečení má vlastní ikonu porovná tooother výstrahy.</span><span class="sxs-lookup"><span data-stu-id="477f4-118">Notice that hello security incident description has a different icon compared tooother alerts.</span></span> <span data-ttu-id="477f4-119">Klikněte na jeho tooview více podrobností o tohoto incidentu.</span><span class="sxs-lookup"><span data-stu-id="477f4-119">Click on it tooview more details about this incident.</span></span>

    ![Incident zabezpečení](./media/security-center-incident/security-center-incident-fig3.png)

4. <span data-ttu-id="477f4-121">Na hello **incident** okno se zobrazí další podrobnosti o této bezpečnostního incidentu, což zahrnuje jeho úplný popis jeho závažnosti (což je v tomto případě vysoká), jeho aktuální stav (v tomto případě je stále *aktivní*, což naznačuje hello uživatele nepřijme akci tooit – to můžete provést kliknutím pravým tlačítkem na incident hello v hello **výstrahy zabezpečení** okno), hello napadení prostředků (v tomto případě *VM1*), hello nápravy kroky pro hello incident, a v dolním podokně hello máte hello výstrahy, které byly součástí tohoto incidentu.</span><span class="sxs-lookup"><span data-stu-id="477f4-121">On hello **incident** blade you will see more details about this security incident, which includes its full description, its severity (which in this case is high), its current state (in this case it is still *active*, which implies hello user hasn't taken an action tooit - this can be done by right clicking on hello incident in hello **Security alerts** blade), hello attacked resource (in this case *VM1*), hello remediation steps for hello incident, and in hello bottom pane you have hello alerts that were included in this incident.</span></span> <span data-ttu-id="477f4-122">Pokud chcete další informace o každé výstraze tooobtain, jednoduše klikněte na jeho a další okno otevře, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="477f4-122">If you want tooobtain more information on each alert, just click on it and another blade will open, as shown below:</span></span>

    ![Incident zabezpečení](./media/security-center-incident/security-center-incident-fig4.png)

<span data-ttu-id="477f4-124">Hello informace v tomto okně se bude lišit podle výstrahy toohello.</span><span class="sxs-lookup"><span data-stu-id="477f4-124">hello information on this blade will vary according toohello alert.</span></span> <span data-ttu-id="477f4-125">Čtení [správy a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) pro další informace o toomanage tyto výstrahy.</span><span class="sxs-lookup"><span data-stu-id="477f4-125">Read [Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) for more information on how toomanage these alerts.</span></span> <span data-ttu-id="477f4-126">Některé důležité informace týkající se této funkce:</span><span class="sxs-lookup"><span data-stu-id="477f4-126">Some important considerations regarding this capability:</span></span>

* <span data-ttu-id="477f4-127">Nový filtr umožňuje toocustomize, které vaše tooIncident zobrazení pouze výstrahy pouze nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="477f4-127">A new filter enables you toocustomize your view tooIncident only, Alerts only, or both.</span></span>
* <span data-ttu-id="477f4-128">Hello stejná výstraha může existovat v rámci incidentu (pokud existuje), stejně jako toobe viditelné jako samostatnou výstrahu.</span><span class="sxs-lookup"><span data-stu-id="477f4-128">hello same alert can exist as part of an Incident (if applicable), as well as toobe visible as a standalone alert.</span></span>

## <a name="see-also"></a><span data-ttu-id="477f4-129">Viz také</span><span class="sxs-lookup"><span data-stu-id="477f4-129">See also</span></span>
<span data-ttu-id="477f4-130">V tomto dokumentu jste zjistili, jak toouse hello incidentu funkce zabezpečení ve službě Security Center.</span><span class="sxs-lookup"><span data-stu-id="477f4-130">In this document, you learned how toouse hello security incident capability in Security Center.</span></span> <span data-ttu-id="477f4-131">toolearn Další informace o Security Center, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="477f4-131">toolearn more about Security Center, see hello following:</span></span>

* [<span data-ttu-id="477f4-132">Správa a zpracování výstrah toosecurity v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="477f4-132">Managing and responding toosecurity alerts in Azure Security Center</span></span>](security-center-managing-and-responding-alerts.md)
* [<span data-ttu-id="477f4-133">Funkce detekce ve službě Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="477f4-133">Azure Security Center Detection Capabilities</span></span>](security-center-detection-capabilities.md)
* [<span data-ttu-id="477f4-134">Průvodce plánováním a provozem služby Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="477f4-134">Azure Security Center Planning and Operations Guide</span></span>](security-center-planning-and-operations-guide.md)
* [<span data-ttu-id="477f4-135">Správa a zpracování výstrah toosecurity v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="477f4-135">Managing and responding toosecurity alerts in Azure Security Center</span></span>](security-center-managing-and-responding-alerts.md)
* <span data-ttu-id="477f4-136">[Nejčastější dotazy k Azure Security Center](security-center-faq.md)– přečtěte si nejčastější dotazy o použití služby hello.</span><span class="sxs-lookup"><span data-stu-id="477f4-136">[Azure Security Center FAQ](security-center-faq.md)--Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="477f4-137">[Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="477f4-137">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/)--Find blog posts about Azure security and compliance.</span></span>
