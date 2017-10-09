---
title: "aaaHow tooresize virtuálního počítače s Linuxem pomocí hello 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Jak tooscale nahoru nebo vertikálně virtuální počítač s Linuxem, změnou hello velikost virtuálního počítače."
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
ms.openlocfilehash: e8fba485b5bcc7824f546de5cf3df77624a28008
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="resize-a-linux-virtual-machine-using-cli-20"></a><span data-ttu-id="dec8b-103">Změnit velikost virtuální počítač s Linuxem pomocí rozhraní příkazového řádku 2.0</span><span class="sxs-lookup"><span data-stu-id="dec8b-103">Resize a Linux virtual machine using CLI 2.0</span></span>

<span data-ttu-id="dec8b-104">Po zřízení virtuálního počítače (VM) hello virtuálních počítačů můžete škálovat nahoru nebo dolů, změnou hello [velikost virtuálního počítače][vm-sizes].</span><span class="sxs-lookup"><span data-stu-id="dec8b-104">After you provision a virtual machine (VM), you can scale hello VM up or down by changing hello [VM size][vm-sizes].</span></span> <span data-ttu-id="dec8b-105">V některých případech se musí nejprve navrácení hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="dec8b-105">In some cases, you must deallocate hello VM first.</span></span> <span data-ttu-id="dec8b-106">Je nutné toodeallocate hello virtuálních počítačů, v případě potřeby hello velikost není k dispozici v clusteru hello hardwaru, který je hostitelem hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="dec8b-106">You need toodeallocate hello VM if hello desired size is not available on hello hardware cluster that is hosting hello VM.</span></span> <span data-ttu-id="dec8b-107">Tento článek podrobně popisuje, jak hello tooresize a virtuální počítač s Linuxem pomocí Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="dec8b-107">This article details how tooresize a Linux VM with hello Azure CLI 2.0.</span></span> <span data-ttu-id="dec8b-108">Můžete také provést tyto kroky hello [Azure CLI 1.0](change-vm-size-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="dec8b-108">You can also perform these steps with hello [Azure CLI 1.0](change-vm-size-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="resize-a-vm"></a><span data-ttu-id="dec8b-109">Změna velikosti virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="dec8b-109">Resize a VM</span></span>
<span data-ttu-id="dec8b-110">tooresize virtuální počítač, budete potřebovat hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="dec8b-110">tooresize a VM, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

1. <span data-ttu-id="dec8b-111">Velikosti zobrazení hello seznam dostupných virtuálních počítačů v clusteru hardwaru hello je hostitelem hello virtuálních počítačů s [az virtuálních počítačů seznamu-virtuálních počítačů-změny velikosti options](/cli/azure/vm#list-vm-resize-options).</span><span class="sxs-lookup"><span data-stu-id="dec8b-111">View hello list of available VM sizes on hello hardware cluster where hello VM is hosted with [az vm list-vm-resize-options](/cli/azure/vm#list-vm-resize-options).</span></span> <span data-ttu-id="dec8b-112">Hello následující příklad uvádí velikosti virtuálních počítačů pro hello virtuálního počítače s názvem `myVM` ve skupině prostředků hello `myResourceGroup` oblasti:</span><span class="sxs-lookup"><span data-stu-id="dec8b-112">hello following example lists VM sizes for hello VM named `myVM` in hello resource group `myResourceGroup` region:</span></span>
   
    ```azurecli
    az vm list-vm-resize-options --resource-group myResourceGroup --name myVM --output table
    ```

2. <span data-ttu-id="dec8b-113">V případě potřeby je uvedena velikost virtuálního počítače hello změnit velikost virtuálního počítače s hello [změnit velikost virtuálního počítače az](/cli/azure/vm#resize).</span><span class="sxs-lookup"><span data-stu-id="dec8b-113">If hello desired VM size is listed, resize hello VM with [az vm resize](/cli/azure/vm#resize).</span></span> <span data-ttu-id="dec8b-114">Následující příklad změní velikost Hello hello virtuálního počítače s názvem `myVM` toohello `Standard_DS3_v2` velikost:</span><span class="sxs-lookup"><span data-stu-id="dec8b-114">hello following example resizes hello VM named `myVM` toohello `Standard_DS3_v2` size:</span></span>
   
    ```azurecli
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    ```
   
    <span data-ttu-id="dec8b-115">Hello virtuální počítač se restartuje během tohoto procesu.</span><span class="sxs-lookup"><span data-stu-id="dec8b-115">hello VM restarts during this process.</span></span> <span data-ttu-id="dec8b-116">Po restartování hello jsou přemapování stávajícího operačního systému a datové disky.</span><span class="sxs-lookup"><span data-stu-id="dec8b-116">After hello restart, your existing OS and data disks are remapped.</span></span> <span data-ttu-id="dec8b-117">Nic na hello dočasné disku se ztratí.</span><span class="sxs-lookup"><span data-stu-id="dec8b-117">Anything on hello temporary disk is lost.</span></span>

3. <span data-ttu-id="dec8b-118">V případě potřeby hello velikost virtuálního počítače není uveden, je nutné toofirst navrácení hello virtuálního počítače s [az OM deallocate](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="dec8b-118">If hello desired VM size is not listed, you need toofirst deallocate hello VM with [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="dec8b-119">Tento proces umožňuje toothen hello virtuální počítač se změněnou tooany velikost dostupné hello oblast podporuje a pak spustit.</span><span class="sxs-lookup"><span data-stu-id="dec8b-119">This process allows hello VM toothen be resized tooany size available that hello region supports and then started.</span></span> <span data-ttu-id="dec8b-120">Hello následující kroky navrácení, změnit velikost a spusťte hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="dec8b-120">hello following steps deallocate, resize, and then start hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>
   
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    az vm start --resource-group myResourceGroup --name myVM
    ```
   
   > [!WARNING]
   > <span data-ttu-id="dec8b-121">Rušení přidělení hello virtuální počítač také uvolní všechny dynamické IP adresy přiřazené toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="dec8b-121">Deallocating hello VM also releases any dynamic IP addresses assigned toohello VM.</span></span> <span data-ttu-id="dec8b-122">ovlivněné nejsou Hello operačního systému a datové disky.</span><span class="sxs-lookup"><span data-stu-id="dec8b-122">hello OS and data disks are not affected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dec8b-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dec8b-123">Next steps</span></span>
<span data-ttu-id="dec8b-124">Pro další škálovatelnost spustit více instancí virtuálního počítače a horizontální rozšíření kapacity. Další informace najdete v tématu [automaticky škálovat počítače se systémem Linux v sadě škálování virtuálního počítače][scale-set].</span><span class="sxs-lookup"><span data-stu-id="dec8b-124">For additional scalability, run multiple VM instances and scale out. For more information, see [Automatically scale Linux machines in a Virtual Machine Scale Set][scale-set].</span></span> 

<!-- links -->
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]:sizes.md
