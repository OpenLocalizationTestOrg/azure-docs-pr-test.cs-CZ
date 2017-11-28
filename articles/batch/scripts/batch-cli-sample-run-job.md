---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku – spuštění úlohy pomocí služby Batch | Microsoft Docs"
description: "Azure ukázka skriptu rozhraní příkazového řádku – spuštění úlohy pomocí služby Batch"
services: batch
documentationcenter: 
author: annatisch
manager: daryls
editor: tysonn
ms.assetid: 
ms.service: batch
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: antisch
ms.openlocfilehash: f73a7cb341e550fd1c92f92201e20b27fa20d238
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="running-jobs-on-azure-batch-with-azure-cli"></a><span data-ttu-id="ec62d-103">Spuštění úlohy v Azure Batch pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="ec62d-103">Running jobs on Azure Batch with Azure CLI</span></span>

<span data-ttu-id="ec62d-104">Tento skript vytvoří dávkovou úlohu a přidá řadu úloh toohello úlohy.</span><span class="sxs-lookup"><span data-stu-id="ec62d-104">This script creates a Batch job and adds a series of tasks toohello job.</span></span> <span data-ttu-id="ec62d-105">Také ukazuje, jak toomonitor úlohy a její úkoly.</span><span class="sxs-lookup"><span data-stu-id="ec62d-105">It also demonstrates how toomonitor a job and its tasks.</span></span> <span data-ttu-id="ec62d-106">Nakonec ukazuje, jak tooquery hello služby Batch efektivně pro informace o hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="ec62d-106">Finally, it shows how tooquery hello Batch service efficiently for information about hello job's tasks.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ec62d-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ec62d-107">Prerequisites</span></span>

- <span data-ttu-id="ec62d-108">Instalace hello hello pokyny uvedené v hello pomocí rozhraní příkazového řádku Azure [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli), pokud jste tak již neučinili.</span><span class="sxs-lookup"><span data-stu-id="ec62d-108">Install hello Azure CLI using hello instructions provided in hello [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli), if you have not already done so.</span></span>
- <span data-ttu-id="ec62d-109">Pokud jste již nemáte, vytvořte účet Batch.</span><span class="sxs-lookup"><span data-stu-id="ec62d-109">Create a Batch account if you don't already have one.</span></span> <span data-ttu-id="ec62d-110">V tématu [vytvořit dávkový účet s hello rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) ukázkový skript, který vytvoří účet.</span><span class="sxs-lookup"><span data-stu-id="ec62d-110">See [Create a Batch account with hello Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) for a sample script that creates an account.</span></span>
- <span data-ttu-id="ec62d-111">Pokud jste tak ještě neučinili, nakonfigurujte toorun aplikaci ze spouštěcí úkol.</span><span class="sxs-lookup"><span data-stu-id="ec62d-111">Configure an application toorun from a start task if you haven't yet done so.</span></span> <span data-ttu-id="ec62d-112">V tématu [přidání tooAzure aplikace Batch pomocí rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) ukázkový skript, který se vytvoří aplikace a odesílá tooAzure balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec62d-112">See [Adding applications tooAzure Batch with Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) for a sample script that creates an application and uploads an application package tooAzure.</span></span>
- <span data-ttu-id="ec62d-113">Nakonfigurujte fond, na které hello bude úloha spuštěna.</span><span class="sxs-lookup"><span data-stu-id="ec62d-113">Configure a pool on which hello job will run.</span></span> <span data-ttu-id="ec62d-114">V tématu [fondech Správa Azure Batch pomocí rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/batch/batch-cli-sample-manage-pool) ukázkový skript, který vytvoří fond s konfigurace cloudové služby nebo konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ec62d-114">See [Managing Azure Batch pools with Azure CLI](https://docs.microsoft.com/azure/batch/batch-cli-sample-manage-pool) for a sample script that creates a pool with either a Cloud Service Configuration or a Virtual Machine Configuration.</span></span>

## <a name="sample-script"></a><span data-ttu-id="ec62d-115">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="ec62d-115">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/batch/run-job/run-job.sh "Run Job")]

## <a name="clean-up-job"></a><span data-ttu-id="ec62d-116">Vyčištění úlohy</span><span class="sxs-lookup"><span data-stu-id="ec62d-116">Clean up job</span></span>

<span data-ttu-id="ec62d-117">Po spuštění hello výše ukázkový skript, spusťte následující příkaz tooremove hello úlohy a všechny její úkoly.</span><span class="sxs-lookup"><span data-stu-id="ec62d-117">After you run hello above sample script, run hello following command tooremove the job and all of its tasks.</span></span> <span data-ttu-id="ec62d-118">Všimněte si, že fond hello potřebovat toobe odstranit samostatně.</span><span class="sxs-lookup"><span data-stu-id="ec62d-118">Note that hello pool will need toobe deleted separately.</span></span> <span data-ttu-id="ec62d-119">V tématu [fondech Správa Azure Batch pomocí rozhraní příkazového řádku Azure](./batch-cli-sample-manage-pool.md) Další informace o vytváření a odstraňování fondy.</span><span class="sxs-lookup"><span data-stu-id="ec62d-119">See [Managing Azure Batch pools with Azure CLI](./batch-cli-sample-manage-pool.md) for more information on creating and deleting pools.</span></span>

```azurecli
az batch job delete --job-id myjob
```

## <a name="script-explanation"></a><span data-ttu-id="ec62d-120">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="ec62d-120">Script explanation</span></span>

<span data-ttu-id="ec62d-121">Tento skript používá následující příkazy toocreate hello dávkové úlohy a úlohy.</span><span class="sxs-lookup"><span data-stu-id="ec62d-121">This script uses hello following commands toocreate a Batch job and tasks.</span></span> <span data-ttu-id="ec62d-122">Každý příkaz v dokumentaci k toocommand specifické hello tabulky odkazů.</span><span class="sxs-lookup"><span data-stu-id="ec62d-122">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="ec62d-123">Příkaz</span><span class="sxs-lookup"><span data-stu-id="ec62d-123">Command</span></span> | <span data-ttu-id="ec62d-124">Poznámky</span><span class="sxs-lookup"><span data-stu-id="ec62d-124">Notes</span></span> |
|---|---|
| [<span data-ttu-id="ec62d-125">připojení k účtu batch az</span><span class="sxs-lookup"><span data-stu-id="ec62d-125">az batch account login</span></span>](https://docs.microsoft.com/cli/azure/batch/account#login) | <span data-ttu-id="ec62d-126">Ověřování na základě účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="ec62d-126">Authenticate against a Batch account.</span></span>  |
| [<span data-ttu-id="ec62d-127">vytvoření úlohy batch az</span><span class="sxs-lookup"><span data-stu-id="ec62d-127">az batch job create</span></span>](https://docs.microsoft.com/cli/azure/batch/job#create) | <span data-ttu-id="ec62d-128">Vytvoří dávkovou úlohu.</span><span class="sxs-lookup"><span data-stu-id="ec62d-128">Creates a Batch job.</span></span>  |
| [<span data-ttu-id="ec62d-129">sady úloh az batch</span><span class="sxs-lookup"><span data-stu-id="ec62d-129">az batch job set</span></span>](https://docs.microsoft.com/cli/azure/batch/job#set) | <span data-ttu-id="ec62d-130">Aktualizace vlastnosti dávkovou úlohu.</span><span class="sxs-lookup"><span data-stu-id="ec62d-130">Updates properties of a Batch job.</span></span>  |
| [<span data-ttu-id="ec62d-131">Zobrazit úlohy batch az</span><span class="sxs-lookup"><span data-stu-id="ec62d-131">az batch job show</span></span>](https://docs.microsoft.com/cli/azure/batch/job#show) | <span data-ttu-id="ec62d-132">Načte podrobnosti o zadaný dávkovou úlohu.</span><span class="sxs-lookup"><span data-stu-id="ec62d-132">Retrieves details of a specified Batch job.</span></span>  |
| [<span data-ttu-id="ec62d-133">vytvoření úlohy batch az</span><span class="sxs-lookup"><span data-stu-id="ec62d-133">az batch task create</span></span>](https://docs.microsoft.com/cli/azure/batch/task#create) | <span data-ttu-id="ec62d-134">Přidá že určenou toohello úloh dávkovou úlohu.</span><span class="sxs-lookup"><span data-stu-id="ec62d-134">Adds a task toohello specified Batch job.</span></span>  |
| [<span data-ttu-id="ec62d-135">Zobrazit úlohy batch az</span><span class="sxs-lookup"><span data-stu-id="ec62d-135">az batch task show</span></span>](https://docs.microsoft.com/cli/azure/batch/task#show) | <span data-ttu-id="ec62d-136">Načte hello podrobnosti úlohy z hello zadat dávkovou úlohu.</span><span class="sxs-lookup"><span data-stu-id="ec62d-136">Retrieves hello details of a task from hello specified Batch job.</span></span>  |
| [<span data-ttu-id="ec62d-137">Seznam úkolů az batch</span><span class="sxs-lookup"><span data-stu-id="ec62d-137">az batch task list</span></span>](https://docs.microsoft.com/cli/azure/batch/task#list) | <span data-ttu-id="ec62d-138">Obsahuje hello úkoly spojené s hello zadanou úlohu.</span><span class="sxs-lookup"><span data-stu-id="ec62d-138">Lists hello tasks associated with hello specified job.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="ec62d-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ec62d-139">Next steps</span></span>

<span data-ttu-id="ec62d-140">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ec62d-140">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="ec62d-141">Další ukázky skriptu rozhraní příkazového řádku Batch lze nalézt v hello [dokumentaci k rozhraní příkazového řádku Azure Batch](../batch-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="ec62d-141">Additional Batch CLI script samples can be found in hello [Azure Batch CLI documentation](../batch-cli-samples.md).</span></span>
