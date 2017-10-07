---
title: "zásady aaaAzure prostředků pro zásady vytváření názvů | Microsoft Docs"
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
ms.openlocfilehash: c8384b231263fb694aed8b936a953d5c0ca31e71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="apply-resource-policies-for-names-and-text"></a><span data-ttu-id="03f4a-103">Použít zásady prostředků pro názvy a text.</span><span class="sxs-lookup"><span data-stu-id="03f4a-103">Apply resource policies for names and text</span></span>
<span data-ttu-id="03f4a-104">Toto téma ukazuje několik [zásady prostředků](resource-manager-policy.md) můžete použít tooestablish konvence pojmenování a text.</span><span class="sxs-lookup"><span data-stu-id="03f4a-104">This topic shows several [resource policies](resource-manager-policy.md) you can apply tooestablish naming and text conventions.</span></span> <span data-ttu-id="03f4a-105">Tyto zásady zajistit konzistenci pro názvy prostředků a hodnoty značky.</span><span class="sxs-lookup"><span data-stu-id="03f4a-105">These policies ensure consistency for resource names and tag values.</span></span> 

## <a name="set-naming-convention-with-wildcard"></a><span data-ttu-id="03f4a-106">Nastavit zásady vytváření názvů se zástupnými znaky</span><span class="sxs-lookup"><span data-stu-id="03f4a-106">Set naming convention with wildcard</span></span>
<span data-ttu-id="03f4a-107">Hello následující příklad ukazuje použití hello zástupný znak, který je podporovaný rozhraním hello **jako** podmínku.</span><span class="sxs-lookup"><span data-stu-id="03f4a-107">hello following example shows hello use of wildcard, which is supported by hello **like** condition.</span></span> <span data-ttu-id="03f4a-108">Hello podmínku stavy, pokud hello název neshoduje uvedených vzor hello (namePrefix\*nameSuffix) pak hello požadavek je odepřen:</span><span class="sxs-lookup"><span data-stu-id="03f4a-108">hello condition states that if hello name does match hello mentioned pattern (namePrefix\*nameSuffix) then deny hello request:</span></span>

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

## <a name="set-naming-convention-with-pattern"></a><span data-ttu-id="03f4a-109">Nastavit zásady vytváření názvů pomocí vzoru</span><span class="sxs-lookup"><span data-stu-id="03f4a-109">Set naming convention with pattern</span></span>

<span data-ttu-id="03f4a-110">toospecify, že názvy prostředků odpovídá vzorku, použijte hello vyhovují podmínce.</span><span class="sxs-lookup"><span data-stu-id="03f4a-110">toospecify that resource names match a pattern, use hello match condition.</span></span> <span data-ttu-id="03f4a-111">Hello následující příklad vyžaduje toostart názvy s `contoso` a obsahovat šesti další písmena:</span><span class="sxs-lookup"><span data-stu-id="03f4a-111">hello following example requires names toostart with `contoso` and contain six additional letters:</span></span>

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

## <a name="set-date-pattern-for-tag-value"></a><span data-ttu-id="03f4a-112">Nastavit datum vzor pro hodnotu značky</span><span class="sxs-lookup"><span data-stu-id="03f4a-112">Set date pattern for tag value</span></span>

<span data-ttu-id="03f4a-113">toorequire datum vzorec dvě číslice, pomlčky, tři písmena, dash a čtyři číslice, použijte:</span><span class="sxs-lookup"><span data-stu-id="03f4a-113">toorequire a date pattern of two digits, dash, three letters, dash, and four digits, use:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="03f4a-114">Další kroky</span><span class="sxs-lookup"><span data-stu-id="03f4a-114">Next steps</span></span>
* <span data-ttu-id="03f4a-115">Po definování zásad pravidlo (jak je znázorněno v předchozích příkladech hello), budete potřebovat definice zásady hello toocreate a přiřaďte ho tooa oboru.</span><span class="sxs-lookup"><span data-stu-id="03f4a-115">After defining a policy rule (as shown in hello preceding examples), you need toocreate hello policy definition and assign it tooa scope.</span></span> <span data-ttu-id="03f4a-116">obor Hello může být předplatné, skupinu prostředků nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="03f4a-116">hello scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="03f4a-117">v tématu Zásady tooassign prostřednictvím portálu hello [Azure pomocí portálu tooassign a spravovat zásady prostředků](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="03f4a-117">tooassign policies through hello portal, see [Use Azure portal tooassign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="03f4a-118">zásady tooassign prostřednictvím REST API, Powershellu nebo příkazového řádku Azure CLI, najdete v části [přiřadit a spravovat zásady prostřednictvím skriptu](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="03f4a-118">tooassign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span> 
* <span data-ttu-id="03f4a-119">Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="03f4a-119">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

