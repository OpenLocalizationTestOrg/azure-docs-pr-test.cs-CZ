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
# <a name="resize-a-linux-vm-with-azure-cli-10"></a>Změnit velikost virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku Azure 1.0

## <a name="overview"></a>Přehled

Po zřízení virtuálního počítače (VM) hello virtuálních počítačů můžete škálovat nahoru nebo dolů, změnou hello [velikost virtuálního počítače][vm-sizes]. V některých případech se musí nejprve navrácení hello virtuálních počítačů. To může nastat, když hello novou velikost není k dispozici v clusteru hello hardwaru, který je hostitelem hello virtuálních počítačů.

Tento článek ukazuje, jak tooresize a virtuální počítač s Linuxem pomocí hello [rozhraní příkazového řádku Azure][azure-cli].

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku
Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

- [Azure CLI 1.0](#resize-a-linux-vm) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)
- [Azure CLI 2.0](change-vm-size.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello


## <a name="resize-a-linux-vm"></a>Změnit velikost virtuálního počítače s Linuxem
tooresize virtuální počítač, proveďte následující kroky hello.

1. Spusťte následující příkaz rozhraní příkazového řádku hello. Tento příkaz vypíše hello velikosti virtuálních počítačů, které jsou k dispozici v clusteru hardwaru hello hello virtuální počítač je hostitelem.
   
    ```azurecli
    azure vm sizes -g myResourceGroup --vm-name myVM
    ```
2. V případě, že je uvedena velikost potřeby hello spusťte následující příkaz tooresize hello virtuálních počítačů hello.
   
    ```azurecli
    azure vm set -g myResourceGroup --vm-size <new-vm-size> -n myVM  \
        --enable-boot-diagnostics
        --boot-diagnostics-storage-uri https://mystorageaccount.blob.core.windows.net/ 
    ```
   
    během tohoto procesu se restartuje Hello virtuálních počítačů. Po restartování hello bude přemapování stávajícího operačního systému a datové disky. Nic na hello dočasné disku budou ztraceny.
   
    Použití hello `--enable-boot-diagnostics` možnost umožňuje [spouštění diagnostiky][boot-diagnostics], toolog související toostartup žádné chyby.
3. Jinak v případě potřeby hello není uvedena velikost spusťte hello následující příkazy toodeallocate hello virtuálního počítače, jeho velikost a pak restartujte hello virtuálních počítačů.
   
    ```azurecli
    azure vm deallocate -g myResourceGroup myVM
    azure vm set -g myResourceGroup --vm-size <new-vm-size> -n myVM \
        --enable-boot-diagnostics --boot-diagnostics-storage-uri \
        https://mystorageaccount.blob.core.windows.net/ 
    azure vm start -g myResourceGroup myVM
    ```
   
   > [!WARNING]
   > Rušení přidělení hello virtuální počítač také uvolní všechny dynamické IP adresy přiřazené toohello virtuálních počítačů. ovlivněné nejsou Hello operačního systému a datové disky.
   > 
   > 

## <a name="next-steps"></a>Další kroky
Pro další škálovatelnost spustit více instancí virtuálního počítače a horizontální rozšíření kapacity. Další informace najdete v tématu [automaticky škálovat počítače se systémem Linux v sadě škálování virtuálního počítače][scale-set]. 

<!-- links -->

[azure-cli]:../../cli-install-nodejs.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]:sizes.md
