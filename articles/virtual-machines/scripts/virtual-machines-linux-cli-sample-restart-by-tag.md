---
title: "Ukázka skriptu rozhraní příkazového řádku Azure – virtuální počítače restartovat | Microsoft Docs"
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
ms.openlocfilehash: 4d0fe95287c91a4b656904f9007ceaaf866e155f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="restart-vms"></a><span data-ttu-id="95b4d-103">Restartujte virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="95b4d-103">Restart VMs</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

<span data-ttu-id="95b4d-104">Tento příklad ukazuje několik způsobů, jak získat některé virtuální počítače a je restartovat.</span><span class="sxs-lookup"><span data-stu-id="95b4d-104">This sample shows a couple of ways to get some VMs and restart them.</span></span>

<span data-ttu-id="95b4d-105">První restartování všech virtuálních počítačů ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="95b4d-105">The first restarts all the VMs in the resource group.</span></span>

```bash
az vm restart --ids $(az vm list --resource-group myResourceGroup --query "[].id" -o tsv)
```

<span data-ttu-id="95b4d-106">Získá druhý s příznakem virtuálních počítačů pomocí `az resouce list` a filtrů prostředky, které jsou virtuální počítače a znovu tyto virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="95b4d-106">The second gets the tagged VMs using `az resouce list` and filters to the resources that are VMs, and restarts those VMs.</span></span>

```bash
az vm restart --ids $(az resource list --tag "restart-tag" --query "[?type=='Microsoft.Compute/virtualMachines'].id" -o tsv)
```

<span data-ttu-id="95b4d-107">Tato ukázka funguje v prostředí Bash.</span><span class="sxs-lookup"><span data-stu-id="95b4d-107">This sample works in a Bash shell.</span></span> <span data-ttu-id="95b4d-108">Možnosti na spouštění skriptů rozhraní příkazového řádku Azure v klientovi Windows najdete v tématu [běžící ve Windows Azure CLI](../windows/cli-options.md).</span><span class="sxs-lookup"><span data-stu-id="95b4d-108">For options on running Azure CLI scripts on Windows client, see [Running the Azure CLI in Windows](../windows/cli-options.md).</span></span>


## <a name="sample-script"></a><span data-ttu-id="95b4d-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="95b4d-109">Sample script</span></span>

<span data-ttu-id="95b4d-110">Ukázka má tři skripty.</span><span class="sxs-lookup"><span data-stu-id="95b4d-110">The sample has three scripts.</span></span>
<span data-ttu-id="95b4d-111">První z nich zřídí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="95b4d-111">The first one provisions the virtual machines.</span></span>
<span data-ttu-id="95b4d-112">Takže příkaz vrátí bez čekání na jednotlivé virtuální počítače, které se má zřídit používá možnost Ne čekání.</span><span class="sxs-lookup"><span data-stu-id="95b4d-112">It uses the no-wait option so the command returns without waiting on each VM to be provisioned.</span></span>
<span data-ttu-id="95b4d-113">Druhý čeká kompletní zřízení virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="95b4d-113">The second waits for the VMs to be fully provisioned.</span></span>
<span data-ttu-id="95b4d-114">Třetí skript restartování všech virtuálních počítačů, které byly zřízené, pak jenom s příznakem virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="95b4d-114">The third script restarts all the VMs that were provisioned, and then just the tagged VMs.</span></span>

### <a name="provision-the-vms"></a><span data-ttu-id="95b4d-115">Zřizování virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="95b4d-115">Provision the VMs</span></span>

<span data-ttu-id="95b4d-116">Tento skript vytvoří skupinu prostředků a vytvoří tři virtuální počítače restartovat.</span><span class="sxs-lookup"><span data-stu-id="95b4d-116">This script creates a resource group and then it creates three VMs to restart.</span></span>
<span data-ttu-id="95b4d-117">Dvě z nich jsou označené.</span><span class="sxs-lookup"><span data-stu-id="95b4d-117">Two of them are tagged.</span></span>

<span data-ttu-id="95b4d-118">[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-machine/restart-by-tag/provision.sh "zřizovat virtuální počítače")]</span><span class="sxs-lookup"><span data-stu-id="95b4d-118">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/provision.sh "Provision the VMs")]</span></span>

### <a name="wait"></a><span data-ttu-id="95b4d-119">Wait</span><span class="sxs-lookup"><span data-stu-id="95b4d-119">Wait</span></span>

<span data-ttu-id="95b4d-120">Tento skript kontroluje stav zřizování každých 20 sekund, dokud všechny tři virtuální počítače jsou zřízené nebo jeden z nich selže zřídit.</span><span class="sxs-lookup"><span data-stu-id="95b4d-120">This script checks on the provisioning status every 20 seconds until all three VMs are provisioned, or one of them fails to provision.</span></span>

<span data-ttu-id="95b4d-121">[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-machine/restart-by-tag/wait.sh "čekat na virtuální počítače, které se mají zřídit")]</span><span class="sxs-lookup"><span data-stu-id="95b4d-121">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/wait.sh "Wait for the VMs to be provisioned")]</span></span>

### <a name="restart-the-vms"></a><span data-ttu-id="95b4d-122">Restartujte virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="95b4d-122">Restart the VMs</span></span>

<span data-ttu-id="95b4d-123">Tento skript restartuje všechny virtuální počítače ve skupině prostředků, a pak znovu právě s příznakem virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="95b4d-123">This script restarts all the VMs in the resource group, and then it restarts just the tagged VMs.</span></span>

<span data-ttu-id="95b4d-124">[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-machine/restart-by-tag/restart.sh "restartovat virtuální počítače podle značky")]</span><span class="sxs-lookup"><span data-stu-id="95b4d-124">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/restart.sh "Restart VMs by tag")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="95b4d-125">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="95b4d-125">Clean up deployment</span></span> 

<span data-ttu-id="95b4d-126">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků, virtuální počítače a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="95b4d-126">After the script sample has been run, the following command can be used to remove the resource groups, VMs, and all related resources.</span></span>

```azurecli-interactive 
az group delete -n myResourceGroup --no-wait --yes
```

## <a name="script-explanation"></a><span data-ttu-id="95b4d-127">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="95b4d-127">Script explanation</span></span>

<span data-ttu-id="95b4d-128">Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální počítač, skupina dostupnosti, nástroj pro vyrovnávání zatížení a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="95b4d-128">This script uses the following commands to create a resource group, virtual machine, availability set, load balancer, and all related resources.</span></span> <span data-ttu-id="95b4d-129">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="95b4d-129">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="95b4d-130">Příkaz</span><span class="sxs-lookup"><span data-stu-id="95b4d-130">Command</span></span> | <span data-ttu-id="95b4d-131">Poznámky</span><span class="sxs-lookup"><span data-stu-id="95b4d-131">Notes</span></span> |
|---|---|
| [<span data-ttu-id="95b4d-132">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="95b4d-132">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="95b4d-133">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="95b4d-133">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="95b4d-134">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="95b4d-134">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | <span data-ttu-id="95b4d-135">Vytvoří virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="95b4d-135">Creates the virtual machines.</span></span>  |
| [<span data-ttu-id="95b4d-136">Seznam virtuálních počítačů az</span><span class="sxs-lookup"><span data-stu-id="95b4d-136">az vm list</span></span>](https://docs.microsoft.com/cli/azure/vm#list) | <span data-ttu-id="95b4d-137">Použít s `--query` , aby se před restartováním, jsou zřizovat virtuální počítače a potom získat ID virtuálních počítačů se je znovu spustit.</span><span class="sxs-lookup"><span data-stu-id="95b4d-137">Used with `--query` to ensure the VMs are provisioned before restarting them, and then to get the IDs of the VMs to restart them.</span></span> |
| [<span data-ttu-id="95b4d-138">Seznam zdrojů az</span><span class="sxs-lookup"><span data-stu-id="95b4d-138">az resource list</span></span>](https://docs.microsoft.com/cli/azure/vm#list) | <span data-ttu-id="95b4d-139">Použít s `--query` získat ID virtuálních počítačů pomocí značky.</span><span class="sxs-lookup"><span data-stu-id="95b4d-139">Used with `--query` to get the IDs of the VMs using the tag.</span></span> |
| [<span data-ttu-id="95b4d-140">AZ restartování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="95b4d-140">az vm restart</span></span>](https://docs.microsoft.com/cli/azure/vm#list) | <span data-ttu-id="95b4d-141">Restartování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="95b4d-141">Restarts the VMs.</span></span> |
| [<span data-ttu-id="95b4d-142">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="95b4d-142">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="95b4d-143">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="95b4d-143">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="95b4d-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="95b4d-144">Next steps</span></span>

<span data-ttu-id="95b4d-145">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="95b4d-145">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="95b4d-146">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="95b4d-146">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
