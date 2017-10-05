---
title: "Azure CLI skriptu – vytvoření brány firewall pro Azure Cosmos DB | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření brány firewall pro Azure Cosmos DB"
services: cosmos-db
documentationcenter: cosmosdb
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: sammvcple
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
ms.date: 06/02/2017
ms.author: mimig
ms.openlocfilehash: 51f61901e1b1615e48582690dea701a01a56ebca
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-cosmos-db-create-a-firewall-using-the-azure-cli"></a><span data-ttu-id="86069-103">Azure Cosmos DB: Vytvoření brány firewall pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="86069-103">Azure Cosmos DB: Create a firewall using the Azure CLI</span></span>

<span data-ttu-id="86069-104">Tento ukázkový skript rozhraní příkazového řádku vytvoří zásady brány firewall pro jakýkoli druh účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="86069-104">This sample CLI script creates a firewall policy for any kind of Azure Cosmos DB account.</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="86069-105">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="86069-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="86069-106">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="86069-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="86069-107">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="86069-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="86069-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="86069-108">Sample script</span></span>

<span data-ttu-id="86069-109">[!code-azurecli-interactive[hlavní](../../../cli_scripts/cosmosdb/secure-cosmosdb-create-firewall/secure-cosmosdb-create-firewall.sh?highlight=38-42 "vytvoření Azure Cosmos DB brány firewall")]</span><span class="sxs-lookup"><span data-stu-id="86069-109">[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/secure-cosmosdb-create-firewall/secure-cosmosdb-create-firewall.sh?highlight=38-42 "Create an Azure Cosmos DB firewall")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="86069-110">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="86069-110">Clean up deployment</span></span>

<span data-ttu-id="86069-111">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="86069-111">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="86069-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="86069-112">Script explanation</span></span>

<span data-ttu-id="86069-113">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="86069-113">This script uses the following commands.</span></span> <span data-ttu-id="86069-114">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="86069-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="86069-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="86069-115">Command</span></span> | <span data-ttu-id="86069-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="86069-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="86069-117">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="86069-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="86069-118">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="86069-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="86069-119">Vytvoření az cosmosdb</span><span class="sxs-lookup"><span data-stu-id="86069-119">az cosmosdb create</span></span>](https://docs.microsoft.com/cli/azure/cosmosdb#create) | <span data-ttu-id="86069-120">Vytvoří účet Azure CosmosDB.</span><span class="sxs-lookup"><span data-stu-id="86069-120">Creates an Azure CosmosDB account.</span></span> |
| [<span data-ttu-id="86069-121">AZ cosmosdb aktualizace</span><span class="sxs-lookup"><span data-stu-id="86069-121">az cosmosdb update</span></span>](https://docs.microsoft.com/cli/azure/cosmosdb#update) | <span data-ttu-id="86069-122">Aktualizuje účet Azure CosmosDB zahrnout nastavení brány firewall.</span><span class="sxs-lookup"><span data-stu-id="86069-122">Updates an Azure CosmosDB account to include firewall settings.</span></span> |
| [<span data-ttu-id="86069-123">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="86069-123">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="86069-124">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="86069-124">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="86069-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="86069-125">Next steps</span></span>

<span data-ttu-id="86069-126">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="86069-126">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="86069-127">Další ukázky skript příkazového řádku DB Cosmos Azure lze nalézt v [dokumentace Azure Cosmos DB CLI](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="86069-127">Additional Azure Cosmos DB CLI script samples can be found in the [Azure Cosmos DB CLI documentation](../cli-samples.md).</span></span>
