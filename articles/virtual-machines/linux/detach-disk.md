---
title: "aaaDetach datový disk od virtuálního počítače s Linuxem – Azure | Microsoft Docs"
description: "Přečtěte si toodetach datový disk z virtuálního počítače v Azure pomocí rozhraní příkazového řádku 2.0 nebo hello portálu Azure."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.topic: article
ms.date: 03/21/2017
ms.author: cynthn
ms.openlocfilehash: 1c6145fc97f13179457225e93e0fb7adc261a65b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodetach-a-data-disk-from-a-linux-virtual-machine"></a>Jak toodetach datový disk z virtuálního počítače systému Linux

Pokud již nepotřebujete datový disk, který je připojený tooa virtuální počítač, můžete ho snadno odpojit. Odebere hello disk z hello virtuálního počítače, ale neodstraní z úložiště. 

> [!WARNING]
> Pokud se odpojit disk není automaticky odstraněn. Pokud odebíráte tooPremium úložiště, budete moct dále tooincur poplatky za úložiště pro hello disk. Další informace najdete v části příliš[ceny a fakturace při použití služby Premium Storage](../../storage/common/storage-premium-storage.md#pricing-and-billing). 
> 
> 

Pokud chcete toouse hello existující data na disku hello znovu, můžete ji můžete opět připojit toohello stejného virtuálního počítače nebo jiný.  

## <a name="detach-a-data-disk-using-cli-20"></a>Odpojit datový disk pomocí rozhraní příkazového řádku 2.0

```azurecli
az vm disk detach -g myResourceGroup --vm-name myVm -n myDataDisk
```

Hello disk zůstává v úložišti, ale je už připojené tooa virtuální počítač.


## <a name="detach-a-data-disk-using-hello-portal"></a>Odpojit datový disk pomocí portálu hello
1. V portálu centra hello, vyberte **virtuální počítače**.
2. Vyberte hello virtuální počítač, který má datový disk hello toodetach a klikněte na **Zastavit** toodeallocate hello virtuálních počítačů.
3. V okně hello virtuálního počítače, vyberte **disky**.
4. Hello horní části hello **disky** vyberte **upravit**.
5. V hello **disky** okně toohello daleko vpravo od hello datový disk, které chcete toodetach, klikněte na tlačítko hello ![obrázek tlačítka odpojení](./media/detach-disk/detach.png) odpojit tlačítko.
5. Po hello disk odebrat, klikněte na Uložit na hello horní části okna hello.
6. V okně hello virtuální počítač, klikněte na tlačítko **přehled** a pak klikněte na tlačítko hello **spustit** tlačítko hello horní části hello okno toorestart hello virtuálních počítačů.

Hello disk zůstává v úložišti, ale je už připojené tooa virtuální počítač.








## <a name="next-steps"></a>Další kroky
Pokud chcete tooreuse hello datový disk, jste právě [připojte ji tooanother virtuálních počítačů](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

