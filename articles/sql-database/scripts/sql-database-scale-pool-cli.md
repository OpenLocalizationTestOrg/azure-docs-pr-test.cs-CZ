---
title: "Příklad aaaCLI škáluje SQL elastického fondu Azure SQL Database | Microsoft Docs"
description: "Azure CLI příklad skriptu tooscale elastický fond SQL v databázi SQL Azure"
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
ms.openlocfilehash: 436128b8183213f78b9abc2ec46efe2a3ed3c37c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-cli-tooscale-a-sql-elastic-pool-in-azure-sql-database"></a><span data-ttu-id="27482-103">Pomocí rozhraní příkazového řádku tooscale elastický fond SQL v databázi SQL Azure</span><span class="sxs-lookup"><span data-stu-id="27482-103">Use CLI tooscale a SQL elastic pool in Azure SQL Database</span></span>

<span data-ttu-id="27482-104">Tento příklad skriptu rozhraní příkazového řádku Azure vytvoří SQL elastické fondy, přesune databáze ve fondu a změny úrovně výkonu elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="27482-104">This Azure CLI script example creates SQL elastic pools, moves pooled databases, and changes elastic pool performance levels.</span></span> 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="27482-105">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="27482-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="27482-106">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="27482-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="27482-107">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="27482-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="27482-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="27482-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/scale-pool/scale-pool.sh "Move database between pools")]

## <a name="clean-up-deployment"></a><span data-ttu-id="27482-109">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="27482-109">Clean up deployment</span></span>

<span data-ttu-id="27482-110">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="27482-110">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="27482-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="27482-111">Script explanation</span></span>

<span data-ttu-id="27482-112">Tento skript používá hello následující příkazy toocreate skupinu prostředků, logického serveru, databáze SQL a pravidla brány firewall.</span><span class="sxs-lookup"><span data-stu-id="27482-112">This script uses hello following commands toocreate a resource group, logical server, SQL Database, and firewall rules.</span></span> <span data-ttu-id="27482-113">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="27482-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="27482-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="27482-114">Command</span></span> | <span data-ttu-id="27482-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="27482-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="27482-116">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="27482-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="27482-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="27482-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="27482-118">Vytvoření az sql serveru</span><span class="sxs-lookup"><span data-stu-id="27482-118">az sql server create</span></span>](https://docs.microsoft.com/cli/azure/sql/server#create) | <span data-ttu-id="27482-119">Vytvoří logického serveru, hostitelé hello databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="27482-119">Creates a logical server that hosts hello SQL Database.</span></span> |
| [<span data-ttu-id="27482-120">Vytvoření sql az Elastická fondy</span><span class="sxs-lookup"><span data-stu-id="27482-120">az sql elastic-pools create</span></span>](https://docs.microsoft.com/cli/azure/sql/elastic-pool#create) | <span data-ttu-id="27482-121">Vytvoří fond elastické databáze v rámci hello logického serveru.</span><span class="sxs-lookup"><span data-stu-id="27482-121">Creates an elastic database pool within hello logical server.</span></span> |
| [<span data-ttu-id="27482-122">Vytvoření az sql db</span><span class="sxs-lookup"><span data-stu-id="27482-122">az sql db create</span></span>](https://docs.microsoft.com/cli/azure/sql/db#create) | <span data-ttu-id="27482-123">Vytvoří hello SQL Database v hello logického serveru.</span><span class="sxs-lookup"><span data-stu-id="27482-123">Creates hello SQL Database in hello logical server.</span></span> |
| [<span data-ttu-id="27482-124">aktualizace elastického fondu sql az</span><span class="sxs-lookup"><span data-stu-id="27482-124">az sql elastic-pools update</span></span>](https://docs.microsoft.com/cli/azure/sql/elastic-pool#update) | <span data-ttu-id="27482-125">Aktualizace fondu elastické databáze v tento příklad změny hello přiřazené eDTU.</span><span class="sxs-lookup"><span data-stu-id="27482-125">Updates an elastic database pool, in this example changes hello assigned eDTU.</span></span> |
| [<span data-ttu-id="27482-126">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="27482-126">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="27482-127">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="27482-127">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="27482-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="27482-128">Next steps</span></span>

<span data-ttu-id="27482-129">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="27482-129">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="27482-130">Další ukázky skriptu SQL databáze rozhraní příkazového řádku najdete v hello [dokumentace Azure SQL Database](../sql-database-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="27482-130">Additional SQL Database CLI script samples can be found in hello [Azure SQL Database documentation](../sql-database-cli-samples.md).</span></span>
