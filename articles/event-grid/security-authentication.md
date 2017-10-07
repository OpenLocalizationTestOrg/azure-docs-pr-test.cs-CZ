---
title: "aaaAzure mřížky událostí zabezpečení a ověřování"
description: "Popisuje mřížky událostí Azure a jeho koncepty."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/14/2017
ms.author: babanisa
ms.openlocfilehash: 74f692c7e3a30856c3a80cbbfe82a26bf4c1c9b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-grid-security-and-authentication"></a><span data-ttu-id="9ea5c-103">Události zabezpečení mřížky a ověřování</span><span class="sxs-lookup"><span data-stu-id="9ea5c-103">Event Grid security and authentication</span></span> 

<span data-ttu-id="9ea5c-104">Azure mřížky událostí má tři typy ověřování:</span><span class="sxs-lookup"><span data-stu-id="9ea5c-104">Azure Event Grid has three types of authentication:</span></span>

* <span data-ttu-id="9ea5c-105">Odběry událostí</span><span class="sxs-lookup"><span data-stu-id="9ea5c-105">Event subscriptions</span></span>
* <span data-ttu-id="9ea5c-106">Publikování události</span><span class="sxs-lookup"><span data-stu-id="9ea5c-106">Event publishing</span></span>
* <span data-ttu-id="9ea5c-107">Doručení události Webhooku</span><span class="sxs-lookup"><span data-stu-id="9ea5c-107">WebHook event delivery</span></span>

## <a name="webhook-event-delivery"></a><span data-ttu-id="9ea5c-108">Události Webhooku doručení</span><span class="sxs-lookup"><span data-stu-id="9ea5c-108">WebHook Event delivery</span></span>

<span data-ttu-id="9ea5c-109">Webhooky jsou jedním z mnoha způsoby tooreceive události v reálném čase z Azure událostí mřížky.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-109">Webhooks are one of many ways tooreceive events in real time from Azure Event Grid.</span></span>

<span data-ttu-id="9ea5c-110">Pokaždé, když je nový připravené toobe událostí doručit, odešle událost mřížky požadavek HTTP s tooyour Webhooku s hello událostí v textu hello.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-110">Every time there is a new event ready toobe delivered, Event Grid sends an HTTP request with tooyour WebHook with hello event in hello body.</span></span>

<span data-ttu-id="9ea5c-111">Při registraci svůj vlastní koncový bod Webhooku s událostí mřížky, se vám pošle požadavek POST s kódem jednoduché ověření v pořadí tooprove koncový bod vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-111">When you register your own WebHook endpoint with Event Grid, it sends you a POST request with a simple validation code in order tooprove endpoint ownership.</span></span> <span data-ttu-id="9ea5c-112">Vaše aplikace musí toorespond podle zobrazování back hello ověřovacího kódu.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-112">Your app needs toorespond by echoing back hello validation code.</span></span> <span data-ttu-id="9ea5c-113">Událost mřížky nebude poskytovat události tooWebHook koncových bodů, které nebyly hello ověření proběhlo úspěšně.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-113">Event Grid will not deliver events tooWebHook endpoints that have not passed hello validation.</span></span>
 
### <a name="validation-details"></a><span data-ttu-id="9ea5c-114">Podrobnosti o ověření:</span><span class="sxs-lookup"><span data-stu-id="9ea5c-114">Validation details:</span></span>

* <span data-ttu-id="9ea5c-115">V době hello aktualizace vytvoření odběru událostí odešle událost mřížky "SubscriptionValidationEvent" události toohello cílového koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-115">At hello time of event subscription creation/update, Event Grid posts a “SubscriptionValidationEvent” event toohello target endpoint.</span></span>
* <span data-ttu-id="9ea5c-116">Hello události obsahuje hodnotu hlavičky "Ověření: typ události".</span><span class="sxs-lookup"><span data-stu-id="9ea5c-116">hello event contains a header value “Event-Type: Validation”.</span></span>
* <span data-ttu-id="9ea5c-117">textu Hello události má hello stejného schématu jako ostatní události událostí mřížky.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-117">hello event body has hello same schema as other Event Grid events.</span></span>
* <span data-ttu-id="9ea5c-118">data události Hello například zahrnuje vlastnost "ValidationCode" s náhodně generované řetězec "ValidationCode: acb13...".</span><span class="sxs-lookup"><span data-stu-id="9ea5c-118">hello event data includes a “ValidationCode” property with a randomly generated string e.g. “ValidationCode: acb13…”.</span></span>

<span data-ttu-id="9ea5c-119">V modulu snap-in vlastnictví koncový bod tooprove pořadí odezvu back hello ověření kódu např "ValidationResponse: acb13...".</span><span class="sxs-lookup"><span data-stu-id="9ea5c-119">In order tooprove endpoint ownership, echo back hello validation code e.g “ValidationResponse: acb13…”.</span></span>

<span data-ttu-id="9ea5c-120">Nakonec je důležité toonote mřížky událostí Azure podporuje pouze HTTPS webhooku koncové body.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-120">Finally, it is important toonote that Azure Event Grid only supports HTTPS webhook endpoints.</span></span>
## <a name="event-subscription"></a><span data-ttu-id="9ea5c-121">Odběru událostí</span><span class="sxs-lookup"><span data-stu-id="9ea5c-121">Event subscription</span></span>

<span data-ttu-id="9ea5c-122">toosubscribe tooan událostí, musíte mít hello **Microsoft.EventGrid/EventSubscriptions/Write** požadováno oprávnění na hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-122">toosubscribe tooan event, you must have hello **Microsoft.EventGrid/EventSubscriptions/Write** permission on hello required resource.</span></span> <span data-ttu-id="9ea5c-123">Je nutné toto oprávnění, protože píšete nové předplatné v oboru hello hello prostředku.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-123">You need this permission because you are writing a new subscription at hello scope of hello resource.</span></span> <span data-ttu-id="9ea5c-124">Hello požadované liší v závislosti na tom, jestli se odběru tooa systému téma nebo vlastních prostředků.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-124">hello required resource differs based on whether you are subscribing tooa system topic or custom topic.</span></span> <span data-ttu-id="9ea5c-125">Oba typy jsou popsané v této části.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-125">Both types are described in this section.</span></span>

### <a name="system-topics-azure-service-publishers"></a><span data-ttu-id="9ea5c-126">Témata týkající se systému (vydavateli služby Azure)</span><span class="sxs-lookup"><span data-stu-id="9ea5c-126">System topics (Azure service publishers)</span></span>

<span data-ttu-id="9ea5c-127">Pro systém témata budete potřebovat oprávnění toowrite nové předplatné událostí v oboru hello hello prostředků publikování hello události.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-127">For system topics, you need permission toowrite a new event subscription at hello scope of hello resource publishing hello event.</span></span> <span data-ttu-id="9ea5c-128">Formát Hello hello prostředku je:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`</span><span class="sxs-lookup"><span data-stu-id="9ea5c-128">hello format of hello resource is: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`</span></span>

<span data-ttu-id="9ea5c-129">Například toosubscribe tooan událostí na účet úložiště s názvem **UCET**, potřebujete oprávnění Microsoft.EventGrid/EventSubscriptions/Write hello na:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`</span><span class="sxs-lookup"><span data-stu-id="9ea5c-129">For example, toosubscribe tooan event on a storage account named **myacct**, you need hello Microsoft.EventGrid/EventSubscriptions/Write permission on: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`</span></span>

### <a name="custom-topics"></a><span data-ttu-id="9ea5c-130">Vlastní témata</span><span class="sxs-lookup"><span data-stu-id="9ea5c-130">Custom topics</span></span>

<span data-ttu-id="9ea5c-131">Pro vlastní témata budete potřebovat oprávnění toowrite nové předplatné událostí v oboru hello hello mřížky události tématu.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-131">For custom topics, you need permission toowrite a new event subscription at hello scope of hello Event Grid topic.</span></span> <span data-ttu-id="9ea5c-132">Formát Hello hello prostředku je:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`</span><span class="sxs-lookup"><span data-stu-id="9ea5c-132">hello format of hello resource is: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`</span></span>

<span data-ttu-id="9ea5c-133">Například toosubscribe tooa vlastní téma s názvem **mytopic**, potřebujete oprávnění Microsoft.EventGrid/EventSubscriptions/Write hello na:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`</span><span class="sxs-lookup"><span data-stu-id="9ea5c-133">For example, toosubscribe tooa custom topic named **mytopic**, you need hello Microsoft.EventGrid/EventSubscriptions/Write permission on: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`</span></span>

## <a name="topic-publishing"></a><span data-ttu-id="9ea5c-134">Téma publikování</span><span class="sxs-lookup"><span data-stu-id="9ea5c-134">Topic publishing</span></span>

<span data-ttu-id="9ea5c-135">Témata týkající se použití sdíleného přístupového podpisu (SAS) nebo ověření pomocí klíče.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-135">Topics use either Shared Access Signature (SAS) or key authentication.</span></span> <span data-ttu-id="9ea5c-136">Doporučujeme, abyste SAS, ale ověření pomocí klíče poskytuje jednoduché programování a je kompatibilní s mnoha existující webhooku vydavatelů.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-136">We recommend SAS, but key authentication provides simple programming, and is compatible with many existing webhook publishers.</span></span> 

<span data-ttu-id="9ea5c-137">Můžete zahrnout hello ověřování hodnota v hlavičce protokolu HTTP hello.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-137">You include hello authentication value in hello HTTP header.</span></span> <span data-ttu-id="9ea5c-138">SAS, použijte **tokenu sas Æg** pro hodnotu hlavičky hello.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-138">For SAS, use **aeg-sas-token** for hello header value.</span></span> <span data-ttu-id="9ea5c-139">Pro ověření pomocí klíče, použít **klíč sas Æg** pro hodnotu hlavičky hello.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-139">For key authentication, use **aeg-sas-key** for hello header value.</span></span>

### <a name="key-authentication"></a><span data-ttu-id="9ea5c-140">Ověření pomocí klíče</span><span class="sxs-lookup"><span data-stu-id="9ea5c-140">Key authentication</span></span>

<span data-ttu-id="9ea5c-141">Ověření pomocí klíče je hello nejjednodušší formu ověření.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-141">Key authentication is hello simplest form of authentication.</span></span> <span data-ttu-id="9ea5c-142">Použijte formát hello:`aeg-sas-key: <your key>`</span><span class="sxs-lookup"><span data-stu-id="9ea5c-142">Use hello format: `aeg-sas-key: <your key>`</span></span>

<span data-ttu-id="9ea5c-143">Například předáte klíč se:</span><span class="sxs-lookup"><span data-stu-id="9ea5c-143">For example, you pass a key with:</span></span> 

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a><span data-ttu-id="9ea5c-144">Tokeny SAS</span><span class="sxs-lookup"><span data-stu-id="9ea5c-144">SAS tokens</span></span>

<span data-ttu-id="9ea5c-145">Tokeny SAS pro událost mřížky zahrnují hello prostředků, čas vypršení platnosti a podpis.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-145">SAS tokens for Event Grid include hello resource, an expiration time, and a signature.</span></span> <span data-ttu-id="9ea5c-146">Hello formát tokenu SAS hello je: `r={resource}&e={expiration}&s={signature}`.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-146">hello format of hello SAS token is: `r={resource}&e={expiration}&s={signature}`.</span></span>

<span data-ttu-id="9ea5c-147">Hello prostředků je hello cesta pro téma toowhich hello odesílání událostí.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-147">hello resource is hello path for hello topic toowhich you are sending events.</span></span> <span data-ttu-id="9ea5c-148">Například je cesta platná prostředku:`https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`</span><span class="sxs-lookup"><span data-stu-id="9ea5c-148">For example, a valid resource path is: `https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`</span></span>

<span data-ttu-id="9ea5c-149">Vygenerování hello podpisu z klíče.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-149">You generate hello signature from a key.</span></span>

<span data-ttu-id="9ea5c-150">Například platná **tokenu sas Æg** hodnota je:</span><span class="sxs-lookup"><span data-stu-id="9ea5c-150">For example, a valid **aeg-sas-token** value is:</span></span>

```http
aeg-sas-token: r=https%3a%2f%2fmytopic.eventgrid.azure.net%2feventGrid%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=a4oNHpRZygINC%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
``` 

<span data-ttu-id="9ea5c-151">Hello následující příklad vytvoří SAS token pro použití s mřížky událostí:</span><span class="sxs-lookup"><span data-stu-id="9ea5c-151">hello following example creates a SAS token for use with Event Grid:</span></span>

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

## <a name="management-access-control"></a><span data-ttu-id="9ea5c-152">Správa řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="9ea5c-152">Management Access Control</span></span>

<span data-ttu-id="9ea5c-153">Azure mřížky událostí umožňuje vám toocontrol hello úroveň přístupu zadané toodifferent uživatelé toodo různé operace správy, jako je například seznam událostí odběry, vytvořit nové a generovat klíče.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-153">Azure Event Grid allows you toocontrol hello level of access given toodifferent users toodo various management operations such as list event subscriptions, create new ones, and generate keys.</span></span> <span data-ttu-id="9ea5c-154">Událost mřížky využívá Azure na základě Role přístupu zkontrolujte (RBAC) pro tento.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-154">Event Grid utilizes Azure's Role Based Access Check (RBAC) for this.</span></span>

### <a name="operation-types"></a><span data-ttu-id="9ea5c-155">Typy operací</span><span class="sxs-lookup"><span data-stu-id="9ea5c-155">Operation types</span></span>

<span data-ttu-id="9ea5c-156">Azure událostí mřížky podporuje hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="9ea5c-156">Azure event grid supports hello following actions:</span></span>

* <span data-ttu-id="9ea5c-157">Microsoft.EventGrid/*/read</span><span class="sxs-lookup"><span data-stu-id="9ea5c-157">Microsoft.EventGrid/*/read</span></span> 
* <span data-ttu-id="9ea5c-158">Microsoft.EventGrid/*/write</span><span class="sxs-lookup"><span data-stu-id="9ea5c-158">Microsoft.EventGrid/*/write</span></span> 
* <span data-ttu-id="9ea5c-159">Microsoft.EventGrid/*/delete</span><span class="sxs-lookup"><span data-stu-id="9ea5c-159">Microsoft.EventGrid/*/delete</span></span> 
* <span data-ttu-id="9ea5c-160">Microsoft.EventGrid/eventSubscriptions/getFullUrl/action</span><span class="sxs-lookup"><span data-stu-id="9ea5c-160">Microsoft.EventGrid/eventSubscriptions/getFullUrl/action</span></span> 
* <span data-ttu-id="9ea5c-161">Microsoft.EventGrid/topics/listKeys/action</span><span class="sxs-lookup"><span data-stu-id="9ea5c-161">Microsoft.EventGrid/topics/listKeys/action</span></span> 
* <span data-ttu-id="9ea5c-162">Microsoft.EventGrid/topics/regenerateKey/action</span><span class="sxs-lookup"><span data-stu-id="9ea5c-162">Microsoft.EventGrid/topics/regenerateKey/action</span></span>

<span data-ttu-id="9ea5c-163">Hello poslední tři operace návratový potenciálně tajné informace, které získá filtrované mimo normální operace čtení.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-163">hello last three operations return potentially secret information which gets filtered out of normal read operations.</span></span> <span data-ttu-id="9ea5c-164">Je vhodné pro vás toorestrict přístup toothese operace.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-164">It is best practice for you toorestrict access toothese operations.</span></span> <span data-ttu-id="9ea5c-165">Můžete vytvořit vlastní role pomocí [prostředí Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md), [rozhraní příkazového řádku Azure (CLI)](../active-directory/role-based-access-control-manage-access-azure-cli.md)a hello [REST API](../active-directory/role-based-access-control-manage-access-rest.md).</span><span class="sxs-lookup"><span data-stu-id="9ea5c-165">Custom roles can be created using [Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md), [Azure Command-Line Interface (CLI)](../active-directory/role-based-access-control-manage-access-azure-cli.md), and hello [REST API](../active-directory/role-based-access-control-manage-access-rest.md).</span></span>

### <a name="enforcing-role-based-access-check-rbac"></a><span data-ttu-id="9ea5c-166">Vynucování Role na základě kontroly přístupu (RBAC)</span><span class="sxs-lookup"><span data-stu-id="9ea5c-166">Enforcing Role Based Access Check (RBAC)</span></span>

<span data-ttu-id="9ea5c-167">Použijte následující postup tooenforce RBAC pro různé uživatele hello:</span><span class="sxs-lookup"><span data-stu-id="9ea5c-167">Use hello following steps tooenforce RBAC for different users:</span></span>

#### <a name="create-a-custom-role-definition-file-json"></a><span data-ttu-id="9ea5c-168">Vytvořte soubor definice vlastních rolí (.json)</span><span class="sxs-lookup"><span data-stu-id="9ea5c-168">Create a custom role definition file (.json)</span></span>

<span data-ttu-id="9ea5c-169">Hello následují definice rolí událostí mřížky ukázkové, které uživatelům umožňují tooperform různé sady akcí.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-169">hello following are sample Event Grid role definitions which allow users tooperform different sets of actions.</span></span>

<span data-ttu-id="9ea5c-170">**EventGridReadOnlyRole.json**: Povolit jenom pro čtení jenom operace.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-170">**EventGridReadOnlyRole.json**: Only allow read only operations.</span></span>
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

<span data-ttu-id="9ea5c-171">**EventGridNoDeleteListKeysRole.json**: Povolit akce s omezeným přístupem po ale zakáže akce odstranění.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-171">**EventGridNoDeleteListKeysRole.json**: Allow restricted post actions but disallow delete actions.</span></span>
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
 
<span data-ttu-id="9ea5c-172">**EventGridContributorRole.json**: umožňuje všechny akce mřížky událostí.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-172">**EventGridContributorRole.json**: Allows all event grid actions.</span></span>  
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

#### <a name="install-and-login-tooazure-cli"></a><span data-ttu-id="9ea5c-173">Instalace a přihlášení tooAzure rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="9ea5c-173">Install and login tooAzure CLI</span></span>

* <span data-ttu-id="9ea5c-174">Azure CLI verze 0.8.8 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-174">Azure CLI version 0.8.8 or later.</span></span> <span data-ttu-id="9ea5c-175">tooinstall hello nejnovější verzi a přidružit ho ke svému předplatnému Azure, najdete v části [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="9ea5c-175">tooinstall hello latest version and associate it with your Azure subscription, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="9ea5c-176">Azure Resource Manager v rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-176">Azure Resource Manager in Azure CLI.</span></span> <span data-ttu-id="9ea5c-177">Přejděte příliš[hello pomocí rozhraní příkazového řádku Azure s hello Resource Manager](../xplat-cli-azure-resource-manager.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="9ea5c-177">Go too[Using hello Azure CLI with hello Resource Manager](../xplat-cli-azure-resource-manager.md) for more details.</span></span>

#### <a name="create-a-custom-role"></a><span data-ttu-id="9ea5c-178">Vytvořit vlastní roli</span><span class="sxs-lookup"><span data-stu-id="9ea5c-178">Create a custom role</span></span>

<span data-ttu-id="9ea5c-179">toocreate vlastní roli, použijte:</span><span class="sxs-lookup"><span data-stu-id="9ea5c-179">toocreate a custom role, use:</span></span>

    azure role create --inputfile <file path>

#### <a name="assign-hello-role-tooa-user"></a><span data-ttu-id="9ea5c-180">Přiřadit uživatele tooa role hello</span><span class="sxs-lookup"><span data-stu-id="9ea5c-180">Assign hello role tooa user</span></span>


    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>


## <a name="next-steps"></a><span data-ttu-id="9ea5c-181">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9ea5c-181">Next steps</span></span>

* <span data-ttu-id="9ea5c-182">TooEvent Úvod mřížky, najdete v části [o mřížky událostí](overview.md)</span><span class="sxs-lookup"><span data-stu-id="9ea5c-182">For an introduction tooEvent Grid, see [About Event Grid](overview.md)</span></span>
