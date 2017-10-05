---
title: "Azure mřížky událostí zabezpečení a ověřování"
description: "Popisuje mřížky událostí Azure a jeho koncepty."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/14/2017
ms.author: babanisa
ms.openlocfilehash: b6e1c7587c0b47d04862b4850741aaa3b7d191a8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="event-grid-security-and-authentication"></a><span data-ttu-id="8bdd9-103">Události zabezpečení mřížky a ověřování</span><span class="sxs-lookup"><span data-stu-id="8bdd9-103">Event Grid security and authentication</span></span> 

<span data-ttu-id="8bdd9-104">Azure mřížky událostí má tři typy ověřování:</span><span class="sxs-lookup"><span data-stu-id="8bdd9-104">Azure Event Grid has three types of authentication:</span></span>

* <span data-ttu-id="8bdd9-105">Odběry událostí</span><span class="sxs-lookup"><span data-stu-id="8bdd9-105">Event subscriptions</span></span>
* <span data-ttu-id="8bdd9-106">Publikování události</span><span class="sxs-lookup"><span data-stu-id="8bdd9-106">Event publishing</span></span>
* <span data-ttu-id="8bdd9-107">Doručení události Webhooku</span><span class="sxs-lookup"><span data-stu-id="8bdd9-107">WebHook event delivery</span></span>

## <a name="webhook-event-delivery"></a><span data-ttu-id="8bdd9-108">Události Webhooku doručení</span><span class="sxs-lookup"><span data-stu-id="8bdd9-108">WebHook Event delivery</span></span>

<span data-ttu-id="8bdd9-109">Webhook má jedna řada způsobů pro příjem událostí v reálném čase z Azure událostí mřížky.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-109">Webhooks are one of many ways to receive events in real time from Azure Event Grid.</span></span>

<span data-ttu-id="8bdd9-110">Pokaždé, když je připraven k dodání novou událost, odešle událost mřížky požadavek HTTP s vaší Webhooku s událostí v textu.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-110">Every time there is a new event ready to be delivered, Event Grid sends an HTTP request with to your WebHook with the event in the body.</span></span>

<span data-ttu-id="8bdd9-111">Při registraci svůj vlastní koncový bod Webhooku s událostí mřížky, odešle požadavek POST s kódem jednoduché ověření aby bylo možné prokázat vlastnictví koncový bod.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-111">When you register your own WebHook endpoint with Event Grid, it sends you a POST request with a simple validation code in order to prove endpoint ownership.</span></span> <span data-ttu-id="8bdd9-112">Vaše aplikace musí odpovídat tak, že odezva zpět ověřovacího kódu.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-112">Your app needs to respond by echoing back the validation code.</span></span> <span data-ttu-id="8bdd9-113">Událost mřížky nebude poskytovat události Webhooku koncových bodů, které nebyly předány ověření.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-113">Event Grid will not deliver events to WebHook endpoints that have not passed the validation.</span></span>
 
### <a name="validation-details"></a><span data-ttu-id="8bdd9-114">Podrobnosti o ověření:</span><span class="sxs-lookup"><span data-stu-id="8bdd9-114">Validation details:</span></span>

* <span data-ttu-id="8bdd9-115">Během aktualizace vytvoření odběru událostí odešle událost mřížky "SubscriptionValidationEvent" událostí do cílového koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-115">At the time of event subscription creation/update, Event Grid posts a “SubscriptionValidationEvent” event to the target endpoint.</span></span>
* <span data-ttu-id="8bdd9-116">Události obsahuje hodnotu hlavičky "Ověření: typ události".</span><span class="sxs-lookup"><span data-stu-id="8bdd9-116">The event contains a header value “Event-Type: Validation”.</span></span>
* <span data-ttu-id="8bdd9-117">V textu události má stejné schéma jako ostatní události událostí mřížky.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-117">The event body has the same schema as other Event Grid events.</span></span>
* <span data-ttu-id="8bdd9-118">Data události například zahrnuje vlastnost "ValidationCode" s náhodně generované řetězec "ValidationCode: acb13...".</span><span class="sxs-lookup"><span data-stu-id="8bdd9-118">The event data includes a “ValidationCode” property with a randomly generated string e.g. “ValidationCode: acb13…”.</span></span>

<span data-ttu-id="8bdd9-119">Aby bylo možné prokázat vlastnictví koncový bod, vracení např kód ověření "ValidationResponse: acb13...".</span><span class="sxs-lookup"><span data-stu-id="8bdd9-119">In order to prove endpoint ownership, echo back the validation code e.g “ValidationResponse: acb13…”.</span></span>

<span data-ttu-id="8bdd9-120">Nakonec je důležité si uvědomit, mřížky událostí Azure podporuje pouze HTTPS webhooku koncové body.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-120">Finally, it is important to note that Azure Event Grid only supports HTTPS webhook endpoints.</span></span>
## <a name="event-subscription"></a><span data-ttu-id="8bdd9-121">Odběru událostí</span><span class="sxs-lookup"><span data-stu-id="8bdd9-121">Event subscription</span></span>

<span data-ttu-id="8bdd9-122">K odběru události, musíte mít **Microsoft.EventGrid/EventSubscriptions/Write** oprávnění na požadovaný prostředek.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-122">To subscribe to an event, you must have the **Microsoft.EventGrid/EventSubscriptions/Write** permission on the required resource.</span></span> <span data-ttu-id="8bdd9-123">Je nutné toto oprávnění, protože píšete nové předplatné v oboru prostředku.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-123">You need this permission because you are writing a new subscription at the scope of the resource.</span></span> <span data-ttu-id="8bdd9-124">Požadovaný prostředek se liší v závislosti na tom, jestli se odběru tématu systému nebo vlastní heslo.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-124">The required resource differs based on whether you are subscribing to a system topic or custom topic.</span></span> <span data-ttu-id="8bdd9-125">Oba typy jsou popsané v této části.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-125">Both types are described in this section.</span></span>

### <a name="system-topics-azure-service-publishers"></a><span data-ttu-id="8bdd9-126">Témata týkající se systému (vydavateli služby Azure)</span><span class="sxs-lookup"><span data-stu-id="8bdd9-126">System topics (Azure service publishers)</span></span>

<span data-ttu-id="8bdd9-127">Pro systém témata budete potřebovat oprávnění k zápisu nového odběru události v oboru prostředku publikování události.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-127">For system topics, you need permission to write a new event subscription at the scope of the resource publishing the event.</span></span> <span data-ttu-id="8bdd9-128">Formát prostředku není:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`</span><span class="sxs-lookup"><span data-stu-id="8bdd9-128">The format of the resource is: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`</span></span>

<span data-ttu-id="8bdd9-129">Například k odběru události na účet úložiště s názvem **UCET**, potřebujete oprávnění Microsoft.EventGrid/EventSubscriptions/Write na:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`</span><span class="sxs-lookup"><span data-stu-id="8bdd9-129">For example, to subscribe to an event on a storage account named **myacct**, you need the Microsoft.EventGrid/EventSubscriptions/Write permission on: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`</span></span>

### <a name="custom-topics"></a><span data-ttu-id="8bdd9-130">Vlastní témata</span><span class="sxs-lookup"><span data-stu-id="8bdd9-130">Custom topics</span></span>

<span data-ttu-id="8bdd9-131">Pro vlastní témata budete potřebovat oprávnění k zápisu nového odběru události v oboru tématu událostí mřížky.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-131">For custom topics, you need permission to write a new event subscription at the scope of the Event Grid topic.</span></span> <span data-ttu-id="8bdd9-132">Formát prostředku není:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`</span><span class="sxs-lookup"><span data-stu-id="8bdd9-132">The format of the resource is: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`</span></span>

<span data-ttu-id="8bdd9-133">Například k odběru vlastní téma s názvem **mytopic**, potřebujete oprávnění Microsoft.EventGrid/EventSubscriptions/Write na:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`</span><span class="sxs-lookup"><span data-stu-id="8bdd9-133">For example, to subscribe to a custom topic named **mytopic**, you need the Microsoft.EventGrid/EventSubscriptions/Write permission on: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`</span></span>

## <a name="topic-publishing"></a><span data-ttu-id="8bdd9-134">Téma publikování</span><span class="sxs-lookup"><span data-stu-id="8bdd9-134">Topic publishing</span></span>

<span data-ttu-id="8bdd9-135">Témata týkající se použití sdíleného přístupového podpisu (SAS) nebo ověření pomocí klíče.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-135">Topics use either Shared Access Signature (SAS) or key authentication.</span></span> <span data-ttu-id="8bdd9-136">Doporučujeme, abyste SAS, ale ověření pomocí klíče poskytuje jednoduché programování a je kompatibilní s mnoha existující webhooku vydavatelů.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-136">We recommend SAS, but key authentication provides simple programming, and is compatible with many existing webhook publishers.</span></span> 

<span data-ttu-id="8bdd9-137">Můžete zahrnout ověřování hodnota v hlavičce protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-137">You include the authentication value in the HTTP header.</span></span> <span data-ttu-id="8bdd9-138">SAS, použijte **tokenu sas Æg** pro hodnotu hlavičky.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-138">For SAS, use **aeg-sas-token** for the header value.</span></span> <span data-ttu-id="8bdd9-139">Pro ověření pomocí klíče, použít **klíč sas Æg** pro hodnotu hlavičky.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-139">For key authentication, use **aeg-sas-key** for the header value.</span></span>

### <a name="key-authentication"></a><span data-ttu-id="8bdd9-140">Ověření pomocí klíče</span><span class="sxs-lookup"><span data-stu-id="8bdd9-140">Key authentication</span></span>

<span data-ttu-id="8bdd9-141">Ověření pomocí klíče je nejjednodušší formu ověřování.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-141">Key authentication is the simplest form of authentication.</span></span> <span data-ttu-id="8bdd9-142">Použijte formát:`aeg-sas-key: <your key>`</span><span class="sxs-lookup"><span data-stu-id="8bdd9-142">Use the format: `aeg-sas-key: <your key>`</span></span>

<span data-ttu-id="8bdd9-143">Například předáte klíč se:</span><span class="sxs-lookup"><span data-stu-id="8bdd9-143">For example, you pass a key with:</span></span> 

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a><span data-ttu-id="8bdd9-144">Tokeny SAS</span><span class="sxs-lookup"><span data-stu-id="8bdd9-144">SAS tokens</span></span>

<span data-ttu-id="8bdd9-145">Tokeny SAS pro událost mřížky zahrnují prostředku, čas vypršení platnosti a podpis.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-145">SAS tokens for Event Grid include the resource, an expiration time, and a signature.</span></span> <span data-ttu-id="8bdd9-146">Formát tokenu SAS: `r={resource}&e={expiration}&s={signature}`.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-146">The format of the SAS token is: `r={resource}&e={expiration}&s={signature}`.</span></span>

<span data-ttu-id="8bdd9-147">Prostředek je cesta k tématu, ke kterému jsou odesílání událostí.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-147">The resource is the path for the topic to which you are sending events.</span></span> <span data-ttu-id="8bdd9-148">Například je cesta platná prostředku:`https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`</span><span class="sxs-lookup"><span data-stu-id="8bdd9-148">For example, a valid resource path is: `https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`</span></span>

<span data-ttu-id="8bdd9-149">Generování podpisu z klíče.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-149">You generate the signature from a key.</span></span>

<span data-ttu-id="8bdd9-150">Například platná **tokenu sas Æg** hodnota je:</span><span class="sxs-lookup"><span data-stu-id="8bdd9-150">For example, a valid **aeg-sas-token** value is:</span></span>

```http
aeg-sas-token: r=https%3a%2f%2fmytopic.eventgrid.azure.net%2feventGrid%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=a4oNHpRZygINC%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
``` 

<span data-ttu-id="8bdd9-151">Následující příklad vytvoří SAS token pro použití s mřížky událostí:</span><span class="sxs-lookup"><span data-stu-id="8bdd9-151">The following example creates a SAS token for use with Event Grid:</span></span>

```cs
static string BuildSharedAccessSignature(string resource, DateTime expirationUtc, string key)
{
    const char Resource = 'r';
    const char Expiration = 'e';
    const char Signature = 's';

    string encodedResource = HttpUtility.UrlEncode(resource);
    string encodedExpirationUtc = HttpUtility.UrlEncode(expirationUtc.ToString());

    string unsignedSas = $"{Resource}={encodedResource}&{Expiration}={encodedExpirationUtc}";
    using (var hmac = new HMACSHA256(Convert.FromBase64String(key)))
    {
        string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(unsignedSas)));
        string encodedSignature = HttpUtility.UrlEncode(signature);
        string signedSas = $"{unsignedSas}&{Signature}={encodedSignature}";

        return signedSas;
    }
}
```

## <a name="management-access-control"></a><span data-ttu-id="8bdd9-152">Správa řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="8bdd9-152">Management Access Control</span></span>

<span data-ttu-id="8bdd9-153">Azure mřížky událostí můžete řídit úroveň přístupu k různým uživatelům dělat různé operace správy, jako je například seznam událostí odběry, vytvořte nové a generovat klíče.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-153">Azure Event Grid allows you to control the level of access given to different users to do various management operations such as list event subscriptions, create new ones, and generate keys.</span></span> <span data-ttu-id="8bdd9-154">Událost mřížky využívá Azure na základě Role přístupu zkontrolujte (RBAC) pro tento.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-154">Event Grid utilizes Azure's Role Based Access Check (RBAC) for this.</span></span>

### <a name="operation-types"></a><span data-ttu-id="8bdd9-155">Typy operací</span><span class="sxs-lookup"><span data-stu-id="8bdd9-155">Operation types</span></span>

<span data-ttu-id="8bdd9-156">Mřížky událostí Azure podporuje následující akce:</span><span class="sxs-lookup"><span data-stu-id="8bdd9-156">Azure event grid supports the following actions:</span></span>

* <span data-ttu-id="8bdd9-157">Microsoft.EventGrid/*/read</span><span class="sxs-lookup"><span data-stu-id="8bdd9-157">Microsoft.EventGrid/*/read</span></span> 
* <span data-ttu-id="8bdd9-158">Microsoft.EventGrid/*/write</span><span class="sxs-lookup"><span data-stu-id="8bdd9-158">Microsoft.EventGrid/*/write</span></span> 
* <span data-ttu-id="8bdd9-159">Microsoft.EventGrid/*/delete</span><span class="sxs-lookup"><span data-stu-id="8bdd9-159">Microsoft.EventGrid/*/delete</span></span> 
* <span data-ttu-id="8bdd9-160">Microsoft.EventGrid/eventSubscriptions/getFullUrl/action</span><span class="sxs-lookup"><span data-stu-id="8bdd9-160">Microsoft.EventGrid/eventSubscriptions/getFullUrl/action</span></span> 
* <span data-ttu-id="8bdd9-161">Microsoft.EventGrid/topics/listKeys/action</span><span class="sxs-lookup"><span data-stu-id="8bdd9-161">Microsoft.EventGrid/topics/listKeys/action</span></span> 
* <span data-ttu-id="8bdd9-162">Microsoft.EventGrid/topics/regenerateKey/action</span><span class="sxs-lookup"><span data-stu-id="8bdd9-162">Microsoft.EventGrid/topics/regenerateKey/action</span></span>

<span data-ttu-id="8bdd9-163">Vrátí poslední tři operace potenciálně tajné informace, který získá filtrován mimo normální operace čtení.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-163">The last three operations return potentially secret information which gets filtered out of normal read operations.</span></span> <span data-ttu-id="8bdd9-164">Je osvědčeným postupem můžete omezit přístup na tyto operace.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-164">It is best practice for you to restrict access to these operations.</span></span> <span data-ttu-id="8bdd9-165">Můžete vytvořit vlastní role pomocí [prostředí Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md), [rozhraní příkazového řádku Azure (CLI)](../active-directory/role-based-access-control-manage-access-azure-cli.md)a [REST API](../active-directory/role-based-access-control-manage-access-rest.md).</span><span class="sxs-lookup"><span data-stu-id="8bdd9-165">Custom roles can be created using [Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md), [Azure Command-Line Interface (CLI)](../active-directory/role-based-access-control-manage-access-azure-cli.md), and the [REST API](../active-directory/role-based-access-control-manage-access-rest.md).</span></span>

### <a name="enforcing-role-based-access-check-rbac"></a><span data-ttu-id="8bdd9-166">Vynucování Role na základě kontroly přístupu (RBAC)</span><span class="sxs-lookup"><span data-stu-id="8bdd9-166">Enforcing Role Based Access Check (RBAC)</span></span>

<span data-ttu-id="8bdd9-167">Použijte následující postup k vynucení RBAC pro různé uživatele:</span><span class="sxs-lookup"><span data-stu-id="8bdd9-167">Use the following steps to enforce RBAC for different users:</span></span>

#### <a name="create-a-custom-role-definition-file-json"></a><span data-ttu-id="8bdd9-168">Vytvořte soubor definice vlastních rolí (.json)</span><span class="sxs-lookup"><span data-stu-id="8bdd9-168">Create a custom role definition file (.json)</span></span>

<span data-ttu-id="8bdd9-169">Níže jsou uvedeny definice rolí událostí mřížky ukázka, která umožní uživatelům provádět různé sady akcí.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-169">The following are sample Event Grid role definitions which allow users to perform different sets of actions.</span></span>

<span data-ttu-id="8bdd9-170">**EventGridReadOnlyRole.json**: Povolit jenom pro čtení jenom operace.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-170">**EventGridReadOnlyRole.json**: Only allow read only operations.</span></span>
```json
{ 
  "Name": "Event grid read only role", 
  "Id": "7C0B6B59-A278-4B62-BA19-411B70753856", 
  "IsCustom": true, 
  "Description": "Event grid read only role", 
  "Actions": [ 
    "Microsoft.EventGrid/*/read" 
  ], 
  "NotActions": [ 
  ], 
  "AssignableScopes": [ 
    "/subscriptions/<Subscription Id>" 
  ] 
}
```

<span data-ttu-id="8bdd9-171">**EventGridNoDeleteListKeysRole.json**: Povolit akce s omezeným přístupem po ale zakáže akce odstranění.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-171">**EventGridNoDeleteListKeysRole.json**: Allow restricted post actions but disallow delete actions.</span></span>
```json
{ 
  "Name": "Event grid No Delete Listkeys role", 
  "Id": "B9170838-5F9D-4103-A1DE-60496F7C9174", 
  "IsCustom": true, 
  "Description": "Event grid No Delete Listkeys role", 
  "Actions": [     
    "Microsoft.EventGrid/*/write", 
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action" 
    "Microsoft.EventGrid/topics/listkeys/action", 
    "Microsoft.EventGrid/topics/regenerateKey/action" 
  ], 
  "NotActions": [ 
    "Microsoft.EventGrid/*/delete" 
  ], 
  "AssignableScopes": [ 
    "/subscriptions/<Subscription id>" 
  ] 
}
```
 
<span data-ttu-id="8bdd9-172">**EventGridContributorRole.json**: umožňuje všechny akce mřížky událostí.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-172">**EventGridContributorRole.json**: Allows all event grid actions.</span></span>  
```json
{ 
  "Name": "Event grid contributor role", 
  "Id": "4BA6FB33-2955-491B-A74F-53C9126C9514", 
  "IsCustom": true, 
  "Description": "Event grid contributor role", 
  "Actions": [ 
    "Microsoft.EventGrid/*/write", 
    "Microsoft.EventGrid/*/delete", 
    "Microsoft.EventGrid/topics/listkeys/action", 
    "Microsoft.EventGrid/topics/regenerateKey/action", 
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action" 
  ], 
  "NotActions": [], 
  "AssignableScopes": [ 
    "/subscriptions/d48566a8-2428-4a6c-8347-9675d09fb851" 
  ] 
} 
```

#### <a name="install-and-login-to-azure-cli"></a><span data-ttu-id="8bdd9-173">Instalace a přihlášení k rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="8bdd9-173">Install and login to Azure CLI</span></span>

* <span data-ttu-id="8bdd9-174">Azure CLI verze 0.8.8 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-174">Azure CLI version 0.8.8 or later.</span></span> <span data-ttu-id="8bdd9-175">Nainstalujte nejnovější verzi a přidružit ho ke svému předplatnému Azure, najdete v tématu [instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="8bdd9-175">To install the latest version and associate it with your Azure subscription, see [Install and Configure the Azure CLI](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="8bdd9-176">Azure Resource Manager v rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-176">Azure Resource Manager in Azure CLI.</span></span> <span data-ttu-id="8bdd9-177">Přejděte na [pomocí rozhraní příkazového řádku Azure s Resource Managerem](../xplat-cli-azure-resource-manager.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="8bdd9-177">Go to [Using the Azure CLI with the Resource Manager](../xplat-cli-azure-resource-manager.md) for more details.</span></span>

#### <a name="create-a-custom-role"></a><span data-ttu-id="8bdd9-178">Vytvořit vlastní roli</span><span class="sxs-lookup"><span data-stu-id="8bdd9-178">Create a custom role</span></span>

<span data-ttu-id="8bdd9-179">Chcete-li vytvořit vlastní roli, použijte:</span><span class="sxs-lookup"><span data-stu-id="8bdd9-179">To create a custom role, use:</span></span>

    azure role create --inputfile <file path>

#### <a name="assign-the-role-to-a-user"></a><span data-ttu-id="8bdd9-180">Přiřazení role uživatele</span><span class="sxs-lookup"><span data-stu-id="8bdd9-180">Assign the role to a user</span></span>


    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>


## <a name="next-steps"></a><span data-ttu-id="8bdd9-181">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8bdd9-181">Next steps</span></span>

* <span data-ttu-id="8bdd9-182">Úvod k mřížce událostí, naleznete v části [o mřížky událostí](overview.md)</span><span class="sxs-lookup"><span data-stu-id="8bdd9-182">For an introduction to Event Grid, see [About Event Grid](overview.md)</span></span>
