---
title: "Elastický fond rozhraní příkazového řádku příklad přesunutí Azure SQL database SQL | Microsoft Docs"
description: "Azure CLI ukázkový skript přesunout databázi SQL v elastický fond SQL."
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
ms.openlocfilehash: 1dc31a0b20f36e28a58896ed63a5e0395ae1d3af
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-cli-to-move-an-azure-sql-database-in-a-sql-elastic-pool"></a><span data-ttu-id="96aec-103">Pomocí rozhraní příkazového řádku přesouvat Azure SQL database v elastický fond SQL.</span><span class="sxs-lookup"><span data-stu-id="96aec-103">Use CLI to move an Azure SQL database in a SQL elastic pool</span></span>

<span data-ttu-id="96aec-104">Tento příklad skriptu rozhraní příkazového řádku Azure vytvoří dvě elastické fondy a Azure SQL database se přesouvají z jedné elastický fond SQL do jiného elastický fond SQL a pak se posouvá databázi z elastického fondu na úroveň výkonu jedné databáze Azure.</span><span class="sxs-lookup"><span data-stu-id="96aec-104">This Azure CLI script example creates two elastic pools and moves an Azure SQL database from one SQL elastic pool into another SQL elastic pool, and then moves the database out of elastic pool to a single Azure database performance level.</span></span> 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="96aec-105">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="96aec-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="96aec-106">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="96aec-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="96aec-107">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="96aec-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="96aec-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="96aec-108">Sample script</span></span>

<span data-ttu-id="96aec-109">[!code-azurecli-interactive[hlavní](../../../cli_scripts/sql-database/move-database-between-pools/move-database-between-pools.sh "přesun databáze mezi fondy")]</span><span class="sxs-lookup"><span data-stu-id="96aec-109">[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/move-database-between-pools/move-database-between-pools.sh "Move database between pools")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="96aec-110">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="96aec-110">Clean up deployment</span></span>

<span data-ttu-id="96aec-111">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="96aec-111">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="96aec-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="96aec-112">Script explanation</span></span>

<span data-ttu-id="96aec-113">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="96aec-113">This script uses the following commands.</span></span> <span data-ttu-id="96aec-114">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="96aec-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="96aec-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="96aec-115">Command</span></span> | <span data-ttu-id="96aec-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="96aec-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="96aec-117">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="96aec-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="96aec-118">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="96aec-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="96aec-119">Vytvoření az sql serveru</span><span class="sxs-lookup"><span data-stu-id="96aec-119">az sql server create</span></span>](https://docs.microsoft.com/cli/azure/sql/server#create) | <span data-ttu-id="96aec-120">Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="96aec-120">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="96aec-121">Vytvoření sql az Elastická fondy</span><span class="sxs-lookup"><span data-stu-id="96aec-121">az sql elastic-pools create</span></span>](https://docs.microsoft.com/cli/azure/sql/elastic-pool#create) | <span data-ttu-id="96aec-122">Vytvoří elastického fondu v rámci logického serveru.</span><span class="sxs-lookup"><span data-stu-id="96aec-122">Creates an elastic pool within the logical server.</span></span> |
| [<span data-ttu-id="96aec-123">Vytvoření az sql db</span><span class="sxs-lookup"><span data-stu-id="96aec-123">az sql db create</span></span>](https://docs.microsoft.com/cli/azure/sql/db#create) | <span data-ttu-id="96aec-124">Vytvoří databázi logického serveru jako jednu, nebo databázi ve fondu.</span><span class="sxs-lookup"><span data-stu-id="96aec-124">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="96aec-125">aktualizace databáze sql az</span><span class="sxs-lookup"><span data-stu-id="96aec-125">az sql db update</span></span>](https://docs.microsoft.com/cli/azure/sql/db#update) | <span data-ttu-id="96aec-126">Aktualizuje vlastnosti databáze nebo přesouvá databázi do, mimo nebo mezi elastické fondy.</span><span class="sxs-lookup"><span data-stu-id="96aec-126">Updates database properties or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="96aec-127">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="96aec-127">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="96aec-128">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="96aec-128">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="96aec-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="96aec-129">Next steps</span></span>

<span data-ttu-id="96aec-130">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="96aec-130">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="96aec-131">Další ukázky skriptu SQL databáze rozhraní příkazového řádku najdete v [dokumentace Azure SQL Database](../sql-database-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="96aec-131">Additional SQL Database CLI script samples can be found in the [Azure SQL Database documentation](../sql-database-cli-samples.md).</span></span>


