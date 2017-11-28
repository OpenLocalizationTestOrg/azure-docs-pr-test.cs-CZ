---
title: "Ukázka skriptu aaaAzure prostředí PowerShell - provoz sítě virtuálních počítačů filtr | Microsoft Docs"
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
ms.openlocfilehash: 39eae6a43a8dc7f9fc616ef3ec50f95443fd3547
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a><span data-ttu-id="98c49-103">Filtrovat příchozí a odchozí provoz sítě virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="98c49-103">Filter inbound and outbound VM network traffic</span></span>

<span data-ttu-id="98c49-104">Tento ukázkový skript vytvoří virtuální síť s podsítí front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="98c49-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="98c49-105">Příchozí síťový provoz toohello front-end podsíť je omezená tooHTTP a protokolu HTTPS, zatímco odchozí provoz toohello Internetu z podsítě hello back-end není povolená.</span><span class="sxs-lookup"><span data-stu-id="98c49-105">Inbound network traffic toohello front-end subnet is limited tooHTTP, and HTTPS, while outbound traffic toohello Internet from hello back-end subnet is not permitted.</span></span> <span data-ttu-id="98c49-106">Po spuštění skriptu hello, budete mít jeden virtuální počítač se dvěma síťovými adaptéry.</span><span class="sxs-lookup"><span data-stu-id="98c49-106">After running hello script, you will have one virtual machine with two NICs.</span></span> <span data-ttu-id="98c49-107">Každý síťový adaptér je připojený tooa jiné podsíti.</span><span class="sxs-lookup"><span data-stu-id="98c49-107">Each NIC is connected tooa different subnet.</span></span>

<span data-ttu-id="98c49-108">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí hello instrukce najít v hello hello [prostředí Azure PowerShell průvodce](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)a poté spusťte `Login-AzureRmAccount` toocreate připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="98c49-108">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="98c49-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="98c49-109">Sample script</span></span>


[!code-powershell[main](../../../powershell_scripts/virtual-network/filter-network-traffic/filter-network-traffic.ps1  "Filter VM network traffic")]

## <a name="clean-up-deployment"></a><span data-ttu-id="98c49-110">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="98c49-110">Clean up deployment</span></span> 

<span data-ttu-id="98c49-111">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="98c49-111">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="98c49-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="98c49-112">Script explanation</span></span>

<span data-ttu-id="98c49-113">Tento skript používá následující příkazy toocreate hello skupinu prostředků, virtuální sítě a skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="98c49-113">This script uses hello following commands toocreate a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="98c49-114">Každý příkaz v dokumentaci k toocommand specifické hello tabulky odkazů.</span><span class="sxs-lookup"><span data-stu-id="98c49-114">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="98c49-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="98c49-115">Command</span></span> | <span data-ttu-id="98c49-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="98c49-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="98c49-117">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="98c49-117">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="98c49-118">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="98c49-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="98c49-119">Nové AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="98c49-119">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="98c49-120">Vytvoří objekt konfigurace podsítě</span><span class="sxs-lookup"><span data-stu-id="98c49-120">Creates a subnet configuration object</span></span> |
| [<span data-ttu-id="98c49-121">Nový AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="98c49-121">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="98c49-122">Vytvoří virtuální síť Azure a front-end podsítě.</span><span class="sxs-lookup"><span data-stu-id="98c49-122">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="98c49-123">Nové AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="98c49-123">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="98c49-124">Vytvoří toobe pravidla zabezpečení přiřazené tooa skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="98c49-124">Creates security rules toobe assigned tooa network security group.</span></span> |
| [<span data-ttu-id="98c49-125">Nové AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="98c49-125">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) |<span data-ttu-id="98c49-126">Vytvoří pravidla NSG, které povolí nebo blokuje specifické porty toospecific podsítě.</span><span class="sxs-lookup"><span data-stu-id="98c49-126">Creates NSG rules that allow or block specific ports toospecific subnets.</span></span> |
| [<span data-ttu-id="98c49-127">Set-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="98c49-127">Set-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="98c49-128">Přidruží toosubnets skupiny Nsg.</span><span class="sxs-lookup"><span data-stu-id="98c49-128">Associates NSGs toosubnets.</span></span> |
| [<span data-ttu-id="98c49-129">Nové AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="98c49-129">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="98c49-130">Vytvoří veřejnou hello tooaccess IP adresy virtuálních počítačů z hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="98c49-130">Creates a public IP address tooaccess hello VM from hello Internet.</span></span> |
| [<span data-ttu-id="98c49-131">Nové AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="98c49-131">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="98c49-132">Vytvoří rozhraní virtuální sítě a připojí je toohello virtuální sítě front-end a back-end podsítě.</span><span class="sxs-lookup"><span data-stu-id="98c49-132">Creates virtual network interfaces and attaches them toohello virtual network's front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="98c49-133">Nové AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="98c49-133">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="98c49-134">Vytvoří konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="98c49-134">Creates a VM configuration.</span></span> <span data-ttu-id="98c49-135">Tato konfigurace zahrnuje informace, jako je název virtuálního počítače, operační systém a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="98c49-135">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="98c49-136">Konfigurace Hello se používá při vytváření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="98c49-136">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="98c49-137">Nový AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="98c49-137">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="98c49-138">Vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="98c49-138">Create a virtual machine.</span></span> |
|[<span data-ttu-id="98c49-139">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="98c49-139">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="98c49-140">Odebere skupinu prostředků a všechny prostředky obsažené v rámci.</span><span class="sxs-lookup"><span data-stu-id="98c49-140">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="98c49-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="98c49-141">Next steps</span></span>

<span data-ttu-id="98c49-142">Další informace o hello prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="98c49-142">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/azure/overview).</span></span>

<span data-ttu-id="98c49-143">Další ukázky sítě skript prostředí PowerShell najdete v hello [přehled sítě Azure dokumentaci](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="98c49-143">Additional networking PowerShell script samples can be found in hello [Azure Networking Overview documentation](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>
