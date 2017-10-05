---
title: "Funkce šablon Azure Resource Manager - prostředky | Microsoft Docs"
description: "Popisuje funkce pro použití v šablonu Azure Resource Manager k načtení hodnoty o prostředcích."
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
ms.date: 08/09/2017
ms.author: tomfitz
ms.openlocfilehash: 494ade55f21c19d9c68d5cc52756528401d9bb77
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="resource-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="01c89-103">Funkce prostředků pro šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="01c89-103">Resource functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="01c89-104">Resource Manager poskytuje následující funkce pro získání hodnoty prostředku:</span><span class="sxs-lookup"><span data-stu-id="01c89-104">Resource Manager provides the following functions for getting resource values:</span></span>

* [<span data-ttu-id="01c89-105">listKeys a seznamu {Value}</span><span class="sxs-lookup"><span data-stu-id="01c89-105">listKeys and list{Value}</span></span>](#listkeys)
* [<span data-ttu-id="01c89-106">Zprostředkovatelé</span><span class="sxs-lookup"><span data-stu-id="01c89-106">providers</span></span>](#providers)
* [<span data-ttu-id="01c89-107">referenční dokumentace</span><span class="sxs-lookup"><span data-stu-id="01c89-107">reference</span></span>](#reference)
* [<span data-ttu-id="01c89-108">Skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="01c89-108">resourceGroup</span></span>](#resourcegroup)
* [<span data-ttu-id="01c89-109">ID prostředku</span><span class="sxs-lookup"><span data-stu-id="01c89-109">resourceId</span></span>](#resourceid)
* [<span data-ttu-id="01c89-110">předplatné</span><span class="sxs-lookup"><span data-stu-id="01c89-110">subscription</span></span>](#subscription)

<span data-ttu-id="01c89-111">Chcete-li získat hodnoty z parametrů, proměnné nebo aktuální nasazení, přečtěte si téma [funkce hodnota nasazení](resource-group-template-functions-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="01c89-111">To get values from parameters, variables, or the current deployment, see [Deployment value functions](resource-group-template-functions-deployment.md).</span></span>

<a id="listkeys" />
<a id="list" />

## <a name="listkeys-and-listvalue"></a><span data-ttu-id="01c89-112">listKeys a seznamu {Value}</span><span class="sxs-lookup"><span data-stu-id="01c89-112">listKeys and list{Value}</span></span>
`listKeys(resourceName or resourceIdentifier, apiVersion)`

`list{Value}(resourceName or resourceIdentifier, apiVersion)`

<span data-ttu-id="01c89-113">Vrátí hodnoty pro libovolný typ prostředku, který podporuje operaci seznamu.</span><span class="sxs-lookup"><span data-stu-id="01c89-113">Returns the values for any resource type that supports the list operation.</span></span> <span data-ttu-id="01c89-114">Nejběžnější využití `listKeys`.</span><span class="sxs-lookup"><span data-stu-id="01c89-114">The most common usage is `listKeys`.</span></span> 

### <a name="parameters"></a><span data-ttu-id="01c89-115">Parametry</span><span class="sxs-lookup"><span data-stu-id="01c89-115">Parameters</span></span>

| <span data-ttu-id="01c89-116">Parametr</span><span class="sxs-lookup"><span data-stu-id="01c89-116">Parameter</span></span> | <span data-ttu-id="01c89-117">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="01c89-117">Required</span></span> | <span data-ttu-id="01c89-118">Typ</span><span class="sxs-lookup"><span data-stu-id="01c89-118">Type</span></span> | <span data-ttu-id="01c89-119">Popis</span><span class="sxs-lookup"><span data-stu-id="01c89-119">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="01c89-120">resourceName nebo resourceIdentifier</span><span class="sxs-lookup"><span data-stu-id="01c89-120">resourceName or resourceIdentifier</span></span> |<span data-ttu-id="01c89-121">Ano</span><span class="sxs-lookup"><span data-stu-id="01c89-121">Yes</span></span> |<span data-ttu-id="01c89-122">Řetězec</span><span class="sxs-lookup"><span data-stu-id="01c89-122">string</span></span> |<span data-ttu-id="01c89-123">Jedinečný identifikátor pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="01c89-123">Unique identifier for the resource.</span></span> |
| <span data-ttu-id="01c89-124">apiVersion</span><span class="sxs-lookup"><span data-stu-id="01c89-124">apiVersion</span></span> |<span data-ttu-id="01c89-125">Ano</span><span class="sxs-lookup"><span data-stu-id="01c89-125">Yes</span></span> |<span data-ttu-id="01c89-126">Řetězec</span><span class="sxs-lookup"><span data-stu-id="01c89-126">string</span></span> |<span data-ttu-id="01c89-127">Verze rozhraní API stav modulu runtime prostředků.</span><span class="sxs-lookup"><span data-stu-id="01c89-127">API version of resource runtime state.</span></span> <span data-ttu-id="01c89-128">Obvykle ve formátu **rrrr mm-dd**.</span><span class="sxs-lookup"><span data-stu-id="01c89-128">Typically, in the format, **yyyy-mm-dd**.</span></span> |

### <a name="return-value"></a><span data-ttu-id="01c89-129">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="01c89-129">Return value</span></span>

<span data-ttu-id="01c89-130">Objekt vrácený z listKeys má následující formát:</span><span class="sxs-lookup"><span data-stu-id="01c89-130">The returned object from listKeys has the following format:</span></span>

```json
{
  "keys": [
    {
      "keyName": "key1",
      "permissions": "Full",
      "value": "{value}"
    },
    {
      "keyName": "key2",
      "permissions": "Full",
      "value": "{value}"
    }
  ]
}
```

<span data-ttu-id="01c89-131">Jiné funkce seznamu mají návratový různých formátech.</span><span class="sxs-lookup"><span data-stu-id="01c89-131">Other list functions have different return formats.</span></span> <span data-ttu-id="01c89-132">Pokud chcete zobrazit formát funkce, její zahrnutí do části výstupy jak je znázorněno v příkladu šablony.</span><span class="sxs-lookup"><span data-stu-id="01c89-132">To see the format of a function, include it in the outputs section as shown in the example template.</span></span> 

### <a name="remarks"></a><span data-ttu-id="01c89-133">Poznámky</span><span class="sxs-lookup"><span data-stu-id="01c89-133">Remarks</span></span>

<span data-ttu-id="01c89-134">Všechny operace, který začíná **seznamu** lze použít jako funkci ve vaší šabloně.</span><span class="sxs-lookup"><span data-stu-id="01c89-134">Any operation that starts with **list** can be used as a function in your template.</span></span> <span data-ttu-id="01c89-135">K dispozici operace zahrnují nejen listKeys, ale také operace jako `list`, `listAdminKeys`, a `listStatus`.</span><span class="sxs-lookup"><span data-stu-id="01c89-135">The available operations include not only listKeys, but also operations like `list`, `listAdminKeys`, and `listStatus`.</span></span> <span data-ttu-id="01c89-136">Ale nemůžete použít **seznamu** činnosti, které vyžadují hodnoty v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="01c89-136">However, you cannot use **list** operations that require values in the request body.</span></span> <span data-ttu-id="01c89-137">Například [SAS účtu seznamu](/rest/api/storagerp/storageaccounts#StorageAccounts_ListAccountSAS) operace vyžaduje parametry text požadavku jako *signedExpiry*, takže ji nelze použít v rámci šablon.</span><span class="sxs-lookup"><span data-stu-id="01c89-137">For example, the [List Account SAS](/rest/api/storagerp/storageaccounts#StorageAccounts_ListAccountSAS) operation requires request body parameters like *signedExpiry*, so you cannot use it within a template.</span></span>

<span data-ttu-id="01c89-138">Pokud chcete zjistit, jaké typy prostředků obsahovat operaci seznamu, máte následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="01c89-138">To determine which resource types have a list operation, you have the following options:</span></span>

* <span data-ttu-id="01c89-139">Zobrazení [operace REST API](/rest/api/) pro poskytovatele prostředků a podívejte se na seznam operací.</span><span class="sxs-lookup"><span data-stu-id="01c89-139">View the [REST API operations](/rest/api/) for a resource provider, and look for list operations.</span></span> <span data-ttu-id="01c89-140">Například účty úložiště mít [listKeys operaci](/rest/api/storagerp/storageaccounts#StorageAccounts_ListKeys).</span><span class="sxs-lookup"><span data-stu-id="01c89-140">For example, storage accounts have the [listKeys operation](/rest/api/storagerp/storageaccounts#StorageAccounts_ListKeys).</span></span>
* <span data-ttu-id="01c89-141">Použití [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="01c89-141">Use the [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) PowerShell cmdlet.</span></span> <span data-ttu-id="01c89-142">Následující příklad načte všechny operace výpisu pro účty úložiště:</span><span class="sxs-lookup"><span data-stu-id="01c89-142">The following example gets all list operations for storage accounts:</span></span>

  ```powershell
  Get-AzureRmProviderOperation -OperationSearchString "Microsoft.Storage/*" | where {$_.Operation -like "*list*"} | FT Operation
  ```
* <span data-ttu-id="01c89-143">Pomocí následujícího příkazu příkazového řádku Azure CLI filtrovat seznam způsobů:</span><span class="sxs-lookup"><span data-stu-id="01c89-143">Use the following Azure CLI command to filter only the list operations:</span></span>

  ```azurecli
  az provider operation show --namespace Microsoft.Storage --query "resourceTypes[?name=='storageAccounts'].operations[].name | [?contains(@, 'list')]"
  ```

<span data-ttu-id="01c89-144">Zadejte prostředek pomocí [resourceId funkce](#resourceid), formát nebo `{providerNamespace}/{resourceType}/{resourceName}`.</span><span class="sxs-lookup"><span data-stu-id="01c89-144">Specify the resource by using either the [resourceId function](#resourceid), or the format `{providerNamespace}/{resourceType}/{resourceName}`.</span></span>


### <a name="example"></a><span data-ttu-id="01c89-145">Příklad</span><span class="sxs-lookup"><span data-stu-id="01c89-145">Example</span></span>

<span data-ttu-id="01c89-146">Následující příklad ukazuje, jak vrátit primární a sekundární klíče z účtu úložiště v části výstupy.</span><span class="sxs-lookup"><span data-stu-id="01c89-146">The following example shows how to return the primary and secondary keys from a storage account in the outputs section.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountId": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "storageKeysOutput": {
            "value": "[listKeys(parameters('storageAccountId'), '2016-01-01')]",
            "type" : "object"
        }
    }
}
``` 

<a id="providers" />

## <a name="providers"></a><span data-ttu-id="01c89-147">Zprostředkovatelé</span><span class="sxs-lookup"><span data-stu-id="01c89-147">providers</span></span>
`providers(providerNamespace, [resourceType])`

<span data-ttu-id="01c89-148">Vrátí informace o poskytovatele prostředků a jeho typy podporovaných zdrojů.</span><span class="sxs-lookup"><span data-stu-id="01c89-148">Returns information about a resource provider and its supported resource types.</span></span> <span data-ttu-id="01c89-149">Pokud není zadán typ prostředku, funkce vrátí všechny podporované typy pro poskytovatele prostředků.</span><span class="sxs-lookup"><span data-stu-id="01c89-149">If you do not provide a resource type, the function returns all the supported types for the resource provider.</span></span>

### <a name="parameters"></a><span data-ttu-id="01c89-150">Parametry</span><span class="sxs-lookup"><span data-stu-id="01c89-150">Parameters</span></span>

| <span data-ttu-id="01c89-151">Parametr</span><span class="sxs-lookup"><span data-stu-id="01c89-151">Parameter</span></span> | <span data-ttu-id="01c89-152">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="01c89-152">Required</span></span> | <span data-ttu-id="01c89-153">Typ</span><span class="sxs-lookup"><span data-stu-id="01c89-153">Type</span></span> | <span data-ttu-id="01c89-154">Popis</span><span class="sxs-lookup"><span data-stu-id="01c89-154">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="01c89-155">providerNamespace</span><span class="sxs-lookup"><span data-stu-id="01c89-155">providerNamespace</span></span> |<span data-ttu-id="01c89-156">Ano</span><span class="sxs-lookup"><span data-stu-id="01c89-156">Yes</span></span> |<span data-ttu-id="01c89-157">Řetězec</span><span class="sxs-lookup"><span data-stu-id="01c89-157">string</span></span> |<span data-ttu-id="01c89-158">Namespace zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="01c89-158">Namespace of the provider</span></span> |
| <span data-ttu-id="01c89-159">Typ prostředku</span><span class="sxs-lookup"><span data-stu-id="01c89-159">resourceType</span></span> |<span data-ttu-id="01c89-160">Ne</span><span class="sxs-lookup"><span data-stu-id="01c89-160">No</span></span> |<span data-ttu-id="01c89-161">Řetězec</span><span class="sxs-lookup"><span data-stu-id="01c89-161">string</span></span> |<span data-ttu-id="01c89-162">Typ prostředku v rámci zadaného oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="01c89-162">The type of resource within the specified namespace.</span></span> |

### <a name="return-value"></a><span data-ttu-id="01c89-163">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="01c89-163">Return value</span></span>

<span data-ttu-id="01c89-164">Každý podporovaný typ je vrácený v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="01c89-164">Each supported type is returned in the following format:</span></span> 

```json
{
    "resourceType": "{name of resource type}",
    "locations": [ all supported locations ],
    "apiVersions": [ all supported API versions ]
}
```

<span data-ttu-id="01c89-165">Řazení pole vrácené hodnoty není zaručena.</span><span class="sxs-lookup"><span data-stu-id="01c89-165">Array ordering of the returned values is not guaranteed.</span></span>

### <a name="example"></a><span data-ttu-id="01c89-166">Příklad</span><span class="sxs-lookup"><span data-stu-id="01c89-166">Example</span></span>

<span data-ttu-id="01c89-167">Následující příklad ukazuje, jak chcete používat funkci zprostředkovatele:</span><span class="sxs-lookup"><span data-stu-id="01c89-167">The following example shows how to use the provider function:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "providerOutput": {
            "value": "[providers('Microsoft.Web', 'sites')]",
            "type" : "object"
        }
    }
}
```

<span data-ttu-id="01c89-168">V předchozím příkladu vrátí objekt v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="01c89-168">The preceding example returns an object in the following format:</span></span>

```json
{
  "resourceType": "sites",
  "locations": [
    "South Central US",
    "North Europe",
    "West Europe",
    "Southeast Asia",
    ...
  ],
  "apiVersions": [
    "2016-08-01",
    "2016-03-01",
    "2015-08-01-preview",
    "2015-08-01",
    ...
  ]
}
```

<a id="reference" />

## <a name="reference"></a><span data-ttu-id="01c89-169">Referenční dokumentace</span><span class="sxs-lookup"><span data-stu-id="01c89-169">reference</span></span>
`reference(resourceName or resourceIdentifier, [apiVersion])`

<span data-ttu-id="01c89-170">Vrátí objekt představující stav modulu runtime prostředku.</span><span class="sxs-lookup"><span data-stu-id="01c89-170">Returns an object representing a resource's runtime state.</span></span>

### <a name="parameters"></a><span data-ttu-id="01c89-171">Parametry</span><span class="sxs-lookup"><span data-stu-id="01c89-171">Parameters</span></span>

| <span data-ttu-id="01c89-172">Parametr</span><span class="sxs-lookup"><span data-stu-id="01c89-172">Parameter</span></span> | <span data-ttu-id="01c89-173">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="01c89-173">Required</span></span> | <span data-ttu-id="01c89-174">Typ</span><span class="sxs-lookup"><span data-stu-id="01c89-174">Type</span></span> | <span data-ttu-id="01c89-175">Popis</span><span class="sxs-lookup"><span data-stu-id="01c89-175">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="01c89-176">resourceName nebo resourceIdentifier</span><span class="sxs-lookup"><span data-stu-id="01c89-176">resourceName or resourceIdentifier</span></span> |<span data-ttu-id="01c89-177">Ano</span><span class="sxs-lookup"><span data-stu-id="01c89-177">Yes</span></span> |<span data-ttu-id="01c89-178">Řetězec</span><span class="sxs-lookup"><span data-stu-id="01c89-178">string</span></span> |<span data-ttu-id="01c89-179">Název nebo identifikátor prostředku.</span><span class="sxs-lookup"><span data-stu-id="01c89-179">Name or unique identifier of a resource.</span></span> |
| <span data-ttu-id="01c89-180">apiVersion</span><span class="sxs-lookup"><span data-stu-id="01c89-180">apiVersion</span></span> |<span data-ttu-id="01c89-181">Ne</span><span class="sxs-lookup"><span data-stu-id="01c89-181">No</span></span> |<span data-ttu-id="01c89-182">Řetězec</span><span class="sxs-lookup"><span data-stu-id="01c89-182">string</span></span> |<span data-ttu-id="01c89-183">Verze rozhraní API zadaný prostředek.</span><span class="sxs-lookup"><span data-stu-id="01c89-183">API version of the specified resource.</span></span> <span data-ttu-id="01c89-184">Tento parametr zahrnout do prostředku není zřízený v rámci stejné šablony.</span><span class="sxs-lookup"><span data-stu-id="01c89-184">Include this parameter when the resource is not provisioned within same template.</span></span> <span data-ttu-id="01c89-185">Obvykle ve formátu **rrrr mm-dd**.</span><span class="sxs-lookup"><span data-stu-id="01c89-185">Typically, in the format, **yyyy-mm-dd**.</span></span> |

### <a name="return-value"></a><span data-ttu-id="01c89-186">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="01c89-186">Return value</span></span>

<span data-ttu-id="01c89-187">Každý typ prostředku vrátí různé vlastnosti pro odkaz funkce.</span><span class="sxs-lookup"><span data-stu-id="01c89-187">Every resource type returns different properties for the reference function.</span></span> <span data-ttu-id="01c89-188">Funkce nevrací předdefinovaný formát.</span><span class="sxs-lookup"><span data-stu-id="01c89-188">The function does not return a single, predefined format.</span></span> <span data-ttu-id="01c89-189">Pokud chcete zobrazit vlastnosti pro typ prostředku, vrátí objekt v části výstupy, jak je znázorněno v příkladu.</span><span class="sxs-lookup"><span data-stu-id="01c89-189">To see the properties for a resource type, return the object in the outputs section as shown in the example.</span></span>

### <a name="remarks"></a><span data-ttu-id="01c89-190">Poznámky</span><span class="sxs-lookup"><span data-stu-id="01c89-190">Remarks</span></span>

<span data-ttu-id="01c89-191">Funkce odkaz odvozuje svou hodnotu z stav modulu runtime a proto jej nelze použít v sekci proměnných.</span><span class="sxs-lookup"><span data-stu-id="01c89-191">The reference function derives its value from a runtime state, and therefore cannot be used in the variables section.</span></span> <span data-ttu-id="01c89-192">V části výstupů šablony slouží.</span><span class="sxs-lookup"><span data-stu-id="01c89-192">It can be used in outputs section of a template.</span></span> 

<span data-ttu-id="01c89-193">Pomocí funkce odkaz je implicitně deklarovat, že jeden prostředek závisí na jiný prostředek, pokud odkazovaného prostředku je zřízený v rámci stejné šablony.</span><span class="sxs-lookup"><span data-stu-id="01c89-193">By using the reference function, you implicitly declare that one resource depends on another resource if the referenced resource is provisioned within same template.</span></span> <span data-ttu-id="01c89-194">Není nutné také používat vlastnost dependsOn.</span><span class="sxs-lookup"><span data-stu-id="01c89-194">You do not need to also use the dependsOn property.</span></span> <span data-ttu-id="01c89-195">Funkce, nebude hodnocen až odkazovaných prostředků po dokončení nasazení.</span><span class="sxs-lookup"><span data-stu-id="01c89-195">The function is not evaluated until the referenced resource has completed deployment.</span></span>

<span data-ttu-id="01c89-196">Pokud chcete zobrazit názvy a hodnoty pro typ prostředku, vytvořte šablonu, která vrací objekt v části výstupy.</span><span class="sxs-lookup"><span data-stu-id="01c89-196">To see the property names and values for a resource type, create a template that returns the object in the outputs section.</span></span> <span data-ttu-id="01c89-197">Pokud máte existující prostředek daného typu, šablony vrátí objekt bez nasazení žádné nové prostředky.</span><span class="sxs-lookup"><span data-stu-id="01c89-197">If you have an existing resource of that type, your template returns the object without deploying any new resources.</span></span> 

<span data-ttu-id="01c89-198">Obvykle se používá **odkaz** funkci vrátíte konkrétní hodnotu z objektu, například identifikátor URI koncového bodu objektu blob nebo plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="01c89-198">Typically, you use the **reference** function to return a particular value from an object, such as the blob endpoint URI or fully qualified domain name.</span></span>

```json
"outputs": {
    "BlobUri": {
        "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
        "type" : "string"
    },
    "FQDN": {
        "value": "[reference(concat('Microsoft.Network/publicIPAddresses/', parameters('ipAddressName')), '2016-03-30').dnsSettings.fqdn]",
        "type" : "string"
    }
}
```

### <a name="example"></a><span data-ttu-id="01c89-199">Příklad</span><span class="sxs-lookup"><span data-stu-id="01c89-199">Example</span></span>

<span data-ttu-id="01c89-200">K nasazení a odkaz na zdroj v stejné šablony, použijte:</span><span class="sxs-lookup"><span data-stu-id="01c89-200">To deploy and reference the resource in the same template, use:</span></span>

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "storageAccountName": { 
          "type": "string"
      }
  },
  "resources": [
    {
      "name": "[parameters('storageAccountName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-12-01",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {
      "referenceOutput": {
          "type": "object",
          "value": "[reference(parameters('storageAccountName'))]"
      }
    }
}
``` 

<span data-ttu-id="01c89-201">V předchozím příkladu vrátí objekt v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="01c89-201">The preceding example returns an object in the following format:</span></span>

```json
{
   "creationTime": "2017-06-13T21:24:46.618364Z",
   "primaryEndpoints": {
     "blob": "https://examplestorage.blob.core.windows.net/",
     "file": "https://examplestorage.file.core.windows.net/",
     "queue": "https://examplestorage.queue.core.windows.net/",
     "table": "https://examplestorage.table.core.windows.net/"
   },
   "primaryLocation": "southcentralus",
   "provisioningState": "Succeeded",
   "statusOfPrimary": "available",
   "supportsHttpsTrafficOnly": false
}
```

<span data-ttu-id="01c89-202">V následujícím příkladu odkazuje na účet úložiště, který není nasazený v této šabloně.</span><span class="sxs-lookup"><span data-stu-id="01c89-202">The following example references a storage account that is not deployed in this template.</span></span> <span data-ttu-id="01c89-203">V rámci stejné skupině prostředků už existuje účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="01c89-203">The storage account already exists within the same resource group.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountName": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "ExistingStorage": {
            "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01')]",
            "type" : "object"
        }
    }
}
```

<a id="resourcegroup" />

## <a name="resourcegroup"></a><span data-ttu-id="01c89-204">Skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="01c89-204">resourceGroup</span></span>
`resourceGroup()`

<span data-ttu-id="01c89-205">Vrátí objekt, který představuje aktuální skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="01c89-205">Returns an object that represents the current resource group.</span></span> 

### <a name="return-value"></a><span data-ttu-id="01c89-206">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="01c89-206">Return value</span></span>

<span data-ttu-id="01c89-207">Vrácený objekt je v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="01c89-207">The returned object is in the following format:</span></span>

```json
{
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}",
  "name": "{resourceGroupName}",
  "location": "{resourceGroupLocation}",
  "tags": {
  },
  "properties": {
    "provisioningState": "{status}"
  }
}
```

### <a name="remarks"></a><span data-ttu-id="01c89-208">Poznámky</span><span class="sxs-lookup"><span data-stu-id="01c89-208">Remarks</span></span>

<span data-ttu-id="01c89-209">Běžně se používají funkce resourceGroup je vytvoření prostředků ve stejném umístění jako pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="01c89-209">A common use of the resourceGroup function is to create resources in the same location as the resource group.</span></span> <span data-ttu-id="01c89-210">Následující příklad používá umístění skupiny prostředků přiřadit umístění pro webový server.</span><span class="sxs-lookup"><span data-stu-id="01c89-210">The following example uses the resource group location to assign the location for a web site.</span></span>

```json
"resources": [
   {
      "apiVersion": "2016-08-01",
      "type": "Microsoft.Web/sites",
      "name": "[parameters('siteName')]",
      "location": "[resourceGroup().location]",
      ...
   }
]
```

### <a name="example"></a><span data-ttu-id="01c89-211">Příklad</span><span class="sxs-lookup"><span data-stu-id="01c89-211">Example</span></span>

<span data-ttu-id="01c89-212">Následující šablony vrací vlastnosti skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="01c89-212">The following template returns the properties of the resource group.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[resourceGroup()]",
            "type" : "object"
        }
    }
}
```

<span data-ttu-id="01c89-213">V předchozím příkladu vrátí objekt v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="01c89-213">The preceding example returns an object in the following format:</span></span>

```json
{
  "id": "/subscriptions/{subscription-id}/resourceGroups/examplegroup",
  "name": "examplegroup",
  "location": "southcentralus",
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

<a id="resourceid" />

## <a name="resourceid"></a><span data-ttu-id="01c89-214">resourceId</span><span class="sxs-lookup"><span data-stu-id="01c89-214">resourceId</span></span>
`resourceId([subscriptionId], [resourceGroupName], resourceType, resourceName1, [resourceName2]...)`

<span data-ttu-id="01c89-215">Vrací jedinečný identifikátor prostředku.</span><span class="sxs-lookup"><span data-stu-id="01c89-215">Returns the unique identifier of a resource.</span></span> <span data-ttu-id="01c89-216">Tuto funkci použít, když se název prostředku nejednoznačný nebo není zřízené v rámci stejné šablony.</span><span class="sxs-lookup"><span data-stu-id="01c89-216">You use this function when the resource name is ambiguous or not provisioned within the same template.</span></span> 

### <a name="parameters"></a><span data-ttu-id="01c89-217">Parametry</span><span class="sxs-lookup"><span data-stu-id="01c89-217">Parameters</span></span>

| <span data-ttu-id="01c89-218">Parametr</span><span class="sxs-lookup"><span data-stu-id="01c89-218">Parameter</span></span> | <span data-ttu-id="01c89-219">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="01c89-219">Required</span></span> | <span data-ttu-id="01c89-220">Typ</span><span class="sxs-lookup"><span data-stu-id="01c89-220">Type</span></span> | <span data-ttu-id="01c89-221">Popis</span><span class="sxs-lookup"><span data-stu-id="01c89-221">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="01c89-222">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="01c89-222">subscriptionId</span></span> |<span data-ttu-id="01c89-223">Ne</span><span class="sxs-lookup"><span data-stu-id="01c89-223">No</span></span> |<span data-ttu-id="01c89-224">řetězec (ve formátu identifikátoru GUID)</span><span class="sxs-lookup"><span data-stu-id="01c89-224">string (In GUID format)</span></span> |<span data-ttu-id="01c89-225">Výchozí hodnota je aktuální předplatné.</span><span class="sxs-lookup"><span data-stu-id="01c89-225">Default value is the current subscription.</span></span> <span data-ttu-id="01c89-226">Tuto hodnotu zadejte, když potřebujete načíst prostředku v jiné předplatné.</span><span class="sxs-lookup"><span data-stu-id="01c89-226">Specify this value when you need to retrieve a resource in another subscription.</span></span> |
| <span data-ttu-id="01c89-227">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="01c89-227">resourceGroupName</span></span> |<span data-ttu-id="01c89-228">Ne</span><span class="sxs-lookup"><span data-stu-id="01c89-228">No</span></span> |<span data-ttu-id="01c89-229">Řetězec</span><span class="sxs-lookup"><span data-stu-id="01c89-229">string</span></span> |<span data-ttu-id="01c89-230">Výchozí hodnota je aktuální skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="01c89-230">Default value is current resource group.</span></span> <span data-ttu-id="01c89-231">Tuto hodnotu zadejte, když potřebujete načíst prostředek v jiné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="01c89-231">Specify this value when you need to retrieve a resource in another resource group.</span></span> |
| <span data-ttu-id="01c89-232">Typ prostředku</span><span class="sxs-lookup"><span data-stu-id="01c89-232">resourceType</span></span> |<span data-ttu-id="01c89-233">Ano</span><span class="sxs-lookup"><span data-stu-id="01c89-233">Yes</span></span> |<span data-ttu-id="01c89-234">Řetězec</span><span class="sxs-lookup"><span data-stu-id="01c89-234">string</span></span> |<span data-ttu-id="01c89-235">Typ prostředku, včetně obor názvů zprostředkovatele prostředků.</span><span class="sxs-lookup"><span data-stu-id="01c89-235">Type of resource including resource provider namespace.</span></span> |
| <span data-ttu-id="01c89-236">resourceName1</span><span class="sxs-lookup"><span data-stu-id="01c89-236">resourceName1</span></span> |<span data-ttu-id="01c89-237">Ano</span><span class="sxs-lookup"><span data-stu-id="01c89-237">Yes</span></span> |<span data-ttu-id="01c89-238">Řetězec</span><span class="sxs-lookup"><span data-stu-id="01c89-238">string</span></span> |<span data-ttu-id="01c89-239">Název prostředku.</span><span class="sxs-lookup"><span data-stu-id="01c89-239">Name of resource.</span></span> |
| <span data-ttu-id="01c89-240">resourceName2</span><span class="sxs-lookup"><span data-stu-id="01c89-240">resourceName2</span></span> |<span data-ttu-id="01c89-241">Ne</span><span class="sxs-lookup"><span data-stu-id="01c89-241">No</span></span> |<span data-ttu-id="01c89-242">Řetězec</span><span class="sxs-lookup"><span data-stu-id="01c89-242">string</span></span> |<span data-ttu-id="01c89-243">Další prostředků název segment Pokud je vnořený prostředek.</span><span class="sxs-lookup"><span data-stu-id="01c89-243">Next resource name segment if resource is nested.</span></span> |

### <a name="return-value"></a><span data-ttu-id="01c89-244">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="01c89-244">Return value</span></span>

<span data-ttu-id="01c89-245">Identifikátor se vrátí v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="01c89-245">The identifier is returned in the following format:</span></span>

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

### <a name="remarks"></a><span data-ttu-id="01c89-246">Poznámky</span><span class="sxs-lookup"><span data-stu-id="01c89-246">Remarks</span></span>

<span data-ttu-id="01c89-247">Hodnoty parametrů, které zadáte závisí na tom, zda prostředek je ve stejné skupině předplatného a prostředků pro aktuální nasazení.</span><span class="sxs-lookup"><span data-stu-id="01c89-247">The parameter values you specify depend on whether the resource is in the same subscription and resource group as the current deployment.</span></span>

<span data-ttu-id="01c89-248">Pokud chcete získat ID prostředku pro účet úložiště ve stejném předplatném a skupině prostředků, použijte:</span><span class="sxs-lookup"><span data-stu-id="01c89-248">To get the resource ID for a storage account in the same subscription and resource group, use:</span></span>

```json
"[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]"
```

<span data-ttu-id="01c89-249">Pokud chcete získat ID prostředku pro účet úložiště v rámci stejného předplatného, ale jiné skupině prostředků, použijte:</span><span class="sxs-lookup"><span data-stu-id="01c89-249">To get the resource ID for a storage account in the same subscription but a different resource group, use:</span></span>

```json
"[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

<span data-ttu-id="01c89-250">Pokud chcete získat ID prostředku pro účet úložiště na jiné předplatné a skupina prostředků, použijte:</span><span class="sxs-lookup"><span data-stu-id="01c89-250">To get the resource ID for a storage account in a different subscription and resource group, use:</span></span>

```json
"[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

<span data-ttu-id="01c89-251">ID prostředku pro databázi v jiné skupině prostředků, použijte:</span><span class="sxs-lookup"><span data-stu-id="01c89-251">To get the resource ID for a database in a different resource group, use:</span></span>

```json
"[resourceId('otherResourceGroup', 'Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]"
```

<span data-ttu-id="01c89-252">Často musíte tuto funkci použít, pokud používáte účet úložiště nebo virtuální sítě ve skupině alternativní prostředků.</span><span class="sxs-lookup"><span data-stu-id="01c89-252">Often, you need to use this function when using a storage account or virtual network in an alternate resource group.</span></span> <span data-ttu-id="01c89-253">Následující příklad ukazuje, jak lze prostředků ze skupiny pro externí zdroj snadno použít:</span><span class="sxs-lookup"><span data-stu-id="01c89-253">The following example shows how a resource from an external resource group can easily be used:</span></span>

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "virtualNetworkName": {
          "type": "string"
      },
      "virtualNetworkResourceGroup": {
          "type": "string"
      },
      "subnet1Name": {
          "type": "string"
      },
      "nicName": {
          "type": "string"
      }
  },
  "variables": {
      "vnetID": "[resourceId(parameters('virtualNetworkResourceGroup'), 'Microsoft.Network/virtualNetworks', parameters('virtualNetworkName'))]",
      "subnet1Ref": "[concat(variables('vnetID'),'/subnets/', parameters('subnet1Name'))]"
  },
  "resources": [
  {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[parameters('nicName')]",
      "location": "[parameters('location')]",
      "properties": {
          "ipConfigurations": [{
              "name": "ipconfig1",
              "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "subnet": {
                      "id": "[variables('subnet1Ref')]"
                  }
              }
          }]
       }
  }]
}
```

### <a name="example"></a><span data-ttu-id="01c89-254">Příklad</span><span class="sxs-lookup"><span data-stu-id="01c89-254">Example</span></span>

<span data-ttu-id="01c89-255">Následující příklad vrací ID prostředku pro účet úložiště ve skupině prostředků:</span><span class="sxs-lookup"><span data-stu-id="01c89-255">The following example returns the resource ID for a storage account in the resource group:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "sameRGOutput": {
            "value": "[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "differentRGOutput": {
            "value": "[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "differentSubOutput": {
            "value": "[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "nestedResourceOutput": {
            "value": "[resourceId('Microsoft.SQL/servers/databases', 'serverName', 'databaseName')]",
            "type" : "string"
        }
    }
}
```

<span data-ttu-id="01c89-256">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="01c89-256">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="01c89-257">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="01c89-257">Name</span></span> | <span data-ttu-id="01c89-258">Typ</span><span class="sxs-lookup"><span data-stu-id="01c89-258">Type</span></span> | <span data-ttu-id="01c89-259">Hodnota</span><span class="sxs-lookup"><span data-stu-id="01c89-259">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="01c89-260">sameRGOutput</span><span class="sxs-lookup"><span data-stu-id="01c89-260">sameRGOutput</span></span> | <span data-ttu-id="01c89-261">Řetězec</span><span class="sxs-lookup"><span data-stu-id="01c89-261">String</span></span> | <span data-ttu-id="01c89-262">/subscriptions/{Current-Sub-ID}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span><span class="sxs-lookup"><span data-stu-id="01c89-262">/subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span></span> |
| <span data-ttu-id="01c89-263">differentRGOutput</span><span class="sxs-lookup"><span data-stu-id="01c89-263">differentRGOutput</span></span> | <span data-ttu-id="01c89-264">Řetězec</span><span class="sxs-lookup"><span data-stu-id="01c89-264">String</span></span> | <span data-ttu-id="01c89-265">/subscriptions/{Current-Sub-ID}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span><span class="sxs-lookup"><span data-stu-id="01c89-265">/subscriptions/{current-sub-id}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span></span> |
| <span data-ttu-id="01c89-266">differentSubOutput</span><span class="sxs-lookup"><span data-stu-id="01c89-266">differentSubOutput</span></span> | <span data-ttu-id="01c89-267">Řetězec</span><span class="sxs-lookup"><span data-stu-id="01c89-267">String</span></span> | <span data-ttu-id="01c89-268">/subscriptions/{different-Sub-ID}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span><span class="sxs-lookup"><span data-stu-id="01c89-268">/subscriptions/{different-sub-id}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span></span> |
| <span data-ttu-id="01c89-269">nestedResourceOutput</span><span class="sxs-lookup"><span data-stu-id="01c89-269">nestedResourceOutput</span></span> | <span data-ttu-id="01c89-270">Řetězec</span><span class="sxs-lookup"><span data-stu-id="01c89-270">String</span></span> | <span data-ttu-id="01c89-271">/subscriptions/{Current-Sub-ID}/resourceGroups/examplegroup/providers/Microsoft.SQL/Servers/servername/Databases/databaseName</span><span class="sxs-lookup"><span data-stu-id="01c89-271">/subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.SQL/servers/serverName/databases/databaseName</span></span> |

<a id="subscription" />

## <a name="subscription"></a><span data-ttu-id="01c89-272">předplatné</span><span class="sxs-lookup"><span data-stu-id="01c89-272">subscription</span></span>
`subscription()`

<span data-ttu-id="01c89-273">Vrátí podrobnosti o předplatném pro aktuální nasazení.</span><span class="sxs-lookup"><span data-stu-id="01c89-273">Returns details about the subscription for the current deployment.</span></span> 

### <a name="return-value"></a><span data-ttu-id="01c89-274">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="01c89-274">Return value</span></span>

<span data-ttu-id="01c89-275">Funkce vrátí hodnotu v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="01c89-275">The function returns the following format:</span></span>

```json
{
    "id": "/subscriptions/{subscription-id}",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}",
    "displayName": "{name-of-subscription}"
}
```

### <a name="example"></a><span data-ttu-id="01c89-276">Příklad</span><span class="sxs-lookup"><span data-stu-id="01c89-276">Example</span></span>

<span data-ttu-id="01c89-277">Následující příklad ukazuje volaná v části výstupy funkce předplatného.</span><span class="sxs-lookup"><span data-stu-id="01c89-277">The following example shows the subscription function called in the outputs section.</span></span> 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[subscription()]",
            "type" : "object"
        }
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="01c89-278">Další kroky</span><span class="sxs-lookup"><span data-stu-id="01c89-278">Next steps</span></span>
* <span data-ttu-id="01c89-279">Popis v částech šablonu Azure Resource Manager naleznete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="01c89-279">For a description of the sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="01c89-280">Sloučit několik šablon, najdete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="01c89-280">To merge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="01c89-281">K iteraci v zadaného počtu opakování při vytváření typu prostředku, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="01c89-281">To iterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="01c89-282">Postup nasazení šablony, které jste vytvořili, najdete v sekci [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="01c89-282">To see how to deploy the template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

