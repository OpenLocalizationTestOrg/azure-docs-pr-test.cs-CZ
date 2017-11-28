---
title: "aaaUse čítače výkonu v Azure Diagnostics | Microsoft Docs"
description: "Pomocí čítače výkonu v cloudové služby Azure nebo virtuální počítač toofind kritická místa a optimalizaci výkonu."
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: tysonn
ms.assetid: fc4c61e9-d49d-4ed9-a32c-b91cdf851909
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/29/2016
ms.author: robb
ms.openlocfilehash: f3250816c01fc6e164a6aae48da5035845e6d2b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-use-performance-counters-in-an-azure-application"></a><span data-ttu-id="1068c-103">Vytváření a používání čítače výkonu v aplikaci Azure</span><span class="sxs-lookup"><span data-stu-id="1068c-103">Create and use performance counters in an Azure application</span></span>
<span data-ttu-id="1068c-104">Tento článek popisuje hello výhody a jak čítače výkonu tooput do aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="1068c-104">This article describes hello benefits of and how tooput performance counters into your Azure application.</span></span> <span data-ttu-id="1068c-105">Můžete je použít toocollect data, najít kritická místa a vyladit výkon systému a aplikací.</span><span class="sxs-lookup"><span data-stu-id="1068c-105">You can use them toocollect data, find bottlenecks, and tune system and application performance.</span></span>

<span data-ttu-id="1068c-106">Čítače výkonu, které jsou k dispozici pro Windows Server, služba IIS a ASP.NET může také shromažďovat a použít toodetermine hello stavu Azure webové role, role pracovního procesu a virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1068c-106">Performance counters available for Windows Server, IIS, and ASP.NET can also be collected and used toodetermine hello health of your Azure web roles, worker roles, and Virtual Machines.</span></span> <span data-ttu-id="1068c-107">Můžete také vytvořit a použít vlastní čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="1068c-107">You can also create and use custom performance counters.</span></span>  

<span data-ttu-id="1068c-108">Můžete zkontrolovat data čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="1068c-108">You can examine performance counter data</span></span>

1. <span data-ttu-id="1068c-109">Přímo na hostiteli aplikace hello s nástrojem Sledování výkonu hello přistupovat pomocí vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="1068c-109">Directly on hello application host with hello Performance Monitor tool accessed using Remote Desktop</span></span>
2. <span data-ttu-id="1068c-110">S pomocí hello Azure Management Pack nástroje System Center Operations Manager</span><span class="sxs-lookup"><span data-stu-id="1068c-110">With System Center Operations Manager using hello Azure Management Pack</span></span>
3. <span data-ttu-id="1068c-111">Pomocí jiných nástrojů monitorování, které přístup hello diagnostických dat přenesených tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="1068c-111">With other monitoring tools that access hello diagnostic data transferred tooAzure storage.</span></span> <span data-ttu-id="1068c-112">V tématu [úložiště a zobrazení diagnostických dat ve službě Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="1068c-112">See [Store and View Diagnostic Data in Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) for more information.</span></span>  

<span data-ttu-id="1068c-113">Další informace o sledování výkonu hello vaší aplikace v hello [portál Azure](http://portal.azure.com/), najdete v části [jak tooMonitor cloudových služeb](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/).</span><span class="sxs-lookup"><span data-stu-id="1068c-113">For more information on monitoring hello performance of your application in hello [Azure portal](http://portal.azure.com/), see [How tooMonitor Cloud Services](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/).</span></span>

<span data-ttu-id="1068c-114">Další podrobné pokyny k vytváření protokolování a trasování strategie a používání diagnostiky a jiné problémy tootroubleshoot technik a optimalizaci aplikace Azure, najdete v části [řešení potíží s osvědčené postupy pro vývoj Azure Aplikace](https://msdn.microsoft.com/library/azure/hh771389.aspx).</span><span class="sxs-lookup"><span data-stu-id="1068c-114">For additional in-depth guidance on creating a logging and tracing strategy and using diagnostics and other techniques tootroubleshoot problems and optimize Azure applications, see [Troubleshooting Best Practices for Developing Azure Applications](https://msdn.microsoft.com/library/azure/hh771389.aspx).</span></span>

## <a name="enable-performance-counter-monitoring"></a><span data-ttu-id="1068c-115">Povolit monitorování čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="1068c-115">Enable performance counter monitoring</span></span>
<span data-ttu-id="1068c-116">Ve výchozím nastavení nejsou povoleny čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="1068c-116">Performance counters are not enabled by default.</span></span> <span data-ttu-id="1068c-117">Spuštění úlohy nebo aplikace, musíte upravit diagnostiky výchozí hello čítače výkonu specifických hello tooinclude konfigurace agenta chcete toomonitor pro každou instanci role.</span><span class="sxs-lookup"><span data-stu-id="1068c-117">Your application or a startup task must modify hello default diagnostics agent configuration tooinclude hello specific performance counters that you wish toomonitor for each role instance.</span></span>

### <a name="performance-counters-available-for-microsoft-azure"></a><span data-ttu-id="1068c-118">Čítače výkonu, které jsou k dispozici pro Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="1068c-118">Performance counters available for Microsoft Azure</span></span>
<span data-ttu-id="1068c-119">Azure poskytuje podmnožinu hello čítačů výkonu, které jsou k dispozici pro Windows Server, služba IIS a hello zásobníku technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1068c-119">Azure provides a subset of hello performance counters available for Windows Server, IIS and hello ASP.NET stack.</span></span> <span data-ttu-id="1068c-120">Hello následující tabulka uvádí některé čítače výkonu hello konkrétní týkající se aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="1068c-120">hello following table lists some of hello performance counters of particular interest for Azure applications.</span></span>

| <span data-ttu-id="1068c-121">Kategorie čítače: Objekt (Instance)</span><span class="sxs-lookup"><span data-stu-id="1068c-121">Counter Category: Object (Instance)</span></span> | <span data-ttu-id="1068c-122">Název čítače</span><span class="sxs-lookup"><span data-stu-id="1068c-122">Counter Name</span></span> | <span data-ttu-id="1068c-123">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="1068c-123">Reference</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1068c-124">CLR – výjimky na rozhraní .NET (*globální*)</span><span class="sxs-lookup"><span data-stu-id="1068c-124">.NET CLR Exceptions(*Global*)</span></span> |<span data-ttu-id="1068c-125"># Výjimek / s</span><span class="sxs-lookup"><span data-stu-id="1068c-125"># Exceps Thrown / sec</span></span> |<span data-ttu-id="1068c-126">Čítače výkonu výjimky</span><span class="sxs-lookup"><span data-stu-id="1068c-126">Exception Performance Counters</span></span> |
| <span data-ttu-id="1068c-127">Využívání paměti rozhraním .NET CLR (*globální*)</span><span class="sxs-lookup"><span data-stu-id="1068c-127">.NET CLR Memory(*Global*)</span></span> |<span data-ttu-id="1068c-128">Čas</span><span class="sxs-lookup"><span data-stu-id="1068c-128">% Time in GC</span></span> |<span data-ttu-id="1068c-129">Čítače výkonu paměti</span><span class="sxs-lookup"><span data-stu-id="1068c-129">Memory Performance Counters</span></span> |
| <span data-ttu-id="1068c-130">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1068c-130">ASP.NET</span></span> |<span data-ttu-id="1068c-131">Restartování aplikace</span><span class="sxs-lookup"><span data-stu-id="1068c-131">Application Restarts</span></span> |<span data-ttu-id="1068c-132">Čítače výkonu pro technologii ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1068c-132">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="1068c-133">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1068c-133">ASP.NET</span></span> |<span data-ttu-id="1068c-134">Čas provádění požadavku</span><span class="sxs-lookup"><span data-stu-id="1068c-134">Request Execution Time</span></span> |<span data-ttu-id="1068c-135">Čítače výkonu pro technologii ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1068c-135">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="1068c-136">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1068c-136">ASP.NET</span></span> |<span data-ttu-id="1068c-137">Odpojené požadavky</span><span class="sxs-lookup"><span data-stu-id="1068c-137">Requests Disconnected</span></span> |<span data-ttu-id="1068c-138">Čítače výkonu pro technologii ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1068c-138">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="1068c-139">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1068c-139">ASP.NET</span></span> |<span data-ttu-id="1068c-140">Restartování pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="1068c-140">Worker Process Restarts</span></span> |<span data-ttu-id="1068c-141">Čítače výkonu pro technologii ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1068c-141">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="1068c-142">Aplikace ASP.NET (**celkový**)</span><span class="sxs-lookup"><span data-stu-id="1068c-142">ASP.NET Applications(**Total**)</span></span> |<span data-ttu-id="1068c-143">Celkový počet požadavků</span><span class="sxs-lookup"><span data-stu-id="1068c-143">Requests Total</span></span> |<span data-ttu-id="1068c-144">Čítače výkonu pro technologii ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1068c-144">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="1068c-145">Aplikace ASP.NET (**celkový**)</span><span class="sxs-lookup"><span data-stu-id="1068c-145">ASP.NET Applications(**Total**)</span></span> |<span data-ttu-id="1068c-146">Počet požadavků za sekundu</span><span class="sxs-lookup"><span data-stu-id="1068c-146">Requests/Sec</span></span> |<span data-ttu-id="1068c-147">Čítače výkonu pro technologii ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1068c-147">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="1068c-148">Položku ASP.NET v4.0.30319</span><span class="sxs-lookup"><span data-stu-id="1068c-148">ASP.NET v4.0.30319</span></span> |<span data-ttu-id="1068c-149">Čas provádění požadavku</span><span class="sxs-lookup"><span data-stu-id="1068c-149">Request Execution Time</span></span> |<span data-ttu-id="1068c-150">Čítače výkonu pro technologii ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1068c-150">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="1068c-151">Položku ASP.NET v4.0.30319</span><span class="sxs-lookup"><span data-stu-id="1068c-151">ASP.NET v4.0.30319</span></span> |<span data-ttu-id="1068c-152">Čekací doba požadavku</span><span class="sxs-lookup"><span data-stu-id="1068c-152">Request Wait Time</span></span> |<span data-ttu-id="1068c-153">Čítače výkonu pro technologii ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1068c-153">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="1068c-154">Položku ASP.NET v4.0.30319</span><span class="sxs-lookup"><span data-stu-id="1068c-154">ASP.NET v4.0.30319</span></span> |<span data-ttu-id="1068c-155">Aktuální požadavky</span><span class="sxs-lookup"><span data-stu-id="1068c-155">Requests Current</span></span> |<span data-ttu-id="1068c-156">Čítače výkonu pro technologii ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1068c-156">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="1068c-157">Položku ASP.NET v4.0.30319</span><span class="sxs-lookup"><span data-stu-id="1068c-157">ASP.NET v4.0.30319</span></span> |<span data-ttu-id="1068c-158">Požadavky ve frontě</span><span class="sxs-lookup"><span data-stu-id="1068c-158">Requests Queued</span></span> |<span data-ttu-id="1068c-159">Čítače výkonu pro technologii ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1068c-159">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="1068c-160">Položku ASP.NET v4.0.30319</span><span class="sxs-lookup"><span data-stu-id="1068c-160">ASP.NET v4.0.30319</span></span> |<span data-ttu-id="1068c-161">Zamítnuté požadavky</span><span class="sxs-lookup"><span data-stu-id="1068c-161">Requests Rejected</span></span> |<span data-ttu-id="1068c-162">Čítače výkonu pro technologii ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1068c-162">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="1068c-163">Memory (Paměť)</span><span class="sxs-lookup"><span data-stu-id="1068c-163">Memory</span></span> |<span data-ttu-id="1068c-164">Počet MB k dispozici</span><span class="sxs-lookup"><span data-stu-id="1068c-164">Available MBytes</span></span> |<span data-ttu-id="1068c-165">Čítače výkonu paměti</span><span class="sxs-lookup"><span data-stu-id="1068c-165">Memory Performance Counters</span></span> |
| <span data-ttu-id="1068c-166">Memory (Paměť)</span><span class="sxs-lookup"><span data-stu-id="1068c-166">Memory</span></span> |<span data-ttu-id="1068c-167">Svěřené bajty</span><span class="sxs-lookup"><span data-stu-id="1068c-167">Committed Bytes</span></span> |<span data-ttu-id="1068c-168">Čítače výkonu paměti</span><span class="sxs-lookup"><span data-stu-id="1068c-168">Memory Performance Counters</span></span> |
| <span data-ttu-id="1068c-169">Procesor(_celkem)</span><span class="sxs-lookup"><span data-stu-id="1068c-169">Processor(_Total)</span></span> |<span data-ttu-id="1068c-170">% Času procesoru</span><span class="sxs-lookup"><span data-stu-id="1068c-170">% Processor Time</span></span> |<span data-ttu-id="1068c-171">Čítače výkonu pro technologii ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1068c-171">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="1068c-172">TCPv4</span><span class="sxs-lookup"><span data-stu-id="1068c-172">TCPv4</span></span> |<span data-ttu-id="1068c-173">Počet selhání připojení</span><span class="sxs-lookup"><span data-stu-id="1068c-173">Connection Failures</span></span> |<span data-ttu-id="1068c-174">Objekt TCP</span><span class="sxs-lookup"><span data-stu-id="1068c-174">TCP Object</span></span> |
| <span data-ttu-id="1068c-175">TCPv4</span><span class="sxs-lookup"><span data-stu-id="1068c-175">TCPv4</span></span> |<span data-ttu-id="1068c-176">Vytvořeno připojení</span><span class="sxs-lookup"><span data-stu-id="1068c-176">Connections Established</span></span> |<span data-ttu-id="1068c-177">Objekt TCP</span><span class="sxs-lookup"><span data-stu-id="1068c-177">TCP Object</span></span> |
| <span data-ttu-id="1068c-178">TCPv4</span><span class="sxs-lookup"><span data-stu-id="1068c-178">TCPv4</span></span> |<span data-ttu-id="1068c-179">Resetování připojení</span><span class="sxs-lookup"><span data-stu-id="1068c-179">Connections Reset</span></span> |<span data-ttu-id="1068c-180">Objekt TCP</span><span class="sxs-lookup"><span data-stu-id="1068c-180">TCP Object</span></span> |
| <span data-ttu-id="1068c-181">TCPv4</span><span class="sxs-lookup"><span data-stu-id="1068c-181">TCPv4</span></span> |<span data-ttu-id="1068c-182">Segmenty odeslaných za sekundu</span><span class="sxs-lookup"><span data-stu-id="1068c-182">Segments Sent/sec</span></span> |<span data-ttu-id="1068c-183">Objekt TCP</span><span class="sxs-lookup"><span data-stu-id="1068c-183">TCP Object</span></span> |
| <span data-ttu-id="1068c-184">Interface(*) sítě</span><span class="sxs-lookup"><span data-stu-id="1068c-184">Network Interface(*)</span></span> |<span data-ttu-id="1068c-185">Přijaté bajty/s</span><span class="sxs-lookup"><span data-stu-id="1068c-185">Bytes Received/sec</span></span> |<span data-ttu-id="1068c-186">Objekt rozhraní sítě</span><span class="sxs-lookup"><span data-stu-id="1068c-186">Network Interface Object</span></span> |
| <span data-ttu-id="1068c-187">Interface(*) sítě</span><span class="sxs-lookup"><span data-stu-id="1068c-187">Network Interface(*)</span></span> |<span data-ttu-id="1068c-188">Odeslané bajty/s</span><span class="sxs-lookup"><span data-stu-id="1068c-188">Bytes Sent/sec</span></span> |<span data-ttu-id="1068c-189">Objekt rozhraní sítě</span><span class="sxs-lookup"><span data-stu-id="1068c-189">Network Interface Object</span></span> |
| <span data-ttu-id="1068c-190">Síťové rozhraní (Microsoft sběrnice virtuálního počítače síťový adaptér _2)</span><span class="sxs-lookup"><span data-stu-id="1068c-190">Network Interface(Microsoft Virtual Machine Bus Network Adapter _2)</span></span> |<span data-ttu-id="1068c-191">Přijaté bajty/s</span><span class="sxs-lookup"><span data-stu-id="1068c-191">Bytes Received/sec</span></span> |<span data-ttu-id="1068c-192">Objekt rozhraní sítě</span><span class="sxs-lookup"><span data-stu-id="1068c-192">Network Interface Object</span></span> |
| <span data-ttu-id="1068c-193">Síťové rozhraní (Microsoft sběrnice virtuálního počítače síťový adaptér _2)</span><span class="sxs-lookup"><span data-stu-id="1068c-193">Network Interface(Microsoft Virtual Machine Bus Network Adapter _2)</span></span> |<span data-ttu-id="1068c-194">Odeslané bajty/s</span><span class="sxs-lookup"><span data-stu-id="1068c-194">Bytes Sent/sec</span></span> |<span data-ttu-id="1068c-195">Objekt rozhraní sítě</span><span class="sxs-lookup"><span data-stu-id="1068c-195">Network Interface Object</span></span> |
| <span data-ttu-id="1068c-196">Síťové rozhraní (Microsoft sběrnice virtuálního počítače síťový adaptér _2)</span><span class="sxs-lookup"><span data-stu-id="1068c-196">Network Interface(Microsoft Virtual Machine Bus Network Adapter _2)</span></span> |<span data-ttu-id="1068c-197">Čítač Bajty celkem/s</span><span class="sxs-lookup"><span data-stu-id="1068c-197">Bytes Total/sec</span></span> |<span data-ttu-id="1068c-198">Objekt rozhraní sítě</span><span class="sxs-lookup"><span data-stu-id="1068c-198">Network Interface Object</span></span> |

## <a name="create-and-add-custom-performance-counters-tooyour-application"></a><span data-ttu-id="1068c-199">Vytvořit a přidat vlastní výkonu čítače tooyour aplikace</span><span class="sxs-lookup"><span data-stu-id="1068c-199">Create and add custom performance counters tooyour application</span></span>
<span data-ttu-id="1068c-200">Azure má toocreate podporu a upravte vlastní čítače výkonu pro webových rolí a rolí pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="1068c-200">Azure has support toocreate and modify custom performance counters for web roles and worker roles.</span></span> <span data-ttu-id="1068c-201">Hello čítače může být použité tootrack a monitorování chování specifické pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1068c-201">hello counters may be used tootrack and monitor application-specific behavior.</span></span> <span data-ttu-id="1068c-202">Můžete vytvořit a kategorie čítače výkonu vlastní a specifikátory odstranit z úloha spuštění, webovou roli nebo role pracovního procesu se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="1068c-202">You can create and delete custom performance counter categories and specifiers from a startup task, web role, or worker role with elevated permissions.</span></span>

> [!NOTE]
> <span data-ttu-id="1068c-203">Kód, který provede změny čítače výkonu toocustom musí mít zvýšená oprávnění toorun.</span><span class="sxs-lookup"><span data-stu-id="1068c-203">Code that makes changes toocustom performance counters must have elevated permissions toorun.</span></span> <span data-ttu-id="1068c-204">Pokud je kód hello v webovou roli nebo role pracovního procesu, hello role musí obsahovat značky hello <Runtime executionContext="elevated" /> v hello ServiceDefinition.csdef souboru pro hello role tooinitialize správně.</span><span class="sxs-lookup"><span data-stu-id="1068c-204">If hello code is in a web role or worker role, hello role must include hello tag <Runtime executionContext="elevated" /> in hello ServiceDefinition.csdef file for hello role tooinitialize properly.</span></span>
>
>

<span data-ttu-id="1068c-205">Můžete odeslat vlastní výkon čítač uložení dat v tooAzure hello Diagnostika agenta.</span><span class="sxs-lookup"><span data-stu-id="1068c-205">You can send custom performance counter data tooAzure storage using hello diagnostics agent.</span></span>

<span data-ttu-id="1068c-206">data čítače Hello standardní výkonu je generován hello, který zpracovává Azure.</span><span class="sxs-lookup"><span data-stu-id="1068c-206">hello standard performance counter data is generated by hello Azure processes.</span></span> <span data-ttu-id="1068c-207">Data čítače výkonu vlastní musí být vytvořeny webovou aplikací role role nebo pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="1068c-207">Custom performance counter data must be created by your web role or worker role application.</span></span> <span data-ttu-id="1068c-208">V tématu [typy čítačů výkonu](https://msdn.microsoft.com/library/z573042h.aspx) informace o typech hello dat, která mohou být uloženy ve vlastní čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="1068c-208">See [Performance Counter Types](https://msdn.microsoft.com/library/z573042h.aspx) for information on hello types of data that can be stored in custom performance counters.</span></span> <span data-ttu-id="1068c-209">V tématu [Ukázka čítače výkonu](http://code.msdn.microsoft.com/azure/) pro příklad, který vytvoří a nastaví data čítače výkonu vlastní ve webové roli.</span><span class="sxs-lookup"><span data-stu-id="1068c-209">See [PerformanceCounters Sample](http://code.msdn.microsoft.com/azure/) for an example that creates and sets custom performance counter data in a web role.</span></span>

## <a name="store-and-view-performance-counter-data"></a><span data-ttu-id="1068c-210">Úložiště a zobrazení data čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="1068c-210">Store and view performance counter data</span></span>
<span data-ttu-id="1068c-211">Azure data čítače výkonu pomocí jiné diagnostické informace ukládá do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1068c-211">Azure caches performance counter data with other diagnostic information.</span></span> <span data-ttu-id="1068c-212">Tato data jsou k dispozici pro vzdálené monitorování hello role instance je spuštěn pomocí nástroje tooview přístup ke vzdálené ploše, jako je monitorování výkonu.</span><span class="sxs-lookup"><span data-stu-id="1068c-212">This data is available for remote monitoring while hello role instance is running using remote desktop access tooview tools such as Performance Monitor.</span></span> <span data-ttu-id="1068c-213">toopersist hello data mimo hello instanci role, hello Diagnostika agenta musíte přenést hello data tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="1068c-213">toopersist hello data outside of hello role instance, hello diagnostics agent must transfer hello data tooAzure storage.</span></span> <span data-ttu-id="1068c-214">limit velikosti Hello data čítače výkonu hello do mezipaměti se dá nakonfigurovat v hello Diagnostika agenta, nebo může být nakonfigurován toobe součástí sdílené limit pro všechny hello diagnostická data.</span><span class="sxs-lookup"><span data-stu-id="1068c-214">hello size limit of hello cached performance counter data can be configured in hello diagnostics agent, or it may be configured toobe part of a shared limit for all hello diagnostic data.</span></span> <span data-ttu-id="1068c-215">Další informace o nastavení hello velikost vyrovnávací paměti najdete v tématu [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) a [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx).</span><span class="sxs-lookup"><span data-stu-id="1068c-215">For more information about setting hello buffer size, see [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) and [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx).</span></span> <span data-ttu-id="1068c-216">V tématu [úložiště a zobrazení diagnostických dat ve službě Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) přehled nastavení hello agenta tootransfer data tooa účet úložiště pro diagnostiku.</span><span class="sxs-lookup"><span data-stu-id="1068c-216">See [Store and View Diagnostic Data in Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) for an overview of setting up hello diagnostics agent tootransfer data tooa storage account.</span></span>

<span data-ttu-id="1068c-217">Každá instance čítače výkonu nakonfigurované se zaznamenává v zadané vzorkovací frekvenci a hello Vzorkovat data se přenáší toohello účet úložiště tak, že žádost o přenos plánované nebo žádost o přenos na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="1068c-217">Each configured performance counter instance is recorded at a specified sampling rate, and hello sampled data is transferred toohello storage account either by a scheduled transfer request or an on-demand transfer request.</span></span> <span data-ttu-id="1068c-218">Automatické přenosy může být naplánovaná tak často, jak jednou za minutu.</span><span class="sxs-lookup"><span data-stu-id="1068c-218">Automatic transfers may be scheduled as often as once per minute.</span></span> <span data-ttu-id="1068c-219">Data čítače výkonu přenesených hello Diagnostika agenta je uložena v tabulce, WADPerformanceCountersTable, v účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="1068c-219">Performance counter data transferred by hello diagnostics agent is stored in a table, WADPerformanceCountersTable, in hello storage account.</span></span> <span data-ttu-id="1068c-220">Tato tabulka může získat přístup a dotaz pomocí metod úložiště Azure úrovně standard rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1068c-220">This table may be accessed and queried with standard Azure storage API methods.</span></span> <span data-ttu-id="1068c-221">V tématu [Ukázka čítače výkonu Microsoft Azure](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) příklad dotazování a zobrazení data čítače výkonu z tabulky WADPerformanceCountersTable hello.</span><span class="sxs-lookup"><span data-stu-id="1068c-221">See [Microsoft Azure PerformanceCounters Sample](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) for an example of querying and displaying performance counter data from hello WADPerformanceCountersTable table.</span></span>

> [!NOTE]
> <span data-ttu-id="1068c-222">V závislosti na hello Diagnostika agenta přenosu četnost a fronty latence hello nejnovější data čítače výkonu v účtu úložiště hello může být zastaralý několik minut.</span><span class="sxs-lookup"><span data-stu-id="1068c-222">Depending on hello diagnostics agent transfer frequency and queue latency, hello most recent performance counter data in hello storage account may be several minutes out of date.</span></span>
>
>

## <a name="enable-performance-counters-using-diagnostics-configuration-file"></a><span data-ttu-id="1068c-223">Povolit čítače výkonu pomocí diagnostiky konfiguračního souboru</span><span class="sxs-lookup"><span data-stu-id="1068c-223">Enable performance counters using diagnostics configuration file</span></span>
<span data-ttu-id="1068c-224">Použijte následující postup tooenable čítače výkonu v aplikaci Azure hello.</span><span class="sxs-lookup"><span data-stu-id="1068c-224">Use hello following procedure tooenable performance counters in your Azure application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1068c-225">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1068c-225">Prerequisites</span></span>
<span data-ttu-id="1068c-226">Této části se předpokládá, že máte importovat monitorování diagnostiky hello do vaší aplikace a přidat hello diagnostiky konfigurační soubor tooyour řešení sady Visual Studio (diagnostics.wadcfg v SDK 2.4 a pod ním nebo diagnostics.wadcfgx v SDK 2.5 a vyšší).</span><span class="sxs-lookup"><span data-stu-id="1068c-226">This section assumes that you have imported hello Diagnostics monitor into your application and added hello diagnostics configuration file tooyour Visual Studio solution (diagnostics.wadcfg in SDK 2.4 and below or diagnostics.wadcfgx in SDK 2.5 and above).</span></span> <span data-ttu-id="1068c-227">Informace o kroky 1 a 2 v [povolení diagnostiky Azure Cloud Services a virtuálních počítačů](cloud-services-dotnet-diagnostics.md)) Další informace.</span><span class="sxs-lookup"><span data-stu-id="1068c-227">See steps 1 and 2 in [Enabling Diagnostics in Azure Cloud Services and Virtual Machines](cloud-services-dotnet-diagnostics.md)) for more information.</span></span>

## <a name="step-1-collect-and-store-data-from-performance-counters"></a><span data-ttu-id="1068c-228">Krok 1: Shromažďovat a ukládat data z čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="1068c-228">Step 1: Collect and store data from performance counters</span></span>
<span data-ttu-id="1068c-229">Po přidání, že hello diagnostiky souboru tooyour řešení sady Visual Studio, můžete nakonfigurovat hello kolekce a ukládání data čítače výkonu v aplikaci Azure.</span><span class="sxs-lookup"><span data-stu-id="1068c-229">After you have added hello diagnostics file tooyour Visual Studio solution, you can configure hello collection and storage of performance counter data in an Azure application.</span></span> <span data-ttu-id="1068c-230">To se provádí přidáním soubor diagnostiky toohello čítačů výkonu.</span><span class="sxs-lookup"><span data-stu-id="1068c-230">This is done by adding performance counters toohello diagnostics file.</span></span> <span data-ttu-id="1068c-231">Diagnostická data, včetně čítačů výkonu, je nejprve shromažďují na instanci hello.</span><span class="sxs-lookup"><span data-stu-id="1068c-231">Diagnostics data, including performance counters, is first collected on hello instance.</span></span> <span data-ttu-id="1068c-232">Hello data je pak trvalou toohello WADPerformanceCountersTable tabulky v hello služby Azure Table, tak budete také potřebovat toospecify hello účet úložiště ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1068c-232">hello data is then persisted toohello WADPerformanceCountersTable table in hello Azure Table service, so you will also need toospecify hello storage account in your application.</span></span> <span data-ttu-id="1068c-233">Pokud testujete aplikace místně v hello výpočetní emulátor, můžete také ukládat diagnostická data místně v hello emulátoru úložiště.</span><span class="sxs-lookup"><span data-stu-id="1068c-233">If you're testing your application locally in hello Compute Emulator, you can also store diagnostics data locally in hello Storage Emulator.</span></span> <span data-ttu-id="1068c-234">Než bude ukládat data diagnostiky, je nutné nejprve přejít toohello [portál Azure](http://portal.azure.com/) a vytvoření účtu úložiště classic.</span><span class="sxs-lookup"><span data-stu-id="1068c-234">Before you store diagnostics data, you must first go toohello [Azure portal](http://portal.azure.com/) and create a classic storage account.</span></span> <span data-ttu-id="1068c-235">Osvědčeným postupem je toolocate svůj účet úložiště hello stejného geografického umístění jako vaše aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="1068c-235">A best practice is toolocate your storage account in hello same geo-location as your Azure application.</span></span> <span data-ttu-id="1068c-236">Podle zachování hello Azure aplikace a účet úložiště jsou v Dobrý den stejného geografického umístění, vyhněte se platícího náklady na externí šířku pásma a snižování latence.</span><span class="sxs-lookup"><span data-stu-id="1068c-236">By keeping hello Azure application and storage account are in hello same geo-location, you avoid paying external bandwidth costs and reduce latency.</span></span>

### <a name="add-performance-counters-toohello-diagnostics-file"></a><span data-ttu-id="1068c-237">Přidejte soubor diagnostiky toohello čítačů výkonu</span><span class="sxs-lookup"><span data-stu-id="1068c-237">Add performance counters toohello diagnostics file</span></span>
<span data-ttu-id="1068c-238">Existuje mnoho čítačů, které můžete použít.</span><span class="sxs-lookup"><span data-stu-id="1068c-238">There are many counters you can use.</span></span> <span data-ttu-id="1068c-239">Hello následující příklad ukazuje několik čítače výkonu, které se doporučují pro webové a pracovní role monitorování.</span><span class="sxs-lookup"><span data-stu-id="1068c-239">hello following example shows several performance counters that are recommended for web and worker role monitoring.</span></span>

<span data-ttu-id="1068c-240">Otevřete soubor diagnostiky hello (diagnostics.wadcfg SDK 2.4 a nižší nebo diagnostics.wadcfgx v SDK 2.5 a vyšší) a přidejte následující toohello DiagnosticMonitorConfiguration element hello:</span><span class="sxs-lookup"><span data-stu-id="1068c-240">Open hello diagnostics file (diagnostics.wadcfg in SDK 2.4 and below or diagnostics.wadcfgx in SDK 2.5 and above) and add hello following toohello DiagnosticMonitorConfiguration element:</span></span>

```xml
<PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
    <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT30S" />

    <!-- Use hello Process(w3wp) category counters in a web role -->

    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\% Processor Time" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Private Bytes" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Thread Count" sampleRate="PT30S" />

    <!-- Use hello Process(WaWorkerHost) category counters in a worker role.
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\% Processor Time" sampleRate="PT30S" />
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Private Bytes" sampleRate="PT30S" />
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Thread Count" sampleRate="PT30S" />
    -->

    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Interop(_Global_)\# of marshalling" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Loading(_Global_)\% Time Loading" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR LocksAndThreads(_Global_)\Contention Rate / sec" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Memory(_Global_)\# Bytes in all Heaps" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Networking(_Global_)\Connections Established" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Remoting(_Global_)\Remote Calls/sec" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Jit(_Global_)\% Time in Jit" sampleRate="PT30S" />
</PerformanceCounters>
```

<span data-ttu-id="1068c-241">Hello bufferQuotaInMB atribut, který určuje maximální množství hello úložiště systému souboru, který je k dispozici pro typ kolekce dat hello (Azure protokoly, protokoly služby IIS, atd.).</span><span class="sxs-lookup"><span data-stu-id="1068c-241">hello bufferQuotaInMB attribute, which specifies hello maximum amount of file system storage that is available for hello data collection type (Azure logs, IIS logs, etc.).</span></span> <span data-ttu-id="1068c-242">Hello výchozí hodnota je 0.</span><span class="sxs-lookup"><span data-stu-id="1068c-242">hello default is 0.</span></span> <span data-ttu-id="1068c-243">Když je dosaženo podílu hello, nejstarší data hello je odstranit, protože se přidá nová data.</span><span class="sxs-lookup"><span data-stu-id="1068c-243">When hello quota is reached, hello oldest data is deleted as new data is added.</span></span> <span data-ttu-id="1068c-244">Hello součet všech vlastností bufferQuotaInMB hello musí být větší než hodnota hello hello OverallQuotaInMB atributu.</span><span class="sxs-lookup"><span data-stu-id="1068c-244">hello sum of all hello bufferQuotaInMB properties must be greater than hello value of hello OverallQuotaInMB attribute.</span></span> <span data-ttu-id="1068c-245">Další podrobné informace pro určení toho, jak velké úložiště se bude vyžadovat hello sběr dat diagnostiky, naleznete v hello část instalace WAD [řešení potíží s osvědčené postupy pro vývoj aplikací Azure](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx).</span><span class="sxs-lookup"><span data-stu-id="1068c-245">For a more detailed discussion of determining how much storage will be required for hello collection of diagnostics data, see hello Setup WAD section of [Troubleshooting Best Practices for Developing Azure Applications](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx).</span></span>

<span data-ttu-id="1068c-246">Hello scheduledTransferPeriod atribut, který určuje hello interval mezi naplánované přenosů dat, zaokrouhlit toohello nejbližší minutu.</span><span class="sxs-lookup"><span data-stu-id="1068c-246">hello scheduledTransferPeriod attribute, which specifies hello interval between scheduled transfers of data, rounded up toohello nearest minute.</span></span> <span data-ttu-id="1068c-247">V následující příklady hello nastavení tooPT30M (30 minut).</span><span class="sxs-lookup"><span data-stu-id="1068c-247">In hello following examples, it is set tooPT30M (30 minutes).</span></span> <span data-ttu-id="1068c-248">Hello přenos období tooa malá hodnota nastavení, jako je 1 minuta, bude nepříznivě ovlivnit výkon vaší aplikace v produkčním prostředí ale může být užitečná pro zobrazení diagnostiky rychle práce při testování.</span><span class="sxs-lookup"><span data-stu-id="1068c-248">Setting hello transfer period tooa small value, such as 1 minute, will adversely impact your application's performance in production but can be useful for seeing diagnostics working quickly when you are testing.</span></span> <span data-ttu-id="1068c-249">Hello naplánované přenos perioda musí být dostatečně malé, tooensure, diagnostická data nejsou přepsána na hello instanci, ale dostatečně velké na to, že nebude mít vliv hello výkon aplikace.</span><span class="sxs-lookup"><span data-stu-id="1068c-249">hello scheduled transfer period should be small enough tooensure that diagnostic data is not overwritten on hello instance, but large enough that it will not impact hello performance of your application.</span></span>

<span data-ttu-id="1068c-250">Určuje atribut counterSpecifier Hello toocollect čítače výkonu hello.</span><span class="sxs-lookup"><span data-stu-id="1068c-250">hello counterSpecifier attribute specifies hello performance counter toocollect.</span></span> <span data-ttu-id="1068c-251">Určuje atribut sampleRate Hello hello rychlost, jakou hello čítače výkonu se odeberou, v takovém případě 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="1068c-251">hello sampleRate attribute specifies hello rate at which hello performance counter should be sampled, in this case 30 seconds.</span></span>

<span data-ttu-id="1068c-252">Po přidání, že čítače výkonu hello, které chcete toocollect, uložte soubor změny toohello diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="1068c-252">Once you've added hello performance counters that you want toocollect, save your changes toohello diagnostics file.</span></span> <span data-ttu-id="1068c-253">Dále je nutné zadat účet toospecify hello úložiště, který bude formou hello diagnostická data.</span><span class="sxs-lookup"><span data-stu-id="1068c-253">Next, you need toospecify hello storage account that hello diagnostics data will be persisted to.</span></span>

### <a name="specify-hello-storage-account"></a><span data-ttu-id="1068c-254">Zadejte účet úložiště hello</span><span class="sxs-lookup"><span data-stu-id="1068c-254">Specify hello storage account</span></span>
<span data-ttu-id="1068c-255">toopersist účet vaší diagnostické informace tooyour Azure Storage, je nutné zadat připojovací řetězec v souboru konfigurace (souboru ServiceConfiguration.cscfg) služby.</span><span class="sxs-lookup"><span data-stu-id="1068c-255">toopersist your diagnostics information tooyour Azure Storage account, you must specify a connection string in your service configuration (ServiceConfiguration.cscfg) file.</span></span>

<span data-ttu-id="1068c-256">Pro Azure SDK 2.5 hello účet úložiště lze v souboru diagnostics.wadcfgx hello.</span><span class="sxs-lookup"><span data-stu-id="1068c-256">For Azure SDK 2.5, hello Storage Account can be specified in hello diagnostics.wadcfgx file.</span></span>

> [!NOTE]
> <span data-ttu-id="1068c-257">Tyto pokyny platí pouze tooAzure SDK 2.4 a níže.</span><span class="sxs-lookup"><span data-stu-id="1068c-257">These instructions only apply tooAzure SDK 2.4 and below.</span></span> <span data-ttu-id="1068c-258">Pro Azure SDK 2.5 hello účet úložiště lze v souboru diagnostics.wadcfgx hello.</span><span class="sxs-lookup"><span data-stu-id="1068c-258">For Azure SDK 2.5, hello Storage Account can be specified in hello diagnostics.wadcfgx file.</span></span>
>
>

<span data-ttu-id="1068c-259">tooset hello připojovací řetězce:</span><span class="sxs-lookup"><span data-stu-id="1068c-259">tooset hello connection strings:</span></span>

1. <span data-ttu-id="1068c-260">Otevřete soubor ServiceConfiguration.Cloud.cscfg hello pomocí oblíbeném textovém editoru a sadu hello připojovací řetězec pro úložiště.</span><span class="sxs-lookup"><span data-stu-id="1068c-260">Open hello ServiceConfiguration.Cloud.cscfg file using your favorite text editor and set hello connection string for your storage.</span></span> <span data-ttu-id="1068c-261">Hello *AccountName* a *AccountKey* hodnoty se nacházejí v hello portál Azure hello panelu účet úložiště, v části přístupové klíče.</span><span class="sxs-lookup"><span data-stu-id="1068c-261">hello *AccountName* and *AccountKey* values are found in hello Azure portal in hello storage account dashboard, under Access keys.</span></span>

    ```xml
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>"/>
    </ConfigurationSettings>
    ```
2. <span data-ttu-id="1068c-262">Uložte soubor ServiceConfiguration.Cloud.cscfg hello.</span><span class="sxs-lookup"><span data-stu-id="1068c-262">Save hello ServiceConfiguration.Cloud.cscfg file.</span></span>
3. <span data-ttu-id="1068c-263">Otevřete soubor ServiceConfiguration.Local.cscfg hello a ověřte, zda je UseDevelopmentStorage nastavena tootrue.</span><span class="sxs-lookup"><span data-stu-id="1068c-263">Open hello ServiceConfiguration.Local.cscfg file and verify that UseDevelopmentStorage is set tootrue.</span></span>

    ```xml
    <ConfigurationSettings>
      <Settingname="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true"/>
    </ConfigurationSettings>
    ```
   <span data-ttu-id="1068c-264">Teď, když jsou nastavené hello připojovací řetězce, aplikace se uchová účet úložiště tooyour diagnostiky dat při nasazení vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="1068c-264">Now that hello connection strings are set, your application will persist diagnostics data tooyour storage account when your application is deployed.</span></span>
4. <span data-ttu-id="1068c-265">Uložte a sestavení projektu a nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="1068c-265">Save and build your project, then deploy your application.</span></span>

## <a name="step-2-optional-create-custom-performance-counters"></a><span data-ttu-id="1068c-266">Krok 2: (Volitelné) vytvořit vlastní čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="1068c-266">Step 2: (Optional) Create custom performance counters</span></span>
<span data-ttu-id="1068c-267">Kromě toho toohello předem definované čítače výkonu, můžete přidat že vlastní vlastního výkonu čítače toomonitor role web nebo worker.</span><span class="sxs-lookup"><span data-stu-id="1068c-267">In addition toohello pre-defined performance counters, you can add your own custom performance counters toomonitor web or worker roles.</span></span> <span data-ttu-id="1068c-268">Vlastní čítače výkonu může být použité tootrack a monitorování chování specifické pro aplikaci a můžou vytvořit nebo odstranit v úloha spuštění, webovou roli nebo role pracovního procesu se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="1068c-268">Custom performance counters may be used tootrack and monitor application-specific behavior and can be created or deleted in a startup task, web role, or worker role with elevated permissions.</span></span>

<span data-ttu-id="1068c-269">agent Azure diagnostics Hello aktualizuje hello konfiguraci čítače výkonnosti ze souboru .wadcfg hello jednu minutu po spuštění.</span><span class="sxs-lookup"><span data-stu-id="1068c-269">hello Azure diagnostics agent refreshes hello performance counter configuration from hello .wadcfg file one minute after starting.</span></span>  <span data-ttu-id="1068c-270">Pokud vytvoříte vlastní čítače výkonu v hello metoda OnStart a úlohami spuštění trvá déle než minutu tooexecute, vaše vlastní čítače výkonu nebudou byl vytvořen při hello Azure Diagnostics agent se pokusí tooload je.</span><span class="sxs-lookup"><span data-stu-id="1068c-270">If you create custom performance counters in hello OnStart method and your startup tasks take longer than one minute tooexecute, your custom performance counters will not have been created when hello Azure Diagnostics agent tries tooload them.</span></span>  <span data-ttu-id="1068c-271">V tomto scénáři uvidíte, že Azure Diagnostics správně zaznamená všechny diagnostická data s výjimkou vaše vlastní čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="1068c-271">In this scenario, you will see that Azure Diagnostics correctly captures all diagnostics data except your custom performance counters.</span></span>  <span data-ttu-id="1068c-272">tooresolve tento problém, vytvořte hello čítače výkonu v úloze pro spuštění nebo přesunout, že některé vaše úloha spuštění metoda OnStart toohello fungovat po vytvoření hello čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="1068c-272">tooresolve this issue, create hello performance counters in a startup task or move some of your startup task work toohello OnStart method after creating hello performance counters.</span></span>

<span data-ttu-id="1068c-273">Proveďte následující kroky toocreate jednoduché vlastní výkon čítač s názvem "\MyCustomCounterCategory\MyButton1Counter" hello:</span><span class="sxs-lookup"><span data-stu-id="1068c-273">Perform hello following steps toocreate a simple custom performance counter named "\MyCustomCounterCategory\MyButton1Counter":</span></span>

1. <span data-ttu-id="1068c-274">Otevřete soubor definice služby (CSDEF) hello pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1068c-274">Open hello service definition file (CSDEF) for your application.</span></span>
2. <span data-ttu-id="1068c-275">Přidejte hello Runtime element toohello WebRole nebo WorkerRole element tooallow provádění se zvýšenými oprávněními:</span><span class="sxs-lookup"><span data-stu-id="1068c-275">Add hello Runtime element toohello WebRole or WorkerRole element tooallow execution with elevated privileges:</span></span>

    ```xml
    <runtime executioncontext="elevated"/>
    ```
3. <span data-ttu-id="1068c-276">Uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="1068c-276">Save hello file.</span></span>
4. <span data-ttu-id="1068c-277">Otevřete soubor diagnostiky hello (diagnostics.wadcfg SDK 2.4 a nižší nebo diagnostics.wadcfgx v SDK 2.5 a vyšší) a přidejte následující toohello DiagnosticMonitorConfiguration hello</span><span class="sxs-lookup"><span data-stu-id="1068c-277">Open hello diagnostics file (diagnostics.wadcfg in SDK 2.4 and below or diagnostics.wadcfgx in SDK 2.5 and above) and add hello following toohello DiagnosticMonitorConfiguration</span></span>

    ```xml
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
      <PerformanceCounterConfiguration counterSpecifier="\MyCustomCounterCategory\MyButton1Counter" sampleRate="PT30S"/>
    </PerformanceCounters>
    ```
5. <span data-ttu-id="1068c-278">Uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="1068c-278">Save hello file.</span></span>
6. <span data-ttu-id="1068c-279">Před vyvoláním základní vytvořte kategorie čítače výkonu vlastní hello metoda OnStart hello vaši roli. OnStart.</span><span class="sxs-lookup"><span data-stu-id="1068c-279">Create hello custom performance counter category in hello OnStart method of your role, before invoking base.OnStart.</span></span> <span data-ttu-id="1068c-280">Hello následující příklad jazyka C# vytvoří vlastní kategorii, pokud ještě neexistuje:</span><span class="sxs-lookup"><span data-stu-id="1068c-280">hello following C# example creates a custom category, if it does not already exist:</span></span>

    ```csharp
    public override bool OnStart()
    {
      if (!PerformanceCounterCategory.Exists("MyCustomCounterCategory"))
      {
         CounterCreationDataCollection counterCollection = new CounterCreationDataCollection();

         // add a counter tracking user button1 clicks
         CounterCreationData operationTotal1 = new CounterCreationData();
         operationTotal1.CounterName = "MyButton1Counter";
         operationTotal1.CounterHelp = "My Custom Counter for Button1";
         operationTotal1.CounterType = PerformanceCounterType.NumberOfItems32;
         counterCollection.Add(operationTotal1);

         PerformanceCounterCategory.Create(
           "MyCustomCounterCategory",
           "My Custom Counter Category",
           PerformanceCounterCategoryType.SingleInstance, counterCollection);

         Trace.WriteLine("Custom counter category created.");
      }
      else {
        Trace.WriteLine("Custom counter category already exists.");
      }

    return base.OnStart();
    }
    ```
7. <span data-ttu-id="1068c-281">Aktualizujte hello čítače v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="1068c-281">Update hello counters within your application.</span></span> <span data-ttu-id="1068c-282">Následující ukázka Hello aktualizací čítače výkonu vlastní Button1_Click událostí:</span><span class="sxs-lookup"><span data-stu-id="1068c-282">hello following example updates a custom performance counter on Button1_Click events:</span></span>

    ```csharp
    protected void Button1_Click(object sender, EventArgs e)
    {
      PerformanceCounter button1Counter = new PerformanceCounter(
        "MyCustomCounterCategory",
        "MyButton1Counter",
        string.Empty,
        false);
      button1Counter.Increment();
      this.Button1.Text = "Button 1 count: " +
        button1Counter.RawValue.ToString();
    }
    ```
8. <span data-ttu-id="1068c-283">Uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="1068c-283">Save hello file.</span></span>  

<span data-ttu-id="1068c-284">Data čítače výkonu vlastní budou shromažďována teď hello Azure diagnostics monitorování.</span><span class="sxs-lookup"><span data-stu-id="1068c-284">Custom performance counter data will now be collected by hello Azure diagnostics monitor.</span></span>

## <a name="step-3-query-performance-counter-data"></a><span data-ttu-id="1068c-285">Krok 3: Dotaz na data čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="1068c-285">Step 3: Query performance counter data</span></span>
<span data-ttu-id="1068c-286">Jakmile se vaše aplikace je nasazená a spuštěna, bude zahájeno monitorování diagnostiky hello shromažďování čítačů výkonu a uložením tohoto úložiště tooAzure data.</span><span class="sxs-lookup"><span data-stu-id="1068c-286">Once your application is deployed and running, hello Diagnostics monitor will begin collecting performance counters and persisting that data tooAzure storage.</span></span> <span data-ttu-id="1068c-287">Pomocí nástrojů, jako je Průzkumníka serveru v sadě Visual Studio, [Azure Storage Explorer](http://azurestorageexplorer.codeplex.com/), nebo [Azure Diagnostics Manager](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) podle Cerebrata čítače výkonu hello tooview data v hello Tabulka WADPerformanceCountersTable.</span><span class="sxs-lookup"><span data-stu-id="1068c-287">You use tools such as Server Explorer in Visual Studio,  [Azure Storage Explorer](http://azurestorageexplorer.codeplex.com/), or [Azure Diagnostics Manager](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) by Cerebrata tooview hello performance counters data in hello WADPerformanceCountersTable table.</span></span> <span data-ttu-id="1068c-288">Můžete také prostřednictvím kódu programu dotazovat pomocí služby Table hello [C#](../cosmos-db/table-storage-how-to-use-dotnet.md), [Java](../cosmos-db/table-storage-how-to-use-java.md), [Node.js](../cosmos-db/table-storage-how-to-use-nodejs.md), [Python](../cosmos-db/table-storage-how-to-use-python.md), [Ruby](../cosmos-db/table-storage-how-to-use-ruby.md), nebo [PHP](../cosmos-db/table-storage-how-to-use-php.md).</span><span class="sxs-lookup"><span data-stu-id="1068c-288">You can also programmatically query hello Table service using [C#](../cosmos-db/table-storage-how-to-use-dotnet.md),  [Java](../cosmos-db/table-storage-how-to-use-java.md),  [Node.js](../cosmos-db/table-storage-how-to-use-nodejs.md), [Python](../cosmos-db/table-storage-how-to-use-python.md), [Ruby](../cosmos-db/table-storage-how-to-use-ruby.md), or [PHP](../cosmos-db/table-storage-how-to-use-php.md).</span></span>

<span data-ttu-id="1068c-289">Hello následující C# příklad ukazuje základní dotaz hello WADPerformanceCountersTable tabulky a uloží soubor CSV tooa data diagnostiky hello.</span><span class="sxs-lookup"><span data-stu-id="1068c-289">hello following C# example shows a basic query against hello WADPerformanceCountersTable table and saves hello diagnostics data tooa CSV file.</span></span> <span data-ttu-id="1068c-290">Po uložení souboru CSV tooa hello čítače výkonu můžete hello vytváření grafů možnosti v aplikaci Microsoft Excel nebo některé další data hello nástroj toovisualize.</span><span class="sxs-lookup"><span data-stu-id="1068c-290">Once hello performance counters are saved tooa CSV file, you can use hello graphing capabilities in Microsoft Excel or some other tool toovisualize hello data.</span></span> <span data-ttu-id="1068c-291">Být jisti tooadd tooMicrosoft.WindowsAzure.Storage.dll odkaz, který je součástí hello Azure SDK pro .NET října 2012 a novější.</span><span class="sxs-lookup"><span data-stu-id="1068c-291">Be sure tooadd a reference tooMicrosoft.WindowsAzure.Storage.dll, which is included in hello Azure SDK for .NET October 2012 and later.</span></span> <span data-ttu-id="1068c-292">sestavení Hello je nainstalovaná toohello % Program Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version-num\ref\ adresář.</span><span class="sxs-lookup"><span data-stu-id="1068c-292">hello assembly is installed toohello %Program Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version-num\ref\ directory.</span></span>

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Table;
...

// Get hello connection string. When using Microsoft Azure Cloud Services, it is recommended
// you store your connection string using hello Microsoft Azure service configuration
// system (*.csdef and *.cscfg files). You can you use hello CloudConfigurationManager type
// tooretrieve your storage connection string.  If you're not using Cloud Services, it's
// recommended that you store hello connection string in your web.config or app.config file.
// Use hello ConfigurationManager type tooretrieve your storage connection string.

string connectionString = Microsoft.WindowsAzure.CloudConfigurationManager.GetSetting("StorageConnectionString");
//string connectionString = System.Configuration.ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString;

// Get a reference toohello storage account using hello connection string.  You can also use hello development
// storage account (Storage Emulator) for local debugging.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
//CloudStorageAccount storageAccount = CloudStorageAccount.DevelopmentStorageAccount;

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "WADPerformanceCountersTable" table.
CloudTable table = tableClient.GetTableReference("WADPerformanceCountersTable");

// Create hello table query, filter on a specific CounterName, DeploymentId and RoleInstance.
TableQuery<PerformanceCountersEntity> query = new TableQuery<PerformanceCountersEntity>()
  .Where(
    TableQuery.CombineFilters(
      TableQuery.GenerateFilterCondition("CounterName", QueryComparisons.Equal, @"\Processor(_Total)\% Processor Time"),
      TableOperators.And,
      TableQuery.CombineFilters(
      TableQuery.GenerateFilterCondition("DeploymentId", QueryComparisons.Equal, "ec26b7a1720447e1bcdeefc41c4892a3"),
      TableOperators.And,
      TableQuery.GenerateFilterCondition("RoleInstance", QueryComparisons.Equal, "WebRole1_IN_0")
    )
  )
);

// Execute hello table query.
IEnumerable<PerformanceCountersEntity> result = table.ExecuteQuery(query);

// Process hello query results and build a CSV file.
StringBuilder sb = new StringBuilder("TimeStamp,EventTickCount,DeploymentId,Role,RoleInstance,CounterName,CounterValue\n");

foreach (PerformanceCountersEntity entity in result)
{
  sb.Append(entity.Timestamp + "," + entity.EventTickCount + "," + entity.DeploymentId + ","
    + entity.Role + "," + entity.RoleInstance + "," + entity.CounterName + "," + entity.CounterValue+"\n");
}

StreamWriter sw = File.CreateText(@"C:\temp\PerfCounters.csv");
sw.Write(sb.ToString());
sw.Close();
```

<span data-ttu-id="1068c-293">Entity se mapují tooC # objektů pomocí vlastní třídy odvozené od **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="1068c-293">Entities map tooC# objects using a custom class derived from **TableEntity**.</span></span> <span data-ttu-id="1068c-294">Hello následující kód definuje třídu entity představující čítače výkonu ve hello **WADPerformanceCountersTable** tabulky.</span><span class="sxs-lookup"><span data-stu-id="1068c-294">hello following code defines an entity class that represents a performance counter in hello **WADPerformanceCountersTable** table.</span></span>

```csharp
public class PerformanceCountersEntity : TableEntity
{
  public long EventTickCount { get; set; }
  public string DeploymentId { get; set; }
  public string Role { get; set; }
  public string RoleInstance { get; set; }
  public string CounterName { get; set; }
  public double CounterValue { get; set; }
}
```

## <a name="next-steps"></a><span data-ttu-id="1068c-295">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1068c-295">Next Steps</span></span>
[<span data-ttu-id="1068c-296">Zobrazit další články v Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="1068c-296">View additional articles on Azure Diagnostics</span></span>](../azure-diagnostics.md)
