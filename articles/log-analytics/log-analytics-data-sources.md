---
title: Konfigurace zdroje dat v OMS Log Analytics | Microsoft Docs
description: "Zdroje dat zadat data, analýzy protokolů shromažďuje z agentů a dalších připojené zdroje.  Tento článek popisuje základní informace o tom, jak analýzy protokolů používá zdroje dat, vysvětluje podrobnosti o tom, jak je nakonfigurovat a poskytuje k dispozici různé datové zdroje."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 67710115-c861-40f8-a377-57c7fa6909b4
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/23/2017
ms.author: bwren
ms.openlocfilehash: 00d030a502cf70ea9a5dea767f560cdf2919573e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="data-sources-in-log-analytics"></a><span data-ttu-id="c34e2-104">Zdroje dat v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="c34e2-104">Data sources in Log Analytics</span></span>
<span data-ttu-id="c34e2-105">Analýzy protokolů shromažďuje data z připojené zdroje v pracovním prostoru OMS a ukládá je v úložišti OMS.</span><span class="sxs-lookup"><span data-stu-id="c34e2-105">Log Analytics collects data from the Connected Sources in your OMS workspace and stores it in OMS repository.</span></span>  <span data-ttu-id="c34e2-106">Data, která se shromažďují z každé je definována zdroje dat, který nakonfigurujete.</span><span class="sxs-lookup"><span data-stu-id="c34e2-106">The data that is collected from each is defined by the Data Sources that you configure.</span></span>  <span data-ttu-id="c34e2-107">Data v úložišti OMS se ukládají jako sada záznamů.</span><span class="sxs-lookup"><span data-stu-id="c34e2-107">Data in the OMS repository is stored as a set of records.</span></span>  <span data-ttu-id="c34e2-108">Všechny zdroje dat, vytvoří záznamy určitého typu pomocí jednotlivých typů má svou vlastní sadu vlastností.</span><span class="sxs-lookup"><span data-stu-id="c34e2-108">Each data source creates records of a particular type with each type having its own set of properties.</span></span>

![Přihlaste se shromažďování dat Analytics](./media/log-analytics-data-sources/overview.png)

<span data-ttu-id="c34e2-110">Zdroje dat se liší od OMS řešení, které také shromažďovat data z připojené zdroje a vytvářet záznamy v úložišti OMS.</span><span class="sxs-lookup"><span data-stu-id="c34e2-110">Data Sources are different than OMS Solutions which also collect data from Connected Sources and create records in the OMS repository.</span></span>  <span data-ttu-id="c34e2-111">Řešení můžete přidat do pracovního prostoru z Galerie řešení a obvykle poskytne nástrojů pro další analýzu na portálu OMS.</span><span class="sxs-lookup"><span data-stu-id="c34e2-111">Solutions can be added to your workspace from the Solutions Gallery and will typically provide additional analysis tools in the OMS portal.</span></span>  

## <a name="summary-of-data-sources"></a><span data-ttu-id="c34e2-112">Souhrn datových zdrojů</span><span class="sxs-lookup"><span data-stu-id="c34e2-112">Summary of data sources</span></span>
<span data-ttu-id="c34e2-113">Zdroje dat, které jsou aktuálně k dispozici v analýzy protokolů jsou uvedeny v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="c34e2-113">The data sources that are currently available in Log Analytics are listed in the following table.</span></span>  <span data-ttu-id="c34e2-114">Každý má odkaz na samostatný článek poskytuje podrobnosti pro tento zdroj dat..</span><span class="sxs-lookup"><span data-stu-id="c34e2-114">Each has a link to a separate article providing detail for that data source.</span></span>

| <span data-ttu-id="c34e2-115">Zdroj dat</span><span class="sxs-lookup"><span data-stu-id="c34e2-115">Data Source</span></span> | <span data-ttu-id="c34e2-116">Typ události</span><span class="sxs-lookup"><span data-stu-id="c34e2-116">Event Type</span></span> | <span data-ttu-id="c34e2-117">Popis</span><span class="sxs-lookup"><span data-stu-id="c34e2-117">Description</span></span> |
|:--- |:--- |:--- |
| [<span data-ttu-id="c34e2-118">Vlastní protokoly</span><span class="sxs-lookup"><span data-stu-id="c34e2-118">Custom logs</span></span>](log-analytics-data-sources-custom-logs.md) |<span data-ttu-id="c34e2-119">\<Název protokolu\>_CL</span><span class="sxs-lookup"><span data-stu-id="c34e2-119">\<LogName\>_CL</span></span> |<span data-ttu-id="c34e2-120">Textové soubory v systému Windows nebo Linux agenty obsahujícího informace o protokolu.</span><span class="sxs-lookup"><span data-stu-id="c34e2-120">Text files on Windows or Linux agents containing log information.</span></span> |
| [<span data-ttu-id="c34e2-121">Protokoly událostí systému Windows</span><span class="sxs-lookup"><span data-stu-id="c34e2-121">Windows Event logs</span></span>](log-analytics-data-sources-windows-events.md) |<span data-ttu-id="c34e2-122">Událost</span><span class="sxs-lookup"><span data-stu-id="c34e2-122">Event</span></span> |<span data-ttu-id="c34e2-123">Události shromážděné z protokolu událostí na počítačích s Windows.</span><span class="sxs-lookup"><span data-stu-id="c34e2-123">Events collected from the event log on Windows computers.</span></span> |
| [<span data-ttu-id="c34e2-124">Čítače výkonu systému Windows</span><span class="sxs-lookup"><span data-stu-id="c34e2-124">Windows Performance counters</span></span>](log-analytics-data-sources-performance-counters.md) |<span data-ttu-id="c34e2-125">Výkonu</span><span class="sxs-lookup"><span data-stu-id="c34e2-125">Perf</span></span> |<span data-ttu-id="c34e2-126">Čítače výkonu shromážděných z počítače se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="c34e2-126">Performance counters collected from Windows computers.</span></span> |
| [<span data-ttu-id="c34e2-127">Čítače výkonu systému Linux</span><span class="sxs-lookup"><span data-stu-id="c34e2-127">Linux Performance counters</span></span>](log-analytics-data-sources-performance-counters.md) |<span data-ttu-id="c34e2-128">Výkonu</span><span class="sxs-lookup"><span data-stu-id="c34e2-128">Perf</span></span> |<span data-ttu-id="c34e2-129">Čítače výkonu shromážděné z počítače se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="c34e2-129">Performance counters collected from Linux computers.</span></span> |
| [<span data-ttu-id="c34e2-130">Protokoly IIS</span><span class="sxs-lookup"><span data-stu-id="c34e2-130">IIS logs</span></span>](log-analytics-data-sources-iis-logs.md) |<span data-ttu-id="c34e2-131">W3CIISLog</span><span class="sxs-lookup"><span data-stu-id="c34e2-131">W3CIISLog</span></span> |<span data-ttu-id="c34e2-132">Internetová informační služba protokoly ve formátu W3C.</span><span class="sxs-lookup"><span data-stu-id="c34e2-132">Internet Information Services logs in W3C format.</span></span> |
| [<span data-ttu-id="c34e2-133">Syslog</span><span class="sxs-lookup"><span data-stu-id="c34e2-133">Syslog</span></span>](log-analytics-data-sources-syslog.md) |<span data-ttu-id="c34e2-134">Syslog</span><span class="sxs-lookup"><span data-stu-id="c34e2-134">Syslog</span></span> |<span data-ttu-id="c34e2-135">Události procesu Syslog na počítačích systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="c34e2-135">Syslog events on Windows or Linux computers.</span></span> |

## <a name="configuring-data-sources"></a><span data-ttu-id="c34e2-136">Konfigurace zdroje dat</span><span class="sxs-lookup"><span data-stu-id="c34e2-136">Configuring data sources</span></span>
<span data-ttu-id="c34e2-137">Konfigurace zdroje dat z **Data** nabídky v analýzy protokolů **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="c34e2-137">You configure data sources from the **Data** menu in Log Analytics **Settings**.</span></span>  <span data-ttu-id="c34e2-138">Jakákoli konfigurace se doručí na všech připojených zdrojů v pracovním prostoru OMS.</span><span class="sxs-lookup"><span data-stu-id="c34e2-138">Any configuration is delivered to all connected sources in your OMS workspace.</span></span>  <span data-ttu-id="c34e2-139">Veškeré agenty nelze aktuálně vyloučit z této konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c34e2-139">You cannot currently exclude any agents from this configuration.</span></span>

![Konfigurace události systému Windows](./media/log-analytics-data-sources/configure-events.png)

1. <span data-ttu-id="c34e2-141">V konzole OMS klikněte na **nastavení** dlaždici nebo **nastavení** tlačítka v horní části obrazovky.</span><span class="sxs-lookup"><span data-stu-id="c34e2-141">In the OMS console click the **Settings** tile or the **Settings** button at the top of the screen.</span></span>
2. <span data-ttu-id="c34e2-142">Vyberte **Data**.</span><span class="sxs-lookup"><span data-stu-id="c34e2-142">Select **Data**.</span></span>
3. <span data-ttu-id="c34e2-143">Klikněte na zdroj dat konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c34e2-143">Click on the data source to configure.</span></span>
4. <span data-ttu-id="c34e2-144">Použijte odkaz na dokumentaci pro každý zdroj dat v tabulce Podrobnosti na jejich konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="c34e2-144">Follow the link to the documentation for each data source in the above table for details on their configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="c34e2-145">Momentálně nelze nakonfigurovat analýzy protokolů zdroje dat na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c34e2-145">You cannot currently configure Log Analytics data sources in the Azure portal.</span></span>

## <a name="data-collection"></a><span data-ttu-id="c34e2-146">Shromažďování dat</span><span class="sxs-lookup"><span data-stu-id="c34e2-146">Data collection</span></span>
<span data-ttu-id="c34e2-147">Konfigurace zdroje dat se dodávají s agenty, které jsou připojeny přímo k Log Analytics během několika minut.</span><span class="sxs-lookup"><span data-stu-id="c34e2-147">Data source configurations are delivered to agents that are directly connected to Log Analytics within a few minutes.</span></span>  <span data-ttu-id="c34e2-148">Zadaná data se shromažďují z agenta a doručeny přímo k Log Analytics v intervalech, které jsou specifické pro každý zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="c34e2-148">The specified data is collected from the agent and delivered directly to Log Analytics at intervals specific to each data source.</span></span>  <span data-ttu-id="c34e2-149">Najdete v dokumentaci pro každý zdroj dat pro tyto konkrétní.</span><span class="sxs-lookup"><span data-stu-id="c34e2-149">See the documentation for each data source for these specifics.</span></span>

<span data-ttu-id="c34e2-150">Konfigurace zdroje dat pro System Center Operations Manager (SCOM) agenty v připojené skupiny pro správu, jsou přeložit na sady management Pack a doručit do skupiny pro správu každých 5 minut, ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="c34e2-150">For System Center Operations Manager (SCOM) agents in a connected management group, data source configurations are translated into management packs and delivered to the management group every 5 minutes by default.</span></span>  <span data-ttu-id="c34e2-151">Agent soubory ke stažení sady management pack jako libovolný jiný a shromažďuje zadaná data.</span><span class="sxs-lookup"><span data-stu-id="c34e2-151">The agent downloads the management pack like any other and collects the specified data.</span></span> <span data-ttu-id="c34e2-152">V závislosti na zdroji dat, že buď odeslána na server pro správu, který předává data k analýze protokolů budou data nebo bude agent posílat data k analýze protokolů bez průchodu přes server pro správu.</span><span class="sxs-lookup"><span data-stu-id="c34e2-152">Depending on the data source the data will be either sent to a management server which forwards the data to the Log Analytics, or the agent will send the data to Log Analytics without going through the management server.</span></span> <span data-ttu-id="c34e2-153">Odkazovat na [podrobnosti kolekce dat pro OMS funkcí a řešení](log-analytics-add-solutions.md#data-collection-details) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="c34e2-153">Refer to [data collection details for OMS features and solutions](log-analytics-add-solutions.md#data-collection-details) for details.</span></span>  <span data-ttu-id="c34e2-154">Další informace o podrobnosti o připojení SCOM a OMS a úprava frekvence tato konfigurace je dodána na [konfigurace integrace s nástrojem System Center Operations Manager](log-analytics-om-agents.md).</span><span class="sxs-lookup"><span data-stu-id="c34e2-154">You can read about details of connecting SCOM and OMS and modifying the frequency that configuration is delivered at [Configure Integration with System Center Operations Manager](log-analytics-om-agents.md).</span></span>

<span data-ttu-id="c34e2-155">Pokud agenta nemůže připojit k analýze protokolů nebo Operations Manager, bude pokračovat ke shromažďování dat, která bude poskytovat, když se naváže připojení.</span><span class="sxs-lookup"><span data-stu-id="c34e2-155">If the agent is unable to connect to Log Analytics or Operations Manager, it will continue to collect data that it will deliver when it establishes a connection.</span></span>  <span data-ttu-id="c34e2-156">Data mohou být ztracena, pokud objem dat dosáhne maximální velikost mezipaměti klienta, nebo pokud agent není schopen navázat připojení do 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="c34e2-156">Data can be lost if the amount of data reaches the maximum cache size for the client, or if the agent is not able to establish a connection within 24 hours.</span></span>

## <a name="log-analytics-records"></a><span data-ttu-id="c34e2-157">Záznamy služby Log Analytics</span><span class="sxs-lookup"><span data-stu-id="c34e2-157">Log Analytics records</span></span>
<span data-ttu-id="c34e2-158">Všechna data shromažďovaná společností analýzy protokolů je uložen v úložišti OMS jako záznamy.</span><span class="sxs-lookup"><span data-stu-id="c34e2-158">All data collected by Log Analytics is stored in the OMS repository as records.</span></span>  <span data-ttu-id="c34e2-159">Zaznamenává shromážděné z různých zdrojů dat. bude mít vlastní sadu vlastností a identifikovat podle jejich **typ** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c34e2-159">Records collected by different data sources will have their own set of properties and be identified by their **Type** property.</span></span>  <span data-ttu-id="c34e2-160">Najdete v dokumentaci pro každý zdroj dat a řešení podrobnosti pro každý typ záznamu.</span><span class="sxs-lookup"><span data-stu-id="c34e2-160">See the documentation for each data source and solution for details on each record type.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c34e2-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c34e2-161">Next Steps</span></span>
* <span data-ttu-id="c34e2-162">Další informace o [řešení](log-analytics-add-solutions.md) , přidání funkce do analýzy protokolů a také shromažďovat data do úložiště OMS.</span><span class="sxs-lookup"><span data-stu-id="c34e2-162">Learn about [solutions](log-analytics-add-solutions.md) that add functionality to Log Analytics and also collect data into the OMS repository.</span></span>
* <span data-ttu-id="c34e2-163">Další informace o [protokolu hledání](log-analytics-log-searches.md) analyzovat data shromážděná ze zdrojů dat a řešení.</span><span class="sxs-lookup"><span data-stu-id="c34e2-163">Learn about [log searches](log-analytics-log-searches.md) to analyze the data collected from data sources and solutions.</span></span>  
* <span data-ttu-id="c34e2-164">Konfigurace [výstrahy](log-analytics-alerts.md) jako proaktivně upozornění na kritický data shromážděná ze zdrojů dat a řešení.</span><span class="sxs-lookup"><span data-stu-id="c34e2-164">Configure [alerts](log-analytics-alerts.md) to proactively notify you of critical data collected from data sources and solutions.</span></span>