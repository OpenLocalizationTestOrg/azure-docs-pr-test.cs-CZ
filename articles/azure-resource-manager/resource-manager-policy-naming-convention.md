---
title: "Zásady prostředků Azure pro zásady vytváření názvů | Microsoft Docs"
description: "Popisuje zásady správce prostředků Azure pro zásady vytváření názvů prostředků."
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
ms.date: 06/27/2017
ms.author: tomfitz
ms.openlocfilehash: 51b3519bbba8cb4c768bfdd7dadf92fced434f22
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="apply-resource-policies-for-names-and-text"></a><span data-ttu-id="0d71e-103">Použít zásady prostředků pro názvy a text.</span><span class="sxs-lookup"><span data-stu-id="0d71e-103">Apply resource policies for names and text</span></span>
<span data-ttu-id="0d71e-104">Toto téma ukazuje několik [zásady prostředků](resource-manager-policy.md) můžete použít k vytvoření konvence pojmenování a text.</span><span class="sxs-lookup"><span data-stu-id="0d71e-104">This topic shows several [resource policies](resource-manager-policy.md) you can apply to establish naming and text conventions.</span></span> <span data-ttu-id="0d71e-105">Tyto zásady zajistit konzistenci pro názvy prostředků a hodnoty značky.</span><span class="sxs-lookup"><span data-stu-id="0d71e-105">These policies ensure consistency for resource names and tag values.</span></span> 

## <a name="set-naming-convention-with-wildcard"></a><span data-ttu-id="0d71e-106">Nastavit zásady vytváření názvů se zástupnými znaky</span><span class="sxs-lookup"><span data-stu-id="0d71e-106">Set naming convention with wildcard</span></span>
<span data-ttu-id="0d71e-107">Následující příklad ukazuje použití zástupných znaků, který je podporovaný rozhraním **jako** podmínku.</span><span class="sxs-lookup"><span data-stu-id="0d71e-107">The following example shows the use of wildcard, which is supported by the **like** condition.</span></span> <span data-ttu-id="0d71e-108">Podmínka uvádí, že pokud název neodpovídá vzoru uvedených (namePrefix\*nameSuffix) pak daný požadavek je odepřen:</span><span class="sxs-lookup"><span data-stu-id="0d71e-108">The condition states that if the name does match the mentioned pattern (namePrefix\*nameSuffix) then deny the request:</span></span>

```json
{
  "if": {
    "not": {
      "field": "name",
      "like": "namePrefix*nameSuffix"
    }
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="set-naming-convention-with-pattern"></a><span data-ttu-id="0d71e-109">Nastavit zásady vytváření názvů pomocí vzoru</span><span class="sxs-lookup"><span data-stu-id="0d71e-109">Set naming convention with pattern</span></span>

<span data-ttu-id="0d71e-110">Chcete-li určit, že názvy prostředků odpovídá vzorku, použijte podmínku shodu.</span><span class="sxs-lookup"><span data-stu-id="0d71e-110">To specify that resource names match a pattern, use the match condition.</span></span> <span data-ttu-id="0d71e-111">Následující příklad vyžaduje názvy začínat `contoso` a obsahovat šesti další písmena:</span><span class="sxs-lookup"><span data-stu-id="0d71e-111">The following example requires names to start with `contoso` and contain six additional letters:</span></span>

```json
{
  "if": {
    "not": {
      "field": "name",
      "match": "contoso??????"
    }
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="set-date-pattern-for-tag-value"></a><span data-ttu-id="0d71e-112">Nastavit datum vzor pro hodnotu značky</span><span class="sxs-lookup"><span data-stu-id="0d71e-112">Set date pattern for tag value</span></span>

<span data-ttu-id="0d71e-113">Tak, aby vyžadovala datum vzorec dvě číslice, pomlčky, tři písmena, dash a čtyři číslice, použijte:</span><span class="sxs-lookup"><span data-stu-id="0d71e-113">To require a date pattern of two digits, dash, three letters, dash, and four digits, use:</span></span>

```json
{
  "if": {
    "field": "tags.date",
    "match": "##-???-####"
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="next-steps"></a><span data-ttu-id="0d71e-114">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0d71e-114">Next steps</span></span>
* <span data-ttu-id="0d71e-115">Po definování zásad pravidlo (jak je znázorněno v předchozích ukázkách), musíte k vytvoření definice zásady a přiřadit obor.</span><span class="sxs-lookup"><span data-stu-id="0d71e-115">After defining a policy rule (as shown in the preceding examples), you need to create the policy definition and assign it to a scope.</span></span> <span data-ttu-id="0d71e-116">Obor může být předplatné, skupinu prostředků nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="0d71e-116">The scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="0d71e-117">K přiřazení zásad prostřednictvím portálu, najdete v části [portálu Azure použijte přiřadit a spravovat zásady prostředků](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="0d71e-117">To assign policies through the portal, see [Use Azure portal to assign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="0d71e-118">K přiřazení zásad pomocí rozhraní REST API, Powershellu nebo příkazového řádku Azure CLI, najdete v části [přiřadit a spravovat zásady prostřednictvím skriptu](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="0d71e-118">To assign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span> 
* <span data-ttu-id="0d71e-119">Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="0d71e-119">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

