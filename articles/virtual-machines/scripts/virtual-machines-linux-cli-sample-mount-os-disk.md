---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - disk operačního systému připojit | Microsoft Docs"
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
ms.openlocfilehash: 5c614d09a64780575b70424d29052f1a6affec59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-vms-operating-system-disk"></a><span data-ttu-id="4d02d-103">Řešení potíží s diskem operačního systému virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="4d02d-103">Troubleshoot a VMs operating system disk</span></span>

<span data-ttu-id="4d02d-104">Tento skript připojí hello disk operačního systému virtuálního počítače se nezdařilo nebo problematické jako datový disk tooa druhý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="4d02d-104">This script mounts hello operating system disk of a failed or problematic virtual machine as a data disk tooa second virtual machine.</span></span> <span data-ttu-id="4d02d-105">To může být užitečné při řešení potíží s disku problémy nebo obnovení dat.</span><span class="sxs-lookup"><span data-stu-id="4d02d-105">This can be useful when troubleshooting disk issues or recovering data.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="4d02d-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="4d02d-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/mount-os-disk/mount-os-disk.sh "Quick Create VM")]

## <a name="script-explanation"></a><span data-ttu-id="4d02d-107">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="4d02d-107">Script explanation</span></span>

<span data-ttu-id="4d02d-108">Tento skript používá hello následující příkazy toocreate skupinu prostředků, virtuální počítač, a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="4d02d-108">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="4d02d-109">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="4d02d-109">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="4d02d-110">Příkaz</span><span class="sxs-lookup"><span data-stu-id="4d02d-110">Command</span></span> | <span data-ttu-id="4d02d-111">Poznámky</span><span class="sxs-lookup"><span data-stu-id="4d02d-111">Notes</span></span> |
|---|---|
| [<span data-ttu-id="4d02d-112">Zobrazit az virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="4d02d-112">az vm show</span></span>](https://docs.microsoft.com/cli/azure/vm#show) | <span data-ttu-id="4d02d-113">Vrátí seznam virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4d02d-113">Return a list of virtual machines.</span></span> <span data-ttu-id="4d02d-114">V takovém případě je možnost dotazu hello disku operačního systému virtuálního počítače používané tooreturn hello.</span><span class="sxs-lookup"><span data-stu-id="4d02d-114">In this case, hello query option is used tooreturn hello virtual machine operating system disk.</span></span> <span data-ttu-id="4d02d-115">Tato hodnota se pak přidá tooa název proměnné 'uri'.</span><span class="sxs-lookup"><span data-stu-id="4d02d-115">This value is then added tooa variable name ‘uri’.</span></span> |
| [<span data-ttu-id="4d02d-116">Odstranění virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="4d02d-116">az vm delete</span></span>](https://docs.microsoft.com/cli/azure/vm#delete) | <span data-ttu-id="4d02d-117">Odstraní virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="4d02d-117">Deletes a virtual machine.</span></span> |
| [<span data-ttu-id="4d02d-118">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="4d02d-118">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="4d02d-119">Vytvoří virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="4d02d-119">Creates a virtual machine.</span></span>  |
| [<span data-ttu-id="4d02d-120">připojit disk az virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="4d02d-120">az vm disk attach</span></span>](https://docs.microsoft.com/cli/azure/vm/disk#attach) | <span data-ttu-id="4d02d-121">Připojí disku tooa virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4d02d-121">Attaches a disk tooa virtual machine.</span></span> |
| [<span data-ttu-id="4d02d-122">virtuální počítač az seznam ip adres</span><span class="sxs-lookup"><span data-stu-id="4d02d-122">az vm list-ip-addresses</span></span>](https://docs.microsoft.com/cli/azure/vm#list-ip-addresses) | <span data-ttu-id="4d02d-123">Vrátí hello IP adresy virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4d02d-123">Returns hello IP addresses of a virtual machine.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4d02d-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4d02d-124">Next steps</span></span>

<span data-ttu-id="4d02d-125">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4d02d-125">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="4d02d-126">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4d02d-126">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
