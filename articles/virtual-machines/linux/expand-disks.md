---
title: "aaaExpand virtuální pevné disky na virtuální počítač s Linuxem v Azure | Microsoft Docs"
description: "Zjistěte, jak hello tooexpand virtuální pevné disky na virtuální počítač s Linuxem pomocí Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/21/2017
ms.author: iainfou
ms.openlocfilehash: 7c09a682cb4322c027e57667640e8f1f8e6612f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooexpand-virtual-hard-disks-on-a-linux-vm-with-hello-azure-cli"></a>Jak hello tooexpand virtuální pevné disky na virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure
Hello výchozí velikost virtuálního pevného disku pro hello operační systém (OS) je obvykle 30 GB na virtuální počítač s Linuxem (VM) v Azure. Můžete [přidat datových disků](add-disk.md) tooprovide dalšího volného místa, ale může také chcete tooexpand stávající datový disk. Tento článek popisuje, jak tooexpand spravovaná disky pro virtuální počítač s Linuxem pomocí hello 2.0 rozhraní příkazového řádku Azure. Můžete také rozšířit hello nespravované disk operačního systému s hello [Azure CLI 1.0](expand-disks-nodejs.md).

> [!WARNING]
> Ujistěte se, můžete zálohovat data před provedením disku změnit velikost operace vždy. Další informace najdete v tématu [zálohovat virtuální počítače s Linuxem v Azure](tutorial-backup-vms.md).

## <a name="expand-disk"></a>Rozbalte disk
Ujistěte se, že máte hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).

Tento článek vyžaduje existující virtuální počítač v Azure s alespoň jeden datový disk připojený a připravený. Pokud jste ještě není virtuální počítač, který můžete použít, najdete v části [vytvořit a připravit virtuální počítač s datovými disky](tutorial-manage-disks.md#create-and-attach-disks).

V hello následující ukázky, nahraďte vlastními hodnotami příklad názvy parametrů. Zahrnout názvy parametrů příklad *myResourceGroup* a *Můjvp*.

1. Operace na virtuálních pevných discích nelze provést s hello virtuální počítač spuštěn. Zrušit přidělení virtuálního počítače s [az OM deallocate](/cli/azure/vm#deallocate). Hello následující příklad zruší přidělení hello virtuálního počítače s názvem *Můjvp* v hello skupinu prostředků s názvem *myResourceGroup*:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > `az vm stop`neuvolní hello výpočetní prostředky. toorelease výpočetní prostředky, použijte `az vm deallocate`. Hello virtuálního počítače musí být navrácena tooexpand hello virtuální pevný disk.

2. Zobrazit seznam spravovaných disků ve skupině prostředků s [seznam disků az](/cli/azure/disk#list). Hello tento příklad zobrazuje seznam spravovaných disků ve skupině prostředků hello s názvem *myResourceGroup*:

    ```azurecli
    az disk list \
        --resource-group myResourceGroup \
        --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
        --output table
    ```

    Rozbalte hello požadované disku s [az disku aktualizace](/cli/azure/disk#update). Hello následující příklad rozšíří hello spravované disk s názvem *myDataDisk* toobe *200*Gb velikost:

    ```azurecli
    az disk update \
        --resource-group myResourceGroup \
        --name myDataDisk \
        --size-gb 200
    ```

    > [!NOTE]
    > Když rozšiřujete se spravovaným diskem, hello aktualizovat velikost je namapované toohello nejbližší velikost spravovaného disku. Tabulku hello spravovaných disků na dostupné velikosti a vrstvy, najdete v části [spravované disky přehled Azure – ceny a fakturace](../windows/managed-disks-overview.md#pricing-and-billing).

3. Spuštění virtuálního počítače s [spuštění virtuálního počítače az](/cli/azure/vm#start). Následující příklad spustí Hello hello virtuálního počítače s názvem *Můjvp* v hello skupinu prostředků s názvem *myResourceGroup*:

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

4. SSH tooyour virtuálních počítačů s příslušnými přihlašovacími údaji hello. Můžete získat hello veřejnou IP adresu vašeho virtuálního počítače s [az virtuálních počítačů zobrazit](/cli/azure/vm#show):

    ```azurecli
    az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
    ```

5. toouse hello rozšířit disku, musíte tooexpand hello základní oddíl a systému souborů.

    a. Pokud je již připojen, odpojte hello disk:

    ```bash
    sudo umount /dev/sdc1
    ```

    b. Použití `parted` tooview informace na disku a změňte velikost oddílu hello:

    ```bash
    sudo parted /dev/sdc
    ```

    Zobrazení informací o hello existující rozložení oddílů s `print`. Hello výstup je podobné toohello následující příklad, který ukazuje, že se základní disk hello 215 Gb velikost:

    ```bash
    GNU Parted 3.2
    Using /dev/sdc1
    Welcome tooGNU Parted! Type 'help' tooview a list of commands.
    (parted) print
    Model: Unknown Msft Virtual Disk (scsi)
    Disk /dev/sdc1: 215GB
    Sector size (logical/physical): 512B/4096B
    Partition Table: loop
    Disk Flags:
    
    Number  Start  End    Size   File system  Flags
        1      0.00B  107GB  107GB  ext4
    ```

    c. Rozbalte oddíl hello s `resizepart`. Zadejte číslo oddílu hello, *1*a velikost pro nový oddíl hello:

    ```bash
    (parted) resizepart
    Partition number? 1
    End?  [107GB]? 215GB
    ```

    d. tooexit, zadejte`quit`

5. S oddílem hello po změně velikosti, ověření konzistence oddílu hello s `e2fsck`:

    ```bash
    sudo e2fsck -f /dev/sdc1
    ```

6. Nyní změnit velikost hello filesystem s `resize2fs`:

    ```bash
    sudo resize2fs /dev/sdc1
    ```

7. Připojení hello oddílu toohello požadovaného umístění, jako například `/datadrive`:

    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```

8. změně velikosti disku tooverify hello operačního systému, použijte `df -h`. Hello následující příklad výstupu zobrazuje hello datová jednotka, */dev/sdc1*, je nyní 200 GB:

    ```bash
    Filesystem      Size   Used  Avail Use% Mounted on
    /dev/sdc1        197G   60M   187G   1% /datadrive
    ```

## <a name="next-steps"></a>Další kroky
Pokud potřebujete další úložiště, můžete také [přidat tooa datové disky virtuálního počítače s Linuxem](add-disk.md). Další informace o šifrování disku najdete v tématu [hello šifrovat disky na virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure](encrypt-disks.md).
