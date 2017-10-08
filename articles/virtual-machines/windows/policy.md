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
# <a name="apply-policies-toowindows-vms-with-azure-resource-manager"></a><span data-ttu-id="34889-103">Použít zásady tooWindows virtuálních počítačů pomocí Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="34889-103">Apply policies tooWindows VMs with Azure Resource Manager</span></span>
<span data-ttu-id="34889-104">Pomocí zásad můžete vynutit organizaci různé konvence a pravidla v rámci rozlehlé sítě hello.</span><span class="sxs-lookup"><span data-stu-id="34889-104">By using policies, an organization can enforce various conventions and rules throughout hello enterprise.</span></span> <span data-ttu-id="34889-105">Vynucení hello požadovaného chování může pomoci zmírnit rizika při přidání toohello úspěch organizace hello.</span><span class="sxs-lookup"><span data-stu-id="34889-105">Enforcement of hello desired behavior can help mitigate risk while contributing toohello success of hello organization.</span></span> <span data-ttu-id="34889-106">V tomto článku jsme popisují, jak můžete použít Azure Resource Manager zásady toodefine hello požadovaného chování pro virtuální počítače vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="34889-106">In this article, we describe how you can use Azure Resource Manager policies toodefine hello desired behavior for your organization’s Virtual Machines.</span></span>

<span data-ttu-id="34889-107">Toopolicies Úvod najdete v části [zásady používání toomanage prostředků a řízení přístupu](../../azure-resource-manager/resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="34889-107">For an introduction toopolicies, see [Use Policy toomanage resources and control access](../../azure-resource-manager/resource-manager-policy.md).</span></span>

## <a name="permitted-virtual-machines"></a><span data-ttu-id="34889-108">Povolené virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="34889-108">Permitted Virtual Machines</span></span>
<span data-ttu-id="34889-109">tooensure, jestli jsou kompatibilní s aplikací virtuálních počítačů ve vaší organizaci, můžete omezit hello povolené operační systémy.</span><span class="sxs-lookup"><span data-stu-id="34889-109">tooensure that virtual machines for your organization are compatible with an application, you can restrict hello permitted operating systems.</span></span> <span data-ttu-id="34889-110">Ve hello následující ukázka zásad můžete povolit pouze virtuální počítače s Windows Server 2012 R2 Datacenter toobe vytvořit:</span><span class="sxs-lookup"><span data-stu-id="34889-110">In hello following policy example, you allow only Windows Server 2012 R2 Datacenter Virtual Machines toobe created:</span></span>

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

<span data-ttu-id="34889-111">Použijte zástupný znak toomodify hello předcházející zásad tooallow všechny bitové kopie systému Windows Server Datacenter:</span><span class="sxs-lookup"><span data-stu-id="34889-111">Use a wild card toomodify hello preceding policy tooallow any Windows Server Datacenter image:</span></span>

```json
{
  "field": "Microsoft.Compute/imageSku",
  "like": "*Datacenter"
}
```

<span data-ttu-id="34889-112">Použijte anyOf toomodify hello předcházející zásad tooallow žádné Windows Server 2012 R2 Datacenter nebo vyšší bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="34889-112">Use anyOf toomodify hello preceding policy tooallow any Windows Server 2012 R2 Datacenter or higher image:</span></span>

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

<span data-ttu-id="34889-113">Informace o polí zásad najdete v tématu [zásad aliasy](../../azure-resource-manager/resource-manager-policy.md#aliases).</span><span class="sxs-lookup"><span data-stu-id="34889-113">For information about policy fields, see [Policy aliases](../../azure-resource-manager/resource-manager-policy.md#aliases).</span></span>

## <a name="managed-disks"></a><span data-ttu-id="34889-114">Managed Disks</span><span class="sxs-lookup"><span data-stu-id="34889-114">Managed disks</span></span>

<span data-ttu-id="34889-115">toorequire hello použití spravovaných disků, použijte hello následující zásady:</span><span class="sxs-lookup"><span data-stu-id="34889-115">toorequire hello use of managed disks, use hello following policy:</span></span>

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

## <a name="images-for-virtual-machines"></a><span data-ttu-id="34889-116">Bitové kopie u virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="34889-116">Images for Virtual Machines</span></span>

<span data-ttu-id="34889-117">Z bezpečnostních důvodů se může vyžadovat, aby byly nasazené schválené vlastní Image ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="34889-117">For security reasons, you can require that only approved custom images are deployed in your environment.</span></span> <span data-ttu-id="34889-118">Můžete zadat buď hello skupinu prostředků, která obsahuje hello schválení bitových kopií, nebo konkrétní Image schválené hello.</span><span class="sxs-lookup"><span data-stu-id="34889-118">You can specify either hello resource group that contains hello approved images, or hello specific approved images.</span></span>

<span data-ttu-id="34889-119">Následující ukázka Hello vyžaduje bitové kopie z skupiny schválené prostředků:</span><span class="sxs-lookup"><span data-stu-id="34889-119">hello following example requires images from an approved resource group:</span></span>

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

<span data-ttu-id="34889-120">Hello následující příklad určuje hello schválení image ID:</span><span class="sxs-lookup"><span data-stu-id="34889-120">hello following example specifies hello approved image IDs:</span></span>

```json
{
    "field": "Microsoft.Compute/imageId",
    "in": ["{imageId1}","{imageId2}"]
}
```

## <a name="virtual-machine-extensions"></a><span data-ttu-id="34889-121">Rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="34889-121">Virtual Machine extensions</span></span>

<span data-ttu-id="34889-122">Můžete chtít tooforbid využití určitých typů rozšíření.</span><span class="sxs-lookup"><span data-stu-id="34889-122">You may want tooforbid usage of certain types of extensions.</span></span> <span data-ttu-id="34889-123">Například rozšíření pravděpodobně není kompatibilní s obrázky určité vlastního virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="34889-123">For example, an extension may not be compatible with certain custom virtual machine images.</span></span> <span data-ttu-id="34889-124">Následující příklad ukazuje, jak Hello tooblock konkrétní rozšíření.</span><span class="sxs-lookup"><span data-stu-id="34889-124">hello following example shows how tooblock a specific extension.</span></span> <span data-ttu-id="34889-125">Vydavatel a typ toodetermine používá které tooblock rozšíření.</span><span class="sxs-lookup"><span data-stu-id="34889-125">It uses publisher and type toodetermine which extension tooblock.</span></span>

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


## <a name="azure-hybrid-use-benefit"></a><span data-ttu-id="34889-126">Výhody použití Azure hybridní</span><span class="sxs-lookup"><span data-stu-id="34889-126">Azure Hybrid Use Benefit</span></span>

<span data-ttu-id="34889-127">Pokud máte licenci místní, můžete uložit poplatek hello licence na virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="34889-127">When you have an on-premise license, you can save hello license fee on your virtual machines.</span></span> <span data-ttu-id="34889-128">Pokud nemáte licenci hello, by měl nezakazuje možnost hello.</span><span class="sxs-lookup"><span data-stu-id="34889-128">When you don't have hello license, you should forbid hello option.</span></span> <span data-ttu-id="34889-129">Hello následující zásady zakazuje použití Azure hybridní použití zvýhodnění (AHUB):</span><span class="sxs-lookup"><span data-stu-id="34889-129">hello following policy forbids usage of Azure Hybrid Use Benefit (AHUB):</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="34889-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="34889-130">Next steps</span></span>
* <span data-ttu-id="34889-131">Po definování zásad pravidlo (jak je znázorněno v předchozích příkladech hello), budete potřebovat definice zásady hello toocreate a přiřaďte ho tooa oboru.</span><span class="sxs-lookup"><span data-stu-id="34889-131">After defining a policy rule (as shown in hello preceding examples), you need toocreate hello policy definition and assign it tooa scope.</span></span> <span data-ttu-id="34889-132">obor Hello může být předplatné, skupinu prostředků nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="34889-132">hello scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="34889-133">v tématu Zásady tooassign prostřednictvím portálu hello [Azure pomocí portálu tooassign a spravovat zásady prostředků](../../azure-resource-manager/resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="34889-133">tooassign policies through hello portal, see [Use Azure portal tooassign and manage resource policies](../../azure-resource-manager/resource-manager-policy-portal.md).</span></span> <span data-ttu-id="34889-134">zásady tooassign prostřednictvím REST API, Powershellu nebo příkazového řádku Azure CLI, najdete v části [přiřadit a spravovat zásady prostřednictvím skriptu](../../azure-resource-manager/resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="34889-134">tooassign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](../../azure-resource-manager/resource-manager-policy-create-assign.md).</span></span>
* <span data-ttu-id="34889-135">Úvod tooresource zásady, najdete v části [přehled zásad prostředků](../../azure-resource-manager/resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="34889-135">For an introduction tooresource policies, see [Resource policy overview](../../azure-resource-manager/resource-manager-policy.md).</span></span>
* <span data-ttu-id="34889-136">Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](../../azure-resource-manager/resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="34889-136">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](../../azure-resource-manager/resource-manager-subscription-governance.md).</span></span>
