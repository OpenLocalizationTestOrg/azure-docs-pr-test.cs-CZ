---
title: "asynchronní operace aaaAzure | Microsoft Docs"
description: "Popisuje, jak tootrack asynchronních operací v Azure."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/11/2017
ms.author: tomfitz
ms.openlocfilehash: b81254196013adf87998eff11a50993efa52d40d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="track-asynchronous-azure-operations"></a><span data-ttu-id="e74b0-103">Sledování asynchronní operace v Azure</span><span class="sxs-lookup"><span data-stu-id="e74b0-103">Track asynchronous Azure operations</span></span>
<span data-ttu-id="e74b0-104">Některé operace Azure REST spustit asynchronně, protože hello operaci nelze dokončit rychle.</span><span class="sxs-lookup"><span data-stu-id="e74b0-104">Some Azure REST operations run asynchronously because hello operation cannot be completed quickly.</span></span> <span data-ttu-id="e74b0-105">Toto téma popisuje, jak tootrack hello stav asynchronní operace prostřednictvím hodnoty, vrátí se v odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="e74b0-105">This topic describes how tootrack hello status of asynchronous operations through values returned in hello response.</span></span>  

## <a name="status-codes-for-asynchronous-operations"></a><span data-ttu-id="e74b0-106">Stavové kódy pro asynchronní operace</span><span class="sxs-lookup"><span data-stu-id="e74b0-106">Status codes for asynchronous operations</span></span>
<span data-ttu-id="e74b0-107">Asynchronní operace původně vrátí kód stavu HTTP buď:</span><span class="sxs-lookup"><span data-stu-id="e74b0-107">An asynchronous operation initially returns an HTTP status code of either:</span></span>

* <span data-ttu-id="e74b0-108">201 (vytvořeno)</span><span class="sxs-lookup"><span data-stu-id="e74b0-108">201 (Created)</span></span>
* <span data-ttu-id="e74b0-109">202 (platných)</span><span class="sxs-lookup"><span data-stu-id="e74b0-109">202 (Accepted)</span></span> 

<span data-ttu-id="e74b0-110">Při úspěšném dokončení operace hello, vrátí buď:</span><span class="sxs-lookup"><span data-stu-id="e74b0-110">When hello operation successfully completes, it returns either:</span></span>

* <span data-ttu-id="e74b0-111">200 (OK)</span><span class="sxs-lookup"><span data-stu-id="e74b0-111">200 (OK)</span></span>
* <span data-ttu-id="e74b0-112">204 (žádný obsah)</span><span class="sxs-lookup"><span data-stu-id="e74b0-112">204 (No Content)</span></span> 

<span data-ttu-id="e74b0-113">Odkazovat toohello [dokumentace k REST API](/rest/api/) toosee hello odpovědi pro operaci hello jsou prováděny.</span><span class="sxs-lookup"><span data-stu-id="e74b0-113">Refer toohello [REST API documentation](/rest/api/) toosee hello responses for hello operation you are executing.</span></span> 

## <a name="monitor-status-of-operation"></a><span data-ttu-id="e74b0-114">Sledujte stav operace</span><span class="sxs-lookup"><span data-stu-id="e74b0-114">Monitor status of operation</span></span>
<span data-ttu-id="e74b0-115">Hello asynchronní REST operations návratový hodnoty hlavičky, které používají toodetermine hello stav hello operaci.</span><span class="sxs-lookup"><span data-stu-id="e74b0-115">hello asynchronous REST operations return header values, which you use toodetermine hello status of hello operation.</span></span> <span data-ttu-id="e74b0-116">Potenciálně existují tři hlavičky tooexamine hodnoty:</span><span class="sxs-lookup"><span data-stu-id="e74b0-116">There are potentially three header values tooexamine:</span></span>

* <span data-ttu-id="e74b0-117">`Azure-AsyncOperation`-Adresa URL pro kontrolu hello probíhající stav operace hello.</span><span class="sxs-lookup"><span data-stu-id="e74b0-117">`Azure-AsyncOperation` - URL for checking hello ongoing status of hello operation.</span></span> <span data-ttu-id="e74b0-118">Pokud vaše operace vrací hodnotu této, vždy používejte it (namísto umístění) tootrack hello stav operace hello.</span><span class="sxs-lookup"><span data-stu-id="e74b0-118">If your operation returns this value, always use it (instead of Location) tootrack hello status of hello operation.</span></span>
* <span data-ttu-id="e74b0-119">`Location`-Adresa URL pro určení po dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="e74b0-119">`Location` - URL for determining when an operation has completed.</span></span> <span data-ttu-id="e74b0-120">Tato hodnota se používá jenom v případě, že Azure AsyncOperation nevrátí.</span><span class="sxs-lookup"><span data-stu-id="e74b0-120">Use this value only when Azure-AsyncOperation is not returned.</span></span>
* <span data-ttu-id="e74b0-121">`Retry-After`-hello počet sekund toowait před zaškrtnutím hello stav asynchronní operace hello.</span><span class="sxs-lookup"><span data-stu-id="e74b0-121">`Retry-After` - hello number of seconds toowait before checking hello status of hello asynchronous operation.</span></span>

<span data-ttu-id="e74b0-122">Všechny tyto hodnoty se ale vrátí se nemusí být vždy asynchronní operaci.</span><span class="sxs-lookup"><span data-stu-id="e74b0-122">However, not every asynchronous operation returns all these values.</span></span> <span data-ttu-id="e74b0-123">Například může být nutné hodnota hlavičky Azure AsyncOperation hello tooevaluate pro jednu operaci a hodnota hlavičky hello umístění pro jiná operace.</span><span class="sxs-lookup"><span data-stu-id="e74b0-123">For example, you may need tooevaluate hello Azure-AsyncOperation header value for one operation, and hello Location header value for another operation.</span></span> 

<span data-ttu-id="e74b0-124">Hodnoty hlavičky hello načíst, protože by načíst všechny hodnoty v záhlaví požadavku.</span><span class="sxs-lookup"><span data-stu-id="e74b0-124">You retrieve hello header values as you would retrieve any header value for a request.</span></span> <span data-ttu-id="e74b0-125">Například v jazyce C#, můžete načíst hodnotu hlavičky hello z `HttpWebResponse` objekt s názvem `response` s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="e74b0-125">For example, in C#, you retrieve hello header value from an `HttpWebResponse` object named `response` with hello following code:</span></span>

```cs
response.Headers.GetValues("Azure-AsyncOperation").GetValue(0)
```

## <a name="azure-asyncoperation-request-and-response"></a><span data-ttu-id="e74b0-126">Azure AsyncOperation žádosti a odpovědi</span><span class="sxs-lookup"><span data-stu-id="e74b0-126">Azure-AsyncOperation request and response</span></span>

<span data-ttu-id="e74b0-127">tooget hello stav asynchronní operace hello, odeslat požadavek GET toohello URL v Azure AsyncOperation hodnotu hlavičky.</span><span class="sxs-lookup"><span data-stu-id="e74b0-127">tooget hello status of hello asynchronous operation, send a GET request toohello URL in Azure-AsyncOperation header value.</span></span>

<span data-ttu-id="e74b0-128">Hello text odpovědi hello z této operace obsahuje informace o operaci hello.</span><span class="sxs-lookup"><span data-stu-id="e74b0-128">hello body of hello response from this operation contains information about hello operation.</span></span> <span data-ttu-id="e74b0-129">Hello následující příklad ukazuje hello vrácená z operace hello možné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="e74b0-129">hello following example shows hello possible values returned from hello operation:</span></span>

```json
{
    "id": "{resource path from GET operation}",
    "name": "{operation-id}", 
    "status" : "Succeeded | Failed | Canceled | {resource provider values}", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
    "endTime": "2017-01-06T20:56:56.002812+00:00",
    "percentComplete": {double between 0 and 100 },
    "properties": {
        /* Specific resource provider values for successful operations */
    },
    "error" : { 
        "code": "{error code}",  
        "message": "{error description}" 
    }
}
```

<span data-ttu-id="e74b0-130">Pouze `status` se vrátí pro všechny odpovědi.</span><span class="sxs-lookup"><span data-stu-id="e74b0-130">Only `status` is returned for all responses.</span></span> <span data-ttu-id="e74b0-131">Pokud stav hello se nezdařilo nebo zrušeno, je vrácen objekt chyba Hello.</span><span class="sxs-lookup"><span data-stu-id="e74b0-131">hello error object is returned when hello status is Failed or Canceled.</span></span> <span data-ttu-id="e74b0-132">Všechny ostatní hodnoty jsou volitelné. tedy hello odpovědi, které obdržíte může vypadat jinak, než příklad hello.</span><span class="sxs-lookup"><span data-stu-id="e74b0-132">All other values are optional; therefore, hello response you receive may look different than hello example.</span></span>

## <a name="provisioningstate-values"></a><span data-ttu-id="e74b0-133">hodnoty stavu zřizování</span><span class="sxs-lookup"><span data-stu-id="e74b0-133">provisioningState values</span></span>

<span data-ttu-id="e74b0-134">Operace, které slouží k vytvoření, aktualizace nebo odstranění (DELETE PUT, PATCH,) prostředku obvykle vrací `provisioningState` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e74b0-134">Operations that create, update, or delete (PUT, PATCH, DELETE) a resource typically return a `provisioningState` value.</span></span> <span data-ttu-id="e74b0-135">Po dokončení operace se vrátí jednu z následujících tří hodnot:</span><span class="sxs-lookup"><span data-stu-id="e74b0-135">When an operation has completed, one of following three values is returned:</span></span> 

* <span data-ttu-id="e74b0-136">Úspěch</span><span class="sxs-lookup"><span data-stu-id="e74b0-136">Succeeded</span></span>
* <span data-ttu-id="e74b0-137">Se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="e74b0-137">Failed</span></span>
* <span data-ttu-id="e74b0-138">Zrušeno</span><span class="sxs-lookup"><span data-stu-id="e74b0-138">Canceled</span></span>

<span data-ttu-id="e74b0-139">Všechny ostatní hodnoty označují, že je stále spuštěna operace hello.</span><span class="sxs-lookup"><span data-stu-id="e74b0-139">All other values indicate hello operation is still running.</span></span> <span data-ttu-id="e74b0-140">Poskytovatel prostředků Hello může vrátit vlastní hodnotu, která určuje jeho stav.</span><span class="sxs-lookup"><span data-stu-id="e74b0-140">hello resource provider can return a customized value that indicates its state.</span></span> <span data-ttu-id="e74b0-141">Například můžete obdržet **platných** po hello žádosti přijaté a spuštěná.</span><span class="sxs-lookup"><span data-stu-id="e74b0-141">For example, you may receive **Accepted** when hello request is received and running.</span></span>

## <a name="example-requests-and-responses"></a><span data-ttu-id="e74b0-142">Příklad požadavky a odpovědi</span><span class="sxs-lookup"><span data-stu-id="e74b0-142">Example requests and responses</span></span>

### <a name="start-virtual-machine-202-with-azure-asyncoperation"></a><span data-ttu-id="e74b0-143">Spustit virtuální počítač (202 s Azure AsyncOperation)</span><span class="sxs-lookup"><span data-stu-id="e74b0-143">Start virtual machine (202 with Azure-AsyncOperation)</span></span>
<span data-ttu-id="e74b0-144">Tento příklad ukazuje, jak toodetermine hello stav **spustit** operace pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="e74b0-144">This example shows how toodetermine hello status of **start** operation for virtual machines.</span></span> <span data-ttu-id="e74b0-145">počáteční žádost Hello je hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="e74b0-145">hello initial request is in hello following format:</span></span>

```HTTP
POST 
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Compute/virtualMachines/{vm-name}/start?api-version=2016-03-30
```

<span data-ttu-id="e74b0-146">Vrátí stavový kód 202.</span><span class="sxs-lookup"><span data-stu-id="e74b0-146">It returns status code 202.</span></span> <span data-ttu-id="e74b0-147">Mezi hodnoty hlavičky hello zobrazí:</span><span class="sxs-lookup"><span data-stu-id="e74b0-147">Among hello header values, you see:</span></span>

```HTTP
Azure-AsyncOperation : https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2016-03-30
```

<span data-ttu-id="e74b0-148">toocheck hello stav asynchronní operace hello odesílání jinou adresu URL toothat požadavku.</span><span class="sxs-lookup"><span data-stu-id="e74b0-148">toocheck hello status of hello asynchronous operation, sending another request toothat URL.</span></span>

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2016-03-30
```

<span data-ttu-id="e74b0-149">text odpovědi Hello obsahuje hello stav operace hello:</span><span class="sxs-lookup"><span data-stu-id="e74b0-149">hello response body contains hello status of hello operation:</span></span>

```json
{
  "startTime": "2017-01-06T18:58:24.7596323+00:00",
  "status": "InProgress",
  "name": "9a062a88-e463-4697-bef2-fe039df73a02"
}
```

### <a name="deploy-resources-201-with-azure-asyncoperation"></a><span data-ttu-id="e74b0-150">Nasadit prostředky (201 s Azure AsyncOperation)</span><span class="sxs-lookup"><span data-stu-id="e74b0-150">Deploy resources (201 with Azure-AsyncOperation)</span></span>

<span data-ttu-id="e74b0-151">Tento příklad ukazuje, jak toodetermine hello stav **nasazení** operace pro nasazení tooAzure prostředky.</span><span class="sxs-lookup"><span data-stu-id="e74b0-151">This example shows how toodetermine hello status of **deployments** operation for deploying resources tooAzure.</span></span> <span data-ttu-id="e74b0-152">počáteční žádost Hello je hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="e74b0-152">hello initial request is in hello following format:</span></span>

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/microsoft.resources/deployments/{deployment-name}?api-version=2016-09-01
```

<span data-ttu-id="e74b0-153">Vrátí stavový kód 201.</span><span class="sxs-lookup"><span data-stu-id="e74b0-153">It returns status code 201.</span></span> <span data-ttu-id="e74b0-154">Hello text odpovědi hello zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="e74b0-154">hello body of hello response includes:</span></span>

```json
"provisioningState":"Accepted",
```

<span data-ttu-id="e74b0-155">Mezi hodnoty hlavičky hello zobrazí:</span><span class="sxs-lookup"><span data-stu-id="e74b0-155">Among hello header values, you see:</span></span>

```HTTP
Azure-AsyncOperation: https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2016-09-01
```

<span data-ttu-id="e74b0-156">toocheck hello stav asynchronní operace hello odesílání jinou adresu URL toothat požadavku.</span><span class="sxs-lookup"><span data-stu-id="e74b0-156">toocheck hello status of hello asynchronous operation, sending another request toothat URL.</span></span>

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2016-09-01
```

<span data-ttu-id="e74b0-157">text odpovědi Hello obsahuje hello stav operace hello:</span><span class="sxs-lookup"><span data-stu-id="e74b0-157">hello response body contains hello status of hello operation:</span></span>

```json
{"status":"Running"}
```

<span data-ttu-id="e74b0-158">Po dokončení nasazení hello hello odpovědi obsahuje:</span><span class="sxs-lookup"><span data-stu-id="e74b0-158">When hello deployment is finished, hello response contains:</span></span>

```json
{"status":"Succeeded"}
```

### <a name="create-storage-account-202-with-location-and-retry-after"></a><span data-ttu-id="e74b0-159">Vytvořit účet úložiště (202 umístění a zkuste to znovu za)</span><span class="sxs-lookup"><span data-stu-id="e74b0-159">Create storage account (202 with Location and Retry-After)</span></span>

<span data-ttu-id="e74b0-160">Tento příklad ukazuje, jak toodetermine hello stav hello **vytvořit** operace pro účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="e74b0-160">This example shows how toodetermine hello status of hello **create** operation for storage accounts.</span></span> <span data-ttu-id="e74b0-161">počáteční žádost Hello je hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="e74b0-161">hello initial request is in hello following format:</span></span>

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}?api-version=2016-01-01
```

<span data-ttu-id="e74b0-162">A hello text žádosti obsahuje vlastnosti pro účet úložiště hello:</span><span class="sxs-lookup"><span data-stu-id="e74b0-162">And hello request body contains properties for hello storage account:</span></span>

```json
{ "location": "South Central US", "properties": {}, "sku": { "name": "Standard_LRS" }, "kind": "Storage" }
```

<span data-ttu-id="e74b0-163">Vrátí stavový kód 202.</span><span class="sxs-lookup"><span data-stu-id="e74b0-163">It returns status code 202.</span></span> <span data-ttu-id="e74b0-164">Mezi hodnoty hlavičky hello najdete v části hello následující dvě hodnoty:</span><span class="sxs-lookup"><span data-stu-id="e74b0-164">Among hello header values, you see hello following two values:</span></span>

```HTTP
Location: https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2016-01-01
Retry-After: 17
```

<span data-ttu-id="e74b0-165">Po uplynutí počtu sekund zadaný v opakovat po, zkontrolujte hello stav asynchronní operace hello odesláním jinou adresu URL toothat požadavku.</span><span class="sxs-lookup"><span data-stu-id="e74b0-165">After waiting for number of seconds specified in Retry-After, check hello status of hello asynchronous operation by sending another request toothat URL.</span></span>

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2016-01-01
```

<span data-ttu-id="e74b0-166">Pokud požadavek hello je stále spuštěná, obdržíte kód stavu 202.</span><span class="sxs-lookup"><span data-stu-id="e74b0-166">If hello request is still running, you receive a status code 202.</span></span> <span data-ttu-id="e74b0-167">Pokud požadavek hello byl dokončen, vaše přijímat stavovým kódem 200 a hello text odpovědi hello obsahuje vlastnosti hello hello účtu úložiště, který byl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="e74b0-167">If hello request has completed, your receive a status code 200, and hello body of hello response contains hello properties of hello storage account that has been created.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e74b0-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e74b0-168">Next steps</span></span>

* <span data-ttu-id="e74b0-169">Dokumentaci o každé operace REST najdete v tématu [dokumentace k REST API](/rest/api/).</span><span class="sxs-lookup"><span data-stu-id="e74b0-169">For documentation about each REST operation, see [REST API documentation](/rest/api/).</span></span>
* <span data-ttu-id="e74b0-170">Informace o správě prostředků prostřednictvím hello REST API Resource Manageru najdete v tématu [hello pomocí REST API Resource Manageru](resource-manager-rest-api.md).</span><span class="sxs-lookup"><span data-stu-id="e74b0-170">For information about managing resources through hello Resource Manager REST API, see [Using hello Resource Manager REST API](resource-manager-rest-api.md).</span></span>
* <span data-ttu-id="e74b0-171">informace o nasazení šablony prostřednictvím hello REST API Resource Manageru najdete v tématu [nasazení prostředků pomocí šablony Resource Manageru a REST API Resource Manageru](resource-group-template-deploy-rest.md).</span><span class="sxs-lookup"><span data-stu-id="e74b0-171">for information about deploying templates through hello Resource Manager REST API, see [Deploy resources with Resource Manager templates and Resource Manager REST API](resource-group-template-deploy-rest.md).</span></span>
