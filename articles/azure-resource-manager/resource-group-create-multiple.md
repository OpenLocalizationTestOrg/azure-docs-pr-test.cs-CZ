---
title: "aaaDeploy více instancí prostředků Azure | Microsoft Docs"
description: "Pomocí operace kopírování a polí v tooiterate šablony Azure Resource Manager vícekrát při nasazení prostředků."
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
ms.openlocfilehash: a3bd42f694053317c30b639c33dc4efae41a9a9b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-multiple-instances-of-a-resource-or-property-in-azure-resource-manager-templates"></a><span data-ttu-id="48dee-103">Nasazení více instancí prostředek nebo vlastnost v šablonách Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="48dee-103">Deploy multiple instances of a resource or property in Azure Resource Manager templates</span></span>
<span data-ttu-id="48dee-104">Toto téma ukazuje, jak tooiterate ve vaší toocreate šablony Azure Resource Manager více instancí prostředku nebo více instancí vlastnosti prostředku.</span><span class="sxs-lookup"><span data-stu-id="48dee-104">This topic shows you how tooiterate in your Azure Resource Manager template toocreate multiple instances of a resource, or multiple instances of a property on a resource.</span></span>

<span data-ttu-id="48dee-105">Pokud je třeba tooadd logiku tooyour šablony, která vám umožní toospecify tom, jestli je nasazený prostředku, najdete v části [podmíněně nasazení prostředků](#conditionally-deploy-resource).</span><span class="sxs-lookup"><span data-stu-id="48dee-105">If you need tooadd logic tooyour template that enables you toospecify whether a resource is deployed, see [Conditionally deploy resource](#conditionally-deploy-resource).</span></span>

## <a name="resource-iteration"></a><span data-ttu-id="48dee-106">Iterace prostředků</span><span class="sxs-lookup"><span data-stu-id="48dee-106">Resource iteration</span></span>
<span data-ttu-id="48dee-107">Přidání více instancí typu prostředku, toocreate `copy` typ prostředku toohello elementu.</span><span class="sxs-lookup"><span data-stu-id="48dee-107">toocreate multiple instances of a resource type, add a `copy` element toohello resource type.</span></span> <span data-ttu-id="48dee-108">V elementu hello kopírování zadejte hello počet iterací a název pro tento smyčky.</span><span class="sxs-lookup"><span data-stu-id="48dee-108">In hello copy element, you specify hello number of iterations and a name for this loop.</span></span> <span data-ttu-id="48dee-109">Hodnota počtu Hello musí být kladné celé číslo a nesmí být delší než 800.</span><span class="sxs-lookup"><span data-stu-id="48dee-109">hello count value must be a positive integer and cannot exceed 800.</span></span> <span data-ttu-id="48dee-110">Resource Manager vytvoří hello prostředky paralelně.</span><span class="sxs-lookup"><span data-stu-id="48dee-110">Resource Manager creates hello resources in parallel.</span></span> <span data-ttu-id="48dee-111">Proto není zaručena hello pořadí, ve které byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="48dee-111">Therefore, hello order in which they are created is not guaranteed.</span></span> <span data-ttu-id="48dee-112">toocreate vstupní prostředky v pořadí, v tématu [sériové kopie](#serial-copy).</span><span class="sxs-lookup"><span data-stu-id="48dee-112">toocreate iterated resources in sequence, see [Serial copy](#serial-copy).</span></span> 

<span data-ttu-id="48dee-113">Hello prostředků toocreate vícekrát trvá hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="48dee-113">hello resource toocreate multiple times takes hello following format:</span></span>

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

<span data-ttu-id="48dee-114">Všimněte si, že hello název každého prostředku zahrnuje hello `copyIndex()` funkce, která vrací aktuální iterace hello ve smyčce hello.</span><span class="sxs-lookup"><span data-stu-id="48dee-114">Notice that hello name of each resource includes hello `copyIndex()` function, which returns hello current iteration in hello loop.</span></span> <span data-ttu-id="48dee-115">`copyIndex()`je počítáno od nuly.</span><span class="sxs-lookup"><span data-stu-id="48dee-115">`copyIndex()` is zero-based.</span></span> <span data-ttu-id="48dee-116">Ano, hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="48dee-116">So, hello following example:</span></span>

```json
"name": "[concat('storage', copyIndex())]",
```

<span data-ttu-id="48dee-117">Vytvoří tyto názvy:</span><span class="sxs-lookup"><span data-stu-id="48dee-117">Creates these names:</span></span>

* <span data-ttu-id="48dee-118">storage0</span><span class="sxs-lookup"><span data-stu-id="48dee-118">storage0</span></span>
* <span data-ttu-id="48dee-119">storage1</span><span class="sxs-lookup"><span data-stu-id="48dee-119">storage1</span></span>
* <span data-ttu-id="48dee-120">storage2.</span><span class="sxs-lookup"><span data-stu-id="48dee-120">storage2.</span></span>

<span data-ttu-id="48dee-121">hodnotu indexu hello toooffset, může předat hodnotu ve funkci copyIndex() hello.</span><span class="sxs-lookup"><span data-stu-id="48dee-121">toooffset hello index value, you can pass a value in hello copyIndex() function.</span></span> <span data-ttu-id="48dee-122">Hello počet iterací tooperform je stále zadaný v elementu hello kopírování, ale hodnota hello copyIndex je posunut hello zadaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="48dee-122">hello number of iterations tooperform is still specified in hello copy element, but hello value of copyIndex is offset by hello specified value.</span></span> <span data-ttu-id="48dee-123">Ano, hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="48dee-123">So, hello following example:</span></span>

```json
"name": "[concat('storage', copyIndex(1))]",
```

<span data-ttu-id="48dee-124">Vytvoří tyto názvy:</span><span class="sxs-lookup"><span data-stu-id="48dee-124">Creates these names:</span></span>

* <span data-ttu-id="48dee-125">storage1</span><span class="sxs-lookup"><span data-stu-id="48dee-125">storage1</span></span>
* <span data-ttu-id="48dee-126">storage2</span><span class="sxs-lookup"><span data-stu-id="48dee-126">storage2</span></span>
* <span data-ttu-id="48dee-127">storage3</span><span class="sxs-lookup"><span data-stu-id="48dee-127">storage3</span></span>

<span data-ttu-id="48dee-128">operace kopírování Hello je užitečné při práci s poli, protože můžete iterovat každý prvek v poli hello.</span><span class="sxs-lookup"><span data-stu-id="48dee-128">hello copy operation is helpful when working with arrays because you can iterate through each element in hello array.</span></span> <span data-ttu-id="48dee-129">Použití hello `length` funkce na hello pole toospecify hello počet iterací, a `copyIndex` tooretrieve hello aktuální index v poli hello.</span><span class="sxs-lookup"><span data-stu-id="48dee-129">Use hello `length` function on hello array toospecify hello count for iterations, and `copyIndex` tooretrieve hello current index in hello array.</span></span> <span data-ttu-id="48dee-130">Ano, hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="48dee-130">So, hello following example:</span></span>

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

<span data-ttu-id="48dee-131">Vytvoří tyto názvy:</span><span class="sxs-lookup"><span data-stu-id="48dee-131">Creates these names:</span></span>

* <span data-ttu-id="48dee-132">storagecontoso</span><span class="sxs-lookup"><span data-stu-id="48dee-132">storagecontoso</span></span>
* <span data-ttu-id="48dee-133">storagefabrikam</span><span class="sxs-lookup"><span data-stu-id="48dee-133">storagefabrikam</span></span>
* <span data-ttu-id="48dee-134">storagecoho</span><span class="sxs-lookup"><span data-stu-id="48dee-134">storagecoho</span></span>

## <a name="serial-copy"></a><span data-ttu-id="48dee-135">Kopírování sériového portu</span><span class="sxs-lookup"><span data-stu-id="48dee-135">Serial copy</span></span>

<span data-ttu-id="48dee-136">Při použití hello kopie element toocreate více instancí na typ prostředku, správce prostředků, ve výchozím nastavení, nasadí tyto instance paralelně.</span><span class="sxs-lookup"><span data-stu-id="48dee-136">When you use hello copy element toocreate multiple instances of a resource type, Resource Manager, by default, deploys those instances in parallel.</span></span> <span data-ttu-id="48dee-137">Ale můžete chtít toospecify této hello, že se prostředky nasadí v pořadí.</span><span class="sxs-lookup"><span data-stu-id="48dee-137">However, you may want toospecify that hello resources are deployed in sequence.</span></span> <span data-ttu-id="48dee-138">Například při aktualizaci provozním prostředí, můžete tak pouze několik aktualizací toostagger hello jsou aktualizovány v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="48dee-138">For example, when updating a production environment, you may want toostagger hello updates so only a certain number are updated at any one time.</span></span>

<span data-ttu-id="48dee-139">Resource Manager poskytuje vlastnosti hello kopie elementu, které umožňují tooserially můžete nasadit několik instancí.</span><span class="sxs-lookup"><span data-stu-id="48dee-139">Resource Manager provides properties on hello copy element that enable you tooserially deploy multiple instances.</span></span> <span data-ttu-id="48dee-140">V sadě hello kopírování elementů, `mode` příliš**sériové** a `batchSize` toohello počet instancí toodeploy najednou.</span><span class="sxs-lookup"><span data-stu-id="48dee-140">In hello copy element, set `mode` too**serial** and `batchSize` toohello number of instances toodeploy at a time.</span></span> <span data-ttu-id="48dee-141">Sériového portu v režimu Resource Manager vytvoří závislost na starší instancí ve smyčce hello tak nespustí jeden batch, dokud se nedokončí hello předchozí dávka.</span><span class="sxs-lookup"><span data-stu-id="48dee-141">With serial mode, Resource Manager creates a dependency on earlier instances in hello loop, so it does not start one batch until hello previous batch completes.</span></span>

```json
"copy": {
    "name": "iterator",
    "count": "[parameters('numberToDeploy')]",
    "mode": "serial",
    "batchSize": 2
},
```

<span data-ttu-id="48dee-142">Hello vlastnost režimu také přijímá **paralelní**, což je výchozí hodnota hello.</span><span class="sxs-lookup"><span data-stu-id="48dee-142">hello mode property also accepts **parallel**, which is hello default value.</span></span>

<span data-ttu-id="48dee-143">tootest sériové kopírování bez vytváření skutečné prostředků hello použít následující šablonu, která nasadí prázdný vnořené šablony:</span><span class="sxs-lookup"><span data-stu-id="48dee-143">tootest serial copy without creating actual resources, use hello following template that deploys empty nested templates:</span></span>

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

<span data-ttu-id="48dee-144">V historii nasazení hello Všimněte si, že hello vnořené nasazení se zpracovávají v pořadí.</span><span class="sxs-lookup"><span data-stu-id="48dee-144">In hello deployment history, notice that hello nested deployments are processed in sequence.</span></span>

![sériové nasazení](./media/resource-group-create-multiple/serial-copy.png)

<span data-ttu-id="48dee-146">Realističtější scénáři hello následující ukázka nasadí dvě instance v době virtuálního počítače s Linuxem z vnořené šablony:</span><span class="sxs-lookup"><span data-stu-id="48dee-146">For a more realistic scenario, hello following example deploys two instances at a time of a Linux VM from a nested template:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "adminUsername": {
            "type": "string",
            "metadata": {
                "description": "User name for hello Virtual Machine."
            }
        },
        "adminPassword": {
            "type": "securestring",
            "metadata": {
                "description": "Password for hello Virtual Machine."
            }
        },
        "dnsLabelPrefix": {
            "type": "string",
            "metadata": {
                "description": "Unique DNS Name for hello Public IP used tooaccess hello Virtual Machine."
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
                "description": "hello Ubuntu version for hello VM. This will pick a fully patched image of this given Ubuntu version."
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

## <a name="property-iteration"></a><span data-ttu-id="48dee-147">Vlastnost iterace</span><span class="sxs-lookup"><span data-stu-id="48dee-147">Property iteration</span></span>

<span data-ttu-id="48dee-148">Přidání více hodnot pro vlastnost prostředku, toocreate `copy` pole v elementu vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="48dee-148">toocreate multiple values for a property on a resource, add a `copy` array in hello properties element.</span></span> <span data-ttu-id="48dee-149">Toto pole obsahuje objekty a každý objekt má hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="48dee-149">This array contains objects, and each object has hello following properties:</span></span>

* <span data-ttu-id="48dee-150">název – název hello z hello vlastnost toocreate více hodnot pro</span><span class="sxs-lookup"><span data-stu-id="48dee-150">name - hello name of hello property toocreate multiple values for</span></span>
* <span data-ttu-id="48dee-151">počet - hello toocreate hodnoty</span><span class="sxs-lookup"><span data-stu-id="48dee-151">count - hello number of values toocreate</span></span>
* <span data-ttu-id="48dee-152">(vstup) – objekt, který obsahuje hello hodnoty tooassign toohello vlastnost</span><span class="sxs-lookup"><span data-stu-id="48dee-152">input - an object that contains hello values tooassign toohello property</span></span>  

<span data-ttu-id="48dee-153">Následující příklad ukazuje, jak Hello tooapply `copy` toohello dataDisks vlastnost na virtuálním počítači:</span><span class="sxs-lookup"><span data-stu-id="48dee-153">hello following example shows how tooapply `copy` toohello dataDisks property on a virtual machine:</span></span>

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

<span data-ttu-id="48dee-154">Všimněte si, že při použití `copyIndex` uvnitř iterace vlastnost, je nutné zadat název hello iterace hello.</span><span class="sxs-lookup"><span data-stu-id="48dee-154">Notice that when using `copyIndex` inside a property iteration, you must provide hello name of hello iteration.</span></span> <span data-ttu-id="48dee-155">Nemáte tooprovide hello název při použití s iterace prostředků.</span><span class="sxs-lookup"><span data-stu-id="48dee-155">You do not have tooprovide hello name when used with resource iteration.</span></span>

<span data-ttu-id="48dee-156">Správce prostředků rozšíří hello `copy` pole během nasazení.</span><span class="sxs-lookup"><span data-stu-id="48dee-156">Resource Manager expands hello `copy` array during deployment.</span></span> <span data-ttu-id="48dee-157">Název Hello hello pole bude hello název vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="48dee-157">hello name of hello array becomes hello name of hello property.</span></span> <span data-ttu-id="48dee-158">vstupní hodnoty Hello stát hello vlastnosti objektu.</span><span class="sxs-lookup"><span data-stu-id="48dee-158">hello input values become hello object properties.</span></span> <span data-ttu-id="48dee-159">Hello nasazení šablony se změní na:</span><span class="sxs-lookup"><span data-stu-id="48dee-159">hello deployed template becomes:</span></span>

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

<span data-ttu-id="48dee-160">Iterace prostředků a vlastnosti můžete použít společně.</span><span class="sxs-lookup"><span data-stu-id="48dee-160">You can use resource and property iteration together.</span></span> <span data-ttu-id="48dee-161">Odkaz na vlastnost hello iterace podle názvu.</span><span class="sxs-lookup"><span data-stu-id="48dee-161">Reference hello property iteration by name.</span></span>

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

<span data-ttu-id="48dee-162">Můžete zahrnout pouze jeden element kopie hello vlastnosti pro každého prostředku.</span><span class="sxs-lookup"><span data-stu-id="48dee-162">You can only include one copy element in hello properties for each resource.</span></span> <span data-ttu-id="48dee-163">toospecify smyčky iterace pro více než jednu vlastnost, definovat více objektů v poli Kopírovat hello.</span><span class="sxs-lookup"><span data-stu-id="48dee-163">toospecify an iteration loop for more than one property, define multiple objects in hello copy array.</span></span> <span data-ttu-id="48dee-164">Každý objekt je vstupní samostatně.</span><span class="sxs-lookup"><span data-stu-id="48dee-164">Each object is iterated separately.</span></span> <span data-ttu-id="48dee-165">Například toocreate více instancí obou hello `frontendIPConfigurations` vlastnost a hello `loadBalancingRules` vlastnost zařízení na Vyrovnávání zatížení, zadejte oba objekty v elementu jedna kopie:</span><span class="sxs-lookup"><span data-stu-id="48dee-165">For example, toocreate multiple instances of both hello `frontendIPConfigurations` property and hello `loadBalancingRules` property on a load balancer, define both objects in a single copy element:</span></span> 

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

## <a name="depend-on-resources-in-a-loop"></a><span data-ttu-id="48dee-166">Závisí na prostředky ve smyčce</span><span class="sxs-lookup"><span data-stu-id="48dee-166">Depend on resources in a loop</span></span>
<span data-ttu-id="48dee-167">Určíte, že je prostředek nenasadí po jiný prostředek pomocí hello `dependsOn` elementu.</span><span class="sxs-lookup"><span data-stu-id="48dee-167">You specify that a resource is deployed after another resource by using hello `dependsOn` element.</span></span> <span data-ttu-id="48dee-168">toodeploy na prostředek, který závisí na hello kolekce prostředků ve smyčce, zadejte jméno hello hello kopírovací smyčkou v elementu dependsOn hello.</span><span class="sxs-lookup"><span data-stu-id="48dee-168">toodeploy a resource that depends on hello collection of resources in a loop, provide hello name of hello copy loop in hello dependsOn element.</span></span> <span data-ttu-id="48dee-169">Hello následující příklad ukazuje, jak hello toodeploy tři účty úložiště před nasazením virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="48dee-169">hello following example shows how toodeploy three storage accounts before deploying hello Virtual Machine.</span></span> <span data-ttu-id="48dee-170">Hello úplné definice virtuálního počítače se nezobrazí.</span><span class="sxs-lookup"><span data-stu-id="48dee-170">hello full Virtual Machine definition is not shown.</span></span> <span data-ttu-id="48dee-171">Všimněte si tohoto prvku hello kopie má název nastaven příliš`storagecopy` a hello dependsOn element pro hello virtuálních počítačů je také nastaven příliš`storagecopy`.</span><span class="sxs-lookup"><span data-stu-id="48dee-171">Notice that hello copy element has name set too`storagecopy` and hello dependsOn element for hello Virtual Machines is also set too`storagecopy`.</span></span>

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

## <a name="create-multiple-instances-of-a-child-resource"></a><span data-ttu-id="48dee-172">Vytvoření více instancí podřízených prostředků</span><span class="sxs-lookup"><span data-stu-id="48dee-172">Create multiple instances of a child resource</span></span>
<span data-ttu-id="48dee-173">Kopírovací smyčkou nelze použít pro podřízený prostředek.</span><span class="sxs-lookup"><span data-stu-id="48dee-173">You cannot use a copy loop for a child resource.</span></span> <span data-ttu-id="48dee-174">toocreate více instancí na prostředek, který je obvykle definovat jako vnořené jiný prostředek, tento prostředek musíte vytvořit místo jako prostředek nejvyšší úrovně.</span><span class="sxs-lookup"><span data-stu-id="48dee-174">toocreate multiple instances of a resource that you typically define as nested within another resource, you must instead create that resource as a top-level resource.</span></span> <span data-ttu-id="48dee-175">Můžete definovat relaci hello s hello nadřazený prostředek prostřednictvím hello typ a název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="48dee-175">You define hello relationship with hello parent resource through hello type and name properties.</span></span>

<span data-ttu-id="48dee-176">Předpokládejme například, že definujete obvykle datovou sadu jako podřízený prostředek v rámci služby data factory.</span><span class="sxs-lookup"><span data-stu-id="48dee-176">For example, suppose you typically define a dataset as a child resource within a data factory.</span></span>

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

<span data-ttu-id="48dee-177">toocreate více instancí datových sad, přesuňte jej mimo objekt pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="48dee-177">toocreate multiple instances of data sets, move it outside of hello data factory.</span></span> <span data-ttu-id="48dee-178">Hello datovou sadu musí být na stejné úrovni jako objekt pro vytváření dat hello hello, ale je stále prostředek podřízeného objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="48dee-178">hello dataset must be at hello same level as hello data factory, but it is still a child resource of hello data factory.</span></span> <span data-ttu-id="48dee-179">Můžete zachovat hello vztah mezi datovou sadu a objektu pro vytváření dat prostřednictvím hello typ a název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="48dee-179">You preserve hello relationship between data set and data factory through hello type and name properties.</span></span> <span data-ttu-id="48dee-180">Vzhledem k tomu, že už se nedá odvodit typ od pozice v šabloně hello, je nutné zadat typ hello plně kvalifikovaný ve formátu hello: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.</span><span class="sxs-lookup"><span data-stu-id="48dee-180">Since type can no longer be inferred from its position in hello template, you must provide hello fully qualified type in hello format: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.</span></span>

<span data-ttu-id="48dee-181">tooestablish relaci nadřazený podřízený s instanci objektu pro vytváření dat hello, zadejte název pro hello datovou sadu, která zahrnuje název prostředku nadřazené hello.</span><span class="sxs-lookup"><span data-stu-id="48dee-181">tooestablish a parent/child relationship with an instance of hello data factory, provide a name for hello data set that includes hello parent resource name.</span></span> <span data-ttu-id="48dee-182">Použijte formát hello: `{parent-resource-name}/{child-resource-name}`.</span><span class="sxs-lookup"><span data-stu-id="48dee-182">Use hello format: `{parent-resource-name}/{child-resource-name}`.</span></span>  

<span data-ttu-id="48dee-183">Hello následující příklad ukazuje implementaci hello:</span><span class="sxs-lookup"><span data-stu-id="48dee-183">hello following example shows hello implementation:</span></span>

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

## <a name="conditionally-deploy-resource"></a><span data-ttu-id="48dee-184">Podmíněná nasazení prostředků</span><span class="sxs-lookup"><span data-stu-id="48dee-184">Conditionally deploy resource</span></span>

<span data-ttu-id="48dee-185">toospecify, jestli je nasazená prostředku, použít hello `condition` elementu.</span><span class="sxs-lookup"><span data-stu-id="48dee-185">toospecify whether a resource is deployed, use hello `condition` element.</span></span> <span data-ttu-id="48dee-186">Hello hodnota pro tento element překládá tootrue, nebo hodnotu NEPRAVDA.</span><span class="sxs-lookup"><span data-stu-id="48dee-186">hello value for this element resolves tootrue or false.</span></span> <span data-ttu-id="48dee-187">Když je hello hodnota true, je nasazený hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="48dee-187">When hello value is true, hello resource is deployed.</span></span> <span data-ttu-id="48dee-188">Když je hello hodnota false, není nasazen hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="48dee-188">When hello value is false, hello resource is not deployed.</span></span> <span data-ttu-id="48dee-189">Například toospecify zda nový účet úložiště je nasazena nebo existující účet úložiště se používá, použijte:</span><span class="sxs-lookup"><span data-stu-id="48dee-189">For example, toospecify whether a new storage account is deployed or an existing storage account is used, use:</span></span>

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

<span data-ttu-id="48dee-190">Příklad použití nový nebo existující prostředek, naleznete v části [nový nebo stávající šablonu podmínku](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).</span><span class="sxs-lookup"><span data-stu-id="48dee-190">For an example of using a new or existing resource, see [New or existing condition template](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).</span></span>

<span data-ttu-id="48dee-191">Příklad použití hesla nebo SSH klíče toodeploy virtuálního počítače, naleznete v části [uživatelské jméno nebo SSH podmínku šablony](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).</span><span class="sxs-lookup"><span data-stu-id="48dee-191">For an example of using a password or SSH key toodeploy virtual machine, see [Username or SSH condition template](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).</span></span>

## <a name="next-steps"></a><span data-ttu-id="48dee-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="48dee-192">Next steps</span></span>
* <span data-ttu-id="48dee-193">Pokud chcete toolearn o hello části šablony, najdete v části [vytváření šablon Azure Resource Manager](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="48dee-193">If you want toolearn about hello sections of a template, see [Authoring Azure Resource Manager Templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="48dee-194">toolearn jak toodeploy šablony, najdete v části [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="48dee-194">toolearn how toodeploy your template, see [Deploy an application with Azure Resource Manager Template](resource-group-template-deploy.md).</span></span>

