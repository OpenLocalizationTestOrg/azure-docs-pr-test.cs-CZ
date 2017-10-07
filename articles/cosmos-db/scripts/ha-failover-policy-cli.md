---
title: "rozhraní příkazového řádku skriptu-vytvořit zásadu převzetí služeb při selhání pro zajištění vysoké dostupnosti aaaAzure | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvořte zásadu převzetí služeb při selhání pro zajištění vysoké dostupnosti"
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
ms.openlocfilehash: 9076f4ef23fceb4208c934c57ac6899f0b58ffd3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-failover-policy-for-high-availability-using-hello-azure-cli"></a><span data-ttu-id="66830-103">Vytvořit zásadu převzetí služeb při selhání pro zajištění vysoké dostupnosti pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="66830-103">Create a failover policy for high availability using hello Azure CLI</span></span>

<span data-ttu-id="66830-104">Tento ukázkový skript rozhraní příkazového řádku vytvoří účet Azure Cosmos DB a nakonfiguruje jej pro vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="66830-104">This sample CLI script creates an Azure Cosmos DB account, and then configures it for high availability.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="66830-105">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="66830-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="66830-106">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="66830-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="66830-107">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="66830-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="66830-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="66830-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/high-availability-cosmosdb-configure-failover/high-availability-cosmosdb-configure-failover.sh?highlight=23-27 "Create an Azure Cosmos DB failover policy")]

## <a name="clean-up-deployment"></a><span data-ttu-id="66830-109">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="66830-109">Clean up deployment</span></span>

<span data-ttu-id="66830-110">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="66830-110">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="66830-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="66830-111">Script explanation</span></span>

<span data-ttu-id="66830-112">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="66830-112">This script uses hello following commands.</span></span> <span data-ttu-id="66830-113">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="66830-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="66830-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="66830-114">Command</span></span> | <span data-ttu-id="66830-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="66830-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="66830-116">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="66830-116">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="66830-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="66830-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="66830-118">Vytvoření az cosmosdb</span><span class="sxs-lookup"><span data-stu-id="66830-118">az cosmosdb create</span></span>](/cli/azure/sql/server#create) | <span data-ttu-id="66830-119">Vytvoří účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="66830-119">Creates an Azure Cosmos DB account.</span></span> |
| [<span data-ttu-id="66830-120">AZ cosmosdb aktualizace</span><span class="sxs-lookup"><span data-stu-id="66830-120">az cosmosdb update</span></span>](/cli/azure/cosmosdb#update) | <span data-ttu-id="66830-121">Aktualizuje účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="66830-121">Updates Azure Cosmos DB account.</span></span> |
| [<span data-ttu-id="66830-122">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="66830-122">az group delete</span></span>](/cli/azure/resource#delete) | <span data-ttu-id="66830-123">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="66830-123">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="66830-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="66830-124">Next steps</span></span>

<span data-ttu-id="66830-125">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="66830-125">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="66830-126">Další ukázky skript příkazového řádku DB Cosmos Azure lze nalézt v hello [dokumentace Azure Cosmos DB CLI](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="66830-126">Additional Azure Cosmos DB CLI script samples can be found in hello [Azure Cosmos DB CLI documentation](../cli-samples.md).</span></span>
