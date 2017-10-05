---
title: "Vytvořit a nahrát VHD na základě CentOS Linux v Azure"
description: "Naučte se vytvořit a odeslat Azure virtuálního pevného disku (VHD) obsahující operační systém na základě CentOS Linux."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 0e518e92-e981-43f4-b12c-9cba1064c4bb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 010f4b05b35fa1f31c14f34a5fae9298fcd831e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="prepare-a-centos-based-virtual-machine-for-azure"></a><span data-ttu-id="a7e29-103">Příprava virtuálního počítače založeného na CentOS pro Azure</span><span class="sxs-lookup"><span data-stu-id="a7e29-103">Prepare a CentOS-based virtual machine for Azure</span></span>
* [<span data-ttu-id="a7e29-104">Příprava virtuálního počítače, CentOS 6.x pro Azure.</span><span class="sxs-lookup"><span data-stu-id="a7e29-104">Prepare a CentOS 6.x virtual machine for Azure</span></span>](#centos-6x)
* [<span data-ttu-id="a7e29-105">Příprava virtuálního počítače, CentOS 7.0 + pro Azure.</span><span class="sxs-lookup"><span data-stu-id="a7e29-105">Prepare a CentOS 7.0+ virtual machine for Azure</span></span>](#centos-70)

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a><span data-ttu-id="a7e29-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a7e29-106">Prerequisites</span></span>
<span data-ttu-id="a7e29-107">Tento článek předpokládá, že jste již nainstalovali CentOS (nebo podobné odvozených) operační systém Linux virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="a7e29-107">This article assumes that you have already installed a CentOS (or similar derivative) Linux operating system to a virtual hard disk.</span></span> <span data-ttu-id="a7e29-108">Postup vytvoření souborů VHD, například řešení virtualizace jako je například technologie Hyper-V existuje několik nástrojů.</span><span class="sxs-lookup"><span data-stu-id="a7e29-108">Multiple tools exist to create .vhd files, for example a virtualization solution such as Hyper-V.</span></span> <span data-ttu-id="a7e29-109">Pokyny najdete v tématu [nainstalovat roli Hyper-V a konfigurace virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="a7e29-109">For instructions, see [Install the Hyper-V Role and Configure a Virtual Machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

<span data-ttu-id="a7e29-110">**Poznámky k instalaci centOS**</span><span class="sxs-lookup"><span data-stu-id="a7e29-110">**CentOS installation notes**</span></span>

* <span data-ttu-id="a7e29-111">Najdete také [obecné poznámky k instalaci Linux](create-upload-generic.md#general-linux-installation-notes) Příprava na Azure Linux další tipy pro.</span><span class="sxs-lookup"><span data-stu-id="a7e29-111">Please see also [General Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more tips on preparing Linux for Azure.</span></span>
* <span data-ttu-id="a7e29-112">Formát VHDX není podporován v Azure, pouze **pevný virtuální pevný disk**.</span><span class="sxs-lookup"><span data-stu-id="a7e29-112">The VHDX format is not supported in Azure, only **fixed VHD**.</span></span>  <span data-ttu-id="a7e29-113">Disk můžete převést do formátu virtuálního pevného disku pomocí Správce technologie Hyper-V nebo rutiny convert-VHD prostředí.</span><span class="sxs-lookup"><span data-stu-id="a7e29-113">You can convert the disk to VHD format using Hyper-V Manager or the convert-vhd cmdlet.</span></span> <span data-ttu-id="a7e29-114">Pokud používáte VirtualBox to znamená výběr **pevnou velikost** oproti výchozí přidělí dynamicky při vytváření disku.</span><span class="sxs-lookup"><span data-stu-id="a7e29-114">If you are using VirtualBox this means selecting **Fixed size** as opposed to the default dynamically allocated when creating the disk.</span></span>
* <span data-ttu-id="a7e29-115">Při instalaci systému Linux je *doporučená* použít standardní oddíly spíše než LVM (často výchozí pro mnoho instalace).</span><span class="sxs-lookup"><span data-stu-id="a7e29-115">When installing the Linux system it is *recommended* that you use standard partitions rather than LVM (often the default for many installations).</span></span> <span data-ttu-id="a7e29-116">Tím se vyhnete LVM název je v konfliktu s klonovaný virtuální počítače, zvlášť pokud někdy musí být připojen k virtuálnímu počítači jiné identické pro řešení potíží s disk s operačním systémem.</span><span class="sxs-lookup"><span data-stu-id="a7e29-116">This will avoid LVM name conflicts with cloned VMs, particularly if an OS disk ever needs to be attached to another identical VM for troubleshooting.</span></span> <span data-ttu-id="a7e29-117">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mohou být použity na datové disky.</span><span class="sxs-lookup"><span data-stu-id="a7e29-117">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks.</span></span>
* <span data-ttu-id="a7e29-118">Vyžaduje se podpora jádra pro připojení systémy souborů UDF.</span><span class="sxs-lookup"><span data-stu-id="a7e29-118">Kernel support for mounting UDF file systems is required.</span></span> <span data-ttu-id="a7e29-119">Při prvním spuštění v Azure je předána zřizování konfigurace do virtuálního počítače s Linuxem pomocí formátu UDF média, který je připojen k Host.</span><span class="sxs-lookup"><span data-stu-id="a7e29-119">At first boot on Azure the provisioning configuration is passed to the Linux VM via UDF-formatted media that is attached to the guest.</span></span> <span data-ttu-id="a7e29-120">Azure Linux agent musí být schopný se připojit a načíst jeho konfiguraci a zřídit virtuální počítač v systému souborů UDF.</span><span class="sxs-lookup"><span data-stu-id="a7e29-120">The Azure Linux agent must be able to mount the UDF file system to read its configuration and provision the VM.</span></span>
* <span data-ttu-id="a7e29-121">Verze jádra Linux pod 2.6.37 nepodporují NUMA v technologii Hyper-V s větší velikostí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a7e29-121">Linux kernel versions below 2.6.37 do not support NUMA on Hyper-V with larger VM sizes.</span></span> <span data-ttu-id="a7e29-122">-Li tento problém ovlivňuje především starší distribuce pomocí nadřazený Red Hat 2.6.32 jádra která byla opravena v RHEL 6.6 (jádra 2.6.32 504).</span><span class="sxs-lookup"><span data-stu-id="a7e29-122">This issue primarily impacts older distributions using the upstream Red Hat 2.6.32 kernel, and was fixed in RHEL 6.6 (kernel-2.6.32-504).</span></span> <span data-ttu-id="a7e29-123">Systémy s operačním systémem vlastní jádra starší než 2.6.37 nebo na základě RHEL jádra starší než 2.6.32-504 musíte nastavit parametr spouštěcího `numa=off` na příkazového řádku v grub.conf jádra.</span><span class="sxs-lookup"><span data-stu-id="a7e29-123">Systems running custom kernels older than 2.6.37, or RHEL-based kernels older than 2.6.32-504 must set the boot parameter `numa=off` on the kernel command-line in grub.conf.</span></span> <span data-ttu-id="a7e29-124">Další informace najdete v části Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span><span class="sxs-lookup"><span data-stu-id="a7e29-124">For more information see Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span></span>
* <span data-ttu-id="a7e29-125">Nekonfigurujte přepnutí oddílu na disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="a7e29-125">Do not configure a swap partition on the OS disk.</span></span> <span data-ttu-id="a7e29-126">Chcete-li vytvořit odkládací soubor na disku dočasných prostředků lze nakonfigurovat agenta systému Linux.</span><span class="sxs-lookup"><span data-stu-id="a7e29-126">The Linux agent can be configured to create a swap file on the temporary resource disk.</span></span>  <span data-ttu-id="a7e29-127">Další informace o této naleznete v následujících krocích.</span><span class="sxs-lookup"><span data-stu-id="a7e29-127">More information about this can be found in the steps below.</span></span>
* <span data-ttu-id="a7e29-128">Všechny virtuální pevné disky musí mít velikostí, které jsou násobky 1 MB.</span><span class="sxs-lookup"><span data-stu-id="a7e29-128">All of the VHDs must have sizes that are multiples of 1 MB.</span></span>

## <a name="centos-6x"></a><span data-ttu-id="a7e29-129">CentOS 6.x</span><span class="sxs-lookup"><span data-stu-id="a7e29-129">CentOS 6.x</span></span>

1. <span data-ttu-id="a7e29-130">Ve Správci technologie Hyper-V vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a7e29-130">In Hyper-V Manager, select the virtual machine.</span></span>

2. <span data-ttu-id="a7e29-131">Klikněte na tlačítko **Connect** k otevření okna konzoly pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a7e29-131">Click **Connect** to open a console window for the virtual machine.</span></span>

3. <span data-ttu-id="a7e29-132">V CentOS 6 NetworkManager může narušovat Azure Linux agent.</span><span class="sxs-lookup"><span data-stu-id="a7e29-132">In CentOS 6, NetworkManager can interfere with the Azure Linux agent.</span></span> <span data-ttu-id="a7e29-133">Odinstalaci tohoto balíčku tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a7e29-133">Uninstall this package by running the following command:</span></span>
   
        # sudo rpm -e --nodeps NetworkManager

4. <span data-ttu-id="a7e29-134">Vytvořte nebo upravte soubor `/etc/sysconfig/network` a přidejte následující text:</span><span class="sxs-lookup"><span data-stu-id="a7e29-134">Create or edit the file `/etc/sysconfig/network` and add the following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. <span data-ttu-id="a7e29-135">Vytvořte nebo upravte soubor `/etc/sysconfig/network-scripts/ifcfg-eth0` a přidejte následující text:</span><span class="sxs-lookup"><span data-stu-id="a7e29-135">Create or edit the file `/etc/sysconfig/network-scripts/ifcfg-eth0` and add the following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6. <span data-ttu-id="a7e29-136">Změňte pravidla udev, aby nedošlo ke generování statická pravidla pro alespoň jedno rozhraní sítě Ethernet.</span><span class="sxs-lookup"><span data-stu-id="a7e29-136">Modify udev rules to avoid generating static rules for the Ethernet interface(s).</span></span> <span data-ttu-id="a7e29-137">Tato pravidla může způsobit problémy při klonování virtuálního počítače v Microsoft Azure nebo Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="a7e29-137">These rules can cause problems when cloning a virtual machine in Microsoft Azure or Hyper-V:</span></span>
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. <span data-ttu-id="a7e29-138">Zajistěte, aby že síťové služby se spustí při spuštění spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="a7e29-138">Ensure the network service will start at boot time by running the following command:</span></span>
   
        # sudo chkconfig network on

8. <span data-ttu-id="a7e29-139">Pokud chcete použít OpenLogic zrcadlení, které jsou hostované v datových centrech Azure, a potom nahraďte `/etc/yum.repos.d/CentOS-Base.repo` soubor s následující úložiště.</span><span class="sxs-lookup"><span data-stu-id="a7e29-139">If you would like to use the OpenLogic mirrors that are hosted within the Azure datacenters, then replace the `/etc/yum.repos.d/CentOS-Base.repo` file with the following repositories.</span></span>  <span data-ttu-id="a7e29-140">Taky se přidá **[openlogic]** úložiště, který zahrnuje další balíčků, jako jsou Azure Linux agent:</span><span class="sxs-lookup"><span data-stu-id="a7e29-140">This will also add the **[openlogic]** repository that includes additional packages such as the Azure Linux agent:</span></span>

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0
        
        [base]
        name=CentOS-$releasever - Base
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
        
        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
        
        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
        
        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=contrib&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

    >[!Note]
    <span data-ttu-id="a7e29-141">Zbývající části této příručky bude předpokládat, že používáte alespoň `[openlogic]` úložišti, který se použije k instalaci níže Azure Linux agent.</span><span class="sxs-lookup"><span data-stu-id="a7e29-141">The rest of this guide will assume you are using at least the `[openlogic]` repo, which will be used to install the Azure Linux agent below.</span></span>


9. <span data-ttu-id="a7e29-142">Přidejte následující řádek na /etc/yum.conf:</span><span class="sxs-lookup"><span data-stu-id="a7e29-142">Add the following line to /etc/yum.conf:</span></span>
    
        http_caching=packages

10. <span data-ttu-id="a7e29-143">Spusťte následující příkaz, který zrušte aktuální metadata yum a aktualizujte systém s nejnovější balíčky:</span><span class="sxs-lookup"><span data-stu-id="a7e29-143">Run the following command to clear the current yum metadata and update the system with the latest packages:</span></span>
    
        # yum clean all

    <span data-ttu-id="a7e29-144">Pokud chcete vytvořit bitovou kopii pro starší verze CentOS, doporučujeme aktualizovat všechny balíčky na nejnovější:</span><span class="sxs-lookup"><span data-stu-id="a7e29-144">Unless you are creating an image for an older version of CentOS, it is recommended to update all the packages to the latest:</span></span>

        # sudo yum -y update

    <span data-ttu-id="a7e29-145">Po spuštění tohoto příkazu může být nutný restart.</span><span class="sxs-lookup"><span data-stu-id="a7e29-145">A reboot may be required after running this command.</span></span>

11. <span data-ttu-id="a7e29-146">(Volitelné) Nainstalujte ovladače pro integraci služby Linux (LIS).</span><span class="sxs-lookup"><span data-stu-id="a7e29-146">(Optional) Install the drivers for the Linux Integration Services (LIS).</span></span>
   
    >[!IMPORTANT]
    <span data-ttu-id="a7e29-147">V kroku **požadované** pro CentOS 6.3 a starší a volitelné pro pozdějších verzích.</span><span class="sxs-lookup"><span data-stu-id="a7e29-147">The step is **required** for CentOS 6.3 and earlier, and optional for later releases.</span></span>

        # sudo rpm -e hypervkvpd  ## (may return error if not installed, that's OK)
        # sudo yum install microsoft-hyper-v

    <span data-ttu-id="a7e29-148">Alternativně můžete podle pokynů ruční instalace na [stránce pro stažení LIS](https://go.microsoft.com/fwlink/?linkid=403033) k instalaci RPM do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a7e29-148">Alternatively, you can follow the manual installation instructions on the [LIS download page](https://go.microsoft.com/fwlink/?linkid=403033) to install the RPM onto your VM.</span></span>
 
12. <span data-ttu-id="a7e29-149">Instalace Azure Linux Agent a závislosti:</span><span class="sxs-lookup"><span data-stu-id="a7e29-149">Install the Azure Linux Agent and dependencies:</span></span>
    
        # sudo yum install python-pyasn1 WALinuxAgent
    
    <span data-ttu-id="a7e29-150">Odebere balíček WALinuxAgent NetworkManager a balíčky NetworkManager gnome Pokud nebyly již odebrána jak je popsáno v kroku 3.</span><span class="sxs-lookup"><span data-stu-id="a7e29-150">The WALinuxAgent package will remove the NetworkManager and NetworkManager-gnome packages if they were not already removed as described in step 3.</span></span>


13. <span data-ttu-id="a7e29-151">Upravte řádku spouštěcí jádra ve vaší konfiguraci grub pro zahrnutí dalších jádra parametry Azure.</span><span class="sxs-lookup"><span data-stu-id="a7e29-151">Modify the kernel boot line in your grub configuration to include additional kernel parameters for Azure.</span></span> <span data-ttu-id="a7e29-152">Chcete-li to provést, otevřete `/boot/grub/menu.lst` v textovém editoru a ujistěte se, že výchozí jádra zahrnuje následující parametry:</span><span class="sxs-lookup"><span data-stu-id="a7e29-152">To do this, open `/boot/grub/menu.lst` in a text editor and ensure that the default kernel includes the following parameters:</span></span>
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    <span data-ttu-id="a7e29-153">To také zajistí, všechny zprávy konzoly odešlou do první sériového portu, který může být užitečné Azure podporu pro ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="a7e29-153">This will also ensure all console messages are sent to the first serial port, which can assist Azure support with debugging issues.</span></span>
    
    <span data-ttu-id="a7e29-154">Kromě výše uvedeného, doporučuje se *odebrat* následující parametry:</span><span class="sxs-lookup"><span data-stu-id="a7e29-154">In addition to the above, it is recommended to *remove* the following parameters:</span></span>
    
        rhgb quiet crashkernel=auto
    
    <span data-ttu-id="a7e29-155">Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly pro odeslání do sériového portu.</span><span class="sxs-lookup"><span data-stu-id="a7e29-155">Graphical and quiet boot are not useful in a cloud environment where we want all the logs to be sent to the serial port.</span></span>  <span data-ttu-id="a7e29-156">`crashkernel` Možnost může být vlevo nakonfigurované v případě potřeby, ale Všimněte si, že tento parametr se sníží množství dostupné paměti ve virtuálním počítači 128 MB nebo informace, které mohou způsobovat menší velikostí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a7e29-156">The `crashkernel` option may be left configured if desired, but note that this parameter will reduce the amount of available memory in the VM by 128MB or more, which may be problematic on the smaller VM sizes.</span></span>

    >[!Important]
    <span data-ttu-id="a7e29-157">CentOS verze 6.5 a starší, musíte taky nastavit parametr jádra `numa=off`.</span><span class="sxs-lookup"><span data-stu-id="a7e29-157">CentOS 6.5 and earlier must also set the kernel parameter `numa=off`.</span></span> <span data-ttu-id="a7e29-158">V tématu Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span><span class="sxs-lookup"><span data-stu-id="a7e29-158">See Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span></span>

14. <span data-ttu-id="a7e29-159">Ujistěte se, že SSH server je nainstalován a nakonfigurován na spuštění při spuštění.</span><span class="sxs-lookup"><span data-stu-id="a7e29-159">Ensure that the SSH server is installed and configured to start at boot time.</span></span>  <span data-ttu-id="a7e29-160">Obvykle je to výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="a7e29-160">This is usually the default.</span></span>

15. <span data-ttu-id="a7e29-161">Nevytvářejte odkládacího prostoru na disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="a7e29-161">Do not create swap space on the OS disk.</span></span>
    
    <span data-ttu-id="a7e29-162">Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku místní prostředek, který je připojen k virtuálnímu počítači po zřízení v Azure.</span><span class="sxs-lookup"><span data-stu-id="a7e29-162">The Azure Linux Agent can automatically configure swap space using the local resource disk that is attached to the VM after provisioning on Azure.</span></span> <span data-ttu-id="a7e29-163">Všimněte si, že je disk místní prostředek *dočasné* na disku a může vyprázdněny, když je virtuální počítač zrušit.</span><span class="sxs-lookup"><span data-stu-id="a7e29-163">Note that the local resource disk is a *temporary* disk, and might be emptied when the VM is deprovisioned.</span></span> <span data-ttu-id="a7e29-164">Po instalaci Azure Linux Agent (viz předchozí krok), upravte následující parametry v `/etc/waagent.conf` odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a7e29-164">After installing the Azure Linux Agent (see previous step), modify the following parameters in `/etc/waagent.conf` appropriately:</span></span>
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. <span data-ttu-id="a7e29-165">Spusťte následující příkaz pro zrušení zřízení virtuálního počítače a jeho přípravu pro zřizování na Azure:</span><span class="sxs-lookup"><span data-stu-id="a7e29-165">Run the following commands to deprovision the virtual machine and prepare it for provisioning on Azure:</span></span>
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

17. <span data-ttu-id="a7e29-166">Klikněte na tlačítko **akce -> vypnutí dolů** ve Správci technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="a7e29-166">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="a7e29-167">Svůj disk VHD Linux je nyní připravena k odeslání do Azure.</span><span class="sxs-lookup"><span data-stu-id="a7e29-167">Your Linux VHD is now ready to be uploaded to Azure.</span></span>


- - -
## <a name="centos-70"></a><span data-ttu-id="a7e29-168">CentOS 7.0 +</span><span class="sxs-lookup"><span data-stu-id="a7e29-168">CentOS 7.0+</span></span>
<span data-ttu-id="a7e29-169">**Změny v CentOS 7 (a podobné odvozené konfigurace)**</span><span class="sxs-lookup"><span data-stu-id="a7e29-169">**Changes in CentOS 7 (and similar derivatives)**</span></span>

<span data-ttu-id="a7e29-170">Příprava CentOS 7 virtuálního počítače na Azure je velmi podobné CentOS 6, ale existuje několik důležitých rozdílů vhodné poznamenat:</span><span class="sxs-lookup"><span data-stu-id="a7e29-170">Preparing a CentOS 7 virtual machine for Azure is very similar to CentOS 6, however there are several important differences worth noting:</span></span>

* <span data-ttu-id="a7e29-171">NetworkManager balíčku už v konfliktu s Azure Linux agent.</span><span class="sxs-lookup"><span data-stu-id="a7e29-171">The NetworkManager package no longer conflicts with the Azure Linux agent.</span></span> <span data-ttu-id="a7e29-172">Tento balíček je ve výchozím nastavení nainstalovaná a doporučujeme vám, že se neodebere.</span><span class="sxs-lookup"><span data-stu-id="a7e29-172">This package is installed by default and we recommend that it is not removed.</span></span>
* <span data-ttu-id="a7e29-173">GRUB2 se teď používá jako výchozí zaváděcí program pro spouštění, tak postup úpravy parametry jádra se změnila (viz níže).</span><span class="sxs-lookup"><span data-stu-id="a7e29-173">GRUB2 is now used as the default bootloader, so the procedure for editing kernel parameters has changed (see below).</span></span>
* <span data-ttu-id="a7e29-174">XFS je nyní na výchozí systém souborů.</span><span class="sxs-lookup"><span data-stu-id="a7e29-174">XFS is now the default file system.</span></span> <span data-ttu-id="a7e29-175">Systém souborů ext4 pořád můžou použít v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="a7e29-175">The ext4 file system can still be used if desired.</span></span>

<span data-ttu-id="a7e29-176">**Kroky konfigurace**</span><span class="sxs-lookup"><span data-stu-id="a7e29-176">**Configuration Steps**</span></span>

1. <span data-ttu-id="a7e29-177">Ve Správci technologie Hyper-V vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a7e29-177">In Hyper-V Manager, select the virtual machine.</span></span>

2. <span data-ttu-id="a7e29-178">Klikněte na tlačítko **Connect** k otevření okna konzoly pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a7e29-178">Click **Connect** to open a console window for the virtual machine.</span></span>

3. <span data-ttu-id="a7e29-179">Vytvořte nebo upravte soubor `/etc/sysconfig/network` a přidejte následující text:</span><span class="sxs-lookup"><span data-stu-id="a7e29-179">Create or edit the file `/etc/sysconfig/network` and add the following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4. <span data-ttu-id="a7e29-180">Vytvořte nebo upravte soubor `/etc/sysconfig/network-scripts/ifcfg-eth0` a přidejte následující text:</span><span class="sxs-lookup"><span data-stu-id="a7e29-180">Create or edit the file `/etc/sysconfig/network-scripts/ifcfg-eth0` and add the following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

5. <span data-ttu-id="a7e29-181">Změňte pravidla udev, aby nedošlo ke generování statická pravidla pro alespoň jedno rozhraní sítě Ethernet.</span><span class="sxs-lookup"><span data-stu-id="a7e29-181">Modify udev rules to avoid generating static rules for the Ethernet interface(s).</span></span> <span data-ttu-id="a7e29-182">Tato pravidla může způsobit problémy při klonování virtuálního počítače v Microsoft Azure nebo Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="a7e29-182">These rules can cause problems when cloning a virtual machine in Microsoft Azure or Hyper-V:</span></span>
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. <span data-ttu-id="a7e29-183">Pokud chcete použít OpenLogic zrcadlení, které jsou hostované v datových centrech Azure, a potom nahraďte `/etc/yum.repos.d/CentOS-Base.repo` soubor s následující úložiště.</span><span class="sxs-lookup"><span data-stu-id="a7e29-183">If you would like to use the OpenLogic mirrors that are hosted within the Azure datacenters, then replace the `/etc/yum.repos.d/CentOS-Base.repo` file with the following repositories.</span></span>  <span data-ttu-id="a7e29-184">Taky se přidá **[openlogic]** úložiště, který obsahuje balíčky pro Azure Linux agent:</span><span class="sxs-lookup"><span data-stu-id="a7e29-184">This will also add the **[openlogic]** repository that includes packages for the Azure Linux agent:</span></span>
   
        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0
        
        [base]
        name=CentOS-$releasever - Base
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
        
        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
        
        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
        
        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

    >[!Note]
    <span data-ttu-id="a7e29-185">Zbývající části této příručky bude předpokládat, že používáte alespoň `[openlogic]` úložišti, který se použije k instalaci níže Azure Linux agent.</span><span class="sxs-lookup"><span data-stu-id="a7e29-185">The rest of this guide will assume you are using at least the `[openlogic]` repo, which will be used to install the Azure Linux agent below.</span></span>

7. <span data-ttu-id="a7e29-186">Spusťte následující příkaz, který zrušte aktuální metadata yum a nainstalujte všechny aktualizace:</span><span class="sxs-lookup"><span data-stu-id="a7e29-186">Run the following command to clear the current yum metadata and install any updates:</span></span>
   
        # sudo yum clean all

    <span data-ttu-id="a7e29-187">Pokud chcete vytvořit bitovou kopii pro starší verze CentOS, doporučujeme aktualizovat všechny balíčky na nejnovější:</span><span class="sxs-lookup"><span data-stu-id="a7e29-187">Unless you are creating an image for an older version of CentOS, it is recommended to update all the packages to the latest:</span></span>

        # sudo yum -y update

    <span data-ttu-id="a7e29-188">Restartování, může být nutné po spuštění tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="a7e29-188">A reboot maybe required after running this command.</span></span>

8. <span data-ttu-id="a7e29-189">Upravte řádku spouštěcí jádra ve vaší konfiguraci grub pro zahrnutí dalších jádra parametry Azure.</span><span class="sxs-lookup"><span data-stu-id="a7e29-189">Modify the kernel boot line in your grub configuration to include additional kernel parameters for Azure.</span></span> <span data-ttu-id="a7e29-190">Chcete-li to provést, otevřete `/etc/default/grub` v textovém editoru a upravit `GRUB_CMDLINE_LINUX` parametr, například:</span><span class="sxs-lookup"><span data-stu-id="a7e29-190">To do this, open `/etc/default/grub` in a text editor and edit the `GRUB_CMDLINE_LINUX` parameter, for example:</span></span>
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   <span data-ttu-id="a7e29-191">To také zajistí, všechny zprávy konzoly odešlou do první sériového portu, který může být užitečné Azure podporu pro ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="a7e29-191">This will also ensure all console messages are sent to the first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="a7e29-192">Je také vypne nové zásady vytváření názvů CentOS 7 pro síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="a7e29-192">It also turns off the new CentOS 7 naming conventions for NICs.</span></span> <span data-ttu-id="a7e29-193">Kromě výše uvedeného, doporučuje se *odebrat* následující parametry:</span><span class="sxs-lookup"><span data-stu-id="a7e29-193">In addition to the above, it is recommended to *remove* the following parameters:</span></span>
   
        rhgb quiet crashkernel=auto
   
    <span data-ttu-id="a7e29-194">Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly pro odeslání do sériového portu.</span><span class="sxs-lookup"><span data-stu-id="a7e29-194">Graphical and quiet boot are not useful in a cloud environment where we want all the logs to be sent to the serial port.</span></span> <span data-ttu-id="a7e29-195">`crashkernel` Možnost může být vlevo nakonfigurované v případě potřeby, ale Všimněte si, že tento parametr se sníží množství dostupné paměti ve virtuálním počítači 128 MB nebo informace, které mohou způsobovat menší velikostí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a7e29-195">The `crashkernel` option may be left configured if desired, but note that this parameter will reduce the amount of available memory in the VM by 128MB or more, which may be problematic on the smaller VM sizes.</span></span>

9. <span data-ttu-id="a7e29-196">Po dokončení úprav `/etc/default/grub` za výše, spusťte následující příkaz k opětovnému sestavení grub konfigurace:</span><span class="sxs-lookup"><span data-stu-id="a7e29-196">Once you are done editing `/etc/default/grub` per above, run the following command to rebuild the grub configuration:</span></span>
   
        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

10. <span data-ttu-id="a7e29-197">Pokud vytváření bitové kopie z **VMWare, VirtualBox nebo KVM:** ovladače zajistěte, aby technologie Hyper-V jsou součástí initramfs:</span><span class="sxs-lookup"><span data-stu-id="a7e29-197">If building the image from **VMWare, VirtualBox or KVM:** Ensure the Hyper-V drivers are included in the initramfs:</span></span>
   
   <span data-ttu-id="a7e29-198">Upravit `/etc/dracut.conf`, přidejte obsah:</span><span class="sxs-lookup"><span data-stu-id="a7e29-198">Edit `/etc/dracut.conf`, add content:</span></span>
   
        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”
   
   <span data-ttu-id="a7e29-199">Znovu sestavte initramfs:</span><span class="sxs-lookup"><span data-stu-id="a7e29-199">Rebuild the initramfs:</span></span>
   
        # sudo dracut –f -v

11. <span data-ttu-id="a7e29-200">Instalace Azure Linux Agent a závislosti:</span><span class="sxs-lookup"><span data-stu-id="a7e29-200">Install the Azure Linux Agent and dependencies:</span></span>

        # sudo yum install python-pyasn1 WALinuxAgent
        # sudo systemctl enable waagent

12. <span data-ttu-id="a7e29-201">Nevytvářejte odkládacího prostoru na disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="a7e29-201">Do not create swap space on the OS disk.</span></span>
   
   <span data-ttu-id="a7e29-202">Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku místní prostředek, který je připojen k virtuálnímu počítači po zřízení v Azure.</span><span class="sxs-lookup"><span data-stu-id="a7e29-202">The Azure Linux Agent can automatically configure swap space using the local resource disk that is attached to the VM after provisioning on Azure.</span></span> <span data-ttu-id="a7e29-203">Všimněte si, že je disk místní prostředek *dočasné* na disku a může vyprázdněny, když je virtuální počítač zrušit.</span><span class="sxs-lookup"><span data-stu-id="a7e29-203">Note that the local resource disk is a *temporary* disk, and might be emptied when the VM is deprovisioned.</span></span> <span data-ttu-id="a7e29-204">Po instalaci Azure Linux Agent (viz předchozí krok), upravte následující parametry v `/etc/waagent.conf` odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a7e29-204">After installing the Azure Linux Agent (see previous step), modify the following parameters in `/etc/waagent.conf` appropriately:</span></span>
   
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. <span data-ttu-id="a7e29-205">Spusťte následující příkaz pro zrušení zřízení virtuálního počítače a jeho přípravu pro zřizování na Azure:</span><span class="sxs-lookup"><span data-stu-id="a7e29-205">Run the following commands to deprovision the virtual machine and prepare it for provisioning on Azure:</span></span>
   
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. <span data-ttu-id="a7e29-206">Klikněte na tlačítko **akce -> vypnutí dolů** ve Správci technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="a7e29-206">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="a7e29-207">Svůj disk VHD Linux je nyní připravena k odeslání do Azure.</span><span class="sxs-lookup"><span data-stu-id="a7e29-207">Your Linux VHD is now ready to be uploaded to Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a7e29-208">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a7e29-208">Next steps</span></span>
<span data-ttu-id="a7e29-209">Nyní jste připraveni použít systému CentOS Linux virtuální pevný disk k vytvoření nové virtuální počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="a7e29-209">You're now ready to use your CentOS Linux virtual hard disk to create new virtual machines in Azure.</span></span> <span data-ttu-id="a7e29-210">Pokud je poprvé, že jste nahrávání souboru VHD do Azure, najdete v části kroky 2 a 3 v [vytváření a odesílání virtuální pevný disk, který obsahuje operační systém Linux](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a7e29-210">If this is the first time that you're uploading the .vhd file to Azure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains the Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

