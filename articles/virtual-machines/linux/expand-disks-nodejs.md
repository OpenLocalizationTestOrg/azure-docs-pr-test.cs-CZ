---
title: "disk aaaExpand operačního systému na virtuální počítač s Linuxem s hello 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak tooexpand hello virtuální disk operačního systému (OS) na virtuální počítač s Linuxem pomocí hello 1.0 rozhraní příkazového řádku Azure a modelu nasazení Resource Manager hello"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 0db78c0b86b48b2c5358611e11bb0b7ad781a559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="expand-os-disk-on-a-linux-vm-using-hello-azure-cli-with-hello-azure-cli-10"></a>Rozbalte disk operačního systému na virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure hello hello Azure CLI 1.0
Hello výchozí velikost virtuálního pevného disku pro hello operační systém (OS) je obvykle 30 GB na virtuální počítač s Linuxem (VM) v Azure. Můžete [přidat datových disků](add-disk.md) tooprovide dalšího volného místa, ale také může přát tooexpand hello operačního systému disku. Tento článek popisuje, jak tooexpand hello operačního systému na disku pro virtuální počítač s Linuxem pomocí nespravované disky hello Azure CLI 1.0.

## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku
Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

- [Azure CLI 1.0](#prerequisites) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)
- [Azure CLI 2.0](expand-disks.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello

## <a name="prerequisites"></a>Požadavky
Je třeba hello [nejnovější Azure CLI 1.0](../../cli-install-nodejs.md) nainstalován a přihlášení tooan [účet Azure](https://azure.microsoft.com/pricing/free-trial/) pomocí režimu Resource Manager hello takto:

```azurecli
azure config mode arm
```

V hello následující ukázky, nahraďte vlastními hodnotami příklad názvy parametrů. Zahrnout názvy parametrů příklad *myResourceGroup* a *Můjvp*.

## <a name="expand-os-disk"></a>Rozbalení disku operačního systému

1. Operace na virtuálních pevných discích nelze provést s hello virtuální počítač spuštěn. Hello následující příklad zastaví a zruší přidělení hello virtuálního počítače s názvem *Můjvp* v hello skupinu prostředků s názvem *myResourceGroup*:

    ```azurecli
    azure vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > `azure vm stop`neuvolní hello výpočetní prostředky. toorelease výpočetní prostředky, použijte `azure vm deallocate`. Hello virtuálního počítače musí být navrácena tooexpand hello virtuální pevný disk.

2. Aktualizovat hello velikost hello nespravované disk operačního systému pomocí hello `azure vm set` příkaz. Následující příklad aktualizace Hello hello virtuálního počítače s názvem *Můjvp* v hello skupinu prostředků s názvem *myResourceGroup* toobe *50* GB:

    ```azurecli
    azure vm set \
        --resource-group myResourceGroup \
        --name myVM \
        --new-os-disk-size 50
    ```

3. Spusťte virtuální počítač následujícím způsobem:

    ```azurecli
    azure vm start --resource-group myResourceGroup --name myVM
    ```

4. SSH tooyour virtuálních počítačů s příslušnými přihlašovacími údaji hello. změně velikosti disku tooverify hello operačního systému, použijte `df -h`. primární oddíl hello ukazuje následující příklad výstupu Hello (*/dev/sda1*) je nyní 50 GB:

    ```bash
    Filesystem      Size  Used Avail Use% Mounted on
    udev            1.7G     0  1.7G   0% /dev
    tmpfs           344M  5.0M  340M   2% /run
    /dev/sda1        49G  1.3G   48G   3% /
    ```

## <a name="next-steps"></a>Další kroky
Pokud potřebujete další úložiště, můžete také [přidat tooa datové disky virtuálního počítače s Linuxem](add-disk.md). Další informace o šifrování disku najdete v tématu [hello šifrovat disky na virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure](encrypt-disks.md).
