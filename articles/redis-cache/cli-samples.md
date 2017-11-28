---
title: "aaaAzure rozhraní příkazového řádku Redis cache ukázky | Microsoft Docs"
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
ms.openlocfilehash: a2eb6f7267951faca599a43ff29b46beb05fea6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cli-samples-for-azure-redis-cache"></a><span data-ttu-id="d8129-103">Ukázky rozhraní příkazového řádku Azure pro Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="d8129-103">Azure CLI Samples for Azure Redis Cache</span></span>

<span data-ttu-id="d8129-104">Hello následující tabulka obsahuje odkazy toobash skripty vyvíjené hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="d8129-104">hello following table includes links toobash scripts built using hello Azure CLI.</span></span>

| | |
|---|---|
|<span data-ttu-id="d8129-105">**Vytvoření mezipaměti**</span><span class="sxs-lookup"><span data-stu-id="d8129-105">**Create cache**</span></span>||
| [<span data-ttu-id="d8129-106">Vytvoření mezipaměti</span><span class="sxs-lookup"><span data-stu-id="d8129-106">Create a cache</span></span>](./scripts/create-cache.md) | <span data-ttu-id="d8129-107">Vytvoří skupinu prostředků a základní úroveň Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="d8129-107">Creates a resource group and a basic tier Azure Redis Cache.</span></span> |
| [<span data-ttu-id="d8129-108">Vytvoření mezipaměti premium s clusteringem</span><span class="sxs-lookup"><span data-stu-id="d8129-108">Create a premium cache with clustering</span></span>](./scripts/create-premium-cache-cluster.md) | <span data-ttu-id="d8129-109">Vytvoří skupinu prostředků a mezipaměti úroveň premium s povoleným clusteringem.</span><span class="sxs-lookup"><span data-stu-id="d8129-109">Creates a resource group and a premium tier cache with clustering enabled.</span></span>|
| [<span data-ttu-id="d8129-110">Získat podrobnosti o mezipaměti</span><span class="sxs-lookup"><span data-stu-id="d8129-110">Get cache details</span></span>](./scripts/show-cache.md) | <span data-ttu-id="d8129-111">Získá podrobnosti o instanci služby Azure Redis Cache, včetně Stav zřizování.</span><span class="sxs-lookup"><span data-stu-id="d8129-111">Gets details of an Azure Redis Cache instance, including provisioning status.</span></span> |
| [<span data-ttu-id="d8129-112">Získat název hostitele hello, porty a klíčů</span><span class="sxs-lookup"><span data-stu-id="d8129-112">Get hello hostname, ports, and keys</span></span>](./scripts/cache-keys-ports.md) | <span data-ttu-id="d8129-113">Získá název hostitele hello, porty a klíče pro instanci služby Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="d8129-113">Gets hello hostname, ports, and keys for an Azure Redis Cache instance.</span></span> |
|<span data-ttu-id="d8129-114">**Webové aplikace a mezipaměti**</span><span class="sxs-lookup"><span data-stu-id="d8129-114">**Web app plus cache**</span></span>||
| [<span data-ttu-id="d8129-115">Připojit mezipaměti redis tooa webové aplikace</span><span class="sxs-lookup"><span data-stu-id="d8129-115">Connect a web app tooa redis cache</span></span>](./../app-service-web/scripts/app-service-cli-app-service-redis.md) | <span data-ttu-id="d8129-116">Vytvoří webové aplikace Azure a mezipaměti redis a potom přidá hello redis připojení podrobnosti toohello nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="d8129-116">Creates an Azure web app and a redis cache, then adds hello redis connection details toohello app settings.</span></span> |
|<span data-ttu-id="d8129-117">**Odstranit mezipaměť**</span><span class="sxs-lookup"><span data-stu-id="d8129-117">**Delete cache**</span></span>||
| [<span data-ttu-id="d8129-118">Odstranit mezipaměť</span><span class="sxs-lookup"><span data-stu-id="d8129-118">Delete a cache</span></span>](./scripts/delete-cache.md) | <span data-ttu-id="d8129-119">Odstraní instanci služby Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="d8129-119">Deletes an Azure Redis Cache instance</span></span>  |
| | |

<span data-ttu-id="d8129-120">Další informace o Azure CLI 2.0, naleznete v části [nainstalovat Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) a [Začínáme s Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="d8129-120">For more information about Azure CLI 2.0, see [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) and [Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).</span></span>