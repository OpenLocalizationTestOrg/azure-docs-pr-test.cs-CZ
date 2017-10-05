---
title: "Analyzovat zabezpečení sítě s Azure sítě sledovacích procesů zabezpečení skupiny zobrazení – 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje postup použití Azure CLI 1.0 analyzovat zabezpečení virtuálních počítačů s zobrazení skupiny zabezpečení."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: a986ff4f-7e0c-4994-95e1-4ac824986500
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 2c4c494dcc4fe1a85c5feb29506c35fb03066479
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-azure-cli-10"></a><span data-ttu-id="05d38-103">Analýza zabezpečení vašeho virtuálního počítače s zobrazení skupiny zabezpečení pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="05d38-103">Analyze your Virtual Machine security with Security Group View using Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="05d38-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="05d38-104">PowerShell</span></span>](network-watcher-security-group-view-powershell.md)
> - [<span data-ttu-id="05d38-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="05d38-105">CLI 1.0</span></span>](network-watcher-security-group-view-cli-nodejs.md)
> - [<span data-ttu-id="05d38-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="05d38-106">CLI 2.0</span></span>](network-watcher-security-group-view-cli.md)
> - [<span data-ttu-id="05d38-107">REST API</span><span class="sxs-lookup"><span data-stu-id="05d38-107">REST API</span></span>](network-watcher-security-group-view-rest.md)

<span data-ttu-id="05d38-108">Zobrazení skupiny zabezpečení vrátí pravidla zabezpečení sítě nakonfigurované a efektivní, která se použijí k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="05d38-108">Security group view returns configured and effective network security rules that are applied to a virtual machine.</span></span> <span data-ttu-id="05d38-109">Tato možnost je užitečná k auditování a diagnostice skupin zabezpečení sítě a pravidel, které jsou nakonfigurované na virtuálním počítači zajistit provoz se správně povolený nebo zakázaný.</span><span class="sxs-lookup"><span data-stu-id="05d38-109">This capability is useful to audit and diagnose Network Security Groups and rules that are configured on a VM to ensure traffic is being correctly allowed or denied.</span></span> <span data-ttu-id="05d38-110">V tomto článku jsme ukazují, jak načíst pravidla zabezpečení nakonfigurované a efektivní k virtuálnímu počítači pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="05d38-110">In this article, we show you how to retrieve the configured and effective security rules to a virtual machine using Azure CLI</span></span>

<span data-ttu-id="05d38-111">Tento článek používá 1.0 rozhraní příkazového řádku Azure a platformy, která je dostupná pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="05d38-111">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="05d38-112">Sledovací proces sítě aktuálně používá pro podporu rozhraní příkazového řádku Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="05d38-112">Network Watcher currently uses Azure CLI 1.0 for CLI support.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="05d38-113">Než začnete</span><span class="sxs-lookup"><span data-stu-id="05d38-113">Before you begin</span></span>

<span data-ttu-id="05d38-114">Tento scénář předpokládá, že už jste udělali kroky v [vytvořit sledovací proces sítě](network-watcher-create.md) vytvořit sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="05d38-114">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="05d38-115">Scénář</span><span class="sxs-lookup"><span data-stu-id="05d38-115">Scenario</span></span>

<span data-ttu-id="05d38-116">Scénář popsaná v tomto článku načte pravidla nakonfigurované a efektivní zabezpečení pro daný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="05d38-116">The scenario covered in this article retrieves the configured and effective security rules for a given virtual machine.</span></span>

## <a name="get-a-vm"></a><span data-ttu-id="05d38-117">Získat virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="05d38-117">Get a VM</span></span>

<span data-ttu-id="05d38-118">Virtuální počítač je potřeba spustit `vm list` rutiny.</span><span class="sxs-lookup"><span data-stu-id="05d38-118">A virtual machine is required to run the `vm list` cmdlet.</span></span> <span data-ttu-id="05d38-119">Následující příkaz zobrazí virtuální machinese ve skupině prostředků:</span><span class="sxs-lookup"><span data-stu-id="05d38-119">The following command lists the virtual machinese in a resource group:</span></span>

```azurecli
azure vm list -g resourceGroupName
```

<span data-ttu-id="05d38-120">Jakmile znáte virtuální počítač, můžete použít `vm show` rutiny jeho Id prostředku:</span><span class="sxs-lookup"><span data-stu-id="05d38-120">Once you know the virtual machine, you can use the `vm show` cmdlet to get its resource Id:</span></span>

```azurecli
azure vm show -g resourceGroupName -n virtualMachineName
```

## <a name="retrieve-security-group-view"></a><span data-ttu-id="05d38-121">Načtení zobrazení skupiny zabezpečení</span><span class="sxs-lookup"><span data-stu-id="05d38-121">Retrieve security group view</span></span>

<span data-ttu-id="05d38-122">Dalším krokem je načíst výsledky zobrazení skupiny zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="05d38-122">The next step is to retrieve the security group view result.</span></span> <span data-ttu-id="05d38-123">Přidávání "--json" příznak naformátuje výsledky ve formátu json.</span><span class="sxs-lookup"><span data-stu-id="05d38-123">Adding the "--json" flag will format the results in json.</span></span>

```azurecli
azure network watcher security-group-view -g resourceGroupName -n networkWatcherName -t targetResourceId --json
```

## <a name="viewing-the-results"></a><span data-ttu-id="05d38-124">Zobrazení výsledků</span><span class="sxs-lookup"><span data-stu-id="05d38-124">Viewing the results</span></span>

<span data-ttu-id="05d38-125">V následujícím příkladu je zkrácení odpověď výsledky vrácené.</span><span class="sxs-lookup"><span data-stu-id="05d38-125">The following example is a shortened response of the results returned.</span></span> <span data-ttu-id="05d38-126">Výsledky zobrazit všechna pravidla zabezpečení efektivní a použitých na virtuálním počítači rozdělení v skupiny **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, a **EffectiveSecurityRules**.</span><span class="sxs-lookup"><span data-stu-id="05d38-126">The results show all the effective and applied security rules on the virtual machine broken down in groups of **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, and **EffectiveSecurityRules**.</span></span>

```json
{
  "networkInterfaces": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testnic",
      "securityRuleAssociations": {
        "networkInterfaceAssociation": {
          "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testvm",
          "securityRules": [
            {
              "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/test-nsg/securityRules/default-allow-rdp",
              "protocol": "TCP",
              "sourcePortRange": "*",
              "destinationPortRange": "3389",
              "sourceAddressPrefix": "*",
              "destinationAddressPrefix": "*",
              "access": "Allow",
              "priority": 1000,
              "direction": "Inbound",
              "provisioningState": "Succeeded",
              "name": "default-allow-rdp",
              "etag": "W/\"00000000-0000-0000-0000-000000000000\""
            }
          ]
        },
        "defaultSecurityRules": [
          {
            "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/networkSecurityGroups//defaultSecurityRules/",
            "description": "Allow inbound traffic from all VMs in VNET",
            "protocol": "*",
            "sourcePortRange": "*",
            "destinationPortRange": "*",
            "sourceAddressPrefix": "VirtualNetwork",
            "destinationAddressPrefix": "VirtualNetwork",
            "access": "Allow",
            "priority": 65000,
            "direction": "Inbound",
            "provisioningState": "Succeeded",
            "name": "AllowVnetInBound"
          }
        ]
      }
    }
  ]
}
```

## <a name="next-steps"></a><span data-ttu-id="05d38-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="05d38-127">Next steps</span></span>

<span data-ttu-id="05d38-128">Navštivte [auditování zabezpečení sítě skupiny (NSG) s sledovací proces sítě](network-watcher-nsg-auditing-powershell.md) se dozvíte, jak automatizovat ověření skupin zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="05d38-128">Visit [Auditing Network Security Groups (NSG) with Network Watcher](network-watcher-nsg-auditing-powershell.md) to learn how to automate validation of Network Security Groups.</span></span>

<span data-ttu-id="05d38-129">Další informace o zabezpečení pravidla, která se použijí k síťovým prostředkům, navštivte stránky [přehled zobrazení skupiny zabezpečení](network-watcher-security-group-view-overview.md)</span><span class="sxs-lookup"><span data-stu-id="05d38-129">Learn more about the security rules that are applied to your network resources by visiting [Security group view overview](network-watcher-security-group-view-overview.md)</span></span>
