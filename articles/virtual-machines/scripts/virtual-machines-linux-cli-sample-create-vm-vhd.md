---
title: "Rozhraní příkazového řádku Azure Script ukázka – vytvoření virtuálního počítače s virtuální pevný disk | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření virtuálního počítače pomocí virtuálního pevného disku."
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
ms.date: 03/09/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: fab65296a552c1839522c5254a868a3dc96227f7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-a-virtual-hard-disk"></a><span data-ttu-id="98ce1-103">Vytvoření virtuálního počítače s virtuálním pevným diskem</span><span class="sxs-lookup"><span data-stu-id="98ce1-103">Create a VM with a virtual hard disk</span></span>

<span data-ttu-id="98ce1-104">Tento příklad vytvoří virtuální počítač pomocí virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="98ce1-104">This example creates a virtual machine using a VHD.</span></span>
<span data-ttu-id="98ce1-105">Vytvoří skupinu prostředků, účet úložiště a kontejner a potom vytvoří virtuální počítač odesláním disku VHD do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="98ce1-105">It creates a resource group, a storage account, and a container, then it creates a VM by uploading the VHD to the container.</span></span>
<span data-ttu-id="98ce1-106">Nahradí ssh veřejný klíč pomocí veřejného klíče, máte přístup k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="98ce1-106">It replaces the ssh public key with your public key so that you have access to the VM.</span></span>

<span data-ttu-id="98ce1-107">Budete potřebovat spouštěcí virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="98ce1-107">You'll need a bootable VHD.</span></span>
<span data-ttu-id="98ce1-108">Můžete stáhnout z https://azclisamples.blob.core.windows.net/vhds/sample.vhd virtuální pevný disk, který jsme použili nebo použít vlastní virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="98ce1-108">You can download the VHD that we used from https://azclisamples.blob.core.windows.net/vhds/sample.vhd, or use your own VHD.</span></span> <span data-ttu-id="98ce1-109">Skript hledá `~/sample.vhd`.</span><span class="sxs-lookup"><span data-stu-id="98ce1-109">The script looks for `~/sample.vhd`.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="98ce1-110">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="98ce1-110">Sample script</span></span>

<span data-ttu-id="98ce1-111">[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-machine/create-vm-vhd/create-vm-vhd.sh "vytvoření virtuálního počítače pomocí virtuálního pevného disku")]</span><span class="sxs-lookup"><span data-stu-id="98ce1-111">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-vhd/create-vm-vhd.sh "Create VM using a VHD")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="98ce1-112">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="98ce1-112">Clean up deployment</span></span> 

<span data-ttu-id="98ce1-113">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="98ce1-113">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete -n az-cli-vhd
```

## <a name="script-explanation"></a><span data-ttu-id="98ce1-114">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="98ce1-114">Script explanation</span></span>

<span data-ttu-id="98ce1-115">Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální počítač, skupina dostupnosti, nástroj pro vyrovnávání zatížení a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="98ce1-115">This script uses the following commands to create a resource group, virtual machine, availability set, load balancer, and all related resources.</span></span> <span data-ttu-id="98ce1-116">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="98ce1-116">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="98ce1-117">Příkaz</span><span class="sxs-lookup"><span data-stu-id="98ce1-117">Command</span></span> | <span data-ttu-id="98ce1-118">Poznámky</span><span class="sxs-lookup"><span data-stu-id="98ce1-118">Notes</span></span> |
|---|---|
| [<span data-ttu-id="98ce1-119">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="98ce1-119">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="98ce1-120">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="98ce1-120">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="98ce1-121">seznam účtů úložiště az</span><span class="sxs-lookup"><span data-stu-id="98ce1-121">az storage account list</span></span>](https://docs.microsoft.com/cli/azure/storage/account#list) | <span data-ttu-id="98ce1-122">Účty úložiště seznamy</span><span class="sxs-lookup"><span data-stu-id="98ce1-122">Lists storage accounts</span></span> |
| [<span data-ttu-id="98ce1-123">AZ název účtu úložiště kontrola-</span><span class="sxs-lookup"><span data-stu-id="98ce1-123">az storage account check-name</span></span>](https://docs.microsoft.com/cli/azure/storage/account#check-name) | <span data-ttu-id="98ce1-124">Ověří, zda je platný název účtu úložiště a že ještě neexistuje</span><span class="sxs-lookup"><span data-stu-id="98ce1-124">Checks that a storage account name is valid and that it doesn't already exist</span></span> |
| [<span data-ttu-id="98ce1-125">seznam klíčů účtu úložiště az</span><span class="sxs-lookup"><span data-stu-id="98ce1-125">az storage account keys list</span></span>](https://docs.microsoft.com/cli/azure/storage/account/keys#list) | <span data-ttu-id="98ce1-126">Obsahuje seznam klíčů pro účty úložiště</span><span class="sxs-lookup"><span data-stu-id="98ce1-126">Lists keys for the storage accounts</span></span> |
| [<span data-ttu-id="98ce1-127">Objekt blob úložiště az existuje</span><span class="sxs-lookup"><span data-stu-id="98ce1-127">az storage blob exists</span></span>](https://docs.microsoft.com/cli/azure/storage/blob#exists) | <span data-ttu-id="98ce1-128">Ověří, zda existuje objektu blob</span><span class="sxs-lookup"><span data-stu-id="98ce1-128">Checks whether the blob exists</span></span> |
| [<span data-ttu-id="98ce1-129">Vytvoření kontejneru úložiště az</span><span class="sxs-lookup"><span data-stu-id="98ce1-129">az storage container create</span></span>](https://docs.microsoft.com/cli/azure/storage/container#create) | <span data-ttu-id="98ce1-130">Vytvoří kontejner v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="98ce1-130">Creates a container in a storage account.</span></span> |
| [<span data-ttu-id="98ce1-131">Nahrání objektu blob úložiště az</span><span class="sxs-lookup"><span data-stu-id="98ce1-131">az storage blob upload</span></span>](https://docs.microsoft.com/cli/azure/storage/blob#upload) | <span data-ttu-id="98ce1-132">Vytvoří objekt blob v kontejneru, tím, že nahrajete virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="98ce1-132">Creates a blob in the container by uploading the VHD.</span></span> |
| [<span data-ttu-id="98ce1-133">Seznam virtuálních počítačů az</span><span class="sxs-lookup"><span data-stu-id="98ce1-133">az vm list</span></span>](https://docs.microsoft.com/cli/azure/vm#list) | <span data-ttu-id="98ce1-134">Použít s `--query` zkontrolujte, zda název virtuálního počítače je používán.</span><span class="sxs-lookup"><span data-stu-id="98ce1-134">Used with `--query` check whether the VM name is in use.</span></span> | 
| [<span data-ttu-id="98ce1-135">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="98ce1-135">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | <span data-ttu-id="98ce1-136">Vytvoří virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="98ce1-136">Creates the virtual machines.</span></span> |
| [<span data-ttu-id="98ce1-137">AZ virtuálních počítačů přístupu sada linux uživatele</span><span class="sxs-lookup"><span data-stu-id="98ce1-137">az vm access set-linux-user</span></span>](https://docs.microsoft.com/cli/azure/vm/access#set-linux-user) | <span data-ttu-id="98ce1-138">Obnoví klíč SSH poskytnout aktuální uživatel přístup k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="98ce1-138">Resets the SSH key to give the current user access to the VM.</span></span> |
| [<span data-ttu-id="98ce1-139">virtuální počítač az seznam ip adres</span><span class="sxs-lookup"><span data-stu-id="98ce1-139">az vm list-ip-addresses</span></span>](https://docs.microsoft.com/cli/azure/vm#list-ip-addresses) | <span data-ttu-id="98ce1-140">Získá IP adresu virtuálního počítače, který byl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="98ce1-140">Gets the IP address of the VM that was created.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="98ce1-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="98ce1-141">Next steps</span></span>

<span data-ttu-id="98ce1-142">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="98ce1-142">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="98ce1-143">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="98ce1-143">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
