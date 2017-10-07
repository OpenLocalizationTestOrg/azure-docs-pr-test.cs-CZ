---
title: "aaaMount Azure File storage ve virtuální počítače s Linuxem pomocí protokolu SMB | Microsoft Docs"
description: "Jak toomount Azure File storage ve virtuální počítače s Linuxem pomocí protokolu SMB s hello 2.0 rozhraní příkazového řádku Azure"
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
ms.date: 02/13/2017
ms.author: v-livech
ms.openlocfilehash: 7b34c81e602748b78c924f44a54b876744c28f56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="mount-azure-file-storage-on-linux-vms-using-smb"></a>Připojení Azure File storage na virtuální počítače s Linuxem pomocí protokolu SMB

Tento článek ukazuje, jak připojit tooutilize hello službu úložiště Azure File na virtuální počítač s Linuxem pomocí SMB s hello 2.0 rozhraní příkazového řádku Azure. Azure File storage nabízí sdílené složky v cloudu hello pomocí standardního protokol SMB hello. Můžete také provést tyto kroky hello [Azure CLI 1.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Hello požadavky jsou:

- [Účet Azure](https://azure.microsoft.com/pricing/free-trial/)
- [Soubory veřejného a privátního klíče SSH](mac-create-ssh-keys.md)

## <a name="quick-commands"></a>Rychlé příkazy

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
toodo Ano, přidejte následující řádek toohello hello `/etc/fstab`:

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## <a name="detailed-walkthrough"></a>Podrobný postup

File storage nabízí sdílené složky v cloudu hello, které používají standardní protokol SMB hello. Hello nejnovější verze služby úložiště File můžete také připojit sdílenou složku z jakékoli operační systém, který podporuje protokol SMB 3.0. Pokud používáte připojení protokolu SMB v systému Linux, získáte snadno zálohy tooa robustní, trvalé archivováním umístění úložiště podporovaný SLA.

Přesunutí souborů z připojení SMB tooan virtuálního počítače, který je hostován na soubor úložiště je že skvělým způsobem toodebug protokoly. Je to způsobeno hello sdílet stejný protokol SMB může být připojen místně tooyour Mac, Linux nebo Windows pracovní stanice. SMB není hello nejlepší řešení pro streamování Linux nebo aplikace protokolů v reálném čase, protože není hello protokolu SMB vytvořené toohandle těchto funkcí velkou protokolování. Nástroje protokolování vyhrazené, jednotná vrstvy, jako je Fluentd bude vhodnější než SMB pro shromažďování Linux a aplikace protokolování výstupu.

Pro tento podrobný návod vytvoříme hello požadavky potřeby toofirst vytvořit hello sdílenou složku úložiště a následné připojení prostřednictvím protokolu SMB na virtuální počítač s Linuxem.

1. Vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create) toohold hello sdílené složky.

    skupinu prostředků s názvem toocreate `myResourceGroup` v umístění "Západní USA" hello, použijte následující ukázka hello:

    ```azurecli
    az group create --name myResourceGroup --location westus
    ```

2. Vytvoření účtu úložiště Azure s [vytvořit účet úložiště az](/cli/azure/storage/account#create) toostore hello skutečné soubory.

    toocreate účet úložiště s názvem můj_účet_úložiště pomocí hello Standard_LRS úložiště SKU, použijte hello následující ukázka:

    ```azurecli
    az storage account create --resource-group myResourceGroup \
        --name mystorageaccount \
        --location westus \
        --sku Standard_LRS
    ```

3. Zobrazit hello klíče účtu úložiště.

    Když vytvoříte účet úložiště, klíče účtu hello jsou vytvořeny v párech, aby mohou otáčet bez výpadku služby. Když přepnete toohello druhý klíč v páru hello, můžete vytvořit nový pár klíčů. Nových klíčů účtu úložiště se vytváří vždy v párech, a zajistit, abyste měli vždy alespoň jeden nepoužívané úložiště účet klíče připravené tooswitch k.

    Zobrazit hello klíče účtu úložiště s hello [seznam klíčů účtu úložiště az](/cli/azure/storage/account/keys#list). Hello klíče účtu úložiště pro hello s názvem `mystorageaccount` jsou uvedeny v následující ukázka hello:

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --account-name mystorageaccount
    ```

    tooextract jeden klíč, použijte hello `--query` příznak. Hello následující příklad extrahuje první klíč hello (`[0]`):

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --account-name mystorageaccount \
        --query '[0].{Key:value}' --output tsv
    ```

4. Vytvořte sdílenou složku File storage hello.

    Hello sdílenou složku úložiště obsahuje hello se sdílená složka SMB [vytvořit sdílenou složku úložiště az](/cli/azure/storage/share#create). kvóta Hello je vždy vyjádřené v gigabajtech (GB). Jeden z klíčů hello předat z předchozí hello `az storage account keys list` příkaz. Vytvořte sdílenou složku s názvem mystorageshare s kvótou 10 GB s použitím hello následující ukázka:

    ```azurecli
    az storage share create --name mystorageshare \
        --quota 10 \
        --account-name mystorageaccount \
        --account-key nPOgPR<--snip-->4Q==
    ```

5. Vytvořte adresář přípojného bodu.

    Vytvořte místní adresář v hello Linux souboru systému toomount hello k sdílená složka SMB. Nic zapsané nebo čtení z adresáře místní připojení hello se předají toohello sdílená složka SMB, který je hostován na úložiště File. toocreate do místního adresáře v/mnt/mymountdirectory, použijte hello následující ukázka:

    ```bash
    sudo mkdir -p /mnt/mymountdirectory
    ```

6. Připojte hello SMB sdílenou složku toohello místního adresáře.

    Zadejte vlastní uživatelské jméno účtu úložiště a klíč účtu úložiště pro přihlašovací údaje pro připojení k hello následujícím způsobem:

    ```azurecli
    sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountdirectory -o vers=3.0,username=mystorageaccount,password=mystorageaccountkey,dir_mode=0777,file_mode=0777
    ```

7. Zachovat hello SMB připojit prostřednictvím restartování počítače.

    Když restartujete hello virtuálního počítače s Linuxem, hello připojené sdílené složky SMB nepřipojené během vypnutí. hello tooremount sdílená složka SMB na spuštění, přidejte řádek toohello Linux /etc/fstab. Linux používá hello fstab souboru toolist hello systémy souborů je nutné toomount během procesu spuštění hello. Přidání sdílené složky SMB hello zajišťuje, že aby hello sdílená úložiště je trvale připojeného souboru systém pro virtuální počítač s Linuxem hello. Přidání hello soubor úložiště SMB sdílenou složku tooa nového virtuálního počítače je možné, pokud používáte cloudové init.

    ```bash
    //myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    ```

## <a name="next-steps"></a>Další kroky

- [Během vytváření pomocí toocustomize init cloudu virtuálního počítače s Linuxem](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Přidat tooa disku virtuálního počítače s Linuxem](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Šifrování disky na virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure hello](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
