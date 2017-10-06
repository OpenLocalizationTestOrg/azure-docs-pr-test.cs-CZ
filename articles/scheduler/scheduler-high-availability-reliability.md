---
title: aaaScheduler vysokou dostupnost a spolehlivost
description: Scheduler vysokou dostupnost a spolehlivost
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 5ec78e60-a9b9-405a-91a8-f010f3872d50
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/16/2016
ms.author: deli
ms.openlocfilehash: 5c9efb333eb42b393adc5deea657ca99206d425e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scheduler-high-availability-and-reliability"></a><span data-ttu-id="61ae8-103">Scheduler vysokou dostupnost a spolehlivost</span><span class="sxs-lookup"><span data-stu-id="61ae8-103">Scheduler High-Availability and Reliability</span></span>
## <a name="azure-scheduler-high-availability"></a><span data-ttu-id="61ae8-104">Vysoká dostupnost Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="61ae8-104">Azure Scheduler High-Availability</span></span>
<span data-ttu-id="61ae8-105">Jako základní platformu Azure služby Azure Scheduler je vysoce dostupný a funkce nasazení geograficky redundantní služby a úlohy místní geografické replikace.</span><span class="sxs-lookup"><span data-stu-id="61ae8-105">As a core Azure platform service, Azure Scheduler is highly available and features both geo-redundant service deployment and geo-regional job replication.</span></span>

### <a name="geo-redundant-service-deployment"></a><span data-ttu-id="61ae8-106">Nasazení služby geograficky redundantní</span><span class="sxs-lookup"><span data-stu-id="61ae8-106">Geo-redundant service deployment</span></span>
<span data-ttu-id="61ae8-107">Azure Scheduler je k dispozici prostřednictvím hello uživatelského rozhraní v téměř každé geografické oblasti, která je v Azure ještě dnes.</span><span class="sxs-lookup"><span data-stu-id="61ae8-107">Azure Scheduler is available via hello UI in almost every geo region that's in Azure today.</span></span> <span data-ttu-id="61ae8-108">Hello seznam oblastí, které jsou k dispozici ve službě Azure Scheduler je [tady](https://azure.microsoft.com/regions/#services).</span><span class="sxs-lookup"><span data-stu-id="61ae8-108">hello list of regions that Azure Scheduler is available in is [listed here](https://azure.microsoft.com/regions/#services).</span></span> <span data-ttu-id="61ae8-109">Pokud datového centra v hostované oblasti vykresleno není k dispozici, jsou hello převzetí služeb při selhání služby Azure Scheduler tak, aby služba hello je dostupná z jiného datového centra.</span><span class="sxs-lookup"><span data-stu-id="61ae8-109">If a data center in a hosted region is rendered unavailable, hello failover capabilities of Azure Scheduler are such that hello service is available from another data center.</span></span>

### <a name="geo-regional-job-replication"></a><span data-ttu-id="61ae8-110">Geograficky místní úlohy replikace</span><span class="sxs-lookup"><span data-stu-id="61ae8-110">Geo-regional job replication</span></span>
<span data-ttu-id="61ae8-111">Je nejen hello Azure Scheduler front-end pro požadavky na správu, ale vlastní úlohu k dispozici je také geograficky replikované.</span><span class="sxs-lookup"><span data-stu-id="61ae8-111">Not only is hello Azure Scheduler front-end available for management requests, but your own job is also geo-replicated.</span></span> <span data-ttu-id="61ae8-112">Po výpadku v jedné oblasti Azure Scheduler převezme a zajišťuje, že spuštění této úlohy hello z jiného datového centra v hello spárované geografické oblasti.</span><span class="sxs-lookup"><span data-stu-id="61ae8-112">When there’s an outage in one region, Azure Scheduler fails over and ensures that hello job is run from another data center in hello paired geographic region.</span></span>

<span data-ttu-id="61ae8-113">Například pokud jste vytvořili úlohu v jihu USA, Azure Scheduler automaticky replikuje této úlohy v Severní jihu USA.</span><span class="sxs-lookup"><span data-stu-id="61ae8-113">For example, if you’ve created a job in South Central US, Azure Scheduler automatically replicates that job in North Central US.</span></span> <span data-ttu-id="61ae8-114">Když dojde k selhání v jihu USA, Azure Scheduler zajistí, že spuštění této úlohy hello z Sever střední USA.</span><span class="sxs-lookup"><span data-stu-id="61ae8-114">When there’s a failure in South Central US, Azure Scheduler ensures that hello job is run from North Central US.</span></span> 

![][1]

<span data-ttu-id="61ae8-115">V důsledku toho Azure Scheduler zajistí, že vaše data zůstane v rámci hello širší stejné zeměpisné oblasti v případě selhání Azure.</span><span class="sxs-lookup"><span data-stu-id="61ae8-115">As a result, Azure Scheduler ensures that your data stays within hello same broader geographic region in case of an Azure failure.</span></span> <span data-ttu-id="61ae8-116">V důsledku toho nemusí duplicitní vysokou dostupnost vaší úlohy jenom tooadd – Azure Scheduler automaticky poskytuje vysokou dostupnost funkcí pro úlohy.</span><span class="sxs-lookup"><span data-stu-id="61ae8-116">As a result, you need not duplicate your job just tooadd high availability – Azure Scheduler automatically provides high-availability capabilities for your jobs.</span></span>

## <a name="azure-scheduler-reliability"></a><span data-ttu-id="61ae8-117">Spolehlivost Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="61ae8-117">Azure Scheduler Reliability</span></span>
<span data-ttu-id="61ae8-118">Azure Scheduler zaručuje vlastní vysokou dostupnost a přistupují toouser vytvoření úlohy.</span><span class="sxs-lookup"><span data-stu-id="61ae8-118">Azure Scheduler guarantees its own high-availability and takes a different approach toouser-created jobs.</span></span> <span data-ttu-id="61ae8-119">Například vaší práce může vyvolat koncový bod protokolu HTTP, který je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="61ae8-119">For example, your job may invoke an HTTP endpoint that’s unavailable.</span></span> <span data-ttu-id="61ae8-120">Azure Scheduler přesto pokusí tooexecute úlohu úspěšně, tím, že jste toodeal alternativní možnosti s chybou.</span><span class="sxs-lookup"><span data-stu-id="61ae8-120">Azure Scheduler nonetheless tries tooexecute your job successfully, by giving you alternative options toodeal with failure.</span></span> <span data-ttu-id="61ae8-121">Azure Scheduler k tomu dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="61ae8-121">Azure Scheduler does this in two ways:</span></span>

### <a name="configurable-retry-policy-via-retrypolicy"></a><span data-ttu-id="61ae8-122">Konfigurovat zásady opakujte prostřednictvím "retryPolicy"</span><span class="sxs-lookup"><span data-stu-id="61ae8-122">Configurable Retry Policy via “retryPolicy”</span></span>
<span data-ttu-id="61ae8-123">Azure Scheduler vám umožní tooconfigure zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="61ae8-123">Azure Scheduler allows you tooconfigure a retry policy.</span></span> <span data-ttu-id="61ae8-124">Ve výchozím nastavení pokud úloha selže, Scheduler pokusí hello úlohy znovu čtyři vícekrát v intervalech 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="61ae8-124">By default, if a job fails, Scheduler tries hello job again four more times, at 30-second intervals.</span></span> <span data-ttu-id="61ae8-125">Můžete znovu nakonfigurovat tato opakování zásad toobe agresivnější (například desetkrát, v intervalech 30 sekund) nebo volnější (například dvakrát, v denních intervalech.)</span><span class="sxs-lookup"><span data-stu-id="61ae8-125">You may re-configure this retry policy toobe more aggressive (for example, ten times, at 30-second intervals) or looser (for example, two times, at daily intervals.)</span></span>

<span data-ttu-id="61ae8-126">Jako příklad když to může pomoct můžete vytvořit úlohu, která spouští jednou za týden a vyvolá koncový bod HTTP.</span><span class="sxs-lookup"><span data-stu-id="61ae8-126">As an example of when this may help, you may create a job that runs once a week and invokes an HTTP endpoint.</span></span> <span data-ttu-id="61ae8-127">Pokud koncový bod hello HTTP je vypnutý po několik hodin, když vaše úloha běží, nemusí chcete toowait jeden další týden v případě hello úlohy toorun znovu vzhledem k tomu, že selžou i hello zásady opakování výchozí.</span><span class="sxs-lookup"><span data-stu-id="61ae8-127">If hello HTTP endpoint is down for a few hours when your job runs, you may not want toowait one more week for hello job toorun again since even hello default retry policy will fail.</span></span> <span data-ttu-id="61ae8-128">V takových případech může překonfigurujete hello standardní opakování zásad tooretry každé tři hodiny (například) namísto každých 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="61ae8-128">In such cases, you may reconfigure hello standard retry policy tooretry every three hours (for example) instead of every 30 seconds.</span></span>

<span data-ttu-id="61ae8-129">jak tooconfigure zásady opakovaných pokusů, odkazovat příliš toolearn[retryPolicy](scheduler-concepts-terms.md#retrypolicy).</span><span class="sxs-lookup"><span data-stu-id="61ae8-129">toolearn how tooconfigure a retry policy, refer too[retryPolicy](scheduler-concepts-terms.md#retrypolicy).</span></span>

### <a name="alternate-endpoint-configurability-via-erroraction"></a><span data-ttu-id="61ae8-130">Alternativní koncový bod možnosti konfigurace: prostřednictvím "errorAction"</span><span class="sxs-lookup"><span data-stu-id="61ae8-130">Alternate Endpoint Configurability via “errorAction”</span></span>
<span data-ttu-id="61ae8-131">Pokud hello cílového koncového bodu pro úlohu Azure Scheduler zůstane nedostupný, Azure Scheduler přejde toohello alternativní koncový bod zpracování chyb po následující jeho zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="61ae8-131">If hello target endpoint for your Azure Scheduler job remains unreachable, Azure Scheduler falls back toohello alternate error-handling endpoint after following its retry policy.</span></span> <span data-ttu-id="61ae8-132">Pokud je nakonfigurovaný koncový bod alternativní zpracování chyb, Azure Scheduler jej spustí.</span><span class="sxs-lookup"><span data-stu-id="61ae8-132">If an alternate error-handling endpoint is configured, Azure Scheduler invokes it.</span></span> <span data-ttu-id="61ae8-133">Alternativní koncový bod vlastní úlohy jsou vysoce dostupné v hello vzhled selhání.</span><span class="sxs-lookup"><span data-stu-id="61ae8-133">With an alternate endpoint, your own jobs are highly available in hello face of failure.</span></span>

<span data-ttu-id="61ae8-134">Jako příklad hello obrázku níže Azure Scheduler odpovídá jeho toohit zásady opakování New Yorku webové služby.</span><span class="sxs-lookup"><span data-stu-id="61ae8-134">As an example, in hello diagram below, Azure Scheduler follows its retry policy toohit a New York web service.</span></span> <span data-ttu-id="61ae8-135">Po hello opakuje pokus selže, zkontroluje, jestli je alternativním.</span><span class="sxs-lookup"><span data-stu-id="61ae8-135">After hello retries fail, it checks if there's an alternate.</span></span> <span data-ttu-id="61ae8-136">Potom přejde dopředu a spustí provádění požadavků toohello alternativní s hello stejné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="61ae8-136">It then goes ahead and starts making requests toohello alternate with hello same retry policy.</span></span>

![][2]

<span data-ttu-id="61ae8-137">Všimněte si, že hello stejné zásady opakování platí tooboth hello původní akce a akce alternativní chyby hello.</span><span class="sxs-lookup"><span data-stu-id="61ae8-137">Note that hello same retry policy applies tooboth hello original action and hello alternate error action.</span></span> <span data-ttu-id="61ae8-138">Typ akce jeho také možné toohave hello alternativní Chyba akce být odlišný od typu akce hello hlavní akce.</span><span class="sxs-lookup"><span data-stu-id="61ae8-138">It’s also possible toohave hello alternate error action’s action type be different from hello main action’s action type.</span></span> <span data-ttu-id="61ae8-139">Například při hello hlavní akce může být vyvolání koncový bod protokolu HTTP, akce chyby hello místo pravděpodobně fronty úložiště, fronty service bus nebo akce témata sběrnice služby, která provádí protokolování chyb.</span><span class="sxs-lookup"><span data-stu-id="61ae8-139">For example, while hello main action may be invoking an HTTP endpoint, hello error action may instead be a storage queue, service bus queue, or service bus topic action that does error-logging.</span></span>

<span data-ttu-id="61ae8-140">jak tooconfigure alternativní koncový bod, najdete příliš toolearn[errorAction](scheduler-concepts-terms.md#action-and-erroraction).</span><span class="sxs-lookup"><span data-stu-id="61ae8-140">toolearn how tooconfigure an alternate endpoint, refer too[errorAction](scheduler-concepts-terms.md#action-and-erroraction).</span></span>

## <a name="see-also"></a><span data-ttu-id="61ae8-141">Viz také</span><span class="sxs-lookup"><span data-stu-id="61ae8-141">See Also</span></span>
 [<span data-ttu-id="61ae8-142">Co je Scheduler?</span><span class="sxs-lookup"><span data-stu-id="61ae8-142">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="61ae8-143">Koncepty, terminologie a hierarchie entit Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="61ae8-143">Azure Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="61ae8-144">Začněte používat plánovače v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="61ae8-144">Get started using Scheduler in hello Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="61ae8-145">Plány a fakturace v Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="61ae8-145">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="61ae8-146">Jak toobuild komplexní plány a pokročilé opakování ve službě Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="61ae8-146">How toobuild complex schedules and advanced recurrence with Azure Scheduler</span></span>](scheduler-advanced-complexity.md)

 [<span data-ttu-id="61ae8-147">REST API Azure Scheduleru – referenční informace</span><span class="sxs-lookup"><span data-stu-id="61ae8-147">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="61ae8-148">Rutiny PowerShellu pro Azure Scheduler – referenční informace</span><span class="sxs-lookup"><span data-stu-id="61ae8-148">Azure Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="61ae8-149">Omezení, výchozí hodnoty a chybové kódy Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="61ae8-149">Azure Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

 [<span data-ttu-id="61ae8-150">Odchozí ověření Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="61ae8-150">Azure Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

[1]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png

[2]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png
