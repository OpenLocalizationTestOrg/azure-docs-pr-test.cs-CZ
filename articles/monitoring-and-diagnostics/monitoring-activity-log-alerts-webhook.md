---
title: "Pochopení schéma webhooku použít ve výstrahách aktivity protokolu | Microsoft Docs"
description: "Další informace o schématu formátu JSON, který je odeslána do URL webhooku se nenačetla, když se aktivuje výstrahu protokolu aktivit."
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
ms.openlocfilehash: 75c71bcd16573d4f4dd3377c623aa9b414aa3906
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="webhooks-for-azure-activity-log-alerts"></a><span data-ttu-id="f54e6-103">Webhooky Azure aktivity protokolu výstrahy</span><span class="sxs-lookup"><span data-stu-id="f54e6-103">Webhooks for Azure activity log alerts</span></span>
<span data-ttu-id="f54e6-104">Jako součást definice skupiny akce můžete nakonfigurovat webhooku koncových bodů pro příjem oznámení o výstrahách protokolu aktivit.</span><span class="sxs-lookup"><span data-stu-id="f54e6-104">As part of the definition of an action group, you can configure webhook endpoints to receive activity log alert notifications.</span></span> <span data-ttu-id="f54e6-105">Pomocí webhooků je možné směrovat tato oznámení s dalšími systémy pro následné zpracování nebo vlastní akce.</span><span class="sxs-lookup"><span data-stu-id="f54e6-105">With webhooks, you can route these notifications to other systems for post-processing or custom actions.</span></span> <span data-ttu-id="f54e6-106">Tento článek ukazuje, jak vypadá pro HTTP POST na webhook, jehož datové části.</span><span class="sxs-lookup"><span data-stu-id="f54e6-106">This article shows what the payload for the HTTP POST to a webhook looks like.</span></span>

<span data-ttu-id="f54e6-107">Další informace o protokolu upozornění, najdete v tématu Jak [vytvořit Azure aktivitu protokolu výstrahy](monitoring-activity-log-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="f54e6-107">For more information on activity log alerts, see how to [create Azure activity log alerts](monitoring-activity-log-alerts.md).</span></span>

<span data-ttu-id="f54e6-108">Informace o skupinách akce najdete v tématu Jak [vytvořit skupiny akce](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="f54e6-108">For information on action groups, see how to [create action groups](monitoring-action-groups.md).</span></span>

## <a name="authenticate-the-webhook"></a><span data-ttu-id="f54e6-109">Ověření webhooku</span><span class="sxs-lookup"><span data-stu-id="f54e6-109">Authenticate the webhook</span></span>
<span data-ttu-id="f54e6-110">Webhooku můžete volitelně použít ověření na základě tokenu pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="f54e6-110">The webhook can optionally use token-based authorization for authentication.</span></span> <span data-ttu-id="f54e6-111">Identifikátor URI je uložit s tokenu ID, například webhooku `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`.</span><span class="sxs-lookup"><span data-stu-id="f54e6-111">The webhook URI is saved with a token ID, for example, `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`.</span></span>

## <a name="payload-schema"></a><span data-ttu-id="f54e6-112">Datová část schématu</span><span class="sxs-lookup"><span data-stu-id="f54e6-112">Payload schema</span></span>
<span data-ttu-id="f54e6-113">Datová část JSON, které jsou obsažené v operaci POST liší v závislosti na pole data.context.activityLog.eventSource datové části.</span><span class="sxs-lookup"><span data-stu-id="f54e6-113">The JSON payload contained in the POST operation differs based on the payload's data.context.activityLog.eventSource field.</span></span>

###<a name="common"></a><span data-ttu-id="f54e6-114">Běžné</span><span class="sxs-lookup"><span data-stu-id="f54e6-114">Common</span></span>
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
###<a name="administrative"></a><span data-ttu-id="f54e6-115">Pro správu</span><span class="sxs-lookup"><span data-stu-id="f54e6-115">Administrative</span></span>
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
###<a name="servicehealth"></a><span data-ttu-id="f54e6-116">ServiceHealth</span><span class="sxs-lookup"><span data-stu-id="f54e6-116">ServiceHealth</span></span>
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

<span data-ttu-id="f54e6-117">Podrobnosti konkrétní schématu služby stavu oznámení aktivity protokolu výstrah najdete v tématu [oznámení o stavu služby](monitoring-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="f54e6-117">For specific schema details on service health notification activity log alerts, see [Service health notifications](monitoring-service-notifications.md).</span></span>

<span data-ttu-id="f54e6-118">Podrobnosti konkrétní schématu na všechny ostatní výstrahy protokolu aktivit najdete v tématu [přehled protokolu Azure činnosti](monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="f54e6-118">For specific schema details on all other activity log alerts, see [Overview of the Azure activity log](monitoring-overview-activity-logs.md).</span></span>

| <span data-ttu-id="f54e6-119">Název elementu</span><span class="sxs-lookup"><span data-stu-id="f54e6-119">Element name</span></span> | <span data-ttu-id="f54e6-120">Popis</span><span class="sxs-lookup"><span data-stu-id="f54e6-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="f54e6-121">status</span><span class="sxs-lookup"><span data-stu-id="f54e6-121">status</span></span> |<span data-ttu-id="f54e6-122">Používá pro výstrahy, metriky.</span><span class="sxs-lookup"><span data-stu-id="f54e6-122">Used for metric alerts.</span></span> <span data-ttu-id="f54e6-123">Vždy nastaven na "aktivovaný" pro aktivitu protokolu výstrahy.</span><span class="sxs-lookup"><span data-stu-id="f54e6-123">Always set to "activated" for activity log alerts.</span></span> |
| <span data-ttu-id="f54e6-124">Kontext</span><span class="sxs-lookup"><span data-stu-id="f54e6-124">context</span></span> |<span data-ttu-id="f54e6-125">Kontext události.</span><span class="sxs-lookup"><span data-stu-id="f54e6-125">Context of the event.</span></span> |
| <span data-ttu-id="f54e6-126">resourceProviderName</span><span class="sxs-lookup"><span data-stu-id="f54e6-126">resourceProviderName</span></span> |<span data-ttu-id="f54e6-127">Zprostředkovatel prostředků ovlivněné prostředku.</span><span class="sxs-lookup"><span data-stu-id="f54e6-127">The resource provider of the impacted resource.</span></span> |
| <span data-ttu-id="f54e6-128">conditionType</span><span class="sxs-lookup"><span data-stu-id="f54e6-128">conditionType</span></span> |<span data-ttu-id="f54e6-129">Vždy "událost".</span><span class="sxs-lookup"><span data-stu-id="f54e6-129">Always "Event."</span></span> |
| <span data-ttu-id="f54e6-130">jméno</span><span class="sxs-lookup"><span data-stu-id="f54e6-130">name</span></span> |<span data-ttu-id="f54e6-131">Název pravidla výstrahy.</span><span class="sxs-lookup"><span data-stu-id="f54e6-131">Name of the alert rule.</span></span> |
| <span data-ttu-id="f54e6-132">id</span><span class="sxs-lookup"><span data-stu-id="f54e6-132">id</span></span> |<span data-ttu-id="f54e6-133">ID prostředku výstrahy.</span><span class="sxs-lookup"><span data-stu-id="f54e6-133">Resource ID of the alert.</span></span> |
| <span data-ttu-id="f54e6-134">Popis</span><span class="sxs-lookup"><span data-stu-id="f54e6-134">description</span></span> |<span data-ttu-id="f54e6-135">Popis výstrahy nastavit při vytváření výstrahu.</span><span class="sxs-lookup"><span data-stu-id="f54e6-135">Alert description set when the alert is created.</span></span> |
| <span data-ttu-id="f54e6-136">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="f54e6-136">subscriptionId</span></span> |<span data-ttu-id="f54e6-137">ID předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="f54e6-137">Azure subscription ID.</span></span> |
| <span data-ttu-id="f54e6-138">časové razítko</span><span class="sxs-lookup"><span data-stu-id="f54e6-138">timestamp</span></span> |<span data-ttu-id="f54e6-139">Čas, kdy byla generována událost pomocí služby Azure, který požadavek zpracoval.</span><span class="sxs-lookup"><span data-stu-id="f54e6-139">Time at which the event was generated by the Azure service that processed the request.</span></span> |
| <span data-ttu-id="f54e6-140">resourceId</span><span class="sxs-lookup"><span data-stu-id="f54e6-140">resourceId</span></span> |<span data-ttu-id="f54e6-141">ID prostředku ovlivněné prostředku.</span><span class="sxs-lookup"><span data-stu-id="f54e6-141">Resource ID of the impacted resource.</span></span> |
| <span data-ttu-id="f54e6-142">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="f54e6-142">resourceGroupName</span></span> |<span data-ttu-id="f54e6-143">Název skupiny prostředků pro prostředek dopad.</span><span class="sxs-lookup"><span data-stu-id="f54e6-143">Name of the resource group for the impacted resource.</span></span> |
| <span data-ttu-id="f54e6-144">properties</span><span class="sxs-lookup"><span data-stu-id="f54e6-144">properties</span></span> |<span data-ttu-id="f54e6-145">Sada `<Key, Value>` páry (tedy `Dictionary<String, String>`) obsahující podrobnosti o události.</span><span class="sxs-lookup"><span data-stu-id="f54e6-145">Set of `<Key, Value>` pairs (that is, `Dictionary<String, String>`) that includes details about the event.</span></span> |
| <span data-ttu-id="f54e6-146">Události</span><span class="sxs-lookup"><span data-stu-id="f54e6-146">event</span></span> |<span data-ttu-id="f54e6-147">Element, který obsahuje metadata o události.</span><span class="sxs-lookup"><span data-stu-id="f54e6-147">Element that contains metadata about the event.</span></span> |
| <span data-ttu-id="f54e6-148">Autorizace</span><span class="sxs-lookup"><span data-stu-id="f54e6-148">authorization</span></span> |<span data-ttu-id="f54e6-149">Řízení přístupu na základě Role vlastnosti události.</span><span class="sxs-lookup"><span data-stu-id="f54e6-149">The Role-Based Access Control properties of the event.</span></span> <span data-ttu-id="f54e6-150">Tyto vlastnosti obvykle obsahovat akci, role a obor.</span><span class="sxs-lookup"><span data-stu-id="f54e6-150">These properties usually include the action, the role, and the scope.</span></span> |
| <span data-ttu-id="f54e6-151">category</span><span class="sxs-lookup"><span data-stu-id="f54e6-151">category</span></span> |<span data-ttu-id="f54e6-152">Kategorie události.</span><span class="sxs-lookup"><span data-stu-id="f54e6-152">Category of the event.</span></span> <span data-ttu-id="f54e6-153">Podporované hodnoty jsou správy, výstrahy, zabezpečení, ServiceHealth a doporučení.</span><span class="sxs-lookup"><span data-stu-id="f54e6-153">Supported values include Administrative, Alert, Security, ServiceHealth, and Recommendation.</span></span> |
| <span data-ttu-id="f54e6-154">volající</span><span class="sxs-lookup"><span data-stu-id="f54e6-154">caller</span></span> |<span data-ttu-id="f54e6-155">E-mailovou adresu uživatele, který provádí operace, deklarace hlavní název uživatele nebo název SPN deklarace identity na základě dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="f54e6-155">Email address of the user who performed the operation, UPN claim, or SPN claim based on availability.</span></span> <span data-ttu-id="f54e6-156">Může mít hodnotu null pro určité systémová volání.</span><span class="sxs-lookup"><span data-stu-id="f54e6-156">Can be null for certain system calls.</span></span> |
| <span data-ttu-id="f54e6-157">correlationId</span><span class="sxs-lookup"><span data-stu-id="f54e6-157">correlationId</span></span> |<span data-ttu-id="f54e6-158">Obvykle GUID ve formátu řetězce.</span><span class="sxs-lookup"><span data-stu-id="f54e6-158">Usually a GUID in string format.</span></span> <span data-ttu-id="f54e6-159">Události se correlationId patřit do stejné větší akce a obvykle sdílet correlationId.</span><span class="sxs-lookup"><span data-stu-id="f54e6-159">Events with correlationId belong to the same larger action and usually share a correlationId.</span></span> |
| <span data-ttu-id="f54e6-160">eventDescription</span><span class="sxs-lookup"><span data-stu-id="f54e6-160">eventDescription</span></span> |<span data-ttu-id="f54e6-161">Statický text popis události.</span><span class="sxs-lookup"><span data-stu-id="f54e6-161">Static text description of the event.</span></span> |
| <span data-ttu-id="f54e6-162">eventDataId</span><span class="sxs-lookup"><span data-stu-id="f54e6-162">eventDataId</span></span> |<span data-ttu-id="f54e6-163">Jedinečný identifikátor pro událost.</span><span class="sxs-lookup"><span data-stu-id="f54e6-163">Unique identifier for the event.</span></span> |
| <span data-ttu-id="f54e6-164">EventSource</span><span class="sxs-lookup"><span data-stu-id="f54e6-164">eventSource</span></span> |<span data-ttu-id="f54e6-165">Název služby Azure nebo infrastruktury, které vygenerovalo událost.</span><span class="sxs-lookup"><span data-stu-id="f54e6-165">Name of the Azure service or infrastructure that generated the event.</span></span> |
| <span data-ttu-id="f54e6-166">požadavku HTTP</span><span class="sxs-lookup"><span data-stu-id="f54e6-166">httpRequest</span></span> |<span data-ttu-id="f54e6-167">Požadavek obvykle obsahuje clientRequestId, clientIpAddress a metodou HTTP (například přidat).</span><span class="sxs-lookup"><span data-stu-id="f54e6-167">The request usually includes the clientRequestId, clientIpAddress, and HTTP method (for example, PUT).</span></span> |
| <span data-ttu-id="f54e6-168">úroveň</span><span class="sxs-lookup"><span data-stu-id="f54e6-168">level</span></span> |<span data-ttu-id="f54e6-169">Jeden z následujících hodnot: kritická, chyba, upozornění, informační a Verbose.</span><span class="sxs-lookup"><span data-stu-id="f54e6-169">One of the following values: Critical, Error, Warning, Informational, and Verbose.</span></span> |
| <span data-ttu-id="f54e6-170">operationId</span><span class="sxs-lookup"><span data-stu-id="f54e6-170">operationId</span></span> |<span data-ttu-id="f54e6-171">Obvykle GUID sdílen události odpovídající jedné operace.</span><span class="sxs-lookup"><span data-stu-id="f54e6-171">Usually a GUID shared among the events corresponding to single operation.</span></span> |
| <span data-ttu-id="f54e6-172">operationName</span><span class="sxs-lookup"><span data-stu-id="f54e6-172">operationName</span></span> |<span data-ttu-id="f54e6-173">Název operace.</span><span class="sxs-lookup"><span data-stu-id="f54e6-173">Name of the operation.</span></span> |
| <span data-ttu-id="f54e6-174">properties</span><span class="sxs-lookup"><span data-stu-id="f54e6-174">properties</span></span> |<span data-ttu-id="f54e6-175">Vlastnosti události.</span><span class="sxs-lookup"><span data-stu-id="f54e6-175">Properties of the event.</span></span> |
| <span data-ttu-id="f54e6-176">status</span><span class="sxs-lookup"><span data-stu-id="f54e6-176">status</span></span> |<span data-ttu-id="f54e6-177">Řetězec.</span><span class="sxs-lookup"><span data-stu-id="f54e6-177">String.</span></span> <span data-ttu-id="f54e6-178">Stav operace.</span><span class="sxs-lookup"><span data-stu-id="f54e6-178">Status of the operation.</span></span> <span data-ttu-id="f54e6-179">Běžné hodnoty zahrnují Začínáme, probíhá, bylo úspěšné, neúspěšné, aktivní a vyřešeno.</span><span class="sxs-lookup"><span data-stu-id="f54e6-179">Common values include Started, In Progress, Succeeded, Failed, Active, and Resolved.</span></span> |
| <span data-ttu-id="f54e6-180">Podřízený stav</span><span class="sxs-lookup"><span data-stu-id="f54e6-180">subStatus</span></span> |<span data-ttu-id="f54e6-181">Obvykle zahrnuje stavový kód HTTP odpovídající volání REST.</span><span class="sxs-lookup"><span data-stu-id="f54e6-181">Usually includes the HTTP status code of the corresponding REST call.</span></span> <span data-ttu-id="f54e6-182">Může také obsahovat další řetězce, které popisují podřízeného stavu.</span><span class="sxs-lookup"><span data-stu-id="f54e6-182">It might also include other strings that describe a substatus.</span></span> <span data-ttu-id="f54e6-183">Běžné substatus hodnoty zahrnují OK (stavový kód HTTP: 200), které byly vytvořeny (stavový kód HTTP: 201), platné (stavový kód HTTP: 202), ne obsahu (stavový kód HTTP: 204), chybný požadavek (stavový kód HTTP: 400), nebyl nalezen (stavový kód HTTP: 404), konflikt (stavový kód HTTP: 409 ), Vnitřní chybu serveru (kód stavu HTTP: 500), služba není k dispozici (kód stavu HTTP: 503) a vypršel časový limit brány (kód stavu HTTP: 504).</span><span class="sxs-lookup"><span data-stu-id="f54e6-183">Common substatus values include OK (HTTP Status Code: 200), Created (HTTP Status Code: 201), Accepted (HTTP Status Code: 202), No Content (HTTP Status Code: 204), Bad Request (HTTP Status Code: 400), Not Found (HTTP Status Code: 404), Conflict (HTTP Status Code: 409), Internal Server Error (HTTP Status Code: 500), Service Unavailable (HTTP Status Code: 503), and Gateway Timeout (HTTP Status Code: 504).</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f54e6-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f54e6-184">Next steps</span></span>
* <span data-ttu-id="f54e6-185">[Další informace o protokolu činnosti](monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="f54e6-185">[Learn more about the activity log](monitoring-overview-activity-logs.md).</span></span>
* <span data-ttu-id="f54e6-186">[Spustit skripty služby Azure automation (Runbooky) na Azure výstrahy](http://go.microsoft.com/fwlink/?LinkId=627081).</span><span class="sxs-lookup"><span data-stu-id="f54e6-186">[Execute Azure automation scripts (Runbooks) on Azure alerts](http://go.microsoft.com/fwlink/?LinkId=627081).</span></span>
* <span data-ttu-id="f54e6-187">[Pomocí aplikace logiky odeslat zprávu SMS prostřednictvím Twilio z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="f54e6-187">[Use a logic app to send an SMS via Twilio from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).</span></span> <span data-ttu-id="f54e6-188">V tomto příkladu je metriky výstrahy, ale je možné ji upravit pro práci s výstrahu protokolu aktivit.</span><span class="sxs-lookup"><span data-stu-id="f54e6-188">This example is for metric alerts, but it can be modified to work with an activity log alert.</span></span>
* <span data-ttu-id="f54e6-189">[Použití aplikace logiky odeslat zprávu Slack z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="f54e6-189">[Use a logic app to send a Slack message from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).</span></span> <span data-ttu-id="f54e6-190">V tomto příkladu je metriky výstrahy, ale je možné ji upravit pro práci s výstrahu protokolu aktivit.</span><span class="sxs-lookup"><span data-stu-id="f54e6-190">This example is for metric alerts, but it can be modified to work with an activity log alert.</span></span>
* <span data-ttu-id="f54e6-191">[Použití aplikace logiky k odeslání zprávy do fronty Azure z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="f54e6-191">[Use a logic app to send a message to an Azure queue from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).</span></span> <span data-ttu-id="f54e6-192">V tomto příkladu je metriky výstrahy, ale je možné ji upravit pro práci s výstrahu protokolu aktivit.</span><span class="sxs-lookup"><span data-stu-id="f54e6-192">This example is for metric alerts, but it can be modified to work with an activity log alert.</span></span>
