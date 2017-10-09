---
title: "Ukázka skriptu aaaAzure prostředí PowerShell - směrovat provoz prostřednictvím zařízení virtuální sítě | Microsoft Docs"
description: "Azure PowerShell ukázka skriptu - směrovat provoz prostřednictvím brány firewall sítě virtuálního zařízení."
services: virtual-network
documentationcenter: virtual-network
author: georgewallace
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: 3b999f3289d654c00d5becb973e2883896457d52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a><span data-ttu-id="36349-103">Směrovat provoz prostřednictvím sítě virtuálního zařízení</span><span class="sxs-lookup"><span data-stu-id="36349-103">Route traffic through a network virtual appliance</span></span>

<span data-ttu-id="36349-104">Tento ukázkový skript vytvoří virtuální síť s podsítí front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="36349-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="36349-105">Vytvoří také virtuální počítač s IP Adresou předávání povoleno tooroute provozu mezi dvěma podsítěmi hello.</span><span class="sxs-lookup"><span data-stu-id="36349-105">It also creates a VM with IP forwarding enabled tooroute traffic between hello two subnets.</span></span> <span data-ttu-id="36349-106">Po spuštění skriptu hello můžete nasadit software pro sítě, jako například aplikací brány firewall, toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="36349-106">After running hello script you can deploy network software, such as a firewall application, toohello VM.</span></span>

<span data-ttu-id="36349-107">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí hello instrukce najít v hello hello [prostředí Azure PowerShell průvodce](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)a poté spusťte `Login-AzureRmAccount` toocreate připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="36349-107">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="36349-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="36349-108">Sample script</span></span>


[!code-powershell[main](../../../powershell_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.ps1 "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a><span data-ttu-id="36349-109">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="36349-109">Clean up deployment</span></span> 

<span data-ttu-id="36349-110">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="36349-110">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```
## <a name="script-explanation"></a><span data-ttu-id="36349-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="36349-111">Script explanation</span></span>

<span data-ttu-id="36349-112">Tento skript používá následující příkazy toocreate hello skupinu prostředků, virtuální sítě a skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="36349-112">This script uses hello following commands toocreate a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="36349-113">Každý příkaz v dokumentaci k toocommand specifické hello tabulky odkazů.</span><span class="sxs-lookup"><span data-stu-id="36349-113">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="36349-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="36349-114">Command</span></span> | <span data-ttu-id="36349-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="36349-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="36349-116">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="36349-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | <span data-ttu-id="36349-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="36349-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="36349-118">Nový AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="36349-118">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="36349-119">Vytvoří virtuální síť Azure a front-end podsítě.</span><span class="sxs-lookup"><span data-stu-id="36349-119">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="36349-120">Nové AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="36349-120">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="36349-121">Vytvoří back-end a DMZ podsítě.</span><span class="sxs-lookup"><span data-stu-id="36349-121">Creates back-end and DMZ subnets.</span></span> |
| [<span data-ttu-id="36349-122">Nové AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="36349-122">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="36349-123">Vytvoří veřejnou hello tooaccess IP adresy virtuálních počítačů z hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="36349-123">Creates a public IP address tooaccess hello VM from hello Internet.</span></span> |
| [<span data-ttu-id="36349-124">Nové AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="36349-124">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="36349-125">Vytvoří rozhraní virtuální sítě a předávání IP povolit pro ni.</span><span class="sxs-lookup"><span data-stu-id="36349-125">Creates a virtual network interface and enable IP forwarding for it.</span></span> |
| [<span data-ttu-id="36349-126">Nové AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="36349-126">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="36349-127">Vytvoří skupinu zabezpečení sítě (NSG).</span><span class="sxs-lookup"><span data-stu-id="36349-127">Creates a network security group (NSG).</span></span> |
| [<span data-ttu-id="36349-128">Nové AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="36349-128">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="36349-129">Vytvoří pravidla NSG, které umožňují, že porty HTTP a HTTPS příchozí toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="36349-129">Creates NSG rules that allow HTTP and HTTPS ports inbound toohello VM.</span></span> |
| [<span data-ttu-id="36349-130">Set-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="36349-130">Set-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)| <span data-ttu-id="36349-131">Partnerů hello skupiny Nsg a toosubnets tabulky trasy.</span><span class="sxs-lookup"><span data-stu-id="36349-131">Associates hello NSGs and route tables toosubnets.</span></span> |
| [<span data-ttu-id="36349-132">Nové AzureRmRouteTable</span><span class="sxs-lookup"><span data-stu-id="36349-132">New-AzureRmRouteTable</span></span>](/powershell/module/azurerm.network/new-azurermroutetable)| <span data-ttu-id="36349-133">Vytvoří směrovací tabulku pro všechny trasy.</span><span class="sxs-lookup"><span data-stu-id="36349-133">Creates a route table for all routes.</span></span> |
| [<span data-ttu-id="36349-134">Nové AzureRMRouteConfig</span><span class="sxs-lookup"><span data-stu-id="36349-134">New-AzureRMRouteConfig</span></span>](/powershell/module/azurerm.network/new-azurermrouteconfig)| <span data-ttu-id="36349-135">Vytvoří trasy tooroute provoz mezi podsítěmi a hello Internet prostřednictvím hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="36349-135">Creates routes tooroute traffic between subnets and hello Internet through hello VM.</span></span> |
| [<span data-ttu-id="36349-136">Nový AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="36349-136">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="36349-137">Vytvoří virtuální počítač a připojí tooit hello síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="36349-137">Creates a virtual machine and attaches hello NIC tooit.</span></span> <span data-ttu-id="36349-138">Tento příkaz také určuje toouse bitové kopie virtuálního počítače hello a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="36349-138">This command also specifies hello virtual machine image toouse and administrative credentials.</span></span> |
| [<span data-ttu-id="36349-139">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="36349-139">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup)  | <span data-ttu-id="36349-140">Odstraní skupinu prostředků a všechny prostředky, které obsahuje.</span><span class="sxs-lookup"><span data-stu-id="36349-140">Deletes a resource group and all resources it contains.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="36349-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="36349-141">Next steps</span></span>

<span data-ttu-id="36349-142">Další informace o hello prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="36349-142">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/azure/overview).</span></span>

<span data-ttu-id="36349-143">Další ukázky sítě skript prostředí PowerShell najdete v hello [přehled sítě Azure dokumentaci](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="36349-143">Additional networking PowerShell script samples can be found in hello [Azure Networking Overview documentation](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>
