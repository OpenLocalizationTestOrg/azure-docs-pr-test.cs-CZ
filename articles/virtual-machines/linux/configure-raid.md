---
title: "aaaConfigure softwaru diskového pole RAID na virtuálním počítači se systémem Linux | Microsoft Docs"
description: "Zjistěte, jak toouse mdadm tooconfigure RAID v systému Linux v Azure."
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
ms.openlocfilehash: f06e2679d953faf88ffee9991226cdb3cc1cbdb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-software-raid-on-linux"></a><span data-ttu-id="158fa-103">Konfigurace softwarového pole RAID v Linuxu</span><span class="sxs-lookup"><span data-stu-id="158fa-103">Configure Software RAID on Linux</span></span>
<span data-ttu-id="158fa-104">Jedná se o běžné software toouse scénář RAID na virtuální počítače s Linuxem v Azure toopresent dat více připojené disky jako jedno zařízení RAID.</span><span class="sxs-lookup"><span data-stu-id="158fa-104">It's a common scenario toouse software RAID on Linux virtual machines in Azure toopresent multiple attached data disks as a single RAID device.</span></span> <span data-ttu-id="158fa-105">Obvykle to být použité tooimprove výkonu a povolit pro zlepšila propustnost porovnání toousing právě jeden disk.</span><span class="sxs-lookup"><span data-stu-id="158fa-105">Typically this can be used tooimprove performance and allow for improved throughput compared toousing just a single disk.</span></span>

## <a name="attaching-data-disks"></a><span data-ttu-id="158fa-106">Připojení datových disků</span><span class="sxs-lookup"><span data-stu-id="158fa-106">Attaching data disks</span></span>
<span data-ttu-id="158fa-107">Minimálně dva prázdné datové disky jsou potřebné tooconfigure zařízení RAID.</span><span class="sxs-lookup"><span data-stu-id="158fa-107">Two or more empty data disks are needed tooconfigure a RAID device.</span></span>  <span data-ttu-id="158fa-108">Hello primární důvod pro vytváření diskového pole RAID zařízení je tooimprove výkon disku vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="158fa-108">hello primary reason for creating a RAID device is tooimprove performance of your disk IO.</span></span>  <span data-ttu-id="158fa-109">V závislosti na vašich potřebách vstupně-výstupní operace, můžete vybrat tooattach disky, které jsou uložené v našich standardní úložiště s až too500 vstupně-výstupní operace/ps na disk nebo naše storage úrovně Premium s až too5000 vstupně-výstupní operace/ps na disk.</span><span class="sxs-lookup"><span data-stu-id="158fa-109">Based on your IO needs, you can choose tooattach disks that are stored in our Standard Storage, with up too500 IO/ps per disk or our Premium storage with up too5000 IO/ps per disk.</span></span> <span data-ttu-id="158fa-110">Tento článek nezabývá podrobnosti o tom, tooprovision a připojte datové disky tooa Linux virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="158fa-110">This article does not go into detail on how tooprovision and attach data disks tooa Linux virtual machine.</span></span>  <span data-ttu-id="158fa-111">Najdete v článku Microsoft Azure hello [připojit disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) pro podrobné pokyny, jak tooattach prázdný datový disk tooa Linux virtuálního počítače na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="158fa-111">See hello Microsoft Azure article [attach a disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for detailed instructions on how tooattach an empty data disk tooa Linux virtual machine on Azure.</span></span>

## <a name="install-hello-mdadm-utility"></a><span data-ttu-id="158fa-112">Nainstalujte nástroj mdadm hello</span><span class="sxs-lookup"><span data-stu-id="158fa-112">Install hello mdadm utility</span></span>
* <span data-ttu-id="158fa-113">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="158fa-113">**Ubuntu**</span></span>
```bash
sudo apt-get update
sudo apt-get install mdadm
```

* <span data-ttu-id="158fa-114">**CentOS & Oracle Linux**</span><span class="sxs-lookup"><span data-stu-id="158fa-114">**CentOS & Oracle Linux**</span></span>
```bash
sudo yum install mdadm
```

* <span data-ttu-id="158fa-115">**SLES a openSUSE**</span><span class="sxs-lookup"><span data-stu-id="158fa-115">**SLES and openSUSE**</span></span>
```bash  
zypper install mdadm
```

## <a name="create-hello-disk-partitions"></a><span data-ttu-id="158fa-116">Vytvoření oddílů disku hello</span><span class="sxs-lookup"><span data-stu-id="158fa-116">Create hello disk partitions</span></span>
<span data-ttu-id="158fa-117">V tomto příkladu vytvoříme na /dev/sdc oddíl jediný disk.</span><span class="sxs-lookup"><span data-stu-id="158fa-117">In this example, we create a single disk partition on /dev/sdc.</span></span> <span data-ttu-id="158fa-118">Hello nový diskový oddíl bude volána /dev/sdc1.</span><span class="sxs-lookup"><span data-stu-id="158fa-118">hello new disk partition will be called /dev/sdc1.</span></span>

1. <span data-ttu-id="158fa-119">Spustit `fdisk` toobegin vytváření oddílů</span><span class="sxs-lookup"><span data-stu-id="158fa-119">Start `fdisk` toobegin creating partitions</span></span>

    ```bash
    sudo fdisk /dev/sdc
    Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
    Building a new DOS disklabel with disk identifier 0xa34cb70c.
    Changes will remain in memory only, until you decide toowrite them.
    After that, of course, hello previous content won't be recoverable.

    WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
                    switch off hello mode (command 'c') and change display units to
                    sectors (command 'u').
    ```

2. <span data-ttu-id="158fa-120">Stiskněte klávesu n na výzvy toocreate hello  **n** ové oddílu:</span><span class="sxs-lookup"><span data-stu-id="158fa-120">Press 'n' at hello prompt toocreate a **n**ew partition:</span></span>

    ```bash
    Command (m for help): n
    ```

3. <span data-ttu-id="158fa-121">Stiskněte klávesu toocreate "p" **p**rimární oddílu:</span><span class="sxs-lookup"><span data-stu-id="158fa-121">Next, press 'p' toocreate a **p**rimary partition:</span></span>

    ```bash 
    Command action
            e   extended
            p   primary partition (1-4)
    ```

4. <span data-ttu-id="158fa-122">Stiskněte číslo oddílu '1' tooselect 1:</span><span class="sxs-lookup"><span data-stu-id="158fa-122">Press '1' tooselect partition number 1:</span></span>

    ```bash
    Partition number (1-4): 1
    ```

5. <span data-ttu-id="158fa-123">Vyberte hello výchozí bod hello nový oddíl, nebo klikněte na tlačítko `<enter>` tooaccept hello výchozí tooplace hello oddílu na začátku hello hello volné místo na jednotce hello:</span><span class="sxs-lookup"><span data-stu-id="158fa-123">Select hello starting point of hello new partition, or press `<enter>` tooaccept hello default tooplace hello partition at hello beginning of hello free space on hello drive:</span></span>

    ```bash   
    First cylinder (1-1305, default 1):
    Using default value 1
    ```

6. <span data-ttu-id="158fa-124">Vyberte velikost hello hello oddílu, například typ '+10G' toocreate oddíl 10 GB.</span><span class="sxs-lookup"><span data-stu-id="158fa-124">Select hello size of hello partition, for example type '+10G' toocreate a 10 gigabyte partition.</span></span> <span data-ttu-id="158fa-125">Také můžete stisknout klávesu `<enter>` vytvořte jeden oddíl, který zahrnuje celou jednotku hello:</span><span class="sxs-lookup"><span data-stu-id="158fa-125">Or, press `<enter>` create a single partition that spans hello entire drive:</span></span>

    ```bash   
    Last cylinder, +cylinders or +size{K,M,G} (1-1305, default 1305): 
    Using default value 1305
    ```

7. <span data-ttu-id="158fa-126">V dalším kroku změnit hello ID a **t**typ oddílu hello z výchozí hello ID '83' tooID (Linux), fd' (Linux raid automaticky):</span><span class="sxs-lookup"><span data-stu-id="158fa-126">Next, change hello ID and **t**ype of hello partition from hello default ID '83' (Linux) tooID 'fd' (Linux raid auto):</span></span>

    ```bash  
    Command (m for help): t
    Selected partition 1
    Hex code (type L toolist codes): fd
    ```

8. <span data-ttu-id="158fa-127">Nakonec zápisu hello oddílu tabulky toohello jednotky a ukončete fdisk:</span><span class="sxs-lookup"><span data-stu-id="158fa-127">Finally, write hello partition table toohello drive and exit fdisk:</span></span>

    ```bash   
    Command (m for help): w
    hello partition table has been altered!
    ```

## <a name="create-hello-raid-array"></a><span data-ttu-id="158fa-128">Vytvořit hello typu</span><span class="sxs-lookup"><span data-stu-id="158fa-128">Create hello RAID array</span></span>
1. <span data-ttu-id="158fa-129">Následující příklad se "stripe" (úroveň pole RAID 0) tři oddíly nachází na tři samostatné datové disky (sdc1, sdd1, sde1) Hello.</span><span class="sxs-lookup"><span data-stu-id="158fa-129">hello following example will "stripe" (RAID level 0) three partitions located on three separate data disks (sdc1, sdd1, sde1).</span></span>  <span data-ttu-id="158fa-130">Po spuštění tohoto příkazu nové zařízení RAID názvem **/dev/md127** je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="158fa-130">After running this command a new RAID device called **/dev/md127** is created.</span></span> <span data-ttu-id="158fa-131">Všimněte si, že pokud tyto datové disky jsme dříve součástí jiného nefunkční pole RAID může být nutné tooadd hello `--force` parametr toohello `mdadm` příkaz:</span><span class="sxs-lookup"><span data-stu-id="158fa-131">Also note that if these data disks we previously part of another defunct RAID array it may be necessary tooadd hello `--force` parameter toohello `mdadm` command:</span></span>

    ```bash  
    sudo mdadm --create /dev/md127 --level 0 --raid-devices 3 \
        /dev/sdc1 /dev/sdd1 /dev/sde1
    ```

2. <span data-ttu-id="158fa-132">Vytvořit systém souborů hello na nové zařízení RAID hello</span><span class="sxs-lookup"><span data-stu-id="158fa-132">Create hello file system on hello new RAID device</span></span>
   
    <span data-ttu-id="158fa-133">a.</span><span class="sxs-lookup"><span data-stu-id="158fa-133">a.</span></span> <span data-ttu-id="158fa-134">**CentOS, Oracle Linux, SLES 12, openSUSE a Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="158fa-134">**CentOS, Oracle Linux, SLES 12, openSUSE, and Ubuntu**</span></span>

    ```bash   
    sudo mkfs -t ext4 /dev/md127
    ```
   
    <span data-ttu-id="158fa-135">b.</span><span class="sxs-lookup"><span data-stu-id="158fa-135">b.</span></span> <span data-ttu-id="158fa-136">**SLES 11**</span><span class="sxs-lookup"><span data-stu-id="158fa-136">**SLES 11**</span></span>

    ```bash
    sudo mkfs -t ext3 /dev/md127
    ```
   
    <span data-ttu-id="158fa-137">c.</span><span class="sxs-lookup"><span data-stu-id="158fa-137">c.</span></span> <span data-ttu-id="158fa-138">**SLES 11** – povolit boot.md a vytvořit mdadm.conf</span><span class="sxs-lookup"><span data-stu-id="158fa-138">**SLES 11** - enable boot.md and create mdadm.conf</span></span>

    ```bash
    sudo -i chkconfig --add boot.md
    sudo echo 'DEVICE /dev/sd*[0-9]' >> /etc/mdadm.conf
    ```
   
   > [!NOTE]
   > <span data-ttu-id="158fa-139">Po provedení těchto změn na systémy SUSE může být nutný restart.</span><span class="sxs-lookup"><span data-stu-id="158fa-139">A reboot may be required after making these changes on SUSE systems.</span></span> <span data-ttu-id="158fa-140">Tento krok je *není* na SLES 12 vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="158fa-140">This step is *not* required on SLES 12.</span></span>
   > 
   > 

## <a name="add-hello-new-file-system-tooetcfstab"></a><span data-ttu-id="158fa-141">Přidat hello nový soubor systému příliš/atd/fstab</span><span class="sxs-lookup"><span data-stu-id="158fa-141">Add hello new file system too/etc/fstab</span></span>
> [!IMPORTANT]
> <span data-ttu-id="158fa-142">Nesprávně úprav hello /etc/fstab souboru může mít za následek nelze spustit systém.</span><span class="sxs-lookup"><span data-stu-id="158fa-142">Improperly editing hello /etc/fstab file could result in an unbootable system.</span></span> <span data-ttu-id="158fa-143">Pokud si jisti, naleznete v dokumentaci toohello distribuční informace o tom, jak upravit tooproperly tento soubor.</span><span class="sxs-lookup"><span data-stu-id="158fa-143">If unsure, refer toohello distribution's documentation for information on how tooproperly edit this file.</span></span> <span data-ttu-id="158fa-144">Dále je doporučeno, jestli je vytvořená záloha souboru /etc/fstab hello před úpravou.</span><span class="sxs-lookup"><span data-stu-id="158fa-144">It is also recommended that a backup of hello /etc/fstab file is created before editing.</span></span>

1. <span data-ttu-id="158fa-145">Vytvořte hello potřeby přípojný bod pro nový systém souborů, například:</span><span class="sxs-lookup"><span data-stu-id="158fa-145">Create hello desired mount point for your new file system, for example:</span></span>

    ```bash
    sudo mkdir /data
    ```
2. <span data-ttu-id="158fa-146">Při úpravě /etc/fstab, hello **UUID** by měla být použité tooreference hello systému, nikoli hello zařízení název souboru.</span><span class="sxs-lookup"><span data-stu-id="158fa-146">When editing /etc/fstab, hello **UUID** should be used tooreference hello file system rather than hello device name.</span></span>  <span data-ttu-id="158fa-147">Použití hello `blkid` nástroj toodetermine hello UUID pro hello nový systém souborů:</span><span class="sxs-lookup"><span data-stu-id="158fa-147">Use hello `blkid` utility toodetermine hello UUID for hello new file system:</span></span>

    ```bash   
    sudo /sbin/blkid
    ...........
    /dev/md127: UUID="aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" TYPE="ext4"
    ```

3. <span data-ttu-id="158fa-148">V textovém editoru otevřete /etc/fstab a přidejte položku pro hello nový systém souborů, například:</span><span class="sxs-lookup"><span data-stu-id="158fa-148">Open /etc/fstab in a text editor and add an entry for hello new file system, for example:</span></span>

    ```bash   
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults  0  2
    ```
   
    <span data-ttu-id="158fa-149">Nebo na **SLES 11**:</span><span class="sxs-lookup"><span data-stu-id="158fa-149">Or on **SLES 11**:</span></span>

    ```bash
    /dev/disk/by-uuid/aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext3  defaults  0  2
    ```
   
    <span data-ttu-id="158fa-150">Potom uložte a zavřete /etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="158fa-150">Then, save and close /etc/fstab.</span></span>

4. <span data-ttu-id="158fa-151">Test této hello etc/správnost zadání fstab:</span><span class="sxs-lookup"><span data-stu-id="158fa-151">Test that hello /etc/fstab entry is correct:</span></span>

    ```bash  
    sudo mount -a
    ```

    <span data-ttu-id="158fa-152">Pokud tento příkaz výsledkem chybovou zprávu, Zkontrolujte prosím hello syntaxe v souboru /etc/fstab hello.</span><span class="sxs-lookup"><span data-stu-id="158fa-152">If this command results in an error message, please check hello syntax in hello /etc/fstab file.</span></span>
   
    <span data-ttu-id="158fa-153">Následně spusťte hello `mount` připojení k systému souborů hello tooensure příkaz:</span><span class="sxs-lookup"><span data-stu-id="158fa-153">Next run hello `mount` command tooensure hello file system is mounted:</span></span>

    ```bash   
    mount
    .................
    /dev/md127 on /data type ext4 (rw)
    ```

5. <span data-ttu-id="158fa-154">(Volitelné) Bezporuchový spouštěcí parametry</span><span class="sxs-lookup"><span data-stu-id="158fa-154">(Optional) Failsafe Boot Parameters</span></span>
   
    <span data-ttu-id="158fa-155">**Konfigurace fstab**</span><span class="sxs-lookup"><span data-stu-id="158fa-155">**fstab configuration**</span></span>
   
    <span data-ttu-id="158fa-156">Velkém množství distribucí obsahovat buď hello `nobootwait` nebo `nofail` připojit parametry, které mohou být přidány toohello/etc/fstab souboru.</span><span class="sxs-lookup"><span data-stu-id="158fa-156">Many distributions include either hello `nobootwait` or `nofail` mount parameters that may be added toohello /etc/fstab file.</span></span> <span data-ttu-id="158fa-157">Tyto parametry umožňují selhání při připojení příslušného systému souborů a povolit tooboot toocontinue systému Linux hello, i když je systém souborů nelze tooproperly připojení hello RAID.</span><span class="sxs-lookup"><span data-stu-id="158fa-157">These parameters allow for failures when mounting a particular file system and allow hello Linux system toocontinue tooboot even if it is unable tooproperly mount hello RAID file system.</span></span> <span data-ttu-id="158fa-158">Další informace o těchto parametrů naleznete v dokumentaci tooyour distribuce.</span><span class="sxs-lookup"><span data-stu-id="158fa-158">Refer tooyour distribution's documentation for more information on these parameters.</span></span>
   
    <span data-ttu-id="158fa-159">Příklad (Ubuntu):</span><span class="sxs-lookup"><span data-stu-id="158fa-159">Example (Ubuntu):</span></span>

    ```bash  
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,nobootwait  0  2
    ```   

    <span data-ttu-id="158fa-160">**Parametry spuštění systému Linux**</span><span class="sxs-lookup"><span data-stu-id="158fa-160">**Linux boot parameters**</span></span>
   
    <span data-ttu-id="158fa-161">V přidání toohello výše parametry, hello jádra parametr "`bootdegraded=true`" Povolit hello systému tooboot i v případě hello RAID je považována za poškozený nebo ke snížení, například pokud datová jednotka je neúmyslně odebere z hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="158fa-161">In addition toohello above parameters, hello kernel parameter "`bootdegraded=true`" can allow hello system tooboot even if hello RAID is perceived as damaged or degraded, for example if a data drive is inadvertently removed from hello virtual machine.</span></span> <span data-ttu-id="158fa-162">Ve výchozím nastavení může také výsledkem-spouštěcího systému.</span><span class="sxs-lookup"><span data-stu-id="158fa-162">By default this could also result in a non-bootable system.</span></span>
   
    <span data-ttu-id="158fa-163">Naleznete v dokumentaci tooyour distribuční na tom, jak tooproperly upravit parametry jádra.</span><span class="sxs-lookup"><span data-stu-id="158fa-163">Please refer tooyour distribution's documentation on how tooproperly edit kernel parameters.</span></span> <span data-ttu-id="158fa-164">Například ve velkém množství distribucí (CentOS, Oracle Linux SLES 11.) tyto parametry mohou být přidány ručně toohello "`/boot/grub/menu.lst`" soubor.</span><span class="sxs-lookup"><span data-stu-id="158fa-164">For example, in many distributions (CentOS, Oracle Linux, SLES 11) these parameters may be added manually toohello "`/boot/grub/menu.lst`" file.</span></span>  <span data-ttu-id="158fa-165">Na Ubuntu tento parametr je možné přidat toohello `GRUB_CMDLINE_LINUX_DEFAULT` proměnné na "/ etc/výchozí/grub".</span><span class="sxs-lookup"><span data-stu-id="158fa-165">On Ubuntu this parameter can be added toohello `GRUB_CMDLINE_LINUX_DEFAULT` variable on "/etc/default/grub".</span></span>


## <a name="trimunmap-support"></a><span data-ttu-id="158fa-166">Podpora uvolnění dočasné paměti nebo UNMAP</span><span class="sxs-lookup"><span data-stu-id="158fa-166">TRIM/UNMAP support</span></span>
<span data-ttu-id="158fa-167">Některé jádra Linux podporují toodiscard operace TRIM/UNMAP nepoužívané bloky na disku hello.</span><span class="sxs-lookup"><span data-stu-id="158fa-167">Some Linux kernels support TRIM/UNMAP operations toodiscard unused blocks on hello disk.</span></span> <span data-ttu-id="158fa-168">Tyto operace jsou užitečné hlavně v tooinform standardní úložiště Azure, které odstraněné stránky již nejsou platné a může být vymazány.</span><span class="sxs-lookup"><span data-stu-id="158fa-168">These operations are primarily useful in standard storage tooinform Azure that deleted pages are no longer valid and can be discarded.</span></span> <span data-ttu-id="158fa-169">Zahození stránky můžete uložit náklady, pokud chcete vytvořit velkých souborů a pak odstraňte je.</span><span class="sxs-lookup"><span data-stu-id="158fa-169">Discarding pages can save cost if you create large files and then delete them.</span></span>

> [!NOTE]
> <span data-ttu-id="158fa-170">RAID nemusí zadávání příkazů zahození, velikost bloku hello hello pole je nastaven tooless než výchozí hello (512 KB).</span><span class="sxs-lookup"><span data-stu-id="158fa-170">RAID may not issue discard commands if hello chunk size for hello array is set tooless than hello default (512KB).</span></span> <span data-ttu-id="158fa-171">Důvodem je, že zrušit mapování hello členitosti na hello hostitele je také 512KB.</span><span class="sxs-lookup"><span data-stu-id="158fa-171">This is because hello unmap granularity on hello Host is also 512KB.</span></span> <span data-ttu-id="158fa-172">Pokud jste změnili velikost bloku hello pole prostřednictvím na mdadm `--chunk=` parametr a potom zrušit mapování/TRIM požadavky může být ignorována hello jádra.</span><span class="sxs-lookup"><span data-stu-id="158fa-172">If you modified hello array's chunk size via mdadm's `--chunk=` parameter, then TRIM/unmap requests may be ignored by hello kernel.</span></span>

<span data-ttu-id="158fa-173">Existují dva způsoby tooenable TRIM podporují ve virtuálním počítačům s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="158fa-173">There are two ways tooenable TRIM support in your Linux VM.</span></span> <span data-ttu-id="158fa-174">Obvyklým způsobem podívejte se distribuční hello doporučenému přístupu:</span><span class="sxs-lookup"><span data-stu-id="158fa-174">As usual, consult your distribution for hello recommended approach:</span></span>

- <span data-ttu-id="158fa-175">Použití hello `discard` připojit možnost v `/etc/fstab`, například:</span><span class="sxs-lookup"><span data-stu-id="158fa-175">Use hello `discard` mount option in `/etc/fstab`, for example:</span></span>

    ```bash
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,discard  0  2
    ```

- <span data-ttu-id="158fa-176">V některých případech hello `discard` možnost může mít vliv na výkon.</span><span class="sxs-lookup"><span data-stu-id="158fa-176">In some cases hello `discard` option may have performance implications.</span></span> <span data-ttu-id="158fa-177">Alternativně můžete spustit hello `fstrim` ručně příkaz z příkazového řádku hello, nebo ho přidat tooyour crontab toorun pravidelně:</span><span class="sxs-lookup"><span data-stu-id="158fa-177">Alternatively, you can run hello `fstrim` command manually from hello command line, or add it tooyour crontab toorun regularly:</span></span>

    <span data-ttu-id="158fa-178">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="158fa-178">**Ubuntu**</span></span>

    ```bash
    # sudo apt-get install util-linux
    # sudo fstrim /data
    ```

    <span data-ttu-id="158fa-179">**RHEL nebo CentOS**</span><span class="sxs-lookup"><span data-stu-id="158fa-179">**RHEL/CentOS**</span></span>
    ```bash
    # sudo yum install util-linux
    # sudo fstrim /data
    ```
