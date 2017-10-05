---
title: "Azure CLI skriptu ukázkové – webovou aplikaci připojit k databázi Cosmos | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – webovou aplikaci připojit k databázi systému Cosmos"
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
ms.openlocfilehash: ff5e7a794033cc51120831e09b055a86affb28a4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-a-web-app-to-cosmos-db"></a><span data-ttu-id="ed2ba-103">Webovou aplikaci připojit k databázi systému Cosmos</span><span class="sxs-lookup"><span data-stu-id="ed2ba-103">Connect a web app to Cosmos DB</span></span>

<span data-ttu-id="ed2ba-104">V tomto scénáři se dozvíte, jak vytvořit účet Azure Cosmos DB a webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="ed2ba-104">In this scenario you will learn how to create an Azure Cosmos DB account and an Azure web app.</span></span> <span data-ttu-id="ed2ba-105">Potom propojíte Cosmos databáze pro webovou aplikaci pomocí nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="ed2ba-105">Then you will link the Cosmos DB to the web app using app settings.</span></span>


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="ed2ba-106">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ed2ba-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="ed2ba-107">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="ed2ba-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="ed2ba-108">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ed2ba-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="ed2ba-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="ed2ba-109">Sample script</span></span>

<span data-ttu-id="ed2ba-110">[!code-azurecli-interactive[hlavní](../../../cli_scripts/app-service/connect-to-documentdb/connect-to-documentdb.sh "Azure Cosmos DB")]</span><span class="sxs-lookup"><span data-stu-id="ed2ba-110">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-documentdb/connect-to-documentdb.sh "Azure Cosmos DB")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="ed2ba-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="ed2ba-111">Script explanation</span></span>

<span data-ttu-id="ed2ba-112">Tento skript používá následující příkazy k vytvoření skupiny prostředků, webové aplikace, Cosmos DB a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="ed2ba-112">This script uses the following commands to create a resource group, web app, Cosmos DB and all related resources.</span></span> <span data-ttu-id="ed2ba-113">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="ed2ba-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="ed2ba-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="ed2ba-114">Command</span></span> | <span data-ttu-id="ed2ba-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="ed2ba-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="ed2ba-116">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="ed2ba-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="ed2ba-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="ed2ba-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="ed2ba-118">Vytvořit plán aplikační služby az</span><span class="sxs-lookup"><span data-stu-id="ed2ba-118">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="ed2ba-119">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="ed2ba-119">Creates an App Service plan.</span></span> <span data-ttu-id="ed2ba-120">Toto je jako serverové farmy pro Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ed2ba-120">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="ed2ba-121">Vytvoření az webapp</span><span class="sxs-lookup"><span data-stu-id="ed2ba-121">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="ed2ba-122">Vytvoří webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="ed2ba-122">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="ed2ba-123">Vytvoření az cosmosdb</span><span class="sxs-lookup"><span data-stu-id="ed2ba-123">az cosmosdb create</span></span>](https://docs.microsoft.com/en-us/cli/azure/cosmosdb#create) | <span data-ttu-id="ed2ba-124">Vytvoří účet Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ed2ba-124">Creates a Cosmos DB account.</span></span> <span data-ttu-id="ed2ba-125">Toto je, kde bude uložena data.</span><span class="sxs-lookup"><span data-stu-id="ed2ba-125">This is where the data will be stored.</span></span> |
| [<span data-ttu-id="ed2ba-126">AZ cosmosdb seznamu klíčů</span><span class="sxs-lookup"><span data-stu-id="ed2ba-126">az cosmosdb list-keys</span></span>](https://docs.microsoft.com/en-us/cli/azure/cosmosdb#list-keys) | <span data-ttu-id="ed2ba-127">Uvádí přístupové klíče pro zadaný účet Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ed2ba-127">Lists the access keys for the specified Cosmos DB account.</span></span> |
| [<span data-ttu-id="ed2ba-128">AZ webapp konfigurace appsettings sady</span><span class="sxs-lookup"><span data-stu-id="ed2ba-128">az webapp config appsettings set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#set) | <span data-ttu-id="ed2ba-129">Vytvoří nebo aktualizuje nastavení aplikace pro webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="ed2ba-129">Creates or updates an app setting for an Azure web app.</span></span> <span data-ttu-id="ed2ba-130">Nastavení aplikace jsou zveřejněné jako proměnné prostředí pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ed2ba-130">App settings are exposed as environment variables for your app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ed2ba-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ed2ba-131">Next steps</span></span>

<span data-ttu-id="ed2ba-132">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ed2ba-132">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="ed2ba-133">Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [dokumentaci služby Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="ed2ba-133">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
