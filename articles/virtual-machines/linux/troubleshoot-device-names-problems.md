---
title: "názvy zařízení aaaLinux virtuálního počítače se změní v Azure | Microsoft Docs"
description: "Vysvětluje hello proč názvy zařízení se změní a nabízí řešení tohoto problému."
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
ms.openlocfilehash: 4d3a5853d61edd2c8e8b85ab69e5ed3b3bc00bb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-linux-vm-device-names-are-changed"></a><span data-ttu-id="8f224-103">Řešení potíží: Názvy zařízení virtuálního počítače s Linuxem se změnilo</span><span class="sxs-lookup"><span data-stu-id="8f224-103">Troubleshooting: Linux VM device names are changed</span></span>

<span data-ttu-id="8f224-104">Hello článek vysvětluje, proč se po restartování systému Linux virtuálního počítače (VM), nebo znovu připojte hello disky změnit názvy zařízení.</span><span class="sxs-lookup"><span data-stu-id="8f224-104">hello article explains why device names are changed after you restart a Linux virtual machine (VM), or reattach hello disks.</span></span> <span data-ttu-id="8f224-105">Poskytuje také hello řešení tohoto problému.</span><span class="sxs-lookup"><span data-stu-id="8f224-105">It also provides hello solution for this problem.</span></span>

## <a name="symptom"></a><span data-ttu-id="8f224-106">Příznaky</span><span class="sxs-lookup"><span data-stu-id="8f224-106">Symptom</span></span>

<span data-ttu-id="8f224-107">Může dojít k následujícím problémům, když běží virtuální počítače s Linuxem v Microsoft Azure hello.</span><span class="sxs-lookup"><span data-stu-id="8f224-107">You may experience hello following problems when running Linux VMs in Microsoft Azure.</span></span>

- <span data-ttu-id="8f224-108">Hello virtuálního počítače selže tooboot po restartu.</span><span class="sxs-lookup"><span data-stu-id="8f224-108">hello VM fails tooboot after a restart.</span></span>

- <span data-ttu-id="8f224-109">Pokud datových disků jsou odpojit a znovu připojit, se mění hello názvy zařízení pro disky.</span><span class="sxs-lookup"><span data-stu-id="8f224-109">If data disks are detached and reattached, hello devices names for disks are changed.</span></span>

- <span data-ttu-id="8f224-110">Aplikace nebo skriptu, který odkazuje na disk pomocí názvu zařízení se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="8f224-110">An application or script that references a disk by using device name fails.</span></span> <span data-ttu-id="8f224-111">Zjistíte, že hello se změnil název zařízení hello disku.</span><span class="sxs-lookup"><span data-stu-id="8f224-111">You find that hello device name of hello disk is changed.</span></span>

## <a name="cause"></a><span data-ttu-id="8f224-112">Příčina</span><span class="sxs-lookup"><span data-stu-id="8f224-112">Cause</span></span>

<span data-ttu-id="8f224-113">Zařízení cesty v systému Linux se nezaručuje, že toobe konzistentní napříč restartování.</span><span class="sxs-lookup"><span data-stu-id="8f224-113">Device paths in Linux are not guaranteed toobe consistent across restarts.</span></span> <span data-ttu-id="8f224-114">Název zařízení se skládá z hlavní (písmeno) a menší čísla.</span><span class="sxs-lookup"><span data-stu-id="8f224-114">Device names consist of major (letter) and minor numbers.</span></span>  <span data-ttu-id="8f224-115">Pokud ovladač zařízení úložiště Linux hello zjistí nové zařízení, přiřadí tooit čísla hlavní a podverze zařízení z rozsahu hello k dispozici.</span><span class="sxs-lookup"><span data-stu-id="8f224-115">When hello Linux storage device driver detects a new device, it assigns major and minor device numbers tooit from hello available range.</span></span> <span data-ttu-id="8f224-116">Při odebrání zařízení jsou čísla zařízení hello uvolněné toobe znovu později.</span><span class="sxs-lookup"><span data-stu-id="8f224-116">When a device is removed, hello device numbers are freed toobe reused later.</span></span>

<span data-ttu-id="8f224-117">Hello k problému dochází kvůli hello zařízení kontrolu v systému Linux naplánované subsystémem SCSI hello probíhá asynchronně.</span><span class="sxs-lookup"><span data-stu-id="8f224-117">hello problem occurs because hello device scanning in Linux scheduled by hello SCSI subsystem happens asynchronously.</span></span> <span data-ttu-id="8f224-118">pojmenování cesta Hello konečné zařízení se může lišit mezi restarty.</span><span class="sxs-lookup"><span data-stu-id="8f224-118">hello final device path naming may vary across restarts.</span></span> 

## <a name="solution"></a><span data-ttu-id="8f224-119">Řešení</span><span class="sxs-lookup"><span data-stu-id="8f224-119">Solution</span></span>

<span data-ttu-id="8f224-120">tooresolve tento problém používat trvalé názvy.</span><span class="sxs-lookup"><span data-stu-id="8f224-120">tooresolve this problem, use persistent naming.</span></span> <span data-ttu-id="8f224-121">Existují čtyři metody toopersistent pojmenování - prostřednictvím systému souborů štítek, uuid, id a cestu.</span><span class="sxs-lookup"><span data-stu-id="8f224-121">There are four methods toopersistent naming - by filesystem label, by uuid, by id and by path.</span></span> <span data-ttu-id="8f224-122">Doporučujeme hello filesystem popisek a UUID metody pro virtuální počítače Linux Azure.</span><span class="sxs-lookup"><span data-stu-id="8f224-122">We recommend hello filesystem label and UUID methods for Azure Linux VMs.</span></span> 

<span data-ttu-id="8f224-123">Většina distribuce taky zadat buď hello **nofail** nebo **nobootwait** fstab možnosti.</span><span class="sxs-lookup"><span data-stu-id="8f224-123">Most distributions also provide either hello **nofail** or **nobootwait** fstab options.</span></span> <span data-ttu-id="8f224-124">Tyto možnosti Povolit tooboot systému i v případě selhání disku hello toomount při spuštění.</span><span class="sxs-lookup"><span data-stu-id="8f224-124">These options enable a system tooboot even if hello disk fails toomount at startup.</span></span> <span data-ttu-id="8f224-125">Další informace o těchto parametrů jsou uvedeny v dokumentaci distribuční hello.</span><span class="sxs-lookup"><span data-stu-id="8f224-125">Check hello distribution's documentation for more information about these parameters.</span></span> <span data-ttu-id="8f224-126">Další informace o tom, jak tooconfigure virtuálního počítače s Linuxem toouse UUID, když přidáte datový disk, najdete v části [připojit toohello virtuálního počítače s Linuxem toomount hello nový disk](add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk).</span><span class="sxs-lookup"><span data-stu-id="8f224-126">For more information about how tooconfigure a Linux VM toouse a UUID when you add a data disk, see [Connect toohello Linux VM toomount hello new disk](add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk).</span></span> 

<span data-ttu-id="8f224-127">Když hello Azure Linux agent nainstalovaný na virtuálním počítači, používá Udev pravidla tooconstruct sadu symbolické odkazy v části **/dev/disk/azure**.</span><span class="sxs-lookup"><span data-stu-id="8f224-127">When hello Azure Linux agent is installed on a VM, it uses Udev rules tooconstruct a set of symbolic links under **/dev/disk/azure**.</span></span> <span data-ttu-id="8f224-128">Tato pravidla Udev lze použít aplikace a skripty tooidentify disky připojené toohello virtuálních počítačů, jejich typu a hello logické jednotky.</span><span class="sxs-lookup"><span data-stu-id="8f224-128">These Udev rules can be used by applications and scripts tooidentify disks are attached toohello VM, their type, and hello LUN.</span></span>

## <a name="more-information"></a><span data-ttu-id="8f224-129">Další informace</span><span class="sxs-lookup"><span data-stu-id="8f224-129">More information</span></span>

### <a name="identify-disk-luns"></a><span data-ttu-id="8f224-130">Identifikovat logické diskové jednotky</span><span class="sxs-lookup"><span data-stu-id="8f224-130">Identify disk LUNs</span></span>

<span data-ttu-id="8f224-131">Aplikace můžete použít toofind logických jednotek, všechny hello připojené disky a vytváření symbolické odkazy.</span><span class="sxs-lookup"><span data-stu-id="8f224-131">An application can use LUNs toofind all hello attached disks and constructing symbolic links.</span></span> <span data-ttu-id="8f224-132">Hello Azure Linux agent teď obsahuje udev pravidla, která nastavit symbolické odkazy z zařízeních toohello logickou jednotku LUN, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="8f224-132">hello Azure Linux agent now includes udev rules that set up symbolic links from a LUN toohello devices, as follows:</span></span>

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
                                 

<span data-ttu-id="8f224-133">Informace o logických jednotkách můžete také načíst z hosta hello Linux pomocí lsscsi nebo podobné nástroj následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="8f224-133">LUN information can also be retrieved from hello Linux guest using lsscsi or similar tool as follows.</span></span>

       $ sudo lsscsi

      [1:0:0:0] cd/dvd Msft Virtual CD/ROM 1.0 /dev/sr0

      [2:0:0:0] disk Msft Virtual Disk 1.0 /dev/sda

      [3:0:1:0] disk Msft Virtual Disk 1.0 /dev/sdb

      [5:0:0:0] disk Msft Virtual Disk 1.0 /dev/sdc

      [5:0:0:1] disk Msft Virtual Disk 1.0 /dev/sdd

<span data-ttu-id="8f224-134">Tato informace o logických jednotkách hosta lze použít s předplatného Azure metadata tooidentify hello umístění v úložišti Azure hello virtuálního pevného disku, která ukládá data oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="8f224-134">This guest LUN information can be used with Azure subscription metadata tooidentify hello location in Azure storage of hello VHD which stores hello partition data.</span></span> <span data-ttu-id="8f224-135">Například použijte hello az rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="8f224-135">For example, use hello az cli:</span></span>

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

### <a name="discover-filesystem-uuids-by-using-blkid"></a><span data-ttu-id="8f224-136">Zjišťovat identifikátory UUID systému souborů pomocí blkid</span><span class="sxs-lookup"><span data-stu-id="8f224-136">Discover filesystem UUIDs by using blkid</span></span>

<span data-ttu-id="8f224-137">Soubor skriptu nebo aplikace může číst výstup hello blkid nebo podobné zdroje informací a vytvořit symbolické odkazy v **/dev** pro použití.</span><span class="sxs-lookup"><span data-stu-id="8f224-137">A script or application can read hello output of blkid, or similar sources of information, and construct symbolic links in **/dev** for use.</span></span> <span data-ttu-id="8f224-138">výstup Hello zobrazí hello identifikátory UUID všech disků připojených toohello virtuálních počítačů a hello zařízení souboru toowhich jsou přidružené:</span><span class="sxs-lookup"><span data-stu-id="8f224-138">hello output will show hello UUIDs of all disks attached toohello VM and hello device file toowhich they are associated:</span></span>

    $ sudo blkid -s UUID

    /dev/sr0: UUID="120B021372645f72"
    /dev/sda1: UUID="52c6959b-79b0-4bdd-8ed6-71e0ba782fb4"
    /dev/sdb1: UUID="176250df-9c7c-436f-94e4-d13f9bdea744"
    /dev/sdc1: UUID="b0048738-4ecc-4837-9793-49ce296d2692"

<span data-ttu-id="8f224-139">Hello příkaz waagent udev pravidla vytvořit sadu symbolické odkazy v části **/dev/disk/azure**:</span><span class="sxs-lookup"><span data-stu-id="8f224-139">hello waagent udev rules construct a set of symbolic links under **/dev/disk/azure**:</span></span>


    $ ls -l /dev/disk/azure

    total 0
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 resource -> ../../sdb
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 resource-part1 -> ../../sdb1
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 root -> ../../sda
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 root-part1 -> ../../sda1


<span data-ttu-id="8f224-140">Tyto informace můžete použít aplikace Hello identifikovat hello spouštěcí disk zařízení a prostředků (dočasné) disku hello.</span><span class="sxs-lookup"><span data-stu-id="8f224-140">hello application can use this information identify hello boot disk device and hello resource (ephemeral) disk.</span></span> <span data-ttu-id="8f224-141">V Azure, aplikace by měla odkazovat příliš**/dev/disk/azure/root-part1** nebo **/dev/disk/azure-resource-part1** toodiscover tyto oddíly.</span><span class="sxs-lookup"><span data-stu-id="8f224-141">In Azure, applications should refer too**/dev/disk/azure/root-part1** or **/dev/disk/azure-resource-part1** toodiscover these partitions.</span></span>

<span data-ttu-id="8f224-142">Pokud existují další oddíly hello blkid seznamu, jsou umístěny na datový disk.</span><span class="sxs-lookup"><span data-stu-id="8f224-142">If there are additional partitions from hello blkid list, they reside on a data disk.</span></span> <span data-ttu-id="8f224-143">Aplikace můžete udržovat hello UUID pro tyto oddíly a použijte cestu jako hello níže název zařízení hello toodiscover za běhu:</span><span class="sxs-lookup"><span data-stu-id="8f224-143">Applications can maintain hello UUID for these partitions and use a path like hello below toodiscover hello device name at runtime:</span></span>

    $ ls -l /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692

    lrwxrwxrwx 1 root root 10 Jun 19 15:57 /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692 -> ../../sdc1

    
### <a name="get-hello-latest-azure-storage-rules"></a><span data-ttu-id="8f224-144">Získat nejnovější pravidla úložiště Azure hello</span><span class="sxs-lookup"><span data-stu-id="8f224-144">Get hello latest Azure storage rules</span></span>

<span data-ttu-id="8f224-145">toohello nejnovější pravidla úložiště Azure, spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="8f224-145">toohello latest Azure storage rules, run th following commands:</span></span>

    # sudo curl -o /etc/udev/rules.d/66-azure-storage.rules https://raw.githubusercontent.com/Azure/WALinuxAgent/master/config/66-azure-storage.rules
    # sudo udevadm trigger --subsystem-match=block


<span data-ttu-id="8f224-146">Další informace najdete v tématu hello následující články:</span><span class="sxs-lookup"><span data-stu-id="8f224-146">For more information, see hello following articles:</span></span>

- [<span data-ttu-id="8f224-147">Ubuntu: Pomocí UUID</span><span class="sxs-lookup"><span data-stu-id="8f224-147">Ubuntu: Using UUID</span></span>](https://help.ubuntu.com/community/UsingUUID)

- [<span data-ttu-id="8f224-148">Red Hat: Trvalé pojmenování</span><span class="sxs-lookup"><span data-stu-id="8f224-148">Red Hat: Persistent Naming</span></span>](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Storage_Administration_Guide/persistent_naming.html)

- [<span data-ttu-id="8f224-149">Linux: Co identifikátory UUID využití.</span><span class="sxs-lookup"><span data-stu-id="8f224-149">Linux: What UUIDs can do for you</span></span>](https://www.linux.com/news/what-uuids-can-do-you)

- [<span data-ttu-id="8f224-150">Proces Udev: Úvod tooDevice správy v moderní systému Linux</span><span class="sxs-lookup"><span data-stu-id="8f224-150">Udev: Introduction tooDevice Management In Modern Linux System</span></span>](https://www.linux.com/news/udev-introduction-device-management-modern-linux-system)

