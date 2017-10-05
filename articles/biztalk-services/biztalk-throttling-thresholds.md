---
title: "Další informace o omezování ve službě BizTalk Services | Microsoft Docs"
description: "Další informace o omezování prahové hodnoty a výsledná modul runtime chování služby BizTalk Services. Omezení je na základě využití paměti a počet zpráv. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: f6663cf2-cda4-4bac-855e-27d2ad5c4fa4
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 145e7470bbc01c676a1fb5856c0f9a8726e667fc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="biztalk-services-throttling"></a><span data-ttu-id="5828d-105">BizTalk Services: Omezování</span><span class="sxs-lookup"><span data-stu-id="5828d-105">BizTalk Services: Throttling</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

<span data-ttu-id="5828d-106">Služba Azure BizTalk Services implementuje služby omezování na základě podmínek, dvě: využití paměti a počtu souběžných zpracování zpráv.</span><span class="sxs-lookup"><span data-stu-id="5828d-106">Azure BizTalk Services implements service throttling based on two conditions: memory usage and the number of simultaneous messages processing.</span></span> <span data-ttu-id="5828d-107">Toto téma uvádí omezení prahové hodnoty a popisuje modul Runtime chování, když dojde k omezení podmínku.</span><span class="sxs-lookup"><span data-stu-id="5828d-107">This topic lists the throttling thresholds and describes the Runtime behavior when a throttling condition occurs.</span></span>

## <a name="throttling-thresholds"></a><span data-ttu-id="5828d-108">Omezení šířky pásma prahové hodnoty</span><span class="sxs-lookup"><span data-stu-id="5828d-108">Throttling Thresholds</span></span>
<span data-ttu-id="5828d-109">Následující tabulka uvádí omezení zdroje a prahové hodnoty:</span><span class="sxs-lookup"><span data-stu-id="5828d-109">The following table lists the throttling source and thresholds:</span></span>

|  | <span data-ttu-id="5828d-110">Popis</span><span class="sxs-lookup"><span data-stu-id="5828d-110">Description</span></span> | <span data-ttu-id="5828d-111">Nízké prahové hodnoty</span><span class="sxs-lookup"><span data-stu-id="5828d-111">Low Threshold</span></span> | <span data-ttu-id="5828d-112">Horní prahové hodnoty</span><span class="sxs-lookup"><span data-stu-id="5828d-112">High Threshold</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5828d-113">Memory (Paměť)</span><span class="sxs-lookup"><span data-stu-id="5828d-113">Memory</span></span> |<span data-ttu-id="5828d-114">% z celkových systémových paměti k dispozici nebo PageFileBytes.</span><span class="sxs-lookup"><span data-stu-id="5828d-114">% of total system memory available/PageFileBytes.</span></span> <p><p><span data-ttu-id="5828d-115">Celkový počet dostupných PageFileBytes je přibližně 2 časy RAM systému.</span><span class="sxs-lookup"><span data-stu-id="5828d-115">Total available PageFileBytes is approximately 2 times the RAM of the system.</span></span> |<span data-ttu-id="5828d-116">60%</span><span class="sxs-lookup"><span data-stu-id="5828d-116">60%</span></span> |<span data-ttu-id="5828d-117">70%</span><span class="sxs-lookup"><span data-stu-id="5828d-117">70%</span></span> |
| <span data-ttu-id="5828d-118">Zpracování zpráv</span><span class="sxs-lookup"><span data-stu-id="5828d-118">Message Processing</span></span> |<span data-ttu-id="5828d-119">Počet současně zpracování zpráv</span><span class="sxs-lookup"><span data-stu-id="5828d-119">Number of messages processing simultaneously</span></span> |<span data-ttu-id="5828d-120">40 * počet jader</span><span class="sxs-lookup"><span data-stu-id="5828d-120">40 * number of cores</span></span> |<span data-ttu-id="5828d-121">100 * počet jader</span><span class="sxs-lookup"><span data-stu-id="5828d-121">100 * number of cores</span></span> |

<span data-ttu-id="5828d-122">Když je dosaženo horní prahové hodnoty, začne omezení služby Azure BizTalk Services.</span><span class="sxs-lookup"><span data-stu-id="5828d-122">When a high threshold is reached, Azure BizTalk Services starts to throttle.</span></span> <span data-ttu-id="5828d-123">Omezení zastaví, když je dosaženo nízké prahové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5828d-123">Throttling stops when the low threshold is reached.</span></span> <span data-ttu-id="5828d-124">Například služby používá 65 % systémové paměti.</span><span class="sxs-lookup"><span data-stu-id="5828d-124">For example, your service is using 65% system memory.</span></span> <span data-ttu-id="5828d-125">V takovém případě není omezení služby.</span><span class="sxs-lookup"><span data-stu-id="5828d-125">In this situation, the service does not throttle.</span></span> <span data-ttu-id="5828d-126">Vaše služba spustí pomocí 70 % systémové paměti.</span><span class="sxs-lookup"><span data-stu-id="5828d-126">Your service starts using 70% system memory.</span></span> <span data-ttu-id="5828d-127">V takovém případě služba omezí generovaný a pokračuje v omezení, dokud služba používá 60 % (nízké prahové hodnoty) systémové paměti.</span><span class="sxs-lookup"><span data-stu-id="5828d-127">In this situation, the service throttles and continues to throttle until the service uses 60% (low threshold) system memory.</span></span>

<span data-ttu-id="5828d-128">Služba Azure BizTalk Services sleduje stav omezení (normální stav oproti omezenému stavu) a omezení doby trvání.</span><span class="sxs-lookup"><span data-stu-id="5828d-128">Azure BizTalk Services tracks the throttling status (normal state vs. throttled state) and the throttling duration.</span></span>

## <a name="runtime-behavior"></a><span data-ttu-id="5828d-129">Modul runtime chování</span><span class="sxs-lookup"><span data-stu-id="5828d-129">Runtime Behavior</span></span>
<span data-ttu-id="5828d-130">Když služba Azure BizTalk Services vstupuje do stavu omezení, dojde k následující položky:</span><span class="sxs-lookup"><span data-stu-id="5828d-130">When Azure BizTalk Services enters a throttling state, the following occurs:</span></span>

* <span data-ttu-id="5828d-131">Omezení je na instanci role.</span><span class="sxs-lookup"><span data-stu-id="5828d-131">Throttling is per role instance.</span></span> <span data-ttu-id="5828d-132">Například:</span><span class="sxs-lookup"><span data-stu-id="5828d-132">For example:</span></span><br/>
  <span data-ttu-id="5828d-133">RoleInstanceA je omezení.</span><span class="sxs-lookup"><span data-stu-id="5828d-133">RoleInstanceA is throttling.</span></span> <span data-ttu-id="5828d-134">RoleInstanceB není omezení.</span><span class="sxs-lookup"><span data-stu-id="5828d-134">RoleInstanceB is not throttling.</span></span> <span data-ttu-id="5828d-135">V takovém případě zpracování zpráv v RoleInstanceB podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="5828d-135">In this situation, messages in RoleInstanceB are processed as expected.</span></span> <span data-ttu-id="5828d-136">Zprávy v RoleInstanceA se zahodí a dojít k následující chybě:</span><span class="sxs-lookup"><span data-stu-id="5828d-136">Messages in RoleInstanceA are discarded and fail with the following error:</span></span><br/><br/><span data-ttu-id="5828d-137">
  **Server je zaneprázdněn. Zkuste to prosím znovu.**</span><span class="sxs-lookup"><span data-stu-id="5828d-137">
**Server is busy. Please try again.**</span></span><br/><br/>
* <span data-ttu-id="5828d-138">Žádné zdroje vyžádání nezadávejte dotazovat nebo stáhnout zprávy.</span><span class="sxs-lookup"><span data-stu-id="5828d-138">Any pull sources do not poll or download a message.</span></span> <span data-ttu-id="5828d-139">Například:</span><span class="sxs-lookup"><span data-stu-id="5828d-139">For example:</span></span><br/>
  <span data-ttu-id="5828d-140">Kanál vrátí zprávy z externího zdroje FTP.</span><span class="sxs-lookup"><span data-stu-id="5828d-140">A pipeline pulls messages from an external FTP source.</span></span> <span data-ttu-id="5828d-141">Získá instanci role provádění vyžádání do stavu omezení.</span><span class="sxs-lookup"><span data-stu-id="5828d-141">The role instance doing the pull gets into a throttling state.</span></span> <span data-ttu-id="5828d-142">V takovém případě kanál zastaví stahování další zprávy, dokud role instance zastavení omezení.</span><span class="sxs-lookup"><span data-stu-id="5828d-142">In this situation, the pipeline stops downloading additional messages until the role instance stops throttling.</span></span>
* <span data-ttu-id="5828d-143">Odpověď je odeslat klientovi, aby klient může znovu odeslat zprávu.</span><span class="sxs-lookup"><span data-stu-id="5828d-143">A response is sent to the client so the client can resubmit the message.</span></span>
* <span data-ttu-id="5828d-144">Musíte počkat, dokud bude omezení je vyřešený.</span><span class="sxs-lookup"><span data-stu-id="5828d-144">You must wait until the throttling is resolved.</span></span> <span data-ttu-id="5828d-145">Konkrétně je nutné počkat, dokud nebude dosaženo nízké prahové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5828d-145">Specifically, you must wait until the low threshold is reached.</span></span>

## <a name="important-notes"></a><span data-ttu-id="5828d-146">Důležité poznámky</span><span class="sxs-lookup"><span data-stu-id="5828d-146">Important notes</span></span>
* <span data-ttu-id="5828d-147">Omezení nelze zakázat.</span><span class="sxs-lookup"><span data-stu-id="5828d-147">Throttling cannot be disabled.</span></span>
* <span data-ttu-id="5828d-148">Omezení šířky pásma prahové hodnoty nemůže být upraven.</span><span class="sxs-lookup"><span data-stu-id="5828d-148">Throttling thresholds cannot be modified.</span></span>
* <span data-ttu-id="5828d-149">Omezení je implementováno celého systému.</span><span class="sxs-lookup"><span data-stu-id="5828d-149">Throttling is implemented system-wide.</span></span>
* <span data-ttu-id="5828d-150">Server databáze SQL Azure má také předdefinované omezení.</span><span class="sxs-lookup"><span data-stu-id="5828d-150">The Azure SQL Database Server also has built-in throttling.</span></span>

## <a name="additional-azure-biztalk-services-topics"></a><span data-ttu-id="5828d-151">Další témata týkající se služby Azure BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="5828d-151">Additional Azure BizTalk Services topics</span></span>
* [<span data-ttu-id="5828d-152">Instalace služby Azure BizTalk Services SDK</span><span class="sxs-lookup"><span data-stu-id="5828d-152">Installing the Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [<span data-ttu-id="5828d-153">Kurzy: Služby Azure BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="5828d-153">Tutorials: Azure BizTalk Services</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [<span data-ttu-id="5828d-154">Jak začít používat sadu SDK Azure BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="5828d-154">How do I Start Using the Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [<span data-ttu-id="5828d-155">Služby Azure BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="5828d-155">Azure BizTalk Services</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a><span data-ttu-id="5828d-156">Viz také</span><span class="sxs-lookup"><span data-stu-id="5828d-156">See Also</span></span>
* [<span data-ttu-id="5828d-157">BizTalk Services: Developer, Basic, Standard a Premium tabulka edic</span><span class="sxs-lookup"><span data-stu-id="5828d-157">BizTalk Services: Developer, Basic, Standard and Premium Editions Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [<span data-ttu-id="5828d-158">BizTalk Services: Zřízení pomocí Azure portál classic</span><span class="sxs-lookup"><span data-stu-id="5828d-158">BizTalk Services: Provisioning Using Azure classic portal</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [<span data-ttu-id="5828d-159">BizTalk Services: Tabulka stavů zřízení</span><span class="sxs-lookup"><span data-stu-id="5828d-159">BizTalk Services: Provisioning Status Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [<span data-ttu-id="5828d-160">BizTalk Services: Karty Řídicí panel, Sledování a Škálování</span><span class="sxs-lookup"><span data-stu-id="5828d-160">BizTalk Services: Dashboard, Monitor and Scale tabs</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [<span data-ttu-id="5828d-161">BizTalk Services: Zálohování a obnovení</span><span class="sxs-lookup"><span data-stu-id="5828d-161">BizTalk Services: Backup and Restore</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [<span data-ttu-id="5828d-162">BizTalk Services: Název a klíč vystavitele</span><span class="sxs-lookup"><span data-stu-id="5828d-162">BizTalk Services: Issuer Name and Issuer Key</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>

