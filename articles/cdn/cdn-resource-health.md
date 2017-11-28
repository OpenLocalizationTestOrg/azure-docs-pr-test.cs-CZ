---
title: "Stav hello aaaMonitor prostředků Azure CDN | Microsoft Docs"
description: "Zjistěte, jak toomonitor hello stav svých prostředků Azure CDN pomocí Azure Resource Health."
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
ms.openlocfilehash: 0a77e56d2fecae4bde6c83730c05375853a6638a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-hello-health-of-azure-cdn-resources"></a><span data-ttu-id="dcb6d-103">Sledování stavu hello prostředků Azure CDN</span><span class="sxs-lookup"><span data-stu-id="dcb6d-103">Monitor hello health of Azure CDN resources</span></span>
  
<span data-ttu-id="dcb6d-104">Stav Azure CDN prostředku je podmnožinou [stavu prostředků Azure](../resource-health/resource-health-overview.md).</span><span class="sxs-lookup"><span data-stu-id="dcb6d-104">Azure CDN Resource health is a subset of [Azure resource health](../resource-health/resource-health-overview.md).</span></span>  <span data-ttu-id="dcb6d-105">Můžete použít Azure resource health toomonitor hello stav CDN prostředků a přijímat řešitelné pokyny tootroubleshoot problémy.</span><span class="sxs-lookup"><span data-stu-id="dcb6d-105">You can use Azure resource health toomonitor hello health of CDN resources and receive actionable guidance tootroubleshoot problems.</span></span>

>[!IMPORTANT] 
><span data-ttu-id="dcb6d-106">Stav prostředku Azure CDN aktuálně pouze účty pro hello stavu doručování globální a funkcí rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="dcb6d-106">Azure CDN resource health only currently accounts for hello health of global CDN delivery and API capabilities.</span></span>  <span data-ttu-id="dcb6d-107">Stav prostředku Azure CDN neověřuje jednotlivých koncových bodů CDN.</span><span class="sxs-lookup"><span data-stu-id="dcb6d-107">Azure CDN resource health does not verify individual CDN endpoints.</span></span>
>
><span data-ttu-id="dcb6d-108">Hello signály, které kanálu stav prostředku Azure CDN může být až too15 minut zpožděna.</span><span class="sxs-lookup"><span data-stu-id="dcb6d-108">hello signals that feed Azure CDN resource health may be up too15 minutes delayed.</span></span>

## <a name="how-toofind-azure-cdn-resource-health"></a><span data-ttu-id="dcb6d-109">Jak toofind stav prostředku Azure CDN</span><span class="sxs-lookup"><span data-stu-id="dcb6d-109">How toofind Azure CDN resource health</span></span>

1. <span data-ttu-id="dcb6d-110">V hello [portál Azure](https://portal.azure.com), procházet tooyour profil CDN.</span><span class="sxs-lookup"><span data-stu-id="dcb6d-110">In hello [Azure portal](https://portal.azure.com), browse tooyour CDN profile.</span></span>

2. <span data-ttu-id="dcb6d-111">Klikněte na tlačítko hello **nastavení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dcb6d-111">Click hello **Settings** button.</span></span>

    ![Tlačítko Nastavení](./media/cdn-resource-health/cdn-profile-settings.png)

3. <span data-ttu-id="dcb6d-113">V části *podpory a řešení potíží s*, klikněte na tlačítko **stav prostředku**.</span><span class="sxs-lookup"><span data-stu-id="dcb6d-113">Under *Support + troubleshooting*, click **Resource health**.</span></span>

    ![Stav prostředku CDN](./media/cdn-resource-health/cdn-resource-health3.png)

>[!TIP] 
><span data-ttu-id="dcb6d-115">Můžete také vyhledat CDN prostředky uvedené v hello *stav prostředku* dlaždici v hello *Nápověda a podpora* okno.</span><span class="sxs-lookup"><span data-stu-id="dcb6d-115">You can also find CDN resources listed in hello *Resource health* tile in hello *Help + support* blade.</span></span>  <span data-ttu-id="dcb6d-116">Můžete rychle získat příliš*Nápověda a podpora* kliknutím hello v kroužku **?**</span><span class="sxs-lookup"><span data-stu-id="dcb6d-116">You can quickly get too*Help + support* by clicking hello circled **?**</span></span> <span data-ttu-id="dcb6d-117">v hello pravého horního rohu portálu hello.</span><span class="sxs-lookup"><span data-stu-id="dcb6d-117">in hello upper right corner of hello portal.</span></span>
>
> ![Nápověda a podpora](./media/cdn-resource-health/cdn-help-support.png)

## <a name="azure-cdn-specific-messages"></a><span data-ttu-id="dcb6d-119">Azure CDN konkrétní zprávy</span><span class="sxs-lookup"><span data-stu-id="dcb6d-119">Azure CDN-specific messages</span></span>

<span data-ttu-id="dcb6d-120">Stav prostředku CDN související tooAzure stavy naleznete níže.</span><span class="sxs-lookup"><span data-stu-id="dcb6d-120">Statuses related tooAzure CDN resource health can be found below.</span></span>

|<span data-ttu-id="dcb6d-121">Zpráva</span><span class="sxs-lookup"><span data-stu-id="dcb6d-121">Message</span></span> | <span data-ttu-id="dcb6d-122">Doporučená akce</span><span class="sxs-lookup"><span data-stu-id="dcb6d-122">Recommended Action</span></span> |
|---|---|
|<span data-ttu-id="dcb6d-123">Vám může mít zastavena, odebráno nebo špatně nakonfigurovaný minimálně jeden koncové body CDN</span><span class="sxs-lookup"><span data-stu-id="dcb6d-123">You may have stopped, removed, or misconfigured one or more of your CDN endpoints</span></span> | <span data-ttu-id="dcb6d-124">Vám může mít zastavena, odebráno nebo špatně nakonfigurovaný minimálně jeden koncové body CDN.</span><span class="sxs-lookup"><span data-stu-id="dcb6d-124">You may have stopped, removed, or misconfigured one or more of your CDN endpoints.</span></span>|
|<span data-ttu-id="dcb6d-125">Je nám líto, hello CDN management service není momentálně k dispozici</span><span class="sxs-lookup"><span data-stu-id="dcb6d-125">We are sorry, hello CDN management service is currently unavailable</span></span> | <span data-ttu-id="dcb6d-126">Pravidelně kontrolujte stav aktualizací; Pokud problém přetrvává i po hello očekává doba řešení, obraťte se na podporu.</span><span class="sxs-lookup"><span data-stu-id="dcb6d-126">Check back here for status updates; If your problem persists after hello expected resolution time, contact support.</span></span>|
|<span data-ttu-id="dcb6d-127">Je nám líto, že koncové body CDN může být ovlivněno probíhající problémy s některé z našich poskytovatelů CDN</span><span class="sxs-lookup"><span data-stu-id="dcb6d-127">We're sorry, your CDN endpoints may be impacted by ongoing issues with some of our CDN providers</span></span> | <span data-ttu-id="dcb6d-128">Pravidelně kontrolujte stav aktualizací; Jak používat hello Poradce při potížích nástroj toolearn tootest počátečního a koncového bodu CDN; Pokud problém přetrvává i po hello očekává doba řešení, obraťte se na podporu.</span><span class="sxs-lookup"><span data-stu-id="dcb6d-128">Check back here for status updates; Use hello Troubleshoot tool toolearn how tootest your origin and CDN endpoint; If your problem persists after hello expected resolution time, contact support.</span></span> |
|<span data-ttu-id="dcb6d-129">Je nám líto, změny konfigurace koncového bodu CDN pozorují zpoždění šíření</span><span class="sxs-lookup"><span data-stu-id="dcb6d-129">We're sorry, CDN endpoint configuration changes are experiencing propagation delays</span></span> | <span data-ttu-id="dcb6d-130">Pravidelně kontrolujte stav aktualizací; Pokud vaše změny konfigurace nebudou rozšířeny plně v čase hello očekávání, obraťte se na podporu.</span><span class="sxs-lookup"><span data-stu-id="dcb6d-130">Check back here for status updates; If your configuration changes are not fully propagated in hello expected time, contact support.</span></span>|
|<span data-ttu-id="dcb6d-131">Je nám líto, že nemůžeme mají problémy načítání hello doplňkovém portálu</span><span class="sxs-lookup"><span data-stu-id="dcb6d-131">We're sorry, we are experiencing issues loading hello supplemental portal</span></span> | <span data-ttu-id="dcb6d-132">Pravidelně kontrolujte stav aktualizací; Pokud problém přetrvává i po hello očekává doba řešení, obraťte se na podporu.</span><span class="sxs-lookup"><span data-stu-id="dcb6d-132">Check back here for status updates; If your problem persists after hello expected resolution time, contact support.</span></span>|
<span data-ttu-id="dcb6d-133">Je nám líto, že nemůžeme mají problémy se některé z našich poskytovatelů CDN</span><span class="sxs-lookup"><span data-stu-id="dcb6d-133">We are sorry, we are experiencing issues with some of our CDN providers</span></span> | <span data-ttu-id="dcb6d-134">Pravidelně kontrolujte stav aktualizací; Pokud problém přetrvává i po hello očekává doba řešení, obraťte se na podporu.</span><span class="sxs-lookup"><span data-stu-id="dcb6d-134">Check back here for status updates; If your problem persists after hello expected resolution time, contact support.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="dcb6d-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dcb6d-135">Next steps</span></span>

- [<span data-ttu-id="dcb6d-136">Přečtěte si přehled o stavu prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="dcb6d-136">Read an overview of Azure resource health</span></span>](../resource-health/resource-health-overview.md)
- [<span data-ttu-id="dcb6d-137">Řešení problémů s kompresí CDN</span><span class="sxs-lookup"><span data-stu-id="dcb6d-137">Troubleshoot issues with CDN compression</span></span>](./cdn-troubleshoot-compression.md)
- [<span data-ttu-id="dcb6d-138">Řešení problémů s chyb 404</span><span class="sxs-lookup"><span data-stu-id="dcb6d-138">Troubleshoot issues with 404 errors</span></span>](./cdn-troubleshoot-endpoint.md)