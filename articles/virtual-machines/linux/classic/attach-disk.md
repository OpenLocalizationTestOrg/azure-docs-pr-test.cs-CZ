---
title: "Připojit disk do virtuálního počítače s Linuxem v Azure | Microsoft Docs"
description: "Zjistěte, jak připojit datový disk do virtuálního počítače s Linuxem pomocí modelu nasazení Classic a inicializujte disk tak, aby byl připravený k použití"
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
ms.openlocfilehash: 017ba7197e11c2b222082833d5acabb9e542b762
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-attach-a-data-disk-to-a-linux-virtual-machine"></a><span data-ttu-id="f3c89-103">Tom, jak připojit datový Disk pro virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="f3c89-103">How to Attach a Data Disk to a Linux Virtual Machine</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="f3c89-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="f3c89-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="f3c89-105">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="f3c89-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="f3c89-106">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="f3c89-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="f3c89-107">V tématu Jak [připojit datový disk pomocí modelu nasazení Resource Manager](../add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f3c89-107">See how to [attach a data disk using the Resource Manager deployment model](../add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="f3c89-108">Můžete připojit prázdné disky a disky, které obsahují data pro virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="f3c89-108">You can attach both empty disks and disks that contain data to your Azure VMs.</span></span> <span data-ttu-id="f3c89-109">Oba typy disků jsou soubory VHD, které jsou umístěny v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="f3c89-109">Both types of disks are .vhd files that reside in an Azure storage account.</span></span> <span data-ttu-id="f3c89-110">Jako přidávání každý disk, na počítač s Linuxem, jakmile připojíte disk musíte inicializovat a naformátovat ho tak, aby byl připravený k použití.</span><span class="sxs-lookup"><span data-stu-id="f3c89-110">As with adding any disk to a Linux machine, after you attach the disk you need to initialize and format it so it's ready for use.</span></span> <span data-ttu-id="f3c89-111">Tento článek údaje prázdný disků i disků již obsahující data do virtuálních počítačů a také jak pak inicializace a formátování nový disk se připojuje.</span><span class="sxs-lookup"><span data-stu-id="f3c89-111">This article details attaching both empty disks and disks already containing data to your VMs, as well as how to then initialize and format a new disk.</span></span>

> [!NOTE]
> <span data-ttu-id="f3c89-112">Je vhodné použít jeden nebo více samostatných disků k ukládání dat virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f3c89-112">It's a best practice to use one or more separate disks to store a virtual machine's data.</span></span> <span data-ttu-id="f3c89-113">Když vytvoříte virtuální počítač Azure, je disk operačního systému a dočasný disk.</span><span class="sxs-lookup"><span data-stu-id="f3c89-113">When you create an Azure virtual machine, it has an operating system disk and a temporary disk.</span></span> <span data-ttu-id="f3c89-114">**Nepoužívejte dočasné disku k uložení dat trvalé.**</span><span class="sxs-lookup"><span data-stu-id="f3c89-114">**Do not use the temporary disk to store persistent data.**</span></span> <span data-ttu-id="f3c89-115">Jak již název napovídá, obsahuje pouze dočasné úložiště.</span><span class="sxs-lookup"><span data-stu-id="f3c89-115">As the name implies, it provides temporary storage only.</span></span> <span data-ttu-id="f3c89-116">Vzhledem k tomu, že není uložena v úložišti Azure nabízí žádné redundance nebo zálohování.</span><span class="sxs-lookup"><span data-stu-id="f3c89-116">It offers no redundancy or backup because it doesn't reside in Azure storage.</span></span>
> <span data-ttu-id="f3c89-117">Je obvykle spravuje Azure Linux Agent a automaticky připojit k dočasným diskovým **/mnt nebo prostředků** (nebo **/mnt** Ubuntu Image).</span><span class="sxs-lookup"><span data-stu-id="f3c89-117">The temporary disk is typically managed by the Azure Linux Agent and automatically mounted to **/mnt/resource** (or **/mnt** on Ubuntu images).</span></span> <span data-ttu-id="f3c89-118">Na druhé straně datový disk může být pojmenován jádrem Linux něco podobného jako `/dev/sdc`, a je třeba k oddílu, formátování a připojte tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="f3c89-118">On the other hand, a data disk might be named by the Linux kernel something like `/dev/sdc`, and you need to partition, format, and mount this resource.</span></span> <span data-ttu-id="f3c89-119">Najdete v článku [Azure Linux Agent uživatelská příručka] [ Agent] podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="f3c89-119">See the [Azure Linux Agent User Guide][Agent] for details.</span></span>
> 
> 

[!INCLUDE [howto-attach-disk-windows-linux](../../../../includes/howto-attach-disk-linux.md)]

## <a name="initialize-a-new-data-disk-in-linux"></a><span data-ttu-id="f3c89-120">Inicializace nový datový disk v systému Linux</span><span class="sxs-lookup"><span data-stu-id="f3c89-120">Initialize a new data disk in Linux</span></span>
1. <span data-ttu-id="f3c89-121">SSH k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="f3c89-121">SSH to your VM.</span></span> <span data-ttu-id="f3c89-122">Další informace najdete v tématu [přihlášení do virtuálního počítače se systémem Linux][Logon].</span><span class="sxs-lookup"><span data-stu-id="f3c89-122">For more information, see [How to log on to a virtual machine running Linux][Logon].</span></span>
2. <span data-ttu-id="f3c89-123">Dále je třeba najít identifikátor zařízení pro datový disk k chybě při inicializaci.</span><span class="sxs-lookup"><span data-stu-id="f3c89-123">Next you need to find the device identifier for the data disk to initialize.</span></span> <span data-ttu-id="f3c89-124">Existují dva způsoby, jak to udělat:</span><span class="sxs-lookup"><span data-stu-id="f3c89-124">There are two ways to do that:</span></span>
   
    <span data-ttu-id="f3c89-125">(a) Grep pro zařízení SCSI v protokolech, například následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f3c89-125">a) Grep for SCSI devices in the logs, such as in the following command:</span></span>
   
    ```bash
    sudo grep SCSI /var/log/messages
    ```
   
    <span data-ttu-id="f3c89-126">Pro poslední Ubuntu distribuce, budete muset použít `sudo grep SCSI /var/log/syslog` protože protokolování tak, aby `/var/log/messages` může ve výchozím nastavení zakázané.</span><span class="sxs-lookup"><span data-stu-id="f3c89-126">For recent Ubuntu distributions, you may need to use `sudo grep SCSI /var/log/syslog` because logging to `/var/log/messages` might be disabled by default.</span></span>
   
    <span data-ttu-id="f3c89-127">Můžete najít identifikátor poslední datový disk, která byla přidána do zpráv, které se zobrazují.</span><span class="sxs-lookup"><span data-stu-id="f3c89-127">You can find the identifier of the last data disk that was added in the messages that are displayed.</span></span>
   
    ![Získávání zpráv disku](./media/attach-disk/scsidisklog.png)
   
    <span data-ttu-id="f3c89-129">NEBO</span><span class="sxs-lookup"><span data-stu-id="f3c89-129">OR</span></span>
   
    <span data-ttu-id="f3c89-130">b) použití `lsscsi` příkazu zjistit id zařízení.</span><span class="sxs-lookup"><span data-stu-id="f3c89-130">b) Use the `lsscsi` command to find out the device id.</span></span> <span data-ttu-id="f3c89-131">`lsscsi` můžete nainstalovat pomocí příkazu `yum install lsscsi` (v distribucích založených na Red Hat) nebo `apt-get install lsscsi` (v distribucích založených na Debian).</span><span class="sxs-lookup"><span data-stu-id="f3c89-131">`lsscsi` can be installed by either `yum install lsscsi` (on Red Hat based distributions) or `apt-get install lsscsi` (on Debian based distributions).</span></span> <span data-ttu-id="f3c89-132">Můžete najít na disku, kterou hledáte podle jeho *lun* nebo **číslo logické jednotky**.</span><span class="sxs-lookup"><span data-stu-id="f3c89-132">You can find the disk you are looking for by its *lun* or **logical unit number**.</span></span> <span data-ttu-id="f3c89-133">Například *lun* pro disky můžete z snadno pohledu `azure vm disk list <virtual-machine>` jako:</span><span class="sxs-lookup"><span data-stu-id="f3c89-133">For example, the *lun* for the disks you attached can be easily seen from `azure vm disk list <virtual-machine>` as:</span></span>

    ```azurecli
    azure vm disk list myVM
    ```

    <span data-ttu-id="f3c89-134">Výstup je podobný tomuto:</span><span class="sxs-lookup"><span data-stu-id="f3c89-134">The output is similar to the following:</span></span>

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
   
    <span data-ttu-id="f3c89-135">Tato data se výstup porovnání `lsscsi` pro stejné ukázkové virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="f3c89-135">Compare this data with the output of `lsscsi` for the same sample virtual machine:</span></span>
   
    ```bash
    [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
    [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
    [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
    [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
    ```
   
    <span data-ttu-id="f3c89-136">Je poslední číslo v řazené kolekci členů v každém řádku *lun*.</span><span class="sxs-lookup"><span data-stu-id="f3c89-136">The last number in the tuple in each row is the *lun*.</span></span> <span data-ttu-id="f3c89-137">V tématu `man lsscsi` Další informace.</span><span class="sxs-lookup"><span data-stu-id="f3c89-137">See `man lsscsi` for more information.</span></span>
3. <span data-ttu-id="f3c89-138">Do příkazového řádku zadejte následující příkaz k vytvoření vašeho zařízení:</span><span class="sxs-lookup"><span data-stu-id="f3c89-138">At the prompt, type the following command to create your device:</span></span>
   
    ```bash
    sudo fdisk /dev/sdc
    ```

4. <span data-ttu-id="f3c89-139">Po zobrazení výzvy zadejte  **n**  k vytvoření oddílu.</span><span class="sxs-lookup"><span data-stu-id="f3c89-139">When prompted, type **n** to create a partition.</span></span>

    ![Vytvoření zařízení](./media/attach-disk/fdisknewpartition.png)

5. <span data-ttu-id="f3c89-141">Po zobrazení výzvy zadejte **p** změnit primární oddíl na oddíl.</span><span class="sxs-lookup"><span data-stu-id="f3c89-141">When prompted, type **p** to make the partition the primary partition.</span></span> <span data-ttu-id="f3c89-142">Typ **1** na první oddíl a pak zadejte zadejte přijměte výchozí hodnotu cylindr.</span><span class="sxs-lookup"><span data-stu-id="f3c89-142">Type **1** to make it the first partition, and then type enter to accept the default value for the cylinder.</span></span> <span data-ttu-id="f3c89-143">U některých systémů může zobrazit výchozí hodnoty, první a poslední sektory, místo cylindr.</span><span class="sxs-lookup"><span data-stu-id="f3c89-143">On some systems, it can show the default values of the first and the last sectors, instead of the cylinder.</span></span> <span data-ttu-id="f3c89-144">Můžete tak, aby přijímal tyto výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f3c89-144">You can choose to accept these defaults.</span></span>

    ![Vytvořit oddíl](./media/attach-disk/fdisknewpartdetails.png)


6. <span data-ttu-id="f3c89-146">Typ **p** a zobrazit podrobnosti o disk, který je rozdělena na oddíly.</span><span class="sxs-lookup"><span data-stu-id="f3c89-146">Type **p** to see the details about the disk that is being partitioned.</span></span>

    ![Informace o disku seznamu](./media/attach-disk/fdiskpartitiondetails.png)


7. <span data-ttu-id="f3c89-148">Typ **w** se zapsat nastavení disku.</span><span class="sxs-lookup"><span data-stu-id="f3c89-148">Type **w** to write the settings for the disk.</span></span>

    ![Zápis disku změny](./media/attach-disk/fdiskwritedisk.png)

8. <span data-ttu-id="f3c89-150">Nyní můžete vytvořit systém souborů na nový oddíl.</span><span class="sxs-lookup"><span data-stu-id="f3c89-150">Now you can create the file system on the new partition.</span></span> <span data-ttu-id="f3c89-151">Připojit číslo oddílu k ID zařízení (v následujícím příkladu `/dev/sdc1`).</span><span class="sxs-lookup"><span data-stu-id="f3c89-151">Append the partition number to the device ID (in the following example `/dev/sdc1`).</span></span> <span data-ttu-id="f3c89-152">Následující příklad vytvoří oddíl ext4 na /dev/sdc1:</span><span class="sxs-lookup"><span data-stu-id="f3c89-152">The following example creates an ext4 partition on /dev/sdc1:</span></span>
   
    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
   
    ![Vytvořit systém souborů](./media/attach-disk/mkfsext4.png)
   
   > [!NOTE]
   > <span data-ttu-id="f3c89-154">Systémy SuSE Linux Enterprise 11 pro systémy souborů ext4 podporují pouze oprávnění jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="f3c89-154">SuSE Linux Enterprise 11 systems only support read-only access for ext4 file systems.</span></span> <span data-ttu-id="f3c89-155">Pro tyto systémy doporučujeme formátovat jako ext3, nikoli ext4 nový systém souborů.</span><span class="sxs-lookup"><span data-stu-id="f3c89-155">For these systems, it is recommended to format the new file system as ext3 rather than ext4.</span></span>

9. <span data-ttu-id="f3c89-156">Aby byl adresář připojit nový systém souborů, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f3c89-156">Make a directory to mount the new file system, as follows:</span></span>
   
    ```bash
    sudo mkdir /datadrive
    ```

10. <span data-ttu-id="f3c89-157">Nakonec můžete připojit jednotku, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f3c89-157">Finally you can mount the drive, as follows:</span></span>
   
    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```
   
    <span data-ttu-id="f3c89-158">Datový disk je teď připravený k použití jako **/datadrive**.</span><span class="sxs-lookup"><span data-stu-id="f3c89-158">The data disk is now ready to use as **/datadrive**.</span></span>
   
    ![Vytvoření adresáře a připojit disk](./media/attach-disk/mkdirandmount.png)

11. <span data-ttu-id="f3c89-160">Přidejte nový disk /etc/fstab:</span><span class="sxs-lookup"><span data-stu-id="f3c89-160">Add the new drive to /etc/fstab:</span></span>
   
    <span data-ttu-id="f3c89-161">K zajištění, že jednotka je znovu připojeny automaticky po restartování systému musí být přidané do souboru/etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="f3c89-161">To ensure the drive is remounted automatically after a reboot it must be added to the /etc/fstab file.</span></span> <span data-ttu-id="f3c89-162">Kromě toho je důrazně doporučujeme, aby identifikátor UUID (univerzálně jedinečný identifikátor) se používá v /etc/fstab k odkazování na jednotku, nikoli jen název zařízení (tj. /dev/sdc1).</span><span class="sxs-lookup"><span data-stu-id="f3c89-162">In addition, it is highly recommended that the UUID (Universally Unique IDentifier) is used in /etc/fstab to refer to the drive rather than just the device name (i.e. /dev/sdc1).</span></span> <span data-ttu-id="f3c89-163">Pomocí identifikátoru UUID zabraňuje nesprávnou disku se připojí na určitém místě, když operačního systému zjistí chybu disk během spuštění a všechny zbývající datové disky poté přiřazeny ID těchto zařízení.</span><span class="sxs-lookup"><span data-stu-id="f3c89-163">Using the UUID avoids the incorrect disk being mounted to a given location if the OS detects a disk error during boot and any remaining data disks then being assigned those device IDs.</span></span> <span data-ttu-id="f3c89-164">Chcete-li najít identifikátor UUID nový disk, můžete použít **blkid** nástroj:</span><span class="sxs-lookup"><span data-stu-id="f3c89-164">To find the UUID of the new drive, you can use the **blkid** utility:</span></span>
   
    ```bash
    sudo -i blkid
    ```
   
    <span data-ttu-id="f3c89-165">Výstup bude vypadat podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="f3c89-165">The output looks similar to the following example:</span></span>
   
    ```bash
    /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
    /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
    /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
    ```

    > [!NOTE]
    > <span data-ttu-id="f3c89-166">Nesprávně úpravy **/etc/fstab** soubor může mít za následek nelze spustit systém.</span><span class="sxs-lookup"><span data-stu-id="f3c89-166">Improperly editing the **/etc/fstab** file could result in an unbootable system.</span></span> <span data-ttu-id="f3c89-167">Pokud jistí, naleznete distribuční dokumentaci informace o tom, jak správně upravit tento soubor.</span><span class="sxs-lookup"><span data-stu-id="f3c89-167">If unsure, refer to the distribution's documentation for information on how to properly edit this file.</span></span> <span data-ttu-id="f3c89-168">Dále je doporučeno, jestli je vytvořená záloha souboru /etc/fstab před úpravou.</span><span class="sxs-lookup"><span data-stu-id="f3c89-168">It is also recommended that a backup of the /etc/fstab file is created before editing.</span></span>

    <span data-ttu-id="f3c89-169">Dále otevřete **/etc/fstab** soubor v textovém editoru:</span><span class="sxs-lookup"><span data-stu-id="f3c89-169">Next, open the **/etc/fstab** file in a text editor:</span></span>

    ```bash
    sudo vi /etc/fstab
    ```

    <span data-ttu-id="f3c89-170">V tomto příkladu používáme UUID hodnotu pro nové **/dev/sdc1** zařízení, který byl vytvořen v předchozích krocích a přípojný bod **/datadrive**.</span><span class="sxs-lookup"><span data-stu-id="f3c89-170">In this example, we use the UUID value for the new **/dev/sdc1** device that was created in the previous steps, and the mountpoint **/datadrive**.</span></span> <span data-ttu-id="f3c89-171">Přidejte následující řádek na konec **/etc/fstab** souboru:</span><span class="sxs-lookup"><span data-stu-id="f3c89-171">Add the following line to the end of the **/etc/fstab** file:</span></span>

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
    ```

    <span data-ttu-id="f3c89-172">Nebo na systémy založené na systému SuSE Linux budete muset použít mírně odlišný formát:</span><span class="sxs-lookup"><span data-stu-id="f3c89-172">Or, on systems based on SuSE Linux you may need to use a slightly different format:</span></span>

    ```sh
    /dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext3   defaults,nofail   1   2
    ```

    > [!NOTE]
    > <span data-ttu-id="f3c89-173">`nofail` Možnost zajistí, že virtuální počítač spustí i v případě, že systém souborů je poškozený nebo disk neexistuje při spuštění.</span><span class="sxs-lookup"><span data-stu-id="f3c89-173">The `nofail` option ensures that the VM starts even if the filesystem is corrupt or the disk does not exist at boot time.</span></span> <span data-ttu-id="f3c89-174">Bez této možnosti se můžete setkat chování jak je popsáno v [nelze SSH pro virtuální počítač s Linuxem z důvodu chyb FSTAB](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/).</span><span class="sxs-lookup"><span data-stu-id="f3c89-174">Without this option, you may encounter behavior as described in [Cannot SSH to Linux VM due to FSTAB errors](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/).</span></span>

    <span data-ttu-id="f3c89-175">Nyní můžete otestovat, že systém souborů správně připojený odpojování a pak opakovanému připojení systému souborů, tj.</span><span class="sxs-lookup"><span data-stu-id="f3c89-175">You can now test that the file system is mounted properly by unmounting and then remounting the file system, i.e.</span></span> <span data-ttu-id="f3c89-176">Příklad použití přípojného bodu `/datadrive` vytvořené v dřívějších krocích:</span><span class="sxs-lookup"><span data-stu-id="f3c89-176">using the example mount point `/datadrive` created in the earlier steps:</span></span>

    ```bash
    sudo umount /datadrive
    sudo mount /datadrive
    ```

    <span data-ttu-id="f3c89-177">Pokud `mount` příkaz vytvořil chybu, zkontrolujte soubor fstab/etc/pro správnou syntaxi.</span><span class="sxs-lookup"><span data-stu-id="f3c89-177">If the `mount` command produces an error, check the /etc/fstab file for correct syntax.</span></span> <span data-ttu-id="f3c89-178">Pokud budou vytvořeny další datové jednotky nebo oddíly, zadejte je do/etc/fstab také samostatně.</span><span class="sxs-lookup"><span data-stu-id="f3c89-178">If additional data drives or partitions are created, enter them into /etc/fstab separately as well.</span></span>

    <span data-ttu-id="f3c89-179">Zkontrolujte jednotku s možností zápisu pomocí tohoto příkazu:</span><span class="sxs-lookup"><span data-stu-id="f3c89-179">Make the drive writable by using this command:</span></span>

    ```bash
    sudo chmod go+w /datadrive
    ```

    > [!NOTE]
    > <span data-ttu-id="f3c89-180">Následně odebrat datový disk bez úprav fstab může způsobit selhání spuštění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f3c89-180">Subsequently removing a data disk without editing fstab could cause the VM to fail to boot.</span></span> <span data-ttu-id="f3c89-181">Pokud je to běžné v situaci, většina distribuce zadejte buď `nofail` nebo `nobootwait` dobou spuštění fstab možnosti, které umožňují spustit i v případě, že na disku se nepodaří připojit v systému.</span><span class="sxs-lookup"><span data-stu-id="f3c89-181">If this is a common occurrence, most distributions provide either the `nofail` and/or `nobootwait` fstab options that allow a system to boot even if the disk fails to mount at boot time.</span></span> <span data-ttu-id="f3c89-182">Další informace o těchto parametrů naleznete v dokumentaci vaší distribuce.</span><span class="sxs-lookup"><span data-stu-id="f3c89-182">Consult your distribution's documentation for more information on these parameters.</span></span>

### <a name="trimunmap-support-for-linux-in-azure"></a><span data-ttu-id="f3c89-183">Podpora uvolnění dočasné paměti nebo UNMAP pro Linux v Azure</span><span class="sxs-lookup"><span data-stu-id="f3c89-183">TRIM/UNMAP support for Linux in Azure</span></span>
<span data-ttu-id="f3c89-184">Některé Linux jádra podporovat operace TRIM/UNMAP vyřadí nepoužívané bloky na disku.</span><span class="sxs-lookup"><span data-stu-id="f3c89-184">Some Linux kernels support TRIM/UNMAP operations to discard unused blocks on the disk.</span></span> <span data-ttu-id="f3c89-185">Tyto operace jsou užitečné hlavně v standardní úložiště k informování Azure, které odstraněné stránky již nejsou platné a může být vymazány.</span><span class="sxs-lookup"><span data-stu-id="f3c89-185">These operations are primarily useful in standard storage to inform Azure that deleted pages are no longer valid and can be discarded.</span></span> <span data-ttu-id="f3c89-186">Zahození stránky můžete uložit náklady, pokud chcete vytvořit velkých souborů a pak odstraňte je.</span><span class="sxs-lookup"><span data-stu-id="f3c89-186">Discarding pages can save cost if you create large files and then delete them.</span></span>

<span data-ttu-id="f3c89-187">Existují dva způsoby, jak povolit TRIM podporují ve virtuálním počítačům s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="f3c89-187">There are two ways to enable TRIM support in your Linux VM.</span></span> <span data-ttu-id="f3c89-188">Obvyklým způsobem podívejte se distribuční o doporučený postup:</span><span class="sxs-lookup"><span data-stu-id="f3c89-188">As usual, consult your distribution for the recommended approach:</span></span>

* <span data-ttu-id="f3c89-189">Použití `discard` připojit možnost v `/etc/fstab`, například:</span><span class="sxs-lookup"><span data-stu-id="f3c89-189">Use the `discard` mount option in `/etc/fstab`, for example:</span></span>

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2
    ```

* <span data-ttu-id="f3c89-190">V některých případech `discard` možnost může mít vliv na výkon.</span><span class="sxs-lookup"><span data-stu-id="f3c89-190">In some cases the `discard` option may have performance implications.</span></span> <span data-ttu-id="f3c89-191">Alternativně můžete spustit `fstrim` ručně příkaz z příkazového řádku, nebo ho přidat do vaší crontab pravidelně spouštět:</span><span class="sxs-lookup"><span data-stu-id="f3c89-191">Alternatively, you can run the `fstrim` command manually from the command line, or add it to your crontab to run regularly:</span></span>
  
    <span data-ttu-id="f3c89-192">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="f3c89-192">**Ubuntu**</span></span>
  
    ```bash
    sudo apt-get install util-linux
    sudo fstrim /datadrive
    ```
  
    <span data-ttu-id="f3c89-193">**RHEL nebo CentOS**</span><span class="sxs-lookup"><span data-stu-id="f3c89-193">**RHEL/CentOS**</span></span>
  
    ```bash
    sudo yum install util-linux
    sudo fstrim /datadrive
    ```

## <a name="troubleshooting"></a><span data-ttu-id="f3c89-194">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="f3c89-194">Troubleshooting</span></span>
[!INCLUDE [virtual-machines-linux-lunzero](../../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a><span data-ttu-id="f3c89-195">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f3c89-195">Next Steps</span></span>
<span data-ttu-id="f3c89-196">Další informace o používání virtuálním počítačům s Linuxem v těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="f3c89-196">You can read more about using your Linux VM in the following articles:</span></span>

* <span data-ttu-id="f3c89-197">[Jak se přihlásit do virtuálního počítače se systémem Linux][Logon]</span><span class="sxs-lookup"><span data-stu-id="f3c89-197">[How to log on to a virtual machine running Linux][Logon]</span></span>
* [<span data-ttu-id="f3c89-198">Jak se odpojit disk z virtuálního počítače systému Linux</span><span class="sxs-lookup"><span data-stu-id="f3c89-198">How to detach a disk from a Linux virtual machine</span></span>](detach-disk.md)
* [<span data-ttu-id="f3c89-199">Pomocí rozhraní příkazového řádku Azure s modelem nasazení Classic</span><span class="sxs-lookup"><span data-stu-id="f3c89-199">Using the Azure CLI with the Classic deployment model</span></span>](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [<span data-ttu-id="f3c89-200">Konfigurace RAID na virtuální počítač s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="f3c89-200">Configure RAID on a Linux VM in Azure</span></span>](../configure-raid.md)
* [<span data-ttu-id="f3c89-201">Konfigurace LVM na virtuální počítač s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="f3c89-201">Configure LVM on a Linux VM in Azure</span></span>](../configure-lvm.md)

<!--Link references-->
[Agent]:../agent-user-guide.md
[Logon]:../mac-create-ssh-keys.md
