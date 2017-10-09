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
# <a name="resize-a-linux-virtual-machine-using-cli-20"></a>Změnit velikost virtuální počítač s Linuxem pomocí rozhraní příkazového řádku 2.0

Po zřízení virtuálního počítače (VM) hello virtuálních počítačů můžete škálovat nahoru nebo dolů, změnou hello [velikost virtuálního počítače][vm-sizes]. V některých případech se musí nejprve navrácení hello virtuálních počítačů. Je nutné toodeallocate hello virtuálních počítačů, v případě potřeby hello velikost není k dispozici v clusteru hello hardwaru, který je hostitelem hello virtuálních počítačů. Tento článek podrobně popisuje, jak hello tooresize a virtuální počítač s Linuxem pomocí Azure CLI 2.0. Můžete také provést tyto kroky hello [Azure CLI 1.0](change-vm-size-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="resize-a-vm"></a>Změna velikosti virtuálního počítače
tooresize virtuální počítač, budete potřebovat hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).

1. Velikosti zobrazení hello seznam dostupných virtuálních počítačů v clusteru hardwaru hello je hostitelem hello virtuálních počítačů s [az virtuálních počítačů seznamu-virtuálních počítačů-změny velikosti options](/cli/azure/vm#list-vm-resize-options). Hello následující příklad uvádí velikosti virtuálních počítačů pro hello virtuálního počítače s názvem `myVM` ve skupině prostředků hello `myResourceGroup` oblasti:
   
    ```azurecli
    az vm list-vm-resize-options --resource-group myResourceGroup --name myVM --output table
    ```

2. V případě potřeby je uvedena velikost virtuálního počítače hello změnit velikost virtuálního počítače s hello [změnit velikost virtuálního počítače az](/cli/azure/vm#resize). Následující příklad změní velikost Hello hello virtuálního počítače s názvem `myVM` toohello `Standard_DS3_v2` velikost:
   
    ```azurecli
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    ```
   
    Hello virtuální počítač se restartuje během tohoto procesu. Po restartování hello jsou přemapování stávajícího operačního systému a datové disky. Nic na hello dočasné disku se ztratí.

3. V případě potřeby hello velikost virtuálního počítače není uveden, je nutné toofirst navrácení hello virtuálního počítače s [az OM deallocate](/cli/azure/vm#deallocate). Tento proces umožňuje toothen hello virtuální počítač se změněnou tooany velikost dostupné hello oblast podporuje a pak spustit. Hello následující kroky navrácení, změnit velikost a spusťte hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`:
   
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    az vm start --resource-group myResourceGroup --name myVM
    ```
   
   > [!WARNING]
   > Rušení přidělení hello virtuální počítač také uvolní všechny dynamické IP adresy přiřazené toohello virtuálních počítačů. ovlivněné nejsou Hello operačního systému a datové disky.

## <a name="next-steps"></a>Další kroky
Pro další škálovatelnost spustit více instancí virtuálního počítače a horizontální rozšíření kapacity. Další informace najdete v tématu [automaticky škálovat počítače se systémem Linux v sadě škálování virtuálního počítače][scale-set]. 

<!-- links -->
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]:sizes.md
