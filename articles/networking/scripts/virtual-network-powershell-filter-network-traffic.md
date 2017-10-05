---
title: "Ukázka skriptu Azure Powershellu - provoz sítě virtuálních počítačů filtr | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – filtrovat příchozí a odchozí provoz sítě virtuálních počítačů."
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
ms.openlocfilehash: e871ba2f370157936c2aaabc804dc9f5aea6d7ca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a><span data-ttu-id="8a7f8-103">Filtrovat příchozí a odchozí provoz sítě virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="8a7f8-103">Filter inbound and outbound VM network traffic</span></span>

<span data-ttu-id="8a7f8-104">Tento ukázkový skript vytvoří virtuální síť s podsítí front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="8a7f8-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="8a7f8-105">Příchozí síťový provoz do front-endu podsíť je omezený na protokolu HTTP a HTTPS, zatímco odchozí provoz k Internetu z podsítě back-end není povolen.</span><span class="sxs-lookup"><span data-stu-id="8a7f8-105">Inbound network traffic to the front-end subnet is limited to HTTP, and HTTPS, while outbound traffic to the Internet from the back-end subnet is not permitted.</span></span> <span data-ttu-id="8a7f8-106">Po spuštění skriptu, budete mít jeden virtuální počítač se dvěma síťovými adaptéry.</span><span class="sxs-lookup"><span data-stu-id="8a7f8-106">After running the script, you will have one virtual machine with two NICs.</span></span> <span data-ttu-id="8a7f8-107">Každý síťový adaptér je připojený k jiné podsíti.</span><span class="sxs-lookup"><span data-stu-id="8a7f8-107">Each NIC is connected to a different subnet.</span></span>

<span data-ttu-id="8a7f8-108">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)a poté spusťte `Login-AzureRmAccount` vytvořit připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="8a7f8-108">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="8a7f8-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="8a7f8-109">Sample script</span></span>


<span data-ttu-id="8a7f8-110">[!code-powershell[hlavní](../../../powershell_scripts/virtual-network/filter-network-traffic/filter-network-traffic.ps1  "provoz sítě virtuálních počítačů filtru")]</span><span class="sxs-lookup"><span data-stu-id="8a7f8-110">[!code-powershell[main](../../../powershell_scripts/virtual-network/filter-network-traffic/filter-network-traffic.ps1  "Filter VM network traffic")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="8a7f8-111">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="8a7f8-111">Clean up deployment</span></span> 

<span data-ttu-id="8a7f8-112">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="8a7f8-112">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="8a7f8-113">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="8a7f8-113">Script explanation</span></span>

<span data-ttu-id="8a7f8-114">Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální sítě a skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="8a7f8-114">This script uses the following commands to create a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="8a7f8-115">Každý příkaz v tabulce odkazy na dokumentaci specifické pro příkaz.</span><span class="sxs-lookup"><span data-stu-id="8a7f8-115">Each command in the table links to command-specific documentation.</span></span>

| <span data-ttu-id="8a7f8-116">Příkaz</span><span class="sxs-lookup"><span data-stu-id="8a7f8-116">Command</span></span> | <span data-ttu-id="8a7f8-117">Poznámky</span><span class="sxs-lookup"><span data-stu-id="8a7f8-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="8a7f8-118">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="8a7f8-118">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="8a7f8-119">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="8a7f8-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="8a7f8-120">Nové AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="8a7f8-120">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="8a7f8-121">Vytvoří objekt konfigurace podsítě</span><span class="sxs-lookup"><span data-stu-id="8a7f8-121">Creates a subnet configuration object</span></span> |
| [<span data-ttu-id="8a7f8-122">Nový AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="8a7f8-122">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="8a7f8-123">Vytvoří virtuální síť Azure a front-end podsítě.</span><span class="sxs-lookup"><span data-stu-id="8a7f8-123">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="8a7f8-124">Nové AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="8a7f8-124">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="8a7f8-125">Vytvoří pravidla zabezpečení pro přiřazení skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="8a7f8-125">Creates security rules to be assigned to a network security group.</span></span> |
| [<span data-ttu-id="8a7f8-126">Nové AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="8a7f8-126">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) |<span data-ttu-id="8a7f8-127">Vytvoří pravidla NSG, které povolí nebo blokuje specifické porty ke konkrétním podsítím.</span><span class="sxs-lookup"><span data-stu-id="8a7f8-127">Creates NSG rules that allow or block specific ports to specific subnets.</span></span> |
| [<span data-ttu-id="8a7f8-128">Set-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="8a7f8-128">Set-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="8a7f8-129">Přidruží skupiny Nsg na podsítě.</span><span class="sxs-lookup"><span data-stu-id="8a7f8-129">Associates NSGs to subnets.</span></span> |
| [<span data-ttu-id="8a7f8-130">Nové AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="8a7f8-130">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="8a7f8-131">Vytvoří veřejnou IP adresu z Internetu přístup k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="8a7f8-131">Creates a public IP address to access the VM from the Internet.</span></span> |
| [<span data-ttu-id="8a7f8-132">Nové AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="8a7f8-132">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="8a7f8-133">Vytvoří rozhraní virtuální sítě a připojí je k podsítím virtuální sítě front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="8a7f8-133">Creates virtual network interfaces and attaches them to the virtual network's front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="8a7f8-134">Nové AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="8a7f8-134">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="8a7f8-135">Vytvoří konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8a7f8-135">Creates a VM configuration.</span></span> <span data-ttu-id="8a7f8-136">Tato konfigurace zahrnuje informace, jako je název virtuálního počítače, operační systém a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="8a7f8-136">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="8a7f8-137">Konfigurace se používá při vytváření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="8a7f8-137">The configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="8a7f8-138">Nový AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="8a7f8-138">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="8a7f8-139">Vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8a7f8-139">Create a virtual machine.</span></span> |
|[<span data-ttu-id="8a7f8-140">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="8a7f8-140">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="8a7f8-141">Odebere skupinu prostředků a všechny prostředky obsažené v rámci.</span><span class="sxs-lookup"><span data-stu-id="8a7f8-141">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="8a7f8-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8a7f8-142">Next steps</span></span>

<span data-ttu-id="8a7f8-143">Další informace o prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8a7f8-143">For more information on the Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/azure/overview).</span></span>

<span data-ttu-id="8a7f8-144">Další ukázky sítě skript prostředí PowerShell najdete v [přehled sítě Azure dokumentaci](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8a7f8-144">Additional networking PowerShell script samples can be found in the [Azure Networking Overview documentation](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>