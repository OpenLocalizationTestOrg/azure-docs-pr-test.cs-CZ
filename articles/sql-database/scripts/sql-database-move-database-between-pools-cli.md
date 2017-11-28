---
title: "elastický fond aaaCLI příklad přesunutí Azure SQL database SQL | Microsoft Docs"
description: "Azure CLI příklad skriptu toomove databázi SQL v elastický fond SQL."
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 07/05/2017
ms.author: janeng
ms.openlocfilehash: 841eb57d2d49612c3fadd3a6424a2b0309c69719
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-cli-toomove-an-azure-sql-database-in-a-sql-elastic-pool"></a><span data-ttu-id="2daa2-103">Použití rozhraní příkazového řádku toomove Azure SQL database v elastický fond SQL.</span><span class="sxs-lookup"><span data-stu-id="2daa2-103">Use CLI toomove an Azure SQL database in a SQL elastic pool</span></span>

<span data-ttu-id="2daa2-104">Tento příklad skriptu rozhraní příkazového řádku Azure vytvoří dvě elastické fondy a Azure SQL database se přesouvají z jedné elastický fond SQL do jiného elastický fond SQL a pak se posouvá hello databáze mimo úroveň výkonu izolovaných databází Azure tooa elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="2daa2-104">This Azure CLI script example creates two elastic pools and moves an Azure SQL database from one SQL elastic pool into another SQL elastic pool, and then moves hello database out of elastic pool tooa single Azure database performance level.</span></span> 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="2daa2-105">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="2daa2-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="2daa2-106">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="2daa2-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="2daa2-107">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="2daa2-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="2daa2-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="2daa2-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/move-database-between-pools/move-database-between-pools.sh "Move database between pools")]

## <a name="clean-up-deployment"></a><span data-ttu-id="2daa2-109">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="2daa2-109">Clean up deployment</span></span>

<span data-ttu-id="2daa2-110">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="2daa2-110">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="2daa2-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="2daa2-111">Script explanation</span></span>

<span data-ttu-id="2daa2-112">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="2daa2-112">This script uses hello following commands.</span></span> <span data-ttu-id="2daa2-113">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="2daa2-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="2daa2-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="2daa2-114">Command</span></span> | <span data-ttu-id="2daa2-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="2daa2-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="2daa2-116">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="2daa2-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="2daa2-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="2daa2-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="2daa2-118">Vytvoření az sql serveru</span><span class="sxs-lookup"><span data-stu-id="2daa2-118">az sql server create</span></span>](https://docs.microsoft.com/cli/azure/sql/server#create) | <span data-ttu-id="2daa2-119">Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="2daa2-119">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="2daa2-120">Vytvoření sql az Elastická fondy</span><span class="sxs-lookup"><span data-stu-id="2daa2-120">az sql elastic-pools create</span></span>](https://docs.microsoft.com/cli/azure/sql/elastic-pool#create) | <span data-ttu-id="2daa2-121">Vytvoří elastického fondu v rámci hello logického serveru.</span><span class="sxs-lookup"><span data-stu-id="2daa2-121">Creates an elastic pool within hello logical server.</span></span> |
| [<span data-ttu-id="2daa2-122">Vytvoření az sql db</span><span class="sxs-lookup"><span data-stu-id="2daa2-122">az sql db create</span></span>](https://docs.microsoft.com/cli/azure/sql/db#create) | <span data-ttu-id="2daa2-123">Vytvoří databázi logického serveru jako jednu, nebo databázi ve fondu.</span><span class="sxs-lookup"><span data-stu-id="2daa2-123">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="2daa2-124">aktualizace databáze sql az</span><span class="sxs-lookup"><span data-stu-id="2daa2-124">az sql db update</span></span>](https://docs.microsoft.com/cli/azure/sql/db#update) | <span data-ttu-id="2daa2-125">Aktualizuje vlastnosti databáze nebo přesouvá databázi do, mimo nebo mezi elastické fondy.</span><span class="sxs-lookup"><span data-stu-id="2daa2-125">Updates database properties or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="2daa2-126">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="2daa2-126">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="2daa2-127">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="2daa2-127">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="2daa2-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2daa2-128">Next steps</span></span>

<span data-ttu-id="2daa2-129">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2daa2-129">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="2daa2-130">Další ukázky skriptu SQL databáze rozhraní příkazového řádku najdete v hello [dokumentace Azure SQL Database](../sql-database-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="2daa2-130">Additional SQL Database CLI script samples can be found in hello [Azure SQL Database documentation](../sql-database-cli-samples.md).</span></span>


