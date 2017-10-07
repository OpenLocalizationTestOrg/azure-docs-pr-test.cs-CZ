---
title: "zásady aaaAzure prostředků pro účty úložiště | Microsoft Docs"
description: "Popisuje zásady Azure Resource Manageru pro správu nasazení hello účtů úložiště."
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
ms.openlocfilehash: d37fc4bcf7cdec71b0e14f6231fc138bfb6a7893
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="apply-resource-policies-toostorage-accounts"></a><span data-ttu-id="c337d-103">Použít účty toostorage zásady prostředků</span><span class="sxs-lookup"><span data-stu-id="c337d-103">Apply resource policies toostorage accounts</span></span>
<span data-ttu-id="c337d-104">Toto téma ukazuje několik [zásady prostředků](resource-manager-policy.md) můžete použít účty úložiště tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c337d-104">This topic shows several [resource policies](resource-manager-policy.md) you can apply tooAzure storage accounts.</span></span> <span data-ttu-id="c337d-105">Tyto zásady zajistit konzistenci pro účty úložiště hello nasadit ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="c337d-105">These policies ensure consistency for hello storage accounts deployed in your organization.</span></span> 

## <a name="define-permitted-storage-account-types"></a><span data-ttu-id="c337d-106">Definování typů účet povolených úložiště</span><span class="sxs-lookup"><span data-stu-id="c337d-106">Define permitted storage account types</span></span>

<span data-ttu-id="c337d-107">Hello tyto zásady omezují který [typy účtů úložiště](../storage/common/storage-redundancy.md) lze nasadit:</span><span class="sxs-lookup"><span data-stu-id="c337d-107">hello following policy restricts which [storage account types](../storage/common/storage-redundancy.md) can be deployed:</span></span>

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

<span data-ttu-id="c337d-108">Pravidlo zásad podobně jako parametr pro přijetí hello povolená SKU je k dispozici jako definice předdefinovaných zásad.</span><span class="sxs-lookup"><span data-stu-id="c337d-108">A similar policy rule with a parameter for accepting hello allowed SKUs is available as a built-in policy definition.</span></span> <span data-ttu-id="c337d-109">předdefinované zásady Hello má ID prostředku hello `/providers/Microsoft.Authorization/policyDefinitions/7433c107-6db4-4ad1-b57a-a76dce0154a1`.</span><span class="sxs-lookup"><span data-stu-id="c337d-109">hello built-in policy has hello resource ID of `/providers/Microsoft.Authorization/policyDefinitions/7433c107-6db4-4ad1-b57a-a76dce0154a1`.</span></span> 

## <a name="define-permitted-access-tier"></a><span data-ttu-id="c337d-110">Definovat úroveň povolených přístupu</span><span class="sxs-lookup"><span data-stu-id="c337d-110">Define permitted access tier</span></span>

<span data-ttu-id="c337d-111">Hello tyto zásady určuje typ hello [úroveň přístupu](../storage/blobs/storage-blob-storage-tiers.md) , lze zadat pro účty úložiště:</span><span class="sxs-lookup"><span data-stu-id="c337d-111">hello following policy specifies hello type of [access tier](../storage/blobs/storage-blob-storage-tiers.md) that can be specified for storage accounts:</span></span>

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

## <a name="ensure-encryption-is-enabled"></a><span data-ttu-id="c337d-112">Ujistěte se, že je povolené šifrování</span><span class="sxs-lookup"><span data-stu-id="c337d-112">Ensure encryption is enabled</span></span>

<span data-ttu-id="c337d-113">Hello tyto zásady vyžaduje všechny tooenable účty úložiště [šifrování služby úložiště](../storage/common/storage-service-encryption.md):</span><span class="sxs-lookup"><span data-stu-id="c337d-113">hello following policy requires all storage accounts tooenable [Storage service encryption](../storage/common/storage-service-encryption.md):</span></span>

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

<span data-ttu-id="c337d-114">Toto pravidlo zásad je také k dispozici jako definici předdefinované zásady s ID prostředku hello `/providers/Microsoft.Authorization/policyDefinitions/7c5a74bf-ae94-4a74-8fcf-644d1e0e6e6f`.</span><span class="sxs-lookup"><span data-stu-id="c337d-114">This policy rule is also available as a built-in policy definition with hello resource ID of `/providers/Microsoft.Authorization/policyDefinitions/7c5a74bf-ae94-4a74-8fcf-644d1e0e6e6f`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c337d-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c337d-115">Next steps</span></span>
* <span data-ttu-id="c337d-116">Po definování zásad pravidlo (jak je znázorněno v předchozích příkladech hello), budete potřebovat definice zásady hello toocreate a přiřaďte ho tooa oboru.</span><span class="sxs-lookup"><span data-stu-id="c337d-116">After defining a policy rule (as shown in hello preceding examples), you need toocreate hello policy definition and assign it tooa scope.</span></span> <span data-ttu-id="c337d-117">obor Hello může být předplatné, skupinu prostředků nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="c337d-117">hello scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="c337d-118">v tématu Zásady tooassign prostřednictvím portálu hello [Azure pomocí portálu tooassign a spravovat zásady prostředků](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c337d-118">tooassign policies through hello portal, see [Use Azure portal tooassign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="c337d-119">zásady tooassign prostřednictvím REST API, Powershellu nebo příkazového řádku Azure CLI, najdete v části [přiřadit a spravovat zásady prostřednictvím skriptu](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="c337d-119">tooassign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span> 
* <span data-ttu-id="c337d-120">Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="c337d-120">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

