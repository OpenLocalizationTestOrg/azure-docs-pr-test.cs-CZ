---
title: "Jak změnit velikost virtuálního počítače s Linuxem pomocí Azure CLI 2.0 | Microsoft Docs"
description: "Jak škálovat nahoru i dolů virtuální počítač s Linuxem změnou velikosti virtuálního počítače."
services: virtual-machines-linux
documentationcenter: na
author: mikewasson
manager: timlt
editor: 
tags: 
ms.assetid: e163f878-b919-45c5-9f5a-75a64f3b14a0
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/10/2017
ms.author: mwasson
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 23fc9f7f34732079682857d4ee685fe811751698
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="resize-a-linux-virtual-machine-using-cli-20"></a><span data-ttu-id="5f26e-103">Změnit velikost virtuální počítač s Linuxem pomocí rozhraní příkazového řádku 2.0</span><span class="sxs-lookup"><span data-stu-id="5f26e-103">Resize a Linux virtual machine using CLI 2.0</span></span>

<span data-ttu-id="5f26e-104">Po zřízení virtuálního počítače (VM) virtuálního počítače můžete škálovat nahoru nebo dolů, můžete změnit [velikost virtuálního počítače][vm-sizes].</span><span class="sxs-lookup"><span data-stu-id="5f26e-104">After you provision a virtual machine (VM), you can scale the VM up or down by changing the [VM size][vm-sizes].</span></span> <span data-ttu-id="5f26e-105">V některých případech se musí nejprve navrácení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5f26e-105">In some cases, you must deallocate the VM first.</span></span> <span data-ttu-id="5f26e-106">Budete muset zrušit přidělení virtuálního počítače, pokud požadovaná velikost není k dispozici na hardwaru clusteru, který je hostitelem virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5f26e-106">You need to deallocate the VM if the desired size is not available on the hardware cluster that is hosting the VM.</span></span> <span data-ttu-id="5f26e-107">Tento článek podrobné informace o tom, jak změnit velikost virtuálního počítače s Linuxem pomocí Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="5f26e-107">This article details how to resize a Linux VM with the Azure CLI 2.0.</span></span> <span data-ttu-id="5f26e-108">K provedení těchto kroků můžete také využít [Azure CLI 1.0](change-vm-size-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5f26e-108">You can also perform these steps with the [Azure CLI 1.0](change-vm-size-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="resize-a-vm"></a><span data-ttu-id="5f26e-109">Změna velikosti virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="5f26e-109">Resize a VM</span></span>
<span data-ttu-id="5f26e-110">Chcete-li změnit velikost virtuálního počítače, je třeba nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení k účtu Azure pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="5f26e-110">To resize a VM, you need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

1. <span data-ttu-id="5f26e-111">Zobrazit seznam dostupných velikostí virtuálních počítačů v clusteru hardwaru je hostitelem virtuálního počítače s [az virtuálních počítačů seznamu-virtuálních počítačů-změny velikosti options](/cli/azure/vm#list-vm-resize-options).</span><span class="sxs-lookup"><span data-stu-id="5f26e-111">View the list of available VM sizes on the hardware cluster where the VM is hosted with [az vm list-vm-resize-options](/cli/azure/vm#list-vm-resize-options).</span></span> <span data-ttu-id="5f26e-112">Následující příklad vypíše velikosti virtuálních počítačů pro virtuální počítač s názvem `myVM` ve skupině prostředků `myResourceGroup` oblasti:</span><span class="sxs-lookup"><span data-stu-id="5f26e-112">The following example lists VM sizes for the VM named `myVM` in the resource group `myResourceGroup` region:</span></span>
   
    ```azurecli
    az vm list-vm-resize-options --resource-group myResourceGroup --name myVM --output table
    ```

2. <span data-ttu-id="5f26e-113">V případě, že požadovaná velikost virtuálního počítače je uvedena, změňte velikost virtuálního počítače s [změnit velikost virtuálního počítače az](/cli/azure/vm#resize).</span><span class="sxs-lookup"><span data-stu-id="5f26e-113">If the desired VM size is listed, resize the VM with [az vm resize](/cli/azure/vm#resize).</span></span> <span data-ttu-id="5f26e-114">Následující příklad změní velikost virtuálního počítače s názvem `myVM` k `Standard_DS3_v2` velikost:</span><span class="sxs-lookup"><span data-stu-id="5f26e-114">The following example resizes the VM named `myVM` to the `Standard_DS3_v2` size:</span></span>
   
    ```azurecli
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    ```
   
    <span data-ttu-id="5f26e-115">Virtuální počítač se restartuje během tohoto procesu.</span><span class="sxs-lookup"><span data-stu-id="5f26e-115">The VM restarts during this process.</span></span> <span data-ttu-id="5f26e-116">Po restartování jsou mapovány stávajícího operačního systému a datové disky.</span><span class="sxs-lookup"><span data-stu-id="5f26e-116">After the restart, your existing OS and data disks are remapped.</span></span> <span data-ttu-id="5f26e-117">Nic na dočasné disku se ztratí.</span><span class="sxs-lookup"><span data-stu-id="5f26e-117">Anything on the temporary disk is lost.</span></span>

3. <span data-ttu-id="5f26e-118">Pokud není uvedené požadované velikosti virtuálního počítače, je třeba nejprve zrušit přidělení virtuálního počítače s [az OM deallocate](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="5f26e-118">If the desired VM size is not listed, you need to first deallocate the VM with [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="5f26e-119">Tento proces umožňuje virtuálnímu počítači potom změnit na libovolnou velikost, která je k dispozici, že oblast podporuje a pak spustit.</span><span class="sxs-lookup"><span data-stu-id="5f26e-119">This process allows the VM to then be resized to any size available that the region supports and then started.</span></span> <span data-ttu-id="5f26e-120">Následující kroky navrácení, přizpůsobit a pak spusťte virtuální počítač s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="5f26e-120">The following steps deallocate, resize, and then start the VM named `myVM` in the resource group named `myResourceGroup`:</span></span>
   
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    az vm start --resource-group myResourceGroup --name myVM
    ```
   
   > [!WARNING]
   > <span data-ttu-id="5f26e-121">Rušení přidělení virtuálního počítače uvolní také všechny dynamické IP adresy přiřazené k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="5f26e-121">Deallocating the VM also releases any dynamic IP addresses assigned to the VM.</span></span> <span data-ttu-id="5f26e-122">Ovlivněné nejsou disky operačního systému a data.</span><span class="sxs-lookup"><span data-stu-id="5f26e-122">The OS and data disks are not affected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5f26e-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5f26e-123">Next steps</span></span>
<span data-ttu-id="5f26e-124">Pro další škálovatelnost spustit více instancí virtuálního počítače a horizontální rozšíření kapacity.</span><span class="sxs-lookup"><span data-stu-id="5f26e-124">For additional scalability, run multiple VM instances and scale out.</span></span> <span data-ttu-id="5f26e-125">Další informace najdete v tématu [automaticky škálovat počítače se systémem Linux v sadě škálování virtuálního počítače][scale-set].</span><span class="sxs-lookup"><span data-stu-id="5f26e-125">For more information, see [Automatically scale Linux machines in a Virtual Machine Scale Set][scale-set].</span></span> 

<!-- links -->
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]:sizes.md
