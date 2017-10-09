---
title: aaaWhat je Azure Scheduler? | Dokumentace Microsoftu
description: "Azure Scheduler vám umožní toodeclaratively popisují akce toorun v cloudu hello. Potom naplánuje a automaticky spustí tyto akce."
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 52aa6ae1-4c3d-43fb-81b0-6792c84bcfae
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 062e25ae473510264dc0038198c05e7ac1e86210
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-scheduler"></a><span data-ttu-id="a8ae7-105">Co je Azure Scheduler?</span><span class="sxs-lookup"><span data-stu-id="a8ae7-105">What is Azure Scheduler?</span></span>
<span data-ttu-id="a8ae7-106">Azure Scheduler vám umožní toodeclaratively popisují akce toorun v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="a8ae7-106">Azure Scheduler allows you toodeclaratively describe actions toorun in hello cloud.</span></span> <span data-ttu-id="a8ae7-107">Potom naplánuje a automaticky spustí tyto akce.</span><span class="sxs-lookup"><span data-stu-id="a8ae7-107">It then schedules and runs those actions automatically.</span></span>  <span data-ttu-id="a8ae7-108">Scheduler k tomu pomocí [hello portál Azure](scheduler-get-started-portal.md), kód, [REST API](https://msdn.microsoft.com/library/mt629143.aspx), nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a8ae7-108">Scheduler does this by using [hello Azure portal](scheduler-get-started-portal.md), code, [REST API](https://msdn.microsoft.com/library/mt629143.aspx), or Azure PowerShell.</span></span>

<span data-ttu-id="a8ae7-109">Scheduler vytváří, udržuje a vyvolává naplánovanou práci.</span><span class="sxs-lookup"><span data-stu-id="a8ae7-109">Scheduler creates, maintains, and invokes scheduled work.</span></span>  <span data-ttu-id="a8ae7-110">Scheduler nehostuje žádné úlohy ani nespouští žádný kód.</span><span class="sxs-lookup"><span data-stu-id="a8ae7-110">Scheduler does not host any workloads or run any code.</span></span> <span data-ttu-id="a8ae7-111">On *vyvolává* kód hostovaný jinde – v Azure, v lokálním umístění nebo u jiného poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="a8ae7-111">It only *invokes* code hosted elsewhere—in Azure, on-premises, or with another provider.</span></span> <span data-ttu-id="a8ae7-112">Vyvolává přes HTTP, frontu úložiště, frontu sběrnice nebo téma sběrnice.</span><span class="sxs-lookup"><span data-stu-id="a8ae7-112">It invokes via HTTP, HTTPS, a storage queue, a service bus queue, or a service bus topic.</span></span>

<span data-ttu-id="a8ae7-113">Scheduler plánuje [úlohy](scheduler-concepts-terms.md), uchovává historii výsledků provedení úloh, která jeden můžete zkontrolovat, a deterministicky a spolehlivě spuštění toobe plány úloh.</span><span class="sxs-lookup"><span data-stu-id="a8ae7-113">Scheduler schedules [jobs](scheduler-concepts-terms.md), keeps a history of job execution results that one can review, and deterministically and reliably schedules workloads toobe run.</span></span> <span data-ttu-id="a8ae7-114">Azure WebJobs (součást hello funkce Web Apps v Azure App Service) a další možnosti plánování Azure používají Scheduler hello pozadí.</span><span class="sxs-lookup"><span data-stu-id="a8ae7-114">Azure WebJobs (part of hello Web Apps feature in Azure App Service) and other Azure scheduling capabilities use Scheduler in hello background.</span></span> <span data-ttu-id="a8ae7-115">Hello [REST API Scheduleru](https://msdn.microsoft.com/library/mt629143.aspx) pomáhá spravovat hello komunikaci pro tyto akce.</span><span class="sxs-lookup"><span data-stu-id="a8ae7-115">hello [Scheduler REST API](https://msdn.microsoft.com/library/mt629143.aspx) helps manage hello communication for these actions.</span></span> <span data-ttu-id="a8ae7-116">Scheduler jako takový bez problémů podporuje [komplexní plánování a pokročilé opakování](scheduler-advanced-complexity.md).</span><span class="sxs-lookup"><span data-stu-id="a8ae7-116">As such, Scheduler supports [complex schedules and advanced recurrence](scheduler-advanced-complexity.md) easily.</span></span>

<span data-ttu-id="a8ae7-117">Existuje několik situací, se samo toohello použití Scheduleru.</span><span class="sxs-lookup"><span data-stu-id="a8ae7-117">There are several scenarios that lend themselves toohello usage of Scheduler.</span></span> <span data-ttu-id="a8ae7-118">Například:</span><span class="sxs-lookup"><span data-stu-id="a8ae7-118">For example:</span></span>

* <span data-ttu-id="a8ae7-119">*Opakující se akce aplikace:* Pravidelné shromažďování dat z Twitteru do informačního kanálu.</span><span class="sxs-lookup"><span data-stu-id="a8ae7-119">*Recurring application actions:* Periodically gathering data from Twitter into a feed.</span></span>
* <span data-ttu-id="a8ae7-120">*Každodenní údržba:* Každodenní vyřazování protokolů, zálohování a další úkoly týkající se údržby.</span><span class="sxs-lookup"><span data-stu-id="a8ae7-120">*Daily maintenance:* Daily pruning of logs, performing backups, and other maintenance tasks.</span></span> <span data-ttu-id="a8ae7-121">Například může správce zvolit tooback hello databáze v 1:00</span><span class="sxs-lookup"><span data-stu-id="a8ae7-121">For example, an administrator may choose tooback up hello database at 1:00 A.M.</span></span> <span data-ttu-id="a8ae7-122">každý den pro hello dalších devět měsíců.</span><span class="sxs-lookup"><span data-stu-id="a8ae7-122">every day for hello next nine months.</span></span>

<span data-ttu-id="a8ae7-123">Scheduler vám umožní toocreate, aktualizovat, odstranit, zobrazit a spravovat úlohy a [kolekce úloh](scheduler-concepts-terms.md) programově pomocí skriptů a hello portálu.</span><span class="sxs-lookup"><span data-stu-id="a8ae7-123">Scheduler allows you toocreate, update, delete, view, and manage jobs and [job collections](scheduler-concepts-terms.md) programmatically, by using scripts, and in hello portal.</span></span>

## <a name="see-also"></a><span data-ttu-id="a8ae7-124">Viz také</span><span class="sxs-lookup"><span data-stu-id="a8ae7-124">See also</span></span>
 [<span data-ttu-id="a8ae7-125">Koncepty, terminologie a hierarchie entit Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="a8ae7-125">Azure Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="a8ae7-126">Začněte používat plánovače v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a8ae7-126">Get started using Scheduler in hello Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="a8ae7-127">Plány a fakturace v Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="a8ae7-127">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="a8ae7-128">Jak toobuild komplexní plány a pokročilé opakování ve službě Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="a8ae7-128">How toobuild complex schedules and advanced recurrence with Azure Scheduler</span></span>](scheduler-advanced-complexity.md)

 [<span data-ttu-id="a8ae7-129">REST API Azure Scheduleru – referenční informace</span><span class="sxs-lookup"><span data-stu-id="a8ae7-129">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="a8ae7-130">Rutiny PowerShellu pro Azure Scheduler – referenční informace</span><span class="sxs-lookup"><span data-stu-id="a8ae7-130">Azure Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="a8ae7-131">Vysoká dostupnost a spolehlivost Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="a8ae7-131">Azure Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="a8ae7-132">Omezení, výchozí hodnoty a chybové kódy Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="a8ae7-132">Azure Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

 [<span data-ttu-id="a8ae7-133">Odchozí ověření Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="a8ae7-133">Azure Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

