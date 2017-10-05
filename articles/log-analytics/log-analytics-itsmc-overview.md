---
title: "IT služby konektoru Management v OMS | Microsoft Docs"
description: "Pomocí konektoru služby správy IT centrálně monitorovat a spravovat ITSM pracovních položek v OMS a rychle vyřešit všechny problémy."
services: log-analytics
documentationcenter: 
author: JYOTHIRMAISURI
manager: riyazp
editor: 
ms.assetid: 0b1414d9-b0a7-4e4e-a652-d3a6ff1118c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: v-jysur
ms.openlocfilehash: 54974ef06efdae69ddbfa12b1ba9278b48b113d3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="centrally-manage-itsm-work-items-using-it-service-management-connector-preview"></a><span data-ttu-id="f54d5-103">Centrálně spravovat ITSM pracovních položek pomocí IT Service Management Connector (Preview)</span><span class="sxs-lookup"><span data-stu-id="f54d5-103">Centrally manage ITSM work items using IT Service Management Connector (Preview)</span></span>

![Symbol konektoru služby správy IT](./media/log-analytics-itsmc/itsmc-symbol.png)

<span data-ttu-id="f54d5-105">Konektor pro správu služby IT (ITSMC) v OMS analýzy protokolů můžete použít centrálně monitorovat a spravovat pracovní položky v rámci ITSM produktů nebo služeb.</span><span class="sxs-lookup"><span data-stu-id="f54d5-105">You can use the IT Service Management Connector (ITSMC) in OMS Log Analytics to centrally monitor and manage work items across your ITSM products/services.</span></span>

<span data-ttu-id="f54d5-106">Konektor služby správy IT se integruje s analýzy protokolů OMS existující IT služby správy (ITSM) produkty a služby.</span><span class="sxs-lookup"><span data-stu-id="f54d5-106">The IT Service Management Connector integrates your existing IT Service Management (ITSM) products and services with OMS Log Analytics.</span></span>  <span data-ttu-id="f54d5-107">Řešení obsahuje obousměrnou integraci s ITSM produktů nebo služeb, přičemž poskytuje uživatelům OMS možnost pro vytvoření incidentů, výstrahy nebo události v ITSM řešení.</span><span class="sxs-lookup"><span data-stu-id="f54d5-107">The solution has bidirectional integration with ITSM products/services, where it provides the OMS users an option to create incidents, alerts, or events in ITSM solution.</span></span> <span data-ttu-id="f54d5-108">Konektor importuje také data, jako jsou incidenty a žádostí o změny z ITSM řešení do analýzy protokolů OMS.</span><span class="sxs-lookup"><span data-stu-id="f54d5-108">The connector also  imports data such as incidents, and change requests from ITSM solution into OMS Log Analytics.</span></span>

<span data-ttu-id="f54d5-109">Pomocí konektoru služby správy IT můžete:</span><span class="sxs-lookup"><span data-stu-id="f54d5-109">With IT Service Management Connector, you can:</span></span>

  - <span data-ttu-id="f54d5-110">Centrálně monitorovat a spravovat pracovní položky pro ITSM produkty nebo služby používané ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="f54d5-110">Centrally monitor and manage work items for ITSM products/services used across your organization.</span></span>
  - <span data-ttu-id="f54d5-111">Vytvoření ITSM pracovních položek (například výstrahy, události, incident) v ITSM z OMS výstrah a prostřednictvím protokolu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="f54d5-111">Create ITSM work items (like alert, event, incident) in ITSM from OMS alerts and through log search.</span></span>
  - <span data-ttu-id="f54d5-112">Čtení incidenty a žádosti o změnu z řešení ITSM a korelovat s daty relevantní protokolu v pracovní prostor analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="f54d5-112">Read incidents and change requests from your ITSM solution and correlate with relevant log data in Log Analytics workspace.</span></span>
  - <span data-ttu-id="f54d5-113">Najde všechny neočekávanou a neobvyklé události a vyřešit, ještě před koncovým uživatelům volání a nahlásit oddělení technické podpory.</span><span class="sxs-lookup"><span data-stu-id="f54d5-113">Find any unexpected and unusual events and resolve them, even before the end users call and report them to the helpdesk.</span></span>
  - <span data-ttu-id="f54d5-114">Importovat data pracovních položek do analýzy protokolů a vytváření sestav výkonnosti klíče pro Klíčový ukazatel výkonu.</span><span class="sxs-lookup"><span data-stu-id="f54d5-114">Import work items data into Log Analytics and create key performance indicator (KPI) reports.</span></span>  <span data-ttu-id="f54d5-115">Pomocí těchto sestav, můžete identifikovat, hodnocení a fungovat na některé důležité skutečnosti, jako je například posouzením malwaru.</span><span class="sxs-lookup"><span data-stu-id="f54d5-115">Using these reports, you can identify, assess and act on several important items such as malware assessment.</span></span>
  - <span data-ttu-id="f54d5-116">Zobrazit kurátorované řídicí panely pro podrobnější přehled na incidenty, žádostí o změnu a ovlivněné systémy.</span><span class="sxs-lookup"><span data-stu-id="f54d5-116">View curated dashboards for deeper insights on incidents, change requests and impacted systems.</span></span>
  - <span data-ttu-id="f54d5-117">Řešení potíží s rychlejší pomocí souvztažnosti pomocí jiných řešení pro správu v pracovním prostoru analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="f54d5-117">Troubleshoot faster by correlating with other management solutions in the Log Analytics workspace.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="f54d5-118">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f54d5-118">Prerequisites</span></span>

<span data-ttu-id="f54d5-119">Pokud chcete importovat ITSM pracovních položek do OMS analýzy protokolů, řešení vyžaduje připojení mezi konektor správy služeb IT v OMS a na IT SM produktům a službám, ze kterého můžete importovat pracovní položky.</span><span class="sxs-lookup"><span data-stu-id="f54d5-119">To import the ITSM work items into OMS Log Analytics, the solution requires a connection between the IT Service Management Connector in the OMS and the IT SM product/service from which you import the work items.</span></span>


## <a name="configuration"></a><span data-ttu-id="f54d5-120">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="f54d5-120">Configuration</span></span>

<span data-ttu-id="f54d5-121">Přidání konektoru služby správy IT řešení do prostoru pracovní OMS, pomocí procesu popsaného v tématu [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="f54d5-121">Add the IT Service Management Connector solution to your OMS work space, using the process described in [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span>

<span data-ttu-id="f54d5-122">Konektoru Service Management IT dlaždici, jak vidíte v Galerii řešení:</span><span class="sxs-lookup"><span data-stu-id="f54d5-122">IT Service Management Connector tile as you see in the Solutions gallery:</span></span>

![dlaždice konektoru](./media/log-analytics-itsmc/itsmc-solutions-tile.png)

<span data-ttu-id="f54d5-124">Po úspěšném přidání, zobrazí se konektoru Management Service IT v rámci **OMS** > **nastavení** > **připojené zdroje.**</span><span class="sxs-lookup"><span data-stu-id="f54d5-124">After successful addition, you will see the IT Service Management Connector under **OMS** > **Settings** > **Connected Sources.**</span></span>

![ITSMC připojení](./media/log-analytics-itsmc/itsmc-overview-solution-in-connected-sources.png)

> [!NOTE]

> <span data-ttu-id="f54d5-126">Ve výchozím nastavení konektor služby správy IT aktualizuje data připojení jednou za každých 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="f54d5-126">By default, the IT Service Management Connector refreshes the connection's data once in every 24 hours.</span></span> <span data-ttu-id="f54d5-127">Aktualizovat připojení k data okamžitě pro úpravy nebo aktualizace šablony, které provedete, klepněte na tlačítko Aktualizovat se zobrazí vedle připojení.</span><span class="sxs-lookup"><span data-stu-id="f54d5-127">To refresh your connection's data instantly for any edits or template updates that you make, click the refresh button displayed next to your connection.</span></span>

 ![Aktualizace ITSMC](./media/log-analytics-itsmc/itsmc-connection-refresh.png)

## <a name="management-packs"></a><span data-ttu-id="f54d5-129">Sady Management Pack</span><span class="sxs-lookup"><span data-stu-id="f54d5-129">Management packs</span></span>
<span data-ttu-id="f54d5-130">Toto řešení nevyžaduje žádné sady management Pack.</span><span class="sxs-lookup"><span data-stu-id="f54d5-130">This solution does not require any management packs.</span></span>

## <a name="connected-sources"></a><span data-ttu-id="f54d5-131">Připojené zdroje</span><span class="sxs-lookup"><span data-stu-id="f54d5-131">Connected sources</span></span>

<span data-ttu-id="f54d5-132">Podporuje následující ITSM produkty nebo služby konektoru IT Service Management:</span><span class="sxs-lookup"><span data-stu-id="f54d5-132">The following ITSM products/services are supported by the IT Service Management Connector:</span></span>

- [<span data-ttu-id="f54d5-133">System Center Service Manager (SCSM)</span><span class="sxs-lookup"><span data-stu-id="f54d5-133">System Center Service Manager (SCSM)</span></span>](log-analytics-itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-oms)

- [<span data-ttu-id="f54d5-134">ServiceNow</span><span class="sxs-lookup"><span data-stu-id="f54d5-134">ServiceNow</span></span>](log-analytics-itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-oms)

- [<span data-ttu-id="f54d5-135">Provance</span><span class="sxs-lookup"><span data-stu-id="f54d5-135">Provance</span></span>](log-analytics-itsmc-connections.md#connect-provance-to-it-service-management-connector-in-oms)  

- [<span data-ttu-id="f54d5-136">Cherwell</span><span class="sxs-lookup"><span data-stu-id="f54d5-136">Cherwell</span></span>](log-analytics-itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-oms)

## <a name="using-the-solution"></a><span data-ttu-id="f54d5-137">Použití řešení</span><span class="sxs-lookup"><span data-stu-id="f54d5-137">Using the solution</span></span>

<span data-ttu-id="f54d5-138">Jakmile se připojíte konektor OMS IT službu správy s ITSM službou, analýzy protokolů služby spustí, shromažďování dat z připojených ITSM produkty nebo služby.</span><span class="sxs-lookup"><span data-stu-id="f54d5-138">Once you connect the OMS IT Service Management Connector with your ITSM service, the Log Analytics services starts gathering the data from the connected ITSM products/service.</span></span>

> [!NOTE]
> - <span data-ttu-id="f54d5-139">Dat importovaných pomocí konektoru služby správy IT řešení se zobrazí v analýzy protokolů jako události s názvem **ServiceDesk_CL**.</span><span class="sxs-lookup"><span data-stu-id="f54d5-139">Data imported by IT Service Management Connector solution appears in Log Analytics as events named **ServiceDesk_CL**.</span></span>
- <span data-ttu-id="f54d5-140">Událost obsahuje pole s názvem **ServiceDeskWorkItemType_s**.</span><span class="sxs-lookup"><span data-stu-id="f54d5-140">Event contains a field named **ServiceDeskWorkItemType_s**.</span></span> <span data-ttu-id="f54d5-141">která může převzít jeho hodnotu jako incident, nebo v závislosti na data pracovních položek, které jsou obsažené v žádosti o změnu **ServiceDesk_CL** událostí.</span><span class="sxs-lookup"><span data-stu-id="f54d5-141">which can take its value as incident, or change request, depending on the work item data contained in the **ServiceDesk_CL** event.</span></span>

## <a name="input-data"></a><span data-ttu-id="f54d5-142">Vstupní data</span><span class="sxs-lookup"><span data-stu-id="f54d5-142">Input data</span></span>
<span data-ttu-id="f54d5-143">Pracovní položky, které jsou importovány z ITSM produkty nebo služby.</span><span class="sxs-lookup"><span data-stu-id="f54d5-143">Work items imported from the ITSM products/services.</span></span>

<span data-ttu-id="f54d5-144">Tyto informace jsou uvedeny příklady data shromážděná konektorem správy služeb IT:</span><span class="sxs-lookup"><span data-stu-id="f54d5-144">The following information shows examples of data gathered by the IT Service Management connector:</span></span>

> [!NOTE]
> <span data-ttu-id="f54d5-145">V závislosti na typu pracovní položky naimportovat do analýzy protokolů **ServiceDesk_CL** obsahuje následující pole:</span><span class="sxs-lookup"><span data-stu-id="f54d5-145">Depending on the work item type imported into Log Analytics, **ServiceDesk_CL** contains the following fields:</span></span>

<span data-ttu-id="f54d5-146">**Pracovní položka:** **incidenty**</span><span class="sxs-lookup"><span data-stu-id="f54d5-146">**Work item:** **Incidents**</span></span>  
<span data-ttu-id="f54d5-147">ServiceDeskWorkItemType_s = "Incidentem"</span><span class="sxs-lookup"><span data-stu-id="f54d5-147">ServiceDeskWorkItemType_s="Incident"</span></span>

<span data-ttu-id="f54d5-148">**Pole**</span><span class="sxs-lookup"><span data-stu-id="f54d5-148">**Fields**</span></span>

- <span data-ttu-id="f54d5-149">ServiceDeskConnectionName</span><span class="sxs-lookup"><span data-stu-id="f54d5-149">ServiceDeskConnectionName</span></span>
- <span data-ttu-id="f54d5-150">ID služby podpory</span><span class="sxs-lookup"><span data-stu-id="f54d5-150">Service Desk ID</span></span>
- <span data-ttu-id="f54d5-151">Stav</span><span class="sxs-lookup"><span data-stu-id="f54d5-151">State</span></span>
- <span data-ttu-id="f54d5-152">Naléhavosti</span><span class="sxs-lookup"><span data-stu-id="f54d5-152">Urgency</span></span>
- <span data-ttu-id="f54d5-153">Dopad</span><span class="sxs-lookup"><span data-stu-id="f54d5-153">Impact</span></span>
- <span data-ttu-id="f54d5-154">Priorita</span><span class="sxs-lookup"><span data-stu-id="f54d5-154">Priority</span></span>
- <span data-ttu-id="f54d5-155">Eskalaci</span><span class="sxs-lookup"><span data-stu-id="f54d5-155">Escalation</span></span>
- <span data-ttu-id="f54d5-156">Autor</span><span class="sxs-lookup"><span data-stu-id="f54d5-156">Created By</span></span>
- <span data-ttu-id="f54d5-157">Vyřešil</span><span class="sxs-lookup"><span data-stu-id="f54d5-157">Resolved By</span></span>
- <span data-ttu-id="f54d5-158">Uzavřené</span><span class="sxs-lookup"><span data-stu-id="f54d5-158">Closed By</span></span>
- <span data-ttu-id="f54d5-159">Zdroj</span><span class="sxs-lookup"><span data-stu-id="f54d5-159">Source</span></span>
- <span data-ttu-id="f54d5-160">Přiřazené</span><span class="sxs-lookup"><span data-stu-id="f54d5-160">Assigned To</span></span>
- <span data-ttu-id="f54d5-161">Kategorie</span><span class="sxs-lookup"><span data-stu-id="f54d5-161">Category</span></span>
- <span data-ttu-id="f54d5-162">Název</span><span class="sxs-lookup"><span data-stu-id="f54d5-162">Title</span></span>
- <span data-ttu-id="f54d5-163">Popis</span><span class="sxs-lookup"><span data-stu-id="f54d5-163">Description</span></span>
- <span data-ttu-id="f54d5-164">Datum vytvoření</span><span class="sxs-lookup"><span data-stu-id="f54d5-164">Created Date</span></span>
- <span data-ttu-id="f54d5-165">Datum uzavření</span><span class="sxs-lookup"><span data-stu-id="f54d5-165">Closed Date</span></span>
- <span data-ttu-id="f54d5-166">Datum vyřešení</span><span class="sxs-lookup"><span data-stu-id="f54d5-166">Resolved Date</span></span>
- <span data-ttu-id="f54d5-167">Datum poslední změny</span><span class="sxs-lookup"><span data-stu-id="f54d5-167">Last Modified Date</span></span>
- <span data-ttu-id="f54d5-168">Počítač</span><span class="sxs-lookup"><span data-stu-id="f54d5-168">Computer</span></span>


<span data-ttu-id="f54d5-169">**Pracovní položka:** **žádosti o změnu**</span><span class="sxs-lookup"><span data-stu-id="f54d5-169">**Work item:** **Change Requests**</span></span>

<span data-ttu-id="f54d5-170">ServiceDeskWorkItemType_s = "ChangeRequest"</span><span class="sxs-lookup"><span data-stu-id="f54d5-170">ServiceDeskWorkItemType_s="ChangeRequest"</span></span>

<span data-ttu-id="f54d5-171">**Pole**</span><span class="sxs-lookup"><span data-stu-id="f54d5-171">**Fields**</span></span>
- <span data-ttu-id="f54d5-172">ServiceDeskConnectionName</span><span class="sxs-lookup"><span data-stu-id="f54d5-172">ServiceDeskConnectionName</span></span>
- <span data-ttu-id="f54d5-173">ID služby podpory</span><span class="sxs-lookup"><span data-stu-id="f54d5-173">Service Desk ID</span></span>
- <span data-ttu-id="f54d5-174">Autor</span><span class="sxs-lookup"><span data-stu-id="f54d5-174">Created By</span></span>
- <span data-ttu-id="f54d5-175">Uzavřené</span><span class="sxs-lookup"><span data-stu-id="f54d5-175">Closed By</span></span>
- <span data-ttu-id="f54d5-176">Zdroj</span><span class="sxs-lookup"><span data-stu-id="f54d5-176">Source</span></span>
- <span data-ttu-id="f54d5-177">Přiřazené</span><span class="sxs-lookup"><span data-stu-id="f54d5-177">Assigned To</span></span>
- <span data-ttu-id="f54d5-178">Název</span><span class="sxs-lookup"><span data-stu-id="f54d5-178">Title</span></span>
- <span data-ttu-id="f54d5-179">Typ</span><span class="sxs-lookup"><span data-stu-id="f54d5-179">Type</span></span>
- <span data-ttu-id="f54d5-180">Kategorie</span><span class="sxs-lookup"><span data-stu-id="f54d5-180">Category</span></span>
- <span data-ttu-id="f54d5-181">Stav</span><span class="sxs-lookup"><span data-stu-id="f54d5-181">State</span></span>
- <span data-ttu-id="f54d5-182">Eskalaci</span><span class="sxs-lookup"><span data-stu-id="f54d5-182">Escalation</span></span>
- <span data-ttu-id="f54d5-183">Konflikt stavu</span><span class="sxs-lookup"><span data-stu-id="f54d5-183">Conflict Status</span></span>
- <span data-ttu-id="f54d5-184">Naléhavosti</span><span class="sxs-lookup"><span data-stu-id="f54d5-184">Urgency</span></span>
- <span data-ttu-id="f54d5-185">Priorita</span><span class="sxs-lookup"><span data-stu-id="f54d5-185">Priority</span></span>
- <span data-ttu-id="f54d5-186">Riziko</span><span class="sxs-lookup"><span data-stu-id="f54d5-186">Risk</span></span>
- <span data-ttu-id="f54d5-187">Dopad</span><span class="sxs-lookup"><span data-stu-id="f54d5-187">Impact</span></span>
- <span data-ttu-id="f54d5-188">Přiřazené</span><span class="sxs-lookup"><span data-stu-id="f54d5-188">Assigned To</span></span>
- <span data-ttu-id="f54d5-189">Datum vytvoření</span><span class="sxs-lookup"><span data-stu-id="f54d5-189">Created Date</span></span>
- <span data-ttu-id="f54d5-190">Datum uzavření</span><span class="sxs-lookup"><span data-stu-id="f54d5-190">Closed Date</span></span>
- <span data-ttu-id="f54d5-191">Datum poslední změny</span><span class="sxs-lookup"><span data-stu-id="f54d5-191">Last Modified Date</span></span>
- <span data-ttu-id="f54d5-192">Požadovaná data</span><span class="sxs-lookup"><span data-stu-id="f54d5-192">Requested Date</span></span>
- <span data-ttu-id="f54d5-193">Plánované počáteční datum</span><span class="sxs-lookup"><span data-stu-id="f54d5-193">Planned Start Date</span></span>
- <span data-ttu-id="f54d5-194">Naplánované koncové datum</span><span class="sxs-lookup"><span data-stu-id="f54d5-194">Planned End Date</span></span>
- <span data-ttu-id="f54d5-195">Pracovní počáteční datum</span><span class="sxs-lookup"><span data-stu-id="f54d5-195">Work Start Date</span></span>
- <span data-ttu-id="f54d5-196">Pracovní koncové datum</span><span class="sxs-lookup"><span data-stu-id="f54d5-196">Work End Date</span></span>
- <span data-ttu-id="f54d5-197">Popis</span><span class="sxs-lookup"><span data-stu-id="f54d5-197">Description</span></span>
- <span data-ttu-id="f54d5-198">Počítač</span><span class="sxs-lookup"><span data-stu-id="f54d5-198">Computer</span></span>

## <a name="output-data-for-a-servicenow-incident"></a><span data-ttu-id="f54d5-199">Výstupní data pro ServiceNow incident</span><span class="sxs-lookup"><span data-stu-id="f54d5-199">Output data for a ServiceNow incident</span></span>

| <span data-ttu-id="f54d5-200">Pole OMS</span><span class="sxs-lookup"><span data-stu-id="f54d5-200">OMS field</span></span> | <span data-ttu-id="f54d5-201">ITSM pole</span><span class="sxs-lookup"><span data-stu-id="f54d5-201">ITSM field</span></span> |
|:--- |:--- |
| <span data-ttu-id="f54d5-202">ServiceDeskId_s</span><span class="sxs-lookup"><span data-stu-id="f54d5-202">ServiceDeskId_s</span></span>| <span data-ttu-id="f54d5-203">Číslo</span><span class="sxs-lookup"><span data-stu-id="f54d5-203">Number</span></span> |
| <span data-ttu-id="f54d5-204">IncidentState_s</span><span class="sxs-lookup"><span data-stu-id="f54d5-204">IncidentState_s</span></span> | <span data-ttu-id="f54d5-205">Stav</span><span class="sxs-lookup"><span data-stu-id="f54d5-205">State</span></span> |
| <span data-ttu-id="f54d5-206">Urgency_s</span><span class="sxs-lookup"><span data-stu-id="f54d5-206">Urgency_s</span></span> |<span data-ttu-id="f54d5-207">Naléhavosti</span><span class="sxs-lookup"><span data-stu-id="f54d5-207">Urgency</span></span> |
| <span data-ttu-id="f54d5-208">Impact_s</span><span class="sxs-lookup"><span data-stu-id="f54d5-208">Impact_s</span></span> |<span data-ttu-id="f54d5-209">Dopad</span><span class="sxs-lookup"><span data-stu-id="f54d5-209">Impact</span></span>|
| <span data-ttu-id="f54d5-210">Priority_s</span><span class="sxs-lookup"><span data-stu-id="f54d5-210">Priority_s</span></span> | <span data-ttu-id="f54d5-211">Priorita</span><span class="sxs-lookup"><span data-stu-id="f54d5-211">Priority</span></span> |
| <span data-ttu-id="f54d5-212">CreatedBy_s</span><span class="sxs-lookup"><span data-stu-id="f54d5-212">CreatedBy_s</span></span> | <span data-ttu-id="f54d5-213">Otevřít</span><span class="sxs-lookup"><span data-stu-id="f54d5-213">Opened by</span></span> |
| <span data-ttu-id="f54d5-214">ResolvedBy_s</span><span class="sxs-lookup"><span data-stu-id="f54d5-214">ResolvedBy_s</span></span> | <span data-ttu-id="f54d5-215">Vyřešil</span><span class="sxs-lookup"><span data-stu-id="f54d5-215">Resolved by</span></span>|
| <span data-ttu-id="f54d5-216">ClosedBy_s</span><span class="sxs-lookup"><span data-stu-id="f54d5-216">ClosedBy_s</span></span>  | <span data-ttu-id="f54d5-217">Uzavřené</span><span class="sxs-lookup"><span data-stu-id="f54d5-217">Closed by</span></span> |
| <span data-ttu-id="f54d5-218">Source_s</span><span class="sxs-lookup"><span data-stu-id="f54d5-218">Source_s</span></span>| <span data-ttu-id="f54d5-219">Obraťte se na typ</span><span class="sxs-lookup"><span data-stu-id="f54d5-219">Contact type</span></span> |
| <span data-ttu-id="f54d5-220">AssignedTo_s</span><span class="sxs-lookup"><span data-stu-id="f54d5-220">AssignedTo_s</span></span> | <span data-ttu-id="f54d5-221">Přiřazené</span><span class="sxs-lookup"><span data-stu-id="f54d5-221">Assigned to</span></span>  |
| <span data-ttu-id="f54d5-222">Category_s</span><span class="sxs-lookup"><span data-stu-id="f54d5-222">Category_s</span></span> | <span data-ttu-id="f54d5-223">Kategorie</span><span class="sxs-lookup"><span data-stu-id="f54d5-223">Category</span></span> |
| <span data-ttu-id="f54d5-224">Title_s</span><span class="sxs-lookup"><span data-stu-id="f54d5-224">Title_s</span></span>|  <span data-ttu-id="f54d5-225">Krátký popis</span><span class="sxs-lookup"><span data-stu-id="f54d5-225">Short description</span></span> |
| <span data-ttu-id="f54d5-226">Description_s</span><span class="sxs-lookup"><span data-stu-id="f54d5-226">Description_s</span></span>|  <span data-ttu-id="f54d5-227">Poznámky</span><span class="sxs-lookup"><span data-stu-id="f54d5-227">Notes</span></span> |
| <span data-ttu-id="f54d5-228">CreatedDate_t</span><span class="sxs-lookup"><span data-stu-id="f54d5-228">CreatedDate_t</span></span>|  <span data-ttu-id="f54d5-229">Otevřít</span><span class="sxs-lookup"><span data-stu-id="f54d5-229">Opened</span></span> |
| <span data-ttu-id="f54d5-230">ClosedDate_t</span><span class="sxs-lookup"><span data-stu-id="f54d5-230">ClosedDate_t</span></span>| <span data-ttu-id="f54d5-231">uzavřený</span><span class="sxs-lookup"><span data-stu-id="f54d5-231">closed</span></span>|
| <span data-ttu-id="f54d5-232">ResolvedDate_t</span><span class="sxs-lookup"><span data-stu-id="f54d5-232">ResolvedDate_t</span></span>|<span data-ttu-id="f54d5-233">Vyřešil</span><span class="sxs-lookup"><span data-stu-id="f54d5-233">Resolved</span></span>|
| <span data-ttu-id="f54d5-234">Počítač</span><span class="sxs-lookup"><span data-stu-id="f54d5-234">Computer</span></span>  | <span data-ttu-id="f54d5-235">položky konfigurace</span><span class="sxs-lookup"><span data-stu-id="f54d5-235">Configuration item</span></span> |

## <a name="output-data-for-a-servicenow-change-request"></a><span data-ttu-id="f54d5-236">Výstupní data pro ServiceNow žádost o změnu</span><span class="sxs-lookup"><span data-stu-id="f54d5-236">Output data for a ServiceNow change request</span></span>

| <span data-ttu-id="f54d5-237">Pole OMS</span><span class="sxs-lookup"><span data-stu-id="f54d5-237">OMS field</span></span> | <span data-ttu-id="f54d5-238">ITSM pole</span><span class="sxs-lookup"><span data-stu-id="f54d5-238">ITSM field</span></span> |
|:--- |:--- |
| <span data-ttu-id="f54d5-239">ServiceDeskId_s</span><span class="sxs-lookup"><span data-stu-id="f54d5-239">ServiceDeskId_s</span></span>| <span data-ttu-id="f54d5-240">Číslo</span><span class="sxs-lookup"><span data-stu-id="f54d5-240">Number</span></span> |
| <span data-ttu-id="f54d5-241">CreatedBy_s</span><span class="sxs-lookup"><span data-stu-id="f54d5-241">CreatedBy_s</span></span> | <span data-ttu-id="f54d5-242">Požadoval</span><span class="sxs-lookup"><span data-stu-id="f54d5-242">Requested by</span></span> |
| <span data-ttu-id="f54d5-243">ClosedBy_s</span><span class="sxs-lookup"><span data-stu-id="f54d5-243">ClosedBy_s</span></span> | <span data-ttu-id="f54d5-244">Uzavřené</span><span class="sxs-lookup"><span data-stu-id="f54d5-244">Closed by</span></span> |
| <span data-ttu-id="f54d5-245">AssignedTo_s</span><span class="sxs-lookup"><span data-stu-id="f54d5-245">AssignedTo_s</span></span> | <span data-ttu-id="f54d5-246">Přiřazené</span><span class="sxs-lookup"><span data-stu-id="f54d5-246">Assigned to</span></span>  |
| <span data-ttu-id="f54d5-247">Title_s</span><span class="sxs-lookup"><span data-stu-id="f54d5-247">Title_s</span></span>|  <span data-ttu-id="f54d5-248">Krátký popis</span><span class="sxs-lookup"><span data-stu-id="f54d5-248">Short description</span></span> |
| <span data-ttu-id="f54d5-249">Type_s</span><span class="sxs-lookup"><span data-stu-id="f54d5-249">Type_s</span></span>|  <span data-ttu-id="f54d5-250">Typ</span><span class="sxs-lookup"><span data-stu-id="f54d5-250">Type</span></span> |
| <span data-ttu-id="f54d5-251">Category_s</span><span class="sxs-lookup"><span data-stu-id="f54d5-251">Category_s</span></span>|  <span data-ttu-id="f54d5-252">Catgory</span><span class="sxs-lookup"><span data-stu-id="f54d5-252">Catgory</span></span> |
| <span data-ttu-id="f54d5-253">CRState_s</span><span class="sxs-lookup"><span data-stu-id="f54d5-253">CRState_s</span></span>|  <span data-ttu-id="f54d5-254">Stav</span><span class="sxs-lookup"><span data-stu-id="f54d5-254">State</span></span>|
| <span data-ttu-id="f54d5-255">Urgency_s</span><span class="sxs-lookup"><span data-stu-id="f54d5-255">Urgency_s</span></span>|  <span data-ttu-id="f54d5-256">Naléhavosti</span><span class="sxs-lookup"><span data-stu-id="f54d5-256">Urgency</span></span> |
| <span data-ttu-id="f54d5-257">Priority_s</span><span class="sxs-lookup"><span data-stu-id="f54d5-257">Priority_s</span></span>| <span data-ttu-id="f54d5-258">Priorita</span><span class="sxs-lookup"><span data-stu-id="f54d5-258">Priority</span></span>|
| <span data-ttu-id="f54d5-259">Risk_s</span><span class="sxs-lookup"><span data-stu-id="f54d5-259">Risk_s</span></span>| <span data-ttu-id="f54d5-260">Riziko</span><span class="sxs-lookup"><span data-stu-id="f54d5-260">Risk</span></span>|
| <span data-ttu-id="f54d5-261">Impact_s</span><span class="sxs-lookup"><span data-stu-id="f54d5-261">Impact_s</span></span>| <span data-ttu-id="f54d5-262">Dopad</span><span class="sxs-lookup"><span data-stu-id="f54d5-262">Impact</span></span>|
| <span data-ttu-id="f54d5-263">RequestedDate_t</span><span class="sxs-lookup"><span data-stu-id="f54d5-263">RequestedDate_t</span></span>  | <span data-ttu-id="f54d5-264">Požadovaná data</span><span class="sxs-lookup"><span data-stu-id="f54d5-264">Requested by date</span></span> |
| <span data-ttu-id="f54d5-265">ClosedDate_t</span><span class="sxs-lookup"><span data-stu-id="f54d5-265">ClosedDate_t</span></span> | <span data-ttu-id="f54d5-266">Datum uzavření</span><span class="sxs-lookup"><span data-stu-id="f54d5-266">Closed date</span></span> |
| <span data-ttu-id="f54d5-267">PlannedStartDate_t</span><span class="sxs-lookup"><span data-stu-id="f54d5-267">PlannedStartDate_t</span></span>  |     <span data-ttu-id="f54d5-268">Plánované počáteční datum</span><span class="sxs-lookup"><span data-stu-id="f54d5-268">Planned start date</span></span> |
| <span data-ttu-id="f54d5-269">PlannedEndDate_t</span><span class="sxs-lookup"><span data-stu-id="f54d5-269">PlannedEndDate_t</span></span>  |   <span data-ttu-id="f54d5-270">Plánované koncové datum</span><span class="sxs-lookup"><span data-stu-id="f54d5-270">Planned end date</span></span> |
| <span data-ttu-id="f54d5-271">WorkStartDate_t</span><span class="sxs-lookup"><span data-stu-id="f54d5-271">WorkStartDate_t</span></span>  | <span data-ttu-id="f54d5-272">Skutečné počáteční datum</span><span class="sxs-lookup"><span data-stu-id="f54d5-272">Actual start date</span></span> |
| <span data-ttu-id="f54d5-273">WorkEndDate_t</span><span class="sxs-lookup"><span data-stu-id="f54d5-273">WorkEndDate_t</span></span> | <span data-ttu-id="f54d5-274">Skutečné koncové datum</span><span class="sxs-lookup"><span data-stu-id="f54d5-274">Actual end date</span></span>|
| <span data-ttu-id="f54d5-275">Description_s</span><span class="sxs-lookup"><span data-stu-id="f54d5-275">Description_s</span></span> | <span data-ttu-id="f54d5-276">Popis</span><span class="sxs-lookup"><span data-stu-id="f54d5-276">Description</span></span> |
| <span data-ttu-id="f54d5-277">Počítač</span><span class="sxs-lookup"><span data-stu-id="f54d5-277">Computer</span></span>  | <span data-ttu-id="f54d5-278">Položky konfigurace</span><span class="sxs-lookup"><span data-stu-id="f54d5-278">Configuration Item</span></span> |

<span data-ttu-id="f54d5-279">**Ukázka obrazovky analýzy protokolů pro ITSM data:**</span><span class="sxs-lookup"><span data-stu-id="f54d5-279">**Sample Log Analytics screen for ITSM data:**</span></span>

![Obrazovka analýzy protokolů](./media/log-analytics-itsmc/itsmc-overview-sample-log-analytics.png)

## <a name="it-service-management-connector--integration-with-other-oms-solutions"></a><span data-ttu-id="f54d5-281">Konektoru Service Management IT – integrace s jinými řešeními OMS</span><span class="sxs-lookup"><span data-stu-id="f54d5-281">IT Service Management connector – integration with other OMS solutions</span></span>

<span data-ttu-id="f54d5-282">IT Service Management Connector aktuálně podporuje integraci s řešením mapy služeb.</span><span class="sxs-lookup"><span data-stu-id="f54d5-282">IT Service Management Connector, currently supports integration with the Service Map solution.</span></span>

<span data-ttu-id="f54d5-283">Mapa služeb automaticky vyhledá součásti aplikace v systémech Windows a Linux a mapuje komunikace mezi službami.</span><span class="sxs-lookup"><span data-stu-id="f54d5-283">Service Map automatically discovers the application components on Windows and Linux systems and maps the communication between services.</span></span> <span data-ttu-id="f54d5-284">Umožňuje zobrazit vaše servery co možná z nich – jako vzájemně propojena systémy, které doručují důležité služby.</span><span class="sxs-lookup"><span data-stu-id="f54d5-284">It allows you to view your servers as you think of them – as interconnected systems that deliver critical services.</span></span> <span data-ttu-id="f54d5-285">Mapy služeb zobrazí připojení mezi servery, procesy, a vyžaduje porty mezi žádné připojení TCP architektura žádnou konfiguraci, jiné než instalace agenta.</span><span class="sxs-lookup"><span data-stu-id="f54d5-285">Service Map shows connections between servers, processes, and ports across any TCP-connected architecture with no configuration required other than installation of an agent.</span></span> <span data-ttu-id="f54d5-286">Další informace: [mapy služeb](../operations-management-suite/operations-management-suite-service-map.md).</span><span class="sxs-lookup"><span data-stu-id="f54d5-286">More information: [Service Map](../operations-management-suite/operations-management-suite-service-map.md).</span></span>

<span data-ttu-id="f54d5-287">Díky této integraci můžete zobrazit položky služby podpory, které jsou vytvořené v ITSM řešení, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="f54d5-287">With this integration, you can view the service desk items created in the ITSM solutions as shown in the following example:</span></span>

![<span data-ttu-id="f54d5-288">Integrované řešení</span><span class="sxs-lookup"><span data-stu-id="f54d5-288">Integrated solution</span></span> ](./media/log-analytics-itsmc/itsmc-overview-integrated-solutions.png)
## <a name="create-itsm-work-items-for-oms-alerts"></a><span data-ttu-id="f54d5-289">Vytváření pracovních položek ITSM OMS výstrahy</span><span class="sxs-lookup"><span data-stu-id="f54d5-289">Create ITSM work items for OMS alerts</span></span>

<span data-ttu-id="f54d5-290">Pro OMS výstrahy můžete vytvořit přidružených pracovních položek v připojených ITSM zdroje.</span><span class="sxs-lookup"><span data-stu-id="f54d5-290">For the OMS alerts, you can create associated work items in the connected ITSM sources.</span></span>  <span data-ttu-id="f54d5-291">Chcete-li to provést, použijte následující postup:</span><span class="sxs-lookup"><span data-stu-id="f54d5-291">To do this, use the following procedure:</span></span>

1. <span data-ttu-id="f54d5-292">Z **hledání protokolů** okně Spustit protokolu vyhledávací dotaz. Chcete-li zobrazit data.</span><span class="sxs-lookup"><span data-stu-id="f54d5-292">From **Log Search** window, run a log search query to view data.</span></span> <span data-ttu-id="f54d5-293">Výsledky dotazu jsou zdroj pro pracovní položky.</span><span class="sxs-lookup"><span data-stu-id="f54d5-293">Query results are the source for work items.</span></span>
2. <span data-ttu-id="f54d5-294">V **hledání protokolů**, klikněte na tlačítko **výstrahy** otevřete **přidat pravidlo výstrahy** stránky.</span><span class="sxs-lookup"><span data-stu-id="f54d5-294">In **Log Search**, click **Alert** to open the **Add Alert Rule** page.</span></span>

    ![Obrazovka analýzy protokolů](./media/log-analytics-itsmc/itsmc-work-items-for-oms-alerts.png)

3. <span data-ttu-id="f54d5-296">Na **přidat pravidlo výstrahy** okno, zadejte požadované podrobnosti pro **název**, **závažnost**, **vyhledávací dotaz**, a **výstrah kritéria** (časová okna/metrika měří období).</span><span class="sxs-lookup"><span data-stu-id="f54d5-296">On the **Add Alert Rule** window, provide the required details for **Name**, **Severity**,  **Search query**, and **Alert criteria** (Time Window/Metric measurement).</span></span>
4. <span data-ttu-id="f54d5-297">Vyberte **Ano** pro **ITSM akce**.</span><span class="sxs-lookup"><span data-stu-id="f54d5-297">Select **Yes** for **ITSM Actions**.</span></span>
5. <span data-ttu-id="f54d5-298">Vyberte ITSM připojení z **vyberte připojení** seznamu.</span><span class="sxs-lookup"><span data-stu-id="f54d5-298">Select your ITSM connection from the **Select Connection** list.</span></span>
6. <span data-ttu-id="f54d5-299">Zadejte podrobnosti podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="f54d5-299">Provide the details as required.</span></span>
7. <span data-ttu-id="f54d5-300">Vytvořit samostatný pracovní položku pro každé položky protokolu této výstrahy, vyberte **vytvořit jednotlivé pracovní položky pro každý záznam protokolu** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="f54d5-300">To create a separate work item for each log entry of this alert, select the **Create individual work items for each log entry** checkbox.</span></span>

    <span data-ttu-id="f54d5-301">Nebo</span><span class="sxs-lookup"><span data-stu-id="f54d5-301">Or</span></span>

    <span data-ttu-id="f54d5-302">nechte toto políčko Zrušit vytvořit pouze jeden pracovní položku pro libovolný počet záznamů protokolu v rámci této výstrahy.</span><span class="sxs-lookup"><span data-stu-id="f54d5-302">leave this checkbox unselected to create only one work item for any number of log entries under this alert.</span></span>

7. <span data-ttu-id="f54d5-303">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f54d5-303">Click **Save**.</span></span>

<span data-ttu-id="f54d5-304">Bude vytvořena výstraha OMS ve **výstrahy**.</span><span class="sxs-lookup"><span data-stu-id="f54d5-304">The OMS alert will be created under **Alerts**.</span></span> <span data-ttu-id="f54d5-305">Pracovní položky odpovídající ITSM připojení vytvářejí, když je splněna podmínka zadaný výstrahy.</span><span class="sxs-lookup"><span data-stu-id="f54d5-305">The corresponding ITSM connection's work items are created when the specified alert's condition is met.</span></span>

## <a name="create-itsm-work-items-from-oms-logs"></a><span data-ttu-id="f54d5-306">Vytváření pracovních položek ITSM z protokolů OMS</span><span class="sxs-lookup"><span data-stu-id="f54d5-306">Create ITSM work items from OMS logs</span></span>

<span data-ttu-id="f54d5-307">Pracovní položky můžete vytvořit v připojených zdrojů ITSM pomocí vyhledávání protokolu OMS.</span><span class="sxs-lookup"><span data-stu-id="f54d5-307">You can create work items in the connected ITSM sources by using OMS Log Search.</span></span> <span data-ttu-id="f54d5-308">Chcete-li to provést, použijte následující postup:</span><span class="sxs-lookup"><span data-stu-id="f54d5-308">To do this, use the following procedure:</span></span>

1. <span data-ttu-id="f54d5-309">Z **hledání protokolů**hledání požadovaných dat, vyberte podrobností a klikněte na tlačítko **vytvořit pracovní položka**.</span><span class="sxs-lookup"><span data-stu-id="f54d5-309">From **Log Search**,  search the required data, select the detail, and click **Create work item**.</span></span>

    <span data-ttu-id="f54d5-310">**Vytvořit ITSM pracovní položka** zobrazí se okno:</span><span class="sxs-lookup"><span data-stu-id="f54d5-310">The **Create ITSM Work item** window appears:</span></span>

    ![Obrazovka analýzy protokolů](media/log-analytics-itsmc/itsmc-work-items-from-oms-logs.png)

2.   <span data-ttu-id="f54d5-312">Přidejte následující podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="f54d5-312">Add the following details:</span></span>

  - <span data-ttu-id="f54d5-313">**Název pracovní položky**: název pro pracovní položku.</span><span class="sxs-lookup"><span data-stu-id="f54d5-313">**Work item Title**: Title for the work item.</span></span>
  - <span data-ttu-id="f54d5-314">**Pracovní položky Popis**: popis novou pracovní položku.</span><span class="sxs-lookup"><span data-stu-id="f54d5-314">**Work item Description**: Description for the new work item.</span></span>
  - <span data-ttu-id="f54d5-315">**Vliv na počítače**: název počítače, kde tato data protokolu byla nalezena.</span><span class="sxs-lookup"><span data-stu-id="f54d5-315">**Affected Computer**: Name of the computer where this log data was found.</span></span>
  - <span data-ttu-id="f54d5-316">**Vyberte připojení**: ITSM připojení, ve kterém chcete vytvořit tuto pracovní položku.</span><span class="sxs-lookup"><span data-stu-id="f54d5-316">**Select Connection**:  ITSM connection in which you want to create this work item.</span></span>
  - <span data-ttu-id="f54d5-317">**Pracovní položka**: typ pracovní položky.</span><span class="sxs-lookup"><span data-stu-id="f54d5-317">**Work item**:  Type of work item.</span></span>

3. <span data-ttu-id="f54d5-318">Chcete-li použít existující šablonu pracovní položky pro incidentu, klikněte na tlačítko **Ano** pod **generování pracovní položka založený na šabloně** a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f54d5-318">To use an existing work item template for an incident, click **Yes** under **Generate work item based on the template** option and then click **Create**.</span></span>

    <span data-ttu-id="f54d5-319">Nebo:</span><span class="sxs-lookup"><span data-stu-id="f54d5-319">Or,</span></span>

    <span data-ttu-id="f54d5-320">Klikněte na tlačítko **ne** Pokud chcete zadat vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f54d5-320">Click **No** if you want to provide your customized values.</span></span>

4. <span data-ttu-id="f54d5-321">Zadejte odpovídající hodnoty v **typu Kontakt**, **dopad**, **naléhavost**, **kategorie**, a **dílčí kategorie** textová pole a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f54d5-321">Provide the appropriate values in the **Contact Type**, **Impact**, **Urgency**, **Category**, and **Sub Category** text boxes, and then click **Create**.</span></span>

<span data-ttu-id="f54d5-322">Pracovní položka bude vytvořen v ITSM, které lze také prohlížet v OMS.</span><span class="sxs-lookup"><span data-stu-id="f54d5-322">The work item will be created in the ITSM, which you can also view in OMS.</span></span>

## <a name="troubleshoot-itsm-connections-in-oms"></a><span data-ttu-id="f54d5-323">Řešení potíží s ITSM připojení v OMS</span><span class="sxs-lookup"><span data-stu-id="f54d5-323">Troubleshoot ITSM connections in OMS</span></span>
1.  <span data-ttu-id="f54d5-324">Pokud připojení selže z připojeného zdroje uživatelského rozhraní a můžete získat **Chyba při ukládání připojení** zprávy, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="f54d5-324">If connection fails from connected source's UI and you get the **Error in saving connection** message, do the following:</span></span>
 - <span data-ttu-id="f54d5-325">V případě připojení ServiceNow, Cherwell a Provance Ujistěte se, že jste správně zadali uživatelské jméno a heslo a klient ID nebo klient tajný klíč pro jednotlivá připojení.</span><span class="sxs-lookup"><span data-stu-id="f54d5-325">In case of ServiceNow, Cherwell and Provance connections, ensure you correctly entered  the username/password and  client ID/client secret  for each of the connections.</span></span> <span data-ttu-id="f54d5-326">Pokud chyba přetrvává, zkontrolujte, pokud máte dostatečná oprávnění v rámci odpovídající ITSM produktu pro připojení.</span><span class="sxs-lookup"><span data-stu-id="f54d5-326">If the error persists, check if you have sufficient privileges  in the corresponding ITSM product to make the connection.</span></span>
 - <span data-ttu-id="f54d5-327">V případě portálu Service Manager, zajistěte, aby webová aplikace je úspěšně nasazen a hybridní připojení se vytvoří.</span><span class="sxs-lookup"><span data-stu-id="f54d5-327">In case of Service Manager, ensure that the Web app is successfully deployed and hybrid connection is created.</span></span> <span data-ttu-id="f54d5-328">Ověřte připojení se úspěšně naváže na místní počítač portálu Service Manager, najdete na adresu URL webové aplikace podle popisu v dokumentaci k provádění [hybridní připojení](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).</span><span class="sxs-lookup"><span data-stu-id="f54d5-328">To verify the connection is successfully established with the on-prem Service Manager machine, visit the  Web app URL as detailed in the documentation for making the [hybrid connection](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).</span></span>

2.  <span data-ttu-id="f54d5-329">Pokud není získávání synchronizovat data z ServiceNow v OMS, zajistěte, aby ServiceNow instance není pozastaveno.</span><span class="sxs-lookup"><span data-stu-id="f54d5-329">If data from ServiceNow is not getting synced in OMS, ensure that the ServiceNow instance is not sleeping.</span></span> <span data-ttu-id="f54d5-330">K tomu může dojít chvíli v instance ServiceNow vývojářů, při nečinnosti.</span><span class="sxs-lookup"><span data-stu-id="f54d5-330">This might sometime happen in the ServiceNow Dev instances, when idle.</span></span> <span data-ttu-id="f54d5-331">Jinak ohlaste daný problém.</span><span class="sxs-lookup"><span data-stu-id="f54d5-331">Else, report the issue.</span></span>
3.  <span data-ttu-id="f54d5-332">Pokud při získávání vyvolání výstrahy od OMS, ale pracovní položky nejsou získávání vytvořené v ITSM produktu nebo konfigurace položek nezobrazují vytvořen nebo propojené pracovní položky nebo nějaké obecné informace o, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="f54d5-332">If Alerts are getting fired from OMS but work items are not getting created in ITSM product or configuration items are not getting created/linked to work items or for any generic information, do the following:</span></span>
 -  <span data-ttu-id="f54d5-333">Řešení pro správu konektoru služby IT na portálu OMS může použít k získání shrnutí připojení nebo pracovní položky nebo počítače atd. Klikněte na tlačítko chybová zpráva v okně Stav, přejděte na **hledání protokolů** a zobrazit připojení, který má chyba pomocí podrobnosti v chybové zprávě.</span><span class="sxs-lookup"><span data-stu-id="f54d5-333">IT Service Management Connector solution in OMS portal could be used to get a summary of connections/work items/computers etc. Click the error message in the status blade, navigate to **Log Search** and view the connection that has the error by using the details in the error message.</span></span>
 - <span data-ttu-id="f54d5-334">přímo můžete zobrazit informace týkající se nebo chyby v **hledání protokolů** stránky pomocí *typ = ServiceDeskLog_CL*.</span><span class="sxs-lookup"><span data-stu-id="f54d5-334">you can directly view the errors/related information in the **Log Search** page using *Type=ServiceDeskLog_CL*.</span></span>

## <a name="troubleshoot-service-manager-web-app-deployment"></a><span data-ttu-id="f54d5-335">Řešení potíží s nasazení aplikace webového portálu Service Manager</span><span class="sxs-lookup"><span data-stu-id="f54d5-335">Troubleshoot Service Manager Web App deployment</span></span>
1.  <span data-ttu-id="f54d5-336">V případě jakékoli potíže při nasazení webové aplikace zkontrolujte, zda že máte dostatečná oprávnění v rámci předplatného uvedených vytvořit nebo nasadit prostředky.</span><span class="sxs-lookup"><span data-stu-id="f54d5-336">In case of any trouble with web app deployment, ensure you have sufficient permissions in the subscription mentioned to create/deploy resources.</span></span>
2.  <span data-ttu-id="f54d5-337">Pokud **objektu odkaz není nastaven na instanci objektu** chybová zpráva se zobrazí při spuštění [skriptu](log-analytics-itsmc-service-manager-script.md) zkontrolujte, zda jste zadali platné hodnoty v části **konfigurace uživatele** oddíl.</span><span class="sxs-lookup"><span data-stu-id="f54d5-337">If **Object reference not set to instance of an object** error message appears while running the [script](log-analytics-itsmc-service-manager-script.md) ensure that you entered valid values  under **User Configuration** section.</span></span>
3.  <span data-ttu-id="f54d5-338">Pokud se vám nepodaří vytvoření oboru názvů service bus relay, ujistěte se, že zprostředkovatel požadovaný prostředek je zaregistrován v rámci předplatného.</span><span class="sxs-lookup"><span data-stu-id="f54d5-338">If you fail to create service bus relay namespace, ensure that the required resource provider is registered in the subscription.</span></span> <span data-ttu-id="f54d5-339">Pokud není zaregistrovaný, ručně vytvořte ho z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f54d5-339">If not registered, manually create it from the Azure portal.</span></span> <span data-ttu-id="f54d5-340">Můžete také vytvořit ji při [vytvoření hybridního připojení](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f54d5-340">You can also create it while [creating the hybrid connection](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) from the Azure portal.</span></span>


## <a name="contact-us"></a><span data-ttu-id="f54d5-341">Kontaktujte nás</span><span class="sxs-lookup"><span data-stu-id="f54d5-341">Contact us</span></span>

<span data-ttu-id="f54d5-342">Pro dotazy nebo zpětnou vazbu na správu konektoru služby IT, kontaktujte nás na adrese [ omsitsmfeedback@microsoft.com ](mailto:omsitsmfeedback@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="f54d5-342">For any queries or feedback on the IT Service Management Connector, contact us at [omsitsmfeedback@microsoft.com](mailto:omsitsmfeedback@microsoft.com).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f54d5-343">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f54d5-343">Next steps</span></span>
<span data-ttu-id="f54d5-344">[Přidání ITSM produktů nebo služeb do konektoru služby správy IT](log-analytics-itsmc-connections.md).</span><span class="sxs-lookup"><span data-stu-id="f54d5-344">[Add ITSM products/services to IT Service Management Connector](log-analytics-itsmc-connections.md).</span></span>
