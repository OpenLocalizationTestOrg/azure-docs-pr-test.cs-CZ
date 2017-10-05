---
title: "Analyzovat zabezpečení sítě s Azure sítě sledovacích procesů zabezpečení skupiny zobrazení – prostředí PowerShell | Microsoft Docs"
description: "Tento článek popisuje, jak pomocí prostředí PowerShell k analýze zabezpečení virtuálních počítačů s zobrazení skupiny zabezpečení."
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
ms.openlocfilehash: 363fdd9f1de933bb4050f91e1e111aaf3e419058
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-powershell"></a><span data-ttu-id="c0d6b-103">Analýza zabezpečení vašeho virtuálního počítače s zobrazení skupiny zabezpečení pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c0d6b-103">Analyze your Virtual Machine security with Security Group View using PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="c0d6b-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c0d6b-104">PowerShell</span></span>](network-watcher-security-group-view-powershell.md)
> - [<span data-ttu-id="c0d6b-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="c0d6b-105">CLI 1.0</span></span>](network-watcher-security-group-view-cli-nodejs.md)
> - [<span data-ttu-id="c0d6b-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="c0d6b-106">CLI 2.0</span></span>](network-watcher-security-group-view-cli.md)
> - [<span data-ttu-id="c0d6b-107">REST API</span><span class="sxs-lookup"><span data-stu-id="c0d6b-107">REST API</span></span>](network-watcher-security-group-view-rest.md)

<span data-ttu-id="c0d6b-108">Zobrazení skupiny zabezpečení vrátí pravidla zabezpečení sítě nakonfigurované a efektivní, která se použijí k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="c0d6b-108">Security group view returns configured and effective network security rules that are applied to a virtual machine.</span></span> <span data-ttu-id="c0d6b-109">Tato možnost je užitečná k auditování a diagnostice skupin zabezpečení sítě a pravidel, které jsou nakonfigurované na virtuálním počítači zajistit provoz se správně povolený nebo zakázaný.</span><span class="sxs-lookup"><span data-stu-id="c0d6b-109">This capability is useful to audit and diagnose Network Security Groups and rules that are configured on a VM to ensure traffic is being correctly allowed or denied.</span></span> <span data-ttu-id="c0d6b-110">V tomto článku jsme ukazují, jak načíst pravidla zabezpečení nakonfigurované a efektivní k virtuálnímu počítači pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c0d6b-110">In this article, we show you how to retrieve the configured and effective security rules to a virtual machine using PowerShell</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="c0d6b-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="c0d6b-111">Before you begin</span></span>

<span data-ttu-id="c0d6b-112">V tomto scénáři můžete spustit `Get-AzureRmNetworkWatcherSecurityGroupView` rutiny k načtení informací o pravidlo zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="c0d6b-112">In this scenario, you run the `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet to retrieve the security rule information.</span></span>

<span data-ttu-id="c0d6b-113">Tento scénář předpokládá, že už jste udělali kroky v [vytvořit sledovací proces sítě](network-watcher-create.md) vytvořit sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="c0d6b-113">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="c0d6b-114">Scénář</span><span class="sxs-lookup"><span data-stu-id="c0d6b-114">Scenario</span></span>

<span data-ttu-id="c0d6b-115">Scénář popsaná v tomto článku načte pravidla nakonfigurované a efektivní zabezpečení pro daný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="c0d6b-115">The scenario covered in this article retrieves the configured and effective security rules for a given virtual machine.</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="c0d6b-116">Načtení sledovací proces sítě</span><span class="sxs-lookup"><span data-stu-id="c0d6b-116">Retrieve Network Watcher</span></span>

<span data-ttu-id="c0d6b-117">Prvním krokem je pro získání instance sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="c0d6b-117">The first step is to retrieve the Network Watcher instance.</span></span> <span data-ttu-id="c0d6b-118">Tato proměnná je předána `Get-AzureRmNetworkWatcherSecurityGroupView` rutiny.</span><span class="sxs-lookup"><span data-stu-id="c0d6b-118">This variable is passed to the `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
```

## <a name="get-a-vm"></a><span data-ttu-id="c0d6b-119">Získat virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="c0d6b-119">Get a VM</span></span>

<span data-ttu-id="c0d6b-120">Virtuální počítač je potřeba spustit `Get-AzureRmNetworkWatcherSecurityGroupView` rutiny proti.</span><span class="sxs-lookup"><span data-stu-id="c0d6b-120">A virtual machine is required to run the `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet against.</span></span> <span data-ttu-id="c0d6b-121">Následující příklad načte objektu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c0d6b-121">The following example gets a VM object.</span></span>

```powershell
$VM = Get-AzurermVM -ResourceGroupName testrg -Name testvm1
```

## <a name="retrieve-security-group-view"></a><span data-ttu-id="c0d6b-122">Načtení zobrazení skupiny zabezpečení</span><span class="sxs-lookup"><span data-stu-id="c0d6b-122">Retrieve security group view</span></span>

<span data-ttu-id="c0d6b-123">Dalším krokem je načíst výsledky zobrazení skupiny zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="c0d6b-123">The next step is to retrieve the security group view result.</span></span>

```powershell
$secgroup = Get-AzureRmNetworkWatcherSecurityGroupView -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id
```

## <a name="viewing-the-results"></a><span data-ttu-id="c0d6b-124">Zobrazení výsledků</span><span class="sxs-lookup"><span data-stu-id="c0d6b-124">Viewing the results</span></span>

<span data-ttu-id="c0d6b-125">V následujícím příkladu je zkrácení odpověď výsledky vrácené.</span><span class="sxs-lookup"><span data-stu-id="c0d6b-125">The following example is a shortened response of the results returned.</span></span> <span data-ttu-id="c0d6b-126">Výsledky zobrazit všechna pravidla zabezpečení efektivní a použitých na virtuálním počítači rozdělení v skupiny **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, a **EffectiveSecurityRules**.</span><span class="sxs-lookup"><span data-stu-id="c0d6b-126">The results show all the effective and applied security rules on the virtual machine broken down in groups of **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, and **EffectiveSecurityRules**.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="c0d6b-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c0d6b-127">Next steps</span></span>

<span data-ttu-id="c0d6b-128">Navštivte [auditování zabezpečení sítě skupiny (NSG) s sledovací proces sítě](network-watcher-nsg-auditing-powershell.md) se dozvíte, jak automatizovat ověření skupin zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="c0d6b-128">Visit [Auditing Network Security Groups (NSG) with Network Watcher](network-watcher-nsg-auditing-powershell.md) to learn how to automate validation of Network Security Groups.</span></span>


