---
title: "aaaMount Azure File storage ve virtuální počítače s Linuxem pomocí protokolu SMB 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Jak toomount Azure File storage ve virtuální počítače s Linuxem pomocí protokolu SMB"
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/07/2016
ms.author: v-livech
ms.openlocfilehash: 14a4224228cadb0ae2f05e8e5c8022ee84f138a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="mount-azure-file-storage-on-linux-vms-by-using-smb-with-azure-cli-10"></a>Připojení Azure File storage na virtuální počítače s Linuxem pomocí protokolu SMB 1.0 rozhraní příkazového řádku Azure

Tento článek ukazuje, jak toomount Azure File storage na virtuální počítač s Linuxem pomocí hello zpráva bloku protokol Server (SMB). File storage nabízí sdílené složky v cloudu hello prostřednictvím hello standardní protokol SMB. Hello požadavky jsou:

* [Účet Azure](https://azure.microsoft.com/pricing/free-trial/)
* [Secure Shell (SSH) soubory veřejného a privátního klíče](mac-create-ssh-keys.md)

## <a name="cli-versions-toouse"></a>Toouse verze rozhraní příkazového řádku
Hello úloh můžete dokončit pomocí jedné z hello následující verze rozhraní příkazového řádku (CLI):

- [Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)
- [Azure CLI 2.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)-naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello


## <a name="quick-commands"></a>Rychlé příkazy
Úloha hello tooaccomplish rychle, postupujte podle kroků hello v této části. Podrobné informace a kontext, začít ve hello ["Podrobný návod"](mount-azure-file-storage-on-linux-using-smb.md#detailed-walkthrough) části.

### <a name="prerequisites"></a>Požadavky
* Skupinu prostředků.
* Virtuální síť Azure
* Skupina zabezpečení sítě pomocí SSH příchozí
* Podsíť
* Účet úložiště Azure
* Klíče účtu úložiště Azure
* Sdílenou složku Azure File storage
* Virtuální počítač s Linuxem

Nahradí všechny příklady s vlastním nastavením.

### <a name="create-a-directory-for-hello-local-mount"></a>Vytvořte adresář pro hello místního připojení

```bash
mkdir -p /mnt/mymountpoint
```

### <a name="mount-hello-file-storage-smb-share-toohello-mount-point"></a>Připojit hello soubor úložiště SMB sdílenou složku toohello přípojný bod

```bash
sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mymountpoint -o vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

### <a name="persist-hello-mount-after-a-reboot"></a>Zachovat připojení hello po restartu systému
Přidejte následující řádek příliš hello`/etc/fstab`:

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## <a name="detailed-walkthrough"></a>Podrobný postup

File storage nabízí sdílené složky v cloudu hello, které používají standardní protokol SMB hello. Hello nejnovější verze služby úložiště File můžete také připojit sdílenou složku z jakékoli operační systém, který podporuje protokol SMB 3.0. Pokud používáte připojení protokolu SMB v systému Linux, získáte snadno zálohy tooa robustní, trvalé archivováním umístění úložiště podporovaný SLA.

Přesunutí souborů z připojení SMB tooan virtuálního počítače, který je hostován na soubor úložiště je že skvělým způsobem toodebug protokoly. Je to způsobeno hello sdílet stejný protokol SMB může být připojen místně tooyour Mac, Linux nebo Windows pracovní stanice. SMB není hello nejlepší řešení pro streamování Linux nebo aplikace protokolů v reálném čase, protože není hello protokolu SMB vytvořené toohandle těchto funkcí velkou protokolování. Nástroje protokolování vyhrazené, jednotná vrstvy, jako je Fluentd bude vhodnější než SMB pro shromažďování Linux a aplikace protokolování výstupu.

Pro tento podrobný návod vytvoříme hello požadavky potřeby toofirst vytvořit hello sdílenou složku úložiště a následné připojení prostřednictvím protokolu SMB na virtuální počítač s Linuxem.

1. Vytvořte účet úložiště Azure pomocí hello následující kód:

    ```azurecli
    azure storage account create myStorageAccount \
    --sku-name lrs \
    --kind storage \
    -l westus \
    -g myResourceGroup
    ```

2. Zobrazit hello klíče účtu úložiště.

    Když vytvoříte účet úložiště, klíče účtu hello jsou vytvořeny v párech, aby mohou otáčet bez výpadku služby. Když přepnete toohello druhý klíč v páru hello, můžete vytvořit nový pár klíčů. Nových klíčů účtu úložiště se vytváří vždy v párech, a zajistit, abyste měli vždy alespoň jeden nepoužívané úložiště klíčů tooswitch připravené k. klíče účtu úložiště tooshow hello, použijte hello následující kód:

    ```azurecli
    azure storage account keys list myStorageAccount \
    --resource-group myResourceGroup
    ```
3. Vytvořte sdílenou složku File storage hello.

    Hello sdílenou složku úložiště obsahuje hello sdílenou složku SMB. kvóta Hello je vždy vyjádřené v gigabajtech (GB). toocreate hello sdílenou složku úložiště, použijte hello následující kód:

    ```azurecli
    azure storage share create mystorageshare \
    --quota 10 \
    --account-name myStorageAccount \
    --account-key nPOgPR<--snip-->4Q==
    ```

4. Vytvořte adresář hello přípojného bodu.

    Je nutné vytvořit místní adresář v hello Linux souboru systému toomount hello k sdílená složka SMB. Nic zapsané nebo čtení z adresáře místní připojení hello se předají toohello sdílená složka SMB, který je hostován na úložiště File. toocreate hello adresář, použijte hello následující kód:

    ```bash
    sudo mkdir -p /mnt/mymountdirectory
    ```

5. Připojte hello sdílená složka SMB pomocí hello následující kód:

    ```azurecli
    sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountdirectory -o vers=3.0,username=myStorageAccount,password=myStorageAccountkey,dir_mode=0777,file_mode=0777
    ```

6. Zachovat hello SMB připojit prostřednictvím restartování počítače.

    Když restartujete hello virtuálního počítače s Linuxem, hello připojené sdílené složky SMB nepřipojené během vypnutí. tooremount hello složce SMB na spouštěcí, je nutné přidat řádku toohello Linux /etc/fstab. Linux používá hello fstab souboru toolist hello systémy souborů je nutné toomount během procesu spuštění hello. Přidání sdílené složky SMB hello zajišťuje, že aby hello sdílená úložiště je trvale připojeného souboru systém pro virtuální počítač s Linuxem hello. Přidání hello soubor úložiště SMB sdílenou složku tooa nového virtuálního počítače je možné, pokud používáte cloudové init.

    ```bash
    //myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    ```

## <a name="next-steps"></a>Další kroky

- [Během vytváření pomocí toocustomize init cloudu virtuálního počítače s Linuxem](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Přidat tooa disku virtuálního počítače s Linuxem](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Šifrování disky na virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure hello](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
