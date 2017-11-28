---
title: "Vytvoření sítě pro vícevrstvé aplikace aaaAzure ukázkový skript prostředí PowerShell - | Microsoft Docs"
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
ms.openlocfilehash: 46d6d16dc5dbc8be467359f31346f017727b1abe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-network-for-multi-tier-applications"></a><span data-ttu-id="76e55-103">Vytvoření sítě pro vícevrstvé aplikace</span><span class="sxs-lookup"><span data-stu-id="76e55-103">Create a network for multi-tier applications</span></span>

<span data-ttu-id="76e55-104">Tento ukázkový skript vytvoří virtuální síť s podsítí front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="76e55-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="76e55-105">Podsíť front-end toohello provozu je omezená tooHTTP a SSH, při provozu toohello back-end podsíť je omezená tooMySQL, port 3306.</span><span class="sxs-lookup"><span data-stu-id="76e55-105">Traffic toohello front-end subnet is limited tooHTTP and SSH, while traffic toohello back-end subnet is limited tooMySQL, port 3306.</span></span> <span data-ttu-id="76e55-106">Po spouštění skriptu hello budete mít dva virtuální počítače, jeden v jednotlivých podsítích, kterou můžete nasadit webový server a databáze MySQL software.</span><span class="sxs-lookup"><span data-stu-id="76e55-106">After running hello script, you will have two virtual machines, one in each subnet that you can deploy web server and MySQL software to.</span></span>

<span data-ttu-id="76e55-107">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí hello instrukce najít v hello hello [prostředí Azure PowerShell průvodce](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)a poté spusťte `Login-AzureRmAccount` toocreate připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="76e55-107">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="76e55-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="76e55-108">Sample script</span></span>


[!code-powershell[main](../../../powershell_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.ps1  "Virtual network for multi-tier application")]

## <a name="clean-up-deployment"></a><span data-ttu-id="76e55-109">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="76e55-109">Clean up deployment</span></span> 

<span data-ttu-id="76e55-110">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="76e55-110">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="76e55-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="76e55-111">Script explanation</span></span>

<span data-ttu-id="76e55-112">Tento skript používá následující příkazy toocreate hello skupinu prostředků, virtuální sítě a skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="76e55-112">This script uses hello following commands toocreate a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="76e55-113">Každý příkaz v dokumentaci k toocommand specifické hello tabulky odkazů.</span><span class="sxs-lookup"><span data-stu-id="76e55-113">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="76e55-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="76e55-114">Command</span></span> | <span data-ttu-id="76e55-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="76e55-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="76e55-116">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="76e55-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="76e55-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="76e55-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="76e55-118">Nový AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="76e55-118">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="76e55-119">Vytvoří virtuální síť Azure a front-end podsítě.</span><span class="sxs-lookup"><span data-stu-id="76e55-119">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="76e55-120">Nové AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="76e55-120">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="76e55-121">Vytvoří podsíť back-end.</span><span class="sxs-lookup"><span data-stu-id="76e55-121">Creates a back-end subnet.</span></span> |
| [<span data-ttu-id="76e55-122">Nové AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="76e55-122">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="76e55-123">Vytvoří veřejnou hello tooaccess IP adresy virtuálních počítačů z hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="76e55-123">Creates a public IP address tooaccess hello VM from hello Internet.</span></span> |
| [<span data-ttu-id="76e55-124">Nové AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="76e55-124">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="76e55-125">Vytvoří rozhraní virtuální sítě a připojí je toohello virtuální sítě front-end a back-end podsítě.</span><span class="sxs-lookup"><span data-stu-id="76e55-125">Creates virtual network interfaces and attaches them toohello virtual network's front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="76e55-126">Nové AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="76e55-126">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="76e55-127">Vytvoří skupiny zabezpečení sítě (NSG), které jsou přidružené toohello front-end a back-end podsítě.</span><span class="sxs-lookup"><span data-stu-id="76e55-127">Creates network security groups (NSG) that are associated toohello front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="76e55-128">Nové AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="76e55-128">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) |<span data-ttu-id="76e55-129">Vytvoří pravidla NSG, které povolí nebo blokuje specifické porty toospecific podsítě.</span><span class="sxs-lookup"><span data-stu-id="76e55-129">Creates NSG rules that allow or block specific ports toospecific subnets.</span></span> |
| [<span data-ttu-id="76e55-130">Nový AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="76e55-130">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="76e55-131">Vytvoří virtuální počítače a připojí tooeach síťový adaptér virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="76e55-131">Creates virtual machines and attaches a NIC tooeach VM.</span></span> <span data-ttu-id="76e55-132">Tento příkaz také určuje toouse bitové kopie virtuálního počítače hello a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="76e55-132">This command also specifies hello virtual machine image toouse and administrative credentials.</span></span> |
| [<span data-ttu-id="76e55-133">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="76e55-133">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="76e55-134">Odstraní skupinu prostředků a všechny prostředky, které obsahuje.</span><span class="sxs-lookup"><span data-stu-id="76e55-134">Deletes a resource group and all resources it contains.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="76e55-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="76e55-135">Next steps</span></span>

<span data-ttu-id="76e55-136">Další informace o hello prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="76e55-136">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/azure/overview).</span></span>

<span data-ttu-id="76e55-137">Další ukázky sítě skript prostředí PowerShell najdete v hello [přehled sítě Azure dokumentaci](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="76e55-137">Additional networking PowerShell script samples can be found in hello [Azure Networking Overview documentation](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>
