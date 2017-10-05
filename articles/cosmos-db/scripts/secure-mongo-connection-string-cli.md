---
title: "Azure CLI skriptu Get Azure Cosmos DB připojovací řetězec pro MongoDB aplikace | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - Get Azure Cosmos DB připojovací řetězec pro MongoDB aplikace"
services: cosmos-db
documentationcenter: cosmosdb
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
ms.date: 06/02/2017
ms.author: mimig
ms.openlocfilehash: 916c92cab39306352fdf9dff0e0685fd61832d16
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="get-an-azure-cosmos-db-connection-string-for-mongodb-apps-using-the-azure-cli"></a><span data-ttu-id="d0ce7-103">Získat připojovací řetězec databázi Azure Cosmos pro MongoDB aplikace pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="d0ce7-103">Get an Azure Cosmos DB connection string for MongoDB apps using the Azure CLI</span></span>

<span data-ttu-id="d0ce7-104">Tato ukázka získá připojovací řetězec databázi Cosmos Azure pro MongoDB aplikací pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="d0ce7-104">This sample gets an Azure Cosmos DB connection string for MongoDB apps using the Azure CLI.</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="d0ce7-105">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="d0ce7-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="d0ce7-106">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="d0ce7-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="d0ce7-107">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="d0ce7-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="d0ce7-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="d0ce7-108">Sample script</span></span>

<span data-ttu-id="d0ce7-109">[!code-azurecli-interactive[hlavní](../../../cli_scripts/cosmosdb/secure-cosmosdb-get-mongodb-connection-string/secure-cosmosdb-get-mongodb-connection-string.sh?highlight=36-39 "získání Cosmos DB Azure připojovací řetězec pro MongoDB aplikace")]</span><span class="sxs-lookup"><span data-stu-id="d0ce7-109">[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/secure-cosmosdb-get-mongodb-connection-string/secure-cosmosdb-get-mongodb-connection-string.sh?highlight=36-39 "Get Azure Cosmos DB connection string for MongoDB apps")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="d0ce7-110">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="d0ce7-110">Clean up deployment</span></span>

<span data-ttu-id="d0ce7-111">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="d0ce7-111">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="d0ce7-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="d0ce7-112">Script explanation</span></span>

<span data-ttu-id="d0ce7-113">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="d0ce7-113">This script uses the following commands.</span></span> <span data-ttu-id="d0ce7-114">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="d0ce7-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="d0ce7-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="d0ce7-115">Command</span></span> | <span data-ttu-id="d0ce7-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="d0ce7-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d0ce7-117">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="d0ce7-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="d0ce7-118">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="d0ce7-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="d0ce7-119">AZ cosmosdb aktualizace</span><span class="sxs-lookup"><span data-stu-id="d0ce7-119">az cosmosdb update</span></span>](https://docs.microsoft.com/cli/azure/cosmosdb#update) | <span data-ttu-id="d0ce7-120">Aktualizuje účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d0ce7-120">Updates an Azure Cosmos DB account.</span></span> |
| [<span data-ttu-id="d0ce7-121">AZ cosmosdb seznamu připojení řetězců</span><span class="sxs-lookup"><span data-stu-id="d0ce7-121">az cosmosdb list-connection-strings</span></span>](https://docs.microsoft.com/cli/azure/cosmosdb#list-connection-strings) | <span data-ttu-id="d0ce7-122">Získá připojovací řetězec pro účet.</span><span class="sxs-lookup"><span data-stu-id="d0ce7-122">Gets the connection string for the account.</span></span>|
| [<span data-ttu-id="d0ce7-123">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="d0ce7-123">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="d0ce7-124">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="d0ce7-124">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d0ce7-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d0ce7-125">Next steps</span></span>

<span data-ttu-id="d0ce7-126">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d0ce7-126">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="d0ce7-127">Další ukázky skript příkazového řádku DB Cosmos Azure lze nalézt v [dokumentace Azure Cosmos DB CLI](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="d0ce7-127">Additional Azure Cosmos DB CLI script samples can be found in the [Azure Cosmos DB CLI documentation](../cli-samples.md).</span></span>
