---
title: "Omezení žádostí o Azure Resource Manager | Microsoft Docs"
description: "Popisuje způsob použití omezení šířky pásma s Azure Resource Manager požadavky při dosáhli limity předplatného."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: e1047233-b8e4-4232-8919-3268d93a3824
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/11/2017
ms.author: tomfitz
ms.openlocfilehash: 6d7eeaf460674c3ab98425a5412ffa465b9ffd1d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="throttling-resource-manager-requests"></a><span data-ttu-id="1a437-103">Omezení žádostí Resource Manager</span><span class="sxs-lookup"><span data-stu-id="1a437-103">Throttling Resource Manager requests</span></span>
<span data-ttu-id="1a437-104">Omezení správce prostředků pro každé předplatné a klienta, přečtěte si požadavky na 15 000 za hodinu a požadavků na zápis na 1 200 za hodinu.</span><span class="sxs-lookup"><span data-stu-id="1a437-104">For each subscription and tenant, Resource Manager limits read requests to 15,000 per hour and write requests to 1,200 per hour.</span></span> <span data-ttu-id="1a437-105">Tato omezení se použijí na každou instanci Azure Resource Manager; existuje více instancí v každé oblasti Azure a Azure Resource Manager je nasazen na všechny oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="1a437-105">These limits apply to each Azure Resource Manager instance; there are multiple instances in every Azure region, and Azure Resource Manager is deployed to all Azure regions.</span></span>  <span data-ttu-id="1a437-106">Tím, že v praxi, jsou efektivně mnohem vyšší než výše uvedené omezení, jako uživatel požadavky jsou obecně obsluhovány pomocí mnoha různými instancemi.</span><span class="sxs-lookup"><span data-stu-id="1a437-106">So, in practice, limits are effectively much higher than those listed above, as user requests are generally serviced by many different instances.</span></span>

<span data-ttu-id="1a437-107">Pokud vaše aplikace nebo skriptu dosáhne tyto limity, budete muset omezení své žádosti.</span><span class="sxs-lookup"><span data-stu-id="1a437-107">If your application or script reaches these limits, you need to throttle your requests.</span></span> <span data-ttu-id="1a437-108">Toto téma ukazuje, jak určit zbývající požadavků, které máte před dosažením limitu a jak reagovat, když jste dosáhli limitu.</span><span class="sxs-lookup"><span data-stu-id="1a437-108">This topic shows you how to determine the remaining requests you have before reaching the limit, and how to respond when you have reached the limit.</span></span>

<span data-ttu-id="1a437-109">Když dostanete tento limit, zobrazí se stavový kód HTTP **429 příliš mnoho požadavků**.</span><span class="sxs-lookup"><span data-stu-id="1a437-109">When you reach the limit, you receive the HTTP status code **429 Too many requests**.</span></span>

<span data-ttu-id="1a437-110">Počet požadavků, které je vymezen na vašeho předplatného nebo tenanta.</span><span class="sxs-lookup"><span data-stu-id="1a437-110">The number of requests is scoped to either your subscription or your tenant.</span></span> <span data-ttu-id="1a437-111">Pokud máte více souběžných zasílání požadavků v rámci vašeho předplatného, požadavky z těchto aplikací se aplikace přidávají společně k určení počet zbývajících požadavků.</span><span class="sxs-lookup"><span data-stu-id="1a437-111">If you have multiple, concurrent applications making requests in your subscription, the requests from those applications are added together to determine the number of remaining requests.</span></span>

<span data-ttu-id="1a437-112">Předplatné obor požadavky jsou ty, které zahrnují předávání svoje id předplatného, jako je například načítání prostředku skupiny v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="1a437-112">Subscription scoped requests are ones the involve passing your subscription id, such as retrieving the resource groups in your subscription.</span></span> <span data-ttu-id="1a437-113">Požadavky klienta obor nezahrnují svoje id předplatného, jako je například načítání platná umístění Azure.</span><span class="sxs-lookup"><span data-stu-id="1a437-113">Tenant scoped requests do not include your subscription id, such as retrieving valid Azure locations.</span></span>

## <a name="remaining-requests"></a><span data-ttu-id="1a437-114">Zbývající požadavků</span><span class="sxs-lookup"><span data-stu-id="1a437-114">Remaining requests</span></span>
<span data-ttu-id="1a437-115">Počet zbývajících požadavků, které můžete zjistit tak, že prověří hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="1a437-115">You can determine the number of remaining requests by examining response headers.</span></span> <span data-ttu-id="1a437-116">Každý požadavek obsahuje hodnoty pro číslo zbývající čtení a zápisu požadavků.</span><span class="sxs-lookup"><span data-stu-id="1a437-116">Each request includes values for the number of remaining read and write requests.</span></span> <span data-ttu-id="1a437-117">Následující tabulka popisuje hlavičky odpovědi, které můžete zkontrolovat pro tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="1a437-117">The following table describes the response headers you can examine for those values:</span></span>

| <span data-ttu-id="1a437-118">Hlavička odpovědi</span><span class="sxs-lookup"><span data-stu-id="1a437-118">Response header</span></span> | <span data-ttu-id="1a437-119">Popis</span><span class="sxs-lookup"><span data-stu-id="1a437-119">Description</span></span> |
| --- | --- |
| <span data-ttu-id="1a437-120">x-MS-ratelimit-Remaining-Subscription-reads</span><span class="sxs-lookup"><span data-stu-id="1a437-120">x-ms-ratelimit-remaining-subscription-reads</span></span> |<span data-ttu-id="1a437-121">Předplatné obor čte zbývající</span><span class="sxs-lookup"><span data-stu-id="1a437-121">Subscription scoped reads remaining</span></span> |
| <span data-ttu-id="1a437-122">x-MS-ratelimit-Remaining-Subscription-Writes</span><span class="sxs-lookup"><span data-stu-id="1a437-122">x-ms-ratelimit-remaining-subscription-writes</span></span> |<span data-ttu-id="1a437-123">Předplatné obor zapíše zbývající</span><span class="sxs-lookup"><span data-stu-id="1a437-123">Subscription scoped writes remaining</span></span> |
| <span data-ttu-id="1a437-124">x-MS-ratelimit-Remaining-tenant-reads</span><span class="sxs-lookup"><span data-stu-id="1a437-124">x-ms-ratelimit-remaining-tenant-reads</span></span> |<span data-ttu-id="1a437-125">Obor klienta čte zbývající</span><span class="sxs-lookup"><span data-stu-id="1a437-125">Tenant scoped reads remaining</span></span> |
| <span data-ttu-id="1a437-126">x-MS-ratelimit-Remaining-tenant-Writes</span><span class="sxs-lookup"><span data-stu-id="1a437-126">x-ms-ratelimit-remaining-tenant-writes</span></span> |<span data-ttu-id="1a437-127">Obor klienta zapíše zbývající</span><span class="sxs-lookup"><span data-stu-id="1a437-127">Tenant scoped writes remaining</span></span> |
| <span data-ttu-id="1a437-128">x-MS-ratelimit-Remaining-Subscription-Resource-Requests</span><span class="sxs-lookup"><span data-stu-id="1a437-128">x-ms-ratelimit-remaining-subscription-resource-requests</span></span> |<span data-ttu-id="1a437-129">Předplatné obor zbývající žádosti typu prostředku.</span><span class="sxs-lookup"><span data-stu-id="1a437-129">Subscription scoped resource type requests remaining.</span></span><br /><br /><span data-ttu-id="1a437-130">Hodnotu této hlavičky je vrácena pouze v případě služby má přepsat výchozí limit.</span><span class="sxs-lookup"><span data-stu-id="1a437-130">This header value is only returned if a service has overridden the default limit.</span></span> <span data-ttu-id="1a437-131">Resource Manager přidá tato hodnota místo předplatné čtení nebo zápisu.</span><span class="sxs-lookup"><span data-stu-id="1a437-131">Resource Manager adds this value instead of the subscription reads or writes.</span></span> |
| <span data-ttu-id="1a437-132">x-MS-ratelimit-Remaining-Subscription-Resource-entities-Read</span><span class="sxs-lookup"><span data-stu-id="1a437-132">x-ms-ratelimit-remaining-subscription-resource-entities-read</span></span> |<span data-ttu-id="1a437-133">Předplatné obor zbývající žádosti pro kolekci prostředků typu.</span><span class="sxs-lookup"><span data-stu-id="1a437-133">Subscription scoped resource type collection requests remaining.</span></span><br /><br /><span data-ttu-id="1a437-134">Hodnotu této hlavičky je vrácena pouze v případě služby má přepsat výchozí limit.</span><span class="sxs-lookup"><span data-stu-id="1a437-134">This header value is only returned if a service has overridden the default limit.</span></span> <span data-ttu-id="1a437-135">Tuto hodnotu poskytuje počet zbývajících požadavků kolekce (seznam prostředky).</span><span class="sxs-lookup"><span data-stu-id="1a437-135">This value provides the number of remaining collection requests (list resources).</span></span> |
| <span data-ttu-id="1a437-136">x-MS-ratelimit-Remaining-tenant-Resource-Requests</span><span class="sxs-lookup"><span data-stu-id="1a437-136">x-ms-ratelimit-remaining-tenant-resource-requests</span></span> |<span data-ttu-id="1a437-137">Klienta obor zbývající žádosti typu prostředku.</span><span class="sxs-lookup"><span data-stu-id="1a437-137">Tenant scoped resource type requests remaining.</span></span><br /><br /><span data-ttu-id="1a437-138">Tuto hlavičku přidat jenom pro požadavky na úrovni klienta a pouze v případě služby má přepsat výchozí limit.</span><span class="sxs-lookup"><span data-stu-id="1a437-138">This header is only added for requests at tenant level, and only if a service has overridden the default limit.</span></span> <span data-ttu-id="1a437-139">Resource Manager přidá tato hodnota místo klienta čtení nebo zápisu.</span><span class="sxs-lookup"><span data-stu-id="1a437-139">Resource Manager adds this value instead of the tenant reads or writes.</span></span> |
| <span data-ttu-id="1a437-140">x-MS-ratelimit-Remaining-tenant-Resource-entities-Read</span><span class="sxs-lookup"><span data-stu-id="1a437-140">x-ms-ratelimit-remaining-tenant-resource-entities-read</span></span> |<span data-ttu-id="1a437-141">Klienta obor zbývající žádosti pro kolekci prostředků typu.</span><span class="sxs-lookup"><span data-stu-id="1a437-141">Tenant scoped resource type collection requests remaining.</span></span><br /><br /><span data-ttu-id="1a437-142">Tuto hlavičku přidat jenom pro požadavky na úrovni klienta a pouze v případě služby má přepsat výchozí limit.</span><span class="sxs-lookup"><span data-stu-id="1a437-142">This header is only added for requests at tenant level, and only if a service has overridden the default limit.</span></span> |

## <a name="retrieving-the-header-values"></a><span data-ttu-id="1a437-143">Načítání hodnoty hlavičky</span><span class="sxs-lookup"><span data-stu-id="1a437-143">Retrieving the header values</span></span>
<span data-ttu-id="1a437-144">Načítání tyto hodnoty hlavičky v kódu nebo skriptu je nejsou jiné než načítání žádnou hodnotu hlavičky.</span><span class="sxs-lookup"><span data-stu-id="1a437-144">Retrieving these header values in your code or script is no different than retrieving any header value.</span></span> 

<span data-ttu-id="1a437-145">Například v **C#**, načtení hodnota hlavičky ze **HttpWebResponse** objekt s názvem **odpovědi** následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="1a437-145">For example, in **C#**, you retrieve the header value from an **HttpWebResponse** object named **response** with the following code:</span></span>

```cs
response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)
```

<span data-ttu-id="1a437-146">V **prostředí PowerShell**, načtení hodnota hlavičky z operace Invoke-WebRequest.</span><span class="sxs-lookup"><span data-stu-id="1a437-146">In **PowerShell**, you retrieve the header value from an Invoke-WebRequest operation.</span></span>

```powershell
$r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
$r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
```

<span data-ttu-id="1a437-147">Nebo, pokud chcete zobrazit zbývající požadavky pro ladění, můžete zadat **– ladění** parametr na vaše **prostředí PowerShell** rutiny.</span><span class="sxs-lookup"><span data-stu-id="1a437-147">Or, if want to see the remaining requests for debugging, you can provide the **-Debug** parameter on your **PowerShell** cmdlet.</span></span>

```powershell
Get-AzureRmResourceGroup -Debug
```

<span data-ttu-id="1a437-148">Která vrací mnoho hodnot, včetně následující hodnotu odpovědi:</span><span class="sxs-lookup"><span data-stu-id="1a437-148">Which returns many values, including the following response value:</span></span>

```powershell
...
DEBUG: ============================ HTTP RESPONSE ============================

Status Code:
OK

Headers:
Pragma                        : no-cache
x-ms-ratelimit-remaining-subscription-reads: 14999
...
```

<span data-ttu-id="1a437-149">V **rozhraní příkazového řádku Azure**, je hodnota hlavičky načíst pomocí podrobnější možnosti.</span><span class="sxs-lookup"><span data-stu-id="1a437-149">In **Azure CLI**, you retrieve the header value by using the more verbose option.</span></span>

```azurecli
azure group list -vv --json
```

<span data-ttu-id="1a437-150">Která vrací mnoho hodnot, včetně následujících objektů:</span><span class="sxs-lookup"><span data-stu-id="1a437-150">Which returns many values, including the following object:</span></span>

```azurecli
...
silly: returnObject
{
  "statusCode": 200,
  "header": {
    "cache-control": "no-cache",
    "pragma": "no-cache",
    "content-type": "application/json; charset=utf-8",
    "expires": "-1",
    "x-ms-ratelimit-remaining-subscription-reads": "14998",
    ...
```

## <a name="waiting-before-sending-next-request"></a><span data-ttu-id="1a437-151">Čekání před odesláním další požadavek</span><span class="sxs-lookup"><span data-stu-id="1a437-151">Waiting before sending next request</span></span>
<span data-ttu-id="1a437-152">Po dosažení limitu požadavků Resource Manager vrátí **429** stavový kód HTTP a **opakovat po** hodnota v hlavičce.</span><span class="sxs-lookup"><span data-stu-id="1a437-152">When you reach the request limit, Resource Manager returns the **429** HTTP status code and a **Retry-After** value in the header.</span></span> <span data-ttu-id="1a437-153">**Opakovat po** hodnota určuje počet sekund, které má aplikace čekat (nebo spánku) před odesláním další požadavek.</span><span class="sxs-lookup"><span data-stu-id="1a437-153">The **Retry-After** value specifies the number of seconds your application should wait (or sleep) before sending the next request.</span></span> <span data-ttu-id="1a437-154">Pokud odešlete žádost předtím, než dojde k uplynutí hodnoty opakování, váš požadavek nebude zpracován a se vrátí novou hodnotu opakování.</span><span class="sxs-lookup"><span data-stu-id="1a437-154">If you send a request before the retry value has elapsed, your request is not processed and a new retry value is returned.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1a437-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1a437-155">Next steps</span></span>

* <span data-ttu-id="1a437-156">Další informace o omezení a kvóty najdete v tématu [předplatného Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="1a437-156">For more information about limits and quotas, see [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md).</span></span>
* <span data-ttu-id="1a437-157">Další informace o zpracování asynchronní REST požadavky najdete v tématu [sledovat asynchronní operace Azure](resource-manager-async-operations.md).</span><span class="sxs-lookup"><span data-stu-id="1a437-157">To learn about handling asynchronous REST requests, see [Track asynchronous Azure operations](resource-manager-async-operations.md).</span></span>
