---
title: "zásady aaaAzure prostředků pro značky | Microsoft Docs"
description: "Obsahuje příklady prostředků zásad pro správu značky na prostředky"
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
ms.date: 08/24/2017
ms.author: tomfitz
ms.openlocfilehash: 5a5b3d5ed52b47544b397694b9da0070f61b1faf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="apply-resource-policies-for-tags"></a><span data-ttu-id="9c357-103">Použít zásady prostředků pro značky</span><span class="sxs-lookup"><span data-stu-id="9c357-103">Apply resource policies for tags</span></span>

<span data-ttu-id="9c357-104">Toto téma obsahuje běžné zásady pravidla, že které můžete použít tooensure konzistentní použití značek na prostředky.</span><span class="sxs-lookup"><span data-stu-id="9c357-104">This topic provides common policy rules you can apply tooensure consistent use of tags on resources.</span></span>

<span data-ttu-id="9c357-105">Použití skupiny prostředků tooa zásad značka nebo předplatné se stávajícími prostředky nevztahuje zpětně hello zásad toothose prostředky.</span><span class="sxs-lookup"><span data-stu-id="9c357-105">Applying a tag policy tooa resource group or subscription with existing resources does not retroactively apply hello policy toothose resources.</span></span> <span data-ttu-id="9c357-106">zásady hello tooenforce na tyto prostředky aktivovat toohello aktualizaci existující prostředky.</span><span class="sxs-lookup"><span data-stu-id="9c357-106">tooenforce hello policies on those resources, trigger an update toohello existing resources.</span></span> <span data-ttu-id="9c357-107">Tento článek obsahuje příklad PowerShell pro spuštění aktualizace.</span><span class="sxs-lookup"><span data-stu-id="9c357-107">This article includes a PowerShell example for triggering an update.</span></span>

## <a name="ensure-all-resources-in-a-resource-group-have-a-tagvalue"></a><span data-ttu-id="9c357-108">Ověřte, zda že mají všechny prostředky ve skupině prostředků a jejich hodnoty</span><span class="sxs-lookup"><span data-stu-id="9c357-108">Ensure all resources in a resource group have a tag/value</span></span>

<span data-ttu-id="9c357-109">Běžným požadavkem je, že mají všechny prostředky ve skupině prostředků konkrétní značku a hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9c357-109">A common requirement is that all resources in a resource group have a particular tag and value.</span></span> <span data-ttu-id="9c357-110">Tento požadavek je často náklady na potřebné tootrack oddělení.</span><span class="sxs-lookup"><span data-stu-id="9c357-110">This requirement is often needed tootrack costs by department.</span></span> <span data-ttu-id="9c357-111">musí být splněné Hello následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="9c357-111">hello following conditions must be met:</span></span>

* <span data-ttu-id="9c357-112">Hello požadované značky a hodnoty jsou připojením toonew a aktualizovat prostředky, které nemají hello značky.</span><span class="sxs-lookup"><span data-stu-id="9c357-112">hello required tag and value are appended toonew and updated resources that do not have hello tag.</span></span>
* <span data-ttu-id="9c357-113">Hello požadované značky a hodnotu nelze odebrat ze všech stávajících prostředků.</span><span class="sxs-lookup"><span data-stu-id="9c357-113">hello required tag and value cannot be removed from any existing resources.</span></span>

<span data-ttu-id="9c357-114">Tento požadavek můžete dosáhnout použitím dvě skupiny prostředků tooa integrovaných zásad.</span><span class="sxs-lookup"><span data-stu-id="9c357-114">You accomplish this requirement by applying two built-in policies tooa resource group.</span></span>

| <span data-ttu-id="9c357-115">ID</span><span class="sxs-lookup"><span data-stu-id="9c357-115">ID</span></span> | <span data-ttu-id="9c357-116">Popis</span><span class="sxs-lookup"><span data-stu-id="9c357-116">Description</span></span> |
| ---- | ---- |
| <span data-ttu-id="9c357-117">2a0e14a6-b0a6-4fab-991a-187a4f81c498</span><span class="sxs-lookup"><span data-stu-id="9c357-117">2a0e14a6-b0a6-4fab-991a-187a4f81c498</span></span> | <span data-ttu-id="9c357-118">Když není zadaný uživatelem hello se vztahuje požadované značky a jeho výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9c357-118">Applies a required tag and its default value when it is not specified by hello user.</span></span> |
| <span data-ttu-id="9c357-119">1e30110a-5ceb-460c-a204-c1c3969c6d62</span><span class="sxs-lookup"><span data-stu-id="9c357-119">1e30110a-5ceb-460c-a204-c1c3969c6d62</span></span> | <span data-ttu-id="9c357-120">Vynucuje požadované značky a jeho hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9c357-120">Enforces a required tag and its value.</span></span> |

### <a name="powershell"></a><span data-ttu-id="9c357-121">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9c357-121">PowerShell</span></span>

<span data-ttu-id="9c357-122">Následující skript prostředí PowerShell Hello přiřadí skupinu prostředků tooa definice dvě předdefinované zásady hello.</span><span class="sxs-lookup"><span data-stu-id="9c357-122">hello following PowerShell script assigns hello two built-in policy definitions tooa resource group.</span></span> <span data-ttu-id="9c357-123">Před spuštěním skriptu hello, přiřaďte všechny skupiny prostředků toohello požadované značky.</span><span class="sxs-lookup"><span data-stu-id="9c357-123">Before running hello script, assign all required tags toohello resource group.</span></span> <span data-ttu-id="9c357-124">Jednotlivé značky na skupinu prostředků hello je vyžadován pro hello prostředky ve skupině hello.</span><span class="sxs-lookup"><span data-stu-id="9c357-124">Each tag on hello resource group is required for hello resources in hello group.</span></span> <span data-ttu-id="9c357-125">tooassign tooall skupin prostředků v rámci vašeho předplatného, neposkytují hello `-Name` parametr při získávání skupin prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="9c357-125">tooassign tooall resource groups in your subscription, do not provide hello `-Name` parameter when getting hello resource groups.</span></span>

```powershell
$appendpolicy = Get-AzureRmPolicyDefinition | Where-Object {$_.Name -eq '2a0e14a6-b0a6-4fab-991a-187a4f81c498'}
$denypolicy = Get-AzureRmPolicyDefinition | Where-Object {$_.Name -eq '1e30110a-5ceb-460c-a204-c1c3969c6d62'}

$rgs = Get-AzureRMResourceGroup -Name ExampleGroup

foreach($rg in $rgs)
{
    $tags = $rg.Tags
    foreach($key in $tags.Keys){
        $key 
        $tags[$key]
        New-AzureRmPolicyAssignment -Name ("append"+$key+"tag") -PolicyDefinition $appendpolicy -Scope $rg.ResourceId -tagName $key -tagValue  $tags[$key]
        New-AzureRmPolicyAssignment -Name ("denywithout"+$key+"tag") -PolicyDefinition $denypolicy -Scope $rg.ResourceId -tagName $key -tagValue  $tags[$key]
    }
}
```

<span data-ttu-id="9c357-126">Po přiřazení hello zásady, můžete aktivovat tooall aktualizaci existující prostředky tooenforce hello značky zásady, které jste přidali.</span><span class="sxs-lookup"><span data-stu-id="9c357-126">After assigning hello policies, you can trigger an update tooall existing resources tooenforce hello tag policies you have added.</span></span> <span data-ttu-id="9c357-127">Hello následující skript uchovává všechny značky, které byly na hello prostředky:</span><span class="sxs-lookup"><span data-stu-id="9c357-127">hello following script retains any other tags that existed on hello resources:</span></span>

```powershell
$group = Get-AzureRmResourceGroup -Name "ExampleGroup" 

$resources = Find-AzureRmResource -ResourceGroupName $group.ResourceGroupName 

foreach($r in $resources)
{
    try{
        $r | Set-AzureRmResource -Tags ($a=if($r.Tags -eq $NULL) { @{}} else {$r.Tags}) -Force -UsePatchSemantics
    }
    catch{
        Write-Host  $r.ResourceId + "can't be updated"
    }
}
```

## <a name="require-tags-for-a-resource-type"></a><span data-ttu-id="9c357-128">Vyžadovat značky pro typ prostředku</span><span class="sxs-lookup"><span data-stu-id="9c357-128">Require tags for a resource type</span></span>
<span data-ttu-id="9c357-129">Hello následující příklad ukazuje, jak označit toonest logické operátory toorequire aplikaci pouze na typ zadaného prostředku (v této případu, účty úložiště).</span><span class="sxs-lookup"><span data-stu-id="9c357-129">hello following example shows how toonest logical operators toorequire an application tag for only a specified resource type (in this case, storage accounts).</span></span>

```json
{
    "if": {
        "allOf": [
          {
            "not": {
              "field": "tags",
              "containsKey": "application"
            }
          },
          {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
          }
        ]
    },
    "then": {
        "effect": "audit"
    }
}
```

## <a name="require-tag"></a><span data-ttu-id="9c357-130">Vyžadovat značky</span><span class="sxs-lookup"><span data-stu-id="9c357-130">Require tag</span></span>
<span data-ttu-id="9c357-131">Hello tyto zásady odmítne požadavky, které nemají značku obsahující klíč "costCenter" (můžete použít libovolná hodnota):</span><span class="sxs-lookup"><span data-stu-id="9c357-131">hello following policy denies requests that don't have a tag containing "costCenter" key (any value can be applied):</span></span>

```json
{
  "if": {
    "not" : {
      "field" : "tags",
      "containsKey" : "costCenter"
    }
  },
  "then" : {
    "effect" : "deny"
  }
}
```

## <a name="next-steps"></a><span data-ttu-id="9c357-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9c357-132">Next steps</span></span>
* <span data-ttu-id="9c357-133">Po definování zásad pravidlo (jak je znázorněno v předchozích příkladech hello), budete potřebovat definice zásady hello toocreate a přiřaďte ho tooa oboru.</span><span class="sxs-lookup"><span data-stu-id="9c357-133">After defining a policy rule (as shown in hello preceding examples), you need toocreate hello policy definition and assign it tooa scope.</span></span> <span data-ttu-id="9c357-134">obor Hello může být předplatné, skupinu prostředků nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="9c357-134">hello scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="9c357-135">v tématu Zásady tooassign prostřednictvím portálu hello [Azure pomocí portálu tooassign a spravovat zásady prostředků](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9c357-135">tooassign policies through hello portal, see [Use Azure portal tooassign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="9c357-136">zásady tooassign prostřednictvím REST API, Powershellu nebo příkazového řádku Azure CLI, najdete v části [přiřadit a spravovat zásady prostřednictvím skriptu](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="9c357-136">tooassign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span>
* <span data-ttu-id="9c357-137">Úvod tooresource zásady, najdete v části [přehled zásad prostředků](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="9c357-137">For an introduction tooresource policies, see [Resource policy overview](resource-manager-policy.md).</span></span>
* <span data-ttu-id="9c357-138">Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="9c357-138">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

