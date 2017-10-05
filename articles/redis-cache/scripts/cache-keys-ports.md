---
title: "Ukázka skriptu Azure CLI - Get název hostitele, porty a klíče pro Azure Redis Cache | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - Get název hostitele, porty a klíče pro Azure Redis Cache instance"
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
ms.openlocfilehash: cd9adc784bceb0fff5e7c2bbee2be0950c51c8f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-hostname-ports-and-keys-for-azure-redis-cache"></a><span data-ttu-id="20b1c-103">Získat název hostitele, porty a klíče pro Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="20b1c-103">Get the hostname, ports, and keys for Azure Redis Cache</span></span>

<span data-ttu-id="20b1c-104">V tomto scénáři zjistěte, jak načíst název hostitele, porty a klíče, které slouží k připojení k instanci služby Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="20b1c-104">In this scenario, you learn how to retrieve the hostname, ports, and keys used to connect to an Azure Redis Cache instance.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a><span data-ttu-id="20b1c-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="20b1c-105">Sample script</span></span>

<span data-ttu-id="20b1c-106">[!code-azurecli[hlavní](../../../cli_scripts/redis-cache/cache-keys-ports/cache-keys-ports.sh "Azure Redis Cache")]</span><span class="sxs-lookup"><span data-stu-id="20b1c-106">[!code-azurecli[main](../../../cli_scripts/redis-cache/cache-keys-ports/cache-keys-ports.sh "Azure Redis Cache")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="20b1c-107">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="20b1c-107">Script explanation</span></span>

<span data-ttu-id="20b1c-108">Tento skript používá následující příkazy pro načtení názvu hostitele, klíče a porty instanci služby Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="20b1c-108">This script uses the following commands to retrieve the hostname, keys, and ports of an Azure Redis Cache instance.</span></span> <span data-ttu-id="20b1c-109">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="20b1c-109">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="20b1c-110">Příkaz</span><span class="sxs-lookup"><span data-stu-id="20b1c-110">Command</span></span> | <span data-ttu-id="20b1c-111">Poznámky</span><span class="sxs-lookup"><span data-stu-id="20b1c-111">Notes</span></span> |
|---|---|
| [<span data-ttu-id="20b1c-112">Zobrazit az redis</span><span class="sxs-lookup"><span data-stu-id="20b1c-112">az redis show</span></span>](https://docs.microsoft.com/cli/azure/redis#show) | <span data-ttu-id="20b1c-113">Načtěte podrobnosti o instanci služby Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="20b1c-113">Retrieve details of an Azure Redis Cache instance.</span></span> |
| [<span data-ttu-id="20b1c-114">AZ redis seznamu klíčů</span><span class="sxs-lookup"><span data-stu-id="20b1c-114">az redis list-keys</span></span>](https://docs.microsoft.com/cli/azure/redis#list-keys) | <span data-ttu-id="20b1c-115">Načtěte přístupové klíče pro instanci služby Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="20b1c-115">Retrieve access keys for an Azure Redis Cache instance.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="20b1c-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="20b1c-116">Next steps</span></span>

<span data-ttu-id="20b1c-117">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="20b1c-117">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="20b1c-118">Další ukázky skriptu Azure Redis Cache rozhraní příkazového řádku najdete v [dokumentace k Azure Redis Cache](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="20b1c-118">Additional Azure Redis Cache CLI script samples can be found in the [Azure Redis Cache documentation](../cli-samples.md).</span></span>