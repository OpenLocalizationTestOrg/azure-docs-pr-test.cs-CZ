---
title: "zabezpečení sítě aaaAnalyze s Azure sítě sledovacích procesů zabezpečení skupiny zobrazení – 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje, jak tooanalyze toouse 2.0 rozhraní příkazového řádku Azure a virtuálních počítačů zabezpečení s zobrazení skupiny zabezpečení."
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
ms.openlocfilehash: 31a4cd628f54d7548f495251fd275f099e79a060
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-azure-cli-20"></a><span data-ttu-id="c072a-103">Analýza zabezpečení vašeho virtuálního počítače s zobrazení skupiny zabezpečení pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="c072a-103">Analyze your Virtual Machine security with Security Group View using Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="c072a-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c072a-104">PowerShell</span></span>](network-watcher-security-group-view-powershell.md)
> - [<span data-ttu-id="c072a-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="c072a-105">CLI 1.0</span></span>](network-watcher-security-group-view-cli-nodejs.md)
> - [<span data-ttu-id="c072a-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="c072a-106">CLI 2.0</span></span>](network-watcher-security-group-view-cli.md)
> - [<span data-ttu-id="c072a-107">REST API</span><span class="sxs-lookup"><span data-stu-id="c072a-107">REST API</span></span>](network-watcher-security-group-view-rest.md)

<span data-ttu-id="c072a-108">Zobrazení skupiny zabezpečení vrátí pravidla zabezpečení sítě nakonfigurované a efektivní, které jsou použité tooa virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c072a-108">Security group view returns configured and effective network security rules that are applied tooa virtual machine.</span></span> <span data-ttu-id="c072a-109">Tato funkce je užitečná tooaudit a diagnostikovat skupin zabezpečení sítě, a pravidla, které jsou nakonfigurované na přenosem tooensure virtuálních počítačů se správně povolený nebo zakázaný.</span><span class="sxs-lookup"><span data-stu-id="c072a-109">This capability is useful tooaudit and diagnose Network Security Groups and rules that are configured on a VM tooensure traffic is being correctly allowed or denied.</span></span> <span data-ttu-id="c072a-110">V tomto článku jsme ukážeme, jak tooretrieve hello nakonfigurované a efektivní zabezpečení pravidla tooa virtuálního počítače pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="c072a-110">In this article, we show you how tooretrieve hello configured and effective security rules tooa virtual machine using Azure CLI</span></span>


<span data-ttu-id="c072a-111">Tento článek používá naší nové generace rozhraní příkazového řádku pro model nasazení hello prostředků management, Azure CLI 2.0, která je dostupná pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="c072a-111">This article uses our next generation CLI for hello resource management deployment model, Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="c072a-112">tooperform hello kroky v tomto článku, budete potřebovat příliš[instalace hello rozhraní příkazového řádku Azure pro Mac, Linux a Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="c072a-112">tooperform hello steps in this article, you need too[install hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="c072a-113">Než začnete</span><span class="sxs-lookup"><span data-stu-id="c072a-113">Before you begin</span></span>

<span data-ttu-id="c072a-114">Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="c072a-114">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="c072a-115">Scénář</span><span class="sxs-lookup"><span data-stu-id="c072a-115">Scenario</span></span>

<span data-ttu-id="c072a-116">scénář Hello popsaná v tomto článku načte hello nakonfigurované a pravidla efektivní zabezpečení pro daný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="c072a-116">hello scenario covered in this article retrieves hello configured and effective security rules for a given virtual machine.</span></span>

## <a name="get-a-vm"></a><span data-ttu-id="c072a-117">Získat virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="c072a-117">Get a VM</span></span>

<span data-ttu-id="c072a-118">Virtuální počítač je požadovaná toorun hello `vm list` rutiny.</span><span class="sxs-lookup"><span data-stu-id="c072a-118">A virtual machine is required toorun hello `vm list` cmdlet.</span></span> <span data-ttu-id="c072a-119">Hello následující příkaz vypíše hello virtuální počítače ve skupině prostředků:</span><span class="sxs-lookup"><span data-stu-id="c072a-119">hello following command lists hello virtual machines in a resource group:</span></span>

```azurecli
az vm list -resource-group resourceGroupName
```

<span data-ttu-id="c072a-120">Jakmile znáte hello virtuálního počítače, můžete použít hello `vm show` rutiny tooget jeho Id prostředku:</span><span class="sxs-lookup"><span data-stu-id="c072a-120">Once you know hello virtual machine, you can use hello `vm show` cmdlet tooget its resource Id:</span></span>

```azurecli
az vm show -resource-group resourceGroupName -name virtualMachineName
```

## <a name="retrieve-security-group-view"></a><span data-ttu-id="c072a-121">Načtení zobrazení skupiny zabezpečení</span><span class="sxs-lookup"><span data-stu-id="c072a-121">Retrieve security group view</span></span>

<span data-ttu-id="c072a-122">dalším krokem Hello je tooretrieve hello zabezpečení skupiny zobrazení výsledků.</span><span class="sxs-lookup"><span data-stu-id="c072a-122">hello next step is tooretrieve hello security group view result.</span></span>

```azurecli
az network watcher show-security-group-view --resource-group resourceGroupName --vm vmName
```

## <a name="viewing-hello-results"></a><span data-ttu-id="c072a-123">Zobrazení výsledků hello</span><span class="sxs-lookup"><span data-stu-id="c072a-123">Viewing hello results</span></span>

<span data-ttu-id="c072a-124">Hello následující příklad je zkrácení odpověď vráceny výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="c072a-124">hello following example is a shortened response of hello results returned.</span></span> <span data-ttu-id="c072a-125">Hello výsledky zobrazit všechna pravidla zabezpečení efektivní a použitých hello na virtuálním počítači hello rozdělení v skupiny **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, a  **EffectiveSecurityRules**.</span><span class="sxs-lookup"><span data-stu-id="c072a-125">hello results show all hello effective and applied security rules on hello virtual machine broken down in groups of **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, and **EffectiveSecurityRules**.</span></span>

```json
{
  "networkInterfaces": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkInterfaces/{nicName}",
      "resourceGroup": "{resourceGroupName}",
      "securityRuleAssociations": {
        "defaultSecurityRules": [
          {
            "access": "Allow",
            "description": "Allow inbound traffic from all VMs in VNET",
            "destinationAddressPrefix": "VirtualNetwork",
            "destinationPortRange": "*",
            "direction": "Inbound",
            "etag": null,
            "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups//providers/Microsoft.Network/networkSecurityGroups/{nsgName}/defaultSecurityRules/AllowVnetInBound",
            "name": "AllowVnetInBound",
            "priority": 65000,
            "protocol": "*",
            "provisioningState": "Succeeded",
            "resourceGroup": "",
            "sourceAddressPrefix": "VirtualNetwork",
            "sourcePortRange": "*"
          }...
        ],
        "effectiveSecurityRules": [
          {
            "access": "Deny",
            "destinationAddressPrefix": "*",
            "destinationPortRange": "0-65535",
            "direction": "Outbound",
            "expandedDestinationAddressPrefix": null,
            "expandedSourceAddressPrefix": null,
            "name": "DefaultOutboundDenyAll",
            "priority": 65500,
            "protocol": "All",
            "sourceAddressPrefix": "*",
            "sourcePortRange": "0-65535"
          },
          {
            "access": "Allow",
            "destinationAddressPrefix": "VirtualNetwork",
            "destinationPortRange": "0-65535",
            "direction": "Outbound",
            "expandedDestinationAddressPrefix": [
              "10.1.0.0/24",
              "168.63.129.16/32"
            ],
            "expandedSourceAddressPrefix": [
              "10.1.0.0/24",
              "168.63.129.16/32"
            ],
            "name": "DefaultRule_AllowVnetOutBound",
            "priority": 65000,
            "protocol": "All",
            "sourceAddressPrefix": "VirtualNetwork",
            "sourcePortRange": "0-65535"
          },...
        ],
        "networkInterfaceAssociation": {
          "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkInterfaces/{nicName}",
          "resourceGroup": "{resourceGroupName}",
          "securityRules": [
            {
              "access": "Allow",
              "description": null,
              "destinationAddressPrefix": "*",
              "destinationPortRange": "3389",
              "direction": "Inbound",
              "etag": "W/\"efb606c1-2d54-475a-ab20-da3f80393577\"",
              "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}/securityRules/default-allow-rdp",
              "name": "default-allow-rdp",
              "priority": 1000,
              "protocol": "TCP",
              "provisioningState": "Succeeded",
              "resourceGroup": "{resourceGroupName}",
              "sourceAddressPrefix": "*",
              "sourcePortRange": "*"
            }
          ]
        },
        "subnetAssociation": null
      }
    }
  ]
}
```

## <a name="next-steps"></a><span data-ttu-id="c072a-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c072a-126">Next steps</span></span>

<span data-ttu-id="c072a-127">Navštivte [auditování zabezpečení sítě skupiny (NSG) s sledovací proces sítě](network-watcher-nsg-auditing-powershell.md) toolearn jak tooautomate ověření skupin zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="c072a-127">Visit [Auditing Network Security Groups (NSG) with Network Watcher](network-watcher-nsg-auditing-powershell.md) toolearn how tooautomate validation of Network Security Groups.</span></span>

<span data-ttu-id="c072a-128">Další informace o pravidlech hello zabezpečení, které jsou použité tooyour síťovým prostředkům, navštivte stránky [přehled zobrazení skupiny zabezpečení](network-watcher-security-group-view-overview.md)</span><span class="sxs-lookup"><span data-stu-id="c072a-128">Learn more about hello security rules that are applied tooyour network resources by visiting [Security group view overview](network-watcher-security-group-view-overview.md)</span></span>
