---
title: "aaaCall webhooku protokol činnosti Azure výstrah | Microsoft Docs"
description: "Směrujte aktivity protokolu události tooother služby pro vlastní akce. Například odeslání serveru SMS, protokolu chyby nebo upozornění tým prostřednictvím chatu nebo zasílání zpráv služby service."
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
ms.openlocfilehash: 9017ff3e5165857ec7084a8f07f4123552e55f73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="call-a-webhook-on-azure-activity-log-alerts"></a><span data-ttu-id="a5c55-104">Volat webhook, jehož protokol činnosti Azure výstrah</span><span class="sxs-lookup"><span data-stu-id="a5c55-104">Call a webhook on Azure Activity Log alerts</span></span>
<span data-ttu-id="a5c55-105">Webhooky umožňují tooroute Azure výstrahy systémy tooother oznámení pro následné zpracování nebo vlastní akce.</span><span class="sxs-lookup"><span data-stu-id="a5c55-105">Webhooks allow you tooroute an Azure alert notification tooother systems for post-processing or custom actions.</span></span> <span data-ttu-id="a5c55-106">Webhook, jehož můžete použít na výstrahy tooroute ho tooservices, které odesílají SMS, protokolu chyb, upozornění tým prostřednictvím služby zasílání zpráv nebo chatu nebo provádět řadu dalších akcí.</span><span class="sxs-lookup"><span data-stu-id="a5c55-106">You can use a webhook on an alert tooroute it tooservices that send SMS, log bugs, notify a team via chat/messaging services, or do any number of other actions.</span></span> <span data-ttu-id="a5c55-107">Tento článek popisuje, jak tooset webhooku toobe volána, když se aktivuje upozornění protokol činnosti Azure.</span><span class="sxs-lookup"><span data-stu-id="a5c55-107">This article describes how tooset a webhook toobe called when an Azure Activity Log alert fires.</span></span> <span data-ttu-id="a5c55-108">Také ukazuje, jaké datové části hello hello HTTP POST tooa webhooku vypadá jako.</span><span class="sxs-lookup"><span data-stu-id="a5c55-108">It also shows what hello payload for hello HTTP POST tooa webhook looks like.</span></span> <span data-ttu-id="a5c55-109">Informace o nastavení hello a schéma pro výstrahu Azure metriky [místo toho najdete na této stránce](insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="a5c55-109">For information on hello setup and schema for an Azure metric alert, [see this page instead](insights-webhooks-alerts.md).</span></span> <span data-ttu-id="a5c55-110">Můžete také nastavit protokol aktivit výstrahy toosend e-mailem při aktivaci.</span><span class="sxs-lookup"><span data-stu-id="a5c55-110">You can also set up an Activity Log alert toosend email when activated.</span></span>

> [!NOTE]
> <span data-ttu-id="a5c55-111">Tato funkce je aktuálně ve verzi preview a odeberou se v určitém okamžiku v budoucnu hello.</span><span class="sxs-lookup"><span data-stu-id="a5c55-111">This feature is currently in preview and will be removed at some point in hello future.</span></span>
>
>

<span data-ttu-id="a5c55-112">Můžete nastavit výstrahu protokolu aktivit pomocí hello [rutin prostředí Azure PowerShell](insights-powershell-samples.md#create-metric-alerts), [rozhraní příkazového řádku a platformy](insights-cli-samples.md#work-with-alerts), nebo [REST API služby Azure monitorování](https://msdn.microsoft.com/library/azure/dn933805.aspx).</span><span class="sxs-lookup"><span data-stu-id="a5c55-112">You can set up an Activity Log alert using hello [Azure PowerShell Cmdlets](insights-powershell-samples.md#create-metric-alerts), [Cross-Platform CLI](insights-cli-samples.md#work-with-alerts), or [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx).</span></span> <span data-ttu-id="a5c55-113">V současné době nelze nastavit ho vytvořit pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a5c55-113">Currently, you cannot set one up using hello Azure portal.</span></span>

## <a name="authenticating-hello-webhook"></a><span data-ttu-id="a5c55-114">Ověřování webhooku hello</span><span class="sxs-lookup"><span data-stu-id="a5c55-114">Authenticating hello webhook</span></span>
<span data-ttu-id="a5c55-115">Hello webhooku můžete ověřit pomocí některé z těchto metod:</span><span class="sxs-lookup"><span data-stu-id="a5c55-115">hello webhook can authenticate using either of these methods:</span></span>

1. <span data-ttu-id="a5c55-116">**Na základě tokenu autorizace** -hello webhooku identifikátor URI je uložit s tokenu ID, například`https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`</span><span class="sxs-lookup"><span data-stu-id="a5c55-116">**Token-based authorization** - hello webhook URI is saved with a token ID, for example, `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`</span></span>
2. <span data-ttu-id="a5c55-117">**Základní ověřování** -hello webhooku identifikátor URI je uložit pomocí uživatelského jména a hesla, například`https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`</span><span class="sxs-lookup"><span data-stu-id="a5c55-117">**Basic authorization** - hello webhook URI is saved with a username and password, for example, `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`</span></span>

## <a name="payload-schema"></a><span data-ttu-id="a5c55-118">Datová část schématu</span><span class="sxs-lookup"><span data-stu-id="a5c55-118">Payload schema</span></span>
<span data-ttu-id="a5c55-119">Hello operaci POST obsahuje hello datové části JSON a schéma pro všechny aktivity protokolu výstrahy.</span><span class="sxs-lookup"><span data-stu-id="a5c55-119">hello POST operation contains hello following JSON payload and schema for all Activity Log-based alerts.</span></span> <span data-ttu-id="a5c55-120">Toto schéma je podobné toohello jeden používá metrika na základě výstrahy.</span><span class="sxs-lookup"><span data-stu-id="a5c55-120">This schema is similar toohello one used by metric-based alerts.</span></span>

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

| <span data-ttu-id="a5c55-121">Název elementu</span><span class="sxs-lookup"><span data-stu-id="a5c55-121">Element Name</span></span> | <span data-ttu-id="a5c55-122">Popis</span><span class="sxs-lookup"><span data-stu-id="a5c55-122">Description</span></span> |
| --- | --- |
| <span data-ttu-id="a5c55-123">status</span><span class="sxs-lookup"><span data-stu-id="a5c55-123">status</span></span> |<span data-ttu-id="a5c55-124">Používá pro výstrahy, metriky.</span><span class="sxs-lookup"><span data-stu-id="a5c55-124">Used for metric alerts.</span></span> <span data-ttu-id="a5c55-125">Vždy nastaven příliš "aktivovaný" pro protokol aktivit výstrahy.</span><span class="sxs-lookup"><span data-stu-id="a5c55-125">Always set too"activated" for Activity Log alerts.</span></span> |
| <span data-ttu-id="a5c55-126">Kontext</span><span class="sxs-lookup"><span data-stu-id="a5c55-126">context</span></span> |<span data-ttu-id="a5c55-127">Kontext události hello.</span><span class="sxs-lookup"><span data-stu-id="a5c55-127">Context of hello event.</span></span> |
| <span data-ttu-id="a5c55-128">resourceProviderName</span><span class="sxs-lookup"><span data-stu-id="a5c55-128">resourceProviderName</span></span> |<span data-ttu-id="a5c55-129">Poskytovatel prostředků Hello hello dopad prostředků.</span><span class="sxs-lookup"><span data-stu-id="a5c55-129">hello resource provider of hello impacted resource.</span></span> |
| <span data-ttu-id="a5c55-130">conditionType</span><span class="sxs-lookup"><span data-stu-id="a5c55-130">conditionType</span></span> |<span data-ttu-id="a5c55-131">Vždy "událost".</span><span class="sxs-lookup"><span data-stu-id="a5c55-131">Always "Event."</span></span> |
| <span data-ttu-id="a5c55-132">jméno</span><span class="sxs-lookup"><span data-stu-id="a5c55-132">name</span></span> |<span data-ttu-id="a5c55-133">Název pravidla výstrahy hello.</span><span class="sxs-lookup"><span data-stu-id="a5c55-133">Name of hello alert rule.</span></span> |
| <span data-ttu-id="a5c55-134">id</span><span class="sxs-lookup"><span data-stu-id="a5c55-134">id</span></span> |<span data-ttu-id="a5c55-135">ID prostředku hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="a5c55-135">Resource ID of hello alert.</span></span> |
| <span data-ttu-id="a5c55-136">description</span><span class="sxs-lookup"><span data-stu-id="a5c55-136">description</span></span> |<span data-ttu-id="a5c55-137">Popis výstrahy jako sada během vytváření výstrahy hello.</span><span class="sxs-lookup"><span data-stu-id="a5c55-137">Alert description as set during creation of hello alert.</span></span> |
| <span data-ttu-id="a5c55-138">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="a5c55-138">subscriptionId</span></span> |<span data-ttu-id="a5c55-139">ID předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="a5c55-139">Azure Subscription ID.</span></span> |
| <span data-ttu-id="a5c55-140">časové razítko</span><span class="sxs-lookup"><span data-stu-id="a5c55-140">timestamp</span></span> |<span data-ttu-id="a5c55-141">Čas, na které hello vygenerovalo událost hello služby Azure, který zpracovává požadavek hello.</span><span class="sxs-lookup"><span data-stu-id="a5c55-141">Time at which hello event was generated by hello Azure service that processed hello request.</span></span> |
| <span data-ttu-id="a5c55-142">resourceId</span><span class="sxs-lookup"><span data-stu-id="a5c55-142">resourceId</span></span> |<span data-ttu-id="a5c55-143">ID prostředku hello dopad prostředků.</span><span class="sxs-lookup"><span data-stu-id="a5c55-143">Resource ID of hello impacted resource.</span></span> |
| <span data-ttu-id="a5c55-144">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="a5c55-144">resourceGroupName</span></span> |<span data-ttu-id="a5c55-145">Název skupiny prostředků hello hello dopad prostředků</span><span class="sxs-lookup"><span data-stu-id="a5c55-145">Name of hello resource group for hello impacted resource</span></span> |
| <span data-ttu-id="a5c55-146">properties</span><span class="sxs-lookup"><span data-stu-id="a5c55-146">properties</span></span> |<span data-ttu-id="a5c55-147">Sada `<Key, Value>` páry (tj. `Dictionary<String, String>`) obsahující podrobnosti o události hello.</span><span class="sxs-lookup"><span data-stu-id="a5c55-147">Set of `<Key, Value>` pairs (i.e. `Dictionary<String, String>`) that includes details about hello event.</span></span> |
| <span data-ttu-id="a5c55-148">Události</span><span class="sxs-lookup"><span data-stu-id="a5c55-148">event</span></span> |<span data-ttu-id="a5c55-149">Element obsahující metadata o hello událostí.</span><span class="sxs-lookup"><span data-stu-id="a5c55-149">Element containing metadata about hello event.</span></span> |
| <span data-ttu-id="a5c55-150">Autorizace</span><span class="sxs-lookup"><span data-stu-id="a5c55-150">authorization</span></span> |<span data-ttu-id="a5c55-151">Vlastnosti RBAC Hello hello události.</span><span class="sxs-lookup"><span data-stu-id="a5c55-151">hello RBAC properties of hello event.</span></span> <span data-ttu-id="a5c55-152">Obvykle patří hello "action" a "role" hello "obor."</span><span class="sxs-lookup"><span data-stu-id="a5c55-152">These usually include hello “action”, “role” and hello “scope.”</span></span> |
| <span data-ttu-id="a5c55-153">category</span><span class="sxs-lookup"><span data-stu-id="a5c55-153">category</span></span> |<span data-ttu-id="a5c55-154">Kategorie události hello.</span><span class="sxs-lookup"><span data-stu-id="a5c55-154">Category of hello event.</span></span> <span data-ttu-id="a5c55-155">Podporované hodnoty jsou: pro správu, výstrahy, zabezpečení, ServiceHealth, doporučení.</span><span class="sxs-lookup"><span data-stu-id="a5c55-155">Supported values include: Administrative, Alert, Security, ServiceHealth, Recommendation.</span></span> |
| <span data-ttu-id="a5c55-156">volající</span><span class="sxs-lookup"><span data-stu-id="a5c55-156">caller</span></span> |<span data-ttu-id="a5c55-157">E-mailovou adresu hello uživatele, který provedl hello operace, deklarace hlavní název uživatele nebo název SPN deklarace identity na základě dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="a5c55-157">Email address of hello user who performed hello operation, UPN claim, or SPN claim based on availability.</span></span> <span data-ttu-id="a5c55-158">Může mít hodnotu null pro určité systémová volání.</span><span class="sxs-lookup"><span data-stu-id="a5c55-158">Can be null for certain system calls.</span></span> |
| <span data-ttu-id="a5c55-159">correlationId</span><span class="sxs-lookup"><span data-stu-id="a5c55-159">correlationId</span></span> |<span data-ttu-id="a5c55-160">Obvykle GUID ve formátu řetězce.</span><span class="sxs-lookup"><span data-stu-id="a5c55-160">Usually a GUID in string format.</span></span> <span data-ttu-id="a5c55-161">Události se correlationId patří toohello stejné větší akce a obvykle sdílet correlationId.</span><span class="sxs-lookup"><span data-stu-id="a5c55-161">Events with correlationId belong toohello same larger action and usually share a correlationId.</span></span> |
| <span data-ttu-id="a5c55-162">eventDescription</span><span class="sxs-lookup"><span data-stu-id="a5c55-162">eventDescription</span></span> |<span data-ttu-id="a5c55-163">Statický text popisu události hello.</span><span class="sxs-lookup"><span data-stu-id="a5c55-163">Static text description of hello event.</span></span> |
| <span data-ttu-id="a5c55-164">eventDataId</span><span class="sxs-lookup"><span data-stu-id="a5c55-164">eventDataId</span></span> |<span data-ttu-id="a5c55-165">Jedinečný identifikátor pro událost hello.</span><span class="sxs-lookup"><span data-stu-id="a5c55-165">Unique identifier for hello event.</span></span> |
| <span data-ttu-id="a5c55-166">EventSource</span><span class="sxs-lookup"><span data-stu-id="a5c55-166">eventSource</span></span> |<span data-ttu-id="a5c55-167">Název hello služby Azure nebo infrastrukturu, že generovaný hello událost.</span><span class="sxs-lookup"><span data-stu-id="a5c55-167">Name of hello Azure service or infrastructure that generated hello event.</span></span> |
| <span data-ttu-id="a5c55-168">požadavku HTTP</span><span class="sxs-lookup"><span data-stu-id="a5c55-168">httpRequest</span></span> |<span data-ttu-id="a5c55-169">Obvykle zahrnuje hello "clientRequestId", "clientIpAddress" a "metodu" (například PUT metoda HTTP).</span><span class="sxs-lookup"><span data-stu-id="a5c55-169">Usually includes hello “clientRequestId”, “clientIpAddress” and “method” (HTTP method e.g. PUT).</span></span> |
| <span data-ttu-id="a5c55-170">úroveň</span><span class="sxs-lookup"><span data-stu-id="a5c55-170">level</span></span> |<span data-ttu-id="a5c55-171">Jeden z hello následující hodnoty: "Kritická", "Chyba", "Upozornění", "Informační" a "Verbose."</span><span class="sxs-lookup"><span data-stu-id="a5c55-171">One of hello following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose.”</span></span> |
| <span data-ttu-id="a5c55-172">operationId</span><span class="sxs-lookup"><span data-stu-id="a5c55-172">operationId</span></span> |<span data-ttu-id="a5c55-173">Obvykle GUID sdílen hello události odpovídající toosingle operaci.</span><span class="sxs-lookup"><span data-stu-id="a5c55-173">Usually a GUID shared among hello events corresponding toosingle operation.</span></span> |
| <span data-ttu-id="a5c55-174">operationName</span><span class="sxs-lookup"><span data-stu-id="a5c55-174">operationName</span></span> |<span data-ttu-id="a5c55-175">Název operace hello.</span><span class="sxs-lookup"><span data-stu-id="a5c55-175">Name of hello operation.</span></span> |
| <span data-ttu-id="a5c55-176">properties</span><span class="sxs-lookup"><span data-stu-id="a5c55-176">properties</span></span> |<span data-ttu-id="a5c55-177">Vlastnosti události hello.</span><span class="sxs-lookup"><span data-stu-id="a5c55-177">Properties of hello event.</span></span> |
| <span data-ttu-id="a5c55-178">status</span><span class="sxs-lookup"><span data-stu-id="a5c55-178">status</span></span> |<span data-ttu-id="a5c55-179">Řetězec.</span><span class="sxs-lookup"><span data-stu-id="a5c55-179">String.</span></span> <span data-ttu-id="a5c55-180">Stav operace hello.</span><span class="sxs-lookup"><span data-stu-id="a5c55-180">Status of hello operation.</span></span> <span data-ttu-id="a5c55-181">Běžné hodnoty jsou: "Spustit", "Probíhá", "Bylo úspěšně dokončeno", "Se nezdařilo", "Aktivní", "Přeložit".</span><span class="sxs-lookup"><span data-stu-id="a5c55-181">Common values include: "Started", "In Progress", "Succeeded", "Failed", "Active", "Resolved".</span></span> |
| <span data-ttu-id="a5c55-182">Podřízený stav</span><span class="sxs-lookup"><span data-stu-id="a5c55-182">subStatus</span></span> |<span data-ttu-id="a5c55-183">Obvykle zahrnuje stavový kód HTTP hello hello odpovídající volání REST.</span><span class="sxs-lookup"><span data-stu-id="a5c55-183">Usually includes hello HTTP status code of hello corresponding REST call.</span></span> <span data-ttu-id="a5c55-184">Může také obsahovat jiných řetězců popisující podřízeného stavu.</span><span class="sxs-lookup"><span data-stu-id="a5c55-184">It might also include other strings describing a substatus.</span></span> <span data-ttu-id="a5c55-185">Běžné substatus hodnoty patří: OK (stavový kód HTTP: 200), které byly vytvořeny (stavový kód HTTP: 201), platné (stavový kód HTTP: 202), ne obsahu (stavový kód HTTP: 204), chybný požadavek (stavový kód HTTP: 400), nebyl nalezen (stavový kód HTTP: 404), konflikt (stavový kód HTTP: 409), vnitřní chybu serveru (kód stavu HTTP: 500), služba není k dispozici (kód stavu HTTP: 503), vypršel časový limit brány (kód stavu HTTP: 504)</span><span class="sxs-lookup"><span data-stu-id="a5c55-185">Common substatus values include: OK (HTTP Status Code: 200), Created (HTTP Status Code: 201), Accepted (HTTP Status Code: 202), No Content (HTTP Status Code: 204), Bad Request (HTTP Status Code: 400), Not Found (HTTP Status Code: 404), Conflict (HTTP Status Code: 409), Internal Server Error (HTTP Status Code: 500), Service Unavailable (HTTP Status Code: 503), Gateway Timeout (HTTP Status Code: 504)</span></span> |

## <a name="next-steps"></a><span data-ttu-id="a5c55-186">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a5c55-186">Next steps</span></span>
* [<span data-ttu-id="a5c55-187">Další informace o hello protokol aktivit</span><span class="sxs-lookup"><span data-stu-id="a5c55-187">Learn more about hello Activity Log</span></span>](monitoring-overview-activity-logs.md)
* [<span data-ttu-id="a5c55-188">Spustit skripty Azure Automation (Runbooky) na Azure výstrahy</span><span class="sxs-lookup"><span data-stu-id="a5c55-188">Execute Azure Automation scripts (Runbooks) on Azure alerts</span></span>](http://go.microsoft.com/fwlink/?LinkId=627081)
* <span data-ttu-id="a5c55-189">[Použití aplikace logiky toosend serveru služby SMS prostřednictvím Twilio z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="a5c55-189">[Use Logic App toosend an SMS via Twilio from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).</span></span> <span data-ttu-id="a5c55-190">V tomto příkladu je pro metriky výstrahy, ale může být upravený toowork s výstrahu protokolu aktivit.</span><span class="sxs-lookup"><span data-stu-id="a5c55-190">This example is for metric alerts, but could be modified toowork with an Activity Log alert.</span></span>
* <span data-ttu-id="a5c55-191">[Použití aplikace logiky toosend Slack zprávu z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="a5c55-191">[Use Logic App toosend a Slack message from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).</span></span> <span data-ttu-id="a5c55-192">V tomto příkladu je pro metriky výstrahy, ale může být upravený toowork s výstrahu protokolu aktivit.</span><span class="sxs-lookup"><span data-stu-id="a5c55-192">This example is for metric alerts, but could be modified toowork with an Activity Log alert.</span></span>
* <span data-ttu-id="a5c55-193">[Použití aplikace logiky toosend zpráva tooan Azure Queue z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="a5c55-193">[Use Logic App toosend a message tooan Azure Queue from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).</span></span> <span data-ttu-id="a5c55-194">V tomto příkladu je pro metriky výstrahy, ale může být upravený toowork s výstrahu protokolu aktivit.</span><span class="sxs-lookup"><span data-stu-id="a5c55-194">This example is for metric alerts, but could be modified toowork with an Activity Log alert.</span></span>
