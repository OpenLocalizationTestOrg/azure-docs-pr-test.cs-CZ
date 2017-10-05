---
title: "Monitorování stavu prostředků Azure CDN | Microsoft Docs"
description: "Naučte se monitorovat stav svých prostředků Azure CDN pomocí Azure Resource Health."
services: cdn
documentationcenter: .net
author: zhangmanling
manager: zhangmanling
editor: 
ms.assetid: bf23bd89-35b2-4aca-ac7f-68ee02953f31
ms.service: cdn
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 37fe208f5087f318e665e76825127854b4a11c98
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-the-health-of-azure-cdn-resources"></a><span data-ttu-id="bf57d-103">Monitorování stavu prostředků Azure CDN</span><span class="sxs-lookup"><span data-stu-id="bf57d-103">Monitor the health of Azure CDN resources</span></span>
  
<span data-ttu-id="bf57d-104">Stav Azure CDN prostředku je podmnožinou [stavu prostředků Azure](../resource-health/resource-health-overview.md).</span><span class="sxs-lookup"><span data-stu-id="bf57d-104">Azure CDN Resource health is a subset of [Azure resource health](../resource-health/resource-health-overview.md).</span></span>  <span data-ttu-id="bf57d-105">Stav prostředků Azure můžete sledovat stav CDN prostředků a přijímat řešitelné pokyny k odstraňování problémů.</span><span class="sxs-lookup"><span data-stu-id="bf57d-105">You can use Azure resource health to monitor the health of CDN resources and receive actionable guidance to troubleshoot problems.</span></span>

>[!IMPORTANT] 
><span data-ttu-id="bf57d-106">Stav prostředku Azure CDN pouze aktuálně účty pro stav globální doručování a možnosti rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="bf57d-106">Azure CDN resource health only currently accounts for the health of global CDN delivery and API capabilities.</span></span>  <span data-ttu-id="bf57d-107">Stav prostředku Azure CDN neověřuje jednotlivých koncových bodů CDN.</span><span class="sxs-lookup"><span data-stu-id="bf57d-107">Azure CDN resource health does not verify individual CDN endpoints.</span></span>
>
><span data-ttu-id="bf57d-108">Signály, které kanálu stav prostředku Azure CDN může být až o 15 minut zpožděna.</span><span class="sxs-lookup"><span data-stu-id="bf57d-108">The signals that feed Azure CDN resource health may be up to 15 minutes delayed.</span></span>

## <a name="how-to-find-azure-cdn-resource-health"></a><span data-ttu-id="bf57d-109">Postup nalezení stav prostředku Azure CDN</span><span class="sxs-lookup"><span data-stu-id="bf57d-109">How to find Azure CDN resource health</span></span>

1. <span data-ttu-id="bf57d-110">V [portál Azure](https://portal.azure.com), přejděte na svůj profil CDN.</span><span class="sxs-lookup"><span data-stu-id="bf57d-110">In the [Azure portal](https://portal.azure.com), browse to your CDN profile.</span></span>

2. <span data-ttu-id="bf57d-111">Klikněte **nastavení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bf57d-111">Click the **Settings** button.</span></span>

    ![Tlačítko Nastavení](./media/cdn-resource-health/cdn-profile-settings.png)

3. <span data-ttu-id="bf57d-113">V části *podpory a řešení potíží s*, klikněte na tlačítko **stav prostředku**.</span><span class="sxs-lookup"><span data-stu-id="bf57d-113">Under *Support + troubleshooting*, click **Resource health**.</span></span>

    ![Stav prostředku CDN](./media/cdn-resource-health/cdn-resource-health3.png)

>[!TIP] 
><span data-ttu-id="bf57d-115">Můžete také vyhledat CDN prostředky uvedené v *stav prostředku* dlaždice v nástroji *Nápověda a podpora* okno.</span><span class="sxs-lookup"><span data-stu-id="bf57d-115">You can also find CDN resources listed in the *Resource health* tile in the *Help + support* blade.</span></span>  <span data-ttu-id="bf57d-116">Můžete rychle získat k *Nápověda a podpora* kliknutím v kroužku **?**</span><span class="sxs-lookup"><span data-stu-id="bf57d-116">You can quickly get to *Help + support* by clicking the circled **?**</span></span> <span data-ttu-id="bf57d-117">v pravém horním rohu portálu.</span><span class="sxs-lookup"><span data-stu-id="bf57d-117">in the upper right corner of the portal.</span></span>
>
> ![Nápověda a podpora](./media/cdn-resource-health/cdn-help-support.png)

## <a name="azure-cdn-specific-messages"></a><span data-ttu-id="bf57d-119">Azure CDN konkrétní zprávy</span><span class="sxs-lookup"><span data-stu-id="bf57d-119">Azure CDN-specific messages</span></span>

<span data-ttu-id="bf57d-120">Stavy související s stav prostředku Azure CDN najdete níže.</span><span class="sxs-lookup"><span data-stu-id="bf57d-120">Statuses related to Azure CDN resource health can be found below.</span></span>

|<span data-ttu-id="bf57d-121">Zpráva</span><span class="sxs-lookup"><span data-stu-id="bf57d-121">Message</span></span> | <span data-ttu-id="bf57d-122">Doporučená akce</span><span class="sxs-lookup"><span data-stu-id="bf57d-122">Recommended Action</span></span> |
|---|---|
|<span data-ttu-id="bf57d-123">Vám může mít zastavena, odebráno nebo špatně nakonfigurovaný minimálně jeden koncové body CDN</span><span class="sxs-lookup"><span data-stu-id="bf57d-123">You may have stopped, removed, or misconfigured one or more of your CDN endpoints</span></span> | <span data-ttu-id="bf57d-124">Vám může mít zastavena, odebráno nebo špatně nakonfigurovaný minimálně jeden koncové body CDN.</span><span class="sxs-lookup"><span data-stu-id="bf57d-124">You may have stopped, removed, or misconfigured one or more of your CDN endpoints.</span></span>|
|<span data-ttu-id="bf57d-125">Je nám líto, služba správy CDN není aktuálně dostupná</span><span class="sxs-lookup"><span data-stu-id="bf57d-125">We are sorry, the CDN management service is currently unavailable</span></span> | <span data-ttu-id="bf57d-126">Pravidelně kontrolujte stav aktualizací; Pokud váš problém přetrvává po čase očekávané řešení, obraťte se na podporu.</span><span class="sxs-lookup"><span data-stu-id="bf57d-126">Check back here for status updates; If your problem persists after the expected resolution time, contact support.</span></span>|
|<span data-ttu-id="bf57d-127">Je nám líto, že koncové body CDN může být ovlivněno probíhající problémy s některé z našich poskytovatelů CDN</span><span class="sxs-lookup"><span data-stu-id="bf57d-127">We're sorry, your CDN endpoints may be impacted by ongoing issues with some of our CDN providers</span></span> | <span data-ttu-id="bf57d-128">Pravidelně kontrolujte stav aktualizací; Pomocí nástroje Poradce při potížích se dozvíte, jak k testování počátečního a koncového bodu CDN; Pokud váš problém přetrvává po čase očekávané řešení, obraťte se na podporu.</span><span class="sxs-lookup"><span data-stu-id="bf57d-128">Check back here for status updates; Use the Troubleshoot tool to learn how to test your origin and CDN endpoint; If your problem persists after the expected resolution time, contact support.</span></span> |
|<span data-ttu-id="bf57d-129">Je nám líto, změny konfigurace koncového bodu CDN pozorují zpoždění šíření</span><span class="sxs-lookup"><span data-stu-id="bf57d-129">We're sorry, CDN endpoint configuration changes are experiencing propagation delays</span></span> | <span data-ttu-id="bf57d-130">Pravidelně kontrolujte stav aktualizací; Pokud vaše změny konfigurace nebudou rozšířeny plně v očekávaném čase, kontaktujte podporu.</span><span class="sxs-lookup"><span data-stu-id="bf57d-130">Check back here for status updates; If your configuration changes are not fully propagated in the expected time, contact support.</span></span>|
|<span data-ttu-id="bf57d-131">Je nám líto, že nemůžeme mají problémy načítání na doplňkovém portálu</span><span class="sxs-lookup"><span data-stu-id="bf57d-131">We're sorry, we are experiencing issues loading the supplemental portal</span></span> | <span data-ttu-id="bf57d-132">Pravidelně kontrolujte stav aktualizací; Pokud váš problém přetrvává po čase očekávané řešení, obraťte se na podporu.</span><span class="sxs-lookup"><span data-stu-id="bf57d-132">Check back here for status updates; If your problem persists after the expected resolution time, contact support.</span></span>|
<span data-ttu-id="bf57d-133">Je nám líto, že nemůžeme mají problémy se některé z našich poskytovatelů CDN</span><span class="sxs-lookup"><span data-stu-id="bf57d-133">We are sorry, we are experiencing issues with some of our CDN providers</span></span> | <span data-ttu-id="bf57d-134">Pravidelně kontrolujte stav aktualizací; Pokud váš problém přetrvává po čase očekávané řešení, obraťte se na podporu.</span><span class="sxs-lookup"><span data-stu-id="bf57d-134">Check back here for status updates; If your problem persists after the expected resolution time, contact support.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="bf57d-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bf57d-135">Next steps</span></span>

- [<span data-ttu-id="bf57d-136">Přečtěte si přehled o stavu prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="bf57d-136">Read an overview of Azure resource health</span></span>](../resource-health/resource-health-overview.md)
- [<span data-ttu-id="bf57d-137">Řešení problémů s kompresí CDN</span><span class="sxs-lookup"><span data-stu-id="bf57d-137">Troubleshoot issues with CDN compression</span></span>](./cdn-troubleshoot-compression.md)
- [<span data-ttu-id="bf57d-138">Řešení problémů s chyb 404</span><span class="sxs-lookup"><span data-stu-id="bf57d-138">Troubleshoot issues with 404 errors</span></span>](./cdn-troubleshoot-endpoint.md)