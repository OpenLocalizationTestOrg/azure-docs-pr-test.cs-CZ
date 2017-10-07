---
title: "aaaWire řešení dat v Log Analytics | Microsoft Docs"
description: "Data kabelové sítě je konsolidované sítě a výkon data z počítačů s agenty OMS, včetně nástroje Operations Manager a agenti připojená k systému Windows. Data sítě je v kombinaci s vaší toohelp data protokolu korelovat data."
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
ms.openlocfilehash: adafdf98dfbda9d87759643a1a606a84eafd1348
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="wire-data-20-preview-solution-in-log-analytics"></a><span data-ttu-id="a7895-104">Řešení přenosu dat 2.0 (Preview) v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="a7895-104">Wire Data 2.0 (Preview) solution in Log Analytics</span></span>

![Přenosová datový symbol](./media/log-analytics-wire-data/wire-data2-symbol.png)

<span data-ttu-id="a7895-106">Data kabelové sítě je konsolidované sítě a výkon data z počítačů s agenty OMS, včetně nástroje Operations Manager, připojení systému Windows a Linux agenty.</span><span class="sxs-lookup"><span data-stu-id="a7895-106">Wire data is consolidated network and performance data from computers with OMS agents, including Operations Manager, Windows-connected, and Linux agents.</span></span> <span data-ttu-id="a7895-107">Data sítě je v kombinaci s vaší jiných toohelp data protokolu korelovat data.</span><span class="sxs-lookup"><span data-stu-id="a7895-107">Network data is combined with your other log data toohelp you correlate data.</span></span>

<span data-ttu-id="a7895-108">Kromě toho tooOMS agenti hello řešení přenosu dat používá Microsoft Dependency agentů, které můžete nainstalovat na počítače ve vaší infrastruktuře IT.</span><span class="sxs-lookup"><span data-stu-id="a7895-108">In addition tooOMS agents, hello Wire Data solution uses Microsoft Dependency agents that you install on computers in your IT infrastructure.</span></span> <span data-ttu-id="a7895-109">Závislost agenty monitorování sítě data odeslaná tooand z počítačů pro síť úrovně 2 – 3 v hello [OSI model](https://en.wikipedia.org/wiki/OSI_model), včetně hello různé protokoly a porty používané.</span><span class="sxs-lookup"><span data-stu-id="a7895-109">Dependency Agents monitor network data sent tooand from your computers for network levels 2-3 in hello [OSI model](https://en.wikipedia.org/wiki/OSI_model), including hello various protocols and ports used.</span></span> <span data-ttu-id="a7895-110">Data se pak odešlou tooLog analýzy využití agentů.</span><span class="sxs-lookup"><span data-stu-id="a7895-110">Data is then sent tooLog Analytics using agents.</span></span>

> [!NOTE]
> <span data-ttu-id="a7895-111">Nelze přidat hello předchozí verze hello Data kabelové sítě řešení toonew pracovních prostorů.</span><span class="sxs-lookup"><span data-stu-id="a7895-111">You cannot add hello previous version of hello Wire Data solution toonew workspaces.</span></span> <span data-ttu-id="a7895-112">Pokud máte hello původní Data kabelové sítě povolené žádné řešení, můžete dál toouse ho.</span><span class="sxs-lookup"><span data-stu-id="a7895-112">If you have hello original Wire Data solution enabled, you can continue toouse it.</span></span> <span data-ttu-id="a7895-113">Ale toouse přenosu dat 2.0, je nutné nejprve odebrat hello původní verze.</span><span class="sxs-lookup"><span data-stu-id="a7895-113">However, toouse Wire Data 2.0, you must first remove hello original version.</span></span>

<span data-ttu-id="a7895-114">Ve výchozím nastavení analýzy protokolů shromažďuje data protokolu pro procesoru, paměti, disku a data výkonu sítě z čítačů, které jsou součástí systému Windows.</span><span class="sxs-lookup"><span data-stu-id="a7895-114">By default, Log Analytics collects logged data for CPU, memory, disk, and network performance data from counters built into Windows.</span></span> <span data-ttu-id="a7895-115">Sítě a jiných shromažďování dat se provádí v v reálném čase pro každého agenta, včetně podsítě a úrovni aplikace protokoly hello počítače.</span><span class="sxs-lookup"><span data-stu-id="a7895-115">Network and other data collection is done in real-time for each agent, including subnets and application-level protocols being used by hello computer.</span></span> <span data-ttu-id="a7895-116">Na stránce nastavení hello na kartě hello protokoly můžete přidat další čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="a7895-116">You can add other performance counters on hello Settings page on hello Logs tab.</span></span>

<span data-ttu-id="a7895-117">Pokud jste použili [sFlow](http://www.sflow.org/) nebo jiný software s [společnosti Cisco NetFlow protokol](http://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html), pak hello statistiky a data se zobrazí z data kabelové sítě budou známé tooyou.</span><span class="sxs-lookup"><span data-stu-id="a7895-117">If you've used [sFlow](http://www.sflow.org/) or other software with [Cisco's NetFlow protocol](http://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html), then hello statistics and data you see from wire data will be familiar tooyou.</span></span>

<span data-ttu-id="a7895-118">Mezi typy hello předdefinované dotazy vyhledávání protokolu patří:</span><span class="sxs-lookup"><span data-stu-id="a7895-118">Some of hello types of built-in Log search queries include:</span></span>

- <span data-ttu-id="a7895-119">Agenti poskytující data kabelové sítě</span><span class="sxs-lookup"><span data-stu-id="a7895-119">Agents that provide wire data</span></span>
- <span data-ttu-id="a7895-120">IP adresy agentů poskytujících data kabelové sítě</span><span class="sxs-lookup"><span data-stu-id="a7895-120">IP address of agents providing wire data</span></span>
- <span data-ttu-id="a7895-121">Odchozí komunikace podle IP adresy</span><span class="sxs-lookup"><span data-stu-id="a7895-121">Outbound communications by IP addresses</span></span>
- <span data-ttu-id="a7895-122">Počet bajtů odeslaných protokoly aplikací</span><span class="sxs-lookup"><span data-stu-id="a7895-122">Number of bytes sent by application protocols</span></span>
- <span data-ttu-id="a7895-123">Počet bajtů odeslaných služba aplikace</span><span class="sxs-lookup"><span data-stu-id="a7895-123">Number of bytes sent by an application service</span></span>
- <span data-ttu-id="a7895-124">Počet přijatých bajtů podle různé protokoly</span><span class="sxs-lookup"><span data-stu-id="a7895-124">Bytes received by different protocols</span></span>
- <span data-ttu-id="a7895-125">Celkový počet bajtů odeslaných a přijatých podle verze protokolu IP</span><span class="sxs-lookup"><span data-stu-id="a7895-125">Total bytes sent and received by IP version</span></span>
- <span data-ttu-id="a7895-126">Průměrná latence pro připojení, které byly spolehlivě</span><span class="sxs-lookup"><span data-stu-id="a7895-126">Average latency for connections that were measured reliably</span></span>
- <span data-ttu-id="a7895-127">Počítač, aby iniciovaly nebo přijaly síťový provoz zpracovává</span><span class="sxs-lookup"><span data-stu-id="a7895-127">Computer processes that initiated or received network traffic</span></span>
- <span data-ttu-id="a7895-128">Objem síťového přenosu pro zpracování</span><span class="sxs-lookup"><span data-stu-id="a7895-128">Amount of network traffic for a process</span></span>

<span data-ttu-id="a7895-129">Při hledání pomocí data kabelové sítě, můžete filtrovat a skupiny dat tooview informace o hello nejvyšší agenty a horní protokoly.</span><span class="sxs-lookup"><span data-stu-id="a7895-129">When you search using wire data, you can filter and group data tooview information about hello top agents and top protocols.</span></span> <span data-ttu-id="a7895-130">Nebo můžete zobrazit, kdy některé počítače (IP adresy MAC adresy) přenášená mezi sebou, jak dlouho a kolik data byla odeslána – v zásadě platí, zobrazit metadata o síťovém provozu, který je na základě hledání.</span><span class="sxs-lookup"><span data-stu-id="a7895-130">Or you can view when certain computers (IP addresses/MAC addresses) communicated with each other, for how long, and how much data was sent—basically, you view metadata about network traffic, which is search-based.</span></span>

<span data-ttu-id="a7895-131">Ale vzhledem k tomu, že zobrazíte metadata, není nutně užitečné pro odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="a7895-131">However, since you're viewing metadata, it's not necessarily useful for in-depth troubleshooting.</span></span> <span data-ttu-id="a7895-132">Data kabelové sítě v Log Analytics není úplná sběru dat v síti.</span><span class="sxs-lookup"><span data-stu-id="a7895-132">Wire data in Log Analytics is not a full capture of network data.</span></span> <span data-ttu-id="a7895-133">Ano rozhraní není určeno pro řešení potíží s hloubky úrovni paketů.</span><span class="sxs-lookup"><span data-stu-id="a7895-133">So, it's not intended for deep packet-level troubleshooting.</span></span> <span data-ttu-id="a7895-134">Dobrý den výhodou použití hello agenta, porovnání metod kolekcí tooother, je, že nebudete mít tooinstall zařízení, konfigurovat síťové přepínače nebo preform složitá konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a7895-134">hello advantage of using hello agent, compared tooother collection methods, is that you don't have tooinstall appliances, reconfigure your network switches, or preform complicated configurations.</span></span> <span data-ttu-id="a7895-135">Data kabelové sítě je jednoduše založené na agentovi – nainstalujte agenta hello v počítači a jeho bude monitorování vlastní síťového provozu.</span><span class="sxs-lookup"><span data-stu-id="a7895-135">Wire data is simply agent-based—you install hello agent on a computer and it will monitor its own network traffic.</span></span> <span data-ttu-id="a7895-136">Další výhodou je, když chcete, aby toomonitor úlohy běžící v cloudu poskytovatelů nebo poskytovatel hostitelských služeb nebo Microsoft Azure, kde uživatel hello není vlastníkem vrstvy hello prostředků infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="a7895-136">Another advantage is when you want toomonitor workloads running in cloud providers or hosting service provider or Microsoft Azure, where hello user doesn't own hello fabric layer.</span></span>

## <a name="connected-sources"></a><span data-ttu-id="a7895-137">Připojené zdroje</span><span class="sxs-lookup"><span data-stu-id="a7895-137">Connected sources</span></span>

<span data-ttu-id="a7895-138">Data kabelové sítě získá data z hello Agent služby Dependency společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="a7895-138">Wire Data gets its data from hello Microsoft Dependency Agent.</span></span> <span data-ttu-id="a7895-139">Hello Agent služby Dependency závisí na hello agenta OMS pro jeho připojení tooLog Analytics.</span><span class="sxs-lookup"><span data-stu-id="a7895-139">hello Dependency Agent depends on hello OMS Agent for its connections tooLog Analytics.</span></span> <span data-ttu-id="a7895-140">To znamená, že server musí mít hello agenta OMS nainstalovaný a nakonfigurovaný první, a pak nainstalujte hello Agent služby Dependency.</span><span class="sxs-lookup"><span data-stu-id="a7895-140">This means that a server must have hello OMS Agent installed and configured first, and then you install hello Dependency Agent.</span></span> <span data-ttu-id="a7895-141">Hello následující tabulka popisuje hello připojené zdroje, které podporuje hello Data kabelové sítě řešení.</span><span class="sxs-lookup"><span data-stu-id="a7895-141">hello following table describes hello connected sources that hello Wire Data solution supports.</span></span>

| <span data-ttu-id="a7895-142">**Připojené zdroje**</span><span class="sxs-lookup"><span data-stu-id="a7895-142">**Connected source**</span></span> | <span data-ttu-id="a7895-143">**Podporuje se**</span><span class="sxs-lookup"><span data-stu-id="a7895-143">**Supported**</span></span> | <span data-ttu-id="a7895-144">**Popis**</span><span class="sxs-lookup"><span data-stu-id="a7895-144">**Description**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a7895-145">Agenti systému Windows</span><span class="sxs-lookup"><span data-stu-id="a7895-145">Windows agents</span></span> | <span data-ttu-id="a7895-146">Ano</span><span class="sxs-lookup"><span data-stu-id="a7895-146">Yes</span></span> | <span data-ttu-id="a7895-147">Data kabelové sítě analyzuje a shromažďuje data z počítače se systémem Windows agenta.</span><span class="sxs-lookup"><span data-stu-id="a7895-147">Wire Data analyzes and collects data from Windows agent computers.</span></span> <br><br> <span data-ttu-id="a7895-148">V přidání toohello [agenta OMS](log-analytics-windows-agents.md), vyžadují hello Agent služby Microsoft Dependency agentů v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="a7895-148">In addition toohello [OMS Agent](log-analytics-windows-agents.md), Windows agents require hello Microsoft Dependency Agent.</span></span> <span data-ttu-id="a7895-149">V tématu hello [podporované operační systémy](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) úplný seznam verzí operačního systému.</span><span class="sxs-lookup"><span data-stu-id="a7895-149">See hello [supported operating systems](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) for a complete list of operating system versions.</span></span> |
| <span data-ttu-id="a7895-150">Agenti systému Linux</span><span class="sxs-lookup"><span data-stu-id="a7895-150">Linux agents</span></span> | <span data-ttu-id="a7895-151">Ano</span><span class="sxs-lookup"><span data-stu-id="a7895-151">Yes</span></span> | <span data-ttu-id="a7895-152">Data kabelové sítě analyzuje a shromažďuje data z počítače se systémem Linux agent.</span><span class="sxs-lookup"><span data-stu-id="a7895-152">Wire Data analyzes and collects data from Linux agent computers.</span></span><br><br> <span data-ttu-id="a7895-153">V přidání toohello [agenta OMS](log-analytics-linux-agents.md), agenty Linux vyžadují hello Agent služby Dependency společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="a7895-153">In addition toohello [OMS Agent](log-analytics-linux-agents.md), Linux agents require hello Microsoft Dependency Agent.</span></span> <span data-ttu-id="a7895-154">V tématu hello [podporované operační systémy](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) úplný seznam verzí operačního systému.</span><span class="sxs-lookup"><span data-stu-id="a7895-154">See hello [supported operating systems](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) for a complete list of operating system versions.</span></span> |
| <span data-ttu-id="a7895-155">Skupina pro správu nástroje System Center Operations Manager</span><span class="sxs-lookup"><span data-stu-id="a7895-155">System Center Operations Manager management group</span></span> | <span data-ttu-id="a7895-156">Ano</span><span class="sxs-lookup"><span data-stu-id="a7895-156">Yes</span></span> | <span data-ttu-id="a7895-157">Analyzuje Data kabelové sítě a shromažďuje data z agentů systému Windows a Linux v připojeného [skupiny pro správu System Center Operations Manager](log-analytics-om-agents.md).</span><span class="sxs-lookup"><span data-stu-id="a7895-157">Wire Data analyzes and collects data from Windows and Linux agents in a connected [System Center Operations Manager management group](log-analytics-om-agents.md).</span></span> <br><br> <span data-ttu-id="a7895-158">Přímé připojení z tooLog počítače agenta System Center Operations Manager hello Analytics je požadovaná.</span><span class="sxs-lookup"><span data-stu-id="a7895-158">A direct connection from hello System Center Operations Manager agent computer tooLog Analytics is required.</span></span> <span data-ttu-id="a7895-159">Z hello správy skupiny tooLog Analytics se předají data.</span><span class="sxs-lookup"><span data-stu-id="a7895-159">Data is forwarded from hello management group tooLog Analytics.</span></span> |
| <span data-ttu-id="a7895-160">Účet služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="a7895-160">Azure storage account</span></span> | <span data-ttu-id="a7895-161">Ne</span><span class="sxs-lookup"><span data-stu-id="a7895-161">No</span></span> | <span data-ttu-id="a7895-162">Data kabelové sítě shromažďuje data z počítačů agentů, takže není žádná data z něj toocollect ze služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="a7895-162">Wire Data collects data from agent computers, so there is no data from it toocollect from Azure Storage.</span></span> |

<span data-ttu-id="a7895-163">V systému Windows hello Microsoft Monitoring Agent (MMA) je používána toogather System Center Operations Manager a analýzy protokolů a odesílat data.</span><span class="sxs-lookup"><span data-stu-id="a7895-163">On Windows, hello Microsoft Monitoring Agent (MMA) is used by both System Center Operations Manager and Log Analytics toogather and send data.</span></span> <span data-ttu-id="a7895-164">V závislosti na kontextu hello se nazývá hello agenta hello agenta System Center Operations Manager, OMS Agent, Agent analýzy protokolů, MMA nebo přímé agenta.</span><span class="sxs-lookup"><span data-stu-id="a7895-164">Depending on hello context, hello agent is called hello System Center Operations Manager Agent, OMS Agent, Log Analytics Agent, MMA, or Direct Agent.</span></span> <span data-ttu-id="a7895-165">System Center Operations Manager a analýzy protokolů poskytují mírně různých verzích hello MMA.</span><span class="sxs-lookup"><span data-stu-id="a7895-165">System Center Operations Manager and Log Analytics provide slightly different versions of hello MMA.</span></span> <span data-ttu-id="a7895-166">Tyto verze lze každou zprávu tooSystem Center Operations Manager, tooLog analýzy nebo tooboth.</span><span class="sxs-lookup"><span data-stu-id="a7895-166">These versions can each report tooSystem Center Operations Manager, tooLog Analytics, or tooboth.</span></span>

<span data-ttu-id="a7895-167">V systému Linux hello OMS agenta pro Linux shromažďuje a odesílá data tooLog Analytics.</span><span class="sxs-lookup"><span data-stu-id="a7895-167">On Linux, hello OMS Agent for Linux gathers and sends data tooLog Analytics.</span></span> <span data-ttu-id="a7895-168">Data kabelové sítě můžete použít na serverech s agenty přímé OMS nebo na servery, které jsou připojené tooLog analýzy prostřednictvím skupin pro správu System Center Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="a7895-168">You can use Wire Data on servers with OMS Direct Agents or on servers that are attached tooLog Analytics via System Center Operations Manager management groups.</span></span>

<span data-ttu-id="a7895-169">V tomto článku odkazuje tooall agentů, jestli Linux nebo Windows, zda tooa připojené skupiny pro správu System Center Operations Manager, nebo přímo tooLog Analytics se říká hello _agenta OMS_.</span><span class="sxs-lookup"><span data-stu-id="a7895-169">In this article, references tooall agents, whether Linux or Windows, whether connected tooa System Center Operations Manager management group or directly tooLog Analytics are termed hello _OMS agent_.</span></span> <span data-ttu-id="a7895-170">Název konkrétní nasazení hello hello agenta použijeme jenom v případě, že je potřeba pro kontext.</span><span class="sxs-lookup"><span data-stu-id="a7895-170">We'll use hello specific deployment name of hello agent only if it's needed for context.</span></span>

<span data-ttu-id="a7895-171">Hello Agent služby Dependency nepřenáší samotná data a nevyžaduje žádné změny toofirewalls nebo porty.</span><span class="sxs-lookup"><span data-stu-id="a7895-171">hello Dependency Agent does not transmit any data itself, and it does not require any changes toofirewalls or ports.</span></span> <span data-ttu-id="a7895-172">Hello dat v Data kabelové sítě je vždy přenášených v rámci hello OMS agenta tooLog analýzy, buď přímo nebo pomocí hello OMS brány.</span><span class="sxs-lookup"><span data-stu-id="a7895-172">hello data in Wire Data is always transmitted by hello OMS agent tooLog Analytics, either directly or using hello OMS Gateway.</span></span>

![diagram agenta](./media/log-analytics-wire-data/agents.png)

<span data-ttu-id="a7895-174">Pokud jste uživatele System Center Operations Manager s tooLog připojené skupiny správy Analytics:</span><span class="sxs-lookup"><span data-stu-id="a7895-174">If you are a System Center Operations Manager user with a management group connected tooLog Analytics:</span></span>

- <span data-ttu-id="a7895-175">V případě agenty nástroje System Center Operations Manager můžete přístup hello Internet tooconnect tooLog analýzy, není nutná žádná další konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a7895-175">No additional configuration is required when your System Center Operations Manager agents can access hello Internet tooconnect tooLog Analytics.</span></span>
- <span data-ttu-id="a7895-176">Musíte tooconfigure hello OMS brány toowork nástrojem System Center Operations Manager, když agenty nástroje System Center Operations Manager nelze přistoupit k analýze protokolů přes hello Internet.</span><span class="sxs-lookup"><span data-stu-id="a7895-176">You need tooconfigure hello OMS Gateway toowork with System Center Operations Manager when your System Center Operations Manager agents cannot access Log Analytics over hello Internet.</span></span>

<span data-ttu-id="a7895-177">Pokud používáte hello přímé agenta, je třeba tooconfigure hello OMS vlastní agent tooconnect tooLog analýzy nebo tooyour OMS brány.</span><span class="sxs-lookup"><span data-stu-id="a7895-177">If you are using hello Direct Agent, you need tooconfigure hello OMS agent itself tooconnect tooLog Analytics or tooyour OMS Gateway.</span></span> <span data-ttu-id="a7895-178">Hello OMS brány si můžete stáhnout z hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=52666).</span><span class="sxs-lookup"><span data-stu-id="a7895-178">You can download hello OMS Gateway from hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=52666).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a7895-179">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a7895-179">Prerequisites</span></span>

- <span data-ttu-id="a7895-180">Vyžaduje hello [přehledy a analýzy](https://www.microsoft.com/cloud-platform/operations-management-suite-pricing) nabídka řešení.</span><span class="sxs-lookup"><span data-stu-id="a7895-180">Requires hello [Insight and Analytics](https://www.microsoft.com/cloud-platform/operations-management-suite-pricing) solution offer.</span></span>
- <span data-ttu-id="a7895-181">Pokud používáte předchozí verzi hello hello Data kabelové sítě řešení, je nutné ji odebrat.</span><span class="sxs-lookup"><span data-stu-id="a7895-181">If you're using hello previous version of hello Wire Data solution, you must first remove it.</span></span> <span data-ttu-id="a7895-182">Všechna data zaznamenaná prostřednictvím hello původní Data kabelové sítě řešení je ale stále k dispozici v přenosu dat 2.0 a hledání protokolů.</span><span class="sxs-lookup"><span data-stu-id="a7895-182">However, all data captured through hello original Wire Data solution is still available in Wire Data 2.0 and log search.</span></span>
- <span data-ttu-id="a7895-183">Oprávnění správce se vyžaduje tooinstall nebo odinstalovat hello Agent služby Dependency.</span><span class="sxs-lookup"><span data-stu-id="a7895-183">Administrator privileges are required tooinstall or uninstall hello Dependency Agent.</span></span>
- <span data-ttu-id="a7895-184">Hello Agent služby Dependency musí být nainstalován na počítači s 64bitový operační systém.</span><span class="sxs-lookup"><span data-stu-id="a7895-184">hello Dependency Agent must be installed on a computer with a 64-bit operating system.</span></span>

### <a name="operating-systems"></a><span data-ttu-id="a7895-185">Operační systémy</span><span class="sxs-lookup"><span data-stu-id="a7895-185">Operating systems</span></span>

<span data-ttu-id="a7895-186">Hello následující části uvádějí hello podporované operační systémy pro hello Agent služby Dependency.</span><span class="sxs-lookup"><span data-stu-id="a7895-186">hello following sections list hello supported operating systems for hello Dependency Agent.</span></span> <span data-ttu-id="a7895-187">Data kabelové sítě nepodporuje 32bitové architektury pro všechny operační systémy.</span><span class="sxs-lookup"><span data-stu-id="a7895-187">Wire Data doesn't support 32-bit architectures for any operating system.</span></span>

#### <a name="windows-server"></a><span data-ttu-id="a7895-188">Windows Server</span><span class="sxs-lookup"><span data-stu-id="a7895-188">Windows Server</span></span>

- <span data-ttu-id="a7895-189">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="a7895-189">Windows Server 2016</span></span>
- <span data-ttu-id="a7895-190">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="a7895-190">Windows Server 2012 R2</span></span>
- <span data-ttu-id="a7895-191">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="a7895-191">Windows Server 2012</span></span>
- <span data-ttu-id="a7895-192">Windows Server 2008 R2 SP1</span><span class="sxs-lookup"><span data-stu-id="a7895-192">Windows Server 2008 R2 SP1</span></span>

#### <a name="windows-desktop"></a><span data-ttu-id="a7895-193">Plocha Windows</span><span class="sxs-lookup"><span data-stu-id="a7895-193">Windows desktop</span></span>

- <span data-ttu-id="a7895-194">Windows 10</span><span class="sxs-lookup"><span data-stu-id="a7895-194">Windows 10</span></span>
- <span data-ttu-id="a7895-195">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="a7895-195">Windows 8.1</span></span>
- <span data-ttu-id="a7895-196">Windows 8</span><span class="sxs-lookup"><span data-stu-id="a7895-196">Windows 8</span></span>
- <span data-ttu-id="a7895-197">Windows 7</span><span class="sxs-lookup"><span data-stu-id="a7895-197">Windows 7</span></span>

#### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a><span data-ttu-id="a7895-198">Red Hat Enterprise Linux, CentOS Linux a Oracle Linux (s RHEL jádra)</span><span class="sxs-lookup"><span data-stu-id="a7895-198">Red Hat Enterprise Linux, CentOS Linux, and Oracle Linux (with RHEL Kernel)</span></span>

- <span data-ttu-id="a7895-199">Podporovány jsou pouze výchozí a verze SMP Linux jádra.</span><span class="sxs-lookup"><span data-stu-id="a7895-199">Only default and SMP Linux kernel releases are supported.</span></span>
- <span data-ttu-id="a7895-200">Nestandardní jádra uvolní, například PAE a Xen, nejsou podporovány pro všechny distribuci systému Linux.</span><span class="sxs-lookup"><span data-stu-id="a7895-200">Nonstandard kernel releases, such as PAE and Xen, are not supported for any Linux distribution.</span></span> <span data-ttu-id="a7895-201">Například systém s řetězec verze hello _2.6.16.21-0.8-xen_ není podporován.</span><span class="sxs-lookup"><span data-stu-id="a7895-201">For example, a system with hello release string of _2.6.16.21-0.8-xen_ is not supported.</span></span>
- <span data-ttu-id="a7895-202">Vlastní jádra, včetně opakovaných kompilací standardní jádra, nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="a7895-202">Custom kernels, including recompiles of standard kernels, are not supported.</span></span>
- <span data-ttu-id="a7895-203">CentOSPlus jádra není podporována.</span><span class="sxs-lookup"><span data-stu-id="a7895-203">CentOSPlus kernel is not supported.</span></span>
- <span data-ttu-id="a7895-204">Oracle nedělitelné Enterprise jádra (UEK) je popsaná v další části tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="a7895-204">Oracle Unbreakable Enterprise Kernel (UEK) is covered in a later section of this article.</span></span>

#### <a name="red-hat-linux-7"></a><span data-ttu-id="a7895-205">Red Hat Linux 7</span><span class="sxs-lookup"><span data-stu-id="a7895-205">Red Hat Linux 7</span></span>

| <span data-ttu-id="a7895-206">**Verze operačního systému**</span><span class="sxs-lookup"><span data-stu-id="a7895-206">**OS version**</span></span> | <span data-ttu-id="a7895-207">**Verze jádra**</span><span class="sxs-lookup"><span data-stu-id="a7895-207">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="a7895-208">7.0</span><span class="sxs-lookup"><span data-stu-id="a7895-208">7.0</span></span> | <span data-ttu-id="a7895-209">3.10.0-123</span><span class="sxs-lookup"><span data-stu-id="a7895-209">3.10.0-123</span></span> |
| <span data-ttu-id="a7895-210">7.1</span><span class="sxs-lookup"><span data-stu-id="a7895-210">7.1</span></span> | <span data-ttu-id="a7895-211">3.10.0-229</span><span class="sxs-lookup"><span data-stu-id="a7895-211">3.10.0-229</span></span> |
| <span data-ttu-id="a7895-212">7.2</span><span class="sxs-lookup"><span data-stu-id="a7895-212">7.2</span></span> | <span data-ttu-id="a7895-213">3.10.0-327</span><span class="sxs-lookup"><span data-stu-id="a7895-213">3.10.0-327</span></span> |
| <span data-ttu-id="a7895-214">7.3</span><span class="sxs-lookup"><span data-stu-id="a7895-214">7.3</span></span> | <span data-ttu-id="a7895-215">3.10.0-514</span><span class="sxs-lookup"><span data-stu-id="a7895-215">3.10.0-514</span></span> |

#### <a name="red-hat-linux-6"></a><span data-ttu-id="a7895-216">Red Hat Linux 6</span><span class="sxs-lookup"><span data-stu-id="a7895-216">Red Hat Linux 6</span></span>

| <span data-ttu-id="a7895-217">**Verze operačního systému**</span><span class="sxs-lookup"><span data-stu-id="a7895-217">**OS version**</span></span> | <span data-ttu-id="a7895-218">**Verze jádra**</span><span class="sxs-lookup"><span data-stu-id="a7895-218">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="a7895-219">6.0</span><span class="sxs-lookup"><span data-stu-id="a7895-219">6.0</span></span> | <span data-ttu-id="a7895-220">2.6.32-71</span><span class="sxs-lookup"><span data-stu-id="a7895-220">2.6.32-71</span></span> |
| <span data-ttu-id="a7895-221">6.1</span><span class="sxs-lookup"><span data-stu-id="a7895-221">6.1</span></span> | <span data-ttu-id="a7895-222">2.6.32-131</span><span class="sxs-lookup"><span data-stu-id="a7895-222">2.6.32-131</span></span> |
| <span data-ttu-id="a7895-223">6.2</span><span class="sxs-lookup"><span data-stu-id="a7895-223">6.2</span></span> | <span data-ttu-id="a7895-224">2.6.32-220</span><span class="sxs-lookup"><span data-stu-id="a7895-224">2.6.32-220</span></span> |
| <span data-ttu-id="a7895-225">6.3</span><span class="sxs-lookup"><span data-stu-id="a7895-225">6.3</span></span> | <span data-ttu-id="a7895-226">2.6.32-279</span><span class="sxs-lookup"><span data-stu-id="a7895-226">2.6.32-279</span></span> |
| <span data-ttu-id="a7895-227">6.4</span><span class="sxs-lookup"><span data-stu-id="a7895-227">6.4</span></span> | <span data-ttu-id="a7895-228">2.6.32-358</span><span class="sxs-lookup"><span data-stu-id="a7895-228">2.6.32-358</span></span> |
| <span data-ttu-id="a7895-229">6.5</span><span class="sxs-lookup"><span data-stu-id="a7895-229">6.5</span></span> | <span data-ttu-id="a7895-230">2.6.32-431</span><span class="sxs-lookup"><span data-stu-id="a7895-230">2.6.32-431</span></span> |
| <span data-ttu-id="a7895-231">6.6</span><span class="sxs-lookup"><span data-stu-id="a7895-231">6.6</span></span> | <span data-ttu-id="a7895-232">2.6.32-504</span><span class="sxs-lookup"><span data-stu-id="a7895-232">2.6.32-504</span></span> |
| <span data-ttu-id="a7895-233">6.7</span><span class="sxs-lookup"><span data-stu-id="a7895-233">6.7</span></span> | <span data-ttu-id="a7895-234">2.6.32-573</span><span class="sxs-lookup"><span data-stu-id="a7895-234">2.6.32-573</span></span> |
| <span data-ttu-id="a7895-235">6.8</span><span class="sxs-lookup"><span data-stu-id="a7895-235">6.8</span></span> | <span data-ttu-id="a7895-236">2.6.32-642</span><span class="sxs-lookup"><span data-stu-id="a7895-236">2.6.32-642</span></span> |

#### <a name="red-hat-linux-5"></a><span data-ttu-id="a7895-237">Red Hat Linux 5</span><span class="sxs-lookup"><span data-stu-id="a7895-237">Red Hat Linux 5</span></span>

| <span data-ttu-id="a7895-238">**Verze operačního systému**</span><span class="sxs-lookup"><span data-stu-id="a7895-238">**OS version**</span></span> | <span data-ttu-id="a7895-239">**Verze jádra**</span><span class="sxs-lookup"><span data-stu-id="a7895-239">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="a7895-240">5.8</span><span class="sxs-lookup"><span data-stu-id="a7895-240">5.8</span></span> | <span data-ttu-id="a7895-241">2.6.18-308</span><span class="sxs-lookup"><span data-stu-id="a7895-241">2.6.18-308</span></span> |
| <span data-ttu-id="a7895-242">5.9</span><span class="sxs-lookup"><span data-stu-id="a7895-242">5.9</span></span> | <span data-ttu-id="a7895-243">2.6.18-348</span><span class="sxs-lookup"><span data-stu-id="a7895-243">2.6.18-348</span></span> |
| <span data-ttu-id="a7895-244">5.10</span><span class="sxs-lookup"><span data-stu-id="a7895-244">5.10</span></span> | <span data-ttu-id="a7895-245">2.6.18-371</span><span class="sxs-lookup"><span data-stu-id="a7895-245">2.6.18-371</span></span> |
| <span data-ttu-id="a7895-246">5.11</span><span class="sxs-lookup"><span data-stu-id="a7895-246">5.11</span></span> | <span data-ttu-id="a7895-247">2.6.18-398</span><span class="sxs-lookup"><span data-stu-id="a7895-247">2.6.18-398</span></span> <br> <span data-ttu-id="a7895-248">2.6.18-400</span><span class="sxs-lookup"><span data-stu-id="a7895-248">2.6.18-400</span></span> <br><span data-ttu-id="a7895-249">2.6.18-402</span><span class="sxs-lookup"><span data-stu-id="a7895-249">2.6.18-402</span></span> <br><span data-ttu-id="a7895-250">2.6.18-404</span><span class="sxs-lookup"><span data-stu-id="a7895-250">2.6.18-404</span></span> <br><span data-ttu-id="a7895-251">2.6.18-406</span><span class="sxs-lookup"><span data-stu-id="a7895-251">2.6.18-406</span></span> <br> <span data-ttu-id="a7895-252">2.6.18-407</span><span class="sxs-lookup"><span data-stu-id="a7895-252">2.6.18-407</span></span> <br> <span data-ttu-id="a7895-253">2.6.18-408</span><span class="sxs-lookup"><span data-stu-id="a7895-253">2.6.18-408</span></span> <br> <span data-ttu-id="a7895-254">2.6.18-409</span><span class="sxs-lookup"><span data-stu-id="a7895-254">2.6.18-409</span></span> <br> <span data-ttu-id="a7895-255">2.6.18-410</span><span class="sxs-lookup"><span data-stu-id="a7895-255">2.6.18-410</span></span> <br> <span data-ttu-id="a7895-256">2.6.18-411</span><span class="sxs-lookup"><span data-stu-id="a7895-256">2.6.18-411</span></span> <br> <span data-ttu-id="a7895-257">2.6.18-412</span><span class="sxs-lookup"><span data-stu-id="a7895-257">2.6.18-412</span></span> <br> <span data-ttu-id="a7895-258">2.6.18-416</span><span class="sxs-lookup"><span data-stu-id="a7895-258">2.6.18-416</span></span> <br> <span data-ttu-id="a7895-259">2.6.18-417</span><span class="sxs-lookup"><span data-stu-id="a7895-259">2.6.18-417</span></span> <br> <span data-ttu-id="a7895-260">2.6.18-419</span><span class="sxs-lookup"><span data-stu-id="a7895-260">2.6.18-419</span></span> |

#### <a name="oracle-enterprise-linux-with-unbreakable-enterprise-kernel"></a><span data-ttu-id="a7895-261">Oracle Linux Enterprise s nedělitelné Enterprise jádra</span><span class="sxs-lookup"><span data-stu-id="a7895-261">Oracle Enterprise Linux with Unbreakable Enterprise Kernel</span></span>

#### <a name="oracle-linux-6"></a><span data-ttu-id="a7895-262">Oracle Linux 6</span><span class="sxs-lookup"><span data-stu-id="a7895-262">Oracle Linux 6</span></span>

| <span data-ttu-id="a7895-263">**Verze operačního systému**</span><span class="sxs-lookup"><span data-stu-id="a7895-263">**OS version**</span></span> | <span data-ttu-id="a7895-264">**Verze jádra**</span><span class="sxs-lookup"><span data-stu-id="a7895-264">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="a7895-265">6.2</span><span class="sxs-lookup"><span data-stu-id="a7895-265">6.2</span></span> | <span data-ttu-id="a7895-266">Oracle 2.6.32-300 (UEK R1)</span><span class="sxs-lookup"><span data-stu-id="a7895-266">Oracle 2.6.32-300 (UEK R1)</span></span> |
| <span data-ttu-id="a7895-267">6.3</span><span class="sxs-lookup"><span data-stu-id="a7895-267">6.3</span></span> | <span data-ttu-id="a7895-268">Oracle 2.6.39-200 (UEK R2)</span><span class="sxs-lookup"><span data-stu-id="a7895-268">Oracle 2.6.39-200 (UEK R2)</span></span> |
| <span data-ttu-id="a7895-269">6.4</span><span class="sxs-lookup"><span data-stu-id="a7895-269">6.4</span></span> | <span data-ttu-id="a7895-270">Oracle 2.6.39-400 (UEK R2)</span><span class="sxs-lookup"><span data-stu-id="a7895-270">Oracle 2.6.39-400 (UEK R2)</span></span> |
| <span data-ttu-id="a7895-271">6.5</span><span class="sxs-lookup"><span data-stu-id="a7895-271">6.5</span></span> | <span data-ttu-id="a7895-272">Oracle 2.6.39-400 (UEK R2 i386)</span><span class="sxs-lookup"><span data-stu-id="a7895-272">Oracle 2.6.39-400 (UEK R2 i386)</span></span> |
| <span data-ttu-id="a7895-273">6.6</span><span class="sxs-lookup"><span data-stu-id="a7895-273">6.6</span></span> | <span data-ttu-id="a7895-274">Oracle 2.6.39-400 (UEK R2 i386)</span><span class="sxs-lookup"><span data-stu-id="a7895-274">Oracle 2.6.39-400 (UEK R2 i386)</span></span> |

#### <a name="oracle-linux-5"></a><span data-ttu-id="a7895-275">Oracle Linux 5</span><span class="sxs-lookup"><span data-stu-id="a7895-275">Oracle Linux 5</span></span>

| <span data-ttu-id="a7895-276">**Verze operačního systému**</span><span class="sxs-lookup"><span data-stu-id="a7895-276">**OS version**</span></span> | <span data-ttu-id="a7895-277">**Verze jádra**</span><span class="sxs-lookup"><span data-stu-id="a7895-277">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="a7895-278">5.8</span><span class="sxs-lookup"><span data-stu-id="a7895-278">5.8</span></span> | <span data-ttu-id="a7895-279">Oracle 2.6.32-300 (UEK R1)</span><span class="sxs-lookup"><span data-stu-id="a7895-279">Oracle 2.6.32-300 (UEK R1)</span></span> |
| <span data-ttu-id="a7895-280">5.9</span><span class="sxs-lookup"><span data-stu-id="a7895-280">5.9</span></span> | <span data-ttu-id="a7895-281">Oracle 2.6.39-300 (UEK R2)</span><span class="sxs-lookup"><span data-stu-id="a7895-281">Oracle 2.6.39-300 (UEK R2)</span></span> |
| <span data-ttu-id="a7895-282">5.10</span><span class="sxs-lookup"><span data-stu-id="a7895-282">5.10</span></span> | <span data-ttu-id="a7895-283">Oracle 2.6.39-400 (UEK R2)</span><span class="sxs-lookup"><span data-stu-id="a7895-283">Oracle 2.6.39-400 (UEK R2)</span></span> |
| <span data-ttu-id="a7895-284">5.11</span><span class="sxs-lookup"><span data-stu-id="a7895-284">5.11</span></span> | <span data-ttu-id="a7895-285">Oracle 2.6.39-400 (UEK R2)</span><span class="sxs-lookup"><span data-stu-id="a7895-285">Oracle 2.6.39-400 (UEK R2)</span></span> |

#### <a name="suse-linux-enterprise-server"></a><span data-ttu-id="a7895-286">SUSE Linux Enterprise Server</span><span class="sxs-lookup"><span data-stu-id="a7895-286">SUSE Linux Enterprise Server</span></span>

#### <a name="suse-linux-11"></a><span data-ttu-id="a7895-287">SUSE Linux 11</span><span class="sxs-lookup"><span data-stu-id="a7895-287">SUSE Linux 11</span></span>

| <span data-ttu-id="a7895-288">**Verze operačního systému**</span><span class="sxs-lookup"><span data-stu-id="a7895-288">**OS version**</span></span> | <span data-ttu-id="a7895-289">**Verze jádra**</span><span class="sxs-lookup"><span data-stu-id="a7895-289">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="a7895-290">11</span><span class="sxs-lookup"><span data-stu-id="a7895-290">11</span></span> | <span data-ttu-id="a7895-291">2.6.27</span><span class="sxs-lookup"><span data-stu-id="a7895-291">2.6.27</span></span> |
| <span data-ttu-id="a7895-292">11 SP1</span><span class="sxs-lookup"><span data-stu-id="a7895-292">11 SP1</span></span> | <span data-ttu-id="a7895-293">2.6.32</span><span class="sxs-lookup"><span data-stu-id="a7895-293">2.6.32</span></span> |
| <span data-ttu-id="a7895-294">11 SP2</span><span class="sxs-lookup"><span data-stu-id="a7895-294">11 SP2</span></span> | <span data-ttu-id="a7895-295">3.0.13</span><span class="sxs-lookup"><span data-stu-id="a7895-295">3.0.13</span></span> |
| <span data-ttu-id="a7895-296">11 SP3</span><span class="sxs-lookup"><span data-stu-id="a7895-296">11 SP3</span></span> | <span data-ttu-id="a7895-297">3.0.76</span><span class="sxs-lookup"><span data-stu-id="a7895-297">3.0.76</span></span> |
| <span data-ttu-id="a7895-298">11 SP4</span><span class="sxs-lookup"><span data-stu-id="a7895-298">11 SP4</span></span> | <span data-ttu-id="a7895-299">3.0.101</span><span class="sxs-lookup"><span data-stu-id="a7895-299">3.0.101</span></span> |

#### <a name="suse-linux-10"></a><span data-ttu-id="a7895-300">SUSE Linux 10</span><span class="sxs-lookup"><span data-stu-id="a7895-300">SUSE Linux 10</span></span>

| <span data-ttu-id="a7895-301">**Verze operačního systému**</span><span class="sxs-lookup"><span data-stu-id="a7895-301">**OS version**</span></span> | <span data-ttu-id="a7895-302">**Verze jádra**</span><span class="sxs-lookup"><span data-stu-id="a7895-302">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="a7895-303">10 SP4</span><span class="sxs-lookup"><span data-stu-id="a7895-303">10 SP4</span></span> | <span data-ttu-id="a7895-304">2.6.16.60</span><span class="sxs-lookup"><span data-stu-id="a7895-304">2.6.16.60</span></span> |

#### <a name="dependency-agent-downloads"></a><span data-ttu-id="a7895-305">Agent služby Dependency soubory ke stažení</span><span class="sxs-lookup"><span data-stu-id="a7895-305">Dependency Agent downloads</span></span>

| <span data-ttu-id="a7895-306">**File**</span><span class="sxs-lookup"><span data-stu-id="a7895-306">**File**</span></span> | <span data-ttu-id="a7895-307">**OPERAČNÍ SYSTÉM**</span><span class="sxs-lookup"><span data-stu-id="a7895-307">**OS**</span></span> | <span data-ttu-id="a7895-308">**Verze**</span><span class="sxs-lookup"><span data-stu-id="a7895-308">**Version**</span></span> | <span data-ttu-id="a7895-309">**ALGORITMUS SHA-256**</span><span class="sxs-lookup"><span data-stu-id="a7895-309">**SHA-256**</span></span> |
| --- | --- | --- | --- |
| [<span data-ttu-id="a7895-310">InstallDependencyAgent Windows.exe</span><span class="sxs-lookup"><span data-stu-id="a7895-310">InstallDependencyAgent-Windows.exe</span></span>](https://aka.ms/dependencyagentwindows) | <span data-ttu-id="a7895-311">Windows</span><span class="sxs-lookup"><span data-stu-id="a7895-311">Windows</span></span> | <span data-ttu-id="a7895-312">9.0.5</span><span class="sxs-lookup"><span data-stu-id="a7895-312">9.0.5</span></span> | <span data-ttu-id="a7895-313">73B3F6A2A76A08D58F72A550947FF839B588591C48E6EDDD6DDF73AA3FD82B43</span><span class="sxs-lookup"><span data-stu-id="a7895-313">73B3F6A2A76A08D58F72A550947FF839B588591C48E6EDDD6DDF73AA3FD82B43</span></span> |
| [<span data-ttu-id="a7895-314">InstallDependencyAgent Linux64.bin</span><span class="sxs-lookup"><span data-stu-id="a7895-314">InstallDependencyAgent-Linux64.bin</span></span>](https://aka.ms/dependencyagentlinux) | <span data-ttu-id="a7895-315">Linux</span><span class="sxs-lookup"><span data-stu-id="a7895-315">Linux</span></span> | <span data-ttu-id="a7895-316">9.0.5</span><span class="sxs-lookup"><span data-stu-id="a7895-316">9.0.5</span></span> | <span data-ttu-id="a7895-317">A1BAD0B36EBF79F2B69113A07FCF48C68D90BD169C722689F9C83C69FC032371</span><span class="sxs-lookup"><span data-stu-id="a7895-317">A1BAD0B36EBF79F2B69113A07FCF48C68D90BD169C722689F9C83C69FC032371</span></span> |



## <a name="configuration"></a><span data-ttu-id="a7895-318">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="a7895-318">Configuration</span></span>

<span data-ttu-id="a7895-319">Proveďte následující kroky tooconfigure hello Data kabelové sítě řešení pro vaše pracovní prostory hello.</span><span class="sxs-lookup"><span data-stu-id="a7895-319">Perform hello following steps tooconfigure hello Wire Data solution for your workspaces.</span></span>

1. <span data-ttu-id="a7895-320">Povolit hello analýzy protokolů aktivity řešení z hello [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WireData2OMS?tab=Overview) nebo pomocí hello procesu popsaného v tématu [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="a7895-320">Enable hello Activity Log Analytics solution from hello [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WireData2OMS?tab=Overview) or by using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span>
2. <span data-ttu-id="a7895-321">Nainstalujte hello Agent služby Dependency na každém počítači, kde chcete tooget data.</span><span class="sxs-lookup"><span data-stu-id="a7895-321">Install hello Dependency Agent on each computer where you want tooget data.</span></span> <span data-ttu-id="a7895-322">Hello Agent služby Dependency můžete sledovat okolí tooimmediate připojení, takže nemusí potřebovat agenta, v každém počítači.</span><span class="sxs-lookup"><span data-stu-id="a7895-322">hello Dependency Agent can monitor connections tooimmediate neighbors, so you might not need an agent on every computer.</span></span>

### <a name="install-hello-dependency-agent-on-windows"></a><span data-ttu-id="a7895-323">Nainstalujte hello Agent služby Dependency na systému Windows</span><span class="sxs-lookup"><span data-stu-id="a7895-323">Install hello Dependency Agent on Windows</span></span>

<span data-ttu-id="a7895-324">Oprávnění správce se vyžaduje tooinstall nebo odinstalujte agenta hello.</span><span class="sxs-lookup"><span data-stu-id="a7895-324">Administrator privileges are required tooinstall or uninstall hello agent.</span></span>

<span data-ttu-id="a7895-325">Hello Agent služby Dependency nainstalovaný na počítačích se systémem Windows prostřednictvím InstallDependencyAgent Windows.exe.</span><span class="sxs-lookup"><span data-stu-id="a7895-325">hello Dependency Agent is installed on computers running Windows through InstallDependencyAgent-Windows.exe.</span></span> <span data-ttu-id="a7895-326">Pokud spustíte tento spustitelný soubor, bez jakékoli možnosti, spustí průvodce, můžete postupovat podle tooinstall interaktivně.</span><span class="sxs-lookup"><span data-stu-id="a7895-326">If you run this executable file without any options, it starts a wizard that you can follow tooinstall interactively.</span></span>

<span data-ttu-id="a7895-327">Použijte následující postup tooinstall hello Agent služby Dependency na každém počítači se systémem Windows hello:</span><span class="sxs-lookup"><span data-stu-id="a7895-327">Use hello following steps tooinstall hello Dependency Agent on each computer running Windows:</span></span>

1. <span data-ttu-id="a7895-328">Instalace agenta OMS hello pomocí pokynů hello [toohello počítače připojit Windows analýzy protokolů služby ve službě Azure](log-analytics-windows-agents.md).</span><span class="sxs-lookup"><span data-stu-id="a7895-328">Install hello OMS Agent by using hello instructions at [Connect Windows computers toohello Log Analytics service in Azure](log-analytics-windows-agents.md).</span></span>
2. <span data-ttu-id="a7895-329">Stáhnout agenta pro Windows hello pomocí hello odkaz v předchozí části hello a spusťte jej pomocí hello následující příkaz: InstallDependencyAgent Windows.exe</span><span class="sxs-lookup"><span data-stu-id="a7895-329">Download hello Windows agent using hello link in hello previous section and then run it by using hello following command: InstallDependencyAgent-Windows.exe</span></span>
3. <span data-ttu-id="a7895-330">Postupujte podle hello Průvodce tooinstall hello agenta.</span><span class="sxs-lookup"><span data-stu-id="a7895-330">Follow hello wizard tooinstall hello agent.</span></span>
4. <span data-ttu-id="a7895-331">V případě selhání toostart hello Agent služby Dependency protokolech hello podrobné informace o chybě.</span><span class="sxs-lookup"><span data-stu-id="a7895-331">If hello Dependency Agent fails toostart, check hello logs for detailed error information.</span></span> <span data-ttu-id="a7895-332">Na agentech Windows hello adresář protokolu je %Programfiles%\Microsoft Agent\logs závislostí.</span><span class="sxs-lookup"><span data-stu-id="a7895-332">On Windows agents, hello log directory is %Programfiles%\Microsoft Dependency Agent\logs.</span></span>

#### <a name="windows-command-line"></a><span data-ttu-id="a7895-333">Příkazový řádek systému Windows</span><span class="sxs-lookup"><span data-stu-id="a7895-333">Windows command line</span></span>

<span data-ttu-id="a7895-334">Použijte možnosti hello následující tabulky tooinstall z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="a7895-334">Use options from hello following table tooinstall from a command line.</span></span> <span data-ttu-id="a7895-335">toosee seznam hello instalace příznaky, spusťte instalační program hello pomocí hello /?</span><span class="sxs-lookup"><span data-stu-id="a7895-335">toosee a list of hello installation flags, run hello installer by using hello /?</span></span> <span data-ttu-id="a7895-336">Příznak následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="a7895-336">flag as follows.</span></span>

<span data-ttu-id="a7895-337">InstallDependencyAgent Windows.exe /?</span><span class="sxs-lookup"><span data-stu-id="a7895-337">InstallDependencyAgent-Windows.exe /?</span></span>

| <span data-ttu-id="a7895-338">**Příznak**</span><span class="sxs-lookup"><span data-stu-id="a7895-338">**Flag**</span></span> | <span data-ttu-id="a7895-339">**Popis**</span><span class="sxs-lookup"><span data-stu-id="a7895-339">**Description**</span></span> |
| --- | --- |
| <code>/?</code> | <span data-ttu-id="a7895-340">Získejte seznam možností příkazového řádku hello.</span><span class="sxs-lookup"><span data-stu-id="a7895-340">Get a list of hello command-line options.</span></span> |
| <code>/S</code> | <span data-ttu-id="a7895-341">Proveďte bezobslužnou instalaci s žádné uživatelské výzvy.</span><span class="sxs-lookup"><span data-stu-id="a7895-341">Perform a silent installation with no user prompts.</span></span> |

<span data-ttu-id="a7895-342">Soubory pro hello Agent služby Dependency Windows jsou umístěny v Agent služby Dependency C:\Program Files\Microsoft ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="a7895-342">Files for hello Windows Dependency Agent are placed in C:\Program Files\Microsoft Dependency Agent by default.</span></span>

### <a name="install-hello-dependency-agent-on-linux"></a><span data-ttu-id="a7895-343">Instalace hello Agent služby Dependency na platformě Linux</span><span class="sxs-lookup"><span data-stu-id="a7895-343">Install hello Dependency Agent on Linux</span></span>

<span data-ttu-id="a7895-344">Kořenový přístup je požadovaná tooinstall nebo konfiguraci agenta hello.</span><span class="sxs-lookup"><span data-stu-id="a7895-344">Root access is required tooinstall or configure hello agent.</span></span>

<span data-ttu-id="a7895-345">Hello Agent služby Dependency nainstalovaný na počítače se systémem Linux prostřednictvím InstallDependencyAgent-Linux64.bin, skript prostředí s samorozbalující binární.</span><span class="sxs-lookup"><span data-stu-id="a7895-345">hello Dependency Agent is installed on Linux computers through InstallDependencyAgent-Linux64.bin, a shell script with a self-extracting binary.</span></span> <span data-ttu-id="a7895-346">Soubor hello můžete spustit pomocí _dílet_ nebo přidejte provést samotný soubor toohello oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a7895-346">You can run hello file by using _sh_ or add execute permissions toohello file itself.</span></span>

<span data-ttu-id="a7895-347">Použijte následující postup tooinstall hello Agent služby Dependency na každý počítač se systémem Linux hello:</span><span class="sxs-lookup"><span data-stu-id="a7895-347">Use hello following steps tooinstall hello Dependency Agent on each Linux computer:</span></span>

1. <span data-ttu-id="a7895-348">Instalace agenta OMS hello pomocí pokynů hello [shromažďování a správě dat z počítače se systémem Linux](log-analytics-agent-linux.md).</span><span class="sxs-lookup"><span data-stu-id="a7895-348">Install hello OMS Agent by using hello instructions at [Collect and manage data from Linux computers](log-analytics-agent-linux.md).</span></span>
2. <span data-ttu-id="a7895-349">Agent služby Linux Dependency hello pomocí hello odkaz v předchozí části hello stáhněte a nainstalujte ji jako kořenová pomocí hello následující příkaz: dílet InstallDependencyAgent Linux64.bin</span><span class="sxs-lookup"><span data-stu-id="a7895-349">Download hello Linux Dependency agent using hello link in hello previous section and then install it as root by using hello following command: sh InstallDependencyAgent-Linux64.bin</span></span>
3. <span data-ttu-id="a7895-350">V případě selhání toostart hello Agent služby Dependency protokolech hello podrobné informace o chybě.</span><span class="sxs-lookup"><span data-stu-id="a7895-350">If hello Dependency Agent fails toostart, check hello logs for detailed error information.</span></span> <span data-ttu-id="a7895-351">Na agentech Linux adresář protokolu hello je: /var/opt/microsoft/dependency-agent/log.</span><span class="sxs-lookup"><span data-stu-id="a7895-351">On Linux agents, hello log directory is: /var/opt/microsoft/dependency-agent/log.</span></span>

<span data-ttu-id="a7895-352">toosee seznam hello instalace příznaky, spustit instalační program hello s hello `-help` příznak následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="a7895-352">toosee a list of hello installation flags, run hello installation program with hello `-help` flag as follows.</span></span>

```
InstallDependencyAgent-Linux64.bin -help
```

| <span data-ttu-id="a7895-353">**Příznak**</span><span class="sxs-lookup"><span data-stu-id="a7895-353">**Flag**</span></span> | <span data-ttu-id="a7895-354">**Popis**</span><span class="sxs-lookup"><span data-stu-id="a7895-354">**Description**</span></span> |
| --- | --- |
| <code>-help</code> | <span data-ttu-id="a7895-355">Získejte seznam možností příkazového řádku hello.</span><span class="sxs-lookup"><span data-stu-id="a7895-355">Get a list of hello command-line options.</span></span> |
| <code>-s</code> | <span data-ttu-id="a7895-356">Proveďte bezobslužnou instalaci s žádné uživatelské výzvy.</span><span class="sxs-lookup"><span data-stu-id="a7895-356">Perform a silent installation with no user prompts.</span></span> |
| <code>--check</code> | <span data-ttu-id="a7895-357">Zkontrolujte oprávnění a hello operačního systému, ale není nainstalovaný hello agent.</span><span class="sxs-lookup"><span data-stu-id="a7895-357">Check permissions and hello operating system but do not install hello agent.</span></span> |

<span data-ttu-id="a7895-358">Soubory pro hello Agent služby Dependency jsou umístěny v hello následující adresáře:</span><span class="sxs-lookup"><span data-stu-id="a7895-358">Files for hello Dependency Agent are placed in hello following directories:</span></span>

| <span data-ttu-id="a7895-359">**Soubory**</span><span class="sxs-lookup"><span data-stu-id="a7895-359">**Files**</span></span> | <span data-ttu-id="a7895-360">**Umístění**</span><span class="sxs-lookup"><span data-stu-id="a7895-360">**Location**</span></span> |
| --- | --- |
| <span data-ttu-id="a7895-361">Soubory jádra</span><span class="sxs-lookup"><span data-stu-id="a7895-361">Core files</span></span> | <span data-ttu-id="a7895-362">/OPT/Microsoft/Dependency-Agent</span><span class="sxs-lookup"><span data-stu-id="a7895-362">/opt/microsoft/dependency-agent</span></span> |
| <span data-ttu-id="a7895-363">Soubory protokolu</span><span class="sxs-lookup"><span data-stu-id="a7895-363">Log files</span></span> | <span data-ttu-id="a7895-364">/var/OPT/Microsoft/Dependency-Agent/log</span><span class="sxs-lookup"><span data-stu-id="a7895-364">/var/opt/microsoft/dependency-agent/log</span></span> |
| <span data-ttu-id="a7895-365">Konfigurační soubory</span><span class="sxs-lookup"><span data-stu-id="a7895-365">Config files</span></span> | <span data-ttu-id="a7895-366">/ETC/OPT/Microsoft/Dependency-Agent/config</span><span class="sxs-lookup"><span data-stu-id="a7895-366">/etc/opt/microsoft/dependency-agent/config</span></span> |
| <span data-ttu-id="a7895-367">Služby spustitelné soubory</span><span class="sxs-lookup"><span data-stu-id="a7895-367">Service executable files</span></span> | <span data-ttu-id="a7895-368">/OPT/Microsoft/Dependency-Agent/Bin/Microsoft-Dependency-Agent</span><span class="sxs-lookup"><span data-stu-id="a7895-368">/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent</span></span><br><br><span data-ttu-id="a7895-369">/OPT/Microsoft/Dependency-Agent/Bin/Microsoft-Dependency-Agent-Manager</span><span class="sxs-lookup"><span data-stu-id="a7895-369">/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager</span></span> |
| <span data-ttu-id="a7895-370">Úložiště binární soubory</span><span class="sxs-lookup"><span data-stu-id="a7895-370">Binary storage files</span></span> | <span data-ttu-id="a7895-371">/var/OPT/Microsoft/Dependency-Agent/Storage</span><span class="sxs-lookup"><span data-stu-id="a7895-371">/var/opt/microsoft/dependency-agent/storage</span></span> |

### <a name="installation-script-examples"></a><span data-ttu-id="a7895-372">Příklady skriptů instalace</span><span class="sxs-lookup"><span data-stu-id="a7895-372">Installation script examples</span></span>

<span data-ttu-id="a7895-373">tooeasily nasadit hello Agent služby Dependency na mnoha serverech najednou, pomáhá toouse skriptu.</span><span class="sxs-lookup"><span data-stu-id="a7895-373">tooeasily deploy hello Dependency Agent on many servers at once, it helps toouse a script.</span></span> <span data-ttu-id="a7895-374">Můžete použít následující skript příklady toodownload hello a nainstalovat hello Agent služby Dependency na systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="a7895-374">You can use hello following script examples toodownload and install hello Dependency Agent on either Windows or Linux.</span></span>

#### <a name="powershell-script-for-windows"></a><span data-ttu-id="a7895-375">Skript prostředí PowerShell pro systém Windows</span><span class="sxs-lookup"><span data-stu-id="a7895-375">PowerShell script for Windows</span></span>

```PowerShell

Invoke-WebRequest &quot;https://aka.ms/dependencyagentwindows&quot; -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S

```

#### <a name="shell-script-for-linux"></a><span data-ttu-id="a7895-376">Skript prostředí pro Linux</span><span class="sxs-lookup"><span data-stu-id="a7895-376">Shell script for Linux</span></span>

```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
```

```
sh InstallDependencyAgent-Linux64.bin -s
```

### <a name="desired-state-configuration"></a><span data-ttu-id="a7895-377">Konfigurace požadovaného stavu</span><span class="sxs-lookup"><span data-stu-id="a7895-377">Desired State Configuration</span></span>

<span data-ttu-id="a7895-378">toodeploy hello Agent služby Dependency prostřednictvím konfigurace požadovaného stavu, můžete použít modul xPSDesiredStateConfiguration hello a bit kódu jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="a7895-378">toodeploy hello Dependency Agent via Desired State Configuration, you can use hello xPSDesiredStateConfiguration module and a bit of code like hello following:</span></span>

```
Import-DscResource -ModuleName xPSDesiredStateConfiguration

$DAPackageLocalPath = &quot;C:\InstallDependencyAgent-Windows.exe&quot;



Node $NodeName

{

    # Download and install hello Dependency Agent

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
### <a name="uninstall-hello-dependency-agent"></a><span data-ttu-id="a7895-379">Odinstalujte hello Agent služby Dependency</span><span class="sxs-lookup"><span data-stu-id="a7895-379">Uninstall hello Dependency Agent</span></span>

<span data-ttu-id="a7895-380">Použijte následující oddíly toohelp odebrat hello Agent služby Dependency hello.</span><span class="sxs-lookup"><span data-stu-id="a7895-380">Use hello following sections toohelp you remove hello Dependency Agent.</span></span>

#### <a name="uninstall-hello-dependency-agent-on-windows"></a><span data-ttu-id="a7895-381">Odinstalujte hello Agent služby Dependency v systému Windows</span><span class="sxs-lookup"><span data-stu-id="a7895-381">Uninstall hello Dependency Agent on Windows</span></span>

<span data-ttu-id="a7895-382">Správce můžete odinstalovat hello závislostí agenta pro Windows pomocí ovládacích panelů.</span><span class="sxs-lookup"><span data-stu-id="a7895-382">An administrator can uninstall hello Dependency Agent for Windows through Control Panel.</span></span>

<span data-ttu-id="a7895-383">Správce můžete také spouštět %Programfiles%\Microsoft závislostí Agent\Uninstall.exe toouninstall hello Agent služby Dependency.</span><span class="sxs-lookup"><span data-stu-id="a7895-383">An administrator can also run %Programfiles%\Microsoft Dependency Agent\Uninstall.exe toouninstall hello Dependency Agent.</span></span>

#### <a name="uninstall-hello-dependency-agent-on-linux"></a><span data-ttu-id="a7895-384">Odinstalujte hello Agent služby Dependency na systému Linux</span><span class="sxs-lookup"><span data-stu-id="a7895-384">Uninstall hello Dependency Agent on Linux</span></span>

<span data-ttu-id="a7895-385">Odinstalace toocompletely hello Agent služby Dependency ze systému Linux, je nutné odstranit vlastní agent hello a hello konektor, který se instaluje automaticky s agentem hello.</span><span class="sxs-lookup"><span data-stu-id="a7895-385">toocompletely uninstall hello Dependency Agent from Linux, you must remove hello agent itself and hello connector, which is installed automatically with hello agent.</span></span> <span data-ttu-id="a7895-386">Můžete odinstalovat i pomocí hello následující jeden příkaz:</span><span class="sxs-lookup"><span data-stu-id="a7895-386">You can uninstall both by using hello following single command:</span></span>

```
rpm -e dependency-agent dependency-agent-connector
```

## <a name="management-packs"></a><span data-ttu-id="a7895-387">Sady Management Pack</span><span class="sxs-lookup"><span data-stu-id="a7895-387">Management packs</span></span>

<span data-ttu-id="a7895-388">Po aktivaci Data kabelové sítě v pracovním prostoru analýzy protokolů 300 KB management pack se odesílají servery Windows hello tooall v něm.</span><span class="sxs-lookup"><span data-stu-id="a7895-388">When Wire Data is activated in a Log Analytics workspace, a 300-KB management pack is sent tooall hello Windows servers in that workspace.</span></span> <span data-ttu-id="a7895-389">Pokud používáte System Center Operations Manager agentů v [připojené skupiny pro správu](log-analytics-om-agents.md), z System Center Operations Manager je nasazena hello sady management pack monitorování závislostí.</span><span class="sxs-lookup"><span data-stu-id="a7895-389">If you are using System Center Operations Manager agents in a [connected management group](log-analytics-om-agents.md), hello Dependency Monitor management pack is deployed from System Center Operations Manager.</span></span> <span data-ttu-id="a7895-390">Pokud jsou připojeny přímo hello agentů, analýzy protokolů přináší hello sady management pack.</span><span class="sxs-lookup"><span data-stu-id="a7895-390">If hello agents are directly connected, Log Analytics delivers hello management pack.</span></span>

<span data-ttu-id="a7895-391">Sada management pack Hello jmenuje Microsoft.IntelligencePacks.ApplicationDependencyMonitor.</span><span class="sxs-lookup"><span data-stu-id="a7895-391">hello management pack is named Microsoft.IntelligencePacks.ApplicationDependencyMonitor.</span></span> <span data-ttu-id="a7895-392">Je zapsán do: %Programfiles%\Microsoft monitorování Agent\Agent\Health služby State\Management balíčky.</span><span class="sxs-lookup"><span data-stu-id="a7895-392">It's written to: %Programfiles%\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs.</span></span> <span data-ttu-id="a7895-393">Hello zdroj dat, který používá sada management pack hello je: % Program files%\Microsoft monitorování Agent\Agent\Health služby State\Resources&lt;AutoGeneratedID&gt;\ Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.</span><span class="sxs-lookup"><span data-stu-id="a7895-393">hello data source that hello management pack uses is: %Program files%\Microsoft Monitoring Agent\Agent\Health Service State\Resources&lt;AutoGeneratedID&gt;\Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.</span></span>

## <a name="using-hello-solution"></a><span data-ttu-id="a7895-394">Pomocí řešení hello</span><span class="sxs-lookup"><span data-stu-id="a7895-394">Using hello solution</span></span>

<span data-ttu-id="a7895-395">**Instalace a konfigurace řešení hello**</span><span class="sxs-lookup"><span data-stu-id="a7895-395">**Installing and configuring hello solution**</span></span>

<span data-ttu-id="a7895-396">Použijte následující informace tooinstall hello a nakonfigurujte hello řešení.</span><span class="sxs-lookup"><span data-stu-id="a7895-396">Use hello following information tooinstall and configure hello solution.</span></span>

- <span data-ttu-id="a7895-397">Hello Data kabelové sítě řešení operace čtení dat z počítačů se systémem Windows Server 2012 R2, Windows 8.1 a novější operační systémy.</span><span class="sxs-lookup"><span data-stu-id="a7895-397">hello Wire Data solution acquires data from computers running Windows Server 2012 R2, Windows 8.1, and later operating systems.</span></span>
- <span data-ttu-id="a7895-398">Na počítačích, kam chcete data kabelové sítě tooacquire z se vyžaduje rozhraní Microsoft .NET Framework 4.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="a7895-398">Microsoft .NET Framework 4.0 or later is required on computers where you want tooacquire wire data from.</span></span>
- <span data-ttu-id="a7895-399">Přidat hello Data kabelové sítě řešení tooyour pracovní prostor analýzy protokolů pomocí hello procesu popsaného v tématu [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="a7895-399">Add hello Wire Data solution tooyour Log Analytics workspace using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span> <span data-ttu-id="a7895-400">Není nutná žádná další konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a7895-400">There is no further configuration required.</span></span>
- <span data-ttu-id="a7895-401">Pokud chcete data kabelové sítě tooview pro konkrétní řešení, je nutné řešení hello toohave již přidán tooyour prostoru.</span><span class="sxs-lookup"><span data-stu-id="a7895-401">If you want tooview wire data for a specific solution, you need toohave hello solution already added tooyour workspace.</span></span>

<span data-ttu-id="a7895-402">Po mají nainstalovat agenti a instalaci hello řešení, dlaždice hello 2.0 přenosu dat se zobrazí v pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="a7895-402">After you have agents installed and you install hello solution, hello Wire Data 2.0 tile appears in your workspace.</span></span>

> [!NOTE]
> <span data-ttu-id="a7895-403">V současné době je nutné použít data kabelové sítě portálu tooview hello OMS.</span><span class="sxs-lookup"><span data-stu-id="a7895-403">Currently, you must use hello OMS portal tooview wire data.</span></span> <span data-ttu-id="a7895-404">Nelze použít data kabelové sítě Azure portálu tooview hello.</span><span class="sxs-lookup"><span data-stu-id="a7895-404">You cannot use hello Azure portal tooview wire data.</span></span>

![Dlaždice Data kabelové sítě](./media/log-analytics-wire-data/wire-data-tile.png)

## <a name="using-hello-wire-data-20-solution"></a><span data-ttu-id="a7895-406">Pomocí řešení hello 2.0 přenosu dat</span><span class="sxs-lookup"><span data-stu-id="a7895-406">Using hello Wire Data 2.0 solution</span></span>

<span data-ttu-id="a7895-407">Na portálu OMS hello, klikněte na tlačítko hello **přenosu dat 2.0** řídicí panel dlaždice tooopen hello Data kabelové sítě.</span><span class="sxs-lookup"><span data-stu-id="a7895-407">In hello OMS portal, click hello **Wire Data 2.0** tile tooopen hello Wire Data dashboard.</span></span> <span data-ttu-id="a7895-408">řídicí panel Hello zahrnuje hello oken v hello následující tabulka.</span><span class="sxs-lookup"><span data-stu-id="a7895-408">hello dashboard includes hello blades in hello following table.</span></span> <span data-ttu-id="a7895-409">Každý okno uvádí až too10 položky odpovídající, aby na okno kritéria pro hello zadán oboru a časový rozsah.</span><span class="sxs-lookup"><span data-stu-id="a7895-409">Each blade lists up too10 items matching that blade's criteria for hello specified scope and time range.</span></span> <span data-ttu-id="a7895-410">Můžete spustit vyhledávání protokolu, který vrátí všechny záznamy kliknutím **zobrazit všechny** dole hello v okně hello nebo kliknutím na záhlaví okna hello.</span><span class="sxs-lookup"><span data-stu-id="a7895-410">You can run a log search that returns all records by clicking **See all** at hello bottom of hello blade or by clicking hello blade header.</span></span>

| <span data-ttu-id="a7895-411">**Okno**</span><span class="sxs-lookup"><span data-stu-id="a7895-411">**Blade**</span></span> | <span data-ttu-id="a7895-412">**Popis**</span><span class="sxs-lookup"><span data-stu-id="a7895-412">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="a7895-413">Agenti zachytávající síťový přenos</span><span class="sxs-lookup"><span data-stu-id="a7895-413">Agents capturing network traffic</span></span> | <span data-ttu-id="a7895-414">Zobrazuje hello počet agentů, kteří jsou zachytávání síťového provozu a uvádí hello top 10 počítačů, které jsou zachycení provozu.</span><span class="sxs-lookup"><span data-stu-id="a7895-414">Shows hello number of agents that are capturing network traffic and lists hello top 10 computers that are capturing traffic.</span></span> <span data-ttu-id="a7895-415">Klikněte na číslo toorun hello protokolu vyhledejte <code>Type:WireData &#124; measure Sum(TotalBytes) by Computer &#124; top 500000</code>.</span><span class="sxs-lookup"><span data-stu-id="a7895-415">Click hello number toorun a log search for <code>Type:WireData &#124; measure Sum(TotalBytes) by Computer &#124; top 500000</code>.</span></span> <span data-ttu-id="a7895-416">Klikněte na počítač, v seznamu toorun hello protokolu vyhledávání vrací hello celkový počet bajtů zaznamenat.</span><span class="sxs-lookup"><span data-stu-id="a7895-416">Click a computer in hello list toorun a log search returning hello total number of bytes captured.</span></span> |
| <span data-ttu-id="a7895-417">Místní podsítě</span><span class="sxs-lookup"><span data-stu-id="a7895-417">Local Subnets</span></span> | <span data-ttu-id="a7895-418">Zobrazuje počet hello místní podsítě, které byly zjištěny agenty.</span><span class="sxs-lookup"><span data-stu-id="a7895-418">Shows hello number of local subnets that agents have discovered.</span></span>  <span data-ttu-id="a7895-419">Klikněte na číslo toorun hello protokolu vyhledejte <code>Type:WireData &#124; Measure Sum(TotalBytes) by LocalSubnet</code> , obsahuje seznam všech podsítí s hello počet bajtů odeslaných přes každé z nich.</span><span class="sxs-lookup"><span data-stu-id="a7895-419">Click hello number toorun a log search for <code>Type:WireData &#124; Measure Sum(TotalBytes) by LocalSubnet</code> that lists all subnets with hello number of bytes sent over each one.</span></span> <span data-ttu-id="a7895-420">Klikněte na podsíť v seznamu toorun hello protokolu vyhledávání vrací hello celkový počet bajtů odeslaných přes hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="a7895-420">Click a subnet in hello list toorun a log search returning hello total number of bytes sent over hello subnet.</span></span> |
| <span data-ttu-id="a7895-421">Protokoly na úrovni aplikace</span><span class="sxs-lookup"><span data-stu-id="a7895-421">Application-level Protocols</span></span> | <span data-ttu-id="a7895-422">Zobrazuje hello počet protokoly na úrovni aplikace používána, při zjištění agenty.</span><span class="sxs-lookup"><span data-stu-id="a7895-422">Shows hello number of application-level protocols in use, as discovered by agents.</span></span> <span data-ttu-id="a7895-423">Klikněte na číslo toorun hello protokolu vyhledejte <code>Type:WireData &#124; Measure Sum(TotalBytes) by ApplicationProtocol</code>.</span><span class="sxs-lookup"><span data-stu-id="a7895-423">Click hello number toorun a log search for <code>Type:WireData &#124; Measure Sum(TotalBytes) by ApplicationProtocol</code>.</span></span> <span data-ttu-id="a7895-424">Klikněte na protokol toorun protokolu vyhledávání vrací hello celkový počet bajtů odeslaných pomocí protokolu hello.</span><span class="sxs-lookup"><span data-stu-id="a7895-424">Click a protocol toorun a log search returning hello total number of bytes sent using hello protocol.</span></span> |

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Řídicí panel přenosu dat](./media/log-analytics-wire-data/wire-data-dash.png)

<span data-ttu-id="a7895-426">Můžete použít hello **agenti zachytávající síťový přenos** okno toodetermine, jaký poměr šířky pásma sítě je spotřebovávanou počítače.</span><span class="sxs-lookup"><span data-stu-id="a7895-426">You can use hello **Agents capturing network traffic** blade toodetermine how much network bandwidth is being consumed by computers.</span></span> <span data-ttu-id="a7895-427">Toto okno můžete snadno najít hello _chattiest_ počítač ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="a7895-427">This blade can help you easily find hello _chattiest_ computer in your environment.</span></span> <span data-ttu-id="a7895-428">Tyto počítače může být přetížený, funguje neobvyklým způsobem, nebo pomocí více síťových prostředků, než normální.</span><span class="sxs-lookup"><span data-stu-id="a7895-428">Such computers could be overloaded, acting abnormally, or using more network resources than normal.</span></span>

![Příklad protokolu vyhledávání](./media/log-analytics-wire-data/log-search-example01.png)

<span data-ttu-id="a7895-430">Podobně můžete použít hello **místní podsítě** okno toodetermine kolik síťový provoz je procházení podsítě.</span><span class="sxs-lookup"><span data-stu-id="a7895-430">Similarly, you can use hello **Local Subnets** blade toodetermine how much network traffic is moving through your subnets.</span></span> <span data-ttu-id="a7895-431">Uživatelé jsou často definovat podsítě kolem důležité oblasti pro své aplikace.</span><span class="sxs-lookup"><span data-stu-id="a7895-431">Users often define subnets around critical areas for their applications.</span></span> <span data-ttu-id="a7895-432">Toto okno nabízí zobrazení do těchto oblastí.</span><span class="sxs-lookup"><span data-stu-id="a7895-432">This blade offers a view into those areas.</span></span>

![Příklad protokolu vyhledávání](./media/log-analytics-wire-data/log-search-example02.png)

<span data-ttu-id="a7895-434">Hello **protokoly na úrovni aplikace** okno je užitečné, protože je užitečné vědět, co protokoly jsou používány.</span><span class="sxs-lookup"><span data-stu-id="a7895-434">hello **Application-level Protocols** blade is useful because it's helpful know what protocols are in use.</span></span> <span data-ttu-id="a7895-435">Například by se dalo očekávat SSH toonot má v použít v prostředí vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="a7895-435">For example, you might expect SSH toonot be in use in your network environment.</span></span> <span data-ttu-id="a7895-436">Zobrazení informací, které jsou k dispozici v okně hello můžete rychle potvrďte nebo disprove vaše očekávání.</span><span class="sxs-lookup"><span data-stu-id="a7895-436">Viewing information available in hello blade can quickly confirm or disprove your expectation.</span></span>

![Příklad protokolu vyhledávání](./media/log-analytics-wire-data/log-search-example03.png)

<span data-ttu-id="a7895-438">V tomto příkladu může přejít k podrobnostem počítače, které používají SSH a mnoho dalších podrobností komunikace programem SSH podrobnosti toosee.</span><span class="sxs-lookup"><span data-stu-id="a7895-438">In this example, you could drill-into SSH details toosee which computers are using SSH and many other communication details.</span></span>

![Zo výsledky hledání](./media/log-analytics-wire-data/ssh-details.png)

<span data-ttu-id="a7895-440">Je také užitečné tooknow Pokud provozu protokolu se zvýšením nebo snížením v čase.</span><span class="sxs-lookup"><span data-stu-id="a7895-440">It's also useful tooknow if protocol traffic is increasing or decreasing over time.</span></span> <span data-ttu-id="a7895-441">Například pokud je zvýšení množství hello data přenesená aplikací, který může být něco, co byste měli vědět, nebo že můžete zjistit pozoruhodné.</span><span class="sxs-lookup"><span data-stu-id="a7895-441">For example, if hello amount of data being transmitted by an application is increasing, that might be something you should be aware of, or that you might find noteworthy.</span></span>

## <a name="input-data"></a><span data-ttu-id="a7895-442">Vstupní data</span><span class="sxs-lookup"><span data-stu-id="a7895-442">Input data</span></span>

<span data-ttu-id="a7895-443">Data kabelové sítě shromažďuje metadata o síťovém provozu pomocí hello agentů, které jste povolili.</span><span class="sxs-lookup"><span data-stu-id="a7895-443">Wire data collects metadata about network traffic using hello agents that you have enabled.</span></span> <span data-ttu-id="a7895-444">Každý agent odesílá data o každých 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="a7895-444">Each agent sends data about every 15 seconds.</span></span>

## <a name="output-data"></a><span data-ttu-id="a7895-445">výstupní data</span><span class="sxs-lookup"><span data-stu-id="a7895-445">Output data</span></span>

<span data-ttu-id="a7895-446">Záznam s typem _WireData_ se vytvoří pro každý typ vstupní data.</span><span class="sxs-lookup"><span data-stu-id="a7895-446">A record with a type of _WireData_ is created for each type of input data.</span></span> <span data-ttu-id="a7895-447">WireData záznamy mají vlastnosti zobrazené v následující tabulce hello:</span><span class="sxs-lookup"><span data-stu-id="a7895-447">WireData records have properties shown in hello following table:</span></span>

| <span data-ttu-id="a7895-448">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="a7895-448">Property</span></span> | <span data-ttu-id="a7895-449">Popis</span><span class="sxs-lookup"><span data-stu-id="a7895-449">Description</span></span> |
|---|---|
| <span data-ttu-id="a7895-450">Počítač</span><span class="sxs-lookup"><span data-stu-id="a7895-450">Computer</span></span> | <span data-ttu-id="a7895-451">Název počítače, kde nebyla shromážděna data</span><span class="sxs-lookup"><span data-stu-id="a7895-451">Computer name where data was collected</span></span> |
| <span data-ttu-id="a7895-452">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="a7895-452">TimeGenerated</span></span> | <span data-ttu-id="a7895-453">Čas záznamu hello</span><span class="sxs-lookup"><span data-stu-id="a7895-453">Time of hello record</span></span> |
| <span data-ttu-id="a7895-454">LocalIP</span><span class="sxs-lookup"><span data-stu-id="a7895-454">LocalIP</span></span> | <span data-ttu-id="a7895-455">IP adresa místního počítače hello</span><span class="sxs-lookup"><span data-stu-id="a7895-455">IP address of hello local computer</span></span> |
| <span data-ttu-id="a7895-456">SessionState</span><span class="sxs-lookup"><span data-stu-id="a7895-456">SessionState</span></span> | <span data-ttu-id="a7895-457">Připojení nebo odpojení</span><span class="sxs-lookup"><span data-stu-id="a7895-457">Connected or disconnected</span></span> |
| <span data-ttu-id="a7895-458">ReceivedBytes</span><span class="sxs-lookup"><span data-stu-id="a7895-458">ReceivedBytes</span></span> | <span data-ttu-id="a7895-459">Počet přijatých bajtů</span><span class="sxs-lookup"><span data-stu-id="a7895-459">Amount of bytes received</span></span> |
| <span data-ttu-id="a7895-460">ProtocolName</span><span class="sxs-lookup"><span data-stu-id="a7895-460">ProtocolName</span></span> | <span data-ttu-id="a7895-461">Název používá protokol sítě hello</span><span class="sxs-lookup"><span data-stu-id="a7895-461">Name of hello network protocol used</span></span> |
| <span data-ttu-id="a7895-462">Parametr IPVersion</span><span class="sxs-lookup"><span data-stu-id="a7895-462">IPVersion</span></span> | <span data-ttu-id="a7895-463">Verze protokolu IP</span><span class="sxs-lookup"><span data-stu-id="a7895-463">IP version</span></span> |
| <span data-ttu-id="a7895-464">Směr</span><span class="sxs-lookup"><span data-stu-id="a7895-464">Direction</span></span> | <span data-ttu-id="a7895-465">Příchozí nebo odchozí</span><span class="sxs-lookup"><span data-stu-id="a7895-465">Inbound or outbound</span></span> |
| <span data-ttu-id="a7895-466">MaliciousIP</span><span class="sxs-lookup"><span data-stu-id="a7895-466">MaliciousIP</span></span> | <span data-ttu-id="a7895-467">IP adresa známé škodlivé zdroje</span><span class="sxs-lookup"><span data-stu-id="a7895-467">IP address of a known malicious source</span></span> |
| <span data-ttu-id="a7895-468">Závažnost</span><span class="sxs-lookup"><span data-stu-id="a7895-468">Severity</span></span> | <span data-ttu-id="a7895-469">Závažnost možného malwaru</span><span class="sxs-lookup"><span data-stu-id="a7895-469">Suspected malware severity</span></span> |
| <span data-ttu-id="a7895-470">RemoteIPCountry</span><span class="sxs-lookup"><span data-stu-id="a7895-470">RemoteIPCountry</span></span> | <span data-ttu-id="a7895-471">Země hello vzdálené IP adresy</span><span class="sxs-lookup"><span data-stu-id="a7895-471">Country of hello remote IP address</span></span> |
| <span data-ttu-id="a7895-472">ManagementGroupName</span><span class="sxs-lookup"><span data-stu-id="a7895-472">ManagementGroupName</span></span> | <span data-ttu-id="a7895-473">Název skupiny pro správu nástroje Operations Manager hello</span><span class="sxs-lookup"><span data-stu-id="a7895-473">Name of hello Operations Manager management group</span></span> |
| <span data-ttu-id="a7895-474">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="a7895-474">SourceSystem</span></span> | <span data-ttu-id="a7895-475">Zdroj, kde nebyla shromážděna data</span><span class="sxs-lookup"><span data-stu-id="a7895-475">Source where data was collected</span></span> |
| <span data-ttu-id="a7895-476">SessionStartTime</span><span class="sxs-lookup"><span data-stu-id="a7895-476">SessionStartTime</span></span> | <span data-ttu-id="a7895-477">Čas spuštění relace</span><span class="sxs-lookup"><span data-stu-id="a7895-477">Start time of session</span></span> |
| <span data-ttu-id="a7895-478">SessionEndTime</span><span class="sxs-lookup"><span data-stu-id="a7895-478">SessionEndTime</span></span> | <span data-ttu-id="a7895-479">Čas ukončení relace</span><span class="sxs-lookup"><span data-stu-id="a7895-479">End time of session</span></span> |
| <span data-ttu-id="a7895-480">LocalSubnet</span><span class="sxs-lookup"><span data-stu-id="a7895-480">LocalSubnet</span></span> | <span data-ttu-id="a7895-481">Podsíť, kde nebyla shromážděna data</span><span class="sxs-lookup"><span data-stu-id="a7895-481">Subnet where data was collected</span></span> |
| <span data-ttu-id="a7895-482">LocalPortNumber</span><span class="sxs-lookup"><span data-stu-id="a7895-482">LocalPortNumber</span></span> | <span data-ttu-id="a7895-483">Číslem místního portu</span><span class="sxs-lookup"><span data-stu-id="a7895-483">Local port number</span></span> |
| <span data-ttu-id="a7895-484">Vzdálená adresa IP</span><span class="sxs-lookup"><span data-stu-id="a7895-484">RemoteIP</span></span> | <span data-ttu-id="a7895-485">Vzdálené IP adresy používané hello vzdáleného počítače</span><span class="sxs-lookup"><span data-stu-id="a7895-485">Remote IP address used by hello remote computer</span></span> |
| <span data-ttu-id="a7895-486">RemotePortNumber</span><span class="sxs-lookup"><span data-stu-id="a7895-486">RemotePortNumber</span></span> | <span data-ttu-id="a7895-487">Číslo portu použité podle vzdálené IP adresy hello</span><span class="sxs-lookup"><span data-stu-id="a7895-487">Port number used by hello remote IP address</span></span> |
| <span data-ttu-id="a7895-488">ID relace</span><span class="sxs-lookup"><span data-stu-id="a7895-488">SessionID</span></span> | <span data-ttu-id="a7895-489">Jednoznačná hodnota, která identifikuje relace komunikace mezi dvě IP adresy</span><span class="sxs-lookup"><span data-stu-id="a7895-489">A unique value that identifies communication session between two IP addresses</span></span> |
| <span data-ttu-id="a7895-490">SentBytes</span><span class="sxs-lookup"><span data-stu-id="a7895-490">SentBytes</span></span> | <span data-ttu-id="a7895-491">Počet bajtů odeslaných</span><span class="sxs-lookup"><span data-stu-id="a7895-491">Number of bytes sent</span></span> |
| <span data-ttu-id="a7895-492">TotalBytes</span><span class="sxs-lookup"><span data-stu-id="a7895-492">TotalBytes</span></span> | <span data-ttu-id="a7895-493">Celkový počet bajtů odeslaných během relace</span><span class="sxs-lookup"><span data-stu-id="a7895-493">Total number of bytes sent during session</span></span> |
| <span data-ttu-id="a7895-494">ApplicationProtocol</span><span class="sxs-lookup"><span data-stu-id="a7895-494">ApplicationProtocol</span></span> | <span data-ttu-id="a7895-495">Typ protokolu sítě používá</span><span class="sxs-lookup"><span data-stu-id="a7895-495">Type of network protocol used</span></span>   |
| <span data-ttu-id="a7895-496">ID procesu</span><span class="sxs-lookup"><span data-stu-id="a7895-496">ProcessID</span></span> | <span data-ttu-id="a7895-497">ID procesu systému Windows</span><span class="sxs-lookup"><span data-stu-id="a7895-497">Windows process ID</span></span> |
| <span data-ttu-id="a7895-498">Název_procesu</span><span class="sxs-lookup"><span data-stu-id="a7895-498">ProcessName</span></span> | <span data-ttu-id="a7895-499">Cesta a název souboru procesu hello</span><span class="sxs-lookup"><span data-stu-id="a7895-499">Path and file name of hello process</span></span> |
| <span data-ttu-id="a7895-500">RemoteIPLongitude</span><span class="sxs-lookup"><span data-stu-id="a7895-500">RemoteIPLongitude</span></span> | <span data-ttu-id="a7895-501">Zeměpisná délka IP</span><span class="sxs-lookup"><span data-stu-id="a7895-501">IP longitude value</span></span> |
| <span data-ttu-id="a7895-502">RemoteIPLatitude</span><span class="sxs-lookup"><span data-stu-id="a7895-502">RemoteIPLatitude</span></span> | <span data-ttu-id="a7895-503">Zeměpisná šířka IP</span><span class="sxs-lookup"><span data-stu-id="a7895-503">IP latitude value</span></span> |


## <a name="next-steps"></a><span data-ttu-id="a7895-504">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a7895-504">Next steps</span></span>

- <span data-ttu-id="a7895-505">[V protokolech Hledat](log-analytics-log-searches.md) tooview podrobné záznamy přenosu dat hledání.</span><span class="sxs-lookup"><span data-stu-id="a7895-505">[Search logs](log-analytics-log-searches.md) tooview detailed wire data search records.</span></span>
