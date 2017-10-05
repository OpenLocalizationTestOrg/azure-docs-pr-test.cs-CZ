---
title: "Asynchronní operace v Azure | Microsoft Docs"
description: "Popisuje, jak sledovat asynchronních operací v Azure."
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
ms.openlocfilehash: 9fe3d98cd345aae45722295b6c1b7fc3e9036e95
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="track-asynchronous-azure-operations"></a><span data-ttu-id="edb32-103">Sledování asynchronní operace v Azure</span><span class="sxs-lookup"><span data-stu-id="edb32-103">Track asynchronous Azure operations</span></span>
<span data-ttu-id="edb32-104">Některé operace Azure REST spustit asynchronně, protože operaci nelze dokončit rychle.</span><span class="sxs-lookup"><span data-stu-id="edb32-104">Some Azure REST operations run asynchronously because the operation cannot be completed quickly.</span></span> <span data-ttu-id="edb32-105">Toto téma popisuje, jak sledovat stav asynchronní operace prostřednictvím hodnot vrácených v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="edb32-105">This topic describes how to track the status of asynchronous operations through values returned in the response.</span></span>  

## <a name="status-codes-for-asynchronous-operations"></a><span data-ttu-id="edb32-106">Stavové kódy pro asynchronní operace</span><span class="sxs-lookup"><span data-stu-id="edb32-106">Status codes for asynchronous operations</span></span>
<span data-ttu-id="edb32-107">Asynchronní operace původně vrátí kód stavu HTTP buď:</span><span class="sxs-lookup"><span data-stu-id="edb32-107">An asynchronous operation initially returns an HTTP status code of either:</span></span>

* <span data-ttu-id="edb32-108">201 (vytvořeno)</span><span class="sxs-lookup"><span data-stu-id="edb32-108">201 (Created)</span></span>
* <span data-ttu-id="edb32-109">202 (platných)</span><span class="sxs-lookup"><span data-stu-id="edb32-109">202 (Accepted)</span></span> 

<span data-ttu-id="edb32-110">Po úspěšném dokončení operace, vrátí buď:</span><span class="sxs-lookup"><span data-stu-id="edb32-110">When the operation successfully completes, it returns either:</span></span>

* <span data-ttu-id="edb32-111">200 (OK)</span><span class="sxs-lookup"><span data-stu-id="edb32-111">200 (OK)</span></span>
* <span data-ttu-id="edb32-112">204 (žádný obsah)</span><span class="sxs-lookup"><span data-stu-id="edb32-112">204 (No Content)</span></span> 

<span data-ttu-id="edb32-113">Odkazovat [dokumentace k REST API](/rest/api/) zobrazíte odpovědi pro operaci jsou prováděny.</span><span class="sxs-lookup"><span data-stu-id="edb32-113">Refer to the [REST API documentation](/rest/api/) to see the responses for the operation you are executing.</span></span> 

## <a name="monitor-status-of-operation"></a><span data-ttu-id="edb32-114">Sledujte stav operace</span><span class="sxs-lookup"><span data-stu-id="edb32-114">Monitor status of operation</span></span>
<span data-ttu-id="edb32-115">Asynchronní operace REST návratové hodnoty hlavičky, které můžete použít k určení stavu operace.</span><span class="sxs-lookup"><span data-stu-id="edb32-115">The asynchronous REST operations return header values, which you use to determine the status of the operation.</span></span> <span data-ttu-id="edb32-116">Existují potenciálně tři hodnoty hlavičky k zkontrolujte:</span><span class="sxs-lookup"><span data-stu-id="edb32-116">There are potentially three header values to examine:</span></span>

* <span data-ttu-id="edb32-117">`Azure-AsyncOperation`-Adresa URL pro kontrolu stavu probíhající operace.</span><span class="sxs-lookup"><span data-stu-id="edb32-117">`Azure-AsyncOperation` - URL for checking the ongoing status of the operation.</span></span> <span data-ttu-id="edb32-118">Pokud vaše operace vrací hodnotu této, vždy použijte ho (namísto umístění) sledovat stav operace.</span><span class="sxs-lookup"><span data-stu-id="edb32-118">If your operation returns this value, always use it (instead of Location) to track the status of the operation.</span></span>
* <span data-ttu-id="edb32-119">`Location`-Adresa URL pro určení po dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="edb32-119">`Location` - URL for determining when an operation has completed.</span></span> <span data-ttu-id="edb32-120">Tato hodnota se používá jenom v případě, že Azure AsyncOperation nevrátí.</span><span class="sxs-lookup"><span data-stu-id="edb32-120">Use this value only when Azure-AsyncOperation is not returned.</span></span>
* <span data-ttu-id="edb32-121">`Retry-After`-Počet sekund čekání před kontroluje stav asynchronní operace.</span><span class="sxs-lookup"><span data-stu-id="edb32-121">`Retry-After` - The number of seconds to wait before checking the status of the asynchronous operation.</span></span>

<span data-ttu-id="edb32-122">Všechny tyto hodnoty se ale vrátí se nemusí být vždy asynchronní operaci.</span><span class="sxs-lookup"><span data-stu-id="edb32-122">However, not every asynchronous operation returns all these values.</span></span> <span data-ttu-id="edb32-123">Potřebujete například vyhodnotit hodnotu hlavičky Azure AsyncOperation pro jednu operaci a hodnota hlavičky umístění pro jiná operace.</span><span class="sxs-lookup"><span data-stu-id="edb32-123">For example, you may need to evaluate the Azure-AsyncOperation header value for one operation, and the Location header value for another operation.</span></span> 

<span data-ttu-id="edb32-124">Hodnoty hlavičky načíst, protože by načíst všechny hodnoty v záhlaví požadavku.</span><span class="sxs-lookup"><span data-stu-id="edb32-124">You retrieve the header values as you would retrieve any header value for a request.</span></span> <span data-ttu-id="edb32-125">Například v jazyce C#, načtete hodnota hlavičky ze `HttpWebResponse` objekt s názvem `response` následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="edb32-125">For example, in C#, you retrieve the header value from an `HttpWebResponse` object named `response` with the following code:</span></span>

```cs
response.Headers.GetValues("Azure-AsyncOperation").GetValue(0)
```

## <a name="azure-asyncoperation-request-and-response"></a><span data-ttu-id="edb32-126">Azure AsyncOperation žádosti a odpovědi</span><span class="sxs-lookup"><span data-stu-id="edb32-126">Azure-AsyncOperation request and response</span></span>

<span data-ttu-id="edb32-127">Pokud chcete získat stav asynchronní operace, odeslat požadavek GET na adresu URL v Azure AsyncOperation hodnota hlavičky.</span><span class="sxs-lookup"><span data-stu-id="edb32-127">To get the status of the asynchronous operation, send a GET request to the URL in Azure-AsyncOperation header value.</span></span>

<span data-ttu-id="edb32-128">Text odpovědi z této operace obsahuje informace o operaci.</span><span class="sxs-lookup"><span data-stu-id="edb32-128">The body of the response from this operation contains information about the operation.</span></span> <span data-ttu-id="edb32-129">Následující příklad ukazuje možné hodnoty vrácená z operace:</span><span class="sxs-lookup"><span data-stu-id="edb32-129">The following example shows the possible values returned from the operation:</span></span>

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

<span data-ttu-id="edb32-130">Pouze `status` se vrátí pro všechny odpovědi.</span><span class="sxs-lookup"><span data-stu-id="edb32-130">Only `status` is returned for all responses.</span></span> <span data-ttu-id="edb32-131">Objekt chyba se vrátí, když se stav se nezdařilo nebo zrušení.</span><span class="sxs-lookup"><span data-stu-id="edb32-131">The error object is returned when the status is Failed or Canceled.</span></span> <span data-ttu-id="edb32-132">Všechny ostatní hodnoty jsou volitelné. proto bude vypadat odpovědi, které obdržíte lišit od příkladu.</span><span class="sxs-lookup"><span data-stu-id="edb32-132">All other values are optional; therefore, the response you receive may look different than the example.</span></span>

## <a name="provisioningstate-values"></a><span data-ttu-id="edb32-133">hodnoty stavu zřizování</span><span class="sxs-lookup"><span data-stu-id="edb32-133">provisioningState values</span></span>

<span data-ttu-id="edb32-134">Operace, které slouží k vytvoření, aktualizace nebo odstranění (DELETE PUT, PATCH,) prostředku obvykle vrací `provisioningState` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="edb32-134">Operations that create, update, or delete (PUT, PATCH, DELETE) a resource typically return a `provisioningState` value.</span></span> <span data-ttu-id="edb32-135">Po dokončení operace se vrátí jednu z následujících tří hodnot:</span><span class="sxs-lookup"><span data-stu-id="edb32-135">When an operation has completed, one of following three values is returned:</span></span> 

* <span data-ttu-id="edb32-136">Úspěch</span><span class="sxs-lookup"><span data-stu-id="edb32-136">Succeeded</span></span>
* <span data-ttu-id="edb32-137">Se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="edb32-137">Failed</span></span>
* <span data-ttu-id="edb32-138">Zrušeno</span><span class="sxs-lookup"><span data-stu-id="edb32-138">Canceled</span></span>

<span data-ttu-id="edb32-139">Všechny ostatní hodnoty označují, že operace je stále spuštěná.</span><span class="sxs-lookup"><span data-stu-id="edb32-139">All other values indicate the operation is still running.</span></span> <span data-ttu-id="edb32-140">Poskytovatel prostředku může vrátit vlastní hodnotu, která určuje jeho stav.</span><span class="sxs-lookup"><span data-stu-id="edb32-140">The resource provider can return a customized value that indicates its state.</span></span> <span data-ttu-id="edb32-141">Například můžete obdržet **platných** po žádosti přijaté a spuštěná.</span><span class="sxs-lookup"><span data-stu-id="edb32-141">For example, you may receive **Accepted** when the request is received and running.</span></span>

## <a name="example-requests-and-responses"></a><span data-ttu-id="edb32-142">Příklad požadavky a odpovědi</span><span class="sxs-lookup"><span data-stu-id="edb32-142">Example requests and responses</span></span>

### <a name="start-virtual-machine-202-with-azure-asyncoperation"></a><span data-ttu-id="edb32-143">Spustit virtuální počítač (202 s Azure AsyncOperation)</span><span class="sxs-lookup"><span data-stu-id="edb32-143">Start virtual machine (202 with Azure-AsyncOperation)</span></span>
<span data-ttu-id="edb32-144">Tento příklad ukazuje, jak určit stav **spustit** operace pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="edb32-144">This example shows how to determine the status of **start** operation for virtual machines.</span></span> <span data-ttu-id="edb32-145">Počáteční žádosti je v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="edb32-145">The initial request is in the following format:</span></span>

```HTTP
POST 
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Compute/virtualMachines/{vm-name}/start?api-version=2016-03-30
```

<span data-ttu-id="edb32-146">Vrátí stavový kód 202.</span><span class="sxs-lookup"><span data-stu-id="edb32-146">It returns status code 202.</span></span> <span data-ttu-id="edb32-147">Mezi hodnoty hlavičky zobrazí:</span><span class="sxs-lookup"><span data-stu-id="edb32-147">Among the header values, you see:</span></span>

```HTTP
Azure-AsyncOperation : https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2016-03-30
```

<span data-ttu-id="edb32-148">Chcete-li zkontrolovat stav asynchronní operace odesílání další požadavek na tuto adresu URL.</span><span class="sxs-lookup"><span data-stu-id="edb32-148">To check the status of the asynchronous operation, sending another request to that URL.</span></span>

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2016-03-30
```

<span data-ttu-id="edb32-149">Text odpovědi obsahuje stav operace:</span><span class="sxs-lookup"><span data-stu-id="edb32-149">The response body contains the status of the operation:</span></span>

```json
{
  "startTime": "2017-01-06T18:58:24.7596323+00:00",
  "status": "InProgress",
  "name": "9a062a88-e463-4697-bef2-fe039df73a02"
}
```

### <a name="deploy-resources-201-with-azure-asyncoperation"></a><span data-ttu-id="edb32-150">Nasadit prostředky (201 s Azure AsyncOperation)</span><span class="sxs-lookup"><span data-stu-id="edb32-150">Deploy resources (201 with Azure-AsyncOperation)</span></span>

<span data-ttu-id="edb32-151">Tento příklad ukazuje, jak určit stav **nasazení** operace nasazení prostředků do Azure.</span><span class="sxs-lookup"><span data-stu-id="edb32-151">This example shows how to determine the status of **deployments** operation for deploying resources to Azure.</span></span> <span data-ttu-id="edb32-152">Počáteční žádosti je v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="edb32-152">The initial request is in the following format:</span></span>

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/microsoft.resources/deployments/{deployment-name}?api-version=2016-09-01
```

<span data-ttu-id="edb32-153">Vrátí stavový kód 201.</span><span class="sxs-lookup"><span data-stu-id="edb32-153">It returns status code 201.</span></span> <span data-ttu-id="edb32-154">Text odpovědi obsahuje:</span><span class="sxs-lookup"><span data-stu-id="edb32-154">The body of the response includes:</span></span>

```json
"provisioningState":"Accepted",
```

<span data-ttu-id="edb32-155">Mezi hodnoty hlavičky zobrazí:</span><span class="sxs-lookup"><span data-stu-id="edb32-155">Among the header values, you see:</span></span>

```HTTP
Azure-AsyncOperation: https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2016-09-01
```

<span data-ttu-id="edb32-156">Chcete-li zkontrolovat stav asynchronní operace odesílání další požadavek na tuto adresu URL.</span><span class="sxs-lookup"><span data-stu-id="edb32-156">To check the status of the asynchronous operation, sending another request to that URL.</span></span>

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2016-09-01
```

<span data-ttu-id="edb32-157">Text odpovědi obsahuje stav operace:</span><span class="sxs-lookup"><span data-stu-id="edb32-157">The response body contains the status of the operation:</span></span>

```json
{"status":"Running"}
```

<span data-ttu-id="edb32-158">Po dokončení nasazení obsahuje odpovědi:</span><span class="sxs-lookup"><span data-stu-id="edb32-158">When the deployment is finished, the response contains:</span></span>

```json
{"status":"Succeeded"}
```

### <a name="create-storage-account-202-with-location-and-retry-after"></a><span data-ttu-id="edb32-159">Vytvořit účet úložiště (202 umístění a zkuste to znovu za)</span><span class="sxs-lookup"><span data-stu-id="edb32-159">Create storage account (202 with Location and Retry-After)</span></span>

<span data-ttu-id="edb32-160">Tento příklad ukazuje, jak určit stav **vytvořit** operace pro účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="edb32-160">This example shows how to determine the status of the **create** operation for storage accounts.</span></span> <span data-ttu-id="edb32-161">Počáteční žádosti je v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="edb32-161">The initial request is in the following format:</span></span>

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}?api-version=2016-01-01
```

<span data-ttu-id="edb32-162">A text žádosti obsahuje vlastnosti pro účet úložiště:</span><span class="sxs-lookup"><span data-stu-id="edb32-162">And the request body contains properties for the storage account:</span></span>

```json
{ "location": "South Central US", "properties": {}, "sku": { "name": "Standard_LRS" }, "kind": "Storage" }
```

<span data-ttu-id="edb32-163">Vrátí stavový kód 202.</span><span class="sxs-lookup"><span data-stu-id="edb32-163">It returns status code 202.</span></span> <span data-ttu-id="edb32-164">Mezi hodnoty hlavičky najdete v následujících dvou hodnot:</span><span class="sxs-lookup"><span data-stu-id="edb32-164">Among the header values, you see the following two values:</span></span>

```HTTP
Location: https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2016-01-01
Retry-After: 17
```

<span data-ttu-id="edb32-165">Po čekání počet sekund zadaný v opakovat po, zkontrolujte stav asynchronní operace odesláním další požadavek na tuto adresu URL.</span><span class="sxs-lookup"><span data-stu-id="edb32-165">After waiting for number of seconds specified in Retry-After, check the status of the asynchronous operation by sending another request to that URL.</span></span>

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2016-01-01
```

<span data-ttu-id="edb32-166">Pokud požadavek je stále spuštěná, obdržíte kód stavu 202.</span><span class="sxs-lookup"><span data-stu-id="edb32-166">If the request is still running, you receive a status code 202.</span></span> <span data-ttu-id="edb32-167">Pokud je požadavek dokončen, vaše přijímat stavovým kódem 200 a text odpovědi obsahuje vlastnosti účtu úložiště, který byl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="edb32-167">If the request has completed, your receive a status code 200, and the body of the response contains the properties of the storage account that has been created.</span></span>

## <a name="next-steps"></a><span data-ttu-id="edb32-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="edb32-168">Next steps</span></span>

* <span data-ttu-id="edb32-169">Dokumentaci o každé operace REST najdete v tématu [dokumentace k REST API](/rest/api/).</span><span class="sxs-lookup"><span data-stu-id="edb32-169">For documentation about each REST operation, see [REST API documentation](/rest/api/).</span></span>
* <span data-ttu-id="edb32-170">Informace o správě prostředků prostřednictvím REST API Resource Manageru najdete v tématu [pomocí REST API Resource Manager](resource-manager-rest-api.md).</span><span class="sxs-lookup"><span data-stu-id="edb32-170">For information about managing resources through the Resource Manager REST API, see [Using the Resource Manager REST API](resource-manager-rest-api.md).</span></span>
* <span data-ttu-id="edb32-171">informace o nasazení šablony přes rozhraní REST API Resource Manager najdete v tématu [nasazení prostředků pomocí šablony Resource Manageru a REST API Resource Manageru](resource-group-template-deploy-rest.md).</span><span class="sxs-lookup"><span data-stu-id="edb32-171">for information about deploying templates through the Resource Manager REST API, see [Deploy resources with Resource Manager templates and Resource Manager REST API](resource-group-template-deploy-rest.md).</span></span>