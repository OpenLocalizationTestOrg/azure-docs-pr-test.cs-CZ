---
title: "Vytvoření skriptu rozhraní příkazového řádku aaaAzure účet Azure Cosmos DB MongoDB rozhraní API, databáze a kolekce | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření účtu Azure Cosmos DB MongoDB rozhraní API, databáze a kolekce."
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
ms.openlocfilehash: 84aec7234ef8906ec9ecf87f8da58753fdb5083d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-create-an-mongodb-api-account-using-hello-azure-cli"></a><span data-ttu-id="db034-103">Azure Cosmos DB: Vytvoření účtu MongoDB rozhraní API pomocí rozhraní příkazového řádku Azure hello</span><span class="sxs-lookup"><span data-stu-id="db034-103">Azure Cosmos DB: Create an MongoDB API account using hello Azure CLI</span></span>

<span data-ttu-id="db034-104">Tento ukázkový skript rozhraní příkazového řádku vytvoří účet Azure Cosmos DB MongoDB rozhraní API, databáze a kolekce.</span><span class="sxs-lookup"><span data-stu-id="db034-104">This sample CLI script creates an Azure Cosmos DB MongoDB API account, database, and collection.</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="db034-105">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="db034-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="db034-106">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="db034-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="db034-107">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="db034-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="db034-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="db034-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/create-cosmosdb-mongodb-account/create-cosmosdb-mongodb-account.sh?highlight=15-35 "Create an Azure Cosmos DB MongoDB API account, database, and collection")]

## <a name="clean-up-deployment"></a><span data-ttu-id="db034-109">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="db034-109">Clean up deployment</span></span>

<span data-ttu-id="db034-110">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="db034-110">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="db034-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="db034-111">Script explanation</span></span>

<span data-ttu-id="db034-112">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="db034-112">This script uses hello following commands.</span></span> <span data-ttu-id="db034-113">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="db034-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="db034-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="db034-114">Command</span></span> | <span data-ttu-id="db034-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="db034-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="db034-116">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="db034-116">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="db034-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="db034-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="db034-118">Vytvoření az cosmosdb</span><span class="sxs-lookup"><span data-stu-id="db034-118">az cosmosdb create</span></span>](/cli/azure/cosmosdb#create) | <span data-ttu-id="db034-119">Vytvoří účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="db034-119">Creates an Azure Cosmos DB account.</span></span> |
| [<span data-ttu-id="db034-120">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="db034-120">az group delete</span></span>](/cli/azure/resource#delete) | <span data-ttu-id="db034-121">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="db034-121">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="db034-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="db034-122">Next steps</span></span>

<span data-ttu-id="db034-123">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="db034-123">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="db034-124">Další ukázky skript příkazového řádku DB Cosmos Azure lze nalézt v hello [dokumentace Azure Cosmos DB CLI](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="db034-124">Additional Azure Cosmos DB CLI script samples can be found in hello [Azure Cosmos DB CLI documentation](../cli-samples.md).</span></span>
