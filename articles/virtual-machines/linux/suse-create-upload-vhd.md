---
title: "Vytvořit a nahrát VHD SUSE Linux v Azure"
description: "Naučte se vytvořit a odeslat Azure virtuálního pevného disku (VHD) obsahující operační systém SUSE Linux."
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
ms.openlocfilehash: c829f5d9a90b4260c6f41b2d9e511a0c6cb48f18
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="prepare-a-sles-or-opensuse-virtual-machine-for-azure"></a><span data-ttu-id="43d80-103">Příprava virtuálního počítače se SLES nebo openSUSE pro Azure</span><span class="sxs-lookup"><span data-stu-id="43d80-103">Prepare a SLES or openSUSE virtual machine for Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a><span data-ttu-id="43d80-104">Požadavky</span><span class="sxs-lookup"><span data-stu-id="43d80-104">Prerequisites</span></span>
<span data-ttu-id="43d80-105">Tento článek předpokládá, že jste již nainstalovali SUSE nebo openSUSE operační systém Linux virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="43d80-105">This article assumes that you have already installed a SUSE or openSUSE Linux operating system to a virtual hard disk.</span></span> <span data-ttu-id="43d80-106">Postup vytvoření souborů VHD, například řešení virtualizace jako je například technologie Hyper-V existuje několik nástrojů.</span><span class="sxs-lookup"><span data-stu-id="43d80-106">Multiple tools exist to create .vhd files, for example a virtualization solution such as Hyper-V.</span></span> <span data-ttu-id="43d80-107">Pokyny najdete v tématu [nainstalovat roli Hyper-V a konfigurace virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="43d80-107">For instructions, see [Install the Hyper-V Role and Configure a Virtual Machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

### <a name="sles--opensuse-installation-notes"></a><span data-ttu-id="43d80-108">SLES / openSUSE poznámky k instalaci</span><span class="sxs-lookup"><span data-stu-id="43d80-108">SLES / openSUSE installation notes</span></span>
* <span data-ttu-id="43d80-109">Najdete také [obecné poznámky k instalaci Linux](create-upload-generic.md#general-linux-installation-notes) Příprava na Azure Linux další tipy pro.</span><span class="sxs-lookup"><span data-stu-id="43d80-109">Please see also [General Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more tips on preparing Linux for Azure.</span></span>
* <span data-ttu-id="43d80-110">Formát VHDX není podporován v Azure, pouze **pevný virtuální pevný disk**.</span><span class="sxs-lookup"><span data-stu-id="43d80-110">The VHDX format is not supported in Azure, only **fixed VHD**.</span></span>  <span data-ttu-id="43d80-111">Disk můžete převést do formátu virtuálního pevného disku pomocí Správce technologie Hyper-V nebo rutiny convert-VHD prostředí.</span><span class="sxs-lookup"><span data-stu-id="43d80-111">You can convert the disk to VHD format using Hyper-V Manager or the convert-vhd cmdlet.</span></span>
* <span data-ttu-id="43d80-112">Při instalaci systému Linux se doporučuje použít standardní oddíly spíše než LVM (často výchozí pro mnoho instalace).</span><span class="sxs-lookup"><span data-stu-id="43d80-112">When installing the Linux system it is recommended that you use standard partitions rather than LVM (often the default for many installations).</span></span> <span data-ttu-id="43d80-113">Tím se vyhnete LVM název je v konfliktu s klonovaný virtuální počítače, zvlášť pokud někdy musí být připojené k jiným virtuálním Počítačem pro řešení potíží s disk s operačním systémem.</span><span class="sxs-lookup"><span data-stu-id="43d80-113">This will avoid LVM name conflicts with cloned VMs, particularly if an OS disk ever needs to be attached to another VM for troubleshooting.</span></span> <span data-ttu-id="43d80-114">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) lze použít v datových disků, pokud upřednostňovaný.</span><span class="sxs-lookup"><span data-stu-id="43d80-114">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks if preferred.</span></span>
* <span data-ttu-id="43d80-115">Nekonfigurujte přepnutí oddílu na disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="43d80-115">Do not configure a swap partition on the OS disk.</span></span> <span data-ttu-id="43d80-116">Chcete-li vytvořit odkládací soubor na disku dočasných prostředků lze nakonfigurovat agenta systému Linux.</span><span class="sxs-lookup"><span data-stu-id="43d80-116">The Linux agent can be configured to create a swap file on the temporary resource disk.</span></span>  <span data-ttu-id="43d80-117">Další informace o této naleznete v následujících krocích.</span><span class="sxs-lookup"><span data-stu-id="43d80-117">More information about this can be found in the steps below.</span></span>
* <span data-ttu-id="43d80-118">Všechny virtuální pevné disky musí mít velikostí, které jsou násobky 1 MB.</span><span class="sxs-lookup"><span data-stu-id="43d80-118">All of the VHDs must have sizes that are multiples of 1 MB.</span></span>

## <a name="use-suse-studio"></a><span data-ttu-id="43d80-119">Pomocí SUSE Studio</span><span class="sxs-lookup"><span data-stu-id="43d80-119">Use SUSE Studio</span></span>
<span data-ttu-id="43d80-120">[SUSE Studio](http://www.susestudio.com) můžete snadno vytvořit a spravovat vaše SLES a openSUSE Image Azure a technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="43d80-120">[SUSE Studio](http://www.susestudio.com) can easily create and manage your SLES and openSUSE images for Azure and Hyper-V.</span></span> <span data-ttu-id="43d80-121">Toto je doporučený postup pro přizpůsobení vlastní SLES a openSUSE bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="43d80-121">This is the recommended approach for customizing your own SLES and openSUSE images.</span></span>

<span data-ttu-id="43d80-122">Jako alternativu k vytvoření vlastního virtuálního pevného disku, SUSE, publikuje také Image BYOS (přineste si vlastní předplatné) pro SLES v [VMDepot](https://vmdepot.msopentech.com/User/Show?user=1007).</span><span class="sxs-lookup"><span data-stu-id="43d80-122">As an alternative to building your own VHD, SUSE also publishes BYOS (Bring Your Own Subscription) images for SLES at [VMDepot](https://vmdepot.msopentech.com/User/Show?user=1007).</span></span>

## <a name="prepare-suse-linux-enterprise-server-11-sp4"></a><span data-ttu-id="43d80-123">Příprava SUSE Linux Enterprise Server 11 SP4</span><span class="sxs-lookup"><span data-stu-id="43d80-123">Prepare SUSE Linux Enterprise Server 11 SP4</span></span>
1. <span data-ttu-id="43d80-124">V prostředním podokně Správce technologie Hyper-V vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="43d80-124">In the center pane of Hyper-V Manager, select the virtual machine.</span></span>
2. <span data-ttu-id="43d80-125">Klikněte na tlačítko **Connect** a otevřete okno pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="43d80-125">Click **Connect** to open the window for the virtual machine.</span></span>
3. <span data-ttu-id="43d80-126">Zaregistrujte systému SUSE Linux Enterprise tak, aby ji ke stahování aktualizací a instalaci balíčků.</span><span class="sxs-lookup"><span data-stu-id="43d80-126">Register your SUSE Linux Enterprise system to allow it to download updates and install packages.</span></span>
4. <span data-ttu-id="43d80-127">Aktualizace pomocí nejnovějších oprav systému:</span><span class="sxs-lookup"><span data-stu-id="43d80-127">Update the system with the latest patches:</span></span>
   
        # sudo zypper update
5. <span data-ttu-id="43d80-128">Instalovat agenta systému Linux Azure z úložiště SLES:</span><span class="sxs-lookup"><span data-stu-id="43d80-128">Install the Azure Linux Agent from the SLES repository:</span></span>
   
        # sudo zypper install WALinuxAgent
6. <span data-ttu-id="43d80-129">Zkontrolujte Pokud příkaz waagent nastavená na "on" v chkconfig a pokud ne, povolit pro automatické spuštění:</span><span class="sxs-lookup"><span data-stu-id="43d80-129">Check if waagent is set to "on" in chkconfig, and if not, enable it for autostart:</span></span>
   
        # sudo chkconfig waagent on
7. <span data-ttu-id="43d80-130">Zkontrolujte, zda příkaz waagent služby používá a pokud ne, spusťte ji:</span><span class="sxs-lookup"><span data-stu-id="43d80-130">Check if waagent service is running, and if not, start it:</span></span> 
   
        # sudo service waagent start
8. <span data-ttu-id="43d80-131">Upravte řádku spouštěcí jádra ve vaší konfiguraci grub pro zahrnutí dalších jádra parametry Azure.</span><span class="sxs-lookup"><span data-stu-id="43d80-131">Modify the kernel boot line in your grub configuration to include additional kernel parameters for Azure.</span></span> <span data-ttu-id="43d80-132">Chcete tuto otevřete "/ boot/grub/menu.lst" v textovém editoru a ujistěte se, že výchozí jádra zahrnuje následující parametry:</span><span class="sxs-lookup"><span data-stu-id="43d80-132">To do this open "/boot/grub/menu.lst" in a text editor and ensure that the default kernel includes the following parameters:</span></span>
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
   
    <span data-ttu-id="43d80-133">Tím bude zajištěno, všechny zprávy konzoly odešlou do první sériového portu, který může být užitečné Azure podporu pro ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="43d80-133">This will ensure all console messages are sent to the first serial port, which can assist Azure support with debugging issues.</span></span>
9. <span data-ttu-id="43d80-134">Potvrďte, že /boot/grub/menu.lst a /etc/fstab obě odkazují disku pomocí jeho UUID (podle uuid) namísto ID disku (podle id).</span><span class="sxs-lookup"><span data-stu-id="43d80-134">Confirm that /boot/grub/menu.lst and /etc/fstab both reference the disk using its UUID (by-uuid) instead of the disk ID (by-id).</span></span> 
   
    <span data-ttu-id="43d80-135">Získat UUID disku</span><span class="sxs-lookup"><span data-stu-id="43d80-135">Get disk UUID</span></span>
   
        # ls /dev/disk/by-uuid/
   
    <span data-ttu-id="43d80-136">Pokud /dev/disk/by-id nebo použít, aktualizovat /boot/grub/menu.lst a/etc/fstab s hodnotou správné podle uuid</span><span class="sxs-lookup"><span data-stu-id="43d80-136">If /dev/disk/by-id/ is used, update both /boot/grub/menu.lst and /etc/fstab with the proper by-uuid value</span></span>
   
    <span data-ttu-id="43d80-137">Před změnou</span><span class="sxs-lookup"><span data-stu-id="43d80-137">Before change</span></span>
   
        root=/dev/disk/by-id/SCSI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx-part1
   
    <span data-ttu-id="43d80-138">Po změně</span><span class="sxs-lookup"><span data-stu-id="43d80-138">After change</span></span>
   
        root=/dev/disk/by-uuid/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
10. <span data-ttu-id="43d80-139">Změňte pravidla udev, aby nedošlo ke generování statická pravidla pro alespoň jedno rozhraní sítě Ethernet.</span><span class="sxs-lookup"><span data-stu-id="43d80-139">Modify udev rules to avoid generating static rules for the Ethernet interface(s).</span></span> <span data-ttu-id="43d80-140">Tato pravidla může způsobit problémy při klonování virtuálního počítače v Microsoft Azure nebo Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="43d80-140">These rules can cause problems when cloning a virtual machine in Microsoft Azure or Hyper-V:</span></span>
    
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
11. <span data-ttu-id="43d80-141">Je doporučené upravit soubor "/ etc a sysconfig nebo sítě nebo dhcp" a změnit `DHCLIENT_SET_HOSTNAME` parametr pro následující:</span><span class="sxs-lookup"><span data-stu-id="43d80-141">It is recommended to edit the file "/etc/sysconfig/network/dhcp" and change the `DHCLIENT_SET_HOSTNAME` parameter to the following:</span></span>
    
     <span data-ttu-id="43d80-142">DHCLIENT_SET_HOSTNAME = "žádný"</span><span class="sxs-lookup"><span data-stu-id="43d80-142">DHCLIENT_SET_HOSTNAME="no"</span></span>
12. <span data-ttu-id="43d80-143">Komentář "/ etc/sudoers", nebo pokud existují, odeberte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="43d80-143">In "/etc/sudoers", comment out or remove the following lines if they exist:</span></span>
    
     <span data-ttu-id="43d80-144">Výchozí nastavení targetpw # požádat o heslo cílový uživatel tj kořenové všechny ALL=(ALL) všechny # upozornění!</span><span class="sxs-lookup"><span data-stu-id="43d80-144">Defaults targetpw   # ask for the password of the target user i.e. root ALL    ALL=(ALL) ALL   # WARNING!</span></span> <span data-ttu-id="43d80-145">Pouze to používají společně s 'Výchozí hodnoty targetpw'!</span><span class="sxs-lookup"><span data-stu-id="43d80-145">Only use this together with 'Defaults targetpw'!</span></span>
13. <span data-ttu-id="43d80-146">Ujistěte se, že SSH server je nainstalován a nakonfigurován na spuštění při spuštění.</span><span class="sxs-lookup"><span data-stu-id="43d80-146">Ensure that the SSH server is installed and configured to start at boot time.</span></span>  <span data-ttu-id="43d80-147">Obvykle je to výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="43d80-147">This is usually the default.</span></span>
14. <span data-ttu-id="43d80-148">Nevytvářejte odkládacího prostoru na disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="43d80-148">Do not create swap space on the OS disk.</span></span>
    
    <span data-ttu-id="43d80-149">Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku místní prostředek, který je připojen k virtuálnímu počítači po zřízení v Azure.</span><span class="sxs-lookup"><span data-stu-id="43d80-149">The Azure Linux Agent can automatically configure swap space using the local resource disk that is attached to the VM after provisioning on Azure.</span></span> <span data-ttu-id="43d80-150">Všimněte si, že je disk místní prostředek *dočasné* na disku a může vyprázdněny, když je virtuální počítač zrušit.</span><span class="sxs-lookup"><span data-stu-id="43d80-150">Note that the local resource disk is a *temporary* disk, and might be emptied when the VM is deprovisioned.</span></span> <span data-ttu-id="43d80-151">Po instalaci Azure Linux Agent (viz předchozí krok), upravte následující parametry v /etc/waagent.conf odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="43d80-151">After installing the Azure Linux Agent (see previous step), modify the following parameters in /etc/waagent.conf appropriately:</span></span>
    
     <span data-ttu-id="43d80-152">ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=&#2048;# Poznámka: tuto možnost nastavíte na ať už budete potřebovat mohla být.</span><span class="sxs-lookup"><span data-stu-id="43d80-152">ResourceDisk.Format=y  ResourceDisk.Filesystem=ext4  ResourceDisk.MountPoint=/mnt/resource  ResourceDisk.EnableSwap=y  ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.</span></span>
15. <span data-ttu-id="43d80-153">Spusťte následující příkaz pro zrušení zřízení virtuálního počítače a jeho přípravu pro zřizování na Azure:</span><span class="sxs-lookup"><span data-stu-id="43d80-153">Run the following commands to deprovision the virtual machine and prepare it for provisioning on Azure:</span></span>
    
    # <a name="sudo-waagent--force--deprovision"></a><span data-ttu-id="43d80-154">sudo příkaz waagent-force - deprovision</span><span class="sxs-lookup"><span data-stu-id="43d80-154">sudo waagent -force -deprovision</span></span>
    # <a name="export-histsize0"></a><span data-ttu-id="43d80-155">Export HISTSIZE = 0</span><span class="sxs-lookup"><span data-stu-id="43d80-155">export HISTSIZE=0</span></span>
    # <a name="logout"></a><span data-ttu-id="43d80-156">odhlášení</span><span class="sxs-lookup"><span data-stu-id="43d80-156">logout</span></span>
16. <span data-ttu-id="43d80-157">Klikněte na tlačítko **akce -> vypnutí dolů** ve Správci technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="43d80-157">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="43d80-158">Svůj disk VHD Linux je nyní připravena k odeslání do Azure.</span><span class="sxs-lookup"><span data-stu-id="43d80-158">Your Linux VHD is now ready to be uploaded to Azure.</span></span>

- - -
## <a name="prepare-opensuse-131"></a><span data-ttu-id="43d80-159">Příprava openSUSE 13.1 +</span><span class="sxs-lookup"><span data-stu-id="43d80-159">Prepare openSUSE 13.1+</span></span>
1. <span data-ttu-id="43d80-160">V prostředním podokně Správce technologie Hyper-V vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="43d80-160">In the center pane of Hyper-V Manager, select the virtual machine.</span></span>
2. <span data-ttu-id="43d80-161">Klikněte na tlačítko **Connect** a otevřete okno pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="43d80-161">Click **Connect** to open the window for the virtual machine.</span></span>
3. <span data-ttu-id="43d80-162">V prostředí, spusťte příkaz '`zypper lr`'.</span><span class="sxs-lookup"><span data-stu-id="43d80-162">On the shell, run the command '`zypper lr`'.</span></span> <span data-ttu-id="43d80-163">Pokud tento příkaz vrátí výstup podobný následujícímu, pak úložiště jsou nakonfigurované podle očekávání – bez úprav jsou nezbytné (Všimněte si, že verze čísla se mohou lišit):</span><span class="sxs-lookup"><span data-stu-id="43d80-163">If this command returns output similar to the following, then the repositories are configured as expected--no adjustments are necessary (note that version numbers may vary):</span></span>
   
        # | Alias                 | Name                  | Enabled | Refresh
        --+-----------------------+-----------------------+---------+--------
        1 | Cloud:Tools_13.1      | Cloud:Tools_13.1      | Yes     | Yes
        2 | openSUSE_13.1_OSS     | openSUSE_13.1_OSS     | Yes     | Yes
        3 | openSUSE_13.1_Updates | openSUSE_13.1_Updates | Yes     | Yes
   
    <span data-ttu-id="43d80-164">Pokud příkaz vrátí "Žádné úložiště definované..." použijte následující příkazy pro přidání těchto úložiště:</span><span class="sxs-lookup"><span data-stu-id="43d80-164">If the command returns "No repositories defined..." then use the following commands to add these repos:</span></span>
   
        # sudo zypper ar -f http://download.opensuse.org/repositories/Cloud:Tools/openSUSE_13.1 Cloud:Tools_13.1
        # sudo zypper ar -f http://download.opensuse.org/distribution/13.1/repo/oss openSUSE_13.1_OSS
        # sudo zypper ar -f http://download.opensuse.org/update/13.1 openSUSE_13.1_Updates
   
    <span data-ttu-id="43d80-165">Následně můžete ověřit úložiště přidané spuštěním příkazu '`zypper lr`se znovu.</span><span class="sxs-lookup"><span data-stu-id="43d80-165">You can then verify the repositories have been added by running the command '`zypper lr`' again.</span></span> <span data-ttu-id="43d80-166">V případě, že jeden z úložiště příslušné aktualizace není povolen, můžete ji povolte pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="43d80-166">In case one of the relevant update repositories is not enabled, enable it with following command:</span></span>
   
        # sudo zypper mr -e [NUMBER OF REPOSITORY]
4. <span data-ttu-id="43d80-167">Aktualizujte na nejnovější dostupnou verzi jádra:</span><span class="sxs-lookup"><span data-stu-id="43d80-167">Update the kernel to the latest available version:</span></span>
   
        # sudo zypper up kernel-default
   
    <span data-ttu-id="43d80-168">Nebo aktualizujte systém s nejnovější opravy:</span><span class="sxs-lookup"><span data-stu-id="43d80-168">Or to update the system with all the latest patches:</span></span>
   
        # sudo zypper update
5. <span data-ttu-id="43d80-169">Nainstalujte Azure Linux Agent.</span><span class="sxs-lookup"><span data-stu-id="43d80-169">Install the Azure Linux Agent.</span></span>
   
   # <a name="sudo-zypper-install-walinuxagent"></a><span data-ttu-id="43d80-170">instalace zypper sudo WALinuxAgent</span><span class="sxs-lookup"><span data-stu-id="43d80-170">sudo zypper install WALinuxAgent</span></span>
6. <span data-ttu-id="43d80-171">Upravte řádku spouštěcí jádra ve vaší konfiguraci grub pro zahrnutí dalších jádra parametry Azure.</span><span class="sxs-lookup"><span data-stu-id="43d80-171">Modify the kernel boot line in your grub configuration to include additional kernel parameters for Azure.</span></span> <span data-ttu-id="43d80-172">Chcete-li to provést, otevřete "/ boot/grub/menu.lst" v textovém editoru a ujistěte se, že výchozí jádra zahrnuje následující parametry:</span><span class="sxs-lookup"><span data-stu-id="43d80-172">To do this, open "/boot/grub/menu.lst" in a text editor and ensure that the default kernel includes the following parameters:</span></span>
   
     <span data-ttu-id="43d80-173">konzole = ttyS0 earlyprintk = ttyS0 rootdelay = 300</span><span class="sxs-lookup"><span data-stu-id="43d80-173">console=ttyS0 earlyprintk=ttyS0 rootdelay=300</span></span>
   
   <span data-ttu-id="43d80-174">Tím bude zajištěno, všechny zprávy konzoly odešlou do první sériového portu, který může být užitečné Azure podporu pro ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="43d80-174">This will ensure all console messages are sent to the first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="43d80-175">Kromě toho odeberte tyto parametry z řádku spouštěcí jádra, pokud existují:</span><span class="sxs-lookup"><span data-stu-id="43d80-175">In addition, remove the following parameters from the kernel boot line if they exist:</span></span>
   
     <span data-ttu-id="43d80-176">rezerva libata.atapi_enabled=0 = 0x1f0, 0x8</span><span class="sxs-lookup"><span data-stu-id="43d80-176">libata.atapi_enabled=0 reserve=0x1f0,0x8</span></span>
7. <span data-ttu-id="43d80-177">Je doporučené upravit soubor "/ etc a sysconfig nebo sítě nebo dhcp" a změnit `DHCLIENT_SET_HOSTNAME` parametr pro následující:</span><span class="sxs-lookup"><span data-stu-id="43d80-177">It is recommended to edit the file "/etc/sysconfig/network/dhcp" and change the `DHCLIENT_SET_HOSTNAME` parameter to the following:</span></span>
   
     <span data-ttu-id="43d80-178">DHCLIENT_SET_HOSTNAME = "žádný"</span><span class="sxs-lookup"><span data-stu-id="43d80-178">DHCLIENT_SET_HOSTNAME="no"</span></span>
8. <span data-ttu-id="43d80-179">**Důležité:** komentář "/ etc/sudoers", nebo odeberte následující řádky, pokud existují:</span><span class="sxs-lookup"><span data-stu-id="43d80-179">**Important:** In "/etc/sudoers", comment out or remove the following lines if they exist:</span></span>
   
     <span data-ttu-id="43d80-180">Výchozí nastavení targetpw # požádat o heslo cílový uživatel tj kořenové všechny ALL=(ALL) všechny # upozornění!</span><span class="sxs-lookup"><span data-stu-id="43d80-180">Defaults targetpw   # ask for the password of the target user i.e. root ALL    ALL=(ALL) ALL   # WARNING!</span></span> <span data-ttu-id="43d80-181">Pouze to používají společně s 'Výchozí hodnoty targetpw'!</span><span class="sxs-lookup"><span data-stu-id="43d80-181">Only use this together with 'Defaults targetpw'!</span></span>
9. <span data-ttu-id="43d80-182">Ujistěte se, že SSH server je nainstalován a nakonfigurován na spuštění při spuštění.</span><span class="sxs-lookup"><span data-stu-id="43d80-182">Ensure that the SSH server is installed and configured to start at boot time.</span></span>  <span data-ttu-id="43d80-183">Obvykle je to výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="43d80-183">This is usually the default.</span></span>
10. <span data-ttu-id="43d80-184">Nevytvářejte odkládacího prostoru na disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="43d80-184">Do not create swap space on the OS disk.</span></span>
    
    <span data-ttu-id="43d80-185">Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku místní prostředek, který je připojen k virtuálnímu počítači po zřízení v Azure.</span><span class="sxs-lookup"><span data-stu-id="43d80-185">The Azure Linux Agent can automatically configure swap space using the local resource disk that is attached to the VM after provisioning on Azure.</span></span> <span data-ttu-id="43d80-186">Všimněte si, že je disk místní prostředek *dočasné* na disku a může vyprázdněny, když je virtuální počítač zrušit.</span><span class="sxs-lookup"><span data-stu-id="43d80-186">Note that the local resource disk is a *temporary* disk, and might be emptied when the VM is deprovisioned.</span></span> <span data-ttu-id="43d80-187">Po instalaci Azure Linux Agent (viz předchozí krok), upravte následující parametry v /etc/waagent.conf odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="43d80-187">After installing the Azure Linux Agent (see previous step), modify the following parameters in /etc/waagent.conf appropriately:</span></span>
    
     <span data-ttu-id="43d80-188">ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=&#2048;# Poznámka: tuto možnost nastavíte na ať už budete potřebovat mohla být.</span><span class="sxs-lookup"><span data-stu-id="43d80-188">ResourceDisk.Format=y  ResourceDisk.Filesystem=ext4  ResourceDisk.MountPoint=/mnt/resource  ResourceDisk.EnableSwap=y  ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.</span></span>
11. <span data-ttu-id="43d80-189">Spusťte následující příkaz pro zrušení zřízení virtuálního počítače a jeho přípravu pro zřizování na Azure:</span><span class="sxs-lookup"><span data-stu-id="43d80-189">Run the following commands to deprovision the virtual machine and prepare it for provisioning on Azure:</span></span>
    
    # <a name="sudo-waagent--force--deprovision"></a><span data-ttu-id="43d80-190">sudo příkaz waagent-force - deprovision</span><span class="sxs-lookup"><span data-stu-id="43d80-190">sudo waagent -force -deprovision</span></span>
    # <a name="export-histsize0"></a><span data-ttu-id="43d80-191">Export HISTSIZE = 0</span><span class="sxs-lookup"><span data-stu-id="43d80-191">export HISTSIZE=0</span></span>
    # <a name="logout"></a><span data-ttu-id="43d80-192">odhlášení</span><span class="sxs-lookup"><span data-stu-id="43d80-192">logout</span></span>
12. <span data-ttu-id="43d80-193">Zajistěte, aby že Azure Linux Agent spuštěna při spuštění systému:</span><span class="sxs-lookup"><span data-stu-id="43d80-193">Ensure the Azure Linux Agent runs at startup:</span></span>
    
        # sudo systemctl enable waagent.service
13. <span data-ttu-id="43d80-194">Klikněte na tlačítko **akce -> vypnutí dolů** ve Správci technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="43d80-194">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="43d80-195">Svůj disk VHD Linux je nyní připravena k odeslání do Azure.</span><span class="sxs-lookup"><span data-stu-id="43d80-195">Your Linux VHD is now ready to be uploaded to Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="43d80-196">Další kroky</span><span class="sxs-lookup"><span data-stu-id="43d80-196">Next steps</span></span>
<span data-ttu-id="43d80-197">Nyní jste připraveni použít virtuální pevný disk SUSE Linux k vytvoření nové virtuální počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="43d80-197">You're now ready to use your SUSE Linux virtual hard disk to create new virtual machines in Azure.</span></span> <span data-ttu-id="43d80-198">Pokud je poprvé, že jste nahrávání souboru VHD do Azure, najdete v části kroky 2 a 3 v [vytváření a odesílání virtuální pevný disk, který obsahuje operační systém Linux](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="43d80-198">If this is the first time that you're uploading the .vhd file to Azure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains the Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

