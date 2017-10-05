---
title: "Ukázka skriptu Azure Powershellu - směrovat provoz prostřednictvím zařízení virtuální sítě | Microsoft Docs"
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
ms.openlocfilehash: 883d28dac72a66c2186d222f72b04d68e532cead
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a><span data-ttu-id="4cadf-103">Směrovat provoz prostřednictvím sítě virtuálního zařízení</span><span class="sxs-lookup"><span data-stu-id="4cadf-103">Route traffic through a network virtual appliance</span></span>

<span data-ttu-id="4cadf-104">Tento ukázkový skript vytvoří virtuální síť s podsítí front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="4cadf-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="4cadf-105">Také vytvoří virtuální počítač se ke směrování provozu mezi dvěma podsítěmi povolené předávání IP.</span><span class="sxs-lookup"><span data-stu-id="4cadf-105">It also creates a VM with IP forwarding enabled to route traffic between the two subnets.</span></span> <span data-ttu-id="4cadf-106">Po spuštění skriptu můžete nasadit software pro sítě, jako je například Brána firewall aplikace, do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4cadf-106">After running the script you can deploy network software, such as a firewall application, to the VM.</span></span>

<span data-ttu-id="4cadf-107">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)a poté spusťte `Login-AzureRmAccount` vytvořit připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="4cadf-107">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="4cadf-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="4cadf-108">Sample script</span></span>


<span data-ttu-id="4cadf-109">[!code-powershell[hlavní](../../../powershell_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.ps1 "směrování provozu prostřednictvím sítě virtuálního zařízení")]</span><span class="sxs-lookup"><span data-stu-id="4cadf-109">[!code-powershell[main](../../../powershell_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.ps1 "Route traffic through a network virtual appliance")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="4cadf-110">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="4cadf-110">Clean up deployment</span></span> 

<span data-ttu-id="4cadf-111">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="4cadf-111">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```
## <a name="script-explanation"></a><span data-ttu-id="4cadf-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="4cadf-112">Script explanation</span></span>

<span data-ttu-id="4cadf-113">Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální sítě a skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="4cadf-113">This script uses the following commands to create a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="4cadf-114">Každý příkaz v tabulce odkazy na dokumentaci specifické pro příkaz.</span><span class="sxs-lookup"><span data-stu-id="4cadf-114">Each command in the table links to command-specific documentation.</span></span>

| <span data-ttu-id="4cadf-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="4cadf-115">Command</span></span> | <span data-ttu-id="4cadf-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="4cadf-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="4cadf-117">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="4cadf-117">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | <span data-ttu-id="4cadf-118">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="4cadf-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="4cadf-119">Nový AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="4cadf-119">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="4cadf-120">Vytvoří virtuální síť Azure a front-end podsítě.</span><span class="sxs-lookup"><span data-stu-id="4cadf-120">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="4cadf-121">Nové AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="4cadf-121">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="4cadf-122">Vytvoří back-end a DMZ podsítě.</span><span class="sxs-lookup"><span data-stu-id="4cadf-122">Creates back-end and DMZ subnets.</span></span> |
| [<span data-ttu-id="4cadf-123">Nové AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="4cadf-123">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="4cadf-124">Vytvoří veřejnou IP adresu z Internetu přístup k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="4cadf-124">Creates a public IP address to access the VM from the Internet.</span></span> |
| [<span data-ttu-id="4cadf-125">Nové AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="4cadf-125">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="4cadf-126">Vytvoří rozhraní virtuální sítě a předávání IP povolit pro ni.</span><span class="sxs-lookup"><span data-stu-id="4cadf-126">Creates a virtual network interface and enable IP forwarding for it.</span></span> |
| [<span data-ttu-id="4cadf-127">Nové AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="4cadf-127">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="4cadf-128">Vytvoří skupinu zabezpečení sítě (NSG).</span><span class="sxs-lookup"><span data-stu-id="4cadf-128">Creates a network security group (NSG).</span></span> |
| [<span data-ttu-id="4cadf-129">Nové AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="4cadf-129">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="4cadf-130">Vytvoří pravidla NSG, které umožní příchozí porty HTTP a HTTPS k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="4cadf-130">Creates NSG rules that allow HTTP and HTTPS ports inbound to the VM.</span></span> |
| [<span data-ttu-id="4cadf-131">Set-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="4cadf-131">Set-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)| <span data-ttu-id="4cadf-132">Přidruží skupiny Nsg a směrovací tabulky k podsítím.</span><span class="sxs-lookup"><span data-stu-id="4cadf-132">Associates the NSGs and route tables to subnets.</span></span> |
| [<span data-ttu-id="4cadf-133">Nové AzureRmRouteTable</span><span class="sxs-lookup"><span data-stu-id="4cadf-133">New-AzureRmRouteTable</span></span>](/powershell/module/azurerm.network/new-azurermroutetable)| <span data-ttu-id="4cadf-134">Vytvoří směrovací tabulku pro všechny trasy.</span><span class="sxs-lookup"><span data-stu-id="4cadf-134">Creates a route table for all routes.</span></span> |
| [<span data-ttu-id="4cadf-135">Nové AzureRMRouteConfig</span><span class="sxs-lookup"><span data-stu-id="4cadf-135">New-AzureRMRouteConfig</span></span>](/powershell/module/azurerm.network/new-azurermrouteconfig)| <span data-ttu-id="4cadf-136">Vytvoří směrování směrovat provoz mezi podsítěmi a Internetu prostřednictvím virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4cadf-136">Creates routes to route traffic between subnets and the Internet through the VM.</span></span> |
| [<span data-ttu-id="4cadf-137">Nový AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="4cadf-137">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="4cadf-138">Vytvoří virtuální počítač a k němu připojí na síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="4cadf-138">Creates a virtual machine and attaches the NIC to it.</span></span> <span data-ttu-id="4cadf-139">Tento příkaz také Určuje bitovou kopii virtuálního počítače používat a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="4cadf-139">This command also specifies the virtual machine image to use and administrative credentials.</span></span> |
| [<span data-ttu-id="4cadf-140">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="4cadf-140">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup)  | <span data-ttu-id="4cadf-141">Odstraní skupinu prostředků a všechny prostředky, které obsahuje.</span><span class="sxs-lookup"><span data-stu-id="4cadf-141">Deletes a resource group and all resources it contains.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4cadf-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4cadf-142">Next steps</span></span>

<span data-ttu-id="4cadf-143">Další informace o prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4cadf-143">For more information on the Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/azure/overview).</span></span>

<span data-ttu-id="4cadf-144">Další ukázky sítě skript prostředí PowerShell najdete v [přehled sítě Azure dokumentaci](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4cadf-144">Additional networking PowerShell script samples can be found in the [Azure Networking Overview documentation](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>