---
title: "aaaCLI příklad monitorování škálování jedním Azure SQL database | Microsoft Docs"
description: "Azure CLI příklad skriptu toomonitor a škálování jedné databáze Azure SQL"
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
ms.openlocfilehash: 36031ddd46a947a80fe37884858a84eb66217270
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-cli-toomonitor-and-scale-a-single-sql-database"></a><span data-ttu-id="f81bc-103">Pomocí rozhraní příkazového řádku toomonitor a škálování jedné databáze SQL</span><span class="sxs-lookup"><span data-stu-id="f81bc-103">Use CLI toomonitor and scale a single SQL database</span></span>

<span data-ttu-id="f81bc-104">Tento ukázkový skript příkazového řádku Azure CLI škáluje jedné tooa různých výkonu úrovni databáze Azure SQL po dotaz na informace o velikosti hello hello databáze.</span><span class="sxs-lookup"><span data-stu-id="f81bc-104">This Azure CLI script example scales a single Azure SQL database tooa different performance level after querying hello size information of hello database.</span></span> 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="f81bc-105">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="f81bc-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="f81bc-106">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="f81bc-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="f81bc-107">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="f81bc-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="f81bc-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="f81bc-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.sh "Monitor and scale single SQL Database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="f81bc-109">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="f81bc-109">Clean up deployment</span></span>

<span data-ttu-id="f81bc-110">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="f81bc-110">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="f81bc-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="f81bc-111">Script explanation</span></span>

<span data-ttu-id="f81bc-112">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="f81bc-112">This script uses hello following commands.</span></span> <span data-ttu-id="f81bc-113">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="f81bc-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="f81bc-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="f81bc-114">Command</span></span> | <span data-ttu-id="f81bc-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="f81bc-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f81bc-116">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="f81bc-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="f81bc-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="f81bc-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="f81bc-118">Vytvoření az sql serveru</span><span class="sxs-lookup"><span data-stu-id="f81bc-118">az sql server create</span></span>](https://docs.microsoft.com/cli/azure/sql/server#create) | <span data-ttu-id="f81bc-119">Vytvoří logického serveru, který je hostitelem databáze.</span><span class="sxs-lookup"><span data-stu-id="f81bc-119">Creates a logical server that hosts a database.</span></span> |
| [<span data-ttu-id="f81bc-120">db sql az zobrazit využití</span><span class="sxs-lookup"><span data-stu-id="f81bc-120">az sql db show-usage</span></span>](https://docs.microsoft.com/cli/azure/sql/db#show-usage) | <span data-ttu-id="f81bc-121">Zobrazuje informace o databázi využití velikost hello.</span><span class="sxs-lookup"><span data-stu-id="f81bc-121">Shows hello size usage information for a database.</span></span> |
| [<span data-ttu-id="f81bc-122">aktualizace databáze sql az</span><span class="sxs-lookup"><span data-stu-id="f81bc-122">az sql db update</span></span>](https://docs.microsoft.com/cli/azure/sql/db#update) | <span data-ttu-id="f81bc-123">Aktualizuje vlastnosti databáze (například výkon nebo vrstvě úroveň serveru hello service) nebo přesouvá databázi do, mimo nebo mezi elastické fondy.</span><span class="sxs-lookup"><span data-stu-id="f81bc-123">Updates database properties (such as hello service tier or performance level) or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="f81bc-124">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="f81bc-124">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="f81bc-125">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="f81bc-125">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="f81bc-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f81bc-126">Next steps</span></span>

<span data-ttu-id="f81bc-127">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f81bc-127">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="f81bc-128">Další ukázky skriptu SQL databáze rozhraní příkazového řádku najdete v hello [dokumentace Azure SQL Database](../sql-database-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="f81bc-128">Additional SQL Database CLI script samples can be found in hello [Azure SQL Database documentation](../sql-database-cli-samples.md).</span></span>
