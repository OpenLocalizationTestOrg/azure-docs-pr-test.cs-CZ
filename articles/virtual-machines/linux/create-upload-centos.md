---
title: "aaaCreate a nahrajte na základě CentOS Linux virtuální pevný disk v Azure"
description: "Další informace toocreate a nahrajte Azure virtuálního pevného disku (VHD) obsahující operační systém na základě CentOS Linux."
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
ms.openlocfilehash: 78f36be1d36e3d44cb836c912d143f2c9a72aab5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-a-centos-based-virtual-machine-for-azure"></a><span data-ttu-id="be1da-103">Příprava virtuálního počítače založeného na CentOS pro Azure</span><span class="sxs-lookup"><span data-stu-id="be1da-103">Prepare a CentOS-based virtual machine for Azure</span></span>
* [<span data-ttu-id="be1da-104">Příprava virtuálního počítače, CentOS 6.x pro Azure.</span><span class="sxs-lookup"><span data-stu-id="be1da-104">Prepare a CentOS 6.x virtual machine for Azure</span></span>](#centos-6x)
* [<span data-ttu-id="be1da-105">Příprava virtuálního počítače, CentOS 7.0 + pro Azure.</span><span class="sxs-lookup"><span data-stu-id="be1da-105">Prepare a CentOS 7.0+ virtual machine for Azure</span></span>](#centos-70)

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a><span data-ttu-id="be1da-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="be1da-106">Prerequisites</span></span>
<span data-ttu-id="be1da-107">Tento článek předpokládá, že jste již nainstalovali CentOS (nebo podobné odvozených) operačního systému Linux tooa virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="be1da-107">This article assumes that you have already installed a CentOS (or similar derivative) Linux operating system tooa virtual hard disk.</span></span> <span data-ttu-id="be1da-108">Několik nástrojů existovat toocreate soubory VHD, například řešení virtualizace jako je například technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="be1da-108">Multiple tools exist toocreate .vhd files, for example a virtualization solution such as Hyper-V.</span></span> <span data-ttu-id="be1da-109">Pokyny najdete v tématu [instalace hello Role Hyper-V a konfigurace virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="be1da-109">For instructions, see [Install hello Hyper-V Role and Configure a Virtual Machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

<span data-ttu-id="be1da-110">**Poznámky k instalaci centOS**</span><span class="sxs-lookup"><span data-stu-id="be1da-110">**CentOS installation notes**</span></span>

* <span data-ttu-id="be1da-111">Najdete také [obecné poznámky k instalaci Linux](create-upload-generic.md#general-linux-installation-notes) Příprava na Azure Linux další tipy pro.</span><span class="sxs-lookup"><span data-stu-id="be1da-111">Please see also [General Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more tips on preparing Linux for Azure.</span></span>
* <span data-ttu-id="be1da-112">Formát VHDX Hello není podporován v Azure, pouze **pevný virtuální pevný disk**.</span><span class="sxs-lookup"><span data-stu-id="be1da-112">hello VHDX format is not supported in Azure, only **fixed VHD**.</span></span>  <span data-ttu-id="be1da-113">Můžete převést formát tooVHD hello disku pomocí Správce technologie Hyper-V nebo hello rutiny convert-VHD prostředí.</span><span class="sxs-lookup"><span data-stu-id="be1da-113">You can convert hello disk tooVHD format using Hyper-V Manager or hello convert-vhd cmdlet.</span></span> <span data-ttu-id="be1da-114">Pokud používáte VirtualBox to znamená výběr **pevnou velikost** jako byl proti toohello výchozí přidělí dynamicky při vytváření disku hello.</span><span class="sxs-lookup"><span data-stu-id="be1da-114">If you are using VirtualBox this means selecting **Fixed size** as opposed toohello default dynamically allocated when creating hello disk.</span></span>
* <span data-ttu-id="be1da-115">Při instalaci systému Linux hello je *doporučená* použít standardní oddíly spíše než LVM (často hello výchozí pro mnoho instalace).</span><span class="sxs-lookup"><span data-stu-id="be1da-115">When installing hello Linux system it is *recommended* that you use standard partitions rather than LVM (often hello default for many installations).</span></span> <span data-ttu-id="be1da-116">Tím se vyhnete LVM název je v konfliktu s klonovaný virtuální počítače, zvlášť pokud disk s operačním systémem někdy potřebuje toobe připojené tooanother identické virtuálního počítače pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="be1da-116">This will avoid LVM name conflicts with cloned VMs, particularly if an OS disk ever needs toobe attached tooanother identical VM for troubleshooting.</span></span> <span data-ttu-id="be1da-117">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mohou být použity na datové disky.</span><span class="sxs-lookup"><span data-stu-id="be1da-117">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks.</span></span>
* <span data-ttu-id="be1da-118">Vyžaduje se podpora jádra pro připojení systémy souborů UDF.</span><span class="sxs-lookup"><span data-stu-id="be1da-118">Kernel support for mounting UDF file systems is required.</span></span> <span data-ttu-id="be1da-119">Při prvním spuštění v Azure hello je předána zřizování konfigurace toohello virtuálního počítače s Linuxem pomocí formátu UDF média, který je připojený toohello hosta.</span><span class="sxs-lookup"><span data-stu-id="be1da-119">At first boot on Azure hello provisioning configuration is passed toohello Linux VM via UDF-formatted media that is attached toohello guest.</span></span> <span data-ttu-id="be1da-120">Hello Azure Linux agent musí být schopný toomount hello UDF soubor systému tooread své konfiguraci a zřídit hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="be1da-120">hello Azure Linux agent must be able toomount hello UDF file system tooread its configuration and provision hello VM.</span></span>
* <span data-ttu-id="be1da-121">Verze jádra Linux pod 2.6.37 nepodporují NUMA v technologii Hyper-V s větší velikostí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="be1da-121">Linux kernel versions below 2.6.37 do not support NUMA on Hyper-V with larger VM sizes.</span></span> <span data-ttu-id="be1da-122">-Li tento problém ovlivňuje především starší distribuce pomocí hello proti proudu Red Hat 2.6.32 jádra která byla opravena v RHEL 6.6 (jádra 2.6.32 504).</span><span class="sxs-lookup"><span data-stu-id="be1da-122">This issue primarily impacts older distributions using hello upstream Red Hat 2.6.32 kernel, and was fixed in RHEL 6.6 (kernel-2.6.32-504).</span></span> <span data-ttu-id="be1da-123">Systémy s operačním systémem vlastní jádra starší než 2.6.37 nebo na základě RHEL jádra starší, než se musí nastavit 2.6.32-504 hello parametr spouštěcího `numa=off` na příkazového řádku v grub.conf hello jádra.</span><span class="sxs-lookup"><span data-stu-id="be1da-123">Systems running custom kernels older than 2.6.37, or RHEL-based kernels older than 2.6.32-504 must set hello boot parameter `numa=off` on hello kernel command-line in grub.conf.</span></span> <span data-ttu-id="be1da-124">Další informace najdete v části Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span><span class="sxs-lookup"><span data-stu-id="be1da-124">For more information see Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span></span>
* <span data-ttu-id="be1da-125">Nekonfigurujte přepnutí oddílu na disku operačního systému hello.</span><span class="sxs-lookup"><span data-stu-id="be1da-125">Do not configure a swap partition on hello OS disk.</span></span> <span data-ttu-id="be1da-126">Hello Linux agent může být nakonfigurované toocreate odkládací soubor na disku hello dočasných prostředků.</span><span class="sxs-lookup"><span data-stu-id="be1da-126">hello Linux agent can be configured toocreate a swap file on hello temporary resource disk.</span></span>  <span data-ttu-id="be1da-127">Další informace o této naleznete v následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="be1da-127">More information about this can be found in hello steps below.</span></span>
* <span data-ttu-id="be1da-128">Všechny virtuální pevné disky hello musí mít velikostí, které jsou násobky 1 MB.</span><span class="sxs-lookup"><span data-stu-id="be1da-128">All of hello VHDs must have sizes that are multiples of 1 MB.</span></span>

## <a name="centos-6x"></a><span data-ttu-id="be1da-129">CentOS 6.x</span><span class="sxs-lookup"><span data-stu-id="be1da-129">CentOS 6.x</span></span>

1. <span data-ttu-id="be1da-130">Ve Správci technologie Hyper-V vyberte hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="be1da-130">In Hyper-V Manager, select hello virtual machine.</span></span>

2. <span data-ttu-id="be1da-131">Klikněte na tlačítko **připojit** tooopen okna konzoly pro virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="be1da-131">Click **Connect** tooopen a console window for hello virtual machine.</span></span>

3. <span data-ttu-id="be1da-132">V CentOS 6 NetworkManager může narušovat hello Azure Linux agent.</span><span class="sxs-lookup"><span data-stu-id="be1da-132">In CentOS 6, NetworkManager can interfere with hello Azure Linux agent.</span></span> <span data-ttu-id="be1da-133">Odinstalaci tohoto balíčku spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="be1da-133">Uninstall this package by running hello following command:</span></span>
   
        # sudo rpm -e --nodeps NetworkManager

4. <span data-ttu-id="be1da-134">Vytvořte nebo upravte soubor hello `/etc/sysconfig/network` a přidejte hello následující text:</span><span class="sxs-lookup"><span data-stu-id="be1da-134">Create or edit hello file `/etc/sysconfig/network` and add hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. <span data-ttu-id="be1da-135">Vytvořte nebo upravte soubor hello `/etc/sysconfig/network-scripts/ifcfg-eth0` a přidejte hello následující text:</span><span class="sxs-lookup"><span data-stu-id="be1da-135">Create or edit hello file `/etc/sysconfig/network-scripts/ifcfg-eth0` and add hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6. <span data-ttu-id="be1da-136">Upravte tooavoid udev pravidla generování statická pravidla pro hello alespoň jedno rozhraní sítě Ethernet.</span><span class="sxs-lookup"><span data-stu-id="be1da-136">Modify udev rules tooavoid generating static rules for hello Ethernet interface(s).</span></span> <span data-ttu-id="be1da-137">Tato pravidla může způsobit problémy při klonování virtuálního počítače v Microsoft Azure nebo Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="be1da-137">These rules can cause problems when cloning a virtual machine in Microsoft Azure or Hyper-V:</span></span>
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. <span data-ttu-id="be1da-138">Zajistěte, aby hello síťová služba se spustí při spuštění spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="be1da-138">Ensure hello network service will start at boot time by running hello following command:</span></span>
   
        # sudo chkconfig network on

8. <span data-ttu-id="be1da-139">Pokud chcete toouse hello OpenLogic zrcadlení, které jsou hostované v rámci hello datových centrech Azure, potom můžete nahradit hello `/etc/yum.repos.d/CentOS-Base.repo` soubor s hello následující úložiště.</span><span class="sxs-lookup"><span data-stu-id="be1da-139">If you would like toouse hello OpenLogic mirrors that are hosted within hello Azure datacenters, then replace hello `/etc/yum.repos.d/CentOS-Base.repo` file with hello following repositories.</span></span>  <span data-ttu-id="be1da-140">Taky se přidá hello **[openlogic]** úložiště, který zahrnuje další balíčků, jako jsou hello Azure Linux agent:</span><span class="sxs-lookup"><span data-stu-id="be1da-140">This will also add hello **[openlogic]** repository that includes additional packages such as hello Azure Linux agent:</span></span>

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
    <span data-ttu-id="be1da-141">Hello zbývající části této příručky bude předpokládat, že používáte alespoň hello `[openlogic]` úložišti, který bude použité tooinstall hello Azure Linux agent níže.</span><span class="sxs-lookup"><span data-stu-id="be1da-141">hello rest of this guide will assume you are using at least hello `[openlogic]` repo, which will be used tooinstall hello Azure Linux agent below.</span></span>


9. <span data-ttu-id="be1da-142">Přidejte následující řádek too/etc/yum.conf hello:</span><span class="sxs-lookup"><span data-stu-id="be1da-142">Add hello following line too/etc/yum.conf:</span></span>
    
        http_caching=packages

10. <span data-ttu-id="be1da-143">Spusťte následující příkaz tooclear hello aktuální yum metadata a aktualizace hello systém pomocí nejnovější balíčků hello hello:</span><span class="sxs-lookup"><span data-stu-id="be1da-143">Run hello following command tooclear hello current yum metadata and update hello system with hello latest packages:</span></span>
    
        # yum clean all

    <span data-ttu-id="be1da-144">Pokud chcete vytvořit bitovou kopii pro starší verze CentOS, doporučuje se, že tooupdate, které všechny hello balíčky toohello nejnovější:</span><span class="sxs-lookup"><span data-stu-id="be1da-144">Unless you are creating an image for an older version of CentOS, it is recommended tooupdate all hello packages toohello latest:</span></span>

        # sudo yum -y update

    <span data-ttu-id="be1da-145">Po spuštění tohoto příkazu může být nutný restart.</span><span class="sxs-lookup"><span data-stu-id="be1da-145">A reboot may be required after running this command.</span></span>

11. <span data-ttu-id="be1da-146">(Volitelné) Nainstalujte hello ovladače pro hello Linux integrační služby (LIS).</span><span class="sxs-lookup"><span data-stu-id="be1da-146">(Optional) Install hello drivers for hello Linux Integration Services (LIS).</span></span>
   
    >[!IMPORTANT]
    <span data-ttu-id="be1da-147">Krok Hello je **požadované** pro CentOS 6.3 a starší a volitelné pro pozdějších verzích.</span><span class="sxs-lookup"><span data-stu-id="be1da-147">hello step is **required** for CentOS 6.3 and earlier, and optional for later releases.</span></span>

        # sudo rpm -e hypervkvpd  ## (may return error if not installed, that's OK)
        # sudo yum install microsoft-hyper-v

    <span data-ttu-id="be1da-148">Alternativně můžete postupujte podle pokynů pro ruční instalaci hello na hello [stránce pro stažení LIS](https://go.microsoft.com/fwlink/?linkid=403033) tooinstall hello RPM do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="be1da-148">Alternatively, you can follow hello manual installation instructions on hello [LIS download page](https://go.microsoft.com/fwlink/?linkid=403033) tooinstall hello RPM onto your VM.</span></span>
 
12. <span data-ttu-id="be1da-149">Nainstalujte hello Azure Linux Agent a závislosti:</span><span class="sxs-lookup"><span data-stu-id="be1da-149">Install hello Azure Linux Agent and dependencies:</span></span>
    
        # sudo yum install python-pyasn1 WALinuxAgent
    
    <span data-ttu-id="be1da-150">balíček WALinuxAgent Hello odstraní hello NetworkManager a NetworkManager gnome balíčky, pokud nebyly již odebrána popsaný v kroku 3.</span><span class="sxs-lookup"><span data-stu-id="be1da-150">hello WALinuxAgent package will remove hello NetworkManager and NetworkManager-gnome packages if they were not already removed as described in step 3.</span></span>


13. <span data-ttu-id="be1da-151">Upravte hello jádra spouštěcí řádku grub konfigurace tooinclude další jádra parametry pro Azure.</span><span class="sxs-lookup"><span data-stu-id="be1da-151">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="be1da-152">toodo tuto, otevřete `/boot/grub/menu.lst` v textovém editoru a ujistěte se, že jádra výchozí hello zahrnuje hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="be1da-152">toodo this, open `/boot/grub/menu.lst` in a text editor and ensure that hello default kernel includes hello following parameters:</span></span>
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    <span data-ttu-id="be1da-153">To také zajistí, všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure podporu pro ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="be1da-153">This will also ensure all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span>
    
    <span data-ttu-id="be1da-154">Kromě výše uvedených toohello, se doporučuje příliš*odebrat* hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="be1da-154">In addition toohello above, it is recommended too*remove* hello following parameters:</span></span>
    
        rhgb quiet crashkernel=auto
    
    <span data-ttu-id="be1da-155">Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly toobe hello odeslané toohello sériového portu.</span><span class="sxs-lookup"><span data-stu-id="be1da-155">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span>  <span data-ttu-id="be1da-156">Hello `crashkernel` možnost může být vlevo nakonfigurované v případě potřeby, ale Všimněte si, že tento parametr se sníží hello množství dostupné paměti v hello virtuální počítač podle 128 MB nebo víc, což může být problematické hello menší velikostí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="be1da-156">hello `crashkernel` option may be left configured if desired, but note that this parameter will reduce hello amount of available memory in hello VM by 128MB or more, which may be problematic on hello smaller VM sizes.</span></span>

    >[!Important]
    <span data-ttu-id="be1da-157">CentOS verze 6.5 a starší, musíte taky nastavit parametr jádra hello `numa=off`.</span><span class="sxs-lookup"><span data-stu-id="be1da-157">CentOS 6.5 and earlier must also set hello kernel parameter `numa=off`.</span></span> <span data-ttu-id="be1da-158">V tématu Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span><span class="sxs-lookup"><span data-stu-id="be1da-158">See Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span></span>

14. <span data-ttu-id="be1da-159">Ujistěte se, že server SSH hello je nainstalován a nakonfigurován toostart při spuštění.</span><span class="sxs-lookup"><span data-stu-id="be1da-159">Ensure that hello SSH server is installed and configured toostart at boot time.</span></span>  <span data-ttu-id="be1da-160">Je to obvykle výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="be1da-160">This is usually hello default.</span></span>

15. <span data-ttu-id="be1da-161">Nevytvářejte odkládacího prostoru na disku operačního systému hello.</span><span class="sxs-lookup"><span data-stu-id="be1da-161">Do not create swap space on hello OS disk.</span></span>
    
    <span data-ttu-id="be1da-162">Hello Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku hello místní prostředek, který je připojený toohello virtuálních počítačů po zřízení v Azure.</span><span class="sxs-lookup"><span data-stu-id="be1da-162">hello Azure Linux Agent can automatically configure swap space using hello local resource disk that is attached toohello VM after provisioning on Azure.</span></span> <span data-ttu-id="be1da-163">Všimněte si, je tento disk hello místní prostředek *dočasné* na disku a může být vyprázdnit po deprovisioned hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="be1da-163">Note that hello local resource disk is a *temporary* disk, and might be emptied when hello VM is deprovisioned.</span></span> <span data-ttu-id="be1da-164">Po instalaci hello Azure Linux Agent (viz předchozí krok), upravte hello následující parametry v `/etc/waagent.conf` odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="be1da-164">After installing hello Azure Linux Agent (see previous step), modify hello following parameters in `/etc/waagent.conf` appropriately:</span></span>
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

16. <span data-ttu-id="be1da-165">Spusťte hello následující příkazy toodeprovision hello virtuálního počítače a jeho přípravu pro zřizování na Azure:</span><span class="sxs-lookup"><span data-stu-id="be1da-165">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

17. <span data-ttu-id="be1da-166">Klikněte na tlačítko **akce -> vypnutí dolů** ve Správci technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="be1da-166">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="be1da-167">Svůj disk VHD Linux je nyní připraven toobe nahrán tooAzure.</span><span class="sxs-lookup"><span data-stu-id="be1da-167">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>


- - -
## <a name="centos-70"></a><span data-ttu-id="be1da-168">CentOS 7.0 +</span><span class="sxs-lookup"><span data-stu-id="be1da-168">CentOS 7.0+</span></span>
<span data-ttu-id="be1da-169">**Změny v CentOS 7 (a podobné odvozené konfigurace)**</span><span class="sxs-lookup"><span data-stu-id="be1da-169">**Changes in CentOS 7 (and similar derivatives)**</span></span>

<span data-ttu-id="be1da-170">Příprava CentOS 7 virtuálního počítače na Azure je velmi podobné tooCentOS 6, ale existuje několik důležitých rozdílů vhodné poznamenat:</span><span class="sxs-lookup"><span data-stu-id="be1da-170">Preparing a CentOS 7 virtual machine for Azure is very similar tooCentOS 6, however there are several important differences worth noting:</span></span>

* <span data-ttu-id="be1da-171">Hello NetworkManager balíček už je v konfliktu s hello Azure Linux agent.</span><span class="sxs-lookup"><span data-stu-id="be1da-171">hello NetworkManager package no longer conflicts with hello Azure Linux agent.</span></span> <span data-ttu-id="be1da-172">Tento balíček je ve výchozím nastavení nainstalovaná a doporučujeme vám, že se neodebere.</span><span class="sxs-lookup"><span data-stu-id="be1da-172">This package is installed by default and we recommend that it is not removed.</span></span>
* <span data-ttu-id="be1da-173">GRUB2 se teď používá jako výchozí zaváděcí program pro spouštění text hello, takže hello postup pro úpravy parametry jádra se změnila (viz níže).</span><span class="sxs-lookup"><span data-stu-id="be1da-173">GRUB2 is now used as hello default bootloader, so hello procedure for editing kernel parameters has changed (see below).</span></span>
* <span data-ttu-id="be1da-174">XFS je nyní hello výchozí systém souborů.</span><span class="sxs-lookup"><span data-stu-id="be1da-174">XFS is now hello default file system.</span></span> <span data-ttu-id="be1da-175">systém souborů ext4 Hello pořád můžou použít v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="be1da-175">hello ext4 file system can still be used if desired.</span></span>

<span data-ttu-id="be1da-176">**Kroky konfigurace**</span><span class="sxs-lookup"><span data-stu-id="be1da-176">**Configuration Steps**</span></span>

1. <span data-ttu-id="be1da-177">Ve Správci technologie Hyper-V vyberte hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="be1da-177">In Hyper-V Manager, select hello virtual machine.</span></span>

2. <span data-ttu-id="be1da-178">Klikněte na tlačítko **připojit** tooopen okna konzoly pro virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="be1da-178">Click **Connect** tooopen a console window for hello virtual machine.</span></span>

3. <span data-ttu-id="be1da-179">Vytvořte nebo upravte soubor hello `/etc/sysconfig/network` a přidejte hello následující text:</span><span class="sxs-lookup"><span data-stu-id="be1da-179">Create or edit hello file `/etc/sysconfig/network` and add hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4. <span data-ttu-id="be1da-180">Vytvořte nebo upravte soubor hello `/etc/sysconfig/network-scripts/ifcfg-eth0` a přidejte hello následující text:</span><span class="sxs-lookup"><span data-stu-id="be1da-180">Create or edit hello file `/etc/sysconfig/network-scripts/ifcfg-eth0` and add hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

5. <span data-ttu-id="be1da-181">Upravte tooavoid udev pravidla generování statická pravidla pro hello alespoň jedno rozhraní sítě Ethernet.</span><span class="sxs-lookup"><span data-stu-id="be1da-181">Modify udev rules tooavoid generating static rules for hello Ethernet interface(s).</span></span> <span data-ttu-id="be1da-182">Tato pravidla může způsobit problémy při klonování virtuálního počítače v Microsoft Azure nebo Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="be1da-182">These rules can cause problems when cloning a virtual machine in Microsoft Azure or Hyper-V:</span></span>
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. <span data-ttu-id="be1da-183">Pokud chcete toouse hello OpenLogic zrcadlení, které jsou hostované v rámci hello datových centrech Azure, potom můžete nahradit hello `/etc/yum.repos.d/CentOS-Base.repo` soubor s hello následující úložiště.</span><span class="sxs-lookup"><span data-stu-id="be1da-183">If you would like toouse hello OpenLogic mirrors that are hosted within hello Azure datacenters, then replace hello `/etc/yum.repos.d/CentOS-Base.repo` file with hello following repositories.</span></span>  <span data-ttu-id="be1da-184">Taky se přidá hello **[openlogic]** úložiště, která obsahuje balíčky pro hello Azure Linux agent:</span><span class="sxs-lookup"><span data-stu-id="be1da-184">This will also add hello **[openlogic]** repository that includes packages for hello Azure Linux agent:</span></span>
   
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
    <span data-ttu-id="be1da-185">Hello zbývající části této příručky bude předpokládat, že používáte alespoň hello `[openlogic]` úložišti, který bude použité tooinstall hello Azure Linux agent níže.</span><span class="sxs-lookup"><span data-stu-id="be1da-185">hello rest of this guide will assume you are using at least hello `[openlogic]` repo, which will be used tooinstall hello Azure Linux agent below.</span></span>

7. <span data-ttu-id="be1da-186">Spusťte následující příkaz tooclear hello aktuální yum metadata hello a nainstalujte všechny aktualizace:</span><span class="sxs-lookup"><span data-stu-id="be1da-186">Run hello following command tooclear hello current yum metadata and install any updates:</span></span>
   
        # sudo yum clean all

    <span data-ttu-id="be1da-187">Pokud chcete vytvořit bitovou kopii pro starší verze CentOS, doporučuje se, že tooupdate, které všechny hello balíčky toohello nejnovější:</span><span class="sxs-lookup"><span data-stu-id="be1da-187">Unless you are creating an image for an older version of CentOS, it is recommended tooupdate all hello packages toohello latest:</span></span>

        # sudo yum -y update

    <span data-ttu-id="be1da-188">Restartování, může být nutné po spuštění tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="be1da-188">A reboot maybe required after running this command.</span></span>

8. <span data-ttu-id="be1da-189">Upravte hello jádra spouštěcí řádku grub konfigurace tooinclude další jádra parametry pro Azure.</span><span class="sxs-lookup"><span data-stu-id="be1da-189">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="be1da-190">toodo tuto, otevřete `/etc/default/grub` v textového editoru a úpravy hello `GRUB_CMDLINE_LINUX` parametr, například:</span><span class="sxs-lookup"><span data-stu-id="be1da-190">toodo this, open `/etc/default/grub` in a text editor and edit hello `GRUB_CMDLINE_LINUX` parameter, for example:</span></span>
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   <span data-ttu-id="be1da-191">To také zajistí, všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure podporu pro ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="be1da-191">This will also ensure all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="be1da-192">Je také vypne hello nové CentOS 7 zásady vytváření názvů pro síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="be1da-192">It also turns off hello new CentOS 7 naming conventions for NICs.</span></span> <span data-ttu-id="be1da-193">Kromě výše uvedených toohello, se doporučuje příliš*odebrat* hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="be1da-193">In addition toohello above, it is recommended too*remove* hello following parameters:</span></span>
   
        rhgb quiet crashkernel=auto
   
    <span data-ttu-id="be1da-194">Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly toobe hello odeslané toohello sériového portu.</span><span class="sxs-lookup"><span data-stu-id="be1da-194">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span> <span data-ttu-id="be1da-195">Hello `crashkernel` možnost může být vlevo nakonfigurované v případě potřeby, ale Všimněte si, že tento parametr se sníží hello množství dostupné paměti v hello virtuální počítač podle 128 MB nebo víc, což může být problematické hello menší velikostí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="be1da-195">hello `crashkernel` option may be left configured if desired, but note that this parameter will reduce hello amount of available memory in hello VM by 128MB or more, which may be problematic on hello smaller VM sizes.</span></span>

9. <span data-ttu-id="be1da-196">Po dokončení úprav `/etc/default/grub` za výše, spusťte následující příkaz toorebuild hello grub konfigurace hello:</span><span class="sxs-lookup"><span data-stu-id="be1da-196">Once you are done editing `/etc/default/grub` per above, run hello following command toorebuild hello grub configuration:</span></span>
   
        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

10. <span data-ttu-id="be1da-197">Pokud vytváření hello bitovou kopii z **VMWare, VirtualBox nebo KVM:** zajistěte, aby ovladače hello technologie Hyper-V jsou součástí hello initramfs:</span><span class="sxs-lookup"><span data-stu-id="be1da-197">If building hello image from **VMWare, VirtualBox or KVM:** Ensure hello Hyper-V drivers are included in hello initramfs:</span></span>
   
   <span data-ttu-id="be1da-198">Upravit `/etc/dracut.conf`, přidejte obsah:</span><span class="sxs-lookup"><span data-stu-id="be1da-198">Edit `/etc/dracut.conf`, add content:</span></span>
   
        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”
   
   <span data-ttu-id="be1da-199">Znovu sestavte hello initramfs:</span><span class="sxs-lookup"><span data-stu-id="be1da-199">Rebuild hello initramfs:</span></span>
   
        # sudo dracut –f -v

11. <span data-ttu-id="be1da-200">Nainstalujte hello Azure Linux Agent a závislosti:</span><span class="sxs-lookup"><span data-stu-id="be1da-200">Install hello Azure Linux Agent and dependencies:</span></span>

        # sudo yum install python-pyasn1 WALinuxAgent
        # sudo systemctl enable waagent

12. <span data-ttu-id="be1da-201">Nevytvářejte odkládacího prostoru na disku operačního systému hello.</span><span class="sxs-lookup"><span data-stu-id="be1da-201">Do not create swap space on hello OS disk.</span></span>
   
   <span data-ttu-id="be1da-202">Hello Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku hello místní prostředek, který je připojený toohello virtuálních počítačů po zřízení v Azure.</span><span class="sxs-lookup"><span data-stu-id="be1da-202">hello Azure Linux Agent can automatically configure swap space using hello local resource disk that is attached toohello VM after provisioning on Azure.</span></span> <span data-ttu-id="be1da-203">Všimněte si, je tento disk hello místní prostředek *dočasné* na disku a může být vyprázdnit po deprovisioned hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="be1da-203">Note that hello local resource disk is a *temporary* disk, and might be emptied when hello VM is deprovisioned.</span></span> <span data-ttu-id="be1da-204">Po instalaci hello Azure Linux Agent (viz předchozí krok), upravte hello následující parametry v `/etc/waagent.conf` odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="be1da-204">After installing hello Azure Linux Agent (see previous step), modify hello following parameters in `/etc/waagent.conf` appropriately:</span></span>
   
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

13. <span data-ttu-id="be1da-205">Spusťte hello následující příkazy toodeprovision hello virtuálního počítače a jeho přípravu pro zřizování na Azure:</span><span class="sxs-lookup"><span data-stu-id="be1da-205">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>
   
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. <span data-ttu-id="be1da-206">Klikněte na tlačítko **akce -> vypnutí dolů** ve Správci technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="be1da-206">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="be1da-207">Svůj disk VHD Linux je nyní připraven toobe nahrán tooAzure.</span><span class="sxs-lookup"><span data-stu-id="be1da-207">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="be1da-208">Další kroky</span><span class="sxs-lookup"><span data-stu-id="be1da-208">Next steps</span></span>
<span data-ttu-id="be1da-209">Můžete se teď připravena toouse systému CentOS Linux virtuální pevný disk toocreate nové virtuální počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="be1da-209">You're now ready toouse your CentOS Linux virtual hard disk toocreate new virtual machines in Azure.</span></span> <span data-ttu-id="be1da-210">Pokud je to hello poprvé, že jste odesílání tooAzure soubor VHD hello, najdete v části kroky 2 a 3 v [vytváření a odesílání virtuální pevný disk, který obsahuje operační systém Linux hello](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="be1da-210">If this is hello first time that you're uploading hello .vhd file tooAzure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains hello Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

