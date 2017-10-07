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
# <a name="apply-resource-policies-for-tags"></a>Použít zásady prostředků pro značky

Toto téma obsahuje běžné zásady pravidla, že které můžete použít tooensure konzistentní použití značek na prostředky.

Použití skupiny prostředků tooa zásad značka nebo předplatné se stávajícími prostředky nevztahuje zpětně hello zásad toothose prostředky. zásady hello tooenforce na tyto prostředky aktivovat toohello aktualizaci existující prostředky. Tento článek obsahuje příklad PowerShell pro spuštění aktualizace.

## <a name="ensure-all-resources-in-a-resource-group-have-a-tagvalue"></a>Ověřte, zda že mají všechny prostředky ve skupině prostředků a jejich hodnoty

Běžným požadavkem je, že mají všechny prostředky ve skupině prostředků konkrétní značku a hodnotu. Tento požadavek je často náklady na potřebné tootrack oddělení. musí být splněné Hello následující podmínky:

* Hello požadované značky a hodnoty jsou připojením toonew a aktualizovat prostředky, které nemají hello značky.
* Hello požadované značky a hodnotu nelze odebrat ze všech stávajících prostředků.

Tento požadavek můžete dosáhnout použitím dvě skupiny prostředků tooa integrovaných zásad.

| ID | Popis |
| ---- | ---- |
| 2a0e14a6-b0a6-4fab-991a-187a4f81c498 | Když není zadaný uživatelem hello se vztahuje požadované značky a jeho výchozí hodnotu. |
| 1e30110a-5ceb-460c-a204-c1c3969c6d62 | Vynucuje požadované značky a jeho hodnotu. |

### <a name="powershell"></a>PowerShell

Následující skript prostředí PowerShell Hello přiřadí skupinu prostředků tooa definice dvě předdefinované zásady hello. Před spuštěním skriptu hello, přiřaďte všechny skupiny prostředků toohello požadované značky. Jednotlivé značky na skupinu prostředků hello je vyžadován pro hello prostředky ve skupině hello. tooassign tooall skupin prostředků v rámci vašeho předplatného, neposkytují hello `-Name` parametr při získávání skupin prostředků hello.

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

Po přiřazení hello zásady, můžete aktivovat tooall aktualizaci existující prostředky tooenforce hello značky zásady, které jste přidali. Hello následující skript uchovává všechny značky, které byly na hello prostředky:

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

## <a name="require-tags-for-a-resource-type"></a>Vyžadovat značky pro typ prostředku
Hello následující příklad ukazuje, jak označit toonest logické operátory toorequire aplikaci pouze na typ zadaného prostředku (v této případu, účty úložiště).

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

## <a name="require-tag"></a>Vyžadovat značky
Hello tyto zásady odmítne požadavky, které nemají značku obsahující klíč "costCenter" (můžete použít libovolná hodnota):

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

## <a name="next-steps"></a>Další kroky
* Po definování zásad pravidlo (jak je znázorněno v předchozích příkladech hello), budete potřebovat definice zásady hello toocreate a přiřaďte ho tooa oboru. obor Hello může být předplatné, skupinu prostředků nebo prostředek. v tématu Zásady tooassign prostřednictvím portálu hello [Azure pomocí portálu tooassign a spravovat zásady prostředků](resource-manager-policy-portal.md). zásady tooassign prostřednictvím REST API, Powershellu nebo příkazového řádku Azure CLI, najdete v části [přiřadit a spravovat zásady prostřednictvím skriptu](resource-manager-policy-create-assign.md).
* Úvod tooresource zásady, najdete v části [přehled zásad prostředků](resource-manager-policy.md).
* Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).

