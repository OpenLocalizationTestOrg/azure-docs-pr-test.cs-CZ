---
title: "Názvy zařízení virtuálního počítače s Linuxem se změní v Azure | Microsoft Docs"
description: "Vysvětluje důvod, proč se změní názvy zařízení a poskytují řešení tohoto problému."
services: virtual-machines-linux
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: 
ms.service: virtual-machines-linux
ms.topic: article
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.date: 07/12/2017
ms.author: genli
ms.openlocfilehash: 789f4580901a22dc3aaae9599c7205c76f268403
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="troubleshooting-linux-vm-device-names-are-changed"></a><span data-ttu-id="3ca8e-103">Řešení potíží: Názvy zařízení virtuálního počítače s Linuxem se změnilo</span><span class="sxs-lookup"><span data-stu-id="3ca8e-103">Troubleshooting: Linux VM device names are changed</span></span>

<span data-ttu-id="3ca8e-104">Tento článek vysvětluje, proč se po restartování systému Linux virtuálního počítače (VM), nebo znovu připojte disky změnit názvy zařízení.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-104">The article explains why device names are changed after you restart a Linux virtual machine (VM), or reattach the disks.</span></span> <span data-ttu-id="3ca8e-105">Také poskytuje řešení tohoto problému.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-105">It also provides the solution for this problem.</span></span>

## <a name="symptom"></a><span data-ttu-id="3ca8e-106">Příznaky</span><span class="sxs-lookup"><span data-stu-id="3ca8e-106">Symptom</span></span>

<span data-ttu-id="3ca8e-107">Následující problémy se můžete setkat, když běží virtuální počítače s Linuxem v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-107">You may experience the following problems when running Linux VMs in Microsoft Azure.</span></span>

- <span data-ttu-id="3ca8e-108">Virtuální počítač se nepodaří spustit po restartování.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-108">The VM fails to boot after a restart.</span></span>

- <span data-ttu-id="3ca8e-109">Pokud jsou datové disky odpojit a znovu připojit, se mění názvy zařízení pro disky.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-109">If data disks are detached and reattached, the devices names for disks are changed.</span></span>

- <span data-ttu-id="3ca8e-110">Aplikace nebo skriptu, který odkazuje na disk pomocí názvu zařízení se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-110">An application or script that references a disk by using device name fails.</span></span> <span data-ttu-id="3ca8e-111">Zjistíte, že se změnil název zařízení disku.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-111">You find that the device name of the disk is changed.</span></span>

## <a name="cause"></a><span data-ttu-id="3ca8e-112">Příčina</span><span class="sxs-lookup"><span data-stu-id="3ca8e-112">Cause</span></span>

<span data-ttu-id="3ca8e-113">Zařízení cesty v systému Linux nemusí být konzistentní napříč restartování.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-113">Device paths in Linux are not guaranteed to be consistent across restarts.</span></span> <span data-ttu-id="3ca8e-114">Název zařízení se skládá z hlavní (písmeno) a menší čísla.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-114">Device names consist of major (letter) and minor numbers.</span></span>  <span data-ttu-id="3ca8e-115">Pokud ovladač zařízení úložiště Linux zjistí nové zařízení, přiřadí čísla hlavní a podverze zařízení k němu z rozsahu k dispozici.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-115">When the Linux storage device driver detects a new device, it assigns major and minor device numbers to it from the available range.</span></span> <span data-ttu-id="3ca8e-116">Při odebrání zařízení se znovu později použije uvolněno čísla zařízení.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-116">When a device is removed, the device numbers are freed to be reused later.</span></span>

<span data-ttu-id="3ca8e-117">K problému dochází, protože zařízení kontrolu v systému Linux naplánované subsystémem SCSI probíhá asynchronně.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-117">The problem occurs because the device scanning in Linux scheduled by the SCSI subsystem happens asynchronously.</span></span> <span data-ttu-id="3ca8e-118">Pojmenování cesta konečné zařízení se může lišit mezi restarty.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-118">The final device path naming may vary across restarts.</span></span> 

## <a name="solution"></a><span data-ttu-id="3ca8e-119">Řešení</span><span class="sxs-lookup"><span data-stu-id="3ca8e-119">Solution</span></span>

<span data-ttu-id="3ca8e-120">Chcete-li vyřešit tento problém, použijte trvalé názvy.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-120">To resolve this problem, use persistent naming.</span></span> <span data-ttu-id="3ca8e-121">Existují čtyři metody do trvalé pojmenování – prostřednictvím filesystem štítek, uuid, podle id a cestu.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-121">There are four methods to persistent naming - by filesystem label, by uuid, by id and by path.</span></span> <span data-ttu-id="3ca8e-122">Doporučujeme popisek systému souborů a UUID metody pro virtuální počítače Linux Azure.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-122">We recommend the filesystem label and UUID methods for Azure Linux VMs.</span></span> 

<span data-ttu-id="3ca8e-123">Většina distribuce taky zadat buď **nofail** nebo **nobootwait** fstab možnosti.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-123">Most distributions also provide either the **nofail** or **nobootwait** fstab options.</span></span> <span data-ttu-id="3ca8e-124">Tyto možnosti Povolit systému spustit i v případě, že na disku se nepodaří připojit při spuštění.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-124">These options enable a system to boot even if the disk fails to mount at startup.</span></span> <span data-ttu-id="3ca8e-125">Další informace o těchto parametrů jsou uvedeny v dokumentaci distribuce.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-125">Check the distribution's documentation for more information about these parameters.</span></span> <span data-ttu-id="3ca8e-126">Další informace o tom, jak nakonfigurovat virtuální počítač s Linuxem používat UUID, když přidáte datový disk najdete v tématu [připojit k virtuálního počítače s Linuxem připojit nový disk](add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk).</span><span class="sxs-lookup"><span data-stu-id="3ca8e-126">For more information about how to configure a Linux VM to use a UUID when you add a data disk, see [Connect to the Linux VM to mount the new disk](add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk).</span></span> 

<span data-ttu-id="3ca8e-127">Pokud na virtuálním počítači je nainstalován agent nástroje Azure Linux, používá pravidla Udev vytvořit sadu symbolické odkazy v části **/dev/disk/azure**.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-127">When the Azure Linux agent is installed on a VM, it uses Udev rules to construct a set of symbolic links under **/dev/disk/azure**.</span></span> <span data-ttu-id="3ca8e-128">Tato pravidla Udev lze použít aplikace a skripty k identifikaci disky jsou připojené k virtuálnímu počítači, jejich typu a logickou jednotku LUN.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-128">These Udev rules can be used by applications and scripts to identify disks are attached to the VM, their type, and the LUN.</span></span>

## <a name="more-information"></a><span data-ttu-id="3ca8e-129">Další informace</span><span class="sxs-lookup"><span data-stu-id="3ca8e-129">More information</span></span>

### <a name="identify-disk-luns"></a><span data-ttu-id="3ca8e-130">Identifikovat logické diskové jednotky</span><span class="sxs-lookup"><span data-stu-id="3ca8e-130">Identify disk LUNs</span></span>

<span data-ttu-id="3ca8e-131">Aplikace můžete použít logické jednotky LUN najít všechny připojené disky a vytváření symbolické odkazy.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-131">An application can use LUNs to find all the attached disks and constructing symbolic links.</span></span> <span data-ttu-id="3ca8e-132">Azure Linux agent teď obsahuje udev pravidla, která nastavit symbolické odkazy z logické jednotky do zařízení, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3ca8e-132">The Azure Linux agent now includes udev rules that set up symbolic links from a LUN to the devices, as follows:</span></span>

    $ tree /dev/disk/azure

    /dev/disk/azure
    ├── resource -> ../../sdb
    ├── resource-part1 -> ../../sdb1
    ├── root -> ../../sda
    ├── root-part1 -> ../../sda1
    └── scsi1
        ├── lun0 -> ../../../sdc
        ├── lun0-part1 -> ../../../sdc1
        ├── lun1 -> ../../../sdd
        ├── lun1-part1 -> ../../../sdd1
        ├── lun1-part2 -> ../../../sdd2
        └── lun1-part3 -> ../../../sdd3                                    
                                 

<span data-ttu-id="3ca8e-133">Informace o logických jednotkách můžete také načíst z hosta Linux pomocí lsscsi nebo podobné nástroj následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-133">LUN information can also be retrieved from the Linux guest using lsscsi or similar tool as follows.</span></span>

       $ sudo lsscsi

      [1:0:0:0] cd/dvd Msft Virtual CD/ROM 1.0 /dev/sr0

      [2:0:0:0] disk Msft Virtual Disk 1.0 /dev/sda

      [3:0:1:0] disk Msft Virtual Disk 1.0 /dev/sdb

      [5:0:0:0] disk Msft Virtual Disk 1.0 /dev/sdc

      [5:0:0:1] disk Msft Virtual Disk 1.0 /dev/sdd

<span data-ttu-id="3ca8e-134">Tato informace o logických jednotkách hosta lze s metadaty předplatné Azure pro určení umístění v úložišti Azure virtuálního pevného disku, která ukládá data oddílu.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-134">This guest LUN information can be used with Azure subscription metadata to identify the location in Azure storage of the VHD which stores the partition data.</span></span> <span data-ttu-id="3ca8e-135">Například použijte rozhraní příkazového řádku az:</span><span class="sxs-lookup"><span data-stu-id="3ca8e-135">For example, use the az cli:</span></span>

    $ az vm show --resource-group testVM --name testVM | jq -r .storageProfile.dataDisks                                        
    [                                                                                                                                                                  
    {                                                                                                                                                                  
    "caching": "None",                                                                                                                                              
      "createOption": "empty",                                                                                                                                         
    "diskSizeGb": 1023,                                                                                                                                             
      "image": null,                                                                                                                                                   
    "lun": 0,                                                                                                                                                        
    "managedDisk": null,                                                                                                                                             
    "name": "testVM-20170619-114353",                                                                                                                    
    "vhd": {                                                                                                                                                          
      "uri": "https://testVM.blob.core.windows.net/vhd/testVM-20170619-114353.vhd"                                                       
    }                                                                                                                                                              
    },                                                                                                                                                                
    {                                                                                                                                                                   
    "caching": "None",                                                                                                                                               
    "createOption": "empty",                                                                                                                                         
    "diskSizeGb": 512,                                                                                                                                              
    "image": null,                                                                                                                                                   
    "lun": 1,                                                                                                                                                        
    "managedDisk": null,                                                                                                                                             
    "name": "testVM-20170619-121516",                                                                                                                    
    "vhd": {                                                                                                                                                           
      "uri": "https://testVM.blob.core.windows.net/vhd/testVM-20170619-121516.vhd"                                                       
      }                                                                                                                                                             
      }                                                                                                                                                             
    ]

### <a name="discover-filesystem-uuids-by-using-blkid"></a><span data-ttu-id="3ca8e-136">Zjišťovat identifikátory UUID systému souborů pomocí blkid</span><span class="sxs-lookup"><span data-stu-id="3ca8e-136">Discover filesystem UUIDs by using blkid</span></span>

<span data-ttu-id="3ca8e-137">Soubor skriptu nebo aplikace může číst výstup blkid nebo podobné zdroje informací a vytvořit symbolické odkazy v **/dev** pro použití.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-137">A script or application can read the output of blkid, or similar sources of information, and construct symbolic links in **/dev** for use.</span></span> <span data-ttu-id="3ca8e-138">Výstup bude zobrazovat identifikátory UUID všech disků připojených k virtuálnímu počítači a soubor zařízení, ke kterému jsou přidružené:</span><span class="sxs-lookup"><span data-stu-id="3ca8e-138">The output will show the UUIDs of all disks attached to the VM and the device file to which they are associated:</span></span>

    $ sudo blkid -s UUID

    /dev/sr0: UUID="120B021372645f72"
    /dev/sda1: UUID="52c6959b-79b0-4bdd-8ed6-71e0ba782fb4"
    /dev/sdb1: UUID="176250df-9c7c-436f-94e4-d13f9bdea744"
    /dev/sdc1: UUID="b0048738-4ecc-4837-9793-49ce296d2692"

<span data-ttu-id="3ca8e-139">Příkaz waagent udev pravidla vytvořit sadu symbolické odkazy v části **/dev/disk/azure**:</span><span class="sxs-lookup"><span data-stu-id="3ca8e-139">The waagent udev rules construct a set of symbolic links under **/dev/disk/azure**:</span></span>


    $ ls -l /dev/disk/azure

    total 0
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 resource -> ../../sdb
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 resource-part1 -> ../../sdb1
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 root -> ../../sda
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 root-part1 -> ../../sda1


<span data-ttu-id="3ca8e-140">Tyto informace můžete použít aplikaci identifikovat spouštěcí disk zařízení a prostředků (dočasné) disku.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-140">The application can use this information identify the boot disk device and the resource (ephemeral) disk.</span></span> <span data-ttu-id="3ca8e-141">V Azure, by měla aplikace označují **/dev/disk/azure/root-part1** nebo **/dev/disk/azure-resource-part1** ke zjištění těchto oddílů.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-141">In Azure, applications should refer to **/dev/disk/azure/root-part1** or **/dev/disk/azure-resource-part1** to discover these partitions.</span></span>

<span data-ttu-id="3ca8e-142">Pokud existují další oddíly ze seznamu blkid, jsou umístěny na datový disk.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-142">If there are additional partitions from the blkid list, they reside on a data disk.</span></span> <span data-ttu-id="3ca8e-143">Aplikace můžete udržovat identifikátor UUID pro tyto oddíly a použijte cestu jako zjištění názvu zařízení v době běhu níže:</span><span class="sxs-lookup"><span data-stu-id="3ca8e-143">Applications can maintain the UUID for these partitions and use a path like the below to discover the device name at runtime:</span></span>

    $ ls -l /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692

    lrwxrwxrwx 1 root root 10 Jun 19 15:57 /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692 -> ../../sdc1

    
### <a name="get-the-latest-azure-storage-rules"></a><span data-ttu-id="3ca8e-144">Získat nejnovější pravidla úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="3ca8e-144">Get the latest Azure storage rules</span></span>

<span data-ttu-id="3ca8e-145">Nejnovější pravidla úložiště Azure spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="3ca8e-145">To the latest Azure storage rules, run th following commands:</span></span>

    # sudo curl -o /etc/udev/rules.d/66-azure-storage.rules https://raw.githubusercontent.com/Azure/WALinuxAgent/master/config/66-azure-storage.rules
    # sudo udevadm trigger --subsystem-match=block


<span data-ttu-id="3ca8e-146">Další informace najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="3ca8e-146">For more information, see the following articles:</span></span>

- [<span data-ttu-id="3ca8e-147">Ubuntu: Pomocí UUID</span><span class="sxs-lookup"><span data-stu-id="3ca8e-147">Ubuntu: Using UUID</span></span>](https://help.ubuntu.com/community/UsingUUID)

- [<span data-ttu-id="3ca8e-148">Red Hat: Trvalé pojmenování</span><span class="sxs-lookup"><span data-stu-id="3ca8e-148">Red Hat: Persistent Naming</span></span>](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Storage_Administration_Guide/persistent_naming.html)

- [<span data-ttu-id="3ca8e-149">Linux: Co identifikátory UUID využití.</span><span class="sxs-lookup"><span data-stu-id="3ca8e-149">Linux: What UUIDs can do for you</span></span>](https://www.linux.com/news/what-uuids-can-do-you)

- [<span data-ttu-id="3ca8e-150">Proces Udev: Úvod do správy zařízení v nástroji moderní systému Linux</span><span class="sxs-lookup"><span data-stu-id="3ca8e-150">Udev: Introduction to Device Management In Modern Linux System</span></span>](https://www.linux.com/news/udev-introduction-device-management-modern-linux-system)

