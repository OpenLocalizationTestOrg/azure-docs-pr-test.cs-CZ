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
# <a name="apply-resource-policies-toostorage-accounts"></a>Použít účty toostorage zásady prostředků
Toto téma ukazuje několik [zásady prostředků](resource-manager-policy.md) můžete použít účty úložiště tooAzure. Tyto zásady zajistit konzistenci pro účty úložiště hello nasadit ve vaší organizaci. 

## <a name="define-permitted-storage-account-types"></a>Definování typů účet povolených úložiště

Hello tyto zásady omezují který [typy účtů úložiště](../storage/common/storage-redundancy.md) lze nasadit:

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

Pravidlo zásad podobně jako parametr pro přijetí hello povolená SKU je k dispozici jako definice předdefinovaných zásad. předdefinované zásady Hello má ID prostředku hello `/providers/Microsoft.Authorization/policyDefinitions/7433c107-6db4-4ad1-b57a-a76dce0154a1`. 

## <a name="define-permitted-access-tier"></a>Definovat úroveň povolených přístupu

Hello tyto zásady určuje typ hello [úroveň přístupu](../storage/blobs/storage-blob-storage-tiers.md) , lze zadat pro účty úložiště:

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

## <a name="ensure-encryption-is-enabled"></a>Ujistěte se, že je povolené šifrování

Hello tyto zásady vyžaduje všechny tooenable účty úložiště [šifrování služby úložiště](../storage/common/storage-service-encryption.md):

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

Toto pravidlo zásad je také k dispozici jako definici předdefinované zásady s ID prostředku hello `/providers/Microsoft.Authorization/policyDefinitions/7c5a74bf-ae94-4a74-8fcf-644d1e0e6e6f`.

## <a name="next-steps"></a>Další kroky
* Po definování zásad pravidlo (jak je znázorněno v předchozích příkladech hello), budete potřebovat definice zásady hello toocreate a přiřaďte ho tooa oboru. obor Hello může být předplatné, skupinu prostředků nebo prostředek. v tématu Zásady tooassign prostřednictvím portálu hello [Azure pomocí portálu tooassign a spravovat zásady prostředků](resource-manager-policy-portal.md). zásady tooassign prostřednictvím REST API, Powershellu nebo příkazového řádku Azure CLI, najdete v části [přiřadit a spravovat zásady prostřednictvím skriptu](resource-manager-policy-create-assign.md). 
* Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).

