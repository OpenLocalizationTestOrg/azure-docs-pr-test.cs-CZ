---
title: "aaaHow tooresize virtuálního počítače s Linuxem pomocí hello 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Jak tooscale nahoru nebo vertikálně virtuální počítač s Linuxem, změnou hello velikost virtuálního počítače."
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
ms.openlocfilehash: 43dd955dc2f2dd9d1b2da07ecbfbf2459bcaa4d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="resize-a-linux-vm-with-azure-cli-10"></a><span data-ttu-id="3e57b-103">Změnit velikost virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku Azure 1.0</span><span class="sxs-lookup"><span data-stu-id="3e57b-103">Resize a Linux VM with Azure CLI 1.0</span></span>

## <a name="overview"></a><span data-ttu-id="3e57b-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="3e57b-104">Overview</span></span>

<span data-ttu-id="3e57b-105">Po zřízení virtuálního počítače (VM) hello virtuálních počítačů můžete škálovat nahoru nebo dolů, změnou hello [velikost virtuálního počítače][vm-sizes].</span><span class="sxs-lookup"><span data-stu-id="3e57b-105">After you provision a virtual machine (VM), you can scale hello VM up or down by changing hello [VM size][vm-sizes].</span></span> <span data-ttu-id="3e57b-106">V některých případech se musí nejprve navrácení hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3e57b-106">In some cases, you must deallocate hello VM first.</span></span> <span data-ttu-id="3e57b-107">To může nastat, když hello novou velikost není k dispozici v clusteru hello hardwaru, který je hostitelem hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3e57b-107">This can happen if hello new size is not available on hello hardware cluster that is hosting hello VM.</span></span>

<span data-ttu-id="3e57b-108">Tento článek ukazuje, jak tooresize a virtuální počítač s Linuxem pomocí hello [rozhraní příkazového řádku Azure][azure-cli].</span><span class="sxs-lookup"><span data-stu-id="3e57b-108">This article shows how tooresize a Linux VM using hello [Azure CLI][azure-cli].</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="3e57b-109">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="3e57b-109">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="3e57b-110">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="3e57b-110">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="3e57b-111">[Azure CLI 1.0](#resize-a-linux-vm) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="3e57b-111">[Azure CLI 1.0](#resize-a-linux-vm) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="3e57b-112">[Azure CLI 2.0](change-vm-size.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello</span><span class="sxs-lookup"><span data-stu-id="3e57b-112">[Azure CLI 2.0](change-vm-size.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="resize-a-linux-vm"></a><span data-ttu-id="3e57b-113">Změnit velikost virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="3e57b-113">Resize a Linux VM</span></span>
<span data-ttu-id="3e57b-114">tooresize virtuální počítač, proveďte následující kroky hello.</span><span class="sxs-lookup"><span data-stu-id="3e57b-114">tooresize a VM, perform hello following steps.</span></span>

1. <span data-ttu-id="3e57b-115">Spusťte následující příkaz rozhraní příkazového řádku hello.</span><span class="sxs-lookup"><span data-stu-id="3e57b-115">Run hello following CLI command.</span></span> <span data-ttu-id="3e57b-116">Tento příkaz vypíše hello velikosti virtuálních počítačů, které jsou k dispozici v clusteru hardwaru hello hello virtuální počítač je hostitelem.</span><span class="sxs-lookup"><span data-stu-id="3e57b-116">This command lists hello VM sizes that are available on hello hardware cluster where hello VM is hosted.</span></span>
   
    ```azurecli
    azure vm sizes -g myResourceGroup --vm-name myVM
    ```
2. <span data-ttu-id="3e57b-117">V případě, že je uvedena velikost potřeby hello spusťte následující příkaz tooresize hello virtuálních počítačů hello.</span><span class="sxs-lookup"><span data-stu-id="3e57b-117">If hello desired size is listed, run hello following command tooresize hello VM.</span></span>
   
    ```azurecli
    azure vm set -g myResourceGroup --vm-size <new-vm-size> -n myVM  \
        --enable-boot-diagnostics
        --boot-diagnostics-storage-uri https://mystorageaccount.blob.core.windows.net/ 
    ```
   
    <span data-ttu-id="3e57b-118">během tohoto procesu se restartuje Hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3e57b-118">hello VM will restart during this process.</span></span> <span data-ttu-id="3e57b-119">Po restartování hello bude přemapování stávajícího operačního systému a datové disky.</span><span class="sxs-lookup"><span data-stu-id="3e57b-119">After hello restart, your existing OS and data disks will be remapped.</span></span> <span data-ttu-id="3e57b-120">Nic na hello dočasné disku budou ztraceny.</span><span class="sxs-lookup"><span data-stu-id="3e57b-120">Anything on hello temporary disk will be lost.</span></span>
   
    <span data-ttu-id="3e57b-121">Použití hello `--enable-boot-diagnostics` možnost umožňuje [spouštění diagnostiky][boot-diagnostics], toolog související toostartup žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="3e57b-121">Use hello `--enable-boot-diagnostics` option enables [boot diagnostics][boot-diagnostics], toolog any errors related toostartup.</span></span>
3. <span data-ttu-id="3e57b-122">Jinak v případě potřeby hello není uvedena velikost spusťte hello následující příkazy toodeallocate hello virtuálního počítače, jeho velikost a pak restartujte hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3e57b-122">Otherwise, if hello desired size is not listed, run hello following commands toodeallocate hello VM, resize it, and then restart hello VM.</span></span>
   
    ```azurecli
    azure vm deallocate -g myResourceGroup myVM
    azure vm set -g myResourceGroup --vm-size <new-vm-size> -n myVM \
        --enable-boot-diagnostics --boot-diagnostics-storage-uri \
        https://mystorageaccount.blob.core.windows.net/ 
    azure vm start -g myResourceGroup myVM
    ```
   
   > [!WARNING]
   > <span data-ttu-id="3e57b-123">Rušení přidělení hello virtuální počítač také uvolní všechny dynamické IP adresy přiřazené toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3e57b-123">Deallocating hello VM also releases any dynamic IP addresses assigned toohello VM.</span></span> <span data-ttu-id="3e57b-124">ovlivněné nejsou Hello operačního systému a datové disky.</span><span class="sxs-lookup"><span data-stu-id="3e57b-124">hello OS and data disks are not affected.</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="3e57b-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3e57b-125">Next steps</span></span>
<span data-ttu-id="3e57b-126">Pro další škálovatelnost spustit více instancí virtuálního počítače a horizontální rozšíření kapacity. Další informace najdete v tématu [automaticky škálovat počítače se systémem Linux v sadě škálování virtuálního počítače][scale-set].</span><span class="sxs-lookup"><span data-stu-id="3e57b-126">For additional scalability, run multiple VM instances and scale out. For more information, see [Automatically scale Linux machines in a Virtual Machine Scale Set][scale-set].</span></span> 

<!-- links -->

[azure-cli]:../../cli-install-nodejs.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]:sizes.md
