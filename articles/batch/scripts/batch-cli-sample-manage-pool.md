---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - Správa fondů ve službě Batch | Microsoft Docs"
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
ms.openlocfilehash: 6c9ca9515565aff42752231a080943be8e4c810b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-batch-pools-with-azure-cli"></a><span data-ttu-id="3b2f4-103">Správa fondech Azure Batch pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="3b2f4-103">Managing Azure Batch pools with Azure CLI</span></span>

<span data-ttu-id="3b2f4-104">Tyto skript ukazuje některé hello nástrojů dostupných v toocreate hello rozhraní příkazového řádku Azure a spravovat fondy výpočetních uzlů ve službě Azure Batch hello.</span><span class="sxs-lookup"><span data-stu-id="3b2f4-104">These script demonstrates some of hello tools available in hello Azure CLI toocreate and manage pools of compute nodes in hello Azure Batch service.</span></span>

> [!NOTE]
> <span data-ttu-id="3b2f4-105">Hello příkazy v této ukázce vytvořit virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="3b2f4-105">hello commands in this sample create Azure virtual machines.</span></span> <span data-ttu-id="3b2f4-106">Spuštěných virtuálních počítačů bude nabíhat poplatky tooyour účtu.</span><span class="sxs-lookup"><span data-stu-id="3b2f4-106">Running VMs will accrue charges tooyour account.</span></span> <span data-ttu-id="3b2f4-107">toominimize tyto poplatky, odstraňte hello virtuální počítače po dokončení spuštěné ukázka hello.</span><span class="sxs-lookup"><span data-stu-id="3b2f4-107">toominimize these charges, delete hello VMs once you're done running hello sample.</span></span> <span data-ttu-id="3b2f4-108">V tématu [vyčištění fondy](#clean-up-pools).</span><span class="sxs-lookup"><span data-stu-id="3b2f4-108">See [Clean up pools](#clean-up-pools).</span></span>

<span data-ttu-id="3b2f4-109">Fondy batch můžete nakonfigurovat ve dvou způsobů, buď konfigurací cloudové služby (jenom Windows) nebo konfigurace virtuálního počítače (Windows a Linux).</span><span class="sxs-lookup"><span data-stu-id="3b2f4-109">Batch pools can be configured in two ways, either with a Cloud Services configuration (Windows only), or a Virtual Machine configuration (Windows and Linux).</span></span> <span data-ttu-id="3b2f4-110">Ukázkové skripty Hello níže ukazují, jak toocreate fondy s obě konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3b2f4-110">hello sample scripts below show how toocreate pools with both configurations.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3b2f4-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3b2f4-111">Prerequisites</span></span>

- <span data-ttu-id="3b2f4-112">Instalace hello hello pokyny uvedené v hello pomocí rozhraní příkazového řádku Azure [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli), pokud jste tak již neučinili.</span><span class="sxs-lookup"><span data-stu-id="3b2f4-112">Install hello Azure CLI using hello instructions provided in hello [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli), if you have not already done so.</span></span>
- <span data-ttu-id="3b2f4-113">Pokud jste již nemáte, vytvořte účet Batch.</span><span class="sxs-lookup"><span data-stu-id="3b2f4-113">Create a Batch account if you don't already have one.</span></span> <span data-ttu-id="3b2f4-114">V tématu [vytvořit dávkový účet s hello rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) ukázkový skript, který vytvoří účet.</span><span class="sxs-lookup"><span data-stu-id="3b2f4-114">See [Create a Batch account with hello Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) for a sample script that creates an account.</span></span>
- <span data-ttu-id="3b2f4-115">Pokud jste tak ještě neučinili, nakonfigurujte toorun aplikaci ze spouštěcí úkol.</span><span class="sxs-lookup"><span data-stu-id="3b2f4-115">Configure an application toorun from a start task if you haven't yet done so.</span></span> <span data-ttu-id="3b2f4-116">V tématu [přidání tooAzure aplikace Batch pomocí rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) ukázkový skript, který se vytvoří aplikace a odesílá tooAzure balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="3b2f4-116">See [Adding applications tooAzure Batch with Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) for a sample script that creates an application and uploads an application package tooAzure.</span></span>

## <a name="pool-with-cloud-service-configuration-sample-script"></a><span data-ttu-id="3b2f4-117">Fond se cloudové služby konfigurace ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="3b2f4-117">Pool with cloud service configuration sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/batch/manage-pool/manage-pool-windows.sh "Manage Cloud Services Pools")]

## <a name="pool-with-virtual-machine-configuration-sample-script"></a><span data-ttu-id="3b2f4-118">Fond se skript ukázka konfigurace virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="3b2f4-118">Pool with virtual machine configuration sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/batch/manage-pool/manage-pool-linux.sh "Manage Virtual Machine Pools")]

## <a name="clean-up-pools"></a><span data-ttu-id="3b2f4-119">Vyčištění fondy</span><span class="sxs-lookup"><span data-stu-id="3b2f4-119">Clean up pools</span></span>

<span data-ttu-id="3b2f4-120">Po spuštění hello výše ukázkový skript, spusťte následující příkaz toodelete hello fondy hello.</span><span class="sxs-lookup"><span data-stu-id="3b2f4-120">After you run hello above sample script, run hello following command toodelete hello pools.</span></span>
```azurecli
az batch pool delete --pool-id mypool-windows
az batch pool delete --pool-id mypool-linux
```

## <a name="script-explanation"></a><span data-ttu-id="3b2f4-121">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="3b2f4-121">Script explanation</span></span>

<span data-ttu-id="3b2f4-122">Tento skript používá hello následující příkazy toocreate a manipulace s fondy Batch.</span><span class="sxs-lookup"><span data-stu-id="3b2f4-122">This script uses hello following commands toocreate and manipulate Batch pools.</span></span>
<span data-ttu-id="3b2f4-123">Každý příkaz v dokumentaci k toocommand specifické hello tabulky odkazů.</span><span class="sxs-lookup"><span data-stu-id="3b2f4-123">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="3b2f4-124">Příkaz</span><span class="sxs-lookup"><span data-stu-id="3b2f4-124">Command</span></span> | <span data-ttu-id="3b2f4-125">Poznámky</span><span class="sxs-lookup"><span data-stu-id="3b2f4-125">Notes</span></span> |
|---|---|
| [<span data-ttu-id="3b2f4-126">připojení k účtu batch az</span><span class="sxs-lookup"><span data-stu-id="3b2f4-126">az batch account login</span></span>](https://docs.microsoft.com/cli/azure/batch/account#login) | <span data-ttu-id="3b2f4-127">Ověřování na základě účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="3b2f4-127">Authenticate against a Batch account.</span></span>  |
| [<span data-ttu-id="3b2f4-128">souhrnný seznam aplikací batch az</span><span class="sxs-lookup"><span data-stu-id="3b2f4-128">az batch application summary list</span></span>](https://docs.microsoft.com/cli/azure/batch/application/summary#list) | <span data-ttu-id="3b2f4-129">Zobrazí seznam dostupných aplikací hello hello účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="3b2f4-129">List hello available applications in hello Batch account.</span></span>  |
| [<span data-ttu-id="3b2f4-130">Vytvoření fondu služby batch az</span><span class="sxs-lookup"><span data-stu-id="3b2f4-130">az batch pool create</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#create) | <span data-ttu-id="3b2f4-131">Vytvoření fondu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3b2f4-131">Create a pool of VMs.</span></span>  |
| [<span data-ttu-id="3b2f4-132">Sada fondu batch az</span><span class="sxs-lookup"><span data-stu-id="3b2f4-132">az batch pool set</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#set) | <span data-ttu-id="3b2f4-133">Aktualizujte vlastnosti fondu.</span><span class="sxs-lookup"><span data-stu-id="3b2f4-133">Update properties of a pool.</span></span>  |
| [<span data-ttu-id="3b2f4-134">seznam uzlu. agent SKU fondu batch az</span><span class="sxs-lookup"><span data-stu-id="3b2f4-134">az batch pool node-agent-skus list</span></span>](https://docs.microsoft.com/cli/azure/batch/pool/node-agent-skus#list) | <span data-ttu-id="3b2f4-135">Agent k dispozici uzel seznamu SKU a informace o obrázku.</span><span class="sxs-lookup"><span data-stu-id="3b2f4-135">List available node agent SKUs and image information.</span></span>  |
| [<span data-ttu-id="3b2f4-136">Změna velikosti fondu batch az</span><span class="sxs-lookup"><span data-stu-id="3b2f4-136">az batch pool resize</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#resize) | <span data-ttu-id="3b2f4-137">Zadat počet hello změny velikosti spuštěných virtuálních počítačů v hello fondu.</span><span class="sxs-lookup"><span data-stu-id="3b2f4-137">Resize hello number of running VMs in hello specified pool.</span></span>  |
| [<span data-ttu-id="3b2f4-138">Zobrazit fondu batch az</span><span class="sxs-lookup"><span data-stu-id="3b2f4-138">az batch pool show</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#show) | <span data-ttu-id="3b2f4-139">Zobrazí vlastnosti hello fondu.</span><span class="sxs-lookup"><span data-stu-id="3b2f4-139">Display hello properties of a pool.</span></span>  |
| [<span data-ttu-id="3b2f4-140">Odstranění fondu batch az</span><span class="sxs-lookup"><span data-stu-id="3b2f4-140">az batch pool delete</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#delete) | <span data-ttu-id="3b2f4-141">Odstranit hello zadaný fond.</span><span class="sxs-lookup"><span data-stu-id="3b2f4-141">Delete hello specified pool.</span></span>  |
| [<span data-ttu-id="3b2f4-142">Povolit automatické škálování fondu batch az</span><span class="sxs-lookup"><span data-stu-id="3b2f4-142">az batch pool autoscale enable</span></span>](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#enable) | <span data-ttu-id="3b2f4-143">Povolit automatické škálování na fond a použít vzorec.</span><span class="sxs-lookup"><span data-stu-id="3b2f4-143">Enable auto-scaling on a pool and apply a formula.</span></span>  |
| [<span data-ttu-id="3b2f4-144">zakázat automatické škálování fondu batch az</span><span class="sxs-lookup"><span data-stu-id="3b2f4-144">az batch pool autoscale disable</span></span>](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#disable) | <span data-ttu-id="3b2f4-145">Zakážete automatické škálování ve fondu.</span><span class="sxs-lookup"><span data-stu-id="3b2f4-145">Disable auto-scaling on a pool.</span></span>  |
| [<span data-ttu-id="3b2f4-146">seznam uzlů az batch</span><span class="sxs-lookup"><span data-stu-id="3b2f4-146">az batch node list</span></span>](https://docs.microsoft.com/cli/azure/batch/node#list) | <span data-ttu-id="3b2f4-147">Seznam všech hello výpočetní uzel v hello zadaný fond.</span><span class="sxs-lookup"><span data-stu-id="3b2f4-147">List all hello compute node in hello specified pool.</span></span>  |
| [<span data-ttu-id="3b2f4-148">restartování uzlu az batch</span><span class="sxs-lookup"><span data-stu-id="3b2f4-148">az batch node reboot</span></span>](https://docs.microsoft.com/cli/azure/batch/node#reboot) | <span data-ttu-id="3b2f4-149">Restartování hello zadaný výpočetním uzlu.</span><span class="sxs-lookup"><span data-stu-id="3b2f4-149">Reboot hello specified compute node.</span></span>  |
| [<span data-ttu-id="3b2f4-150">Odstranění uzlu az batch</span><span class="sxs-lookup"><span data-stu-id="3b2f4-150">az batch node delete</span></span>](https://docs.microsoft.com/cli/azure/batch/node#delete) | <span data-ttu-id="3b2f4-151">Odstranění hello uvedené uzly z hello zadaný fond.</span><span class="sxs-lookup"><span data-stu-id="3b2f4-151">Delete hello listed nodes from hello specified pool.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="3b2f4-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3b2f4-152">Next steps</span></span>

<span data-ttu-id="3b2f4-153">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3b2f4-153">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="3b2f4-154">Další ukázky skriptu rozhraní příkazového řádku Batch lze nalézt v hello [dokumentaci k rozhraní příkazového řádku Azure Batch](../batch-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="3b2f4-154">Additional Batch CLI script samples can be found in hello [Azure Batch CLI documentation](../batch-cli-samples.md).</span></span>

