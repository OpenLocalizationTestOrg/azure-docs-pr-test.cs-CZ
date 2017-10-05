---
title: "Rozhraní příkazového řádku Azure ukázkový skript – nasazení zásobníku svítilna ve Škálovací sadě Vyrovnávání zatížení sítě Machin virtuální | Microsoft Docs"
description: "Použití rozšíření vlastních skriptů k nasazení svítilna zásobníku v zatížení = sad v Azure škálování vyrovnáváním virtuálního počítače."
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
ms.date: 04/05/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: 4007e8c85c0ff24bf5030881eac666582714eae3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-the-lamp-stack-in-a-load-balanced-virtual-machine-scale-set"></a><span data-ttu-id="67dd4-103">Nasazení svítilna zásobníku v sadě škálování Vyrovnávání zatížení sítě virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="67dd4-103">Deploy the LAMP stack in a load-balanced virtual machine scale set</span></span>

<span data-ttu-id="67dd4-104">Tento příklad vytvoří škálovací sadu virtuálních počítačů a platí rozšíření, které spouští vlastní skript k nasazení zásobníku svítilna na každém virtuálním počítači v sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="67dd4-104">This example creates a virtual machine scale set and applies an extension that runs a custom script to deploy the LAMP stack on each virtual machine in the scale set.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a><span data-ttu-id="67dd4-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="67dd4-105">Sample script</span></span>

<span data-ttu-id="67dd4-106">[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/build-stack.sh "vytvořit škálování virtuálních počítačů s svítilna zásobníku")]</span><span class="sxs-lookup"><span data-stu-id="67dd4-106">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/build-stack.sh "Create virtual machine scale set with LAMP stack")]</span></span>

## <a name="connect"></a><span data-ttu-id="67dd4-107">Připojení</span><span class="sxs-lookup"><span data-stu-id="67dd4-107">Connect</span></span>

<span data-ttu-id="67dd4-108">Pomocí tohoto kódu Pokud chcete zjistit, jak se připojit k virtuální počítače a vaše škálování nastavit.</span><span class="sxs-lookup"><span data-stu-id="67dd4-108">Use this code to see how to connect to your VMs and your scale set.</span></span>

<span data-ttu-id="67dd4-109">[!code-azurecli[hlavní](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/how-to-access.sh "přístup škálovací sadu virtuálních počítačů")]</span><span class="sxs-lookup"><span data-stu-id="67dd4-109">[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/how-to-access.sh "Access the virtual machine scale set")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="67dd4-110">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="67dd4-110">Clean up deployment</span></span> 

<span data-ttu-id="67dd4-111">Spusťte následující příkaz pro odebrání skupiny prostředků, sadě škálování a virtuální počítače a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="67dd4-111">Run the following command to remove the resource group, the scale set and VMs, and all related resources.</span></span>

```azurecli-interactive 
az group delete -n myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="67dd4-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="67dd4-112">Script explanation</span></span>

<span data-ttu-id="67dd4-113">Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální počítač, skupina dostupnosti, nástroj pro vyrovnávání zatížení a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="67dd4-113">This script uses the following commands to create a resource group, virtual machine, availability set, load balancer, and all related resources.</span></span> <span data-ttu-id="67dd4-114">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="67dd4-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="67dd4-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="67dd4-115">Command</span></span> | <span data-ttu-id="67dd4-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="67dd4-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="67dd4-117">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="67dd4-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="67dd4-118">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="67dd4-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="67dd4-119">Vytvoření az vmss</span><span class="sxs-lookup"><span data-stu-id="67dd4-119">az vmss create</span></span>](https://docs.microsoft.com/cli/azure/vmss#create) | <span data-ttu-id="67dd4-120">Vytvoří škálovací sadu virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="67dd4-120">Creates a virtual machine scale set</span></span> |
| [<span data-ttu-id="67dd4-121">Vytvořit pravidlo lb az sítě</span><span class="sxs-lookup"><span data-stu-id="67dd4-121">az network lb rule create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | <span data-ttu-id="67dd4-122">Přidat koncový bod Vyrovnávání zatížení sítě</span><span class="sxs-lookup"><span data-stu-id="67dd4-122">Add a load-balanced endpoint</span></span> |
| [<span data-ttu-id="67dd4-123">sada rozšíření vmss az</span><span class="sxs-lookup"><span data-stu-id="67dd4-123">az vmss extension set</span></span>](https://docs.microsoft.com/cli/azure/vmss/extension#set) | <span data-ttu-id="67dd4-124">Vytváření rozšíření, který spouští vlastních skriptů při nasazení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="67dd4-124">Create the extension that runs the custom script on deployment of a VM</span></span> |
| [<span data-ttu-id="67dd4-125">AZ vmss aktualizace instance</span><span class="sxs-lookup"><span data-stu-id="67dd4-125">az vmss update-instances</span></span>](https://docs.microsoft.com/cli/azure/vmss#update-instances) | <span data-ttu-id="67dd4-126">Spusťte vlastní skript v rámci instancí virtuálních počítačů, které byly nasazené, než bylo použito rozšíření pro sadu škálování.</span><span class="sxs-lookup"><span data-stu-id="67dd4-126">Run the custom script on the VM instances that were deployed before the extension was applied to the scale set.</span></span> |
| [<span data-ttu-id="67dd4-127">AZ vmss škálování</span><span class="sxs-lookup"><span data-stu-id="67dd4-127">az vmss scale</span></span>](https://docs.microsoft.com/cli/azure/vmss#scale) | <span data-ttu-id="67dd4-128">Škálování měřítka nastavený přidáním více instancí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="67dd4-128">Scale up the scale set by adding more VM instances.</span></span> <span data-ttu-id="67dd4-129">Vlastní skript běží v nich při jejich nasazení.</span><span class="sxs-lookup"><span data-stu-id="67dd4-129">The custom script is run on these when they are deployed.</span></span> |
| [<span data-ttu-id="67dd4-130">seznam veřejné ip az sítě</span><span class="sxs-lookup"><span data-stu-id="67dd4-130">az network public-ip list</span></span>](https://docs.microsoft.com/cli/azure/network/public-ip#list) | <span data-ttu-id="67dd4-131">Získáte IP adresy virtuálních počítačů vytvořené vzorku.</span><span class="sxs-lookup"><span data-stu-id="67dd4-131">Get the IP addresses of the VMs created by the sample.</span></span> |
| [<span data-ttu-id="67dd4-132">Zobrazit lb az sítě</span><span class="sxs-lookup"><span data-stu-id="67dd4-132">az network lb show</span></span>](https://docs.microsoft.com/cli/azure/network/lb#show) | <span data-ttu-id="67dd4-133">Získejte front-endu a back-end porty používané pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="67dd4-133">Get the frontend and backend ports used by the load balancer.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="67dd4-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="67dd4-134">Next steps</span></span>

<span data-ttu-id="67dd4-135">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="67dd4-135">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="67dd4-136">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="67dd4-136">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
