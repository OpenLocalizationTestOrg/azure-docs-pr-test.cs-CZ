---
title: "zabezpečení sítě aaaAnalyze s Azure sítě sledovacích procesů zabezpečení skupiny zobrazení – prostředí PowerShell | Microsoft Docs"
description: "Tento článek popisuje, jak toouse tooanalyze prostředí PowerShell a virtuální počítače zabezpečení s zobrazení skupiny zabezpečení."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 04e76b49-6a1b-4d0f-9a9b-51cf2f4df5a2
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 5e1990d97899bd8585025ec13dd556ab2e034c3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-powershell"></a><span data-ttu-id="4f6b9-103">Analýza zabezpečení vašeho virtuálního počítače s zobrazení skupiny zabezpečení pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="4f6b9-103">Analyze your Virtual Machine security with Security Group View using PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="4f6b9-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4f6b9-104">PowerShell</span></span>](network-watcher-security-group-view-powershell.md)
> - [<span data-ttu-id="4f6b9-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="4f6b9-105">CLI 1.0</span></span>](network-watcher-security-group-view-cli-nodejs.md)
> - [<span data-ttu-id="4f6b9-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="4f6b9-106">CLI 2.0</span></span>](network-watcher-security-group-view-cli.md)
> - [<span data-ttu-id="4f6b9-107">REST API</span><span class="sxs-lookup"><span data-stu-id="4f6b9-107">REST API</span></span>](network-watcher-security-group-view-rest.md)

<span data-ttu-id="4f6b9-108">Zobrazení skupiny zabezpečení vrátí pravidla zabezpečení sítě nakonfigurované a efektivní, které jsou použité tooa virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4f6b9-108">Security group view returns configured and effective network security rules that are applied tooa virtual machine.</span></span> <span data-ttu-id="4f6b9-109">Tato funkce je užitečná tooaudit a diagnostikovat skupin zabezpečení sítě, a pravidla, které jsou nakonfigurované na přenosem tooensure virtuálních počítačů se správně povolený nebo zakázaný.</span><span class="sxs-lookup"><span data-stu-id="4f6b9-109">This capability is useful tooaudit and diagnose Network Security Groups and rules that are configured on a VM tooensure traffic is being correctly allowed or denied.</span></span> <span data-ttu-id="4f6b9-110">V tomto článku jsme ukážeme, jak tooretrieve hello nakonfigurované a efektivní zabezpečení pravidla tooa virtuálního počítače pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="4f6b9-110">In this article, we show you how tooretrieve hello configured and effective security rules tooa virtual machine using PowerShell</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="4f6b9-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="4f6b9-111">Before you begin</span></span>

<span data-ttu-id="4f6b9-112">V tomto scénáři spustíte hello `Get-AzureRmNetworkWatcherSecurityGroupView` rutiny tooretrieve hello zabezpečení informace o pravidle.</span><span class="sxs-lookup"><span data-stu-id="4f6b9-112">In this scenario, you run hello `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet tooretrieve hello security rule information.</span></span>

<span data-ttu-id="4f6b9-113">Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="4f6b9-113">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="4f6b9-114">Scénář</span><span class="sxs-lookup"><span data-stu-id="4f6b9-114">Scenario</span></span>

<span data-ttu-id="4f6b9-115">scénář Hello popsaná v tomto článku načte hello nakonfigurované a pravidla efektivní zabezpečení pro daný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="4f6b9-115">hello scenario covered in this article retrieves hello configured and effective security rules for a given virtual machine.</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="4f6b9-116">Načtení sledovací proces sítě</span><span class="sxs-lookup"><span data-stu-id="4f6b9-116">Retrieve Network Watcher</span></span>

<span data-ttu-id="4f6b9-117">prvním krokem Hello je tooretrieve hello sledovací proces sítě instance.</span><span class="sxs-lookup"><span data-stu-id="4f6b9-117">hello first step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="4f6b9-118">Tato proměnná je předán toohello `Get-AzureRmNetworkWatcherSecurityGroupView` rutiny.</span><span class="sxs-lookup"><span data-stu-id="4f6b9-118">This variable is passed toohello `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
```

## <a name="get-a-vm"></a><span data-ttu-id="4f6b9-119">Získat virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="4f6b9-119">Get a VM</span></span>

<span data-ttu-id="4f6b9-120">Virtuální počítač je požadovaná toorun hello `Get-AzureRmNetworkWatcherSecurityGroupView` rutiny proti.</span><span class="sxs-lookup"><span data-stu-id="4f6b9-120">A virtual machine is required toorun hello `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet against.</span></span> <span data-ttu-id="4f6b9-121">Následující ukázka Hello získá objektu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4f6b9-121">hello following example gets a VM object.</span></span>

```powershell
$VM = Get-AzurermVM -ResourceGroupName testrg -Name testvm1
```

## <a name="retrieve-security-group-view"></a><span data-ttu-id="4f6b9-122">Načtení zobrazení skupiny zabezpečení</span><span class="sxs-lookup"><span data-stu-id="4f6b9-122">Retrieve security group view</span></span>

<span data-ttu-id="4f6b9-123">dalším krokem Hello je tooretrieve hello zabezpečení skupiny zobrazení výsledků.</span><span class="sxs-lookup"><span data-stu-id="4f6b9-123">hello next step is tooretrieve hello security group view result.</span></span>

```powershell
$secgroup = Get-AzureRmNetworkWatcherSecurityGroupView -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id
```

## <a name="viewing-hello-results"></a><span data-ttu-id="4f6b9-124">Zobrazení výsledků hello</span><span class="sxs-lookup"><span data-stu-id="4f6b9-124">Viewing hello results</span></span>

<span data-ttu-id="4f6b9-125">Hello následující příklad je zkrácení odpověď vráceny výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="4f6b9-125">hello following example is a shortened response of hello results returned.</span></span> <span data-ttu-id="4f6b9-126">Hello výsledky zobrazit všechna pravidla zabezpečení efektivní a použitých hello na virtuálním počítači hello rozdělení v skupiny **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, a  **EffectiveSecurityRules**.</span><span class="sxs-lookup"><span data-stu-id="4f6b9-126">hello results show all hello effective and applied security rules on hello virtual machine broken down in groups of **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, and **EffectiveSecurityRules**.</span></span>

```
NetworkInterfaces : [
                      {
                        "NetworkInterfaceSecurityRules": [
                          {
                            "Name": "default-allow-rdp",
                            "Etag": "W/\"d4c411d4-0d62-49dc-8092-3d4b57825740\"",
                            "Id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg2/providers/Microsoft.Network/networkSecurityGroups/testvm2-nsg/securityRules/default-allow-rdp",
                            "Protocol": "TCP",
                            "SourcePortRange": "*",
                            "DestinationPortRange": "3389",
                            "SourceAddressPrefix": "*",
                            "DestinationAddressPrefix": "*",
                            "Access": "Allow",
                            "Priority": 1000,
                            "Direction": "Inbound",
                            "ProvisioningState": "Succeeded"
                          }
                          ...
                        ],
                        "DefaultSecurityRules": [
                          {
                            "Name": "AllowVnetInBound",
                            "Id": "/subscriptions00000000-0000-0000-0000-000000000000/resourceGroups/testrg2/providers/Microsoft.Network/networkSecurityGroups/testvm2-nsg/defaultSecurityRules/",
                            "Description": "Allow inbound traffic from all VMs in VNET",
                            "Protocol": "*",
                            "SourcePortRange": "*",
                            "DestinationPortRange": "*",
                            "SourceAddressPrefix": "VirtualNetwork",
                            "DestinationAddressPrefix": "VirtualNetwork",
                            "Access": "Allow",
                            "Priority": 65000,
                            "Direction": "Inbound",
                            "ProvisioningState": "Succeeded"
                          }
                          ...
                        ],
                        "EffectiveSecurityRules": [
                          {
                            "Name": "DefaultOutboundDenyAll",
                            "Protocol": "All",
                            "SourcePortRange": "0-65535",
                            "DestinationPortRange": "0-65535",
                            "SourceAddressPrefix": "*",
                            "DestinationAddressPrefix": "*",
                            "ExpandedSourceAddressPrefix": [],
                            "ExpandedDestinationAddressPrefix": [],
                            "Access": "Deny",
                            "Priority": 65500,
                            "Direction": "Outbound"
                          },
                          ...
                        ]
                      }
                    ]
```

## <a name="next-steps"></a><span data-ttu-id="4f6b9-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4f6b9-127">Next steps</span></span>

<span data-ttu-id="4f6b9-128">Navštivte [auditování zabezpečení sítě skupiny (NSG) s sledovací proces sítě](network-watcher-nsg-auditing-powershell.md) toolearn jak tooautomate ověření skupin zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="4f6b9-128">Visit [Auditing Network Security Groups (NSG) with Network Watcher](network-watcher-nsg-auditing-powershell.md) toolearn how tooautomate validation of Network Security Groups.</span></span>


