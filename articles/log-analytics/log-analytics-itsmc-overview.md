---
title: aaaIT konektoru Management Service v OMS | Microsoft Docs
description: "Použít hello IT Service Management Connector toocentrally monitorování a správě hello ITSM pracovních položek v OMS a rychle vyřešit všechny problémy."
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
ms.openlocfilehash: 33ed5d432591b836eb41ba982c66c96f22879444
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="centrally-manage-itsm-work-items-using-it-service-management-connector-preview"></a><span data-ttu-id="0ed94-103">Centrálně spravovat ITSM pracovních položek pomocí IT Service Management Connector (Preview)</span><span class="sxs-lookup"><span data-stu-id="0ed94-103">Centrally manage ITSM work items using IT Service Management Connector (Preview)</span></span>

![Symbol konektoru služby správy IT](./media/log-analytics-itsmc/itsmc-symbol.png)

<span data-ttu-id="0ed94-105">Můžete použít hello IT služby správy konektoru (ITSMC) v nástroji Sledování toocentrally OMS analýzy protokolů a spravovat pracovní položky v rámci ITSM produktů nebo služeb.</span><span class="sxs-lookup"><span data-stu-id="0ed94-105">You can use hello IT Service Management Connector (ITSMC) in OMS Log Analytics toocentrally monitor and manage work items across your ITSM products/services.</span></span>

<span data-ttu-id="0ed94-106">Hello IT Service Management Connector se integruje s analýzy protokolů OMS existující IT služby správy (ITSM) produkty a služby.</span><span class="sxs-lookup"><span data-stu-id="0ed94-106">hello IT Service Management Connector integrates your existing IT Service Management (ITSM) products and services with OMS Log Analytics.</span></span>  <span data-ttu-id="0ed94-107">řešení Hello má obousměrnou integraci s ITSM produktů nebo služeb, kde poskytuje hello OMS uživatele možnost toocreate incidenty, výstrahy nebo události v ITSM řešení.</span><span class="sxs-lookup"><span data-stu-id="0ed94-107">hello solution has bidirectional integration with ITSM products/services, where it provides hello OMS users an option toocreate incidents, alerts, or events in ITSM solution.</span></span> <span data-ttu-id="0ed94-108">Hello konektor importuje také data, jako jsou incidenty a žádostí o změny z ITSM řešení do analýzy protokolů OMS.</span><span class="sxs-lookup"><span data-stu-id="0ed94-108">hello connector also  imports data such as incidents, and change requests from ITSM solution into OMS Log Analytics.</span></span>

<span data-ttu-id="0ed94-109">Pomocí konektoru služby správy IT můžete:</span><span class="sxs-lookup"><span data-stu-id="0ed94-109">With IT Service Management Connector, you can:</span></span>

  - <span data-ttu-id="0ed94-110">Centrálně monitorovat a spravovat pracovní položky pro ITSM produkty nebo služby používané ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="0ed94-110">Centrally monitor and manage work items for ITSM products/services used across your organization.</span></span>
  - <span data-ttu-id="0ed94-111">Vytvoření ITSM pracovních položek (například výstrahy, události, incident) v ITSM z OMS výstrah a prostřednictvím protokolu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="0ed94-111">Create ITSM work items (like alert, event, incident) in ITSM from OMS alerts and through log search.</span></span>
  - <span data-ttu-id="0ed94-112">Čtení incidenty a žádosti o změnu z řešení ITSM a korelovat s daty relevantní protokolu v pracovní prostor analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="0ed94-112">Read incidents and change requests from your ITSM solution and correlate with relevant log data in Log Analytics workspace.</span></span>
  - <span data-ttu-id="0ed94-113">Najde všechny neočekávanou a neobvyklé události a vyřešit, ještě před hello koncoví uživatelé volání a sestav je toohello technickou podporu.</span><span class="sxs-lookup"><span data-stu-id="0ed94-113">Find any unexpected and unusual events and resolve them, even before hello end users call and report them toohello helpdesk.</span></span>
  - <span data-ttu-id="0ed94-114">Importovat data pracovních položek do analýzy protokolů a vytváření sestav výkonnosti klíče pro Klíčový ukazatel výkonu.</span><span class="sxs-lookup"><span data-stu-id="0ed94-114">Import work items data into Log Analytics and create key performance indicator (KPI) reports.</span></span>  <span data-ttu-id="0ed94-115">Pomocí těchto sestav, můžete identifikovat, hodnocení a fungovat na některé důležité skutečnosti, jako je například posouzením malwaru.</span><span class="sxs-lookup"><span data-stu-id="0ed94-115">Using these reports, you can identify, assess and act on several important items such as malware assessment.</span></span>
  - <span data-ttu-id="0ed94-116">Zobrazit kurátorované řídicí panely pro podrobnější přehled na incidenty, žádostí o změnu a ovlivněné systémy.</span><span class="sxs-lookup"><span data-stu-id="0ed94-116">View curated dashboards for deeper insights on incidents, change requests and impacted systems.</span></span>
  - <span data-ttu-id="0ed94-117">Řešení potíží s rychlejší pomocí souvztažnosti pomocí jiných řešení pro správu v pracovní prostor analýzy protokolů hello.</span><span class="sxs-lookup"><span data-stu-id="0ed94-117">Troubleshoot faster by correlating with other management solutions in hello Log Analytics workspace.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="0ed94-118">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0ed94-118">Prerequisites</span></span>

<span data-ttu-id="0ed94-119">tooimport hello ITSM pracovních položek do OMS analýzy protokolů, hello řešení vyžaduje připojení mezi hello konektoru služby správy IT v hello OMS a IT SM hello produktům a službám, ze kterých importujete hello pracovní položky.</span><span class="sxs-lookup"><span data-stu-id="0ed94-119">tooimport hello ITSM work items into OMS Log Analytics, hello solution requires a connection between hello IT Service Management Connector in hello OMS and hello IT SM product/service from which you import hello work items.</span></span>


## <a name="configuration"></a><span data-ttu-id="0ed94-120">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="0ed94-120">Configuration</span></span>

<span data-ttu-id="0ed94-121">Přidat hello IT Service Management Connector řešení tooyour OMS pracovní prostor, pomocí hello procesu popsaného v tématu [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="0ed94-121">Add hello IT Service Management Connector solution tooyour OMS work space, using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span>

<span data-ttu-id="0ed94-122">Konektoru Service Management IT dlaždici, jak vidíte v Galerii řešení hello:</span><span class="sxs-lookup"><span data-stu-id="0ed94-122">IT Service Management Connector tile as you see in hello Solutions gallery:</span></span>

![dlaždice konektoru](./media/log-analytics-itsmc/itsmc-solutions-tile.png)

<span data-ttu-id="0ed94-124">Po úspěšném přidání, uvidíte hello správu konektoru služby IT v rámci **OMS** > **nastavení** > **připojené zdroje.**</span><span class="sxs-lookup"><span data-stu-id="0ed94-124">After successful addition, you will see hello IT Service Management Connector under **OMS** > **Settings** > **Connected Sources.**</span></span>

![ITSMC připojení](./media/log-analytics-itsmc/itsmc-overview-solution-in-connected-sources.png)

> [!NOTE]

> <span data-ttu-id="0ed94-126">Ve výchozím nastavení hello IT Service Management Connector aktualizuje data hello připojení jednou za každých 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="0ed94-126">By default, hello IT Service Management Connector refreshes hello connection's data once in every 24 hours.</span></span> <span data-ttu-id="0ed94-127">toorefresh data vaše připojení okamžitě pro všechny úpravy nebo šablonu aktualizace, které provedete, klikněte na tlačítko hello aktualizace tlačítko zobrazí další tooyour připojení.</span><span class="sxs-lookup"><span data-stu-id="0ed94-127">toorefresh your connection's data instantly for any edits or template updates that you make, click hello refresh button displayed next tooyour connection.</span></span>

 ![Aktualizace ITSMC](./media/log-analytics-itsmc/itsmc-connection-refresh.png)

## <a name="management-packs"></a><span data-ttu-id="0ed94-129">Sady Management Pack</span><span class="sxs-lookup"><span data-stu-id="0ed94-129">Management packs</span></span>
<span data-ttu-id="0ed94-130">Toto řešení nevyžaduje žádné sady management Pack.</span><span class="sxs-lookup"><span data-stu-id="0ed94-130">This solution does not require any management packs.</span></span>

## <a name="connected-sources"></a><span data-ttu-id="0ed94-131">Připojené zdroje</span><span class="sxs-lookup"><span data-stu-id="0ed94-131">Connected sources</span></span>

<span data-ttu-id="0ed94-132">Hello následující ITSM produkty nebo služby podporuje hello konektoru služby správy IT:</span><span class="sxs-lookup"><span data-stu-id="0ed94-132">hello following ITSM products/services are supported by hello IT Service Management Connector:</span></span>

- [<span data-ttu-id="0ed94-133">System Center Service Manager (SCSM)</span><span class="sxs-lookup"><span data-stu-id="0ed94-133">System Center Service Manager (SCSM)</span></span>](log-analytics-itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-oms)

- [<span data-ttu-id="0ed94-134">ServiceNow</span><span class="sxs-lookup"><span data-stu-id="0ed94-134">ServiceNow</span></span>](log-analytics-itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-oms)

- [<span data-ttu-id="0ed94-135">Provance</span><span class="sxs-lookup"><span data-stu-id="0ed94-135">Provance</span></span>](log-analytics-itsmc-connections.md#connect-provance-to-it-service-management-connector-in-oms)  

- [<span data-ttu-id="0ed94-136">Cherwell</span><span class="sxs-lookup"><span data-stu-id="0ed94-136">Cherwell</span></span>](log-analytics-itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-oms)

## <a name="using-hello-solution"></a><span data-ttu-id="0ed94-137">Pomocí řešení hello</span><span class="sxs-lookup"><span data-stu-id="0ed94-137">Using hello solution</span></span>

<span data-ttu-id="0ed94-138">Jakmile se připojíte hello OMS IT Service Management Connector s ITSM službou, hello analýzy protokolů služby spustí, shromažďování dat hello z hello připojené ITSM produkty nebo služby.</span><span class="sxs-lookup"><span data-stu-id="0ed94-138">Once you connect hello OMS IT Service Management Connector with your ITSM service, hello Log Analytics services starts gathering hello data from hello connected ITSM products/service.</span></span>

> [!NOTE]
> - <span data-ttu-id="0ed94-139">Dat importovaných pomocí konektoru služby správy IT řešení se zobrazí v analýzy protokolů jako události s názvem **ServiceDesk_CL**.</span><span class="sxs-lookup"><span data-stu-id="0ed94-139">Data imported by IT Service Management Connector solution appears in Log Analytics as events named **ServiceDesk_CL**.</span></span>
- <span data-ttu-id="0ed94-140">Událost obsahuje pole s názvem **ServiceDeskWorkItemType_s**.</span><span class="sxs-lookup"><span data-stu-id="0ed94-140">Event contains a field named **ServiceDeskWorkItemType_s**.</span></span> <span data-ttu-id="0ed94-141">které trvat jeho hodnotu jako incident nebo žádost o změnu, v závislosti na hello pracovních položek data obsažená v hello **ServiceDesk_CL** událostí.</span><span class="sxs-lookup"><span data-stu-id="0ed94-141">which can take its value as incident, or change request, depending on hello work item data contained in hello **ServiceDesk_CL** event.</span></span>

## <a name="input-data"></a><span data-ttu-id="0ed94-142">Vstupní data</span><span class="sxs-lookup"><span data-stu-id="0ed94-142">Input data</span></span>
<span data-ttu-id="0ed94-143">Pracovní položky importovány z hello ITSM produktů nebo služeb.</span><span class="sxs-lookup"><span data-stu-id="0ed94-143">Work items imported from hello ITSM products/services.</span></span>

<span data-ttu-id="0ed94-144">Hello následující informace uvádí příklady data shromážděná konektorem hello správy služeb IT:</span><span class="sxs-lookup"><span data-stu-id="0ed94-144">hello following information shows examples of data gathered by hello IT Service Management connector:</span></span>

> [!NOTE]
> <span data-ttu-id="0ed94-145">V závislosti na hello typ pracovní položky naimportovat do analýzy protokolů **ServiceDesk_CL** obsahuje hello následující pole:</span><span class="sxs-lookup"><span data-stu-id="0ed94-145">Depending on hello work item type imported into Log Analytics, **ServiceDesk_CL** contains hello following fields:</span></span>

<span data-ttu-id="0ed94-146">**Pracovní položka:** **incidenty**</span><span class="sxs-lookup"><span data-stu-id="0ed94-146">**Work item:** **Incidents**</span></span>  
<span data-ttu-id="0ed94-147">ServiceDeskWorkItemType_s = "Incidentem"</span><span class="sxs-lookup"><span data-stu-id="0ed94-147">ServiceDeskWorkItemType_s="Incident"</span></span>

<span data-ttu-id="0ed94-148">**Pole**</span><span class="sxs-lookup"><span data-stu-id="0ed94-148">**Fields**</span></span>

- <span data-ttu-id="0ed94-149">ServiceDeskConnectionName</span><span class="sxs-lookup"><span data-stu-id="0ed94-149">ServiceDeskConnectionName</span></span>
- <span data-ttu-id="0ed94-150">ID služby podpory</span><span class="sxs-lookup"><span data-stu-id="0ed94-150">Service Desk ID</span></span>
- <span data-ttu-id="0ed94-151">Stav</span><span class="sxs-lookup"><span data-stu-id="0ed94-151">State</span></span>
- <span data-ttu-id="0ed94-152">Naléhavosti</span><span class="sxs-lookup"><span data-stu-id="0ed94-152">Urgency</span></span>
- <span data-ttu-id="0ed94-153">Dopad</span><span class="sxs-lookup"><span data-stu-id="0ed94-153">Impact</span></span>
- <span data-ttu-id="0ed94-154">Priorita</span><span class="sxs-lookup"><span data-stu-id="0ed94-154">Priority</span></span>
- <span data-ttu-id="0ed94-155">Eskalaci</span><span class="sxs-lookup"><span data-stu-id="0ed94-155">Escalation</span></span>
- <span data-ttu-id="0ed94-156">Autor</span><span class="sxs-lookup"><span data-stu-id="0ed94-156">Created By</span></span>
- <span data-ttu-id="0ed94-157">Vyřešil</span><span class="sxs-lookup"><span data-stu-id="0ed94-157">Resolved By</span></span>
- <span data-ttu-id="0ed94-158">Uzavřené</span><span class="sxs-lookup"><span data-stu-id="0ed94-158">Closed By</span></span>
- <span data-ttu-id="0ed94-159">Zdroj</span><span class="sxs-lookup"><span data-stu-id="0ed94-159">Source</span></span>
- <span data-ttu-id="0ed94-160">Přiřazené</span><span class="sxs-lookup"><span data-stu-id="0ed94-160">Assigned To</span></span>
- <span data-ttu-id="0ed94-161">Kategorie</span><span class="sxs-lookup"><span data-stu-id="0ed94-161">Category</span></span>
- <span data-ttu-id="0ed94-162">Název</span><span class="sxs-lookup"><span data-stu-id="0ed94-162">Title</span></span>
- <span data-ttu-id="0ed94-163">Popis</span><span class="sxs-lookup"><span data-stu-id="0ed94-163">Description</span></span>
- <span data-ttu-id="0ed94-164">Datum vytvoření</span><span class="sxs-lookup"><span data-stu-id="0ed94-164">Created Date</span></span>
- <span data-ttu-id="0ed94-165">Datum uzavření</span><span class="sxs-lookup"><span data-stu-id="0ed94-165">Closed Date</span></span>
- <span data-ttu-id="0ed94-166">Datum vyřešení</span><span class="sxs-lookup"><span data-stu-id="0ed94-166">Resolved Date</span></span>
- <span data-ttu-id="0ed94-167">Datum poslední změny</span><span class="sxs-lookup"><span data-stu-id="0ed94-167">Last Modified Date</span></span>
- <span data-ttu-id="0ed94-168">Počítač</span><span class="sxs-lookup"><span data-stu-id="0ed94-168">Computer</span></span>


<span data-ttu-id="0ed94-169">**Pracovní položka:** **žádosti o změnu**</span><span class="sxs-lookup"><span data-stu-id="0ed94-169">**Work item:** **Change Requests**</span></span>

<span data-ttu-id="0ed94-170">ServiceDeskWorkItemType_s = "ChangeRequest"</span><span class="sxs-lookup"><span data-stu-id="0ed94-170">ServiceDeskWorkItemType_s="ChangeRequest"</span></span>

<span data-ttu-id="0ed94-171">**Pole**</span><span class="sxs-lookup"><span data-stu-id="0ed94-171">**Fields**</span></span>
- <span data-ttu-id="0ed94-172">ServiceDeskConnectionName</span><span class="sxs-lookup"><span data-stu-id="0ed94-172">ServiceDeskConnectionName</span></span>
- <span data-ttu-id="0ed94-173">ID služby podpory</span><span class="sxs-lookup"><span data-stu-id="0ed94-173">Service Desk ID</span></span>
- <span data-ttu-id="0ed94-174">Autor</span><span class="sxs-lookup"><span data-stu-id="0ed94-174">Created By</span></span>
- <span data-ttu-id="0ed94-175">Uzavřené</span><span class="sxs-lookup"><span data-stu-id="0ed94-175">Closed By</span></span>
- <span data-ttu-id="0ed94-176">Zdroj</span><span class="sxs-lookup"><span data-stu-id="0ed94-176">Source</span></span>
- <span data-ttu-id="0ed94-177">Přiřazené</span><span class="sxs-lookup"><span data-stu-id="0ed94-177">Assigned To</span></span>
- <span data-ttu-id="0ed94-178">Název</span><span class="sxs-lookup"><span data-stu-id="0ed94-178">Title</span></span>
- <span data-ttu-id="0ed94-179">Typ</span><span class="sxs-lookup"><span data-stu-id="0ed94-179">Type</span></span>
- <span data-ttu-id="0ed94-180">Kategorie</span><span class="sxs-lookup"><span data-stu-id="0ed94-180">Category</span></span>
- <span data-ttu-id="0ed94-181">Stav</span><span class="sxs-lookup"><span data-stu-id="0ed94-181">State</span></span>
- <span data-ttu-id="0ed94-182">Eskalaci</span><span class="sxs-lookup"><span data-stu-id="0ed94-182">Escalation</span></span>
- <span data-ttu-id="0ed94-183">Konflikt stavu</span><span class="sxs-lookup"><span data-stu-id="0ed94-183">Conflict Status</span></span>
- <span data-ttu-id="0ed94-184">Naléhavosti</span><span class="sxs-lookup"><span data-stu-id="0ed94-184">Urgency</span></span>
- <span data-ttu-id="0ed94-185">Priorita</span><span class="sxs-lookup"><span data-stu-id="0ed94-185">Priority</span></span>
- <span data-ttu-id="0ed94-186">Riziko</span><span class="sxs-lookup"><span data-stu-id="0ed94-186">Risk</span></span>
- <span data-ttu-id="0ed94-187">Dopad</span><span class="sxs-lookup"><span data-stu-id="0ed94-187">Impact</span></span>
- <span data-ttu-id="0ed94-188">Přiřazené</span><span class="sxs-lookup"><span data-stu-id="0ed94-188">Assigned To</span></span>
- <span data-ttu-id="0ed94-189">Datum vytvoření</span><span class="sxs-lookup"><span data-stu-id="0ed94-189">Created Date</span></span>
- <span data-ttu-id="0ed94-190">Datum uzavření</span><span class="sxs-lookup"><span data-stu-id="0ed94-190">Closed Date</span></span>
- <span data-ttu-id="0ed94-191">Datum poslední změny</span><span class="sxs-lookup"><span data-stu-id="0ed94-191">Last Modified Date</span></span>
- <span data-ttu-id="0ed94-192">Požadovaná data</span><span class="sxs-lookup"><span data-stu-id="0ed94-192">Requested Date</span></span>
- <span data-ttu-id="0ed94-193">Plánované počáteční datum</span><span class="sxs-lookup"><span data-stu-id="0ed94-193">Planned Start Date</span></span>
- <span data-ttu-id="0ed94-194">Naplánované koncové datum</span><span class="sxs-lookup"><span data-stu-id="0ed94-194">Planned End Date</span></span>
- <span data-ttu-id="0ed94-195">Pracovní počáteční datum</span><span class="sxs-lookup"><span data-stu-id="0ed94-195">Work Start Date</span></span>
- <span data-ttu-id="0ed94-196">Pracovní koncové datum</span><span class="sxs-lookup"><span data-stu-id="0ed94-196">Work End Date</span></span>
- <span data-ttu-id="0ed94-197">Popis</span><span class="sxs-lookup"><span data-stu-id="0ed94-197">Description</span></span>
- <span data-ttu-id="0ed94-198">Počítač</span><span class="sxs-lookup"><span data-stu-id="0ed94-198">Computer</span></span>

## <a name="output-data-for-a-servicenow-incident"></a><span data-ttu-id="0ed94-199">Výstupní data pro ServiceNow incident</span><span class="sxs-lookup"><span data-stu-id="0ed94-199">Output data for a ServiceNow incident</span></span>

| <span data-ttu-id="0ed94-200">Pole OMS</span><span class="sxs-lookup"><span data-stu-id="0ed94-200">OMS field</span></span> | <span data-ttu-id="0ed94-201">ITSM pole</span><span class="sxs-lookup"><span data-stu-id="0ed94-201">ITSM field</span></span> |
|:--- |:--- |
| <span data-ttu-id="0ed94-202">ServiceDeskId_s</span><span class="sxs-lookup"><span data-stu-id="0ed94-202">ServiceDeskId_s</span></span>| <span data-ttu-id="0ed94-203">Číslo</span><span class="sxs-lookup"><span data-stu-id="0ed94-203">Number</span></span> |
| <span data-ttu-id="0ed94-204">IncidentState_s</span><span class="sxs-lookup"><span data-stu-id="0ed94-204">IncidentState_s</span></span> | <span data-ttu-id="0ed94-205">Stav</span><span class="sxs-lookup"><span data-stu-id="0ed94-205">State</span></span> |
| <span data-ttu-id="0ed94-206">Urgency_s</span><span class="sxs-lookup"><span data-stu-id="0ed94-206">Urgency_s</span></span> |<span data-ttu-id="0ed94-207">Naléhavosti</span><span class="sxs-lookup"><span data-stu-id="0ed94-207">Urgency</span></span> |
| <span data-ttu-id="0ed94-208">Impact_s</span><span class="sxs-lookup"><span data-stu-id="0ed94-208">Impact_s</span></span> |<span data-ttu-id="0ed94-209">Dopad</span><span class="sxs-lookup"><span data-stu-id="0ed94-209">Impact</span></span>|
| <span data-ttu-id="0ed94-210">Priority_s</span><span class="sxs-lookup"><span data-stu-id="0ed94-210">Priority_s</span></span> | <span data-ttu-id="0ed94-211">Priorita</span><span class="sxs-lookup"><span data-stu-id="0ed94-211">Priority</span></span> |
| <span data-ttu-id="0ed94-212">CreatedBy_s</span><span class="sxs-lookup"><span data-stu-id="0ed94-212">CreatedBy_s</span></span> | <span data-ttu-id="0ed94-213">Otevřít</span><span class="sxs-lookup"><span data-stu-id="0ed94-213">Opened by</span></span> |
| <span data-ttu-id="0ed94-214">ResolvedBy_s</span><span class="sxs-lookup"><span data-stu-id="0ed94-214">ResolvedBy_s</span></span> | <span data-ttu-id="0ed94-215">Vyřešil</span><span class="sxs-lookup"><span data-stu-id="0ed94-215">Resolved by</span></span>|
| <span data-ttu-id="0ed94-216">ClosedBy_s</span><span class="sxs-lookup"><span data-stu-id="0ed94-216">ClosedBy_s</span></span>  | <span data-ttu-id="0ed94-217">Uzavřené</span><span class="sxs-lookup"><span data-stu-id="0ed94-217">Closed by</span></span> |
| <span data-ttu-id="0ed94-218">Source_s</span><span class="sxs-lookup"><span data-stu-id="0ed94-218">Source_s</span></span>| <span data-ttu-id="0ed94-219">Obraťte se na typ</span><span class="sxs-lookup"><span data-stu-id="0ed94-219">Contact type</span></span> |
| <span data-ttu-id="0ed94-220">AssignedTo_s</span><span class="sxs-lookup"><span data-stu-id="0ed94-220">AssignedTo_s</span></span> | <span data-ttu-id="0ed94-221">Přiřazené příliš</span><span class="sxs-lookup"><span data-stu-id="0ed94-221">Assigned too</span></span> |
| <span data-ttu-id="0ed94-222">Category_s</span><span class="sxs-lookup"><span data-stu-id="0ed94-222">Category_s</span></span> | <span data-ttu-id="0ed94-223">Kategorie</span><span class="sxs-lookup"><span data-stu-id="0ed94-223">Category</span></span> |
| <span data-ttu-id="0ed94-224">Title_s</span><span class="sxs-lookup"><span data-stu-id="0ed94-224">Title_s</span></span>|  <span data-ttu-id="0ed94-225">Krátký popis</span><span class="sxs-lookup"><span data-stu-id="0ed94-225">Short description</span></span> |
| <span data-ttu-id="0ed94-226">Description_s</span><span class="sxs-lookup"><span data-stu-id="0ed94-226">Description_s</span></span>|  <span data-ttu-id="0ed94-227">Poznámky</span><span class="sxs-lookup"><span data-stu-id="0ed94-227">Notes</span></span> |
| <span data-ttu-id="0ed94-228">CreatedDate_t</span><span class="sxs-lookup"><span data-stu-id="0ed94-228">CreatedDate_t</span></span>|  <span data-ttu-id="0ed94-229">Otevřít</span><span class="sxs-lookup"><span data-stu-id="0ed94-229">Opened</span></span> |
| <span data-ttu-id="0ed94-230">ClosedDate_t</span><span class="sxs-lookup"><span data-stu-id="0ed94-230">ClosedDate_t</span></span>| <span data-ttu-id="0ed94-231">uzavřený</span><span class="sxs-lookup"><span data-stu-id="0ed94-231">closed</span></span>|
| <span data-ttu-id="0ed94-232">ResolvedDate_t</span><span class="sxs-lookup"><span data-stu-id="0ed94-232">ResolvedDate_t</span></span>|<span data-ttu-id="0ed94-233">Vyřešil</span><span class="sxs-lookup"><span data-stu-id="0ed94-233">Resolved</span></span>|
| <span data-ttu-id="0ed94-234">Počítač</span><span class="sxs-lookup"><span data-stu-id="0ed94-234">Computer</span></span>  | <span data-ttu-id="0ed94-235">položky konfigurace</span><span class="sxs-lookup"><span data-stu-id="0ed94-235">Configuration item</span></span> |

## <a name="output-data-for-a-servicenow-change-request"></a><span data-ttu-id="0ed94-236">Výstupní data pro ServiceNow žádost o změnu</span><span class="sxs-lookup"><span data-stu-id="0ed94-236">Output data for a ServiceNow change request</span></span>

| <span data-ttu-id="0ed94-237">Pole OMS</span><span class="sxs-lookup"><span data-stu-id="0ed94-237">OMS field</span></span> | <span data-ttu-id="0ed94-238">ITSM pole</span><span class="sxs-lookup"><span data-stu-id="0ed94-238">ITSM field</span></span> |
|:--- |:--- |
| <span data-ttu-id="0ed94-239">ServiceDeskId_s</span><span class="sxs-lookup"><span data-stu-id="0ed94-239">ServiceDeskId_s</span></span>| <span data-ttu-id="0ed94-240">Číslo</span><span class="sxs-lookup"><span data-stu-id="0ed94-240">Number</span></span> |
| <span data-ttu-id="0ed94-241">CreatedBy_s</span><span class="sxs-lookup"><span data-stu-id="0ed94-241">CreatedBy_s</span></span> | <span data-ttu-id="0ed94-242">Požadoval</span><span class="sxs-lookup"><span data-stu-id="0ed94-242">Requested by</span></span> |
| <span data-ttu-id="0ed94-243">ClosedBy_s</span><span class="sxs-lookup"><span data-stu-id="0ed94-243">ClosedBy_s</span></span> | <span data-ttu-id="0ed94-244">Uzavřené</span><span class="sxs-lookup"><span data-stu-id="0ed94-244">Closed by</span></span> |
| <span data-ttu-id="0ed94-245">AssignedTo_s</span><span class="sxs-lookup"><span data-stu-id="0ed94-245">AssignedTo_s</span></span> | <span data-ttu-id="0ed94-246">Přiřazené příliš</span><span class="sxs-lookup"><span data-stu-id="0ed94-246">Assigned too</span></span> |
| <span data-ttu-id="0ed94-247">Title_s</span><span class="sxs-lookup"><span data-stu-id="0ed94-247">Title_s</span></span>|  <span data-ttu-id="0ed94-248">Krátký popis</span><span class="sxs-lookup"><span data-stu-id="0ed94-248">Short description</span></span> |
| <span data-ttu-id="0ed94-249">Type_s</span><span class="sxs-lookup"><span data-stu-id="0ed94-249">Type_s</span></span>|  <span data-ttu-id="0ed94-250">Typ</span><span class="sxs-lookup"><span data-stu-id="0ed94-250">Type</span></span> |
| <span data-ttu-id="0ed94-251">Category_s</span><span class="sxs-lookup"><span data-stu-id="0ed94-251">Category_s</span></span>|  <span data-ttu-id="0ed94-252">Catgory</span><span class="sxs-lookup"><span data-stu-id="0ed94-252">Catgory</span></span> |
| <span data-ttu-id="0ed94-253">CRState_s</span><span class="sxs-lookup"><span data-stu-id="0ed94-253">CRState_s</span></span>|  <span data-ttu-id="0ed94-254">Stav</span><span class="sxs-lookup"><span data-stu-id="0ed94-254">State</span></span>|
| <span data-ttu-id="0ed94-255">Urgency_s</span><span class="sxs-lookup"><span data-stu-id="0ed94-255">Urgency_s</span></span>|  <span data-ttu-id="0ed94-256">Naléhavosti</span><span class="sxs-lookup"><span data-stu-id="0ed94-256">Urgency</span></span> |
| <span data-ttu-id="0ed94-257">Priority_s</span><span class="sxs-lookup"><span data-stu-id="0ed94-257">Priority_s</span></span>| <span data-ttu-id="0ed94-258">Priorita</span><span class="sxs-lookup"><span data-stu-id="0ed94-258">Priority</span></span>|
| <span data-ttu-id="0ed94-259">Risk_s</span><span class="sxs-lookup"><span data-stu-id="0ed94-259">Risk_s</span></span>| <span data-ttu-id="0ed94-260">Riziko</span><span class="sxs-lookup"><span data-stu-id="0ed94-260">Risk</span></span>|
| <span data-ttu-id="0ed94-261">Impact_s</span><span class="sxs-lookup"><span data-stu-id="0ed94-261">Impact_s</span></span>| <span data-ttu-id="0ed94-262">Dopad</span><span class="sxs-lookup"><span data-stu-id="0ed94-262">Impact</span></span>|
| <span data-ttu-id="0ed94-263">RequestedDate_t</span><span class="sxs-lookup"><span data-stu-id="0ed94-263">RequestedDate_t</span></span>  | <span data-ttu-id="0ed94-264">Požadovaná data</span><span class="sxs-lookup"><span data-stu-id="0ed94-264">Requested by date</span></span> |
| <span data-ttu-id="0ed94-265">ClosedDate_t</span><span class="sxs-lookup"><span data-stu-id="0ed94-265">ClosedDate_t</span></span> | <span data-ttu-id="0ed94-266">Datum uzavření</span><span class="sxs-lookup"><span data-stu-id="0ed94-266">Closed date</span></span> |
| <span data-ttu-id="0ed94-267">PlannedStartDate_t</span><span class="sxs-lookup"><span data-stu-id="0ed94-267">PlannedStartDate_t</span></span>  |     <span data-ttu-id="0ed94-268">Plánované počáteční datum</span><span class="sxs-lookup"><span data-stu-id="0ed94-268">Planned start date</span></span> |
| <span data-ttu-id="0ed94-269">PlannedEndDate_t</span><span class="sxs-lookup"><span data-stu-id="0ed94-269">PlannedEndDate_t</span></span>  |   <span data-ttu-id="0ed94-270">Plánované koncové datum</span><span class="sxs-lookup"><span data-stu-id="0ed94-270">Planned end date</span></span> |
| <span data-ttu-id="0ed94-271">WorkStartDate_t</span><span class="sxs-lookup"><span data-stu-id="0ed94-271">WorkStartDate_t</span></span>  | <span data-ttu-id="0ed94-272">Skutečné počáteční datum</span><span class="sxs-lookup"><span data-stu-id="0ed94-272">Actual start date</span></span> |
| <span data-ttu-id="0ed94-273">WorkEndDate_t</span><span class="sxs-lookup"><span data-stu-id="0ed94-273">WorkEndDate_t</span></span> | <span data-ttu-id="0ed94-274">Skutečné koncové datum</span><span class="sxs-lookup"><span data-stu-id="0ed94-274">Actual end date</span></span>|
| <span data-ttu-id="0ed94-275">Description_s</span><span class="sxs-lookup"><span data-stu-id="0ed94-275">Description_s</span></span> | <span data-ttu-id="0ed94-276">Popis</span><span class="sxs-lookup"><span data-stu-id="0ed94-276">Description</span></span> |
| <span data-ttu-id="0ed94-277">Počítač</span><span class="sxs-lookup"><span data-stu-id="0ed94-277">Computer</span></span>  | <span data-ttu-id="0ed94-278">Položky konfigurace</span><span class="sxs-lookup"><span data-stu-id="0ed94-278">Configuration Item</span></span> |

<span data-ttu-id="0ed94-279">**Ukázka obrazovky analýzy protokolů pro ITSM data:**</span><span class="sxs-lookup"><span data-stu-id="0ed94-279">**Sample Log Analytics screen for ITSM data:**</span></span>

![Obrazovka analýzy protokolů](./media/log-analytics-itsmc/itsmc-overview-sample-log-analytics.png)

## <a name="it-service-management-connector--integration-with-other-oms-solutions"></a><span data-ttu-id="0ed94-281">Konektoru Service Management IT – integrace s jinými řešeními OMS</span><span class="sxs-lookup"><span data-stu-id="0ed94-281">IT Service Management connector – integration with other OMS solutions</span></span>

<span data-ttu-id="0ed94-282">IT Service Management Connector aktuálně podporuje integraci se službou hello řešení mapy služeb.</span><span class="sxs-lookup"><span data-stu-id="0ed94-282">IT Service Management Connector, currently supports integration with hello Service Map solution.</span></span>

<span data-ttu-id="0ed94-283">Mapa služeb automaticky vyhledá hello součásti aplikace v systému Windows a systémy Linux a mapy hello komunikace mezi službami.</span><span class="sxs-lookup"><span data-stu-id="0ed94-283">Service Map automatically discovers hello application components on Windows and Linux systems and maps hello communication between services.</span></span> <span data-ttu-id="0ed94-284">Umožňuje vám tooview vaše servery, jako jste je – představit jako vzájemně propojena systémy, které poskytování důležitých služeb.</span><span class="sxs-lookup"><span data-stu-id="0ed94-284">It allows you tooview your servers as you think of them – as interconnected systems that deliver critical services.</span></span> <span data-ttu-id="0ed94-285">Mapy služeb zobrazí připojení mezi servery, procesy, a vyžaduje porty mezi žádné připojení TCP architektura žádnou konfiguraci, jiné než instalace agenta.</span><span class="sxs-lookup"><span data-stu-id="0ed94-285">Service Map shows connections between servers, processes, and ports across any TCP-connected architecture with no configuration required other than installation of an agent.</span></span> <span data-ttu-id="0ed94-286">Další informace: [mapy služeb](../operations-management-suite/operations-management-suite-service-map.md).</span><span class="sxs-lookup"><span data-stu-id="0ed94-286">More information: [Service Map](../operations-management-suite/operations-management-suite-service-map.md).</span></span>

<span data-ttu-id="0ed94-287">Díky této integraci můžete zobrazit hello služby podpory položky vytvořené v hello ITSM řešení, jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="0ed94-287">With this integration, you can view hello service desk items created in hello ITSM solutions as shown in hello following example:</span></span>

![<span data-ttu-id="0ed94-288">Integrované řešení</span><span class="sxs-lookup"><span data-stu-id="0ed94-288">Integrated solution</span></span> ](./media/log-analytics-itsmc/itsmc-overview-integrated-solutions.png)
## <a name="create-itsm-work-items-for-oms-alerts"></a><span data-ttu-id="0ed94-289">Vytváření pracovních položek ITSM OMS výstrahy</span><span class="sxs-lookup"><span data-stu-id="0ed94-289">Create ITSM work items for OMS alerts</span></span>

<span data-ttu-id="0ed94-290">Pro hello OMS výstrahy můžete vytvořit přidružených pracovních položek v hello připojené ITSM zdroje.</span><span class="sxs-lookup"><span data-stu-id="0ed94-290">For hello OMS alerts, you can create associated work items in hello connected ITSM sources.</span></span>  <span data-ttu-id="0ed94-291">toodo hello tento, použijte následující postup:</span><span class="sxs-lookup"><span data-stu-id="0ed94-291">toodo this, use hello following procedure:</span></span>

1. <span data-ttu-id="0ed94-292">Z **hledání protokolů** okně tooview dat protokolu vyhledávací dotaz.</span><span class="sxs-lookup"><span data-stu-id="0ed94-292">From **Log Search** window, run a log search query tooview data.</span></span> <span data-ttu-id="0ed94-293">Výsledky dotazu jsou hello zdroj pro pracovní položky.</span><span class="sxs-lookup"><span data-stu-id="0ed94-293">Query results are hello source for work items.</span></span>
2. <span data-ttu-id="0ed94-294">V **hledání protokolů**, klikněte na tlačítko **výstrahy** tooopen hello **přidat pravidlo výstrahy** stránky.</span><span class="sxs-lookup"><span data-stu-id="0ed94-294">In **Log Search**, click **Alert** tooopen hello **Add Alert Rule** page.</span></span>

    ![Obrazovka analýzy protokolů](./media/log-analytics-itsmc/itsmc-work-items-for-oms-alerts.png)

3. <span data-ttu-id="0ed94-296">Na hello **přidat pravidlo výstrahy** okno, zadejte hello požadované podrobnosti **název**, **závažnost**, **vyhledávací dotaz**, a  **Výstrahy kritéria** (časová okna/metrika měří období).</span><span class="sxs-lookup"><span data-stu-id="0ed94-296">On hello **Add Alert Rule** window, provide hello required details for **Name**, **Severity**,  **Search query**, and **Alert criteria** (Time Window/Metric measurement).</span></span>
4. <span data-ttu-id="0ed94-297">Vyberte **Ano** pro **ITSM akce**.</span><span class="sxs-lookup"><span data-stu-id="0ed94-297">Select **Yes** for **ITSM Actions**.</span></span>
5. <span data-ttu-id="0ed94-298">Vyberte připojení ITSM z hello **vyberte připojení** seznamu.</span><span class="sxs-lookup"><span data-stu-id="0ed94-298">Select your ITSM connection from hello **Select Connection** list.</span></span>
6. <span data-ttu-id="0ed94-299">Zadejte podrobnosti hello podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="0ed94-299">Provide hello details as required.</span></span>
7. <span data-ttu-id="0ed94-300">toocreate samostatné pracovní položku pro každou položku protokolu této výstrahy, vyberte hello **vytvořit jednotlivé pracovní položky pro každý záznam protokolu** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="0ed94-300">toocreate a separate work item for each log entry of this alert, select hello **Create individual work items for each log entry** checkbox.</span></span>

    <span data-ttu-id="0ed94-301">Nebo</span><span class="sxs-lookup"><span data-stu-id="0ed94-301">Or</span></span>

    <span data-ttu-id="0ed94-302">nechte toto políčko nezaškrtnuté toocreate jenom jeden pracovní položku pro libovolný počet záznamů protokolu v rámci této výstrahy.</span><span class="sxs-lookup"><span data-stu-id="0ed94-302">leave this checkbox unselected toocreate only one work item for any number of log entries under this alert.</span></span>

7. <span data-ttu-id="0ed94-303">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0ed94-303">Click **Save**.</span></span>

<span data-ttu-id="0ed94-304">bude vytvořena výstraha OMS Hello ve **výstrahy**.</span><span class="sxs-lookup"><span data-stu-id="0ed94-304">hello OMS alert will be created under **Alerts**.</span></span> <span data-ttu-id="0ed94-305">Hello pracovním odpovídající ITSM připojení položky vytvářejí, když je splněna podmínka hello zadaný výstrahy.</span><span class="sxs-lookup"><span data-stu-id="0ed94-305">hello corresponding ITSM connection's work items are created when hello specified alert's condition is met.</span></span>

## <a name="create-itsm-work-items-from-oms-logs"></a><span data-ttu-id="0ed94-306">Vytváření pracovních položek ITSM z protokolů OMS</span><span class="sxs-lookup"><span data-stu-id="0ed94-306">Create ITSM work items from OMS logs</span></span>

<span data-ttu-id="0ed94-307">Pracovní položky můžete vytvořit v zdroje ITSM hello připojené pomocí vyhledávání protokolu OMS.</span><span class="sxs-lookup"><span data-stu-id="0ed94-307">You can create work items in hello connected ITSM sources by using OMS Log Search.</span></span> <span data-ttu-id="0ed94-308">toodo hello tento, použijte následující postup:</span><span class="sxs-lookup"><span data-stu-id="0ed94-308">toodo this, use hello following procedure:</span></span>

1. <span data-ttu-id="0ed94-309">Z **hledání protokolů**, vyhledávání hello požadované dat, vyberte hello podrobností a klikněte na **vytvořit pracovní položka**.</span><span class="sxs-lookup"><span data-stu-id="0ed94-309">From **Log Search**,  search hello required data, select hello detail, and click **Create work item**.</span></span>

    <span data-ttu-id="0ed94-310">Hello **vytvořit ITSM pracovní položka** zobrazí se okno:</span><span class="sxs-lookup"><span data-stu-id="0ed94-310">hello **Create ITSM Work item** window appears:</span></span>

    ![Obrazovka analýzy protokolů](media/log-analytics-itsmc/itsmc-work-items-from-oms-logs.png)

2.   <span data-ttu-id="0ed94-312">Přidejte hello následující podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="0ed94-312">Add hello following details:</span></span>

  - <span data-ttu-id="0ed94-313">**Název pracovní položky**: název hello pracovní položku.</span><span class="sxs-lookup"><span data-stu-id="0ed94-313">**Work item Title**: Title for hello work item.</span></span>
  - <span data-ttu-id="0ed94-314">**Pracovní položky Popis**: popis hello novou pracovní položku.</span><span class="sxs-lookup"><span data-stu-id="0ed94-314">**Work item Description**: Description for hello new work item.</span></span>
  - <span data-ttu-id="0ed94-315">**Vliv na počítače**: název hello počítače, kde tato data protokolu byla nalezena.</span><span class="sxs-lookup"><span data-stu-id="0ed94-315">**Affected Computer**: Name of hello computer where this log data was found.</span></span>
  - <span data-ttu-id="0ed94-316">**Vyberte připojení**: ITSM připojení, ve kterém má toocreate tuto pracovní položku.</span><span class="sxs-lookup"><span data-stu-id="0ed94-316">**Select Connection**:  ITSM connection in which you want toocreate this work item.</span></span>
  - <span data-ttu-id="0ed94-317">**Pracovní položka**: typ pracovní položky.</span><span class="sxs-lookup"><span data-stu-id="0ed94-317">**Work item**:  Type of work item.</span></span>

3. <span data-ttu-id="0ed94-318">toouse existující šablony pracovní položky pro incidentu, klikněte na tlačítko **Ano** pod **generování pracovní položka založený na šabloně hello** a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0ed94-318">toouse an existing work item template for an incident, click **Yes** under **Generate work item based on hello template** option and then click **Create**.</span></span>

    <span data-ttu-id="0ed94-319">Nebo:</span><span class="sxs-lookup"><span data-stu-id="0ed94-319">Or,</span></span>

    <span data-ttu-id="0ed94-320">Klikněte na tlačítko **ne** Pokud chcete tooprovide svoje vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0ed94-320">Click **No** if you want tooprovide your customized values.</span></span>

4. <span data-ttu-id="0ed94-321">Zadejte odpovídající hodnoty hello v hello **typu Kontakt**, **dopad**, **naléhavost**, **kategorie**, a **dílčí kategorie**  textová pole a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0ed94-321">Provide hello appropriate values in hello **Contact Type**, **Impact**, **Urgency**, **Category**, and **Sub Category** text boxes, and then click **Create**.</span></span>

<span data-ttu-id="0ed94-322">Hello pracovní položka bude vytvořen v hello ITSM, které lze také prohlížet v OMS.</span><span class="sxs-lookup"><span data-stu-id="0ed94-322">hello work item will be created in hello ITSM, which you can also view in OMS.</span></span>

## <a name="troubleshoot-itsm-connections-in-oms"></a><span data-ttu-id="0ed94-323">Řešení potíží s ITSM připojení v OMS</span><span class="sxs-lookup"><span data-stu-id="0ed94-323">Troubleshoot ITSM connections in OMS</span></span>
1.  <span data-ttu-id="0ed94-324">Pokud připojení selže z připojeného zdroje uživatelského rozhraní a získat hello **Chyba při ukládání připojení** zprávy, hello následující:</span><span class="sxs-lookup"><span data-stu-id="0ed94-324">If connection fails from connected source's UI and you get hello **Error in saving connection** message, do hello following:</span></span>
 - <span data-ttu-id="0ed94-325">V případě připojení ServiceNow, Cherwell a Provance si zajistěte hello správně zadané uživatelské jméno a heslo a klient ID nebo sdílený tajný klíč klienta pro každé připojení hello.</span><span class="sxs-lookup"><span data-stu-id="0ed94-325">In case of ServiceNow, Cherwell and Provance connections, ensure you correctly entered  hello username/password and  client ID/client secret  for each of hello connections.</span></span> <span data-ttu-id="0ed94-326">Pokud hello chyba přetrvává, zkontrolujte, pokud máte dostatečná oprávnění v hello odpovídající ITSM produktu toomake hello připojení.</span><span class="sxs-lookup"><span data-stu-id="0ed94-326">If hello error persists, check if you have sufficient privileges  in hello corresponding ITSM product toomake hello connection.</span></span>
 - <span data-ttu-id="0ed94-327">V případě portálu Service Manager, zajistěte, aby hello webové aplikace je úspěšně nasazen a hybridní připojení se vytvoří.</span><span class="sxs-lookup"><span data-stu-id="0ed94-327">In case of Service Manager, ensure that hello Web app is successfully deployed and hybrid connection is created.</span></span> <span data-ttu-id="0ed94-328">hello místní portálu Service Manager počítač se úspěšně naváže připojení hello tooverify, navštivte adresa URL webové aplikace hello podle popisu v dokumentaci hello provedení hello [hybridní připojení](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).</span><span class="sxs-lookup"><span data-stu-id="0ed94-328">tooverify hello connection is successfully established with hello on-prem Service Manager machine, visit hello  Web app URL as detailed in hello documentation for making hello [hybrid connection](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).</span></span>

2.  <span data-ttu-id="0ed94-329">Pokud není získávání synchronizovat data z ServiceNow v OMS, zajistěte, že instance ServiceNow hello není pozastaveno.</span><span class="sxs-lookup"><span data-stu-id="0ed94-329">If data from ServiceNow is not getting synced in OMS, ensure that hello ServiceNow instance is not sleeping.</span></span> <span data-ttu-id="0ed94-330">To může nějakou dobu zopakovat dojít v hello instance ServiceNow vývojářů, při nečinnosti.</span><span class="sxs-lookup"><span data-stu-id="0ed94-330">This might sometime happen in hello ServiceNow Dev instances, when idle.</span></span> <span data-ttu-id="0ed94-331">#Else problém hello sestavy.</span><span class="sxs-lookup"><span data-stu-id="0ed94-331">Else, report hello issue.</span></span>
3.  <span data-ttu-id="0ed94-332">Pokud při získávání vyvolání výstrahy od OMS, ale nejsou získávání pracovní položky vytvořené v produktu ITSM nebo položek konfigurace nejsou získávání toowork vytvoření propojené položky nebo pro všechny obecné informace, hello následující:</span><span class="sxs-lookup"><span data-stu-id="0ed94-332">If Alerts are getting fired from OMS but work items are not getting created in ITSM product or configuration items are not getting created/linked toowork items or for any generic information, do hello following:</span></span>
 -  <span data-ttu-id="0ed94-333">Řešení pro správu konektoru služby IT na portálu OMS může být použité tooget souhrn připojení nebo pracovní položky nebo počítače atd. Klikněte na tlačítko hello chybová zpráva v okně hello stav, přejděte příliš**hledání protokolů** a zobrazit hello připojení, které obsahuje chybu hello pomocí příkazu podrobnosti hello v hello chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="0ed94-333">IT Service Management Connector solution in OMS portal could be used tooget a summary of connections/work items/computers etc. Click hello error message in hello status blade, navigate too**Log Search** and view hello connection that has hello error by using hello details in hello error message.</span></span>
 - <span data-ttu-id="0ed94-334">Zobrazí informace týkající se/chyby hello se přímo v hello **hledání protokolů** stránky pomocí *typ = ServiceDeskLog_CL*.</span><span class="sxs-lookup"><span data-stu-id="0ed94-334">you can directly view hello errors/related information in hello **Log Search** page using *Type=ServiceDeskLog_CL*.</span></span>

## <a name="troubleshoot-service-manager-web-app-deployment"></a><span data-ttu-id="0ed94-335">Řešení potíží s nasazení aplikace webového portálu Service Manager</span><span class="sxs-lookup"><span data-stu-id="0ed94-335">Troubleshoot Service Manager Web App deployment</span></span>
1.  <span data-ttu-id="0ed94-336">V případě jakékoli potíže při nasazení webové aplikace, zajistěte, abyste měli dostatečná oprávnění v rámci předplatného hello uvedených toocreate nebo nasazení prostředků.</span><span class="sxs-lookup"><span data-stu-id="0ed94-336">In case of any trouble with web app deployment, ensure you have sufficient permissions in hello subscription mentioned toocreate/deploy resources.</span></span>
2.  <span data-ttu-id="0ed94-337">Pokud **odkaz na objekt není nastaven tooinstance objektu** chybová zpráva se zobrazí při spuštění hello [skriptu](log-analytics-itsmc-service-manager-script.md) zkontrolujte, zda jste zadali platné hodnoty v části **konfigurace uživatele**části.</span><span class="sxs-lookup"><span data-stu-id="0ed94-337">If **Object reference not set tooinstance of an object** error message appears while running hello [script](log-analytics-itsmc-service-manager-script.md) ensure that you entered valid values  under **User Configuration** section.</span></span>
3.  <span data-ttu-id="0ed94-338">Pokud tak předávání názvů toocreate sběrnice, ujistěte se, že hello vyžaduje poskytovatele prostředků je zaregistrován v rámci předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="0ed94-338">If you fail toocreate service bus relay namespace, ensure that hello required resource provider is registered in hello subscription.</span></span> <span data-ttu-id="0ed94-339">Pokud není zaregistrovaný, ručně vytvořte ho z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0ed94-339">If not registered, manually create it from hello Azure portal.</span></span> <span data-ttu-id="0ed94-340">Můžete také vytvořit ji při [vytváření hello hybridní připojení](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0ed94-340">You can also create it while [creating hello hybrid connection](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) from hello Azure portal.</span></span>


## <a name="contact-us"></a><span data-ttu-id="0ed94-341">Kontaktujte nás</span><span class="sxs-lookup"><span data-stu-id="0ed94-341">Contact us</span></span>

<span data-ttu-id="0ed94-342">Pro dotazy nebo zpětnou vazbu na hello konektoru správy IT služby, kontaktujte nás na adrese [ omsitsmfeedback@microsoft.com ](mailto:omsitsmfeedback@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="0ed94-342">For any queries or feedback on hello IT Service Management Connector, contact us at [omsitsmfeedback@microsoft.com](mailto:omsitsmfeedback@microsoft.com).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0ed94-343">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0ed94-343">Next steps</span></span>
<span data-ttu-id="0ed94-344">[Přidat tooIT ITSM produkty nebo služby Service Management Connector](log-analytics-itsmc-connections.md).</span><span class="sxs-lookup"><span data-stu-id="0ed94-344">[Add ITSM products/services tooIT Service Management Connector](log-analytics-itsmc-connections.md).</span></span>
