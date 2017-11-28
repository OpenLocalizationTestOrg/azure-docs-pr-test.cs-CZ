---
title: "aaaAzure schématu aktivity protokolu události | Microsoft Docs"
description: "Pochopení hello událostí schéma pro data do hello protokol aktivit"
author: johnkemnetz
manager: robb
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: johnkem
ms.openlocfilehash: dfece949a20a4d9b4e8a4d488c1c34842d87d586
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-activity-log-event-schema"></a><span data-ttu-id="fbe5c-103">Azure schématu aktivity protokolu události</span><span class="sxs-lookup"><span data-stu-id="fbe5c-103">Azure Activity Log event schema</span></span>
<span data-ttu-id="fbe5c-104">Hello **protokol činnosti Azure** protokolu, který poskytuje přehled o všech událostí na úrovni předplatného, k nimž došlo v Azure.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-104">hello **Azure Activity Log** is a log that provides insight into any subscription-level events that have occurred in Azure.</span></span> <span data-ttu-id="fbe5c-105">Tento článek popisuje hello schématu události podle kategorie dat.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-105">This article describes hello event schema per category of data.</span></span>

## <a name="administrative"></a><span data-ttu-id="fbe5c-106">Pro správu</span><span class="sxs-lookup"><span data-stu-id="fbe5c-106">Administrative</span></span>
<span data-ttu-id="fbe5c-107">Tato kategorie obsahuje hello záznam všech vytvořit, operace aktualizace, odstranění a akce provést pomocí Správce prostředků.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-107">This category contains hello record of all create, update, delete, and action operations performed through Resource Manager.</span></span> <span data-ttu-id="fbe5c-108">Příklady hello, které typy událostí, které zobrazí se v této kategorii "vytvořit virtuální počítač" a "odstranit skupinu zabezpečení sítě" každé akce uživatele nebo aplikace pomocí Správce prostředků je modelovaná jako operace na konkrétní typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-108">Examples of hello types of events you would see in this category include "create virtual machine" and "delete network security group" Every action taken by a user or application using Resource Manager is modeled as an operation on a particular resource type.</span></span> <span data-ttu-id="fbe5c-109">Pokud je typ operace hello zapsat, Delete nebo akce, záznamy hello hello start a úspěch nebo selhání této operace se zaznamenávají do administrativní kategorie hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-109">If hello operation type is Write, Delete, or Action, hello records of both hello start and success or fail of that operation are recorded in hello Administrative category.</span></span> <span data-ttu-id="fbe5c-110">Administrativní kategorie Hello také zahrnuje všechny řízení přístupu na základě toorole změny v předplatném.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-110">hello Administrative category also includes any changes toorole-based access control in a subscription.</span></span>

### <a name="sample-event"></a><span data-ttu-id="fbe5c-111">Událost vzorku</span><span class="sxs-lookup"><span data-stu-id="fbe5c-111">Sample event</span></span>
```json
{
  "authorization": {
    "action": "microsoft.support/supporttickets/write",
    "role": "Subscription Admin",
    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841"
  },
  "caller": "admin@contoso.com",
  "channels": "Operation",
  "claims": {
    "aud": "https://management.core.windows.net/",
    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
    "iat": "1421876371",
    "nbf": "1421876371",
    "exp": "1421880271",
    "ver": "1.0",
    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
    "puid": "20030000801A118C",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
    "name": "John Smith",
    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
    "appidacr": "2",
    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
    "http://schemas.microsoft.com/claims/authnclassreference": "1"
  },
  "correlationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
  "description": "",
  "eventDataId": "44ade6b4-3813-45e6-ae27-7420a95fa2f8",
  "eventName": {
    "value": "EndRequest",
    "localizedValue": "End request"
  },
  "httpRequest": {
    "clientRequestId": "27003b25-91d3-418f-8eb1-29e537dcb249",
    "clientIpAddress": "192.168.35.115",
    "method": "PUT"
  },
  "id": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841/events/44ade6b4-3813-45e6-ae27-7420a95fa2f8/ticks/635574752669792776",
  "level": "Informational",
  "resourceGroupName": "MSSupportGroup",
  "resourceProviderName": {
    "value": "microsoft.support",
    "localizedValue": "microsoft.support"
  },
  "resourceUri": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
  "operationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
  "operationName": {
    "value": "microsoft.support/supporttickets/write",
    "localizedValue": "microsoft.support/supporttickets/write"
  },
  "properties": {
    "statusCode": "Created"
  },
  "status": {
    "value": "Succeeded",
    "localizedValue": "Succeeded"
  },
  "subStatus": {
    "value": "Created",
    "localizedValue": "Created (HTTP Status Code: 201)"
  },
  "eventTimestamp": "2015-01-21T22:14:26.9792776Z",
  "submissionTimestamp": "2015-01-21T22:14:39.9936304Z",
  "subscriptionId": "s1"
}
```

### <a name="property-descriptions"></a><span data-ttu-id="fbe5c-112">Popisy vlastností</span><span class="sxs-lookup"><span data-stu-id="fbe5c-112">Property descriptions</span></span>
| <span data-ttu-id="fbe5c-113">Název elementu</span><span class="sxs-lookup"><span data-stu-id="fbe5c-113">Element Name</span></span> | <span data-ttu-id="fbe5c-114">Popis</span><span class="sxs-lookup"><span data-stu-id="fbe5c-114">Description</span></span> |
| --- | --- |
| <span data-ttu-id="fbe5c-115">Autorizace</span><span class="sxs-lookup"><span data-stu-id="fbe5c-115">authorization</span></span> |<span data-ttu-id="fbe5c-116">Objekt BLOB RBAC vlastností události hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-116">Blob of RBAC properties of hello event.</span></span> <span data-ttu-id="fbe5c-117">Obvykle zahrnuje vlastnosti "action", "role" a "obor" hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-117">Usually includes hello “action”, “role” and “scope” properties.</span></span> |
| <span data-ttu-id="fbe5c-118">volající</span><span class="sxs-lookup"><span data-stu-id="fbe5c-118">caller</span></span> |<span data-ttu-id="fbe5c-119">E-mailovou adresu hello uživatele, který se má provést operaci hello, deklarace nebo SPN deklarace identity na základě dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-119">Email address of hello user who has performed hello operation, UPN claim, or SPN claim based on availability.</span></span> |
| <span data-ttu-id="fbe5c-120">Kanály</span><span class="sxs-lookup"><span data-stu-id="fbe5c-120">channels</span></span> |<span data-ttu-id="fbe5c-121">Jeden z hello následující hodnoty: "Admin", "Operace"</span><span class="sxs-lookup"><span data-stu-id="fbe5c-121">One of hello following values: “Admin”, “Operation”</span></span> |
| <span data-ttu-id="fbe5c-122">Deklarace identity</span><span class="sxs-lookup"><span data-stu-id="fbe5c-122">claims</span></span> |<span data-ttu-id="fbe5c-123">Hello JWT token používá služby Active Directory tooauthenticate hello uživatele nebo aplikace tooperform tuto operaci v správce prostředků.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-123">hello JWT token used by Active Directory tooauthenticate hello user or application tooperform this operation in resource manager.</span></span> |
| <span data-ttu-id="fbe5c-124">correlationId</span><span class="sxs-lookup"><span data-stu-id="fbe5c-124">correlationId</span></span> |<span data-ttu-id="fbe5c-125">Obvykle GUID ve formátu řetězce hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-125">Usually a GUID in hello string format.</span></span> <span data-ttu-id="fbe5c-126">Události, které sdílejí correlationId patří toohello stejné uber akce.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-126">Events that share a correlationId belong toohello same uber action.</span></span> |
| <span data-ttu-id="fbe5c-127">description</span><span class="sxs-lookup"><span data-stu-id="fbe5c-127">description</span></span> |<span data-ttu-id="fbe5c-128">Statický text popisu události.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-128">Static text description of an event.</span></span> |
| <span data-ttu-id="fbe5c-129">eventDataId</span><span class="sxs-lookup"><span data-stu-id="fbe5c-129">eventDataId</span></span> |<span data-ttu-id="fbe5c-130">Jedinečný identifikátor události.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-130">Unique identifier of an event.</span></span> |
| <span data-ttu-id="fbe5c-131">požadavku HTTP</span><span class="sxs-lookup"><span data-stu-id="fbe5c-131">httpRequest</span></span> |<span data-ttu-id="fbe5c-132">Objekt BLOB popisující hello požadavek Http.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-132">Blob describing hello Http Request.</span></span> <span data-ttu-id="fbe5c-133">Obvykle zahrnuje hello "clientRequestId", "clientIpAddress" a "metodu" (metoda HTTP.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-133">Usually includes hello “clientRequestId”, “clientIpAddress” and “method” (HTTP method.</span></span> <span data-ttu-id="fbe5c-134">For example, PUT).</span><span class="sxs-lookup"><span data-stu-id="fbe5c-134">For example, PUT).</span></span> |
| <span data-ttu-id="fbe5c-135">úroveň</span><span class="sxs-lookup"><span data-stu-id="fbe5c-135">level</span></span> |<span data-ttu-id="fbe5c-136">Úroveň události hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-136">Level of hello event.</span></span> <span data-ttu-id="fbe5c-137">Jeden z hello následující hodnoty: "Kritická", "Chyba", "Upozornění", "Informační" a "Podrobné"</span><span class="sxs-lookup"><span data-stu-id="fbe5c-137">One of hello following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose”</span></span> |
| <span data-ttu-id="fbe5c-138">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="fbe5c-138">resourceGroupName</span></span> |<span data-ttu-id="fbe5c-139">Název skupiny prostředků hello hello dopad prostředků.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-139">Name of hello resource group for hello impacted resource.</span></span> |
| <span data-ttu-id="fbe5c-140">resourceProviderName</span><span class="sxs-lookup"><span data-stu-id="fbe5c-140">resourceProviderName</span></span> |<span data-ttu-id="fbe5c-141">Název zprostředkovatele prostředků hello hello dopad prostředků</span><span class="sxs-lookup"><span data-stu-id="fbe5c-141">Name of hello resource provider for hello impacted resource</span></span> |
| <span data-ttu-id="fbe5c-142">resourceId</span><span class="sxs-lookup"><span data-stu-id="fbe5c-142">resourceId</span></span> |<span data-ttu-id="fbe5c-143">Id prostředku hello dopad prostředků.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-143">Resource id of hello impacted resource.</span></span> |
| <span data-ttu-id="fbe5c-144">operationId</span><span class="sxs-lookup"><span data-stu-id="fbe5c-144">operationId</span></span> |<span data-ttu-id="fbe5c-145">Identifikátor GUID sdílen hello události, které odpovídají tooa jedné operace.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-145">A GUID shared among hello events that correspond tooa single operation.</span></span> |
| <span data-ttu-id="fbe5c-146">operationName</span><span class="sxs-lookup"><span data-stu-id="fbe5c-146">operationName</span></span> |<span data-ttu-id="fbe5c-147">Název operace hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-147">Name of hello operation.</span></span> |
| <span data-ttu-id="fbe5c-148">properties</span><span class="sxs-lookup"><span data-stu-id="fbe5c-148">properties</span></span> |<span data-ttu-id="fbe5c-149">Sada `<Key, Value>` páry (tedy slovník) popisující hello podrobnosti události hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-149">Set of `<Key, Value>` pairs (that is, a Dictionary) describing hello details of hello event.</span></span> |
| <span data-ttu-id="fbe5c-150">status</span><span class="sxs-lookup"><span data-stu-id="fbe5c-150">status</span></span> |<span data-ttu-id="fbe5c-151">Řetězec s popisem hello stav operace hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-151">String describing hello status of hello operation.</span></span> <span data-ttu-id="fbe5c-152">Některé běžné hodnoty jsou: spuštění v průběhu, bylo úspěšné, neúspěšné, aktivní, vyřešeno.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-152">Some common values are: Started, In Progress, Succeeded, Failed, Active, Resolved.</span></span> |
| <span data-ttu-id="fbe5c-153">Podřízený stav</span><span class="sxs-lookup"><span data-stu-id="fbe5c-153">subStatus</span></span> |<span data-ttu-id="fbe5c-154">Obvykle hello stavový kód HTTP hello odpovídající volání REST, ale můžou taky patřit jiných řetězců popisující podřízeného stavu, jako jsou tyto hodnoty běžné: OK (stavový kód HTTP: 200), které byly vytvořeny (stavový kód HTTP: 201), platné (stavový kód HTTP: 202), ne obsahu (HTTP Stavový kód: 204), chybný požadavek (kód stavu HTTP: 400), nebyl nalezen (kód stavu HTTP: 404), konflikt (kód stavu HTTP: 409), vnitřní chybu serveru (kód stavu HTTP: 500), služba není k dispozici (kód stavu HTTP: 503), vypršel časový limit brány (kód stavu HTTP: 504).</span><span class="sxs-lookup"><span data-stu-id="fbe5c-154">Usually hello HTTP status code of hello corresponding REST call, but can also include other strings describing a substatus, such as these common values: OK (HTTP Status Code: 200), Created (HTTP Status Code: 201), Accepted (HTTP Status Code: 202), No Content (HTTP Status Code: 204), Bad Request (HTTP Status Code: 400), Not Found (HTTP Status Code: 404), Conflict (HTTP Status Code: 409), Internal Server Error (HTTP Status Code: 500), Service Unavailable (HTTP Status Code: 503), Gateway Timeout (HTTP Status Code: 504).</span></span> |
| <span data-ttu-id="fbe5c-155">eventTimestamp</span><span class="sxs-lookup"><span data-stu-id="fbe5c-155">eventTimestamp</span></span> |<span data-ttu-id="fbe5c-156">Časové razítko při hello událostí vygenerovalo hello zpracování služby Azure hello žádost odpovídající hello událost.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-156">Timestamp when hello event was generated by hello Azure service processing hello request corresponding hello event.</span></span> |
| <span data-ttu-id="fbe5c-157">submissionTimestamp</span><span class="sxs-lookup"><span data-stu-id="fbe5c-157">submissionTimestamp</span></span> |<span data-ttu-id="fbe5c-158">Časové razítko, když hello události jsou k dispozici pro zadávání dotazů.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-158">Timestamp when hello event became available for querying.</span></span> |
| <span data-ttu-id="fbe5c-159">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="fbe5c-159">subscriptionId</span></span> |<span data-ttu-id="fbe5c-160">ID předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-160">Azure Subscription Id.</span></span> |

## <a name="service-health"></a><span data-ttu-id="fbe5c-161">Stav služby</span><span class="sxs-lookup"><span data-stu-id="fbe5c-161">Service health</span></span>
<span data-ttu-id="fbe5c-162">Tato kategorie obsahuje záznam hello všechny služby stavu incidentů, kterým došlo v Azure.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-162">This category contains hello record of any service health incidents that have occurred in Azure.</span></span> <span data-ttu-id="fbe5c-163">Příkladem hello typu události, které se zobrazí se v této kategorii je "SQL Azure v oblasti Východ USA dochází k výpadku."</span><span class="sxs-lookup"><span data-stu-id="fbe5c-163">An example of hello type of event you would see in this category is "SQL Azure in East US is experiencing downtime."</span></span> <span data-ttu-id="fbe5c-164">Události stavu služby existují ve pět variantách: je vyžadována akce, jíž obnovení, Incident, údržby, informace nebo zabezpečení a objeví jenom tehdy, pokud máte prostředku v hello předplatné, které by ovlivňovat hello událostí.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-164">Service health events come in five varieties: Action Required, Assisted Recovery, Incident, Maintenance, Information, or Security, and only appear if you have a resource in hello subscription that would be impacted by hello event.</span></span>

### <a name="sample-event"></a><span data-ttu-id="fbe5c-165">Událost vzorku</span><span class="sxs-lookup"><span data-stu-id="fbe5c-165">Sample event</span></span>
```json
{
  "channels": "Admin",
  "correlationId": "c550176b-8f52-4380-bdc5-36c1b59d3a44",
  "description": "Active: Network Infrastructure - UK South",
  "eventDataId": "c5bc4514-6642-2be3-453e-c6a67841b073",
  "eventName": {
      "value": null
  },
  "category": {
      "value": "ServiceHealth",
      "localizedValue": "Service Health"
  },
  "eventTimestamp": "2017-07-20T23:30:14.8022297Z",
  "id": "/subscriptions/mySubscriptionID/events/c5bc4514-6642-2be3-453e-c6a67841b073/ticks/636361902148022297",
  "level": "Warning",
  "operationName": {
      "value": "Microsoft.ServiceHealth/incident/action",
      "localizedValue": "Microsoft.ServiceHealth/incident/action"
  },
  "resourceProviderName": {
      "value": null
  },
  "resourceType": {
      "value": null,
      "localizedValue": ""
  },
  "resourceId": "/subscriptions/mySubscriptionID",
  "status": {
      "value": "Active",
      "localizedValue": "Active"
  },
  "subStatus": {
      "value": null
  },
  "submissionTimestamp": "2017-07-20T23:30:34.7431946Z",
  "subscriptionId": "mySubscriptionID",
  "properties": {
    "title": "Network Infrastructure - UK South",
    "service": "Service Fabric",
    "region": "UK South",
    "communication": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited tooApp Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Search. Multiple engineering teams are engaged in multiple workflows toomitigate hello impact. hello next update will be provided in 60 minutes, or as events warrant.",
    "incidentType": "Incident",
    "trackingId": "NA0F-BJG",
    "impactStartTime": "2017-07-20T21:41:00.0000000Z",
    "impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"UK South\"}],\"ServiceName\":\"Service Fabric\"}]",
    "defaultLanguageTitle": "Network Infrastructure - UK South",
    "defaultLanguageContent": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited tooApp Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Search. Multiple engineering teams are engaged in multiple workflows toomitigate hello impact. hello next update will be provided in 60 minutes, or as events warrant.",
    "stage": "Active",
    "communicationId": "636361902146035247",
    "version": "0.1.1"
  }
}
```

### <a name="property-descriptions"></a><span data-ttu-id="fbe5c-166">Popisy vlastností</span><span class="sxs-lookup"><span data-stu-id="fbe5c-166">Property descriptions</span></span>
<span data-ttu-id="fbe5c-167">Název elementu</span><span class="sxs-lookup"><span data-stu-id="fbe5c-167">Element Name</span></span> | <span data-ttu-id="fbe5c-168">Popis</span><span class="sxs-lookup"><span data-stu-id="fbe5c-168">Description</span></span>
-------- | -----------
<span data-ttu-id="fbe5c-169">Kanály</span><span class="sxs-lookup"><span data-stu-id="fbe5c-169">channels</span></span> | <span data-ttu-id="fbe5c-170">Patří mezi hello následující hodnoty: "Admin", "Operace"</span><span class="sxs-lookup"><span data-stu-id="fbe5c-170">Is one of hello following values: “Admin”, “Operation”</span></span>
<span data-ttu-id="fbe5c-171">correlationId</span><span class="sxs-lookup"><span data-stu-id="fbe5c-171">correlationId</span></span> | <span data-ttu-id="fbe5c-172">Obvykle je identifikátor GUID ve formátu řetězce hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-172">Is usually a GUID in hello string format.</span></span> <span data-ttu-id="fbe5c-173">Události, patří toohello obvykle sdílet stejnou akci uber hello stejné correlationId.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-173">Events with that belong toohello same uber action usually share hello same correlationId.</span></span>
<span data-ttu-id="fbe5c-174">description</span><span class="sxs-lookup"><span data-stu-id="fbe5c-174">description</span></span> | <span data-ttu-id="fbe5c-175">Popis události hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-175">Description of hello event.</span></span>
<span data-ttu-id="fbe5c-176">eventDataId</span><span class="sxs-lookup"><span data-stu-id="fbe5c-176">eventDataId</span></span> | <span data-ttu-id="fbe5c-177">Hello jedinečný identifikátor události.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-177">hello unique identifier of an event.</span></span>
<span data-ttu-id="fbe5c-178">EventName</span><span class="sxs-lookup"><span data-stu-id="fbe5c-178">eventName</span></span> | <span data-ttu-id="fbe5c-179">Název Hello hello události.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-179">hello title of hello event.</span></span>
<span data-ttu-id="fbe5c-180">úroveň</span><span class="sxs-lookup"><span data-stu-id="fbe5c-180">level</span></span> | <span data-ttu-id="fbe5c-181">Úroveň události hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-181">Level of hello event.</span></span> <span data-ttu-id="fbe5c-182">Jeden z hello následující hodnoty: "Kritická", "Chyba", "Upozornění", "Informační" a "Podrobné"</span><span class="sxs-lookup"><span data-stu-id="fbe5c-182">One of hello following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose”</span></span>
<span data-ttu-id="fbe5c-183">resourceProviderName</span><span class="sxs-lookup"><span data-stu-id="fbe5c-183">resourceProviderName</span></span> | <span data-ttu-id="fbe5c-184">Název zprostředkovatele prostředků hello hello dopad prostředků.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-184">Name of hello resource provider for hello impacted resource.</span></span> <span data-ttu-id="fbe5c-185">Pokud není známý, to bude mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-185">If not known, this will be null.</span></span>
<span data-ttu-id="fbe5c-186">Typ prostředku</span><span class="sxs-lookup"><span data-stu-id="fbe5c-186">resourceType</span></span>| <span data-ttu-id="fbe5c-187">Hello typ prostředku hello dopad prostředků.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-187">hello type of resource of hello impacted resource.</span></span> <span data-ttu-id="fbe5c-188">Pokud není známý, to bude mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-188">If not known, this will be null.</span></span>
<span data-ttu-id="fbe5c-189">Podřízený stav</span><span class="sxs-lookup"><span data-stu-id="fbe5c-189">subStatus</span></span> | <span data-ttu-id="fbe5c-190">Obvykle se hodnota null pro události stavu služby.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-190">Usually null for Service Health events.</span></span>
<span data-ttu-id="fbe5c-191">eventTimestamp</span><span class="sxs-lookup"><span data-stu-id="fbe5c-191">eventTimestamp</span></span> | <span data-ttu-id="fbe5c-192">Časové razítko, když hello protokolu událostí se vygeneroval a odeslání toohello protokol aktivit.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-192">Timestamp when hello log event was generated and submitted toohello Activity Log.</span></span>
<span data-ttu-id="fbe5c-193">submissionTimestamp</span><span class="sxs-lookup"><span data-stu-id="fbe5c-193">submissionTimestamp</span></span> |   <span data-ttu-id="fbe5c-194">Časové razítko, když jsou k dispozici v hello aktivity protokolu události hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-194">Timestamp when hello event became available in hello Activity Log.</span></span>
<span data-ttu-id="fbe5c-195">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="fbe5c-195">subscriptionId</span></span> | <span data-ttu-id="fbe5c-196">předplatné Azure, ve kterém se tato událost byla zaznamenána Hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-196">hello Azure subscription in which this event was logged.</span></span>
<span data-ttu-id="fbe5c-197">status</span><span class="sxs-lookup"><span data-stu-id="fbe5c-197">status</span></span> | <span data-ttu-id="fbe5c-198">Řetězec s popisem hello stav operace hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-198">String describing hello status of hello operation.</span></span> <span data-ttu-id="fbe5c-199">Některé běžné hodnoty jsou: aktivní a vyřešit.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-199">Some common values are: Active, Resolved.</span></span>
<span data-ttu-id="fbe5c-200">operationName</span><span class="sxs-lookup"><span data-stu-id="fbe5c-200">operationName</span></span> | <span data-ttu-id="fbe5c-201">Název operace hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-201">Name of hello operation.</span></span> <span data-ttu-id="fbe5c-202">Obvykle Microsoft.ServiceHealth/incident/action.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-202">Usually Microsoft.ServiceHealth/incident/action.</span></span>
<span data-ttu-id="fbe5c-203">category</span><span class="sxs-lookup"><span data-stu-id="fbe5c-203">category</span></span> | <span data-ttu-id="fbe5c-204">"ServiceHealth"</span><span class="sxs-lookup"><span data-stu-id="fbe5c-204">"ServiceHealth"</span></span>
<span data-ttu-id="fbe5c-205">resourceId</span><span class="sxs-lookup"><span data-stu-id="fbe5c-205">resourceId</span></span> | <span data-ttu-id="fbe5c-206">Id prostředku hello dopad prostředků, pokud je znám.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-206">Resource id of hello impacted resource, if known.</span></span> <span data-ttu-id="fbe5c-207">V opačném případě je zadáno ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-207">Subscription ID is provided otherwise.</span></span>
<span data-ttu-id="fbe5c-208">Properties.Title</span><span class="sxs-lookup"><span data-stu-id="fbe5c-208">Properties.title</span></span> | <span data-ttu-id="fbe5c-209">Hello lokalizovaný název pro tuto komunikaci.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-209">hello localized title for this communication.</span></span> <span data-ttu-id="fbe5c-210">Hello výchozím jazykem je angličtina.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-210">English is hello default language.</span></span>
<span data-ttu-id="fbe5c-211">Properties.Communication</span><span class="sxs-lookup"><span data-stu-id="fbe5c-211">Properties.communication</span></span> | <span data-ttu-id="fbe5c-212">Hello lokalizované podrobnosti hello komunikace s značka jazyka HTML.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-212">hello localized details of hello communication with HTML markup.</span></span> <span data-ttu-id="fbe5c-213">Výchozí hello je angličtina.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-213">English is hello default.</span></span>
<span data-ttu-id="fbe5c-214">Properties.incidentType</span><span class="sxs-lookup"><span data-stu-id="fbe5c-214">Properties.incidentType</span></span> | <span data-ttu-id="fbe5c-215">Možné hodnoty: AssistedRecovery, ActionRequired, informace, incidentů, údržby, zabezpečení</span><span class="sxs-lookup"><span data-stu-id="fbe5c-215">Possible values: AssistedRecovery, ActionRequired, Information, Incident, Maintenance, Security</span></span>
<span data-ttu-id="fbe5c-216">Properties.trackingId</span><span class="sxs-lookup"><span data-stu-id="fbe5c-216">Properties.trackingId</span></span> | <span data-ttu-id="fbe5c-217">Identifikuje hello incident, které se tato událost je přidružen.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-217">Identifies hello incident this event is associated with.</span></span> <span data-ttu-id="fbe5c-218">Pomocí tohoto toocorrelate hello události související tooan incidentu.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-218">Use this toocorrelate hello events related tooan incident.</span></span>
<span data-ttu-id="fbe5c-219">Properties.impactedServices</span><span class="sxs-lookup"><span data-stu-id="fbe5c-219">Properties.impactedServices</span></span> | <span data-ttu-id="fbe5c-220">Uvozený blob JSON, který popisuje hello služeb a oblasti, které jsou ovlivněny incidentem hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-220">An escaped JSON blob which describes hello services and regions that are impacted by hello incident.</span></span> <span data-ttu-id="fbe5c-221">Seznam služeb, z nichž každá má ServiceName a seznam ImpactedRegions, z nichž každá má RegionName.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-221">A list of Services, each of which has a ServiceName and a list of ImpactedRegions, each of which has a RegionName.</span></span>
<span data-ttu-id="fbe5c-222">Properties.defaultLanguageTitle</span><span class="sxs-lookup"><span data-stu-id="fbe5c-222">Properties.defaultLanguageTitle</span></span> | <span data-ttu-id="fbe5c-223">Hello komunikace v angličtině</span><span class="sxs-lookup"><span data-stu-id="fbe5c-223">hello communication in English</span></span>
<span data-ttu-id="fbe5c-224">Properties.defaultLanguageContent</span><span class="sxs-lookup"><span data-stu-id="fbe5c-224">Properties.defaultLanguageContent</span></span> | <span data-ttu-id="fbe5c-225">Hello komunikace v angličtině jako značka jazyka html nebo prostý text</span><span class="sxs-lookup"><span data-stu-id="fbe5c-225">hello communication in English as either html markup or plain text</span></span>
<span data-ttu-id="fbe5c-226">Properties.Stage</span><span class="sxs-lookup"><span data-stu-id="fbe5c-226">Properties.stage</span></span> | <span data-ttu-id="fbe5c-227">Možné hodnoty pro AssistedRecovery, ActionRequired, informace, incidentů, zabezpečení: jsou aktivní, vyřešeno.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-227">Possible values for AssistedRecovery, ActionRequired, Information, Incident, Security: are Active, Resolved.</span></span> <span data-ttu-id="fbe5c-228">Za účelem údržby jsou: aktivní, plánovaná, InProgress, zrušení, Rescheduled, vyřešeno, dokončeno</span><span class="sxs-lookup"><span data-stu-id="fbe5c-228">For Maintenance they are: Active, Planned, InProgress, Canceled, Rescheduled, Resolved, Complete</span></span>
<span data-ttu-id="fbe5c-229">Properties.communicationId</span><span class="sxs-lookup"><span data-stu-id="fbe5c-229">Properties.communicationId</span></span> | <span data-ttu-id="fbe5c-230">komunikace Hello Tato událost souvisí.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-230">hello communication this event is associated.</span></span>

## <a name="alert"></a><span data-ttu-id="fbe5c-231">Výstrahy</span><span class="sxs-lookup"><span data-stu-id="fbe5c-231">Alert</span></span>
<span data-ttu-id="fbe5c-232">Tato kategorie obsahuje záznam hello všechny aktivací Azure výstrah.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-232">This category contains hello record of all activations of Azure alerts.</span></span> <span data-ttu-id="fbe5c-233">Příkladem hello typu události, který zobrazí se v této kategorii je "% využití procesoru na Můjvp přes 80 pro hello za posledních 5 minut."</span><span class="sxs-lookup"><span data-stu-id="fbe5c-233">An example of hello type of event you would see in this category is "CPU % on myVM has been over 80 for hello past 5 minutes."</span></span> <span data-ttu-id="fbe5c-234">Výstrahy koncept mají různé systémy Azure – můžete definovat pravidla nějaká a přijímat oznámení v případě podmínky odpovídají daného pravidla.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-234">A variety of Azure systems have an alerting concept -- you can define a rule of some sort and receive a notification when conditions match that rule.</span></span> <span data-ttu-id="fbe5c-235">Pokaždé, když podporovaných Azure typu výstrahy, aktivuje,' nebo hello podmínky jsou splněny toogenerate oznámení, záznam hello aktivace je také poslat toothis kategorii hello protokol aktivit.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-235">Each time a supported Azure alert type 'activates,' or hello conditions are met toogenerate a notification, a record of hello activation is also pushed toothis category of hello Activity Log.</span></span>

### <a name="sample-event"></a><span data-ttu-id="fbe5c-236">Událost vzorku</span><span class="sxs-lookup"><span data-stu-id="fbe5c-236">Sample event</span></span>

```json
{
  "caller": "Microsoft.Insights/alertRules",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/alertRules"
  },
  "correlationId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "description": "'Disk read LessThan 100000 ([Count]) in hello last 5 minutes' has been resolved for CloudService: myResourceGroup/Production/Event.BackgroundJobsWorker.razzle (myResourceGroup)",
  "eventDataId": "149d4baf-53dc-4cf4-9e29-17de37405cd9",
  "eventName": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "category": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "id": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle/events/149d4baf-53dc-4cf4-9e29-17de37405cd9/ticks/636362258535221920",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "Microsoft.ClassicCompute",
    "localizedValue": "Microsoft.ClassicCompute"
  },
  "resourceId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle",
  "resourceType": {
    "value": "Microsoft.ClassicCompute/domainNames/slots/roles",
    "localizedValue": "Microsoft.ClassicCompute/domainNames/slots/roles"
  },
  "operationId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "operationName": {
    "value": "Microsoft.Insights/AlertRules/Resolved/Action",
    "localizedValue": "Microsoft.Insights/AlertRules/Resolved/Action"
  },
  "properties": {
    "RuleUri": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert",
    "RuleName": "myalert",
    "RuleDescription": "",
    "Threshold": "100000",
    "WindowSizeInMinutes": "5",
    "Aggregation": "Average",
    "Operator": "LessThan",
    "MetricName": "Disk read",
    "MetricUnit": "Count"
  },
  "status": {
    "value": "Resolved",
    "localizedValue": "Resolved"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T09:24:13.522192Z",
  "submissionTimestamp": "2017-07-21T09:24:15.6578651Z",
  "subscriptionId": "mySubscriptionID"
}
```

### <a name="property-descriptions"></a><span data-ttu-id="fbe5c-237">Popisy vlastností</span><span class="sxs-lookup"><span data-stu-id="fbe5c-237">Property descriptions</span></span>
| <span data-ttu-id="fbe5c-238">Název elementu</span><span class="sxs-lookup"><span data-stu-id="fbe5c-238">Element Name</span></span> | <span data-ttu-id="fbe5c-239">Popis</span><span class="sxs-lookup"><span data-stu-id="fbe5c-239">Description</span></span> |
| --- | --- |
| <span data-ttu-id="fbe5c-240">volající</span><span class="sxs-lookup"><span data-stu-id="fbe5c-240">caller</span></span> | <span data-ttu-id="fbe5c-241">Vždy Microsoft.Insights/alertRules</span><span class="sxs-lookup"><span data-stu-id="fbe5c-241">Always Microsoft.Insights/alertRules</span></span> |
| <span data-ttu-id="fbe5c-242">Kanály</span><span class="sxs-lookup"><span data-stu-id="fbe5c-242">channels</span></span> | <span data-ttu-id="fbe5c-243">Vždy "Admin, operace"</span><span class="sxs-lookup"><span data-stu-id="fbe5c-243">Always “Admin, Operation”</span></span> |
| <span data-ttu-id="fbe5c-244">Deklarace identity</span><span class="sxs-lookup"><span data-stu-id="fbe5c-244">claims</span></span> | <span data-ttu-id="fbe5c-245">Objekt blob JSON typu hello hlavního názvu služby (hlavní název služby), nebo prostředku, hello výstrahy stroje.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-245">JSON blob with hello SPN (service principal name), or resource type, of hello alert engine.</span></span> |
| <span data-ttu-id="fbe5c-246">correlationId</span><span class="sxs-lookup"><span data-stu-id="fbe5c-246">correlationId</span></span> | <span data-ttu-id="fbe5c-247">Identifikátor GUID ve formátu řetězce hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-247">A GUID in hello string format.</span></span> |
| <span data-ttu-id="fbe5c-248">description</span><span class="sxs-lookup"><span data-stu-id="fbe5c-248">description</span></span> |<span data-ttu-id="fbe5c-249">Statický text popis výstrah událostí hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-249">Static text description of hello alert event.</span></span> |
| <span data-ttu-id="fbe5c-250">eventDataId</span><span class="sxs-lookup"><span data-stu-id="fbe5c-250">eventDataId</span></span> |<span data-ttu-id="fbe5c-251">Jedinečný identifikátor hello výstrah události.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-251">Unique identifier of hello alert event.</span></span> |
| <span data-ttu-id="fbe5c-252">úroveň</span><span class="sxs-lookup"><span data-stu-id="fbe5c-252">level</span></span> |<span data-ttu-id="fbe5c-253">Úroveň události hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-253">Level of hello event.</span></span> <span data-ttu-id="fbe5c-254">Jeden z hello následující hodnoty: "Kritická", "Chyba", "Upozornění", "Informační" a "Podrobné"</span><span class="sxs-lookup"><span data-stu-id="fbe5c-254">One of hello following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose”</span></span> |
| <span data-ttu-id="fbe5c-255">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="fbe5c-255">resourceGroupName</span></span> |<span data-ttu-id="fbe5c-256">Název skupiny prostředků hello hello dopad prostředků, pokud se jedná o metriky výstrahu.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-256">Name of hello resource group for hello impacted resource if it is a metric alert.</span></span> <span data-ttu-id="fbe5c-257">Pro ostatní typy výstrah jde hello název skupiny prostředků hello, který obsahuje hello výstrahy samotné.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-257">For other alert types, this is hello name of hello resource group that contains hello alert itself.</span></span> |
| <span data-ttu-id="fbe5c-258">resourceProviderName</span><span class="sxs-lookup"><span data-stu-id="fbe5c-258">resourceProviderName</span></span> |<span data-ttu-id="fbe5c-259">Název zprostředkovatele prostředků hello hello ovlivněná prostředků je metriky výstrahy.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-259">Name of hello resource provider for hello impacted resource if it is a metric alert.</span></span> <span data-ttu-id="fbe5c-260">Pro ostatní typy výstrah jde hello název zprostředkovatele prostředků hello hello výstrahy sám sebe.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-260">For other alert types, this is hello name of hello resource provider for hello alert itself.</span></span> |
| <span data-ttu-id="fbe5c-261">resourceId</span><span class="sxs-lookup"><span data-stu-id="fbe5c-261">resourceId</span></span> | <span data-ttu-id="fbe5c-262">Název hello ID prostředku pro hello dopad prostředků, pokud se jedná o metriky výstrahu.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-262">Name of hello resource ID for hello impacted resource if it is a metric alert.</span></span> <span data-ttu-id="fbe5c-263">Pro ostatní typy výstrah Toto je ID prostředku hello hello výstrahy prostředku sám sebe.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-263">For other alert types, this is hello resource ID of hello alert resource itself.</span></span> |
| <span data-ttu-id="fbe5c-264">operationId</span><span class="sxs-lookup"><span data-stu-id="fbe5c-264">operationId</span></span> |<span data-ttu-id="fbe5c-265">Identifikátor GUID sdílen hello události, které odpovídají tooa jedné operace.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-265">A GUID shared among hello events that correspond tooa single operation.</span></span> |
| <span data-ttu-id="fbe5c-266">operationName</span><span class="sxs-lookup"><span data-stu-id="fbe5c-266">operationName</span></span> |<span data-ttu-id="fbe5c-267">Název operace hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-267">Name of hello operation.</span></span> |
| <span data-ttu-id="fbe5c-268">properties</span><span class="sxs-lookup"><span data-stu-id="fbe5c-268">properties</span></span> |<span data-ttu-id="fbe5c-269">Sada `<Key, Value>` páry (tedy slovník) popisující hello podrobnosti události hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-269">Set of `<Key, Value>` pairs (that is, a Dictionary) describing hello details of hello event.</span></span> |
| <span data-ttu-id="fbe5c-270">status</span><span class="sxs-lookup"><span data-stu-id="fbe5c-270">status</span></span> |<span data-ttu-id="fbe5c-271">Řetězec s popisem hello stav operace hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-271">String describing hello status of hello operation.</span></span> <span data-ttu-id="fbe5c-272">Některé běžné hodnoty jsou: spuštění v průběhu, bylo úspěšné, neúspěšné, aktivní, vyřešeno.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-272">Some common values are: Started, In Progress, Succeeded, Failed, Active, Resolved.</span></span> |
| <span data-ttu-id="fbe5c-273">Podřízený stav</span><span class="sxs-lookup"><span data-stu-id="fbe5c-273">subStatus</span></span> | <span data-ttu-id="fbe5c-274">Obvykle se hodnota null pro výstrahy.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-274">Usually null for alerts.</span></span> |
| <span data-ttu-id="fbe5c-275">eventTimestamp</span><span class="sxs-lookup"><span data-stu-id="fbe5c-275">eventTimestamp</span></span> |<span data-ttu-id="fbe5c-276">Časové razítko při hello událostí vygenerovalo hello zpracování služby Azure hello žádost odpovídající hello událost.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-276">Timestamp when hello event was generated by hello Azure service processing hello request corresponding hello event.</span></span> |
| <span data-ttu-id="fbe5c-277">submissionTimestamp</span><span class="sxs-lookup"><span data-stu-id="fbe5c-277">submissionTimestamp</span></span> |<span data-ttu-id="fbe5c-278">Časové razítko, když hello události jsou k dispozici pro zadávání dotazů.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-278">Timestamp when hello event became available for querying.</span></span> |
| <span data-ttu-id="fbe5c-279">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="fbe5c-279">subscriptionId</span></span> |<span data-ttu-id="fbe5c-280">ID předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-280">Azure Subscription Id.</span></span> |

### <a name="properties-field-per-alert-type"></a><span data-ttu-id="fbe5c-281">Vlastnosti pole na každý typ výstrahy</span><span class="sxs-lookup"><span data-stu-id="fbe5c-281">Properties field per alert type</span></span>
<span data-ttu-id="fbe5c-282">Hello vlastnosti pole bude obsahovat různé hodnoty v závislosti na hello zdroj hello výstrah událostí.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-282">hello properties field will contain different values depending on hello source of hello alert event.</span></span> <span data-ttu-id="fbe5c-283">Dva poskytovatelé běžné výstrah událostí jsou výstrahy protokol aktivit a metriky výstrahy.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-283">Two common alert event providers are Activity Log alerts and metric alerts.</span></span>

#### <a name="properties-for-activity-log-alerts"></a><span data-ttu-id="fbe5c-284">Vlastnosti výstrah protokol aktivit</span><span class="sxs-lookup"><span data-stu-id="fbe5c-284">Properties for Activity Log alerts</span></span>
| <span data-ttu-id="fbe5c-285">Název elementu</span><span class="sxs-lookup"><span data-stu-id="fbe5c-285">Element Name</span></span> | <span data-ttu-id="fbe5c-286">Popis</span><span class="sxs-lookup"><span data-stu-id="fbe5c-286">Description</span></span> |
| --- | --- |
| <span data-ttu-id="fbe5c-287">properties.subscriptionId</span><span class="sxs-lookup"><span data-stu-id="fbe5c-287">properties.subscriptionId</span></span> | <span data-ttu-id="fbe5c-288">ID předplatného Hello z hello aktivity protokolu události, který chybu způsobil této aktivity protokolu pravidlo výstrahy toobe aktivován.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-288">hello subscription ID from hello activity log event which caused this activity log alert rule toobe activated.</span></span> |
| <span data-ttu-id="fbe5c-289">properties.eventDataId</span><span class="sxs-lookup"><span data-stu-id="fbe5c-289">properties.eventDataId</span></span> | <span data-ttu-id="fbe5c-290">ID události dat Hello z hello aktivity protokolu události, který chybu způsobil této aktivity protokolu pravidlo výstrahy toobe aktivován.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-290">hello event data ID from hello activity log event which caused this activity log alert rule toobe activated.</span></span> |
| <span data-ttu-id="fbe5c-291">properties.resourceGroup</span><span class="sxs-lookup"><span data-stu-id="fbe5c-291">properties.resourceGroup</span></span> | <span data-ttu-id="fbe5c-292">Skupina prostředků Hello z hello aktivity protokolu události, který chybu způsobil této aktivity protokolu pravidlo výstrahy toobe aktivován.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-292">hello resource group from hello activity log event which caused this activity log alert rule toobe activated.</span></span> |
| <span data-ttu-id="fbe5c-293">properties.resourceId</span><span class="sxs-lookup"><span data-stu-id="fbe5c-293">properties.resourceId</span></span> | <span data-ttu-id="fbe5c-294">ID prostředku Hello z hello aktivity protokolu události, který chybu způsobil této aktivity protokolu pravidlo výstrahy toobe aktivován.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-294">hello resource ID from hello activity log event which caused this activity log alert rule toobe activated.</span></span> |
| <span data-ttu-id="fbe5c-295">properties.eventTimestamp</span><span class="sxs-lookup"><span data-stu-id="fbe5c-295">properties.eventTimestamp</span></span> | <span data-ttu-id="fbe5c-296">časové razítko události Hello hello aktivity protokolu události, který chybu způsobil této aktivity protokolu pravidlo výstrahy toobe aktivován.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-296">hello event timestamp of hello activity log event which caused this activity log alert rule toobe activated.</span></span> |
| <span data-ttu-id="fbe5c-297">properties.operationName</span><span class="sxs-lookup"><span data-stu-id="fbe5c-297">properties.operationName</span></span> | <span data-ttu-id="fbe5c-298">Název operace Hello z hello aktivity protokolu události, který chybu způsobil této aktivity protokolu pravidlo výstrahy toobe aktivován.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-298">hello operation name from hello activity log event which caused this activity log alert rule toobe activated.</span></span> |
| <span data-ttu-id="fbe5c-299">Properties.status</span><span class="sxs-lookup"><span data-stu-id="fbe5c-299">properties.status</span></span> | <span data-ttu-id="fbe5c-300">Stav Hello z hello aktivity protokolu události, který chybu způsobil této aktivity protokolu pravidlo výstrahy toobe aktivován.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-300">hello status from hello activity log event which caused this activity log alert rule toobe activated.</span></span>|

#### <a name="properties-for-metric-alerts"></a><span data-ttu-id="fbe5c-301">Vlastnosti metriky výstrah</span><span class="sxs-lookup"><span data-stu-id="fbe5c-301">Properties for metric alerts</span></span>
| <span data-ttu-id="fbe5c-302">Název elementu</span><span class="sxs-lookup"><span data-stu-id="fbe5c-302">Element Name</span></span> | <span data-ttu-id="fbe5c-303">Popis</span><span class="sxs-lookup"><span data-stu-id="fbe5c-303">Description</span></span> |
| --- | --- |
| <span data-ttu-id="fbe5c-304">Vlastnosti. RuleUri</span><span class="sxs-lookup"><span data-stu-id="fbe5c-304">properties.RuleUri</span></span> | <span data-ttu-id="fbe5c-305">ID prostředku hello metriky pravidlo výstrahy sám sebe.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-305">Resource ID of hello metric alert rule itself.</span></span> |
| <span data-ttu-id="fbe5c-306">Vlastnosti. RuleName</span><span class="sxs-lookup"><span data-stu-id="fbe5c-306">properties.RuleName</span></span> | <span data-ttu-id="fbe5c-307">Hello název metriky pravidlo výstrahy hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-307">hello name of hello metric alert rule.</span></span> |
| <span data-ttu-id="fbe5c-308">Vlastnosti. RuleDescription</span><span class="sxs-lookup"><span data-stu-id="fbe5c-308">properties.RuleDescription</span></span> | <span data-ttu-id="fbe5c-309">Popis Hello hello metriky pravidlo výstrahy (jak je definována v pravidlo výstrahy hello).</span><span class="sxs-lookup"><span data-stu-id="fbe5c-309">hello description of hello metric alert rule (as defined in hello alert rule).</span></span> |
| <span data-ttu-id="fbe5c-310">Vlastnosti. Prahová hodnota</span><span class="sxs-lookup"><span data-stu-id="fbe5c-310">properties.Threshold</span></span> | <span data-ttu-id="fbe5c-311">Prahová hodnota Hello hello vyhodnocení hello metriky pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-311">hello threshold value used in hello evaluation of hello metric alert rule.</span></span> |
| <span data-ttu-id="fbe5c-312">Vlastnosti. WindowSizeInMinutes</span><span class="sxs-lookup"><span data-stu-id="fbe5c-312">properties.WindowSizeInMinutes</span></span> | <span data-ttu-id="fbe5c-313">velikost okna Hello hello vyhodnocení hello metriky pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-313">hello window size used in hello evaluation of hello metric alert rule.</span></span> |
| <span data-ttu-id="fbe5c-314">Vlastnosti. Agregace</span><span class="sxs-lookup"><span data-stu-id="fbe5c-314">properties.Aggregation</span></span> | <span data-ttu-id="fbe5c-315">Typ agregace Hello definované v hello metriky pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-315">hello aggregation type defined in hello metric alert rule.</span></span> |
| <span data-ttu-id="fbe5c-316">Vlastnosti. Operátor</span><span class="sxs-lookup"><span data-stu-id="fbe5c-316">properties.Operator</span></span> | <span data-ttu-id="fbe5c-317">Podmíněný operátor Hello hello vyhodnocení hello metriky pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-317">hello conditional operator used in hello evaluation of hello metric alert rule.</span></span> |
| <span data-ttu-id="fbe5c-318">Vlastnosti. MetricName</span><span class="sxs-lookup"><span data-stu-id="fbe5c-318">properties.MetricName</span></span> | <span data-ttu-id="fbe5c-319">Název metriky Hello hello metriky hello vyhodnocení hello metriky pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-319">hello metric name of hello metric used in hello evaluation of hello metric alert rule.</span></span> |
| <span data-ttu-id="fbe5c-320">Vlastnosti. MetricUnit</span><span class="sxs-lookup"><span data-stu-id="fbe5c-320">properties.MetricUnit</span></span> | <span data-ttu-id="fbe5c-321">Hello metriky jednotky pro metriku hello hello vyhodnocení hello metriky pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-321">hello metric unit for hello metric used in hello evaluation of hello metric alert rule.</span></span> |

## <a name="autoscale"></a><span data-ttu-id="fbe5c-322">Automatické škálování</span><span class="sxs-lookup"><span data-stu-id="fbe5c-322">Autoscale</span></span>
<span data-ttu-id="fbe5c-323">Tato kategorie obsahuje záznam hello jakékoli operace související toohello události modulu hello škálování podle nastavení automatického škálování, které jste definovali ve vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-323">This category contains hello record of any events related toohello operation of hello autoscale engine based on any autoscale settings you have defined in your subscription.</span></span> <span data-ttu-id="fbe5c-324">Příkladem hello typu události, které se zobrazí se v této kategorii je "Škálování rozšiřování škálování využívajících akce se nezdařila."</span><span class="sxs-lookup"><span data-stu-id="fbe5c-324">An example of hello type of event you would see in this category is "Autoscale scale up action failed."</span></span> <span data-ttu-id="fbe5c-325">Použití automatického škálování, můžete automaticky škálovat nebo škálování hello podle počtu instancí v podporovaných prostředků typu čas, den nebo zatížení (metriky) dat, na které se používá nastavení automatického škálování.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-325">Using autoscale, you can automatically scale out or scale in hello number of instances in a supported resource type based on time of day and/or load (metric) data using an autoscale setting.</span></span> <span data-ttu-id="fbe5c-326">Při splnění podmínek hello tooscale nahoru nebo dolů, hello spuštění a úspěšné nebo neúspěšné události budou popsané v této kategorii.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-326">When hello conditions are met tooscale up or down, hello start and succeeded or failed events will be recorded in this category.</span></span>

### <a name="sample-event"></a><span data-ttu-id="fbe5c-327">Událost vzorku</span><span class="sxs-lookup"><span data-stu-id="fbe5c-327">Sample event</span></span>
```json
{
  "caller": "Microsoft.Insights/autoscaleSettings",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/autoscaleSettings"
  },
  "correlationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "description": "hello autoscale engine attempting tooscale resource '/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count too2 instances count.",
  "eventDataId": "a5b92075-1de9-42f1-b52e-6f3e4945a7c7",
  "eventName": {
    "value": "AutoscaleAction",
    "localizedValue": "AutoscaleAction"
  },
  "category": {
    "value": "Autoscale",
    "localizedValue": "Autoscale"
  },
  "id": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup/events/a5b92075-1de9-42f1-b52e-6f3e4945a7c7/ticks/636361956518681572",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "microsoft.insights",
    "localizedValue": "microsoft.insights"
  },
  "resourceId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup",
  "resourceType": {
    "value": "microsoft.insights/autoscalesettings",
    "localizedValue": "microsoft.insights/autoscalesettings"
  },
  "operationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "operationName": {
    "value": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action",
    "localizedValue": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action"
  },
  "properties": {
    "Description": "hello autoscale engine attempting tooscale resource '/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count too2 instances count.",
    "ResourceName": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource",
    "OldInstancesCount": "3",
    "NewInstancesCount": "2",
    "LastScaleActionTime": "Fri, 21 Jul 2017 01:00:51 GMT"
  },
  "status": {
    "value": "Succeeded",
    "localizedValue": "Succeeded"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T01:00:51.8681572Z",
  "submissionTimestamp": "2017-07-21T01:00:52.3008754Z",
  "subscriptionId": "mySubscriptionID"
}

```

### <a name="property-descriptions"></a><span data-ttu-id="fbe5c-328">Popisy vlastností</span><span class="sxs-lookup"><span data-stu-id="fbe5c-328">Property descriptions</span></span>
| <span data-ttu-id="fbe5c-329">Název elementu</span><span class="sxs-lookup"><span data-stu-id="fbe5c-329">Element Name</span></span> | <span data-ttu-id="fbe5c-330">Popis</span><span class="sxs-lookup"><span data-stu-id="fbe5c-330">Description</span></span> |
| --- | --- |
| <span data-ttu-id="fbe5c-331">volající</span><span class="sxs-lookup"><span data-stu-id="fbe5c-331">caller</span></span> | <span data-ttu-id="fbe5c-332">Vždy Microsoft.Insights/autoscaleSettings</span><span class="sxs-lookup"><span data-stu-id="fbe5c-332">Always Microsoft.Insights/autoscaleSettings</span></span> |
| <span data-ttu-id="fbe5c-333">Kanály</span><span class="sxs-lookup"><span data-stu-id="fbe5c-333">channels</span></span> | <span data-ttu-id="fbe5c-334">Vždy "Admin, operace"</span><span class="sxs-lookup"><span data-stu-id="fbe5c-334">Always “Admin, Operation”</span></span> |
| <span data-ttu-id="fbe5c-335">Deklarace identity</span><span class="sxs-lookup"><span data-stu-id="fbe5c-335">claims</span></span> | <span data-ttu-id="fbe5c-336">Objekt blob JSON s hello hlavního názvu služby (hlavní název služby), nebo prostředek typu hello škálování stroje.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-336">JSON blob with hello SPN (service principal name), or resource type, of hello autoscale engine.</span></span> |
| <span data-ttu-id="fbe5c-337">correlationId</span><span class="sxs-lookup"><span data-stu-id="fbe5c-337">correlationId</span></span> | <span data-ttu-id="fbe5c-338">Identifikátor GUID ve formátu řetězce hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-338">A GUID in hello string format.</span></span> |
| <span data-ttu-id="fbe5c-339">description</span><span class="sxs-lookup"><span data-stu-id="fbe5c-339">description</span></span> |<span data-ttu-id="fbe5c-340">Statický text popis události škálování hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-340">Static text description of hello autoscale event.</span></span> |
| <span data-ttu-id="fbe5c-341">eventDataId</span><span class="sxs-lookup"><span data-stu-id="fbe5c-341">eventDataId</span></span> |<span data-ttu-id="fbe5c-342">Jedinečný identifikátor události škálování hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-342">Unique identifier of hello autoscale event.</span></span> |
| <span data-ttu-id="fbe5c-343">úroveň</span><span class="sxs-lookup"><span data-stu-id="fbe5c-343">level</span></span> |<span data-ttu-id="fbe5c-344">Úroveň události hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-344">Level of hello event.</span></span> <span data-ttu-id="fbe5c-345">Jeden z hello následující hodnoty: "Kritická", "Chyba", "Upozornění", "Informační" a "Podrobné"</span><span class="sxs-lookup"><span data-stu-id="fbe5c-345">One of hello following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose”</span></span> |
| <span data-ttu-id="fbe5c-346">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="fbe5c-346">resourceGroupName</span></span> |<span data-ttu-id="fbe5c-347">Název skupiny prostředků hello nastavení automatického škálování hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-347">Name of hello resource group for hello autoscale setting.</span></span> |
| <span data-ttu-id="fbe5c-348">resourceProviderName</span><span class="sxs-lookup"><span data-stu-id="fbe5c-348">resourceProviderName</span></span> |<span data-ttu-id="fbe5c-349">Název zprostředkovatele prostředků hello pro nastavení automatického škálování hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-349">Name of hello resource provider for hello autoscale setting.</span></span> |
| <span data-ttu-id="fbe5c-350">resourceId</span><span class="sxs-lookup"><span data-stu-id="fbe5c-350">resourceId</span></span> |<span data-ttu-id="fbe5c-351">Id prostředku nastavení automatického škálování hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-351">Resource id of hello autoscale setting.</span></span> |
| <span data-ttu-id="fbe5c-352">operationId</span><span class="sxs-lookup"><span data-stu-id="fbe5c-352">operationId</span></span> |<span data-ttu-id="fbe5c-353">Identifikátor GUID sdílen hello události, které odpovídají tooa jedné operace.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-353">A GUID shared among hello events that correspond tooa single operation.</span></span> |
| <span data-ttu-id="fbe5c-354">operationName</span><span class="sxs-lookup"><span data-stu-id="fbe5c-354">operationName</span></span> |<span data-ttu-id="fbe5c-355">Název operace hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-355">Name of hello operation.</span></span> |
| <span data-ttu-id="fbe5c-356">properties</span><span class="sxs-lookup"><span data-stu-id="fbe5c-356">properties</span></span> |<span data-ttu-id="fbe5c-357">Sada `<Key, Value>` páry (tedy slovník) popisující hello podrobnosti události hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-357">Set of `<Key, Value>` pairs (that is, a Dictionary) describing hello details of hello event.</span></span> |
| <span data-ttu-id="fbe5c-358">Vlastnosti. Popis</span><span class="sxs-lookup"><span data-stu-id="fbe5c-358">properties.Description</span></span> | <span data-ttu-id="fbe5c-359">Podrobný popis co modul škálování hello dělal.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-359">Detailed description of what hello autoscale engine was doing.</span></span> |
| <span data-ttu-id="fbe5c-360">Vlastnosti. ResourceName</span><span class="sxs-lookup"><span data-stu-id="fbe5c-360">properties.ResourceName</span></span> | <span data-ttu-id="fbe5c-361">ID prostředku hello dopad prostředků (hello prostředků, na které hello byla provedena akce škálování)</span><span class="sxs-lookup"><span data-stu-id="fbe5c-361">Resource ID of hello impacted resource (hello resource on which hello scale action was being performed)</span></span> |
| <span data-ttu-id="fbe5c-362">Vlastnosti. OldInstancesCount</span><span class="sxs-lookup"><span data-stu-id="fbe5c-362">properties.OldInstancesCount</span></span> | <span data-ttu-id="fbe5c-363">Hello počet instancí před akci škálování hello trvalo vliv.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-363">hello number of instances before hello autoscale action took effect.</span></span> |
| <span data-ttu-id="fbe5c-364">Vlastnosti. NewInstancesCount</span><span class="sxs-lookup"><span data-stu-id="fbe5c-364">properties.NewInstancesCount</span></span> | <span data-ttu-id="fbe5c-365">Hello počet instancí po akci škálování hello trvalo vliv.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-365">hello number of instances after hello autoscale action took effect.</span></span> |
| <span data-ttu-id="fbe5c-366">Vlastnosti. LastScaleActionTime</span><span class="sxs-lookup"><span data-stu-id="fbe5c-366">properties.LastScaleActionTime</span></span> | <span data-ttu-id="fbe5c-367">razítko Hello při akci škálování hello došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-367">hello timestamp of when hello autoscale action occurred.</span></span> |
| <span data-ttu-id="fbe5c-368">status</span><span class="sxs-lookup"><span data-stu-id="fbe5c-368">status</span></span> |<span data-ttu-id="fbe5c-369">Řetězec s popisem hello stav operace hello.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-369">String describing hello status of hello operation.</span></span> <span data-ttu-id="fbe5c-370">Některé běžné hodnoty jsou: spuštění v průběhu, bylo úspěšné, neúspěšné, aktivní, vyřešeno.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-370">Some common values are: Started, In Progress, Succeeded, Failed, Active, Resolved.</span></span> |
| <span data-ttu-id="fbe5c-371">Podřízený stav</span><span class="sxs-lookup"><span data-stu-id="fbe5c-371">subStatus</span></span> | <span data-ttu-id="fbe5c-372">Obvykle se hodnota null pro škálování.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-372">Usually null for autoscale.</span></span> |
| <span data-ttu-id="fbe5c-373">eventTimestamp</span><span class="sxs-lookup"><span data-stu-id="fbe5c-373">eventTimestamp</span></span> |<span data-ttu-id="fbe5c-374">Časové razítko při hello událostí vygenerovalo hello zpracování služby Azure hello žádost odpovídající hello událost.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-374">Timestamp when hello event was generated by hello Azure service processing hello request corresponding hello event.</span></span> |
| <span data-ttu-id="fbe5c-375">submissionTimestamp</span><span class="sxs-lookup"><span data-stu-id="fbe5c-375">submissionTimestamp</span></span> |<span data-ttu-id="fbe5c-376">Časové razítko, když hello události jsou k dispozici pro zadávání dotazů.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-376">Timestamp when hello event became available for querying.</span></span> |
| <span data-ttu-id="fbe5c-377">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="fbe5c-377">subscriptionId</span></span> |<span data-ttu-id="fbe5c-378">ID předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="fbe5c-378">Azure Subscription Id.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="fbe5c-379">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fbe5c-379">Next steps</span></span>
* [<span data-ttu-id="fbe5c-380">Další informace o hello protokol aktivit (dříve protokoly auditu)</span><span class="sxs-lookup"><span data-stu-id="fbe5c-380">Learn more about hello Activity Log (formerly Audit Logs)</span></span>](monitoring-overview-activity-logs.md)
* [<span data-ttu-id="fbe5c-381">Stream hello protokol činnosti Azure tooEvent rozbočovače</span><span class="sxs-lookup"><span data-stu-id="fbe5c-381">Stream hello Azure Activity Log tooEvent Hubs</span></span>](monitoring-stream-activity-logs-event-hubs.md)
