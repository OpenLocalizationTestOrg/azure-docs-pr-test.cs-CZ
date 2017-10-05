---
title: "Funkce šablon Azure Resource Manager - nasazení | Microsoft Docs"
description: "Popisuje funkce pro použití v šablonu Azure Resource Manager načíst informace o nasazení."
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
ms.openlocfilehash: d7e6bcd669d40cb19de44b646505856ecd8f51a0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="deployment-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="a2ea8-103">Nasazení funkce pro šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="a2ea8-103">Deployment functions for Azure Resource Manager templates</span></span> 

<span data-ttu-id="a2ea8-104">Resource Manager poskytuje následující funkce pro získání hodnoty z části šablony a hodnoty týkající se nasazení:</span><span class="sxs-lookup"><span data-stu-id="a2ea8-104">Resource Manager provides the following functions for getting values from sections of the template and values related to the deployment:</span></span>

* [<span data-ttu-id="a2ea8-105">nasazení</span><span class="sxs-lookup"><span data-stu-id="a2ea8-105">deployment</span></span>](#deployment)
* [<span data-ttu-id="a2ea8-106">Parametry</span><span class="sxs-lookup"><span data-stu-id="a2ea8-106">parameters</span></span>](#parameters)
* [<span data-ttu-id="a2ea8-107">proměnné</span><span class="sxs-lookup"><span data-stu-id="a2ea8-107">variables</span></span>](#variables)

<span data-ttu-id="a2ea8-108">Získá hodnoty z prostředků, skupiny prostředků nebo odběrů, najdete v tématu [prostředků funkce](resource-group-template-functions-resource.md).</span><span class="sxs-lookup"><span data-stu-id="a2ea8-108">To get values from resources, resource groups, or subscriptions, see [Resource functions](resource-group-template-functions-resource.md).</span></span>

<a id="deployment" />

## <a name="deployment"></a><span data-ttu-id="a2ea8-109">nasazení</span><span class="sxs-lookup"><span data-stu-id="a2ea8-109">deployment</span></span>
`deployment()`

<span data-ttu-id="a2ea8-110">Vrací informace o aktuální operace nasazení.</span><span class="sxs-lookup"><span data-stu-id="a2ea8-110">Returns information about the current deployment operation.</span></span>

### <a name="return-value"></a><span data-ttu-id="a2ea8-111">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="a2ea8-111">Return value</span></span>

<span data-ttu-id="a2ea8-112">Tato funkce vrátí objekt, který je předán během nasazení.</span><span class="sxs-lookup"><span data-stu-id="a2ea8-112">This function returns the object that is passed during deployment.</span></span> <span data-ttu-id="a2ea8-113">Vlastnosti v vráceného objektu se liší v závislosti na tom, jestli je objekt nasazení předán jako odkaz nebo jako objekt v řádku.</span><span class="sxs-lookup"><span data-stu-id="a2ea8-113">The properties in the returned object differ based on whether the deployment object is passed as a link or as an in-line object.</span></span> <span data-ttu-id="a2ea8-114">Pokud objekt nasazení je předán v řádku, jako třeba při použití **- TemplateFile** parametr v prostředí Azure PowerShell přejděte do místního souboru vráceného objektu má následující formát:</span><span class="sxs-lookup"><span data-stu-id="a2ea8-114">When the deployment object is passed in-line, such as when using the **-TemplateFile** parameter in Azure PowerShell to point to a local file, the returned object has the following format:</span></span>

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

<span data-ttu-id="a2ea8-115">Pokud objekt předaný jako odkaz, například při použití **- TemplateUri** parametr tak, aby odkazoval na vzdálený objekt objektu se vrátí v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="a2ea8-115">When the object is passed as a link, such as when using the **-TemplateUri** parameter to point to a remote object, the object is returned in the following format:</span></span> 

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

### <a name="remarks"></a><span data-ttu-id="a2ea8-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="a2ea8-116">Remarks</span></span>

<span data-ttu-id="a2ea8-117">Deployment() můžete propojit s jinou šablony založené na šabloně nadřazený identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="a2ea8-117">You can use deployment() to link to another template based on the URI of the parent template.</span></span>

```json
"variables": {  
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"  
}
```  

### <a name="example"></a><span data-ttu-id="a2ea8-118">Příklad</span><span class="sxs-lookup"><span data-stu-id="a2ea8-118">Example</span></span>

<span data-ttu-id="a2ea8-119">Následující příklad vrací objekt nasazení:</span><span class="sxs-lookup"><span data-stu-id="a2ea8-119">The following example returns the deployment object:</span></span>

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

<span data-ttu-id="a2ea8-120">V předchozím příkladu vrací objekt následující:</span><span class="sxs-lookup"><span data-stu-id="a2ea8-120">The preceding example returns the following object:</span></span>

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

## <a name="parameters"></a><span data-ttu-id="a2ea8-121">Parametry</span><span class="sxs-lookup"><span data-stu-id="a2ea8-121">parameters</span></span>
`parameters(parameterName)`

<span data-ttu-id="a2ea8-122">Vrátí hodnotu parametru.</span><span class="sxs-lookup"><span data-stu-id="a2ea8-122">Returns a parameter value.</span></span> <span data-ttu-id="a2ea8-123">Zadaný název parametru musí být definován v sekci parametrů šablony.</span><span class="sxs-lookup"><span data-stu-id="a2ea8-123">The specified parameter name must be defined in the parameters section of the template.</span></span>

### <a name="parameters"></a><span data-ttu-id="a2ea8-124">Parametry</span><span class="sxs-lookup"><span data-stu-id="a2ea8-124">Parameters</span></span>

| <span data-ttu-id="a2ea8-125">Parametr</span><span class="sxs-lookup"><span data-stu-id="a2ea8-125">Parameter</span></span> | <span data-ttu-id="a2ea8-126">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="a2ea8-126">Required</span></span> | <span data-ttu-id="a2ea8-127">Typ</span><span class="sxs-lookup"><span data-stu-id="a2ea8-127">Type</span></span> | <span data-ttu-id="a2ea8-128">Popis</span><span class="sxs-lookup"><span data-stu-id="a2ea8-128">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a2ea8-129">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a2ea8-129">parameterName</span></span> |<span data-ttu-id="a2ea8-130">Ano</span><span class="sxs-lookup"><span data-stu-id="a2ea8-130">Yes</span></span> |<span data-ttu-id="a2ea8-131">Řetězec</span><span class="sxs-lookup"><span data-stu-id="a2ea8-131">string</span></span> |<span data-ttu-id="a2ea8-132">Název parametru vrátit.</span><span class="sxs-lookup"><span data-stu-id="a2ea8-132">The name of the parameter to return.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a2ea8-133">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="a2ea8-133">Return value</span></span>

<span data-ttu-id="a2ea8-134">Hodnota zadaného parametru.</span><span class="sxs-lookup"><span data-stu-id="a2ea8-134">The value of the specified parameter.</span></span>

### <a name="remarks"></a><span data-ttu-id="a2ea8-135">Poznámky</span><span class="sxs-lookup"><span data-stu-id="a2ea8-135">Remarks</span></span>

<span data-ttu-id="a2ea8-136">Parametry se obvykle používají pro nastavení hodnot prostředků.</span><span class="sxs-lookup"><span data-stu-id="a2ea8-136">Typically, you use parameters to set resource values.</span></span> <span data-ttu-id="a2ea8-137">Následující příklad nastaví název webové stránky na hodnotu parametru předána během nasazení.</span><span class="sxs-lookup"><span data-stu-id="a2ea8-137">The following example sets the name of web site to the parameter value passed in during deployment.</span></span>

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

### <a name="example"></a><span data-ttu-id="a2ea8-138">Příklad</span><span class="sxs-lookup"><span data-stu-id="a2ea8-138">Example</span></span>

<span data-ttu-id="a2ea8-139">Následující příklad ukazuje zjednodušený použijte parametry funkce.</span><span class="sxs-lookup"><span data-stu-id="a2ea8-139">The following example shows a simplified use of the parameters function.</span></span>

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

<span data-ttu-id="a2ea8-140">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="a2ea8-140">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="a2ea8-141">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="a2ea8-141">Name</span></span> | <span data-ttu-id="a2ea8-142">Typ</span><span class="sxs-lookup"><span data-stu-id="a2ea8-142">Type</span></span> | <span data-ttu-id="a2ea8-143">Hodnota</span><span class="sxs-lookup"><span data-stu-id="a2ea8-143">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a2ea8-144">stringOutput</span><span class="sxs-lookup"><span data-stu-id="a2ea8-144">stringOutput</span></span> | <span data-ttu-id="a2ea8-145">Řetězec</span><span class="sxs-lookup"><span data-stu-id="a2ea8-145">String</span></span> | <span data-ttu-id="a2ea8-146">možnost 1</span><span class="sxs-lookup"><span data-stu-id="a2ea8-146">option 1</span></span> |
| <span data-ttu-id="a2ea8-147">intOutput</span><span class="sxs-lookup"><span data-stu-id="a2ea8-147">intOutput</span></span> | <span data-ttu-id="a2ea8-148">celá čísla</span><span class="sxs-lookup"><span data-stu-id="a2ea8-148">Int</span></span> | <span data-ttu-id="a2ea8-149">1</span><span class="sxs-lookup"><span data-stu-id="a2ea8-149">1</span></span> |
| <span data-ttu-id="a2ea8-150">objectOutput</span><span class="sxs-lookup"><span data-stu-id="a2ea8-150">objectOutput</span></span> | <span data-ttu-id="a2ea8-151">Objekt</span><span class="sxs-lookup"><span data-stu-id="a2ea8-151">Object</span></span> | <span data-ttu-id="a2ea8-152">{"1": "a", "dva": "b"}</span><span class="sxs-lookup"><span data-stu-id="a2ea8-152">{"one": "a", "two": "b"}</span></span> |
| <span data-ttu-id="a2ea8-153">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="a2ea8-153">arrayOutput</span></span> | <span data-ttu-id="a2ea8-154">Pole</span><span class="sxs-lookup"><span data-stu-id="a2ea8-154">Array</span></span> | <span data-ttu-id="a2ea8-155">[1, 2, 3]</span><span class="sxs-lookup"><span data-stu-id="a2ea8-155">[1, 2, 3]</span></span> |
| <span data-ttu-id="a2ea8-156">crossOutput</span><span class="sxs-lookup"><span data-stu-id="a2ea8-156">crossOutput</span></span> | <span data-ttu-id="a2ea8-157">Řetězec</span><span class="sxs-lookup"><span data-stu-id="a2ea8-157">String</span></span> | <span data-ttu-id="a2ea8-158">možnost 1</span><span class="sxs-lookup"><span data-stu-id="a2ea8-158">option 1</span></span> |

<a id="variables" />

## <a name="variables"></a><span data-ttu-id="a2ea8-159">proměnné</span><span class="sxs-lookup"><span data-stu-id="a2ea8-159">variables</span></span>
`variables(variableName)`

<span data-ttu-id="a2ea8-160">Vrátí hodnotu proměnné.</span><span class="sxs-lookup"><span data-stu-id="a2ea8-160">Returns the value of variable.</span></span> <span data-ttu-id="a2ea8-161">Zadaný název proměnné musí být definován v sekci proměnných šablony.</span><span class="sxs-lookup"><span data-stu-id="a2ea8-161">The specified variable name must be defined in the variables section of the template.</span></span>

### <a name="parameters"></a><span data-ttu-id="a2ea8-162">Parametry</span><span class="sxs-lookup"><span data-stu-id="a2ea8-162">Parameters</span></span>

| <span data-ttu-id="a2ea8-163">Parametr</span><span class="sxs-lookup"><span data-stu-id="a2ea8-163">Parameter</span></span> | <span data-ttu-id="a2ea8-164">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="a2ea8-164">Required</span></span> | <span data-ttu-id="a2ea8-165">Typ</span><span class="sxs-lookup"><span data-stu-id="a2ea8-165">Type</span></span> | <span data-ttu-id="a2ea8-166">Popis</span><span class="sxs-lookup"><span data-stu-id="a2ea8-166">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a2ea8-167">NázevProměnné</span><span class="sxs-lookup"><span data-stu-id="a2ea8-167">variableName</span></span> |<span data-ttu-id="a2ea8-168">Ano</span><span class="sxs-lookup"><span data-stu-id="a2ea8-168">Yes</span></span> |<span data-ttu-id="a2ea8-169">Řetězec</span><span class="sxs-lookup"><span data-stu-id="a2ea8-169">String</span></span> |<span data-ttu-id="a2ea8-170">Název proměnné vrátit.</span><span class="sxs-lookup"><span data-stu-id="a2ea8-170">The name of the variable to return.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a2ea8-171">Návratová hodnota</span><span class="sxs-lookup"><span data-stu-id="a2ea8-171">Return value</span></span>

<span data-ttu-id="a2ea8-172">Hodnotu zadanou proměnnou.</span><span class="sxs-lookup"><span data-stu-id="a2ea8-172">The value of the specified variable.</span></span>

### <a name="remarks"></a><span data-ttu-id="a2ea8-173">Poznámky</span><span class="sxs-lookup"><span data-stu-id="a2ea8-173">Remarks</span></span>

<span data-ttu-id="a2ea8-174">Proměnné se obvykle používají pro zjednodušení šablony vytvořením komplexní hodnoty jenom jednou.</span><span class="sxs-lookup"><span data-stu-id="a2ea8-174">Typically, you use variables to simplify your template by constructing complex values only once.</span></span> <span data-ttu-id="a2ea8-175">Následující příklad vytvoří jedinečný název pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="a2ea8-175">The following example constructs a unique name for a storage account.</span></span>

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

### <a name="example"></a><span data-ttu-id="a2ea8-176">Příklad</span><span class="sxs-lookup"><span data-stu-id="a2ea8-176">Example</span></span>

<span data-ttu-id="a2ea8-177">Příklad šablony vrátí různé hodnoty proměnné.</span><span class="sxs-lookup"><span data-stu-id="a2ea8-177">The example template returns different variable values.</span></span>

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

<span data-ttu-id="a2ea8-178">Výstup z předchozího příkladu s výchozími hodnotami je:</span><span class="sxs-lookup"><span data-stu-id="a2ea8-178">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="a2ea8-179">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="a2ea8-179">Name</span></span> | <span data-ttu-id="a2ea8-180">Typ</span><span class="sxs-lookup"><span data-stu-id="a2ea8-180">Type</span></span> | <span data-ttu-id="a2ea8-181">Hodnota</span><span class="sxs-lookup"><span data-stu-id="a2ea8-181">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a2ea8-182">exampleOutput1</span><span class="sxs-lookup"><span data-stu-id="a2ea8-182">exampleOutput1</span></span> | <span data-ttu-id="a2ea8-183">Řetězec</span><span class="sxs-lookup"><span data-stu-id="a2ea8-183">String</span></span> | <span data-ttu-id="a2ea8-184">myVariable</span><span class="sxs-lookup"><span data-stu-id="a2ea8-184">myVariable</span></span> |
| <span data-ttu-id="a2ea8-185">exampleOutput2</span><span class="sxs-lookup"><span data-stu-id="a2ea8-185">exampleOutput2</span></span> | <span data-ttu-id="a2ea8-186">Pole</span><span class="sxs-lookup"><span data-stu-id="a2ea8-186">Array</span></span> | <span data-ttu-id="a2ea8-187">[1, 2, 3, 4]</span><span class="sxs-lookup"><span data-stu-id="a2ea8-187">[1, 2, 3, 4]</span></span> |
| <span data-ttu-id="a2ea8-188">exampleOutput3</span><span class="sxs-lookup"><span data-stu-id="a2ea8-188">exampleOutput3</span></span> | <span data-ttu-id="a2ea8-189">Řetězec</span><span class="sxs-lookup"><span data-stu-id="a2ea8-189">String</span></span> | <span data-ttu-id="a2ea8-190">myVariable</span><span class="sxs-lookup"><span data-stu-id="a2ea8-190">myVariable</span></span> |
| <span data-ttu-id="a2ea8-191">exampleOutput4</span><span class="sxs-lookup"><span data-stu-id="a2ea8-191">exampleOutput4</span></span> |  <span data-ttu-id="a2ea8-192">Objekt</span><span class="sxs-lookup"><span data-stu-id="a2ea8-192">Object</span></span> | <span data-ttu-id="a2ea8-193">{"vlastnost1": "value1", "vlastnost2": "hodnota2"}</span><span class="sxs-lookup"><span data-stu-id="a2ea8-193">{"property1": "value1", "property2": "value2"}</span></span> |

## <a name="next-steps"></a><span data-ttu-id="a2ea8-194">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a2ea8-194">Next steps</span></span>
* <span data-ttu-id="a2ea8-195">Popis v částech šablonu Azure Resource Manager naleznete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="a2ea8-195">For a description of the sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="a2ea8-196">Sloučit několik šablon, najdete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="a2ea8-196">To merge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="a2ea8-197">K iteraci v zadaného počtu opakování při vytváření typu prostředku, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="a2ea8-197">To iterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="a2ea8-198">Postup nasazení šablony, které jste vytvořili, najdete v sekci [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="a2ea8-198">To see how to deploy the template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

