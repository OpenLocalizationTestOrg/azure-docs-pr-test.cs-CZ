---
title: "aaaUnderstand hello webhooku schématu použít ve výstrahách aktivity protokolu | Microsoft Docs"
description: "Další informace o schématu hello hello formátu JSON, který je odeslána adresa URL webhooku tooa, pokud aktivuje výstrahu protokolu aktivit."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: johnkem
ms.openlocfilehash: 75562e0589222d3e392ea73eacfd7414a422d115
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="webhooks-for-azure-activity-log-alerts"></a><span data-ttu-id="a20d7-103">Webhooky Azure aktivity protokolu výstrahy</span><span class="sxs-lookup"><span data-stu-id="a20d7-103">Webhooks for Azure activity log alerts</span></span>
<span data-ttu-id="a20d7-104">Jako součást definice hello akce skupiny můžete nakonfigurovat webhooku koncové body tooreceive aktivity protokolu oznámení o výstrahách.</span><span class="sxs-lookup"><span data-stu-id="a20d7-104">As part of hello definition of an action group, you can configure webhook endpoints tooreceive activity log alert notifications.</span></span> <span data-ttu-id="a20d7-105">Pomocí webhooků je možné směrovat tyto systémy tooother oznámení pro následné zpracování nebo vlastní akce.</span><span class="sxs-lookup"><span data-stu-id="a20d7-105">With webhooks, you can route these notifications tooother systems for post-processing or custom actions.</span></span> <span data-ttu-id="a20d7-106">Tento článek ukazuje, jaké datové části hello hello HTTP POST tooa webhooku vypadá jako.</span><span class="sxs-lookup"><span data-stu-id="a20d7-106">This article shows what hello payload for hello HTTP POST tooa webhook looks like.</span></span>

<span data-ttu-id="a20d7-107">Další informace o protokolu upozornění, najdete v části Jak příliš[vytvořit Azure aktivitu protokolu výstrahy](monitoring-activity-log-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="a20d7-107">For more information on activity log alerts, see how too[create Azure activity log alerts](monitoring-activity-log-alerts.md).</span></span>

<span data-ttu-id="a20d7-108">Informace o skupinách akce, najdete v části Jak příliš[vytvořit skupiny akce](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="a20d7-108">For information on action groups, see how too[create action groups](monitoring-action-groups.md).</span></span>

## <a name="authenticate-hello-webhook"></a><span data-ttu-id="a20d7-109">Ověření webhooku hello</span><span class="sxs-lookup"><span data-stu-id="a20d7-109">Authenticate hello webhook</span></span>
<span data-ttu-id="a20d7-110">Hello webhooku můžete volitelně použít ověření na základě tokenu pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="a20d7-110">hello webhook can optionally use token-based authorization for authentication.</span></span> <span data-ttu-id="a20d7-111">Hello webhooku identifikátor URI je uložit s tokenu ID, například `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`.</span><span class="sxs-lookup"><span data-stu-id="a20d7-111">hello webhook URI is saved with a token ID, for example, `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`.</span></span>

## <a name="payload-schema"></a><span data-ttu-id="a20d7-112">Datová část schématu</span><span class="sxs-lookup"><span data-stu-id="a20d7-112">Payload schema</span></span>
<span data-ttu-id="a20d7-113">datová část JSON Hello obsažené v hello operaci POST liší v závislosti na hello datové data.context.activityLog.eventSource pole.</span><span class="sxs-lookup"><span data-stu-id="a20d7-113">hello JSON payload contained in hello POST operation differs based on hello payload's data.context.activityLog.eventSource field.</span></span>

###<a name="common"></a><span data-ttu-id="a20d7-114">Běžné</span><span class="sxs-lookup"><span data-stu-id="a20d7-114">Common</span></span>
```json
{
    "schemaId": "Microsoft.Insights/activityLogs",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "channels": "Operation",
                "correlationId": "6ac88262-43be-4adf-a11c-bd2179852898",
                "eventSource": "Administrative",
                "eventTimestamp": "2017-03-29T15:43:08.0019532+00:00",
                "eventDataId": "8195a56a-85de-4663-943e-1a2bf401ad94",
                "level": "Informational",
                "operationName": "Microsoft.Insights/actionGroups/write",
                "operationId": "6ac88262-43be-4adf-a11c-bd2179852898",
                "status": "Started",
                "subStatus": "",
                "subscriptionId": "52c65f65-0518-4d37-9719-7dbbfc68c57a",
                "submissionTimestamp": "2017-03-29T15:43:20.3863637+00:00",
                ...
            }
        },
        "properties": {}
    }
}
```
###<a name="administrative"></a><span data-ttu-id="a20d7-115">Pro správu</span><span class="sxs-lookup"><span data-stu-id="a20d7-115">Administrative</span></span>
```json
{
    "schemaId": "Microsoft.Insights/activityLogs",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "authorization": {
                    "action": "Microsoft.Insights/actionGroups/write",
                    "scope": "/subscriptions/52c65f65-0518-4d37-9719-7dbbfc68c57b/resourceGroups/CONTOSO-TEST/providers/Microsoft.Insights/actionGroups/IncidentActions"
                },
                "claims": "{...}",
                "caller": "me@contoso.com",
                "description": "",
                "httpRequest": "{...}",
                "resourceId": "/subscriptions/52c65f65-0518-4d37-9719-7dbbfc68c57b/resourceGroups/CONTOSO-TEST/providers/Microsoft.Insights/actionGroups/IncidentActions",
                "resourceGroupName": "CONTOSO-TEST",
                "resourceProviderName": "Microsoft.Insights",
                "resourceType": "Microsoft.Insights/actionGroups"
            }
        },
        "properties": {}
    }
}

```
###<a name="servicehealth"></a><span data-ttu-id="a20d7-116">ServiceHealth</span><span class="sxs-lookup"><span data-stu-id="a20d7-116">ServiceHealth</span></span>
```json
{
    "schemaId": "unknown",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "properties": {
                    "title": "...",
                    "service": "...",
                    "region": "...",
                    "communication": "...",
                    "incidentType": "Incident",
                    "trackingId": "...",
                    "groupId": "...",
                    "impactStartTime": "3/29/2017 3:43:21 PM",
                    "impactMitigationTime": "3/29/2017 3:43:21 PM",
                    "eventCreationTime": "3/29/2017 3:43:21 PM",
                    "impactedServices": "[{...}]",
                    "defaultLanguageTitle": "...",
                    "defaultLanguageContent": "...",
                    "stage": "Active",
                    "communicationId": "...",
                    "version": "0.1"
                }
            }
        },
        "properties": {}
    }
}
```

<span data-ttu-id="a20d7-117">Podrobnosti konkrétní schématu služby stavu oznámení aktivity protokolu výstrah najdete v tématu [oznámení o stavu služby](monitoring-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="a20d7-117">For specific schema details on service health notification activity log alerts, see [Service health notifications](monitoring-service-notifications.md).</span></span>

<span data-ttu-id="a20d7-118">Podrobnosti konkrétní schématu na všechny ostatní výstrahy protokolu aktivit najdete v tématu [přehled protokol činnosti Azure hello](monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="a20d7-118">For specific schema details on all other activity log alerts, see [Overview of hello Azure activity log](monitoring-overview-activity-logs.md).</span></span>

| <span data-ttu-id="a20d7-119">Název elementu</span><span class="sxs-lookup"><span data-stu-id="a20d7-119">Element name</span></span> | <span data-ttu-id="a20d7-120">Popis</span><span class="sxs-lookup"><span data-stu-id="a20d7-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="a20d7-121">status</span><span class="sxs-lookup"><span data-stu-id="a20d7-121">status</span></span> |<span data-ttu-id="a20d7-122">Používá pro výstrahy, metriky.</span><span class="sxs-lookup"><span data-stu-id="a20d7-122">Used for metric alerts.</span></span> <span data-ttu-id="a20d7-123">Vždy nastaven příliš "aktivovaný" pro aktivitu protokolu výstrahy.</span><span class="sxs-lookup"><span data-stu-id="a20d7-123">Always set too"activated" for activity log alerts.</span></span> |
| <span data-ttu-id="a20d7-124">Kontext</span><span class="sxs-lookup"><span data-stu-id="a20d7-124">context</span></span> |<span data-ttu-id="a20d7-125">Kontext události hello.</span><span class="sxs-lookup"><span data-stu-id="a20d7-125">Context of hello event.</span></span> |
| <span data-ttu-id="a20d7-126">resourceProviderName</span><span class="sxs-lookup"><span data-stu-id="a20d7-126">resourceProviderName</span></span> |<span data-ttu-id="a20d7-127">Poskytovatel prostředků Hello hello dopad prostředků.</span><span class="sxs-lookup"><span data-stu-id="a20d7-127">hello resource provider of hello impacted resource.</span></span> |
| <span data-ttu-id="a20d7-128">conditionType</span><span class="sxs-lookup"><span data-stu-id="a20d7-128">conditionType</span></span> |<span data-ttu-id="a20d7-129">Vždy "událost".</span><span class="sxs-lookup"><span data-stu-id="a20d7-129">Always "Event."</span></span> |
| <span data-ttu-id="a20d7-130">jméno</span><span class="sxs-lookup"><span data-stu-id="a20d7-130">name</span></span> |<span data-ttu-id="a20d7-131">Název pravidla výstrahy hello.</span><span class="sxs-lookup"><span data-stu-id="a20d7-131">Name of hello alert rule.</span></span> |
| <span data-ttu-id="a20d7-132">id</span><span class="sxs-lookup"><span data-stu-id="a20d7-132">id</span></span> |<span data-ttu-id="a20d7-133">ID prostředku hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="a20d7-133">Resource ID of hello alert.</span></span> |
| <span data-ttu-id="a20d7-134">description</span><span class="sxs-lookup"><span data-stu-id="a20d7-134">description</span></span> |<span data-ttu-id="a20d7-135">Popis výstrahy nastavené, když se vytvoří výstraha hello.</span><span class="sxs-lookup"><span data-stu-id="a20d7-135">Alert description set when hello alert is created.</span></span> |
| <span data-ttu-id="a20d7-136">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="a20d7-136">subscriptionId</span></span> |<span data-ttu-id="a20d7-137">ID předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="a20d7-137">Azure subscription ID.</span></span> |
| <span data-ttu-id="a20d7-138">časové razítko</span><span class="sxs-lookup"><span data-stu-id="a20d7-138">timestamp</span></span> |<span data-ttu-id="a20d7-139">Čas, na které hello vygenerovalo událost hello služby Azure, který zpracovává požadavek hello.</span><span class="sxs-lookup"><span data-stu-id="a20d7-139">Time at which hello event was generated by hello Azure service that processed hello request.</span></span> |
| <span data-ttu-id="a20d7-140">resourceId</span><span class="sxs-lookup"><span data-stu-id="a20d7-140">resourceId</span></span> |<span data-ttu-id="a20d7-141">ID prostředku hello dopad prostředků.</span><span class="sxs-lookup"><span data-stu-id="a20d7-141">Resource ID of hello impacted resource.</span></span> |
| <span data-ttu-id="a20d7-142">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="a20d7-142">resourceGroupName</span></span> |<span data-ttu-id="a20d7-143">Název skupiny prostředků hello hello dopad prostředků.</span><span class="sxs-lookup"><span data-stu-id="a20d7-143">Name of hello resource group for hello impacted resource.</span></span> |
| <span data-ttu-id="a20d7-144">properties</span><span class="sxs-lookup"><span data-stu-id="a20d7-144">properties</span></span> |<span data-ttu-id="a20d7-145">Sada `<Key, Value>` páry (tedy `Dictionary<String, String>`) obsahující podrobnosti o události hello.</span><span class="sxs-lookup"><span data-stu-id="a20d7-145">Set of `<Key, Value>` pairs (that is, `Dictionary<String, String>`) that includes details about hello event.</span></span> |
| <span data-ttu-id="a20d7-146">Události</span><span class="sxs-lookup"><span data-stu-id="a20d7-146">event</span></span> |<span data-ttu-id="a20d7-147">Element, který obsahuje metadata o hello událostí.</span><span class="sxs-lookup"><span data-stu-id="a20d7-147">Element that contains metadata about hello event.</span></span> |
| <span data-ttu-id="a20d7-148">Autorizace</span><span class="sxs-lookup"><span data-stu-id="a20d7-148">authorization</span></span> |<span data-ttu-id="a20d7-149">Řízení přístupu na základě Role vlastnosti Hello hello události.</span><span class="sxs-lookup"><span data-stu-id="a20d7-149">hello Role-Based Access Control properties of hello event.</span></span> <span data-ttu-id="a20d7-150">Tyto vlastnosti obvykle zahrnují hello akce, hello role a obor hello.</span><span class="sxs-lookup"><span data-stu-id="a20d7-150">These properties usually include hello action, hello role, and hello scope.</span></span> |
| <span data-ttu-id="a20d7-151">category</span><span class="sxs-lookup"><span data-stu-id="a20d7-151">category</span></span> |<span data-ttu-id="a20d7-152">Kategorie události hello.</span><span class="sxs-lookup"><span data-stu-id="a20d7-152">Category of hello event.</span></span> <span data-ttu-id="a20d7-153">Podporované hodnoty jsou správy, výstrahy, zabezpečení, ServiceHealth a doporučení.</span><span class="sxs-lookup"><span data-stu-id="a20d7-153">Supported values include Administrative, Alert, Security, ServiceHealth, and Recommendation.</span></span> |
| <span data-ttu-id="a20d7-154">volající</span><span class="sxs-lookup"><span data-stu-id="a20d7-154">caller</span></span> |<span data-ttu-id="a20d7-155">E-mailovou adresu hello uživatele, který provedl hello operace, deklarace hlavní název uživatele nebo název SPN deklarace identity na základě dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="a20d7-155">Email address of hello user who performed hello operation, UPN claim, or SPN claim based on availability.</span></span> <span data-ttu-id="a20d7-156">Může mít hodnotu null pro určité systémová volání.</span><span class="sxs-lookup"><span data-stu-id="a20d7-156">Can be null for certain system calls.</span></span> |
| <span data-ttu-id="a20d7-157">correlationId</span><span class="sxs-lookup"><span data-stu-id="a20d7-157">correlationId</span></span> |<span data-ttu-id="a20d7-158">Obvykle GUID ve formátu řetězce.</span><span class="sxs-lookup"><span data-stu-id="a20d7-158">Usually a GUID in string format.</span></span> <span data-ttu-id="a20d7-159">Události se correlationId patří toohello stejné větší akce a obvykle sdílet correlationId.</span><span class="sxs-lookup"><span data-stu-id="a20d7-159">Events with correlationId belong toohello same larger action and usually share a correlationId.</span></span> |
| <span data-ttu-id="a20d7-160">eventDescription</span><span class="sxs-lookup"><span data-stu-id="a20d7-160">eventDescription</span></span> |<span data-ttu-id="a20d7-161">Statický text popisu události hello.</span><span class="sxs-lookup"><span data-stu-id="a20d7-161">Static text description of hello event.</span></span> |
| <span data-ttu-id="a20d7-162">eventDataId</span><span class="sxs-lookup"><span data-stu-id="a20d7-162">eventDataId</span></span> |<span data-ttu-id="a20d7-163">Jedinečný identifikátor pro událost hello.</span><span class="sxs-lookup"><span data-stu-id="a20d7-163">Unique identifier for hello event.</span></span> |
| <span data-ttu-id="a20d7-164">EventSource</span><span class="sxs-lookup"><span data-stu-id="a20d7-164">eventSource</span></span> |<span data-ttu-id="a20d7-165">Název hello služby Azure nebo infrastrukturu, že generovaný hello událost.</span><span class="sxs-lookup"><span data-stu-id="a20d7-165">Name of hello Azure service or infrastructure that generated hello event.</span></span> |
| <span data-ttu-id="a20d7-166">požadavku HTTP</span><span class="sxs-lookup"><span data-stu-id="a20d7-166">httpRequest</span></span> |<span data-ttu-id="a20d7-167">Hello žádosti obvykle zahrnuje hello clientRequestId, clientIpAddress a metoda HTTP (například přidat).</span><span class="sxs-lookup"><span data-stu-id="a20d7-167">hello request usually includes hello clientRequestId, clientIpAddress, and HTTP method (for example, PUT).</span></span> |
| <span data-ttu-id="a20d7-168">úroveň</span><span class="sxs-lookup"><span data-stu-id="a20d7-168">level</span></span> |<span data-ttu-id="a20d7-169">Jeden z následujících hodnot hello: kritická, chyba, upozornění, informační a Verbose.</span><span class="sxs-lookup"><span data-stu-id="a20d7-169">One of hello following values: Critical, Error, Warning, Informational, and Verbose.</span></span> |
| <span data-ttu-id="a20d7-170">operationId</span><span class="sxs-lookup"><span data-stu-id="a20d7-170">operationId</span></span> |<span data-ttu-id="a20d7-171">Obvykle GUID sdílen hello události odpovídající toosingle operaci.</span><span class="sxs-lookup"><span data-stu-id="a20d7-171">Usually a GUID shared among hello events corresponding toosingle operation.</span></span> |
| <span data-ttu-id="a20d7-172">operationName</span><span class="sxs-lookup"><span data-stu-id="a20d7-172">operationName</span></span> |<span data-ttu-id="a20d7-173">Název operace hello.</span><span class="sxs-lookup"><span data-stu-id="a20d7-173">Name of hello operation.</span></span> |
| <span data-ttu-id="a20d7-174">properties</span><span class="sxs-lookup"><span data-stu-id="a20d7-174">properties</span></span> |<span data-ttu-id="a20d7-175">Vlastnosti události hello.</span><span class="sxs-lookup"><span data-stu-id="a20d7-175">Properties of hello event.</span></span> |
| <span data-ttu-id="a20d7-176">status</span><span class="sxs-lookup"><span data-stu-id="a20d7-176">status</span></span> |<span data-ttu-id="a20d7-177">Řetězec.</span><span class="sxs-lookup"><span data-stu-id="a20d7-177">String.</span></span> <span data-ttu-id="a20d7-178">Stav operace hello.</span><span class="sxs-lookup"><span data-stu-id="a20d7-178">Status of hello operation.</span></span> <span data-ttu-id="a20d7-179">Běžné hodnoty zahrnují Začínáme, probíhá, bylo úspěšné, neúspěšné, aktivní a vyřešeno.</span><span class="sxs-lookup"><span data-stu-id="a20d7-179">Common values include Started, In Progress, Succeeded, Failed, Active, and Resolved.</span></span> |
| <span data-ttu-id="a20d7-180">Podřízený stav</span><span class="sxs-lookup"><span data-stu-id="a20d7-180">subStatus</span></span> |<span data-ttu-id="a20d7-181">Obvykle zahrnuje stavový kód HTTP hello hello odpovídající volání REST.</span><span class="sxs-lookup"><span data-stu-id="a20d7-181">Usually includes hello HTTP status code of hello corresponding REST call.</span></span> <span data-ttu-id="a20d7-182">Může také obsahovat další řetězce, které popisují podřízeného stavu.</span><span class="sxs-lookup"><span data-stu-id="a20d7-182">It might also include other strings that describe a substatus.</span></span> <span data-ttu-id="a20d7-183">Běžné substatus hodnoty zahrnují OK (stavový kód HTTP: 200), které byly vytvořeny (stavový kód HTTP: 201), platné (stavový kód HTTP: 202), ne obsahu (stavový kód HTTP: 204), chybný požadavek (stavový kód HTTP: 400), nebyl nalezen (stavový kód HTTP: 404), konflikt (stavový kód HTTP: 409 ), Vnitřní chybu serveru (kód stavu HTTP: 500), služba není k dispozici (kód stavu HTTP: 503) a vypršel časový limit brány (kód stavu HTTP: 504).</span><span class="sxs-lookup"><span data-stu-id="a20d7-183">Common substatus values include OK (HTTP Status Code: 200), Created (HTTP Status Code: 201), Accepted (HTTP Status Code: 202), No Content (HTTP Status Code: 204), Bad Request (HTTP Status Code: 400), Not Found (HTTP Status Code: 404), Conflict (HTTP Status Code: 409), Internal Server Error (HTTP Status Code: 500), Service Unavailable (HTTP Status Code: 503), and Gateway Timeout (HTTP Status Code: 504).</span></span> |

## <a name="next-steps"></a><span data-ttu-id="a20d7-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a20d7-184">Next steps</span></span>
* <span data-ttu-id="a20d7-185">[Další informace o protokolu aktivit hello](monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="a20d7-185">[Learn more about hello activity log](monitoring-overview-activity-logs.md).</span></span>
* <span data-ttu-id="a20d7-186">[Spustit skripty služby Azure automation (Runbooky) na Azure výstrahy](http://go.microsoft.com/fwlink/?LinkId=627081).</span><span class="sxs-lookup"><span data-stu-id="a20d7-186">[Execute Azure automation scripts (Runbooks) on Azure alerts](http://go.microsoft.com/fwlink/?LinkId=627081).</span></span>
* <span data-ttu-id="a20d7-187">[Použít toosend aplikace logiky serveru služby SMS prostřednictvím Twilio z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="a20d7-187">[Use a logic app toosend an SMS via Twilio from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).</span></span> <span data-ttu-id="a20d7-188">V tomto příkladu je metriky výstrahy, ale může být upravený toowork s výstrahu protokolu aktivit.</span><span class="sxs-lookup"><span data-stu-id="a20d7-188">This example is for metric alerts, but it can be modified toowork with an activity log alert.</span></span>
* <span data-ttu-id="a20d7-189">[Použití logiku aplikace toosend Slack zprávu z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="a20d7-189">[Use a logic app toosend a Slack message from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).</span></span> <span data-ttu-id="a20d7-190">V tomto příkladu je metriky výstrahy, ale může být upravený toowork s výstrahu protokolu aktivit.</span><span class="sxs-lookup"><span data-stu-id="a20d7-190">This example is for metric alerts, but it can be modified toowork with an activity log alert.</span></span>
* <span data-ttu-id="a20d7-191">[Použít toosend aplikace logiky fronty zpráv tooan Azure z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="a20d7-191">[Use a logic app toosend a message tooan Azure queue from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).</span></span> <span data-ttu-id="a20d7-192">V tomto příkladu je metriky výstrahy, ale může být upravený toowork s výstrahu protokolu aktivit.</span><span class="sxs-lookup"><span data-stu-id="a20d7-192">This example is for metric alerts, but it can be modified toowork with an activity log alert.</span></span>
