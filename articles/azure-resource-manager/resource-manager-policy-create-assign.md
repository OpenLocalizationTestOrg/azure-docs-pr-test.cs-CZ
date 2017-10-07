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
# <a name="assign-and-manage-resource-policies"></a><span data-ttu-id="41156-103">Přiřadit a spravovat zásady prostředků</span><span class="sxs-lookup"><span data-stu-id="41156-103">Assign and manage resource policies</span></span>

<span data-ttu-id="41156-104">tooimplement zásady, je třeba provést tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="41156-104">tooimplement a policy, you must perform these steps:</span></span>

1. <span data-ttu-id="41156-105">Pokud již existuje v rámci vašeho předplatného, která splňuje vaše požadavky, zkontrolujte toosee zásad definice (včetně integrovaných zásad poskytovaný platformou Azure).</span><span class="sxs-lookup"><span data-stu-id="41156-105">Check policy definitions (including built-in policies provided by Azure) toosee if one already exists in your subscription that fulfills your requirements.</span></span>
2. <span data-ttu-id="41156-106">Pokud existuje, získáte její název.</span><span class="sxs-lookup"><span data-stu-id="41156-106">If one exists, get its name.</span></span>
3. <span data-ttu-id="41156-107">Pokud žádný neexistuje, definovat pravidlo zásad hello textem JSON a přidejte jej jako definici zásady v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="41156-107">If one does not exist, define hello policy rule with JSON, and add it as a policy definition in your subscription.</span></span> <span data-ttu-id="41156-108">Tento krok zpřístupní hello zásady pro přiřazení, ale nevztahuje hello pravidla tooyour předplatné.</span><span class="sxs-lookup"><span data-stu-id="41156-108">This step makes hello policy available for assignment but does not apply hello rules tooyour subscription.</span></span>
4. <span data-ttu-id="41156-109">V obou případech přiřadíte obor tooa zásady hello (například předplatné nebo prostředek skupiny).</span><span class="sxs-lookup"><span data-stu-id="41156-109">For either case, assign hello policy tooa scope (such as a subscription or resource group).</span></span> <span data-ttu-id="41156-110">nevynutí se teď Hello pravidla zásad hello.</span><span class="sxs-lookup"><span data-stu-id="41156-110">hello rules of hello policy are now enforced.</span></span>

<span data-ttu-id="41156-111">Tento článek se zaměřuje na hello kroky toocreate definici zásady a přiřazení této definici oboru tooa prostřednictvím REST API, Powershellu nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="41156-111">This article focuses on hello steps toocreate a policy definition and assign that definition tooa scope through REST API, PowerShell, or Azure CLI.</span></span> <span data-ttu-id="41156-112">Pokud dáváte přednost toouse hello portálu tooassign zásady, najdete v části [Azure pomocí portálu tooassign a spravovat zásady prostředků](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="41156-112">If you prefer toouse hello portal tooassign policies, see [Use Azure portal tooassign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="41156-113">V tomto článku není soustředit na hello syntaxe pro vytvoření definice zásady hello.</span><span class="sxs-lookup"><span data-stu-id="41156-113">This article does not focus on hello syntax for creating hello policy definition.</span></span> <span data-ttu-id="41156-114">Informace o syntaxi zásad najdete v tématu [přehled zásad prostředků](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="41156-114">For information about policy syntax, see [Resource policy overview](resource-manager-policy.md).</span></span>

## <a name="rest-api"></a><span data-ttu-id="41156-115">REST API</span><span class="sxs-lookup"><span data-stu-id="41156-115">REST API</span></span>

### <a name="create-policy-definition"></a><span data-ttu-id="41156-116">Vytvoření definice zásady</span><span class="sxs-lookup"><span data-stu-id="41156-116">Create policy definition</span></span>

<span data-ttu-id="41156-117">Zásady můžete vytvořit pomocí hello [REST API pro definice zásady](/rest/api/resources/policydefinitions).</span><span class="sxs-lookup"><span data-stu-id="41156-117">You can create a policy with hello [REST API for Policy Definitions](/rest/api/resources/policydefinitions).</span></span> <span data-ttu-id="41156-118">Hello REST API vám umožňuje toocreate a odstraňte definice zásady a získat informace o existující definice.</span><span class="sxs-lookup"><span data-stu-id="41156-118">hello REST API enables you toocreate and delete policy definitions, and get information about existing definitions.</span></span>

<span data-ttu-id="41156-119">Spusťte toocreate definici zásady:</span><span class="sxs-lookup"><span data-stu-id="41156-119">toocreate a policy definition, run:</span></span>

```HTTP
PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}
```

<span data-ttu-id="41156-120">Zahrnout podobné toohello v textu požadavku následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="41156-120">Include a request body similar toohello following example:</span></span>

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

### <a name="assign-policy"></a><span data-ttu-id="41156-121">Přiřazení zásad</span><span class="sxs-lookup"><span data-stu-id="41156-121">Assign policy</span></span>

<span data-ttu-id="41156-122">Můžete použít definice zásady hello v oboru hello potřeby prostřednictvím hello [REST API pro přiřazení zásad](/rest/api/resources/policyassignments).</span><span class="sxs-lookup"><span data-stu-id="41156-122">You can apply hello policy definition at hello desired scope through hello [REST API for policy assignments](/rest/api/resources/policyassignments).</span></span> <span data-ttu-id="41156-123">Hello REST API vám umožňuje toocreate odstranit přiřazení zásad a získat informace o existující přiřazení.</span><span class="sxs-lookup"><span data-stu-id="41156-123">hello REST API enables you toocreate and delete policy assignments, and get information about existing assignments.</span></span>

<span data-ttu-id="41156-124">toocreate přiřazení zásady, spusťte:</span><span class="sxs-lookup"><span data-stu-id="41156-124">toocreate a policy assignment, run:</span></span>

```HTTP
PUT https://management.azure.com /subscriptions/{subscription-id}/providers/Microsoft.authorization/policyassignments/{policyAssignmentName}?api-version={api-version}
```

<span data-ttu-id="41156-125">Hello {přiřazení zásady} je hello název přiřazení zásady hello.</span><span class="sxs-lookup"><span data-stu-id="41156-125">hello {policy-assignment} is hello name of hello policy assignment.</span></span>

<span data-ttu-id="41156-126">Zahrnout podobné toohello v textu požadavku následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="41156-126">Include a request body similar toohello following example:</span></span>

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

### <a name="view-policy"></a><span data-ttu-id="41156-127">Zobrazení zásad</span><span class="sxs-lookup"><span data-stu-id="41156-127">View policy</span></span>
<span data-ttu-id="41156-128">tooget zásady a použít hello [získat definice zásady](https://docs.microsoft.com/rest/api/resources/policydefinitions#PolicyDefinitions_Get) operaci.</span><span class="sxs-lookup"><span data-stu-id="41156-128">tooget a policy, use hello [Get policy definition](https://docs.microsoft.com/rest/api/resources/policydefinitions#PolicyDefinitions_Get) operation.</span></span>

### <a name="get-aliases"></a><span data-ttu-id="41156-129">Získat aliasy</span><span class="sxs-lookup"><span data-stu-id="41156-129">Get aliases</span></span>
<span data-ttu-id="41156-130">Můžete načíst aliasy prostřednictvím hello REST API:</span><span class="sxs-lookup"><span data-stu-id="41156-130">You can retrieve aliases through hello REST API:</span></span>

```HTTP
GET /subscriptions/{id}/providers?$expand=resourceTypes/aliases&api-version=2015-11-01
```

<span data-ttu-id="41156-131">Hello následující příklad ukazuje definici alias.</span><span class="sxs-lookup"><span data-stu-id="41156-131">hello following example shows a definition of an alias.</span></span> <span data-ttu-id="41156-132">Jak vidíte, definuje alias cesty v různých verzích rozhraní API, i když dojde ke změně názvu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="41156-132">As you can see, an alias defines paths in different API versions, even when there is a property name change.</span></span> 

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

## <a name="powershell"></a><span data-ttu-id="41156-133">PowerShell</span><span class="sxs-lookup"><span data-stu-id="41156-133">PowerShell</span></span>

<span data-ttu-id="41156-134">Než budete pokračovat s příklady prostředí PowerShell text hello, zajistěte, aby byla [nainstalovaná nejnovější verze hello](/powershell/azure/install-azurerm-ps) prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="41156-134">Before proceeding with hello PowerShell examples, make sure you have [installed hello latest version](/powershell/azure/install-azurerm-ps) of Azure PowerShell.</span></span> <span data-ttu-id="41156-135">Ve verzi 3.6.0 byly přidány zásady parametry.</span><span class="sxs-lookup"><span data-stu-id="41156-135">Policy parameters were added in version 3.6.0.</span></span> <span data-ttu-id="41156-136">Máte starší verzi, příklady hello vrátit že chyba naznačuje hello parametru nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="41156-136">If you have an earlier version, hello examples return an error indicating hello parameter cannot be found.</span></span>

### <a name="view-policy-definitions"></a><span data-ttu-id="41156-137">Definice zásad zobrazení</span><span class="sxs-lookup"><span data-stu-id="41156-137">View policy definitions</span></span>
<span data-ttu-id="41156-138">toosee všechny definice zásady v rámci vašeho předplatného, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="41156-138">toosee all policy definitions in your subscription, use hello following command:</span></span>

```powershell
Get-AzureRmPolicyDefinition
```

<span data-ttu-id="41156-139">Vrátí všechny dostupné zásady definice, včetně integrovaných zásad.</span><span class="sxs-lookup"><span data-stu-id="41156-139">It returns all available policy definitions, including built-in policies.</span></span> <span data-ttu-id="41156-140">Každá zásada se vrátí v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="41156-140">Each policy is returned in hello following format:</span></span>

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

<span data-ttu-id="41156-141">Před pokračováním toocreate definici zásady podívejte se na integrovaných zásad hello.</span><span class="sxs-lookup"><span data-stu-id="41156-141">Before proceeding toocreate a policy definition, look at hello built-in policies.</span></span> <span data-ttu-id="41156-142">Pokud zjistíte předdefinované zásady, které platí hello omezení, které potřebujete, můžete přeskočit vytvoření definice zásady.</span><span class="sxs-lookup"><span data-stu-id="41156-142">If you find a built-in policy that applies hello limits you need, you can skip creating a policy definition.</span></span> <span data-ttu-id="41156-143">Místo toho přiřadíte obor toohello potřeby předdefinované zásady hello.</span><span class="sxs-lookup"><span data-stu-id="41156-143">Instead, assign hello built-in policy toohello desired scope.</span></span>

### <a name="create-policy-definition"></a><span data-ttu-id="41156-144">Vytvoření definice zásady</span><span class="sxs-lookup"><span data-stu-id="41156-144">Create policy definition</span></span>
<span data-ttu-id="41156-145">Můžete vytvořit definici zásady pomocí hello `New-AzureRmPolicyDefinition` rutiny.</span><span class="sxs-lookup"><span data-stu-id="41156-145">You can create a policy definition using hello `New-AzureRmPolicyDefinition` cmdlet.</span></span>

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

<span data-ttu-id="41156-146">výstup Hello je uložen v `$definition` objekt, který se používá při přiřazování zásady.</span><span class="sxs-lookup"><span data-stu-id="41156-146">hello output is stored in a `$definition` object, which is used during policy assignment.</span></span> 

<span data-ttu-id="41156-147">Místo zadání hello JSON jako parametr, můžete zadat hello cesta tooa .json souboru, který obsahuje pravidlo zásad hello.</span><span class="sxs-lookup"><span data-stu-id="41156-147">Rather than specifying hello JSON as a parameter, you can provide hello path tooa .json file containing hello policy rule.</span></span>

```powershell
$definition = New-AzureRmPolicyDefinition -Name coolAccessTier -Description "Policy toospecify access tier." -Policy "c:\policies\coolAccessTier.json"
```

<span data-ttu-id="41156-148">Hello následující příklad vytvoří definici zásady, který obsahuje parametry:</span><span class="sxs-lookup"><span data-stu-id="41156-148">hello following example creates a policy definition that includes parameters:</span></span>

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

### <a name="assign-policy"></a><span data-ttu-id="41156-149">Přiřazení zásad</span><span class="sxs-lookup"><span data-stu-id="41156-149">Assign policy</span></span>

<span data-ttu-id="41156-150">Použít zásady hello v oboru hello potřeby pomocí hello `New-AzureRmPolicyAssignment` rutiny.</span><span class="sxs-lookup"><span data-stu-id="41156-150">You apply hello policy at hello desired scope by using hello `New-AzureRmPolicyAssignment` cmdlet.</span></span> <span data-ttu-id="41156-151">Následující ukázka Hello přiřadí skupinu prostředků tooa hello zásad.</span><span class="sxs-lookup"><span data-stu-id="41156-151">hello following example assigns hello policy tooa resource group.</span></span>

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
New-AzureRMPolicyAssignment -Name accessTierAssignment -Scope $rg.ResourceId -PolicyDefinition $definition
```

<span data-ttu-id="41156-152">tooassign zásadu, která vyžaduje parametry, vytvořte a objekt s těmito hodnotami.</span><span class="sxs-lookup"><span data-stu-id="41156-152">tooassign a policy that requires parameters, create and object with those values.</span></span> <span data-ttu-id="41156-153">Hello následující příklad načte předdefinovaných zásad a předá hodnoty parametrů:</span><span class="sxs-lookup"><span data-stu-id="41156-153">hello following example retrieves a built-in policy and passes in parameters values:</span></span>

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/e5662a6-4747-49cd-b67b-bf8b01975c4c
$array = @("West US", "West US 2")
$param = @{"listOfAllowedLocations"=$array}
New-AzureRMPolicyAssignment -Name locationAssignment -Scope $rg.ResourceId -PolicyDefinition $definition -PolicyParameterObject $param
```

### <a name="view-policy-assignment"></a><span data-ttu-id="41156-154">Přiřazení zásady zobrazení</span><span class="sxs-lookup"><span data-stu-id="41156-154">View policy assignment</span></span>

<span data-ttu-id="41156-155">tooget přiřazení konkrétní zásady, použijte:</span><span class="sxs-lookup"><span data-stu-id="41156-155">tooget a specific policy assignment, use:</span></span>

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
(Get-AzureRmPolicyAssignment -Name accessTierAssignment -Scope $rg.ResourceId
```

<span data-ttu-id="41156-156">pravidlo zásad tooview hello definice zásady, použijte:</span><span class="sxs-lookup"><span data-stu-id="41156-156">tooview hello policy rule for a policy definition, use:</span></span>

```powershell
(Get-AzureRmPolicyDefinition -Name coolAccessTier).Properties.policyRule | ConvertTo-Json
```

### <a name="remove-policy-assignment"></a><span data-ttu-id="41156-157">Odebrat přiřazení zásady</span><span class="sxs-lookup"><span data-stu-id="41156-157">Remove policy assignment</span></span> 

<span data-ttu-id="41156-158">tooremove přiřazení zásady, použijte:</span><span class="sxs-lookup"><span data-stu-id="41156-158">tooremove a policy assignment, use:</span></span>

```powershell
Remove-AzureRmPolicyAssignment -Name regionPolicyAssignment -Scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="azure-cli"></a><span data-ttu-id="41156-159">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="41156-159">Azure CLI</span></span>

### <a name="view-policy-definitions"></a><span data-ttu-id="41156-160">Definice zásad zobrazení</span><span class="sxs-lookup"><span data-stu-id="41156-160">View policy definitions</span></span>
<span data-ttu-id="41156-161">toosee všechny definice zásady v rámci vašeho předplatného, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="41156-161">toosee all policy definitions in your subscription, use hello following command:</span></span>

```azurecli
az policy definition list
```

<span data-ttu-id="41156-162">Vrátí všechny dostupné zásady definice, včetně integrovaných zásad.</span><span class="sxs-lookup"><span data-stu-id="41156-162">It returns all available policy definitions, including built-in policies.</span></span> <span data-ttu-id="41156-163">Každá zásada se vrátí v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="41156-163">Each policy is returned in hello following format:</span></span>

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

<span data-ttu-id="41156-164">Před pokračováním toocreate definici zásady podívejte se na integrovaných zásad hello.</span><span class="sxs-lookup"><span data-stu-id="41156-164">Before proceeding toocreate a policy definition, look at hello built-in policies.</span></span> <span data-ttu-id="41156-165">Pokud zjistíte předdefinované zásady, které platí hello omezení, které potřebujete, můžete přeskočit vytvoření definice zásady.</span><span class="sxs-lookup"><span data-stu-id="41156-165">If you find a built-in policy that applies hello limits you need, you can skip creating a policy definition.</span></span> <span data-ttu-id="41156-166">Místo toho přiřadíte obor toohello potřeby předdefinované zásady hello.</span><span class="sxs-lookup"><span data-stu-id="41156-166">Instead, assign hello built-in policy toohello desired scope.</span></span>

### <a name="create-policy-definition"></a><span data-ttu-id="41156-167">Vytvoření definice zásady</span><span class="sxs-lookup"><span data-stu-id="41156-167">Create policy definition</span></span>

<span data-ttu-id="41156-168">Můžete vytvořit definici zásady pomocí rozhraní příkazového řádku Azure pomocí příkazu definice zásady hello.</span><span class="sxs-lookup"><span data-stu-id="41156-168">You can create a policy definition using Azure CLI with hello policy definition command.</span></span>

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

### <a name="assign-policy"></a><span data-ttu-id="41156-169">Přiřazení zásad</span><span class="sxs-lookup"><span data-stu-id="41156-169">Assign policy</span></span>

<span data-ttu-id="41156-170">Můžete použít obor toohello potřeby zásady hello pomocí příkazu přiřazení zásad hello.</span><span class="sxs-lookup"><span data-stu-id="41156-170">You can apply hello policy toohello desired scope by using hello policy assignment command.</span></span> <span data-ttu-id="41156-171">Následující ukázka Hello přiřadí skupinu prostředků tooa zásad.</span><span class="sxs-lookup"><span data-stu-id="41156-171">hello following example assigns a policy tooa resource group.</span></span>

```azurecli
az policy assignment create --name coolAccessTierAssignment --policy coolAccessTier --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

### <a name="view-policy-assignment"></a><span data-ttu-id="41156-172">Přiřazení zásady zobrazení</span><span class="sxs-lookup"><span data-stu-id="41156-172">View policy assignment</span></span>

<span data-ttu-id="41156-173">tooview přiřazení zásady, zadejte název přiřazení zásady hello a oboru hello:</span><span class="sxs-lookup"><span data-stu-id="41156-173">tooview a policy assignment, provide hello policy assignment name and hello scope:</span></span>

```azurecli
az policy assignment show --name coolAccessTierAssignment --scope "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}"
```

### <a name="remove-policy-assignment"></a><span data-ttu-id="41156-174">Odebrat přiřazení zásady</span><span class="sxs-lookup"><span data-stu-id="41156-174">Remove policy assignment</span></span> 

<span data-ttu-id="41156-175">tooremove přiřazení zásady, použijte:</span><span class="sxs-lookup"><span data-stu-id="41156-175">tooremove a policy assignment, use:</span></span>

```azurecli
az policy assignment delete --name coolAccessTier --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="next-steps"></a><span data-ttu-id="41156-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="41156-176">Next steps</span></span>
* <span data-ttu-id="41156-177">Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="41156-177">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

