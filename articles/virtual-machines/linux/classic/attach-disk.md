---
title: "aaaAttach tooa disku virtuálního počítače s Linuxem v Azure | Microsoft Docs"
description: "Zjistěte, jak tooattach datový disk tooa virtuálního počítače s Linuxem pomocí nasazení Classic hello modelu a inicializovat hello disk tak, aby byl připravený k použití"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 4901384d-2a6f-4f46-bba0-337a348b7f87
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: c76d8479ac2b522d2b6df658cd28f242473f30ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooattach-a-data-disk-tooa-linux-virtual-machine"></a><span data-ttu-id="a6095-103">Jak tooAttach datový Disk tooa Linux virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="a6095-103">How tooAttach a Data Disk tooa Linux Virtual Machine</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="a6095-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="a6095-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="a6095-105">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="a6095-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="a6095-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="a6095-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="a6095-107">V tématu Jak příliš[připojit datový disk pomocí modelu nasazení Resource Manager hello](../add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a6095-107">See how too[attach a data disk using hello Resource Manager deployment model](../add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="a6095-108">Můžete připojit prázdné disky a disky, které obsahují data tooyour virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="a6095-108">You can attach both empty disks and disks that contain data tooyour Azure VMs.</span></span> <span data-ttu-id="a6095-109">Oba typy disků jsou soubory VHD, které jsou umístěny v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="a6095-109">Both types of disks are .vhd files that reside in an Azure storage account.</span></span> <span data-ttu-id="a6095-110">Přidávání jakéhokoli počítače Linux tooa disku po připojit hello disk můžete potřebovat tooinitialize a naformátujte ho tak, aby byl připravený k použití.</span><span class="sxs-lookup"><span data-stu-id="a6095-110">As with adding any disk tooa Linux machine, after you attach hello disk you need tooinitialize and format it so it's ready for use.</span></span> <span data-ttu-id="a6095-111">Tento článek údaje prázdný disků i disků již obsahující data tooyour i virtuálních počítačů, jak toothen inicializace a formátování nový disk se připojuje.</span><span class="sxs-lookup"><span data-stu-id="a6095-111">This article details attaching both empty disks and disks already containing data tooyour VMs, as well as how toothen initialize and format a new disk.</span></span>

> [!NOTE]
> <span data-ttu-id="a6095-112">Je nejlepší postup toouse, jeden nebo více disků toostore oddělení dat virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a6095-112">It's a best practice toouse one or more separate disks toostore a virtual machine's data.</span></span> <span data-ttu-id="a6095-113">Když vytvoříte virtuální počítač Azure, je disk operačního systému a dočasný disk.</span><span class="sxs-lookup"><span data-stu-id="a6095-113">When you create an Azure virtual machine, it has an operating system disk and a temporary disk.</span></span> <span data-ttu-id="a6095-114">**Nepoužívejte hello dočasným diskovým toostore trvalá data.**</span><span class="sxs-lookup"><span data-stu-id="a6095-114">**Do not use hello temporary disk toostore persistent data.**</span></span> <span data-ttu-id="a6095-115">Jak hello název napovídá, obsahuje pouze dočasné úložiště.</span><span class="sxs-lookup"><span data-stu-id="a6095-115">As hello name implies, it provides temporary storage only.</span></span> <span data-ttu-id="a6095-116">Vzhledem k tomu, že není uložena v úložišti Azure nabízí žádné redundance nebo zálohování.</span><span class="sxs-lookup"><span data-stu-id="a6095-116">It offers no redundancy or backup because it doesn't reside in Azure storage.</span></span>
> <span data-ttu-id="a6095-117">dočasným diskovým Hello je obvykle spravuje hello Azure Linux Agent a automaticky připojit příliš**/mnt nebo prostředků** (nebo **/mnt** Ubuntu Image).</span><span class="sxs-lookup"><span data-stu-id="a6095-117">hello temporary disk is typically managed by hello Azure Linux Agent and automatically mounted too**/mnt/resource** (or **/mnt** on Ubuntu images).</span></span> <span data-ttu-id="a6095-118">Na hello druhé straně, datový disk může být pojmenován podle hello Linux jádra něco podobného jako `/dev/sdc`, a je třeba toopartition, formátování a připojte tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="a6095-118">On hello other hand, a data disk might be named by hello Linux kernel something like `/dev/sdc`, and you need toopartition, format, and mount this resource.</span></span> <span data-ttu-id="a6095-119">V tématu hello [Azure Linux Agent uživatelská příručka] [ Agent] podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="a6095-119">See hello [Azure Linux Agent User Guide][Agent] for details.</span></span>
> 
> 

[!INCLUDE [howto-attach-disk-windows-linux](../../../../includes/howto-attach-disk-linux.md)]

## <a name="initialize-a-new-data-disk-in-linux"></a><span data-ttu-id="a6095-120">Inicializace nový datový disk v systému Linux</span><span class="sxs-lookup"><span data-stu-id="a6095-120">Initialize a new data disk in Linux</span></span>
1. <span data-ttu-id="a6095-121">SSH tooyour virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a6095-121">SSH tooyour VM.</span></span> <span data-ttu-id="a6095-122">Další informace najdete v tématu [jak toolog na tooa virtuální počítač se systémem Linux][Logon].</span><span class="sxs-lookup"><span data-stu-id="a6095-122">For more information, see [How toolog on tooa virtual machine running Linux][Logon].</span></span>
2. <span data-ttu-id="a6095-123">Dále pro hello datového disku tooinitialize potřebovat identifikátor zařízení toofind hello.</span><span class="sxs-lookup"><span data-stu-id="a6095-123">Next you need toofind hello device identifier for hello data disk tooinitialize.</span></span> <span data-ttu-id="a6095-124">Existují dva způsoby toodo který:</span><span class="sxs-lookup"><span data-stu-id="a6095-124">There are two ways toodo that:</span></span>
   
    <span data-ttu-id="a6095-125">a) Grep pro zařízení SCSI v hello protokoly, například hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a6095-125">a) Grep for SCSI devices in hello logs, such as in hello following command:</span></span>
   
    ```bash
    sudo grep SCSI /var/log/messages
    ```
   
    <span data-ttu-id="a6095-126">Pro poslední Ubuntu distribuce, pravděpodobně bude třeba toouse `sudo grep SCSI /var/log/syslog` protože protokolování příliš`/var/log/messages` může ve výchozím nastavení zakázané.</span><span class="sxs-lookup"><span data-stu-id="a6095-126">For recent Ubuntu distributions, you may need toouse `sudo grep SCSI /var/log/syslog` because logging too`/var/log/messages` might be disabled by default.</span></span>
   
    <span data-ttu-id="a6095-127">Můžete najít identifikátor hello hello poslední datový disk, který se přidal ve hello zprávy, které jsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="a6095-127">You can find hello identifier of hello last data disk that was added in hello messages that are displayed.</span></span>
   
    ![Získávání zpráv disku hello](./media/attach-disk/scsidisklog.png)
   
    <span data-ttu-id="a6095-129">NEBO</span><span class="sxs-lookup"><span data-stu-id="a6095-129">OR</span></span>
   
    <span data-ttu-id="a6095-130">b) hello použijte `lsscsi` příkaz toofind se id zařízení hello. `lsscsi` může být instalován buď `yum install lsscsi` (na Red Hat na základě distribuce) nebo `apt-get install lsscsi` (na Debian na základě distribuce).</span><span class="sxs-lookup"><span data-stu-id="a6095-130">b) Use hello `lsscsi` command toofind out hello device id. `lsscsi` can be installed by either `yum install lsscsi` (on Red Hat based distributions) or `apt-get install lsscsi` (on Debian based distributions).</span></span> <span data-ttu-id="a6095-131">Můžete najít disk hello hledáte podle jeho *lun* nebo **číslo logické jednotky**.</span><span class="sxs-lookup"><span data-stu-id="a6095-131">You can find hello disk you are looking for by its *lun* or **logical unit number**.</span></span> <span data-ttu-id="a6095-132">Například hello *lun* pro připojené disky hello je snadno vidět z `azure vm disk list <virtual-machine>` jako:</span><span class="sxs-lookup"><span data-stu-id="a6095-132">For example, hello *lun* for hello disks you attached can be easily seen from `azure vm disk list <virtual-machine>` as:</span></span>

    ```azurecli
    azure vm disk list myVM
    ```

    <span data-ttu-id="a6095-133">výstup Hello je podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="a6095-133">hello output is similar toohello following:</span></span>

    ```azurecli
    info:    Executing command vm disk list
    + Fetching disk images
    + Getting virtual machines
    + Getting VM disks
    data:    Lun  Size(GB)  Blob-Name                         OS
    data:    ---  --------  --------------------------------  -----
    data:         30        myVM-2645b8030676c8f8.vhd  Linux
    data:    0    100       myVM-76f7ee1ef0f6dddc.vhd
    info:    vm disk list command OK
    ```
   
    <span data-ttu-id="a6095-134">Porovnat tato data se výstup hello `lsscsi` pro hello stejná ukázková virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="a6095-134">Compare this data with hello output of `lsscsi` for hello same sample virtual machine:</span></span>
   
    ```bash
    [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
    [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
    [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
    [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
    ```
   
    <span data-ttu-id="a6095-135">Poslední číslo hello řazené kolekce členů v jednotlivých řádcích Hello je hello *lun*.</span><span class="sxs-lookup"><span data-stu-id="a6095-135">hello last number in hello tuple in each row is hello *lun*.</span></span> <span data-ttu-id="a6095-136">V tématu `man lsscsi` Další informace.</span><span class="sxs-lookup"><span data-stu-id="a6095-136">See `man lsscsi` for more information.</span></span>
3. <span data-ttu-id="a6095-137">Hello řádku zadejte následující příkaz toocreate hello zařízení:</span><span class="sxs-lookup"><span data-stu-id="a6095-137">At hello prompt, type hello following command toocreate your device:</span></span>
   
    ```bash
    sudo fdisk /dev/sdc
    ```

4. <span data-ttu-id="a6095-138">Po zobrazení výzvy zadejte  **n**  toocreate oddílu.</span><span class="sxs-lookup"><span data-stu-id="a6095-138">When prompted, type **n** toocreate a partition.</span></span>

    ![Vytvoření zařízení](./media/attach-disk/fdisknewpartition.png)

5. <span data-ttu-id="a6095-140">Po zobrazení výzvy zadejte **p** toomake hello oddílu hello primární oddíl.</span><span class="sxs-lookup"><span data-stu-id="a6095-140">When prompted, type **p** toomake hello partition hello primary partition.</span></span> <span data-ttu-id="a6095-141">Typ **1** toomake hello prvního oddílu a poté zadejte zadejte tooaccept hello výchozí hodnota pro cylindr hello.</span><span class="sxs-lookup"><span data-stu-id="a6095-141">Type **1** toomake it hello first partition, and then type enter tooaccept hello default value for hello cylinder.</span></span> <span data-ttu-id="a6095-142">Na některé systémy může zobrazit výchozí hodnoty hello hello první a poslední sektory, místo hello cylindr hello.</span><span class="sxs-lookup"><span data-stu-id="a6095-142">On some systems, it can show hello default values of hello first and hello last sectors, instead of hello cylinder.</span></span> <span data-ttu-id="a6095-143">Tooaccept můžete tyto výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a6095-143">You can choose tooaccept these defaults.</span></span>

    ![Vytvořit oddíl](./media/attach-disk/fdisknewpartdetails.png)


6. <span data-ttu-id="a6095-145">Typ **p** toosee hello podrobnosti o hello disk, který je rozdělena na oddíly.</span><span class="sxs-lookup"><span data-stu-id="a6095-145">Type **p** toosee hello details about hello disk that is being partitioned.</span></span>

    ![Informace o disku seznamu](./media/attach-disk/fdiskpartitiondetails.png)


7. <span data-ttu-id="a6095-147">Typ **w** toowrite hello nastavení pro hello disk.</span><span class="sxs-lookup"><span data-stu-id="a6095-147">Type **w** toowrite hello settings for hello disk.</span></span>

    ![Zápis hello změny na disku](./media/attach-disk/fdiskwritedisk.png)

8. <span data-ttu-id="a6095-149">Nyní můžete vytvořit systém souborů hello na hello nový oddíl.</span><span class="sxs-lookup"><span data-stu-id="a6095-149">Now you can create hello file system on hello new partition.</span></span> <span data-ttu-id="a6095-150">Připojit ID číslo toohello zařízení hello oddílu (v hello následující ukázka `/dev/sdc1`).</span><span class="sxs-lookup"><span data-stu-id="a6095-150">Append hello partition number toohello device ID (in hello following example `/dev/sdc1`).</span></span> <span data-ttu-id="a6095-151">Hello následující příklad vytvoří ext4 oddíl na /dev/sdc1:</span><span class="sxs-lookup"><span data-stu-id="a6095-151">hello following example creates an ext4 partition on /dev/sdc1:</span></span>
   
    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
   
    ![Vytvořit systém souborů](./media/attach-disk/mkfsext4.png)
   
   > [!NOTE]
   > <span data-ttu-id="a6095-153">Systémy SuSE Linux Enterprise 11 pro systémy souborů ext4 podporují pouze oprávnění jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="a6095-153">SuSE Linux Enterprise 11 systems only support read-only access for ext4 file systems.</span></span> <span data-ttu-id="a6095-154">Pro tyto systémy se doporučuje tooformat hello nový systém souborů jako ext3, nikoli ext4.</span><span class="sxs-lookup"><span data-stu-id="a6095-154">For these systems, it is recommended tooformat hello new file system as ext3 rather than ext4.</span></span>

9. <span data-ttu-id="a6095-155">Ujistěte se, adresář toomount hello nový systém souborů, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a6095-155">Make a directory toomount hello new file system, as follows:</span></span>
   
    ```bash
    sudo mkdir /datadrive
    ```

10. <span data-ttu-id="a6095-156">Nakonec můžete připojit hello jednotku, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a6095-156">Finally you can mount hello drive, as follows:</span></span>
   
    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```
   
    <span data-ttu-id="a6095-157">Hello datový disk je nyní připraven toouse jako **/datadrive**.</span><span class="sxs-lookup"><span data-stu-id="a6095-157">hello data disk is now ready toouse as **/datadrive**.</span></span>
   
    ![Vytvoření disku hello hello directory a připojení](./media/attach-disk/mkdirandmount.png)

11. <span data-ttu-id="a6095-159">Přidejte hello nového disku příliš/atd/fstab:</span><span class="sxs-lookup"><span data-stu-id="a6095-159">Add hello new drive too/etc/fstab:</span></span>
   
    <span data-ttu-id="a6095-160">znovu tooensure hello jednotky je připojeny automaticky po restartování počítače, které musí být přidán toohello /etc/fstab souboru.</span><span class="sxs-lookup"><span data-stu-id="a6095-160">tooensure hello drive is remounted automatically after a reboot it must be added toohello /etc/fstab file.</span></span> <span data-ttu-id="a6095-161">Kromě toho důrazně doporučujeme tuto hello UUID (univerzálně jedinečný identifikátor) se používá v /etc/fstab toorefer toohello disku, nikoli jen název zařízení hello (tj. /dev/sdc1).</span><span class="sxs-lookup"><span data-stu-id="a6095-161">In addition, it is highly recommended that hello UUID (Universally Unique IDentifier) is used in /etc/fstab toorefer toohello drive rather than just hello device name (i.e. /dev/sdc1).</span></span> <span data-ttu-id="a6095-162">Pomocí hello UUID zabraňuje hello nesprávnou disku se připojené tooa zadané umístění, pokud hello OS zjistí chybu disk během spuštění a všechny zbývající datových disků, pak se ty přiřazené ID zařízení.</span><span class="sxs-lookup"><span data-stu-id="a6095-162">Using hello UUID avoids hello incorrect disk being mounted tooa given location if hello OS detects a disk error during boot and any remaining data disks then being assigned those device IDs.</span></span> <span data-ttu-id="a6095-163">toofind hello UUID hello nový disk, můžete použít hello **blkid** nástroj:</span><span class="sxs-lookup"><span data-stu-id="a6095-163">toofind hello UUID of hello new drive, you can use hello **blkid** utility:</span></span>
   
    ```bash
    sudo -i blkid
    ```
   
    <span data-ttu-id="a6095-164">výstup Hello vypadá podobně jako toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="a6095-164">hello output looks similar toohello following example:</span></span>
   
    ```bash
    /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
    /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
    /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
    ```

    > [!NOTE]
    > <span data-ttu-id="a6095-165">Nesprávně úpravy hello **/etc/fstab** soubor může mít za následek nelze spustit systém.</span><span class="sxs-lookup"><span data-stu-id="a6095-165">Improperly editing hello **/etc/fstab** file could result in an unbootable system.</span></span> <span data-ttu-id="a6095-166">Pokud si jisti, naleznete v dokumentaci toohello distribuční informace o tom, jak upravit tooproperly tento soubor.</span><span class="sxs-lookup"><span data-stu-id="a6095-166">If unsure, refer toohello distribution's documentation for information on how tooproperly edit this file.</span></span> <span data-ttu-id="a6095-167">Dále je doporučeno, jestli je vytvořená záloha souboru /etc/fstab hello před úpravou.</span><span class="sxs-lookup"><span data-stu-id="a6095-167">It is also recommended that a backup of hello /etc/fstab file is created before editing.</span></span>

    <span data-ttu-id="a6095-168">Dále otevřete hello **/etc/fstab** soubor v textovém editoru:</span><span class="sxs-lookup"><span data-stu-id="a6095-168">Next, open hello **/etc/fstab** file in a text editor:</span></span>

    ```bash
    sudo vi /etc/fstab
    ```

    <span data-ttu-id="a6095-169">V tomto příkladu používáme hello UUID hodnotu pro hello nové **/dev/sdc1** zařízení, který byl vytvořen v předchozích krocích hello a přípojný bod hello **/datadrive**.</span><span class="sxs-lookup"><span data-stu-id="a6095-169">In this example, we use hello UUID value for hello new **/dev/sdc1** device that was created in hello previous steps, and hello mountpoint **/datadrive**.</span></span> <span data-ttu-id="a6095-170">Přidejte následující řádek toohello konec hello hello **/etc/fstab** souboru:</span><span class="sxs-lookup"><span data-stu-id="a6095-170">Add hello following line toohello end of hello **/etc/fstab** file:</span></span>

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
    ```

    <span data-ttu-id="a6095-171">Nebo na systémy založené na systému SuSE Linux může být nutné toouse mírně odlišný formát:</span><span class="sxs-lookup"><span data-stu-id="a6095-171">Or, on systems based on SuSE Linux you may need toouse a slightly different format:</span></span>

    ```sh
    /dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext3   defaults,nofail   1   2
    ```

    > [!NOTE]
    > <span data-ttu-id="a6095-172">Hello `nofail` možnost zajistí, že hello virtuální počítač spustí i v případě systému souborů hello je poškozený nebo hello disk neexistuje při spuštění.</span><span class="sxs-lookup"><span data-stu-id="a6095-172">hello `nofail` option ensures that hello VM starts even if hello filesystem is corrupt or hello disk does not exist at boot time.</span></span> <span data-ttu-id="a6095-173">Bez této možnosti se můžete setkat chování jak je popsáno v [nelze SSH tooLinux virtuálních počítačů z důvodu chyby tooFSTAB](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/).</span><span class="sxs-lookup"><span data-stu-id="a6095-173">Without this option, you may encounter behavior as described in [Cannot SSH tooLinux VM due tooFSTAB errors](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/).</span></span>

    <span data-ttu-id="a6095-174">Nyní můžete otestovat, zda text hello připojení k systému souborů správně odpojování a pak opakovanému připojení hello systému souborů, tj. pomocí hello příklad přípojného bodu `/datadrive` vytvořené v hello dříve kroky:</span><span class="sxs-lookup"><span data-stu-id="a6095-174">You can now test that hello file system is mounted properly by unmounting and then remounting hello file system, i.e. using hello example mount point `/datadrive` created in hello earlier steps:</span></span>

    ```bash
    sudo umount /datadrive
    sudo mount /datadrive
    ```

    <span data-ttu-id="a6095-175">Pokud hello `mount` příkaz vytvořil chybu, zkontrolovat, zda text hello/etc/fstab soubor správnou syntaxi.</span><span class="sxs-lookup"><span data-stu-id="a6095-175">If hello `mount` command produces an error, check hello /etc/fstab file for correct syntax.</span></span> <span data-ttu-id="a6095-176">Pokud budou vytvořeny další datové jednotky nebo oddíly, zadejte je do/etc/fstab také samostatně.</span><span class="sxs-lookup"><span data-stu-id="a6095-176">If additional data drives or partitions are created, enter them into /etc/fstab separately as well.</span></span>

    <span data-ttu-id="a6095-177">Vytvořte jednotku hello zapisovatelné pomocí tohoto příkazu:</span><span class="sxs-lookup"><span data-stu-id="a6095-177">Make hello drive writable by using this command:</span></span>

    ```bash
    sudo chmod go+w /datadrive
    ```

    > [!NOTE]
    > <span data-ttu-id="a6095-178">Následně odebrat datový disk bez úprav fstab by mohlo způsobit tooboot toofail hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a6095-178">Subsequently removing a data disk without editing fstab could cause hello VM toofail tooboot.</span></span> <span data-ttu-id="a6095-179">Pokud je to běžné v situaci, většina distribuce zadejte buď hello `nofail` nebo `nobootwait` fstab možnosti, které umožňují tooboot systému i v případě selhání disku hello toomount při spuštění.</span><span class="sxs-lookup"><span data-stu-id="a6095-179">If this is a common occurrence, most distributions provide either hello `nofail` and/or `nobootwait` fstab options that allow a system tooboot even if hello disk fails toomount at boot time.</span></span> <span data-ttu-id="a6095-180">Další informace o těchto parametrů naleznete v dokumentaci vaší distribuce.</span><span class="sxs-lookup"><span data-stu-id="a6095-180">Consult your distribution's documentation for more information on these parameters.</span></span>

### <a name="trimunmap-support-for-linux-in-azure"></a><span data-ttu-id="a6095-181">Podpora uvolnění dočasné paměti nebo UNMAP pro Linux v Azure</span><span class="sxs-lookup"><span data-stu-id="a6095-181">TRIM/UNMAP support for Linux in Azure</span></span>
<span data-ttu-id="a6095-182">Některé jádra Linux podporují toodiscard operace TRIM/UNMAP nepoužívané bloky na disku hello.</span><span class="sxs-lookup"><span data-stu-id="a6095-182">Some Linux kernels support TRIM/UNMAP operations toodiscard unused blocks on hello disk.</span></span> <span data-ttu-id="a6095-183">Tyto operace jsou užitečné hlavně v tooinform standardní úložiště Azure, které odstraněné stránky již nejsou platné a může být vymazány.</span><span class="sxs-lookup"><span data-stu-id="a6095-183">These operations are primarily useful in standard storage tooinform Azure that deleted pages are no longer valid and can be discarded.</span></span> <span data-ttu-id="a6095-184">Zahození stránky můžete uložit náklady, pokud chcete vytvořit velkých souborů a pak odstraňte je.</span><span class="sxs-lookup"><span data-stu-id="a6095-184">Discarding pages can save cost if you create large files and then delete them.</span></span>

<span data-ttu-id="a6095-185">Existují dva způsoby tooenable TRIM podporují ve virtuálním počítačům s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="a6095-185">There are two ways tooenable TRIM support in your Linux VM.</span></span> <span data-ttu-id="a6095-186">Obvyklým způsobem podívejte se distribuční hello doporučenému přístupu:</span><span class="sxs-lookup"><span data-stu-id="a6095-186">As usual, consult your distribution for hello recommended approach:</span></span>

* <span data-ttu-id="a6095-187">Použití hello `discard` připojit možnost v `/etc/fstab`, například:</span><span class="sxs-lookup"><span data-stu-id="a6095-187">Use hello `discard` mount option in `/etc/fstab`, for example:</span></span>

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2
    ```

* <span data-ttu-id="a6095-188">V některých případech hello `discard` možnost může mít vliv na výkon.</span><span class="sxs-lookup"><span data-stu-id="a6095-188">In some cases hello `discard` option may have performance implications.</span></span> <span data-ttu-id="a6095-189">Alternativně můžete spustit hello `fstrim` ručně příkaz z příkazového řádku hello, nebo ho přidat tooyour crontab toorun pravidelně:</span><span class="sxs-lookup"><span data-stu-id="a6095-189">Alternatively, you can run hello `fstrim` command manually from hello command line, or add it tooyour crontab toorun regularly:</span></span>
  
    <span data-ttu-id="a6095-190">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="a6095-190">**Ubuntu**</span></span>
  
    ```bash
    sudo apt-get install util-linux
    sudo fstrim /datadrive
    ```
  
    <span data-ttu-id="a6095-191">**RHEL nebo CentOS**</span><span class="sxs-lookup"><span data-stu-id="a6095-191">**RHEL/CentOS**</span></span>
  
    ```bash
    sudo yum install util-linux
    sudo fstrim /datadrive
    ```

## <a name="troubleshooting"></a><span data-ttu-id="a6095-192">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="a6095-192">Troubleshooting</span></span>
[!INCLUDE [virtual-machines-linux-lunzero](../../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a><span data-ttu-id="a6095-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a6095-193">Next Steps</span></span>
<span data-ttu-id="a6095-194">Další informace o používání virtuálním počítačům s Linuxem v hello následující články:</span><span class="sxs-lookup"><span data-stu-id="a6095-194">You can read more about using your Linux VM in hello following articles:</span></span>

* <span data-ttu-id="a6095-195">[Jak toolog na tooa virtuální počítač se systémem Linux][Logon]</span><span class="sxs-lookup"><span data-stu-id="a6095-195">[How toolog on tooa virtual machine running Linux][Logon]</span></span>
* [<span data-ttu-id="a6095-196">Jak toodetach disk z virtuálního počítače systému Linux</span><span class="sxs-lookup"><span data-stu-id="a6095-196">How toodetach a disk from a Linux virtual machine</span></span>](detach-disk.md)
* [<span data-ttu-id="a6095-197">Pomocí modelu nasazení Classic hello hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="a6095-197">Using hello Azure CLI with hello Classic deployment model</span></span>](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [<span data-ttu-id="a6095-198">Konfigurace RAID na virtuální počítač s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="a6095-198">Configure RAID on a Linux VM in Azure</span></span>](../configure-raid.md)
* [<span data-ttu-id="a6095-199">Konfigurace LVM na virtuální počítač s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="a6095-199">Configure LVM on a Linux VM in Azure</span></span>](../configure-lvm.md)

<!--Link references-->
[Agent]:../agent-user-guide.md
[Logon]:../mac-create-ssh-keys.md
