---
title: "Ukázek Azure CLI Redis cache | Microsoft Docs"
description: "Ukázek Azure CLI pro Azure Redis Cache."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 8d2de145-50c0-4f76-bf8f-fdf679f03698
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.date: 04/14/2017
ms.author: sdanie
ms.openlocfilehash: a3debf3380b57faa5b7b30f612698fe6de5b7067
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cli-samples-for-azure-redis-cache"></a><span data-ttu-id="89311-103">Ukázky rozhraní příkazového řádku Azure pro Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="89311-103">Azure CLI Samples for Azure Redis Cache</span></span>

<span data-ttu-id="89311-104">Následující tabulka obsahuje odkazy na bash skripty, které jsou vytvořené pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="89311-104">The following table includes links to bash scripts built using the Azure CLI.</span></span>

| | |
|---|---|
|<span data-ttu-id="89311-105">**Vytvoření mezipaměti**</span><span class="sxs-lookup"><span data-stu-id="89311-105">**Create cache**</span></span>||
| [<span data-ttu-id="89311-106">Vytvoření mezipaměti</span><span class="sxs-lookup"><span data-stu-id="89311-106">Create a cache</span></span>](./scripts/create-cache.md) | <span data-ttu-id="89311-107">Vytvoří skupinu prostředků a základní úroveň Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="89311-107">Creates a resource group and a basic tier Azure Redis Cache.</span></span> |
| [<span data-ttu-id="89311-108">Vytvoření mezipaměti premium s clusteringem</span><span class="sxs-lookup"><span data-stu-id="89311-108">Create a premium cache with clustering</span></span>](./scripts/create-premium-cache-cluster.md) | <span data-ttu-id="89311-109">Vytvoří skupinu prostředků a mezipaměti úroveň premium s povoleným clusteringem.</span><span class="sxs-lookup"><span data-stu-id="89311-109">Creates a resource group and a premium tier cache with clustering enabled.</span></span>|
| [<span data-ttu-id="89311-110">Získat podrobnosti o mezipaměti</span><span class="sxs-lookup"><span data-stu-id="89311-110">Get cache details</span></span>](./scripts/show-cache.md) | <span data-ttu-id="89311-111">Získá podrobnosti o instanci služby Azure Redis Cache, včetně Stav zřizování.</span><span class="sxs-lookup"><span data-stu-id="89311-111">Gets details of an Azure Redis Cache instance, including provisioning status.</span></span> |
| [<span data-ttu-id="89311-112">Získat název hostitele, porty a klíčů</span><span class="sxs-lookup"><span data-stu-id="89311-112">Get the hostname, ports, and keys</span></span>](./scripts/cache-keys-ports.md) | <span data-ttu-id="89311-113">Získá název hostitele, porty a klíče pro instanci služby Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="89311-113">Gets the hostname, ports, and keys for an Azure Redis Cache instance.</span></span> |
|<span data-ttu-id="89311-114">**Webové aplikace a mezipaměti**</span><span class="sxs-lookup"><span data-stu-id="89311-114">**Web app plus cache**</span></span>||
| [<span data-ttu-id="89311-115">Připojení webové aplikace do mezipaměti redis</span><span class="sxs-lookup"><span data-stu-id="89311-115">Connect a web app to a redis cache</span></span>](./../app-service-web/scripts/app-service-cli-app-service-redis.md) | <span data-ttu-id="89311-116">Vytvoří webové aplikace Azure a mezipaměti redis a potom přidá podrobnosti připojení redis nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="89311-116">Creates an Azure web app and a redis cache, then adds the redis connection details to the app settings.</span></span> |
|<span data-ttu-id="89311-117">**Odstranit mezipaměť**</span><span class="sxs-lookup"><span data-stu-id="89311-117">**Delete cache**</span></span>||
| [<span data-ttu-id="89311-118">Odstranit mezipaměť</span><span class="sxs-lookup"><span data-stu-id="89311-118">Delete a cache</span></span>](./scripts/delete-cache.md) | <span data-ttu-id="89311-119">Odstraní instanci služby Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="89311-119">Deletes an Azure Redis Cache instance</span></span>  |
| | |

<span data-ttu-id="89311-120">Další informace o Azure CLI 2.0, naleznete v části [nainstalovat Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) a [Začínáme s Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="89311-120">For more information about Azure CLI 2.0, see [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) and [Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).</span></span>