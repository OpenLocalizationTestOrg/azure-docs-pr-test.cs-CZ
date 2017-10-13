---
title: "Vynutit zabezpečení se zásadami na virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Tom, jak používat zásady pro správce prostředků Linux virtuální počítač Azure"
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
ms.openlocfilehash: 58eaab4fa03afc1e6a5e38bef691cce62a921ea9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="apply-policies-to-linux-vms-with-azure-resource-manager"></a>Zásady použít pro virtuální počítače s Linuxem pomocí Azure Resource Manageru
Pomocí zásad můžete vynutit organizaci různé konvence a pravidla v rámci podniku. Vynucení požadované chování může pomoci zmírnit rizika při přispívání do úspěch organizace. V tomto článku jsme popisují, jak lze pomocí Azure Resource Manager zásad můžete určit požadované chování pro virtuální počítače vaší organizace.

Úvod do zásady, najdete v části [použití zásad ke správě prostředků a řízení přístupu](../../azure-resource-manager/resource-manager-policy.md).

## <a name="permitted-virtual-machines"></a>Povolené virtuální počítače
Zajistit, že virtuální počítače ve vaší organizaci, jsou kompatibilní s aplikací, můžete omezit povolených operační systémy. V následujícím příkladu zásady Povolit pouze Ubuntu 14.04.2-LTS virtuální počítače, který se má vytvořit.

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

K úpravě předchozí zásada umožnit žádný obrázek Ubuntu LTS použijte zástupný znak: 

```json
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*LTS"
}
```

Informace o polí zásad najdete v tématu [zásad aliasy](../../azure-resource-manager/resource-manager-policy.md#aliases).

## <a name="managed-disks"></a>Managed Disks

Pokud chcete vyžadovat použití spravovaných disků, použijte tyto zásady:

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

Z bezpečnostních důvodů se může vyžadovat, aby byly nasazené schválené vlastní Image ve vašem prostředí. Můžete zadat buď skupinu prostředků, která obsahuje Image schválené nebo konkrétní schválených bitové kopie.

Následující příklad vyžaduje bitové kopie z skupiny schválené prostředků:

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

Následující příklad určuje ID schválené bitové kopie:

```json
{
    "field": "Microsoft.Compute/imageId",
    "in": ["{imageId1}","{imageId2}"]
}
```

## <a name="virtual-machine-extensions"></a>Rozšíření virtuálního počítače

Můžete chtít nezakazuje využití určitých typů rozšíření. Například rozšíření pravděpodobně není kompatibilní s obrázky určité vlastního virtuálního počítače. Následující příklad ukazuje, jak chcete blokovat konkrétní rozšíření. Pomocí vydavatele a typ určit, které rozšíření, které chcete blokovat.

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
* Po definování zásad pravidlo (jak je znázorněno v předchozích ukázkách), musíte k vytvoření definice zásady a přiřadit obor. Obor může být předplatné, skupinu prostředků nebo prostředek. K přiřazení zásad prostřednictvím portálu, najdete v části [portálu Azure použijte přiřadit a spravovat zásady prostředků](../../azure-resource-manager/resource-manager-policy-portal.md). K přiřazení zásad pomocí rozhraní REST API, Powershellu nebo příkazového řádku Azure CLI, najdete v části [přiřadit a spravovat zásady prostřednictvím skriptu](../../azure-resource-manager/resource-manager-policy-create-assign.md).
* Úvod do zásad prostředků, najdete v části [přehled zásad prostředků](../../azure-resource-manager/resource-manager-policy.md).
* Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](../../azure-resource-manager/resource-manager-subscription-governance.md).
