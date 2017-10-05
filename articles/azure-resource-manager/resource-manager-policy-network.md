---
title: "Zásady prostředků Azure pro síťové prostředky | Microsoft Docs"
description: "Popisuje zásady Azure Resource Manageru pro správu nasazení síťových prostředků."
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
ms.openlocfilehash: bca66bbdd9da9b3e4099d0d961f42c9368a17f5e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="apply-resource-policies-to-network-resources"></a><span data-ttu-id="200a7-103">Použít zásady prostředků k síťovým prostředkům</span><span class="sxs-lookup"><span data-stu-id="200a7-103">Apply resource policies to network resources</span></span>
<span data-ttu-id="200a7-104">Tento článek ukazuje příklad [zásada prostředků](resource-manager-policy.md) můžete použít pro brány virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="200a7-104">This article shows an example [resource policy](resource-manager-policy.md) you can apply to Azure virtual network gateways.</span></span> <span data-ttu-id="200a7-105">Tato zásada zajistí konzistence pro brány nasadit ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="200a7-105">This policy ensures consistency for gateways deployed in your organization.</span></span> 

## <a name="define-permitted-virtual-network-gateway-sku"></a><span data-ttu-id="200a7-106">Definování skladová položka brány virtuální sítě povolené</span><span class="sxs-lookup"><span data-stu-id="200a7-106">Define permitted virtual network gateway SKU</span></span>

<span data-ttu-id="200a7-107">Tyto zásady omezují, SKU, které mohou být nasazeny pro brány virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="200a7-107">The following policy restricts which SKUs can be deployed for virtual network gateways:</span></span>

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Network/virtualNetworkGateways"
      },
      {
        "not": {
          "field": "Microsoft.Network/virtualNetworkGateways/sku.name",
          "in": [
            "Basic",
            "VpnGw1"
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

## <a name="next-steps"></a><span data-ttu-id="200a7-108">Další kroky</span><span class="sxs-lookup"><span data-stu-id="200a7-108">Next steps</span></span>
* <span data-ttu-id="200a7-109">Po definování zásad pravidlo (jak je znázorněno v předchozích ukázkách), musíte k vytvoření definice zásady a přiřadit obor.</span><span class="sxs-lookup"><span data-stu-id="200a7-109">After defining a policy rule (as shown in the preceding examples), you need to create the policy definition and assign it to a scope.</span></span> <span data-ttu-id="200a7-110">Obor může být předplatné, skupinu prostředků nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="200a7-110">The scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="200a7-111">K přiřazení zásad prostřednictvím portálu, najdete v části [portálu Azure použijte přiřadit a spravovat zásady prostředků](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="200a7-111">To assign policies through the portal, see [Use Azure portal to assign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="200a7-112">K přiřazení zásad pomocí rozhraní REST API, Powershellu nebo příkazového řádku Azure CLI, najdete v části [přiřadit a spravovat zásady prostřednictvím skriptu](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="200a7-112">To assign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span> 
* <span data-ttu-id="200a7-113">Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="200a7-113">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

