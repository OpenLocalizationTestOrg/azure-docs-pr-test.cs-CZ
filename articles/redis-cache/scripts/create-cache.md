---
title: "Rozhraní příkazového řádku Azure ukázkový skript – vytvoření Azure Redis Cache | Microsoft Docs"
description: "Rozhraní příkazového řádku Azure ukázkový skript – vytvoření Azure Redis Cache"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
tags: azure-service-management
ms.assetid: afd7f6e0-9297-4c98-a95e-597be939cef7
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 04/14/2017
ms.author: sdanie
ms.openlocfilehash: c6b153d80de4cbf2bec1bc70d67be7befa0c5ec3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-redis-cache"></a><span data-ttu-id="51bfb-103">Vytvoření Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="51bfb-103">Create an Azure Redis Cache</span></span>

<span data-ttu-id="51bfb-104">V tomto scénáři zjistěte, jak vytvořit Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="51bfb-104">In this scenario, you learn how to create an Azure Redis Cache.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a><span data-ttu-id="51bfb-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="51bfb-105">Sample script</span></span>

<span data-ttu-id="51bfb-106">[!code-azurecli[hlavní](../../../cli_scripts/redis-cache/create-cache/create-cache.sh "Azure Redis Cache")]</span><span class="sxs-lookup"><span data-stu-id="51bfb-106">[!code-azurecli[main](../../../cli_scripts/redis-cache/create-cache/create-cache.sh "Azure Redis Cache")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="51bfb-107">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="51bfb-107">Script explanation</span></span>

<span data-ttu-id="51bfb-108">Tento skript používá následující příkazy k vytvoření skupiny prostředků a mezipaměti redis.</span><span class="sxs-lookup"><span data-stu-id="51bfb-108">This script uses the following commands to create a resource group and a redis cache.</span></span> <span data-ttu-id="51bfb-109">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="51bfb-109">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="51bfb-110">Příkaz</span><span class="sxs-lookup"><span data-stu-id="51bfb-110">Command</span></span> | <span data-ttu-id="51bfb-111">Poznámky</span><span class="sxs-lookup"><span data-stu-id="51bfb-111">Notes</span></span> |
|---|---|
| [<span data-ttu-id="51bfb-112">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="51bfb-112">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="51bfb-113">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="51bfb-113">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="51bfb-114">Vytvoření az redis</span><span class="sxs-lookup"><span data-stu-id="51bfb-114">az redis create</span></span>](https://docs.microsoft.com/cli/azure/redis#create) | <span data-ttu-id="51bfb-115">Vytvořte instanci služby Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="51bfb-115">Create Redis Cache instance.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="51bfb-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="51bfb-116">Next steps</span></span>

<span data-ttu-id="51bfb-117">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="51bfb-117">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="51bfb-118">Další ukázky skriptu Azure Redis Cache rozhraní příkazového řádku najdete v [dokumentace k Azure Redis Cache](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="51bfb-118">Additional Azure Redis Cache CLI script samples can be found in the [Azure Redis Cache documentation](../cli-samples.md).</span></span>