---
title: "Přehled Azure monitorování | Microsoft Docs"
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
ms.openlocfilehash: 619a004b9aff99be68988e1f7be3ccad400a8a0e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="overview-of-azure-monitor"></a><span data-ttu-id="60797-104">Přehled Azure monitorování</span><span class="sxs-lookup"><span data-stu-id="60797-104">Overview of Azure Monitor</span></span>
<span data-ttu-id="60797-105">Tento článek obsahuje přehled služby Azure monitorování v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="60797-105">This article provides an overview of the Azure Monitor service in Microsoft Azure.</span></span> <span data-ttu-id="60797-106">Popisuje, co monitorování Azure nepodporuje a poskytuje odkazy na další informace o tom, jak používat Azure monitorování.</span><span class="sxs-lookup"><span data-stu-id="60797-106">It discusses what Azure Monitor does and provides pointers to additional information on how to use Azure Monitor.</span></span>  <span data-ttu-id="60797-107">Pokud upřednostňujete video úvod, najdete v části Další kroky odkazy v dolní části tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="60797-107">If you prefer a video introduction, see Next steps links at the bottom of this article.</span></span> 

## <a name="why-monitor-your-application-or-system"></a><span data-ttu-id="60797-108">Proč monitorování aplikace nebo systému</span><span class="sxs-lookup"><span data-stu-id="60797-108">Why monitor your application or system</span></span>
<span data-ttu-id="60797-109">Cloudové aplikace jsou komplexní s mnoha přesunutí částmi.</span><span class="sxs-lookup"><span data-stu-id="60797-109">Cloud applications are complex with many moving parts.</span></span> <span data-ttu-id="60797-110">Monitorování poskytuje data a ujistěte se, že vaše aplikace zůstává nahoru a spuštěna v dobrém stavu.</span><span class="sxs-lookup"><span data-stu-id="60797-110">Monitoring provides data to ensure that your application stays up and running in a healthy state.</span></span> <span data-ttu-id="60797-111">Také pomáhá stave vypnout potenciální problémy nebo vyřešit potíže s uplynulou těch, které jsou.</span><span class="sxs-lookup"><span data-stu-id="60797-111">It also helps you to stave off potential problems or troubleshoot past ones.</span></span> <span data-ttu-id="60797-112">Kromě toho můžete data monitorování a získáte přehled o hloubkové o vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="60797-112">In addition, you can use monitoring data to gain deep insights about your application.</span></span> <span data-ttu-id="60797-113">Dané znalosti můžete dozvíte, jak zlepšit výkon aplikace nebo udržovatelnosti nebo automatizaci akcí, které by jinak vyžadují ruční zásah.</span><span class="sxs-lookup"><span data-stu-id="60797-113">That knowledge can help you to improve application performance or maintainability, or automate actions that would otherwise require manual intervention.</span></span>


## <a name="azure-monitor-and-microsofts-other-monitoring-products"></a><span data-ttu-id="60797-114">Azure monitorování a Microsoft je další monitorování produkty</span><span class="sxs-lookup"><span data-stu-id="60797-114">Azure Monitor and Microsoft's other monitoring products</span></span>
<span data-ttu-id="60797-115">Monitorování Azure poskytuje základní úroveň infrastruktura metriky a protokoly pro většina služeb v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="60797-115">Azure Monitor provides base level infrastructure metrics and logs for most services in Microsoft Azure.</span></span> <span data-ttu-id="60797-116">Služby Azure, které ještě Neumísťujte svá data do Azure monitorování se pro něj existuje v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="60797-116">Azure services that do not yet put their data into Azure Monitor will put it there in the future.</span></span>

<span data-ttu-id="60797-117">Microsoft dodává další produkty a služby, které poskytují další možnosti monitorování pro vývojáře, DevOps nebo Ops IT, které mají také na místní instalace.</span><span class="sxs-lookup"><span data-stu-id="60797-117">Microsoft ships additional products and services that provide additional monitoring capabilities for developers, DevOps, or IT Ops that also have on-premises installations.</span></span> <span data-ttu-id="60797-118">Přehled a pochopení jak tyto různé produkty a služby spolupracují, najdete v části [monitorování v Microsoft Azure](monitoring-overview.md).</span><span class="sxs-lookup"><span data-stu-id="60797-118">For an overview and understanding of how these different products and services work together, see [Monitoring in Microsoft Azure](monitoring-overview.md).</span></span>

## <a name="monitoring-sources---compute"></a><span data-ttu-id="60797-119">Monitorování zdroje - výpočetní</span><span class="sxs-lookup"><span data-stu-id="60797-119">Monitoring Sources - Compute</span></span>

![Model pro monitorování a Diagnostika pro jiný výpočetní prostředky](./media/monitoring-overview-azure-monitor/Monitoring_Azure_Resources-compute_v6.png)

<span data-ttu-id="60797-121">Zahrnout výpočetní služby</span><span class="sxs-lookup"><span data-stu-id="60797-121">The Compute services include</span></span> 
- <span data-ttu-id="60797-122">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="60797-122">Cloud Services</span></span> 
- <span data-ttu-id="60797-123">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="60797-123">Virtual Machines</span></span> 
- <span data-ttu-id="60797-124">Sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="60797-124">Virtual Machine scale sets</span></span> 
- <span data-ttu-id="60797-125">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="60797-125">Service Fabric</span></span>

### <a name="application---diagnostics-logs-application-logs-and-metrics"></a><span data-ttu-id="60797-126">Aplikace – diagnostické protokoly, protokoly aplikací a metriky</span><span class="sxs-lookup"><span data-stu-id="60797-126">Application - Diagnostics Logs, Application Logs, and Metrics</span></span>
<span data-ttu-id="60797-127">Aplikace můžete spustit nad hostovaného operačního systému v výpočetní modelu.</span><span class="sxs-lookup"><span data-stu-id="60797-127">Applications can run on top of the Guest OS in the compute model.</span></span> <span data-ttu-id="60797-128">Emitování jejich vlastní sadu protokolů a metriky.</span><span class="sxs-lookup"><span data-stu-id="60797-128">They emit their own set of logs and metrics.</span></span> <span data-ttu-id="60797-129">Azure monitorování spoléhá na rozšíření diagnostiky Azure (Windows nebo Linux) ke shromažďování většina protokoly a metriky na úrovni aplikace.</span><span class="sxs-lookup"><span data-stu-id="60797-129">Azure Monitor relies on the Azure diagnostics extension (Windows or Linux) to collect most application level metrics and logs.</span></span> <span data-ttu-id="60797-130">Zahrnout typy</span><span class="sxs-lookup"><span data-stu-id="60797-130">The types include</span></span>

* <span data-ttu-id="60797-131">Čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="60797-131">Performance counters</span></span>
* <span data-ttu-id="60797-132">Protokoly aplikací</span><span class="sxs-lookup"><span data-stu-id="60797-132">Application Logs</span></span>
* <span data-ttu-id="60797-133">Protokoly událostí systému Windows</span><span class="sxs-lookup"><span data-stu-id="60797-133">Windows Event Logs</span></span>
* <span data-ttu-id="60797-134">Zdroj události rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="60797-134">.NET Event Source</span></span>
* <span data-ttu-id="60797-135">Protokoly služby IIS</span><span class="sxs-lookup"><span data-stu-id="60797-135">IIS Logs</span></span>
* <span data-ttu-id="60797-136">Manifest na základě trasování událostí pro Windows</span><span class="sxs-lookup"><span data-stu-id="60797-136">Manifest based ETW</span></span>
* <span data-ttu-id="60797-137">Výpisy stavu systému</span><span class="sxs-lookup"><span data-stu-id="60797-137">Crash Dumps</span></span>
* <span data-ttu-id="60797-138">Protokoly chyb zákazníka</span><span class="sxs-lookup"><span data-stu-id="60797-138">Customer Error Logs</span></span>

<span data-ttu-id="60797-139">Bez rozšíření diagnostiky jsou k dispozici pouze několik metriky jako využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="60797-139">Without the diagnostics extension, only a few metrics like CPU usage are available.</span></span> 

### <a name="host-and-guest-vm-metrics"></a><span data-ttu-id="60797-140">Metriky hostitele a hostů virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="60797-140">Host and Guest VM metrics</span></span>
<span data-ttu-id="60797-141">Výše uvedených výpočetní prostředky mít vyhrazený hostitelský počítač a hostovaného operačního systému komunikují s.</span><span class="sxs-lookup"><span data-stu-id="60797-141">The previously listed compute resources have a dedicated host VM and guest OS they interact with.</span></span> <span data-ttu-id="60797-142">Hostitele virtuálního počítače a hostovaný operační systém jsou ekvivalentní kořenových virtuálních počítačů a hosta virtuálního počítače v hypervisoru modelu technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="60797-142">The host VM and guest OS are the equivalent of root VM and guest VM in the Hyper-V hypervisor model.</span></span> <span data-ttu-id="60797-143">Můžete shromažďovat metriky na obojí.</span><span class="sxs-lookup"><span data-stu-id="60797-143">You can collect metrics on both.</span></span> <span data-ttu-id="60797-144">Může taky shromažďovat diagnostické protokoly na hostovaný operační systém.</span><span class="sxs-lookup"><span data-stu-id="60797-144">You can also collect diagnostics logs on the guest OS.</span></span>   

### <a name="activity-log"></a><span data-ttu-id="60797-145">Protokol aktivit</span><span class="sxs-lookup"><span data-stu-id="60797-145">Activity Log</span></span>
<span data-ttu-id="60797-146">Protokol aktivit (dříve označovaný jako provozní nebo protokoly auditu) můžete vyhledat informace o vašem prostředku, jak je vidět infrastruktura Azure.</span><span class="sxs-lookup"><span data-stu-id="60797-146">You can search the Activity Log (previously called Operational or Audit Logs) for information about your resource as seen by the Azure infrastructure.</span></span> <span data-ttu-id="60797-147">V protokolu obsahuje informace, jako jsou časy, kdy jsou prostředky vytvořit nebo zničeno.</span><span class="sxs-lookup"><span data-stu-id="60797-147">The log contains information such as times when resources are created or destroyed.</span></span>  <span data-ttu-id="60797-148">Další informace najdete v tématu [protokol aktivit přehled](monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="60797-148">For more information, see [Overview of Activity Log](monitoring-overview-activity-logs.md).</span></span> 

## <a name="monitoring-sources---everything-else"></a><span data-ttu-id="60797-149">Monitorování zdroje - všem ostatním</span><span class="sxs-lookup"><span data-stu-id="60797-149">Monitoring Sources - everything else</span></span>

![Model pro monitorování a Diagnostika pro výpočetní prostředky](./media/monitoring-overview-azure-monitor/Monitoring_Azure_Resources-non-compute_v6.png)


### <a name="resource---metrics-and-diagnostics-logs"></a><span data-ttu-id="60797-151">Prostředku, metriky a protokolů diagnostiky</span><span class="sxs-lookup"><span data-stu-id="60797-151">Resource - Metrics and Diagnostics Logs</span></span>
<span data-ttu-id="60797-152">Shromážditelného metriky a diagnostické protokoly lišit v závislosti na typu prostředku.</span><span class="sxs-lookup"><span data-stu-id="60797-152">Collectable metrics and diagnostics logs vary based on the resource type.</span></span> <span data-ttu-id="60797-153">Například Web Apps poskytuje statistiky na v/v disku a procesoru v procentech.</span><span class="sxs-lookup"><span data-stu-id="60797-153">For example, Web Apps provides statistics on the Disk IO and Percent CPU.</span></span> <span data-ttu-id="60797-154">Tyto metriky neexistují pro fronty Service Bus, která poskytuje metriky jako propustnost velikost a zprávu fronty.</span><span class="sxs-lookup"><span data-stu-id="60797-154">Those metrics don't exist for a Service Bus queue, which instead provides metrics like queue size and message throughput.</span></span> <span data-ttu-id="60797-155">Seznam shromážditelného metriky pro každý zdroj je k dispozici na [podporované metriky](monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="60797-155">A list of collectable metrics for each resource is available at [supported metrics](monitoring-supported-metrics.md).</span></span> 

### <a name="host-and-guest-vm-metrics"></a><span data-ttu-id="60797-156">Metriky hostitele a hostů virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="60797-156">Host and Guest VM metrics</span></span>
<span data-ttu-id="60797-157">Není nezbytně mapování 1:1 mezi prostředku a na konkrétní hostitele nebo hosta virtuálního počítače, metriky nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="60797-157">There is not necessarily a 1:1 mapping between your resource and a particular Host or Guest VM so metrics are not available.</span></span>

### <a name="activity-log"></a><span data-ttu-id="60797-158">Protokol aktivit</span><span class="sxs-lookup"><span data-stu-id="60797-158">Activity Log</span></span>
<span data-ttu-id="60797-159">Protokol aktivit je stejné jako výpočetní prostředky.</span><span class="sxs-lookup"><span data-stu-id="60797-159">The activity log is the same as for compute resources.</span></span>  

## <a name="uses-for-monitoring-data"></a><span data-ttu-id="60797-160">Používá pro monitorování dat.</span><span class="sxs-lookup"><span data-stu-id="60797-160">Uses for Monitoring Data</span></span>
<span data-ttu-id="60797-161">Jakmile shromáždíte vaše data, můžete provést na následujícím obrázku se ho v Azure monitorování</span><span class="sxs-lookup"><span data-stu-id="60797-161">Once you collect your data, you can do the following with it in Azure Monitor</span></span>

### <a name="route"></a><span data-ttu-id="60797-162">Trasa</span><span class="sxs-lookup"><span data-stu-id="60797-162">Route</span></span>
<span data-ttu-id="60797-163">Můžete Streamovat monitorování data do jiných umístění v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="60797-163">You can stream monitoring data to other locations in real time.</span></span>

<span data-ttu-id="60797-164">Příklady obsahují:</span><span class="sxs-lookup"><span data-stu-id="60797-164">Examples include:</span></span>

- <span data-ttu-id="60797-165">Odesílání Application Insights, abyste mohli používat širší nástroje vizualizaci a analýzu.</span><span class="sxs-lookup"><span data-stu-id="60797-165">Send to Application Insights so you can use its richer visualization and analysis tools.</span></span>
- <span data-ttu-id="60797-166">Odesílat do centra událostí, takže může směrovat nástroje třetích stran.</span><span class="sxs-lookup"><span data-stu-id="60797-166">Send to Event Hubs so you can route to third-party tools.</span></span> 

### <a name="store-and-archive"></a><span data-ttu-id="60797-167">Úložiště a archivu</span><span class="sxs-lookup"><span data-stu-id="60797-167">Store and Archive</span></span>
<span data-ttu-id="60797-168">Některá data monitorování již uložené a k dispozici v monitorování Azure pro sadu časového intervalu.</span><span class="sxs-lookup"><span data-stu-id="60797-168">Some monitoring data is already stored and available in Azure Monitor for a set amount of time.</span></span> 
- <span data-ttu-id="60797-169">Metriky se uchovávají po dobu 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="60797-169">Metrics are stored for 30 days.</span></span> 
- <span data-ttu-id="60797-170">Položky protokolu aktivity se uchovávají po dobu 90 dnů.</span><span class="sxs-lookup"><span data-stu-id="60797-170">Activity log entries are stored for 90 days.</span></span> 
- <span data-ttu-id="60797-171">Diagnostické protokoly nejsou uložené ve všech.</span><span class="sxs-lookup"><span data-stu-id="60797-171">Diagnostics logs are not stored at all.</span></span> 

<span data-ttu-id="60797-172">Pokud chcete k ukládání dat delší než časová období uvedených výše, můžete použít úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="60797-172">If you want to store data longer than the time periods listed above, you can use an Azure storage.</span></span> <span data-ttu-id="60797-173">Sledování dat je uložen v váš účet úložiště na základě zásad uchovávání informací, které nastavíte.</span><span class="sxs-lookup"><span data-stu-id="60797-173">Monitoring data is kept in your storage acccount based on a retention policy you set.</span></span> <span data-ttu-id="60797-174">Budete muset platit na místo, které zabírají data v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="60797-174">You do have to pay for the space the data takes up in Azure storage.</span></span> 

<span data-ttu-id="60797-175">Několik způsobů, jak používat tato data:</span><span class="sxs-lookup"><span data-stu-id="60797-175">A few ways to use this data:</span></span>

- <span data-ttu-id="60797-176">Jakmile zapsána, můžete mít další nástroje v rámci nebo mimo Azure číst a zpracovávat.</span><span class="sxs-lookup"><span data-stu-id="60797-176">Once written, you can have other tools within or outside of Azure read it and process it.</span></span>
- <span data-ttu-id="60797-177">Stáhněte si data místně pro místní archivaci nebo změňte vaše zásady uchovávání informací v cloudu a ponechat data pro dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="60797-177">You download the data locally for a local archive or change your retention policy in the cloud to keep data for extended periods of time.</span></span>  
- <span data-ttu-id="60797-178">Ponechat data v Azure storage po neomezenou dobu pro účely archivu.</span><span class="sxs-lookup"><span data-stu-id="60797-178">You leave the data in Azure storage indefinitely for archive purposes.</span></span> 

### <a name="query"></a><span data-ttu-id="60797-179">Dotaz</span><span class="sxs-lookup"><span data-stu-id="60797-179">Query</span></span>
<span data-ttu-id="60797-180">Monitorování REST API služby Azure, pro různé platformy příkazy rozhraní příkazového řádku (CLI), rutiny prostředí PowerShell nebo sady .NET SDK můžete použít pro přístup k datům v systému nebo úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="60797-180">You can use the Azure Monitor REST API, cross platform Command-Line Interface (CLI) commands, PowerShell cmdlets, or the .NET SDK to access the data in the system or Azure storage</span></span>

<span data-ttu-id="60797-181">Příklady obsahují:</span><span class="sxs-lookup"><span data-stu-id="60797-181">Examples include:</span></span>

* <span data-ttu-id="60797-182">Získávání dat pro vlastní monitorování aplikací, který jste napsali</span><span class="sxs-lookup"><span data-stu-id="60797-182">Getting data for a custom monitoring application you have written</span></span>
* <span data-ttu-id="60797-183">Vytváření vlastních dotazů a odesílání dat do aplikace třetích stran.</span><span class="sxs-lookup"><span data-stu-id="60797-183">Creating custom queries and sending that data to a third-party application.</span></span>

### <a name="visualize"></a><span data-ttu-id="60797-184">Vizualizace</span><span class="sxs-lookup"><span data-stu-id="60797-184">Visualize</span></span>
<span data-ttu-id="60797-185">Vizualizace dat monitorování v grafy vám pomůže najít trendy rychlejší než vyhledávání prostřednictvím samotná data.</span><span class="sxs-lookup"><span data-stu-id="60797-185">Visualizing your monitoring data in graphics and charts helps you find trends quicker than looking through the data itself.</span></span>  

<span data-ttu-id="60797-186">Několik metod vizualizace patří:</span><span class="sxs-lookup"><span data-stu-id="60797-186">A few visualization methods include:</span></span>

* <span data-ttu-id="60797-187">Použití webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="60797-187">Use the Azure portal</span></span>
* <span data-ttu-id="60797-188">Data trasy do služby Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="60797-188">Route data to Azure Application Insights</span></span>
* <span data-ttu-id="60797-189">Data trasy k Microsoft PowerBI</span><span class="sxs-lookup"><span data-stu-id="60797-189">Route data to Microsoft PowerBI</span></span>
* <span data-ttu-id="60797-190">Směrování dat na nástroj třetí strany vizualizaci pomocí buď živé streamování nebo tak, že nástroj pro čtení z archivu v úložišti Azure</span><span class="sxs-lookup"><span data-stu-id="60797-190">Route the data to a third-party visualization tool using either live streaming or by having the tool read from an archive in Azure storage</span></span>


### <a name="automate"></a><span data-ttu-id="60797-191">Automatizace</span><span class="sxs-lookup"><span data-stu-id="60797-191">Automate</span></span>
<span data-ttu-id="60797-192">Můžete použít data monitorování výstrahy aktivační události nebo dokonce celé procesy.</span><span class="sxs-lookup"><span data-stu-id="60797-192">You can use monitoring data to trigger alerts or even whole processes.</span></span> <span data-ttu-id="60797-193">Příklady obsahují:</span><span class="sxs-lookup"><span data-stu-id="60797-193">Examples include:</span></span>

* <span data-ttu-id="60797-194">Data pro automatické škálování výpočetních instancích použijte nahoru nebo dolů podle zatížení aplikace.</span><span class="sxs-lookup"><span data-stu-id="60797-194">Use data to autoscale compute instances up or down based on application load.</span></span>
* <span data-ttu-id="60797-195">Odesílání e-mailů, když metriky překračuje předem určené prahové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="60797-195">Send emails when a metric crosses a predetermined threshold.</span></span>
* <span data-ttu-id="60797-196">Volání adresu URL webu (webhooku) k provedení akce v rámci systému mimo Azure</span><span class="sxs-lookup"><span data-stu-id="60797-196">Call a web URL (webhook) to execute an action in a system outside of Azure</span></span>
* <span data-ttu-id="60797-197">Spuštění sady runbook ve službě Azure automation k provedení jakékoli řadu úloh</span><span class="sxs-lookup"><span data-stu-id="60797-197">Start a runbook in Azure automation to perform any variety of tasks</span></span>

## <a name="methods-of-accessing-azure-monitor"></a><span data-ttu-id="60797-198">Metody přístupu k Azure monitorování</span><span class="sxs-lookup"><span data-stu-id="60797-198">Methods of accessing Azure Monitor</span></span>
<span data-ttu-id="60797-199">Obecně platí můžete upravit data sledování, směrování a načítání pomocí jedné z následujících metod.</span><span class="sxs-lookup"><span data-stu-id="60797-199">In general, you can manipulate data tracking, routing, and retrieval using one of the following methods.</span></span> <span data-ttu-id="60797-200">Ne všechny metody jsou k dispozici pro všechny akce nebo datové typy.</span><span class="sxs-lookup"><span data-stu-id="60797-200">Not all methods are available for all actions or data types.</span></span>

* [<span data-ttu-id="60797-201">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="60797-201">Azure portal</span></span>](https://portal.azure.com)
* [<span data-ttu-id="60797-202">PowerShell</span><span class="sxs-lookup"><span data-stu-id="60797-202">PowerShell</span></span>](insights-powershell-samples.md)  
* [<span data-ttu-id="60797-203">Napříč platformami rozhraní příkazového řádku (CLI)</span><span class="sxs-lookup"><span data-stu-id="60797-203">Cross-platform Command Line Interface (CLI)</span></span>](insights-cli-samples.md)
* [<span data-ttu-id="60797-204">REST API</span><span class="sxs-lookup"><span data-stu-id="60797-204">REST API</span></span>](https://docs.microsoft.com/rest/api/monitor/)
* [<span data-ttu-id="60797-205">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="60797-205">.NET SDK</span></span>](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor)

## <a name="next-steps"></a><span data-ttu-id="60797-206">Další kroky</span><span class="sxs-lookup"><span data-stu-id="60797-206">Next steps</span></span>
<span data-ttu-id="60797-207">Další informace o</span><span class="sxs-lookup"><span data-stu-id="60797-207">Learn more about</span></span>
- <span data-ttu-id="60797-208">Je k dispozici na video s návodem právě Azure monitorování</span><span class="sxs-lookup"><span data-stu-id="60797-208">A video walkthrough of just Azure Monitor is available at</span></span>  
<span data-ttu-id="60797-209">[Začínáme s Azure monitorování](https://channel9.msdn.com/Blogs/Azure-Monitoring/Get-Started-with-Azure-Monitor).</span><span class="sxs-lookup"><span data-stu-id="60797-209">[Get Started with Azure Monitor](https://channel9.msdn.com/Blogs/Azure-Monitoring/Get-Started-with-Azure-Monitor).</span></span> <span data-ttu-id="60797-210">Další video vysvětlením scénář, kde můžete použít Azure monitorování je k dispozici na [monitorování prozkoumat Microsoft Azure a Diagnostika](https://channel9.msdn.com/events/Ignite/2016/BRK2234) a [Azure monitorování v videa z Ignite 2016](https://myignite.microsoft.com/videos/4977)</span><span class="sxs-lookup"><span data-stu-id="60797-210">An additional video explaining a scenario where you can use Azure Monitor is available at [Explore Microsoft Azure monitoring and diagnostics](https://channel9.msdn.com/events/Ignite/2016/BRK2234) and [Azure Monitor in a video from Ignite 2016](https://myignite.microsoft.com/videos/4977)</span></span>
- <span data-ttu-id="60797-211">Spuštění prostřednictvím rozhraní Azure monitorování v [Začínáme s Azure monitorování](monitoring-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="60797-211">Run through the Azure Monitor interface in [Getting Started with Azure Monitor](monitoring-get-started.md)</span></span>
- <span data-ttu-id="60797-212">Nastavit [rozšíření diagnostiky Azure](../azure-diagnostics.md) Pokud se pokoušíte diagnostikovat problémy s cloudové služby, virtuální počítač škálování virtuálního počítače nebo aplikace Service Fabric sady.</span><span class="sxs-lookup"><span data-stu-id="60797-212">Set up the [Azure Diagnostics Extensions](../azure-diagnostics.md) if you are attempting to diagnose problems in your Cloud Service, Virtual Machine, Virtual machine scale sets, or Service Fabric application.</span></span>
- <span data-ttu-id="60797-213">[Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) Pokud se pokoušíte diagnostiky problémů v aplikaci služby webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="60797-213">[Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) if you are trying to diagnostic problems in your App Service Web app.</span></span>
- <span data-ttu-id="60797-214">[Řešení potíží s Azure Storage](../storage/common/storage-e2e-troubleshooting.md) při použití úložiště objektů BLOB, tabulek a front</span><span class="sxs-lookup"><span data-stu-id="60797-214">[Troubleshooting Azure Storage](../storage/common/storage-e2e-troubleshooting.md) when using Storage Blobs, Tables, or Queues</span></span>
- <span data-ttu-id="60797-215">[Analýza protokolu](https://azure.microsoft.com/documentation/services/log-analytics/) a [služby Operations Management Suite](https://www.microsoft.com/oms/)</span><span class="sxs-lookup"><span data-stu-id="60797-215">[Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) and the [Operations Management Suite](https://www.microsoft.com/oms/)</span></span>
