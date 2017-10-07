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
# <a name="deploy-multiple-instances-of-a-resource-or-property-in-azure-resource-manager-templates"></a>Nasazení více instancí prostředek nebo vlastnost v šablonách Azure Resource Manager
Toto téma ukazuje, jak tooiterate ve vaší toocreate šablony Azure Resource Manager více instancí prostředku nebo více instancí vlastnosti prostředku.

Pokud je třeba tooadd logiku tooyour šablony, která vám umožní toospecify tom, jestli je nasazený prostředku, najdete v části [podmíněně nasazení prostředků](#conditionally-deploy-resource).

## <a name="resource-iteration"></a>Iterace prostředků
Přidání více instancí typu prostředku, toocreate `copy` typ prostředku toohello elementu. V elementu hello kopírování zadejte hello počet iterací a název pro tento smyčky. Hodnota počtu Hello musí být kladné celé číslo a nesmí být delší než 800. Resource Manager vytvoří hello prostředky paralelně. Proto není zaručena hello pořadí, ve které byly vytvořeny. toocreate vstupní prostředky v pořadí, v tématu [sériové kopie](#serial-copy). 

Hello prostředků toocreate vícekrát trvá hello následující formát:

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

Všimněte si, že hello název každého prostředku zahrnuje hello `copyIndex()` funkce, která vrací aktuální iterace hello ve smyčce hello. `copyIndex()`je počítáno od nuly. Ano, hello následující ukázka:

```json
"name": "[concat('storage', copyIndex())]",
```

Vytvoří tyto názvy:

* storage0
* storage1
* storage2.

hodnotu indexu hello toooffset, může předat hodnotu ve funkci copyIndex() hello. Hello počet iterací tooperform je stále zadaný v elementu hello kopírování, ale hodnota hello copyIndex je posunut hello zadaná hodnota. Ano, hello následující ukázka:

```json
"name": "[concat('storage', copyIndex(1))]",
```

Vytvoří tyto názvy:

* storage1
* storage2
* storage3

operace kopírování Hello je užitečné při práci s poli, protože můžete iterovat každý prvek v poli hello. Použití hello `length` funkce na hello pole toospecify hello počet iterací, a `copyIndex` tooretrieve hello aktuální index v poli hello. Ano, hello následující ukázka:

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

Vytvoří tyto názvy:

* storagecontoso
* storagefabrikam
* storagecoho

## <a name="serial-copy"></a>Kopírování sériového portu

Při použití hello kopie element toocreate více instancí na typ prostředku, správce prostředků, ve výchozím nastavení, nasadí tyto instance paralelně. Ale můžete chtít toospecify této hello, že se prostředky nasadí v pořadí. Například při aktualizaci provozním prostředí, můžete tak pouze několik aktualizací toostagger hello jsou aktualizovány v daném okamžiku.

Resource Manager poskytuje vlastnosti hello kopie elementu, které umožňují tooserially můžete nasadit několik instancí. V sadě hello kopírování elementů, `mode` příliš**sériové** a `batchSize` toohello počet instancí toodeploy najednou. Sériového portu v režimu Resource Manager vytvoří závislost na starší instancí ve smyčce hello tak nespustí jeden batch, dokud se nedokončí hello předchozí dávka.

```json
"copy": {
    "name": "iterator",
    "count": "[parameters('numberToDeploy')]",
    "mode": "serial",
    "batchSize": 2
},
```

Hello vlastnost režimu také přijímá **paralelní**, což je výchozí hodnota hello.

tootest sériové kopírování bez vytváření skutečné prostředků hello použít následující šablonu, která nasadí prázdný vnořené šablony:

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

V historii nasazení hello Všimněte si, že hello vnořené nasazení se zpracovávají v pořadí.

![sériové nasazení](./media/resource-group-create-multiple/serial-copy.png)

Realističtější scénáři hello následující ukázka nasadí dvě instance v době virtuálního počítače s Linuxem z vnořené šablony:

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

## <a name="property-iteration"></a>Vlastnost iterace

Přidání více hodnot pro vlastnost prostředku, toocreate `copy` pole v elementu vlastnosti hello. Toto pole obsahuje objekty a každý objekt má hello následující vlastnosti:

* název – název hello z hello vlastnost toocreate více hodnot pro
* počet - hello toocreate hodnoty
* (vstup) – objekt, který obsahuje hello hodnoty tooassign toohello vlastnost  

Následující příklad ukazuje, jak Hello tooapply `copy` toohello dataDisks vlastnost na virtuálním počítači:

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

Všimněte si, že při použití `copyIndex` uvnitř iterace vlastnost, je nutné zadat název hello iterace hello. Nemáte tooprovide hello název při použití s iterace prostředků.

Správce prostředků rozšíří hello `copy` pole během nasazení. Název Hello hello pole bude hello název vlastnosti hello. vstupní hodnoty Hello stát hello vlastnosti objektu. Hello nasazení šablony se změní na:

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

Iterace prostředků a vlastnosti můžete použít společně. Odkaz na vlastnost hello iterace podle názvu.

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

Můžete zahrnout pouze jeden element kopie hello vlastnosti pro každého prostředku. toospecify smyčky iterace pro více než jednu vlastnost, definovat více objektů v poli Kopírovat hello. Každý objekt je vstupní samostatně. Například toocreate více instancí obou hello `frontendIPConfigurations` vlastnost a hello `loadBalancingRules` vlastnost zařízení na Vyrovnávání zatížení, zadejte oba objekty v elementu jedna kopie: 

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

## <a name="depend-on-resources-in-a-loop"></a>Závisí na prostředky ve smyčce
Určíte, že je prostředek nenasadí po jiný prostředek pomocí hello `dependsOn` elementu. toodeploy na prostředek, který závisí na hello kolekce prostředků ve smyčce, zadejte jméno hello hello kopírovací smyčkou v elementu dependsOn hello. Hello následující příklad ukazuje, jak hello toodeploy tři účty úložiště před nasazením virtuálního počítače. Hello úplné definice virtuálního počítače se nezobrazí. Všimněte si tohoto prvku hello kopie má název nastaven příliš`storagecopy` a hello dependsOn element pro hello virtuálních počítačů je také nastaven příliš`storagecopy`.

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

## <a name="create-multiple-instances-of-a-child-resource"></a>Vytvoření více instancí podřízených prostředků
Kopírovací smyčkou nelze použít pro podřízený prostředek. toocreate více instancí na prostředek, který je obvykle definovat jako vnořené jiný prostředek, tento prostředek musíte vytvořit místo jako prostředek nejvyšší úrovně. Můžete definovat relaci hello s hello nadřazený prostředek prostřednictvím hello typ a název vlastnosti.

Předpokládejme například, že definujete obvykle datovou sadu jako podřízený prostředek v rámci služby data factory.

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

toocreate více instancí datových sad, přesuňte jej mimo objekt pro vytváření dat hello. Hello datovou sadu musí být na stejné úrovni jako objekt pro vytváření dat hello hello, ale je stále prostředek podřízeného objektu pro vytváření dat hello. Můžete zachovat hello vztah mezi datovou sadu a objektu pro vytváření dat prostřednictvím hello typ a název vlastnosti. Vzhledem k tomu, že už se nedá odvodit typ od pozice v šabloně hello, je nutné zadat typ hello plně kvalifikovaný ve formátu hello: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.

tooestablish relaci nadřazený podřízený s instanci objektu pro vytváření dat hello, zadejte název pro hello datovou sadu, která zahrnuje název prostředku nadřazené hello. Použijte formát hello: `{parent-resource-name}/{child-resource-name}`.  

Hello následující příklad ukazuje implementaci hello:

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

## <a name="conditionally-deploy-resource"></a>Podmíněná nasazení prostředků

toospecify, jestli je nasazená prostředku, použít hello `condition` elementu. Hello hodnota pro tento element překládá tootrue, nebo hodnotu NEPRAVDA. Když je hello hodnota true, je nasazený hello prostředků. Když je hello hodnota false, není nasazen hello prostředků. Například toospecify zda nový účet úložiště je nasazena nebo existující účet úložiště se používá, použijte:

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

Příklad použití nový nebo existující prostředek, naleznete v části [nový nebo stávající šablonu podmínku](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).

Příklad použití hesla nebo SSH klíče toodeploy virtuálního počítače, naleznete v části [uživatelské jméno nebo SSH podmínku šablony](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).

## <a name="next-steps"></a>Další kroky
* Pokud chcete toolearn o hello části šablony, najdete v části [vytváření šablon Azure Resource Manager](resource-group-authoring-templates.md).
* toolearn jak toodeploy šablony, najdete v části [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).

