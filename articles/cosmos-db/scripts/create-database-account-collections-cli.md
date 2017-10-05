---
title: "Azure CLI skriptu-vytvořit účet rozhraní API služby Azure Cosmos databáze DocumentDB, databázi a kolekci | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření účtu Azure Cosmos databáze DocumentDB rozhraní API, databáze a kolekce"
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
ms.date: 06/06/2017
ms.author: mimig
ms.openlocfilehash: 28f99d56404e47adcd375d9f3106cc234469cbfd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-create-an-documentdb-api-account-using-cli"></a><span data-ttu-id="b6347-103">Azure Cosmos DB: Vytvoření účtu DocumentDB rozhraní API pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="b6347-103">Azure Cosmos DB: Create an DocumentDB API account using CLI</span></span>

<span data-ttu-id="b6347-104">Tento ukázkový skript rozhraní příkazového řádku vytvoří účet rozhraní API služby Azure Cosmos databáze DocumentDB, databázi a kolekci.</span><span class="sxs-lookup"><span data-stu-id="b6347-104">This sample CLI script creates an Azure Cosmos DB DocumentDB API account, database, and collection.</span></span>  

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="b6347-105">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="b6347-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="b6347-106">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="b6347-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="b6347-107">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b6347-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="b6347-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="b6347-108">Sample script</span></span>

<span data-ttu-id="b6347-109">[!code-azurecli-interactive[hlavní](../../../cli_scripts/cosmosdb/create-cosmosdb-account-database/create-cosmosdb-account-database.sh?highlight=15-35 "vytvoření účtu Azure Cosmos databáze DocumentDB rozhraní API, databáze a kolekce")]</span><span class="sxs-lookup"><span data-stu-id="b6347-109">[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/create-cosmosdb-account-database/create-cosmosdb-account-database.sh?highlight=15-35 "Create an Azure Cosmos DB DocumentDB API account, database, and collection")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="b6347-110">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="b6347-110">Clean up deployment</span></span>

<span data-ttu-id="b6347-111">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="b6347-111">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="b6347-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="b6347-112">Script explanation</span></span>

<span data-ttu-id="b6347-113">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="b6347-113">This script uses the following commands.</span></span> <span data-ttu-id="b6347-114">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="b6347-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="b6347-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="b6347-115">Command</span></span> | <span data-ttu-id="b6347-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="b6347-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="b6347-117">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="b6347-117">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="b6347-118">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="b6347-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="b6347-119">Vytvoření az cosmosdb</span><span class="sxs-lookup"><span data-stu-id="b6347-119">az cosmosdb create</span></span>](/cli/azure/cosmosdb#create) | <span data-ttu-id="b6347-120">Vytvoří účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b6347-120">Creates an Azure Cosmos DB account.</span></span> |
| [<span data-ttu-id="b6347-121">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="b6347-121">az group delete</span></span>](/cli/azure/resource#delete) | <span data-ttu-id="b6347-122">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="b6347-122">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b6347-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b6347-123">Next steps</span></span>

<span data-ttu-id="b6347-124">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b6347-124">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="b6347-125">Další ukázky skript příkazového řádku DB Cosmos Azure lze nalézt v [dokumentace Azure Cosmos DB CLI](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="b6347-125">Additional Azure Cosmos DB CLI script samples can be found in the [Azure Cosmos DB CLI documentation](../cli-samples.md).</span></span>
