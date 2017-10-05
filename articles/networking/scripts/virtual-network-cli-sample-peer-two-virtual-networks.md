---
title: "Ukázka skriptu Azure CLI - sdílené dvě virtuální sítě | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - sdílené dvě virtuální sítě"
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: kumud
ms.openlocfilehash: a2b8fda288072e2fb0087988bbd68d3e4d9031d8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="peer-two-virtual-networks"></a><span data-ttu-id="285c6-103">Peer dvě virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="285c6-103">Peer two virtual networks</span></span>

<span data-ttu-id="285c6-104">Tento skript vytvoří a připojí dvou virtuálních sítí ve stejné oblasti trhough síť Azure.</span><span class="sxs-lookup"><span data-stu-id="285c6-104">This script creates and connects two virtual networks in the same region trhough the Azure network.</span></span> <span data-ttu-id="285c6-105">Po spuštění skriptu, vytvoříte partnerský vztah mezi dvěma virtuálními sítěmi.</span><span class="sxs-lookup"><span data-stu-id="285c6-105">After running the script, you will create a peering between two virtual networks.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a><span data-ttu-id="285c6-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="285c6-106">Sample script</span></span>

<span data-ttu-id="285c6-107">[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.sh "Peer dvě sítě")]</span><span class="sxs-lookup"><span data-stu-id="285c6-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.sh "Peer two networks")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="285c6-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="285c6-108">Clean up deployment</span></span> 

<span data-ttu-id="285c6-109">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="285c6-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="285c6-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="285c6-110">Script explanation</span></span>

<span data-ttu-id="285c6-111">Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="285c6-111">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="285c6-112">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="285c6-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="285c6-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="285c6-113">Command</span></span> | <span data-ttu-id="285c6-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="285c6-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="285c6-115">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="285c6-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="285c6-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="285c6-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="285c6-117">Vytvoření sítě vnet az</span><span class="sxs-lookup"><span data-stu-id="285c6-117">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="285c6-118">Vytvoří virtuální síť Azure a podsíť.</span><span class="sxs-lookup"><span data-stu-id="285c6-118">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="285c6-119">virtuální síť az sítě partnerský vztah vytvořit.</span><span class="sxs-lookup"><span data-stu-id="285c6-119">az network vnet peering create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet/peering#create) | <span data-ttu-id="285c6-120">Vytvoří partnerský vztah mezi dvěma virtuálními sítěmi.</span><span class="sxs-lookup"><span data-stu-id="285c6-120">Creates a peering between two virtual networks.</span></span>  |
| [<span data-ttu-id="285c6-121">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="285c6-121">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="285c6-122">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="285c6-122">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="285c6-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="285c6-123">Next steps</span></span>

<span data-ttu-id="285c6-124">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="285c6-124">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="285c6-125">Další síťové ukázky skriptu rozhraní příkazového řádku najdete v [přehled sítě Azure dokumentaci](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="285c6-125">Additional networking CLI script samples can be found in the [Azure Networking Overview documentation](../cli-samples.md).</span></span>
