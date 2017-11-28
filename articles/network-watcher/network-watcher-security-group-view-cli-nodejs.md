---
title: "zabezpečení sítě aaaAnalyze s Azure sítě sledovacích procesů zabezpečení skupiny zobrazení – 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje, jak tooanalyze toouse 1.0 rozhraní příkazového řádku Azure a virtuálních počítačů zabezpečení s zobrazení skupiny zabezpečení."
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
ms.openlocfilehash: 96383a734b94d215d5b0f3d47339e46940d700b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-azure-cli-10"></a><span data-ttu-id="a5f7b-103">Analýza zabezpečení vašeho virtuálního počítače s zobrazení skupiny zabezpečení pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="a5f7b-103">Analyze your Virtual Machine security with Security Group View using Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="a5f7b-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a5f7b-104">PowerShell</span></span>](network-watcher-security-group-view-powershell.md)
> - [<span data-ttu-id="a5f7b-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="a5f7b-105">CLI 1.0</span></span>](network-watcher-security-group-view-cli-nodejs.md)
> - [<span data-ttu-id="a5f7b-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="a5f7b-106">CLI 2.0</span></span>](network-watcher-security-group-view-cli.md)
> - [<span data-ttu-id="a5f7b-107">REST API</span><span class="sxs-lookup"><span data-stu-id="a5f7b-107">REST API</span></span>](network-watcher-security-group-view-rest.md)

<span data-ttu-id="a5f7b-108">Zobrazení skupiny zabezpečení vrátí pravidla zabezpečení sítě nakonfigurované a efektivní, které jsou použité tooa virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-108">Security group view returns configured and effective network security rules that are applied tooa virtual machine.</span></span> <span data-ttu-id="a5f7b-109">Tato funkce je užitečná tooaudit a diagnostikovat skupin zabezpečení sítě, a pravidla, které jsou nakonfigurované na přenosem tooensure virtuálních počítačů se správně povolený nebo zakázaný.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-109">This capability is useful tooaudit and diagnose Network Security Groups and rules that are configured on a VM tooensure traffic is being correctly allowed or denied.</span></span> <span data-ttu-id="a5f7b-110">V tomto článku jsme ukážeme, jak tooretrieve hello nakonfigurované a efektivní zabezpečení pravidla tooa virtuálního počítače pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="a5f7b-110">In this article, we show you how tooretrieve hello configured and effective security rules tooa virtual machine using Azure CLI</span></span>

<span data-ttu-id="a5f7b-111">Tento článek používá 1.0 rozhraní příkazového řádku Azure a platformy, která je dostupná pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-111">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="a5f7b-112">Sledovací proces sítě aktuálně používá pro podporu rozhraní příkazového řádku Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-112">Network Watcher currently uses Azure CLI 1.0 for CLI support.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="a5f7b-113">Než začnete</span><span class="sxs-lookup"><span data-stu-id="a5f7b-113">Before you begin</span></span>

<span data-ttu-id="a5f7b-114">Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-114">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="a5f7b-115">Scénář</span><span class="sxs-lookup"><span data-stu-id="a5f7b-115">Scenario</span></span>

<span data-ttu-id="a5f7b-116">scénář Hello popsaná v tomto článku načte hello nakonfigurované a pravidla efektivní zabezpečení pro daný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-116">hello scenario covered in this article retrieves hello configured and effective security rules for a given virtual machine.</span></span>

## <a name="get-a-vm"></a><span data-ttu-id="a5f7b-117">Získat virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="a5f7b-117">Get a VM</span></span>

<span data-ttu-id="a5f7b-118">Virtuální počítač je požadovaná toorun hello `vm list` rutiny.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-118">A virtual machine is required toorun hello `vm list` cmdlet.</span></span> <span data-ttu-id="a5f7b-119">Hello následující příkaz vypíše hello virtuální machinese ve skupině prostředků:</span><span class="sxs-lookup"><span data-stu-id="a5f7b-119">hello following command lists hello virtual machinese in a resource group:</span></span>

```azurecli
azure vm list -g resourceGroupName
```

<span data-ttu-id="a5f7b-120">Jakmile znáte hello virtuálního počítače, můžete použít hello `vm show` rutiny tooget jeho Id prostředku:</span><span class="sxs-lookup"><span data-stu-id="a5f7b-120">Once you know hello virtual machine, you can use hello `vm show` cmdlet tooget its resource Id:</span></span>

```azurecli
azure vm show -g resourceGroupName -n virtualMachineName
```

## <a name="retrieve-security-group-view"></a><span data-ttu-id="a5f7b-121">Načtení zobrazení skupiny zabezpečení</span><span class="sxs-lookup"><span data-stu-id="a5f7b-121">Retrieve security group view</span></span>

<span data-ttu-id="a5f7b-122">dalším krokem Hello je tooretrieve hello zabezpečení skupiny zobrazení výsledků.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-122">hello next step is tooretrieve hello security group view result.</span></span> <span data-ttu-id="a5f7b-123">Přidání hello "--json" příznak naformátuje výsledky hello ve formátu json.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-123">Adding hello "--json" flag will format hello results in json.</span></span>

```azurecli
azure network watcher security-group-view -g resourceGroupName -n networkWatcherName -t targetResourceId --json
```

## <a name="viewing-hello-results"></a><span data-ttu-id="a5f7b-124">Zobrazení výsledků hello</span><span class="sxs-lookup"><span data-stu-id="a5f7b-124">Viewing hello results</span></span>

<span data-ttu-id="a5f7b-125">Hello následující příklad je zkrácení odpověď vráceny výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-125">hello following example is a shortened response of hello results returned.</span></span> <span data-ttu-id="a5f7b-126">Hello výsledky zobrazit všechna pravidla zabezpečení efektivní a použitých hello na virtuálním počítači hello rozdělení v skupiny **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, a  **EffectiveSecurityRules**.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-126">hello results show all hello effective and applied security rules on hello virtual machine broken down in groups of **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, and **EffectiveSecurityRules**.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="a5f7b-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a5f7b-127">Next steps</span></span>

<span data-ttu-id="a5f7b-128">Navštivte [auditování zabezpečení sítě skupiny (NSG) s sledovací proces sítě](network-watcher-nsg-auditing-powershell.md) toolearn jak tooautomate ověření skupin zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-128">Visit [Auditing Network Security Groups (NSG) with Network Watcher](network-watcher-nsg-auditing-powershell.md) toolearn how tooautomate validation of Network Security Groups.</span></span>

<span data-ttu-id="a5f7b-129">Další informace o pravidlech hello zabezpečení, které jsou použité tooyour síťovým prostředkům, navštivte stránky [přehled zobrazení skupiny zabezpečení](network-watcher-security-group-view-overview.md)</span><span class="sxs-lookup"><span data-stu-id="a5f7b-129">Learn more about hello security rules that are applied tooyour network resources by visiting [Security group view overview](network-watcher-security-group-view-overview.md)</span></span>
