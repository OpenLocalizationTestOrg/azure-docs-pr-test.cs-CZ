---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - Get hello název hostitele, porty a klíče pro Azure Redis Cache | Microsoft Docs"
description: "Azure ukázka skriptu rozhraní příkazového řádku - Get hello hostname, porty a klíče pro instanci služby Azure Redis Cache"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
tags: azure-service-management
ms.assetid: 761eb24e-2ba7-418d-8fc3-431153e69a90
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 04/14/2017
ms.author: sdanie
ms.openlocfilehash: e6e794558087d6568438c439e2bf99fc46eeb8bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-hostname-ports-and-keys-for-azure-redis-cache"></a><span data-ttu-id="edaed-103">Získat název hostitele hello, porty a klíče pro Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="edaed-103">Get hello hostname, ports, and keys for Azure Redis Cache</span></span>

<span data-ttu-id="edaed-104">V tomto scénáři zjistíte, jak používat tooretrieve hello hostname, porty a klíče instanci tooconnect tooan Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="edaed-104">In this scenario, you learn how tooretrieve hello hostname, ports, and keys used tooconnect tooan Azure Redis Cache instance.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a><span data-ttu-id="edaed-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="edaed-105">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/redis-cache/cache-keys-ports/cache-keys-ports.sh "Azure Redis Cache")]


## <a name="script-explanation"></a><span data-ttu-id="edaed-106">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="edaed-106">Script explanation</span></span>

<span data-ttu-id="edaed-107">Tento skript používá hello následující příkazy tooretrieve hello název hostitele, klíče a porty instanci služby Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="edaed-107">This script uses hello following commands tooretrieve hello hostname, keys, and ports of an Azure Redis Cache instance.</span></span> <span data-ttu-id="edaed-108">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="edaed-108">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="edaed-109">Příkaz</span><span class="sxs-lookup"><span data-stu-id="edaed-109">Command</span></span> | <span data-ttu-id="edaed-110">Poznámky</span><span class="sxs-lookup"><span data-stu-id="edaed-110">Notes</span></span> |
|---|---|
| [<span data-ttu-id="edaed-111">Zobrazit az redis</span><span class="sxs-lookup"><span data-stu-id="edaed-111">az redis show</span></span>](https://docs.microsoft.com/cli/azure/redis#show) | <span data-ttu-id="edaed-112">Načtěte podrobnosti o instanci služby Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="edaed-112">Retrieve details of an Azure Redis Cache instance.</span></span> |
| [<span data-ttu-id="edaed-113">AZ redis seznamu klíčů</span><span class="sxs-lookup"><span data-stu-id="edaed-113">az redis list-keys</span></span>](https://docs.microsoft.com/cli/azure/redis#list-keys) | <span data-ttu-id="edaed-114">Načtěte přístupové klíče pro instanci služby Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="edaed-114">Retrieve access keys for an Azure Redis Cache instance.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="edaed-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="edaed-115">Next steps</span></span>

<span data-ttu-id="edaed-116">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="edaed-116">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="edaed-117">Další ukázky skriptu Azure Redis Cache rozhraní příkazového řádku najdete v hello [dokumentace k Azure Redis Cache](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="edaed-117">Additional Azure Redis Cache CLI script samples can be found in hello [Azure Redis Cache documentation](../cli-samples.md).</span></span>
