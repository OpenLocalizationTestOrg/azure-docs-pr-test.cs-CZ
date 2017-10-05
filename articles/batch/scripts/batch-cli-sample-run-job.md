---
title: "Azure ukázka skriptu rozhraní příkazového řádku – spuštění úlohy pomocí služby Batch | Microsoft Docs"
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
ms.openlocfilehash: 5fe1e3595d9459e60b2fd54d6f17f6822731f453
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="running-jobs-on-azure-batch-with-azure-cli"></a><span data-ttu-id="980f7-103">Spuštění úlohy v Azure Batch pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="980f7-103">Running jobs on Azure Batch with Azure CLI</span></span>

<span data-ttu-id="980f7-104">Tento skript vytvoří dávkovou úlohu a přidá řadu úkolů do úlohy.</span><span class="sxs-lookup"><span data-stu-id="980f7-104">This script creates a Batch job and adds a series of tasks to the job.</span></span> <span data-ttu-id="980f7-105">Také ukazuje, jak sledovat úlohy a její úkoly.</span><span class="sxs-lookup"><span data-stu-id="980f7-105">It also demonstrates how to monitor a job and its tasks.</span></span> <span data-ttu-id="980f7-106">Nakonec ukazuje, jak zadat dotaz na službu Batch efektivně pro informace o úlohách úlohy.</span><span class="sxs-lookup"><span data-stu-id="980f7-106">Finally, it shows how to query the Batch service efficiently for information about the job's tasks.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="980f7-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="980f7-107">Prerequisites</span></span>

- <span data-ttu-id="980f7-108">Instalace rozhraní příkazového řádku Azure pomocí pokynů uvedených v [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli), pokud jste tak již neučinili.</span><span class="sxs-lookup"><span data-stu-id="980f7-108">Install the Azure CLI using the instructions provided in the [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli), if you have not already done so.</span></span>
- <span data-ttu-id="980f7-109">Pokud jste již nemáte, vytvořte účet Batch.</span><span class="sxs-lookup"><span data-stu-id="980f7-109">Create a Batch account if you don't already have one.</span></span> <span data-ttu-id="980f7-110">V tématu [vytvořit účet Batch pomocí rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) ukázkový skript, který vytvoří účet.</span><span class="sxs-lookup"><span data-stu-id="980f7-110">See [Create a Batch account with the Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) for a sample script that creates an account.</span></span>
- <span data-ttu-id="980f7-111">Konfigurace aplikace pro spuštění ze spouštěcí úkol, pokud jste tak ještě neučinili.</span><span class="sxs-lookup"><span data-stu-id="980f7-111">Configure an application to run from a start task if you haven't yet done so.</span></span> <span data-ttu-id="980f7-112">V tématu [přidání aplikací do Azure Batch pomocí rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) ukázkový skript, který se vytvoří aplikace a odesílá balíček aplikace do Azure.</span><span class="sxs-lookup"><span data-stu-id="980f7-112">See [Adding applications to Azure Batch with Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) for a sample script that creates an application and uploads an application package to Azure.</span></span>
- <span data-ttu-id="980f7-113">Nakonfigurujte fond, na kterém má být úloha spuštěna.</span><span class="sxs-lookup"><span data-stu-id="980f7-113">Configure a pool on which the job will run.</span></span> <span data-ttu-id="980f7-114">V tématu [fondech Správa Azure Batch pomocí rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/batch/batch-cli-sample-manage-pool) ukázkový skript, který vytvoří fond s konfigurace cloudové služby nebo konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="980f7-114">See [Managing Azure Batch pools with Azure CLI](https://docs.microsoft.com/azure/batch/batch-cli-sample-manage-pool) for a sample script that creates a pool with either a Cloud Service Configuration or a Virtual Machine Configuration.</span></span>

## <a name="sample-script"></a><span data-ttu-id="980f7-115">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="980f7-115">Sample script</span></span>

<span data-ttu-id="980f7-116">[!code-azurecli[hlavní](../../../cli_scripts/batch/run-job/run-job.sh "spuštění úlohy")]</span><span class="sxs-lookup"><span data-stu-id="980f7-116">[!code-azurecli[main](../../../cli_scripts/batch/run-job/run-job.sh "Run Job")]</span></span>

## <a name="clean-up-job"></a><span data-ttu-id="980f7-117">Vyčištění úlohy</span><span class="sxs-lookup"><span data-stu-id="980f7-117">Clean up job</span></span>

<span data-ttu-id="980f7-118">Po spuštění výše uvedený ukázkový skript, spusťte následující příkaz pro odebrání úlohy a všechny její úkoly.</span><span class="sxs-lookup"><span data-stu-id="980f7-118">After you run the above sample script, run the following command to remove the job and all of its tasks.</span></span> <span data-ttu-id="980f7-119">Všimněte si, že bude třeba fondu odstranit samostatně.</span><span class="sxs-lookup"><span data-stu-id="980f7-119">Note that the pool will need to be deleted separately.</span></span> <span data-ttu-id="980f7-120">V tématu [fondech Správa Azure Batch pomocí rozhraní příkazového řádku Azure](./batch-cli-sample-manage-pool.md) Další informace o vytváření a odstraňování fondy.</span><span class="sxs-lookup"><span data-stu-id="980f7-120">See [Managing Azure Batch pools with Azure CLI](./batch-cli-sample-manage-pool.md) for more information on creating and deleting pools.</span></span>

```azurecli
az batch job delete --job-id myjob
```

## <a name="script-explanation"></a><span data-ttu-id="980f7-121">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="980f7-121">Script explanation</span></span>

<span data-ttu-id="980f7-122">Tento skript používá následující příkazy k vytvoření úlohy Batch a úloh.</span><span class="sxs-lookup"><span data-stu-id="980f7-122">This script uses the following commands to create a Batch job and tasks.</span></span> <span data-ttu-id="980f7-123">Každý příkaz v tabulce odkazy na dokumentaci specifické pro příkaz.</span><span class="sxs-lookup"><span data-stu-id="980f7-123">Each command in the table links to command-specific documentation.</span></span>

| <span data-ttu-id="980f7-124">Příkaz</span><span class="sxs-lookup"><span data-stu-id="980f7-124">Command</span></span> | <span data-ttu-id="980f7-125">Poznámky</span><span class="sxs-lookup"><span data-stu-id="980f7-125">Notes</span></span> |
|---|---|
| [<span data-ttu-id="980f7-126">připojení k účtu batch az</span><span class="sxs-lookup"><span data-stu-id="980f7-126">az batch account login</span></span>](https://docs.microsoft.com/cli/azure/batch/account#login) | <span data-ttu-id="980f7-127">Ověřování na základě účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="980f7-127">Authenticate against a Batch account.</span></span>  |
| [<span data-ttu-id="980f7-128">vytvoření úlohy batch az</span><span class="sxs-lookup"><span data-stu-id="980f7-128">az batch job create</span></span>](https://docs.microsoft.com/cli/azure/batch/job#create) | <span data-ttu-id="980f7-129">Vytvoří dávkovou úlohu.</span><span class="sxs-lookup"><span data-stu-id="980f7-129">Creates a Batch job.</span></span>  |
| [<span data-ttu-id="980f7-130">sady úloh az batch</span><span class="sxs-lookup"><span data-stu-id="980f7-130">az batch job set</span></span>](https://docs.microsoft.com/cli/azure/batch/job#set) | <span data-ttu-id="980f7-131">Aktualizace vlastnosti dávkovou úlohu.</span><span class="sxs-lookup"><span data-stu-id="980f7-131">Updates properties of a Batch job.</span></span>  |
| [<span data-ttu-id="980f7-132">Zobrazit úlohy batch az</span><span class="sxs-lookup"><span data-stu-id="980f7-132">az batch job show</span></span>](https://docs.microsoft.com/cli/azure/batch/job#show) | <span data-ttu-id="980f7-133">Načte podrobnosti o zadaný dávkovou úlohu.</span><span class="sxs-lookup"><span data-stu-id="980f7-133">Retrieves details of a specified Batch job.</span></span>  |
| [<span data-ttu-id="980f7-134">vytvoření úlohy batch az</span><span class="sxs-lookup"><span data-stu-id="980f7-134">az batch task create</span></span>](https://docs.microsoft.com/cli/azure/batch/task#create) | <span data-ttu-id="980f7-135">Přidá úlohu do zadané úlohy Batch.</span><span class="sxs-lookup"><span data-stu-id="980f7-135">Adds a task to the specified Batch job.</span></span>  |
| [<span data-ttu-id="980f7-136">Zobrazit úlohy batch az</span><span class="sxs-lookup"><span data-stu-id="980f7-136">az batch task show</span></span>](https://docs.microsoft.com/cli/azure/batch/task#show) | <span data-ttu-id="980f7-137">Načte podrobnosti úlohy z zadaný dávkové úlohy.</span><span class="sxs-lookup"><span data-stu-id="980f7-137">Retrieves the details of a task from the specified Batch job.</span></span>  |
| [<span data-ttu-id="980f7-138">Seznam úkolů az batch</span><span class="sxs-lookup"><span data-stu-id="980f7-138">az batch task list</span></span>](https://docs.microsoft.com/cli/azure/batch/task#list) | <span data-ttu-id="980f7-139">Obsahuje úkoly spojené s zadanou úlohu.</span><span class="sxs-lookup"><span data-stu-id="980f7-139">Lists the tasks associated with the specified job.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="980f7-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="980f7-140">Next steps</span></span>

<span data-ttu-id="980f7-141">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="980f7-141">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="980f7-142">Další ukázky skriptu Batch rozhraní příkazového řádku najdete v [dokumentaci k rozhraní příkazového řádku Azure Batch](../batch-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="980f7-142">Additional Batch CLI script samples can be found in the [Azure Batch CLI documentation](../batch-cli-samples.md).</span></span>
