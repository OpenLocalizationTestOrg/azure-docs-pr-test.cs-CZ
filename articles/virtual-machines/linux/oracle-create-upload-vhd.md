---
title: "Vytvoření a nahrání virtuálního pevného disku Oracle Linux | Microsoft Docs"
description: "Naučte se vytvořit a odeslat Azure virtuální pevný disk (VHD), který obsahuje operační systém Oracle Linux."
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
ms.openlocfilehash: c631ddf3acf6df7364c03eb4691b78be0493e0d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a><span data-ttu-id="d2fd5-103">Příprava virtuálního počítače s Oracle Linuxem pro Azure</span><span class="sxs-lookup"><span data-stu-id="d2fd5-103">Prepare an Oracle Linux virtual machine for Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a><span data-ttu-id="d2fd5-104">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d2fd5-104">Prerequisites</span></span>
<span data-ttu-id="d2fd5-105">Tento článek předpokládá, že jste již nainstalovali Oracle Linux operačního systému na virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-105">This article assumes that you have already installed an Oracle Linux operating system to a virtual hard disk.</span></span> <span data-ttu-id="d2fd5-106">Postup vytvoření souborů VHD, například řešení virtualizace jako je například technologie Hyper-V existuje několik nástrojů.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-106">Multiple tools exist to create .vhd files, for example a virtualization solution such as Hyper-V.</span></span> <span data-ttu-id="d2fd5-107">Pokyny najdete v tématu [nainstalovat roli Hyper-V a konfigurace virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="d2fd5-107">For instructions, see [Install the Hyper-V Role and Configure a Virtual Machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

### <a name="oracle-linux-installation-notes"></a><span data-ttu-id="d2fd5-108">Poznámky k instalaci Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="d2fd5-108">Oracle Linux installation notes</span></span>
* <span data-ttu-id="d2fd5-109">Najdete také [obecné poznámky k instalaci Linux](create-upload-generic.md#general-linux-installation-notes) Příprava na Azure Linux další tipy pro.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-109">Please see also [General Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more tips on preparing Linux for Azure.</span></span>
* <span data-ttu-id="d2fd5-110">Oracle Red Hat kompatibilní jádra a jejich UEK3 (nedělitelné Enterprise jádra) jsou podporovány v Hyper-V a Azure.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-110">Oracle's Red Hat compatible kernel and their UEK3 (Unbreakable Enterprise Kernel) are both supported on Hyper-V and Azure.</span></span> <span data-ttu-id="d2fd5-111">Nejlepších výsledků dosáhnete nezapomeňte aktualizovat na nejnovější jádra při přípravě virtuálního pevného disku vašeho Oracle Linux.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-111">For best results, please be sure to update to the latest kernel while preparing your Oracle Linux VHD.</span></span>
* <span data-ttu-id="d2fd5-112">Oracle na UEK2 se nepodporuje na Hyper-V a Azure, protože neobsahuje požadované ovladače.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-112">Oracle's UEK2 is not supported on Hyper-V and Azure as it does not include the required drivers.</span></span>
* <span data-ttu-id="d2fd5-113">Formát VHDX není podporován v Azure, pouze **pevný virtuální pevný disk**.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-113">The VHDX format is not supported in Azure, only **fixed VHD**.</span></span>  <span data-ttu-id="d2fd5-114">Disk můžete převést do formátu virtuálního pevného disku pomocí Správce technologie Hyper-V nebo rutiny convert-VHD prostředí.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-114">You can convert the disk to VHD format using Hyper-V Manager or the convert-vhd cmdlet.</span></span>
* <span data-ttu-id="d2fd5-115">Při instalaci systému Linux se doporučuje použít standardní oddíly spíše než LVM (často výchozí pro mnoho instalace).</span><span class="sxs-lookup"><span data-stu-id="d2fd5-115">When installing the Linux system it is recommended that you use standard partitions rather than LVM (often the default for many installations).</span></span> <span data-ttu-id="d2fd5-116">Tím se vyhnete LVM název je v konfliktu s klonovaný virtuální počítače, zvlášť pokud někdy musí být připojené k jiným virtuálním Počítačem pro řešení potíží s disk s operačním systémem.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-116">This will avoid LVM name conflicts with cloned VMs, particularly if an OS disk ever needs to be attached to another VM for troubleshooting.</span></span> <span data-ttu-id="d2fd5-117">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) lze použít v datových disků, pokud upřednostňovaný.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-117">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks if preferred.</span></span>
* <span data-ttu-id="d2fd5-118">Pro větší velikosti virtuálních počítačů z důvodu chyby ve verzích systému Linux jádra níže 2.6.37 nepodporuje NUMA.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-118">NUMA is not supported for larger VM sizes due to a bug in Linux kernel versions below 2.6.37.</span></span> <span data-ttu-id="d2fd5-119">Tento problém ovlivňuje především distribuce pomocí nadřazený Red Hat 2.6.32 jádra.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-119">This issue primarily impacts distributions using the upstream Red Hat 2.6.32 kernel.</span></span> <span data-ttu-id="d2fd5-120">Ruční instalace agenta Azure Linux (příkaz waagent) automaticky zakáže NUMA v konfiguraci GRUB jádra systému Linux.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-120">Manual installation of the Azure Linux agent (waagent) will automatically disable NUMA in the GRUB configuration for the Linux kernel.</span></span> <span data-ttu-id="d2fd5-121">Další informace o této naleznete v následujících krocích.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-121">More information about this can be found in the steps below.</span></span>
* <span data-ttu-id="d2fd5-122">Nekonfigurujte přepnutí oddílu na disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-122">Do not configure a swap partition on the OS disk.</span></span> <span data-ttu-id="d2fd5-123">Chcete-li vytvořit odkládací soubor na disku dočasných prostředků lze nakonfigurovat agenta systému Linux.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-123">The Linux agent can be configured to create a swap file on the temporary resource disk.</span></span>  <span data-ttu-id="d2fd5-124">Další informace o této naleznete v následujících krocích.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-124">More information about this can be found in the steps below.</span></span>
* <span data-ttu-id="d2fd5-125">Všechny virtuální pevné disky musí mít velikostí, které jsou násobky 1 MB.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-125">All of the VHDs must have sizes that are multiples of 1 MB.</span></span>
* <span data-ttu-id="d2fd5-126">Ujistěte se, že `Addons` je povoleno úložiště.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-126">Make sure that the `Addons` repository is enabled.</span></span> <span data-ttu-id="d2fd5-127">Upravte soubor `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) nebo `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux) a změňte řádek `enabled=0` k `enabled=1` pod **[ol6_addons]** nebo **[ol7_addons]** v tomto souboru.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-127">Edit the file `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) or `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux ), and change the line `enabled=0` to `enabled=1` under **[ol6_addons]** or **[ol7_addons]** in this file.</span></span>

## <a name="oracle-linux-64"></a><span data-ttu-id="d2fd5-128">Oracle Linux 6.4 +</span><span class="sxs-lookup"><span data-stu-id="d2fd5-128">Oracle Linux 6.4+</span></span>
<span data-ttu-id="d2fd5-129">Je třeba provést určitou konfiguraci kroky v operačním systému pro virtuální počítač na spuštění v Azure.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-129">You must complete specific configuration steps in the operating system for the virtual machine to run in Azure.</span></span>

1. <span data-ttu-id="d2fd5-130">V prostředním podokně Správce technologie Hyper-V vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-130">In the center pane of Hyper-V Manager, select the virtual machine.</span></span>
2. <span data-ttu-id="d2fd5-131">Klikněte na tlačítko **Connect** a otevřete okno pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-131">Click **Connect** to open the window for the virtual machine.</span></span>
3. <span data-ttu-id="d2fd5-132">Odinstalace NetworkManager spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="d2fd5-132">Uninstall NetworkManager by running the following command:</span></span>
   
        # sudo rpm -e --nodeps NetworkManager
   
    <span data-ttu-id="d2fd5-133">**Poznámka:** Pokud ještě není nainstalovaný balíček, tento příkaz se nezdaří s chybovou zprávou.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-133">**Note:** If the package is not already installed, this command will fail with an error message.</span></span> <span data-ttu-id="d2fd5-134">Toto chování se očekává.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-134">This is expected.</span></span>
4. <span data-ttu-id="d2fd5-135">Vytvořte soubor s názvem **sítě** v `/etc/sysconfig/` adresář, který obsahuje následující text:</span><span class="sxs-lookup"><span data-stu-id="d2fd5-135">Create a file named **network** in the `/etc/sysconfig/` directory that contains the following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
5. <span data-ttu-id="d2fd5-136">Vytvořte soubor s názvem **ifcfg eth0** v `/etc/sysconfig/network-scripts/` adresář, který obsahuje následující text:</span><span class="sxs-lookup"><span data-stu-id="d2fd5-136">Create a file named **ifcfg-eth0** in the `/etc/sysconfig/network-scripts/` directory that contains the following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
6. <span data-ttu-id="d2fd5-137">Změňte pravidla udev, aby nedošlo ke generování statická pravidla pro alespoň jedno rozhraní sítě Ethernet.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-137">Modify udev rules to avoid generating static rules for the Ethernet interface(s).</span></span> <span data-ttu-id="d2fd5-138">Tato pravidla může způsobit problémy při klonování virtuálního počítače v Microsoft Azure nebo Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="d2fd5-138">These rules can cause problems when cloning a virtual machine in Microsoft Azure or Hyper-V:</span></span>
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
7. <span data-ttu-id="d2fd5-139">Zajistěte, aby že síťové služby se spustí při spuštění spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="d2fd5-139">Ensure the network service will start at boot time by running the following command:</span></span>
   
        # chkconfig network on
8. <span data-ttu-id="d2fd5-140">Nainstalujte python pyasn1 spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="d2fd5-140">Install python-pyasn1 by running the following command:</span></span>
   
        # sudo yum install python-pyasn1
9. <span data-ttu-id="d2fd5-141">Upravte řádku spouštěcí jádra ve vaší konfiguraci grub pro zahrnutí dalších jádra parametry Azure.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-141">Modify the kernel boot line in your grub configuration to include additional kernel parameters for Azure.</span></span> <span data-ttu-id="d2fd5-142">Chcete tuto otevřete "/ boot/grub/menu.lst" v textovém editoru a ujistěte se, že výchozí jádra zahrnuje následující parametry:</span><span class="sxs-lookup"><span data-stu-id="d2fd5-142">To do this open "/boot/grub/menu.lst" in a text editor and ensure that the default kernel includes the following parameters:</span></span>
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off
   
   <span data-ttu-id="d2fd5-143">To také zajistí, všechny zprávy konzoly odešlou do první sériového portu, který může být užitečné Azure podporu pro ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-143">This will also ensure all console messages are sent to the first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="d2fd5-144">Tato akce zakáže NUMA z důvodu chyby Oracle Red Hat kompatibilní jádra systému.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-144">This will disable NUMA due to a bug in Oracle's Red Hat compatible kernel.</span></span>
   
   <span data-ttu-id="d2fd5-145">Kromě výše uvedeného, doporučuje se *odebrat* následující parametry:</span><span class="sxs-lookup"><span data-stu-id="d2fd5-145">In addition to the above, it is recommended to *remove* the following parameters:</span></span>
   
        rhgb quiet crashkernel=auto
   
   <span data-ttu-id="d2fd5-146">Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly pro odeslání do sériového portu.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-146">Graphical and quiet boot are not useful in a cloud environment where we want all the logs to be sent to the serial port.</span></span>
   
   <span data-ttu-id="d2fd5-147">`crashkernel` Možnost může být vlevo nakonfigurované v případě potřeby, ale Všimněte si, že tento parametr se sníží množství dostupné paměti ve virtuálním počítači 128 MB nebo informace, které mohou způsobovat menší velikostí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-147">The `crashkernel` option may be left configured if desired, but note that this parameter will reduce the amount of available memory in the VM by 128MB or more, which may be problematic on the smaller VM sizes.</span></span>
10. <span data-ttu-id="d2fd5-148">Ujistěte se, že SSH server je nainstalován a nakonfigurován na spuštění při spuštění.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-148">Ensure that the SSH server is installed and configured to start at boot time.</span></span>  <span data-ttu-id="d2fd5-149">Obvykle je to výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-149">This is usually the default.</span></span>
11. <span data-ttu-id="d2fd5-150">Nainstalujte Azure Linux Agent spuštěním následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-150">Install the Azure Linux Agent by running the following command.</span></span> <span data-ttu-id="d2fd5-151">Nejnovější verze je 2.0.15.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-151">The latest version is 2.0.15.</span></span>
    
        # sudo yum install WALinuxAgent
    
    <span data-ttu-id="d2fd5-152">Upozorňujeme, že instalace balíčku WALinuxAgent dojde k odebrání NetworkManager a balíčky NetworkManager gnome Pokud nebyly již odebrána jak je popsáno v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-152">Note that installing the WALinuxAgent package will remove the NetworkManager and NetworkManager-gnome packages if they were not already removed as described in step 2.</span></span>
12. <span data-ttu-id="d2fd5-153">Nevytvářejte odkládacího prostoru na disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-153">Do not create swap space on the OS disk.</span></span>
    
    <span data-ttu-id="d2fd5-154">Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku místní prostředek, který je připojen k virtuálnímu počítači po zřízení v Azure.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-154">The Azure Linux Agent can automatically configure swap space using the local resource disk that is attached to the VM after provisioning on Azure.</span></span> <span data-ttu-id="d2fd5-155">Všimněte si, že je disk místní prostředek *dočasné* na disku a může vyprázdněny, když je virtuální počítač zrušit.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-155">Note that the local resource disk is a *temporary* disk, and might be emptied when the VM is deprovisioned.</span></span> <span data-ttu-id="d2fd5-156">Po instalaci Azure Linux Agent (viz předchozí krok), upravte následující parametry v /etc/waagent.conf odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d2fd5-156">After installing the Azure Linux Agent (see previous step), modify the following parameters in /etc/waagent.conf appropriately:</span></span>
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.
13. <span data-ttu-id="d2fd5-157">Spusťte následující příkaz pro zrušení zřízení virtuálního počítače a jeho přípravu pro zřizování na Azure:</span><span class="sxs-lookup"><span data-stu-id="d2fd5-157">Run the following commands to deprovision the virtual machine and prepare it for provisioning on Azure:</span></span>
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
14. <span data-ttu-id="d2fd5-158">Klikněte na tlačítko **akce -> vypnutí dolů** ve Správci technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-158">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="d2fd5-159">Svůj disk VHD Linux je nyní připravena k odeslání do Azure.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-159">Your Linux VHD is now ready to be uploaded to Azure.</span></span>

- - -
## <a name="oracle-linux-70"></a><span data-ttu-id="d2fd5-160">Oracle Linux 7.0 +</span><span class="sxs-lookup"><span data-stu-id="d2fd5-160">Oracle Linux 7.0+</span></span>
<span data-ttu-id="d2fd5-161">**Změny v systému Oracle Linux 7**</span><span class="sxs-lookup"><span data-stu-id="d2fd5-161">**Changes in Oracle Linux 7**</span></span>

<span data-ttu-id="d2fd5-162">Připravuje se virtuální počítač Oracle Linux 7 pro Azure je velmi podobné Oracle Linux 6, ale existuje několik důležitých rozdílů vhodné poznamenat:</span><span class="sxs-lookup"><span data-stu-id="d2fd5-162">Preparing an Oracle Linux 7 virtual machine for Azure is very similar to Oracle Linux 6, however there are several important differences worth noting:</span></span>

* <span data-ttu-id="d2fd5-163">Red Hat kompatibilní jádra a Oracle je UEK3 jsou podporovány v Azure.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-163">Both the Red Hat compatible kernel and Oracle's UEK3 are supported in Azure.</span></span>  <span data-ttu-id="d2fd5-164">Doporučuje se UEK3 jádra.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-164">The UEK3 kernel is recommended.</span></span>
* <span data-ttu-id="d2fd5-165">NetworkManager balíčku už v konfliktu s Azure Linux agent.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-165">The NetworkManager package no longer conflicts with the Azure Linux agent.</span></span> <span data-ttu-id="d2fd5-166">Tento balíček je ve výchozím nastavení nainstalovaná a doporučujeme vám, že se neodebere.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-166">This package is installed by default and we recommend that it is not removed.</span></span>
* <span data-ttu-id="d2fd5-167">GRUB2 se teď používá jako výchozí zaváděcí program pro spouštění, tak postup úpravy parametry jádra se změnila (viz níže).</span><span class="sxs-lookup"><span data-stu-id="d2fd5-167">GRUB2 is now used as the default bootloader, so the procedure for editing kernel parameters has changed (see below).</span></span>
* <span data-ttu-id="d2fd5-168">XFS je nyní na výchozí systém souborů.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-168">XFS is now the default file system.</span></span> <span data-ttu-id="d2fd5-169">Systém souborů ext4 pořád můžou použít v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-169">The ext4 file system can still be used if desired.</span></span>

<span data-ttu-id="d2fd5-170">**Kroky konfigurace**</span><span class="sxs-lookup"><span data-stu-id="d2fd5-170">**Configuration steps**</span></span>

1. <span data-ttu-id="d2fd5-171">Ve Správci technologie Hyper-V vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-171">In Hyper-V Manager, select the virtual machine.</span></span>
2. <span data-ttu-id="d2fd5-172">Klikněte na tlačítko **Connect** k otevření okna konzoly pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-172">Click **Connect** to open a console window for the virtual machine.</span></span>
3. <span data-ttu-id="d2fd5-173">Vytvořte soubor s názvem **sítě** v `/etc/sysconfig/` adresář, který obsahuje následující text:</span><span class="sxs-lookup"><span data-stu-id="d2fd5-173">Create a file named **network** in the `/etc/sysconfig/` directory that contains the following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
4. <span data-ttu-id="d2fd5-174">Vytvořte soubor s názvem **ifcfg eth0** v `/etc/sysconfig/network-scripts/` adresář, který obsahuje následující text:</span><span class="sxs-lookup"><span data-stu-id="d2fd5-174">Create a file named **ifcfg-eth0** in the `/etc/sysconfig/network-scripts/` directory that contains the following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
5. <span data-ttu-id="d2fd5-175">Změňte pravidla udev, aby nedošlo ke generování statická pravidla pro alespoň jedno rozhraní sítě Ethernet.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-175">Modify udev rules to avoid generating static rules for the Ethernet interface(s).</span></span> <span data-ttu-id="d2fd5-176">Tato pravidla může způsobit problémy při klonování virtuálního počítače v Microsoft Azure nebo Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="d2fd5-176">These rules can cause problems when cloning a virtual machine in Microsoft Azure or Hyper-V:</span></span>
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
6. <span data-ttu-id="d2fd5-177">Zajistěte, aby že síťové služby se spustí při spuštění spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="d2fd5-177">Ensure the network service will start at boot time by running the following command:</span></span>
   
        # sudo chkconfig network on
7. <span data-ttu-id="d2fd5-178">Instalovat balíček python pyasn1 spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="d2fd5-178">Install the python-pyasn1 package by running the following command:</span></span>
   
        # sudo yum install python-pyasn1
8. <span data-ttu-id="d2fd5-179">Spusťte následující příkaz, který zrušte aktuální metadata yum a nainstalujte všechny aktualizace:</span><span class="sxs-lookup"><span data-stu-id="d2fd5-179">Run the following command to clear the current yum metadata and install any updates:</span></span>
   
        # sudo yum clean all
        # sudo yum -y update
9. <span data-ttu-id="d2fd5-180">Upravte řádku spouštěcí jádra ve vaší konfiguraci grub pro zahrnutí dalších jádra parametry Azure.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-180">Modify the kernel boot line in your grub configuration to include additional kernel parameters for Azure.</span></span> <span data-ttu-id="d2fd5-181">K tomu otevřete "/ etc/výchozí/grub" v textovém editoru a upravit `GRUB_CMDLINE_LINUX` parametr, například:</span><span class="sxs-lookup"><span data-stu-id="d2fd5-181">To do this open "/etc/default/grub" in a text editor and edit the `GRUB_CMDLINE_LINUX` parameter, for example:</span></span>
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   <span data-ttu-id="d2fd5-182">To také zajistí, všechny zprávy konzoly odešlou do první sériového portu, který může být užitečné Azure podporu pro ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-182">This will also ensure all console messages are sent to the first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="d2fd5-183">Je také vypne nové zásady vytváření názvů OEL 7 pro síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-183">It also turns off the new OEL 7 naming conventions for NICs.</span></span> <span data-ttu-id="d2fd5-184">Kromě výše uvedeného, doporučuje se *odebrat* následující parametry:</span><span class="sxs-lookup"><span data-stu-id="d2fd5-184">In addition to the above, it is recommended to *remove* the following parameters:</span></span>
   
       rhgb quiet crashkernel=auto
   
   <span data-ttu-id="d2fd5-185">Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly pro odeslání do sériového portu.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-185">Graphical and quiet boot are not useful in a cloud environment where we want all the logs to be sent to the serial port.</span></span>
   
   <span data-ttu-id="d2fd5-186">`crashkernel` Možnost může být vlevo nakonfigurované v případě potřeby, ale Všimněte si, že tento parametr se sníží množství dostupné paměti ve virtuálním počítači 128 MB nebo informace, které mohou způsobovat menší velikostí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-186">The `crashkernel` option may be left configured if desired, but note that this parameter will reduce the amount of available memory in the VM by 128MB or more, which may be problematic on the smaller VM sizes.</span></span>
10. <span data-ttu-id="d2fd5-187">Po dokončení úprav "/ etc/výchozí/grub" za výše, spusťte následující příkaz k opětovnému sestavení grub konfigurace:</span><span class="sxs-lookup"><span data-stu-id="d2fd5-187">Once you are done editing "/etc/default/grub" per above, run the following command to rebuild the grub configuration:</span></span>
    
        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg
11. <span data-ttu-id="d2fd5-188">Ujistěte se, že SSH server je nainstalován a nakonfigurován na spuštění při spuštění.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-188">Ensure that the SSH server is installed and configured to start at boot time.</span></span>  <span data-ttu-id="d2fd5-189">Obvykle je to výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-189">This is usually the default.</span></span>
12. <span data-ttu-id="d2fd5-190">Nainstalujte Azure Linux Agent spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="d2fd5-190">Install the Azure Linux Agent by running the following command:</span></span>
    
        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent
13. <span data-ttu-id="d2fd5-191">Nevytvářejte odkládacího prostoru na disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-191">Do not create swap space on the OS disk.</span></span>
    
    <span data-ttu-id="d2fd5-192">Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku místní prostředek, který je připojen k virtuálnímu počítači po zřízení v Azure.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-192">The Azure Linux Agent can automatically configure swap space using the local resource disk that is attached to the VM after provisioning on Azure.</span></span> <span data-ttu-id="d2fd5-193">Všimněte si, že je disk místní prostředek *dočasné* na disku a může vyprázdněny, když je virtuální počítač zrušit.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-193">Note that the local resource disk is a *temporary* disk, and might be emptied when the VM is deprovisioned.</span></span> <span data-ttu-id="d2fd5-194">Po instalaci Azure Linux Agent (viz předchozí krok), upravte následující parametry v /etc/waagent.conf odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d2fd5-194">After installing the Azure Linux Agent (see the previous step), modify the following parameters in /etc/waagent.conf appropriately:</span></span>
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.
14. <span data-ttu-id="d2fd5-195">Spusťte následující příkaz pro zrušení zřízení virtuálního počítače a jeho přípravu pro zřizování na Azure:</span><span class="sxs-lookup"><span data-stu-id="d2fd5-195">Run the following commands to deprovision the virtual machine and prepare it for provisioning on Azure:</span></span>
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
15. <span data-ttu-id="d2fd5-196">Klikněte na tlačítko **akce -> vypnutí dolů** ve Správci technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-196">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="d2fd5-197">Svůj disk VHD Linux je nyní připravena k odeslání do Azure.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-197">Your Linux VHD is now ready to be uploaded to Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d2fd5-198">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d2fd5-198">Next steps</span></span>
<span data-ttu-id="d2fd5-199">Nyní jste připraveni používat vaše VHD Oracle Linux k vytvoření nové virtuální počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="d2fd5-199">You're now ready to use your Oracle Linux .vhd to create new virtual machines in Azure.</span></span> <span data-ttu-id="d2fd5-200">Pokud je poprvé, že jste nahrávání souboru VHD do Azure, najdete v části kroky 2 a 3 v [vytváření a odesílání virtuální pevný disk, který obsahuje operační systém Linux](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d2fd5-200">If this is the first time that you're uploading the .vhd file to Azure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains the Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

