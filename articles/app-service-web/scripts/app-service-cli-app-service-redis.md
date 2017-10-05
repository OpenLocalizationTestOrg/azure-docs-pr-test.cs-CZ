---
title: "Azure CLI skriptu ukázkové – webovou aplikaci připojit k mezipaměti redis | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – webovou aplikaci připojit k mezipaměti redis"
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
ms.openlocfilehash: b697c8508a6c3422b6b0d0ca36843a9c884b505f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-a-web-app-to-a-redis-cache"></a><span data-ttu-id="d6fa2-103">Připojení webové aplikace do mezipaměti redis</span><span class="sxs-lookup"><span data-stu-id="d6fa2-103">Connect a web app to a redis cache</span></span>

<span data-ttu-id="d6fa2-104">V tomto scénáři se dozvíte, jak vytvořit webové aplikace Azure a Azure redis cache.</span><span class="sxs-lookup"><span data-stu-id="d6fa2-104">In this scenario you will learn how to create an Azure redis cache and an Azure web app.</span></span> <span data-ttu-id="d6fa2-105">Potom propojíte mezipaměť redis pro webovou aplikaci pomocí nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="d6fa2-105">Then you will link the redis cache to the web app using app settings.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="d6fa2-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="d6fa2-106">Sample script</span></span>

<span data-ttu-id="d6fa2-107">[!code-azurecli-interactive[hlavní](../../../cli_scripts/app-service/connect-to-redis/connect-to-redis.sh "Azure Redis Cache")]</span><span class="sxs-lookup"><span data-stu-id="d6fa2-107">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-redis/connect-to-redis.sh "Azure Redis Cache")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="d6fa2-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="d6fa2-108">Script explanation</span></span>

<span data-ttu-id="d6fa2-109">Tento skript používá následující příkazy k vytvoření skupiny prostředků, webové aplikace, redis cache a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="d6fa2-109">This script uses the following commands to create a resource group, web app, redis cache and all related resources.</span></span> <span data-ttu-id="d6fa2-110">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="d6fa2-110">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="d6fa2-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="d6fa2-111">Command</span></span> | <span data-ttu-id="d6fa2-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="d6fa2-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d6fa2-113">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="d6fa2-113">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="d6fa2-114">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="d6fa2-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="d6fa2-115">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="d6fa2-115">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="d6fa2-116">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="d6fa2-116">Creates an App Service plan.</span></span> <span data-ttu-id="d6fa2-117">Toto je jako serverové farmy pro Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d6fa2-117">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="d6fa2-118">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="d6fa2-118">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="d6fa2-119">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="d6fa2-119">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="d6fa2-120">Vytvoření az redis</span><span class="sxs-lookup"><span data-stu-id="d6fa2-120">az redis create</span></span>](https://docs.microsoft.com/en-us/cli/azure/redis#create) | <span data-ttu-id="d6fa2-121">Vytvořte novou instanci služby Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="d6fa2-121">Create new Redis Cache instance.</span></span> <span data-ttu-id="d6fa2-122">Toto je, kde bude uložena data.</span><span class="sxs-lookup"><span data-stu-id="d6fa2-122">This is where the data will be stored.</span></span> |
| [<span data-ttu-id="d6fa2-123">AZ redis seznamu klíčů</span><span class="sxs-lookup"><span data-stu-id="d6fa2-123">az redis list-keys</span></span>](https://docs.microsoft.com/en-us/cli/azure/redis#list-keys) | <span data-ttu-id="d6fa2-124">Uvádí přístupové klíče pro instanci služby redis cache.</span><span class="sxs-lookup"><span data-stu-id="d6fa2-124">Lists the access keys for the redis cache instance.</span></span> |
| [<span data-ttu-id="d6fa2-125">AZ webapp konfigurace appsettings sady</span><span class="sxs-lookup"><span data-stu-id="d6fa2-125">az webapp config appsettings set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#set) | <span data-ttu-id="d6fa2-126">Vytvoří nebo aktualizuje nastavení aplikace pro webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="d6fa2-126">Creates or updates an app setting for an Azure web app.</span></span> <span data-ttu-id="d6fa2-127">Nastavení aplikace jsou zveřejněné jako proměnné prostředí pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d6fa2-127">App settings are exposed as environment variables for your app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d6fa2-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d6fa2-128">Next steps</span></span>

<span data-ttu-id="d6fa2-129">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d6fa2-129">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="d6fa2-130">Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="d6fa2-130">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
