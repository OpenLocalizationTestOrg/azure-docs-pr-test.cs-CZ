---
title: "Jak změnit velikost virtuálního počítače s Linuxem pomocí Azure CLI 1.0 | Microsoft Docs"
description: "Jak škálovat nahoru i dolů virtuální počítač s Linuxem změnou velikosti virtuálního počítače."
services: virtual-machines-linux
documentationcenter: na
author: mikewasson
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/16/2016
ms.author: mwasson
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 72f5a3cd6463befd5108040ed166984281bfc5f0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="resize-a-linux-vm-with-azure-cli-10"></a><span data-ttu-id="55cbc-103">Změnit velikost virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku Azure 1.0</span><span class="sxs-lookup"><span data-stu-id="55cbc-103">Resize a Linux VM with Azure CLI 1.0</span></span>

## <a name="overview"></a><span data-ttu-id="55cbc-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="55cbc-104">Overview</span></span>

<span data-ttu-id="55cbc-105">Po zřízení virtuálního počítače (VM) virtuálního počítače můžete škálovat nahoru nebo dolů, můžete změnit [velikost virtuálního počítače][vm-sizes].</span><span class="sxs-lookup"><span data-stu-id="55cbc-105">After you provision a virtual machine (VM), you can scale the VM up or down by changing the [VM size][vm-sizes].</span></span> <span data-ttu-id="55cbc-106">V některých případech se musí nejprve navrácení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="55cbc-106">In some cases, you must deallocate the VM first.</span></span> <span data-ttu-id="55cbc-107">To může dojít, pokud nová velikost není k dispozici na hardwaru clusteru, který je hostitelem virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="55cbc-107">This can happen if the new size is not available on the hardware cluster that is hosting the VM.</span></span>

<span data-ttu-id="55cbc-108">Tento článek ukazuje, jak změnit velikost virtuálního počítače s Linuxem pomocí [rozhraní příkazového řádku Azure][azure-cli].</span><span class="sxs-lookup"><span data-stu-id="55cbc-108">This article shows how to resize a Linux VM using the [Azure CLI][azure-cli].</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="55cbc-109">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="55cbc-109">CLI versions to complete the task</span></span>
<span data-ttu-id="55cbc-110">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="55cbc-110">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="55cbc-111">[Azure CLI 1.0](#resize-a-linux-vm) – naše rozhraní příkazového řádku pro classic a resource správu modelech nasazení (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="55cbc-111">[Azure CLI 1.0](#resize-a-linux-vm) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="55cbc-112">[Azure CLI 2.0](change-vm-size.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="55cbc-112">[Azure CLI 2.0](change-vm-size.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for the resource management deployment model</span></span>


## <a name="resize-a-linux-vm"></a><span data-ttu-id="55cbc-113">Změnit velikost virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="55cbc-113">Resize a Linux VM</span></span>
<span data-ttu-id="55cbc-114">Ke změně velikosti virtuálního počítače, proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="55cbc-114">To resize a VM, perform the following steps.</span></span>

1. <span data-ttu-id="55cbc-115">Spusťte následující příkaz rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="55cbc-115">Run the following CLI command.</span></span> <span data-ttu-id="55cbc-116">Tento příkaz vypíše velikosti virtuálních počítačů, které jsou k dispozici na hardwaru clusteru je hostitelem virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="55cbc-116">This command lists the VM sizes that are available on the hardware cluster where the VM is hosted.</span></span>
   
    ```azurecli
    azure vm sizes -g myResourceGroup --vm-name myVM
    ```
2. <span data-ttu-id="55cbc-117">Pokud požadovaná velikost je uveden, spusťte následující příkaz ke změně velikosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="55cbc-117">If the desired size is listed, run the following command to resize the VM.</span></span>
   
    ```azurecli
    azure vm set -g myResourceGroup --vm-size <new-vm-size> -n myVM  \
        --enable-boot-diagnostics
        --boot-diagnostics-storage-uri https://mystorageaccount.blob.core.windows.net/ 
    ```
   
    <span data-ttu-id="55cbc-118">Virtuální počítač se restartuje během tohoto procesu.</span><span class="sxs-lookup"><span data-stu-id="55cbc-118">The VM will restart during this process.</span></span> <span data-ttu-id="55cbc-119">Po restartování bude přemapování stávajícího operačního systému a datové disky.</span><span class="sxs-lookup"><span data-stu-id="55cbc-119">After the restart, your existing OS and data disks will be remapped.</span></span> <span data-ttu-id="55cbc-120">Nic na dočasné disku budou ztraceny.</span><span class="sxs-lookup"><span data-stu-id="55cbc-120">Anything on the temporary disk will be lost.</span></span>
   
    <span data-ttu-id="55cbc-121">Použití `--enable-boot-diagnostics` možnost umožňuje [spouštění diagnostiky][boot-diagnostics], do protokolu chyby související se spuštění.</span><span class="sxs-lookup"><span data-stu-id="55cbc-121">Use the `--enable-boot-diagnostics` option enables [boot diagnostics][boot-diagnostics], to log any errors related to startup.</span></span>
3. <span data-ttu-id="55cbc-122">Jinak Pokud není uvedené požadované velikosti, spusťte následující příkazy se zrušit přidělení virtuálního počítače, jeho velikost a pak restartujte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="55cbc-122">Otherwise, if the desired size is not listed, run the following commands to deallocate the VM, resize it, and then restart the VM.</span></span>
   
    ```azurecli
    azure vm deallocate -g myResourceGroup myVM
    azure vm set -g myResourceGroup --vm-size <new-vm-size> -n myVM \
        --enable-boot-diagnostics --boot-diagnostics-storage-uri \
        https://mystorageaccount.blob.core.windows.net/ 
    azure vm start -g myResourceGroup myVM
    ```
   
   > [!WARNING]
   > <span data-ttu-id="55cbc-123">Rušení přidělení virtuálního počítače uvolní také všechny dynamické IP adresy přiřazené k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="55cbc-123">Deallocating the VM also releases any dynamic IP addresses assigned to the VM.</span></span> <span data-ttu-id="55cbc-124">Ovlivněné nejsou disky operačního systému a data.</span><span class="sxs-lookup"><span data-stu-id="55cbc-124">The OS and data disks are not affected.</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="55cbc-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="55cbc-125">Next steps</span></span>
<span data-ttu-id="55cbc-126">Pro další škálovatelnost spustit více instancí virtuálního počítače a horizontální rozšíření kapacity.</span><span class="sxs-lookup"><span data-stu-id="55cbc-126">For additional scalability, run multiple VM instances and scale out.</span></span> <span data-ttu-id="55cbc-127">Další informace najdete v tématu [automaticky škálovat počítače se systémem Linux v sadě škálování virtuálního počítače][scale-set].</span><span class="sxs-lookup"><span data-stu-id="55cbc-127">For more information, see [Automatically scale Linux machines in a Virtual Machine Scale Set][scale-set].</span></span> 

<!-- links -->

[azure-cli]:../../cli-install-nodejs.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]:sizes.md
