---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - Peer dvě virtuální sítě | Microsoft Docs"
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
ms.openlocfilehash: 54dabb2b9e05951d10f1b6b4f61ca592ce11d364
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="peer-two-virtual-networks"></a><span data-ttu-id="5ec8f-103">Peer dvě virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="5ec8f-103">Peer two virtual networks</span></span>

<span data-ttu-id="5ec8f-104">Tento skript vytvoří a připojí dvou virtuálních sítí v hello stejné oblasti trhough hello síť Azure.</span><span class="sxs-lookup"><span data-stu-id="5ec8f-104">This script creates and connects two virtual networks in hello same region trhough hello Azure network.</span></span> <span data-ttu-id="5ec8f-105">Po spuštění skriptu hello, vytvoříte partnerský vztah mezi dvěma virtuálními sítěmi.</span><span class="sxs-lookup"><span data-stu-id="5ec8f-105">After running hello script, you will create a peering between two virtual networks.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a><span data-ttu-id="5ec8f-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="5ec8f-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.sh "Peer two networks")]

## <a name="clean-up-deployment"></a><span data-ttu-id="5ec8f-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="5ec8f-107">Clean up deployment</span></span> 

<span data-ttu-id="5ec8f-108">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="5ec8f-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="5ec8f-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="5ec8f-109">Script explanation</span></span>

<span data-ttu-id="5ec8f-110">Tento skript používá hello následující příkazy toocreate skupinu prostředků, virtuální počítač, a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="5ec8f-110">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="5ec8f-111">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="5ec8f-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="5ec8f-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="5ec8f-112">Command</span></span> | <span data-ttu-id="5ec8f-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="5ec8f-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="5ec8f-114">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="5ec8f-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="5ec8f-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="5ec8f-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="5ec8f-116">Vytvoření sítě vnet az</span><span class="sxs-lookup"><span data-stu-id="5ec8f-116">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="5ec8f-117">Vytvoří virtuální síť Azure a podsíť.</span><span class="sxs-lookup"><span data-stu-id="5ec8f-117">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="5ec8f-118">virtuální síť az sítě partnerský vztah vytvořit.</span><span class="sxs-lookup"><span data-stu-id="5ec8f-118">az network vnet peering create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet/peering#create) | <span data-ttu-id="5ec8f-119">Vytvoří partnerský vztah mezi dvěma virtuálními sítěmi.</span><span class="sxs-lookup"><span data-stu-id="5ec8f-119">Creates a peering between two virtual networks.</span></span>  |
| [<span data-ttu-id="5ec8f-120">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="5ec8f-120">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="5ec8f-121">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="5ec8f-121">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="5ec8f-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5ec8f-122">Next steps</span></span>

<span data-ttu-id="5ec8f-123">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5ec8f-123">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="5ec8f-124">Další síťové rozhraní příkazového řádku skriptu ukázky lze nalézt v hello [přehled sítě Azure dokumentaci](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="5ec8f-124">Additional networking CLI script samples can be found in hello [Azure Networking Overview documentation](../cli-samples.md).</span></span>
