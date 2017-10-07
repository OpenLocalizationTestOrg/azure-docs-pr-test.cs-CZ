---
title: "aaaCreate a nahrání virtuálního pevného disku Linux SUSE v Azure"
description: "Další informace toocreate a nahrajte Azure virtuálního pevného disku (VHD) obsahující operační systém SUSE Linux."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 066d01a6-2a54-4718-bcd0-90fe7a5303a1
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: szark
ms.openlocfilehash: 9185c7e67279357f00db0f43e944e96c58f0dd60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-a-sles-or-opensuse-virtual-machine-for-azure"></a><span data-ttu-id="d7553-103">Příprava virtuálního počítače se SLES nebo openSUSE pro Azure</span><span class="sxs-lookup"><span data-stu-id="d7553-103">Prepare a SLES or openSUSE virtual machine for Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a><span data-ttu-id="d7553-104">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d7553-104">Prerequisites</span></span>
<span data-ttu-id="d7553-105">Tento článek předpokládá, že jste již nainstalovali SUSE nebo openSUSE Linux tooa virtuální pevný disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="d7553-105">This article assumes that you have already installed a SUSE or openSUSE Linux operating system tooa virtual hard disk.</span></span> <span data-ttu-id="d7553-106">Několik nástrojů existovat toocreate soubory VHD, například řešení virtualizace jako je například technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="d7553-106">Multiple tools exist toocreate .vhd files, for example a virtualization solution such as Hyper-V.</span></span> <span data-ttu-id="d7553-107">Pokyny najdete v tématu [instalace hello Role Hyper-V a konfigurace virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="d7553-107">For instructions, see [Install hello Hyper-V Role and Configure a Virtual Machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

### <a name="sles--opensuse-installation-notes"></a><span data-ttu-id="d7553-108">SLES / openSUSE poznámky k instalaci</span><span class="sxs-lookup"><span data-stu-id="d7553-108">SLES / openSUSE installation notes</span></span>
* <span data-ttu-id="d7553-109">Najdete také [obecné poznámky k instalaci Linux](create-upload-generic.md#general-linux-installation-notes) Příprava na Azure Linux další tipy pro.</span><span class="sxs-lookup"><span data-stu-id="d7553-109">Please see also [General Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more tips on preparing Linux for Azure.</span></span>
* <span data-ttu-id="d7553-110">Formát VHDX Hello není podporován v Azure, pouze **pevný virtuální pevný disk**.</span><span class="sxs-lookup"><span data-stu-id="d7553-110">hello VHDX format is not supported in Azure, only **fixed VHD**.</span></span>  <span data-ttu-id="d7553-111">Můžete převést formát tooVHD hello disku pomocí Správce technologie Hyper-V nebo hello rutiny convert-VHD prostředí.</span><span class="sxs-lookup"><span data-stu-id="d7553-111">You can convert hello disk tooVHD format using Hyper-V Manager or hello convert-vhd cmdlet.</span></span>
* <span data-ttu-id="d7553-112">Při instalaci systému Linux hello se doporučuje použít standardní oddíly spíše než LVM (často hello výchozí pro mnoho instalace).</span><span class="sxs-lookup"><span data-stu-id="d7553-112">When installing hello Linux system it is recommended that you use standard partitions rather than LVM (often hello default for many installations).</span></span> <span data-ttu-id="d7553-113">Tím se vyhnete LVM název je v konfliktu s klonovaný virtuální počítače, zvlášť pokud operační systém disk někdy potřebovat tooanother toobe připojených virtuálních počítačů pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="d7553-113">This will avoid LVM name conflicts with cloned VMs, particularly if an OS disk ever needs toobe attached tooanother VM for troubleshooting.</span></span> <span data-ttu-id="d7553-114">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) lze použít v datových disků, pokud upřednostňovaný.</span><span class="sxs-lookup"><span data-stu-id="d7553-114">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks if preferred.</span></span>
* <span data-ttu-id="d7553-115">Nekonfigurujte přepnutí oddílu na disku operačního systému hello.</span><span class="sxs-lookup"><span data-stu-id="d7553-115">Do not configure a swap partition on hello OS disk.</span></span> <span data-ttu-id="d7553-116">Hello Linux agent může být nakonfigurované toocreate odkládací soubor na disku hello dočasných prostředků.</span><span class="sxs-lookup"><span data-stu-id="d7553-116">hello Linux agent can be configured toocreate a swap file on hello temporary resource disk.</span></span>  <span data-ttu-id="d7553-117">Další informace o této naleznete v následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="d7553-117">More information about this can be found in hello steps below.</span></span>
* <span data-ttu-id="d7553-118">Všechny virtuální pevné disky hello musí mít velikostí, které jsou násobky 1 MB.</span><span class="sxs-lookup"><span data-stu-id="d7553-118">All of hello VHDs must have sizes that are multiples of 1 MB.</span></span>

## <a name="use-suse-studio"></a><span data-ttu-id="d7553-119">Pomocí SUSE Studio</span><span class="sxs-lookup"><span data-stu-id="d7553-119">Use SUSE Studio</span></span>
<span data-ttu-id="d7553-120">[SUSE Studio](http://www.susestudio.com) můžete snadno vytvořit a spravovat vaše SLES a openSUSE Image Azure a technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="d7553-120">[SUSE Studio](http://www.susestudio.com) can easily create and manage your SLES and openSUSE images for Azure and Hyper-V.</span></span> <span data-ttu-id="d7553-121">Toto je doporučená přístup pro přizpůsobení vlastní Image SLES a openSUSE hello.</span><span class="sxs-lookup"><span data-stu-id="d7553-121">This is hello recommended approach for customizing your own SLES and openSUSE images.</span></span>

<span data-ttu-id="d7553-122">Jako alternativní toobuilding vlastního virtuálního pevného disku, SUSE, publikuje také Image BYOS (přineste si vlastní předplatné) pro SLES v [VMDepot](https://vmdepot.msopentech.com/User/Show?user=1007).</span><span class="sxs-lookup"><span data-stu-id="d7553-122">As an alternative toobuilding your own VHD, SUSE also publishes BYOS (Bring Your Own Subscription) images for SLES at [VMDepot](https://vmdepot.msopentech.com/User/Show?user=1007).</span></span>

## <a name="prepare-suse-linux-enterprise-server-11-sp4"></a><span data-ttu-id="d7553-123">Příprava SUSE Linux Enterprise Server 11 SP4</span><span class="sxs-lookup"><span data-stu-id="d7553-123">Prepare SUSE Linux Enterprise Server 11 SP4</span></span>
1. <span data-ttu-id="d7553-124">V prostředním podokně hello Správce technologie Hyper-V vyberte hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d7553-124">In hello center pane of Hyper-V Manager, select hello virtual machine.</span></span>
2. <span data-ttu-id="d7553-125">Klikněte na tlačítko **připojit** tooopen hello okna pro hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d7553-125">Click **Connect** tooopen hello window for hello virtual machine.</span></span>
3. <span data-ttu-id="d7553-126">Zaregistrovat vaše tooallow systému SUSE Linux Enterprise ho toodownload aktualizace a instalační balíčky.</span><span class="sxs-lookup"><span data-stu-id="d7553-126">Register your SUSE Linux Enterprise system tooallow it toodownload updates and install packages.</span></span>
4. <span data-ttu-id="d7553-127">Aktualizujte systém hello hello nejnovější opravy:</span><span class="sxs-lookup"><span data-stu-id="d7553-127">Update hello system with hello latest patches:</span></span>
   
        # sudo zypper update
5. <span data-ttu-id="d7553-128">Hello Azure Linux Agent nainstalujte z úložiště SLES hello:</span><span class="sxs-lookup"><span data-stu-id="d7553-128">Install hello Azure Linux Agent from hello SLES repository:</span></span>
   
        # sudo zypper install WALinuxAgent
6. <span data-ttu-id="d7553-129">Zkontrolujte, pokud je příkaz waagent příliš nastavte "na" v chkconfig a pokud ne, povolit pro automatické spuštění:</span><span class="sxs-lookup"><span data-stu-id="d7553-129">Check if waagent is set too"on" in chkconfig, and if not, enable it for autostart:</span></span>
   
        # sudo chkconfig waagent on
7. <span data-ttu-id="d7553-130">Zkontrolujte, zda příkaz waagent služby používá a pokud ne, spusťte ji:</span><span class="sxs-lookup"><span data-stu-id="d7553-130">Check if waagent service is running, and if not, start it:</span></span> 
   
        # sudo service waagent start
8. <span data-ttu-id="d7553-131">Upravte hello jádra spouštěcí řádku grub konfigurace tooinclude další jádra parametry pro Azure.</span><span class="sxs-lookup"><span data-stu-id="d7553-131">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="d7553-132">toodo tomto otevřete "/ boot/grub/menu.lst" v textovém editoru a ujistěte se, že jádra výchozí hello zahrnuje hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="d7553-132">toodo this open "/boot/grub/menu.lst" in a text editor and ensure that hello default kernel includes hello following parameters:</span></span>
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
   
    <span data-ttu-id="d7553-133">Tím bude zajištěno, všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure podporu pro ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="d7553-133">This will ensure all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span>
9. <span data-ttu-id="d7553-134">Potvrďte, že fstab /boot/grub/menu.lst a/etc/obou odkaz hello disku pomocí jeho UUID (podle uuid) namísto ID disku hello (podle id).</span><span class="sxs-lookup"><span data-stu-id="d7553-134">Confirm that /boot/grub/menu.lst and /etc/fstab both reference hello disk using its UUID (by-uuid) instead of hello disk ID (by-id).</span></span> 
   
    <span data-ttu-id="d7553-135">Získat UUID disku</span><span class="sxs-lookup"><span data-stu-id="d7553-135">Get disk UUID</span></span>
   
        # ls /dev/disk/by-uuid/
   
    <span data-ttu-id="d7553-136">Pokud /dev/disk/by-id nebo použít, aktualizovat /boot/grub/menu.lst a/etc/fstab s hodnotou správné podle uuid hello</span><span class="sxs-lookup"><span data-stu-id="d7553-136">If /dev/disk/by-id/ is used, update both /boot/grub/menu.lst and /etc/fstab with hello proper by-uuid value</span></span>
   
    <span data-ttu-id="d7553-137">Před změnou</span><span class="sxs-lookup"><span data-stu-id="d7553-137">Before change</span></span>
   
        root=/dev/disk/by-id/SCSI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx-part1
   
    <span data-ttu-id="d7553-138">Po změně</span><span class="sxs-lookup"><span data-stu-id="d7553-138">After change</span></span>
   
        root=/dev/disk/by-uuid/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
10. <span data-ttu-id="d7553-139">Upravte tooavoid udev pravidla generování statická pravidla pro hello alespoň jedno rozhraní sítě Ethernet.</span><span class="sxs-lookup"><span data-stu-id="d7553-139">Modify udev rules tooavoid generating static rules for hello Ethernet interface(s).</span></span> <span data-ttu-id="d7553-140">Tato pravidla může způsobit problémy při klonování virtuálního počítače v Microsoft Azure nebo Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="d7553-140">These rules can cause problems when cloning a virtual machine in Microsoft Azure or Hyper-V:</span></span>
    
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
11. <span data-ttu-id="d7553-141">Je doporučeno souboru hello tooedit "/ etc a sysconfig nebo sítě nebo dhcp" a změňte hello `DHCLIENT_SET_HOSTNAME` toohello následující parametr:</span><span class="sxs-lookup"><span data-stu-id="d7553-141">It is recommended tooedit hello file "/etc/sysconfig/network/dhcp" and change hello `DHCLIENT_SET_HOSTNAME` parameter toohello following:</span></span>
    
     <span data-ttu-id="d7553-142">DHCLIENT_SET_HOSTNAME = "žádný"</span><span class="sxs-lookup"><span data-stu-id="d7553-142">DHCLIENT_SET_HOSTNAME="no"</span></span>
12. <span data-ttu-id="d7553-143">Komentář "/ etc/sudoers", nebo odeberte hello, pokud existují následující řádky:</span><span class="sxs-lookup"><span data-stu-id="d7553-143">In "/etc/sudoers", comment out or remove hello following lines if they exist:</span></span>
    
     <span data-ttu-id="d7553-144">Výchozí nastavení targetpw # požádat o heslo hello hello cílové uživatele tj kořenové všechny ALL=(ALL) všechny # upozornění!</span><span class="sxs-lookup"><span data-stu-id="d7553-144">Defaults targetpw   # ask for hello password of hello target user i.e. root ALL    ALL=(ALL) ALL   # WARNING!</span></span> <span data-ttu-id="d7553-145">Pouze to používají společně s 'Výchozí hodnoty targetpw'!</span><span class="sxs-lookup"><span data-stu-id="d7553-145">Only use this together with 'Defaults targetpw'!</span></span>
13. <span data-ttu-id="d7553-146">Ujistěte se, že server SSH hello je nainstalován a nakonfigurován toostart při spuštění.</span><span class="sxs-lookup"><span data-stu-id="d7553-146">Ensure that hello SSH server is installed and configured toostart at boot time.</span></span>  <span data-ttu-id="d7553-147">Je to obvykle výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="d7553-147">This is usually hello default.</span></span>
14. <span data-ttu-id="d7553-148">Nevytvářejte odkládacího prostoru na disku operačního systému hello.</span><span class="sxs-lookup"><span data-stu-id="d7553-148">Do not create swap space on hello OS disk.</span></span>
    
    <span data-ttu-id="d7553-149">Hello Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku hello místní prostředek, který je připojený toohello virtuálních počítačů po zřízení v Azure.</span><span class="sxs-lookup"><span data-stu-id="d7553-149">hello Azure Linux Agent can automatically configure swap space using hello local resource disk that is attached toohello VM after provisioning on Azure.</span></span> <span data-ttu-id="d7553-150">Všimněte si, je tento disk hello místní prostředek *dočasné* na disku a může být vyprázdnit po deprovisioned hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d7553-150">Note that hello local resource disk is a *temporary* disk, and might be emptied when hello VM is deprovisioned.</span></span> <span data-ttu-id="d7553-151">Po instalaci hello Azure Linux Agent (viz předchozí krok), upravte hello následující parametry v /etc/waagent.conf odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d7553-151">After installing hello Azure Linux Agent (see previous step), modify hello following parameters in /etc/waagent.conf appropriately:</span></span>
    
     <span data-ttu-id="d7553-152">ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=&#2048;# Poznámka: nastavte tento toowhatever potřebovat toobe.</span><span class="sxs-lookup"><span data-stu-id="d7553-152">ResourceDisk.Format=y  ResourceDisk.Filesystem=ext4  ResourceDisk.MountPoint=/mnt/resource  ResourceDisk.EnableSwap=y  ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.</span></span>
15. <span data-ttu-id="d7553-153">Spusťte hello následující příkazy toodeprovision hello virtuálního počítače a jeho přípravu pro zřizování na Azure:</span><span class="sxs-lookup"><span data-stu-id="d7553-153">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>
    
    # <a name="sudo-waagent--force--deprovision"></a><span data-ttu-id="d7553-154">sudo příkaz waagent-force - deprovision</span><span class="sxs-lookup"><span data-stu-id="d7553-154">sudo waagent -force -deprovision</span></span>
    # <a name="export-histsize0"></a><span data-ttu-id="d7553-155">Export HISTSIZE = 0</span><span class="sxs-lookup"><span data-stu-id="d7553-155">export HISTSIZE=0</span></span>
    # <a name="logout"></a><span data-ttu-id="d7553-156">odhlášení</span><span class="sxs-lookup"><span data-stu-id="d7553-156">logout</span></span>
16. <span data-ttu-id="d7553-157">Klikněte na tlačítko **akce -> vypnutí dolů** ve Správci technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="d7553-157">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="d7553-158">Svůj disk VHD Linux je nyní připraven toobe nahrán tooAzure.</span><span class="sxs-lookup"><span data-stu-id="d7553-158">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>

- - -
## <a name="prepare-opensuse-131"></a><span data-ttu-id="d7553-159">Příprava openSUSE 13.1 +</span><span class="sxs-lookup"><span data-stu-id="d7553-159">Prepare openSUSE 13.1+</span></span>
1. <span data-ttu-id="d7553-160">V prostředním podokně hello Správce technologie Hyper-V vyberte hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d7553-160">In hello center pane of Hyper-V Manager, select hello virtual machine.</span></span>
2. <span data-ttu-id="d7553-161">Klikněte na tlačítko **připojit** tooopen hello okna pro hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d7553-161">Click **Connect** tooopen hello window for hello virtual machine.</span></span>
3. <span data-ttu-id="d7553-162">V prostředí hello, spusťte příkaz hello '`zypper lr`'.</span><span class="sxs-lookup"><span data-stu-id="d7553-162">On hello shell, run hello command '`zypper lr`'.</span></span> <span data-ttu-id="d7553-163">Pokud tento příkaz vrátí výstup podobný toohello následující, pak hello úložiště jsou nakonfigurované podle očekávání – bez úprav jsou nezbytné (Všimněte si, že se může lišit čísla verzí):</span><span class="sxs-lookup"><span data-stu-id="d7553-163">If this command returns output similar toohello following, then hello repositories are configured as expected--no adjustments are necessary (note that version numbers may vary):</span></span>
   
        # | Alias                 | Name                  | Enabled | Refresh
        --+-----------------------+-----------------------+---------+--------
        1 | Cloud:Tools_13.1      | Cloud:Tools_13.1      | Yes     | Yes
        2 | openSUSE_13.1_OSS     | openSUSE_13.1_OSS     | Yes     | Yes
        3 | openSUSE_13.1_Updates | openSUSE_13.1_Updates | Yes     | Yes
   
    <span data-ttu-id="d7553-164">Pokud příkaz hello vrátí "Žádné úložiště definované..." použijte následující příkazy tooadd hello tyto úložiště:</span><span class="sxs-lookup"><span data-stu-id="d7553-164">If hello command returns "No repositories defined..." then use hello following commands tooadd these repos:</span></span>
   
        # sudo zypper ar -f http://download.opensuse.org/repositories/Cloud:Tools/openSUSE_13.1 Cloud:Tools_13.1
        # sudo zypper ar -f http://download.opensuse.org/distribution/13.1/repo/oss openSUSE_13.1_OSS
        # sudo zypper ar -f http://download.opensuse.org/update/13.1 openSUSE_13.1_Updates
   
    <span data-ttu-id="d7553-165">Následně můžete ověřit spuštěním příkazu hello byly přidány hello úložiště se`zypper lr`se znovu.</span><span class="sxs-lookup"><span data-stu-id="d7553-165">You can then verify hello repositories have been added by running hello command '`zypper lr`' again.</span></span> <span data-ttu-id="d7553-166">V případě, že jeden z úložiště hello příslušné aktualizace není povolen, můžete ji povolte pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="d7553-166">In case one of hello relevant update repositories is not enabled, enable it with following command:</span></span>
   
        # sudo zypper mr -e [NUMBER OF REPOSITORY]
4. <span data-ttu-id="d7553-167">Aktualizace hello jádra toohello nejnovější dostupnou verzi:</span><span class="sxs-lookup"><span data-stu-id="d7553-167">Update hello kernel toohello latest available version:</span></span>
   
        # sudo zypper up kernel-default
   
    <span data-ttu-id="d7553-168">Nebo tooupdate hello systém s všechny nejnovější opravy hello:</span><span class="sxs-lookup"><span data-stu-id="d7553-168">Or tooupdate hello system with all hello latest patches:</span></span>
   
        # sudo zypper update
5. <span data-ttu-id="d7553-169">Nainstalujte hello Azure Linux Agent.</span><span class="sxs-lookup"><span data-stu-id="d7553-169">Install hello Azure Linux Agent.</span></span>
   
   # <a name="sudo-zypper-install-walinuxagent"></a><span data-ttu-id="d7553-170">instalace zypper sudo WALinuxAgent</span><span class="sxs-lookup"><span data-stu-id="d7553-170">sudo zypper install WALinuxAgent</span></span>
6. <span data-ttu-id="d7553-171">Upravte hello jádra spouštěcí řádku grub konfigurace tooinclude další jádra parametry pro Azure.</span><span class="sxs-lookup"><span data-stu-id="d7553-171">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="d7553-172">toodo tuto, otevřete "/ boot/grub/menu.lst" v textovém editoru a ujistěte se, že jádra výchozí hello zahrnuje hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="d7553-172">toodo this, open "/boot/grub/menu.lst" in a text editor and ensure that hello default kernel includes hello following parameters:</span></span>
   
     <span data-ttu-id="d7553-173">konzole = ttyS0 earlyprintk = ttyS0 rootdelay = 300</span><span class="sxs-lookup"><span data-stu-id="d7553-173">console=ttyS0 earlyprintk=ttyS0 rootdelay=300</span></span>
   
   <span data-ttu-id="d7553-174">Tím bude zajištěno, všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure podporu pro ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="d7553-174">This will ensure all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="d7553-175">Kromě toho odeberte hello následující parametry z řádku spouštěcí jádra hello, pokud existují:</span><span class="sxs-lookup"><span data-stu-id="d7553-175">In addition, remove hello following parameters from hello kernel boot line if they exist:</span></span>
   
     <span data-ttu-id="d7553-176">rezerva libata.atapi_enabled=0 = 0x1f0, 0x8</span><span class="sxs-lookup"><span data-stu-id="d7553-176">libata.atapi_enabled=0 reserve=0x1f0,0x8</span></span>
7. <span data-ttu-id="d7553-177">Je doporučeno souboru hello tooedit "/ etc a sysconfig nebo sítě nebo dhcp" a změňte hello `DHCLIENT_SET_HOSTNAME` toohello následující parametr:</span><span class="sxs-lookup"><span data-stu-id="d7553-177">It is recommended tooedit hello file "/etc/sysconfig/network/dhcp" and change hello `DHCLIENT_SET_HOSTNAME` parameter toohello following:</span></span>
   
     <span data-ttu-id="d7553-178">DHCLIENT_SET_HOSTNAME = "žádný"</span><span class="sxs-lookup"><span data-stu-id="d7553-178">DHCLIENT_SET_HOSTNAME="no"</span></span>
8. <span data-ttu-id="d7553-179">**Důležité:** v "/ etc/sudoers", komentář nebo odeberte hello, pokud existují následující řádky:</span><span class="sxs-lookup"><span data-stu-id="d7553-179">**Important:** In "/etc/sudoers", comment out or remove hello following lines if they exist:</span></span>
   
     <span data-ttu-id="d7553-180">Výchozí nastavení targetpw # požádat o heslo hello hello cílové uživatele tj kořenové všechny ALL=(ALL) všechny # upozornění!</span><span class="sxs-lookup"><span data-stu-id="d7553-180">Defaults targetpw   # ask for hello password of hello target user i.e. root ALL    ALL=(ALL) ALL   # WARNING!</span></span> <span data-ttu-id="d7553-181">Pouze to používají společně s 'Výchozí hodnoty targetpw'!</span><span class="sxs-lookup"><span data-stu-id="d7553-181">Only use this together with 'Defaults targetpw'!</span></span>
9. <span data-ttu-id="d7553-182">Ujistěte se, že server SSH hello je nainstalován a nakonfigurován toostart při spuštění.</span><span class="sxs-lookup"><span data-stu-id="d7553-182">Ensure that hello SSH server is installed and configured toostart at boot time.</span></span>  <span data-ttu-id="d7553-183">Je to obvykle výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="d7553-183">This is usually hello default.</span></span>
10. <span data-ttu-id="d7553-184">Nevytvářejte odkládacího prostoru na disku operačního systému hello.</span><span class="sxs-lookup"><span data-stu-id="d7553-184">Do not create swap space on hello OS disk.</span></span>
    
    <span data-ttu-id="d7553-185">Hello Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku hello místní prostředek, který je připojený toohello virtuálních počítačů po zřízení v Azure.</span><span class="sxs-lookup"><span data-stu-id="d7553-185">hello Azure Linux Agent can automatically configure swap space using hello local resource disk that is attached toohello VM after provisioning on Azure.</span></span> <span data-ttu-id="d7553-186">Všimněte si, je tento disk hello místní prostředek *dočasné* na disku a může být vyprázdnit po deprovisioned hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d7553-186">Note that hello local resource disk is a *temporary* disk, and might be emptied when hello VM is deprovisioned.</span></span> <span data-ttu-id="d7553-187">Po instalaci hello Azure Linux Agent (viz předchozí krok), upravte hello následující parametry v /etc/waagent.conf odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d7553-187">After installing hello Azure Linux Agent (see previous step), modify hello following parameters in /etc/waagent.conf appropriately:</span></span>
    
     <span data-ttu-id="d7553-188">ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=&#2048;# Poznámka: nastavte tento toowhatever potřebovat toobe.</span><span class="sxs-lookup"><span data-stu-id="d7553-188">ResourceDisk.Format=y  ResourceDisk.Filesystem=ext4  ResourceDisk.MountPoint=/mnt/resource  ResourceDisk.EnableSwap=y  ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.</span></span>
11. <span data-ttu-id="d7553-189">Spusťte hello následující příkazy toodeprovision hello virtuálního počítače a jeho přípravu pro zřizování na Azure:</span><span class="sxs-lookup"><span data-stu-id="d7553-189">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>
    
    # <a name="sudo-waagent--force--deprovision"></a><span data-ttu-id="d7553-190">sudo příkaz waagent-force - deprovision</span><span class="sxs-lookup"><span data-stu-id="d7553-190">sudo waagent -force -deprovision</span></span>
    # <a name="export-histsize0"></a><span data-ttu-id="d7553-191">Export HISTSIZE = 0</span><span class="sxs-lookup"><span data-stu-id="d7553-191">export HISTSIZE=0</span></span>
    # <a name="logout"></a><span data-ttu-id="d7553-192">odhlášení</span><span class="sxs-lookup"><span data-stu-id="d7553-192">logout</span></span>
12. <span data-ttu-id="d7553-193">Ujistěte se, hello Azure Linux Agent spustí při spuštění:</span><span class="sxs-lookup"><span data-stu-id="d7553-193">Ensure hello Azure Linux Agent runs at startup:</span></span>
    
        # sudo systemctl enable waagent.service
13. <span data-ttu-id="d7553-194">Klikněte na tlačítko **akce -> vypnutí dolů** ve Správci technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="d7553-194">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="d7553-195">Svůj disk VHD Linux je nyní připraven toobe nahrán tooAzure.</span><span class="sxs-lookup"><span data-stu-id="d7553-195">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d7553-196">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d7553-196">Next steps</span></span>
<span data-ttu-id="d7553-197">Můžete se teď připravena toouse SUSE Linux virtuální pevný disk toocreate nové virtuální počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="d7553-197">You're now ready toouse your SUSE Linux virtual hard disk toocreate new virtual machines in Azure.</span></span> <span data-ttu-id="d7553-198">Pokud je to hello poprvé, že jste odesílání tooAzure soubor VHD hello, najdete v části kroky 2 a 3 v [vytváření a odesílání virtuální pevný disk, který obsahuje operační systém Linux hello](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d7553-198">If this is hello first time that you're uploading hello .vhd file tooAzure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains hello Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

