---
title: "Volat webhook, jehož protokol činnosti Azure výstrah | Microsoft Docs"
description: "Události protokolu aktivit trasy k jiným službám pro vlastní akce. Například odeslání serveru SMS, protokolu chyby nebo upozornění tým prostřednictvím chatu nebo zasílání zpráv služby service."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 64d333d1-7f37-4a00-9d16-dda6e69a113b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: johnkem
ms.openlocfilehash: 341ab32ad0ec691285fbf1537ee298ab30156a5d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="call-a-webhook-on-azure-activity-log-alerts"></a><span data-ttu-id="9f153-104">Volat webhook, jehož protokol činnosti Azure výstrah</span><span class="sxs-lookup"><span data-stu-id="9f153-104">Call a webhook on Azure Activity Log alerts</span></span>
<span data-ttu-id="9f153-105">Webhooky umožňují směrovat Azure oznámení výstrahy k jiným systémům pro následné zpracování nebo vlastní akce.</span><span class="sxs-lookup"><span data-stu-id="9f153-105">Webhooks allow you to route an Azure alert notification to other systems for post-processing or custom actions.</span></span> <span data-ttu-id="9f153-106">Webhook, jehož na výstrahu slouží k směrovat do služeb, které odeslání serveru SMS, protokolu chyb, upozornění tým prostřednictvím služby zasílání zpráv nebo chatu nebo provádět řadu dalších akcí.</span><span class="sxs-lookup"><span data-stu-id="9f153-106">You can use a webhook on an alert to route it to services that send SMS, log bugs, notify a team via chat/messaging services, or do any number of other actions.</span></span> <span data-ttu-id="9f153-107">Tento článek popisuje, jak nastavit webhooku se volá, když se aktivuje upozornění protokol činnosti Azure.</span><span class="sxs-lookup"><span data-stu-id="9f153-107">This article describes how to set a webhook to be called when an Azure Activity Log alert fires.</span></span> <span data-ttu-id="9f153-108">Také ukazuje, jak vypadá pro HTTP POST na webhook, jehož datové části.</span><span class="sxs-lookup"><span data-stu-id="9f153-108">It also shows what the payload for the HTTP POST to a webhook looks like.</span></span> <span data-ttu-id="9f153-109">Informace o instalaci a schéma pro výstrahu Azure metriky [místo toho najdete na této stránce](insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="9f153-109">For information on the setup and schema for an Azure metric alert, [see this page instead](insights-webhooks-alerts.md).</span></span> <span data-ttu-id="9f153-110">Můžete také nastavit tak výstrahu protokolu aktivit k odesílání e-mailu, pokud je aktivován.</span><span class="sxs-lookup"><span data-stu-id="9f153-110">You can also set up an Activity Log alert to send email when activated.</span></span>

> [!NOTE]
> <span data-ttu-id="9f153-111">Tato funkce je aktuálně ve verzi preview a odeberou se v určitém okamžiku v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="9f153-111">This feature is currently in preview and will be removed at some point in the future.</span></span>
>
>

<span data-ttu-id="9f153-112">Můžete nastavit k protokolu aktivit výstrah pomocí [rutin prostředí Azure PowerShell](insights-powershell-samples.md#create-metric-alerts), [rozhraní příkazového řádku a platformy](insights-cli-samples.md#work-with-alerts), nebo [REST API služby Azure monitorování](https://msdn.microsoft.com/library/azure/dn933805.aspx).</span><span class="sxs-lookup"><span data-stu-id="9f153-112">You can set up an Activity Log alert using the [Azure PowerShell Cmdlets](insights-powershell-samples.md#create-metric-alerts), [Cross-Platform CLI](insights-cli-samples.md#work-with-alerts), or [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx).</span></span> <span data-ttu-id="9f153-113">V současné době nelze nastavit ho vytvořit pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9f153-113">Currently, you cannot set one up using the Azure portal.</span></span>

## <a name="authenticating-the-webhook"></a><span data-ttu-id="9f153-114">Ověřování webhooku</span><span class="sxs-lookup"><span data-stu-id="9f153-114">Authenticating the webhook</span></span>
<span data-ttu-id="9f153-115">Webhooku můžete ověřit pomocí některé z těchto metod:</span><span class="sxs-lookup"><span data-stu-id="9f153-115">The webhook can authenticate using either of these methods:</span></span>

1. <span data-ttu-id="9f153-116">**Na základě tokenu autorizace** -webhooku identifikátor URI je uložit s tokenu ID, například`https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`</span><span class="sxs-lookup"><span data-stu-id="9f153-116">**Token-based authorization** - The webhook URI is saved with a token ID, for example, `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`</span></span>
2. <span data-ttu-id="9f153-117">**Základní ověřování** -webhooku identifikátor URI je uložit pomocí uživatelského jména a hesla, například`https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`</span><span class="sxs-lookup"><span data-stu-id="9f153-117">**Basic authorization** - The webhook URI is saved with a username and password, for example, `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`</span></span>

## <a name="payload-schema"></a><span data-ttu-id="9f153-118">Datová část schématu</span><span class="sxs-lookup"><span data-stu-id="9f153-118">Payload schema</span></span>
<span data-ttu-id="9f153-119">Operaci POST obsahuje následující datové části JSON a schéma pro všechny výstrahy na základě protokolu aktivit.</span><span class="sxs-lookup"><span data-stu-id="9f153-119">The POST operation contains the following JSON payload and schema for all Activity Log-based alerts.</span></span> <span data-ttu-id="9f153-120">Toto schéma je podobné používá metrika na základě výstrahy.</span><span class="sxs-lookup"><span data-stu-id="9f153-120">This schema is similar to the one used by metric-based alerts.</span></span>

```
{
        "status": "Activated",
        "context": {
                "resourceProviderName": "Microsoft.Web",
                "event": {
                        "$type": "Microsoft.WindowsAzure.Management.Monitoring.Automation.Notifications.GenericNotifications.Datacontracts.InstanceEventContext, Microsoft.WindowsAzure.Management.Mon.Automation",
                        "authorization": {
                                "action": "Microsoft.Web/sites/start/action",
                                "scope": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest"
                        },
                        "eventDataId": "327caaca-08d7-41b1-86d8-27d0a7adb92d",
                        "category": "Administrative",
                        "caller": "myname@mycompany.com",
                        "httpRequest": {
                                "clientRequestId": "f58cead8-c9ed-43af-8710-55e64def208d",
                                "clientIpAddress": "104.43.166.155",
                                "method": "POST"
                        },
                        "status": "Succeeded",
                        "subStatus": "OK",
                        "level": "Informational",
                        "correlationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "eventDescription": "",
                        "operationName": "Microsoft.Web/sites/start/action",
                        "operationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "properties": {
                                "$type": "Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage",
                                "statusCode": "OK",
                                "serviceRequestId": "f7716681-496a-4f5c-8d14-d564bcf54714"
                        }
                },
                "timestamp": "Friday, March 11, 2016 9:13:23 PM",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/alertonevent2",
                "name": "alertonevent2",
                "description": "test alert on event start",
                "conditionType": "Event",
                "subscriptionId": "s1",
                "resourceId": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest",
                "resourceGroupName": "rg1"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```

| <span data-ttu-id="9f153-121">Název elementu</span><span class="sxs-lookup"><span data-stu-id="9f153-121">Element Name</span></span> | <span data-ttu-id="9f153-122">Popis</span><span class="sxs-lookup"><span data-stu-id="9f153-122">Description</span></span> |
| --- | --- |
| <span data-ttu-id="9f153-123">status</span><span class="sxs-lookup"><span data-stu-id="9f153-123">status</span></span> |<span data-ttu-id="9f153-124">Používá pro výstrahy, metriky.</span><span class="sxs-lookup"><span data-stu-id="9f153-124">Used for metric alerts.</span></span> <span data-ttu-id="9f153-125">Vždy nastaven na "aktivovat" pro protokol aktivit výstrahy.</span><span class="sxs-lookup"><span data-stu-id="9f153-125">Always set to "activated" for Activity Log alerts.</span></span> |
| <span data-ttu-id="9f153-126">Kontext</span><span class="sxs-lookup"><span data-stu-id="9f153-126">context</span></span> |<span data-ttu-id="9f153-127">Kontext události.</span><span class="sxs-lookup"><span data-stu-id="9f153-127">Context of the event.</span></span> |
| <span data-ttu-id="9f153-128">resourceProviderName</span><span class="sxs-lookup"><span data-stu-id="9f153-128">resourceProviderName</span></span> |<span data-ttu-id="9f153-129">Zprostředkovatel prostředků ovlivněné prostředku.</span><span class="sxs-lookup"><span data-stu-id="9f153-129">The resource provider of the impacted resource.</span></span> |
| <span data-ttu-id="9f153-130">conditionType</span><span class="sxs-lookup"><span data-stu-id="9f153-130">conditionType</span></span> |<span data-ttu-id="9f153-131">Vždy "událost".</span><span class="sxs-lookup"><span data-stu-id="9f153-131">Always "Event."</span></span> |
| <span data-ttu-id="9f153-132">jméno</span><span class="sxs-lookup"><span data-stu-id="9f153-132">name</span></span> |<span data-ttu-id="9f153-133">Název pravidla výstrahy.</span><span class="sxs-lookup"><span data-stu-id="9f153-133">Name of the alert rule.</span></span> |
| <span data-ttu-id="9f153-134">id</span><span class="sxs-lookup"><span data-stu-id="9f153-134">id</span></span> |<span data-ttu-id="9f153-135">ID prostředku výstrahy.</span><span class="sxs-lookup"><span data-stu-id="9f153-135">Resource ID of the alert.</span></span> |
| <span data-ttu-id="9f153-136">Popis</span><span class="sxs-lookup"><span data-stu-id="9f153-136">description</span></span> |<span data-ttu-id="9f153-137">Popis výstrahy jako sada během vytváření výstrahy</span><span class="sxs-lookup"><span data-stu-id="9f153-137">Alert description as set during creation of the alert.</span></span> |
| <span data-ttu-id="9f153-138">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="9f153-138">subscriptionId</span></span> |<span data-ttu-id="9f153-139">ID předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="9f153-139">Azure Subscription ID.</span></span> |
| <span data-ttu-id="9f153-140">časové razítko</span><span class="sxs-lookup"><span data-stu-id="9f153-140">timestamp</span></span> |<span data-ttu-id="9f153-141">Čas, kdy byla generována událost pomocí služby Azure, který požadavek zpracoval.</span><span class="sxs-lookup"><span data-stu-id="9f153-141">Time at which the event was generated by the Azure service that processed the request.</span></span> |
| <span data-ttu-id="9f153-142">resourceId</span><span class="sxs-lookup"><span data-stu-id="9f153-142">resourceId</span></span> |<span data-ttu-id="9f153-143">ID prostředku ovlivněné prostředku.</span><span class="sxs-lookup"><span data-stu-id="9f153-143">Resource ID of the impacted resource.</span></span> |
| <span data-ttu-id="9f153-144">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="9f153-144">resourceGroupName</span></span> |<span data-ttu-id="9f153-145">Název skupiny prostředků ovlivněné prostředku</span><span class="sxs-lookup"><span data-stu-id="9f153-145">Name of the resource group for the impacted resource</span></span> |
| <span data-ttu-id="9f153-146">properties</span><span class="sxs-lookup"><span data-stu-id="9f153-146">properties</span></span> |<span data-ttu-id="9f153-147">Sada `<Key, Value>` páry (tj. `Dictionary<String, String>`) obsahující podrobnosti o události.</span><span class="sxs-lookup"><span data-stu-id="9f153-147">Set of `<Key, Value>` pairs (i.e. `Dictionary<String, String>`) that includes details about the event.</span></span> |
| <span data-ttu-id="9f153-148">Události</span><span class="sxs-lookup"><span data-stu-id="9f153-148">event</span></span> |<span data-ttu-id="9f153-149">Element obsahující metadata o události.</span><span class="sxs-lookup"><span data-stu-id="9f153-149">Element containing metadata about the event.</span></span> |
| <span data-ttu-id="9f153-150">Autorizace</span><span class="sxs-lookup"><span data-stu-id="9f153-150">authorization</span></span> |<span data-ttu-id="9f153-151">Vlastnosti RBAC události.</span><span class="sxs-lookup"><span data-stu-id="9f153-151">The RBAC properties of the event.</span></span> <span data-ttu-id="9f153-152">Obvykle patří "action", "role" a "obor".</span><span class="sxs-lookup"><span data-stu-id="9f153-152">These usually include the “action”, “role” and the “scope.”</span></span> |
| <span data-ttu-id="9f153-153">category</span><span class="sxs-lookup"><span data-stu-id="9f153-153">category</span></span> |<span data-ttu-id="9f153-154">Kategorie události.</span><span class="sxs-lookup"><span data-stu-id="9f153-154">Category of the event.</span></span> <span data-ttu-id="9f153-155">Podporované hodnoty jsou: pro správu, výstrahy, zabezpečení, ServiceHealth, doporučení.</span><span class="sxs-lookup"><span data-stu-id="9f153-155">Supported values include: Administrative, Alert, Security, ServiceHealth, Recommendation.</span></span> |
| <span data-ttu-id="9f153-156">volající</span><span class="sxs-lookup"><span data-stu-id="9f153-156">caller</span></span> |<span data-ttu-id="9f153-157">E-mailovou adresu uživatele, který provádí operace, deklarace hlavní název uživatele nebo název SPN deklarace identity na základě dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="9f153-157">Email address of the user who performed the operation, UPN claim, or SPN claim based on availability.</span></span> <span data-ttu-id="9f153-158">Může mít hodnotu null pro určité systémová volání.</span><span class="sxs-lookup"><span data-stu-id="9f153-158">Can be null for certain system calls.</span></span> |
| <span data-ttu-id="9f153-159">correlationId</span><span class="sxs-lookup"><span data-stu-id="9f153-159">correlationId</span></span> |<span data-ttu-id="9f153-160">Obvykle GUID ve formátu řetězce.</span><span class="sxs-lookup"><span data-stu-id="9f153-160">Usually a GUID in string format.</span></span> <span data-ttu-id="9f153-161">Události se correlationId patřit do stejné větší akce a obvykle sdílet correlationId.</span><span class="sxs-lookup"><span data-stu-id="9f153-161">Events with correlationId belong to the same larger action and usually share a correlationId.</span></span> |
| <span data-ttu-id="9f153-162">eventDescription</span><span class="sxs-lookup"><span data-stu-id="9f153-162">eventDescription</span></span> |<span data-ttu-id="9f153-163">Statický text popis události.</span><span class="sxs-lookup"><span data-stu-id="9f153-163">Static text description of the event.</span></span> |
| <span data-ttu-id="9f153-164">eventDataId</span><span class="sxs-lookup"><span data-stu-id="9f153-164">eventDataId</span></span> |<span data-ttu-id="9f153-165">Jedinečný identifikátor pro událost.</span><span class="sxs-lookup"><span data-stu-id="9f153-165">Unique identifier for the event.</span></span> |
| <span data-ttu-id="9f153-166">EventSource</span><span class="sxs-lookup"><span data-stu-id="9f153-166">eventSource</span></span> |<span data-ttu-id="9f153-167">Název služby Azure nebo infrastruktury, které vygenerovalo událost.</span><span class="sxs-lookup"><span data-stu-id="9f153-167">Name of the Azure service or infrastructure that generated the event.</span></span> |
| <span data-ttu-id="9f153-168">požadavku HTTP</span><span class="sxs-lookup"><span data-stu-id="9f153-168">httpRequest</span></span> |<span data-ttu-id="9f153-169">Obvykle zahrnuje "clientRequestId", "clientIpAddress" a "metodu" (například PUT metoda HTTP).</span><span class="sxs-lookup"><span data-stu-id="9f153-169">Usually includes the “clientRequestId”, “clientIpAddress” and “method” (HTTP method e.g. PUT).</span></span> |
| <span data-ttu-id="9f153-170">úroveň</span><span class="sxs-lookup"><span data-stu-id="9f153-170">level</span></span> |<span data-ttu-id="9f153-171">Jeden z následujících hodnot: "Kritická", "Chyba", "Upozornění", "Informační" a "Verbose."</span><span class="sxs-lookup"><span data-stu-id="9f153-171">One of the following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose.”</span></span> |
| <span data-ttu-id="9f153-172">operationId</span><span class="sxs-lookup"><span data-stu-id="9f153-172">operationId</span></span> |<span data-ttu-id="9f153-173">Obvykle GUID sdílen události odpovídající jedné operace.</span><span class="sxs-lookup"><span data-stu-id="9f153-173">Usually a GUID shared among the events corresponding to single operation.</span></span> |
| <span data-ttu-id="9f153-174">operationName</span><span class="sxs-lookup"><span data-stu-id="9f153-174">operationName</span></span> |<span data-ttu-id="9f153-175">Název operace.</span><span class="sxs-lookup"><span data-stu-id="9f153-175">Name of the operation.</span></span> |
| <span data-ttu-id="9f153-176">properties</span><span class="sxs-lookup"><span data-stu-id="9f153-176">properties</span></span> |<span data-ttu-id="9f153-177">Vlastnosti události.</span><span class="sxs-lookup"><span data-stu-id="9f153-177">Properties of the event.</span></span> |
| <span data-ttu-id="9f153-178">status</span><span class="sxs-lookup"><span data-stu-id="9f153-178">status</span></span> |<span data-ttu-id="9f153-179">Řetězec.</span><span class="sxs-lookup"><span data-stu-id="9f153-179">String.</span></span> <span data-ttu-id="9f153-180">Stav operace.</span><span class="sxs-lookup"><span data-stu-id="9f153-180">Status of the operation.</span></span> <span data-ttu-id="9f153-181">Běžné hodnoty jsou: "Spustit", "Probíhá", "Bylo úspěšně dokončeno", "Se nezdařilo", "Aktivní", "Přeložit".</span><span class="sxs-lookup"><span data-stu-id="9f153-181">Common values include: "Started", "In Progress", "Succeeded", "Failed", "Active", "Resolved".</span></span> |
| <span data-ttu-id="9f153-182">Podřízený stav</span><span class="sxs-lookup"><span data-stu-id="9f153-182">subStatus</span></span> |<span data-ttu-id="9f153-183">Obvykle zahrnuje stavový kód HTTP odpovídající volání REST.</span><span class="sxs-lookup"><span data-stu-id="9f153-183">Usually includes the HTTP status code of the corresponding REST call.</span></span> <span data-ttu-id="9f153-184">Může také obsahovat jiných řetězců popisující podřízeného stavu.</span><span class="sxs-lookup"><span data-stu-id="9f153-184">It might also include other strings describing a substatus.</span></span> <span data-ttu-id="9f153-185">Běžné substatus hodnoty patří: OK (stavový kód HTTP: 200), které byly vytvořeny (stavový kód HTTP: 201), platné (stavový kód HTTP: 202), ne obsahu (stavový kód HTTP: 204), chybný požadavek (stavový kód HTTP: 400), nebyl nalezen (stavový kód HTTP: 404), konflikt (stavový kód HTTP: 409), vnitřní chybu serveru (kód stavu HTTP: 500), služba není k dispozici (kód stavu HTTP: 503), vypršel časový limit brány (kód stavu HTTP: 504)</span><span class="sxs-lookup"><span data-stu-id="9f153-185">Common substatus values include: OK (HTTP Status Code: 200), Created (HTTP Status Code: 201), Accepted (HTTP Status Code: 202), No Content (HTTP Status Code: 204), Bad Request (HTTP Status Code: 400), Not Found (HTTP Status Code: 404), Conflict (HTTP Status Code: 409), Internal Server Error (HTTP Status Code: 500), Service Unavailable (HTTP Status Code: 503), Gateway Timeout (HTTP Status Code: 504)</span></span> |

## <a name="next-steps"></a><span data-ttu-id="9f153-186">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9f153-186">Next steps</span></span>
* [<span data-ttu-id="9f153-187">Další informace o protokolu aktivit</span><span class="sxs-lookup"><span data-stu-id="9f153-187">Learn more about the Activity Log</span></span>](monitoring-overview-activity-logs.md)
* [<span data-ttu-id="9f153-188">Spustit skripty Azure Automation (Runbooky) na Azure výstrahy</span><span class="sxs-lookup"><span data-stu-id="9f153-188">Execute Azure Automation scripts (Runbooks) on Azure alerts</span></span>](http://go.microsoft.com/fwlink/?LinkId=627081)
* <span data-ttu-id="9f153-189">[Pomocí aplikace logiky můžete odeslat zprávu SMS prostřednictvím Twilio z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="9f153-189">[Use Logic App to send an SMS via Twilio from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).</span></span> <span data-ttu-id="9f153-190">V tomto příkladu je metriky výstrahy, ale je možné upravovat pro práci s výstrahu protokolu aktivit.</span><span class="sxs-lookup"><span data-stu-id="9f153-190">This example is for metric alerts, but could be modified to work with an Activity Log alert.</span></span>
* <span data-ttu-id="9f153-191">[Použití aplikace logiky odeslat zprávu Slack z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="9f153-191">[Use Logic App to send a Slack message from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).</span></span> <span data-ttu-id="9f153-192">V tomto příkladu je metriky výstrahy, ale je možné upravovat pro práci s výstrahu protokolu aktivit.</span><span class="sxs-lookup"><span data-stu-id="9f153-192">This example is for metric alerts, but could be modified to work with an Activity Log alert.</span></span>
* <span data-ttu-id="9f153-193">[Použití aplikace logiky odeslat zprávu do fronty Azure z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="9f153-193">[Use Logic App to send a message to an Azure Queue from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).</span></span> <span data-ttu-id="9f153-194">V tomto příkladu je metriky výstrahy, ale je možné upravovat pro práci s výstrahu protokolu aktivit.</span><span class="sxs-lookup"><span data-stu-id="9f153-194">This example is for metric alerts, but could be modified to work with an Activity Log alert.</span></span>
