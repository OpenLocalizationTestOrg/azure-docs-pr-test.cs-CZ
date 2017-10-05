---
title: "Vynutit zabezpečení se zásadami na virtuálních počítačích Windows v Azure | Microsoft Docs"
description: "Tom, jak používat zásady pro správce prostředků Windows virtuální počítač Azure"
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
ms.openlocfilehash: 246f5958478fd6d9afc9ba990413ab08429bd25d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="apply-policies-to-windows-vms-with-azure-resource-manager"></a><span data-ttu-id="522f9-103">Použití zásad u virtuálních počítačů s Windows pomocí Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="522f9-103">Apply policies to Windows VMs with Azure Resource Manager</span></span>
<span data-ttu-id="522f9-104">Pomocí zásad můžete vynutit organizaci různé konvence a pravidla v rámci podniku.</span><span class="sxs-lookup"><span data-stu-id="522f9-104">By using policies, an organization can enforce various conventions and rules throughout the enterprise.</span></span> <span data-ttu-id="522f9-105">Vynucení požadované chování může pomoci zmírnit rizika při přispívání do úspěch organizace.</span><span class="sxs-lookup"><span data-stu-id="522f9-105">Enforcement of the desired behavior can help mitigate risk while contributing to the success of the organization.</span></span> <span data-ttu-id="522f9-106">V tomto článku jsme popisují, jak lze pomocí Azure Resource Manager zásad můžete určit požadované chování pro virtuální počítače vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="522f9-106">In this article, we describe how you can use Azure Resource Manager policies to define the desired behavior for your organization’s Virtual Machines.</span></span>

<span data-ttu-id="522f9-107">Úvod do zásady, najdete v části [použití zásad ke správě prostředků a řízení přístupu](../../azure-resource-manager/resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="522f9-107">For an introduction to policies, see [Use Policy to manage resources and control access](../../azure-resource-manager/resource-manager-policy.md).</span></span>

## <a name="permitted-virtual-machines"></a><span data-ttu-id="522f9-108">Povolené virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="522f9-108">Permitted Virtual Machines</span></span>
<span data-ttu-id="522f9-109">Zajistit, že virtuální počítače ve vaší organizaci, jsou kompatibilní s aplikací, můžete omezit povolených operační systémy.</span><span class="sxs-lookup"><span data-stu-id="522f9-109">To ensure that virtual machines for your organization are compatible with an application, you can restrict the permitted operating systems.</span></span> <span data-ttu-id="522f9-110">V následujícím příkladu zásad můžete povolit jenom Windows serveru 2012 R2 Datacenter virtuálních počítačů, který se má vytvořit:</span><span class="sxs-lookup"><span data-stu-id="522f9-110">In the following policy example, you allow only Windows Server 2012 R2 Datacenter Virtual Machines to be created:</span></span>

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

<span data-ttu-id="522f9-111">K úpravě předchozí zásada umožnit všechny bitové kopie systému Windows Server Datacenter použijte zástupný znak:</span><span class="sxs-lookup"><span data-stu-id="522f9-111">Use a wild card to modify the preceding policy to allow any Windows Server Datacenter image:</span></span>

```json
{
  "field": "Microsoft.Compute/imageSku",
  "like": "*Datacenter"
}
```

<span data-ttu-id="522f9-112">K úpravě předchozí zásada umožnit žádné Windows Server 2012 R2 Datacenter nebo vyšší bitové kopie použijte anyOf:</span><span class="sxs-lookup"><span data-stu-id="522f9-112">Use anyOf to modify the preceding policy to allow any Windows Server 2012 R2 Datacenter or higher image:</span></span>

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

<span data-ttu-id="522f9-113">Informace o polí zásad najdete v tématu [zásad aliasy](../../azure-resource-manager/resource-manager-policy.md#aliases).</span><span class="sxs-lookup"><span data-stu-id="522f9-113">For information about policy fields, see [Policy aliases](../../azure-resource-manager/resource-manager-policy.md#aliases).</span></span>

## <a name="managed-disks"></a><span data-ttu-id="522f9-114">Managed Disks</span><span class="sxs-lookup"><span data-stu-id="522f9-114">Managed disks</span></span>

<span data-ttu-id="522f9-115">Pokud chcete vyžadovat použití spravovaných disků, použijte tyto zásady:</span><span class="sxs-lookup"><span data-stu-id="522f9-115">To require the use of managed disks, use the following policy:</span></span>

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

## <a name="images-for-virtual-machines"></a><span data-ttu-id="522f9-116">Bitové kopie u virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="522f9-116">Images for Virtual Machines</span></span>

<span data-ttu-id="522f9-117">Z bezpečnostních důvodů se může vyžadovat, aby byly nasazené schválené vlastní Image ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="522f9-117">For security reasons, you can require that only approved custom images are deployed in your environment.</span></span> <span data-ttu-id="522f9-118">Můžete zadat buď skupinu prostředků, která obsahuje Image schválené nebo konkrétní schválených bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="522f9-118">You can specify either the resource group that contains the approved images, or the specific approved images.</span></span>

<span data-ttu-id="522f9-119">Následující příklad vyžaduje bitové kopie z skupiny schválené prostředků:</span><span class="sxs-lookup"><span data-stu-id="522f9-119">The following example requires images from an approved resource group:</span></span>

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

<span data-ttu-id="522f9-120">Následující příklad určuje ID schválené bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="522f9-120">The following example specifies the approved image IDs:</span></span>

```json
{
    "field": "Microsoft.Compute/imageId",
    "in": ["{imageId1}","{imageId2}"]
}
```

## <a name="virtual-machine-extensions"></a><span data-ttu-id="522f9-121">Rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="522f9-121">Virtual Machine extensions</span></span>

<span data-ttu-id="522f9-122">Můžete chtít nezakazuje využití určitých typů rozšíření.</span><span class="sxs-lookup"><span data-stu-id="522f9-122">You may want to forbid usage of certain types of extensions.</span></span> <span data-ttu-id="522f9-123">Například rozšíření pravděpodobně není kompatibilní s obrázky určité vlastního virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="522f9-123">For example, an extension may not be compatible with certain custom virtual machine images.</span></span> <span data-ttu-id="522f9-124">Následující příklad ukazuje, jak chcete blokovat konkrétní rozšíření.</span><span class="sxs-lookup"><span data-stu-id="522f9-124">The following example shows how to block a specific extension.</span></span> <span data-ttu-id="522f9-125">Pomocí vydavatele a typ určit, které rozšíření, které chcete blokovat.</span><span class="sxs-lookup"><span data-stu-id="522f9-125">It uses publisher and type to determine which extension to block.</span></span>

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


## <a name="azure-hybrid-use-benefit"></a><span data-ttu-id="522f9-126">Výhody použití Azure hybridní</span><span class="sxs-lookup"><span data-stu-id="522f9-126">Azure Hybrid Use Benefit</span></span>

<span data-ttu-id="522f9-127">Pokud máte licenci místní, můžete uložit poplatek licence na virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="522f9-127">When you have an on-premise license, you can save the license fee on your virtual machines.</span></span> <span data-ttu-id="522f9-128">Pokud nemáte licenci, by měl nezakazuje možnost.</span><span class="sxs-lookup"><span data-stu-id="522f9-128">When you don't have the license, you should forbid the option.</span></span> <span data-ttu-id="522f9-129">Tyto zásady zakazuje použití Azure hybridní použití zvýhodnění (AHUB):</span><span class="sxs-lookup"><span data-stu-id="522f9-129">The following policy forbids usage of Azure Hybrid Use Benefit (AHUB):</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="522f9-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="522f9-130">Next steps</span></span>
* <span data-ttu-id="522f9-131">Po definování zásad pravidlo (jak je znázorněno v předchozích ukázkách), musíte k vytvoření definice zásady a přiřadit obor.</span><span class="sxs-lookup"><span data-stu-id="522f9-131">After defining a policy rule (as shown in the preceding examples), you need to create the policy definition and assign it to a scope.</span></span> <span data-ttu-id="522f9-132">Obor může být předplatné, skupinu prostředků nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="522f9-132">The scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="522f9-133">K přiřazení zásad prostřednictvím portálu, najdete v části [portálu Azure použijte přiřadit a spravovat zásady prostředků](../../azure-resource-manager/resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="522f9-133">To assign policies through the portal, see [Use Azure portal to assign and manage resource policies](../../azure-resource-manager/resource-manager-policy-portal.md).</span></span> <span data-ttu-id="522f9-134">K přiřazení zásad pomocí rozhraní REST API, Powershellu nebo příkazového řádku Azure CLI, najdete v části [přiřadit a spravovat zásady prostřednictvím skriptu](../../azure-resource-manager/resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="522f9-134">To assign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](../../azure-resource-manager/resource-manager-policy-create-assign.md).</span></span>
* <span data-ttu-id="522f9-135">Úvod do zásad prostředků, najdete v části [přehled zásad prostředků](../../azure-resource-manager/resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="522f9-135">For an introduction to resource policies, see [Resource policy overview](../../azure-resource-manager/resource-manager-policy.md).</span></span>
* <span data-ttu-id="522f9-136">Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](../../azure-resource-manager/resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="522f9-136">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](../../azure-resource-manager/resource-manager-subscription-governance.md).</span></span>
