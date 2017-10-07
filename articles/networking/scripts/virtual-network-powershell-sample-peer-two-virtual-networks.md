---
title: "aaaAzure ukázkový skript prostředí PowerShell - Peer dvě virtuální sítě | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - sdílené dvě virtuální sítě"
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
ms.openlocfilehash: 8b66085c35de2fc30bcef57a00d7d370911d1f3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="peer-two-virtual-networks"></a><span data-ttu-id="bd004-103">Peer dvě virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="bd004-103">Peer two virtual networks</span></span>

<span data-ttu-id="bd004-104">Tento skript vytvoří a připojí dvou virtuálních sítí v hello stejné oblasti trhough hello síť Azure.</span><span class="sxs-lookup"><span data-stu-id="bd004-104">This script creates and connects two virtual networks in hello same region trhough hello Azure network.</span></span> <span data-ttu-id="bd004-105">Po spuštění skriptu hello, vytvoříte partnerský vztah mezi dvěma virtuálními sítěmi.</span><span class="sxs-lookup"><span data-stu-id="bd004-105">After running hello script, you will create a peering between two virtual networks.</span></span>

<span data-ttu-id="bd004-106">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí hello instrukce najít v hello hello [prostředí Azure PowerShell průvodce](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)a poté spusťte `Login-AzureRmAccount` toocreate připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="bd004-106">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="bd004-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="bd004-107">Sample script</span></span>

[!code-azurepowershell[main](../../../powershell_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.ps1 "Peer two networks")]

## <a name="clean-up-deployment"></a><span data-ttu-id="bd004-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="bd004-108">Clean up deployment</span></span> 

<span data-ttu-id="bd004-109">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="bd004-109">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="bd004-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="bd004-110">Script explanation</span></span>

<span data-ttu-id="bd004-111">Tento skript používá hello následující příkazy toocreate skupinu prostředků, virtuální počítač, a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="bd004-111">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="bd004-112">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="bd004-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="bd004-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="bd004-113">Command</span></span> | <span data-ttu-id="bd004-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="bd004-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="bd004-115">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="bd004-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="bd004-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="bd004-116">Creates a resource group in which all resources are stored.</span></span> | 
| [<span data-ttu-id="bd004-117">Nový AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="bd004-117">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork)| <span data-ttu-id="bd004-118">Vytvoří virtuální síť Azure a podsíť.</span><span class="sxs-lookup"><span data-stu-id="bd004-118">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="bd004-119">Přidat AzureRmVirtualNetworkPeering</span><span class="sxs-lookup"><span data-stu-id="bd004-119">Add-AzureRmVirtualNetworkPeering</span></span>](/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering) | <span data-ttu-id="bd004-120">Vytvoří partnerský vztah mezi dvěma virtuálními sítěmi.</span><span class="sxs-lookup"><span data-stu-id="bd004-120">Creates a peering between two virtual networks.</span></span>  |
| [<span data-ttu-id="bd004-121">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="bd004-121">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="bd004-122">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="bd004-122">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="bd004-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bd004-123">Next steps</span></span>

<span data-ttu-id="bd004-124">Další informace o hello prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="bd004-124">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/azure/overview).</span></span>

<span data-ttu-id="bd004-125">Další ukázky sítě skript prostředí PowerShell najdete v hello [přehled sítě Azure dokumentaci](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bd004-125">Additional networking PowerShell script samples can be found in hello [Azure Networking Overview documentation](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>
