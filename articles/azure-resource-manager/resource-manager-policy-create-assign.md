---
title: "aaaAssign a spravovat zásady prostředků Azure | Microsoft Docs"
description: "Popisuje, jak tooapply skupin prostředků Azure zásady toosubscriptions a prostředků a jak zásady tooview prostředků."
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
ms.date: 07/26/2017
ms.author: tomfitz
ms.openlocfilehash: b6999b43bbcc80d2fde9911352fd4352fa453443
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="assign-and-manage-resource-policies"></a>Přiřadit a spravovat zásady prostředků

tooimplement zásady, je třeba provést tyto kroky:

1. Pokud již existuje v rámci vašeho předplatného, která splňuje vaše požadavky, zkontrolujte toosee zásad definice (včetně integrovaných zásad poskytovaný platformou Azure).
2. Pokud existuje, získáte její název.
3. Pokud žádný neexistuje, definovat pravidlo zásad hello textem JSON a přidejte jej jako definici zásady v rámci vašeho předplatného. Tento krok zpřístupní hello zásady pro přiřazení, ale nevztahuje hello pravidla tooyour předplatné.
4. V obou případech přiřadíte obor tooa zásady hello (například předplatné nebo prostředek skupiny). nevynutí se teď Hello pravidla zásad hello.

Tento článek se zaměřuje na hello kroky toocreate definici zásady a přiřazení této definici oboru tooa prostřednictvím REST API, Powershellu nebo rozhraní příkazového řádku Azure. Pokud dáváte přednost toouse hello portálu tooassign zásady, najdete v části [Azure pomocí portálu tooassign a spravovat zásady prostředků](resource-manager-policy-portal.md). V tomto článku není soustředit na hello syntaxe pro vytvoření definice zásady hello. Informace o syntaxi zásad najdete v tématu [přehled zásad prostředků](resource-manager-policy.md).

## <a name="rest-api"></a>REST API

### <a name="create-policy-definition"></a>Vytvoření definice zásady

Zásady můžete vytvořit pomocí hello [REST API pro definice zásady](/rest/api/resources/policydefinitions). Hello REST API vám umožňuje toocreate a odstraňte definice zásady a získat informace o existující definice.

Spusťte toocreate definici zásady:

```HTTP
PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}
```

Zahrnout podobné toohello v textu požadavku následující ukázka:

```json
{
  "properties": {
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "hello list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you toorestrict hello locations your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "in": "[parameters('allowedLocations')]"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

### <a name="assign-policy"></a>Přiřazení zásad

Můžete použít definice zásady hello v oboru hello potřeby prostřednictvím hello [REST API pro přiřazení zásad](/rest/api/resources/policyassignments). Hello REST API vám umožňuje toocreate odstranit přiřazení zásad a získat informace o existující přiřazení.

toocreate přiřazení zásady, spusťte:

```HTTP
PUT https://management.azure.com /subscriptions/{subscription-id}/providers/Microsoft.authorization/policyassignments/{policyAssignmentName}?api-version={api-version}
```

Hello {přiřazení zásady} je hello název přiřazení zásady hello.

Zahrnout podobné toohello v textu požadavku následující ukázka:

```json
{
  "properties":{
    "displayName":"West US only policy assignment on hello subscription ",
    "description":"Resources can only be provisioned in West US regions",
    "parameters": {
      "allowedLocations": { "value": ["northeurope", "westus"] }
     },
    "policyDefinitionId":"/subscriptions/{subscription-id}/providers/Microsoft.Authorization/policyDefinitions/{definition-name}",
      "scope":"/subscriptions/{subscription-id}"
  },
}
```

### <a name="view-policy"></a>Zobrazení zásad
tooget zásady a použít hello [získat definice zásady](https://docs.microsoft.com/rest/api/resources/policydefinitions#PolicyDefinitions_Get) operaci.

### <a name="get-aliases"></a>Získat aliasy
Můžete načíst aliasy prostřednictvím hello REST API:

```HTTP
GET /subscriptions/{id}/providers?$expand=resourceTypes/aliases&api-version=2015-11-01
```

Hello následující příklad ukazuje definici alias. Jak vidíte, definuje alias cesty v různých verzích rozhraní API, i když dojde ke změně názvu vlastnosti. 

```json
"aliases": [
    {
      "name": "Microsoft.Storage/storageAccounts/sku.name",
      "paths": [
        {
          "path": "properties.accountType",
          "apiVersions": [
            "2015-06-15",
            "2015-05-01-preview"
          ]
        },
        {
          "path": "sku.name",
          "apiVersions": [
            "2016-01-01"
          ]
        }
      ]
    }
]
```

## <a name="powershell"></a>PowerShell

Než budete pokračovat s příklady prostředí PowerShell text hello, zajistěte, aby byla [nainstalovaná nejnovější verze hello](/powershell/azure/install-azurerm-ps) prostředí Azure PowerShell. Ve verzi 3.6.0 byly přidány zásady parametry. Máte starší verzi, příklady hello vrátit že chyba naznačuje hello parametru nebyl nalezen.

### <a name="view-policy-definitions"></a>Definice zásad zobrazení
toosee všechny definice zásady v rámci vašeho předplatného, hello použijte následující příkaz:

```powershell
Get-AzureRmPolicyDefinition
```

Vrátí všechny dostupné zásady definice, včetně integrovaných zásad. Každá zásada se vrátí v hello následující formát:

```powershell
Name               : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceId         : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceName       : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceType       : Microsoft.Authorization/policyDefinitions
Properties         : @{displayName=Allowed locations; policyType=BuiltIn; description=This policy enables you to
                     restrict hello locations your organization can specify when deploying resources. Use tooenforce
                     your geo-compliance requirements.; parameters=; policyRule=}
PolicyDefinitionId : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
```

Před pokračováním toocreate definici zásady podívejte se na integrovaných zásad hello. Pokud zjistíte předdefinované zásady, které platí hello omezení, které potřebujete, můžete přeskočit vytvoření definice zásady. Místo toho přiřadíte obor toohello potřeby předdefinované zásady hello.

### <a name="create-policy-definition"></a>Vytvoření definice zásady
Můžete vytvořit definici zásady pomocí hello `New-AzureRmPolicyDefinition` rutiny.

```powershell
$definition = New-AzureRmPolicyDefinition -Name coolAccessTier -Description "Policy toospecify access tier." -Policy '{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "field": "kind",
        "equals": "BlobStorage"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/accessTier",
          "equals": "cool"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}'
```            

výstup Hello je uložen v `$definition` objekt, který se používá při přiřazování zásady. 

Místo zadání hello JSON jako parametr, můžete zadat hello cesta tooa .json souboru, který obsahuje pravidlo zásad hello.

```powershell
$definition = New-AzureRmPolicyDefinition -Name coolAccessTier -Description "Policy toospecify access tier." -Policy "c:\policies\coolAccessTier.json"
```

Hello následující příklad vytvoří definici zásady, který obsahuje parametry:

```powershell
$policy = '{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "not": {
                    "field": "location",
                    "in": "[parameters(''allowedLocations'')]"
                }
            }
        ]
    },
    "then": {
        "effect": "Deny"
    }
}'

$parameters = '{
    "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "hello list of locations that can be specified when deploying storage accounts.",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
    }
}' 

$definition = New-AzureRmPolicyDefinition -Name storageLocations -Description "Policy toospecify locations for storage accounts." -Policy $policy -Parameter $parameters 
```

### <a name="assign-policy"></a>Přiřazení zásad

Použít zásady hello v oboru hello potřeby pomocí hello `New-AzureRmPolicyAssignment` rutiny. Následující ukázka Hello přiřadí skupinu prostředků tooa hello zásad.

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
New-AzureRMPolicyAssignment -Name accessTierAssignment -Scope $rg.ResourceId -PolicyDefinition $definition
```

tooassign zásadu, která vyžaduje parametry, vytvořte a objekt s těmito hodnotami. Hello následující příklad načte předdefinovaných zásad a předá hodnoty parametrů:

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/e5662a6-4747-49cd-b67b-bf8b01975c4c
$array = @("West US", "West US 2")
$param = @{"listOfAllowedLocations"=$array}
New-AzureRMPolicyAssignment -Name locationAssignment -Scope $rg.ResourceId -PolicyDefinition $definition -PolicyParameterObject $param
```

### <a name="view-policy-assignment"></a>Přiřazení zásady zobrazení

tooget přiřazení konkrétní zásady, použijte:

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
(Get-AzureRmPolicyAssignment -Name accessTierAssignment -Scope $rg.ResourceId
```

pravidlo zásad tooview hello definice zásady, použijte:

```powershell
(Get-AzureRmPolicyDefinition -Name coolAccessTier).Properties.policyRule | ConvertTo-Json
```

### <a name="remove-policy-assignment"></a>Odebrat přiřazení zásady 

tooremove přiřazení zásady, použijte:

```powershell
Remove-AzureRmPolicyAssignment -Name regionPolicyAssignment -Scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="azure-cli"></a>Azure CLI

### <a name="view-policy-definitions"></a>Definice zásad zobrazení
toosee všechny definice zásady v rámci vašeho předplatného, hello použijte následující příkaz:

```azurecli
az policy definition list
```

Vrátí všechny dostupné zásady definice, včetně integrovaných zásad. Každá zásada se vrátí v hello následující formát:

```azurecli
{                                                            
  "description": "This policy enables you toorestrict hello locations your organization can specify when deploying resources. Use tooenforce your geo-compliance requirements.",                      
  "displayName": "Allowed locations",
  "id": "/providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c",
  "name": "e56962a6-4747-49cd-b67b-bf8b01975c4c",
  "policyRule": {
    "if": {
      "not": {
        "field": "location",
        "in": "[parameters('listOfAllowedLocations')]"
      }
    },
    "then": {
      "effect": "Deny"
    }
  },
  "policyType": "BuiltIn"
}
```

Před pokračováním toocreate definici zásady podívejte se na integrovaných zásad hello. Pokud zjistíte předdefinované zásady, které platí hello omezení, které potřebujete, můžete přeskočit vytvoření definice zásady. Místo toho přiřadíte obor toohello potřeby předdefinované zásady hello.

### <a name="create-policy-definition"></a>Vytvoření definice zásady

Můžete vytvořit definici zásady pomocí rozhraní příkazového řádku Azure pomocí příkazu definice zásady hello.

```azurecli
az policy definition create --name coolAccessTier --description "Policy toospecify access tier." --rules '{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "field": "kind",
        "equals": "BlobStorage"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/accessTier",
          "equals": "cool"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}'    
```

### <a name="assign-policy"></a>Přiřazení zásad

Můžete použít obor toohello potřeby zásady hello pomocí příkazu přiřazení zásad hello. Následující ukázka Hello přiřadí skupinu prostředků tooa zásad.

```azurecli
az policy assignment create --name coolAccessTierAssignment --policy coolAccessTier --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

### <a name="view-policy-assignment"></a>Přiřazení zásady zobrazení

tooview přiřazení zásady, zadejte název přiřazení zásady hello a oboru hello:

```azurecli
az policy assignment show --name coolAccessTierAssignment --scope "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}"
```

### <a name="remove-policy-assignment"></a>Odebrat přiřazení zásady 

tooremove přiřazení zásady, použijte:

```azurecli
az policy assignment delete --name coolAccessTier --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="next-steps"></a>Další kroky
* Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).

