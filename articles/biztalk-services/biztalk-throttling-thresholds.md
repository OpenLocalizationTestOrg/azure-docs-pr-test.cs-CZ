---
title: "aaaLearn o omezování ve službě BizTalk Services | Microsoft Docs"
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
ms.openlocfilehash: 46c8806c3a1f4eeb793f721f849771e0ec155197
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="biztalk-services-throttling"></a><span data-ttu-id="19a0c-105">BizTalk Services: Omezování</span><span class="sxs-lookup"><span data-stu-id="19a0c-105">BizTalk Services: Throttling</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

<span data-ttu-id="19a0c-106">Služba Azure BizTalk Services implementuje služby omezování na základě podmínek, dvě: paměti využití a hello počet souběžných zpracování zpráv.</span><span class="sxs-lookup"><span data-stu-id="19a0c-106">Azure BizTalk Services implements service throttling based on two conditions: memory usage and hello number of simultaneous messages processing.</span></span> <span data-ttu-id="19a0c-107">Toto téma uvádí hello omezení prahové hodnoty a popisuje hello modul Runtime chování, když dojde k omezení podmínku.</span><span class="sxs-lookup"><span data-stu-id="19a0c-107">This topic lists hello throttling thresholds and describes hello Runtime behavior when a throttling condition occurs.</span></span>

## <a name="throttling-thresholds"></a><span data-ttu-id="19a0c-108">Omezení šířky pásma prahové hodnoty</span><span class="sxs-lookup"><span data-stu-id="19a0c-108">Throttling Thresholds</span></span>
<span data-ttu-id="19a0c-109">Následující tabulka seznamy Hello hello omezení zdroje a prahové hodnoty:</span><span class="sxs-lookup"><span data-stu-id="19a0c-109">hello following table lists hello throttling source and thresholds:</span></span>

|  | <span data-ttu-id="19a0c-110">Popis</span><span class="sxs-lookup"><span data-stu-id="19a0c-110">Description</span></span> | <span data-ttu-id="19a0c-111">Nízké prahové hodnoty</span><span class="sxs-lookup"><span data-stu-id="19a0c-111">Low Threshold</span></span> | <span data-ttu-id="19a0c-112">Horní prahové hodnoty</span><span class="sxs-lookup"><span data-stu-id="19a0c-112">High Threshold</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="19a0c-113">Memory (Paměť)</span><span class="sxs-lookup"><span data-stu-id="19a0c-113">Memory</span></span> |<span data-ttu-id="19a0c-114">% z celkových systémových paměti k dispozici nebo PageFileBytes.</span><span class="sxs-lookup"><span data-stu-id="19a0c-114">% of total system memory available/PageFileBytes.</span></span> <p><p><span data-ttu-id="19a0c-115">Celkový počet dostupných PageFileBytes je přibližně 2 časy hello RAM hello systému.</span><span class="sxs-lookup"><span data-stu-id="19a0c-115">Total available PageFileBytes is approximately 2 times hello RAM of hello system.</span></span> |<span data-ttu-id="19a0c-116">60%</span><span class="sxs-lookup"><span data-stu-id="19a0c-116">60%</span></span> |<span data-ttu-id="19a0c-117">70%</span><span class="sxs-lookup"><span data-stu-id="19a0c-117">70%</span></span> |
| <span data-ttu-id="19a0c-118">Zpracování zpráv</span><span class="sxs-lookup"><span data-stu-id="19a0c-118">Message Processing</span></span> |<span data-ttu-id="19a0c-119">Počet současně zpracování zpráv</span><span class="sxs-lookup"><span data-stu-id="19a0c-119">Number of messages processing simultaneously</span></span> |<span data-ttu-id="19a0c-120">40 * počet jader</span><span class="sxs-lookup"><span data-stu-id="19a0c-120">40 * number of cores</span></span> |<span data-ttu-id="19a0c-121">100 * počet jader</span><span class="sxs-lookup"><span data-stu-id="19a0c-121">100 * number of cores</span></span> |

<span data-ttu-id="19a0c-122">Když je dosaženo horní prahové hodnoty, spustí se služba Azure BizTalk Services toothrottle.</span><span class="sxs-lookup"><span data-stu-id="19a0c-122">When a high threshold is reached, Azure BizTalk Services starts toothrottle.</span></span> <span data-ttu-id="19a0c-123">Omezení zastaví, když je dosaženo hello nízké prahové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="19a0c-123">Throttling stops when hello low threshold is reached.</span></span> <span data-ttu-id="19a0c-124">Například služby používá 65 % systémové paměti.</span><span class="sxs-lookup"><span data-stu-id="19a0c-124">For example, your service is using 65% system memory.</span></span> <span data-ttu-id="19a0c-125">V takovém případě není omezení služby hello.</span><span class="sxs-lookup"><span data-stu-id="19a0c-125">In this situation, hello service does not throttle.</span></span> <span data-ttu-id="19a0c-126">Vaše služba spustí pomocí 70 % systémové paměti.</span><span class="sxs-lookup"><span data-stu-id="19a0c-126">Your service starts using 70% system memory.</span></span> <span data-ttu-id="19a0c-127">V takovém případě služba hello omezí generovaný a toothrottle se opakuje, dokud služba hello používá 60 % (nízké prahové hodnoty) systémové paměti.</span><span class="sxs-lookup"><span data-stu-id="19a0c-127">In this situation, hello service throttles and continues toothrottle until hello service uses 60% (low threshold) system memory.</span></span>

<span data-ttu-id="19a0c-128">Služba Azure BizTalk Services sleduje hello omezení stav (normální stav oproti omezenému stavu) a hello omezení doby trvání.</span><span class="sxs-lookup"><span data-stu-id="19a0c-128">Azure BizTalk Services tracks hello throttling status (normal state vs. throttled state) and hello throttling duration.</span></span>

## <a name="runtime-behavior"></a><span data-ttu-id="19a0c-129">Modul runtime chování</span><span class="sxs-lookup"><span data-stu-id="19a0c-129">Runtime Behavior</span></span>
<span data-ttu-id="19a0c-130">Když služba Azure BizTalk Services vstupuje do stavu omezení, dojde k hello následující:</span><span class="sxs-lookup"><span data-stu-id="19a0c-130">When Azure BizTalk Services enters a throttling state, hello following occurs:</span></span>

* <span data-ttu-id="19a0c-131">Omezení je na instanci role.</span><span class="sxs-lookup"><span data-stu-id="19a0c-131">Throttling is per role instance.</span></span> <span data-ttu-id="19a0c-132">Například:</span><span class="sxs-lookup"><span data-stu-id="19a0c-132">For example:</span></span><br/>
  <span data-ttu-id="19a0c-133">RoleInstanceA je omezení.</span><span class="sxs-lookup"><span data-stu-id="19a0c-133">RoleInstanceA is throttling.</span></span> <span data-ttu-id="19a0c-134">RoleInstanceB není omezení.</span><span class="sxs-lookup"><span data-stu-id="19a0c-134">RoleInstanceB is not throttling.</span></span> <span data-ttu-id="19a0c-135">V takovém případě zpracování zpráv v RoleInstanceB podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="19a0c-135">In this situation, messages in RoleInstanceB are processed as expected.</span></span> <span data-ttu-id="19a0c-136">Zprávy v RoleInstanceA se zahodí a dojde k chybě hello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="19a0c-136">Messages in RoleInstanceA are discarded and fail with hello following error:</span></span><br/><br/><span data-ttu-id="19a0c-137">
  **Server je zaneprázdněn. Zkuste to prosím znovu.**</span><span class="sxs-lookup"><span data-stu-id="19a0c-137">
**Server is busy. Please try again.**</span></span><br/><br/>
* <span data-ttu-id="19a0c-138">Žádné zdroje vyžádání nezadávejte dotazovat nebo stáhnout zprávy.</span><span class="sxs-lookup"><span data-stu-id="19a0c-138">Any pull sources do not poll or download a message.</span></span> <span data-ttu-id="19a0c-139">Například:</span><span class="sxs-lookup"><span data-stu-id="19a0c-139">For example:</span></span><br/>
  <span data-ttu-id="19a0c-140">Kanál vrátí zprávy z externího zdroje FTP.</span><span class="sxs-lookup"><span data-stu-id="19a0c-140">A pipeline pulls messages from an external FTP source.</span></span> <span data-ttu-id="19a0c-141">Získá instanci role Hello provádění hello vyžádání do stavu omezení.</span><span class="sxs-lookup"><span data-stu-id="19a0c-141">hello role instance doing hello pull gets into a throttling state.</span></span> <span data-ttu-id="19a0c-142">V takovém případě hello kanál zastaví stahování další zprávy, dokud hello role instance zastaví, omezení šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="19a0c-142">In this situation, hello pipeline stops downloading additional messages until hello role instance stops throttling.</span></span>
* <span data-ttu-id="19a0c-143">Odpověď je odeslat toohello klienta, aby hello klienta můžete znovu odeslat uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="19a0c-143">A response is sent toohello client so hello client can resubmit hello message.</span></span>
* <span data-ttu-id="19a0c-144">Je nutné počkat, až do vyřešení hello omezení.</span><span class="sxs-lookup"><span data-stu-id="19a0c-144">You must wait until hello throttling is resolved.</span></span> <span data-ttu-id="19a0c-145">Konkrétně je nutné počkat, dokud nebude dosaženo hello nízké prahové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="19a0c-145">Specifically, you must wait until hello low threshold is reached.</span></span>

## <a name="important-notes"></a><span data-ttu-id="19a0c-146">Důležité poznámky</span><span class="sxs-lookup"><span data-stu-id="19a0c-146">Important notes</span></span>
* <span data-ttu-id="19a0c-147">Omezení nelze zakázat.</span><span class="sxs-lookup"><span data-stu-id="19a0c-147">Throttling cannot be disabled.</span></span>
* <span data-ttu-id="19a0c-148">Omezení šířky pásma prahové hodnoty nemůže být upraven.</span><span class="sxs-lookup"><span data-stu-id="19a0c-148">Throttling thresholds cannot be modified.</span></span>
* <span data-ttu-id="19a0c-149">Omezení je implementováno celého systému.</span><span class="sxs-lookup"><span data-stu-id="19a0c-149">Throttling is implemented system-wide.</span></span>
* <span data-ttu-id="19a0c-150">Hello Server databází SQL Azure má také předdefinované omezení.</span><span class="sxs-lookup"><span data-stu-id="19a0c-150">hello Azure SQL Database Server also has built-in throttling.</span></span>

## <a name="additional-azure-biztalk-services-topics"></a><span data-ttu-id="19a0c-151">Další témata týkající se služby Azure BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="19a0c-151">Additional Azure BizTalk Services topics</span></span>
* [<span data-ttu-id="19a0c-152">Instalace hello Azure BizTalk Services SDK</span><span class="sxs-lookup"><span data-stu-id="19a0c-152">Installing hello Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [<span data-ttu-id="19a0c-153">Kurzy: Služby Azure BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="19a0c-153">Tutorials: Azure BizTalk Services</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [<span data-ttu-id="19a0c-154">Jak začít používat hello Azure BizTalk Services SDK</span><span class="sxs-lookup"><span data-stu-id="19a0c-154">How do I Start Using hello Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [<span data-ttu-id="19a0c-155">Služby Azure BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="19a0c-155">Azure BizTalk Services</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a><span data-ttu-id="19a0c-156">Viz také</span><span class="sxs-lookup"><span data-stu-id="19a0c-156">See Also</span></span>
* [<span data-ttu-id="19a0c-157">BizTalk Services: Developer, Basic, Standard a Premium tabulka edic</span><span class="sxs-lookup"><span data-stu-id="19a0c-157">BizTalk Services: Developer, Basic, Standard and Premium Editions Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [<span data-ttu-id="19a0c-158">BizTalk Services: Zřízení pomocí Azure portál classic</span><span class="sxs-lookup"><span data-stu-id="19a0c-158">BizTalk Services: Provisioning Using Azure classic portal</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [<span data-ttu-id="19a0c-159">BizTalk Services: Tabulka stavů zřízení</span><span class="sxs-lookup"><span data-stu-id="19a0c-159">BizTalk Services: Provisioning Status Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [<span data-ttu-id="19a0c-160">BizTalk Services: Karty Řídicí panel, Sledování a Škálování</span><span class="sxs-lookup"><span data-stu-id="19a0c-160">BizTalk Services: Dashboard, Monitor and Scale tabs</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [<span data-ttu-id="19a0c-161">BizTalk Services: Zálohování a obnovení</span><span class="sxs-lookup"><span data-stu-id="19a0c-161">BizTalk Services: Backup and Restore</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [<span data-ttu-id="19a0c-162">BizTalk Services: Název a klíč vystavitele</span><span class="sxs-lookup"><span data-stu-id="19a0c-162">BizTalk Services: Issuer Name and Issuer Key</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>

