---
title: "aaaConfigure LVM na virtuálním počítači se systémem Linux | Microsoft Docs"
description: "Zjistěte, jak tooconfigure LVM v systému Linux v Azure."
services: virtual-machines-linux
documentationcenter: na
author: szarkos
manager: timlt
editor: tysonn
tag: azure-service-management,azure-resource-manager
ms.assetid: 7f533725-1484-479d-9472-6b3098d0aecc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 8daf792d87c6bb3d91a2eddcd01cfab34fd28cff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-lvm-on-a-linux-vm-in-azure"></a><span data-ttu-id="b9c0a-103">Konfigurace LVM na virtuální počítač s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="b9c0a-103">Configure LVM on a Linux VM in Azure</span></span>
<span data-ttu-id="b9c0a-104">Tento dokument popisuje jak tooconfigure Manager logické svazek (LVM) ve virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-104">This document will discuss how tooconfigure Logical Volume Manager (LVM) in your Azure virtual machine.</span></span> <span data-ttu-id="b9c0a-105">I když je to vhodné tooconfigure LVM na každý disk připojen toohello virtuálního počítače, ve výchozím nastavení většina cloudové Image nebude mít LVM nakonfigurované na hello disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-105">While it is feasible tooconfigure LVM on any disk attached toohello virtual machine, by default most cloud images will not have LVM configured on hello OS disk.</span></span> <span data-ttu-id="b9c0a-106">To je tooprevent problémy se skupinami duplicitní svazku, pokud hello disk s operačním systémem je někdy připojena tooanother virtuální počítač hello stejné distribuce a typ, tj. během na scénář zotavení.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-106">This is tooprevent problems with duplicate volume groups if hello OS disk is ever attached tooanother VM of hello same distribution and type, i.e. during a recovery scenario.</span></span> <span data-ttu-id="b9c0a-107">Proto se doporučuje jenom toouse LVM na hello datových disků.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-107">Therefore it is recommended only toouse LVM on hello data disks.</span></span>

## <a name="linear-vs-striped-logical-volumes"></a><span data-ttu-id="b9c0a-108">Lineární oproti logické prokládané svazky</span><span class="sxs-lookup"><span data-stu-id="b9c0a-108">Linear vs. striped logical volumes</span></span>
<span data-ttu-id="b9c0a-109">LVM lze použít toocombine počet fyzických disků do do jednoho svazku úložiště.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-109">LVM can be used toocombine a number of physical disks into a single storage volume.</span></span> <span data-ttu-id="b9c0a-110">Ve výchozím nastavení LVM obvykle vytvoří lineární logické svazcích, což znamená, že hello fyzického úložiště je zřetězen společně.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-110">By default LVM will usually create linear logical volumes, which means that hello physical storage is concatenated together.</span></span> <span data-ttu-id="b9c0a-111">V takovém případě operace čtení a zápisu obvykle pouze odešle tooa jediný disk.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-111">In this case read/write operations will typically only be sent tooa single disk.</span></span> <span data-ttu-id="b9c0a-112">Naproti tomu můžeme také prokládané svazky lze vytvářet logické kde čtení a zápisu jsou distribuované toomultiple disky patřící do skupiny svazku hello (tj. podobné tooRAID0).</span><span class="sxs-lookup"><span data-stu-id="b9c0a-112">In contrast, we can also create striped logical volumes where reads and writes are distributed toomultiple disks contained in hello volume group (i.e. similar tooRAID0).</span></span> <span data-ttu-id="b9c0a-113">Z důvodů výkonu, pravděpodobně budete chtít toostripe logické svazků tak, aby čtení a zápisy využívat všechny disky připojené data.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-113">For performance reasons it is likely you will want toostripe your logical volumes so that reads and writes utilize all your attached data disks.</span></span>

<span data-ttu-id="b9c0a-114">Tento dokument se popisují, jak toocombine několik datové disky do jednoho svazku skupiny a pak vytvořte Prokládané logické svazku.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-114">This document will describe how toocombine several data disks into a single volume group, and then create a striped logical volume.</span></span> <span data-ttu-id="b9c0a-115">Hello kroky jsou poněkud zobecněný toowork s většina distribuce.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-115">hello steps below are somewhat generalized toowork with most distributions.</span></span> <span data-ttu-id="b9c0a-116">Ve většině případů hello nástroje a pracovní postupy pro správu LVM v Azure nejsou zásadně liší od jiných prostředích.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-116">In most cases hello utilities and workflows for managing LVM on Azure are not fundamentally different than other environments.</span></span> <span data-ttu-id="b9c0a-117">Obvyklým způsobem také najdete dodavatele Linux dokumentace a osvědčené postupy pro použití s konkrétní distribuční LVM.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-117">As usual, please also consult your Linux vendor for documentation and best practices for using LVM with your particular distribution.</span></span>

## <a name="attaching-data-disks"></a><span data-ttu-id="b9c0a-118">Připojení datových disků</span><span class="sxs-lookup"><span data-stu-id="b9c0a-118">Attaching data disks</span></span>
<span data-ttu-id="b9c0a-119">Obvykle jednu bude chcete toostart dvě nebo více disků prázdný data při použití LVM.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-119">One will usually want toostart with two or more empty data disks when using LVM.</span></span> <span data-ttu-id="b9c0a-120">V závislosti na vašich potřebách vstupně-výstupní operace, můžete vybrat tooattach disky, které jsou uložené v našich standardní úložiště s až too500 vstupně-výstupní operace/ps na disk nebo naše storage úrovně Premium s až too5000 vstupně-výstupní operace/ps na disk.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-120">Based on your IO needs, you can choose tooattach disks that are stored in our Standard Storage, with up too500 IO/ps per disk or our Premium storage with up too5000 IO/ps per disk.</span></span> <span data-ttu-id="b9c0a-121">Tento článek nepřejde do podrobnosti o tom, tooprovision a připojte datové disky tooa Linux virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-121">This article will not go into detail on how tooprovision and attach data disks tooa Linux virtual machine.</span></span> <span data-ttu-id="b9c0a-122">Naleznete v článku Microsoft Azure hello [připojit disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) pro podrobné pokyny, jak tooattach prázdný datový disk tooa Linux virtuálního počítače na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-122">Please see hello Microsoft Azure article [attach a disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for detailed instructions on how tooattach an empty data disk tooa Linux virtual machine on Azure.</span></span>

## <a name="install-hello-lvm-utilities"></a><span data-ttu-id="b9c0a-123">Nainstalujte nástroje LVM hello</span><span class="sxs-lookup"><span data-stu-id="b9c0a-123">Install hello LVM utilities</span></span>
* <span data-ttu-id="b9c0a-124">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="b9c0a-124">**Ubuntu**</span></span>

    ```bash  
    sudo apt-get update
    sudo apt-get install lvm2
    ```

* <span data-ttu-id="b9c0a-125">**RHEL, CentOS a Oracle Linux**</span><span class="sxs-lookup"><span data-stu-id="b9c0a-125">**RHEL, CentOS & Oracle Linux**</span></span>

    ```bash   
    sudo yum install lvm2
    ```

* <span data-ttu-id="b9c0a-126">**SLES 12 a openSUSE**</span><span class="sxs-lookup"><span data-stu-id="b9c0a-126">**SLES 12 and openSUSE**</span></span>

    ```bash   
    sudo zypper install lvm2
    ```

* <span data-ttu-id="b9c0a-127">**SLES 11**</span><span class="sxs-lookup"><span data-stu-id="b9c0a-127">**SLES 11**</span></span>

    ```bash   
    sudo zypper install lvm2
    ```

    <span data-ttu-id="b9c0a-128">Na SLES11 je nutné upravit také `/etc/sysconfig/lvm` a nastavte `LVM_ACTIVATED_ON_DISCOVERED` příliš "Povolit":</span><span class="sxs-lookup"><span data-stu-id="b9c0a-128">On SLES11 you must also edit `/etc/sysconfig/lvm` and set `LVM_ACTIVATED_ON_DISCOVERED` too"enable":</span></span>

    ```sh   
    LVM_ACTIVATED_ON_DISCOVERED="enable" 
    ```

## <a name="configure-lvm"></a><span data-ttu-id="b9c0a-129">Konfigurace LVM</span><span class="sxs-lookup"><span data-stu-id="b9c0a-129">Configure LVM</span></span>
<span data-ttu-id="b9c0a-130">V tomto průvodci budeme předpokládat připojeny tři datových disků, které budete označujeme tooas `/dev/sdc`, `/dev/sdd` a `/dev/sde`.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-130">In this guide we will assume you have attached three data disks, which we'll refer tooas `/dev/sdc`, `/dev/sdd` and `/dev/sde`.</span></span> <span data-ttu-id="b9c0a-131">Poznámka: tyto nesmí být vždy možné hello stejné názvy cest v virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-131">Note that these may not always be hello same path names in your VM.</span></span> <span data-ttu-id="b9c0a-132">Můžete spustit '`sudo fdisk -l`' nebo podobné toolist příkaz dostupné disky.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-132">You can run '`sudo fdisk -l`' or similar command toolist your available disks.</span></span>

1. <span data-ttu-id="b9c0a-133">Příprava fyzických svazků hello:</span><span class="sxs-lookup"><span data-stu-id="b9c0a-133">Prepare hello physical volumes:</span></span>

    ```bash    
    sudo pvcreate /dev/sd[cde]
    Physical volume "/dev/sdc" successfully created
    Physical volume "/dev/sdd" successfully created
    Physical volume "/dev/sde" successfully created
    ```

2. <span data-ttu-id="b9c0a-134">Vytvořte skupinu svazku.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-134">Create a volume group.</span></span> <span data-ttu-id="b9c0a-135">V tomto příkladu voláme hello svazku skupiny `data-vg01`:</span><span class="sxs-lookup"><span data-stu-id="b9c0a-135">In this example we are calling hello volume group `data-vg01`:</span></span>

    ```bash    
    sudo vgcreate data-vg01 /dev/sd[cde]
    Volume group "data-vg01" successfully created
    ```

3. <span data-ttu-id="b9c0a-136">Vytvoření logické svazky hello.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-136">Create hello logical volume(s).</span></span> <span data-ttu-id="b9c0a-137">Hello příkaz níže jsme dojde k vytvoření jedné logické svazek nazvaný `data-lv01` toospan hello celý svazek skupiny, ale Všimněte si, je také vhodná toocreate více logických svazky ve skupině svazku hello.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-137">hello command below we will create a single logical volume called `data-lv01` toospan hello entire volume group, but note that it is also feasible toocreate multiple logical volumes in hello volume group.</span></span>

    ```bash   
    sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
    Logical volume "data-lv01" created.
    ```

4. <span data-ttu-id="b9c0a-138">Formátování svazku logické hello</span><span class="sxs-lookup"><span data-stu-id="b9c0a-138">Format hello logical volume</span></span>

    ```bash  
    sudo mkfs -t ext4 /dev/data-vg01/data-lv01
    ```
   
   > [!NOTE]
   > <span data-ttu-id="b9c0a-139">S použitím SLES11 `-t ext3` místo ext4.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-139">With SLES11 use `-t ext3` instead of ext4.</span></span> <span data-ttu-id="b9c0a-140">SLES11 podporuje pouze systémy tooext4 oprávnění jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-140">SLES11 only supports read-only access tooext4 filesystems.</span></span>

## <a name="add-hello-new-file-system-tooetcfstab"></a><span data-ttu-id="b9c0a-141">Přidat hello nový soubor systému příliš/atd/fstab</span><span class="sxs-lookup"><span data-stu-id="b9c0a-141">Add hello new file system too/etc/fstab</span></span>
> [!IMPORTANT]
> <span data-ttu-id="b9c0a-142">Nesprávně úpravy hello `/etc/fstab` soubor může mít za následek nelze spustit systém.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-142">Improperly editing hello `/etc/fstab` file could result in an unbootable system.</span></span> <span data-ttu-id="b9c0a-143">Pokud jistí, naleznete v dokumentaci toohello distribuční informace o tom, jak upravit tooproperly tento soubor.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-143">If unsure, please refer toohello distribution's documentation for information on how tooproperly edit this file.</span></span> <span data-ttu-id="b9c0a-144">Je také doporučeno zálohu hello `/etc/fstab` soubor je vytvořen před úpravou.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-144">It is also recommended that a backup of hello `/etc/fstab` file is created before editing.</span></span>

1. <span data-ttu-id="b9c0a-145">Vytvořte hello potřeby přípojný bod pro nový systém souborů, například:</span><span class="sxs-lookup"><span data-stu-id="b9c0a-145">Create hello desired mount point for your new file system, for example:</span></span>

    ```bash  
    sudo mkdir /data
    ```

2. <span data-ttu-id="b9c0a-146">Vyhledejte cestu logické svazku hello</span><span class="sxs-lookup"><span data-stu-id="b9c0a-146">Locate hello logical volume path</span></span>

    ```bash    
    lvdisplay
    --- Logical volume ---
    LV Path                /dev/data-vg01/data-lv01
    ....
    ```

3. <span data-ttu-id="b9c0a-147">Otevřete `/etc/fstab` v textovém editoru a přidejte položku pro hello nový systém souborů, například:</span><span class="sxs-lookup"><span data-stu-id="b9c0a-147">Open `/etc/fstab` in a text editor and add an entry for hello new file system, for example:</span></span>

    ```bash    
    /dev/data-vg01/data-lv01  /data  ext4  defaults  0  2
    ```   
    <span data-ttu-id="b9c0a-148">Potom uložte a zavřete `/etc/fstab`.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-148">Then, save and close `/etc/fstab`.</span></span>

4. <span data-ttu-id="b9c0a-149">Testování této hello `/etc/fstab` správnost zadání:</span><span class="sxs-lookup"><span data-stu-id="b9c0a-149">Test that hello `/etc/fstab` entry is correct:</span></span>

    ```bash    
    sudo mount -a
    ```

    <span data-ttu-id="b9c0a-150">Pokud tento příkaz má za následek chybové zprávy zkontrolujte syntaxi hello hello `/etc/fstab` souboru.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-150">If this command results in an error message please check hello syntax in hello `/etc/fstab` file.</span></span>
   
    <span data-ttu-id="b9c0a-151">Následně spusťte hello `mount` připojení k systému souborů hello tooensure příkaz:</span><span class="sxs-lookup"><span data-stu-id="b9c0a-151">Next run hello `mount` command tooensure hello file system is mounted:</span></span>

    ```bash    
    mount
    ......
    /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)
    ```

5. <span data-ttu-id="b9c0a-152">(Volitelné) Spouštěcí parametry bezporuchový v`/etc/fstab`</span><span class="sxs-lookup"><span data-stu-id="b9c0a-152">(Optional) Failsafe boot parameters in `/etc/fstab`</span></span>
   
    <span data-ttu-id="b9c0a-153">Velkém množství distribucí obsahovat buď hello `nobootwait` nebo `nofail` připojit parametry, které mohou být přidány toohello `/etc/fstab` souboru.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-153">Many distributions include either hello `nobootwait` or `nofail` mount parameters that may be added toohello `/etc/fstab` file.</span></span> <span data-ttu-id="b9c0a-154">Tyto parametry umožňují selhání při připojení příslušného systému souborů a povolit tooboot toocontinue systému Linux hello, i když je systém souborů nelze tooproperly připojení hello RAID.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-154">These parameters allow for failures when mounting a particular file system and allow hello Linux system toocontinue tooboot even if it is unable tooproperly mount hello RAID file system.</span></span> <span data-ttu-id="b9c0a-155">Další informace o těchto parametrů naleznete v dokumentaci tooyour distribuční.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-155">Please refer tooyour distribution's documentation for more information on these parameters.</span></span>
   
    <span data-ttu-id="b9c0a-156">Příklad (Ubuntu):</span><span class="sxs-lookup"><span data-stu-id="b9c0a-156">Example (Ubuntu):</span></span>

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
    ```

## <a name="trimunmap-support"></a><span data-ttu-id="b9c0a-157">Podpora uvolnění dočasné paměti nebo UNMAP</span><span class="sxs-lookup"><span data-stu-id="b9c0a-157">TRIM/UNMAP support</span></span>
<span data-ttu-id="b9c0a-158">Některé jádra Linux podporují toodiscard operace TRIM/UNMAP nepoužívané bloky na disku hello.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-158">Some Linux kernels support TRIM/UNMAP operations toodiscard unused blocks on hello disk.</span></span> <span data-ttu-id="b9c0a-159">Tyto operace jsou užitečné hlavně v tooinform standardní úložiště Azure, které odstraněné stránky již nejsou platné a může být vymazány.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-159">These operations are primarily useful in standard storage tooinform Azure that deleted pages are no longer valid and can be discarded.</span></span> <span data-ttu-id="b9c0a-160">Zahození stránky můžete uložit náklady, pokud chcete vytvořit velkých souborů a pak odstraňte je.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-160">Discarding pages can save cost if you create large files and then delete them.</span></span>

<span data-ttu-id="b9c0a-161">Existují dva způsoby tooenable TRIM podporují ve virtuálním počítačům s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-161">There are two ways tooenable TRIM support in your Linux VM.</span></span> <span data-ttu-id="b9c0a-162">Obvyklým způsobem podívejte se distribuční hello doporučenému přístupu:</span><span class="sxs-lookup"><span data-stu-id="b9c0a-162">As usual, consult your distribution for hello recommended approach:</span></span>

- <span data-ttu-id="b9c0a-163">Použití hello `discard` připojit možnost v `/etc/fstab`, například:</span><span class="sxs-lookup"><span data-stu-id="b9c0a-163">Use hello `discard` mount option in `/etc/fstab`, for example:</span></span>

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,discard  0  2
    ```

- <span data-ttu-id="b9c0a-164">V některých případech hello `discard` možnost může mít vliv na výkon.</span><span class="sxs-lookup"><span data-stu-id="b9c0a-164">In some cases hello `discard` option may have performance implications.</span></span> <span data-ttu-id="b9c0a-165">Alternativně můžete spustit hello `fstrim` ručně příkaz z příkazového řádku hello, nebo ho přidat tooyour crontab toorun pravidelně:</span><span class="sxs-lookup"><span data-stu-id="b9c0a-165">Alternatively, you can run hello `fstrim` command manually from hello command line, or add it tooyour crontab toorun regularly:</span></span>

    <span data-ttu-id="b9c0a-166">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="b9c0a-166">**Ubuntu**</span></span>

    ```bash 
    # sudo apt-get install util-linux
    # sudo fstrim /datadrive
    ```

    <span data-ttu-id="b9c0a-167">**RHEL nebo CentOS**</span><span class="sxs-lookup"><span data-stu-id="b9c0a-167">**RHEL/CentOS**</span></span>

    ```bash 
    # sudo yum install util-linux
    # sudo fstrim /datadrive
    ```
