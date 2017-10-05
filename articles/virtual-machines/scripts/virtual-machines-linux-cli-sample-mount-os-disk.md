---
title: "Ukázka skriptu Azure CLI - disk operačního systému připojit | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - připojení disku operačního systému"
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
ms.openlocfilehash: b54a94d833644ebfae4c1fac59e4753ab723ced4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-a-vms-operating-system-disk"></a><span data-ttu-id="f60df-103">Řešení potíží s diskem operačního systému virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="f60df-103">Troubleshoot a VMs operating system disk</span></span>

<span data-ttu-id="f60df-104">Tento skript připojí disk operačního systému virtuálního počítače se nezdařilo nebo problematické jako datový disk do druhého virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f60df-104">This script mounts the operating system disk of a failed or problematic virtual machine as a data disk to a second virtual machine.</span></span> <span data-ttu-id="f60df-105">To může být užitečné při řešení potíží s disku problémy nebo obnovení dat.</span><span class="sxs-lookup"><span data-stu-id="f60df-105">This can be useful when troubleshooting disk issues or recovering data.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="f60df-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="f60df-106">Sample script</span></span>

<span data-ttu-id="f60df-107">[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-machine/mount-os-disk/mount-os-disk.sh "rychlé vytvoření virtuálního počítače")]</span><span class="sxs-lookup"><span data-stu-id="f60df-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/mount-os-disk/mount-os-disk.sh "Quick Create VM")]</span></span>

## <a name="script-explanation"></a><span data-ttu-id="f60df-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="f60df-108">Script explanation</span></span>

<span data-ttu-id="f60df-109">Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="f60df-109">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="f60df-110">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="f60df-110">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="f60df-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="f60df-111">Command</span></span> | <span data-ttu-id="f60df-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="f60df-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f60df-113">Zobrazit az virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="f60df-113">az vm show</span></span>](https://docs.microsoft.com/cli/azure/vm#show) | <span data-ttu-id="f60df-114">Vrátí seznam virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f60df-114">Return a list of virtual machines.</span></span> <span data-ttu-id="f60df-115">V takovém případě se používá možnost dotazu vrátit disku operačního systému virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f60df-115">In this case, the query option is used to return the virtual machine operating system disk.</span></span> <span data-ttu-id="f60df-116">Tato hodnota se pak přidá k názvu proměnné 'uri'.</span><span class="sxs-lookup"><span data-stu-id="f60df-116">This value is then added to a variable name ‘uri’.</span></span> |
| [<span data-ttu-id="f60df-117">Odstranění virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="f60df-117">az vm delete</span></span>](https://docs.microsoft.com/cli/azure/vm#delete) | <span data-ttu-id="f60df-118">Odstraní virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="f60df-118">Deletes a virtual machine.</span></span> |
| [<span data-ttu-id="f60df-119">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="f60df-119">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="f60df-120">Vytvoří virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="f60df-120">Creates a virtual machine.</span></span>  |
| [<span data-ttu-id="f60df-121">připojit disk az virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="f60df-121">az vm disk attach</span></span>](https://docs.microsoft.com/cli/azure/vm/disk#attach) | <span data-ttu-id="f60df-122">Disk se připojuje k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="f60df-122">Attaches a disk to a virtual machine.</span></span> |
| [<span data-ttu-id="f60df-123">virtuální počítač az seznam ip adres</span><span class="sxs-lookup"><span data-stu-id="f60df-123">az vm list-ip-addresses</span></span>](https://docs.microsoft.com/cli/azure/vm#list-ip-addresses) | <span data-ttu-id="f60df-124">Vrátí IP adresy virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f60df-124">Returns the IP addresses of a virtual machine.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f60df-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f60df-125">Next steps</span></span>

<span data-ttu-id="f60df-126">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f60df-126">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="f60df-127">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f60df-127">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
