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
# <a name="apply-policies-toolinux-vms-with-azure-resource-manager"></a><span data-ttu-id="bbafd-103">Použít zásady tooLinux virtuálních počítačů pomocí Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="bbafd-103">Apply policies tooLinux VMs with Azure Resource Manager</span></span>
<span data-ttu-id="bbafd-104">Pomocí zásad můžete vynutit organizaci různé konvence a pravidla v rámci rozlehlé sítě hello.</span><span class="sxs-lookup"><span data-stu-id="bbafd-104">By using policies, an organization can enforce various conventions and rules throughout hello enterprise.</span></span> <span data-ttu-id="bbafd-105">Vynucení hello požadovaného chování může pomoci zmírnit rizika při přidání toohello úspěch organizace hello.</span><span class="sxs-lookup"><span data-stu-id="bbafd-105">Enforcement of hello desired behavior can help mitigate risk while contributing toohello success of hello organization.</span></span> <span data-ttu-id="bbafd-106">V tomto článku jsme popisují, jak můžete použít Azure Resource Manager zásady toodefine hello požadovaného chování pro virtuální počítače vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="bbafd-106">In this article, we describe how you can use Azure Resource Manager policies toodefine hello desired behavior for your organization's Virtual Machines.</span></span>

<span data-ttu-id="bbafd-107">Toopolicies Úvod najdete v části [zásady používání toomanage prostředků a řízení přístupu](../../azure-resource-manager/resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="bbafd-107">For an introduction toopolicies, see [Use Policy toomanage resources and control access](../../azure-resource-manager/resource-manager-policy.md).</span></span>

## <a name="permitted-virtual-machines"></a><span data-ttu-id="bbafd-108">Povolené virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="bbafd-108">Permitted Virtual Machines</span></span>
<span data-ttu-id="bbafd-109">tooensure, jestli jsou kompatibilní s aplikací virtuálních počítačů ve vaší organizaci, můžete omezit hello povolené operační systémy.</span><span class="sxs-lookup"><span data-stu-id="bbafd-109">tooensure that virtual machines for your organization are compatible with an application, you can restrict hello permitted operating systems.</span></span> <span data-ttu-id="bbafd-110">Ve hello následující ukázka zásad, můžete povolit pouze virtuální počítače, 14.04.2-LTS Ubuntu toobe vytvořili.</span><span class="sxs-lookup"><span data-stu-id="bbafd-110">In hello following policy example, you allow only Ubuntu 14.04.2-LTS Virtual Machines toobe created.</span></span>

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

<span data-ttu-id="bbafd-111">Použijte hello toomodify zástupný znak předcházející žádný obrázek Ubuntu LTS tooallow zásad:</span><span class="sxs-lookup"><span data-stu-id="bbafd-111">Use a wild card toomodify hello preceding policy tooallow any Ubuntu LTS image:</span></span> 

```json
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*LTS"
}
```

<span data-ttu-id="bbafd-112">Informace o polí zásad najdete v tématu [zásad aliasy](../../azure-resource-manager/resource-manager-policy.md#aliases).</span><span class="sxs-lookup"><span data-stu-id="bbafd-112">For information about policy fields, see [Policy aliases](../../azure-resource-manager/resource-manager-policy.md#aliases).</span></span>

## <a name="managed-disks"></a><span data-ttu-id="bbafd-113">Managed Disks</span><span class="sxs-lookup"><span data-stu-id="bbafd-113">Managed disks</span></span>

<span data-ttu-id="bbafd-114">toorequire hello použití spravovaných disků, použijte hello následující zásady:</span><span class="sxs-lookup"><span data-stu-id="bbafd-114">toorequire hello use of managed disks, use hello following policy:</span></span>

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

## <a name="images-for-virtual-machines"></a><span data-ttu-id="bbafd-115">Bitové kopie u virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="bbafd-115">Images for Virtual Machines</span></span>

<span data-ttu-id="bbafd-116">Z bezpečnostních důvodů se může vyžadovat, aby byly nasazené schválené vlastní Image ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="bbafd-116">For security reasons, you can require that only approved custom images are deployed in your environment.</span></span> <span data-ttu-id="bbafd-117">Můžete zadat buď hello skupinu prostředků, která obsahuje hello schválení bitových kopií, nebo konkrétní Image schválené hello.</span><span class="sxs-lookup"><span data-stu-id="bbafd-117">You can specify either hello resource group that contains hello approved images, or hello specific approved images.</span></span>

<span data-ttu-id="bbafd-118">Následující ukázka Hello vyžaduje bitové kopie z skupiny schválené prostředků:</span><span class="sxs-lookup"><span data-stu-id="bbafd-118">hello following example requires images from an approved resource group:</span></span>

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

<span data-ttu-id="bbafd-119">Hello následující příklad určuje hello schválení image ID:</span><span class="sxs-lookup"><span data-stu-id="bbafd-119">hello following example specifies hello approved image IDs:</span></span>

```json
{
    "field": "Microsoft.Compute/imageId",
    "in": ["{imageId1}","{imageId2}"]
}
```

## <a name="virtual-machine-extensions"></a><span data-ttu-id="bbafd-120">Rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="bbafd-120">Virtual Machine extensions</span></span>

<span data-ttu-id="bbafd-121">Můžete chtít tooforbid využití určitých typů rozšíření.</span><span class="sxs-lookup"><span data-stu-id="bbafd-121">You may want tooforbid usage of certain types of extensions.</span></span> <span data-ttu-id="bbafd-122">Například rozšíření pravděpodobně není kompatibilní s obrázky určité vlastního virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bbafd-122">For example, an extension may not be compatible with certain custom virtual machine images.</span></span> <span data-ttu-id="bbafd-123">Následující příklad ukazuje, jak Hello tooblock konkrétní rozšíření.</span><span class="sxs-lookup"><span data-stu-id="bbafd-123">hello following example shows how tooblock a specific extension.</span></span> <span data-ttu-id="bbafd-124">Vydavatel a typ toodetermine používá které tooblock rozšíření.</span><span class="sxs-lookup"><span data-stu-id="bbafd-124">It uses publisher and type toodetermine which extension tooblock.</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="bbafd-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bbafd-125">Next steps</span></span>
* <span data-ttu-id="bbafd-126">Po definování zásad pravidlo (jak je znázorněno v předchozích příkladech hello), budete potřebovat definice zásady hello toocreate a přiřaďte ho tooa oboru.</span><span class="sxs-lookup"><span data-stu-id="bbafd-126">After defining a policy rule (as shown in hello preceding examples), you need toocreate hello policy definition and assign it tooa scope.</span></span> <span data-ttu-id="bbafd-127">obor Hello může být předplatné, skupinu prostředků nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="bbafd-127">hello scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="bbafd-128">v tématu Zásady tooassign prostřednictvím portálu hello [Azure pomocí portálu tooassign a spravovat zásady prostředků](../../azure-resource-manager/resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="bbafd-128">tooassign policies through hello portal, see [Use Azure portal tooassign and manage resource policies](../../azure-resource-manager/resource-manager-policy-portal.md).</span></span> <span data-ttu-id="bbafd-129">zásady tooassign prostřednictvím REST API, Powershellu nebo příkazového řádku Azure CLI, najdete v části [přiřadit a spravovat zásady prostřednictvím skriptu](../../azure-resource-manager/resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="bbafd-129">tooassign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](../../azure-resource-manager/resource-manager-policy-create-assign.md).</span></span>
* <span data-ttu-id="bbafd-130">Úvod tooresource zásady, najdete v části [přehled zásad prostředků](../../azure-resource-manager/resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="bbafd-130">For an introduction tooresource policies, see [Resource policy overview](../../azure-resource-manager/resource-manager-policy.md).</span></span>
* <span data-ttu-id="bbafd-131">Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](../../azure-resource-manager/resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="bbafd-131">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](../../azure-resource-manager/resource-manager-subscription-governance.md).</span></span>
