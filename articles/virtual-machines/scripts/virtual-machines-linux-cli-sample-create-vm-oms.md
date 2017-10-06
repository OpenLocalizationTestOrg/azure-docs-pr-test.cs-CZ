---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvoření virtuálního počítače s Linuxem pomocí OMS monitorování | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření virtuálního počítače s Linuxem pomocí monitorování OMS"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 7a329d4f90a20e0e11faa1f5cafd0701574dc440
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-vm-with-operations-management-suite"></a><span data-ttu-id="5d560-103">Monitorování virtuálních počítačů pomocí služby Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="5d560-103">Monitor a VM with Operations Management Suite</span></span>

<span data-ttu-id="5d560-104">Tento skript vytvoří virtuální počítač Azure, nainstaluje hello agenta Operations Management Suite (OMS) a zaregistruje hello systém s pracovní prostor služby OMS.</span><span class="sxs-lookup"><span data-stu-id="5d560-104">This script creates an Azure Virtual Machine, installs hello Operations Management Suite (OMS) agent, and enrolls hello system with an OMS workspace.</span></span> <span data-ttu-id="5d560-105">Po spuštění skriptu hello se nebude zobrazovat v konzole OMS hello hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5d560-105">Once hello script has run, hello virtual machine will be visible in hello OMS console.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="5d560-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="5d560-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-monitor-oms/create-vm-monitor-oms.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a><span data-ttu-id="5d560-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="5d560-107">Clean up deployment</span></span> 

<span data-ttu-id="5d560-108">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="5d560-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="5d560-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="5d560-109">Script explanation</span></span>

<span data-ttu-id="5d560-110">Tento skript používá hello následující příkazy toocreate skupinu prostředků, virtuální počítač, a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="5d560-110">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="5d560-111">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="5d560-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="5d560-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="5d560-112">Command</span></span> | <span data-ttu-id="5d560-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="5d560-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="5d560-114">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="5d560-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="5d560-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="5d560-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="5d560-116">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="5d560-116">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="5d560-117">Vytvoří hello virtuální počítač a připojí ho toohello síťové karty, virtuální síť, podsíť a NSG.</span><span class="sxs-lookup"><span data-stu-id="5d560-117">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="5d560-118">Tento příkaz také určuje toobe bitové kopie virtuálního počítače hello používá a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="5d560-118">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="5d560-119">sada rozšíření virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="5d560-119">azure vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="5d560-120">Virtuální počítač spouští rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5d560-120">Runs a VM extension against a virtual machine.</span></span> <span data-ttu-id="5d560-121">V takovém případě hello rozšíření agenta Operations Management Suite je agent OMS hello použité tooinstall a zaregistrovat hello virtuálních počítačů v pracovním prostoru OMS.</span><span class="sxs-lookup"><span data-stu-id="5d560-121">In this case, hello Operations Management Suite agent extension is used tooinstall hello OMS agent and enroll hello VM in an OMS workspace.</span></span> |
| [<span data-ttu-id="5d560-122">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="5d560-122">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="5d560-123">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="5d560-123">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="5d560-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5d560-124">Next steps</span></span>

<span data-ttu-id="5d560-125">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5d560-125">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="5d560-126">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5d560-126">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
