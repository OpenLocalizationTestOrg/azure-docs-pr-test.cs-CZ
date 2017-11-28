---
title: Co je Azure Scheduler? | Dokumentace Microsoftu
description: "Azure Scheduler vám umožní deklarativně popisovat akce, které mají běžet v cloudu. Potom naplánuje a automaticky spustí tyto akce."
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
ms.openlocfilehash: a3bf1aacd6978499d7ef77cbcb451a06b857ac38
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-azure-scheduler"></a><span data-ttu-id="21222-105">Co je Azure Scheduler?</span><span class="sxs-lookup"><span data-stu-id="21222-105">What is Azure Scheduler?</span></span>
<span data-ttu-id="21222-106">Azure Scheduler vám umožní deklarativně popisovat akce, které mají běžet v cloudu.</span><span class="sxs-lookup"><span data-stu-id="21222-106">Azure Scheduler allows you to declaratively describe actions to run in the cloud.</span></span> <span data-ttu-id="21222-107">Potom naplánuje a automaticky spustí tyto akce.</span><span class="sxs-lookup"><span data-stu-id="21222-107">It then schedules and runs those actions automatically.</span></span>  <span data-ttu-id="21222-108">Scheduler k tomu použije [portál Azure](scheduler-get-started-portal.md), kód, [REST API](https://msdn.microsoft.com/library/mt629143.aspx) nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="21222-108">Scheduler does this by using [the Azure portal](scheduler-get-started-portal.md), code, [REST API](https://msdn.microsoft.com/library/mt629143.aspx), or Azure PowerShell.</span></span>

<span data-ttu-id="21222-109">Scheduler vytváří, udržuje a vyvolává naplánovanou práci.</span><span class="sxs-lookup"><span data-stu-id="21222-109">Scheduler creates, maintains, and invokes scheduled work.</span></span>  <span data-ttu-id="21222-110">Scheduler nehostuje žádné úlohy ani nespouští žádný kód.</span><span class="sxs-lookup"><span data-stu-id="21222-110">Scheduler does not host any workloads or run any code.</span></span> <span data-ttu-id="21222-111">On *vyvolává* kód hostovaný jinde – v Azure, v lokálním umístění nebo u jiného poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="21222-111">It only *invokes* code hosted elsewhere—in Azure, on-premises, or with another provider.</span></span> <span data-ttu-id="21222-112">Vyvolává přes HTTP, frontu úložiště, frontu sběrnice nebo téma sběrnice.</span><span class="sxs-lookup"><span data-stu-id="21222-112">It invokes via HTTP, HTTPS, a storage queue, a service bus queue, or a service bus topic.</span></span>

<span data-ttu-id="21222-113">Scheduler plánuje [úlohy](scheduler-concepts-terms.md) uchovává historii výsledků provedení úloh, která se dá zobrazit, a deterministicky a spolehlivě plánuje spouštění úloh.</span><span class="sxs-lookup"><span data-stu-id="21222-113">Scheduler schedules [jobs](scheduler-concepts-terms.md), keeps a history of job execution results that one can review, and deterministically and reliably schedules workloads to be run.</span></span> <span data-ttu-id="21222-114">Azure WebJobs (součást funkce Web Apps v Azure App Service) a další plánovací nástroje Azure používají Scheduler na pozadí.</span><span class="sxs-lookup"><span data-stu-id="21222-114">Azure WebJobs (part of the Web Apps feature in Azure App Service) and other Azure scheduling capabilities use Scheduler in the background.</span></span> <span data-ttu-id="21222-115">[REST API Scheduleru ](https://msdn.microsoft.com/library/mt629143.aspx) pomáhá spravovat komunikaci pro tyto akce.</span><span class="sxs-lookup"><span data-stu-id="21222-115">The [Scheduler REST API](https://msdn.microsoft.com/library/mt629143.aspx) helps manage the communication for these actions.</span></span> <span data-ttu-id="21222-116">Scheduler jako takový bez problémů podporuje [komplexní plánování a pokročilé opakování](scheduler-advanced-complexity.md).</span><span class="sxs-lookup"><span data-stu-id="21222-116">As such, Scheduler supports [complex schedules and advanced recurrence](scheduler-advanced-complexity.md) easily.</span></span>

<span data-ttu-id="21222-117">Použití Scheduleru se samo nabízí v několika situacích.</span><span class="sxs-lookup"><span data-stu-id="21222-117">There are several scenarios that lend themselves to the usage of Scheduler.</span></span> <span data-ttu-id="21222-118">Příklad:</span><span class="sxs-lookup"><span data-stu-id="21222-118">For example:</span></span>

* <span data-ttu-id="21222-119">*Opakující se akce aplikace:* Pravidelné shromažďování dat z Twitteru do informačního kanálu.</span><span class="sxs-lookup"><span data-stu-id="21222-119">*Recurring application actions:* Periodically gathering data from Twitter into a feed.</span></span>
* <span data-ttu-id="21222-120">*Každodenní údržba:* Každodenní vyřazování protokolů, zálohování a další úkoly týkající se údržby.</span><span class="sxs-lookup"><span data-stu-id="21222-120">*Daily maintenance:* Daily pruning of logs, performing backups, and other maintenance tasks.</span></span> <span data-ttu-id="21222-121">Správce se například může rozhodnout zálohovat databáze v 1:00 v noci</span><span class="sxs-lookup"><span data-stu-id="21222-121">For example, an administrator may choose to back up the database at 1:00 A.M.</span></span> <span data-ttu-id="21222-122">každý den dalších devět měsíců.</span><span class="sxs-lookup"><span data-stu-id="21222-122">every day for the next nine months.</span></span>

<span data-ttu-id="21222-123">Scheduler umožní vytvářet, aktualizovat, odstraňovat, zobrazovat a spravovat úlohy a [kolekce úloh](scheduler-concepts-terms.md) programově pomocí skriptů a pomocí portálu.</span><span class="sxs-lookup"><span data-stu-id="21222-123">Scheduler allows you to create, update, delete, view, and manage jobs and [job collections](scheduler-concepts-terms.md) programmatically, by using scripts, and in the portal.</span></span>

## <a name="see-also"></a><span data-ttu-id="21222-124">Viz také</span><span class="sxs-lookup"><span data-stu-id="21222-124">See also</span></span>
 [<span data-ttu-id="21222-125">Koncepty, terminologie a hierarchie entit Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="21222-125">Azure Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="21222-126">Úvod do používání Scheduleru na portálu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="21222-126">Get started using Scheduler in the Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="21222-127">Plány a fakturace v Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="21222-127">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="21222-128">Sestavení komplexních plánů a pokročilé opakování v Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="21222-128">How to build complex schedules and advanced recurrence with Azure Scheduler</span></span>](scheduler-advanced-complexity.md)

 [<span data-ttu-id="21222-129">REST API Azure Scheduleru – referenční informace</span><span class="sxs-lookup"><span data-stu-id="21222-129">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="21222-130">Rutiny PowerShellu pro Azure Scheduler – referenční informace</span><span class="sxs-lookup"><span data-stu-id="21222-130">Azure Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="21222-131">Vysoká dostupnost a spolehlivost Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="21222-131">Azure Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="21222-132">Omezení, výchozí hodnoty a chybové kódy Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="21222-132">Azure Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

 [<span data-ttu-id="21222-133">Odchozí ověření Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="21222-133">Azure Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

