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
# <a name="deployment-functions-for-azure-resource-manager-templates"></a>Nasazení funkce pro šablony Azure Resource Manager 

Resource Manager poskytuje hello následující funkce pro získání hodnoty z části hello šablony a hodnoty související toohello nasazení:

* [nasazení](#deployment)
* [Parametry](#parameters)
* [proměnné](#variables)

tooget hodnoty z prostředků, skupiny prostředků nebo předplatného, najdete v části [prostředků funkce](resource-group-template-functions-resource.md).

<a id="deployment" />

## <a name="deployment"></a>nasazení
`deployment()`

Vrací informace o aktuální operace nasazení hello.

### <a name="return-value"></a>Návratová hodnota

Tato funkce vrátí hello objekt, který je předán během nasazení. Vlastnosti Hello v hello vrátil objekt se liší v závislosti na tom, jestli hello nasazení je předán objekt jako odkaz nebo jako objekt v řádku. Pokud objekt nasazení hello je předán v řádku, jako třeba při použití hello **- TemplateFile** parametr v prostředí Azure PowerShell toopoint tooa místního souboru, hello vrátí objekt má hello následující formát:

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

Pokud objekt hello je předán jako odkaz, například při použití hello **- TemplateUri** parametr toopoint tooa vzdáleného objektu, je vrácen objekt hello v hello následující formát: 

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

### <a name="remarks"></a>Poznámky

Můžete použít deployment() toolink tooanother šablony založené na hello URI hello nadřazené šablony.

```json
"variables": {  
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"  
}
```  

### <a name="example"></a>Příklad

Hello následující příklad vrací objekt nasazení hello:

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

Hello předchozí příklad vrací hello následujících objektů:

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

## <a name="parameters"></a>parameters
`parameters(parameterName)`

Vrátí hodnotu parametru. Hello zadaný název parametru musí být definovány v části Parametry hello hello šablony.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| Název parametru |Ano |Řetězec |Název Hello tooreturn parametr hello. |

### <a name="return-value"></a>Návratová hodnota

parametr zadána hodnota Hello hello.

### <a name="remarks"></a>Poznámky

Parametry tooset prostředků hodnoty se obvykle používá. Hello následující příklad ilustruje hello název webu toohello předaná hodnota parametru v během nasazování.

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

### <a name="example"></a>Příklad

Hello následující příklad ukazuje zjednodušený použití hello parametry funkce.

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

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| stringOutput | Řetězec | možnost 1 |
| intOutput | celá čísla | 1 |
| objectOutput | Objekt | {"1": "a", "dva": "b"} |
| arrayOutput | Pole | [1, 2, 3] |
| crossOutput | Řetězec | možnost 1 |

<a id="variables" />

## <a name="variables"></a>proměnné
`variables(variableName)`

Vrátí hello hodnotu proměnné. Zadaný název proměnné Hello musí být definovány v části proměnných hello hello šablony.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| NázevProměnné |Ano |Řetězec |Název proměnné tooreturn hello Hello. |

### <a name="return-value"></a>Návratová hodnota

Proměnná zadána hodnota Hello hello.

### <a name="remarks"></a>Poznámky

Obvykle použijete toosimplify proměnných šablony vytvořením komplexní hodnoty jenom jednou. Hello následující příklad vytvoří jedinečný název pro účet úložiště.

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

### <a name="example"></a>Příklad

Příklad šablony Hello vrátí různé hodnoty proměnné.

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

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| exampleOutput1 | Řetězec | myVariable |
| exampleOutput2 | Pole | [1, 2, 3, 4] |
| exampleOutput3 | Řetězec | myVariable |
| exampleOutput4 |  Objekt | {"vlastnost1": "value1", "vlastnost2": "hodnota2"} |

## <a name="next-steps"></a>Další kroky
* Popis části hello šablonu Azure Resource Manager naleznete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).
* toomerge několik šablon, najdete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).
* tooiterate zadaného počtu opakování při vytváření typu prostředku, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).
* toosee způsobu toodeploy hello šablony vytvoříte, najdete v [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).

