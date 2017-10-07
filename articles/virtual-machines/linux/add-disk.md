---
title: "hello aaaAdd disku tooLinux virtuální počítač pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Přečtěte si tooadd tooyour trvalé disku virtuálního počítače s Linuxem pomocí hello Azure CLI 1.0 a 2.0."
keywords: "virtuální počítač Linux, přidejte disk prostředků"
services: virtual-machines-linux
documentationcenter: 
author: rickstercdn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 3005a066-7a84-4dc5-bdaa-574c75e6e411
ms.service: virtual-machines-linux
ms.topic: article
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.date: 02/02/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0dc5236be62d96b70dd47a7f621f626a037e22aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-disk-tooa-linux-vm"></a>Přidat tooa disku virtuálního počítače s Linuxem
Tento článek ukazuje, jak tooattach a trvalé disk tooyour virtuálních počítačů, což může zachovat data - i v případě, že virtuální počítač je znovu poskytnuto kvůli toomaintenance nebo změnou velikosti. 

## <a name="quick-commands"></a>Rychlé příkazy
Následující příklad připojí Hello `50`toohello GB disk virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`:

toouse spravované disky:

```azurecli
az vm disk attach -g myResourceGroup --vm-name myVM --disk myDataDisk \
  --new --size-gb 50
```

toouse nespravované disky:

```azurecli
az vm unmanaged-disk attach -g myResourceGroup -n myUnmanagedDisk --vm-name myVM \
  --new --size-gb 50
```

## <a name="attach-a-managed-disk"></a>Připojte se spravovaným diskem

Pomocí spravovaného disků můžete toofocus na virtuální počítače a jejich disky bez starostí o účtech úložiště Azure. Můžete rychle vytvořit a připojit se spravovaným diskem tooa virtuální počítač pomocí hello stejnou skupinu prostředků Azure, nebo můžete vytvořit libovolný počet disků a připojte je.


### <a name="attach-a-new-disk-tooa-vm"></a>Připojit nový disk tooa virtuálních počítačů

Pokud potřebujete pouze nový disk na vašem virtuálním počítači, můžete použít hello `az vm disk attach` příkaz.

```azurecli
az vm disk attach -g myResourceGroup --vm-name myVM --disk myDataDisk \
  --new --size-gb 50
```

### <a name="attach-an-existing-disk"></a>Připojení stávajícího disku 

V mnoha případech připojit disky, které již byly vytvořeny. Nejprve najít id hello disku a pak předejte tuto toohello `az vm disk attach` příkaz. Hello následující příklad používá disk vytvořit s `az disk create -g myResourceGroup -n myDataDisk --size-gb 50`.

```azurecli
# find hello disk id
diskId=$(az disk show -g myResourceGroup -n myDataDisk --query 'id' -o tsv)
az vm disk attach -g myResourceGroup --vm-name myVM --disk $diskId
```

Hello výstup vypadá podobně jako následující hello (můžete použít hello `-o table` možnost výstup hello tooformat tooany příkazu v):

```json
{
  "accountType": "Standard_LRS",
  "creationData": {
    "createOption": "Empty",
    "imageReference": null,
    "sourceResourceId": null,
    "sourceUri": null,
    "storageAccountId": null
  },
  "diskSizeGb": 50,
  "encryptionSettings": null,
  "id": "/subscriptions/<guid>/resourceGroups/rasquill-script/providers/Microsoft.Compute/disks/myDataDisk",
  "location": "westus",
  "name": "myDataDisk",
  "osType": null,
  "ownerId": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "tags": null,
  "timeCreated": "2017-02-02T23:35:47.708082+00:00",
  "type": "Microsoft.Compute/disks"
}
```


## <a name="attach-an-unmanaged-disk"></a>Připojte nespravované disk

Nový disk se připojuje je rychlý, pokud není paměti vytváření disku v hello stejný účet úložiště jako virtuální počítač. Typ `azure vm disk attach-new` toocreate a připojit nový disk GB pro virtuální počítač. Pokud účet úložiště není explicitně identifikovat, každý disk vytvoříte je umístěn v hello stejný účet úložiště, které se nachází disk s operačním systémem. Následující příklad připojí Hello `50`toohello GB disk virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`:

```azurecli
az vm unmanaged-disk attach -g myResourceGroup -n myUnmanagedDisk --vm-name myVM \
  --new --size-gb 50
```

## <a name="connect-toohello-linux-vm-toomount-hello-new-disk"></a>Připojit toohello virtuálního počítače s Linuxem toomount hello nový disk
> [!NOTE]
> Toto téma připojí tooa virtuálních počítačů pomocí uživatelských jmen a hesel. veřejné a soukromé klíče dvojice toocommunicate toouse s virtuálního počítače, najdete v části [jak tooUse SSH s Linuxem v Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 
> 
> 

Je třeba tooSSH do vaší toopartition virtuálního počítače Azure, formátování a připojit nový disk, virtuálním počítačům s Linuxem můžete ji použít. Pokud si nejste obeznámeni s připojením s **ssh**, hello příkaz má podobu hello `ssh <username>@<FQDNofAzureVM> -p <hello ssh port>`a vypadá hello následující:

```bash
ssh ops@mypublicdns.westus.cloudapp.azure.com -p 22
```

Výstup

```bash
hello authenticity of host 'mypublicdns.westus.cloudapp.azure.com (191.239.51.1)' can't be established.
ECDSA key fingerprint is bx:xx:xx:xx:xx:xx:xx:xx:xx:x:x:x:x:x:x:xx.
Are you sure you want toocontinue connecting (yes/no)? yes
Warning: Permanently added 'mypublicdns.westus.cloudapp.azure.com,191.239.51.1' (ECDSA) toohello list of known hosts.
ops@mypublicdns.westus.cloudapp.azure.com's password:
Welcome tooUbuntu 14.04.2 LTS (GNU/Linux 3.16.0-37-generic x86_64)

* Documentation:  https://help.ubuntu.com/

System information as of Fri May 22 21:02:32 UTC 2015

System load: 0.37              Memory usage: 2%   Processes:       207
Usage of /:  41.4% of 1.94GB   Swap usage:   0%   Users logged in: 0

Graph this data and manage this system at:
  https://landscape.canonical.com/

Get cloud support with Ubuntu Advantage Cloud Guest:
  http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

hello programs included with hello Ubuntu system are free software;
hello exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, toohello extent permitted by
applicable law.

ops@myVM:~$
```

Teď, když jste připojení tooyour virtuálních počítačů, jste připraveni tooattach disk.  Nejprve najde hello disk, s využitím `dmesg | grep SCSI` (metoda hello používáte toodiscover nový disk se mohou lišit). V takovém případě ji vypadá podobně jako:

```bash
dmesg | grep SCSI
```

Výstup

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

a v případě hello tohoto tématu hello `sdc` disk je hello ten, který má být. Nyní oddílu hello disk s `sudo fdisk /dev/sdc` – za předpokladu, že ve vaší případu hello byl disk `sdc`a zkontrolujte primární disku v oddílu 1 a přijmout hello ostatní výchozí hodnoty.

```bash
sudo fdisk /dev/sdc
```

Výstup

```bash
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0x2a59b123.
Changes will remain in memory only, until you decide toowrite them.
After that, of course, hello previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-10485759, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
Using default value 10485759
```

Vytvořit oddíl hello zadáním `p` hello příkazového řádku:

```bash
Command (m for help): p

Disk /dev/sdc: 5368 MB, 5368709120 bytes
255 heads, 63 sectors/track, 652 cylinders, total 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x2a59b123

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1            2048    10485759     5241856   83  Linux

Command (m for help): w
hello partition table has been altered!

Calling ioctl() toore-read partition table.
Syncing disks.
```

A zapsat systému souborů toohello oddílu pomocí hello **mkfs** příkaz, název zařízení typu a hello systému souborů. V tomto tématu, že používáme `ext4` a `/dev/sdc1` z výše:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Výstup

```bash
mke2fs 1.42.9 (4-Feb-2014)
Discarding device blocks: done
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
327680 inodes, 1310464 blocks
65523 blocks (5.00%) reserved for hello super user
First data block=0
Maximum filesystem blocks=1342177280
40 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
    32768, 98304, 163840, 229376, 294912, 819200, 884736
Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```

Teď vytvoříme directory toomount hello souboru systému pomocí `mkdir`:

```bash
sudo mkdir /datadrive
```

A připojte pomocí directory hello `mount`:

```bash
sudo mount /dev/sdc1 /datadrive
```

Hello datový disk je nyní připraven toouse jako `/datadrive`.

```bash
ls
```

Výstup

```bash
bin   datadrive  etc   initrd.img  lib64       media  opt   root  sbin  sys  usr  vmlinuz
boot  dev        home  lib         lost+found  mnt    proc  run   srv   tmp  var
```

znovu tooensure hello jednotky je připojeny automaticky po restartování počítače, které musí být přidán toohello /etc/fstab souboru. Kromě toho důrazně doporučujeme, aby hello UUID (univerzálně jedinečný identifikátor) se používá v etc/fstab toorefer toohello jednotka místo názvu zařízení právě hello (jako je třeba `/dev/sdc1`). Pokud hello OS zjistí chyba disku během spouštění, pomocí hello UUID zabraňuje hello nesprávnou disku se připojené tooa zadané umístění. Zbývající datových disků by pak přiřadit tyto stejné ID zařízení. toofind hello UUID hello nový disk, použijte hello **blkid** nástroj:

```bash
sudo -i blkid
```

výstup Hello vypadá podobně jako toohello následující:

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

> [!NOTE]
> Nesprávně úpravy hello **/etc/fstab** soubor může mít za následek nelze spustit systém. Pokud si jisti, naleznete v dokumentaci toohello distribuční informace o tom, jak upravit tooproperly tento soubor. Dále je doporučeno, jestli je vytvořená záloha souboru /etc/fstab hello před úpravou.
> 
> 

Dále otevřete hello **/etc/fstab** soubor v textovém editoru:

```bash
sudo vi /etc/fstab
```

V tomto příkladu používáme hello UUID hodnotu pro hello nové **/dev/sdc1** zařízení, který byl vytvořen v předchozích krocích hello a přípojný bod hello **/datadrive**. Přidejte následující řádek toohello konec hello hello **/etc/fstab** souboru:

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

> [!NOTE]
> Později odebrat datový disk bez úprav fstab by mohlo způsobit tooboot toofail hello virtuálních počítačů. Většina distribuce zadejte buď hello `nofail` nebo `nobootwait` fstab možnosti. Tyto možnosti Povolit tooboot systému i v případě selhání disku hello toomount při spuštění. Další informace o těchto parametrů naleznete v dokumentaci vaší distribuce.
> 
> Hello **nofail** možnost zajistí, že hello virtuální počítač spustí i v případě systému souborů hello je poškozený nebo hello disk neexistuje při spuštění. Bez této možnosti se můžete setkat chování jak je popsáno v [nelze SSH tooLinux virtuálních počítačů z důvodu chyby tooFSTAB](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/)

### <a name="trimunmap-support-for-linux-in-azure"></a>Podpora uvolnění dočasné paměti nebo UNMAP pro Linux v Azure
Některé jádra Linux podporují toodiscard operace TRIM/UNMAP nepoužívané bloky na disku hello. To je užitečné hlavně v tooinform standardní úložiště Azure, které odstraněné stránky již nejsou platné a může být vymazány. Pokud chcete vytvořit velkých souborů a pak odstraňte je to můžete uložit náklady.

Existují dva způsoby tooenable TRIM podporují ve virtuálním počítačům s Linuxem. Obvyklým způsobem podívejte se distribuční hello doporučenému přístupu:

* Použití hello `discard` připojit možnost v `/etc/fstab`, například:

    ```bash
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2
    ```
* V některých případech hello `discard` možnost může mít vliv na výkon. Alternativně můžete spustit hello `fstrim` ručně příkaz z příkazového řádku hello, nebo ho přidat tooyour crontab toorun pravidelně:
  
    **Ubuntu**
  
    ```bash
    sudo apt-get install util-linux
    sudo fstrim /datadrive
    ```
  
    **RHEL nebo CentOS**

    ```bash
    sudo yum install util-linux
    sudo fstrim /datadrive
    ```

## <a name="troubleshooting"></a>Řešení potíží
[!INCLUDE [virtual-machines-linux-lunzero](../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Další kroky
* Mějte na paměti, že nový disk není k dispozici toohello virtuálních počítačů Pokud dojde k restartování Pokud zápisu této informace tooyour [fstab](http://en.wikipedia.org/wiki/Fstab) souboru.
* tooensure virtuálním počítačům s Linuxem je správně nakonfigurována, zkontrolujte hello [optimalizovat výkon počítače Linux](optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) doporučení.
* Rozbalte vaše kapacita úložiště přidáním dalších disků a [konfigurace RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) další výkonu.

