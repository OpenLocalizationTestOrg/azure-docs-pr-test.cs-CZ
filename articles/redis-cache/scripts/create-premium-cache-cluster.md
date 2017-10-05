---
title: "Azure CLI skriptu ukázkové – vytvoření mezipaměť Azure Redis Cache Premium s clusteringem | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření úroveň Premium Azure Redis Cache s clusteringem"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
tags: azure-service-management
ms.assetid: 07bcceae-2521-4fe3-b88f-ed833104ddd2
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 04/14/2017
ms.author: sdanie
ms.openlocfilehash: 87d0fe4c3eaa8f7b75343a36a069ecdac8241d74
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-premium-azure-redis-cache-with-clustering"></a><span data-ttu-id="c2037-103">Vytvořit mezipaměť Azure Redis Cache Premium s clusteringem</span><span class="sxs-lookup"><span data-stu-id="c2037-103">Create a Premium Azure Redis Cache with clustering</span></span>

<span data-ttu-id="c2037-104">V tomto scénáři zjistíte, jak vytvořit úrovni Premium 6 GB Azure Redis Cache s povoleným clusteringem a dvě horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="c2037-104">In this scenario, you learn how to create a 6 GB Premium tier Azure Redis Cache with clustering enabled and two shards.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a><span data-ttu-id="c2037-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="c2037-105">Sample script</span></span>

<span data-ttu-id="c2037-106">[!code-azurecli[hlavní](../../../cli_scripts/redis-cache/create-premium-cache-cluster/create-premium-cache-cluster.sh "Azure Redis Cache")]</span><span class="sxs-lookup"><span data-stu-id="c2037-106">[!code-azurecli[main](../../../cli_scripts/redis-cache/create-premium-cache-cluster/create-premium-cache-cluster.sh "Azure Redis Cache")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="c2037-107">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="c2037-107">Script explanation</span></span>

<span data-ttu-id="c2037-108">Tento skript používá následující příkazy k vytvoření skupiny prostředků a mezipaměti redis úroveň Premium s clusteringem povolit.</span><span class="sxs-lookup"><span data-stu-id="c2037-108">This script uses the following commands to create a resource group and a Premium tier redis cache with clustering enable.</span></span> <span data-ttu-id="c2037-109">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="c2037-109">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="c2037-110">Příkaz</span><span class="sxs-lookup"><span data-stu-id="c2037-110">Command</span></span> | <span data-ttu-id="c2037-111">Poznámky</span><span class="sxs-lookup"><span data-stu-id="c2037-111">Notes</span></span> |
|---|---|
| [<span data-ttu-id="c2037-112">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="c2037-112">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="c2037-113">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="c2037-113">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="c2037-114">Vytvoření az redis</span><span class="sxs-lookup"><span data-stu-id="c2037-114">az redis create</span></span>](https://docs.microsoft.com/cli/azure/redis#create) | <span data-ttu-id="c2037-115">Vytvořte instanci služby Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="c2037-115">Create Redis Cache instance.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="c2037-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c2037-116">Next steps</span></span>

<span data-ttu-id="c2037-117">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c2037-117">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="c2037-118">Další ukázky skriptu Azure Redis Cache rozhraní příkazového řádku najdete v [dokumentace k Azure Redis Cache](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="c2037-118">Additional Azure Redis Cache CLI script samples can be found in the [Azure Redis Cache documentation](../cli-samples.md).</span></span>