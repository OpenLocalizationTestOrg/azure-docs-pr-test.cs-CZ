---
title: "Zásady prostředků Azure pro účty úložiště | Microsoft Docs"
description: "Popisuje zásady Azure Resource Manageru pro správu nasazení účty úložiště."
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
ms.date: 07/05/2017
ms.author: tomfitz
ms.openlocfilehash: 6612ee61f5c50e743241b92030660cea7ae7094d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="apply-resource-policies-to-storage-accounts"></a><span data-ttu-id="6dbb9-103">Účty úložiště použít zásady prostředků</span><span class="sxs-lookup"><span data-stu-id="6dbb9-103">Apply resource policies to storage accounts</span></span>
<span data-ttu-id="6dbb9-104">Toto téma ukazuje několik [zásady prostředků](resource-manager-policy.md) můžete použít pro účty úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="6dbb9-104">This topic shows several [resource policies](resource-manager-policy.md) you can apply to Azure storage accounts.</span></span> <span data-ttu-id="6dbb9-105">Tyto zásady zajistit konzistenci pro účty úložiště, který je nasazen ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="6dbb9-105">These policies ensure consistency for the storage accounts deployed in your organization.</span></span> 

## <a name="define-permitted-storage-account-types"></a><span data-ttu-id="6dbb9-106">Definování typů účet povolených úložiště</span><span class="sxs-lookup"><span data-stu-id="6dbb9-106">Define permitted storage account types</span></span>

<span data-ttu-id="6dbb9-107">Tyto zásady omezují, který [typy účtů úložiště](../storage/common/storage-redundancy.md) lze nasadit:</span><span class="sxs-lookup"><span data-stu-id="6dbb9-107">The following policy restricts which [storage account types](../storage/common/storage-redundancy.md) can be deployed:</span></span>

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/sku.name",
          "in": [
            "Standard_LRS",
            "Standard_GRS"
          ]
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

<span data-ttu-id="6dbb9-108">Pravidlo zásad podobně jako parametr pro přijetí povolená SKU je k dispozici jako definice předdefinovaných zásad.</span><span class="sxs-lookup"><span data-stu-id="6dbb9-108">A similar policy rule with a parameter for accepting the allowed SKUs is available as a built-in policy definition.</span></span> <span data-ttu-id="6dbb9-109">Předdefinované zásady má ID prostředku `/providers/Microsoft.Authorization/policyDefinitions/7433c107-6db4-4ad1-b57a-a76dce0154a1`.</span><span class="sxs-lookup"><span data-stu-id="6dbb9-109">The built-in policy has the resource ID of `/providers/Microsoft.Authorization/policyDefinitions/7433c107-6db4-4ad1-b57a-a76dce0154a1`.</span></span> 

## <a name="define-permitted-access-tier"></a><span data-ttu-id="6dbb9-110">Definovat úroveň povolených přístupu</span><span class="sxs-lookup"><span data-stu-id="6dbb9-110">Define permitted access tier</span></span>

<span data-ttu-id="6dbb9-111">Tyto zásady určuje typ [úroveň přístupu](../storage/blobs/storage-blob-storage-tiers.md) , lze zadat pro účty úložiště:</span><span class="sxs-lookup"><span data-stu-id="6dbb9-111">The following policy specifies the type of [access tier](../storage/blobs/storage-blob-storage-tiers.md) that can be specified for storage accounts:</span></span>

```json
{
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
}
```

## <a name="ensure-encryption-is-enabled"></a><span data-ttu-id="6dbb9-112">Ujistěte se, že je povolené šifrování</span><span class="sxs-lookup"><span data-stu-id="6dbb9-112">Ensure encryption is enabled</span></span>

<span data-ttu-id="6dbb9-113">Tyto zásady vyžaduje všechny účty úložiště, abyste umožnili [šifrování služby úložiště](../storage/common/storage-service-encryption.md):</span><span class="sxs-lookup"><span data-stu-id="6dbb9-113">The following policy requires all storage accounts to enable [Storage service encryption](../storage/common/storage-service-encryption.md):</span></span>

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/enableBlobEncryption",
          "equals": "true"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

<span data-ttu-id="6dbb9-114">Toto pravidlo zásad je také k dispozici jako definici předdefinované zásady s ID prostředku `/providers/Microsoft.Authorization/policyDefinitions/7c5a74bf-ae94-4a74-8fcf-644d1e0e6e6f`.</span><span class="sxs-lookup"><span data-stu-id="6dbb9-114">This policy rule is also available as a built-in policy definition with the resource ID of `/providers/Microsoft.Authorization/policyDefinitions/7c5a74bf-ae94-4a74-8fcf-644d1e0e6e6f`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6dbb9-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6dbb9-115">Next steps</span></span>
* <span data-ttu-id="6dbb9-116">Po definování zásad pravidlo (jak je znázorněno v předchozích ukázkách), musíte k vytvoření definice zásady a přiřadit obor.</span><span class="sxs-lookup"><span data-stu-id="6dbb9-116">After defining a policy rule (as shown in the preceding examples), you need to create the policy definition and assign it to a scope.</span></span> <span data-ttu-id="6dbb9-117">Obor může být předplatné, skupinu prostředků nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="6dbb9-117">The scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="6dbb9-118">K přiřazení zásad prostřednictvím portálu, najdete v části [portálu Azure použijte přiřadit a spravovat zásady prostředků](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6dbb9-118">To assign policies through the portal, see [Use Azure portal to assign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="6dbb9-119">K přiřazení zásad pomocí rozhraní REST API, Powershellu nebo příkazového řádku Azure CLI, najdete v části [přiřadit a spravovat zásady prostřednictvím skriptu](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="6dbb9-119">To assign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span> 
* <span data-ttu-id="6dbb9-120">Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="6dbb9-120">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

