---
title: "Použití čítačů výkonu v konzoli Azure Diagnostics | Microsoft Docs"
description: "Čítače výkonu v Azure cloudové služby nebo virtuálního počítače používejte k vyhledání kritická místa a optimalizaci výkonu."
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
ms.openlocfilehash: 2cf765cb034725199127c547a9b8b997a4a6089c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-use-performance-counters-in-an-azure-application"></a><span data-ttu-id="65cff-103">Vytváření a používání čítače výkonu v aplikaci Azure</span><span class="sxs-lookup"><span data-stu-id="65cff-103">Create and use performance counters in an Azure application</span></span>
<span data-ttu-id="65cff-104">Tento článek popisuje výhody a jak převést čítače výkonu do aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="65cff-104">This article describes the benefits of and how to put performance counters into your Azure application.</span></span> <span data-ttu-id="65cff-105">Můžete je používat pro shromažďování dat, najít kritická místa a vyladit výkon systému a aplikací.</span><span class="sxs-lookup"><span data-stu-id="65cff-105">You can use them to collect data, find bottlenecks, and tune system and application performance.</span></span>

<span data-ttu-id="65cff-106">Čítače výkonu, které jsou k dispozici pro Windows Server, služba IIS a ASP.NET můžete také shromažďovat a používá k určení stavu Azure webové role, role pracovního procesu a virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="65cff-106">Performance counters available for Windows Server, IIS, and ASP.NET can also be collected and used to determine the health of your Azure web roles, worker roles, and Virtual Machines.</span></span> <span data-ttu-id="65cff-107">Můžete také vytvořit a použít vlastní čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="65cff-107">You can also create and use custom performance counters.</span></span>  

<span data-ttu-id="65cff-108">Můžete zkontrolovat data čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="65cff-108">You can examine performance counter data</span></span>

1. <span data-ttu-id="65cff-109">Přímo na hostiteli aplikace pomocí nástroje Sledování výkonu přistupovat pomocí vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="65cff-109">Directly on the application host with the Performance Monitor tool accessed using Remote Desktop</span></span>
2. <span data-ttu-id="65cff-110">S nástrojem System Center Operations Manager pomocí sady pro správu Azure</span><span class="sxs-lookup"><span data-stu-id="65cff-110">With System Center Operations Manager using the Azure Management Pack</span></span>
3. <span data-ttu-id="65cff-111">Pomocí jiných nástrojů monitorování, které přístup diagnostických dat přenést do úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="65cff-111">With other monitoring tools that access the diagnostic data transferred to Azure storage.</span></span> <span data-ttu-id="65cff-112">V tématu [úložiště a zobrazení diagnostických dat ve službě Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="65cff-112">See [Store and View Diagnostic Data in Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) for more information.</span></span>  

<span data-ttu-id="65cff-113">Další informace o monitorování výkonu aplikace v [portál Azure](http://portal.azure.com/), najdete v části [postup monitorování cloudové služby](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/).</span><span class="sxs-lookup"><span data-stu-id="65cff-113">For more information on monitoring the performance of your application in the [Azure portal](http://portal.azure.com/), see [How to Monitor Cloud Services](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/).</span></span>

<span data-ttu-id="65cff-114">Další podrobné pokyny k vytváření protokolování a trasování strategie a pomocí diagnostiky a další postupy k řešení problémů a optimalizaci aplikace Azure, najdete v části [řešení potíží s osvědčené postupy pro vývoj aplikací Azure](https://msdn.microsoft.com/library/azure/hh771389.aspx).</span><span class="sxs-lookup"><span data-stu-id="65cff-114">For additional in-depth guidance on creating a logging and tracing strategy and using diagnostics and other techniques to troubleshoot problems and optimize Azure applications, see [Troubleshooting Best Practices for Developing Azure Applications](https://msdn.microsoft.com/library/azure/hh771389.aspx).</span></span>

## <a name="enable-performance-counter-monitoring"></a><span data-ttu-id="65cff-115">Povolit monitorování čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="65cff-115">Enable performance counter monitoring</span></span>
<span data-ttu-id="65cff-116">Ve výchozím nastavení nejsou povoleny čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="65cff-116">Performance counters are not enabled by default.</span></span> <span data-ttu-id="65cff-117">Aplikace nebo úloha spuštění musíte změnit výchozí konfiguraci agenta diagnostiky zahrnout čítače výkonu konkrétní, které chcete sledovat pro každou instanci role.</span><span class="sxs-lookup"><span data-stu-id="65cff-117">Your application or a startup task must modify the default diagnostics agent configuration to include the specific performance counters that you wish to monitor for each role instance.</span></span>

### <a name="performance-counters-available-for-microsoft-azure"></a><span data-ttu-id="65cff-118">Čítače výkonu, které jsou k dispozici pro Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="65cff-118">Performance counters available for Microsoft Azure</span></span>
<span data-ttu-id="65cff-119">Azure poskytuje podmnožinu dostupných čítačů výkonu pro Windows Server, služba IIS a ASP.NET zásobníku.</span><span class="sxs-lookup"><span data-stu-id="65cff-119">Azure provides a subset of the performance counters available for Windows Server, IIS and the ASP.NET stack.</span></span> <span data-ttu-id="65cff-120">Následující tabulka uvádí některé čítače výkonu konkrétní týkající se aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="65cff-120">The following table lists some of the performance counters of particular interest for Azure applications.</span></span>

| <span data-ttu-id="65cff-121">Kategorie čítače: Objekt (Instance)</span><span class="sxs-lookup"><span data-stu-id="65cff-121">Counter Category: Object (Instance)</span></span> | <span data-ttu-id="65cff-122">Název čítače</span><span class="sxs-lookup"><span data-stu-id="65cff-122">Counter Name</span></span> | <span data-ttu-id="65cff-123">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="65cff-123">Reference</span></span> |
| --- | --- | --- |
| <span data-ttu-id="65cff-124">CLR – výjimky na rozhraní .NET (*globální*)</span><span class="sxs-lookup"><span data-stu-id="65cff-124">.NET CLR Exceptions(*Global*)</span></span> |<span data-ttu-id="65cff-125"># Výjimek / s</span><span class="sxs-lookup"><span data-stu-id="65cff-125"># Exceps Thrown / sec</span></span> |<span data-ttu-id="65cff-126">Čítače výkonu výjimky</span><span class="sxs-lookup"><span data-stu-id="65cff-126">Exception Performance Counters</span></span> |
| <span data-ttu-id="65cff-127">Využívání paměti rozhraním .NET CLR (*globální*)</span><span class="sxs-lookup"><span data-stu-id="65cff-127">.NET CLR Memory(*Global*)</span></span> |<span data-ttu-id="65cff-128">Čas</span><span class="sxs-lookup"><span data-stu-id="65cff-128">% Time in GC</span></span> |<span data-ttu-id="65cff-129">Čítače výkonu paměti</span><span class="sxs-lookup"><span data-stu-id="65cff-129">Memory Performance Counters</span></span> |
| <span data-ttu-id="65cff-130">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="65cff-130">ASP.NET</span></span> |<span data-ttu-id="65cff-131">Restartování aplikace</span><span class="sxs-lookup"><span data-stu-id="65cff-131">Application Restarts</span></span> |<span data-ttu-id="65cff-132">Čítače výkonu pro technologii ASP.NET</span><span class="sxs-lookup"><span data-stu-id="65cff-132">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="65cff-133">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="65cff-133">ASP.NET</span></span> |<span data-ttu-id="65cff-134">Čas provádění požadavku</span><span class="sxs-lookup"><span data-stu-id="65cff-134">Request Execution Time</span></span> |<span data-ttu-id="65cff-135">Čítače výkonu pro technologii ASP.NET</span><span class="sxs-lookup"><span data-stu-id="65cff-135">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="65cff-136">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="65cff-136">ASP.NET</span></span> |<span data-ttu-id="65cff-137">Odpojené požadavky</span><span class="sxs-lookup"><span data-stu-id="65cff-137">Requests Disconnected</span></span> |<span data-ttu-id="65cff-138">Čítače výkonu pro technologii ASP.NET</span><span class="sxs-lookup"><span data-stu-id="65cff-138">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="65cff-139">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="65cff-139">ASP.NET</span></span> |<span data-ttu-id="65cff-140">Restartování pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="65cff-140">Worker Process Restarts</span></span> |<span data-ttu-id="65cff-141">Čítače výkonu pro technologii ASP.NET</span><span class="sxs-lookup"><span data-stu-id="65cff-141">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="65cff-142">Aplikace ASP.NET (**celkový**)</span><span class="sxs-lookup"><span data-stu-id="65cff-142">ASP.NET Applications(**Total**)</span></span> |<span data-ttu-id="65cff-143">Celkový počet požadavků</span><span class="sxs-lookup"><span data-stu-id="65cff-143">Requests Total</span></span> |<span data-ttu-id="65cff-144">Čítače výkonu pro technologii ASP.NET</span><span class="sxs-lookup"><span data-stu-id="65cff-144">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="65cff-145">Aplikace ASP.NET (**celkový**)</span><span class="sxs-lookup"><span data-stu-id="65cff-145">ASP.NET Applications(**Total**)</span></span> |<span data-ttu-id="65cff-146">Počet požadavků za sekundu</span><span class="sxs-lookup"><span data-stu-id="65cff-146">Requests/Sec</span></span> |<span data-ttu-id="65cff-147">Čítače výkonu pro technologii ASP.NET</span><span class="sxs-lookup"><span data-stu-id="65cff-147">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="65cff-148">Položku ASP.NET v4.0.30319</span><span class="sxs-lookup"><span data-stu-id="65cff-148">ASP.NET v4.0.30319</span></span> |<span data-ttu-id="65cff-149">Čas provádění požadavku</span><span class="sxs-lookup"><span data-stu-id="65cff-149">Request Execution Time</span></span> |<span data-ttu-id="65cff-150">Čítače výkonu pro technologii ASP.NET</span><span class="sxs-lookup"><span data-stu-id="65cff-150">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="65cff-151">Položku ASP.NET v4.0.30319</span><span class="sxs-lookup"><span data-stu-id="65cff-151">ASP.NET v4.0.30319</span></span> |<span data-ttu-id="65cff-152">Čekací doba požadavku</span><span class="sxs-lookup"><span data-stu-id="65cff-152">Request Wait Time</span></span> |<span data-ttu-id="65cff-153">Čítače výkonu pro technologii ASP.NET</span><span class="sxs-lookup"><span data-stu-id="65cff-153">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="65cff-154">Položku ASP.NET v4.0.30319</span><span class="sxs-lookup"><span data-stu-id="65cff-154">ASP.NET v4.0.30319</span></span> |<span data-ttu-id="65cff-155">Aktuální požadavky</span><span class="sxs-lookup"><span data-stu-id="65cff-155">Requests Current</span></span> |<span data-ttu-id="65cff-156">Čítače výkonu pro technologii ASP.NET</span><span class="sxs-lookup"><span data-stu-id="65cff-156">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="65cff-157">Položku ASP.NET v4.0.30319</span><span class="sxs-lookup"><span data-stu-id="65cff-157">ASP.NET v4.0.30319</span></span> |<span data-ttu-id="65cff-158">Požadavky ve frontě</span><span class="sxs-lookup"><span data-stu-id="65cff-158">Requests Queued</span></span> |<span data-ttu-id="65cff-159">Čítače výkonu pro technologii ASP.NET</span><span class="sxs-lookup"><span data-stu-id="65cff-159">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="65cff-160">Položku ASP.NET v4.0.30319</span><span class="sxs-lookup"><span data-stu-id="65cff-160">ASP.NET v4.0.30319</span></span> |<span data-ttu-id="65cff-161">Zamítnuté požadavky</span><span class="sxs-lookup"><span data-stu-id="65cff-161">Requests Rejected</span></span> |<span data-ttu-id="65cff-162">Čítače výkonu pro technologii ASP.NET</span><span class="sxs-lookup"><span data-stu-id="65cff-162">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="65cff-163">Memory (Paměť)</span><span class="sxs-lookup"><span data-stu-id="65cff-163">Memory</span></span> |<span data-ttu-id="65cff-164">Počet MB k dispozici</span><span class="sxs-lookup"><span data-stu-id="65cff-164">Available MBytes</span></span> |<span data-ttu-id="65cff-165">Čítače výkonu paměti</span><span class="sxs-lookup"><span data-stu-id="65cff-165">Memory Performance Counters</span></span> |
| <span data-ttu-id="65cff-166">Memory (Paměť)</span><span class="sxs-lookup"><span data-stu-id="65cff-166">Memory</span></span> |<span data-ttu-id="65cff-167">Svěřené bajty</span><span class="sxs-lookup"><span data-stu-id="65cff-167">Committed Bytes</span></span> |<span data-ttu-id="65cff-168">Čítače výkonu paměti</span><span class="sxs-lookup"><span data-stu-id="65cff-168">Memory Performance Counters</span></span> |
| <span data-ttu-id="65cff-169">Procesor(_celkem)</span><span class="sxs-lookup"><span data-stu-id="65cff-169">Processor(_Total)</span></span> |<span data-ttu-id="65cff-170">% Času procesoru</span><span class="sxs-lookup"><span data-stu-id="65cff-170">% Processor Time</span></span> |<span data-ttu-id="65cff-171">Čítače výkonu pro technologii ASP.NET</span><span class="sxs-lookup"><span data-stu-id="65cff-171">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="65cff-172">TCPv4</span><span class="sxs-lookup"><span data-stu-id="65cff-172">TCPv4</span></span> |<span data-ttu-id="65cff-173">Počet selhání připojení</span><span class="sxs-lookup"><span data-stu-id="65cff-173">Connection Failures</span></span> |<span data-ttu-id="65cff-174">Objekt TCP</span><span class="sxs-lookup"><span data-stu-id="65cff-174">TCP Object</span></span> |
| <span data-ttu-id="65cff-175">TCPv4</span><span class="sxs-lookup"><span data-stu-id="65cff-175">TCPv4</span></span> |<span data-ttu-id="65cff-176">Vytvořeno připojení</span><span class="sxs-lookup"><span data-stu-id="65cff-176">Connections Established</span></span> |<span data-ttu-id="65cff-177">Objekt TCP</span><span class="sxs-lookup"><span data-stu-id="65cff-177">TCP Object</span></span> |
| <span data-ttu-id="65cff-178">TCPv4</span><span class="sxs-lookup"><span data-stu-id="65cff-178">TCPv4</span></span> |<span data-ttu-id="65cff-179">Resetování připojení</span><span class="sxs-lookup"><span data-stu-id="65cff-179">Connections Reset</span></span> |<span data-ttu-id="65cff-180">Objekt TCP</span><span class="sxs-lookup"><span data-stu-id="65cff-180">TCP Object</span></span> |
| <span data-ttu-id="65cff-181">TCPv4</span><span class="sxs-lookup"><span data-stu-id="65cff-181">TCPv4</span></span> |<span data-ttu-id="65cff-182">Segmenty odeslaných za sekundu</span><span class="sxs-lookup"><span data-stu-id="65cff-182">Segments Sent/sec</span></span> |<span data-ttu-id="65cff-183">Objekt TCP</span><span class="sxs-lookup"><span data-stu-id="65cff-183">TCP Object</span></span> |
| <span data-ttu-id="65cff-184">Interface(*) sítě</span><span class="sxs-lookup"><span data-stu-id="65cff-184">Network Interface(*)</span></span> |<span data-ttu-id="65cff-185">Přijaté bajty/s</span><span class="sxs-lookup"><span data-stu-id="65cff-185">Bytes Received/sec</span></span> |<span data-ttu-id="65cff-186">Objekt rozhraní sítě</span><span class="sxs-lookup"><span data-stu-id="65cff-186">Network Interface Object</span></span> |
| <span data-ttu-id="65cff-187">Interface(*) sítě</span><span class="sxs-lookup"><span data-stu-id="65cff-187">Network Interface(*)</span></span> |<span data-ttu-id="65cff-188">Odeslané bajty/s</span><span class="sxs-lookup"><span data-stu-id="65cff-188">Bytes Sent/sec</span></span> |<span data-ttu-id="65cff-189">Objekt rozhraní sítě</span><span class="sxs-lookup"><span data-stu-id="65cff-189">Network Interface Object</span></span> |
| <span data-ttu-id="65cff-190">Síťové rozhraní (Microsoft sběrnice virtuálního počítače síťový adaptér _2)</span><span class="sxs-lookup"><span data-stu-id="65cff-190">Network Interface(Microsoft Virtual Machine Bus Network Adapter _2)</span></span> |<span data-ttu-id="65cff-191">Přijaté bajty/s</span><span class="sxs-lookup"><span data-stu-id="65cff-191">Bytes Received/sec</span></span> |<span data-ttu-id="65cff-192">Objekt rozhraní sítě</span><span class="sxs-lookup"><span data-stu-id="65cff-192">Network Interface Object</span></span> |
| <span data-ttu-id="65cff-193">Síťové rozhraní (Microsoft sběrnice virtuálního počítače síťový adaptér _2)</span><span class="sxs-lookup"><span data-stu-id="65cff-193">Network Interface(Microsoft Virtual Machine Bus Network Adapter _2)</span></span> |<span data-ttu-id="65cff-194">Odeslané bajty/s</span><span class="sxs-lookup"><span data-stu-id="65cff-194">Bytes Sent/sec</span></span> |<span data-ttu-id="65cff-195">Objekt rozhraní sítě</span><span class="sxs-lookup"><span data-stu-id="65cff-195">Network Interface Object</span></span> |
| <span data-ttu-id="65cff-196">Síťové rozhraní (Microsoft sběrnice virtuálního počítače síťový adaptér _2)</span><span class="sxs-lookup"><span data-stu-id="65cff-196">Network Interface(Microsoft Virtual Machine Bus Network Adapter _2)</span></span> |<span data-ttu-id="65cff-197">Čítač Bajty celkem/s</span><span class="sxs-lookup"><span data-stu-id="65cff-197">Bytes Total/sec</span></span> |<span data-ttu-id="65cff-198">Objekt rozhraní sítě</span><span class="sxs-lookup"><span data-stu-id="65cff-198">Network Interface Object</span></span> |

## <a name="create-and-add-custom-performance-counters-to-your-application"></a><span data-ttu-id="65cff-199">Vytvořit a přidat vlastní čítače výkonu do vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="65cff-199">Create and add custom performance counters to your application</span></span>
<span data-ttu-id="65cff-200">Azure má podporu vytvářet a upravovat vlastní čítače výkonu pro webových rolí a rolí pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="65cff-200">Azure has support to create and modify custom performance counters for web roles and worker roles.</span></span> <span data-ttu-id="65cff-201">Čítače lze sledovat a monitorovat chování specifické pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="65cff-201">The counters may be used to track and monitor application-specific behavior.</span></span> <span data-ttu-id="65cff-202">Můžete vytvořit a kategorie čítače výkonu vlastní a specifikátory odstranit z úloha spuštění, webovou roli nebo role pracovního procesu se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="65cff-202">You can create and delete custom performance counter categories and specifiers from a startup task, web role, or worker role with elevated permissions.</span></span>

> [!NOTE]
> <span data-ttu-id="65cff-203">Kód, který provede změny vlastní čítače výkonu, musí mít zvýšená oprávnění ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="65cff-203">Code that makes changes to custom performance counters must have elevated permissions to run.</span></span> <span data-ttu-id="65cff-204">Pokud je kód v webovou roli nebo role pracovního procesu, role musí obsahovat značky <Runtime executionContext="elevated" /> v souboru ServiceDefinition.csdef pro roli tak, aby správně inicializovat.</span><span class="sxs-lookup"><span data-stu-id="65cff-204">If the code is in a web role or worker role, the role must include the tag <Runtime executionContext="elevated" /> in the ServiceDefinition.csdef file for the role to initialize properly.</span></span>
>
>

<span data-ttu-id="65cff-205">Můžete odeslat data čítače výkonu vlastní do úložiště Azure pomocí diagnostiky agenta.</span><span class="sxs-lookup"><span data-stu-id="65cff-205">You can send custom performance counter data to Azure storage using the diagnostics agent.</span></span>

<span data-ttu-id="65cff-206">Data čítače standardní výkonu je generován Azure procesy.</span><span class="sxs-lookup"><span data-stu-id="65cff-206">The standard performance counter data is generated by the Azure processes.</span></span> <span data-ttu-id="65cff-207">Data čítače výkonu vlastní musí být vytvořeny webovou aplikací role role nebo pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="65cff-207">Custom performance counter data must be created by your web role or worker role application.</span></span> <span data-ttu-id="65cff-208">V tématu [typy čítačů výkonu](https://msdn.microsoft.com/library/z573042h.aspx) informace o typech dat, která mohou být uloženy ve vlastní čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="65cff-208">See [Performance Counter Types](https://msdn.microsoft.com/library/z573042h.aspx) for information on the types of data that can be stored in custom performance counters.</span></span> <span data-ttu-id="65cff-209">V tématu [Ukázka čítače výkonu](http://code.msdn.microsoft.com/azure/) pro příklad, který vytvoří a nastaví data čítače výkonu vlastní ve webové roli.</span><span class="sxs-lookup"><span data-stu-id="65cff-209">See [PerformanceCounters Sample](http://code.msdn.microsoft.com/azure/) for an example that creates and sets custom performance counter data in a web role.</span></span>

## <a name="store-and-view-performance-counter-data"></a><span data-ttu-id="65cff-210">Úložiště a zobrazení data čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="65cff-210">Store and view performance counter data</span></span>
<span data-ttu-id="65cff-211">Azure data čítače výkonu pomocí jiné diagnostické informace ukládá do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="65cff-211">Azure caches performance counter data with other diagnostic information.</span></span> <span data-ttu-id="65cff-212">Tato data jsou k dispozici pro vzdálené monitorování, když role instance běží, chcete-li zobrazit nástroje, jako je monitorování výkonu pomocí přístup ke vzdálené ploše.</span><span class="sxs-lookup"><span data-stu-id="65cff-212">This data is available for remote monitoring while the role instance is running using remote desktop access to view tools such as Performance Monitor.</span></span> <span data-ttu-id="65cff-213">Zachování dat mimo instanci role, agent diagnostiky data musíte přenést do úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="65cff-213">To persist the data outside of the role instance, the diagnostics agent must transfer the data to Azure storage.</span></span> <span data-ttu-id="65cff-214">Omezení velikosti data čítače výkonu v mezipaměti se dá nakonfigurovat v agenta diagnostiky nebo může být nakonfigurována jako součást sdílených limitu diagnostická data.</span><span class="sxs-lookup"><span data-stu-id="65cff-214">The size limit of the cached performance counter data can be configured in the diagnostics agent, or it may be configured to be part of a shared limit for all the diagnostic data.</span></span> <span data-ttu-id="65cff-215">Další informace o nastavení velikosti vyrovnávací paměti najdete v tématu [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) a [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx).</span><span class="sxs-lookup"><span data-stu-id="65cff-215">For more information about setting the buffer size, see [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) and [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx).</span></span> <span data-ttu-id="65cff-216">V tématu [úložiště a zobrazení diagnostických dat ve službě Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) přehled nastavení diagnostiky agenta k přenosu dat do účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="65cff-216">See [Store and View Diagnostic Data in Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) for an overview of setting up the diagnostics agent to transfer data to a storage account.</span></span>

<span data-ttu-id="65cff-217">Každá instance čítače výkonu nakonfigurované se zaznamenává v zadané vzorkovací frekvenci a jen Vzorkovaná data se přenáší k účtu úložiště tak, že žádost o přenos plánované nebo žádost o přenos na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="65cff-217">Each configured performance counter instance is recorded at a specified sampling rate, and the sampled data is transferred to the storage account either by a scheduled transfer request or an on-demand transfer request.</span></span> <span data-ttu-id="65cff-218">Automatické přenosy může být naplánovaná tak často, jak jednou za minutu.</span><span class="sxs-lookup"><span data-stu-id="65cff-218">Automatic transfers may be scheduled as often as once per minute.</span></span> <span data-ttu-id="65cff-219">Přenést agentem diagnostiky data čítače výkonu je uložena v tabulce, WADPerformanceCountersTable, v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="65cff-219">Performance counter data transferred by the diagnostics agent is stored in a table, WADPerformanceCountersTable, in the storage account.</span></span> <span data-ttu-id="65cff-220">Tato tabulka může získat přístup a dotaz pomocí metod úložiště Azure úrovně standard rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="65cff-220">This table may be accessed and queried with standard Azure storage API methods.</span></span> <span data-ttu-id="65cff-221">V tématu [Ukázka čítače výkonu Microsoft Azure](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) příklad dotazování a zobrazení data čítače výkonu z tabulky WADPerformanceCountersTable.</span><span class="sxs-lookup"><span data-stu-id="65cff-221">See [Microsoft Azure PerformanceCounters Sample](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) for an example of querying and displaying performance counter data from the WADPerformanceCountersTable table.</span></span>

> [!NOTE]
> <span data-ttu-id="65cff-222">V závislosti na Diagnostika agenta přenosu četnost a fronty latence nejnovější data čítače výkonu v účtu úložiště může být zastaralý několik minut.</span><span class="sxs-lookup"><span data-stu-id="65cff-222">Depending on the diagnostics agent transfer frequency and queue latency, the most recent performance counter data in the storage account may be several minutes out of date.</span></span>
>
>

## <a name="enable-performance-counters-using-diagnostics-configuration-file"></a><span data-ttu-id="65cff-223">Povolit čítače výkonu pomocí diagnostiky konfiguračního souboru</span><span class="sxs-lookup"><span data-stu-id="65cff-223">Enable performance counters using diagnostics configuration file</span></span>
<span data-ttu-id="65cff-224">Pomocí následujícího postupu umožníte čítače výkonu v aplikaci Azure.</span><span class="sxs-lookup"><span data-stu-id="65cff-224">Use the following procedure to enable performance counters in your Azure application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="65cff-225">Požadavky</span><span class="sxs-lookup"><span data-stu-id="65cff-225">Prerequisites</span></span>
<span data-ttu-id="65cff-226">Této části se předpokládá, že máte importovat monitorování diagnostiky aplikace a přidat konfiguračního souboru diagnostiku do řešení sady Visual Studio (diagnostics.wadcfg SDK 2.4 a nižší nebo diagnostics.wadcfgx SDK 2.5 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="65cff-226">This section assumes that you have imported the Diagnostics monitor into your application and added the diagnostics configuration file to your Visual Studio solution (diagnostics.wadcfg in SDK 2.4 and below or diagnostics.wadcfgx in SDK 2.5 and above).</span></span> <span data-ttu-id="65cff-227">Informace o kroky 1 a 2 v [povolení diagnostiky Azure Cloud Services a virtuálních počítačů](cloud-services-dotnet-diagnostics.md)) Další informace.</span><span class="sxs-lookup"><span data-stu-id="65cff-227">See steps 1 and 2 in [Enabling Diagnostics in Azure Cloud Services and Virtual Machines](cloud-services-dotnet-diagnostics.md)) for more information.</span></span>

## <a name="step-1-collect-and-store-data-from-performance-counters"></a><span data-ttu-id="65cff-228">Krok 1: Shromažďovat a ukládat data z čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="65cff-228">Step 1: Collect and store data from performance counters</span></span>
<span data-ttu-id="65cff-229">Po přidání souboru diagnostiky k řešení sady Visual Studio, můžete nakonfigurovat kolekce a ukládání data čítače výkonu v aplikaci Azure.</span><span class="sxs-lookup"><span data-stu-id="65cff-229">After you have added the diagnostics file to your Visual Studio solution, you can configure the collection and storage of performance counter data in an Azure application.</span></span> <span data-ttu-id="65cff-230">To se provádí přidáním čítačů výkonu k souboru diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="65cff-230">This is done by adding performance counters to the diagnostics file.</span></span> <span data-ttu-id="65cff-231">Diagnostická data, včetně čítačů výkonu, je nejprve shromážděné v instanci.</span><span class="sxs-lookup"><span data-stu-id="65cff-231">Diagnostics data, including performance counters, is first collected on the instance.</span></span> <span data-ttu-id="65cff-232">Data se pak jako trvalý, do tabulky WADPerformanceCountersTable ve službě Azure Table, budete taky muset zadat účet úložiště ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="65cff-232">The data is then persisted to the WADPerformanceCountersTable table in the Azure Table service, so you will also need to specify the storage account in your application.</span></span> <span data-ttu-id="65cff-233">Pokud testujete aplikace místně v emulátoru výpočetní, můžete také ukládat diagnostická data místně v emulátoru úložiště.</span><span class="sxs-lookup"><span data-stu-id="65cff-233">If you're testing your application locally in the Compute Emulator, you can also store diagnostics data locally in the Storage Emulator.</span></span> <span data-ttu-id="65cff-234">Než bude ukládat data diagnostiky, musíte nejdřív přejít na [portál Azure](http://portal.azure.com/) a vytvoření účtu úložiště classic.</span><span class="sxs-lookup"><span data-stu-id="65cff-234">Before you store diagnostics data, you must first go to the [Azure portal](http://portal.azure.com/) and create a classic storage account.</span></span> <span data-ttu-id="65cff-235">Osvědčeným postupem je najít váš účet úložiště ve stejném geograficky umístění jako vaše aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="65cff-235">A best practice is to locate your storage account in the same geo-location as your Azure application.</span></span> <span data-ttu-id="65cff-236">Podle uchování aplikací Azure a účet úložiště se v stejného geografického umístění, vyhněte se platícího náklady na externí šířku pásma a snižování latence.</span><span class="sxs-lookup"><span data-stu-id="65cff-236">By keeping the Azure application and storage account are in the same geo-location, you avoid paying external bandwidth costs and reduce latency.</span></span>

### <a name="add-performance-counters-to-the-diagnostics-file"></a><span data-ttu-id="65cff-237">Přidání čítačů výkonu k souboru diagnostiky</span><span class="sxs-lookup"><span data-stu-id="65cff-237">Add performance counters to the diagnostics file</span></span>
<span data-ttu-id="65cff-238">Existuje mnoho čítačů, které můžete použít.</span><span class="sxs-lookup"><span data-stu-id="65cff-238">There are many counters you can use.</span></span> <span data-ttu-id="65cff-239">Následující příklad ukazuje několik čítače výkonu, které se doporučují pro webové a pracovní role monitorování.</span><span class="sxs-lookup"><span data-stu-id="65cff-239">The following example shows several performance counters that are recommended for web and worker role monitoring.</span></span>

<span data-ttu-id="65cff-240">Otevřete soubor diagnostiky (diagnostics.wadcfg SDK 2.4 a nižší nebo diagnostics.wadcfgx v SDK 2.5 a vyšší) a přidejte následující DiagnosticMonitorConfiguration element:</span><span class="sxs-lookup"><span data-stu-id="65cff-240">Open the diagnostics file (diagnostics.wadcfg in SDK 2.4 and below or diagnostics.wadcfgx in SDK 2.5 and above) and add the following to the DiagnosticMonitorConfiguration element:</span></span>

```xml
<PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
    <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT30S" />

    <!-- Use the Process(w3wp) category counters in a web role -->

    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\% Processor Time" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Private Bytes" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Thread Count" sampleRate="PT30S" />

    <!-- Use the Process(WaWorkerHost) category counters in a worker role.
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

<span data-ttu-id="65cff-241">Atribut bufferQuotaInMB, který určuje maximální množství úložiště systému souborů, které je dostupné pro vybraný typ kolekce dat (Azure protokoly, protokoly služby IIS, atd.).</span><span class="sxs-lookup"><span data-stu-id="65cff-241">The bufferQuotaInMB attribute, which specifies the maximum amount of file system storage that is available for the data collection type (Azure logs, IIS logs, etc.).</span></span> <span data-ttu-id="65cff-242">Výchozí hodnota je 0.</span><span class="sxs-lookup"><span data-stu-id="65cff-242">The default is 0.</span></span> <span data-ttu-id="65cff-243">Když je dosaženo kvóty, nejstarší data je odstranit, protože se přidá nová data.</span><span class="sxs-lookup"><span data-stu-id="65cff-243">When the quota is reached, the oldest data is deleted as new data is added.</span></span> <span data-ttu-id="65cff-244">Součet všech vlastností bufferQuotaInMB musí být větší než hodnota atributu OverallQuotaInMB.</span><span class="sxs-lookup"><span data-stu-id="65cff-244">The sum of all the bufferQuotaInMB properties must be greater than the value of the OverallQuotaInMB attribute.</span></span> <span data-ttu-id="65cff-245">Podrobnější informace o určení, jak velké úložiště se bude vyžadovat pro sběr dat diagnostiky, najdete v části instalace WAD [řešení potíží s osvědčené postupy pro vývoj aplikací Azure](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx).</span><span class="sxs-lookup"><span data-stu-id="65cff-245">For a more detailed discussion of determining how much storage will be required for the collection of diagnostics data, see the Setup WAD section of [Troubleshooting Best Practices for Developing Azure Applications](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx).</span></span>

<span data-ttu-id="65cff-246">ScheduledTransferPeriod atribut, který určuje interval mezi naplánované přenosů dat, zaokrouhlený nahoru na nejbližší minutu.</span><span class="sxs-lookup"><span data-stu-id="65cff-246">The scheduledTransferPeriod attribute, which specifies the interval between scheduled transfers of data, rounded up to the nearest minute.</span></span> <span data-ttu-id="65cff-247">V následujících příkladech jinak je nastavená na PT30M (30 minut).</span><span class="sxs-lookup"><span data-stu-id="65cff-247">In the following examples, it is set to PT30M (30 minutes).</span></span> <span data-ttu-id="65cff-248">Nastavení v období na malou hodnotu, jako je 1 minuta, bude nepříznivě ovlivnit výkon vaší aplikace v provozním prostředí, ale může být užitečná pro vidět diagnostiky rychle práce při testování.</span><span class="sxs-lookup"><span data-stu-id="65cff-248">Setting the transfer period to a small value, such as 1 minute, will adversely impact your application's performance in production but can be useful for seeing diagnostics working quickly when you are testing.</span></span> <span data-ttu-id="65cff-249">V naplánované období musí být dostatečně malé, aby Ujistěte se, že není přepsán diagnostických dat v instanci, ale dostatečně velké na to, že nebude mít dopad na výkon vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="65cff-249">The scheduled transfer period should be small enough to ensure that diagnostic data is not overwritten on the instance, but large enough that it will not impact the performance of your application.</span></span>

<span data-ttu-id="65cff-250">Atribut counterSpecifier Určuje čítače výkonu ke shromažďování.</span><span class="sxs-lookup"><span data-stu-id="65cff-250">The counterSpecifier attribute specifies the performance counter to collect.</span></span> <span data-ttu-id="65cff-251">Atribut sampleRate určuje rychlost, jakou čítače výkonu se odeberou, v takovém případě 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="65cff-251">The sampleRate attribute specifies the rate at which the performance counter should be sampled, in this case 30 seconds.</span></span>

<span data-ttu-id="65cff-252">Po přidání čítačů výkonu, které chcete shromáždit, uložte změny do souboru diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="65cff-252">Once you've added the performance counters that you want to collect, save your changes to the diagnostics file.</span></span> <span data-ttu-id="65cff-253">Dále je třeba zadat účet úložiště, který bude formou diagnostická data.</span><span class="sxs-lookup"><span data-stu-id="65cff-253">Next, you need to specify the storage account that the diagnostics data will be persisted to.</span></span>

### <a name="specify-the-storage-account"></a><span data-ttu-id="65cff-254">Zadejte účet úložiště</span><span class="sxs-lookup"><span data-stu-id="65cff-254">Specify the storage account</span></span>
<span data-ttu-id="65cff-255">Chcete-li zachovat vaše diagnostické informace k vašemu účtu úložiště Azure, musíte zadat připojovací řetězec v souboru konfigurace (souboru ServiceConfiguration.cscfg) služby.</span><span class="sxs-lookup"><span data-stu-id="65cff-255">To persist your diagnostics information to your Azure Storage account, you must specify a connection string in your service configuration (ServiceConfiguration.cscfg) file.</span></span>

<span data-ttu-id="65cff-256">Pro Azure SDK 2.5 můžete zadat účet úložiště v souboru diagnostics.wadcfgx.</span><span class="sxs-lookup"><span data-stu-id="65cff-256">For Azure SDK 2.5, the Storage Account can be specified in the diagnostics.wadcfgx file.</span></span>

> [!NOTE]
> <span data-ttu-id="65cff-257">Tyto pokyny platí pouze Azure SDK 2.4 a níže.</span><span class="sxs-lookup"><span data-stu-id="65cff-257">These instructions only apply to Azure SDK 2.4 and below.</span></span> <span data-ttu-id="65cff-258">Pro Azure SDK 2.5 můžete zadat účet úložiště v souboru diagnostics.wadcfgx.</span><span class="sxs-lookup"><span data-stu-id="65cff-258">For Azure SDK 2.5, the Storage Account can be specified in the diagnostics.wadcfgx file.</span></span>
>
>

<span data-ttu-id="65cff-259">Chcete-li nastavit připojovací řetězce:</span><span class="sxs-lookup"><span data-stu-id="65cff-259">To set the connection strings:</span></span>

1. <span data-ttu-id="65cff-260">Otevřete soubor ServiceConfiguration.Cloud.cscfg pomocí svém oblíbeném textovém editoru a nastavte připojovací řetězec pro úložiště.</span><span class="sxs-lookup"><span data-stu-id="65cff-260">Open the ServiceConfiguration.Cloud.cscfg file using your favorite text editor and set the connection string for your storage.</span></span> <span data-ttu-id="65cff-261">*AccountName* a *AccountKey* hodnoty se nacházejí v portálu Azure v řídicím panelu účet úložiště, v části přístupové klíče.</span><span class="sxs-lookup"><span data-stu-id="65cff-261">The *AccountName* and *AccountKey* values are found in the Azure portal in the storage account dashboard, under Access keys.</span></span>

    ```xml
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>"/>
    </ConfigurationSettings>
    ```
2. <span data-ttu-id="65cff-262">Uložte soubor ServiceConfiguration.Cloud.cscfg.</span><span class="sxs-lookup"><span data-stu-id="65cff-262">Save the ServiceConfiguration.Cloud.cscfg file.</span></span>
3. <span data-ttu-id="65cff-263">Otevřete soubor ServiceConfiguration.Local.cscfg a ověřte, zda je UseDevelopmentStorage nastavena na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="65cff-263">Open the ServiceConfiguration.Local.cscfg file and verify that UseDevelopmentStorage is set to true.</span></span>

    ```xml
    <ConfigurationSettings>
      <Settingname="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true"/>
    </ConfigurationSettings>
    ```
   <span data-ttu-id="65cff-264">Teď, když jsou nastavené připojovací řetězce, aplikace se uchová diagnostická data do účtu úložiště při nasazení vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="65cff-264">Now that the connection strings are set, your application will persist diagnostics data to your storage account when your application is deployed.</span></span>
4. <span data-ttu-id="65cff-265">Uložte a sestavení projektu a nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="65cff-265">Save and build your project, then deploy your application.</span></span>

## <a name="step-2-optional-create-custom-performance-counters"></a><span data-ttu-id="65cff-266">Krok 2: (Volitelné) vytvořit vlastní čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="65cff-266">Step 2: (Optional) Create custom performance counters</span></span>
<span data-ttu-id="65cff-267">Kromě předdefinovaných čítače můžete přidat vlastní vlastní čítače výkonu pro monitorování role web nebo worker.</span><span class="sxs-lookup"><span data-stu-id="65cff-267">In addition to the pre-defined performance counters, you can add your own custom performance counters to monitor web or worker roles.</span></span> <span data-ttu-id="65cff-268">Vlastní čítače výkonu lze sledovat a monitorovat chování specifické pro aplikaci a můžou vytvořit nebo odstranit v úloha spuštění, webovou roli nebo role pracovního procesu se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="65cff-268">Custom performance counters may be used to track and monitor application-specific behavior and can be created or deleted in a startup task, web role, or worker role with elevated permissions.</span></span>

<span data-ttu-id="65cff-269">Agent Azure diagnostics aktualizuje výkonu čítač konfigurace ze souboru .wadcfg jednu minutu po spuštění.</span><span class="sxs-lookup"><span data-stu-id="65cff-269">The Azure diagnostics agent refreshes the performance counter configuration from the .wadcfg file one minute after starting.</span></span>  <span data-ttu-id="65cff-270">Pokud vytvoříte vlastní čítače výkonu v metodě OnStart a úlohami spuštění trvá déle než minutu provést, vaše vlastní čítače výkonu nebudou byl vytvořen agenta Azure Diagnostics pokusí načíst je.</span><span class="sxs-lookup"><span data-stu-id="65cff-270">If you create custom performance counters in the OnStart method and your startup tasks take longer than one minute to execute, your custom performance counters will not have been created when the Azure Diagnostics agent tries to load them.</span></span>  <span data-ttu-id="65cff-271">V tomto scénáři uvidíte, že Azure Diagnostics správně zaznamená všechny diagnostická data s výjimkou vaše vlastní čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="65cff-271">In this scenario, you will see that Azure Diagnostics correctly captures all diagnostics data except your custom performance counters.</span></span>  <span data-ttu-id="65cff-272">Chcete-li vyřešit tento problém, vytvořte čítače výkonu v úloze pro spuštění nebo přesunout některé vaše úloha spuštění metody OnStart fungovat po vytvoření čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="65cff-272">To resolve this issue, create the performance counters in a startup task or move some of your startup task work to the OnStart method after creating the performance counters.</span></span>

<span data-ttu-id="65cff-273">Proveďte následující kroky k vytvoření jednoduché vlastní výkon čítač s názvem "\MyCustomCounterCategory\MyButton1Counter":</span><span class="sxs-lookup"><span data-stu-id="65cff-273">Perform the following steps to create a simple custom performance counter named "\MyCustomCounterCategory\MyButton1Counter":</span></span>

1. <span data-ttu-id="65cff-274">Otevřete soubor definice služby (CSDEF) pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="65cff-274">Open the service definition file (CSDEF) for your application.</span></span>
2. <span data-ttu-id="65cff-275">Přidáte element Runtime WebRole nebo WorkerRole elementu, který chcete povolit spuštění se zvýšenými oprávněními:</span><span class="sxs-lookup"><span data-stu-id="65cff-275">Add the Runtime element to the WebRole or WorkerRole element to allow execution with elevated privileges:</span></span>

    ```xml
    <runtime executioncontext="elevated"/>
    ```
3. <span data-ttu-id="65cff-276">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="65cff-276">Save the file.</span></span>
4. <span data-ttu-id="65cff-277">Otevřete soubor diagnostiky (diagnostics.wadcfg SDK 2.4 a nižší nebo diagnostics.wadcfgx v SDK 2.5 a vyšší) a přidejte následující DiagnosticMonitorConfiguration</span><span class="sxs-lookup"><span data-stu-id="65cff-277">Open the diagnostics file (diagnostics.wadcfg in SDK 2.4 and below or diagnostics.wadcfgx in SDK 2.5 and above) and add the following to the DiagnosticMonitorConfiguration</span></span>

    ```xml
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
      <PerformanceCounterConfiguration counterSpecifier="\MyCustomCounterCategory\MyButton1Counter" sampleRate="PT30S"/>
    </PerformanceCounters>
    ```
5. <span data-ttu-id="65cff-278">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="65cff-278">Save the file.</span></span>
6. <span data-ttu-id="65cff-279">Vytvoření kategorie čítače výkonu vlastní metoda OnStart role, před vyvoláním base. OnStart.</span><span class="sxs-lookup"><span data-stu-id="65cff-279">Create the custom performance counter category in the OnStart method of your role, before invoking base.OnStart.</span></span> <span data-ttu-id="65cff-280">Následující příklad jazyka C# vytvoří vlastní kategorii, pokud ještě neexistuje:</span><span class="sxs-lookup"><span data-stu-id="65cff-280">The following C# example creates a custom category, if it does not already exist:</span></span>

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
7. <span data-ttu-id="65cff-281">Aktualizace čítačů v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="65cff-281">Update the counters within your application.</span></span> <span data-ttu-id="65cff-282">Následující příklad aktualizací čítače výkonu vlastní Button1_Click událostí:</span><span class="sxs-lookup"><span data-stu-id="65cff-282">The following example updates a custom performance counter on Button1_Click events:</span></span>

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
8. <span data-ttu-id="65cff-283">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="65cff-283">Save the file.</span></span>  

<span data-ttu-id="65cff-284">Data čítače výkonu vlastní budou shromažďována teď monitorování Azure diagnostics.</span><span class="sxs-lookup"><span data-stu-id="65cff-284">Custom performance counter data will now be collected by the Azure diagnostics monitor.</span></span>

## <a name="step-3-query-performance-counter-data"></a><span data-ttu-id="65cff-285">Krok 3: Dotaz na data čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="65cff-285">Step 3: Query performance counter data</span></span>
<span data-ttu-id="65cff-286">Jakmile se vaše aplikace je nasazená a spuštěna, bude zahájeno monitorování diagnostiky shromažďování čítačů výkonu a uložením dat do úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="65cff-286">Once your application is deployed and running, the Diagnostics monitor will begin collecting performance counters and persisting that data to Azure storage.</span></span> <span data-ttu-id="65cff-287">Pomocí nástrojů, jako je Průzkumníka serveru v sadě Visual Studio, [Azure Storage Explorer](http://azurestorageexplorer.codeplex.com/), nebo [Azure Diagnostics Manager](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) podle Cerebrata Chcete-li zobrazit čítače výkonu dat v tabulce WADPerformanceCountersTable.</span><span class="sxs-lookup"><span data-stu-id="65cff-287">You use tools such as Server Explorer in Visual Studio,  [Azure Storage Explorer](http://azurestorageexplorer.codeplex.com/), or [Azure Diagnostics Manager](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) by Cerebrata to view the performance counters data in the WADPerformanceCountersTable table.</span></span> <span data-ttu-id="65cff-288">Můžete také programově dotazovat pomocí služby Table [C#](../cosmos-db/table-storage-how-to-use-dotnet.md), [Java](../cosmos-db/table-storage-how-to-use-java.md), [Node.js](../cosmos-db/table-storage-how-to-use-nodejs.md), [Python](../cosmos-db/table-storage-how-to-use-python.md), [Ruby](../cosmos-db/table-storage-how-to-use-ruby.md), nebo [PHP](../cosmos-db/table-storage-how-to-use-php.md).</span><span class="sxs-lookup"><span data-stu-id="65cff-288">You can also programmatically query the Table service using [C#](../cosmos-db/table-storage-how-to-use-dotnet.md),  [Java](../cosmos-db/table-storage-how-to-use-java.md),  [Node.js](../cosmos-db/table-storage-how-to-use-nodejs.md), [Python](../cosmos-db/table-storage-how-to-use-python.md), [Ruby](../cosmos-db/table-storage-how-to-use-ruby.md), or [PHP](../cosmos-db/table-storage-how-to-use-php.md).</span></span>

<span data-ttu-id="65cff-289">V následujícím příkladu C# ukazuje základní dotaz s tabulkou WADPerformanceCountersTable a uloží diagnostická data do souboru CSV.</span><span class="sxs-lookup"><span data-stu-id="65cff-289">The following C# example shows a basic query against the WADPerformanceCountersTable table and saves the diagnostics data to a CSV file.</span></span> <span data-ttu-id="65cff-290">Po čítače výkonu se uloží do souboru CSV, můžete k vizualizaci dat grafovým možnosti v aplikaci Microsoft Excel nebo nějaký jiný nástroj.</span><span class="sxs-lookup"><span data-stu-id="65cff-290">Once the performance counters are saved to a CSV file, you can use the graphing capabilities in Microsoft Excel or some other tool to visualize the data.</span></span> <span data-ttu-id="65cff-291">Nezapomeňte přidat odkaz na Microsoft.WindowsAzure.Storage.dll, který je obsažen v sadě Azure SDK pro .NET říjen 2012 a novějších.</span><span class="sxs-lookup"><span data-stu-id="65cff-291">Be sure to add a reference to Microsoft.WindowsAzure.Storage.dll, which is included in the Azure SDK for .NET October 2012 and later.</span></span> <span data-ttu-id="65cff-292">Sestavení je nainstalována k adresáři % Program Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version-num\ref\.</span><span class="sxs-lookup"><span data-stu-id="65cff-292">The assembly is installed to the %Program Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version-num\ref\ directory.</span></span>

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Table;
...

// Get the connection string. When using Microsoft Azure Cloud Services, it is recommended
// you store your connection string using the Microsoft Azure service configuration
// system (*.csdef and *.cscfg files). You can you use the CloudConfigurationManager type
// to retrieve your storage connection string.  If you're not using Cloud Services, it's
// recommended that you store the connection string in your web.config or app.config file.
// Use the ConfigurationManager type to retrieve your storage connection string.

string connectionString = Microsoft.WindowsAzure.CloudConfigurationManager.GetSetting("StorageConnectionString");
//string connectionString = System.Configuration.ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString;

// Get a reference to the storage account using the connection string.  You can also use the development
// storage account (Storage Emulator) for local debugging.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
//CloudStorageAccount storageAccount = CloudStorageAccount.DevelopmentStorageAccount;

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "WADPerformanceCountersTable" table.
CloudTable table = tableClient.GetTableReference("WADPerformanceCountersTable");

// Create the table query, filter on a specific CounterName, DeploymentId and RoleInstance.
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

// Execute the table query.
IEnumerable<PerformanceCountersEntity> result = table.ExecuteQuery(query);

// Process the query results and build a CSV file.
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

<span data-ttu-id="65cff-293">Entity se mapují na objekty C# pomocí vlastní třídy odvozené od **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="65cff-293">Entities map to C# objects using a custom class derived from **TableEntity**.</span></span> <span data-ttu-id="65cff-294">Následující kód definuje třídu entity, který představuje čítače výkonu ve **WADPerformanceCountersTable** tabulky.</span><span class="sxs-lookup"><span data-stu-id="65cff-294">The following code defines an entity class that represents a performance counter in the **WADPerformanceCountersTable** table.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="65cff-295">Další kroky</span><span class="sxs-lookup"><span data-stu-id="65cff-295">Next Steps</span></span>
[<span data-ttu-id="65cff-296">Zobrazit další články v Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="65cff-296">View additional articles on Azure Diagnostics</span></span>](../azure-diagnostics.md)
