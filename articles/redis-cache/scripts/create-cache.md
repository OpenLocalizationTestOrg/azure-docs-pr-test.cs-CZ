---
title: "Vytvoření aaaAzure ukázka skriptu rozhraní příkazového řádku - Azure Redis Cache | Microsoft Docs"
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
ms.openlocfilehash: 85b007a426fbd4752034ec8663835963d140dd75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-redis-cache"></a><span data-ttu-id="91a12-103">Vytvoření Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="91a12-103">Create an Azure Redis Cache</span></span>

<span data-ttu-id="91a12-104">V tomto scénáři zjistíte, jak toocreate Azure Redis mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="91a12-104">In this scenario, you learn how toocreate an Azure Redis Cache.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a><span data-ttu-id="91a12-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="91a12-105">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/redis-cache/create-cache/create-cache.sh "Azure Redis Cache")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="91a12-106">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="91a12-106">Script explanation</span></span>

<span data-ttu-id="91a12-107">Tento skript používá následující příkazy toocreate hello skupinu prostředků a mezipaměti redis.</span><span class="sxs-lookup"><span data-stu-id="91a12-107">This script uses hello following commands toocreate a resource group and a redis cache.</span></span> <span data-ttu-id="91a12-108">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="91a12-108">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="91a12-109">Příkaz</span><span class="sxs-lookup"><span data-stu-id="91a12-109">Command</span></span> | <span data-ttu-id="91a12-110">Poznámky</span><span class="sxs-lookup"><span data-stu-id="91a12-110">Notes</span></span> |
|---|---|
| [<span data-ttu-id="91a12-111">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="91a12-111">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="91a12-112">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="91a12-112">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="91a12-113">Vytvoření az redis</span><span class="sxs-lookup"><span data-stu-id="91a12-113">az redis create</span></span>](https://docs.microsoft.com/cli/azure/redis#create) | <span data-ttu-id="91a12-114">Vytvořte instanci služby Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="91a12-114">Create Redis Cache instance.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="91a12-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="91a12-115">Next steps</span></span>

<span data-ttu-id="91a12-116">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="91a12-116">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="91a12-117">Další ukázky skriptu Azure Redis Cache rozhraní příkazového řádku najdete v hello [dokumentace k Azure Redis Cache](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="91a12-117">Additional Azure Redis Cache CLI script samples can be found in hello [Azure Redis Cache documentation](../cli-samples.md).</span></span>
