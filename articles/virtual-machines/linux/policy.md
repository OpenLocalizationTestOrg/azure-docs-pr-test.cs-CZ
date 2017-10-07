---
title: "aaaEnforce zabezpečení se zásadami na virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Jak tooapply tooan zásad Azure Resource Manager Linux virtuálního počítače"
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 06778ab4-f8ff-4eed-ae10-26a276fc3faa
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: singhkay
ms.openlocfilehash: 5abd0c937578aba7e72b62c65b4eef9a9737aa2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="apply-policies-toolinux-vms-with-azure-resource-manager"></a>Použít zásady tooLinux virtuálních počítačů pomocí Azure Resource Manageru
Pomocí zásad můžete vynutit organizaci různé konvence a pravidla v rámci rozlehlé sítě hello. Vynucení hello požadovaného chování může pomoci zmírnit rizika při přidání toohello úspěch organizace hello. V tomto článku jsme popisují, jak můžete použít Azure Resource Manager zásady toodefine hello požadovaného chování pro virtuální počítače vaší organizace.

Toopolicies Úvod najdete v části [zásady používání toomanage prostředků a řízení přístupu](../../azure-resource-manager/resource-manager-policy.md).

## <a name="permitted-virtual-machines"></a>Povolené virtuální počítače
tooensure, jestli jsou kompatibilní s aplikací virtuálních počítačů ve vaší organizaci, můžete omezit hello povolené operační systémy. Ve hello následující ukázka zásad, můžete povolit pouze virtuální počítače, 14.04.2-LTS Ubuntu toobe vytvořili.

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
                "Canonical"
              ]
            },
            {
              "field": "Microsoft.Compute/imageOffer",
              "in": [
                "UbuntuServer"
              ]
            },
            {
              "field": "Microsoft.Compute/imageSku",
              "in": [
                "14.04.2-LTS"
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

Použijte hello toomodify zástupný znak předcházející žádný obrázek Ubuntu LTS tooallow zásad: 

```json
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*LTS"
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


## <a name="next-steps"></a>Další kroky
* Po definování zásad pravidlo (jak je znázorněno v předchozích příkladech hello), budete potřebovat definice zásady hello toocreate a přiřaďte ho tooa oboru. obor Hello může být předplatné, skupinu prostředků nebo prostředek. v tématu Zásady tooassign prostřednictvím portálu hello [Azure pomocí portálu tooassign a spravovat zásady prostředků](../../azure-resource-manager/resource-manager-policy-portal.md). zásady tooassign prostřednictvím REST API, Powershellu nebo příkazového řádku Azure CLI, najdete v části [přiřadit a spravovat zásady prostřednictvím skriptu](../../azure-resource-manager/resource-manager-policy-create-assign.md).
* Úvod tooresource zásady, najdete v části [přehled zásad prostředků](../../azure-resource-manager/resource-manager-policy.md).
* Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](../../azure-resource-manager/resource-manager-subscription-governance.md).
