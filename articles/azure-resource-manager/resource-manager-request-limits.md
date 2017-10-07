---
title: "omezení žádostí o aaaAzure Resource Manager | Microsoft Docs"
description: "Popisuje, jak toouse omezení pomocí Azure Resource Manageru požadavky při dosáhli limity předplatného."
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
ms.openlocfilehash: ea274f3145af36aac753730e1f280d8a8b59c3bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="throttling-resource-manager-requests"></a><span data-ttu-id="22c4f-103">Omezení žádostí Resource Manager</span><span class="sxs-lookup"><span data-stu-id="22c4f-103">Throttling Resource Manager requests</span></span>
<span data-ttu-id="22c4f-104">Pro každé předplatné a klienta Resource Manager omezení čtení too15 požadavky, 000 za hodinu a zápisu too1 požadavky, 200 za hodinu.</span><span class="sxs-lookup"><span data-stu-id="22c4f-104">For each subscription and tenant, Resource Manager limits read requests too15,000 per hour and write requests too1,200 per hour.</span></span> <span data-ttu-id="22c4f-105">Tyto limity platí instanci Azure Resource Manager tooeach; existuje více instancí v každé oblasti Azure a Azure Resource Manager je nasazené tooall Azure oblasti.</span><span class="sxs-lookup"><span data-stu-id="22c4f-105">These limits apply tooeach Azure Resource Manager instance; there are multiple instances in every Azure region, and Azure Resource Manager is deployed tooall Azure regions.</span></span>  <span data-ttu-id="22c4f-106">Tím, že v praxi, jsou efektivně mnohem vyšší než výše uvedené omezení, jako uživatel požadavky jsou obecně obsluhovány pomocí mnoha různými instancemi.</span><span class="sxs-lookup"><span data-stu-id="22c4f-106">So, in practice, limits are effectively much higher than those listed above, as user requests are generally serviced by many different instances.</span></span>

<span data-ttu-id="22c4f-107">Pokud vaše aplikace nebo skriptu dosáhne tyto limity, je nutné toothrottle své žádosti.</span><span class="sxs-lookup"><span data-stu-id="22c4f-107">If your application or script reaches these limits, you need toothrottle your requests.</span></span> <span data-ttu-id="22c4f-108">Toto téma ukazuje, jak toodetermine hello zbývající požadavky, je nutné před dosažením limitu hello a jak toorespond, když jste dosáhli limitu hello.</span><span class="sxs-lookup"><span data-stu-id="22c4f-108">This topic shows you how toodetermine hello remaining requests you have before reaching hello limit, and how toorespond when you have reached hello limit.</span></span>

<span data-ttu-id="22c4f-109">Po dosažení limitu hello obdržíte kód stavu hello HTTP **429 příliš mnoho požadavků**.</span><span class="sxs-lookup"><span data-stu-id="22c4f-109">When you reach hello limit, you receive hello HTTP status code **429 Too many requests**.</span></span>

<span data-ttu-id="22c4f-110">Hello počet požadavků, které je vymezená tooeither vašeho předplatného nebo tenanta.</span><span class="sxs-lookup"><span data-stu-id="22c4f-110">hello number of requests is scoped tooeither your subscription or your tenant.</span></span> <span data-ttu-id="22c4f-111">Pokud máte více souběžných zasílání požadavků v rámci vašeho předplatného, hello požadavky z těchto aplikací se aplikace přidávají společně toodetermine hello počet zbývajících požadavků.</span><span class="sxs-lookup"><span data-stu-id="22c4f-111">If you have multiple, concurrent applications making requests in your subscription, hello requests from those applications are added together toodetermine hello number of remaining requests.</span></span>

<span data-ttu-id="22c4f-112">Předplatné obor požadavky jsou ty, které zahrnují hello předávání svoje id předplatného, jako je například načítání hello skupiny prostředků v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="22c4f-112">Subscription scoped requests are ones hello involve passing your subscription id, such as retrieving hello resource groups in your subscription.</span></span> <span data-ttu-id="22c4f-113">Požadavky klienta obor nezahrnují svoje id předplatného, jako je například načítání platná umístění Azure.</span><span class="sxs-lookup"><span data-stu-id="22c4f-113">Tenant scoped requests do not include your subscription id, such as retrieving valid Azure locations.</span></span>

## <a name="remaining-requests"></a><span data-ttu-id="22c4f-114">Zbývající požadavků</span><span class="sxs-lookup"><span data-stu-id="22c4f-114">Remaining requests</span></span>
<span data-ttu-id="22c4f-115">Hello počet zbývajících požadavků, které můžete zjistit tak, že prověří hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="22c4f-115">You can determine hello number of remaining requests by examining response headers.</span></span> <span data-ttu-id="22c4f-116">Každý požadavek obsahuje hodnoty pro počet hello zbývající ke čtení a zápisu požadavků.</span><span class="sxs-lookup"><span data-stu-id="22c4f-116">Each request includes values for hello number of remaining read and write requests.</span></span> <span data-ttu-id="22c4f-117">Hello následující tabulka popisuje hello hlavičky odpovědi, které můžete zkontrolovat pro tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="22c4f-117">hello following table describes hello response headers you can examine for those values:</span></span>

| <span data-ttu-id="22c4f-118">Hlavička odpovědi</span><span class="sxs-lookup"><span data-stu-id="22c4f-118">Response header</span></span> | <span data-ttu-id="22c4f-119">Popis</span><span class="sxs-lookup"><span data-stu-id="22c4f-119">Description</span></span> |
| --- | --- |
| <span data-ttu-id="22c4f-120">x-MS-ratelimit-Remaining-Subscription-reads</span><span class="sxs-lookup"><span data-stu-id="22c4f-120">x-ms-ratelimit-remaining-subscription-reads</span></span> |<span data-ttu-id="22c4f-121">Předplatné obor čte zbývající</span><span class="sxs-lookup"><span data-stu-id="22c4f-121">Subscription scoped reads remaining</span></span> |
| <span data-ttu-id="22c4f-122">x-MS-ratelimit-Remaining-Subscription-Writes</span><span class="sxs-lookup"><span data-stu-id="22c4f-122">x-ms-ratelimit-remaining-subscription-writes</span></span> |<span data-ttu-id="22c4f-123">Předplatné obor zapíše zbývající</span><span class="sxs-lookup"><span data-stu-id="22c4f-123">Subscription scoped writes remaining</span></span> |
| <span data-ttu-id="22c4f-124">x-MS-ratelimit-Remaining-tenant-reads</span><span class="sxs-lookup"><span data-stu-id="22c4f-124">x-ms-ratelimit-remaining-tenant-reads</span></span> |<span data-ttu-id="22c4f-125">Obor klienta čte zbývající</span><span class="sxs-lookup"><span data-stu-id="22c4f-125">Tenant scoped reads remaining</span></span> |
| <span data-ttu-id="22c4f-126">x-MS-ratelimit-Remaining-tenant-Writes</span><span class="sxs-lookup"><span data-stu-id="22c4f-126">x-ms-ratelimit-remaining-tenant-writes</span></span> |<span data-ttu-id="22c4f-127">Obor klienta zapíše zbývající</span><span class="sxs-lookup"><span data-stu-id="22c4f-127">Tenant scoped writes remaining</span></span> |
| <span data-ttu-id="22c4f-128">x-MS-ratelimit-Remaining-Subscription-Resource-Requests</span><span class="sxs-lookup"><span data-stu-id="22c4f-128">x-ms-ratelimit-remaining-subscription-resource-requests</span></span> |<span data-ttu-id="22c4f-129">Předplatné obor zbývající žádosti typu prostředku.</span><span class="sxs-lookup"><span data-stu-id="22c4f-129">Subscription scoped resource type requests remaining.</span></span><br /><br /><span data-ttu-id="22c4f-130">Hodnotu této hlavičky je vrácena pouze v případě služby má přepsat výchozí limit hello.</span><span class="sxs-lookup"><span data-stu-id="22c4f-130">This header value is only returned if a service has overridden hello default limit.</span></span> <span data-ttu-id="22c4f-131">Resource Manager přidá tato hodnota místo hello předplatné čtení nebo zápisu.</span><span class="sxs-lookup"><span data-stu-id="22c4f-131">Resource Manager adds this value instead of hello subscription reads or writes.</span></span> |
| <span data-ttu-id="22c4f-132">x-MS-ratelimit-Remaining-Subscription-Resource-entities-Read</span><span class="sxs-lookup"><span data-stu-id="22c4f-132">x-ms-ratelimit-remaining-subscription-resource-entities-read</span></span> |<span data-ttu-id="22c4f-133">Předplatné obor zbývající žádosti pro kolekci prostředků typu.</span><span class="sxs-lookup"><span data-stu-id="22c4f-133">Subscription scoped resource type collection requests remaining.</span></span><br /><br /><span data-ttu-id="22c4f-134">Hodnotu této hlavičky je vrácena pouze v případě služby má přepsat výchozí limit hello.</span><span class="sxs-lookup"><span data-stu-id="22c4f-134">This header value is only returned if a service has overridden hello default limit.</span></span> <span data-ttu-id="22c4f-135">Tuto hodnotu poskytuje hello počet zbývajících požadavků kolekce (seznam prostředky).</span><span class="sxs-lookup"><span data-stu-id="22c4f-135">This value provides hello number of remaining collection requests (list resources).</span></span> |
| <span data-ttu-id="22c4f-136">x-MS-ratelimit-Remaining-tenant-Resource-Requests</span><span class="sxs-lookup"><span data-stu-id="22c4f-136">x-ms-ratelimit-remaining-tenant-resource-requests</span></span> |<span data-ttu-id="22c4f-137">Klienta obor zbývající žádosti typu prostředku.</span><span class="sxs-lookup"><span data-stu-id="22c4f-137">Tenant scoped resource type requests remaining.</span></span><br /><br /><span data-ttu-id="22c4f-138">Tuto hlavičku přidat jenom pro požadavky na úrovni klienta a pouze v případě služby má přepsat výchozí limit hello.</span><span class="sxs-lookup"><span data-stu-id="22c4f-138">This header is only added for requests at tenant level, and only if a service has overridden hello default limit.</span></span> <span data-ttu-id="22c4f-139">Resource Manager přidá tato hodnota místo hello klienta čtení nebo zápisu.</span><span class="sxs-lookup"><span data-stu-id="22c4f-139">Resource Manager adds this value instead of hello tenant reads or writes.</span></span> |
| <span data-ttu-id="22c4f-140">x-MS-ratelimit-Remaining-tenant-Resource-entities-Read</span><span class="sxs-lookup"><span data-stu-id="22c4f-140">x-ms-ratelimit-remaining-tenant-resource-entities-read</span></span> |<span data-ttu-id="22c4f-141">Klienta obor zbývající žádosti pro kolekci prostředků typu.</span><span class="sxs-lookup"><span data-stu-id="22c4f-141">Tenant scoped resource type collection requests remaining.</span></span><br /><br /><span data-ttu-id="22c4f-142">Tuto hlavičku přidat jenom pro požadavky na úrovni klienta a pouze v případě služby má přepsat výchozí limit hello.</span><span class="sxs-lookup"><span data-stu-id="22c4f-142">This header is only added for requests at tenant level, and only if a service has overridden hello default limit.</span></span> |

## <a name="retrieving-hello-header-values"></a><span data-ttu-id="22c4f-143">Načítání hodnoty hlavičky hello</span><span class="sxs-lookup"><span data-stu-id="22c4f-143">Retrieving hello header values</span></span>
<span data-ttu-id="22c4f-144">Načítání tyto hodnoty hlavičky v kódu nebo skriptu je nejsou jiné než načítání žádnou hodnotu hlavičky.</span><span class="sxs-lookup"><span data-stu-id="22c4f-144">Retrieving these header values in your code or script is no different than retrieving any header value.</span></span> 

<span data-ttu-id="22c4f-145">Například v **C#**, načtení hello hodnota hlavičky ze **HttpWebResponse** objekt s názvem **odpovědi** s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="22c4f-145">For example, in **C#**, you retrieve hello header value from an **HttpWebResponse** object named **response** with hello following code:</span></span>

```cs
response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)
```

<span data-ttu-id="22c4f-146">V **prostředí PowerShell**, načíst hodnotu hlavičky hello z operace Invoke-WebRequest.</span><span class="sxs-lookup"><span data-stu-id="22c4f-146">In **PowerShell**, you retrieve hello header value from an Invoke-WebRequest operation.</span></span>

```powershell
$r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
$r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
```

<span data-ttu-id="22c4f-147">Nebo, pokud chcete zbývající hello toosee požadavky pro ladění, můžete zadat hello **– ladění** parametr na vaše **prostředí PowerShell** rutiny.</span><span class="sxs-lookup"><span data-stu-id="22c4f-147">Or, if want toosee hello remaining requests for debugging, you can provide hello **-Debug** parameter on your **PowerShell** cmdlet.</span></span>

```powershell
Get-AzureRmResourceGroup -Debug
```

<span data-ttu-id="22c4f-148">Která vrací mnoho hodnot, včetně hello následující hodnota odezvy:</span><span class="sxs-lookup"><span data-stu-id="22c4f-148">Which returns many values, including hello following response value:</span></span>

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

<span data-ttu-id="22c4f-149">V **rozhraní příkazového řádku Azure**, načíst hodnotu hlavičky hello pomocí hello podrobnější možnost.</span><span class="sxs-lookup"><span data-stu-id="22c4f-149">In **Azure CLI**, you retrieve hello header value by using hello more verbose option.</span></span>

```azurecli
azure group list -vv --json
```

<span data-ttu-id="22c4f-150">Která vrací mnoho hodnot, včetně hello následujících objektů:</span><span class="sxs-lookup"><span data-stu-id="22c4f-150">Which returns many values, including hello following object:</span></span>

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

## <a name="waiting-before-sending-next-request"></a><span data-ttu-id="22c4f-151">Čekání před odesláním další požadavek</span><span class="sxs-lookup"><span data-stu-id="22c4f-151">Waiting before sending next request</span></span>
<span data-ttu-id="22c4f-152">Po dosažení limitu požadavků hello Resource Manager vrátí hello **429** stavový kód HTTP a **opakovat po** hodnota v hlavičce hello.</span><span class="sxs-lookup"><span data-stu-id="22c4f-152">When you reach hello request limit, Resource Manager returns hello **429** HTTP status code and a **Retry-After** value in hello header.</span></span> <span data-ttu-id="22c4f-153">Hello **opakovat po** hodnota určuje hello počet sekund, které má aplikace čekat (nebo spánku) před odesláním další požadavek hello.</span><span class="sxs-lookup"><span data-stu-id="22c4f-153">hello **Retry-After** value specifies hello number of seconds your application should wait (or sleep) before sending hello next request.</span></span> <span data-ttu-id="22c4f-154">Pokud odešlete žádost předtím, než dojde k uplynutí hodnota pro opakovaný pokus hello, váš požadavek nebude zpracován a se vrátí novou hodnotu opakování.</span><span class="sxs-lookup"><span data-stu-id="22c4f-154">If you send a request before hello retry value has elapsed, your request is not processed and a new retry value is returned.</span></span>

## <a name="next-steps"></a><span data-ttu-id="22c4f-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="22c4f-155">Next steps</span></span>

* <span data-ttu-id="22c4f-156">Další informace o omezení a kvóty najdete v tématu [předplatného Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="22c4f-156">For more information about limits and quotas, see [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md).</span></span>
* <span data-ttu-id="22c4f-157">toolearn o zpracování asynchronní REST žádostí, najdete v části [sledovat asynchronní operace Azure](resource-manager-async-operations.md).</span><span class="sxs-lookup"><span data-stu-id="22c4f-157">toolearn about handling asynchronous REST requests, see [Track asynchronous Azure operations](resource-manager-async-operations.md).</span></span>
