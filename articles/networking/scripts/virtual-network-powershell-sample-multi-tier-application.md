---
title: "Azure skript prostředí PowerShell ukázkový – vytvoření sítě pro vícevrstvé aplikace | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – vytvoření virtuální sítě pro vícevrstvé aplikace."
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
ms.openlocfilehash: ab49e78ef17b093d2bbe4e3276a1ece3a4247f91
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-network-for-multi-tier-applications"></a><span data-ttu-id="4ba0d-103">Vytvoření sítě pro vícevrstvé aplikace</span><span class="sxs-lookup"><span data-stu-id="4ba0d-103">Create a network for multi-tier applications</span></span>

<span data-ttu-id="4ba0d-104">Tento ukázkový skript vytvoří virtuální síť s podsítí front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="4ba0d-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="4ba0d-105">Provoz do front-endu podsítě je omezen na protokolu HTTP a SSH, zatímco provozu do podsítě back-end je omezený na MySQL, port 3306.</span><span class="sxs-lookup"><span data-stu-id="4ba0d-105">Traffic to the front-end subnet is limited to HTTP and SSH, while traffic to the back-end subnet is limited to MySQL, port 3306.</span></span> <span data-ttu-id="4ba0d-106">Po spuštění skriptu, budete mít dva virtuální počítače, jeden v jednotlivých podsítích, kterou můžete nasadit webový server a databáze MySQL software.</span><span class="sxs-lookup"><span data-stu-id="4ba0d-106">After running the script, you will have two virtual machines, one in each subnet that you can deploy web server and MySQL software to.</span></span>

<span data-ttu-id="4ba0d-107">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)a poté spusťte `Login-AzureRmAccount` vytvořit připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="4ba0d-107">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="4ba0d-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="4ba0d-108">Sample script</span></span>


<span data-ttu-id="4ba0d-109">[!code-powershell[hlavní](../../../powershell_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.ps1  "virtuální sítě pro vícevrstvé aplikace")]</span><span class="sxs-lookup"><span data-stu-id="4ba0d-109">[!code-powershell[main](../../../powershell_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.ps1  "Virtual network for multi-tier application")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="4ba0d-110">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="4ba0d-110">Clean up deployment</span></span> 

<span data-ttu-id="4ba0d-111">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="4ba0d-111">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="4ba0d-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="4ba0d-112">Script explanation</span></span>

<span data-ttu-id="4ba0d-113">Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální sítě a skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="4ba0d-113">This script uses the following commands to create a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="4ba0d-114">Každý příkaz v tabulce odkazy na dokumentaci specifické pro příkaz.</span><span class="sxs-lookup"><span data-stu-id="4ba0d-114">Each command in the table links to command-specific documentation.</span></span>

| <span data-ttu-id="4ba0d-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="4ba0d-115">Command</span></span> | <span data-ttu-id="4ba0d-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="4ba0d-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="4ba0d-117">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="4ba0d-117">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="4ba0d-118">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="4ba0d-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="4ba0d-119">Nový AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="4ba0d-119">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="4ba0d-120">Vytvoří virtuální síť Azure a front-end podsítě.</span><span class="sxs-lookup"><span data-stu-id="4ba0d-120">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="4ba0d-121">Nové AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="4ba0d-121">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="4ba0d-122">Vytvoří podsíť back-end.</span><span class="sxs-lookup"><span data-stu-id="4ba0d-122">Creates a back-end subnet.</span></span> |
| [<span data-ttu-id="4ba0d-123">Nové AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="4ba0d-123">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="4ba0d-124">Vytvoří veřejnou IP adresu z Internetu přístup k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="4ba0d-124">Creates a public IP address to access the VM from the Internet.</span></span> |
| [<span data-ttu-id="4ba0d-125">Nové AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="4ba0d-125">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="4ba0d-126">Vytvoří rozhraní virtuální sítě a připojí je k podsítím virtuální sítě front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="4ba0d-126">Creates virtual network interfaces and attaches them to the virtual network's front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="4ba0d-127">Nové AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="4ba0d-127">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="4ba0d-128">Vytvoří síť skupiny zabezpečení (NSG), které jsou přidruženy k podsítím front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="4ba0d-128">Creates network security groups (NSG) that are associated to the front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="4ba0d-129">Nové AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="4ba0d-129">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) |<span data-ttu-id="4ba0d-130">Vytvoří pravidla NSG, které povolí nebo blokuje specifické porty ke konkrétním podsítím.</span><span class="sxs-lookup"><span data-stu-id="4ba0d-130">Creates NSG rules that allow or block specific ports to specific subnets.</span></span> |
| [<span data-ttu-id="4ba0d-131">Nový AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="4ba0d-131">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="4ba0d-132">Vytvoří virtuální počítače a připojí síťovou kartu pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="4ba0d-132">Creates virtual machines and attaches a NIC to each VM.</span></span> <span data-ttu-id="4ba0d-133">Tento příkaz také Určuje bitovou kopii virtuálního počítače používat a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="4ba0d-133">This command also specifies the virtual machine image to use and administrative credentials.</span></span> |
| [<span data-ttu-id="4ba0d-134">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="4ba0d-134">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="4ba0d-135">Odstraní skupinu prostředků a všechny prostředky, které obsahuje.</span><span class="sxs-lookup"><span data-stu-id="4ba0d-135">Deletes a resource group and all resources it contains.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4ba0d-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4ba0d-136">Next steps</span></span>

<span data-ttu-id="4ba0d-137">Další informace o prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4ba0d-137">For more information on the Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/azure/overview).</span></span>

<span data-ttu-id="4ba0d-138">Další ukázky sítě skript prostředí PowerShell najdete v [přehled sítě Azure dokumentaci](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4ba0d-138">Additional networking PowerShell script samples can be found in the [Azure Networking Overview documentation](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>