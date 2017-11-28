---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvořit mezipaměť Azure Redis Cache Premium s clusteringem | Microsoft Docs"
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
ms.openlocfilehash: ca34d40059b282cb2abc7e3e2b8771226029744c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-premium-azure-redis-cache-with-clustering"></a><span data-ttu-id="bf739-103">Vytvořit mezipaměť Azure Redis Cache Premium s clusteringem</span><span class="sxs-lookup"><span data-stu-id="bf739-103">Create a Premium Azure Redis Cache with clustering</span></span>

<span data-ttu-id="bf739-104">V tomto scénáři zjistíte, jak toocreate úroveň Premium 6 GB Azure Redis Cache s clusteringem povolené a dvě horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="bf739-104">In this scenario, you learn how toocreate a 6 GB Premium tier Azure Redis Cache with clustering enabled and two shards.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a><span data-ttu-id="bf739-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="bf739-105">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/redis-cache/create-premium-cache-cluster/create-premium-cache-cluster.sh "Azure Redis Cache")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="bf739-106">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="bf739-106">Script explanation</span></span>

<span data-ttu-id="bf739-107">Tento skript používá následující příkazy toocreate hello skupinu prostředků a mezipaměti redis úroveň Premium s clusteringem povolit.</span><span class="sxs-lookup"><span data-stu-id="bf739-107">This script uses hello following commands toocreate a resource group and a Premium tier redis cache with clustering enable.</span></span> <span data-ttu-id="bf739-108">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="bf739-108">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="bf739-109">Příkaz</span><span class="sxs-lookup"><span data-stu-id="bf739-109">Command</span></span> | <span data-ttu-id="bf739-110">Poznámky</span><span class="sxs-lookup"><span data-stu-id="bf739-110">Notes</span></span> |
|---|---|
| [<span data-ttu-id="bf739-111">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="bf739-111">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="bf739-112">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="bf739-112">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="bf739-113">Vytvoření az redis</span><span class="sxs-lookup"><span data-stu-id="bf739-113">az redis create</span></span>](https://docs.microsoft.com/cli/azure/redis#create) | <span data-ttu-id="bf739-114">Vytvořte instanci služby Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="bf739-114">Create Redis Cache instance.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="bf739-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bf739-115">Next steps</span></span>

<span data-ttu-id="bf739-116">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="bf739-116">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="bf739-117">Další ukázky skriptu Azure Redis Cache rozhraní příkazového řádku najdete v hello [dokumentace k Azure Redis Cache](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="bf739-117">Additional Azure Redis Cache CLI script samples can be found in hello [Azure Redis Cache documentation](../cli-samples.md).</span></span>
