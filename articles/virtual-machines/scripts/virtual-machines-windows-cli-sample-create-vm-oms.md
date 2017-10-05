---
title: "Azure CLI skriptu ukázkové – vytvoření virtuálního počítače Windows serveru 2016 s monitorováním OMS | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření virtuálního počítače Windows serveru 2016 s monitorováním OMS"
services: virtual-machines-Windows
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-Windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.openlocfilehash: ddb191163061dc47712e024c8c1d7a6f4bdf325b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-a-vm-with-operations-management-suite"></a><span data-ttu-id="7b181-103">Monitorování virtuálních počítačů pomocí služby Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="7b181-103">Monitor a VM with Operations Management Suite</span></span>

<span data-ttu-id="7b181-104">Tento skript vytvoří virtuální počítač Azure, nainstaluje agenta nástroje Operations Management Suite (OMS) a zaregistruje systém s pracovní prostor služby OMS.</span><span class="sxs-lookup"><span data-stu-id="7b181-104">This script creates an Azure Virtual Machine, installs the Operations Management Suite (OMS) agent, and enrolls the system with an OMS workspace.</span></span> <span data-ttu-id="7b181-105">Po spuštění skriptu se nebude zobrazovat v konzole OMS virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7b181-105">Once the script has run, the virtual machine will be visible in the OMS console.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="7b181-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="7b181-106">Sample script</span></span>

<span data-ttu-id="7b181-107">[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-machine/create-vm-monitor-oms/create-windows-vm-monitor-oms.sh "rychlé vytvoření virtuálního počítače")]</span><span class="sxs-lookup"><span data-stu-id="7b181-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-monitor-oms/create-windows-vm-monitor-oms.sh "Quick Create VM")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="7b181-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="7b181-108">Clean up deployment</span></span> 

<span data-ttu-id="7b181-109">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="7b181-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="7b181-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="7b181-110">Script explanation</span></span>

<span data-ttu-id="7b181-111">Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="7b181-111">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="7b181-112">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="7b181-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="7b181-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="7b181-113">Command</span></span> | <span data-ttu-id="7b181-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="7b181-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="7b181-115">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="7b181-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="7b181-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="7b181-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="7b181-117">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="7b181-117">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="7b181-118">Vytvoří virtuální počítač a připojí jej k síťové karty, virtuální sítě, podsítě a NSG.</span><span class="sxs-lookup"><span data-stu-id="7b181-118">Creates the virtual machine and connects it to the network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="7b181-119">Tento příkaz také Určuje bitovou kopii virtuálního počítače, který se má použít a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="7b181-119">This command also specifies the virtual machine image to be used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="7b181-120">sada rozšíření virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="7b181-120">azure vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="7b181-121">Virtuální počítač spouští rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7b181-121">Runs a VM extension against a virtual machine.</span></span> <span data-ttu-id="7b181-122">V takovém případě rozšíření agenta Operations Management Suite se používá pro instalaci agenta OMS a registraci virtuálních počítačů v pracovním prostoru OMS.</span><span class="sxs-lookup"><span data-stu-id="7b181-122">In this case, the Operations Management Suite agent extension is used to install the OMS agent and enroll the VM in an OMS workspace.</span></span> |
| [<span data-ttu-id="7b181-123">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="7b181-123">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="7b181-124">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="7b181-124">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="7b181-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7b181-125">Next steps</span></span>

<span data-ttu-id="7b181-126">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7b181-126">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="7b181-127">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7b181-127">Additional virtual machine CLI script samples can be found in the [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
