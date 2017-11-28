---
title: "funkce pro šablony Resource Manageru ze aaaAzure - nasazení | Microsoft Docs"
description: "Popisuje funkce toouse hello v informace tooretrieve nasazení šablony Azure Resource Manager."
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
ms.date: 06/13/2017
ms.author: tomfitz
ms.openlocfilehash: 458c3f740504fdd6799ed24cc386219726737636
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deployment-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="425c5-103">Nasazení funkce pro šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="425c5-103">Deployment functions for Azure Resource Manager templates</span></span> 

<span data-ttu-id="425c5-104">Resource Manager poskytuje hello následující funkce pro získání hodnoty z části hello šablony a hodnoty související toohello nasazení:</span><span class="sxs-lookup"><span data-stu-id="425c5-104">Resource Manager provides hello following functions for getting values from sections of hello template and values related toohello deployment:</span></span>

* [<span data-ttu-id="425c5-105">nasazení</span><span class="sxs-lookup"><span data-stu-id="425c5-105">deployment</span></span>](#deployment)
* [<span data-ttu-id="425c5-106">Parametry</span><span class="sxs-lookup"><span data-stu-id="425c5-106">parameters</span></span>](#parameters)
* [<span data-ttu-id="425c5-107">proměnné</span><span class="sxs-lookup"><span data-stu-id="425c5-107">variables</span></span>](#variables)

<span data-ttu-id="425c5-108">tooget hodnoty z prostředků, skupiny prostředků nebo předplatného, najdete v části [prostředků funkce](resource-group-template-functions-resource.md).</span><span class="sxs-lookup"><span data-stu-id="425c5-108">tooget values from resources, resource groups, or subscriptions, see [Resource functions](resource-group-template-functions-resource.md).</span></span>

<a id="deployment" />

## <a name="deployment"></a><span data-ttu-id="425c5-109">nasazení</span><span class="sxs-lookup"><span data-stu-id="425c5-109">deployment</span></span>
`deployment()`

<span data-ttu-id="425c5-110">Vrací informace o aktuální operace nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="425c5-110">Returns information about hello current deployment operation.</span></span>

### <a name="return-value"></a><span data-ttu-id="425c5-111">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="425c5-111">Return value</span></span>

<span data-ttu-id="425c5-112">Tato funkce vrátí hello objekt, který je předán během nasazení.</span><span class="sxs-lookup"><span data-stu-id="425c5-112">This function returns hello object that is passed during deployment.</span></span> <span data-ttu-id="425c5-113">Vlastnosti Hello v hello vrátil objekt se liší v závislosti na tom, jestli hello nasazení je předán objekt jako odkaz nebo jako objekt v řádku.</span><span class="sxs-lookup"><span data-stu-id="425c5-113">hello properties in hello returned object differ based on whether hello deployment object is passed as a link or as an in-line object.</span></span> <span data-ttu-id="425c5-114">Pokud objekt nasazení hello je předán v řádku, jako třeba při použití hello **- TemplateFile** parametr v prostředí Azure PowerShell toopoint tooa místního souboru, hello vrátí objekt má hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="425c5-114">When hello deployment object is passed in-line, such as when using hello **-TemplateFile** parameter in Azure PowerShell toopoint tooa local file, hello returned object has hello following format:</span></span>

```json
{
    "name": "",
    "properties": {
        "template": {
            "$schema": "",
            "contentVersion": "",
            "parameters": {},
            "variables": {},
            "resources": [
            ],
            "outputs": {}
        },
        "parameters": {},
        "mode": "",
        "provisioningState": ""
    }
}
```

<span data-ttu-id="425c5-115">Pokud objekt hello je předán jako odkaz, například při použití hello **- TemplateUri** parametr toopoint tooa vzdáleného objektu, je vrácen objekt hello v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="425c5-115">When hello object is passed as a link, such as when using hello **-TemplateUri** parameter toopoint tooa remote object, hello object is returned in hello following format:</span></span> 

```json
{
    "name": "",
    "properties": {
        "templateLink": {
            "uri": ""
        },
        "template": {
            "$schema": "",
            "contentVersion": "",
            "parameters": {},
            "variables": {},
            "resources": [],
            "outputs": {}
        },
        "parameters": {},
        "mode": "",
        "provisioningState": ""
    }
}
```

### <a name="remarks"></a><span data-ttu-id="425c5-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="425c5-116">Remarks</span></span>

<span data-ttu-id="425c5-117">Můžete použít deployment() toolink tooanother šablony založené na hello URI hello nadřazené šablony.</span><span class="sxs-lookup"><span data-stu-id="425c5-117">You can use deployment() toolink tooanother template based on hello URI of hello parent template.</span></span>

```json
"variables": {  
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"  
}
```  

### <a name="example"></a><span data-ttu-id="425c5-118">Příklad</span><span class="sxs-lookup"><span data-stu-id="425c5-118">Example</span></span>

<span data-ttu-id="425c5-119">Hello následující příklad vrací objekt nasazení hello:</span><span class="sxs-lookup"><span data-stu-id="425c5-119">hello following example returns hello deployment object:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[deployment()]",
            "type" : "object"
        }
    }
}
```

<span data-ttu-id="425c5-120">Hello předchozí příklad vrací hello následujících objektů:</span><span class="sxs-lookup"><span data-stu-id="425c5-120">hello preceding example returns hello following object:</span></span>

```json
{
  "name": "deployment",
  "properties": {
    "template": {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "resources": [],
      "outputs": {
        "subscriptionOutput": {
          "type": "Object",
          "value": "[deployment()]"
        }
      }
    },
    "parameters": {},
    "mode": "Incremental",
    "provisioningState": "Accepted"
  }
}
```

<a id="parameters" />

## <a name="parameters"></a><span data-ttu-id="425c5-121">parameters</span><span class="sxs-lookup"><span data-stu-id="425c5-121">parameters</span></span>
`parameters(parameterName)`

<span data-ttu-id="425c5-122">Vrátí hodnotu parametru.</span><span class="sxs-lookup"><span data-stu-id="425c5-122">Returns a parameter value.</span></span> <span data-ttu-id="425c5-123">Hello zadaný název parametru musí být definovány v části Parametry hello hello šablony.</span><span class="sxs-lookup"><span data-stu-id="425c5-123">hello specified parameter name must be defined in hello parameters section of hello template.</span></span>

### <a name="parameters"></a><span data-ttu-id="425c5-124">Parametry</span><span class="sxs-lookup"><span data-stu-id="425c5-124">Parameters</span></span>

| <span data-ttu-id="425c5-125">Parametr</span><span class="sxs-lookup"><span data-stu-id="425c5-125">Parameter</span></span> | <span data-ttu-id="425c5-126">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="425c5-126">Required</span></span> | <span data-ttu-id="425c5-127">Typ</span><span class="sxs-lookup"><span data-stu-id="425c5-127">Type</span></span> | <span data-ttu-id="425c5-128">Popis</span><span class="sxs-lookup"><span data-stu-id="425c5-128">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="425c5-129">Název parametru</span><span class="sxs-lookup"><span data-stu-id="425c5-129">parameterName</span></span> |<span data-ttu-id="425c5-130">Ano</span><span class="sxs-lookup"><span data-stu-id="425c5-130">Yes</span></span> |<span data-ttu-id="425c5-131">Řetězec</span><span class="sxs-lookup"><span data-stu-id="425c5-131">string</span></span> |<span data-ttu-id="425c5-132">Název Hello tooreturn parametr hello.</span><span class="sxs-lookup"><span data-stu-id="425c5-132">hello name of hello parameter tooreturn.</span></span> |

### <a name="return-value"></a><span data-ttu-id="425c5-133">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="425c5-133">Return value</span></span>

<span data-ttu-id="425c5-134">parametr zadána hodnota Hello hello.</span><span class="sxs-lookup"><span data-stu-id="425c5-134">hello value of hello specified parameter.</span></span>

### <a name="remarks"></a><span data-ttu-id="425c5-135">Poznámky</span><span class="sxs-lookup"><span data-stu-id="425c5-135">Remarks</span></span>

<span data-ttu-id="425c5-136">Parametry tooset prostředků hodnoty se obvykle používá.</span><span class="sxs-lookup"><span data-stu-id="425c5-136">Typically, you use parameters tooset resource values.</span></span> <span data-ttu-id="425c5-137">Hello následující příklad ilustruje hello název webu toohello předaná hodnota parametru v během nasazování.</span><span class="sxs-lookup"><span data-stu-id="425c5-137">hello following example sets hello name of web site toohello parameter value passed in during deployment.</span></span>

```json
"parameters": { 
  "siteName": {
      "type": "string"
  }
},
"resources": [
   {
      "apiVersion": "2016-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/Sites",
      ...
   }
]
```

### <a name="example"></a><span data-ttu-id="425c5-138">Příklad</span><span class="sxs-lookup"><span data-stu-id="425c5-138">Example</span></span>

<span data-ttu-id="425c5-139">Hello následující příklad ukazuje zjednodušený použití hello parametry funkce.</span><span class="sxs-lookup"><span data-stu-id="425c5-139">hello following example shows a simplified use of hello parameters function.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringParameter": {
            "type" : "string",
            "defaultValue": "option 1"
        },
        "intParameter": {
            "type": "int",
            "defaultValue": 1
        },
        "objectParameter": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b"}
        },
        "arrayParameter": {
            "type": "array",
            "defaultValue": [1, 2, 3]
        },
        "crossParameter": {
            "type": "string",
            "defaultValue": "[parameters('stringParameter')]"
        }
    },
    "variables": {},
    "resources": [],
    "outputs": {
        "stringOutput": {
            "value": "[parameters('stringParameter')]",
            "type" : "string"
        },
        "intOutput": {
            "value": "[parameters('intParameter')]",
            "type" : "int"
        },
        "objectOutput": {
            "value": "[parameters('objectParameter')]",
            "type" : "object"
        },
        "arrayOutput": {
            "value": "[parameters('arrayParameter')]",
            "type" : "array"
        },
        "crossOutput": {
            "value": "[parameters('crossParameter')]",
            "type" : "string"
        }
    }
}
```

<span data-ttu-id="425c5-140">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="425c5-140">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="425c5-141">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="425c5-141">Name</span></span> | <span data-ttu-id="425c5-142">Typ</span><span class="sxs-lookup"><span data-stu-id="425c5-142">Type</span></span> | <span data-ttu-id="425c5-143">Hodnota</span><span class="sxs-lookup"><span data-stu-id="425c5-143">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="425c5-144">stringOutput</span><span class="sxs-lookup"><span data-stu-id="425c5-144">stringOutput</span></span> | <span data-ttu-id="425c5-145">Řetězec</span><span class="sxs-lookup"><span data-stu-id="425c5-145">String</span></span> | <span data-ttu-id="425c5-146">možnost 1</span><span class="sxs-lookup"><span data-stu-id="425c5-146">option 1</span></span> |
| <span data-ttu-id="425c5-147">intOutput</span><span class="sxs-lookup"><span data-stu-id="425c5-147">intOutput</span></span> | <span data-ttu-id="425c5-148">celá čísla</span><span class="sxs-lookup"><span data-stu-id="425c5-148">Int</span></span> | <span data-ttu-id="425c5-149">1</span><span class="sxs-lookup"><span data-stu-id="425c5-149">1</span></span> |
| <span data-ttu-id="425c5-150">objectOutput</span><span class="sxs-lookup"><span data-stu-id="425c5-150">objectOutput</span></span> | <span data-ttu-id="425c5-151">Objekt</span><span class="sxs-lookup"><span data-stu-id="425c5-151">Object</span></span> | <span data-ttu-id="425c5-152">{"1": "a", "dva": "b"}</span><span class="sxs-lookup"><span data-stu-id="425c5-152">{"one": "a", "two": "b"}</span></span> |
| <span data-ttu-id="425c5-153">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="425c5-153">arrayOutput</span></span> | <span data-ttu-id="425c5-154">Pole</span><span class="sxs-lookup"><span data-stu-id="425c5-154">Array</span></span> | <span data-ttu-id="425c5-155">[1, 2, 3]</span><span class="sxs-lookup"><span data-stu-id="425c5-155">[1, 2, 3]</span></span> |
| <span data-ttu-id="425c5-156">crossOutput</span><span class="sxs-lookup"><span data-stu-id="425c5-156">crossOutput</span></span> | <span data-ttu-id="425c5-157">Řetězec</span><span class="sxs-lookup"><span data-stu-id="425c5-157">String</span></span> | <span data-ttu-id="425c5-158">možnost 1</span><span class="sxs-lookup"><span data-stu-id="425c5-158">option 1</span></span> |

<a id="variables" />

## <a name="variables"></a><span data-ttu-id="425c5-159">proměnné</span><span class="sxs-lookup"><span data-stu-id="425c5-159">variables</span></span>
`variables(variableName)`

<span data-ttu-id="425c5-160">Vrátí hello hodnotu proměnné.</span><span class="sxs-lookup"><span data-stu-id="425c5-160">Returns hello value of variable.</span></span> <span data-ttu-id="425c5-161">Zadaný název proměnné Hello musí být definovány v části proměnných hello hello šablony.</span><span class="sxs-lookup"><span data-stu-id="425c5-161">hello specified variable name must be defined in hello variables section of hello template.</span></span>

### <a name="parameters"></a><span data-ttu-id="425c5-162">Parametry</span><span class="sxs-lookup"><span data-stu-id="425c5-162">Parameters</span></span>

| <span data-ttu-id="425c5-163">Parametr</span><span class="sxs-lookup"><span data-stu-id="425c5-163">Parameter</span></span> | <span data-ttu-id="425c5-164">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="425c5-164">Required</span></span> | <span data-ttu-id="425c5-165">Typ</span><span class="sxs-lookup"><span data-stu-id="425c5-165">Type</span></span> | <span data-ttu-id="425c5-166">Popis</span><span class="sxs-lookup"><span data-stu-id="425c5-166">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="425c5-167">NázevProměnné</span><span class="sxs-lookup"><span data-stu-id="425c5-167">variableName</span></span> |<span data-ttu-id="425c5-168">Ano</span><span class="sxs-lookup"><span data-stu-id="425c5-168">Yes</span></span> |<span data-ttu-id="425c5-169">Řetězec</span><span class="sxs-lookup"><span data-stu-id="425c5-169">String</span></span> |<span data-ttu-id="425c5-170">Název proměnné tooreturn hello Hello.</span><span class="sxs-lookup"><span data-stu-id="425c5-170">hello name of hello variable tooreturn.</span></span> |

### <a name="return-value"></a><span data-ttu-id="425c5-171">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="425c5-171">Return value</span></span>

<span data-ttu-id="425c5-172">Proměnná zadána hodnota Hello hello.</span><span class="sxs-lookup"><span data-stu-id="425c5-172">hello value of hello specified variable.</span></span>

### <a name="remarks"></a><span data-ttu-id="425c5-173">Poznámky</span><span class="sxs-lookup"><span data-stu-id="425c5-173">Remarks</span></span>

<span data-ttu-id="425c5-174">Obvykle použijete toosimplify proměnných šablony vytvořením komplexní hodnoty jenom jednou.</span><span class="sxs-lookup"><span data-stu-id="425c5-174">Typically, you use variables toosimplify your template by constructing complex values only once.</span></span> <span data-ttu-id="425c5-175">Hello následující příklad vytvoří jedinečný název pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="425c5-175">hello following example constructs a unique name for a storage account.</span></span>

```json
"variables": {
    "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
},
"resources": [
    {
        "type": "Microsoft.Storage/storageAccounts",
        "name": "[variables('storageName')]",
        ...
    },
    {
        "type": "Microsoft.Compute/virtualMachines",
        "dependsOn": [
            "[variables('storageName')]"
        ],
        ...
    }
],
```

### <a name="example"></a><span data-ttu-id="425c5-176">Příklad</span><span class="sxs-lookup"><span data-stu-id="425c5-176">Example</span></span>

<span data-ttu-id="425c5-177">Příklad šablony Hello vrátí různé hodnoty proměnné.</span><span class="sxs-lookup"><span data-stu-id="425c5-177">hello example template returns different variable values.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {
        "var1": "myVariable",
        "var2": [ 1,2,3,4 ],
        "var3": "[ variables('var1') ]",
        "var4": {
            "property1": "value1",
            "property2": "value2"
        }
    },
    "resources": [],
    "outputs": {
        "exampleOutput1": {
            "value": "[variables('var1')]",
            "type" : "string"
        },
        "exampleOutput2": {
            "value": "[variables('var2')]",
            "type" : "array"
        },
        "exampleOutput3": {
            "value": "[variables('var3')]",
            "type" : "string"
        },
        "exampleOutput4": {
            "value": "[variables('var4')]",
            "type" : "object"
        }
    }
}
```

<span data-ttu-id="425c5-178">Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:</span><span class="sxs-lookup"><span data-stu-id="425c5-178">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="425c5-179">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="425c5-179">Name</span></span> | <span data-ttu-id="425c5-180">Typ</span><span class="sxs-lookup"><span data-stu-id="425c5-180">Type</span></span> | <span data-ttu-id="425c5-181">Hodnota</span><span class="sxs-lookup"><span data-stu-id="425c5-181">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="425c5-182">exampleOutput1</span><span class="sxs-lookup"><span data-stu-id="425c5-182">exampleOutput1</span></span> | <span data-ttu-id="425c5-183">Řetězec</span><span class="sxs-lookup"><span data-stu-id="425c5-183">String</span></span> | <span data-ttu-id="425c5-184">myVariable</span><span class="sxs-lookup"><span data-stu-id="425c5-184">myVariable</span></span> |
| <span data-ttu-id="425c5-185">exampleOutput2</span><span class="sxs-lookup"><span data-stu-id="425c5-185">exampleOutput2</span></span> | <span data-ttu-id="425c5-186">Pole</span><span class="sxs-lookup"><span data-stu-id="425c5-186">Array</span></span> | <span data-ttu-id="425c5-187">[1, 2, 3, 4]</span><span class="sxs-lookup"><span data-stu-id="425c5-187">[1, 2, 3, 4]</span></span> |
| <span data-ttu-id="425c5-188">exampleOutput3</span><span class="sxs-lookup"><span data-stu-id="425c5-188">exampleOutput3</span></span> | <span data-ttu-id="425c5-189">Řetězec</span><span class="sxs-lookup"><span data-stu-id="425c5-189">String</span></span> | <span data-ttu-id="425c5-190">myVariable</span><span class="sxs-lookup"><span data-stu-id="425c5-190">myVariable</span></span> |
| <span data-ttu-id="425c5-191">exampleOutput4</span><span class="sxs-lookup"><span data-stu-id="425c5-191">exampleOutput4</span></span> |  <span data-ttu-id="425c5-192">Objekt</span><span class="sxs-lookup"><span data-stu-id="425c5-192">Object</span></span> | <span data-ttu-id="425c5-193">{"vlastnost1": "value1", "vlastnost2": "hodnota2"}</span><span class="sxs-lookup"><span data-stu-id="425c5-193">{"property1": "value1", "property2": "value2"}</span></span> |

## <a name="next-steps"></a><span data-ttu-id="425c5-194">Další kroky</span><span class="sxs-lookup"><span data-stu-id="425c5-194">Next steps</span></span>
* <span data-ttu-id="425c5-195">Popis části hello šablonu Azure Resource Manager naleznete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="425c5-195">For a description of hello sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="425c5-196">toomerge několik šablon, najdete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="425c5-196">toomerge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="425c5-197">tooiterate zadaného počtu opakování při vytváření typu prostředku, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="425c5-197">tooiterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="425c5-198">toosee způsobu toodeploy hello šablony vytvoříte, najdete v [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="425c5-198">toosee how toodeploy hello template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

