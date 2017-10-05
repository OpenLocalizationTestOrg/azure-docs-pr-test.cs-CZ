---
title: "Ukázka skriptu Azure CLI - získáte podrobnosti Azure Redis Cache | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - získáte podrobnosti Azure Redis Cache"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
tags: azure-service-management
ms.assetid: 155924e6-00d5-4a8c-ba99-5189f300464a
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 04/14/2017
ms.author: sdanie
ms.openlocfilehash: 9f4eb32227bd8a68837eabd58b9d058bc4995d17
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-details-of-an-azure-redis-cache"></a><span data-ttu-id="f3b9c-103">Podrobnosti o Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="f3b9c-103">Get details of an Azure Redis Cache</span></span>

<span data-ttu-id="f3b9c-104">V tomto scénáři zjistěte, jak načíst podrobnosti o instanci služby Azure Redis Cache, včetně jeho stav zřizování.</span><span class="sxs-lookup"><span data-stu-id="f3b9c-104">In this scenario, you learn how to retrieve the details of an Azure Redis Cache instance, including its provisioning status.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a><span data-ttu-id="f3b9c-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="f3b9c-105">Sample script</span></span>

<span data-ttu-id="f3b9c-106">[!code-azurecli[hlavní](../../../cli_scripts/redis-cache/show-cache/show-cache.sh "Azure Redis Cache")]</span><span class="sxs-lookup"><span data-stu-id="f3b9c-106">[!code-azurecli[main](../../../cli_scripts/redis-cache/show-cache/show-cache.sh "Azure Redis Cache")]</span></span>

## <a name="script-explanation"></a><span data-ttu-id="f3b9c-107">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="f3b9c-107">Script explanation</span></span>

<span data-ttu-id="f3b9c-108">Tento skript používá následující příkazy k načtení podrobností o instanci služby Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="f3b9c-108">This script uses the following commands to retrieve the details of an Azure Redis Cache instance.</span></span> <span data-ttu-id="f3b9c-109">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="f3b9c-109">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="f3b9c-110">Příkaz</span><span class="sxs-lookup"><span data-stu-id="f3b9c-110">Command</span></span> | <span data-ttu-id="f3b9c-111">Poznámky</span><span class="sxs-lookup"><span data-stu-id="f3b9c-111">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f3b9c-112">Zobrazit az redis</span><span class="sxs-lookup"><span data-stu-id="f3b9c-112">az redis show</span></span>](https://docs.microsoft.com/cli/azure/redis#show) | <span data-ttu-id="f3b9c-113">Načtěte podrobnosti o instanci služby Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="f3b9c-113">Retrieve details of an Azure Redis Cache instance.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="f3b9c-114">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f3b9c-114">Next steps</span></span>

<span data-ttu-id="f3b9c-115">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f3b9c-115">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="f3b9c-116">Další ukázky skriptu Azure Redis Cache rozhraní příkazového řádku najdete v [dokumentace k Azure Redis Cache](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="f3b9c-116">Additional Azure Redis Cache CLI script samples can be found in the [Azure Redis Cache documentation](../cli-samples.md).</span></span>