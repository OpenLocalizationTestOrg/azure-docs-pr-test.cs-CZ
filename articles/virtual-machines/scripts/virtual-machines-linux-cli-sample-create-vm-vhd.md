---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvoření virtuálního počítače s virtuální pevný disk | Microsoft Docs"
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
ms.openlocfilehash: ce39092697a51e4e8a8e59ba8eb919955f616458
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-a-virtual-hard-disk"></a><span data-ttu-id="dba81-103">Vytvoření virtuálního počítače s virtuálním pevným diskem</span><span class="sxs-lookup"><span data-stu-id="dba81-103">Create a VM with a virtual hard disk</span></span>

<span data-ttu-id="dba81-104">Tento příklad vytvoří virtuální počítač pomocí virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="dba81-104">This example creates a virtual machine using a VHD.</span></span>
<span data-ttu-id="dba81-105">Vytvoří skupinu prostředků, účet úložiště a kontejner a potom vytvoří virtuálního počítače tím, že nahrajete hello virtuálního pevného disku toohello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="dba81-105">It creates a resource group, a storage account, and a container, then it creates a VM by uploading hello VHD toohello container.</span></span>
<span data-ttu-id="dba81-106">Nahradí hello ssh veřejného klíče pomocí veřejného klíče, aby měli přístup toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="dba81-106">It replaces hello ssh public key with your public key so that you have access toohello VM.</span></span>

<span data-ttu-id="dba81-107">Budete potřebovat spouštěcí virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="dba81-107">You'll need a bootable VHD.</span></span>
<span data-ttu-id="dba81-108">Můžete stáhnout z https://azclisamples.blob.core.windows.net/vhds/sample.vhd hello virtuálního pevného disku, který jsme použili nebo použít vlastní virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="dba81-108">You can download hello VHD that we used from https://azclisamples.blob.core.windows.net/vhds/sample.vhd, or use your own VHD.</span></span> <span data-ttu-id="dba81-109">skript Hello hledá `~/sample.vhd`.</span><span class="sxs-lookup"><span data-stu-id="dba81-109">hello script looks for `~/sample.vhd`.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="dba81-110">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="dba81-110">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-vhd/create-vm-vhd.sh "Create VM using a VHD")]

## <a name="clean-up-deployment"></a><span data-ttu-id="dba81-111">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="dba81-111">Clean up deployment</span></span> 

<span data-ttu-id="dba81-112">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="dba81-112">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete -n az-cli-vhd
```

## <a name="script-explanation"></a><span data-ttu-id="dba81-113">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="dba81-113">Script explanation</span></span>

<span data-ttu-id="dba81-114">Tento skript používá hello následující příkazy toocreate skupinu prostředků, virtuální počítač, skupinu dostupnosti, nástroj pro vyrovnávání zatížení a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="dba81-114">This script uses hello following commands toocreate a resource group, virtual machine, availability set, load balancer, and all related resources.</span></span> <span data-ttu-id="dba81-115">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="dba81-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="dba81-116">Příkaz</span><span class="sxs-lookup"><span data-stu-id="dba81-116">Command</span></span> | <span data-ttu-id="dba81-117">Poznámky</span><span class="sxs-lookup"><span data-stu-id="dba81-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="dba81-118">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="dba81-118">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="dba81-119">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="dba81-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="dba81-120">seznam účtů úložiště az</span><span class="sxs-lookup"><span data-stu-id="dba81-120">az storage account list</span></span>](https://docs.microsoft.com/cli/azure/storage/account#list) | <span data-ttu-id="dba81-121">Účty úložiště seznamy</span><span class="sxs-lookup"><span data-stu-id="dba81-121">Lists storage accounts</span></span> |
| [<span data-ttu-id="dba81-122">AZ název účtu úložiště kontrola-</span><span class="sxs-lookup"><span data-stu-id="dba81-122">az storage account check-name</span></span>](https://docs.microsoft.com/cli/azure/storage/account#check-name) | <span data-ttu-id="dba81-123">Ověří, zda je platný název účtu úložiště a že ještě neexistuje</span><span class="sxs-lookup"><span data-stu-id="dba81-123">Checks that a storage account name is valid and that it doesn't already exist</span></span> |
| [<span data-ttu-id="dba81-124">seznam klíčů účtu úložiště az</span><span class="sxs-lookup"><span data-stu-id="dba81-124">az storage account keys list</span></span>](https://docs.microsoft.com/cli/azure/storage/account/keys#list) | <span data-ttu-id="dba81-125">Obsahuje seznam klíčů pro účty úložiště hello</span><span class="sxs-lookup"><span data-stu-id="dba81-125">Lists keys for hello storage accounts</span></span> |
| [<span data-ttu-id="dba81-126">Objekt blob úložiště az existuje</span><span class="sxs-lookup"><span data-stu-id="dba81-126">az storage blob exists</span></span>](https://docs.microsoft.com/cli/azure/storage/blob#exists) | <span data-ttu-id="dba81-127">Ověří, zda existuje hello objektů blob</span><span class="sxs-lookup"><span data-stu-id="dba81-127">Checks whether hello blob exists</span></span> |
| [<span data-ttu-id="dba81-128">Vytvoření kontejneru úložiště az</span><span class="sxs-lookup"><span data-stu-id="dba81-128">az storage container create</span></span>](https://docs.microsoft.com/cli/azure/storage/container#create) | <span data-ttu-id="dba81-129">Vytvoří kontejner v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="dba81-129">Creates a container in a storage account.</span></span> |
| [<span data-ttu-id="dba81-130">Nahrání objektu blob úložiště az</span><span class="sxs-lookup"><span data-stu-id="dba81-130">az storage blob upload</span></span>](https://docs.microsoft.com/cli/azure/storage/blob#upload) | <span data-ttu-id="dba81-131">Vytvoří objekt blob v kontejneru hello odesílání hello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="dba81-131">Creates a blob in hello container by uploading hello VHD.</span></span> |
| [<span data-ttu-id="dba81-132">Seznam virtuálních počítačů az</span><span class="sxs-lookup"><span data-stu-id="dba81-132">az vm list</span></span>](https://docs.microsoft.com/cli/azure/vm#list) | <span data-ttu-id="dba81-133">Použít s `--query` zkontrolujte, zda text hello název virtuálního počítače se používá.</span><span class="sxs-lookup"><span data-stu-id="dba81-133">Used with `--query` check whether hello VM name is in use.</span></span> | 
| [<span data-ttu-id="dba81-134">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="dba81-134">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | <span data-ttu-id="dba81-135">Vytvoří hello virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="dba81-135">Creates hello virtual machines.</span></span> |
| [<span data-ttu-id="dba81-136">AZ virtuálních počítačů přístupu sada linux uživatele</span><span class="sxs-lookup"><span data-stu-id="dba81-136">az vm access set-linux-user</span></span>](https://docs.microsoft.com/cli/azure/vm/access#set-linux-user) | <span data-ttu-id="dba81-137">Obnoví hello SSH klíče toogive hello aktuální uživatel přístup toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="dba81-137">Resets hello SSH key toogive hello current user access toohello VM.</span></span> |
| [<span data-ttu-id="dba81-138">virtuální počítač az seznam ip adres</span><span class="sxs-lookup"><span data-stu-id="dba81-138">az vm list-ip-addresses</span></span>](https://docs.microsoft.com/cli/azure/vm#list-ip-addresses) | <span data-ttu-id="dba81-139">Získá IP adresu hello hello virtuální počítač, který byl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="dba81-139">Gets hello IP address of hello VM that was created.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="dba81-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dba81-140">Next steps</span></span>

<span data-ttu-id="dba81-141">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="dba81-141">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="dba81-142">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="dba81-142">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
