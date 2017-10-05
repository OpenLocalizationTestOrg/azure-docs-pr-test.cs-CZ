---
title: "Azure CLI skriptu-vytvořit zásadu převzetí služeb při selhání pro zajištění vysoké dostupnosti | Microsoft Docs"
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
ms.openlocfilehash: 96083d66cc1a2ef179f9313c1b3ed04162c1c048
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-failover-policy-for-high-availability-using-the-azure-cli"></a><span data-ttu-id="5770c-103">Vytvořit zásadu převzetí služeb při selhání pro vysokou dostupnost, pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="5770c-103">Create a failover policy for high availability using the Azure CLI</span></span>

<span data-ttu-id="5770c-104">Tento ukázkový skript rozhraní příkazového řádku vytvoří účet Azure Cosmos DB a nakonfiguruje jej pro vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="5770c-104">This sample CLI script creates an Azure Cosmos DB account, and then configures it for high availability.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="5770c-105">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="5770c-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="5770c-106">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="5770c-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="5770c-107">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="5770c-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="5770c-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="5770c-108">Sample script</span></span>

<span data-ttu-id="5770c-109">[!code-azurecli-interactive[hlavní](../../../cli_scripts/cosmosdb/high-availability-cosmosdb-configure-failover/high-availability-cosmosdb-configure-failover.sh?highlight=23-27 "vytvořit zásadu pro Azure Cosmos DB převzetí služeb při selhání")]</span><span class="sxs-lookup"><span data-stu-id="5770c-109">[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/high-availability-cosmosdb-configure-failover/high-availability-cosmosdb-configure-failover.sh?highlight=23-27 "Create an Azure Cosmos DB failover policy")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="5770c-110">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="5770c-110">Clean up deployment</span></span>

<span data-ttu-id="5770c-111">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="5770c-111">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="5770c-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="5770c-112">Script explanation</span></span>

<span data-ttu-id="5770c-113">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="5770c-113">This script uses the following commands.</span></span> <span data-ttu-id="5770c-114">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="5770c-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="5770c-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="5770c-115">Command</span></span> | <span data-ttu-id="5770c-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="5770c-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="5770c-117">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="5770c-117">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="5770c-118">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="5770c-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="5770c-119">Vytvoření az cosmosdb</span><span class="sxs-lookup"><span data-stu-id="5770c-119">az cosmosdb create</span></span>](/cli/azure/sql/server#create) | <span data-ttu-id="5770c-120">Vytvoří účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="5770c-120">Creates an Azure Cosmos DB account.</span></span> |
| [<span data-ttu-id="5770c-121">AZ cosmosdb aktualizace</span><span class="sxs-lookup"><span data-stu-id="5770c-121">az cosmosdb update</span></span>](/cli/azure/cosmosdb#update) | <span data-ttu-id="5770c-122">Aktualizuje účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="5770c-122">Updates Azure Cosmos DB account.</span></span> |
| [<span data-ttu-id="5770c-123">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="5770c-123">az group delete</span></span>](/cli/azure/resource#delete) | <span data-ttu-id="5770c-124">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="5770c-124">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="5770c-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5770c-125">Next steps</span></span>

<span data-ttu-id="5770c-126">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5770c-126">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="5770c-127">Další ukázky skript příkazového řádku DB Cosmos Azure lze nalézt v [dokumentace Azure Cosmos DB CLI](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="5770c-127">Additional Azure Cosmos DB CLI script samples can be found in the [Azure Cosmos DB CLI documentation](../cli-samples.md).</span></span>
