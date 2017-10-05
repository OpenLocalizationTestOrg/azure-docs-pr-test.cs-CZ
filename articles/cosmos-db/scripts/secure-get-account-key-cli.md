---
title: "Účet Azure CLI skriptu-Get klíče pro Azure Cosmos DB | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - Get klíče účtu pro Azure Cosmos DB"
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
ms.openlocfilehash: 3df211cdc8878033c8b792da00cce9773ae57a36
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="get-account-keys-for-azure-cosmos-db-using-the-azure-cli"></a><span data-ttu-id="8456c-103">Získání klíče účtu pro Azure DB Cosmos pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="8456c-103">Get account keys for Azure Cosmos DB using the Azure CLI</span></span>

<span data-ttu-id="8456c-104">Tato ukázka získá klíče účtu pro jakýkoli druh účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="8456c-104">This sample gets account keys for any kind of Azure Cosmos DB account.</span></span>  

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="8456c-105">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="8456c-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="8456c-106">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="8456c-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="8456c-107">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="8456c-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="8456c-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="8456c-108">Sample script</span></span>

<span data-ttu-id="8456c-109">[!code-azurecli-interactive[hlavní](../../../cli_scripts/cosmosdb/scale-cosmosdb-get-account-key/secure-cosmosdb-get-account-key.sh?highlight=22-25 "klíče účtu získání Cosmos databázi Azure")]</span><span class="sxs-lookup"><span data-stu-id="8456c-109">[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/scale-cosmosdb-get-account-key/secure-cosmosdb-get-account-key.sh?highlight=22-25 "Get Azure Cosmos DB account keys")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="8456c-110">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="8456c-110">Clean up deployment</span></span>

<span data-ttu-id="8456c-111">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="8456c-111">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="8456c-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="8456c-112">Script explanation</span></span>

<span data-ttu-id="8456c-113">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="8456c-113">This script uses the following commands.</span></span> <span data-ttu-id="8456c-114">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="8456c-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="8456c-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="8456c-115">Command</span></span> | <span data-ttu-id="8456c-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="8456c-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="8456c-117">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="8456c-117">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="8456c-118">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="8456c-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="8456c-119">AZ cosmosdb aktualizace</span><span class="sxs-lookup"><span data-stu-id="8456c-119">az cosmosdb update</span></span>](https://docs.microsoft.com/cli/azure/cosmosdb#update) | <span data-ttu-id="8456c-120">Aktualizuje účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="8456c-120">Updates an Azure Cosmos DB account.</span></span> |
| [<span data-ttu-id="8456c-121">AZ cosmosdb seznamu klíčů</span><span class="sxs-lookup"><span data-stu-id="8456c-121">az cosmosdb list-keys</span></span>](https://docs.microsoft.com/cli/azure/cosmosdb#list-keys) | <span data-ttu-id="8456c-122">Vytvoří logického serveru, který je hostitelem databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="8456c-122">Creates a logical server that hosts the SQL Database.</span></span> |
| [<span data-ttu-id="8456c-123">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="8456c-123">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="8456c-124">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="8456c-124">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="8456c-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8456c-125">Next steps</span></span>

<span data-ttu-id="8456c-126">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8456c-126">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="8456c-127">Další ukázky skript příkazového řádku DB Cosmos Azure lze nalézt v [dokumentace Azure Cosmos DB CLI](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="8456c-127">Additional Azure Cosmos DB CLI script samples can be found in the [Azure Cosmos DB CLI documentation](../cli-samples.md).</span></span>
