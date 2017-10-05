---
title: Scheduler vysokou dostupnost a spolehlivost
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
ms.openlocfilehash: 7e7fe49de7814b6058468d630f8638720e5864f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="scheduler-high-availability-and-reliability"></a><span data-ttu-id="e0860-103">Scheduler vysokou dostupnost a spolehlivost</span><span class="sxs-lookup"><span data-stu-id="e0860-103">Scheduler High-Availability and Reliability</span></span>
## <a name="azure-scheduler-high-availability"></a><span data-ttu-id="e0860-104">Vysoká dostupnost Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="e0860-104">Azure Scheduler High-Availability</span></span>
<span data-ttu-id="e0860-105">Jako základní platformu Azure služby Azure Scheduler je vysoce dostupný a funkce nasazení geograficky redundantní služby a úlohy místní geografické replikace.</span><span class="sxs-lookup"><span data-stu-id="e0860-105">As a core Azure platform service, Azure Scheduler is highly available and features both geo-redundant service deployment and geo-regional job replication.</span></span>

### <a name="geo-redundant-service-deployment"></a><span data-ttu-id="e0860-106">Nasazení služby geograficky redundantní</span><span class="sxs-lookup"><span data-stu-id="e0860-106">Geo-redundant service deployment</span></span>
<span data-ttu-id="e0860-107">Azure Scheduler je k dispozici prostřednictvím uživatelského rozhraní v téměř každé geografické oblasti, která je v Azure ještě dnes.</span><span class="sxs-lookup"><span data-stu-id="e0860-107">Azure Scheduler is available via the UI in almost every geo region that's in Azure today.</span></span> <span data-ttu-id="e0860-108">Seznam oblastí, které jsou k dispozici ve službě Azure Scheduler je [tady](https://azure.microsoft.com/regions/#services).</span><span class="sxs-lookup"><span data-stu-id="e0860-108">The list of regions that Azure Scheduler is available in is [listed here](https://azure.microsoft.com/regions/#services).</span></span> <span data-ttu-id="e0860-109">Pokud datového centra v hostované oblasti vykresleno není k dispozici, jsou možnosti převzetí služeb při selhání Azure Scheduler tak, že služba je k dispozici z jiného datového centra.</span><span class="sxs-lookup"><span data-stu-id="e0860-109">If a data center in a hosted region is rendered unavailable, the failover capabilities of Azure Scheduler are such that the service is available from another data center.</span></span>

### <a name="geo-regional-job-replication"></a><span data-ttu-id="e0860-110">Geograficky místní úlohy replikace</span><span class="sxs-lookup"><span data-stu-id="e0860-110">Geo-regional job replication</span></span>
<span data-ttu-id="e0860-111">Ne jenom je Azure Scheduler front-end pro požadavky na správu, ale vlastní úlohu k dispozici je také geograficky replikované.</span><span class="sxs-lookup"><span data-stu-id="e0860-111">Not only is the Azure Scheduler front-end available for management requests, but your own job is also geo-replicated.</span></span> <span data-ttu-id="e0860-112">Po výpadku v jedné oblasti Azure Scheduler převezme a zajistí, že se úlohu spustit z jiného datového centra ve spárované zeměpisné oblasti.</span><span class="sxs-lookup"><span data-stu-id="e0860-112">When there’s an outage in one region, Azure Scheduler fails over and ensures that the job is run from another data center in the paired geographic region.</span></span>

<span data-ttu-id="e0860-113">Například pokud jste vytvořili úlohu v jihu USA, Azure Scheduler automaticky replikuje této úlohy v Severní jihu USA.</span><span class="sxs-lookup"><span data-stu-id="e0860-113">For example, if you’ve created a job in South Central US, Azure Scheduler automatically replicates that job in North Central US.</span></span> <span data-ttu-id="e0860-114">Když dojde k selhání v jihu USA, Azure Scheduler zajistí úlohy je z Sever střední USA.</span><span class="sxs-lookup"><span data-stu-id="e0860-114">When there’s a failure in South Central US, Azure Scheduler ensures that the job is run from North Central US.</span></span> 

![][1]

<span data-ttu-id="e0860-115">V důsledku toho Azure Scheduler zajistí, že vaše data zůstává v rámci stejné zeměpisné oblasti širší v případě selhání Azure.</span><span class="sxs-lookup"><span data-stu-id="e0860-115">As a result, Azure Scheduler ensures that your data stays within the same broader geographic region in case of an Azure failure.</span></span> <span data-ttu-id="e0860-116">V důsledku toho nemusí duplicitní úlohu jenom k přidání vysoká dostupnost – Azure Scheduler automaticky poskytuje vysokou dostupnost funkcí pro úlohy.</span><span class="sxs-lookup"><span data-stu-id="e0860-116">As a result, you need not duplicate your job just to add high availability – Azure Scheduler automatically provides high-availability capabilities for your jobs.</span></span>

## <a name="azure-scheduler-reliability"></a><span data-ttu-id="e0860-117">Spolehlivost Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="e0860-117">Azure Scheduler Reliability</span></span>
<span data-ttu-id="e0860-118">Azure Scheduler zaručuje vlastní vysokou dostupnost a přistupují k úlohy vytvořené uživatelem.</span><span class="sxs-lookup"><span data-stu-id="e0860-118">Azure Scheduler guarantees its own high-availability and takes a different approach to user-created jobs.</span></span> <span data-ttu-id="e0860-119">Například vaší práce může vyvolat koncový bod protokolu HTTP, který je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="e0860-119">For example, your job may invoke an HTTP endpoint that’s unavailable.</span></span> <span data-ttu-id="e0860-120">Azure Scheduler přesto pokusí provést úlohu úspěšně, tím, že jste alternativní možnosti, jak nakládat s chybou.</span><span class="sxs-lookup"><span data-stu-id="e0860-120">Azure Scheduler nonetheless tries to execute your job successfully, by giving you alternative options to deal with failure.</span></span> <span data-ttu-id="e0860-121">Azure Scheduler k tomu dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="e0860-121">Azure Scheduler does this in two ways:</span></span>

### <a name="configurable-retry-policy-via-retrypolicy"></a><span data-ttu-id="e0860-122">Konfigurovat zásady opakujte prostřednictvím "retryPolicy"</span><span class="sxs-lookup"><span data-stu-id="e0860-122">Configurable Retry Policy via “retryPolicy”</span></span>
<span data-ttu-id="e0860-123">Azure Scheduler umožňuje konfigurovat zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="e0860-123">Azure Scheduler allows you to configure a retry policy.</span></span> <span data-ttu-id="e0860-124">Ve výchozím nastavení pokud úloha selže, Scheduler pokusí úlohu znovu čtyři vícekrát v intervalech 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="e0860-124">By default, if a job fails, Scheduler tries the job again four more times, at 30-second intervals.</span></span> <span data-ttu-id="e0860-125">Můžete znovu nakonfigurovat tyto zásady opakování být agresivnější (například desetkrát, v intervalech 30 sekund) nebo volnější (například dvakrát, v denních intervalech.)</span><span class="sxs-lookup"><span data-stu-id="e0860-125">You may re-configure this retry policy to be more aggressive (for example, ten times, at 30-second intervals) or looser (for example, two times, at daily intervals.)</span></span>

<span data-ttu-id="e0860-126">Jako příklad když to může pomoct můžete vytvořit úlohu, která spouští jednou za týden a vyvolá koncový bod HTTP.</span><span class="sxs-lookup"><span data-stu-id="e0860-126">As an example of when this may help, you may create a job that runs once a week and invokes an HTTP endpoint.</span></span> <span data-ttu-id="e0860-127">Pokud koncový bod HTTP je vypnutý po několik hodin, když vaše úloha běží, nemusí chcete čekat jeden další týden pro úlohu znovu spustit, protože i výchozí zásady opakovaných pokusů se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="e0860-127">If the HTTP endpoint is down for a few hours when your job runs, you may not want to wait one more week for the job to run again since even the default retry policy will fail.</span></span> <span data-ttu-id="e0860-128">V takových případech může znovu nakonfigurovat zásady opakování standardní opakovat každé tři hodiny, (například) namísto každých 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="e0860-128">In such cases, you may reconfigure the standard retry policy to retry every three hours (for example) instead of every 30 seconds.</span></span>

<span data-ttu-id="e0860-129">Zjistěte, jak nakonfigurovat zásady opakovaných pokusů, najdete v tématu [retryPolicy](scheduler-concepts-terms.md#retrypolicy).</span><span class="sxs-lookup"><span data-stu-id="e0860-129">To learn how to configure a retry policy, refer to [retryPolicy](scheduler-concepts-terms.md#retrypolicy).</span></span>

### <a name="alternate-endpoint-configurability-via-erroraction"></a><span data-ttu-id="e0860-130">Alternativní koncový bod možnosti konfigurace: prostřednictvím "errorAction"</span><span class="sxs-lookup"><span data-stu-id="e0860-130">Alternate Endpoint Configurability via “errorAction”</span></span>
<span data-ttu-id="e0860-131">Pokud cílový koncový bod pro úlohu Azure Scheduler zůstane nedostupný, Azure Scheduler spadne zpět na alternativní koncový bod zpracování chyb po následující jeho zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="e0860-131">If the target endpoint for your Azure Scheduler job remains unreachable, Azure Scheduler falls back to the alternate error-handling endpoint after following its retry policy.</span></span> <span data-ttu-id="e0860-132">Pokud je nakonfigurovaný koncový bod alternativní zpracování chyb, Azure Scheduler jej spustí.</span><span class="sxs-lookup"><span data-stu-id="e0860-132">If an alternate error-handling endpoint is configured, Azure Scheduler invokes it.</span></span> <span data-ttu-id="e0860-133">Alternativní koncový bod jsou vysoce dostupné při krátkodobém selhání vlastní úlohy.</span><span class="sxs-lookup"><span data-stu-id="e0860-133">With an alternate endpoint, your own jobs are highly available in the face of failure.</span></span>

<span data-ttu-id="e0860-134">Jako příklad na obrázku níže Azure Scheduler odpovídá jeho zásady opakovaných pokusů a stiskněte tlačítko New Yorku webové služby.</span><span class="sxs-lookup"><span data-stu-id="e0860-134">As an example, in the diagram below, Azure Scheduler follows its retry policy to hit a New York web service.</span></span> <span data-ttu-id="e0860-135">Po opakované pokusy selžou, zkontroluje, jestli je alternativním.</span><span class="sxs-lookup"><span data-stu-id="e0860-135">After the retries fail, it checks if there's an alternate.</span></span> <span data-ttu-id="e0860-136">Potom přejde dopředu a spustí zasílání požadavků na alternativní s stejné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="e0860-136">It then goes ahead and starts making requests to the alternate with the same retry policy.</span></span>

![][2]

<span data-ttu-id="e0860-137">Všimněte si, že platí stejné zásady opakovaných pokusů pro původní akce a akce alternativní chyby.</span><span class="sxs-lookup"><span data-stu-id="e0860-137">Note that the same retry policy applies to both the original action and the alternate error action.</span></span> <span data-ttu-id="e0860-138">Je také možné, že typ akce akce alternativní chyby se liší od hlavní akce typ akce.</span><span class="sxs-lookup"><span data-stu-id="e0860-138">It’s also possible to have the alternate error action’s action type be different from the main action’s action type.</span></span> <span data-ttu-id="e0860-139">Například při hlavní akce může být vyvolání koncový bod protokolu HTTP, Chyba akce místo pravděpodobně fronty úložiště, fronty service bus nebo akce témata sběrnice služby, která provádí protokolování chyb.</span><span class="sxs-lookup"><span data-stu-id="e0860-139">For example, while the main action may be invoking an HTTP endpoint, the error action may instead be a storage queue, service bus queue, or service bus topic action that does error-logging.</span></span>

<span data-ttu-id="e0860-140">Naučte se konfigurovat alternativní koncový bod, najdete v tématu [errorAction](scheduler-concepts-terms.md#action-and-erroraction).</span><span class="sxs-lookup"><span data-stu-id="e0860-140">To learn how to configure an alternate endpoint, refer to [errorAction](scheduler-concepts-terms.md#action-and-erroraction).</span></span>

## <a name="see-also"></a><span data-ttu-id="e0860-141">Viz také</span><span class="sxs-lookup"><span data-stu-id="e0860-141">See Also</span></span>
 [<span data-ttu-id="e0860-142">Co je Scheduler?</span><span class="sxs-lookup"><span data-stu-id="e0860-142">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="e0860-143">Koncepty, terminologie a hierarchie entit Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="e0860-143">Azure Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="e0860-144">Úvod do používání Scheduleru na portálu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e0860-144">Get started using Scheduler in the Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="e0860-145">Plány a fakturace v Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="e0860-145">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="e0860-146">Sestavení komplexních plánů a pokročilé opakování v Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="e0860-146">How to build complex schedules and advanced recurrence with Azure Scheduler</span></span>](scheduler-advanced-complexity.md)

 [<span data-ttu-id="e0860-147">REST API Azure Scheduleru – referenční informace</span><span class="sxs-lookup"><span data-stu-id="e0860-147">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="e0860-148">Rutiny PowerShellu pro Azure Scheduler – referenční informace</span><span class="sxs-lookup"><span data-stu-id="e0860-148">Azure Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="e0860-149">Omezení, výchozí hodnoty a chybové kódy Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="e0860-149">Azure Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

 [<span data-ttu-id="e0860-150">Odchozí ověření Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="e0860-150">Azure Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

[1]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png

[2]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png
