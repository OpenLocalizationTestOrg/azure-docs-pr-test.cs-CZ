---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku – připojení webové aplikaci tooCosmos DB | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – připojení webové aplikaci tooCosmos DB"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: bbbdbc42-efb5-4b4f-8ba6-c03c9d16a7ea
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 1f2123378b9d5812fa793730f7fa5a5bc9ab63c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-web-app-toocosmos-db"></a><span data-ttu-id="093b8-103">Připojení webové aplikaci tooCosmos DB</span><span class="sxs-lookup"><span data-stu-id="093b8-103">Connect a web app tooCosmos DB</span></span>

<span data-ttu-id="093b8-104">V tomto scénáři se dozvíte, jak toocreate účet Azure Cosmos DB a Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="093b8-104">In this scenario you will learn how toocreate an Azure Cosmos DB account and an Azure web app.</span></span> <span data-ttu-id="093b8-105">Potom propojíte hello Cosmos DB toohello webovou aplikaci pomocí nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="093b8-105">Then you will link hello Cosmos DB toohello web app using app settings.</span></span>


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="093b8-106">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="093b8-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="093b8-107">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="093b8-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="093b8-108">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="093b8-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="093b8-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="093b8-109">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-documentdb/connect-to-documentdb.sh "Azure Cosmos DB")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="093b8-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="093b8-110">Script explanation</span></span>

<span data-ttu-id="093b8-111">Tento skript používá hello následující příkazy toocreate skupinu prostředků, webové aplikace, databáze Cosmos a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="093b8-111">This script uses hello following commands toocreate a resource group, web app, Cosmos DB and all related resources.</span></span> <span data-ttu-id="093b8-112">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="093b8-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="093b8-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="093b8-113">Command</span></span> | <span data-ttu-id="093b8-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="093b8-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="093b8-115">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="093b8-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="093b8-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="093b8-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="093b8-117">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="093b8-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="093b8-118">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="093b8-118">Creates an App Service plan.</span></span> <span data-ttu-id="093b8-119">Toto je jako serverové farmy pro Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="093b8-119">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="093b8-120">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="093b8-120">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="093b8-121">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="093b8-121">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="093b8-122">Vytvoření az cosmosdb</span><span class="sxs-lookup"><span data-stu-id="093b8-122">az cosmosdb create</span></span>](https://docs.microsoft.com/en-us/cli/azure/cosmosdb#create) | <span data-ttu-id="093b8-123">Vytvoří účet Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="093b8-123">Creates a Cosmos DB account.</span></span> <span data-ttu-id="093b8-124">Toto je, kde bude uložena hello data.</span><span class="sxs-lookup"><span data-stu-id="093b8-124">This is where hello data will be stored.</span></span> |
| [<span data-ttu-id="093b8-125">AZ cosmosdb seznamu klíčů</span><span class="sxs-lookup"><span data-stu-id="093b8-125">az cosmosdb list-keys</span></span>](https://docs.microsoft.com/en-us/cli/azure/cosmosdb#list-keys) | <span data-ttu-id="093b8-126">Seznamy hello přístupové klíče pro hello zadali účet Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="093b8-126">Lists hello access keys for hello specified Cosmos DB account.</span></span> |
| [<span data-ttu-id="093b8-127">AZ webapp konfigurace appsettings sady</span><span class="sxs-lookup"><span data-stu-id="093b8-127">az webapp config appsettings set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#set) | <span data-ttu-id="093b8-128">Vytvoří nebo aktualizuje nastavení aplikace pro webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="093b8-128">Creates or updates an app setting for an Azure web app.</span></span> <span data-ttu-id="093b8-129">Nastavení aplikace jsou zveřejněné jako proměnné prostředí pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="093b8-129">App settings are exposed as environment variables for your app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="093b8-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="093b8-130">Next steps</span></span>

<span data-ttu-id="093b8-131">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="093b8-131">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="093b8-132">Další ukázky skriptu rozhraní příkazového řádku služby aplikace naleznete v hello [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="093b8-132">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
