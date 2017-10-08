---
title: "aaaEnforce zabezpečení se zásadami na virtuálních počítačích Windows v Azure | Microsoft Docs"
description: "Jak tooapply zásad tooan virtuálního počítače Azure Resource Manager Windows"
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 0b71ba54-01db-43ad-9bca-8ab358ae141b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: kasing
ms.openlocfilehash: b31c8a03ecf8eed6a929f97fe4146ea14364404f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="apply-policies-toowindows-vms-with-azure-resource-manager"></a>Použít zásady tooWindows virtuálních počítačů pomocí Azure Resource Manageru
Pomocí zásad můžete vynutit organizaci různé konvence a pravidla v rámci rozlehlé sítě hello. Vynucení hello požadovaného chování může pomoci zmírnit rizika při přidání toohello úspěch organizace hello. V tomto článku jsme popisují, jak můžete použít Azure Resource Manager zásady toodefine hello požadovaného chování pro virtuální počítače vaší organizace.

Toopolicies Úvod najdete v části [zásady používání toomanage prostředků a řízení přístupu](../../azure-resource-manager/resource-manager-policy.md).

## <a name="permitted-virtual-machines"></a>Povolené virtuální počítače
tooensure, jestli jsou kompatibilní s aplikací virtuálních počítačů ve vaší organizaci, můžete omezit hello povolené operační systémy. Ve hello následující ukázka zásad můžete povolit pouze virtuální počítače s Windows Server 2012 R2 Datacenter toobe vytvořit:

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "in": [
          "Microsoft.Compute/disks",
          "Microsoft.Compute/virtualMachines",
          "Microsoft.Compute/VirtualMachineScaleSets"
        ]
      },
      {
        "not": {
          "allOf": [
            {
              "field": "Microsoft.Compute/imagePublisher",
              "in": [
                "MicrosoftWindowsServer"
              ]
            },
            {
              "field": "Microsoft.Compute/imageOffer",
              "in": [
                "WindowsServer"
              ]
            },
            {
              "field": "Microsoft.Compute/imageSku",
              "in": [
                "2012-R2-Datacenter"
              ]
            },
            {
              "field": "Microsoft.Compute/imageVersion",
              "in": [
                "latest"
              ]
            }
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

Použijte zástupný znak toomodify hello předcházející zásad tooallow všechny bitové kopie systému Windows Server Datacenter:

```json
{
  "field": "Microsoft.Compute/imageSku",
  "like": "*Datacenter"
}
```

Použijte anyOf toomodify hello předcházející zásad tooallow žádné Windows Server 2012 R2 Datacenter nebo vyšší bitové kopie:

```json
{
  "anyOf": [
    {
      "field": "Microsoft.Compute/imageSku",
      "like": "2012-R2-Datacenter*"
    },
    {
      "field": "Microsoft.Compute/imageSku",
      "like": "2016-Datacenter*"
    }
  ]
}
```

Informace o polí zásad najdete v tématu [zásad aliasy](../../azure-resource-manager/resource-manager-policy.md#aliases).

## <a name="managed-disks"></a>Managed Disks

toorequire hello použití spravovaných disků, použijte hello následující zásady:

```json
{
  "if": {
    "anyOf": [
      {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/virtualMachines"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/osDisk.uri",
            "exists": true
          }
        ]
      },
      {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/VirtualMachineScaleSets"
          },
          {
            "anyOf": [
              {
                "field": "Microsoft.Compute/VirtualMachineScaleSets/osDisk.vhdContainers",
                "exists": true
              },
              {
                "field": "Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl",
                "exists": true
              }
            ]
          }
        ]
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="images-for-virtual-machines"></a>Bitové kopie u virtuálních počítačů

Z bezpečnostních důvodů se může vyžadovat, aby byly nasazené schválené vlastní Image ve vašem prostředí. Můžete zadat buď hello skupinu prostředků, která obsahuje hello schválení bitových kopií, nebo konkrétní Image schválené hello.

Následující ukázka Hello vyžaduje bitové kopie z skupiny schválené prostředků:

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "in": [
                    "Microsoft.Compute/virtualMachines",
                    "Microsoft.Compute/VirtualMachineScaleSets"
                ]
            },
            {
                "not": {
                    "field": "Microsoft.Compute/imageId",
                    "contains": "resourceGroups/CustomImage"
                }
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
} 
```

Hello následující příklad určuje hello schválení image ID:

```json
{
    "field": "Microsoft.Compute/imageId",
    "in": ["{imageId1}","{imageId2}"]
}
```

## <a name="virtual-machine-extensions"></a>Rozšíření virtuálního počítače

Můžete chtít tooforbid využití určitých typů rozšíření. Například rozšíření pravděpodobně není kompatibilní s obrázky určité vlastního virtuálního počítače. Následující příklad ukazuje, jak Hello tooblock konkrétní rozšíření. Vydavatel a typ toodetermine používá které tooblock rozšíření.

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.Compute/virtualMachines/extensions"
            },
            {
                "field": "Microsoft.Compute/virtualMachines/extensions/publisher",
                "equals": "Microsoft.Compute"
            },
            {
                "field": "Microsoft.Compute/virtualMachines/extensions/type",
                "equals": "{extension-type}"

      }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```


## <a name="azure-hybrid-use-benefit"></a>Výhody použití Azure hybridní

Pokud máte licenci místní, můžete uložit poplatek hello licence na virtuálních počítačích. Pokud nemáte licenci hello, by měl nezakazuje možnost hello. Hello následující zásady zakazuje použití Azure hybridní použití zvýhodnění (AHUB):

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "in":[ "Microsoft.Compute/virtualMachines","Microsoft.Compute/VirtualMachineScaleSets"]
            },
            {
                "field": "Microsoft.Compute/licenseType",
                "exists": true
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```

## <a name="next-steps"></a>Další kroky
* Po definování zásad pravidlo (jak je znázorněno v předchozích příkladech hello), budete potřebovat definice zásady hello toocreate a přiřaďte ho tooa oboru. obor Hello může být předplatné, skupinu prostředků nebo prostředek. v tématu Zásady tooassign prostřednictvím portálu hello [Azure pomocí portálu tooassign a spravovat zásady prostředků](../../azure-resource-manager/resource-manager-policy-portal.md). zásady tooassign prostřednictvím REST API, Powershellu nebo příkazového řádku Azure CLI, najdete v části [přiřadit a spravovat zásady prostřednictvím skriptu](../../azure-resource-manager/resource-manager-policy-create-assign.md).
* Úvod tooresource zásady, najdete v části [přehled zásad prostředků](../../azure-resource-manager/resource-manager-policy.md).
* Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](../../azure-resource-manager/resource-manager-subscription-governance.md).
