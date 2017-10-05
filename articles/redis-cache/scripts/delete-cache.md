---
title: "Ukázka skriptu rozhraní příkazového řádku Azure - odstranění Azure Redis Cache | Microsoft Docs"
description: "Ukázka skriptu rozhraní příkazového řádku Azure - odstranění Azure Redis Cache"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
tags: azure-service-management
ms.assetid: 7beded7a-d2c9-43a6-b3b4-b8079c11de4a
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 04/14/2017
ms.author: sdanie
ms.openlocfilehash: f959823b3a7c5b0262f693ecad1e6efc4eec4f35
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="delete-an-azure-redis-cache"></a><span data-ttu-id="167c0-103">Odstranit Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="167c0-103">Delete an Azure Redis Cache</span></span>

<span data-ttu-id="167c0-104">V tomto scénáři zjistěte, jak odstranit Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="167c0-104">In this scenario, you learn how to delete an Azure Redis Cache.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a><span data-ttu-id="167c0-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="167c0-105">Sample script</span></span>

<span data-ttu-id="167c0-106">[!code-azurecli[hlavní](../../../cli_scripts/redis-cache/delete-cache/delete-cache.sh "Azure Redis Cache")]</span><span class="sxs-lookup"><span data-stu-id="167c0-106">[!code-azurecli[main](../../../cli_scripts/redis-cache/delete-cache/delete-cache.sh "Azure Redis Cache")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="167c0-107">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="167c0-107">Script explanation</span></span>

<span data-ttu-id="167c0-108">Tento skript používá následující příkazy odstranit instanci služby Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="167c0-108">This script uses the following commands to delete an Azure Redis Cache instance.</span></span> <span data-ttu-id="167c0-109">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="167c0-109">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="167c0-110">Příkaz</span><span class="sxs-lookup"><span data-stu-id="167c0-110">Command</span></span> | <span data-ttu-id="167c0-111">Poznámky</span><span class="sxs-lookup"><span data-stu-id="167c0-111">Notes</span></span> |
|---|---|
| [<span data-ttu-id="167c0-112">odstranění az redis</span><span class="sxs-lookup"><span data-stu-id="167c0-112">az redis delete</span></span>](https://docs.microsoft.com/cli/azure/redis#delete) | <span data-ttu-id="167c0-113">Odstraňte instanci služby Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="167c0-113">Delete Redis Cache instance.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="167c0-114">Další kroky</span><span class="sxs-lookup"><span data-stu-id="167c0-114">Next steps</span></span>

<span data-ttu-id="167c0-115">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="167c0-115">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="167c0-116">Další ukázky skriptu Azure Redis Cache rozhraní příkazového řádku najdete v [dokumentace k Azure Redis Cache](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="167c0-116">Additional Azure Redis Cache CLI script samples can be found in the [Azure Redis Cache documentation](../cli-samples.md).</span></span>