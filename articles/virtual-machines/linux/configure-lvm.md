---
title: "Konfigurace LVM na virtuální počítač s Linuxem | Microsoft Docs"
description: "Naučte se konfigurovat LVM v systému Linux v Azure."
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
ms.openlocfilehash: 7926627aaa3f0da935131f491d927ab5cb4b35c9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-lvm-on-a-linux-vm-in-azure"></a><span data-ttu-id="e2fa4-103">Konfigurace LVM na virtuální počítač s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="e2fa4-103">Configure LVM on a Linux VM in Azure</span></span>
<span data-ttu-id="e2fa4-104">Tento dokument popisuje postup konfigurace logické svazku Manager (LVM) ve virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-104">This document will discuss how to configure Logical Volume Manager (LVM) in your Azure virtual machine.</span></span> <span data-ttu-id="e2fa4-105">I když je to vhodné konfigurace LVM na všechny disky připojené k virtuálnímu počítači, ve výchozím nastavení většina cloudu Image nebude mít LVM nakonfigurované na disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-105">While it is feasible to configure LVM on any disk attached to the virtual machine, by default most cloud images will not have LVM configured on the OS disk.</span></span> <span data-ttu-id="e2fa4-106">Toto je zabránit problémům s duplicitní svazku skupiny, pokud disk operačního systému je někdy připojena k jiné virtuální počítač se stejným distribuce a typem, tj. během na scénář zotavení.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-106">This is to prevent problems with duplicate volume groups if the OS disk is ever attached to another VM of the same distribution and type, i.e. during a recovery scenario.</span></span> <span data-ttu-id="e2fa4-107">Proto se doporučuje jenom pro použití LVM v datových disků.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-107">Therefore it is recommended only to use LVM on the data disks.</span></span>

## <a name="linear-vs-striped-logical-volumes"></a><span data-ttu-id="e2fa4-108">Lineární oproti logické prokládané svazky</span><span class="sxs-lookup"><span data-stu-id="e2fa4-108">Linear vs. striped logical volumes</span></span>
<span data-ttu-id="e2fa4-109">LVM umožňuje zkombinovat do jednoho svazku úložiště počet fyzických disků.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-109">LVM can be used to combine a number of physical disks into a single storage volume.</span></span> <span data-ttu-id="e2fa4-110">Ve výchozím nastavení LVM obvykle vytvoří lineární logické svazcích, což znamená, že fyzického úložiště je zřetězen dohromady.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-110">By default LVM will usually create linear logical volumes, which means that the physical storage is concatenated together.</span></span> <span data-ttu-id="e2fa4-111">V takovém případě operace čtení a zápisu obvykle pouze odešle na jeden disk.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-111">In this case read/write operations will typically only be sent to a single disk.</span></span> <span data-ttu-id="e2fa4-112">Naproti tomu můžeme také prokládané svazky lze vytvářet logické kde čtení a zápisu jsou distribuovány do více disků, které jsou obsaženy ve skupině svazek (tj. Podobně jako 0).</span><span class="sxs-lookup"><span data-stu-id="e2fa4-112">In contrast, we can also create striped logical volumes where reads and writes are distributed to multiple disks contained in the volume group (i.e. similar to RAID0).</span></span> <span data-ttu-id="e2fa4-113">Z důvodů výkonu, pravděpodobně budete chtít rozkládají logické svazků tak, aby čtení a zápisy využívat všechny disky připojené data.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-113">For performance reasons it is likely you will want to stripe your logical volumes so that reads and writes utilize all your attached data disks.</span></span>

<span data-ttu-id="e2fa4-114">Tento dokument popisuje, jak kombinovat několik datových disků do skupiny jednom svazku a vytvořit Prokládané logické svazek.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-114">This document will describe how to combine several data disks into a single volume group, and then create a striped logical volume.</span></span> <span data-ttu-id="e2fa4-115">Následující postup poněkud zobecněné pro práci s většina distribuce.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-115">The steps below are somewhat generalized to work with most distributions.</span></span> <span data-ttu-id="e2fa4-116">Ve většině případů nástroje a pracovní postupy pro správu LVM v Azure nejsou zásadně liší od jiných prostředích.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-116">In most cases the utilities and workflows for managing LVM on Azure are not fundamentally different than other environments.</span></span> <span data-ttu-id="e2fa4-117">Obvyklým způsobem také najdete dodavatele Linux dokumentace a osvědčené postupy pro použití s konkrétní distribuční LVM.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-117">As usual, please also consult your Linux vendor for documentation and best practices for using LVM with your particular distribution.</span></span>

## <a name="attaching-data-disks"></a><span data-ttu-id="e2fa4-118">Připojení datových disků</span><span class="sxs-lookup"><span data-stu-id="e2fa4-118">Attaching data disks</span></span>
<span data-ttu-id="e2fa4-119">Jeden obvykle chtít začít s minimálně dva prázdné datových disků, při použití LVM.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-119">One will usually want to start with two or more empty data disks when using LVM.</span></span> <span data-ttu-id="e2fa4-120">Podle potřeb vstupně-výstupní operace, můžete připojit disky, které jsou uložené v našem standardního úložiště, s maximálně 500 vstupně-výstupní operace/ps pro na disk nebo naše storage úrovně Premium s až 5000 vstupně-výstupní operace za ps na disk.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-120">Based on your IO needs, you can choose to attach disks that are stored in our Standard Storage, with up to 500 IO/ps per disk or our Premium storage with up to 5000 IO/ps per disk.</span></span> <span data-ttu-id="e2fa4-121">Tento článek nebude přejděte do podrobnosti o tom, jak zřídit a připojte datových disků pro virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-121">This article will not go into detail on how to provision and attach data disks to a Linux virtual machine.</span></span> <span data-ttu-id="e2fa4-122">Podrobnosti najdete v článku Microsoft Azure [připojit disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) podrobné pokyny o tom, jak připojit prázdný datový disk pro virtuální počítač s Linuxem v Azure.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-122">Please see the Microsoft Azure article [attach a disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for detailed instructions on how to attach an empty data disk to a Linux virtual machine on Azure.</span></span>

## <a name="install-the-lvm-utilities"></a><span data-ttu-id="e2fa4-123">Nainstalujte nástroje LVM</span><span class="sxs-lookup"><span data-stu-id="e2fa4-123">Install the LVM utilities</span></span>
* <span data-ttu-id="e2fa4-124">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="e2fa4-124">**Ubuntu**</span></span>

    ```bash  
    sudo apt-get update
    sudo apt-get install lvm2
    ```

* <span data-ttu-id="e2fa4-125">**RHEL, CentOS a Oracle Linux**</span><span class="sxs-lookup"><span data-stu-id="e2fa4-125">**RHEL, CentOS & Oracle Linux**</span></span>

    ```bash   
    sudo yum install lvm2
    ```

* <span data-ttu-id="e2fa4-126">**SLES 12 a openSUSE**</span><span class="sxs-lookup"><span data-stu-id="e2fa4-126">**SLES 12 and openSUSE**</span></span>

    ```bash   
    sudo zypper install lvm2
    ```

* <span data-ttu-id="e2fa4-127">**SLES 11**</span><span class="sxs-lookup"><span data-stu-id="e2fa4-127">**SLES 11**</span></span>

    ```bash   
    sudo zypper install lvm2
    ```

    <span data-ttu-id="e2fa4-128">Na SLES11 je nutné upravit také `/etc/sysconfig/lvm` a nastavte `LVM_ACTIVATED_ON_DISCOVERED` "povolení":</span><span class="sxs-lookup"><span data-stu-id="e2fa4-128">On SLES11 you must also edit `/etc/sysconfig/lvm` and set `LVM_ACTIVATED_ON_DISCOVERED` to "enable":</span></span>

    ```sh   
    LVM_ACTIVATED_ON_DISCOVERED="enable" 
    ```

## <a name="configure-lvm"></a><span data-ttu-id="e2fa4-129">Konfigurace LVM</span><span class="sxs-lookup"><span data-stu-id="e2fa4-129">Configure LVM</span></span>
<span data-ttu-id="e2fa4-130">V tomto průvodci budeme předpokládat připojeny tři datových disků, které budeme označovat jako `/dev/sdc`, `/dev/sdd` a `/dev/sde`.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-130">In this guide we will assume you have attached three data disks, which we'll refer to as `/dev/sdc`, `/dev/sdd` and `/dev/sde`.</span></span> <span data-ttu-id="e2fa4-131">Všimněte si, že to nemusí být vždy stejné názvy cest v virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-131">Note that these may not always be the same path names in your VM.</span></span> <span data-ttu-id="e2fa4-132">Můžete spustit '`sudo fdisk -l`' nebo seznam dostupných disků podobné příkazu.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-132">You can run '`sudo fdisk -l`' or similar command to list your available disks.</span></span>

1. <span data-ttu-id="e2fa4-133">Příprava fyzických svazků:</span><span class="sxs-lookup"><span data-stu-id="e2fa4-133">Prepare the physical volumes:</span></span>

    ```bash    
    sudo pvcreate /dev/sd[cde]
    Physical volume "/dev/sdc" successfully created
    Physical volume "/dev/sdd" successfully created
    Physical volume "/dev/sde" successfully created
    ```

2. <span data-ttu-id="e2fa4-134">Vytvořte skupinu svazku.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-134">Create a volume group.</span></span> <span data-ttu-id="e2fa4-135">V tomto příkladu voláme skupině svazku `data-vg01`:</span><span class="sxs-lookup"><span data-stu-id="e2fa4-135">In this example we are calling the volume group `data-vg01`:</span></span>

    ```bash    
    sudo vgcreate data-vg01 /dev/sd[cde]
    Volume group "data-vg01" successfully created
    ```

3. <span data-ttu-id="e2fa4-136">Vytvoření logické svazky.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-136">Create the logical volume(s).</span></span> <span data-ttu-id="e2fa4-137">Příkaz níže jsme dojde k vytvoření jedné logické svazek nazvaný `data-lv01` span skupině celý svazek, ale Všimněte si, že je také nelze vytvořit více logických svazky ve skupině svazku.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-137">The command below we will create a single logical volume called `data-lv01` to span the entire volume group, but note that it is also feasible to create multiple logical volumes in the volume group.</span></span>

    ```bash   
    sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
    Logical volume "data-lv01" created.
    ```

4. <span data-ttu-id="e2fa4-138">Naformátujte svazek logického</span><span class="sxs-lookup"><span data-stu-id="e2fa4-138">Format the logical volume</span></span>

    ```bash  
    sudo mkfs -t ext4 /dev/data-vg01/data-lv01
    ```
   
   > [!NOTE]
   > <span data-ttu-id="e2fa4-139">S použitím SLES11 `-t ext3` místo ext4.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-139">With SLES11 use `-t ext3` instead of ext4.</span></span> <span data-ttu-id="e2fa4-140">SLES11 pouze podporuje přístup jen pro čtení k ext4 systémy.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-140">SLES11 only supports read-only access to ext4 filesystems.</span></span>

## <a name="add-the-new-file-system-to-etcfstab"></a><span data-ttu-id="e2fa4-141">Přidat nový systém souborů do /etc/fstab</span><span class="sxs-lookup"><span data-stu-id="e2fa4-141">Add the new file system to /etc/fstab</span></span>
> [!IMPORTANT]
> <span data-ttu-id="e2fa4-142">Nesprávně úpravy `/etc/fstab` souboru může mít za následek nelze spustit systém.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-142">Improperly editing the `/etc/fstab` file could result in an unbootable system.</span></span> <span data-ttu-id="e2fa4-143">Pokud si jisti, naleznete distribuční dokumentaci informace o tom, jak správně upravit tento soubor.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-143">If unsure, please refer to the distribution's documentation for information on how to properly edit this file.</span></span> <span data-ttu-id="e2fa4-144">Je také doporučeno zálohu `/etc/fstab` soubor je vytvořen před úpravou.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-144">It is also recommended that a backup of the `/etc/fstab` file is created before editing.</span></span>

1. <span data-ttu-id="e2fa4-145">Vytvořte požadované přípojného bodu pro nový systém souborů, například:</span><span class="sxs-lookup"><span data-stu-id="e2fa4-145">Create the desired mount point for your new file system, for example:</span></span>

    ```bash  
    sudo mkdir /data
    ```

2. <span data-ttu-id="e2fa4-146">Vyhledejte cestu logické svazku</span><span class="sxs-lookup"><span data-stu-id="e2fa4-146">Locate the logical volume path</span></span>

    ```bash    
    lvdisplay
    --- Logical volume ---
    LV Path                /dev/data-vg01/data-lv01
    ....
    ```

3. <span data-ttu-id="e2fa4-147">Otevřete `/etc/fstab` v textovém editoru a přidejte položku pro nový systém souborů, například:</span><span class="sxs-lookup"><span data-stu-id="e2fa4-147">Open `/etc/fstab` in a text editor and add an entry for the new file system, for example:</span></span>

    ```bash    
    /dev/data-vg01/data-lv01  /data  ext4  defaults  0  2
    ```   
    <span data-ttu-id="e2fa4-148">Potom uložte a zavřete `/etc/fstab`.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-148">Then, save and close `/etc/fstab`.</span></span>

4. <span data-ttu-id="e2fa4-149">Otestujte, že `/etc/fstab` správnost zadání:</span><span class="sxs-lookup"><span data-stu-id="e2fa4-149">Test that the `/etc/fstab` entry is correct:</span></span>

    ```bash    
    sudo mount -a
    ```

    <span data-ttu-id="e2fa4-150">Pokud tento příkaz výsledkem chybovou zprávu ve prosím zkontrolujte syntaxi `/etc/fstab` souboru.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-150">If this command results in an error message please check the syntax in the `/etc/fstab` file.</span></span>
   
    <span data-ttu-id="e2fa4-151">Následně spusťte `mount` zajistěte připojení k systému souborů:</span><span class="sxs-lookup"><span data-stu-id="e2fa4-151">Next run the `mount` command to ensure the file system is mounted:</span></span>

    ```bash    
    mount
    ......
    /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)
    ```

5. <span data-ttu-id="e2fa4-152">(Volitelné) Spouštěcí parametry bezporuchový v`/etc/fstab`</span><span class="sxs-lookup"><span data-stu-id="e2fa4-152">(Optional) Failsafe boot parameters in `/etc/fstab`</span></span>
   
    <span data-ttu-id="e2fa4-153">Velkém množství distribucí obsahovat buď `nobootwait` nebo `nofail` připojit parametry, které mohou být přidány do `/etc/fstab` souboru.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-153">Many distributions include either the `nobootwait` or `nofail` mount parameters that may be added to the `/etc/fstab` file.</span></span> <span data-ttu-id="e2fa4-154">Tyto parametry umožňují selhání při připojení příslušného systému souborů a povolit spuštění i v případě, že nelze správně připojit RAID systému souborů i nadále systému Linux.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-154">These parameters allow for failures when mounting a particular file system and allow the Linux system to continue to boot even if it is unable to properly mount the RAID file system.</span></span> <span data-ttu-id="e2fa4-155">Naleznete v dokumentaci distribuční na další informace o těchto parametrů.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-155">Please refer to your distribution's documentation for more information on these parameters.</span></span>
   
    <span data-ttu-id="e2fa4-156">Příklad (Ubuntu):</span><span class="sxs-lookup"><span data-stu-id="e2fa4-156">Example (Ubuntu):</span></span>

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
    ```

## <a name="trimunmap-support"></a><span data-ttu-id="e2fa4-157">Podpora uvolnění dočasné paměti nebo UNMAP</span><span class="sxs-lookup"><span data-stu-id="e2fa4-157">TRIM/UNMAP support</span></span>
<span data-ttu-id="e2fa4-158">Některé Linux jádra podporovat operace TRIM/UNMAP vyřadí nepoužívané bloky na disku.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-158">Some Linux kernels support TRIM/UNMAP operations to discard unused blocks on the disk.</span></span> <span data-ttu-id="e2fa4-159">Tyto operace jsou užitečné hlavně v standardní úložiště k informování Azure, které odstraněné stránky již nejsou platné a může být vymazány.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-159">These operations are primarily useful in standard storage to inform Azure that deleted pages are no longer valid and can be discarded.</span></span> <span data-ttu-id="e2fa4-160">Zahození stránky můžete uložit náklady, pokud chcete vytvořit velkých souborů a pak odstraňte je.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-160">Discarding pages can save cost if you create large files and then delete them.</span></span>

<span data-ttu-id="e2fa4-161">Existují dva způsoby, jak povolit TRIM podporují ve virtuálním počítačům s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-161">There are two ways to enable TRIM support in your Linux VM.</span></span> <span data-ttu-id="e2fa4-162">Obvyklým způsobem podívejte se distribuční o doporučený postup:</span><span class="sxs-lookup"><span data-stu-id="e2fa4-162">As usual, consult your distribution for the recommended approach:</span></span>

- <span data-ttu-id="e2fa4-163">Použití `discard` připojit možnost v `/etc/fstab`, například:</span><span class="sxs-lookup"><span data-stu-id="e2fa4-163">Use the `discard` mount option in `/etc/fstab`, for example:</span></span>

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,discard  0  2
    ```

- <span data-ttu-id="e2fa4-164">V některých případech `discard` možnost může mít vliv na výkon.</span><span class="sxs-lookup"><span data-stu-id="e2fa4-164">In some cases the `discard` option may have performance implications.</span></span> <span data-ttu-id="e2fa4-165">Alternativně můžete spustit `fstrim` ručně příkaz z příkazového řádku, nebo ho přidat do vaší crontab pravidelně spouštět:</span><span class="sxs-lookup"><span data-stu-id="e2fa4-165">Alternatively, you can run the `fstrim` command manually from the command line, or add it to your crontab to run regularly:</span></span>

    <span data-ttu-id="e2fa4-166">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="e2fa4-166">**Ubuntu**</span></span>

    ```bash 
    # sudo apt-get install util-linux
    # sudo fstrim /datadrive
    ```

    <span data-ttu-id="e2fa4-167">**RHEL nebo CentOS**</span><span class="sxs-lookup"><span data-stu-id="e2fa4-167">**RHEL/CentOS**</span></span>

    ```bash 
    # sudo yum install util-linux
    # sudo fstrim /datadrive
    ```
