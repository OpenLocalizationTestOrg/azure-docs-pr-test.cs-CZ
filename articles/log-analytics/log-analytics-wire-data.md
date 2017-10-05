---
title: "Propojit řešení dat v Log Analytics | Microsoft Docs"
description: "Data kabelové sítě je konsolidované sítě a výkon data z počítačů s agenty OMS, včetně nástroje Operations Manager a agenti připojená k systému Windows. Data sítě spolu s daty protokolu ke korelaci data."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: fc3d7127-0baa-4772-858a-5ba995d1519b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: banders
ms.openlocfilehash: eb8ae80f91b9ecad666ab7a2257d99e5669f5b88
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="wire-data-20-preview-solution-in-log-analytics"></a><span data-ttu-id="81192-104">Řešení přenosu dat 2.0 (Preview) v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="81192-104">Wire Data 2.0 (Preview) solution in Log Analytics</span></span>

![Přenosová datový symbol](./media/log-analytics-wire-data/wire-data2-symbol.png)

<span data-ttu-id="81192-106">Data kabelové sítě je konsolidované sítě a výkon data z počítačů s agenty OMS, včetně nástroje Operations Manager, připojení systému Windows a Linux agenty.</span><span class="sxs-lookup"><span data-stu-id="81192-106">Wire data is consolidated network and performance data from computers with OMS agents, including Operations Manager, Windows-connected, and Linux agents.</span></span> <span data-ttu-id="81192-107">Data sítě spolu s další data protokolu ke korelaci data.</span><span class="sxs-lookup"><span data-stu-id="81192-107">Network data is combined with your other log data to help you correlate data.</span></span>

<span data-ttu-id="81192-108">Kromě OMS agentů používá Data kabelové sítě řešení Microsoft Dependency agentů, které můžete nainstalovat na počítače ve vaší infrastruktuře IT.</span><span class="sxs-lookup"><span data-stu-id="81192-108">In addition to OMS agents, the Wire Data solution uses Microsoft Dependency agents that you install on computers in your IT infrastructure.</span></span> <span data-ttu-id="81192-109">Závislost agenti monitorují síťová data odesílaná do a z vašich počítačů pro síť úrovně 2 – 3 [OSI model](https://en.wikipedia.org/wiki/OSI_model), včetně různých protokoly a porty používané.</span><span class="sxs-lookup"><span data-stu-id="81192-109">Dependency Agents monitor network data sent to and from your computers for network levels 2-3 in the [OSI model](https://en.wikipedia.org/wiki/OSI_model), including the various protocols and ports used.</span></span> <span data-ttu-id="81192-110">Data se pak posílají do analýzy protokolů pomocí agentů.</span><span class="sxs-lookup"><span data-stu-id="81192-110">Data is then sent to Log Analytics using agents.</span></span>

> [!NOTE]
> <span data-ttu-id="81192-111">V předchozí verzi řešení přenosu dat nelze přidat do nové pracovní prostory.</span><span class="sxs-lookup"><span data-stu-id="81192-111">You cannot add the previous version of the Wire Data solution to new workspaces.</span></span> <span data-ttu-id="81192-112">Pokud máte v původním řešení Data kabelové sítě povolené, můžete ji použít.</span><span class="sxs-lookup"><span data-stu-id="81192-112">If you have the original Wire Data solution enabled, you can continue to use it.</span></span> <span data-ttu-id="81192-113">Však použít přenosu dat 2.0, je nutné nejprve odebrat původní verze.</span><span class="sxs-lookup"><span data-stu-id="81192-113">However, to use Wire Data 2.0, you must first remove the original version.</span></span>

<span data-ttu-id="81192-114">Ve výchozím nastavení analýzy protokolů shromažďuje data protokolu pro procesoru, paměti, disku a data výkonu sítě z čítačů, které jsou součástí systému Windows.</span><span class="sxs-lookup"><span data-stu-id="81192-114">By default, Log Analytics collects logged data for CPU, memory, disk, and network performance data from counters built into Windows.</span></span> <span data-ttu-id="81192-115">Sítě a jiných shromažďování dat se provádí v v reálném čase pro každého agenta, včetně podsítě a úrovni aplikace protokoly používá pro počítač.</span><span class="sxs-lookup"><span data-stu-id="81192-115">Network and other data collection is done in real-time for each agent, including subnets and application-level protocols being used by the computer.</span></span> <span data-ttu-id="81192-116">Na stránce nastavení na kartě protokoly můžete přidat další čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="81192-116">You can add other performance counters on the Settings page on the Logs tab.</span></span>

<span data-ttu-id="81192-117">Pokud jste použili [sFlow](http://www.sflow.org/) nebo jiný software s [společnosti Cisco NetFlow protokol](http://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html), pak bude povědomé statistiky a data se zobrazí z data kabelové sítě.</span><span class="sxs-lookup"><span data-stu-id="81192-117">If you've used [sFlow](http://www.sflow.org/) or other software with [Cisco's NetFlow protocol](http://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html), then the statistics and data you see from wire data will be familiar to you.</span></span>

<span data-ttu-id="81192-118">Některé typy předdefinované dotazy vyhledávání protokolu patří:</span><span class="sxs-lookup"><span data-stu-id="81192-118">Some of the types of built-in Log search queries include:</span></span>

- <span data-ttu-id="81192-119">Agenti poskytující data kabelové sítě</span><span class="sxs-lookup"><span data-stu-id="81192-119">Agents that provide wire data</span></span>
- <span data-ttu-id="81192-120">IP adresy agentů poskytujících data kabelové sítě</span><span class="sxs-lookup"><span data-stu-id="81192-120">IP address of agents providing wire data</span></span>
- <span data-ttu-id="81192-121">Odchozí komunikace podle IP adresy</span><span class="sxs-lookup"><span data-stu-id="81192-121">Outbound communications by IP addresses</span></span>
- <span data-ttu-id="81192-122">Počet bajtů odeslaných protokoly aplikací</span><span class="sxs-lookup"><span data-stu-id="81192-122">Number of bytes sent by application protocols</span></span>
- <span data-ttu-id="81192-123">Počet bajtů odeslaných služba aplikace</span><span class="sxs-lookup"><span data-stu-id="81192-123">Number of bytes sent by an application service</span></span>
- <span data-ttu-id="81192-124">Počet přijatých bajtů podle různé protokoly</span><span class="sxs-lookup"><span data-stu-id="81192-124">Bytes received by different protocols</span></span>
- <span data-ttu-id="81192-125">Celkový počet bajtů odeslaných a přijatých podle verze protokolu IP</span><span class="sxs-lookup"><span data-stu-id="81192-125">Total bytes sent and received by IP version</span></span>
- <span data-ttu-id="81192-126">Průměrná latence pro připojení, které byly spolehlivě</span><span class="sxs-lookup"><span data-stu-id="81192-126">Average latency for connections that were measured reliably</span></span>
- <span data-ttu-id="81192-127">Počítač, aby iniciovaly nebo přijaly síťový provoz zpracovává</span><span class="sxs-lookup"><span data-stu-id="81192-127">Computer processes that initiated or received network traffic</span></span>
- <span data-ttu-id="81192-128">Objem síťového přenosu pro zpracování</span><span class="sxs-lookup"><span data-stu-id="81192-128">Amount of network traffic for a process</span></span>

<span data-ttu-id="81192-129">Při hledání pomocí data kabelové sítě, můžete filtrovat a data skupiny k zobrazení informací o nejvyšší agentů a horní protokoly.</span><span class="sxs-lookup"><span data-stu-id="81192-129">When you search using wire data, you can filter and group data to view information about the top agents and top protocols.</span></span> <span data-ttu-id="81192-130">Nebo můžete zobrazit, kdy některé počítače (IP adresy MAC adresy) přenášená mezi sebou, jak dlouho a kolik data byla odeslána – v zásadě platí, zobrazit metadata o síťovém provozu, který je na základě hledání.</span><span class="sxs-lookup"><span data-stu-id="81192-130">Or you can view when certain computers (IP addresses/MAC addresses) communicated with each other, for how long, and how much data was sent—basically, you view metadata about network traffic, which is search-based.</span></span>

<span data-ttu-id="81192-131">Ale vzhledem k tomu, že zobrazíte metadata, není nutně užitečné pro odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="81192-131">However, since you're viewing metadata, it's not necessarily useful for in-depth troubleshooting.</span></span> <span data-ttu-id="81192-132">Data kabelové sítě v Log Analytics není úplná sběru dat v síti.</span><span class="sxs-lookup"><span data-stu-id="81192-132">Wire data in Log Analytics is not a full capture of network data.</span></span> <span data-ttu-id="81192-133">Ano rozhraní není určeno pro řešení potíží s hloubky úrovni paketů.</span><span class="sxs-lookup"><span data-stu-id="81192-133">So, it's not intended for deep packet-level troubleshooting.</span></span> <span data-ttu-id="81192-134">Výhodou použití agenta ve srovnání s jinými metodami kolekci, je, že nemusíte instalovat zařízení, konfigurovat síťové přepínače nebo preform složitá konfigurace.</span><span class="sxs-lookup"><span data-stu-id="81192-134">The advantage of using the agent, compared to other collection methods, is that you don't have to install appliances, reconfigure your network switches, or preform complicated configurations.</span></span> <span data-ttu-id="81192-135">Data kabelové sítě je jednoduše založené na agentovi – nainstalujte agenta na počítači a monitorovat vlastní síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="81192-135">Wire data is simply agent-based—you install the agent on a computer and it will monitor its own network traffic.</span></span> <span data-ttu-id="81192-136">Další výhodou je, když chcete monitorovat úlohy běžící v cloudu poskytovatelů nebo poskytovatel hostitelských služeb nebo Microsoft Azure, kde uživatel nemá vlastní vrstvě prostředků infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="81192-136">Another advantage is when you want to monitor workloads running in cloud providers or hosting service provider or Microsoft Azure, where the user doesn't own the fabric layer.</span></span>

## <a name="connected-sources"></a><span data-ttu-id="81192-137">Připojené zdroje</span><span class="sxs-lookup"><span data-stu-id="81192-137">Connected sources</span></span>

<span data-ttu-id="81192-138">Data kabelové sítě získává data od agenta nástroje Microsoft závislostí.</span><span class="sxs-lookup"><span data-stu-id="81192-138">Wire Data gets its data from the Microsoft Dependency Agent.</span></span> <span data-ttu-id="81192-139">Agent závislostí závisí na agenta OMS pro připojení k analýze protokolů.</span><span class="sxs-lookup"><span data-stu-id="81192-139">The Dependency Agent depends on the OMS Agent for its connections to Log Analytics.</span></span> <span data-ttu-id="81192-140">To znamená, že server musí mít agenta OMS nainstalovaný a nakonfigurovaný nejprve a pak nainstalujte agenta závislostí.</span><span class="sxs-lookup"><span data-stu-id="81192-140">This means that a server must have the OMS Agent installed and configured first, and then you install the Dependency Agent.</span></span> <span data-ttu-id="81192-141">Následující tabulka popisuje připojených zdrojů, které podporuje řešení Data kabelové sítě.</span><span class="sxs-lookup"><span data-stu-id="81192-141">The following table describes the connected sources that the Wire Data solution supports.</span></span>

| <span data-ttu-id="81192-142">**Připojené zdroje**</span><span class="sxs-lookup"><span data-stu-id="81192-142">**Connected source**</span></span> | <span data-ttu-id="81192-143">**Podporuje se**</span><span class="sxs-lookup"><span data-stu-id="81192-143">**Supported**</span></span> | <span data-ttu-id="81192-144">**Popis**</span><span class="sxs-lookup"><span data-stu-id="81192-144">**Description**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="81192-145">Agenti systému Windows</span><span class="sxs-lookup"><span data-stu-id="81192-145">Windows agents</span></span> | <span data-ttu-id="81192-146">Ano</span><span class="sxs-lookup"><span data-stu-id="81192-146">Yes</span></span> | <span data-ttu-id="81192-147">Data kabelové sítě analyzuje a shromažďuje data z počítače se systémem Windows agenta.</span><span class="sxs-lookup"><span data-stu-id="81192-147">Wire Data analyzes and collects data from Windows agent computers.</span></span> <br><br> <span data-ttu-id="81192-148">Kromě [agenta OMS](log-analytics-windows-agents.md), Agent služby Microsoft Dependency vyžadují agentů v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="81192-148">In addition to the [OMS Agent](log-analytics-windows-agents.md), Windows agents require the Microsoft Dependency Agent.</span></span> <span data-ttu-id="81192-149">Najdete v článku [podporované operační systémy](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) úplný seznam verzí operačního systému.</span><span class="sxs-lookup"><span data-stu-id="81192-149">See the [supported operating systems](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) for a complete list of operating system versions.</span></span> |
| <span data-ttu-id="81192-150">Agenti systému Linux</span><span class="sxs-lookup"><span data-stu-id="81192-150">Linux agents</span></span> | <span data-ttu-id="81192-151">Ano</span><span class="sxs-lookup"><span data-stu-id="81192-151">Yes</span></span> | <span data-ttu-id="81192-152">Data kabelové sítě analyzuje a shromažďuje data z počítače se systémem Linux agent.</span><span class="sxs-lookup"><span data-stu-id="81192-152">Wire Data analyzes and collects data from Linux agent computers.</span></span><br><br> <span data-ttu-id="81192-153">Kromě [agenta OMS](log-analytics-linux-agents.md), agenty Linux vyžadují Microsoft Agent závislostí.</span><span class="sxs-lookup"><span data-stu-id="81192-153">In addition to the [OMS Agent](log-analytics-linux-agents.md), Linux agents require the Microsoft Dependency Agent.</span></span> <span data-ttu-id="81192-154">Najdete v článku [podporované operační systémy](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) úplný seznam verzí operačního systému.</span><span class="sxs-lookup"><span data-stu-id="81192-154">See the [supported operating systems](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) for a complete list of operating system versions.</span></span> |
| <span data-ttu-id="81192-155">Skupina pro správu nástroje System Center Operations Manager</span><span class="sxs-lookup"><span data-stu-id="81192-155">System Center Operations Manager management group</span></span> | <span data-ttu-id="81192-156">Ano</span><span class="sxs-lookup"><span data-stu-id="81192-156">Yes</span></span> | <span data-ttu-id="81192-157">Analyzuje Data kabelové sítě a shromažďuje data z agentů systému Windows a Linux v připojeného [skupiny pro správu System Center Operations Manager](log-analytics-om-agents.md).</span><span class="sxs-lookup"><span data-stu-id="81192-157">Wire Data analyzes and collects data from Windows and Linux agents in a connected [System Center Operations Manager management group](log-analytics-om-agents.md).</span></span> <br><br> <span data-ttu-id="81192-158">Je nutné přímé připojení z počítače agenta System Center Operations Manager k analýze protokolů.</span><span class="sxs-lookup"><span data-stu-id="81192-158">A direct connection from the System Center Operations Manager agent computer to Log Analytics is required.</span></span> <span data-ttu-id="81192-159">K analýze protokolů se předají data ze skupiny pro správu.</span><span class="sxs-lookup"><span data-stu-id="81192-159">Data is forwarded from the management group to Log Analytics.</span></span> |
| <span data-ttu-id="81192-160">Účet služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="81192-160">Azure storage account</span></span> | <span data-ttu-id="81192-161">Ne</span><span class="sxs-lookup"><span data-stu-id="81192-161">No</span></span> | <span data-ttu-id="81192-162">Data kabelové sítě shromažďuje data z počítačů agentů, takže není žádná data z něj shromažďovat ze služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="81192-162">Wire Data collects data from agent computers, so there is no data from it to collect from Azure Storage.</span></span> |

<span data-ttu-id="81192-163">V systému Windows Microsoft Monitoring Agent (MMA) použít System Center Operations Manager a analýzy protokolů shromažďovat a odesílat data.</span><span class="sxs-lookup"><span data-stu-id="81192-163">On Windows, the Microsoft Monitoring Agent (MMA) is used by both System Center Operations Manager and Log Analytics to gather and send data.</span></span> <span data-ttu-id="81192-164">V závislosti na kontextu se nazývá agenta agenta System Center Operations Manager, OMS Agent, Agent analýzy protokolů, MMA nebo přímé agenta.</span><span class="sxs-lookup"><span data-stu-id="81192-164">Depending on the context, the agent is called the System Center Operations Manager Agent, OMS Agent, Log Analytics Agent, MMA, or Direct Agent.</span></span> <span data-ttu-id="81192-165">System Center Operations Manager a analýzy protokolů poskytují mírně různé verze MMA.</span><span class="sxs-lookup"><span data-stu-id="81192-165">System Center Operations Manager and Log Analytics provide slightly different versions of the MMA.</span></span> <span data-ttu-id="81192-166">Tyto verze lze každou sestavu System Center Operations Manager, analýzy protokolů nebo do obou.</span><span class="sxs-lookup"><span data-stu-id="81192-166">These versions can each report to System Center Operations Manager, to Log Analytics, or to both.</span></span>

<span data-ttu-id="81192-167">V systému Linux OMS agenta pro Linux shromažďuje a odesílá data k analýze protokolů.</span><span class="sxs-lookup"><span data-stu-id="81192-167">On Linux, the OMS Agent for Linux gathers and sends data to Log Analytics.</span></span> <span data-ttu-id="81192-168">Data kabelové sítě můžete použít na serverech s agenty přímé OMS nebo na servery, které jsou připojené k analýze protokolů prostřednictvím skupin pro správu System Center Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="81192-168">You can use Wire Data on servers with OMS Direct Agents or on servers that are attached to Log Analytics via System Center Operations Manager management groups.</span></span>

<span data-ttu-id="81192-169">V tomto článku, odkazuje na všechny agenty, zda Linux nebo Windows, ať už připojené ke skupině pro správu System Center Operations Manager nebo přímo k Log Analytics se říká _agenta OMS_.</span><span class="sxs-lookup"><span data-stu-id="81192-169">In this article, references to all agents, whether Linux or Windows, whether connected to a System Center Operations Manager management group or directly to Log Analytics are termed the _OMS agent_.</span></span> <span data-ttu-id="81192-170">Název konkrétní nasazení agenta použijeme jenom v případě, že je potřeba pro kontext.</span><span class="sxs-lookup"><span data-stu-id="81192-170">We'll use the specific deployment name of the agent only if it's needed for context.</span></span>

<span data-ttu-id="81192-171">Agent závislostí nepřenáší samotná data a nevyžaduje žádné změny brány firewall nebo porty.</span><span class="sxs-lookup"><span data-stu-id="81192-171">The Dependency Agent does not transmit any data itself, and it does not require any changes to firewalls or ports.</span></span> <span data-ttu-id="81192-172">Data v Data kabelové sítě vždy přenášená agentem OMS k analýze protokolů, buď přímo nebo pomocí brány OMS.</span><span class="sxs-lookup"><span data-stu-id="81192-172">The data in Wire Data is always transmitted by the OMS agent to Log Analytics, either directly or using the OMS Gateway.</span></span>

![diagram agenta](./media/log-analytics-wire-data/agents.png)

<span data-ttu-id="81192-174">Pokud jste uživatele System Center Operations Manager, který má skupinu pro správu připojené k analýze protokolů:</span><span class="sxs-lookup"><span data-stu-id="81192-174">If you are a System Center Operations Manager user with a management group connected to Log Analytics:</span></span>

- <span data-ttu-id="81192-175">Pokud agenty nástroje System Center Operations Manager můžete přístup k Internetu, aby se připojení k analýze protokolů, není nutná žádná další konfigurace.</span><span class="sxs-lookup"><span data-stu-id="81192-175">No additional configuration is required when your System Center Operations Manager agents can access the Internet to connect to Log Analytics.</span></span>
- <span data-ttu-id="81192-176">Budete muset nakonfigurovat bránu OMS fungovat s nástrojem System Center Operations Manager, když agenty nástroje System Center Operations Manager nelze získat přístup k analýze protokolů přes Internet.</span><span class="sxs-lookup"><span data-stu-id="81192-176">You need to configure the OMS Gateway to work with System Center Operations Manager when your System Center Operations Manager agents cannot access Log Analytics over the Internet.</span></span>

<span data-ttu-id="81192-177">Pokud používáte přímé Agent, musíte nakonfigurovat agenta OMS připojit se k analýze protokolů nebo k bráně OMS.</span><span class="sxs-lookup"><span data-stu-id="81192-177">If you are using the Direct Agent, you need to configure the OMS agent itself to connect to Log Analytics or to your OMS Gateway.</span></span> <span data-ttu-id="81192-178">Si můžete stáhnout z brány OMS [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=52666).</span><span class="sxs-lookup"><span data-stu-id="81192-178">You can download the OMS Gateway from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=52666).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="81192-179">Požadavky</span><span class="sxs-lookup"><span data-stu-id="81192-179">Prerequisites</span></span>

- <span data-ttu-id="81192-180">Vyžaduje [přehledy a analýzy](https://www.microsoft.com/cloud-platform/operations-management-suite-pricing) nabídka řešení.</span><span class="sxs-lookup"><span data-stu-id="81192-180">Requires the [Insight and Analytics](https://www.microsoft.com/cloud-platform/operations-management-suite-pricing) solution offer.</span></span>
- <span data-ttu-id="81192-181">Pokud používáte předchozí verzi řešení Data kabelové sítě, je nutné ji odebrat.</span><span class="sxs-lookup"><span data-stu-id="81192-181">If you're using the previous version of the Wire Data solution, you must first remove it.</span></span> <span data-ttu-id="81192-182">Všechna data zaznamenaná v původní Data kabelové sítě řešení je ale stále k dispozici v přenosu dat 2.0 a hledání protokolů.</span><span class="sxs-lookup"><span data-stu-id="81192-182">However, all data captured through the original Wire Data solution is still available in Wire Data 2.0 and log search.</span></span>
- <span data-ttu-id="81192-183">Pro instalaci nebo odinstalaci agenta závislosti jsou vyžadována oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="81192-183">Administrator privileges are required to install or uninstall the Dependency Agent.</span></span>
- <span data-ttu-id="81192-184">Závislost agenta musí být nainstalován na počítači s 64bitový operační systém.</span><span class="sxs-lookup"><span data-stu-id="81192-184">The Dependency Agent must be installed on a computer with a 64-bit operating system.</span></span>

### <a name="operating-systems"></a><span data-ttu-id="81192-185">Operační systémy</span><span class="sxs-lookup"><span data-stu-id="81192-185">Operating systems</span></span>

<span data-ttu-id="81192-186">Následující části uvádějí podporované operační systémy pro agenta závislostí.</span><span class="sxs-lookup"><span data-stu-id="81192-186">The following sections list the supported operating systems for the Dependency Agent.</span></span> <span data-ttu-id="81192-187">Data kabelové sítě nepodporuje 32bitové architektury pro všechny operační systémy.</span><span class="sxs-lookup"><span data-stu-id="81192-187">Wire Data doesn't support 32-bit architectures for any operating system.</span></span>

#### <a name="windows-server"></a><span data-ttu-id="81192-188">Windows Server</span><span class="sxs-lookup"><span data-stu-id="81192-188">Windows Server</span></span>

- <span data-ttu-id="81192-189">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="81192-189">Windows Server 2016</span></span>
- <span data-ttu-id="81192-190">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="81192-190">Windows Server 2012 R2</span></span>
- <span data-ttu-id="81192-191">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="81192-191">Windows Server 2012</span></span>
- <span data-ttu-id="81192-192">Windows Server 2008 R2 SP1</span><span class="sxs-lookup"><span data-stu-id="81192-192">Windows Server 2008 R2 SP1</span></span>

#### <a name="windows-desktop"></a><span data-ttu-id="81192-193">Plocha Windows</span><span class="sxs-lookup"><span data-stu-id="81192-193">Windows desktop</span></span>

- <span data-ttu-id="81192-194">Windows 10</span><span class="sxs-lookup"><span data-stu-id="81192-194">Windows 10</span></span>
- <span data-ttu-id="81192-195">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="81192-195">Windows 8.1</span></span>
- <span data-ttu-id="81192-196">Windows 8</span><span class="sxs-lookup"><span data-stu-id="81192-196">Windows 8</span></span>
- <span data-ttu-id="81192-197">Windows 7</span><span class="sxs-lookup"><span data-stu-id="81192-197">Windows 7</span></span>

#### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a><span data-ttu-id="81192-198">Red Hat Enterprise Linux, CentOS Linux a Oracle Linux (s RHEL jádra)</span><span class="sxs-lookup"><span data-stu-id="81192-198">Red Hat Enterprise Linux, CentOS Linux, and Oracle Linux (with RHEL Kernel)</span></span>

- <span data-ttu-id="81192-199">Podporovány jsou pouze výchozí a verze SMP Linux jádra.</span><span class="sxs-lookup"><span data-stu-id="81192-199">Only default and SMP Linux kernel releases are supported.</span></span>
- <span data-ttu-id="81192-200">Nestandardní jádra uvolní, například PAE a Xen, nejsou podporovány pro všechny distribuci systému Linux.</span><span class="sxs-lookup"><span data-stu-id="81192-200">Nonstandard kernel releases, such as PAE and Xen, are not supported for any Linux distribution.</span></span> <span data-ttu-id="81192-201">Například systém s řetězec verze _2.6.16.21-0.8-xen_ není podporován.</span><span class="sxs-lookup"><span data-stu-id="81192-201">For example, a system with the release string of _2.6.16.21-0.8-xen_ is not supported.</span></span>
- <span data-ttu-id="81192-202">Vlastní jádra, včetně opakovaných kompilací standardní jádra, nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="81192-202">Custom kernels, including recompiles of standard kernels, are not supported.</span></span>
- <span data-ttu-id="81192-203">CentOSPlus jádra není podporována.</span><span class="sxs-lookup"><span data-stu-id="81192-203">CentOSPlus kernel is not supported.</span></span>
- <span data-ttu-id="81192-204">Oracle nedělitelné Enterprise jádra (UEK) je popsaná v další části tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="81192-204">Oracle Unbreakable Enterprise Kernel (UEK) is covered in a later section of this article.</span></span>

#### <a name="red-hat-linux-7"></a><span data-ttu-id="81192-205">Red Hat Linux 7</span><span class="sxs-lookup"><span data-stu-id="81192-205">Red Hat Linux 7</span></span>

| <span data-ttu-id="81192-206">**Verze operačního systému**</span><span class="sxs-lookup"><span data-stu-id="81192-206">**OS version**</span></span> | <span data-ttu-id="81192-207">**Verze jádra**</span><span class="sxs-lookup"><span data-stu-id="81192-207">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="81192-208">7.0</span><span class="sxs-lookup"><span data-stu-id="81192-208">7.0</span></span> | <span data-ttu-id="81192-209">3.10.0-123</span><span class="sxs-lookup"><span data-stu-id="81192-209">3.10.0-123</span></span> |
| <span data-ttu-id="81192-210">7.1</span><span class="sxs-lookup"><span data-stu-id="81192-210">7.1</span></span> | <span data-ttu-id="81192-211">3.10.0-229</span><span class="sxs-lookup"><span data-stu-id="81192-211">3.10.0-229</span></span> |
| <span data-ttu-id="81192-212">7.2</span><span class="sxs-lookup"><span data-stu-id="81192-212">7.2</span></span> | <span data-ttu-id="81192-213">3.10.0-327</span><span class="sxs-lookup"><span data-stu-id="81192-213">3.10.0-327</span></span> |
| <span data-ttu-id="81192-214">7.3</span><span class="sxs-lookup"><span data-stu-id="81192-214">7.3</span></span> | <span data-ttu-id="81192-215">3.10.0-514</span><span class="sxs-lookup"><span data-stu-id="81192-215">3.10.0-514</span></span> |

#### <a name="red-hat-linux-6"></a><span data-ttu-id="81192-216">Red Hat Linux 6</span><span class="sxs-lookup"><span data-stu-id="81192-216">Red Hat Linux 6</span></span>

| <span data-ttu-id="81192-217">**Verze operačního systému**</span><span class="sxs-lookup"><span data-stu-id="81192-217">**OS version**</span></span> | <span data-ttu-id="81192-218">**Verze jádra**</span><span class="sxs-lookup"><span data-stu-id="81192-218">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="81192-219">6.0</span><span class="sxs-lookup"><span data-stu-id="81192-219">6.0</span></span> | <span data-ttu-id="81192-220">2.6.32-71</span><span class="sxs-lookup"><span data-stu-id="81192-220">2.6.32-71</span></span> |
| <span data-ttu-id="81192-221">6.1</span><span class="sxs-lookup"><span data-stu-id="81192-221">6.1</span></span> | <span data-ttu-id="81192-222">2.6.32-131</span><span class="sxs-lookup"><span data-stu-id="81192-222">2.6.32-131</span></span> |
| <span data-ttu-id="81192-223">6.2</span><span class="sxs-lookup"><span data-stu-id="81192-223">6.2</span></span> | <span data-ttu-id="81192-224">2.6.32-220</span><span class="sxs-lookup"><span data-stu-id="81192-224">2.6.32-220</span></span> |
| <span data-ttu-id="81192-225">6.3</span><span class="sxs-lookup"><span data-stu-id="81192-225">6.3</span></span> | <span data-ttu-id="81192-226">2.6.32-279</span><span class="sxs-lookup"><span data-stu-id="81192-226">2.6.32-279</span></span> |
| <span data-ttu-id="81192-227">6.4</span><span class="sxs-lookup"><span data-stu-id="81192-227">6.4</span></span> | <span data-ttu-id="81192-228">2.6.32-358</span><span class="sxs-lookup"><span data-stu-id="81192-228">2.6.32-358</span></span> |
| <span data-ttu-id="81192-229">6.5</span><span class="sxs-lookup"><span data-stu-id="81192-229">6.5</span></span> | <span data-ttu-id="81192-230">2.6.32-431</span><span class="sxs-lookup"><span data-stu-id="81192-230">2.6.32-431</span></span> |
| <span data-ttu-id="81192-231">6.6</span><span class="sxs-lookup"><span data-stu-id="81192-231">6.6</span></span> | <span data-ttu-id="81192-232">2.6.32-504</span><span class="sxs-lookup"><span data-stu-id="81192-232">2.6.32-504</span></span> |
| <span data-ttu-id="81192-233">6.7</span><span class="sxs-lookup"><span data-stu-id="81192-233">6.7</span></span> | <span data-ttu-id="81192-234">2.6.32-573</span><span class="sxs-lookup"><span data-stu-id="81192-234">2.6.32-573</span></span> |
| <span data-ttu-id="81192-235">6.8</span><span class="sxs-lookup"><span data-stu-id="81192-235">6.8</span></span> | <span data-ttu-id="81192-236">2.6.32-642</span><span class="sxs-lookup"><span data-stu-id="81192-236">2.6.32-642</span></span> |

#### <a name="red-hat-linux-5"></a><span data-ttu-id="81192-237">Red Hat Linux 5</span><span class="sxs-lookup"><span data-stu-id="81192-237">Red Hat Linux 5</span></span>

| <span data-ttu-id="81192-238">**Verze operačního systému**</span><span class="sxs-lookup"><span data-stu-id="81192-238">**OS version**</span></span> | <span data-ttu-id="81192-239">**Verze jádra**</span><span class="sxs-lookup"><span data-stu-id="81192-239">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="81192-240">5.8</span><span class="sxs-lookup"><span data-stu-id="81192-240">5.8</span></span> | <span data-ttu-id="81192-241">2.6.18-308</span><span class="sxs-lookup"><span data-stu-id="81192-241">2.6.18-308</span></span> |
| <span data-ttu-id="81192-242">5.9</span><span class="sxs-lookup"><span data-stu-id="81192-242">5.9</span></span> | <span data-ttu-id="81192-243">2.6.18-348</span><span class="sxs-lookup"><span data-stu-id="81192-243">2.6.18-348</span></span> |
| <span data-ttu-id="81192-244">5.10</span><span class="sxs-lookup"><span data-stu-id="81192-244">5.10</span></span> | <span data-ttu-id="81192-245">2.6.18-371</span><span class="sxs-lookup"><span data-stu-id="81192-245">2.6.18-371</span></span> |
| <span data-ttu-id="81192-246">5.11</span><span class="sxs-lookup"><span data-stu-id="81192-246">5.11</span></span> | <span data-ttu-id="81192-247">2.6.18-398</span><span class="sxs-lookup"><span data-stu-id="81192-247">2.6.18-398</span></span> <br> <span data-ttu-id="81192-248">2.6.18-400</span><span class="sxs-lookup"><span data-stu-id="81192-248">2.6.18-400</span></span> <br><span data-ttu-id="81192-249">2.6.18-402</span><span class="sxs-lookup"><span data-stu-id="81192-249">2.6.18-402</span></span> <br><span data-ttu-id="81192-250">2.6.18-404</span><span class="sxs-lookup"><span data-stu-id="81192-250">2.6.18-404</span></span> <br><span data-ttu-id="81192-251">2.6.18-406</span><span class="sxs-lookup"><span data-stu-id="81192-251">2.6.18-406</span></span> <br> <span data-ttu-id="81192-252">2.6.18-407</span><span class="sxs-lookup"><span data-stu-id="81192-252">2.6.18-407</span></span> <br> <span data-ttu-id="81192-253">2.6.18-408</span><span class="sxs-lookup"><span data-stu-id="81192-253">2.6.18-408</span></span> <br> <span data-ttu-id="81192-254">2.6.18-409</span><span class="sxs-lookup"><span data-stu-id="81192-254">2.6.18-409</span></span> <br> <span data-ttu-id="81192-255">2.6.18-410</span><span class="sxs-lookup"><span data-stu-id="81192-255">2.6.18-410</span></span> <br> <span data-ttu-id="81192-256">2.6.18-411</span><span class="sxs-lookup"><span data-stu-id="81192-256">2.6.18-411</span></span> <br> <span data-ttu-id="81192-257">2.6.18-412</span><span class="sxs-lookup"><span data-stu-id="81192-257">2.6.18-412</span></span> <br> <span data-ttu-id="81192-258">2.6.18-416</span><span class="sxs-lookup"><span data-stu-id="81192-258">2.6.18-416</span></span> <br> <span data-ttu-id="81192-259">2.6.18-417</span><span class="sxs-lookup"><span data-stu-id="81192-259">2.6.18-417</span></span> <br> <span data-ttu-id="81192-260">2.6.18-419</span><span class="sxs-lookup"><span data-stu-id="81192-260">2.6.18-419</span></span> |

#### <a name="oracle-enterprise-linux-with-unbreakable-enterprise-kernel"></a><span data-ttu-id="81192-261">Oracle Linux Enterprise s nedělitelné Enterprise jádra</span><span class="sxs-lookup"><span data-stu-id="81192-261">Oracle Enterprise Linux with Unbreakable Enterprise Kernel</span></span>

#### <a name="oracle-linux-6"></a><span data-ttu-id="81192-262">Oracle Linux 6</span><span class="sxs-lookup"><span data-stu-id="81192-262">Oracle Linux 6</span></span>

| <span data-ttu-id="81192-263">**Verze operačního systému**</span><span class="sxs-lookup"><span data-stu-id="81192-263">**OS version**</span></span> | <span data-ttu-id="81192-264">**Verze jádra**</span><span class="sxs-lookup"><span data-stu-id="81192-264">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="81192-265">6.2</span><span class="sxs-lookup"><span data-stu-id="81192-265">6.2</span></span> | <span data-ttu-id="81192-266">Oracle 2.6.32-300 (UEK R1)</span><span class="sxs-lookup"><span data-stu-id="81192-266">Oracle 2.6.32-300 (UEK R1)</span></span> |
| <span data-ttu-id="81192-267">6.3</span><span class="sxs-lookup"><span data-stu-id="81192-267">6.3</span></span> | <span data-ttu-id="81192-268">Oracle 2.6.39-200 (UEK R2)</span><span class="sxs-lookup"><span data-stu-id="81192-268">Oracle 2.6.39-200 (UEK R2)</span></span> |
| <span data-ttu-id="81192-269">6.4</span><span class="sxs-lookup"><span data-stu-id="81192-269">6.4</span></span> | <span data-ttu-id="81192-270">Oracle 2.6.39-400 (UEK R2)</span><span class="sxs-lookup"><span data-stu-id="81192-270">Oracle 2.6.39-400 (UEK R2)</span></span> |
| <span data-ttu-id="81192-271">6.5</span><span class="sxs-lookup"><span data-stu-id="81192-271">6.5</span></span> | <span data-ttu-id="81192-272">Oracle 2.6.39-400 (UEK R2 i386)</span><span class="sxs-lookup"><span data-stu-id="81192-272">Oracle 2.6.39-400 (UEK R2 i386)</span></span> |
| <span data-ttu-id="81192-273">6.6</span><span class="sxs-lookup"><span data-stu-id="81192-273">6.6</span></span> | <span data-ttu-id="81192-274">Oracle 2.6.39-400 (UEK R2 i386)</span><span class="sxs-lookup"><span data-stu-id="81192-274">Oracle 2.6.39-400 (UEK R2 i386)</span></span> |

#### <a name="oracle-linux-5"></a><span data-ttu-id="81192-275">Oracle Linux 5</span><span class="sxs-lookup"><span data-stu-id="81192-275">Oracle Linux 5</span></span>

| <span data-ttu-id="81192-276">**Verze operačního systému**</span><span class="sxs-lookup"><span data-stu-id="81192-276">**OS version**</span></span> | <span data-ttu-id="81192-277">**Verze jádra**</span><span class="sxs-lookup"><span data-stu-id="81192-277">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="81192-278">5.8</span><span class="sxs-lookup"><span data-stu-id="81192-278">5.8</span></span> | <span data-ttu-id="81192-279">Oracle 2.6.32-300 (UEK R1)</span><span class="sxs-lookup"><span data-stu-id="81192-279">Oracle 2.6.32-300 (UEK R1)</span></span> |
| <span data-ttu-id="81192-280">5.9</span><span class="sxs-lookup"><span data-stu-id="81192-280">5.9</span></span> | <span data-ttu-id="81192-281">Oracle 2.6.39-300 (UEK R2)</span><span class="sxs-lookup"><span data-stu-id="81192-281">Oracle 2.6.39-300 (UEK R2)</span></span> |
| <span data-ttu-id="81192-282">5.10</span><span class="sxs-lookup"><span data-stu-id="81192-282">5.10</span></span> | <span data-ttu-id="81192-283">Oracle 2.6.39-400 (UEK R2)</span><span class="sxs-lookup"><span data-stu-id="81192-283">Oracle 2.6.39-400 (UEK R2)</span></span> |
| <span data-ttu-id="81192-284">5.11</span><span class="sxs-lookup"><span data-stu-id="81192-284">5.11</span></span> | <span data-ttu-id="81192-285">Oracle 2.6.39-400 (UEK R2)</span><span class="sxs-lookup"><span data-stu-id="81192-285">Oracle 2.6.39-400 (UEK R2)</span></span> |

#### <a name="suse-linux-enterprise-server"></a><span data-ttu-id="81192-286">SUSE Linux Enterprise Server</span><span class="sxs-lookup"><span data-stu-id="81192-286">SUSE Linux Enterprise Server</span></span>

#### <a name="suse-linux-11"></a><span data-ttu-id="81192-287">SUSE Linux 11</span><span class="sxs-lookup"><span data-stu-id="81192-287">SUSE Linux 11</span></span>

| <span data-ttu-id="81192-288">**Verze operačního systému**</span><span class="sxs-lookup"><span data-stu-id="81192-288">**OS version**</span></span> | <span data-ttu-id="81192-289">**Verze jádra**</span><span class="sxs-lookup"><span data-stu-id="81192-289">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="81192-290">11</span><span class="sxs-lookup"><span data-stu-id="81192-290">11</span></span> | <span data-ttu-id="81192-291">2.6.27</span><span class="sxs-lookup"><span data-stu-id="81192-291">2.6.27</span></span> |
| <span data-ttu-id="81192-292">11 SP1</span><span class="sxs-lookup"><span data-stu-id="81192-292">11 SP1</span></span> | <span data-ttu-id="81192-293">2.6.32</span><span class="sxs-lookup"><span data-stu-id="81192-293">2.6.32</span></span> |
| <span data-ttu-id="81192-294">11 SP2</span><span class="sxs-lookup"><span data-stu-id="81192-294">11 SP2</span></span> | <span data-ttu-id="81192-295">3.0.13</span><span class="sxs-lookup"><span data-stu-id="81192-295">3.0.13</span></span> |
| <span data-ttu-id="81192-296">11 SP3</span><span class="sxs-lookup"><span data-stu-id="81192-296">11 SP3</span></span> | <span data-ttu-id="81192-297">3.0.76</span><span class="sxs-lookup"><span data-stu-id="81192-297">3.0.76</span></span> |
| <span data-ttu-id="81192-298">11 SP4</span><span class="sxs-lookup"><span data-stu-id="81192-298">11 SP4</span></span> | <span data-ttu-id="81192-299">3.0.101</span><span class="sxs-lookup"><span data-stu-id="81192-299">3.0.101</span></span> |

#### <a name="suse-linux-10"></a><span data-ttu-id="81192-300">SUSE Linux 10</span><span class="sxs-lookup"><span data-stu-id="81192-300">SUSE Linux 10</span></span>

| <span data-ttu-id="81192-301">**Verze operačního systému**</span><span class="sxs-lookup"><span data-stu-id="81192-301">**OS version**</span></span> | <span data-ttu-id="81192-302">**Verze jádra**</span><span class="sxs-lookup"><span data-stu-id="81192-302">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="81192-303">10 SP4</span><span class="sxs-lookup"><span data-stu-id="81192-303">10 SP4</span></span> | <span data-ttu-id="81192-304">2.6.16.60</span><span class="sxs-lookup"><span data-stu-id="81192-304">2.6.16.60</span></span> |

#### <a name="dependency-agent-downloads"></a><span data-ttu-id="81192-305">Agent služby Dependency soubory ke stažení</span><span class="sxs-lookup"><span data-stu-id="81192-305">Dependency Agent downloads</span></span>

| <span data-ttu-id="81192-306">**File**</span><span class="sxs-lookup"><span data-stu-id="81192-306">**File**</span></span> | <span data-ttu-id="81192-307">**OPERAČNÍ SYSTÉM**</span><span class="sxs-lookup"><span data-stu-id="81192-307">**OS**</span></span> | <span data-ttu-id="81192-308">**Verze**</span><span class="sxs-lookup"><span data-stu-id="81192-308">**Version**</span></span> | <span data-ttu-id="81192-309">**ALGORITMUS SHA-256**</span><span class="sxs-lookup"><span data-stu-id="81192-309">**SHA-256**</span></span> |
| --- | --- | --- | --- |
| [<span data-ttu-id="81192-310">InstallDependencyAgent Windows.exe</span><span class="sxs-lookup"><span data-stu-id="81192-310">InstallDependencyAgent-Windows.exe</span></span>](https://aka.ms/dependencyagentwindows) | <span data-ttu-id="81192-311">Windows</span><span class="sxs-lookup"><span data-stu-id="81192-311">Windows</span></span> | <span data-ttu-id="81192-312">9.0.5</span><span class="sxs-lookup"><span data-stu-id="81192-312">9.0.5</span></span> | <span data-ttu-id="81192-313">73B3F6A2A76A08D58F72A550947FF839B588591C48E6EDDD6DDF73AA3FD82B43</span><span class="sxs-lookup"><span data-stu-id="81192-313">73B3F6A2A76A08D58F72A550947FF839B588591C48E6EDDD6DDF73AA3FD82B43</span></span> |
| [<span data-ttu-id="81192-314">InstallDependencyAgent Linux64.bin</span><span class="sxs-lookup"><span data-stu-id="81192-314">InstallDependencyAgent-Linux64.bin</span></span>](https://aka.ms/dependencyagentlinux) | <span data-ttu-id="81192-315">Linux</span><span class="sxs-lookup"><span data-stu-id="81192-315">Linux</span></span> | <span data-ttu-id="81192-316">9.0.5</span><span class="sxs-lookup"><span data-stu-id="81192-316">9.0.5</span></span> | <span data-ttu-id="81192-317">A1BAD0B36EBF79F2B69113A07FCF48C68D90BD169C722689F9C83C69FC032371</span><span class="sxs-lookup"><span data-stu-id="81192-317">A1BAD0B36EBF79F2B69113A07FCF48C68D90BD169C722689F9C83C69FC032371</span></span> |



## <a name="configuration"></a><span data-ttu-id="81192-318">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="81192-318">Configuration</span></span>

<span data-ttu-id="81192-319">Proveďte následující postup pro konfiguraci řešení Data kabelové sítě pro vaše pracovní prostory.</span><span class="sxs-lookup"><span data-stu-id="81192-319">Perform the following steps to configure the Wire Data solution for your workspaces.</span></span>

1. <span data-ttu-id="81192-320">Povolit řešení analýzy protokolů aktivity z [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WireData2OMS?tab=Overview) nebo pomocí procesu popsaného v tématu [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="81192-320">Enable the Activity Log Analytics solution from the [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WireData2OMS?tab=Overview) or by using the process described in [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span>
2. <span data-ttu-id="81192-321">Nainstalujte agenta závislost na každém počítači, ve které chcete načíst data.</span><span class="sxs-lookup"><span data-stu-id="81192-321">Install the Dependency Agent on each computer where you want to get data.</span></span> <span data-ttu-id="81192-322">Závislost agenta můžete monitorovat připojení k okamžité Sousedé BGP, proto musíte nemusí agenta na každý počítač.</span><span class="sxs-lookup"><span data-stu-id="81192-322">The Dependency Agent can monitor connections to immediate neighbors, so you might not need an agent on every computer.</span></span>

### <a name="install-the-dependency-agent-on-windows"></a><span data-ttu-id="81192-323">Nainstalujte agenta závislostí v systému Windows</span><span class="sxs-lookup"><span data-stu-id="81192-323">Install the Dependency Agent on Windows</span></span>

<span data-ttu-id="81192-324">Pro instalaci nebo odinstalaci agenta jsou vyžadována oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="81192-324">Administrator privileges are required to install or uninstall the agent.</span></span>

<span data-ttu-id="81192-325">Je závislost Agent nainstalován v počítačích se systémem Windows prostřednictvím InstallDependencyAgent Windows.exe.</span><span class="sxs-lookup"><span data-stu-id="81192-325">The Dependency Agent is installed on computers running Windows through InstallDependencyAgent-Windows.exe.</span></span> <span data-ttu-id="81192-326">Pokud spustíte tento spustitelný soubor, bez jakékoli možnosti, spustí průvodce, který vám pomůžou při interaktivní instalaci.</span><span class="sxs-lookup"><span data-stu-id="81192-326">If you run this executable file without any options, it starts a wizard that you can follow to install interactively.</span></span>

<span data-ttu-id="81192-327">Použijte následující kroky pro instalaci agenta závislost na každém počítači se systémem Windows:</span><span class="sxs-lookup"><span data-stu-id="81192-327">Use the following steps to install the Dependency Agent on each computer running Windows:</span></span>

1. <span data-ttu-id="81192-328">Nainstalovat agenta OMS pomocí pokynů v [počítače se systémem Windows se připojit ke službě Analýza protokolů v Azure](log-analytics-windows-agents.md).</span><span class="sxs-lookup"><span data-stu-id="81192-328">Install the OMS Agent by using the instructions at [Connect Windows computers to the Log Analytics service in Azure](log-analytics-windows-agents.md).</span></span>
2. <span data-ttu-id="81192-329">Stáhnout agenta pro Windows pomocí odkazu v předchozí části a spusťte jej pomocí následujícího příkazu: InstallDependencyAgent Windows.exe</span><span class="sxs-lookup"><span data-stu-id="81192-329">Download the Windows agent using the link in the previous section and then run it by using the following command: InstallDependencyAgent-Windows.exe</span></span>
3. <span data-ttu-id="81192-330">Postupujte podle pokynů průvodce k instalaci agenta.</span><span class="sxs-lookup"><span data-stu-id="81192-330">Follow the wizard to install the agent.</span></span>
4. <span data-ttu-id="81192-331">Pokud Agent služby Dependency se nepodaří spustit, zkontrolujte protokoly podrobné informace o chybě.</span><span class="sxs-lookup"><span data-stu-id="81192-331">If the Dependency Agent fails to start, check the logs for detailed error information.</span></span> <span data-ttu-id="81192-332">Na agenty se systémem Windows k adresáři protokolu není %Programfiles%\Microsoft Agent\logs závislostí.</span><span class="sxs-lookup"><span data-stu-id="81192-332">On Windows agents, the log directory is %Programfiles%\Microsoft Dependency Agent\logs.</span></span>

#### <a name="windows-command-line"></a><span data-ttu-id="81192-333">Příkazový řádek systému Windows</span><span class="sxs-lookup"><span data-stu-id="81192-333">Windows command line</span></span>

<span data-ttu-id="81192-334">Nainstalujte z příkazového řádku pomocí možnosti z v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="81192-334">Use options from the following table to install from a command line.</span></span> <span data-ttu-id="81192-335">Pokud chcete zobrazit seznam příznaky instalace, spusťte instalační program pomocí /?</span><span class="sxs-lookup"><span data-stu-id="81192-335">To see a list of the installation flags, run the installer by using the /?</span></span> <span data-ttu-id="81192-336">Příznak následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="81192-336">flag as follows.</span></span>

<span data-ttu-id="81192-337">InstallDependencyAgent Windows.exe /?</span><span class="sxs-lookup"><span data-stu-id="81192-337">InstallDependencyAgent-Windows.exe /?</span></span>

| <span data-ttu-id="81192-338">**Příznak**</span><span class="sxs-lookup"><span data-stu-id="81192-338">**Flag**</span></span> | <span data-ttu-id="81192-339">**Popis**</span><span class="sxs-lookup"><span data-stu-id="81192-339">**Description**</span></span> |
| --- | --- |
| <code>/?</code> | <span data-ttu-id="81192-340">Získejte seznam možností příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="81192-340">Get a list of the command-line options.</span></span> |
| <code>/S</code> | <span data-ttu-id="81192-341">Proveďte bezobslužnou instalaci s žádné uživatelské výzvy.</span><span class="sxs-lookup"><span data-stu-id="81192-341">Perform a silent installation with no user prompts.</span></span> |

<span data-ttu-id="81192-342">Soubory pro Windows Agent závislosti jsou umístěny v Agent služby Dependency C:\Program Files\Microsoft ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="81192-342">Files for the Windows Dependency Agent are placed in C:\Program Files\Microsoft Dependency Agent by default.</span></span>

### <a name="install-the-dependency-agent-on-linux"></a><span data-ttu-id="81192-343">Nainstalujte agenta závislostí v systému Linux</span><span class="sxs-lookup"><span data-stu-id="81192-343">Install the Dependency Agent on Linux</span></span>

<span data-ttu-id="81192-344">Kořenový přístup je nutný k instalaci nebo konfiguraci agenta.</span><span class="sxs-lookup"><span data-stu-id="81192-344">Root access is required to install or configure the agent.</span></span>

<span data-ttu-id="81192-345">Agent závislostí je nainstalován na počítače se systémem Linux prostřednictvím InstallDependencyAgent-Linux64.bin, skript prostředí s samorozbalující binární.</span><span class="sxs-lookup"><span data-stu-id="81192-345">The Dependency Agent is installed on Linux computers through InstallDependencyAgent-Linux64.bin, a shell script with a self-extracting binary.</span></span> <span data-ttu-id="81192-346">Soubor můžete spustit pomocí _dílet_ nebo přidejte oprávnění ke samotném souboru.</span><span class="sxs-lookup"><span data-stu-id="81192-346">You can run the file by using _sh_ or add execute permissions to the file itself.</span></span>

<span data-ttu-id="81192-347">Použijte následující kroky pro instalaci agenta závislost na každý počítač se systémem Linux:</span><span class="sxs-lookup"><span data-stu-id="81192-347">Use the following steps to install the Dependency Agent on each Linux computer:</span></span>

1. <span data-ttu-id="81192-348">Nainstalovat agenta OMS pomocí pokynů v [shromažďování a správě dat z počítače se systémem Linux](log-analytics-agent-linux.md).</span><span class="sxs-lookup"><span data-stu-id="81192-348">Install the OMS Agent by using the instructions at [Collect and manage data from Linux computers](log-analytics-agent-linux.md).</span></span>
2. <span data-ttu-id="81192-349">Stáhnout agenta závislostí Linux pomocí odkazu v předchozí části a potom ji nainstalovat jako kořenového adresáře pomocí následujícího příkazu: dílet InstallDependencyAgent Linux64.bin</span><span class="sxs-lookup"><span data-stu-id="81192-349">Download the Linux Dependency agent using the link in the previous section and then install it as root by using the following command: sh InstallDependencyAgent-Linux64.bin</span></span>
3. <span data-ttu-id="81192-350">Pokud Agent služby Dependency se nepodaří spustit, zkontrolujte protokoly podrobné informace o chybě.</span><span class="sxs-lookup"><span data-stu-id="81192-350">If the Dependency Agent fails to start, check the logs for detailed error information.</span></span> <span data-ttu-id="81192-351">V agentech Linux, k adresáři protokolu není: /var/opt/microsoft/dependency-agent/log.</span><span class="sxs-lookup"><span data-stu-id="81192-351">On Linux agents, the log directory is: /var/opt/microsoft/dependency-agent/log.</span></span>

<span data-ttu-id="81192-352">Pokud chcete zobrazit seznam příznaky instalace, spusťte instalační program s `-help` příznak následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="81192-352">To see a list of the installation flags, run the installation program with the `-help` flag as follows.</span></span>

```
InstallDependencyAgent-Linux64.bin -help
```

| <span data-ttu-id="81192-353">**Příznak**</span><span class="sxs-lookup"><span data-stu-id="81192-353">**Flag**</span></span> | <span data-ttu-id="81192-354">**Popis**</span><span class="sxs-lookup"><span data-stu-id="81192-354">**Description**</span></span> |
| --- | --- |
| <code>-help</code> | <span data-ttu-id="81192-355">Získejte seznam možností příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="81192-355">Get a list of the command-line options.</span></span> |
| <code>-s</code> | <span data-ttu-id="81192-356">Proveďte bezobslužnou instalaci s žádné uživatelské výzvy.</span><span class="sxs-lookup"><span data-stu-id="81192-356">Perform a silent installation with no user prompts.</span></span> |
| <code>--check</code> | <span data-ttu-id="81192-357">Zkontrolujte oprávnění a operační systém, ale není nainstalovaný agent.</span><span class="sxs-lookup"><span data-stu-id="81192-357">Check permissions and the operating system but do not install the agent.</span></span> |

<span data-ttu-id="81192-358">Soubory pro agenta závislosti jsou umístěny v adresáři pro následující:</span><span class="sxs-lookup"><span data-stu-id="81192-358">Files for the Dependency Agent are placed in the following directories:</span></span>

| <span data-ttu-id="81192-359">**Soubory**</span><span class="sxs-lookup"><span data-stu-id="81192-359">**Files**</span></span> | <span data-ttu-id="81192-360">**Umístění**</span><span class="sxs-lookup"><span data-stu-id="81192-360">**Location**</span></span> |
| --- | --- |
| <span data-ttu-id="81192-361">Soubory jádra</span><span class="sxs-lookup"><span data-stu-id="81192-361">Core files</span></span> | <span data-ttu-id="81192-362">/OPT/Microsoft/Dependency-Agent</span><span class="sxs-lookup"><span data-stu-id="81192-362">/opt/microsoft/dependency-agent</span></span> |
| <span data-ttu-id="81192-363">Soubory protokolu</span><span class="sxs-lookup"><span data-stu-id="81192-363">Log files</span></span> | <span data-ttu-id="81192-364">/var/OPT/Microsoft/Dependency-Agent/log</span><span class="sxs-lookup"><span data-stu-id="81192-364">/var/opt/microsoft/dependency-agent/log</span></span> |
| <span data-ttu-id="81192-365">Konfigurační soubory</span><span class="sxs-lookup"><span data-stu-id="81192-365">Config files</span></span> | <span data-ttu-id="81192-366">/ETC/OPT/Microsoft/Dependency-Agent/config</span><span class="sxs-lookup"><span data-stu-id="81192-366">/etc/opt/microsoft/dependency-agent/config</span></span> |
| <span data-ttu-id="81192-367">Služby spustitelné soubory</span><span class="sxs-lookup"><span data-stu-id="81192-367">Service executable files</span></span> | <span data-ttu-id="81192-368">/OPT/Microsoft/Dependency-Agent/Bin/Microsoft-Dependency-Agent</span><span class="sxs-lookup"><span data-stu-id="81192-368">/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent</span></span><br><br><span data-ttu-id="81192-369">/OPT/Microsoft/Dependency-Agent/Bin/Microsoft-Dependency-Agent-Manager</span><span class="sxs-lookup"><span data-stu-id="81192-369">/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager</span></span> |
| <span data-ttu-id="81192-370">Úložiště binární soubory</span><span class="sxs-lookup"><span data-stu-id="81192-370">Binary storage files</span></span> | <span data-ttu-id="81192-371">/var/OPT/Microsoft/Dependency-Agent/Storage</span><span class="sxs-lookup"><span data-stu-id="81192-371">/var/opt/microsoft/dependency-agent/storage</span></span> |

### <a name="installation-script-examples"></a><span data-ttu-id="81192-372">Příklady skriptů instalace</span><span class="sxs-lookup"><span data-stu-id="81192-372">Installation script examples</span></span>

<span data-ttu-id="81192-373">Chcete-li snadno nasadit agenta závislosti na počtu serverů najednou, je dobré pomocí skriptu.</span><span class="sxs-lookup"><span data-stu-id="81192-373">To easily deploy the Dependency Agent on many servers at once, it helps to use a script.</span></span> <span data-ttu-id="81192-374">Následující příklady skriptu můžete použít ke stažení a instalaci závislostí agenta v systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="81192-374">You can use the following script examples to download and install the Dependency Agent on either Windows or Linux.</span></span>

#### <a name="powershell-script-for-windows"></a><span data-ttu-id="81192-375">Skript prostředí PowerShell pro systém Windows</span><span class="sxs-lookup"><span data-stu-id="81192-375">PowerShell script for Windows</span></span>

```PowerShell

Invoke-WebRequest &quot;https://aka.ms/dependencyagentwindows&quot; -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S

```

#### <a name="shell-script-for-linux"></a><span data-ttu-id="81192-376">Skript prostředí pro Linux</span><span class="sxs-lookup"><span data-stu-id="81192-376">Shell script for Linux</span></span>

```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
```

```
sh InstallDependencyAgent-Linux64.bin -s
```

### <a name="desired-state-configuration"></a><span data-ttu-id="81192-377">Konfigurace požadovaného stavu</span><span class="sxs-lookup"><span data-stu-id="81192-377">Desired State Configuration</span></span>

<span data-ttu-id="81192-378">Nasazení agenta nástroje závislostí prostřednictvím konfigurace požadovaného stavu, můžete použít modul xPSDesiredStateConfiguration a bit kódu takto:</span><span class="sxs-lookup"><span data-stu-id="81192-378">To deploy the Dependency Agent via Desired State Configuration, you can use the xPSDesiredStateConfiguration module and a bit of code like the following:</span></span>

```
Import-DscResource -ModuleName xPSDesiredStateConfiguration

$DAPackageLocalPath = &quot;C:\InstallDependencyAgent-Windows.exe&quot;



Node $NodeName

{

    # Download and install the Dependency Agent

    xRemoteFile DAPackage

    {

        Uri = &quot;https://aka.ms/dependencyagentwindows&quot;

        DestinationPath = $DAPackageLocalPath

        DependsOn = &quot;[Package]OI&quot;

    }

    xPackage DA

    {

        Ensure=&quot;Present&quot;

        Name = &quot;Dependency Agent&quot;

        Path = $DAPackageLocalPath

        Arguments = '/S'

        ProductId = &quot;&quot;

        InstalledCheckRegKey = &quot;HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\DependencyAgent&quot;

        InstalledCheckRegValueName = &quot;DisplayName&quot;

        InstalledCheckRegValueData = &quot;Dependency Agent&quot;

    }

}

```
### <a name="uninstall-the-dependency-agent"></a><span data-ttu-id="81192-379">Odinstalujte agenta závislostí</span><span class="sxs-lookup"><span data-stu-id="81192-379">Uninstall the Dependency Agent</span></span>

<span data-ttu-id="81192-380">Pomocí následujících částí můžete odebrat agenta závislostí.</span><span class="sxs-lookup"><span data-stu-id="81192-380">Use the following sections to help you remove the Dependency Agent.</span></span>

#### <a name="uninstall-the-dependency-agent-on-windows"></a><span data-ttu-id="81192-381">Odinstalujte agenta závislostí v systému Windows</span><span class="sxs-lookup"><span data-stu-id="81192-381">Uninstall the Dependency Agent on Windows</span></span>

<span data-ttu-id="81192-382">Správce můžete odinstalovat závislostí agenta pro Windows pomocí ovládacích panelů.</span><span class="sxs-lookup"><span data-stu-id="81192-382">An administrator can uninstall the Dependency Agent for Windows through Control Panel.</span></span>

<span data-ttu-id="81192-383">Správce můžete také spouštět %Programfiles%\Microsoft závislostí Agent\Uninstall.exe odinstalace agenta závislostí.</span><span class="sxs-lookup"><span data-stu-id="81192-383">An administrator can also run %Programfiles%\Microsoft Dependency Agent\Uninstall.exe to uninstall the Dependency Agent.</span></span>

#### <a name="uninstall-the-dependency-agent-on-linux"></a><span data-ttu-id="81192-384">Odinstalujte agenta závislostí v systému Linux</span><span class="sxs-lookup"><span data-stu-id="81192-384">Uninstall the Dependency Agent on Linux</span></span>

<span data-ttu-id="81192-385">Úplně odinstalujte agenta závislostí ze systému Linux, je nutné odebrat vlastní agent a konektor, který je automaticky nainstalován s agentem.</span><span class="sxs-lookup"><span data-stu-id="81192-385">To completely uninstall the Dependency Agent from Linux, you must remove the agent itself and the connector, which is installed automatically with the agent.</span></span> <span data-ttu-id="81192-386">Můžete odinstalovat i pomocí následujících jeden příkaz:</span><span class="sxs-lookup"><span data-stu-id="81192-386">You can uninstall both by using the following single command:</span></span>

```
rpm -e dependency-agent dependency-agent-connector
```

## <a name="management-packs"></a><span data-ttu-id="81192-387">Sady Management Pack</span><span class="sxs-lookup"><span data-stu-id="81192-387">Management packs</span></span>

<span data-ttu-id="81192-388">Po aktivaci Data kabelové sítě v pracovním prostoru analýzy protokolů 300 KB management pack je odeslány na všechny servery Windows v něm.</span><span class="sxs-lookup"><span data-stu-id="81192-388">When Wire Data is activated in a Log Analytics workspace, a 300-KB management pack is sent to all the Windows servers in that workspace.</span></span> <span data-ttu-id="81192-389">Pokud používáte System Center Operations Manager agentů v [připojené skupiny pro správu](log-analytics-om-agents.md), z System Center Operations Manager je nasazena sada management pack monitorování závislostí.</span><span class="sxs-lookup"><span data-stu-id="81192-389">If you are using System Center Operations Manager agents in a [connected management group](log-analytics-om-agents.md), the Dependency Monitor management pack is deployed from System Center Operations Manager.</span></span> <span data-ttu-id="81192-390">Pokud jsou připojeny přímo agentů, analýzy protokolů přináší sadu management pack.</span><span class="sxs-lookup"><span data-stu-id="81192-390">If the agents are directly connected, Log Analytics delivers the management pack.</span></span>

<span data-ttu-id="81192-391">Sada management pack je s názvem Microsoft.IntelligencePacks.ApplicationDependencyMonitor.</span><span class="sxs-lookup"><span data-stu-id="81192-391">The management pack is named Microsoft.IntelligencePacks.ApplicationDependencyMonitor.</span></span> <span data-ttu-id="81192-392">Je zapsán do: %Programfiles%\Microsoft monitorování Agent\Agent\Health služby State\Management balíčky.</span><span class="sxs-lookup"><span data-stu-id="81192-392">It's written to: %Programfiles%\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs.</span></span> <span data-ttu-id="81192-393">Zdroj dat, který používá sada management pack je: % Program files%\Microsoft monitorování Agent\Agent\Health služby State\Resources&lt;AutoGeneratedID&gt;\Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.</span><span class="sxs-lookup"><span data-stu-id="81192-393">The data source that the management pack uses is: %Program files%\Microsoft Monitoring Agent\Agent\Health Service State\Resources&lt;AutoGeneratedID&gt;\Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.</span></span>

## <a name="using-the-solution"></a><span data-ttu-id="81192-394">Použití řešení</span><span class="sxs-lookup"><span data-stu-id="81192-394">Using the solution</span></span>

<span data-ttu-id="81192-395">**Instalace a konfigurace řešení**</span><span class="sxs-lookup"><span data-stu-id="81192-395">**Installing and configuring the solution**</span></span>

<span data-ttu-id="81192-396">Použijte následující informace k instalaci a konfiguraci řešení.</span><span class="sxs-lookup"><span data-stu-id="81192-396">Use the following information to install and configure the solution.</span></span>

- <span data-ttu-id="81192-397">Řešení Data kabelové sítě operace čtení dat z počítačů se systémem Windows Server 2012 R2, Windows 8.1 a novější operační systémy.</span><span class="sxs-lookup"><span data-stu-id="81192-397">The Wire Data solution acquires data from computers running Windows Server 2012 R2, Windows 8.1, and later operating systems.</span></span>
- <span data-ttu-id="81192-398">Na počítačích, ve které chcete získat data kabelové sítě z se vyžaduje rozhraní Microsoft .NET Framework 4.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="81192-398">Microsoft .NET Framework 4.0 or later is required on computers where you want to acquire wire data from.</span></span>
- <span data-ttu-id="81192-399">Přidat řešení přenosu dat do pracovního prostoru analýzy protokolů pomocí procesu popsaného v tématu [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="81192-399">Add the Wire Data solution to your Log Analytics workspace using the process described in [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span> <span data-ttu-id="81192-400">Není nutná žádná další konfigurace.</span><span class="sxs-lookup"><span data-stu-id="81192-400">There is no further configuration required.</span></span>
- <span data-ttu-id="81192-401">Pokud chcete zobrazit data kabelové sítě pro konkrétní řešení, budete muset mít řešení již přidán do pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="81192-401">If you want to view wire data for a specific solution, you need to have the solution already added to your workspace.</span></span>

<span data-ttu-id="81192-402">Po mají nainstalovat agenti a nainstalovat řešení, dlaždice 2.0 přenosu dat se zobrazí v pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="81192-402">After you have agents installed and you install the solution, the Wire Data 2.0 tile appears in your workspace.</span></span>

> [!NOTE]
> <span data-ttu-id="81192-403">V současné době vyžaduje použití portálu OMS Chcete-li zobrazit data kabelové sítě.</span><span class="sxs-lookup"><span data-stu-id="81192-403">Currently, you must use the OMS portal to view wire data.</span></span> <span data-ttu-id="81192-404">Chcete-li zobrazit data kabelové sítě nelze pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="81192-404">You cannot use the Azure portal to view wire data.</span></span>

![Dlaždice Data kabelové sítě](./media/log-analytics-wire-data/wire-data-tile.png)

## <a name="using-the-wire-data-20-solution"></a><span data-ttu-id="81192-406">Pomocí řešení přenosu dat 2.0</span><span class="sxs-lookup"><span data-stu-id="81192-406">Using the Wire Data 2.0 solution</span></span>

<span data-ttu-id="81192-407">Na portálu OMS, klikněte **přenosu dat 2.0** dlaždici otevřete řídicí panel Data kabelové sítě.</span><span class="sxs-lookup"><span data-stu-id="81192-407">In the OMS portal, click the **Wire Data 2.0** tile to open the Wire Data dashboard.</span></span> <span data-ttu-id="81192-408">Řídicí panel obsahuje okna v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="81192-408">The dashboard includes the blades in the following table.</span></span> <span data-ttu-id="81192-409">Každý okno uvádí až 10 položky odpovídající kritériím tohoto okna pro zadaný obor a časový rozsah.</span><span class="sxs-lookup"><span data-stu-id="81192-409">Each blade lists up to 10 items matching that blade's criteria for the specified scope and time range.</span></span> <span data-ttu-id="81192-410">Můžete spustit vyhledávání protokolu, který vrátí všechny záznamy kliknutím **zobrazit všechny** v dolní části okna, nebo kliknutím na záhlaví okna.</span><span class="sxs-lookup"><span data-stu-id="81192-410">You can run a log search that returns all records by clicking **See all** at the bottom of the blade or by clicking the blade header.</span></span>

| <span data-ttu-id="81192-411">**Okno**</span><span class="sxs-lookup"><span data-stu-id="81192-411">**Blade**</span></span> | <span data-ttu-id="81192-412">**Popis**</span><span class="sxs-lookup"><span data-stu-id="81192-412">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="81192-413">Agenti zachytávající síťový přenos</span><span class="sxs-lookup"><span data-stu-id="81192-413">Agents capturing network traffic</span></span> | <span data-ttu-id="81192-414">Zobrazuje počet agentů, kteří jsou zachytávání síťového provozu a uvádí top 10 počítačů, které jsou zachycení provozu.</span><span class="sxs-lookup"><span data-stu-id="81192-414">Shows the number of agents that are capturing network traffic and lists the top 10 computers that are capturing traffic.</span></span> <span data-ttu-id="81192-415">Klikněte na číslo ke spuštění protokolu vyhledejte <code>Type:WireData &#124; measure Sum(TotalBytes) by Computer &#124; top 500000</code>.</span><span class="sxs-lookup"><span data-stu-id="81192-415">Click the number to run a log search for <code>Type:WireData &#124; measure Sum(TotalBytes) by Computer &#124; top 500000</code>.</span></span> <span data-ttu-id="81192-416">Klikněte na počítač, v seznamu ke spuštění vyhledávání protokolu vrátí celkový počet bajtů zaznamenat.</span><span class="sxs-lookup"><span data-stu-id="81192-416">Click a computer in the list to run a log search returning the total number of bytes captured.</span></span> |
| <span data-ttu-id="81192-417">Místní podsítě</span><span class="sxs-lookup"><span data-stu-id="81192-417">Local Subnets</span></span> | <span data-ttu-id="81192-418">Zobrazuje počet místní podsítě, které byly zjištěny agenty.</span><span class="sxs-lookup"><span data-stu-id="81192-418">Shows the number of local subnets that agents have discovered.</span></span>  <span data-ttu-id="81192-419">Klikněte na číslo ke spuštění protokolu vyhledejte <code>Type:WireData &#124; Measure Sum(TotalBytes) by LocalSubnet</code> , obsahuje seznam všech podsítí s počet bajtů odeslaných přes každé z nich.</span><span class="sxs-lookup"><span data-stu-id="81192-419">Click the number to run a log search for <code>Type:WireData &#124; Measure Sum(TotalBytes) by LocalSubnet</code> that lists all subnets with the number of bytes sent over each one.</span></span> <span data-ttu-id="81192-420">Klikněte na podsíť v seznamu ke spuštění vyhledávání protokolu vrátí celkový počet bajtů odeslaných přes podsíť.</span><span class="sxs-lookup"><span data-stu-id="81192-420">Click a subnet in the list to run a log search returning the total number of bytes sent over the subnet.</span></span> |
| <span data-ttu-id="81192-421">Protokoly na úrovni aplikace</span><span class="sxs-lookup"><span data-stu-id="81192-421">Application-level Protocols</span></span> | <span data-ttu-id="81192-422">Zobrazuje počet protokoly na úrovni aplikace používána, při zjištění agenty.</span><span class="sxs-lookup"><span data-stu-id="81192-422">Shows the number of application-level protocols in use, as discovered by agents.</span></span> <span data-ttu-id="81192-423">Klikněte na číslo ke spuštění protokolu vyhledejte <code>Type:WireData &#124; Measure Sum(TotalBytes) by ApplicationProtocol</code>.</span><span class="sxs-lookup"><span data-stu-id="81192-423">Click the number to run a log search for <code>Type:WireData &#124; Measure Sum(TotalBytes) by ApplicationProtocol</code>.</span></span> <span data-ttu-id="81192-424">Klikněte na protokol spuštění vyhledávání protokolu vrátí celkový počet bajtů odeslaných pomocí protokolu.</span><span class="sxs-lookup"><span data-stu-id="81192-424">Click a protocol to run a log search returning the total number of bytes sent using the protocol.</span></span> |

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Řídicí panel přenosu dat](./media/log-analytics-wire-data/wire-data-dash.png)

<span data-ttu-id="81192-426">Můžete použít **agenti zachytávající síťový přenos** okno a určit, jaký poměr šířky pásma sítě je spotřebovávanou počítače.</span><span class="sxs-lookup"><span data-stu-id="81192-426">You can use the **Agents capturing network traffic** blade to determine how much network bandwidth is being consumed by computers.</span></span> <span data-ttu-id="81192-427">Toto okno vám může pomoci snadno najít _chattiest_ počítač ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="81192-427">This blade can help you easily find the _chattiest_ computer in your environment.</span></span> <span data-ttu-id="81192-428">Tyto počítače může být přetížený, funguje neobvyklým způsobem, nebo pomocí více síťových prostředků, než normální.</span><span class="sxs-lookup"><span data-stu-id="81192-428">Such computers could be overloaded, acting abnormally, or using more network resources than normal.</span></span>

![Příklad protokolu vyhledávání](./media/log-analytics-wire-data/log-search-example01.png)

<span data-ttu-id="81192-430">Podobně můžete použít **místní podsítě** okno a zjistit, kolik síťový provoz přesunutí prostřednictvím podsítě.</span><span class="sxs-lookup"><span data-stu-id="81192-430">Similarly, you can use the **Local Subnets** blade to determine how much network traffic is moving through your subnets.</span></span> <span data-ttu-id="81192-431">Uživatelé jsou často definovat podsítě kolem důležité oblasti pro své aplikace.</span><span class="sxs-lookup"><span data-stu-id="81192-431">Users often define subnets around critical areas for their applications.</span></span> <span data-ttu-id="81192-432">Toto okno nabízí zobrazení do těchto oblastí.</span><span class="sxs-lookup"><span data-stu-id="81192-432">This blade offers a view into those areas.</span></span>

![Příklad protokolu vyhledávání](./media/log-analytics-wire-data/log-search-example02.png)

<span data-ttu-id="81192-434">**Protokoly na úrovni aplikace** okno je užitečné, protože je užitečné vědět, co protokoly jsou používány.</span><span class="sxs-lookup"><span data-stu-id="81192-434">The **Application-level Protocols** blade is useful because it's helpful know what protocols are in use.</span></span> <span data-ttu-id="81192-435">Například by se dalo očekávat SSH, zda se nepoužívá v prostředí vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="81192-435">For example, you might expect SSH to not be in use in your network environment.</span></span> <span data-ttu-id="81192-436">Zobrazení informací, které jsou k dispozici v okně můžete rychle potvrďte nebo disprove vaše očekávání.</span><span class="sxs-lookup"><span data-stu-id="81192-436">Viewing information available in the blade can quickly confirm or disprove your expectation.</span></span>

![Příklad protokolu vyhledávání](./media/log-analytics-wire-data/log-search-example03.png)

<span data-ttu-id="81192-438">V tomto příkladu může přejít k podrobnostem podrobností SSH a zjistit, které počítače používají SSH a mnoho dalších podrobností o komunikaci.</span><span class="sxs-lookup"><span data-stu-id="81192-438">In this example, you could drill-into SSH details to see which computers are using SSH and many other communication details.</span></span>

![Zo výsledky hledání](./media/log-analytics-wire-data/ssh-details.png)

<span data-ttu-id="81192-440">Je také užitečné vědět, pokud je provozu protokolu zvýšením nebo snížením v čase.</span><span class="sxs-lookup"><span data-stu-id="81192-440">It's also useful to know if protocol traffic is increasing or decreasing over time.</span></span> <span data-ttu-id="81192-441">Například pokud roste množství dat přenášených aplikací, který může být něco, co byste měli vědět, nebo že můžete zjistit pozoruhodné.</span><span class="sxs-lookup"><span data-stu-id="81192-441">For example, if the amount of data being transmitted by an application is increasing, that might be something you should be aware of, or that you might find noteworthy.</span></span>

## <a name="input-data"></a><span data-ttu-id="81192-442">Vstupní data</span><span class="sxs-lookup"><span data-stu-id="81192-442">Input data</span></span>

<span data-ttu-id="81192-443">Data kabelové sítě shromažďuje metadata o síťovém provozu pomocí agentů, které jste povolili.</span><span class="sxs-lookup"><span data-stu-id="81192-443">Wire data collects metadata about network traffic using the agents that you have enabled.</span></span> <span data-ttu-id="81192-444">Každý agent odesílá data o každých 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="81192-444">Each agent sends data about every 15 seconds.</span></span>

## <a name="output-data"></a><span data-ttu-id="81192-445">výstupní data</span><span class="sxs-lookup"><span data-stu-id="81192-445">Output data</span></span>

<span data-ttu-id="81192-446">Záznam s typem _WireData_ se vytvoří pro každý typ vstupní data.</span><span class="sxs-lookup"><span data-stu-id="81192-446">A record with a type of _WireData_ is created for each type of input data.</span></span> <span data-ttu-id="81192-447">WireData záznamy mají vlastnosti zobrazené v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="81192-447">WireData records have properties shown in the following table:</span></span>

| <span data-ttu-id="81192-448">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="81192-448">Property</span></span> | <span data-ttu-id="81192-449">Popis</span><span class="sxs-lookup"><span data-stu-id="81192-449">Description</span></span> |
|---|---|
| <span data-ttu-id="81192-450">Počítač</span><span class="sxs-lookup"><span data-stu-id="81192-450">Computer</span></span> | <span data-ttu-id="81192-451">Název počítače, kde nebyla shromážděna data</span><span class="sxs-lookup"><span data-stu-id="81192-451">Computer name where data was collected</span></span> |
| <span data-ttu-id="81192-452">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="81192-452">TimeGenerated</span></span> | <span data-ttu-id="81192-453">Čas záznamu</span><span class="sxs-lookup"><span data-stu-id="81192-453">Time of the record</span></span> |
| <span data-ttu-id="81192-454">LocalIP</span><span class="sxs-lookup"><span data-stu-id="81192-454">LocalIP</span></span> | <span data-ttu-id="81192-455">IP adresa místního počítače</span><span class="sxs-lookup"><span data-stu-id="81192-455">IP address of the local computer</span></span> |
| <span data-ttu-id="81192-456">SessionState</span><span class="sxs-lookup"><span data-stu-id="81192-456">SessionState</span></span> | <span data-ttu-id="81192-457">Připojení nebo odpojení</span><span class="sxs-lookup"><span data-stu-id="81192-457">Connected or disconnected</span></span> |
| <span data-ttu-id="81192-458">ReceivedBytes</span><span class="sxs-lookup"><span data-stu-id="81192-458">ReceivedBytes</span></span> | <span data-ttu-id="81192-459">Počet přijatých bajtů</span><span class="sxs-lookup"><span data-stu-id="81192-459">Amount of bytes received</span></span> |
| <span data-ttu-id="81192-460">ProtocolName</span><span class="sxs-lookup"><span data-stu-id="81192-460">ProtocolName</span></span> | <span data-ttu-id="81192-461">Název sítě protokol použitý</span><span class="sxs-lookup"><span data-stu-id="81192-461">Name of the network protocol used</span></span> |
| <span data-ttu-id="81192-462">Parametr IPVersion</span><span class="sxs-lookup"><span data-stu-id="81192-462">IPVersion</span></span> | <span data-ttu-id="81192-463">Verze protokolu IP</span><span class="sxs-lookup"><span data-stu-id="81192-463">IP version</span></span> |
| <span data-ttu-id="81192-464">Směr</span><span class="sxs-lookup"><span data-stu-id="81192-464">Direction</span></span> | <span data-ttu-id="81192-465">Příchozí nebo odchozí</span><span class="sxs-lookup"><span data-stu-id="81192-465">Inbound or outbound</span></span> |
| <span data-ttu-id="81192-466">MaliciousIP</span><span class="sxs-lookup"><span data-stu-id="81192-466">MaliciousIP</span></span> | <span data-ttu-id="81192-467">IP adresa známé škodlivé zdroje</span><span class="sxs-lookup"><span data-stu-id="81192-467">IP address of a known malicious source</span></span> |
| <span data-ttu-id="81192-468">Závažnost</span><span class="sxs-lookup"><span data-stu-id="81192-468">Severity</span></span> | <span data-ttu-id="81192-469">Závažnost možného malwaru</span><span class="sxs-lookup"><span data-stu-id="81192-469">Suspected malware severity</span></span> |
| <span data-ttu-id="81192-470">RemoteIPCountry</span><span class="sxs-lookup"><span data-stu-id="81192-470">RemoteIPCountry</span></span> | <span data-ttu-id="81192-471">Země vzdálené IP adresy</span><span class="sxs-lookup"><span data-stu-id="81192-471">Country of the remote IP address</span></span> |
| <span data-ttu-id="81192-472">ManagementGroupName</span><span class="sxs-lookup"><span data-stu-id="81192-472">ManagementGroupName</span></span> | <span data-ttu-id="81192-473">Název skupiny pro správu nástroje Operations Manager</span><span class="sxs-lookup"><span data-stu-id="81192-473">Name of the Operations Manager management group</span></span> |
| <span data-ttu-id="81192-474">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="81192-474">SourceSystem</span></span> | <span data-ttu-id="81192-475">Zdroj, kde nebyla shromážděna data</span><span class="sxs-lookup"><span data-stu-id="81192-475">Source where data was collected</span></span> |
| <span data-ttu-id="81192-476">SessionStartTime</span><span class="sxs-lookup"><span data-stu-id="81192-476">SessionStartTime</span></span> | <span data-ttu-id="81192-477">Čas spuštění relace</span><span class="sxs-lookup"><span data-stu-id="81192-477">Start time of session</span></span> |
| <span data-ttu-id="81192-478">SessionEndTime</span><span class="sxs-lookup"><span data-stu-id="81192-478">SessionEndTime</span></span> | <span data-ttu-id="81192-479">Čas ukončení relace</span><span class="sxs-lookup"><span data-stu-id="81192-479">End time of session</span></span> |
| <span data-ttu-id="81192-480">LocalSubnet</span><span class="sxs-lookup"><span data-stu-id="81192-480">LocalSubnet</span></span> | <span data-ttu-id="81192-481">Podsíť, kde nebyla shromážděna data</span><span class="sxs-lookup"><span data-stu-id="81192-481">Subnet where data was collected</span></span> |
| <span data-ttu-id="81192-482">LocalPortNumber</span><span class="sxs-lookup"><span data-stu-id="81192-482">LocalPortNumber</span></span> | <span data-ttu-id="81192-483">Číslem místního portu</span><span class="sxs-lookup"><span data-stu-id="81192-483">Local port number</span></span> |
| <span data-ttu-id="81192-484">Vzdálená adresa IP</span><span class="sxs-lookup"><span data-stu-id="81192-484">RemoteIP</span></span> | <span data-ttu-id="81192-485">Vzdálené IP adresy používané vzdáleného počítače</span><span class="sxs-lookup"><span data-stu-id="81192-485">Remote IP address used by the remote computer</span></span> |
| <span data-ttu-id="81192-486">RemotePortNumber</span><span class="sxs-lookup"><span data-stu-id="81192-486">RemotePortNumber</span></span> | <span data-ttu-id="81192-487">Číslo portu použité podle vzdálené IP adresy</span><span class="sxs-lookup"><span data-stu-id="81192-487">Port number used by the remote IP address</span></span> |
| <span data-ttu-id="81192-488">ID relace</span><span class="sxs-lookup"><span data-stu-id="81192-488">SessionID</span></span> | <span data-ttu-id="81192-489">Jednoznačná hodnota, která identifikuje relace komunikace mezi dvě IP adresy</span><span class="sxs-lookup"><span data-stu-id="81192-489">A unique value that identifies communication session between two IP addresses</span></span> |
| <span data-ttu-id="81192-490">SentBytes</span><span class="sxs-lookup"><span data-stu-id="81192-490">SentBytes</span></span> | <span data-ttu-id="81192-491">Počet bajtů odeslaných</span><span class="sxs-lookup"><span data-stu-id="81192-491">Number of bytes sent</span></span> |
| <span data-ttu-id="81192-492">TotalBytes</span><span class="sxs-lookup"><span data-stu-id="81192-492">TotalBytes</span></span> | <span data-ttu-id="81192-493">Celkový počet bajtů odeslaných během relace</span><span class="sxs-lookup"><span data-stu-id="81192-493">Total number of bytes sent during session</span></span> |
| <span data-ttu-id="81192-494">ApplicationProtocol</span><span class="sxs-lookup"><span data-stu-id="81192-494">ApplicationProtocol</span></span> | <span data-ttu-id="81192-495">Typ protokolu sítě používá</span><span class="sxs-lookup"><span data-stu-id="81192-495">Type of network protocol used</span></span>   |
| <span data-ttu-id="81192-496">ID procesu</span><span class="sxs-lookup"><span data-stu-id="81192-496">ProcessID</span></span> | <span data-ttu-id="81192-497">ID procesu systému Windows</span><span class="sxs-lookup"><span data-stu-id="81192-497">Windows process ID</span></span> |
| <span data-ttu-id="81192-498">Název_procesu</span><span class="sxs-lookup"><span data-stu-id="81192-498">ProcessName</span></span> | <span data-ttu-id="81192-499">Cesta a název souboru procesu</span><span class="sxs-lookup"><span data-stu-id="81192-499">Path and file name of the process</span></span> |
| <span data-ttu-id="81192-500">RemoteIPLongitude</span><span class="sxs-lookup"><span data-stu-id="81192-500">RemoteIPLongitude</span></span> | <span data-ttu-id="81192-501">Zeměpisná délka IP</span><span class="sxs-lookup"><span data-stu-id="81192-501">IP longitude value</span></span> |
| <span data-ttu-id="81192-502">RemoteIPLatitude</span><span class="sxs-lookup"><span data-stu-id="81192-502">RemoteIPLatitude</span></span> | <span data-ttu-id="81192-503">Zeměpisná šířka IP</span><span class="sxs-lookup"><span data-stu-id="81192-503">IP latitude value</span></span> |


## <a name="next-steps"></a><span data-ttu-id="81192-504">Další kroky</span><span class="sxs-lookup"><span data-stu-id="81192-504">Next steps</span></span>

- <span data-ttu-id="81192-505">[V protokolech Hledat](log-analytics-log-searches.md) zobrazíte podrobné přenosu dat vyhledávání záznamů.</span><span class="sxs-lookup"><span data-stu-id="81192-505">[Search logs](log-analytics-log-searches.md) to view detailed wire data search records.</span></span>
