---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvoření virtuálního počítače Windows serveru 2016 s OMS monitorování | Microsoft Docs"
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
ms.openlocfilehash: 4f070430ccc5d5402ed4a80ead3b78eff25dcd1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-vm-with-operations-management-suite"></a><span data-ttu-id="845a2-103">Monitorování virtuálních počítačů pomocí služby Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="845a2-103">Monitor a VM with Operations Management Suite</span></span>

<span data-ttu-id="845a2-104">Tento skript vytvoří virtuální počítač Azure, nainstaluje hello agenta Operations Management Suite (OMS) a zaregistruje hello systém s pracovní prostor služby OMS.</span><span class="sxs-lookup"><span data-stu-id="845a2-104">This script creates an Azure Virtual Machine, installs hello Operations Management Suite (OMS) agent, and enrolls hello system with an OMS workspace.</span></span> <span data-ttu-id="845a2-105">Po spuštění skriptu hello se nebude zobrazovat v konzole OMS hello hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="845a2-105">Once hello script has run, hello virtual machine will be visible in hello OMS console.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="845a2-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="845a2-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-monitor-oms/create-windows-vm-monitor-oms.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a><span data-ttu-id="845a2-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="845a2-107">Clean up deployment</span></span> 

<span data-ttu-id="845a2-108">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="845a2-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="845a2-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="845a2-109">Script explanation</span></span>

<span data-ttu-id="845a2-110">Tento skript používá hello následující příkazy toocreate skupinu prostředků, virtuální počítač, a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="845a2-110">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="845a2-111">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="845a2-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="845a2-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="845a2-112">Command</span></span> | <span data-ttu-id="845a2-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="845a2-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="845a2-114">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="845a2-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="845a2-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="845a2-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="845a2-116">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="845a2-116">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="845a2-117">Vytvoří hello virtuální počítač a připojí ho toohello síťové karty, virtuální síť, podsíť a NSG.</span><span class="sxs-lookup"><span data-stu-id="845a2-117">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="845a2-118">Tento příkaz také určuje toobe bitové kopie virtuálního počítače hello používá a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="845a2-118">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="845a2-119">sada rozšíření virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="845a2-119">azure vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="845a2-120">Virtuální počítač spouští rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="845a2-120">Runs a VM extension against a virtual machine.</span></span> <span data-ttu-id="845a2-121">V takovém případě hello rozšíření agenta Operations Management Suite je agent OMS hello použité tooinstall a zaregistrovat hello virtuálních počítačů v pracovním prostoru OMS.</span><span class="sxs-lookup"><span data-stu-id="845a2-121">In this case, hello Operations Management Suite agent extension is used tooinstall hello OMS agent and enroll hello VM in an OMS workspace.</span></span> |
| [<span data-ttu-id="845a2-122">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="845a2-122">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="845a2-123">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="845a2-123">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="845a2-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="845a2-124">Next steps</span></span>

<span data-ttu-id="845a2-125">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="845a2-125">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="845a2-126">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuálního počítače Windows Azure dokumentaci](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="845a2-126">Additional virtual machine CLI script samples can be found in hello [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
