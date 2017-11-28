---
title: "funkce pro šablony správce prostředků ze aaaAzure - prostředky | Microsoft Docs"
description: "Popisuje funkce toouse hello hodnoty tooretrieve šablony Azure Resource Manager o prostředcích."
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
ms.openlocfilehash: c9d524b338b8b7ea6d8c9e0135d48e4fb8f167c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="resource-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="39220-103">Funkce prostředků pro šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="39220-103">Resource functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="39220-104">Resource Manager poskytuje následující funkce pro načtení prostředků hodnot hello:</span><span class="sxs-lookup"><span data-stu-id="39220-104">Resource Manager provides hello following functions for getting resource values:</span></span>

* [<span data-ttu-id="39220-105">listKeys a seznamu {Value}</span><span class="sxs-lookup"><span data-stu-id="39220-105">listKeys and list{Value}</span></span>](#listkeys)
* [<span data-ttu-id="39220-106">Zprostředkovatelé</span><span class="sxs-lookup"><span data-stu-id="39220-106">providers</span></span>](#providers)
* [<span data-ttu-id="39220-107">referenční dokumentace</span><span class="sxs-lookup"><span data-stu-id="39220-107">reference</span></span>](#reference)
* [<span data-ttu-id="39220-108">Skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="39220-108">resourceGroup</span></span>](#resourcegroup)
* [<span data-ttu-id="39220-109">ID prostředku</span><span class="sxs-lookup"><span data-stu-id="39220-109">resourceId</span></span>](#resourceid)
* [<span data-ttu-id="39220-110">předplatné</span><span class="sxs-lookup"><span data-stu-id="39220-110">subscription</span></span>](#subscription)

<span data-ttu-id="39220-111">tooget hodnoty z parametrů, proměnné nebo hello aktuální nasazení, najdete v části [funkce hodnota nasazení](resource-group-template-functions-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="39220-111">tooget values from parameters, variables, or hello current deployment, see [Deployment value functions](resource-group-template-functions-deployment.md).</span></span>

<a id="listkeys" />
<a id="list" />

## <a name="listkeys-and-listvalue"></a><span data-ttu-id="39220-112">listKeys a seznamu {Value}</span><span class="sxs-lookup"><span data-stu-id="39220-112">listKeys and list{Value}</span></span>
`listKeys(resourceName or resourceIdentifier, apiVersion)`

`list{Value}(resourceName or resourceIdentifier, apiVersion)`

<span data-ttu-id="39220-113">Vrátí hello hodnoty pro libovolný typ prostředku, který podporuje hello seznamu operaci.</span><span class="sxs-lookup"><span data-stu-id="39220-113">Returns hello values for any resource type that supports hello list operation.</span></span> <span data-ttu-id="39220-114">Nejběžnější využití Hello `listKeys`.</span><span class="sxs-lookup"><span data-stu-id="39220-114">hello most common usage is `listKeys`.</span></span> 

### <a name="parameters"></a><span data-ttu-id="39220-115">Parametry</span><span class="sxs-lookup"><span data-stu-id="39220-115">Parameters</span></span>

| <span data-ttu-id="39220-116">Parametr</span><span class="sxs-lookup"><span data-stu-id="39220-116">Parameter</span></span> | <span data-ttu-id="39220-117">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="39220-117">Required</span></span> | <span data-ttu-id="39220-118">Typ</span><span class="sxs-lookup"><span data-stu-id="39220-118">Type</span></span> | <span data-ttu-id="39220-119">Popis</span><span class="sxs-lookup"><span data-stu-id="39220-119">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="39220-120">resourceName nebo resourceIdentifier</span><span class="sxs-lookup"><span data-stu-id="39220-120">resourceName or resourceIdentifier</span></span> |<span data-ttu-id="39220-121">Ano</span><span class="sxs-lookup"><span data-stu-id="39220-121">Yes</span></span> |<span data-ttu-id="39220-122">Řetězec</span><span class="sxs-lookup"><span data-stu-id="39220-122">string</span></span> |<span data-ttu-id="39220-123">Jedinečný identifikátor pro prostředek hello.</span><span class="sxs-lookup"><span data-stu-id="39220-123">Unique identifier for hello resource.</span></span> |
| <span data-ttu-id="39220-124">apiVersion</span><span class="sxs-lookup"><span data-stu-id="39220-124">apiVersion</span></span> |<span data-ttu-id="39220-125">Ano</span><span class="sxs-lookup"><span data-stu-id="39220-125">Yes</span></span> |<span data-ttu-id="39220-126">Řetězec</span><span class="sxs-lookup"><span data-stu-id="39220-126">string</span></span> |<span data-ttu-id="39220-127">Verze rozhraní API stav modulu runtime prostředků.</span><span class="sxs-lookup"><span data-stu-id="39220-127">API version of resource runtime state.</span></span> <span data-ttu-id="39220-128">Obvykle ve formátu hello **rrrr mm-dd**.</span><span class="sxs-lookup"><span data-stu-id="39220-128">Typically, in hello format, **yyyy-mm-dd**.</span></span> |

### <a name="return-value"></a><span data-ttu-id="39220-129">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="39220-129">Return value</span></span>

<span data-ttu-id="39220-130">Hello vrátit objekt z listKeys má hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="39220-130">hello returned object from listKeys has hello following format:</span></span>

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

<span data-ttu-id="39220-131">Jiné funkce seznamu mají návratový různých formátech.</span><span class="sxs-lookup"><span data-stu-id="39220-131">Other list functions have different return formats.</span></span> <span data-ttu-id="39220-132">Formát hello toosee funkce, její zahrnutí do části výstupy hello jak je znázorněno v příkladu šablony hello.</span><span class="sxs-lookup"><span data-stu-id="39220-132">toosee hello format of a function, include it in hello outputs section as shown in hello example template.</span></span> 

### <a name="remarks"></a><span data-ttu-id="39220-133">Poznámky</span><span class="sxs-lookup"><span data-stu-id="39220-133">Remarks</span></span>

<span data-ttu-id="39220-134">Všechny operace, který začíná **seznamu** lze použít jako funkci ve vaší šabloně.</span><span class="sxs-lookup"><span data-stu-id="39220-134">Any operation that starts with **list** can be used as a function in your template.</span></span> <span data-ttu-id="39220-135">Hello k dispozici operace zahrnují nejen listKeys, ale také operace jako `list`, `listAdminKeys`, a `listStatus`.</span><span class="sxs-lookup"><span data-stu-id="39220-135">hello available operations include not only listKeys, but also operations like `list`, `listAdminKeys`, and `listStatus`.</span></span> <span data-ttu-id="39220-136">Ale nemůžete použít **seznamu** činnosti, které vyžadují hodnoty v hello text žádosti.</span><span class="sxs-lookup"><span data-stu-id="39220-136">However, you cannot use **list** operations that require values in hello request body.</span></span> <span data-ttu-id="39220-137">Například hello [SAS účtu seznamu](/rest/api/storagerp/storageaccounts#StorageAccounts_ListAccountSAS) operace vyžaduje parametry text požadavku jako *signedExpiry*, takže ji nelze použít v rámci šablon.</span><span class="sxs-lookup"><span data-stu-id="39220-137">For example, hello [List Account SAS](/rest/api/storagerp/storageaccounts#StorageAccounts_ListAccountSAS) operation requires request body parameters like *signedExpiry*, so you cannot use it within a template.</span></span>

<span data-ttu-id="39220-138">jaké typy prostředků obsahovat operaci seznamu toodetermine, máte hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="39220-138">toodetermine which resource types have a list operation, you have hello following options:</span></span>

* <span data-ttu-id="39220-139">Zobrazení hello [operace REST API](/rest/api/) pro poskytovatele prostředků a podívejte se na seznam operací.</span><span class="sxs-lookup"><span data-stu-id="39220-139">View hello [REST API operations](/rest/api/) for a resource provider, and look for list operations.</span></span> <span data-ttu-id="39220-140">Účty úložiště mít například hello [listKeys operaci](/rest/api/storagerp/storageaccounts#StorageAccounts_ListKeys).</span><span class="sxs-lookup"><span data-stu-id="39220-140">For example, storage accounts have hello [listKeys operation](/rest/api/storagerp/storageaccounts#StorageAccounts_ListKeys).</span></span>
* <span data-ttu-id="39220-141">Použití hello [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="39220-141">Use hello [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) PowerShell cmdlet.</span></span> <span data-ttu-id="39220-142">Hello následující příklad načte všechny operace výpisu pro účty úložiště:</span><span class="sxs-lookup"><span data-stu-id="39220-142">hello following example gets all list operations for storage accounts:</span></span>

  ```powershell
  Get-AzureRmProviderOperation -OperationSearchString "Microsoft.Storage/*" | where {$_.Operation -like "*list*"} | FT Operation
  ```
* <span data-ttu-id="39220-143">Použijte následující příkaz rozhraní příkazového řádku Azure toofilter pouze hello operace výpisu hello:</span><span class="sxs-lookup"><span data-stu-id="39220-143">Use hello following Azure CLI command toofilter only hello list operations:</span></span>

  ```azurecli
  az provider operation show --namespace Microsoft.Storage --query "resourceTypes[?name=='storageAccounts'].operations[].name | [?contains(@, 'list')]"
  ```

<span data-ttu-id="39220-144">Zadejte hello prostředků pomocí buď hello [resourceId funkce](#resourceid), nebo hello formát `{providerNamespace}/{resourceType}/{resourceName}`.</span><span class="sxs-lookup"><span data-stu-id="39220-144">Specify hello resource by using either hello [resourceId function](#resourceid), or hello format `{providerNamespace}/{resourceType}/{resourceName}`.</span></span>


### <a name="example"></a><span data-ttu-id="39220-145">Příklad</span><span class="sxs-lookup"><span data-stu-id="39220-145">Example</span></span>

<span data-ttu-id="39220-146">Hello následující příklad ukazuje, jak tooreturn hello primární a sekundární klíče z účtu úložiště v hello výstupy části.</span><span class="sxs-lookup"><span data-stu-id="39220-146">hello following example shows how tooreturn hello primary and secondary keys from a storage account in hello outputs section.</span></span>

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

## <a name="providers"></a><span data-ttu-id="39220-147">Zprostředkovatelé</span><span class="sxs-lookup"><span data-stu-id="39220-147">providers</span></span>
`providers(providerNamespace, [resourceType])`

<span data-ttu-id="39220-148">Vrátí informace o poskytovatele prostředků a jeho typy podporovaných zdrojů.</span><span class="sxs-lookup"><span data-stu-id="39220-148">Returns information about a resource provider and its supported resource types.</span></span> <span data-ttu-id="39220-149">Pokud není zadán typ prostředku, vrátí funkce hello všechny typy hello podporované pro zprostředkovatele prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="39220-149">If you do not provide a resource type, hello function returns all hello supported types for hello resource provider.</span></span>

### <a name="parameters"></a><span data-ttu-id="39220-150">Parametry</span><span class="sxs-lookup"><span data-stu-id="39220-150">Parameters</span></span>

| <span data-ttu-id="39220-151">Parametr</span><span class="sxs-lookup"><span data-stu-id="39220-151">Parameter</span></span> | <span data-ttu-id="39220-152">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="39220-152">Required</span></span> | <span data-ttu-id="39220-153">Typ</span><span class="sxs-lookup"><span data-stu-id="39220-153">Type</span></span> | <span data-ttu-id="39220-154">Popis</span><span class="sxs-lookup"><span data-stu-id="39220-154">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="39220-155">providerNamespace</span><span class="sxs-lookup"><span data-stu-id="39220-155">providerNamespace</span></span> |<span data-ttu-id="39220-156">Ano</span><span class="sxs-lookup"><span data-stu-id="39220-156">Yes</span></span> |<span data-ttu-id="39220-157">Řetězec</span><span class="sxs-lookup"><span data-stu-id="39220-157">string</span></span> |<span data-ttu-id="39220-158">Namespace hello zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="39220-158">Namespace of hello provider</span></span> |
| <span data-ttu-id="39220-159">Typ prostředku</span><span class="sxs-lookup"><span data-stu-id="39220-159">resourceType</span></span> |<span data-ttu-id="39220-160">Ne</span><span class="sxs-lookup"><span data-stu-id="39220-160">No</span></span> |<span data-ttu-id="39220-161">Řetězec</span><span class="sxs-lookup"><span data-stu-id="39220-161">string</span></span> |<span data-ttu-id="39220-162">Typ prostředku v rámci hello Hello zadat obor názvů.</span><span class="sxs-lookup"><span data-stu-id="39220-162">hello type of resource within hello specified namespace.</span></span> |

### <a name="return-value"></a><span data-ttu-id="39220-163">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="39220-163">Return value</span></span>

<span data-ttu-id="39220-164">Každý podporovaný typ je vrácený v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="39220-164">Each supported type is returned in hello following format:</span></span> 

```json
{
    "resourceType": "{name of resource type}",
    "locations": [ all supported locations ],
    "apiVersions": [ all supported API versions ]
}
```

<span data-ttu-id="39220-165">Pole řazení hello vrátil hodnoty není zaručena.</span><span class="sxs-lookup"><span data-stu-id="39220-165">Array ordering of hello returned values is not guaranteed.</span></span>

### <a name="example"></a><span data-ttu-id="39220-166">Příklad</span><span class="sxs-lookup"><span data-stu-id="39220-166">Example</span></span>

<span data-ttu-id="39220-167">Hello následující příklad ukazuje, jak toouse hello zprostředkovatele funkce:</span><span class="sxs-lookup"><span data-stu-id="39220-167">hello following example shows how toouse hello provider function:</span></span>

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

<span data-ttu-id="39220-168">Hello předchozí příklad vrátí objekt ve formátu hello:</span><span class="sxs-lookup"><span data-stu-id="39220-168">hello preceding example returns an object in hello following format:</span></span>

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

## <a name="reference"></a><span data-ttu-id="39220-169">Referenční dokumentace</span><span class="sxs-lookup"><span data-stu-id="39220-169">reference</span></span>
`reference(resourceName or resourceIdentifier, [apiVersion])`

<span data-ttu-id="39220-170">Vrátí objekt představující stav modulu runtime prostředku.</span><span class="sxs-lookup"><span data-stu-id="39220-170">Returns an object representing a resource's runtime state.</span></span>

### <a name="parameters"></a><span data-ttu-id="39220-171">Parametry</span><span class="sxs-lookup"><span data-stu-id="39220-171">Parameters</span></span>

| <span data-ttu-id="39220-172">Parametr</span><span class="sxs-lookup"><span data-stu-id="39220-172">Parameter</span></span> | <span data-ttu-id="39220-173">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="39220-173">Required</span></span> | <span data-ttu-id="39220-174">Typ</span><span class="sxs-lookup"><span data-stu-id="39220-174">Type</span></span> | <span data-ttu-id="39220-175">Popis</span><span class="sxs-lookup"><span data-stu-id="39220-175">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="39220-176">resourceName nebo resourceIdentifier</span><span class="sxs-lookup"><span data-stu-id="39220-176">resourceName or resourceIdentifier</span></span> |<span data-ttu-id="39220-177">Ano</span><span class="sxs-lookup"><span data-stu-id="39220-177">Yes</span></span> |<span data-ttu-id="39220-178">Řetězec</span><span class="sxs-lookup"><span data-stu-id="39220-178">string</span></span> |<span data-ttu-id="39220-179">Název nebo identifikátor prostředku.</span><span class="sxs-lookup"><span data-stu-id="39220-179">Name or unique identifier of a resource.</span></span> |
| <span data-ttu-id="39220-180">apiVersion</span><span class="sxs-lookup"><span data-stu-id="39220-180">apiVersion</span></span> |<span data-ttu-id="39220-181">Ne</span><span class="sxs-lookup"><span data-stu-id="39220-181">No</span></span> |<span data-ttu-id="39220-182">Řetězec</span><span class="sxs-lookup"><span data-stu-id="39220-182">string</span></span> |<span data-ttu-id="39220-183">Verze rozhraní API hello zadaný prostředek.</span><span class="sxs-lookup"><span data-stu-id="39220-183">API version of hello specified resource.</span></span> <span data-ttu-id="39220-184">Zahrnout tento parametr hello prostředků není zřízený v rámci stejné šablony.</span><span class="sxs-lookup"><span data-stu-id="39220-184">Include this parameter when hello resource is not provisioned within same template.</span></span> <span data-ttu-id="39220-185">Obvykle ve formátu hello **rrrr mm-dd**.</span><span class="sxs-lookup"><span data-stu-id="39220-185">Typically, in hello format, **yyyy-mm-dd**.</span></span> |

### <a name="return-value"></a><span data-ttu-id="39220-186">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="39220-186">Return value</span></span>

<span data-ttu-id="39220-187">Každý typ prostředku vrátí různé vlastnosti pro hello odkaz funkce.</span><span class="sxs-lookup"><span data-stu-id="39220-187">Every resource type returns different properties for hello reference function.</span></span> <span data-ttu-id="39220-188">Funkce Hello nevrací předdefinovaný formát.</span><span class="sxs-lookup"><span data-stu-id="39220-188">hello function does not return a single, predefined format.</span></span> <span data-ttu-id="39220-189">toosee hello vlastnosti pro typ prostředku, vrátí, že objekt hello v hello výstupy části, jak je znázorněno v příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="39220-189">toosee hello properties for a resource type, return hello object in hello outputs section as shown in hello example.</span></span>

### <a name="remarks"></a><span data-ttu-id="39220-190">Poznámky</span><span class="sxs-lookup"><span data-stu-id="39220-190">Remarks</span></span>

<span data-ttu-id="39220-191">Funkce odkaz Hello odvozuje svou hodnotu z stav modulu runtime a proto jej nelze použít v části proměnných hello.</span><span class="sxs-lookup"><span data-stu-id="39220-191">hello reference function derives its value from a runtime state, and therefore cannot be used in hello variables section.</span></span> <span data-ttu-id="39220-192">V části výstupů šablony slouží.</span><span class="sxs-lookup"><span data-stu-id="39220-192">It can be used in outputs section of a template.</span></span> 

<span data-ttu-id="39220-193">Pomocí funkce odkaz hello je implicitně deklarovat, že jeden prostředek závisí na jiný prostředek, pokud hello odkazuje prostředků je zřízený v rámci stejné šablony.</span><span class="sxs-lookup"><span data-stu-id="39220-193">By using hello reference function, you implicitly declare that one resource depends on another resource if hello referenced resource is provisioned within same template.</span></span> <span data-ttu-id="39220-194">Není nutné tooalso použití hello dependsOn vlastnost.</span><span class="sxs-lookup"><span data-stu-id="39220-194">You do not need tooalso use hello dependsOn property.</span></span> <span data-ttu-id="39220-195">Hello funkce, nebude hodnocen až hello odkazovaných prostředků po dokončení nasazení.</span><span class="sxs-lookup"><span data-stu-id="39220-195">hello function is not evaluated until hello referenced resource has completed deployment.</span></span>

<span data-ttu-id="39220-196">toosee hello názvy a hodnoty vlastností pro typ prostředku, vytvořit šablonu, která vrací objekt hello v části výstupy hello.</span><span class="sxs-lookup"><span data-stu-id="39220-196">toosee hello property names and values for a resource type, create a template that returns hello object in hello outputs section.</span></span> <span data-ttu-id="39220-197">Pokud máte existující prostředek daného typu, šablony vrátí objekt hello bez nasazení žádné nové prostředky.</span><span class="sxs-lookup"><span data-stu-id="39220-197">If you have an existing resource of that type, your template returns hello object without deploying any new resources.</span></span> 

<span data-ttu-id="39220-198">Obvykle použijete hello **odkaz** funkce tooreturn určitou hodnotu z objektu, například koncový bod objektů blob hello URI nebo plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="39220-198">Typically, you use hello **reference** function tooreturn a particular value from an object, such as hello blob endpoint URI or fully qualified domain name.</span></span>

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

### <a name="example"></a><span data-ttu-id="39220-199">Příklad</span><span class="sxs-lookup"><span data-stu-id="39220-199">Example</span></span>

<span data-ttu-id="39220-200">referenční dokumentace a toodeploy hello prostředku v hello stejné šablony, použijte:</span><span class="sxs-lookup"><span data-stu-id="39220-200">toodeploy and reference hello resource in hello same template, use:</span></span>

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

<span data-ttu-id="39220-201">Hello předchozí příklad vrátí objekt ve formátu hello:</span><span class="sxs-lookup"><span data-stu-id="39220-201">hello preceding example returns an object in hello following format:</span></span>

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

<span data-ttu-id="39220-202">Hello následujícím příkladu odkazuje na účet úložiště, který není nasazený v této šabloně.</span><span class="sxs-lookup"><span data-stu-id="39220-202">hello following example references a storage account that is not deployed in this template.</span></span> <span data-ttu-id="39220-203">Hello účet úložiště už existuje v rámci hello stejné skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="39220-203">hello storage account already exists within hello same resource group.</span></span>

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

## <a name="resourcegroup"></a><span data-ttu-id="39220-204">Skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="39220-204">resourceGroup</span></span>
`resourceGroup()`

<span data-ttu-id="39220-205">Vrátí objekt, který představuje aktuální skupině prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="39220-205">Returns an object that represents hello current resource group.</span></span> 

### <a name="return-value"></a><span data-ttu-id="39220-206">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="39220-206">Return value</span></span>

<span data-ttu-id="39220-207">Hello vrátit objekt je ve formátu hello:</span><span class="sxs-lookup"><span data-stu-id="39220-207">hello returned object is in hello following format:</span></span>

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

### <a name="remarks"></a><span data-ttu-id="39220-208">Poznámky</span><span class="sxs-lookup"><span data-stu-id="39220-208">Remarks</span></span>

<span data-ttu-id="39220-209">Běžně se používají funkce resourceGroup hello je toocreate prostředky v hello stejné umístění jako skupina prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="39220-209">A common use of hello resourceGroup function is toocreate resources in hello same location as hello resource group.</span></span> <span data-ttu-id="39220-210">Hello následující příklad používá umístění tooassign hello skupiny umístění hello prostředků pro webovou stránku.</span><span class="sxs-lookup"><span data-stu-id="39220-210">hello following example uses hello resource group location tooassign hello location for a web site.</span></span>

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

### <a name="example"></a><span data-ttu-id="39220-211">Příklad</span><span class="sxs-lookup"><span data-stu-id="39220-211">Example</span></span>

<span data-ttu-id="39220-212">Hello následující šablony vrátí hello vlastnosti skupiny zdrojů hello.</span><span class="sxs-lookup"><span data-stu-id="39220-212">hello following template returns hello properties of hello resource group.</span></span>

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

<span data-ttu-id="39220-213">Hello předchozí příklad vrátí objekt ve formátu hello:</span><span class="sxs-lookup"><span data-stu-id="39220-213">hello preceding example returns an object in hello following format:</span></span>

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

## <a name="resourceid"></a><span data-ttu-id="39220-214">resourceId</span><span class="sxs-lookup"><span data-stu-id="39220-214">resourceId</span></span>
`resourceId([subscriptionId], [resourceGroupName], resourceType, resourceName1, [resourceName2]...)`

<span data-ttu-id="39220-215">Vrátí hello jedinečný identifikátor prostředku.</span><span class="sxs-lookup"><span data-stu-id="39220-215">Returns hello unique identifier of a resource.</span></span> <span data-ttu-id="39220-216">Tuto funkci použít, když se název prostředku hello nejednoznačný nebo není zřízené v rámci hello stejné šablony.</span><span class="sxs-lookup"><span data-stu-id="39220-216">You use this function when hello resource name is ambiguous or not provisioned within hello same template.</span></span> 

### <a name="parameters"></a><span data-ttu-id="39220-217">Parametry</span><span class="sxs-lookup"><span data-stu-id="39220-217">Parameters</span></span>

| <span data-ttu-id="39220-218">Parametr</span><span class="sxs-lookup"><span data-stu-id="39220-218">Parameter</span></span> | <span data-ttu-id="39220-219">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="39220-219">Required</span></span> | <span data-ttu-id="39220-220">Typ</span><span class="sxs-lookup"><span data-stu-id="39220-220">Type</span></span> | <span data-ttu-id="39220-221">Popis</span><span class="sxs-lookup"><span data-stu-id="39220-221">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="39220-222">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="39220-222">subscriptionId</span></span> |<span data-ttu-id="39220-223">Ne</span><span class="sxs-lookup"><span data-stu-id="39220-223">No</span></span> |<span data-ttu-id="39220-224">řetězec (ve formátu identifikátoru GUID)</span><span class="sxs-lookup"><span data-stu-id="39220-224">string (In GUID format)</span></span> |<span data-ttu-id="39220-225">Výchozí hodnota je aktuální předplatné hello.</span><span class="sxs-lookup"><span data-stu-id="39220-225">Default value is hello current subscription.</span></span> <span data-ttu-id="39220-226">Tuto hodnotu zadejte, když potřebujete tooretrieve prostředku v jiné předplatné.</span><span class="sxs-lookup"><span data-stu-id="39220-226">Specify this value when you need tooretrieve a resource in another subscription.</span></span> |
| <span data-ttu-id="39220-227">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="39220-227">resourceGroupName</span></span> |<span data-ttu-id="39220-228">Ne</span><span class="sxs-lookup"><span data-stu-id="39220-228">No</span></span> |<span data-ttu-id="39220-229">Řetězec</span><span class="sxs-lookup"><span data-stu-id="39220-229">string</span></span> |<span data-ttu-id="39220-230">Výchozí hodnota je aktuální skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="39220-230">Default value is current resource group.</span></span> <span data-ttu-id="39220-231">Tuto hodnotu zadejte, když potřebujete tooretrieve prostředku v jiné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="39220-231">Specify this value when you need tooretrieve a resource in another resource group.</span></span> |
| <span data-ttu-id="39220-232">Typ prostředku</span><span class="sxs-lookup"><span data-stu-id="39220-232">resourceType</span></span> |<span data-ttu-id="39220-233">Ano</span><span class="sxs-lookup"><span data-stu-id="39220-233">Yes</span></span> |<span data-ttu-id="39220-234">Řetězec</span><span class="sxs-lookup"><span data-stu-id="39220-234">string</span></span> |<span data-ttu-id="39220-235">Typ prostředku, včetně obor názvů zprostředkovatele prostředků.</span><span class="sxs-lookup"><span data-stu-id="39220-235">Type of resource including resource provider namespace.</span></span> |
| <span data-ttu-id="39220-236">resourceName1</span><span class="sxs-lookup"><span data-stu-id="39220-236">resourceName1</span></span> |<span data-ttu-id="39220-237">Ano</span><span class="sxs-lookup"><span data-stu-id="39220-237">Yes</span></span> |<span data-ttu-id="39220-238">Řetězec</span><span class="sxs-lookup"><span data-stu-id="39220-238">string</span></span> |<span data-ttu-id="39220-239">Název prostředku.</span><span class="sxs-lookup"><span data-stu-id="39220-239">Name of resource.</span></span> |
| <span data-ttu-id="39220-240">resourceName2</span><span class="sxs-lookup"><span data-stu-id="39220-240">resourceName2</span></span> |<span data-ttu-id="39220-241">Ne</span><span class="sxs-lookup"><span data-stu-id="39220-241">No</span></span> |<span data-ttu-id="39220-242">Řetězec</span><span class="sxs-lookup"><span data-stu-id="39220-242">string</span></span> |<span data-ttu-id="39220-243">Další prostředků název segment Pokud je vnořený prostředek.</span><span class="sxs-lookup"><span data-stu-id="39220-243">Next resource name segment if resource is nested.</span></span> |

### <a name="return-value"></a><span data-ttu-id="39220-244">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="39220-244">Return value</span></span>

<span data-ttu-id="39220-245">identifikátor Hello je vrácený v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="39220-245">hello identifier is returned in hello following format:</span></span>

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

### <a name="remarks"></a><span data-ttu-id="39220-246">Poznámky</span><span class="sxs-lookup"><span data-stu-id="39220-246">Remarks</span></span>

<span data-ttu-id="39220-247">Hello hodnoty parametrů můžete zadat závisí na tom, zda text hello prostředků v hello stejné předplatném nebo skupině prostředků jako hello aktuální nasazení.</span><span class="sxs-lookup"><span data-stu-id="39220-247">hello parameter values you specify depend on whether hello resource is in hello same subscription and resource group as hello current deployment.</span></span>

<span data-ttu-id="39220-248">tooget hello prostředků ID pro účet úložiště na hello stejné předplatné a skupina prostředků, použijte:</span><span class="sxs-lookup"><span data-stu-id="39220-248">tooget hello resource ID for a storage account in hello same subscription and resource group, use:</span></span>

```json
"[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]"
```

<span data-ttu-id="39220-249">ID prostředku hello tooget pro účet úložiště na hello stejného předplatného, ale jiné skupině prostředků, použijte:</span><span class="sxs-lookup"><span data-stu-id="39220-249">tooget hello resource ID for a storage account in hello same subscription but a different resource group, use:</span></span>

```json
"[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

<span data-ttu-id="39220-250">ID prostředku hello tooget pro účet úložiště na jiné předplatné a skupina prostředků, použijte:</span><span class="sxs-lookup"><span data-stu-id="39220-250">tooget hello resource ID for a storage account in a different subscription and resource group, use:</span></span>

```json
"[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

<span data-ttu-id="39220-251">ID prostředku hello tooget pro databázi v jiné skupině prostředků, použijte:</span><span class="sxs-lookup"><span data-stu-id="39220-251">tooget hello resource ID for a database in a different resource group, use:</span></span>

```json
"[resourceId('otherResourceGroup', 'Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]"
```

<span data-ttu-id="39220-252">Často musíte toouse tuto funkci při použití účtu úložiště nebo virtuální sítě ve skupině alternativní prostředků.</span><span class="sxs-lookup"><span data-stu-id="39220-252">Often, you need toouse this function when using a storage account or virtual network in an alternate resource group.</span></span> <span data-ttu-id="39220-253">Hello následující příklad ukazuje, jak lze prostředků ze skupiny pro externí zdroj snadno použít:</span><span class="sxs-lookup"><span data-stu-id="39220-253">hello following example shows how a resource from an external resource group can easily be used:</span></span>

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

### <a name="example"></a><span data-ttu-id="39220-254">Příklad</span><span class="sxs-lookup"><span data-stu-id="39220-254">Example</span></span>

<span data-ttu-id="39220-255">Hello následující příklad vrátí hello ID prostředku pro účet úložiště ve skupině prostředků hello:</span><span class="sxs-lookup"><span data-stu-id="39220-255">hello following example returns hello resource ID for a storage account in hello resource group:</span></span>

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

<span data-ttu-id="39220-256">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="39220-256">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="39220-257">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="39220-257">Name</span></span> | <span data-ttu-id="39220-258">Typ</span><span class="sxs-lookup"><span data-stu-id="39220-258">Type</span></span> | <span data-ttu-id="39220-259">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39220-259">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="39220-260">sameRGOutput</span><span class="sxs-lookup"><span data-stu-id="39220-260">sameRGOutput</span></span> | <span data-ttu-id="39220-261">Řetězec</span><span class="sxs-lookup"><span data-stu-id="39220-261">String</span></span> | <span data-ttu-id="39220-262">/subscriptions/{Current-Sub-ID}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span><span class="sxs-lookup"><span data-stu-id="39220-262">/subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span></span> |
| <span data-ttu-id="39220-263">differentRGOutput</span><span class="sxs-lookup"><span data-stu-id="39220-263">differentRGOutput</span></span> | <span data-ttu-id="39220-264">Řetězec</span><span class="sxs-lookup"><span data-stu-id="39220-264">String</span></span> | <span data-ttu-id="39220-265">/subscriptions/{Current-Sub-ID}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span><span class="sxs-lookup"><span data-stu-id="39220-265">/subscriptions/{current-sub-id}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span></span> |
| <span data-ttu-id="39220-266">differentSubOutput</span><span class="sxs-lookup"><span data-stu-id="39220-266">differentSubOutput</span></span> | <span data-ttu-id="39220-267">Řetězec</span><span class="sxs-lookup"><span data-stu-id="39220-267">String</span></span> | <span data-ttu-id="39220-268">/subscriptions/{different-Sub-ID}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span><span class="sxs-lookup"><span data-stu-id="39220-268">/subscriptions/{different-sub-id}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span></span> |
| <span data-ttu-id="39220-269">nestedResourceOutput</span><span class="sxs-lookup"><span data-stu-id="39220-269">nestedResourceOutput</span></span> | <span data-ttu-id="39220-270">Řetězec</span><span class="sxs-lookup"><span data-stu-id="39220-270">String</span></span> | <span data-ttu-id="39220-271">/subscriptions/{Current-Sub-ID}/resourceGroups/examplegroup/providers/Microsoft.SQL/Servers/servername/Databases/databaseName</span><span class="sxs-lookup"><span data-stu-id="39220-271">/subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.SQL/servers/serverName/databases/databaseName</span></span> |

<a id="subscription" />

## <a name="subscription"></a><span data-ttu-id="39220-272">předplatné</span><span class="sxs-lookup"><span data-stu-id="39220-272">subscription</span></span>
`subscription()`

<span data-ttu-id="39220-273">Vrátí podrobnosti o předplatném hello hello aktuální nasazení.</span><span class="sxs-lookup"><span data-stu-id="39220-273">Returns details about hello subscription for hello current deployment.</span></span> 

### <a name="return-value"></a><span data-ttu-id="39220-274">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="39220-274">Return value</span></span>

<span data-ttu-id="39220-275">Funkce Hello vrací hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="39220-275">hello function returns hello following format:</span></span>

```json
{
    "id": "/subscriptions/{subscription-id}",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}",
    "displayName": "{name-of-subscription}"
}
```

### <a name="example"></a><span data-ttu-id="39220-276">Příklad</span><span class="sxs-lookup"><span data-stu-id="39220-276">Example</span></span>

<span data-ttu-id="39220-277">Hello následující příklad ukazuje funkce předplatné hello volat v části výstupy hello.</span><span class="sxs-lookup"><span data-stu-id="39220-277">hello following example shows hello subscription function called in hello outputs section.</span></span> 

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

## <a name="next-steps"></a><span data-ttu-id="39220-278">Další kroky</span><span class="sxs-lookup"><span data-stu-id="39220-278">Next steps</span></span>
* <span data-ttu-id="39220-279">Popis části hello šablonu Azure Resource Manager naleznete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="39220-279">For a description of hello sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="39220-280">toomerge několik šablon, najdete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="39220-280">toomerge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="39220-281">tooiterate zadaného počtu opakování při vytváření typu prostředku, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="39220-281">tooiterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="39220-282">toosee způsobu toodeploy hello šablony vytvoříte, najdete v [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="39220-282">toosee how toodeploy hello template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

