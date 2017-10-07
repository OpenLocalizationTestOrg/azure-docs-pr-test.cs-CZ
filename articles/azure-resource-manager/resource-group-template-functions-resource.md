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
# <a name="resource-functions-for-azure-resource-manager-templates"></a>Funkce prostředků pro šablony Azure Resource Manager

Resource Manager poskytuje následující funkce pro načtení prostředků hodnot hello:

* [listKeys a seznamu {Value}](#listkeys)
* [Zprostředkovatelé](#providers)
* [referenční dokumentace](#reference)
* [Skupina prostředků](#resourcegroup)
* [ID prostředku](#resourceid)
* [předplatné](#subscription)

tooget hodnoty z parametrů, proměnné nebo hello aktuální nasazení, najdete v části [funkce hodnota nasazení](resource-group-template-functions-deployment.md).

<a id="listkeys" />
<a id="list" />

## <a name="listkeys-and-listvalue"></a>listKeys a seznamu {Value}
`listKeys(resourceName or resourceIdentifier, apiVersion)`

`list{Value}(resourceName or resourceIdentifier, apiVersion)`

Vrátí hello hodnoty pro libovolný typ prostředku, který podporuje hello seznamu operaci. Nejběžnější využití Hello `listKeys`. 

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| resourceName nebo resourceIdentifier |Ano |Řetězec |Jedinečný identifikátor pro prostředek hello. |
| apiVersion |Ano |Řetězec |Verze rozhraní API stav modulu runtime prostředků. Obvykle ve formátu hello **rrrr mm-dd**. |

### <a name="return-value"></a>Návratová hodnota

Hello vrátit objekt z listKeys má hello následující formát:

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

Jiné funkce seznamu mají návratový různých formátech. Formát hello toosee funkce, její zahrnutí do části výstupy hello jak je znázorněno v příkladu šablony hello. 

### <a name="remarks"></a>Poznámky

Všechny operace, který začíná **seznamu** lze použít jako funkci ve vaší šabloně. Hello k dispozici operace zahrnují nejen listKeys, ale také operace jako `list`, `listAdminKeys`, a `listStatus`. Ale nemůžete použít **seznamu** činnosti, které vyžadují hodnoty v hello text žádosti. Například hello [SAS účtu seznamu](/rest/api/storagerp/storageaccounts#StorageAccounts_ListAccountSAS) operace vyžaduje parametry text požadavku jako *signedExpiry*, takže ji nelze použít v rámci šablon.

jaké typy prostředků obsahovat operaci seznamu toodetermine, máte hello následující možnosti:

* Zobrazení hello [operace REST API](/rest/api/) pro poskytovatele prostředků a podívejte se na seznam operací. Účty úložiště mít například hello [listKeys operaci](/rest/api/storagerp/storageaccounts#StorageAccounts_ListKeys).
* Použití hello [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) rutiny prostředí PowerShell. Hello následující příklad načte všechny operace výpisu pro účty úložiště:

  ```powershell
  Get-AzureRmProviderOperation -OperationSearchString "Microsoft.Storage/*" | where {$_.Operation -like "*list*"} | FT Operation
  ```
* Použijte následující příkaz rozhraní příkazového řádku Azure toofilter pouze hello operace výpisu hello:

  ```azurecli
  az provider operation show --namespace Microsoft.Storage --query "resourceTypes[?name=='storageAccounts'].operations[].name | [?contains(@, 'list')]"
  ```

Zadejte hello prostředků pomocí buď hello [resourceId funkce](#resourceid), nebo hello formát `{providerNamespace}/{resourceType}/{resourceName}`.


### <a name="example"></a>Příklad

Hello následující příklad ukazuje, jak tooreturn hello primární a sekundární klíče z účtu úložiště v hello výstupy části.

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

## <a name="providers"></a>Zprostředkovatelé
`providers(providerNamespace, [resourceType])`

Vrátí informace o poskytovatele prostředků a jeho typy podporovaných zdrojů. Pokud není zadán typ prostředku, vrátí funkce hello všechny typy hello podporované pro zprostředkovatele prostředků hello.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| providerNamespace |Ano |Řetězec |Namespace hello zprostředkovatele |
| Typ prostředku |Ne |Řetězec |Typ prostředku v rámci hello Hello zadat obor názvů. |

### <a name="return-value"></a>Návratová hodnota

Každý podporovaný typ je vrácený v hello následující formát: 

```json
{
    "resourceType": "{name of resource type}",
    "locations": [ all supported locations ],
    "apiVersions": [ all supported API versions ]
}
```

Pole řazení hello vrátil hodnoty není zaručena.

### <a name="example"></a>Příklad

Hello následující příklad ukazuje, jak toouse hello zprostředkovatele funkce:

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

Hello předchozí příklad vrátí objekt ve formátu hello:

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

## <a name="reference"></a>Referenční dokumentace
`reference(resourceName or resourceIdentifier, [apiVersion])`

Vrátí objekt představující stav modulu runtime prostředku.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| resourceName nebo resourceIdentifier |Ano |Řetězec |Název nebo identifikátor prostředku. |
| apiVersion |Ne |Řetězec |Verze rozhraní API hello zadaný prostředek. Zahrnout tento parametr hello prostředků není zřízený v rámci stejné šablony. Obvykle ve formátu hello **rrrr mm-dd**. |

### <a name="return-value"></a>Návratová hodnota

Každý typ prostředku vrátí různé vlastnosti pro hello odkaz funkce. Funkce Hello nevrací předdefinovaný formát. toosee hello vlastnosti pro typ prostředku, vrátí, že objekt hello v hello výstupy části, jak je znázorněno v příkladu hello.

### <a name="remarks"></a>Poznámky

Funkce odkaz Hello odvozuje svou hodnotu z stav modulu runtime a proto jej nelze použít v části proměnných hello. V části výstupů šablony slouží. 

Pomocí funkce odkaz hello je implicitně deklarovat, že jeden prostředek závisí na jiný prostředek, pokud hello odkazuje prostředků je zřízený v rámci stejné šablony. Není nutné tooalso použití hello dependsOn vlastnost. Hello funkce, nebude hodnocen až hello odkazovaných prostředků po dokončení nasazení.

toosee hello názvy a hodnoty vlastností pro typ prostředku, vytvořit šablonu, která vrací objekt hello v části výstupy hello. Pokud máte existující prostředek daného typu, šablony vrátí objekt hello bez nasazení žádné nové prostředky. 

Obvykle použijete hello **odkaz** funkce tooreturn určitou hodnotu z objektu, například koncový bod objektů blob hello URI nebo plně kvalifikovaný název domény.

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

### <a name="example"></a>Příklad

referenční dokumentace a toodeploy hello prostředku v hello stejné šablony, použijte:

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

Hello předchozí příklad vrátí objekt ve formátu hello:

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

Hello následujícím příkladu odkazuje na účet úložiště, který není nasazený v této šabloně. Hello účet úložiště už existuje v rámci hello stejné skupiny prostředků.

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

## <a name="resourcegroup"></a>Skupina prostředků
`resourceGroup()`

Vrátí objekt, který představuje aktuální skupině prostředků hello. 

### <a name="return-value"></a>Návratová hodnota

Hello vrátit objekt je ve formátu hello:

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

### <a name="remarks"></a>Poznámky

Běžně se používají funkce resourceGroup hello je toocreate prostředky v hello stejné umístění jako skupina prostředků hello. Hello následující příklad používá umístění tooassign hello skupiny umístění hello prostředků pro webovou stránku.

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

### <a name="example"></a>Příklad

Hello následující šablony vrátí hello vlastnosti skupiny zdrojů hello.

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

Hello předchozí příklad vrátí objekt ve formátu hello:

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

## <a name="resourceid"></a>resourceId
`resourceId([subscriptionId], [resourceGroupName], resourceType, resourceName1, [resourceName2]...)`

Vrátí hello jedinečný identifikátor prostředku. Tuto funkci použít, když se název prostředku hello nejednoznačný nebo není zřízené v rámci hello stejné šablony. 

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| subscriptionId |Ne |řetězec (ve formátu identifikátoru GUID) |Výchozí hodnota je aktuální předplatné hello. Tuto hodnotu zadejte, když potřebujete tooretrieve prostředku v jiné předplatné. |
| Název skupiny prostředků |Ne |Řetězec |Výchozí hodnota je aktuální skupině prostředků. Tuto hodnotu zadejte, když potřebujete tooretrieve prostředku v jiné skupině prostředků. |
| Typ prostředku |Ano |Řetězec |Typ prostředku, včetně obor názvů zprostředkovatele prostředků. |
| resourceName1 |Ano |Řetězec |Název prostředku. |
| resourceName2 |Ne |Řetězec |Další prostředků název segment Pokud je vnořený prostředek. |

### <a name="return-value"></a>Návratová hodnota

identifikátor Hello je vrácený v hello následující formát:

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

### <a name="remarks"></a>Poznámky

Hello hodnoty parametrů můžete zadat závisí na tom, zda text hello prostředků v hello stejné předplatném nebo skupině prostředků jako hello aktuální nasazení.

tooget hello prostředků ID pro účet úložiště na hello stejné předplatné a skupina prostředků, použijte:

```json
"[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]"
```

ID prostředku hello tooget pro účet úložiště na hello stejného předplatného, ale jiné skupině prostředků, použijte:

```json
"[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

ID prostředku hello tooget pro účet úložiště na jiné předplatné a skupina prostředků, použijte:

```json
"[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

ID prostředku hello tooget pro databázi v jiné skupině prostředků, použijte:

```json
"[resourceId('otherResourceGroup', 'Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]"
```

Často musíte toouse tuto funkci při použití účtu úložiště nebo virtuální sítě ve skupině alternativní prostředků. Hello následující příklad ukazuje, jak lze prostředků ze skupiny pro externí zdroj snadno použít:

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

### <a name="example"></a>Příklad

Hello následující příklad vrátí hello ID prostředku pro účet úložiště ve skupině prostředků hello:

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

Hello výstup z hello předchozí příklad s hello výchozí hodnoty je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| sameRGOutput | Řetězec | /subscriptions/{Current-Sub-ID}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentRGOutput | Řetězec | /subscriptions/{Current-Sub-ID}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentSubOutput | Řetězec | /subscriptions/{different-Sub-ID}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| nestedResourceOutput | Řetězec | /subscriptions/{Current-Sub-ID}/resourceGroups/examplegroup/providers/Microsoft.SQL/Servers/servername/Databases/databaseName |

<a id="subscription" />

## <a name="subscription"></a>předplatné
`subscription()`

Vrátí podrobnosti o předplatném hello hello aktuální nasazení. 

### <a name="return-value"></a>Návratová hodnota

Funkce Hello vrací hello následující formát:

```json
{
    "id": "/subscriptions/{subscription-id}",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}",
    "displayName": "{name-of-subscription}"
}
```

### <a name="example"></a>Příklad

Hello následující příklad ukazuje funkce předplatné hello volat v části výstupy hello. 

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

## <a name="next-steps"></a>Další kroky
* Popis části hello šablonu Azure Resource Manager naleznete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).
* toomerge několik šablon, najdete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).
* tooiterate zadaného počtu opakování při vytváření typu prostředku, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).
* toosee způsobu toodeploy hello šablony vytvoříte, najdete v [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).

