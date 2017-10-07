---
title: "Odstranit aaaAzure ukázka skriptu rozhraní příkazového řádku - Azure Redis Cache | Microsoft Docs"
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
ms.openlocfilehash: 788277f6464d40fedc597ce7f3041130312c07a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="delete-an-azure-redis-cache"></a><span data-ttu-id="1531f-103">Odstranit Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="1531f-103">Delete an Azure Redis Cache</span></span>

<span data-ttu-id="1531f-104">V tomto scénáři zjistíte, jak toodelete Azure Redis mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1531f-104">In this scenario, you learn how toodelete an Azure Redis Cache.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a><span data-ttu-id="1531f-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="1531f-105">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/redis-cache/delete-cache/delete-cache.sh "Azure Redis Cache")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="1531f-106">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="1531f-106">Script explanation</span></span>

<span data-ttu-id="1531f-107">Tento skript používá hello následující příkazy toodelete instanci služby Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="1531f-107">This script uses hello following commands toodelete an Azure Redis Cache instance.</span></span> <span data-ttu-id="1531f-108">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="1531f-108">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="1531f-109">Příkaz</span><span class="sxs-lookup"><span data-stu-id="1531f-109">Command</span></span> | <span data-ttu-id="1531f-110">Poznámky</span><span class="sxs-lookup"><span data-stu-id="1531f-110">Notes</span></span> |
|---|---|
| [<span data-ttu-id="1531f-111">odstranění az redis</span><span class="sxs-lookup"><span data-stu-id="1531f-111">az redis delete</span></span>](https://docs.microsoft.com/cli/azure/redis#delete) | <span data-ttu-id="1531f-112">Odstraňte instanci služby Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="1531f-112">Delete Redis Cache instance.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="1531f-113">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1531f-113">Next steps</span></span>

<span data-ttu-id="1531f-114">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1531f-114">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="1531f-115">Další ukázky skriptu Azure Redis Cache rozhraní příkazového řádku najdete v hello [dokumentace k Azure Redis Cache](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="1531f-115">Additional Azure Redis Cache CLI script samples can be found in hello [Azure Redis Cache documentation](../cli-samples.md).</span></span>
