---
title: "Nasazení více instancí prostředků Azure | Microsoft Docs"
description: "Použití operace kopírování a polí ve šablonu Azure Resource Manager k iteraci v vícekrát při nasazení prostředků."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.assetid: 94d95810-a87b-460f-8e82-c69d462ac3ca
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/26/2017
ms.author: tomfitz
ms.openlocfilehash: ed8e3081d2b2e07938d7cf3aa5f95f6dde81bc66
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-multiple-instances-of-a-resource-or-property-in-azure-resource-manager-templates"></a><span data-ttu-id="5fedd-103">Nasazení více instancí prostředek nebo vlastnost v šablonách Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="5fedd-103">Deploy multiple instances of a resource or property in Azure Resource Manager templates</span></span>
<span data-ttu-id="5fedd-104">Toto téma ukazuje, jak k iteraci v šablony Azure Resource Manager můžete vytvořit více instancí prostředku nebo více instancí vlastnosti prostředku.</span><span class="sxs-lookup"><span data-stu-id="5fedd-104">This topic shows you how to iterate in your Azure Resource Manager template to create multiple instances of a resource, or multiple instances of a property on a resource.</span></span>

<span data-ttu-id="5fedd-105">Pokud potřebujete přidat logiku do šablony, která umožňuje určit, jestli je prostředek nasazené, najdete v části [podmíněně nasazení prostředků](#conditionally-deploy-resource).</span><span class="sxs-lookup"><span data-stu-id="5fedd-105">If you need to add logic to your template that enables you to specify whether a resource is deployed, see [Conditionally deploy resource](#conditionally-deploy-resource).</span></span>

## <a name="resource-iteration"></a><span data-ttu-id="5fedd-106">Iterace prostředků</span><span class="sxs-lookup"><span data-stu-id="5fedd-106">Resource iteration</span></span>
<span data-ttu-id="5fedd-107">Chcete-li vytvořit více instancí typu prostředku, přidejte `copy` element pro typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="5fedd-107">To create multiple instances of a resource type, add a `copy` element to the resource type.</span></span> <span data-ttu-id="5fedd-108">V elementu kopírování zadejte počet opakování a název pro tento smyčky.</span><span class="sxs-lookup"><span data-stu-id="5fedd-108">In the copy element, you specify the number of iterations and a name for this loop.</span></span> <span data-ttu-id="5fedd-109">Hodnota počtu musí být kladné celé číslo a nesmí být delší než 800.</span><span class="sxs-lookup"><span data-stu-id="5fedd-109">The count value must be a positive integer and cannot exceed 800.</span></span> <span data-ttu-id="5fedd-110">Resource Manager vytvoří prostředky paralelně.</span><span class="sxs-lookup"><span data-stu-id="5fedd-110">Resource Manager creates the resources in parallel.</span></span> <span data-ttu-id="5fedd-111">Proto není zaručena pořadí, ve které byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="5fedd-111">Therefore, the order in which they are created is not guaranteed.</span></span> <span data-ttu-id="5fedd-112">Pokud chcete vytvořit vstupní prostředky v pořadí, v tématu [sériové kopie](#serial-copy).</span><span class="sxs-lookup"><span data-stu-id="5fedd-112">To create iterated resources in sequence, see [Serial copy](#serial-copy).</span></span> 

<span data-ttu-id="5fedd-113">Prostředek pro vytvoření vícekrát má následující formát:</span><span class="sxs-lookup"><span data-stu-id="5fedd-113">The resource to create multiple times takes the following format:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 3
            }
        }
    ],
    "outputs": {}
}
```

<span data-ttu-id="5fedd-114">Všimněte si, že obsahuje název každého prostředku `copyIndex()` funkci, která vrátí na aktuální iteraci smyčky.</span><span class="sxs-lookup"><span data-stu-id="5fedd-114">Notice that the name of each resource includes the `copyIndex()` function, which returns the current iteration in the loop.</span></span> <span data-ttu-id="5fedd-115">`copyIndex()`je počítáno od nuly.</span><span class="sxs-lookup"><span data-stu-id="5fedd-115">`copyIndex()` is zero-based.</span></span> <span data-ttu-id="5fedd-116">To, v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="5fedd-116">So, the following example:</span></span>

```json
"name": "[concat('storage', copyIndex())]",
```

<span data-ttu-id="5fedd-117">Vytvoří tyto názvy:</span><span class="sxs-lookup"><span data-stu-id="5fedd-117">Creates these names:</span></span>

* <span data-ttu-id="5fedd-118">storage0</span><span class="sxs-lookup"><span data-stu-id="5fedd-118">storage0</span></span>
* <span data-ttu-id="5fedd-119">storage1</span><span class="sxs-lookup"><span data-stu-id="5fedd-119">storage1</span></span>
* <span data-ttu-id="5fedd-120">storage2.</span><span class="sxs-lookup"><span data-stu-id="5fedd-120">storage2.</span></span>

<span data-ttu-id="5fedd-121">K posunutí hodnotu indexu, může předat hodnotu ve funkci copyIndex().</span><span class="sxs-lookup"><span data-stu-id="5fedd-121">To offset the index value, you can pass a value in the copyIndex() function.</span></span> <span data-ttu-id="5fedd-122">V elementu kopie je stále zadat počet opakování provést, ale zadaná hodnota je posunut hodnotu copyIndex.</span><span class="sxs-lookup"><span data-stu-id="5fedd-122">The number of iterations to perform is still specified in the copy element, but the value of copyIndex is offset by the specified value.</span></span> <span data-ttu-id="5fedd-123">To, v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="5fedd-123">So, the following example:</span></span>

```json
"name": "[concat('storage', copyIndex(1))]",
```

<span data-ttu-id="5fedd-124">Vytvoří tyto názvy:</span><span class="sxs-lookup"><span data-stu-id="5fedd-124">Creates these names:</span></span>

* <span data-ttu-id="5fedd-125">storage1</span><span class="sxs-lookup"><span data-stu-id="5fedd-125">storage1</span></span>
* <span data-ttu-id="5fedd-126">storage2</span><span class="sxs-lookup"><span data-stu-id="5fedd-126">storage2</span></span>
* <span data-ttu-id="5fedd-127">storage3</span><span class="sxs-lookup"><span data-stu-id="5fedd-127">storage3</span></span>

<span data-ttu-id="5fedd-128">Operace kopírování je užitečné při práci s poli, protože můžete iterovat každý prvek v poli.</span><span class="sxs-lookup"><span data-stu-id="5fedd-128">The copy operation is helpful when working with arrays because you can iterate through each element in the array.</span></span> <span data-ttu-id="5fedd-129">Použití `length` funkce v poli zadat počet opakování, a `copyIndex` načíst aktuální index v poli.</span><span class="sxs-lookup"><span data-stu-id="5fedd-129">Use the `length` function on the array to specify the count for iterations, and `copyIndex` to retrieve the current index in the array.</span></span> <span data-ttu-id="5fedd-130">To, v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="5fedd-130">So, the following example:</span></span>

```json
"parameters": { 
  "org": { 
     "type": "array", 
     "defaultValue": [ 
         "contoso", 
         "fabrikam", 
         "coho" 
      ] 
  }
}, 
"resources": [ 
  { 
      "name": "[concat('storage', parameters('org')[copyIndex()])]", 
      "copy": { 
         "name": "storagecopy", 
         "count": "[length(parameters('org'))]" 
      }, 
      ...
  } 
]
```

<span data-ttu-id="5fedd-131">Vytvoří tyto názvy:</span><span class="sxs-lookup"><span data-stu-id="5fedd-131">Creates these names:</span></span>

* <span data-ttu-id="5fedd-132">storagecontoso</span><span class="sxs-lookup"><span data-stu-id="5fedd-132">storagecontoso</span></span>
* <span data-ttu-id="5fedd-133">storagefabrikam</span><span class="sxs-lookup"><span data-stu-id="5fedd-133">storagefabrikam</span></span>
* <span data-ttu-id="5fedd-134">storagecoho</span><span class="sxs-lookup"><span data-stu-id="5fedd-134">storagecoho</span></span>

## <a name="serial-copy"></a><span data-ttu-id="5fedd-135">Kopírování sériového portu</span><span class="sxs-lookup"><span data-stu-id="5fedd-135">Serial copy</span></span>

<span data-ttu-id="5fedd-136">Při použití elementu kopie vytvořit více instancí na typ prostředku, správce prostředků, ve výchozím nastavení, nasadí tyto instance paralelně.</span><span class="sxs-lookup"><span data-stu-id="5fedd-136">When you use the copy element to create multiple instances of a resource type, Resource Manager, by default, deploys those instances in parallel.</span></span> <span data-ttu-id="5fedd-137">Můžete však zadat, aby byly prostředky nasazené v pořadí.</span><span class="sxs-lookup"><span data-stu-id="5fedd-137">However, you may want to specify that the resources are deployed in sequence.</span></span> <span data-ttu-id="5fedd-138">Například při aktualizaci provozním prostředí, můžete tak rozložte aktualizací jenom několik jsou aktualizovány v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="5fedd-138">For example, when updating a production environment, you may want to stagger the updates so only a certain number are updated at any one time.</span></span>

<span data-ttu-id="5fedd-139">Resource Manager poskytuje vlastnosti na kopírování elementu, který vám umožní sériově nasazení více instancí.</span><span class="sxs-lookup"><span data-stu-id="5fedd-139">Resource Manager provides properties on the copy element that enable you to serially deploy multiple instances.</span></span> <span data-ttu-id="5fedd-140">V sadě elementů kopie `mode` k **sériové** a `batchSize` na počet instancí pro nasazení v čase.</span><span class="sxs-lookup"><span data-stu-id="5fedd-140">In the copy element, set `mode` to **serial** and `batchSize` to the number of instances to deploy at a time.</span></span> <span data-ttu-id="5fedd-141">Sériového portu v režimu Resource Manager vytvoří závislost na dřívější instancí ve smyčce, tak nespustí jeden batch, dokud se nedokončí předchozí dávka.</span><span class="sxs-lookup"><span data-stu-id="5fedd-141">With serial mode, Resource Manager creates a dependency on earlier instances in the loop, so it does not start one batch until the previous batch completes.</span></span>

```json
"copy": {
    "name": "iterator",
    "count": "[parameters('numberToDeploy')]",
    "mode": "serial",
    "batchSize": 2
},
```

<span data-ttu-id="5fedd-142">Vlastnost režimu také přijímá **paralelní**, což je výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="5fedd-142">The mode property also accepts **parallel**, which is the default value.</span></span>

<span data-ttu-id="5fedd-143">K otestování sériové kopírování bez vytváření skutečné prostředků, použijte následující šablony, která nasadí prázdný vnořené šablony:</span><span class="sxs-lookup"><span data-stu-id="5fedd-143">To test serial copy without creating actual resources, use the following template that deploys empty nested templates:</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "numberToDeploy": {
      "type": "int",
      "minValue": 2,
      "defaultValue": 5
    }
  },
  "resources": [
    {
      "apiVersion": "2015-01-01",
      "type": "Microsoft.Resources/deployments",
      "name": "[concat('loop-', copyIndex())]",
      "copy": {
        "name": "iterator",
        "count": "[parameters('numberToDeploy')]",
        "mode": "serial",
        "batchSize": 1
      },
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {},
          "variables": {},
          "resources": [],
          "outputs": {
          }
        }
      }
    }
  ],
  "outputs": {
  }
}
```

<span data-ttu-id="5fedd-144">V historii nasazení Všimněte si, že vnořené nasazení se zpracovávají v pořadí.</span><span class="sxs-lookup"><span data-stu-id="5fedd-144">In the deployment history, notice that the nested deployments are processed in sequence.</span></span>

![sériové nasazení](./media/resource-group-create-multiple/serial-copy.png)

<span data-ttu-id="5fedd-146">Následující příklad realističtější scénáři nasadí dvě instance v době virtuálního počítače s Linuxem z vnořené šablony:</span><span class="sxs-lookup"><span data-stu-id="5fedd-146">For a more realistic scenario, the following example deploys two instances at a time of a Linux VM from a nested template:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "adminUsername": {
            "type": "string",
            "metadata": {
                "description": "User name for the Virtual Machine."
            }
        },
        "adminPassword": {
            "type": "securestring",
            "metadata": {
                "description": "Password for the Virtual Machine."
            }
        },
        "dnsLabelPrefix": {
            "type": "string",
            "metadata": {
                "description": "Unique DNS Name for the Public IP used to access the Virtual Machine."
            }
        },
        "ubuntuOSVersion": {
            "type": "string",
            "defaultValue": "16.04.0-LTS",
            "allowedValues": [
                "12.04.5-LTS",
                "14.04.5-LTS",
                "15.10",
                "16.04.0-LTS"
            ],
            "metadata": {
                "description": "The Ubuntu version for the VM. This will pick a fully patched image of this given Ubuntu version."
            }
        }
    },
    "variables": {
        "templatelink": "https://raw.githubusercontent.com/rjmax/Build2017/master/Act1.TemplateEnhancements/Chapter03.LinuxVM.json"
    },
    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "[concat('nestedDeployment',copyIndex())]",
            "type": "Microsoft.Resources/deployments",
            "copy": {
                "name": "myCopySet",
                "count": 4,
                "mode": "serial",
                "batchSize": 2
            },
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "uri": "[variables('templatelink')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "adminUsername": {
                        "value": "[parameters('adminUsername')]"
                    },
                    "adminPassword": {
                        "value": "[parameters('adminPassword')]"
                    },
                    "dnsLabelPrefix": {
                        "value": "[parameters('dnsLabelPrefix')]"
                    },
                    "ubuntuOSVersion": {
                        "value": "[parameters('ubuntuOSVersion')]"
                    },
                    "index":{
                        "value": "[copyIndex()]"
                    }
                }
            }
        }
    ]
}
```

## <a name="property-iteration"></a><span data-ttu-id="5fedd-147">Vlastnost iterace</span><span class="sxs-lookup"><span data-stu-id="5fedd-147">Property iteration</span></span>

<span data-ttu-id="5fedd-148">Chcete-li vytvořit více hodnot pro vlastnost prostředku, přidejte `copy` pole v prvku vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="5fedd-148">To create multiple values for a property on a resource, add a `copy` array in the properties element.</span></span> <span data-ttu-id="5fedd-149">Toto pole obsahuje objekty a každý objekt má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="5fedd-149">This array contains objects, and each object has the following properties:</span></span>

* <span data-ttu-id="5fedd-150">název – název vlastnosti pro vytvoření více hodnot pro</span><span class="sxs-lookup"><span data-stu-id="5fedd-150">name - the name of the property to create multiple values for</span></span>
* <span data-ttu-id="5fedd-151">počet - počet hodnot k vytvoření</span><span class="sxs-lookup"><span data-stu-id="5fedd-151">count - the number of values to create</span></span>
* <span data-ttu-id="5fedd-152">(vstup) – objekt, který obsahuje hodnoty, které mají přiřadit k vlastnosti</span><span class="sxs-lookup"><span data-stu-id="5fedd-152">input - an object that contains the values to assign to the property</span></span>  

<span data-ttu-id="5fedd-153">Následující příklad ukazuje, jak se má použít `copy` pro dataDisks vlastnost na virtuálním počítači:</span><span class="sxs-lookup"><span data-stu-id="5fedd-153">The following example shows how to apply `copy` to the dataDisks property on a virtual machine:</span></span>

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "copy": [{
          "name": "dataDisks",
          "count": 3,
          "input": {
              "lun": "[copyIndex('dataDisks')]",
              "createOption": "Empty",
              "diskSizeGB": "1023"
          }
      }],
      ...
```

<span data-ttu-id="5fedd-154">Všimněte si, že při použití `copyIndex` uvnitř iterace vlastnost, je nutné zadat název iterace.</span><span class="sxs-lookup"><span data-stu-id="5fedd-154">Notice that when using `copyIndex` inside a property iteration, you must provide the name of the iteration.</span></span> <span data-ttu-id="5fedd-155">Nemáte k zadání názvu při použití s iterace prostředků.</span><span class="sxs-lookup"><span data-stu-id="5fedd-155">You do not have to provide the name when used with resource iteration.</span></span>

<span data-ttu-id="5fedd-156">Správce prostředků rozšíří `copy` pole během nasazení.</span><span class="sxs-lookup"><span data-stu-id="5fedd-156">Resource Manager expands the `copy` array during deployment.</span></span> <span data-ttu-id="5fedd-157">Název pole bude název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="5fedd-157">The name of the array becomes the name of the property.</span></span> <span data-ttu-id="5fedd-158">Vstupní hodnoty stát vlastnosti objektu.</span><span class="sxs-lookup"><span data-stu-id="5fedd-158">The input values become the object properties.</span></span> <span data-ttu-id="5fedd-159">Nasazené šablony se změní na:</span><span class="sxs-lookup"><span data-stu-id="5fedd-159">The deployed template becomes:</span></span>

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "dataDisks": [
          {
              "lun": 0,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          },
          {
              "lun": 1,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          },
          {
              "lun": 2,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          }
      }],
      ...
```

<span data-ttu-id="5fedd-160">Iterace prostředků a vlastnosti můžete použít společně.</span><span class="sxs-lookup"><span data-stu-id="5fedd-160">You can use resource and property iteration together.</span></span> <span data-ttu-id="5fedd-161">Odkaz na vlastnost iterace podle názvu.</span><span class="sxs-lookup"><span data-stu-id="5fedd-161">Reference the property iteration by name.</span></span>

```json
{
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[concat(parameters('vnetname'), copyIndex())]",
    "apiVersion": "2016-06-01",
    "copy":{
        "count": 2,
        "name": "vnetloop"
    },
    "location": "[resourceGroup().location]",
    "properties": {
        "addressSpace": {
            "addressPrefixes": [
                "[parameters('addressPrefix')]"
            ]
        },
        "copy": [
            {
                "name": "subnets",
                "count": 2,
                "input": {
                    "name": "[concat('subnet-', copyIndex('subnets'))]",
                    "properties": {
                        "addressPrefix": "[variables('subnetAddressPrefix')[copyIndex('subnets')]]"
                    }
                }
            }
        ]
    }
}
```

<span data-ttu-id="5fedd-162">Ve vlastnostech pro každý prostředek může zahrnovat pouze jeden element kopírování.</span><span class="sxs-lookup"><span data-stu-id="5fedd-162">You can only include one copy element in the properties for each resource.</span></span> <span data-ttu-id="5fedd-163">Chcete-li zadat smyčky iterace pro více než jednu vlastnost, definujte více objektů v poli Kopírovat.</span><span class="sxs-lookup"><span data-stu-id="5fedd-163">To specify an iteration loop for more than one property, define multiple objects in the copy array.</span></span> <span data-ttu-id="5fedd-164">Každý objekt je vstupní samostatně.</span><span class="sxs-lookup"><span data-stu-id="5fedd-164">Each object is iterated separately.</span></span> <span data-ttu-id="5fedd-165">Chcete-li například vytvořit více instancí i `frontendIPConfigurations` vlastnost a `loadBalancingRules` vlastnost zařízení na Vyrovnávání zatížení, zadejte oba objekty v elementu jedna kopie:</span><span class="sxs-lookup"><span data-stu-id="5fedd-165">For example, to create multiple instances of both the `frontendIPConfigurations` property and the `loadBalancingRules` property on a load balancer, define both objects in a single copy element:</span></span> 

```json
{
    "name": "[variables('loadBalancerName')]",
    "type": "Microsoft.Network/loadBalancers",
    "properties": {
        "copy": [
          {
              "name": "frontendIPConfigurations",
              "count": 2,
              "input": {
                  "name": "[concat('loadBalancerFrontEnd', copyIndex('frontendIPConfigurations', 1))]",
                  "properties": {
                      "publicIPAddress": {
                          "id": "[variables(concat('publicIPAddressID', copyIndex('frontendIPConfigurations', 1)))]"
                      }
                  }
              }
          },
          {
              "name": "loadBalancingRules",
              "count": 2,
              "input": {
                  "name": "[concat('LBRuleForVIP', copyIndex('loadBalancingRules', 1))]",
                  "properties": {
                      "frontendIPConfiguration": {
                          "id": "[variables(concat('frontEndIPConfigID', copyIndex('loadBalancingRules', 1)))]"
                      },
                      "backendAddressPool": {
                          "id": "[variables('lbBackendPoolID')]"
                      },
                      "protocol": "tcp",
                      "frontendPort": "[variables(concat('frontEndPort' copyIndex('loadBalancingRules', 1))]",
                      "backendPort": "[variables(concat('backEndPort' copyIndex('loadBalancingRules', 1))]",
                      "probe": {
                          "id": "[variables('lbProbeID')]"
                      }
                  }
              }
          }
        ],
        ...
    }
}
```

## <a name="depend-on-resources-in-a-loop"></a><span data-ttu-id="5fedd-166">Závisí na prostředky ve smyčce</span><span class="sxs-lookup"><span data-stu-id="5fedd-166">Depend on resources in a loop</span></span>
<span data-ttu-id="5fedd-167">Určíte, že je prostředek nasazeno po jiný prostředek pomocí `dependsOn` elementu.</span><span class="sxs-lookup"><span data-stu-id="5fedd-167">You specify that a resource is deployed after another resource by using the `dependsOn` element.</span></span> <span data-ttu-id="5fedd-168">K nasazení na prostředek, který závisí na kolekci prostředků ve smyčce, zadejte název kopírovací smyčkou v elementu dependsOn.</span><span class="sxs-lookup"><span data-stu-id="5fedd-168">To deploy a resource that depends on the collection of resources in a loop, provide the name of the copy loop in the dependsOn element.</span></span> <span data-ttu-id="5fedd-169">Následující příklad ukazuje, jak nasadit tři účty úložiště před nasazením virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5fedd-169">The following example shows how to deploy three storage accounts before deploying the Virtual Machine.</span></span> <span data-ttu-id="5fedd-170">Úplná definice virtuálního počítače se nezobrazí.</span><span class="sxs-lookup"><span data-stu-id="5fedd-170">The full Virtual Machine definition is not shown.</span></span> <span data-ttu-id="5fedd-171">Všimněte si, že element kopie má název nastaven `storagecopy` a element dependsOn pro virtuální počítače je také nastavena na `storagecopy`.</span><span class="sxs-lookup"><span data-stu-id="5fedd-171">Notice that the copy element has name set to `storagecopy` and the dependsOn element for the Virtual Machines is also set to `storagecopy`.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 3
            }
        },
        {
            "apiVersion": "2015-06-15", 
            "type": "Microsoft.Compute/virtualMachines", 
            "name": "[concat('VM', uniqueString(resourceGroup().id))]",  
            "dependsOn": ["storagecopy"],
            ...
        }
    ],
    "outputs": {}
}
```

## <a name="create-multiple-instances-of-a-child-resource"></a><span data-ttu-id="5fedd-172">Vytvoření více instancí podřízených prostředků</span><span class="sxs-lookup"><span data-stu-id="5fedd-172">Create multiple instances of a child resource</span></span>
<span data-ttu-id="5fedd-173">Kopírovací smyčkou nelze použít pro podřízený prostředek.</span><span class="sxs-lookup"><span data-stu-id="5fedd-173">You cannot use a copy loop for a child resource.</span></span> <span data-ttu-id="5fedd-174">Pokud chcete vytvořit několik instancí na prostředek, který je obvykle definovat jako vnořené v rámci jiný prostředek, musíte místo toho vytvořit prostředku jako prostředek nejvyšší úrovně.</span><span class="sxs-lookup"><span data-stu-id="5fedd-174">To create multiple instances of a resource that you typically define as nested within another resource, you must instead create that resource as a top-level resource.</span></span> <span data-ttu-id="5fedd-175">Můžete definovat relaci s nadřazený prostředek prostřednictvím typ a název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="5fedd-175">You define the relationship with the parent resource through the type and name properties.</span></span>

<span data-ttu-id="5fedd-176">Předpokládejme například, že definujete obvykle datovou sadu jako podřízený prostředek v rámci služby data factory.</span><span class="sxs-lookup"><span data-stu-id="5fedd-176">For example, suppose you typically define a dataset as a child resource within a data factory.</span></span>

```json
"resources": [
{
    "type": "Microsoft.DataFactory/datafactories",
    "name": "exampleDataFactory",
    ...
    "resources": [
    {
        "type": "datasets",
        "name": "exampleDataSet",
        "dependsOn": [
            "exampleDataFactory"
        ],
        ...
    }
}]
```

<span data-ttu-id="5fedd-177">Pokud chcete vytvořit více instancí datových sad, přesuňte jej mimo služby data factory.</span><span class="sxs-lookup"><span data-stu-id="5fedd-177">To create multiple instances of data sets, move it outside of the data factory.</span></span> <span data-ttu-id="5fedd-178">Datová sada musí být na stejné úrovni jako objekt pro vytváření dat, ale je stále prostředek podřízeného objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="5fedd-178">The dataset must be at the same level as the data factory, but it is still a child resource of the data factory.</span></span> <span data-ttu-id="5fedd-179">Můžete zachovat vztah mezi datovou sadu a objektu pro vytváření dat pomocí vlastnosti typu a název.</span><span class="sxs-lookup"><span data-stu-id="5fedd-179">You preserve the relationship between data set and data factory through the type and name properties.</span></span> <span data-ttu-id="5fedd-180">Vzhledem k tomu, že už se nedá odvodit typ od pozice v šabloně, je nutné zadat plně kvalifikovaný typ ve formátu: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.</span><span class="sxs-lookup"><span data-stu-id="5fedd-180">Since type can no longer be inferred from its position in the template, you must provide the fully qualified type in the format: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.</span></span>

<span data-ttu-id="5fedd-181">K vytvoření vztahu nadřazený podřízený s instancí objektu pro vytváření dat, zadejte název pro datovou sadou, která obsahuje název nadřazené prostředků.</span><span class="sxs-lookup"><span data-stu-id="5fedd-181">To establish a parent/child relationship with an instance of the data factory, provide a name for the data set that includes the parent resource name.</span></span> <span data-ttu-id="5fedd-182">Použijte formát: `{parent-resource-name}/{child-resource-name}`.</span><span class="sxs-lookup"><span data-stu-id="5fedd-182">Use the format: `{parent-resource-name}/{child-resource-name}`.</span></span>  

<span data-ttu-id="5fedd-183">Následující příklad ukazuje implementaci:</span><span class="sxs-lookup"><span data-stu-id="5fedd-183">The following example shows the implementation:</span></span>

```json
"resources": [
{
    "type": "Microsoft.DataFactory/datafactories",
    "name": "exampleDataFactory",
    ...
},
{
    "type": "Microsoft.DataFactory/datafactories/datasets",
    "name": "[concat('exampleDataFactory', '/', 'exampleDataSet', copyIndex())]",
    "dependsOn": [
        "exampleDataFactory"
    ],
    "copy": { 
        "name": "datasetcopy", 
        "count": "3" 
    } 
    ...
}]
```

## <a name="conditionally-deploy-resource"></a><span data-ttu-id="5fedd-184">Podmíněná nasazení prostředků</span><span class="sxs-lookup"><span data-stu-id="5fedd-184">Conditionally deploy resource</span></span>

<span data-ttu-id="5fedd-185">Chcete-li určit, jestli je nasazené prostředku, použijte `condition` elementu.</span><span class="sxs-lookup"><span data-stu-id="5fedd-185">To specify whether a resource is deployed, use the `condition` element.</span></span> <span data-ttu-id="5fedd-186">Hodnota pro tento element překládá true nebo false.</span><span class="sxs-lookup"><span data-stu-id="5fedd-186">The value for this element resolves to true or false.</span></span> <span data-ttu-id="5fedd-187">Pokud je hodnota true, prostředek je nasazena.</span><span class="sxs-lookup"><span data-stu-id="5fedd-187">When the value is true, the resource is deployed.</span></span> <span data-ttu-id="5fedd-188">Pokud je hodnota false, prostředek není nasazen.</span><span class="sxs-lookup"><span data-stu-id="5fedd-188">When the value is false, the resource is not deployed.</span></span> <span data-ttu-id="5fedd-189">Například k určení, zda nový účet úložiště je nasazena nebo existující účet úložiště se používá, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="5fedd-189">For example, to specify whether a new storage account is deployed or an existing storage account is used, use:</span></span>

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

<span data-ttu-id="5fedd-190">Příklad použití nový nebo existující prostředek, naleznete v části [nový nebo stávající šablonu podmínku](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).</span><span class="sxs-lookup"><span data-stu-id="5fedd-190">For an example of using a new or existing resource, see [New or existing condition template](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).</span></span>

<span data-ttu-id="5fedd-191">Příklad použití heslo nebo klíč SSH pro virtuální počítač nejde nasadit, naleznete v části [uživatelské jméno nebo SSH podmínku šablony](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).</span><span class="sxs-lookup"><span data-stu-id="5fedd-191">For an example of using a password or SSH key to deploy virtual machine, see [Username or SSH condition template](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5fedd-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5fedd-192">Next steps</span></span>
* <span data-ttu-id="5fedd-193">Pokud chcete další informace o části šablony, najdete v části [vytváření šablon Azure Resource Manager](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="5fedd-193">If you want to learn about the sections of a template, see [Authoring Azure Resource Manager Templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="5fedd-194">Informace o nasazení šablony najdete v tématu [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="5fedd-194">To learn how to deploy your template, see [Deploy an application with Azure Resource Manager Template](resource-group-template-deploy.md).</span></span>

