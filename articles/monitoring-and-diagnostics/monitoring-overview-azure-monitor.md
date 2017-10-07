---
title: "aaaAzure Přehled monitorování | Microsoft Docs"
description: "Azure monitorování shromažďuje statistiky pro použití v výstrahy, webhooků, škálování a automatizace. Článek taky seznam dalších možností monitorování společnosti Microsoft."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: robb
ms.openlocfilehash: ffa304e7b158f0fceb7f60ab88fab291976aa0e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-azure-monitor"></a><span data-ttu-id="7d6e5-104">Přehled Azure monitorování</span><span class="sxs-lookup"><span data-stu-id="7d6e5-104">Overview of Azure Monitor</span></span>
<span data-ttu-id="7d6e5-105">Tento článek obsahuje přehled hello Azure monitorování služby ve službě Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-105">This article provides an overview of hello Azure Monitor service in Microsoft Azure.</span></span> <span data-ttu-id="7d6e5-106">Popisuje, co monitorování Azure nepodporuje a obsahuje ukazatele tooadditional informace o toouse Azure monitorování.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-106">It discusses what Azure Monitor does and provides pointers tooadditional information on how toouse Azure Monitor.</span></span>  <span data-ttu-id="7d6e5-107">Pokud upřednostňujete video úvod, najdete v části Další odkazy na kroky v hello dolní části tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-107">If you prefer a video introduction, see Next steps links at hello bottom of this article.</span></span> 

## <a name="why-monitor-your-application-or-system"></a><span data-ttu-id="7d6e5-108">Proč monitorování aplikace nebo systému</span><span class="sxs-lookup"><span data-stu-id="7d6e5-108">Why monitor your application or system</span></span>
<span data-ttu-id="7d6e5-109">Cloudové aplikace jsou komplexní s mnoha přesunutí částmi.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-109">Cloud applications are complex with many moving parts.</span></span> <span data-ttu-id="7d6e5-110">Monitorování poskytuje tooensure data, která vaše aplikace zůstává nahoru a spuštěn v dobrém stavu.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-110">Monitoring provides data tooensure that your application stays up and running in a healthy state.</span></span> <span data-ttu-id="7d6e5-111">Pomáhá také můžete toostave vypnout potenciální problémy a řešení potíží s uplynulou těch, které jsou.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-111">It also helps you toostave off potential problems or troubleshoot past ones.</span></span> <span data-ttu-id="7d6e5-112">Kromě toho můžete použít monitorování hlubšímu porozumění toogain data o vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-112">In addition, you can use monitoring data toogain deep insights about your application.</span></span> <span data-ttu-id="7d6e5-113">Dané znalosti můžete vám pomohou tooimprove výkon aplikace nebo udržovatelnosti nebo automatizaci akcí, které by jinak vyžadují ruční zásah.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-113">That knowledge can help you tooimprove application performance or maintainability, or automate actions that would otherwise require manual intervention.</span></span>


## <a name="azure-monitor-and-microsofts-other-monitoring-products"></a><span data-ttu-id="7d6e5-114">Azure monitorování a Microsoft je další monitorování produkty</span><span class="sxs-lookup"><span data-stu-id="7d6e5-114">Azure Monitor and Microsoft's other monitoring products</span></span>
<span data-ttu-id="7d6e5-115">Monitorování Azure poskytuje základní úroveň infrastruktura metriky a protokoly pro většina služeb v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-115">Azure Monitor provides base level infrastructure metrics and logs for most services in Microsoft Azure.</span></span> <span data-ttu-id="7d6e5-116">Služby Azure, které ještě Neumísťujte svá data do Azure monitorování se pro něj existuje hello budoucí.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-116">Azure services that do not yet put their data into Azure Monitor will put it there in hello future.</span></span>

<span data-ttu-id="7d6e5-117">Microsoft dodává další produkty a služby, které poskytují další možnosti monitorování pro vývojáře, DevOps nebo Ops IT, které mají také na místní instalace.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-117">Microsoft ships additional products and services that provide additional monitoring capabilities for developers, DevOps, or IT Ops that also have on-premises installations.</span></span> <span data-ttu-id="7d6e5-118">Přehled a pochopení jak tyto různé produkty a služby spolupracují, najdete v části [monitorování v Microsoft Azure](monitoring-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7d6e5-118">For an overview and understanding of how these different products and services work together, see [Monitoring in Microsoft Azure](monitoring-overview.md).</span></span>

## <a name="monitoring-sources---compute"></a><span data-ttu-id="7d6e5-119">Monitorování zdroje - výpočetní</span><span class="sxs-lookup"><span data-stu-id="7d6e5-119">Monitoring Sources - Compute</span></span>

![Model pro monitorování a Diagnostika pro jiný výpočetní prostředky](./media/monitoring-overview-azure-monitor/Monitoring_Azure_Resources-compute_v6.png)

<span data-ttu-id="7d6e5-121">Zahrnout Hello výpočetní služby</span><span class="sxs-lookup"><span data-stu-id="7d6e5-121">hello Compute services include</span></span> 
- <span data-ttu-id="7d6e5-122">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="7d6e5-122">Cloud Services</span></span> 
- <span data-ttu-id="7d6e5-123">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="7d6e5-123">Virtual Machines</span></span> 
- <span data-ttu-id="7d6e5-124">Sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="7d6e5-124">Virtual Machine scale sets</span></span> 
- <span data-ttu-id="7d6e5-125">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="7d6e5-125">Service Fabric</span></span>

### <a name="application---diagnostics-logs-application-logs-and-metrics"></a><span data-ttu-id="7d6e5-126">Aplikace – diagnostické protokoly, protokoly aplikací a metriky</span><span class="sxs-lookup"><span data-stu-id="7d6e5-126">Application - Diagnostics Logs, Application Logs, and Metrics</span></span>
<span data-ttu-id="7d6e5-127">Aplikace můžete spustit nad hello hostovaného operačního systému v modelu výpočetní hello.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-127">Applications can run on top of hello Guest OS in hello compute model.</span></span> <span data-ttu-id="7d6e5-128">Emitování jejich vlastní sadu protokolů a metriky.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-128">They emit their own set of logs and metrics.</span></span> <span data-ttu-id="7d6e5-129">Azure monitorování spoléhá na hello Azure diagnostics rozšíření (Windows nebo Linux) toocollect většina protokoly a metriky na úrovni aplikace.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-129">Azure Monitor relies on hello Azure diagnostics extension (Windows or Linux) toocollect most application level metrics and logs.</span></span> <span data-ttu-id="7d6e5-130">typy Hello</span><span class="sxs-lookup"><span data-stu-id="7d6e5-130">hello types include</span></span>

* <span data-ttu-id="7d6e5-131">Čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="7d6e5-131">Performance counters</span></span>
* <span data-ttu-id="7d6e5-132">Protokoly aplikací</span><span class="sxs-lookup"><span data-stu-id="7d6e5-132">Application Logs</span></span>
* <span data-ttu-id="7d6e5-133">Protokoly událostí systému Windows</span><span class="sxs-lookup"><span data-stu-id="7d6e5-133">Windows Event Logs</span></span>
* <span data-ttu-id="7d6e5-134">Zdroj události rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="7d6e5-134">.NET Event Source</span></span>
* <span data-ttu-id="7d6e5-135">Protokoly služby IIS</span><span class="sxs-lookup"><span data-stu-id="7d6e5-135">IIS Logs</span></span>
* <span data-ttu-id="7d6e5-136">Manifest na základě trasování událostí pro Windows</span><span class="sxs-lookup"><span data-stu-id="7d6e5-136">Manifest based ETW</span></span>
* <span data-ttu-id="7d6e5-137">Výpisy stavu systému</span><span class="sxs-lookup"><span data-stu-id="7d6e5-137">Crash Dumps</span></span>
* <span data-ttu-id="7d6e5-138">Protokoly chyb zákazníka</span><span class="sxs-lookup"><span data-stu-id="7d6e5-138">Customer Error Logs</span></span>

<span data-ttu-id="7d6e5-139">Bez hello rozšíření diagnostiky jsou k dispozici pouze několik metriky jako využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-139">Without hello diagnostics extension, only a few metrics like CPU usage are available.</span></span> 

### <a name="host-and-guest-vm-metrics"></a><span data-ttu-id="7d6e5-140">Metriky hostitele a hostů virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="7d6e5-140">Host and Guest VM metrics</span></span>
<span data-ttu-id="7d6e5-141">Hello výše uvedených výpočetní prostředky mít vyhrazený hostitelský počítač, hostovaný operační systém, které komunikují s.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-141">hello previously listed compute resources have a dedicated host VM and guest OS they interact with.</span></span> <span data-ttu-id="7d6e5-142">Hello hostitele virtuálních počítačů a hostovaný operační systém jsou ekvivalentní hello kořenové virtuálních počítačů a virtuálním počítači hosta v modelu hypervisoru hello technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-142">hello host VM and guest OS are hello equivalent of root VM and guest VM in hello Hyper-V hypervisor model.</span></span> <span data-ttu-id="7d6e5-143">Můžete shromažďovat metriky na obojí.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-143">You can collect metrics on both.</span></span> <span data-ttu-id="7d6e5-144">Může taky shromažďovat diagnostické protokoly na hello hostovaný operační systém.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-144">You can also collect diagnostics logs on hello guest OS.</span></span>   

### <a name="activity-log"></a><span data-ttu-id="7d6e5-145">Protokol aktivit</span><span class="sxs-lookup"><span data-stu-id="7d6e5-145">Activity Log</span></span>
<span data-ttu-id="7d6e5-146">Hello protokol aktivit (dříve označovaný jako provozní nebo protokoly auditu) můžete vyhledat informace o vašem prostředku jakém ho vidí hello infrastruktury Azure.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-146">You can search hello Activity Log (previously called Operational or Audit Logs) for information about your resource as seen by hello Azure infrastructure.</span></span> <span data-ttu-id="7d6e5-147">Hello protokolu obsahuje informace, jako jsou časy, kdy jsou prostředky vytvořit nebo zničeno.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-147">hello log contains information such as times when resources are created or destroyed.</span></span>  <span data-ttu-id="7d6e5-148">Další informace najdete v tématu [protokol aktivit přehled](monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="7d6e5-148">For more information, see [Overview of Activity Log](monitoring-overview-activity-logs.md).</span></span> 

## <a name="monitoring-sources---everything-else"></a><span data-ttu-id="7d6e5-149">Monitorování zdroje - všem ostatním</span><span class="sxs-lookup"><span data-stu-id="7d6e5-149">Monitoring Sources - everything else</span></span>

![Model pro monitorování a Diagnostika pro výpočetní prostředky](./media/monitoring-overview-azure-monitor/Monitoring_Azure_Resources-non-compute_v6.png)


### <a name="resource---metrics-and-diagnostics-logs"></a><span data-ttu-id="7d6e5-151">Prostředku, metriky a protokolů diagnostiky</span><span class="sxs-lookup"><span data-stu-id="7d6e5-151">Resource - Metrics and Diagnostics Logs</span></span>
<span data-ttu-id="7d6e5-152">Lišit v závislosti na typu prostředku hello shromážditelného metriky a diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-152">Collectable metrics and diagnostics logs vary based on hello resource type.</span></span> <span data-ttu-id="7d6e5-153">Například Web Apps poskytuje statistiky na hello v/v disku a procesoru v procentech.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-153">For example, Web Apps provides statistics on hello Disk IO and Percent CPU.</span></span> <span data-ttu-id="7d6e5-154">Tyto metriky neexistují pro fronty Service Bus, která poskytuje metriky jako propustnost velikost a zprávu fronty.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-154">Those metrics don't exist for a Service Bus queue, which instead provides metrics like queue size and message throughput.</span></span> <span data-ttu-id="7d6e5-155">Seznam shromážditelného metriky pro každý zdroj je k dispozici na [podporované metriky](monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="7d6e5-155">A list of collectable metrics for each resource is available at [supported metrics](monitoring-supported-metrics.md).</span></span> 

### <a name="host-and-guest-vm-metrics"></a><span data-ttu-id="7d6e5-156">Metriky hostitele a hostů virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="7d6e5-156">Host and Guest VM metrics</span></span>
<span data-ttu-id="7d6e5-157">Není nezbytně mapování 1:1 mezi prostředku a na konkrétní hostitele nebo hosta virtuálního počítače, metriky nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-157">There is not necessarily a 1:1 mapping between your resource and a particular Host or Guest VM so metrics are not available.</span></span>

### <a name="activity-log"></a><span data-ttu-id="7d6e5-158">Protokol aktivit</span><span class="sxs-lookup"><span data-stu-id="7d6e5-158">Activity Log</span></span>
<span data-ttu-id="7d6e5-159">Protokol aktivit Hello je hello stejné jako pro výpočetní prostředky.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-159">hello activity log is hello same as for compute resources.</span></span>  

## <a name="uses-for-monitoring-data"></a><span data-ttu-id="7d6e5-160">Používá pro monitorování dat.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-160">Uses for Monitoring Data</span></span>
<span data-ttu-id="7d6e5-161">Jakmile shromáždíte vaše data, můžete provést následující ho v Azure monitorování hello</span><span class="sxs-lookup"><span data-stu-id="7d6e5-161">Once you collect your data, you can do hello following with it in Azure Monitor</span></span>

### <a name="route"></a><span data-ttu-id="7d6e5-162">Trasa</span><span class="sxs-lookup"><span data-stu-id="7d6e5-162">Route</span></span>
<span data-ttu-id="7d6e5-163">Dá Streamovat monitorování tooother umístění dat v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-163">You can stream monitoring data tooother locations in real time.</span></span>

<span data-ttu-id="7d6e5-164">Příklady obsahují:</span><span class="sxs-lookup"><span data-stu-id="7d6e5-164">Examples include:</span></span>

- <span data-ttu-id="7d6e5-165">Odesláno tooApplication statistiky, můžete použít bohatší nástroje vizualizaci a analýzu.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-165">Send tooApplication Insights so you can use its richer visualization and analysis tools.</span></span>
- <span data-ttu-id="7d6e5-166">Odesláno tooEvent rozbočovače, je možné směrovat nástroje toothird výrobců.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-166">Send tooEvent Hubs so you can route toothird-party tools.</span></span> 

### <a name="store-and-archive"></a><span data-ttu-id="7d6e5-167">Úložiště a archivu</span><span class="sxs-lookup"><span data-stu-id="7d6e5-167">Store and Archive</span></span>
<span data-ttu-id="7d6e5-168">Některá data monitorování již uložené a k dispozici v monitorování Azure pro sadu časového intervalu.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-168">Some monitoring data is already stored and available in Azure Monitor for a set amount of time.</span></span> 
- <span data-ttu-id="7d6e5-169">Metriky se uchovávají po dobu 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-169">Metrics are stored for 30 days.</span></span> 
- <span data-ttu-id="7d6e5-170">Položky protokolu aktivity se uchovávají po dobu 90 dnů.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-170">Activity log entries are stored for 90 days.</span></span> 
- <span data-ttu-id="7d6e5-171">Diagnostické protokoly nejsou uložené ve všech.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-171">Diagnostics logs are not stored at all.</span></span> 

<span data-ttu-id="7d6e5-172">Pokud chcete data toostore delší než hello časových období uvedených výše, můžete použít úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-172">If you want toostore data longer than hello time periods listed above, you can use an Azure storage.</span></span> <span data-ttu-id="7d6e5-173">Sledování dat je uložen v váš účet úložiště na základě zásad uchovávání informací, které nastavíte.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-173">Monitoring data is kept in your storage acccount based on a retention policy you set.</span></span> <span data-ttu-id="7d6e5-174">Máte toopay pro hello místo hello trvá dat v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-174">You do have toopay for hello space hello data takes up in Azure storage.</span></span> 

<span data-ttu-id="7d6e5-175">Několik způsobů toouse tato data:</span><span class="sxs-lookup"><span data-stu-id="7d6e5-175">A few ways toouse this data:</span></span>

- <span data-ttu-id="7d6e5-176">Jakmile zapsána, můžete mít další nástroje v rámci nebo mimo Azure číst a zpracovávat.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-176">Once written, you can have other tools within or outside of Azure read it and process it.</span></span>
- <span data-ttu-id="7d6e5-177">Stáhněte si hello data místně pro místní archivaci nebo změňte vaše zásady uchovávání informací v datech tookeep cloudu hello dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-177">You download hello data locally for a local archive or change your retention policy in hello cloud tookeep data for extended periods of time.</span></span>  
- <span data-ttu-id="7d6e5-178">Necháte hello dat v úložišti Azure po neomezenou dobu pro účely archivu.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-178">You leave hello data in Azure storage indefinitely for archive purposes.</span></span> 

### <a name="query"></a><span data-ttu-id="7d6e5-179">Dotaz</span><span class="sxs-lookup"><span data-stu-id="7d6e5-179">Query</span></span>
<span data-ttu-id="7d6e5-180">Můžete použít hello monitorování rozhraní REST API Azure, křížové platformy příkazy rozhraní příkazového řádku (CLI), rutiny prostředí PowerShell nebo sady .NET SDK tooaccess hello hello dat v systému hello nebo úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="7d6e5-180">You can use hello Azure Monitor REST API, cross platform Command-Line Interface (CLI) commands, PowerShell cmdlets, or hello .NET SDK tooaccess hello data in hello system or Azure storage</span></span>

<span data-ttu-id="7d6e5-181">Příklady obsahují:</span><span class="sxs-lookup"><span data-stu-id="7d6e5-181">Examples include:</span></span>

* <span data-ttu-id="7d6e5-182">Získávání dat pro vlastní monitorování aplikací, který jste napsali</span><span class="sxs-lookup"><span data-stu-id="7d6e5-182">Getting data for a custom monitoring application you have written</span></span>
* <span data-ttu-id="7d6e5-183">Vytváření vlastních dotazů a odesílání tuto aplikaci dat tooa třetích stran.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-183">Creating custom queries and sending that data tooa third-party application.</span></span>

### <a name="visualize"></a><span data-ttu-id="7d6e5-184">Vizualizace</span><span class="sxs-lookup"><span data-stu-id="7d6e5-184">Visualize</span></span>
<span data-ttu-id="7d6e5-185">Vizualizace dat monitorování v grafy vám pomůže najít trendy rychlejší než vyhledávání prostřednictvím hello samotná data.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-185">Visualizing your monitoring data in graphics and charts helps you find trends quicker than looking through hello data itself.</span></span>  

<span data-ttu-id="7d6e5-186">Několik metod vizualizace patří:</span><span class="sxs-lookup"><span data-stu-id="7d6e5-186">A few visualization methods include:</span></span>

* <span data-ttu-id="7d6e5-187">Hello použití portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7d6e5-187">Use hello Azure portal</span></span>
* <span data-ttu-id="7d6e5-188">TooAzure data trasy Application Insights</span><span class="sxs-lookup"><span data-stu-id="7d6e5-188">Route data tooAzure Application Insights</span></span>
* <span data-ttu-id="7d6e5-189">TooMicrosoft data trasy PowerBI</span><span class="sxs-lookup"><span data-stu-id="7d6e5-189">Route data tooMicrosoft PowerBI</span></span>
* <span data-ttu-id="7d6e5-190">Trasy hello dat tooa třetích stran vizualizace nástroj pomocí živě Streamovat nebo tak, že nástroj hello číst z archivu v úložišti Azure</span><span class="sxs-lookup"><span data-stu-id="7d6e5-190">Route hello data tooa third-party visualization tool using either live streaming or by having hello tool read from an archive in Azure storage</span></span>


### <a name="automate"></a><span data-ttu-id="7d6e5-191">Automatizace</span><span class="sxs-lookup"><span data-stu-id="7d6e5-191">Automate</span></span>
<span data-ttu-id="7d6e5-192">Můžete použít monitorování upozornění na data tootrigger nebo i v celé procesy.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-192">You can use monitoring data tootrigger alerts or even whole processes.</span></span> <span data-ttu-id="7d6e5-193">Příklady obsahují:</span><span class="sxs-lookup"><span data-stu-id="7d6e5-193">Examples include:</span></span>

* <span data-ttu-id="7d6e5-194">Použijte data tooautoscale výpočetních instancích nahoru nebo dolů podle zatížení aplikace.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-194">Use data tooautoscale compute instances up or down based on application load.</span></span>
* <span data-ttu-id="7d6e5-195">Odesílání e-mailů, když metriky překračuje předem určené prahové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-195">Send emails when a metric crosses a predetermined threshold.</span></span>
* <span data-ttu-id="7d6e5-196">Volání webovou adresu URL (webhooku) tooexecute akce v rámci systému mimo Azure</span><span class="sxs-lookup"><span data-stu-id="7d6e5-196">Call a web URL (webhook) tooexecute an action in a system outside of Azure</span></span>
* <span data-ttu-id="7d6e5-197">Spuštění sady runbook v Azure automation tooperform všechny různé úlohy</span><span class="sxs-lookup"><span data-stu-id="7d6e5-197">Start a runbook in Azure automation tooperform any variety of tasks</span></span>

## <a name="methods-of-accessing-azure-monitor"></a><span data-ttu-id="7d6e5-198">Metody přístupu k Azure monitorování</span><span class="sxs-lookup"><span data-stu-id="7d6e5-198">Methods of accessing Azure Monitor</span></span>
<span data-ttu-id="7d6e5-199">Obecně platí můžete upravit data sledování, směrování a načítání pomocí jedné z následujících metod hello.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-199">In general, you can manipulate data tracking, routing, and retrieval using one of hello following methods.</span></span> <span data-ttu-id="7d6e5-200">Ne všechny metody jsou k dispozici pro všechny akce nebo datové typy.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-200">Not all methods are available for all actions or data types.</span></span>

* [<span data-ttu-id="7d6e5-201">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7d6e5-201">Azure portal</span></span>](https://portal.azure.com)
* [<span data-ttu-id="7d6e5-202">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7d6e5-202">PowerShell</span></span>](insights-powershell-samples.md)  
* [<span data-ttu-id="7d6e5-203">Napříč platformami rozhraní příkazového řádku (CLI)</span><span class="sxs-lookup"><span data-stu-id="7d6e5-203">Cross-platform Command Line Interface (CLI)</span></span>](insights-cli-samples.md)
* [<span data-ttu-id="7d6e5-204">REST API</span><span class="sxs-lookup"><span data-stu-id="7d6e5-204">REST API</span></span>](https://docs.microsoft.com/rest/api/monitor/)
* [<span data-ttu-id="7d6e5-205">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="7d6e5-205">.NET SDK</span></span>](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor)

## <a name="next-steps"></a><span data-ttu-id="7d6e5-206">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7d6e5-206">Next steps</span></span>
<span data-ttu-id="7d6e5-207">Další informace o</span><span class="sxs-lookup"><span data-stu-id="7d6e5-207">Learn more about</span></span>
- <span data-ttu-id="7d6e5-208">Je k dispozici na video s návodem právě Azure monitorování</span><span class="sxs-lookup"><span data-stu-id="7d6e5-208">A video walkthrough of just Azure Monitor is available at</span></span>  
<span data-ttu-id="7d6e5-209">[Začínáme s Azure monitorování](https://channel9.msdn.com/Blogs/Azure-Monitoring/Get-Started-with-Azure-Monitor).</span><span class="sxs-lookup"><span data-stu-id="7d6e5-209">[Get Started with Azure Monitor](https://channel9.msdn.com/Blogs/Azure-Monitoring/Get-Started-with-Azure-Monitor).</span></span> <span data-ttu-id="7d6e5-210">Další video vysvětlením scénář, kde můžete použít Azure monitorování je k dispozici na [monitorování prozkoumat Microsoft Azure a Diagnostika](https://channel9.msdn.com/events/Ignite/2016/BRK2234) a [Azure monitorování v videa z Ignite 2016](https://myignite.microsoft.com/videos/4977)</span><span class="sxs-lookup"><span data-stu-id="7d6e5-210">An additional video explaining a scenario where you can use Azure Monitor is available at [Explore Microsoft Azure monitoring and diagnostics](https://channel9.msdn.com/events/Ignite/2016/BRK2234) and [Azure Monitor in a video from Ignite 2016](https://myignite.microsoft.com/videos/4977)</span></span>
- <span data-ttu-id="7d6e5-211">Spuštění prostřednictvím rozhraní Azure monitorování hello v [Začínáme s Azure monitorování](monitoring-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="7d6e5-211">Run through hello Azure Monitor interface in [Getting Started with Azure Monitor](monitoring-get-started.md)</span></span>
- <span data-ttu-id="7d6e5-212">Nastavit hello [rozšíření diagnostiky Azure](../azure-diagnostics.md) Pokud se pokoušíte toodiagnose problémy v rámci cloudové služby, virtuální počítač škálování virtuálního počítače nebo aplikace Service Fabric sady.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-212">Set up hello [Azure Diagnostics Extensions](../azure-diagnostics.md) if you are attempting toodiagnose problems in your Cloud Service, Virtual Machine, Virtual machine scale sets, or Service Fabric application.</span></span>
- <span data-ttu-id="7d6e5-213">[Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) se pokoušíte toodiagnostic problémy v aplikaci služby webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="7d6e5-213">[Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) if you are trying toodiagnostic problems in your App Service Web app.</span></span>
- <span data-ttu-id="7d6e5-214">[Řešení potíží s Azure Storage](../storage/common/storage-e2e-troubleshooting.md) při použití úložiště objektů BLOB, tabulek a front</span><span class="sxs-lookup"><span data-stu-id="7d6e5-214">[Troubleshooting Azure Storage](../storage/common/storage-e2e-troubleshooting.md) when using Storage Blobs, Tables, or Queues</span></span>
- <span data-ttu-id="7d6e5-215">[Analýza protokolu](https://azure.microsoft.com/documentation/services/log-analytics/) a hello [Operations Management Suite](https://www.microsoft.com/oms/)</span><span class="sxs-lookup"><span data-stu-id="7d6e5-215">[Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) and hello [Operations Management Suite](https://www.microsoft.com/oms/)</span></span>
