---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - restartovat virtuální počítače | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - restartování virtuálních počítačů, značky a ID"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/01/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: a1ae07bd1d2be906553bef817385a4a333a10eca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restart-vms"></a><span data-ttu-id="04806-103">Restartujte virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="04806-103">Restart VMs</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

<span data-ttu-id="04806-104">Tato ukázka zobrazuje několika způsoby tooget některé virtuální počítače a je restartovat.</span><span class="sxs-lookup"><span data-stu-id="04806-104">This sample shows a couple of ways tooget some VMs and restart them.</span></span>

<span data-ttu-id="04806-105">Hello nejprve restartuje všechny hello virtuální počítače ve skupině prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="04806-105">hello first restarts all hello VMs in hello resource group.</span></span>

```bash
az vm restart --ids $(az vm list --resource-group myResourceGroup --query "[].id" -o tsv)
```

<span data-ttu-id="04806-106">Hello druhý hello získá označené virtuálních počítačů pomocí `az resouce list` a filtry toohello prostředky, které jsou virtuální počítače a znovu tyto virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="04806-106">hello second gets hello tagged VMs using `az resouce list` and filters toohello resources that are VMs, and restarts those VMs.</span></span>

```bash
az vm restart --ids $(az resource list --tag "restart-tag" --query "[?type=='Microsoft.Compute/virtualMachines'].id" -o tsv)
```

<span data-ttu-id="04806-107">Tato ukázka funguje v prostředí Bash.</span><span class="sxs-lookup"><span data-stu-id="04806-107">This sample works in a Bash shell.</span></span> <span data-ttu-id="04806-108">Možnosti na spouštění skriptů rozhraní příkazového řádku Azure v klientovi Windows najdete v tématu [běžící ve Windows hello rozhraní příkazového řádku Azure](../windows/cli-options.md).</span><span class="sxs-lookup"><span data-stu-id="04806-108">For options on running Azure CLI scripts on Windows client, see [Running hello Azure CLI in Windows](../windows/cli-options.md).</span></span>


## <a name="sample-script"></a><span data-ttu-id="04806-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="04806-109">Sample script</span></span>

<span data-ttu-id="04806-110">Ukázka Hello má tři skripty.</span><span class="sxs-lookup"><span data-stu-id="04806-110">hello sample has three scripts.</span></span>
<span data-ttu-id="04806-111">Hello první z nich zřizuje hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="04806-111">hello first one provisions hello virtual machines.</span></span>
<span data-ttu-id="04806-112">Možnost Ne čekání hello používá tak hello příkaz vrátí bez čekání na každý počítač toobe zřízený.</span><span class="sxs-lookup"><span data-stu-id="04806-112">It uses hello no-wait option so hello command returns without waiting on each VM toobe provisioned.</span></span>
<span data-ttu-id="04806-113">Hello druhý čeká toobe virtuální počítače hello plně zřízený.</span><span class="sxs-lookup"><span data-stu-id="04806-113">hello second waits for hello VMs toobe fully provisioned.</span></span>
<span data-ttu-id="04806-114">třetí skriptu Hello restartuje všechny hello virtuální počítače, které byly zřízeny, a pak jenom hello označené virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="04806-114">hello third script restarts all hello VMs that were provisioned, and then just hello tagged VMs.</span></span>

### <a name="provision-hello-vms"></a><span data-ttu-id="04806-115">Hello zřizování virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="04806-115">Provision hello VMs</span></span>

<span data-ttu-id="04806-116">Tento skript vytvoří skupinu prostředků a vytvoří tři toorestart virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="04806-116">This script creates a resource group and then it creates three VMs toorestart.</span></span>
<span data-ttu-id="04806-117">Dvě z nich jsou označené.</span><span class="sxs-lookup"><span data-stu-id="04806-117">Two of them are tagged.</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/provision.sh "Provision hello VMs")]

### <a name="wait"></a><span data-ttu-id="04806-118">Wait</span><span class="sxs-lookup"><span data-stu-id="04806-118">Wait</span></span>

<span data-ttu-id="04806-119">Tento skript kontroluje na stav zřizování každých 20 sekund, dokud se zřizují všechny tři virtuální počítače, nebo jeden z nich selže tooprovision hello.</span><span class="sxs-lookup"><span data-stu-id="04806-119">This script checks on hello provisioning status every 20 seconds until all three VMs are provisioned, or one of them fails tooprovision.</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/wait.sh "Wait for hello VMs toobe provisioned")]

### <a name="restart-hello-vms"></a><span data-ttu-id="04806-120">Restartujte virtuální počítače hello</span><span class="sxs-lookup"><span data-stu-id="04806-120">Restart hello VMs</span></span>

<span data-ttu-id="04806-121">Tento skript restartuje všechny virtuální počítače hello ve skupině prostředků hello, a poté znovu spustí jenom virtuální počítače s příznakem hello.</span><span class="sxs-lookup"><span data-stu-id="04806-121">This script restarts all hello VMs in hello resource group, and then it restarts just hello tagged VMs.</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/restart.sh "Restart VMs by tag")]

## <a name="clean-up-deployment"></a><span data-ttu-id="04806-122">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="04806-122">Clean up deployment</span></span> 

<span data-ttu-id="04806-123">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello, virtuální počítače a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="04806-123">After hello script sample has been run, hello following command can be used tooremove hello resource groups, VMs, and all related resources.</span></span>

```azurecli-interactive 
az group delete -n myResourceGroup --no-wait --yes
```

## <a name="script-explanation"></a><span data-ttu-id="04806-124">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="04806-124">Script explanation</span></span>

<span data-ttu-id="04806-125">Tento skript používá hello následující příkazy toocreate skupinu prostředků, virtuální počítač, skupinu dostupnosti, nástroj pro vyrovnávání zatížení a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="04806-125">This script uses hello following commands toocreate a resource group, virtual machine, availability set, load balancer, and all related resources.</span></span> <span data-ttu-id="04806-126">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="04806-126">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="04806-127">Příkaz</span><span class="sxs-lookup"><span data-stu-id="04806-127">Command</span></span> | <span data-ttu-id="04806-128">Poznámky</span><span class="sxs-lookup"><span data-stu-id="04806-128">Notes</span></span> |
|---|---|
| [<span data-ttu-id="04806-129">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="04806-129">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="04806-130">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="04806-130">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="04806-131">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="04806-131">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | <span data-ttu-id="04806-132">Vytvoří hello virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="04806-132">Creates hello virtual machines.</span></span>  |
| [<span data-ttu-id="04806-133">Seznam virtuálních počítačů az</span><span class="sxs-lookup"><span data-stu-id="04806-133">az vm list</span></span>](https://docs.microsoft.com/cli/azure/vm#list) | <span data-ttu-id="04806-134">Použít s `--query` tooensure hello virtuálních počítačů, které jsou zřízené před restartováním je a pak tooget hello ID toorestart hello virtuálních počítačů je.</span><span class="sxs-lookup"><span data-stu-id="04806-134">Used with `--query` tooensure hello VMs are provisioned before restarting them, and then tooget hello IDs of hello VMs toorestart them.</span></span> |
| [<span data-ttu-id="04806-135">Seznam zdrojů az</span><span class="sxs-lookup"><span data-stu-id="04806-135">az resource list</span></span>](https://docs.microsoft.com/cli/azure/vm#list) | <span data-ttu-id="04806-136">Použít s `--query` tooget hello ID hello virtuálních počítačů pomocí hello značky.</span><span class="sxs-lookup"><span data-stu-id="04806-136">Used with `--query` tooget hello IDs of hello VMs using hello tag.</span></span> |
| [<span data-ttu-id="04806-137">AZ restartování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="04806-137">az vm restart</span></span>](https://docs.microsoft.com/cli/azure/vm#list) | <span data-ttu-id="04806-138">Restartuje hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="04806-138">Restarts hello VMs.</span></span> |
| [<span data-ttu-id="04806-139">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="04806-139">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="04806-140">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="04806-140">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="04806-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="04806-141">Next steps</span></span>

<span data-ttu-id="04806-142">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="04806-142">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="04806-143">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="04806-143">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
