---
title: "Ukázka skriptu rozhraní příkazového řádku Azure - Správa fondů ve službě Batch | Microsoft Docs"
description: "Ukázka skriptu rozhraní příkazového řádku Azure - Správa fondů ve službě Batch"
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
ms.openlocfilehash: 2556b02459886390b803407c5cb828687229a44e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="managing-azure-batch-pools-with-azure-cli"></a><span data-ttu-id="a160f-103">Správa fondech Azure Batch pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="a160f-103">Managing Azure Batch pools with Azure CLI</span></span>

<span data-ttu-id="a160f-104">Tyto skriptu ukazuje některé z nástrojů dostupných v rozhraní příkazového řádku Azure k vytváření a správě fondů výpočetních uzlů ve službě Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="a160f-104">These script demonstrates some of the tools available in the Azure CLI to create and manage pools of compute nodes in the Azure Batch service.</span></span>

> [!NOTE]
> <span data-ttu-id="a160f-105">Příkazy v této ukázce vytvořit virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="a160f-105">The commands in this sample create Azure virtual machines.</span></span> <span data-ttu-id="a160f-106">Spuštěných virtuálních počítačů bude nabíhat poplatky ke svému účtu.</span><span class="sxs-lookup"><span data-stu-id="a160f-106">Running VMs will accrue charges to your account.</span></span> <span data-ttu-id="a160f-107">Chcete-li minimalizovat tyto poplatky, odstraňte virtuální počítače po dokončení spuštění ukázky.</span><span class="sxs-lookup"><span data-stu-id="a160f-107">To minimize these charges, delete the VMs once you're done running the sample.</span></span> <span data-ttu-id="a160f-108">V tématu [vyčištění fondy](#clean-up-pools).</span><span class="sxs-lookup"><span data-stu-id="a160f-108">See [Clean up pools](#clean-up-pools).</span></span>

<span data-ttu-id="a160f-109">Fondy batch můžete nakonfigurovat ve dvou způsobů, buď konfigurací cloudové služby (jenom Windows) nebo konfigurace virtuálního počítače (Windows a Linux).</span><span class="sxs-lookup"><span data-stu-id="a160f-109">Batch pools can be configured in two ways, either with a Cloud Services configuration (Windows only), or a Virtual Machine configuration (Windows and Linux).</span></span> <span data-ttu-id="a160f-110">Následující ukázkové skripty ukazují, jak vytvořit fondy s obě konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a160f-110">The sample scripts below show how to create pools with both configurations.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a160f-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a160f-111">Prerequisites</span></span>

- <span data-ttu-id="a160f-112">Instalace rozhraní příkazového řádku Azure pomocí pokynů uvedených v [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli), pokud jste tak již neučinili.</span><span class="sxs-lookup"><span data-stu-id="a160f-112">Install the Azure CLI using the instructions provided in the [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli), if you have not already done so.</span></span>
- <span data-ttu-id="a160f-113">Pokud jste již nemáte, vytvořte účet Batch.</span><span class="sxs-lookup"><span data-stu-id="a160f-113">Create a Batch account if you don't already have one.</span></span> <span data-ttu-id="a160f-114">V tématu [vytvořit účet Batch pomocí rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) ukázkový skript, který vytvoří účet.</span><span class="sxs-lookup"><span data-stu-id="a160f-114">See [Create a Batch account with the Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) for a sample script that creates an account.</span></span>
- <span data-ttu-id="a160f-115">Konfigurace aplikace pro spuštění ze spouštěcí úkol, pokud jste tak ještě neučinili.</span><span class="sxs-lookup"><span data-stu-id="a160f-115">Configure an application to run from a start task if you haven't yet done so.</span></span> <span data-ttu-id="a160f-116">V tématu [přidání aplikací do Azure Batch pomocí rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) ukázkový skript, který se vytvoří aplikace a odesílá balíček aplikace do Azure.</span><span class="sxs-lookup"><span data-stu-id="a160f-116">See [Adding applications to Azure Batch with Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) for a sample script that creates an application and uploads an application package to Azure.</span></span>

## <a name="pool-with-cloud-service-configuration-sample-script"></a><span data-ttu-id="a160f-117">Fond se cloudové služby konfigurace ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="a160f-117">Pool with cloud service configuration sample script</span></span>

<span data-ttu-id="a160f-118">[!code-azurecli[hlavní](../../../cli_scripts/batch/manage-pool/manage-pool-windows.sh "spravovat fondy cloudové služby")]</span><span class="sxs-lookup"><span data-stu-id="a160f-118">[!code-azurecli[main](../../../cli_scripts/batch/manage-pool/manage-pool-windows.sh "Manage Cloud Services Pools")]</span></span>

## <a name="pool-with-virtual-machine-configuration-sample-script"></a><span data-ttu-id="a160f-119">Fond se skript ukázka konfigurace virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="a160f-119">Pool with virtual machine configuration sample script</span></span>

<span data-ttu-id="a160f-120">[!code-azurecli[hlavní](../../../cli_scripts/batch/manage-pool/manage-pool-linux.sh "spravovat fondy virtuálního počítače")]</span><span class="sxs-lookup"><span data-stu-id="a160f-120">[!code-azurecli[main](../../../cli_scripts/batch/manage-pool/manage-pool-linux.sh "Manage Virtual Machine Pools")]</span></span>

## <a name="clean-up-pools"></a><span data-ttu-id="a160f-121">Vyčištění fondy</span><span class="sxs-lookup"><span data-stu-id="a160f-121">Clean up pools</span></span>

<span data-ttu-id="a160f-122">Po spuštění výše uvedený ukázkový skript, spusťte následující příkaz k odstranění fondů.</span><span class="sxs-lookup"><span data-stu-id="a160f-122">After you run the above sample script, run the following command to delete the pools.</span></span>
```azurecli
az batch pool delete --pool-id mypool-windows
az batch pool delete --pool-id mypool-linux
```

## <a name="script-explanation"></a><span data-ttu-id="a160f-123">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="a160f-123">Script explanation</span></span>

<span data-ttu-id="a160f-124">Tento skript používá následující příkazy k vytvoření a manipulace s fondy Batch.</span><span class="sxs-lookup"><span data-stu-id="a160f-124">This script uses the following commands to create and manipulate Batch pools.</span></span>
<span data-ttu-id="a160f-125">Každý příkaz v tabulce odkazy na dokumentaci specifické pro příkaz.</span><span class="sxs-lookup"><span data-stu-id="a160f-125">Each command in the table links to command-specific documentation.</span></span>

| <span data-ttu-id="a160f-126">Příkaz</span><span class="sxs-lookup"><span data-stu-id="a160f-126">Command</span></span> | <span data-ttu-id="a160f-127">Poznámky</span><span class="sxs-lookup"><span data-stu-id="a160f-127">Notes</span></span> |
|---|---|
| [<span data-ttu-id="a160f-128">připojení k účtu batch az</span><span class="sxs-lookup"><span data-stu-id="a160f-128">az batch account login</span></span>](https://docs.microsoft.com/cli/azure/batch/account#login) | <span data-ttu-id="a160f-129">Ověřování na základě účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="a160f-129">Authenticate against a Batch account.</span></span>  |
| [<span data-ttu-id="a160f-130">souhrnný seznam aplikací batch az</span><span class="sxs-lookup"><span data-stu-id="a160f-130">az batch application summary list</span></span>](https://docs.microsoft.com/cli/azure/batch/application/summary#list) | <span data-ttu-id="a160f-131">Zobrazí seznam dostupných aplikací na účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="a160f-131">List the available applications in the Batch account.</span></span>  |
| [<span data-ttu-id="a160f-132">Vytvoření fondu služby batch az</span><span class="sxs-lookup"><span data-stu-id="a160f-132">az batch pool create</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#create) | <span data-ttu-id="a160f-133">Vytvoření fondu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a160f-133">Create a pool of VMs.</span></span>  |
| [<span data-ttu-id="a160f-134">Sada fondu batch az</span><span class="sxs-lookup"><span data-stu-id="a160f-134">az batch pool set</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#set) | <span data-ttu-id="a160f-135">Aktualizujte vlastnosti fondu.</span><span class="sxs-lookup"><span data-stu-id="a160f-135">Update properties of a pool.</span></span>  |
| [<span data-ttu-id="a160f-136">seznam uzlu. agent SKU fondu batch az</span><span class="sxs-lookup"><span data-stu-id="a160f-136">az batch pool node-agent-skus list</span></span>](https://docs.microsoft.com/cli/azure/batch/pool/node-agent-skus#list) | <span data-ttu-id="a160f-137">Agent k dispozici uzel seznamu SKU a informace o obrázku.</span><span class="sxs-lookup"><span data-stu-id="a160f-137">List available node agent SKUs and image information.</span></span>  |
| [<span data-ttu-id="a160f-138">Změna velikosti fondu batch az</span><span class="sxs-lookup"><span data-stu-id="a160f-138">az batch pool resize</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#resize) | <span data-ttu-id="a160f-139">Resize – počet běžících virtuálních počítačů zadaného fondu.</span><span class="sxs-lookup"><span data-stu-id="a160f-139">Resize the number of running VMs in the specified pool.</span></span>  |
| [<span data-ttu-id="a160f-140">Zobrazit fondu batch az</span><span class="sxs-lookup"><span data-stu-id="a160f-140">az batch pool show</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#show) | <span data-ttu-id="a160f-141">Zobrazí vlastnosti fondu.</span><span class="sxs-lookup"><span data-stu-id="a160f-141">Display the properties of a pool.</span></span>  |
| [<span data-ttu-id="a160f-142">Odstranění fondu batch az</span><span class="sxs-lookup"><span data-stu-id="a160f-142">az batch pool delete</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#delete) | <span data-ttu-id="a160f-143">Odstraní zadaný fond.</span><span class="sxs-lookup"><span data-stu-id="a160f-143">Delete the specified pool.</span></span>  |
| [<span data-ttu-id="a160f-144">Povolit automatické škálování fondu batch az</span><span class="sxs-lookup"><span data-stu-id="a160f-144">az batch pool autoscale enable</span></span>](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#enable) | <span data-ttu-id="a160f-145">Povolit automatické škálování na fond a použít vzorec.</span><span class="sxs-lookup"><span data-stu-id="a160f-145">Enable auto-scaling on a pool and apply a formula.</span></span>  |
| [<span data-ttu-id="a160f-146">zakázat automatické škálování fondu batch az</span><span class="sxs-lookup"><span data-stu-id="a160f-146">az batch pool autoscale disable</span></span>](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#disable) | <span data-ttu-id="a160f-147">Zakážete automatické škálování ve fondu.</span><span class="sxs-lookup"><span data-stu-id="a160f-147">Disable auto-scaling on a pool.</span></span>  |
| [<span data-ttu-id="a160f-148">seznam uzlů az batch</span><span class="sxs-lookup"><span data-stu-id="a160f-148">az batch node list</span></span>](https://docs.microsoft.com/cli/azure/batch/node#list) | <span data-ttu-id="a160f-149">Zobrazí seznam všech výpočetním uzlu ve fondu zadaný.</span><span class="sxs-lookup"><span data-stu-id="a160f-149">List all the compute node in the specified pool.</span></span>  |
| [<span data-ttu-id="a160f-150">restartování uzlu az batch</span><span class="sxs-lookup"><span data-stu-id="a160f-150">az batch node reboot</span></span>](https://docs.microsoft.com/cli/azure/batch/node#reboot) | <span data-ttu-id="a160f-151">Restartování zadaný výpočetním uzlu.</span><span class="sxs-lookup"><span data-stu-id="a160f-151">Reboot the specified compute node.</span></span>  |
| [<span data-ttu-id="a160f-152">Odstranění uzlu az batch</span><span class="sxs-lookup"><span data-stu-id="a160f-152">az batch node delete</span></span>](https://docs.microsoft.com/cli/azure/batch/node#delete) | <span data-ttu-id="a160f-153">Odstraňte uvedené uzly ze zadaného fondu.</span><span class="sxs-lookup"><span data-stu-id="a160f-153">Delete the listed nodes from the specified pool.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="a160f-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a160f-154">Next steps</span></span>

<span data-ttu-id="a160f-155">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a160f-155">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="a160f-156">Další ukázky skriptu Batch rozhraní příkazového řádku najdete v [dokumentaci k rozhraní příkazového řádku Azure Batch](../batch-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="a160f-156">Additional Batch CLI script samples can be found in the [Azure Batch CLI documentation](../batch-cli-samples.md).</span></span>

