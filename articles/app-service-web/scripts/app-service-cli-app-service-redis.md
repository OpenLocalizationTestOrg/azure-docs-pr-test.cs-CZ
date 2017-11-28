---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - připojení mezipaměti redis webové aplikace tooa | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – připojit mezipaměti redis tooa webové aplikace"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: bc8345b2-8487-40c6-a91f-77414e8688e6
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: b911e6643591b8f07aeb64d4d62876c0fa156a8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-web-app-tooa-redis-cache"></a><span data-ttu-id="407bc-103">Připojit mezipaměti redis tooa webové aplikace</span><span class="sxs-lookup"><span data-stu-id="407bc-103">Connect a web app tooa redis cache</span></span>

<span data-ttu-id="407bc-104">V tomto scénáři se dozvíte, jak toocreate Azure redis cache a webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="407bc-104">In this scenario you will learn how toocreate an Azure redis cache and an Azure web app.</span></span> <span data-ttu-id="407bc-105">Potom propojíte hello redis cache toohello webovou aplikaci pomocí nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="407bc-105">Then you will link hello redis cache toohello web app using app settings.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="407bc-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="407bc-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-redis/connect-to-redis.sh "Azure Redis Cache")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="407bc-107">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="407bc-107">Script explanation</span></span>

<span data-ttu-id="407bc-108">Tento skript používá hello následující příkazy toocreate skupinu prostředků, webové aplikace, redis cache a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="407bc-108">This script uses hello following commands toocreate a resource group, web app, redis cache and all related resources.</span></span> <span data-ttu-id="407bc-109">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="407bc-109">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="407bc-110">Příkaz</span><span class="sxs-lookup"><span data-stu-id="407bc-110">Command</span></span> | <span data-ttu-id="407bc-111">Poznámky</span><span class="sxs-lookup"><span data-stu-id="407bc-111">Notes</span></span> |
|---|---|
| [<span data-ttu-id="407bc-112">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="407bc-112">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="407bc-113">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="407bc-113">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="407bc-114">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="407bc-114">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="407bc-115">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="407bc-115">Creates an App Service plan.</span></span> <span data-ttu-id="407bc-116">Toto je jako serverové farmy pro Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="407bc-116">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="407bc-117">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="407bc-117">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="407bc-118">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="407bc-118">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="407bc-119">Vytvoření az redis</span><span class="sxs-lookup"><span data-stu-id="407bc-119">az redis create</span></span>](https://docs.microsoft.com/en-us/cli/azure/redis#create) | <span data-ttu-id="407bc-120">Vytvořte novou instanci služby Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="407bc-120">Create new Redis Cache instance.</span></span> <span data-ttu-id="407bc-121">Toto je, kde bude uložena hello data.</span><span class="sxs-lookup"><span data-stu-id="407bc-121">This is where hello data will be stored.</span></span> |
| [<span data-ttu-id="407bc-122">AZ redis seznamu klíčů</span><span class="sxs-lookup"><span data-stu-id="407bc-122">az redis list-keys</span></span>](https://docs.microsoft.com/en-us/cli/azure/redis#list-keys) | <span data-ttu-id="407bc-123">Uvádí hello přístupové klíče pro instanci služby redis cache hello.</span><span class="sxs-lookup"><span data-stu-id="407bc-123">Lists hello access keys for hello redis cache instance.</span></span> |
| [<span data-ttu-id="407bc-124">AZ webapp konfigurace appsettings sady</span><span class="sxs-lookup"><span data-stu-id="407bc-124">az webapp config appsettings set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#set) | <span data-ttu-id="407bc-125">Vytvoří nebo aktualizuje nastavení aplikace pro webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="407bc-125">Creates or updates an app setting for an Azure web app.</span></span> <span data-ttu-id="407bc-126">Nastavení aplikace jsou zveřejněné jako proměnné prostředí pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="407bc-126">App settings are exposed as environment variables for your app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="407bc-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="407bc-127">Next steps</span></span>

<span data-ttu-id="407bc-128">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="407bc-128">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="407bc-129">Další ukázky skriptu rozhraní příkazového řádku služby aplikace naleznete v hello [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="407bc-129">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
