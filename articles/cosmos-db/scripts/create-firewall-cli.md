---
title: "rozhraní příkazového řádku skriptu – vytvoření brány firewall pro Azure Cosmos DB aaaAzure | Microsoft Docs"
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
ms.openlocfilehash: d4bee4f37906033c96826b9662d2ba396325c792
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-create-a-firewall-using-hello-azure-cli"></a><span data-ttu-id="a5535-103">Azure Cosmos DB: Vytvoření brány firewall pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="a5535-103">Azure Cosmos DB: Create a firewall using hello Azure CLI</span></span>

<span data-ttu-id="a5535-104">Tento ukázkový skript rozhraní příkazového řádku vytvoří zásady brány firewall pro jakýkoli druh účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a5535-104">This sample CLI script creates a firewall policy for any kind of Azure Cosmos DB account.</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="a5535-105">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="a5535-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="a5535-106">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="a5535-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="a5535-107">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a5535-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="a5535-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="a5535-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/secure-cosmosdb-create-firewall/secure-cosmosdb-create-firewall.sh?highlight=38-42 "Create an Azure Cosmos DB firewall")]

## <a name="clean-up-deployment"></a><span data-ttu-id="a5535-109">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="a5535-109">Clean up deployment</span></span>

<span data-ttu-id="a5535-110">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="a5535-110">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="a5535-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="a5535-111">Script explanation</span></span>

<span data-ttu-id="a5535-112">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="a5535-112">This script uses hello following commands.</span></span> <span data-ttu-id="a5535-113">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="a5535-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="a5535-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="a5535-114">Command</span></span> | <span data-ttu-id="a5535-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="a5535-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="a5535-116">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="a5535-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="a5535-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="a5535-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="a5535-118">Vytvoření az cosmosdb</span><span class="sxs-lookup"><span data-stu-id="a5535-118">az cosmosdb create</span></span>](https://docs.microsoft.com/cli/azure/cosmosdb#create) | <span data-ttu-id="a5535-119">Vytvoří účet Azure CosmosDB.</span><span class="sxs-lookup"><span data-stu-id="a5535-119">Creates an Azure CosmosDB account.</span></span> |
| [<span data-ttu-id="a5535-120">AZ cosmosdb aktualizace</span><span class="sxs-lookup"><span data-stu-id="a5535-120">az cosmosdb update</span></span>](https://docs.microsoft.com/cli/azure/cosmosdb#update) | <span data-ttu-id="a5535-121">Aktualizace nastavení služby Azure CosmosDB tooinclude účtu brány firewall.</span><span class="sxs-lookup"><span data-stu-id="a5535-121">Updates an Azure CosmosDB account tooinclude firewall settings.</span></span> |
| [<span data-ttu-id="a5535-122">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="a5535-122">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="a5535-123">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="a5535-123">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="a5535-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a5535-124">Next steps</span></span>

<span data-ttu-id="a5535-125">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a5535-125">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="a5535-126">Další ukázky skript příkazového řádku DB Cosmos Azure lze nalézt v hello [dokumentace Azure Cosmos DB CLI](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="a5535-126">Additional Azure Cosmos DB CLI script samples can be found in hello [Azure Cosmos DB CLI documentation](../cli-samples.md).</span></span>
