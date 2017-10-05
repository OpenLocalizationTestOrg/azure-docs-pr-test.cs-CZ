---
title: "Příklad rozhraní příkazového řádku škáluje SQL elastického fondu Azure SQL Database | Microsoft Docs"
description: "Azure CLI ukázkový skript škálování elastický fond SQL v databázi SQL Azure"
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
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 888d2b7b7c118fede82d39881570a3b3d7b09961
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-cli-to-scale-a-sql-elastic-pool-in-azure-sql-database"></a><span data-ttu-id="3e25b-103">Použití rozhraní příkazového řádku pro škálování elastický fond SQL v databázi SQL Azure</span><span class="sxs-lookup"><span data-stu-id="3e25b-103">Use CLI to scale a SQL elastic pool in Azure SQL Database</span></span>

<span data-ttu-id="3e25b-104">Tento příklad skriptu rozhraní příkazového řádku Azure vytvoří SQL elastické fondy, přesune databáze ve fondu a změny úrovně výkonu elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="3e25b-104">This Azure CLI script example creates SQL elastic pools, moves pooled databases, and changes elastic pool performance levels.</span></span> 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="3e25b-105">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="3e25b-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="3e25b-106">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="3e25b-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="3e25b-107">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="3e25b-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="3e25b-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="3e25b-108">Sample script</span></span>

<span data-ttu-id="3e25b-109">[!code-azurecli-interactive[hlavní](../../../cli_scripts/sql-database/scale-pool/scale-pool.sh "přesun databáze mezi fondy")]</span><span class="sxs-lookup"><span data-stu-id="3e25b-109">[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/scale-pool/scale-pool.sh "Move database between pools")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="3e25b-110">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="3e25b-110">Clean up deployment</span></span>

<span data-ttu-id="3e25b-111">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="3e25b-111">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="3e25b-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="3e25b-112">Script explanation</span></span>

<span data-ttu-id="3e25b-113">Tento skript používá následující příkazy k vytvoření skupiny prostředků, logického serveru, databáze SQL a pravidla brány firewall.</span><span class="sxs-lookup"><span data-stu-id="3e25b-113">This script uses the following commands to create a resource group, logical server, SQL Database, and firewall rules.</span></span> <span data-ttu-id="3e25b-114">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="3e25b-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="3e25b-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="3e25b-115">Command</span></span> | <span data-ttu-id="3e25b-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="3e25b-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="3e25b-117">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="3e25b-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="3e25b-118">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="3e25b-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="3e25b-119">Vytvoření az sql serveru</span><span class="sxs-lookup"><span data-stu-id="3e25b-119">az sql server create</span></span>](https://docs.microsoft.com/cli/azure/sql/server#create) | <span data-ttu-id="3e25b-120">Vytvoří logického serveru, který je hostitelem databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="3e25b-120">Creates a logical server that hosts the SQL Database.</span></span> |
| [<span data-ttu-id="3e25b-121">Vytvoření sql az Elastická fondy</span><span class="sxs-lookup"><span data-stu-id="3e25b-121">az sql elastic-pools create</span></span>](https://docs.microsoft.com/cli/azure/sql/elastic-pool#create) | <span data-ttu-id="3e25b-122">Vytvoří fond elastické databáze v rámci logického serveru.</span><span class="sxs-lookup"><span data-stu-id="3e25b-122">Creates an elastic database pool within the logical server.</span></span> |
| [<span data-ttu-id="3e25b-123">Vytvoření az sql db</span><span class="sxs-lookup"><span data-stu-id="3e25b-123">az sql db create</span></span>](https://docs.microsoft.com/cli/azure/sql/db#create) | <span data-ttu-id="3e25b-124">Vytvoří databázi SQL v logického serveru.</span><span class="sxs-lookup"><span data-stu-id="3e25b-124">Creates the SQL Database in the logical server.</span></span> |
| [<span data-ttu-id="3e25b-125">aktualizace elastického fondu sql az</span><span class="sxs-lookup"><span data-stu-id="3e25b-125">az sql elastic-pools update</span></span>](https://docs.microsoft.com/cli/azure/sql/elastic-pool#update) | <span data-ttu-id="3e25b-126">Aktualizace fondu elastické databáze, v tomto příkladu se změní přiřazené eDTU.</span><span class="sxs-lookup"><span data-stu-id="3e25b-126">Updates an elastic database pool, in this example changes the assigned eDTU.</span></span> |
| [<span data-ttu-id="3e25b-127">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="3e25b-127">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="3e25b-128">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="3e25b-128">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="3e25b-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3e25b-129">Next steps</span></span>

<span data-ttu-id="3e25b-130">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3e25b-130">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="3e25b-131">Další ukázky skriptu SQL databáze rozhraní příkazového řádku najdete v [dokumentace Azure SQL Database](../sql-database-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="3e25b-131">Additional SQL Database CLI script samples can be found in the [Azure SQL Database documentation](../sql-database-cli-samples.md).</span></span>
