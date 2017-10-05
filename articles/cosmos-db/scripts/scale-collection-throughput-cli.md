---
title: "Azure CLI skriptu škálování Azure Cosmos DB kontejneru propustnost | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - propustnost contianer škálování Azure Cosmos DB"
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
ms.openlocfilehash: f08733cd4074c7144b20a0592522423e729e6f1d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="scale-azure-cosmos-db-container-throughput-using-the-azure-cli"></a><span data-ttu-id="261a6-103">Škálování Azure Cosmos DB kontejneru propustnost pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="261a6-103">Scale Azure Cosmos DB container throughput using the Azure CLI</span></span>

<span data-ttu-id="261a6-104">Tato ukázka škáluje propustnost kontejner pro jakýkoli druh kontejner Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="261a6-104">This sample scales container throughput for any kind of Azure Cosmos DB container.</span></span>  

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="261a6-105">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="261a6-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="261a6-106">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="261a6-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="261a6-107">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="261a6-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="261a6-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="261a6-108">Sample script</span></span>

<span data-ttu-id="261a6-109">[!code-azurecli-interactive[hlavní](../../../cli_scripts/cosmosdb/scale-cosmosdb-throughput/scale-cosmosdb-throughput.sh?highlight=40-46 "propustnost škálování Azure Cosmos DB")]</span><span class="sxs-lookup"><span data-stu-id="261a6-109">[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/scale-cosmosdb-throughput/scale-cosmosdb-throughput.sh?highlight=40-46 "Scale Azure Cosmos DB throughput")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="261a6-110">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="261a6-110">Clean up deployment</span></span>

<span data-ttu-id="261a6-111">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="261a6-111">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="261a6-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="261a6-112">Script explanation</span></span>

<span data-ttu-id="261a6-113">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="261a6-113">This script uses the following commands.</span></span> <span data-ttu-id="261a6-114">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="261a6-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="261a6-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="261a6-115">Command</span></span> | <span data-ttu-id="261a6-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="261a6-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="261a6-117">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="261a6-117">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="261a6-118">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="261a6-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="261a6-119">AZ cosmosdb aktualizace</span><span class="sxs-lookup"><span data-stu-id="261a6-119">az cosmosdb update</span></span>](https://docs.microsoft.com/cli/azure/cosmosdb#update) | <span data-ttu-id="261a6-120">Aktualizuje účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="261a6-120">Updates an Azure Cosmos DB account.</span></span> |
| [<span data-ttu-id="261a6-121">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="261a6-121">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="261a6-122">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="261a6-122">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="261a6-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="261a6-123">Next steps</span></span>

<span data-ttu-id="261a6-124">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="261a6-124">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="261a6-125">Další ukázky skript příkazového řádku DB Cosmos Azure lze nalézt v [dokumentace Azure Cosmos DB CLI](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="261a6-125">Additional Azure Cosmos DB CLI script samples can be found in the [Azure Cosmos DB CLI documentation](../cli-samples.md).</span></span>
