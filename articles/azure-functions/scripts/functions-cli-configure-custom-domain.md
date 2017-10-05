---
title: "Ukázka skriptu Azure CLI - mapy vlastní domény do aplikaci funkce | Microsoft Docs"
description: "Azure CLI ukázka skriptu - mapy vlastní doménu pro funkce aplikace v Azure."
services: functions
documentationcenter: 
author: ggailey777
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: d127e347-7581-47d7-b289-e0f51f2fbfbc
ms.service: functions
ms.workload: na
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/01/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 6fcea6d32f9dd25b0fafb4f895f60d8320ac9df8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="map-a-custom-domain-to-a-function-app"></a><span data-ttu-id="44d32-103">Namapovat vlastní doménu aplikace pro funkce</span><span class="sxs-lookup"><span data-stu-id="44d32-103">Map a custom domain to a function app</span></span>

<span data-ttu-id="44d32-104">Tento ukázkový skript vytvoří aplikaci funkce s související prostředky a potom mapuje `www.<yourdomain>` k němu.</span><span class="sxs-lookup"><span data-stu-id="44d32-104">This sample script creates a function app with related resources, and then maps `www.<yourdomain>` to it.</span></span> <span data-ttu-id="44d32-105">Funkce aplikace k mapování na vlastní doménu, musí být vytvořený v plán služby App Service a nejsou v plánu spotřeby.</span><span class="sxs-lookup"><span data-stu-id="44d32-105">To map to a custom domain, your function app must be created in an App Service plan and not in a consumption plan.</span></span> <span data-ttu-id="44d32-106">Azure Functions podporuje pouze mapování vlastní doménu pomocí záznam.</span><span class="sxs-lookup"><span data-stu-id="44d32-106">Azure Functions only supports mapping a custom domain using an A record.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="44d32-107">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="44d32-107">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="44d32-108">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="44d32-108">Run `az --version` to find the version.</span></span> <span data-ttu-id="44d32-109">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="44d32-109">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="sample-script"></a><span data-ttu-id="44d32-110">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="44d32-110">Sample script</span></span>

<span data-ttu-id="44d32-111">[!code-azurecli-interactive[hlavní](../../../cli_scripts/azure-functions/configure-custom-domain/configure-custom-domain.sh?highlight=3 "namapovat vlastní doménu aplikace pro funkce")]</span><span class="sxs-lookup"><span data-stu-id="44d32-111">[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-custom-domain/configure-custom-domain.sh?highlight=3 "Map a custom domain to a function app")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="44d32-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="44d32-112">Script explanation</span></span>

<span data-ttu-id="44d32-113">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="44d32-113">This script uses the following commands.</span></span> <span data-ttu-id="44d32-114">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="44d32-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="44d32-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="44d32-115">Command</span></span> | <span data-ttu-id="44d32-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="44d32-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="44d32-117">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="44d32-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="44d32-118">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="44d32-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="44d32-119">Vytvořit účet úložiště az</span><span class="sxs-lookup"><span data-stu-id="44d32-119">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account#create) | <span data-ttu-id="44d32-120">Vytvoří účet úložiště vyžaduje aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="44d32-120">Creates a storage account required by the function app.</span></span> |
| [<span data-ttu-id="44d32-121">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="44d32-121">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="44d32-122">Vytvoří plán služby App Service potřeba namapovat vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="44d32-122">Creates an App Service plan required to map a custom domain.</span></span> |
| [<span data-ttu-id="44d32-123">Vytvoření az functionapp</span><span class="sxs-lookup"><span data-stu-id="44d32-123">az functionapp create</span></span>]() | <span data-ttu-id="44d32-124">Vytvoří aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="44d32-124">Creates a function app.</span></span> |
| [<span data-ttu-id="44d32-125">Přidat az název hostitele konfigurace webové služby App Service</span><span class="sxs-lookup"><span data-stu-id="44d32-125">az appservice web config hostname add</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#add) | <span data-ttu-id="44d32-126">Vlastní doména se mapuje na aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="44d32-126">Maps a custom domain to a function app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="44d32-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="44d32-127">Next steps</span></span>

<span data-ttu-id="44d32-128">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="44d32-128">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="44d32-129">Další ukázky skriptu funkce rozhraní příkazového řádku najdete v [dokumentace Azure Functions]().</span><span class="sxs-lookup"><span data-stu-id="44d32-129">Additional Functions CLI script samples can be found in the [Azure Functions documentation]().</span></span>
