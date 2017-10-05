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
# <a name="resource-functions-for-azure-resource-manager-templates"></a>Funkce prostředků pro šablony Azure Resource Manager

Resource Manager poskytuje následující funkce pro získání hodnoty prostředku:

* [listKeys a seznamu {Value}](#listkeys)
* [Zprostředkovatelé](#providers)
* [referenční dokumentace](#reference)
* [Skupina prostředků](#resourcegroup)
* [ID prostředku](#resourceid)
* [předplatné](#subscription)

Chcete-li získat hodnoty z parametrů, proměnné nebo aktuální nasazení, přečtěte si téma [funkce hodnota nasazení](resource-group-template-functions-deployment.md).

<a id="listkeys" />
<a id="list" />

## <a name="listkeys-and-listvalue"></a>listKeys a seznamu {Value}
`listKeys(resourceName or resourceIdentifier, apiVersion)`

`list{Value}(resourceName or resourceIdentifier, apiVersion)`

Vrátí hodnoty pro libovolný typ prostředku, který podporuje operaci seznamu. Nejběžnější využití `listKeys`. 

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| resourceName nebo resourceIdentifier |Ano |Řetězec |Jedinečný identifikátor pro prostředek. |
| apiVersion |Ano |Řetězec |Verze rozhraní API stav modulu runtime prostředků. Obvykle ve formátu **rrrr mm-dd**. |

### <a name="return-value"></a>Návratová hodnota

Objekt vrácený z listKeys má následující formát:

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

Jiné funkce seznamu mají návratový různých formátech. Pokud chcete zobrazit formát funkce, její zahrnutí do části výstupy jak je znázorněno v příkladu šablony. 

### <a name="remarks"></a>Poznámky

Všechny operace, který začíná **seznamu** lze použít jako funkci ve vaší šabloně. K dispozici operace zahrnují nejen listKeys, ale také operace jako `list`, `listAdminKeys`, a `listStatus`. Ale nemůžete použít **seznamu** činnosti, které vyžadují hodnoty v textu požadavku. Například [SAS účtu seznamu](/rest/api/storagerp/storageaccounts#StorageAccounts_ListAccountSAS) operace vyžaduje parametry text požadavku jako *signedExpiry*, takže ji nelze použít v rámci šablon.

Pokud chcete zjistit, jaké typy prostředků obsahovat operaci seznamu, máte následující možnosti:

* Zobrazení [operace REST API](/rest/api/) pro poskytovatele prostředků a podívejte se na seznam operací. Například účty úložiště mít [listKeys operaci](/rest/api/storagerp/storageaccounts#StorageAccounts_ListKeys).
* Použití [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) rutiny prostředí PowerShell. Následující příklad načte všechny operace výpisu pro účty úložiště:

  ```powershell
  Get-AzureRmProviderOperation -OperationSearchString "Microsoft.Storage/*" | where {$_.Operation -like "*list*"} | FT Operation
  ```
* Pomocí následujícího příkazu příkazového řádku Azure CLI filtrovat seznam způsobů:

  ```azurecli
  az provider operation show --namespace Microsoft.Storage --query "resourceTypes[?name=='storageAccounts'].operations[].name | [?contains(@, 'list')]"
  ```

Zadejte prostředek pomocí [resourceId funkce](#resourceid), formát nebo `{providerNamespace}/{resourceType}/{resourceName}`.


### <a name="example"></a>Příklad

Následující příklad ukazuje, jak vrátit primární a sekundární klíče z účtu úložiště v části výstupy.

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

Vrátí informace o poskytovatele prostředků a jeho typy podporovaných zdrojů. Pokud není zadán typ prostředku, funkce vrátí všechny podporované typy pro poskytovatele prostředků.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| providerNamespace |Ano |Řetězec |Namespace zprostředkovatele |
| Typ prostředku |Ne |Řetězec |Typ prostředku v rámci zadaného oboru názvů. |

### <a name="return-value"></a>Návratová hodnota

Každý podporovaný typ je vrácený v následujícím formátu: 

```json
{
    "resourceType": "{name of resource type}",
    "locations": [ all supported locations ],
    "apiVersions": [ all supported API versions ]
}
```

Řazení pole vrácené hodnoty není zaručena.

### <a name="example"></a>Příklad

Následující příklad ukazuje, jak chcete používat funkci zprostředkovatele:

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

V předchozím příkladu vrátí objekt v následujícím formátu:

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
| apiVersion |Ne |Řetězec |Verze rozhraní API zadaný prostředek. Tento parametr zahrnout do prostředku není zřízený v rámci stejné šablony. Obvykle ve formátu **rrrr mm-dd**. |

### <a name="return-value"></a>Návratová hodnota

Každý typ prostředku vrátí různé vlastnosti pro odkaz funkce. Funkce nevrací předdefinovaný formát. Pokud chcete zobrazit vlastnosti pro typ prostředku, vrátí objekt v části výstupy, jak je znázorněno v příkladu.

### <a name="remarks"></a>Poznámky

Funkce odkaz odvozuje svou hodnotu z stav modulu runtime a proto jej nelze použít v sekci proměnných. V části výstupů šablony slouží. 

Pomocí funkce odkaz je implicitně deklarovat, že jeden prostředek závisí na jiný prostředek, pokud odkazovaného prostředku je zřízený v rámci stejné šablony. Není nutné také používat vlastnost dependsOn. Funkce, nebude hodnocen až odkazovaných prostředků po dokončení nasazení.

Pokud chcete zobrazit názvy a hodnoty pro typ prostředku, vytvořte šablonu, která vrací objekt v části výstupy. Pokud máte existující prostředek daného typu, šablony vrátí objekt bez nasazení žádné nové prostředky. 

Obvykle se používá **odkaz** funkci vrátíte konkrétní hodnotu z objektu, například identifikátor URI koncového bodu objektu blob nebo plně kvalifikovaný název domény.

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

K nasazení a odkaz na zdroj v stejné šablony, použijte:

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

V předchozím příkladu vrátí objekt v následujícím formátu:

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

V následujícím příkladu odkazuje na účet úložiště, který není nasazený v této šabloně. V rámci stejné skupině prostředků už existuje účet úložiště.

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

Vrátí objekt, který představuje aktuální skupině prostředků. 

### <a name="return-value"></a>Návratová hodnota

Vrácený objekt je v následujícím formátu:

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

Běžně se používají funkce resourceGroup je vytvoření prostředků ve stejném umístění jako pro skupinu prostředků. Následující příklad používá umístění skupiny prostředků přiřadit umístění pro webový server.

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

Následující šablony vrací vlastnosti skupiny prostředků.

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

V předchozím příkladu vrátí objekt v následujícím formátu:

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

Vrací jedinečný identifikátor prostředku. Tuto funkci použít, když se název prostředku nejednoznačný nebo není zřízené v rámci stejné šablony. 

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Popis |
|:--- |:--- |:--- |:--- |
| subscriptionId |Ne |řetězec (ve formátu identifikátoru GUID) |Výchozí hodnota je aktuální předplatné. Tuto hodnotu zadejte, když potřebujete načíst prostředku v jiné předplatné. |
| Název skupiny prostředků |Ne |Řetězec |Výchozí hodnota je aktuální skupině prostředků. Tuto hodnotu zadejte, když potřebujete načíst prostředek v jiné skupině prostředků. |
| Typ prostředku |Ano |Řetězec |Typ prostředku, včetně obor názvů zprostředkovatele prostředků. |
| resourceName1 |Ano |Řetězec |Název prostředku. |
| resourceName2 |Ne |Řetězec |Další prostředků název segment Pokud je vnořený prostředek. |

### <a name="return-value"></a>Návratová hodnota

Identifikátor se vrátí v následujícím formátu:

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

### <a name="remarks"></a>Poznámky

Hodnoty parametrů, které zadáte závisí na tom, zda prostředek je ve stejné skupině předplatného a prostředků pro aktuální nasazení.

Pokud chcete získat ID prostředku pro účet úložiště ve stejném předplatném a skupině prostředků, použijte:

```json
"[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]"
```

Pokud chcete získat ID prostředku pro účet úložiště v rámci stejného předplatného, ale jiné skupině prostředků, použijte:

```json
"[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

Pokud chcete získat ID prostředku pro účet úložiště na jiné předplatné a skupina prostředků, použijte:

```json
"[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

ID prostředku pro databázi v jiné skupině prostředků, použijte:

```json
"[resourceId('otherResourceGroup', 'Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]"
```

Často musíte tuto funkci použít, pokud používáte účet úložiště nebo virtuální sítě ve skupině alternativní prostředků. Následující příklad ukazuje, jak lze prostředků ze skupiny pro externí zdroj snadno použít:

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

Následující příklad vrací ID prostředku pro účet úložiště ve skupině prostředků:

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

Výstup z předchozího příkladu s výchozími hodnotami je:

| Name (Název) | Typ | Hodnota |
| ---- | ---- | ----- |
| sameRGOutput | Řetězec | /subscriptions/{Current-Sub-ID}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentRGOutput | Řetězec | /subscriptions/{Current-Sub-ID}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentSubOutput | Řetězec | /subscriptions/{different-Sub-ID}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| nestedResourceOutput | Řetězec | /subscriptions/{Current-Sub-ID}/resourceGroups/examplegroup/providers/Microsoft.SQL/Servers/servername/Databases/databaseName |

<a id="subscription" />

## <a name="subscription"></a>předplatné
`subscription()`

Vrátí podrobnosti o předplatném pro aktuální nasazení. 

### <a name="return-value"></a>Návratová hodnota

Funkce vrátí hodnotu v následujícím formátu:

```json
{
    "id": "/subscriptions/{subscription-id}",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}",
    "displayName": "{name-of-subscription}"
}
```

### <a name="example"></a>Příklad

Následující příklad ukazuje volaná v části výstupy funkce předplatného. 

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
* Popis v částech šablonu Azure Resource Manager naleznete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).
* Sloučit několik šablon, najdete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).
* K iteraci v zadaného počtu opakování při vytváření typu prostředku, najdete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).
* Postup nasazení šablony, které jste vytvořili, najdete v sekci [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).

