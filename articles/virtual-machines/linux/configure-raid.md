---
title: "Konfigurace softwaru diskového pole RAID na virtuálním počítači se systémem Linux | Microsoft Docs"
description: "Další informace o použití mdadm ke konfiguraci RAID na systému Linux v Azure."
services: virtual-machines-linux
documentationcenter: na
author: rickstercdn
manager: timlt
editor: tysonn
tag: azure-service-management,azure-resource-manager
ms.assetid: f3cb2786-bda6-4d2c-9aaf-2db80f490feb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: rclaus
ms.openlocfilehash: 12f540a700fbf85e579e8aadc9f6def039299ff7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-software-raid-on-linux"></a><span data-ttu-id="30de5-103">Konfigurace softwarového pole RAID v Linuxu</span><span class="sxs-lookup"><span data-stu-id="30de5-103">Configure Software RAID on Linux</span></span>
<span data-ttu-id="30de5-104">Je běžný scénář pomocí softwaru diskového pole RAID na virtuální počítače s Linuxem v Azure k dispozici více připojených datových disků jako jedno zařízení RAID.</span><span class="sxs-lookup"><span data-stu-id="30de5-104">It's a common scenario to use software RAID on Linux virtual machines in Azure to present multiple attached data disks as a single RAID device.</span></span> <span data-ttu-id="30de5-105">Obvykle to lze zlepšit výkon a umožňuje lepší výkon ve srovnání s použitím právě jeden disk.</span><span class="sxs-lookup"><span data-stu-id="30de5-105">Typically this can be used to improve performance and allow for improved throughput compared to using just a single disk.</span></span>

## <a name="attaching-data-disks"></a><span data-ttu-id="30de5-106">Připojení datových disků</span><span class="sxs-lookup"><span data-stu-id="30de5-106">Attaching data disks</span></span>
<span data-ttu-id="30de5-107">Dvě nebo více prázdný datových disků jsou potřeba ke konfiguraci RAID zařízení.</span><span class="sxs-lookup"><span data-stu-id="30de5-107">Two or more empty data disks are needed to configure a RAID device.</span></span>  <span data-ttu-id="30de5-108">Hlavním důvodem pro vytváření diskového pole RAID zařízení je ke zlepšení výkonu disku vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="30de5-108">The primary reason for creating a RAID device is to improve performance of your disk IO.</span></span>  <span data-ttu-id="30de5-109">Podle potřeb vstupně-výstupní operace, můžete připojit disky, které jsou uložené v našem standardního úložiště, s maximálně 500 vstupně-výstupní operace/ps pro na disk nebo naše storage úrovně Premium s až 5000 vstupně-výstupní operace za ps na disk.</span><span class="sxs-lookup"><span data-stu-id="30de5-109">Based on your IO needs, you can choose to attach disks that are stored in our Standard Storage, with up to 500 IO/ps per disk or our Premium storage with up to 5000 IO/ps per disk.</span></span> <span data-ttu-id="30de5-110">Tento článek nepřekračuje podrobnosti o tom, jak zřídit a připojte datových disků pro virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="30de5-110">This article does not go into detail on how to provision and attach data disks to a Linux virtual machine.</span></span>  <span data-ttu-id="30de5-111">Najdete v článku Microsoft Azure [připojit disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) podrobné pokyny o tom, jak připojit prázdný datový disk pro virtuální počítač s Linuxem v Azure.</span><span class="sxs-lookup"><span data-stu-id="30de5-111">See the Microsoft Azure article [attach a disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for detailed instructions on how to attach an empty data disk to a Linux virtual machine on Azure.</span></span>

## <a name="install-the-mdadm-utility"></a><span data-ttu-id="30de5-112">Nainstalujte nástroj mdadm</span><span class="sxs-lookup"><span data-stu-id="30de5-112">Install the mdadm utility</span></span>
* <span data-ttu-id="30de5-113">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="30de5-113">**Ubuntu**</span></span>
```bash
sudo apt-get update
sudo apt-get install mdadm
```

* <span data-ttu-id="30de5-114">**CentOS & Oracle Linux**</span><span class="sxs-lookup"><span data-stu-id="30de5-114">**CentOS & Oracle Linux**</span></span>
```bash
sudo yum install mdadm
```

* <span data-ttu-id="30de5-115">**SLES a openSUSE**</span><span class="sxs-lookup"><span data-stu-id="30de5-115">**SLES and openSUSE**</span></span>
```bash  
zypper install mdadm
```

## <a name="create-the-disk-partitions"></a><span data-ttu-id="30de5-116">Vytvoření oddílů disku</span><span class="sxs-lookup"><span data-stu-id="30de5-116">Create the disk partitions</span></span>
<span data-ttu-id="30de5-117">V tomto příkladu vytvoříme na /dev/sdc oddíl jediný disk.</span><span class="sxs-lookup"><span data-stu-id="30de5-117">In this example, we create a single disk partition on /dev/sdc.</span></span> <span data-ttu-id="30de5-118">Nový diskový oddíl bude volána /dev/sdc1.</span><span class="sxs-lookup"><span data-stu-id="30de5-118">The new disk partition will be called /dev/sdc1.</span></span>

1. <span data-ttu-id="30de5-119">Spustit `fdisk` zahajte proces vytváření oddílů</span><span class="sxs-lookup"><span data-stu-id="30de5-119">Start `fdisk` to begin creating partitions</span></span>

    ```bash
    sudo fdisk /dev/sdc
    Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
    Building a new DOS disklabel with disk identifier 0xa34cb70c.
    Changes will remain in memory only, until you decide to write them.
    After that, of course, the previous content won't be recoverable.

    WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
                    switch off the mode (command 'c') and change display units to
                    sectors (command 'u').
    ```

2. <span data-ttu-id="30de5-120">Stiskněte klávesu n příkazového řádku k vytvoření  **n** ové oddílu:</span><span class="sxs-lookup"><span data-stu-id="30de5-120">Press 'n' at the prompt to create a **n**ew partition:</span></span>

    ```bash
    Command (m for help): n
    ```

3. <span data-ttu-id="30de5-121">Stiskněte klávesu "p" k vytvoření **p**rimární oddílu:</span><span class="sxs-lookup"><span data-stu-id="30de5-121">Next, press 'p' to create a **p**rimary partition:</span></span>

    ```bash 
    Command action
            e   extended
            p   primary partition (1-4)
    ```

4. <span data-ttu-id="30de5-122">Stiskněte klávesu '1' a vyberte číslo oddílu 1:</span><span class="sxs-lookup"><span data-stu-id="30de5-122">Press '1' to select partition number 1:</span></span>

    ```bash
    Partition number (1-4): 1
    ```

5. <span data-ttu-id="30de5-123">Vyberte výchozí bod nového oddílu, nebo klikněte na tlačítko `<enter>` přijměte výchozí nastavení pro umístění oddílu na začátku volného místa na disku:</span><span class="sxs-lookup"><span data-stu-id="30de5-123">Select the starting point of the new partition, or press `<enter>` to accept the default to place the partition at the beginning of the free space on the drive:</span></span>

    ```bash   
    First cylinder (1-1305, default 1):
    Using default value 1
    ```

6. <span data-ttu-id="30de5-124">Vyberte velikost oddílu, například typ '+10G' k vytvoření oddílu 10 GB.</span><span class="sxs-lookup"><span data-stu-id="30de5-124">Select the size of the partition, for example type '+10G' to create a 10 gigabyte partition.</span></span> <span data-ttu-id="30de5-125">Také můžete stisknout klávesu `<enter>` vytvořte jeden oddíl, který zahrnuje celou jednotku:</span><span class="sxs-lookup"><span data-stu-id="30de5-125">Or, press `<enter>` create a single partition that spans the entire drive:</span></span>

    ```bash   
    Last cylinder, +cylinders or +size{K,M,G} (1-1305, default 1305): 
    Using default value 1305
    ```

7. <span data-ttu-id="30de5-126">V dalším kroku změňte ID a **t**typ oddílu z výchozí ID '83' (Linux) ID, fd' (Linux raid automaticky):</span><span class="sxs-lookup"><span data-stu-id="30de5-126">Next, change the ID and **t**ype of the partition from the default ID '83' (Linux) to ID 'fd' (Linux raid auto):</span></span>

    ```bash  
    Command (m for help): t
    Selected partition 1
    Hex code (type L to list codes): fd
    ```

8. <span data-ttu-id="30de5-127">Nakonec tabulce oddílu zápis na disk a ukončete fdisk:</span><span class="sxs-lookup"><span data-stu-id="30de5-127">Finally, write the partition table to the drive and exit fdisk:</span></span>

    ```bash   
    Command (m for help): w
    The partition table has been altered!
    ```

## <a name="create-the-raid-array"></a><span data-ttu-id="30de5-128">Vytvoření pole RAID</span><span class="sxs-lookup"><span data-stu-id="30de5-128">Create the RAID array</span></span>
1. <span data-ttu-id="30de5-129">Následující příklad se "stripe" (úroveň pole RAID 0) tři oddíly nachází na tři samostatné datové disky (sdc1, sdd1, sde1).</span><span class="sxs-lookup"><span data-stu-id="30de5-129">The following example will "stripe" (RAID level 0) three partitions located on three separate data disks (sdc1, sdd1, sde1).</span></span>  <span data-ttu-id="30de5-130">Po spuštění tohoto příkazu nové zařízení RAID názvem **/dev/md127** je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="30de5-130">After running this command a new RAID device called **/dev/md127** is created.</span></span> <span data-ttu-id="30de5-131">Všimněte si, že pokud tyto datové disky jsme dříve součástí jiného nefunkční pole RAID může být potřeba přidat `--force` parametru `mdadm` příkaz:</span><span class="sxs-lookup"><span data-stu-id="30de5-131">Also note that if these data disks we previously part of another defunct RAID array it may be necessary to add the `--force` parameter to the `mdadm` command:</span></span>

    ```bash  
    sudo mdadm --create /dev/md127 --level 0 --raid-devices 3 \
        /dev/sdc1 /dev/sdd1 /dev/sde1
    ```

2. <span data-ttu-id="30de5-132">Vytvořit systém souborů na nové zařízení RAID</span><span class="sxs-lookup"><span data-stu-id="30de5-132">Create the file system on the new RAID device</span></span>
   
    <span data-ttu-id="30de5-133">a.</span><span class="sxs-lookup"><span data-stu-id="30de5-133">a.</span></span> <span data-ttu-id="30de5-134">**CentOS, Oracle Linux, SLES 12, openSUSE a Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="30de5-134">**CentOS, Oracle Linux, SLES 12, openSUSE, and Ubuntu**</span></span>

    ```bash   
    sudo mkfs -t ext4 /dev/md127
    ```
   
    <span data-ttu-id="30de5-135">b.</span><span class="sxs-lookup"><span data-stu-id="30de5-135">b.</span></span> <span data-ttu-id="30de5-136">**SLES 11**</span><span class="sxs-lookup"><span data-stu-id="30de5-136">**SLES 11**</span></span>

    ```bash
    sudo mkfs -t ext3 /dev/md127
    ```
   
    <span data-ttu-id="30de5-137">c.</span><span class="sxs-lookup"><span data-stu-id="30de5-137">c.</span></span> <span data-ttu-id="30de5-138">**SLES 11** – povolit boot.md a vytvořit mdadm.conf</span><span class="sxs-lookup"><span data-stu-id="30de5-138">**SLES 11** - enable boot.md and create mdadm.conf</span></span>

    ```bash
    sudo -i chkconfig --add boot.md
    sudo echo 'DEVICE /dev/sd*[0-9]' >> /etc/mdadm.conf
    ```
   
   > [!NOTE]
   > <span data-ttu-id="30de5-139">Po provedení těchto změn na systémy SUSE může být nutný restart.</span><span class="sxs-lookup"><span data-stu-id="30de5-139">A reboot may be required after making these changes on SUSE systems.</span></span> <span data-ttu-id="30de5-140">Tento krok je *není* na SLES 12 vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="30de5-140">This step is *not* required on SLES 12.</span></span>
   > 
   > 

## <a name="add-the-new-file-system-to-etcfstab"></a><span data-ttu-id="30de5-141">Přidat nový systém souborů do /etc/fstab</span><span class="sxs-lookup"><span data-stu-id="30de5-141">Add the new file system to /etc/fstab</span></span>
> [!IMPORTANT]
> <span data-ttu-id="30de5-142">Nesprávně úprav souboru /etc/fstab může mít za následek nelze spustit systém.</span><span class="sxs-lookup"><span data-stu-id="30de5-142">Improperly editing the /etc/fstab file could result in an unbootable system.</span></span> <span data-ttu-id="30de5-143">Pokud jistí, naleznete distribuční dokumentaci informace o tom, jak správně upravit tento soubor.</span><span class="sxs-lookup"><span data-stu-id="30de5-143">If unsure, refer to the distribution's documentation for information on how to properly edit this file.</span></span> <span data-ttu-id="30de5-144">Dále je doporučeno, jestli je vytvořená záloha souboru /etc/fstab před úpravou.</span><span class="sxs-lookup"><span data-stu-id="30de5-144">It is also recommended that a backup of the /etc/fstab file is created before editing.</span></span>

1. <span data-ttu-id="30de5-145">Vytvořte požadované přípojného bodu pro nový systém souborů, například:</span><span class="sxs-lookup"><span data-stu-id="30de5-145">Create the desired mount point for your new file system, for example:</span></span>

    ```bash
    sudo mkdir /data
    ```
2. <span data-ttu-id="30de5-146">Při úpravě /etc/fstab, **UUID** slouží k odkazování na systém souborů, nikoli název zařízení.</span><span class="sxs-lookup"><span data-stu-id="30de5-146">When editing /etc/fstab, the **UUID** should be used to reference the file system rather than the device name.</span></span>  <span data-ttu-id="30de5-147">Použití `blkid` nástroj k určení identifikátor UUID pro nový systém souborů:</span><span class="sxs-lookup"><span data-stu-id="30de5-147">Use the `blkid` utility to determine the UUID for the new file system:</span></span>

    ```bash   
    sudo /sbin/blkid
    ...........
    /dev/md127: UUID="aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" TYPE="ext4"
    ```

3. <span data-ttu-id="30de5-148">V textovém editoru otevřete /etc/fstab a přidejte položku pro nový systém souborů, například:</span><span class="sxs-lookup"><span data-stu-id="30de5-148">Open /etc/fstab in a text editor and add an entry for the new file system, for example:</span></span>

    ```bash   
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults  0  2
    ```
   
    <span data-ttu-id="30de5-149">Nebo na **SLES 11**:</span><span class="sxs-lookup"><span data-stu-id="30de5-149">Or on **SLES 11**:</span></span>

    ```bash
    /dev/disk/by-uuid/aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext3  defaults  0  2
    ```
   
    <span data-ttu-id="30de5-150">Potom uložte a zavřete /etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="30de5-150">Then, save and close /etc/fstab.</span></span>

4. <span data-ttu-id="30de5-151">Testovací správnost zadání fstab/etc /:</span><span class="sxs-lookup"><span data-stu-id="30de5-151">Test that the /etc/fstab entry is correct:</span></span>

    ```bash  
    sudo mount -a
    ```

    <span data-ttu-id="30de5-152">Pokud tento příkaz výsledkem chybovou zprávu, Zkontrolujte prosím syntaxe v souboru /etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="30de5-152">If this command results in an error message, please check the syntax in the /etc/fstab file.</span></span>
   
    <span data-ttu-id="30de5-153">Následně spusťte `mount` zajistěte připojení k systému souborů:</span><span class="sxs-lookup"><span data-stu-id="30de5-153">Next run the `mount` command to ensure the file system is mounted:</span></span>

    ```bash   
    mount
    .................
    /dev/md127 on /data type ext4 (rw)
    ```

5. <span data-ttu-id="30de5-154">(Volitelné) Bezporuchový spouštěcí parametry</span><span class="sxs-lookup"><span data-stu-id="30de5-154">(Optional) Failsafe Boot Parameters</span></span>
   
    <span data-ttu-id="30de5-155">**Konfigurace fstab**</span><span class="sxs-lookup"><span data-stu-id="30de5-155">**fstab configuration**</span></span>
   
    <span data-ttu-id="30de5-156">Velkém množství distribucí obsahovat buď `nobootwait` nebo `nofail` připojit parametry, které mohou být přidány do souboru/etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="30de5-156">Many distributions include either the `nobootwait` or `nofail` mount parameters that may be added to the /etc/fstab file.</span></span> <span data-ttu-id="30de5-157">Tyto parametry umožňují selhání při připojení příslušného systému souborů a povolit spuštění i v případě, že nelze správně připojit RAID systému souborů i nadále systému Linux.</span><span class="sxs-lookup"><span data-stu-id="30de5-157">These parameters allow for failures when mounting a particular file system and allow the Linux system to continue to boot even if it is unable to properly mount the RAID file system.</span></span> <span data-ttu-id="30de5-158">Vyhledejte vaší distribuční dokumentaci další informace o těchto parametrů.</span><span class="sxs-lookup"><span data-stu-id="30de5-158">Refer to your distribution's documentation for more information on these parameters.</span></span>
   
    <span data-ttu-id="30de5-159">Příklad (Ubuntu):</span><span class="sxs-lookup"><span data-stu-id="30de5-159">Example (Ubuntu):</span></span>

    ```bash  
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,nobootwait  0  2
    ```   

    <span data-ttu-id="30de5-160">**Parametry spuštění systému Linux**</span><span class="sxs-lookup"><span data-stu-id="30de5-160">**Linux boot parameters**</span></span>
   
    <span data-ttu-id="30de5-161">Kromě výše uvedených parametrů, parametr jádra "`bootdegraded=true`" Povolit systém tak, aby i v případě, že RAID je považována za poškozený nebo sníženou pro příklad, pokud datová jednotka nechtěně odebrán z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="30de5-161">In addition to the above parameters, the kernel parameter "`bootdegraded=true`" can allow the system to boot even if the RAID is perceived as damaged or degraded, for example if a data drive is inadvertently removed from the virtual machine.</span></span> <span data-ttu-id="30de5-162">Ve výchozím nastavení může také výsledkem-spouštěcího systému.</span><span class="sxs-lookup"><span data-stu-id="30de5-162">By default this could also result in a non-bootable system.</span></span>
   
    <span data-ttu-id="30de5-163">Naleznete vaší distribuční dokumentaci o tom, jak správně upravit parametry jádra.</span><span class="sxs-lookup"><span data-stu-id="30de5-163">Please refer to your distribution's documentation on how to properly edit kernel parameters.</span></span> <span data-ttu-id="30de5-164">Například ve velkém množství distribucí (CentOS, Oracle Linux SLES 11.) tyto parametry mohou být přidány ručně na "`/boot/grub/menu.lst`" soubor.</span><span class="sxs-lookup"><span data-stu-id="30de5-164">For example, in many distributions (CentOS, Oracle Linux, SLES 11) these parameters may be added manually to the "`/boot/grub/menu.lst`" file.</span></span>  <span data-ttu-id="30de5-165">Na Ubuntu tento parametr lze přidat do `GRUB_CMDLINE_LINUX_DEFAULT` proměnné na "/ etc/výchozí/grub".</span><span class="sxs-lookup"><span data-stu-id="30de5-165">On Ubuntu this parameter can be added to the `GRUB_CMDLINE_LINUX_DEFAULT` variable on "/etc/default/grub".</span></span>


## <a name="trimunmap-support"></a><span data-ttu-id="30de5-166">Podpora uvolnění dočasné paměti nebo UNMAP</span><span class="sxs-lookup"><span data-stu-id="30de5-166">TRIM/UNMAP support</span></span>
<span data-ttu-id="30de5-167">Některé Linux jádra podporovat operace TRIM/UNMAP vyřadí nepoužívané bloky na disku.</span><span class="sxs-lookup"><span data-stu-id="30de5-167">Some Linux kernels support TRIM/UNMAP operations to discard unused blocks on the disk.</span></span> <span data-ttu-id="30de5-168">Tyto operace jsou užitečné hlavně v standardní úložiště k informování Azure, které odstraněné stránky již nejsou platné a může být vymazány.</span><span class="sxs-lookup"><span data-stu-id="30de5-168">These operations are primarily useful in standard storage to inform Azure that deleted pages are no longer valid and can be discarded.</span></span> <span data-ttu-id="30de5-169">Zahození stránky můžete uložit náklady, pokud chcete vytvořit velkých souborů a pak odstraňte je.</span><span class="sxs-lookup"><span data-stu-id="30de5-169">Discarding pages can save cost if you create large files and then delete them.</span></span>

> [!NOTE]
> <span data-ttu-id="30de5-170">RAID nemusí zadávání příkazů zahození, velikost bloku pro toto pole je nastaven na hodnotu menší než výchozí (512 KB).</span><span class="sxs-lookup"><span data-stu-id="30de5-170">RAID may not issue discard commands if the chunk size for the array is set to less than the default (512KB).</span></span> <span data-ttu-id="30de5-171">Je to proto členitost unmap na hostiteli je také 512KB.</span><span class="sxs-lookup"><span data-stu-id="30de5-171">This is because the unmap granularity on the Host is also 512KB.</span></span> <span data-ttu-id="30de5-172">Pokud jste změnili velikost bloku pole prostřednictvím na mdadm `--chunk=` parametr a potom zrušit mapování/TRIM žádosti může být ignorován jádrem.</span><span class="sxs-lookup"><span data-stu-id="30de5-172">If you modified the array's chunk size via mdadm's `--chunk=` parameter, then TRIM/unmap requests may be ignored by the kernel.</span></span>

<span data-ttu-id="30de5-173">Existují dva způsoby, jak povolit TRIM podporují ve virtuálním počítačům s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="30de5-173">There are two ways to enable TRIM support in your Linux VM.</span></span> <span data-ttu-id="30de5-174">Obvyklým způsobem podívejte se distribuční o doporučený postup:</span><span class="sxs-lookup"><span data-stu-id="30de5-174">As usual, consult your distribution for the recommended approach:</span></span>

- <span data-ttu-id="30de5-175">Použití `discard` připojit možnost v `/etc/fstab`, například:</span><span class="sxs-lookup"><span data-stu-id="30de5-175">Use the `discard` mount option in `/etc/fstab`, for example:</span></span>

    ```bash
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,discard  0  2
    ```

- <span data-ttu-id="30de5-176">V některých případech `discard` možnost může mít vliv na výkon.</span><span class="sxs-lookup"><span data-stu-id="30de5-176">In some cases the `discard` option may have performance implications.</span></span> <span data-ttu-id="30de5-177">Alternativně můžete spustit `fstrim` ručně příkaz z příkazového řádku, nebo ho přidat do vaší crontab pravidelně spouštět:</span><span class="sxs-lookup"><span data-stu-id="30de5-177">Alternatively, you can run the `fstrim` command manually from the command line, or add it to your crontab to run regularly:</span></span>

    <span data-ttu-id="30de5-178">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="30de5-178">**Ubuntu**</span></span>

    ```bash
    # sudo apt-get install util-linux
    # sudo fstrim /data
    ```

    <span data-ttu-id="30de5-179">**RHEL nebo CentOS**</span><span class="sxs-lookup"><span data-stu-id="30de5-179">**RHEL/CentOS**</span></span>
    ```bash
    # sudo yum install util-linux
    # sudo fstrim /data
    ```
