---
title: "aaaCreate a nahrání virtuálního pevného disku Oracle Linux | Microsoft Docs"
description: "Další informace toocreate a nahrajte Azure virtuální pevný disk (VHD), který obsahuje operační systém Oracle Linux."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-service-management,azure-resource-manager
ms.assetid: dd96f771-26eb-4391-9a89-8c8b6d691822
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: szark
ms.openlocfilehash: be9cf284d7f5e7122a106506a343e53e9f1ac56e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a><span data-ttu-id="fe424-103">Příprava virtuálního počítače s Oracle Linuxem pro Azure</span><span class="sxs-lookup"><span data-stu-id="fe424-103">Prepare an Oracle Linux virtual machine for Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a><span data-ttu-id="fe424-104">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fe424-104">Prerequisites</span></span>
<span data-ttu-id="fe424-105">Tento článek předpokládá, že jste již nainstalovali Oracle Linux tooa virtuální pevný disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="fe424-105">This article assumes that you have already installed an Oracle Linux operating system tooa virtual hard disk.</span></span> <span data-ttu-id="fe424-106">Několik nástrojů existovat toocreate soubory VHD, například řešení virtualizace jako je například technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="fe424-106">Multiple tools exist toocreate .vhd files, for example a virtualization solution such as Hyper-V.</span></span> <span data-ttu-id="fe424-107">Pokyny najdete v tématu [instalace hello Role Hyper-V a konfigurace virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="fe424-107">For instructions, see [Install hello Hyper-V Role and Configure a Virtual Machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

### <a name="oracle-linux-installation-notes"></a><span data-ttu-id="fe424-108">Poznámky k instalaci Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="fe424-108">Oracle Linux installation notes</span></span>
* <span data-ttu-id="fe424-109">Najdete také [obecné poznámky k instalaci Linux](create-upload-generic.md#general-linux-installation-notes) Příprava na Azure Linux další tipy pro.</span><span class="sxs-lookup"><span data-stu-id="fe424-109">Please see also [General Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more tips on preparing Linux for Azure.</span></span>
* <span data-ttu-id="fe424-110">Oracle Red Hat kompatibilní jádra a jejich UEK3 (nedělitelné Enterprise jádra) jsou podporovány v Hyper-V a Azure.</span><span class="sxs-lookup"><span data-stu-id="fe424-110">Oracle's Red Hat compatible kernel and their UEK3 (Unbreakable Enterprise Kernel) are both supported on Hyper-V and Azure.</span></span> <span data-ttu-id="fe424-111">Nejlepších výsledků dosáhnete buďte jisti tooupdate toohello nejnovější jádra při přípravě virtuálního pevného disku vašeho Oracle Linux.</span><span class="sxs-lookup"><span data-stu-id="fe424-111">For best results, please be sure tooupdate toohello latest kernel while preparing your Oracle Linux VHD.</span></span>
* <span data-ttu-id="fe424-112">Oracle na UEK2 se nepodporuje na Hyper-V a Azure, protože neobsahuje hello požadované ovladače.</span><span class="sxs-lookup"><span data-stu-id="fe424-112">Oracle's UEK2 is not supported on Hyper-V and Azure as it does not include hello required drivers.</span></span>
* <span data-ttu-id="fe424-113">Formát VHDX Hello není podporován v Azure, pouze **pevný virtuální pevný disk**.</span><span class="sxs-lookup"><span data-stu-id="fe424-113">hello VHDX format is not supported in Azure, only **fixed VHD**.</span></span>  <span data-ttu-id="fe424-114">Můžete převést formát tooVHD hello disku pomocí Správce technologie Hyper-V nebo hello rutiny convert-VHD prostředí.</span><span class="sxs-lookup"><span data-stu-id="fe424-114">You can convert hello disk tooVHD format using Hyper-V Manager or hello convert-vhd cmdlet.</span></span>
* <span data-ttu-id="fe424-115">Při instalaci systému Linux hello se doporučuje použít standardní oddíly spíše než LVM (často hello výchozí pro mnoho instalace).</span><span class="sxs-lookup"><span data-stu-id="fe424-115">When installing hello Linux system it is recommended that you use standard partitions rather than LVM (often hello default for many installations).</span></span> <span data-ttu-id="fe424-116">Tím se vyhnete LVM název je v konfliktu s klonovaný virtuální počítače, zvlášť pokud operační systém disk někdy potřebovat tooanother toobe připojených virtuálních počítačů pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="fe424-116">This will avoid LVM name conflicts with cloned VMs, particularly if an OS disk ever needs toobe attached tooanother VM for troubleshooting.</span></span> <span data-ttu-id="fe424-117">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) lze použít v datových disků, pokud upřednostňovaný.</span><span class="sxs-lookup"><span data-stu-id="fe424-117">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks if preferred.</span></span>
* <span data-ttu-id="fe424-118">Pro větší velikosti virtuálních počítačů z důvodu chyb tooa ve verzích systému Linux jádra níže 2.6.37 nepodporuje NUMA.</span><span class="sxs-lookup"><span data-stu-id="fe424-118">NUMA is not supported for larger VM sizes due tooa bug in Linux kernel versions below 2.6.37.</span></span> <span data-ttu-id="fe424-119">Toto vydání primárně ovlivňuje distribuce pomocí hello nadřazeného Red Hat 2.6.32 jádra.</span><span class="sxs-lookup"><span data-stu-id="fe424-119">This issue primarily impacts distributions using hello upstream Red Hat 2.6.32 kernel.</span></span> <span data-ttu-id="fe424-120">Ruční instalace agenta Azure Linux hello (příkaz waagent) automaticky zakáže NUMA v hello GRUB konfiguraci pro jádro Linux hello.</span><span class="sxs-lookup"><span data-stu-id="fe424-120">Manual installation of hello Azure Linux agent (waagent) will automatically disable NUMA in hello GRUB configuration for hello Linux kernel.</span></span> <span data-ttu-id="fe424-121">Další informace o této naleznete v následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="fe424-121">More information about this can be found in hello steps below.</span></span>
* <span data-ttu-id="fe424-122">Nekonfigurujte přepnutí oddílu na disku operačního systému hello.</span><span class="sxs-lookup"><span data-stu-id="fe424-122">Do not configure a swap partition on hello OS disk.</span></span> <span data-ttu-id="fe424-123">Hello Linux agent může být nakonfigurované toocreate odkládací soubor na disku hello dočasných prostředků.</span><span class="sxs-lookup"><span data-stu-id="fe424-123">hello Linux agent can be configured toocreate a swap file on hello temporary resource disk.</span></span>  <span data-ttu-id="fe424-124">Další informace o této naleznete v následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="fe424-124">More information about this can be found in hello steps below.</span></span>
* <span data-ttu-id="fe424-125">Všechny virtuální pevné disky hello musí mít velikostí, které jsou násobky 1 MB.</span><span class="sxs-lookup"><span data-stu-id="fe424-125">All of hello VHDs must have sizes that are multiples of 1 MB.</span></span>
* <span data-ttu-id="fe424-126">Ujistěte se, že hello `Addons` je povoleno úložiště.</span><span class="sxs-lookup"><span data-stu-id="fe424-126">Make sure that hello `Addons` repository is enabled.</span></span> <span data-ttu-id="fe424-127">Upravte soubor hello `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) nebo `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux) a změňte řádek hello `enabled=0` příliš`enabled=1` pod **[ol6_addons]** nebo **[ol7_addons]** v tomto soubor.</span><span class="sxs-lookup"><span data-stu-id="fe424-127">Edit hello file `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) or `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux ), and change hello line `enabled=0` too`enabled=1` under **[ol6_addons]** or **[ol7_addons]** in this file.</span></span>

## <a name="oracle-linux-64"></a><span data-ttu-id="fe424-128">Oracle Linux 6.4 +</span><span class="sxs-lookup"><span data-stu-id="fe424-128">Oracle Linux 6.4+</span></span>
<span data-ttu-id="fe424-129">Je třeba provést určitou konfiguraci kroky v hello operační systém pro virtuální počítač toorun hello v Azure.</span><span class="sxs-lookup"><span data-stu-id="fe424-129">You must complete specific configuration steps in hello operating system for hello virtual machine toorun in Azure.</span></span>

1. <span data-ttu-id="fe424-130">V prostředním podokně hello Správce technologie Hyper-V vyberte hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fe424-130">In hello center pane of Hyper-V Manager, select hello virtual machine.</span></span>
2. <span data-ttu-id="fe424-131">Klikněte na tlačítko **připojit** tooopen hello okna pro hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fe424-131">Click **Connect** tooopen hello window for hello virtual machine.</span></span>
3. <span data-ttu-id="fe424-132">Odinstalace NetworkManager spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fe424-132">Uninstall NetworkManager by running hello following command:</span></span>
   
        # sudo rpm -e --nodeps NetworkManager
   
    <span data-ttu-id="fe424-133">**Poznámka:** Pokud ještě není nainstalovaný balíček hello, tento příkaz se nezdaří s chybovou zprávou.</span><span class="sxs-lookup"><span data-stu-id="fe424-133">**Note:** If hello package is not already installed, this command will fail with an error message.</span></span> <span data-ttu-id="fe424-134">Toto chování se očekává.</span><span class="sxs-lookup"><span data-stu-id="fe424-134">This is expected.</span></span>
4. <span data-ttu-id="fe424-135">Vytvořte soubor s názvem **sítě** v hello `/etc/sysconfig/` adresář, který obsahuje hello následující text:</span><span class="sxs-lookup"><span data-stu-id="fe424-135">Create a file named **network** in hello `/etc/sysconfig/` directory that contains hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
5. <span data-ttu-id="fe424-136">Vytvořte soubor s názvem **ifcfg eth0** v hello `/etc/sysconfig/network-scripts/` adresář, který obsahuje hello následující text:</span><span class="sxs-lookup"><span data-stu-id="fe424-136">Create a file named **ifcfg-eth0** in hello `/etc/sysconfig/network-scripts/` directory that contains hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
6. <span data-ttu-id="fe424-137">Upravte tooavoid udev pravidla generování statická pravidla pro hello alespoň jedno rozhraní sítě Ethernet.</span><span class="sxs-lookup"><span data-stu-id="fe424-137">Modify udev rules tooavoid generating static rules for hello Ethernet interface(s).</span></span> <span data-ttu-id="fe424-138">Tato pravidla může způsobit problémy při klonování virtuálního počítače v Microsoft Azure nebo Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="fe424-138">These rules can cause problems when cloning a virtual machine in Microsoft Azure or Hyper-V:</span></span>
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
7. <span data-ttu-id="fe424-139">Zajistěte, aby hello síťová služba se spustí při spuštění spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fe424-139">Ensure hello network service will start at boot time by running hello following command:</span></span>
   
        # chkconfig network on
8. <span data-ttu-id="fe424-140">Instalace jazyka python pyasn1 spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fe424-140">Install python-pyasn1 by running hello following command:</span></span>
   
        # sudo yum install python-pyasn1
9. <span data-ttu-id="fe424-141">Upravte hello jádra spouštěcí řádku grub konfigurace tooinclude další jádra parametry pro Azure.</span><span class="sxs-lookup"><span data-stu-id="fe424-141">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="fe424-142">toodo tomto otevřete "/ boot/grub/menu.lst" v textovém editoru a ujistěte se, že jádra výchozí hello zahrnuje hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="fe424-142">toodo this open "/boot/grub/menu.lst" in a text editor and ensure that hello default kernel includes hello following parameters:</span></span>
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off
   
   <span data-ttu-id="fe424-143">To také zajistí, všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure podporu pro ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="fe424-143">This will also ensure all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="fe424-144">Tato akce zakáže NUMA z důvodu chyb tooa Oracle Red Hat kompatibilní jádra systému.</span><span class="sxs-lookup"><span data-stu-id="fe424-144">This will disable NUMA due tooa bug in Oracle's Red Hat compatible kernel.</span></span>
   
   <span data-ttu-id="fe424-145">Kromě výše uvedených toohello, se doporučuje příliš*odebrat* hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="fe424-145">In addition toohello above, it is recommended too*remove* hello following parameters:</span></span>
   
        rhgb quiet crashkernel=auto
   
   <span data-ttu-id="fe424-146">Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly toobe hello odeslané toohello sériového portu.</span><span class="sxs-lookup"><span data-stu-id="fe424-146">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span>
   
   <span data-ttu-id="fe424-147">Hello `crashkernel` možnost může být vlevo nakonfigurované v případě potřeby, ale Všimněte si, že tento parametr se sníží hello množství dostupné paměti v hello virtuální počítač podle 128 MB nebo víc, což může být problematické hello menší velikostí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fe424-147">hello `crashkernel` option may be left configured if desired, but note that this parameter will reduce hello amount of available memory in hello VM by 128MB or more, which may be problematic on hello smaller VM sizes.</span></span>
10. <span data-ttu-id="fe424-148">Ujistěte se, že server SSH hello je nainstalován a nakonfigurován toostart při spuštění.</span><span class="sxs-lookup"><span data-stu-id="fe424-148">Ensure that hello SSH server is installed and configured toostart at boot time.</span></span>  <span data-ttu-id="fe424-149">Je to obvykle výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="fe424-149">This is usually hello default.</span></span>
11. <span data-ttu-id="fe424-150">Nainstalujte hello Azure Linux Agent tak, že spustíte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="fe424-150">Install hello Azure Linux Agent by running hello following command.</span></span> <span data-ttu-id="fe424-151">nejnovější verze Hello je 2.0.15.</span><span class="sxs-lookup"><span data-stu-id="fe424-151">hello latest version is 2.0.15.</span></span>
    
        # sudo yum install WALinuxAgent
    
    <span data-ttu-id="fe424-152">Upozorňujeme, že dojde k instalaci balíčku WALinuxAgent hello odebrání hello NetworkManager a balíčky NetworkManager gnome Pokud nebyly již odebrána jak je popsáno v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="fe424-152">Note that installing hello WALinuxAgent package will remove hello NetworkManager and NetworkManager-gnome packages if they were not already removed as described in step 2.</span></span>
12. <span data-ttu-id="fe424-153">Nevytvářejte odkládacího prostoru na disku operačního systému hello.</span><span class="sxs-lookup"><span data-stu-id="fe424-153">Do not create swap space on hello OS disk.</span></span>
    
    <span data-ttu-id="fe424-154">Hello Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku hello místní prostředek, který je připojený toohello virtuálních počítačů po zřízení v Azure.</span><span class="sxs-lookup"><span data-stu-id="fe424-154">hello Azure Linux Agent can automatically configure swap space using hello local resource disk that is attached toohello VM after provisioning on Azure.</span></span> <span data-ttu-id="fe424-155">Všimněte si, je tento disk hello místní prostředek *dočasné* na disku a může být vyprázdnit po deprovisioned hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fe424-155">Note that hello local resource disk is a *temporary* disk, and might be emptied when hello VM is deprovisioned.</span></span> <span data-ttu-id="fe424-156">Po instalaci hello Azure Linux Agent (viz předchozí krok), upravte hello následující parametry v /etc/waagent.conf odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fe424-156">After installing hello Azure Linux Agent (see previous step), modify hello following parameters in /etc/waagent.conf appropriately:</span></span>
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.
13. <span data-ttu-id="fe424-157">Spusťte hello následující příkazy toodeprovision hello virtuálního počítače a jeho přípravu pro zřizování na Azure:</span><span class="sxs-lookup"><span data-stu-id="fe424-157">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
14. <span data-ttu-id="fe424-158">Klikněte na tlačítko **akce -> vypnutí dolů** ve Správci technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="fe424-158">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="fe424-159">Svůj disk VHD Linux je nyní připraven toobe nahrán tooAzure.</span><span class="sxs-lookup"><span data-stu-id="fe424-159">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>

- - -
## <a name="oracle-linux-70"></a><span data-ttu-id="fe424-160">Oracle Linux 7.0 +</span><span class="sxs-lookup"><span data-stu-id="fe424-160">Oracle Linux 7.0+</span></span>
<span data-ttu-id="fe424-161">**Změny v systému Oracle Linux 7**</span><span class="sxs-lookup"><span data-stu-id="fe424-161">**Changes in Oracle Linux 7**</span></span>

<span data-ttu-id="fe424-162">Připravuje se virtuální počítač Oracle Linux 7 pro Azure je velmi podobné tooOracle Linux 6, ale existuje několik důležitých rozdílů vhodné poznamenat:</span><span class="sxs-lookup"><span data-stu-id="fe424-162">Preparing an Oracle Linux 7 virtual machine for Azure is very similar tooOracle Linux 6, however there are several important differences worth noting:</span></span>

* <span data-ttu-id="fe424-163">Hello Red Hat kompatibilní jádra a Oracle je UEK3 jsou podporovány v Azure.</span><span class="sxs-lookup"><span data-stu-id="fe424-163">Both hello Red Hat compatible kernel and Oracle's UEK3 are supported in Azure.</span></span>  <span data-ttu-id="fe424-164">doporučuje se Hello UEK3 jádra.</span><span class="sxs-lookup"><span data-stu-id="fe424-164">hello UEK3 kernel is recommended.</span></span>
* <span data-ttu-id="fe424-165">Hello NetworkManager balíček už je v konfliktu s hello Azure Linux agent.</span><span class="sxs-lookup"><span data-stu-id="fe424-165">hello NetworkManager package no longer conflicts with hello Azure Linux agent.</span></span> <span data-ttu-id="fe424-166">Tento balíček je ve výchozím nastavení nainstalovaná a doporučujeme vám, že se neodebere.</span><span class="sxs-lookup"><span data-stu-id="fe424-166">This package is installed by default and we recommend that it is not removed.</span></span>
* <span data-ttu-id="fe424-167">GRUB2 se teď používá jako výchozí zaváděcí program pro spouštění text hello, takže hello postup pro úpravy parametry jádra se změnila (viz níže).</span><span class="sxs-lookup"><span data-stu-id="fe424-167">GRUB2 is now used as hello default bootloader, so hello procedure for editing kernel parameters has changed (see below).</span></span>
* <span data-ttu-id="fe424-168">XFS je nyní hello výchozí systém souborů.</span><span class="sxs-lookup"><span data-stu-id="fe424-168">XFS is now hello default file system.</span></span> <span data-ttu-id="fe424-169">systém souborů ext4 Hello pořád můžou použít v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="fe424-169">hello ext4 file system can still be used if desired.</span></span>

<span data-ttu-id="fe424-170">**Kroky konfigurace**</span><span class="sxs-lookup"><span data-stu-id="fe424-170">**Configuration steps**</span></span>

1. <span data-ttu-id="fe424-171">Ve Správci technologie Hyper-V vyberte hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fe424-171">In Hyper-V Manager, select hello virtual machine.</span></span>
2. <span data-ttu-id="fe424-172">Klikněte na tlačítko **připojit** tooopen okna konzoly pro virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="fe424-172">Click **Connect** tooopen a console window for hello virtual machine.</span></span>
3. <span data-ttu-id="fe424-173">Vytvořte soubor s názvem **sítě** v hello `/etc/sysconfig/` adresář, který obsahuje hello následující text:</span><span class="sxs-lookup"><span data-stu-id="fe424-173">Create a file named **network** in hello `/etc/sysconfig/` directory that contains hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
4. <span data-ttu-id="fe424-174">Vytvořte soubor s názvem **ifcfg eth0** v hello `/etc/sysconfig/network-scripts/` adresář, který obsahuje hello následující text:</span><span class="sxs-lookup"><span data-stu-id="fe424-174">Create a file named **ifcfg-eth0** in hello `/etc/sysconfig/network-scripts/` directory that contains hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
5. <span data-ttu-id="fe424-175">Upravte tooavoid udev pravidla generování statická pravidla pro hello alespoň jedno rozhraní sítě Ethernet.</span><span class="sxs-lookup"><span data-stu-id="fe424-175">Modify udev rules tooavoid generating static rules for hello Ethernet interface(s).</span></span> <span data-ttu-id="fe424-176">Tato pravidla může způsobit problémy při klonování virtuálního počítače v Microsoft Azure nebo Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="fe424-176">These rules can cause problems when cloning a virtual machine in Microsoft Azure or Hyper-V:</span></span>
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
6. <span data-ttu-id="fe424-177">Zajistěte, aby hello síťová služba se spustí při spuštění spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fe424-177">Ensure hello network service will start at boot time by running hello following command:</span></span>
   
        # sudo chkconfig network on
7. <span data-ttu-id="fe424-178">Instalovat balíček python pyasn1 hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fe424-178">Install hello python-pyasn1 package by running hello following command:</span></span>
   
        # sudo yum install python-pyasn1
8. <span data-ttu-id="fe424-179">Spusťte následující příkaz tooclear hello aktuální yum metadata hello a nainstalujte všechny aktualizace:</span><span class="sxs-lookup"><span data-stu-id="fe424-179">Run hello following command tooclear hello current yum metadata and install any updates:</span></span>
   
        # sudo yum clean all
        # sudo yum -y update
9. <span data-ttu-id="fe424-180">Upravte hello jádra spouštěcí řádku grub konfigurace tooinclude další jádra parametry pro Azure.</span><span class="sxs-lookup"><span data-stu-id="fe424-180">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="fe424-181">toodo tomto otevřete "/ etc/výchozí/grub" v textu editoru a úpravy hello `GRUB_CMDLINE_LINUX` parametr, například:</span><span class="sxs-lookup"><span data-stu-id="fe424-181">toodo this open "/etc/default/grub" in a text editor and edit hello `GRUB_CMDLINE_LINUX` parameter, for example:</span></span>
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   <span data-ttu-id="fe424-182">To také zajistí, všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure podporu pro ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="fe424-182">This will also ensure all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="fe424-183">Je také vypne hello nové OEL 7 zásady vytváření názvů pro síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="fe424-183">It also turns off hello new OEL 7 naming conventions for NICs.</span></span> <span data-ttu-id="fe424-184">Kromě výše uvedených toohello, se doporučuje příliš*odebrat* hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="fe424-184">In addition toohello above, it is recommended too*remove* hello following parameters:</span></span>
   
       rhgb quiet crashkernel=auto
   
   <span data-ttu-id="fe424-185">Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly toobe hello odeslané toohello sériového portu.</span><span class="sxs-lookup"><span data-stu-id="fe424-185">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span>
   
   <span data-ttu-id="fe424-186">Hello `crashkernel` možnost může být vlevo nakonfigurované v případě potřeby, ale Všimněte si, že tento parametr se sníží hello množství dostupné paměti v hello virtuální počítač podle 128 MB nebo víc, což může být problematické hello menší velikostí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fe424-186">hello `crashkernel` option may be left configured if desired, but note that this parameter will reduce hello amount of available memory in hello VM by 128MB or more, which may be problematic on hello smaller VM sizes.</span></span>
10. <span data-ttu-id="fe424-187">Po dokončení úprav "/ etc/výchozí/grub" za výše, spusťte následující příkaz toorebuild hello grub konfigurace hello:</span><span class="sxs-lookup"><span data-stu-id="fe424-187">Once you are done editing "/etc/default/grub" per above, run hello following command toorebuild hello grub configuration:</span></span>
    
        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg
11. <span data-ttu-id="fe424-188">Ujistěte se, že server SSH hello je nainstalován a nakonfigurován toostart při spuštění.</span><span class="sxs-lookup"><span data-stu-id="fe424-188">Ensure that hello SSH server is installed and configured toostart at boot time.</span></span>  <span data-ttu-id="fe424-189">Je to obvykle výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="fe424-189">This is usually hello default.</span></span>
12. <span data-ttu-id="fe424-190">Nainstalujte hello Azure Linux Agent spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fe424-190">Install hello Azure Linux Agent by running hello following command:</span></span>
    
        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent
13. <span data-ttu-id="fe424-191">Nevytvářejte odkládacího prostoru na disku operačního systému hello.</span><span class="sxs-lookup"><span data-stu-id="fe424-191">Do not create swap space on hello OS disk.</span></span>
    
    <span data-ttu-id="fe424-192">Hello Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku hello místní prostředek, který je připojený toohello virtuálních počítačů po zřízení v Azure.</span><span class="sxs-lookup"><span data-stu-id="fe424-192">hello Azure Linux Agent can automatically configure swap space using hello local resource disk that is attached toohello VM after provisioning on Azure.</span></span> <span data-ttu-id="fe424-193">Všimněte si, je tento disk hello místní prostředek *dočasné* na disku a může být vyprázdnit po deprovisioned hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fe424-193">Note that hello local resource disk is a *temporary* disk, and might be emptied when hello VM is deprovisioned.</span></span> <span data-ttu-id="fe424-194">Po instalaci hello Azure Linux Agent (viz předchozí krok hello), upravte hello následující parametry v /etc/waagent.conf odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fe424-194">After installing hello Azure Linux Agent (see hello previous step), modify hello following parameters in /etc/waagent.conf appropriately:</span></span>
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.
14. <span data-ttu-id="fe424-195">Spusťte hello následující příkazy toodeprovision hello virtuálního počítače a jeho přípravu pro zřizování na Azure:</span><span class="sxs-lookup"><span data-stu-id="fe424-195">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
15. <span data-ttu-id="fe424-196">Klikněte na tlačítko **akce -> vypnutí dolů** ve Správci technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="fe424-196">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="fe424-197">Svůj disk VHD Linux je nyní připraven toobe nahrán tooAzure.</span><span class="sxs-lookup"><span data-stu-id="fe424-197">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fe424-198">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fe424-198">Next steps</span></span>
<span data-ttu-id="fe424-199">Můžete se teď připravena toouse Oracle Linux VHD toocreate nové virtuální počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="fe424-199">You're now ready toouse your Oracle Linux .vhd toocreate new virtual machines in Azure.</span></span> <span data-ttu-id="fe424-200">Pokud je to hello poprvé, že jste odesílání tooAzure soubor VHD hello, najdete v části kroky 2 a 3 v [vytváření a odesílání virtuální pevný disk, který obsahuje operační systém Linux hello](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="fe424-200">If this is hello first time that you're uploading hello .vhd file tooAzure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains hello Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

