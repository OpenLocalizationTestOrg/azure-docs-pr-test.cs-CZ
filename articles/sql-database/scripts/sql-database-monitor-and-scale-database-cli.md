---
title: "Rozhraní příkazového řádku příklad monitorování škálování jedním Azure SQL database | Microsoft Docs"
description: "Azure CLI ukázkový skript ke sledování a škálování jedné databáze Azure SQL"
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
ms.openlocfilehash: 01911b85268244a8fddb32aa726f8a870abbaf77
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-cli-to-monitor-and-scale-a-single-sql-database"></a><span data-ttu-id="8111f-103">Pomocí rozhraní příkazového řádku ke sledování a škálování jedné databáze SQL</span><span class="sxs-lookup"><span data-stu-id="8111f-103">Use CLI to monitor and scale a single SQL database</span></span>

<span data-ttu-id="8111f-104">Tento ukázkový skript příkazového řádku Azure CLI škáluje jedné databáze Azure SQL na úroveň výkonu různých po dotaz na informace o velikosti databáze.</span><span class="sxs-lookup"><span data-stu-id="8111f-104">This Azure CLI script example scales a single Azure SQL database to a different performance level after querying the size information of the database.</span></span> 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="8111f-105">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="8111f-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="8111f-106">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="8111f-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="8111f-107">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="8111f-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="8111f-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="8111f-108">Sample script</span></span>

<span data-ttu-id="8111f-109">[!code-azurecli-interactive[hlavní](../../../cli_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.sh "sledování a škálování jedna databáze SQL")]</span><span class="sxs-lookup"><span data-stu-id="8111f-109">[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.sh "Monitor and scale single SQL Database")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="8111f-110">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="8111f-110">Clean up deployment</span></span>

<span data-ttu-id="8111f-111">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="8111f-111">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="8111f-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="8111f-112">Script explanation</span></span>

<span data-ttu-id="8111f-113">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="8111f-113">This script uses the following commands.</span></span> <span data-ttu-id="8111f-114">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="8111f-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="8111f-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="8111f-115">Command</span></span> | <span data-ttu-id="8111f-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="8111f-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="8111f-117">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="8111f-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="8111f-118">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="8111f-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="8111f-119">Vytvoření az sql serveru</span><span class="sxs-lookup"><span data-stu-id="8111f-119">az sql server create</span></span>](https://docs.microsoft.com/cli/azure/sql/server#create) | <span data-ttu-id="8111f-120">Vytvoří logického serveru, který je hostitelem databáze.</span><span class="sxs-lookup"><span data-stu-id="8111f-120">Creates a logical server that hosts a database.</span></span> |
| [<span data-ttu-id="8111f-121">db sql az zobrazit využití</span><span class="sxs-lookup"><span data-stu-id="8111f-121">az sql db show-usage</span></span>](https://docs.microsoft.com/cli/azure/sql/db#show-usage) | <span data-ttu-id="8111f-122">Zobrazuje informace o využití velikost pro databázi.</span><span class="sxs-lookup"><span data-stu-id="8111f-122">Shows the size usage information for a database.</span></span> |
| [<span data-ttu-id="8111f-123">aktualizace databáze sql az</span><span class="sxs-lookup"><span data-stu-id="8111f-123">az sql db update</span></span>](https://docs.microsoft.com/cli/azure/sql/db#update) | <span data-ttu-id="8111f-124">Aktualizuje vlastnosti databáze (například výkon nebo vrstvě úrovně služby) nebo přesouvá databázi do, mimo nebo mezi elastické fondy.</span><span class="sxs-lookup"><span data-stu-id="8111f-124">Updates database properties (such as the service tier or performance level) or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="8111f-125">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="8111f-125">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="8111f-126">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="8111f-126">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="8111f-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8111f-127">Next steps</span></span>

<span data-ttu-id="8111f-128">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8111f-128">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="8111f-129">Další ukázky skriptu SQL databáze rozhraní příkazového řádku najdete v [dokumentace Azure SQL Database](../sql-database-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="8111f-129">Additional SQL Database CLI script samples can be found in the [Azure SQL Database documentation](../sql-database-cli-samples.md).</span></span>
