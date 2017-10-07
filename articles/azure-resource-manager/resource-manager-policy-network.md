---
title: "zásady aaaAzure prostředků pro síťové prostředky | Microsoft Docs"
description: "Popisuje zásady Azure Resource Manageru pro správu nasazení hello síťových prostředků."
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
ms.openlocfilehash: a6072c1c30db0a4e4a1cae04efc7828d14069709
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="apply-resource-policies-toonetwork-resources"></a><span data-ttu-id="a053e-103">Použití prostředků zásady toonetwork prostředky</span><span class="sxs-lookup"><span data-stu-id="a053e-103">Apply resource policies toonetwork resources</span></span>
<span data-ttu-id="a053e-104">Tento článek ukazuje příklad [zásada prostředků](resource-manager-policy.md) tooAzure brány virtuální sítě můžete použít.</span><span class="sxs-lookup"><span data-stu-id="a053e-104">This article shows an example [resource policy](resource-manager-policy.md) you can apply tooAzure virtual network gateways.</span></span> <span data-ttu-id="a053e-105">Tato zásada zajistí konzistence pro brány nasadit ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="a053e-105">This policy ensures consistency for gateways deployed in your organization.</span></span> 

## <a name="define-permitted-virtual-network-gateway-sku"></a><span data-ttu-id="a053e-106">Definování skladová položka brány virtuální sítě povolené</span><span class="sxs-lookup"><span data-stu-id="a053e-106">Define permitted virtual network gateway SKU</span></span>

<span data-ttu-id="a053e-107">Hello následující zásady omezuje, SKU, které mohou být nasazeny pro brány virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="a053e-107">hello following policy restricts which SKUs can be deployed for virtual network gateways:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="a053e-108">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a053e-108">Next steps</span></span>
* <span data-ttu-id="a053e-109">Po definování zásad pravidlo (jak je znázorněno v předchozích příkladech hello), budete potřebovat definice zásady hello toocreate a přiřaďte ho tooa oboru.</span><span class="sxs-lookup"><span data-stu-id="a053e-109">After defining a policy rule (as shown in hello preceding examples), you need toocreate hello policy definition and assign it tooa scope.</span></span> <span data-ttu-id="a053e-110">obor Hello může být předplatné, skupinu prostředků nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="a053e-110">hello scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="a053e-111">v tématu Zásady tooassign prostřednictvím portálu hello [Azure pomocí portálu tooassign a spravovat zásady prostředků](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a053e-111">tooassign policies through hello portal, see [Use Azure portal tooassign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="a053e-112">zásady tooassign prostřednictvím REST API, Powershellu nebo příkazového řádku Azure CLI, najdete v části [přiřadit a spravovat zásady prostřednictvím skriptu](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="a053e-112">tooassign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span> 
* <span data-ttu-id="a053e-113">Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="a053e-113">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

