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
# <a name="apply-policies-to-linux-vms-with-azure-resource-manager"></a><span data-ttu-id="6beb1-103">Zásady použít pro virtuální počítače s Linuxem pomocí Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="6beb1-103">Apply policies to Linux VMs with Azure Resource Manager</span></span>
<span data-ttu-id="6beb1-104">Pomocí zásad můžete vynutit organizaci různé konvence a pravidla v rámci podniku.</span><span class="sxs-lookup"><span data-stu-id="6beb1-104">By using policies, an organization can enforce various conventions and rules throughout the enterprise.</span></span> <span data-ttu-id="6beb1-105">Vynucení požadované chování může pomoci zmírnit rizika při přispívání do úspěch organizace.</span><span class="sxs-lookup"><span data-stu-id="6beb1-105">Enforcement of the desired behavior can help mitigate risk while contributing to the success of the organization.</span></span> <span data-ttu-id="6beb1-106">V tomto článku jsme popisují, jak lze pomocí Azure Resource Manager zásad můžete určit požadované chování pro virtuální počítače vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="6beb1-106">In this article, we describe how you can use Azure Resource Manager policies to define the desired behavior for your organization's Virtual Machines.</span></span>

<span data-ttu-id="6beb1-107">Úvod do zásady, najdete v části [použití zásad ke správě prostředků a řízení přístupu](../../azure-resource-manager/resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="6beb1-107">For an introduction to policies, see [Use Policy to manage resources and control access](../../azure-resource-manager/resource-manager-policy.md).</span></span>

## <a name="permitted-virtual-machines"></a><span data-ttu-id="6beb1-108">Povolené virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="6beb1-108">Permitted Virtual Machines</span></span>
<span data-ttu-id="6beb1-109">Zajistit, že virtuální počítače ve vaší organizaci, jsou kompatibilní s aplikací, můžete omezit povolených operační systémy.</span><span class="sxs-lookup"><span data-stu-id="6beb1-109">To ensure that virtual machines for your organization are compatible with an application, you can restrict the permitted operating systems.</span></span> <span data-ttu-id="6beb1-110">V následujícím příkladu zásady Povolit pouze Ubuntu 14.04.2-LTS virtuální počítače, který se má vytvořit.</span><span class="sxs-lookup"><span data-stu-id="6beb1-110">In the following policy example, you allow only Ubuntu 14.04.2-LTS Virtual Machines to be created.</span></span>

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

<span data-ttu-id="6beb1-111">K úpravě předchozí zásada umožnit žádný obrázek Ubuntu LTS použijte zástupný znak:</span><span class="sxs-lookup"><span data-stu-id="6beb1-111">Use a wild card to modify the preceding policy to allow any Ubuntu LTS image:</span></span> 

```json
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*LTS"
}
```

<span data-ttu-id="6beb1-112">Informace o polí zásad najdete v tématu [zásad aliasy](../../azure-resource-manager/resource-manager-policy.md#aliases).</span><span class="sxs-lookup"><span data-stu-id="6beb1-112">For information about policy fields, see [Policy aliases](../../azure-resource-manager/resource-manager-policy.md#aliases).</span></span>

## <a name="managed-disks"></a><span data-ttu-id="6beb1-113">Managed Disks</span><span class="sxs-lookup"><span data-stu-id="6beb1-113">Managed disks</span></span>

<span data-ttu-id="6beb1-114">Pokud chcete vyžadovat použití spravovaných disků, použijte tyto zásady:</span><span class="sxs-lookup"><span data-stu-id="6beb1-114">To require the use of managed disks, use the following policy:</span></span>

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

## <a name="images-for-virtual-machines"></a><span data-ttu-id="6beb1-115">Bitové kopie u virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="6beb1-115">Images for Virtual Machines</span></span>

<span data-ttu-id="6beb1-116">Z bezpečnostních důvodů se může vyžadovat, aby byly nasazené schválené vlastní Image ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="6beb1-116">For security reasons, you can require that only approved custom images are deployed in your environment.</span></span> <span data-ttu-id="6beb1-117">Můžete zadat buď skupinu prostředků, která obsahuje Image schválené nebo konkrétní schválených bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="6beb1-117">You can specify either the resource group that contains the approved images, or the specific approved images.</span></span>

<span data-ttu-id="6beb1-118">Následující příklad vyžaduje bitové kopie z skupiny schválené prostředků:</span><span class="sxs-lookup"><span data-stu-id="6beb1-118">The following example requires images from an approved resource group:</span></span>

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

<span data-ttu-id="6beb1-119">Následující příklad určuje ID schválené bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="6beb1-119">The following example specifies the approved image IDs:</span></span>

```json
{
    "field": "Microsoft.Compute/imageId",
    "in": ["{imageId1}","{imageId2}"]
}
```

## <a name="virtual-machine-extensions"></a><span data-ttu-id="6beb1-120">Rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6beb1-120">Virtual Machine extensions</span></span>

<span data-ttu-id="6beb1-121">Můžete chtít nezakazuje využití určitých typů rozšíření.</span><span class="sxs-lookup"><span data-stu-id="6beb1-121">You may want to forbid usage of certain types of extensions.</span></span> <span data-ttu-id="6beb1-122">Například rozšíření pravděpodobně není kompatibilní s obrázky určité vlastního virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6beb1-122">For example, an extension may not be compatible with certain custom virtual machine images.</span></span> <span data-ttu-id="6beb1-123">Následující příklad ukazuje, jak chcete blokovat konkrétní rozšíření.</span><span class="sxs-lookup"><span data-stu-id="6beb1-123">The following example shows how to block a specific extension.</span></span> <span data-ttu-id="6beb1-124">Pomocí vydavatele a typ určit, které rozšíření, které chcete blokovat.</span><span class="sxs-lookup"><span data-stu-id="6beb1-124">It uses publisher and type to determine which extension to block.</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="6beb1-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6beb1-125">Next steps</span></span>
* <span data-ttu-id="6beb1-126">Po definování zásad pravidlo (jak je znázorněno v předchozích ukázkách), musíte k vytvoření definice zásady a přiřadit obor.</span><span class="sxs-lookup"><span data-stu-id="6beb1-126">After defining a policy rule (as shown in the preceding examples), you need to create the policy definition and assign it to a scope.</span></span> <span data-ttu-id="6beb1-127">Obor může být předplatné, skupinu prostředků nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="6beb1-127">The scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="6beb1-128">K přiřazení zásad prostřednictvím portálu, najdete v části [portálu Azure použijte přiřadit a spravovat zásady prostředků](../../azure-resource-manager/resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6beb1-128">To assign policies through the portal, see [Use Azure portal to assign and manage resource policies](../../azure-resource-manager/resource-manager-policy-portal.md).</span></span> <span data-ttu-id="6beb1-129">K přiřazení zásad pomocí rozhraní REST API, Powershellu nebo příkazového řádku Azure CLI, najdete v části [přiřadit a spravovat zásady prostřednictvím skriptu](../../azure-resource-manager/resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="6beb1-129">To assign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](../../azure-resource-manager/resource-manager-policy-create-assign.md).</span></span>
* <span data-ttu-id="6beb1-130">Úvod do zásad prostředků, najdete v části [přehled zásad prostředků](../../azure-resource-manager/resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="6beb1-130">For an introduction to resource policies, see [Resource policy overview](../../azure-resource-manager/resource-manager-policy.md).</span></span>
* <span data-ttu-id="6beb1-131">Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](../../azure-resource-manager/resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="6beb1-131">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](../../azure-resource-manager/resource-manager-subscription-governance.md).</span></span>
