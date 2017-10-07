---
title: "aaaManaging partnerských řešení v Azure Security Center | Microsoft Docs"
description: "Tento dokument vás provede jak Azure Security Center umožňuje monitorování v první pohled hello stav vašich partnerských řešení integrovaných ve vašem předplatném Azure."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2017
ms.author: terrylan
ms.openlocfilehash: fc97aedf709b9044bfd3d4ecae0b58d5fa716bbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-partner-solutions-with-azure-security-center"></a><span data-ttu-id="83977-103">Sledování partnerských řešení pomocí Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="83977-103">Monitoring partner solutions with Azure Security Center</span></span>
<span data-ttu-id="83977-104">Tento dokument vás provede jak toomonitor hello stav vašich partnerských řešení v Azure Security Center.</span><span class="sxs-lookup"><span data-stu-id="83977-104">This document walks you through how toomonitor hello health status of your partner solutions in Azure Security Center.</span></span>

> [!NOTE]
> <span data-ttu-id="83977-105">Toto téma představuje hello služby pomocí příklad nasazení.</span><span class="sxs-lookup"><span data-stu-id="83977-105">This document introduces hello service by using an example deployment.</span></span> <span data-ttu-id="83977-106">Tento dokument není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="83977-106">This document is not a step-by-step guide.</span></span>
>
>

## <a name="monitoring-partner-solutions"></a><span data-ttu-id="83977-107">Sledování partnerských řešení</span><span class="sxs-lookup"><span data-stu-id="83977-107">Monitoring partner solutions</span></span>
<span data-ttu-id="83977-108">Hello **Partner solutions** na hello dlaždici **Security Center** okno umožňuje sledovat na první pohled hello stav vašich partnerských řešení, které jsou integrované s předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="83977-108">hello **Partner solutions** tile on hello **Security Center** blade lets you monitor at a glance hello health status of your partner solutions that are integrated with your Azure subscription.</span></span>

![Dlaždice Partner solutions (Partnerská řešení)][1]

<span data-ttu-id="83977-110">Hello **Partner solutions** dlaždice zobrazí hello počet partnerských řešení integrovaných ve vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="83977-110">hello **Partner solutions** tile displays hello number of partner solutions integrated with your subscription.</span></span> <span data-ttu-id="83977-111">Pokud je integrovaná žádná řešení, dlaždice hello zobrazí hello hodnotu nula.</span><span class="sxs-lookup"><span data-stu-id="83977-111">If there are no solutions integrated, hello tile displays hello number zero.</span></span>

<span data-ttu-id="83977-112">tooview hello stav vašich partnerských řešení:</span><span class="sxs-lookup"><span data-stu-id="83977-112">tooview hello health of your partner solutions:</span></span>

1. <span data-ttu-id="83977-113">Vyberte hello **Partner solutions** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="83977-113">Select hello **Partner solutions** tile.</span></span> <span data-ttu-id="83977-114">Hello **Partner solutions** tooSecurity Center připojené otevře se okno se seznamem partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="83977-114">hello **Partner solutions** blade opens displaying a list of your partner solutions connected tooSecurity Center.</span></span>

   ![Partnerská řešení][3]

   <span data-ttu-id="83977-116">Hello stav partnerských řešení může být:</span><span class="sxs-lookup"><span data-stu-id="83977-116">hello status of a partner solution can be:</span></span>

   * <span data-ttu-id="83977-117">Chráněné (zelená) – Stav je zcela v pořádku.</span><span class="sxs-lookup"><span data-stu-id="83977-117">Protected (green) - there is no health issue.</span></span>
   * <span data-ttu-id="83977-118">Není v pořádku (červená) – Existuje problém stavu, které si žádá okamžitou pozornost.</span><span class="sxs-lookup"><span data-stu-id="83977-118">Unhealthy (red) - there is a health issue that requires immediate attention.</span></span>
   * <span data-ttu-id="83977-119">Ukončeno generování sestav (oranžová) – řešení hello byla zastavena hlásit svůj stav.</span><span class="sxs-lookup"><span data-stu-id="83977-119">Stopped reporting (orange) - hello solution has stopped reporting its health.</span></span>
   * <span data-ttu-id="83977-120">Neznámý stav ochrany (oranžová) – hello stav řešení hello Neznámý v tuto chvíli kvůli tooa se nezdařil proces přidávání nového prostředku toohello existujícího řešení.</span><span class="sxs-lookup"><span data-stu-id="83977-120">Unknown protection status (orange) - hello health of hello solution is unknown at this time due tooa failed process of adding a new resource toohello existing solution.</span></span>
   * <span data-ttu-id="83977-121">Neuveden (šedá) – řešení hello neohlásil nic ještě nemusí být na řešení stav uvedený, pokud se nedávno připojil a ještě probíhá jeho nasazení.</span><span class="sxs-lookup"><span data-stu-id="83977-121">Not reported (gray) - hello solution has not reported anything yet, a solution's status may be unreported if it has recently been connected and is still deploying.</span></span>

2. <span data-ttu-id="83977-122">Vyberte jedno partnerské řešení.</span><span class="sxs-lookup"><span data-stu-id="83977-122">Select a partner solution.</span></span> <span data-ttu-id="83977-123">V tomto příkladu umožní vyberte hello **Qualys** řešení.</span><span class="sxs-lookup"><span data-stu-id="83977-123">In this example, lets select hello **Qualys** solution.</span></span>  <span data-ttu-id="83977-124">Otevře se okno ukazuje, že stav hello hello partnerského řešení a řešení hello přidružené prostředky.</span><span class="sxs-lookup"><span data-stu-id="83977-124">A blade opens showing you hello status of hello partner solution and hello solution's associated resources.</span></span> <span data-ttu-id="83977-125">Vyberte **řešení konzoly** tooopen hello partnera správu prostředí pro toto řešení.</span><span class="sxs-lookup"><span data-stu-id="83977-125">Select **Solution console** tooopen hello partner management experience for this solution.</span></span>

   ![Podrobné zobrazení partnerského řešení][4]
3. <span data-ttu-id="83977-127">Přejděte zpět toohello **Qualys** a vyberte **odkaz VM**.</span><span class="sxs-lookup"><span data-stu-id="83977-127">Go back toohello **Qualys** blade and select **Link VM**.</span></span> <span data-ttu-id="83977-128">Hello **odkaz aplikace** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="83977-128">hello **Link Applications** blade opens.</span></span> <span data-ttu-id="83977-129">Sem se můžete připojit prostředky toohello partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="83977-129">Here you can connect resources toohello partner solution.</span></span>

   ![Odkaz prostředky toopartner řešení][5]

## <a name="next-steps"></a><span data-ttu-id="83977-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="83977-131">Next steps</span></span>
<span data-ttu-id="83977-132">V tomto dokumentu jste přináší toohello **partnerských řešení** dlaždici v Centru zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="83977-132">In this document, you were introduced toohello **Partner Solutions** tile in Security Center.</span></span> <span data-ttu-id="83977-133">toolearn Další informace o Security Center, najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="83977-133">toolearn more about Security Center, see hello following articles:</span></span>

* <span data-ttu-id="83977-134">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – Další informace jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="83977-134">[Setting security policies in Azure Security Center](security-center-policies.md) — Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="83977-135">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="83977-135">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) — Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="83977-136">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="83977-136">[Security health monitoring in Azure Security Center](security-center-monitoring.md) — Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="83977-137">[Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – Další informace jak toomanage a reakce toosecurity výstrahy.</span><span class="sxs-lookup"><span data-stu-id="83977-137">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) — Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="83977-138">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.</span><span class="sxs-lookup"><span data-stu-id="83977-138">[Azure Security Center FAQ](security-center-faq.md) — Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="83977-139">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – získejte nejnovější informace zabezpečení Azure hello a informace.</span><span class="sxs-lookup"><span data-stu-id="83977-139">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) — Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[3]: ./media/security-center-partner-solutions/partner-solutions.png
[4]: ./media/security-center-partner-solutions/partner-solutions-detail.png
[5]: ./media/security-center-partner-solutions/link-applications.png
