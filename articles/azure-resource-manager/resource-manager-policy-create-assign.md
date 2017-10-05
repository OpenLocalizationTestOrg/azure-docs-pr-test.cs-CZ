---
title: "Přiřadit a spravovat zásady prostředků Azure | Microsoft Docs"
description: "Popisuje, jak chcete použít zásady prostředků Azure pro skupiny prostředků a předplatná a postup zobrazení zásad prostředků."
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
ms.openlocfilehash: b204cffa8fab0ad27a9f78a81c04f0a0225d95f5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="assign-and-manage-resource-policies"></a><span data-ttu-id="4a9b3-103">Přiřadit a spravovat zásady prostředků</span><span class="sxs-lookup"><span data-stu-id="4a9b3-103">Assign and manage resource policies</span></span>

<span data-ttu-id="4a9b3-104">Chcete-li implementovat zásady, musíte provést tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="4a9b3-104">To implement a policy, you must perform these steps:</span></span>

1. <span data-ttu-id="4a9b3-105">Definice zásad (včetně integrovaných zásad poskytovaný platformou Azure) zkontrolujte, zda jeden již existuje v rámci vašeho předplatného, která splňuje vaše požadavky.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-105">Check policy definitions (including built-in policies provided by Azure) to see if one already exists in your subscription that fulfills your requirements.</span></span>
2. <span data-ttu-id="4a9b3-106">Pokud existuje, získáte její název.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-106">If one exists, get its name.</span></span>
3. <span data-ttu-id="4a9b3-107">Pokud žádný neexistuje, definovat pravidlo zásad textem JSON a přidejte jej jako definici zásady v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-107">If one does not exist, define the policy rule with JSON, and add it as a policy definition in your subscription.</span></span> <span data-ttu-id="4a9b3-108">Tento krok zpřístupní zásady pro přiřazení, ale není použití pravidel pro vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-108">This step makes the policy available for assignment but does not apply the rules to your subscription.</span></span>
4. <span data-ttu-id="4a9b3-109">V obou případech přiřaďte zásady oboru (například předplatné nebo prostředek skupiny).</span><span class="sxs-lookup"><span data-stu-id="4a9b3-109">For either case, assign the policy to a scope (such as a subscription or resource group).</span></span> <span data-ttu-id="4a9b3-110">Pravidla zásad se teď vynucují.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-110">The rules of the policy are now enforced.</span></span>

<span data-ttu-id="4a9b3-111">Tento článek se zaměřuje na kroky k vytvoření definice zásady a přiřazení této definici obor prostřednictvím REST API, Powershellu nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-111">This article focuses on the steps to create a policy definition and assign that definition to a scope through REST API, PowerShell, or Azure CLI.</span></span> <span data-ttu-id="4a9b3-112">Pokud byste radši chtěli použít portálu přiřadit zásady, najdete v článku [portálu Azure použijte přiřadit a spravovat zásady prostředků](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4a9b3-112">If you prefer to use the portal to assign policies, see [Use Azure portal to assign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="4a9b3-113">V tomto článku není soustředit na syntaxi pro vytvoření definice zásady.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-113">This article does not focus on the syntax for creating the policy definition.</span></span> <span data-ttu-id="4a9b3-114">Informace o syntaxi zásad najdete v tématu [přehled zásad prostředků](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="4a9b3-114">For information about policy syntax, see [Resource policy overview](resource-manager-policy.md).</span></span>

## <a name="rest-api"></a><span data-ttu-id="4a9b3-115">REST API</span><span class="sxs-lookup"><span data-stu-id="4a9b3-115">REST API</span></span>

### <a name="create-policy-definition"></a><span data-ttu-id="4a9b3-116">Vytvoření definice zásady</span><span class="sxs-lookup"><span data-stu-id="4a9b3-116">Create policy definition</span></span>

<span data-ttu-id="4a9b3-117">Můžete vytvořit a zásadou [REST API pro definice zásady](/rest/api/resources/policydefinitions).</span><span class="sxs-lookup"><span data-stu-id="4a9b3-117">You can create a policy with the [REST API for Policy Definitions](/rest/api/resources/policydefinitions).</span></span> <span data-ttu-id="4a9b3-118">Rozhraní REST API umožňuje vytvářet a odstraňovat definice zásady a získat informace o existující definice.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-118">The REST API enables you to create and delete policy definitions, and get information about existing definitions.</span></span>

<span data-ttu-id="4a9b3-119">K vytvoření definice zásady, spusťte příkaz:</span><span class="sxs-lookup"><span data-stu-id="4a9b3-119">To create a policy definition, run:</span></span>

```HTTP
PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}
```

<span data-ttu-id="4a9b3-120">Zahrnout obsah žádosti podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="4a9b3-120">Include a request body similar to the following example:</span></span>

```json
{
  "properties": {
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
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

### <a name="assign-policy"></a><span data-ttu-id="4a9b3-121">Přiřazení zásad</span><span class="sxs-lookup"><span data-stu-id="4a9b3-121">Assign policy</span></span>

<span data-ttu-id="4a9b3-122">Můžete použít definice zásady na požadovaný rozsah prostřednictvím [REST API pro přiřazení zásad](/rest/api/resources/policyassignments).</span><span class="sxs-lookup"><span data-stu-id="4a9b3-122">You can apply the policy definition at the desired scope through the [REST API for policy assignments](/rest/api/resources/policyassignments).</span></span> <span data-ttu-id="4a9b3-123">Rozhraní REST API umožňuje vytvářet a odstraňovat přiřazení zásad a získat informace o existující přiřazení.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-123">The REST API enables you to create and delete policy assignments, and get information about existing assignments.</span></span>

<span data-ttu-id="4a9b3-124">Chcete-li vytvořit přiřazení zásady, spusťte:</span><span class="sxs-lookup"><span data-stu-id="4a9b3-124">To create a policy assignment, run:</span></span>

```HTTP
PUT https://management.azure.com /subscriptions/{subscription-id}/providers/Microsoft.authorization/policyassignments/{policyAssignmentName}?api-version={api-version}
```

<span data-ttu-id="4a9b3-125">{-Přiřazení zásady} je název přiřazení zásady.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-125">The {policy-assignment} is the name of the policy assignment.</span></span>

<span data-ttu-id="4a9b3-126">Zahrnout obsah žádosti podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="4a9b3-126">Include a request body similar to the following example:</span></span>

```json
{
  "properties":{
    "displayName":"West US only policy assignment on the subscription ",
    "description":"Resources can only be provisioned in West US regions",
    "parameters": {
      "allowedLocations": { "value": ["northeurope", "westus"] }
     },
    "policyDefinitionId":"/subscriptions/{subscription-id}/providers/Microsoft.Authorization/policyDefinitions/{definition-name}",
      "scope":"/subscriptions/{subscription-id}"
  },
}
```

### <a name="view-policy"></a><span data-ttu-id="4a9b3-127">Zobrazení zásad</span><span class="sxs-lookup"><span data-stu-id="4a9b3-127">View policy</span></span>
<span data-ttu-id="4a9b3-128">Zásady, použijte [získat definice zásady](https://docs.microsoft.com/rest/api/resources/policydefinitions#PolicyDefinitions_Get) operaci.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-128">To get a policy, use the [Get policy definition](https://docs.microsoft.com/rest/api/resources/policydefinitions#PolicyDefinitions_Get) operation.</span></span>

### <a name="get-aliases"></a><span data-ttu-id="4a9b3-129">Získat aliasy</span><span class="sxs-lookup"><span data-stu-id="4a9b3-129">Get aliases</span></span>
<span data-ttu-id="4a9b3-130">Můžete načíst aliasy přes rozhraní REST API:</span><span class="sxs-lookup"><span data-stu-id="4a9b3-130">You can retrieve aliases through the REST API:</span></span>

```HTTP
GET /subscriptions/{id}/providers?$expand=resourceTypes/aliases&api-version=2015-11-01
```

<span data-ttu-id="4a9b3-131">Následující příklad ukazuje definici alias.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-131">The following example shows a definition of an alias.</span></span> <span data-ttu-id="4a9b3-132">Jak vidíte, definuje alias cesty v různých verzích rozhraní API, i když dojde ke změně názvu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-132">As you can see, an alias defines paths in different API versions, even when there is a property name change.</span></span> 

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

## <a name="powershell"></a><span data-ttu-id="4a9b3-133">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4a9b3-133">PowerShell</span></span>

<span data-ttu-id="4a9b3-134">Než budete pokračovat s příklady prostředí PowerShell, zajistěte, aby byla [nainstalovali nejnovější verzi](/powershell/azure/install-azurerm-ps) prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-134">Before proceeding with the PowerShell examples, make sure you have [installed the latest version](/powershell/azure/install-azurerm-ps) of Azure PowerShell.</span></span> <span data-ttu-id="4a9b3-135">Ve verzi 3.6.0 byly přidány zásady parametry.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-135">Policy parameters were added in version 3.6.0.</span></span> <span data-ttu-id="4a9b3-136">Pokud máte starší verzi, příklady vrátí chybu oznamující, že parametr nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-136">If you have an earlier version, the examples return an error indicating the parameter cannot be found.</span></span>

### <a name="view-policy-definitions"></a><span data-ttu-id="4a9b3-137">Definice zásad zobrazení</span><span class="sxs-lookup"><span data-stu-id="4a9b3-137">View policy definitions</span></span>
<span data-ttu-id="4a9b3-138">Pokud chcete zobrazit všechny definice zásady v rámci vašeho předplatného, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4a9b3-138">To see all policy definitions in your subscription, use the following command:</span></span>

```powershell
Get-AzureRmPolicyDefinition
```

<span data-ttu-id="4a9b3-139">Vrátí všechny dostupné zásady definice, včetně integrovaných zásad.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-139">It returns all available policy definitions, including built-in policies.</span></span> <span data-ttu-id="4a9b3-140">Každá zásada se vrátí v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="4a9b3-140">Each policy is returned in the following format:</span></span>

```powershell
Name               : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceId         : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceName       : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceType       : Microsoft.Authorization/policyDefinitions
Properties         : @{displayName=Allowed locations; policyType=BuiltIn; description=This policy enables you to
                     restrict the locations your organization can specify when deploying resources. Use to enforce
                     your geo-compliance requirements.; parameters=; policyRule=}
PolicyDefinitionId : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
```

<span data-ttu-id="4a9b3-141">Než budete pokračovat k vytvoření definice zásady, podívejte se na integrovaných zásad.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-141">Before proceeding to create a policy definition, look at the built-in policies.</span></span> <span data-ttu-id="4a9b3-142">Pokud zjistíte předdefinované zásady, které platí omezení, které potřebujete, můžete přeskočit vytvoření definice zásady.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-142">If you find a built-in policy that applies the limits you need, you can skip creating a policy definition.</span></span> <span data-ttu-id="4a9b3-143">Předdefinované zásady místo toho přiřadíte požadovaný rozsah.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-143">Instead, assign the built-in policy to the desired scope.</span></span>

### <a name="create-policy-definition"></a><span data-ttu-id="4a9b3-144">Vytvoření definice zásady</span><span class="sxs-lookup"><span data-stu-id="4a9b3-144">Create policy definition</span></span>
<span data-ttu-id="4a9b3-145">Můžete vytvořit pomocí definice zásady `New-AzureRmPolicyDefinition` rutiny.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-145">You can create a policy definition using the `New-AzureRmPolicyDefinition` cmdlet.</span></span>

```powershell
$definition = New-AzureRmPolicyDefinition -Name coolAccessTier -Description "Policy to specify access tier." -Policy '{
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

<span data-ttu-id="4a9b3-146">Výstup je uložen v `$definition` objekt, který se používá při přiřazování zásady.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-146">The output is stored in a `$definition` object, which is used during policy assignment.</span></span> 

<span data-ttu-id="4a9b3-147">Místo zadání JSON jako parametr, můžete zadat cestu k souboru .json, který obsahuje pravidlo zásad.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-147">Rather than specifying the JSON as a parameter, you can provide the path to a .json file containing the policy rule.</span></span>

```powershell
$definition = New-AzureRmPolicyDefinition -Name coolAccessTier -Description "Policy to specify access tier." -Policy "c:\policies\coolAccessTier.json"
```

<span data-ttu-id="4a9b3-148">Následující příklad vytvoří definici zásady, který obsahuje parametry:</span><span class="sxs-lookup"><span data-stu-id="4a9b3-148">The following example creates a policy definition that includes parameters:</span></span>

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
          "description": "The list of locations that can be specified when deploying storage accounts.",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
    }
}' 

$definition = New-AzureRmPolicyDefinition -Name storageLocations -Description "Policy to specify locations for storage accounts." -Policy $policy -Parameter $parameters 
```

### <a name="assign-policy"></a><span data-ttu-id="4a9b3-149">Přiřazení zásad</span><span class="sxs-lookup"><span data-stu-id="4a9b3-149">Assign policy</span></span>

<span data-ttu-id="4a9b3-150">Použít v souladu se zásadami požadovaný rozsah pomocí `New-AzureRmPolicyAssignment` rutiny.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-150">You apply the policy at the desired scope by using the `New-AzureRmPolicyAssignment` cmdlet.</span></span> <span data-ttu-id="4a9b3-151">Následující příklad přiřadí zásady do skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-151">The following example assigns the policy to a resource group.</span></span>

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
New-AzureRMPolicyAssignment -Name accessTierAssignment -Scope $rg.ResourceId -PolicyDefinition $definition
```

<span data-ttu-id="4a9b3-152">Přiřazení zásad, která vyžaduje parametry, vytvořte a objekt s těmito hodnotami.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-152">To assign a policy that requires parameters, create and object with those values.</span></span> <span data-ttu-id="4a9b3-153">Následující příklad načte předdefinovaných zásad a předá hodnoty parametrů:</span><span class="sxs-lookup"><span data-stu-id="4a9b3-153">The following example retrieves a built-in policy and passes in parameters values:</span></span>

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/e5662a6-4747-49cd-b67b-bf8b01975c4c
$array = @("West US", "West US 2")
$param = @{"listOfAllowedLocations"=$array}
New-AzureRMPolicyAssignment -Name locationAssignment -Scope $rg.ResourceId -PolicyDefinition $definition -PolicyParameterObject $param
```

### <a name="view-policy-assignment"></a><span data-ttu-id="4a9b3-154">Přiřazení zásady zobrazení</span><span class="sxs-lookup"><span data-stu-id="4a9b3-154">View policy assignment</span></span>

<span data-ttu-id="4a9b3-155">Přiřazení konkrétní zásady, použijte:</span><span class="sxs-lookup"><span data-stu-id="4a9b3-155">To get a specific policy assignment, use:</span></span>

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
(Get-AzureRmPolicyAssignment -Name accessTierAssignment -Scope $rg.ResourceId
```

<span data-ttu-id="4a9b3-156">Chcete-li zobrazit pravidlo zásad pro definici zásady, použijte:</span><span class="sxs-lookup"><span data-stu-id="4a9b3-156">To view the policy rule for a policy definition, use:</span></span>

```powershell
(Get-AzureRmPolicyDefinition -Name coolAccessTier).Properties.policyRule | ConvertTo-Json
```

### <a name="remove-policy-assignment"></a><span data-ttu-id="4a9b3-157">Odebrat přiřazení zásady</span><span class="sxs-lookup"><span data-stu-id="4a9b3-157">Remove policy assignment</span></span> 

<span data-ttu-id="4a9b3-158">Chcete-li odebrat přiřazení zásady, použijte:</span><span class="sxs-lookup"><span data-stu-id="4a9b3-158">To remove a policy assignment, use:</span></span>

```powershell
Remove-AzureRmPolicyAssignment -Name regionPolicyAssignment -Scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="azure-cli"></a><span data-ttu-id="4a9b3-159">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="4a9b3-159">Azure CLI</span></span>

### <a name="view-policy-definitions"></a><span data-ttu-id="4a9b3-160">Definice zásad zobrazení</span><span class="sxs-lookup"><span data-stu-id="4a9b3-160">View policy definitions</span></span>
<span data-ttu-id="4a9b3-161">Pokud chcete zobrazit všechny definice zásady v rámci vašeho předplatného, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4a9b3-161">To see all policy definitions in your subscription, use the following command:</span></span>

```azurecli
az policy definition list
```

<span data-ttu-id="4a9b3-162">Vrátí všechny dostupné zásady definice, včetně integrovaných zásad.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-162">It returns all available policy definitions, including built-in policies.</span></span> <span data-ttu-id="4a9b3-163">Každá zásada se vrátí v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="4a9b3-163">Each policy is returned in the following format:</span></span>

```azurecli
{                                                            
  "description": "This policy enables you to restrict the locations your organization can specify when deploying resources. Use to enforce your geo-compliance requirements.",                      
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

<span data-ttu-id="4a9b3-164">Než budete pokračovat k vytvoření definice zásady, podívejte se na integrovaných zásad.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-164">Before proceeding to create a policy definition, look at the built-in policies.</span></span> <span data-ttu-id="4a9b3-165">Pokud zjistíte předdefinované zásady, které platí omezení, které potřebujete, můžete přeskočit vytvoření definice zásady.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-165">If you find a built-in policy that applies the limits you need, you can skip creating a policy definition.</span></span> <span data-ttu-id="4a9b3-166">Předdefinované zásady místo toho přiřadíte požadovaný rozsah.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-166">Instead, assign the built-in policy to the desired scope.</span></span>

### <a name="create-policy-definition"></a><span data-ttu-id="4a9b3-167">Vytvoření definice zásady</span><span class="sxs-lookup"><span data-stu-id="4a9b3-167">Create policy definition</span></span>

<span data-ttu-id="4a9b3-168">Můžete vytvořit definici zásady pomocí rozhraní příkazového řádku Azure pomocí příkazu definice zásady.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-168">You can create a policy definition using Azure CLI with the policy definition command.</span></span>

```azurecli
az policy definition create --name coolAccessTier --description "Policy to specify access tier." --rules '{
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

### <a name="assign-policy"></a><span data-ttu-id="4a9b3-169">Přiřazení zásad</span><span class="sxs-lookup"><span data-stu-id="4a9b3-169">Assign policy</span></span>

<span data-ttu-id="4a9b3-170">Zásady můžete použít k požadovaný rozsah pomocí příkazu přiřazení zásad.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-170">You can apply the policy to the desired scope by using the policy assignment command.</span></span> <span data-ttu-id="4a9b3-171">Následující příklad přiřadí zásadu do skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="4a9b3-171">The following example assigns a policy to a resource group.</span></span>

```azurecli
az policy assignment create --name coolAccessTierAssignment --policy coolAccessTier --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

### <a name="view-policy-assignment"></a><span data-ttu-id="4a9b3-172">Přiřazení zásady zobrazení</span><span class="sxs-lookup"><span data-stu-id="4a9b3-172">View policy assignment</span></span>

<span data-ttu-id="4a9b3-173">Chcete-li zobrazit přiřazení zásady, zadejte název přiřazení zásady a oboru:</span><span class="sxs-lookup"><span data-stu-id="4a9b3-173">To view a policy assignment, provide the policy assignment name and the scope:</span></span>

```azurecli
az policy assignment show --name coolAccessTierAssignment --scope "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}"
```

### <a name="remove-policy-assignment"></a><span data-ttu-id="4a9b3-174">Odebrat přiřazení zásady</span><span class="sxs-lookup"><span data-stu-id="4a9b3-174">Remove policy assignment</span></span> 

<span data-ttu-id="4a9b3-175">Chcete-li odebrat přiřazení zásady, použijte:</span><span class="sxs-lookup"><span data-stu-id="4a9b3-175">To remove a policy assignment, use:</span></span>

```azurecli
az policy assignment delete --name coolAccessTier --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="next-steps"></a><span data-ttu-id="4a9b3-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4a9b3-176">Next steps</span></span>
* <span data-ttu-id="4a9b3-177">Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="4a9b3-177">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

