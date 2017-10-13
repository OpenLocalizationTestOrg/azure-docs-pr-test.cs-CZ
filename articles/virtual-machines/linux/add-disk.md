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
# <a name="add-a-disk-tooa-linux-vm"></a><span data-ttu-id="6ca9e-104">Přidat tooa disku virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="6ca9e-104">Add a disk tooa Linux VM</span></span>
<span data-ttu-id="6ca9e-105">Tento článek ukazuje, jak tooattach a trvalé disk tooyour virtuálních počítačů, což může zachovat data - i v případě, že virtuální počítač je znovu poskytnuto kvůli toomaintenance nebo změnou velikosti.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-105">This article shows how tooattach a persistent disk tooyour VM so that you can preserve your data - even if your VM is reprovisioned due toomaintenance or resizing.</span></span> 

## <a name="quick-commands"></a><span data-ttu-id="6ca9e-106">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="6ca9e-106">Quick Commands</span></span>
<span data-ttu-id="6ca9e-107">Následující příklad připojí Hello `50`toohello GB disk virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="6ca9e-107">hello following example attaches a `50`GB disk toohello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

<span data-ttu-id="6ca9e-108">toouse spravované disky:</span><span class="sxs-lookup"><span data-stu-id="6ca9e-108">toouse managed disks:</span></span>

```azurecli
az vm disk attach -g myResourceGroup --vm-name myVM --disk myDataDisk \
  --new --size-gb 50
```

<span data-ttu-id="6ca9e-109">toouse nespravované disky:</span><span class="sxs-lookup"><span data-stu-id="6ca9e-109">toouse unmanaged disks:</span></span>

```azurecli
az vm unmanaged-disk attach -g myResourceGroup -n myUnmanagedDisk --vm-name myVM \
  --new --size-gb 50
```

## <a name="attach-a-managed-disk"></a><span data-ttu-id="6ca9e-110">Připojte se spravovaným diskem</span><span class="sxs-lookup"><span data-stu-id="6ca9e-110">Attach a managed disk</span></span>

<span data-ttu-id="6ca9e-111">Pomocí spravovaného disků můžete toofocus na virtuální počítače a jejich disky bez starostí o účtech úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-111">Using managed disks enables you toofocus on your VMs and their disks without worrying about Azure Storage accounts.</span></span> <span data-ttu-id="6ca9e-112">Můžete rychle vytvořit a připojit se spravovaným diskem tooa virtuální počítač pomocí hello stejnou skupinu prostředků Azure, nebo můžete vytvořit libovolný počet disků a připojte je.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-112">You can quickly create and attach a managed disk tooa VM using hello same Azure resource group, or you can create any number of disks and then attach them.</span></span>


### <a name="attach-a-new-disk-tooa-vm"></a><span data-ttu-id="6ca9e-113">Připojit nový disk tooa virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="6ca9e-113">Attach a new disk tooa VM</span></span>

<span data-ttu-id="6ca9e-114">Pokud potřebujete pouze nový disk na vašem virtuálním počítači, můžete použít hello `az vm disk attach` příkaz.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-114">If you just need a new disk on your VM, you can use hello `az vm disk attach` command.</span></span>

```azurecli
az vm disk attach -g myResourceGroup --vm-name myVM --disk myDataDisk \
  --new --size-gb 50
```

### <a name="attach-an-existing-disk"></a><span data-ttu-id="6ca9e-115">Připojení stávajícího disku</span><span class="sxs-lookup"><span data-stu-id="6ca9e-115">Attach an existing disk</span></span> 

<span data-ttu-id="6ca9e-116">V mnoha případech připojit disky, které již byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-116">In many cases you attach disks that have already been created.</span></span> <span data-ttu-id="6ca9e-117">Nejprve najít id hello disku a pak předejte tuto toohello `az vm disk attach` příkaz.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-117">You first find hello disk id and then pass that toohello `az vm disk attach` command.</span></span> <span data-ttu-id="6ca9e-118">Hello následující příklad používá disk vytvořit s `az disk create -g myResourceGroup -n myDataDisk --size-gb 50`.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-118">hello following example uses a disk created with `az disk create -g myResourceGroup -n myDataDisk --size-gb 50`.</span></span>

```azurecli
# find hello disk id
diskId=$(az disk show -g myResourceGroup -n myDataDisk --query 'id' -o tsv)
az vm disk attach -g myResourceGroup --vm-name myVM --disk $diskId
```

<span data-ttu-id="6ca9e-119">Hello výstup vypadá podobně jako následující hello (můžete použít hello `-o table` možnost výstup hello tooformat tooany příkazu v):</span><span class="sxs-lookup"><span data-stu-id="6ca9e-119">hello output looks something like hello following (you can use hello `-o table` option tooany command tooformat hello output in ):</span></span>

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


## <a name="attach-an-unmanaged-disk"></a><span data-ttu-id="6ca9e-120">Připojte nespravované disk</span><span class="sxs-lookup"><span data-stu-id="6ca9e-120">Attach an unmanaged disk</span></span>

<span data-ttu-id="6ca9e-121">Nový disk se připojuje je rychlý, pokud není paměti vytváření disku v hello stejný účet úložiště jako virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-121">Attaching a new disk is quick if you do not mind creating a disk in hello same storage account as your VM.</span></span> <span data-ttu-id="6ca9e-122">Typ `azure vm disk attach-new` toocreate a připojit nový disk GB pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-122">Type `azure vm disk attach-new` toocreate and attach a new GB disk for your VM.</span></span> <span data-ttu-id="6ca9e-123">Pokud účet úložiště není explicitně identifikovat, každý disk vytvoříte je umístěn v hello stejný účet úložiště, které se nachází disk s operačním systémem.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-123">If you do not explicitly identify a storage account, any disk you create is placed in hello same storage account where your OS disk resides.</span></span> <span data-ttu-id="6ca9e-124">Následující příklad připojí Hello `50`toohello GB disk virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="6ca9e-124">hello following example attaches a `50`GB disk toohello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
az vm unmanaged-disk attach -g myResourceGroup -n myUnmanagedDisk --vm-name myVM \
  --new --size-gb 50
```

## <a name="connect-toohello-linux-vm-toomount-hello-new-disk"></a><span data-ttu-id="6ca9e-125">Připojit toohello virtuálního počítače s Linuxem toomount hello nový disk</span><span class="sxs-lookup"><span data-stu-id="6ca9e-125">Connect toohello Linux VM toomount hello new disk</span></span>
> [!NOTE]
> <span data-ttu-id="6ca9e-126">Toto téma připojí tooa virtuálních počítačů pomocí uživatelských jmen a hesel.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-126">This topic connects tooa VM using usernames and passwords.</span></span> <span data-ttu-id="6ca9e-127">veřejné a soukromé klíče dvojice toocommunicate toouse s virtuálního počítače, najdete v části [jak tooUse SSH s Linuxem v Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6ca9e-127">toouse public and private key pairs toocommunicate with your VM, see [How tooUse SSH with Linux on Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 
> 
> 

<span data-ttu-id="6ca9e-128">Je třeba tooSSH do vaší toopartition virtuálního počítače Azure, formátování a připojit nový disk, virtuálním počítačům s Linuxem můžete ji použít.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-128">You need tooSSH into your Azure VM toopartition, format, and mount your new disk so your Linux VM can use it.</span></span> <span data-ttu-id="6ca9e-129">Pokud si nejste obeznámeni s připojením s **ssh**, hello příkaz má podobu hello `ssh <username>@<FQDNofAzureVM> -p <hello ssh port>`a vypadá hello následující:</span><span class="sxs-lookup"><span data-stu-id="6ca9e-129">If you're not familiar with connecting with **ssh**, hello command takes hello form `ssh <username>@<FQDNofAzureVM> -p <hello ssh port>`, and looks like hello following:</span></span>

```bash
ssh ops@mypublicdns.westus.cloudapp.azure.com -p 22
```

<span data-ttu-id="6ca9e-130">Výstup</span><span class="sxs-lookup"><span data-stu-id="6ca9e-130">Output</span></span>

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

<span data-ttu-id="6ca9e-131">Teď, když jste připojení tooyour virtuálních počítačů, jste připraveni tooattach disk.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-131">Now that you're connected tooyour VM, you're ready tooattach a disk.</span></span>  <span data-ttu-id="6ca9e-132">Nejprve najde hello disk, s využitím `dmesg | grep SCSI` (metoda hello používáte toodiscover nový disk se mohou lišit).</span><span class="sxs-lookup"><span data-stu-id="6ca9e-132">First find hello disk, using `dmesg | grep SCSI` (hello method you use toodiscover your new disk may vary).</span></span> <span data-ttu-id="6ca9e-133">V takovém případě ji vypadá podobně jako:</span><span class="sxs-lookup"><span data-stu-id="6ca9e-133">In this case, it looks something like:</span></span>

```bash
dmesg | grep SCSI
```

<span data-ttu-id="6ca9e-134">Výstup</span><span class="sxs-lookup"><span data-stu-id="6ca9e-134">Output</span></span>

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

<span data-ttu-id="6ca9e-135">a v případě hello tohoto tématu hello `sdc` disk je hello ten, který má být.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-135">and in hello case of this topic, hello `sdc` disk is hello one that we want.</span></span> <span data-ttu-id="6ca9e-136">Nyní oddílu hello disk s `sudo fdisk /dev/sdc` – za předpokladu, že ve vaší případu hello byl disk `sdc`a zkontrolujte primární disku v oddílu 1 a přijmout hello ostatní výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-136">Now partition hello disk with `sudo fdisk /dev/sdc` -- assuming that in your case hello disk was `sdc`, and make it a primary disk on partition 1, and accept hello other defaults.</span></span>

```bash
sudo fdisk /dev/sdc
```

<span data-ttu-id="6ca9e-137">Výstup</span><span class="sxs-lookup"><span data-stu-id="6ca9e-137">Output</span></span>

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

<span data-ttu-id="6ca9e-138">Vytvořit oddíl hello zadáním `p` hello příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="6ca9e-138">Create hello partition by typing `p` at hello prompt:</span></span>

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

<span data-ttu-id="6ca9e-139">A zapsat systému souborů toohello oddílu pomocí hello **mkfs** příkaz, název zařízení typu a hello systému souborů.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-139">And write a file system toohello partition by using hello **mkfs** command, specifying your filesystem type and hello device name.</span></span> <span data-ttu-id="6ca9e-140">V tomto tématu, že používáme `ext4` a `/dev/sdc1` z výše:</span><span class="sxs-lookup"><span data-stu-id="6ca9e-140">In this topic, we're using `ext4` and `/dev/sdc1` from above:</span></span>

```bash
sudo mkfs -t ext4 /dev/sdc1
```

<span data-ttu-id="6ca9e-141">Výstup</span><span class="sxs-lookup"><span data-stu-id="6ca9e-141">Output</span></span>

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

<span data-ttu-id="6ca9e-142">Teď vytvoříme directory toomount hello souboru systému pomocí `mkdir`:</span><span class="sxs-lookup"><span data-stu-id="6ca9e-142">Now we create a directory toomount hello file system using `mkdir`:</span></span>

```bash
sudo mkdir /datadrive
```

<span data-ttu-id="6ca9e-143">A připojte pomocí directory hello `mount`:</span><span class="sxs-lookup"><span data-stu-id="6ca9e-143">And you mount hello directory using `mount`:</span></span>

```bash
sudo mount /dev/sdc1 /datadrive
```

<span data-ttu-id="6ca9e-144">Hello datový disk je nyní připraven toouse jako `/datadrive`.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-144">hello data disk is now ready toouse as `/datadrive`.</span></span>

```bash
ls
```

<span data-ttu-id="6ca9e-145">Výstup</span><span class="sxs-lookup"><span data-stu-id="6ca9e-145">Output</span></span>

```bash
bin   datadrive  etc   initrd.img  lib64       media  opt   root  sbin  sys  usr  vmlinuz
boot  dev        home  lib         lost+found  mnt    proc  run   srv   tmp  var
```

<span data-ttu-id="6ca9e-146">znovu tooensure hello jednotky je připojeny automaticky po restartování počítače, které musí být přidán toohello /etc/fstab souboru.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-146">tooensure hello drive is remounted automatically after a reboot it must be added toohello /etc/fstab file.</span></span> <span data-ttu-id="6ca9e-147">Kromě toho důrazně doporučujeme, aby hello UUID (univerzálně jedinečný identifikátor) se používá v etc/fstab toorefer toohello jednotka místo názvu zařízení právě hello (jako je třeba `/dev/sdc1`).</span><span class="sxs-lookup"><span data-stu-id="6ca9e-147">In addition, it is highly recommended that hello UUID (Universally Unique IDentifier) is used in /etc/fstab toorefer toohello drive rather than just hello device name (such as, `/dev/sdc1`).</span></span> <span data-ttu-id="6ca9e-148">Pokud hello OS zjistí chyba disku během spouštění, pomocí hello UUID zabraňuje hello nesprávnou disku se připojené tooa zadané umístění.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-148">If hello OS detects a disk error during boot, using hello UUID avoids hello incorrect disk being mounted tooa given location.</span></span> <span data-ttu-id="6ca9e-149">Zbývající datových disků by pak přiřadit tyto stejné ID zařízení.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-149">Remaining data disks would then be assigned those same device IDs.</span></span> <span data-ttu-id="6ca9e-150">toofind hello UUID hello nový disk, použijte hello **blkid** nástroj:</span><span class="sxs-lookup"><span data-stu-id="6ca9e-150">toofind hello UUID of hello new drive, use hello **blkid** utility:</span></span>

```bash
sudo -i blkid
```

<span data-ttu-id="6ca9e-151">výstup Hello vypadá podobně jako toohello následující:</span><span class="sxs-lookup"><span data-stu-id="6ca9e-151">hello output looks similar toohello following:</span></span>

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

> [!NOTE]
> <span data-ttu-id="6ca9e-152">Nesprávně úpravy hello **/etc/fstab** soubor může mít za následek nelze spustit systém.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-152">Improperly editing hello **/etc/fstab** file could result in an unbootable system.</span></span> <span data-ttu-id="6ca9e-153">Pokud si jisti, naleznete v dokumentaci toohello distribuční informace o tom, jak upravit tooproperly tento soubor.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-153">If unsure, refer toohello distribution's documentation for information on how tooproperly edit this file.</span></span> <span data-ttu-id="6ca9e-154">Dále je doporučeno, jestli je vytvořená záloha souboru /etc/fstab hello před úpravou.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-154">It is also recommended that a backup of hello /etc/fstab file is created before editing.</span></span>
> 
> 

<span data-ttu-id="6ca9e-155">Dále otevřete hello **/etc/fstab** soubor v textovém editoru:</span><span class="sxs-lookup"><span data-stu-id="6ca9e-155">Next, open hello **/etc/fstab** file in a text editor:</span></span>

```bash
sudo vi /etc/fstab
```

<span data-ttu-id="6ca9e-156">V tomto příkladu používáme hello UUID hodnotu pro hello nové **/dev/sdc1** zařízení, který byl vytvořen v předchozích krocích hello a přípojný bod hello **/datadrive**.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-156">In this example, we use hello UUID value for hello new **/dev/sdc1** device that was created in hello previous steps, and hello mountpoint **/datadrive**.</span></span> <span data-ttu-id="6ca9e-157">Přidejte následující řádek toohello konec hello hello **/etc/fstab** souboru:</span><span class="sxs-lookup"><span data-stu-id="6ca9e-157">Add hello following line toohello end of hello **/etc/fstab** file:</span></span>

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

> [!NOTE]
> <span data-ttu-id="6ca9e-158">Později odebrat datový disk bez úprav fstab by mohlo způsobit tooboot toofail hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-158">Later removing a data disk without editing fstab could cause hello VM toofail tooboot.</span></span> <span data-ttu-id="6ca9e-159">Většina distribuce zadejte buď hello `nofail` nebo `nobootwait` fstab možnosti.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-159">Most distributions provide either hello `nofail` and/or `nobootwait` fstab options.</span></span> <span data-ttu-id="6ca9e-160">Tyto možnosti Povolit tooboot systému i v případě selhání disku hello toomount při spuštění.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-160">These options allow a system tooboot even if hello disk fails toomount at boot time.</span></span> <span data-ttu-id="6ca9e-161">Další informace o těchto parametrů naleznete v dokumentaci vaší distribuce.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-161">Consult your distribution's documentation for more information on these parameters.</span></span>
> 
> <span data-ttu-id="6ca9e-162">Hello **nofail** možnost zajistí, že hello virtuální počítač spustí i v případě systému souborů hello je poškozený nebo hello disk neexistuje při spuštění.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-162">hello **nofail** option ensures that hello VM starts even if hello filesystem is corrupt or hello disk does not exist at boot time.</span></span> <span data-ttu-id="6ca9e-163">Bez této možnosti se můžete setkat chování jak je popsáno v [nelze SSH tooLinux virtuálních počítačů z důvodu chyby tooFSTAB](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/)</span><span class="sxs-lookup"><span data-stu-id="6ca9e-163">Without this option, you may encounter behavior as described in [Cannot SSH tooLinux VM due tooFSTAB errors](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/)</span></span>

### <a name="trimunmap-support-for-linux-in-azure"></a><span data-ttu-id="6ca9e-164">Podpora uvolnění dočasné paměti nebo UNMAP pro Linux v Azure</span><span class="sxs-lookup"><span data-stu-id="6ca9e-164">TRIM/UNMAP support for Linux in Azure</span></span>
<span data-ttu-id="6ca9e-165">Některé jádra Linux podporují toodiscard operace TRIM/UNMAP nepoužívané bloky na disku hello.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-165">Some Linux kernels support TRIM/UNMAP operations toodiscard unused blocks on hello disk.</span></span> <span data-ttu-id="6ca9e-166">To je užitečné hlavně v tooinform standardní úložiště Azure, které odstraněné stránky již nejsou platné a může být vymazány.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-166">This is primarily useful in standard storage tooinform Azure that deleted pages are no longer valid and can be discarded.</span></span> <span data-ttu-id="6ca9e-167">Pokud chcete vytvořit velkých souborů a pak odstraňte je to můžete uložit náklady.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-167">This can save cost if you create large files and then delete them.</span></span>

<span data-ttu-id="6ca9e-168">Existují dva způsoby tooenable TRIM podporují ve virtuálním počítačům s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-168">There are two ways tooenable TRIM support in your Linux VM.</span></span> <span data-ttu-id="6ca9e-169">Obvyklým způsobem podívejte se distribuční hello doporučenému přístupu:</span><span class="sxs-lookup"><span data-stu-id="6ca9e-169">As usual, consult your distribution for hello recommended approach:</span></span>

* <span data-ttu-id="6ca9e-170">Použití hello `discard` připojit možnost v `/etc/fstab`, například:</span><span class="sxs-lookup"><span data-stu-id="6ca9e-170">Use hello `discard` mount option in `/etc/fstab`, for example:</span></span>

    ```bash
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2
    ```
* <span data-ttu-id="6ca9e-171">V některých případech hello `discard` možnost může mít vliv na výkon.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-171">In some cases hello `discard` option may have performance implications.</span></span> <span data-ttu-id="6ca9e-172">Alternativně můžete spustit hello `fstrim` ručně příkaz z příkazového řádku hello, nebo ho přidat tooyour crontab toorun pravidelně:</span><span class="sxs-lookup"><span data-stu-id="6ca9e-172">Alternatively, you can run hello `fstrim` command manually from hello command line, or add it tooyour crontab toorun regularly:</span></span>
  
    <span data-ttu-id="6ca9e-173">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="6ca9e-173">**Ubuntu**</span></span>
  
    ```bash
    sudo apt-get install util-linux
    sudo fstrim /datadrive
    ```
  
    <span data-ttu-id="6ca9e-174">**RHEL nebo CentOS**</span><span class="sxs-lookup"><span data-stu-id="6ca9e-174">**RHEL/CentOS**</span></span>

    ```bash
    sudo yum install util-linux
    sudo fstrim /datadrive
    ```

## <a name="troubleshooting"></a><span data-ttu-id="6ca9e-175">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="6ca9e-175">Troubleshooting</span></span>
[!INCLUDE [virtual-machines-linux-lunzero](../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a><span data-ttu-id="6ca9e-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6ca9e-176">Next Steps</span></span>
* <span data-ttu-id="6ca9e-177">Mějte na paměti, že nový disk není k dispozici toohello virtuálních počítačů Pokud dojde k restartování Pokud zápisu této informace tooyour [fstab](http://en.wikipedia.org/wiki/Fstab) souboru.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-177">Remember, that your new disk is not available toohello VM if it reboots unless you write that information tooyour [fstab](http://en.wikipedia.org/wiki/Fstab) file.</span></span>
* <span data-ttu-id="6ca9e-178">tooensure virtuálním počítačům s Linuxem je správně nakonfigurována, zkontrolujte hello [optimalizovat výkon počítače Linux](optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) doporučení.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-178">tooensure your Linux VM is configured correctly, review hello [Optimize your Linux machine performance](optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) recommendations.</span></span>
* <span data-ttu-id="6ca9e-179">Rozbalte vaše kapacita úložiště přidáním dalších disků a [konfigurace RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) další výkonu.</span><span class="sxs-lookup"><span data-stu-id="6ca9e-179">Expand your storage capacity by adding additional disks and [configure RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for additional performance.</span></span>
